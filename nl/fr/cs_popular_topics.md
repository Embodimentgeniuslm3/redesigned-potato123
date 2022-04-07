---

copyright:
  years: 2014, 2019
lastupdated: "2019-04-15"

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



# Rubriques les plus consultées pour {{site.data.keyword.containerlong_notm}}
{: #cs_popular_topics}

Tenez-vous au courant de ce qui se passe dans {{site.data.keyword.containerlong}}. Découvrez les nouvelles fonctions à explorer, une astuce pour les expérimenter ou quelques rubriques populaires que d'autres développeurs jugent désormais utiles.
{:shortdesc}



## Rubriques les plus consultées en avril 2019
{: #apr19}

<table summary="Ce tableau présente les rubriques les plus consultées. La lecture des lignes s'effectue de gauche à droite, avec la date indiquée dans la première colonne, le titre de la fonction dans la deuxième colonne et la description dans la troisième colonne.">
<caption>Rubriques les plus consultées pour les conteneurs et les clusters Kubernetes en avril 2019</caption>
<thead>
<th>Date</th>
<th>Titre</th>
<th>Description</th>
</thead>
<tbody>
<tr>
<td>15 avril 2019</td>
<td>[Enregistrement d'un nom d'hôte d'équilibreur de charge de réseau (NLB)](/docs/containers?topic=containers-loadbalancer#loadbalancer_hostname)</td>
<td>Après avoir configuré des équilibreurs de charge de réseau publics (NLB) afin d'exposer vos applications sur Internet, vous pouvez à présent créer des entrées DNS pour les adresses IP NLB en créant des noms d'hôte. {{site.data.keyword.Bluemix_notm}} se charge de générer et gérer le certificat SSL d'assistant pour le nom d'hôte. Vous pouvez également configurer des moniteurs TCP/HTTP(S) afin d'effectuer un diagnostic d'intégrité des adresses IP NLB derrière chaque nom d'hôte.</td>
</tr>
<tr>
<td>8 avril 2019</td>
<td>[Terminal Kubernetes dans votre navigateur Web (bêta)](/docs/containers?topic=containers-cs_cli_install#cli_web)</td>
<td>Si vous utilisez le tableau de bord de cluster dans la console {{site.data.keyword.Bluemix_notm}} pour gérer vos clusters mais que vous souhaitez effectuer rapidement des modifications de configuration plus évoluées, vous pouvez désormais exécuter des commandes CLI directement à partir de votre navigateur Web dans le terminal Kubernetes. Sur la page de détails d'un cluster, lancez le terminal Kubernetes en cliquant sur le bouton **Terminal**. Notez que le terminal Kubernetes est publié en tant que module complémentaire bêta et qu'il n'est pas prévu de l'utiliser dans des clusters de production. </td>
</tr>
<tr>
<td>4 avril 2019</td>
<td>[Maîtres à haute disponibilité désormais présents à Sydney](/docs/containers?topic=containers-regions-and-zones#zones)</td>
<td>Lorsque vous [créez un cluster](/docs/containers?topic=containers-clusters#clusters_ui) dans une agglomération à zones multiples, y compris Sydney, les répliques de votre maître Kubernetes sont réparties entre les différentes zones à des fins de haute disponibilité. </td>
</tr>
</tbody></table>

## Rubriques les plus consultées en mars 2019
{: #mar19}

<table summary="Ce tableau présente les rubriques les plus consultées. La lecture des lignes s'effectue de gauche à droite, avec la date indiquée dans la première colonne, le titre de la fonction dans la deuxième colonne et la description dans la troisième colonne.">
<caption>Rubriques les plus consultées pour les conteneurs et les clusters Kubernetes en mars 2019</caption>
<thead>
<th>Date</th>
<th>Titre</th>
<th>Description</th>
</thead>
<tbody>
<tr>
<td>21 mars 2019</td>
<td>Introduction des noeuds finaux de service privés pour votre maître cluster Kubernetes</td>
<td>Par défaut, {{site.data.keyword.containerlong_notm}} configure votre cluster avec accès sur un VLAN public et un VLAN privé. Auparavant, si vous souhaitiez disposer d'un [cluster avec VLAN privé uniquement](/docs/containers?topic=containers-plan_clusters#private_clusters), il vous fallait configurer un dispositif de passerelle pour connecter les noeuds worker du cluster au maître. Vous pouvez désormais utiliser le noeud final de service privé. Lorsque le noeud final de service privé est activé, tout le trafic entre les noeuds worker et le maître s'effectue sur le réseau privé, sans nécessiter de dispositif de passerelle. En plus de cette sécurité renforcée, le trafic entrant et sortant sur le réseau privé est [illimité et gratuit ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/cloud/bandwidth). Vous pouvez toujours garder un noeud final de service public pour accéder à votre maître Kubernetes sur Internet, par exemple pour exécuter des commandes `kubectl` sans être sur le réseau privé.<br><br>
Pour utiliser des noeuds finaux de service privés, vous devez activer la fonction [VRF](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) et les [noeuds finaux de service](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started) pour votre compte d'infrastructure IBM Cloud (SoftLayer). Votre cluster doit exécuter Kubernetes version 1.11 ou ultérieure. S'il exécute une version plus ancienne de Kubernetes, [mettez-le à jour pour passer au moins à la version 1.11](/docs/containers?topic=containers-update#update). Pour plus d'informations, cliquez sur les liens suivants :<ul>
<li>[Description de la communication entre le maître et les noeuds worker avec des noeuds finaux de service](/docs/containers?topic=containers-cs_network_ov#cs_network_ov_master)</li>
<li>[Configuration du noeud final de service privé](/docs/containers?topic=containers-cs_network_cluster#set-up-private-se)</li>
<li>[Passage d'un noeud final de service public à un noeud final de service privé](/docs/containers?topic=containers-cs_network_cluster#migrate-to-private-se)</li>
<li>Si vous disposez d'un pare-feu sur le réseau privé : [Ajout d'adresses IP privées pour {{site.data.keyword.containerlong_notm}}, {{site.data.keyword.registrylong_notm}} et d'autres services {{site.data.keyword.Bluemix_notm}}](/docs/containers?topic=containers-firewall#firewall_outbound)</li>
</ul>
<p class="important">Si vous passez à un cluster avec noeud final de service privé uniquement, assurez-vous que votre cluster peut toujours communiquer avec d'autres services {{site.data.keyword.Bluemix_notm}} que vous utilisez. Le [stockage SDS Portworx](/docs/containers?topic=containers-portworx#portworx) et le  [programme de mise à l'échelle automatique de cluster](/docs/containers?topic=containers-ca#ca) ne prennent pas en charge le noeud final de service privé uniquement. Utilisez un cluster disposant à la fois de noeuds finaux de service public et privé. Le [stockage de fichiers basé sur NFS](/docs/containers?topic=containers-file_storage#file_storage) est pris en charge si votre cluster exécute Kubernetes version 1.13.4_1513, 1.12.6_1544, 1.11.8_1550, 1.10.13_1551 ou plus. </p>
</td>
</tr>
<tr>
<td>7 mars 2019</td>
<td>[Le programme de mise à l'échelle automatique de cluster (cluster autoscaler) passe de la version bêta à GA](/docs/containers?topic=containers-ca#ca)</td>
<td>Le programme de mise à l'échelle automatique de cluster (cluster autoscaler) est désormais disponible. Installez le plug-in Helm et commencez à mettre à l'échelle les pools de noeuds worker de votre cluster automatiquement pour augmenter ou réduire le nombre de noeuds worker selon la taille requise pour vos charges de travail planifiées.<br><br>
Besoin d'aide ou commentaires sur le programme de mise à l'échelle automatique de cluster ? Si vous êtes un utilisateur externe, [enregistrez-vous sur le site Slack public ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://bxcs-slack-invite.mybluemix.net/) et publiez vos demandes sur le canal [#cluster-autoscaler ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://ibm-container-service.slack.com/messages/CF6APMLBB). Si vous êtes IBMer, publiez-les sur le canal [Slack interne ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://ibm-argonauts.slack.com/messages/C90D3KZUL).</td>
</tr>
</tbody></table>




## Rubriques les plus consultées en février 2019
{: #feb19}

<table summary="Ce tableau présente les rubriques les plus consultées. La lecture des lignes s'effectue de gauche à droite, avec la date indiquée dans la première colonne, le titre de la fonction dans la deuxième colonne et la description dans la troisième colonne.">
<caption>Rubriques les plus consultées pour les conteneurs et les clusters Kubernetes en février 2019</caption>
<thead>
<th>Date</th>
<th>Titre</th>
<th>Description</th>
</thead>
<tbody>
<tr>
<td>25 février</td>
<td>Contrôle granulaire accru pour l'extraction d'images d'{{site.data.keyword.registrylong_notm}}</td>
<td>Lorsque vous déployez vos charges de travail dans des clusters {{site.data.keyword.containerlong_notm}}, vos conteneurs peuvent désormais extraire des images à partir des [nouveaux noms de domaine `icr.io` pour {{site.data.keyword.registrylong_notm}}](/docs/services/Registry?topic=registry-registry_overview#registry_regions). Par ailleurs, vous pouvez utiliser des règles d'accès à granularité fine dans {{site.data.keyword.Bluemix_notm}} IAM pour contrôler l'accès à vos images. Pour plus d'informations, voir [Comment votre cluster est-il autorisé à extraire des images](/docs/containers?topic=containers-images#cluster_registry_auth).</td>
</tr>
<tr>
<td>21 février</td>
<td>[Zone disponible à Mexico](/docs/containers?topic=containers-regions-and-zones#regions-and-zones)</td>
<td>Bienvenue à Mexico (`mex01`), nouvelle zone pour les clusters de la région Sud des Etats-Unis. Si vous disposez d'un pare-feu, veillez à [ouvrir les ports de pare-feu](/docs/containers?topic=containers-firewall#firewall_outbound) correspondant à cette zone et aux autres zones situées dans la région où se trouve votre cluster.</td>
</tr>
<tr><td>6 février 2019</td>
<td>[Istio sur {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-istio)</td>
<td>Istio sur {{site.data.keyword.containerlong_notm}} offre une installation transparente d'Istio, des mises à jour automatiques et la gestion du cycle de vie des composants du plan de contrôle Istio, ainsi que l'intégration aux outils de surveillance et de consignation de la plateforme. En un seul clic, vous pouvez obtenir tous les composants de base d'Istio, des fonctions de suivi, de surveillance et de visualisation supplémentaires, ainsi que le modèle opérationnel d'application BookInfo. Istio sur {{site.data.keyword.containerlong_notm}} est proposé en tant que module complémentaire géré, de sorte qu'{{site.data.keyword.Bluemix_notm}} puisse conserver automatiquement tous vos composants Istio à jour.</td>
</tr>
<tr>
<td>6 février 2019</td>
<td>[Knative sur {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-knative_tutorial)</td>
<td>Knative est une plateforme open source qui étend les capacités de Kubernetes pour vous aider à créer des applications modernes, conteneurisées, axées sur la source et sans serveur, en plus de votre cluster Kubernetes. Managed Knative on {{site.data.keyword.containerlong_notm}} est un module complémentaire géré qui intègre Knative et Istio directement avec votre cluster Kubernetes. Les versions de Knative et d'Istio dans le module complémentaire sont testées par IBM et prises en charge pour être utilisées dans {{site.data.keyword.containerlong_notm}}.</td>
</tr>
</tbody></table>


## Rubriques les plus consultées en janvier 2019
{: #jan19}

<table summary="Ce tableau présente les rubriques les plus consultées. La lecture des lignes s'effectue de gauche à droite, avec la date indiquée dans la première colonne, le titre de la fonction dans la deuxième colonne et la description dans la troisième colonne.">
<caption>Rubriques les plus consultées pour les conteneurs et les clusters Kubernetes en janvier 2019</caption>
<thead>
<th>Date</th>
<th>Titre</th>
<th>Description</th>
</thead>
<tbody>
<tr>
<td>30 janvier</td>
<td>Rôles d'accès au service {{site.data.keyword.Bluemix_notm}} IAM et espaces de nom Kubernetes</td>
<td>{{site.data.keyword.containerlong_notm}} prend désormais en charge les [rôles d'accès au service](/docs/containers?topic=containers-access_reference#service) {{site.data.keyword.Bluemix_notm}} IAM. Ces rôles d'accès au service s'alignent avec les contrôles d'accès [Kubernetes RBAC](/docs/containers?topic=containers-users#role-binding) pour autoriser les utilisateurs à exécuter des actions `kubectl` avec le cluster pour gérer des ressources Kubernetes, telles que les pods et les déploiements. De plus, vous pouvez [limiter l'accès utilisateur à un espace de nom Kubernetes spécifique](/docs/containers?topic=containers-users#platform) dans le cluster en utilisant des rôles d'accès au service {{site.data.keyword.Bluemix_notm}} IAM. Les [rôles d'accès à une plateforme](/docs/containers?topic=containers-access_reference#iam_platform) sont désormais utilisés pour autoriser les utilisateurs à effectuer des actions `ibmcloud ks` pour gérer l'infrastructure de votre cluster, par exemple les noeuds worker.<br><br>Les rôles d'accès au service {{site.data.keyword.Bluemix_notm}} IAM sont automatiquement ajoutés aux comptes {{site.data.keyword.containerlong_notm}} et aux clusters existants en fonction des droits dont disposaient déjà vos utilisateurs avec les rôles d'accès à une plateforme IAM. Dorénavant, vous pouvez utiliser IAM pour créer des groupes d'accès, ajouter des utilisateurs aux groupes d'accès et affecter des rôles d'accès à une plateforme ou un service à ces groupes. Pour plus d'informations, consultez le blogue [Introducing service roles and namespaces in IAM for more granular control of cluster access ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/blogs/bluemix/2019/02/introducing-service-roles-and-namespaces-in-iam-for-more-granular-control-of-cluster-access/).</td>
</tr>
<tr>
<td>8 janvier</td>
<td>[Aperçu du programme de mise à l'échelle automatique de cluster (Cluster autoscaler) version bêta](/docs/containers?topic=containers-ca#ca)</td>
<td>Effectuez la mise à l'échelle automatique des pools de noeuds worker dans votre cluster pour augmenter ou réduire le nombre de noeuds worker dans ces pools en fonction de la taille requise par vos charges de travail planifiées. Pour tester cette version bêta, vous devez en demander l'accès sur liste blanche.</td>
</tr>
<tr>
<td>8 janvier</td>
<td>Outil de débogage et de diagnostic d'[{{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-cs_troubleshoot#debug_utility)</td>
<td>Vous avez parfois du mal à obtenir tous les fichiers YAML et d'autres informations dont vous avez besoin pour identifier et résoudre un problème ? Essayez l'outil de débogage et de diagnostic d'{{site.data.keyword.containerlong_notm}} pour bénéficier d'une interface graphique qui vous aidera à regrouper les informations liées au cluster, au déploiement et au réseau.</td>
</tr>
</tbody></table>

## Rubriques les plus consultées en décembre 2018
{: #dec18}

<table summary="Ce tableau présente les rubriques les plus consultées. La lecture des lignes s'effectue de gauche à droite, avec la date indiquée dans la première colonne, le titre de la fonction dans la deuxième colonne et la description dans la troisième colonne.">
<caption>Rubriques les plus consultées pour les conteneurs et les clusters Kubernetes en décembre 2018</caption>
<thead>
<th>Date</th>
<th>Titre</th>
<th>Description</th>
</thead>
<tbody>
<tr>
<td>11 décembre</td>
<td>Prise en charge de SDS avec Portworx</td>
<td>Gérez du stockage persistant pour vos bases de données conteneurisées et d'autres applications avec état, ou pour partager des données entre des pods sur plusieurs zones avec Portworx. [Portworx ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://portworx.com/products/introduction/) est une solution de stockage défini par logiciel (SDS) qui gère le stockage par blocs local de vos noeuds worker pour créer une couche de stockage persistant unifiée pour vos applications. En utilisant la réplication de volume de chaque volume au niveau conteneur sur plusieurs noeuds worker, Portworx assure la persistance et l'accessibilité des données sur plusieurs zones. Pour plus d'informations, voir [Stockage de données sur SDS (Software-Defined Storage) avec Portworx](/docs/containers?topic=containers-portworx#portworx).    </td>
</tr>
<tr>
<td>6 décembre</td>
<td>{{site.data.keyword.mon_full_notm}}</td>
<td>Gagnez en visibilité opérationnelle sur les performances et l'état de santé de vos applications en déployant Sysdig en tant que service tiers sur vos noeuds worker pour transférer des métriques à {{site.data.keyword.monitoringlong}}. Pour plus d'informations, voir [Analyzing metrics for an app that is deployed in a Kubernetes cluster](/docs/services/Monitoring-with-Sysdig/tutorials?topic=Sysdig-kubernetes_cluster#kubernetes_cluster). **Remarque** : si vous utilisez {{site.data.keyword.mon_full_notm}} avec des clusters exécutant Kubernetes version 1.11 ou ultérieure, toutes les métriques ne sont pas collectées car Sysdig ne prend pas en charge `containerd` actuellement.</td>
</tr>
<tr>
<td>4 décembre</td>
<td>[Réserves de ressources de noeud worker](/docs/containers?topic=containers-plan_clusters#resource_limit_node)</td>
<td>Vous déployez tellement d'applications que vous craignez de dépasser la capacité de votre cluster ? Les réserves de ressources de noeud worker et les fonctions d'expulsion de Kubernetes protège la capacité de traitement de votre cluster pour qu'il dispose de suffisamment de ressources pour rester opérationnel. Il vous suffit d'ajouter quelques noeuds worker et vos pods relancent automatiquement leur planification. Les réserves de ressources de noeud worker sont mises à jour dans les dernières [modifications des correctifs de version](/docs/containers?topic=containers-changelog#changelog).</td>
</tr>
</tbody></table>

## Rubriques les plus consultées en novembre 2018
{: #nov18}

<table summary="Ce tableau présente les rubriques les plus consultées. La lecture des lignes s'effectue de gauche à droite, avec la date indiquée dans la première colonne, le titre de la fonction dans la deuxième colonne et la description dans la troisième colonne.">
<caption>Rubriques les plus consultées pour les conteneurs et les clusters Kubernetes en novembre 2018</caption>
<thead>
<th>Date</th>
<th>Titre</th>
<th>Description</th>
</thead>
<tbody>
<tr>
<td>29 novembre</td>
<td>[Zone disponible dans Chennai](/docs/containers?topic=containers-regions-and-zones)</td>
<td>Bienvenue à Chennai en Inde, une nouvelle zone pour les clusters dans la zone Asie-Pacifique nord. Si vous disposez d'un pare-feu, veillez à [ouvrir les ports de pare-feu](/docs/containers?topic=containers-firewall#firewall) correspondant à cette zone et aux autres zones situées dans la région où se trouve votre cluster.</td>
</tr>
<tr>
<td>27 novembre</td>
<td>{{site.data.keyword.la_full_notm}}</td>
<td>Ajoutez des fonctionnalités de gestion de journaux à votre cluster en déployant LogDNA en tant que service tiers sur vos noeuds worker afin de gérer les journaux à partir de vos conteneurs de pod. Pour plus d'informations, voir [Managing Kubernetes cluster logs with {{site.data.keyword.loganalysisfull_notm}} with LogDNA](/docs/services/Log-Analysis-with-LogDNA/tutorials?topic=LogDNA-kube#kube).</td>
</tr>
<tr>
<td>7 novembre</td>
<td>Equilibreur de charge de réseau 2.0 (bêta)</td>
<td>Vous pouvez désormais choisir entre un [NLB 1.0 ou 2.0](/docs/containers?topic=containers-loadbalancer#planning_ipvs) pour exposer vos applications de cluster au public de manière sécurisée.</td>
</tr>
<tr>
<td>7 novembre</td>
<td>La version Kubernetes 1.12 est disponible</td>
<td>Désormais, vous pouvez mettre à jour ou créer des clusters exécutant [Kubernetes version 1.12](/docs/containers?topic=containers-cs_versions#cs_v112). Par défaut, les clusters 1.12 sont fournis avec des maîtres Kubernetes à haute disponibilité.</td>
</tr>
<tr>
<td>7 novembre</td>
<td>Maîtres à haute disponibilité</td>
<td>Des maîtres à haute disponibilité sont disponibles pour les clusters qui exécutent Kubernetes version 1.10. Tous les avantages décrits dans l'entrée précédente pour les clusters 1.11 s'appliquent aux clusters 1.10, ainsi que les [étapes de préparation](/docs/containers?topic=containers-cs_versions#110_ha-masters) que vous devez suivre.</td>
</tr>
<tr>
<td>1er novembre</td>
<td>Maîtres à haute disponibilité dans les clusters exécutant Kubernetes version 1.11</td>
<td>Dans une zone unique, votre maître est hautement disponible et comprend des répliques sur des hôtes physiques distincts pour le serveur d'API Kubernetes, le composant etcd, le planificateur et le gestionnaire de contrôleurs afin de les protéger en cas d'indisponibilité due par exemple à une mise à jour de cluster. Si votre cluster se trouve dans une zone compatible avec plusieurs zones, votre maître à haute disponibilité est également réparti sur plusieurs zones pour le protéger en cas de défaillance d'une zone.<br>Pour connaître les actions que vous devez effectuer, voir [Mise à jour des maîtres cluster pour la haute disponibilité](/docs/containers?topic=containers-cs_versions#ha-masters). Ces actions de préparation sont applicables dans les cas suivants :<ul>
<li>Si vous disposez d'un pare-feu ou de règles réseau Calico personnalisées.</li>
<li>Si vous utilisez les ports d'hôte `2040` ou `2041` sur vos noeuds worker.</li>
<li>Si vous avez utilisé l'adresse IP du noeud du maître cluster pour accéder au maître depuis le cluster.</li>
<li>Si vous disposez d'un processus automatique pour appeler l'API ou l'interface de ligne de commande Calico (`calicoctl`), par exemple pour créer des règles Calico.</li>
<li>Si vous utilisez des règles réseau Kubernetes ou Calico pour contrôler l'accès du trafic sortant du pod vers le maître.</li></ul></td>
</tr>
</tbody></table>

## Rubriques les plus consultées en octobre 2018
{: #oct18}

<table summary="Ce tableau présente les rubriques les plus consultées. La lecture des lignes s'effectue de gauche à droite, avec la date indiquée dans la première colonne, le titre de la fonction dans la deuxième colonne et la description dans la troisième colonne.">
<caption>Rubriques les plus consultées pour les conteneurs et les clusters Kubernetes en octobre 2018</caption>
<thead>
<th>Date</th>
<th>Titre</th>
<th>Description</th>
</thead>
<tbody>
<tr>
<td>25 octobre</td>
<td>[Zone disponible à Milan](/docs/containers?topic=containers-regions-and-zones)</td>
<td>Bienvenue à Milan en Italie, une nouvelle zone pour les clusters payants dans la région Europe centrale. Auparavant, Milan n'était disponible que pour les clusters gratuits. Si vous disposez d'un pare-feu, veillez à [ouvrir les ports de pare-feu](/docs/containers?topic=containers-firewall#firewall) correspondant à cette zone et aux autres zones situées dans la région où se trouve votre cluster.</td>
</tr>
<tr>
<td>22 octobre</td>
<td>[Nouvel emplacement à zones multiples pour Londres, `lon05`](/docs/containers?topic=containers-regions-and-zones#zones)</td>
<td>L'agglomération de Londres à zones multiples remplace la zone `lon02` par la nouvelle zone `lon05` qui comporte plus de ressources d'infrastructure que `lon02`. Créez des clusters à zones multiples dans `lon05`. Les clusters existants de la zone `lon02` sont pris en charge mais les nouveaux clusters à zones multiples doivent dorénavant utiliser `lon05`.</td>
</tr>
<tr>
<td>5 octobre</td>
<td>Intégration à {{site.data.keyword.keymanagementservicefull}} (bêta)</td>
<td>Vous pouvez chiffrer les valeurs confidentielles (secrets) Kubernetes dans votre cluster en [activant {{site.data.keyword.keymanagementserviceshort}} (bêta)](/docs/containers?topic=containers-encryption#keyprotect).</td>
</tr>
<tr>
<td>4 octobre</td>
<td>[{{site.data.keyword.registrylong}} est désormais intégré à {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM)](/docs/services/Registry?topic=registry-iam#iam)</td>
<td>Vous pouvez utiliser {{site.data.keyword.Bluemix_notm}} IAM pour contrôler l'accès aux ressources de votre registre, par exemple pour extraire, envoyer et générer des images. Lorsque vous créez un cluster, vous créez également un jeton de registre pour que le cluster puisse utiliser votre registre. Par conséquent, vous avez besoin du rôle de gestion de plateforme **Administrateur** du registre au niveau du compte pour créer un cluster. Pour activer {{site.data.keyword.Bluemix_notm}} IAM pour le compte de votre registre, voir la rubrique sur l'[activation de l'application des règles aux utilisateurs existants](/docs/services/Registry?topic=registry-user#existing_users).</td>
</tr>
<tr>
<td>1er octobre</td>
<td>[Groupes de ressources](/docs/containers?topic=containers-users#resource_groups)</td>
<td>Vous pouvez utiliser des groupes de ressources pour séparer vos ressources {{site.data.keyword.Bluemix_notm}} en pipelines, services ou autres regroupements pour faciliter l'affectation d'accès et les mesures du niveau d'utilisation. Désormais, {{site.data.keyword.containerlong_notm}} prend en charge la création de clusters dans le groupe `default` ou tout autre groupe de ressources que vous créez.</td>
</tr>
</tbody></table>

## Rubriques les plus consultées en septembre 2018
{: #sept18}

<table summary="Ce tableau présente les rubriques les plus consultées. La lecture des lignes s'effectue de gauche à droite, avec la date indiquée dans la première colonne, le titre de la fonction dans la deuxième colonne et la description dans la troisième colonne.">
<caption>Rubriques les plus consultées pour les conteneurs et les clusters Kubernetes en septembre 2018</caption>
<thead>
<th>Date</th>
<th>Titre</th>
<th>Description</th>
</thead>
<tbody>
<tr>
<td>25 septembre</td>
<td>[Nouvelles zones disponibles](/docs/containers?topic=containers-regions-and-zones)</td>
<td>Vous disposez désormais d'autres options pour les zones de déploiement de vos applications.
<ul><li>Bienvenue à San José, avec deux nouvelles zones dans la région Sud des Etats-Unis, `sjc03` et `sjc04`. Si vous disposez d'un pare-feu, veillez à [ouvrir les ports de pare-feu](/docs/containers?topic=containers-firewall#firewall) correspondant à cette zone et aux autres zones situées dans la région où se trouve votre cluster.</li>
<li>Avec deux nouvelles zones `tok04` et `tok05`, vous pouvez désormais [créer des clusters à zones multiples](/docs/containers?topic=containers-plan_clusters#multizone) à Tokyo dans la région Asie-Pacifique nord.</li></ul></td>
</tr>
<tr>
<td>5 septembre</td>
<td>[Zone disponible à Oslo](/docs/containers?topic=containers-regions-and-zones)</td>
<td>Bienvenue à Oslo en Norvège, une nouvelle zone dans la région Europe centrale. Si vous disposez d'un pare-feu, veillez à [ouvrir les ports de pare-feu](/docs/containers?topic=containers-firewall#firewall) correspondant à cette zone et aux autres zones situées dans la région où se trouve votre cluster.</td>
</tr>
</tbody></table>

## Rubriques les plus consultées en août 2018
{: #aug18}

<table summary="Ce tableau présente les rubriques les plus consultées. La lecture des lignes s'effectue de gauche à droite, avec la date indiquée dans la première colonne, le titre de la fonction dans la deuxième colonne et la description dans la troisième colonne.">
<caption>Rubriques les plus consultées pour les conteneurs et les clusters Kubernetes en août 2018</caption>
<thead>
<th>Date</th>
<th>Titre</th>
<th>Description</th>
</thead>
<tbody>
<tr>
<td>31 août</td>
<td>{{site.data.keyword.cos_full_notm}} est désormais intégré avec {{site.data.keyword.containerlong}}</td>
<td>Utilisez des réservations de volume persistant (PVC) Kubernetes natives pour mettre à disposition {{site.data.keyword.cos_full_notm}} dans votre cluster. {{site.data.keyword.cos_full_notm}} convient le mieux aux charges de travail à lecture intensive et pour le stockage de données sur plusieurs zones dans un cluster à zones multiples. Commencez par [créer une instance de service {{site.data.keyword.cos_full_notm}}](/docs/containers?topic=containers-object_storage#create_cos_service) et [installer le plug-in {{site.data.keyword.cos_full_notm}}](/docs/containers?topic=containers-object_storage#install_cos) dans votre cluster. </br></br>Vous n'êtes pas certain de la solution de stockage qui vous convient le mieux ? Commencez [ici](/docs/containers?topic=containers-storage_planning#choose_storage_solution) pour analyser vos données et choisir la solution de stockage appropriée pour vos données. </td>
</tr>
<tr>
<td>14 août</td>
<td>Mettez à jour vos cluster avec Kubernetes version 1.11 pour affecter des priorités aux pods</td>
<td>Après avoir mis à jour votre cluster vers [Kubernetes version 1.11](/docs/containers?topic=containers-cs_versions#cs_v111), vous pouvez tirer parti des nouvelles fonctions, telles que des performances d'environnement d'exécution de conteneur accrues avec `containerd` ou l'[affectation de priorité aux pods](/docs/containers?topic=containers-pod_priority#pod_priority).</td>
</tr>
</tbody></table>

## Rubriques les plus consultées en juillet 2018
{: #july18}

<table summary="Ce tableau présente les rubriques les plus consultées. La lecture des lignes s'effectue de gauche à droite, avec la date indiquée dans la première colonne, le titre de la fonction dans la deuxième colonne et la description dans la troisième colonne.">
<caption>Rubriques les plus consultées pour les conteneurs et les clusters Kubernetes en juillet 2018</caption>
<thead>
<th>Date</th>
<th>Titre</th>
<th>Description</th>
</thead>
<tbody>
<tr>
<td>30 juillet</td>
<td>[Fournissez votre propre contrôleur Ingress](/docs/containers?topic=containers-ingress#user_managed)</td>
<td>Vous avez des exigences spécifiques en matière de sécurité ou d'autres exigences particulières pour le contrôleur Ingress de votre cluster ? Dans ce cas, vous pouvez envisager d'exécuter votre propre contrôleur Ingress au lieu du contrôleur par défaut.</td>
</tr>
<tr>
<td>10 juillet</td>
<td>Introduction des clusters à zones multiples</td>
<td>Vous voulez améliorer la disponibilité de votre cluster ? Vous pouvez désormais étendre votre cluster dans plusieurs zones de certaines agglomérations. Pour plus d'informations, voir [Création de clusters à zones multiples dans {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-plan_clusters#multizone).</td>
</tr>
</tbody></table>

## Rubriques les plus consultées en juin 2018
{: #june18}

<table summary="Ce tableau présente les rubriques les plus consultées. La lecture des lignes s'effectue de gauche à droite, avec la date indiquée dans la première colonne, le titre de la fonction dans la deuxième colonne et la description dans la troisième colonne.">
<caption>Rubriques les plus consultées pour les conteneurs et les clusters Kubernetes en juin 2018</caption>
<thead>
<th>Date</th>
<th>Titre</th>
<th>Description</th>
</thead>
<tbody>
<tr>
<td>13 juin</td>
<td>Le nom de la commande `bx` de l'interface de ligne de commande a été remplacé par `ic`</td>
<td>Lorsque vous téléchargez la version la plus récente de l'interface de ligne de commande (CLI) d'{{site.data.keyword.Bluemix_notm}}, vous exécutez désormais les commandes en utilisant le préfixe `ic` au lieu de `bx`. Par exemple, pour afficher la liste de vos clusters, vous exécutez la commande `ibmcloud ks clusters`.</td>
</tr>
<tr>
<td>12 juin</td>
<td>[Politiques de sécurité de pod](/docs/containers?topic=containers-psp)</td>
<td>Pour les clusters exécutant Kubernetes version 1.10.3 ou ultérieure, vous pouvez
configurer des politiques de sécurité de pod pour autoriser certaines personnes à créer et mettre à jour des pods dans {{site.data.keyword.containerlong_notm}}.</td>
</tr>
<tr>
<td>6 juin</td>
<td>[Prise en charge de TLS pour le sous-domaine générique Ingress fourni par IBM](/docs/containers?topic=containers-ingress#wildcard_tls)</td>
<td>Pour les clusters créés à partir du 6 juin 2018, le certificat TLS du sous-domaine Ingress fourni par IBM est désormais un certificat générique pouvant être utilisé pour le sous-domaine générique enregistré. Pour les clusters créés avant le 6 juin 2018, le certificat TLS sera mis à jour et remplacé par un certificat générique lors du renouvellement du certificat TLS utilisé actuellement.</td>
</tr>
</tbody></table>

## Rubriques les plus consultées en mai 2018
{: #may18}


<table summary="Ce tableau présente les rubriques les plus consultées. La lecture des lignes s'effectue de gauche à droite, avec la date indiquée dans la première colonne, le titre de la fonction dans la deuxième colonne et la description dans la troisième colonne.">
<caption>Rubriques les plus consultées pour les conteneurs et les clusters Kubernetes en mai 2018</caption>
<thead>
<th>Date</th>
<th>Titre</th>
<th>Description</th>
</thead>
<tbody>
<tr>
<td>24 mai</td>
<td>[Nouveau format de sous-domaine Ingress](/docs/containers?topic=containers-ingress)</td>
<td>Les clusters créés après le 24 mai ont un domaine Ingress qui leur est affecté dans un nouveau format : <code>&lt;cluster_name&gt;.&lt;region&gt;.containers.appdomain.cloud</code>. Lorsque vous utilisez Ingress pour exposer vos applications, vous pouvez recourir au nouveau sous-domaine pour accéder à vos applications sur Internet.</td>
</tr>
<tr>
<td>14 mai</td>
<td>[Mise à jour : déployez vos charges de travail sur des unités bare metal GPU dans le monde entier](/docs/containers?topic=containers-app#gpu_app)</td>
<td>Si vous disposez d'un [type de machine d'unité de traitement graphique (GPU)](/docs/containers?topic=containers-plan_clusters#shared_dedicated_node) dans votre cluster, vous pouvez planifier des applications nécessitant de nombreux calculs mathématiques. Le noeud worker GPU peut traiter la charge de travail de votre application à la fois sur l'unité centrale (UC) et le processeur graphique (GPU) pour améliorer les performances.</td>
</tr>
<tr>
<td>3 mai</td>
<td>[Container Image Security Enforcement (bêta)](/docs/services/Registry?topic=registry-security_enforce#security_enforce)</td>
<td>Besoin d'aide supplémentaire pour votre équipe pour savoir quelle image extraire dans vos conteneurs d'application ? Essayez la version bêta de Container Image Security Enforcement pour vérifier les images de conteneur avant de les déployer. Disponible pour les clusters exécutant Kubernetes 1.9 ou version ultérieure.</td>
</tr>
<tr>
<td>1er mai</td>
<td>[Déploiement du tableau de bord Kubernetes à partir de la console](/docs/containers?topic=containers-app#cli_dashboard)</td>
<td>Avez-vous déjà envisagé de pouvoir accéder au tableau de bord Kubernetes en un seul clic ? Vérifiez la présence du bouton **Tableau de bord Kubernetes** dans la console {{site.data.keyword.Bluemix_notm}}.</td>
</tr>
</tbody></table>




## Rubriques les plus consultées en avril 2018
{: #apr18}

<table summary="Ce tableau présente les rubriques les plus consultées. La lecture des lignes s'effectue de gauche à droite, avec la date indiquée dans la première colonne, le titre de la fonction dans la deuxième colonne et la description dans la troisième colonne.">
<caption>Rubriques les plus consultées pour les conteneurs et les clusters Kubernetes en avril 2018</caption>
<thead>
<th>Date</th>
<th>Titre</th>
<th>Description</th>
</thead>
<tbody>
<tr>
<td>17 avril</td>
<td>{{site.data.keyword.Bluemix_notm}} Block Storage</td>
<td>Installez le [plug-in](/docs/containers?topic=containers-block_storage#install_block) {{site.data.keyword.Bluemix_notm}} Block Storage pour sauvegarder des données persistantes dans du stockage par blocs. Ensuite, vous pouvez [créer](/docs/containers?topic=containers-block_storage#add_block) ou [utiliser du stockage par blocs existant](/docs/containers?topic=containers-block_storage#existing_block) pour votre cluster.</td>
</tr>
<tr>
<td>13 avril</td>
<td>[Nouveau tutoriel pour la migration d'applications Cloud Foundry dans des clusters](/docs/containers?topic=containers-cf_tutorial#cf_tutorial)</td>
<td>Vous disposez d'une application Cloud Foundry ? Découvrez comment déployer le même code de cette application dans un conteneur qui s'exécute dans un cluster Kubernetes.</td>
</tr>
<tr>
<td>5 avril</td>
<td>[Filtrage des journaux](/docs/containers?topic=containers-health#filter-logs)</td>
<td>Filtrez des journaux spécifiques pour empêcher leur transfert. Les journaux peuvent être filtrés pour un espace de nom, un nom de conteneur, un niveau de consignation et une chaîne de message spécifiques.</td>
</tr>
</tbody></table>

## Rubriques les plus consultées en mars 2018
{: #mar18}

<table summary="Ce tableau présente les rubriques les plus consultées. La lecture des lignes s'effectue de gauche à droite, avec la date indiquée dans la première colonne, le titre de la fonction dans la deuxième colonne et la description dans la troisième colonne.">
<caption>Rubriques les plus consultées pour les conteneurs et les clusters Kubernetes en mars 2018</caption>
<thead>
<th>Date</th>
<th>Titre</th>
<th>Description</th>
</thead>
<tbody>
<tr>
<td>16 mars</td>
<td>[Mise à disposition d'un cluster bare metal avec calcul sécurisé](/docs/containers?topic=containers-plan_clusters#shared_dedicated_node)</td>
<td>Créez un cluster bare metal qui exécute [Kubernetes version 1.9](/docs/containers?topic=containers-cs_versions#cs_v19) ou ultérieure et activez la fonction de calcul sécurisé pour vérifier que vos noeuds worker ne font pas l'objet de falsification.</td>
</tr>
<tr>
<td>14 mars</td>
<td>[Connexion sécurisée avec {{site.data.keyword.appid_full}}](/docs/containers?topic=containers-supported_integrations#appid)</td>
<td>Améliorez vos applications qui s'exécutent dans {{site.data.keyword.containerlong_notm}} en obligeant les utilisateurs à se connecter.</td>
</tr>
<tr>
<td>13 mars</td>
<td>[Zone disponible à São Paulo](/docs/containers?topic=containers-regions-and-zones)</td>
<td>Bienvenue à São Paulo au Brésil, nouvelle zone intégrée dans la région Sud des Etats-Unis. Si vous disposez d'un pare-feu, veillez à [ouvrir les ports de pare-feu](/docs/containers?topic=containers-firewall#firewall) correspondant à cette zone et aux autres zones situées dans la région où se trouve votre cluster.</td>
</tr>
</tbody></table>

## Rubriques les plus consultées en février 2018
{: #feb18}

<table summary="Ce tableau présente les rubriques les plus consultées. La lecture des lignes s'effectue de gauche à droite, avec la date indiquée dans la première colonne, le titre de la fonction dans la deuxième colonne et la description dans la troisième colonne.">
<caption>Rubriques les plus consultées pour les conteneurs et les clusters Kubernetes en février 2018</caption>
<thead>
<th>Date</th>
<th>Titre</th>
<th>Description</th>
</thead>
<tbody>
<tr>
<td>27 février</td>
<td>Images HVM (Hardware virtual machine) pour les noeuds worker</td>
<td>Améliorez les performances d'E-S de vos charges de travail avec des images HVM. Activez ces images sur chaque noeud worker en place à l'aide de la [commande](/docs/containers?topic=containers-cs_cli_reference#cs_worker_reload) `ibmcloud ks worker-reload` ou de la [commande](/docs/containers?topic=containers-cs_cli_reference#cs_worker_update) `ibmcloud ks worker-update`.</td>
</tr>
<tr>
<td>26 février</td>
<td>[Mise à l'échelle automatique avec KubeDNS](https://kubernetes.io/docs/tasks/administer-cluster/dns-horizontal-autoscaling/)</td>
<td>KubeDNS s'adapte désormais à l'évolution de votre cluster. Vous pouvez ajuster les ratios de mise à l'échelle à l'aide de la commande : `kubectl -n kube-system edit cm kube-dns-autoscaler`.</td>
</tr>
<tr>
<td>23 février</td>
<td>Affichage de la console Web pour la [consignation](/docs/containers?topic=containers-health#view_logs) et les [métriques](/docs/containers?topic=containers-health#view_metrics)</td>
<td>Affichez facilement les données des journaux et des métriques sur votre cluster et ses composants avec une interface utilisateur Web améliorée. Consultez la page des détails de votre cluster pour savoir comment y accéder.</td>
</tr>
<tr>
<td>20 février</td>
<td>Images chiffrées et [contenu sécurisé, signé](/docs/services/Registry?topic=registry-registry_trustedcontent#registry_trustedcontent)</td>
<td>Dans {{site.data.keyword.registryshort_notm}}, vous pouvez signer et chiffrer des images pour assurer leur intégrité lorsque vous les stockez dans votre espace de nom de registre. Exécutez vos instances de conteneur en utilisant uniquement du contenu sécurisé.</td>
</tr>
<tr>
<td>19 février</td>
<td>[Configuration du VPN IPSec strongSwan](/docs/containers?topic=containers-vpn#vpn-setup)</td>
<td>Déployez rapidement la charte Helm du VPN IPSec strongSwan pour connecter votre cluster {{site.data.keyword.containerlong_notm}} de manière sécurisée à votre centre de données sur site sans dispositif de routeur virtuel (VRA).</td>
</tr>
<tr>
<td>14 février</td>
<td>[Zone disponible à Séoul](/docs/containers?topic=containers-regions-and-zones)</td>
<td>Juste au bon moment pour les Jeux Olympiques, déployez un cluster Kubernetes à Séoul dans la région Asie-Pacifique nord. Si vous disposez d'un pare-feu, veillez à [ouvrir les ports de pare-feu](/docs/containers?topic=containers-firewall#firewall) correspondant à cette zone et aux autres zones situées dans la région où se trouve votre cluster.</td>
</tr>
<tr>
<td>8 février</td>
<td>[Mise à jour vers Kubernetes 1.9](/docs/containers?topic=containers-cs_versions#cs_v19)</td>
<td>Examinez les modifications à apporter à vos clusters avant d'effectuer la mise à jour vers Kubernetes 1.9.</td>
</tr>
</tbody></table>

## Rubriques les plus consultées en janvier 2018
{: #jan18}

<table summary="Ce tableau présente les rubriques les plus consultées. La lecture des lignes s'effectue de gauche à droite, avec la date indiquée dans la première colonne, le titre de la fonction dans la deuxième colonne et la description dans la troisième colonne.">
<caption>Rubriques les plus consultées pour les conteneurs et les clusters Kubernetes en janvier 2018</caption>
<thead>
<th>Date</th>
<th>Titre</th>
<th>Description</th>
</thead>
<tbody>
<td>25 janvier</td>
<td>[Registre global disponible](/docs/services/Registry?topic=registry-registry_overview#registry_regions)</td>
<td>Avec le registre {{site.data.keyword.registryshort_notm}}, vous pouvez utiliser le registre global `registry.bluemix.net` pour extraire les images publiques fournies par IBM.</td>
</tr>
<tr>
<td>23 janvier</td>
<td>[Zones disponibles à Singapour et Montréal (Canada)](/docs/containers?topic=containers-regions-and-zones)</td>
<td>Singapour et Montréal sont les zones disponibles dans les régions {{site.data.keyword.containerlong_notm}} Asie-Pacifique nord et Est des Etats-Unis. Si vous disposez d'un pare-feu, veillez à [ouvrir les ports de pare-feu](/docs/containers?topic=containers-firewall#firewall) correspondant à ces zones et aux autres zones situées dans la région où se trouve votre cluster.</td>
</tr>
<tr>
<td>8 janvier</td>
<td>[Versions disponibles améliorées](/docs/containers?topic=containers-cs_cli_reference#cs_machine_types)</td>
<td>Les types de machine virtuelle de la série 2 comprennent du stockage SSD et le chiffrement de disque. [Déplacez vos charges de travail](/docs/containers?topic=containers-update#machine_type) sur ces versions pour améliorer les performances et la stabilité.</td>
</tr>
</tbody></table>

## Discussion avec des développeurs partageant les mêmes idées sur Slack
{: #slack}

Vous pouvez voir les thèmes de discussion d'autres personnes et poser vos propres questions dans [{{site.data.keyword.containerlong_notm}} Slack. ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://ibm-container-service.slack.com)
{:shortdesc}

Si vous n'utilisez pas un ID IBM pour votre compte {{site.data.keyword.Bluemix_notm}}, [demandez une invitation](https://bxcs-slack-invite.mybluemix.net/) sur ce site Slack.
{: tip}
