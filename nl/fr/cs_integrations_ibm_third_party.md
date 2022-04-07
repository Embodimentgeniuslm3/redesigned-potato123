---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-07"

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


# Services IBM Cloud et intégrations tierces
{: #ibm-3rd-party-integrations}

Vous pouvez utiliser des services d'infrastructure et de plateforme {{site.data.keyword.Bluemix_notm}} et d'autres intégrations tierces pour ajouter des fonctions supplémentaires à votre cluster.
{: shortdesc}

## Services IBM Cloud
{: #ibm-cloud-services}

Passez en revue les informations ci-après pour voir de quelle façon les services d'infrastructure et de plateforme {{site.data.keyword.Bluemix_notm}} sont intégrés à {{site.data.keyword.containerlong_notm}} et découvrir comment les utiliser dans votre cluster.
{: shortdesc}

### Services de plateforme IBM Cloud
{: #platform-services}

Tous les services de plateforme {{site.data.keyword.Bluemix_notm}} prenant en charge des clés de service peuvent être intégrés à l'aide de la [liaison de service](/docs/containers?topic=containers-service-binding) {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

Une liaison de service est un moyen de créer rapidement des données d'identification de service pour un service {{site.data.keyword.Bluemix_notm}} et de stocker ces données dans une valeur confidentielle Kubernetes dans votre cluster. La valeur confidentielle est chiffrée automatiquement dans etcd pour protéger vos données. Vos applications peuvent utiliser les données d'identification de la valeur confidentielle pour accéder à votre instance de service {{site.data.keyword.Bluemix_notm}}.

Les services qui ne prennent pas en charge les clés de service fournissent généralement une API que vous pouvez utiliser directement dans votre application. 

Pour obtenir une présentation des services {{site.data.keyword.Bluemix_notm}} populaires, voir [Intégrations populaires](/docs/containers?topic=containers-supported_integrations#popular_services).

### Services d'infrastructure IBM Cloud
{: #infrastructure-services}

Etant donné qu'{{site.data.keyword.containerlong_notm}} vous permet de créer un cluster Kubernetes qui est basé sur l'infrastructure {{site.data.keyword.Bluemix_notm}}, certains services d'infrastructure, comme les serveurs virtuels, les serveurs bare metal ou les VLAN, sont entièrement intégrés dans  {{site.data.keyword.containerlong_notm}}. Vous créez et gérez ces instances de service à l'aide de l'API, de l'interface CLI ou de la console {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

Des solutions de stockage persistant prises en charge, telles que {{site.data.keyword.Bluemix_notm}} File Storage, {{site.data.keyword.Bluemix_notm}} Block Storage ou {{site.data.keyword.cos_full}}, sont intégrées en tant que pilotes flexibles Kubernetes et peuvent être configurées à l'aide de [chartes Helm](/docs/containers?topic=containers-helm). La charte Helm configure automatiquement des classes de stockage Kubernetes, le fournisseur de stockage et le pilote de stockage dans votre cluster. Vous pouvez utiliser les classes de stockage pour mettre à disposition un stockage persistant à l'aide de réservations de volume persistant. 

Pour sécuriser votre réseau de cluster ou vous connecter à un centre de données sur site, vous pouvez configurer l'une des options suivantes :
- [Service VPN IPSec strongSwan](/docs/containers?topic=containers-vpn#vpn-setup)
- [{{site.data.keyword.BluDirectLink}}](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link)
- [Fonction de routeur virtuel (VRA)](/docs/containers?topic=containers-vpn#vyatta)
- [Dispositif de sécurité Fortigate (FSA)](/docs/services/vmwaresolutions/services?topic=vmware-solutions-fsa_considerations)

## Communauté Kubernetes et intégrations open source
{: #kube-community-tools}

Etant donné vous êtes propriétaire des clusters standard que vous créez dans {{site.data.keyword.containerlong_notm}}, vous pouvez choisir d'installer des solutions tierces afin d'ajouter des fonctions supplémentaires à votre cluster.
{: shortdesc}

Certaines technologies open source, comme Knative, Istio, LogDNA, Sysdig ou Portworx, sont testées par IBM et fournies en tant que modules complémentaires gérés, chartes Helm ou services {{site.data.keyword.Bluemix_notm}} qui sont exploités par le fournisseur de service en partenariat avec IBM. Ces outils open source sont entièrement intégrés au système de facturation et de support {{site.data.keyword.Bluemix_notm}}. 

Vous pouvez installer d'autres outils open source dans votre cluster, mais ces outils peuvent ne pas être gérés, pris en charge ou vérifiés pour fonctionner dans {{site.data.keyword.containerlong_notm}}.

### Intégrations exploitées en partenariat
{: #open-source-partners}

Pour plus d'informations sur les partenaires {{site.data.keyword.containerlong_notm}} et les avantages liés à l'utilisation de chaque solution fournie par ces partenaires, voir [Partenaires {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-service-partners).

### Modules complémentaires gérés
{: #cluster-add-ons}
{{site.data.keyword.containerlong_notm}} fournit des intégrations open source populaires, telles que   [Knative](/docs/containers?topic=containers-serverless-apps-knative) ou [Istio](/docs/containers?topic=containers-istio), à l'aide de [modules complémentaires gérés](/docs/containers?topic=containers-managed-addons). Les modules complémentaires gérés sont un moyen, testé par IBM et approuvé pour être utilisé dans {{site.data.keyword.containerlong_notm}}, d'installer facilement un outil open source dans votre cluster. 

Les modules complémentaires gérés sont entièrement intégrés dans l'organisation de support {{site.data.keyword.Bluemix_notm}}. Pour toute question ou problématique relative à l'utilisation des modules complémentaires gérés, vous pouvez utiliser l'un des canaux de support {{site.data.keyword.containerlong_notm}}. Pour plus d'informations, voir [Aide et assistance](/docs/containers?topic=containers-cs_troubleshoot_clusters#clusters_getting_help).

Si l'outil que vous ajoutez à votre cluster entraîne des coûts, ceux-ci sont automatiquement intégrés et répertoriés dans le cadre de votre facturation {{site.data.keyword.Bluemix_notm}}. Le cycle de facturation est déterminé par {{site.data.keyword.Bluemix_notm}} en fonction du moment où vous activez le module complémentaire dans votre cluster.

### Autres intégrations tierces
{: #kube-community-helm}

Vous pouvez installer n'importe quel outil open source tiers qui s'intègre à kubernetes. Par exemple, la communauté Kubernetes désigne certaines chartes Helm comme étant de type `stable` ou `incubator`. Notez que le fonctionnement de ces chartes ou outils dans {{site.data.keyword.containerlong_notm}} n'est pas vérifié. Si l'outil requiert une licence, vous devez acheter une licence avant de pouvoir l'utiliser. Pour obtenir une présentation des chartes Helm disponibles auprès de la communauté Kubernetes, voir les référentiels `kubernetes` et `kubernetes-incubator` dans le catalogue de [chartes Helm![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/kubernetes/solutions/helm-charts).
{: shortdesc}

Les coûts engendrés par l'utilisation d'une intégration open source tierce ne sont pas inclus dans votre facture {{site.data.keyword.Bluemix_notm}} mensuelle.

L'installation d'intégrations open source tierces ou de chartes Helm provenant de la communauté Kubernetes peut modifier la configuration par défaut de votre cluster et placer celui-ci dans un état non pris en charge. En cas de problèmes lors de l'utilisation de l'un de ces outils, consultez la communauté Kubernetes ou le fournisseur de services directement.
{: important}
