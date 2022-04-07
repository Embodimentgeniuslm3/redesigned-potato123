---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

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
{:preview: .preview}f


# Intégrations IBM Cloud et tierces prises en charge
{: #supported_integrations}

Vous pouvez utiliser différents services externes et services de catalogue avec un cluster Kubernetes standard dans {{site.data.keyword.containerlong}}.
{:shortdesc}

## Intégrations populaires
{: #popular_services}

<table summary="Le tableau présente les services disponibles que vous pouvez ajouter à votre cluster et qui ont le plus de succès auprès des utilisateurs d'{{site.data.keyword.containerlong_notm}}. La lecture des lignes s'effectue de gauche à droite, avec le nom du service dans la première colonne et une description de ce service dans la seconde colonne.">
<caption>Services populaires</caption>
<thead>
<tr>
<th>Service</th>
<th>Catégorie </th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.cloudaccesstrailfull}}</td>
<td>Journaux d'activité de cluster</td>
<td>Surveillez les activités d'administration intervenant dans votre cluster en analysant les journaux via Grafana. Pour plus d'informations sur ce service, voir la documentation d'[Activity Tracker](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started). Pour plus d'informations sur les types d'événement dont vous pouvez assurer le suivi, voir [Evénements Activity Tracker](/docs/containers?topic=containers-at_events).</td>
</tr>
<tr>
<td>{{site.data.keyword.appid_full}}</td>
<td>Authentification</td>
<td>Ajoutez un niveau de sécurité à vos applications avec [{{site.data.keyword.appid_short}}](/docs/services/appid?topic=appid-getting-started) en obligeant les utilisateurs à se connecter. Pour l'authentification de demandes HTTP/HTTPS Web ou d'API vers votre application, vous pouvez intégrer {{site.data.keyword.appid_short_notm}} au service Ingress en utilisant l'[annotation Ingress d'authentification {{site.data.keyword.appid_short_notm}}](/docs/containers?topic=containers-ingress_annotation#appid-auth).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix}} Block Storage</td>
<td>Stockage par blocs</td>
<td>[{{site.data.keyword.Bluemix_notm}} Block Storage](/docs/infrastructure/BlockStorage?topic=BlockStorage-getting-started#getting-started) est une solution de stockage iSCSI persistant hautes performances que vous pouvez ajouter à vos applications en utilisant des volumes persistants (PV) Kubernetes. Utilisez du stockage par blocs pour déployer des applications avec état dans une zone unique ou sous forme de stockage haute performance pour des pods uniques. Pour plus d'informations sur la mise à disposition de stockage par blocs dans votre cluster, voir [Stockage de données sur {{site.data.keyword.Bluemix_notm}} Block Storage](/docs/containers?topic=containers-block_storage#block_storage)</td>
</tr>
<tr>
<td>{{site.data.keyword.cloudcerts_full}}</td>
<td>Certificats TLS</td>
<td>Vous pouvez utiliser <a href="/docs/services/certificate-manager?topic=certificate-manager-getting-started#getting-started" target="_blank">{{site.data.keyword.cloudcerts_long}} <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a> pour stocker et gérer les certificats SSL pour vos applications. Pour plus d'informations, voir <a href="https://www.ibm.com/blogs/bluemix/2018/01/use-ibm-cloud-certificate-manager-ibm-cloud-container-service-deploy-custom-domain-tls-certificates/" target="_blank">Use {{site.data.keyword.cloudcerts_long_notm}} with {{site.data.keyword.containerlong_notm}} to deploy custom domain TLS Certificates <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a>. </td>
</tr>
<tr>
<td>{{site.data.keyword.registrylong}}</td>
<td>Images de conteneur</td>
<td>Configurez votre propre référentiel d'images Docker sécurisé où vous pourrez stocker et partager des images en toute sécurité entre les utilisateurs du cluster. Pour plus d'informations, voir la <a href="/docs/services/Registry?topic=registry-getting-started" target="_blank">documentation {{site.data.keyword.registrylong}} <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a>.</td>
</tr>
<tr>
<td>{{site.data.keyword.contdelivery_full}}</td>
<td>Automatisation de la génération</td>
<td>Permet d'automatiser vos générations d'applications et vos déploiements de conteneur sur les clusters Kubernetes en utilisant une chaîne d'outils. Pour plus d'informations sur la configuration, voir le blogue <a href="https://developer.ibm.com/recipes/tutorials/deploy-kubernetes-pods-to-the-bluemix-container-service-using-devops-pipelines/" target="_blank">Deploy Kubernetes pods to the {{site.data.keyword.containerlong_notm}} using DevOps Pipelines <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a>. </td>
</tr>
<tr>
<td>{{site.data.keyword.datashield_full}} (bêta)</td>
<td>Chiffrement de la mémoire</td>
<td>Vous pouvez utiliser <a href="/docs/services/data-shield?topic=data-shield-getting-started#getting-started" target="_blank">{{site.data.keyword.datashield_short}} <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a> pour chiffrer votre mémoire de données. {{site.data.keyword.datashield_short}} est intégré à la technologie Intel® Software Guard Extensions (SGX) et Fortanix® pour que le code et les données de vos charges de travail de conteneur {{site.data.keyword.Bluemix_notm}} soient protégés lors de leur utilisation. Le code d'application et les données s'exécutent dans des enclaves d'UC fortifiées, qui correspondent à des zones de mémoire sécurisées sur le noeud worker qui protègent les aspects critiques de l'application, ce qui permet de conserver le code et les données confidentiels et inchangés.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix}} File Storage</td>
<td>Stockage de fichiers</td>
<td>[{{site.data.keyword.Bluemix_notm}} File Storage](/docs/infrastructure/FileStorage?topic=FileStorage-getting-started#getting-started) est une solution de stockage de fichiers NFS persistant, rapide et flexible en réseau que vous pouvez ajouter à vos applications en utilisant des volumes persistants Kubernetes. Vous pouvez choisir entre des niveaux de stockage prédéfinis avec des tailles en gigaoctets (Go) et un nombre d'opérations d'entrée-sortie par seconde (IOPS) répondant aux exigences de vos charges de travail. Pour plus d'informations sur la mise à disposition de stockage de fichiers dans votre cluster, voir [Stockage de données sur {{site.data.keyword.Bluemix_notm}} File Storage](/docs/containers?topic=containers-file_storage#file_storage).</td>
</tr>
<tr>
<td>{{site.data.keyword.keymanagementservicefull}}</td>
<td>chiffrement de données</td>
<td>Chiffrez les valeurs confidentielles (secrets) Kubernetes figurant dans votre cluster en activant {{site.data.keyword.keymanagementserviceshort}}. Le chiffrement des valeurs confidentielles Kubernetes empêche des utilisateurs non autorisés d'accéder aux informations sensibles du cluster.<br>Pour configurer le chiffrement, voir <a href="/docs/containers?topic=containers-encryption#keyprotect">Chiffrement de valeurs confidentielles Kubernetes à l'aide de {{site.data.keyword.keymanagementserviceshort}}</a>.<br>Pour plus d'informations, voir la <a href="/docs/services/key-protect?topic=key-protect-getting-started-tutorial" target="_blank">documentation {{site.data.keyword.keymanagementserviceshort}} <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a>.</td>
</tr>
<tr>
<td>{{site.data.keyword.la_full}}</td>
<td>Journaux de cluster et d'application</td>
<td>Ajoutez des fonctionnalités de gestion de journaux à votre cluster en déployant LogDNA en tant que service tiers sur vos noeuds worker afin de gérer les journaux à partir de vos conteneurs de pod. Pour plus d'informations, voir [Managing Kubernetes cluster logs with {{site.data.keyword.loganalysisfull_notm}} with LogDNA](/docs/services/Log-Analysis-with-LogDNA/tutorials?topic=LogDNA-kube#kube).</td>
</tr>
<tr>
<td>{{site.data.keyword.mon_full}}</td>
<td>Métriques de cluster et d'application</td>
<td>Gagnez en visibilité opérationnelle sur les performances et l'état de santé de vos applications en déployant Sysdig en tant que service tiers sur vos noeuds worker pour transférer des métriques à {{site.data.keyword.monitoringlong}}. Pour plus d'informations, voir [Analyzing metrics for an app that is deployed in a Kubernetes cluster](/docs/services/Monitoring-with-Sysdig/tutorials?topic=Sysdig-kubernetes_cluster#kubernetes_cluster). </td>
</tr>
<tr>
<td>{{site.data.keyword.cos_full}}</td>
<td>Stockage d'objets</td>
<td>Les données stockées avec {{site.data.keyword.cos_short}} sont chiffrées et disséminées entre plusieurs emplacements géographiques. Elles sont accessibles via HTTP en utilisant une API REST. Vous pouvez utiliser l'[image ibm-backup-restore](/docs/services/RegistryImages/ibm-backup-restore?topic=RegistryImages-ibmbackup_restore_starter) pour configurer le service de sorte à effectuer des sauvegardes ponctuelles ou planifiées pour les données figurant dans vos clusters. Pour plus d'informations sur le service, voir la <a href="/docs/services/cloud-object-storage?topic=cloud-object-storage-about" target="_blank">documentation {{site.data.keyword.cos_short}} <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a>.</td>
</tr>
<tr>
<td>Istio sur {{site.data.keyword.containerlong_notm}}</td>
<td>Gestion de microservice</td>
<td><a href="https://www.ibm.com/cloud/info/istio" target="_blank">Istio <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a> est un service open source qui fournit aux développeurs un moyen de connecter, sécuriser, gérer et surveiller un réseau de microservices, appelé également maillage de services, sur des plateformes d'orchestration de cloud. Istio sur {{site.data.keyword.containerlong}} propose une installation en une seule étape d'Istio dans votre cluster via un module complémentaire géré. En un seul clic, vous pouvez obtenir tous les composants de base d'Istio, des fonctions de suivi, de surveillance et de visualisation supplémentaires, ainsi que le modèle opérationnel d'application BookInfo. Pour commencer, voir [Utilisation du module complémentaire Istio géré (bêta)](/docs/containers?topic=containers-istio).</td>
</tr>
<tr>
<td>Knative</td>
<td>Applications sans serveur</td>
<td>[Knative ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/knative/docs) est une plateforme open source développée par IBM, Google, Pivotal, Red Hat, Cisco et d'autres firmes dans l'objectif d'étendre les capacités de Kubernetes pour vous aider à créer des applications modernes, conteneurisées, axées sur la source et sans serveur, en plus de votre cluster Kubernetes. Cette plateforme utilise une approche cohérente couvrant plusieurs infrastructures et langages de programmation pour permettre l'abstraction de la charge opérationnelle de construction, déploiement et gestion des charges de travail dans Kubernetes pour que les développeurs puissent se concentrer sur ce qui les intéresse le plus : le code source. Pour plus d'informations, voir [Déploiement d'applications sans serveur avec Knative](/docs/containers?topic=containers-serverless-apps-knative). </td>
</tr>
<tr>
<td>Portworx</td>
<td>Stockage pour les applications avec état</td>
<td>[Portworx ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://portworx.com/products/introduction/) est une solution de stockage SDS à haute disponibilité que vous pouvez utiliser pour gérer du stockage persistant pour vos bases de données conteneurisées et d'autres applications avec état, ou pour partager des données entre des pods sur plusieurs zones. Vous pouvez installer Portworx avec une charte Helm et mettre à disposition du stockage pour vos applications en utilisant des volumes persistants Kubernetes. Pour plus d'informations sur la configuration de Portworx dans votre cluster, voir [Stockage de données sur SDS (Software-Defined Storage) avec Portworx](/docs/containers?topic=containers-portworx#portworx).</td>
</tr>
<tr>
<td>Razee</td>
<td>Automatisation du déploiement</td>
<td>[Razee ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://razee.io/) est un projet open source qui automatise et gère le déploiement de ressources Kubernetes sur des clusters, des environnements et des fournisseurs de cloud, et vous aide à visualiser des informations de déploiement pour vos ressources afin de vous permettre de surveiller le processus de déploiement et détecter les problèmes de déploiement plus rapidement. Pour obtenir plus d'informations sur Razee et savoir comment le configurer dans votre cluster pour automatiser votre processus de déploiement, voir la [documentation Razee ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/razee-io/Razee).</td>
</tr>
</tbody>
</table>

<br />


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
<td> <a href="https://helm.sh" target="_blank">Helm <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a> est un gestionnaire de package Kubernetes. Vous pouvez créer de nouvelles chartes Helm ou utiliser des chartes Helm préexistantes pour définir, installer et mettre à niveau des applications Kubernetes complexes qui s'exécutent dans les clusters {{site.data.keyword.containerlong_notm}}. <p>Pour plus d'informations, voir [Configuration de Helm dans {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-helm).</p></td>
</tr>
<tr>
<td>{{site.data.keyword.contdelivery_full}}</td>
<td>Permet d'automatiser vos générations d'applications et vos déploiements de conteneur sur les clusters Kubernetes en utilisant une chaîne d'outils. Pour plus d'informations sur la configuration, voir le blogue <a href="https://developer.ibm.com/recipes/tutorials/deploy-kubernetes-pods-to-the-bluemix-container-service-using-devops-pipelines/" target="_blank">Deploy Kubernetes pods to the {{site.data.keyword.containerlong_notm}} using DevOps Pipelines <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a>. </td>
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
<td>[Knative ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/knative/docs) est une plateforme open source développée par IBM, Google, Pivotal, Red Hat, Cisco et d'autres firmes dans l'objectif d'étendre les capacités de Kubernetes pour vous aider à créer des applications modernes, conteneurisées, axées sur la source et sans serveur, en plus de votre cluster Kubernetes. Cette plateforme utilise une approche cohérente couvrant plusieurs infrastructures et langages de programmation pour permettre l'abstraction de la charge opérationnelle de construction, déploiement et gestion des charges de travail dans Kubernetes pour que les développeurs puissent se concentrer sur ce qui les intéresse le plus : le code source. Pour plus d'informations, voir [Déploiement d'applications sans serveur avec Knative](/docs/containers?topic=containers-serverless-apps-knative). </td>
</tr>
<tr>
<td>Razee</td>
<td>[Razee ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://razee.io/) est un projet open source qui automatise et gère le déploiement de ressources Kubernetes sur des clusters, des environnements et des fournisseurs de cloud, et vous aide à visualiser des informations de déploiement pour vos ressources afin de vous permettre de surveiller le processus de déploiement et détecter les problèmes de déploiement plus rapidement. Pour obtenir plus d'informations sur Razee et savoir comment le configurer dans votre cluster pour automatiser votre processus de déploiement, voir la [documentation Razee ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/razee-io/Razee).</td>
</tr>
</tbody>
</table>

<br />


## Services de cloud hybride
{: #hybrid_cloud_services}

<table summary="Le tableau présente les services disponibles que vous pouvez connecter votre cluster à des centres de données sur site. La lecture des lignes s'effectue de gauche à droite, avec le nom du service dans la première colonne et une description de ce service dans la seconde colonne.">
<caption>Services de cloud hybride</caption>
<thead>
<tr>
<th>Service</th>
<th>Description</th>
</tr>
</thead>
<tbody>
  <tr>
    <td>{{site.data.keyword.BluDirectLink}}</td>
    <td>[{{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-about-ibm-cloud-direct-link) vous permet de créer une connexion privée directe entre vos environnements de réseau distants et {{site.data.keyword.containerlong_notm}} sans routage sur l'Internet public. Les offres {{site.data.keyword.Bluemix_notm}} Direct Link sont utiles lorsque vous devez implémenter des charges de travail hybrides, des charges de travail inter-fournisseur, des transferts de données volumineux ou fréquents ou des charges de travail privées. Pour choisir une offre {{site.data.keyword.Bluemix_notm}} Direct Link et configurer une connexion {{site.data.keyword.Bluemix_notm}} Direct Link, voir [Initiation à  {{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link#how-do-i-know-which-type-of-ibm-cloud-direct-link-i-need-) dans la documentation {{site.data.keyword.Bluemix_notm}} Direct Link. </td>
  </tr>
<tr>
  <td>Service VPN IPSec strongSwan</td>
  <td>Configurez un [service VPN IPSec strongSwan ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.strongswan.org/about.html) connectant de manière sécurisée votre cluster Kubernetes avec un réseau sur site. Le service VPN IPSec strongSwan fournit un canal de communication de bout en bout sécurisé sur Internet, basé sur l'ensemble de protocoles IPSec (Internet Protocol Security) aux normes de l'industrie. Pour configurer une connexion sécurisée entre votre cluster et un réseau sur site, [configurez et déployez le service VPN IPSec strongSwan](/docs/containers?topic=containers-vpn#vpn-setup) directement dans un pod de votre cluster.</td>
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
<td>Surveillez les activités d'administration intervenant dans votre cluster en analysant les journaux via Grafana. Pour plus d'informations sur ce service, voir la documentation d'[Activity Tracker](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started). Pour plus d'informations sur les types d'événement dont vous pouvez assurer le suivi, voir [Evénements Activity Tracker](/docs/containers?topic=containers-at_events).</td>
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
<td>Prometheus est un outil open source de surveillance, de consignation et d'alerte conçu pour Kubernetes. Prometheus extrait des informations détaillées sur le cluster, les noeuds worker et l'état de santé du déploiement à partir des informations de consignation de Kubernetes. L'activité de l'UC, de la mémoire, des E-S et du réseau est collectée pour chaque conteneur qui s'exécute dans un cluster. Vous pouvez utiliser les données collectées dans des demandes ou des alertes personnalisées pour surveiller les performances et les charges de travail dans votre cluster.

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
<td>[Weave Scope ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.weave.works/oss/scope/) fournit un diagramme visuel de vos ressources dans un cluster Kubernetes, notamment des services, pods, conteneurs, processus, noeuds, et autres. Weave Scope fournit des métriques interactives sur l'utilisation de l'UC et de la mémoire, ainsi que des outils pour suivi et exécution dans un conteneur.</li></ol>
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
  <tr>
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
  <td>Les données stockées avec {{site.data.keyword.cos_short}} sont chiffrées et disséminées entre plusieurs emplacements géographiques. Elles sont accessibles via HTTP en utilisant une API REST. Vous pouvez utiliser l'[image ibm-backup-restore](/docs/services/RegistryImages/ibm-backup-restore?topic=RegistryImages-ibmbackup_restore_starter) pour configurer le service de sorte à effectuer des sauvegardes ponctuelles ou planifiées pour les données figurant dans vos clusters. Pour plus d'informations sur le service, voir la <a href="/docs/services/cloud-object-storage?topic=cloud-object-storage-about" target="_blank">documentation {{site.data.keyword.cos_short}} <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a>.</td>
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
  <td>Vous pouvez faire choix parmi toute une gamme de services de base de données {{site.data.keyword.Bluemix_notm}}, notamment {{site.data.keyword.composeForMongoDB_full}} ou {{site.data.keyword.cloudantfull}} pour déployer des solutions de base de données évolutives à haute disponibilité dans votre cluster. Pour obtenir une liste des bases de données disponibles, voir le [catalogue {{site.data.keyword.Bluemix_notm}} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/catalog?category=databases).  </td>
  </tr>
  </tbody>
  </table>
