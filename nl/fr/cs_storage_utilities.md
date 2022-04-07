---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: kubernetes, iks, local persistent storage

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



# Utilitaires de stockage IBM Cloud
{: #utilities}

## Installation du plug-in IBM Cloud Block Storage Attacher (bêta)
{: #block_storage_attacher}

Utilisez le plug-in {{site.data.keyword.Bluemix_notm}} Block Storage Attacher pour connecter du stockage par blocs brut, non formaté et non monté sur un noeud worker dans votre cluster.  
{: shortdesc}

Par exemple, vous envisagez de stocker vos données avec une solution de stockage défini par logiciel (SDS), telle que [Portworx](/docs/containers?topic=containers-portworx), mais vous ne voulez pas utiliser des noeuds worker bare metal optimisés pour l'utilisation de SDS et fournis avec des disques locaux supplémentaires. Pour ajouter des disques locaux à votre noeud worker non SDS, vous devez créer manuellement vos unités de stockage par blocs dans votre compte d'infrastructure {{site.data.keyword.Bluemix_notm}} et utilisez le plug-in {{site.data.keyword.Bluemix_notm}} Block Volume Attacher pour connecter le stockage à votre noeud worker non SDS.

Le plug-in {{site.data.keyword.Bluemix_notm}} Block Volume Attacher crée des pods sur tous les noeuds worker au sein d'un ensemble de démons (daemonSet) et configure une classe de stockage Kubernetes que vous pouvez utiliser par la suite pour connecter l'unité de stockage par blocs à votre noeud worker non SDS.

Vous recherchez des instructions pour mettre à jour ou supprimer le plug-in {{site.data.keyword.Bluemix_notm}} Block Volume Attacher ? Voir [Mise à jour du plug-in](#update_block_attacher) et [Retrait du plug-in](#remove_block_attacher).
{: tip}

1.  [Suivez les instructions](/docs/containers?topic=containers-helm#public_helm_install) d'installation du client Helm sur votre machine locale, et installez le serveur Helm (Tiller) avec un compte de service dans votre cluster.

2.  Vérifiez que Tiller est installé avec un compte de service.

    ```
    kubectl get serviceaccount -n kube-system | grep tiller
    ```
    {: pre}

    Exemple de sortie :

    ```
    NAME                                 SECRETS   AGE
    tiller                               1         2m
    ```
    {: screen}

3. Mettez à jour le référentiel Helm pour extraire la dernière version de toutes les chartes Helm figurant dans ce référentiel.
   ```
   helm repo update
   ```
   {: pre}

4. Installez le plug-in {{site.data.keyword.Bluemix_notm}} Block Volume Attacher. Lors de l'installation de ce plug-in, des classes de stockage par blocs prédéfinies sont ajoutées dans votre cluster.
   ```
   helm install iks-charts/ibm-block-storage-attacher --name block-attacher
   ```
   {: pre}

   Exemple de sortie :
   ```
   NAME:   block-volume-attacher
   LAST DEPLOYED: Thu Sep 13 22:48:18 2018
   NAMESPACE: default
   STATUS: DEPLOYED

   RESOURCES:
   ==> v1beta1/ClusterRoleBinding
   NAME                             AGE
   ibmcloud-block-storage-attacher  1s

   ==> v1beta1/DaemonSet
   NAME                             DESIRED  CURRENT  READY  UP-TO-DATE  AVAILABLE  NODE SELECTOR  AGE
   ibmcloud-block-storage-attacher  0        0        0      0           0          <none>         1s

   ==> v1/StorageClass
   NAME                 PROVISIONER                AGE
   ibmc-block-attacher  ibm.io/ibmc-blockattacher  1s

   ==> v1/ServiceAccount
   NAME                             SECRETS  AGE
   ibmcloud-block-storage-attacher  1        1s

   ==> v1beta1/ClusterRole
   NAME                             AGE
   ibmcloud-block-storage-attacher  1s

   NOTES:
   Thank you for installing: ibmcloud-block-storage-attacher.   Your release is named: block-volume-attacher

   Please refer Chart README.md file for attaching a block storage
   Please refer Chart RELEASE.md to see the release details/fixes
   ```
   {: screen}

5. Vérifiez que l'ensemble de démons d'{{site.data.keyword.Bluemix_notm}} Block Volume Attacher est installé.
   ```
   kubectl get pod -n kube-system -o wide | grep attacher
   ```
   {: pre}

   Exemple de sortie :
   ```
   ibmcloud-block-storage-attacher-z7cv6           1/1       Running            0          19m
   ```
   {: screen}

   L'installation a abouti lorsque vous voyez un ou plusieurs pods **ibmcloud-block-storage-attacher**. Le nombre de pods est égal au nombre de noeuds worker dans votre cluster. Tous les pods doivent être à l'état **Running**.

6. Vérifiez que la classe de stockage d'{{site.data.keyword.Bluemix_notm}} Block Volume Attacher a été créée.
   ```
   kubectl get storageclasses | grep attacher
   ```
   {: pre}

   Exemple de sortie :
   ```
   ibmc-block-attacher       ibm.io/ibmc-blockattacher   11m
   ```
   {: screen}

### Mise à jour du plug-in IBM Cloud Block Storage Attacher
{: #update_block_attacher}

Vous pouvez mettre à niveau votre plug-in {{site.data.keyword.Bluemix_notm}} Block Storage Attacher pour passer à la version la plus récente.
{: shortdesc}

1. Mettez à jour le référentiel Helm pour extraire la dernière version de toutes les chartes Helm figurant dans ce référentiel.
   ```
   helm repo update
   ```
   {: pre}

2. Facultatif : téléchargez la charte Helm la plus récente sur votre machine locale. Ensuite, extrayez le package et consultez le fichier `release.md` pour trouver les informations relatives à la dernière édition.
   ```
   helm fetch iks-charts/ibmcloud-block-storage-plugin
   ```
   {: pre}

3. Recherchez le nom de la charte Helm correspondant au plug-in {{site.data.keyword.Bluemix_notm}} Block Storage Attacher.
   ```
   helm ls | grep ibm-block-storage-attacher
   ```
   {: pre}

   Exemple de sortie :
   ```
   <helm_chart_name>	1       	Wed Aug  1 14:55:15 2018	DEPLOYED	ibm-block-storage-attacher-1.0.0	default
   ```
   {: screen}

4. Mettez à niveau le plug-in {{site.data.keyword.Bluemix_notm}} Block Storage Attacher à la dernière version.
   ```
   helm upgrade --force --recreate-pods <helm_chart_name> ibm-block-storage-attacher
   ```
   {: pre}

### Retrait du plug-in IBM Cloud Block Volume Attacher
{: #remove_block_attacher}

Si vous n'envisagez pas de mettre à disposition ou d'utiliser le plug-in {{site.data.keyword.Bluemix_notm}} Block Storage Attacher dans votre cluster, vous pouvez désinstaller la charte Helm.
{: shortdesc}

1. Recherchez le nom de la charte Helm correspondant au plug-in {{site.data.keyword.Bluemix_notm}} Block Storage Attacher.
   ```
   helm ls | grep ibm-block-storage-attacher
   ```
   {: pre}

   Exemple de sortie :
   ```
   <helm_chart_name>	1       	Wed Aug  1 14:55:15 2018	DEPLOYED	ibm-block-storage-attacher-1.0.0	default
   ```
   {: screen}

2. Supprimez le plug-in {{site.data.keyword.Bluemix_notm}} Block Storage Attacher en retirant la charte Helm.
   ```
   helm delete <helm_chart_name> --purge
   ```
   {: pre}

3. Vérifiez que les pods du plug-in {{site.data.keyword.Bluemix_notm}} Block Storage Attacher sont bien retirés.
   ```
   kubectl get pod -n kube-system -o wide | grep attacher
   ```
   {: pre}

   Le retrait des pods a abouti lorsqu'aucun pod n'est affiché dans la sortie de votre interface de ligne de commande.

4. Vérifiez que la classe de stockage du plug-in {{site.data.keyword.Bluemix_notm}} Block Storage Attacher est bien retirée.
   ```
   kubectl get storageclasses | grep attacher
   ```
   {: pre}

   Le retrait de la classe de stockage a abouti lorsqu'aucune classe de stockage n'est affichée dans la sortie de votre interface de ligne de commande.

## Mise à disposition automatique de stockage par blocs non formaté et autorisation de vos noeuds worker à accéder au stockage
{: #automatic_block}

Vous pouvez utiliser le plug-in {{site.data.keyword.Bluemix_notm}} Block Volume Attacher pour ajouter automatiquement du stockage par blocs brut, non formaté et non monté avec la même configuration à tous les noeuds worker dans votre cluster.
{: shortdesc}

Le conteneur `mkpvyaml` inclus dans le plug-in {{site.data.keyword.Bluemix_notm}} Block Volume Attacher est configuré pour exécuter un script qui recherche tous les noeuds worker de votre cluster, crée du stockage brut dans le portail de l'infrastructure {{site.data.keyword.Bluemix_notm}}, puis autorise les noeuds worker à accéder au stockage.

Pour ajouter d'autres configurations de stockage par blocs, ajouter du stockage par bloc uniquement à un sous-ensemble de noeuds worker ou disposer de plus de contrôle sur le processus de mise à disposition, choisissez d'[ajouter du stockage par blocs manuellement](#manual_block).
{: tip}


1. Connectez-vous à {{site.data.keyword.Bluemix_notm}} et ciblez le groupe de ressources dans lequel se trouve votre cluster.
   ```
   ibmcloud login
   ```
   {: pre}

2.  Clonez le référentiel des utilitaires de stockage d'{{site.data.keyword.Bluemix_notm}}.
    ```
    git clone https://github.com/IBM/ibmcloud-storage-utilities.git
    ```
    {: pre}

3. Accédez au répertoire `block-storage-utilities`.
   ```
   cd ibmcloud-storage-utilities/block-storage-provisioner
   ```
   {: pre}

4. Ouvrez le fichier `yamlgen.yaml` et spécifiez la configuration de stockage par blocs que vous voulez ajouter à tous les noeuds worker dans le cluster.
   ```
   #
   # Can only specify 'performance' OR 'endurance' and associated clause
   #
   cluster:  <cluster_name>    #  Enter the name of your cluster
   region:   <region>          #  Enter the IBM Cloud Kubernetes Service region where you created your cluster
   type:  <storage_type>       #  Enter the type of storage that you want to provision. Choose between "performance" or "endurance"
   offering: storage_as_a_service
   # performance:
   #    - iops:  <iops>
   endurance:
     - tier:  2                #   [0.25|2|4|10]
   size:  [ 20 ]               #   Enter the size of your storage in gigabytes
   ```
   {: codeblock}

   <table>
   <caption>Description des composants du fichier YAML</caption>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants du fichier YAML</th>
   </thead>
   <tbody>
   <tr>
   <td><code>cluster</code></td>
   <td>Entrez le nom du cluster dans lequel vous souhaitez ajouter du stockage par blocs brut. </td>
   </tr>
   <tr>
   <td><code>region</code></td>
   <td>Entrez la région {{site.data.keyword.containerlong_notm}} dans laquelle vous avez créé le cluster. Exécutez la commande <code>[bxcs] cluster-get --cluster &lt;cluster_name_or_ID&gt;</code> pour trouver la région de votre cluster.  </td>
   </tr>
   <tr>
   <td><code>type</code></td>
   <td>Entrez le type de stockage que vous souhaitez mettre à disposition. Choisissez entre <code>performance</code> ou <code>endurance</code>. Pour plus d'informations, voir [Détermination de la configuration de votre stockage par blocs](/docs/containers?topic=containers-block_storage#block_predefined_storageclass).  </td>
   </tr>
   <tr>
   <td><code>performance.iops</code></td>
   <td>Si vous souhaitez mettre à disposition du stockage de type `performance`, entrez le nombre d'E-S/s (IOPS). Pour plus d'informations, voir [Détermination de la configuration de votre stockage par blocs](/docs/containers?topic=containers-block_storage#block_predefined_storageclass). Si vous souhaitez mettre à disposition du stockage de type `endurance`, supprimez cette section ou mettez-la en commentaire en ajoutant `#` au début de chaque ligne.
   </tr>
   <tr>
   <td><code>endurance.tier</code></td>
   <td>Si vous souhaitez mettre à disposition du stockage de type `endurance`, entrez le nombre d'IOPS par gigaoctet. Par exemple, pour mettre à disposition du stockage par blocs tel qu'il est défini dans la classe de stockage `ibmc-block-bronze`, entrez 2. Pour plus d'informations, voir [Détermination de la configuration de votre stockage par blocs](/docs/containers?topic=containers-block_storage#block_predefined_storageclass). Si vous souhaitez mettre à disposition du stockage de type `performance`, supprimez cette section ou mettez-la en commentaire en ajoutant `#` au début de chaque ligne. </td>
   </tr>
   <tr>
   <td><code>size</code></td>
   <td>Entrez la taille de votre stockage en gigaoctets. Voir [Détermination de la configuration de votre stockage par blocs](/docs/containers?topic=containers-block_storage#block_predefined_storageclass) pour connaître les tailles prises en charge pour votre niveau de stockage. </td>
   </tr>
   </tbody>
   </table>  

5. Récupérez votre nom d'utilisateur pour l'infrastructure IBM Cloud (SoftLayer), ainsi que la clé d'API. Le nom d'utilisateur et la clé d'API sont utilisés par le script `mkpvyaml` pour accéder au cluster.
   1. Connectez-vous à la [console {{site.data.keyword.Bluemix_notm}}![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/).
   2. Dans le menu ![Icône de menu](../icons/icon_hamburger.svg "Icône de menu"), sélectionnez **Infrastructure**.
   3. Dans la barre de menu, sélectionnez **Compte** > **Utilisateurs** > **Liste d'utilisateurs**.
   4. Recherchez l'utilisateur dont vous voulez récupérer le nom d'utilisateur et la clé d'API.
   5. Cliquez sur **Générer** pour générer la clé d'API ou sur **Afficher** pour visualiser votre clé d'API. Une fenêtre en incrustation s'ouvre indiquant le nom d'utilisateur de l'infrastructure et la clé d'API.

6. Stockez les données d'identification dans une variable d'environnement.
   1. Ajoutez les variables d'environnement.
      ```
      export SL_USERNAME=<infrastructure_username>
      ```
      {: pre}

      ```
      export SL_API_KEY=<infrastructure_api_key>
      ```
      {: pre}

   2. Vérifiez que vos variables d'environnement sont créées.
      ```
      printenv
      ```
      {: pre}

7.  Générez et exécutez le conteneur `mkpvyaml`. Lorsque vous exécutez le conteneur à partir de l'image, le script `mkpvyaml.py` est exécuté. Ce script ajoute une unité de stockage par blocs à tous les noeuds worker du cluster et autorise chaque noeud worker à accéder à cette unité. A la fin du script, un fichier YAML nommé `pv-<cluster_name>.yaml` est généré pour vous pour que vous puissiez l'utiliser par la suite pour créer les volumes persistants dans le cluster.
    1.  Générez le conteneur `mkpvyaml`.
        ```
        docker build -t mkpvyaml .
        ```
        {: pre}
        Exemple de sortie :
        ```
        Sending build context to Docker daemon   29.7kB
        Step 1/16 : FROM ubuntu:18.10
        18.10: Pulling from library/ubuntu
        5940862bcfcd: Pull complete
        a496d03c4a24: Pull complete
        5d5e0ccd5d0c: Pull complete
        ba24b170ddf1: Pull complete
        Digest: sha256:20b5d52b03712e2ba8819eb53be07612c67bb87560f121cc195af27208da10e0
        Status: Downloaded newer image for ubuntu:18.10
         ---> 0bfd76efee03
        Step 2/16 : RUN apt-get update
         ---> Running in 85cedae315ce
        Get:1 http://security.ubuntu.com/ubuntu cosmic-security InRelease [83.2 kB]
        Get:2 http://archive.ubuntu.com/ubuntu cosmic InRelease [242 kB]
        ...
        Step 16/16 : ENTRYPOINT [ "/docker-entrypoint.sh" ]
         ---> Running in 9a6842f3dbe3
        Removing intermediate container 9a6842f3dbe3
         ---> 7926f5384fc7
        Successfully built 7926f5384fc7
        Successfully tagged mkpvyaml:latest
        ```
        {: screen}
    2.  Exécutez le conteneur pour lancer l'exécution du script `mkpvyaml.py`.
        ```
        docker run --rm -v `pwd`:/data -v ~/.bluemix:/config -e SL_API_KEY=$SL_API_KEY -e SL_USERNAME=$SL_USERNAME mkpvyaml
        ```
        {: pre}

        Exemple de sortie :
        ```
        Unable to find image 'portworx/iks-mkpvyaml:latest' locally
        latest: Pulling from portworx/iks-mkpvyaml
        72f45ff89b78: Already exists
        9f034a33b165: Already exists
        386fee7ab4d3: Already exists
        f941b4ac6aa8: Already exists
        fe93e194fcda: Pull complete
        f29a13da1c0a: Pull complete
        41d6e46c1515: Pull complete
        e89af7a21257: Pull complete
        b8a7a212d72e: Pull complete
        5e07391a6f39: Pull complete
        51539879626c: Pull complete
        cdbc4e813dcb: Pull complete
        6cc28f4142cf: Pull complete
        45bbaad87b7c: Pull complete
        05b0c8595749: Pull complete
        Digest: sha256:43ac58a8e951994e65f89ed997c173ede1fa26fb41e5e764edfd3fc12daf0120
        Status: Downloaded newer image for portworx/iks-mkpvyaml:latest

        kube-dal10-abc1d23e123456ac8b12a3c1e12b123a4 10.XXX.XX.XX
        Creating Vol of size 20 with type:  endurance
                  Ordering block storage of size: 20 for host: kube-dal10-abc1d23e123456ac8b12a3c1e12b123a4
                  ORDER ID =  30087291
        kube-dal10-abc1d23e123456ac8b12a3c1e12b123a4 10.XXX.XX.XX
        Creating Vol of size 20 with type:  endurance
                  Ordering block storage of size: 20 for host: kube-dal10-abc1d23e123456ac8b12a3c1e12b123a4
                  ORDER ID =  30087293
        kube-dal12-abc1d23e123456ac8b12a3c1e12b456v1 10.XXX.XX.XX
        Creating Vol of size 20 with type:  endurance
                  Ordering block storage of size: 20 for host: kube-dal12-abc1d23e123456ac8b12a3c1e12b456v1
                  ORDER ID =  30085655
                  No volume yet ... for orderID :  30087291
                  No volume yet ... for orderID :  30087291
                  No volume yet ... for orderID :  30087291
                  No volume yet ... for orderID :  30087291
                  Order ID =  30087291 has created VolId =  12345678
                  Granting access to volume: 12345678 for HostIP: 10.XXX.XX.XX
                  Order ID =  30087293 has created VolId =  87654321
                  Granting access to volume: 87654321 for HostIP: 10.XXX.XX.XX
                  Vol 53002831 is not yet ready ...
                  Granting access to volume: 87654321 for HostIP: 10.XXX.XX.XX
                  Order ID =  30085655 has created VolId =  98712345
                  Granting access to volume: 98712345 for HostIP: 10.XXX.XX.XX
        Output file created as :  pv-<cluster_name>.yaml
        ```
        {: screen}

7. [Connectez les unités de stockage par blocs à vos noeuds worker](#attach_block).

## Ajout manuel de stockage par blocs à des noeuds worker spécifiques
{: #manual_block}

Utilisez cette option pour ajouter différentes configurations de stockage par blocs, ajouter du stockage par blocs à un sous-ensemble de noeuds worker uniquement ou pour disposer de plus de contrôle sur le processus de mise à disposition.
{: shortdesc}

1. Répertoriez les noeuds worker de votre cluster et notez leur adresse IP privée et la zone des noeuds worker non SDS où vous souhaitez ajouter une unité de stockage par blocs.
   ```
   ibmcloud ks workers --cluster <cluster_name_or_ID>
   ```
   {: pre}

2. Examinez les étapes 3 et 4 dans la rubrique [Détermination de la configuration de votre stockage par blocs](/docs/containers?topic=containers-block_storage#block_predefined_storageclass) pour choisir le type, la taille et le nombre d'opérations d'entrée-sortie par seconde (IOPS) de l'unité de stockage par blocs que vous souhaitez ajouter à votre noeud worker non SDS.    

3. Créez l'unité de stockage par blocs dans la même zone que votre noeud worker non SDS.

   **Exemple de mise à disposition d'un stockage par blocs de type endurance de 20 Go avec deux IOPS par Go :**
   ```
   ibmcloud sl block volume-order --storage-type endurance --size 20 --tier 2 --os-type LINUX --datacenter dal10
   ```
   {: pre}

   **Exemple de mise à disposition de stockage par blocs de type performance de 20 Go avec 100 IOPS :**
   ```
   ibmcloud sl block volume-order --storage-type performance --size 20 --iops 100 --os-type LINUX --datacenter dal10
   ```
   {: pre}

4. Vérifiez que l'unité de stockage par blocs est créée et notez l'**`id`** du volume. **Remarque :** si vous ne voyez pas tout de suite votre unité de stockage par blocs, patientez quelques minutes. Réexécutez ensuite cette commande.
   ```
   ibmcloud sl block volume-list
   ```
   {: pre}

   Exemple de sortie :
   ```
   id         username          datacenter   storage_type                capacity_gb   bytes_used   ip_addr         lunId   active_transactions
   123456789  IBM02SL1234567-8  dal10        performance_block_storage   20            -            161.12.34.123   0       0   
   ```
   {: screen}

5. Consultez les détails de votre volume et notez les valeurs de **`Target IP`** et **`LUN Id`**.
   ```
   ibmcloud sl block volume-detail <volume_ID>
   ```
   {: pre}

   Exemple de sortie :
   ```
   Name                       Value
   ID                         1234567890
   User name                  IBM123A4567890-1
   Type                       performance_block_storage
   Capacity (GB)              20
   LUN Id                     0
   IOPs                       100
   Datacenter                 dal10
   Target IP                  161.12.34.123
   # of Active Transactions   0
   Replicant Count            0
   ```
   {: screen}

6. Autorisez l'accès du noeud worker non SDS à l'unité de stockage par blocs. Remplacez `<volume_ID>` par l'ID du volume de votre unité de stockage par blocs que vous avez récupéré précédemment, et `<private_worker_IP>` par l'adresse IP privée du noeud worker non SDS que vous souhaitez utiliser pour connecter l'unité.

   ```
   ibmcloud sl block access-authorize <volume_ID> -p <private_worker_IP>
   ```
   {: pre}

   Exemple de sortie :
   ```
   The IP address 123456789 was authorized to access <volume_ID>.
   ```
   {: screen}

7. Vérifiez que votre noeud worker non SDS est bien autorisé et notez les valeurs de **`host_iqn`**, **`username`** et **`password`**.
   ```
   ibmcloud sl block access-list <volume_ID>
   ```
   {: pre}

   Exemple de sortie :
   ```
   ID          name                 type   private_ip_address   source_subnet   host_iqn                                      username   password           allowed_host_id
   123456789   <private_worker_IP>  IP     <private_worker_IP>  -               iqn.2018-09.com.ibm:ibm02su1543159-i106288771   IBM02SU1543159-I106288771   R6lqLBj9al6e2lbp   1146581   
   ```
   {: screen}

   L'autorisation est validée lorsque les paramètres **`host_iqn`**, **`username`** et **`password`** sont affectés.

8. [Connectez les unités de stockage par blocs à vos noeuds worker](#attach_block).


## Connexion de stockage par blocs brut à des noeuds worker non SDS
{: #attach_block}

Pour connecter l'unité de stockage par blocs à un noeud worker non SDS, vous devez créer un volume persistant (PV) avec la classe de stockage {{site.data.keyword.Bluemix_notm}} Block Volume Attacher et les détails de l'unité de stockage par blocs.
{: shortdesc}

**Avant de commencer** :
- Vérifiez que vous avez créé [automatiquement](#automatic_block) ou [manuellement](#manual_block) du stockage par blocs brut, non formaté et non monté dans vos noeuds worker non SDS.
- [Connectez-vous à votre compte. Le cas échéant, ciblez le groupe de ressources approprié. Définissez le contexte pour votre cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

**Pour connecter du stockage par blocs brut à des noeuds worker non SDS** :
1. Préparez la création du volume persistant.  
   - **Si vous avez utilisé le conteneur `mkpvyaml` :**
     1. Ouvrez le fichier `pv-<cluster_name>.yaml`.
        ```
        nano pv-<cluster_name>.yaml
        ```
        {: pre}

     2. Examinez la configuration de vos volumes persistants.

   - **Si vous avez ajouté manuellement du stockage par blocs :**
     1. Créez un fichier `pv.yaml`. La commande suivante crée le fichier avec l'éditeur `nano`.
        ```
        nano pv.yaml
        ```
        {: pre}

     2. Ajoutez les détails de votre unité de stockage par blocs dans le volume persistant (pv).
        ```
        apiVersion: v1
        kind: PersistentVolume
        metadata:
          name: <pv_name>
          annotations:
            ibm.io/iqn: "<IQN_hostname>"
            ibm.io/username: "<username>"
            ibm.io/password: "<password>"
            ibm.io/targetip: "<targetIP>"
            ibm.io/lunid: "<lunID>"
            ibm.io/nodeip: "<private_worker_IP>"
            ibm.io/volID: "<volume_ID>"
        spec:
          capacity:
            storage: <size>
          accessModes:
            - ReadWriteOnce
          hostPath:
              path: /
          storageClassName: ibmc-block-attacher
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
      	<td>Entrez un nom pour votre volume persistant.</td>
      	</tr>
        <tr>
        <td><code>ibm.io/iqn</code></td>
        <td>Entrez le nom d'hôte IQN que vous avez récupéré précédemment. </td>
        </tr>
        <tr>
        <td><code>ibm.io/username</code></td>
        <td>Entrez le nom d'utilisateur de l'infrastructure IBM Cloud (SoftLayer) que vous avez récupéré précédemment. </td>
        </tr>
        <tr>
        <td><code>ibm.io/password</code></td>
        <td>Entrez le mot de passe de l'infrastructure IBM Cloud (SoftLayer) que vous avez récupéré précédemment. </td>
        </tr>
        <tr>
        <td><code>ibm.io/targetip</code></td>
        <td>Entrez l'adresse IP cible que vous avez récupérée précédemment. </td>
        </tr>
        <tr>
        <td><code>ibm.io/lunid</code></td>
        <td>Entrez l'ID de numéro d'unité logique (LUN) de votre unité de stockage par blocs que vous avez récupéré précédemment. </td>
        </tr>
        <tr>
        <td><code>ibm.io/nodeip</code></td>
        <td>Entrez l'adresse IP privée du noeud worker à laquelle connecter l'unité de stockage par blocs et que vous avez autorisée auparavant à accéder à cette unité. </td>
        </tr>
        <tr>
          <td><code>ibm.io/volID</code></td>
        <td>Entrez l'ID du volume de stockage par blocs que vous avez récupéré précédemment. </td>
        </tr>
        <tr>
        <td><code>storage</code></td>
        <td>Entrez la taille de l'unité de stockage par blocs que vous avez créée précédemment. Par exemple, si votre unité comporte 20 gigaoctets, entrez <code>20Gi</code>.  </td>
        </tr>
        </tbody>
        </table>
2. Créez le volume persistant pour connecter l'unité de stockage par blocs à votre noeud worker non SDS.
   - **Si vous avez utilisé le conteneur `mkpvyaml` :**
     ```
     kubectl apply -f pv-<cluster_name>.yaml
     ```
     {: pre}

   - **Si vous avez ajouté manuellement le stockage par blocs :**
     ```
     kubectl apply -f  block-volume-attacher/pv.yaml
     ```
     {: pre}

3. Vérifiez que le stockage par blocs est bien connecté à votre noeud worker.
   ```
   kubectl describe pv <pv_name>
   ```
   {: pre}

   Exemple de sortie :
   ```
   Name:            kube-wdc07-cr398f790bc285496dbeb8e9137bc6409a-w1-pv1
   Labels:          <none>
   Annotations:     ibm.io/attachstatus=attached
                    ibm.io/dm=/dev/dm-1
                    ibm.io/iqn=iqn.2018-09.com.ibm:ibm02su1543159-i106288771
                    ibm.io/lunid=0
                    ibm.io/mpath=3600a09803830445455244c4a38754c66
                    ibm.io/nodeip=10.176.48.67
                    ibm.io/password=R6lqLBj9al6e2lbp
                    ibm.io/targetip=161.26.98.114
                    ibm.io/username=IBM02SU1543159-I106288771
                    kubectl.kubernetes.io/last-applied-configuration={"apiVersion":"v1","kind":"PersistentVolume","metadata":{"annotations":{"ibm.io/iqn":"iqn.2018-09.com.ibm:ibm02su1543159-i106288771","ibm.io/lunid":"0"...
   Finalizers:      []
   StorageClass:    ibmc-block-attacher
   Status:          Available
   Claim:
   Reclaim Policy:  Retain
   Access Modes:    RWO
   Capacity:        20Gi
   Node Affinity:   <none>
   Message:
   Source:
       Type:          HostPath (bare host directory volume)
       Path:          /
       HostPathType:
   Events:            <none>
   ```
   {: screen}

   L'unité de stockage par blocs est correctement connectée lorsque la valeur de **ibm.io/dm** est définie avec un ID d'unité, tel que `/dev/dm/1`, et que vous voyez **ibm.io/attachstatus=attached** dans la section **Annotations** de la sortie de votre interface de ligne de commande.

Si vous souhaitez déconnecter un volume, supprimez le volume persistant. Les volumes déconnectés sont toujours accessibles à un noeud worker spécifique et sont reconnectés lorsque vous créez un nouveau volume persistant avec la classe de stockage {{site.data.keyword.Bluemix_notm}} Block Volume Attacher pour connecter un autre volume au même noeud worker. Pour éviter de reconnecter l'ancien volume déconnecté, n'autorisez plus le noeud worker à accéder au volume déconnecté en exécutant la commande `ibmcloud sl block access-revoke`. La déconnexion du volume ne retire pas le volume de votre compte d'infrastructure IBM Cloud (SoftLayer). Pour annuler la facturation de votre volume, vous devez [retirer manuellement le stockage de votre compte d'infrastructure IBM Cloud (SoftLayer)](/docs/containers?topic=containers-cleanup).
{: note}


