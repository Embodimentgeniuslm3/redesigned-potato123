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


# Journal des modifications de l'interface de ligne de commande (CLI)
{: #cs_cli_changelog}

Dans le terminal, vous êtes averti des mises à jour disponibles pour l'interface de ligne de commande `ibmcloud` et les plug-ins. Veillez à maintenir votre interface de ligne de commande à jour pour pouvoir utiliser l'ensemble des commandes et des indicateurs.{:shortdesc}

Pour installer le plug-in de l'interface CLI {{site.data.keyword.containerlong}}, voir [Installation de l'interface de ligne de commande](/docs/containers?topic=containers-cs_cli_install#cs_cli_install_steps).

Consultez le tableau suivant pour obtenir un récapitulatif des modifications de chaque version du plug-in d'interface de ligne de commande {{site.data.keyword.containerlong_notm}}.

<table summary="Présentation des modifications de version pour le plug-in de l'interface de ligne de commande {{site.data.keyword.containerlong_notm}} ">
<caption>Journal des modifications pour le plug-in de l'interface de ligne de commande {{site.data.keyword.containerlong_notm}}</caption>
<thead>
<tr>
<th>Version</th>
<th>Date de publication</th>
<th>Modifications</th>
</tr>
</thead>
<tbody>
<tr>
<td>0.3.34</td>
<td>31 mai 2019</td>
<td>Ajout de la prise en charge relative à la création de clusters Red Hat OpenShift on IBM Cloud. <ul>
<li>Ajout de la prise en charge relative aux versions OpenShift dans l'indicateur `--kube-version` de la commande `cluster-create`. Par exemple, pour créer un cluster OpenShift standard, vous pouvez spécifier `--kube-version 3.11_openshift` dans votre commande `cluster-create`. </li>
<li>Ajout de la commande `versions` permettant de répertorier toutes les versions Kubernetes et OpenShift prises en charge. </li>
<li>Dépréciation de la commande `kube-versions`. </li>
</ul></td>
</tr>
<tr>
<td>0.3.33</td>
<td>30 mai 2019</td>
<td><ul>
<li>Ajoute de l'indicateur <code>--powershell</code> à la commande `cluster-config` pour extraire des variables d'environnement Kubernetes au format Windows PowerShell. </li>
<li>Dépréciation des commandes `region-get`, `region-set` et `regions`. Pour plus d'informations, voir [Fonctionnalité de noeud final global](/docs/containers?topic=containers-regions-and-zones#endpoint).</li>
</ul></td>
</tr>
<tr>
<td>0.3.28</td>
<td>23 mai 2019</td>
<td><ul><li>Ajout de la commande [<code>ibmcloud ks alb-create</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_create) permettant de créer des ALB Ingress. Pour plus d'informations, voir [Mise à l'échelle des ALB](/docs/containers?topic=containers-ingress#scale_albs).</li>
<li>Ajout de la commande [<code>ibmcloud ks infra-permissions-get</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#infra_permissions_get) permettant de vérifier si les droits d'infrastructure suggérés ou manquants pour la région et le groupe de ressources ciblés ne contiennent pas les données d'identification qui autorisent l'[accès au portefeuille d'infrastructure IBM Cloud (SoftLayer)](/docs/containers?topic=containers-users#api_key). </li>
<li>Ajout de l'indicateur <code>--private-only</code> à la commande `zone-network-set` afin d'annuler la définition du VLAN public pour les métadonnées de pool de noeuds worker de sorte que les noeuds worker qui figureront ultérieurement dans cette zone de pool de noeuds worker soient connectés à un VLAN privé uniquement. </li>
<li>Retrait de l'indicateur <code>--force-update</code> de la commande `worker-update`. </li>
<li>Ajout de la colonne **ID VLAN** à la sortie des commandes `albs` et `alb-get`.</li>
<li>Ajout de la colonne **Métropole multizone** à la sortie de la commande `supported-locations` pour désigner les zones compatibles avec plusieurs zones. </li>
<li>Ajout des zones **Etat du maître** et **Santé du maître** à la sortie de la commande `cluster-get`. Pour plus d'informations, voir [Etats du maître](/docs/containers?topic=containers-health#states_master).</li>
<li>Mise à jour de la traduction du texte d'aide</li>
</ul></td>
</tr>
<tr>
<td>0.3.8</td>
<td>30 avril 2019</td>
<td>Ajout de la prise en charge de la [fonctionnalité de noeud final global](/docs/containers?topic=containers-regions-and-zones#endpoint) dans la version `0.3`. Par défaut, vous pouvez désormais afficher et gérer toutes vos ressources {{site.data.keyword.containerlong_notm}} dans tous les emplacements. Vous n'êtes pas obligé de cibler une région cible pour gérer des ressources.</li>
<ul><li>Ajout de la commande [<code>ibmcloud ks supported-locations</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_supported-locations) pour répertorier tous les emplacements pris en charge par {{site.data.keyword.containerlong_notm}}. </li>
<li>Ajout de l'indicateur <code>--locations</code> aux commandes `clusters` et `zones` pour filtrer les ressources en fonction d'un ou de plusieurs emplacements. </li>
<li>Ajout de l'indicateur <code>--region</code> aux commandes `credential-set/unset/get`, `api-key-reset` et `vlan-spanning-get`. Pour exécuter ces commandes, vous devez spécifier une région dans l'indicateur `--region`. </li></ul></td>
</tr>
<tr>
<td>0.2.102</td>
<td>15 avril 2019</td>
<td>Ajout du [groupe de commandes `ibmcloud ks nlb-dns`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#nlb-dns) afin d'enregistrer et de gérer un nom d'hôte pour les adresses IP NLB, et du [groupe de commandes `ibmcloud ks nlb-dns-monitor` ](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-monitor) afin de créer et modifier des moniteurs de diagnostic d'intégrité pour les noms d'hôte NLB. Pour plus d'informations, voir [Enregistrement d'adresses IP NLB avec un nom d'hôte DNS](/docs/containers?topic=containers-loadbalancer#loadbalancer_hostname_dns).
</td>
</tr>
<tr>
<td>0.2.99</td>
<td>9 avril 2019</td>
<td><ul>
<li>Mise à jour du texte d'aide</li>
<li>Mise à jour de la version Go vers 1.12.2.</li>
</ul></td>
</tr>
<tr>
<td>0.2.95</td>
<td>3 avril 2019</td>
<td><ul>
<li>Ajout de la prise en charge de gestion des versions pour les modules complémentaires de cluster gérés</li>
<ul><li>Ajout de la commande [<code>ibmcloud ks addon-versions</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_addon_versions).</li>
<li>Ajout de l'option <code>--version</code> aux commandes [ibmcloud ks cluster-addon-enable](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_addon_enable).</li></ul>
<li>Mise à jour de la traduction du texte d'aide</li>
<li>Mises à jour des liens rapides vers la documentation dans le texte d'aide</li>
<li>Correction d'un bogue pour lequel les messages d'erreur JSON sont imprimés dans un format incorrect</li>
<li>Correction d'un bogue pour lequel l'utilisation de l'option silencieuse (`-s`) sur certaines commandes empêchait l'impression des erreurs</li>
</ul></td>
</tr>
<tr>
<td>0.2.80</td>
<td>19 mars 2019</td>
<td><ul>
<li>Prise en charge de l'activation de la [communication entre le maître et les noeuds worker avec des noeuds finaux de service](/docs/containers?topic=containers-plan_clusters#workeruser-master) dans les clusters standard exécutant Kubernetes version 1.11 ou ultérieure dans les [comptes avec la fonction VRF activée](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started).<ul>
<li>Ajout des indicateurs `--private-service-endpoint` et `--public-service-endpoint` dans la commande [<code>ibmcloud ks cluster-create</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_create).</li>
<li>Ajout des zones d'URL de noeud final de service public et de service privé (**Public Service Endpoint URL** et **Private Service Endpoint URL**) dans la sortie de la commande <code>ibmcloud ks cluster-get</code>.</li>
<li>Ajout de la commande [<code>ibmcloud ks cluster-feature-enable private-service-endpoint</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable_private_service_endpoint).</li>
<li>Ajout de la commande [<code>ibmcloud ks cluster-feature-enable public-service-endpoint</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable_public_service_endpoint).</li>
<li>Ajout de la commande [<code>ibmcloud ks cluster-feature-disable public-service-endpoint</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_disable).</li>
</ul></li>
<li>Mise à jour de la documentation et de la traduction.</li>
<li>Mise à jour de la version Go à 1.11.6.</li>
<li>Résolution des problèmes de réseau par intermittence pour les utilisateurs macOS.</li>
</ul></td>
</tr>
<tr>
<td>0.2.75</td>
<td>14 mars 2019</td>
<td><ul><li>Masquage du code HTML brut dans les sorties d'erreur.</li>
<li>Correction de fautes de frappe dans le texte d'aide.</li>
<li>Correction de la traduction du texte d'aide.</li>
</ul></td>
</tr>
<tr>
<td>0.2.61</td>
<td>26 fév. 2019</td>
<td><ul>
<li>Ajout de la commande `cluster-pull-secret-apply`, qui crée un ID de service IAM pour le cluster, les règles, la clé d'API et les valeurs confidentielles d'extraction d'image pour que les conteneurs qui s'exécutent dans l'espace de nom Kubernetes `default` puissent extraire des images depuis IBM Cloud Container Registry. Pour les nouveaux clusters, les valeurs confidentielles d'extraction d'image qui utilisent des données d'identification IAM sont créées par défaut. Utilisez cette commande pour mettre à jour des clusters existants ou si votre cluster comporte une erreur de valeur confidentielle d'extraction d'image lors de sa création. Pour plus d'informations, voir [la documentation](https://test.cloud.ibm.com/docs/containers?topic=containers-images#cluster_registry_auth).</li>
<li>Correction d'un bogue où les échecs de la commande `ibmcloud ks init` entraînaient l'impression en sortie de l'aide.</li>
</ul></td>
</tr>
<tr>
<td>0.2.53</td>
<td>19 fév. 2019</td>
<td><ul><li>Correction d'un bogue où la région était ignorée pour les commandes `ibmcloud ks api-key-reset`, `ibmcloud ks credential-get/set` et `ibmcloud ks vlan-spanning-get`.</li>
<li>Amélioration des performances pour la commande `ibmcloud ks worker-update`.</li>
<li>Ajout de la version du module complémentaire dans les invites de la commande `ibmcloud ks cluster-addon-enable`.</li>
</ul></td>
</tr>
<tr>
<td>0.2.44</td>
<td>8 fév. 2019</td>
<td><ul>
<li>Ajout de l'option `--skip-rbac` dans la commande `ibmcloud ks cluster-config` pour ignorer l'ajout de rôles RBAC Kubernetes aux utilisateurs en fonction des rôles d'accès au service {{site.data.keyword.Bluemix_notm}} IAM dans la configuration du cluster. Incluez cette option uniquement si vous [gérez vos propres rôles Kubernetes RBAC](/docs/containers?topic=containers-users#rbac). Si vous utilisez des [rôles d'accès au service {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-access_reference#service) pour gérer tous vos utilisateurs RBAC, n'incluez pas cette option.</li>
<li>Mise à jour de la version Go à 1.11.5.</li>
</ul></td>
</tr>
<tr>
<td>0.2.40</td>
<td>6 fév. 2019</td>
<td><ul>
<li>Ajout des commandes [<code>ibmcloud ks cluster-addons</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_addons), [<code>ibmcloud ks cluster-addon-enable</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_addon_enable) et [<code>ibmcloud ks cluster-addon-disable</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_addon_disable) pour utiliser des modules complémentaires de cluster gérés, tels qu'[Istio](/docs/containers?topic=containers-istio) et [Knative](/docs/containers?topic=containers-serverless-apps-knative) pour {{site.data.keyword.containerlong_notm}}.</li>
<li>Amélioration du texte d'aide de la commande <code>ibmcloud ks vlans</code> pour les utilisateurs d'{{site.data.keyword.Bluemix_dedicated_notm}}.</li></ul></td>
</tr>
<tr>
<td>0.2.30</td>
<td>31 jan. 2019</td>
<td>Augmentation de la valeur de délai d'attente par défaut à `500s` pour la commande `ibmcloud ks cluster-config`.</td>
</tr>
<tr>
<td>0.2.19</td>
<td>16 jan. 2019</td>
<td><ul><li>Ajout de la variable d'environnement `IKS_BETA_VERSION` pour activer la version bêta remaniée de l'interface de ligne de commande du plug-in {{site.data.keyword.containerlong_notm}}. Pour découvrir la version remaniée, voir [Utilisation de la structure de commande bêta](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_beta).</li>
<li>Augmentation de la valeur du délai d'attente par défaut à `60s` pour la commande `ibmcloud ks subnets`.</li>
<li>Corrections d'erreurs mineures et de traduction.</li></ul></td>
</tr>
<tr>
<td>0.1.668</td>
<td>18 déc. 2018</td>
<td><ul><li>Le noeud final d'API par défaut <code>https://containers.bluemix.net</code> a été remplacé par <code>https://containers.cloud.ibm.com</code>.</li>
<li>Correction d'erreurs pour que les traductions s'affichent correctement dans l'aide des commandes et les messages d'erreur.</li>
<li>Affichage plus rapide de l'aide sur les commandes.</li></ul></td>
</tr>
<tr>
<td>0.1.654</td>
<td>5 déc. 2018</td>
<td>Mise à jour de la documentation et de la traduction.</td>
</tr>
<tr>
<td>0.1.638</td>
<td>15 nov. 2018</td>
<td>
<ul><li>Ajout de l'alias [<code>ibmcloud ks cluster-refresh</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_apiserver_refresh) à la commande `apiserver-refresh`. </li>
<li>Ajout du nom du groupe de ressources dans le résultat des commandes <code>ibmcloud ks cluster-get</code> et <code>ibmcloud ks clusters</code>.</li></ul>
</td>
</tr>
<tr>
<td>0.1.635</td>
<td>6 nov. 2018</td>
<td>Ajout de commandes pour la gestion des mises à jour automatiques du module complémentaire de cluster ALB Ingress :<ul>
<li>[<code>ibmcloud ks alb-autoupdate-disable</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_autoupdate_disable)</li>
<li>[<code>ibmcloud ks alb-autoupdate-enable</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_autoupdate_enable)</li>
<li>[<code>ibmcloud ks alb-autoupdate-get</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_autoupdate_get)</li>
<li>[<code>ibmcloud ks alb-rollback</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_rollback)</li>
<li>[<code>ibmcloud ks alb-update</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_update)</li>
</ul></td>
</tr>
<tr>
<td>0.1.621</td>
<td>30 oct. 2018</td>
<td><ul>
<li>Ajout de la [commande <code>ibmcloud ks credential-get</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credential_get).</li>
<li>Prise en charge de la source de journal <code>storage</code> pour toutes les commandes de cluster liées à la consignation. Pour plus d'informations, voir <a href="/docs/containers?topic=containers-health#logging">Description de l'acheminement des journaux de cluster et d'application</a>.</li>
<li>Ajout de l'indicateur `--network` à la [commande <code>ibmcloud ks cluster-config</code> ](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_config), qui télécharge le fichier de configuration Calico pour exécuter toutes les commandes Calico. </li>
<li>Corrections d'erreur mineures et restructuration</li></ul>
</td>
</tr>
<tr>
<td>0.1.593</td>
<td>10 oct. 2018</td>
<td><ul><li>Ajout de l'ID du groupe de ressource dans la sortie de la commande <code>ibmcloud ks cluster-get</code>.</li>
<li>Lorsque [{{site.data.keyword.keymanagementserviceshort}} est activé](/docs/containers?topic=containers-encryption#keyprotect) en tant que fournisseur de service de gestion des clés (KMS) dans votre cluster, ajout de la zone KMS activée dans la sortie de la commande <code>ibmcloud ks cluster-get</code>.</li></ul></td>
</tr>
<tr>
<td>0.1.591</td>
<td>2 oct. 2018</td>
<td>Ajout de la prise en charge des [groupes de ressources](/docs/containers?topic=containers-clusters#cluster_prepare).</td>
</tr>
<tr>
<td>0.1.590</td>
<td>1er oct. 2018</td>
<td><ul>
<li>Ajout des commandes [<code>ibmcloud ks logging-collect</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_collect) et [<code>ibmcloud ks logging-collect-status</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_collect_status) pour la collecte des journaux de serveur d'API dans votre cluster.</li>
<li>Ajout de la [commande <code>ibmcloud ks key-protect-enable</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_key_protect) pour activer {{site.data.keyword.keymanagementserviceshort}} en tant que fournisseur de service de gestion des clés (KMS) dans votre cluster. </li>
<li>Ajout de l'indicateur <code>--skip-master-health</code> dans les commandes [ibmcloud ks worker-reboot](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reboot) et [ibmcloud ks worker-reload](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reboot) pour ignorer le diagnostic d'intégrité du maître avant de lancer une opération de réamorçage ou de rechargement.</li>
<li>Remplacement du nom de l'adresse e-mail du propriétaire <code>Owner Email</code> par <code>Owner</code> dans la sortie de la commande <code>ibmcloud ks cluster-get</code>.</li></ul></td>
</tr>
</tbody>
</table>
