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



# Tutoriel : Création de clusters Kubernetes
{: #cs_cluster_tutorial}

Avec ce tutoriel, vous allez pouvoir déployer et gérer un cluster Kubernetes dans {{site.data.keyword.containerlong}}. Découvrez comment automatiser le déploiement, l'exploitation, la mise à l'échelle et la surveillance d'applications conteneurisées dans un cluster.
{:shortdesc}

Dans cette série de tutoriels, vous découvrirez comment une entreprise de relations publiques fictive utilise des fonctionnalités Kubernetes pour déployer une application conteneurisée dans {{site.data.keyword.Bluemix_notm}}. En tirant parti d'{{site.data.keyword.toneanalyzerfull}}, l'entreprise de RP analyse ses communiqués de presse et reçoit des commentaires en retour.


## Objectifs
{: #tutorials_objectives}

Dans ce premier tutoriel, vous endossez le rôle d'administrateur réseau de l'entreprise de relations publiques. Vous configurez un cluster Kubernetes personnalisé utilisé pour déployer et tester une version Hello World de l'application dans {{site.data.keyword.containerlong_notm}}.
{:shortdesc}

-   Créez un cluster avec 1 pool de noeuds worker contenant 1 noeud worker.
-   Installez les interfaces de ligne de commande (CLI) pour exécuter des [commandes Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubectl.docs.kubernetes.io/) et gérer des images Docker dans {{site.data.keyword.registrylong_notm}}.
-   Créez un référentiel d'images privé dans {{site.data.keyword.registrylong_notm}} pour y stocker vos images.
-   Ajoutez le service {{site.data.keyword.toneanalyzershort}} au cluster afin que n'importe quelle application dans le cluster puisse utiliser ce service.


## Durée
{: #tutorials_time}

40 minutes


## Public
{: #tutorials_audience}

Ce tutoriel est destiné aux développeurs de logiciel et aux administrateurs réseau qui créent un cluster Kubernetes pour la première fois.
{: shortdesc}

## Prérequis
{: #tutorials_prereqs}

-  Consultez la procédure à suivre pour [préparer la création du cluster](/docs/containers?topic=containers-clusters#cluster_prepare).
-  Vérifiez que vous disposez des règles d'accès suivantes :
    - [Rôle de plateforme {{site.data.keyword.Bluemix_notm}} IAM **Administrateur**](/docs/containers?topic=containers-users#platform) pour {{site.data.keyword.containerlong_notm}}
    - [Rôle de plateforme {{site.data.keyword.Bluemix_notm}} IAM **Administrateur**](/docs/containers?topic=containers-users#platform) pour {{site.data.keyword.registrylong_notm}}
    - [Rôle de service {{site.data.keyword.Bluemix_notm}} IAM **Auteur** ou **Responsable**](/docs/containers?topic=containers-users#platform) pour {{site.data.keyword.containerlong_notm}}


## Leçon 1 : Création d'un cluster et configuration de l'interface CLI
{: #cs_cluster_tutorial_lesson1}

Créez votre cluster Kubernetes dans la console {{site.data.keyword.Bluemix_notm}} et installez les interfaces CLI requises.
{: shortdesc}

**Création de votre cluster**

Comme la mise à disposition peut prendre quelques minutes, créez votre cluster avant d'installer les interfaces de ligne de commande.

1.  [Dans la console {{site.data.keyword.Bluemix_notm}} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/kubernetes/catalog/cluster/create), créez un cluster gratuit ou standard avec 1 pool worker comportant 1 noeud worker.

    Vous pouvez également créer un [cluster dans l'interface CLI](/docs/containers?topic=containers-clusters#clusters_cli_steps).
    {: tip}

Pendant la mise à disposition de votre cluster, installez les interfaces CLI suivantes qui sont utilisées pour gérer les clusters :
-   Interface CLI d'{{site.data.keyword.Bluemix_notm}}
-   Plug-in {{site.data.keyword.containerlong_notm}}
-   Interface CLI de Kubernetes
-   Plug-in {{site.data.keyword.registryshort_notm}}

Si vous souhaitez utiliser la console {{site.data.keyword.Bluemix_notm}} à la place, une fois votre cluster créé, vous pouvez exécuter des commandes CLI directement depuis votre navigateur Web dans le [terminal Kubernetes](/docs/containers?topic=containers-cs_cli_install#cli_web).
{: tip}

</br>
**Installation des interfaces de ligne de commandes et prérequis associés**

1. Installez l'[interface de ligne de commande {{site.data.keyword.Bluemix_notm}}![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](/docs/cli?topic=cloud-cli-getting-started). Cette installation inclut :
  - L'interface de ligne de commande {{site.data.keyword.Bluemix_notm}} de base. Le préfixe pour l'exécution de commandes via l'interface CLI d'{{site.data.keyword.Bluemix_notm}} est `ibmcloud`.
  - Le plug-in {{site.data.keyword.containerlong_notm}}. Le préfixe pour l'exécution de commandes via l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}} est `ibmcloud ks`.
  - Le plug-in {{site.data.keyword.registryshort_notm}}. Utilisez ce plug-in pour configurer et gérer un référentiel d'images privé dans {{site.data.keyword.registryshort_notm}}. Le préfixe pour l'exécution de commandes de registre est `ibmcloud cr`.

2. Connectez-vous à l'interface CLI d'{{site.data.keyword.Bluemix_notm}}. A l'invite, entrez vos données d'identification {{site.data.keyword.Bluemix_notm}}.
  ```
  ibmcloud login
  ```
  {: pre}

  Si vous disposez d'un ID fédéré, utilisez l'indicateur `--sso` pour vous connecter. Entrez votre nom d'utilisateur et utilisez l'URL mentionnée dans la sortie de l'interface de ligne de commande pour extraire votre code d'accès à usage unique.
  {: tip}

3. Suivez les invites pour sélectionner un compte.

5. Vérifiez que les plug-ins sont correctement installés.
  ```
  ibmcloud plugin list
  ```
  {: pre}

  Le plug-in {{site.data.keyword.containerlong_notm}} est affiché dans les résultats en tant que **container-service**, et le plug-in {{site.data.keyword.registryshort_notm}} est affiché dans les résultats en tant que **container-registry**.

6. Pour déployer des applications dans vos clusters, [installez l'interface CLI de Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/tasks/tools/install-kubectl/). Le préfixe pour l'exécution de commandes via l'interface CLI de Kubernetes est `kubectl`.

  1. Téléchargez la version `major.minor` de l'interface CLI Kubernetes qui correspond à la version `major.minor` que vous envisagez d'utiliser. La version Kubernetes par défaut en cours d'{{site.data.keyword.containerlong_notm}} est la version 1.13.6.
    - **OS X** : [https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/darwin/amd64/kubectl ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/darwin/amd64/kubectl)
    - **Linux** : [https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/linux/amd64/kubectl ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/linux/amd64/kubectl)
    - **Windows** : [https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/windows/amd64/kubectl.exe ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/windows/amd64/kubectl.exe)

  2. Si vous utilisez un système OS X ou Linux, procédez comme suit :

    1. Déplacez le fichier exécutable vers le répertoire `/usr/local/bin`.
      ```
      mv /filepath/kubectl /usr/local/bin/kubectl
      ```
      {: pre}

    2. Vérifiez que `/usr/local/bin` est listé dans votre variable système `PATH`. La variable `PATH` contient tous les répertoires où votre système d'exploitation peut trouver des fichiers exécutables. Les répertoires mentionnés dans la variable `PATH` ont des objets différents. `/usr/local/bin` stocke les fichiers exécutables de logiciels qui ne font pas partie du système d'exploitation et qui ont été installés manuellement par l'administrateur système.
      ```
      echo $PATH
      ```
      {: pre}
      Exemple de sortie d'interface CLI :
      ```
      /usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
      ```
      {: screen}

    3. Rendez le fichier exécutable.
      ```
      chmod +x /usr/local/bin/kubectl
      ```
      {: pre}

Bien joué ! Vous avez installé les interfaces CLI utilisées par la suite dans les leçons et les tutoriels. Configurez ensuite votre environnement de cluster et ajoutez le service {{site.data.keyword.toneanalyzershort}}.


## Leçon 2 : Configuration de votre registre privé
{: #cs_cluster_tutorial_lesson2}

Configurez un référentiel d'images privé dans {{site.data.keyword.registryshort_notm}} et ajoutez des valeurs confidentielles à votre cluster Kubernetes de sorte que l'application puisse accéder au service {{site.data.keyword.toneanalyzershort}}.
{: shortdesc}

1.  Si votre cluster se trouve dans un autre groupe de ressources que `default`, ciblez ce groupe de ressources. Pour voir à quel groupe de ressources appartient chaque cluster, exécutez la commande `ibmcloud ks clusters`.
   ```
   ibmcloud target -g <resource_group_name>
   ```
   {: pre}

2.  Configurez votre référentiel d'images privé dans {{site.data.keyword.registryshort_notm}} pour stocker de manière sécurisée et partager vos images Docker avec tous les utilisateurs du cluster. Un référentiel d'images privé dans {{site.data.keyword.Bluemix_notm}} est identifié par un espace de nom. L'espace de nom est utilisé pour créer une URL unique vers votre référentiel d'images que vos développeurs peuvent utiliser pour accéder aux images Docker privées.

    Découvrez comment [sécuriser vos informations personnelles](/docs/containers?topic=containers-security#pi) lorsque vous utilisez des images de conteneur.

    Dans cet exemple, l'entreprise de RP désire créer un seul registre d'images dans {{site.data.keyword.registryshort_notm}} et spécifie donc `pr_firm` comme nom d'espace dans lequel regrouper toutes les images de leur compte. Remplacez &lt;namespace&gt; par un espace de nom de votre choix sans rapport avec ce tutoriel.

    ```
    ibmcloud cr namespace-add <namespace>
    ```
    {: pre}

3.  Avant de passer à l'étape suivante, vérifiez que le déploiement de votre noeud worker a abouti.

    ```
    ibmcloud ks workers --cluster <cluster_name_or_ID>
    ```
    {: pre}

    Lorsque l'allocation de votre noeud worker a abouti, son statut passe à **Ready** et vous pouvez alors commencer à lier des services {{site.data.keyword.Bluemix_notm}}.

    ```
    ID                                                 Public IP       Private IP       Machine Type   State    Status   Zone   Version
    kube-mil01-pafe24f557f070463caf9e31ecf2d96625-w1   169.xx.xxx.xxx   10.xxx.xx.xxx   free           normal   Ready    mil01      1.13.6
    ```
    {: screen}

## Leçon 3 : Configuration de votre environnement de cluster
{: #cs_cluster_tutorial_lesson3}

Définissez le contexte de votre cluster Kubernetes dans l'interface CLI.
{: shortdesc}

Chaque fois que vous vous connectez à l'interface CLI d'{{site.data.keyword.containerlong}} pour gérer vos clusters, vous devez lancer ces commandes pour définir le chemin d'accès au fichier de configuration du cluster par le biais d'une variable de session. L'interface CLI de Kubernetes utilise cette variable pour localiser un fichier de configuration local et les certificats requis pour connexion au cluster dans {{site.data.keyword.Bluemix_notm}}.

1.  Obtenez la commande permettant de définir la variable d'environnement et téléchargez les fichiers de configuration Kubernetes.
    ```
    ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
    ```
    {: pre}

    Une fois les fichiers de configuration téléchargés, une commande s'affiche ; elle vous permet de définir le chemin vers le fichier de configuration Kubernetes local en tant que variable d'environnement.

    Exemple pour OS X :
    ```
    export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/pr_firm_cluster/kube-config-prod-par02-pr_firm_cluster.yml
    ```
    {: screen}
    Vous utilisez Windows PowerShell ? Ajoutez l'indicateur `--powershell` pour obtenir des variables d'environnement au format Windows PowerShell.
    {: tip}

2.  Copiez et collez la commande qui s'affiche sur votre terminal pour définir la variable d'environnement `KUBECONFIG`.

3.  Vérifiez que la variable d'environnement `KUBECONFIG` est correctement définie.

    Exemple pour OS X :

    ```
    echo $KUBECONFIG
    ```
    {: pre}

    Sortie :

    ```
    /Users/<user_name>/.bluemix/plugins/container-service/clusters/pr_firm_cluster/kube-config-prod-par02-pr_firm_cluster.yml
    ```
    {: screen}

4.  Vérifiez que les commandes `kubectl` fonctionnent correctement avec votre cluster en vérifiant la version du serveur CLI de Kubernetes.

    ```
    kubectl version  --short
    ```
    {: pre}

    Exemple de sortie :

    ```
    Client Version: v1.13.6
    Server Version: v1.13.6
    ```
    {: screen}

## Leçon 4 : Ajout d'un service à votre cluster
{: #cs_cluster_tutorial_lesson4}

Via les services {{site.data.keyword.Bluemix_notm}}, vous pouvez tirer parti dans vos applications des fonctionnalités que vous avez déjà développées. Tout service {{site.data.keyword.Bluemix_notm}} lié au cluster Kubernetes peut être utilisé par une application quelconque déployée dans ce cluster. Répétez les étapes ci-après pour chaque service {{site.data.keyword.Bluemix_notm}} que vous désirez utiliser avec vos applications.
{: shortdesc}

1.  Ajoutez le service {{site.data.keyword.toneanalyzershort}} à votre compte {{site.data.keyword.Bluemix_notm}}. Remplacez <service_name> par le nom de votre instance de service.

    Lorsque vous ajoutez le service {{site.data.keyword.toneanalyzershort}} à votre compte, un message s'affiche pour indiquer que ce service n'est pas gratuit. Si vous modérez votre appel d'API, ce tutoriel n'est pas assujetti à des frais pour le service {{site.data.keyword.watson}}. [Consultez les informations de tarification du service {{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/catalog/services/tone-analyzer).
    {: note}

    ```
    ibmcloud resource service-instance-create <service_name> tone-analyzer standard us-south
    ```
    {: pre}

2.  Associez l'instance {{site.data.keyword.toneanalyzershort}} à l'espace de nom Kubernetes `default` pour le cluster. Ensuite, vous pourrez créer vos propres espaces de nom pour gérer les accès utilisateur aux ressources Kubernetes, mais dans l'immédiat, utilisez l'espace de nom `default`. Les espaces de nom Kubernetes sont différents de l'espace de nom du registre que vous avez créé auparavant.

    ```
    ibmcloud ks cluster-service-bind --cluster <cluster_name> --namespace default --service <service_name>
    ```
    {: pre}

    Sortie :

    ```
    ibmcloud ks cluster-service-bind pr_firm_cluster default mytoneanalyzer
    Binding service instance to namespace...
    OK
    Namespace:	default
    Secret name:	binding-mytoneanalyzer
    ```
    {: screen}

3.  Vérifiez que la valeur confidentielle Kubernetes a bien été créée dans votre espace de nom de cluster. Tous les services {{site.data.keyword.Bluemix_notm}} sont définis par un fichier JSON qui comprend des informations confidentielles, telles que la clé d'API d'{{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM) et l'URL que le conteneur utilise pour obtenir l'accès. Des valeurs confidentielles (secrets) Kubernetes sont utilisées pour un stockage sécurisé de ces informations. Dans cet exemple, la valeur confidentielle contient la clé d'API permettant d'accéder à l'instance {{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}} mise à disposition dans votre compte.

    ```
    kubectl get secrets --namespace=default
    ```
    {: pre}

    Sortie :

    ```
    NAME                       TYPE                                  DATA      AGE
    binding-mytoneanalyzer     Opaque                                1         1m
    bluemix-default-secret     kubernetes.io/dockercfg               1         1h
    default-token-kf97z        kubernetes.io/service-account-token   3         1h
    ```
    {: screen}

</br>
Parfait ! Votre cluster est configuré et votre environnement local est prêt pour le déploiement d'applications dans le cluster.

## Etape suivante ?
{: #tutorials_next}

* Testez vos connaissances en [répondant à un quiz ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://ibmcloud-quizzes.mybluemix.net/containers/cluster_tutorial/quiz.php).
* Expérimentez le [tutoriel : Déploiement d'applications dans des clusters Kubernetes](/docs/containers?topic=containers-cs_apps_tutorial#cs_apps_tutorial) pour déployer l'application de l'entreprise de RP dans le cluster que vous avez créé.
