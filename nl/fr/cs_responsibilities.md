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


# Vos responsabilités liées à l'utilisation d'{{site.data.keyword.containerlong_notm}}
{: #responsibilities_iks}

Prenez connaissance des responsabilités qui vous incombent en matière de gestion de cluster et des dispositions applicables lorsque vous utilisez {{site.data.keyword.containerlong}}.
{:shortdesc}

## Responsabilités en matière de gestion de cluster
{: #responsibilities}

IBM fournit une plateforme cloud d'entreprise destinée à vous permettre de déployer des applications en même temps qu'{{site.data.keyword.Bluemix_notm}} DevOps, l'intelligence artificielle, des données et des services de sécurité. Vous choisissez comment configurer, intégrer et utiliser vos applications et services dans le cloud.
{:shortdesc}

<table summary="Le tableau présente les responsabilités d'IBM et les vôtres. La lecture des lignes s'effectue de gauche à droite, avec des icônes représentant chaque responsabilité dans la colonne 1 et la description correspondante dans la colonne 2.">
<caption>Responsabilités d'IBM et les vôtres</caption>
  <thead>
  <th colspan=2>Responsabilités par type</th>
  </thead>
  <tbody>
    <tr>
    <td align="center"><img src="images/icon_clouddownload.svg" alt="Icône d'un cloud avec une flèche pointant vers le bas"/><br>Infrastructure de cloud</td>
    <td>
    **Responsabilités d'IBM** :
    <ul><li>Déployer un maître dédié hautement disponible entièrement géré dans un compte d'infrastructure appartenant à IBM et sécurisé pour chaque cluster.</li>
    <li>Mettre à disposition des noeuds worker dans votre compte d'infrastructure IBM Cloud (SoftLayer).</li>
    <li>Configurer des composants de gestion de cluster, par exemple, des VLAN et des équilibreurs de charge.</li>
    <li>Répondre aux demandes relatives à davantage d'infrastructure, par exemple, l'ajout et le retrait de noeuds worker, la création de sous-réseaux par défaut et la mise à disposition de volumes de stockage en réponse à des réservations de volume persistant.</li>
    <li>Intégrer des ressources d'infrastructure commandées qui fonctionneront automatiquement avec votre architecture de cluster et qui deviendront disponibles sur vos applications et charges de travail déployées.</li></ul>
    <br><br>
    **Vos responsabilités** :
    <ul><li>Utiliser l'API, l'interface de ligne de commande ou les outils de console fournis pour ajuster la capacité de [calcul](/docs/containers?topic=containers-clusters#clusters) et de [stockage](/docs/containers?topic=containers-storage_planning#storage_planning) et pour ajuster la [configuration réseau](/docs/containers?topic=containers-cs_network_cluster#cs_network_cluster) en fonction des besoins de votre charge de travail.</li></ul><br><br>
    </td>
     </tr>
     <tr>
     <td align="center"><img src="images/icon_tools.svg" alt="Icône en forme de clé"/><br>Cluster géré</td>
     <td>
     **Responsabilités d'IBM** :
     <ul><li>Fournir une suite d'outils destinés à automatiser la gestion de cluster, par exemple l'[API {{site.data.keyword.containerlong_notm}} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://containers.cloud.ibm.com/global/swagger-global-api/), le [plug-in d'interface de ligne de commande](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli) et la [console ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/kubernetes/clusters).</li>
     <li>Appliquer automatiquement les mises à jour de sécurité, de version et de système d'exploitation de correctif du maître. Faites en sorte que les mises à jour majeures et mineures soient mises à votre disposition pour être appliquées.</li>
     <li>Mettre à jour et récupérer des composants {{site.data.keyword.containerlong_notm}} et Kubernetes opérationnels dans le cluster, tels que l'équilibreur de charge d'application Ingress et le plug-in de stockage de fichiers.</li>
     <li>Sauvegarder et récupérer les données dans etcd, par exemple, vos fichiers de configuration de charge de travail Kubernetes.</li>
     <li>Configurer une connexion OpenVPN entre le maître et les noeuds worker lorsque le cluster est créé.</li>
     <li>Surveiller et signaler l'état de santé du maître et des noeuds worker dans les différentes interfaces.</li>
     <li>Fournir les mises à jour de sécurité, de version et de système d'exploitation de correctif majeures et mineures pour les noeuds worker.</li>
     <li>Répondre aux demandes d'automatisation de mise à jour et de récupération des noeuds worker. Fournir la fonction facultative de [récupération automatique de noeud worker](/docs/containers?topic=containers-health#autorecovery).</li>
     <li>Fournir des outils, tels que le [programme de mise à l'échelle automatique de cluster](/docs/containers?topic=containers-ca#ca), afin d'étendre votre infrastructure de cluster.</li>
     </ul>
     <br><br>
     **Vos responsabilités** :
     <ul>
     <li>Utiliser l'API, l'interface de ligne de commande ou les outils de console pour [appliquer](/docs/containers?topic=containers-update#update) les mises à jour de maître Kubernetes majeures et mineures qui sont fournies et les mises à jour de noeud worker majeures et mineures.</li>
     <li>Utiliser l'API, l'interface de ligne de commande ou les outils de console pour [récupérer](/docs/containers?topic=containers-cs_troubleshoot#cs_troubleshoot) vos ressources d'infrastructure ou installer et configurer la fonction facultative de [récupération automatique de noeud worker](/docs/containers?topic=containers-health#autorecovery).</li></ul>
     <br><br></td>
      </tr>
    <tr>
      <td align="center"><img src="images/icon_locked.svg" alt="Icône en forme de verrou"/><br>Environnement très sécurisé</td>
      <td>
      **Responsabilités d'IBM** :
      <ul>
      <li>Gérer des contrôles de conformité à [différentes normes de l'industrie](/docs/containers?topic=containers-faqs#standards), telles que PCI DSS.</li>
      <li>Surveiller, isoler et récupérer le maître cluster.</li>
      <li>Fournir des répliques hautement disponibles du serveur d'API maître Kubernetes, du composant etcd, du planificateur et du gestionnaire de contrôleur afin de les protéger en cas d'indisponibilité du maître.</li>
      <li>Appliquer automatiquement des mises à jour de correctif de sécurité pour le maître et fournir des mises à jour de correctif pour les noeuds worker.</li>
      <li>Activer certains paramètres de sécurité, tels que les disques chiffrés sur des noeuds worker.</li>
      <li>Désactiver certaines actions non sécurisées pour les noeuds worker, par exemple en n'autorisant pas les utilisateurs à ouvrir une session SSH sur l'hôte.</li>
      <li>Chiffrer la communication entre le maître et les noeuds worker avec TLS.</li>
      <li>Fournir des images Linux compatibles avec CIS pour les systèmes d'exploitation de noeud worker.</li>
      <li>Surveiller en continu les images de maître et de noeud worker afin de détecter tout problème de conformité pour les vulnérabilités et la sécurité.</li>
      <li>Mettre à disposition des noeuds worker avec deux partitions de données SSD locales chiffrées avec AES 256 bits.</li>
      <li>Fournir des options pour la connectivité de réseau du cluster, par exemple, des noeuds finaux de service publics et privés.</li>
      <li>Fournir des options pour l'isolement de traitement, par exemple, des machines virtuelles dédiées, bare metal et bare metal avec la fonction de calcul sécurisé (Trusted Compute).</li>
      <li>Intégrer un contrôle d'accès à base de rôles (RBAC) Kubernetes à {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM).</li>
      </ul>
      <br><br>
      **Vos responsabilités** :
      <ul>
      <li>Utiliser l'API, l'interface de ligne de commande ou les outils de console afin d'appliquer les [mises à jour de correctif de sécurité](/docs/containers?topic=containers-changelog#changelog) pour vos noeuds worker.</li>
      <li>Choisir comment configurer votre [réseau de cluster](/docs/containers?topic=containers-plan_clusters) et configurer d'autres [paramètres de sécurité](/docs/containers?topic=containers-security#security) afin de répondre à vos besoins en matière de conformité et de sécurité de votre charge de travail. Le cas échéant, configurer votre [pare-feu](/docs/containers?topic=containers-firewall#firewall).</li></ul>
      <br><br></td>
      </tr>

      <tr>
        <td align="center"><img src="images/icon_code.svg" alt="Icône en forme de crochets de code"/><br>Orchestration d'application</td>
        <td>
        **Responsabilités d'IBM** :
        <ul>
        <li>Mettre à disposition des clusters avec des composants Kubernetes installés de sorte que vous puissiez accéder à l'API Kubernetes.</li>
        <li>Fournir un certain nombre de modules complémentaires gérés afin d'étendre les fonctions de votre application, par exemple, [Istio](/docs/containers?topic=containers-istio#istio) et [Knative](/docs/containers?topic=containers-serverless-apps-knative). La maintenance est simplifiée car IBM fournit l'installation et les mises à jour pour les modules complémentaires gérés.</li>
        <li>Fournir une intégration de cluster avec des technologies de partenariat tierces, par exemple {{site.data.keyword.la_short}}, {{site.data.keyword.mon_short}} et Portworx.</li>
        <li>Fournir une automatisation afin d'activer une liaison de service à d'autres services {{site.data.keyword.Bluemix_notm}} .</li>
        <li>Créer des clusters avec des valeurs confidentielles d'extraction d'image de sorte que vos déploiements dans l'espace de nom Kubernetes `default` puissent extraire des images d'{{site.data.keyword.registrylong_notm}}.</li>
        <li>Fournir des classes de stockage et des plug-ins pour prendre en charge les volumes persistants qui seront utilisés avec vos applications.</li>
        <li>Créer des clusters avec des adresses IP de sous-réseau réservées pour exposer des applications de façon externe.</li>
        <li>Prendre en charge des équilibreurs de charge publics et privés Kubernetes natifs et des routes Ingress pour exposer des services de façon externe.</li>
        </ul>
        <br><br>
        **Vos responsabilités** :
        <ul>
        <li>Utiliser les outils et les fonctions fournis pour [configurer et déployer](/docs/containers?topic=containers-app#app), [configurer des droits](/docs/containers?topic=containers-users#users), [intégrer à d'autres services](/docs/containers?topic=containers-supported_integrations#supported_integrations),  [servir de façon externe](/docs/containers?topic=containers-cs_network_planning#cs_network_planning), [surveiller l'état de santé](/docs/containers?topic=containers-health#health),[sauvegarder et restaurer des données](/docs/containers?topic=containers-storage_planning#storage_planning) et gérer vos charges de travail [hautement disponibles](/docs/containers?topic=containers-ha#ha) et résilientes.</li>
        </ul>
        </td>
        </tr>
  </tbody>
  </table>

<br />


## Usage abusif d'{{site.data.keyword.containerlong_notm}}
{: #terms}

Les clients ne doivent pas utiliser à mauvais escient {{site.data.keyword.containerlong_notm}}.
{:shortdesc}

L'utilisation à mauvais escient inclut :

*   Toute activité illégale
*   Distribution ou exécution de logiciel malveillant
*   Endommager {{site.data.keyword.containerlong_notm}} ou porter atteinte à l'utilisation d'{{site.data.keyword.containerlong_notm}} par autrui
*   Endommager ou porter atteinte à l'utilisation d'un autre service ou système par autrui
*   Accès non autorisé à un service ou système quelconque
*   Modification non autorisée d'un service ou système quelconque
*   Violation des droits d'autrui

Voir [Dispositions des services cloud](/docs/overview/terms-of-use?topic=overview-terms#terms) pour prendre connaissance des conditions générales d'utilisation.
