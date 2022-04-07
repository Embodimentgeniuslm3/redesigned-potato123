---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-05"

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



# Traitement des incidents liés au stockage en cluster
{: #cs_troubleshoot_storage}

Lorsque vous utilisez {{site.data.keyword.containerlong}}, tenez compte des techniques décrites ci-dessous pour identifier et résoudre les incidents liés au stockage en cluster.
{: shortdesc}

Si vous rencontrez un problème d'ordre plus général, expérimentez le [débogage de cluster](/docs/containers?topic=containers-cs_troubleshoot).
{: tip}


## Débogage des incidents de stockage persistant
{: #debug_storage}

Passez en revue les options permettant de déboguer le stockage persistant et d'identifier les causes premières des échecs.
{: shortdesc}

1. Vérifiez que vous utilisez la dernière version du plug-in {{site.data.keyword.Bluemix_notm}} et {{site.data.keyword.containerlong_notm}}.  
   ```
   ibmcloud update
   ```
   {: pre}
   
   ```
   ibmcloud plugin repo-plugins
   ```
   {: pre}

2. Vérifiez que la version de l'interface CLI `kubectl` que vous exécutez sur votre machine locale correspond à la version Kubernetes qui est installée dans votre cluster.  
   1. Affichez la version de l'interface CLI `kubectl` qui est installée dans votre cluster et sur votre machine locale.
      ```
      kubectl version
      ```
      {: pre} 
   
      Exemple de sortie : 
      ```
      Client Version: version.Info{Major:"1", Minor:"13", GitVersion:"v1.13.5", GitCommit:"641856db18352033a0d96dbc99153fa3b27298e5", GitTreeState:"clean", BuildDate:"2019-03-25T15:53:57Z", GoVersion:"go1.12.1", Compiler:"gc", Platform:"darwin/amd64"}
      Server Version: version.Info{Major:"1", Minor:"13", GitVersion:"v1.13.5+IKS", GitCommit:"e15454c2216a73b59e9a059fd2def4e6712a7cf0", GitTreeState:"clean", BuildDate:"2019-04-01T10:08:07Z", GoVersion:"go1.11.5", Compiler:"gc", Platform:"linux/amd64"}
      ```
      {: screen}
    
      Les versions d'interface CLI correspondent si la même version est visible dans `GitVersion` pour le client et le serveur. Vous pouvez ignorer la partie `+IKS` de la version du serveur.
   2. Si les versions de l'interface CLI `kubectl` sur votre machine locale et votre cluster ne correspondent pas, [mettez à jour votre cluster](/docs/containers?topic=containers-update) ou [installez une autre version d'interface CLI sur votre machine locale](/docs/containers?topic=containers-cs_cli_install#kubectl). 

3. Pour le stockage par blocs, le stockage d'objets et Portworx uniquement : vérifiez que vous avez [installé le serveur Helm Tiller avec un compte de services Kubernetes](/docs/containers?topic=containers-helm#public_helm_install). 

4. Pour le stockage par blocs, le stockage d'objets et Portworx uniquement : vérifiez que vous avez installé la dernière version de la charte Helm pour le plug-in.  
   
   **Stockage par blocs et stockage d'objets** : 
   
   1. Mettez à jour vos référentiels de charte Helm.
      ```
      helm repo update
      ```
      {: pre}
      
   2. Répertoriez les chartes Helm dans le référentiel `iks-charts`.
      ```
      helm search iks-charts
      ```
      {: pre}
      
      Exemple de sortie : 
      ```
      iks-charts/ibm-block-storage-attacher          	1.0.2        A Helm chart for installing ibmcloud block storage attach...
      iks-charts/ibm-iks-cluster-autoscaler          	1.0.5        A Helm chart for installing the IBM Cloud cluster autoscaler
      iks-charts/ibm-object-storage-plugin           	1.0.6        A Helm chart for installing ibmcloud object storage plugin  
      iks-charts/ibm-worker-recovery                 	1.10.46      IBM Autorecovery system allows automatic recovery of unhe...
      ...
      ```
      {: screen}
      
   3. Répertoriez les chartes Helm installées dans votre cluster et comparez la version que vous avez installée avec la version qui est disponible.
      ```
      helm ls
      ```
      {: pre}
      
   4. Si une version plus récente est disponible, installez-la. Pour obtenir des instructions, voir [Mise à jour du plug-in {{site.data.keyword.Bluemix_notm}} Block Storage](/docs/containers?topic=containers-block_storage#updating-the-ibm-cloud-block-storage-plug-in) et [Mise à jour du plug-in {{site.data.keyword.cos_full_notm}}](/docs/containers?topic=containers-object_storage#update_cos_plugin). 
   
   **Portworx**: 
   
   1. Recherchez la [dernière version de charte Helm ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/portworx/helm/tree/master/charts/portworx) qui est disponible.  
   
   2. Répertoriez les chartes Helm installées dans votre cluster et comparez la version que vous avez installée avec la version qui est disponible.
      ```
      helm ls
      ```
      {: pre}
   
   3. Si une version plus récente est disponible, installez-la. Pour obtenir des instructions, voir [Mise à jour de Portworx dans votre cluster](/docs/containers?topic=containers-portworx#update_portworx). 
   
5. Vérifiez que les pods de pilote de stockage et de plug-in sont à l'état **En cours d'exécution**. 
   1. Répertoriez les pods dans l'espace de nom `kube-system`.
      ```
      kubectl get pods -n kube-system 
      ```
      {: pre}
      
   2. Si les pods ne sont pas à l'état **En cours d'exécution**, obtenez davantage de détails sur le pod pour trouver la cause première de l'échec. Selon l'état de votre pod, vous ne pourrez peut-être pas exécuter toutes les commandes ci-dessous :
      ```
      kubectl describe pod <pod_name> -n kube-system
      ```
      {: pre}
      
      ```
      kubectl logs <pod_name> -n kube-system
      ```
      {: pre}
      
   3. Analysez la section **Events** de la sortie CLI de la commande `kubectl describe pod` et les derniers journaux pour trouver la cause première de l'erreur.  
   
6. Vérifiez si votre réservation de volume persistant a été correctement mise à disposition.  
   1. Vérifiez l'état de votre réservation de volume persistant. Une réservation de volume persistant est correctement mise à disposition si elle est à l'état **Lié**.
      ```
      kubectl get pvc
      ```
      {: pre}
      
   2. Si l'état de la réservation de volume persistant est **En attente**, récupérez le message d'erreur correspondant.
      ```
      kubectl describe pvc <pvc_name>
      ```
      {: pre}
      
   3. Passez en revue les erreurs courantes qui sont susceptibles de se produire durant la création de réservation de volume persistant.  
      - [Stockage de fichiers et stockage par blocs : la réservation de volume persistant reste à l'état En attente](#file_pvc_pending)
      - [Stockage d'objets : la réservation de volume persistant reste à l'état En attente](#cos_pvc_pending)
   
7. Vérifiez si le pod qui monte votre instance de stockage est correctement déployé.  
   1. Affichez la liste des pods de votre cluster. Un pod est correctement déployé s'il est à l'état **En cours d'exécution**.
      ```
      kubectl get pods
      ```
      {: pre}
      
   2. Obtenez les détails relatifs à votre pod et vérifiez si des erreurs sont affichées dans la section **Events** de votre sortie CLI.
      ```
      kubectl describe pod <pod_name>
      ```
      {: pre}
   
   3. Extrayez les journaux relatifs à votre application et vérifiez s'ils contiennent des messages d'erreur.
      ```
      kubectl logs <pod_name>
      ```
      {: pre}
   
   4. Passez en revue les erreurs courantes qui sont susceptibles de se produire lorsque vous montez une réservation de volume persistant sur votre application.  
      - [Stockage de fichiers : une application ne peut pas accéder à une réservation de volume persistant ou écrire des données sur une réservation de volume persistant](#file_app_failures)
      - [Stockage par blocs : une application ne peut pas accéder à une réservation de volume persistant ou écrire des données sur une réservation de volume persistant](#block_app_failures)
      - [Stockage d'objets : l'accès aux fichiers avec un utilisateur non root échoue](#cos_nonroot_access)
      

## Stockage de fichiers et stockage par blocs : la réservation de volume persistant reste à l'état En attente
{: #file_pvc_pending}

{: tsSymptoms}
Lorsque vous créez une réservation de volume persistant et que vous exécutez `kubectl get pvc <pvc_name>`, votre réservation de volume persistant reste à l'état **En attente**, même après un certain temps d'attente.  

{: tsCauses}
Durant la création et la liaison d'une réservation de volume persistant, de nombreuses tâches diverses sont exécutées par le plug-in de stockage de fichiers et de stockage par blocs. Chaque tâche peut échouer et générer un message d'erreur différent. 

{: tsResolve}

1. Recherchez la cause première du problème lié au fait que la réservation de volume persistant reste à l'état **En attente**.  
   ```
   kubectl describe pvc <pvc_name> -n <namespace>
   ```
   {: pre}
   
2. Passez en revue les descriptions et les résolutions relatives aux messages d'erreur courants. 
   
   <table>
   <thead>
     <th>Message d'erreur</th>
     <th>Description</th>
     <th>Procédure de résolution</th>
  </thead>
  <tbody>
    <tr>
      <td><code>User doesn't have permissions to create or manage Storage</code></br></br><code>Failed to find any valid softlayer credentials in configuration file</code></br></br><code>Storage with the order ID %d could not be created after retrying for %d seconds.</code></br></br><code>Unable to locate datacenter with name <datacenter_name>.</code></td>
      <td>La clé d'API IAM ou la clé d'API de l'infrastructure IBM Cloud (SoftLayer) qui est stockée dans la valeur confidentielle Kubernetes `storage-secret-store` de votre cluster ne dispose pas des droits requis pour mettre à disposition le stockage persistant. </td>
      <td>Voir [Echec de la création de réservation de volume persistant en raison de droits manquants](#missing_permissions). </td>
    </tr>
    <tr>
      <td><code>Your order will exceed the maximum number of storage volumes allowed. Please contact Sales</code></td>
      <td>Chaque compte {{site.data.keyword.Bluemix_notm}} est configuré avec un nombre maximal d'instances de stockage pouvant être créés. En créant la réservation de volume persistant, vous dépassez le nombre maximal d'instances de stockage. </td>
      <td>Pour créer une réservation de volume persistant, choisissez l'une des options ci-après. <ul><li>Retirez les réservations de volume persistant inutilisées.</li><li>Demande au propriétaire du compte {{site.data.keyword.Bluemix_notm}} d'augmenter votre quota de stockage en [ouvrant un cas de support](/docs/get-support?topic=get-support-getting-customer-support).</li></ul> </td>
    </tr>
    <tr>
      <td><code>Unable to find the exact ItemPriceIds(type|size|iops) for the specified storage</code> </br></br><code>Failed to place storage order with the storage provider</code></td>
      <td>La taille de stockage et les IOPS que vous avez spécifiées dans votre réservation de volume persistant ne sont pas prises en charge par le type de stockage que vous avez choisi et ne peuvent pas être utilisées avec la classe de stockage spécifiée. </td>
      <td>Consultez les sections [Détermination de la configuration de stockage de fichiers](/docs/containers?topic=containers-file_storage#file_predefined_storageclass) et [Détermination de la configuration de stockage par blocs](/docs/containers?topic=containers-block_storage#block_predefined_storageclass) pour identifier les tailles de stockage et les IOPS prises en charge pour la classe de stockage que vous souhaitez utiliser. Corrigez la taille et les IOPS, puis recréez la réservation de volume persistant. </td>
    </tr>
    <tr>
  <td><code>Failed to find the datacenter name in configuration file</code></td>
      <td>Le centre de données que vous avez spécifié dans votre réservation de volume persistant n'existe pas. </td>
  <td>Exécutez la commande <code>ibmcloud ks locations</code> pour afficher la liste des centres de données disponibles. Corrigez le centre de données dans votre réservation de volume persistant et recréez celle-ci. </td>
    </tr>
    <tr>
  <td><code>Failed to place storage order with the storage provider</code></br></br><code>Storage with the order ID 12345 could not be created after retrying for xx seconds. </code></br></br><code>Failed to do subnet authorizations for the storage 12345.</code><code>Storage 12345 has ongoing active transactions and could not be completed after retrying for xx seconds.</code></td>
  <td>La taille de stockage, les IOPS et le type de stockage sont peut-être incompatibles avec la classe de stockage que vous avez choisie, ou le noeud final d'API de l'infrastructure {{site.data.keyword.Bluemix_notm}} n'est pas disponible actuellement. </td>
  <td>Consultez les sections [Détermination de la configuration de stockage de fichiers](/docs/containers?topic=containers-file_storage#file_predefined_storageclass) et [Détermination de la configuration de stockage par blocs](/docs/containers?topic=containers-block_storage#block_predefined_storageclass) pour identifier les tailles de stockage et les IOPS prises en charge pour la classe de stockage et le type de stockage que vous souhaitez utiliser. Ensuite, supprimez la réservation de volume persistant et recréez-la.
</td>
  </tr>
  <tr>
  <td><code>Failed to find the storage with storage id 12345. </code></td>
  <td>Vous souhaitez créer une réservation de volume persistant pour une instance de stockage existante en utilisant la mise à disposition statique Kubernetes, mais l'instance de stockage que vous avez spécifiée est introuvable. </td>
  <td>Suivez les instructions relatives à la mise à disposition du [stockage de fichiers](/docs/containers?topic=containers-file_storage#existing_file) ou du [stockage par blocs](/docs/containers?topic=containers-block_storage#existing_block) existant dans votre cluster pour être certain d'extraire les informations appropriées pour votre instance de stockage existante. Ensuite, supprimez la réservation de volume persistant et recréez-la.
</td>
  </tr>
  <tr>
  <td><code>Storage type not provided, expected storage type is `Endurance` or `Performance`. </code></td>
  <td>Vous avez créé une classe de stockage personnalisée et spécifié un type de stockage qui n'est pas pris en charge. </td>
  <td>Mettez à jour votre classe de stockage personnalisée afin de spécifier `Endurance` ou `Performance` comme type de stockage. Pour obtenir des exemples de classe de stockage personnalisée, voir les exemples de classe de stockage personnalisée pour le [stockage de fichiers](/docs/containers?topic=containers-file_storage#file_custom_storageclass) et le [stockage par blocs](/docs/containers?topic=containers-block_storage#block_custom_storageclass). </td>
  </tr>
  </tbody>
  </table>
  
## Stockage de fichiers : une application ne peut pas accéder à une réservation de volume persistant ou écrire des données sur une réservation de volume persistant
{: #file_app_failures}

Lorsque vous montez une réservation de volume persistant sur votre pod, il se peut que des erreurs se produisent lors de l'accès à la réservation de volume persistant ou lors de l'écriture de données sur celle-ci.
{: shortdesc}

1. Répertoriez les pods dans votre cluster et examinez l'état du pod. 
   ```
   kubectl get pods
   ```
   {: pre}
   
2. Recherchez la cause première du problème lié au fait que votre application ne peut pas accéder à la réservation de volume persistant ni écrire de données sur la réservation de volume persistant. 
   ```
   kubectl describe pod <pod_name>
   ```
   {: pre}
   
   ```
   kubectl logs <pod_name>
   ```
   {: pre}

3. Passez en revue les erreurs courantes qui sont susceptibles de se produire lorsque vous montez une réservation de volume persistant sur un pod.  
   <table>
   <thead>
     <th>Symptôme ou message d'erreur</th>
     <th>Description</th>
     <th>Procédure de résolution</th>
  </thead>
  <tbody>
    <tr>
      <td>Votre pod est bloqué à l'état <strong>ContainerCreating</strong>. </br></br><code>MountVolume.SetUp failed for volume ... read-only file system</code></td>
      <td>Des problèmes de réseau se sont produits au niveau du système de back end de l'infrastructure {{site.data.keyword.Bluemix_notm}}. Pour protéger vos données et éviter l'altération de données, {{site.data.keyword.Bluemix_notm}} a automatiquement déconnecté le serveur de stockage de fichiers afin d'empêcher les opérations d'écriture sur des partages de fichiers NFS. </td>
      <td>See [Stockage de fichiers : les systèmes de fichiers de vos noeuds worker passent en mode lecture seule](#readonly_nodes)</td>
      </tr>
      <tr>
  <td><code>write-permission</code> </br></br><code>do not have required permission</code></br></br><code>cannot create directory '/bitnami/mariadb/data': Permission denied </code></td>
  <td>Dans votre déploiement, vous avez spécifié un utilisateur non root comme propriétaire du chemin de montage du stockage de fichiers NFS. Par défaut, les utilisateurs non root ne disposent pas de droits en écriture sur le chemin de montage du volume correspondant au stockage sauvegardé par NFS. </td>
  <td>Voir [Stockage de fichiers : l'application échoue lorsqu'un utilisateur non root détient le chemin de montage du stockage de fichiers NFS](#nonroot)</td>
  </tr>
  <tr>
  <td>Si vous avez spécifié un utilisateur non root comme propriétaire du chemin de montage du stockage du stockage de fichiers NFS ou déployé une charte Helm avec un ID utilisateur non root spécifié, l'utilisateur ne peut pas écrire de données sur le stockage monté. </td>
  <td>Le déploiement ou la configuration de charte Helm spécifie le contexte de sécurité pour les ID <code>fsGroup</code> (ID de groupe) et <code>runAsUser</code> (ID utilisateur) du pod. </td>
  <td>See [Stockage de fichiers : échec de l'ajout de l'accès d'utilisateur non root au stockage persistant](#cs_storage_nonroot)</td>
  </tr>
</tbody>
</table>

### Stockage de fichiers : les systèmes de fichiers de vos noeuds worker passent en mode lecture seule
{: #readonly_nodes}

{: tsSymptoms}
{: #stuck_creating_state}
Vous pouvez voir l'un des symptômes suivants :
- Lorsque vous exécutez la commande `kubectl get pods -o wide`, vous voyez que plusieurs pods qui s'exécutent sur le même noeud worker sont bloqués à l'état `ContainerCreating`.
- Lorsque vous exécutez une commande `kubectl describe`, vous voyez s'afficher l'erreur suivante à la section **Events** : `MountVolume.SetUp failed for volume ... read-only file system`.

{: tsCauses}
Le système de fichiers sur le noeud worker est en lecture seule.

{: tsResolve}
1.  Sauvegardez les données éventuelles stockées sur le noeud worker ou dans vos conteneurs.
2.  Pour une solution à court terme sur le noeud worker existant, rechargez le noeud worker.
    <pre class="pre"><code>ibmcloud ks worker-reload --cluster &lt;cluster_name&gt; --worker &lt;worker_ID&gt;</code></pre>

Pour une solution à long terme, [mettez à jour le type de machine de votre pool de noeuds worker](/docs/containers?topic=containers-update#machine_type).

<br />



### Stockage de fichiers : l'application échoue lorsqu'un utilisateur non root détient le chemin de montage du stockage de fichiers NFS
{: #nonroot}

{: tsSymptoms}
Une fois que vous [ajoutez du stockage NFS](/docs/containers?topic=containers-file_storage#file_app_volume_mount) dans votre déploiement, le conteneur ne parvient pas à se déployer. Lorsque vous extrayez les journaux de votre conteneur, vous pourrez voir les messages d'erreur suivants. Il y a échec du pod qui est bloqué dans un cycle de rechargement.

```
write-permission
```
{: screen}

```
do not have required permission
```
{: screen}

```
cannot create directory '/bitnami/mariadb/data': Permission denied
```
{: screen}

{: tsCauses}
Par défaut, les utilisateurs non root ne disposent pas de droits en écriture sur le chemin de montage du volume correspondant au stockage sauvegardé par NFS. Certaines images d'application courantes, telles que Jenkins et Nexus3, spécifient un utilisateur non root qui détient le chemin de montage dans le fichier Dockerfile. Lorsque vous créez un conteneur à partir de ce fichier Dockerfile, la création échoue car l'utilisateur non root ne dispose pas des droits nécessaires pour accéder au chemin de montage. Pour octroyer des droits en écriture, vous pouvez modifier le fichier Dockerfile pour ajouter temporairement l'utilisateur non root au groupe d'utilisateurs root avant que les droits sur le chemin de montage soient modifiés, ou utiliser un conteneur init.

Si vous utilisez une charte Helm pour déployer l'image, modifiez le déploiement Helm pour utiliser un conteneur init.
{:tip}



{: tsResolve}
Lorsque vous ajoutez un [conteneur init![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/) dans votre déploiement, vous pouvez octroyer à un utilisateur non root spécifié dans votre fichier Dockerfile les droits en écriture pour le chemin de montage du volume à l'intérieur du conteneur qui pointe vers votre partage de fichiers NFS. Le conteneur init démarre avant le conteneur de votre application. Le conteneur init crée le chemin de montage du volume dans le conteneur, modifie le chemin de montage de sorte à ce que l'utilisateur (non root) correct en soit détenteur, puis se ferme. Ensuite, le conteneur de votre application avec l'utilisateur non root qui doit écrire dans le chemin de montage démarre. Comme le chemin est déjà détenu par l'utilisateur non root, l'écriture dans le chemin de montage aboutit. Si vous ne voulez pas utiliser un conteneur init, vous pouvez modifier le fichier Dockerfile afin d'ajouter l'accès d'utilisateur non root au stockage de fichiers NFS.


Avant de commencer : [connectez-vous à votre compte. Le cas échéant, ciblez le groupe de ressources approprié. Définissez le contexte pour votre cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  Ouvrez le fichier Dockerfile de votre application et récupérez l'ID utilisateur (UID) et l'ID du groupe (GID) de l'utilisateur auquel vous voulez accorder les droits en écriture sur le chemin de montage du volume. Dans l'exemple d'un fichier Dockerfile Jenkins, les informations sont :
    - UID : `1000`
    - GID : `1000`

    **Exemple** :

    ```
    FROM openjdk:8-jdk

    RUN apt-get update &&apt-get install -y git curl &&rm -rf /var/lib/apt/lists/*

    ARG user=jenkins
    ARG group=jenkins
    ARG uid=1000
    ARG gid=1000
    ARG http_port=8080
    ARG agent_port=50000

    ENV JENKINS_HOME /var/jenkins_home
    ENV JENKINS_SLAVE_AGENT_PORT ${agent_port}
    ...
    ```
    {:screen}

2.  Ajoutez du stockage persistant à votre application en créant une réservation de volume persistant (PVC). Cet exemple utilise la classe de stockage `ibmc-file-bronze`. Pour consulter les classes de stockage disponibles, exécutez la commande `kubectl get storageclasses`.

    ```
    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
      name: mypvc
      annotations:
        volume.beta.kubernetes.io/storage-class: "ibmc-file-bronze"
    spec:
      accessModes:
        - ReadWriteMany
      resources:
        requests:
          storage: 20Gi
    ```
    {: codeblock}

3.  Créez la réservation de volume persistant (PVC).

    ```
    kubectl apply -f mypvc.yaml
    ```
    {: pre}

4.  Dans votre fichier `.yaml` de déploiement, ajoutez le conteneur init. Insérez les identificateurs UID et GID que vous avez récupérés précédemment.

    ```
    initContainers:
    - name: initcontainer # Or replace the name
      image: alpine:latest
      command: ["/bin/sh", "-c"]
      args:
        - chown <UID>:<GID> /mount; # Replace UID and GID with values from the Dockerfile
      volumeMounts:
      - name: volume # Or you can replace with any name
        mountPath: /mount # Must match the mount path in the args line
    ```
    {: codeblock}

    **Exemple** pour un déploiement Jenkins :

    ```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: my_pod
    spec:
      replicas: 1
      selector:
        matchLabels:
          app: jenkins      
      template:
        metadata:
          labels:
            app: jenkins
        spec:
          containers:
          - name: jenkins
            image: jenkins
            volumeMounts:
            - mountPath: /var/jenkins_home
              name: volume
          volumes:
          - name: volume
            persistentVolumeClaim:
              claimName: mypvc
          initContainers:
          - name: permissionsfix
            image: alpine:latest
            command: ["/bin/sh", "-c"]
            args:
              - chown 1000:1000 /mount;
            volumeMounts:
            - name: volume
              mountPath: /mount
    ```
    {: codeblock}

5.  Créez le pod et montez la PVC dessus.

    ```
    kubectl apply -f my_pod.yaml
    ```
    {: pre}

6. Vérifiez que le montage du volume sur votre pod a réussi. Notez le nom du pod et le chemin **Containers/Mounts**.

    ```
    kubectl describe pod <my_pod>
    ```
    {: pre}

    **Exemple de sortie** :

    ```
    Name:		    mypod-123456789
    Namespace:	default
    ...
    Init Containers:
    ...
    Mounts:
      /mount from volume (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-cp9f0 (ro)
    ...
    Containers:
      jenkins:
        Container ID:
        Image:		jenkins
        Image ID:
        Port:		  <none>
        State:		Waiting
          Reason:		PodInitializing
        Ready:		False
        Restart Count:	0
        Environment:	<none>
        Mounts:
          /var/jenkins_home from volume (rw)
          /var/run/secrets/kubernetes.io/serviceaccount from default-token-cp9f0 (ro)
    ...
    Volumes:
      myvol:
        Type: PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
        ClaimName: mypvc
        ReadOnly: false

    ```
    {: screen}

7.  Connectez-vous au pod en utilisant le nom du pod que vous avez noté précédemment.

    ```
    kubectl exec -it <my_pod-123456789> /bin/bash
    ```
    {: pre}

8. Vérifiez les droits octroyés pour le chemin de montage de votre conteneur. Dans l'exemple, le chemin de montage est `/var/jenkins_home`.

    ```
    ls -ln /var/jenkins_home
    ```
    {: pre}

    **Exemple de sortie** :

    ```
    jenkins@mypod-123456789:/$ ls -ln /var/jenkins_home
    total 12
    -rw-r--r-- 1 1000 1000  102 Mar  9 19:58 copy_reference_file.log
    drwxr-xr-x 2 1000 1000 4096 Mar  9 19:58 init.groovy.d
    drwxr-xr-x 9 1000 1000 4096 Mar  9 20:16 war
    ```
    {: screen}

    Cette sortie montre que les identificateurs GID et UID de votre fichier Dockerfile (dans cet exemple, `1000` et `1000`) sont propriétaires du chemin de montage à l'intérieur du conteneur.

<br />


### Stockage de fichiers : échec de l'ajout de l'accès d'utilisateur non root au stockage persistant
{: #cs_storage_nonroot}

{: tsSymptoms}
Une fois que vous avez [ajouté l'accès d'utilisateur non root au stockage persistant](#nonroot) ou déployé une charte Helm avec un ID d'utilisateur non root, l'utilisateur n'a pas accès en écriture au stockage monté.

{: tsCauses}
Le déploiement ou la configuration de la charte Helm indique le [contexte de sécurité](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/) pour `fsGroup` (ID de groupe) et `runAsUser` (ID utilisateur) du pod. Actuellement, {{site.data.keyword.containerlong_notm}} ne prend pas en charge la spécification `fsGroup` et n'accepte que `runAsUser` défini avec la valeur `0` (droits de l'utilisateur root).

{: tsResolve}
Supprimez les zones `securityContext` dans la configuration pour `fsGroup` et `runAsUser` du fichier de configuration de l'image, du déploiement ou de la charte Helm, et effectuez un redéploiement. Si vous devez remplacer la propriété du chemin de montage `nobody`, [ajoutez un accès d'utilisateur non root](#nonroot). Une fois que vous avez ajouté la propriété [non root `initContainer`](#nonroot), définissez `runAsUser` au niveau du conteneur et non pas au niveau du pod.

<br />




## Stockage par blocs : une application ne peut pas accéder à une réservation de volume persistant ou écrire des données sur une réservation de volume persistant
{: #block_app_failures}

Lorsque vous montez une réservation de volume persistant sur votre pod, il se peut que des erreurs se produisent lors de l'accès à la réservation de volume persistant ou lors de l'écriture de données sur celle-ci.
{: shortdesc}

1. Répertoriez les pods dans votre cluster et examinez l'état du pod. 
   ```
   kubectl get pods
   ```
   {: pre}
   
2. Recherchez la cause première du problème lié au fait que votre application ne peut pas accéder à la réservation de volume persistant ni écrire de données sur la réservation de volume persistant. 
   ```
   kubectl describe pod <pod_name>
   ```
   {: pre}
   
   ```
   kubectl logs <pod_name>
   ```
   {: pre}

3. Passez en revue les erreurs courantes qui sont susceptibles de se produire lorsque vous montez une réservation de volume persistant sur un pod.  
   <table>
   <thead>
     <th>Symptôme ou message d'erreur</th>
     <th>Description</th>
     <th>Procédure de résolution</th>
  </thead>
  <tbody>
    <tr>
      <td>Votre pod est bloqué à l'état <strong>ContainerCreating</strong> ou <strong>CrashLoopBackOff</strong>. </br></br><code>MountVolume.SetUp failed for volume ... read-only.</code></td>
      <td>Des problèmes de réseau se sont produits au niveau du système de back end de l'infrastructure {{site.data.keyword.Bluemix_notm}}. Pour protéger vos données et éviter l'altération de données, {{site.data.keyword.Bluemix_notm}} a automatiquement déconnecté le serveur de stockage par blocs afin d'empêcher les opérations d'écriture sur des instances de stockage par blocs. </td>
      <td>See [Stockage par blocs : le stockage par blocs passe en lecture seule](#readonly_block)</td>
      </tr>
      <tr>
  <td><code>failed to mount the volume as "ext4", it already contains xfs. Mount error: mount failed: exit status 32</code> </td>
        <td>Vous souhaitez monter une instance de stockage par blocs existante qui a été configurée avec un système de fichiers <code>XFS</code>. Lorsque vous avez créé le volume persistant et la réservation de volume persistant correspondante, vous avez spécifié <code>ext4</code> ou aucun système de fichiers. Le système de fichiers que vous spécifiez dans votre volume persistant doit être identique à celui qui est configuré dans votre instance de stockage par blocs. </td>
  <td>Voir [Stockage par blocs : échec de montage d'un stockage par blocs existant sur un pod en raison d'un système de fichiers incorrect](#block_filesystem)</td>
  </tr>
</tbody>
</table>

### Stockage par blocs : le stockage par blocs passe en lecture seule
{: #readonly_block}

{: tsSymptoms}
Vous pouvez voir les symptômes suivants :
- Lorsque vous exécutez la commande `kubectl get pods -o wide`, vous voyez que plusieurs pods sur le même noeud worker sont bloqués à l'état `ContainerCreating` ou `CrashLoopBackOff`. Tous ces pods utilisent la même instance de stockage par blocs.
- Lorsque vous exécutez une commande `kubectl describe pod`, vous voyez s'afficher l'erreur suivante dans la section **Events** : `MountVolume.SetUp failed for volume ... read-only`.

{: tsCauses}
Si une erreur réseau se produit lorsqu'un pod effectue une opération d'écriture dans un volume, l'infrastructure IBM Cloud (SoftLayer) protège les données sur le volume de toute corruption en faisant passer le volume en mode lecture seule. Les pods qui utilisent ce volume ne peuvent plus continuer à écrire dans le volume et sont en échec.

{: tsResolve}
1. Vérifiez la version du plug-in {{site.data.keyword.Bluemix_notm}} Block Storage installée dans votre cluster.
   ```
   helm ls
   ```
   {: pre}

2. Vérifiez que vous utilisez la [dernière version du plug-in {{site.data.keyword.Bluemix_notm}} Block Storage](https://cloud.ibm.com/kubernetes/solutions/helm-charts/ibm/ibmcloud-block-storage-plugin). Si ce n'est pas le cas, [mettez votre plug-in à jour](/docs/containers?topic=containers-block_storage#updating-the-ibm-cloud-block-storage-plug-in).
3. Si vous avez utilisé un déploiement Kubernetes pour votre pod, redémarrez le pod en échec en le retirant et en laissant Kubernetes le recréer. Si vous n'avez pas utilisé un déploiement, extrayez le fichier YAML qui a été utilisé pour créer votre pod en exécutant la commande `kubectl get pod <pod_name> -o yaml >pod.yaml`. Ensuite, supprimez le pod et recréez-le manuellement.
    ```
    kubectl delete pod <pod_name>
    ```
    {: pre}

4. Vérifiez si le fait de recréer votre pod a résolu le problème. Dans le cas contraire, rechargez le noeud worker.
   1. Retrouvez dans quel noeud worker s'exécute votre pod et notez l'adresse IP privée affectée à votre noeud worker.
      ```
      kubectl describe pod <pod_name> | grep Node
      ```
      {: pre}

      Exemple de sortie :
      ```
      Node:               10.75.XX.XXX/10.75.XX.XXX
      Node-Selectors:  <none>
      ```
      {: screen}

   2. Récupérez l'**ID** de votre noeud worker en utilisant l'adresse IP privée que vous avez obtenue à l'étape précédente.
      ```
      ibmcloud ks workers --cluster <cluster_name_or_ID>
      ```
      {: pre}

   3. [Rechargez le noeud worker](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) normalement.


<br />


### Stockage par blocs : échec de montage d'un stockage par blocs existant sur un pod en raison d'un système de fichiers incorrect
{: #block_filesystem}

{: tsSymptoms}
Lorsque vous exécutez la commande `kubectl describe pod <pod_name>`, l'erreur suivante s'affiche :
```
failed to mount the volume as "ext4", it already contains xfs. Mount error: mount failed: exit status 32
```
{: screen}

{: tsCauses}
Vous disposez d'une unité de stockage par blocs configurée avec un système de fichiers `XFS`. Pour monter cette unité sur votre pod, vous [avez créé un volume persistant (PV)](/docs/containers?topic=containers-block_storage#existing_block) qui spécifiait `ext4` pour votre système de fichiers dans la section `spec/flexVolume/fsType`. Si aucun système de fichiers n'est défini, le volume persistant prend la valeur par défaut `ext4`.
Le volume persistant a été créé et lié à votre instance de stockage par blocs. Cependant, lorsque vous tentez de monter le volume persistant sur votre cluster en utilisant une réservation de volume persistant (PVC) correspondante, le montage du volume échoue. Vous ne pouvez pas monter votre instance de stockage par blocs `XFS` avec un système de fichiers `ext4` sur le pod.

{: tsResolve}
Mettez à jour le système de fichiers dans le volume persistant (PV) existant en remplaçant `ext4` par `XFS`.

1. Affichez la liste des volumes persistants existants dans votre cluster et notez le nom du volume persistant que vous avez utilisé pour votre instance de stockage par blocs.
   ```
   kubectl get pv
   ```
   {: pre}

2. Sauvegardez le fichier YAML du volume persistant (PV) sur votre machine locale.
   ```
   kubectl get pv <pv_name> -o yaml > <filepath/xfs_pv.yaml>
   ```
   {: pre}

3. Ouvrez le fichier YAML et modifiez `fsType` en remplaçant `ext4` par `xfs`.
4. Replacez le volume persistant dans votre cluster.
   ```
   kubectl replace --force -f <filepath/xfs_pv.yaml>
   ```
   {: pre}

5. Connectez-vous au pod sur lequel vous avez monté le volume persistant.
   ```
   kubectl exec -it <pod_name> sh
   ```
   {: pre}

6. Vérifiez que le système de fichiers est passé à `XFS`.
   ```
   df -Th
   ```
   {: pre}

   Exemple de sortie :
   ```
   Filesystem Type Size Used Avail Use% Mounted on /dev/mapper/3600a098031234546d5d4c9876654e35 xfs 20G 33M 20G 1% /myvolumepath
   ```
   {: screen}

<br />



## Stockage d'objets : l'installation du plug-in Helm `ibmc` d'{{site.data.keyword.cos_full_notm}} échoue
{: #cos_helm_fails}

{: tsSymptoms}
Lorsque vous installez le plug-in Helm `ibmc` d'{{site.data.keyword.cos_full_notm}}, l'installation échoue avec les erreurs suivantes :
```
Error: symlink /Users/ibm/ibmcloud-object-storage-plugin/helm-ibmc /Users/ibm/.helm/plugins/helm-ibmc: file exists
```
{: screen}

```
Error: fork/exec /home/iksadmin/.helm/plugins/helm-ibmc/ibmc.sh: permission denied
```
{: screen}

{: tsCauses}
Lorsque le plug-in Helm `ibmc` est installé, un lien symbolique (symlink) est créé du répertoire `./helm/plugins/helm-ibmc` vers le répertoire dans lequel set trouve le plug-in Helm `ibmc` sur votre système local, il s'agit en général de `./ibmcloud-object-storage-plugin/helm-ibmc`. Lorsque vous retirez le plug-in Helm `ibmc` de votre système local, ou que vous déplacez le répertoire du plug-in Helm `ibmc` à un autre emplacement, le lien symbolique est conservé.

Si une erreur `permission denied` est générée, cela signifie que vous ne disposez pas des droits `read`, `write` et `execute` requis sur le fichier bash `ibmc.sh` qui permettent d'exécuter les commandes du plug-in Helm `ibmc`. 

{: tsResolve}

**Pour les erreurs de lien symbolique** : 

1. Retirez le plug-in Helm d'{{site.data.keyword.cos_full_notm}}.
   ```
   rm -rf ~/.helm/plugins/helm-ibmc
   ```
   {: pre}

2. Suivez les instructions décrites dans la [documentation](/docs/containers?topic=containers-object_storage#install_cos) pour réinstaller le plug-in Helm `ibmc` et le plug-in {{site.data.keyword.cos_full_notm}}. 

**Pour les erreurs de droit** : 

1. Modifiez les droits pour le plug-in `ibmc`.  
   ```
   chmod 755 ~/.helm/plugins/helm-ibmc/ibmc.sh
   ```
   {: pre}
   
2. Essayez le plug-in Helm `ibm`.  
   ```
   helm ibmc --help
   ```
   {: pre}
   
3. [Continuez d'installer le plug-in {{site.data.keyword.cos_full_notm}}](/docs/containers?topic=containers-object_storage#install_cos). 


<br />


## Stockage d'objets : la réservation de volume persistant reste à l'état En attente
{: #cos_pvc_pending}

{: tsSymptoms}
Lorsque vous créez une réservation de volume persistant et que vous exécutez `kubectl get pvc <pvc_name>`, votre réservation de volume persistant reste à l'état **En attente**, même après un certain temps d'attente.  

{: tsCauses}
Durant la création et la liaison d'une réservation de volume persistant, de nombreuses tâches diverses sont exécutées par le plug-in {{site.data.keyword.cos_full_notm}}. Chaque tâche peut échouer et générer un message d'erreur différent. 

{: tsResolve}

1. Recherchez la cause première du problème lié au fait que la réservation de volume persistant reste à l'état **En attente**.  
   ```
   kubectl describe pvc <pvc_name> -n <namespace>
   ```
   {: pre}
   
2. Passez en revue les descriptions et les résolutions relatives aux messages d'erreur courants. 
   
   <table>
   <thead>
     <th>Message d'erreur</th>
     <th>Description</th>
     <th>Procédure de résolution</th>
  </thead>
  <tbody>
    <tr>
      <td>`User doesn't have permissions to create or manage Storage`</td>
      <td>La clé d'API IAM ou la clé d'API de l'infrastructure IBM Cloud (SoftLayer) qui est stockée dans la valeur confidentielle Kubernetes `storage-secret-store` de votre cluster ne dispose pas des droits requis pour mettre à disposition le stockage persistant. </td>
      <td>Voir [Echec de la création de réservation de volume persistant en raison de droits manquants](#missing_permissions). </td>
    </tr>
    <tr>
      <td>`cannot get credentials: cannot get secret <secret_name>: secrets "<secret_name>" not found`</td>
      <td>La valeur confidentielle Kubernetes qui détient vos données d'identification de service {{site.data.keyword.cos_full_notm}} n'existe pas dans le même espace de nom que celui de la réservation de volume persistant ou du pod. </td>
      <td>See [La création de la réservation de volume persistant (PVC) ou du pod échoue car la valeur confidentielle Kubernetes est introuvable](#cos_secret_access_fails).</td>
    </tr>
    <tr>
      <td>`cannot get credentials: Wrong Secret Type.Provided secret of type XXXX.Expected type ibm/ibmc-s3fs`</td>
      <td>La valeur confidentielle Kubernetes que vous avez créée pour votre instance de service {{site.data.keyword.cos_full_notm}} n'inclut pas `type: ibm/ibmc-s3fs`.</td>
      <td>Editez la valeur confidentielle Kubernetes qui détient vos données d'identification {{site.data.keyword.cos_full_notm}} à ajouter ou remplacez la valeur de `type` par `ibm/ibmc-s3fs`. </td>
    </tr>
    <tr>
      <td>`Bad value for ibm.io/object-store-endpoint XXXX: scheme is missing. Must be of the form http://<hostname> or https://<hostname>`</br> </br> `Bad value for ibm.io/iam-endpoint XXXX: scheme is missing. Must be of the form http://<hostname< or https://<hostname>`</td>
      <td>Le format du noeud final d'API s3fs ou du noeud final d'API IAM est incorrect, ou le noeud final d'API s3fs n'a pas pu être extrait en fonction de l'emplacement de votre cluster. </td>
      <td>Voir [Echec de la création de réservation de volume persistant en raison d'un noeud final d'API s3fs incorrect](#cos_api_endpoint_failure).</td>
    </tr>
    <tr>
      <td>`object-path cannot be set when auto-create is enabled`</td>
      <td>Vous avez spécifié un sous-répertoire existant dans votre compartiment que vous souhaitez monter sur votre réservation de volume persistant à l'aide de l'annotation `ibm.io/object-path`. Si vous définissez un sous-répertoire, vos devez désactiver la fonction de création automatique de compartiment. </td>
      <td>Dans votre réservation de volume persistant, définissez `ibm.io/auto-create-bucket: "false"` et indiquez le nom du compartiment existant dans `ibm.io/bucket`.</td>
    </tr>
    <tr>
    <td>`bucket auto-create must be enabled when bucket auto-delete is enabled`</td>
    <td>Dans votre réservation de volume persistant, vous définissez `ibm.io/auto-delete-bucket: true` pour supprimer automatiquement vos données, le compartiment et le volume persistant lorsque vous retirez la réservation de volume persistant. Cette option nécessite que la propriété `ibm.io/auto-create-bucket` et la propriété `ibm.io/bucket` aient respectivement pour valeur <strong>true</strong> et `""` au même moment. </td>
    <td>Dans votre réservation de volume persistant, définissez `ibm.io/auto-create-bucket: true` et `ibm.io/bucket: ""` de sorte que votre compartiment soit créé automatiquement avec un nom au format `tmp-s3fs-xxxx`. </td>
    </tr>
    <tr>
    <td>`bucket cannot be set when auto-delete is enabled`</td>
    <td>Dans votre réservation de volume persistant, vous définissez `ibm.io/auto-delete-bucket: true` pour supprimer automatiquement vos données, le compartiment et le volume persistant lorsque vous retirez la réservation de volume persistant. Cette option nécessite que la propriété `ibm.io/auto-create-bucket` et la propriété `ibm.io/bucket` aient respectivement pour valeur <strong>true</strong> et `""` en même temps. </td>
    <td>Dans votre réservation de volume persistant, définissez `ibm.io/auto-create-bucket: true` et `ibm.io/bucket: ""` de sorte que votre compartiment soit créé automatiquement avec un nom au format `tmp-s3fs-xxxx`. </td>
    </tr>
    <tr>
    <td>`cannot create bucket using API key without service-instance-id`</td>
    <td>Si vous souhaitez utiliser des clés d'API IAM pour accéder à votre instance de service {{site.data.keyword.cos_full_notm}}, vous devez stocker la clé d'API et l'ID de l'instance de service {{site.data.keyword.cos_full_notm}} dans une valeur confidentielle Kubernetes. </td>
    <td>Voir [Création d'une valeur confidentielle pour les données d'identification du service Object Storage](/docs/containers?topic=containers-object_storage#create_cos_secret). </td>
    </tr>
    <tr>
      <td>`object-path “<subdirectory_name>” not found inside bucket <bucket_name>`</td>
      <td>Vous avez spécifié un sous-répertoire existant dans votre compartiment que vous souhaitez monter sur votre réservation de volume persistant à l'aide de l'annotation `ibm.io/object-path`. Ce sous-répertoire est introuvable dans le compartiment que vous avez spécifié. </td>
      <td>Vérifiez que le sous-répertoire que vous avez spécifié dans `ibm.io/object-path` existe dans le compartiment que vous avez indiqué dans `ibm.io/bucket`. </td>
    </tr>
        <tr>
          <td>`BucketAlreadyExists: The requested bucket name is not available. The bucket namespace is shared by all users of the system. Please select a different name and try again.`</td>
          <td>Vous avez défini `ibm.io/auto-create-bucket: true` et spécifié un nom de compartiment en même temps, ou vous avez indiqué un nom de compartiment qui existe déjà dans {{site.data.keyword.cos_full_notm}}. Les noms de compartiment doivent être uniques dans toutes les instances de service et régions dans {{site.data.keyword.cos_full_notm}}.  </td>
          <td>Prenez soin de définir `ibm.io/auto-create-bucket: false` et de fournir un nom de compartiment qui est unique dans {{site.data.keyword.cos_full_notm}}. Si vous souhaitez utiliser le plug-in {{site.data.keyword.cos_full_notm}} pour créer automatiquement un nom de compartiment, définissez `ibm.io/auto-create-bucket: true` et `ibm.io/bucket: ""`. Votre compartiment est créé avec un nom unique au format `tmp-s3fs-xxxx`. </td>
        </tr>
        <tr>
          <td>`cannot access bucket <bucket_name>: NotFound: Not Found`</td>
          <td>Vous avez tenté d'accéder à un compartiment que vous n'avez pas créé, ou la classe de stockage et le noeud final d'API s3fs que vous avez spécifiés ne correspondent pas à la classe de stockage et au noeud final d'API s3fs qui étaient utilisés lorsque le compartiment a été créé. </td>
          <td>Voir [Accès impossible à un compartiment existant](#cos_access_bucket_fails). </td>
        </tr>
        <tr>
          <td>`Put https://s3-api.dal-us-geo.objectstorage.service.networklayer.com/<bucket_name>: net/http: invalid header field value "AWS4-HMAC-SHA256 Credential=1234a12a123a123a1a123aa1a123a123\n\n/20190412/us-standard/s3/aws4_request, SignedHeaders=host;x-amz-content-sha256;x-amz-date, Signature=12aa1abc123456aabb12aas12aa123456sb123456abc" for key Authorization`</td>
          <td>Les valeurs contenues dans votre valeur confidentielle Kubernetes ne sont pas correctement codées en base64.  </td>
          <td>Passez en revue les valeurs contenues dans votre valeur confidentielle Kubernetes et codez chacune d'elles en base64. Vous pouvez également utiliser la commande [`kubectl create secret`](/docs/containers?topic=containers-object_storage#create_cos_secret) pour créer une nouvelle valeur confidentielle et laisser Kubernetes coder automatiquement vos valeurs en base64. </td>
        </tr>
        <tr>
          <td>`cannot access bucket <bucket_name>: Forbidden: Forbidden`</td>
          <td>Vous avez spécifié `ibm.io/auto-create-bucket: false` et tenté d'accéder à un compartiment que vous n'aviez pas créé ou la clé d'accès de service ou l'ID de clé d'accès de vos données d'identification HMAC {{site.data.keyword.cos_full_notm}} sont incorrectes. </td>
          <td>Vous ne pouvez pas accéder à un compartiment que vous n'avez pas créé. Créez un nouveau compartiment au lieu de définir `ibm.io/auto-create-bucket: true` et `ibm.io/bucket: ""`. Si vous êtes le propriétaire du compartiment, voir [La création de la PVC échoue en raison de données d'identification incorrectes ou d'un accès refusé](#cred_failure) pour vérifier vos données d'identification. </td>
        </tr>
        <tr>
          <td>`cannot create bucket <bucket_name>: AccessDenied: Access Denied`</td>
          <td>Vous avez spécifié `ibm.io/auto-create-bucket: true` pour créer automatiquement un compartiment dans {{site.data.keyword.cos_full_notm}}, mais le rôle d'accès de service IAM **Lecteur** est affecté aux données d'identification que vous avez indiquées dans la valeur confidentielle Kubernetes. Ce rôle ne permet pas la création de compartiment dans {{site.data.keyword.cos_full_notm}}. </td>
          <td>Voir [La création de la PVC échoue en raison de données d'identification incorrectes ou d'un accès refusé](#cred_failure)</td>
        </tr>
        <tr>
          <td>`cannot create bucket <bucket_name>: AccessForbidden: Access Forbidden`</td>
          <td>Vous avez spécifié `ibm.io/auto-create-bucket: true` et indiqué le nom d'un compartiment existant dans `ibm.io/bucket`. En outre, le rôle d'accès de service IAM **Lecteur** est affecté aux données d'identification que vous avez indiquées dans la valeur confidentielle Kubernetes. Ce rôle ne permet pas la création de compartiment dans {{site.data.keyword.cos_full_notm}}. </td>
          <td>Pour utiliser un compartiment existant, définissez `ibm.io/auto-create-bucket: false` et indiquez le nom de votre compartiment existant dans `ibm.io/bucket`. Pour créer automatiquement un compartiment à l'aide de votre valeur confidentielle Kubernetes existante, définissez `ibm.io/bucket: ""` et suivez les instructions décrites dans la rubrique [La création de la PVC échoue en raison de données d'identification incorrectes ou d'un accès refusé](#cred_failure) pour vérifier les données d'identification dans votre valeur confidentielle Kubernetes. </td>
        </tr>
        <tr>
          <td>`cannot create bucket <bucket_name>: SignatureDoesNotMatch: The request signature we calculated does not match the signature you provided. Check your AWS Secret Access Key and signing method. For more information, see REST Authentication and SOAP Authentication for details`</td>
          <td>La clé d'accès secrète {{site.data.keyword.cos_full_notm}} des données d'identification HMAC que vous avez fournies dans votre valeur confidentielle Kubernetes n'est pas correcte. </td>
          <td>Voir [La création de la PVC échoue en raison de données d'identification incorrectes ou d'un accès refusé](#cred_failure)</td>
        </tr>
        <tr>
          <td>`cannot create bucket <bucket_name>: InvalidAccessKeyId: The AWS Access Key ID you provided does not exist in our records`</td>
          <td>L'ID de clé d'accès {{site.data.keyword.cos_full_notm}} ou la clé d'accès secrète des données d'identification HMAC que vous avez fournies dans votre valeur confidentielle Kubernetes n'est pas correcte. </td>
          <td>Voir [La création de la PVC échoue en raison de données d'identification incorrectes ou d'un accès refusé](#cred_failure)</td>
        </tr>
        <tr>
          <td>`cannot create bucket <bucket_name>: CredentialsEndpointError: failed to load credentials` </br> </br> `cannot access bucket <bucket_name>: CredentialsEndpointError: failed to load credentials`</td>
          <td>La clé d'API {{site.data.keyword.cos_full_notm}} de vos données d'identification IAM et l'identificateur global unique de votre instance de service {{site.data.keyword.cos_full_notm}} ne sont pas corrects. </td>
          <td>Voir [La création de la PVC échoue en raison de données d'identification incorrectes ou d'un accès refusé](#cred_failure)</td>
        </tr>
  </tbody>
    </table>
    

### Stockage d'objets : La création de la réservation de volume persistant (PVC) ou du pod échoue car la valeur confidentielle Kubernetes est introuvable
{: #cos_secret_access_fails}

{: tsSymptoms}
Lorsque vous créez votre réservation de volume persistant ou déployez un pod qui monte la PVC, la création ou le déploiement échoue.

- Exemple de message d'erreur pour l'échec de la création d'une PVC :
  ```
  cannot get credentials: cannot get secret tsecret-key: secrets "secret-key" not found
  ```
  {: screen}

- Exemple de message d'erreur pour l'échec de la création d'un pod :
  ```
  persistentvolumeclaim "pvc-3" not found (répété à 3 reprises)
  ```
  {: screen}

{: tsCauses}
La valeur confidentielle (secret) Kubernetes dans laquelle vous stockez les données d'identification du service {{site.data.keyword.cos_full_notm}}, la PVC et le pod ne figure pas dans le même espace de nom Kubernetes. Lorsque la valeur confidentielle est déployée dans un autre espace de nom que votre PVC ou votre pod, la valeur confidentielle est inaccessible.

{: tsResolve}
Cette tâche nécessite le [rôle de service {{site.data.keyword.Bluemix_notm}} IAM **Auteur** ou **Responsable**](/docs/containers?topic=containers-users#platform) pour tous les espaces de nom.

1. Répertoriez les valeurs confidentielles figurant dans votre cluster et examinez l'espace de nom Kubernetes dans lequel est créée la valeur confidentielle Kubernetes pour votre instance de service {{site.data.keyword.cos_full_notm}}. La valeur confidentielle doit afficher `ibm/ibmc-s3fs` comme **Type**.
   ```
   kubectl get secrets --all-namespaces
   ```
   {: pre}

2. Examinez le fichier de configuration YAML de votre PVC et de votre pod pour vérifier que vous avez utilisé le même espace de nom. Si vous voulez déployer un pod dans un autre espace de nom que celui dans lequel figure votre valeur confidentielle, [créez une autre valeur confidentielle](/docs/containers?topic=containers-object_storage#create_cos_secret) dans cet espace de nom.

3. Créez la PVC ou déployez le pod dans l'espace de nom approprié.

<br />


### Stockage d'objets : la création de la PVC échoue en raison de données d'identification incorrectes ou d'un accès refusé
{: #cred_failure}

{: tsSymptoms}
Lorsque vous créez la PVC, vous voyez un message d'erreur de ce type :

```
SignatureDoesNotMatch: The request signature we calculated does not match the signature you provided. Check your AWS Secret Access Key and signing method. For more information, see REST Authentication and SOAP Authentication for details.
```
{: screen}

```
AccessDenied: Access Denied status code: 403
```
{: screen}

```
CredentialsEndpointError: failed to load credentials
```
{: screen}

```
InvalidAccessKeyId: The AWS Access Key ID you provided does not exist in our records`
```
{: screen}

```
cannot access bucket <bucket_name>: Forbidden: Forbidden
```
{: screen}


{: tsCauses}
Les données d'identification du service {{site.data.keyword.cos_full_notm}} que vous utilisez pour accéder à l'instance de service sont peut-être incorrectes ou n'autorisent que l'accès en lecture à votre compartiment.

{: tsResolve}
1. Dans la navigation de la page des détails du service, cliquez sur **Données d'identification pour le service**.
2. Recherchez vos données d'identification, puis cliquez sur **Afficher les données d'identification**.
3. Dans la section **iam_role_crn**, vérifiez que vous disposez du rôle `Writer` ou `Manager`. Si vous ne disposez pas du rôle approprié, vous devez créer de nouvelles données d'identification de service {{site.data.keyword.cos_full_notm}} avec les droits adéquats.  
4. Si le rôle dont vous disposez est approprié, vérifiez que vous utilisez les valeurs **access_key_id** et **secret_access_key** adéquates dans votre valeur confidentielle Kubernetes secret. 
5. [Créez une nouvelle valeur confidentielle avec les valeurs **access_key_id** et **secret_access_key**](/docs/containers?topic=containers-object_storage#create_cos_secret) mises à jour. 


<br />


### Stockage d'objets : Echec de la création de réservation de volume persistant en raison d'un noeud final d'API s3fs ou IAM incorrect
{: #cos_api_endpoint_failure}

{: tsSymptoms}
Lorsque vous créez la réservation de volume persistant, elle reste à l'état En attente. Après que vous avez exécuté la commande `kubectl describe pvc <pvc_name>`, l'un des messages d'erreur suivants s'affiche : 

```
Bad value for ibm.io/object-store-endpoint XXXX: scheme is missing. Must be of the form http://<hostname> or https://<hostname>
```
{: screen}

```
Bad value for ibm.io/iam-endpoint XXXX: scheme is missing. Must be of the form http://<hostname> or https://<hostname>
```
{: screen}

{: tsCauses}
Le format du noeud final d'API s3fs pour le compartiment que vous souhaitez utiliser est peut-être incorrect, ou votre cluster est déployé dans un emplacement qui est pris en charge dans {{site.data.keyword.containerlong_notm}} mais qui n'est pas encore pris en charge par le plug-in {{site.data.keyword.cos_full_notm}}.  

{: tsResolve}
1. Vérifiez le noeud final d'API s3fs qui a été automatiquement affecté par le plug-in Helm `ibmc` à vos classes de stockage durant l'installation du plug-in {{site.data.keyword.cos_full_notm}}. Le noeud final est basé sur l'emplacement dans lequel votre cluster est déployé.   
   ```
   kubectl get sc ibmc-s3fs-standard-regional -o yaml | grep object-store-endpoint
   ```
   {: pre}

   Si la commande renvoie `ibm.io/object-store-endpoint: NA`, votre cluster est déployé dans un emplacement qui est pris en charge dans {{site.data.keyword.containerlong_notm}} mais qui n'est pas encore pris en charge par le plug-in {{site.data.keyword.cos_full_notm}}. Pour ajouter l'emplacement à {{site.data.keyword.containerlong_notm}}, postez une question dans votre canal Slack public ou ouvrez un cas de support {{site.data.keyword.Bluemix_notm}}. Pour plus d'informations, voir [Aide et assistance](/docs/containers?topic=containers-cs_troubleshoot#ts_getting_help). 
   
2. Si vous avez ajouté manuellement le noeud final d'API s3fs avec l'annotation `ibm.io/endpoint` ou le noeud final d'API IAM avec l'annotation `ibm.io/iam-endpoint` dans votre réservation de volume persistant, vérifiez que vous avez ajouté les noeuds finaux au format `https://<s3fs_api_endpoint>` et `https://<iam_api_endpoint>`. L'annotation écrase les noeuds finaux d'API qui sont définis automatiquement par le plug-in `ibmc` dans les classes de stockage {{site.data.keyword.cos_full_notm}}. 
   ```
   kubectl describe pvc <pvc_name>
   ```
   {: pre}
   
<br />


### Stockage d'objets : accès impossible à un compartiment existant
{: #cos_access_bucket_fails}

{: tsSymptoms}
Lorsque vous créez la PVC, le compartiment dans {{site.data.keyword.cos_full_notm}} est inaccessible. Vous voyez un message d'erreur de ce type :

```
Failed to provision volume with StorageClass "ibmc-s3fs-standard-regional": pvc:1b2345678b69175abc98y873e2:cannot access bucket <bucket_name>: NotFound: Not Found
```
{: screen}

{: tsCauses}
Il se peut que vous ayez utilisé la mauvaise classe de stockage pour accéder à votre compartiment ou que vous ayez tenté d'accéder à un compartiment que vous n'avez pas créé. Vous ne pouvez pas accéder à un compartiment que vous n'avez pas créé.  

{: tsResolve}
1. Dans le [tableau de bord {{site.data.keyword.Bluemix_notm}} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/), sélectionnez votre instance de service {{site.data.keyword.cos_full_notm}}.
2. Sélectionnez **Compartiments**.
3. Examinez les informations de **Classe** et d'**Emplacement** de votre compartiment existant.
4. Choisissez la [classe de stockage appropriée](/docs/containers?topic=containers-object_storage#cos_storageclass_reference).
5. Prenez soin de définir `ibm.io/auto-create-bucket: false` et de fournir le nom correct de votre compartiment existant.  

<br />


## Stockage d'objets : l'accès aux fichiers avec un utilisateur non root échoue
{: #cos_nonroot_access}

{: tsSymptoms}
Vous avez téléchargé des fichiers dans votre instance de service {{site.data.keyword.cos_full_notm}} en utilisant la console ou l'API REST. Lorsque vous essayez d'accéder à ces fichiers avec un utilisateur non root que vous avez défini avec `runAsUser` dans le déploiement de votre application, l'accès à ces fichiers est refusé.

{: tsCauses}
Dans Linux, un fichier ou un répertoire comporte 3 groupes d'accès : `Owner`, `Group` et `Other`. Lorsque vous téléchargez un fichier dans {{site.data.keyword.cos_full_notm}} en utilisant la console ou l'API REST, les droits pour les groupes `Owner`, `Group` et `Other` sont retirés. Les droits correspondant à chaque fichier ressemblent à ceci :

```
d--------- 1 root root 0 Jan 1 1970 <file_name>
```
{: screen}

Lorsque vous téléchargez un fichier en utilisant le plug-in {{site.data.keyword.cos_full_notm}}, les droits correspondant à ce fichier sont conservés et ne changent pas.

{: tsResolve}
Pour accéder au fichier avec un utilisateur non root, cet utilisateur doit disposer des droits en lecture et en écriture pour le fichier. Modifier les droits sur un fichier dans le cadre du déploiement de votre pod nécessite une opération d'écriture. {{site.data.keyword.cos_full_notm}} n'est pas conçu pour les charges de travail d'écriture. La mise à jour des droits lors du déploiement du pod peut empêcher votre pod de passer à l'état `Running` (en cours d'exécution).

Pour y remédier, avant de monter la PVC sur votre pod d'application, créez un autre pod pour définir les droits nécessaires pour l'utilisateur non root.

1. Vérifiez les droits de vos fichiers dans votre compartiment.
   1. Créez un fichier de configuration pour votre pod `test-permission` et nommez le fichier `test-permission.yaml`.
      ```
      apiVersion: v1
      kind: Pod
      metadata:
        name: test-permission
      spec:
        containers:
        - name: test-permission
          image: nginx
          volumeMounts:
          - name: cos-vol
            mountPath: /test
        volumes:
        - name: cos-vol
          persistentVolumeClaim:
            claimName: <pvc_name>
      ```
      {: codeblock}

   2. Créez le pod `test-permission`.
      ```
      kubectl apply -f test-permission.yaml
      ```
      {: pre}

   3. Connectez-vous à votre pod.
      ```
      kubectl exec test-permission -it bash
      ```
      {: pre}

   4. Accédez à votre chemin de montage et répertoriez les droits correspondant à vos fichiers.
      ```
      cd test && ls -al
      ```
      {: pre}

      Exemple de sortie :
      ```
      d--------- 1 root root 0 Jan 1 1970 <file_name>
      ```
      {: screen}

2. Supprimez le pod.
   ```
   kubectl delete pod test-permission
   ```
   {: pre}

3. Créez un fichier de configuration pour le pod que vous utilisez pour corriger les droits de vos fichiers et nommez-le `fix-permission.yaml`.
   ```
   apiVersion: v1
   kind: Pod
   metadata:
     name: fix-permission 
     namespace: <namespace>
   spec:
     containers:
     - name: fix-permission
       image: busybox
       command: ['sh', '-c']
       args: ['chown -R <nonroot_userID> <mount_path>/*; find <mount_path>/ -type d -print -exec chmod u=+rwx,g=+rx {} \;']
       volumeMounts:
       - mountPath: "<mount_path>"
         name: cos-volume
     volumes:
     - name: cos-volume
       persistentVolumeClaim:
         claimName: <pvc_name>
    ```
    {: codeblock}

3. Créez le pod `fix-permission`.
   ```
   kubectl apply -f fix-permission.yaml
   ```
   {: pre}

4. Patientez jusqu'à ce que le pod passe à l'état `Completed`.  
   ```
   kubectl get pod fix-permission
   ```
   {: pre}

5. Supprimez le pod `fix-permission`.
   ```
   kubectl delete pod fix-permission
   ```
   {: pre}

5. Recréez le pod `test-permission` que vous avez utilisé auparavant pour vérifier les droits.
   ```
   kubectl apply -f test-permission.yaml
   ```
   {: pre}

5. Vérifiez que les droits de vos fichiers sont mis à jour.
   1. Connectez-vous à votre pod.
      ```
      kubectl exec test-permission -it bash
      ```
      {: pre}

   2. Accédez à votre chemin de montage et répertoriez les droits correspondant à vos fichiers.
      ```
      cd test && ls -al
      ```
      {: pre}

      Exemple de sortie :
      ```
      -rwxrwx--- 1 <nonroot_userID> root 6193 Aug 21 17:06 <file_name>
      ```
      {: screen}

6. Supprimez le pod `test-permission`.
   ```
   kubectl delete pod test-permission
   ```
   {: pre}

7. Montez la PVC sur l'application avec l'utilisateur non root.

   Définissez l'utilisateur non root sous `runAsUser` sans définir `fsGroup` dans votre fichier YAML de déploiement en même temps. Définir `fsGroup` déclenche la mise à jour par le plug-in {{site.data.keyword.cos_full_notm}} des droits du groupe pour tous les fichiers dans un compartiment lorsque le pod est déployé. La mise à jour des droits est une opération d'écriture qui peut empêcher votre pod de passer à l'état `Running` (en cours d'exécution).
   {: important}

Après avoir défini les droits de fichier corrects dans votre instance de service {{site.data.keyword.cos_full_notm}}, ne téléchargez pas de fichiers via la console ou l'API REST. Utilisez le plug-in {{site.data.keyword.cos_full_notm}} pour ajouter des fichiers dans votre instance de service.
{: tip}

<br />


   
## Echec de la création de réservation de volume persistant en raison de droits manquants
{: #missing_permissions}

{: tsSymptoms}
Lorsque vous créez une réservation de volume persistant, elle reste à l'état En attente. Lorsque vous exécutez la commande `kubectl describe pvc <pvc_name>`, un message d'erreur semblable à celui présenté ci-dessous s'affiche : 

```
User doesn't have permissions to create or manage Storage
```
{: screen}

{: tsCauses}
La clé d'API IAM ou la clé d'API de l'infrastructure IBM Cloud (SoftLayer) qui est stockée dans la valeur confidentielle Kubernetes `storage-secret-store` de votre cluster ne dispose pas des droits requis pour mettre à disposition le stockage persistant.  

{: tsResolve}
1. Extrayez la clé IAM ou la clé d'API de l'infrastructure IBM Cloud (SoftLayer) qui est stockée dans la valeur confidentielle Kubernetes `storage-secret-store` de votre cluster et vérifiez que la clé d'API correcte est utilisée.  
   ```
   kubectl get secret storage-secret-store -n kube-system -o yaml | grep slclient.toml: | awk '{print $2}' | base64 --decode
   ```
   {: pre}
   
   Exemple de sortie : 
   ```
   [Bluemix]
   iam_url = "https://iam.bluemix.net"
   iam_client_id = "bx"
   iam_client_secret = "bx"
   iam_api_key = "<iam_api_key>"
   refresh_token = ""
   pay_tier = "paid"
   containers_api_route = "https://us-south.containers.bluemix.net"

   [Softlayer]
   encryption = true
   softlayer_username = ""
   softlayer_api_key = ""
   softlayer_endpoint_url = "https://api.softlayer.com/rest/v3"
   softlayer_iam_endpoint_url = "https://api.softlayer.com/mobile/v3"
   softlayer_datacenter = "dal10"
   ```
   {: screen}
   
   La clé d'API IAM est répertoriée dans la section `Bluemix.iam_api_key` de votre sortie CLI. Si aucune valeur `Softlayer.softlayer_api_key` n'est définie au même moment, la clé d'API IAM est utilisée pour déterminer vos droits sur l'infrastructure. La clé d'API IAM est définie automatiquement par l'utilisateur qui exécute la première action nécessitant le rôle de plateforme IAM **Administrateur** dans un groupe de ressources et une région. Si une autre clé d'API est définie dans `Softlayer.softlayer_api_key`, cette clé est prioritaire sur la clé d'API IAM. La valeur `Softlayer.softlayer_api_key` est définie lorsqu'un administrateur de cluster exécute la commande `ibmcloud ks credentials-set`.  
   
2. Si vous souhaitez modifier les données d'identification, mettez à jour la clé d'API qui est utilisée.  
    1.  Pour mettre à jour la clé d'API IAM, utilisez la [commande](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_api_key_reset) `ibmcloud ks api-key-reset`. Pour mettre à jour la clé d'infrastructure IBM Cloud (SoftLayer), utilisez la [commande](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_set) `ibmcloud ks credential-set`. 
    2. Attendez 10 à 15 minutes que la mise à jour de la valeur confidentielle Kubernetes `storage-secret-store` soit effectuée, puis vérifiez que la clé est mise à jour.
       ```
       kubectl get secret storage-secret-store -n kube-system -o yaml | grep slclient.toml: | awk '{print $2}' | base64 --decode
       ```
       {: pre}
   
3. Si la clé d'API est correcte, vérifiez qu'elle est dotée des droits appropriés permettant de mettre à disposition le stockage persistant. 
   1. Contactez le propriétaire de compte pour vérifier les droits de la clé d'API.  
   2. En tant que propriétaire de compte, sélectionnez **Gérer** > **Accès (IAM)** dans la barre de navigation dans la console {{site.data.keyword.Bluemix_notm}}. 
   3. Sélectionnez **Utilisateurs** et recherchez l'utilisation dont vous souhaitez utiliser la clé d'API.  
   4. Dans le menu Actions, sélectionnez **Gérer les détails de l'utilisateur**. 
   5. Accédez à l'onglet **Infrastructure classique**.  
   6. Développez la catégorie **Compte** et vérifiez que les droits **Ajouter/Mettre à niveau stockage (StorageLayer)** sont affectés.  
   7. Développez la catégorie **Services** et vérifiez que les droits **Gestion de stockage** sont affectés.  
4. Retirez la réservation de volume persistant ayant échoué.  
   ```
   kubectl delete pvc <pvc_name>
   ```
   {: pre}
   
5. Recréez la réservation de volume persistant.  
   ```
   kubectl apply -f pvc.yaml
   ```
   {: pre}


## Aide et assistance
{: #storage_getting_help}

Vous avez encore des problèmes avec votre cluster ?
{: shortdesc}

-  Dans le terminal, vous êtes averti des mises à jour disponibles pour l'interface de ligne de commande `ibmcloud` et les plug-ins. Veillez à maintenir votre interface de ligne de commande à jour pour pouvoir utiliser l'ensemble des commandes et des indicateurs.
-   Pour voir si {{site.data.keyword.Bluemix_notm}} est disponible, [vérifiez la page Statut d'{{site.data.keyword.Bluemix_notm}} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/status?selected=status).
-   Publiez une question sur le site [{{site.data.keyword.containerlong_notm}} Slack ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://ibm-container-service.slack.com).
    Si vous n'utilisez pas un ID IBM pour votre compte {{site.data.keyword.Bluemix_notm}}, [demandez une invitation](https://bxcs-slack-invite.mybluemix.net/) sur ce site Slack.
    {: tip}
-   Consultez les forums pour établir si d'autres utilisateurs ont rencontré le même problème. Lorsque vous utilisez les forums pour poser une question, balisez votre question de sorte que les équipes de développement {{site.data.keyword.Bluemix_notm}} la voient.
    -   Si vous avez des questions d'ordre technique sur le développement ou le déploiement de clusters ou d'applications à l'aide d'{{site.data.keyword.containerlong_notm}}, publiez-les sur le site [Stack Overflow ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://stackoverflow.com/questions/tagged/ibm-cloud+containers) en leur adjoignant les balises `ibm-cloud`, `kubernetes` et `containers`.
    -   Pour toute question sur le service et les instructions de mise en route, utilisez le forum [IBM Developer Answers ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix). Incluez les balises `ibm-cloud` et `containers`.
    Voir [Comment obtenir de l'aide](/docs/get-support?topic=get-support-getting-customer-support#using-avatar) pour plus d'informations sur l'utilisation des forums.
-   Contactez le support IBM en ouvrant un cas. Pour savoir comment ouvrir un cas de support IBM ou obtenir les niveaux de support et la gravité des cas, voir [Contacter le support](/docs/get-support?topic=get-support-getting-customer-support).
Lorsque vous signalez un problème, incluez l'ID de votre cluster. Pour identifier l'ID du cluster, exécutez la commande `ibmcloud ks clusters`. Vous pouvez également utiliser l'[outil de débogage et de diagnostic d'{{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-cs_troubleshoot#debug_utility) pour regrouper et exporter des informations pertinentes de votre cluster à partager avec le support IBM.
{: tip}

