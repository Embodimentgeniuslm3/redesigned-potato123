---

copyright:
  years: 2014, 2019
lastupdated: "2019-05-31"

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


# Pourquoi utiliser {{site.data.keyword.containerlong_notm}} ?
{: #cs_ov}

{{site.data.keyword.containerlong}} propose des outils puissants en combinant les conteneurs de Docker, la technologie de Kubernetes, une expérience utilisateur intuitive, ainsi qu'une sécurité et un isolement intégrés pour automatiser le déploiement, l'exploitation, la mise à l'échelle et la surveillance d'applications conteneurisées dans un cluster d'hôtes de calcul. Pour obtenir des informations sur la certification, voir [Compliance on the {{site.data.keyword.Bluemix_notm}} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/cloud/compliance).
{:shortdesc}


## Avantages de l'utilisation du service
{: #benefits}

Les clusters sont déployés sur des hôtes de calcul qui fournissent des capacités Kubernetes natives et des capacités spécifiques à {{site.data.keyword.IBM_notm}}.
{:shortdesc}

|Avantage|Description|
|-------|-----------|
|Clusters Kubernetes à service exclusif avec isolement de l'infrastructure de traitement, de réseau et de stockage|<ul><li>Créez votre propre infrastructure personnalisée afin de répondre aux besoins de votre organisation.</li><li>Allouez à un maître Kubernetes dédié et sécurisé, des noeuds worker, des réseaux virtuels et un espace de stockage en utilisant les ressources fournies par l'infrastructure IBM Cloud (SoftLayer).</li><li>Le maître Kubernetes entièrement géré est constamment surveillé et mis à jour par {{site.data.keyword.IBM_notm}} pour que votre cluster soit toujours disponible.</li><li>Option permettant de mettre à disposition des noeuds worker en tant que serveurs bare metal avec la fonction Calcul sécurisé.</li><li>Stockez les données persistantes, partagez les données entre les pods Kubernetes et restaurez les données en cas de besoin avec le service de volumes intégré et sécurisé.</li><li>Tirez parti de la prise en charge complète de toutes les API Kubernetes natives.</li></ul>|
| Clusters à zones multiples pour une haute disponibilité accrue | <ul><li>Gérez facilement les noeuds worker d'un même type de machine (UC, mémoire, virtuelle ou physique) avec des pools de noeuds worker.</li><li>Protégez-vous en cas de défaillance d'une zone en répartissant les noeuds uniformément entre les différentes zones et en utilisant des déploiements de pod anti-affinité pour vos applications.</li><li>Réduisez les coûts en utilisant des clusters à zones multiples au lieu de dupliquer les ressources dans un cluster distinct.</li><li>Bénéficiez de l'équilibrage de charge automatique entre vos applications avec l'équilibreur de charge pour zones multiples (MZLB) configuré automatiquement pour vous dans chaque zone du cluster.</li></ul>|
| Maîtres à haute disponibilité | <ul><li>Réduisez la durée d'indisponibilité du cluster, notamment lors de mise à jour du maître avec les maîtres à haute disponibilité mis à disposition automatiquement lorsque vous créez un cluster.</li><li>Répartissez vos maîtres entre les zones dans un [cluster à zones multiples](/docs/containers?topic=containers-ha_clusters#multizone) pour protéger votre cluster en cas de défaillance d'une zone.</li></ul> |
|Conformité en matière de sécurité d'image avec Vulnerability Advisor|<ul><li>Configurez votre propre référentiel dans notre registre d'images privé Docker sécurisé où les images sont stockées et partagées par tous les utilisateurs dans l'organisation.</li><li>Tirez parti de l'analyse automatique des images dans votre registre {{site.data.keyword.Bluemix_notm}} privé.</li><li>Examinez les recommandations spécifiques au système d'exploitation utilisé dans l'image afin de corriger les vulnérabilités potentielles.</li></ul>|
|Surveillance en continu de l'état de santé du cluster|<ul><li>Utilisez le tableau de bord du cluster pour déterminer rapidement et gérer l'état de santé de votre cluster, des noeuds worker et des déploiements de conteneurs.</li><li>Accédez à des métriques de consommation détaillées en utilisant {{site.data.keyword.monitoringlong}} et élargissez rapidement votre cluster pour répondre aux charges de travail.</li><li>Examinez les informations de consignation à l'aide d'{{site.data.keyword.loganalysislong}} pour voir les activités détaillées du cluster.</li></ul>|
|Exposition sécurisée des applications au public|<ul><li>Sélectionnez une adresse IP publique, une route fournie par {{site.data.keyword.IBM_notm}} ou votre propre domaine personnalisé pour accéder à des services dans votre cluster depuis Internet.</li></ul>|
|Intégration de services {{site.data.keyword.Bluemix_notm}}|<ul><li>Vous pouvez ajouter des fonctionnalités supplémentaires à votre application via l'intégration de services {{site.data.keyword.Bluemix_notm}}, tels que les API Watson, Blockchain, des services de données ou Internet of Things (IoT).</li></ul>|
{: caption="Avantages d'{{site.data.keyword.containerlong_notm}}" caption-side="top"}

Prêt à démarrer ? Suivez le [tutoriel de création d'un cluster Kubernetes](/docs/containers?topic=containers-cs_cluster_tutorial#cs_cluster_tutorial).

<br />


## Comparaison d'offres et de leurs combinaisons
{: #differentiation}

Vous pouvez exécuter {{site.data.keyword.containerlong_notm}} dans {{site.data.keyword.Bluemix_notm}} Public, dans {{site.data.keyword.Bluemix_notm}} Private ou dans une configuration hybride.
{:shortdesc}


<table>
<caption>Différences entre les configurations d'{{site.data.keyword.containershort_notm}}</caption>
<col width="22%">
<col width="78%">
 <thead>
 <th>Configuration d'{{site.data.keyword.containershort_notm}}</th>
 <th>Description</th>
 </thead>
 <tbody>
 <tr>
 <td>{{site.data.keyword.Bluemix_notm}} Public, hors site</td>
 <td>Avec {{site.data.keyword.Bluemix_notm}} Public sur du [matériel partagé ou dédié ou sur des machines bare metal](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes), vous pouvez héberger vos applications dans des clusters sur le cloud à l'aide d'{{site.data.keyword.containerlong_notm}}. Vous pouvez également créer un cluster avec des pools de noeud worker dans plusieurs zones pour une disponibilité accrue de vos applications. {{site.data.keyword.containerlong_notm}} sur {{site.data.keyword.Bluemix_notm}} Public propose des outils puissants en combinant les conteneurs de Docker, la technologie de Kubernetes, une expérience utilisateur intuitive, ainsi qu'une sécurité et un isolement intégrés pour automatiser le déploiement, l'exploitation, la mise à l'échelle et la surveillance d'applications conteneurisées dans un cluster d'hôtes de calcul.<br><br>Pour plus d'informations voir [Technologie d'{{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-ibm-cloud-kubernetes-service-technology).
 </td>
 </tr>
 <tr>
 <td>{{site.data.keyword.Bluemix_notm}} Private, sur site</td>
 <td>{{site.data.keyword.Bluemix_notm}} Private est une plateforme applicative pouvant être installée en local sur vos propres machines. Vous pouvez choisir d'utiliser Kubernetes dans {{site.data.keyword.Bluemix_notm}} Private lorsque vous devez développer et gérer des applications conteneurisées sur site dans votre propre environnement contrôlé et protégé derrière un pare-feu. <br><br>Pour plus d'informations, voir la [documentation du produit {{site.data.keyword.Bluemix_notm}} Private ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_1.2.0/kc_welcome_containers.html).
 </td>
 </tr>
 <tr>
 <td>Configuration hybride</td>
 <td>Une configuration hybride est l'utilisation combinée de services qui s'exécutent dans l'environnement {{site.data.keyword.Bluemix_notm}} Public hors site et d'autres services exécutés sur site, par exemple une application dans l'environnement {{site.data.keyword.Bluemix_notm}} Private. Exemples de configuration hybride : <ul><li>Mise à disposition d'un cluster avec {{site.data.keyword.containerlong_notm}} dans l'environnement {{site.data.keyword.Bluemix_notm}} Public, mais en connectant ce cluster à une base de données sur site.</li><li>Mise à disposition d'un cluster avec {{site.data.keyword.containerlong_notm}} dans l'environnement {{site.data.keyword.Bluemix_notm}} Private et déploiement d'une application dans ce cluster. Cependant, cette application peut utiliser un service {{site.data.keyword.ibmwatson}}, tel que {{site.data.keyword.toneanalyzershort}}, dans l'environnement {{site.data.keyword.Bluemix_notm}} Public.</li></ul><br>Pour activer la communication entre les services qui s'exécutent dans {{site.data.keyword.Bluemix_notm}} Public ou Dedicated et les services qui s'exécutent sur site, vous devez [configurer une connexion VPN](/docs/containers?topic=containers-vpn). Pour plus d'informations, voir [Utilisation d'{{site.data.keyword.containerlong_notm}} avec {{site.data.keyword.Bluemix_notm}} Private](/docs/containers?topic=containers-hybrid_iks_icp).
 </td>
 </tr>
 </tbody>
</table>

<br />


## Comparaison des clusters gratuits et standard
{: #cluster_types}

Vous pouvez créer un cluster gratuit ou n'importe quel nombre de clusters standard. Faites-vous la main avec les clusters gratuits pour vous familiariser avec diverses fonctionnalités de Kubernetes ou créez des clusters standard pour exploiter toutes les fonctionnalités de Kubernetes pour déployer des applications. Les clusters gratuits sont supprimés automatiquement au bout de 30 jours.
{:shortdesc}

Si vous disposez d'un cluster gratuit et que vous souhaitez effectuer une mise à niveau pour passer à un cluster standard, vous pouvez [créer un cluster standard](/docs/containers?topic=containers-clusters#clusters_ui). Déployez ensuite les fichiers YAML des ressources Kubernetes que vous avez créées avec votre cluster gratuit sur le cluster standard.

|Caractéristiques|Clusters gratuits|Clusters standard|
|---------------|-------------|-----------------|
|[Mise en réseau au sein d'un cluster](/docs/containers?topic=containers-security#network)|<img src="images/confirm.svg" width="32" alt="Fonction disponible" style="width:32px;" />|<img src="images/confirm.svg" width="32" alt="Fonction disponible" style="width:32px;" />|
|[Accès à des applications réseau publiques par un service NodePort avec une adresse IP non stable](/docs/containers?topic=containers-nodeport)|<img src="images/confirm.svg" width="32" alt="Fonction disponible" style="width:32px;" />|<img src="images/confirm.svg" width="32" alt="Fonction disponible" style="width:32px;" />|
|[Gestion de l'accès utilisateur](/docs/containers?topic=containers-users#access_policies)|<img src="images/confirm.svg" width="32" alt="Fonction disponible" style="width:32px;" />|<img src="images/confirm.svg" width="32" alt="Fonction disponible" style="width:32px;" />|
|[Accès au service {{site.data.keyword.Bluemix_notm}} à partir du cluster et des applications](/docs/containers?topic=containers-service-binding#bind-services)|<img src="images/confirm.svg" width="32" alt="Fonction disponible" style="width:32px;" />|<img src="images/confirm.svg" width="32" alt="Fonction disponible" style="width:32px;" />|
|[Espace disque sur le noeud worker pour stockage non persistant](/docs/containers?topic=containers-storage_planning#non_persistent_overview)|<img src="images/confirm.svg" width="32" alt="Fonction disponible" style="width:32px;" />|<img src="images/confirm.svg" width="32" alt="Fonction disponible" style="width:32px;" />|
| [Possibilité de créer un cluster dans toutes les régions {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-regions-and-zones) | | <img src="images/confirm.svg" width="32" alt="Fonction disponible" style="width:32px;" /> |
|[Clusters à zones multiples pour une haute disponibilité accrue](/docs/containers?topic=containers-ha_clusters#multizone) | |<img src="images/confirm.svg" width="32" alt="Fonction disponible" style="width:32px;" />|
| Maîtres répliqués pour une haute disponibilité accrue | | <img src="images/confirm.svg" width="32" alt="Fonction disponible" style="width:32px;" /> |
|[Nombre de noeuds worker évolutif pour augmenter la capacité](/docs/containers?topic=containers-app#app_scaling)| |<img src="images/confirm.svg" width="32" alt="Fonction disponible" style="width:32px;" />|
|[Stockage de fichiers NFS persistant avec volumes](/docs/containers?topic=containers-file_storage#file_storage)| |<img src="images/confirm.svg" width="32" alt="Fonction disponible" style="width:32px;" />|
|[Accès à des applications réseau publiques ou privées par un service d'équilibreur de charge de réseau avec une adresse IP stable](/docs/containers?topic=containers-loadbalancer)| |<img src="images/confirm.svg" width="32" alt="Fonction disponible" style="width:32px;" />|
|[Accès à des applications réseau publiques par un service Ingress avec une adresse IP stable et une URL personnalisable](/docs/containers?topic=containers-ingress#planning)| |<img src="images/confirm.svg" width="32" alt="Fonction disponible" style="width:32px;" />|
|[Adresses IP publiques portables](/docs/containers?topic=containers-subnets#review_ip)| |<img src="images/confirm.svg" width="32" alt="Fonction disponible" style="width:32px;" />|
|[Consignation et surveillance](/docs/containers?topic=containers-health#logging)| |<img src="images/confirm.svg" width="32" alt="Fonction disponible" style="width:32px;" />|
|[Option permettant de mettre à disposition vos noeuds worker sur des serveurs physiques (bare metal)](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes) | |<img src="images/confirm.svg" width="32" alt="Fonction disponible" style="width:32px;" />|
|[Option permettant de mettre à disposition des noeuds worker bare metal avec la fonction de calcul sécurisé](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes) | |<img src="images/confirm.svg" width="32" alt="Fonction disponible" style="width:32px;" />|
{: caption="Caractéristiques des clusters gratuits et standard" caption-side="top"}
