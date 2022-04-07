---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-11"

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



# Suppression de stockage persistant dans un cluster
{: #cleanup}

Lorsque vous configurez du stockage persistant dans votre cluster, vous disposez de trois composants principaux : la réservation de volume persistant (PVC) Kubernetes qui sollicite le stockage, le volume persistant (PV) Kubernetes monté sur un pod et décrit dans la PVC, et l'instance de stockage de l'infrastructure IBM Cloud (SoftLayer), comme par exemple du stockage de fichiers NFS ou du stockage par blocs. Selon comment vous avez créé votre stockage, il vous faudra peut-être les supprimer tous les trois séparément.
{:shortdesc}

## Nettoyage de stockage persistant
{: #storage_remove}

Description de vos options de suppression :

**J'ai supprimé mon cluster. Dois-je supprimer autre chose pour supprimer du stockage persistant ?**</br>
Ça dépend. Lorsque vous supprimez un cluster, la réservation de volume persistant (PVC) et le volume persistant (PV) sont supprimés. Cependant, c'est à vous de choisir si vous supprimez l'instance de stockage associée dans l'infrastructure IBM Cloud (SoftLayer). Si vous choisissez de ne pas la supprimer, elle existe toujours. Par ailleurs, si vous avez supprimé votre cluster alors qu'il n'était pas à l'état sain, le stockage peut encore exister, même si vous choisissez de le supprimer. Suivez les instructions, notamment l'étape permettant de [supprimer votre instance de stockage](#sl_delete_storage) dans l'infrastructure IBM Cloud (SoftLayer).

**Puis-je supprimer la PVC pour supprimer l'intégralité de mon stockage ?**</br>
C'est possible. Si vous [créez le stockage persistant de manière dynamique](/docs/containers?topic=containers-kube_concepts#dynamic_provisioning) et que vous sélectionnez une classe de stockage dont le nom ne contient pas `retain`, lorsque vous supprimez la PVC, le volume persistant et l'instance de stockage de l'infrastructure IBM Cloud (SoftLayer) sont également supprimés.

Dans tous les autres cas, suivez les instructions permettant de vérifier le statut de la réservation de volume persistant (PVC), du volume persistant (PV) et de l'unité de stockage physique et supprimez-les séparément si nécessaire.

**Suis-je facturé pour le stockage après que je l'ai supprimé ?**</br>
Tout dépend de ce que vous supprimez et du type de facturation. Si vous supprimez la réservation de volume persistant et le volume persistant, mais pas l'instance dans votre compte d'infrastructure IBM Cloud (SoftLayer), cette instance continue à exister et vous êtes facturé pour son utilisation. Vous devez tout supprimer pour ne plus être facturé. De plus, lorsque vous spécifiez le type de facturation (`billingType`) dans la PVC, vous pouvez choisir entre une facturation à l'heure (`hourly`) ou au mois (`monthly`). Si vous avez choisi `monthly`, votre instance est facturée tous les mois. Lorsque vous supprimez l'instance, vous êtes facturé jusqu'à la fin du mois en cours.


<p class="important">Lorsque vous nettoyez du stockage persistant, vous supprimez toutes les données qui y sont stockées. Si vous avez besoin d'une copie des données, effectuez une sauvegarde de [stockage de fichiers](/docs/containers?topic=containers-file_storage#file_backup_restore) ou de [stockage par blocs](/docs/containers?topic=containers-block_storage#block_backup_restore).</p>

Avant de commencer : [connectez-vous à votre compte. Le cas échéant, ciblez le groupe de ressources approprié. Définissez le contexte pour votre cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

Pour nettoyer des données persistantes :

1.  Répertoriez les réservations de volume persistant (PVC) figurant dans votre cluster et notez le nom de la PVC (**`NAME`**), la classe de stockage (**`STORAGECLASS`**) et le nom du volume persistant lié à la PVC indiqué sous **`VOLUME`**.
    ```
    kubectl get pvc
    ```
    {: pre}

    Exemple de sortie :
    ```
    NAME                  STATUS    VOLUME                                     CAPACITY   ACCESSMODES   STORAGECLASS            AGE
    claim1-block-bronze   Bound     pvc-06886b77-102b-11e8-968a-f6612bb731fb   20Gi       RWO           ibmc-block-bronze       78d
    claim-file-bronze     Bound     pvc-457a2b96-fafc-11e7-8ff9-b6c8f770356c   4Gi        RWX           ibmc-file-bronze-retain 105d
    claim-file-silve      Bound     pvc-1efef0ba-0c48-11e8-968a-f6612bb731fb   24Gi       RWX           ibmc-file-silver        83d
    ```
    {: screen}

2. Passez en revue la **`règle de récupération`** et le **`type de facturation`** correspondant à la classe de stockage.
   ```
   kubectl describe storageclass <storageclass_name>
   ```
   {: pre}

   Si la politique de récupération indique `Delete`, votre volume persistant et le stockage physique sont supprimés en même temps que la PVC. Si la politique de récupération indique `Retain` ou si vous avez mis à disposition votre stockage sans classe de stockage, votre volume persistant et votre stockage physique ne sont pas supprimés en même temps que la PVC. Vous devez supprimer la PVC, le volume persistant et le stockage physique séparément.

   Si vous êtes facturé tous les mois pour le stockage, vous êtes redevable pour le mois complet, même si vous supprimez le stockage avant la fin du cycle de facturation.
   {: important}

3. Supprimez les pods qui montent la réservation de volume persistant. 
   1. Répertoriez les pods qui montent la réservation de volume persistant.
      ```
      kubectl get pods --all-namespaces -o=jsonpath='{range .items[*]}{"\n"}{.metadata.name}{":\t"}{range .spec.volumes[*]}{.persistentVolumeClaim.claimName}{" "}{end}{end}' | grep "<pvc_name>"
      ```
      {: pre}

      Exemple de sortie :
      ```
      blockdepl-12345-prz7b:	claim1-block-bronze  
      ```
      {: screen}

      Si aucun pod n'est renvoyé dans la sortie de l'interface CLI, aucun pod n'utilise la PVC.

   2. Supprimez le pod utilisant la PVC. Si le pod fait partie d'un déploiement, supprimez le déploiement.
      ```
      kubectl delete pod <pod_name>
      ```
      {: pre}

   3. Vérifiez que le pod est supprimé.
      ```
      kubectl get pods
      ```
      {: pre}

4. Supprimez la PVC.
   ```
   kubectl delete pvc <pvc_name>
   ```
   {: pre}

5. Examinez le statut de votre volume persistant. Utilisez le nom du volume persistant que vous avez récupéré précédemment sous **`VOLUME`**.
   ```
   kubectl get pv <pv_name>
   ```
   {: pre}

   Lorsque vous supprimez la PVC, le volume persistant lié à cette PVC est libéré. Selon le mode de mise à disposition de votre stockage, votre volume persistant va passer de l'état `Deleting` si sa suppression est automatique ou à l'état `Released` si vous devez supprimer manuellement le volume persistant. **Remarque** : pour les volumes persistants supprimés automatiquement, l'état peut brièvement indiquer `Released` avant sa suppression définitive. Exécutez à nouveau la commande au bout de quelques minutes pour voir si le volume persistant est supprimé.

6. Si votre volume persistant n'est pas supprimé, supprimez-le manuellement.
   ```
   kubectl delete pv <pv_name>
   ```
   {: pre}

7. Vérifiez que le volume persistant est supprimé.
   ```
   kubectl get pv
   ```
   {: pre}

8. {: #sl_delete_storage}Répertoriez l'instance de stockage physique vers laquelle pointait votre volume persistant et notez l'**`id`** de cette instance.

   **Stockage de fichiers :**
   ```
   ibmcloud sl file volume-list --columns id  --columns notes | grep <pv_name>
   ```
   {: pre}
   **Stockage par blocs :**
   ```
   ibmcloud sl block volume-list --columns id --columns notes | grep <pv_name>
   ```
   {: pre}

   Si vous avez supprimé votre cluster et que vous n'arrivez pas à retrouver le nom du volume persistant, remplacez `grep <pv_name>` par `grep <cluster_id>` pour afficher toutes les unités de stockage associées à votre cluster.
   {: tip}

   Exemple de sortie :
   ```
   id         notes   
   12345678   {"plugin":"ibm-file-plugin-5b55b7b77b-55bb7","region":"us-south","cluster":"aa1a11a1a11b2b2bb22b22222c3c3333","type":"Endurance","ns":"default","pvc":"mypvc","pv":"pvc-d979977d-d79d-77d9-9d7d-d7d97ddd99d7","storageclass":"ibmc-file-gold"}
   ```
   {: screen}

   Description des informations indiquées sous **notes** :
   *  **`"plugin":"ibm-file-plugin-5b55b7b77b-55bb7"`** : plug-in de stockage utilisé par le cluster.
   *  **`"region":"us-south"`** : région dans laquelle se trouve votre cluster.
   *  **`"cluster":"aa1a11a1a11b2b2bb22b22222c3c3333"`** : ID du cluster associé à l'instance de stockage.
   *  **`"type":"Endurance"`** : type de stockage de fichiers ou de stockage par blocs (`Endurance` ou `Performance`).
   *  **`"ns":"default"`** : espace de nom dans lequel est déployée l'instance de stockage.
   *  **`"pvc":"mypvc"`** : nom de la réservation de volume persistant (PVC) associée à l'instance de stockage.
   *  **`"pv":"pvc-d979977d-d79d-77d9-9d7d-d7d97ddd99d7"`** : volume persistant (PV) associé à l'instance de stockage.
   *  **`"storageclass":"ibmc-file-gold"`** : type de classe de stockage : bronze, silver, gold ou custom.

9. Supprimez l'instance de stockage physique.

   **Stockage de fichiers :**
   ```
   ibmcloud sl file volume-cancel <filestorage_id>
   ```
   {: pre}

   **Stockage par blocs :**
   ```
   ibmcloud sl block volume-cancel <blockstorage_id>
   ```
   {: pre}

9. Vérifiez que l'instance de stockage physique est supprimée. L'exécution du processus de suppression peut prendre quelques jours.

   **Stockage de fichiers :**
   ```
   ibmcloud sl file volume-list
   ```
   {: pre}
   **Stockage par blocs :**
   ```
   ibmcloud sl block volume-list
   ```
   {: pre}
