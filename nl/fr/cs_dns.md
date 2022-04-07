---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-11"

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


# Configuration du fournisseur DNS de cluster
{: #cluster_dns}

Un nom DNS (Domain Name System) est affecté à chaque service dans votre cluster {{site.data.keyword.containerlong}} et est enregistré par le fournisseur DNS de cluster pour résoudre les demandes DNS. En fonction de la version Kubernetes de votre cluster, vous pouvez choisir entre le fournisseur Kubernetes DNS (KubeDNS) ou [CoreDNS ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://coredns.io/). Pour plus d'informations sur DNS pour les services et les pods, voir [la documentation Kubernetes![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/concepts/services-networking/dns-pod-service/).
{: shortdesc}

**Quelles sont les versions de Kubernetes prises en charge selon les fournisseurs DNS de cluster ?**<br>

| Version Kubernetes | Valeur par défaut pour les nouveaux clusters | Description |
|---|---|---|
| 1.14 et ultérieure | CoreDNS | Si un cluster utilise KubeDNS et est mis à jour vers la version 1.14 ou une version ultérieure, le fournisseur DNS de cluster est migré automatiquement depuis KubeDNS vers CoreDNS durant la mise à jour du cluster.  Vous ne pouvez pas changer de fournisseur DNS pour revenir à KubeDNS. |
| 1.13 | CoreDNS | Les clusters qui mis à jour vers la version 1.13 à partir d'une version antérieure conservent le fournisseur DNS qu'ils utilisaient au moment de la mise à jour. Si vous souhaitez en utiliser un autre, [changez de fournisseur DNS](#dns_set). |
| 1.12 | KubeDNS | Pour utiliser CoreDNS à la place, [changez de fournisseur DNS](#set_coredns). |
| 1.11 ou antérieure | KubeDNS | Vous ne pouvez pas changer de fournisseur DNS pour passer à CoreDNS. |
{: caption="Fournisseur DNS de cluster par défaut en fonction des versions de Kubernetes" caption-side="top"}

**Quels sont les avantages liés à l'utilisation de CoreDNS au lieu de KubeDNS ?**<br>
CoreDNS est le fournisseur DNS de cluster pris en charge par défaut pour Kubernetes version 1.13 et versions ultérieures, et il est récemment devenu un [projet Cloud Native Computing Foundation (CNCF) gradué ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.cncf.io/projects/). Un projet gradué est minutieusement testé, renforcé et il est prêt pour une adoption au niveau de la production. 

Comme mentionné dans l'[annonce Kubernetes![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/blog/2018/12/03/kubernetes-1-13-release-announcement/), CoreDNS est un serveur DNS à usage général faisant autorité qui fournit une intégration à Kubernetes compatible avec les versions antérieures et extensible. Etant donné que CoreDNS est un simple fichier exécutable et un processus unique, il a moins de dépendances et de pièces mobiles susceptibles de présenter des problèmes que le précédent fournisseur DSN de cluster. Le projet est également écrit dans le même langage que le projet Kubernetes, `Go`, ce qui contribue à protéger la mémoire. Enfin, CoreDNS prend en charge des cas d'utilisation plus souples que KubeDNS car vous pouvez créer des entrées DNS personnalisées, telles que des [configurations communes dans la documentation CoreDNS ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://coredns.io/manual/toc/#setups).

## Mise à l'échelle automatique du fournisseur DNS de cluster
{: #dns_autoscale}

Par défaut, votre fournisseur DNS de cluster {{site.data.keyword.containerlong_notm}} comprend un déploiement pour la mise à l'échelle automatique des pods DNS en accord avec le nombre de noeuds worker et de coeurs présents dans le cluster. Vous pouvez affiner les paramètres du programme de mise à l'échelle automatique de DNS en modifiant l'élément configmap correspondant à la mise à l'échelle automatique de DNS. Par exemple, si vos applications utilisent beaucoup le fournisseur DNS de cluster, vous devrez peut-être augmenter le nombre minimal de pods DNS pour prendre en charge l'application. Pour plus d'informations, voir [la documentation Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/tasks/administer-cluster/dns-horizontal-autoscaling/).
{: shortdesc}

Avant de commencer : [connectez-vous à votre compte. Le cas échéant, ciblez le groupe de ressources approprié. Définissez le contexte pour votre cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  Vérifiez que le déploiement du fournisseur DNS de cluster est disponible. Vous pouvez avoir un programme de mise à l'échelle automatique pour les fournisseurs KubeDNS, CoreDNS, ou pour ces deux fournisseurs, installé dans votre cluster. Si les deux programmes de mise à l'échelle automatique de DNS sont installés, recherchez celui qui est utilisé en examinant la colonne **AVAILABLE** dans la sortie de l'interface de ligne de commande. Le déploiement qui est utilisé est répertorié avec un déploiement disponible.
    ```
    kubectl get deployment -n kube-system | grep dns-autoscaler
    ```
    {: pre}

    Exemple de sortie :
    ```
    NAME                   DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
    coredns-autoscaler     0         0         0            0           32d
    kube-dns-autoscaler    1         1         1            1           260d
    ```
    {: screen}
2.  Procurez-vous le nom de la mappe de configuration (configmap) des paramètres du programme de mise à l'échelle automatique de DNS.
    ```
    kubectl get configmap -n kube-system
    ```
    {: pre}    
3.  Editez les paramètres par défaut pour le programme de mise à l'échelle automatique de DNS. Recherchez la zone `data.linear`, qui a la valeur par défaut de un pod DNS pour 16 noeuds worker ou 256 coeurs, avec un minimum de deux pods DNS indépendamment de la taille du cluster (`preventSinglePointFailure: true`). Pour plus d'informations, voir [la documentation Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/tasks/administer-cluster/dns-horizontal-autoscaling/#tuning-autoscaling-parameters).
    ```
    kubectl edit configmap -n kube-system <dns-autoscaler>
    ```
    {: pre}

    Exemple de sortie :
    ```
    apiVersion: v1
    data:
      linear: '{"coresPerReplica":256,"nodesPerReplica":16,"preventSinglePointFailure":true}'
    kind: ConfigMap
    metadata:
    ...
    ```
    {: screen}

## Personnalisation du fournisseur DNS de cluster
{: #dns_customize}

Vous pouvez personnaliser votre fournisseur DNS de cluster {{site.data.keyword.containerlong_notm}} en éditant la mappe de configuration de DNS. Par exemple, vous envisagez peut-être de configurer des domaines de substitution (`stubdomains`) et des serveurs de nom (nameservers) en amont pour résoudre les services qui pointent vers des hôtes externes. Par ailleurs, si vous utilisez CoreDNS, vous pouvez configurer plusieurs fichiers [Corefiles ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://coredns.io/2017/07/23/corefile-explained/) dans la mappe de configuration (confimap) CoreDNS. Pour plus d'informations, voir [la documentation Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/tasks/administer-cluster/dns-custom-nameservers/).
{: shortdesc}

Avant de commencer : [connectez-vous à votre compte. Le cas échéant, ciblez le groupe de ressources approprié. Définissez le contexte pour votre cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  Vérifiez que le déploiement du fournisseur DNS de cluster est disponible. Vous pouvez avoir un fournisseur DNS de cluster pour les fournisseurs KubeDNS, CoreDNS, ou pour ces deux fournisseurs, installé dans votre cluster. Si les deux fournisseurs DNS sont installés, recherchez celui qui est utilisé en examinant la colonne **AVAILABLE** dans la sortie de l'interface de ligne de commande. Le déploiement qui est utilisé est répertorié avec un déploiement disponible.
    ```
    kubectl get deployment -n kube-system -l k8s-app=kube-dns
    ```
    {: pre}

    Exemple de sortie :
    ```
    NAME                   DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
    coredns                0         0         0            0           89d
    kube-dns-amd64         2         2         2            2           89d
    ```
    {: screen}
2.  Editez les paramètres par défaut pour la mappe de configuration (configmap) de CoreDNS ou KubeDNS.

    *   **Pour CoreDNS** : utilisez un fichier Corefile dans la section `data` de configmap pour personnaliser des domaines de substitution (`stubdomains`) et des serveurs de nom en amont. Pour plus d'informations, voir [la documentation Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/tasks/administer-cluster/dns-custom-nameservers/#coredns).
        ```
        kubectl edit configmap -n kube-system coredns
        ```
        {: pre}

        **Exemple de sortie pour CoreDNS** :
          ```
          apiVersion: v1
          kind: ConfigMap
          metadata:
            name: coredns
            namespace: kube-system
          data:
            Corefile: |
              abc.com:53 {
                  errors
                  cache 30
                  proxy . 1.2.3.4
              }
              .:53 {
                  errors
                  health
                  kubernetes cluster.local in-addr.arpa ip6.arpa {
                     pods insecure
                     upstream 172.16.0.1
                     fallthrough in-addr.arpa ip6.arpa
                  }
                  prometheus :9153
                  proxy . /etc/resolv.conf
                  cache 30
                  loop
                  reload
                  loadbalance
              }
          ```
          {: screen}

          Vous avez de nombreuses personnalisations à organiser ? Dans Kubernetes version 1.12.6_1543 ou ultérieure, vous pouvez ajouter plusieurs fichiers Corefiles dans la configmap de CoreDNS. Pour plus d'informations, voir [la documentation sur l'importation de fichier Corefile ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://coredns.io/plugins/import/).
          {: tip}

    *   **Pour KubeDNS** : configurez des domaines de substitution (`stubdomains`) et des serveurs de nom en amont dans la section `data` de configmap. Pour plus d'informations, voir [la documentation Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/tasks/administer-cluster/dns-custom-nameservers/#kube-dns).
        ```
        kubectl edit configmap -n kube-system kube-dns
        ```
        {: pre}

        **Exemple de sortie pour KubeDNS** :
        ```
        apiVersion: v1
        kind: ConfigMap
        metadata:
          name: kube-dns
          namespace: kube-system
        data:
          stubDomains: |
          {"abc.com": ["1.2.3.4"]}
        ```
        {: screen}

## Configuration d'un fournisseur DNS de cluster avec CoreDNS ou KubeDNS
{: #dns_set}

Si vous disposez d'un cluster {{site.data.keyword.containerlong_notm}} exécutant Kubernetes version 1.12 ou 1.13, vous pouvez choisir entre Kubernetes DNS (KubeDNS) ou CoreDNS comme fournisseur DNS de cluster.
{: shortdesc}

Les clusters qui exécutent d'autres versions Kubernetes ne peuvent pas définir le fournisseur DNS de cluster. La version 1.11 et les versions antérieures prennent en charge uniquement KubeDNS, et la version 1.14 et les versions ultérieures prennent en charge uniquement CoreDNS.
{: note}

**Avant de commencer** :
1.  [Connectez-vous à votre compte. Le cas échéant, ciblez le groupe de ressources approprié. Définissez le contexte pour votre cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
2.  Déterminez le fournisseur DNS de cluster actuel. Dans l'exemple suivant, il s'agit de KubeDNS.
    ```
    kubectl cluster-info
    ```
    {: pre}

    Exemple de sortie :
    ```
    ...
    KubeDNS is running at https://c2.us-south.containers.cloud.ibm.com:20190/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
    ...
    ```
    {: screen}
3.  En fonction du fournisseur DNS utilisé par votre cluster, procédez comme suit pour changer de fournisseur DNS.
    *  [Passer à CoreDNS](#set_coredns).
    *  [Passer à KubeDNS](#set_kubedns).

### Configuration de CoreDNS comme fournisseur DNS de cluster
{: #set_coredns}

Configurez CoreDNS au lieu de KubeDNS comme fournisseur DNS de cluster.
{: shortdesc}

1.  Si vous avez personnalisé la configmap du fournisseur KubeDNS ou du programme de mise à l'échelle automatique KubeDNS, reportez les personnalisations appliquées dans les configmaps de CoreDNS.
    *   Pour la configmap de `kube-dns` dans l'espace de nom `kube-system`, reportez les [personnalisations de DNS ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/tasks/administer-cluster/dns-custom-nameservers/) dans la configmap de `coredns` dans l'espace de nom `kube-system`. La syntaxe est différente entre les éléments configmap `kube-dns` et `coredns`. Pour consulter un exemple, voir [la documentation Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/tasks/administer-cluster/dns-custom-nameservers/#coredns-configuration-equivalent-to-kube-dns).
    *   Pour la configmap `kube-dns-autoscaler` dans l'espace de nom `kube-system`, reportez les [personnalisations du programme de mise à l'échelle automatique du service DNS ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/tasks/administer-cluster/dns-horizontal-autoscaling/) dans la configmap `coredns-autoscaler` dans l'espace de nom `kube-system`. La syntaxe des personnalisations ne change pas.
2.  Réduisez le déploiement du programme de mise à l'échelle automatique de KubeDNS.
    ```
    kubectl scale deployment -n kube-system --replicas=0 kube-dns-autoscaler
    ```
    {: pre}
3.  Attendez que les pods soient supprimés.
    ```
    kubectl get pods -n kube-system -l k8s-app=kube-dns-autoscaler
    ```
    {: pre}
4.  Réduisez le déploiement de KubeDNS.
    ```
    kubectl scale deployment -n kube-system --replicas=0 kube-dns-amd64
    ```
    {: pre}
5.  Augmentez le déploiement du programme de mise à l'échelle automatique de CoreDNS.
    ```
    kubectl scale deployment -n kube-system --replicas=1 coredns-autoscaler
    ```
    {: pre}
6.  Labellisez et annotez le service DNS de cluster pour CoreDNS.
    ```
    kubectl label service --overwrite -n kube-system kube-dns kubernetes.io/name=CoreDNS
    ```
    {: pre}
    ```
    kubectl annotate service --overwrite -n kube-system kube-dns prometheus.io/port=9153
    ```
    {: pre}
    ```
    kubectl annotate service --overwrite -n kube-system kube-dns prometheus.io/scrape=true
    ```
    {: pre}
7.  **Facultatif** : si vous envisagez d'utiliser Prometheus pour collecter des métriques sur les pods CoreDNS, vous devez ajouter un port de métriques dans le service `kube-dns` à partir duquel vous effectuez la migration.
    ```
    kubectl -n kube-system patch svc kube-dns --patch '{"spec": {"ports": [{"name":"metrics","port":9153,"protocol":"TCP"}]}}' --type strategic
    ```
    {: pre}



### Configuration de KubeDNS comme fournisseur DNS de cluster
{: #set_kubedns}

Configurez KubeDNS au lieu de CoreDNS comme fournisseur DNS de cluster.
{: shortdesc}

1.  Si vous avez personnalisé la configmap du fournisseur CoreDNS ou du programme de mise à l'échelle automatique CoreDNS, reportez les personnalisations appliquées dans les configmaps de KubeDNS.
    *   Pour la configmap de `coredns` dans l'espace de nom `kube-system`, reportez les [personnalisations de DNS ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/tasks/administer-cluster/dns-custom-nameservers/) dans la configmap de `kube-dns` dans l'espace de nom `kube-system`. La syntaxe est différente entre les éléments configmap `kube-dns` et `coredns`. Pour consulter un exemple, voir [la documentation Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/tasks/administer-cluster/dns-custom-nameservers/#coredns-configuration-equivalent-to-kube-dns).
    *   Pour la configmap `coredns-autoscaler` dans l'espace de nom `kube-system`, reportez les [personnalisations du programme de mise à l'échelle automatique du service DNS ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/tasks/administer-cluster/dns-horizontal-autoscaling/) dans la configmap `kube-dns-autoscaler` dans l'espace de nom `kube-system`. La syntaxe des personnalisations ne change pas.
2.  Réduisez le déploiement du programme de mise à l'échelle automatique de CoreDNS.
    ```
    kubectl scale deployment -n kube-system --replicas=0 coredns-autoscaler
    ```
    {: pre}
3.  Attendez que les pods soient supprimés.
    ```
    kubectl get pods -n kube-system -l k8s-app=coredns-autoscaler
    ```
    {: pre}
4.  Réduisez le déploiement de CoreDNS.
    ```
    kubectl scale deployment -n kube-system --replicas=0 coredns
    ```
    {: pre}
5.  Augmentez le déploiement du programme de mise à l'échelle automatique de KubeDNS.
    ```
    kubectl scale deployment -n kube-system --replicas=1 kube-dns-autoscaler
    ```
    {: pre}
6.  Labellisez et annotez le service DNS de cluster pour KubeDNS.
    ```
    kubectl label service --overwrite -n kube-system kube-dns kubernetes.io/name=KubeDNS
    ```
    {: pre}
    ```
    kubectl annotate service --overwrite -n kube-system kube-dns prometheus.io/port-
    ```
    {: pre}
    ```
    kubectl annotate service --overwrite -n kube-system kube-dns prometheus.io/scrape-
    ```
    {: pre}
7.  **Facultatif** : si vous avez utilisé Prometheus pour collecter des métriques sur les pods CoreDNS, votre service  `kube-dns` comporte un port pour les métriques. Comme KubeDNS n'a pas besoin d'inclure ce port de métriques, vous pouvez retirer le port du service.
    ```
    kubectl -n kube-system patch svc kube-dns --patch '{"spec": {"ports": [{"name":"dns","port":53,"protocol":"UDP"},{"name":"dns-tcp","port":53,"protocol":"TCP"}]}}' --type merge
    ```
    {: pre}
