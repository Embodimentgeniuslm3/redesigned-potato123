---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-07"

keywords: kubernetes, iks, clusters, worker nodes, worker pools, delete

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
{:gif: data-image-type='gif'}


# Création de clusters
{: #clusters}

Créez un cluster Kubernetes dans {{site.data.keyword.containerlong}}.
{: shortdesc}

Vous démarrez ? Suivez le [tutoriel de création d'un cluster Kubernetes](/docs/containers?topic=containers-cs_cluster_tutorial#cs_cluster_tutorial). Vous ne savez pas quelle configuration de cluster choisir ? Reportez-vous à [Planification d'une configuration de réseau pour votre cluster](/docs/containers?topic=containers-plan_clusters).
{: tip}

Avez-vous déjà créé un cluster et voulez juste consulter rapidement des exemples de commandes ? Essayez de suivre ces exemples.
*  **Cluster gratuit** :
   ```
   ibmcloud ks cluster-create --name my_cluster
   ```
   {: pre}
*  **Cluster standard, machine virtuelle partagée** :
   ```
   ibmcloud ks cluster-create --name my_cluster --zone dal10 --machine-type b3c.4x16 --hardware shared --workers 3 --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID>
   ```
   {: pre}
*  **Cluster standard, bare metal** :
   ```
   ibmcloud ks cluster-create --name my_cluster --zone dal10 --machine-type mb2c.4x32 --hardware dedicated --workers 3 --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID>
   ```
   {: pre}
*  **Cluster standard, machine virtuelle avec des noeuds finaux de service publié et privé dans des comptes avec VRF activé** :
   ```
   ibmcloud ks cluster-create --name my_cluster --zone dal10 --machine-type b3c.4x16 --hardware shared --workers 3 --public-service-endpoint --private-service-endpoint --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID>
   ```
   {: pre}
*  **Cluster standard qui utilise des VLAN privés et le noeud final de service privé uniquement** :
   ```
   ibmcloud ks cluster-create --name my_cluster --zone dal10 --machine-type b3c.4x16 --hardware shared --workers 3 --private-service-endpoint --private-vlan <private_VLAN_ID> --private-only
   ```
   {: pre}

<br />


## Préparation de la création de clusters au niveau du compte
{: #cluster_prepare}

Préparez votre compte {{site.data.keyword.Bluemix_notm}} pour {{site.data.keyword.containerlong_notm}}. Il s'agit de préparations, qui, une fois créées par l'administrateur du compte, n'ont plus besoin d'être modifiées à chaque fois que vous créez un cluster. Cependant, chaque fois que vous créez un cluster, vous souhaitez quand même vérifier que l'état actuel au niveau du compte correspond bien à celui qu'il vous faut.
{: shortdesc}

1. [Créez ou mettez à jour votre compte pour passer à un compte facturable ({{site.data.keyword.Bluemix_notm}} Paiement à la carte ou Abonnement)](https://cloud.ibm.com/registration/).

2. [Configurez une clé d'API {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-users#api_key) dans les régions où vous souhaitez créer des clusters. Affectez la clé d'API avec les droits appropriés pour créer des clusters :
  * Rôle **Superutilisateur** pour l'infrastructure IBM Cloud (SoftLayer).
  * Rôle de gestion de plateforme **Administrateur** pour {{site.data.keyword.containerlong_notm}} au niveau du compte.
  * Rôle de gestion de plateforme **Administrateur** pour {{site.data.keyword.registrylong_notm}} au niveau du compte. Si votre compte est antérieur au 4 octobre 2018, vous devez [activer des règles {{site.data.keyword.Bluemix_notm}} IAM pour {{site.data.keyword.registryshort_notm}}](/docs/services/Registry?topic=registry-user#existing_users). Avec les règles IAM, vous pouvez contrôler l'accès aux ressources, telles qu'aux espaces de nom d'un registre.

  Vous êtes le propriétaire du compte ? Dans ce cas vous disposez déjà des droits nécessaires. Lorsque vous créez un cluster, la clé d'API de cette région et de ce groupe de ressources est définie avec vos données d'identification.
  {: tip}

3. Vérifiez que vous disposez du rôle de plateforme **Administrateur** pour {{site.data.keyword.containerlong_notm}}. Pour permettre à votre cluster d'extraire des images du registre privé, vous avez également besoin du rôle de plateforme **Administrateur** pour {{site.data.keyword.registrylong_notm}}.
  1. Dans la barre de menu de la [console {{site.data.keyword.Bluemix_notm}} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/), cliquez sur **Gérer > Accès (IAM)**.
  2. Cliquez sur la page **Utilisateurs**, puis dans le tableau, sélectionnez-vous.
  3. Dans l'onglet **Règles d'accès**, confirmez que votre **Rôle** est **Administrateur**. Vous pouvez être **Administrateur** de toutes les ressources du compte ou au moins pour {{site.data.keyword.containershort_notm}}. **Remarque** : si vous disposez du rôle **Administrateur** pour {{site.data.keyword.containershort_notm}} dans un seul groupe de ressources ou une seule région plutôt que pour tout le compte, vous devez au moins disposer du rôle **Afficheur** au niveau du compte pour voir les réseaux locaux virtuels (VLAN) du compte.
  <p class="tip">Assurez-vous que votre administrateur de compte ne vous affecte pas le rôle de plateforme **Administrateur** en même temps qu'un rôle de service. Vous devez affecter les rôles de plateforme et de service séparément.</p>

4. Si votre compte utilise plusieurs groupes de ressources, envisagez la stratégie de votre compte à utiliser pour [gérer les groupes de ressources](/docs/containers?topic=containers-users#resource_groups).
  * Le cluster est créé dans le groupe de ressources que vous ciblez lorsque vous vous connectez à {{site.data.keyword.Bluemix_notm}}. Si vous ne ciblez pas de groupe de ressources, le groupe de ressources par défaut est automatiquement ciblé.
  * Si vous désirez créer un cluster dans un autre groupe de ressources que le groupe par défaut, vous devez au moins disposer du rôle **Afficheur** pour le groupe de ressources. Si aucun rôle ne vous est affecté pour le groupe de ressources et que vous êtes toujours **Administrateur** du service au sein du groupe de ressources, votre cluster est créé dans le groupe de ressources par défaut.
  * Vous ne pouvez pas modifier le groupe de ressources d'un cluster. En outre, si vous devez utiliser la [commande](/docs/containers-cli-plugin?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_service_bind) `ibmcloud ks cluster-service-bind` pour [intégrer un service {{site.data.keyword.Bluemix_notm}}](/docs/containers?topic=containers-service-binding#bind-services), ce dernier doit se trouver dans le même groupe de ressources que le cluster. Les services qui n'utilisent pas de groupes de ressources, par exemple, {{site.data.keyword.registrylong_notm}}, ou qui n'ont pas besoin de liaison de service, par exemple, {{site.data.keyword.la_full_notm}}, fonctionnent même si le cluster se trouve dans un autre groupe de ressources.
  * Si vous envisagez d'utiliser [{{site.data.keyword.monitoringlong_notm}} pour les métriques](/docs/containers?topic=containers-health#view_metrics), pensez à donner un nom à votre cluster qui soit unique pour tous les groupes de ressources et toutes les régions de votre compte afin d'éviter des conflits de noms pour les métriques.
  * Les clusters gratuits sont automatiquement créés dans le groupe de ressources `par défaut`. 

5. **Clusters standard** : planifiez la [configuration de réseau](/docs/containers?topic=containers-plan_clusters) de votre cluster de sorte que celui-ci réponde aux besoins en termes de charges de travail et d'environnement. Ensuite, configurez votre mise en réseau d'infrastructure IBM Cloud (SoftLayer) pour autoriser la communication entre les noeuds worker et le maître et entre les utilisateurs et le maître :
  * Pour utiliser le noeud final de service privé uniquement ou les noeuds finaux de service publics et privés (exécution de charges de travail accessibles sur Internet ou extension de votre centre de données sur site) : 
    1. Activez [VRF](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) dans votre compte d'infrastructure IBM Cloud (SoftLayer).
    2. [Activez votre compte {{site.data.keyword.Bluemix_notm}} pour utiliser des noeuds finaux de service](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started).
    <p class="note">Le maître Kubernetes est accessible via le noeud final de service privé si les utilisateurs de cluster autorisés se trouvent dans votre réseau privé {{site.data.keyword.Bluemix_notm}} ou s'ils sont connectés au réseau privé via une [connexion VPN](/docs/infrastructure/iaas-vpn?topic=VPN-gettingstarted-with-virtual-private-networking) ou [{{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link). Toutefois, la communication avec le maître Kubernetes via le noeud final de service privé doit passer par la plage d'adresses IP <code>166.X.X.X</code>, qui n'est pas routable à partir d'une connexion VPN, ou par {{site.data.keyword.Bluemix_notm}} Direct Link. Vous pouvez exposer le noeud final de service privé du maître pour vos utilisateurs de cluster en utilisant un équilibreur de charge de réseau (NLB) privé. Le NLB privé expose le noeud final de service privé du maître en tant que plage d'adresses IP de cluster <code>10.X.X.X</code> interne auxquelles les utilisateurs peuvent accéder à l'aide de la connexion VPN ou {{site.data.keyword.Bluemix_notm}} Direct Link. Si vous activez uniquement le noeud final de service privé, vous pouvez utiliser le tableau de bord Kubernetes ou activer temporairement le noeud final de service public pour créer le NLB privé.
Pour plus d'informations, voir [Accès aux clusters via le noeud final de service privé](/docs/containers?topic=containers-clusters#access_on_prem).</p>

  * Pour utiliser le noeud final de service public uniquement (exécution de charges de travail accessibles sur Internet) :
    1. Activez la fonction [Spanning VLAN](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning) pour votre compte d'infrastructure IBM Cloud (SoftLayer) afin que vos noeuds worker puissent communiquer entre eux sur le réseau privé. Pour effectuer cette action, vous devez disposer du [droit d'infrastructure](/docs/containers?topic=containers-users#infra_access) **Réseau > Gérer le spanning VLAN pour réseau**, ou vous pouvez demander au propriétaire du compte de l'activer. Pour vérifier si le spanning VLAN est déjà activé, utilisez la [commande](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get) `ibmcloud ks vlan-spanning-get --region <region>`. 
  * Pour utiliser un périphérique de passerelle (extension de votre centre de données sur site) :
    1. Activez la fonction [Spanning VLAN](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning) pour votre compte d'infrastructure IBM Cloud (SoftLayer) afin que vos noeuds worker puissent communiquer entre eux sur le réseau privé. Pour effectuer cette action, vous devez disposer du [droit d'infrastructure](/docs/containers?topic=containers-users#infra_access) **Réseau > Gérer le spanning VLAN pour réseau**, ou vous pouvez demander au propriétaire du compte de l'activer. Pour vérifier si le spanning VLAN est déjà activé, utilisez la [commande](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get) `ibmcloud ks vlan-spanning-get --region <region>`. 
    2. Configurez un périphérique de passerelle. Par exemple, vous pouvez choisir de configurer un [dispositif de routeur virtuel (VRA)](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-about-the-vra) ou un [dispositif de sécurité Fortigate (FSA)](/docs/services/vmwaresolutions/services?topic=vmware-solutions-fsa_considerations) qui fera office de pare-feu pour autoriser le trafic nécessaire et bloquera le trafic indésirable. 
    3. [Ouvrez les ports et les adresses IP privés requis](/docs/containers?topic=containers-firewall#firewall_outbound) pour chaque région de sorte que le maître et les noeuds worker puissent communiquer et pour les services {{site.data.keyword.Bluemix_notm}} que vous prévoyez d'utiliser. 

<br />


## Préparation de la création de clusters au niveau du cluster
{: #prepare_cluster_level}

Après avoir configuré votre compte pour la création de clusters, préparez la configuration de votre cluster. Il s'agit de préparations qui impactent votre cluster à chaque fois que vous créez un cluster.
{: shortdesc}

1. Faites votre choix entre un [cluster gratuit ou standard](/docs/containers?topic=containers-cs_ov#cluster_types). Vous pouvez créer 1 cluster gratuit pour expérimenter certaines fonctionnalités pour une durée de 30 jours ou créer des clusters standard entièrement personnalisables avec l'isolement matériel de votre choix. Créez un cluster standard pour obtenir d'autres avantages et contrôler les performances de votre cluster.

2. Pour les clusters standard, planifiez votre configuration de cluster. 
  * Déterminez le type de cluster à créer : [cluster à zone unique](/docs/containers?topic=containers-ha_clusters#single_zone) ou [cluster à zones multiples](/docs/containers?topic=containers-ha_clusters#multizone). Notez que les clusters à zones multiples sont disponibles uniquement à certains emplacements.
  * Choisissez le type de [matériel et d'isolement](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes) à appliquer aux noeuds worker de votre cluster, y compris votre choix entre des machines virtuelles ou bare metal.

3. Pour les clusters standard, vous pouvez [estimer les coûts](/docs/billing-usage?topic=billing-usage-cost#cost) dans la console {{site.data.keyword.Bluemix_notm}}. Pour plus d'informations sur les frais qui ne sont pas forcément compris dans l'estimateur, voir [Tarification et facturation](/docs/containers?topic=containers-faqs#charges).

4. Si vous créez le cluster dans un environnement protégé derrière un pare-feu, par exemple pour les clusters qui étendent votre centre de données sur site, [autorisez le trafic réseau sortant vers les adresses IP publiques et privées](/docs/containers?topic=containers-firewall#firewall_outbound) des services {{site.data.keyword.Bluemix_notm}} que vous envisagez d'utiliser.

<br />


## Création d'un cluster gratuit
{: #clusters_free}

Vous pouvez utiliser votre premier cluster gratuit pour vous familiariser avec le mode de fonctionnement d'{{site.data.keyword.containerlong_notm}}. Avec les clusters gratuits, vous pouvez faire connaissance avec la terminologie utilisée, exécuter un tutoriel et prendre vos repères avant de vous lancer dans les clusters standard de niveau production. Ne vous inquiétez pas, vous bénéficiez toujours d'un cluster gratuit même si vous avez un compte facturable.
{: shortdesc}

Les clusters gratuits incluent un noeud worker configuré avec 2 UC virtuelles et 4 Go de mémoire et ont un cycle de vie de 30 jours. A l'issue de cette période, le cluster arrive à expiration et il est supprimé ainsi que toutes les données qu'il contient. Les données supprimées ne sont pas sauvegardées par {{site.data.keyword.Bluemix_notm}} et ne peuvent pas être récupérées. Veillez à effectuer la sauvegarde des données importantes.
{: note}

### Création d'un cluster gratuit dans la console
{: #clusters_ui_free}

1. Dans le [catalogue ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/catalog?category=containers), sélectionnez **{{site.data.keyword.containershort_notm}}** pour créer un cluster.
2. Sélectionnez le plan de cluster **Gratuit**.
3. Sélectionnez une région géographique dans laquelle déployer votre cluster.
4. Sélectionnez une agglomération dans la géographie. Votre cluster est créé dans une zone de cette agglomération. 
5. Attribuez un nom à votre cluster. Le nom doit commencer par une lettre, peut contenir des lettres, des nombres et des tirets (-) et ne doit pas dépasser 35 caractères. Utilisez un nom unique dans les régions. 
6. Cliquez sur **Créer un cluster**. Par défaut un pool de noeuds worker contenant un noeud worker est créé. Vous pouvez voir la progression du déploiement du noeud worker dans l'onglet **Noeuds worker**. Une fois le déploiement terminé, vous pouvez voir si le cluster est prêt dans l'onglet **Vue d'ensemble**. Notez que même si le cluster est prêt, certaines parties du cluster qui sont utilisées par d'autres services, telles que les valeurs confidentielles d'extraction d'image de registre, sont peut-être toujours en cours de traitement.

  A chaque noeud worker sont affectés un ID de noeud worker unique et un nom de domaine qui ne doivent pas être modifiés manuellement après la création du cluster. La modification de l'ID ou du domaine empêcherait le maître Kubernetes de gérer votre cluster.
  {: important}
7. Une fois votre cluster créé, vous pouvez [commencer à gérer votre cluster en configurant votre session CLI](#access_cluster).

### Création d'un cluster gratuit dans l'interface CLI
{: #clusters_cli_free}

Avant de commencer, installez l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}} et le [plug-in {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-cs_cli_install#cs_cli_install).

1. Connectez-vous à l'interface de ligne de commande d'{{site.data.keyword.Bluemix_notm}}.
  1. Connectez-vous et entrez vos données d'identification {{site.data.keyword.Bluemix_notm}} lorsque vous y êtes invité.
     ```
     ibmcloud login
     ```
     {: pre}

     Si vous disposez d'un ID fédéré, utilisez `ibmcloud login --sso` pour vous connecter à l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}}. Entrez votre nom d'utilisateur et utilisez l'URL mentionnée dans la sortie de l'interface de ligne de commande pour extraire votre code d'accès à usage unique. Si la connexion échoue alors que vous omettez l'option `--sso` et aboutit en incluant l'option `--sso`, ceci indique que votre ID est fédéré.
     {: tip}

  2. Si vous disposez de plusieurs comptes {{site.data.keyword.Bluemix_notm}}, sélectionnez le compte que vous voulez utiliser pour créer votre cluster Kubernetes.

  3. Pour créer le cluster gratuit dans une région spécifique, vous devez cibler cette région. Vous pouvez créer un cluster gratuit dans `ap-south`, `eu-central`, `uk-south` ou `us-south`. Le cluster est créé dans une zone de cette région.
```
     ibmcloud ks region-set
     ```
     {: pre}

2. Créer un cluster.
  ```
  ibmcloud ks cluster-create --name my_cluster
  ```
  {: pre}

3. Vérifiez que la création du cluster a été demandée. La commande de la machine de noeud worker peut prendre quelques minutes.
    ```
    ibmcloud ks clusters
    ```
    {: pre}

    Lorsque la mise à disposition de votre cluster est finalisée, le statut du cluster passe à **deployed**.
    ```
    Name         ID                                   State      Created          Workers   Zone       Version     Resource Group Name
    my_cluster   paf97e8843e29941b49c598f516de72101   deployed   20170201162433   1         mil01      1.13.6      Default
    ```
    {: screen}

4. Vérifiez l'état du noeud worker.
    ```
    ibmcloud ks workers --cluster <cluster_name_or_ID>
    ```
    {: pre}

    Lorsque le noeud worker est prêt, l'état passe à **normal** et le statut indique **Ready**. Lorsque le statut du noeud indique **Ready**, vous pouvez accéder au cluster. Notez que même si le cluster est prêt, certaines parties du cluster qui sont utilisées par d'autres services, telles que les valeurs confidentielles Ingress ou les valeurs confidentielles d'extraction d'image de registre, sont peut-être toujours en cours de traitement.
    ```
    ID                                                 Public IP       Private IP      Machine Type   State    Status   Zone        Version     Resource Group Name
    kube-mil01-paf97e8843e29941b49c598f516de72101-w1   169.xx.xxx.xxx  10.xxx.xx.xxx   free           normal   Ready    mil01       1.13.6      Default
    ```
    {: screen}

    A chaque noeud worker sont affectés un ID de noeud worker unique et un nom de domaine qui ne doivent pas être modifiés manuellement après la création du cluster. La modification de l'ID ou du domaine empêcherait le maître Kubernetes de gérer votre cluster.
    {: important}

5. Une fois votre cluster créé, vous pouvez [commencer à gérer votre cluster en configurant votre session CLI](#access_cluster).

<br />


## Création d'un cluster standard
{: #clusters_standard}

Utilisez l'interface CLI {{site.data.keyword.Bluemix_notm}} ou la console {{site.data.keyword.Bluemix_notm}} pour créer un cluster standard entièrement personnalisable avec un isolement de matériel de votre choix et accédez à des fonctions, par exemple plusieurs noeuds worker, pour bénéficier d'un environnement à haute disponibilité.
{: shortdesc}

### Création d'un cluster standard dans la console
{: #clusters_ui}

1. Dans le [catalogue ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/catalog?category=containers), sélectionnez **{{site.data.keyword.containershort_notm}}** pour créer un cluster.

2. Sélectionnez un groupe de ressources dans lequel créer votre cluster.
  **Remarque **:
    * Un cluster ne peut être créé que dans un seul groupe de ressources, et une fois le cluster créé, vous ne pouvez pas modifier son groupe de ressources.
    * Pour créer des clusters dans un autre groupe de ressources que le groupe par défaut, vous devez au moins disposer du [rôle **Afficheur**](/docs/containers?topic=containers-users#platform) pour le groupe de ressources.

3. Sélectionnez une région géographique dans laquelle déployer votre cluster.

4. Attribuez un nom unique à votre cluster. Le nom doit commencer par une lettre, peut contenir des lettres, des nombres et des tirets (-) et ne doit pas dépasser 35 caractères. Utilisez un nom unique dans les régions. Le nom du cluster et la région dans laquelle est déployé le cluster constituent le nom de domaine qualifié complet du sous-domaine Ingress. Pour garantir que ce sous-domaine est unique dans une région, le nom de cluster peut être tronqué et complété par une valeur aléatoire dans le nom de domaine Ingress.
 **Remarque** : toute modification de l'ID unique ou du nom de domaine affecté lors de la création empêche le maître Kubernetes de gérer votre cluster.

5. Sélectionnez la disponibilité **Zone unique** ou **Zones multiples**. Dans un cluster à zones multiples, le noeud maître est déployé dans une zone compatible avec plusieurs zones et les ressources de votre cluster sont réparties sur plusieurs zones.

6. Entrez les détails de votre zone et de votre agglomération.
  * Clusters à zones multiples :
    1. Sélectionnez une agglomération. Pour des performances optimales, sélectionnez l'agglomération la plus proche de vous. Vos options peuvent être limitées par géographie. 
    2. Sélectionnez les zones spécifiques dans lesquelles vous souhaitez héberger votre cluster. Vous devez sélectionner au moins 1 zone mais vous pouvez en sélectionner autant que vous voulez. Si vous sélectionnez plusieurs zones, les noeuds worker sont répartis sur les zones que vous avez choisies ce qui vous offre une plus grande disponibilité. Si vous vous contentez de sélectionner 1 zone, vous pouvez [ajouter des zones dans votre cluster](/docs/containers?topic=containers-add_workers#add_zone) une fois qu'il a été créé.
  * Clusters à zone unique : Sélectionnez une zone dans laquelle vous souhaitez héberger votre cluster. Pour des performances optimales, sélectionnez la zone la plus proche de vous. Vos options peuvent être limitées par géographie. 

7. Pour chaque zone, choisissez des VLAN.
  * Pour créer un cluster dans lequel vous exécutez des charges de travail accessibles sur Internet :
    1. Sélectionnez un VLAN public et un VLAN privé dans votre compte d'infrastructure IBM Cloud (SoftLayer) pour chaque zone. Les noeuds worker communiquent entre eux en utilisant le VLAN privé et peuvent communiquer avec le maître Kubernetes en utilisant le VLAN public ou privé. Si vous ne disposez pas de VLAN public ou privé dans cette zone, un VLAN public et un VLAN privé sont automatiquement créés pour vous. Vous pouvez utiliser le même VLAN pour plusieurs clusters.
  * Pour créer un cluster qui étend votre centre de données sur site sur le réseau privé uniquement, qui étend votre centre de données sur site avec la possibilité d'ajouter ultérieurement un accès public limité ou d'étendre votre centre de données sur site et qui fournit un accès public limité via un périphérique de passerelle :
    1. Sélectionnez un VLAN privé à partir de votre compte d'infrastructure IBM Cloud (SoftLayer) pour chaque zone. Les noeuds worker communiquent entre eux via le VLAN privé. Si vous ne disposez pas de VLAN privé dans une zone, un VLAN privé est automatiquement créé pour vous. Vous pouvez utiliser le même VLAN pour plusieurs clusters.
    2. Pour le VLAN public, sélectionnez **Aucun**.

8. Pour **Noeud final de service maître**, choisissez le mode de communication entre votre maître Kubernetes et vos noeuds worker. 
  * Pour créer un cluster dans lequel vous exécutez des charges de travail accessibles sur Internet :
    * Si la fonction VRF et des noeuds finaux de service sont activés dans votre compte {{site.data.keyword.Bluemix_notm}}, sélectionnez **Noeuds finaux public et privé**.
    * Si vous ne parvenez pas à activer la fonction VRF ou si vous ne souhaitez pas le faire, sélectionnez **Noeud final public uniquement**. 
  * Pour créer un cluster qui étend votre centre de données sur site uniquement ou un cluster qui étend votre centre de données sur site et fournit un accès public limité avec des noeuds worker de périphérie, sélectionnez **Noeuds finaux privés et publics** ou **Noeud final privé uniquement**. Vérifiez que vous avez activé la fonction VRF et des noeuds finaux de service dans votre compte {{site.data.keyword.Bluemix_notm}}. Notez que si vous activez le noeud final de service privé uniquement, vous devez [exposer le noeud final maître via un équilibreur de charge de réseau privé](#access_on_prem) de sorte que les utilisateurs puissent accéder au maître via une connexion VPN ou {{site.data.keyword.BluDirectLink}}. 
  * Pour créer un cluster qui étend votre centre de données sur site et fournit un accès public limité avec un périphérique de passerelle, sélectionnez **Noeud final public uniquement**. 

9. Configurez votre pool de noeuds worker par défaut. Les pools de noeuds worker sont des groupes de noeuds worker qui partagent la même configuration. Vous pouvez toujours ajouter d'autres pools de noeuds worker à votre cluster par la suite.
  1. Sélectionnez la version du serveur d'API Kubernetes pour le noeud du maître cluster et les noeuds worker.
  2. Filtrez les versions de noeud worker en sélectionnant un type de machine. Le type virtuel est facturé à l'heure et le type bare metal est facturé au mois.
    - **Bare metal** : facturés au mois, les serveurs bare metal sont mis à disposition manuellement par l'infrastructure IBM Cloud (SoftLayer) après leur commande et cette opération peut prendre plus d'un jour ouvrable. La configuration bare metal convient le mieux aux applications à hautes performances qui nécessitent plus de ressources et de contrôle hôte. Vous pouvez également choisir d'activer la fonction [Calcul sécurisé](/docs/containers?topic=containers-security#trusted_compute) pour vérifier que vos noeuds worker ne font pas l'objet de falsification. La fonction de calcul sécurisé (Trusted Compute) est disponible pour les types de machine bare metal sélectionnés. Par exemple, les versions GPU `mgXc` ne prennent pas en charge la fonction de calcul sécurisé. Si vous n'activez pas cette fonction lors de la création du cluster mais souhaitez le faire ultérieurement, vous pouvez utiliser la [commande](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable) `ibmcloud ks feature-enable`. Après avoir activé cette fonction, vous ne pourrez plus la désactiver par la suite.
    Assurez-vous de vouloir mettre à disposition une machine bare metal. Comme elle est facturée au mois, si vous l'annulez immédiatement après l'avoir commandée par erreur, vous serez toujours redevable pour le mois complet.
    {:tip}
    - **Virtuel - partagé** : les ressources de l'infrastructure, telles que l'hyperviseur et le matériel physique, sont partagées par vous et d'autres clients IBM, mais vous êtes seul à accéder à chaque noeud worker. Bien que cette solution soit moins onéreuse et suffisante dans la plupart des cas, vérifiez cependant les consignes de votre entreprise relatives aux exigences en termes de performance et d'infrastructure.
    - **Virtuel - dédié** : vos noeuds worker sont hébergés sur l'infrastructure réservée à votre compte. Vos ressources physiques sont complètement isolées.
  3. Sélectionnez une version. La version définit le nombre d'UC virtuelles, la mémoire et l'espace disque configurés dans chaque noeud worker et rendus disponibles pour les conteneurs. Les types de machines virtuelles et bare metal disponibles varient en fonction de la zone de déploiement du cluster. Après avoir créé votre cluster, vous pouvez ajouter différents types de machine en ajoutant un noeud worker ou un pool de noeuds worker dans le cluster.
  4. Indiquez le nombre de noeuds worker dont vous avez besoin dans le cluster. Le nombre de noeuds worker que vous entrez est répliqué sur le nombre de zones que vous avez sélectionnées. Cela signifie que si vous disposez de 2 zones et que vous sélectionnez 3 noeuds worker, 6 noeuds sont mis à disposition et 3 noeuds résident dans chaque zone.

10. Cliquez sur **Créer un cluster**. Un pool de noeuds worker est créé avec le nombre de noeuds worker que vous avez spécifié. Vous pouvez voir la progression du déploiement du noeud worker dans l'onglet **Noeuds worker**. Une fois le déploiement terminé, vous pouvez voir si le cluster est prêt dans l'onglet **Vue d'ensemble**. Notez que même si le cluster est prêt, certaines parties du cluster qui sont utilisées par d'autres services, telles que les valeurs confidentielles Ingress ou les valeurs confidentielles d'extraction d'image de registre, sont peut-être toujours en cours de traitement.
    

  A chaque noeud worker sont affectés un ID de noeud worker unique et un nom de domaine qui ne doivent pas être modifiés manuellement après la création du cluster. La modification de l'ID ou du domaine empêcherait le maître Kubernetes de gérer votre cluster.
  {: important}

11. Une fois votre cluster créé, vous pouvez [commencer à gérer votre cluster en configurant votre session CLI](#access_cluster).

### Création d'un cluster standard dans l'interface de ligne de commande
{: #clusters_cli_steps}

Avant de commencer, installez l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}} et le [plug-in {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-cs_cli_install#cs_cli_install).

1. Connectez-vous à l'interface de ligne de commande d'{{site.data.keyword.Bluemix_notm}}.
  1. Connectez-vous et entrez vos données d'identification {{site.data.keyword.Bluemix_notm}} lorsque vous y êtes invité.
     ```
     ibmcloud login
     ```
     {: pre}

     Si vous disposez d'un ID fédéré, utilisez `ibmcloud login --sso` pour vous connecter à l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}}. Entrez votre nom d'utilisateur et utilisez l'URL mentionnée dans la sortie de l'interface de ligne de commande pour extraire votre code d'accès à usage unique. Si la connexion échoue alors que vous omettez l'option `--sso` et aboutit en incluant l'option `--sso`, ceci indique que votre ID est fédéré.
     {: tip}

  2. Si vous disposez de plusieurs comptes {{site.data.keyword.Bluemix_notm}}, sélectionnez le compte que vous voulez utiliser pour créer votre cluster Kubernetes.

  3. Pour créer des clusters dans un autre groupe de ressources que le groupe par défaut, ciblez ce groupe de ressources. **Remarque **:
      * Un cluster ne peut être créé que dans un seul groupe de ressources, et une fois le cluster créé, vous ne pouvez pas modifier son groupe de ressources.
      * Vous devez au moins disposer du [rôle **Afficheur**](/docs/containers?topic=containers-users#platform) pour le groupe de ressources.

      ```
      ibmcloud target -g <resource_group_name>
      ```
      {: pre}

2. Passez en revue les zones qui sont disponibles. Dans la sortie de la commande suivante, le **type d'emplacement** des zones est `dc`. Pour étendre votre cluster à plusieurs zones, vous devez créer le cluster dans une [zone compatible avec plusieurs zones](/docs/containers?topic=containers-regions-and-zones#zones). Les zones compatibles avec plusieurs zones ont une valeur d'agglomération dans la colonne **Métropole multizone**.
    ```
    ibmcloud ks supported-locations
    ```
    {: pre}
    Si vous sélectionnez une zone à l'étranger, il se peut que vous ayez besoin d'une autorisation légale pour stocker physiquement les données dans un pays étranger.
    {: note}

3. Passez en revue les versions de noeud worker qui sont disponibles dans cette zone. La version de noeud worker spécifie les hôtes de calcul virtuels ou physiques disponibles pour chaque noeud worker.
  - **Virtuel** : facturées à l'heure, les machines virtuelles sont mises à disposition sur du matériel partagé ou dédié.
  - **Physique** : facturés au mois, les serveurs bare metal sont mis à disposition manuellement par l'infrastructure IBM Cloud (SoftLayer) après leur commande et cette opération peut prendre plus d'un jour ouvrable. La configuration bare metal convient le mieux aux applications à hautes performances qui nécessitent plus de ressources et de contrôle hôte.
  - **Machines physiques avec la fonction de calcul sécurisé** : vous pouvez choisir d'activer la fonction de [calcul sécurisé](/docs/containers?topic=containers-security#trusted_compute) pour vérifier que vos noeuds worker ne font pas l'objet de falsification. La fonction de calcul sécurisé (Trusted Compute) est disponible pour les types de machine bare metal sélectionnés. Par exemple, les versions GPU `mgXc` ne prennent pas en charge la fonction de calcul sécurisé. Si vous n'activez pas cette fonction lors de la création du cluster mais souhaitez le faire ultérieurement, vous pouvez utiliser la [commande](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable) `ibmcloud ks feature-enable`. Après avoir activé cette fonction, vous ne pourrez plus la désactiver par la suite.
  - **Types de machine** : pour décider du type de machine à déployer, examinez les combinaisons coeur/mémoire/stockage du [matériel disponible pour les noeuds worker](/docs/containers?topic=containers-planning_worker_nodes#shared_dedicated_node). Après avoir créé votre cluster, vous pouvez ajouter différents types de machine physique ou virtuelle en [ajoutant un pool de noeuds worker](/docs/containers?topic=containers-add_workers#add_pool).

     Assurez-vous de vouloir mettre à disposition une machine bare metal. Comme elle est facturée au mois, si vous l'annulez immédiatement après l'avoir commandée par erreur, vous serez toujours redevable pour le mois complet.
     {:tip}

  ```
  ibmcloud ks machine-types --zone <zone>
  ```
  {: pre}

4. Vérifiez si des VLAN pour la zone existent déjà dans l'infrastructure IBM Cloud (SoftLayer) pour ce compte.
  ```
  ibmcloud ks vlans --zone <zone>
  ```
  {: pre}

  Exemple de sortie :
  ```
  ID        Name                Number   Type      Router  
        1519999   vlan   1355     private   bcr02a.dal10  
        1519898   vlan   1357     private   bcr02a.dal10 
        1518787   vlan   1252     public   fcr02a.dal10 
        1518888   vlan   1254     public    fcr02a.dal10
  ```
  {: screen}
  * Pour créer un cluster dans lequel vous exécutez des charges de travail accessibles sur Internet, vérifiez si des VLAN public et privé existent. S'il existe déjà un réseau virtuel public et privé, notez les routeurs correspondants. Les routeurs de VLAN privé commencent toujours par <code>bcr</code> (routeur de back-end) et les routeurs de VLAN public par <code>fcr</code> (routeur de front-end). Lors de la création d'un cluster et de la spécification des VLAN publics et privés, le nombre et la combinaison de lettres après ces préfixes doivent correspondre. Dans l'exemple de sortie, n'importe quel VLAN privé peut être utilisé avec l'un des VLAN publics étant donné que les routeurs incluent tous `02a.dal10`.
  * Pour créer un cluster qui étend votre centre de données sur site sur le réseau privé uniquement, qui étend votre centre de données sur site avec la possibilité d'ajouter ultérieurement un accès public limité via des noeuds worker de périphérie ou qui étend votre centre de données sur site et fournit un accès public limité via un périphérique de passerelle, vérifiez si un VLAN privé existe. Si vous disposez d'un VLAN privé, notez l'ID. 

5. Exécutez la commande `cluster-create`. Par défaut, les disques de noeud worker sont chiffrés avec AES 256 bits et le cluster est facturé par heures d'utilisation. 
  * Pour créer un cluster dans lequel vous exécutez des charges de travail accessibles sur Internet :
    ```
    ibmcloud ks cluster-create --zone <zone> --machine-type <machine_type> --hardware <shared_or_dedicated> --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID> --workers <number> --name <cluster_name> --kube-version <major.minor.patch> [--private-service-endpoint] [--public-service-endpoint] [--disable-disk-encrypt] [--trusted]
    ```
    {: pre}
  * Pour créer un cluster qui étend votre centre de données sur site sur le réseau privé, avec la possibilité d'ajouter ultérieurement un accès public limité via des noeuds worker de périphérie :
    ```
    ibmcloud ks cluster-create --zone <zone> --machine-type <machine_type> --hardware <shared_or_dedicated> --private-vlan <private_VLAN_ID> --private-only --workers <number> --name <cluster_name> --kube-version <major.minor.patch> --private-service-endpoint [--public-service-endpoint] [--disable-disk-encrypt] [--trusted]
    ```
    {: pre}
  * Pour créer un cluster qui étend votre centre de données sur site et fournit un accès public limité via un périphérique de passerelle :
```
    ibmcloud ks cluster-create --zone <zone> --machine-type <machine_type> --hardware <shared_or_dedicated> --private-vlan <private_VLAN_ID> --private-only --workers <number> --name <cluster_name> --kube-version <major.minor.patch> --public-service-endpoint [--disable-disk-encrypt] [--trusted]
    ```
    {: pre}

    <table>
    <caption>Composantes de la commande cluster-create</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composantes de cette commande</th>
    </thead>
    <tbody>
    <tr>
    <td><code>cluster-create</code></td>
    <td>Commande de création d'un cluster dans votre organisation {{site.data.keyword.Bluemix_notm}}.</td>
    </tr>
    <tr>
    <td><code>--zone <em>&lt;zone&gt;</em></code></td>
    <td>Spécifiez l'ID de zone {{site.data.keyword.Bluemix_notm}} où vous souhaitez créer votre cluster et que vous avez choisi à l'étape 4. </td>
    </tr>
    <tr>
    <td><code>--machine-type <em>&lt;machine_type&gt;</em></code></td>
    <td>Indiquez le type de machine que vous avez choisi à l'étape 5.</td>
    </tr>
    <tr>
    <td><code>--hardware <em>&lt;shared_or_dedicated&gt;</em></code></td>
    <td>Spécifiez le niveau d'isolement de matériel pour votre noeud worker. Utilisez dedicated pour que toutes les ressources physiques vous soient dédiées exclusivement ou shared pour permettre leur partage avec d'autres clients IBM. La valeur par défaut est shared. Cette valeur est facultative pour les clusters standard de MV. Pour les types de machine bare metal, indiquez `dedicated`.</td>
    </tr>
    <tr>
    <td><code>--public-vlan <em>&lt;public_vlan_id&gt;</em></code></td>
    <td>Si vous disposez déjà d'un VLAN public configuré dans votre compte d'infrastructure IBM Cloud (SoftLayer) pour cette zone, entrez l'ID du VLAN public que vous avez trouvé à l'étape 4. <p>Les routeurs de VLAN privé commencent toujours par <code>bcr</code> (routeur de back-end) et les routeurs de VLAN public par <code>fcr</code> (routeur de front-end). Lors de la création d'un cluster et de la spécification des VLAN publics et privés, le nombre et la combinaison de lettres après ces préfixes doivent correspondre.</p></td>
    </tr>
    <tr>
    <td><code>--private-vlan <em>&lt;private_vlan_id&gt;</em></code></td>
    <td>Si vous disposez déjà d'un VLAN privé configuré dans votre compte d'infrastructure IBM Cloud (SoftLayer) pour cette zone, entrez l'ID du VLAN privé que vous avez trouvé à l'étape 4. Si vous ne possédez pas de VLAN privé dans votre compte, ne spécifiez pas cette option. {{site.data.keyword.containerlong_notm}} crée automatiquement un VLAN privé pour vous.<p>Les routeurs de VLAN privé commencent toujours par <code>bcr</code> (routeur de back-end) et les routeurs de VLAN public par <code>fcr</code> (routeur de front-end). Lors de la création d'un cluster et de la spécification des VLAN publics et privés, le nombre et la combinaison de lettres après ces préfixes doivent correspondre.</p></td>
    </tr>
    <tr>
    <td><code>--private-only </code></td>
    <td>Créez le cluster avec des VLAN privés uniquement. Si vous incluez cette option, n'ajoutez pas l'option <code>--public-vlan</code>. </td>
    </tr>
    <tr>
    <td><code>--name <em>&lt;name&gt;</em></code></td>
    <td>Indiquez un nom pour le cluster.
Le nom doit commencer par une lettre, peut contenir des lettres, des nombres et des tirets (-) et ne doit pas dépasser 35 caractères. Utilisez un nom unique dans les régions. Le nom du cluster et la région dans laquelle est déployé le cluster constituent le nom de domaine qualifié complet du sous-domaine Ingress. Pour garantir que ce sous-domaine est unique dans une région, le nom de cluster peut être tronqué et complété par une valeur aléatoire dans le nom de domaine Ingress.
</td>
    </tr>
    <tr>
    <td><code>--workers <em>&lt;number&gt;</em></code></td>
    <td>Spécifiez le nombre de noeuds worker à inclure dans le cluster. Si l'option <code>--workers</code> n'est pas spécifiée, 1 noeud worker est créé.</td>
    </tr>
    <tr>
    <td><code>--kube-version <em>&lt;major.minor.patch&gt;</em></code></td>
    <td>Version Kubernetes du noeud du maître cluster. Cette valeur est facultative. Lorsque la version n'est pas spécifiée, le cluster est créé avec la valeur par défaut des versions Kubernetes prises en charge. Pour voir les versions disponibles, exécutez la commande <code>ibmcloud ks versions</code>.
</td>
    </tr>
    <tr>
    <td><code>--private-service-endpoint</code></td>
    <td>**Dans les [comptes activés pour VRF](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started)** : activez le noeud final de service privé de sorte que votre maître Kubernetes et les noeuds worker puissent communiquer sur le VLAN privé. De plus, vous pouvez choisir d'activer le noeud final de service public en utilisant l'indicateur `--public-service-endpoint` pour accéder à votre cluster sur Internet. Si vous activez uniquement le noeud final de service privé, vous devez être connecté au VLAN privé pour communiquer avec le maître Kubernetes. Après avoir activé le noeud final de service privé, vous ne pourrez plus le désactiver par la suite.<br><br>Après avoir créé le cluster, vous pouvez obtenir le noeud final en exécutant la commande `ibmcloud ks cluster-get --cluster <cluster_name_or_ID>`. </td>
    </tr>
    <tr>
    <td><code>--public-service-endpoint</code></td>
    <td>Activez le noeud final de service public pour que le maître Kubernetes soit accessible sur le réseau public, par exemple pour exécuter des commandes `kubectl` depuis votre terminal et pour que votre maître Kubernetes et les noeuds worker puissent communiquer sur le VLAN public. Vous pouvez désactiver le noeud final de service public si vous voulez un cluster privé uniquement.<br><br>Après avoir créé le cluster, vous pouvez obtenir le noeud final en exécutant la commande `ibmcloud ks cluster-get --cluster <cluster_name_or_ID>`. </td>
    </tr>
    <tr>
    <td><code>--disable-disk-encrypt</code></td>
    <td>Par défaut, les noeuds worker disposent du [chiffrement de disque](/docs/containers?topic=containers-security#encrypted_disk) avec l'algorithme AES 256 bits. Incluez cette option si vous désirez désactiver le chiffrement.</td>
    </tr>
    <tr>
    <td><code>--trusted</code></td>
    <td>**Clusters bare metal** : activez la fonction [Calcul sécurisé](/docs/containers?topic=containers-security#trusted_compute) pour vérifier que vos noeuds worker bare metal ne font pas l'objet de falsification. La fonction de calcul sécurisé (Trusted Compute) est disponible pour les types de machine bare metal sélectionnés. Par exemple, les versions GPU `mgXc` ne prennent pas en charge la fonction de calcul sécurisé. Si vous n'activez pas cette fonction lors de la création du cluster mais souhaitez le faire ultérieurement, vous pouvez utiliser la [commande](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable) `ibmcloud ks feature-enable`. Après avoir activé cette fonction, vous ne pourrez plus la désactiver par la suite.</td>
    </tr>
    </tbody></table>

6. Vérifiez que la création du cluster a été demandée. Pour les machines virtuelles, la commande des postes de noeud worker et la mise à disposition et la configuration du cluster dans votre compte peuvent prendre quelques minutes. Les machines physiques bare metal sont mises à disposition par interaction manuelle avec l'infrastructure IBM Cloud (SoftLayer) et cette opération peut prendre plus d'un jour ouvrable.
    ```
    ibmcloud ks clusters
    ```
    {: pre}

    Lorsque la mise à disposition de votre cluster est finalisée, le statut du cluster passe à **deployed**.
    ```
    Name         ID                                   State      Created          Workers   Zone       Version     Resource Group Name
    my_cluster   paf97e8843e29941b49c598f516de72101   deployed   20170201162433   1         mil01      1.13.6      Default
    ```
    {: screen}

7. Vérifiez le statut des noeuds worker.
    ```
    ibmcloud ks workers --cluster <cluster_name_or_ID>
    ```
    {: pre}

    Lorsque les noeuds worker sont prêts, l'état passe à **normal** et le statut indique **Ready**. Lorsque le statut du noeud indique **Ready**, vous pouvez accéder au cluster. Notez que même si le cluster est prêt, certaines parties du cluster qui sont utilisées par d'autres services, telles que les valeurs confidentielles Ingress ou les valeurs confidentielles d'extraction d'image de registre, sont peut-être toujours en cours de traitement.
    Notez que si vous avez créé votre cluster avec un VLAN privé uniquement, aucune adresse **IP publique** n'est affectée à vos noeuds worker. 

    ```
    ID                                                 Public IP       Private IP      Machine Type   State    Status   Zone        Version     Resource Group Name
    kube-mil01-paf97e8843e29941b49c598f516de72101-w1   169.xx.xxx.xxx  10.xxx.xx.xxx   standard       normal   Ready    mil01       1.13.6      Default
    ```
    {: screen}

    A chaque noeud worker sont affectés un ID de noeud worker unique et un nom de domaine qui ne doivent pas être modifiés manuellement après la création du cluster. La modification de l'ID ou du domaine empêcherait le maître Kubernetes de gérer votre cluster.
    {: important}

8. Une fois votre cluster créé, vous pouvez [commencer à gérer votre cluster en configurant votre session CLI](#access_cluster).

<br />


## Accès à votre cluster
{: #access_cluster}

Une fois votre cluster créé, vous pouvez commencer à gérer votre cluster en configurant votre session CLI.
{: shortdesc}

### Accès aux clusters via le noeud final de service public
{: #access_internet}

Pour gérer votre cluster, définissez le cluster que vous avez créé en tant que contexte pour une session CLI afin d'exécuter des commandes `kubectl`.
{: shortdesc}

1. Si votre réseau est protégé par un pare-feu d'entreprise, autorisez l'accès aux noeuds finaux d'API et aux ports {{site.data.keyword.Bluemix_notm}} et {{site.data.keyword.containerlong_notm}}. 
  1. [Autorisez l'accès aux noeuds finaux publics pour l'API `ibmcloud` et l'API `ibmcloud ks` dans votre pare-feu](/docs/containers?topic=containers-firewall#firewall_bx). 
  2. [Autorisez vos utilisateurs de cluster autorisés à exécuter des commandes `kubectl`](/docs/containers?topic=containers-firewall#firewall_kubectl) pour accéder au maître via le noeud final de service public uniquement, le noeud final de service privé uniquement ou les noeuds finaux de service publics et privés. 
  3. [Autorisez vos utilisateurs de cluster autorisés à exécuter des commandes `calicotl`](/docs/containers?topic=containers-firewall#firewall_calicoctl) pour gérer les règles de réseau Calico dans votre cluster. 

2. Définissez le cluster que vous avez créé comme contexte de cette session. Effectuez ces étapes de configuration à chaque fois que vous utilisez votre cluster.

  Si vous souhaitez utiliser la console {{site.data.keyword.Bluemix_notm}} à la place, vous pouvez exécuter des commandes CLI directement depuis votre navigateur Web dans le [terminal Kubernetes](/docs/containers?topic=containers-cs_cli_install#cli_web).
  {: tip}
  1. Obtenez la commande permettant de définir la variable d'environnement et téléchargez les fichiers de configuration Kubernetes.
      ```
      ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
      ```
      {: pre}
      Une fois les fichiers de configuration téléchargés, une commande s'affiche ; elle vous permet de définir le chemin vers le fichier de configuration Kubernetes local en tant que variable d'environnement.

      Exemple pour OS X :
      ```
      export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/mycluster/kube-config-prod-dal10-mycluster.yml
      ```
      {: screen}
  2. Copiez et collez la commande qui s'affiche sur votre terminal pour définir la variable d'environnement `KUBECONFIG`.
  3. Vérifiez que la variable d'environnement `KUBECONFIG` est correctement définie.
      Exemple pour OS X :
      ```
      echo $KUBECONFIG
      ```
      {: pre}

      Sortie :
      ```
      /Users/<user_name>/.bluemix/plugins/container-service/clusters/mycluster/kube-config-prod-dal10-mycluster.yml
      ```
      {: screen}

3. Lancez le tableau de bord Kubernetes via le port par défaut `8001`.
  1. Affectez le numéro de port par défaut au proxy.
      ```
      kubectl proxy
      ```
      {: pre}

      ```
      Starting to serve on 127.0.0.1:8001
      ```
      {: screen}

  2.  Ouvrez l'URL suivante dans un navigateur Web pour accéder au tableau de bord Kubernetes.
      ```
      http://localhost:8001/ui
      ```
      {: codeblock}

### Accès aux clusters via le noeud final de service privé
{: #access_on_prem}

Le maître Kubernetes est accessible via le noeud final de service privé si les utilisateurs de cluster autorisés se trouvent dans votre réseau privé {{site.data.keyword.Bluemix_notm}} ou s'ils sont connectés au réseau privé via une [connexion VPN](/docs/infrastructure/iaas-vpn?topic=VPN-gettingstarted-with-virtual-private-networking) ou [{{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link). Toutefois, la communication avec le maître Kubernetes via le noeud final de service privé doit passer par la plage d'adresses IP <code>166.X.X.X</code>, qui n'est pas routable à partir d'une connexion VPN, ou par {{site.data.keyword.Bluemix_notm}} Direct Link. Vous pouvez exposer le noeud final de service privé du maître pour vos utilisateurs de cluster en utilisant un équilibreur de charge de réseau (NLB) privé. Le NLB privé expose le noeud final de service privé du maître en tant que plage d'adresses IP de cluster <code>10.X.X.X</code> interne auxquelles les utilisateurs peuvent accéder à l'aide de la connexion VPN ou {{site.data.keyword.Bluemix_notm}} Direct Link. Si vous activez uniquement le noeud final de service privé, vous pouvez utiliser le tableau de bord Kubernetes ou activer temporairement le noeud final de service public pour créer le NLB privé.
{: shortdesc}

1. Si votre réseau est protégé par un pare-feu d'entreprise, autorisez l'accès aux noeuds finaux d'API et aux ports {{site.data.keyword.Bluemix_notm}} et {{site.data.keyword.containerlong_notm}}. 
  1. [Autorisez l'accès aux noeuds finaux publics pour l'API `ibmcloud` et l'API `ibmcloud ks` dans votre pare-feu](/docs/containers?topic=containers-firewall#firewall_bx). 
  2. [Autorisez vos utilisateurs de cluster autorisés à exécuter des commandes `kubectl`](/docs/containers?topic=containers-firewall#firewall_kubectl). Notez que vous ne pouvez pas tester la connexion à votre cluster lors de l'étape 6 tant que vous n'exposez pas le noeud final de service privé du maître sur le cluster à l'aide d'un NLB privé. 

2. Obtenez l'URL et le port de noeud final de service privé pour votre cluster.
  ```
  ibmcloud ks cluster-get --cluster <cluster_name_or_ID>
  ```
  {: pre}

  Dans cet exemple de sortie, l'**URL de noeud final de service privé** est `https://c1.private.us-east.containers.cloud.ibm.com:25073`.
  ```
  Name:                           setest
  ID:                             b8dcc56743394fd19c9f3db7b990e5e3
  State:                          normal
  Created:                        2019-04-25T16:03:34+0000
  Location:                       wdc04
  Master URL:                     https://c1.private.us-east.containers.cloud.ibm.com:25073
  Public Service Endpoint URL:    -
  Private Service Endpoint URL:   https://c1.private.us-east.containers.cloud.ibm.com:25073
  Master Location:                Washington D.C.
  ...
  ```
  {: screen}

3. Créez un fichier YAML nommé `kube-api-via-nlb.yaml`. Ce fichier YAML crée un service `LoadBalancer` privé et exposé le noeud final de service privé via ce NLB. Remplacez `<private_service_endpoint_port>` par le port que vous avez trouvé lors de l'étape précédente. 
  ```
  apiVersion: v1
  kind: Service
  metadata:
    name: kube-api-via-nlb
    annotations:
      service.kubernetes.io/ibm-load-balancer-cloud-provider-ip-type: private
    namespace: default
  spec:
    type: LoadBalancer
    ports:
    - protocol: TCP
      port: <private_service_endpoint_port>
      targetPort: <private_service_endpoint_port>
  ---
  kind: Endpoints
  apiVersion: v1
  metadata:
    name: kube-api-via-nlb
  subsets:
    - addresses:
        - ip: 172.20.0.1
      ports:
        - port: 2040
  ```
  {: codeblock}

4. Pour créer le NLB privé, vous devez être connecté au maître cluster. Comme vous ne pouvez pas encore vous connecter via le noeud final de service privé à partir d'une connexion VPN ou {{site.data.keyword.Bluemix_notm}} Direct Link, vous devez vous connecter au maître cluster et créer le NLB à l'aide du noeud final de service public ou du tableau de bord Kubernetes. 
  * Si vous avez activé le noeud final de service privé uniquement, vous pouvez utiliser le tableau de bord Kubernetes pour créer le NLB.
Le tableau de bord achemine automatiquement toutes les demandes vers le noeud final de service privé du maître. 
    1.  Connectez-vous à la [console{{site.data.keyword.Bluemix_notm}}](https://cloud.ibm.com/).
    2.  Dans la barre de menu, sélectionnez le compte que vous souhaitez utiliser.
    3.  Dans le menu ![Icône de menu](../icons/icon_hamburger.svg "Icône de menu"), cliquez sur **Kubernetes**.
    4.  Sur la page **Clusters**, cliquez sur le cluster auquel vous souhaitez accéder.
    5.  Sur la page des détails du cluster, cliquez sur **Tableau de bord Kubernetes**.
    6.  Cliquez sur **+ Créer**.
    7.  Sélectionnez **Créer à partir d'un fichier**, téléchargez le fichier `kube-api-via-nlb.yaml`, puis cliquez sur **Télécharger**.
    8.  Sur la page **Présentation**, vérifiez que le service `kube-api-via-nlb` est créé. Dans la colonne **Noeuds finaux externes**, notez l'adresse `10.x.x.x`. Cette adresse IP expose le noeud final de service privé pour le maître Kubernetes sur le port que vous avez spécifié dans votre fichier YAML. 

  * Si vous avez également activé le noeud final de service public, vous avez déjà accès au maître.
    1. Créez le NLB et le noeud final.
      ```
      kubectl apply -f kube-api-via-nlb.yaml
      ```
      {: pre}
    2. Vérifiez que le NLB `kube-api-via-nlb` est créé. Dans la sortie, notez l'adresse **EXTERNAL-IP** `10.x.x.x`. Cette adresse IP expose le noeud final de service privé pour le maître Kubernetes sur le port que vous avez spécifié dans votre fichier YAML.
      ```
      kubectl get svc -o wide
      ```
      {: pre}

      Dans cet exemple de sortie, l'adresse IP pour le noeud final de service privé du maître Kubernetes est `10.186.92.42`.
      ```
      NAME                     TYPE           CLUSTER-IP       EXTERNAL-IP      PORT(S)          AGE   SELECTOR
      kube-api-via-nlb         LoadBalancer   172.21.150.118   10.186.92.42     443:32235/TCP    10m   <none>
      ...
      ```
      {: screen}

  <p class="note">Si vous souhaitez vous connecter au maître à l'aide du [service VPN strongSwan](/docs/containers?topic=containers-vpn#vpn-setup), notez l'adresse **Cluster IP** `172.21.x.x` à utiliser à la place dans l'étape suivante. Etant donné que le pod VPN strongSwan s'exécute dans votre cluster, il peut accéder au NLB à l'aide de l'adresse IP du service IP de cluster interne. Dans votre fichier `config.yaml` pour la charte Helm strongSwan, assurez-vous que le routage CIDR du sous-réseau de service Kubernetes, `172.21.0.0/16`, figure dans le paramètre `local.subnet`. </p>

5. Sur les machines client où vous et vos utilisateurs exécutent des commandes `kubectl`, ajoutez l'adresse IP NLB et l'URL de noeud final de service privé au fichier `/etc/hosts`. N'ajoutez pas de ports dans l'adresse IP et l'URL et n'incluez pas `https://` dans l'URL.
  * Pour les utilisateurs OSX et Linux :
    ```
    sudo nano /etc/hosts
    ```
    {: pre}
  * Pour les utilisateurs Windows :
    ```
    notepad C:\Windows\System32\drivers\etc\hosts
    ```
    {: pre}

    En fonction des droits de votre machine locale, vous devrez peut-être exécuter Notepad en tant qu'administrateur pour éditer le fichier hosts.
    {: tip}

  Exemple de texte à ajouter :
  ```
  10.186.92.42      c1.private.us-east.containers.cloud.ibm.com
  ```
  {: codeblock}

6. Vérifiez que vous êtes connecté au réseau privé via une connexion VPN ou {{site.data.keyword.Bluemix_notm}} Direct Link. 

7. Assurez-vous que les commandes `kubectl` s'exécutent correctement via le noeud final de service privé en vérifiant la version du serveur CLI de Kubernetes.
  ```
  kubectl version  --short
  ```
  {: pre}

  Exemple de sortie :
  ```
  Client Version: v1.13.6
  Server Version: v1.13.6
  ```
  {: screen}

<br />


## Etapes suivantes
{: #next_steps}

Une fois le cluster opérationnel, vous pouvez réaliser les tâches suivantes :
- Si vous avez créé le cluster dans une zone compatible avec plusieurs zones, [répartissez les noeuds worker en ajoutant une zone à votre cluster](/docs/containers?topic=containers-add_workers).
- [Déployez une application dans votre cluster.](/docs/containers?topic=containers-app#app_cli)
- [Configurez votre propre registre privé dans {{site.data.keyword.Bluemix_notm}} pour stocker et partager des images Docker avec d'autres utilisateurs.](/docs/services/Registry?topic=registry-getting-started)
- [Configurez le programme de mise à l'échelle automatique du cluster (cluster autoscaler)](/docs/containers?topic=containers-ca#ca) pour l'ajout et le retrait automatique des noeuds worker dans vos pools de noeuds worker en fonction des demandes de ressources de vos charges de travail.
- Contrôlez qui peut créer des pods dans votre cluster avec les [politiques de sécurité de pod](/docs/containers?topic=containers-psp).
- Activez les modules complémentaires gérés [Istio](/docs/containers?topic=containers-istio) et [Knative](/docs/containers?topic=containers-serverless-apps-knative) pour étendre les capacités de votre cluster. 

Ensuite, vous pouvez vérifier les étapes de configuration de réseau suivantes pour votre configuration de cluster :

### Exécution de charges de travail d'application accessible sur Internet dans un cluster
{: #next_steps_internet}

* Isolez les charges de travail en réseau sur des [noeuds worker de périphérie](/docs/containers?topic=containers-edge).
* Exposez vos applications avec des [services de mise en réseau publics](/docs/containers?topic=containers-cs_network_planning#public_access).
* Contrôlez le trafic public vers les services de réseau qui exposent vos applications en créant des  [règles pre-DNAT Calico](/docs/containers?topic=containers-network_policies#block_ingress), telles que des règles d'inscription sur liste blanche et des règles d'inscription sur liste noire. 
* Connectez votre cluster à des services dans des réseaux privés hors de votre compte {{site.data.keyword.Bluemix_notm}} en configurant un [service VPN IPSec strongSwan](/docs/containers?topic=containers-vpn).

### Extension de votre centre de données sur site à un cluster et autorisation de l'accès public limité à l'aide de noeuds de périphérie et de règles réseau Calico
{: #next_steps_calico}

* Connectez votre cluster à des services dans des réseaux privés hors de votre compte {{site.data.keyword.Bluemix_notm}} en configurant une connexion [{{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link) ou le   [service VPN IPSec strongSwan](/docs/containers?topic=containers-vpn#vpn-setup).{{site.data.keyword.Bluemix_notm}} Direct Link permet la communication entre des applications et des services de votre cluster et un réseau sur site via le réseau privé, tandis que strongSwan autorise la communication via un tunnel VPN chiffré sur le réseau public. 
* Isolez les charges de travail de mise en réseau public en créant un [pool de noeuds worker de périphérie](/docs/containers?topic=containers-edge) regroupant des noeuds worker qui sont connectés à des VLAN publics et privés. 
* Exposez vos applications avec des [services de mise en réseau privés](/docs/containers?topic=containers-cs_network_planning#private_access).
* [Créez des règles réseau hôte Calico](/docs/containers?topic=containers-network_policies#isolate_workers) pour bloquer l'accès public à des pods, isoler votre cluster sur le réseau privé et autoriser l'accès à d'autres services {{site.data.keyword.Bluemix_notm}}. 

### Extension de votre centre de données sur site à un cluster et autorisation de l'accès public limité à l'aide d'un périphérique de passerelle
{: #next_steps_gateway}

* Si vous configurez également votre pare-feu de passerelle pour le réseau privé, vous devez [autoriser la communication entre les noeuds worker et permettre à votre cluster d'accéder aux ressources d'infrastructure sur le réseau privé](/docs/containers?topic=containers-firewall#firewall_private).

* Pour connecter en toute sécurité vos noeuds worker et vos applications à des réseaux privés en dehors de votre compte {{site.data.keyword.Bluemix_notm}}, configurez un noeud final VPN IPSec sur votre périphérique de passerelle. Ensuite, [configurez le service VPN IPSec strongSwan](/docs/containers?topic=containers-vpn#vpn-setup) dans votre cluster pour utiliser le noeud final VPN sur votre passerelle ou [configurez la connectivité VPN directement avec VRA](/docs/containers?topic=containers-vpn#vyatta).
* Exposez vos applications avec des [services de mise en réseau privés](/docs/containers?topic=containers-cs_network_planning#private_access).
* [Ouvrez les ports et les adresses IP requis](/docs/containers?topic=containers-firewall#firewall_inbound) dans votre pare-feu de périphérique de passerelle pour autoriser le trafic entrant vers les services de mise en réseau. 

### Extension de votre centre de données sur site à un cluster sur le réseau privé uniquement
{: #next_steps_extend}

* Si vous disposez d'un pare-feu sur le réseau privé, [autorisez la communication entre les noeuds worker et laissez votre cluster accéder aux ressources de l'infrastructure via le réseau privé](/docs/containers?topic=containers-firewall#firewall_private).

* Connectez votre cluster à des services dans des réseaux privés hors de votre compte {{site.data.keyword.Bluemix_notm}} en configurant une [connexion {{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link). 
* Exposez vos applications sur le réseau privé avec des [services de mise en réseau privés](/docs/containers?topic=containers-cs_network_planning#private_access).
