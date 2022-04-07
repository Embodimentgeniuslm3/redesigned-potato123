---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: kubernetes, iks, oks, iro, openshift, red hat, red hat openshift, rhos

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

# Tutoriel : Création d'un cluster Red Hat OpenShift on IBM Cloud (bêta)
{: #openshift_tutorial}

Red Hat OpenShift on IBM Cloud est disponible en version bêta pour tester des clusters OpenShift. Les fonctions d'{{site.data.keyword.containerlong}} ne sont pas toutes disponibles dans la version bêta. De plus, les clusters bêta OpenShift que vous créez ne sont conservés que pendant 30 jours après l'expiration de la version bêta et après la commercialisation de Red Hat OpenShift on IBM Cloud.
{: preview}

Avec la version **bêta Red Hat OpenShift on IBM Cloud**, vous pouvez créer des clusters {{site.data.keyword.containerlong_notm}} dotés de noeuds worker qui sont fournis installés avec le logiciel de plateforme d'orchestration de conteneur OpenShift. Vous bénéficiez de tous les [avantages d'{{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-responsibilities_iks) géré pour votre environnement d'infrastructure de cluster tout en utilisant les [outils OpenShift et le catalogue ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://docs.openshift.com/container-platform/3.11/welcome/index.html) qui s'exécute sur Red Hat Enterprise Linux pour vos déploiements d'application.
{: shortdesc}

Les noeuds worker OpenShift sont disponibles uniquement pour les clusters standard. Red Hat OpenShift on IBM Cloud prend en charge OpenShift version 3.11 uniquement, qui inclut Kubernetes version 1.11.
{: note}

## Objectifs
{: #openshift_objectives}

Au cours des leçons de ce tutoriel, vous créez un cluster Red Hat OpenShift on IBM Cloud standard, vous ouvrez la console OpenShift, vous accédez à des composants OpenShift intégrés, vous déployez une application qui utilise des services {{site.data.keyword.Bluemix_notm}} dans un projet OpenShift et vous exposez l'application sur une route OpenShift de manière à permettre aux utilisateurs externes d'accéder au service.
{: shortdesc}

Cette page contient également des informations sur l'architecture de cluster OpenShift et les limitations de la version bêta, et explique comment fournir des commentaires et obtenir de l'aide.

## Durée
{: #openshift_time}
45 minutes

## Public
{: #openshift_audience}

Ce tutoriel est destiné aux administrateurs de cluster désireux d'apprendre à créer un cluster Red Hat OpenShift on IBM Cloud.
{: shortdesc}

## Prérequis
{: #openshift_prereqs}

*   Vérifiez que vous disposez des règles d'accès {{site.data.keyword.Bluemix_notm}} IAM suivantes :
    *   [Rôle de plateforme **Administrateur**](/docs/containers?topic=containers-users#platform) pour {{site.data.keyword.containerlong_notm}}
    *   [Rôle de service **Auteur** ou **Responsable**](/docs/containers?topic=containers-users#platform) pour {{site.data.keyword.containerlong_notm}}
    *   [Rôle de plateforme **Administrateur**](/docs/containers?topic=containers-users#platform) pour {{site.data.keyword.registrylong_notm}}
*    Assurez-vous que la [clé d'API](/docs/containers?topic=containers-users#api_key) pour la région et le groupe de ressources {{site.data.keyword.Bluemix_notm}} est configurée avec les droits d'infrastructure appropriés, **Superutilisateur**, ou les [rôles minimum](/docs/containers?topic=containers-access_reference#infra) pour créer un cluster. 
*   Installez les outils de ligne de commande. 
    *   [Installez l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}}(`ibmcloud`), le plug-in {{site.data.keyword.containershort_notm}} (`ibmcloud ks`) et le plug-in {{site.data.keyword.registryshort_notm}} (`ibmcloud cr`)](/docs/containers?topic=containers-cs_cli_install#cs_cli_install_steps).
    *   [Installez les interfaces de ligne de commande OpenShift Origin (`oc`) et Kubernetes (`kubectl`)](/docs/containers?topic=containers-cs_cli_install#cli_oc).

<br />


## Présentation de l'architecture
{: #openshift_architecture}

Le diagramme et le tableau suivants décrivent les composants par défaut qui sont configurés dans une architecture Red Hat OpenShift on IBM Cloud :
{: shortdesc}

![Architecture de cluster Red Hat OpenShift on IBM Cloud](images/cs_org_ov_both_ses_rhos.png)

| Composants du maître| Description |
|:-----------------|:-----------------|
|Répliques | Les composants du maître, y compris le serveur d'API OpenShift Kubernetes et le magasin de données etcd, possèdent trois répliques et, s'ils se trouvent dans une agglomération à zones multiples, ils sont répartis entre les zones pour une haute disponibilité plus élevée. Les composants du maître sont sauvegardés toutes les 8 heures.|
| `rhos-api` |Le serveur d'API OpenShift Kubernetes constitue le point d'entrée principal pour toutes les demandes de gestion de cluster du noeud worker au maître Kubernetes. Le serveur d'API valide et traite les demandes qui modifient l'état des ressources Kubernetes, tels que les pods ou les services, et enregistre cet état dans le magasin de données etcd.|
| `openvpn-server` | Le serveur OpenVPN utilise le client OpenVPN pour connecter le maître au noeud worker de manière sécurisée. Cette connexion prend en charge les appels `apiserver proxy` vers vos pods et services, ainsi que les appels `kubectl exec`, `attach` et `logs` vers le kubelet.|
| `etcd` | etcd est un magasin de paires clé/valeur à haute disponibilité qui contient l'état de toutes les ressources Kubernetes d'un cluster, comme par exemple les services, les déploiements et les pods. Les données dans etcd sont sauvegardées sur une instance de stockage chiffrée gérée par IBM.|
| `rhos-controller` |Le gestionnaire de contrôleurs OpenShift examine les pods qui viennent d'être créés et décide de l'emplacement de leur déploiement en fonction de la capacité, des besoins en matière de performances, des contraintes en matière de réglementation, des spécifications en matière d'anti-affinité et des exigences liées aux charges de travail. Si aucun noeud worker ne répond à ces exigences, le pod n'est pas déployé dans le cluster. Le contrôleur examine également l'état des ressources de cluster, telles que les jeux de répliques. Lorsque l'état d'une ressource change, par exemple si un pod d'un jeu de répliques tombe en panne, le gestionnaire de contrôleurs initie les actions correctives pour atteindre l'état requis. `rhos-controller` fonctionne comme le planificateur et le gestionnaire de contrôleurs dans une configuration Kubernetes native. |
| `cloud-controller-manager` | Le gestionnaire de contrôleurs de cloud gère des composants propres au fournisseur de cloud, tels que l'équilibreur de charge {{site.data.keyword.Bluemix_notm}}. |
{: caption="Tableau 1. Composants du maître OpenShift" caption-side="top"}
{: #rhos-components-1}
{: tab-title="Master"}
{: tab-group="rhos-components"}
{: class="simple-tab-table"}

| Composants de noeud worker | Description |
|:-----------------|:-----------------|
|Système d'exploitation | Les noeuds Red Hat OpenShift on IBM Cloud s'exécutent sur le système d'exploitation Red Hat Enterprise Linux 7 (RHEL 7). |
|Projets| OpenShift organise vos ressources dans des projets, qui sont des espaces de nom Kubernetes avec des annotations, et inclut beaucoup plus de composants que les clusters Kubernetes natifs pour exécuter des fonctions OpenShift telles que le catalogue. Les lignes suivantes décrivent certains composants de projets. Pour plus d'informations, voir [Projets et utilisateurs ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://docs.openshift.com/container-platform/3.11/architecture/core_concepts/projects_and_users.html).|
| `kube-system` | Cet espace de nom inclut de nombreux composants qui sont utilisés pour exécuter Kubernetes sur le noeud worker. <ul><li>**`ibm-master-proxy`** : `ibm-master-proxy` est ensemble de démons qui transfère les demandes du noeud worker aux adresses IP des répliques du maître à haute disponibilité. Dans les clusters à zone unique, le maître comporte trois répliques sur des hôtes distincts. Pour les clusters qui se trouvent dans une zone compatible avec plusieurs zones, le maître comporte trois répliques réparties sur les différentes zones. Un équilibreur de charge à haute disponibilité transfère les demandes vers le nom de domaine maître aux répliques du maître. </li><li>**`openvpn-client`** : Le client OpenVPN utilise le serveur OpenVPN pour connecter le maître au noeud worker de manière sécurisée. Cette connexion prend en charge les appels `apiserver proxy` vers vos pods et services, ainsi que les appels `kubectl exec`, `attach` et `logs` vers le kubelet.</li><li>**`kubelet`** : Le kubelet est un agent de noeud worker qui s'exécute sur tous les noeuds worker et qui est chargé de surveiller l'intégrité des pods qui s'exécutent sur le noeud worker et de contrôler les événements envoyés par le serveur d'API Kubernetes. En fonction des événements, le kubelet crée ou supprime des pods, assure la mise en place des sondes Liveness probe et Readiness probe et renvoie le statut des pods au serveur d'API Kubernetes.</li><li>**`calico`** : Calico gère les règles réseau pour votre cluster et inclut un petit nombre de composants pour gérer la connectivité du réseau de conteneur, l'affectation d'adresse IP et le contrôle de trafic réseau. </li><li>**Autres composants** : L'espace de nom `kube-system` inclut également des composants pour gérer des ressources fournies par IBM, par exemple, des plug-in de stockage pour le stockage de fichiers et le stockage par blocs, un équilibreur de charge d'application (ALB) Ingress, la consignation `fluentd` et `keepalived`.</li></ul>|
| `ibm-system` | Cet espace de nom inclut le déploiement `ibm-cloud-provider-ip` qui fonctionne avec `keepalived` pour fournir un diagnostic d'intégrité et un équilibrage de charge de couche 4 pour les demandes vers les pods d'application. |
| `kube-proxy-and-dns`| Cet espace de nom inclut les composants permettant de valider le trafic réseau entrant par rapport à des règles `iptables` qui sont configurées sur le noeud worker, et les demandes proxy qui sont autorisées à entrer dans le cluster ou à en sortir. |
| `default` | Cet espace de nom est utilisé si vous ne spécifiez pas d'espace de nom ou si vous créez un projet pour vos ressources Kubernetes. En outre, l'espace de nom par défaut comprend les composants suivants pour prendre en charge vos clusters OpenShift.<ul><li>**`router`** : OpenShift utilise des [routes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://docs.openshift.com/container-platform/3.11/architecture/networking/routes.html) pour exposer le service d'une application sur un nom d'hôte de sorte que les clients externes puissent atteindre le service. Le routeur mappe le service au nom d'hôte. </li><li>**`docker-registry`** et **`registry-console`** : OpenShift fournit un  [registre d'images de conteneur interne ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://docs.openshift.com/container-platform/3.11/install_config/registry/index.html) que vous pouvez utiliser pour gérer et visualiser des images localement via la console. Sinon, vous pouvez aussi configurer l'{{site.data.keyword.registrylong_notm}} privé.</li></ul>|
|Autres projets| Les autres composants sont installés dans différents espaces de nom par défaut pour activer les fonctions, telles que la consignation, la surveillance et la console OpenShift. <ul><li><code>ibm-cert-store</code></li><li><code>kube-public</code></li><li><code>kube-service-catalog</code></li><li><code>openshift</code></li><li><code>openshift-ansible-service-broker</code></li><li><code>openshift-console</code></li><li><code>openshift-infra</code></li><li><code>openshift-monitoring</code></li><li><code>openshift-node</code></li><li><code>openshift-template-service-broker</code></li><li><code>openshift-web-console</code></li></ul>|
{: caption="Tableau 2. Composants du noeud worker OpenShift" caption-side="top"}
{: #rhos-components-2}
{: tab-title="Worker nodes"}
{: tab-group="rhos-components"}
{: class="simple-tab-table"}

<br />


## Leçon 1 : Création d'un cluster Red Hat OpenShift on IBM Cloud
{: #openshift_create_cluster}

Vous pouvez créer un cluster Red Hat OpenShift on IBM Cloud dans {{site.data.keyword.containerlong_notm}} à l'aide de la [console](#openshift_create_cluster_console) ou de l'[interface CLI](#openshift_create_cluster_cli). Pour en savoir plus sur les composants qui sont configurés lorsque vous créez un cluster, voir [Présentation de l'architecture](#openshift_architecture). OpenShift est disponible pour les clusters standard uniquement. Pour en savoir plus sur le prix des clusters standard, reportez-vous à la [foire aux questions](/docs/containers?topic=containers-faqs#charges).{:shortdesc}

Vous pouvez créer des clusters uniquement dans le groupe de ressources **par défaut**. Les clusters OpenShift que vous créez dans le cadre de la version bêta ne sont conservés que pendant 30 jours après l'expiration de la version bêta et après la commercialisation de Red Hat OpenShift on IBM Cloud.
{: important}

### Création d'un cluster à l'aide de la console
{: #openshift_create_cluster_console}

Créez un cluster OpenShift standard dans la console {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

Avant de commencer, [effectuez les tâches prérequises](#openshift_prereqs) de manière à posséder les droits appropriés pour créer un cluster. 

1.  Créez un cluster.
    1.  Connectez-vous à votre compte [{{site.data.keyword.Bluemix_notm}} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/).
    2.  Dans le menu général ![Icône de menu général](../icons/icon_hamburger.svg "Icône de menu général"), sélectionnez **Kubernetes** et cliquez sur **Créer un cluster**.
    3.  Choisissez les détails de configuration et le nom de votre cluster. Pour la version bêta, les clusters OpenShift sont disponibles uniquement en tant que clusters standard dans les centres de données situés à Washington DC et à Londres. 
        *   Pour **Sélectionner un plan**, choisissez **Standard**.
        *   Pour **Groupe de ressources**, vous devez utiliser la valeur définie **par défaut**.
        *   Pour **Emplacement**, définissez **Amérique du Nord** ou **Europe** pour la zone géographique, sélectionnez la disponibilité **Zone unique** ou **Zones multiples**, puis sélectionnez les zones de noeud worker **Washington, DC** ou **Londres**. 
        *   Pour **Pool de noeuds worker par défaut**, sélectionnez la version de cluster **OpenShift**. Red Hat OpenShift on IBM Cloud prend en charge OpenShift version 3.11 uniquement, qui inclut Kubernetes version 1.11. Choisissez une version disponible pour vos noeuds worker, dotée idéalement d'au moins quatre coeurs de 16 Go de mémoire RAM. 
        *   Définissez un nombre de noeuds worker à créer par zone, par exemple, 3. 
    4.  Pour finir, cliquez sur **Créer un cluster**.<p class="note">Votre opération de création de cluster peut durer un certain temps. Lorsque le cluster passe à l'état **Normal**, il faut environ encore 10 minutes au réseau de cluster et aux composants d'équilibrage de charge pour déployer et mettre à jour le domaine de cluster que vous utilisez pour la console Web OpenShift et d'autres routes. Attendez que le cluster soit prêt avant de passer à l'étape suivante. Pour cela, vérifiez que le **sous-domaine Ingress** respecte le modèle `<cluster_name>.<region>.containers.appdomain.cloud`.</p>
2.  Sur la page des détails de cluster, cliquez sur **Console Web OpenShift**.
3.  Dans le menu déroulant dans la barre de menus de la plateforme de conteneur OpenShift, cliquez sur **Console d'application**. La console d'application répertorie tous les espaces de nom de projet dans votre cluster. Vous pouvez accéder à un espace de nom pour visualiser vos applications, vos générations et d'autres ressources Kubernetes. 
4.  Pour terminer la leçon suivante en travaillant dans le terminal, cliquez sur votre profil **IAM#user.name@email.com > Copy Login Command**. Collez la commande de connexion `oc` copiée dans votre terminal pour vous authentifier via l'interface CLI. 

### Création d'un cluster à l'aide de l'interface CLI
{: #openshift_create_cluster_cli}

Créez un cluster OpenShift standard à l'aide de l'interface CLI {{site.data.keyword.Bluemix_notm}}.
{: shortdesc}

Avant de commencer, [effectuez les tâches prérequises](#openshift_prereqs) de manière à posséder les droits appropriés pour créer un cluster, l'interface CLI et les plug-in `ibmcloud` et les interfaces CLI `oc` et `kubectl`. 

1.  Connectez-vous au compte que vous avez configuré pour créer des clusters OpenShift. Ciblez la région **us-east** ou **eu-gb** et le groupe de ressources **par défaut**. Si vous disposez d'un account fédéré, ajoutez l'indicateur `--sso`.
    ```
    ibmcloud login -r (us-east|eu-gb) -g default [--sso]
    ```
    {: pre}
2.  Créez un cluster.
    ```
    ibmcloud ks cluster-create --name <name> --location <zone> --kube-version <openshift_version> --machine-type <worker_node_flavor> --workers <number_of_worker_nodes_per_zone> --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID>
    ```
    {: pre}

    Exemple de commande de création d'un cluster avec trois noeuds worker dotés de quatre coeurs et de 16 Go de mémoire, situé à Washington, DC.

    ```
    ibmcloud ks cluster-create --name my_openshift --location wdc04 --kube-version 3.11_openshift --machine-type b3c.4x16.encrypted  --workers 3 --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID>
    ```
    {: pre}

    <table summary="La première ligne du tableau est répartie sur deux colonnes. La lecture des autres lignes s'effectue de gauche à droite, avec les composantes de la commande dans la première colonne et la description correspondante dans la deuxième colonne.">
 <caption>Composantes de la commande cluster-create</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composantes de cette commande</th>
    </thead>
    <tbody>
    <tr>
    <td><code>cluster-create</code></td>
    <td>Commande de création d'un cluster d'infrastructure classique dans votre compte {{site.data.keyword.Bluemix_notm}}.</td>
    </tr>
    <tr>
    <td><code>--name <em>&lt;name&gt;</em></code></td>
    <td>Entrez un nom pour votre cluster. Le nom doit commencer par une lettre, peut contenir des lettres, des nombres et des tirets (-) et ne doit pas dépasser 35 caractères. Utilisez un nom unique dans les régions {{site.data.keyword.Bluemix_notm}}. </td>
    </tr>
    <tr>
    <td><code>--location <em>&lt;zone&gt;</em></code></td>
    <td>Indiquez la zone dans laquelle vous souhaitez créer votre cluster. Pour la version bêta, les zones disponibles sont `wdc04, wdc06, wdc07, lon04, lon05,` ou `lon06`.</p></td>
    </tr>
    <tr>
      <td><code>--kube-version <em>&lt;openshift_version&gt;</em></code></td>
      <td>Vous devez choisir une version OpenShift prise en charge. Les versions OpenShift incluent une version Kubernetes qui diffère des versions Kubernetes disponibles sur les clusters Kubernetes Ubuntu natifs. Pour voir les versions OpenShift disponibles, exécutez la commande `ibmcloud ks versions`.
Pour créer un cluster avec la version de correctif la plus récente, vous pouvez spécifier uniquement la version majeure et la version mineure, par exemple, ` 3.11_openshift`.<br><br>Red Hat OpenShift on IBM Cloud prend en charge OpenShift version 3.11 uniquement, qui inclut Kubernetes version 1.11. </td>
    </tr>
    <tr>
    <td><code>--machine-type <em>&lt;worker_node_flavor&gt;</em></code></td>
    <td>Choisissez un type de machine. Vous pouvez déployer vos noeuds worker en tant que machines virtuelles sur du matériel partagé ou dédié ou en tant que machines physiques sur un serveur bare metal. Les types de machines virtuelles et physiques disponibles varient en fonction de la zone de déploiement du cluster. Pour afficher la liste des types de machine disponibles, exécutez la commande `ibmcloud ks machine-types --zone <zone>`.</td>
    </tr>
    <tr>
    <td><code>--workers <em>&lt;number_of_worker_nodes_per_zone&gt;</em></code></td>
    <td>Nombre de noeuds worker à inclure dans le cluster. Vous souhaiterez peut-être spécifier au moins trois noeuds worker afin que votre cluster dispose de suffisamment de ressources pour exécuter les composants par défaut et pour assurer la haute disponibilité. Si l'option <code>--workers</code> n'est pas spécifiée, un noeud worker est créé.</td>
    </tr>
    <tr>
    <td><code>--public-vlan <em>&lt;public_vlan_id&gt;</em></code></td>
    <td>Si vous disposez déjà d'un VLAN public configuré dans votre compte d'infrastructure IBM Cloud (SoftLayer) pour cette zone, entrez l'ID du VLAN public. Pour voir les VLAN disponibles, exécutez la commande `ibmcloud ks vlans --zone <zone>`.<br><br>Si vous ne disposez pas d'un VLAN public dans votre compte, ne spécifiez pas cette option. {{site.data.keyword.containerlong_notm}} crée automatiquement un VLAN public pour vous. </td>
    </tr>
    <tr>
    <td><code>--private-vlan <em>&lt;private_vlan_id&gt;</em></code></td>
    <td>Si vous disposez déjà d'un VLAN privé configuré dans votre compte d'infrastructure IBM Cloud (SoftLayer) pour cette zone, entrez l'ID du VLAN privé. Pour voir les VLAN disponibles, exécutez la commande `ibmcloud ks vlans --zone <zone>`.<br><br>Si vous ne disposez pas d'un VLAN privé dans votre compte, ne spécifiez pas cette option. {{site.data.keyword.containerlong_notm}} crée automatiquement un VLAN privé pour vous.</td>
    </tr>
    </tbody></table>
3.  Répertoriez les détails de votre cluster. Examinez l'**état** du cluster, vérifiez le **sous-domaine Ingress** et notez l'**URL maître**.<p class="note">Votre opération de création de cluster peut durer un certain temps. Lorsque le cluster passe à l'état **Normal**, il faut environ encore 10 minutes au réseau de cluster et aux composants d'équilibrage de charge pour déployer et mettre à jour le domaine de cluster que vous utilisez pour la console Web OpenShift et d'autres routes. Attendez que le cluster soit prêt avant de passer à l'étape suivante. Pour cela, vérifiez que le **sous-domaine Ingress** respecte le modèle `<cluster_name>.<region>.containers.appdomain.cloud`.</p>
    ```
    ibmcloud ks cluster-get --cluster <cluster_name_or_ID>
    ```
    {: pre}
4.  Téléchargez les fichiers de configuration pour vous connecter à votre cluster.
    ```
    ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
    ```
    {: pre}

    Une fois les fichiers de configuration téléchargés, une commande s'affiche. Vous pouvez copier et coller cette commande pour définir le chemin vers le fichier de configuration Kubernetes local en tant que variable d'environnement.

    Exemple pour OS X :

    ```
    export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-<datacenter>-<cluster_name>.yml
    ```
    {: screen}
5.  Dans votre navigateur, accédez à l'adresse de votre **URL maître** et ajoutez `/console`. Par exemple, `https://c0.containers.cloud.ibm.com:23652/console`.
6.  Cliquez sur votre profil **IAM#user.name@email.com > Copy Login Command**. Collez la commande de connexion `oc` copiée dans votre terminal pour vous authentifier via l'interface CLI. <p class="tip">Sauvegardez votre URL maître de cluster pour accéder ultérieurement à la console OpenShift. Dans les sessions suivantes, vous pourrez ignorer l'étape `cluster-config` et copier la commande de connexion à partir de la console. </p>
7.  Vérifiez que les commandes `oc` s'exécutent correctement avec votre cluster en vérifiant la version. 

    ```
    oc version
    ```
    {: pre}

    Exemple de sortie :

    ```
    oc v3.11.0+0cbc58b
    kubernetes v1.11.0+d4cacc0
    features: Basic-Auth

    Server https://c0.containers.cloud.ibm.com:23652
    openshift v3.11.98
    kubernetes v1.11.0+d4cacc0
    ```
    {: screen}

    Si vous ne pouvez pas effectuer d'opérations nécessitant les droits Administrateur, par exemple, l'affichage de tous les noeuds worker ou pods dans un cluster, téléchargez les certificats TLS et les fichiers de droits pour l'administrateur de cluster en exécutant la commande `ibmcloud ks cluster-config --cluster <cluster_name_or_ID> --admin`.
    {: tip}

<br />


## Leçon 2 : Accès aux services OpenShift intégrés
{: #openshift_access_oc_services}

Red Hat OpenShift on IBM Cloud est fourni avec des services intégrés que vous pouvez utiliser pour faire fonctionner votre cluster, par exemple, la console OpenShift, Prometheus et Grafana. Pour la version bêta, afin d'accéder à ces services, vous pouvez utiliser l'hôte local d'une [route ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://docs.openshift.com/container-platform/3.11/architecture/networking/routes.html). Les noms de domaine de route par défaut suivent un modèle propre au cluster : `<service_name>-<namespace>.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud`.
{:shortdesc}

Vous pouvez accéder aux routes de service OpenShift intégrées à partir de la [console](#openshift_services_console) ou de l'[interface CLI](#openshift_services_cli). Vous souhaiterez peut-être utiliser la console pour parcourir les ressources Kubernetes dans un projet. A l'aide de l'interface CLI, vous pouvez répertorier des ressources, telles que les routes des projets. 

### Accès aux services OpenShift intégrés à partir de la console
{: #openshift_services_console}
1.  A partir de la console Web OpenShift, dans le menu déroulant dans la barre de menus de la plateforme de conteneur OpenShift, cliquez sur **Console d'application**. 
2.  Sélectionnez le projet **par défaut**, puis dans le panneau de navigation, cliquez sur **Applications > Pods**.
3.  Vérifiez que les pods de **routeur** sont à l'état **En cours d'exécution**. Le routeur fonctionne comme point d'entrée pour le trafic réseau externe. Vous pouvez utiliser le routeur pour exposer publiquement les services dans votre cluster sur l'adresse IP externe du routeur à l'aide d'une route. Le routeur écoute sur l'interface réseau d'hôte publique, contrairement aux pods d'application qui écoutent uniquement sur des adresses IP privées. Le routeur met en cache les demandes externes de noms d'hôte de route vers les adresses IP des pods d'application qui sont identifiés par le service que vous avez associé au nom d'hôte de route. 
4.  Dans le panneau de navigateur de projet **par défaut**, cliquez sur **Applications > Déploiements**, puis cliquez sur le déploiement **registry-console**. Pour ouvrir la console de registre interne, vous devez mettre à jour l'URL de fournisseur de manière à pouvoir y accéder en interne. 
    1.  Dans l'onglet **Environnement** de la page des détails **registry-console**, recherchez la zone **OPENSHIFT_OAUTH_PROVIDER_URL**.  
    2. Dans la zone de valeur, ajoutez `-e` après `c1`, comme dans `https://c1-e.eu-gb.containers.cloud.ibm.com:20399`. 
    3. Cliquez sur **Sauvegarder**. A présent, le déploiement de registre de console est accessible via le noeud final d'API public du maître cluster. 
    4.  Dans le panneau de navigation de projet **par défaut**, cliquez sur **Applications > Routes**. Pour ouvrir la console de registre, cliquez sur la valeur de **nom d'hôte**, par exemple, `https://registry-console-default.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud`.<p class="note">Pour la version bêta, la console de registre utilise des certificats TLS autosignés, par conséquent, vous devez choisir de poursuivre pour accéder à la console de registre. Dans Google Chrome, cliquez sur **Avancé > Aller à <URL_maître_cluster>**. D'autres navigateurs ont des options similaires. Si vous ne pouvez pas poursuivre avec cette option, essayez d'ouvrir l'URL dans un navigateur privé. </p>
5.  Dans la barre de menus de la plateforme de conteneur OpenShift, cliquez sur **Console de cluster**. 
6.  Dans le panneau de navigation, développez **Surveillance**. 
7.  Cliquez sur l'outil de surveillance intégré auquel vous souhaitez accéder, par exemple, **Tableaux de bord**. La route Grafana s'ouvre, `https://grafana-openshift-monitoring.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud`.<p class="note">Lors de votre premier accès au nom d'hôte, vous devrez peut-être vous authentifier, par exemple en cliquant sur **Connexion avec OpenShift** et en autorisant l'accès à votre identité IAM. </p>

### Accès aux services OpenShift intégrés à partir de l'interface CLI
{: #openshift_services_cli}

1.  A partir de la console Web OpenShift, cliquez sur votre profil **IAM#user.name@email.com > Copy Login Command** et collez la commande de connexion dans votre terminal pour vous authentifier.
    ```
    oc login https://c1-e.<region>.containers.cloud.ibm.com:<port> --token=<access_token>
    ```
    {: pre}
2.  Vérifiez que votre routeur est déployé. Le routeur fonctionne comme point d'entrée pour le trafic réseau externe. Vous pouvez utiliser le routeur pour exposer publiquement les services dans votre cluster sur l'adresse IP externe du routeur à l'aide d'une route. Le routeur écoute sur l'interface réseau d'hôte publique, contrairement aux pods d'application qui écoutent uniquement sur des adresses IP privées. Le routeur met en cache les demandes externes de noms d'hôte de route vers les adresses IP des pods d'application qui sont identifiés par le service que vous avez associé au nom d'hôte de route.
    ```
    oc get svc router -n default
    ```
    {: pre}

    Exemple de sortie :
    ```
    NAME      TYPE           CLUSTER-IP               EXTERNAL-IP     PORT(S)                      AGE
    router    LoadBalancer   172.21.xxx.xxx   169.xx.xxx.xxx   80:30399/TCP,443:32651/TCP                      5h
    ```
    {: screen}
2.  Obtenez le nom d'hôte **Hôte/Port** de la route de service à laquelle vous souhaitez accéder. Par exemple, vous souhaiterez peut-être accéder à votre tableau de bord Grafana pour vérifier les métriques relatives à l'utilisation des ressources de votre cluster. Les noms de domaine de route par défaut suivent un modèle propre au cluster : `<service_name>-<namespace>.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud`.
```
    oc get route --all-namespaces
    ```
    {: pre}

    Exemple de sortie :
    ```
    NAMESPACE                          NAME                HOST/PORT                                                                    PATH                  SERVICES            PORT               TERMINATION          WILDCARD
    default                            registry-console    registry-console-default.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud                              registry-console    registry-console   passthrough          None
    kube-service-catalog               apiserver           apiserver-kube-service-catalog.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud                        apiserver           secure             passthrough          None
    openshift-ansible-service-broker   asb-1338            asb-1338-openshift-ansible-service-broker.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud            asb                 1338               reencrypt            None
    openshift-console                  console             console.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud                                              console             https              reencrypt/Redirect   None
    openshift-monitoring               alertmanager-main   alertmanager-main-openshift-monitoring.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud                alertmanager-main   web                reencrypt            None
    openshift-monitoring               grafana             grafana-openshift-monitoring.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud                          grafana             https              reencrypt            None
    openshift-monitoring               prometheus-k8s      prometheus-k8s-openshift-monitoring.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud                   prometheus-k8s      web                reencrypt
    ```
    {: screen}
3.  **Mise à jour ponctuelle de registre** : pour rendre votre console de registre interne accessible depuis Internet, éditez le déploiement `registry-console` pour utiliser le noeud final d'API public de votre maître cluster comme URL de fournisseur OpenShift. Le noeud final d'API public a le même format que le noeud final d'API privé, mais il inclut un `-e` supplémentaire dans l'URL.
    ```
    oc edit deploy registry-console -n default
    ```
    {: pre}
    
    Dans la zone `Pod Template.Containers.registry-console.Environment.OPENSHIFT_OAUTH_PROVIDER_URL`, ajoutez `-e` après `c1`, comme dans `https://ce.eu-gb.containers.cloud.ibm.com:20399`.
    ```
    Name:                   registry-console
    Namespace:              default
    ...
    Pod Template:
      Labels:  name=registry-console
      Containers:
       registry-console:
        Image:      registry.eu-gb.bluemix.net/armada-master/iksorigin-registrconsole:v3.11.98-6
        ...
        Environment:
          OPENSHIFT_OAUTH_PROVIDER_URL:  https://c1-e.eu-gb.containers.cloud.ibm.com:20399
          ...
    ```
    {: screen}
4.  Dans votre navigateur Web, ouvrez la route à laquelle vous souhaitez accéder, par exemple, `https://grafana-openshift-monitoring.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud`. Lors de votre premier accès au nom d'hôte, vous devrez peut-être vous authentifier, par exemple en cliquant sur **Connexion avec OpenShift** et en autorisant l'accès à votre identité IAM. 

<br>
Vous êtes maintenant dans l'application OpenShift intégrée ! Par exemple, si vous êtes dans Grafana, vous souhaiterez peut-être vérifier votre utilisation d'UC d'espace de nom ou d'autres graphiques. Pour accéder à d'autres outils intégrés, ouvrez leurs noms d'hôte de route. 

<br />


## Leçon 3 : Déploiement d'une application sur votre cluster OpenShift
{: #openshift_deploy_app}

Red Hat OpenShift on IBM Cloud vous permet de créer une nouvelle application et d'exposer votre service d'application via un routeur OpenShift pour permettre à des utilisateurs externes de l'utiliser.
{: shortdesc}

Si vous avez fait une pause après la dernière leçon et démarré une nouvelle session de terminal, prenez soin de vous reconnecter à votre cluster. Ouvrez votre console OpenShift : `https://<master_URL>/console`. Par exemple, `https://c0.containers.cloud.ibm.com:23652/console`. Cliquez ensuite sur votre profil **IAM#user.name@email.com > Copy Login Command** et collez la commande de connexion `oc` copiée dans votre terminal pour vous authentifier via l'interface CLI.
{: tip}

1.  Créez un projet pour votre application Hello World. Un projet est une version OpenShift d'un espace de nom Kubernetes avec des annotations supplémentaires.
    ```
    oc new-project hello-world
    ```
    {: pre}
2.  Créez l'exemple d'application [à partir du code source ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/IBM/container-service-getting-started-wt). La commande OpenShift `new-app` vous permet de faire référence à un répertoire dans un référentiel distant qui contient le fichier Dockerfile et le code d'application pour créer votre image. La commande crée l'image, stocke l'image dans le registre Docker local et crée les configurations de déploiement d'application (`dc`) et les services (`svc`). Pour plus d'informations sur la création de nouvelles applications, [voir la documentation OpenShift ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://docs.openshift.com/container-platform/3.11/dev_guide/application_lifecycle/new_app.html).
    ```
    oc new-app --name hello-world https://github.com/IBM/container-service-getting-started-wt --context-dir="Lab 1"
    ```
    {: pre}
3.  Vérifiez que les composants de l'exemple d'application Hello World sont créés. 
    1.  Recherchez l'image **hello-world** dans le registre Docker intégré du cluster en accédant à la console de registre dans votre navigateur. Vérifiez que vous avez mis à jour l'URL du fournisseur de console de registre avec `-e` comme indiqué dans la leçon précédente.
        ```
        https://registry-console-default.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud/
        ```
        {: codeblock}
    2.  Répertoriez les services **hello-world** et notez le nom de service. Votre application écoute le trafic sur ces adresses IP de cluster internes sauf si vous créez une route pour le service de sorte que le routeur puisse acheminer des demandes de trafic externes vers l'application.
        ```
        oc get svc -n hello-world
        ```
        {: pre}

        Exemple de sortie :
        ```
        NAME          TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)    AGE
        hello-world   ClusterIP   172.21.xxx.xxx   <nones>       8080/TCP   31m
        ```
        {: screen}
    3.  Répertoriez les pods. Les pods dont le nom comporte `build` sont des travaux **terminés** dans le cadre du nouveau processus de création d'application. Assurez-vous que le pod **hello-world** est à l'état **En cours d'exécution**.
        ```
        oc get pods -n hello-world
        ```
        {: pre}

        Exemple de sortie :
        ```
        NAME                READY     STATUS             RESTARTS   AGE
        hello-world-9cv7d   1/1       Running            0          30m
        hello-world-build   0/1       Completed          0          31m
        ```
        {: screen}
4.  Configurez un chemin afin de pouvoir accéder publiquement au service {{site.data.keyword.toneanalyzershort}}. Par défaut, le nom d'hôte est au format suivant : `<service_name>-<namespace>.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud`. Si vous souhaitez personnaliser le nom d'hôte, ajoutez l'indicateur `--hostname=<hostname>`. 
    1.  Créez une route pour le service **hello-world**.
        ```
        oc create route edge --service=hello-world -n hello-world
        ```
        {: pre}
    2.  Obtenez l'adresse de nom d'hôte de route dans la sortie **Hôte/Port**.
        ```
        oc get route -n hello-world
        ```
        {: pre}
        Exemple de sortie :
        ```
        NAME          HOST/PORT                         PATH                                        SERVICES      PORT       TERMINATION   WILDCARD
        hello-world   hello-world-hello.world.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud    hello-world   8080-tcp   edge/Allow    None
        ```
        {: screen}
5.  Accédez à votre application.
    ```
    curl https://hello-world-hello-world.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud
    ```
    {: pre}
    
    Exemple de sortie :
    ```
    Hello world from hello-world-9cv7d! Your app is up and running in a cluster!
    ```
    {: screen}
6.  **Facultatif** Pour nettoyer les ressources que vous avez créées dans cette leçon, vous pouvez utiliser les libellés affectés à chaque application. 
    1.  Répertoriez toutes les ressources pour chaque application du projet `hello-world`.
        ```
        oc get all -l app=hello-world -o name -n hello-world
        ```
        {: pre}
        Exemple de sortie :
        ```
        pod/hello-world-1-dh2ff
        replicationcontroller/hello-world-1
        service/hello-world
        deploymentconfig.apps.openshift.io/hello-world
        buildconfig.build.openshift.io/hello-world
        build.build.openshift.io/hello-world-1
        imagestream.image.openshift.io/hello-world
        imagestream.image.openshift.io/node
        route.route.openshift.io/hello-world
        ```
        {: screen}
    2.  Supprimez toutes les ressources que vous avez créées.
        ```
        oc delete all -l app=hello-world -n hello-world
        ```
        {: pre}



<br />


## Leçon 4 : Configuration des modules complémentaires LogDNA et Sysdig pour surveiller l'état de santé du cluster
{: #openshift_logdna_sysdig}

Etant donné que OpenShift configure par défaut des [contraintes de contexte de sécurité (SCC) ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://docs.openshift.com/container-platform/3.11/admin_guide/manage_scc.html) plus strictes que le système Kubernetes natif, vous constaterez peut-être que certaines des applications ou des modules complémentaires de cluster que vous utilisez sur le système Kubernetes natif ne peuvent pas être déployés sur OpenShift de la même manière. En particulier, un grand nombre d'images doit être exécuté en tant qu'utilisateur `root` ou en tant que conteneur privilégié, ce qui est interdit dans OpenShift par défaut. Dans cette leçon, vous apprenez à modifier les SCC par défaut en créant des comptes de sécurité privilégiés et en mettant à jour la valeur de `securityContext` dans la spécification de pod pour utiliser deux modules complémentaires {{site.data.keyword.containerlong_notm}} populaires : {{site.data.keyword.la_full_notm}} et {{site.data.keyword.mon_full_notm}}.
{: shortdesc}

Avant de commencer, connectez-vous à votre cluster en tant qu'administrateur. 
1.  Ouvrez votre console OpenShift : `https://<master_URL>/console`. Par exemple, `https://c0.containers.cloud.ibm.com:23652/console`.
2.  Cliquez sur votre profil **IAM#user.name@email.com > Copy Login Command** et collez la commande de connexion `oc` copiée dans votre terminal afin de vous authentifier via l'interface CLI. 
3.  Téléchargez les fichiers de configuration d'administrateur pour votre cluster.
    ```
    ibmcloud ks cluster-config --cluster <cluster_name_or_ID> --admin
    ```
    {: pre}
    
    Une fois les fichiers de configuration téléchargés, une commande s'affiche. Vous pouvez copier et coller cette commande pour définir le chemin vers le fichier de configuration Kubernetes local en tant que variable d'environnement.

    Exemple pour OS X :

    ```
    export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-<datacenter>-<cluster_name>.yml
    ```
    {: screen}
4.  Poursuivez la leçon pour configurer [{{site.data.keyword.la_short}}](#openshift_logdna) et [{{site.data.keyword.mon_short}}](#openshift_sysdig).

### Leçon 4a : Configuration de LogDNA
{: #openshift_logdna}

Configurez un projet et un compte de service privilégié pour {{site.data.keyword.la_full_notm}}. Ensuite, créez une instance {{site.data.keyword.la_short}} dans votre compte {{site.data.keyword.Bluemix_notm}}. Pour intégrer votre instance {{site.data.keyword.la_short}} à votre cluster OpenShift, vous devez modifier l'ensemble de démons qui est déployé afin d'utiliser le compte de service privilégié pour une exécution en tant que root.
{: shortdesc}

1.  Configurez le projet et le compte de service privilégié pour LogDNA. 
    1.  En tant qu'administrateur de cluster, créez un projet `logdna`.
        ```
        oc adm new-project logdna
        ```
        {: pre}
    2.  Ciblez le projet de sorte que les ressources que vous créez par la suite se trouvent dans l'espace de nom de projet `logdna`.
        ```
        oc project logdna
        ```
        {: pre}
    3.  Créez un compte de service pour le projet `logdna`.
        ```
        oc create serviceaccount logdna
        ```
        {: pre}
    4.  Ajoutez une contrainte de contexte de sécurité privilégiée au compte de service pour le projet `logdna`. <p class="note">Pour déterminer quelles sont les droits que la politique SCC `privileged` accorde au compte de service, exécutez la commande `oc describe scc privileged`. Pour plus d'informations sur les SCC, voir la [documentation OpenShift![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://docs.openshift.com/container-platform/3.11/admin_guide/manage_scc.html).</p>
        ```
        oc adm policy add-scc-to-user privileged -n logdna -z logdna
        ```
        {: pre}
2.  Créez votre instance {{site.data.keyword.la_full_notm}} dans le même groupe de ressources que votre cluster. Sélectionnez un plan de tarification qui détermine la durée de conservation de vos journaux, par exemple, `lite` qui permet de conserver les journaux pendant 0 jour. Il n'est pas nécessaire que la région corresponde à la région de votre cluster. Pour plus d'informations, voir [Mise à disposition d'une instance](/docs/services/Log-Analysis-with-LogDNA/tutorials?topic=LogDNA-provision) et [Plans de tarification](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-about#overview_pricing_plans).
    ```
    ibmcloud resource service-instance-create <service_instance_name> logdna (lite|7-days|14-days|30-days) <region> [-g <resource_group>]
    ```
    {: pre}
    
    Exemple de commande :
    ```
    ibmcloud resource service-instance-create logdna-openshift logdna lite us-south
    ```
    {: pre}
    
    Dans la sortie, notez l'**ID** de l'instance de service, qui est au format `crn:v1:bluemix:public:logdna:<region>:<ID_string>::`.
    ```
    L'instance de service <name> a été créée. 
                 
    Name:         <name>   
    ID:           crn:v1:bluemix:public:logdna:<region>:<ID_string>::   
    GUID:         <guid>   
    Location:     <region>   
    ...
    ```
    {: screen}    
3.  Obtenez votre clé d'ingestion d'instance {{site.data.keyword.la_short}}. La clé d'ingestion LogDNA permet d'ouvrir une socket Web sécurisée sur le serveur d'ingestion LogDNA et d'authentifier l'agent de journalisation auprès du service {{site.data.keyword.la_short}}. 
    1.  Créez une clé de service pour votre instance LogDNA.
        ```
        ibmcloud resource service-key-create <key_name> Administrator --instance-id <logdna_instance_ID>
        ```
        {: pre}
    2.  Notez la clé d'ingestion (**ingestion_key**) de votre clé de service.
        ```
        ibmcloud resource service-key <key_name>
        ```
        {: pre}
        
        Exemple de sortie :
        ```
        Name:          <key_name>  
        ID:            crn:v1:bluemix:public:logdna:<region>:<ID_string>::    
        Created At:    Thu Jun  6 21:31:25 UTC 2019   
        State:         active   
        Credentials:                                   
                       apikey:                   <api_key_value>      
                       iam_apikey_description:   Auto-generated for key <ID>     
                       iam_apikey_name:          <key_name>       
                       iam_role_crn:             crn:v1:bluemix:public:iam::::role:Administrator      
                       iam_serviceid_crn:        crn:v1:bluemix:public:iam-identity::<ID_string>     
                       ingestion_key:            111a11aa1a1a11a1a11a1111aa1a1a11    
        ```
        {: screen}
4.  Créez une valeur confidentielle Kubernetes afin de stocker votre clé d'ingestion LogDNA pour votre instance de service.
    ```
     oc create secret generic logdna-agent-key --from-literal=logdna-agent-key=<logDNA_ingestion_key>
    ```
    {: pre}
5.  Créez un ensemble de démons Kubernetes pour déployer l'agent LogDNA sur chaque noeud worker de votre cluster Kubernetes. L'agent LogDNA collecte les journaux avec l'extension `*.log` et les fichiers sans extension stockés dans le répertoire `/var/log` de votre pod. Par défaut, les journaux sont collectés à partir de tous les espaces de nom, y compris `kube-system`, et automatiquement envoyés au service {{site.data.keyword.la_short}}.
    ```
    oc create -f https://assets.us-south.logging.cloud.ibm.com/clients/logdna-agent-ds.yaml
    ```
    {: pre}
6.  Editez la configuration d'ensemble de démons d'agent LogDNA pour faire référence au compte de service que vous avez précédemment créé et pour affecter la valeur Privilégié au contexte de sécurité.
    ```
    oc edit ds logdna-agent
    ```
    {: pre}
    
    Dans le fichier de configuration, ajoutez les spécifications suivantes :
    *   Dans `spec.template.spec`, ajoutez `serviceAccount: logdna`.
    *   Dans `spec.template.spec.containers`, ajoutez `securityContext: privileged: true`.
    *   Si vous avez créé votre instance {{site.data.keyword.la_short}} dans une région autre que `us-south`, mettez à jour les valeurs de variable d'environnement `spec.template.spec.containers.env` pour `LDAPIHOST` et `LDLOGHOST` avec `<region>`.

    Exemple de sortie :
    ```
    apiVersion: extensions/v1beta1
    kind: DaemonSet
    spec:
      ...
      template:
        ...
        spec:
          serviceAccount: logdna
          containers:
          - securityContext:
              privileged: true
            ...
            env :
            - name: LOGDNA_AGENT_KEY
              valueFrom:
                secretKeyRef:
                  key: logdna-agent-key
                  name: logdna-agent-key
            - name: LDAPIHOST
              value: api.<region>.logging.cloud.ibm.com
            - name: LDLOGHOST
              value: logs.<region>.logging.cloud.ibm.com
          ...
    ```
    {: screen}
7.  Vérifiez que le pod `logdna-agent` sur chaque noeud est à l'état **En cours d'exécution**.
    ```
    oc get pods
    ```
    {: pre}
8.  A partir de [{{site.data.keyword.Bluemix_notm}} Observability > Console de journalisation](https://cloud.ibm.com/observe/logging), sur la ligne correspondant à votre instance {{site.data.keyword.la_short}}, cliquez sur **Afficher LogDNA**. Le tableau de bord LogDNA s'ouvre et vous pouvez commencer à analyser vos journaux. 

Pour en savoir plus sur l'utilisation de {{site.data.keyword.la_short}}, voir [Etapes suivantes](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-kube#kube_next_steps).

### Leçon 4b : Configuration de Sysdig
{: #openshift_sysdig}

Créez une instance {{site.data.keyword.mon_full_notm}} dans votre compte {{site.data.keyword.Bluemix_notm}}. Pour intégrer votre instance {{site.data.keyword.mon_short}} à votre cluster OpenShift, vous devez exécuter un script qui crée un projet et un compte de service privilégié pour l'agent Sysdig.
{: shortdesc}

1.  Créez votre instance {{site.data.keyword.mon_full_notm}} dans le même groupe de ressources que votre cluster. Sélectionnez un plan de tarification qui détermine la durée de conservation de vos journaux, par exemple, `lite`. Il n'est pas nécessaire que la région corresponde à la région de votre cluster. Pour plus d'informations, voir [Mise à disposition d'une instance](/docs/services/Monitoring-with-Sysdig?topic=Sysdig-provision).
    ```
    ibmcloud resource service-instance-create <service_instance_name> sysdig-monitor (lite|graduated-tier) <region> [-g <resource_group>]
    ```
    {: pre}
    
    Exemple de commande :
    ```
    ibmcloud resource service-instance-create sysdig-openshift sysdig-monitor lite us-south
    ```
    {: pre}
    
    Dans la sortie, notez l'**ID** de l'instance de service, qui est au format `crn:v1:bluemix:public:logdna:<region>:<ID_string>::`.
    ```
    L'instance de service <name> a été créée. 
                 
    Name:         <name>   
    ID:           crn:v1:bluemix:public:sysdig-monitor:<region>:<ID_string>::   
    GUID:         <guid>   
    Location:     <region>   
    ...
    ```
    {: screen}    
2.  Obtenez votre clé d'accès d'instance {{site.data.keyword.mon_short}}. La clé d'accès Sysdig permet d'ouvrir une socket Web sécurisée sur le serveur d'ingestion Sysdig et d'authentifier l'agent de surveillance auprès du service {{site.data.keyword.mon_short}}. 
    1.  Créez une clé de service pour votre instance Sysdig.
        ```
        ibmcloud resource service-key-create <key_name> Administrator --instance-id <sysdig_instance_ID>
        ```
        {: pre}
    2.  Notez la clé d'accès Sysdig (**Sysdig Access Key**) et le noeud final de collecteur Sysdig (**Sysdig Collector Endpoint**) de votre clé de service.
        ```
        ibmcloud resource service-key <key_name>
        ```
        {: pre}
        
        Exemple de sortie :
        ```
        Name:          <key_name>  
        ID:            crn:v1:bluemix:public:sysdig-monitor:<region>:<ID_string>::    
        Created At:    Thu Jun  6 21:31:25 UTC 2019   
        State:         active   
        Credentials:                                   
                       Sysdig Access Key:           11a1aa11-aa1a-1a1a-a111-11a11aa1aa11      
                       Sysdig Collector Endpoint:   ingest.<region>.monitoring.cloud.ibm.com      
                       Sysdig Customer Id:          11111      
                       Sysdig Endpoint:             https://<region>.monitoring.cloud.ibm.com  
                       apikey:                   <api_key_value>      
                       iam_apikey_description:   Auto-generated for key <ID>     
                       iam_apikey_name:          <key_name>       
                       iam_role_crn:             crn:v1:bluemix:public:iam::::role:Administrator      
                       iam_serviceid_crn:        crn:v1:bluemix:public:iam-identity::<ID_string>       
        ```
        {: screen}
3.  Exécutez le script pour configurer un projet `ibm-observe` avec un compte de service privilégié et un ensemble de démons Kubernetes afin de déployer l'agent Sysdig sur chaque noeud worker de votre cluster Kubernetes. L'agent Sysdig collecte des métriques, telles que l'utilisation de l'unité centrale du noeud worker, l'utilisation de la mémoire du noeud worker, le trafic HTTP vers et depuis vos conteneurs, et les données relatives à plusieurs composants d'infrastructure.  

    Dans la commande suivante, remplacez <sysdig_access_key> and <sysdig_collector_endpoint> par les valeurs de la clé de service que vous avez créée précédemment. Pour <tag>, vous pouvez associer des balises à votre agent Sysdig, par exemple, `role:service,location:us-south`, pour vous aider à identifier l'environnement d'où proviennent les métriques. 

    ```
    curl -sL https://ibm.biz/install-sysdig-k8s-agent | bash -s -- -a <sysdig_access_key> -c <sysdig_collector_endpoint> -t <tag> -ac 'sysdig_capture_enabled: false' --openshift
    ```
    {: pre}
    
    Exemple de sortie : 
    ```
    * Detecting operating system
    * Downloading Sysdig cluster role yaml
    * Downloading Sysdig config map yaml
    * Downloading Sysdig daemonset v2 yaml
    * Creating project: ibm-observe
    * Creating sysdig-agent serviceaccount in project: ibm-observe
    * Creating sysdig-agent access policies
    * Creating sysdig-agent secret using the ACCESS_KEY provided
    * Retreiving the IKS Cluster ID and Cluster Name
    * Setting cluster name as <cluster_name>
    * Setting ibm.containers-kubernetes.cluster.id 1fbd0c2ab7dd4c9bb1f2c2f7b36f5c47
    * Updating agent configmap and applying to cluster
    * Setting tags
    * Setting collector endpoint
    * Adding additional configuration to dragent.yaml
    * Enabling Prometheus
    configmap/sysdig-agent created
    * Deploying the sysdig agent
    daemonset.extensions/sysdig-agent created
    ```
    {: screen}
        
4.  Vérifiez que les pods `sydig-agent` sur chaque noeud indiquent que **1/1** pods sont prêts et que chacun d'eux est à l'état **En cours d'exécution**.
    ```
    oc get pods
    ```
    {: pre}
    
    Exemple de sortie :
    ```
    NAME                 READY     STATUS    RESTARTS   AGE
    sysdig-agent-qrbcq   1/1       Running   0          1m
    sysdig-agent-rhrgz   1/1       Running   0          1m
    ```
    {: screen}
5.  A partir de [{{site.data.keyword.Bluemix_notm}} Observability > Console de surveillance](https://cloud.ibm.com/observe/logging), sur la ligne correspondant à votre instance {{site.data.keyword.mon_short}}, cliquez sur **Afficher Sysdig**. Le tableau de bord Sysdig s'ouvre et vous pouvez commencer à analyser vos métriques de cluster.

Pour en savoir plus sur l'utilisation de {{site.data.keyword.mon_short}}, voir [Etapes suivantes](/docs/services/Monitoring-with-Sysdig?topic=Sysdig-kubernetes_cluster#kubernetes_cluster_next_steps).

### Facultatif : Nettoyage
{: #openshift_logdna_sysdig_cleanup}

Retirez les instances {{site.data.keyword.la_short}} et {{site.data.keyword.mon_short}} de votre cluster et le compte {{site.data.keyword.Bluemix_notm}}. Notez que sauf si vous stockez les journaux et les métriques dans un [stockage permanent](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-archiving), vous ne pouvez pas accéder à ces informations après avoir supprimer les instances de votre compte.
{: shortdesc}

1.  Nettoyez les instances {{site.data.keyword.la_short}} et {{site.data.keyword.mon_short}} dans votre cluster en retirant les projets que vous avez créés pour elles. Lorsque vous supprimez un projet, ses ressources, telles que les comptes de service et les ensembles de démons sont également retirés.
    ```
    oc delete project logdna
    ```
    {: pre}
    ```
    oc delete project ibm-observe
    ```
    {: pre}
2.  Retirez les instances de votre compte {{site.data.keyword.Bluemix_notm}}. 
    *   [Retrait d'une instance {{site.data.keyword.la_short}}](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-remove).
    *   [Retrait d'une instance {{site.data.keyword.mon_short}}](/docs/services/Monitoring-with-Sysdig?topic=Sysdig-remove).

<br />


## Limitations
{: #openshift_limitations}

La version bêta de Red Hat OpenShift on IBM Cloud est commercialisée avec les limitations suivantes :
{: shortdesc}

**Cluster** :
*   Vous pouvez créer uniquement des clusters standard et non des clusters gratuits. 
*   Les emplacements sont disponibles dans deux régions métropolitaines à zones multiples Washington DC et Londres. Les zones prises en charge sont `wdc04, wdc06, wdc07, lon04, lon05,` et `lon06`.
*   Vous ne pouvez pas créer un cluster avec des noeuds worker qui exécutent plusieurs systèmes d'exploitation, tels que OpenShift sur Red Hat Enterprise Linux et le système Kubernetes natif sur Ubuntu.
*   Le [programme de mise à l'échelle automatique de cluster](/docs/containers?topic=containers-ca) n'est pas pris en charge car il requiert Kubernetes version 1.12 ou ultérieure. OpenShift 3.11 inclut uniquement Kubernetes version 1.11.



**Stockage** :
*   Le stockage de fichiers, le stockage par blocs et le stockage d'objets Cloud d'{{site.data.keyword.Bluemix_notm}} sont pris en charge. Le stockage SDS Portworx n'est pas pris en charge. 
*   En raison de la manière dont le stockage de fichiers NFS {{site.data.keyword.Bluemix_notm}} configure les droits utilisateur Linux, il se peut que vous rencontriez des erreurs lorsque vous utilisez le stockage de fichiers. Si tel est le cas, vous devrez peut-être configurer des [contraintes de contexte de sécurité OpenShift ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://docs.openshift.com/container-platform/3.11/admin_guide/manage_scc.html) ou utiliser un autre type de stockage. 

**Mise en réseau** :
*   Calico est utilisé comme fournisseur de politique de réseau à la place de OpenShift SDN.

**Modules complémentaires, intégrations et autres services**:
*   Les modules complémentaires {{site.data.keyword.containerlong_notm}} tels que Istio, Knative et le terminal Kubernetes ne sont pas disponibles. 
*   Les chartes Helm ne sont pas certifiées pour fonctionner dans des clusters OpenShift, à l'exception d'{{site.data.keyword.Bluemix_notm}} Object Storage.
*   Les clusters ne sont pas déployés avec des valeurs confidentielles d'extraction d'image pour les domaines {{site.data.keyword.registryshort_notm}} `icr.io`. Vous pouvez [créer vos propres valeurs confidentielles d'extraction d'image](/docs/containers?topic=containers-images#other_registry_accounts) ou bien utiliser le registre Docker intégré pour les clusters OpenShift. 

**Applications** :
*   Par défaut, OpenShift configure des paramètres de sécurité plus stricts que le système Kubernetes natif. Pour plus d'informations, voir la documentation OpenShift relative à la [gestion des contraintes de contexte de sécurité (SCC)![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://docs.openshift.com/container-platform/3.11/admin_guide/manage_scc.html).
*   Par exemple, les applications qui sont configurées pour s'exécuter en tant que root peuvent échouer, avec les pods à l'état `CrashLoopBackOff`. Pour résoudre ce problème, vous pouvez soit modifier les contraintes de contexte de sécurité par défaut, soit utiliser une image qui n'est pas exécutée en tant que root. 
*   OpenShift est configuré par défait avec un registre Docker local. Si vous souhaitez utiliser des images qui sont stockées dans vos noms de domaine {{site.data.keyword.registrylong_notm}} `icr.io` privés distants, vous devez créer vous-même les valeurs confidentielles pour chaque registre régional et global. Vous pouvez  [copier les valeurs confidentielles `default-<region>-icr-io`](/docs/containers?topic=containers-images#copy_imagePullSecret) depuis l'espace de nom `default` vers l'espace de nom à partir duquel vous souhaitez extraire des images, ou vous pouvez [créer votre propre valeur confidentielle](/docs/containers?topic=containers-images#other_registry_accounts). Ensuite, [ajoutez la valeur confidentielle d'extraction d'image](/docs/containers?topic=containers-images#use_imagePullSecret) à votre configuration de déploiement ou au compte de service d'espace de nom. 
*   La console OpenShift est utilisée à la place du tableau de bord Kubernetes. 

<br />


## Etape suivante ?
{: #openshift_next}

Pour plus d'informations sur l'utilisation de vos applications et services de routage, voir [OpenShift Developer Guide](https://docs.openshift.com/container-platform/3.11/dev_guide/index.html).

<br />


## Commentaires et questions
{: #openshift_support}

Dans la version bêta, les clusters Red Hat OpenShift on IBM Cloud ne sont pas couverts par le support IBM ni le support Red Hat. Le support fourni consiste simplement à vous aider à évaluer le produit dans le cadre de la préparation à sa disponibilité générale.
{: important}

Pour nous poser des questions ou nous faire part de vos commentaires, utilisez Slack. 
*   Si vous êtes un utilisateur externe, utilisez le canal [#openshift ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://ibm-container-service.slack.com/messages/CKCJLJCH4).  
*   Si vous êtes un IBMer, utilisez le canal [#iks-openshift-users ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://ibm-argonauts.slack.com/messages/CJH0UPN2D). 

Si vous n'utilisez pas un IBMid pour votre compte {{site.data.keyword.Bluemix_notm}}, [demandez une invitation](https://bxcs-slack-invite.mybluemix.net/) à Slack.
{: tip}
