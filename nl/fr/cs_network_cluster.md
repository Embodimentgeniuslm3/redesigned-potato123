---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-06"

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


# Modification des noeuds finaux de service ou des connexions VLAN
{: #cs_network_cluster}

Après avoir configuré votre réseau lorsque vous [créez un cluster](/docs/containers?topic=containers-clusters), vous pouvez modifier les noeuds finaux de service via lesquels votre maître Kubernetes est accessible ou modifier les connexions VLAN pour vos noeuds worker.
{: shortdesc}

## Configuration du noeud final de service privé
{: #set-up-private-se}

Dans les clusters exécutant Kubernetes version 1.11 ou ultérieure, activez ou désactivez le noeud final de service privé pour votre cluster.
{: shortdesc}

Le noeud final de service privé rend votre maître Kubernetes accessible en privé. Vos noeuds worker et vos utilisateurs de cluster autorisés peuvent communiquer avec le maître Kubernetes sur le réseau privé. Pour déterminer si vous pouvez activer le noeud final de service privé, voir [Communication entre les noeuds worker et le maître et entre les utilisateurs et le maître](/docs/containers?topic=containers-plan_clusters#internet-facing). Notez que vous ne pouvez pas désactiver le noeud final de service privé une fois que vous l'avez activé.

Avez-vous créé un cluster avec un noeud final de service privé uniquement lorsque vous avez activé votre compte pour [VRF](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) et les [noeuds finaux de service](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started)? Essayez de [configurer le noeud final de service public](#set-up-public-se) de manière à pouvoir utiliser votre cluster jusqu'à ce que vos cas de support soient traités pour mettre à jour votre compte.
{: tip}

1. Activez [VRF](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) dans votre compte d'infrastructure IBM Cloud (SoftLayer).
2. [Activez votre compte {{site.data.keyword.Bluemix_notm}} pour utiliser des noeuds finaux de service](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started).
3. Activez le noeud final de service privé.
   ```
   ibmcloud ks cluster-feature-enable private-service-endpoint --cluster <cluster_name_or_ID>
   ```
   {: pre}
4. Actualisez le serveur d'API du maître Kubernetes pour l'utilisation du noeud final de service privé. Vous pouvez le faire à l'invite de l'interface de ligne de commande ou exécuter la commande suivante.
   ```
   ibmcloud ks apiserver-refresh --cluster <cluster_name_or_ID>
   ```
   {: pre}

5. [Créez une mappe de configuration (configmap)](/docs/containers?topic=containers-update#worker-up-configmap) pour contrôler le nombre maximal de noeuds worker pouvant être indisponibles à moment donné dans votre cluster. Lorsque vous mettez à jour vos noeuds worker, la mappe de configuration permet d'éviter l'interruption de vos applications car les applications sont replanifiées dans l'ordre sur les noeuds worker disponibles.
6. Mettez à jour tous les noeuds worker de votre cluster pour récupérer la configuration du noeud final de service privé.

   <p class="important">En émettant la commande de mise à jour, les noeuds worker sont rechargés pour récupérer la configuration du noeud final de service. Si aucun noeud worker n'est disponible, vous devez [recharger manuellement les noeuds worker](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli). Dans ce cas, veillez à effectuer les opérations cordon, drain et à gérer l'ordre pour contrôler le nombre maximal de noeuds worker indisponibles à moment donné.</p>
   ```
   ibmcloud ks worker-update --cluster <cluster_name_or_ID> --workers <worker1,worker2>
   ```
   {: pre}

8. Si le cluster se trouve dans un environnement protégé par un pare-feu :
  * [Autorisez vos utilisateurs de cluster autorisés à exécuter des commandes `kubectl` pour accéder au maître via le noeud final de service privé.](/docs/containers?topic=containers-firewall#firewall_kubectl)
  * [Autorisez le trafic réseau sortant sur les adresses IP privées](/docs/containers?topic=containers-firewall#firewall_outbound) pour les ressources d'infrastructure et les services {{site.data.keyword.Bluemix_notm}} que vous envisagez d'utiliser.

9. Facultatif : pour utiliser le noeud final de service privé uniquement, désactivez le noeud final de service public.
   ```
   ibmcloud ks cluster-feature-disable public-service-endpoint --cluster <cluster_name_or_ID>
   ```
   {: pre}

<br />


## Configuration du noeud final de service public
{: #set-up-public-se}

Activez ou désactivez le noeud final de service public pour votre cluster.
{: shortdesc}

Le noeud final de service public rend votre maître Kubernetes accessible au public. Vos noeuds worker et vos utilisateurs de cluster autorisés peuvent communiquer de manière sécurisée avec le maître Kubernetes sur le réseau public. Pour plus d'informations, voir [Communication entre les noeuds worker et le maître et entre les utilisateurs et le maître](/docs/containers?topic=containers-plan_clusters#internet-facing).

**Procédure d'activation**</br>
Si vous avez précédemment désactivé le noeud final public, vous pouvez le réactiver. 
1. Activez le noeud final de service public.
   ```
   ibmcloud ks cluster-feature-enable public-service-endpoint --cluster <cluster_name_or_ID>
   ```
   {: pre}
2. Actualisez le serveur d'API du maître Kubernetes pour l'utilisation du noeud final de service public. Vous pouvez le faire à l'invite de l'interface de ligne de commande ou exécuter la commande suivante.
   ```
   ibmcloud ks apiserver-refresh --cluster <cluster_name_or_ID>
   ```
   {: pre}

   </br>

**Procédure de désactivation**</br>
Pour désactiver le noeud final de service public, vous devez d'abord activer le noeud final de service privé pour que vos noeuds worker puissent communiquer avec le maître Kubernetes.
1. Activez le noeud final de service privé.
   ```
   ibmcloud ks cluster-feature-enable private-service-endpoint --cluster <cluster_name_or_ID>
   ```
   {: pre}
2. Actualisez le serveur d'API du maître Kubernetes pour l'utilisation du noeud final de service privé à l'invite de l'interface de ligne de commande ou en exécutant la commande suivante.
   ```
   ibmcloud ks apiserver-refresh --cluster <cluster_name_or_ID>
   ```
   {: pre}
3. [Créez une mappe de configuration (configmap)](/docs/containers?topic=containers-update#worker-up-configmap) pour contrôler le nombre maximal de noeuds worker pouvant être indisponibles à moment donné dans votre cluster. Lorsque vous mettez à jour vos noeuds worker, la mappe de configuration permet d'éviter l'interruption de vos applications car les applications sont replanifiées dans l'ordre sur les noeuds worker disponibles.

4. Mettez à jour tous les noeuds worker de votre cluster pour récupérer la configuration du noeud final de service privé.

   <p class="important">En émettant la commande de mise à jour, les noeuds worker sont rechargés pour récupérer la configuration du noeud final de service. Si aucun noeud worker n'est disponible, vous devez [recharger manuellement les noeuds worker](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli). Dans ce cas, veillez à effectuer les opérations cordon, drain et à gérer l'ordre pour contrôler le nombre maximal de noeuds worker indisponibles à moment donné.</p>
   ```
   ibmcloud ks worker-update --cluster <cluster_name_or_ID> --workers <worker1,worker2>
   ```
  {: pre}
5. Désactivez le noeud final de service public.
   ```
   ibmcloud ks cluster-feature-disable public-service-endpoint --cluster <cluster_name_or_ID>
   ```
   {: pre}

## Passage du noeud final de service public au noeud final de service privé
{: #migrate-to-private-se}

Dans les clusters exécutant Kubernetes version 1.11 ou ultérieure, activez les noeuds worker pour communiquer avec le maître via le réseau privé à la place du réseau public en activant le noeud final de service privé.
{: shortdesc}

Tous les clusters connectés à un VLAN public et un VLAN privé utilisent le noeud final de service public par défaut. Vos noeuds worker et vos utilisateurs de cluster autorisés peuvent communiquer de manière sécurisée avec le maître Kubernetes sur le réseau public. Pour activer les noeuds worker pour communiquer avec le maître Kubernetes via le réseau privé à la place du réseau public, vous pouvez activer le noeud final de service privé. Ensuite, vous pouvez éventuellement désactiver le noeud final de service public.
* Si vous activez le noeud final de service privé en conservant le noeud final de service public activé, les noeuds worker communiquent toujours avec le maître via le réseau privé, mais vos utilisateurs peuvent communiquer avec le maître via le réseau public ou privé.
* Si vous activez le noeud final de service privé en désactivant le noeud final de service public, les noeuds worker et les utilisateurs doivent communiquer avec le maître via le réseau privé.

Notez que vous ne pouvez pas désactiver le noeud final de service privé une fois que vous l'avez activé.

1. Activez [VRF](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) dans votre compte d'infrastructure IBM Cloud (SoftLayer).
2. [Activez votre compte {{site.data.keyword.Bluemix_notm}} pour utiliser des noeuds finaux de service](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started).
3. Activez le noeud final de service privé.
   ```
   ibmcloud ks cluster-feature-enable private-service-endpoint --cluster <cluster_name_or_ID>
   ```
   {: pre}
4. Actualisez le serveur d'API du maître Kubernetes pour l'utilisation du noeud final de service privé à l'invite de l'interface de ligne de commande ou en exécutant la commande suivante.
   ```
   ibmcloud ks apiserver-refresh --cluster <cluster_name_or_ID>
   ```
   {: pre}
5. [Créez une mappe de configuration (configmap)](/docs/containers?topic=containers-update#worker-up-configmap) pour contrôler le nombre maximal de noeuds worker pouvant être indisponibles à moment donné dans votre cluster. Lorsque vous mettez à jour vos noeuds worker, la mappe de configuration permet d'éviter l'interruption de vos applications car les applications sont replanifiées dans l'ordre sur les noeuds worker disponibles.

6.  Mettez à jour tous les noeuds worker de votre cluster pour récupérer la configuration du noeud final de service privé.

    <p class="important">En émettant la commande de mise à jour, les noeuds worker sont rechargés pour récupérer la configuration du noeud final de service. Si aucun noeud worker n'est disponible, vous devez [recharger manuellement les noeuds worker](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli). Dans ce cas, veillez à effectuer les opérations cordon, drain et à gérer l'ordre pour contrôler le nombre maximal de noeuds worker indisponibles à moment donné.</p>
    ```
    ibmcloud ks worker-update --cluster <cluster_name_or_ID> --workers <worker1,worker2>
    ```
    {: pre}

7. Facultatif : désactivez le noeud final de service public.
   ```
   ibmcloud ks cluster-feature-disable public-service-endpoint --cluster <cluster_name_or_ID>
   ```
   {: pre}

<br />


## Modification des connexions de VLAN de vos noeuds worker
{: #change-vlans}

Lorsque vous créez un cluster, vous déterminez si la connexion de vos noeuds worker doit s'effectuer avec un VLAN public et un VLAN privé ou uniquement avec un VLAN privé. Vos noeuds worker font partie de pools de noeuds worker qui stockent les métadonnées de réseau incluant les VLAN à utiliser pour mettre à disposition les futurs noeuds worker dans le pool. Vous envisagerez peut-être de modifier la configuration de la connectivité de VLAN de votre cluster ultérieurement dans les cas suivants.
{: shortdesc}

* Les VLAN du pool de noeuds worker dans une zone ont atteint leur capacité maximale et vous devez prévoir l'utilisation d'un nouveau VLAN pour les noeuds worker de votre cluster.
* Vous disposez d'un cluster avec des noeuds worker figurant à la fois sur des VLAN public et privé, mais vous voulez passer à un [cluster privé uniquement](/docs/containers?topic=containers-plan_clusters#private_clusters).
* Vous disposez d'un cluster privé uniquement, mais vous souhaitez avoir quelques noeuds worker, par exemple un pool de noeuds worker de [noeuds de périphérie](/docs/containers?topic=containers-edge#edge) sur le VLAN public pour exposer vos applications sur Internet.

Vous essayez à la place de modifier le noeud final de service pour la communication entre le maître et les noeuds worker ? Consultez la rubrique sur la configuration de noeuds finaux de service [publics](#set-up-public-se) et [privés](#set-up-private-se).
{: tip}

Avant de commencer :
* [Connectez-vous à votre compte. Le cas échéant, ciblez le groupe de ressources approprié. Définissez le contexte pour votre cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
* Si vos noeuds worker sont autonomes (s'ils ne font pas partie d'un pool de noeuds worker), [mettez-les à jour pour les intégrer dans un pool de noeuds worker](/docs/containers?topic=containers-update#standalone_to_workerpool).

Pour modifier les VLAN utilisés par un pool de noeuds worker pour mettre à disposition des noeuds worker :

1. Affichez la liste des pools de noeuds worker dans votre cluster.
  ```
  ibmcloud ks worker-pools --cluster <cluster_name_or_ID>
  ```
  {: pre}

2. Déterminez les zones d'un des pools de noeuds worker. Dans la sortie, recherchez **Zones**.
  ```
  ibmcloud ks worker-pool-get --cluster <cluster_name_or_ID> --worker-pool <pool_name>
  ```
  {: pre}

3. Pour chaque zone identifiée à l'étape précédente, obtenez un VLAN public et un VLAN privé compatibles.

  1. Vérifiez les VLAN publics et privés répertoriés sous **Type** dans la sortie.
     ```
     ibmcloud ks vlans --zone <zone>
     ```
     {: pre}

  2. Vérifiez que le VLAN public et le VLAN privé dans la zone sont compatibles. Pour qu'ils soient compatibles, le routeur (**Router**) doit avoir le même ID de pod. Dans cet exemple, les ID de pod de **Router** correspondent : `01a` et `01a`. Si un ID de pod était `01a` et que l'autre était `02a`, vous ne pourriez pas définir ces ID de VLAN public et privé pour votre pool de noeuds worker.
     ```
     ID        Name   Number   Type      Router         Supports Virtual Workers
    229xxxx          1234     private   bcr01a.dal12   true
    229xxxx          5678     public    fcr01a.dal12   true
     ```
     {: screen}

  3. Si vous devez commander un nouveau VLAN public et un nouveau VLAN privé, vous pouvez le faire dans la [console {{site.data.keyword.Bluemix_notm}}](/docs/infrastructure/vlans?topic=vlans-ordering-premium-vlans#ordering-premium-vlans) ou utiliser la commande suivante. N'oubliez pas que les VLAN doivent être compatibles, avec des ID de pod de **Router** correspondants, comme indiqué à l'étape précédente. Si vous créez une paire de nouveaux VLAN public-privé, ces VLAN doivent être compatibles.
     ```
     ibmcloud sl vlan create -t [public|private] -d <zone> -r <compatible_router>
     ```
     {: pre}

  4. Notez les ID des VLAN compatibles.

4. Configurez un pool de noeuds worker avec les mêmes métadonnées de réseau VLAN pour chaque zone. Vous pouvez créer un nouveau pool de noeuds worker ou modifier un pool existant.

  * **Créer un nouveau pool de noeuds worker** : voir [Ajout de noeuds worker en créant un nouveau pool de noeuds worker](/docs/containers?topic=containers-add_workers#add_pool).

  * **Modifier un pool de noeuds worker existant** : définissez les métadonnées de réseau du pool de noeuds worker pour utiliser le VLAN pour chaque zone. Les noeuds worker déjà créés dans le pool continuent à utiliser les VLAN précédents, mais les nouveaux noeuds worker du pool utilisent les métadonnées du nouveau VLAN que vous avez défini.

    * Exemple pour ajouter un VLAN public et un VLAN privé, comme si vous passiez d'un VLAN privé uniquement à une paire de VLAN public-privé :
      ```
      ibmcloud ks zone-network-set --zone <zone> --cluster <cluster_name_or_ID> --worker-pools <pool_name> --private-vlan <private_vlan_id> --public-vlan <public_vlan_id>
      ```
      {: pre}

    * Exemple pour ajouter uniquement un VLAN privé, comme si vous passiez d'une paire de VLAN public-privé à un VLAN privé uniquement lorsque vous avez un [compte avec la fonction VRF activée qui utilise des noeuds finaux de service](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started) :
      ```
      ibmcloud ks zone-network-set --zone <zone> --cluster <cluster_name_or_ID> --worker-pools <pool_name> --private-vlan <private_vlan_id> --public-vlan <public_vlan_id>
      ```
      {: pre}

5. Ajoutez des noeuds worker dans le pool de noeuds worker en redimensionnant le pool.
   ```
   ibmcloud ks worker-pool-resize --cluster <cluster_name_or_ID> --worker-pool <pool_name>  --size-per-zone <number_of_workers_per_zone>
   ```
   {: pre}

   Si vous souhaitez retirer des noeuds worker qui utilisent les anciennes métadonnées de réseau, modifiez le nombre de noeuds worker par zone pour doubler leur nombre dans chaque zone. Un peu plus loin dans cette procédure, vous pourrez effectuer des opérations cordon, drain et remove sur les noeuds worker précédents.
  {: tip}

6. Vérifiez que les nouveaux noeuds worker sont créés avec les adresses **IP publique** et **IP privée** appropriées dans la sortie. Par exemple, si vous passez d'un pool de noeuds worker avec une paire de VLAN public-privé à un pool de noeuds worker avec VLAN privé uniquement, les nouveaux noeuds worker n'auront qu'une adresse IP privée. Inversement, si vous passez d'un pool de noeuds worker avec un VLAN privé uniquement à un pool de noeuds worker avec une paire de VLAN public-privé, les nouveaux noeuds worker auront une adresse IP publique et une adresse IP privée.
   ```
   ibmcloud ks workers --cluster <cluster_name_or_ID> --worker-pool <pool_name>
   ```
   {: pre}

7. Facultatif : retirez les noeuds worker avec les métadonnées de réseau précédentes du pool de noeuds worker.
  1. Dans la sortie de l'étape précédente, notez l'**ID** et l'adresse IP privée (**Private IP**) des noeuds worker que vous souhaitez retirer du pool de noeuds worker.
  2. Marquez le noeud worker comme non planifiable dans un processus désigné par cordon. Lorsque vous exécutez ce processus sur un noeud worker, vous le rendez indisponible pour toute planification de pod ultérieure.
     ```
     kubectl cordon <worker_private_ip>
     ```
     {: pre}
  3. Vérifiez que la planification de pod est désactivée pour votre noeud worker.
     ```
     kubectl get nodes
     ```
     {: pre}
     Votre noeud worker n'est pas activé pour la planification de pod si le statut affiche **`SchedulingDisabled`**.
  4. Imposez le retrait des pods de votre noeud worker et leur replanification sur les noeuds worker restants dans le cluster.
     ```
     kubectl drain <worker_private_ip>
     ```
     {: pre}
     Ce processus peut prendre quelques minutes.
  5. Supprimez le noeud worker. Utilisez l'ID du noeud worker que vous avez récupéré précédemment.
     ```
     ibmcloud ks worker-rm --cluster <cluster_name_or_ID> --worker <worker_name_or_ID>
     ```
     {: pre}
  6. Vérifiez que le noeud worker est supprimé.
     ```
     ibmcloud ks workers --cluster <cluster_name_or_ID> --worker-pool <pool_name>
     ```
     {: pre}

8. Facultatif : vous pouvez répéter les étapes 2 à 7 pour chaque pool de noeuds worker dans votre cluster. Lorsque vous avez terminé, tous les noeuds worker de votre cluster sont configurés avec les nouveaux VLAN.

9. Les ALB par défaut dans votre cluster sont toujours liés à l'ancien VLAN car leurs adresses IP proviennent d'un sous-réseau de ce VLAN. Les ALB pouvant être déplacés d'un VLAN à un autre, vous pouvez à la place [créer des ALB sur les nouveaux VLAN et désactiver les ALB sur les anciens VLAN](/docs/containers?topic=containers-ingress#migrate-alb-vlan).
