---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-11"

keywords: kubernetes, iks

subcollection: containers

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:preview: .preview}


# Restriction du trafic réseau aux noeuds worker de périphérie
{: #edge}

Les noeuds worker de périphérie peuvent améliorer la sécurité de votre cluster en limitant les accès aux noeuds worker en externe et en isolant la charge de travail du réseau dans {{site.data.keyword.containerlong}}.
{:shortdesc}

Lorsque ces noeuds worker sont marqués pour mise en réseau uniquement, les autres charges de travail ne peuvent pas consommer d'unité centrale ou de mémoire ni interférer avec le réseau.

Si vous disposez d'un cluster à zones multiples et que vous voulez limiter le trafic réseau aux noeuds worker de périphérie, au moins deux noeuds worker de périphérie doivent être activés dans chaque zone pour assurer la haute disponibilité de l'équilibreur de charge ou des pods Ingress. Créez un pool de noeuds worker de périphérie couvrant toutes les zones de votre cluster, avec au moins deux noeuds worker par zone.
{: tip}

## Etiquetage de noeuds worker en tant que noeuds de périphérie
{: #edge_nodes}

Ajoutez l'étiquette `dedicated=edge` à au moins deux noeuds worker sur chaque VLAN public ou privé de votre cluster par garantir qu'Ingress et les équilibreurs de charge de réseau (NLB) sont déployés uniquement sur ces noeuds worker.
{:shortdesc}

Dans Kubernetes versions 1.14 et ultérieures, vous pouvez déployer des NLB et des ALB publics et privés sur des noeuds worker de périphérie.
Dans Kubernetes versions 1.13 et précédentes, les ALB publics et privés et les NLB publics peuvent être déployés sur des noeuds de périphérie, mais les NLB privés doivent être déployés sur des noeuds worker autres que des noeuds de périphérie dans votre cluster uniquement.
{: note}

Avant de commencer :

* Vérifiez que vous disposez des [rôles {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform) suivants :
  * N'importe quel rôle de plateforme
  * Rôle de service **Auteur** ou **Responsable** pour tous les espaces de nom
* [Connectez-vous à votre compte. Le cas échéant, ciblez le groupe de ressources approprié. Définissez le contexte pour votre cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

</br>Pour libeller des noeuds worker en tant que noeuds de périphérie :

1. [Créez un nouveau pool de noeuds worker](/docs/containers?topic=containers-add_workers#add_pool) couvrant toutes les zones de votre cluster et comportant au moins deux noeuds worker par zone. Dans la commande `ibmcloud ks worker-pool-create`, ajoutez l'indicateur `--labels dedicated=edge` pour étiqueter tous les noeuds de périphérie du pool. Tous les noeuds worker de ce pool, y compris les noeuds worker que vous ajoutez ultérieurement, sont étiquetés en tant que noeuds de périphérie. 
  <p class="tip">Si vous souhaitez utiliser un pool de noeuds worker existant, celui-ci doit couvrir toutes les zones de votre cluster et comporter au moins deux noeuds worker par zone. Vous pouvez affecter au pool de noeuds le libellé `dedicated=edge` à l'aide de l'[API de pool de noeuds worker PATCH](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/PatchWorkerPool). Dans le corps de la demande, utilisez le format JSON ci-après.
    Une fois qu'un noeud worker est marqué avec `dedicated=edge`, tous les noeuds worker existants et suivants prennent ce libellé et les services d'équilibreur de charge et Ingress sont déployés sur un noeud worker de périphérie.<pre class="screen">
      {
        "labels": {"dedicated":"edge"},
        "state": "labels"
      }</pre></p>

2. Vérifiez que le pool de noeuds worker et les noeuds worker comportent le libellé `dedicated=edge`. 
  * Pour vérifier le pool de noeuds worker :
    ```
    ibmcloud ks worker-pool-get --cluster <cluster_name_or_ID> --worker-pool <worker_pool_name_or_ID>
    ```
    {: pre}

  * Pour vérifier les noeuds worker individuels, examinez la zone **Labels** de la sortie de la commande suivante :
    ```
    kubectl describe node <worker_node_private_IP>
    ```
    {: pre}

3. Extrayez tous les NLB et ALB existants dans le cluster.
  ```
  kubectl get services --all-namespaces | grep LoadBalancer
  ```
  {: pre}

  Dans la sortie, notez l'espace de nom (**Namespace**) et le nom (**Name**) de chaque service d'équilibreur de charge. Par exemple, dans la sortie suivante, il y a quatre services d'équilibreur de charge : un NLB public dans l'espace de nom `default` et un ALB privé et deux ALB publics dans l'espace de nom `kube-system`. 
  ```
  NAMESPACE     NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP     PORT(S)                                     AGE
  default       webserver-lb                                     LoadBalancer   172.21.190.18    169.46.17.2     80:30597/TCP                                10m
  kube-system   private-crdf253b6025d64944ab99ed63bb4567b6-alb1  LoadBalancer   172.21.158.78    10.185.94.150   80:31015/TCP,443:31401/TCP,9443:32352/TCP   25d
  kube-system   public-crdf253b6025d64944ab99ed63bb4567b6-alb1   LoadBalancer   172.21.84.248    169.48.228.78   80:30286/TCP,443:31363/TCP                  1h
  kube-system   public-crdf253b6025d64944ab99ed63bb4567b6-alb2   LoadBalancer   172.21.229.73    169.46.17.6     80:31104/TCP,443:31138/TCP                  57m
  ```
  {: screen}

4. A partir de la sortie de l'étape précédente, exécutez la commande suivante pour chaque NLB et ALB. Cette commande redéploie le NLB ou l'ALB sur un noeud worker de périphérie. 

  Si votre cluster exécute Kubernetes versions 1.14 ou ultérieures, vous pouvez déployer des NLB et des ALB publics et privés sur les noeuds de périphérie. Dans Kubernetes versions 1.13 et antérieures, seuls les ALB publics et privés et les NLB publics peuvent être déployés sur des noeuds de périphérie, par conséquent, ne redéployez pas de services NLB privés.
  {: note}

  ```
  kubectl get service -n <namespace> <service_name> -o yaml | kubectl apply -f -
  ```
  {: pre}

  Exemple de sortie :
  ```
  service "webserver-lb" configured
  ```
  {: screen}

5. Pour vous assurer que les charges de travail de réseau sont limitées à des noeuds de périphérie, vérifiez que les pods NLB et ALB sont planifiés sur les noeuds de périphérie et qu'ils ne sont pas planifiés sur des noeuds autres que des noeuds de périphérie.

  * Pods NLB :
    1. Vérifiez que les pods NLB sont déployés sur des noeuds de périphérie. Recherchez l'adresse IP externe du service d'équilibreur de charge qui figure dans la sortie à l'étape 3. Remplacez les points (`.`) par des tirets (`-`). Exemple illustrant le NLB `webserver-lb` dont l'adresse IP externe est `169.46.17.2` :
      ```
      kubectl describe nodes -l dedicated=edge | grep "169-46-17-2"
      ```
      {: pre}

      Exemple de sortie :
      ```
      ibm-system                 ibm-cloud-provider-ip-169-46-17-2-76fcb4965d-wz6dg                 5m (0%)       0 (0%)      10Mi (0%)        0 (0%)
      ibm-system                 ibm-cloud-provider-ip-169-46-17-2-76fcb4965d-2z64r                 5m (0%)       0 (0%)      10Mi (0%)        0 (0%)
      ```
      {: screen}
    2. Vérifiez qu'aucun pod NLB n'est déployé sur des noeuds autres que des noeuds de périphérie. Exemple illustrant le NLB `webserver-lb` dont l'adresse IP externe est `169.46.17.2` :
      ```
      kubectl describe nodes -l dedicated!=edge | grep "169-46-17-2"
      ```
      {: pre}
      * Si les pods NLB sont correctement déployés sur des noeuds de périphérie, aucun pod NLB n'est renvoyé. Vos NLB sont replanifiés uniquement sur des noeuds worker de périphérie.
      * Si des pods NLB sont renvoyés, passez à l'étape suivante. 

  * Pods ALB :
    1. Vérifiez que tous les pods ALB sont déployés sur des noeuds de périphérie. Recherchez le mot clé `alb`. Chaque ALB public et privé comporte deux pods. Exemple :
      ```
      kubectl describe nodes -l dedicated=edge | grep alb
      ```
      {: pre}

      Exemple de sortie pour un cluster comportant deux zones dans lequel un ALB privé par défaut et deux ALB publics par défaut sont activés :
      ```
      kube-system                private-crdf253b6025d64944ab99ed63bb4567b6-alb1-d5dd478db-27pv4    0 (0%)        0 (0%)      0 (0%)           0 (0%)
      kube-system                private-crdf253b6025d64944ab99ed63bb4567b6-alb1-d5dd478db-7p9q6    0 (0%)        0 (0%)      0 (0%)           0 (0%)
      kube-system                public-crdf253b6025d64944ab99ed63bb4567b6-alb1-5ff8cdff89-s77z6    0 (0%)        0 (0%)      0 (0%)           0 (0%)
      kube-system                public-crdf253b6025d64944ab99ed63bb4567b6-alb1-5ff8cdff89-kvs9f    0 (0%)        0 (0%)      0 (0%)           0 (0%)
      kube-system                public-crdf253b6025d64944ab99ed63bb4567b6-alb2-57df7c7b5b-tp6xw    0 (0%)        0 (0%)      0 (0%)           0 (0%)
      kube-system                public-crdf253b6025d64944ab99ed63bb4567b6-alb2-57df7c7b5b-z5p2v    0 (0%)        0 (0%)      0 (0%)           0 (0%)
      ```
      {: screen}

    2. Vérifiez qu'aucun pod ALB n'est déployé sur des noeuds autres que des noeuds de périphérie. Exemple :
      ```
      kubectl describe nodes -l dedicated!=edge | grep alb
      ```
      {: pre}
      * Si les pods ALB sont correctement déployés sur des noeuds de périphérie, aucun pod ALB n'est renvoyé. Vos NLB sont replanifiés uniquement sur des noeuds worker de périphérie.
      * Si des pods ALB sont renvoyés, passez à l'étape suivante. 

6. Si des pods NLB ou ALB sont encore déployés sur des noeuds autres que des noeuds de périphérie, vous pouvez supprimer les pods afin qu'ils soient redéployés sur des noeuds de périphérie. **Important** : supprimez un pod à la fois et vérifiez que le pod est replanifié sur un noeud de périphérie avant de supprimer d'autres pods. 
  1. Supprimez un pod. Exemple illustrant un cas où l'un des pods NLB `webserver-lb` n'a pas été planifié sur un noeud de périphérie :
    ```
    kubectl delete pod ibm-cloud-provider-ip-169-46-17-2-76fcb4965d-wz6dg
    ```
    {: pre}

  2. Vérifiez que le pod est replanifié sur un noeud worker de périphérie. La replanification est automatique, mais peut durer quelques minutes. Exemple illustrant le NLB `webserver-lb` dont l'adresse IP externe est `169.46.17.2` :
      ```
      kubectl describe nodes -l dedicated=edge | grep "169-46-17-2"
      ```
    {: pre}

    Exemple de sortie :
    ```
    ibm-system                 ibm-cloud-provider-ip-169-46-17-2-76fcb4965d-wz6dg                 5m (0%)       0 (0%)      10Mi (0%)        0 (0%)
    ibm-system                 ibm-cloud-provider-ip-169-46-17-2-76fcb4965d-2z64r                 5m (0%)       0 (0%)      10Mi (0%)        0 (0%)
    ```
    {: screen}

</br>Vous avez libellé des noeuds worker avec `dedicated=edge` et redéployé tous les ALB et NLB existants sur les noeuds de périphérie. Tous les ALB et NLB ajoutés ultérieurement au cluster seront également déployés sur un noeud de périphérie de votre pool de noeuds worker de périphérie. Ensuite, empêchez d'autres [charges de travail de s'exécuter sur des noeuds worker de périphérie](#edge_workloads) et [bloquez le trafic entrant vers des ports de noeud (NodePort) sur des noeuds worker](/docs/containers?topic=containers-network_policies#block_ingress).

<br />


## Empêcher l'exécution de charges de travail sur des noeuds worker de périphérie
{: #edge_workloads}

Un avantage des noeuds worker de périphérie est le fait qu'ils peuvent être définis pour n'exécuter que des services de mise en réseau.
{:shortdesc}

La tolérance `dedicated=edge` implique que tous les services NLB et tous les services ALB Ingress sont déployés uniquement sur les noeuds worker libellés. Toutefois, pour empêcher d'autres charges de travail de s'exécuter sur des noeuds worker de périphérie et de consommer des ressources de noeud worker, vous devez utiliser une [annotation Kubernetes taints![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/).

Avant de commencer :
- Vérifiez que vous disposez du [rôle de service {{site.data.keyword.Bluemix_notm}} IAM **Responsable** pour tous les espaces de nom](/docs/containers?topic=containers-users#platform).
- [Connectez-vous à votre compte. Le cas échéant, ciblez le groupe de ressources approprié. Définissez le contexte pour votre cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

</br>Pour empêcher l'exécution d'autres charges de travail sur des noeuds worker de périphérie :

1. Appliquez à chaque noeud worker portant le libellé `dedicated=edge` une annotation taint qui empêche l'exécution des pods sur le noeud worker et qui supprime du noeud worker ceux qui n'ont pas le libellé `dedicated=edge`. Les pods retirés sont redéployés sur d'autres noeuds worker dont la capacité le permet.
  ```
  kubectl taint node -l dedicated=edge dedicated=edge:NoSchedule dedicated=edge:NoExecute
  ```
  {: pre}
  Maintenant, seuls les pods ayant la tolérance `dedicated=edge` sont déployés sur vos noeuds worker de périphérie.

2. Vérifiez que vos noeuds de périphérie comportent des annotations taint. 
  ```
  kubectl describe nodes -l dedicated=edge | grep "Taint|Hostname"
  ```
  {: pre}

  Exemple de sortie :
  ```
  Taints:             dedicated=edge:NoExecute
    Hostname:    10.176.48.83
  Taints:             dedicated=edge:NoExecute
    Hostname:    10.184.58.7
  ```
  {: screen}

3. Si vous choisissez d'[activer la conservation de l'adresse IP source pour un service NLB 1.0](/docs/containers?topic=containers-loadbalancer#node_affinity_tolerations), assurez-vous que les pods d'application sont planifiés sur les noeuds worker de périphérie en [ajoutant l'affinité de noeud de périphérie aux pods d'application](/docs/containers?topic=containers-loadbalancer#lb_edge_nodes). Les pods d'application doivent être planifiés sur des noeuds de périphérie pour recevoir des demandes entrantes.

4. Pour supprimer une annotation taint, exécutez la commande suivante.
    ```
    kubectl taint node <node_name> dedicated:NoSchedule- dedicated:NoExecute-
    ```
    {: pre}
