---

copyright:
  years: 2014, 2019
lastupdated: "2019-05-31"

keywords: kubernetes, iks, helm

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


# Partenaires IBM Cloud Kubernetes Service
{: #service-partners}

IBM s'emploie à faire d'{{site.data.keyword.containerlong_notm}} un service Kubernetes particulièrement performant qui vous aide à faire migrer, exploiter et administrer vos charge de travail Kubernetes. Afin de vous fournir toutes les fonctions dont vous avez besoin pour exécuter des charges de travail de production dans le cloud, {{site.data.keyword.containerlong_notm}} s'associe à d'autres fournisseurs de service tiers pour améliorer votre cluster avec des outils de journalisation, de surveillance et de stockage de haut niveau.
{: shortdesc}

Passez en revue nos partenaires et les avantages propres à chaque solution qu'ils fournissent. Pour découvrir les autres services open source tiers et {{site.data.keyword.Bluemix_notm}} que vous pouvez utiliser dans votre cluster, voir [Présentation des intégrations {{site.data.keyword.Bluemix_notm}} et tierces](/docs/containers?topic=containers-ibm-3rd-party-integrations).

## LogDNA
{: #logdna-partner}

{{site.data.keyword.la_full_notm}} fournit [LogDNA ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://logdna.com/) comme service tiers que vous pouvez utiliser pour ajouter des fonctions de journalisation intelligentes à votre cluster et à vos applications. 

### Avantages
{: #logdna-benefits}

Vous trouverez dans le tableau suivant une liste des principaux avantages liés à l'utilisation de LogDNA :
{: shortdesc}

|Avantage|Description|
|-------------|------------------------------|
|Gestion de journal et analyse de journal centralisées|Lorsque vous configurez le cluster comme source de journal, LogDNA commence automatiquement à collecter des informations de journalisation pour vos noeuds worker, vos pods, vos applications et votre réseau. Vos journaux sont automatiquement analysés, indexés, balisés et agrégés par LogDNA et visualisés dans le tableau de bord LogDNA et vous pouvez facilement étudier les ressources de votre cluster. Vous pouvez utiliser l'outil de représentation graphique intégré afin de visualiser les codes d'erreur ou les entrées de journal les plus courants. |
|Repérabilité facilitée avec une syntaxe de recherche de type Google|LogDNA utilise une syntaxe de recherche de type Google prenant en charge des termes standard, des opérations `AND` et `OR`, et vous permet d'exclure ou de combiner des termes de recherche afin de retrouver vos journaux plus facilement. Grâce à une indexation intelligente de journaux, vous pouvez passer à une entrée de journal spécifique à tout moment. |
|Chiffrement en transit et au repos|LogDNA chiffre automatiquement vs journaux afin de les sécuriser lorsqu'ils sont en transit et au repos. |
|Alertes personnalisées et vues de journal|Vous pouvez utiliser le tableau de bord pour trouver les journaux qui correspondent à vos critères de recherche, sauvegarder ces journaux dans une vue et partager cette vue avec d'autres utilisateurs pour simplifier le débogage entre les membres de l'équipe. Vous pouvez également utiliser cette vue pour créer une alerte que vous pouvez envoyer aux systèmes en aval, comme PagerDuty, Slack, ou par courrier électronique.   |
|Tableaux de bord prêts à l'emploi et personnalisés|Vous pouvez faire votre choix parmi une grande variété de tableaux de bord existants ou créer votre propre tableau de bord pour visualiser les journaux comme vous le souhaitez. |

### Intégration à {{site.data.keyword.containerlong_notm}}
{: #logdna-integration}

LogDNA est fourni par {{site.data.keyword.la_full_notm}}, un service de plateforme {{site.data.keyword.Bluemix_notm}} que vous pouvez utiliser avec votre cluster. {{site.data.keyword.la_full_notm}} est exploité par LogDNA en partenariat avec IBM.
{: shortdesc}

Pour utiliser LogDNA dans votre cluster, vous devez mettre à disposition une instance d'{{site.data.keyword.la_full_notm}} dans votre compte {{site.data.keyword.Bluemix_notm}} et configurer vos clusters Kubernetes en tant que source de journal. Une fois le cluster configuré, les journaux sont automatiquement collectés et transmis à votre instance de service {{site.data.keyword.la_full_notm}}. Vous pouvez utiliser le tableau de bord {{site.data.keyword.la_full_notm}} pour accéder à vos journaux.   

Pour plus d'informations, voir [Gestion des journaux de cluster Kubernetes à l'aide d'{{site.data.keyword.la_full_notm}}](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-kube).

### Facturation et assistance
{: #logdna-billing-support}

{{site.data.keyword.la_full_notm}} est entièrement intégré dans le système de support {{site.data.keyword.Bluemix_notm}}. Si vous rencontrez un problème lors de l'utilisation d'{{site.data.keyword.la_full_notm}}, publiez une question sur la chaîne `logdna-on-iks` dans [{{site.data.keyword.containerlong_notm}} Slack ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://ibm-container-service.slack.com/), ou ouvrez un [cas de support {{site.data.keyword.Bluemix_notm}}](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support). Connectez-vous à Slack en utilisant votre IBMid. Si vous n'utilisez pas un IBMid pour votre compte {{site.data.keyword.Bluemix_notm}}, [demandez une invitation à Slack ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://bxcs-slack-invite.mybluemix.net/).

## Sysdig
{: #sydig-partner}

{{site.data.keyword.mon_full_notm}} fournit [Sysdig Monitor ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://sysdig.com/products/monitor/) comme système d'analyse de conteneur tiers natif pour le cloud que vous pouvez utiliser pour mieux comprendre les performances et l'intégrité de vos hôtes, applications, conteneurs et réseaux de calcul.
{: shortdesc}

### Avantages
{: #sydig-benefits}

Vous trouverez dans le tableau suivant une liste des principaux avantages liés à l'utilisation de Sysdig :
{: shortdesc}

|Avantage|Description|
|-------------|------------------------------|
|Accès automatique aux métriques natives pour le cloud et personnalisées pour Prometheus|Faites votre choix parmi une grande variété de métriques natives pour le cloud prédéfinies et personnalisées pour Prometheus qui vous permettront de mieux comprendre les performances et l'intégrité de vos hôtes, applications, conteneurs et réseaux de calcul. |
|Traitement des incidents à l'aide de filtres avancés|Sysdig Monitor crée des topologies de réseau qui montrent comment vos noeuds worker sont connectés et comment vos services Kubernetes communiquent entre eux. Vous pouvez accéder à des conteneurs et des appels système uniques à partir de vos noeuds worker tout en regroupant et en visualisant des métriques importantes pour chaque ressource. Par exemple, utilisez ces métriques pour trouver des services qui reçoivent la plupart des demandes ou des services avec des requêtes et des temps de réponse qui sont lents. Vous pouvez combiner ces données avec des événements kubernetes, des événements CI/CD personnalisés, ou des validations de code. |Détection automatique d'anomalies et alertes personnalisées|Définissez des règles et des seuils pour être averti en cas d'anomalies détectées dans votre cluster ou vos groupes de ressources afin de permettre à Sysdig de vous prévenir lorsqu'une ressource agit différemment des autres ressources. Vous pouvez envoyer ces alertes à des outils en aval, tels que ServiceNow, PagerDuty, Slack, VictorOps, ou par courrier électronique. |
|Tableaux de bord prêts à l'emploi et personnalisés|Vous pouvez faire votre choix parmi une grande variété de tableaux de bord existants ou créer votre propre tableau de bord pour visualiser les métriques de vos microservices comme vous le souhaitez. |
{: caption="Avantages liés à l'utilisation de Sysdig Monitor" caption-side="top"}

### Intégration à {{site.data.keyword.containerlong_notm}}
{: #sysdig-integration}

Sysdig Monitor est fourni par {{site.data.keyword.mon_full_notm}}, un service de plateforme {{site.data.keyword.Bluemix_notm}} que vous pouvez utiliser avec votre cluster. {{site.data.keyword.mon_full_notm}} est exploité par Sysdig en partenariat avec IBM.
{: shortdesc}

Pour utiliser Sysdig Monitor dans votre cluster, vous devez mettre à disposition une instance d'{{site.data.keyword.mon_full_notm}} dans votre compte {{site.data.keyword.Bluemix_notm}} et configurer vos clusters Kubernetes en tant que source de métriques. Une fois le cluster configuré, les métriques sont automatiquement collectées et transmises à votre instance de service {{site.data.keyword.mon_full_notm}}. Vous pouvez utiliser le tableau de bord {{site.data.keyword.mon_full_notm}} pour accéder à vos métriques.    

Pour plus d'informations, voir [Analyse des métriques relatives à une application qui est déployée dans un cluster Kubernetes](/docs/services/Monitoring-with-Sysdig?topic=Sysdig-kubernetes_cluster).

### Facturation et assistance
{: #sysdig-billing-support}

Sysdig Monitor étant fourni par {{site.data.keyword.mon_full_notm}}, votre utilisation est incluse dans la facturation {{site.data.keyword.Bluemix_notm}} pour les services de plateforme. Pour obtenir des informations de tarification, passez en revue les plans disponibles dans le [catalogue {{site.data.keyword.Bluemix_notm}} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/observe/monitoring/create).

{{site.data.keyword.mon_full_notm}} est entièrement intégré dans le système de support {{site.data.keyword.Bluemix_notm}}. Si vous rencontrez un problème lors de l'utilisation d'{{site.data.keyword.mon_full_notm}}, publiez une question dans le canal `sysdig-monitoring` dans [{{site.data.keyword.containerlong_notm}} Slack ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://ibm-container-service.slack.com/), ou ouvrez un [cas de support {{site.data.keyword.Bluemix_notm}}](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support). Connectez-vous à Slack en utilisant votre IBMid. Si vous n'utilisez pas un IBMid pour votre compte {{site.data.keyword.Bluemix_notm}}, [demandez une invitation à Slack ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://bxcs-slack-invite.mybluemix.net/).

## Portworx
{: #portworx-parter}

[Portworx ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://portworx.com/products/introduction/) est une solution de stockage SDS à haute disponibilité que vous pouvez utiliser afin de gérer du stockage persistant local pour vos bases de données conteneurisées et d'autres applications avec état, ou afin de partager des données entre des pods sur plusieurs zones.
{: shortdesc}

**Qu'est-ce qu'une solution SDS (Software-Defined Storage) ?** </br>
Une solution de stockage défini par logiciel (SDS) abstrait les unités de stockage de différents types, tailles ou de fournisseurs différents, rattachées aux noeuds worker dans votre cluster. Des noeuds worker avec du stockage disponible sur des disques durs sont ajoutés sous forme de noeud à un cluster de stockage. Dans ce cluster, le stockage physique est virtualisé et présenté sous forme de pool de stockage virtuel à l'utilisateur. Le cluster de stockage est géré par le logiciel SDS. Si les données doivent être hébergées sur le cluster de stockage, le logiciel SDS détermine où les stocker pour obtenir une disponibilité optimale. Votre stockage virtuel est fourni avec un ensemble commun de fonctions et de services dont vous pouvez tirer parti sans tenir compte de l'architecture de stockage sous-jacente effective.

### Avantages
{: #portworx-benefits}

Vous trouverez dans le tableau suivant une liste des principaux avantages liés à l'utilisation de Portworx :
{: shortdesc}

|Avantage|Description|
|-------------|------------------------------|
|Stockage natif pour le cloud et gestion de données pour les applications avec état|Portworx regroupe le stockage local disponible qui est lié à vos noeuds worker et dont le type ou la taille peut varier, et crée une couche de stockage persistant unifiée pour les bases de données conteneurisées ou d'autres applications avec état que vous envisagez d'exécuter dans le cluster. En utilisant des réservations de volume persistant Kubernetes,  vous pouvez ajouter du stockage persistant local à vos applications afin de stocker des données. |
|Données hautement disponibles avec réplication de volume|Portworx réplique automatiquement des données dans vos volumes sur plusieurs noeuds worker et zones de votre cluster, ainsi, vos données sont accessibles en permanence et votre application avec état peut être replanifiée sur un autre noeud worker en cas de défaillance ou de réamorçage d'un noeud worker. |
|Prise en charge de l'exécution en mode hyperconvergé (`hyper-converged`)|Portworx peut être configuré pour s'exécuter en mode hyperconvergé ([`hyper-converged`) ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://docs.portworx.com/portworx-install-with-kubernetes/storage-operations/hyperconvergence/) pour garantir que vos ressources de calcul et le stockage résident toujours sur le même noeud worker. Lorsque votre application doit être replanifiée, Portworx la transfère sur un noeud worker où se trouve l'une de vos répliques de volume pour garantir la vitesse d'accès au disque local et les meilleures performances pour votre application avec état. |
|Chiffrement de données à l'aide d'{{site.data.keyword.keymanagementservicelong_notm}}|Vous pouvez [configurer des clés de chiffrement {{site.data.keyword.keymanagementservicelong_notm}}](/docs/containers?topic=containers-portworx#encrypt_volumes) qui sont sécurisées par des modules de sécurité matérielle de type cloud certifiés FIPS 140-2 niveau 2 pour protéger les données de vos volumes. Vous pouvez choisir entre l'utilisation d'une clé de chiffrement pour chiffrer tous vos volumes dans un cluster ou l'utilisation d'une clé de chiffrement pour chaque volume. Portworx utilise cette clé pour chiffrer les données au repos ou en transit lorsque les données sont envoyées à un autre noeud worker.|
|Images instantanées intégrées et sauvegardes de cloud|Vous pouvez sauvegarder l'état en cours d'un volume et ses données en créant une [image instantanée Portworx ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://docs.portworx.com/portworx-install-with-kubernetes/storage-operations/create-snapshots/). Les images instantanées peuvent être stockées sur votre cluster Portworx local ou dans le cloud. |
|Surveillance intégrée avec Lighthouse|[Lighthouse ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://docs.portworx.com/reference/lighthouse/) est un outil graphique intuitif qui vous aide à gérer et surveiller vos clusters et images instantanées de volume Portworx. Avec Lighthouse, vous pouvez afficher l'état de santé de votre cluster Portworx, notamment le nombre de noeuds de stockage et de volumes disponibles, la capacité disponible, et analyser vos données dans Prometheus, Grafana ou Kibana.|
{: caption="Avantages liés à l'utilisation de Portworx" caption-side="top"}

### Intégration à {{site.data.keyword.containerlong_notm}}
{: #portworx-integration}

{{site.data.keyword.containerlong_notm}} fournit des versions de noeud worker optimisées pour une utilisation avec SDS et qui sont accompagnées d'un ou plusieurs disques locaux RAW, non formatés et non montés, que vous pouvez utiliser pour stocker vos données. Portworx offre des performances optimales lorsque vous utilisez des [machines de noeud worker SDS](/docs/containers?topic=containers-planning_worker_nodes#sds) d'un débit réseau de 10 Gbit/s. Vous pouvez toutefois installer Portworx sur des versions non SDS de noeud worker, mais vous risquez de ne pas obtenir les performances requises par votre application.
{: shortdesc}

Portworx est installé en utilisant une [charte Helm](/docs/containers?topic=containers-portworx#install_portworx). Lorsque vous installez la charte Helm, Portworx analyse automatiquement le stockage persistant local qui est disponible dans votre cluster et ajoute le stockage à la couche de stockage Portworx. Pour ajouter du stockage à partir de votre couche de stockage Portworx à vos applications, vous devez utiliser des [réservations de volume persistant Kubernetes](/docs/containers?topic=containers-portworx#add_portworx_storage).

Pour installer Portworx dans votre cluster, vous devez disposer d'une licence Portworx. Si vous novice, vous pouvez utiliser l'édition `px-enterprise` comme version d'évaluation. Cette version d'évaluation vous offre les fonctionnalités complètes de Portworx que vous pouvez tester pendant 30 jours. Lorsque la version d'évaluation a expiré, vous devez [acheter une licence Portworx ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](/docs/containers?topic=containers-portworx#portworx_license) pour continuer à utiliser votre cluster Portworx. 

Pour plus d'informations sur l'installation et l'utilisation de Portworx avec {{site.data.keyword.containerlong_notm}}, voir [Stockage de données sur SDS (Software-Defined Storage) avec Portworx](/docs/containers?topic=containers-portworx).

### Facturation et assistance
{: #portworx-billing-support}

Les machines de noeud worker SDS qui sont fournies avec des disques locaux, et les machines virtuelles que vous utilisez pour Portworx sont incluses dans votre facture {{site.data.keyword.containerlong_notm}} mensuelle. Pour obtenir des informations de tarification, consultez le [catalogue {{site.data.keyword.Bluemix_notm}}![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/kubernetes/catalog/cluster). La licence Portworx est un coût distinct et n'est pas incluse dans votre facture mensuelle.
{: shortdesc}

Si vous rencontrez un problème lors de l'utilisation de Portworx ou si vous voulez discuter des configurations de Portworx pour votre cas particulier, publiez une question sur la chaîne `portworx-on-iks` dans [{{site.data.keyword.containerlong_notm}} Slack ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://ibm-container-service.slack.com/). Connectez-vous à Slack en utilisant votre IBMid. Si vous n'utilisez pas un IBMid pour votre compte {{site.data.keyword.Bluemix_notm}}, [demandez une invitation à Slack ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://bxcs-slack-invite.mybluemix.net/).
