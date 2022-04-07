---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: kubernetes, iks, ibmcloud, ic, ks, kubectl

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



# Configuration de l'interface CLI et de l'API
{: #cs_cli_install}

Vous pouvez utiliser l'interface CLI ou l'API {{site.data.keyword.containerlong}} pour créer et gérer vos clusters Kubernetes.
{:shortdesc}

## Installation de l'interface CLI et des plug-in IBM Cloud
{: #cs_cli_install_steps}

Installez les interfaces CLI requises pour créer et gérer vos clusters Kubernetes dans {{site.data.keyword.containerlong_notm}} et pour déployer des applications conteneurisées dans votre cluster.
{:shortdesc}

Cette tâche inclut les informations relatives à l'installation de ces interfaces CLI et des plug-ins :

-   Interface CLI d'{{site.data.keyword.Bluemix_notm}}
-   Plug-in {{site.data.keyword.containerlong_notm}}
-   Plug-in {{site.data.keyword.registryshort_notm}}

Si vous souhaitez utiliser la console {{site.data.keyword.Bluemix_notm}} à la place, une fois votre cluster créé, vous pouvez exécuter des commandes CLI directement depuis votre navigateur Web dans le [terminal Kubernetes](#cli_web).
{: tip}

<br>
Pour installer les interfaces CLI, procédez comme suit :

1.  Installez l'[interface CLI {{site.data.keyword.Bluemix_notm}} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](/docs/cli?topic=cloud-cli-getting-started#idt-prereq). Cette installation inclut :
    -   L'interface CLI {{site.data.keyword.Bluemix_notm}} de base (`ibmcloud`).
    -   Le plug-in {{site.data.keyword.containerlong_notm}} (`ibmcloud ks`).
    -   Le plug-in {{site.data.keyword.registryshort_notm}} (`ibmcloud cr`). Utilisez ce plug-in pour mettre en place votre propre espace de nom dans un registre d'images privé à service partagé, haute disponibilité et évolutif, hébergé par IBM, et pour stocker et partager des images Docker avec d'autres utilisateurs. Les images Docker sont requises pour déployer des conteneurs dans un cluster.
    -   L'interface CLI Kubernetes (`kubectl`) correspondant à la version par défaut : 1.13.6.<p class="note">Si vous prévoyez d'utiliser un cluster qui exécute une autre version, vous devrez peut-être [installer cette version de l'interface CLI Kubernetes séparément](#kubectl). Si vous disposez d'un cluster (OpenShift), vous [installez les interfaces CLI `oc` et `kubectl` en même temps](#cli_oc).</p>
    -   L'interface CLI Helm (`helm`). Vous pouvez être amené à utiliser Helm en tant que gestionnaire de package pour installer des services {{site.data.keyword.Bluemix_notm}} et des applications complexes sur votre cluster via des chartes Helm. Vous devez tout de même [configurer Helm](/docs/containers?topic=containers-helm) dans chaque cluster où vous souhaitez utiliser Helm.

    Vous prévoyez une utilisation intensive de l'interface CLI ? Essayez d'[activer la fonction d'autocomplétion de l'interpréteur de commandes pour l'interface CLI d'{{site.data.keyword.Bluemix_notm}} (Linux/MacOS uniquement)](/docs/cli/reference/ibmcloud?topic=cloud-cli-shell-autocomplete#shell-autocomplete-linux).
    {: tip}

2.  Connectez-vous à l'interface CLI d'{{site.data.keyword.Bluemix_notm}}. A l'invite, entrez vos données d'identification {{site.data.keyword.Bluemix_notm}}.
    ```
    ibmcloud login
    ```
    {: pre}

    Si vous disposez d'un ID fédéré, utilisez `ibmcloud login --sso` pour vous connecter à l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}}. Entrez votre nom d'utilisateur et utilisez l'URL mentionnée dans la sortie de l'interface de ligne de commande pour extraire votre code d'accès à usage unique. Si la connexion échoue alors que vous omettez l'option `--sso` et aboutit en incluant l'option `--sso`, ceci indique que votre ID est fédéré.
    {: tip}

3.  vérifiez que le plug-in {{site.data.keyword.containerlong_notm}} et les plug-in {{site.data.keyword.registryshort_notm}} sont correctement installés.
    ```
    ibmcloud plugin list
    ```
    {: pre}

    Exemple de sortie :
    ```
    Listing installed plug-ins...

    Plugin Name                            Version   Status        
    container-registry                     0.1.373     
    container-service/kubernetes-service   0.3.23   
    ```
    {: screen}

Pour des informations de référence sur ces interfaces de ligne de commande, reportez-vous à la documentation relative à ces outils.

-   [Commandes `ibmcloud`](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_cli#ibmcloud_cli)
-   [Commandes `ibmcloud ks`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli)
-   [Commandes `ibmcloud cr`](/docs/services/Registry?topic=container-registry-cli-plugin-containerregcli)

## Installation de l'interface CLI Kubernetes (`kubectl`)
{: #kubectl}

Pour afficher une version locale du tableau de bord Kubernetes et déployer des applications dans vos clusters, installez l'interface CLI Kubernetes (`kubectl`). La dernière version stable de `kubectl` est installée avec l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}} de base. Toutefois, pour utiliser votre cluster, vous devez installer à la place la version `major.minor` de l'interface de ligne de commande Kubernetes qui correspond à la version `major.minor` du cluster Kubernetes que vous prévoyez d'utiliser. Si vous utilisez une version d'interface CLI `kubectl` qui ne correspond pas au moins à la version principale et secondaire (`major.minor`) de vos clusters, vous risquez d'obtenir des résultats inattendus. Par exemple, [Kubernetes ne prend pas en charge ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/setup/version-skew-policy/) les versions client de `kubectl` qui diffèrent d'au moins 2 niveaux par rapport à la version du serveur (n +/- 2). Assurez-vous de maintenir votre cluster Kubernetes et les versions de l'interface CLI à jour.
{: shortdesc}

Vous utilisez un cluster OpenShift ? Installez à la place l'interface CLI OpenShift Origin (`oc`) qui est fournie avec `kubectl`. Si vous disposez à la fois de clusters Red Hat OpenShift on IBM Cloud et de clusters {{site.data.keyword.containershort_notm}} natifs sur Ubuntu, prenez soin d'utiliser le fichier binaire `kubectl` qui correspond à la version Kubernetes `major.minor` de votre cluster.
{: tip}

1.  Si vous disposez déjà d'un cluster, vérifiez que la version de l'interface CLI `kubectl` de votre client correspond à celle du serveur d'API de cluster. 
    1.  [Connectez-vous à votre compte. Le cas échéant, ciblez le groupe de ressources approprié. Définissez le contexte pour votre cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
    2.  Comparez les versions du client et du serveur. Si la version du client et celle du serveur ne correspondent pas, passez à l'étape suivante. Si les versions correspondent, cela signifie que vous avez déjà installé la version appropriée de `kubectl`.
        ```
        kubectl version  --short
        ```
        {: pre}
2.  Téléchargez la version `major.minor` de l'interface CLI Kubernetes qui correspond à la version `major.minor` que vous envisagez d'utiliser. La version Kubernetes par défaut en cours d'{{site.data.keyword.containerlong_notm}} est la version 1.13.6.
    -   **OS X** : [https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/darwin/amd64/kubectl ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/darwin/amd64/kubectl)
    -   **Linux** : [https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/linux/amd64/kubectl ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/linux/amd64/kubectl)
    -   **Windows** : Installez l'interface de ligne de commande Kubernetes dans le même répertoire que l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}}. Cette configuration vous évite diverses modifications de chemin de fichier lorsque vous exécutez des commandes par la suite. [https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/windows/amd64/kubectl.exe ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/windows/amd64/kubectl.exe)

3.  Si vous utilisez OS X ou Linux, procédez comme suit :
    1.  Déplacez le fichier exécutable vers le répertoire `/usr/local/bin`.
        ```
        mv /<filepath>/kubectl /usr/local/bin/kubectl
        ```
        {: pre}

    2.  Vérifiez que `/usr/local/bin` est listé dans votre variable système `PATH`. La variable `PATH` contient tous les répertoires où votre système d'exploitation peut trouver des fichiers exécutables. Les répertoires mentionnés dans la variable `PATH` ont des objets différents. `/usr/local/bin` stocke les fichiers exécutables de logiciels qui ne font pas partie du système d'exploitation et qui ont été installés manuellement par l'administrateur système.
        ```
        echo $PATH
        ```
        {: pre}
        Exemple de sortie d'interface CLI :
        ```
        /usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
        ```
        {: screen}

    3.  Rendez le fichier exécutable.
        ```
        chmod +x /usr/local/bin/kubectl
        ```
        {: pre}
4.  **Facultatif** : [Activez le remplissage automatique pour les commandes `kubectl` ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/tasks/tools/install-kubectl/#enabling-shell-autocompletion). Les étapes varient en fonction du shell que vous utilisez. 

Ensuite, passez à l'étape [Création de clusters Kubernetes depuis l'interface CLI d'{{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-clusters#clusters_cli_steps).

Pour plus d'informations sur l'interface CLI Kubernetes, voir la [documentation de référence `kubectl` ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubectl.docs.kubernetes.io/).
{: note}

<br />


## Installation de la version bêta de l'interface CLI OpenShift Origin (`oc`)
{: #cli_oc}

[Red Hat OpenShift on IBM Cloud](/docs/containers?topic=containers-openshift_tutorial) est disponible en version bêta afin de pouvoir tester les clusters OpenShift.
{: preview}

Pour afficher une version locale du tableau de bord OpenShift et déployer des applications dans vos clusters Red Hat OpenShift on IBM Cloud, installez l'interface CLI OpenShift Origin (`oc`). L'interface CLI `oc` inclut une version correspondante de l'interface CLI Kubernetes (`kubectl`). Pour plus d'informations, voir la [documentation OpenShift ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://docs.openshift.com/container-platform/3.11/cli_reference/get_started_cli.html).
{: shortdesc}

Vous utilisez à la fois des clusters Red Hat OpenShift on IBM Cloud et des clusters {{site.data.keyword.containershort_notm}} natifs sur Ubuntu ? L'interface CLI `oc` est fournie avec les fichiers binaires `oc` et `kubectl`, mais vos différents clusters peuvent exécuter différentes versions de Kubernetes, par exemple, la version 1.11 sur OpenShift et la version 1.13.6 sur Ubuntu. Prenez soin d'utiliser le fichier binaire pour `kubectl` qui correspond à la version Kubernetes `major.minor` de votre cluster.
{: note}

1.  [Téléchargez l'interface CLI OpenShift Origin ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.okd.io/download.html) pour votre version de système d'exploitation local et OpenShift. La version OpenShift par défaut en cours est 3.11.

2.  Si vous utilisez Mac OS ou Linux, procédez comme suit pour ajouter les fichiers binaires à votre variable système `PATH`. Si vous utilisez Windows, installez l'interface CLI `oc` dans le même répertoire que l'interface CLI {{site.data.keyword.Bluemix_notm}}. Cette configuration vous évite diverses modifications de chemin de fichier lorsque vous exécutez des commandes par la suite.
    1.  Déplacez les fichiers exécutables `oc` et `kubectl` vers le répertoire `/usr/local/bin`.
        ```
        mv /<filepath>/oc /usr/local/bin/oc && mv /<filepath>/kubectl /usr/local/bin/kubectl
        ```
        {: pre}

    2.  Vérifiez que `/usr/local/bin` est listé dans votre variable système `PATH`. La variable `PATH` contient tous les répertoires où votre système d'exploitation peut trouver des fichiers exécutables. Les répertoires mentionnés dans la variable `PATH` ont des objets différents. `/usr/local/bin` stocke les fichiers exécutables de logiciels qui ne font pas partie du système d'exploitation et qui ont été installés manuellement par l'administrateur système.
        ```
        echo $PATH
        ```
        {: pre}
        Exemple de sortie d'interface CLI :
        ```
        /usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
        ```
        {: screen}
3.  **Facultatif** : [Activez le remplissage automatique pour les commandes `kubectl` ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/tasks/tools/install-kubectl/#enabling-shell-autocompletion). Les étapes varient en fonction du shell que vous utilisez. Vous pouvez répéter les étapes pour activer le remplissage automatique pour les commandes `oc`. Par exemple, dans bash sous Linux, au lieu de la commande `kubectl completion bash >/etc/bash_completion.d/kubectl`, vous pouvez exécuter `oc completion bash >/etc/bash_completion.d/oc_completion`.

Ensuite, démarrez la procédure [Création d'un cluster Red Hat OpenShift on IBM Cloud (bêta)](/docs/containers?topic=containers-openshift_tutorial).

Pour plus d'informations sur l'interface CLI OpenShift Origin, voir la [documentation `oc` ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://docs.openshift.com/container-platform/3.11/cli_reference/basic_cli_operations.html).
{: note}

<br />


## Exécution de l'interface de ligne de commande dans un conteneur sur votre ordinateur
{: #cs_cli_container}

Au lieu d'installer chacune des interfaces de ligne de commande individuellement sur votre ordinateur, vous pouvez les installer dans un conteneur qui s'exécute sur votre ordinateur.
{:shortdesc}

Avant de commencer, [installez Docker ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.docker.com/community-edition#/download) pour générer et exécuter des images en local. Si vous utilisez Windows 8 ou version antérieure, vous pouvez installer à la place [Docker Toolbox ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://docs.docker.com/toolbox/toolbox_install_windows/).

1. Créez une image à partir du fichier Dockerfile fourni.

    ```
    docker build -t <image_name> https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/install-clis-container/Dockerfile
    ```
    {: pre}

2. Déployez l'image localement en tant que conteneur et montez un volume pour accéder aux fichiers locaux.

    ```
    docker run -it -v /local/path:/container/volume <image_name>
    ```
    {: pre}

3. Commencez par exécuter les commandes `ibmcloud ks` et `kubectl` à partir de l'interpréteur de commandes interactif. Si vous créez des données que vous voulez sauvegarder, enregistrez-les sur le volume que vous avez monté. Lorsque vous quittez l'interpréteur de commandes, le conteneur s'arrête.

<br />



## Configuration de l'interface CLI pour l'exécution de commandes `kubectl`
{: #cs_cli_configure}

Vous pouvez utiliser les commandes fournies avec l'interface de ligne de commande Kubernetes pour gérer les clusters dans {{site.data.keyword.Bluemix_notm}}.
{:shortdesc}

Toutes les commandes `kubectl` disponibles dans Kubernetes 1.13.6 sont prises en charge pour être utilisées avec les clusters dans {{site.data.keyword.Bluemix_notm}}. Après avoir créé un cluster, définissez le contexte de votre interface de ligne de commande locale vers ce cluster à l'aide d'une variable d'environnement. Vous pouvez ensuite exécuter les commandes Kubernetes `kubectl` pour utiliser votre cluster dans {{site.data.keyword.Bluemix_notm}}.

Avant de pouvoir exécuter des commandes `kubectl` :
* [Installez les interfaces de ligne de commande (CLI) requises](#cs_cli_install).
* [Créez un cluster](/docs/containers?topic=containers-clusters#clusters_cli_steps). 
* Vérifiez que vous disposez d'un [rôle de service](/docs/containers?topic=containers-users#platform) qui vous octroie le rôle Kubernetes RBAC approprié pour travailler avec des ressources Kubernetes. Si vous disposez uniquement d'un rôle de service mais pas d'un rôle de plateforme, l'administrateur de cluster doit vous fournir l'ID et le nom du cluster ou vous affecter le rôle de plateforme **Afficheur** pour visualiser les clusters.

Pour utiliser des commandes `kubectl` :

1.  Connectez-vous à l'interface CLI d'{{site.data.keyword.Bluemix_notm}}. A l'invite, entrez vos données d'identification {{site.data.keyword.Bluemix_notm}}.

    ```
    ibmcloud login
    ```
    {: pre}

    Si vous disposez d'un ID fédéré, utilisez `ibmcloud login --sso` pour vous connecter à l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}}. Entrez votre nom d'utilisateur et utilisez l'URL mentionnée dans la sortie de l'interface de ligne de commande pour extraire votre code d'accès à usage unique. Si la connexion échoue alors que vous omettez l'option `--sso` et aboutit en incluant l'option `--sso`, ceci indique que votre ID est fédéré.
    {: tip}

2.  Sélectionnez un compte {{site.data.keyword.Bluemix_notm}}. Si vous êtes affecté à plusieurs organisations {{site.data.keyword.Bluemix_notm}}, sélectionnez celle dans laquelle le cluster a été créé. Les clusters sont spécifiques à une organisation, mais sont indépendants d'un espace {{site.data.keyword.Bluemix_notm}}. Vous n'avez donc pas besoin de sélectionner un espace.

3.  Pour créer et utiliser des clusters dans un autre groupe de ressources que le groupe par défaut, ciblez ce groupe de ressources. Pour voir à quel groupe de ressources appartient chaque cluster, exécutez la commande `ibmcloud ks clusters`. **Remarque** : vous devez disposer de l'[accès **Afficheur**](/docs/containers?topic=containers-users#platform) au groupe de ressources.
    ```
    ibmcloud target -g <resource_group_name>
    ```
    {: pre}

4.  Répertoriez tous les clusters du compte pour obtenir le nom du cluster. Si vous disposez uniquement d'un rôle d'accès au service {{site.data.keyword.Bluemix_notm}} IAM et que vous ne pouvez pas afficher les clusters, demandez à l'administrateur de cluster de vous affecter le rôle de plateforme IAM **Afficheur**, ou de vous fournir le nom et l'ID du cluster.

    ```
    ibmcloud ks clusters
    ```
    {: pre}

5.  Définissez le cluster que vous avez créé comme contexte de cette session. Effectuez ces étapes de configuration à chaque fois que vous utilisez votre cluster.
    1.  Obtenez la commande permettant de définir la variable d'environnement et téléchargez les fichiers de configuration Kubernetes. <p class="tip">Vous utilisez Windows PowerShell ? Ajoutez l'indicateur `--powershell` pour obtenir des variables d'environnement au format Windows PowerShell.
    </p>
        ```
        ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
        ```
        {: pre}

        Une fois les fichiers de configuration téléchargés, une commande s'affiche ; elle vous permet de définir le chemin vers le fichier de configuration Kubernetes local en tant que variable d'environnement.

        Exemple :

        ```
        export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/mycluster/kube-config-prod-dal10-mycluster.yml
        ```
        {: screen}

    2.  Copiez et collez la commande qui s'affiche sur votre terminal pour définir la variable d'environnement `KUBECONFIG`.

        **Utilisateurs Mac ou Linux** : au lieu d'exécuter la commande `ibmcloud ks cluster-config` et de copier la variable d'environnement `KUBECONFIG`, vous pouvez exécuter la commande `ibmcloud ks cluster-config --export <cluster-name>`. Selon votre interpréteur de commandes, vous pouvez configurer votre shell en exécutant la commande `eval $(ibmcloud ks cluster-config --export <cluster-name>)`.
        {: tip}

    3.  Vérifiez que la variable d'environnement `KUBECONFIG` est correctement définie.

        Exemple :

        ```
        echo $KUBECONFIG
        ```
        {: pre}

        Sortie :
        ```
        /Users/<user_name>/.bluemix/plugins/container-service/clusters/mycluster/kube-config-prod-dal10-mycluster.yml
        ```
        {: screen}

6.  Vérifiez que les commandes `kubectl` fonctionnent correctement avec votre cluster en identifiant la version du serveur CLI de Kubernetes.

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

Vous pouvez à présent exécuter des commandes `kubectl` pour gérer vos clusters dans {{site.data.keyword.Bluemix_notm}}. Pour obtenir la liste complète des commandes, voir la [documentation Kubernetes![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubectl.docs.kubernetes.io/).

**Astuce :** si vous utilisez Windows et que l'interface CLI de Kubernetes n'est pas installée dans le même répertoire que l'interface CLI d'{{site.data.keyword.Bluemix_notm}}, vous devez basculer entre les répertoires en spécifiant le chemin d'accès de l'installation de l'interface CLI de Kubernetes pour que l'exécution de `kubectl` puisse aboutir.


<br />




## Mise à jour de l'interface de ligne de commande
{: #cs_cli_upgrade}

Vous pouvez mettre régulièrement à jour les interfaces de ligne de commande afin d'utiliser les nouvelles fonctions.
{:shortdesc}

Cette tâche inclut les informations pour la mise à jour de ces interfaces CLI.

-   Interface CLI d'{{site.data.keyword.Bluemix_notm}} version 0.8.0 ou ultérieure
-   Plug-in {{site.data.keyword.containerlong_notm}}
-   Interface CLI de Kubernetes version 1.13.6 ou ultérieure
-   Plug-in {{site.data.keyword.registryshort_notm}}

<br>
Pour mettre à jour les interfaces de ligne de commande, procédez comme suit :

1.  Mettez à jour l'interface de ligne de commande d'{{site.data.keyword.Bluemix_notm}}. Téléchargez la [version la plus récente ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](/docs/cli?topic=cloud-cli-getting-started) et exécutez le programme d'installation.

2. Connectez-vous à l'interface CLI d'{{site.data.keyword.Bluemix_notm}}. A l'invite, entrez vos données d'identification {{site.data.keyword.Bluemix_notm}}.

    ```
    ibmcloud login
    ```
    {: pre}

     Si vous disposez d'un ID fédéré, utilisez `ibmcloud login --sso` pour vous connecter à l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}}. Entrez votre nom d'utilisateur et utilisez l'URL mentionnée dans la sortie de l'interface de ligne de commande pour extraire votre code d'accès à usage unique. Si la connexion échoue alors que vous omettez l'option `--sso` et aboutit en incluant l'option `--sso`, ceci indique que votre ID est fédéré.
     {: tip}

3.  Mettez à jour le plug-in {{site.data.keyword.containerlong_notm}}.
    1.  Installez la mise à jour à partir du référentiel de plug-in {{site.data.keyword.Bluemix_notm}}.

        ```
        ibmcloud plugin update container-service 
        ```
        {: pre}

    2.  Vérifiez l'installation du plug-in en exécutant la commande suivante et en vérifiant la liste des plug-ins installés.

        ```
        ibmcloud plugin list
        ```
        {: pre}

        Le plug-in {{site.data.keyword.containerlong_notm}} est affiché dans les résultats en tant que container-service (service de conteneur).

    3.  Initialisez l'interface CLI.

        ```
        ibmcloud ks init
        ```
        {: pre}

4.  [Mettez à jour l'interface CLI Kubernetes](#kubectl).

5.  Mettez à jour le plug-in {{site.data.keyword.registryshort_notm}}.
    1.  Installez la mise à jour à partir du référentiel de plug-in {{site.data.keyword.Bluemix_notm}}.

        ```
        ibmcloud plugin update container-registry 
        ```
        {: pre}

    2.  Vérifiez l'installation du plug-in en exécutant la commande suivante et en vérifiant la liste des plug-ins installés.

        ```
        ibmcloud plugin list
        ```
        {: pre}

        Le plug-in du registre s'affiche dans les résultats sous la forme container-registry.

<br />


## Désinstallation de l'interface de ligne de commande
{: #cs_cli_uninstall}

Si vous n'avez plus besoin de l'interface CLI, vous pouvez la désinstaller.
{:shortdesc}

Cette tâche inclut les informations relatives au retrait de ces interfaces CLI :


-   Plug-in {{site.data.keyword.containerlong_notm}}
-   Interface CLI de Kubernetes
-   Plug-in {{site.data.keyword.registryshort_notm}}

Pour désinstaller les interfaces CLI, procédez comme suit :

1.  Désinstallez le plug-in
{{site.data.keyword.containerlong_notm}}.

    ```
    ibmcloud plugin uninstall container-service
    ```
    {: pre}

2.  Désinstallez le plug-in
{{site.data.keyword.registryshort_notm}}.

    ```
    ibmcloud plugin uninstall container-registry
    ```
    {: pre}

3.  Vérifiez que les plug-ins ont bien été désinstallés en exécutant la commande suivante et en examinant la liste des plug-ins installés.

    ```
    ibmcloud plugin list
    ```
    {: pre}

    Les plug-ins container-service et container-registry ne sont pas affichés dans les résultats.

<br />


## Utilisation du terminal Kubernetes dans votre navigateur Web (bêta)
{: #cli_web}

Le terminal Kubernetes vous permet d'utiliser l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}} pour gérer votre cluster directement à partir de votre navigateur Web.
{: shortdesc}

Le terminal Kubernetes est publié en tant que module complémentaire {{site.data.keyword.containerlong_notm}} bêta. Il est susceptible d'être modifié en raison des commentaires en retour de la part des utilisateurs et de tests qui seront effectués ultérieurement. N'utilisez pas cette fonction dans des clusters de production pour éviter tout effet secondaire inattendu.
{: important}

Si vous utilisez le tableau de bord de cluster dans la console {{site.data.keyword.Bluemix_notm}} pour gérer vos clusters mais que vous souhaitez effectuer rapidement des modifications de configuration plus évoluées, vous pouvez désormais exécuter des commandes CLI directement à partir de votre navigateur Web dans le terminal Kubernetes. Le terminal Kubernetes est activé avec l'[interface CLI {{site.data.keyword.Bluemix_notm}} de base ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](/docs/cli?topic=cloud-cli-getting-started), le plug-in {{site.data.keyword.containerlong_notm}} et le plug-in {{site.data.keyword.registryshort_notm}}. De plus, le contexte de terminal est déjà défini sur le cluster que vous gérez, par conséquent, vous pouvez exécuter des commandes `kubectl` kubernetes pour utiliser votre cluster.

Tous les fichiers que vous téléchargez et éditez localement, par exemple, des fichiers YAML, sont stockés temporairement dans le terminal Kubernetes et ne sont pa conservés d'une session à l'autre.
{: note}

Pour installer et lancer le terminal Kubernetes :

1.  Connectez-vous à la [console{{site.data.keyword.Bluemix_notm}}](https://cloud.ibm.com/).
2.  Dans la barre de menu, sélectionnez le compte que vous souhaitez utiliser.
3.  Dans le menu ![Icône de menu](../icons/icon_hamburger.svg "Icône de menu"), cliquez sur **Kubernetes**.
4.  Sur la page **Clusters**, cliquez sur le cluster auquel vous souhaitez accéder.
5.  Sur la page des détails du cluster, cliquez sur le bouton **Terminal**.
6.  Cliquez sur **Installer**. L'installation du module complémentaire de terminal peut prendre quelques minutes.
7.  Cliquez à nouveau sur le bouton **Terminal**. Le terminal s'ouvre dans votre navigateur.

La prochaine fois, vous pourrez lancer le terminal Kubernetes simplement en cliquant sur le bouton **Terminal**.

<br />


## Automatisation des déploiements de cluster à l'aide de l'API
{: #cs_api}

Vous pouvez utiliser l'API {{site.data.keyword.containerlong_notm}} pour automatiser la création, le déploiement et la gestion de vos clusters Kubernetes.
{:shortdesc}

L'API {{site.data.keyword.containerlong_notm}} requiert de fournir des informations d'en-tête dans votre demande d'API, lesquelles peuvent varier selon l'API à utiliser. Pour déterminer les informations d'en-tête nécessaires à votre API, voir la [documentation sur l'API {{site.data.keyword.containerlong_notm}} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://us-south.containers.cloud.ibm.com/swagger-api).

Pour s'authentifier auprès d'{{site.data.keyword.containerlong_notm}}, vous devez fournir un jeton {{site.data.keyword.Bluemix_notm}} IAM (Identity and Access Management) qui est généré avec vos données d'identification {{site.data.keyword.Bluemix_notm}} et qui comprend l'ID du compte {{site.data.keyword.Bluemix_notm}} dans lequel a été créé le cluster. Selon votre mode d'authentification auprès d'{{site.data.keyword.Bluemix_notm}}, vous pouvez choisir parmi les options suivantes pour automatiser la création de votre jeton {{site.data.keyword.Bluemix_notm}} IAM.

Vous pouvez également utiliser le [fichier JSON Swagger de l'API ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://containers.cloud.ibm.com/global/swagger-global-api/swagger.json) pour générer un client pouvant interagir avec l'API dans le cadre de votre travail d'automatisation.
{: tip}

<table summary="Types d'ID et options avec le paramètre d'entrée dans la colonne 1 et la valeur dans la colonne 2.">
<caption>Types d'ID et options</caption>
<thead>
<th>ID {{site.data.keyword.Bluemix_notm}}</th>
<th>Mes options</th>
</thead>
<tbody>
<tr>
<td>ID non fédéré</td>
<td><ul><li><strong>Générez une clé d'API {{site.data.keyword.Bluemix_notm}} :</strong> en guise d'alternative à l'utilisation du nom d'utilisateur et du mot de passe {{site.data.keyword.Bluemix_notm}}, vous pouvez <a href="/docs/iam?topic=iam-userapikey#create_user_key" target="_blank">utiliser des clés d'API {{site.data.keyword.Bluemix_notm}}</a>. Les clés d'API {{site.data.keyword.Bluemix_notm}} dépendent du compte {{site.data.keyword.Bluemix_notm}} pour lequel elles sont générées. Vous ne pouvez pas combiner votre clé d'API {{site.data.keyword.Bluemix_notm}} avec un autre ID de compte dans le même jeton {{site.data.keyword.Bluemix_notm}} IAM. Pour accéder aux clusters créés avec un compte autre que celui sur lequel votre clé d'API {{site.data.keyword.Bluemix_notm}} est basée, vous devez vous connecter au compte pour générer une nouvelle clé d'API.</li>
<li><strong>Nom d'utilisateur et mot de passe {{site.data.keyword.Bluemix_notm}} :</strong> vous pouvez suivre la procédure indiquée dans cette rubrique pour automatiser complètement la création de votre jeton d'accès {{site.data.keyword.Bluemix_notm}} IAM.</li></ul>
</tr>
<tr>
<td>ID fédéré</td>
<td><ul><li><strong>Générez une clé d'API {{site.data.keyword.Bluemix_notm}} :</strong> <a href="/docs/iam?topic=iam-userapikey#create_user_key" target="_blank">les clés d'API {{site.data.keyword.Bluemix_notm}}</a> dépendent du compte {{site.data.keyword.Bluemix_notm}} pour lequel elles sont générées. Vous ne pouvez pas combiner votre clé d'API {{site.data.keyword.Bluemix_notm}} avec un autre ID de compte dans le même jeton {{site.data.keyword.Bluemix_notm}} IAM. Pour accéder aux clusters créés avec un compte autre que celui sur lequel votre clé d'API {{site.data.keyword.Bluemix_notm}} est basée, vous devez vous connecter au compte pour générer une nouvelle clé d'API.</li>
<li><strong>Utilisez un code d'accès à usage unique : </strong> si vous vous authentifiez auprès d'{{site.data.keyword.Bluemix_notm}} en utilisant un code d'accès à usage unique, vous ne pouvez pas automatiser complètement la création de votre jeton {{site.data.keyword.Bluemix_notm}} IAM car l'extraction de ce code d'accès nécessite une interaction manuelle avec votre navigateur Web. Pour automatiser complètement la création de votre jeton {{site.data.keyword.Bluemix_notm}} IAM, vous devez créer une clé d'API {{site.data.keyword.Bluemix_notm}} à la place.</ul></td>
</tr>
</tbody>
</table>

1.  Créez votre jeton d'accès {{site.data.keyword.Bluemix_notm}} IAM. Les informations de corps contenues dans votre demande varient en fonction de la méthode d'authentification {{site.data.keyword.Bluemix_notm}} que vous utilisez.

    ```
    POST https://iam.bluemix.net/identity/token
    ```
    {: codeblock}

    <table summary="Paramètres d'entrée permettant d'extraire des jetons IAM, avec le paramètre d'entrée dans la colonne 1 et la valeur dans la colonne 2.">
    <caption>Paramètres d'entrée permettant d'obtenir des jetons IAM</caption>
    <thead>
        <th>Paramètres d'entrée</th>
        <th>Valeurs</th>
    </thead>
    <tbody>
    <tr>
    <td>En-tête</td>
    <td><ul><li>Content-Type: application/x-www-form-urlencoded</li> <li>Authorization: Basic Yng6Yng=<p><strong>Remarque</strong> : <code>Yng6Yng=</code> est égal à l'autorisation codée dans l'URL correspondant au nom d'utilisateur <strong>bx</strong> et au mot de passe <strong>bx</strong>.</p></li></ul>
    </td>
    </tr>
    <tr>
    <td>Corps pour le nom d'utilisateur et le mot de passe {{site.data.keyword.Bluemix_notm}}</td>
    <td><ul><li>`grant_type: password`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`username` : votre nom d'utilisateur {{site.data.keyword.Bluemix_notm}}.</li>
    <li>`password` : votre mot de passe {{site.data.keyword.Bluemix_notm}}.</li>
    <li>`uaa_client_id: cf`</li>
    <li>`uaa_client_secret:`</br><strong>Remarque</strong> : ajoutez la clé <code>uaa_client_secret</code> sans aucune valeur spécifiée.</li></ul></td>
    </tr>
    <tr>
    <td>Corps pour les clés d'API {{site.data.keyword.Bluemix_notm}}</td>
    <td><ul><li>`grant_type: urn:ibm:params:oauth:grant-type:apikey`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`apikey` : votre  clé d'API {{site.data.keyword.Bluemix_notm}}</li>
    <li>`uaa_client_id: cf`</li>
    <li>`uaa_client_secret:` </br><strong>Remarque</strong> : ajoutez la clé <code>uaa_client_secret</code> sans aucune valeur spécifiée.</li></ul></td>
    </tr>
    <tr>
    <td>Corps pour le code d'accès à usage unique {{site.data.keyword.Bluemix_notm}}</td>
      <td><ul><li><code>grant_type: urn:ibm:params:oauth:grant-type:passcode</code></li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`passcode` : votre code d'accès à usage unique {{site.data.keyword.Bluemix_notm}}. Exécutez la commande `ibmcloud login --sso` et suivez les instructions de la sortie de l'interface CLI pour extraire votre code d'accès à usage unique en utilisant votre navigateur Web.</li>
    <li>`uaa_client_id: cf`</li>
    <li>`uaa_client_secret:` </br><strong>Remarque</strong> : ajoutez la clé <code>uaa_client_secret</code> sans aucune valeur spécifiée.</li></ul>
    </td>
    </tr>
    </tbody>
    </table>

    Exemple de sortie pour l'utilisation d'une clé d'API :

    ```
    {
    "access_token": "<iam_access_token>",
    "refresh_token": "<iam_refresh_token>",
    "uaa_token": "<uaa_token>",
    "uaa_refresh_token": "<uaa_refresh_token>",
    "token_type": "Bearer",
    "expires_in": 3600,
    "expiration": 1493747503
    "scope": "ibm openid"
    }

    ```
    {: screen}

    Le jeton {{site.data.keyword.Bluemix_notm}} IAM figure dans la zone **access_token** de la sortie de votre API. Notez la valeur du jeton {{site.data.keyword.Bluemix_notm}} IAM pour extraire des informations d'en-tête supplémentaires dans les étapes suivantes.

2.  Extrayez l'ID du compte {{site.data.keyword.Bluemix_notm}} que vous souhaitez utiliser. Remplacez `<iam_access_token>` par le jeton {{site.data.keyword.Bluemix_notm}} IAM que vous avez extrait de la zone **access_token** de votre sortie d'API à l'étape précédente. Dans votre sortie d'API, l'ID de votre compte {{site.data.keyword.Bluemix_notm}} se trouve dans la zone **resources.metadata.guid**.

    ```
    GET https://accountmanagement.ng.bluemix.net/v1/accounts
    ```
    {: codeblock}

    <table summary="Paramètres d'entrée permettant d'obtenir l'ID de compte {{site.data.keyword.Bluemix_notm}}, avec le paramètre d'entrée dans la colonne 1 et la valeur dans la colonne 2.">
    <caption>Paramètres d'entrée permettant d'obtenir un ID de compte {{site.data.keyword.Bluemix_notm}}</caption>
    <thead>
  	<th>Paramètres d'entrée</th>
  	<th>Valeurs</th>
    </thead>
    <tbody>
  	<tr>
  		<td>En-têtes</td>
      <td><ul><li><code>Content-Type: application/json</code></li>
        <li>`Authorization: bearer <iam_access_token>`</li>
        <li><code>Accept: application/json</code></li></ul></td>
  	</tr>
    </tbody>
    </table>

    Exemple de sortie :

    ```
    {
      "total_results": 3,
      "total_pages": 1,
      "prev_url": null,
      "next_url": null,
      "resources":
        {
          "metadata": {
            "guid": "<account_ID>",
            "url": "/v1/accounts/<account_ID>",
            "created_at": "2016-01-07T18:55:09.726Z",
            "updated_at": "2017-04-28T23:46:03.739Z",
            "origin": "BSS"
    ...
    ```
    {: screen}

3.  Générez un nouveau jeton {{site.data.keyword.Bluemix_notm}} IAM comprenant vos données d'identification {{site.data.keyword.Bluemix_notm}} et l'ID du compte que vous souhaitez utiliser.

    Si vous utilisez une clé d'API {{site.data.keyword.Bluemix_notm}}, vous devez utiliser l'ID du compte {{site.data.keyword.Bluemix_notm}} pour lequel la clé d'API a été créée. Pour accéder aux clusters figurant dans un autre compte, connectez-vous à ce compte et créez une clé d'API {{site.data.keyword.Bluemix_notm}} basée sur ce compte.
    {: note}

    ```
    POST https://iam.bluemix.net/identity/token
    ```
    {: codeblock}

    <table summary="Paramètres d'entrée permettant d'extraire des jetons IAM, avec le paramètre d'entrée dans la colonne 1 et la valeur dans la colonne 2.">
    <caption>Paramètres d'entrée permettant d'obtenir des jetons IAM</caption>
    <thead>
        <th>Paramètres d'entrée</th>
        <th>Valeurs</th>
    </thead>
    <tbody>
    <tr>
    <td>En-tête</td>
    <td><ul><li>`Content-Type: application/x-www-form-urlencoded`</li> <li>`Authorization: Basic Yng6Yng=`<p><strong>Remarque</strong> : <code>Yng6Yng=</code> est égal à l'autorisation codée dans l'URL correspondant au nom d'utilisateur <strong>bx</strong> et au mot de passe <strong>bx</strong>.</p></li></ul>
    </td>
    </tr>
    <tr>
    <td>Corps pour le nom d'utilisateur et le mot de passe {{site.data.keyword.Bluemix_notm}}</td>
    <td><ul><li>`grant_type: password`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`username` : votre nom d'utilisateur {{site.data.keyword.Bluemix_notm}}. </li>
    <li>`password` : votre mot de passe {{site.data.keyword.Bluemix_notm}}. </li>
    <li>`uaa_client_ID: cf`</li>
    <li>`uaa_client_secret:` </br><strong>Remarque</strong> : ajoutez la clé <code>uaa_client_secret</code> sans aucune valeur spécifiée.</li>
    <li>`bss_account` : ID de compte {{site.data.keyword.Bluemix_notm}} que vous avez extrait lors de l'étape précédente.</li></ul>
    </td>
    </tr>
    <tr>
    <td>Corps pour les clés d'API {{site.data.keyword.Bluemix_notm}}</td>
    <td><ul><li>`grant_type: urn:ibm:params:oauth:grant-type:apikey`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`apikey` : votre clé d'API {{site.data.keyword.Bluemix_notm}}</li>
    <li>`uaa_client_ID: cf`</li>
    <li>`uaa_client_secret:` </br><strong>Remarque</strong> : ajoutez la clé <code>uaa_client_secret</code> sans aucune valeur spécifiée.</li>
    <li>`bss_account` : ID de compte {{site.data.keyword.Bluemix_notm}} que vous avez extrait lors de l'étape précédente.</li></ul>
      </td>
    </tr>
    <tr>
    <td>Corps pour le code d'accès à usage unique {{site.data.keyword.Bluemix_notm}}</td>
    <td><ul><li>`grant_type: urn:ibm:params:oauth:grant-type:passcode`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`passcode` : votre code d'accès {{site.data.keyword.Bluemix_notm}}. </li>
    <li>`uaa_client_ID: cf`</li>
    <li>`uaa_client_secret:` </br><strong>Remarque</strong> : ajoutez la clé <code>uaa_client_secret</code> sans aucune valeur spécifiée.</li>
    <li>`bss_account` : ID de compte {{site.data.keyword.Bluemix_notm}} que vous avez extrait lors de l'étape précédente.</li></ul></td>
    </tr>
    </tbody>
    </table>

    Exemple de sortie :

    ```
    {
      "access_token": "<iam_token>",
      "refresh_token": "<iam_refresh_token>",
      "token_type": "Bearer",
      "expires_in": 3600,
      "expiration": 1493747503
    }

    ```
    {: screen}

    Le jeton {{site.data.keyword.Bluemix_notm}} IAM figure dans la zone **access_token** et le jeton d'actualisation figure dans la zone **refresh_token** de la sortie d'API.

4.  Répertoriez les régions {{site.data.keyword.containerlong_notm}} disponibles et sélectionnez celle dans laquelle vous souhaitez travailler. Utilisez le jeton d'accès IAM et le jeton d'actualisation issus de l'étape précédente pour créer vos informations d'en-tête.
    ```
    GET https://containers.cloud.ibm.com/v1/regions
    ```
    {: codeblock}

    <table summary="Paramètres d'entrée permettant d'extraire des régions {{site.data.keyword.containerlong_notm}}, avec le paramètre d'entrée dans la colonne 1 et la valeur dans la colonne 2.">
    <caption>Paramètres d'entrée permettant d'extraire des régions {{site.data.keyword.containerlong_notm}}.</caption>
    <thead>
    <th>Paramètres d'entrée</th>
    <th>Valeurs</th>
    </thead>
    <tbody>
    <tr>
    <td>En-tête</td>
    <td><ul><li>`Authorization: bearer <iam_token>`</li>
    <li>`X-Auth-Refresh-Token: <refresh_token>`</li></ul></td>
    </tr>
    </tbody>
    </table>

    Exemple de sortie :
    ```
    {
    "regions": [
        {
            "name": "ap-north",
            "alias": "jp-tok",
            "cfURL": "api.au-syd.bluemix.net",
            "freeEnabled": false
        },
        {
            "name": "ap-south",
            "alias": "au-syd",
            "cfURL": "api.au-syd.bluemix.net",
            "freeEnabled": true
        },
        {
            "name": "eu-central",
            "alias": "eu-de",
            "cfURL": "api.eu-de.bluemix.net",
            "freeEnabled": true
        },
        {
            "name": "uk-south",
            "alias": "eu-gb",
            "cfURL": "api.eu-gb.bluemix.net",
            "freeEnabled": true
        },
        {
            "name": "us-east",
            "alias": "us-east",
            "cfURL": "api.ng.bluemix.net",
            "freeEnabled": false
        },
        {
            "name": "us-south",
            "alias": "us-south",
            "cfURL": "api.ng.bluemix.net",
            "freeEnabled": true
        }
    ]
    }
    ```
    {: screen}

5.  Répertoriez tous les clusters de la région {{site.data.keyword.containerlong_notm}} que vous avez sélectionnée. Si vous souhaitez [exécuter des demandes d'API Kubernetes sur votre cluster](#kube_api), prenez soin de noter les éléments **id** et **region** de votre cluster.

     ```
     GET https://containers.cloud.ibm.com/v1/clusters
     ```
     {: codeblock}

     <table summary="Paramètres d'entrée permettant d'utiliser l'API {{site.data.keyword.containerlong_notm}}, avec le paramètre d'entrée dans la colonne 1 et la valeur dans la colonne 2.">
     <caption>Paramètres d'entrée permettant d'utiliser l'API {{site.data.keyword.containerlong_notm}}.</caption>
     <thead>
     <th>Paramètres d'entrée</th>
     <th>Valeurs</th>
     </thead>
     <tbody>
     <tr>
     <td>En-tête</td>
     <td><ul><li>`Authorization: bearer <iam_token>`</li>
       <li>`X-Auth-Refresh-Token: <refresh_token>`</li><li>`X-Region: <region>`</li></ul></td>
     </tr>
     </tbody>
     </table>

5.  Examinez la [documentation sur l'API {{site.data.keyword.containerlong_notm}} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://containers.cloud.ibm.com/global/swagger-global-api) pour obtenir la liste des API prises en charge. 

<br />


## Utilisation de votre cluster à l'aide de l'API Kubernetes
{: #kube_api}

Vous pouvez utiliser l'[API Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/reference/using-api/api-overview/) pour interagir avec votre cluster dans {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

Les instructions ci-après requièrent un accès de réseau public dans votre cluster pour établir une connexion au noeud final de service public de votre maître Kubernetes.
{: note}

1. Suivez les étapes décrites dans la rubrique [Automatisation des déploiements de cluster à l'aide de l'API](#cs_api) pour extraire votre jeton d'accès {{site.data.keyword.Bluemix_notm}} IAM, votre jeton d'actualisation, l'ID du cluster dans lequel vous souhaitez exécuter les demandes d'API Kubernetes et la région {{site.data.keyword.containerlong_notm}} dans laquelle se trouve votre cluster.

2. Extrayez le jeton d'actualisation délégué {{site.data.keyword.Bluemix_notm}} IAM.
   ```
   POST https://iam.bluemix.net/identity/token
   ```
   {: codeblock}

   <table summary="Paramètres d'entrée permettant d'extraire un jeton d'actualisation délégué IAM, avec le paramètre d'entrée dans la colonne 1 et la valeur dans la colonne 2.">
   <caption>Paramètres d'entrée permettant d'extraire un jeton d'actualisation délégué IAM. </caption>
   <thead>
   <th>Paramètres d'entrée</th>
   <th>Valeurs</th>
   </thead>
   <tbody>
   <tr>
   <td>En-tête</td>
   <td><ul><li>`Content-Type: application/x-www-form-urlencoded`</li> <li>`Authorization: Basic Yng6Yng=`</br><strong>Remarque</strong> : <code>Yng6Yng=</code> est égal à l'autorisation codée dans l'URL correspondant au nom d'utilisateur <strong>bx</strong> et au mot de passe <strong>bx</strong>.</li><li>`cache-control: no-cache`</li></ul>
   </td>
   </tr>
   <tr>
   <td>Corps</td>
   <td><ul><li>`delegated_refresh_token_expiry: 600`</li>
   <li>`receiver_client_ids: kube`</li>
   <li>`response_type: delegated_refresh_token` </li>
   <li>`refresh_token` : votre jeton d'actualisation {{site.data.keyword.Bluemix_notm}} IAM. </li>
   <li>`grant_type: refresh_token`</li></ul></td>
   </tr>
   </tbody>
   </table>

   Exemple de sortie :
   ```
   {
    "delegated_refresh_token": <delegated_refresh_token>
   }
   ```
   {: screen}

3. Extrayez un jeton d'ID {{site.data.keyword.Bluemix_notm}} IAM, un jeton d'accès IAM et un jeton d'actualisation IAM en utilisant le jeton d'actualisation délégué obtenu lors de l'étape précédente. Dans votre sortie d'API, le jeton d'ID IAM se trouve dans la zone **id_token**, le jeton d'accès IAM se trouve dans la zone **access_token** et le jeton d'actualisation IAM se trouve dans la zone **refresh_token**.
   ```
   POST https://iam.bluemix.net/identity/token
   ```
   {: codeblock}

   <table summary="Paramètres d'entrée permettant d'extraire des jetons d'ID et d'accès IAM, avec le paramètre d'entrée dans la colonne 1 et la valeur dans la colonne 2.">
   <caption>Paramètres d'entrée permettant d'extraire des jetons d'ID et d'accès IAM</caption>
   <thead>
   <th>Paramètres d'entrée</th>
   <th>Valeurs</th>
   </thead>
   <tbody>
   <tr>
   <td>En-tête</td>
   <td><ul><li>`Content-Type: application/x-www-form-urlencoded`</li> <li>`Authorization: Basic a3ViZTprdWJl`</br><strong>Remarque</strong> : <code>a3ViZTprdWJl</code> est égal à l'autorisation codée dans l'URL correspondant au nom d'utilisateur <strong><code>kube</code></strong> et au mot de passe <strong><code>kube</code></strong>.</li><li>`cache-control: no-cache`</li></ul>
   </td>
   </tr>
   <tr>
   <td>Corps</td>
   <td><ul><li>`refresh_token` : votre jeton d'actualisation délégué {{site.data.keyword.Bluemix_notm}} IAM. </li>
   <li>`grant_type: urn:ibm:params:oauth:grant-type:delegated-refresh-token`</li></ul></td>
   </tr>
   </tbody>
   </table>

   Exemple de sortie :
   ```
   {
    "access_token": "<iam_access_token>",
    "id_token": "<iam_id_token>",
    "refresh_token": "<iam_refresh_token>",
    "token_type": "Bearer",
    "expires_in": 3600,
    "expiration": 1553629664,
    "scope": "ibm openid containers-kubernetes"
   }
   ```
   {: screen}

4. Extrayez l'URL publique de votre maître Kubernetes en utilisant le jeton d'accès IAM, le jeton d'ID IAM, le jeton d'actualisation IAM et la région {{site.data.keyword.containerlong_notm}} dans laquelle se trouve votre cluster. L'URL figure dans la zone **`publicServiceEndpointURL`** de votre sortie d'API.
   ```
   GET https://containers.cloud.ibm.com/v1/clusters/<cluster_ID>
   ```
   {: codeblock}

   <table summary="Paramètres d'entrée permettant d'extraire le noeud final de service public pour votre maître Kubernetes, avec le paramètre d'entrée dans la colonne 1 et la valeur dans la colonne 2.">
   <caption>Paramètres d'entrée permettant d'extraire le noeud final de service public pour votre maître Kubernetes.</caption>
   <thead>
   <th>Paramètres d'entrée</th>
   <th>Valeurs</th>
   </thead>
   <tbody>
   <tr>
   <td>En-tête</td>
     <td><ul><li>`Authorization` : votre jeton d'accès {{site.data.keyword.Bluemix_notm}} IAM.</li><li>`X-Auth-Refresh-Token` : votre jeton d'actualisation {{site.data.keyword.Bluemix_notm}} IAM.</li><li>`X-Region` : région {{site.data.keyword.containerlong_notm}} de votre cluster que vous avez extraite avec l'API `GET https://containers.cloud.ibm.com/v1/clusters` dans [Automatisation des déploiements de cluster à l'aide de l'API](#cs_api). </li></ul>
   </td>
   </tr>
   <tr>
   <td>Chemin</td>
   <td>`<cluster_ID> :` ID de votre cluster que vous avez extrait avec l'API `GET https://containers.cloud.ibm.com/v1/clusters` dans [Automatisation des déploiements de cluster à l'aide de l'API](#cs_api).      </td>
   </tr>
   </tbody>
   </table>

   Exemple de sortie :
   ```
   {
    "location": "Dallas",
    "dataCenter": "dal10",
    "multiAzCapable": true,
    "vlans": null,
    "worker_vlans": null,
    "workerZones": [
        "dal10"
    ],
    "id": "1abc123b123b124ab1234a1a12a12a12",
    "name": "mycluster",
    "region": "us-south",
    ...
    "publicServiceEndpointURL": "https://c7.us-south.containers.cloud.ibm.com:27078"
   }
   ```
   {: screen}

5. Exécutez des demandes d'API Kubernetes sur votre cluster à l'aide du jeton d'ID IAM que vous avez extrait précédemment. Par exemple, répertoriez la version de Kubernetes qui s'exécute dans votre cluster.

   Si vous avez activé la vérification de certificat SSL dans votre structure de test d'API, prenez soin de désactiver cette fonction.
   {: tip}

   ```
   GET <publicServiceEndpointURL>/version
   ```
   {: codeblock}

   <table summary="Paramètres d'entrée permettant de visualiser la version de Kubernetes qui s'exécute dans votre cluster, avec le paramètre d'entrée dans la colonne 1 et la valeur dans la colonne 2.">
   <caption>Paramètres d'entrée permettant de visualiser la version de Kubernetes qui s'exécute dans votre cluster. </caption>
   <thead>
   <th>Paramètres d'entrée</th>
   <th>Valeurs</th>
   </thead>
   <tbody>
   <tr>
   <td>En-tête</td>
   <td>`Authorization: bearer <id_token>`</td>
   </tr>
   <tr>
   <td>Chemin</td>
   <td>`<publicServiceEndpointURL>` : URL **`publicServiceEndpointURL`** de votre maître Kubernetes que vous avez extraite lors de l'étape précédente.      </td>
   </tr>
   </tbody>
   </table>

   Exemple de sortie :
   ```
   {
    "major": "1",
    "minor": "13",
    "gitVersion": "v1.13.4+IKS",
    "gitCommit": "c35166bd86eaa91d17af1c08289ffeab3e71e11e",
    "gitTreeState": "clean",
    "buildDate": "2019-03-21T10:08:03Z",
    "goVersion": "go1.11.5",
    "compiler": "gc",
    "platform": "linux/amd64"
   }
   ```
   {: screen}

6. Examinez la [documentation sur l'API Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/reference/kubernetes-api/) pour obtenir la liste des API prises en charge pour la dernière version de Kubernetes. Prenez soin d'utiliser la documentation d'API correspondant à la version Kubernetes de votre cluster. Si vous n'utilisez pas la dernière version Kubernetes, ajoutez votre version à la fin de l'URL. Par exemple, pour accéder à la documentation d'API de la version 1.12, ajoutez `v1.12`.


## Actualisation des jetons d'accès {{site.data.keyword.Bluemix_notm}} IAM et acquisition de nouveaux jetons d'actualisation avec l'API
{: #cs_api_refresh}

Chaque jeton d'accès {{site.data.keyword.Bluemix_notm}} IAM (Identity and Access Management) émis via l'API expire au bout d'une heure. Vous devez actualiser régulièrement votre jeton d'accès pour garantir l'accès à l'API {{site.data.keyword.Bluemix_notm}}. Vous pouvez utiliser la même procédure pour obtenir un nouveau jeton d'actualisation.
{:shortdesc}

Avant de commencer, vérifiez que vous disposez d'un jeton d'actualisation {{site.data.keyword.Bluemix_notm}} IAM ou d'une clé d'API {{site.data.keyword.Bluemix_notm}} que vous pourrez utiliser pour demander un nouveau jeton d'accès.
- **Jeton d'actualisation :** suivez les instructions indiquées dans [Automatisation de la procédure de création et de gestion de cluster via l'API {{site.data.keyword.Bluemix_notm}}](#cs_api).
- **Clé d'API :** récupérez votre clé d'API [{{site.data.keyword.Bluemix_notm}} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/) comme suit :
   1. Dans la barre de menu, cliquez sur **Gérer** > **Accès (IAM)**.
   2. Cliquez sur la page **Utilisateurs**, puis sélectionnez-vous.
   3. Dans le panneau **Clés d'API**, cliquez sur **Créer une clé d'API IBM Cloud**.
   4. Entrez un **Nom** et une **Description** pour la clé d'API et cliquez sur **Créer**.
   4. Cliquez sur **Afficher** pour voir la clé d'API qui a été générée pour vous.
   5. Copiez la clé d'API pour pouvoir l'utiliser pour récupérer votre nouveau jeton d'accès {{site.data.keyword.Bluemix_notm}} IAM.

Si vous désirez créer un jeton {{site.data.keyword.Bluemix_notm}} IAM ou obtenir un nouveau jeton d'actualisation, procédez comme suit :

1.  Générez un nouveau jeton {{site.data.keyword.Bluemix_notm}} IAM en utilisant le jeton d'actualisation ou la clé d'API {{site.data.keyword.Bluemix_notm}}.
    ```
    POST https://iam.bluemix.net/identity/token
    ```
    {: codeblock}

    <table summary="Paramètres d'entrée permettant de générer un nouveau jeton IAM, avec le paramètre d'entrée dans la colonne 1 et la valeur dans la colonne 2.">
    <caption>Paramètres d'entrée pour un nouveau jeton {{site.data.keyword.Bluemix_notm}} IAM</caption>
    <thead>
    <th>Paramètres d'entrée</th>
    <th>Valeurs</th>
    </thead>
    <tbody>
    <tr>
    <td>En-tête</td>
    <td><ul><li>`Content-Type: application/x-www-form-urlencoded`</li>
      <li>`Authorization: Basic Yng6Yng=`</br></br><strong>Remarque :</strong> <code>Yng6Yng=</code> est égal à l'autorisation codée dans l'URL correspondant au nom d'utilisateur <strong>bx</strong> et au mot de passe <strong>bx</strong>.</li></ul></td>
    </tr>
    <tr>
    <td>Corps lorsque vous utilisez le jeton d'actualisation</td>
    <td><ul><li>`grant_type: refresh_token`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`refresh_token:` votre jeton d'actualisation {{site.data.keyword.Bluemix_notm}} IAM. </li>
    <li>`uaa_client_ID: cf`</li>
    <li>`uaa_client_secret:`</li>
    <li>`bss_account:` votre ID de compte {{site.data.keyword.Bluemix_notm}}. </li></ul><strong>Remarque</strong> : ajoutez la clé <code>uaa_client_secret</code> sans spécifier de valeur.</td>
    </tr>
    <tr>
      <td>Corps lorsque vous utilisez la clé d'API {{site.data.keyword.Bluemix_notm}}</td>
      <td><ul><li>`grant_type: urn:ibm:params:oauth:grant-type:apikey`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`apikey:` votre clé d'API {{site.data.keyword.Bluemix_notm}} </li>
    <li>`uaa_client_ID: cf`</li>
        <li>`uaa_client_secret:`</li></ul><strong>Remarque :</strong> ajoutez la clé <code>uaa_client_secret</code> sans spécifier de valeur.</td>
    </tr>
    </tbody>
    </table>

    Exemple de sortie d'API :

    ```
    {
      "access_token": "<iam_token>",
      "refresh_token": "<iam_refresh_token>",
      "uaa_token": "<uaa_token>",
      "uaa_refresh_token": "<uaa_refresh_token>",
      "token_type": "Bearer",
      "expires_in": 3600,
      "expiration": 1493747503
    }

    ```
    {: screen}

    Votre nouveau jeton {{site.data.keyword.Bluemix_notm}} IAM figure dans la zone **access_token** et le jeton d'actualisation dans la zone **refresh_token** de la sortie d'API.

2.  Continuez à travailler avec la [documentation sur l'API {{site.data.keyword.containerlong_notm}} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://containers.cloud.ibm.com/global/swagger-global-api) en utilisant le jeton de l'étape précédente. 

<br />


## Actualisation des jetons d'accès {{site.data.keyword.Bluemix_notm}} IAM et acquisition de nouveaux jetons d'actualisation avec l'interface de ligne de commande (CLI)
{: #cs_cli_refresh}

Lorsque vous démarrez une nouvelle session d'interface de ligne de commande (CLI) ou si 24 heures se sont écoulées dans votre session CLI en cours, vous devez définir le contexte de votre cluster en exécutant la commande `ibmcloud ks cluster-config --cluster <cluster_name>`. Lorsque vous définissez le contexte de votre cluster avec cette commande, le fichier `kubeconfig` de votre cluster Kubernetes est téléchargé. De plus, un jeton d'ID {{site.data.keyword.Bluemix_notm}} IAM (Identity and Access Management) et un jeton d'actualisation sont émis pour l'authentification.
{: shortdesc}

**Jeton d'ID** : chaque jeton d'ID IAM émis avec l'interface de ligne de commande expire au bout d'une heure. Lorsqu'il expiré, le jeton d'actualisation est envoyé au fournisseur de jeton pour actualiser le jeton d'ID. Votre authentification est actualisée et vous pouvez continuer à exécuter des commandes sur votre cluster.

**Jeton d'actualisation** : les jetons d'actualisation expire au bout de 30 jours. Si le jeton d'actualisation a expiré, le jeton d'ID ne peut plus être actualisé et vous ne pouvez plus continuer à exécuter des commandes dans l'interface de ligne de commande. Vous pouvez obtenir un nouveau jeton d'actualisation en exécutant la commande `ibmcloud ks cluster-config --cluster <cluster_name>`. Cette commande actualise également votre jeton d'ID.
