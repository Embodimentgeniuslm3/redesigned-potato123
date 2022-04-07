---

copyright:
  years: 2014, 2019
lastupdated: "2019-04-11"

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


# Initiation à {{site.data.keyword.containerlong_notm}}
{: #container_index}

Prenez une longueur d'avance avec {{site.data.keyword.containerlong}} en déployant des applications à haute disponibilité dans des conteneurs Docker qui s'exécutent dans des clusters Kubernetes.
{:shortdesc}

Les conteneurs sont un moyen standard pour conditionner des applications et toutes leurs dépendances afin de déplacer les applications d'un environnement à un autre en toute transparence. A la différence des machines virtuelles, les conteneurs n'incluent pas le système d'exploitation. Seuls le code d'application, l'environnement d'exécution, les outils système, les bibliothèques et les paramètres sont inclus dans les conteneurs. Les conteneurs sont plus légers, plus portables et plus efficaces que les machines virtuelles.

## Initiation aux clusters
{: #clusters_gs_index}

Vous avez l'intention de déployer une application dans un conteneur ? Pas si vite ! Commencez par créer un cluster Kubernetes. Kubernetes est un outil d'orchestration pour conteneurs. Kubernetes permet aux développeurs de déployer en un éclair des applications hautement disponibles en utilisant la puissance et la flexibilité des clusters.
{:shortdesc}

Qu'est-ce qu'un cluster ? Un cluster est un ensemble de ressources, de noeuds worker, de réseaux et de périphériques de stockage permettant d'assurer la haute disponibilité des applications. Dès que vous disposez d'un cluster, vous pouvez déployer vos applications dans des conteneurs.

Avant de commencer, procurez-vous le type de [compte {{site.data.keyword.Bluemix_notm}}](https://cloud.ibm.com/registration) qui vous convient :
* **Facturable (Paiement à la carte ou Abonnement)** : vous pouvez créer un cluster gratuit. Vous pouvez également mettre à disposition des ressources d'infrastructure IBM Cloud (SoftLayer) pour les créer et les utiliser dans des clusters standard.
* **Lite** : vous ne pouvez pas créer de cluster gratuit ou standard. [Effectuez la mise à niveau de votre compte](/docs/account?topic=account-accountfaqs#changeacct) pour passer à un compte facturable.

Pour créer un cluster gratuit :

1.  Dans le [**Catalogue** {{site.data.keyword.Bluemix_notm}} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/catalog?category=containers), sélectionnez **{{site.data.keyword.containershort_notm}}** et cliquez sur **Créer**. La page de configuration du cluster s'ouvre. Par défaut, l'option **Cluster gratuit** est sélectionnée.

2.  Attribuez un nom unique à votre cluster.

3.  Cliquez sur **Créer un cluster**. Un pool de noeuds worker contenant 1 noeud worker est créé. La mise à disposition du noeud worker peut prendre quelques minutes, et vous pouvez suivre la progression dans l'onglet **Noeuds worker**. Lorsque le statut passe à `Ready` vous pouvez commencer à travailler avec votre cluster.

<br>

Bien joué ! Vous avez créé votre premier cluster Kubernetes. Voici quelques détails sur votre cluster gratuit :

*   **Type de machine** : le cluster gratuit dispose d'un noeud worker virtuel intégré dans un pool de noeuds worker, avec 2 UC, 4 Go de mémoire et un disque SAN de 100 Go à la disposition de vos applications. Lorsque vous créez un cluster standard, vous pouvez choisir entre des machines physiques (bare metal) ou virtuelles, ainsi que diverses tailles de machine.
*   **Maître géré** : le noeud worker est surveillé et géré de manière centralisée par un maître Kubernetes appartenant à {{site.data.keyword.IBM_notm}}, dédié et à haute disponibilité, qui contrôle et surveille l'ensemble des ressources Kubernetes dans le cluster. Vous pouvez vous concentrer sur votre noeud worker et sur les applications qui y sont déployées sans avoir aussi à gérer ce maître.
*   **Ressources d'infrastructure** : les ressources nécessaires pour l'exécution du cluster, par exemple les réseaux locaux virtuels (VLAN) et les adresses IP, sont gérées dans un compte d'infrastructure IBM Cloud (SoftLayer) appartenant à {{site.data.keyword.IBM_notm}}. Lorsque vous créez un cluster standard, vous gérez ces ressources dans votre propre compte d'infrastructure IBM Cloud (SoftLayer). Vous pouvez en savoir plus sur ces ressources et les [droits nécessaires](/docs/containers?topic=containers-users#infra_access) lorsque vous créez un cluster standard.
*   **Autres options** : les clusters gratuits sont déployés au sein de la géographie que vous avez sélectionnée, mais vous ne pouvez pas sélectionner dans quelle zone. Pour pouvoir contrôler la zone, la mise en réseau et le stockage persistant, créez un cluster standard. [En savoir plus sur les avantages des clusters gratuits et standard](/docs/containers?topic=containers-cs_ov#cluster_types).

<br>

**Etape suivante ?**</br>
Expérimentez certaines fonctionnalités avec votre cluster gratuit avant qu'il n'arrive à expiration.

* Parcourez le [premier tutoriel {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-cs_cluster_tutorial#cs_cluster_tutorial) pour créer un cluster Kubernetes, installer l'interface de ligne de commande ou utiliser le terminal Kubernetes, créer un registre privé, configurer l'environnement de votre cluster et ajouter un service dans votre cluster.
* Continuez sur votre lancée en suivant le [deuxième tutoriel d'{{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-cs_apps_tutorial#cs_apps_tutorial) pour savoir comment déployer des applications dans votre cluster.
* [Créez un cluster standard](/docs/containers?topic=containers-clusters#clusters_ui) avec plusieurs noeuds pour obtenir une plus grande disponibilité.


