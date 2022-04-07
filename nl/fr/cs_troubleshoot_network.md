---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-05"

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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}


# Traitement des incidents liés à la mise en réseau au sein d'un cluster
{: #cs_troubleshoot_network}

Lorsque vous utilisez {{site.data.keyword.containerlong}}, envisagez l'utilisation de ces techniques pour identifier et résoudre les incidents liés à la mise en réseau au sein d'un cluster.
{: shortdesc}

Vous rencontrez des difficultés pour connecter votre application via Ingress ? Essayez de [déboguer Ingress](/docs/containers?topic=containers-cs_troubleshoot_debug_ingress).
{: tip}

Lorsque vous traitez les incidents, vous pouvez utiliser l'[outil de débogage et de diagnostic d'{{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-cs_troubleshoot#debug_utility) pour exécuter des tests et regrouper des informations pertinentes sur les réseaux, Ingress et strongSwan concernant votre cluster.
{: tip}

## Impossible de se connecter à une application via un service d'équilibreur de charge de réseau (NLB)
{: #cs_loadbalancer_fails}

{: tsSymptoms}
Vous avez exposé votre application au public en créant un service NLB dans votre cluster. Lorsque vous avez essayé de vous connecter à votre application en utilisant l'adresse IP publique du NLB, la connexion a échoué ou expiré.

{: tsCauses}
Il se peut que votre service NLB ne fonctionne pas correctement pour l'une des raisons suivantes :

-   Le cluster est un cluster gratuit ou un cluster standard avec un seul noeud worker.
-   Le cluster n'est pas encore complètement déployé.
-   Le script de configuration pour votre service NLB comporte des erreurs.

{: tsResolve}
Pour identifier et résoudre les problèmes liés à votre service NLB :

1.  Prenez soin de configurer un cluster standard qui est entièrement déployé et qui comporte au moins deux noeuds worker afin d'assurer la haute disponibilité de votre service NLB.

  ```
  ibmcloud ks workers --cluster <cluster_name_or_ID>
  ```
  {: pre}

    Dans la sortie générée par votre interface de ligne de commande, vérifiez que la valeur **Ready** apparaît dans la zone **Status** pour vos noeuds worker et qu'une autre valeur que **free** est spécifiée dans la zone **Machine Type**

2. Pour les NLB version 2.0 : vérifiez que vous avez rempli les [prérequis pour les NLB 2.0](/docs/containers?topic=containers-loadbalancer#ipvs_provision).

3. Vérifiez que le fichier de configuration du service NLB est correct.
    * NLB version 2.0 :
        ```
        apiVersion: v1
        kind: Service
        metadata:
          name: myservice
          annotations:
            service.kubernetes.io/ibm-load-balancer-cloud-provider-enable-features: "ipvs"
        spec:
          type: LoadBalancer
          selector:
            <selector_key>:<selector_value>
          ports:
           - protocol: TCP
             port: 8080
          externalTrafficPolicy: Local
        ```
        {: screen}

        1. Vérifiez que vous avez défini **LoadBalancer** comme type de service.
        2. Vérifiez que vous avez inclus l'annotation `service.kubernetes.io/ibm-load-balancer-cloud-provider-enable-features: "ipvs"`.
        3. Dans la section `spec.selector` du service LoadBalancer, assurez-vous que `<selector_key>` et `<selector_value>` correspondent à la paire clé/valeur que vous avez utilisée dans la section `spec.template.metadata.labels` de votre fichier YAML de déploiement. Si les libellés ne correspondent pas, la section **Endpoints** de votre service LoadBalancer affiche **<none>** et votre application n'est pas accessible sur Internet.
        4. Vérifiez que vous avez utilisé le **port** sur lequel votre application est en mode écoute.
        5. Vérifiez que vous avez défini `externalTrafficPolicy` sur `Local`.

    * NLB version 1.0 :
        ```
        apiVersion: v1
    kind: Service
    metadata:
      name: myservice
    spec:
      type: LoadBalancer
      selector:
        <selector_key>:<selector_value>
      ports:
           - protocol: TCP
             port: 8080
        ```
        {: screen}

        1. Vérifiez que vous avez défini **LoadBalancer** comme type de service.
        2. Dans la section `spec.selector` du service LoadBalancer, assurez-vous que `<selector_key>` et `<selector_value>` correspondent à la paire clé/valeur que vous avez utilisée dans la section `spec.template.metadata.labels` de votre fichier YAML de déploiement. Si les libellés ne correspondent pas, la section **Endpoints** de votre service LoadBalancer affiche **<none>** et votre application n'est pas accessible sur Internet.
        3. Vérifiez que vous avez utilisé le **port** sur lequel votre application est en mode écoute.

3.  Vérifiez votre service NLB et passez en revue la section **Events** à la recherche d'éventuelles erreurs.

    ```
    kubectl describe service <myservice>
    ```
    {: pre}

    Recherchez les messages d'erreur suivants :

    <ul><li><pre class="screen"><code>Clusters with one node must use services of type NodePort</code></pre></br>Pour utiliser le service NLB, vous devez disposer d'un cluster standard et d'au moins deux noeuds worker.</li>
    <li><pre class="screen"><code>No cloud provider IPs are available to fulfill the NLB service request. Add a portable subnet to the cluster and try again</code></pre></br>Ce message d'erreur indique qu'il ne reste aucune adresse IP publique portable à attribuer à votre service NLB. Pour savoir comment demander des adresses IP publiques portables pour votre cluster, voir la rubrique <a href="/docs/containers?topic=containers-subnets#subnets">Ajout de sous-réseaux à des clusters</a>. Dès lors que des adresses IP publiques portables sont disponibles pour le cluster, le service NLB est automatiquement créé.</li>
    <li><pre class="screen"><code>Requested cloud provider IP <cloud-provider-ip> is not available. The following cloud provider IPs are available: <available-cloud-provider-ips></code></pre></br>Vous avez défini une adresse IP publique portable pour votre fichier YAML d'équilibreur de charge à l'aide de la section **`loadBalancerIP`**, or, cette adresse IP publique portable n'est pas disponible dans votre sous-réseau public portable. Dans la section **`loadBalancerIP`** de votre script de configuration, supprimez l'adresse IP existante et ajoutez l'une des adresses IP publiques portables disponibles. Vous pouvez également retirer la section **`loadBalancerIP`** de votre script de sorte qu'une adresse IP publique portable disponible puisse être allouée automatiquement.</li>
    <li><pre class="screen"><code>No available nodes for NLB services</code></pre>Vous ne disposez pas de suffisamment de noeuds worker pour déployer un service NLB. Il se pourrait que vous ayez déployé un cluster standard avec plusieurs noeuds worker, mais que la mise à disposition des noeuds worker ait échoué.</li>
    <ol><li>Affichez la liste des noeuds worker disponibles.</br><pre class="pre"><code>kubectl get nodes</code></pre></li>
    <li>Si au moins deux noeuds worker disponibles sont trouvés, affichez les détails de ces noeuds worker.</br><pre class="pre"><code>ibmcloud ks worker-get --cluster &lt;cluster_name_or_ID&gt; --worker &lt;worker_ID&gt;</code></pre></li>
    <li>Vérifiez que les ID des VLAN privé et public pour les noeuds worker renvoyés par les commandes <code>kubectl get nodes</code> et <code>ibmcloud ks worker-get</code> correspondent.</li></ol></li></ul>

4.  Si vous utilisez un domaine personnalisé pour vous connecter à votre service NLB, assurez-vous que votre domaine personnalisé est mappé à l'adresse IP publique de votre service NLB. 
    1.  Identifiez l'adresse IP publique de votre service NLB.
        ```
        kubectl describe service <service_name> | grep "LoadBalancer Ingress"
        ```
        {: pre}

    2.  Assurez-vous que votre domaine personnalisé est mappé à l'adresse IP publique portable de votre service NLB dans le pointeur (enregistrement PTR).

<br />


## Impossible de se connecter à une application via Ingress
{: #cs_ingress_fails}

{: tsSymptoms}
Vous avez exposé votre application au public en créant une ressource Ingress pour votre application dans votre cluster. Lorsque vous avez essayé de vous connecter à votre application en utilisant l'adresse IP publique ou le sous-domaine de l'équilibreur de charge d'application (ALB) Ingress, la connexion a échoué ou expiré.

{: tsResolve}
Vérifiez d'abord que votre cluster est entièrement déployé et qu'il comporte au moins deux noeuds worker afin d'assurer la haute disponibilité de votre équilibreur de charge ALB.
```
ibmcloud ks workers --cluster <cluster_name_or_ID>
```
{: pre}

Dans la sortie générée par votre interface de ligne de commande, vérifiez que la valeur **Ready** apparaît dans la zone **Status** pour vos noeuds worker et qu'une autre valeur que **free** est spécifiée dans la zone **Machine Type**

* Si votre cluster standard est entièrement déployé et comporte au moins 2 noeuds worker par zone, mais qu'aucun **sous-domaine Ingress** n'est disponible, voir [Impossible d'obtenir un sous-domaine pour l'équilibreur de charge d'application (ALB) Ingress](/docs/containers?topic=containers-cs_troubleshoot_network#cs_subnet_limit).
* Pour résoudre d'autres problèmes, traitez les incidents relatifs à votre configuration Ingress en suivant les étapes indiquées dans la section [Débogage d'Ingress](/docs/containers?topic=containers-cs_troubleshoot_debug_ingress).

<br />


## Problèmes liés au secret d'équilibreur de charge d'application (ALB) Ingress
{: #cs_albsecret_fails}

{: tsSymptoms}
Une fois que vous avez déployé une valeur confidentielle d'équilibreur de charge ALB Ingress dans votre cluster en utilisant la commande `ibmcloud ks alb-cert-deploy`, la zone `Description` n'est pas actualisée avec le nom de valeur confidentielle lorsque vous affichez votre certificat dans {{site.data.keyword.cloudcerts_full_notm}}.

Lorsque vous listez les informations sur la valeur confidentielle de l'équilibreur de charge ALB, son statut indique `*_failed` (Echec). Par exemple, `create_failed`, `update_failed`, `delete_failed`.

{: tsResolve}
Ci-dessous figurent les motifs pour lesquels la valeur confidentielle de l'équilibreur de charge ALB peut échouer, ainsi que les étapes de résolution correspondantes :

<table>
<caption>Traitement des incidents liés aux valeurs confidentielles de l'équilibreur de charge d'application Ingress</caption>
 <thead>
 <th>Motifs</th>
 <th>Procédure de résolution du problème</th>
 </thead>
 <tbody>
 <tr>
 <td>Les rôles d'accès requis pour télécharger et mettre à jour des données de certificat ne vous ont pas été attribués.</td>
 <td>Contactez l'administrateur de votre compte afin qu'il vous affecte les rôles {{site.data.keyword.Bluemix_notm}} IAM suivants :<ul><li>Les rôles de service **Responsable** et **Auteur** pour votre instance {{site.data.keyword.cloudcerts_full_notm}}. Pour plus d'informations, voir la rubrique sur la <a href="/docs/services/certificate-manager?topic=certificate-manager-managing-service-access-roles#managing-service-access-roles">gestion des rôles d'accès au service</a> pour {{site.data.keyword.cloudcerts_short}}.</li><li>Le <a href="/docs/containers?topic=containers-users#platform">rôle de plateforme **Administrateur**</a> pour le cluster.</li></ul></td>
 </tr>
 <tr>
 <td>Le CRN de certificat indiqué lors de la création, de la mise à jour ou de la suppression ne relève pas du même compte que le cluster.</td>
 <td>Vérifiez que le CRN de certificat que vous avez fourni est importé dans une instance du service {{site.data.keyword.cloudcerts_short}} déployée dans le même compte que votre cluster.</td>
 </tr>
 <tr>
 <td>Le CRN de certificat fourni lors de la création est incorrect.</td>
 <td><ol><li>Vérifiez l'exactitude de la chaîne de CRN de certificat soumise.</li><li>Si le CRN du certificat est exact, essayez de mettre à jour la valeur confidentielle : <code>ibmcloud ks alb-cert-deploy --update --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></li><li>Si cette commande indique le statut <code>update_failed</code>, supprimez la valeur confidentielle : <code>ibmcloud ks alb-cert-rm --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt;</code></li><li>Déployez à nouveau la valeur confidentielle : <code>ibmcloud ks alb-cert-deploy --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></li></ol></td>
 </tr>
 <tr>
 <td>Le CRN de certificat soumis lors de la mise à jour est incorrect.</td>
 <td><ol><li>Vérifiez l'exactitude de la chaîne de CRN de certificat soumise.</li><li>Si le CRN de certificat est exact, supprimez la valeur confidentielle : <code>ibmcloud ks alb-cert-rm --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt;</code></li><li>Déployez à nouveau la valeur confidentielle : <code>ibmcloud ks alb-cert-deploy --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></li><li>Essayez de mettre à jour la valeur confidentielle : <code>ibmcloud ks alb-cert-deploy --update --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></li></ol></td>
 </tr>
 <tr>
 <td>Le service {{site.data.keyword.cloudcerts_long_notm}} est confronté à un temps d'indisponibilité.</td>
 <td>Vérifiez que votre service {{site.data.keyword.cloudcerts_short}} est opérationnel.</td>
 </tr>
 <tr>
 <td>La valeur confidentielle que vous avez importée porte le même nom que la valeur confidentielle d'Ingress fournie par IBM.</td>
 <td>Renommez votre valeur confidentielle. Vous pouvez vérifier le nom de la valeur confidentielle Ingress fournie par IBM en exécutant la commande `ibmcloud ks cluster-get --cluster <cluster_name_or_ID> | grep Ingress`.</td>
 </tr>
 </tbody></table>

<br />


## Impossible d'obtenir un sous-domaine pour un ALB Ingress, un ALB n'est pas déployé dans une zone ou impossible de déployer un équilibreur de charge
{: #cs_subnet_limit}

{: tsSymptoms}
* Aucun sous-domaine Ingress : lorsque vous exécutez la commande `ibmcloud ks cluster-get --cluster <cluster>`, votre cluster est à l'état `normal` mais aucun **sous-domaine Ingress** n'est disponible. 
* Un ALB n'est pas déployé dans une zone : lorsque vous disposez d'un cluster à zones multiples et que vous exécutez la commande `ibmcloud ks albs <cluster>`, aucun ALB n'est déployé dans une zone. Par exemple, si vous disposez de noeuds worker dans 3 zones différentes, vous pouvez voir une sortie similaire à ce qui suit, où un ALB public ne s'est pas déployé dans la troisième zone.
  ```
  ALB ID                                            Enabled    Status     Type      ALB IP           Zone    Build                          ALB VLAN ID
  private-cr96039a75fddb4ad1a09ced6699c88888-alb1   false      disabled   private   -                dal10   ingress:411/ingress-auth:315   2294021
  private-cr96039a75fddb4ad1a09ced6699c88888-alb2   false      disabled   private   -                dal12   ingress:411/ingress-auth:315   2234947
  private-cr96039a75fddb4ad1a09ced6699c88888-alb3   false      disabled   private   -                dal13   ingress:411/ingress-auth:315   2234943
  public-cr96039a75fddb4ad1a09ced6699c88888-alb1    true       enabled    public    169.xx.xxx.xxx   dal10   ingress:411/ingress-auth:315   2294019
  public-cr96039a75fddb4ad1a09ced6699c88888-alb2    true       enabled    public    169.xx.xxx.xxx   dal12   ingress:411/ingress-auth:315   2234945
  ```
  {: screen}
* Impossible de déployer un équilibreur de charge : lorsque vous décrivez l'objet ConfigMap `ibm-cloud-provider-vlan-ip-config`, il se peut qu'un message d'erreur semblable à celui décrit ci-après s'affiche :
  ```
  kubectl get cm ibm-cloud-provider-vlan-ip-config
  ```
  {: pre}
  ```
  Warning  CreatingLoadBalancerFailed ... ErrorSubnetLimitReached: There are already the maximum number of subnets permitted in this VLAN.
  ```
  {: screen}

{: tsCauses}
Dans les clusters standard, la première fois que vous créez un cluster dans une zone, un VLAN public et un VLAN privé sont automatiquement mis à votre disposition dans cette zone dans votre compte d'infrastructure IBM Cloud (SoftLayer). Dans cette zone, 1 sous-réseau portable est demandé sur le VLAN public que vous spécifiez et 1 sous-réseau portable privé est demandé sur le VLAN privé que vous spécifiez. Pour {{site.data.keyword.containerlong_notm}}, les VLAN sont limités à 40 sous-réseaux. Si le VLAN du cluster d'une zone a déjà atteint cette limite, le **sous-domaine Ingress** ne peut pas être mis à disposition, l'équilibreur de charge d'application (ALB) Ingress public pour cette zone ne peut pas être mis à disposition ou il se peut qu'aucune adresse IP publique portable ne soit disponible pour créer un équilibreur de charge de réseau.

Pour afficher le nombre de sous-réseaux d'un VLAN :
1.  Dans la [console de l'infrastructure IBM Cloud (SoftLayer)](https://cloud.ibm.com/classic?), sélectionnez **Réseau** > **Gestion IP** > **VLAN**.
2.  Cliquez sur le **Numéro de VLAN** du VLAN que vous avez utilisé pour créer votre cluster. Examinez la section **Sous-réseaux** pour voir s'il existe 40 sous-réseaux ou plus.

{: tsResolve}
Si vous avez besoin d'un nouveau VLAN, commandez-en un en [contactant le support {{site.data.keyword.Bluemix_notm}}](/docs/infrastructure/vlans?topic=vlans-ordering-premium-vlans#ordering-premium-vlans). Ensuite, [créez un cluster](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_create) qui utilise ce nouveau VLAN.

Si vous avez un autre VLAN disponible, vous pouvez [configurer le spanning VLAN](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning) dans votre cluster existant. Vous pouvez ensuite ajouter de nouveaux noeuds worker au cluster qui utilise l'autre VLAN avec les sous-réseaux disponibles. Pour vérifier si le spanning VLAN est déjà activé, utilisez la [commande](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get) `ibmcloud ks vlan-spanning-get --region <region>`. 

Si vous n'utilisez pas tous les sous-réseaux du VLAN, vous pouvez réutiliser des sous-réseaux sur le VLAN en les ajoutant au cluster.
1. Vérifiez que le sous-réseau que vous souhaitez utiliser est disponible.
  <p class="note">Le compte d'infrastructure que vous utilisez peut être partagé entre plusieurs comptes {{site.data.keyword.Bluemix_notm}}. Dans ce cas, même si vous exécutez la commande `ibmcloud ks subnets` pour voir les sous-réseaux avec les clusters liés (**Bound Clusters**), vous ne pourrez voir que les informations concernant vos clusters. Vérifiez avec le propriétaire du compte d'infrastructure que les sous-réseaux sont disponibles et qu'ils ne sont pas utilisés par un autre compte ou une autre équipe.</p>

2. Utilisez la se the [commande `ibmcloud ks cluster-subnet-add` ](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_subnet_add) pour faire en sorte qu'un sous-réseau existant soit disponible pour votre cluster. 

3. Vérifiez que le sous-réseau a bien été créé et ajouté à votre cluster. Le CIDR du sous-réseau est répertorié dans la section **Subnet VLANs**.
    ```
    ibmcloud ks cluster-get --showResources <cluster_name_or_ID>
    ```
    {: pre}

    Dans cet exemple de sortie, un deuxième sous-réseau a été ajouté au VLAN public `2234945` :
    ```
    Subnet VLANs
    VLAN ID   Subnet CIDR          Public   User-managed
    2234947   10.xxx.xx.xxx/29     false    false
    2234945   169.xx.xxx.xxx/29    true     false
    2234945   169.xx.xxx.xxx/29    true     false
    ```
    {: screen}

4. Vérifiez que les adresses IP portables issues du sous-réseau que vous avez ajouté soient utilisées pour les ALB ou les équilibreurs de charge dans votre cluster. Il peut s'écouler plusieurs minutes avant que les services n'utilisent les adresses IP portables issues du sous-réseau nouvellement ajouté. 
  * Aucun sous-domaine Ingress : exécutez la commande `ibmcloud ks cluster-get --cluster <cluster>` pour vérifier que la zone **Sous-domaine Ingress** est renseignée. 
  * Un ALB n'est pas déployé dans une zone : exécutez la commande `ibmcloud ks albs --cluster <cluster>` pour vérifier que l'ALB manquant est déployé. 
  * Impossible de déployer un équilibreur de charge : exécutez la commande `kubectl get svc -n kube-system` pour vérifier que l'équilibreur de charge est doté d'une adresse IP externe (valeur **EXTERNAL-IP**).

<br />


## La connexion via WebSocket s'interrompt au bout de 60 secondes
{: #cs_ingress_websocket}

{: tsSymptoms}
Votre service Ingress expose une application qui utilise WebSocket. Cependant, la connexion entre un client et votre application WebSocket s'interrompt lorsqu'aucun trafic n'est échangé entre eux durant 60 secondes.

{: tsCauses}
La connexion à votre application WebSocket peut être abandonnée au bout de 60 secondes d'inactivité pour l'une des raisons suivantes :

* Votre connexion Internet a un proxy ou un pare-feu qui ne tolère pas les connexions longues.
* Une expiration de délai dans l'ALB pour l'application WebSocket met fin à la connexion.

{: tsResolve}
Pour empêcher l'interruption de la connexion au bout de 60 secondes d'inactivité :

1. Si vous vous connectez à votre application WebSocket via un proxy ou un pare-feu, veillez à ce que ce proxy ou ce pare-feu ne soit pas configuré pour mettre fin automatiquement aux connexions longues.

2. Pour maintenir la connexion, vous pouvez augmenter la valeur du délai d'expiration ou définir un signal de présence dans votre application.
<dl><dt>Modifier le délai d'expiration</dt>
<dd>Augmentez la valeur du paramètre `proxy-read-timeout` dans la configuration de votre équilibreur de charge d'application (ALB). Par exemple, pour remplacer le délai de `60s` par une valeur supérieure, telle que `300s`, ajoutez cette [annotation](/docs/containers?topic=containers-ingress_annotation#connection) au fichier de la ressource Ingress : `ingress.bluemix.net/proxy-read-timeout: "serviceName=<service_name> timeout=300s"`. Le délai est modifié pour tous les ALB publics figurant dans votre cluster.</dd>
<dt>Configurer un signal de présence</dt>
<dd>Si vous n'envisagez pas de modifier la valeur du délai de lecture par défaut de l'ALB, configurez un signal de présence dans votre application WebSocket. Lorsque vous configurez un protocole de signal de présence à l'aide d'une infrastructure de type [WAMP ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://wamp-proto.org/), le serveur en amont de l'application envoie périodiquement un message "ping" à intervalles réguliers et le client répond par un message "pong". Définissez l'intervalle du signal de présence avec une valeur inférieure ou égale à 58 secondes pour que le trafic "ping/pong" conserve la connexion active avant la fin effective du délai d'expiration.</dd></dl>

<br />


## La conservation de l'adresse IP source échoue lors de l'utilisation de noeuds avec taint
{: #cs_source_ip_fails}

{: tsSymptoms}
Vous avez activé la conservation de l'adresse IP source pour un service [d'équilibreur de charge version 1.0](/docs/containers?topic=containers-loadbalancer#node_affinity_tolerations) ou [d'ALB Ingress](/docs/containers?topic=containers-ingress#preserve_source_ip) en remplaçant `externalTrafficPolicy` par `Local` dans le fichier de configuration du service. Cependant, aucun trafic n'atteint le service de back end de votre application.

{: tsCauses}
Lorsque vous activez la conservation de l'adresse IP source pour les services d'équilibreur de charge ou d'ALB Ingress, l'adresse IP source de la demande client est conservée. Le service achemine le trafic vers les pods d'application sur le même noeud worker uniquement pour garantir que l'adresse du paquet de demandes n'est pas modifiée. En principe, des pods de service d'équilibreur de charge ou d'ALB Ingress sont déployés sur les mêmes noeuds worker où sont déployés les pods d'application. Il existe cependant des situations où les pods de service et les pods d'application ne sont pas forcément planifiés sur le même noeud worker. Si vous utilisez des [éléments taint de Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/) sur des noeuds worker, l'exécution des pods qui n'ont pas d'élément toleration taint est bloquée sur les noeuds worker avec taint. La conservation de l'adresse IP source peut ne pas fonctionner suivant le type d'élément taint que vous avez utilisé :

* **Eléments taint de noeud de périphérie** : vous avez [ajouté le libellé `dedicated=edge`](/docs/containers?topic=containers-edge#edge_nodes) à au moins deux noeuds worker sur chaque VLAN public dans votre cluster pour garantir que les pods d'équilibreur de charge et d'Ingress se déploient uniquement dans ces noeuds worker. Ensuite, vous avez également [ajouté un élément taint à ces noeuds de périphérie](/docs/containers?topic=containers-edge#edge_workloads) pour empêcher toute autre charge de travail de s'exécuter sur les noeuds de périphérie. Mais vous n'avez pas ajouté de règles d'affinité (affinity) et de tolérance (toleration) dans le déploiement de votre application. Vos pods d'application ne peuvent pas être planifiés sur les mêmes noeuds avec taint que les pods de service et aucun trafic n'atteint le service de back end de votre application.

* **Eléments taint personnalisés** : vous avez utilisé des éléments taint personnalisés sur plusieurs noeuds de sorte que seuls les pods d'application avec un élément toleration taint puissent se déployer sur ces noeuds. Vous avez ajouté des règles d'affinité (affinity) et de tolérance (toleration) aux déploiements de votre application et du service d'équilibreur de charge ou Ingress de sorte que leurs pods se déploient uniquement sur ces noeuds. Cependant, les pods `ibm-cloud-provider-ip` `keepalived` automatiquement créés dans l'espace de nom `ibm-system` garantissent que les pods d'équilibreur de charge et les pods d'application sont toujours planifiés sur le même noeud worker. Ces pods `keepalived` ne disposent pas d'éléments tolerations pour les éléments taint personnalisés que vous avez utilisés. Ils ne peuvent pas être déployés sur les mêmes noeuds avec taint où s'exécutent vos pods d'application, et aucun trafic n'atteint le service de back end de votre application.

{: tsResolve}
Corrigez le problème en choisissant l'une des options suivantes :

* **Eléments taint de noeud de périphérie** : pour garantir que vos pods d'équilibreur de charge et d'application se déploient sur des noeuds avec taint, [ajouter des règles d'affinité de noeud de périphérie et des tolérances dans le déploiement de votre application](/docs/containers?topic=containers-loadbalancer#lb_edge_nodes). Par défaut, les pods d'équilibreur de charge et d'ALB Ingress disposent de ces règles d'affinité et de ces tolérances.

* **Eléments taint personnalisés** : retirez les éléments taint personnalisés pour lesquels les pods `keepalived` ne disposent pas de tolérances (tolerations). Vous pouvez à la place [labelliser les noeuds worker en tant que noeuds de périphérie, puis ajouter un élément taint à ces noeuds de périphérie](/docs/containers?topic=containers-edge).

Si vous exécutez l'une des options ci-dessus mais que les pods `keepalived` ne sont pas encore planifiés, consultez des informations complémentaires sur les pods `keepalived` :

1. Obtenez les pods `keepalived`.
    ```
    kubectl get pods -n ibm-system
    ```
    {: pre}

2. Dans la sortie obtenue, recherchez les pods `ibm-cloud-provider-ip` dont la valeur de **Status** est `Pending`. Exemple :
    ```
    ibm-cloud-provider-ip-169-61-XX-XX-55967b5b8c-7zv9t     0/1       Pending   0          2m        <none>          <none>
    ibm-cloud-provider-ip-169-61-XX-XX-55967b5b8c-8ptvg     0/1       Pending   0          2m        <none>          <none>
    ```
    {:screen}

3. Décrivez chaque pod `keepalived` et recherchez la section **Events**. Traitez tous les messages d'erreur ou d'avertissement qui sont répertoriés.
    ```
    kubectl describe pod ibm-cloud-provider-ip-169-61-XX-XX-55967b5b8c-7zv9t -n ibm-system
    ```
    {: pre}

<br />


## Impossible d'établir une connectivité VPN avec la charte Helm strongSwan
{: #cs_vpn_fails}

{: tsSymptoms}
Lorsque vous vérifiez la connectivité VPN en exécutant `kubectl exec $STRONGSWAN_POD -- ipsec status`, vous ne voyez pas le statut `ESTABLISHED`, ou le pod du VPN est à l'état `ERROR` ou ne cesse de planter et redémarrer.

{: tsCauses}
Le fichier de configuration de votre charte Helm contient des valeurs incorrectes ou manquantes, ou comporte des erreurs de syntaxe.

{: tsResolve}
Lorsque vous essayez d'établir une connectivité VPN avec la charte Helm strongSwan, il est fort probable que le statut du VPN ne soit pas `ESTABLISHED` la première fois. Vous pourrez vérifier plusieurs types d'erreurs et modifier votre fichier de configuration en conséquence. Pour identifier et résoudre les incidents liés à la connectivité VPN strongSwan :

1. [Testez et vérifiez la connectivité VPN strongSwan](/docs/containers?topic=containers-vpn#vpn_test) en exécutant les cinq tests Helm inclus dans la définition de la charte strongSwan.

2. Si vous ne parvenez pas à établir la connectivité VPN après avoir exécuté les tests Helm, vous pouvez exécuter l'outil de débogage VPN inclus dans l'image de pod VPN.

    1. Définissez la variable d'environnement `STRONGSWAN_POD`.

        ```
        export STRONGSWAN_POD=$(kubectl get pod -l app=strongswan,release=vpn -o jsonpath='{ .items[0].metadata.name }')
        ```
        {: pre}

    2. Exécutez l'outil de débogage.

        ```
        kubectl exec  $STRONGSWAN_POD -- vpnDebug
        ```
        {: pre}

        L'outil renvoie plusieurs pages d'informations à mesure qu'il exécute différents tests pour détecter les problèmes réseau courants. Les lignes de sortie commençant par `ERROR`, `WARNING`, `VERIFY` ou `CHECK` indiquent des erreurs possibles liées à la connectivité VPN.

    <br />


## Impossible d'installer une nouvelle édition de charte Helm strongSwan
{: #cs_strongswan_release}

{: tsSymptoms}
Vous modifiez votre charte Helm strongSwan et essayez d'installer une nouvelle édition en exécutant la commande `helm install -f config.yaml --name=vpn ibm/strongswan`. Pourtant, vous obtenez l'erreur suivante :
```
Error: release vpn failed: deployments.extensions "vpn-strongswan" already exists
```
{: screen}

{: tsCauses}
Cette erreur indique que l'édition précédente de la charte strongSwan n'a pas été complètement désinstallée.

{: tsResolve}

1. Supprimez l'édition précédente de la charte.
    ```
    helm delete --purge vpn
    ```
    {: pre}

2. Supprimez le déploiement de l'édition précédente. La suppression du déploiement et du pod associé peut prendre jusqu'à 1 minute.
    ```
    kubectl delete deploy vpn-strongswan
    ```
    {: pre}

3. Vérifiez que le déploiement a été supprimé. Le déploiement `vpn-strongswan` n'apparaît pas dans la liste.
    ```
    kubectl get deployments
    ```
    {: pre}

4. Réinstallez la charte Helm strongSwan mise à jour avec un nouveau nom d'édition.
    ```
    helm install -f config.yaml --name=vpn ibm/strongswan
    ```
    {: pre}

<br />


## La connectivité VPN strongSwan échoue après l'ajout ou la suppression de noeuds worker
{: #cs_vpn_fails_worker_add}

{: tsSymptoms}
Vous avez déjà établi une connexion VPN opérationnelle en utilisant le service VPN IPSec strongSwan. Cependant, après avoir ajouté ou supprimé un noeud worker sur votre cluster, vous expérimentez un ou plusieurs symptômes de ce type :

* Vous n'obtenez pas le statut VPN `ESTABLISHED`
* Vous ne parvenez pas à accéder aux nouveaux noeuds worker à partir de votre réseau local
* Vous ne pouvez pas accéder au réseau distant à partir des pods qui s'exécutent sur les nouveaux noeuds worker

{: tsCauses}
Si vous avez ajouté un noeud worker dans un pool de noeuds worker :

* Le noeud worker a été mis en place sur un nouveau sous-réseau privé qui n'est pas exposé via la connexion VPN avec vos paramètres `localSubnetNAT` ou `local.subnet`
* Les routes VPN ne peuvent pas être ajoutées au noeud worker car celui-ci possède des annotations taint ou des libellés qui ne sont pas inclus dans vos paramètres `tolerations` ou `nodeSelector`
* Le pod VPN s'exécute sur le nouveau noeud worker, mais l'adresse IP publique de ce noeud n'est pas autorisée via le pare-feu local

Si vous avez supprimé un noeud worker :

* Ce noeud worker constituait le seul noeud sur lequel un pod VPN s'exécutait, en raison des restrictions relatives à certaines annotations taint ou libellés dans vos paramètres `tolerations` ou `nodeSelector`

{: tsResolve}
Mettez à jour les valeurs de la charte Helm pour répercuter les modifications du noeud worker :

1. Supprimez la charte Helm existante.

    ```
    helm delete --purge <release_name>
    ```
    {: pre}

2. Ouvrez le fichier de configuration correspondant au service VPN strongSwan.

    ```
    helm inspect values ibm/strongswan > config.yaml
    ```
    {: pre}

3. Vérifiez les paramètres suivants et modifiez les paramètres pour répercuter les noeuds worker ajoutés ou supprimés selon les besoins.

    Si vous avez ajouté un noeud worker :

    <table>
    <caption>Paramètres du noeud worker</caption>
     <thead>
     <th>Paramètre</th>
     <th>Description</th>
     </thead>
     <tbody>
     <tr>
     <td><code>localSubnetNAT</code></td>
     <td>Le noeud worker ajouté peut être déployé sur un nouveau sous-réseau privé différent des autres sous-réseaux existants sur lesquels résident les autres noeuds worker. Si vous utilisez la conversion NAT de sous-réseau pour remapper les adresses IP locales privées de votre cluster et que le noeud worker a été ajouté sur un nouveau sous-réseau, ajoutez le CIDR du nouveau sous-réseau à ce paramètre.</td>
     </tr>
     <tr>
     <td><code>nodeSelector</code></td>
     <td>Si vous avez précédemment limité le déploiement du pod VPN aux noeuds worker avec un libellé spécifique, vérifiez que le noeud worker ajouté comporte également ce libellé.</td>
     </tr>
     <tr>
     <td><code>tolerations</code></td>
     <td>Si le noeud worker ajouté comporte des annotations taint, modifiez ce paramètre pour autoriser le pod VPN à s'exécuter sur tous les noeuds worker avec des annotations taint ou ayant des annotations taint spécifiques.</td>
     </tr>
     <tr>
     <td><code>local.subnet</code></td>
     <td>Le noeud worker ajouté peut être déployé sur un nouveau sous-réseau privé différent des sous-réseaux existants sur lesquels résident les autres noeuds worker. Si vos applications sont exposées par les services NodePort ou LoadBalancer sur le réseau privé et qu'elles sont sur le noeud worker ajouté, ajoutez le nouveau CIDR du sous-réseau à ce paramètre. **Remarque** : si vous ajoutez des valeurs au paramètre `local.subnet`, vérifiez les paramètres VPN du sous-réseau local pour voir s'ils doivent également faire l'objet d'une mise à jour.</td>
     </tr>
     </tbody></table>

    Si vous avez supprimé un noeud worker :

    <table>
    <caption>Paramètres du noeud worker</caption>
     <thead>
     <th>Paramètre</th>
     <th>Description</th>
     </thead>
     <tbody>
     <tr>
     <td><code>localSubnetNAT</code></td>
     <td>Si vous utilisez la conversion NAT de sous-réseau pour remapper des adresses IP locales privées spécifiques, supprimez de ce paramètre les adresses IP provenant de l'ancien noeud worker. Si vous utilisez la conversion NAT de sous-réseau pour remapper des sous-réseaux entiers et qu'aucun noeud worker n'est présent sur un sous-réseau, supprimez le CIDR de sous-réseau de ce paramètre.</td>
     </tr>
     <tr>
     <td><code>nodeSelector</code></td>
     <td>Si vous avez limité le pod VPN pour qu'il s'exécute sur un noeud worker unique et que ce noeud a été supprimé, modifiez ce paramètre pour autoriser le pod VPN à s'exécuter sur d'autres noeuds worker.</td>
     </tr>
     <tr>
     <td><code>tolerations</code></td>
     <td>Si le noeud worker que vous avez supprimé n'avait pas d'annotation taint, mais que seuls les noeuds restants ont des annotations taint, modifiez ce paramètre pour autoriser le pod VPN à s'exécuter sur tous les noeuds comportant des annotations taint ou des annotations taint spécifiques.
     </td>
     </tr>
     </tbody></table>

4. Installez la nouvelle charte Helm avec vos valeurs mises à jour.

    ```
    helm install -f config.yaml --name=<release_name> ibm/strongswan
    ```
    {: pre}

5. Vérifiez le statut de déploiement de la charte. Lorsque la charte est prête, la zone **STATUS** vers le haut de la sortie indique `DEPLOYED`.

    ```
    helm status <release_name>
    ```
    {: pre}

6. Dans certains cas, il vous faudra changer vos paramètres locaux et vos paramètres de pare-feu pour les adapter aux modifications que vous avez apportées au fichier de configuration VPN.

7. Démarrez le VPN.
    * Si la connexion VPN est initialisée par le cluster (`ipsec.auto` est défini sur `start`), démarrez le VPN sur la passerelle locale, puis démarrez le VPN sur le cluster.
    * Si la connexion VPN est initialisée par la passerelle locale (`ipsec.auto` est défini sur `auto`), démarrez le VPN sur le cluster, puis démarrez le VPN sur la passerelle locale.

8. Définissez la variable d'environnement `STRONGSWAN_POD`.

    ```
    export STRONGSWAN_POD=$(kubectl get pod -l app=strongswan,release=<release_name> -o jsonpath='{ .items[0].metadata.name }')
    ```
    {: pre}

9. Vérifiez le statut du réseau privé virtuel.

    ```
    kubectl exec $STRONGSWAN_POD -- ipsec status
    ```
    {: pre}

    * Si la connexion VPN a le statut `ESTABLISHED`, la connexion a abouti. Aucune autre action n'est requise.

    * Si vous rencontrez toujours des problèmes de connexion, voir [Impossible d'établir une connectivité VPN avec la charte Helm strongSwan](#cs_vpn_fails) pour continuer à résoudre les problèmes de connexion VPN.

<br />



## Récupération impossible des règles réseau Calico
{: #cs_calico_fails}

{: tsSymptoms}
Lorsque vous essayez d'afficher les règles réseau Calico dans votre cluster en exécutant la commande `calicoctl get policy`, vous obtenez l'un des résultats imprévisibles ou messages d'erreur suivants :
- Une liste vide
- Une liste de règles Calico v2 au lieu de règles v3
- `Failed to create Calico API client: syntax error in calicoctl.cfg: invalid config file: unknown APIVersion 'projectcalico.org/v3'`

Lorsque vous essayez d'afficher les règles réseau Calico dans votre cluster en exécutant la commande `calicoctl get GlobalNetworkPolicy`, vous obtenez l'un des résultats imprévisibles ou messages d'erreur suivants :
- Une liste vide
- `Failed to create Calico API client: syntax error in calicoctl.cfg: invalid config file: unknown APIVersion 'v1'`
- `Failed to create Calico API client: syntax error in calicoctl.cfg: invalid config file: unknown APIVersion 'projectcalico.org/v3'`
- `Failed to get resources: Resource type 'GlobalNetworkPolicy' is not supported`

{: tsCauses}
Pour utiliser des règles Calico, quatre facteurs doivent être en phase : la version de votre cluster Kubernetes, la version de l'interface de ligne de commande (CLI) Calico, la syntaxe du fichier de configuration Calico et les commandes d'affichage des règles. Au moins un de ces facteurs n'est pas à la version correcte.

{: tsResolve}
Vous devez utiliser l'interface CLI de Calico v3.3 ou ultérieure, la syntaxe du fichier de configuration `calicoctl.cfg` v3 et les commandes `calicoctl get GlobalNetworkPolicy` et `calicoctl get NetworkPolicy`.

Pour vous assurer que tous les facteurs Calico sont en phase :

1. [Installez et configurez l'interface CLI de Calico version 3.3 ou ultérieure](/docs/containers?topic=containers-network_policies#cli_install).
2. Vérifiez que les règles que vous créez et que vous voulez appliquer à votre cluster utilisent la [syntaxe de Calico v3 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://docs.projectcalico.org/v3.3/reference/calicoctl/resources/networkpolicy). Si vous disposez d'un fichier `.yaml` de règles existant ou d'un fichier `.json` avec la syntaxe de Calico v2, vous pouvez le convertir en syntaxe de Calico v3 en utilisant la commande [`calicoctl convert` ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://docs.projectcalico.org/v3.3/reference/calicoctl/commands/convert).
3. Pour [afficher les règles](/docs/containers?topic=containers-network_policies#view_policies), prenez soin d'utiliser la commande `calicoctl get GlobalNetworkPolicy` pour les règles globales et `calicoctl get NetworkPolicy --namespace <policy_namespace>` pour les règles limitées à des espaces de nom spécifiques.

<br />


## Impossible d'ajouter des noeuds worker en raison d'un ID de VLAN non valide
{: #suspended}

{: tsSymptoms}
Votre compte {{site.data.keyword.Bluemix_notm}} a été suspendu ou tous les noeuds worker de votre cluster ont été supprimés. Une fois le compte réactivé, vous ne pouvez pas ajouter de noeuds worker lorsque vous tentez de redimensionner ou de rééquilibrer votre pool de noeuds worker. Vous voyez un message d'erreur de ce type :

```
SoftLayerAPIError(SoftLayer_Exception_Public): Could not obtain network VLAN with id #123456.
```
{: screen}

{: tsCauses}
Lorsqu'un compte est suspendu, les noeuds worker qui figuraient dans le compte sont supprimés. S'il n'y a aucun noeud worker dans un cluster, l'infrastructure IBM Cloud (SoftLayer) récupère les VLAN public et privé associés. Cependant, le pool de noeuds worker dispose toujours des ID de VLAN précédents parmi ses métadonnées et il utilise ces deux ID qui ne sont plus disponibles lors du rééquilibrage ou du redimensionnement du pool. Les noeuds ne parviennent pas à être créés car ces VLAN ne sont plus associés au cluster.

{: tsResolve}

Vous pouvez [supprimer votre pool de noeuds worker](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_rm), puis [créer un nouveau pool de noeuds worker](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_create).

Sinon, vous pouvez conserver votre pool de noeuds worker en commandant des nouveaux VLAN que vous utiliserez pour créer de nouveaux noeuds worker dans le pool.

Avant de commencer : [connectez-vous à votre compte. Le cas échéant, ciblez le groupe de ressources approprié. Définissez le contexte pour votre cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  Pour obtenir les zones pour lesquelles vous devez obtenir les nouveaux ID de VLAN, notez l'**emplacement** dans la sortie de la commande suivante. **Remarque** : Si vous disposez d'un cluster à zones multiples, vous avez besoin des ID de VLAN pour chaque zone.

    ```
    ibmcloud ks clusters
    ```
    {: pre}

2.  Obtenez un nouveau VLAN privé et un nouveau VLAN public pour chaque zone dans laquelle réside votre cluster en [contactant le support {{site.data.keyword.Bluemix_notm}}](/docs/infrastructure/vlans?topic=vlans-ordering-premium-vlans#ordering-premium-vlans).

3.  Notez les ID des nouveaux VLAN privé et VLAN public pour chaque zone.

4.  Notez le nom de votre pool de noeuds worker.

    ```
    ibmcloud ks worker-pools --cluster <cluster_name_or_ID>
    ```
    {: pre}

5.  Utilisez la [commande](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_zone_network_set) `zone-network-set` pour modifier les métadonnées de réseau du pool de noeuds worker. 

    ```
    ibmcloud ks zone-network-set --zone <zone> --cluster <cluster_name_or_ID> -- worker-pools <worker-pool> --private-vlan <private_vlan_ID> --public-vlan <public_vlan_ID>
    ```
    {: pre}

6.  **Cluster à zones multiples uniquement** : répétez l'**étape 5** pour chaque zone dans votre cluster.

7.  Rééquilibrez ou redimensionnez votre pool de noeuds worker pour ajouter des noeuds worker utilisant les nouveaux ID des VLAN. Exemple :

    ```
    ibmcloud ks worker-pool-resize --cluster <cluster_name_or_ID> --worker-pool <worker_pool> --size-per-zone <number_of_workers_per_zone>
    ```
    {: pre}

8.  Vérifiez que vos noeuds worker ont été créés.

    ```
    ibmcloud ks workers --cluster <cluster_name_or_ID> --worker-pool <worker_pool>
    ```
    {: pre}

<br />



## Aide et assistance
{: #network_getting_help}

Vous avez encore des problèmes avec votre cluster ?
{: shortdesc}

-  Dans le terminal, vous êtes averti des mises à jour disponibles pour l'interface de ligne de commande `ibmcloud` et les plug-ins. Veillez à maintenir votre interface de ligne de commande à jour pour pouvoir utiliser l'ensemble des commandes et des indicateurs.
-   Pour voir si {{site.data.keyword.Bluemix_notm}} est disponible, [vérifiez la page Statut d'{{site.data.keyword.Bluemix_notm}} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/status?selected=status).
-   Publiez une question sur le site [{{site.data.keyword.containerlong_notm}} Slack ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://ibm-container-service.slack.com).
    Si vous n'utilisez pas un ID IBM pour votre compte {{site.data.keyword.Bluemix_notm}}, [demandez une invitation](https://bxcs-slack-invite.mybluemix.net/) sur ce site Slack.
    {: tip}
-   Consultez les forums pour établir si d'autres utilisateurs ont rencontré le même problème. Lorsque vous utilisez les forums pour poser une question, balisez votre question de sorte que les équipes de développement {{site.data.keyword.Bluemix_notm}} la voient.
    -   Si vous avez des questions d'ordre technique sur le développement ou le déploiement de clusters ou d'applications à l'aide d'{{site.data.keyword.containerlong_notm}}, publiez-les sur le site [Stack Overflow ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://stackoverflow.com/questions/tagged/ibm-cloud+containers) en leur adjoignant les balises `ibm-cloud`, `kubernetes` et `containers`.
    -   Pour toute question sur le service et les instructions de mise en route, utilisez le forum [IBM Developer Answers ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix). Incluez les balises `ibm-cloud` et `containers`.
    Voir [Comment obtenir de l'aide](/docs/get-support?topic=get-support-getting-customer-support#using-avatar) pour plus d'informations sur l'utilisation des forums.
-   Contactez le support IBM en ouvrant un cas. Pour savoir comment ouvrir un cas de support IBM ou obtenir les niveaux de support et la gravité des cas, voir [Contacter le support](/docs/get-support?topic=get-support-getting-customer-support).
Lorsque vous signalez un problème, incluez l'ID de votre cluster. Pour identifier l'ID du cluster, exécutez la commande `ibmcloud ks clusters`. Vous pouvez également utiliser l'[outil de débogage et de diagnostic d'{{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-cs_troubleshoot#debug_utility) pour regrouper et exporter des informations pertinentes de votre cluster à partager avec le support IBM.
{: tip}

