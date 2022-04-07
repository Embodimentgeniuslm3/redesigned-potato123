---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

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

# Notes sur l'édition
{: #iks-release}

Utilisez les notes sur l'édition pour en savoir plus sur les dernières modifications apportées à la documentation {{site.data.keyword.containerlong}} qui sont regroupées par mois.
{:shortdesc}

## Juin 2019
{: #jun19}

<table summary="Le tableau présente des notes sur l'édition. La lecture des lignes s'effectue de gauche à droite, avec la date dans la première colonne, l'intitulé de la fonction dans la deuxième colonne et une description dans la troisième colonne.">
<caption>Mises à jour de la documentation {{site.data.keyword.containerlong_notm}} apportées en juin 2019</caption>
<thead>
<th>Date</th>
<th>Description</th>
</thead>
<tbody>
<tr>
  <td>7 juin 2019</td>
  <td><ul>
  <li><strong>Accès au maître Kubernetes via le noeud final de service privé</strong> : Ajout d'[étapes](/docs/containers?topic=containers-clusters#access_on_prem) pour exposer le noeud final de service privé via un équilibreur de charge privé. Une fois que vous avez exécuté ces étapes, vos utilisateurs de cluster autorisés peuvent accéder au maître Kubernetes à partir d'une connexion VPN ou {{site.data.keyword.Bluemix_notm}} Direct Link. </li>
  <li><strong>{{site.data.keyword.BluDirectLink}}</strong> : Ajout de {{site.data.keyword.Bluemix_notm}} Direct Link aux pages [Connectivité VPN](/docs/containers?topic=containers-vpn) et [Cloud hybride](/docs/containers?topic=containers-hybrid_iks_icp) afin de pouvoir créer une connexion privée directe entre vos environnements de réseau distants et {{site.data.keyword.containerlong_notm}} sans routage sur l'Internet public. </li>
  <li><strong>Journal des modifications de l'ALB Ingress</strong> : Mise à jour de l'image `ingress-auth` de l'[ALB vers la génération 330](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog).</li>
  <li><strong>Bêta OpenShift</strong> : [Ajout d'une leçon](/docs/containers?topic=containers-openshift_tutorial#openshift_logdna_sysdig) expliquant comment modifier des déploiements d'application pour des contraintes de contexte de sécurité privilégiées pour les modules complémentaires {{site.data.keyword.la_full_notm}} et {{site.data.keyword.mon_full_notm}}. </li>
  </ul></td>
</tr>
<tr>
  <td>6 juin 2019</td>
  <td><ul>
  <li><strong>Journal des modifications de Fluentd</strong> : Ajout d'un [journal des modifications version Fluentd](/docs/containers?topic=containers-cluster-add-ons-changelog#fluentd_changelog).</li>
  <li><strong>Journal des modifications de l'ALB Ingress</strong> : Mise à jour de l'image `nginx-ingress` de l'[ALB vers la génération 470](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog).</li>
  </ul></td>
</tr>
<tr>
  <td>5 juin 2019</td>
  <td><ul>
  <li><strong>Référence CLI</strong> : Mise à jour de la [page de référence CLI](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli) afin de refléter plusieurs modifications apportées à l'[édition de la version 0.3.34](/docs/containers?topic=containers-cs_cli_changelog) du plug-in CLI {{site.data.keyword.containerlong_notm}}. </li>
  <li><strong>Nouveau ! Clusters Red Hat OpenShift on IBM Cloud (bêta)</strong> : Avec la version bêta Red Hat OpenShift on IBM Cloud, vous pouvez créer des clusters {{site.data.keyword.containerlong_notm}} dotés de noeuds worker qui sont fournis installés avec le logiciel de plateforme d'orchestration de conteneur OpenShift. Vous bénéficiez de tous les avantages d'{{site.data.keyword.containerlong_notm}} géré pour votre environnement d'infrastructure de cluster tout en utilisant les [outils OpenShift et le catalogue ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://docs.openshift.com/container-platform/3.11/welcome/index.html) qui s'exécute sur Red Hat Enterprise Linux pour vos déploiements d'application. Pour commencer, voir [Tutoriel : Création d'un cluster Red Hat OpenShift on IBM Cloud (bêta)](/docs/containers?topic=containers-openshift_tutorial).</li>
  </ul></td>
</tr>
<tr>
  <td>4 juin 2019</td>
  <td><ul>
  <li><strong>Journaux de modifications de version</strong> : Mise à jour des journaux de modifications pour les éditions de correctifs [1.14.2_1521](/docs/containers?topic=containers-changelog#1142_1521), [1.13.6_1524](/docs/containers?topic=containers-changelog#1136_1524), [1.12.9_1555](/docs/containers?topic=containers-changelog#1129_1555) et [1.11.10_1561](/docs/containers?topic=containers-changelog#11110_1561). </li></ul>
  </td>
</tr>
<tr>
  <td>3 juin 2019</td>
  <td><ul>
  <li><strong>Utilisation de votre propre contrôleur Ingress</strong> : Mise à jour des [étapes](/docs/containers?topic=containers-ingress#user_managed) visant à répercuter les modifications apportées au contrôleur de communauté par défaut et à imposer un diagnostic d'intégrité pour les adresses IP de contrôleur dans des clusters à zones multiples. </li>
  <li><strong>{{site.data.keyword.cos_full_notm}}</strong> : Mise à jour des [étapes](/docs/containers?topic=containers-object_storage#install_cos) d'installation du plug-in {{site.data.keyword.cos_full_notm}} avec ou sans le serveur Helm, Tiller.</li>
  <li><strong>Journal des modifications de l'ALB Ingress</strong> : Mise à jour de l'image `nginx-ingress` de l'[ALB vers la génération 467](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog).</li>
  <li><strong>Kustomize</strong> : Ajout d'un exemple de [réutilisation des fichiers de configuration Kubernetes dans plusieurs environnements avec Kustomize](/docs/containers?topic=containers-app#kustomize).</li>
  <li><strong>Razee</strong> : Ajout de [Razee ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/razee-io/Razee) aux intégrations prises en charge afin de visualiser des informations de déploiement dans le cluster et d'automatiser le déploiement de ressources Kubernetes. </li></ul>
  </td>
</tr>
</tbody></table>

## Mai 2019
{: #may19}

<table summary="Le tableau présente des notes sur l'édition. La lecture des lignes s'effectue de gauche à droite, avec la date dans la première colonne, l'intitulé de la fonction dans la deuxième colonne et une description dans la troisième colonne.">
<caption>Mises à jour de la documentation {{site.data.keyword.containerlong_notm}} apportées en mai 2019</caption>
<thead>
<th>Date</th>
<th>Description</th>
</thead>
<tbody>
<tr>
  <td>30 mai 2019</td>
  <td><ul>
  <li><strong>Référence CLI</strong> : Mise à jour de la [page de référence CLI](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli) afin de refléter plusieurs modifications apportées à l'[édition de la version 0.3.33](/docs/containers?topic=containers-cs_cli_changelog) du plug-in CLI {{site.data.keyword.containerlong_notm}}. </li>
  <li><strong>Traitement des incidents liés au stockage</strong> :<ul>
  <li>Ajout d'une rubrique de traitement des incidents liés au stockage de fichiers et au stockage par blocs pour les [états de réservation de volume persistant en attente](/docs/containers?topic=containers-cs_troubleshoot_storage#file_pvc_pending).</li>
  <li>Ajout d'une rubrique de traitement des incidents liés au stockage par blocs traitant de l'impossibilité pour [une application d'accéder à une réservation de volume persistant ou d'écrire des données sur une réservation de volume persistant](/docs/containers?topic=containers-cs_troubleshoot_storage#block_app_failures).</li>
  </ul></li>
  </ul></td>
</tr>
<tr>
  <td>28 mai 2019</td>
  <td><ul>
  <li><strong>Journal des modifications de modules complémentaires de cluster</strong> : Mise à jour de l'image `nginx-ingress` de l'[ALB vers la génération 462](/docs/containers?topic=containers-cluster-add-ons-changelog).</li>
  <li><strong>Traitement des incidents liés au registre</strong> : Ajout d'une [rubrique de traitement des incidents](/docs/containers?topic=containers-cs_troubleshoot_clusters#ts_image_pull_create) traitant de l'impossibilité pour votre cluster d'extraire des images d'{{site.data.keyword.registryfull}} en raison d'une erreur lors de la création du cluster.
  </li>
  <li><strong>Traitement des incidents liés au stockage</strong> :<ul>
  <li>Ajout d'une rubrique de [débogage des échecs de stockage persistant](/docs/containers?topic=containers-cs_troubleshoot_storage#debug_storage).</li>
  <li>Ajout d'une rubrique de traitement des incidents liés aux [échecs de création de réservation de volume persistant en raison de droits manquants](/docs/containers?topic=containers-cs_troubleshoot_storage#missing_permissions).</li>
  </ul></li>
  </ul></td>
</tr>
<tr>
  <td>23 mai 2019</td>
  <td><ul>
  <li><strong>Référence CLI</strong> : Mise à jour de la [page de référence CLI](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli) afin de refléter plusieurs modifications apportées à l'[édition de la version 0.3.28](/docs/containers?topic=containers-cs_cli_changelog) du plug-in CLI {{site.data.keyword.containerlong_notm}}. </li>
  <li><strong>Journal des modifications de modules complémentaires de cluster</strong> : Mise à jour de l'image `nginx-ingress` de l'[ALB vers la génération 457](/docs/containers?topic=containers-cluster-add-ons-changelog).</li>
  <li><strong>Etats de cluster et de noeud worker</strong> : Mise à jour de la [page de journalisation et de surveillance](/docs/containers?topic=containers-health#states) avec l'ajout de tableaux de référence sur les états de cluster et de noeud worker. </li>
  <li><strong>Création et planification de cluster</strong> : Des informations sur la planification, la création et le retrait de cluster et sur la planification de réseau sont désormais disponibles sur les pages suivantes :
    <ul><li>[Planification d'une configuration de réseau pour votre cluster](/docs/containers?topic=containers-plan_clusters)</li>
    <li>[Planification de votre cluster pour la haute disponibilité](/docs/containers?topic=containers-ha_clusters)</li>
    <li>[Planification de la configuration de noeud worker](/docs/containers?topic=containers-planning_worker_nodes)</li>
    <li>[Création de clusters](/docs/containers?topic=containers-clusters)</li>
    <li>[Ajout de noeuds worker et de zones dans les clusters](/docs/containers?topic=containers-add_workers)</li>
    <li>[Suppression de clusters](/docs/containers?topic=containers-remove)</li>
    <li>[Modification des noeuds finaux de service ou des connexions VLAN](/docs/containers?topic=containers-cs_network_cluster)</li></ul>
  </li>
  <li><strong>Mises à jour de version de cluster</strong> : Mise à jour de la [politique de versions non prises en charge](/docs/containers?topic=containers-cs_versions) avec une remarque indiquant que vous ne pouvez pas mettre à jour des clusters si la version maître est antérieure d'au moins trois versions à la plus ancienne version prise en charge. Vous pouvez voir si le cluster n'est **pas pris en charge** en examinant la zone **Etat** lorsque vous répertoriez les clusters.</li>
  <li><strong>Istio</strong> : Mise à jour de la [page Istio](/docs/containers?topic=containers-istio) avec le retrait de la limitation selon laquelle Istio ne fonctionne pas dans des clusters qui sont connectés à un VLAN privé uniquement. Ajout d'une étape à la [rubrique Mise à jour des modules complémentaires gérés](/docs/containers?topic=containers-managed-addons#updating-managed-add-ons) afin de recréer des passerelles Istio qui utilisent des sections TLS une fois la mise à jour du module complémentaire géré Istio terminée. </li>
  <li><strong>Rubriques populaires</strong> : Remplacement des rubriques populaires par cette page de notes sur l'édition pour les nouvelles fonctions et les mises à jour qui sont propres à {{site.data.keyword.containershort_notm}}. Pour obtenir les dernières informations sur les produits {{site.data.keyword.Bluemix_notm}}, voir la page [Announcements](https://www.ibm.com/cloud/blog/announcements).</li>
  </ul></td>
</tr>
<tr>
  <td>20 mai 2019</td>
  <td><ul>
  <li><strong>Journaux de modifications de version</strong> : Ajout de [journaux de modifications pour les groupes de correctifs de noeud worker](/docs/containers?topic=containers-changelog).</li>
  </ul></td>
</tr>
<tr>
  <td>16 mai 2019</td>
  <td><ul>
  <li><strong>Référence CLI</strong> : Mise à jour de la [page de référence CLI](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli) afin d'ajouter des noeuds finaux COS pour les commandes `logging-collect` et de stipuler que `apiserver-refresh` redémarre les composants maître Kubernetes. </li>
  <li><strong>Non prise en charge : Kubernetes version 1.10</strong> : [Kubernetes version 1.10](/docs/containers?topic=containers-cs_versions#cs_v114) est désormais non pris en charge. </li>
  </ul></td>
</tr>
<tr>
  <td>15 mai 2019</td>
  <td><ul>
  <li><strong>Version Kubernetes par défaut</strong> : La version Kubernetes par défaut est désormais la version 1.13.6.</li>
  <li><strong>Limites de service</strong> : Ajout d'une [rubrique sur les limites de service](/docs/containers?topic=containers-ibm-cloud-kubernetes-service-technology#tech_limits).</li>
  </ul></td>
</tr>
<tr>
  <td>13 mai 2019</td>
  <td><ul>
  <li><strong>Journaux de modifications de version</strong> : Ajout d'informations stipulant que de nouvelles mises à jour de correctif sont disponibles pour les version [1.14.1_1518](/docs/containers?topic=containers-changelog#1141_1518), [1.13.6_1521](/docs/containers?topic=containers-changelog#1136_1521), [1.12.8_1552](/docs/containers?topic=containers-changelog#1128_1552), [1.11.10_1558](/docs/containers?topic=containers-changelog#11110_1558) et [1.10.13_1558](/docs/containers?topic=containers-changelog#11013_1558).</li>
  <li><strong>Versions de noeud worker</strong> : Retrait de toutes les [versions de noeud worker de machine virtuelle](/docs/containers?topic=containers-planning_worker_nodes#vm) avec au moins 48 coeurs par [statut de cloud ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/status?component=containers-kubernetes&selected=status). Vous pouvez tout de même mettre à disposition des [noeuds worker bare metal](/docs/containers?topic=containers-plan_clusters#bm) avec au moins 48 coeurs. </li></ul></td>
</tr>
<tr>
  <td>8 mai 2019</td>
  <td><ul>
  <li><strong>API</strong> : Ajout d'un lien vers la [documentation swagger d'API globale ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://containers.cloud.ibm.com/global/swagger-global-api/#/).</li>
  <li><strong>Stockage d'objets Cloud</strong> : [Ajout d'un guide de traitement des incidents liés au stockage d'objets Cloud](/docs/containers?topic=containers-cs_troubleshoot_storage#cos_pvc_pending) dans vos clusters {{site.data.keyword.containerlong_notm}}. </li>
  <li><strong>Stratégie Kubernetes</strong> : Ajout d'une rubrique décrivant les [connaissances et les compétences techniques qu'il est recommandé de posséder avant de déplacer ses applications vers {{site.data.keyword.containerlong_notm}}?](/docs/containers?topic=containers-strategy#knowledge).</li>
  <li><strong>Kubernetes version 1.14</strong> : Ajout d'une remarque stipulant que l'[édition 1.14 de Kubernetes](/docs/containers?topic=containers-cs_versions#cs_v114) est certifiée. </li>
  <li><strong>Rubriques de référence</strong> : Mise à jour d'informations pour différentes opérations de liaison de service, de `consignation` et `nlb` sur la pages de référence [d'accès utilisateur](/docs/containers?topic=containers-access_reference) et [CLI](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli). </li></ul></td>
</tr>
<tr>
  <td>7 mai 2019</td>
  <td><ul>
  <li><strong>Fournisseur DNS de cluster</strong> : [Description des avantages de CoreDNS](/docs/containers?topic=containers-cluster_dns) maintenant que les clusters qui exécutent Kubernetes 1.14 et plus prennent en charge uniquement CoreDNS.</li>
  <li><strong>Noeuds de périphérie</strong> : Ajout d'une prise en charge d'équilibreur de charge privé pour des [noeuds de périphérie](/docs/containers?topic=containers-edge).</li>
  <li><strong>Clusters gratuits</strong> : Précision des emplacements où les [clusters gratuits](/docs/containers?topic=containers-regions-and-zones#regions_free) sont pris en charge. </li>
  <li><strong>Nouveau ! Intégrations</strong> : Ajout et restructuration d'informations sur les [services et les intégrations tiers {{site.data.keyword.Bluemix_notm}}](/docs/containers?topic=containers-ibm-3rd-party-integrations), les [intégrations populaires](/docs/containers?topic=containers-supported_integrations) et les [partenariats](/docs/containers?topic=containers-service-partners).</li>
  <li><strong>Nouveau ! Kubernetes version 1.14</strong> : Créez ou mettez à jour vos clusters vers [Kubernetes 1.14](/docs/containers?topic=containers-cs_versions#cs_v114).</li>
  <li><strong>Dépréciation de Kubernetes version 1.11</strong> : [Mettez à jour](/docs/containers?topic=containers-update) les clusters qui exécutent [Kubernetes 1.11](/docs/containers?topic=containers-cs_versions#cs_v111) avant qu'ils ne soient plus pris en charge. </li>
  <li><strong>Droits</strong> : Ajout d'une questions à la foire aux questions, [Quelles politiques d'accès dois-je fournir à mes utilisateurs de cluster ?](/docs/containers?topic=containers-faqs#faq_access)</li>
  <li><strong>Pools de noeuds worker</strong> : Ajout d'instructions expliquant comment [appliquer des libellés à des pools de noeuds worker existants](/docs/containers?topic=containers-add_workers#worker_pool_labels).</li>
  <li><strong>Rubriques de référence</strong> : Mise à jour de pages de référence [Journal des modifications](/docs/containers?topic=containers-changelog#changelog) pour la prise en charge de nouvelles fonctions, telles que Kubernetes 1.14. </li></ul></td>
</tr>
<tr>
  <td>1 mai 2019</td>
  <td><strong>Affectation d'un accès d'infrastructure</strong> : Révision des [étapes permettant d'affecter des droits IAM pour l'ouverture de cas de support](/docs/containers?topic=containers-users#infra_access).</td>
</tr>
</tbody></table>


