---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: kubernetes, iks, helm, without tiller, private cluster tiller, integrations, helm chart

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



# Ajout de services à l'aide de chartes Helm
{: #helm}

Vous pouvez ajouter des applications Kubernetes complexes à votre cluster en utilisant des chartes Helm.
{: shortdesc}

**Qu'est-ce que Helm et comment l'utiliser ?** </br>
[Helm ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://helm.sh) est un gestionnaire de package Kubernetes qui utilise des chartes Helm pour définir, installer et mettre à niveau des applications Kubernetes complexes dans votre cluster. Les chartes Helm regroupent les spécifications permettant de générer des fichiers YAML pour les ressources Kubernetes qui créent votre application. Ces ressources Kubernetes sont automatiquement appliquées dans votre cluster et se voient affecter une version par Helm. Vous pouvez également utiliser Helm pour spécifier et conditionner votre propre application et laisser Helm générer les fichiers YAML pour vos ressources Kubernetes.  

Pour utiliser Helm dans votre cluster, vous devez installer l'interface de ligne de commande Helm sur votre machine locale et le serveur Helm Tiller dans chaque cluster où vous souhaitez utiliser Helm.

**Quelles chartes Helm sont prises en charge dans {{site.data.keyword.containerlong_notm}} ?** </br>
Pour obtenir une présentation des chartes Helm disponibles, voir le [catalogue de chartes Helm ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/kubernetes/solutions/helm-charts). Les chartes Helm répertoriées dans ce catalogue sont regroupées comme suit :

- **iks-charts** : chartes Helm qui sont approuvées pour {{site.data.keyword.containerlong_notm}}. Le nom de ce référentiel, `ibm`, a été remplacé par `iks-charts`.
- **ibm-charts** : chartes Helm qui sont approuvées pour les clusters {{site.data.keyword.containerlong_notm}} et {{site.data.keyword.Bluemix_notm}} Private.
- **kubernetes** : chartes Helm qui sont fournies par la communauté Kubernetes et considérées comme étant de type `stable` par la gouvernance de communauté. Le fonctionnement de ces chartes dans les clusters {{site.data.keyword.containerlong_notm}} ou {{site.data.keyword.Bluemix_notm}} Private n'est pas vérifié.
- **kubernetes-incubator** : chartes Helm qui sont fournies par la communauté Kubernetes et considérées comme étant de type `incubator` par la gouvernance de communauté. Le fonctionnement de ces chartes dans les clusters {{site.data.keyword.containerlong_notm}} ou {{site.data.keyword.Bluemix_notm}} Private n'est pas vérifié.

Les chartes Helm des référentiels **iks-charts** et **ibm-charts** sont entièrement intégrées dans l'organisation de support {{site.data.keyword.Bluemix_notm}}. Pour toute question ou problématique relative à l'utilisation de ces chartes Helm, vous pouvez utiliser l'un des canaux de support {{site.data.keyword.containerlong_notm}}. Pour plus d'informations, voir [Aide et assistance](/docs/containers?topic=containers-cs_troubleshoot_clusters#clusters_getting_help).

**Quels sont les prérequis pour l'utilisation de Helm et puis-je utiliser Helm dans un cluster privé ?** </br>
Pour déployer des chartes Helm, vous devez installer l'interface de ligne de commande (CLI) Helm sur votre machine locale et installer le serveur Helm Tiller dans votre cluster. L'image de Tiller est stockée dans le registre public Google Container Registry. Pour accéder à l'image lors de l'installation de Tiller, votre cluster doit autoriser la connectivité de réseau public au registre public Google Container Registry. Les clusters qui activent le noeud final de service public peuvent accéder automatiquement à l'image. Les clusters privés protégés avec un pare-feu personnalisé ou les clusters ayant activé le noeud final de service privé uniquement, n'autorisent pas l'accès à l'image de Tiller. Vous pouvez à la place [extraire l'image sur votre machine locale et l'insérer dans votre espace de nom dans {{site.data.keyword.registryshort_notm}}](#private_local_tiller) ou [installer des chartes Helm sans utiliser le serveur Tiller](#private_install_without_tiller).


## Configuration de Helm dans un cluster avec accès public
{: #public_helm_install}

Si votre cluster a activé le noeud final de service public, vous pouvez installer le serveur Helm Tiller en utilisant l'image publique figurant dans Google Container Registry.
{: shortdesc}

Avant de commencer :
- [Connectez-vous à votre compte. Le cas échéant, ciblez le groupe de ressources approprié. Définissez le contexte pour votre cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
- Pour installer Tiller avec un compte de service Kubernetes et la liaison de rôle de cluster dans l'espace de nom `kube-system`, vous devez disposer du [rôle `cluster-admin`](/docs/containers?topic=containers-users#access_policies).

Pour installer Helm dans un cluster avec un accès public :

1. Installez l'<a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">interface de ligne de commande (CLI) Helm <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a> sur votre machine locale.

2. Vérifiez si vous avez déjà installé Tiller avec un compte de service Kubernetes dans votre cluster.
   ```
   kubectl get serviceaccount --all-namespaces | grep tiller
   ```
   {: pre}

   Exemple de sortie si Tiller est installé :
   ```
   kube-system      tiller                               1         189d
   ```
   {: screen}

   L'exemple de sortie comprend l'espace de nom Kubernetes et le nom du compte de service pour Tiller. Si Tiller n'est pas installé avec un compte de service dans votre cluster, aucune sortie d'interface de ligne de commande n'est renvoyée.

3. **Important** : pour maintenir la sécurité du cluster, configurez Tiller avec un compte de service et une liaison de rôle de cluster dans votre cluster.
   - **Si Tiller est installé avec un compte de service : **
     1. Créez une liaison de rôle de cluster pour le compte de service Tiller. Remplacez `<namespace>` par l'espace de nom dans lequel Tiller est installé dans votre cluster.
        ```
        kubectl create clusterrolebinding tiller --clusterrole=cluster-admin --serviceaccount=<namespace>:tiller -n <namespace>
        ```
        {: pre}

     2. Mettez à jour Tiller. Remplacez `<tiller_service_account_name>` par le nom du compte de service Kubernetes pour Tiller que vous avez extrait lors de l'étape précédente.
        ```
        helm init --upgrade --service-account <tiller_service_account_name>
        ```
        {: pre}

     3. Vérifiez que la zone **Status** du pod `tiller-deploy` indique `Running` dans votre cluster.
        ```
        kubectl get pods -n <namespace> -l app=helm
        ```
        {: pre}

        Exemple de sortie :

        ```
        NAME                            READY     STATUS    RESTARTS   AGE
    tiller-deploy-352283156-nzbcm   1/1       Running   0          2m
        ```
        {: screen}

   - **Si Tiller n'est pas installé avec un compte de service :**
     1. Créez un compte de service Kubernetes et une liaison de rôle de cluster pour Tiller dans l'espace de nom `kube-system` de votre cluster.
        ```
        kubectl create serviceaccount tiller -n kube-system
        ```
        {: pre}

        ```
        kubectl create clusterrolebinding tiller --clusterrole=cluster-admin --serviceaccount=kube-system:tiller -n kube-system
        ```
        {: pre}

     2. Vérifiez que le compte de service Tiller est créé.
        ```
        kubectl get serviceaccount -n kube-system tiller
        ```
        {: pre}

        Exemple de sortie :
        ```
        NAME                                 SECRETS   AGE
    tiller                               1         2m
        ```
        {: screen}

     3. Initialisez l'interface de ligne de commande Helm et installez Tiller dans votre cluster avec le compte de service que vous avez créé.
        ```
        helm init --service-account tiller
        ```
        {: pre}

     4. Vérifiez que la zone **Status** du pod `tiller-deploy` indique `Running` dans votre cluster.
        ```
        kubectl get pods -n kube-system -l app=helm
        ```
        {: pre}

        Exemple de sortie :
        ```
        NAME                            READY     STATUS    RESTARTS   AGE
    tiller-deploy-352283156-nzbcm   1/1       Running   0          2m
        ```
        {: screen}

4. Ajoutez les référentiels Helm d'{{site.data.keyword.Bluemix_notm}} dans votre instance Helm.
   ```
   helm repo add iks-charts https://icr.io/helm/iks-charts
   ```
   {: pre}

   ```
   helm repo add ibm-charts https://icr.io/helm/ibm-charts
   ```
   {: pre}

5. Mettez à jour les référentiels pour extraire les dernières versions de toutes les chartes Helm.
   ```
   helm repo update
   ```
   {: pre}

6. Répertoriez les chartes Helm actuellement disponibles dans les référentiels {{site.data.keyword.Bluemix_notm}}.
   ```
   helm search iks-charts
   ```
   {: pre}

   ```
   helm search ibm-charts
   ```
   {: pre}

7. Identifiez la charte Helm que vous voulez installer et suivez les instructions indiquées dans le fichier `README` de la charte pour installer la charte Helm dans votre cluster.


## Clusters privés : Insertion de l'image de Tiller dans votre espace de nom dans IBM Cloud Container Registry
{: #private_local_tiller}

Vous pouvez extraire l'image de Tiller sur votre machine locale, puis l'insérer dans votre espace de nom dans {{site.data.keyword.registryshort_notm}} et installer Tiller dans votre cluster privé en utilisant l'image d'{{site.data.keyword.registryshort_notm}}.
{: shortdesc}

Si vous souhaitez installer une charte Helm sans utiliser Tiller, voir [Clusters privés : Installation de chartes Helm sans utiliser Tiller](#private_install_without_tiller).
{: tip}

Avant de commencer :
- Installez Docker sur votre machine locale. Si vous avez installé l'[interface de ligne de commande {{site.data.keyword.Bluemix_notm}}](/docs/cli?topic=cloud-cli-getting-started), Docker est déjà installé.
- [Installez le plug-in de l'interface de ligne de commande {{site.data.keyword.registryshort_notm}} et configurez un espace de nom](/docs/services/Registry?topic=registry-getting-started#gs_registry_cli_install).
- Pour installer Tiller avec un compte de service Kubernetes et la liaison de rôle de cluster dans l'espace de nom `kube-system`, vous devez disposer du [rôle `cluster-admin`](/docs/containers?topic=containers-users#access_policies).

Pour installer Tiller en utilisant {{site.data.keyword.registryshort_notm}} :

1. Installez l'<a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">interface de ligne de commande (CLI) Helm <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a> sur votre machine locale.
2. Connectez-vous à votre cluster privé à l'aide du tunnel VPN de l'infrastructure {{site.data.keyword.Bluemix_notm}} que vous avez configuré.
3. **Important** : pour assurer la sécurité du cluster, créez un compte de service pour Tiller dans l'espace de nom `kube-system` et une liaison de rôle de cluster RBAC Kubernetes pour le pod `tiller-deploy` en appliquant le fichier YAML suivant à partir du [référentiel `kube-samples` d'{{site.data.keyword.Bluemix_notm}}](https://github.com/IBM-Cloud/kube-samples/blob/master/rbac/serviceaccount-tiller.yaml).
    1. [Procurez-vous le fichier YAML du compte de service et de liaison de rôle de cluster Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/rbac/serviceaccount-tiller.yaml).

    2. Créez les ressources Kubernetes dans votre cluster.
       ```
       kubectl apply -f service-account.yaml
       ```
       {: pre}

       ```
       kubectl apply -f cluster-role-binding.yaml
       ```
       {: pre}

4. [Recherchez la version de Tiller ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://console.cloud.google.com/gcr/images/kubernetes-helm/GLOBAL/tiller?gcrImageListsize=30) que vous souhaitez installer dans votre cluster. Si vous n'avez pas besoin d'une version particulière, utilisez la version la plus récente.

5. Extrayez l'image de Tiller du registre Google Container Registry public sur votre machine locale.
   ```
   docker pull gcr.io/kubernetes-helm/tiller:v2.11.0
   ```
   {: pre}

   Exemple de sortie :
   ```
   docker pull gcr.io/kubernetes-helm/tiller:v2.13.0
   v2.13.0: Pulling from kubernetes-helm/tiller
   48ecbb6b270e: Pull complete
   d3fa0712c71b: Pull complete
   bf13a43b92e9: Pull complete
   b3f98be98675: Pull complete
   Digest: sha256:c4bf03bb67b3ae07e38e834f29dc7fd43f472f67cad3c078279ff1bbbb463aa6
   Status: Downloaded newer image for gcr.io/kubernetes-helm/tiller:v2.13.0
   ```
   {: screen}

6. [Insérez l'image de Tiller dans votre espace de nom dans {{site.data.keyword.registryshort_notm}}](/docs/services/Registry?topic=registry-getting-started#gs_registry_images_pushing).

7. Pour accéder à l'image qui se trouve dans {{site.data.keyword.registryshort_notm}} à partir de votre cluster, [copiez la valeur confidentielle d'extraction d'image depuis l'espace de nom par défaut vers l'espace de nom `kube-system`](/docs/containers?topic=containers-images#copy_imagePullSecret).

8. Installez Tiller dans votre cluster privé en utilisant l'image que vous avez stockée dans votre espace de nom dans {{site.data.keyword.registryshort_notm}}.
   ```
   helm init --tiller-image <region>.icr.io/<mynamespace>/<myimage>:<tag> --service-account tiller
   ```
   {: pre}

9. Ajoutez les référentiels Helm d'{{site.data.keyword.Bluemix_notm}} dans votre instance Helm.
   ```
   helm repo add iks-charts https://icr.io/helm/iks-charts
   ```
   {: pre}

   ```
   helm repo add ibm-charts https://icr.io/helm/ibm-charts
   ```
   {: pre}

10. Mettez à jour les référentiels pour extraire les dernières versions de toutes les chartes Helm.
    ```
    helm repo update
    ```
    {: pre}

11. Répertoriez les chartes Helm actuellement disponibles dans les référentiels {{site.data.keyword.Bluemix_notm}}.
    ```
    helm search iks-charts
    ```
    {: pre}

    ```
    helm search ibm-charts
    ```
    {: pre}

12. Identifiez la charte Helm que vous voulez installer et suivez les instructions indiquées dans le fichier `README` de la charte pour installer la charte Helm dans votre cluster.


## Clusters privés : Installation des chartes Helm sans utiliser Tiller
{: #private_install_without_tiller}

Si vous ne souhaitez pas installer Tiller dans votre cluster privé, vous pouvez créer manuellement les fichiers YAML de la charte Helm et les appliquer en utilisant des commandes `kubectl`.
{: shortdesc}

Les étapes indiquées dans cet exemple montrent comment installer des chartes Helm à partir des référentiels de chartes Helm d'{{site.data.keyword.Bluemix_notm}} dans votre cluster privé. Si vous voulez installer une charte Helm qui n'est pas stockée dans l'un des référentiels de chartes Helm d'{{site.data.keyword.Bluemix_notm}}, vous devez suivre les instructions de cette rubrique pour créer les fichiers YAML pour votre charte Helm. Par ailleurs, vous devez télécharger l'image de la charte Helm à partir du registre de conteneur public, l'insérer dans votre espace de nom dans {{site.data.keyword.registryshort_notm}} et mettre à jour le fichier `values.yaml` pour utiliser cette image dans {{site.data.keyword.registryshort_notm}}.
{: note}

1. Installez l'<a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">interface de ligne de commande (CLI) Helm <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a> sur votre machine locale.
2. Connectez-vous à votre cluster privé à l'aide du tunnel VPN de l'infrastructure {{site.data.keyword.Bluemix_notm}} que vous avez configuré.
3. Ajoutez les référentiels Helm d'{{site.data.keyword.Bluemix_notm}} dans votre instance Helm.
   ```
   helm repo add iks-charts https://icr.io/helm/iks-charts
   ```
   {: pre}

   ```
   helm repo add ibm-charts https://icr.io/helm/ibm-charts
   ```
   {: pre}

4. Mettez à jour les référentiels pour extraire les dernières versions de toutes les chartes Helm.
   ```
   helm repo update
   ```
   {: pre}

5. Répertoriez les chartes Helm actuellement disponibles dans les référentiels {{site.data.keyword.Bluemix_notm}}.
   ```
   helm search iks-charts
   ```
   {: pre}

   ```
   helm search ibm-charts
   ```
   {: pre}

6. Identifiez la charte Helm que vous voulez installer, téléchargez la charte Helm sur votre machine locale et décompressez les fichiers de votre charte Helm. L'exemple suivant montre comment télécharger la charte Helm pour le programme de mise à l'échelle automatique de cluster (cluster autoscaler) version 1.0.3 et décompresser les fichiers dans un répertoire `cluster-autoscaler`.
   ```
   helm fetch iks-charts/ibm-iks-cluster-autoscaler --untar --untardir ./cluster-autoscaler --version 1.0.3
   ```
   {: pre}

7. Accédez au répertoire dans lequel vous avez décompressé les fichiers de la charte Helm.
   ```
   cd cluster-autoscaler
   ```
   {: pre}

8. Créez un répertoire `output` pour les fichiers YAML que vous générez à l'aide des fichiers dans votre charte Helm.
   ```
   mkdir output
   ```
   {: pre}

9. Ouvrez le fichier `values.yaml` et apportez toutes les modifications requises par les instructions d'installation de la charte Helm.
   ```
   nano ibm-iks-cluster-autoscaler/values.yaml
   ```
   {: pre}

10. Utilisez votre installation Helm locale pour créer tous les fichiers YAML Kubernetes pour votre charte Helm. Les fichiers YAML sont stockés dans le répertoire `output` que vous avez créé précédemment.
    ```
    helm template --values ./ibm-iks-cluster-autoscaler/values.yaml --output-dir ./output ./ibm-iks-cluster-autoscaler
    ```
    {: pre}

    Exemple de sortie :
    ```
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-configmap.yaml
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-service-account-roles.yaml
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-service.yaml
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-deployment.yaml
    ```
    {: screen}

11. Déployez tous les fichiers YAML dans votre cluster privé.
    ```
    kubectl apply --recursive --filename ./output
    ```
   {: pre}

12. Facultatif : supprimez tous les fichiers YAML du répertoire `output`.
    ```
    kubectl delete --recursive --filename ./output
    ```
    {: pre}

## Liens associés à Helm
{: #helm_links}

Cliquez sur les liens suivants pour accéder à d'autres informations relatives à Helm :
{: shortdesc}

* Affichez les chartes Helm que vous pouvez utiliser dans {{site.data.keyword.containerlong_notm}} dans le [Catalogue de chartes Helm ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/kubernetes/solutions/helm-charts) dans la console.
* Pour en savoir plus sur les commandes Helm que vous pouvez utiliser pour configurer et gérer des chartes Helm, voir la <a href="https://docs.helm.sh/helm/" target="_blank">documentation Helm <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a>.
* Pour en savoir plus sur comment augmenter la vitesse de déploiement avec les chartes Helm de Kubernetes, voir [Increase deployment velocity with Kubernetes Helm Charts ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://developer.ibm.com/recipes/tutorials/increase-deployment-velocity-with-kubernetes-helm-charts/).
