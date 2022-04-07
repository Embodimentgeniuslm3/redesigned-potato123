---

copyright:
  years: 2014, 2019
lastupdated: "2019-05-31"

keywords: kubernetes, iks, clusters, worker nodes, worker pools, delete

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
{:gif: data-image-type='gif'}


# Suppression de clusters
{: #remove}

Les clusters gratuits et standard créés avec un compte facturable doivent être supprimés manuellement lorsqu'ils ne sont plus nécessaires afin qu'ils ne consomment plus de ressources.
{:shortdesc}

<p class="important">
Aucune sauvegarde de votre cluster ou de vos données n'est effectuée dans votre stockage persistant. Lorsque vous supprimez un cluster, vous pouvez également opter pour la suppression de votre stockage persistant. Le stockage persistant que vous avez mis à disposition en utilisant une classe de stockage `delete` est supprimé définitivement dans l'infrastructure IBM Cloud (SoftLayer) si vous choisissez de supprimer votre stockage persistant. Si vous avez mis à disposition votre stockage persistant en utilisant une classe de stockage `retain` et que vous choisissez de supprimer votre stockage, le cluster, le volume persistant et la réservation de volume persistant (PVC) sont supprimés, mais l'instance de stockage persistant dans votre compte d'infrastructure IBM Cloud (SoftLayer) est conservée. </br>
</br>Lorsque vous supprimez un cluster, vous supprimez également les sous-réseaux éventuels qui sont automatiquement fournis lorsque vous avez créé le cluster et que vous avez créés en exécutant la commande `ibmcloud ks cluster-subnet-create`. Cependant, si vous avez ajouté manuellement des sous-réseaux existants à votre cluster avec la commande `ibmcloud ks cluster-subnet-add`, ces sous-réseaux ne sont pas retirés de votre compte d'infrastructure IBM Cloud (SoftLayer) et vous pouvez les réutiliser dans d'autres clusters.</p>

Avant de commencer :
* Notez l'ID de votre cluster. Vous en aurez besoin pour rechercher et retirer les ressources d'infrastructure IBM Cloud (SoftLayer) associées qui ne sont pas automatiquement supprimées avec votre cluster.
* Si vous souhaitez supprimer les données dans votre stockage persistant, [familiarisez-vous avec les options de suppression](/docs/containers?topic=containers-cleanup#cleanup).
* Vérifiez que vous disposez du [rôle de plateforme {{site.data.keyword.Bluemix_notm}} IAM **Administrateur**](/docs/containers?topic=containers-users#platform).

Pour supprimer un cluster :

1. Facultatif : A partir de l'interface de ligne de commande, sauvegardez une copie de toutes les données dans votre cluster sur un fichier YAML local. 
  ```
  kubectl get all --all-namespaces -o yaml
  ```
  {: pre}

2. Retirez le cluster.
  - Depuis la console {{site.data.keyword.Bluemix_notm}}
    1.  Sélectionnez votre cluster et cliquez sur **Supprimer** dans le menu **Plus d'actions...**.

  - Depuis l'interface CLI d'{{site.data.keyword.Bluemix_notm}}
    1.  Répertoriez les clusters disponibles.

        ```
        ibmcloud ks clusters
        ```
        {: pre}

    2.  Supprimez le cluster.

        ```
        ibmcloud ks cluster-rm --cluster <cluster_name_or_ID>
        ```
        {: pre}

    3.  Suivez les invites et indiquez si vous souhaitez supprimer des ressources de cluster, notamment des conteneurs, des pods, des services liés, du stockage persistant et des valeurs confidentielles.
      - **Stockage persistant** : un stockage persistant procure une haute disponibilité à vos données. Si vous avez créé une réservation de volume persistant via un [partage de fichiers existant](/docs/containers?topic=containers-file_storage#existing_file), vous ne pouvez pas supprimer ce dernier lorsque vous supprimez le cluster. Vous devez ultérieurement supprimer ce partage de fichiers manuellement de votre portefeuille d'infrastructure IBM Cloud (SoftLayer).

          En raison du cycle de facturation mensuel, une réservation de volume persistant ne peut pas être supprimée le dernier jour du mois. Si vous supprimez la réservation de volume persistant le dernier jour du mois, la suppression reste en attente jusqu'au début du mois suivant.
          {: note}

Etapes suivantes :
- Lorsqu'il n'est plus répertorié dans la liste des clusters disponibles lorsque vous exécutez la commande `ibmcloud ks clusters`, vous pouvez réutiliser le nom d'un cluster supprimé.
- Si vous conservez les sous-réseaux, vous pouvez [les réutiliser dans un nouveau cluster](/docs/containers?topic=containers-subnets#subnets_custom) ou les supprimer manuellement de votre portefeuille d'infrastructure IBM Cloud (SoftLayer).
- Si vous conservez du stockage persistant, vous pouvez [supprimer votre stockage](/docs/containers?topic=containers-cleanup#cleanup) par la suite dans le tableau de bord de l'infrastructure IBM Cloud (SoftLayer) dans la console {{site.data.keyword.Bluemix_notm}}.
