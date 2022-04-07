---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-03"

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


# Stockage de données sur IBM Cloud Object Storage
{: #object_storage}

[{{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-about) est un type de stockage persistant à haute disponibilité que vous pouvez monter sur des applications qui s'exécutent dans un cluster Kubernetes en utilisant le plug-in {{site.data.keyword.cos_full_notm}}. Il s'agit d'un plug-in de volume Flex Kubernetes qui connecte des compartiments Cloud {{site.data.keyword.cos_short}} à des pods dans votre cluster. Les informations stockées avec {{site.data.keyword.cos_full_notm}} sont chiffrées en transit et au repos. Elles sont réparties sur plusieurs sites géographiques et accessibles via HTTP à l'aide d'une API REST.
{: shortdesc}

Pour se connecter à {{site.data.keyword.cos_full_notm}}, votre cluster nécessite un accès réseau public pour s'authentifier auprès d'{{site.data.keyword.Bluemix_notm}} Identity and Access Management. Si vous disposez d'un cluster privé uniquement, vous pouvez communiquer avec le noeud final de service privé {{site.data.keyword.cos_full_notm}} si vous installez la version `1.0.3` ou ultérieure du plug-in, et que vous configurez votre instance de service {{site.data.keyword.cos_full_notm}} pour l'authentification HMAC. Si vous ne voulez pas utiliser l'authentification HMAC, vous devez ouvrir tout le trafic réseau sortant sur le port 443 pour que le plug-in fonctionne correctement dans un cluster privé.
{: important}

Avec la version 1.0.5, le plug-in {{site.data.keyword.cos_full_notm}}, nommé `ibmcloud-object-storage-plugin`, est renommé `ibm-object-storage-plugin`. Pour installer la nouvelle version du plug-in, vous devez [désinstaller l'ancienne installation de la charte Helm](#remove_cos_plugin) et [réinstaller la charte Helm avec la nouvelle version du plug-in {{site.data.keyword.cos_full_notm}}](#install_cos).
{: note}

## Création de votre instance de service Object Storage
{: #create_cos_service}

Avant de commencer à utiliser du stockage d'objets dans votre cluster, vous devez mettre à disposition une instance de service {{site.data.keyword.cos_full_notm}} dans votre compte.
{: shortdesc}

Le plug-in {{site.data.keyword.cos_full_notm}} est configuré pour utiliser un noeud final d'API s3. Par exemple, vous envisagez peut-être d'utiliser un serveur Cloud Object Storage local, comme [Minio](https://cloud.ibm.com/kubernetes/solutions/helm-charts/ibm-charts/ibm-minio-objectstore), ou vous connecter à un noeud final d'API s3 que vous avez configuré sur un autre fournisseur de cloud au lieu d'utiliser une instance de service {{site.data.keyword.cos_full_notm}}.

Pour créer une instance de service {{site.data.keyword.cos_full_notm}}, procédez comme suit. Si vous envisagez d'utiliser un serveur Cloud Object Storage local ou un autre noeud final d'API s3, reportez-vous à la documentation du fournisseur pour configurer votre instance Cloud Object Storage.

1. Déployez une instance de service {{site.data.keyword.cos_full_notm}}.
   1.  Ouvrez la [page du catalogue {{site.data.keyword.cos_full_notm}}](https://cloud.ibm.com/catalog/services/cloud-object-storage).
   2.  Entrez un nom pour votre instance de service, tel que `cos-backup` et sélectionnez le même groupe de ressources que celui dans lequel figure votre cluster. Pour afficher le groupe de ressources de votre cluster, exécutez la commande `ibmcloud ks cluster-get --cluster <cluster_name_or_ID>`.   
   3.  Consultez les [options de plan ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api) pour prendre connaissance des informations de tarification et sélectionner un plan.
   4.  Cliquez sur **Créer**. La page des détails du service s'ouvre.
2. {: #service_credentials}Récupérez les données d'identification du service {{site.data.keyword.cos_full_notm}}.
   1.  Dans la navigation de la page des détails du service, cliquez sur **Données d'identification pour le service**.
   2.  Cliquez sur **Nouvelles données d'identification**. Une boîte de dialogue s'affiche.
   3.  Entrez un nom pour vos données d'identification.
   4.  Dans la liste déroulante **Rôle**, sélectionnez `Writer` ou `Manager`. Lorsque vous sélectionnez `Reader`, vous ne pouvez pas utiliser les données d'identification pour créer des compartiments dans {{site.data.keyword.cos_full_notm}} et écrire des données à l'intérieur.
   5.  Facultatif : dans la zone **Ajouter des paramètres de configuration en ligne (facultatif)**, entrez `{"HMAC":true}` pour créer des données d'identification HMAC pour le service {{site.data.keyword.cos_full_notm}}. L'authentification HMAC ajoute une couche supplémentaire de sécurité à l'authentification OAuth2 en empêchant l'utilisation incorrecte de jetons OAuth2 arrivés à expiration ou créés de manière aléatoire. **Important** : si vous disposez d'un cluster privé uniquement sans accès public, vous devez utiliser l'authentification HMAC pour avoir accès au service {{site.data.keyword.cos_full_notm}} sur le réseau privé.
   6.  Cliquez sur **Ajouter**. Vos nouvelles données d'identification sont répertoriées dans le tableau **Données d'identification pour le service**.
   7.  Cliquez sur **Afficher les données d'identification**.
   8.  Notez la clé d'API (**apikey**) afin d'utiliser des jetons OAuth2 pour l'authentification auprès du service {{site.data.keyword.cos_full_notm}}. Pour l'authentification HMAC, dans la section **cos_hmac_keys**, notez l'ID de la clé d'accès **access_key_id** et la clé **secret_access_key**.
3. [Stockez les données d'identification du service dans une valeur confidentielle Kubernetes dans le cluster](#create_cos_secret) pour activer l'accès à votre instance de service {{site.data.keyword.cos_full_notm}}.

## Création d'une valeur confidentielle pour les données d'identification du service Object Storage
{: #create_cos_secret}

Pour accéder à votre instance de service {{site.data.keyword.cos_full_notm}} pour lire et écrire des données, vous devez stocker les données d'identification de manière sécurisée dans une valeur confidentielle Kubernetes. Le plug-in {{site.data.keyword.cos_full_notm}} utilise ces données pour toute opération de lecture ou d'écriture dans votre compartiment.
{: shortdesc}

Pour créer une valeur confidentielle (secret) Kubernetes pour une instance de service {{site.data.keyword.cos_full_notm}}, procédez comme suit. Si vous envisagez d'utiliser un serveur Cloud Object Storage local ou un autre noeud final d'API s3, créez une valeur confidentielle Kubernetes avec les données d'identification appropriées.

Avant de commencer : [connectez-vous à votre compte. Le cas échéant, ciblez le groupe de ressources approprié. Définissez le contexte pour votre cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1. Récupérez la valeur **apikey** ou **access_key_id** et **secret_access_key** de vos [données d'identification pour le service {{site.data.keyword.cos_full_notm}}](#service_credentials).

2. Obtenez l'identificateur global unique (**GUID**) de votre instance de service {{site.data.keyword.cos_full_notm}}.
   ```
   ibmcloud resource service-instance <service_name> | grep GUID
   ```
   {: pre}

3. Créez un secret Kubernetes pour stocker vos données d'identification de service. Lorsque vous créez votre secret, toutes les valeurs sont automatiquement codées en base64.

   **Exemple d'utilisation de la clé d'API :**
   ```
   kubectl create secret generic cos-write-access --type=ibm/ibmc-s3fs --from-literal=api-key=<api_key> --from-literal=service-instance-id=<service_instance_guid>
   ```
   {: pre}

   **Exemple d'authentification HMAC :**
   ```
   kubectl create secret generic cos-write-access --type=ibm/ibmc-s3fs --from-literal=access-key=<access_key_ID> --from-literal=secret-key=<secret_access_key>    
   ```
   {: pre}

   <table>
   <caption>Description des composantes de la commande</caption>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composantes de la commande</th>
   </thead>
   <tbody>
   <tr>
   <td><code>api-key</code></td>
   <td>Entrez la clé d'API que vous avez extraite de vos données d'identification pour le service {{site.data.keyword.cos_full_notm}} auparavant. Pour utiliser l'authentification HMAC, indiquez à la place <code>access-key</code> et <code>secret-key</code>.  </td>
   </tr>
   <tr>
   <td><code>access-key</code></td>
   <td>Entrez l'ID de la clé d'accès que vous avez extrait de vos données d'identification pour le service {{site.data.keyword.cos_full_notm}} auparavant. Pour utiliser l'authentification OAuth2, indiquez <code>api-key</code> à la place.  </td>
   </tr>
   <tr>
   <td><code>secret-key</code></td>
   <td>Entrez la clé d'accès secrète que vous avez extraite de vos données d'identification pour le service {{site.data.keyword.cos_full_notm}} auparavant. Pour utiliser l'authentification OAuth2, indiquez <code>api-key</code> à la place.</td>
   </tr>
   <tr>
   <td><code>service-instance-id</code></td>
   <td>Entrez l'identificateur unique global (GUID) de votre instance de service {{site.data.keyword.cos_full_notm}} que vous avez extrait auparavant. </td>
   </tr>
   </tbody>
   </table>

4. Vérifiez que la valeur confidentielle a bien été créée dans votre espace de nom.
   ```
   kubectl get secret
   ```
   {: pre}

5. [Installez le plug-in {{site.data.keyword.cos_full_notm}}](#install_cos) ou, s'il est déjà installé, [déterminez la configuration]( #configure_cos) de votre compartiment {{site.data.keyword.cos_full_notm}}.

## Installation du plug-in IBM Cloud Object Storage
{: #install_cos}

Installez le plug-in {{site.data.keyword.cos_full_notm}} avec une charte Helm pour configurer des classes de stockage prédéfinies pour {{site.data.keyword.cos_full_notm}}. Vous pouvez utiliser ces classes de stockage pour créer une réservation de volume persistant afin de mettre à disposition {{site.data.keyword.cos_full_notm}} pour vos applications.
{: shortdesc}

Vous recherchez des instructions pour mettre à jour ou supprimer le plug-in {{site.data.keyword.cos_full_notm}} ? Voir [Mise à jour du plug-in](#update_cos_plugin) et [Retrait du plug-in](#remove_cos_plugin).
{: tip}

Avant de commencer : [connectez-vous à votre compte. Le cas échéant, ciblez le groupe de ressources approprié. Définissez le contexte pour votre cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1. Vérifiez que votre noeud worker applique le correctif le plus récent pour votre édition.
   1. Répertoriez la version actuelle de correctif de vos noeuds worker.
      ```
      ibmcloud ks workers --cluster <cluster_name_or_ID>
      ```
      {: pre}

      Exemple de sortie :
      ```
      OK
      ID                                                  Public IP        Private IP     Machine Type           State    Status   Zone    Version
      kube-dal10-crb1a23b456789ac1b20b2nc1e12b345ab-w26   169.xx.xxx.xxx    10.xxx.xx.xxx   b3c.4x16.encrypted     normal   Ready    dal10   1.13.6_1523*
      ```
      {: screen}

      Si votre noeud worker n'applique pas la dernière version de correctif, vous apercevez un astérisque (`*`) dans la colonne **Version** de la sortie de l'interface de ligne de commande.

   2. Consultez le [journal des modifications de version](/docs/containers?topic=containers-changelog#changelog) pour rechercher les modifications qui ont été apportées dans la dernière version de correctif.

   3. Appliquez la dernière version de correctif en rechargeant votre noeud worker. Suivez les instructions indiquées dans la [commande ibmcloud ks worker-reload](/docs/containers?topic=containers-cs_cli_reference#cs_worker_reload) pour replanifier correctement tous les pods en cours d'exécution sur votre noeud worker avant de le recharger. Notez que durant le rechargement, la machine de votre noeud worker est mise à jour avec l'image la plus récente et les données sont supprimées si elles ne sont pas [stockées hors du noeud worker](/docs/containers?topic=containers-storage_planning#persistent_storage_overview).

2.  Choisissez d'installer le plug-in {{site.data.keyword.cos_full_notm}} avec ou sans le serveur Helm, Tiller. Ensuite, [suivez les instructions](/docs/containers?topic=containers-helm#public_helm_install) pour installer le client Helm sur votre machine locale, et, si vous souhaitez, pour installer Tiller avec un compte de service dans votre cluster.

3. Si vous souhaitez installer le plug-in avec Tiller, assurez-vous que ce dernier est installé avec un compte de service. 
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

4. Ajoutez le référentiel Helm {{site.data.keyword.Bluemix_notm}} dans votre cluster :
   ```
   helm repo add iks-charts https://icr.io/helm/iks-charts
   ```
   {: pre}

4. Mettez à jour le référentiel Helm pour extraire la dernière version de toutes les chartes Helm figurant dans ce référentiel.
   ```
   helm repo update
   ```
   {: pre}

5. Téléchargez les chartes Helm et décompressez-les dans votre répertoire de travail.
   ```
   helm fetch --untar iks-charts/ibm-object-storage-plugin
   ```
   {: pre}

7. Si vous utilisez OS X ou une distribution Linux, installez le plug-in Helm `ibmc` d'{{site.data.keyword.cos_full_notm}}. Ce plug-in est utilisé pour extraire automatiquement l'emplacement de votre cluster et définir le noeud final d'API pour vos compartiments {{site.data.keyword.cos_full_notm}} dans vos classes de stockage. Si vous utilisez le système d'exploitation Windows, passez à l'étape suivante.
   1. Installez le plug-in Helm.
      ```
      helm plugin install ./ibm-object-storage-plugin/helm-ibmc
      ```
      {: pre}

      Exemple de sortie :
      ```
      Installed plugin: ibmc
      ```
      {: screen}
      
      Si le message d'erreur `Error: plugin already exists` s'affiche, retirez le plug-in Helm `ibmc` en exécutant la commande `rm -rf ~/.helm/plugins/helm-ibmc`.
      {: tip}

   2. Vérifiez que le plug-in `ibmc` est installé correctement.
      ```
      helm ibmc --help
      ```
      {: pre}

      Exemple de sortie :
      ```
      Install or upgrade Helm charts in IBM K8S Service(IKS) and IBM Cloud Private(ICP)

      Commandes disponibles :
         helm ibmc install [CHART][flags]                      Installer une charte Helm
         helm ibmc upgrade [RELEASE][CHART] [flags]            Mettre à niveau l'édition vers une nouvelle version de la charte Helm
         helm ibmc template [CHART][flags] [--apply|--delete]  Installer/Désinstaller une charte Helm sans tiller

      Indicateurs disponibles :
         -h, --help                    (Facultatif) Ce texte.
         -u, --update                  (Facultatif) Mettez à jour ce plugin vers la version la plus récente

      Exemple de syntaxe :
         Avec Tiller :
             Install:   helm ibmc install iks-charts/ibm-object-storage-plugin --name ibm-object-storage-plugin
         Sans Tiller :
             Install:   helm ibmc template iks-charts/ibm-object-storage-plugin --apply
             Dry-run:   helm ibmc template iks-charts/ibm-object-storage-plugin
             Uninstall: helm ibmc template iks-charts/ibm-object-storage-plugin --delete

      Remarque :
         1. Il est recommandé de toujours effectuer une installation vers la version la plus récente de la charte ibm-object-storage-plugin. 
         2. Il est recommandé de toujours disposer d'une mise à jour client de 'kubectl'.
      ```
      {: screen}

      Si la sortie comporte l'erreur `Error: fork/exec /home/iksadmin/.helm/plugins/helm-ibmc/ibmc.sh: permission denied`, exécutez la commande `chmod 755 ~/.helm/plugins/helm-ibmc/ibmc.sh`. Puis réexécutez la commande `helm ibmc --help`.
      {: tip}

8. Facultatif : Limitez l'accès du plug-in {{site.data.keyword.cos_full_notm}} uniquement aux valeurs confidentielles contenant vos données d'identification pour le service {{site.data.keyword.cos_full_notm}}. Par défaut, le plug-in est autorisé à accéder à toutes les valeurs confidentielles Kubernetes dans votre cluster.
   1. [Créez votre instance de service {{site.data.keyword.cos_full_notm}}](#create_cos_service).
   2. [Stockez vos données d'identification pour le service {{site.data.keyword.cos_full_notm}} dans une valeur confidentielle Kubernetes](#create_cos_secret).
   3. Accédez au répertoire `templates` et répertoriez les fichiers disponibles.  
      ```
      cd ibm-object-storage-plugin/templates && ls
      ```
      {: pre}

   4. Ouvrez le fichier `provisioner-sa.yaml` et recherchez la définition `ClusterRole` `ibmcloud-object-storage-secret-reader`.
   6. Ajoutez le nom de la valeur confidentielle que vous avez créée auparavant dans la liste des valeurs confidentielles auxquelles le plug-in est autorisé à accéder dans la section `resourceNames`.
      ```
      kind: ClusterRole
      apiVersion: rbac.authorization.k8s.io/v1beta1
      metadata:
        name: ibmcloud-object-storage-secret-reader
      rules:
      - apiGroups: [""]
        resources: ["secrets"]
        resourceNames: ["<secret_name1>","<secret_name2>"]
        verbs: ["get"]
      ```
      {: codeblock}
   7. Sauvegardez vos modifications.

9. Installez le plug-in {{site.data.keyword.cos_full_notm}}. Lorsque vous installez le plug-in, des classes de stockage prédéfinies sont ajoutées dans votre cluster.

   - **Pour OS X et Linux :**
     - Si vous avez ignoré l'étape précédente, effectuez l'installation sans vous limiter à l'utilisation de valeurs confidentielles Kubernetes spécifiques.</br>
       **Sans Tiller** :
       ```
       helm ibmc template iks-charts/ibm-object-storage-plugin --apply
       ```
       {: pre}
       
       **Avec Tiller** :
       ```
       helm ibmc install iks-charts/ibm-object-storage-plugin --name ibm-object-storage-plugin
       ```
       {: pre}

     - Si vous avez exécuté l'étape précédente, effectuez l'installation en vous limitant à des valeurs confidentielles Kubernetes spécifiques.</br>
**Sans Tiller** :
       ```
       cd ../..
       helm ibmc template ./ibm-object-storage-plugin --apply 
       ```
       {: pre}
       
       **Avec Tiller** :
       ```
       cd ../..
       helm ibmc install ./ibm-object-storage-plugin --name ibm-object-storage-plugin
       ```
       {: pre}

   - **Pour Windows :**
     1. Extrayez la zone dans laquelle est déployé votre cluster et stockez-la dans une variable d'environnement.
        ```
        export DC_NAME=$(kubectl get cm cluster-info -n kube-system -o jsonpath='{.data.cluster-config\.json}' | grep datacenter | awk -F ': ' '{print $2}' | sed 's/\"//g' |sed 's/,//g')
        ```
        {: pre}

     2. Vérifiez que la variable d'environnement est bien définie.
        ```
        printenv
        ```
        {: pre}

     3. Installez la charte Helm.
        - Si vous avez ignoré l'étape précédente, effectuez l'installation sans vous limiter à l'utilisation de valeurs confidentielles Kubernetes spécifiques.</br>
       **Sans Tiller** :
       ```
       helm ibmc template iks-charts/ibm-object-storage-plugin --apply
       ```
          {: pre}
          
          **Avec Tiller** :
       ```
          helm ibmc install iks-charts/ibm-object-storage-plugin --name ibm-object-storage-plugin
       ```
          {: pre}

        - Si vous avez exécuté l'étape précédente, effectuez l'installation en vous limitant à des valeurs confidentielles Kubernetes spécifiques.</br>
**Sans Tiller** :
       ```
cd ../..
          helm ibmc template ./ibm-object-storage-plugin --apply
       ```
          {: pre}
          
          **Avec Tiller** :
       ```
          cd ../..
          helm ibmc install ./ibm-object-storage-plugin --name ibm-object-storage-plugin
       ```
          {: pre}


   Exemple de sortie pour une installation sans Tiller :
   ```
   Rendering the Helm chart templates...
   DC: dal10
   Chart: iks-charts/ibm-object-storage-plugin
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-cold-cross-region.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-cold-regional.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-flex-cross-region.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-flex-perf-cross-region.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-flex-perf-regional.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-flex-regional.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-standard-cross-region.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-standard-perf-cross-region.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-standard-perf-regional.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-standard-regional.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-vault-cross-region.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-vault-regional.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/flex-driver-sa.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/provisioner-sa.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/flex-driver.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/tests/check-driver-install.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/provisioner.yaml
   Installing the Helm chart...
   serviceaccount/ibmcloud-object-storage-driver created
   daemonset.apps/ibmcloud-object-storage-driver created
   storageclass.storage.k8s.io/ibmc-s3fs-cold-cross-region created
   storageclass.storage.k8s.io/ibmc-s3fs-cold-regional created
   storageclass.storage.k8s.io/ibmc-s3fs-flex-cross-region created
   storageclass.storage.k8s.io/ibmc-s3fs-flex-perf-cross-region created
   storageclass.storage.k8s.io/ibmc-s3fs-flex-perf-regional created
   storageclass.storage.k8s.io/ibmc-s3fs-flex-regional created
   storageclass.storage.k8s.io/ibmc-s3fs-standard-cross-region created
   storageclass.storage.k8s.io/ibmc-s3fs-standard-perf-cross-region created
   storageclass.storage.k8s.io/ibmc-s3fs-standard-perf-regional created
   storageclass.storage.k8s.io/ibmc-s3fs-standard-regional created
   storageclass.storage.k8s.io/ibmc-s3fs-vault-cross-region created
   storageclass.storage.k8s.io/ibmc-s3fs-vault-regional created
   serviceaccount/ibmcloud-object-storage-plugin created
   clusterrole.rbac.authorization.k8s.io/ibmcloud-object-storage-plugin created
   clusterrole.rbac.authorization.k8s.io/ibmcloud-object-storage-secret-reader created
   clusterrolebinding.rbac.authorization.k8s.io/ibmcloud-object-storage-plugin created
   clusterrolebinding.rbac.authorization.k8s.io/ibmcloud-object-storage-secret-reader created
   deployment.apps/ibmcloud-object-storage-plugin created
   pod/ibmcloud-object-storage-driver-test created
   ```
   {: screen}

10. Vérifiez que le plug-in est installé correctement.
    ```
    kubectl get pod -n kube-system -o wide | grep object
    ```
    {: pre}

    Exemple de sortie :
    ```
    ibmcloud-object-storage-driver-9n8g8                              1/1       Running   0          2m
   ibmcloud-object-storage-plugin-7c774d484b-pcnnx                   1/1       Running   0          2m
    ```
    {: screen}

    L'installation a abouti lorsque vous voyez un pod `ibmcloud-object-storage-plugin` et un ou plusieurs pods `ibmcloud-object-storage-driver`. Le nombre de pods `ibmcloud-object-storage-driver` est égal au nombre de noeuds worker figurant dans votre cluster. Tous les pods doivent être à l'état `Running` pour que le plug-in fonctionne correctement. En cas de défaillance du pod, exécutez `kubectl describe pod -n kube-system <pod_name>` pour rechercher la cause première du problème.

11. Vérifiez que la création des classes de stockage a abouti.
    ```
    kubectl get storageclass | grep s3
    ```
    {: pre}

    Exemple de sortie :
    ```
    ibmc-s3fs-cold-cross-region            ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-cold-regional                ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-flex-cross-region            ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-flex-perf-cross-region       ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-flex-perf-regional           ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-flex-regional                ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-standard-cross-region        ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-standard-perf-cross-region   ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-standard-perf-regional       ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-standard-regional            ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-vault-cross-region           ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-vault-regional               ibm.io/ibmc-s3fs   8m
    ```
    {: screen}

12. Répétez ces étapes pour tous les clusters sur lesquels vous voulez accéder à des compartiments {{site.data.keyword.cos_full_notm}}.

### Mise à jour du plug-in IBM Cloud Object Storage
{: #update_cos_plugin}

Vous pouvez mettre à niveau le plug-in {{site.data.keyword.cos_full_notm}} existant à la version la plus récente.
{: shortdesc}

1. Si vous aviez déjà installé la version 1.0.4 ou une version antérieure de la charte Helm nommée `ibmcloud-object-storage-plugin`, retirez cette installation Helm de votre cluster. Ensuite, réinstaller la charte Helm. 
   1. Vérifiez si l'ancienne version de la charte Helm {{site.data.keyword.cos_full_notm}} est installée dans votre cluster.  
      ```
      helm ls | grep ibmcloud-object-storage-plugin
      ```
      {: pre}

      Exemple de sortie :
      ```
      ibmcloud-object-storage-plugin	1       	Mon Sep 18 15:31:40 2017	DEPLOYED	ibmcloud-object-storage-plugin-1.0.4	default
      ```
      {: screen}

   2. Si vous disposez de la version 1.0.4 ou d'une version antérieure de la charte Helm nommée `ibmcloud-object-storage-plugin`, retirez cette chart Helm de votre cluster. Si vous disposez de la version 1.0.5 ou d'une version ultérieure de la charte Helm nommée `ibm-object-storage-plugin`, passez à l'étape 2.
      ```
      helm delete --purge ibmcloud-object-storage-plugin
      ```
      {: pre}

   3. Suivez les étapes décrites dans la rubrique [Installation du plug-in {{site.data.keyword.cos_full_notm}}](#install_cos) pour installer la dernière version du plug-in {{site.data.keyword.cos_full_notm}}. 

2. Mettez à jour le référentiel Helm d'{{site.data.keyword.Bluemix_notm}} pour extraire la dernière version de toutes les chartes Helm figurant dans ce référentiel.
   ```
   helm repo update
   ```
   {: pre}

3. Si vous utilisez OS X ou une distribution Linux, mettez à jour le plug-in Helm `ibmc` d'{{site.data.keyword.cos_full_notm}} vers la version la plus récente. 
   ```
   helm ibmc --update
   ```
   {: pre}

4. Téléchargez la charte Helm {{site.data.keyword.cos_full_notm}} la plus récente sur votre machine locale et extrayez le package pour consulter le fichier `release.md` afin de récupérer les dernières informations de version.
   ```
   helm fetch --untar iks-charts/ibm-object-storage-plugin
   ```
   {: pre}

5. Mettez à niveau le plug-in. </br>
   **Sans Tiller** :
        
   ```
   helm ibmc template iks-charts/ibm-object-storage-plugin --update
   ```
   {: pre}
     
   **Avec Tiller** :
        
   1. Recherchez le nom d'installation de votre charte Helm.
      ```
      helm ls | grep ibm-object-storage-plugin
      ```
      {: pre}

      Exemple de sortie :
      ```
      <helm_chart_name> 	1       	Mon Sep 18 15:31:40 2017	DEPLOYED	ibm-object-storage-plugin-1.0.5	default
      ```
      {: screen}

   2. Mettez à niveau la charte Helm {{site.data.keyword.cos_full_notm}} à la version la plus récente.
      ```   
      helm ibmc upgrade <helm_chart_name> iks-charts/ibm-object-storage-plugin --force --recreate-pods -f
      ```
      {: pre}

6. Vérifiez que la mise à niveau du pod `ibmcloud-object-storage-plugin` a abouti.  
   ```
   kubectl rollout status deployment/ibmcloud-object-storage-plugin -n kube-system
   ```
   {: pre}

   La mise à niveau du plug-in a abouti lorsque vous voyez le message `deployment "ibmcloud-object-storage-plugin" successfully rolled out` dans la sortie de l'interface de ligne de commande.

7. Vérifiez que la mise à niveau du pod `ibmcloud-object-storage-driver` a abouti.
   ```
   kubectl rollout status ds/ibmcloud-object-storage-driver -n kube-system
   ```
   {: pre}

   La mise à niveau a abouti lorsque vous voyez le message `daemon "ibmcloud-object-storage-driver" successfully rolled out` dans la sortie de l'interface de ligne de commande.

8. Vérifiez que les pods {{site.data.keyword.cos_full_notm}} sont à l'état `Running`.
   ```
   kubectl get pods -n kube-system -o wide | grep object-storage
   ```
   {: pre}


### Retrait du plug-in IBM Cloud Object Storage
{: #remove_cos_plugin}

Si vous ne souhaitez pas mettre à disposition et utiliser {{site.data.keyword.cos_full_notm}} dans votre cluster, vous pouvez désinstaller le plug-in.
{: shortdesc}

Le retrait du plug-in ne retire pas les réservations de volume persistant (PVC), les volumes persistants (PV) ou les données. Lorsque vous retirez le plug-in, tous les pods associés et les ensembles de démons sont retirés de votre cluster. Vous ne pouvez pas mettre à disposition une autre instance d'{{site.data.keyword.cos_full_notm}} pour votre cluster ou utiliser des réservations de volume persistants et des volumes persistants après avoir retiré le plug-in, sauf si vous configurez votre application pour utiliser directement l'API {{site.data.keyword.cos_full_notm}}.
{: important}

Avant de commencer :

- [Ciblez votre interface CLI sur le cluster](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).
- Vérifiez que vous n'avez pas de réservations de volume persistant ou de volumes persistants utilisant {{site.data.keyword.cos_full_notm}}. Pour répertorier tous les pods utilisés pour le montage d'une PVC spécifique, exécutez la commande `kubectl get pods --all-namespaces -o=jsonpath='{range .items[*]}{"\n"}{.metadata.name}{":\t"}{range .spec.volumes[*]}{.persistentVolumeClaim.claimName}{" "}{end}{end}' | grep "<pvc_name>"`.

Pour retirer le plug-in :

1. Retirez le plug-in de votre cluster. </br>
   **Avec Tiller** :
        
   1. Recherchez le nom d'installation de votre charte Helm.
      ```
      helm ls | grep object-storage-plugin
      ```
      {: pre}

      Exemple de sortie :
      ```
      <helm_chart_name> 	1       	Mon Sep 18 15:31:40 2017	DEPLOYED	ibmcloud-object-storage-plugin-1.0.0	default
      ```
      {: screen}

   2. Supprimez le plug-in {{site.data.keyword.cos_full_notm}} en retirant la charte Helm.
      ```
      helm delete --purge <helm_chart_name>
      ```
      {: pre}

   **Sans Tiller** :        
   ```
   helm ibmc template iks-charts/ibm-object-storage-plugin --delete
   ```
   {: pre}

2. Vérifiez que les pods {{site.data.keyword.cos_full_notm}} sont retirés.
   ```
   kubectl get pod -n kube-system | grep object-storage
   ```
   {: pre}

   Le retrait des pods a abouti lorsqu'aucun pod n'est affiché dans la sortie de votre interface de ligne de commande.

3. Vérifiez que les classes de stockage sont bien retirées.
   ```
   kubectl get storageclasses | grep s3
   ```
   {: pre}

   Le retrait des classes de stockage a abouti lorsqu'aucune classe de stockage n'est affichée dans la sortie de votre interface de ligne de commande.

4. Si vous utilisez OS X ou une distribution Linux, retirez le plug-in Helm `ibmc`. Si vous utilisez Windows, cette étape n'est pas nécessaire.
   1. Retirez le plug-in `ibmc`.
      ```
      rm -rf ~/.helm/plugins/helm-ibmc
      ```
      {: pre}

   2. Vérifiez que le plug-in `ibmc` est bien retiré.
      ```
      helm plugin list
      ```
      {: pre}

      Exemple de sortie :
     ```
     NAME	VERSION	DESCRIPTION
     ```
     {: screen}

     Le retrait du plug-in `ibmc` a abouti si le plug-in `ibmc` n'est pas répertorié dans la sortie de l'interface de ligne de commande.


## Détermination de la configuration de stockage d'objets
{: #configure_cos}

{{site.data.keyword.containerlong_notm}} fournit des classes de stockage prédéfinies que vous pouvez utiliser pour créer des compartiments avec une configuration spécifique.
{: shortdesc}

1. Répertoriez les classes de stockage disponibles dans {{site.data.keyword.containerlong_notm}}.
   ```
   kubectl get storageclasses | grep s3
   ```
   {: pre}

   Exemple de sortie :
   ```
   ibmc-s3fs-cold-cross-region            ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-cold-regional                ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-flex-cross-region            ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-flex-perf-cross-region       ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-flex-perf-regional           ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-flex-regional                ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-standard-cross-region        ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-standard-perf-cross-region   ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-standard-perf-regional       ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-standard-regional            ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-vault-cross-region           ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-vault-regional               ibm.io/ibmc-s3fs   8m
   ```
   {: screen}

2. Sélectionnez une classe de stockage qui convient à vos exigences en matière d'accès aux données. La classe de stockage détermine la [tarification ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api) pour la capacité de stockage, les opérations de lecture et d'écriture, ainsi que la bande passante sortante d'un compartiment. L'option qui vous convient est fonction de la fréquence de lecture et d'écriture des données sur votre instance de service.
   - **Standard** : option utilisée pour les données les plus sollicitées auxquelles l'accès est fréquent. Les cas d'utilisation courants sont les applications Web ou mobiles.
   - **Vault** : option utilisée pour les charges de travail ou les données dont l'accès n'est pas fréquent, par exemple une fois par mois ou moins. Les cas d'utilisation courants sont les archives, la conservation à court-terme des données, la conservation de documents numériques, le remplacement d'une bande et la reprise après incident.
   - **Cold** : option utilisée pour les données les moins sollicitées qui font l'objet d'un accès peu fréquent (tous les 90 jours ou moins) ou les données inactives. Les cas d'utilisation courants sont les archives, les sauvegardes à long terme, les données d'historique que vous conservez à des fins de conformité ou les charges de travail ou les applications auxquelles l'accès est plutôt rare.
   - **Flex** : option utilisée pour les charges de travail et les données qui ne suivent pas un mode d'utilisation particulier ou qui sont beaucoup trop volumineuses pour déterminer ou prévoir un mode d'utilisation particulier. **Astuce :** consultez ce [blogue ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/blogs/bluemix/2017/03/interconnect-2017-changing-rules-storage/) pour en savoir plus sur le fonctionnement de la classe de stockage Flex comparée aux autres classes de stockage classiques.   

3. Déterminez le niveau de résilience des données stockées dans votre compartiment.
   - **Cross-region** : avec cette option, vos données sont stockées entre trois régions au sein d'une géolocalisation pour assurer une haute disponibilité. Si vous disposez de charges de travail réparties sur plusieurs régions, les demandes sont acheminées vers le noeud final régional le plus proche. Le noeud final d'API correspondant à la géolocalisation est automatiquement défini par le plug-in Helm `ibmc` que vous avez installé précédemment en fonction de l'emplacement de votre cluster. Par exemple, si votre cluster se trouve dans la région du Sud des Etats-Unis (`US South`), vos classes de stockage sont configurées pour utiliser le noeud final d'API `US GEO` pour vos compartiments. Voir [Régions et noeuds finaux](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-endpoints#endpoints) pour plus d'informations.  
   - **Regional** : avec cette option, vos données sont répliquées entre plusieurs zones au sein d'une région. Si vous disposez de charges de travail situées dans la même région, vous observez des temps d'attente plus faibles et de meilleures performances qu'avec une configuration inter-régionale. Le noeud final régional est automatiquement défini par le plug-in Helm `ibmc` que vous avez installé auparavant en fonction de l'emplacement de votre cluster. Par exemple, si votre cluster se trouve dans la région du Sud des Etats-Unis (`US South`), vos classes de stockage ont été configurées pour utiliser le noeud final d'API `US South` pour vos compartiments. Voir [Régions et noeuds finaux](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-endpoints#endpoints) pour plus d'informations.

4. Passez en revue la configuration détaillée d'un compartiment {{site.data.keyword.cos_full_notm}} pour une classe de stockage.
   ```
   kubectl describe storageclass <storageclass_name>
   ```
   {: pre}

   Exemple de sortie :
   ```
   Name:                  ibmc-s3fs-standard-cross-region
   IsDefaultClass:        No
   Annotations:           <none>
   Provisioner:           ibm.io/ibmc-s3fs
   Parameters:            ibm.io/chunk-size-mb=16,ibm.io/curl-debug=false,ibm.io/debug-level=warn,ibm.io/iam-endpoint=https://iam.bluemix.net,ibm.io/kernel-cache=true,ibm.io/multireq-max=20,ibm.io/object-store-endpoint=https://s3-api.dal-us-geo.objectstorage.service.networklayer.com,ibm.io/object-store-storage-class=us-standard,ibm.io/parallel-count=2,ibm.io/s3fs-fuse-retry-count=5,ibm.io/stat-cache-size=100000,ibm.io/tls-cipher-suite=AESGCM
   AllowVolumeExpansion:  <unset>
   MountOptions:          <none>
   ReclaimPolicy:         Delete
   VolumeBindingMode:     Immediate
   Events:                <none>
   ```
   {: screen}

   <table>
   <caption>Description des détails d'une classe de stockage</caption>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants du fichier YAML</th>
   </thead>
   <tbody>
   <tr>
   <td><code>ibm.io/chunk-size-mb</code></td>
   <td>Taille d'un bloc de données dont la lecture ou l'écriture s'effectue dans {{site.data.keyword.cos_full_notm}} exprimée en mégaoctets. Les classes de stockage dont le nom contient <code>perf</code> sont configurées avec 52 mégaoctets. Les classes de stockage dont le nom ne contient pas <code>perf</code> utilisent des blocs de 16 mégaoctets. Par exemple, pour lire un fichier d'une taille de 1 Go, le plug-in lit ce fichier par blocs de 16 ou 52 mégaoctets. </td>
   </tr>
   <tr>
   <td><code>ibm.io/curl-debug</code></td>
   <td>Active la consignation des demandes envoyées à l'instance de service {{site.data.keyword.cos_full_notm}}. Si cette option est activée, les journaux sont envoyés à `syslog` et vous pouvez [transférer les journaux à un serveur de consignation externe](/docs/containers?topic=containers-health#logging). Par défaut, toutes les classes de stockage sont définies sur <strong>false</strong> pour désactiver cette fonction de consignation. </td>
   </tr>
   <tr>
   <td><code>ibm.io/debug-level</code></td>
   <td>Niveau de consignation défini par le plug-in {{site.data.keyword.cos_full_notm}}. Toutes les classes de stockage sont définies avec le niveau de consignation <strong>WARN</strong>. </td>
   </tr>
   <tr>
   <td><code>ibm.io/iam-endpoint</code></td>
   <td>Noeud final d'API pour {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM). </td>
   </tr>
   <tr>
   <td><code>ibm.io/kernel-cache</code></td>
   <td>Active ou désactive le cache de mémoire tampon du noyau pour le point de montage du volume. Si cette option est activée, les données lues dans {{site.data.keyword.cos_full_notm}} sont stockées dans le cache du noyau pour garantir un accès en lecture rapide à vos données. Si cette option est désactivée, les données ne sont pas mises en cache et sont toujours lues à partir d'{{site.data.keyword.cos_full_notm}}. Le cache du noyau est activé pour les classes de stockage <code>standard</code> et <code>flex</code> et désactivé pour les classes de stockage <code>cold</code> et <code>vault</code>. </td>
   </tr>
   <tr>
   <td><code>ibm.io/multireq-max</code></td>
   <td>Nombre maximal de demandes parallèles pouvant être envoyées à l'instance de service {{site.data.keyword.cos_full_notm}} pour répertorier les fichiers d'un répertoire. Toutes les classes de stockage sont configurées avec un maximum de 20 demandes parallèles.  </td>
   </tr>
   <tr>
   <td><code>ibm.io/object-store-endpoint</code></td>
   <td>Noeud final d'API à utiliser pour accéder au compartiment dans votre instance de service {{site.data.keyword.cos_full_notm}}. Le noeud final est automatiquement défini en fonction de la région dans laquelle se trouve votre cluster. **Remarque** : si vous désirez accéder à un compartiment existant situé dans une autre région que celle de votre cluster, vous devez créer une [classe de stockage personnalisée](/docs/containers?topic=containers-kube_concepts#customized_storageclass) et utiliser le noeud final d'API correspondant à votre compartiment.</td>
   </tr>
   <tr>
   <td><code>ibm.io/object-store-storage-class</code></td>
   <td>Nom de la classe de stockage. </td>
   </tr>
   <tr>
   <td><code>ibm.io/parallel-count</code></td>
   <td>Nombre maximal de demandes parallèles pouvant être envoyées à l'instance de service {{site.data.keyword.cos_full_notm}} pour une seule opération de lecture ou d'écriture. Les classes de stockage dont le nom contient <code>perf</code> sont configurées avec un maximum de 20 demandes parallèles. Les classes de stockage dont le nom ne contient pas <code>perf</code> sont configurées avec 2 demandes parallèles par défaut.  </td>
   </tr>
   <tr>
   <td><code>ibm.io/s3fs-fuse-retry-count</code></td>
   <td>Nombre maximal de relances d'une opération de lecture ou d'écriture avant de considérer qu'une opération a échoué. Toutes les classes de stockage sont configurées avec un maximum de 5 relances.  </td>
   </tr>
   <tr>
   <td><code>ibm.io/stat-cache-size</code></td>
   <td>Nombre maximal d'enregistrements conservés dans le cache de métadonnées d'{{site.data.keyword.cos_full_notm}}. Tous les enregistrements peuvent avoir jusqu'à 0,5 kilooctet. Toutes les classes de stockage définissent le nombre maximal d'enregistrements à 100000 par défaut. </td>
   </tr>
   <tr>
   <td><code>ibm.io/tls-cipher-suite</code></td>
   <td>Suite de chiffrement TLS qui doit être utilisée lorsqu'une connexion à {{site.data.keyword.cos_full_notm}} est établie via le noeud final HTTPS. La valeur de la suite de chiffrement doit être conforme au [format OpenSSL ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.openssl.org/docs/man1.0.2/apps/ciphers.html). Par défaut, toutes les classes de stockage utilisent la suite de chiffrement <strong><code>AESGCM</code></strong>.  </td>
   </tr>
   </tbody>
   </table>

   Pour plus d'informations sur chaque classe de stockage, voir [Référence des classes de stockage](#cos_storageclass_reference). Si vous souhaitez modifier l'une des valeurs prédéfinies, créez votre propre [classe de stockage personnalisée](/docs/containers?topic=containers-kube_concepts#customized_storageclass).
   {: tip}

5. Déterminez le nom de votre compartiment. Ce nom doit être unique dans {{site.data.keyword.cos_full_notm}}. Vous pouvez également opter pour la création automatique d'un nom pour votre compartiment par le plug-in {{site.data.keyword.cos_full_notm}}. Pour organiser les données dans un compartiment, vous pouvez créer des sous-répertoires.

   La classe de stockage que vous avez choisie auparavant détermine la tarification de la totalité de votre compartiment. Vous ne pouvez pas définir des classes de stockage différentes pour les sous-répertoires. Si vous souhaitez stocker les données avec différentes exigences en termes d'accès, envisagez la création de plusieurs compartiments en utilisant plusieurs réservations de volume persistant (PVC).
   {: note}

6. Déterminez si vous voulez conserver vos données ainsi que le compartiment, après la suppression du cluster ou de la réservation de volume persistant (PVC). Lorsque vous supprimez la PVC, le volume persistant est toujours supprimé. Vous pouvez déterminer si vous souhaitez également supprimer les données et le compartiment lorsque vous supprimez la PVC. Votre instance de service {{site.data.keyword.cos_full_notm}} est indépendante de la règle de conservation que vous sélectionnez pour vos données et n'est jamais retirée lorsque vous supprimez une PVC.

Dès que vous avez déterminé la configuration que vous désirez, vous êtes prêt à [créer une PVC](#add_cos) pour mettre à disposition {{site.data.keyword.cos_full_notm}}.

## Ajout d'Object Storage à vos applications
{: #add_cos}

Créez une réservation de volume persistant (PVC) pour mettre à disposition {{site.data.keyword.cos_full_notm}} pour votre cluster.
{: shortdesc}

En fonction des paramètres que vous choisissez dans votre PVC, vous pouvez mettre à disposition {{site.data.keyword.cos_full_notm}} ainsi :
- [Provisionnement dynamique](/docs/containers?topic=containers-kube_concepts#dynamic_provisioning) : lorsque vous créez la PVC, le volume persistant (PV) correspondant et le compartiment figurant dans votre instance de service {{site.data.keyword.cos_full_notm}} sont créés automatiquement.
- [Provisionnement statique](/docs/containers?topic=containers-kube_concepts#static_provisioning) : vous pouvez référencer un compartiment existant dans votre instance de service {{site.data.keyword.cos_full_notm}} au sein de la PVC. Lorsque vous créez la PVC, le volume persistant correspondant est automatiquement créé et lié à votre compartiment existant dans {{site.data.keyword.cos_full_notm}}.

Avant de commencer :
- [Créez et préparez votre instance de service {{site.data.keyword.cos_full_notm}}](#create_cos_service).
- [Créez une valeur confidentielle pour stocker vos données d'identification pour le service {{site.data.keyword.cos_full_notm}}](#create_cos_secret).
- [Déterminez la configuration à adopter pour {{site.data.keyword.cos_full_notm}}](#configure_cos).

Pour ajouter {{site.data.keyword.cos_full_notm}} dans votre cluster :

1. Créez un fichier de configuration pour définir votre réservation de volume persistant (PVC).
   ```
   kind: PersistentVolumeClaim
   apiVersion: v1
   metadata:
     name: <pvc_name>
     namespace: <namespace>
     annotations:
       ibm.io/auto-create-bucket: "<true_or_false>"
       ibm.io/auto-delete-bucket: "<true_or_false>"
       ibm.io/bucket: "<bucket_name>"
       ibm.io/object-path: "<bucket_subdirectory>"
       ibm.io/secret-name: "<secret_name>"
       ibm.io/endpoint: "https://<s3fs_service_endpoint>"
   spec:
     accessModes:
       - ReadWriteOnce
     resources:
       requests:
         storage: 8Gi # Enter a fictitious value
     storageClassName: <storage_class>
   ```
   {: codeblock}

   <table>
   <caption>Description des composants du fichier YAML</caption>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants du fichier YAML</th>
   </thead>
   <tbody>
   <tr>
   <td><code>metadata.name</code></td>
   <td>Entrez le nom de la réservation de volume persistant (PVC).</td>
   </tr>
   <tr>
   <td><code>metadata.namespace</code></td>
   <td>Entrez l'espace de nom dans lequel vous désirez créer la réservation de volume persistant (PVC). La PVC doit être créée dans le même espace de nom où vous avez créé la valeur confidentielle Kubernetes afin d'enregistrer vos données d'identification pour le service {{site.data.keyword.cos_full_notm}} et où vous voulez exécuter votre pod. </td>
   </tr>
   <tr>
   <td><code>ibm.io/auto-create-bucket</code></td>
   <td>Sélectionnez l'une des options suivantes : <ul><li><strong>true</strong> : lorsque vous créez la PVC, le volume persistant et le compartiment dans votre instance de service {{site.data.keyword.cos_full_notm}} sont automatiquement créés. Choisissez cette option pour créer un nouveau compartiment dans votre instance de service {{site.data.keyword.cos_full_notm}}. </li><li><strong>false</strong> : choisissez cette option pour accéder aux données d'un compartiment existant. Lorsque vous créez la PVC, le volume persistant est automatiquement créé et lié au compartiment que vous spécifiez dans <code>ibm.io/bucket</code>.</td>
   </tr>
   <tr>
   <td><code>ibm.io/auto-delete-bucket</code></td>
   <td>Sélectionnez l'une des options suivantes : <ul><li><strong>true</strong> : vos données, le compartiment et le volume persistant sont automatiquement supprimés en même temps que la PVC. Il reste votre instance de service {{site.data.keyword.cos_full_notm}} qui n'est pas supprimée. Si vous choisissez de définir cette option avec la valeur <strong>true</strong>, vous devez définir <code>ibm.io/auto-create-bucket: true</code> et <code>ibm.io/bucket: ""</code> pour que votre compartiment soit automatiquement créé avec un nom au format <code>tmp-s3fs-xxxx</code>. </li><li><strong>false</strong> : lorsque vous supprimez la PVC, le volume persistant est automatiquement supprimé mais vos données et le compartiment figurant dans votre instance de service {{site.data.keyword.cos_full_notm}} sont conservés. Pour accéder à vos données, vous devez créer une nouvelle PVC avec le nom de votre compartiment existant. </li></ul>
   <tr>
   <td><code>ibm.io/bucket</code></td>
   <td>Sélectionnez l'une des options suivantes : <ul><li>si <code>ibm.io/auto-create-bucket</code> a la valeur <strong>true</strong> : entrez le nom du compartiment que vous voulez créer dans {{site.data.keyword.cos_full_notm}}. De plus, si <code>ibm.io/auto-delete-bucket</code> a la valeur <strong>true</strong>, vous devez laisser cette zone vide pour affecter automatiquement un nom à votre compartiment au format <code>tmp-s3fs-xxxx</code>. Ce nom doit être unique dans {{site.data.keyword.cos_full_notm}}. </li><li>Si <code>ibm.io/auto-create-bucket</code> a la valeur <strong>false</strong> : entrez le nom du compartiment existant auquel vous souhaitez accéder dans le cluster </li></ul> </td>
   </tr>
   <tr>
   <td><code>ibm.io/object-path</code></td>
   <td>Facultatif : entrez le nom du sous-répertoire présent dans votre compartiment que vous voulez monter. Utilisez cette option pour monter uniquement un sous-répertoire et non le compartiment dans son ensemble. Pour monter un sous-répertoire, vous devez définir <code>ibm.io/auto-create-bucket: "false"</code> et fournir le nom du compartiment dans <code>ibm.io/bucket</code>. </li></ul> </td>
   </tr>
   <tr>
   <td><code>ibm.io/secret-name</code></td>
   <td>Entrez le nom de la valeur confidentielle qui contient les données d'identification {{site.data.keyword.cos_full_notm}} que vous avez créées précédemment. </td>
   </tr>
   <tr>
  <td><code>ibm.io/endpoint</code></td>
  <td>Si vous avez créé votre instance de service {{site.data.keyword.cos_full_notm}} dans un autre emplacement que votre cluster, entrez le noeud final de service privé ou public de votre instance de service {{site.data.keyword.cos_full_notm}} que vous souhaitez utiliser. Pour obtenir une présentation des noeuds finaux de service disponibles, voir [Informations supplémentaires sur les noeuds finaux](/docs/services/cloud-object-storage?topic=cloud-object-storage-advanced-endpoints). Par défaut, le plug-in Helm <code>ibmc</code> extrait automatiquement l'emplacement de votre cluster et crée les classes de stockage à l'aide du noeud final de service privé {{site.data.keyword.cos_full_notm}} correspondant à l'emplacement de votre cluster. Si le cluster correspond à une zone d'agglomération, telle que `dal10`, le noeud final de service privé {{site.data.keyword.cos_full_notm}} pour l'agglomération, en l'occurrence, Dallas, est utilisé. Pour vérifier que le noeud final de service dans vos classes de stockage correspond au noeud final de service de votre instance de service, exécutez la commande `kubectl describe storageclass <storageclassname>`. Prenez soin d'entre votre noeud final de service au format `https://<s3fs_private_service_endpoint>` pour les noeuds finaux de service privé ou au format `http://<s3fs_public_service_endpoint>` pour les noeuds finaux de service public. Si le noeud final de service dans votre classe de stockage correspond au noeud final de service de votre instance de service {{site.data.keyword.cos_full_notm}}, n'incluez pas l'option <code>ibm.io/endpoint</code> dans votre fichier YAML de réservation de volume persistant. </td>
  </tr>
   <tr>
   <td><code>resources.requests.storage</code></td>
   <td>Taille fictive de votre compartiment {{site.data.keyword.cos_full_notm}} en gigaoctets. La taille est requise par Kubernetes, mais n'est pas respectée dans {{site.data.keyword.cos_full_notm}}. Vous pouvez entrer la taille de votre choix. L'espace réel que vous utilisez dans {{site.data.keyword.cos_full_notm}} peut être différent et est facturé en fonction du [tableau de tarification ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api). </td>
   </tr>
   <tr>
   <td><code>spec.storageClassName</code></td>
   <td>Sélectionnez l'une des options suivantes : <ul><li>Si <code>ibm.io/auto-create-bucket</code> a la valeur <strong>true</strong> : entrez la classe de stockage que vous désirez utiliser pour votre nouveau compartiment. </li><li>Si <code>ibm.io/auto-create-bucket</code> a la valeur <strong>false</strong> : entrez la classe de stockage que vous avez utilisée pour créer votre compartiment existant. </br></br>Si vous avez créé votre compartiment manuellement dans votre instance de service {{site.data.keyword.cos_full_notm}} et que vous ne vous souvenez pas de la classe de stockage que vous avez utilisée, recherchez votre instance de service dans le tableau de bord {{site.data.keyword.Bluemix}} et passez en revue les zones <strong>Classe</strong> et <strong>Emplacement</strong> de votre compartiment existant. Utilisez ensuite la [classe de stockage](#cos_storageclass_reference) appropriée. <p class="note">Le noeud final d'API {{site.data.keyword.cos_full_notm}} défini dans votre classe de stockage est basé sur la région dans laquelle se trouve votre cluster. Pour accéder à un compartiment situé dans une autre région que celle de votre cluster, vous devez créer une [classe de stockage personnalisée](/docs/containers?topic=containers-kube_concepts#customized_storageclass) et utiliser le noeud final d'API correspondant à votre compartiment.</p></li></ul>  </td>
   </tr>
   </tbody>
   </table>

2. Créez la réservation de volume persistant (PVC).
   ```
   kubectl apply -f filepath/pvc.yaml
   ```
   {: pre}

3. Vérifiez que votre PVC est créée et liée au volume persistant (PV).
   ```
   kubectl get pvc
   ```
   {: pre}

   Exemple de sortie :
   ```
   NAME                  STATUS    VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS                     AGE
   s3fs-test-pvc         Bound     pvc-b38b30f9-1234-11e8-ad2b-t910456jbe12   8Gi        RWO            ibmc-s3fs-standard-cross-region  1h
   ```
   {: screen}

4. Facultatif : si vous envisagez d'accéder à vos données avec un utilisateur non root, ou d'ajouter des fichiers à un compartiment {{site.data.keyword.cos_full_notm}} existant en utilisant directement la console ou l'API, vérifiez que les [droits appropriés sont affectés aux fichiers](/docs/containers?topic=containers-cs_troubleshoot_storage#cos_nonroot_access) pour que votre application puisse lire et mettre à jour les fichiers comme il convient.

4.  {: #cos_app_volume_mount}Pour monter le volume persistant (PV) sur votre déploiement, créez un fichier de configuration `.yaml` et spécifiez la réservation de volume persistant (PVC) associée au PV.

    ```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: <deployment_name>
      labels:
        app: <deployment_label>
    spec:
      selector:
        matchLabels:
          app: <app_name>
      template:
        metadata:
          labels:
            app: <app_name>
        spec:
          containers:
          - image: <image_name>
            name: <container_name>
            securityContext:
              runAsUser: <non_root_user>
              fsGroup: <non_root_user> #only applicable for clusters that run Kubernetes version 1.13 or later
            volumeMounts:
            - name: <volume_name>
              mountPath: /<file_path>
          volumes:
          - name: <volume_name>
            persistentVolumeClaim:
              claimName: <pvc_name>
    ```
    {: codeblock}

    <table>
    <caption>Description des composants du fichier YAML</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants du fichier YAML</th>
    </thead>
    <tbody>
        <tr>
    <td><code>metadata.labels.app</code></td>
    <td>Libellé du déploiement.</td>
      </tr>
      <tr>
        <td><code>spec.selector.matchLabels.app</code> <br/> <code>spec.template.metadata.labels.app</code></td>
        <td>Libellé de votre application.</td>
      </tr>
    <tr>
    <td><code>template.metadata.labels.app</code></td>
    <td>Libellé du déploiement.</td>
      </tr>
    <tr>
    <td><code>spec.containers.image</code></td>
    <td>Nom de l'image que vous désirez utiliser. Pour répertorier les images disponibles dans votre compte {{site.data.keyword.registryshort_notm}}, exécutez la commande `ibmcloud cr image-list`.</td>
    </tr>
    <tr>
    <td><code>spec.containers.name</code></td>
    <td>Nom du conteneur que vous désirez déployer dans votre cluster.</td>
    </tr>
    <tr>
    <td><code>spec.containers.securityContext.runAsUser</code></td>
    <td>Facultatif : pour exécuter l'application avec un utilisateur non root dans un cluster qui exécute Kubernetes version 1.12 ou antérieure, indiquez le [contexte de sécurité ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/) de votre pod en définissant l'utilisateur non root sans indiquer `fsGroup` dans votre fichier YAML de déploiement en même temps. Définir `fsGroup` déclenche la mise à jour par le plug-in {{site.data.keyword.cos_full_notm}} des droits du groupe sur tous les fichiers d'un compartiment lorsque le pod est déployé. La mise à jour des droits est une opération d'écriture qui affecte les performances. En fonction du nombre de fichiers dont vous disposez, la mise à jour de droits peut empêcher votre pod d'être opérationnel et de passer à l'état <code>Running</code>. </br></br>Si vous disposez d'un cluster qui exécute Kubernetes version 1.13 ou ultérieure et du plug-in {{site.data.keyword.Bluemix_notm}} Object Storage plug-in version 1.0.4 ou ultérieure, vous pouvez modifier le propriétaire du point de montage s3fs. Pour changer le propriétaire, spécifiez le contexte de sécurité en affectant à `runAsUser` et `fsGroup` l'ID utilisateur non root qui doit être le propriétaire du point de montage s3fs. Si ces deux valeurs sont différentes, le point de montage devient automatiquement la propriété de l'utilisateur `root`.  </td>
    </tr>
    <tr>
    <td><code>spec.containers.volumeMounts.mountPath</code></td>
    <td>Chemin absolu du répertoire où est monté le volume dans le conteneur. Si vous souhaitez partager un volume entre différentes applications, vous pouvez spécifier des [sous-chemins (subPaths) de volume ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/concepts/storage/volumes/#using-subpath) pour chacune de vos applications.</td>
    </tr>
    <tr>
    <td><code>spec.containers.volumeMounts.name</code></td>
    <td>Nom du volume à monter sur votre pod.</td>
    </tr>
    <tr>
    <td><code>volumes.name</code></td>
    <td>Nom du volume à monter sur votre pod. Généralement, ce nom est identique à <code>volumeMounts/name</code>.</td>
    </tr>
    <tr>
    <td><code>volumes.persistentVolumeClaim.claimName</code></td>
    <td>Nom de la réservation de volume persistant (PVC) liée au volume persistant (PV) que vous voulez utiliser. </td>
    </tr>
    </tbody></table>

5.  Créez le déploiement.
     ```
     kubectl apply -f <local_yaml_path>
     ```
     {: pre}

6.  Vérifiez que le montage du volume persistant (PV) a abouti.

     ```
     kubectl describe deployment <deployment_name>
     ```
     {: pre}

     Le point de montage est indiqué dans la zone **Volume Mounts** et le volume est indiqué dans la zone **Volumes**.

     ```
      Volume Mounts:
          /var/run/secrets/kubernetes.io/serviceaccount from default-token-tqp61 (ro)
          /volumemount from myvol (rw)
     ...
     Volumes:
      myvol:
        Type: PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
        ClaimName: mypvc
        ReadOnly: false
     ```
     {: screen}

7. Vérifiez que vous pouvez écrire des données dans votre instance de service {{site.data.keyword.cos_full_notm}}.
   1. Connectez-vous au pod qui monte votre volume persistant.
      ```
      kubectl exec <pod_name> -it bash
      ```
      {: pre}

   2. Accédez au chemin de montage du volume que vous avez défini dans le déploiement de votre application.
   3. Créez un fichier texte.
      ```
      echo "This is a test" > test.txt
      ```
      {: pre}

   4. Dans le tableau de bord {{site.data.keyword.Bluemix}}, accédez à votre instance de service {{site.data.keyword.cos_full_notm}}.
   5. Dans le menu, sélectionnez **Compartiments**.
   6. Ouvrez votre compartiment et vérifiez que le fichier `test.txt` que vous avez créé est visible.


## Utilisation de stockage d'objets dans un ensemble avec état (StatefulSet)
{: #cos_statefulset}

Si vous disposez d'une application avec état, telle qu'une base de données, vous pouvez créer des ensembles avec état utilisant {{site.data.keyword.cos_full_notm}} pour stocker les données de votre application. Sinon, vous pouvez utiliser une base de données {{site.data.keyword.Bluemix_notm}} DaaS (Database-as-a-Service), telle que {{site.data.keyword.cloudant_short_notm}}, et stocker vos données dans le cloud.
{: shortdesc}

Avant de commencer :
- [Créez et préparez votre instance de service {{site.data.keyword.cos_full_notm}}](#create_cos_service).
- [Créez une valeur confidentielle pour stocker vos données d'identification pour le service {{site.data.keyword.cos_full_notm}}](#create_cos_secret).
- [Déterminez la configuration à adopter pour {{site.data.keyword.cos_full_notm}}](#configure_cos).

Pour déployer un ensemble avec état utilisant du stockage d'objets :

1. Créez un fichier de configuration pour votre ensemble avec état et le service que vous utilisez pour exposer cet ensemble. Les exemples suivants illustrent comment déployer NGINX en tant qu'ensemble avec état avec 3 répliques, chaque réplique utilisant un compartiment distinct, ou avec toutes les répliques partageant le même compartiment.

   **Exemple de création d'un ensemble avec état avec 3 répliques, chaque réplique utilisant un compartiment distinct**:
   ```
   apiVersion: v1
   kind: Service
   metadata:
     name: nginx-v01
     namespace: default
     labels:
       app: nginx-v01 # must match spec.template.metadata.labels and spec.selector.matchLabels in stateful set YAML
   spec:
     ports:
     - port: 80
       name: web
     clusterIP: None
     selector:
       app: nginx-v01 # must match spec.template.metadata.labels and spec.selector.matchLabels in stateful set YAML
   ---
   apiVersion: apps/v1
   kind: StatefulSet
   metadata:
     name: web-v01
     namespace: default
   spec:
     selector:
       matchLabels:
         app: nginx-v01 # must match spec.template.metadata.labels in stateful set YAML and metadata.labels in service YAML
     serviceName: "nginx-v01"
     replicas: 3 
     template:
       metadata:
         labels:
           app: nginx-v01 # must match spec.selector.matchLabels in stateful set YAML and metadata.labels in service YAML
       spec:
         terminationGracePeriodSeconds: 10
         containers:
         - name: nginx
           image: k8s.gcr.io/nginx-slim:0.8
           ports:
           - containerPort: 80
             name: web
           volumeMounts:
           - name: mypvc
             mountPath: /usr/share/nginx/html
     volumeClaimTemplates:
     - metadata:
         name: mypvc
         annotations:
           ibm.io/auto-create-bucket: "true"
           ibm.io/auto-delete-bucket: "true"
           ibm.io/bucket: ""
           ibm.io/secret-name: mysecret 
           volume.beta.kubernetes.io/storage-class: ibmc-s3fs-standard-perf-cross-region 
           volume.beta.kubernetes.io/storage-provisioner: ibm.io/ibmc-s3fs
       spec:
         accessModes: [ "ReadWriteOnce" ]
         storageClassName: "ibmc-s3fs-standard-perf-cross-region"
         resources:
           requests:
             storage: 1Gi
   ```
   {: codeblock}

   **Exemple de création d'un ensemble avec état avec 3 répliques partageant le même compartiment nommé `mybucket`** :
   ```
   apiVersion: v1
   kind: Service
   metadata:
     name: nginx-v01
     namespace: default
     labels:
       app: nginx-v01 # must match spec.template.metadata.labels and spec.selector.matchLabels in stateful set YAML
   spec:
     ports:
     - port: 80
       name: web
     clusterIP: None
     selector:
       app: nginx-v01 # must match spec.template.metadata.labels and spec.selector.matchLabels in stateful set YAML
   --- 
   apiVersion: apps/v1
   kind: StatefulSet
   metadata:
     name: web-v01
     namespace: default
   spec:
     selector:
       matchLabels:
         app: nginx-v01 # must match spec.template.metadata.labels in stateful set YAML and metadata.labels in service YAML
     serviceName: "nginx-v01"
     replicas: 3 
     template:
       metadata:
         labels:
           app: nginx-v01 # must match spec.selector.matchLabels in stateful set YAML and metadata.labels in service YAML
       spec:
         terminationGracePeriodSeconds: 10
         containers:
         - name: nginx
           image: k8s.gcr.io/nginx-slim:0.8
           ports:
           - containerPort: 80
             name: web
           volumeMounts:
           - name: mypvc
             mountPath: /usr/share/nginx/html
     volumeClaimTemplates:
     - metadata:
         name: mypvc
         annotations:
           ibm.io/auto-create-bucket: "false"
           ibm.io/auto-delete-bucket: "false"
           ibm.io/bucket: mybucket
           ibm.io/secret-name: mysecret
           volume.beta.kubernetes.io/storage-class: ibmc-s3fs-standard-perf-cross-region 
           volume.beta.kubernetes.io/storage-provisioner: ibm.io/ibmc-s3fs
       spec:
         accessModes: [ "ReadOnlyMany" ]
         storageClassName: "ibmc-s3fs-standard-perf-cross-region"
         resources:
           requests:
             storage: 1Gi
   ```
   {: codeblock}


   <table>
    <caption>Description des composants du fichier YAML de l'ensemble avec état</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants du fichier YAML de l'ensemble avec état</th>
    </thead>
    <tbody>
    <tr>
    <td style="text-align:left"><code>metadata.name</code></td>
    <td style="text-align:left">Entrez un nom pour votre ensemble avec état. Le nom que vous indiquez est utilisé pour créer le nom de votre PVC au format : <code>&lt;volume_name&gt;-&lt;statefulset_name&gt;-&lt;replica_number&gt;</code>. </td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.serviceName</code></td>
    <td style="text-align:left">Entrez le nom du service que vous souhaitez utiliser pour exposer votre ensemble avec état. </td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.replicas</code></td>
    <td style="text-align:left">Entrez le nombre de répliques de votre ensemble avec état. </td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.selector.matchLabels</code></td>
    <td style="text-align:left">Entrez tous les libellés que vous souhaitez inclure dans votre ensemble avec état et votre PVC. Les libellés que vous indiquez dans la section <code>volumeClaimTemplates</code> de votre ensemble avec état ne sont pas reconnus par Kubernetes. A la place, vous devez définir ces libellés dans les sections <code>spec.selector.matchLabels</code> et <code>spec.template.metadata.labels</code> du fichier YAML de votre ensemble avec état. Pour vérifier que toutes les répliques de votre ensemble avec état sont incluses dans l'équilibrage de charge de votre service, incluez le libellé que vous avez déjà utilisé dans la section <code>spec.selector</code> du fichier YAML de votre service. </td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.template.metadata.labels</code></td>
    <td style="text-align:left">Entrez les mêmes libellés que vous avez ajoutés dans la section <code>spec.selector.matchLabels</code> du fichier YAML de votre ensemble avec état. </td>
    </tr>
    <tr>
    <td><code>spec.template.spec.</code></br><code>terminationGracePeriodSeconds</code></td>
    <td>Entrez le nombre de secondes à accorder au <code>kubelet</code> pour terminer correctement le pod qui exécute la réplique de votre ensemble avec état. Pour plus d'informations, voir l'article sur la [suppression des pods ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/tasks/run-application/force-delete-stateful-set-pod/#delete-pods). </td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.volumeClaimTemplates.</code></br><code>metadata.name</code></td>
    <td style="text-align:left">Entrez un nom pour votre volume. Utilisez le même nom que vous avez défini dans la section <code>spec.containers.volumeMount.name</code>. Le nom que vous indiquez ici est utilisé pour créer le nom de votre PVC au format : <code>&lt;nom_volume&gt;-&lt;nom_ensemble_avec_état&gt;-&lt;nombre_répliques&gt;</code>. </td>
    </tr>
    <tr>
    <td><code>spec.volumeClaimTemplates.metadata</code></br><code>annotions.ibm.io/auto-create-bucket</code></td>
    <td>Sélectionnez l'une des options suivantes : <ul><li><strong>true : </strong>sélectionnez cette option pour créer automatiquement un compartiment pour chaque réplique de l'ensemble avec état. </li><li><strong>false : </strong>sélectionnez cette option si vous souhaitez que les répliques de votre ensemble avec état partagent un compartiment existant. Assurez-vous que le nom du compartiment est défini dans la section <code>spec.volumeClaimTemplates.metadata.annotions.ibm.io/bucket</code> du fichier YAML de votre ensemble avec état.</li></ul></td>
    </tr>
    <tr>
    <td><code>spec.volumeClaimTemplates.metadata</code></br><code>annotions.ibm.io/auto-delete-bucket</code></td>
    <td>Sélectionnez l'une des options suivantes : <ul><li><strong>true : </strong>vos données, le compartiment et le volume persistant sont automatiquement supprimés en même temps que la PVC. Il reste votre instance de service {{site.data.keyword.cos_full_notm}} qui n'est pas supprimée. Si vous choisissez de définir cette option avec la valeur true, vous devez définir <code>ibm.io/auto-create-bucket: true</code> et <code>ibm.io/bucket: ""</code> pour que votre compartiment soit automatiquement créé avec un nom au format <code>tmp-s3fs-xxxx</code>. </li><li><strong>false : </strong>lorsque vous supprimez la PVC, le volume persistant est automatiquement supprimé mais vos données et le compartiment figurant dans votre instance de service {{site.data.keyword.cos_full_notm}} sont conservés. Pour accéder à vos données, vous devez créer une nouvelle PVC avec le nom de votre compartiment existant.</li></ul></td>
    </tr>
    <tr>
    <td><code>spec.volumeClaimTemplates.metadata</code></br><code>annotions.ibm.io/bucket</code></td>
    <td>Sélectionnez l'une des options suivantes : <ul><li><strong>Si <code>ibm.io/auto-create-bucket</code> a la valeur true : </strong>entrez le nom du compartiment que vous voulez créer dans {{site.data.keyword.cos_full_notm}}. De plus, si <code>ibm.io/auto-delete-bucket</code> a la valeur <strong>true</strong>, vous devez laisser cette zone vide pour affecter automatiquement un nom à votre compartiment au format tmp-s3fs-xxxx. Ce nom doit être unique dans {{site.data.keyword.cos_full_notm}}.</li><li><strong>Si <code>ibm.io/auto-create-bucket</code> a la valeur false : </strong>entrez le nom du compartiment existant auquel vous souhaitez accéder dans le cluster.</li></ul></td>
    </tr>
    <tr>
    <td><code>spec.volumeClaimTemplates.metadata</code></br><code>annotions.ibm.io/secret-name</code></td>
    <td>Entrez le nom de la valeur confidentielle qui contient les données d'identification {{site.data.keyword.cos_full_notm}} que vous avez créées précédemment.</td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.volumeClaimTemplates.metadata.</code></br><code>annotations.volume.beta.</code></br><code>kubernetes.io/storage-class</code></td>
    <td style="text-align:left">Entrez la classe de stockage de votre choix. Sélectionnez l'une des options suivantes : <ul><li><strong>Si <code>ibm.io/auto-create-bucket</code> a la valeur true : </strong>entrez la classe de stockage que vous désirez utiliser pour votre nouveau compartiment.</li><li><strong>Si <code>ibm.io/auto-create-bucket</code> a la valeur false : </strong>entrez la classe de stockage que vous avez utilisée pour créer votre compartiment existant. </li></ul></br> Pour répertorier les classes de stockage existantes, exécutez la commande <code>kubectl get storageclasses | grep s3</code>. Si vous n'indiquez pas de classe de stockage, la PVC est créée avec la classe de stockage par défaut définie dans votre cluster. Vérifiez que la classe de stockage par défaut comporte <code>ibm.io/ibmc-s3fs</code> dans la section 'provisioner' de sorte que votre ensemble avec état soit mis à disposition avec du stockage d'objets.</td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.volumeClaimTemplates.</code></br><code>spec.storageClassName</code></td>
    <td>Entrez la même classe de stockage que vous avez indiquée dans la section <code>spec.volumeClaimTemplates.metadata.annotations.volume.beta.kubernetes.io/storage-class</code> du fichier YAML de votre ensemble avec état.  </td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.volumeClaimTemplates.spec.</code></br><code>resource.requests.storage</code></td>
    <td>Entrez une taille fictive pour votre compartiment {{site.data.keyword.cos_full_notm}} en gigaoctets. La taille est requise par Kubernetes, mais n'est pas respectée dans {{site.data.keyword.cos_full_notm}}. Vous pouvez entrer la taille de votre choix. L'espace réel que vous utilisez dans {{site.data.keyword.cos_full_notm}} peut être différent et est facturé en fonction du [tableau de tarification ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api).</td>
    </tr>
    </tbody></table>


## Sauvegarde et restauration des données
{: #cos_backup_restore}

{{site.data.keyword.cos_full_notm}} est configuré pour assurer la haute durabilité de vos données de sorte à éviter la perte de vos données. Vous pourrez obtenir l'accord sur les niveaux de service (SLA) dans les [conditions d'utilisation du service {{site.data.keyword.cos_full_notm}} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www-03.ibm.com/software/sla/sladb.nsf/sla/bm-7857-03).
{: shortdesc}

{{site.data.keyword.cos_full_notm}} ne fournit aucun historique de version de vos données. Si vous devez conserver et accéder à des versions antérieures de vos données, vous devez définir votre application de sorte à gérer l'historique des données ou implémenter d'autres solutions de sauvegarde. Par exemple, vous pouvez envisager de stocker vos données {{site.data.keyword.cos_full_notm}} dans votre base de données sur site ou d'utiliser des bandes pour archiver vos données.
{: note}

## Référence des classes de stockage
{: #cos_storageclass_reference}

### Standard
{: #standard}

<table>
<caption>Classe Object Storage : standard</caption>
<thead>
<th>Caractéristiques</th>
<th>Paramètre</th>
</thead>
<tbody>
<tr>
<td>Nom</td>
<td><code>ibmc-s3fs-standard-cross-region</code></br><code>ibmc-s3fs-standard-perf-cross-region</code></br><code>ibmc-s3fs-standard-regional</code></br><code>ibmc-s3fs-standard-perf-regional</code></td>
</tr>
<tr>
<td>Noeud final de résilience par défaut</td>
<td>Le noeud final de résilience est automatiquement défini en fonction de l'emplacement de votre cluster. Voir [Régions et noeuds finaux](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-endpoints#endpoints) pour plus d'informations. </td>
</tr>
<tr>
<td>Taille de bloc</td>
<td>Classes de stockage sans `perf` : 16 Mo</br>Classes de stockage avec `perf` : 52 Mo</td>
</tr>
<tr>
<td>Cache du noyau</td>
<td>Activé</td>
</tr>
<tr>
<td>Facturation</td>
<td>Mensuelle</td>
</tr>
<tr>
<td>Tarification</td>
<td>[Tarification ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api)</td>
</tr>
</tbody>
</table>

### Vault
{: #Vault}

<table>
<caption>Classe Object Storage : vault</caption>
<thead>
<th>Caractéristiques</th>
<th>Paramètre</th>
</thead>
<tbody>
<tr>
<td>Nom</td>
<td><code>ibmc-s3fs-vault-cross-region</code></br><code>ibmc-s3fs-vault-regional</code></td>
</tr>
<tr>
<td>Noeud final de résilience par défaut</td>
<td>Le noeud final de résilience est automatiquement défini en fonction de l'emplacement de votre cluster. Voir [Régions et noeuds finaux](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-endpoints#endpoints) pour plus d'informations. </td>
</tr>
<tr>
<td>Taille de bloc</td>
<td>16 Mo</td>
</tr>
<tr>
<td>Cache du noyau</td>
<td>Désactivé</td>
</tr>
<tr>
<td>Facturation</td>
<td>Mensuelle</td>
</tr>
<tr>
<td>Tarification</td>
<td>[Tarification ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api)</td>
</tr>
</tbody>
</table>

### Cold
{: #cold}

<table>
<caption>Classe Object Storage : cold</caption>
<thead>
<th>Caractéristiques</th>
<th>Paramètre</th>
</thead>
<tbody>
<tr>
<td>Nom</td>
<td><code>ibmc-s3fs-flex-cross-region</code></br><code>ibmc-s3fs-flex-perf-cross-region</code></br><code>ibmc-s3fs-flex-regional</code></br><code>ibmc-s3fs-flex-perf-regional</code></td>
</tr>
<tr>
<td>Noeud final de résilience par défaut</td>
<td>Le noeud final de résilience est automatiquement défini en fonction de l'emplacement de votre cluster. Voir [Régions et noeuds finaux](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-endpoints#endpoints) pour plus d'informations. </td>
</tr>
<tr>
<td>Taille de bloc</td>
<td>16 Mo</td>
</tr>
<tr>
<td>Cache du noyau</td>
<td>Désactivé</td>
</tr>
<tr>
<td>Facturation</td>
<td>Mensuelle</td>
</tr>
<tr>
<td>Tarification</td>
<td>[Tarification ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api)</td>
</tr>
</tbody>
</table>

### Flex
{: #flex}

<table>
<caption>Classe Object Storage : flex</caption>
<thead>
<th>Caractéristiques</th>
<th>Paramètre</th>
</thead>
<tbody>
<tr>
<td>Nom</td>
<td><code>ibmc-s3fs-cold-cross-region</code></br><code>ibmc-s3fs-flex-perf-cross-region</code></br><code>ibmc-s3fs-cold-regional</code></br><code>ibmc-s3fs-flex-perf-regional</code></td>
</tr>
<tr>
<td>Noeud final de résilience par défaut</td>
<td>Le noeud final de résilience est automatiquement défini en fonction de l'emplacement de votre cluster. Voir [Régions et noeuds finaux](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-endpoints#endpoints) pour plus d'informations. </td>
</tr>
<tr>
<td>Taille de bloc</td>
<td>Classes de stockage sans `perf` : 16 Mo</br>Classes de stockage avec `perf` : 52 Mo</td>
</tr>
<tr>
<td>Cache du noyau</td>
<td>Activé</td>
</tr>
<tr>
<td>Facturation</td>
<td>Mensuelle</td>
</tr>
<tr>
<td>Tarification</td>
<td>[Tarification ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api)</td>
</tr>
</tbody>
</table>
