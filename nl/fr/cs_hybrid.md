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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}



# Cloud hybride
{: #hybrid_iks_icp}

Si vous disposez d'un compte {{site.data.keyword.Bluemix}} Private, vous pouvez l'utiliser avec une sélection de services {{site.data.keyword.Bluemix_notm}}, notamment {{site.data.keyword.containerlong}}. Pour plus d'informations, voir le blogue concernant l'[expérience hybride sur {{site.data.keyword.Bluemix_notm}} Private et IBM Cloud Public![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](http://ibm.biz/hybridJune2018).
{: shortdesc}

Vous comprenez les [offres {{site.data.keyword.Bluemix_notm}} ](/docs/containers?topic=containers-cs_ov#differentiation) et avez développé votre stratégie Kubernetes en fonction des [charges de travail à exécuter sur le cloud](/docs/containers?topic=containers-strategy#cloud_workloads). A présent, vous pouvez connecter votre cloud public et privé à l'aide du service VPN strongSwan ou de {{site.data.keyword.BluDirectLink}}.

* Le [service VPN strongSwan](#hybrid_vpn) connecte votre cluster Kubernetes à un réseau sur site de façon sécurisée via un canal de communication de bout en bout sécurisé sur Internet, basé sur l'ensemble de protocoles IPSec (Internet Protocol Security) aux normes de l'industrie. 
* Avec [{{site.data.keyword.Bluemix_notm}} Direct Link](#hybrid_dl), vous pouvez créer une connexion privée directe entre vos environnements de réseau distants et {{site.data.keyword.containerlong_notm}} sans routage sur l'internet public. 

Après avoir connecté votre cloud public et privé, vous pouvez [réutiliser vos packages privés pour des conteneurs publics](#hybrid_ppa_importer).

## Connexion de votre cloud public et de votre cloud privé au réseau privé virtuel (VPN) strongSwan
{: #hybrid_vpn}

Etablissez une connexion VPN entre votre cluster Kubernetes public et votre instance {{site.data.keyword.Bluemix}} Private pour autoriser la communication bidirectionnelle.
{: shortdesc}

1.  Créez un cluster standard avec {{site.data.keyword.containerlong}} dans {{site.data.keyword.Bluemix_notm}} Public ou utilisez un cluster existant. Pour créer un cluster, choisissez entre les options suivantes :
    - [Créer un cluster standard à partir de la console de ou l'interface CLI](/docs/containers?topic=containers-clusters#clusters_ui).
    - [Utiliser Cloud Automation Manager (CAM) pour créer un cluster à partir d'un modèle prédéfini![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/support/knowledgecenter/SS2L37_2.1.0.3/cam_deploy_IKS.html). Lorsque vous déployez un cluster avec CAM, Helm Tiller est automatiquement installé pour vous.

2.  Dans votre cluster {{site.data.keyword.containerlong_notm}}, [suivez les instructions indiquées pour configurer le service VPN IPSec strongSwan](/docs/containers?topic=containers-vpn#vpn_configure).

    *  Pour l'[étape 2](/docs/containers?topic=containers-vpn#strongswan_2), notez ceci :

       * La valeur de `local.id` que vous avez définie dans votre cluster {{site.data.keyword.containerlong_notm}} doit correspondre à la valeur que vous avez définie par la suite pour `remote.id` dans votre cluster {{site.data.keyword.Bluemix}} Private.
       * La valeur de `remote.id` que vous avez définie dans votre cluster {{site.data.keyword.containerlong_notm}} doit correspondre à la valeur que vous avez définie par la suite pour `local.id` dans votre cluster {{site.data.keyword.Bluemix}} Private.
       * La valeur de `preshared.secret` que vous avez définie dans votre cluster {{site.data.keyword.containerlong_notm}} doit correspondre à la valeur que vous avez définie par la suite pour `preshared.secret` dans votre cluster {{site.data.keyword.Bluemix}} Private.

    *  Pour l'[étape 3](/docs/containers?topic=containers-vpn#strongswan_3), configurez strongSwan pour une connexion VPN **entrante**.

       ```
       ipsec.auto: add
       loadBalancerIP: <portable_public_IP>
       ```
       {: codeblock}

3.  Notez l'adresse IP publique portable que vous avez définie comme `loadbalancerIP`.

    ```
    kubectl get svc vpn-strongswan
    ```
    {: pre}

4.  [Créez un cluster dans {{site.data.keyword.Bluemix_notm}} Private![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/installing/installing.html).

5.  Dans votre cluster {{site.data.keyword.Bluemix_notm}} Private, déployez le service VPN IPSec strongSwan.

    1.  [Appliquez les solutions de contournement du VPN IPSec strongSwan ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/support/knowledgecenter/SS2L37_2.1.0.3/cam_strongswan.html).

    2.  [Configurez la charte Helm du VPN strongSwan ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/app_center/create_release.html) dans votre cluster privé.

        *  Dans les paramètres de configuration, définissez la zone de la passerelle distante (**Remote gateway**) avec la valeur de l'adresse IP publique portable que vous avez définie comme `loadbalancerIP` de votre cluster {{site.data.keyword.containerlong_notm}}.

           ```
           Operation at startup: start
           ...
           Remote gateway: <portable_public_IP>
           ...
           ```
           {: codeblock}

        *  N'oubliez pas que la valeur `local.id` privée doit correspondre à la valeur `remote.id` publique, la valeur `remote.id` privée doit correspondre à la valeur `local.id` publique, et les valeurs `preshared.secret` publiques et privées doivent correspondre.

        Vous pouvez maintenant établir une connexion entre le cluster {{site.data.keyword.Bluemix_notm}} Private et le cluster {{site.data.keyword.containerlong_notm}}.

7.  [Testez la connexion VPN](/docs/containers?topic=containers-vpn#vpn_test) entre vos clusters.

8.  Répétez ces étapes pour chaque cluster que vous souhaitez connecter.

**Etape suivante ?**

*   [Exécutez vos images logicielles sous licence dans des clusters publics](#hybrid_ppa_importer).
*   Pour gérer plusieurs clusters Kubernetes en cloud, par exemple, dans {{site.data.keyword.Bluemix_notm}} Public et {{site.data.keyword.Bluemix_notm}} Private, voir [IBM Multicloud Manager ![Icône de lien etxterne](../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.1.0/mcm/getting_started/introduction.html).


## Connexion de votre cloud public et privé à l'aide de {{site.data.keyword.Bluemix_notm}} Direct Link
{: #hybrid_dl}

Avec [{{site.data.keyword.BluDirectLink}}](/docs/infrastructure/direct-link?topic=direct-link-about-ibm-cloud-direct-link), vous pouvez créer une connexion privée directe entre vos environnements de réseau distants et {{site.data.keyword.containerlong_notm}} sans routage sur l'internet public.
{: shortdesc}

Pour connecter votre cloud public et votre instance {{site.data.keyword.Bluemix}} Private sur site, vous pouvez utiliser l'une des quatre offres suivantes :
* {{site.data.keyword.Bluemix_notm}} Direct Link Connect
* {{site.data.keyword.Bluemix_notm}} Direct Link Exchange
* {{site.data.keyword.Bluemix_notm}} Direct Link Dedicated
* {{site.data.keyword.Bluemix_notm}} Direct Link Dedicated Hosting

Pour choisir une offre {{site.data.keyword.Bluemix_notm}} Direct Link et configurer une connexion {{site.data.keyword.Bluemix_notm}} Direct Link, voir [Initiation à  {{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link#how-do-i-know-which-type-of-ibm-cloud-direct-link-i-need-) dans la documentation {{site.data.keyword.Bluemix_notm}} Direct Link. 

**Etape suivante ?**</br>
* [Exécutez vos images logicielles sous licence dans des clusters publics](#hybrid_ppa_importer).
* Pour gérer plusieurs clusters Kubernetes en cloud, par exemple, dans {{site.data.keyword.Bluemix_notm}} Public et {{site.data.keyword.Bluemix_notm}} Private, voir [IBM Multicloud Manager ![Icône de lien etxterne](../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.1.0/mcm/getting_started/introduction.html).

<br />


## Exécution d'images {{site.data.keyword.Bluemix_notm}} Private dans des conteneurs Kubernetes publics
{: #hybrid_ppa_importer}

Vous pouvez exécuter une sélection de produits IBM sous licence conditionnés pour {{site.data.keyword.Bluemix_notm}} Private sur un cluster dans {{site.data.keyword.Bluemix_notm}} Public.  
{: shortdesc}

Le logiciel sous licence est disponible dans [IBM Passport Advantage ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www-01.ibm.com/software/passportadvantage/index.html). Pour utiliser ce logiciel dans un cluster d'{{site.data.keyword.Bluemix_notm}} Public, vous devez télécharger le logiciel, puis extraire et transférer l'image dans votre espace de nom dans {{site.data.keyword.registryshort}}. Indépendamment de l'environnement que vous envisagez d'utiliser pour le logiciel, vous devez d'abord obtenir la licence obligatoire pour utiliser le produit.

Le tableau suivant est une présentation des produits {{site.data.keyword.Bluemix_notm}} Private disponibles que vous pouvez utiliser sur votre cluster dans {{site.data.keyword.Bluemix_notm}} Public.

| Nom du produit | Version | Référence |
| --- | --- | --- |
| IBM Db2 Direct Advanced Edition Server | 11.1 | CNU3TML |
| IBM Db2 Advanced Enterprise Server Edition Server | 11.1 | CNU3SML |
| IBM MQ Advanced | 9.1.0.0, 9.1.1,0, 9.1.2.0 | - |
| IBM WebSphere Application Server Liberty | 16.0.0.3 | Image Docker Hub |
{: caption="Tableau. Produits {{site.data.keyword.Bluemix_notm}} Private pris en charge à utiliser dans {{site.data.keyword.Bluemix_notm}} Public." caption-side="top"}

Avant de commencer :
- [Installez le plug-in de l'interface CLI {{site.data.keyword.registryshort}} (`ibmcloud cr`)](/docs/services/Registry?topic=registry-registry_setup_cli_namespace#cli_namespace_registry_cli_install).
- [Configurez un espace de nom dans {{site.data.keyword.registryshort}}](/docs/services/Registry?topic=registry-registry_setup_cli_namespace#registry_namespace_setup) ou récupérez votre espace de nom existant en exécutant la commande `ibmcloud cr namespaces`.
- [Ciblez votre interface CLI `kubectl` sur votre cluster](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).
- [Installez l'interface CLI de Helm et configurez Tiller dans votre cluster](/docs/containers?topic=containers-helm#public_helm_install).

Pour déployer une image {{site.data.keyword.Bluemix_notm}} Private sur un cluster dans {{site.data.keyword.Bluemix_notm}} Public :

1.  Suivez les étapes indiquées dans la [documentation d'{{site.data.keyword.registryshort}}](/docs/services/Registry?topic=registry-ts_index#ts_ppa) pour télécharger le logiciel sous licence depuis IBM Passport Advantage, envoyer l'image dans votre espace de nom et installer la charte Helm dans votre cluster.

    **Pour IBM WebSphere Application Server Liberty** :

    1.  Au lieu d'obtenir l'image à partir d'IBM Passport Advantage, utilisez l'[image Docker Hub ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://hub.docker.com/_/websphere-liberty/). Pour obtenir les instructions nécessaires pour obtenir une licence de production, voir [Mise à niveau de l'image Docker Hub pour obtenir une image de production![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/WASdev/ci.docker/tree/master/ga/production-upgrade).

    2.  Suivez les [instructions de la charte Helm Liberty![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/support/knowledgecenter/en/SSEQTP_liberty/com.ibm.websphere.wlp.doc/ae/rwlp_icp_helm.html).

2.  Vérifiez que la zone **STATUS** de la charte Helm indique `DEPLOYED`. Si ce n'est pas le cas, patientez quelques minutes, puis réessayez.
    ```
    helm status <helm_chart_name>
    ```
    {: pre}

3.  Reportez-vous à la documentation spécifique au produit pour plus d'informations sur la configuration et l'utilisation du produit avec votre cluster.

    - [IBM Db2 Direct Advanced Edition Server ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/support/knowledgecenter/en/SSEPGG_11.1.0/com.ibm.db2.luw.licensing.doc/doc/c0070181.html)
    - [IBM MQ Advanced ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/support/knowledgecenter/en/SSFKSJ_9.0.0/com.ibm.mq.helphome.v90.doc/WelcomePagev9r0.html)
    - [IBM WebSphere Application Server Liberty ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/support/knowledgecenter/en/SSEQTP_liberty/as_ditamaps/was900_welcome_liberty.html)
