---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-06"

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



# Ajout de noeuds worker et de zones dans les clusters
{: #add_workers}

Afin d'augmenter la disponibilité de vos applications, vous pouvez ajouter des noeuds worker à une ou plusieurs zones déjà présentes dans votre cluster. Pour protéger vos applications en cas de défaillance d'une zone, vous pouvez ajouter des zones dans votre cluster.
{:shortdesc}

Lorsque vous créez un cluster, les noeuds worker sont mis à disposition dans un pool de noeuds worker. Après la création du cluster, vous pouvez ajouter d'autres noeuds worker à un pool en le redimensionnant ou en ajoutant d'autres pools de noeuds worker. Par défaut, le pool de noeuds worker est présent dans une zone. Les clusters disposant d'un pool de noeuds worker dans une seule zone sont appelés des clusters à zone unique. Lorsque vous ajoutez d'autres zones dans le cluster, le pool de noeuds worker est présent dans les différentes zones. Les clusters ayant un pool de noeuds worker réparti sur plusieurs zones sont appelés des clusters à zones multiples.

Si vous disposez d'un cluster à zones multiples, conservez les ressources des noeuds worker associés équilibrées. Assurez-vous que tous les pools de noeuds worker sont répartis sur les mêmes zones et ajoutez ou supprimez des noeuds worker en redimensionnant les pools au lieu d'ajouter des noeuds individuels.
{: tip}

Avant de commencer, assurez-vous de disposer du [rôle de plateforme {{site.data.keyword.Bluemix_notm}} IAM **Opérateur** ou **Administrateur**](/docs/containers?topic=containers-users#platform). Ensuite, choisissez l'une des sections suivantes :
  * [Ajouter des noeuds worker en redimensionnant un pool de noeuds worker existant dans votre cluster](#resize_pool)
  * [Ajouter des noeuds worker en ajoutant un pool de noeuds worker dans votre cluster](#add_pool)
  * [Ajouter une zone dans votre cluster et répliquer les noeuds worker de vos pools de noeuds worker dans plusieurs zones](#add_zone)
  * [Déprécié : Ajouter un noeud worker autonome dans un cluster](#standalone)

Après avoir configuré votre pool de noeuds worker, vous pouvez [configurer le programme de mise à l'échelle automatique du cluster (cluster autoscaler)](/docs/containers?topic=containers-ca#ca) pour l'ajout et le retrait automatique des noeuds worker dans vos pools de noeuds worker en fonction des demandes de ressources de vos charges de travail.
{:tip}

## Ajout de noeuds worker en redimensionnant un pool de noeuds worker existant
{: #resize_pool}

Vous pouvez ajouter ou réduire le nombre de noeuds worker présents dans votre cluster en redimensionnant un pool de noeuds worker existant, que ce pool figure dans une zone ou soit réparti sur plusieurs zones.
{: shortdesc}

Prenons l'exemple d'un cluster avec un pool de noeuds worker comportant trois noeuds worker par zone.
* S'il s'agit d'un cluster à zone unique présent dans la zone `dal10`, le pool de noeuds worker a trois noeuds worker dans `dal10`. Le cluster dispose d'un total de trois noeuds worker.
* S'il s'agit d'un cluster à zones multiples, présent dans les zones `dal10` et `dal12`, le pool de noeuds worker a trois noeuds dans `dal10` et trois autres dans `dal12`. Le cluster dispose d'un total de six noeuds worker.

Pour les pools de noeuds worker bare metal, n'oubliez pas que la facture est mensuelle. Si vous en augmentez ou en réduisez la taille, il y aura des répercussions sur les coûts mensuels.
{: tip}

Pour redimensionner le pool de noeuds worker, modifiez le nombre de noeuds worker déployés par le pool de noeuds worker dans chaque zone :

1. Récupérez le nom du pool de noeuds worker que vous souhaitez redimensionner.
    ```
    ibmcloud ks worker-pools --cluster <cluster_name_or_ID>
    ```
    {: pre}

2. Redimensionnez le pool de noeuds worker en indiquant le nombre de noeuds worker que vous voulez déployer dans chaque zone. La valeur minimale est 1.
    ```
    ibmcloud ks worker-pool-resize --cluster <cluster_name_or_ID> --worker-pool <pool_name>  --size-per-zone <number_of_workers_per_zone>
    ```
    {: pre}

3. Vérifiez que le pool de noeuds worker est redimensionné.
    ```
    ibmcloud ks workers --cluster <cluster_name_or_ID> --worker-pool <pool_name>
    ```
    {: pre}

    Exemple de sortie pour un pool de noeuds worker présents dans deux zones, `dal10` et `dal12` qui est redimensionné à deux noeuds worker par zone :
    ```
    ID                                                 Public IP        Private IP      Machine Type      State    Status  Zone    Version
    kube-dal10-crb20b637238ea471f8d4a8b881aae4962-w7   169.xx.xxx.xxx   10.xxx.xx.xxx   b3c.4x16          normal   Ready   dal10   1.8.6_1504
    kube-dal10-crb20b637238ea471f8d4a8b881aae4962-w8   169.xx.xxx.xxx   10.xxx.xx.xxx   b3c.4x16          normal   Ready   dal10   1.8.6_1504
    kube-dal12-crb20b637238ea471f8d4a8b881aae4962-w9   169.xx.xxx.xxx   10.xxx.xx.xxx   b3c.4x16          normal   Ready   dal12   1.8.6_1504
    kube-dal12-crb20b637238ea471f8d4a8b881aae4962-w10  169.xx.xxx.xxx   10.xxx.xx.xxx   b3c.4x16          normal   Ready   dal12   1.8.6_1504
    ```
    {: screen}

## Ajout de noeuds worker en créant un nouveau pool de noeuds worker
{: #add_pool}

Vous pouvez ajouter des noeuds worker dans votre cluster en créant un nouveau pool de noeuds worker.
{:shortdesc}

1. Extrayez les zones de noeud worker (**Worker Zones**) de votre cluster et choisissez la zone dans laquelle vous souhaitez déployer les noeuds worker dans votre pool de noeuds worker. Si vous avez un cluster à zone unique, vous devez utiliser la zone que vous voyez dans la zone **Worker Zones**. Pour les clusters à zones multiples, vous pouvez choisir l'une des **zones de noeud worker** de votre cluster, ou ajouter l'une des [agglomérations à zones multiples](/docs/containers?topic=containers-regions-and-zones#zones) de la région dans laquelle se trouve votre cluster. Vous pouvez obtenir la liste des zones disponibles en exécutant la commande `ibmcloud ks zones`.
   ```
   ibmcloud ks cluster-get --cluster <cluster_name_or_ID>
   ```
   {: pre}

   Exemple de sortie :
   ```
   ...
   Worker Zones: dal10, dal12, dal13
   ```
   {: screen}

2. Pour chaque zone, répertoriez les VLAN publics et privés disponibles. Notez le VLAN privé et le VLAN public que vous souhaitez utiliser. Si vous ne disposez pas de VLAN privé ou public, ils sont automatiquement créés pour vous lorsque vous ajoutez une zone dans votre pool de noeuds worker.
   ```
   ibmcloud ks vlans --zone <zone>
   ```
   {: pre}

3.  Pour chaque zone, consultez les [types de machine disponibles pour les noeuds worker](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes).

    ```
    ibmcloud ks machine-types <zone>
    ```
    {: pre}

4. Créez un pool de noeuds worker. Incluez l'option `--labels` pour marquer automatiquement les noeuds worker du pool avec le libellé `key=value`. Si vous avez mis à disposition un pool de noeuds worker bare metal, indiquez `--hardware dedicated`.
   ```
   ibmcloud ks worker-pool-create --name <pool_name> --cluster <cluster_name_or_ID> --machine-type <machine_type> --size-per-zone <number_of_workers_per_zone> --hardware <dedicated_or_shared> --labels <key=value>
   ```
   {: pre}

5. Vérifiez que le pool de noeuds worker est créé.
   ```
   ibmcloud ks worker-pools --cluster <cluster_name_or_ID>
   ```
   {: pre}

6. Par défaut, l'ajout d'un pool de noeuds worker crée un pool sans aucune zone. Pour déployer les noeuds worker dans une zone, vous devez ajouter les zones que vous avez récupérées auparavant dans le pool de noeuds worker. Si vous souhaitez répartir vos noeuds worker sur plusieurs zones, répétez cette commande pour chaque zone.
   ```
   ibmcloud ks zone-add --zone <zone> --cluster <cluster_name_or_ID> --worker-pools <pool_name> --private-vlan <private_VLAN_ID> --public-vlan <public_VLAN_ID>
   ```
   {: pre}

7. Vérifiez que les noeuds worker sont présents dans la zone que vous avez ajoutée. Vos noeuds worker sont prêts lorsque leur statut passe de **provision_pending** à **normal**.
   ```
   ibmcloud ks workers --cluster <cluster_name_or_ID> --worker-pool <pool_name>
   ```
   {: pre}

   Exemple de sortie :
   ```
   ID                                                 Public IP        Private IP      Machine Type      State    Status  Zone    Version
   kube-dal10-crb20b637238ea471f8d4a8b881aae4962-w7   169.xx.xxx.xxx   10.xxx.xx.xxx   b3c.4x16          provision_pending   Ready   dal10   1.8.6_1504
   kube-dal10-crb20b637238ea471f8d4a8b881aae4962-w8   169.xx.xxx.xxx   10.xxx.xx.xxx   b3c.4x16          provision_pending   Ready   dal10   1.8.6_1504
   ```
   {: screen}

## Ajout de noeuds worker en ajoutant une zone dans un pool de noeuds worker
{: #add_zone}

Vous pouvez étendre votre cluster sur plusieurs zones au sein d'une région en ajoutant une zone dans votre pool de noeuds worker.
{:shortdesc}

Lorsque vous ajoutez une zone dans un pool de noeuds worker, les noeuds worker définis dans votre pool de noeuds worker sont mis à disposition dans la nouvelle zone et pris en compte pour la planification des charges de travail à venir. {{site.data.keyword.containerlong_notm}} ajoute automatiquement le libellé `failure-domain.beta.kubernetes.io/region` pour la région et le libellé `failure-domain.beta.kubernetes.io/zone` pour la zone dans chaque noeud worker. Le planificateur de Kubernetes utilise ces libellés pour répartir les pods sur les zones situées dans la même région.

Si vous disposez de plusieurs pools de noeuds worker dans votre cluster, ajoutez la zone à tous ces pools de sorte que les noeuds worker soient répartis uniformément dans votre cluster.

Avant de commencer :
*  Pour ajouter une zone à votre pool de noeuds worker, ce pool doit se trouver dans une [zone compatible avec plusieurs zones](/docs/containers?topic=containers-regions-and-zones#zones). Si ce n'est pas le cas, envisagez la [création d'un nouveau pool de noeuds worker](#add_pool).
*  Si vous disposez de plusieurs VLAN pour un cluster, de plusieurs sous-réseaux sur le même VLAN ou d'un cluster à zones multiples, vous devez activer une fonction [VRF (Virtual Router Function)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) pour votre compte d'infrastructure IBM Cloud (SoftLayer) pour que vos noeuds worker puissent communiquer entre eux sur le réseau privé. Pour activer la fonction VRF, [contactez le représentant de votre compte d'infrastructure IBM Cloud (SoftLayer)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#how-you-can-initiate-the-conversion). Si vous ne parvenez pas à activer la fonction VRF ou si vous ne souhaitez pas le faire, activez la fonction [Spanning VLAN](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning). Pour effectuer cette action, vous devez disposer du [droit d'infrastructure](/docs/containers?topic=containers-users#infra_access) **Réseau > Gérer le spanning VLAN pour réseau**, ou vous pouvez demander au propriétaire du compte de l'activer. Pour vérifier si le spanning VLAN est déjà activé, utilisez la [commande](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get) `ibmcloud ks vlan-spanning-get --region <region>`. 

Pour ajouter une zone avec des noeuds worker dans votre pool de noeuds worker :

1. Répertoriez les zones disponibles et sélectionnez la zone que vous souhaitez ajouter à votre pool de noeuds worker. La zone que vous choisissez doit être une zone compatible avec plusieurs zones.
   ```
   ibmcloud ks zones
   ```
   {: pre}

2. Répertoriez les VLAN disponibles dans cette zone. Si vous ne disposez pas de VLAN privé ou public, ils sont automatiquement créés pour vous lorsque vous ajoutez une zone dans votre pool de noeuds worker.
   ```
   ibmcloud ks vlans --zone <zone>
   ```
   {: pre}

3. Répertoriez les pools de noeuds worker dans votre cluster et notez leurs noms.
   ```
   ibmcloud ks worker-pools --cluster <cluster_name_or_ID>
   ```
   {: pre}

4. Ajoutez la zone dans votre pool de noeuds worker. Si vous disposez de plusieurs pools de noeuds worker, ajoutez la zone à tous vos pools, pour que votre cluster soit équilibré dans toutes les zones. Remplacez `<pool1_id_or_name,pool2_id_or_name,...>` par les noms de tous vos pools worker dans une liste séparée par des virgules.

    Un VLAN privé et un VLAN public doivent exister avant d'ajouter une zone à plusieurs pools de noeuds worker. Si vous ne disposez pas de VLAN privé et public dans cette zone, ajoutez d'abord la zone à un pool de noeuds worker pour que ces VLAN soient créés pour vous. Ensuite, vous pouvez ajouter cette zone à d'autres pools de noeuds worker en spécifiant le VLAN privé et le VLAN public créés pour vous.
    {: note}

   Si vous souhaitez utiliser des VLAN différents pour des pools de noeuds worker différents, répétez cette commande pour chaque VLAN et les pools de noeuds worker correspondants associés. Les nouveaux noeuds worker sont ajoutés aux VLAN que vous spécifiez, mais les VLAN pour les noeuds worker existants restent inchangés.
   {: tip}
   ```
   ibmcloud ks zone-add --zone <zone> --cluster <cluster_name_or_ID> --worker-pools <pool1_name,pool2_name,...> --private-vlan <private_VLAN_ID> --public-vlan <public_VLAN_ID>
   ```
   {: pre}

5. Vérifiez que la zone est ajoutée dans votre cluster. Recherchez la zone ajoutée dans la zone **Worker zones** dans la sortie. Notez que le nombre total de noeuds worker dans la zone **Workers** a augmenté avec l'ajout de nouveaux noeuds worker dans la zone ajoutée.
  ```
  ibmcloud ks cluster-get --cluster <cluster_name_or_ID>
  ```
  {: pre}

  Exemple de sortie :
  ```
  Name:                           mycluster
  ID:                             df253b6025d64944ab99ed63bb4567b6
  State:                          normal
  Created:                        2018-09-28T15:43:15+0000
  Location:                       dal10
  Master URL:                     https://c3.<region>.containers.cloud.ibm.com:30426
  Public Service Endpoint URL:    https://c3.<region>.containers.cloud.ibm.com:30426
  Private Service Endpoint URL:   https://c3-private.<region>.containers.cloud.ibm.com:31140
  Master Location:                Dallas
  Master Status:                  Ready (21 hours ago)
  Ingress Subdomain:              mycluster.us-south.containers.appdomain.cloud
  Ingress Secret:                 mycluster
  Workers:                        6
  Worker Zones:                   dal10, dal12
  Version:                        1.11.3_1524
  Owner:                          owner@email.com
  Resource Group ID:              a8a12accd63b437bbd6d58fb6a462ca7
  Resource Group Name:            Default
  ```
  {: screen}

## Déprécié : Ajout de noeuds worker autonomes
{: #standalone}

Si vous disposez d'un cluster qui a été créé avant l'introduction des pools de noeuds worker, vous pouvez utiliser les commandes dépréciées pour ajouter des noeuds worker autonomes.
{: deprecated}

Si vous disposez d'un cluster qui a été créé après l'introduction des pools de noeuds worker, vous ne pouvez pas ajouter des noeuds worker autonomes. A la place, vous pouvez [créer un pool de noeuds worker](#add_pool), [redimensionner un pool de noeuds worker existant](#resize_pool) ou [ajouter une zone dans un pool de noeuds worker](#add_zone) pour ajouter des noeuds worker dans votre cluster.
{: note}

1. Répertoriez les zones disponibles et sélectionnez la zone dans laquelle vous souhaitez ajouter des noeuds worker.
   ```
   ibmcloud ks zones
   ```
   {: pre}

2. Répertoriez les VLAN disponibles dans cette zone et notez leur ID.
   ```
   ibmcloud ks vlans --zone <zone>
   ```
   {: pre}

3. Répertoriez les types de machine disponibles dans cette zone.
   ```
   ibmcloud ks machine-types --zone <zone>
   ```
   {: pre}

4. Ajoutez des noeuds worker autonomes dans le cluster. Pour les types de machine bare metal, indiquez `dedicated`.
   ```
   ibmcloud ks worker-add --cluster <cluster_name_or_ID> --workers <number_of_worker_nodes> --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID> --machine-type <machine_type> --hardware <shared_or_dedicated>
   ```
   {: pre}

5. Vérifiez que les noeuds worker ont été créés.
   ```
   ibmcloud ks workers --cluster <cluster_name_or_ID>
   ```
   {: pre}

<br />


## Ajout de libellés à des pools worker existants
{: #worker_pool_labels}

Vous pouvez affecter un libellé à un pool de noeuds worker lorsque vous [créez le pool de noeuds worker](#add_pool), ou ultérieurement en suivant ces étapes. Une fois qu'un pool de noeuds worker est libellé, tous les noeuds worker existants et suivants prennent ce libellé. Vous pouvez utiliser des libellés pour déployer des charges de travail spécifiques uniquement sur les noeuds worker présents dans le pool, par exemple, des [noeuds de périphérique pour un trafic réseau d'équilibreur de charge](/docs/containers?topic=containers-edge).
{: shortdesc}

Avant de commencer : [connectez-vous à votre compte. Le cas échéant, ciblez le groupe de ressources approprié. Définissez le contexte pour votre cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  Répertoriez les pools de noeuds worker disponibles dans votre cluster.
    ```
    ibmcloud ks worker-pools --cluster <cluster_name_or_ID>
    ```
    {: pre}
2.  Pour affecter au pool de noeuds worker un libellé `key=value`, utilisez l'[API de pool de noeuds worker PATCH ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/PatchWorkerPool). Formatez le corps de la demande comment dans l'exemple JSON ci-après.
    ```
    {
      "labels": {"key":"value"},
      "state": "labels"
    }
    ```
    {: codeblock}
3.  Vérifiez que le pool de noeuds worker et le noeud worker comportent le libellé `key=value` que vous avez affecté. 
    *   Pour vérifier les pools de noeuds worker :
        ```
        ibmcloud ks worker-pool-get --cluster <cluster_name_or_ID> --worker-pool <worker_pool_name_or_ID>
        ```
        {: pre}
    *   Pour vérifier les noeuds worker :
        1.  Répertoriez les noeuds worker figurant dans le pool de noeuds worker et notez l'**adresse IP privée**.
            ```
            ibmcloud ks workers --cluster <cluster_name_or_ID> --worker-pool <worker_pool_name_or_ID>
            ```
            {: pre}
        2.  Vérifiez la zone **Labels** dans la sortie.
            ```
            kubectl describe node <worker_node_private_IP>
            ```
            {: pre}

Une fois que vous avez libellé votre pool de noeuds worker, vous pouvez utiliser le [libellé dans vos déploiements d'application](/docs/containers?topic=containers-app#label) pour que vos charges de travail exécutent uniquement ces noeuds worker, ou des [annotations taint ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/) pour empêcher les déploiements de s'exécuter sur ces noeuds worker. 

<br />


## Reprise automatique pour vos noeuds worker
{: #planning_autorecovery}

Des composants essentiels, tels que `containerd`, `kubelet`, `kube-proxy` et `calico`, doivent être fonctionnels pour assurer un noeud worker Kubernetes sain. Avec le temps, ces composants peuvent connaître des défaillances et laisser votre noeud worker dans un état non opérationnel. Si les noeuds worker ne sont pas opérationnels, la capacité totale du cluster diminue et votre application peut devenir indisponible.
{:shortdesc}

Vous pouvez [configurer des diagnostics d'intégrité pour votre noeud worker et activer la reprise automatique](/docs/containers?topic=containers-health#autorecovery). Si le système de reprise automatique détecte un mauvais état de santé d'un noeud worker d'après les vérifications configurées, il déclenche une mesure corrective (par exemple, un rechargement du système d'exploitation) sur le noeud worker. Pour plus d'informations sur le fonctionnement de la reprise automatique, consultez le [blogue Autorecovery ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/blogs/bluemix/2017/12/autorecovery-utilizes-consistent-hashing-high-availability/).
