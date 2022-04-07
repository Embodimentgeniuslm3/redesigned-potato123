---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: kubernetes, iks

subcollection: containers

---

{:new_window: target="blank"}
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



# Affectation d'accès au cluster
{: #users}

En tant qu'administrateur de cluster, vous pouvez définir des règles d'accès à votre cluster {{site.data.keyword.containerlong}} pour créer différents niveaux d'accès pour différents utilisateurs. Par exemple, vous pouvez autoriser certains utilisateurs à travailler avec des ressources d'infrastructure de cluster et d'autres à déployer uniquement des conteneurs.
{: shortdesc}

Depuis le 30 janvier 2019, {{site.data.keyword.containerlong_notm}} dispose d'un nouveau mode d'autorisation des utilisateurs avec {{site.data.keyword.Bluemix_notm}} IAM : les [rôles d'accès au service](/docs/containers?topic=containers-access_reference#service). Ces rôles de service sont utilisés pour accorder l'accès aux ressources au sein du cluster, par exemple aux espaces de nom Kubernetes. Pour plus d'informations, consultez le blogue, [Introducing service roles and namespaces in IAM for more granular control of cluster access ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/blogs/bluemix/2019/02/introducing-service-roles-and-namespaces-in-iam-for-more-granular-control-of-cluster-access/).
{: note}

## Description des rôles et des règles d'accès
{: #access_policies}

Les règles d'accès déterminent le niveau d'accès des utilisateurs figurant dans votre compte {{site.data.keyword.Bluemix_notm}} aux ressources sur la plateforme {{site.data.keyword.Bluemix_notm}}. Une règle affecte un ou plusieurs rôles à un utilisateur pour définir l'étendue de l'accès à un seul service ou à un ensemble de services et de ressources regroupés au sein d'un groupe de ressources. Chaque service d'{{site.data.keyword.Bluemix_notm}} peut nécessiter son propre ensemble de règles d'accès.
{: shortdesc}

Lorsque vous élaborez votre plan pour la gestion de l'accès des utilisateurs, tenez compte des étapes générales suivantes :
1.  [Sélectionnez la règle d'accès et le rôle appropriés pour vos utilisateurs](#access_roles)
2.  [Affectez des rôles d'accès à des utilisateurs individuels ou à des groupes d'utilisateurs dans {{site.data.keyword.Bluemix_notm}} IAM](#iam_individuals_groups)
3.  [Définissez l'étendue des accès utilisateur à des instances de cluster, des espaces de nom Kubernetes ou des groupes de ressources](#resource_groups)

Une fois que vous avez saisi comment gérer les rôles, les utilisateurs et les ressources dans votre compte, consultez la rubrique [Configuration de l'accès à votre cluster](#access-checklist) pour obtenir une liste de contrôle pour savoir comment configurer les accès.

### Sélection de la règle d'accès et du rôle appropriés pour vos utilisateurs
{: #access_roles}

Vous devez définir des règles d'accès pour tous les utilisateurs qui utilisent {{site.data.keyword.containerlong_notm}}. La portée d'une règle d'accès s'appuie sur un ou plusieurs rôles définis pour un utilisateur, qui déterminent les actions que cet utilisateur peut effectuer. Certaines règles sont prédéfinies, mais d'autres peuvent être personnalisées. La même règle s'applique que l'utilisateur effectue la demande à partir de la console {{site.data.keyword.containerlong_notm}} ou de l'interface de ligne de commande (CLI), et ce, même lorsque les actions sont exécutées dans l'infrastructure IBM Cloud (SoftLayer).
{: shortdesc}

L'image suivante présente les différents types de droits et de rôles, les types d'action attribués à chaque rôle et les relations entre les rôles.

<img src="images/user_access.png" alt="Dans {{site.data.keyword.containerlong_notm}}, vous pouvez affecter des rôles de plateforme IAM, des rôles de service IAM, des rôles Cloud Foundry et des rôles d'infrastructure roles." style="border-style: none"/>

Pour voir les droits spécifiques à {{site.data.keyword.containerlong_notm}} pour chaque rôle, consultez la rubrique de référence [Droits d'accès des utilisateurs](/docs/containers?topic=containers-access_reference).
{: tip}

<dl>
<dt><a href="#platform">Rôles de service et de plateforme {{site.data.keyword.Bluemix_notm}} IAM</a></dt>
<dd>{{site.data.keyword.containerlong_notm}} utilise des rôles de service et de plateforme {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM) pour octroyer l'accès des utilisateurs au cluster.
<ul><li>**Plateforme** : les rôles de plateforme déterminent les actions pouvant être effectuées par les utilisateurs sur l'infrastructure du cluster à l'aide de la console, de l'API {{site.data.keyword.containerlong_notm}} et de l'interface de ligne de commande (commandes `ibmcloud ks`). Les rôles de plateforme n'octroient pas d'accès à l'API Kubernetes. Vous pouvez définir des règles pour ces rôles par groupe de ressources, région ou instance de cluster. Bien que les rôles de plateforme vous autorisent à effectuer des actions d'infrastructure sur le cluster, ils ne vous donnent pas accès aux ressources d'infrastructure IBM Cloud (SoftLayer). L'accès à ces ressources est déterminé par la [clé d'API définie pour la région](#api_key). Exemples d'actions autorisées par les rôles de plateforme : création ou retrait de clusters, liaison de services à un cluster, gestion des réseaux et des ressources de stockage ou ajout de noeuds worker supplémentaires.<br><br>Si vous n'affectez que des rôles de plateforme aux utilisateurs, ils ne pourront pas interagir avec les ressources Kubernetes au sein du cluster. Ils pourront toutefois exécuter la [commande](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_config) `ibmcloud ks cluster-config`. Vous pouvez ensuite autoriser les utilisateurs à effectuer une sélection d'actions Kubernetes en utilisant des [règles RBAC personnalisées](/docs/containers?topic=containers-users#role-binding). Vous serez habilité à le faire si votre organisation utilise actuellement des règles RBAC personnalisées pour contrôler l'accès à Kubernetes et envisage de continuer à utiliser ces règles au lieu de recourir à la place des rôles de service.</li>
<li>**Service** : les rôles de service octroient les règles RBAC Kubernetes correspondantes qui sont attribuées à un utilisateur au sein d'un cluster. Ainsi, les rôles de service accordent l'accès à l'API, au tableau de bord et à l'interface de ligne de commande (`kubectl`) de Kubernetes. Vous pouvez définir la portée de la règle pour les rôles de service par groupe de ressources, région ou instance de cluster. Par ailleurs, vous pouvez également définir l'étendue des rôles de service dans les espaces Kubernetes figurant dans tous les clusters, dans des clusters individuels ou dans les clusters de toute la région. Lorsque vous définissez l'étendue d'un rôle de service à un espace de nom, vous ne pouvez pas appliquer la règle à un groupe de ressources ou affecter un rôle de plateforme en même temps. Exemples d'actions autorisées par les rôles de service : création de déploiements d'application, ajout d'espaces de nom ou configuration de mappes de configuration (configmap).<br><br>Si vous n'affectez que des rôles de service aux utilisateurs, ils ne pourront pas afficher ou interagir avec des ressources {{site.data.keyword.containerlong_notm}}. Pour que les utilisateurs accèdent au cluster et utilisent des ressources Kubernetes du cluster, vous devez leur donner le nom et l'ID du cluster pour leur permettre d'exécuter la [commande](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_config) `ibmcloud ks cluster-config`, puis [lancer le tableau de bord Kubernetes à partir de l'interface de ligne de commande](/docs/containers?topic=containers-app#db_cli). Si vous souhaitez que ces utilisateurs soient toujours en mesure d'accéder aux clusters {{site.data.keyword.containerlong_notm}} via la console et d'afficher les clusters et d'autres ressources d'infrastructure à partir de l'interface de ligne de commande, donnez-leur le rôle de plateforme **Afficheur**.</li></ul></dd>
<dt><a href="#role-binding">RBAC</a></dt>
<dd>Dans Kubernetes, le contrôle d'accès à base de rôles (RBAC) constitue un moyen de sécuriser les ressources au sein de votre cluster. Les rôles RBAC déterminent les actions de Kubernetes que les utilisateurs peuvent effectuer sur ces ressources. Tous les utilisateurs auxquels est affecté un rôle de service bénéficient automatiquement d'un rôle de cluster RBAC correspondant. Ce rôle de cluster RBAC est appliqué dans un espace de nom spécifique ou dans tous les espaces de nom, selon que vous ayez défini la portée de la règle à un espace de nom. </br></br>
Exemples d'actions autorisées par les rôles RBAC : création d'objets, tels que des pods ou lecture des journaux de pod.</dd>
<dt><a href="#api_key">Infrastructure</a></dt>
<dd>Les rôles d'infrastructure autorisent l'accès à vos ressources d'infrastructure IBM Cloud (SoftLayer). Configurez un utilisateur avec le rôle d'infrastructure **Superutilisateur** et stockez les données d'identification de cet utilisateur dans une clé d'API. Configurez ensuite la clé d'API dans chaque région où vous souhaitez créer des clusters. Une fois la clé d'API configurée, d'autres utilisateurs auxquels vous avez octroyé l'accès à {{site.data.keyword.containerlong_notm}} n'ont pas besoin de rôles d'infrastructure car la clé d'API est partagée par tous les utilisateurs au sein de la région. A la place, les rôles de plateforme {{site.data.keyword.Bluemix_notm}} IAM déterminent les actions d'infrastructure que les utilisateurs sont autorisés à effectuer. Si vous ne définissez pas la clé d'API avec le rôle d'infrastructure <strong>Superutilisateur</strong> complet ou si vous devez octroyer l'accès des utilisateurs à des unités spécifiques, vous pouvez [personnaliser les droits d'infrastructure](#infra_access). </br></br>
Exemple d'actions autorisées par les rôles d'infrastructure : affichage des détails de machines de noeud worker ou édition de ressources de mise en réseau et de ressources de stockage.</dd>
<dt>Cloud Foundry</dt>
<dd>Certains services ne peuvent pas être gérés avec {{site.data.keyword.Bluemix_notm}} IAM. Si vous utilisez l'un de ces services, vous pouvez continuer à utiliser des rôles utilisateur Cloud Foundry pour contrôler l'accès à ces services. Les rôles Cloud Foundry octroient l'accès à des organisations et à des espaces figurant dans le compte. Pour voir la liste des services Cloud Foundry dans {{site.data.keyword.Bluemix_notm}}, exécutez la commande <code>ibmcloud service list</code>. </br></br>
Exemple d'actions autorisées par les rôles Cloud Foundry : création d'une nouvelle instance de service Cloud Foundry ou liaison d'une instance Cloud Foundry à un cluster. Pour en savoir plus, consultez les [rôles d'organisation et d'espace](/docs/iam?topic=iam-cfaccess) disponibles ou la procédure permettant de [gérer l'accès à Cloud Foundry](/docs/iam?topic=iam-mngcf) dans la documentation {{site.data.keyword.Bluemix_notm}} IAM.</dd>
</dl>

### Affectation de rôles d'accès à des utilisateurs individuels ou à des groupes d'utilisateurs dans {{site.data.keyword.Bluemix_notm}} IAM
{: #iam_individuals_groups}

Lorsque vous définissez des règles {{site.data.keyword.Bluemix_notm}} IAM, vous pouvez affecter des rôles à un utilisateur individuel ou à un groupe d'utilisateurs.
{: shortdesc}

<dl>
<dt>Utilisateurs individuels</dt>
<dd>Vous pouvez disposer d'un utilisateur nécessitant plus ou moins de droits que le reste de votre équipe. Vous pouvez personnaliser les droits d'accès sur une base individuelle de sorte que chaque personne dispose des droits dont elle a besoin pour accomplir sa tâche. Vous pouvez affecter plusieurs rôles {{site.data.keyword.Bluemix_notm}} IAM à chaque utilisateur.</dd>
<dt>Plusieurs utilisateurs au sein d'un groupe d'accès</dt>
<dd>Vous pouvez créer un groupe d'utilisateurs et affecter des droits d'accès à ce groupe. Par exemple, vous pouvez regrouper tous les chefs d'équipe et affecter l'accès administrateur à ce groupe. Vous pouvez ensuite regrouper tous les développeurs et affecter uniquement les droits d'accès en écriture à ce groupe. Vous pouvez affecter plusieurs rôles {{site.data.keyword.Bluemix_notm}} IAM à chaque groupe d'accès. Lorsque vous affectez des droits à un groupe, tous les utilisateurs ajoutés ou retirés dans ce groupe sont impactés. Si vous ajoutez un utilisateur dans le groupe, il dispose également de cet accès supplémentaire. S'il est supprimé, son accès est révoqué.</dd>
</dl>

Les rôles {{site.data.keyword.Bluemix_notm}} IAM ne peuvent pas être affectés à un compte de service. Vous pouvez à la place [affecter directement des rôles RBAC aux comptes de service](#rbac).
{: tip}

Vous devez également indiquer si les utilisateurs ont accès à un cluster dans un groupe de ressources, à tous les clusters dans un groupe de ressources ou à tous les clusters dans tous les groupes de ressources de votre compte.

### Définition de l'étendue de l'accès utilisateur à des instances de cluster, des espaces de nom ou des groupes de ressources
{: #resource_groups}

Dans {{site.data.keyword.Bluemix_notm}} IAM, vous pouvez affecter des rôles d'accès utilisateur à des instances de ressources, des espaces de nom Kubernetes ou des groupes de ressources.
{: shortdesc}

Lorsque vous créez votre compte {{site.data.keyword.Bluemix_notm}}, le groupe de ressources par défaut est créé automatiquement. Si vous n'indiquez pas de groupe de ressources lorsque vous créez la ressource, les instances de ressource (clusters) appartiennent au groupe de ressources par défaut. Dans {{site.data.keyword.Bluemix_notm}} IAM, un espace de nom Kubernetes correspond au type de ressource d'une instance de ressource (cluster). Si vous souhaitez ajouter un groupe de ressources dans votre compte, consultez les rubriques [Meilleures pratiques pour la configuration de votre compte](/docs/account?topic=account-account_setup) et [Configuration de vos groupes de ressources](/docs/resources?topic=resources-bp_resourcegroups#setuprgs).

<dl>
<dt>Instance de ressource</dt>
  <dd><p>Chaque service {{site.data.keyword.Bluemix_notm}} dans votre compte est une ressource comportant des instances. L'instance varie selon le service. Par exemple, dans {{site.data.keyword.containerlong_notm}}, l'instance est un cluster, mais dans {{site.data.keyword.cloudcerts_long_notm}}, l'instance est un certificat. Par défaut, les ressources appartiennent également au groupe de ressources par défaut de votre compte. Vous pouvez affecter à des utilisateurs un rôle d'accès à une instance de ressource dans les scénarios suivants.
  <ul><li>Tous les services {{site.data.keyword.Bluemix_notm}} IAM de votre compte, y compris tous les clusters dans {{site.data.keyword.containerlong_notm}} et les images dans {{site.data.keyword.registrylong_notm}}.</li>
  <li>Toutes les instances au sein d'un service, par exemple tous les clusters dans {{site.data.keyword.containerlong_notm}}.</li>
  <li>Toutes les instances d'un service au sein d'une région, par exemple tous les clusters dans la région Sud des Etats-Unis (**US South**) d'{{site.data.keyword.containerlong_notm}}.</li>
  <li>Une instance individuelle, par exemple un cluster.</li></ul></dd>
<dt>Espace de nom Kubernetes</dt>
  <dd><p>Dans le cadre des instances de ressource de cluster dans {{site.data.keyword.Bluemix_notm}} IAM, vous pouvez affecter aux utilisateurs des rôles d'accès au service pour accéder aux espaces Kubernetes dans vos clusters.</p>
  <p>Lorsque vous accordez l'accès à un espace de nom, la règle s'applique à toutes les instances présentes et futures de cet espace de nom dans tous les clusters que vous autorisez. Par exemple, supposons que vous vouliez qu'un groupe d'utilisateurs nommé `dev` puisse déployer des ressources Kubernetes dans un espace de nom nommé `test` dans tous vos clusters situés dans la région Asie-Pacifique nord. Si vous affectez le rôle d'accès au service **Writer** au groupe d'accès `dev` pour l'espace de nom Kubernetes test dans tous les clusters de la région Asie-Pacifique nord dans le groupe de ressources `default`, le groupe `dev` pourra accéder à l'espace de nom `test` de n'importe quel cluster en Asie-Pacifique nord dans le groupe de ressources `default` disposant actuellement ou réellement d'un espace de nom nommé test.</p>
  <p class="important">Si vous définissez la portée d'un rôle de service à un espace de nom, vous ne pouvez pas appliquer la règle à un groupe de ressources ou affecter un rôle de plateforme en même temps.</p></dd>
<dt>Groupe de ressources</dt>
  <dd><p>Vous pouvez organiser les ressources de votre compte en groupes personnalisables pour pouvoir affecter rapidement à des utilisateurs individuels ou à des groupes d'utilisateurs l'accès à plusieurs ressources à la fois. Les groupes de ressources peuvent aider les opérateurs et les administrateurs à filtrer les ressources pour afficher leur utilisation actuelle, identifier et résoudre des incidents et gérer des équipes.</p>
  <p class="important">Un cluster peut être créé dans un seul groupe de ressources que vous ne pouvez plus modifier par la suite. Si vous créez un cluster dans le mauvais groupe de ressources, vous devez supprimer le cluster et le créer à nouveau dans le groupe de ressources approprié. En outre, si vous devez utiliser la [commande](/docs/containers-cli-plugin?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_service_bind) `ibmcloud ks cluster-service-bind` pour [intégrer un service {{site.data.keyword.Bluemix_notm}}](/docs/containers?topic=containers-service-binding#bind-services), ce dernier doit se trouver dans le même groupe de ressources que le cluster. Les services qui n'utilisent pas de groupes de ressources, par exemple, {{site.data.keyword.registrylong_notm}}, ou qui n'ont pas besoin de liaison de service, par exemple, {{site.data.keyword.la_full_notm}}, fonctionnent même si le cluster se trouve dans un autre groupe de ressources.</p>
  <p>Si vous envisagez d'utiliser [{{site.data.keyword.monitoringlong_notm}} pour les métriques](/docs/containers?topic=containers-health#view_metrics), veillez à donner des noms uniques aux clusters dans les groupes de ressources et les régions de votre compte afin d'éviter des conflits de noms pour les métriques. Vous ne pouvez pas renommer un cluster.</p>
  <p>Vous pouvez affecter à des utilisateurs un rôle d'accès à un groupe de ressources dans les scénarios suivants. Notez que contrairement aux instances de ressource, vous ne pouvez pas octroyer l'accès à une instance individuelle au sein d'un groupe de ressources.</p>
  <ul><li>Tous les services {{site.data.keyword.Bluemix_notm}} IAM dans le groupe de ressources, y compris tous les clusters dans {{site.data.keyword.containerlong_notm}} et les images dans {{site.data.keyword.registrylong_notm}}.</li>
  <li>Toutes les instances au sein d'un service dans le groupe de ressources, par exemple tous les clusters dans {{site.data.keyword.containerlong_notm}}.</li>
  <li>Toutes les instances au sein d'une région d'un service dans le groupe de ressources, par exemple tous les clusters dans la région Sud des Etats-Unis (**US South**) d'{{site.data.keyword.containerlong_notm}}.</li></ul></dd>
</dl>

<br />


## Configuration de l'accès à votre cluster
{: #access-checklist}

Une fois que vous avez [compris comment gérer les rôles, les utilisateurs et les ressources dans votre compte](#access_policies), utilisez la liste de contrôle suivante pour configurer l'accès des utilisateurs dans votre cluster.
{: shortdesc}

<p class="tip">N'affectez pas de rôles de plateforme {{site.data.keyword.Bluemix_notm}} en même temps qu'un rôle de service. Vous devez affecter les rôles de plateforme et de service séparément.</p>

1. [Définissez la clé d'API](#api_key) pour toutes les régions et les groupes de ressources dans lesquels vous voulez créer des clusters.
2. Invitez des utilisateurs sur votre compte et [affectez-leur des rôles {{site.data.keyword.Bluemix_notm}} IAM](#platform) pour {{site.data.keyword.containerlong_notm}}. 
3. Pour autoriser des utilisateurs à lier des services au cluster ou à afficher des journaux transmis à partir de configurations de consignation de cluster, [octroyez aux utilisateurs des rôles Cloud Foundry](/docs/iam?topic=iam-mngcf) pour l'organisation et l'espace dans lesquels sont déployés les services ou à l'emplacement de collecte des journaux.
4. Si vous utilisez des espaces de nom Kubernetes pour isoler les ressources dans le cluster, octroyez l'accès aux espaces de nom en [affectant aux utilisateurs des rôles de service {{site.data.keyword.Bluemix_notm}} IAM pour les espaces de nom](#platform).
5. Pour d'autres outils d'automatisation comme par exemple votre pipeline CI/CD, configurez des comptes de service et [affectez-leur des droits RBAC Kubernetes](#rbac).
6. Pour d'autres configurations avancées afin de contrôler l'accès aux ressources de votre cluster au niveau du pod, voir [Configuration de politiques de sécurité de pod](/docs/containers?topic=containers-psp).

</br>

Pour plus d'informations sur la configuration de votre compte et de vos ressources, suivez ce tutoriel sur les [meilleures pratiques en matière d'organisation des utilisateurs, des équipes et des applications](/docs/tutorials?topic=solution-tutorials-users-teams-applications).
{: tip}

<br />


## Configuration de la clé d'API pour activer l'accès au portefeuille de l'infrastructure
{: #api_key}

Pour réussir à mettre à disposition des clusters et à travailler avec, vous devez vérifier que votre compte {{site.data.keyword.Bluemix_notm}} est configuré correctement pour accéder au portefeuille de l'infrastructure IBM Cloud (SoftLayer) dans chaque groupe de ressource et région dans lesquels se trouvent vos clusters.
{: shortdesc}

**Dans la plupart des cas** : votre compte de Paiement à la carte {{site.data.keyword.Bluemix_notm}} a déjà accès au portefeuille de l'infrastructure IBM Cloud (SoftLayer). Pour configurer l'accès d'{{site.data.keyword.containerlong_notm}} au portefeuille, le **propriétaire du compte** doit définir la clé d'API pour la région et le groupe de ressources.

1. Connectez-vous au terminal en tant que propriétaire du compte.
    ```
    ibmcloud login [--sso]
    ```
    {: pre}

2. Ciblez le groupe de ressources dans lequel vous voulez définir la clé d'API. Si vous ne le faites pas, la clé d'API est définie pour le groupe de ressources par défaut. Pour afficher la liste des groupes de ressources disponibles, exécutez la commande `ibmcloud resource groups`.
    ```
    ibmcloud target -g <resource_group_name>
    ```
    {:pre}

3. Définissez la clé d'API pour la région et le groupe de ressources.
    ```
    ibmcloud ks api-key-reset --region <region>
    ```
    {: pre}    

4. Vérifiez que la clé d'API est définie.
    ```
    ibmcloud ks api-key-info --cluster <cluster_name_or_ID>
    ```
    {: pre}

5. Répétez ces opérations pour chaque région et groupe de ressources dans lesquels vous souhaitez créer des clusters.

**Autres options possibles et informations supplémentaires** :  pour connaître les différentes méthodes d'accès au portefeuille de l'infrastructure IBM Cloud (SoftLayer), consultez les sections suivantes.
* Si vous n'êtes pas sûr que votre compte dispose déjà de l'accès au portefeuille de l'infrastructure IBM Cloud (SoftLayer), voir [Comprendre l'accès au portefeuille de l'infrastructure IBM Cloud (SoftLayer)](#understand_infra).
* Si le propriétaire du compte ne définit pas la clé d'API, [vérifiez que l'utilisateur qui définit la clé d'API dispose des droits appropriés](#owner_permissions).
* Pour plus d'informations sur l'utilisation de votre compte par défaut pour définir la clé d'API, voir [Accès au portefeuille de l'infrastructure avec votre compte de paiement à la carte {{site.data.keyword.Bluemix_notm}} par défaut](#default_account).
* Si vous ne disposez pas d'un compte de paiement à la carte par défaut ou si vous devez utiliser un autre compte d'infrastructure IBM Cloud (SoftLayer), voir [Accès à un autre compte d'infrastructure IBM Cloud (SoftLayer)](#credentials).

### Comprendre l'accès au portefeuille de l'infrastructure IBM Cloud (SoftLayer)
{: #understand_infra}

Déterminez si votre compte a accès au portefeuille de l'infrastructure IBM Cloud (SoftLayer) et découvrez comment {{site.data.keyword.containerlong_notm}} utilise la clé d'API pour accéder au portefeuille.
{: shortdesc}

**Mon compte a-t-il déjà accès au portefeuille de l'infrastructure IBM Cloud (SoftLayer) ?**</br>

Pour accéder au portefeuille de l'infrastructure IBM Cloud (SoftLayer), vous utilisez un compte {{site.data.keyword.Bluemix_notm}} Paiement à la carte. Si vous disposez d'un autre type de compte, consultez vos options dans le tableau suivant.

<table summary="Le tableau présente les options de création de cluster standard par type de compte. La lecture des lignes s'effectue de gauche à droite, avec la description du compte dans la première colonne, et les options de création d'un cluster standard dans la deuxième colonne.">
<caption>Options de création de cluster standard par type de compte</caption>
  <thead>
  <th>Description du compte</th>
  <th>Options de création d'un cluster standard</th>
  </thead>
  <tbody>
    <tr>
      <td>Les **comptes Lite** ne peuvent pas mettre à disposition des clusters.</td>
      <td>[Effectuez la mise à niveau de votre compte Lite pour passer à un compte {{site.data.keyword.Bluemix_notm}} Paiement à la carte](/docs/account?topic=account-accounts#paygo).</td>
    </tr>
    <tr>
      <td>Les **comptes Paiement à la carte** offrent l'accès au portefeuille de l'infrastructure.</td>
      <td>Vous pouvez créer des clusters standard. Utilisez une clé d'API pour configurer les droits sur l'infrastructure pour vos clusters.</td>
    </tr>
    <tr>
      <td>Les **comptes Abonnement** ne sont pas configurés avec l'accès au portefeuille de l'infrastructure IBM Cloud (SoftLayer).</td>
      <td><p><strong>Option 1 :</strong> [créez un compte Paiement à la carte](/docs/account?topic=account-accounts#paygo) configuré avec accès au portefeuille de l'infrastructure IBM Cloud (SoftLayer). Si vous sélectionnez cette option, vous disposerez de deux comptes {{site.data.keyword.Bluemix_notm}} avec facturation distincte.</p><p>Si vous souhaitez continuer à utiliser votre ancien compte Abonnement, vous pouvez utiliser le nouveau compte Paiement à la carte pour générer une clé d'API dans l'infrastructure IBM Cloud (SoftLayer). Vous devez ensuite définir manuellement la clé d'API de l'infrastructure IBM Cloud (SoftLayer) pour votre compte Abonnement. N'oubliez pas que les ressources d'infrastructure IBM Cloud (SoftLayer) sont facturées via votre nouveau compte Paiement à la carte.</p><p><strong>Option 2 :</strong> si vous disposez déjà d'un compte d'infrastructure IBM Cloud (SoftLayer) existant que vous désirez utiliser, vous pouvez définir manuellement les données d'identification de votre compte d'infrastructure IBM Cloud (SoftLayer) pour votre compte {{site.data.keyword.Bluemix_notm}}.</p><p class="note">Lorsque vous effectuez une liaison manuelle vers un compte d'infrastructure IBM Cloud (SoftLayer), les données d'identification sont utilisées pour toutes les actions spécifiques de l'infrastructure IBM Cloud (SoftLayer) effectuées dans votre compte {{site.data.keyword.Bluemix_notm}}. Vous devez vérifier que la clé d'API que vous avez définie dispose de [droits d'infrastructure suffisants](/docs/containers?topic=containers-users#infra_access) pour que vos utilisateurs puissent créer et gérer des clusters.</p></td>
    </tr>
    <tr>
      <td>**Comptes d'infrastructure IBM Cloud (SoftLayer)**, aucun compte {{site.data.keyword.Bluemix_notm}}</td>
      <td><p>[Créez un compte {{site.data.keyword.Bluemix_notm}} Paiement à la carte](/docs/account?topic=account-accounts#paygo). Vous disposez de deux comptes d'infrastructure IBM Cloud (SoftLayer) différents avec facturation distincte.</p><p>Par défaut, votre nouveau compte {{site.data.keyword.Bluemix_notm}} utilise le nouveau compte d'infrastructure. Pour continuer à utiliser l'ancien compte d'infrastructure, définissez les données d'identification manuellement.</p></td>
    </tr>
  </tbody>
  </table>

**Une fois mon portefeuille de l'infrastructure configuré, comment {{site.data.keyword.containerlong_notm}} accède-t-il au portefeuille ?**</br>

{{site.data.keyword.containerlong_notm}} accède au portefeuille de l'infrastructure IBM Cloud (SoftLayer) à l'aide d'une clé d'API. Cette clé d'API contient les données d'identification d'un utilisateur ayant accès au compte d'infrastructure IBM Cloud (SoftLayer). Les clés d'API sont définies par région au sein d'un groupe de ressources. Elles sont partagées par les utilisateurs situés dans cette région.
 
Pour activer l'accès de tous les utilisateurs au portefeuille de l'infrastructure IBM Cloud (SoftLayer), l'utilisateur dont les données d'identification sont stockées dans la clé d'API doit disposer [du rôle d'infrastructure **Superutilisateur** et du rôle de plateforme **Administrateur** pour {{site.data.keyword.containerlong_notm}} et pour {{site.data.keyword.registryshort_notm}}](#owner_permissions) dans votre compte {{site.data.keyword.Bluemix_notm}}. Laissez ensuite cet utilisateur effectuer la première opération d'administration dans une région et un groupe de ressources. Les données d'identification d'infrastructure de l'utilisateur sont stockées dans une clé d'API correspondant à cette région et à ce groupe de ressources.

D'autres utilisateurs dans le compte partagent la clé d'API pour accéder à l'infrastructure. Lorsque les utilisateurs se connectent au compte {{site.data.keyword.Bluemix_notm}}, un jeton {{site.data.keyword.Bluemix_notm}} IAM basé sur la clé d'API est généré pour la session de l'interface de ligne de commande (CLI) et permet aux commandes liées à l'infrastructure de s'exécuter dans un cluster.

Pour voir le jeton {{site.data.keyword.Bluemix_notm}} IAM correspondant à une session CLI, vous pouvez exécuter la commande `ibmcloud iam oauth-tokens`. Les jetons {{site.data.keyword.Bluemix_notm}} IAM peuvent également être utilisés pour [effectuer des appels directement dans l'API {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-cs_cli_install#cs_api).
{: tip}

**Si les utilisateurs peuvent accéder au portefeuille au moyen d'un jeton {{site.data.keyword.Bluemix_notm}} IAM, comment puis-je limiter les commandes qu'ils peuvent utiliser ?**

Après avoir configuré l'accès des utilisateurs au portefeuille dans votre compte, vous pouvez contrôler les actions qu'ils peuvent effectuer en leur affectant le [rôle de plateforme](#platform) approprié. L'affectation de rôles {{site.data.keyword.Bluemix_notm}} IAM aux utilisateurs permet de limiter leur champ d'action à un certain nombre de commandes qu'ils pourront exécuter sur un cluster. Par exemple, si le propriétaire de la clé d'API dispose du rôle d'infrastructure **Superutilisateur**, toutes les commandes liées à l'infrastructure peuvent être exécutées dans un cluster. Cependant, selon le rôle {{site.data.keyword.Bluemix_notm}} IAM qui lui est affecté, l'utilisateur ne pourra exécuter que quelques-unes de ces commandes liées à l'infrastructure.

Par exemple, si vous envisagez de créer un cluster dans une nouvelle région, assurez-vous que le premier cluster est créé par un utilisateur doté du rôle d'infrastructure **Superutilisateur**, par exemple le propriétaire du compte. Vous pouvez inviter ensuite des utilisateurs individuels ou des utilisateurs figurant dans des groupes d'accès {{site.data.keyword.Bluemix_notm}} IAM dans cette région en définissant pour eux des règles de gestion de la plateforme dans cette région. Un utilisateur avec un rôle de plateforme **Afficheur** n'est pas autorisé à ajouter un noeud worker. Par conséquent, l'action `worker-add` échoue, même si la clé d'API dispose des droits d'infrastructure corrects. Si vous remplacez le rôle de plateforme de l'utilisateur par **Opérateur**, l'utilisateur est autorisé à ajouter un noeud worker. L'action `worker-add` réussit car l'utilisateur est autorisé et la clé d'API est définie correctement. Vous n'avez pas besoin d'éditer les droits de l'infrastructure IBM Cloud (SoftLayer).

Pour effectuer l'audit des actions pouvant être exécutées par les utilisateurs dans votre compte, vous pouvez utiliser [{{site.data.keyword.cloudaccesstrailshort}}](/docs/containers?topic=containers-at_events) pour afficher tous les événements liés au cluster.
{: tip}

**Et si je ne compte pas affecter le rôle d'infrastructure Superutilisateur au propriétaire de la clé d'API ou des données d'identification ?**</br>

Pour des raisons de conformité, de sécurité ou de facturation, vous ne souhaiterez pas forcément octroyer le rôle d'infrastructure **Superutilisateur** à l'utilisateur ayant défini la clé d'API ou dont les données d'identification sont définies avec la commande `ibmcloud ks credential-set`. En revanche, si cet utilisateur ne dispose pas du rôle **Superutilisateur**, les actions liées à l'infrastructure, par exemple la création d'un cluster ou le rechargement d'un noeud worker, risquent d'échouer. Au lieu d'utiliser des rôles de plateforme {{site.data.keyword.Bluemix_notm}} IAM pour contrôler l'accès des utilisateurs à l'infrastructure, vous devez [définir des droits d'infrastructure IBM Cloud (SoftLayer) spécifiques](#infra_access) pour les utilisateurs.

**Que se passe-t-il l'utilisateur qui a défini la clé d'API pour une région et un groupe de ressources quitte la société ?**

Si l'utilisateur quitte votre organisation, le propriétaire du compte {{site.data.keyword.Bluemix_notm}} peut retirer les droits de cet utilisateur. Cependant, avant de retirer les droits d'accès spécifiques à un utilisateur ou de retirer un utilisateur de votre compte, vous devez réinitialiser la clé d'API avec les données d'identification d'un autre utilisateur. Autrement, les autres utilisateurs du compte pourraient perdre leur accès au portail de l'infrastructure IBM Cloud (SoftLayer) et les commandes liées à l'infrastructure pourraient échouer. Pour plus d'informations, voir [Retrait de droits utilisateur](#removing).

**Comment puis-je verrouiller mon cluster si ma clé d'API se trouve compromise ?**

Si une clé d'API définie pour une région et un groupe de ressources dans votre cluster est compromise, [supprimez-la](/docs/iam?topic=iam-userapikey#delete_user_key) pour qu'il n'y ait plus aucun appel effectué en utilisant cette clé d'API pour l'authentification. Pour plus d'informations sur la sécurisation des accès au serveur d'API Kubernetes, voir la rubrique [Sécurité du serveur d'API Kubernetes et d'etcd](/docs/containers?topic=containers-security#apiserver).

**Comment configurer la clé d'API pour mon cluster ?**</br>

Selon le type de compte que vous utilisez pour accéder au portefeuille de l'infrastructure IBM Cloud (SoftLayer) :
* [Un compte {{site.data.keyword.Bluemix_notm}} Paiement à la carte par défaut](#default_account)
* [Un autre compte d'infrastructure IBM Cloud (SoftLayer) qui ne soit pas lié à votre compte {{site.data.keyword.Bluemix_notm}} Paiement à la carte par défaut](#credentials)

### Vérification des droits du propriétaire de la clé d'API ou des données d'identification de l'infrastructure
{: #owner_permissions}

Pour garantir que toutes les actions liées à l'infrastructure peuvent être effectuées dans le cluster, l'utilisateur dont vous voulez définir les données d'identification pour la clé d'API doit disposer des droits appropriés.
{: shortdesc}

1. Connectez-vous à la [console {{site.data.keyword.Bluemix_notm}}![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/).

2. Pour garantir que toutes les actions liées au compte peuvent être effectuées sans problème, vérifiez que l'utilisateur dispose des rôles de plateforme {{site.data.keyword.Bluemix_notm}} IAM adéquats.
    1. Dans la barre de menu, sélectionnez **Gérer > Accès (IAM)**, puis cliquez sur la page **Utilisateurs**.
    2. Cliquez sur le nom de l'utilisateur pour lequel vous voulez définir la clé d'API ou dont vous souhaitez définir les données d'identification pour la clé d'API, puis cliquez sur l'onglet **Règles d'accès**.
    3. Si l'utilisateur ne dispose pas du rôle de plateforme **Administrateur** pour tous les clusters {{site.data.keyword.containerlong_notm}} dans toutes les régions, [affectez ce rôle de plateforme à l'utilisateur](#platform).
    4. Si l'utilisateur ne dispose pas au minimum du rôle de plateforme **Afficheur** pour le groupe de ressources dans lequel vous voulez définir la clé d'API, [affectez ce rôle de groupe de ressources à l'utilisateur](#platform).
    5. Pour créer des clusters, l'utilisateur doit également disposer du rôle de plateforme **Administrateur** pour {{site.data.keyword.registrylong_notm}} au niveau du compte. Ne limitez pas les règles d'{{site.data.keyword.registryshort_notm}} au niveau du groupe de ressources.

3. Pour garantir que toutes les actions liées à l'infrastructure peuvent être effectuées sans problème, vérifiez que l'utilisateur dispose des règles d'accès à l'infrastructure adéquates.
    1.  Dans la barre de menu, sélectionnez **Gérer > Accès (IAM)**.
    2.  Sélectionnez l'onglet **Utilisateurs** et cliquez sur l'utilisateur.
    3. Dans le panneau **Clés d'API**, vérifiez que l'utilisateur dispose d'une **Clé d'API d'infrastructure classique** ou cliquez sur **Créer une clé d'API IBM Cloud**. Pour plus d'informations, voir [Gestion des clés d'API d'infrastructure classique](/docs/iam?topic=iam-classic_keys#classic_keys).
    4. Cliquez sur l'onglet **Infrastructure classique**, puis sur l'onglet **Droits**.
    5. Si l'utilisateur n'a pas toutes les catégories cochées, vous pouvez utiliser la liste déroulante **Ensembles de droits** pour lui affecter le rôle **Superutilisateur**. Vous pouvez aussi développer chaque catégorie et attribuer à l'utilisateur les [droits d'infrastructure](/docs/containers?topic=containers-access_reference#infra) requis.

### Accès au portefeuille de l'infrastructure avec votre compte {{site.data.keyword.Bluemix_notm}} Paiement à la carte par défaut
{: #default_account}

Si vous disposez d'un compte {{site.data.keyword.Bluemix_notm}} Paiement à la carte, vous avez accès à un portefeuille de l'infrastructure IBM Cloud (SoftLayer) lié par défaut. La clé d'API est utilisée pour commander des ressources d'infrastructure à partir de ce portefeuille de l'infrastructure IBM Cloud (SoftLayer), par exemple de nouveaux noeuds worker ou réseaux locaux virtuels (VLAN).
{: shortdec}

Vous pouvez trouver le propriétaire actuel de la clé d'API en exécutant la commande [`ibmcloud ks api-key-info --cluster <cluster>`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_api_key_info). Si vous vous rendez compte que vous devez mettre à jour la clé d'API qui est stockée pour une région, vous pouvez le faire en exécutant la commande [`ibmcloud ks api-key-reset --region <region>`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_api_key_reset). Cette commande nécessite la règle d'accès admin {{site.data.keyword.containerlong_notm}} et stocke la clé d'API de l'utilisateur qui exécute cette commande dans le compte.

Assurez-vous de vouloir réinitialiser la clé et d'en mesurer l'impact sur votre application. La clé est utilisée à plusieurs endroits et peut entraîner des modifications avec rupture si elle est modifiée inutilement.
{: note}

**Avant de commencer** :
- Si le propriétaire du compte ne définit pas la clé d'API, [vérifiez que l'utilisateur qui définit la clé d'API dispose des droits appropriés](#owner_permissions).
- [Connectez-vous à votre compte. Le cas échéant, ciblez le groupe de ressources approprié. Définissez le contexte pour votre cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

Pour définir la clé d'API permettant d'accéder au portefeuille de l'infrastructure IBM Cloud (SoftLayer) :

1.  Définissez la clé d'API pour la région et le groupe de ressources où se trouve le cluster.
    1.  Connectez-vous au terminal avec l'utilisateur dont vous voulez utiliser les droits d'infrastructure.
    2.  Ciblez le groupe de ressources dans lequel vous voulez définir la clé d'API. Si vous ne le faites pas, la clé d'API est définie pour le groupe de ressources par défaut.
        ```
        ibmcloud target -g <resource_group_name>
        ```
        {:pre}
    4.  Définissez la clé d'API de l'utilisateur pour la région.
        ```
    ibmcloud ks api-key-reset --region <region>
    ```
        {: pre}    
    5.  Vérifiez que la clé d'API est définie.
        ```
        ibmcloud ks api-key-info --cluster <cluster_name_or_ID>
        ```
        {: pre}

2. [Créez un cluster](/docs/containers?topic=containers-clusters). Pour créer le cluster, les données d'identification de la clé d'API que vous avez définies pour la région et le groupe de ressources sont utilisées.

### Accès à un autre compte d'infrastructure IBM Cloud (SoftLayer)
{: #credentials}

Au lieu d'utiliser le compte d'infrastructure IBM Cloud (SoftLayer) lié par défaut pour commander l'infrastructure des clusters d'une région, vous envisagerez peut-être d'utiliser un autre compte d'infrastructure IBM Cloud (SoftLayer) dont vous disposez déjà. Vous pouvez lier ce compte d'infrastructure à votre compte {{site.data.keyword.Bluemix_notm}} en utilisant la commande [`ibmcloud ks credential-set`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_set). Les données d'identification de l'infrastructure IBM Cloud (SoftLayer) sont utilisées à la place de celles du compte Paiement à la carte par défaut stockées pour la région.
{: shortdesc}

Les données d'identification de l'infrastructure IBM Cloud (SoftLayer) définies par la commande `ibmcloud ks credential-set` sont conservées une fois votre session terminée. Si vous supprimez les données d'identification de l'infrastructure IBM Cloud (SoftLayer) définies manuellement avec la commande [`ibmcloud ks credential-unset --region <region>`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_unset), les données d'identification du compte Paiement à la carte par défaut sont utilisées. Toutefois, ce changement de données d'identification peut entraîner des [clusters orphelins](/docs/containers?topic=containers-cs_troubleshoot_clusters#orphaned).
{: important}

**Avant de commencer** :
- Si vous n'utilisez pas les données d'identification du propriétaire du compte, [vérifiez que l'utilisateur dont vous voulez définir les données d'identification pour la clé d'API dispose des droits appropriés](#owner_permissions).
- [Connectez-vous à votre compte. Le cas échéant, ciblez le groupe de ressources approprié. Définissez le contexte pour votre cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

Pour définir les données d'identification du compte d'infrastructure permettant d'accéder au portefeuille d'infrastructure IBM Cloud (SoftLayer) :

1. Obtenez le compte d'infrastructure que vous désirez utiliser pour accéder au portefeuille de l'infrastructure IBM Cloud (SoftLayer). Vous avez différentes options possibles en fonction du [compte dont vous disposez actuellement](#understand_infra).

2.  Définissez les données d'identifications d'API avec l'utilisateur pour le compte approprié.

    1.  Obtenez les données d'identification d'API d'infrastructure de l'utilisateur. Notez que les données d'identification sont différentes de l'IBMid.

        1.  Dans la [console {{site.data.keyword.Bluemix_notm}} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/), sélectionnez le tableau **Gérer** > **Accès (IAM)** > **Utilisateurs** et cliquez sur le nom de l'utilisateur.

        2.  Dans la section **Clés d'API**, recherchez ou créez une clé d'API d'infrastructure classique.   

    2.  Définissez les données d'identification d'API d'infrastructure à utiliser.
        ```
        ibmcloud ks credential-set --infrastructure-username <infrastructure_API_username> --infrastructure-api-key <infrastructure_API_authentication_key> --region <region>
        ```
        {: pre}

    3. Vérifiez que les données d'identification sont définies.
        ```
        ibmcloud ks credential-get --region <region>
        ```
        Exemple de sortie :
        ```
        Infrastructure credentials for user name user@email.com set for resource group default.
        ```
        {: screen}

3. [Créez un cluster](/docs/containers?topic=containers-clusters). Pour créer le cluster, les données d'identification de l'infrastructure que vous avez définies pour la région et le groupe de ressources sont utilisées.

4. Vérifiez que votre cluster utilise les données d'identification du compte d'infrastructure que vous avez définies.
  1. Ouvrez la [console {{site.data.keyword.containerlong_notm}} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/kubernetes/clusters) et sélectionnez votre cluster. 
  2. Dans l'onglet Vue d'ensemble, recherchez une zone correspondant à un **utilisateur d'infrastructure**. 
  3. Si vous voyez cette zone, vous n'utilisez pas les données d'identification par défaut de l'infrastructure qui sont fournies avec un compte Paiement à la carte dans cette région. La région est configurée pour utiliser d'autres données d'identification du compte d'infrastructure que vous avez définies.

<br />


## Octroi d'accès utilisateur à votre cluster via {{site.data.keyword.Bluemix_notm}} IAM
{: #platform}

Définissez les règles de gestion de plateforme et d'accès au service {{site.data.keyword.Bluemix_notm}} IAM dans la [console {{site.data.keyword.Bluemix_notm}}](#add_users) ou l'[interface de ligne de commande (CLI)](#add_users_cli) de sorte que les utilisateurs puissent travailler avec des clusters dans {{site.data.keyword.containerlong_notm}}. Avant de commencer, consultez la rubrique [Description des rôles et des règles d'accès](#access_policies) pour examiner ce que sont les règles, à qui vous pouvez les affecter et quelles sont les ressources pouvant en bénéficier.
{: shortdesc}

Les rôles {{site.data.keyword.Bluemix_notm}} IAM ne peuvent pas être affectés à un compte de service. Vous pouvez à la place [affecter directement des rôles RBAC aux comptes de service](#rbac).
{: tip}

### Affectation de rôles {{site.data.keyword.Bluemix_notm}} IAM avec la console
{: #add_users}

Octroyez l'accès utilisateur à vos clusters en affectant des rôles de gestion de plateforme et des rôles d'accès au service {{site.data.keyword.Bluemix_notm}} IAM avec la console {{site.data.keyword.Bluemix_notm}}.
{: shortdesc}

<p class="tip">N'affectez pas de rôles de plateforme en même temps qu'un rôle de service. Vous devez affecter les rôles de plateforme et de service séparément.</p>

Avant de commencer, vérifiez que le rôle de plateforme **Administrateur** vous a été affecté pour le compte {{site.data.keyword.Bluemix_notm}} dans lequel vous travaillez.

1. Connectez-vous à la [console {{site.data.keyword.Bluemix_notm}}![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/). Dans la barre de menu, sélectionnez **Gérer > Accès (IAM)**.

2. Sélectionnez des utilisateurs individuellement ou créez un groupe d'accès d'utilisateurs.
    * **Pour affecter des rôles à un utilisateur individuel** :
      1. Dans le panneau de navigation de gauche, cliquez sur la page **Utilisateurs**, puis sur le nom de l'utilisateur dont vous souhaitez définir les droits. Si cet utilisateur n'est pas présent dans la liste, cliquez sur **Inviter des utilisateurs** pour l'ajouter au compte.
      2. Cliquez sur l'onglet **Règles d'accès**, puis sur **Affecter un accès**.
    * **Pour affecter des rôles à plusieurs utilisateurs dans un groupe d'accès** :
      1. Dans le panneau de navigation de gauch, cliquez sur la page **Groupes d'accès**.
      2. Cliquez sur **Créer** et indiquez un **Nom** et une **Description** à votre groupe. Cliquez sur **Créer**.
      3. Cliquez sur **Ajouter des utilisateurs** pour ajouter des personnes dans votre groupe d'accès. Une liste d'utilisateurs ayant accès à votre compte s'affiche.
      4. Sélectionnez la case à cocher en regard des utilisateurs que vous désirez ajouter dans le groupe. Une boîte de dialogue s'affiche.
      5. Cliquez sur **Ajouter au groupe**.
      6. Cliquez sur l'onglet **Règles d'accès**.
      7. Cliquez sur **Affecter un accès**.

3. Affectez une règle pour des groupes ou des instances de ressource. Si vous souhaitez définir la portée de la règle à un espace de nom, vous devez affecter la règle à des instances de ressource (et non pas à des groupes de ressources).
  * **Pour les groupes de ressources** :
    1. Cliquez sur **Affecter l'accès au sein d'un groupe de ressources**.
    2. Sélectionnez le nom du groupe de ressources.
    3. Dans la liste déroulante **Affecter l'accès à un groupe de ressources**, choisissez le niveau d'autorisation à attribuer à l'utilisateur sur le groupe de ressources même (et non pas sur les ressources au sein du groupe). Par exemple, pour permettre aux utilisateurs de voir les clusters auxquels ils ont accès dans plusieurs groupes de ressources, affectez-leur le rôle **Afficheur** pour chaque groupe de ressources.
    4. Dans la liste **Services**, sélectionnez **{{site.data.keyword.containershort_notm}}**.
    5. Dans la liste **Région**, sélectionnez une région ou toutes les régions.
    6. Sélectionnez un rôle pour la règle.
       * **Rôle d'accès à une plateforme** : octroie l'accès à {{site.data.keyword.containerlong_notm}} permettant aux utilisateurs de gérer les ressources d'infrastructure, telles que des clusters, des noeuds worker, des pools de noeuds worker,des équilibreurs de charge d'application Ingress et du stockage. Pour obtenir la liste des actions prises en charge par rôle, voir la [page de référence des rôles de plateforme](/docs/containers?topic=containers-access_reference#iam_platform).
       * **Rôle d'accès à un service** : octroie l'accès à Kubernetes sous forme d'accès depuis un cluster pour permettre aux utilisateurs de gérer des ressources Kubernetes, telles que des pods, des déploiements, des services et des espaces de nom. Pour obtenir la liste des actions prises en charge par rôle, voir la [page de référence des rôles de service](/docs/containers?topic=containers-access_reference#service).<p class="note">Vous ne pouvez pas définir la portée d'un rôle d'accès à un service à un espace de nom si vous affectez le rôle au niveau du groupe de ressources. Affectez l'accès à une instance de ressource à la place. N'affectez pas non plus de rôle de plateforme en même temps qu'un rôle de service.</p>
    7. Cliquez sur **Affecter**.
    8. **Facultatif** : si vous affectez uniquement un rôle de service aux utilisateurs, vous devez leur donner le nom et l'ID du cluster pour qu'ils puissent exécuter la [commande](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_config) `ibmcloud ks cluster-config`, et   [lancer le tableau de bord Kubernetes à partir de l'interface de ligne de commande](/docs/containers?topic=containers-app#db_cli) ou interagir avec l'API Kubernetes. Si vous souhaitez que ces utilisateurs soient toujours en mesure d'accéder aux clusters {{site.data.keyword.containerlong_notm}} via la console et d'afficher les clusters et d'autres ressources d'infrastructure à partir de l'interface de ligne de commande, répétez cette procédure pour leur attribuer le rôle de plateforme **Afficheur**.
  * **Pour les instances de ressource à l'intérieur de groupes de ressources ou couvrant des groupes de ressources** :
    1. Cliquez sur **Affecter l'accès aux ressources**.
    2. Dans la liste **Services**, sélectionnez **{{site.data.keyword.containershort_notm}}**.
    3. Dans la liste **Région**, sélectionnez une région ou toutes les régions.
    4. Dans la liste **Cluster**, sélectionnez une ou toutes les instances de cluster.
    5. Dans la zone **Espace de nom**, entrez le nom de l'espace de nom Kubernetes que vous voulez définir comme portée pour la règle d'_accès au service_. Notez que vous ne pouvez pas définir la portée d'une règle d'_accès à une plateforme_ à un espace de nom. La règle autorise l'accès à des espaces de nom dans tous les clusters que vous avez sélectionnés précédemment, par exemple tous les clusters d'une région. Pour accorder l'accès à tous les espaces de nom, vous pouvez laisser la zone Espace de nom vide.
    6. Sélectionnez un rôle pour la règle.
       *  **Rôle d'accès à une plateforme** : octroie l'accès à {{site.data.keyword.containerlong_notm}} permettant aux utilisateurs de gérer les ressources d'infrastructure, telles que des clusters, des noeuds worker, des pools de noeuds worker,des équilibreurs de charge d'application Ingress et du stockage. Pour obtenir la liste des actions prises en charge par rôle, voir la [page de référence des rôles de plateforme](/docs/containers?topic=containers-access_reference#iam_platform).
          * Si vous affectez à un utilisateur le rôle de plateforme **Administrateur** pour un cluster unique, vous devez également lui affecter le rôle de plateforme **Afficheur** pour tous les clusters de cette région dans le groupe de ressources.
          * Si la portée de la règle est définie à un espace de nom, vous ne pouvez pas également affecter un rôle de plateforme en même temps. Si vous souhaitez que l'utilisateur dispose également d'un rôle de plateforme, répétez ces étapes mais laissez la zone Espace de nom vide et n'affectez qu'un rôle de plateforme (ne réaffectez pas un rôle d'accès au service).
       * **Rôle d'accès à un service** : octroie l'accès à Kubernetes sous forme d'accès depuis un cluster pour permettre aux utilisateurs de gérer des ressources Kubernetes, telles que des pods, des déploiements, des services et des espaces de nom. Pour obtenir la liste des actions prises en charge par rôle, voir la [page de référence des rôles de service](/docs/containers?topic=containers-access_reference#service).
    7. Cliquez sur **Affecter**.
    8. **Facultatif** : si vous affectez uniquement des rôles de service aux utilisateurs, vous devez leur donner le nom et l'ID du cluster pour qu'ils puissent exécuter la [commande](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_config) `ibmcloud ks cluster-config`, et [lancer le tableau de bord Kubernetes à partir de l'interface de ligne de commande](/docs/containers?topic=containers-app#db_cli) ou interagir avec l'API Kubernetes. Si vous souhaitez que ces utilisateurs soient toujours en mesure d'accéder aux clusters {{site.data.keyword.containerlong_notm}} via la console et d'afficher les clusters et d'autres ressources d'infrastructure à partir de l'interface de ligne de commande, répétez cette procédure pour leur attribuer le rôle de plateforme **Afficheur**.

4.  Facultatif : si vous souhaitez que les utilisateurs puissent travailler avec des clusters dans un groupe de ressources autre que le groupe par défaut, ces utilisateurs nécessitent d'autres accès aux groupes de ressources dans lesquels se trouvent les clusters. Vous pouvez affecter au minimum à ces utilisateurs le rôle de plateforme **Afficheur** pour les groupes de ressources, si vous ne l'avez pas déjà fait.
    1.  Cliquez sur **Affecter l'accès au sein d'un groupe de ressources**.
    2.  Sélectionnez le nom du groupe de ressources.
    3.  Dans la liste **Affecter l'accès à un groupe de ressources**, sélectionnez le rôle **Afficheur**. Ce rôle autorise les utilisateurs à accéder au groupe de ressources même, mais pas aux ressources figurant dans le groupe.
    4.  Cliquez sur **Affecter**.

5.  Pour que l'utilisateur soit ajouté, les droits RBAC doivent être synchronisés dans le cluster. L'utilisateur auquel l'accès est autorisé doit [lancer le tableau de bord Kubernetes](/docs/containers?topic=containers-app#db_gui) pour débuter la synchronisation. Les droits RBAC sont mis en cache, donc la synchronisation n'est pas forcément instantanée.

### Affectation de rôles {{site.data.keyword.Bluemix_notm}} IAM avec l'interface de ligne de commande (CLI)
{: #add_users_cli}

Octroyez l'accès utilisateur à vos clusters en affectant des rôles de gestion de plateforme et des rôles d'accès au service {{site.data.keyword.Bluemix_notm}} IAM avec l'interface de ligne de commande.
{: shortdesc}

**Avant de commencer** :

- Vérifiez que le rôle de plateforme {{site.data.keyword.Bluemix_notm}} IAM `cluster-admin` vous a été affecté pour le compte {{site.data.keyword.Bluemix_notm}} dans lequel vous travaillez.
- Vérifiez que l'utilisateur est ajouté au compte. Dans le cas contraire, invitez l'utilisateur dans votre compte en exécutant la commande `ibmcloud account user-invite <user@email.com>`.
- [Connectez-vous à votre compte. Le cas échéant, ciblez le groupe de ressources approprié. Définissez le contexte pour votre cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
- Indiquez si vous souhaitez affecter des rôles [d'accès au service ou à la plateforme](/docs/containers?topic=containers-users#access_policies). Les étapes de l'interface de ligne de commande varient en fonction du rôle d'accès que vous souhaitez affecter :
  * [Affecter des rôles de plateforme à partir de l'interface de ligne de commande](#add_users_cli_platform)
  * [Affecter des rôles de service à partir de l'interface de ligne de commande](#add_users_cli_service)

**Pour affecter des rôles de _plateforme_ {{site.data.keyword.Bluemix_notm}} IAM à partir de l'interface de ligne de commande :**
{: #add_users_cli_platform}

1.  Créez une règle d'accès {{site.data.keyword.Bluemix_notm}} IAM pour définir les droits d'accès pour {{site.data.keyword.containerlong_notm}} (**`--service-name containers-kubernetes`**). Définissez la portée de la règle d'accès en fonction des éléments auxquels vous souhaitez affecter l'accès.

    <table summary="Ce tableau présente les zones d'accès que vous pouvez cibler dans la portée de la règle en utilisant des indicateurs d'interface de ligne de commande (CLI). La lecture des lignes s'effectue de gauche à droite, avec la portée dans la première colonne, l'indicateur CLI dans la deuxième colonne et la description dans la troisième colonne.">
    <caption>Options pour définir la portée de la règle d'accès</caption>
      <thead>
      <th>Portée</th>
      <th>Indicateur de l'interface CLI</th>
      <th>Description</th>
      </thead>
      <tbody>
        <tr>
        <td>Utilisateur</td>
        <td>N/A</td>
        <td>Vous pouvez affecter la règle à un utilisateur individuel ou à un groupe d'utilisateurs. Placez cet argument de position immédiatement après la commande.
        <ul><li>**Utilisateur individuel** : entrez l'adresse e-mail de l'utilisateur.</li>
        <li>**Groupe d'accès** : entrez le nom du groupe d'accès des utilisateurs. Vous pouvez créer un groupe d'accès avec la commande `ibmcloud iam access-group-create`. Pour afficher la liste des groupes d'accès disponibles, exécutez la commande `ibmcloud iam access-groups`. Pour ajouter un utilisateur à un groupe d'accès, exécutez la commande `ibmcloud iam access-group-user-add <access_group_name> <user_email>`.</li></ul></td>
        </tr>
        <tr>
        <td>Groupe de ressources</td>
        <td>`--resource-group-name`</td>
        <td>Vous pouvez affecter une règle pour un groupe de ressources. Si vous n'indiquez pas de groupe de ressources ou d'ID de cluster spécifique, la règle s'applique à tous les clusters pour tous les groupes de ressources. Pour afficher la liste des groupes de ressources disponibles, exécutez la commande `ibmcloud resource groups`.</td>
        </tr>
        <tr>
        <td>Cluster</td>
        <td>`--service-instance`</td>
        <td>Vous pouvez limiter la règle à un seul cluster. Pour afficher les ID de vos clusters, exécutez la commande `ibmcloud ks clusters`. **Remarque** : si vous affectez à un utilisateur le rôle de plateforme **Administrateur** pour un cluster unique, vous devez également lui affecter le rôle de plateforme **Afficheur** pour tous les clusters de la région dans le groupe de ressources.</td>
        </tr>
        <tr>
        <td>Région</td>
        <td>`--region`</td>
        <td>Vous pouvez définir la portée de la règle pour l'appliquer aux clusters situés dans une région particulière. Si vous n'indiquez pas de région ou d'ID de cluster spécifique, la règle s'applique à tous les clusters pour toutes les régions. Pour afficher la liste des régions disponibles, exécutez la commande `ibmcloud ks regions` et utilisez la valeur dans la colonne **Alias de région**.</td>
        </tr>
        <tr>
        <td>Rôle</td>
        <td>`--role`</td>
        <td>Choisissez le [rôle de plateforme](/docs/containers?topic=containers-access_reference#iam_platform) que vous souhaitez affecter. Les valeurs possibles sont : `Administrator`, `Operator`, `Editor` ou `Viewer`.</td>
        </tr>
      </tbody>
      </table>

    **Exemples de commandes**:

    *  Affectez à un utilisateur individuel le rôle de plateforme **Afficheur** sur un cluster dans le groupe de ressources par défaut et la région Est des Etats-Unis (us-east) :
       ```
       ibmcloud iam user-policy-create user@email.com --resource-group-name default --service-name containers-kubernetes --region us-east --service-instance clusterID-1111aa2b2bb22bb3333c3c4444dd4ee5 --roles Viewer
       ```
       {: pre}

    *  Affectez à un utilisateur individuel le rôle d'accès à la plateforme **Administrator** sur tous les clusters dans le groupe de ressources `HR` :
       ```
       ibmcloud iam user-policy-create user@email.com --resource-group-name HR --service-name containers-kubernetes [--region <region>] --roles Administrator
       ```
       {: pre}

    *  Affectez à un groupe d'utilisateurs nommé `auditors` le rôle de plateforme **Afficheur** sur tous les clusters dans tous les groupes de ressources :
       ```
       ibmcloud iam access-group-policy-create auditors --service-name containers-kubernetes --roles Viewer
       ```
       {: pre}

2. Si vous souhaitez que les utilisateurs puissent travailler avec des clusters dans un groupe de ressources autre que le groupe par défaut, ces utilisateurs nécessitent d'autres accès aux groupes de ressources dans lesquels se trouvent les clusters. Vous pouvez affecter au minimum à ces utilisateurs le rôle **Afficheur** pour les groupes de ressources. Pour obtenir l'ID du groupe de ressources, exécutez la commande `ibmcloud resource group <resource_group_name> --id`.
    *   Pour les utilisateurs individuels :
        ```
        ibmcloud iam user-policy-create <user@email.com> --resource-type resource-group --resource <resource_group_ID> --roles Viewer
        ```
        {: pre}
    *   Pour les groupes d'accès :
        ```
        ibmcloud iam access-group-policy-create <access_group> --resource-type resource-group --resource <resource_group_ID> --roles Viewer
        ```
        {: pre}

3.  Vérifiez que l'utilisateur ou le groupe d'accès dispose du rôle de plateforme affecté.
    *   Pour les utilisateurs individuels :
        ```
        ibmcloud iam user-policies <user@email.com>
        ```
        {: pre}
    *   Pour les groupes d'accès :
        ```
        ibmcloud iam access-group-policies <access_group>
        ```
        {: pre}

<br>
<br>

**Pour affecter des rôles de _service_ {{site.data.keyword.Bluemix_notm}} IAM à partir de l'interface de ligne de commande :**
{: #add_users_cli_service}

1.  Procurez-vous les informations utilisateur relatives à l'utilisateur individuel ou au groupe d'accès auquel vous souhaitez affecter le rôle de service.

    1.  Procurez-vous votre **ID de compte**.
        ```
        ibmcloud account show
        ```
        {: pre}
    2.  Pour les utilisateurs individuels, procurez-vous l'**ID utilisateur** et l'**ID unique IBM** qui leur sont associés.
        ```
        ibmcloud account users --account-id <account_ID> --output JSON
        ```
        {: pre}
    3.  Pour les groupes d'accès, procurez-vous le **nom** et l'**ID** qui leur sont associés.
        ```
        ibmcloud iam access-groups
        ```
        {: pre}

2.  Créez un fichier `policy.json` qui définit la portée du rôle d'accès au service à un espace de nom Kubernetes dans votre cluster.

    ```
    {
        "subjects": [
            {
                "attributes": [
                    {
                        "name": "(iam_id|access_group_id)",
                        "value": "<user_or_group_ID>"
                    }
                ]
            }
        ],
        "roles": [
            {
                "role_id": "crn:v1:bluemix:public:iam::::serviceRole:<(Manager|Writer|Reader)>"
            }
        ],
        "resources": [
            {
                "attributes": [
                    {
                        "name": "accountId",
                        "value": "<account_ID>"
                    },
                    {
                        "name": "serviceName",
                        "value": "containers-kubernetes"
                    },
                    {
                        "name": "serviceInstance",
                        "value": "<cluster_ID1,cluster_ID2>"
                    },
                    {
                        "name": "namespace",
                        "value": "<namespace_name>"
                    }
                ]
            }
        ]
    }
    ```
    {: codeblock}

    <table summary="Le tableau présente les zones à renseigner pour le fichier JSON. La lecture des lignes s'effectue de gauche à droite, avec la portée dans la première colonne, l'indicateur d'interface de ligne de commande dans la deuxième colonne et la description dans la troisième colonne.">
    <caption>Description des composants du fichier JSON</caption>
      <thead>
      <th colspan=2><img src="images/idea.png" alt="Idea icon"/>Description des composants du fichier JSON</th>
      </thead>
      <tbody>
        <tr>
        <td>`subjects.attributes`</td>
        <td>Entrez les détails {{site.data.keyword.Bluemix_notm}} IAM pour l'utilisateur individuel ou le groupe d'accès que vous avez précédemment extrait.
        <ul><li>Pour les utilisateurs individuels, indiquez `iam_id` pour la zone `name`. Entrez l'**ID unique IBM** que vous avez précédemment extrait pour la zone `value`.</li>
        <li>Pour les groupes d'accès, indiquez `access_group_id` pour la zone `name`. Entrez l'**ID** que vous avez précédemment extrait pour la zone `value`.</li></ul></td>
        </tr>
        <tr>
        <td>`roles.role_id`</td>
        <td>Choisissez le [rôle d'accès au service IAM](/docs/containers?topic=containers-access_reference#service) que vous souhaitez affecter. Les valeurs possibles sont :
        <ul><li>`crn:v1:bluemix:public:iam::::serviceRole:Manager`</li>
        <li>`crn:v1:bluemix:public:iam::::serviceRole:Writer`</li>
        <li>`crn:v1:bluemix:public:iam::::serviceRole:Reader`</li></ul></td>
        </tr>
        <tr>
        <td>`resources.attributes`</td>
        <td>Configurez la portée de la politique sur votre compte, cluster et espace de nom. Laissez de côté les zones `"name"` comme indiqué dans l'exemple et définissez certaines zones `"value"` comme suit :
        <ul><li>**Pour `"accountId"`**, entrez l'ID de compte {{site.data.keyword.Bluemix_notm}} que vous avez précédemment extrait.</li>
        <li>**Pour `"serviceName"`**, conservez le nom de service indiqué : `containers-kubernetes`.</li>
        <li>**Pour `"serviceInstance"`**, entrez votre ID de cluster. Pour plusieurs clusters, séparez-les par une virgule. Pour obtenir l'ID du cluster, exécutez la commande `ibmcloud ks clusters`.</li>
        <li>**Pour `"namespace"`**, entrez un espace de nom Kubernetes dans votre cluster. Pour afficher la liste des espaces de nom dans votre cluster, exécutez la commande `kubectl get namespaces`. <p class="note">Pour affecter la politique d'accès à tous les espaces de nom d'un cluster, retirez la totalité de l'entrée `{"name": "namespace", "value": "<namespace_name"}`.</p></li></td>
        </tr>
      </tbody>
      </table>

3.  Appliquez la politique {{site.data.keyword.Bluemix_notm}} IAM à un utilisateur individuel ou à un groupe d'accès.
    *   Pour les utilisateurs individuels :
        ```
        ibmcloud iam user-policy-create <user@email.com> --file <filepath>/policy.json
        ```
        {: pre}
    *   Pour les groupes d'accès :
        ```
        ibmcloud iam access-group-policy-create <access_group> --file <filepath>/policy.json
        ```
        {: pre}

4.  Si vous affectez uniquement des rôles de service aux utilisateurs, vous devez leur donner le nom et l'ID du cluster pour qu'ils puissent exécuter la [commande](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_config) `ibmcloud ks cluster-config`, et [lancer le tableau de bord Kubernetes à partir de l'interface de ligne de commande](/docs/containers?topic=containers-app#db_cli) ou interagir avec l'API Kubernetes. Si vous souhaitez que ces utilisateurs soient toujours en mesure d'accéder aux clusters {{site.data.keyword.containerlong_notm}} via la console et d'afficher les clusters et d'autres ressources d'infrastructure à partir de l'interface de ligne de commande, [donnez-leur le rôle de plateforme **Afficheur**](#add_users_cli_platform).

5.  Pour que les modifications prennent effet, l'utilisateur auquel l'accès a été octroyé doit actualiser la configuration du cluster. Les utilisateurs ne sont pas ajoutés aux liaisons de rôle tant qu'ils n'actualisent pas individuellement la configuration du cluster, même si vous avez ajouté plusieurs utilisateurs en même temps. Les utilisateurs ne sont pas non plus ajoutés à une liaison de rôle s'ils disposent de droits supérieurs. Par exemple, si les utilisateurs disposent d'un rôle de cluster et se trouvent dans une liaison de rôle de cluster, ils ne sont pas non plus ajoutés à chaque liaison de rôle d'espace de nom individuelle.
    ```
    ibmcloud ks cluster-config --cluster <cluster_name_or_id>
    ```
    {: pre}

6.  **Facultatif** : vérifiez que l'utilisateur est ajouté à la [liaison de rôle RBAC ou de rôle de cluster](#role-binding) correspondante. Notez que vous devez être administrateur du cluster (rôle de service **Responsable** dans tous les espaces de nom) pour vérifier les liaisons de rôle et les liaisons de rôle de cluster.
    Vérifiez les liaisons de rôle ou les liaisons de rôle de cluster correspondant au rôle.
    *   Reader :
        ```
        kubectl get rolebinding ibm-view -o yaml -n <namespace>
        ```
        {: pre}
    *   Writer :
        ```
        kubectl get rolebinding ibm-edit -o yaml -n <namespace>
        ```
        {: pre}
    *   Manager, avec une portée définie à l'espace de nom :
        ```
        kubectl get rolebinding ibm-operate -o yaml -n <namespace>
        ```
        {: pre}
    *   Manager, pour tous les espaces de nom :
        ```
        kubectl get clusterrolebinding ibm-admin -o yaml
        ```
        {: pre}

    **Exemple de sortie** : vous obtenez l'exemple de sortie suivant si vous affectez à l'utilisateur `user@email.com` et au groupe d'accès `team1` le rôle de service **Lecteur** et que vous exécutez la commande `kubectl get rolebinding ibm-view -o yaml -n default`.

    ```
    apiVersion: rbac.authorization.k8s.io/v1
    kind: RoleBinding
    metadata:
      creationTimestamp: 2018-05-23T14:34:24Z
      name: ibm-view
      namespace: default
      resourceVersion: "8192510"
      selfLink: /apis/rbac.authorization.k8s.io/v1/namespaces/default/rolebindings/ibm-view
      uid: 63f62887-5e96-11e8-8a75-b229c11ba64a
    roleRef:
      apiGroup: rbac.authorization.k8s.io
      kind: ClusterRole
      name: view
    subjects:
    - apiGroup: rbac.authorization.k8s.io
      kind: User
      name: IAM#user@email.com
    - apiGroup: rbac.authorization.k8s.io
      kind: group
      name: team1
    ```
    {: screen}

<br />


## Affectation de droits RBAC
{: #role-binding}

Utilisez des rôles RBAC pour définir les actions qu'un utilisateur peut effectuer pour utiliser des ressources Kubernetes dans votre cluster.
{: shortdesc}

**Que sont les rôles RBAC et les rôles de cluster ?**</br>
Les rôles RBAC et les rôles de cluster définissent un ensemble de droits pour déterminer comment les utilisateurs peuvent interagir avec les ressources Kubernetes dans votre cluster. Un rôle porte sur les ressources au sein d'un espace de nom spécifique, tel qu'un déploiement. Un rôle de cluster porte sur les ressources à l'échelle du cluster, par exemple des noeuds worker ou sur les ressources d'un espace de nom pouvant se trouver dans chaque espace de nom, comme par exemple des pods.

**Que sont les liaisons de rôle RBAC et les liaisons de rôle de cluster ?**</br>
Les liaisons de rôle appliquent des rôles RBAC ou des rôles de cluster à un espace de nom spécifique. Lorsque vous utilisez une liaison de rôle pour appliquer un rôle, vous octroyez à un utilisateur l'accès à une ressource spécifique dans un espace de nom spécifique. Lorsque vous utilisez une liaison de rôle pour appliquer un rôle de cluster, vous octroyez à un utilisateur l'accès aux ressources propres à un espace de nom pouvant se trouver dans chaque espace de nom, par exemple des pods, mais uniquement au sein d'un espace de nom spécifique.

Les liaisons de rôle de cluster appliquent des rôles de cluster RBAC à tous les espaces de nom du cluster. Lorsque vous utilisez une liaison de rôle de cluster pour appliquer un rôle de cluster, vous octroyez l'accès aux ressources à l'échelle du cluster, par exemple des noeuds worker, ou aux ressources d'espace de nom dans tous les espaces de nom, par exemple des pods.

**A quoi ressemblent ces rôles dans mon cluster ?**</br>
Si vous souhaitez que les utilisateurs soient en mesure d'interagir avec des ressources Kubernetes depuis un cluster, vous devez leur affecter l'accès à un ou plusieurs espaces de nom via des [rôles de service {{site.data.keyword.Bluemix_notm}} IAM](#platform). Tous les utilisateurs auxquels est affecté un rôle de service bénéficient automatiquement d'un rôle de cluster RBAC correspondant. Ces rôles de cluster RBAC sont prédéfinis et permettent aux utilisateurs d'interagir avec les ressources Kubernetes dans votre cluster. En outre, une liaison de rôle est créée pour appliquer le rôle de cluster à un espace de nom spécifique ou à tous les espaces de nom.

Pour en savoir plus sur les actions autorisées par chaque rôle RBAC, consultez la rubrique de référence [Rôles de service {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-access_reference#service). Pour afficher les droits octroyés par chaque rôle RBAC à des ressources Kubernetes individuelles, consultez [Droits sur les ressources Kubernetes par rôle RBAC](/docs/containers?topic=containers-access_reference#rbac_ref).
{: tip}

**Puis-je créer des rôles personnalisés ou des rôles de cluster ?**
Les rôles de cluster `view`, `edit`, `admin` et `cluster-admin` sont des rôles prédéfinis qui sont automatiquement créés lorsque vous affectez à un utilisateur le rôle de service {{site.data.keyword.Bluemix_notm}} IAM correspondant. Pour octroyer d'autres droits Kubernetes, vous pouvez [créer vos propres droits RBAC personnalisés](#rbac). Les rôles RBAC personnalisés s'ajoutent aux rôles RBAC que vous avez pu affecter avec des rôles de service. Ils ne modifient en rien les rôles RBAC et ne les remplacent pas. Notez que pour créer des droits RBAC personnalisés, vous devez disposer du rôle d'accès au service IAM **Manager** qui vous donne le rôle RBAC Kubernetes `cluster-admin`. Les autres utilisateurs n'ont toutefois pas besoin d'un rôle d'accès au service IAM si vous gérez vos propres rôles RBAC personnalisés.

Vous créez vos propres règles RBAC ? Veillez à ne pas modifier les liaisons de rôle IBM existantes figurant dans le cluster ou attribuez le même nom aux nouvelles liaisons de rôle. Toutes les modifications apportées aux liaisons de rôle RBAC fournies par IBM sont remplacées à intervalles réguliers. Créez vos propres liaisons de rôle à la place.
{: tip}

**Quand dois-je utiliser des liaisons de rôle de cluster et des liaisons de rôle qui ne sont pas liées aux droits d'accès {{site.data.keyword.Bluemix_notm}} IAM et que je définis moi-même ?**
Vous envisagerez peut-être d'autoriser certaines personnes à créer et à mettre à jour des pods dans votre cluster. Avec les [politiques de sécurité de pod](/docs/containers?topic=containers-psp#psp), vous pouvez utiliser des liaisons de rôle de cluster existantes fournies avec votre cluster ou créez les vôtres.

Vous pouvez également souhaiter intégrer des modules complémentaires dans votre cluster. Par exemple, lorsque vous [configurez Helm dans votre cluster](/docs/containers?topic=containers-helm#public_helm_install), vous devez créer un compte de service pour Tiller dans l'espace de nom `kube-system` et une liaison de rôle de cluster RBAC Kubernetes pour le pod `tiller-deploy`.

### Création de droits RBAC personnalisés pour les utilisateurs, les groupes ou les comptes de service
{: #rbac}

Les rôles de cluster `view`, `edit`, `admin` et `cluster-admin` sont automatiquement créés lorsque vous affectez le rôle de service {{site.data.keyword.Bluemix_notm}} IAM correspondant. Vous nécessitez que les règles d'accès à votre cluster soient plus granulaires que ne le permettent les droits d'accès prédéfinis ? Pas de problème ! Vous pouvez créer des rôles RBAC et des rôles de cluster personnalisés.
{: shortdesc}

Vous pouvez affecter des rôles RBAC et des rôles de cluster à des utilisateurs individuels, à des groupes d'utilisateurs (dans les clusters exécutant Kubernetes version 1.11 ou ultérieure) ou à des comptes de service. Lorsqu'une liaison est créée pour un groupe, elle concerne tout utilisateur qui est ajouté ou supprimé dans ce groupe. Lorsque vous ajoutez des utilisateurs dans un groupe, ils ont accès aux droits du groupe en plus des droits d'accès individuels que vous leur octroyez. S'ils sont supprimés, leur accès est révoqué. Notez que vous ne pouvez pas ajouter des comptes de service à des groupes d'accès.

Pour affecter l'accès à un processus qui s'exécute dans des pods, par exemple une chaîne d'outils à distribution continue, vous pouvez utiliser des [comptes de service (`ServiceAccounts`) Kubernetes  ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/reference/access-authn-authz/service-accounts-admin/). Pour suivre un tutoriel qui démontre comment configurer des comptes de service pour Travis et Jenkins et pour affecter des rôles RBAC personnalisés aux comptes de service, voir l'article de blogue [Kubernetes `ServiceAccounts` for use in automated systems ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://medium.com/@jakekitchener/kubernetes-serviceaccounts-for-use-in-automated-systems-515297974982).

Pour éviter toute modification avec rupture, ne modifiez pas les rôles de cluster prédéfinis `view`, `edit`, `admin` et `cluster-admin`. Les rôles RBAC personnalisés s'ajoutent aux rôles RBAC que vous avez pu affecter avec des rôles d'accès au service {{site.data.keyword.Bluemix_notm}}. Ils ne modifient en rien les rôles RBAC et ne les remplacent pas.
{: important}

**Comment créer un rôle ou un rôle de cluster ? Faut-il l'appliquer avec une liaison de rôle ou une liaison de rôle de cluster ?**

* **Accès dans un espace de nom** : pour autoriser un utilisateur, un groupe d'accès ou un compte de service à accéder à une ressource dans un espace de nom spécifique, choisissez l'une des combinaisons suivantes :
  * Créez un rôle et appliquez-le avec une liaison de rôle. Cette option est utile pour contrôler l'accès à une ressource unique qui n'existe que dans un espace de nom, par exemple un déploiement d'application.
  * Créez un rôle de cluster et appliquez-le avec une liaison de rôle. Cette option est utile pour contrôler l'accès à des ressources générales dans un espace de nom, par exemple des pods.
* **Accès à l'échelle du cluster** : pour autoriser un utilisateur ou un groupe d'accès à accéder à des ressources à l'échelle du cluster ou à des ressources dans tous les espaces de nom, créez un rôle de cluster et appliquez-le avec une liaison de rôle de cluster. Cette option est utile pour contrôler l'accès aux ressources qui ne sont pas limitées aux espaces de nom, par exemple des noeuds worker, ou à des ressources dans tous les espaces de nom de votre cluster, par exemple des pods dans chaque espace de nom.

**Avant de commencer** :

- Ciblez l'[interface CLI de Kubernetes](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure) sur votre cluster.
- Vérifiez que vous disposez du [rôle de service {{site.data.keyword.Bluemix_notm}} IAM **Manager**](/docs/containers?topic=containers-users#platform) pour tous les espaces de nom.
- Pour affecter l'accès à des utilisateurs individuels ou à des utilisateurs dans un groupe d'accès, vérifiez que l'utilisateur ou le groupe dispose au moins d'un [rôle de plateforme {{site.data.keyword.Bluemix_notm}} IAM](#platform) au niveau du service {{site.data.keyword.containerlong_notm}}.

**Pour créer des droits RBAC personnalisés** :

1. Créez le rôle ou le rôle de cluster avec l'accès que vous souhaitez affecter.

    1. Créez un fichier `.yaml` pour définir le rôle ou le rôle de cluster.

        ```
        kind: Role
        apiVersion: rbac.authorization.k8s.io/v1
        metadata:
          namespace: default
          name: my_role
        rules:
        - apiGroups: [""]
          resources: ["pods"]
          verbs: ["get", "watch", "list"]
        - apiGroups: ["apps", "extensions"]
          resources: ["daemonsets", "deployments"]
          verbs: ["get", "list", "watch", "create", "update", "patch", "delete"]
        ```
        {: codeblock}

        <table>
        <caption>Description des composants du fichier YAML</caption>
          <thead>
            <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Descriptions des composants du fichier YAML</th>
          </thead>
          <tbody>
            <tr>
              <td><code>kind</code></td>
              <td>Utilisez `Role` pour octroyer l'accès aux ressources dans un espace de nom spécifique. Utilisez `ClusterRole` pour octroyer l'accès aux ressources à l'échelle du cluster, par exemple des noeuds worker, ou à des ressources limitées à des espaces de nom, par exemple des pods dans tous les espaces de nom.</td>
            </tr>
            <tr>
              <td><code>apiVersion</code></td>
              <td><ul><li>Pour les clusters exécutant Kubernetes 1.8 ou version ultérieure, utilisez `rbac.authorization.k8s.io/v1`. </li><li>Pour les versions antérieures, utilisez `apiVersion: rbac.authorization.k8s.io/v1beta1`.</li></ul></td>
            </tr>
            <tr>
              <td><code>metadata.namespace</code></td>
              <td>Pour la section kind `Role` uniquement : indiquez l'espace de nom Kubernetes auquel l'accès est octroyé.</td>
            </tr>
            <tr>
              <td><code>metadata.name</code></td>
              <td>Nom du rôle ou du rôle de cluster.</td>
            </tr>
            <tr>
              <td><code>rules.apiGroups</code></td>
              <td>Indiquez les [groupes d'API ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/reference/using-api/api-overview/#api-groups) Kubernetes avec lesquels vous voulez que les utilisateurs puissent interagir, par exemple `"apps"`, `"batch"` ou `"extensions"`. Pour accéder au groupe d'API principal sur le chemin REST `api/v1`, laissez le groupe vide : `[""]`.</td>
            </tr>
            <tr>
              <td><code>rules.resources</code></td>
              <td>Indiquez les [types de ressources ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/reference/kubectl/cheatsheet/) Kubernetes auxquelles vous souhaitez octroyer l'accès, par exemple `"daemonsets"`, `"deployments"`, `"events"` ou `"ingresses"`. Si vous spécifiez `"nodes"`, la valeur de kind doit être `ClusterRole`.</td>
            </tr>
            <tr>
              <td><code>rules.verbs</code></td>
              <td>Indiquez les types d'[actions ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubectl.docs.kubernetes.io/) que vous désirez que les utilisateurs puissent effectuer, par exemple `"get"`, `"list"`, `"describe"`, `"create"` ou `"delete"`.</td>
            </tr>
          </tbody>
        </table>

    2. Créez le rôle ou le rôle de cluster dans votre cluster.

        ```
        kubectl apply -f my_role.yaml
        ```
        {: pre}

    3. Vérifiez que le rôle ou le rôle de cluster est créé.
      * Rôle :
          ```
          kubectl get roles -n <namespace>
          ```
          {: pre}

      * Rôle de cluster :
          ```
          kubectl get clusterroles
          ```
          {: pre}

2. Liez les utilisateurs au rôle ou au rôle de cluster.

    1. Créez un fichier `.yaml` pour lier les utilisateurs à votre rôle ou rôle de cluster. Notez l'unique URL à utiliser pour chaque nom dans 'subjects'.

        ```
        kind: RoleBinding
        apiVersion: rbac.authorization.k8s.io/v1
        metadata:
          name: my_role_binding
          namespace: default
        subjects:
        - kind: User
          name: IAM#user1@example.com
          apiGroup: rbac.authorization.k8s.io
        - kind: Group
          name: team1
          apiGroup: rbac.authorization.k8s.io
        - kind: ServiceAccount
          name: <service_account_name>
          namespace: <kubernetes_namespace>
        roleRef:
          kind: Role
          name: my_role
          apiGroup: rbac.authorization.k8s.io
        ```
        {: codeblock}

        <table>
        <caption>Description des composants du fichier YAML</caption>
          <thead>
            <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Descriptions des composants du fichier YAML</th>
          </thead>
          <tbody>
            <tr>
              <td><code>kind</code></td>
              <td><ul><li>Indiquez `RoleBinding` pour un rôle (`Role`) ou un rôle de cluster (`ClusterRole`) spécifique à un espace de nom.</li><li>Indiquez `ClusterRoleBinding` pour un rôle de cluster (`ClusterRole`) à l'échelle d'un cluster.</li></ul></td>
            </tr>
            <tr>
              <td><code>apiVersion</code></td>
              <td><ul><li>Pour les clusters exécutant Kubernetes 1.8 ou version ultérieure, utilisez `rbac.authorization.k8s.io/v1`. </li><li>Pour les versions antérieures, utilisez `apiVersion: rbac.authorization.k8s.io/v1beta1`.</li></ul></td>
            </tr>
            <tr>
              <td><code>metadata.namespace</code></td>
              <td><ul><li>Pour la section kind `RoleBinding` : indiquez l'espace de nom Kubernetes auquel l'accès est octroyé.</li><li>Pour la section kind `ClusterRoleBinding` : n'utilisez pas la zone `namespace`.</li></ul></td>
            </tr>
            <tr>
              <td><code>metadata.name</code></td>
              <td>Nommez la liaison de rôle ou la liaison de rôle de cluster.</td>
            </tr>
            <tr>
              <td><code>subjects.kind</code></td>
              <td>Indiquez l'une des valeurs suivantes pour kind :
              <ul><li>`User` : liez le rôle RBAC ou le rôle de cluster à un utilisateur individuel dans votre compte.</li>
              <li>`Group` : pour les clusters exécutant Kubernetes 1.11 ou version ultérieure, liez le rôle RBAC ou le rôle de cluster à un [groupe d'accès {{site.data.keyword.Bluemix_notm}} IAM](/docs/iam?topic=iam-groups#groups) dans votre compte.</li>
              <li>`ServiceAccount` : liez le rôle RBAC ou le rôle de cluster à un compte de service dans un espace de nom de votre cluster.</li></ul></td>
            </tr>
            <tr>
              <td><code>subjects.name</code></td>
              <td><ul><li>Pour `User` : ajoutez l'adresse électronique de l'utilisateur individuel à `IAM#` comme suit : <code>IAM#user@email.com</code>.</li>
              <li>Pour `Group` : pour les clusters exécutant Kubernetes version 1.11 ou ultérieure, indiquez le nom du [groupe d'accès {{site.data.keyword.Bluemix_notm}} IAM](/docs/iam?topic=iam-groups#groups) dans votre compte.</li>
              <li>Pour `ServiceAccount` : spécifiez le nom du compte de service.</li></ul></td>
            </tr>
            <tr>
              <td><code>subjects.apiGroup</code></td>
              <td><ul><li>Pour `User` ou `Group` : utilisez `rbac.authorization.k8s.io`.</li>
              <li>Pour `ServiceAccount` : n'incluez pas cette zone.</li></ul></td>
            </tr>
            <tr>
              <td><code>subjects.namespace</code></td>
              <td>Pour `ServiceAccount` uniquement : indiquez le nom de l'espace de nom Kubernetes dans lequel est déployé le compte de service.</td>
            </tr>
            <tr>
              <td><code>roleRef.kind</code></td>
              <td>Entrez la même valeur que pour `kind` dans le fichier `.yaml` du rôle : `Role` ou `ClusterRole`.</td>
            </tr>
            <tr>
              <td><code>roleRef.name</code></td>
              <td>Entrez le nom du fichier `.yaml` du rôle.</td>
            </tr>
            <tr>
              <td><code>roleRef.apiGroup</code></td>
              <td>Utilisez `rbac.authorization.k8s.io`.</td>
            </tr>
          </tbody>
        </table>

    2. Créez la ressource de liaison de rôle ou de liaison de rôle de cluster dans votre cluster.

        ```
        kubectl apply -f my_role_binding.yaml
        ```
        {: pre}

    3.  Vérifiez que la liaison est créée.

        ```
        kubectl get rolebinding -n <namespace>
        ```
        {: pre}

3. Facultatif : pour imposer le même niveau d'accès utilisateur dans d'autres espaces de nom, vous pouvez copier les liaisons de rôle pour ces rôles ou rôles de cluster dans d'autres espaces de nom.
    1. Copiez la liaison de rôle d'un espace de nom à un autre.
        ```
        kubectl get rolebinding <role_binding_name> -o yaml | sed 's/<namespace_1>/<namespace_2>/g' | kubectl -n <namespace_2> create -f -
        ```
        {: pre}

        Par exemple, pour copier la liaison de rôle `custom-role` de l'espace de nom `default` dans l'espace de nom `testns` :
        ```
        kubectl get rolebinding custom-role -o yaml | sed 's/default/testns/g' | kubectl -n testns create -f -
        ```
        {: pre}

    2. Vérifiez que la liaison de rôle a été copiée. Si vous avez ajouté un groupe d'accès {{site.data.keyword.Bluemix_notm}} IAM dans la liaison de rôle, chaque utilisateur de ce groupe est ajouté individuellement, et non pas en tant qu'ID de groupe d'accès.
        ```
        kubectl get rolebinding -n <namespace_2>
        ```
        {: pre}

Vous venez de créer et de lier un rôle de cluster ou un rôle RBAC Kubernetes personnalisé, effectuez à présent le suivi auprès des utilisateurs. Demandez-leur de tester une action qu'ils ont le droit d'effectuer en fonction du rôle, par exemple, supprimer un pod.

### Extension des droits existants en agrégeant des rôles de cluster 
{: #rbac_aggregate}

Vous pouvez étendre les droits existantes de vos utilisateurs en agrégeant ou en combinant des rôles de cluster à d'autres rôles de cluster. Lorsque vous affectez un rôle de service {{site.data.keyword.Bluemix_notm}} à un utilisateur, celui-ci est ajouté à un [rôle de cluster RBAC Kubernetes correspondant](/docs/containers?topic=containers-access_reference#service). Cependant, vous souhaiterez peut-être autoriser certains utilisateurs à effectuer des opérations supplémentaires.
{: shortdesc}

Par exemple, un utilisateur doté du rôle de cluster `admin` propre à un espace de nom ne peut pas utiliser la commande `kubectl top pods` pour visualiser les métriques de pod pour tous les pods dans l'espace de nom. Vous pouvez agréger un rôle de cluster de manière à autoriser les utilisateurs dotés du rôle de cluster `admin` à exécuter la commande `top pods`. Pour plus d'informations, [voir la documentation Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#aggregated-clusterroles).

**Quelles sont certaines des opérations courantes pour lesquelles je souhaite étendre les droits pour un rôle de cluster par défaut ?**<br>
Passez en revue [les opérations autorisées par chaque rôle de cluster RBAC par défaut](/docs/containers?topic=containers-access_reference#rbac_ref) afin d'obtenir une bonne idée de ce que vos utilisateurs peuvent faire, puis comparez ces opérations autorisées à celles que vos utilisateurs doivent pouvoir effectuer. 

Si vos utilisateurs disposant du même rôle de cluster rencontrent des erreurs semblables aux suivantes pour le même type d'opération, vous souhaiterez peut-être étendre le rôle de cluster afin d'inclure cette opération. 

```
Error from server (Forbidden): pods.metrics.k8s.io is forbidden: User "IAM#myname@example.com" cannot list resource "pods" in API group "metrics.k8s.io" in the namespace "mynamespace"
```
{: screen}

**Pour agréger des rôles de cluster** :

Avant de commencer : [connectez-vous à votre compte. Le cas échéant, ciblez le groupe de ressources approprié. Définissez le contexte pour votre cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  Créez un fichier YAML de rôle de cluster. Dans la section `labels`, spécifiez le rôle de cluster existant auquel vous souhaitez agréger des droits. L'exemple suivant étend le rôle de cluster `admin` prédéfini pour autoriser les utilisateurs à exécuter `kubectl top pods`. Pour obtenir davantage d'exemples, [voir la documentation Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#aggregated-clusterroles).
    ```
    apiVersion: rbac.authorization.k8s.io/v1
    kind: ClusterRole
    metadata:
      name: view-pod-metrics
      labels:
        rbac.authorization.k8s.io/aggregate-to-admin: "true"
    rules:
    - apiGroups:
      - "metrics.k8s.io"
      resources:
      - pods
      verbs:
      - list
    ```
    {: codeblock}
    
    <table>
    <caption>Description des composants du fichier YAML</caption>
      <thead>
        <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Descriptions des composants du fichier YAML</th>
      </thead>
      <tbody>
        <tr>
          <td><code>metadata.name</code></td>
          <td>Entrez un nom pour le rôle de cluster. Vous ne **devez pas utiliser** les noms de rôle de cluster prédéfinis : `view`, `edit`, `admin` et `cluster-admin`.</td>
        </tr>
        <tr>
          <td><code>metadata.labels</code></td>
          <td>Ajoutez un libellé correspondant au rôle de cluster que vous souhaitez agréger au format `rbac.authorization.k8s.io/aggregate-to-<cluster_role> : "true"`. Les libellés pour les rôles de cluster prédéfinies sont les suivants :<ul>
          <li>Rôle de service **Manager** IAM, propre à un espace de nom : `rbac.authorization.k8s.io/aggregate-to-admin: "true"`</li>
          <li>Rôle de service **Writer** IAM : `rbac.authorization.k8s.io/aggregate-to-edit: "true"`</li>
          <li>Rôle de service **Reader** IAM : `rbac.authorization.k8s.io/aggregate-to-view: "true"`</li></ul></td>
        </tr>
        <tr>
          <td><code>rules.apiGroups</code></td>
          <td>Indiquez les [groupes d'API ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/reference/using-api/api-overview/#api-groups) Kubernetes avec lesquels vous voulez que les utilisateurs puissent interagir, par exemple `"apps"`, `"batch"` ou `"extensions"`. Pour accéder au groupe d'API principal sur le chemin REST `api/v1`, laissez le groupe vide : `[""]`.</td>
        </tr>
        <tr>
          <td><code>rules.resources</code></td>
          <td>Indiquez les [types de ressources ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/reference/kubectl/cheatsheet/) Kubernetes auxquelles vous souhaitez octroyer l'accès, par exemple `"daemonsets"`, `"deployments"`, `"events"` ou `"ingresses"`.</td>
        </tr>
        <tr>
          <td><code>rules.verbs</code></td>
          <td>Indiquez les types d'[actions ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubectl.docs.kubernetes.io/) que vous désirez que les utilisateurs puissent effectuer, par exemple `"get"`, `"list"`, `"describe"`, `"create"` ou `"delete"`.</td>
        </tr>
      </tbody>
    </table>
2.  Créez le rôle de cluster dans votre cluster. Tout utilisateur ayant une liaison de rôle au rôle de cluster `admin` dispose à présent des droits supplémentaires issus du rôle de cluster `view-pod-metrics`.
    ```
    kubectl apply -f <cluster_role_file.yaml>
    ```
    {: pre}
3.  Assurez le suivi des utilisateurs qui disposent du rôle de cluster `admin`. Demandez-leur d'[actualiser leur configuration de cluster](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure) et de tester l'action, par exemple, `kubectl top pods`.


<br />


## Personnalisation des droits d'infrastructure
{: #infra_access}

Lorsque vous affectez le rôle d'infrastructure **Superutilisateur** à l'administrateur qui définit la clé d'API ou dont les données d'identification sont définies, d'autres utilisateurs du compte partagent la clé d'API ou les données d'identification pour effectuer des actions de l'infrastructure. Vous pouvez ensuite contrôler les actions de l'infrastructure pouvant être effectuées par les utilisateurs en affectant le [rôle de plateforme {{site.data.keyword.Bluemix_notm}} IAM](#platform) approprié. Vous n'avez pas besoin d'éditer les droits de l'infrastructure IBM Cloud (SoftLayer).
{: shortdesc}

Pour des raisons de conformité, de sécurité ou de facturation, vous ne souhaiterez pas forcément octroyer le rôle d'infrastructure **Superutilisateur** à l'utilisateur ayant défini la clé d'API ou dont les données d'identification sont définies avec la commande `ibmcloud ks credential-set`. En revanche, si cet utilisateur ne dispose pas du rôle **Superutilisateur**, les actions liées à l'infrastructure, par exemple la création d'un cluster ou le rechargement d'un noeud worker, risquent d'échouer. Au lieu d'utiliser des rôles de plateforme {{site.data.keyword.Bluemix_notm}} IAM pour contrôler l'accès des utilisateurs à l'infrastructure, vous devez définir des droits d'infrastructure IBM Cloud (SoftLayer) spécifiques pour les utilisateurs.

Par exemple, si la fonction VRF n'est pas activée pour votre compte, votre propriétaire de compte IBM Cloud infrastructure (SoftLayer) doit activer la fonction Spanning VLAN. Le propriétaire du compte peut également affecter à un utilisateur le droit **Réseau > Gérer spanning VLAN pour réseau** pour que cet utilisateur puisse activer la fonction Spanning VLAN. Pour plus d'informations, voir [Spanning VLAN pour les communications entre les VLAN](/docs/containers?topic=containers-subnets#basics_segmentation).

<p class="tip">Vous avez déjà défini les données d'identification de l'infrastructure à l'aide de la commande `ibmcloud ks credential-set` ? Vous pouvez vérifier si les droits d'infrastructure suggérés ou requis sont manquants dans les données d'identification en exécutant la [commande `ibmcloud ks infra-permissions-get --region <region>`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#infra_permissions_get). Dans la sortie, si des droits suggérés ou requis sont manquants, vous pouvez utiliser les étapes de cette section pour affecter l'accès requis. </p>

Avant de commencer, vérifiez que vous êtes bien le propriétaire du compte ou que vous disposez du rôle **Superutilisateur** et de l'accès à toutes les unités. Vous ne pouvez pas octroyer un accès utilisateur dont vous ne bénéficiez pas vous-même.

1. Connectez-vous à la [console {{site.data.keyword.Bluemix_notm}} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com). Dans la barre de menu, sélectionnez **Gérer > Accès (IAM)**.

2. Cliquez sur la page **Utilisateurs**, puis sur le nom de l'utilisateur dont vous souhaitez définir les droits.

3. Cliquez sur l'onglet **Infrastructure classique**, puis sur l'onglet **Droits**.

4. Personnalisez l'accès des utilisateurs. Les autorisations dont les utilisateurs ont besoin dépendent des ressources d'infrastructure qu'ils doivent utiliser. Vous disposez de deux options pour affecter l'accès :
    * Utiliser la liste déroulante **Ensembles de droits** pour affecter l'un des rôles prédéfinis suivants. Après avoir sélectionné un rôle, cliquez sur **Définir**.
        * **Affichage uniquement** permet à l'utilisateur d'afficher les détails de l'infrastructure uniquement.
        * **Utilisateur de base** octroie à l'utilisateur certains mais pas tous les droits sur l'infrastructure.
        * **Superutilisateur** octroie à l'utilisateur tous les droits sur l'infrastructure.
    * Sélectionner des droits individuels pour chaque catégorie. Pour consulter les droits nécessaires à l'exécution de tâches courantes dans {{site.data.keyword.containerlong_notm}}, voir [Droits d'accès des utilisateurs](/docs/containers?topic=containers-access_reference#infra).

5.  Cliquez sur **Sauvegarder**.

6.  Dans l'onglet **Périphérique**, sélectionnez les périphériques auxquels accorder l'accès.

    * Dans le groupe **Sélectionner un type**, vous pouvez accorder l'accès à tous les serveurs bare metal, dédiés et virtuels permettant ainsi à tous les utilisateurs de travailler avec tous les [types de machine pour les noeuds worker](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes).
    * Dans le groupe **Activer un accès ultérieur**, vous pouvez accorder l'accès utilisateur à tous les serveurs bare metal, dédiés et virtuels qui seront ajoutés par la suite.
    * Dans la table des périphériques, vérifiez que les périphériques appropriés sont sélectionnés.

7. Pour sauvegarder vos modifications, cliquez sur **Définir**.

8. **Important** : si vous affectez des droits de manière à permettre à un utilisateur de gérer des clusters et des noeuds worker, vous devez affecter à l'utilisateur un accès lui permettant de gérer des cas de support. 
  1. Cliquez sur l'onglet **Règles d'accès**, puis sur **Affecter un accès**.
  2. Cliquez sur la carte **Affecter l'accès aux services de gestion des comptes**. 
  3. Sélectionnez **Centre de support**.
  4. Pour autoriser l'utilisateur à visualiser, ajouter et éditer des cas de support, sélectionnez **Administrateur**.
  5. Cliquez sur **Affecter**.

Rétromigration de droits ? L'exécution de cette action peut prendre quelques minutes.
{: tip}

<br />


## Retrait de droits utilisateur
{: #removing}

Si un utilisateur n'a plus besoin de droits d'accès spécifiques ou qu'il quitte votre organisation, le propriétaire du compte {{site.data.keyword.Bluemix_notm}} peut retirer les droits de cet utilisateur.
{: shortdesc}

Avant de retirer des droits d'accès spécifiques à un utilisateur ou retirer complètement un utilisateur de votre compte, vérifiez que les données d'identification d'infrastructure de l'utilisateur ne sont pas utilisées pour définir la clé d'API ou pour la commande `ibmcloud ks credential-set`. Autrement, les autres utilisateurs du compte pourraient perdre leur accès au portail de l'infrastructure IBM Cloud (SoftLayer) et les commandes liées à l'infrastructure pourraient échouer.
{: important}

1. Ciblez le contexte de l'interface de ligne de commande sur une région et un groupe de ressources dans lequel vous disposez de clusters.
    ```
    ibmcloud target -g <resource_group_name> -r <region>
    ```
    {: pre}

2. Vérifiez le propriétaire de la clé d'API ou des données d'infrastructure définies pour cette région et ce groupe de ressources.
    * Si vous utilisez la [clé d'API pour accéder au portefeuille de l'infrastructure IBM Cloud (SoftLayer)](#default_account) :
        ```
        ibmcloud ks api-key-info --cluster <cluster_name_or_id>
        ```
        {: pre}
    * Si vous définissez les [données d'identification d'infrastructure pour accéder au portefeuille de l'infrastructure IBM Cloud (SoftLayer)](#credentials) :
        ```
        ibmcloud ks credential-get --region <region>
        ```
        {: pre}

3. Si le nom d'utilisateur de l'utilisateur est renvoyé, utilisez les données d'identification d'un autre utilisateur pour définir la clé d'API ou des données d'identification de l'infrastructure.

  Si le propriétaire du compte ne définit pas la clé d'API ou si vous ne définissez pas les données d'identification de l'infrastructure du propriétaire du compte, [vérifiez que l'utilisateur qui définit la clé d'API ou dont vous définissez les données d'identification dispose des droits appropriés](#owner_permissions).
  {: note}

    * Pour réinitialiser la clé d'API :
        ```
    ibmcloud ks api-key-reset --region <region>
    ```
        {: pre}
    * Pour réinitialiser les données d'identification de l'infrastructure :
        ```
        ibmcloud ks credential-set --infrastructure-username <infrastructure_API_username> --infrastructure-api-key <infrastructure_API_authentication_key> --region <region>
        ```
        {: pre}

4. Répétez ces étapes pour chaque combinaison de groupes de ressources et de régions où vous détenez des clusters.

### Retrait d'un utilisateur de votre compte
{: #remove_user}

Si un utilisateur dans votre compte quitte votre organisation, vous devez retirer les droits de cet utilisateur avec précaution pour vous assurer de ne pas avoir de clusters orphelins ou d'autres ressources orphelines au final. Vous pouvez ensuite retirer l'utilisateur de votre compte {{site.data.keyword.Bluemix_notm}}.
{: shortdesc}

Avant de commencer :
- [Vérifiez que les données d'identification d'infrastructure de l'utilisateur ne sont pas utilisées pour définir la clé d'API ou pour la commande `ibmcloud ks credential-set`](#removing).
- Si vous disposez d'autres instances de service dans votre compte {{site.data.keyword.Bluemix_notm}} que l'utilisateur a pu mettre à disposition, recherchez ces services dans la documentation pour obtenir les éventuelles étapes à suivre avant de retirer l'utilisateur du compte.

Avant le départ de l'utilisateur, le propriétaire du compte {{site.data.keyword.Bluemix_notm}} doit procéder comme suit pour empêcher toute modification avec rupture dans {{site.data.keyword.containerlong_notm}}.

1. Déterminez les clusters créés par l'utilisateur.
    1.  Connectez-vous à la [console {{site.data.keyword.containerlong_notm}} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/kubernetes/clusters).
    2.  Dans le tableau, sélectionnez votre cluster.
    3.  Dans l'onglet **Vue d'ensemble**, recherchez la zone **Propriétaire**.

2. Pour chaque cluster créé par l'utilisateur, procédez comme suit :
    1. Vérifiez quel est le compte de l'infrastructure que l'utilisateur a utilisé pour mettre à disposition le cluster.
        1.  Dans l'onglet **Noeuds worker**, sélectionnez un noeud worker et notez son **ID**.
        2.  Ouvrez le menu ![Icône de menu](../icons/icon_hamburger.svg "Icône de menu") et cliquez sur **Infrastructure classique**.
        3.  Dans le panneau de navigation de l'infrastructure, cliquez sur **Unités > Liste des unités**.
        4.  Recherchez l'ID du noeud worker que vous avez noté précédemment.
        5.  Si vous ne trouvez pas l'ID du noeud worker, c'est qu'il n'est pas mis à disposition dans ce compte d'infrastructure. Passez à un autre compte d'infrastructure et réessayez.
    2. Déterminez ce qu'il advient du compte de l'infrastructure utilisé par l'utilisateur pour mettre à disposition les clusters, une fois que cet utilisateur n'est plus là.
        * Si l'utilisateur n'est pas propriétaire du compte de l'infrastructure, les autres utilisateurs ont accès à ce compte et il est conservé après le départ de cet utilisateur. Vous pouvez continuer à travailler avec ces clusters dans votre compte. Vérifiez qu'il existe au mois un autre utilisateur doté du [rôle de plateforme **Administrateur**](#platform) pour les clusters.
        * Si l'utilisateur est propriétaire du compte de l'infrastructure, le compte de l'infrastructure est supprimé lors du départ de l'utilisateur. Vous ne pouvez plus continuer à travailler avec ces clusters. Pour éviter d'avoir des clusters orphelins, l'utilisateur doit supprimer les clusters avant son départ. Si l'utilisateur est parti et que les clusters n'ont pas été supprimés, vous devez utiliser la commande `ibmcloud ks credential-set` pour changer les données d'identification de l'infrastructure sur le compte dans lequel les noeuds worker du cluster sont mis à disposition et supprimer le cluster. Pour plus d'informations, voir [Impossible de modifier ou de supprimer des ressources d'infrastructure dans un cluster orphelin](/docs/containers?topic=containers-cs_troubleshoot_clusters#orphaned).

3. Retirez l'utilisateur du compte {{site.data.keyword.Bluemix_notm}}.
    1. Dans la barre de menu, sélectionnez **Gérer > Accès (IAM)**. Cliquez ensuite sur la page **Utilisateurs**.
    2. Cliquez sur le nom d'utilisateur correspondant à l'utilisateur.
    3. Dans l'entrée du tableau correspondant à l'utilisateur, cliquez sur le **menu Action** ![Icône de menu Action](../icons/action-menu-icon.svg "Icône de menu Action") **> Retirer l'utilisateur**. Lorsque vous retirez un utilisateur, les rôles de plateforme {{site.data.keyword.Bluemix_notm}} IAM, les rôles Cloud Foundry et les rôles de l'infrastructure IBM Cloud (SoftLayer) affectés à cet utilisateur sont automatiquement supprimés.

4.  Lorsque les droits de la plateforme {{site.data.keyword.Bluemix_notm}} IAM sont retirés, les droits de l'utilisateur sont également automatiquement retirés des rôles RBAC prédéfinis associés. Cependant, si vous avez créé des rôles de cluster ou des rôles RBAC personnalisés, [retirez l'utilisateur de ces liaisons de rôle RBAC ou de ces liaisons de rôle de cluster](#remove_custom_rbac).<p class="note">Le processus de retrait de droits {{site.data.keyword.Bluemix_notm}} IAM est asynchrone et peut prendre un certain temps.</p>


### Retrait de droits spécifiques
{: #remove_permissions}

Pour retirer des droits spécifiques à un utilisateur, vous pouvez retirer des règles d'accès individuelles qui ont été affectées à cet utilisateur.
{: shortdesc}

Avant de commencer, [vérifiez que les données d'identification d'infrastructure de l'utilisateur ne sont pas utilisées pour définir la clé d'API ou pour la commande `ibmcloud ks credential-set`](#removing). Vous pouvez ensuite retirer :
* [un utilisateur d'un groupe d'accès](#remove_access_group)
* [les droits de la plateforme {{site.data.keyword.Bluemix_notm}} IAM et les droits RBAC associés](#remove_iam_rbac) d'un utilisateur
* [les droits RBAC personnalisés d'un utilisateur](#remove_custom_rbac)
* [les droits Cloud Foundry d'un utilisateur](#remove_cloud_foundry)
* [les droits d'infrastructure d'un utilisateur](#remove_infra)

#### Retirer un utilisateur d'un groupe d'accès
{: #remove_access_group}

1. Connectez-vous à la [console {{site.data.keyword.Bluemix_notm}} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/). Dans la barre de menu, sélectionnez **Gérer > Accès (IAM)**.
2. Cliquez sur la page **Utilisateurs**, puis sur le nom de l'utilisateur auquel vous souhaitez retirer des droits.
3. Cliquez sur l'onglet **Groupe d'accès**.
4. Dans l'entrée du tableau correspondant au groupe d'accès, cliquez sur le **menu Actions** ![Icône de menu Action](../icons/action-menu-icon.svg "Icône de menu Action") **> Retirer l'utilisateur**. Lorsque l'utilisateur est retiré, tout rôle affecté au groupe d'accès est retiré de l'utilisateur.

#### Retirer des droits de plateforme {{site.data.keyword.Bluemix_notm}} IAM et les droits RBAC prédéfinis associés
{: #remove_iam_rbac}

1. Connectez-vous à la [console {{site.data.keyword.Bluemix_notm}}![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/). Dans la barre de menu, sélectionnez **Gérer > Accès (IAM)**.
2. Cliquez sur la page **Utilisateurs**, puis sur le nom de l'utilisateur auquel vous souhaitez retirer des droits.
3. Dans l'entrée du tableau correspondant à l'utilisateur, cliquez sur le **menu Actions** ![Icône de menu Action](../icons/action-menu-icon.svg "Icône de menu Action") **> Retirer l'utilisateur**.
5. Lorsque les droits de la plateforme {{site.data.keyword.Bluemix_notm}} IAM sont retirés, les droits de l'utilisateur sont également automatiquement retirés des rôles RBAC prédéfinis associés. Pour mettre à jour les rôles RBAC avec les modifications, exécutez la commande `ibmcloud ks cluster-config`. Cependant, si vous avez créé des [rôles RBAC personnalisés ou des rôles de cluster](#rbac), vous devez retirer l'utilisateur des fichiers `.yaml` correspondant à ces liaisons de rôle RBAC ou à ces liaisons de rôle de cluster. Voir la procédure ci-dessous pour retirer des droits RBAC.

#### Retirer des droits RBAC personnalisés
{: #remove_custom_rbac}

Si vous n'avez plus besoin des droits RBAC personnalisés, vous pouvez les retirer.
{: shortdesc}

1. Ouvrez le fichier `.yaml` correspondant à la liaison de rôle ou à la liaison de rôle de cluster que vous avez créée.
2. Dans la section `subjects`, retirez la section correspondant à l'utilisateur.
3. Sauvegardez le fichier.
4. Appliquez les modifications dans la ressource de liaison de rôle ou de liaison de rôle de cluster dans votre cluster.
    ```
    kubectl apply -f my_role_binding.yaml
    ```
    {: pre}

#### Retirer les droits Cloud Foundry
{: #remove_cloud_foundry}

Pour retirer tous les droits Cloud Foundry d'un utilisateur, vous pouvez retirer les rôles d'organisation de l'utilisateur. Si vous souhaitez simplement retirer la possibilité de lier des services dans un cluster à un utilisateur, retirez uniquement les rôles d'espace de l'utilisateur.
{: shortdesc}

1. Connectez-vous à la [console{{site.data.keyword.Bluemix_notm}}](https://cloud.ibm.com/). Dans la barre de menu, sélectionnez **Gérer > Accès (IAM)**.
2. Cliquez sur la page **Utilisateurs**, puis sur le nom de l'utilisateur auquel vous souhaitez retirer des droits.
3. Cliquez sur l'onglet **Accès Cloud Foundry**.
    * Pour retirer le rôle d'espace de l'utilisateur :
        1. Développez l'entrée du tableau correspondant à l'organisation dans laquelle se trouve l'espace.
        2. Dans l'entrée du tableau correspondant au rôle d'espace, cliquez dans le menu Actions et sélectionnez **Editer un rôle d'espace**.
        3. Supprimez un rôle en cliquant sur le bouton Fermer.
        4. Pour retirer tous les rôles d'espace, sélectionnez **Aucun rôle d'espace** dans la liste déroulante.
        5. Cliquez sur **Sauvegarder un rôle**.
    * Pour retirer le rôle d'organisation de l'utilisateur :
        1. Dans l'entrée du tableau correspondant au rôle d'organisation, cliquez sur le menu Actions et sélectionnez **Editer le rôle d'organisation**.
        3. Supprimez un rôle en cliquant sur le bouton Fermer.
        4. Pour retirer tous les rôles d'organisation, sélectionnez **Aucun rôle d'organisation** dans la liste déroulante.
        5. Cliquez sur **Sauvegarder un rôle**.

#### Retirer les droits de l'infrastructure IBM Cloud (SoftLayer)
{: #remove_infra}

Vous pouvez retirer des droits d'infrastructure IBM Cloud (SoftLayer) pour un utilisateur à l'aide de la console {{site.data.keyword.Bluemix_notm}}.
{: shortdesc}

1. Connectez-vous à la [console {{site.data.keyword.Bluemix_notm}}![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/). Dans la barre de menu, sélectionnez **Gérer > Accès (IAM)**.
2. Cliquez sur la page **Utilisateurs**, puis sur le nom de l'utilisateur auquel vous souhaitez retirer des droits.
4. Cliquez sur l'onglet **Infrastructure classique**, puis sur les onglets **Droits, Périphériques ou Sous-réseaux VPN**.
5. Dans chaque onglet, désélectionnez des droits spécifiques.
6. Pour sauvegarder vos modifications, cliquez sur **Définir** et sur **Sauvegarder**. Les droits sont rétromigrés au bout de quelques minutes.
