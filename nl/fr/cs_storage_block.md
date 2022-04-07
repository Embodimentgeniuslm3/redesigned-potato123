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
{:tip: .tip}rwo
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:preview: .preview}



# Stockage de données sur IBM Block Storage pour IBM Cloud
{: #block_storage}

{{site.data.keyword.Bluemix_notm}} Block Storage est une solution de stockage iSCSI persistant hautes performances que vous pouvez ajouter à vos applications en utilisant des volumes persistants (PV) Kubernetes. Vous pouvez choisir entre des niveaux de stockage prédéfinis avec des tailles en gigaoctets (Go) et un nombre d'opérations d'entrée-sortie par seconde (IOPS) répondant aux exigences de vos charges de travail. Pour déterminer si {{site.data.keyword.Bluemix_notm}} Block Storage est l'option de stockage qui vous convient le mieux, voir [Choix d'une solution de stockage](/docs/containers?topic=containers-storage_planning#choose_storage_solution). Pour obtenir les informations de tarification, voir [Facturation](/docs/infrastructure/BlockStorage?topic=BlockStorage-About#billing).
{: shortdesc}

{{site.data.keyword.Bluemix_notm}} Block Storage est disponible uniquement pour les clusters standard. Si votre cluster n'a pas accès au réseau public, par exemple s'il s'agit d'un cluster privé derrière un pare-feu ou d'un cluster avec uniquement le noeud final de service privé activé, vérifiez que vous avez installé le plug-in {{site.data.keyword.Bluemix_notm}} Block Storage version 1.3.0 ou ultérieure pour vous connecter à votre instance de stockage par blocs sur le réseau privé. Les instances de stockage par blocs sont spécifiques à une seule zone. Si vous disposez d'un cluster à zones multiples, tenez compte des [options de stockage persistant dans les zones multiples](/docs/containers?topic=containers-storage_planning#persistent_storage_overview).
{: important}

## Installation du plug-in {{site.data.keyword.Bluemix_notm}} Block Storage sur votre cluster
{: #install_block}

Installez le plug-in {{site.data.keyword.Bluemix_notm}} Block Storage avec une charte Helm pour configurer des classes de stockage prédéfinies pour le stockage par blocs. Ces classes de stockage vous permettent de créer une réservation de volume persistant (PVC) pour mettre à disposition du stockage par blocs pour vos applications.
{: shortdesc}

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

2.  [Suivez les instructions](/docs/containers?topic=containers-helm#public_helm_install) d'installation du client Helm sur votre machine locale, et installez le serveur Helm (Tiller) avec un compte de service dans votre cluster.

    L'installation du serveur Helm Tiller nécessite une connexion de réseau public au registre public Google Container Registry. Si votre cluster n'a pas accès au réseau public, par exemple s'il s'agit d'un cluster privé derrière un pare-feu ou d'un cluster avec uniquement le noeud final de service privé activé, vous pouvez choisir d'[extraire l'image de Tiller sur votre machine locale et d'insérer cette image dans votre espace de nom dans {{site.data.keyword.registryshort_notm}}](/docs/containers?topic=containers-helm#private_local_tiller), ou d'[installer la charte Helm sans utiliser Tiller](/docs/containers?topic=containers-helm#private_install_without_tiller).
    {: note}

3.  Vérifiez que Tiller est installé avec un compte de service.

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

4. Ajoutez le référentiel de la charte Helm {{site.data.keyword.Bluemix_notm}} dans le cluster dans lequel vous souhaitez utiliser le plug-in {{site.data.keyword.Bluemix_notm}} Block Storage.
   ```
   helm repo add iks-charts https://icr.io/helm/iks-charts
   ```
   {: pre}

5. Mettez à jour le référentiel Helm pour extraire la dernière version de toutes les chartes Helm figurant dans ce référentiel.
   ```
   helm repo update
   ```
   {: pre}

6. Installez le plug-in {{site.data.keyword.Bluemix_notm}} Block Storage. Lorsque vous installez ce plug-in, des classes de stockage par blocs prédéfinies sont ajoutées dans votre cluster.
   ```
   helm install iks-charts/ibmcloud-block-storage-plugin
   ```
   {: pre}

   Exemple de sortie :
   ```
   NAME:   bald-olm
   LAST DEPLOYED: Wed Apr 18 10:02:55 2018
   NAMESPACE: default
   STATUS: DEPLOYED

   RESOURCES:
   ==> v1beta1/DaemonSet
   NAME                           DESIRED  CURRENT  READY  UP-TO-DATE  AVAILABLE  NODE SELECTOR  AGE
   ibmcloud-block-storage-driver  0        0        0      0           0          <none>         0s

   ==> v1beta1/Deployment
   NAME                           DESIRED  CURRENT  UP-TO-DATE  AVAILABLE  AGE
   ibmcloud-block-storage-plugin  1        0        0           0          0s

   ==> v1/StorageClass
   NAME                      PROVISIONER        AGE
   ibmc-block-bronze         ibm.io/ibmc-block  0s
   ibmc-block-custom         ibm.io/ibmc-block  0s
   ibmc-block-gold           ibm.io/ibmc-block  0s
   ibmc-block-retain-bronze  ibm.io/ibmc-block  0s
   ibmc-block-retain-custom  ibm.io/ibmc-block  0s
   ibmc-block-retain-gold    ibm.io/ibmc-block  0s
   ibmc-block-retain-silver  ibm.io/ibmc-block  0s
   ibmc-block-silver         ibm.io/ibmc-block  0s

   ==> v1/ServiceAccount
   NAME                           SECRETS  AGE
   ibmcloud-block-storage-plugin  1        0s

   ==> v1beta1/ClusterRole
   NAME                           AGE
   ibmcloud-block-storage-plugin  0s

   ==> v1beta1/ClusterRoleBinding
   NAME                           AGE
   ibmcloud-block-storage-plugin  0s

   NOTES:
   Thank you for installing: ibmcloud-block-storage-plugin.   Your release is named: bald-olm
   ```
   {: screen}

7. Vérifiez que l'installation a abouti.
   ```
   kubectl get pod -n kube-system | grep block
   ```
   {: pre}

   Exemple de sortie :
   ```
   ibmcloud-block-storage-driver-kh4mt                              1/1       Running   0          27d       10.118.98.19   10.118.98.19
   ibmcloud-block-storage-plugin-58c5f9dc86-pbl4t                   1/1       Running   0          14d       172.21.0.204   10.118.98.19
   ```
   {: screen}

   L'installation réussit lorsque vous voyez un pod `ibmcloud-block-storage-plugin` et un ou plusieurs pods `ibmcloud-block-storage-driver`. Le nombre de pods `ibmcloud-block-storage-driver` est égal au nombre de noeuds worker dans votre cluster. Tous les pods doivent être à l'état **Running**.

8. Vérifiez que les classes de stockage pour le stockage par blocs ont été ajoutées dans votre cluster.
   ```
   kubectl get storageclasses | grep block
   ```
   {: pre}

   Exemple de sortie :
   ```
   ibmc-block-bronze            ibm.io/ibmc-block
   ibmc-block-custom            ibm.io/ibmc-block
   ibmc-block-gold              ibm.io/ibmc-block
   ibmc-block-retain-bronze     ibm.io/ibmc-block
   ibmc-block-retain-custom     ibm.io/ibmc-block
   ibmc-block-retain-gold       ibm.io/ibmc-block
   ibmc-block-retain-silver     ibm.io/ibmc-block
   ibmc-block-silver            ibm.io/ibmc-block
   ```
   {: screen}

9. Répétez ces étapes pour chaque cluster sur lequel vous souhaitez fournir du stockage par blocs.

Vous pouvez maintenant passer à la [création d'une réservation de volume persistant (PVC)](#add_block) pour mettre à disposition du stockage par blocs pour votre application.


### Mise à jour du plug-in {{site.data.keyword.Bluemix_notm}} Block Storage
Vous pouvez mettre à niveau le plug-in {{site.data.keyword.Bluemix_notm}} Block Storage existant à la version la plus récente.
{: shortdesc}

Avant de commencer : [connectez-vous à votre compte. Le cas échéant, ciblez le groupe de ressources approprié. Définissez le contexte pour votre cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

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

3. Recherchez le nom de la charte Helm correspondant au stockage par blocs que vous avez installée dans votre cluster.
   ```
   helm ls | grep ibmcloud-block-storage-plugin
   ```
   {: pre}

   Exemple de sortie :
   ```
   myhelmchart 	1       	Mon Sep 18 15:31:40 2017	DEPLOYED	ibmcloud-block-storage-plugin-0.1.0	default
   ```
   {: screen}

4. Mettez à niveau le plug-in {{site.data.keyword.Bluemix_notm}} Block Storage à la version la plus récente.
   ```
   helm upgrade --force --recreate-pods <helm_chart_name>  iks-charts/ibmcloud-block-storage-plugin
   ```
   {: pre}

5. Facultatif : lorsque vous mettez à jour le plug-in, la classe de stockage `default` n'est pas définie. Pour définir la classe de stockage par défaut par une classe de stockage de votre choix, exécutez la commande suivante.
   ```
   kubectl patch storageclass <storageclass> -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'
   ```
   {: pre}


### Retrait du plug-in {{site.data.keyword.Bluemix_notm}} Block Storage
Si vous ne souhaitez pas mettre à disposition et utiliser {{site.data.keyword.Bluemix_notm}} Block Storage dans votre cluster, vous pouvez désinstaller la charte Helm.
{: shortdesc}

Le retrait du plug-in ne retire pas les réservations de volume persistant (PVC), les volumes persistants (PV) ou les données. Lorsque vous retirez le plug-in, tous les pods associés et les ensembles de démons sont retirés de votre cluster. Vous ne pouvez pas fournir de nouveau stockage par blocs pour votre cluster ou utiliser des réservations de volume persistant et des volumes persistants de stockage par blocs existants une fois le plug-in retiré.
{: important}

Avant de commencer :
- [Connectez-vous à votre compte. Le cas échéant, ciblez le groupe de ressources approprié. Définissez le contexte pour votre cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
- Vérifiez que vous n'avez plus de réservation de volume persistant (PVC) ou de volume persistant (PV) dans votre cluster utilisant du stockage par blocs.

Pour supprimer le plug-in :

1. Recherchez le nom de la charte Helm correspondant au stockage par blocs que vous avez installée dans votre cluster.
   ```
   helm ls | grep ibmcloud-block-storage-plugin
   ```
   {: pre}

   Exemple de sortie :
   ```
   myhelmchart 	1       	Mon Sep 18 15:31:40 2017	DEPLOYED	ibmcloud-block-storage-plugin-0.1.0	default
   ```
   {: screen}

2. Supprimez le plug-in {{site.data.keyword.Bluemix_notm}} Block Storage.
   ```
   helm delete <helm_chart_name>
   ```
   {: pre}

3. Vérifiez que les pods de stockage par blocs sont retirés.
   ```
   kubectl get pod -n kube-system | grep ibmcloud-block-storage-plugin
   ```
   {: pre}
   Le retrait des pods aboutit lorsqu'aucun pod n'est affiché dans la sortie de votre interface de ligne de commande.

4. Vérifiez que les classes de stockage du stockage par blocs sont retirées.
   ```
   kubectl get storageclasses | grep block
   ```
   {: pre}
   Le retrait des classes de stockage aboutit lorsqu'aucune classe de stockage n'est affichée dans la sortie de votre interface de ligne de commande.

<br />



## Détermination de la configuration de stockage par blocs
{: #block_predefined_storageclass}

{{site.data.keyword.containerlong}} fournit des classes de stockage prédéfinies pour le stockage par blocs que vous pouvez utiliser pour mettre à disposition du stockage par blocs avec une configuration spécifique.
{: shortdesc}

Toutes les classes de stockage indiquent le type de stockage par blocs à mettre à disposition, y compris la taille disponible, les opérations d'entrée-sortie par seconde (IOPS), le système de fichiers, ainsi que la règle de conservation.  

Choisissez votre configuration de stockage avec précaution afin de disposer d'une capacité suffisante pour stocker vos données. Après avoir mis à disposition un type de stockage spécifique en utilisant une classe de stockage, vous ne pouvez plus modifier la taille, le type, le nombre d'opérations d'entrée-sortie par seconde (IOPS) ou la règle de conservation de l'unité de stockage. Si vous avez besoin de plus de stockage ou de stockage avec une configuration différente, vous devez [créer une nouvelle instance de stockage et copier les données](/docs/containers?topic=containers-kube_concepts#update_storageclass) de l'ancienne instance de stockage sur la nouvelle.
{: important}

1. Répertoriez les classes de stockage disponibles dans {{site.data.keyword.containerlong}}.
    ```
    kubectl get storageclasses | grep block
    ```
    {: pre}

    Exemple de sortie :
    ```
    $ kubectl get storageclasses
    NAME                         TYPE
    ibmc-block-custom            ibm.io/ibmc-block
    ibmc-block-bronze            ibm.io/ibmc-block
    ibmc-block-gold              ibm.io/ibmc-block
    ibmc-block-silver            ibm.io/ibmc-block
    ibmc-block-retain-bronze     ibm.io/ibmc-block
    ibmc-block-retain-silver     ibm.io/ibmc-block
    ibmc-block-retain-gold       ibm.io/ibmc-block
    ibmc-block-retain-custom     ibm.io/ibmc-block
    ```
    {: screen}

2. Examinez la configuration d'une classe de stockage.
   ```
   kubectl describe storageclass <storageclass_name>
   ```
   {: pre}

   Pour plus d'informations sur chaque classe de stockage, voir [Référence des classes de stockage](#block_storageclass_reference). Si vous ne trouvez pas ce que vous cherchez, envisagez de créer votre propre classe de stockage personnalisée. Pour commencer, consultez les [exemples de classes de stockage personnalisées](#block_custom_storageclass).
   {: tip}

3. Sélectionnez le type de stockage par blocs que vous désirez mettre à disposition.
   - **Classes de stockage Bronze, Silver et Gold :** ces classes de stockage mettent à disposition du [stockage Endurance](/docs/infrastructure/BlockStorage?topic=BlockStorage-About#provendurance). Le stockage Endurance vous permet de choisir la taille de stockage en gigaoctets à des niveaux d'opérations d'entrée-sortie par seconde prédéfinis.
   - **Classe de stockage personnalisée :** cette classe de stockage met à disposition du [stockage Performance](/docs/infrastructure/BlockStorage?topic=BlockStorage-About#provperformance). Avec le stockage Performance, vous disposez d'un contrôle accru sur la taille de stockage et les opérations d'entrée-sortie par seconde.

4. Choisissez la taille et le nombre d'IOPS de votre stockage par blocs. La taille et le nombre d'IOPS définissent le nombre total d'IOPS (opérations d'entrée-sortie par seconde) qui sert d'indicateur pour mesurer la rapidité de votre stockage. Plus votre stockage comporte d'IOPS, plus il traite rapidement les opérations de lecture/écriture.
   - **Classes de stockage Bronze, Silver et Gold :** ces classes de stockage sont fournies avec un nombre fixe d'IOPS par gigaoctet et sont mises à disposition sur des disques durs SSD. Le nombre total d'IOPS dépend de la taille de stockage que vous choisissez. Vous pouvez sélectionner tout nombre entier représentant la taille en gigaoctets dans la plage de taille autorisée, par exemple 20 Gi, 256 Gi ou 11854 Gi. Pour déterminer le nombre total d'IOPS, vous devez multiplier les IOPS par la taille sélectionnée. Par exemple, si vous sélectionnez la taille de stockage 1000Gi dans la classe de stockage Silver fournie avec 4 IOPS par Go, vous disposerez d'un stockage avec au total 4000 IOPS.  
     <table>
         <caption>Tableau des plages de tailles de classe de stockage et nombre d'opérations d'entrée-sortie par seconde (IOPS) par gigaoctet</caption>
         <thead>
         <th>Classe de stockage</th>
         <th>IOPS par gigaoctet</th>
         <th>Plage de tailles en gigaoctets</th>
         </thead>
         <tbody>
         <tr>
         <td>Bronze</td>
         <td>2 IOPS/Go</td>
         <td>20-12000 Gi</td>
         </tr>
         <tr>
         <td>Silver</td>
         <td>4 IOPS/Go</td>
         <td>20-12000 Gi</td>
         </tr>
         <tr>
         <td>Gold</td>
         <td>10 IOPS/Go</td>
         <td>20-4000 Gi</td>
         </tr>
         </tbody></table>
   - **Classe de stockage personnalisée :** lorsque vous choisissez cette classe de stockage, vous disposez d'un contrôle accru sur la taille et les IOPS que vous souhaitez. En ce qui concerne la taille, vous pouvez sélectionner n'importe quel nombre entier comme valeur de gigaoctets dans la plage de tailles autorisée. La taille que vous choisissez détermine la plage d'IOPS dont vous pourrez bénéficier. Vous pouvez choisir une valeur d'IOPS multiple de 100 comprise dans la plage spécifiée. Le nombre d'IOPS que vous choisissez est statique et ne s'adapte pas à la taille du stockage. Par exemple, si vous choisissez 40Gi avec 100 IOPS, le nombre total d'IOPS restera 100. </br></br>e rapport IOPS/gigaoctet détermine le type de disque dur mis à votre disposition. Par exemple, si vous avez 500Gi à 100 IOPS, votre rapport IOPS/gigaoctet est 0,2. Un stockage avec un rapport inférieur ou égal à 0,3 est fourni sur des disques durs SATA. Si votre rapport est supérieur à 0,3, votre stockage est fourni sur des disques durs SSD.
     <table>
         <caption>Tableau des plages de tailles de classe de stockage personnalisée et nombre d'opérations d'entrée-sortie par seconde (IOPS)</caption>
         <thead>
         <th>Plage de tailles en gigaoctets</th>
         <th>Plage d'IOPS en multiples de 100</th>
         </thead>
         <tbody>
         <tr>
         <td>20-39 Gi</td>
         <td>100-1000 IOPS</td>
         </tr>
         <tr>
         <td>40-79 Gi</td>
         <td>100-2000 IOPS</td>
         </tr>
         <tr>
         <td>80-99 Gi</td>
         <td>100-4000 IOPS</td>
         </tr>
         <tr>
         <td>100-499 Gi</td>
         <td>100-6000 IOPS</td>
         </tr>
         <tr>
         <td>500-999 Gi</td>
         <td>100-10000 IOPS</td>
         </tr>
         <tr>
         <td>1000-1999 Gi</td>
         <td>100-20000 IOPS</td>
         </tr>
         <tr>
         <td>2000-2999 Gi</td>
         <td>200-40000 IOPS</td>
         </tr>
         <tr>
         <td>3000-3999 Gi</td>
         <td>200-48000 IOPS</td>
         </tr>
         <tr>
         <td>4000-7999 Gi</td>
         <td>300-48000 IOPS</td>
         </tr>
         <tr>
         <td>8000-9999 Gi</td>
         <td>500-48000 IOPS</td>
         </tr>
         <tr>
         <td>10000-12000 Gi</td>
         <td>1000-48000 IOPS</td>
         </tr>
         </tbody></table>

5. Déterminez si vous voulez conserver vos données après la suppression du cluster ou de la réservation de volume persistant (PVC).
   - Pour conserver vos données, choisissez une classe de stockage `retain`. Lorsque vous supprimez la réservation PVC, seule la PVC est supprimée. Le volume persistant (PV), l'unité de stockage physique dans votre compte d'infrastructure IBM Cloud (SoftLayer), ainsi que vos données existent toujours. Pour récupérer le stockage et l'utiliser à nouveau dans votre cluster, vous devez supprimer le volume persistant et suivre les étapes pour [utiliser du stockage par blocs existant](#existing_block).
   - Si vous souhaitez que le volume persistant, les données et votre unité physique de stockage par blocs soient supprimés en même temps que la réservation PVC, choisissez une classe de stockage sans `retain`.

6. Choisissez une facturation à l'heure ou mensuelle. Pour plus d'informations, consultez la [tarification ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/cloud/block-storage/pricing). Par défaut, toutes les unités de stockage par blocs sont mises à disposition avec une facturation à l'heure.

<br />



## Ajout de stockage par blocs à des applications
{: #add_block}

Créez une réservation de volume persistant (PVC) pour le [provisionnement dynamique](/docs/containers?topic=containers-kube_concepts#dynamic_provisioning) de stockage par blocs pour votre cluster. Le provisionnement dynamique crée automatiquement le volume persistant (PV) correspondant et commande l'unité de stockage réelle dans votre compte d'infrastructure IBM Cloud (SoftLayer).
{:shortdesc}

Le stockage par blocs est fourni avec un mode d'accès de type `ReadWriteOnce`. Vous ne pouvez le monter que sur un seul pod dans un seul noeud worker du cluster à la fois.
{: note}

Avant de commencer :
- Si vous disposez d'un pare-feu, [autorisez l'accès sortant](/docs/containers?topic=containers-firewall#pvc) pour les plages d'adresses IP de l'infrastructure IBM Cloud (SoftLayer) des zones dans lesquelles résident vos clusters, de manière à pouvoir créer des réservations de volume persistant (PVC).
- Installez le [plug-in {{site.data.keyword.Bluemix_notm}} Block Storage](#install_block).
- [Optez pour une classe de stockage prédéfinie](#block_predefined_storageclass) ou créez une [classe de stockage personnalisée](#block_custom_storageclass).

Vous cherchez à déployer du stockage par blocs dans un ensemble avec état (StatefulSet) ? Voir [Utilisation de stockage par blocs dans un ensemble avec état](#block_statefulset) pour plus d'informations.
{: tip}

Pour ajouter du stockage par blocs :

1.  Créez un fichier de configuration pour définir votre réservation de volume persistant (PVC) et sauvegardez la configuration sous forme de fichier `.yaml`.

    -  **Exemple d'utilisation de classes de stockage Bronze, Silver et Gold** :
       Le fichier `.yaml` suivant crée une réservation nommée `mypvc` de la classe de stockage `"ibmc-block-silver"`, facturée à l'heure (`"hourly"`), avec une taille de `24Gi`.

       ```
       apiVersion: v1
       kind: PersistentVolumeClaim
       metadata:
         name: mypvc
         labels:
           billingType: "hourly"
	       region: us-south
           zone: dal13
       spec:
         accessModes:
           - ReadWriteOnce
         resources:
           requests:
             storage: 24Gi
	     storageClassName: ibmc-block-silver
       ```
       {: codeblock}

    -  **Exemple d'utilisation de la classe de stockage personnalisée** :
       Le fichier `.yaml` suivant crée une réservation nommée `mypvc` de la classe de stockage `ibmc-block-retain-custom`, facturée à l'heure (`"hourly"`), avec une taille en gigaoctets de `45Gi` et `"300"` IOPS.

       ```
       apiVersion: v1
       kind: PersistentVolumeClaim
       metadata:
         name: mypvc
         labels:
           billingType: "hourly"
	       region: us-south
           zone: dal13
       spec:
         accessModes:
           - ReadWriteOnce
         resources:
           requests:
             storage: 45Gi
             iops: "300"
	     storageClassName: ibmc-block-retain-custom
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
         <td><code>metadata.labels.billingType</code></td>
         <td>Indiquez la fréquence de calcul de votre facture de stockage, au mois ("monthly") ou à l'heure ("hourly"). La valeur par défaut est "hourly".</td>
       </tr>
       <tr>
       <td><code>metadata.labels.region</code></td>
       <td>Indiquez la région dans laquelle vous souhaitez mettre à disposition votre stockage par blocs. Si vous spécifiez la région, vous devez également indiquer une zone. Si vous n'indiquez pas de région, ou si la région indiquée est introuvable, le stockage est créé dans la même région que votre cluster. <p class="note">Cette option est uniquement prise en charge avec le plug-in IBM Cloud Block Storage version 1.0.1 ou supérieure. Pour les versions antérieures du plug-in, si vous disposez d'un cluster à zones multiples, la zone dans laquelle votre stockage est mis à disposition est sélectionnée en mode circulaire pour équilibrer les demandes de volume uniformément dans toutes les zones. Pour indiquer la zone destinée à votre stockage, vous pouvez d'abord créer une [classe de stockage personnalisée](#block_multizone_yaml). Créez ensuite une réservation de volume persistant (PVC) avec votre classe de stockage personnalisée.</p></td>
       </tr>
       <tr>
       <td><code>metadata.labels.zone</code></td>
	<td>Indiquez la zone dans laquelle vous souhaitez mettre à disposition votre stockage par blocs. Si vous spécifiez la zone, vous devez également indiquer une région. Si vous n'indiquez pas de zone, ou si la zone indiquée est introuvable dans un cluster à zones multiples, la zone est sélectionnée en mode circulaire. <p class="note">Cette option est uniquement prise en charge avec le plug-in IBM Cloud Block Storage version 1.0.1 ou supérieure. Pour les versions antérieures du plug-in, si vous disposez d'un cluster à zones multiples, la zone dans laquelle votre stockage est mis à disposition est sélectionnée en mode circulaire pour équilibrer les demandes de volume uniformément dans toutes les zones. Pour indiquer la zone destinée à votre stockage, vous pouvez d'abord créer une [classe de stockage personnalisée](#block_multizone_yaml). Créez ensuite une réservation de volume persistant (PVC) avec votre classe de stockage personnalisée.</p></td>
	</tr>
        <tr>
        <td><code>spec.resources.requests.storage</code></td>
        <td>Entrez la taille du stockage par blocs en gigaoctets (Gi). Une fois le stockage mis à disposition, vous ne pouvez plus modifier la taille de votre stockage par blocs. Veillez à indiquer une taille correspondant à la quantité de données que vous envisagez de stocker. </td>
        </tr>
        <tr>
        <td><code>spec.resources.requests.iops</code></td>
        <td>Cette option est disponible uniquement pour les classes de stockage personnalisées (`ibmc-block-custom / ibmc-block-retain-custom`). Indiquez le nombre total d'opérations d'entrée-sortie par seconde (IOPS) pour le stockage, en sélectionnant un multiple de 100 dans la plage autorisée. Si vous choisissez une valeur IOPS autre que celle répertoriée, la valeur IOPS est arrondie à la valeur supérieure.</td>
        </tr>
	<tr>
	<td><code>spec.storageClassName</code></td>
	<td>Nom de la classe de stockage que vous envisagez d'utiliser pour mettre à disposition du stockage par blocs. Vous pouvez choisir l'une des [classes de stockage fournies par IBM](#block_storageclass_reference) ou [créer votre propre classe de stockage](#block_custom_storageclass). </br> Si vous n'indiquez pas de classe de stockage, le volume persistant (PV) est créé avec la classe de stockage par défaut <code>ibmc-file-bronze</code>.<p>**Astuce :** pour modifier la classe de stockage par défaut, exécutez la commande <code>kubectl patch storageclass &lt;storageclass&gt; -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'</code> et remplacez <code>&lt;storageclass&gt;</code> par le nom de la classe de stockage.</p></td>
	</tr>
        </tbody></table>

    Si vous souhaitez utiliser une classe de stockage personnalisée, créez votre réservation de volume persistant (PVC) avec le nom de classe de stockage correspondant, un nombre d'IOPS et une taille valides.   
    {: tip}

2.  Créez la réservation de volume persistant (PVC).

    ```
    kubectl apply -f mypvc.yaml
    ```
    {: pre}

3.  Vérifiez que votre PVC est créée et liée au volume persistant (PV). Ce processus peut prendre quelques minutes.

    ```
    kubectl describe pvc mypvc
    ```
    {: pre}

    Exemple de sortie :

    ```
    Name: mypvc
    Namespace: default
    StorageClass: ""
    Status:		Bound
    Volume:		pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2
    Labels:		<none>
    Capacity:	20Gi
    Access Modes:	RWO
    Events:
      FirstSeen	LastSeen	Count	From								SubObjectPath	Type		Reason			Message
      ---------	--------	-----	----								-------------	--------	------			-------
      3m 3m 1 {ibm.io/ibmc-block 31898035-3011-11e7-a6a4-7a08779efd33 } Normal Provisioning External provisioner is provisioning volume  for claim "default/my-persistent-volume-claim"
       3m 1m	 10 {persistentvolume-controller } Normal ExternalProvisioning cannot find provisioner "ibm.io/ibmc-block", expecting that  a volume for the claim is provisioned either manually or via external software
       1m 1m 1 {ibm.io/ibmc-block 31898035-3011-11e7-a6a4-7a08779efd33 } Normal ProvisioningSucceeded	Successfully provisioned volume  pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2

    ```
    {: screen}

4.  {: #block_app_volume_mount}Pour monter le volume persistant (PV) sur votre déploiement, créez un fichier de configuration `.yaml` et spécifiez la réservation de volume persistant (PVC) associée au PV.

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
    <td><code>spec.containers.volumeMounts.mountPath</code></td>
    <td>Chemin absolu du répertoire où est monté le volume dans le conteneur. Les données écrites dans le chemin de montage sont stockées sous le répertoire racine dans votre instance de stockage par blocs physique. Si vous souhaitez partager un volume entre différentes applications, vous pouvez spécifier des [sous-chemins (subPaths) de volume ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/concepts/storage/volumes/#using-subpath) pour chacune de vos applications. </td>
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

<br />




## Utilisation de stockage par blocs existant dans votre cluster
{: #existing_block}

Si vous disposez déjà d'une unité de stockage physique que vous souhaitez utiliser dans votre cluster, vous pouvez créer le volume persistant (PV) et la réservation de volume persistant (PVC) manuellement pour un [provisionnement statique](/docs/containers?topic=containers-kube_concepts#static_provisioning) du stockage.
{: shortdesc}

Avant de commencer à monter votre stockage existant sur une application, vous devez obtenir toutes les informations nécessaires pour votre volume persistant.  

### Etape 1 : Récupération des informations de votre stockage par blocs existant
{: #existing-block-1}

1.  Récupérez ou générez une clé d'API pour votre compte d'infrastructure IBM Cloud (SoftLayer).
    1. Connectez-vous au [portail d'infrastructure IBM Cloud (SoftLayer) ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/classic?).
    2. Sélectionnez **Compte**, puis **Utilisateurs** et ensuite **Liste d'utilisateurs**.
    3. Recherchez votre ID utilisateur.
    4. Dans la colonne **Clé d'API**, cliquez sur **Générer** pour générer la clé d'API ou sur **Afficher** pour afficher votre clé d'API existante.
2.  Récupérez le nom d'utilisateur d'API correspondant à votre compte d'infrastructure IBM Cloud (SoftLayer).
    1. Dans le menu **Liste d'utilisateurs**, sélectionnez votre ID utilisateur.
    2. Dans la section **Informations d'accès à l'API**, retrouvez votre **Nom d'utilisateur de l'API**.
3.  Connectez-vous au plug-in d'interface de ligne de commande (CLI) de l'infrastructure IBM Cloud.
    ```
    ibmcloud sl init
    ```
    {: pre}

4.  Optez pour l'authentification à l'aide du nom d'utilisateur et de la clé d'API de votre compte d'infrastructure IBM Cloud (SoftLayer).
5.  Entrez le nom d'utilisateur et la clé d'API que vous avez récupérés dans les étapes précédentes.
6.  Affichez la liste des unités de stockage par blocs.
    ```
    ibmcloud sl block volume-list
    ```
    {: pre}

    Exemple de sortie :
    ```
    id         username            datacenter   storage_type              capacity_gb   bytes_used   ip_addr         lunId   active_transactions
    38642141   IBM02SEL1543159-1   dal10        endurance_block_storage   20            -            169.xx.xxx.xxx   170     0
    ```
    {: screen}

7.  Notez les valeurs correspondant à `id`, `ip_addr`, `capacity_gb`, `datacenter` et `lunId` de l'unité de stockage par blocs que vous souhaitez monter dans votre cluster. **Remarque :** pour monter du stockage existant sur un cluster, vous devez disposer d'un noeud worker dans la même zone que votre stockage. Pour vérifier la zone de votre noeud worker, exécutez la commande `ibmcloud ks workers --cluster <cluster_name_or_ID>`.

### Etape 2 : Création d'un volume persistant (PV) et d'une réservation de volume persistant (PVC) correspondante
{: #existing-block-2}

1.  Facultatif : si vous disposez de stockage mis à disposition avec une classe de stockage `retain`, lorsque vous supprimez la réservation de volume persistant (PVC), le volume persistant (PV) et l'unité de stockage physique ne sont pas supprimés. Pour réutiliser le stockage dans votre cluster, vous devez d'abord supprimer le volume persistant.
    1. Répertoriez les volumes persistants (PV) existants.
       ```
       kubectl get pv
       ```
       {: pre}

       Recherchez le volume persistant (PV) appartenant à votre stockage persistant. Ce PV est à l'état `released`.

    2. Supprimez le volume persistant.
       ```
       kubectl delete pv <pv_name>
       ```
       {: pre}

    3. Vérifiez que la suppression du volume persistant a abouti.
       ```
       kubectl get pv
       ```
       {: pre}

2.  Créez un fichier de configuration pour votre volume persistant. Incluez les valeurs des paramètres de stockage par blocs suivants : `id`, `ip_addr`, `capacity_gb`, `datacenter` et `lunIdID` que vous avez récupérées précédemment.

    ```
    apiVersion: v1
    kind: PersistentVolume
    metadata:
      name: mypv
      labels:
         failure-domain.beta.kubernetes.io/region: <region>
         failure-domain.beta.kubernetes.io/zone: <zone>
    spec:
      capacity:
        storage: "<storage_size>"
      accessModes:
        - ReadWriteOnce
      flexVolume:
        driver: "ibm/ibmc-block"
        fsType: "<fs_type>"
        options:
          "Lun": "<lun_ID>"
          "TargetPortal": "<IP_address>"
          "VolumeID": "<volume_ID>"
          "volumeName": "<volume_name>"
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
    <td>Entrez le nom du volume persistant que vous désirez créer.</td>
    </tr>
    <tr>
    <td><code>metadata.labels</code></td>
    <td>Entrez la région et la zone que vous avez récupérées précédemment. Vous devez disposer d'au moins un noeud worker dans la même région et dans la même zone que votre stockage persistant pour monter le stockage sur votre cluster. S'il existe déjà un volume persistant pour votre stockage, [ajoutez un libellé de zone et de région](/docs/containers?topic=containers-kube_concepts#storage_multizone) à votre volume persistant.
    </tr>
    <tr>
    <td><code>spec.flexVolume.fsType</code></td>
    <td>Entrez le type de système de fichiers configuré pour votre stockage par blocs existant. Choisissez entre <code>ext4</code> ou <code>xfs</code>. Si vous ne spécifiez pas cette option, le volume persistant (PV) prend la valeur par défaut <code>ext4</code>. Si le mauvais type `fsType` est défini, la création du volume persistant aboutit, mais le montage de ce volume sur un pod est voué à l'échec. </td></tr>	    
    <tr>
    <td><code>spec.capacity.storage</code></td>
    <td>Entrez la taille de stockage du stockage par blocs existant que vous avez récupérée à l'étape précédente dans <code>capacity-gb</code>. La taille de stockage doit être indiquée en gigaoctets, par exemple 20Gi (20 Go) ou 1000Gi (1 To).</td>
    </tr>
    <tr>
    <td><code>flexVolume.options.Lun</code></td>
    <td>Entrez l'ID de numéro d'unité logique de votre stockage par blocs que vous avez récupéré précédemment dans l'élément <code>lunId</code>.</td>
    </tr>
    <tr>
    <td><code>flexVolume.options.TargetPortal</code></td>
    <td>Entrez l'adresse IP de votre stockage par blocs que vous avez récupérée précédemment dans l'élément <code>ip_addr</code>. </td>
    </tr>
    <tr>
	    <td><code>flexVolume.options.VolumeId</code></td>
	    <td>Entrez l'ID de votre stockage par blocs que vous avez récupéré précédemment dans l'élément <code>id</code>.</td>
	    </tr>
	    <tr>
		    <td><code>flexVolume.options.volumeName</code></td>
		    <td>Entrez un nom pour votre volume.</td>
	    </tr>
    </tbody></table>

3.  Créez le volume persistant dans votre cluster.
    ```
    kubectl apply -f mypv.yaml
    ```
    {: pre}

4. Vérifiez que le volume persistant (PV) est créé.
    ```
    kubectl get pv
    ```
    {: pre}

5. Créez un autre fichier de configuration pour créer votre réservation de volume persistant (PVC). Pour que cette réservation corresponde au volume persistant que vous avez créé auparavant, vous devez sélectionner la même valeur pour `storage` et `accessMode`. La zone `storage-class` doit être vide. Si une de ces zones ne correspond pas au volume persistant (PV), un nouveau PV est créé automatiquement à la place.

     ```
     kind: PersistentVolumeClaim
     apiVersion: v1
     metadata:
      name: mypvc
     spec:
      accessModes:
        - ReadWriteOnce
      resources:
        requests:
          storage: "<storage_size>"
      storageClassName:
     ```
     {: codeblock}

6.  Créez votre réservation de volume persistant (PVC).
     ```
     kubectl apply -f mypvc.yaml
     ```
     {: pre}

7.  Vérifiez que votre réservation PVC est créée et liée au volume persistant que vous avez créé auparavant. Ce processus peut prendre quelques minutes.
     ```
     kubectl describe pvc mypvc
     ```
     {: pre}

     Exemple de sortie :

     ```
     Name: mypvc
    Namespace: default
    StorageClass: ""
     Status: Bound
     Volume: pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2
     Labels: <none>
     Capacity: 20Gi
     Access Modes: RWO
     Events:
       FirstSeen LastSeen Count From        SubObjectPath Type Reason Message
       --------- -------- ----- ----        ------------- -------- ------ -------
       3m 3m 1 {ibm.io/ibmc-block 31898035-3011-11e7-a6a4-7a08779efd33 } Normal Provisioning External provisioner is provisioning volume  for claim "default/my-persistent-volume-claim"
       3m 1m	 10 {persistentvolume-controller } Normal ExternalProvisioning cannot find provisioner "ibm.io/ibmc-block", expecting that  a volume for the claim is provisioned either manually or via external software
       1m 1m 1 {ibm.io/ibmc-block 31898035-3011-11e7-a6a4-7a08779efd33 } Normal ProvisioningSucceeded	Successfully provisioned volume  pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2
     ```
     {: screen}

Vous venez de créer un volume persistant que vous avez lié à une réservation PVC. Les utilisateurs du cluster peuvent désormais [monter la réservation PVC](#block_app_volume_mount) sur leurs déploiements et commencer à effectuer des opérations de lecture et d'écriture sur le volume persistant.

<br />



## Utilisation de stockage par blocs dans un ensemble avec état (StatefulSet)
{: #block_statefulset}

Si vous disposez d'une application avec état, telle qu'une base de données, vous pouvez créer des ensembles avec état utilisant du stockage par blocs pour stocker les données de votre application. Sinon, vous pouvez utiliser {{site.data.keyword.Bluemix_notm}} DaaS (Database-as-a-Service) et stocker vos données dans le cloud.
{: shortdesc}

**De quoi dois-je tenir compte en ajoutant du stockage par blocs dans un ensemble avec état ?** </br>
Pour ajouter du stockage dans un ensemble avec état, vous spécifiez la configuration de votre stockage dans la section `volumeClaimTemplates` du fichier YAML de l'ensemble avec état. La section `volumeClaimTemplates` constitue la base de votre réservation de volume persistant (PVC) et peut inclure la classe de stockage et la taille ou le nombre d'opérations d'entrée-sortie par seconde (IOPS) du stockage par blocs que vous souhaitez mettre à disposition. Cependant, si vous prévoyez d'ajouter des libellés (labels) dans la section `volumeClaimTemplates`, Kubernetes n'inclut pas ces libellés en créant la PVC. Vous devez les ajouter directement dans l'ensemble avec état à la place.

Vous ne pouvez pas déployer deux ensembles avec état en même temps. Si vous essayez de créer un ensemble avec état avant le déploiement complet d'un autre ensemble, le déploiement de votre ensemble avec état peut entraîner des résultats imprévisibles.
{: important}

**Comment créer mon ensemble avec état dans une zone spécifique ?** </br>
Dans un cluster à zones multiples, vous pouvez spécifier la zone et la région dans lesquelles créer votre ensemble avec état dans les sections `spec.selector.matchLabels` et `spec.template.metadata.labels` du fichier YAML de votre ensemble avec état. Sinon, vous pouvez ajouter ces libellés dans une [classe de stockage personnalisée](/docs/containers?topic=containers-kube_concepts#customized_storageclass) et utiliser cette classe de stockage dans la section `volumeClaimTemplates` de votre ensemble avec état.

**Puis-je retarder la liaison d'un volume persistant à mon pod avec état en attendant que le pod soit prêt ?**<br>
Oui, vous pouvez [créer une classe de stockage personnalisée](#topology_yaml) pour votre PVC et y inclure la zone [`volumeBindingMode: WaitForFirstConsumer` ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/concepts/storage/storage-classes/#volume-binding-mode).

**Quelles sont les options possibles pour ajouter du stockage par blocs à un ensemble avec état ?** </br>
Si vous souhaitez créer automatiquement votre PVC lorsque vous créez l'ensemble avec état, utilisez la [mise à disposition dynamique](#block_dynamic_statefulset). Vous pouvez également opter pour une [mise à disposition anticipée de vos PVC ou utiliser des PVC existantes](#block_static_statefulset) avec votre ensemble avec état.  

### Mise à disposition dynamique : Création de la PVC lorsque vous créez un ensemble avec état (statefulset)
{: #block_dynamic_statefulset}

Utilisez cette option pour créer automatiquement la PVC lorsque vous créez l'ensemble avec état.
{: shortdesc}

Avant de commencer : [connectez-vous à votre compte. Le cas échéant, ciblez le groupe de ressources approprié. Définissez le contexte pour votre cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1. Vérifiez que tous les ensembles avec état existants de votre cluster sont entièrement déployés. Si un ensemble avec état est en cours de déploiement, vous ne pouvez pas lancer la création de votre ensemble avec état. Vous devez attendre jusqu'à ce que tous les ensembles avec état de votre cluster soient entièrement déployés pour éviter d'obtenir des résultats imprévisibles.
   1. Répertoriez les ensembles avec état existants dans votre cluster.
      ```
      kubectl get statefulset --all-namespaces
      ```
      {: pre}

      Exemple de sortie :
      ```
      NAME              DESIRED   CURRENT   AGE
      mystatefulset     3         3         6s
      ```
      {: screen}

   2. Affichez le **statut des pods** de chaque ensemble avec état pour vérifier que le déploiement de tous les ensembles avec état est terminé.  
      ```
      kubectl describe statefulset <statefulset_name>
      ```
      {: pre}

      Exemple de sortie :
      ```
      Name:               nginx
      Namespace:          default
      CreationTimestamp:  Fri, 05 Oct 2018 13:22:41 -0400
      Selector:           app=nginx,billingType=hourly,region=us-south,zone=dal10
      Labels:             app=nginx
                          billingType=hourly
                          region=us-south
                          zone=dal10
      Annotations:        kubectl.kubernetes.io/last-applied-configuration={"apiVersion":"apps/v1","kind":"StatefulSet","metadata":{"annotations":{},"name":"nginx","namespace":"default"},"spec":{"podManagementPolicy":"Par...
      Replicas:           3 desired | 3 total
      Pods Status:        0 Running / 3 Waiting / 0 Succeeded / 0 Failed
      Pod Template:
        Labels:  app=nginx
                 billingType=hourly
                 region=us-south
                 zone=dal10
      ...
      ```
      {: screen}

      Un ensemble avec état est entièrement déployé lorsque le nombre de répliques que vous trouvez dans la section **Replicas** de la sortie de l'interface de ligne de commande est égale au nombre de pods en cours d'exécution (**Running**) dans la section **Pods Status**. Si un ensemble avec état n'est pas tout à fait déployé, patientez jusqu'à ce que le déploiement soit terminé avant de poursuivre.

2. Créez un fichier de configuration pour votre ensemble avec état et le service que vous utilisez pour exposer cet ensemble.

   - **Exemple d'ensemble avec état qui spécifie une zone :**

     L'exemple suivant montre comment déployer NGINX sous forme d'ensemble avec état avec 3 répliques. Pour chaque réplique, une unité de stockage par blocs de 20 gigaoctets est mise à disposition en fonction des spécifications qui sont définies dans la classe de stockage `ibmc-block-retain-bronze`. Toutes les unités de stockage sont mises à disposition dans la zone `dal10`. Comme le stockage par blocs n'est pas accessible à partir d'autres zones, toutes les répliques de l'ensemble avec état sont également déployées sur des noeuds worker qui se trouvent dans la zone `dal10`.

     ```
     apiVersion: v1
     kind: Service
     metadata:
      name: nginx
      labels:
        app: nginx
     spec:
      ports:
      - port: 80
        name: web
      clusterIP: None
      selector:
        app: nginx
     ---
     apiVersion: apps/v1
     kind: StatefulSet
     metadata:
      name: nginx
     spec:
      serviceName: "nginx"
      replicas: 3
      podManagementPolicy: Parallel
      selector:
        matchLabels:
          app: nginx
          billingType: "hourly"
          region: "us-south"
          zone: "dal10"
      template:
        metadata:
          labels:
            app: nginx
            billingType: "hourly"
            region: "us-south"
            zone: "dal10"
        spec:
          containers:
          - name: nginx
            image: k8s.gcr.io/nginx-slim:0.8
            ports:
            - containerPort: 80
              name: web
            volumeMounts:
            - name: myvol
              mountPath: /usr/share/nginx/html
      volumeClaimTemplates:
      - metadata:
          name: myvol
        spec:
          accessModes:
          - ReadWriteOnce
          resources:
            requests:
              storage: 20Gi
              iops: "300" #required only for performance storage
	      storageClassName: ibmc-block-retain-bronze
     ```
     {: codeblock}

   - **Exemple d'ensemble avec état avec une règle d'anti-affinité et création de stockage par blocs différée :**

     L'exemple suivant montre comment déployer NGINX sous forme d'ensemble avec état avec 3 répliques. L'ensemble avec état n'indique pas la région et la zone où est créé le stockage par blocs. A la place, l'ensemble avec état utilise une règle d'anti-affinité pour garantir que les pods sont répartis sur les noeuds worker et les zones. En définissant la zone `topologykey: failure-domain.beta.kubernetes.io/zone`, le planificateur ne peut pas planifier un pod sur un noeud worker si ce noeud se trouve dans la même zone qu'un pod avec le libellé `app: nginx`. Pour chaque pod d'ensemble avec état, deux PVC sont créées selon la définition indiquée à la section `volumeClaimTemplates`, mais la création des instances de stockage par blocs est retardée jusqu'à ce qu'un pod d'ensemble avec état qui utilise le stockage soit planifié. Cette configuration est appelée [planification de volume tenant compte de la topologie (topology-aware)](https://kubernetes.io/blog/2018/10/11/topology-aware-volume-provisioning-in-kubernetes/).

     ```
     apiVersion: storage.k8s.io/v1
     kind: StorageClass
     metadata:
       name: ibmc-block-bronze-delayed
     parameters:
       billingType: hourly
       classVersion: "2"
       fsType: ext4
       iopsPerGB: "2"
       sizeRange: '[20-12000]Gi'
       type: Endurance
     provisioner: ibm.io/ibmc-block
     reclaimPolicy: Delete
     volumeBindingMode: WaitForFirstConsumer
     ---
     apiVersion: v1
     kind: Service
     metadata:
       name: nginx
       labels:
         app: nginx
     spec:
       ports:
       - port: 80
         name: web
       clusterIP: None
       selector:
         app: nginx
     ---
     apiVersion: apps/v1
     kind: StatefulSet
     metadata:
       name: web
     spec:
       serviceName: "nginx"
       replicas: 3
       podManagementPolicy: "Parallel"
       selector:
         matchLabels:
           app: nginx
       template:
         metadata:
           labels:
             app: nginx
         spec:
           affinity:
             podAntiAffinity:
               preferredDuringSchedulingIgnoredDuringExecution:
               - weight: 100
        podAffinityTerm:
          labelSelector:
            matchExpressions:
                     - key: app
              operator: In
              values:
                       - nginx
                   topologyKey: failure-domain.beta.kubernetes.io/zone
           containers:
           - name: nginx
             image: k8s.gcr.io/nginx-slim:0.8
             ports:
             - containerPort: 80
               name: web
             volumeMounts:
             - name: www
               mountPath: /usr/share/nginx/html
             - name: wwwww
               mountPath: /tmp1
       volumeClaimTemplates:
       - metadata:
           name: myvol1
         spec:
           accessModes:
           - ReadWriteOnce # access mode
           resources:
             requests:
               storage: 20Gi
	       storageClassName: ibmc-block-bronze-delayed
       - metadata:
           name: myvol2
         spec:
           accessModes:
           - ReadWriteOnce # access mode
           resources:
             requests:
               storage: 20Gi
	       storageClassName: ibmc-block-bronze-delayed
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
     <td style="text-align:left"><code>spec.podManagementPolicy</code></td>
     <td style="text-align:left">Entrez la règle de gestion de pod que vous souhaitez utiliser pour votre ensemble avec état. Sélectionnez l'une des options suivantes : <ul><li><strong>`OrderedReady` : </strong>avec cette option, les répliques de l'ensemble avec état sont déployées l'une après l'autre. Par exemple, si vous avez spécifié 3 répliques, Kubernetes crée la PVC pour la première réplique, attend jusqu'à ce que la PVC soit liée, déploie la réplique de l'ensemble avec état et monte la PVC sur la réplique. Une fois le déploiement terminé, la deuxième réplique est déployée. Pour plus d'informations sur cette option, voir [`OrderedReady` Pod Management ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/#orderedready-pod-management). </li><li><strong>Parallel : </strong>avec cette option, le déploiement de toutes les répliques de l'ensemble avec état démarre en même temps. Si votre application prend en charge le déploiement parallèle des répliques, utilisez cette option pour gagner du temps pour le déploiement de vos PVC et des répliques de l'ensemble avec état. </li></ul></td>
     </tr>
     <tr>
     <td style="text-align:left"><code>spec.selector.matchLabels</code></td>
     <td style="text-align:left">Entrez tous les libellés que vous souhaitez inclure dans votre ensemble avec état et votre PVC. Les libellés que vous indiquez dans la section <code>volumeClaimTemplates</code> de votre ensemble avec état ne sont pas reconnus par Kubernetes. Exemples de libellés que vous pouvez envisager d'inclure : <ul><li><code><strong>region</strong></code> et <code><strong>zone</strong></code> : si vous voulez que toutes les répliques de votre ensemble avec état et toutes vos PVC soient créées dans une zone spécifique, ajoutez les deux libellés. Vous pouvez également indiquer la zone et la région dans la classe de stockage que vous utilisez. Si vous n'indiquez pas de zone et de région et que vous disposez d'un cluster à zones multiples, la zone dans laquelle votre stockage est mis à disposition est sélectionnée en mode circulaire pour équilibrer équitablement les demandes de volume sur toutes les zones.</li><li><code><strong>billingType</strong></code> : entrez le type de facturation que vous désirez utiliser pour vos PVC. Choisissez entre une facturation à l'heure (<code>hourly</code>) ou au mois (<code>monthly</code>). Si vous n'indiquez pas ce libellé, toutes les PVC sont créées avec un type de facturation à l'heure. </li></ul></td>
     </tr>
     <tr>
     <td style="text-align:left"><code>spec.template.metadata.labels</code></td>
     <td style="text-align:left">Entrez les mêmes libellés que vous avez ajoutés dans la section <code>spec.selector.matchLabels</code>. </td>
     </tr>
     <tr>
     <td style="text-align:left"><code>spec.template.spec.affinity</code></td>
     <td style="text-align:left">Spécifiez votre règle d'anti-affinité pour garantir que les pods de votre ensemble avec état sont répartis sur les noeuds worker et les zones. L'exemple montre une règle d'anti-affinité dans laquelle le pod de l'ensemble avec état préfère ne pas être planifié sur un noeud worker où s'exécute un pod avec le libellé `app: nginx`. La section `topologykey: failure-domain.beta.kubernetes.io/zone` limite davantage cette règle d'anti-affinité et empêche la planification du pod sur un noeud worker si ce noeud figure dans la même zone que le pod ayant le libellé `app: nginx`. En utilisant cette règle d'anti-affinité, vous pouvez appliquer l'anti-affinité aux différents noeuds worker et aux différentes zones. </td>
     </tr>
     <tr>
     <td style="text-align:left"><code>spec.volumeClaimTemplates.metadata.name</code></td>
     <td style="text-align:left">Entrez un nom pour votre volume. Utilisez le même nom que vous avez défini dans la section <code>spec.containers.volumeMount.name</code>. Le nom que vous indiquez ici est utilisé pour créer le nom de votre PVC au format : <code>&lt;nom_volume&gt;-&lt;nom_ensemble_avec_état&gt;-&lt;nombre_répliques&gt;</code>. </td>
     </tr>
     <tr>
     <td style="text-align:left"><code>spec.volumeClaimTemplates.spec.resources.</code></br><code>requests.storage</code></td>
     <td style="text-align:left">Entrez la taille du stockage par blocs en gigaoctets (Gi).</td>
     </tr>
     <tr>
     <td style="text-align:left"><code>spec.volumeClaimTemplates.spec.resources.</code></br><code>requests.iops</code></td>
     <td style="text-align:left">Pour mettre à disposition du [stockage Performance](#block_predefined_storageclass), entrez le nombre d'opérations d'entrée-sortie par seconde (IOPS). Si vous utilisez une classe de stockage Endurance et que vous indiquez un nombre d'IOPS, le nombre d'IOPS est ignoré. Le nombre d'IOPS indiqué dans votre classe de stockage est utilisé à la place.  </td>
     </tr>
     <tr>
     <td style="text-align:left"><code>spec.volumeClaimTemplates.</code></br><code>spec.storageClassName</code></td>
     <td style="text-align:left">Entrez la classe de stockage de votre choix. Pour répertorier les classes de stockage existantes, exécutez la commande <code>kubectl get storageclasses | grep block</code>. Si vous n'indiquez pas de classe de stockage, la PVC est créée avec la classe de stockage par défaut définie dans votre cluster. Vérifiez que la classe de stockage par défaut comporte <code>ibm.io/ibmc-block</code> dans la section 'provisioner' de sorte que votre ensemble avec état soit mis à disposition avec du stockage par blocs.</td>
     </tr>
     </tbody></table>

4. Créez votre ensemble avec état.
   ```
   kubectl apply -f statefulset.yaml
   ```
   {: pre}

5. Patientez jusqu'à ce que votre ensemble avec état soit déployé.
   ```
   kubectl describe statefulset <statefulset_name>
   ```
   {: pre}

   Pour voir le statut actuel de vos PVC, exécutez la commande `kubectl get pvc`. Le nom de votre PVC est au format suivant : `<volume_name>-<statefulset_name>-<replica_number>`.
   {: tip}

### Mise à disposition statique : Utilisation de PVC existantes avec un ensemble avec état
{: #block_static_statefulset}

Vous pouvez mettre à disposition vos PVC de manière anticipée avant de créer votre ensemble avec état ou utiliser des PVC existantes avec votre ensemble avec état.
{: shortdesc}

Lorsque vous [effectuez une mise à disposition dynamique de vos PVC lors de la création de l'ensemble avec état](#block_dynamic_statefulset), le nom de la PVC est affecté en fonction des valeurs que vous avez utilisées dans le fichier YAML de l'ensemble avec état. Pour que l'ensemble avec état utilise des PVC existantes, le nom de vos PVC doit correspondre à celui qui serait automatiquement créé via une mise à disposition dynamique.

Avant de commencer : [connectez-vous à votre compte. Le cas échéant, ciblez le groupe de ressources approprié. Définissez le contexte pour votre cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1. Si vous souhaitez effectuer une mise à disposition préalable de la PVC pour votre ensemble avec état avant de créer l'ensemble avec état, suivez les étapes 1 à 3 de la section [Ajout de stockage par blocs à des applications](#add_block) pour créer une PVC pour chaque réplique de l'ensemble avec état. Prenez soin de créer votre PVC avec un nom qui respecte le format suivant : `<volume_name>-<statefulset_name>-<replica_number>`.
   - **`<volume_name>`** : utilisez le nom de volume que vous souhaitez indiquer dans la section `spec.volumeClaimTemplates.metadata.name` de votre ensemble avec état, par exemple `nginxvol`.
   - **`<statefulset_name>`** : utilisez le nom d'ensemble avec état que vous souhaitez indiquer dans la section `metadata.name` de votre ensemble avec état, par exemple `nginx_statefulset`.
   - **`<replica_number>`** : entrez le nombre de répliques à partir de 0.

   Par exemple, si vous devez créer 3 répliques de l'ensemble avec état, créez 3 PVC avec les noms suivants : `nginxvol-nginx_statefulset-0`, `nginxvol-nginx_statefulset-1` et `nginxvol-nginx_statefulset-2`.  

   Vous envisagez de créer une PVC et un volume persistant pour une unité de stockage existante ? Créez votre PVC et le volume persistant en utilisant une [mise à disposition statique](#existing_block).

2. Suivez les étapes indiquées à la section [Mise à disposition dynamique : Création de la PVC lorsque vous créez un ensemble avec état](#block_dynamic_statefulset) pour créer votre ensemble avec état. Le nom de votre PVC est au format suivant : `<volume_name>-<statefulset_name>-<replica_number>`. Veillez à utiliser les valeurs suivantes pour le nom de votre PVC dans la spécification de l'ensemble avec état :
   - **`spec.volumeClaimTemplates.metadata.name`** : entrez le nom de volume (`<volume_name>`) du nom de votre PVC.
   - **`metadata.name`** : entrez le nom de l'ensemble avec état (`<statefulset_name>`) du nom de votre PVC.
   - **`spec.replicas`** : entrez le nombre de répliques que vous souhaitez créer pour votre ensemble avec état. Le nombre de répliques doit être égal au nombre de PVC que vous avez créées précédemment.

   Si vos PVC se trouvent dans des zones différentes, n'incluez pas de libellé de région ou de zone dans votre ensemble avec état.
   {: note}

3. Vérifiez que les PVC sont utilisées dans les pods de réplique de votre ensemble avec état.
   1. Affichez la liste des pods de votre cluster. Identifiez les pods appartenant à votre ensemble avec état.
      ```
      kubectl get pods
      ```
      {: pre}

   2. Vérifiez que votre PVC existante est montée sur la réplique de votre ensemble avec état. Examinez la valeur de **`ClaimName`** dans la section **`Volumes`** de la sortie de l'interface de ligne de commande.
      ```
      kubectl describe pod <pod_name>
      ```
      {: pre}

      Exemple de sortie :
      ```
      Name:           nginx-0
      Namespace:      default
      Node:           10.xxx.xx.xxx/10.xxx.xx.xxx
      Start Time:     Fri, 05 Oct 2018 13:24:59 -0400
      ...
      Volumes:
        myvol:
          Type:       PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
          ClaimName:  myvol-nginx-0
     ...
      ```
      {: screen}

<br />


## Modification de la taille et du nombre d'opérations d'entrée-sortie par seconde (IOPS) de votre unité de stockage
{: #block_change_storage_configuration}

Si vous envisagez d'augmenter la capacité de stockage ou les performances, vous pouvez modifier votre volume existant.
{: shortdesc}

Pour toute question concernant la facturation ou la procédure à suivre pour modifier votre stockage en utilisant la console {{site.data.keyword.Bluemix_notm}}, voir [Extension de la capacité de stockage par blocs](/docs/infrastructure/BlockStorage?topic=BlockStorage-expandingcapacity#expandingcapacity) et [Ajustement des IOPS](/docs/infrastructure/BlockStorage?topic=BlockStorage-adjustingIOPS). Les mises à jour que vous effectuez à partir de la console ne sont pas reflétées dans le volume persistant (PV). Pour ajouter ces informations au volume persistant, exécutez `kubectl patch pv <pv_name>` et mettez à jour manuellement la taille et les IOPS dans la section **Labels** et **Annotation** de votre volume persistant.
{: tip}

1. Répertoriez les réservations de volume persistant (PVC) et notez le nom du volume persistant (PV) associé indiqué dans la colonne **VOLUME**.
   ```
   kubectl get pvc
   ```
   {: pre}

   Exemple de sortie :
   ```
   NAME             STATUS    VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS        AGE
   myvol            Bound     pvc-01ac123a-123b-12c3-abcd-0a1234cb12d3   20Gi       RWO            ibmc-block-bronze    147d
   ```
   {: screen}

2. Si vous souhaitez modifier les IOPS et la taille de votre stockage par blocs, commencez par éditer les IOPS dans la section `metadata.labels.IOPS` de votre volume persistant. Vous pouvez passer à une valeur IOPS inférieur ou supérieur. Prenez soin d'entrer une valeur IOPS qui est prise en charge pour votre type de stockage. Par exemple, si vous disposez d'un stockage par blocs de type Endurance avec 4 IOPS, vous pouvez remplacer la valeur IOPS par 2 ou 10. Pour obtenir davantage de valeurs IOPS prises en charge, voir [Détermination de la configuration de votre stockage par blocs](/docs/containers?topic=containers-block_storage#block_predefined_storageclass).
   ```
   kubectl edit pv <pv_name>
   ```
   {: pre}

   Pour modifier la valeur IOPS à partir de l'interface de ligne de commande, vous devez également modifier la taille de votre stockage par blocs Si vous souhaitez modifier uniquement la valeur IOPS, mais pas la taille, vous devez [demander la modification de la valeur IOPS à partir de la console](/docs/infrastructure/BlockStorage?topic=BlockStorage-adjustingIOPS).
   {: note}

3. Editez la réservation de volume persistant et la nouvelle taille dans la section `spec.resources.requests.storage` de votre réservation de volume persistant. Vous pouvez passer à une taille supérieure mais uniquement dans la limite de la capacité maximale qui est définie par votre classe de stockage. Vous ne pouvez pas réduire la taille de votre stockage existant. Pour connaître les tailles valides pour votre classe de stockage, voir [Détermination de la configuration de stockage par blocs](/docs/containers?topic=containers-block_storage#block_predefined_storageclass).
   ```
   kubectl edit pvc <pvc_name>
   ```
   {: pre}

4. Vérifiez que l'extension de volume a été demandée. Vous pouvez déterminer que la demande de l'extension de volume a abouti lorsqu'un message `FileSystemResizePending` apparaît dans la section **Conditions** de la sortie de votre interface de ligne de commande.  
   ```
   kubectl describe pvc <pvc_name>
   ```
   {: pre}

   Exemple de sortie :
   ```
   ...
   Conditions:
   Type                      Status  LastProbeTime                     LastTransitionTime                Reason  Message
   ----                      ------  -----------------                 ------------------                ------  -------
   FileSystemResizePending   True    Mon, 01 Jan 0001 00:00:00 +0000   Thu, 25 Apr 2019 15:52:49 -0400           Waiting for user to (re-)start a pod to finish file system resize of volume on node.
   ```
   {: screen}

5. Répertoriez tous les pods qui montent la réservation de volume persistant.
      Si votre réservation de volume persistant est montée par un pod, l'extension de volume est traitée automatiquement. Si votre réservation de volume persistant n'est pas montée par un pod, vous devez effectuer cette opération afin que l'extension de volume puisse être traitée.  
   ```
   kubectl get pods --all-namespaces -o=jsonpath='{range .items[*]}{"\n"}{.metadata.name}{":\t"}{range .spec.volumes[*]}{.persistentVolumeClaim.claimName}{" "}{end}{end}' | grep "<pvc_name>"
   ```
   {: pre}

   Les pods montés sont renvoyés au format `<pod_name>: <pvc_name>`.


6. Si votre réservation de volume persistant n'est pas montée par un pod, [créez un pod ou un déploiement et montez la réservation de volume persistant](/docs/containers?topic=containers-block_storage#add_block). Si votre réservation de volume persistant est montée par un pod, passez à l'étape suivante.  

7. Surveillez le statut de l'extension de volume. L'extension de volume est terminée lorsque le message `"message":"Success"` apparaît dans la sortie de votre interface de ligne de commande. 
   ```
   kubectl get pv <pv_name> -o go-template=$'{{index .metadata.annotations "ibm.io/volume-expansion-status"}}\n'
   ```
   {: pre}

   Exemple de sortie :
   ```
   {"size":50,"iops":500,"orderid":38832711,"start":"2019-04-30T17:00:37Z","end":"2019-04-30T17:05:27Z","status":"complete","message":"Success"}
   ```
   {: screen}

8. Vérifiez que la taille les IOPS ont été modifiés dans la section **Labels** de la sortie de l'interface CLI. 
   ```
   kubectl describe pv <pv_name>
   ```
   {: pre}

   Exemple de sortie :
   ```
   ...
   Labels:       CapacityGb=50
                 Datacenter=dal10
                 IOPS=500
   ```
   {: screen}


## Sauvegarde et restauration des données
{: #block_backup_restore}

Le stockage par blocs est mis à disposition au même emplacement que les noeuds worker dans votre cluster. Le stockage est hébergé par IBM sur des serveurs en cluster pour qu'il soit disponible si un serveur tombe en panne. Cependant, le stockage par blocs n'est pas sauvegardé automatiquement et risque d'être inaccessible en cas de défaillance de l'emplacement global. Pour éviter que vos données soient perdues ou endommagées, vous pouvez configurer des sauvegardes régulières que vous pourrez utiliser pour récupérer vos données si nécessaire.
{: shortdesc}

Passez en revue les options de sauvegarde et restauration suivantes pour votre stockage par blocs :

<dl>
  <dt>Configurer la prise d'instantanés régulière</dt>
  <dd><p>Vous pouvez [configurer la prise d'instantanés régulière de votre stockage par blocs](/docs/infrastructure/BlockStorage?topic=BlockStorage-snapshots#snapshots). Un instantané est une image en lecture seule qui capture l'état de l'instance à un moment donné. Pour stocker l'instantané, vous devez demander de l'espace d'image instantanée dans votre stockage par blocs. Les instantanés sont stockés dans l'instance de stockage existante figurant dans la même zone. Vous pouvez restaurer des données à partir d'un instantané si l'utilisateur supprime accidentellement des données importantes du volume. </br></br> <strong>Pour créer un instantané de votre volume :</strong><ol><li>[Connectez-vous à votre compte. Le cas échéant, ciblez le groupe de ressources approprié. Définissez le contexte pour votre cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)</li><li>Connectez-vous à l'interface de ligne de commande `ibmcloud sl`. <pre class="pre"><code>    ibmcloud sl init
    </code></pre></li><li>Répertoriez les volumes persistants (PV) existants dans votre cluster. <pre class="pre"><code>kubectl get pv</code></pre></li><li>Obtenez les détails du volume persistant pour lequel vous voulez créer un espace d'instantané et notez l'ID du volume, la taille et le nombre d'entrées-sorties par seconde (IOPS). <pre class="pre"><code>kubectl describe pv &lt;pv_name&gt;</code></pre> La taille et le nombre d'IOPS sont affichés dans la section <strong>Labels</strong> de la sortie de l'interface CLI. Pour trouver l'ID du volume, consultez l'annotation <code>ibm.io/network-storage-id</code> dans la sortie de l'interface CLI. </li><li>Créez la taille de l'instantané pour le volume existant avec les paramètres que vous avez récupérés à l'étape précédente. <pre class="pre"><code>ibmcloud sl block snapshot-order &lt;volume_ID&gt; --size &lt;size&gt; --tier &lt;iops&gt;</code></pre></li><li>Attendez que la taille de l'instantané soit créée. <pre class="pre"><code>ibmcloud sl block volume-detail &lt;volume_ID&gt;</code></pre>La taille de l'instantané est mise à disposition lorsque la section <strong>Snapshot Size (GB)</strong> de la sortie de l'interface CLI passe de 0 à la taille que vous avez commandée. </li><li>Créez l'instantané de votre volume et notez l'ID de l'instantané qui a été créé pour vous. <pre class="pre"><code>ibmcloud sl block snapshot-create &lt;volume_ID&gt;</code></pre></li><li>Vérifiez que la création de l'instantané a abouti. <pre class="pre"><code>ibmcloud sl block snapshot-list &lt;volume_ID&gt;</code></pre></li></ol></br><strong>Pour restaurer les données d'un instantané sur un volume existant : </strong><pre class="pre"><code>ibmcloud sl block snapshot-restore &lt;volume_ID&gt; &lt;snapshot_ID&gt;</code></pre></p></dd>
  <dt>Répliquer les instantanés dans une autre zone</dt>
 <dd><p>Pour protéger vos données en cas de défaillance d'une zone, vous pouvez [répliquer des instantanés](/docs/infrastructure/BlockStorage?topic=BlockStorage-replication#replication) sur une instance de stockage par blocs configurée dans une autre zone. Les données peuvent être répliquées du stockage principal uniquement vers le stockage de sauvegarde. Vous ne pouvez pas monter une instance de stockage par blocs répliquée dans un cluster. En cas de défaillance de votre stockage principal, vous pouvez manuellement définir votre stockage de sauvegarde répliqué comme stockage principal. Vous pouvez ensuite le monter sur votre cluster. Une fois votre stockage principal restauré, vous pouvez récupérer les données dans le stockage de sauvegarde.</p></dd>
 <dt>Dupliquer le stockage</dt>
 <dd><p>Vous pouvez [dupliquer votre instance de stockage par blocs](/docs/infrastructure/BlockStorage?topic=BlockStorage-duplicatevolume#duplicatevolume) dans la même zone que l'instance de stockage d'origine. Un doublon contient les mêmes données que l'instance de stockage d'origine au moment où vous créez le doublon. Contrairement aux répliques, le doublon s'utilise comme une instance de stockage indépendante de l'original. Pour effectuer la duplication, configurez d'abord des instantanés pour le volume.</p></dd>
  <dt>Sauvegarder les données dans {{site.data.keyword.cos_full}}</dt>
  <dd><p>Vous pouvez utiliser l'[**image ibm-backup-restore**](/docs/services/RegistryImages/ibm-backup-restore?topic=RegistryImages-ibmbackup_restore_starter#ibmbackup_restore_starter) pour constituer un pod de sauvegarde et de restauration dans votre cluster. Ce pod contient un script pour exécuter une sauvegarde unique ou régulière d'une réservation de volume persistant (PVC) dans votre cluster. Les données sont stockées dans votre instance {{site.data.keyword.cos_full}} que vous avez configurée dans une zone.</p><p class="note">Le stockage par blocs est monté avec un mode d'accès RWO. Ce type d'accès autorise uniquement le montage d'un pod à la fois sur le stockage par blocs. Pour sauvegarder vos données, vous devez démonter le pod d'application du stockage, le monter sur votre pod de sauvegarde, sauvegardez les données et monter à nouveau le stockage sur votre pod d'application. </p>
Pour rendre vos données hautement disponibles et protéger votre application en cas de défaillance d'une zone, configurez une deuxième instance {{site.data.keyword.cos_short}} et répliquez les données entre les différentes zones. Si vous devez restaurer des données à partir de votre instance {{site.data.keyword.cos_short}}, utilisez le script de restauration fourni avec l'image.</dd>
<dt>Copier les données depuis et vers des pods et des conteneurs</dt>
<dd><p>Vous pouvez utiliser la [commande![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/reference/kubectl/overview/#cp) `kubectl cp` pour copier des fichiers et des répertoires depuis et vers des pods ou des conteneurs spécifiques dans votre cluster.</p>
<p>Avant de commencer : [connectez-vous à votre compte. Le cas échéant, ciblez le groupe de ressources approprié. Définissez le contexte pour votre cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure) Si vous n'indiquez pas de conteneur avec <code>-c</code>, la commande utilise le premier conteneur disponible dans le pod.</p>
<p>Vous pouvez utiliser la commande de plusieurs manières :</p>
<ul>
<li>Copier les données de votre machine locale vers un pod dans votre cluster : <pre class="pre"><code>kubectl cp <var>&lt;local_filepath&gt;/&lt;filename&gt;</var> <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;</var></code></pre></li>
<li>Copier les données d'un pod de votre cluster vers votre machine locale : <pre class="pre"><code>kubectl cp <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;/&lt;filename&gt;</var> <var>&lt;local_filepath&gt;/&lt;filename&gt;</var></code></pre></li>
<li>Copier les données de votre machine locale vers un conteneur spécifique qui s'exécute dans un pod de votre cluster : <pre class="pre"><code>kubectl cp <var>&lt;local_filepath&gt;/&lt;filename&gt;</var> <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;</var> -c <var>&lt;container&gt;</var></code></pre></li>
</ul></dd>
  </dl>

<br />


## Référence des classes de stockage
{: #block_storageclass_reference}

### Bronze
{: #block_bronze}

<table>
<caption>Classe de stockage par blocs : bronze</caption>
<thead>
<th>Caractéristiques</th>
<th>Paramètre</th>
</thead>
<tbody>
<tr>
<td>Nom</td>
<td><code>ibmc-block-bronze</code></br><code>ibmc-block-retain-bronze</code></td>
</tr>
<tr>
<td>Type</td>
<td>[Stockage Endurance](/docs/infrastructure/BlockStorage?topic=BlockStorage-About#provendurance)</td>
</tr>
<tr>
<td>Système de fichiers</td>
<td>ext4</td>
</tr>
<tr>
<td>IOPS par gigaoctet</td>
<td>2</td>
</tr>
<tr>
<td>Plage de tailles en gigaoctets</td>
<td>20-12000 Gi</td>
</tr>
<tr>
<td>Disque dur</td>
<td>SSD</td>
</tr>
<tr>
<td>Facturation</td>
<td>Le type de facturation par défaut dépend de votre version de plug-in {{site.data.keyword.Bluemix_notm}} Block Storage : <ul><li> Version 1.0.1 et supérieure : à l'heure</li><li>Version 1.0.0 et inférieure : au mois</li></ul></td>
</tr>
<tr>
<td>Tarification</td>
<td>[Informations de tarification ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/cloud/block-storage/pricing)</td>
</tr>
</tbody>
</table>


### Silver
{: #block_silver}

<table>
<caption>Classe de stockage par blocs : silver</caption>
<thead>
<th>Caractéristiques</th>
<th>Paramètre</th>
</thead>
<tbody>
<tr>
<td>Nom</td>
<td><code>ibmc-block-silver</code></br><code>ibmc-block-retain-silver</code></td>
</tr>
<tr>
<td>Type</td>
<td>[Stockage Endurance](/docs/infrastructure/BlockStorage?topic=BlockStorage-About#provendurance)</td>
</tr>
<tr>
<td>Système de fichiers</td>
<td>ext4</td>
</tr>
<tr>
<td>IOPS par gigaoctet</td>
<td>4</td>
</tr>
<tr>
<td>Plage de tailles en gigaoctets</td>
<td>20-12000 Gi</td>
</tr>
<tr>
<td>Disque dur</td>
<td>SSD</td>
</tr>
<tr>
<td>Facturation</td>
<td>Le type de facturation par défaut dépend de votre version de plug-in {{site.data.keyword.Bluemix_notm}} Block Storage : <ul><li> Version 1.0.1 et supérieure : à l'heure</li><li>Version 1.0.0 et inférieure : au mois</li></ul></td>
</tr>
<tr>
<td>Tarification</td>
<td>[Informations de tarification ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/cloud/block-storage/pricing)</td>
</tr>
</tbody>
</table>

### Gold
{: #block_gold}

<table>
<caption>Classe de stockage par blocs : gold</caption>
<thead>
<th>Caractéristiques</th>
<th>Paramètre</th>
</thead>
<tbody>
<tr>
<td>Nom</td>
<td><code>ibmc-block-gold</code></br><code>ibmc-block-retain-gold</code></td>
</tr>
<tr>
<td>Type</td>
<td>[Stockage Endurance](/docs/infrastructure/BlockStorage?topic=BlockStorage-About#provendurance)</td>
</tr>
<tr>
<td>Système de fichiers</td>
<td>ext4</td>
</tr>
<tr>
<td>IOPS par gigaoctet</td>
<td>10</td>
</tr>
<tr>
<td>Plage de tailles en gigaoctets</td>
<td>20-4000 Gi</td>
</tr>
<tr>
<td>Disque dur</td>
<td>SSD</td>
</tr>
<tr>
<td>Facturation</td>
<td>Le type de facturation par défaut dépend de votre version de plug-in {{site.data.keyword.Bluemix_notm}} Block Storage : <ul><li> Version 1.0.1 et supérieure : à l'heure</li><li>Version 1.0.0 et inférieure : au mois</li></ul></td>
</tr>
<tr>
<td>Tarification</td>
<td>[Informations de tarification ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/cloud/block-storage/pricing)</td>
</tr>
</tbody>
</table>

### Personnalisée
{: #block_custom}

<table>
<caption>Classe de stockage de bloc : personnalisée</caption>
<thead>
<th>Caractéristiques</th>
<th>Paramètre</th>
</thead>
<tbody>
<tr>
<td>Nom</td>
<td><code>ibmc-block-custom</code></br><code>ibmc-block-retain-custom</code></td>
</tr>
<tr>
<td>Type</td>
<td>[Performances](/docs/infrastructure/BlockStorage?topic=BlockStorage-About#provperformance)</td>
</tr>
<tr>
<td>Système de fichiers</td>
<td>ext4</td>
</tr>
<tr>
<td>IOPS et taille</td>
<td><strong>Plage de tailles en gigaoctets / plage d'IOPS en multiples de 100</strong><ul><li>20-39 Gi / 100-1000 IOPS</li><li>40-79 Gi / 100-2000 IOPS</li><li>80-99 Gi / 100-4000 IOPS</li><li>100-499 Gi / 100-6000 IOPS</li><li>500-999 Gi / 100-10000 IOPS</li><li>1000-1999 Gi / 100-20000 IOPS</li><li>2000-2999 Gi / 200-40000 IOPS</li><li>3000-3999 Gi / 200-48000 IOPS</li><li>4000-7999 Gi / 300-48000 IOPS</li><li>8000-9999 Gi / 500-48000 IOPS</li><li>10000-12000 Gi / 1000-48000 IOPS</li></ul></td>
</tr>
<tr>
<td>Disque dur</td>
<td>Rapport IOPS/gigaoctet qui détermine le type de disque dur mis à disposition. Pour déterminer ce rapport, divisez la valeur des IOPS par la taille de votre stockage. </br></br>Exemple : </br>Vous avez choisi 500Gi de stockage avec 100 IOPS. Votre rapport est 0,2 (100 IOPS/500Gi). </br></br><strong>Présentation des types de disque dur en fonction du rapport :</strong><ul><li>Inférieur ou égal à 0,3 : SATA</li><li>Supérieur à 0,3 : SSD</li></ul></td>
</tr>
<tr>
<td>Facturation</td>
<td>Le type de facturation par défaut dépend de votre version de plug-in {{site.data.keyword.Bluemix_notm}} Block Storage : <ul><li> Version 1.0.1 et supérieure : à l'heure</li><li>Version 1.0.0 et inférieure : au mois</li></ul></td>
</tr>
<tr>
<td>Tarification</td>
<td>[Informations de tarification ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/cloud/block-storage/pricing)</td>
</tr>
</tbody>
</table>

<br />


## Exemples de classes de stockage personnalisées
{: #block_custom_storageclass}

Vous pouvez créer une classe de stockage personnalisée et l'utiliser dans votre PVC.
{: shortdesc}

{{site.data.keyword.containerlong_notm}} fournit des [classes de stockage prédéfinies](#block_storageclass_reference) pour mettre à disposition du stockage par blocs avec une configuration et un niveau particuliers. Dans certains cas, vous envisagerez de mettre à disposition du stockage avec une autre configuration qui n'est pas couverte dans les classes de stockage prédéfinies. Vous pouvez utiliser les exemples de cette rubrique pour trouver des modèles de classes de stockage personnalisées.

Pour créer votre classe de stockage personnalisée, voir [Personnalisation d'une classe de stockage](/docs/containers?topic=containers-kube_concepts#customized_storageclass). [Utilisez ensuite votre classe de stockage personnalisée dans votre PVC](#add_block).

### Création de stockage tenant compte de la topologie
{: #topology_yaml}

Pour utiliser du stockage par blocs dans un cluster à zones multiples, votre pod doit être planifié dans la même zone que votre instance de stockage par blocs pour que vous puissiez effectuer des opérations de lecture et d'écriture dans le volume. Avant l'introduction de ce type de planification par Kubernetes, la mise à disposition dynamique de votre stockage créait automatiquement l'instance de stockage par blocs dès qu'une PVC était créée. Ensuite, lorsque vous créiez votre pod, le planificateur Kubernetes essayait de déployer le pod dans le même centre de données que votre instance de stockage par blocs.
{: shortdesc}

La création de l'instance de stockage par blocs sans connaître les contraintes liées au pod peut entraîner des résultats indésirables. Par exemple, il peut arriver que votre pod ne puisse pas être planifié sur le même noeud worker que votre stockage car ce noeud ne dispose pas de ressources suffisantes ou qu'il comporte l'annotation taint et n'autorise pas la planification du pod. Avec une planification de volume tenant compte de la topologie, la création de l'instance de stockage par blocs est différée jusqu'à ce que le premier pod utilisant le stockage soit créé.

La planification de volume tenant compte de la topologie est prise en charge uniquement sur les clusters exécutant Kubernetes version 1.12 ou ultérieure. Pour utiliser cette fonction, vérifiez que vous avez installé le plug-in {{site.data.keyword.Bluemix_notm}} Block Storage version 1.2.0 ou ultérieure.
{: note}

Les exemples suivants montrent comment créer des classes de stockage qui retardent la création de l'instance de stockage par blocs jusqu'à ce que le premier pod utilisant ce stockage soit prêt à être planifié. Pour différer la création, vous devez inclure l'option `volumeBindingMode: WaitForFirstConsumer`. Si vous omettez cette option, le paramètre `volumeBindingMode` est automatiquement défini avec `Immediate` et l'instance de stockage est créée lorsque vous créez la PVC.

- **Exemple pour du stockage par blocs de type Endurance :**
  ```
  apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
    name: ibmc-block-bronze-delayed
  parameters:
    billingType: hourly
    classVersion: "2"
    fsType: ext4
    iopsPerGB: "2"
    sizeRange: '[20-12000]Gi'
    type: Endurance
  provisioner: ibm.io/ibmc-block
  reclaimPolicy: Delete
  volumeBindingMode: WaitForFirstConsumer
  ```
  {: codeblock}

- **Exemple pour du stockage par blocs de type Performance :**
  ```
  apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
   name: ibmc-block-performance-storageclass
   labels:
     kubernetes.io/cluster-service: "true"
  provisioner: ibm.io/ibmc-block
  parameters:
   billingType: "hourly"
   classVersion: "2"
   sizeIOPSRange: |-
     "[20-39]Gi:[100-1000]"
     "[40-79]Gi:[100-2000]"
     "[80-99]Gi:[100-4000]"
     "[100-499]Gi:[100-6000]"
     "[500-999]Gi:[100-10000]"
     "[1000-1999]Gi:[100-20000]"
     "[2000-2999]Gi:[200-40000]"
     "[3000-3999]Gi:[200-48000]"
     "[4000-7999]Gi:[300-48000]"
     "[8000-9999]Gi:[500-48000]"
     "[10000-12000]Gi:[1000-48000]"
   type: "Performance"
  reclaimPolicy: Delete
  volumeBindingMode: WaitForFirstConsumer
  ```
  {: codeblock}

### Spécification de la zone et de la région
{: #block_multizone_yaml}

Si vous souhaitez créer votre stockage par blocs dans une zone précise, vous pouvez spécifier la zone et la région dans une classe de stockage personnalisée.
{: shortdesc}

Utilisez la classe de stockage personnalisée avec le plug-in {{site.data.keyword.Bluemix_notm}} Block Storage version 1.0.0 ou pour [mettre à disposition du stockage par blocs de manière statique](#existing_block) dans une zone spécifique. Dans tous les autres cas, [indiquez la zone directement dans votre PVC](#add_block).
{: note}

Le fichier `.yaml` suivant personnalise une classe de stockage basée sur la classe de stockage `ibm-block-silver` sans retain : le type `type` est `"Endurance"`, le nombre d'IOPS par gigaoctet (`iopsPerGB`) est `4`, la plage de tailles (`sizeRange`) est `"[20-12000]Gi"` et la règle de récupération (`reclaimPolicy`) est définie par `"Delete"`. La zone indiquée est `dal12`. Pour utiliser une autre classe de stockage comme référence, voir la rubrique [Référence des classes de stockage](#block_storageclass_reference).

Créez la classe de stockage dans la même région et dans la même zone que votre cluster et vos noeuds worker. Pour obtenir la région dans laquelle se trouve votre cluster, exécutez la commande `ibmcloud ks cluster-get --cluster <cluster_name_or_ID>` et recherchez le préfixe de la région dans la zone **Master URL**, par exemple `eu-de` dans l'URL `https://c2.eu-de.containers.cloud.ibm.com:11111`. Pour obtenir la zone dans laquelle se trouve votre noeud worker, exécutez la commande `ibmcloud ks workers --cluster <cluster_name_or_ID>`.

- **Exemple pour du stockage par blocs de type Endurance :**
  ```
  apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
    name: ibmc-block-silver-mycustom-storageclass
    labels:
      kubernetes.io/cluster-service: "true"
  provisioner: ibm.io/ibmc-block
  parameters:
    zone: "dal12"
    region: "us-south"
    type: "Endurance"
    iopsPerGB: "4"
    sizeRange: "[20-12000]Gi"
  reclaimPolicy: "Delete"
  ```
  {: codeblock}

- **Exemple pour du stockage par blocs de type Performance :**
  ```
  apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
    name: ibmc-block-performance-storageclass
    labels:
      kubernetes.io/cluster-service: "true"
  provisioner: ibm.io/ibmc-block
  parameters:
    zone: "dal12"
    region: "us-south"
    type: "Performance"
    sizeIOPSRange: |-
      "[20-39]Gi:[100-1000]"
      "[40-79]Gi:[100-2000]"
      "[80-99]Gi:[100-4000]"
      "[100-499]Gi:[100-6000]"
      "[500-999]Gi:[100-10000]"
      "[1000-1999]Gi:[100-20000]"
      "[2000-2999]Gi:[200-40000]"
      "[3000-3999]Gi:[200-48000]"
      "[4000-7999]Gi:[300-48000]"
      "[8000-9999]Gi:[500-48000]"
      "[10000-12000]Gi:[1000-48000]"
  reclaimPolicy: "Delete"
  ```
  {: codeblock}

### Montage de stockage par blocs avec un système de fichiers `XFS`
{: #xfs}

Les exemples suivants illustrent la création d'une classe de stockage qui met à disposition du stockage par blocs avec un système de fichiers `XFS`.
{: shortdesc}

- **Exemple pour du stockage par blocs de type Endurance :**
  ```
  apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
    name: ibmc-block-custom-xfs
    labels:
      addonmanager.kubernetes.io/mode: Reconcile
  provisioner: ibm.io/ibmc-block
  parameters:
    type: "Endurance"
    iopsPerGB: "4"
    sizeRange: "[20-12000]Gi"
    fsType: "xfs"
  reclaimPolicy: "Delete"
  ```

- **Exemple pour du stockage par blocs de type Performance :**
  ```
  apiVersion: storage.k8s.io/v1
   kind: StorageClass
   metadata:
     name: ibmc-block-custom-xfs
     labels:
       addonmanager.kubernetes.io/mode: Reconcile
   provisioner: ibm.io/ibmc-block
   parameters:
     type: "Performance"
     sizeIOPSRange: |-
       [20-39]Gi:[100-1000]
       [40-79]Gi:[100-2000]
       [80-99]Gi:[100-4000]
       [100-499]Gi:[100-6000]
       [500-999]Gi:[100-10000]
       [1000-1999]Gi:[100-20000]
       [2000-2999]Gi:[200-40000]
       [3000-3999]Gi:[200-48000]
       [4000-7999]Gi:[300-48000]
       [8000-9999]Gi:[500-48000]
       [10000-12000]Gi:[1000-48000]
    fsType: "xfs"
     reclaimPolicy: "Delete"
     classVersion: "2"
  ```
  {: codeblock}

<br />

