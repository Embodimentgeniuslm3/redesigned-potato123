---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-06"

keywords: kubernetes, iks, ImagePullBackOff, registry, image, failed to pull image,

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


# Traitement des incidents liés aux clusters et aux noeuds worker
{: #cs_troubleshoot_clusters}

Lorsque vous utilisez {{site.data.keyword.containerlong}}, tenez compte de ces techniques pour identifier et résoudre les incidents liés à vos clusters et à vos noeuds worker.
{: shortdesc}

<p class="tip">Si vous rencontrez un problème d'ordre plus général, expérimentez le [débogage de cluster](/docs/containers?topic=containers-cs_troubleshoot).<br>En outre, lorsque vous traitez les incidents, vous pouvez utiliser l'[outil de débogage et de diagnostic d'{{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-cs_troubleshoot#debug_utility) pour exécuter des tests et regrouper des informations pertinentes dans votre cluster.</p>

## Impossible de créer un cluster ou de gérer des noeuds worker suite à des erreurs de droits
{: #cs_credentials}

{: tsSymptoms}
Vous essayez de gérer des noeuds worker pour un cluster nouveau ou existant en exécutant l'une des commandes suivantes.
* Mise à disposition de noeuds worker : `ibmcloud ks cluster-create`, `ibmcloud ks worker-pool-rebalance` ou `ibmcloud ks worker-pool-resize`
* Rechargement de noeuds worker : `ibmcloud ks worker-reload` ou `ibmcloud ks worker-update`
* Réamorçage de noeuds worker : `ibmcloud ks worker-reboot`
* Suppression de noeuds worker : `ibmcloud ks cluster-rm`, `ibmcloud ks worker-rm`, `ibmcloud ks worker-pool-rebalance` ou `ibmcloud ks worker-pool-resize`

Cependant, vous recevez un erreur message de ce type.

```
Nous n'avons pas pu nous connecter à votre compte d'infrastructure IBM Cloud.
La création d'un cluster standard nécessite un compte de type Paiement à la carte lié aux
conditions d'un compte d'infrastructure IBM Cloud (SoftLayer) ou l'utilisation de l'interface de ligne de commande {{site.data.keyword.containerlong_notm}} pour définir vos clés d'API d'infrastructure {{site.data.keyword.Bluemix_notm}}.
```
{: screen}

```
Exception liée à l'infrastructure {{site.data.keyword.Bluemix_notm}} : des droits sont nécessaires pour réserver 'Item'.
```
{: screen}

```
Noeud worker introuvable. Vérifiez les droits sur l'infrastructure {{site.data.keyword.Bluemix_notm}}.
```
{: screen}

```
Exception liée à l'infrastructure {{site.data.keyword.Bluemix_notm}} : l'utilisateur ne dispose pas des droits sur l'infrastructure {{site.data.keyword.Bluemix_notm}} nécessaires pour ajouter des serveurs
```
{: screen}

```
Echec de la demande d'échange de jetons IAM : Impossible de créer un jeton de portail IMS, car aucun compte IMS n'est lié au compte BSS sélectionné
```
{: screen}

```
Le cluster n'a pas pu être configuré avec le registre. Vérifiez que vous disposez du rôle Administrateur pour {{site.data.keyword.registrylong_notm}}.
```
{: screen}

{: tsCauses}
Les [droits sur l'infrastructure](/docs/containers?topic=containers-access_reference#infra) sont manquants pour les données d'identification de l'infrastructure qui sont définies pour la région et le groupe de ressources. Les données d'identification d'infrastructure de l'utilisateur sont généralement stockées sous forme de [clé d'API](/docs/containers?topic=containers-users#api_key) pour la région et le groupe de ressources. Plus rarement, si vous utilisez un [autre type de compte {{site.data.keyword.Bluemix_notm}}](/docs/containers?topic=containers-users#understand_infra), vous devrez peut-être [définir manuellement les données d'identification d'infrastructure](/docs/containers?topic=containers-users#credentials). Si vous utilisez un autre compte d'infrastructure IBM Cloud (SoftLayer) pour mettre à disposition des ressources d'infrastructure, vous risquez d'avoir des [clusters orphelins](#orphaned) dans votre compte.

{: tsResolve}
Le propriétaire de compte doit configurer correctement les données d'identification du compte d'infrastructure. Ces données dépendent du type de compte d'infrastructure que vous utilisez.

Avant de commencer, [connectez-vous à votre compte. Le cas échéant, ciblez le groupe de ressources approprié. Définissez le contexte pour votre cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).

1.  Identifiez les données d'identification d'utilisateur qui sont utilisées pour les droits d'infrastructure de la région et du groupe de ressources. 
    1.  Vérifiez la clé d'API pour une région et un groupe de ressources du cluster.
        ```
        ibmcloud ks api-key-info --cluster <cluster_name_or_ID>
        ```
        {: pre}

        Exemple de sortie :
        ```
        Getting information about the API key owner for cluster <cluster_name>...
        OK
        Name                Email   
        <user_name>         <name@email.com>
        ```
        {: screen}
    2.  Vérifiez si le compte d'infrastructure pour la région et le groupe de ressources est défini manuellement pour utiliser un autre compte d'infrastructure IBM Cloud (SoftLayer).
        ```
        ibmcloud ks credential-get --region <us-south>
        ```
        {: pre}

        **Exemple de sortie si des données d'identification sont définies pour utiliser un autre compte**. Dans ce cas, les données d'identification d'infrastructure de l'utilisateur sont utilisées pour la région et le groupe de ressources que vous avez ciblés, même si les données d'identification d'un autre utilisateur sont stockées dans la clé d'API que vous avez extraite à l'étape précédente.
        ```
        OK
        Infrastructure credentials for user name <1234567_name@email.com> set for resource group <resource_group_name>.
        ```
        {: screen}

        **Exemple de sortie si des données d'identification ne sont pas définies pour utiliser un autre compte**. Dans ce cas, le propriétaire de la clé d'API que vous avez extraite à l'étape précédente possède les données d'identification d'infrastructure qui sont utilisées pour la région et le groupe de ressources.
        ```
        FAILED
        No credentials set for resource group <resource_group_name>.: The user credentials could not be found. (E0051)
        ```
        {: screen}
2.  Validez les droits d'infrastructure dont l'utilisateur dispose. 
    1.  Répertoriez les droits d'infrastructure suggérés et requis pour la région et le groupe de ressources.
        ```
        ibmcloud ks infra-permissions-get --region <region>
        ```
        {: pre}
    2.  Assurez-vous que le [propriétaire de données d'identification d'infrastructure pour la clé d'API ou le compte défini manuellement dispose des droits appropriés](/docs/containers?topic=containers-users#owner_permissions).
    3.  Si nécessaire, vous pouvez modifier la [clé d'API](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_api_key_reset) ou [définir manuellement](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_set) le propriétaire des données d'identification d'infrastructure pour la région et le groupe de ressources.
3.  Assurez-vous que les droits ainsi modifiés permettent aux utilisateurs autorisés d'effectuer des opérations d'infrastructure pour le cluster.
    1.  Par exemple, vous souhaiterez peut-être supprimer un noeud worker.
        ```
        ibmcloud ks worker-rm --cluster <cluster_name_or_ID> --worker <worker_node_ID>
        ```
        {: pre}
    2.  Assurez-vous que le noeud worker est retiré.
        ```
        ibmcloud ks worker-get --cluster <cluster_name_or_ID> --worker <worker_node_ID>
        ```
        {: pre}

        Exemple de sortie si le retrait du noeud worker a abouti. L'opération `worker-get` échoue car le noeud worker est supprimé. Les droits d'infrastructure sont correctement configurés.
        ```
        FAILED
        The specified worker node could not be found. (E0011)
        ```
        {: screen}

    3.  Si le noeud worker n'est pas retiré, vérifiez les [zones **Etat** et **Statut**](/docs/containers?topic=containers-cs_troubleshoot#debug_worker_nodes), ainsi que les [problèmes courants liés aux noeuds worker](/docs/containers?topic=containers-cs_troubleshoot#common_worker_nodes_issues) pour passer au débogage. 
    4.  Si vous définissez manuellement des données d'identification et les noeuds worker du cluster ne sont toujours pas visibles dans votre compte d'infrastructure, vous pouvez vérifier si le [cluster est orphelin](#orphaned).

<br />


## Le pare-feu empêche l'exécution de commandes via la ligne de commande
{: #ts_firewall_clis}

{: tsSymptoms}
Lorsque vous exécutez des commandes `ibmcloud`, `kubectl` ou `calicoctl` depuis l'interface de ligne de commande, ces commandes échouent.

{: tsCauses}
Des règles réseau d'entreprise empêchent peut-être l'accès depuis votre système local à des noeuds finaux publics via des proxys ou des pare-feux.

{: tsResolve}
[Autorisez l'accès TCP afin que les commandes CLI fonctionnent](/docs/containers?topic=containers-firewall#firewall_bx). Cette tâche nécessite le [rôle de plateforme {{site.data.keyword.Bluemix_notm}} IAM **Administrateur**](/docs/containers?topic=containers-users#platform) pour le cluster.


## Impossible d'accéder aux ressources dans mon cluster
{: #cs_firewall}

{: tsSymptoms}
Lorsque les noeuds worker de votre cluster ne parviennent pas à communiquer sur le réseau privé, vous pouvez voir différents types de symptôme.

- Exemple de message d'erreur lorsque vous exécutez la commande `kubectl exec`, `attach`, `logs`, `proxy` ou `port-forward` :
  ```
  Error from server: error dialing backend: dial tcp XXX.XXX.XXX:10250: getsockopt: connection timed out
  ```
  {: screen}

- Exemple de message d'erreur lorsque la commande `kubectl proxy` aboutit mais que le tableau de bord Kubernetes n'est pas disponible :
  ```
  timeout on 172.xxx.xxx.xxx
  ```
  {: screen}

- Erreur de message d'erreur lorsque la commande `kubectl proxy` échoue ou en cas d'échec de connexion à votre service :
  ```
  Connection refused
  ```
  {: screen}

  ```
  Connection timed out
  ```
  {: screen}

  ```
  Unable to connect to the server: net/http: TLS handshake timeout
  ```
  {: screen}


{: tsCauses}
Pour accéder aux ressources dans un cluster, vos noeuds worker doivent être en mesure de communiquer sur le réseau privé. Vous disposez éventuellement de Vyatta ou d'un autre pare-feu configuré ou vous avez peut-être personnalisé les paramètres de votre pare-feu dans votre compte d'infrastructure IBM Cloud (SoftLayer). {{site.data.keyword.containerlong_notm}} requiert que certaines adresses IP et certains ports soient ouverts pour permettre la communication entre le noeud worker et le maître Kubernetes et inversement. Si vos noeuds worker sont répartis entre plusieurs zones, vous devez autoriser la communication sur le réseau privé en activant la fonction Spanning VLAN. La communication entre les noeuds worker peut également s'avérer impossible si vos noeuds worker sont pris dans une boucle de rechargement.

{: tsResolve}
1. Répertoriez les noeuds worker dans votre cluster et vérifiez qu'ils ne sont pas immobilisés à l'état `Reloading`.
   ```
   ibmcloud ks workers --cluster <cluster_name_or_id>
   ```
   {: pre}

2. Si vous disposez d'un cluster à zones multiples et que votre compte n'est pas activé pour la fonction de routeur virtuel (VRF), vérifiez que vous [avez activé le spanning VLAN](/docs/containers?topic=containers-subnets#subnet-routing) pour votre compte.
3. Si vous disposez de Vyatta ou d'autres valeurs de pare-feu personnalisé, assurez-vous que vous [avez ouvert les ports nécessaires](/docs/containers?topic=containers-firewall#firewall_outbound) pour autoriser le cluster à accéder aux services et aux ressources d'infrastructure.

<br />



## Impossible de visualiser ou d'utiliser un cluster
{: #cs_cluster_access}

{: tsSymptoms}
* Vous ne parvenez pas à trouver un cluster. Lorsque vous exécutez la commande `ibmcloud ks clusters`, le cluster n'apparaît pas dans le résultat.
* Vous n'êtes pas en mesure d'utiliser un cluster. Lorsque vous exécutez la commande `ibmcloud ks cluster-config` ou d'autres commandes spécifiques au cluster, le cluster est introuvable.


{: tsCauses}
Dans {{site.data.keyword.Bluemix_notm}}, chaque ressource doit figurer dans un groupe de ressources. Par exemple, le cluster `mycluster` doit exister dans le groupe de ressources `default`. Lorsque le propriétaire du compte vous donne accès aux ressources en vous affectant un rôle de plateforme {{site.data.keyword.Bluemix_notm}} IAM, l'accès peut être octroyé à une ressource spécifique ou au groupe de ressources. Lorsque l'accès à une ressource spécifique vous a été octroyé, vous n'avez pas accès au groupe de ressources. Dans ce cas, vous n'avez pas besoin de cibler un groupe de ressource pour utiliser les clusters auxquels vous avez accès. Si vous ciblez un autre groupe de ressources que le groupe dans lequel figure le cluster, les actions par rapport à ce cluster risquent d'échouer. Inversement, lorsque l'accès à une ressource vous est affecté dans le cadre de votre accès à un groupe de ressources, vous devez cibler un groupe de ressources pour utiliser un cluster dans ce groupe. Si vous ne ciblez pas votre session d'interface de ligne de commande sur le groupe de ressources dans lequel figure le cluster, les actions par rapport à ce cluster risquent d'échouer.

Si vous ne pouvez pas trouver ou utiliser un cluster, vous pouvez rencontrer les problèmes suivants :
* Vous avez accès au cluster et au groupe de ressources dans lequel figure le cluster mais votre session d'interface de ligne de commande n'est pas ciblée sur le groupe de ressources hébergeant le cluster.
* Vous avez accès au cluster mais pas dans le cadre du groupe de ressources dans lequel figure le cluster. Votre session d'interface de ligne de commande est ciblée sur ce groupe de ressources ou un autre groupe de ressources.
* Vous n'avez pas accès au cluster.

{: tsResolve}
Pour vérifier vos droits d'accès utilisateur :

1. Répertoriez tous les droits utilisateur dont vous disposez.
    ```
    ibmcloud iam user-policies <your_user_name>
    ```
    {: pre}

2. Vérifiez si vous avez accès au cluster et au groupe de ressources dans lequel figure le cluster.
    1. Recherchez une règle ayant une valeur **Resource Group Name** pour le groupe de ressources du cluster et une valeur **Memo** correspondant à `Policy applies to the resource group`. Si vous avez cette règle, vous avez accès au groupe de ressources. Par exemple, cette règle indique qu'un utilisateur a accès au groupe de ressources `test-rg` :
        ```
        Policy ID:   3ec2c069-fc64-4916-af9e-e6f318e2a16c
        Roles:       Viewer
        Resources:
                     Resource Group ID     50c9b81c983e438b8e42b2e8eca04065
                     Resource Group Name   test-rg
                     Memo                  Policy applies to the resource group
        ```
        {: screen}
    2. Recherchez une règle ayant une valeur **Resource Group Name** pour le groupe de ressources du cluster, une valeur de **Service Name** correspondant à `containers-kubernetes` ou aucune valeur, et une valeur de **Memo** correspondant à `Policy applies to the resource(s) within the resource group`. Si vous trouvez cette règle, vous avez accès aux clusters ou à toutes les ressources au sein du groupe de ressources. Par exemple, cette règle indique qu'un utilisateur a accès aux clusters du groupe de ressources `test-rg` :
        ```
        Policy ID:   e0ad889d-56ba-416c-89ae-a03f3cd8eeea
        Roles:       Administrator
        Resources:
                     Resource Group ID     a8a12accd63b437bbd6d58fb6a462ca7
                     Resource Group Name   test-rg
                     Service Name          containers-kubernetes
                     Service Instance
                     Region
                     Resource Type
                     Resource
                     Memo                  Policy applies to the resource(s) within the resource group
        ```
        {: screen}
    3. Si vous disposez de ces deux règles, passez au premier point de l'étape 4. Si vous ne disposez pas de la règle de l'étape 2a et que vous disposez de la règle de l'étape 2b, passez au deuxième point de l'étape 4. Si vous ne disposez d'aucune de ces règles, passez à l'étape 3.

3. Vérifiez si vous avez accès au cluster mais pas dans le cadre de l'accès au groupe de ressources dans lequel figure le cluster.
    1. Recherchez la règle n'ayant aucune valeur pour les zones **Policy ID** et **Roles**. Si vous disposez de cette règle, vous avez accès au cluster dans le cadre de l'accès à votre compte complet. Par exemple, cette règle indique qu'un utilisateur a accès à toutes les ressources du compte :
        ```
        Policy ID:   8898bdfd-d520-49a7-85f8-c0d382c4934e
        Roles:       Administrator, Manager
        Resources:
                     Service Name
                     Service Instance
                     Region
                     Resource Type
                     Resource
        ```
        {: screen}
    2. Recherchez une règle dont la valeur de **Service Name** est `containers-kubernetes` et la valeur de **Service Instance** correspond à l'ID du cluster. Vous pouvez obtenir l'ID d'un cluster en exécutant la commande `ibmcloud ks cluster-get --cluster <cluster_name>`. Par exemple, cette règle indique qu'un utilisateur a accès à un cluster spécifique :
        ```
        Policy ID:   140555ce-93ac-4fb2-b15d-6ad726795d90
        Roles:       Administrator
        Resources:
                     Service Name       containers-kubernetes
                     Service Instance   df253b6025d64944ab99ed63bb4567b6
                     Region
                     Resource Type
                     Resource
        ```
        {: screen}
    3. Si vous disposez de l'une de ces règles, passez au second point de l'étape 4. Si vous n'avez aucune de ces règles, passez au troisième point de l'étape 4.

4. En fonction de vos règles d'accès, choisissez l'une des options suivantes.
    * Si vous avez accès au cluster et au groupe de ressources dans lequel figure le cluster :
      1. Ciblez le groupe de ressources. **Remarque** : vous ne pouvez pas utiliser les clusters d'autres groupes de ressources tant que vous ciblez ce groupe de ressources.
          ```
          ibmcloud target -g <resource_group>
          ```
          {: pre}

      2. Ciblez le cluster.
          ```
          ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
          ```
          {: pre}

    * Si vous avez accès au cluster mais pas au groupe de ressources dans lequel figure le cluster :
      1. Ne ciblez pas de groupe de ressources. Si vous avez déjà ciblé un groupe de ressources, ne le ciblez plus :
        ```
        ibmcloud target --unset-resource-group
        ```
        {: pre}

      2. Ciblez le cluster.
        ```
        ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
        ```
        {: pre}

    * Si vous n'avez pas accès au cluster :
        1. Demandez au propriétaire de votre compte de vous affecter un [rôle de plateforme {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform) pour ce cluster.
        2. Ne ciblez pas de groupe de ressources. Si vous avez déjà ciblé un groupe de ressources, arrêtez de le cibler :
          ```
          ibmcloud target --unset-resource-group
          ```
          {: pre}
        3. Ciblez le cluster.
          ```
          ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
          ```
          {: pre}

<br />


## L'accès à votre noeud worker à l'aide de SSH échoue
{: #cs_ssh_worker}

{: tsSymptoms}
Vous ne pouvez pas accéder à votre noeud worker à l'aide d'une connexion SSH.

{: tsCauses}
La connexion SSH par mot de passe n'est pas disponible sur les noeuds worker.

{: tsResolve}
Utilisez un objet Kubernetes [`DaemonSet` ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) pour les actions que vous devez effectuer sur tous les noeuds, ou utilisez des travaux pour les actions à usage unique que vous devez exécuter.

<br />


## L'ID d'instance bare metal n'est pas cohérent avec les enregistrements de noeuds worker
{: #bm_machine_id}

{: tsSymptoms}
Lorsque vous utilisez des commandes `ibmcloud ks worker` avec votre noeud worker bare metal, vous obtenez un message de ce type :

```
Instance ID inconsistent with worker records
```
{: screen}

{: tsCauses}
L'ID de la machine peut devenir incohérent avec l'enregistrement du noeud worker {{site.data.keyword.containerlong_notm}} lorsque cette machine fait l'objet de problèmes matériel. Lorsque l'infrastructure IBM Cloud (SoftLayer) résout un problème de ce type, un composant peut être remplacé dans le système et le service ne parvient pas à l'identifier.

{: tsResolve}
Pour qu'{{site.data.keyword.containerlong_notm}} identifie à nouveau cette machine, [rechargez le noeud worker bare metal](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload). **Remarque** : l'opération de rechargement met également à jour la [version du correctif](/docs/containers?topic=containers-changelog).

Vous pouvez également [supprimer le noeud worker bare metal](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_rm). **Remarque** : les instances bare metal sont facturées au mois.

<br />


## Impossible de modifier ou de supprimer des ressources d'infrastructure dans un cluster orphelin
{: #orphaned}

{: tsSymptoms}
Vous ne parvenez pas à exécuter des commandes liées à l'infrastructure sur votre cluster, par exemple :
* Ajouter ou retirer des noeuds worker
* Recharger ou réamorcer des noeuds worker
* Redimensionner des pools de noeuds worker
* Mettre à jour votre cluster

Vous ne pouvez pas visualiser les noeuds worker du cluster dans votre compte d'infrastructure IBM Cloud (SoftLayer). Cependant, vous ne pouvez pas mettre à jour et gérer d'autres clusters du compte.

De plus, vous avez vérifié que vous disposiez des [données d'identification d'infrastructure appropriées](#cs_credentials).

{: tsCauses}
Il se peut que le cluster soit mis à disposition dans un compte d'infrastructure IBM Cloud (SoftLayer) qui n'est plus lié à votre compte {{site.data.keyword.containerlong_notm}}. Le cluster est orphelin. Comme les ressources se trouvent dans un autre compte, vous ne disposez pas des données d'identification requises pour modifier ces ressources.

Considérez le scénario suivant pour savoir comment les clusters peuvent devenir orphelins.
1.  Vous disposez d'un compte {{site.data.keyword.Bluemix_notm}} Paiement à la carte.
2.  Vous créez un cluster nommé `Cluster1`. Les noeuds worker et d'autres ressources d'infrastructure sont mises à disposition dans le compte d'infrastructure fourni avec votre compte Paiement à la carte.
3.  Par la suite, vous constatez que votre équipe utilise un compte d'infrastructure IBM Cloud (SoftLayer) existant ou partagé. Vous utilisez la commande `ibmcloud ks credential-set` pour modifier les données d'identification du compte d'infrastructure IBM Cloud (SoftLayer) afin d'utiliser le compte de votre équipe.
4.  Vous créez un autre cluster nommé `Cluster2`. Les noeuds worker et d'autres ressources d'infrastructure sont mises à disposition dans le compte d'infrastructure de l'équipe.
5.  Vous remarquez que le cluster `Cluster1` nécessite une mise à jour ou un rechargement de noeud worker ou vous voulez juste le nettoyer en le supprimant. Cependant, comme `Cluster1` a été mis à disposition dans un autre compte d'infrastructure, vous ne pouvez pas modifier ses ressources d'infrastructure. `Cluster1` est un cluster orphelin.
6.  Vous suivez les étapes de résolution dans la section suivante, sans redéfinir vos données d'identification dans le compte de votre équipe. Vous pouvez supprimer `Cluster1`, mais `Cluster2` est orphelin.
7.  Vous restaurez vos données d'identification du compte d'infrastructure dans le compte de l'équipe qui a créé `Cluster2`. Désormais vous n'avez plus de cluster orphelin.

<br>

{: tsResolve}
1.  Vérifiez quel est le compte d'infrastructure que la région dans laquelle se trouve votre cluster utilise pour mettre à disposition des clusters.
    1.  Connectez-vous à la [console de clusters {{site.data.keyword.containerlong_notm}} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/kubernetes/clusters).
    2.  Dans le tableau, sélectionnez votre cluster.
    3.  Dans l'onglet **Vue d'ensemble**, recherchez une zone correspondant à un **utilisateur d'infrastructure**. Cette zone vous aide à déterminer si votre compte {{site.data.keyword.containerlong_notm}} utilise un autre compte d'infrastructure que le compte par défaut.
        * Si vous ne voyez pas la zone de l'**utilisateur d'infrastructure**, vous disposez d'un compte de paiement à la carte qui utilise les mêmes données d'identification pour vos comptes d'infrastructure et de plateforme. Le cluster qui ne peut pas être modifié peut être mis à disposition dans un autre compte d'infrastructure.
        * Si vous voyez une zone de l'**utilisateur d'infrastructure**, vous utilisez un autre compte d'infrastructure que celui fourni avec votre compte Paiement à la carte. Ces données d'identification s'appliquent à tous les clusters de la région. Le cluster qui ne peut pas être modifié peut être mis à disposition dans un compte Paiement à la carte ou un autre compte d'infrastructure.
2.  Vérifiez quel est le compte d'infrastructure qui a été utilisé pour mettre à disposition le cluster.
    1.  Dans l'onglet **Noeuds worker**, sélectionnez un noeud worker et notez son **ID**.
    2.  Ouvrez le menu ![Icône de menu](../icons/icon_hamburger.svg "Icône de menu") et cliquez sur **Infrastructure classique**.
    3.  Dans le panneau de navigation de l'infrastructure, cliquez sur **Unités > Liste des unités**.
    4.  Recherchez l'ID du noeud worker que vous avez noté précédemment.
    5.  Si vous ne trouvez pas l'ID du noeud worker, c'est qu'il n'est pas mis à disposition dans ce compte d'infrastructure. Passez à un autre compte d'infrastructure et réessayez.
3.  Utilisez la [commande](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_set) `ibmcloud ks credential-set` pour modifier vos données d'identification d'infrastructure dans le compte où sont mis à disposition les noeuds worker du cluster, que vous avez obtenu à l'étape précédente.
    Si vous n'avez plus accès à ces données d'identification ou que vous ne pouvez pas les obtenir, vous pouvez ouvrir un cas de support {{site.data.keyword.Bluemix_notm}} pour supprimer le cluster orphelin.
    {: note}
4.  [Supprimez le cluster](/docs/containers?topic=containers-remove).
5.  Si vous le souhaitez, réinitialisez les données d'identification dans le compte précédent. Notez que si vous avez créé des clusters avec un autre compte d'infrastructure que le compte où vous avez basculé, vous pouvez rendre ces clusters orphelins.
    * Pour définir les données d'identification à un autre compte d'infrastructure, utilisez la [commande](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_set) `ibmcloud ks credential-set`. 
    * Pour utiliser les données d'identification par défaut fournies avec votre compte {{site.data.keyword.Bluemix_notm}} de type Paiement à la carte, utilisez la [commande](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_unset) `ibmcloud ks credential-unset --region <region>`. 

<br />


## Les commandes `kubectl` dépassent le délai d'attente
{: #exec_logs_fail}

{: tsSymptoms}
Si vous exécutez des commandes de type `kubectl exec`, `kubectl attach`, `kubectl proxy`, `kubectl port-forward` ou `kubectl logs`, vous voyez s'afficher le message suivant.

  ```
  <workerIP>:10250: getsockopt: connection timed out
  ```
  {: screen}

{: tsCauses}
La connexion OpenVPN entre le noeud maître et les noeuds worker ne fonctionne pas correctement.

{: tsResolve}
1. Si vous disposez de plusieurs VLAN pour un cluster, de plusieurs sous-réseaux sur le même VLAN ou d'un cluster à zones multiples, vous devez activer une fonction [VRF (Virtual Router Function)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) pour votre compte d'infrastructure IBM Cloud (SoftLayer) pour que vos noeuds worker puissent communiquer entre eux sur le réseau privé. Pour activer la fonction VRF, [contactez le représentant de votre compte d'infrastructure IBM Cloud (SoftLayer)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#how-you-can-initiate-the-conversion). Si vous ne parvenez pas à activer la fonction VRF ou si vous ne souhaitez pas le faire, activez la fonction [Spanning VLAN](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning). Pour effectuer cette action, vous devez disposer du [droit d'infrastructure](/docs/containers?topic=containers-users#infra_access) **Réseau > Gérer le spanning VLAN pour réseau**, ou vous pouvez demander au propriétaire du compte de l'activer. Pour vérifier si le spanning VLAN est déjà activé, utilisez la [commande](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get) `ibmcloud ks vlan-spanning-get --region <region>`. 
2. Redémarrez le pod du client OpenVPN.
  ```
  kubectl delete pod -n kube-system -l app=vpn
  ```
  {: pre}
3. Si le message d'erreur s'affiche toujours, le noeud worker sur lequel réside le pod VPN est peut-être défectueux. Pour redémarrer le pod VPN et le replanifier sur un autre noeud worker, utilisez les [commandes cordon, drain et réamorcez le noeud worker](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reboot) sur lequel se trouve le pod VPN.

<br />


## La liaison d'un service à un cluster renvoie une erreur due à l'utilisation du même nom
{: #cs_duplicate_services}

{: tsSymptoms}
Lorsque vous exécutez `ibmcloud ks cluster-service-bind --cluster <cluster_name> --namespace <namespace> --service <service_instance_name>`, le message suivant s'affiche :

```
Multiple services with the same name were found.
Run 'ibmcloud service list' to view available Bluemix service instances...
```
{: screen}

{: tsCauses}
Il se peut que plusieurs instances de service aient le même nom dans différentes régions.

{: tsResolve}
Utilisez l'identificateur global unique (GUID) du service au lieu du nom de l'instance de service dans la commande `ibmcloud ks cluster-service-bind`.

1. [Connecte-vous à la région {{site.data.keyword.Bluemix_notm}} qui inclut l'instance de service à lier.](/docs/containers?topic=containers-regions-and-zones#bluemix_regions)

2. Extrayez l'identificateur global unique (GUID) de l'instance de service.
  ```
  ibmcloud service show <service_instance_name> --guid
  ```
  {: pre}

  Sortie :
  ```
  Invoking 'cf service <service_instance_name> --guid'...
  <service_instance_GUID>
  ```
  {: screen}
3. Liez à nouveau le service au cluster.
  ```
  ibmcloud ks cluster-service-bind --cluster <cluster_name> --namespace <namespace> --service <service_instance_GUID>
  ```
  {: pre}

<br />


## La liaison d'un service à un cluster renvoie une erreur indiquant que le service est introuvable
{: #cs_not_found_services}

{: tsSymptoms}
Lorsque vous exécutez `ibmcloud ks cluster-service-bind --cluster <cluster_name> --namespace <namespace> --service <service_instance_name>`, le message suivant s'affiche :

```
Binding service to a namespace...
FAILED

The specified IBM Cloud service could not be found. If you just created the service, wait a little and then try to bind it again. To view available IBM Cloud service instances, run 'ibmcloud service list'. (E0023)
```
{: screen}

{: tsCauses}
Pour lier des services à un cluster vous devez disposer du rôle utilisateur Développeur Cloud Foundry pour l'espace où l'instance de service est mise à disposition. Vous devez également disposer du rôle d'accès de plateforme {{site.data.keyword.Bluemix_notm}} IAM Editeur à {{site.data.keyword.containerlong}}. Pour accéder à l'instance de service, vous devez être connecté à l'espace dans lequel l'instance de service est mise à disposition.

{: tsResolve}

**En tant qu'utilisateur :**

1. Connectez-vous à {{site.data.keyword.Bluemix_notm}}.
   ```
   ibmcloud login
   ```
   {: pre}

2. Ciblez l'organisation et l'espace où l'instance de service est mise à disposition.
   ```
   ibmcloud target -o <org> -s <space>
   ```
   {: pre}

3. Vérifiez que vous êtes dans le bon espace en affichant la liste de vos instances de service.
   ```
   ibmcloud service list
   ```
   {: pre}

4. Essayez d'effectuer à nouveau la liaison du service. Si vous obtenez la même erreur, contactez l'administrateur du compte et vérifiez que vous disposez des droits suffisants pour effectuer des liaisons de services (voir la procédure suivante de l'administrateur du compte).

**En tant qu'administrateur du compte :**

1. Vérifiez que l'utilisateur qui rencontre ce problème dispose des [droits d'éditeur pour {{site.data.keyword.containerlong}}](/docs/iam?topic=iam-iammanidaccser#edit_existing).

2. Vérifiez que l'utilisateur qui rencontre ce problème dispose du [rôle de développeur Cloud Foundry pour l'espace](/docs/iam?topic=iam-mngcf#update_cf_access) dans lequel le service est mis à disposition.

3. Si les droits adéquats existent, essayez d'affecter un autre droit, puis d'affecter à nouveau le droit requis.

4. Patientez quelques minutes, puis laissez l'utilisateur effectuer une nouvelle tentative de liaison du service.

5. Si le problème n'est toujours pas résolu, les droits {{site.data.keyword.Bluemix_notm}} IAM ne sont pas synchronisés et vous ne pouvez pas résoudre le problème vous-même. [Contactez le support IBM](/docs/get-support?topic=get-support-getting-customer-support) en ouvrant un cas de support. Veillez à fournir l'ID du cluster, l'ID utilisateur et l'ID de l'instance de service.
   1. Récupérez l'ID du cluster.
      ```
      ibmcloud ks clusters
      ```
      {: pre}

   2. Récupérez l'ID de l'instance de service.
      ```
      ibmcloud service show <service_name> --guid
      ```
      {: pre}


<br />


## La liaison d'un service à un cluster renvoie une erreur indiquant que le service ne prend pas en charge les clés de service
{: #cs_service_keys}

{: tsSymptoms}
Lorsque vous exécutez `ibmcloud ks cluster-service-bind --cluster <cluster_name> --namespace <namespace> --service <service_instance_name>`, le message suivant s'affiche :

```
This service doesn't support creation of keys
```
{: screen}

{: tsCauses}
Certains services dans {{site.data.keyword.Bluemix_notm}}, tels qu'{{site.data.keyword.keymanagementservicelong}} ne prennent pas en charge la création de données d'identification de service, appelées également clés de service. Si les clés de service ne sont pas prises en charge, un service ne peut pas être lié à un cluster. Pour obtenir une liste de services prenant en charge la création de clés de service, voir [Utilisation des services {{site.data.keyword.Bluemix_notm}} avec des applications externes](/docs/resources?topic=resources-externalapp#externalapp).

{: tsResolve}
Pour intégrer des services qui ne prennent pas en charge les clés de service, vérifiez si ces services fournissent une API que vous pouvez utiliser pour accéder directement au service à partir de votre application. Par exemple, pour utiliser {{site.data.keyword.keymanagementservicelong}}, voir [Référence d'API ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/apidocs/kms?language=curl).

<br />


## Après la mise à jour ou le rechargement d'un noeud worker, des noeuds et des pods en double apparaissent
{: #cs_duplicate_nodes}

{: tsSymptoms}
Lorsque vous exécutez la commande `kubectl get nodes`, vous voyez des noeuds worker en double avec le statut **`NotReady`**. Les noeuds worker avec le statut **`NotReady`** ont des adresses IP publiques, alors que les noeuds worker avec le statut **`Ready`** ont des adresses IP privées.

{: tsCauses}
D'anciens clusters affichaient la liste des noeuds worker avec l'adresse IP publique de cluster. A présent, les noeuds worker sont répertoriés avec l'adresse IP privée du cluster. Lorsque vous rechargez ou mettez à jour un noeud, l'adresse IP est modifiée, mais la référence à l'adresse IP publique est conservée.

{: tsResolve}
Le service n'est pas interrompu en raison de ces doublons, mais vous pouvez supprimer les références aux anciens noeuds worker du serveur d'API.

  ```
  kubectl delete node <node_name1> <node_name2>
  ```
  {: pre}

<br />


## Echec de l'accès à un pod sur un nouveau noeud worker et dépassement du délai d'attente
{: #cs_nodes_duplicate_ip}

{: tsSymptoms}
Vous avez supprimé un noeud worker dans votre cluster et ajouté un noeud worker. Lorsque vous avez déployé un pod ou un service Kubernetes, la ressource n'a pas pu accéder au noeud worker nouvellement créé et un dépassement de délai d'attente s'est produit pour la connexion.

{: tsCauses}
Si vous supprimez un noeud worker de votre cluster et que vous ajoutez un noeud worker, il se peut que l'adresse IP privée du noeud worker supprimé soit affectée au nouveau noeud worker. Calico utilise cette adresse IP privée en tant que balise et continue d'essayer d'atteindre le noeud supprimé.

{: tsResolve}
Mettez manuellement à jour la référence de l'adresse IP privée pour qu'elle pointe vers le noeud approprié.

1.  Vérifiez que vous possédez deux noeuds worker associés à la même **adresse IP privée**. Notez l'**adresse IP privée** et l'**ID** du noeud worker supprimé.

  ```
  ibmcloud ks workers --cluster <cluster_name_or_id>
  ```
  {: pre}

  ```
  ID                                                 Public IP       Private IP       Machine Type   State     Status   Zone   Version
  kube-dal10-cr9b7371a7fcbe46d08e04f046d5e6d8b4-w1   169.xx.xxx.xxx  10.xxx.xx.xxx    b3c.4x16       normal    Ready    dal10      1.13.6
  kube-dal10-cr9b7371a7fcbe46d08e04f046d5e6d8b4-w2   169.xx.xxx.xxx  10.xxx.xx.xxx    b3c.4x16       deleted    -       dal10      1.13.6
  ```
  {: screen}

2.  Installez l'[interface de ligne de commande Calico](/docs/containers?topic=containers-network_policies#adding_network_policies).
3.  Répertoriez les noeuds worker disponibles dans Calico. Remplacez <path_to_file> par le chemin d'accès local au fichier de configuration Calico.

  ```
  calicoctl get nodes --config=filepath/calicoctl.cfg
  ```
  {: pre}

  ```
  NAME
  kube-dal10-cr9b7371a7faaa46d08e04f046d5e6d8b4-w1
  kube-dal10-cr9b7371a7faaa46d08e04f046d5e6d8b4-w2
  ```
  {: screen}

4.  Supprimez le noeud worker en double dans Calico. Remplacez NODE_ID par l'ID du noeud worker.

  ```
  calicoctl delete node NODE_ID --config=<path_to_file>/calicoctl.cfg
  ```
  {: pre}

5.  Réamorcez le noeud worker qui n'a pas été supprimé.

  ```
  ibmcloud ks worker-reboot --cluster <cluster_name_or_id> --worker <worker_id>
  ```
  {: pre}


Le noeud supprimé n'apparaît plus dans Calico.

<br />




## Les pods ne parviennent pas à se déployer en raison d'une politique de sécurité de pod
{: #cs_psp}

{: tsSymptoms}
Après avoir créé un pod ou exécuté la commande `kubectl get events` pour vérifier le déploiement d'un pod, un message d'erreur de ce type s'affiche.

```
unable to validate against any pod security policy
```
{: screen}

{: tsCauses}
[Le contrôleur d'admission `PodSecurityPolicy`](/docs/containers?topic=containers-psp) vérifie l'autorisation du compte utilisateur ou du compte de service, par exemple un déploiement ou Helm tiller, qui a essayé de créer le pod. Si aucune politique de sécurité de pod ne prend en charge ce compte utilisateur ou ce compte de service, le contrôleur d'admission `PodSecurityPolicy` empêche la création des pods.

Si vous avez supprimé une ressource de politique de sécurité de pod pour la [gestion de cluster {{site.data.keyword.IBM_notm}}](/docs/containers?topic=containers-psp#ibm_psp), vous pouvez expérimenter des erreurs similaires.

{: tsResolve}
Vérifiez que le compte utilisateur ou le compte de service est autorisé par une politique de sécurité de pod. Vous pouvez être amené à [modifier une politique existante](/docs/containers?topic=containers-psp#customize_psp).

Si vous avez supprimé une ressource de gestion de cluster {{site.data.keyword.IBM_notm}}, actualisez le maître Kubernetes pour la restaurer.

1.  [Connectez-vous à votre compte. Le cas échéant, ciblez le groupe de ressources approprié. Définissez le contexte pour votre cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
2.  Actualisez le maître Kubernetes pour restaurer la ressource.

    ```
    ibmcloud ks apiserver-refresh
    ```
    {: pre}


<br />




## Le cluster reste à l'état En attente
{: #cs_cluster_pending}

{: tsSymptoms}
Lorsque vous déployez votre cluster, il reste à l'état en attente (pending) et ne démarre pas.

{: tsCauses}
Si vous venez de créer le cluster, les noeuds worker sont peut-être encore en cours de configuration. Si vous patientez déjà depuis un bon moment, il se peut que votre réseau local virtuel (VLAN) soit non valide.

{: tsResolve}

Vous pouvez envisager l'une des solutions suivantes :
  - Vérifiez le statut de votre cluster en exécutant la commande `ibmcloud ks clusters`. Vérifiez ensuite que vos noeuds worker sont bien déployés en exécutant la commande `ibmcloud ks workers --cluster <cluster_name>`.
  - Vérifiez si votre réseau local virtuel (VLAN) est valide. Pour être valide, un VLAN doit être associé à une infrastructure pouvant héberger un noeud worker avec un stockage sur disque local. Vous pouvez [afficher la liste de vos VLAN](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlans) en exécutant la commande `ibmcloud ks vlans --zone <zone>`. Si le VLAN n'apparaît pas dans la liste, il n'est pas valide. Choisissez-en un autre.

<br />


## Erreur lors de la création de cluster : Impossible d'extraire l'image du registre
{: #ts_image_pull_create}

{: tsSymptoms}
Lorsque vous avez créé un cluster, vous avez reçu un message d'erreur semblable à celui présenté ci-dessous :


```
Votre cluster ne peut pas extraire d'images des domaines 'icr.io' IBM Cloud Container Registry car une règle d'accès IAM n'a pas pu être créée. Vérifiez que vous disposez du rôle de plateforme Administrateur IAM sur IBM Cloud Container Registry. Créez ensuite un secret d'extraction d'image avec les données d'identification IAM pour le registre en exécutant 'ibmcloud ks cluster-pull-secret-apply'.
```
{: screen}

{: tsCauses}
Durant la création de cluster, un ID de service est créé pour votre cluster et la règle d'accès de service **Lecteur** est affectée à {{site.data.keyword.registrylong_notm}}. Ensuite, une clé d'API pour cet ID de service est générée et stockée dans une [valeur confidentielle d'extraction d'image](/docs/containers?topic=containers-images#cluster_registry_auth) pour autoriser le cluster à extraire des images d'{{site.data.keyword.registrylong_notm}}.

Pour pouvoir affecter la règle d'accès de service **Lecteur** à l'ID de service durant la création de cluster, vous devez avoir la règle d'accès de plateforme **Administrateur** sur {{site.data.keyword.registrylong_notm}}.

{: tsResolve}

Etapes :
1.  Assurez-vous que le propriétaire de compte vous octroie le rôle **Administrateur** sur {{site.data.keyword.registrylong_notm}}.
    ```
    ibmcloud iam user-policy-create <your_user_email> --service-name container-registry --roles Administrator
    ```
    {: pre}
2.  [Utilisez la commande `ibmcloud ks cluster-pull-secret-apply` ](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_pull_secret_apply) pour recréer une valeur confidentielle d'extraction d'image avec les données d'identification de registre appropriées.

<br />


## Echec de l'extraction d'une image du registre avec `ImagePullBackOff` ou erreurs d'autorisation
{: #ts_image_pull}

{: tsSymptoms}

Lorsque vous déployez une charge de travail qui extrait une image d'{{site.data.keyword.registrylong_notm}}, vos pods échouent avec le statut **`ImagePullBackOff`**.

```
kubectl get pods
```
{: pre}

```
NAME         READY     STATUS             RESTARTS   AGE
<pod_name>   0/1       ImagePullBackOff   0          2m
```
{: screen}

Lorsque vous décrivez le pod, vous voyez des erreurs d'authentification de ce type :

```
kubectl describe pod <pod_name>
```
{: pre}

```
Failed to pull image "<region>.icr.io/<namespace>/<image>:<tag>" ... unauthorized: authentication required
Failed to pull image "<region>.icr.io/<namespace>/<image>:<tag>" ... 401 Unauthorized
```
{: screen}

```
Failed to pull image "registry.ng.bluemix.net/<namespace>/<image>:<tag>" ... unauthorized: authentication required
Failed to pull image "registry.ng.bluemix.net/<namespace>/<image>:<tag>" ... 401 Unauthorized
```
{: screen}

{: tsCauses}
Votre cluster utilise une clé d'API ou un jeton stocké dans une [valeur confidentielle d'extraction d'image (imagePullSecret)](/docs/containers?topic=containers-images#cluster_registry_auth) pour autoriser le cluster à extraire des images d'{{site.data.keyword.registrylong_notm}}. Par défaut, les nouveaux clusters ont des secrets d'extraction d'image qui utilisent des clés d'API pour que le cluster puisse extraire des images d'un registre `icr.io` régional pour les conteneurs déployés dans l'espace de nom Kubernetes `default`. Si le secret d'extraction d'image du cluster utilise un jeton, l'accès par défaut à {{site.data.keyword.registrylong_notm}} est limité à certains registres régionaux uniquement qui utilisent les domaines `<region>.registry.bluemix.net` dépréciés.

{: tsResolve}

1.  Vérifiez que vous utilisez le nom et l'étiquette d'image corrects dans votre fichier YAML de déploiement.
    ```
    ibmcloud cr images
    ```
    {: pre}
2.  Obtenez le fichier de configuration d'un pod à l'état d'échec et recherchez la section `imagePullSecrets`.
    ```
    kubectl get pod <pod_name> -o yaml
    ```
    {: pre}

    Exemple de sortie :
    ```
    ...
    imagePullSecrets:
    - name: bluemix-default-secret
    - name: bluemix-default-secret-regional
    - name: bluemix-default-secret-international
    - name: default-us-icr-io
    - name: default-uk-icr-io
    - name: default-de-icr-io
    - name: default-au-icr-io
    - name: default-jp-icr-io
    - name: default-icr-io
    ...
    ```
    {: screen}
3.  Si aucune valeur confidentielle (secret) d'extraction d'image n'est affichée dans la liste, configurez-en une dans votre espace de nom.
    1.  [Copiez les valeurs confidentielles d'extraction d'image de l'espace de nom Kubernetes `default` vers l'espace de nom dans lequel vous souhaitez déployer votre charge de travail](/docs/containers?topic=containers-images#copy_imagePullSecret).
    2.  [Ajoutez la valeur confidentielle d'extraction d'image dans le compte de service pour cet espace de nom Kubernetes](/docs/containers?topic=containers-images#store_imagePullSecret) pour que tous les pods dans l'espace de nom puissent utiliser les données d'identification de cette valeur confidentielle.
4.  Si des valeurs confidentielles d'extraction d'image sont répertoriées, déterminez le type de données d'identification que vous utilisez pour accéder au registre de conteneur.
    *   **Déprécié** : si la valeur confidentielle comporte `bluemix` dans le nom, vous utilisez un jeton de registre pour l'authentification avec les noms de domaine `registry.<region>.bluemix.net` dépréciés. Passez à la section [Traitement des incidents liés aux valeurs confidentielles d'extraction d'image utilisant des jetons](#ts_image_pull_token).
    *   Si la valeur confidentielle comporte `icr` dans le nom, vous utilisez une clé d'API pour l'authentification avec les noms de domaine `icr.io`. Passez à la section [Traitement des incidents liés aux valeurs confidentielles d'extraction d'image utilisant des clés d'API](#ts_image_pull_apikey).
    *   Si vous disposez de ces deux types de valeur confidentielle, vous utilisez les deux méthodes d'authentification. Utilisez désormais les noms de domaine `icr.io` dans vos fichiers YAML de déploiement correspondant à l'image de conteneur. Passez à la section [Traitement des incidents liés aux valeurs confidentielles d'extraction d'image utilisant des clés d'API](#ts_image_pull_apikey).

<br>
<br>

**Traitement des incidents liés aux secrets d'extraction d'image utilisant des clés d'API**</br>
{: #ts_image_pull_apikey}

Si votre configuration de pod contient une valeur confidentielle d'extraction d'image qui utilise une clé d'API, vérifiez que les données d'identification de la clé d'API sont configurées correctement.
{: shortdesc}

Les étapes suivantes considèrent que la clé d'API stocke les données d'identification d'un ID de service. Si vous avez configuré votre valeur confidentielle d'extraction d'image pour l'utilisation de la clé d'API d'un utilisateur individuel, vous devez vérifier les droits {{site.data.keyword.Bluemix_notm}} IAM et les données d'identification de cet utilisateur.
{: note}

1.  Recherchez l'ID de service utilisé par la clé d'API pour la valeur confidentielle d'extraction d'image en examinant la section **Description**. L'ID de service créé avec le cluster indique `ID for <cluster_name>` et est utilisé dans l'espace de nom Kubernetes `default`. Si vous avez créé un autre ID de service pour accéder à un autre espace de nom Kubernetes ou pour modifier des droits {{site.data.keyword.Bluemix_notm}} IAM, vous avez personnalisé la description.
    ```
    ibmcloud iam service-ids
    ```
    {: pre}

    Exemple de sortie :
    ```
    UUID                Name               Created At              Last Updated            Description                                                                                                                                                                                         Locked     
    ServiceId-aa11...   <service_ID_name>  2019-02-01T19:01+0000   2019-02-01T19:01+0000   ID for <cluster_name>                                                                                                                                         false   
    ServiceId-bb22...   <service_ID_name>  2019-02-01T19:01+0000   2019-02-01T19:01+0000   Service ID for IBM Cloud Container Registry in Kubernetes cluster <cluster_name> namespace <kube_namespace>                                                                                                                                         false    
    ```
    {: screen}
2.  Vérifiez que l'ID de service bénéficie au moins d'une [règle de rôle d'accès au service {{site.data.keyword.Bluemix_notm}} IAM **Lecteur** pour {{site.data.keyword.registryshort_notm}}](/docs/services/Registry?topic=registry-user#create). Si l'ID de service ne dispose pas du rôle de service **Lecteur**, [modifiez les règles IAM](/docs/iam?topic=iam-serviceidpolicy#access_edit). Si les règles sont correctes, passez à l'étape suivante pour voir si les données d'identification sont valides.
    ```
    ibmcloud iam service-policies <service_ID_name>
    ```
    {: pre}

    Exemple de sortie :
    ```              
    Policy ID:   a111a111-b22b-333c-d4dd-e555555555e5   
    Roles:       Reader   
    Resources:                            
                  Service Name       container-registry      
                  Service Instance         
                  Région                  
                  Resource Type      namespace      
                  Resource           <registry_namespace>  
    ```
    {: screen}  
3.  Vérifiez si les données d'identification de la valeur confidentielle d'extraction d'image sont valides.
    1.  Obtenez la configuration de la valeur confidentielle d'extraction d'image. Si le pod ne se trouve pas dans l'espace de nom `default`, incluez l'indicateur `-n`.
        ```
        kubectl get secret <image_pull_secret_name> -o yaml [-n <namespace>]
        ```
        {: pre}
    2.  Dans la sortie, copiez la valeur codée en base64 de la zone `.dockercfg`.
        ```
        apiVersion: v1
        kind: Secret
        data:
          .dockercfg: eyJyZWdp...==
        ...
        ```
        {: screen}
    3.  Décodez la chaîne en base64. Par exemple, sous OS X, vous pouvez exécuter la commande suivante.
        ```
        echo -n "<base64_string>" | base64 --decode
        ```
        {: pre}

        Exemple de sortie :
        ```
        {"auths":{"<region>.icr.io":{"username":"iamapikey","password":"<password_string>","email":"<name@abc.com>","auth":"<auth_string>"}}}
        ```
        {: screen}
    4.  Comparez le nom de domaine du registre régional de la valeur confidentielle d'extraction d'image au nom de domaine que vous avez spécifié dans l'image de conteneur. Par défaut, les nouveaux clusters ont des valeurs confidentielles d'extraction d'image pour chaque nom de domaine de registre régional pour les conteneurs qui s'exécutent dans l'espace de nom Kubernetes `default`. Cependant, si vous avez modifié les valeurs par défaut ou si vous utilisez un autre espace de nom Kubernetes, vous ne disposez pas forcément d'une valeur confidentielle d'extraction d'image pour le registre régional. [Copiez une valeur confidentielle d'extraction d'image](/docs/containers?topic=containers-images#copy_imagePullSecret) pour le nom de domaine du registre régional.
    5.  Connectez-vous au registre sur votre machine locale en utilisant le nom d'utilisateur (`username`) et le mot de passe (`password`) de votre valeur confidentielle d'extraction d'image. Si vous n'arrivez pas à vous connecter, il vous faudra peut-être corriger l'ID de service.
        ```
        docker login -u iamapikey -p <password_string> <region>.icr.io
        ```
        {: pre}
        1.  Créez à nouveau l'ID de service du cluster, les règles {{site.data.keyword.Bluemix_notm}} IAM, la clé d'API et des valeurs confidentielles d'extraction d'image pour les conteneurs qui s'exécutent dans les espaces de nom Kubernetes `default`.
            ```
            ibmcloud ks cluster-pull-secret-apply --cluster <cluster_name_or_ID>
            ```
            {: pre}
        2.  Créez à nouveau votre déploiement dans l'espace de nom Kubernetes `default`. Si vous voyez encore le message d'erreur d'autorisation, répétez les étapes 1 à 5 avec les nouvelles valeurs confidentielles d'extraction d'image. Si vous ne parvenez toujours pas à vous connecter, [contactez l'équipe IBM sur Slack ou ouvrez un cas de support {{site.data.keyword.Bluemix_notm}}](#clusters_getting_help).
    6.  Si la connexion est établie, extrayez une image en local. Si la commande échoue avec une erreur de type `access denied`, le compte du registre se trouve dans un autre compte {{site.data.keyword.Bluemix_notm}} que celui dans lequel réside votre cluster. [Créez une valeur confidentielle d'extraction d'image pour accéder aux images dans l'autre compte](/docs/containers?topic=containers-images#other_registry_accounts). Si vous pouvez extraire une image sur votre machine locale, votre clé d'API dispose des droits appropriés, mais la configuration d'API dans votre cluster n'est pas correcte. Vous ne pouvez pas résoudre ce problème. [Contactez l'équipe IBM sur Slack ou ouvrez un cas de support {{site.data.keyword.Bluemix_notm}}](#clusters_getting_help).
        ```
        docker pull <region>icr.io/<namespace>/<image>:<tag>
        ```
        {: pre}

<br>
<br>

**Déprécié : traitement des incidents liés aux secrets d'extraction d'image utilisant des jetons**</br>
{: #ts_image_pull_token}

Si votre configuration de pod contient un secret d'extraction d'image qui utilise un jeton, vérifiez que les données d'identification du jeton sont valides.
{: shortdesc}

Cette méthode consistant à utiliser un jeton pour autoriser l'accès du cluster à {{site.data.keyword.registrylong_notm}} est prise en charge pour les noms de domaine `registry.bluemix.net` mais elle est dépréciée. [Utilisez à la place la méthode de clé d'API](/docs/containers?topic=containers-images#cluster_registry_auth) pour autoriser l'accès au cluster aux nouveaux noms de domaine de registre `icr.io`.
{: deprecated}

1.  Obtenez la configuration de la valeur confidentielle d'extraction d'image. Si le pod ne se trouve pas dans l'espace de nom `default`, incluez l'indicateur `-n`.
    ```
    kubectl get secret <image_pull_secret_name> -o yaml [-n <namespace>]
    ```
    {: pre}
2.  Dans la sortie, copiez la valeur codée en base64 de la zone `.dockercfg`.
    ```
    apiVersion: v1
        kind: Secret
        data:
      .dockercfg: eyJyZWdp...==
    ...
    ```
    {: screen}
3.  Décodez la chaîne en base64. Par exemple, sous OS X, vous pouvez exécuter la commande suivante.
    ```
    echo -n "<base64_string>" | base64 --decode
    ```
    {: pre}

    Exemple de sortie :
    ```
    {"auths":{"registry.<region>.bluemix.net":{"username":"token","password":"<password_string>","email":"<name@abc.com>","auth":"<auth_string>"}}}
    ```
    {: screen}
4.  Comparez le nom de domaine du registre régional au nom de domaine que vous avez spécifié dans l'image de conteneur. Par exemple, si la valeur confidentielle d'extraction d'image autorise l'accès au domaine `registry.ng.bluemix.net` et que vous avez spécifié une image stockée dans `registry.eu-de.bluemix.net`, vous devez [créer un jeton à utiliser dans une valeur confidentielle d'extraction d'image](/docs/containers?topic=containers-images#token_other_regions_accounts) pour `registry.eu-de.bluemix.net`.
5.  Connectez-vous au registre sur votre machine locale en utilisant le nom d'utilisateur (`username`) et le mot de passe (`password`) de la valeur confidentielle d'extraction d'image. Si vous n'arrivez pas à vous connecter, le jeton n'est pas correct et vous ne pouvez pas résoudre vous-même le problème. [Contactez l'équipe IBM sur Slack ou ouvrez un cas de support {{site.data.keyword.Bluemix_notm}}](#clusters_getting_help).
    ```
    docker login -u token -p <password_string> registry.<region>.bluemix.net
    ```
    {: pre}
6.  Si la connexion est établie, extrayez une image en local. Si la commande échoue avec une erreur de type `access denied`, le compte du registre se trouve dans un autre compte {{site.data.keyword.Bluemix_notm}} que celui dans lequel réside votre cluster. [Créez une valeur confidentielle d'extraction d'image pour accéder aux images dans l'autre compte](/docs/containers?topic=containers-images#token_other_regions_accounts). Si la commande aboutit, [contactez l'équipe IBM sur Slack, ou ouvrez un cas de support {{site.data.keyword.Bluemix_notm}}](#clusters_getting_help).
    ```
    docker pull registry.<region>.bluemix.net/<namespace>/<image>:<tag>
    ```
    {: pre}

<br />


## Des pods sont toujours à l'état en attente
{: #cs_pods_pending}

{: tsSymptoms}
Lorsque vous exécutez `kubectl get pods`, vous constatez que des pods sont toujours à l'état en attente (**pending**).

{: tsCauses}
Si vous venez juste de créer le cluster Kubernetes, il se peut que les noeuds worker soient encore en phase de configuration.

S'il s'agit d'un cluster existant :
*  Il se peut que la capacité de votre cluster soit insuffisante pour déployer le pod.
*  Le pod peut avoir dépassé une limite ou une demande de ressources.

{: tsResolve}
Cette tâche nécessite le [rôle de plateforme **Administrateur**](/docs/containers?topic=containers-users#platform) d'{{site.data.keyword.Bluemix_notm}} IAM pour le cluster et le [rôle de service **Responsable**](/docs/containers?topic=containers-users#platform) pour tous les espaces de nom.

Si vous venez de créer le cluster Kubernetes, exécutez la commande suivante et attendez l'initialisation des noeuds worker.

```
kubectl get nodes
```
{: pre}

S'il s'agit d'un cluster existant, vérifiez sa capacité.

1.  Définissez le proxy avec le numéro de port par défaut.

  ```
  kubectl proxy
  ```
   {: pre}

2.  Ouvrez le tableau de bord Kubernetes.

  ```
  http://localhost:8001/ui
  ```
  {: pre}

3.  Vérifiez que vous disposez de suffisamment de capacité dans votre cluster pour déployer le pod.

4.  Si vous ne disposez pas d'une capacité suffisante dans votre cluster, redimensionnez votre pool de noeuds worker pour ajouter des noeuds supplémentaires.

    1.  Passez en revue les tailles et les types de machine de vos pools de noeuds worker pour décider du pool à redimensionner.

        ```
        ibmcloud ks worker-pools
        ```
        {: pre}

    2.  Redimensionnez votre pool de noeuds worker pour ajouter des noeuds supplémentaires dans toutes les zones couvertes par le pool.

        ```
        ibmcloud ks worker-pool-resize --worker-pool <worker_pool> --cluster <cluster_name_or_ID> --size-per-zone <workers_per_zone>
        ```
        {: pre}

5.  Facultatif : vérifiez les demandes de ressources de votre pod.

    1.  Confirmez que les valeurs des demandes de ressources (`resources.requests`) ne dépassent pas la capacité du noeud worker. Par exemple, si la demande du pod est `cpu: 4000m` ou 4 coeurs, mais que la taille du noeud worker n'est que de 2 coeurs, le pod ne peut pas être déployé.

        ```
        kubectl get pod <pod_name> -o yaml
        ```
        {: pre}

    2.  Si la demande dépasse la capacité disponible, [ajoutez un nouveau pool de noeuds worker](/docs/containers?topic=containers-add_workers#add_pool) avec des noeuds worker pouvant satisfaire la demande.

6.  Si vos pods sont toujours à l'état **pending** après le déploiement complet du noeud worker, consultez la [documentation Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/tasks/debug-application-cluster/debug-pod-replication-controller/#my-pod-stays-pending) pour effectuer d'autres tâches en vue d'identifier et de résoudre le problème.

<br />


## Les conteneurs ne démarrent pas
{: #containers_do_not_start}

{: tsSymptoms}
Les pods se déploient sur les clusters, mais les conteneurs ne démarrent pas.

{: tsCauses}
Il peut arriver que les conteneurs ne démarrent pas lorsque le quota de registre est atteint.

{: tsResolve}
[Libérez de l'espace de stockage dans {{site.data.keyword.registryshort_notm}}.](/docs/services/Registry?topic=registry-registry_quota#registry_quota_freeup)

<br />


## Les pods ne parviennent pas à redémarrer à plusieurs reprises ou sont retirés de manière imprévisible
{: #pods_fail}

{: tsSymptoms}
Votre pod est sain mais a été retiré de manière imprévisible ou est bloqué dans une boucle de redémarrage.

{: tsCauses}
Vos conteneurs ont peut-être dépassé leurs limites de ressources ou vos pods ont peut-être été remplacés par des pods avec une priorité plus élevée.

{: tsResolve}
Pour voir si un conteneur est arrêté en raison d'une limite de ressources :
<ol><li>Obtenez le nom de votre pod. Si vous avez utilisé un libellé (label), vous pouvez vous en servir pour filtrer vos résultats.<pre class="pre"><code>kubectl get pods --selector='app=wasliberty'</code></pre></li>
<li>Décrivez le pod et recherchez la zone **Nombre de redémarrages**.<pre class="pre"><code>kubectl describe pod</code></pre></li>
<li>Si le pod a redémarré plusieurs fois sur une courte durée, examinez son statut. <pre class="pre"><code>kubectl get pod -o go-template={{range.status.containerStatuses}}{{"Container Name: "}}{{.name}}{{"\r\nLastState: "}}{{.lastState}}{{end}}</code></pre></li>
<li>Passez en revue la raison. Par exemple, `OOM Killed` signifie "mémoire insuffisante", ce qui indique que l'incident du conteneur est lié à une limite de ressources.</li>
<li>Ajoutez de la capacité à votre cluster de sorte à l'ajuster par rapport aux ressources.</li></ol>

<br>

Pour voir si votre pod a été remplacé par des pods de priorité plus élevée :
1.  Obtenez le nom de votre pod.

    ```
    kubectl get pods
    ```
    {: pre}

2.  Décrivez le fichier YAML de votre pod.

    ```
    kubectl get pod <pod_name> -o yaml
    ```
    {: pre}

3.  Vérifiez la zone `priorityClassName`.

    1.  S'il n'y a aucune valeur de zone `priorityClassName`, votre pod a la classe de priorité `globalDefault`. Si l'administrateur de votre cluster n'a pas défini de classe de priorité `globalDefault`, la valeur par défaut est zéro (0) ou la priorité la plus faible. Tout pod avec une classe de priorité plus élevée peut préempter ou retirer votre pod.

    2.  S'il y a une valeur de zone `priorityClassName`, procurez-vous la classe de priorité.

        ```
        kubectl get priorityclass <priority_class_name> -o yaml
        ```
        {: pre}

    3.  Notez la zone `value` pour vérifier la priorité de votre pod.

4.  Répertoriez les classes de priorité existantes dans le cluster.

    ```
    kubectl get priorityclasses
    ```
    {: pre}

5.  Pour chaque classe de priorité, obtenez le fichier YAML et notez la zone `value`.

    ```
    kubectl get priorityclass <priority_class_name> -o yaml
    ```
    {: pre}

6.  Comparez la classe de priorité de votre pod avec les valeurs des autres classes de priorité pour voir si elle est supérieure ou inférieure aux autres.

7.  Répétez les étapes 1 à 3 pour les autres pods du cluster, pour vérifier la classe de priorité qu'ils utilisent. Si leur classe de priorité est supérieure à celle de votre pod, votre pod n'est pas mis à disposition, sauf s'il y a suffisamment de ressources pour votre pod et tous les pods de priorité plus élevée.

8.  Contactez l'administrateur de votre cluster pour ajouter de la capacité à votre cluster et confirmer que les classes de priorité adéquates sont affectées.

<br />


## Impossible d'installer une charte Helm avec les valeurs de configuration mises à jour
{: #cs_helm_install}

{: tsSymptoms}
Lorsque vous essayez d'installer une charte Helm en exécutant la commande `helm install -f config.yaml --namespace=kube-system --name=<release_name> ibm/<chart_name>`, le message d'erreur `Error: failed to download "ibm/<chart_name>"` s'affiche.

{: tsCauses}
L'adresse URL du référentiel {{site.data.keyword.Bluemix_notm}} dans votre instance Helm est peut-être incorrecte.

{: tsResolve}
Pour identifier et résoudre les problèmes liés à votre charte Helm :

1. Affichez la liste des référentiels actuellement disponibles dans votre instance Helm.

    ```
    helm repo list
    ```
    {: pre}

2. Dans la sortie, vérifiez que l'URL pour le référentiel {{site.data.keyword.Bluemix_notm}}, `ibm`, est `https://icr.io/helm/iks-charts`.

    ```
    NAME    URL
    stable  https://kubernetes-charts.storage.googleapis.com
    local   http://127.0.0.1:8888/charts
    ibm     https://icr.io/helm/iks-charts
    ```
    {: screen}

    * Si l'URL n'est pas correcte :

        1. Retirez le référentiel {{site.data.keyword.Bluemix_notm}}.

            ```
            helm repo remove ibm
            ```
            {: pre}

        2. Ajoutez à nouveau le référentiel {{site.data.keyword.Bluemix_notm}}.

            ```
            helm repo add iks-charts https://icr.io/helm/iks-charts
            ```
            {: pre}

    * Si l'URL est correcte, obtenez les dernières mises à jour du référentiel.

        ```
        helm repo update
        ```
        {: pre}

3. Installez la charte Helm avec vos mises à jour.

    ```
    helm install -f config.yaml --namespace=kube-system --name=<release_name> ibm/<chart_name>
    ```
    {: pre}

<br />


## Impossible d'installer le serveur Helm Tiller ou de déployer des conteneurs à partir d'images publiques dans mon cluster
{: #cs_tiller_install}

{: tsSymptoms}

Lorsque vous essayez d'installer le serveur Helm Tiller ou de déployer des images à partir de registres publics, tels que DockerHub, l'installation échoue avec une erreur du type suivant :

```
Failed to pull image "gcr.io/kubernetes-helm/tiller:v2.12.0": rpc error: code = Unknown desc = failed to resolve image "gcr.io/kubernetes-helm/tiller:v2.12.0": no available registry endpoint:
```
{: screen}

{: tsCauses}
Vous avez peut-être configuré un pare-feu personnalisé, indiqué des règles Calico personnalisées ou créé un cluster uniquement privé en utilisant le noeud final de service privé qui bloque la connectivité de réseau public au registre de conteneur dans lequel est stockée l'image.

{: tsResolve}
- Si vous disposez d'un pare-feu personnalisé ou si vous avez défini des règles Calico personnalisées, autorisez le trafic réseau entrant et sortant entre vos noeuds worker et le registre de conteneur dans lequel est stockée l'image. Si l'image est stockée dans {{site.data.keyword.registryshort_notm}}, consultez les ports requis indiqués dans [Autorisation accordée au cluster d'accéder aux ressources de l'infrastructure et à d'autres services](/docs/containers?topic=containers-firewall#firewall_outbound).
- Si vous avez créé un cluster privé en activant le noeud final de service privé uniquement, vous pouvez [activer le noeud final de service public](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_disable) pour votre cluster. Si vous souhaitez installer des chartes Helm dans un cluster privé sans ouvrir une connexion publique, vous pouvez installer Helm [avec Tiller](/docs/containers?topic=containers-helm#private_local_tiller) ou [sans Tiller](/docs/containers?topic=containers-helm#private_install_without_tiller).

<br />


## Aide et assistance
{: #clusters_getting_help}

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

