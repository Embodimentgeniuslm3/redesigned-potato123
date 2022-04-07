---

copyright:
  years: 2014, 2019
lastupdated: "2019-04-09"

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


# Intégration de services
{: #integrations}

Vous pouvez utiliser différents services externes et services de catalogue avec un cluster Kubernetes standard dans {{site.data.keyword.containerlong}}.
{:shortdesc}


## Services DevOps
{: #devops_services}
<table summary="Le tableau présente les services disponibles que vous pouvez ajouter à votre cluster pour ajouter des fonctionnalités DevOps supplémentaires. La lecture des lignes s'effectue de gauche à droite, avec le nom du service dans la première colonne et une description du service dans la deuxième colonne.">
<caption>Services DevOps</caption>
<thead>
<tr>
<th>Service</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.cfee_full_notm}}</td>
<td>Déployez et gérez votre propre plateforme Cloud Foundry en plus d'un cluster Kubernetes pour développer, conditionner, déployer et gérer des applications cloud natives et tirer parti de l'écosystème {{site.data.keyword.Bluemix_notm}} pour lier d'autres services à vos applications. Lorsque vous créez une instance {{site.data.keyword.cfee_full_notm}}, vous devez configurer votre cluster Kubernetes en choisissant le type de machine et les réseaux locaux virtuels (VLAN) pour vos noeuds worker. Votre cluster est ensuite mis à disposition avec {{site.data.keyword.containerlong_notm}} et {{site.data.keyword.cfee_full_notm}} est automatiquement déployé dans votre cluster. Pour plus d'informations sur la configuration d'{{site.data.keyword.cfee_full_notm}}, voir le [tutoriel d'initiation](/docs/cloud-foundry?topic=cloud-foundry-getting-started#getting-started). </td>
</tr>
<tr>
<td>Codeship</td>
<td>Vous pouvez utiliser <a href="https://codeship.com" target="_blank">Codeship <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a> pour l'intégration et la distribution continues de conteneurs. Pour plus d'informations, voir <a href="https://www.ibm.com/blogs/bluemix/2017/10/using-codeship-pro-deploy-workloads-ibm-container-service/" target="_blank">Using Codeship Pro To Deploy Workloads to {{site.data.keyword.containerlong_notm}} <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a>. </td>
</tr>
<tr>
<td>Grafeas</td>
<td>[Grafeas ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://grafeas.io) est un service open source d'intégration et de déploiement en continu (CI/CD) qui fournit une méthode commune pour extraire, stocker et échanger des métadonnées lors du processus de la chaîne d'approvisionnement de logiciel. Par exemple, si vous intégrez Grafeas dans votre processus de génération d'applications, Grafeas peut stocker des informations sur l'auteur de la demande de génération, les résultats d'analyse de vulnérabilité et la validation en terme d'assurance qualité pour que vous puissiez prendre une décision en connaissance de cause pour le déploiement d'une application en production. Vous pouvez utiliser ces métadonnées dans des audits ou pour prouver la conformité de votre chaîne d'approvisionnement de logiciel. </td>
</tr>
<tr>
<td>Helm</td>
<td> <a href="https://helm.sh" target="_blank">Helm <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a> est un gestionnaire de package Kubernetes. Vous pouvez créer de nouvelles chartes Helm ou utiliser des chartes Helm préexistantes pour définir, installer et mettre à niveau des applications Kubernetes complexes qui s'exécutent dans les clusters {{site.data.keyword.containerlong_notm}}. <p>Pour plus d'informations, voir [Configuration de Helm dans {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-integrations#helm).</p></td>
</tr>
<tr>
<td>{{site.data.keyword.contdelivery_full}}</td>
<td>Permet d'automatiser vos générations d'applications et vos déploiements de conteneur sur les clusters Kubernetes en utilisant une chaîne d'outils. Pour obtenir des informations de configuration, voir le blogue <a href="https://developer.ibm.com/recipes/tutorials/deploy-kubernetes-pods-to-the-bluemix-container-service-using-devops-pipelines/" target="_blank">Deploy Kubernetes pods to the {{site.data.keyword.containerlong_notm}} using DevOps Pipelines <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a>. </td>
</tr>
<tr>
<td>Istio sur {{site.data.keyword.containerlong_notm}}</td>
<td><a href="https://www.ibm.com/cloud/info/istio" target="_blank">Istio <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a> est un service open source qui fournit aux développeurs un moyen de connecter, sécuriser, gérer et surveiller un réseau de microservices, appelé également maillage de services, sur des plateformes d'orchestration de cloud. Istio sur {{site.data.keyword.containerlong}} propose une installation en une seule étape d'Istio dans votre cluster via un module complémentaire géré. En un seul clic, vous pouvez obtenir tous les composants de base d'Istio, des fonctions de suivi, de surveillance et de visualisation supplémentaires, ainsi que le modèle opérationnel d'application BookInfo. Pour commencer, voir [Utilisation du module complémentaire Istio géré (bêta)](/docs/containers?topic=containers-istio).</td>
</tr>
<tr>
<td>Jenkins X</td>
<td>Jenkins X est une plateforme d'intégration et de déploiement en continu Kubernetes native que vous pouvez utiliser pour automatiser votre processus de génération. Pour en savoir plus sur comment l'installer sur {{site.data.keyword.containerlong_notm}}, voir [Introducing the Jenkins X open source project ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/blogs/bluemix/2018/08/installing-jenkins-x-on-ibm-cloud-kubernetes-service/).</td>
</tr>
<tr>
<td>Knative</td>
<td>[Knative ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/knative/docs) est une plateforme open source développée par IBM, Google, Pivotal, Red Hat, Cisco et d'autres firmes dans l'objectif d'étendre les capacités de Kubernetes pour vous aider à créer des applications modernes, conteneurisées, axées sur la source et sans serveur, en plus de votre cluster Kubernetes. Cette plateforme utilise une approche cohérente couvrant plusieurs infrastructures et langages de programmation pour permettre l'abstraction de la charge opérationnelle de construction, déploiement et gestion des charges de travail dans Kubernetes pour que les développeurs puissent se concentrer sur ce qui les intéresse le plus : le code source. Pour plus d'informations, voir [Tutoriel : Utilisation du module complémentaire géré Knative pour exécuter des applications sans serveur dans les clusters Kubernetes](/docs/containers?topic=containers-knative_tutorial#knative_tutorial). </td>
</tr>
</tbody>
</table>

<br />



## Services de consignation et de surveillance
{: #health_services}
<table summary="Le tableau présente les services disponibles que vous pouvez ajouter à votre cluster pour ajouter des fonctionnalités supplémentaires de consignation et de surveillance. La lecture des lignes s'effectue de gauche à droite, avec le nom du service dans la première colonne, et une description du service dans la deuxième colonne.">
<caption>Services de consignation et de surveillance</caption>
<thead>
<tr>
<th>Service</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>CoScale</td>
<td>Vous pouvez surveiller les noeuds worker, les conteneurs, les jeux de répliques, les contrôleurs de réplication et les services à l'aide de <a href="https://www.newrelic.com/coscale" target="_blank">CoScale <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a>. Pour plus d'informations, voir <a href="https://www.ibm.com/blogs/bluemix/2017/06/monitoring-ibm-bluemix-container-service-coscale/" target="_blank">Monitoring {{site.data.keyword.containerlong_notm}} with CoScale <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a>. </td>
</tr>
<tr>
<td>Datadog</td>
<td>Vous pouvez surveiller votre cluster et examiner les performances de l'infrastructure et les métriques d'application à l'aide de <a href="https://www.datadoghq.com/" target="_blank">Datadog <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a>. Pour plus d'informations, voir <a href="https://www.ibm.com/blogs/bluemix/2017/07/monitoring-ibm-bluemix-container-service-datadog/" target="_blank">Monitoring {{site.data.keyword.containerlong_notm}} with Datadog <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a>. </td>
</tr>
<tr>
<td>{{site.data.keyword.cloudaccesstrailfull}}</td>
<td>Surveillez les activités d'administration intervenant dans votre cluster en analysant les journaux via Grafana. Pour plus d'informations sur ce service, voir la documentation d'[Activity Tracker](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started-with-cla). Pour plus d'informations sur les types d'événement dont vous pouvez assurer le suivi, voir [Evénements Activity Tracker](/docs/containers?topic=containers-at_events).</td>
</tr>
<tr>
<td>{{site.data.keyword.la_full_notm}}</td>
<td>Ajoutez des fonctionnalités de gestion de journaux à votre cluster en déployant LogDNA en tant que service tiers sur vos noeuds worker afin de gérer les journaux à partir de vos conteneurs de pod. Pour plus d'informations, voir [Managing Kubernetes cluster logs with {{site.data.keyword.loganalysisfull_notm}} with LogDNA](/docs/services/Log-Analysis-with-LogDNA/tutorials?topic=LogDNA-kube#kube).</td>
</tr>
<tr>
<td>{{site.data.keyword.mon_full_notm}}</td>
<td>Gagnez en visibilité opérationnelle sur les performances et l'état de santé de vos applications en déployant Sysdig en tant que service tiers sur vos noeuds worker pour transférer des métriques à {{site.data.keyword.monitoringlong}}. Pour plus d'informations, voir [Analyzing metrics for an app that is deployed in a Kubernetes cluster](/docs/services/Monitoring-with-Sysdig/tutorials?topic=Sysdig-kubernetes_cluster#kubernetes_cluster). </td>
</tr>
<tr>
<td>Instana</td>
<td> <a href="https://www.instana.com/" target="_blank">Instana <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a> permet de surveiller les performances de l'infrastructure et des applications à l'aide d'une interface graphique qui reconnaît et mappe automatiquement vos applications. Instana capture toutes les demandes adressées à vos applications, que vous pouvez utiliser pour identifier et résoudre les problèmes et effectuer une analyse de leur cause première afin d'éviter qu'ils se reproduisent. Consultez l'article de blogue sur le <a href="https://www.instana.com/blog/precise-visibility-applications-ibm-bluemix-container-service/" target="_blank">déploiement d'Instana dans {{site.data.keyword.containerlong_notm}} <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a> pour en savoir plus.</td>
</tr>
<tr>
<td>Prometheus</td>
<td>Prometheus est un outil open source de surveillance, de consignation et d'alerte spécialement conçu pour Kubernetes. Prometheus extrait des informations détaillées sur le cluster, les noeuds worker et l'état de santé du déploiement à partir des informations de consignation de Kubernetes. L'activité de l'UC, de la mémoire, des E-S et du réseau est collectée pour chaque conteneur s'exécutant dans un cluster. Vous pouvez utiliser les données collectées dans des demandes ou des alertes personnalisées pour surveiller les performances et les charges de travail dans votre cluster.

<p>Pour utiliser Prometheus, suivez les <a href="https://github.com/coreos/prometheus-operator/tree/master/contrib/kube-prometheus" target="_blank">instructions CoreOS <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a>.</p>
</td>
</tr>
<tr>
<td>Sematext</td>
<td>Vous pouvez afficher les métriques et les journaux de vos applications conteneurisées à l'aide de <a href="https://sematext.com/" target="_blank">Sematext <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a>. Pour plus d'informations, voir la rubrique sur <a href="https://www.ibm.com/blogs/bluemix/2017/09/monitoring-logging-ibm-bluemix-container-service-sematext/" target="_blank">la surveillance et la consignation des conteneurs avec Sematext <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a>. </td>
</tr>
<tr>
<td>Splunk</td>
<td>Importez et recherchez vos données de métriques, d'objets et de consignation Kubernetes dans Splunk en utilisant Splunk Connect for Kubernetes. Splunk Connect for Kubernetes est une collection de chartes Helm qui déploient le module Fluentd pris en charge par Splunk dans votre cluster Kubernetes, le plug-in Fluentd HTTP Event Collector (HEC) généré par Splunk pour envoyer des journaux et des métadonnées, ainsi qu'un déploiement de métriques pour capturer les métriques de votre cluster. Pour plus d'informations, voir <a href="https://www.ibm.com/blogs/bluemix/2019/02/solving-business-problems-with-splunk-on-ibm-cloud-kubernetes-service/" target="_blank">Solving Business Problems with Splunk on {{site.data.keyword.containerlong_notm}} <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a>.</td>
</tr>
<tr>
<td>Weave Scope</td>
<td>Weave Scope produit un diagramme visuel de vos ressources dans un cluster Kubernetes, notamment des services, pods, conteneurs, processus, noeuds, et autres. Weave Scope fournit des métriques interactives sur l'utilisation de l'UC et de la mémoire, ainsi que des outils pour suivi et exécution dans un conteneur.<p>Pour plus d'informations, voir [Visualisation de ressources de cluster Kubernetes via Weave Scope et {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-integrations#weavescope).</p></li></ol>
</td>
</tr>
</tbody>
</table>

<br />



## Services de sécurité
{: #security_services}

Vous souhaitez avoir une vision globale de l'intégration des services de sécurité {{site.data.keyword.Bluemix_notm}} dans votre cluster ? Consultez le [tutoriel sur l'application de la sécurité de bout en bout pour une application en cloud](/docs/tutorials?topic=solution-tutorials-cloud-e2e-security).
{: shortdesc}

<table summary="Le tableau présente les services disponibles que vous pouvez ajouter à votre cluster pour ajouter des fonctionnalités de sécurité supplémentaires. La lecture des lignes s'effectue de gauche à droite, avec le nom du service dans la première colonne et une description du service dans la deuxième colonne.">
<caption>Services de sécurité</caption>
<thead>
<tr>
<th>Service</th>
<th>Description</th>
</tr>
</thead>
<tbody>
  <tr id="appid">
    <td>{{site.data.keyword.appid_full}}</td>
    <td>Ajoutez un niveau de sécurité à vos applications avec [{{site.data.keyword.appid_short}}](/docs/services/appid?topic=appid-getting-started) en obligeant les utilisateurs à se connecter. Pour l'authentification de demandes HTTP/HTTPS Web ou d'API vers votre application, vous pouvez intégrer {{site.data.keyword.appid_short_notm}} au service Ingress en utilisant l'[annotation Ingress d'authentification {{site.data.keyword.appid_short_notm}}](/docs/containers?topic=containers-ingress_annotation#appid-auth).</td>
  </tr>
<tr>
<td>Aqua Security</td>
  <td>En complément de <a href="/docs/services/va?topic=va-va_index" target="_blank">Vulnerability Advisor</a>, vous pouvez utiliser <a href="https://www.aquasec.com/" target="_blank">Aqua Security <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a> pour renforcer la sécurité de vos déploiements de conteneurs en limitant ce que votre application est autorisée à effectuer. Pour plus d'informations, voir l'article <a href="https://blog.aquasec.com/securing-container-deployments-on-bluemix-with-aqua-security" target="_blank">Securing container deployments on {{site.data.keyword.Bluemix_notm}} with Aqua Security <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a>. </td>
</tr>
<tr>
<td>{{site.data.keyword.cloudcerts_full}}</td>
<td>Vous pouvez utiliser <a href="/docs/services/certificate-manager?topic=certificate-manager-getting-started#getting-started" target="_blank">{{site.data.keyword.cloudcerts_long}} <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a> pour stocker et gérer les certificats SSL pour vos applications. Pour plus d'informations, voir <a href="https://www.ibm.com/blogs/bluemix/2018/01/use-ibm-cloud-certificate-manager-ibm-cloud-container-service-deploy-custom-domain-tls-certificates/" target="_blank">Use {{site.data.keyword.cloudcerts_long_notm}} with {{site.data.keyword.containerlong_notm}} to deploy custom domain TLS Certificates <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a>. </td>
</tr>
<tr>
  <td>{{site.data.keyword.datashield_full}} (bêta)</td>
  <td>Vous pouvez utiliser <a href="/docs/services/data-shield?topic=data-shield-getting-started#getting-started" target="_blank">{{site.data.keyword.datashield_short}} <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a> pour chiffrer votre mémoire de données. {{site.data.keyword.datashield_short}} est intégré à la technologie Intel® Software Guard Extensions (SGX) et Fortanix® pour que le code et les données de vos charges de travail de conteneur {{site.data.keyword.Bluemix_notm}} soient protégés lors de leur utilisation. Le code d'application et les données s'exécutent dans des enclaves d'UC fortifiées, qui correspondent à des zones de mémoire sécurisées sur le noeud worker qui protègent les aspects critiques de l'application, ce qui permet de conserver le code et les données confidentiels et inchangés.</td>
</tr>
<tr>
  <td>{{site.data.keyword.registrylong}}</td>
  <td>Configurez votre propre référentiel d'images Docker sécurisé où vous pourrez stocker et partager des images en toute sécurité entre les utilisateurs du cluster. Pour plus d'informations, voir la <a href="/docs/services/Registry?topic=registry-getting-started" target="_blank">documentation {{site.data.keyword.registrylong}} <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a>.</td>
</tr>
<tr>
  <td>{{site.data.keyword.keymanagementservicefull}}</td>
  <td>Chiffrez les valeurs confidentielles (secrets) Kubernetes figurant dans votre cluster en activant {{site.data.keyword.keymanagementserviceshort}}. Le chiffrement des valeurs confidentielles Kubernetes empêche des utilisateurs non autorisés d'accéder aux informations sensibles du cluster.<br>Pour configurer le chiffrement, voir <a href="/docs/containers?topic=containers-encryption#keyprotect">Chiffrement de valeurs confidentielles Kubernetes à l'aide de {{site.data.keyword.keymanagementserviceshort}}</a>.<br>Pour plus d'informations, voir la <a href="/docs/services/key-protect?topic=key-protect-getting-started-tutorial" target="_blank">documentation {{site.data.keyword.keymanagementserviceshort}} <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a>.</td>
</tr>
<tr>
<td>NeuVector</td>
<td>Vous pouvez protéger vos conteneurs avec un pare-feu natif pour le cloud à l'aide de <a href="https://neuvector.com/" target="_blank">NeuVector <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a>. Pour plus d'informations, voir <a href="https://www.ibm.com/us-en/marketplace/neuvector-container-security" target="_blank">NeuVector Container Security <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a>. </td>
</tr>
<tr>
<td>Twistlock</td>
<td>En complément de <a href="/docs/services/va?topic=va-va_index" target="_blank">Vulnerability Advisor</a>, vous pouvez utiliser <a href="https://www.twistlock.com/" target="_blank">Twistlock <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a> pour gérer les pare-feux, la protection contre les menaces et les réponses aux incidents. Pour plus d'informations, voir <a href="https://www.ibm.com/blogs/bluemix/2017/07/twistlock-ibm-bluemix-container-service/" target="_blank">Twistlock on {{site.data.keyword.containerlong_notm}} <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a>. </td>
</tr>
</tbody>
</table>

<br />



## Services de stockage
{: #storage_services}
<table summary="Le tableau présente les services disponibles que vous pouvez ajouter à votre cluster pour ajouter des capacités de stockage persistant. La lecture des lignes s'effectue de gauche à droite, avec le nom du service dans la première colonne, et une description du service dans la deuxième colonne.">
<caption>Services de stockage</caption>
<thead>
<tr>
<th>Service</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>Heptio Velero</td>
  <td>Vous pouvez utiliser <a href="https://github.com/heptio/velero" target="_blank">Heptio Velero <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a> pour effectuer une sauvegarde et restauration des ressources de cluster et des volumes persistants. Pour plus d'informations, voir la documentation Heptio Velero sur les <a href="https://github.com/heptio/velero/blob/release-0.9/docs/use-cases.md" target="_blank">cas d'utilisation pour la reprise après incident et la migration de cluster <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a>.</td>
</tr>
<tr>
  <td>{{site.data.keyword.Bluemix_notm}} Block Storage</td>
  <td>[{{site.data.keyword.Bluemix_notm}} Block Storage](/docs/infrastructure/BlockStorage?topic=BlockStorage-getting-started#getting-started) est une solution de stockage iSCSI persistant hautes performances que vous pouvez ajouter à vos applications en utilisant des volumes persistants (PV) Kubernetes. Utilisez du stockage par blocs pour déployer des applications avec état dans une zone unique ou sous forme de stockage haute performance pour des pods uniques. Pour plus d'informations sur la mise à disposition de stockage par blocs dans votre cluster, voir [Stockage de données sur {{site.data.keyword.Bluemix_notm}} Block Storage](/docs/containers?topic=containers-block_storage#block_storage)</td>
  </tr>
<tr>
  <td>{{site.data.keyword.cos_full}}</td>
  <td>Les données stockées avec {{site.data.keyword.cos_short}} sont chiffrées et disséminées entre plusieurs emplacements géographiques. Elles sont accessibles via HTTP en utilisant une API REST. Vous pouvez utiliser l'[image ibm-backup-restore](/docs/services/RegistryImages/ibm-backup-restore?topic=RegistryImages-ibmbackup_restore_starter) pour configurer le service de sorte à effectuer des sauvegardes ponctuelles ou planifiées pour les données figurant dans vos clusters. Pour obtenir des informations générales sur le service, voir la <a href="/docs/services/cloud-object-storage?topic=cloud-object-storage-about-ibm-cloud-object-storage" target="_blank">documentation {{site.data.keyword.cos_short}} <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a>.</td>
</tr>
  <tr>
  <td>{{site.data.keyword.Bluemix_notm}} File Storage</td>
  <td>[{{site.data.keyword.Bluemix_notm}} File Storage](/docs/infrastructure/FileStorage?topic=FileStorage-getting-started#getting-started) est une solution de stockage de fichiers NFS persistant, rapide et flexible en réseau que vous pouvez ajouter à vos applications en utilisant des volumes persistants Kubernetes. Vous pouvez choisir entre des niveaux de stockage prédéfinis avec des tailles en gigaoctets (Go) et un nombre d'opérations d'entrée-sortie par seconde (IOPS) répondant aux exigences de vos charges de travail. Pour plus d'informations sur la mise à disposition de stockage de fichiers dans votre cluster, voir [Stockage de données sur {{site.data.keyword.Bluemix_notm}} File Storage](/docs/containers?topic=containers-file_storage#file_storage).</td>
  </tr>
  <tr>
    <td>Portworx</td>
    <td>[Portworx ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://portworx.com/products/introduction/) est une solution de stockage SDS à haute disponibilité que vous pouvez utiliser pour gérer du stockage persistant pour vos bases de données conteneurisées et d'autres applications avec état, ou pour partager des données entre des pods sur plusieurs zones. Vous pouvez installer Portworx avec une charte Helm et mettre à disposition du stockage pour vos applications en utilisant des volumes persistants Kubernetes. Pour plus d'informations sur la configuration de Portworx dans votre cluster, voir [Stockage de données sur SDS (Software-Defined Storage) avec Portworx](/docs/containers?topic=containers-portworx#portworx).</td>
  </tr>
</tbody>
</table>

<br />


## Services de base de données
{: #database_services}

<table summary="Le tableau présente les services disponibles que vous pouvez ajouter à votre cluster pour ajouter des fonctionnalités de base de données. La lecture des lignes s'effectue de gauche à droite, avec le nom du service dans la première colonne et une description du service dans la deuxième colonne.">
<caption>Services de base de données</caption>
<thead>
<tr>
<th>Service</th>
<th>Description</th>
</tr>
</thead>
<tbody>
  <tr>
    <td>{{site.data.keyword.blockchainfull_notm}} Platform 2.0 bêta</td>
    <td>Déployez et gérez votre propre service {{site.data.keyword.blockchainfull_notm}} Platform sur {{site.data.keyword.containerlong_notm}}. Avec {{site.data.keyword.blockchainfull_notm}} Platform 2.0, vous pouvez héberger des réseaux {{site.data.keyword.blockchainfull_notm}} ou créer des organisations qui peuvent rejoindre d'autres réseaux {{site.data.keyword.blockchainfull_notm}} 2.0. Pour plus d'informations sur la configuration d'{{site.data.keyword.blockchainfull_notm}} dans {{site.data.keyword.containerlong_notm}}, voir [A propos d'{{site.data.keyword.blockchainfull_notm}} Platform 2.0 (version bêta gratuite)](/docs/services/blockchain?topic=blockchain-ibp-console-overview#ibp-console-overview).</td>
  </tr>
<tr>
  <td>Bases de données cloud</td>
  <td>Vous pouvez choisir entre toute une gamme de services de base de données {{site.data.keyword.Bluemix_notm}}, notamment {{site.data.keyword.composeForMongoDB_full}} ou {{site.data.keyword.cloudantfull}} pour déployer des solutions de base de données évolutives à haute disponibilité dans votre cluster. Pour obtenir une liste des bases de données disponibles, voir le [catalogue {{site.data.keyword.Bluemix_notm}} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/catalog?category=databases).  </td>
  </tr>
  </tbody>
  </table>


## Ajout de services {{site.data.keyword.Bluemix_notm}} à des clusters
{: #adding_cluster}

Ajoutez des services {{site.data.keyword.Bluemix_notm}} pour améliorer votre cluster Kubernetes en ajoutant des fonctionnalités supplémentaires dans des domaines tels que Watson AI, les données, la sécurité et Internet of Things (IoT).
{:shortdesc}

Vous ne pouvez lier que des services prenant en charge les clés de service. Pour trouver une liste avec des services compatibles avec les clés de service, voir [Utilisation des services {{site.data.keyword.Bluemix_notm}} avec des applications externes](/docs/resources?topic=resources-externalapp#externalapp).
{: note}

Avant de commencer :
- Vérifiez que vous disposez des rôles suivants :
    - [Rôle de service {{site.data.keyword.Bluemix_notm}} IAM **Editeur** ou **Administrateur** ](/docs/containers?topic=containers-users#platform) pour le cluster.
    - [Rôle de service {{site.data.keyword.Bluemix_notm}} IAM **Auteur** ou **Responsable**](/docs/containers?topic=containers-users#platform) pour l'espace de nom dans lequel vous souhaitez lier le service
    - [Rôle Cloud Foundry **Développeur**](/docs/iam?topic=iam-mngcf#mngcf) pour l'espace que vous souhaitez utiliser
- [Connectez-vous à votre compte. Ciblez la région appropriée et, le cas échéant, le groupe de ressources. Définissez le contexte de votre cluster](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).

Pour ajouter un service {{site.data.keyword.Bluemix_notm}} dans votre cluster :

1. [Créez une instance du service {{site.data.keyword.Bluemix_notm}}](/docs/resources?topic=resources-externalapp#externalapp).
    * Certains services {{site.data.keyword.Bluemix_notm}} ne sont disponibles que dans des régions éligibles. Vous pouvez lier un service à votre cluster uniquement si ce service est disponible dans la même région que votre cluster. En outre, si vous souhaitez créer une instance de service dans la zone Washington DC, vous devez utiliser l'interface CLI.
    * Vous devez créer l'instance de service dans le même groupe de ressources que votre cluster. Une ressource peut être créée dans un seul groupe de ressources que vous ne pouvez plus modifier par la suite.

2. Vérifiez le type de service que vous avez créé et notez le **nom** de l'instance de service.
   - **Services Cloud Foundry :**
     ```
     ibmcloud service list
     ```
     {: pre}

     Exemple de sortie :
     ```
     name                         service           plan    bound apps   last operation
     <cf_service_instance_name>   <service_name>    spark                create succeeded
     ```
     {: screen}

  - **Services activés par {{site.data.keyword.Bluemix_notm}} IAM :**
     ```
     ibmcloud resource service-instances
     ```
     {: pre}

     Exemple de sortie :
     ```
     Name                          Location   State    Type               Tags
     <iam_service_instance_name>   <region>   active   service_instance
     ```
     {: screen}

   Vous pouvez également voir les différents types de service dans votre tableau de bord sous **Services Cloud Foundry** et **Services**.

3. Identifiez l'espace de nom du cluster que vous désirez ajouter à votre service. Sélectionnez l'une des options suivantes.
     ```
     kubectl get namespaces
     ```
     {: pre}

4.  Ajoutez le service à votre cluster en utilisant la [commande](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_service_bind) `ibmcloud ks cluster-service-bind`. Pour les services activés par {{site.data.keyword.Bluemix_notm}} IAM, veillez à utiliser l'alias Cloud Foundry que vous avez créé précédemment. Pour les services activés par IAM, vous pouvez également utiliser le rôle d'accès au service par défaut **Auteur**, ou spécifier le rôle d'accès au service avec l'indicateur `--role`. La commande crée une clé de service pour l'instance de service, ou vous pouvez inclure l'indicateur `--key` pour utiliser les données d'identification de la clé de service existante. Si vous utilisez l'indicateur `--key`, n'incluez pas l'indicateur `--role`.
    ```
    ibmcloud ks cluster-service-bind --cluster <cluster_name_or_ID> --namespace <namespace> --service <service_instance_name> [--key <service_instance_key>] [--role <IAM_service_role>]
    ```
    {: pre}

    Une fois que le service a été correctement ajouté à votre cluster, une valeur confidentielle (secret) de cluster est créée pour héberger les données d'identification de votre instance de service. Les valeurs confidentielles sont chiffrées automatiquement dans etcd pour protéger vos données.

    Exemple de sortie :
    ```
    ibmcloud ks cluster-service-bind --cluster mycluster --namespace mynamespace --service cleardb
    Binding service instance to namespace...
    OK
    Namespace:	     mynamespace
    Secret name:     binding-<service_instance_name>
    ```
    {: screen}

5.  Vérifiez les données d'identification du service dans votre valeur confidentielle Kubernetes.
    1. Obtenez les détails de la valeur confidentielle et notez la valeur de **binding**. La valeur de **binding** est codée en base64 et contient les données d'identification pour votre instance de service au format JSON.
       ```
       kubectl get secrets binding-<service_instance_name> --namespace=<namespace> -o yaml
       ```
       {: pre}

       Exemple de sortie :
       ```
       apiVersion: v1
       data:
         binding: <binding>
       kind: Secret
       metadata:
         annotations:
           service-instance-id: 1111aaaa-a1aa-1aa1-1a11-111aa111aa11
           service-key-id: 2b22bb2b-222b-2bb2-2b22-b22222bb2222
         creationTimestamp: 2018-08-07T20:47:14Z
         name: binding-<service_instance_name>
         namespace: <namespace>
         resourceVersion: "6145900"
         selfLink: /api/v1/namespaces/default/secrets/binding-mycloudant
         uid: 33333c33-3c33-33c3-cc33-cc33333333c
       type: Opaque
       ```
       {: screen}

    2. Décodez la valeur de binding.
       ```
       echo "<binding>" | base64 -D
       ```
       {: pre}

       Exemple de sortie :
       ```
       {"apikey":"KL34Ys893284NGJEPFjgrioJ12NElpow","host":"98765aab-9ce1-7tr3-ba87-bfbab6e6d9d6-bluemix.cloudant.com","iam_apikey_description":"Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:cloudantnosqldb:us-south:a/1234g56789cfe8e6388dd2ec098:98746cw-43d7-49ce-947a-d8fe3eebb381::","iam_apikey_name":"auto-generated-apikey-1234abcde-987f-3t64-9d96-d13775ec5663","iam_role_crn":"crn:v1:bluemix:public:iam::::serviceRole:Writer","iam_serviceid_crn":"crn:v1:bluemix:public:iam-identity::a/1234567890brasge5htn2ec098::serviceid:ServiceId-12345vgh-6c4c-ytr12-af6b-467d30d6ef44","password":"jfiavhui12484fnivhuo472nvei23913c3ff","port":443,"url":"https://25c73aac-9ce1-4c24-ba98-bfbab6e6d9d6-bluemix:ugvioev823inreuiegn43donvri29989wiu9t22@25c73aac-9ce1-4c24-ba98-abdrjio123562lnsb-bluemix.cloudant.com","username":"123b45da-9ce1-4c24-ab12-rinwnwub1294-bluemix"}
       ```
       {: screen}

    3. Facultatif : comparez les données d'identification du service que vous avez décodées à l'étape précédente aux données d'identification du service que vous trouvez pour votre instance de service dans le tableau de bord {{site.data.keyword.Bluemix_notm}}.

6. Votre service étant désormais lié à votre cluster, vous devez configurer votre application pour [accéder aux données d'identification du service dans la valeur confidentielle Kubernetes](#adding_app).


## Accès aux données d'identification à partir de vos applications
{: #adding_app}

Pour accéder à une instance de service {{site.data.keyword.Bluemix_notm}} à partir de votre application, vous devez rendre les données d'identification stockées dans la valeur confidentielle Kubernetes accessibles à votre application.
{: shortdesc}

Les données d'identification d'une instance de service sont codées en base64 et stockées au format JSON dans votre valeur confidentielle. Pour accéder aux données figurant dans votre valeur confidentielle, choisissez l'une des options suivantes :
- [Monter la valeur confidentielle sous forme de volume sur votre pod](#mount_secret)
- [Référencer la valeur confidentielle dans les variables d'environnement](#reference_secret)
<br>
Vous voulez sécuriser davantage vos valeurs confidentielles ? Demandez à l'administrateur de votre cluster d'[activer {{site.data.keyword.keymanagementservicefull}}](/docs/containers?topic=containers-encryption#keyprotect) dans votre cluster pour chiffrer les valeurs confidentielles qu'elles soient nouvelles ou existantes, par exemple la valeur confidentielle dans laquelle sont stockées les données d'identification de vos instances de service {{site.data.keyword.Bluemix_notm}}.
{: tip}

Avant de commencer :
-  Vérifiez que vous disposez du [rôle de service {{site.data.keyword.Bluemix_notm}} IAM **Auteur** ou **Responsable**](/docs/containers?topic=containers-users#platform) pour l'espace de nom `kube-system`.
- [Connectez-vous à votre compte. Ciblez la région appropriée et, le cas échéant, le groupe de ressources. Définissez le contexte de votre cluster](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).
- [Ajoutez un service {{site.data.keyword.Bluemix_notm}} à votre cluster](#adding_cluster).

### Montage de la valeur confidentielle sous forme de volume dans votre pod
{: #mount_secret}

Lorsque vous montez la valeur confidentielle sous forme de volume dans votre pod, un fichier nommé `binding` est stocké dans le répertoire de montage du volume. Le fichier `binding` au format JSON comprend toutes les informations et les données d'identification dont vous avez besoin pour accéder au service {{site.data.keyword.Bluemix_notm}}.
{: shortdesc}

1.  Répertoriez les valeurs confidentielles disponibles dans votre cluster et notez le **nom** de la vôtre. Recherchez une valeur confidentielle de type **Opaque**. S'il existe plusieurs valeurs confidentielles, contactez l'administrateur de votre cluster pour identifier la valeur confidentielle appropriée.

    ```
    kubectl get secrets
    ```
    {: pre}

    Exemple de sortie :

    ```
    NAME                                    TYPE                                  DATA      AGE
    binding-<service_instance_name>         Opaque                                1         3m

    ```
    {: screen}

2.  Créez un fichier YAML pour votre déploiement Kubernetes et montez la valeur confidentielle sous forme de volume sur votre pod.
    ```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      labels:
        app: secret-test
      name: secret-test
      namespace: <my_namespace>
    spec:
      selector:
        matchLabels:
          app: secret-test
      replicas: 1
      template:
        metadata:
          labels:
            app: secret-test
        spec:
          containers:
          - image: registry.bluemix.net/ibmliberty:latest
            name: secret-test
            volumeMounts:
            - mountPath: <mount_path>
              name: <volume_name>
          volumes:
          - name: <volume_name>
            secret:
              defaultMode: 420
              secretName: binding-<service_instance_name>
    ```
    {: codeblock}

    <table>
    <caption>Description des composants du fichier YAML</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants du fichier YAML</th>
    </thead>
    <tbody>
    <tr>
    <td><code>volumeMounts.mountPath</code></td>
    <td>Chemin absolu du répertoire où est monté le volume dans le conteneur.</td>
    </tr>
    <tr>
    <td><code>volumeMounts.name</code></br><code>volumes.name</code></td>
    <td>Nom du volume à monter sur votre pod.</td>
    </tr>
    <tr>
    <td><code>secret.defaultMode</code></td>
    <td>Droits d'accès en lecture et écriture à la valeur confidentielle. Utilisez `420` pour définir les droits en lecture seule. </td>
    </tr>
    <tr>
    <td><code>secret.secretName</code></td>
    <td>Nom de la valeur confidentielle que vous avez notée à l'étape précédente.</td>
    </tr></tbody></table>

3.  Créez le pod et montez la valeur confidentielle sous forme de volume.
    ```
    kubectl apply -f secret-test.yaml
    ```
    {: pre}

4.  Vérifiez que le pod a bien été créé.
    ```
    kubectl get pods
    ```
    {: pre}

    Exemple de sortie d'interface CLI :

    ```
    NAME                           READY     STATUS    RESTARTS   AGE
    secret-test-1111454598-gfx32   1/1       Running   0          1m
    ```
    {: screen}

5.  Accédez aux données d'identification pour le service.
    1. Connectez-vous à votre pod.
       ```
       kubectl exec <pod_name> -it bash
       ```
       {: pre}

    2. Accédez au chemin de montage du volume que vous avez défini précédemment et répertoriez les fichiers figurant dans ce chemin.
       ```
       cd <volume_mountpath> && ls
       ```
       {: pre}

       Exemple de sortie :
       ```
       binding
       ```
       {: screen}

       Le fichier `binding` comprend les données d'identification pour le service que vous avez stockées dans la valeur confidentielle Kubernetes.

    4. Affichez les données d'identification du service. Ces données sont stockées sous forme de paires clé-valeur au format JSON.
       ```
       cat binding
       ```
       {: pre}

       Exemple de sortie :
       ```
       {"apikey":"KL34Ys893284NGJEPFjgrioJ12NElpow","host":"98765aab-9ce1-7tr3-ba87-bfbab6e6d9d6-bluemix.cloudant.com","iam_apikey_description":"Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:cloudantnosqldb:us-south:a/1234g56789cfe8e6388dd2ec098:98746cw-43d7-49ce-947a-d8fe3eebb381::","iam_apikey_name":"auto-generated-apikey-1234abcde-987f-3t64-9d96-d13775ec5663","iam_role_crn":"crn:v1:bluemix:public:iam::::serviceRole:Writer","iam_serviceid_crn":"crn:v1:bluemix:public:iam-identity::a/1234567890brasge5htn2ec098::serviceid:ServiceId-12345vgh-6c4c-ytr12-af6b-467d30d6ef44","password":"jfiavhui12484fnivhuo472nvei23913c3ff","port":443,"url":"https://25c73aac-9ce1-4c24-ba98-bfbab6e6d9d6-bluemix:ugvioev823inreuiegn43donvri29989wiu9t22@25c73aac-9ce1-4c24-ba98-abdrjio123562lnsb-bluemix.cloudant.com","username":"123b45da-9ce1-4c24-ab12-rinwnwub1294-bluemix"}
       ```
       {: screen}

    5. Configurez votre application pour analyser le contenu JSON et extraire les informations dont vous avez besoin pour accéder à votre service.


### Référencement de la valeur confidentielle dans les variables d'environnement
{: #reference_secret}

Vous pouvez ajouter les données d'identification du service et d'autres paires clé-valeur figurant dans votre valeur confidentielle Kubernetes sous forme de variables d'environnement dans votre déploiement.
{: shortdesc}

1. Répertoriez les valeurs confidentielles disponibles dans votre cluster et notez le **nom** de la vôtre. Recherchez une valeur confidentielle de type **Opaque**. S'il existe plusieurs valeurs confidentielles, contactez l'administrateur de votre cluster pour identifier la valeur confidentielle appropriée.

    ```
    kubectl get secrets
    ```
    {: pre}

    Exemple de sortie :

    ```
    NAME                                    TYPE                                  DATA      AGE
    binding-<service_instance_name>         Opaque                                1         3m
    ```
    {: screen}

2. Obtenez les détails de votre valeur confidentielle pour rechercher les paires clé-valeur potentielles que vous pouvez référencer sous forme de variables d'environnement dans votre pod. Les données d'identification du service sont stockées dans la clé `binding` de votre valeur confidentielle.
   ```
   kubectl get secrets binding-<service_instance_name> --namespace=<namespace> -o yaml
   ```
   {: pre}

   Exemple de sortie :
   ```
   apiVersion: v1
   data:
     binding: <binding>
   kind: Secret
   metadata:
     annotations:
       service-instance-id: 7123acde-c3ef-4ba2-8c52-439ac007fa70
       service-key-id: 9h30dh8a-023f-4cf4-9d96-d12345ec7890
     creationTimestamp: 2018-08-07T20:47:14Z
     name: binding-<service_instance_name>
     namespace: <namespace>
     resourceVersion: "6145900"
     selfLink: /api/v1/namespaces/default/secrets/binding-mycloudant
     uid: 12345a31-9a83-11e8-ba83-cd49014748f
   type: Opaque
   ```
   {: screen}

3. Créez un fichier YAML pour votre déploiement Kubernetes et indiquez une variable d'environnement qui référence la clé `binding`.
   ```
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     labels:
       app: secret-test
     name: secret-test
     namespace: <my_namespace>
   spec:
     selector:
       matchLabels:
         app: secret-test
     template:
       metadata:
         labels:
           app: secret-test
       spec:
         containers:
         - image: registry.bluemix.net/ibmliberty:latest
           name: secret-test
           env:
           - name: BINDING
             valueFrom:
               secretKeyRef:
                 name: binding-<service_instance_name>
                 key: binding
     ```
     {: codeblock}

     <table>
     <caption>Description des composants du fichier YAML</caption>
     <thead>
     <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants du fichier YAML</th>
     </thead>
     <tbody>
     <tr>
     <td><code>containers.env.name</code></td>
     <td>Nom de votre variable d'environnement.</td>
     </tr>
     <tr>
     <td><code>env.valueFrom.secretKeyRef.name</code></td>
     <td>Nom de la valeur confidentielle que vous avez notée à l'étape précédente.</td>
     </tr>
     <tr>
     <td><code>env.valueFrom.secretKeyRef.key</code></td>
     <td>Clé faisant partie de votre valeur confidentielle que vous voulez référencer dans votre variable d'environnement. Pour référencer les données d'identification du service, vous devez utiliser la clé <strong>binding</strong>.  </td>
     </tr>
     </tbody></table>

4. Créez le pod qui référence la clé `binding` de votre valeur confidentielle sous forme de variable d'environnement.
   ```
   kubectl apply -f secret-test.yaml
   ```
   {: pre}

5. Vérifiez que le pod a bien été créé.
   ```
   kubectl get pods
   ```
   {: pre}

   Exemple de sortie d'interface CLI :
   ```
   NAME                           READY     STATUS    RESTARTS   AGE
    secret-test-1111454598-gfx32   1/1       Running   0          1m
   ```
   {: screen}

6. Vérifiez que la variable d'environnement est définie correctement.
   1. Connectez-vous à votre pod.
      ```
      kubectl exec <pod_name> -it bash
      ```
      {: pre}

   2. Répertoriez toutes les variables d'environnement dans le pod.
      ```
      env
      ```
      {: pre}

      Exemple de sortie :
      ```
      BINDING={"apikey":"KL34Ys893284NGJEPFjgrioJ12NElpow","host":"98765aab-9ce1-7tr3-ba87-bfbab6e6d9d6-bluemix.cloudant.com","iam_apikey_description":"Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:cloudantnosqldb:us-south:a/1234g56789cfe8e6388dd2ec098:98746cw-43d7-49ce-947a-d8fe3eebb381::","iam_apikey_name":"auto-generated-apikey-1234abcde-987f-3t64-9d96-d13775ec5663","iam_role_crn":"crn:v1:bluemix:public:iam::::serviceRole:Writer","iam_serviceid_crn":"crn:v1:bluemix:public:iam-identity::a/1234567890brasge5htn2ec098::serviceid:ServiceId-12345vgh-6c4c-ytr12-af6b-467d30d6ef44","password":"jfiavhui12484fnivhuo472nvei23913c3ff","port":443,"url":"https://25c73aac-9ce1-4c24-ba98-bfbab6e6d9d6-bluemix:ugvioev823inreuiegn43donvri29989wiu9t22@25c73aac-9ce1-4c24-ba98-abdrjio123562lnsb-bluemix.cloudant.com","username":"123b45da-9ce1-4c24-ab12-rinwnwub1294-bluemix"}
      ```
      {: screen}

7. Configurez votre application pour lire la variable d'environnement et analyser le contenu JSON pour extraire les informations dont vous avez besoin pour accéder à votre service.

   Exemple de code en Python :
   ```
   if os.environ.get('BINDING'):
        credentials = json.loads(os.environ.get('BINDING'))
   ```
   {: codeblock}

## Configuration de Helm dans {{site.data.keyword.containerlong_notm}}
{: #helm}

[Helm ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://helm.sh) est un gestionnaire de package Kubernetes. Vous pouvez créer des chartes Helm ou utiliser des chartes Helm préexistantes pour définir, installer et mettre à niveau des applications Kubernetes complexes qui s'exécutent dans les clusters {{site.data.keyword.containerlong_notm}}.
{:shortdesc}

Pour déployer des chartes Helm, vous devez installer l'interface de ligne de commande (CLI) Helm sur votre machine locale et installer le serveur Helm Tiller dans votre cluster. L'image de Tiller est stockée dans le registre public Google Container Registry. Pour accéder à l'image lors de l'installation de Tiller, votre cluster doit autoriser la connectivité de réseau public au registre public Google Container Registry. Les clusters avec le noeud final de service public activé peuvent accéder automatiquement à l'image. Les clusters privés protégés avec un pare-feu personnalisé ou les clusters ayant activé le noeud final de service privé uniquement, n'autorisent pas l'accès à l'image de Tiller. Vous pouvez à la place [extraire l'image sur votre machine locale et l'insérer dans votre espace de nom dans {{site.data.keyword.registryshort_notm}}](#private_local_tiller) ou [installer des chartes Helm sans utiliser le serveur Tiller](#private_install_without_tiller).
{: note}

### Configuration de Helm dans un cluster avec accès public
{: #public_helm_install}

Si votre cluster a activé le noeud final de service public, vous pouvez installer Tiller en utilisant l'image publique figurant dans Google Container Registry.
{: shortdesc}

Avant de commencer : [connectez-vous à votre compte. Ciblez la région appropriée et, le cas échéant, le groupe de ressources. Définissez le contexte de votre cluster](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).

1. Installez l'<a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">interface de ligne de commande Helm  <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a>.

2. **Important** : pour assurer la sécurité du cluster, créez un compte de service pour Tiller dans l'espace de nom `kube-system` et une liaison de rôle de cluster RBAC Kubernetes pour le pod `tiller-deploy` en appliquant le fichier `.yaml` suivant à partir du [référentiel `kube-samples` d'{{site.data.keyword.Bluemix_notm}}](https://github.com/IBM-Cloud/kube-samples/blob/master/rbac/serviceaccount-tiller.yaml). **Remarque** : pour installer Tiller avec le compte de service et la liaison de rôle de cluster dans l'espace de nom `kube-system`, vous devez disposer du [rôle `cluster-admin`](/docs/containers?topic=containers-users#access_policies).
    ```
    kubectl apply -f https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/rbac/serviceaccount-tiller.yaml
    ```
    {: pre}

3. Initialisez Helm et installez Tiller avec le compte de service que vous avez créé.

    ```
    helm init --service-account tiller
    ```
    {: pre}

4.  Vérifiez que l'installation a abouti.
    1.  Vérifiez que le compte de service Tiller est créé.
        ```
        kubectl get serviceaccount -n kube-system tiller
        ```
        {: pre}

        Exemple de sortie :

        ```
        NAME                                 SECRETS   AGE
        tiller                               1         2m
        ```
        {: screen}

    2.  Vérifiez que la zone **Status** du pod `tiller-deploy` indique `Running` dans votre cluster.
        ```
        kubectl get pods -n kube-system -l app=helm
        ```
        {: pre}

        Exemple de sortie :

        ```
        NAME                            READY     STATUS    RESTARTS   AGE
    tiller-deploy-352283156-nzbcm   1/1       Running   0          2m
        ```
        {: screen}

5. Ajoutez les référentiels Helm d'{{site.data.keyword.Bluemix_notm}} dans votre instance Helm.
   ```
   helm repo add ibm  https://registry.bluemix.net/helm/ibm
   ```
   {: pre}

   ```
   helm repo add ibm-charts https://registry.bluemix.net/helm/ibm-charts
   ```
   {: pre}

6. Mettez à jour les référentiels pour extraire les dernières versions de toutes les chartes Helm.
   ```
   helm repo update
   ```
   {: pre}

7. Répertoriez les chartes Helm actuellement disponibles dans les référentiels {{site.data.keyword.Bluemix_notm}}.
   ```
   helm search ibm
   ```
   {: pre}

   ```
   helm search ibm-charts
   ```
   {: pre}

8. Identifiez la charte Helm que vous voulez installer et suivez les instructions indiquées dans le fichier `README` de la charte pour installer la charte Helm dans votre cluster.


### Clusters privés : Insertion de l'image de Tiller dans votre registre privé dans {{site.data.keyword.registryshort_notm}}
{: #private_local_tiller}

Vous pouvez extraire l'image de Tiller sur votre machine locale, puis l'insérer dans votre espace de nom dans {{site.data.keyword.registryshort_notm}} et laissez Helm installer Tiller en utilisant l'image dans {{site.data.keyword.registryshort_notm}}.
{: shortdesc}

Avant de commencer :
- Installez Docker sur votre machine locale. Si vous avez installé l'[interface de ligne de commande {{site.data.keyword.Bluemix_notm}}](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli), Docker est déjà installé.
- [Installez le plug-in de l'interface de ligne de commande {{site.data.keyword.registryshort_notm}} et configurez un espace de nom](/docs/services/Registry?topic=registry-getting-started#gs_registry_cli_install).

Pour installer Tiller en utilisant {{site.data.keyword.registryshort_notm}} :

1. Installez l'<a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">interface de ligne de commande (CLI) Helm <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a> sur votre machine locale.
2. Connectez-vous à votre cluster privé à l'aide du tunnel VPN de l'infrastructure {{site.data.keyword.Bluemix_notm}} que vous avez configuré.
3. **Important** : pour assurer la sécurité du cluster, créez un compte de service pour Tiller dans l'espace de nom `kube-system` et une liaison de rôle de cluster RBAC Kubernetes pour le pod `tiller-deploy` en appliquant le fichier `.yaml` suivant à partir du [référentiel `kube-samples` d'{{site.data.keyword.Bluemix_notm}}](https://github.com/IBM-Cloud/kube-samples/blob/master/rbac/serviceaccount-tiller.yaml). **Remarque** : pour installer Tiller avec le compte de service et la liaison de rôle de cluster dans l'espace de nom `kube-system`, vous devez disposer du [rôle `cluster-admin`](/docs/containers?topic=containers-users#access_policies).
    1. [Obtenez les fichiers YAML du compte de service et de liaison de rôle de cluster Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/rbac/serviceaccount-tiller.yaml).

    2. Créez les ressources Kubernetes dans votre cluster.
       ```
       kubectl apply -f service-account.yaml
       ```
       {: pre}

       ```
       kubectl apply -f cluster-role-binding.yaml
       ```
       {: pre}

4. [Recherchez la version de Tiller ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://console.cloud.google.com/gcr/images/kubernetes-helm/GLOBAL/tiller?gcrImageListsize=30) que vous souhaitez installer dans votre cluster. Si vous n'avez pas besoin d'une version particulière, utilisez la version la plus récente.

5. Extrayez l'image de Tiller sur votre machine locale.
   ```
   docker pull gcr.io/kubernetes-helm/tiller:v2.11.0
   ```
   {: pre}

   Exemple de sortie :
   ```
   docker pull gcr.io/kubernetes-helm/tiller:v2.13.0
   v2.13.0: Pulling from kubernetes-helm/tiller
   48ecbb6b270e: Pull complete
   d3fa0712c71b: Pull complete
   bf13a43b92e9: Pull complete
   b3f98be98675: Pull complete
   Digest: sha256:c4bf03bb67b3ae07e38e834f29dc7fd43f472f67cad3c078279ff1bbbb463aa6
   Status: Downloaded newer image for gcr.io/kubernetes-helm/tiller:v2.13.0
   ```
   {: screen}

6. [Insérez l'image de Tiller dans votre espace de nom dans {{site.data.keyword.registryshort_notm}}](/docs/services/Registry?topic=registry-getting-started#gs_registry_images_pushing).

7. [Copiez la valeur confidentielle d'extraction d'image (imagePullSecret) pour accéder à {{site.data.keyword.registryshort_notm}} depuis votre espace de nom par défaut, dans l'espace de nom `kube-system`](/docs/containers?topic=containers-images#copy_imagePullSecret).

8. Installez Tiller dans votre cluster privé en utilisant l'image que vous avez stockée dans votre espace de nom dans {{site.data.keyword.registryshort_notm}}.
   ```
   helm init --tiller-image <region>.icr.io/<mynamespace>/<myimage>:<tag> --service-account tiller
   ```
   {: pre}

9. Ajoutez les référentiels Helm d'{{site.data.keyword.Bluemix_notm}} dans votre instance Helm.
   ```
   helm repo add ibm  https://registry.bluemix.net/helm/ibm
   ```
   {: pre}

   ```
   helm repo add ibm-charts https://registry.bluemix.net/helm/ibm-charts
   ```
   {: pre}

10. Mettez à jour les référentiels pour extraire les dernières versions de toutes les chartes Helm.
    ```
    helm repo update
    ```
    {: pre}

11. Répertoriez les chartes Helm actuellement disponibles dans les référentiels {{site.data.keyword.Bluemix_notm}}.
    ```
    helm search ibm
    ```
    {: pre}

    ```
    helm search ibm-charts
    ```
    {: pre}

12. Identifiez la charte Helm que vous voulez installer et suivez les instructions indiquées dans le fichier `README` de la charte pour installer la charte Helm dans votre cluster.

### Clusters privés : Installation des chartes Helm sans utiliser Tiller
{: #private_install_without_tiller}

Si vous ne souhaitez pas installer Tiller dans votre cluster privé, vous pouvez créer manuellement les fichiers YAML de la charte Helm et les appliquer en utilisant des commandes `kubectl`.
{: shortdesc}

Les étapes indiquées dans cet exemple montrent comment installer des chartes Helm à partir des référentiels de chartes Helm d'{{site.data.keyword.Bluemix_notm}} dans votre cluster privé. Si vous voulez installer une charte Helm qui n'est pas stockée dans l'un des référentiels de chartes Helm d'{{site.data.keyword.Bluemix_notm}}, vous devez suivre les instructions de cette rubrique pour créer les fichiers YAML pour votre charte Helm. Par ailleurs, vous devez télécharger l'image de la charte Helm à partir du registre de conteneur public, l'insérer dans votre espace de nom dans {{site.data.keyword.registryshort_notm}} et mettre à jour le fichier `values.yaml` pour utiliser cette image dans {{site.data.keyword.registryshort_notm}}.
{: note}

1. Installez l'<a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">interface de ligne de commande (CLI) Helm <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a> sur votre machine locale.
2. Connectez-vous à votre cluster privé à l'aide du tunnel VPN de l'infrastructure {{site.data.keyword.Bluemix_notm}} que vous avez configuré.
3. Ajoutez les référentiels Helm d'{{site.data.keyword.Bluemix_notm}} dans votre instance Helm.
   ```
   helm repo add ibm  https://registry.bluemix.net/helm/ibm
   ```
   {: pre}

   ```
   helm repo add ibm-charts https://registry.bluemix.net/helm/ibm-charts
   ```
   {: pre}

4. Mettez à jour les référentiels pour extraire les dernières versions de toutes les chartes Helm.
   ```
   helm repo update
   ```
   {: pre}

5. Répertoriez les chartes Helm actuellement disponibles dans les référentiels {{site.data.keyword.Bluemix_notm}}.
   ```
   helm search ibm
   ```
   {: pre}

   ```
   helm search ibm-charts
   ```
   {: pre}

6. Identifiez la charte Helm que vous voulez installer, téléchargez la charte Helm sur votre machine locale et décompressez les fichiers de votre charte Helm. L'exemple suivant montre comment télécharger la charte Helm pour le programme de mise à l'échelle automatique de cluster (cluster autoscaler) version 1.0.3 et décompresser les fichiers dans un répertoire `cluster-autoscaler`.
   ```
   helm fetch ibm/ibm-iks-cluster-autoscaler --untar --untardir ./cluster-autoscaler --version 1.0.3
   ```
   {: pre}

7. Accédez au répertoire dans lequel vous avez décompressé les fichiers de la charte Helm.
   ```
   cd cluster-autoscaler
   ```
   {: pre}

8. Créez un répertoire `output` pour les fichiers YAML que vous générez à l'aide des fichiers dans votre charte Helm.
   ```
   mkdir output
   ```
   {: pre}

9. Ouvrez le fichier `values.yaml` et apportez toutes les modifications requises par les instructions d'installation de la charte Helm.
   ```
   nano ibm-iks-cluster-autoscaler/values.yaml
   ```
   {: pre}

10. Utilisez votre installation Helm locale pour créer tous les fichiers YAML Kubernetes pour votre charte Helm. Les fichiers YAML sont stockés dans le répertoire `output` que vous avez créé précédemment.
    ```
    helm template --values ./ibm-iks-cluster-autoscaler/values.yaml --output-dir ./output ./ibm-iks-cluster-autoscaler
    ```
    {: pre}

    Exemple de sortie :
    ```
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-configmap.yaml
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-service-account-roles.yaml
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-service.yaml
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-deployment.yaml
    ```
    {: screen}

11. Déployez tous les fichiers YAML dans votre cluster privé.
    ```
    kubectl apply --recursive --filename ./output
    ```
   {: pre}

12. Facultatif : supprimez tous les fichiers YAML du répertoire `output`.
    ```
    kubectl delete --recursive --filename ./output
    ```
    {: pre}


### Liens associés à Helm
{: #helm_links}

* Pour utiliser la charte Helm strongSwan, voir [Configuration de la connectivité VPN avec la charte Helm du service VPN IPsec strongSwan](/docs/containers?topic=containers-vpn#vpn-setup).
* Affichez les chartes Helm que vous pouvez utiliser avec {{site.data.keyword.Bluemix_notm}} dans le [Catalogue de chartes Helm ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/kubernetes/solutions/helm-charts) dans la console.
* Pour en savoir plus sur les commandes Helm utilisées pour configurer et gérer des chartes Helm, voir la <a href="https://docs.helm.sh/helm/" target="_blank">documentation Helm <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a>.
* Pour en savoir plus sur comment augmenter la vitesse de déploiement avec les chartes Helm de Kubernetes, voir [Increase deployment velocity with Kubernetes Helm Charts ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://developer.ibm.com/recipes/tutorials/increase-deployment-velocity-with-kubernetes-helm-charts/).

## Visualisation de ressources de cluster Kubernetes
{: #weavescope}

Weave Scope produit un diagramme visuel de vos ressources dans un cluster Kubernetes, notamment des services, pods, conteneurs, etc. Weave Scope fournit des métriques interactives sur l'utilisation de l'UC et de la mémoire, ainsi que des outils pour suivi et exécution dans un conteneur.
{:shortdesc}

Avant de commencer :

-   Prenez soin de ne pas exposer les informations de votre cluster sur l'Internet public. Procédez comme suit pour déployer de manière sécurisée Weave Scope et y accéder en local depuis un navigateur Web.
-   Si ce n'est déjà fait, [créez un cluster standard](/docs/containers?topic=containers-clusters#clusters_ui). Weave Scope peut solliciter énormément l'unité centrale, surtout l'application. Exécutez Weave Scope avec des clusters standard plus volumineux et non pas des clusters gratuits.
-  Vérifiez que vous disposez du [rôle de service {{site.data.keyword.Bluemix_notm}} IAM **Manager**](/docs/containers?topic=containers-users#platform) pour tous les espaces de nom.
-   [Connectez-vous à votre compte. Ciblez la région appropriée et, le cas échéant, le groupe de ressources. Définissez le contexte de votre cluster](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).


Pour utiliser Weave Scope avec un cluster, procédez comme suit :
2.  Déployez dans le cluster l'un des fichiers de configuration d'autorisations RBAC fournis.

    Pour activer les droits d'accès en lecture / écriture, exécutez la commande suivante :

    ```
    kubectl apply -f "https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/weave-scope/weave-scope-rbac.yaml"
    ```
    {: pre}

    Pour activer les droits d'accès en lecture seule, exécutez la commande suivante :

    ```
    kubectl apply --namespace weave -f "https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/weave-scope/weave-scope-rbac-readonly.yaml"
    ```
    {: pre}

    Sortie :

    ```
    clusterrole "weave-scope-mgr" created
    clusterrolebinding "weave-scope-mgr-role-binding" created
    ```
    {: screen}

3.  Déployez le service Weave Scope, lequel est accessible en mode privé via l'adresse IP du cluster.

    <pre class="pre">
    <code>kubectl apply --namespace weave -f "https://cloud.weave.works/k8s/scope.yaml?k8s-version=$(kubectl version | base64 | tr -d '&bsol;n')"</code>
    </pre>

    Sortie :

    ```
    serviceaccount "weave-scope" created
    deployment "weave-scope-app" created
    service "weave-scope-app" created
    daemonset "weave-scope-agent" created
    ```
    {: screen}

4.  Exécutez une commande d'acheminement de port pour ouvrir le service sur votre ordinateur. La prochaine fois que vous accéderez à Weave Scope, vous pourrez exécuter cette commande d'acheminement de port sans avoir à compléter à nouveau les étapes de configuration précédentes.

    ```
    kubectl port-forward -n weave "$(kubectl get -n weave pod --selector=weave-scope-component=app -o jsonpath='{.items..metadata.name}')" 4040
    ```
    {: pre}

    Sortie :

    ```
    Forwarding from 127.0.0.1:4040 -> 4040
    Forwarding from [::1]: :1]:4040 -> 4040
    Handling connection for 4040
    ```
    {: screen}

5.  Ouvrez votre navigateur à l'adresse `http://localhost:4040`. Si les composants par défaut ne sont pas déployés, vous obtenez le diagramme suivant. Vous pouvez choisir d'afficher des diagrammes de topologie ou bien des tableaux des ressources Kubernetes dans le cluster.

     <img src="images/weave_scope.png" alt="Exemple de topologie Weave Scope" style="width:357px;" />


[En savoir plus sur les fonctions Weave Scope ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.weave.works/docs/scope/latest/features/).

<br />

