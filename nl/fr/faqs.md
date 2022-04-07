---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-05"

keywords: kubernetes, iks, compliance, security standards

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
{:download: .download}
{:preview: .preview}
{:faq: data-hd-content-type='faq'}


# Foire aux questions
{: #faqs}

## Qu'est-ce que Kubernetes ?
{: #kubernetes}
{: faq}

Kubernetes est une plateforme open source utilisée pour gérer des charges de travail et des services conteneurisés sur plusieurs hôtes et offrir des outils de gestion pour déployer, automatiser, surveiller et mettre à l'échelle de applications conteneurisées avec une intervention manuelle minimale voire nulle. Tous les conteneurs constituant votre microservice sont regroupés dans des pods. Les pods sont des unités logiques facilitant les opérations de gestion et de reconnaissance. Ces pods s'exécutent sur des hôtes de calcul gérés dans un cluster Kubernetes portable, extensible et à réparation spontanée en cas de défaillance.
{: shortdesc}

Pour plus d'informations sur Kubernetes, voir la [documentation Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/home/?path=users&persona=app-developer&level=foundational).

## Comment fonctionne IBM Cloud Kubernetes Service ?
{: #kubernetes_service}
{: faq}

Avec {{site.data.keyword.containerlong_notm}}, vous pouvez créer votre propre cluster Kubernetes pour déployer et gérer des applications conteneurisées sur {{site.data.keyword.Bluemix_notm}}. Vos applications conteneurisées sont hébergées sur des hôtes de calcul de l'infrastructure IBM Cloud (SoftLayer) nommés noeuds worker. Vous pouvez opter pour la mise à disposition de vos hôtes de calcul sous forme de [machines virtuelles](/docs/containers?topic=containers-planning_worker_nodes#vm) avec des ressources partagées ou dédiées, ou sous forme de [machines bare metal](/docs/containers?topic=containers-planning_worker_nodes#bm) pouvant être optimisées pour l'utilisation de processeur graphique et de stockage défini par logiciel (SDS). Vos noeuds worker sont contrôlés par un maître Kubernetes à haute disponibilité configuré, surveillé et géré par IBM. Vous pouvez utiliser l'API ou l'interface de ligne de commande (CLI) d'{{site.data.keyword.containerlong_notm}} pour travailler avec les ressources d'infrastructure de votre cluster et utiliser l'API ou l'interface CLI de Kubernetes pour gérer vos services et vos déploiements.

Pour plus d'informations sur la configuration des ressources de votre cluster, voir [Architecture de service](/docs/containers?topic=containers-ibm-cloud-kubernetes-service-technology#architecture). Pour obtenir une liste des fonctionnalités et des avantages, voir [Pourquoi {{site.data.keyword.containerlong_notm}} ?](/docs/containers?topic=containers-cs_ov#cs_ov).

## Pourquoi dois-je utiliser IBM Cloud Kubernetes Service ?
{: #faq_benefits}
{: faq}

{{site.data.keyword.containerlong_notm}} est une offre Kubernetes gérée qui fournit des outils puissants, une expérience utilisateur intuitive et une sécurité intégrée pour distribuer rapidement des applications que vous pouvez associer à des services de cloud liés à IBM Watson, IA, IoT, DevOps, ainsi qu'à la sécurité et à l'analyse des données. En tant que fournisseur Kubernetes agréé, {{site.data.keyword.containerlong_notm}} fournit une planification intelligente, des fonctions de réparation spontanée, la mise à l'échelle horizontale, la reconnaissance de service et l'équilibrage de charge, des déploiements et rétromigrations automatiques, ainsi que la gestion des configurations et des valeurs confidentielles (secrets). Le service dispose également de fonctions avancées pour simplifier la gestion d'un cluster, la sécurité des conteneurs et les règles d'isolement, la possibilité de concevoir votre propre cluster et des outils opérationnels intégrés pour garantir une certaine cohérence dans les déploiements.

Pour obtenir une présentation détaillée des capacités et des avantages, voir [Pourquoi {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-cs_ov#cs_ov) ?

## Le service est-il fourni avec un maître et des noeuds Kubernetes gérés ?
{: #managed_master_worker}
{: faq}

Tous les clusters Kubernetes dans {{site.data.keyword.containerlong_notm}} sont contrôlés par un maître Kubernetes dédié géré par IBM dans un compte d'infrastructure {{site.data.keyword.Bluemix_notm}} appartenant à IBM. Le maître Kubernetes, y compris tous les composants du maître, les opérations de calcul, les réseaux et les ressources de stockage, sont surveillés en permanence par des ingénieurs IBM SRE (Site Reliability Engineers). Ces ingénieurs appliquent les dernières normes en matière de sécurité, détectent et éliminent les activités malveillantes et travaillent pour assurer la fiabilité et la disponibilité d'{{site.data.keyword.containerlong_notm}}. Des modules complémentaires, tels que Fluentd pour la consignation, qui sont installés automatiquement lorsque vous mettez à disposition le cluster sont automatiquement mis à jour par IBM. Cependant, vous pouvez envisager de désactiver ces mises à jour automatiques pour certains modules complémentaires et les mettre à jour manuellement indépendamment du maître et des noeuds worker. Pour plus d'informations, voir [Mise à jour de modules complémentaires de cluster](/docs/containers?topic=containers-update#addons).

Régulièrement, Kubernetes publie des [mises à jour principales, secondaires ou des correctifs](/docs/containers?topic=containers-cs_versions#version_types). Ces mises à jour peuvent affecter la version du serveur d'API Kubernetes ou d'autres composants dans le maître Kubernetes. IBM met à jour automatiquement la version de correctif, mais vous devez mettre à jour les versions principales et secondaires. Pour plus d'informations, voir [Mise à jour du maître Kubernetes](/docs/containers?topic=containers-update#master).

Les noeuds worker dans les clusters standard sont provisionnés dans votre compte d'infrastructure {{site.data.keyword.Bluemix_notm}}. Les noeuds worker sont dédiés à votre compte et il vous incombe de demander des mises à jour de ces noeuds worker fréquemment pour vous assurer que le système d'exploitation des noeuds worker et les composants {{site.data.keyword.containerlong_notm}} appliquent les mises à jour et les correctifs de sécurité les plus récents. Des mises à jour de sécurité et des correctifs sont mis à disposition par les ingénieurs IBM SRE (Site Reliability Engineers) qui surveillent en permanence l'image Linux installée sur vos noeuds worker pour détecter les vulnérabilités et les problèmes de conformité en matière de sécurité. Pour plus d'informations, voir [Mise à jour des noeuds worker](/docs/containers?topic=containers-update#worker_node).

## Le maître et les noeuds worker Kubernetes sont-ils à haute disponibilité ?
{: #faq_ha}
{: faq}

L'architecture et l'infrastructure d'{{site.data.keyword.containerlong_notm}} est conçue pour assurer la fiabilité, réduire les temps d'attente de traitement et favoriser la disponibilité maximale du service. Par défaut, tous les clusters dans {{site.data.keyword.containerlong_notm}} sont configurés avec plusieurs instances du maître Kubernetes pour assurer la disponibilité et l'accessibilité de vos ressources de cluster, même si une ou plusieurs instances de votre maître Kubernetes sont indisponibles.

Vous pouvez accentuer la haute disponibilité de votre cluster et protéger votre application des interruptions en répartissant vos charges de travail sur plusieurs noeuds worker dans plusieurs zones d'une région. C'est ce que l'on appelle une configuration de [cluster à zones multiples](/docs/containers?topic=containers-ha_clusters#multizone) qui garantit que votre application est accessible, même en cas d'indisponibilité d'un noeud worker ou d'une zone complète.

Pour vous protéger en cas de défaillance complète d'une région, créez [plusieurs clusters et répartissez-les dans des régions {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-ha_clusters#multiple_clusters). En configurant un équilibreur de charge de réseau (NLB) pour vos clusters, vous pouvez obtenir un équilibrage de charge interrégional et une mise en réseau interrégionale de vos clusters.

Si vous avez des données qui doivent être disponibles, même en cas de panne, veillez à stocker vos données dans un [stockage persistant](/docs/containers?topic=containers-storage_planning#storage_planning).

Pour plus d'informations sur les moyens d'obtenir la haute disponibilité pour votre cluster, voir [Haute disponibilité pour {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-ha#ha).

## Quelles sont les options à ma disposition pour sécuriser mon cluster ?
{: #secure_cluster}
{: faq}

Vous pouvez utiliser des fonctions de sécurité intégrées dans {{site.data.keyword.containerlong_notm}} pour protéger les composants dans votre cluster, vos données et les déploiements d'application afin d'assurer la conformité en matière de sécurité et l'intégrité des données. Utilisez ces fonctions pour sécuriser votre serveur d'API Kubernetes, le magasin de données etcd, les noeuds worker, les réseaux, le stockage, les images et les déploiements contre les attaques malveillantes. Vous pouvez également tirer parti des outils de consignation et de surveillance pour détecter les attaques malveillantes et les signes d'utilisation suspecte.

Pour plus d'informations sur les composants de votre cluster et savoir comment sécuriser chaque composant, voir [Sécurité d'{{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-security#security).

## Quelles politiques d'accès dois-je fournir à mes utilisateurs de cluster ?
{: #faq_access}
{: faq}

{{site.data.keyword.containerlong_notm}} utilise {{site.data.keyword.iamshort}} (IAM) pour octroyer un accès aux ressources de cluster via des rôles de plateforme IAM et des politiques de contrôle d'accès à base de rôles (RBAC) Kubernetes via des rôles de service IAM. Pour plus d'informations sur les types de règle d'accès, voir [Sélection de la règle d'accès et du rôle appropriés pour vos utilisateurs](/docs/containers?topic=containers-users#access_roles).
{: shortdesc}

Les règles d'accès que vous affectez aux utilisateurs varient selon ce que vous souhaitez que vos utilisateurs puissent effectuer. Vous trouverez plus d'informations sur les types d'action autorisés par chaque rôle sur la [page de référence d'accès utilisateur](/docs/containers?topic=containers-access_reference) ou en cliquant sur les liens contenus dans le tableau ci-après. Pour savoir comment affecter des règles d'accès, voir [Octroi d'accès utilisateur à votre cluster via {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform).

| Cas d'utilisation | Exemples de rôle et portée |
| --- | --- |
| Auditeur d'application | [Rôle de plateforme Afficheur pour un cluster, une région ou un groupe de ressources](/docs/containers?topic=containers-access_reference#view-actions), [Rôle de service Lecteur pour un cluster, une région ou un groupe de ressources](/docs/containers?topic=containers-access_reference#service). |
|Développeurs d'applications| [Rôle de plateforme Editeur pour un cluster](/docs/containers?topic=containers-access_reference#editor-actions), [Rôle de service Auteur pour un espace de nom](/docs/containers?topic=containers-access_reference#service), [Rôle d'espace de développeur Cloud Foundry](/docs/containers?topic=containers-access_reference#cloud-foundry). |
| Facturation | [Rôle de plateforme Afficheur pour un cluster, une région ou un groupe de ressources](/docs/containers?topic=containers-access_reference#view-actions). |
| Créer un cluster | Droits de niveau compte pour des données d'identification de l'infrastructure Superutilisateur, rôle de plateforme Administrateur sur {{site.data.keyword.containerlong_notm}} et rôle de plateforme Administrateur sur {{site.data.keyword.registrylong_notm}}. Pour plus d'informations, voir [Préparation à la création des clusters](/docs/containers?topic=containers-clusters#cluster_prepare).|
|Administrateur de cluster| [Rôle de plateforme Administrateur pour un cluster](/docs/containers?topic=containers-access_reference#admin-actions), [Rôle de service Responsable dont la portée ne se limite pas à un espace de nom (pour l'ensemble du cluster)](/docs/containers?topic=containers-access_reference#service).|
| Opérateur DevOps | [Rôle de plateforme Opérateur pour un cluster](/docs/containers?topic=containers-access_reference#operator-actions), [Rôle de service Auteur dont la portée ne se limite pas à un espace de nom (pour l'ensemble du cluster)](/docs/containers?topic=containers-access_reference#service), [Rôle d'espace de développeur Cloud Foundry](/docs/containers?topic=containers-access_reference#cloud-foundry).  |
| Opérateur ou ingénieur SRE | [Rôle de plateforme Administrateur pour un cluster, une région ou un groupe de ressources](/docs/containers?topic=containers-access_reference#admin-actions), [Rôle de service Lecteur pour un cluster ou une région](/docs/containers?topic=containers-access_reference#service) ou [Rôle de service Responsable pour tous les espaces de nom de cluster](/docs/containers?topic=containers-access_reference#service) afin de pouvoir exécuter des commandes `kubectl top nodes,pods`. |
{: caption="Types de rôle que vous pouvez affecter en réponse à différents cas d'utilisation." caption-side="top"}

## Où trouver une liste des bulletins de sécurité concernant mon cluster ?
{: #faq_security_bulletins}
{: faq}

En cas de vulnérabilités détectées dans Kubernetes, Kubernetes publie des CVE dans des bulletins de sécurité pour informer les utilisateurs et indiquer les actions que doivent effectuer les utilisateurs pour résoudre ces vulnérabilités. Les bulletins de sécurité qui concernent les utilisateurs {{site.data.keyword.containerlong_notm}} ou la plateforme {{site.data.keyword.Bluemix_notm}} sont publiés dans les [bulletins de sécurité {{site.data.keyword.Bluemix_notm}}](https://cloud.ibm.com/status?component=containers-kubernetes&selected=security).

Certaines vulnérabilités (CVE) nécessitent une mise à jour de correctif de dernier niveau pour une version Kubernetes que vous pouvez installer dans le cadre d'un [processus de mise à jour de cluster](/docs/containers?topic=containers-update#update) normal dans {{site.data.keyword.containerlong_notm}}. Veillez à appliquer les correctifs de sécurité à temps pour protéger votre cluster contre les attaques malveillantes. Pour plus d'informations sur le contenu d'un correctif de sécurité, consultez le [journal des modifications de version](/docs/containers?topic=containers-changelog#changelog).

## Est-ce que le service propose la prise en charge des processeurs graphiques (GPU) et de la technologie bare metal ?
{: #bare_metal_gpu}
{: faq}

Oui, vous pouvez mettre à disposition votre noeud worker sous forme de serveur bare metal physique à service exclusif. Les serveurs bare metal offrent des avantages en termes de hautes performances pour les charges de travail, telles que les données, l'intelligence artificielle ou les processeurs graphiques (GPU). De plus, toutes les ressources matérielles sont dédiées à vos charges de travail, vous n'avez donc pas à vous soucier des "voisins bruyants".

Pour plus d'informations sur les versions bare metal disponibles et les différences entre les machines bare metal et les machines virtuelles, voir [Machines physiques (bare metal)](/docs/containers?topic=containers-planning_worker_nodes#bm).

## Quelles sont les versions de Kubernetes prises en charge par le service ?
{: #supported_kube_versions}
{: faq}

{{site.data.keyword.containerlong_notm}} prend en charge plusieurs versions de Kubernetes simultanément. Lorsque la version la plus récente (n) est publiée, jusqu'à 2 versions antérieures (n-2) sont prises en charge. Les versions au-delà de deux versions avant la version la plus récente (n-3) sont d'abord dépréciées, puis finissent par ne plus être prises en charge. Les versions suivantes sont prises en charge actuellement :

*   La plus récente : 1.14.2
*   Par défaut : 1.13.6
*   Autre : 1.12.9

Pour plus d'informations sur les versions prises en charge et les actions de mise à jour que vous devez entreprendre pour passer d'une version à une autre, voir [Informations de version et actions de mise à jour](/docs/containers?topic=containers-cs_versions#cs_versions).

## Où est disponible ce service ?
{: #supported_regions}
{: faq}

{{site.data.keyword.containerlong_notm}} est disponible dans le monde entier. Vous pouvez créer des clusters standard dans toutes les régions {{site.data.keyword.containerlong_notm}} prises en charge. Les clusters gratuits ne sont disponibles que dans certaines régions.

Pour plus d'informations sur les régions prises en charge, voir [Emplacements](/docs/containers?topic=containers-regions-and-zones#regions-and-zones).

## Quelles sont les normes auxquelles le service est conforme ?
{: #standards}
{: faq}

{{site.data.keyword.containerlong_notm}} implémente des contrôles de conformité aux normes suivantes :
- Cadre Privacy Shield Suisse/Etats-Unis et Privacy Shield UE-EU
- Health Insurance Portability and Accountability Act (HIPAA)
- Normes Service Organization Control (SOC 1, SOC 2 Type 1)
- International Standard on Assurance Engagements 3402 (ISAE 3402), rapports d'assurance quant à la fiabilité des contrôles au niveau de l'organisation des services
- International Organization for Standardization (ISO 27001, ISO 27017, ISO 27018)
- Payment Card Industry Data Security Standard (PCI DSS)

## Puis-je utiliser IBM Cloud et d'autres services avec mon cluster ?
{: #faq_integrations}
{: faq}

Vous pouvez ajouter des services d'infrastructure et de plateforme {{site.data.keyword.Bluemix_notm}}, ainsi que des services de fournisseurs tiers à votre cluster {{site.data.keyword.containerlong_notm}} pour activer l'automatisation, renforcer la sécurité ou améliorer vos fonctions de surveillance et de consignation dans le cluster.

Pour obtenir la liste des services pris en charge, voir [Intégration de services](/docs/containers?topic=containers-supported_integrations#supported_integrations).

## Puis-je connecter mon cluster dans IBM Cloud Public à des applications qui s'exécutent dans mon centre de données local ?
{: #hybrid}
{: faq}

Vous pouvez connecter des services dans {{site.data.keyword.Bluemix_notm}} Public à votre centre de données local pour créer votre propre configuration de cloud hybride. Exemples pour savoir comment vous pouvez tirer parti d'{{site.data.keyword.Bluemix_notm}} Public et Private avec des applications qui s'exécutent dans votre centre de données local :
- Vous créez un cluster avec {{site.data.keyword.containerlong_notm}} dans {{site.data.keyword.Bluemix_notm}} Public, mais vous souhaitez connecter votre cluster à une base de données locale.
- Vous créez un cluster Kubernetes dans {{site.data.keyword.Bluemix_notm}} Private au sein de votre propre centre de données et vous déployez des applications sur votre cluster. Toutefois, votre application peut utiliser un service {{site.data.keyword.ibmwatson_notm}}, tel que Tone Analyzer, dans {{site.data.keyword.Bluemix_notm}} Public.

Pour permettre la communication entre des services qui s'exécutent dans {{site.data.keyword.Bluemix_notm}} Public et des services qui s'exécutent en local, vous devez [configurer une connexion VPN](/docs/containers?topic=containers-vpn#vpn). Pour connecter votre environnement {{site.data.keyword.Bluemix_notm}} Public ou Dedicated à un environnement {{site.data.keyword.Bluemix_notm}} Private, voir [Utilisation d'{{site.data.keyword.containerlong_notm}} avec {{site.data.keyword.Bluemix_notm}} Private](/docs/containers?topic=containers-hybrid_iks_icp#hybrid_iks_icp).

Pour obtenir une présentation des offres {{site.data.keyword.containerlong_notm}} prises en charge, voir [Comparaison d'offres et de leurs combinaisons](/docs/containers?topic=containers-cs_ov#differentiation).

## Puis-je déployer IBM Cloud Kubernetes Service dans mon propre centre de données ?
{: #private}
{: faq}

Si vous n'envisagez pas de déplacer vos applications dans {{site.data.keyword.Bluemix_notm}} Public, mais que vous souhaitez toujours tirer parti des fonctions d'{{site.data.keyword.containerlong_notm}}, vous pouvez installer {{site.data.keyword.Bluemix_notm}} Private. {{site.data.keyword.Bluemix_notm}} Private est une plateforme d'application pouvant être installée en local sur vos propres machines et que vous pouvez utiliser pour développer et gérer des applications conteneurisées sur site dans votre propre environnement contrôlé derrière un pare-feu.

Pour plus d'informations, voir la [documentation du produit {{site.data.keyword.Bluemix_notm}} Private ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_1.2.0/kc_welcome_containers.html).

## Comment suis-je facturé lorsque j'utilise IBM Cloud Kubernetes Service ?
{: #charges}
{: faq}

Avec les clusters {{site.data.keyword.containerlong_notm}}, vous pouvez utiliser les ressources de calcul, de réseau et de stockage de l'infrastructure IBM Cloud (SoftLayer) avec des services de plateforme, tels que Watson AI ou la base de données DaaS (Database-as-a-Service) Compose. Chaque ressource peut induire ses propres frais pouvant être [fixes, mesurés, étalés sur plusieurs niveaux ou réservés](/docs/billing-usage?topic=billing-usage-charges#charges).
* [Noeuds worker](#nodes)
* [Réseau sortant](#bandwidth)
* [Adresses IP de sous-réseau](#subnet_ips)
* [Stockage](#persistent_storage)
* [Services {{site.data.keyword.Bluemix_notm}}](#services)
* [Red Hat OpenShift on IBM Cloud](#rhos_charges)

<dl>
<dt id="nodes">Noeuds worker</dt>
  <dd><p>Les clusters peuvent comporter deux types de noeud worker principaux : machines virtuelles ou physiques (bare metal). La disponibilité des types de machine et leur facturation varient suivant la zone dans laquelle vous déployez votre cluster.</p>
  <p>Les <strong>machines virtuelles</strong> présentent une plus grande flexibilité, une durée de mise à disposition plus rapide et plus de fonctions de mise à l'échelle automatique qu'une machine bare metal, pour un meilleur rapport qualité-prix. Toutefois, les machines virtuelles offrent un avantage non négligeable en termes de performances par rapport aux spécifications bare metal, par exemple le débit en Gbit/s sur les réseaux, les seuils de mémoire et de RAM et les options de stockage. Tenez compte de ces facteurs qui ont un impact sur le coût de votre machine virtuelle :</p>
  <ul><li><strong>Matériel partagé ou dédié</strong> : si vous partagez le matériel sous-jacent de la machine virtuelle, le coût est inférieur à du matériel dédié, mais les ressources physiques ne sont pas dédiées à votre machine virtuelle.</li>
  <li><strong>Facturation à l'heure uniquement</strong> : la facturation à l'heure offre une plus grande flexibilité pour commander et annuler rapidement des machines virtuelles.
  <li><strong>Plusieurs tranches horaires par mois</strong> : la facturation à l'heure est différenciée. Lorsque votre machine virtuelle est commandée pour un certain nombre d'heures par mois, le coût horaire qui vous est facturé diminue. Les tranches horaires s'articulent comme suit : 0 à 150 heures, 151 à 290 heures, 291 à 540 heures et 541 heures et plus.</li></ul>
  <p><strong>Les machines physiques (bare metal)</strong> offrent des avantages en termes de hautes performances pour les charges de travail, telles que les données, l'intelligence artificielle et les processeurs graphiques (GPU). De plus, toutes les ressources matérielles sont dédiées à vos charges de travail de sorte à éviter les "voisins bruyants". Tenez compte des facteurs suivants qui ont un impact sur le coût de votre machine bare metal :</p>
  <ul><li><strong>Facturation mensuelle uniquement</strong> : toutes les machines bare metal sont facturées au mois.</li>
  <li><strong>Processus de commande plus long</strong> :  après avoir commandé ou annulé un serveur bare metal, le processus est finalisé manuellement dans votre compte d'infrastructure IBM Cloud (SoftLayer). Par conséquent, son exécution peut prendre plus d'un jour ouvrable.</li></ul>
  <p>Pour obtenir des détails sur les spécifications des machines, voir [Matériel disponible pour les noeuds worker](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes).</p></dd>

<dt id="bandwidth">Bande passante publique</dt>
  <dd><p>La bande passante désigne le transfert de données publiques du trafic réseau entrant et sortant, à destination et en provenance de ressources {{site.data.keyword.Bluemix_notm}} dans des centres de données situés dans le monde entier. La bande passante publique est facturée par Go. Vous pouvez consulter le récapitulatif de votre bande passante en vous connectant à la [console {{site.data.keyword.Bluemix_notm}}](https://cloud.ibm.com/), en sélectionnant **Infrastructure classique** dans le menu ![Icône de menu](../icons/icon_hamburger.svg "Icône de menu"), puis en sélectionnant la page **Réseau > Bande passante > Récapitulatif**.
  <p>Examinez les facteurs suivants qui ont une incidence sur les frais liés à la bande passante publique :</p>
  <ul><li><strong>Emplacement</strong> : comme pour les noeuds worker, les frais varient en fonction de la zone dans laquelle sont déployées vos ressources.</li>
  <li><strong>Bande passante incluse ou Paiement à la carte</strong> : les machines de vos noeuds worker peuvent être fournies avec une allocation de réseau sortant par mois, par exemple 250 Go pour les machines virtuelles ou 500 Go pour les machines bare metal. Ou bien, l'allocation peut être de type Paiement à la carte en fonction du nombre de Go utilisés.</li>
  <li><strong>Packages à plusieurs niveaux</strong> : lorsque vous avez consommé la bande passante incluse, vous êtes facturé en fonction d'un schéma comportant plusieurs niveaux d'utilisation qui varie selon l'emplacement. Si vous dépassez l'allocation correspondant à un niveau, des frais de transfert de données standard peuvent vous être facturés.</li></ul>
  <p>Pour plus d'informations, voir la page [Bandwidth Packages![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/cloud/bandwidth).</p></dd>

<dt id="subnet_ips">Adresses IP de sous-réseau</dt>
  <dd><p>Lorsque vous créez un cluster standard, un sous-réseau portable public avec 8 adresses IP publiques est commandé et facturé sur votre compte tous les mois.</p><p>Si votre compte d'infrastructure comporte déjà des sous-réseaux disponibles, vous pouvez utiliser ces sous-réseaux à la place. Créez le cluster avec l'`--no-subnets` [indicateur](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_create), puis [réutilisez vos sous-réseaux](/docs/containers?topic=containers-subnets#subnets_custom).</p>
  </dd>

<dt id="persistent_storage">Stockage</dt>
  <dd>Lorsque vous mettez du stockage à disposition, vous pouvez choisir le type et la classe de stockage qui conviennent à votre cas d'utilisation. Les frais facturés varient selon le type de stockage, l'emplacement et les spécifications de l'instance de stockage. Certaines solutions de stockage, par exemple le stockage de fichiers ou le stockage par blocs, vous proposent une sélection de plans horaires ou mensuels. Pour choisir la solution de stockage adaptée, voir [Planification de stockage persistant à haute disponibilité](/docs/containers?topic=containers-storage_planning#storage_planning). Pour plus d'informations, voir :
  <ul><li>[Tarification de stockage de fichiers NFS![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/cloud/file-storage/pricing)</li>
  <li>[Tarification de stockage par blocs![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/cloud/block-storage/pricing)</li>
  <li>[Plans de stockage d'objets![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api)</li></ul></dd>

<dt id="services">Services {{site.data.keyword.Bluemix_notm}}</dt>
  <dd>Chaque service que vous intégrez avec votre cluster a son propre barème de prix. Consultez la documentation de chaque produit et utilisez la console {{site.data.keyword.Bluemix_notm}} pour [estimer les coûts](/docs/billing-usage?topic=billing-usage-cost#cost).</dd>

<dt id="rhos_charges">Red Hat OpenShift on IBM Cloud</dt>
  <dd>
  <p class="preview">[Red Hat OpenShift on IBM Cloud](/docs/containers?topic=containers-openshift_tutorial) est disponible en version bêta afin de pouvoir tester les clusters OpenShift.
</p>Si vous créez un [cluster Red Hat OpenShift on IBM Cloud](/docs/containers?topic=containers-openshift_tutorial), vos noeuds worker sont installés avec le système d'exploitation Red Hat Enterprise Linux, ce qui augmente le prix des [machines de noeud worker](#nodes). Vous devez également disposer d'une licence OpenShift, ce qui implique des frais mensuels en plus des coûts horaires de machine virtuelle ou des coûts mensuels de serveur bare metal. La licence OpenShift couvre chaque tranche de 2 coeurs de la version de noeud worker. Si vous supprimez votre noeud worker avant la fin du mois, votre licence mensuelle est disponible pour les autres noeuds worker du pool. Pour plus d'informations sur les clusters OpenShift, voir [Création d'un cluster Red Hat OpenShift on IBM Cloud](/docs/containers?topic=containers-openshift_tutorial).</dd>

</dl>
<br><br>

Les ressources mensuelles sont facturées le premier jour du mois pour une utilisation le mois précédent. Si vous commandez une ressource mensuelle au milieu du mois, vous êtes facturé au prorata pour ce mois. Cependant, si vous annulez une ressource mensuelle au milieu du mois, elle vous sera facturée pour le mois complet.
{: note}

## Ma plateforme et mes ressources d'infrastructure sont-elles consolidées dans une seule facture ?
{: #bill}
{: faq}

Lorsque vous utilisez un compte {{site.data.keyword.Bluemix_notm}} facturable, les ressources de l'infrastructure et de la plateforme sont récapitulées dans une facture.
Si vous avez associé votre compte {{site.data.keyword.Bluemix_notm}} et votre compte d'infrastructure IBM Cloud (SoftLayer), vous recevez une [facture consolidée](/docs/customer-portal?topic=customer-portal-unifybillaccounts#unifybillaccounts) pour vos ressources de plateforme et d'infrastructure {{site.data.keyword.Bluemix_notm}}.

## Puis-je faire une estimation des coûts ?
{: #cost_estimate}
{: faq}

Oui, voir [Estimation des coûts](/docs/billing-usage?topic=billing-usage-cost#cost). N'oubliez pas que certains frais ne sont pas comptabilisés dans l'estimation, par exemple la tarification différenciée en cas d'augmentation de l'utilisation horaire. Pour plus d'informations, voir [Comment suis-je facturé lorsque j'utilise {{site.data.keyword.containerlong_notm}} ?](#charges).

## Puis-je suivre mon utilisation actuelle ?
{: #usage}
{: faq}

Vous pouvez vérifier votre utilisation actuelle et les totaux mensuels estimés pour vos ressources d'infrastructure et de plateforme {{site.data.keyword.Bluemix_notm}}. Pour plus d'informations, voir [Affichage de votre utilisation](/docs/billing-usage?topic=billing-usage-viewingusage#viewingusage). Pour organiser votre facturation, vous pouvez regrouper vos ressources dans des [groupes de ressources](/docs/resources?topic=resources-bp_resourcegroups#bp_resourcegroups).
