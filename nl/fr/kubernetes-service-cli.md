---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: kubernetes, iks, ibmcloud, ic, ks

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


# Interface CLI d'{{site.data.keyword.containerlong_notm}}
{: #kubernetes-service-cli}

Reportez-vous aux commandes suivantes pour créer et gérer des clusters Kubernetes dans {{site.data.keyword.containerlong}}.
{:shortdesc}

Pour installer le plug-in de l'interface CLI, voir [Installation de l'interface de ligne de commande](/docs/containers?topic=containers-cs_cli_install#cs_cli_install_steps).

Dans le terminal, vous êtes averti des mises à jour disponibles pour l'interface de ligne de commande `ibmcloud` et les plug-ins. Veillez à maintenir votre interface de ligne de commande à jour pour pouvoir utiliser l'ensemble des commandes et des indicateurs.

Vous recherchez des commandes `ibmcloud cr` ? Voir le [guide de référence de l'interface de ligne de commande {{site.data.keyword.registryshort_notm}}](/docs/services/Registry?topic=container-registry-cli-plugin-containerregcli). Vous recherchez des commandes `kubectl` ? Consultez la [documentation Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubectl.docs.kubernetes.io/).
{:tip}


## Utilisation du plug-in {{site.data.keyword.containerlong_notm}} bêta
{: #cs_beta}

Le plug-in {{site.data.keyword.containerlong_notm}} a fait l'objet d'une nouvelle conception disponible en version bêta. Ce plug-in {{site.data.keyword.containerlong_notm}} regroupe désormais les commandes en catégories et change leur structure pour remplacer les tirets par des espaces. En outre, à compter de la version bêta `0.3` (par défaut), une nouvelle [fonctionnalité de noeud final global](/docs/containers?topic=containers-regions-and-zones#endpoint) est disponible.
{: shortdesc}

Les versions bêta suivantes du plug-in {{site.data.keyword.containerlong_notm}} avec la nouvelle conception sont disponibles.
* Le comportement par défaut est `0.3`. Assurez-vous que votre plug-in {{site.data.keyword.containerlong_notm}} utilise la dernière version `0.3` en exécutant la commande `ibmcloud plugin update kubernetes-service`.
* Pour utiliser `0.4` ou `1.0`, affectez à la variable d'environnement `IKS_BETA_VERSION` la version bêta que vous souhaitez utiliser :
    ```
    export IKS_BETA_VERSION=<beta_version>
    ```
    {: pre}
* Pour utiliser la fonctionnalité de noeud final régional déprécié, vous devez affecter à la variable d'environnement `IKS_BETA_VERSION` la valeur `0.2` :
    ```
    export IKS_BETA_VERSION=0.2
    ```
    {: pre}

</br>
<table>
<caption>Versions bêta du plug-in {{site.data.keyword.containerlong_notm}} dans sa nouvelle conception</caption>
  <thead>
    <th>Version bêta</th>
    <th>Structure de sortie `ibmcloud ks help`</th>
    <th>Structure des commandes</th>
    <th>Fonctionnalité d'emplacement</th>
  </thead>
  <tbody>
    <tr>
      <td><code>0.2</code> (déprécié)</td>
      <td>Version existante : les commandes sont structurées avec des tirets et sont répertoriées par ordre alphabétique.</td>
      <td>Version existante et version bêta : vous pouvez exécuter les commandes structurées avec des tirets (`ibmcloud ks alb-cert-get`) ou en version bêta avec des espaces (`ibmcloud ks alb cert get`).</td>
      <td>Régional : [ciblez une région et utilisez un noeud final régional pour utiliser des ressources de cette région](/docs/containers?topic=containers-regions-and-zones#bluemix_regions).</td>
    </tr>
    <tr>
      <td><code>0.3</code> (par défaut)</td>
      <td>Version existante : les commandes sont structurées avec des tirets et sont répertoriées par ordre alphabétique.</td>
      <td>Version existante et version bêta : vous pouvez exécuter les commandes structurées avec des tirets (`ibmcloud ks alb-cert-get`) ou en version bêta avec des espaces (`ibmcloud ks alb cert get`).</td>
      <td>Global : [utilisez le noeud final global pour utiliser des ressources dans n'importe quel emplacement](/docs/containers?topic=containers-regions-and-zones#bluemix_regions).</td>
    </tr>
    <tr>
      <td><code>0.4</code></td>
      <td>Version bêta : les commandes sont structurées avec des espaces et répertoriées par catégories.</td>
      <td>Version existante et version bêta : vous pouvez exécuter les commandes structurées avec des tirets (`ibmcloud ks alb-cert-get`) ou en version bêta avec des espaces (`ibmcloud ks alb cert get`).</td>
      <td>Global : [utilisez le noeud final global pour utiliser des ressources dans n'importe quel emplacement](/docs/containers?topic=containers-regions-and-zones#bluemix_regions).</td>
    </tr>
    <tr>
      <td><code>1.0</code></td>
      <td>Version bêta : les commandes sont structurées avec des espaces et répertoriées par catégories.</td>
      <td>Version bêta : vous ne pouvez exécuter que les commandes structurées avec des espaces de la version bêta (`ibmcloud ks alb cert get`).</td>
      <td>Global : [utilisez le noeud final global pour utiliser des ressources dans n'importe quel emplacement](/docs/containers?topic=containers-regions-and-zones#bluemix_regions).</td>
    </tr>
  </tbody>
</table>

## Commandes ibmcloud ks
{: #cs_commands}

**Astuce :** pour identifier la version du plug-in {{site.data.keyword.containerlong_notm}}, exécutez la commande suivante :

```
ibmcloud plugin list
```
{: pre}

<table summary="Tableau des commandes d'API">
<caption>Commandes d'API</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Commandes d'API</th>
 </thead>
 <tbody>
  <tr>
    <td>[ibmcloud ks api](#cs_cli_api)</td>
    <td>[ibmcloud ks api-key-info](#cs_api_key_info)</td>
    <td>[ibmcloud ks api-key-reset](#cs_api_key_reset)</td>
    <td>[ibmcloud ks apiserver-config-get](#cs_apiserver_config_get)</td>
  </tr>
  <tr>
    <td>[ibmcloud ks apiserver-config-set](#cs_apiserver_config_set)</td>
    <td>[ibmcloud ks apiserver-config-unset](#cs_apiserver_config_unset)</td>
    <td>[ibmcloud ks apiserver-refresh](#cs_apiserver_refresh) (cluster-refresh)</td>
    <td></td>
 </tr>
</tbody>
</table>

<br>

<table summary="Tableau des commandes d'utilisation du plug-in de l'interface CLI">
<caption>Commandes d'utilisation du plug-in de l'interface CLI</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Commandes d'utilisation du plug-in de l'interface CLI</th>
 </thead>
 <tbody>
  <tr>
    <td>[ibmcloud ks help](#cs_help)</td>
    <td>[ibmcloud ks init](#cs_init)</td>
    <td>[ibmcloud ks messages](#cs_messages)</td>
    <td></td>
  </tr>
</tbody>
</table>

<br>

<table summary="Tableau des commandes de cluster : Gestion">
<caption>Commandes de cluster : commandes de gestion</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Commandes de cluster : Gestion</th>
 </thead>
 <tbody>
  <tr>
    <td>[ibmcloud ks addon-versions](#cs_addon_versions)</td>
    <td>[ibmcloud ks cluster-addon-disable](#cs_cluster_addon_disable)</td>
    <td>[ibmcloud ks cluster-addon-enable](#cs_cluster_addon_enable)</td>
    <td>[ibmcloud ks cluster-addons](#cs_cluster_addons)</td>
  </tr>
  <tr>
    <td>[ibmcloud ks cluster-config](#cs_cluster_config)</td>
    <td>[ibmcloud ks cluster-create](#cs_cluster_create)</td>
    <td>[ibmcloud ks cluster-feature-disable](#cs_cluster_feature_disable)</td>
    <td>[ibmcloud ks cluster-feature-enable](#cs_cluster_feature_enable)</td>
  </tr>
  <tr>
    <td>[ibmcloud ks cluster-get](#cs_cluster_get)</td>
    <td>[ibmcloud ks cluster-pull-secret-apply](#cs_cluster_pull_secret_apply)</td>
    <td>[ibmcloud ks cluster-rm](#cs_cluster_rm)</td>
    <td>[ibmcloud ks cluster-update](#cs_cluster_update)</td>
  </tr>
  <tr>
    <td>[ibmcloud ks clusters](#cs_clusters)</td>
    <td>Deprecated: [ibmcloud ks kube-versions](#cs_kube_versions)</td>
    <td>[ibmcloud ks versions](#cs_versions_command)</td>
    <td> </td>
  </tr>
</tbody>
</table>

<br>

<table summary="Tableau des commandes de cluster : Services et intégrations">
<caption>Commandes de cluster : commandes de services et intégrations</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Commandes de cluster : Services et intégrations</th>
 </thead>
 <tbody>
  <tr>
    <td>[ibmcloud ks cluster-service-bind](#cs_cluster_service_bind)</td>
    <td>[ibmcloud ks cluster-service-unbind](#cs_cluster_service_unbind)</td>
    <td>[ibmcloud ks cluster-services](#cs_cluster_services)</td>
    <td>[ibmcloud ks va](#cs_va)</td>
  </tr>
    <td>Version bêta : [ibmcloud ks key-protect-enable](#cs_key_protect)</td>
    <td>[ibmcloud ks webhook-create](#cs_webhook_create)</td>
    <td> </td>
    <td> </td>
  <tr>
  </tr>
</tbody>
</table>

</br>

<table summary="Tableau des commandes de cluster : Sous-réseaux">
<caption>Commandes de cluster : commandes de sous-réseaux</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Commandes de cluster : Sous-réseaux</th>
 </thead>
 <tbody>
  <tr>
    <td>[ibmcloud ks cluster-subnet-add](#cs_cluster_subnet_add)</td>
    <td>[ibmcloud ks cluster-subnet-create](#cs_cluster_subnet_create)</td>
    <td>[ibmcloud ks cluster-user-subnet-add](#cs_cluster_user_subnet_add)</td>
    <td>[ibmcloud ks cluster-user-subnet-rm](#cs_cluster_user_subnet_rm)</td>
  </tr>
  <tr>
    <td>[ibmcloud ks subnets](#cs_subnets)</td>
    <td> </td>
    <td> </td>
    <td> </td>
  </tr>
</tbody>
</table>

</br>

<table summary="Tableau des commandes de l'infrastructure">
<caption>Commandes de cluster : commandes de l'infrastructure</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Commandes de l'infrastructure</th>
 </thead>
 <tbody>
   <tr>
     <td>[        ibmcloud ks credential-get
        ](#cs_credential_get)</td>
     <td>[ibmcloud ks credential-set](#cs_credentials_set)</td>
     <td>[ibmcloud ks credential-unset](#cs_credentials_unset)</td>
     <td>[ibmcloud ks infra-permissions-get](#infra_permissions_get)</td>
   </tr>
   <tr>
     <td>[ibmcloud ks machine-types](#cs_machine_types)</td>
     <td>[ibmcloud ks vlan-spanning-get](#cs_vlan_spanning_get)</td>
     <td>[ibmcloud ks vlans](#cs_vlans)</td>
     <td> </td>
   </tr>
</tbody>
</table>

</br>

<table summary="Tableau des commandes de l'équilibreur de charge d'application (ALB) Ingress">
<caption>Commandes de l'équilibreur de charge d'application (ALB) Ingress</caption>
<col width = 25%>
<col width = 25%>
<col width = 25%>
  <thead>
    <tr>
      <th colspan=4>Commandes de l'équilibreur de charge d'application (ALB) Ingress</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>[ibmcloud ks alb-autoupdate-disable](#cs_alb_autoupdate_disable)</td>
      <td>[ibmcloud ks alb-autoupdate-enable](#cs_alb_autoupdate_enable)</td>
      <td>[ibmcloud ks alb-autoupdate-get](#cs_alb_autoupdate_get)</td>
      <td>Version bêta : [ibmcloud ks alb-cert-deploy](#cs_alb_cert_deploy)</td>
    </tr>
    <tr>
      <td>Version bêta : [ibmcloud ks alb-cert-get](#cs_alb_cert_get)</td>
      <td>Version bêta : [ibmcloud ks alb-cert-rm](#cs_alb_cert_rm)</td>
      <td>[ibmcloud ks alb-certs](#cs_alb_certs)</td>
      <td>[ibmcloud ks alb-configure](#cs_alb_configure)</td>
    </tr>
    <tr>
      <td>[ibmcloud ks alb-create](#cs_alb_create)</td>
      <td>[ibmcloud ks alb-get](#cs_alb_get)</td>
      <td>[ibmcloud ks alb-rollback](#cs_alb_rollback)</td>
      <td>[ibmcloud ks alb-types](#cs_alb_types)</td>
    </tr>
    <tr>
      <td>[ibmcloud ks alb-update](#cs_alb_update)</td>
      <td>[ibmcloud ks albs](#cs_albs)</td>
      <td> </td>
      <td> </td>
    </tr>
  </tbody>
</table>

</br>

<table summary="Tableau des commandes de consignation">
<caption>Commandes de consignation</caption>
<col width = 25%>
<col width = 25%>
<col width = 25%>
  <thead>
    <tr>
      <th colspan=4>Commandes de consignation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>[ibmcloud ks logging-autoupdate-enable](#cs_log_autoupdate_enable)</td>
      <td>[ibmcloud ks logging-autoupdate-disable](#cs_log_autoupdate_disable)</td>
      <td>[ibmcloud ks logging-autoupdate-get](#cs_log_autoupdate_get)</td>
      <td>[ibmcloud ks logging-collect](#cs_log_collect)</td>
    </tr>
    <tr>
      <td>[ibmcloud ks logging-collect-status](#cs_log_collect_status)</td>
      <td>[ibmcloud ks logging-config-create](#cs_logging_create)</td>
      <td>[ibmcloud ks logging-config-get](#cs_logging_get)</td>
      <td>[ibmcloud ks logging-config-refresh](#cs_logging_refresh)</td>
    </tr>
    <tr>
      <td>[ibmcloud ks logging-config-rm](#cs_logging_rm)</td>
      <td>[ibmcloud ks logging-config-update](#cs_logging_update)</td>
      <td>[ibmcloud ks logging-filter-create](#cs_log_filter_create)</td>
      <td>[ibmcloud ks logging-filter-update](#cs_log_filter_update)</td>
    </tr>
    <tr>
      <td>[ibmcloud ks logging-filter-get](#cs_log_filter_view)</td>
      <td>[ibmcloud ks logging-filter-rm](#cs_log_filter_delete)</td>
      <td> </td>
      <td> </td>
    </tr>
  </tbody>
</table>

</br>

<table summary="Tableau des commandes d'équilibreur de charge de réseau (NLB)">
<caption>Commandes d'équilibreur de charge de réseau (NLB)</caption>
<col width = 25%>
<col width = 25%>
<col width = 25%>
  <thead>
    <tr>
      <th colspan=4>Commandes d'équilibreur de charge de réseau (NLB)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>[ibmcloud ks nlb-dns-add](#cs_nlb-dns-add)</td>
      <td>[ibmcloud ks nlb-dns-create](#cs_nlb-dns-create)</td>
      <td>[ibmcloud ks nlb-dns-rm](#cs_nlb-dns-rm)</td>
      <td>[ibmcloud ks nlb-dnss](#cs_nlb-dns-ls)</td>
    </tr>
    <tr>
      <td>[ibmcloud ks nlb-dns-monitor-configure](#cs_nlb-dns-monitor-configure)</td>
      <td>[ibmcloud ks nlb-dns-monitor-disable](#cs_nlb-dns-monitor-disable)</td>
      <td>[ibmcloud ks nlb-dns-monitor-enable](#cs_nlb-dns-monitor-enable)</td>
      <td>[ibmcloud ks nlb-dns-monitor-get](#cs_nlb-dns-monitor-get)</td>
    </tr>
    <tr>
      <td>[ibmcloud ks nlb-dns-monitor-status](#cs_nlb-dns-monitor-status)</td>
      <td>[ibmcloud ks nlb-dns-monitors](#cs_nlb-dns-monitor-ls)</td>
      <td> </td>
      <td> </td>
    </tr>
  </tbody>
</table>

</br>

<table summary="Tableau des commandes de région">
<caption>Commandes de région</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Commandes de région et d'emplacement</th>
 </thead>
 <tbody>
  <tr>
    <td>Déprécié : [ibmcloud ks region-get](#cs_region)</td>
    <td>Déprécié : [ibmcloud ks region-set](#cs_region-set)</td>
    <td>Déprécié : [ibmcloud ks regions](#cs_regions)</td>
    <td>[ibmcloud ks supported-locations](#cs_supported-locations)</td>
  </tr>
  <tr>
    <td>[ibmcloud ks zones](#cs_datacenters)</td>
    <td> </td>
    <td> </td>
    <td> </td>
  </tr>
</tbody>
</table>

</br>


<table summary="Tableau des commandes de noeud worker">
<caption>Commandes de noeud worker</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Commandes de noeud worker</th>
 </thead>
 <tbody>
    <tr>
      <td>Déprécié : [ibmcloud ks worker-add](#cs_worker_add)</td>
      <td>[ibmcloud ks worker-get](#cs_worker_get)</td>
      <td>[ibmcloud ks worker-reboot](#cs_worker_reboot)</td>
      <td>[ibmcloud ks worker-reload](#cs_worker_reload)</td>
    </tr>
    <tr>
      <td>[ibmcloud ks worker-rm](#cs_worker_rm)</td>
      <td>[ibmcloud ks worker-update](#cs_worker_update)</td>
      <td>[ibmcloud ks workers](#cs_workers)</td>
      <td> </td>
    </tr>
  </tbody>
</table>

</br>

<table summary="Tableau des commandes de pool de noeuds worker">
<caption>Commandes de pool de noeuds worker</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Commandes de pool de noeuds worker</th>
 </thead>
 <tbody>
    <tr>
      <td>[ibmcloud ks worker-pool-create](#cs_worker_pool_create)</td>
      <td>[ibmcloud ks worker-pool-get](#cs_worker_pool_get)</td>
      <td>[ibmcloud ks worker-pool-rebalance](#cs_rebalance)</td>
      <td>[ibmcloud ks worker-pool-resize](#cs_worker_pool_resize)</td>
    </tr>
    <tr>
      <td>[ibmcloud ks worker-pool-rm](#cs_worker_pool_rm)</td>
      <td>[ibmcloud ks worker-pools](#cs_worker_pools)</td>
      <td>[ibmcloud ks zone-add](#cs_zone_add)</td>
      <td>[ibmcloud ks zone-network-set](#cs_zone_network_set)</td>
    </tr>
    <tr>
      <td>[ibmcloud ks zone-rm](#cs_zone_rm)</td>
      <td> </td>
      <td> </td>
      <td> </td>
    </tr>
  </tbody>
</table>

## Commandes d'API
{: #api_commands}

</br>
### ibmcloud ks api
{: #cs_cli_api}

Permet de cibler le noeud final d'API pour {{site.data.keyword.containerlong_notm}}. Si vous n'indiquez pas de noeud final, vous pourrez voir les informations sur le noeud final actuellement ciblé.
{: shortdesc}

Les noeuds finaux propres à une région sont dépréciés. Utilisez à la place le [noeud final global](/docs/containers?topic=containers-regions-and-zones#endpoint). Si vous devez utiliser des noeuds finaux régionaux, [affectez à la variable d'environnement `IKS_BETA_VERSION` dans le plug-in {{site.data.keyword.containerlong_notm}} la valeur `0.2`](/docs/containers-cli-plugin?topic=containers-cli-plugin-kubernetes-service-cli#cs_beta).
{: deprecated}

Si vous devez répertorier et gérer les ressources d'une seule région, vous pouvez utiliser la commande `ibmcloud ks api` pour cibler un noeud final régional à la place du noeud final global. 
* Dallas (Sud des Etats-Unis, us-south) : `https://us-south.containers.cloud.ibm.com`
* Francfort (Centre de l'UE, eu-de) : `https://eu-de.containers.cloud.ibm.com`
* Londres (Sud du Royaume-Uni, eu-gb) : `https://eu-gb.containers.cloud.ibm.com`
* Sydney (Asie-Pacifique sud, au-syd) : `https://au-syd.containers.cloud.ibm.com`
* Tokyo (Asie-Pacifique nord, jp-tok) : `https://jp-tok.containers.cloud.ibm.com`
* Washington, D.C. (Est des Etats-Unis, us-east) : `https://us-east.containers.cloud.ibm.com`

Pour utiliser la fonctionnalité globale, vous pouvez réutiliser la commande `ibmcloud ks api` pour cible le noeud final global : `https://containers.cloud.ibm.com`

```
ibmcloud ks api --endpoint ENDPOINT [--insecure] [--skip-ssl-validation] [--api-version VALUE] [-s]
```
{: pre}

**Droits minimum requis** : aucun

**Options de commande** :
<dl>
<dt><code>--endpoint <em>ENDPOINT</em></code></dt>
<dd>Noeud final d'API {{site.data.keyword.containerlong_notm}}. Notez que ce noeud final est différent des noeuds finaux {{site.data.keyword.Bluemix_notm}}. Cette valeur est obligatoire pour définir le noeud final d'API.
   </dd>

<dt><code>--insecure</code></dt>
<dd>Autoriser une connexion HTTP non sécurisée. Cet indicateur est facultatif.</dd>

<dt><code>--skip-ssl-validation</code></dt>
<dd>Autoriser les certificats SSL non sécurisés. Cet indicateur est facultatif.</dd>

<dt><code>--api-version VALUE</code></dt>
<dd>Indiquez la version d'API du service que vous souhaitez utiliser. Cette valeur est facultative.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>

</dl>

**Exemple** : Affichage d'informations sur le noeud final d'API actuellement ciblé.
```
ibmcloud ks api
```
{: pre}

```
API Endpoint:          https://containers.cloud.ibm.com
API Version:           v1
Skip SSL Validation:   false
Region:                us-south
```
{: screen}

</br>
### ibmcloud ks api-key-info
{: #cs_api_key_info}

Permet d'afficher le nom et l'adresse e-mail du propriétaire de la clé d'API {{site.data.keyword.Bluemix_notm}} IAM (Identity and Access Management) dans un groupe de ressources {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

La clé d'API {{site.data.keyword.Bluemix_notm}} IAM est définie automatiquement pour un groupe de ressources et une région lorsque la première action qui nécessite la règle d'accès admin {{site.data.keyword.containerlong_notm}} est effectuée. Par exemple, supposons que l'un de vos administrateurs crée le premier cluster dans le groupe de ressources `default` dans la région `us-south`. Pour cette opération, la clé d'API {{site.data.keyword.Bluemix_notm}} IAM de cet utilisateur est stockée dans le compte correspondant à ce groupe de ressources et à cette région. La clé d'API est utilisée pour commander des ressources dans l'infrastructure IBM Cloud (SoftLayer), par exemple de nouveaux noeuds worker ou réseaux locaux virtuels (VLAN). Une autre clé d'API peut être définie pour chaque région au sein d'un groupe de ressources.

Lorsqu'un autre utilisateur effectue une action qui nécessite une interaction avec le portefeuille d'infrastructure IBM Cloud (SoftLayer) dans ce groupe de ressources et dans cette région, par exemple la création d'un nouveau cluster ou le rechargement d'un noeud worker, la clé d'API stockée est utilisée pour déterminer s'il existe des droits suffisants pour effectuer cette action. Pour vous assurer que les actions liées à l'infrastructure dans votre cluster peuvent être effectuées sans problème, affectez à vos administrateurs {{site.data.keyword.containerlong_notm}} la règle d'accès **Superutilisateur** de l'infrastructure. Pour plus d'informations, voir [Gestion de l'accès utilisateur](/docs/containers?topic=containers-users#infra_access).

Si vous constatez que la clé d'API stockée pour un groupe de ressources ou une région nécessite une mise à jour, vous pouvez le faire en exécutant la commande [ibmcloud ks api-key-reset](#cs_api_key_reset). Cette commande nécessite la règle d'accès admin {{site.data.keyword.containerlong_notm}} et stocke la clé d'API de l'utilisateur qui exécute cette commande dans le compte.

**Astuce :** la clé d'API renvoyée par cette commande ne peut pas être utilisée si les données d'identification ont été définies manuellement à l'aide de la commande [ibmcloud ks credential-set](#cs_credentials_set).

```
ibmcloud ks api-key-info --cluster CLUSTER [--json] [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Afficheur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

<dt><code>--json</code></dt>
<dd>Imprime le résultat de la commande au format JSON. Cette valeur est facultative.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>

</dl>

**Exemple** :
```
ibmcloud ks api-key-info --cluster my_cluster
```
{: pre}

</br>
### ibmcloud ks api-key-reset
{: #cs_api_key_reset}

Permet de remplacer la clé d'API {{site.data.keyword.Bluemix_notm}} IAM en cours dans un groupe de ressources {{site.data.keyword.Bluemix_notm}} et une région {{site.data.keyword.containershort_notm}}.
{: shortdesc}

Cette commande nécessite la règle d'accès admin {{site.data.keyword.containerlong_notm}} et stocke la clé d'API de l'utilisateur qui exécute cette commande dans le compte. La clé d'API {{site.data.keyword.Bluemix_notm}} IAM est nécessaire pour commander l'infrastructure depuis le portefeuille d'infrastructure IBM Cloud (SoftLayer). Une fois stockée, la clé d'API est utilisée pour toutes les actions dans une région qui nécessite des droits d'accès à l'infrastructure indépendamment de l'utilisateur qui exécute cette commande. Pour plus d'informations sur le mode de fonctionnement des clés d'API {{site.data.keyword.Bluemix_notm}} IAM, voir la [commande `ibmcloud ks api-key-info`](#cs_api_key_info).

Avant d'utiliser cette commande, assurez-vous que l'utilisateur qui l'exécute dispose des [droits {{site.data.keyword.containerlong_notm}} et des droits d'infrastructure IBM Cloud (SoftLayer)](/docs/containers?topic=containers-users#users) requis. Ciblez le groupe de ressources et la région pour lesquels vous voulez définir la clé d'API.
{: important}

```
ibmcloud ks api-key-reset --region REGION [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Administrateur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--region <em>REGION</em></code></dt>
<dd>Indiquez une région. Pour afficher la liste des régions disponibles, exécutez la commande <code>ibmcloud ks regions</code>.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks api-key-reset --region us-south
```
{: pre}

</br>
### ibmcloud ks apiserver-config-get audit-webhook
{: #cs_apiserver_config_get}

Permet d'afficher l'URL du service de consignation distant auquel vous envoyez les journaux d'audit de serveur d'API. L'URL a été spécifiée lorsque vous avez créé le back end du webhook pour la configuration de serveur d'API.
{: shortdesc}

```
ibmcloud ks apiserver-config-get audit-webhook --cluster CLUSTER
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Afficheur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>
</dl>
</br>
### ibmcloud ks apiserver-config-set audit-webhook
{: #cs_apiserver_config_set}

Définissez le back end du webhook pour la configuration de serveur d'API. Le back end du webhook achemine les journaux d'audit de serveur d'API à un serveur distant. Une configuration webhook est créée compte tenu des informations que vous soumettez dans les indicateurs de cette commande. Si vous ne soumettez pas d'informations dans les indicateurs, une configuration webhook par défaut est utilisée.
{: shortdesc}

Après avoir défini le webhook, vous devez exécuter la commande `ibmcloud ks apiserver-refresh` pour appliquer les modifications au maître Kubernetes.
{: note}

```
ibmcloud ks apiserver-config-set audit-webhook --cluster CLUSTER [--remoteServer SERVER_URL_OR_IP] [--caCert CA_CERT_PATH] [--clientCert CLIENT_CERT_PATH] [--clientKey CLIENT_KEY_PATH]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Editeur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

<dt><code>--remoteServer <em>SERVER_URL_OR_IP</em></code></dt>
<dd>URL ou adresse IP du service de consignation distant auquel vous désirez envoyer les journaux d'audit. Si vous indiquez une URL de serveur non sécurisée, les certificats éventuels sont ignorés. Cette valeur est facultative.</dd>

<dt><code>--caCert <em>CA_CERT_PATH</em></code></dt>
<dd>Chemin de fichier du certificat de l'autorité de certification utilisé pour vérifier le service de consignation distant. Cette valeur est facultative.</dd>

<dt><code>--clientCert <em>CLIENT_CERT_PATH</em></code></dt>
<dd>Chemin de fichier du certificat client utilisé pour l'authentification auprès du service de consignation distant. Cette valeur est facultative.</dd>

<dt><code>--clientKey <em> CLIENT_KEY_PATH</em></code></dt>
<dd>Chemin de fichier de la clé du client correspondant utilisée pour la connexion au service de consignation distant. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks apiserver-config-set audit-webhook --cluster my_cluster --remoteServer https://audit.example.com/audit --caCert /mnt/etc/kubernetes/apiserver audit/ca.pem --clientCert /mnt/etc/kubernetes/apiserver audit/cert.pem --clientKey /mnt/etc/kubernetes/apiserver audit/key.pem
```
{: pre}

</br>
### ibmcloud ks apiserver-config-unset audit-webhook
{: #cs_apiserver_config_unset}

Désactivez la configuration de back end du webhook pour le serveur d'API du cluster. Cette opération interrompt le transfert des journaux d'audit du serveur d'API vers un serveur distant.
{: shortdesc}

```
ibmcloud ks apiserver-config-unset audit-webhook --cluster CLUSTER
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Editeur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>
</dl>

</br>
### ibmcloud ks apiserver-refresh (cluster-refresh)
{: #cs_apiserver_refresh}

Appliquez les modifications de configuration du maître Kubernetes demandées avec les commandes `ibmcloud ks apiserver-config-set`, `apiserver-config-unset`, `cluster-feature-enable` ou `cluster-feature-disable`. Les composants du maître Kubernetes hautement disponibles sont redémarrés au cours d'un redémarrage séquentiel. Vos noeuds worker, applications et ressources ne sont pas modifiés et continuent à s'exécuter.
{: shortdesc}

```
ibmcloud ks apiserver-refresh --cluster CLUSTER [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Opérateur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

<br />


## Commandes d'utilisation du plug-in de l'interface CLI
{: #cli_plug-in_commands}

### ibmcloud ks help
{: #cs_help}

Affichez une liste de commandes et paramètres pris en charge.
{: shortdesc}

```
ibmcloud ks help
```
{: pre}

**Droits minimum requis** : aucun

**Options de commande** : aucune

</br>
### ibmcloud ks init
{: #cs_init}

Initialisez le plug-in {{site.data.keyword.containerlong_notm}} ou spécifiez la région dans laquelle vous souhaitez créer ou accéder à des clusters Kubernetes.
{: shortdesc}

Les noeuds finaux propres à une région sont dépréciés. Utilisez à la place le [noeud final global](/docs/containers?topic=containers-regions-and-zones#endpoint). Si vous devez utiliser des noeuds finaux régionaux, [affectez à la variable d'environnement `IKS_BETA_VERSION` dans le plug-in {{site.data.keyword.containerlong_notm}} la valeur `0.2`](/docs/containers-cli-plugin?topic=containers-cli-plugin-kubernetes-service-cli#cs_beta).
{: deprecated}

Si vous devez répertorier et gérer les ressources d'une seule région, vous pouvez utiliser la commande `ibmcloud ks init` pour cibler un noeud final régional à la place du noeud final global. 

* Dallas (Sud des Etats-Unis, us-south) : `https://us-south.containers.cloud.ibm.com`
* Francfort (Centre de l'UE, eu-de) : `https://eu-de.containers.cloud.ibm.com`
* Londres (Sud du Royaume-Uni, eu-gb) : `https://eu-gb.containers.cloud.ibm.com`
* Sydney (Asie-Pacifique sud, au-syd) : `https://au-syd.containers.cloud.ibm.com`
* Tokyo (Asie-Pacifique nord, jp-tok) : `https://jp-tok.containers.cloud.ibm.com`
* Washington, D.C. (Est des Etats-Unis, us-east) : `https://us-east.containers.cloud.ibm.com`

Pour utiliser la fonctionnalité globale, vous pouvez réutiliser la commande `ibmcloud ks init` pour cible le noeud final global : `https://containers.cloud.ibm.com`

```
ibmcloud ks init [--host HOST] [--insecure] [-p] [-u] [-s]
```
{: pre}

**Droits minimum requis** : aucun

**Options de commande** :
<dl>
<dt><code>--host <em>HOST</em></code></dt>
<dd>Noeud final d'API {{site.data.keyword.containerlong_notm}} à utiliser. Cette valeur est facultative.</dd>

<dt><code>--insecure</code></dt>
<dd>Autoriser une connexion HTTP non sécurisée.</dd>

<dt><code>-p</code></dt>
<dd>Votre mot de passe IBM Cloud.</dd>

<dt><code>-u</code></dt>
<dd>Votre nom d'utilisateur IBM Cloud.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>

</dl>

**Exemples** :
*  Exemple relatif au ciblage du noeud final régional du Sud des Etats-Unis :
  ```
  ibmcloud ks init --host https://us-south.containers.cloud.ibm.com
  ```
  {: pre}
*  Exemple relatif au nouveau ciblage du noeud final global :
  ```
  ibmcloud ks init --host https://containers.cloud.ibm.com
  ```
  {: pre}
</br>
### ibmcloud ks messages
{: #cs_messages}

Permet d'afficher les messages en cours du plug-in CLI {{site.data.keyword.containerlong_notm}} pour l'utilisateur IBMid.
{: shortdesc}

```
ibmcloud ks messages
```
{: pre}

**Droits minimum requis** : aucun

<br />


## Commandes de cluster : Gestion
{: #cluster_mgmt_commands}

### ibmcloud ks addon-versions
{: #cs_addon_versions}

Afficher la liste des versions prises en charge pour les modules complémentaires gérés dans {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

```
ibmcloud ks addon-versions [--addon ADD-ON_NAME] [--json] [-s]
```
{: pre}

**Droits minimum requis** : aucun

**Options de commande** :
<dl>
<dt><code>--addon <em>ADD-ON_NAME</em></code></dt>
<dd>Facultatif : indiquez un nom de module complémentaire, par exemple, <code>istio</code> ou <code>knative</code>, pour lequel filtrer les versions.</dd>

<dt><code>--json</code></dt>
<dd>Imprime le résultat de la commande au format JSON. Cette valeur est facultative.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :

  ```
  ibmcloud ks addon-versions --addon istio
  ```
  {: pre}

</br>
### ibmcloud ks cluster-addon-disable
{: #cs_cluster_addon_disable}

Désactivez un module complémentaire géré dans un cluster existant. Cette commande doit être associée à l'une des sous-commandes suivantes pour le module complémentaire géré que vous souhaitez désactiver.
{: shortdesc}



#### ibmcloud ks cluster-addon-disable <ph class="ignoreSpelling">istio</ph>
{: #cs_cluster_addon_disable_istio}

Désactivez le module complémentaire Istio géré. Cette commande supprime tous les composants de base d'Istio du cluster, y compris Prometheus.
{: shortdesc}

```
ibmcloud ks cluster-addon-disable istio --cluster CLUSTER [-f]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Administrateur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

<dt><code>-f</code></dt>
<dd>Facultatif : ce module complémentaire est une dépendance des modules complémentaires gérés <code>istio-extras</code>, <code>istio-sample-bookinfo</code> et <code>knative</code>. Incluez cet indicateur pour désactiver également ces modules.</dd>
</dl>

#### ibmcloud ks cluster-addon-disable istio-extras
{: #cs_cluster_addon_disable_istio_extras}

Désactivez le module complémentaire géré Istio extras. Cette opération supprime Grafana, Jeager et Kiali du cluster.
{: shortdesc}

```
ibmcloud ks cluster-addon-disable istio-extras --cluster CLUSTER [-f]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Administrateur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

<dt><code>-f</code></dt>
<dd>Facultatif : ce module complémentaire est une dépendance du module complémentaire géré <code>istio-sample-bookinfo</code>. Incluez cet indicateur pour désactiver également ce module.</dd>
</dl>

#### ibmcloud ks cluster-addon-disable istio-sample-bookinfo
{: #cs_cluster_addon_disable_istio_sample_bookinfo}

Désactivez le module complémentaire géré Istio BookInfo. Cette opération supprime tous les déploiements, pods et d'autres ressources de l'application BookInfo du cluster.
{: shortdesc}

```
ibmcloud ks cluster-addon-disable istio-sample-bookinfo --cluster CLUSTER
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Administrateur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>
</dl>

#### ibmcloud ks cluster-addon-disable <ph class="ignoreSpelling">knative</ph>
{: #cs_cluster_addon_disable_knative}

Désactivez le module complémentaire géré Knative pour retirer l'infrastructure sans serveur Knative du cluster.
{: shortdesc}

```
ibmcloud ks cluster-addon-disable knative --cluster CLUSTER
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Administrateur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>
</dl>

#### ibmcloud ks cluster-addon-disable kube-terminal
{: #cs_cluster_addon_disable_kube-terminal}

Désactivez le module complémentaire [Terminal Kubernetes](/docs/containers?topic=containers-cs_cli_install#cli_web). Pour utiliser le terminal Kubernetes dans la console de cluster {{site.data.keyword.containerlong_notm}}, vous devez d'abord réactiver le module complémentaire.
{: shortdesc}

```
ibmcloud ks cluster-addon-disable kube-terminal --cluster CLUSTER
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Administrateur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>
</dl>

</br>

### ibmcloud ks cluster-addon-enable
{: #cs_cluster_addon_enable}

Activez un module complémentaire géré dans un cluster existant. Cette commande doit être associée à l'une des sous-commandes suivantes pour le module complémentaire géré que vous souhaitez activer.
{: shortdesc}



#### ibmcloud ks cluster-addon-enable <ph class="ignoreSpelling">istio</ph>
{: #cs_cluster_addon_enable_istio}

Activez le [module complémentaire Istio](/docs/containers?topic=containers-istio) géré. Installe les composants de base d'Istio, notamment Prometheus.
{: shortdesc}

```
ibmcloud ks cluster-addon-enable istio --cluster CLUSTER [--version VERSION]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Administrateur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

<dt><code>--version <em>VERSION</em></code></dt>
<dd>Facultatif : Spécifiez la version du module complémentaire à installer. Si aucune version n'est indiquée, la version par défaut est installée.</dd>
</dl>

#### ibmcloud ks cluster-addon-enable istio-extras
{: #cs_cluster_addon_enable_istio_extras}

Activez le module complémentaire géré Istio extras. Cette commande installe Grafana, Jeager et Kiali pour offrir des fonctions de surveillance, de suivi et de visualisation supplémentaires pour Istio.
{: shortdesc}

```
ibmcloud ks cluster-addon-enable istio-extras --cluster CLUSTER [--version VERSION] [-y]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Administrateur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

<dt><code>--version <em>VERSION</em></code></dt>
<dd>Facultatif : Spécifiez la version du module complémentaire à installer. Si aucune version n'est indiquée, la version par défaut est installée.</dd>

<dt><code>-y</code></dt>
<dd>Facultatif : active la dépendance du module complémentaire <code>istio</code>.</dd>
</dl>

#### ibmcloud ks cluster-addon-enable istio-sample-bookinfo
{: #cs_cluster_addon_enable_istio_sample_bookinfo}

Activez le module complémentaire géré Istio BookInfo. Cette commande déploie [le modèle d'application BookInfo pour Istio ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://istio.io/docs/examples/bookinfo/) dans l'espace de nom <code>default</code>.
{: shortdesc}

```
ibmcloud ks cluster-addon-enable istio-sample-bookinfo --cluster CLUSTER [--version VERSION] [-y]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Administrateur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

<dt><code>--version <em>VERSION</em></code></dt>
<dd>Facultatif : Spécifiez la version du module complémentaire à installer. Si aucune version n'est indiquée, la version par défaut est installée.</dd>

<dt><code>-y</code></dt>
<dd>Facultatif : active toutes les dépendances des modules complémentaires <code>istio</code> et <code>istio-extras</code>.</dd>
</dl>

#### ibmcloud ks cluster-addon-enable <ph class="ignoreSpelling">knative</ph>
{: #cs_cluster_addon_enable_knative}

Activez le [module complémentaire Knative](/docs/containers?topic=containers-serverless-apps-knative) géré pour installer l'infrastructure sans serveur Knative.
{: shortdesc}

```
ibmcloud ks cluster-addon-enable knative --cluster CLUSTER [--version VERSION] [-y]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Administrateur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

<dt><code>--version <em>VERSION</em></code></dt>
<dd>Facultatif : Spécifiez la version du module complémentaire à installer. Si aucune version n'est indiquée, la version par défaut est installée.</dd>

<dt><code>-y</code></dt>
<dd>Facultatif : active la dépendance du module complémentaire <code>istio</code>.</dd>
</dl>

#### ibmcloud ks cluster-addon-enable kube-terminal
{: #cs_cluster_addon_enable_kube-terminal}

Activez le module complémentaire [Terminal Kubernetes](/docs/containers?topic=containers-cs_cli_install#cli_web) pour utiliser le terminal Kubernetes dans la console de cluster {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

```
ibmcloud ks cluster-addon-enable kube-terminal --cluster CLUSTER [--version VERSION]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Administrateur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

<dt><code>--version <em>VERSION</em></code></dt>
<dd>Facultatif : Spécifiez la version du module complémentaire à installer. Si aucune version n'est indiquée, la version par défaut est installée.</dd>
</dl>
</br>
### ibmcloud ks cluster-addons
{: #cs_cluster_addons}

Répertorie les modules complémentaires gérés activés dans un cluster.
{: shortdesc}

```
ibmcloud ks cluster-addons --cluster CLUSTER
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Afficheur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>
</dl>


</br>

### ibmcloud ks cluster-config
{: #cs_cluster_config}

Après la connexion, téléchargez les données de configuration et les certificats Kubernetes pour vous connecter à votre cluster et exécuter des commandes `kubectl`. Les fichiers sont téléchargés dans `user_home_directory/.bluemix/plugins/container-service/clusters/<cluster_name>`.
{: shortdesc}

```
ibmcloud ks cluster-config --cluster CLUSTER [--admin] [--export] [--network] [--powershell] [--skip-rbac] [-s] [--yaml]
```
{: pre}

**Droits minimum requis** : rôle de service {{site.data.keyword.Bluemix_notm}} IAM **Afficheur** ou **Lecteur** pour le cluster dans {{site.data.keyword.containerlong_notm}}. De plus, si vous ne disposez que d'un seul rôle de plateforme ou d'un rôle de service, des contraintes supplémentaires sont applicables.
* **Plateforme** : si vous ne disposez que d'un rôle de plateforme, vous pouvez exécuter cette commande mais il vous faut un [rôle de service](/docs/containers?topic=containers-users#platform) ou une [règle RBAC personnalisée](/docs/containers?topic=containers-users#role-binding) pour effectuer des actions Kubernetes dans le cluster.
* **Service** : si vous ne disposez que d'un rôle de service, vous pouvez exécuter cette commande. Toutefois, votre administrateur de cluster doit vous fournir le nom et l'ID du cluster car vous ne pouvez pas exécuter la commande `ibmcloud ks clusters` ou lancer la console {{site.data.keyword.containerlong_notm}} pour afficher les clusters. Après cela, vous pouvez [lancer le tableau de bord Kubernetes à partir de l'interface de ligne de commande](/docs/containers?topic=containers-app#db_cli) et utiliser Kubernetes.

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

<dt><code>--admin</code></dt>
<dd>Télécharger les fichiers de certificat et d'autorisations TLS pour le rôle Superutilisateur. Vous pouvez utiliser les certificats pour automatiser les tâches d'un cluster sans avoir à vous authentifier à nouveau. Les fichiers sont téléchargés dans `<user_home_directory>/.bluemix/plugins/container-service/clusters/<cluster_name>-admin`. Cette valeur est facultative.</dd>

<dt><code>--network</code></dt>
<dd>Télécharger le fichier de configuration Calico, les certificats TLS et les fichiers d'autorisation requis pour exécuter des commandes <code>calicoctl</code> dans votre cluster. Cette valeur est facultative. **Remarque** : afin d'obtenir la commande export pour les certificats et les données de certification Kubernetes téléchargés, vous devez exécuter cette commande sans cet indicateur.</dd>

<dt><code>--export</code></dt>
<dd>Télécharger les certificats et les données de configuration Kubernetes sans autre message que la commande export. Comme aucun message ne s'affiche, vous pouvez utiliser cet indicateur lorsque vous créez des scripts automatisés. Cette valeur est facultative.</dd>

<dt><code>--powershell</code></dt>
<dd>Extraire les variables d'environnement au format Windows PowerShell. </dd>

<dt><code>--skip-rbac</code></dt>
<dd>Ignorer l'ajout de rôles RBAC Kubernetes de l'utilisateur en fonction des rôles d'accès au service {{site.data.keyword.Bluemix_notm}} IAM dans la configuration du cluster. Incluez cette option uniquement si vous [gérez vos propres rôles RBAC Kubernetes](/docs/containers?topic=containers-users#rbac). Si vous utilisez des [rôles d'accès au service {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-access_reference#service) pour gérer tous vos utilisateurs RBAC, n'incluez pas cette option.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>

<dt><code>--yaml</code></dt>
<dd>Imprime le résultat de la commande au format YAML. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks cluster-config --cluster my_cluster
```
{: pre}

</br>
### ibmcloud ks cluster-create
{: #cs_cluster_create}

Permet de créer un cluster dans votre organisation. Pour les clusters gratuits, indiquez le nom du cluster, tout le reste est défini avec des valeurs par défaut. Un cluster gratuit est supprimé automatiquement au bout de 30 jours. Vous ne pouvez disposer que d'un cluster gratuit à la fois. Pour tirer parti de toutes les fonctions de Kubernetes, créez un cluster standard.
{: shortdesc}

```
ibmcloud ks cluster-create [--file FILE_LOCATION] [--hardware HARDWARE] --zone ZONE --machine-type MACHINE_TYPE --name NAME [--kube-version MAJOR.MINOR.PATCH] [--no-subnet] [--private-vlan PRIVATE_VLAN] [--public-vlan PUBLIC_VLAN] [--private-only] [--private-service-endpoint] [--public-service-endpoint] [--workers WORKER] [--disable-disk-encrypt] [--trusted] [-s]
```
{: pre}

**Droits minimum requis** :
* Rôle de plateforme **Administrateur** pour {{site.data.keyword.containerlong_notm}} au niveau du compte
* Rôle de plateforme **Administrateur** pour {{site.data.keyword.registrylong_notm}} au niveau du compte
* Rôle de plateforme **Superutilisateur** pour l'infrastructure IBM Cloud (SoftLayer)

**Options de commande**

<dl>
<dt><code>--file <em>FILE_LOCATION</em></code></dt>

<dd>Chemin d'accès au fichier YAML pour créer votre cluster standard. Au lieu de définir les caractéristiques de votre cluster à l'aide des options fournies dans cette commande, vous pouvez utiliser un fichier YAML. Cette valeur est facultative pour les clusters standard et n'est pas disponible pour les clusters gratuits. <p class="note">Si vous indiquez la même option dans la commande comme paramètre dans le fichier YAML, la valeur de l'option de la commande est prioritaire sur la valeur définie dans le fichier YAML. Par exemple, si vous indiquez un emplacement dans votre fichier YAML et utilisez l'option <code>--zone</code> dans la commande, la valeur que vous avez entrée dans l'option de commande se substitue à la valeur définie dans le fichier YAML.</p>
<pre class="codeblock">
<code>name: <em>&lt;cluster_name&gt;</em>
zone: <em>&lt;zone&gt;</em>
no-subnet: <em>&lt;no-subnet&gt;</em>
machine-type: <em>&lt;machine_type&gt;</em>
private-vlan: <em>&lt;private_VLAN&gt;</em>
public-vlan: <em>&lt;public_VLAN&gt;</em>
private-service-endpoint: <em>&lt;true&gt;</em>
public-service-endpoint: <em>&lt;true&gt;</em>
hardware: <em>&lt;shared_or_dedicated&gt;</em>
workerNum: <em>&lt;number_workers&gt;</em>
kube-version: <em>&lt;kube-version&gt;</em>
diskEncryption: <em>false</em>
trusted: <em>true</em>
</code></pre>
</dd>

<dt><code>--hardware <em>HARDWARE</em></code></dt>
<dd>Niveau d'isolation du matériel pour votre noeud worker. Utilisez dedicated pour que toutes les ressources physiques vous soient dédiées exclusivement ou shared pour permettre leur partage avec d'autres clients IBM. La valeur par défaut est shared. Cette valeur est facultative pour les clusters standard de machine virtuelle et n'est pas disponible pour les clusters gratuits. Pour les types de machine bare metal, indiquez `dedicated`.</dd>

<dt><code>--zone <em>ZONE</em></code></dt>
<dd>Zone où vous voulez créer le cluster. Cette valeur est obligatoire pour les clusters standard. Des clusters gratuits peuvent être créés dans la région que vous avez ciblée à l'aide de la commande <code>ibmcloud ks region-set</code>, mais vous ne pouvez pas spécifier la zone.

<p>Passez en revue les [zones disponibles](/docs/containers?topic=containers-regions-and-zones#zones). Pour étendre votre cluster à plusieurs zones, vous devez créer le cluster dans une [zone compatible avec plusieurs zones](/docs/containers?topic=containers-regions-and-zones#zones).</p>

<p class="note">Si vous sélectionnez une zone à l'étranger, il se peut que vous ayez besoin d'une autorisation légale pour stocker physiquement les données dans un pays étranger.</p>
</dd>

<dt><code>--machine-type <em>MACHINE_TYPE</em></code></dt>
<dd>Choisissez un type de machine. Vous pouvez déployer vos noeuds worker en tant que machines virtuelles sur du matériel partagé ou dédié ou en tant que machines physiques sur un serveur bare metal. Les types de machines virtuelles et physiques disponibles varient en fonction de la zone de déploiement du cluster. Pour plus d'informations, voir la documentation de la [commande ](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_machine_types)`ibmcloud ks machine-types`. Cette valeur est obligatoire pour les clusters standard et n'est pas disponible pour les clusters gratuits.</dd>

<dt><code>--name <em>NAME</em></code></dt>
<dd>Nom du cluster.  Cette valeur est obligatoire. Le nom doit commencer par une lettre, peut contenir des lettres, des nombres et des tirets (-) et ne doit pas dépasser 35 caractères. Utilisez un nom unique dans les régions. Le nom du cluster et la région dans laquelle est déployé le cluster constituent le nom de domaine qualifié complet du sous-domaine Ingress. Pour garantir que ce sous-domaine est unique dans une région, le nom de cluster peut être tronqué et complété par une valeur aléatoire dans le nom de domaine Ingress.
</dd>

<dt><code>--kube-version <em>MAJOR.MINOR.PATCH</em></code></dt>
<dd>Version Kubernetes du noeud du maître cluster. Cette valeur est facultative. Lorsque la version n'est pas spécifiée, le cluster est créé avec la valeur par défaut des versions Kubernetes prises en charge. Pour voir les versions disponibles, exécutez la commande <code>ibmcloud ks versions</code>.
</dd>

<dt><code>--no-subnet</code></dt>
<dd>Par défaut, un sous-réseau public et un sous-réseau privé portables sont créés sur le réseau local virtuel (VLAN) associé au cluster. Incluez l'indicateur <code>--no-subnet</code> afin d'éviter la création de sous-réseaux avec le cluster. Vous pouvez [créer](#cs_cluster_subnet_create) ou [ajouter](#cs_cluster_subnet_add) des sous-réseaux à un cluster ultérieurement.</dd>

<dt><code>--private-vlan <em>PRIVATE_VLAN</em></code></dt>
<dd>
<ul>
<li>Ce paramètre n'est pas disponible pour les clusters gratuits.</li>
<li>S'il s'agit du premier cluster standard que vous créez dans cette zone, n'incluez pas cet indicateur. Un VLAN privé est créé pour vous lorsque le cluster est créé.</li>
<li>Si vous avez déjà créé un cluster standard dans cette zone ou créé un VLAN privé dans l'infrastructure IBM Cloud (SoftLayer), vous devez spécifier ce VLAN privé. Les routeurs de VLAN privé commencent toujours par <code>bcr</code> (routeur de back-end) et les routeurs de VLAN public par <code>fcr</code> (routeur de front-end). Lors de la création d'un cluster et de la spécification des VLAN publics et privés, le nombre et la combinaison de lettres après ces préfixes doivent correspondre.</li>
</ul>
<p>Pour déterminer si vous disposez déjà d'un VLAN privé pour une zone spécifique ou afin d'identifier le nom d'un VLAN privé existant, exécutez la commande <code>ibmcloud ks vlans --zone <em>&lt;zone&gt;</em></code>. </p></dd>

<dt><code>--public-vlan <em>PUBLIC_VLAN</em></code></dt>
<dd>
<ul>
<li>Ce paramètre n'est pas disponible pour les clusters gratuits.</li>
<li>S'il s'agit du premier cluster standard que vous créez dans cette zone, n'utilisez pas cet indicateur. Un VLAN public est créé pour vous lorsque le cluster est créé.</li>
<li>Si vous avez déjà créé un cluster standard dans cette zone ou créé un VLAN public dans l'infrastructure IBM Cloud (SoftLayer), spécifiez ce VLAN public. Si vous désirez connecter vos noeuds worker uniquement à un VLAN privé, n'indiquez pas cette option. Les routeurs de VLAN privé commencent toujours par <code>bcr</code> (routeur de back-end) et les routeurs de VLAN public par <code>fcr</code> (routeur de front-end). Lors de la création d'un cluster et de la spécification des VLAN publics et privés, le nombre et la combinaison de lettres après ces préfixes doivent correspondre.</li>
</ul>

<p>Pour déterminer si vous disposez déjà d'un VLAN public pour une zone spécifique ou afin d'identifier le nom d'un VLAN public existant, exécutez la commande <code>ibmcloud ks vlans --zone <em>&lt;zone&gt;</em></code>. </p></dd>

<dt><code>--private-only </code></dt>
<dd>Utilisez cette option pour empêcher la création d'un VLAN public. Cette valeur est obligatoire uniquement si vous spécifiez l'indicateur `--private-vlan` sans inclure l'indicateur `--public-vlan`.<p class="note">Si les noeuds worker sont configurés uniquement avec un VLAN privé, vous devez activer le noeud final du service privé ou configurer un périphérique de passerelle. Pour plus d'informations, voir [Communication entre les noeuds worker et le maître et entre les utilisateurs et le maître](/docs/containers?topic=containers-plan_clusters#workeruser-master).</p></dd>

<dt><code>--private-service-endpoint</code></dt>
<dd>**Clusters standard exécutant Kubernetes version 1.11 ou ultérieure dans des [comptes avec la fonction VRF activée](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started)** : activez le [noeud final de service privé](/docs/containers?topic=containers-plan_clusters#workeruser-master) pour que le maître Kubernetes et les noeuds worker communiquent via le VLAN privé. De plus, vous pouvez choisir d'activer le noeud final de service public en utilisant l'indicateur `--public-service-endpoint` pour accéder à votre cluster sur Internet. Si vous activez uniquement le noeud final de service privé, vous devez être connecté au VLAN privé pour communiquer avec le maître Kubernetes. Après avoir activé le noeud final de service privé, vous ne pourrez plus le désactiver par la suite.<br><br>Après avoir créé le cluster, vous pouvez obtenir le noeud final en exécutant la commande `ibmcloud ks cluster-get --cluster <cluster_name_or_ID>`. </dd>

<dt><code>--public-service-endpoint</code></dt>
<dd>**Clusters standard exécutant Kubernetes version 1.11 ou ultérieure** : activez le [noeud final de service public](/docs/containers?topic=containers-plan_clusters#workeruser-master) pour que le maître Kubernetes soit accessible via le réseau public, par exemple pour exécuter des commandes `kubectl` depuis votre terminal. Si vous disposez d'un [compte avec la fonction VRF activée](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started) et que vous incluez également l'indicateur `--private-service-endpoint`, la communication entre le maître et les noeuds worker s'effectue sur le réseau privé et sur le réseau public. Vous pouvez désactiver par la suite le noeud final de service public si vous souhaitez un cluster privé uniquement.<br><br>Après avoir créé le cluster, vous pouvez obtenir le noeud final en exécutant la commande `ibmcloud ks cluster-get --cluster <cluster_name_or_ID>`. </dd>

<dt><code>--workers WORKER</code></dt>
<dd>Nombre de noeuds worker que vous désirez déployer dans votre cluster. Si vous ne spécifiez pas cette option, un cluster avec 1 noeud worker est créé. Cette valeur est facultative pour les clusters standard et n'est pas disponible pour les clusters gratuits.
<p class="important">Si vous créez un cluster avec un seul noeud worker par zone, vous rencontrerez peut-être des problèmes liés à Ingress. Pour assurer la haute disponibilité, créez un cluster avec au moins 2 noeuds worker par zone.</p>
<p class="important">A chaque noeud worker sont affectés un ID de noeud worker unique et un nom de domaine qui ne doivent pas être modifiés manuellement après la création du cluster. La modification de l'ID ou du domaine empêcherait le maître Kubernetes de gérer votre cluster.</p></dd>

<dt><code>--disable-disk-encrypt</code></dt>
<dd>Par défaut, les noeuds worker disposent du chiffrement de disque avec l'algorithme AES 256 bits. [En savoir plus](/docs/containers?topic=containers-security#encrypted_disk). Pour désactiver le chiffrement, incluez cette option.</dd>
</dl>

**<code>--trusted</code>**</br>
<p>**Serveur bare metal uniquement** : activez la fonction [Calcul sécurisé](/docs/containers?topic=containers-security#trusted_compute) pour vérifier que vos noeuds worker bare metal ne font pas l'objet de falsification. Si vous n'activez pas cette fonction lors de la création du cluster mais souhaitez le faire ultérieurement, vous pouvez utiliser la [commande](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable) `ibmcloud ks feature-enable`. Après avoir activé cette fonction, vous ne pourrez plus la désactiver par la suite.</p>
<p>Pour vérifier si le type de machine bare metal prend en charge la fonction trust, vérifiez la zone `Trustable` dans la sortie de la [commande](#cs_machine_types) `ibmcloud ks machine-types <zone>`. Pour vérifier que la fonction trust est activée sur un cluster, visualisez la zone **Trust ready** dans la sortie de la [commande](#cs_cluster_get) `ibmcloud ks cluster-get`. Pour vérifier que la fonction trust est activée sur un noeud worker bare metal, visualisez la zone **Trust** dans la sortie de la [commande](#cs_worker_get) `ibmcloud ks worker-get`.</p>

**<code>-s</code>**</br>
Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.

**Exemples** :


**Création d'un cluster gratuit** : indiquez le nom du cluster uniquement. Tout le reste est défini avec des valeurs par défaut. Un cluster gratuit est supprimé automatiquement au bout de 30 jours. Vous ne pouvez disposer que d'un cluster gratuit à la fois. Pour tirer parti de toutes les fonctions de Kubernetes, créez un cluster standard.

```
ibmcloud ks cluster-create --name my_cluster
```
{: pre}

**Création de votre premier cluster standard** : le premier cluster standard créé dans une zone génère également un VLAN privé. Par conséquent, n'incluez pas l'indicateur `--public-vlan`.
{: #example_cluster_create}

```
ibmcloud ks cluster-create --zone dal10 --private-vlan my_private_VLAN_ID --machine-type b3c.4x16 --name my_cluster --hardware shared --workers 2
```
{: pre}

**Création des clusters standard suivants** : si vous avez déjà créé un cluster standard auparavant dans cette zone ou créé un VLAN public dans l'infrastructure IBM Cloud (SoftLayer), indiquez ce VLAN public avec l'indicateur `--public-vlan`. Pour déterminer si vous disposez déjà d'un VLAN public pour une zone spécifique ou pour identifier le nom d'un VLAN public existant, exécutez la commande `ibmcloud ks vlans --zone <zone>`. 

```
ibmcloud ks cluster-create --zone dal10 --public-vlan my_public_VLAN_ID --private-vlan my_private_VLAN_ID --machine-type b3c.4x16 --name my_cluster --hardware shared --workers 2
```
{: pre}

</br>
### ibmcloud ks cluster-feature-disable public-service-endpoint
{: #cs_cluster_feature_disable}

**Dans les clusters exécutant Kubernetes version 1.11 ou ultérieure** : désactivez le noeud final de service public d'un cluster.
{: shortdesc}

**Important** : avant de désactiver le noeud final public, vous devez d'abord procéder comme suit pour activer le noeud final de service privé :
1. Activez le noeud final de service privé en exécutant la commande `ibmcloud ks cluster-feature-enable private-service-endpoint --cluster <cluster_name>`.
2. Suivez les invites dans l'interface de ligne de commande pour actualiser le serveur d'API du maître Kubernetes.
3. [Rechargez tous les noeuds worker dans votre cluster pour récupérer la configuration du noeud final privé](#cs_worker_reload)

```
ibmcloud ks cluster-feature-disable public-service-endpoint --cluster CLUSTER [-s] [-f]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Administrateur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks cluster-feature-disable public-service-endpoint --cluster my_cluster
```
{: pre}

</br>
### ibmcloud ks cluster-feature-enable
{: #cs_cluster_feature_enable}

Active une fonction sur un cluster existant. Cette commande doit être associée à l'une des sous-commandes suivantes pour la fonction que vous souhaitez activer.
{: shortdesc}

#### ibmcloud ks cluster-feature-enable private-service-endpoint
{: #cs_cluster_feature_enable_private_service_endpoint}

Activez le [noeud final de service privé](/docs/containers?topic=containers-plan_clusters#workeruser-master) pour que le maître cluster soit accessible en réseau privé.
{: shortdesc}

Pour exécuter cette commande :
1. Activez [VRF](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) dans votre compte d'infrastructure IBM Cloud (SoftLayer).
2. [Activez votre compte {{site.data.keyword.Bluemix_notm}} pour utiliser des noeuds finaux de service](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started).
3. Exécutez la commande `ibmcloud ks cluster-feature-enable private-service-endpoint --cluster <cluster_name>`.
4. Suivez les invites dans l'interface de ligne de commande pour actualiser le serveur d'API du maître Kubernetes.
5. [Rechargez tous les noeuds worker](#cs_worker_reload) dans votre cluster pour récupérer la configuration de noeud final privé.

```
ibmcloud ks cluster-feature-enable private-service-endpoint --cluster CLUSTER [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Administrateur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks cluster-feature-enable private-service-endpoint --cluster my_cluster
```
{: pre}

#### ibmcloud ks cluster-feature-enable public-service-endpoint
{: #cs_cluster_feature_enable_public_service_endpoint}

Activez le [noeud final de service public](/docs/containers?topic=containers-plan_clusters#workeruser-master) pour que le maître cluster soit accessible en réseau public.
{: shortdesc}

Après avoir exécuté cette commande, vous devez actualiser le serveur d'API pour utiliser le noeud final de service en suivant l'invite dans l'interface de ligne de commande.

```
ibmcloud ks cluster-feature-enable public-service-endpoint --cluster CLUSTER [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Administrateur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks cluster-feature-enable public-service-endpoint --cluster my_cluster
```
{: pre}


#### ibmcloud ks cluster-feature-enable trusted
{: #cs_cluster_feature_enable_trusted}

Activez la fonction [Calcul sécurisé](/docs/containers?topic=containers-security#trusted_compute) pour tous les noeuds worker bare metal pris en charge figurant dans le cluster. Après avoir activé cette fonction, vous ne pourrez plus la désactiver pour le cluster.
{: shortdesc}

Pour vérifier si le type de machine bare metal prend en charge la fonction de confiance (trust), vérifiez la zone **Trustable** dans la sortie de la [commande](#cs_machine_types) `ibmcloud ks machine-types <zone>`. Pour vérifier que la fonction trust est activée sur un cluster, visualisez la zone **Trust ready** dans la sortie de la [commande](#cs_cluster_get) `ibmcloud ks cluster-get`. Pour vérifier que la fonction trust est activée sur un noeud worker bare metal, visualisez la zone **Trust** dans la sortie de la [commande](#cs_worker_get) `ibmcloud ks worker-get`.

```
ibmcloud ks cluster-feature-enable trusted --cluster CLUSTER [-s] [-f]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Administrateur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

<dt><code>-f</code></dt>
<dd>Utilisez cette option pour forcer l'option <code>--trusted</code> sans invites utilisateur. Cette valeur est facultative.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks cluster-feature-enable trusted --cluster my_cluster
```
{: pre}

</br>
### ibmcloud ks cluster-get
{: #cs_cluster_get}

Affichez les détails d'un cluster.
{: shortdesc}

```
ibmcloud ks cluster-get --cluster CLUSTER [--json] [--showResources] [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Afficheur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

<dt><code>--json</code></dt>
<dd>Imprime le résultat de la commande au format JSON. Cette valeur est facultative.</dd>

<dt><code><em>--showResources</em></code></dt>
<dd>Affiche d'autres ressources de cluster, telles que des modules complémentaires, des réseaux locaux virtuels (VLAN), des sous-réseaux et du stockage.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks cluster-get --cluster my_cluster --showResources
```
{: pre}

**Exemple de sortie** :
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

Subnet VLANs
  VLAN ID   Subnet CIDR         Public   User-managed
  2234947   10.xxx.xx.xxx/29    false    false
  2234945   169.xx.xxx.xxx/29  true    false

```
{: screen}

</br>
### ibmcloud ks cluster-pull-secret-apply
{: #cs_cluster_pull_secret_apply}

Créer un ID de service {{site.data.keyword.Bluemix_notm}} IAM pour le cluster, ainsi qu'une règle pour cet ID de service pour affecter le rôle d'accès au service **Lecteur** dans {{site.data.keyword.registrylong_notm}}, puis créer une clé d'API pour l'ID de service. La clé d'API est ensuite stockée dans une valeur Kubernetes `imagePullSecret` pour que vous puissiez extraire des images de vos espaces de nom {{site.data.keyword.registryshort_notm}} pour les conteneurs figurant dans l'espace de nom Kubernetes `default`. Ce processus est automatique lorsque vous créez un cluster. Si vous obtenez une erreur lors du processus de création du cluster ou si vous disposez déjà d'un cluster, vous pouvez utiliser cette commande pour appliquer à nouveau ce processus.
{: shortdesc}

Lorsque vous exécutez cette commande, la création de données d'identification IAM et de valeurs confidentielles d'extraction d'image est initiée et peut durer un certain temps. Vous ne pouvez pas déployer des conteneurs pour extraire une image des domaines `icr.io` d'{{site.data.keyword.registrylong_notm}} tant que les valeurs confidentielles d'extraction d'image ne sont pas créées. Pour vérifier ces valeurs, exécutez la commande `kubectl get secrets | grep icr`.
{: important}

La méthode utilisant la clé d'API remplace la méthode précédente qui consistait à autoriser l'accès d'un cluster à {{site.data.keyword.registrylong_notm}} par la création automatique d'un [jeton](/docs/services/Registry?topic=registry-registry_access#registry_tokens) et en stockant ce jeton dans une valeur confidentielle (secret) d'extraction d'image. Désormais, en utilisant des clés d'API IAM pour accéder à {{site.data.keyword.registrylong_notm}}, vous avez la possibilité de personnaliser des règles IAM pour l'ID de service afin de limiter l'accès à vos espaces de nom ou à des images spécifiques. Par exemple, vous pouvez modifier les règles d'ID de service dans la valeur confidentielle (secret) d'extraction d'image pour extraire des images uniquement à partir d'un espace de nom ou d'une région d'un registre particulier. Avant de personnaliser des règles IAM, vous devez [activer les règles {{site.data.keyword.Bluemix_notm}} IAM pour {{site.data.keyword.registrylong_notm}}](/docs/services/Registry?topic=registry-user#existing_users).

Pour plus d'informations, voir [Comment votre cluster est-il autorisé à extraire des images d'{{site.data.keyword.registrylong_notm}}](/docs/containers?topic=containers-images#cluster_registry_auth).

Si vous avez ajouté des règles IAM à un ID de service existant, notamment pour limiter l'accès à un registre régional, l'ID de service, les règles IAM et la clé d'API pour le paramètre ImagePullSecret sont réinitialisés par cette commande.
{: important}

```
ibmcloud ks cluster-pull-secret-apply --cluster CLUSTER
```
{: pre}

**Droits minimum requis** :
*  Rôle de plateforme **Opérateur ou Administrateur** pour le cluster dans {{site.data.keyword.containerlong_notm}}
*  Rôle de plateforme **Administrateur** dans {{site.data.keyword.registrylong_notm}}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>
</dl>
</br>
### ibmcloud ks cluster-rm
{: #cs_cluster_rm}

Supprime un cluster de votre organisation.
{: shortdesc}

```
ibmcloud ks cluster-rm --cluster CLUSTER [--force-delete-storage] [-f] [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Administrateur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

<dt><code>--force-delete-storage</code></dt>
<dd>Supprime le cluster et tout stockage persistant utilisé par le cluster. **Attention** : si vous incluez cet indicateur, les données stockées dans le cluster ou dans les instances de stockage associées ne seront plus récupérables. Cette valeur est facultative.</dd>

<dt><code>-f</code></dt>
<dd>Utilisez cette option pour forcer la suppression d'un cluster sans invites utilisateur. Cette valeur est facultative.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks cluster-rm --cluster my_cluster
```
{: pre}

</br>
### ibmcloud ks cluster-update
{: #cs_cluster_update}

Mettez à jour le maître Kubernetes à la version par défaut de l'API. Pendant la mise à jour, vous ne pouvez ni accéder au cluster, ni le modifier. Les noeuds worker, les applications et les ressources déployés par l'utilisateur ne sont pas modifiés et continuent à s'exécuter.
{: shortdesc}

Vous pourriez devoir modifier vos fichiers YAML en vue de déploiements ultérieurs. Consultez cette [note sur l'édition](/docs/containers?topic=containers-cs_versions) pour plus de détails.

```
ibmcloud ks cluster-update --cluster CLUSTER [--kube-version MAJOR.MINOR.PATCH] [--force-update] [-f] [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Opérateur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

<dt><code>--kube-version <em>MAJOR.MINOR.PATCH</em></code></dt>
<dd>Version Kubernetes du cluster. Si vous ne spécifiez pas de version, la maître Kubernetes est mis à jour vers la version d'API par défaut. Pour voir les versions disponibles, exécutez la commande [ibmcloud ks kube-versions](#cs_kube_versions). Cette valeur est facultative.</dd>

<dt><code>--force-update</code></dt>
<dd>Cette option tente d'effectuer la mise à jour même si la modification est supérieure à deux niveaux de version secondaire par rapport à la version de noeud worker. Cette valeur est facultative.</dd>

<dt><code>-f</code></dt>
<dd>Force la commande à s'exécuter sans invites utilisateur. Cette valeur est facultative.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks cluster-update --cluster my_cluster
```
{: pre}

</br>
### ibmcloud ks clusters
{: #cs_clusters}

Permet de répertorier tous les clusters de votre compte {{site.data.keyword.Bluemix_notm}}.
{: shortdesc}

Les clusters de tous les emplacements sont renvoyés. Pour filtrer des clusters par emplacement spécifique, ajoutez l'indicateur `--locations`. Par exemple, si vous filtrez des clusters pour l'agglomération `dal`, les clusters à zones multiples de cette agglomération et les clusters à zone unique de centres de données (zones) situés dans cette agglomération sont renvoyés. Si vous filtrez des clusters pour le centre de données `dal10` (zone), les clusters à zones multiples ayant un noeud worker dans cette zone et les clusters à zone unique situés dans cette zone sont renvoyés. Notez que vous pouvez transmettre un emplacement ou une liste d'emplacements séparés par des virgules. 

Si vous utilisez la version bêta `0.2` (existante) du plug-in {{site.data.keyword.containerlong_notm}}, seuls les clusters qui se trouvent dans la région que vous ciblez actuellement sont renvoyés. Pour changer de région, exécutez la commande `ibmcloud ks region-set`.
{: deprecated}

```
ibmcloud ks clusters [--locations LOCATION] [--json] [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Afficheur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--locations <em>LOCATION</em></code></dt>
<dd>Filtrez les zones en fonction d'un emplacement spécifique ou d'une liste d'emplacements séparés par des virgules. Pour voir les emplacements pris en charge, exécutez la commande <code>ibmcloud ks supported-locations</code>.</dd>

<dt><code>--json</code></dt>
<dd>Imprime le résultat de la commande au format JSON. Cette valeur est facultative.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks clusters --locations ams03,wdc,ap
```
{: pre}

</br>
### Déprécié : ibmcloud ks kube-versions
{: #cs_kube_versions}

Affiche la liste des versions Kubernetes prises en charge dans {{site.data.keyword.containerlong_notm}}. Mettez à jour votre [maître cluster](#cs_cluster_update) et vos [noeuds worker](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update) à la version par défaut pour bénéficier des fonctionnalités stables les plus récentes.
{: shortdesc}

Cette commande est dépréciée. Utilisez la commande [ibmcloud ks versions command](#cs_versions_command) à la place.
{: deprecated}

```
ibmcloud ks kube-versions [--json] [-s]
```
{: pre}

**Droits minimum requis** : aucun

**Options de commande** :
<dl>
<dt><code>--json</code></dt>
<dd>Imprime le résultat de la commande au format JSON. Cette valeur est facultative.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks kube-versions
```
{: pre}

</br>

### ibmcloud ks versions
{: #cs_versions_command}

Permet de répertorier toutes les versions de plateforme de conteneur qui sont disponibles pour les clusters {{site.data.keyword.containerlong_notm}}. Mettez à jour votre [maître cluster](#cs_cluster_update) et vos [noeuds worker](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update) à la version par défaut pour bénéficier des fonctionnalités stables les plus récentes.
{: shortdesc}

```
ibmcloud ks versions [--show-version PLATFORM][--json] [-s]
```
{: pre}

**Droits minimum requis** : aucun

**Options de commande** :
<dl>
<dt><code>--show-version</code> <em>PLATFORM</em></dt>
<dd>Permet d'afficher uniquement les versions de la plateforme de conteneur spécifiée. Les valeurs prises en charge sont <code>kubernetes</code> ou <code>openshift</code>.</dd>

<dt><code>--json</code></dt>
<dd>Imprime le résultat de la commande au format JSON. Cette valeur est facultative.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks versions
```
{: pre}

<br />



## Commandes de cluster : Services et intégrations
{: #cluster_services_commands}

### ibmcloud ks cluster-service-bind
{: #cs_cluster_service_bind}

Permet de créer des données d'identification de service pour un service {{site.data.keyword.Bluemix_notm}} et de stocker ces données d'identification dans une valeur confidentielle Kubernetes dans votre cluster. Pour afficher les services {{site.data.keyword.Bluemix_notm}} disponibles dans le catalogue {{site.data.keyword.Bluemix_notm}}, exécutez la commande `ibmcloud service offerings`. **Remarque **: vous ne pouvez ajouter que des services {{site.data.keyword.Bluemix_notm}} qui prennent en charge les clés de service.
{: shortdesc}

Pour plus d'informations sur la liaison de service et pour identifier les services que vous pouvez ajouter à votre cluster, voir [Ajout de services à l'aide de la liaison de service IBM Cloud](/docs/containers?topic=containers-service-binding).

```
ibmcloud ks cluster-service-bind --cluster CLUSTER --namespace KUBERNETES_NAMESPACE --service SERVICE_INSTANCE [--key SERVICE_INSTANCE_KEY] [--role IAM_SERVICE_ROLE] [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Editeur** pour le cluster dans {{site.data.keyword.containerlong_notm}} et rôle Cloud Foundry **Développeur**

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

<dt><code>--key <em>SERVICE_INSTANCE_KEY</em></code></dt>
<dd>Nom ou identificateur global unique (GUID) d'une clé de service existante. Cette valeur est facultative. Lorsque vous utilisez la commande `service-binding`, de nouvelles données d'identification de service sont automatiquement créées pour votre instance de service et prennent automatiquement le rôle d'accès de service **Auteur** IAM pour les services activés par IAM. Si vous souhaitez utiliser une clé de service existante que vous avez créée précédemment, utilisez cette option. Si vous définissez une clé de service, vous ne pouvez pas définir l'option `--role` en même temps, car vos clés de service sont déjà créées avec un rôle d'accès de service IAM. </dd>

<dt><code>--namespace <em>KUBERNETES_NAMESPACE</em></code></dt>
<dd>Nom de l'espace de nom Kubernetes dans lequel vous voulez créer la valeur confidentielle Kubernetes pour vos données d'identification de service. Cette valeur est obligatoire.</dd>

<dt><code>--service <em>SERVICE_INSTANCE</em></code></dt>
<dd>Nom de l'instance de service {{site.data.keyword.Bluemix_notm}} que vous souhaitez lier. Pour trouver le nom, exécutez la commande <code>ibmcloud service list</code> pour les services Cloud Foundry, et la commande <code>ibmcloud resource service-instances</code> pour les services activés par IAM. Cette valeur est obligatoire. </dd>

<dt><code>--role <em>IAM_SERVICE_ROLE</em></code></dt>
<dd>Rôle {{site.data.keyword.Bluemix_notm}} IAM que vous voulez donner à la clé de service. Cette valeur est facultative et peut être utilisée uniquement pour les services activés par IAM. Si vous ne définissez pas cette option, vos données d'identification de service sont automatiquement créées et prennent automatiquement le rôle d'accès de service **Auteur** IAM. Si vous souhaitez utiliser des clés de service existantes en spécifiant l'option `--key`, n'ajoutez pas cette option.<br><br>
Pour répertorier les rôles disponibles pour le service, exécutez la commande `ibmcloud iam roles --service <service_name>`. Le nom de service correspond au nom du service dans le catalogue que vous pouvez obtenir en exécutant la commande `ibmcloud catalog search`.  </dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks cluster-service-bind --cluster my_cluster --namespace my_namespace --service my_service_instance
```
{: pre}

</br>
### ibmcloud ks cluster-service-unbind
{: #cs_cluster_service_unbind}

Retirez un service {{site.data.keyword.Bluemix_notm}} d'un cluster en supprimant sa liaison avec un espace de nom Kubernetes.
{: shortdesc}

Lorsque vous supprimez un service {{site.data.keyword.Bluemix_notm}}, ses données d'identification du service sont supprimées du cluster. Si un pod utilise encore ce service, son opération échoue vu que les données d'identification du service sont introuvables.
{: note}

```
ibmcloud ks cluster-service-unbind --cluster CLUSTER --namespace KUBERNETES_NAMESPACE --service SERVICE_INSTANCE [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Editeur** pour le cluster dans {{site.data.keyword.containerlong_notm}} et rôle Cloud Foundry **Développeur**

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

<dt><code>--namespace <em>KUBERNETES_NAMESPACE</em></code></dt>
<dd>Nom de l'espace de nom Kubernetes. Cette valeur est obligatoire.</dd>

<dt><code>--service <em>SERVICE_INSTANCE</em></code></dt>
<dd>Nom de l'instance de service {{site.data.keyword.Bluemix_notm}} que vous souhaitez retirer. Pour trouver le nom de l'instance de service, exécutez la commande `ibmcloud ks cluster-services --cluster <cluster_name_or_ID>`. Cette valeur est obligatoire.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks cluster-service-unbind --cluster my_cluster --namespace my_namespace --service 8567221
```
{: pre}

</br>
### ibmcloud ks cluster-services
{: #cs_cluster_services}

Répertorie les services liés à un ou à tous les espaces de nom Kubernetes dans un cluster. Si aucune option n'est spécifiée, les services pour l'espace de nom par défaut sont affichés.
{: shortdesc}

```
ibmcloud ks cluster-services --cluster CLUSTER [--namespace KUBERNETES_NAMESPACE] [--all-namespaces] [--json] [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Afficheur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

<dt><code>--namespace <em>KUBERNETES_NAMESPACE</em></code>, <code>-n <em>KUBERNETES_NAMESPACE</em></code></dt>
<dd>Inclut les services liés à un espace de nom spécifique dans un cluster. Cette valeur est facultative.</dd>

<dt><code>--all-namespaces</code></dt>
<dd>Inclut les services liés à tous les espaces de nom dans un cluster. Cette valeur est facultative.</dd>

<dt><code>--json</code></dt>
<dd>Imprime le résultat de la commande au format JSON. Cette valeur est facultative.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks cluster-services --cluster my_cluster --namespace my_namespace
```
{: pre}

</br>
### ibmcloud ks va
{: #cs_va}

Après avoir [installé le scanner de conteneur](/docs/services/va?topic=va-va_index#va_install_container_scanner), affichez un rapport d'évaluation des vulnérabilités d'un conteneur présent dans votre cluster.
{: shortdesc}

```
ibmcloud ks va --container CONTAINER_ID [--extended] [--vulnerabilities] [--configuration-issues] [--json]
```
{: pre}

**Droits minimum requis** : rôle d'accès au service **Lecteur** pour {{site.data.keyword.registrylong_notm}}. **Remarque** : n'affectez aucune règle pour {{site.data.keyword.registryshort_notm}} au niveau du groupe de ressources.

**Options de commande** :
<dl>
<dt><code>--container CONTAINER_ID</code></dt>
<dd><p>ID du conteneur. Cette valeur est obligatoire.</p>
<p>Pour déterminer l'ID de votre conteneur :<ol><li>[Ciblez l'interface CLI de Kubernetes sur votre cluster](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).</li><li>Répertoriez vos pods en exécutant la commande `kubectl get pods`.</li><li>Recherchez la zone **Container ID** dans la sortie de la commande `kubectl describe pod <pod_name>`. Par exemple, `Container ID: containerd://1a11a1aa2b2b22223333c44444ccc555667d7dd777888e8ef99f1011121314g15`.</li><li>Supprimez le préfixe `containerd://` de l'ID avant d'utiliser l'ID de conteneur pour la commande `ibmcloud ks va`. Par exemple, `1a11a1aa2b2b22223333c44444ccc555667d7dd777888e8ef99f1011121314g15`.</li></ol></p></dd>

<dt><code>--extended</code></dt>
<dd><p>Développer la sortie de la commande pour afficher plus d'informations sur les correctifs correspondant aux packages vulnérables. Cette valeur est facultative.</p>
<p>Par défaut, les résultats de l'analyse présentent l'ID, le statut de la règle, les packages concernés et comment y remédier. Avec l'indicateur `--extended`, des informations, telles que le récapitulatif, les consignes de sécurité du fournisseur et le lien vers les mentions légales.</p></dd>

<dt><code>--vulnerabilities</code></dt>
<dd>Restreindre la sortie de commande pour n'afficher que les vulnérabilités de package. Cette valeur est facultative. Vous ne pouvez pas utiliser cet indicateur avec `--configuration-issues`.</dd>

<dt><code>--configuration-issues</code></dt>
<dd>Restreindre la sortie de commande pour n'afficher que les problèmes de configuration. Cette valeur est facultative. Vous ne pouvez pas utiliser cet indicateur avec `--vulnerabilities`.</dd>

<dt><code>--json</code></dt>
<dd>Imprime le résultat de la commande au format JSON. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks va --container 1a11a1aa2b2b22223333c44444ccc555667d7dd777888e8ef99f1011121314g15 --extended --vulnerabilities --json
```
{: pre}

</br>
### Version bêta : ibmcloud ks key-protect-enable
{: #cs_key_protect}

Chiffrez vos valeurs confidentielles (secrets) Kubernetes à l'aide d'[{{site.data.keyword.keymanagementservicefull}} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](/docs/services/key-protect?topic=key-protect-getting-started-tutorial#getting-started-tutorial) comme [fournisseur KMS (Key Management Service) ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/tasks/administer-cluster/kms-provider/) dans votre cluster. Pour faire pivoter une clé dans un cluster avec un chiffrement de clé existant, relancez l'exécution de cette commande avec un nouvel ID de clé racine.
{: shortdesc}

Ne supprimez pas les clés racine dans votre instance {{site.data.keyword.keymanagementserviceshort}}. Ne supprimez pas les clés même si vous effectuez une rotation pour utiliser une nouvelle clé. Vous ne pouvez pas accéder ou retirer les données dans etcd ou les données des secrets dans votre cluster si vous supprimez une clé racine.
{: important}

```
ibmcloud ks key-protect-enable --cluster CLUSTER_NAME_OR_ID --key-protect-url ENDPOINT --key-protect-instance INSTANCE_GUID --crk ROOT_KEY_ID
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Administrateur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--container CLUSTER_NAME_OR_ID</code></dt>
<dd>Nom ou ID du cluster.</dd>

<dt><code>--key-protect-url ENDPOINT</code></dt>
<dd>Noeud final {{site.data.keyword.keymanagementserviceshort}} pour votre instance de cluster. Pour l'obtenir, voir [Noeuds finaux de service par région](/docs/services/key-protect?topic=key-protect-regions#service-endpoints).</dd>

<dt><code>--key-protect-instance INSTANCE_GUID</code></dt>
<dd>Identificateur global unique (GUID) de votre instance {{site.data.keyword.keymanagementserviceshort}}. Pour l'obtenir le GUID, exécutez la commande <code>ibmcloud resource service-instance SERVICE_INSTANCE_NAME --id</code> et copiez la seconde valeur (et non pas le CRN complet).</dd>

<dt><code>--crk ROOT_KEY_ID</code></dt>
<dd>ID de votre clé racine {{site.data.keyword.keymanagementserviceshort}}. Pour obtenir la valeur de CRK, voir [Affichage des clés](/docs/services/key-protect?topic=key-protect-view-keys#view-keys).</dd>
</dl>

**Exemple** :
```
ibmcloud ks key-protect-enable --cluster mycluster --key-protect-url keyprotect.us-south.bluemix.net --key-protect-instance a11aa11a-bbb2-3333-d444-e5e555e5ee5 --crk 1a111a1a-bb22-3c3c-4d44-55e555e55e55
```
{: pre}

</br>
### ibmcloud ks webhook-create
{: #cs_webhook_create}

Permet d'enregistrer un webhook.
{: shortdesc}

```
ibmcloud ks webhook-create --cluster CLUSTER --level LEVEL --type slack --url URL  [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Editeur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

<dt><code>--level <em>LEVEL</em></code></dt>
<dd>Niveau de notification, tel que <code>Normal</code> ou <code>Warning</code>. <code>Warning</code> est la valeur par défaut. Cette valeur est facultative.</dd>

<dt><code>--type <em>slack</em></code></dt>
<dd>Type de webhook. Seul le type slack est pris en charge actuellement. Cette valeur est obligatoire.</dd>

<dt><code>--url <em>URL</em></code></dt>
<dd>URL du webhook. Cette valeur est obligatoire.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks webhook-create --cluster my_cluster --level Normal --type slack --url http://github.com/mywebhook
```
{: pre}

<br />


## Commandes de cluster : Sous-réseaux
{: #cluster_subnets_commands}

### ibmcloud ks cluster-subnet-add
{: #cs_cluster_subnet_add}

Permet de rendre un sous-réseau public ou privé portable existant dans votre compte d'infrastructure IBM Cloud (SoftLayer) disponible pour un cluster ou de réutiliser des sous-réseaux à partir d'un cluster supprimé au lieu d'utiliser les sous-réseaux mis à disposition automatiquement.
{: shortdesc}

<p class="important">Les adresses IP publiques portables sont facturées au mois. Si vous retirez des adresses IP publiques portables après la mise en place de votre cluster, vous devez quand même payer les frais mensuels, même si vous ne les avez utilisées que brièvement. </br>
</br>Lorsque vous rendez un sous-réseau accessible à un cluster, les adresses IP de ce sous-réseau sont utilisées pour la mise en réseau du cluster. Pour éviter des conflits d'adresse IP, prenez soin de n'utiliser le sous-réseau qu'avec un seul cluster. N'utilisez pas en même temps un sous-réseau pour plusieurs clusters ou à d'autres fins en dehors d'{{site.data.keyword.containerlong_notm}}.</br>
</br>Pour activer la communication entre des noeuds worker qui se trouvent dans différents sous-réseaux d'un même VLAN, vous devez [activer le routage entre les sous-réseaux sur le même VLAN](/docs/containers?topic=containers-subnets#subnet-routing).</p>

```
ibmcloud ks cluster-subnet-add --cluster CLUSTER --subnet-id SUBNET [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Opérateur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

<dt><code>--subnet-id <em>SUBNET</em></code></dt>
<dd>ID du sous-réseau. Cette valeur est obligatoire.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks cluster-subnet-add --cluster my_cluster --subnet-id 1643389
```
{: pre}

</br>
### ibmcloud ks cluster-subnet-create
{: #cs_cluster_subnet_create}

Créez un sous-réseau portable dans un compte d'infrastructure IBM Cloud (SoftLayer) sur votre VLAN public ou privé et rendez-le disponible pour un cluster.
{: shortdesc}

<p class="important">Lorsque vous rendez un sous-réseau accessible à un cluster, les adresses IP de ce sous-réseau sont utilisées pour la mise en réseau du cluster. Pour éviter des conflits d'adresse IP, prenez soin de n'utiliser le sous-réseau qu'avec un seul cluster. N'utilisez pas en même temps un sous-réseau pour plusieurs clusters ou à d'autres fins en dehors d'{{site.data.keyword.containerlong_notm}}.</br>
</br>Si vous disposez de plusieurs VLAN pour un cluster, de plusieurs sous-réseaux sur le même VLAN ou d'un cluster à zones multiples, vous devez activer une fonction [VRF (Virtual Router Function)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) pour votre compte d'infrastructure IBM Cloud (SoftLayer) pour que vos noeuds worker puissent communiquer entre eux sur le réseau privé. Pour activer la fonction VRF, [contactez le représentant de votre compte d'infrastructure IBM Cloud (SoftLayer)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#how-you-can-initiate-the-conversion). Si vous ne parvenez pas à activer la fonction VRF ou si vous ne souhaitez pas le faire, activez la fonction [Spanning VLAN](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning). Pour effectuer cette action, vous devez disposer du [droit d'infrastructure](/docs/containers?topic=containers-users#infra_access) **Réseau > Gérer le spanning VLAN pour réseau**, ou vous pouvez demander au propriétaire du compte de l'activer. Pour vérifier si le spanning VLAN est déjà activé, utilisez la [commande](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get) `ibmcloud ks vlan-spanning-get --region <region>`. </p>

```
ibmcloud ks cluster-subnet-create --cluster CLUSTER --size SIZE --vlan VLAN_ID [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Opérateur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire. Pour afficher la liste de vos clusters, utilisez la [commande](#cs_clusters) `ibmcloud ks clusters`.</dd>

<dt><code>--size <em>SIZE</em></code></dt>
<dd>Nombre d'adresses IP que vous désirez créer dans le sous-réseau portable. Valeurs admises : 8, 16, 32 ou 64. <p class="note"> Lorsque vous ajoutez des adresses IP portables à votre sous-réseau, trois adresses IP sont utilisées pour les opérations de réseau internes au cluster. Vous ne pouvez pas utiliser ces trois adresses IP pour votre équilibreurs de charge d'application (ALB) Ingress ou pour créer des services d'équilibreur de charge de réseau (NLB). Par exemple, si vous demandez huit adresses IP publiques portables, vous pouvez en utiliser cinq pour exposer vos applications au public.</p> </dd>

<dt><code>--vlan <em>VLAN_ID</em></code></dt>
<dd>ID du VLAN public ou privé sur lequel vous voulez créer le sous-réseau. Vous devez sélectionner un VLAN public ou privé auquel un noeud worker existant est connecté. Pour voir quels sont les VLAN public ou privé auxquels vos noeuds worker sont connectés, exécutez la commande <code>ibmcloud ks cluster-get --cluster &lt;cluster&gt; --showResources</code> et recherchez la section <strong>Subnet VLANs</strong> dans la sortie de cette commande. Le sous-réseau est fourni dans la même zone que le VLAN.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks cluster-subnet-create --cluster my_cluster --size 8 --vlan 1764905
```
{: pre}

</br>
### ibmcloud ks cluster-user-subnet-add
{: #cs_cluster_user_subnet_add}

Mettez à disposition votre propre sous-réseau privé sur vos clusters {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

Ce sous-réseau privé n'est pas celui qui est fourni par l'infrastructure IBM Cloud (SoftLayer). De ce fait, vous devez configurer tout routage de trafic entrant et sortant pour le sous-réseau. Pour ajouter un sous-réseau d'infrastructure IBM Cloud (SoftLayer), utilisez la [commande](#cs_cluster_subnet_add) `ibmcloud ks cluster-subnet-add`.

<p class="important">Lorsque vous rendez un sous-réseau accessible à un cluster, les adresses IP de ce sous-réseau sont utilisées pour la mise en réseau du cluster. Pour éviter des conflits d'adresse IP, prenez soin de n'utiliser le sous-réseau qu'avec un seul cluster. N'utilisez pas en même temps un sous-réseau pour plusieurs clusters ou à d'autres fins en dehors d'{{site.data.keyword.containerlong_notm}}.</br>
</br>Si vous disposez de plusieurs VLAN pour un cluster, de plusieurs sous-réseaux sur le même VLAN ou d'un cluster à zones multiples, vous devez activer une fonction [VRF (Virtual Router Function)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) pour votre compte d'infrastructure IBM Cloud (SoftLayer) pour que vos noeuds worker puissent communiquer entre eux sur le réseau privé. Pour activer la fonction VRF, [contactez le représentant de votre compte d'infrastructure IBM Cloud (SoftLayer)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#how-you-can-initiate-the-conversion). Si vous ne parvenez pas à activer la fonction VRF ou si vous ne souhaitez pas le faire, activez la fonction [Spanning VLAN](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning). Pour effectuer cette action, vous devez disposer du [droit d'infrastructure](/docs/containers?topic=containers-users#infra_access) **Réseau > Gérer le spanning VLAN pour réseau**, ou vous pouvez demander au propriétaire du compte de l'activer. Pour vérifier si le spanning VLAN est déjà activé, utilisez la [commande](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get) `ibmcloud ks vlan-spanning-get --region <region>`. </p>

```
ibmcloud ks cluster-user-subnet-add --cluster CLUSTER --subnet-cidr SUBNET_CIDR --private-vlan PRIVATE_VLAN
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Opérateur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

<dt><code>--subnet-cidr <em>SUBNET_CIDR</em></code></dt>
<dd>CIDR (Classless InterDomain Routing) du sous-réseau. Cette valeur est obligatoire et ne doit pas entrer en conflit avec un sous-réseau utilisé par l'infrastructure IBM Cloud (SoftLayer). Les préfixes pris en charge sont compris entre `/30` (1 adresse IP) et `/24` (253 adresses IP). Si vous avez défini un CIDR avec une longueur de préfixe et que vous devez modifier sa valeur, ajoutez d'abord le nouveau CIDR, puis [supprimez l'ancien CIDR](#cs_cluster_user_subnet_rm).</dd>

<dt><code>--private-vlan <em>PRIVATE_VLAN</em></code></dt>
<dd>ID du VLAN privé. Cette valeur est obligatoire. Elle doit correspondre à l'ID du VLAN privé d'un ou plusieurs noeuds worker dans le cluster.</dd>
</dl>

**Exemple** :
```
ibmcloud ks cluster-user-subnet-add --cluster my_cluster --subnet-cidr 169.xx.xxx.xxx/29 --private-vlan 1502175
```
{: pre}

</br>
### ibmcloud ks cluster-user-subnet-rm
{: #cs_cluster_user_subnet_rm}

Supprimez votre propre sous-réseau privé du cluster indiqué. Tout service déployé avec une adresse IP depuis votre propre sous-réseau privé reste actif une fois le sous-réseau supprimé.
{: shortdesc}

```
ibmcloud ks cluster-user-subnet-rm --cluster CLUSTER --subnet-cidr SUBNET_CIDR --private-vlan PRIVATE_VLAN
```
{: pre}
**Droits minimum requis** : rôle de plateforme **Opérateur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

<dt><code>--subnet-cidr <em>SUBNET_CIDR</em></code></dt>
<dd>CIDR (Classless InterDomain Routing) du sous-réseau. Cette valeur est obligatoire et doit correspondre au CIDR défini par la [commande](#cs_cluster_user_subnet_add) `ibmcloud ks cluster-user-subnet-add`.</dd>

<dt><code>--private-vlan <em>PRIVATE_VLAN</em></code></dt>
<dd>ID du VLAN privé. Cette valeur est obligatoire et doit correspondre à l'ID du VLAN défini par la [commande](#cs_cluster_user_subnet_add) `ibmcloud ks cluster-user-subnet-add`.</dd>
</dl>

**Exemple** :
```
ibmcloud ks cluster-user-subnet-rm --cluster my_cluster --subnet-cidr 169.xx.xxx.xxx/29 --private-vlan 1502175
```
{: pre}

</br>
### ibmcloud ks subnets
{: #cs_subnets}

Permet de répertorier les sous-réseaux portables disponibles dans votre compte d'infrastructure IBM Cloud (SoftLayer).
{: shortdesc}

Les sous-réseaux de tous les emplacements sont renvoyés. Pour filtrer des sous-réseaux par emplacement spécifique, ajoutez l'indicateur `--locations`. 

```
ibmcloud ks subnets [--locations LOCATIONS] [--json] [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Afficheur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--locations <em>LOCATION</em></code></dt>
<dd>Filtrez les zones en fonction d'un emplacement spécifique ou d'une liste d'emplacements séparés par des virgules. Pour voir les emplacements pris en charge, exécutez la commande <code>ibmcloud ks supported-locations</code>.</dd>

<dt><code>--json</code></dt>
<dd>Imprime le résultat de la commande au format JSON. Cette valeur est facultative.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks subnets --locations ams03,wdc,ap
```
{: pre}

<br />


## Commandes de l'équilibreur de charge d'application (ALB) Ingress
{: #alb_commands}

### ibmcloud ks alb-autoupdate-disable
{: #cs_alb_autoupdate_disable}

Désactivez les mises à jour automatiques de tous les pods d'équilibreur de charge d'application Ingress d'un cluster.
{: shortdesc}

Par défaut, les mises à jour automatiques du module complémentaire de l'équilibreur de charge d'application (ALB) Ingress sont activées. Les pods d'ALB sont automatiquement mis à jour lorsqu'une nouvelle version est disponible. Si vous préférez mettre à jour le module complémentaire manuellement, utilisez cette commande pour désactiver les mises à jour automatiques. Vous pouvez ensuite mettre à jour les pods d'ALB en exécutant la [commande `ibmcloud ks alb-update`](#cs_alb_update).

Lorsque vous mettez à jour une version Kubernetes principale ou secondaire de votre cluster, IBM effectue les modifications nécessaires dans le déploiement Ingress, mais ne change pas la version de votre module complémentaire ALB Ingress. Vous êtes chargé de vérifier la compatibilité des dernières versions de Kubernetes et des images de votre module complémentaire ALB Ingress.

```
ibmcloud ks alb-autoupdate-disable --cluster CLUSTER
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Editeur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Exemple** :
```
ibmcloud ks alb-autoupdate-disable --cluster mycluster
```
{: pre}

</br>
### ibmcloud ks alb-autoupdate-enable
{: #cs_alb_autoupdate_enable}

Activez les mises à jour automatiques de tous les pods d'équilibreur de charge d'application Ingress d'un cluster.
{: shortdesc}

Si les mises à jour automatiques du module complémentaire ALB Ingress sont désactivées, vous pouvez les réactivez. Chaque fois que la version suivante est disponible, les équilibreurs de charge ALB sont automatiquement mis à jour à la dernière version.

```
ibmcloud ks alb-autoupdate-enable --cluster CLUSTER
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Editeur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

</br>
### ibmcloud ks alb-autoupdate-get
{: #cs_alb_autoupdate_get}

Vérifie si les mises à jour automatiques du module complémentaire ALB Ingress sont activées et si les ALB sont mis à jour à la dernière version.
{: shortdesc}

```
ibmcloud ks alb-autoupdate-get --cluster CLUSTER
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Editeur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

</br>
### Version bêta : ibmcloud ks alb-cert-deploy
{: #cs_alb_cert_deploy}

Déploiement ou mise à jour d'un certificat à partir de votre instance {{site.data.keyword.cloudcerts_long_notm}} vers l'équilibreur de charge d'application (ALB) dans un cluster. Vous ne pouvez mettre à jour que des certificats importés depuis la même instance {{site.data.keyword.cloudcerts_long_notm}}.
{: shortdesc}

Lorsque vous importez un certificat avec cette commande, la valeur de secret du certificat est créée dans un espace de nom nommé `ibm-cert-store`. La référence à ce secret est ensuite créée dans l'espace de nom `default`, accessible à n'importe quelle ressource Ingress d'un espace de nom. Lorsque l'équilibreur de charge d'application traite les demandes, il suit cette référence pour récupérer et utiliser cette valeur de secret du certificat depuis l'espace de nom `ibm-cert-store`.

Pour rester dans les [limites de débit](https://cloud.ibm.com/apidocs/certificate-manager#rate-limiting) définies par {{site.data.keyword.cloudcerts_short}}, patientez au moins 45 secondes entre deux commandes `alb-cert-deploy` et `alb-cert-deploy --update` successives.
{: note}

```
ibmcloud ks alb-cert-deploy [--update] --cluster CLUSTER --secret-name SECRET_NAME --cert-crn CERTIFICATE_CRN [--update] [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Administrateur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande**

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

<dt><code>--update</code></dt>
<dd>Met à jour le certificat d'une valeur confidentielle d'équilibreur de charge d'application (ALB) dans un cluster. Cette valeur est facultative.</dd>

<dt><code>--secret-name <em>SECRET_NAME</em></code></dt>
<dd>Spécifiez un nom pour le secret ALB lorsqu'il est créé dans le cluster. Cette valeur est obligatoire. Vérifiez que vous n'avez pas créé une valeur confidentielle ayant le même nom que la valeur confidentielle d'Ingress fournie par IBM. Vous pouvez obtenir le nom du secret Ingress fourni par IBM en exécutant la commande <code>ibmcloud ks cluster-get --cluster <cluster_name_or_ID> | grep Ingress</code>.</dd>

<dt><code>--cert-crn <em>CERTIFICATE_CRN</em></code></dt>
<dd>CRN du certificat. Cette valeur est obligatoire.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemples** :

Exemple de déploiement d'une valeur confidentielle ALB :
```
ibmcloud ks alb-cert-deploy --secret-name my_alb_secret --cluster my_cluster --cert-crn crn:v1:staging:public:cloudcerts:us-south:a/06580c923e40314421d3b6cb40c01c68:0db4351b-0ee1-479d-af37-56a4da9ef30f:certificate:4bc35b7e0badb304e60aef00947ae7ff
```
{: pre}

Exemple de mise à jour d'une valeur confidentielle ALB existante :
```
ibmcloud ks alb-cert-deploy --update --secret-name my_alb_secret --cluster my_cluster --cert-crn crn:v1:staging:public:cloudcerts:us-south:a/06580c923e40314421d3b6cb40c01c68:0db4351b-0ee1-479d-af37-56a4da9ef30f:certificate:7e21fde8ee84a96d29240327daee3eb2
```
{: pre}

</br>
### Version bêta : ibmcloud ks alb-cert-get
{: #cs_alb_cert_get}

Si vous avez importé un certificat de {{site.data.keyword.cloudcerts_short}} dans l'ALB d'un cluster, affichez les informations sur le certificat TLS, telles que les valeurs 'secret' associées à ce certificat.
{: shortdesc}

```
ibmcloud ks alb-cert-get --cluster CLUSTER [--secret-name SECRET_NAME] [--cert-crn CERTIFICATE_CRN] [--json] [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Administrateur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande**

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

<dt><code>--secret-name <em>SECRET_NAME</em></code></dt>
<dd>Nom de la valeur confidentielle ALB. Cette valeur est requise pour obtenir des informations sur une valeur confidentielle ALB spécifique dans le cluster.</dd>

<dt><code>--cert-crn <em>CERTIFICATE_CRN</em></code></dt>
<dd>CRN du certificat. Cette valeur est requise pour obtenir des informations sur toutes les valeurs confidentielles ALB correspondant à un CRN de certificat spécifique dans le cluster.</dd>

<dt><code>--json</code></dt>
<dd>Imprime le résultat de la commande au format JSON. Cette valeur est facultative.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemples** :

Exemple d'obtention d'informations relatives à une valeur confidentielle ALB :
```
ibmcloud ks alb-cert-get --cluster my_cluster --secret-name my_alb_secret
```
{: pre}

Exemple d'obtention d'informations relatives à toutes les valeurs confidentielles ALB correspondant à un CRN de certificat spécifié :
```
ibmcloud ks alb-cert-get --cluster my_cluster --cert-crn  crn:v1:staging:public:cloudcerts:us-south:a/06580c923e40314421d3b6cb40c01c68:0db4351b-0ee1-479d-af37-56a4da9ef30f:certificate:4bc35b7e0badb304e60aef00947ae7ff
```
{: pre}

</br>
### Version bêta : ibmcloud ks alb-cert-rm
{: #cs_alb_cert_rm}

Si vous avez importé un certificat de {{site.data.keyword.cloudcerts_short}} dans l'ALB d'un cluster, supprimez la valeur 'secret' du cluster.
{: shortdesc}

Pour rester dans les [limites de débit](https://cloud.ibm.com/apidocs/certificate-manager#rate-limiting) définies par {{site.data.keyword.cloudcerts_short}}, patientez au moins 45 secondes entre des commandes `alb-cert-rm` successives.
{: note}

```
ibmcloud ks alb-cert-rm --cluster CLUSTER [--secret-name SECRET_NAME] [--cert-crn CERTIFICATE_CRN] [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Administrateur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande**

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

<dt><code>--secret-name <em>SECRET_NAME</em></code></dt>
<dd>Nom de la valeur confidentielle ALB. Cette valeur est requise pour retirer une valeur confidentielle ALB spécifique dans le cluster.</dd>

<dt><code>--cert-crn <em>CERTIFICATE_CRN</em></code></dt>
<dd>CRN du certificat. Cette valeur est requise pour retirer toutes les valeurs confidentielles ALB correspondant à un CRN de certificat spécifique dans le cluster.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>

</dl>

**Exemples** :

Exemple de retrait d'une valeur confidentielle ALB :
```
ibmcloud ks alb-cert-rm --cluster my_cluster --secret-name my_alb_secret
```
{: pre}

Exemple de retrait de toutes les valeurs confidentielles ALB correspondant à un CRN de certificat spécifié :
```
ibmcloud ks alb-cert-rm --cluster my_cluster --cert-crn crn:v1:staging:public:cloudcerts:us-south:a/06580c923e40314421d3b6cb40c01c68:0db4351b-0ee1-479d-af37-56a4da9ef30f:certificate:4bc35b7e0badb304e60aef00947ae7ff
```
{: pre}

</br>
### ibmcloud ks alb-certs
{: #cs_alb_certs}

Affichez la liste des certificats que vous avez importés de votre instance {{site.data.keyword.cloudcerts_long_notm}} vers les équilibreurs de charge d'application (ALB) dans un cluster.
{: shortdesc}

```
ibmcloud ks alb-certs --cluster CLUSTER [--json] [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Administrateur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande**

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

<dt><code>--json</code></dt>
<dd>Imprime le résultat de la commande au format JSON. Cette valeur est facultative.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks alb-certs --cluster my_cluster
```
{: pre}

</br>
### ibmcloud ks alb-configure
{: #cs_alb_configure}

Activation ou désactivation d'un équilibreur de charge ALB dans votre cluster standard.
{: shortdesc}

Vous pouvez utiliser cette commande pour :
* Activez un ALB privé par défaut. Lorsque vous créez un cluster, un ALB privé par défaut est créé pour vous dans chaque zone où se trouvent des noeuds worker et un sous-réseau privé disponible, mais les ALB privés par défaut ne sont pas activés. Toutefois, notez que tous les ALB publics par défaut sont automatiquement activés et que tous les ALB publics ou privés que vous créez à l'aide de la commande `ibmcloud ks alb-create` sont également activés par défaut. 
* Activez un ALB que vous avez précédemment désactivé. 
* Désactivez un ALB sur un ancien VLAN après avoir créé un ALB sur un nouveau VLAN. Pour plus d'informations, voir [Déplacement d'équilibreurs de charge d'application d'un VLAN à un autre](/docs/containers?topic=containers-ingress#migrate-alb-vlan).
* Désactivez le déploiement d'équilibreur de charge d'application fourni par IBM de manière à pouvoir déployer votre propre contrôleur Ingress et optimiser l'enregistrement DNS pour le sous-domaine Ingress fourni par IBM ou le service d'équilibreur de charge qui est utilisé pour exposer le contrôleur Ingress. 

```
ibmcloud ks alb-configure --albID ALB_ID [--enable] [--user-ip USER_IP] [--disable] [--disable-deployment] [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Editeur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code><em>--albID </em>ALB_ID</code></dt>
<dd>ID d'un équilibreur de charge ALB. Exécutez la commande <code>ibmcloud ks albs <em>--cluster </em>CLUSTER</code> pour afficher les ID des équilibreurs de charge ALB d'un cluster. Cette valeur est obligatoire.</dd>

<dt><code>--enable</code></dt>
<dd>Incluez cet indicateur pour activer un équilibreur de charge ALB dans un cluster.</dd>

<dt><code>--disable</code></dt>
<dd>Incluez cet indicateur pour désactiver un équilibreur de charge ALB dans un cluster. <p class="note">Si vous désactivez un ALB, l'adresse IP qui était utilisée par celui-ci revient dans le pool d'adresses IP portables disponibles de sorte qu'un autre service puisse l'utiliser. Si par la suite vous essayez de réactiver l'ALB, celui-ci peut émettre une erreur si l'adresse IP qu'il utilisait précédemment est désormais utilisée par un autre service. Vous pouvez arrêter l'exécution de l'autre service ou spécifier une autre adresse IP à utiliser lorsque vous réactivez l'ALB. </p></dd>

<dt><code>--disable-deployment</code></dt>
<dd>Incluez cet indicateur pour désactiver le déploiement de l'équilibreur de charge ALB fourni par IBM. Il ne supprime pas l'enregistrement DNS du sous-domaine Ingress fourni par IBM ou le service d'équilibreur de charge utilisé pour exposer le contrôleur Ingress.</dd>

<dt><code>--user-ip <em>USER_IP</em></code></dt>
<dd>Facultatif : si vous activez l'ALB avec l'indicateur <code>--enable</code>, vous pouvez spécifier une adresse IP qui se trouve sur un VLAN dans la zone où l'ALB a été créé. L'ALB est activé avec et utilisé cette adresse IP publique ou privée. Notez que cette adresse IP ne doit pas être utilisée par un autre équilibreur de charge ou ALB dans le cluster. Si aucune adresse IP n'est fournie, l'ALB est déployé avec une adresse IP privée ou publique issue du sous-réseau privé ou public portable qui a été mis à disposition automatiquement lorsque vous avez créé le cluster, ou avec l'adresse IP privée ou publique que vous avez affectée à l'ALB.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemples** :

Exemple d'activation d'un équilibreur de charge ALB :
```
ibmcloud ks alb-configure --albID private-cr18a61a63a6a94b658596aa93a087aaa9-alb1 --enable
```
{: pre}

Exemple d'activation d'un équilibreur de charge ALB avec une adresse IP fournie par l'utilisateur :
```
ibmcloud ks alb-configure --albID private-cr18a61a63a6a94b658596aa93a087aaa9-alb1 --enable --user-ip user_ip
```
{: pre}

Exemple de désactivation d'un équilibreur de charge ALB :
```
ibmcloud ks alb-configure --albID public-cr18a61a63a6a94b658596aa93a087aaa9-alb1 --disable
```
{: pre}

</br>

### ibmcloud ks alb-create
{: #cs_alb_create}

Permet de créer un ALB public ou privé dans une zone. L'ALB que vous créez est activé par défaut.
{: shortdesc}

```
ibmcloud ks alb-create --cluster CLUSTER --type PUBLIC|PRIVATE --zone ZONE --vlan VLAN_ID [--user-ip IP] [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Editeur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster.</dd>

<dt><code>--type<em> PUBLIC|PRIVATE</em></code></dt>
<dd>Type d'ALB : <code>public</code> ou <code>private</code>.</dd>

<dt><code>--zone <em>ZONE</em></code></dt>
<dd>Zone dans laquelle créer l'ALB. </dd>

<dt><code>--vlan <em>VLAN_ID</em></code></dt>
<dd>ID du VLAN sur lequel créer l'ALB. Ce VLAN doit correspondre au <code>type</code> de l'ALB et doit figurer dans la même <code>zone</code> que l'ALB que vous souhaitez créer. </dd>

<dt><code>--user-ip <em>IP</em></code></dt>
<dd>Facultatif : adresse IP à affecter à l'ALB. Cette adresse IP doit figurer sur le <code>VLAN</code> que vous avez spécifié et doit figurer dans la même <code>zone</code> que l'ALB que vous souhaitez créer. Notez que cette adresse IP ne doit pas être utilisée par un autre équilibreur de charge ou ALB dans le cluster. </dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks alb-create --cluster mycluster --type public --zone dal10 --vlan 2234945 --user-ip 1.1.1.1
```
{: pre}

</br>

### ibmcloud ks alb-get
{: #cs_alb_get}

Affichez les détails d'un équilibreur de charge d'application Ingress dans un cluster.
{: shortdesc}

```
ibmcloud ks alb-get --albID ALB_ID [--json] [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Afficheur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code><em>--albID </em>ALB_ID</code></dt>
<dd>ID d'un équilibreur de charge ALB. Exécutez la commande <code>ibmcloud ks albs --cluster <em>CLUSTER</em></code> pour afficher les ID des équilibreurs de charge ALB d'un cluster. Cette valeur est obligatoire.</dd>

<dt><code>--json</code></dt>
<dd>Imprime le résultat de la commande au format JSON. Cette valeur est facultative.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks alb-get --albID public-cr18a61a63a6a94b658596aa93a087aaa9-alb1
```
{: pre}

</br>
### ibmcloud ks alb-rollback
{: #cs_alb_rollback}

Si vos pods ALB ont été récemment mis à jour, mais qu'une configuration personnalisée de vos ALB est affectée par la dernière version, vous pouvez rétromigrer la mise à jour à la version que vos pods ALB utilisaient avant. Tous les pods d'équilibreur de charge d'application de votre cluster reviennent à leur état précédent.
{: sortdesc}

Après avoir annulé une mise à jour, les mises à jour automatiques des pods ALB sont désactivées. Pour réactiver les mises à jour automatiques, utilisez la [commande `alb-autoupdate-enable`](#cs_alb_autoupdate_enable).

```
ibmcloud ks alb-rollback --cluster CLUSTER
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Editeur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

</br>
### ibmcloud ks alb-types
{: #cs_alb_types}

Répertoriez les types d'équilibreur de charge d'application Ingress pris en charge dans la région.
{: shortdesc}

```
ibmcloud ks alb-types [--json] [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Afficheur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--json</code></dt>
<dd>Imprime le résultat de la commande au format JSON. Cette valeur est facultative.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

</br>

### ibmcloud ks alb-update
{: #cs_alb_update}

Forcez une mise à jour des pods d'équilibreur de charge d'application Ingress dans le cluster vers la dernière version.
{: shortdesc}

Si les mises à jour automatiques du module complémentaire ALB Ingress sont désactivées et que vous souhaitez le mettre à jour, vous pouvez forcer une mise à jour à usage unique pour vos pods ALB. Lorsque vous optez pour une mise à jour manuelle du module complémentaire, les pods ALB du cluster sont mis à jour à la dernière version. Vous ne pouvez pas mettre à jour un équilibreur de charge ALB individuel ou choisir la version vers laquelle mettre à jour le module complémentaire. Les mises à jour automatiques restent désactivées.

Lorsque vous mettez à jour une version Kubernetes principale ou secondaire de votre cluster, IBM effectue les modifications nécessaires dans le déploiement Ingress, mais ne change pas la version de votre module complémentaire ALB Ingress. Vous êtes chargé de vérifier la compatibilité des dernières versions de Kubernetes et des images de votre module complémentaire ALB Ingress.

```
ibmcloud ks alb-update --cluster CLUSTER
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Editeur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

</br>
### ibmcloud ks albs
{: #cs_albs}

Permet de répertorier les ID ALB Ingress dans un cluster et de voir si une mise à jour pour les pods d'ALB est disponible.
{: shortdesc}

Si aucun ID ALB n'est renvoyé, le cluster n'a pas de sous-réseau portable. Vous pouvez [créer](#cs_cluster_subnet_create) ou [ajouter](#cs_cluster_subnet_add) des sous-réseaux à un cluster.

```
ibmcloud ks albs --cluster CLUSTER [--json] [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Afficheur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code><em>--cluster </em>CLUSTER</code></dt>
<dd>Nom ou ID du cluster sur lequel répertorier les équilibreurs de charge ALB disponibles. Cette valeur est obligatoire.</dd>

<dt><code>--json</code></dt>
<dd>Imprime le résultat de la commande au format JSON. Cette valeur est facultative.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks albs --cluster my_cluster
```
{: pre}

<br />



## Commandes de l'infrastructure
{: #infrastructure_commands}

### ibmcloud ks credential-get
{: #cs_credential_get}

Si vous configurez votre compte {{site.data.keyword.Bluemix_notm}} pour utiliser différentes données d'identification afin d'accéder au portefeuille de l'infrastructure IBM Cloud (SoftLayer), obtenez le nom d'utilisateur de l'infrastructure pour la région et le groupe de ressources que vous ciblez actuellement.
{: shortdesc}

```
ibmcloud ks credential-get --region REGION [-s] [--json]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Afficheur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--region <em>REGION</em></code></dt>
<dd>Indiquez une région. Pour afficher la liste des régions disponibles, exécutez la commande <code>ibmcloud ks regions</code>.</dd>

<dt><code>--json</code></dt>
<dd>Imprime le résultat de la commande au format JSON. Cette valeur est facultative.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks credential-get --region us-south
```
{: pre}

</br>
### ibmcloud ks credential-set
{: #cs_credentials_set}

Permet de définir des données d'identification pour un groupe de ressources et une région qui vous permettent d'accéder au portefeuille de l'infrastructure IBM Cloud (SoftLayer) via votre compte {{site.data.keyword.Bluemix_notm}}.
{: shortdesc}

Si vous disposez d'un compte de paiement à la carte {{site.data.keyword.Bluemix_notm}}, vous avez accès au portefeuille d'infrastructure IBM Cloud (SoftLayer) par défaut. Cependant, vous pouvez utiliser un autre compte d'infrastructure IBM Cloud (SoftLayer) dont vous disposez déjà pour commander l'infrastructure. Vous pouvez lier le compte de cette infrastructure à votre compte {{site.data.keyword.Bluemix_notm}} en utilisant cette commande.

Si les données d'identification de l'infrastructure IBM Cloud (SoftLayer) sont définies manuellement pour une région et un groupe de ressources, ces données sont utilisées pour commander l'infrastructure de tous les clusters au sein de cette région dans le groupe de ressources. Ces données d'identification sont utilisées pour déterminer des droits d'infrastructure, même s'il existe déjà une [clé d'API {{site.data.keyword.Bluemix_notm}} IAM](#cs_api_key_info) pour la région et le groupe de ressources. Si l'utilisateur dont les données d'identification sont stockées ne dispose pas des droits requis pour commander l'infrastructure, les actions liées à l'infrastructure, par exemple la création d'un cluster ou le rechargement d'un noeud worker risquent d'échouer.

Vous ne pouvez pas définir plusieurs données d'identification pour la même région et le même groupe de ressources {{site.data.keyword.containerlong_notm}}.

Avant d'utiliser cette commande, assurez-vous que l'utilisateur dont les données d'identification sont utilisées dispose des droits [{{site.data.keyword.containerlong_notm}} et des droits de l'infrastructure IBM Cloud (SoftLayer)](/docs/containers?topic=containers-users#users) requis.
{: important}

```
ibmcloud ks credential-set --infrastructure-api-key API_KEY --infrastructure-username USERNAME --region REGION [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Administrateur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--infrastructure-username <em>USERNAME</em></code></dt>
<dd>Nom d'utilisateur d'API du compte d'infrastructure IBM Cloud (SoftLayer). Cette valeur est obligatoire. Notez que le nom d'utilisateur de l'API d'infrastructure est différent de l'IBMid. Pour connaître le nom d'utilisateur de l'API d'infrastructure, voir [Gestion des clés d'API d'infrastructure classique](/docs/iam?topic=iam-classic_keys).</dd>

<dt><code>--infrastructure-api-key <em>API_KEY</em></code></dt>
<dd>Clé d'API du compte d'infrastructure IBM Cloud (SoftLayer). Cette valeur est obligatoire. Pour visualiser ou générer une clé d'API d'infrastructure, voir [Gestion des clés d'API d'infrastructure classique](/docs/iam?topic=iam-classic_keys).</dd>

<dt><code>--region <em>REGION</em></code></dt>
<dd>Indiquez une région. Pour afficher la liste des régions disponibles, exécutez la commande <code>ibmcloud ks regions</code>.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks credential-set --infrastructure-api-key <api_key> --infrastructure-username dbmanager --region us-south
```
{: pre}

</br>
### ibmcloud ks credential-unset
{: #cs_credentials_unset}

Permet de retirer les données d'identification pour un groupe de ressources et une région qui vous permettent d'accéder au portefeuille de l'infrastructure IBM Cloud (SoftLayer) via votre compte {{site.data.keyword.Bluemix_notm}}.
{: shortdesc}

Après avoir supprimé les données d'identification, la [clé d'API {{site.data.keyword.Bluemix_notm}} IAM](#cs_api_key_info) est utilisée pour commander des ressources dans l'infrastructure IBM Cloud (SoftLayer).

```
ibmcloud ks credential-unset --region REGION [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Administrateur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--region <em>REGION</em></code></dt>
<dd>Indiquez une région. Pour afficher la liste des régions disponibles, exécutez la commande <code>ibmcloud ks regions</code>.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks credential-unset --region us-south
```
{: pre}


</br>
### ibmcloud ks infra-permissions-get
{: #infra_permissions_get}

Permet de vérifier si les droits d'infrastructure suggérés ou requis sont manquants dans les données d'identification qui permettent [d'accéder au portefeuille de l'infrastructure IBM Cloud (SoftLayer)](/docs/containers?topic=containers-users#api_key) pour le groupe de ressources et la région ciblés.
{: shortdesc}

**Que signifient les droits d'infrastructure requis (`required`) et suggérés (`suggested`) ?**<br>
Si des droits sont manquants dans les données d'identification de l'infrastructure pour une région et un groupe de ressources, la sortie de cette commande renvoie une liste de droits requis (`required`) et suggérés (`suggested`). 
*   **Requis** : Ces droits sont nécessaires pour commander et gérer des ressources d'infrastructure, telles que des noeuds worker. Si l'un de ces droits est manquant dans les données d'identification de l'infrastructure, les actions courantes telles que `worker-reload` peuvent échouer pour tous les clusters dans la région et le groupe de ressources. 
*   **Suggérés** : Il est utile d'inclure ces droits dans vos droits d'infrastructure et ils peuvent s'avérer nécessaires dans certains cas d'utilisation. Par exemple, le droit d'infrastructure `Ajouter la fonction de calcul avec le port réseau public` est suggéré car si vous souhaitez une mise en réseau public, vous aurez besoin de ce droit. Toutefois, si votre cas d'utilisation est un cluster sur le VLAN privé uniquement, le droit n'est pas nécessaire ; il n'est donc pas considéré comme `requis`.

Pour obtenir une liste de cas d'utilisation courants par droit, voir [Rôles d'infrastructure](/docs/containers?topic=containers-access_reference#infra).

**Que se passe-t-il lorsque je vois un droit d'infrastructure que je ne trouve pas dans la console ou le tableau [Rôles d'infrastructure](/docs/containers?topic=containers-access_reference#infra) ?**<br>
Les droits `Cas de support` sont gérés dans une autre partie de la console que les droits d'infrastructure. Voir l'étape 8 de la rubrique [Personnalisation des droits d'infrastructure](/docs/containers?topic=containers-users#infra_access).

**Quels sont les droits d'infrastructure que je peux affecter ?**<br>
Si les règles de votre entreprise en matière de droits sont très strictes, vous devrez peut-être limiter les droits `suggérés` pour le cas d'utilisation de votre cluster. Sinon, assurez-vous que vos données d'identification de l'infrastructure pour la région et le groupe de ressources incluent tous les droits `requis` et `suggérés`. 

Pour la plupart des cas d'utilisation, [configurez la clé d'API](/docs/containers?topic=containers-users#api_key) pour la région et le groupe de ressources avec les droits d'infrastructure appropriés. Si vous devez utiliser un autre compte d'infrastructure, différent de votre compte actuel, [configurez manuellement des données d'identification](/docs/containers?topic=containers-users#credentials).

**Comment puis-je contrôler les actions que les utilisateurs peuvent effectuer ?**<br>
Une fois les données d'identification de l'infrastructure configurées, vous pouvez contrôler les actions que vos utilisateurs peuvent effectuer en affectant à ces derniers des [rôles de plateforme {{site.data.keyword.Bluemix_notm}}](/docs/containers?topic=containers-access_reference#iam_platform) IAM.

```
ibmcloud ks infra-permissions-get --region REGION [--json] [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Afficheur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--region <em>REGION</em></code></dt>
<dd>Indiquez une région. Pour afficher la liste des régions disponibles, exécutez la commande <code>ibmcloud ks regions</code>. Cette valeur est obligatoire.</dd>

<dt><code>--json</code></dt>
<dd>Imprime le résultat de la commande au format JSON. Cette valeur est facultative.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks infra-permissions-get --region us-south
```
{: pre}

Exemple de sortie :
```
Droits de l'agent virtuel manquants

Ajouter un serveur   suggéré
Annuler un serveur          suggéré
Afficher les détails du serveur virtuel   requis

Droits de l'agent physique manquants

Aucune modification n'est recommandée ou requise.

Droits d'accès réseau manquants

Aucune modification n'est recommandée ou requise.

Droits d'accès de stockage manquants

Ajouter du stockage         requis
Gérer du stockage requis
```
{: screen}


</br>
### ibmcloud ks machine-types
{: #cs_machine_types}

Permet d'afficher la liste des types de machine worker, ou versions, disponibles pour vos noeuds worker. Les types de machine varient en fonction de la zone.
{:shortdesc}

Chaque type de machine inclut la quantité d'UC virtuelles, de mémoire et d'espace disque pour chaque noeud worker dans le cluster. Par défaut, le répertoire du disque de stockage secondaire dans lequel sont stockées toutes les données des conteneurs, est chiffré avec le chiffrement LUKS. Si l'option `disable-disk-encrypt` est incluse lors de la création du cluster, les données de l'environnement d'exécution de conteneur de l'hôte ne sont pas chiffrées. [En savoir plus sur le chiffrement](/docs/containers?topic=containers-security#encrypted_disk).

Vous pouvez mettre à disposition votre noeud worker en tant que machine virtuelle sur un matériel dédié ou partagé, ou en tant que machine physique sur un serveur bare metal. [En savoir plus sur les options correspondant à votre type de machine](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes).

```
ibmcloud ks machine-types --zone ZONE [--json] [-s]
```
{: pre}

**Droits minimum requis** : aucun

**Options de commande** :
<dl>
<dt><code>--zone <em>ZONE</em></code></dt>
<dd>Indiquez la zone dans laquelle vous souhaitez afficher la liste des types de machine disponibles. Cette valeur est obligatoire. Passez en revue les [zones disponibles](/docs/containers?topic=containers-regions-and-zones#zones).</dd>

<dt><code>--json</code></dt>
<dd>Imprime le résultat de la commande au format JSON. Cette valeur est facultative.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks machine-types --zone dal10
```
{: pre}

</br>
### ibmcloud ks vlan-spanning-get
{: #cs_vlan_spanning_get}

Affichez le statut de la fonction Spanning VLAN d'un compte d'infrastructure IBM Cloud (SoftLayer). La fonction Spanning VLAN permet à toutes les unités d'un même compte de communiquer les unes avec les autres par le biais du réseau privé, indépendamment du VLAN qui leur est affecté.
{: shortdesc}

```
ibmcloud ks vlan-spanning-get --region REGION [--json] [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Afficheur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--region <em>REGION</em></code></dt>
<dd>Indiquez une région. Pour afficher la liste des régions disponibles, exécutez la commande <code>ibmcloud ks regions</code>.</dd>

<dt><code>--json</code></dt>
<dd>Imprime le résultat de la commande au format JSON. Cette valeur est facultative.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks vlan-spanning-get --region us-south
```
{: pre}

</br>
### ibmcloud ks <ph class="ignoreSpelling">vlans</ph>
{: #cs_vlans}

Répertoriez les VLAN publics et privés disponibles pour une zone dans votre compte d'infrastructure IBM Cloud (SoftLayer). Pour répertorier ces réseaux, vous devez disposer d'un compte payant.
{: shortdesc}

```
ibmcloud ks vlans --zone ZONE [--all] [--json] [-s]
```
{: pre}

**Droits minimum requis** :
* Pour afficher les VLAN auxquels est connecté le cluster dans une zone : rôle de plateforme **Afficheur** pour le cluster dans {{site.data.keyword.containerlong_notm}}
* Pour afficher la liste des VLAN disponibles dans une zone : rôle de plateforme **Afficheur** pour la région dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--zone <em>ZONE</em></code></dt>
<dd>Indiquez la zone où répertorier vos VLAN privés et publics. Cette valeur est obligatoire. Passez en revue les [zones disponibles](/docs/containers?topic=containers-regions-and-zones#zones).</dd>

<dt><code>--all</code></dt>
<dd>Répertorie tous les VLAN disponibles. Par défaut, les VLAN sont filtrés pour n'afficher que ceux qui sont valides. Pour être valide, un VLAN doit être associé à l'infrastructure qui peut héberger un noeud worker avec un stockage sur disque local.</dd>

<dt><code>--json</code></dt>
<dd>Imprime le résultat de la commande au format JSON. Cette valeur est facultative.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks vlans --zone dal10
```
{: pre}

<br />


## Commandes de consignation
{: #logging_commands}

### ibmcloud ks logging-autoupdate-disable
{: #cs_log_autoupdate_disable}

Désactivez les mises à jour automatiques de tous les pods Fluentd d'un cluster.
{: shortdesc}

Désactivez les mises à jour automatiques de vos pods Fluentd dans un cluster spécifique. Lorsque vous mettez à jour une version Kubernetes principale ou secondaire de votre cluster, IBM effectue les modifications nécessaires dans l'élément configmap de Fluentd, mais ne change pas la version de votre module complémentaire Fluentd pour la consignation. Vous êtes chargé de vérifier la compatibilité des dernières versions de Kubernetes et des images de votre module complémentaire.


```
ibmcloud ks logging-autoupdate-disable --cluster CLUSTER
```
{: pre}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster dans lequel vous souhaitez désactiver les mises à jour automatiques du module complémentaire Fluentd. Cette valeur est obligatoire.</dd>
</dl>

</br>

### ibmcloud ks logging-autoupdate-enable
{: #cs_log_autoupdate_enable}

Active les mises à jour automatiques de vos pods Fluentd dans un cluster spécifique. Les pods Fluentd sont automatiquement mis à jour lorsqu'une nouvelle version est disponible.
{: shortdesc}

```
ibmcloud ks logging-autoupdate-enable --cluster CLUSTER
```
{: pre}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster dans lequel vous souhaitez activer les mises à jour automatiques du module complémentaire Fluentd. Cette valeur est obligatoire.</dd>
</dl>

</br>

### ibmcloud ks logging-autoupdate-get
{: #cs_log_autoupdate_get}

Déterminez si vos pods Fluentd sont configurés pour se mettre à jour automatiquement dans un cluster.
{: shortdesc}

```
ibmcloud ks logging-autoupdate-get --cluster CLUSTER
```
{: pre}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster dans lequel vous souhaitez vérifier si les mises à jour automatiques du module complémentaire Fluentd sont activées. Cette valeur est obligatoire.</dd>
</dl>
</br>
### ibmcloud ks logging-collect
{: #cs_log_collect}

Effectuez une demande d'instantané de vos journaux à un moment précis et stockez les journaux dans un compartiment {{site.data.keyword.cos_full_notm}}.
{: shortdesc}

```
ibmcloud ks logging-collect --cluster CLUSTER --cos-bucket BUCKET_NAME --cos-endpoint ENDPOINT --hmac-key-id HMAC_KEY_ID --hmac-key HMAC_KEY --type LOG_TYPE [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Administrateur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster pour lequel vous souhaitez créer un instantané.</dd>

<dt><code>--cos-bucket <em>BUCKET_NAME</em></code></dt>
<dd>Nom du compartiment {{site.data.keyword.cos_short}} dans lequel vous souhaitez stocker vos journaux.</dd>

<dt><code>--cos-endpoint <em>ENDPOINT</em></code></dt>
<dd>Noeud final {{site.data.keyword.cos_short}} de centre de données régional, inter-régional ou unique pour le compartiment dans lequel vous stockez vos journaux. Pour connaître les noeuds finaux disponibles, voir [Noeuds finaux et emplacements de stockage](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-endpoints) dans la documentation {{site.data.keyword.cos_short}} documentation.</dd>

<dt><code>--hmac-key-id <em>HMAC_KEY_ID</em></code></dt>
<dd>ID unique de vos données d'identification HMAC pour votre instance {{site.data.keyword.cos_short}}. </dd>

<dt><code>--hmac-key <em>HMAC_KEY</em></code></dt>
<dd>Clé HMAC pour votre instance {{site.data.keyword.cos_short}}.</dd>

<dt><code>--type <em>LOG_TYPE</em></code></dt>
<dd>Facultatif : Type de journaux dont vous souhaitez créer une image instantanée. Actuellement, la valeur par défaut `master` est la seule option possible.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple de commande** :

```
ibmcloud ks logging-collect --cluster mycluster --cos-bucket mybucket --cos-endpoint s3-api.us-geo.objectstorage.softlayer.net --hmac-key-id e2e7f5c9fo0144563c418dlhi3545m86 --hmac-key c485b9b9fo4376722f692b63743e65e1705301ab051em96j
```
{: pre}

**Exemple de sortie** :

```
There is no specified log type. The default master will be used.
Submitting log collection request for master logs for cluster mycluster...
  OK
  The log collection request was successfully submitted. To view the status of the request run ibmcloud ks logging-collect-status mycluster.
```
{: screen}

</br>
### ibmcloud ks logging-collect-status
{: #cs_log_collect_status}

Vérifiez le statut de demande d'instantané de la collecte de journaux pour votre cluster.
{: shortdesc}

```
ibmcloud ks logging-collect-status --cluster CLUSTER [--json] [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Administrateur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster pour lequel vous souhaitez créer un instantané. Cette valeur est obligatoire.</dd>

<dt><code>--json</code></dt>
<dd>Imprime le résultat de la commande au format JSON. Cette valeur est facultative.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple de commande** :

```
ibmcloud ks logging-collect-status --cluster mycluster
```
{: pre}

**Exemple de sortie** :

```
Getting the status of the last log collection request for cluster mycluster...
  OK
  State     Start Time             Error   Log URLs
  success   2018-09-18 16:49 PDT   - s3-api.us-geo.objectstorage.softlayer.net/mybucket/master-0-0862ae70a9ae6c19845ba3pc0a2a6o56-1297318756.tgz
  s3-api.us-geo.objectstorage.softlayer.net/mybucket/master-1-0862ae70a9ae6c19845ba3pc0a2a6o56-1297318756.tgz
  s3-api.us-geo.objectstorage.softlayer.net/mybucket/master-2-0862ae70a9ae6c19845ba3pc0a2a6o56-1297318756.tgz
```
{: screen}

</br>
### ibmcloud ks logging-config-create
{: #cs_logging_create}

Créez une configuration de consignation. Vous pouvez utiliser cette commande pour acheminer des journaux de conteneurs, applications, noeuds worker, clusters Kubernetes et équilibreurs de charge d'application Ingress à {{site.data.keyword.loganalysisshort_notm}} ou à un serveur syslog externe.
{: shortdesc}

```
ibmcloud ks logging-config-create --cluster CLUSTER --logsource LOG_SOURCE --type LOG_TYPE [--namespace KUBERNETES_NAMESPACE] [--hostname LOG_SERVER_HOSTNAME_OR_IP] [--port LOG_SERVER_PORT] [--space CLUSTER_SPACE] [--org CLUSTER_ORG] [--app-containers CONTAINERS] [--app-paths PATHS_TO_LOGS] [--syslog-protocol PROTOCOL]  [--json] [--skip-validation] [--force-update][-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Editeur** pour le cluster pour toutes les sources de journal sauf `kube-audit` et rôle de plateforme **Administrateur** pour le cluster pour la source de journal `kube-audit`

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster.</dd>

<dt><code>--logsource <em>LOG_SOURCE</em></code></dt>
<dd>Source de journal pour laquelle activer l'acheminement des journaux. Cet argument prend en charge une liste séparée par des virgules de sources de journal auxquelles appliquer la configuration. Valeurs admises : <code>container</code>, <code>application</code>, <code>worker</code>, <code>kubernetes</code>, <code>storage</code>, <code>ingress</code> et <code>kube-audit</code>. Si vous ne fournissez pas de source de journal, des configurations sont créées pour <code>container</code> et <code>ingress</code>.</dd>

<dt><code>--type <em>LOG_TYPE</em></code></dt>
<dd>Destination de transfert de vos journaux. Les options possibles sont : <code>ibm</code> pour transférer vos journaux vers {{site.data.keyword.loganalysisshort_notm}} et <code>syslog</code> pour les transférer vers un serveur externe.<p class="deprecated">{{site.data.keyword.loganalysisshort_notm}} est obsolète. Cette option de commande est prise en charge jusqu'au 30 septembre 2019.</p></dd>

<dt><code>--namespace <em>KUBERNETES_NAMESPACE</em></code></dt>
<dd>Espace de nom Kubernetes depuis lequel vous désirez acheminer des journaux. L'acheminement des journaux n'est pas pris en charge pour les espaces de nom Kubernetes <code>ibm-system</code> et <code>kube-system</code>. Cette valeur est facultative et n'est valide que pour la source de journal conteneur. Si vous n'indiquez pas d'espace de nom, tous les espaces de nom du cluster utilisent cette configuration.</dd>

<dt><code>--hostname <em>LOG_SERVER_HOSTNAME</em></code></dt>
<dd>Si le type de consignation correspond à <code>syslog</code>, nom d'hôte ou adresse ID du serveur collecteur de journal. Cette valeur est obligatoire pour <code>syslog</code>. Si le type de consignation correspond à <code>ibm</code>, URL d'ingestion {{site.data.keyword.loganalysislong_notm}}. Vous trouverez [ici](/docs/services/CloudLogAnalysis?topic=cloudloganalysis-log_ingestion#log_ingestion_urls) la liste des URL d'ingestion disponibles. Si vous ne spécifiez pas d'URL d'ingestion, le noeud final de la région où votre cluster a été créé est utilisé.</dd>

<dt><code>--port <em>LOG_SERVER_PORT</em></code></dt>
<dd>Port du serveur collecteur de journal. Cette valeur est facultative. Si vous ne spécifiez pas de port, le port standard <code>514</code> est utilisé pour <code>syslog</code> et le port standard <code>9091</code> pour <code>ibm</code>.</dd>

<dt><code>--space <em>CLUSTER_SPACE</em></code></dt>
<dd>Facultatif : nom de l'espace Cloud Foundry auquel envoyer les journaux. Cette valeur est facultative et n'est valide que pour le type de journal <code>ibm</code>. Si vous ne spécifiez pas d'espace, les journaux sont envoyés au niveau du compte. Si vous en indiquez un, vous devez également spécifier une organisation.</dd>

<dt><code>--org <em>CLUSTER_ORG</em></code></dt>
<dd>Facultatif : nom de l'organisation Cloud Foundry où réside l'espace. Cette valeur n'est valide que pour le type de journal <code>ibm</code> et est obligatoire si vous avez spécifié un espace.</dd>

<dt><code>--app-paths</code></dt>
<dd>Chemin dans le conteneur utilisé par les applications pour la consignation. Pour transférer des journaux avec le type de source <code>application</code>, vous devez indiquer un chemin. Pour indiquer plusieurs chemins, utilisez une liste séparée par des virgules. Cette valeur est obligatoire pour la source de journal <code>application</code>. Exemple : <code>/var/log/myApp1/&ast;,/var/log/myApp2/&ast;</code></dd>

<dt><code>--syslog-protocol</code></dt>
<dd>Protocole de couche de transfert utilisé lorsque le type de consignation est <code>syslog</code>. Valeurs admises : <code>tcp</code>, <code>tls</code> et la valeur par défaut <code>udp</code>. Lors du transfert vers un serveur rsyslog avec le protocole <code>udp</code>, les journaux dont la taille est supérieure à 1 ko sont tronqués.</dd>

<dt><code>--app-containers</code></dt>
<dd>Pour transférer les journaux à partir d'une application, vous pouvez indiquer le nom du conteneur contenant votre application. Vous pouvez spécifier plusieurs conteneurs en utilisant une liste séparée par des virgules. Si aucun conteneur n'est indiqué, les journaux sont transférés à partir de tous les conteneurs contenant les chemins que vous avez fournis. Cette option n'est valide que pour la source de journal <code>application</code>.</dd>

<dt><code>--json</code></dt>
<dd>Imprime le résultat de la commande au format JSON. Cette valeur est facultative.</dd>

<dt><code>--skip-validation</code></dt>
<dd>Ignore la validation des noms d'organisation et d'espace lorsqu'ils sont spécifiés. Cette opération permet de réduire le temps de traitement, mais une configuration de consignation non valide ne transfère pas correctement les journaux. Cette valeur est facultative.</dd>

<dt><code>--force-update</code></dt>
<dd>Force la mise à jour de vos pods Fluentd à la version la plus récente. Fluentd doit être à la version la plus récente pour apporter des modifications à vos configurations de consignation.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemples** :

Exemple pour le type de journal `syslog` acheminé depuis une source de journal `container` sur le port par défaut 514 :
```
ibmcloud ks logging-config-create my_cluster --logsource container --namespace my_namespace  --hostname 169.xx.xxx.xxx --type syslog
```
{: pre}

Exemple pour le type de journal `syslog` qui achemine des journaux depuis une source `ingress` sur un port différent de celui par défaut :
```
ibmcloud ks logging-config-create --cluster my_cluster --logsource container --hostname 169.xx.xxx.xxx --port 5514 --type syslog
```
{: pre}

</br>
### ibmcloud ks logging-config-get
{: #cs_logging_get}

Affichez toutes les configurations d'acheminement de journaux d'un cluster ou filtrez les configurations de consignation en fonction de la source de journal.
{: shortdesc}

```
ibmcloud ks logging-config-get --cluster CLUSTER [--logsource LOG_SOURCE] [--json] [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Afficheur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

<dt><code>--logsource <em>LOG_SOURCE</em></code></dt>
<dd>Type de source de journal que vous voulez filtrer. Seules les configurations de consignation de cette source de journal dans le cluster sont renvoyées. Valeurs admises : <code>container</code>, <code>storage</code>, <code>application</code>, <code>worker</code>, <code>kubernetes</code>, <code>ingress</code> et <code>kube-audit</code>. Cette valeur est facultative.</dd>

<dt><code>--show-covering-filters</code></dt>
<dd>Affiche les filtres de consignation qui rendent obsolètes les filtres précédents.</dd>

<dt><code>--json</code></dt>
<dd>Imprime le résultat de la commande au format JSON. Cette valeur est facultative.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks logging-config-get --cluster my_cluster --logsource worker
```
{: pre}

</br>
### ibmcloud ks logging-config-refresh
{: #cs_logging_refresh}

Actualise la configuration de consignation pour le cluster. Cette commande actualise le jeton de journalisation de toute configuration de consignation qui achemine des données au niveau de l'espace dans votre cluster.
{: shortdesc}

```
ibmcloud ks logging-config-refresh --cluster CLUSTER [--force-update] [-s]
```

**Droits minimum requis** : rôle de plateforme **Editeur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

<dt><code>--force-update</code></dt>
<dd>Force la mise à jour de vos pods Fluentd à la version la plus récente. Fluentd doit être à la version la plus récente pour apporter des modifications à vos configurations de consignation.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :

  ```
  ibmcloud ks logging-config-refresh --cluster my_cluster
  ```
  {: pre}

</br>
### ibmcloud ks logging-config-rm
{: #cs_logging_rm}

Supprimez une configuration d'acheminement des journaux ou toutes les configurations de consignation d'un cluster. Ceci cesse l'acheminement des journaux à un serveur syslog distant ou à {{site.data.keyword.loganalysisshort_notm}}.
{: shortdesc}

```
ibmcloud ks logging-config-rm --cluster CLUSTER [--id LOG_CONFIG_ID] [--all] [--force-update] [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Editeur** pour le cluster pour toutes les sources de journal sauf `kube-audit` et rôle de plateforme **Administrateur** pour le cluster pour la source de journal `kube-audit`

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

<dt><code>--id <em>LOG_CONFIG_ID</em></code></dt>
<dd>Si vous souhaitez supprimer une seule configuration de consignation, ID de la configuration de consignation.</dd>

<dt><code>--all</code></dt>
<dd>Indicateur permettant de supprimer toutes les configurations de consignation dans un cluster.</dd>

<dt><code>--force-update</code></dt>
<dd>Force la mise à jour de vos pods Fluentd à la version la plus récente. Fluentd doit être à la version la plus récente pour apporter des modifications à vos configurations de consignation.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks logging-config-rm --cluster my_cluster --id f4bc77c0-ee7d-422d-aabf-a4e6b977264e
```
{: pre}

</br>
### ibmcloud ks logging-config-update
{: #cs_logging_update}

Mettez à jour les détails d'une configuration d'acheminement des journaux.
{: shortdesc}

```
ibmcloud ks logging-config-update --cluster CLUSTER --id LOG_CONFIG_ID --type LOG_TYPE  [--namespace NAMESPACE] [--hostname LOG_SERVER_HOSTNAME_OR_IP] [--port LOG_SERVER_PORT] [--space CLUSTER_SPACE] [--org CLUSTER_ORG] [--app-paths PATH] [--app-containers PATH] [--json] [--skipValidation] [--force-update] [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Editeur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

<dt><code>--id <em>LOG_CONFIG_ID</em></code></dt>
<dd>ID de configuration de consignation que vous souhaitez mettre à jour. Cette valeur est obligatoire.</dd>

<dt><code>--type <em>LOG_TYPE</em></code></dt>
<dd>Protocole de transfert de journal que vous souhaitez utiliser. Actuellement, <code>syslog</code> et <code>ibm</code> sont pris en charge. Cette valeur est obligatoire.</dd>

<dt><code>--namespace <em>NAMESPACE</em></code>
<dd>Espace de nom Kubernetes depuis lequel vous désirez acheminer des journaux. L'acheminement des journaux n'est pas pris en charge pour les espaces de nom Kubernetes <code>ibm-system</code> et <code>kube-system</code>. Cette valeur n'est valide que pour la source de journal <code>container</code>. Si vous n'indiquez pas d'espace de nom, tous les espaces de nom du cluster utilisent cette configuration.</dd>

<dt><code>--hostname <em>LOG_SERVER_HOSTNAME</em></code></dt>
<dd>Si le type de consignation correspond à <code>syslog</code>, nom d'hôte ou adresse ID du serveur collecteur de journal. Cette valeur est obligatoire pour <code>syslog</code>. Si le type de consignation correspond à <code>ibm</code>, URL d'ingestion {{site.data.keyword.loganalysislong_notm}}. Vous trouverez [ici](/docs/services/CloudLogAnalysis?topic=cloudloganalysis-log_ingestion#log_ingestion_urls) la liste des URL d'ingestion disponibles. Si vous ne spécifiez pas d'URL d'ingestion, le noeud final de la région où votre cluster a été créé est utilisé.</dd>

<dt><code>--port <em>LOG_SERVER_PORT</em></code></dt>
<dd>Port du serveur collecteur de journal. Cette valeur est facultative lorsque le type de consignation est <code>syslog</code>. Si vous ne spécifiez pas de port, le port standard <code>514</code> est utilisé pour <code>syslog</code> et le port <code>9091</code> pour <code>ibm</code>.</dd>

<dt><code>--space <em>CLUSTER_SPACE</em></code></dt>
<dd>Facultatif : nom de l'espace auquel vous désirez envoyer les journaux. Cette valeur est facultative et n'est valide que pour le type de journal <code>ibm</code>. Si vous ne spécifiez pas d'espace, les journaux sont envoyés au niveau du compte. Si vous en indiquez un, vous devez également spécifier une organisation.</dd>

<dt><code>--org <em>CLUSTER_ORG</em></code></dt>
<dd>Facultatif : nom de l'organisation Cloud Foundry où réside l'espace. Cette valeur n'est valide que pour le type de journal <code>ibm</code> et est obligatoire si vous avez spécifié un espace.</dd>

<dt><code>--app-paths <em>PATH</em>,<em>PATH</em></code></dt>
<dd>Chemin de fichier absolu dans le conteneur, à partir duquel les journaux sont collectés. Les caractères génériques, tels que '/var/log/*.log', peuvent être utilisés, mais les modules glob récursifs, tels que '/var/log/**/test.log', ne peuvent pas être utilisés. Pour indiquer plusieurs chemins, utilisez une liste séparée par des virgules. Cette valeur est obligatoire lorsque vous spécifiez 'application' pour la source journal. </dd>

<dt><code>--app-containers <em>PATH</em>,<em>PATH</em></code></dt>
<dd>Chemin dans les conteneurs utilisés par les applications pour la consignation. Pour transférer des journaux avec le type de source <code>application</code>, vous devez indiquer un chemin. Pour indiquer plusieurs chemins, utilisez une liste séparée par des virgules. Exemple : <code>/var/log/myApp1/&ast;,/var/log/myApp2/&ast;</code></dd>

<dt><code>--json</code></dt>
<dd>Imprime le résultat de la commande au format JSON. Cette valeur est facultative.</dd>

<dt><code>--skipValidation</code></dt>
<dd>Ignore la validation des noms d'organisation et d'espace lorsqu'ils sont spécifiés. Cette opération permet de réduire le temps de traitement, mais une configuration de consignation non valide ne transfère pas correctement les journaux. Cette valeur est facultative.</dd>

<dt><code>--force-update</code></dt>
<dd>Force la mise à jour de vos pods Fluentd à la version la plus récente. Fluentd doit être à la version la plus récente pour apporter des modifications à vos configurations de consignation.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple pour le type de journal `ibm`** :

  ```
  ibmcloud ks logging-config-update my_cluster --id f4bc77c0-ee7d-422d-aabf-a4e6b977264e --type ibm
  ```
  {: pre}

**Exemple pour le type de journal `syslog`** :

  ```
  ibmcloud ks logging-config-update --cluster my_cluster --id f4bc77c0-ee7d-422d-aabf-a4e6b977264e --hostname localhost --port 5514 --type syslog
  ```
  {: pre}

</br>
### ibmcloud ks logging-filter-create
{: #cs_log_filter_create}

Filtrez les journaux transmis par votre configuration de journalisation.
{: shortdesc}

```
ibmcloud ks logging-filter-create --cluster CLUSTER --type LOG_TYPE [--logging-configs CONFIGS] [--namespace KUBERNETES_NAMESPACE] [--container CONTAINER_NAME] [--level LOGGING_LEVEL] [--message MESSAGE] [--regex-message MESSAGE] [--force-update] [--json] [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Editeur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster pour lequel créer un filtre de consignation. Cette valeur est obligatoire.</dd>

<dt><code>--type <em>LOG_TYPE</em></code></dt>
<dd>Type des journaux auquel vous voulez appliquer le filtre. Les types <code>all</code>, <code>container</code> et <code>host</code> sont actuellement pris en charge.</dd>

<dt><code>--logging-configs <em>CONFIGS</em></code></dt>
<dd>Liste séparée par des virgules contenant les ID de vos configurations de consignation. Si cette liste n'est pas fournie, le filtre s'applique à toutes les configurations de consignation du cluster qui sont transmises au filtre. Vous pouvez afficher les configurations de journal qui correspondent au filtre en utilisant l'indicateur <code>--show-matching-configs</code> avec la commande. Cette valeur est facultative.</dd>

<dt><code>--namespace <em>KUBERNETES_NAMESPACE</em></code></dt>
<dd>Espace de nom Kubernetes à partir duquel vous souhaitez filtrer les journaux. Cette valeur est facultative.</dd>

<dt><code>--container <em>CONTAINER_NAME</em></code></dt>
<dd>Nom du conteneur à partir duquel vous voulez filtrer les journaux. Cet indicateur s'applique uniquement lorsque vous utilisez le type de journal <code>container</code>. Cette valeur est facultative.</dd>

<dt><code>--level <em>LOGGING_LEVEL</em></code></dt>
<dd>Filtre les journaux dont le niveau est inférieur ou égal au niveau spécifié. Les valeurs admises, par ordre canonique, sont : <code>fatal</code>, <code>error</code>, <code>warn/warning</code>, <code>info</code>, <code>debug</code> et <code>trace</code>. Cette valeur est facultative. Par exemple, si vous avez filtré les journaux au niveau <code>info</code>, les niveaux <code>debug</code> et <code>trace</code> sont également filtrés. **Remarque** : vous pouvez utiliser cet indicateur uniquement si les messages de journal sont au format JSON et contiennent une zone de niveau. Exemple de sortie : <code>{"log": "hello", "level": "info"}</code></dd>

<dt><code>--message <em>MESSAGE</em></code></dt>
<dd>Filtre les journaux contenant un message particulier n'importe où dans le journal. Cette valeur est facultative. Exemple : les messages "Hello", "!" et "Hello, World!", s'appliqueront au journal "Hello, World!".</dd>

<dt><code>--regex-message <em>MESSAGE</em></code></dt>
<dd>Filtre les journaux qui contiennent un message particulier écrit sous forme d'expression régulière n'importe où dans le journal. Cette valeur est facultative. Exemple : le masque "hello [0-9]" s'appliquera à "hello 1", "hello 2" et "hello 9".</dd>

<dt><code>--force-update</code></dt>
<dd>Force la mise à jour de vos pods Fluentd à la version la plus récente. Fluentd doit être à la version la plus récente pour apporter des modifications à vos configurations de consignation.</dd>

<dt><code>--json</code></dt>
<dd>Imprime le résultat de la commande au format JSON. Cette valeur est facultative.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemples** :

L'exemple suivant permet de filtrer tous les journaux transmis à partir de conteneurs nommés `test-container` dans l'espace de nom par défaut dont le niveau est debug ou inférieur et dont le message de journal contient "GET request".
```
ibmcloud ks logging-filter-create --cluster example-cluster --type container --namespace default --container test-container --level debug --message "GET request"
```
{: pre}

L'exemple suivant permet de filtrer tous les journaux transférés, de niveau info ou inférieur, à partir d'un cluster spécifique. La sortie est renvoyée au format JSON.
```
ibmcloud ks logging-filter-create --cluster example-cluster --type all --level info --json
```
{: pre}

</br>
### ibmcloud ks logging-filter-get
{: #cs_log_filter_view}

Affichez une configuration de filtre de consignation.
{: shortdesc}

```
ibmcloud ks logging-filter-get --cluster CLUSTER [--id FILTER_ID] [--show-matching-configs] [--show-covering-filters] [--json] [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Afficheur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster à partir duquel vous souhaitez afficher les filtres. Cette valeur est obligatoire.</dd>

<dt><code>--id <em>FILTER_ID</em></code></dt>
<dd>ID du filtre de journal que vous désirez afficher.</dd>

<dt><code>--show-matching-configs</code></dt>
<dd>Affiche les configurations de consignation qui correspondent à la configuration que vous consultez actuellement. Cette valeur est facultative.</dd>

<dt><code>--show-covering-filters</code></dt>
<dd>Affiche les filtres de consignation qui rendent obsolètes les filtres précédents. Cette valeur est facultative.</dd>

<dt><code>--json</code></dt>
<dd>Imprime le résultat de la commande au format JSON. Cette valeur est facultative.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks logging-filter-get --cluster mycluster --id 885732 --show-matching-configs
```
{: pre}

</br>
### ibmcloud ks logging-filter-rm
{: #cs_log_filter_delete}

Supprimez un filtre de consignation.
{: shortdesc}

```
ibmcloud ks logging-filter-rm --cluster CLUSTER [--id FILTER_ID] [--all] [--force-update] [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Editeur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster duquel vous souhaitez supprimer un filtre.</dd>

<dt><code>--id <em>FILTER_ID</em></code></dt>
<dd>ID du filtre de journal à supprimer.</dd>

<dt><code>--all</code></dt>
<dd>Supprime tous vos filtres d'acheminement de journaux. Cette valeur est facultative.</dd>

<dt><code>--force-update</code></dt>
<dd>Force la mise à jour de vos pods Fluentd à la version la plus récente. Fluentd doit être à la version la plus récente pour apporter des modifications à vos configurations de consignation.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks logging-filter-rm --cluster mycluster --id 885732
```
{: pre}

</br>
### ibmcloud ks logging-filter-update
{: #cs_log_filter_update}

Mettez à jour un filtre de consignation.
{: shortdesc}

```
ibmcloud ks logging-filter-update --cluster CLUSTER --id FILTER_ID --type LOG_TYPE [--logging-configs CONFIGS] [--namespace KUBERNETES_NAMESPACE] [--container CONTAINER_NAME] [--level LOGGING_LEVEL] [--message MESSAGE] [--regex-message MESSAGE] [--force-update] [--json] [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Editeur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster pour lequel vous souhaitez mettre à jour un filtre de consignation. Cette valeur est obligatoire.</dd>

<dt><code>--id <em>FILTER_ID</em></code></dt>
<dd>ID du filtre de journal à mettre à jour.</dd>

<dt><code>--type <em>LOG_TYPE</em></code></dt>
<dd>Type des journaux auquel vous voulez appliquer le filtre. Les types <code>all</code>, <code>container</code> et <code>host</code> sont actuellement pris en charge.</dd>

<dt><code>--logging-configs <em>CONFIGS</em></code></dt>
<dd>Liste séparée par des virgules contenant les ID de vos configurations de consignation. Si cette liste n'est pas fournie, le filtre s'applique à toutes les configurations de consignation du cluster qui sont transmises au filtre. Vous pouvez afficher les configurations de journal qui correspondent au filtre en utilisant l'indicateur <code>--show-matching-configs</code> avec la commande. Cette valeur est facultative.</dd>

<dt><code>--namespace <em>KUBERNETES_NAMESPACE</em></code></dt>
<dd>Espace de nom Kubernetes à partir duquel vous souhaitez filtrer les journaux. Cette valeur est facultative.</dd>

<dt><code>--container <em>CONTAINER_NAME</em></code></dt>
<dd>Nom du conteneur à partir duquel vous voulez filtrer les journaux. Cet indicateur s'applique uniquement lorsque vous utilisez le type de journal <code>container</code>. Cette valeur est facultative.</dd>

<dt><code>--level <em>LOGGING_LEVEL</em></code></dt>
<dd>Filtre les journaux dont le niveau est inférieur ou égal au niveau spécifié. Les valeurs admises, par ordre canonique, sont : <code>fatal</code>, <code>error</code>, <code>warn/warning</code>, <code>info</code>, <code>debug</code> et <code>trace</code>. Cette valeur est facultative. Par exemple, si vous avez filtré les journaux au niveau <code>info</code>, les niveaux <code>debug</code> et <code>trace</code> sont également filtrés. **Remarque** : vous pouvez utiliser cet indicateur uniquement si les messages de journal sont au format JSON et contiennent une zone de niveau. Exemple de sortie : <code>{"log": "hello", "level": "info"}</code></dd>

<dt><code>--message <em>MESSAGE</em></code></dt>
<dd>Filtre les journaux contenant un message particulier n'importe où dans le journal. Cette valeur est facultative. Exemple : les messages "Hello", "!" et "Hello, World!", s'appliqueront au journal "Hello, World!".</dd>

<dt><code>--regex-message <em>MESSAGE</em></code></dt>
<dd>Filtre les journaux qui contiennent un message particulier écrit sous forme d'expression régulière n'importe où dans le journal. Cette valeur est facultative. Exemple : le masque "hello [0-9]" s'appliquera à "hello 1", "hello 2" et "hello 9".</dd>

<dt><code>--force-update</code></dt>
<dd>Force la mise à jour de vos pods Fluentd à la version la plus récente. Fluentd doit être à la version la plus récente pour apporter des modifications à vos configurations de consignation.</dd>

<dt><code>--json</code></dt>
<dd>Imprime le résultat de la commande au format JSON. Cette valeur est facultative.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemples** :

L'exemple suivant permet de filtrer tous les journaux transmis à partir de conteneurs nommés `test-container` dans l'espace de nom par défaut dont le niveau est debug ou inférieur et dont le message de journal contient "GET request".
```
ibmcloud ks logging-filter-update --cluster example-cluster --id 885274 --type container --namespace default --container test-container --level debug --message "GET request"
```
{: pre}

L'exemple suivant permet de filtrer tous les journaux transférés, de niveau info ou inférieur, à partir d'un cluster spécifique. La sortie est renvoyée au format JSON.
```
ibmcloud ks logging-filter-update --cluster example-cluster --id 274885 --type all --level info --json
```
{: pre}

<br />


## Commandes d'équilibreur de charge de réseau (NLB)
{: #nlb-dns}

Utilisez ce groupe de commandes pour créer et gérer des noms d'hôte pour les adresses IP et les moniteurs de bilan de santé d'équilibreur de charge de réseau (NLB). Pour plus d'informations, voir [Enregistrement d'un nom d'hôte d'équilibreur de charge](/docs/containers?topic=containers-loadbalancer#loadbalancer_hostname).
{: shortdesc}

### ibmcloud ks nlb-dns-add
{: #cs_nlb-dns-add}

Ajoutez une adresse IP d'équilibreur de charge de réseau (NLB) à un nom d'hôte existant que vous avez créé à l'aide de la [commande `ibmcloud ks nlb-dns-create`](#cs_nlb-dns-create).
{: shortdesc}

Par exemple, dans un cluster à zones multiples, vous pouvez créer un équilibreur de charge de réseau dans chaque zone pour exposer une application. Vous enregistrez une adresse IP NLB dans une zone avec un nom d'hôte en exécutant la commande `ibmcloud ks nlb-dns-create`, par conséquent vous pouvez désormais ajouter les adresses IP NLB à partir des autres zones sur ce nom d'hôte existant. Lorsqu'un utilisateur accède à votre nom d'hôte d'application, le client accède à l'une de ces adresses IP de manière aléatoire et la demande est envoyée à cet équilibreur de charge de réseau. Notez que vous devez exécuter la commande suivante pour chaque adresse IP que vous souhaitez ajouter.

```
ibmcloud ks nlb-dns-add --cluster CLUSTER --ip IP --nlb-host HOST_NAME [--json] [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Editeur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

<dt><code>--ip <em>IP</em></code></dt>
<dd>Adresse IP NLB que vous souhaitez ajouter au nom d'hôte. Pour voir vos adresses IP NLB, exécutez la commande <code>kubectl get svc</code>.</dd>

<dt><code>--nlb-host <em>HOST_NAME</em></code></dt>
<dd>Nom d'hôte auquel vous souhaitez ajouter des adresses IP. Pour voir les noms d'hôte existants, exécutez la commande <code>ibmcloud ks nlb-dnss</code>.</dd>

<dt><code>--json</code></dt>
<dd>Imprime le résultat de la commande au format JSON. Cette valeur est facultative.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks nlb-dns-add --cluster mycluster --ip 1.1.1.1 --nlb-host mycluster-a1b2cdef345678g9hi012j3kl4567890-0001.us-south.containers.appdomain.cloud
```
{: pre}

</br>
### ibmcloud ks nlb-dns-create
{: #cs_nlb-dns-create}

Exposez au public votre application en créant un nom d'hôte DNS afin d'enregistrer une adresse IP d'équilibreur de charge de réseau (NLB).
{: shortdesc}

```
ibmcloud ks nlb-dns-create --cluster CLUSTER --ip IP [--json] [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Editeur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

<dt><code>--ip <em>IP</em></code></dt>
<dd>Adresse IP d'équilibreur de charge de réseau que vous souhaitez enregistrer. Pour voir vos adresses IP NLB, exécutez la commande <code>kubectl get svc</code>. Notez que vous pouvez initialement créer le nom d'hôte avec une seule adresse IP. Si vous avez des équilibreurs de charge de réseau dans chaque zone d'un cluster à zones multiples qui exposent une application, vous pouvez ajouter les adresses IP des autres équilibreurs de charge de réseau au nom d'hôte en exécutant la commande [`ibmcloud ks nlb-dns-add`](#cs_nlb-dns-add).</dd>

<dt><code>--json</code></dt>
<dd>Imprime le résultat de la commande au format JSON. Cette valeur est facultative.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks nlb-dns-create --cluster mycluster --ip 1.1.1.1
```
{: pre}

</br>
### ibmcloud ks nlb-dns-rm
{: #cs_nlb-dns-rm}

Retirer une adresse IP d'équilibreur de charge de réseau d'un nom d'hôte. Si vous retirez toutes les adresses IP d'un nom d'hôte, le nom d'hôte existe toujours mais aucune adresse IP ne lui est associée. Notez que vous devez exécuter cette commande pour chaque adresse IP que vous souhaitez retirer.
{: shortdesc}

```
ibmcloud ks nlb-dns-rm --cluster CLUSTER --ip IP --nlb-host HOST_NAME [--json] [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Editeur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

<dt><code>--ip <em>IP</em></code></dt>
<dd>Adresse IP NLB que vous souhaitez retirer. Pour voir vos adresses IP NLB, exécutez la commande <code>kubectl get svc</code>.</dd>

<dt><code>--nlb-host <em>HOST_NAME</em></code></dt>
<dd>Nom d'hôte dont vous souhaitez retirer une adresse IP. Pour voir les noms d'hôte existants, exécutez la commande <code>ibmcloud ks nlb-dnss</code>.</dd>

<dt><code>--json</code></dt>
<dd>Imprime le résultat de la commande au format JSON. Cette valeur est facultative.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks nlb-dns-rm --cluster mycluster --ip 1.1.1.1 --nlb-host mycluster-a1b2cdef345678g9hi012j3kl4567890-0001.us-south.containers.appdomain.cloud
```
{: pre}

</br>
### ibmcloud ks nlb-dnss
{: #cs_nlb-dns-ls}

Répertorier les noms d'hôte et les adresses IP d'équilibreur de charge de réseau qui sont enregistrés dans un cluster.
{: shortdesc}

```
ibmcloud ks nlb-dnss --cluster CLUSTER [--json] [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Editeur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

<dt><code>--json</code></dt>
<dd>Imprime le résultat de la commande au format JSON. Cette valeur est facultative.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks nlb-dnss --cluster mycluster
```
{: pre}

</br>
### ibmcloud ks nlb-dns-monitor
{: #cs_nlb-dns-monitor}

Créer, modifier et afficher les moniteurs de diagnostic d'intégrité pour les noms d'hôte d'équilibreur de charge de réseau dans un cluster. Cette commande doit être associée à l'une des sous-commandes suivantes.
{: shortdesc}

</br>
### ibmcloud ks nlb-dns-monitor-configure
{: #cs_nlb-dns-monitor-configure}

Configurer et le cas échéant activer un moniteur de diagnostic d'intégrité pour un nom d'hôte NLB existant dans un cluster. Lorsque vous activez un moniteur pour votre nom d'hôte, le moniteur réalise un diagnostic d'intégrité de l'adresse IP NLB dans chaque zone et tient à jour les résultats de recherche DNS en fonction de ces diagnostics d'intégrité.
{: shortdesc}

Vous pouvez utiliser cette commande pour créer et activer un nouveau moniteur de diagnostic d'intégrité ou pour mettre à jour les paramètres d'un moniteur de diagnostic d'intégrité existant. Pour créer un nouveau moniteur, ajoutez l'option `--enable` et les options pour tous les paramètres que vous souhaitez configurer. Pour mettre à jour un moniteur existant, ajoutez uniquement les options des paramètres que vous souhaitez modifier.

```
ibmcloud ks nlb-dns-monitor-configure --cluster CLUSTER --nlb-host HOST NAME [--enable] [--desc DESCRIPTION] [--type TYPE] [--method METHOD] [--path PATH] [--timeout TIMEOUT] [--retries RETRIES] [--interval INTERVAL] [--port PORT] [--header HEADER] [--expected-body BODY STRING] [--expected-codes HTTP CODES] [--follows-redirects TRUE] [--allows-insecure TRUE] [--json] [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Editeur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster où le nom d'hôte est enregistré.</dd>

<dt><code>--nlb-host <em>HOST NAME</em></code></dt>
<dd>Nom d'hôte pour lequel configurer un moniteur de diagnostic d'intégrité. Pour répertorier des noms d'hôte, exécutez la commande <code>ibmcloud ks nlb-dnss --cluster CLUSTER</code>.</dd>

<dt><code>--enable</code></dt>
<dd>Ajoutez cette option afin de créer et d'activer un nouveau moniteur de diagnostic d'intégrité pour un nom d'hôte.</dd>

<dt><code>--description <em>DESCRIPTION</em></code></dt>
<dd>Description du moniteur de diagnostic d'intégrité.</dd>

<dt><code>--type <em>TYPE</em></code></dt>
<dd>Protocole à utiliser pour le diagnostic d'intégrité : <code>HTTP</code>, <code>HTTPS</code> ou <code>TCP</code>. Valeur par défaut : <code>HTTP</code></dd>

<dt><code>--method <em>METHOD</em></code></dt>
<dd>Méthode à utiliser pour le diagnostic d'intégrité. Valeur par défaut pour <code>type</code> <code>HTTP</code> et <code>HTTPS</code> : <code>GET</code>. Valeur par défaut pour  <code>type</code> <code>TCP</code> : <code>connection_established</code>.</dd>

<dt><code>--path <em>PATH</em></code></dt>
<dd>Lorsque <code>type</code> a pour valeur <code>HTTPS</code>, chemin de noeud final sur lequel doit porter le diagnostic d'intégrité. Valeur par défaut : <code>/</code></dd>

<dt><code>--timeout <em>TIMEOUT</em></code></dt>
<dd>Délai d'attente, en secondes avant que l'adresse IP soit considérée comme défectueuse. Valeur par défaut : <code>5</code></dd>

<dt><code>--retries <em>RETRIES</em></code></dt>
<dd>Lorsqu'un dépassement de délai d'attente se produit, nombre de tentatives avant que l'adresse IP soit considérée comme défectueuse. Les nouvelles tentatives sont effectuées immédiatement. Valeur par défaut : <code>2</code></dd>

<dt><code>--interval <em>INTERVAL</em></code></dt>
<dd>Intervalle, en secondes, entre chaque diagnostic d'intégrité. Des intervalles courts peuvent améliorer les temps de reprise, mais augmenter la charge sur les adresses IP. Valeur par défaut : <code>60</code></dd>

<dt><code>--port <em>PORT</em></code></dt>
<dd>Numéro de port auquel établir une connexion pour le diagnostic d'intégrité. Lorsque <code>type</code> a pour valeur <code>TCP</code>, ce paramètre est obligatoire. Lorsque <code>type</code> a pour valeur <code>HTTP</code> ou <code>HTTPS</code>, définissez le port uniquement si vous utilisez un port autre que 80 pour HTTP ou 443 pour HTTPS. Valeur par défaut pour TCP : <code>0</code>. Valeur par défaut pour HTTP : <code>80</code>. Valeur par défaut pour HTTPS : <code>443</code>.</dd>

<dt><code>--header <em>HEADER</em></code></dt>
<dd>Lorsque <code>type</code> a pour valeur <code>HTTPS</code> ou <code>HTTPS</code>, en-têtes de demande HTTP à envoyer dans le diagnostic d'intégrité, par exemple, un en-tête d'hôte. L'en-tête User-Agent ne peut pas être remplacé.</dd>

<dt><code>--expected-body <em>BODY STRING</em></code></dt>
<dd>Lorsque <code>type</code> a pour valeur <code>HTTP</code> ou <code>HTTPS</code>, sous-chaîne non sensible à la casse que le diagnostic d'intégrité recherche dans le corps de la réponse. Si chaîne n'est pas trouvée, l'adresse IP est considérée comme défectueuse.</dd>

<dt><code>--expected-codes <em>HTTP CODES</em></code></dt>
<dd>Lorsque <code>type</code> a pour valeur <code>HTTP</code> ou <code>HTTPS</code>, codes HTTP que le diagnostic d'intégrité recherche dans la réponse. Si le code HTTP est introuvable, l'adresse IP est considérée comme défectueuse. Valeur par défaut : <code>2xx</code></dd>

<dt><code>--allows-insecure <em>TRUE</em></code></dt>
<dd>Lorsque <code>type</code> a pour valeur <code>HTTP</code> ou <code>HTTPS</code>, affectez la valeur <code>true</code> pour ne pas valider le certificat.</dd>

<dt><code>--follows-redirects <em>TRUE</em></code></dt>
<dd>Lorsque <code>type</code> a pour valeur <code>HTTP</code> ou <code>HTTPS</code>, affectez la valeur <code>true</code> pour suivre les éventuelles redirections qui sont renvoyées par l'adresse IP.</dd>

<dt><code>--json</code></dt>
<dd>Imprime le résultat de la commande au format JSON. Cette valeur est facultative.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks nlb-dns-monitor-configure --cluster mycluster --nlb-host mycluster-a1b2cdef345678g9hi012j3kl4567890-0001.us-south.containers.appdomain.cloud --enable --desc "Login page monitor" --type HTTPS --method GET --path / --timeout 5 --retries 2 --interval 60  --expected-body "healthy" --expected-codes 2xx --follows-redirects true
```
{: pre}

</br>
### ibmcloud ks nlb-dns-monitor-get
{: #cs_nlb-dns-monitor-get}

Afficher les paramètres d'un moniteur de diagnostic d'intégrité existant.
{: shortdesc}

```
ibmcloud ks nlb-dns-monitor-get --cluster CLUSTER --nlb-host HOST_NAME [--json] [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Editeur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

<dt><code>--nlb-host <em>HOST_NAME</em></code></dt>
<dd>Nom d'hôte sur lequel le moniteur effectue un diagnostic d'intégrité. Pour répertorier des noms d'hôte, exécutez la commande <code>ibmcloud ks nlb-dnss --cluster CLUSTER</code>.</dd>

<dt><code>--json</code></dt>
<dd>Imprime le résultat de la commande au format JSON. Cette valeur est facultative.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks nlb-dns-monitor-get --cluster mycluster --nlb-host mycluster-a1b2cdef345678g9hi012j3kl4567890-0001.us-south.containers.appdomain.cloud
```
{: pre}

</br>
### ibmcloud ks nlb-dns-monitor-disable
{: #cs_nlb-dns-monitor-disable}

Désactiver un moniteur de diagnostic d'intégrité existant pour un nom d'hôte dans un cluster.
{: shortdesc}

```
ibmcloud ks nlb-dns-monitor-disable --cluster CLUSTER --nlb-host HOST_NAME [--json] [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Editeur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

<dt><code>--nlb-host <em>HOST_NAME</em></code></dt>
<dd>Nom d'hôte sur lequel le moniteur effectue un diagnostic d'intégrité. Pour répertorier des noms d'hôte, exécutez la commande <code>ibmcloud ks nlb-dnss --cluster CLUSTER</code>.</dd>

<dt><code>--json</code></dt>
<dd>Imprime le résultat de la commande au format JSON. Cette valeur est facultative.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks nlb-dns-monitor-disable --cluster mycluster --nlb-host mycluster-a1b2cdef345678g9hi012j3kl4567890-0001.us-south.containers.appdomain.cloud
```
{: pre}

</br>
### ibmcloud ks nlb-dns-monitor-enable
{: #cs_nlb-dns-monitor-enable}

Activer un moniteur de diagnostic d'intégrité que vous avez configuré.
{: shortdesc}

Notez que la première fois que vous créez un moniteur de diagnostic d'intégrité, vous devez le configurer et l'activer à l'aide de la commande `ibmcloud ks nlb-dns-monitor-configure`. Utilisez la commande `ibmcloud ks nlb-dns-monitor-enable` uniquement pour activer un moniteur que vous avez configuré mais que vous n'avez pas encore activé, ou pour réactiver un moniteur que vous avez préalablement désactivé.

```
ibmcloud ks nlb-dns-monitor-enable --cluster CLUSTER --nlb-host HOST_NAME [--json] [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Editeur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

<dt><code>--nlb-host <em>HOST_NAME</em></code></dt>
<dd>Nom d'hôte sur lequel le moniteur effectue un diagnostic d'intégrité. Pour répertorier des noms d'hôte, exécutez la commande <code>ibmcloud ks nlb-dnss --cluster CLUSTER</code>.</dd>

<dt><code>--json</code></dt>
<dd>Imprime le résultat de la commande au format JSON. Cette valeur est facultative.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks nlb-dns-monitor-enable --cluster mycluster --nlb-host mycluster-a1b2cdef345678g9hi012j3kl4567890-0001.us-south.containers.appdomain.cloud
```
{: pre}

</br>
### ibmcloud ks nlb-dns-monitor-status
{: #cs_nlb-dns-monitor-status}

Répertorier le statut de diagnostic d'intégrité pour les adresses IP derrière les noms d'hôte NLB dans un cluster.
{: shortdesc}

```
ibmcloud ks nlb-dns-monitor-status --cluster CLUSTER [--nlb-host HOST_NAME] [--json] [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Editeur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

<dt><code>--nlb-host <em>HOST_NAME</em></code></dt>
<dd>Ajoutez cette option afin d'afficher le statut pour un seul nom d'hôte. Pour répertorier des noms d'hôte, exécutez la commande <code>ibmcloud ks nlb-dnss --cluster CLUSTER</code>.</dd>

<dt><code>--json</code></dt>
<dd>Imprime le résultat de la commande au format JSON. Cette valeur est facultative.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks nlb-dns-monitor-status --cluster mycluster
```
{: pre}

</br>
### ibmcloud ks nlb-dns-monitors
{: #cs_nlb-dns-monitor-ls}

Répertorier les paramètres de moniteur de diagnostic d'intégrité pour chaque nom d'hôte NLB dans un cluster.
{: shortdesc}

```
ibmcloud ks nlb-dns-monitors --cluster CLUSTER [--json] [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Editeur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

<dt><code>--json</code></dt>
<dd>Imprime le résultat de la commande au format JSON. Cette valeur est facultative.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks nlb-dns-monitors --cluster mycluster
```
{: pre}

<br />


## Commandes de région et d'emplacement
{: #region_commands}

Utilisez ce groupe de commandes pour afficher les emplacements disponibles, afficher la région ciblée et définir la région ciblée.
{: shortdesc}

### Déprécié : ibmcloud ks region-get
{: #cs_region}

Identifiez la région {{site.data.keyword.containerlong_notm}} actuellement ciblée.
{: shortdesc}

Vous pouvez gérer les ressources auxquelles vous avez accès dans n'importe quel endroit, même si vous définissez une région en exécutant la commande `ibmcloud ks region-set` et que la ressource que vous souhaitez gérer figure dans une autre région. Si vous avez des clusters de même nom dans des régions différentes, vous pouvez utiliser l'ID de cluster lorsque vous exécutez des commandes ou définir une région à l'aide de la commande `ibmcloud ks region-set` et utiliser le nom de cluster lorsque vous exécutez des commandes. 

<p class="deprecated">Comportement existant :<ul><li>Si vous utilisez la version de plug-in {{site.data.keyword.containerlong_notm}} <code>0.3</code> ou ultérieure et que vous devez répertorier et gérer les ressources d'une seule région, vous pouvez utiliser la [commande](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_init) <code>ibmcloud ks init</code> pour cibler un noeud final régional au lieu du noeud final global. </li><li>Si vous [affectez à la variable d'environnement <code>IKS_BETA_VERSION</code> dans le plug-in {{site.data.keyword.containerlong_notm}} la valeur <code>0.2</code>](/docs/containers-cli-plugin?topic=containers-cli-plugin-kubernetes-service-cli#cs_beta), vous créez et gérer des clusters propres à la région. Utilisez la commande <code>ibmcloud ks region-set</code> pour changer de région.</li></ul></p>

```
ibmcloud ks region-get
```
{: pre}

**Droits minimum requis** : aucun

</br>
### Déprécié : ibmcloud ks region-set
{: #cs_region-set}

Définissez la région pour {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

```
ibmcloud ks region-set --region REGION
```
{: pre}

Vous pouvez gérer les ressources auxquelles vous avez accès dans n'importe quel endroit, même si vous définissez une région en exécutant la commande `ibmcloud ks region-set` et que la ressource que vous souhaitez gérer figure dans une autre région. Si vous avez des clusters de même nom dans des régions différentes, vous pouvez utiliser l'ID de cluster lorsque vous exécutez des commandes ou définir une région à l'aide de la commande `ibmcloud ks region-set` et utiliser le nom de cluster lorsque vous exécutez des commandes. 

Si vous utilisez la version bêta `0.2` (existante) du plug-in {{site.data.keyword.containerlong_notm}}, vous créez et gérez des clusters propres à la région. Par exemple, vous pourriez vous connecter à {{site.data.keyword.Bluemix_notm}} dans la région Sud des Etats-Unis et y créer un cluster. Vous pourriez ensuite utiliser la commande `ibmcloud ks region-set eu-central` pour cibler la région Europe centrale et y créer un autre cluster. Enfin, vous pourriez utiliser la commande `ibmcloud ks region-set us-south` pour revenir à la région Sud des Etats-Unis et gérer votre cluster dans cette région.
{: deprecated}

**Droits minimum requis** : aucun

**Options de commande** :
<dl>
<dt><code>--region <em>REGION</em></code></dt>
<dd>Entrez la région que vous désirez cibler. Cette valeur est facultative. Si vous n'indiquez pas la région, vous pouvez la sélectionner dans la liste figurant dans la sortie.

Pour obtenir la liste des régions disponibles, consultez la rubrique [Emplacements](/docs/containers?topic=containers-regions-and-zones) ou utilisez la [commande](#cs_regions) `ibmcloud ks regions`. </dd></dl>

**Exemple** :
```
ibmcloud ks region-set --region eu-central
```
{: pre}

</br>
### Déprécié : ibmcloud ks regions
{: #cs_regions}

Permet de répertorier les régions disponibles. Le nom indiqué dans `Region Name` correspond au nom {{site.data.keyword.containerlong_notm}} et `Region Alias` correspond au nom {{site.data.keyword.Bluemix_notm}} général pour la région.
{: shortdesc}

**Droits minimum requis** : aucun

**Exemple** :
```
ibmcloud ks regions
```
{: pre}

**Sortie **:
```
Region Name   Region Alias
ap-north      jp-tok
ap-south      au-syd
eu-central    eu-de
uk-south      eu-gb
us-east       us-east
us-south      us-south
```
{: screen}

</br>
### ibmcloud ks supported-locations
{: #cs_supported-locations}

Permet de répertorier les emplacements qui sont pris en charge par {{site.data.keyword.containerlong_notm}}. Pour plus d'informations sur les emplacements qui sont renvoyés, voir [Emplacements {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-regions-and-zones#locations).
{: shortdesc}

```
ibmcloud ks supported-locations
```
{: pre}

**Droits minimum requis** : aucun

</br>
### ibmcloud ks zones
{: #cs_datacenters}

Afficher la liste des zones disponibles dans lesquelles vous pouvez créer un cluster.
{: shortdesc}

Les zones de tous les emplacements sont renvoyées. Pour filtrer des zones par emplacement spécifique, ajoutez l'indicateur `--locations`. 

Si vous utilisez la version bêta `0.2` (existante) du plug-in {{site.data.keyword.containerlong_notm}}, les zones disponibles varient en fonction de la région à laquelle vous êtes connecté. Pour changer de région, exécutez la commande `ibmcloud ks region-set`.
{: deprecated}

```
ibmcloud ks zones [--locations LOCATION] [--region-only] [--json] [-s]
```
{: pre}

**Droits minimum** : aucun

**Options de commande** :
<dl>
<dt><code>--locations <em>LOCATION</em></code></dt>
<dd>Filtrez les zones en fonction d'un emplacement spécifique ou d'une liste d'emplacements séparés par des virgules. Pour voir les emplacements pris en charge, exécutez la commande <code>ibmcloud ks supported-locations</code>.</dd>

<dt><code>--region-only</code></dt>
<dd>Affiche uniquement la liste des zones multiples qui se trouvent dans la région à laquelle vous êtes connecté. Cette valeur est facultative.</dd>

<dt><code>--json</code></dt>
<dd>Imprime le résultat de la commande au format JSON. Cette valeur est facultative.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks zones --locations ap
```
{: pre}

<br />




## Commandes de noeud worker
{: worker_node_commands}

### Déprécié : ibmcloud ks worker-add
{: #cs_worker_add}

Permet d'ajouter des noeuds worker autonomes à un cluster. Cette commande est dépréciée. Créez un pool de noeuds worker en exécutant la commande [`ibmcloud ks worker-pool-create`](#cs_worker_pool_create) ou ajoutez des noeuds worker à un pool de noeuds worker en exécutant la commande [`ibmcloud ks worker-pool-resize`](#cs_worker_pool_resize).
{: deprecated}

```
ibmcloud ks worker-add --cluster CLUSTER [--file FILE_LOCATION] [--hardware HARDWARE] --machine-type MACHINE_TYPE --workers NUMBER --private-vlan PRIVATE_VLAN --public-vlan PUBLIC_VLAN [--disable-disk-encrypt] [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Opérateur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

<dt><code>--file <em>FILE_LOCATION</em></code></dt>
<dd>Chemin d'accès au fichier YAML pour ajouter des noeuds worker au cluster. Au lieu de définir des noeuds worker supplémentaires à l'aide des options fournies dans cette commande, vous pouvez utiliser un fichier YAML. Cette valeur est facultative.

<p class="note">Si vous indiquez la même option dans la commande comme paramètre dans le fichier YAML, la valeur de l'option de la commande est prioritaire sur la valeur définie dans le fichier YAML. Par exemple, si vous définissez un type de machine dans votre fichier YAML et que vous utilisez l'option --machine-type dans la commande, la valeur que vous avez entrée dans l'option de commande se substitue à la valeur définie dans le fichier YAML.</p>

<pre class="codeblock">
<code>name: <em>&lt;cluster_name_or_ID&gt;</em>
zone: <em>&lt;zone&gt;</em>
machine-type: <em>&lt;machine_type&gt;</em>
private-vlan: <em>&lt;private_VLAN&gt;</em>
public-vlan: <em>&lt;public_VLAN&gt;</em>
hardware: <em>&lt;shared_or_dedicated&gt;</em>
workerNum: <em>&lt;number_workers&gt;</em>
diskEncryption: <em>false</em></code></pre>

<table>
<caption>Description des composants du fichier YAML</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants du fichier YAML</th>
</thead>
<tbody>
<tr>
<td><code><em>name</em></code></td>
<td>Remplacez <code><em>&lt;cluster_name_or_ID&gt;</em></code> par le nom ou l'ID du cluster sur lequel vous souhaitez ajouter des noeuds worker.</td>
</tr>
<tr>
<td><code><em>zone</em></code></td>
<td>Remplacez <code><em>&lt;zone&gt;</em></code> par la zone dans laquelle déployer vos noeuds worker. Les zones disponibles dépendent de la région à laquelle vous êtes connecté. Pour afficher les zones disponibles, exécutez la commande <code>ibmcloud ks zones</code>.</td>
</tr>
<tr>
<td><code><em>machine-type</em></code></td>
<td>Remplacez <code><em>&lt;machine_type&gt;</em></code> par le type de machine sur lequel vous envisagez de déployer vos noeuds worker. Vous pouvez déployer vos noeuds worker en tant que machines virtuelles sur du matériel partagé ou dédié ou en tant que machines physiques sur un serveur bare metal. Les types de machines virtuelles et physiques disponibles varient en fonction de la zone de déploiement du cluster. Pour plus d'informations, voir la [commande](#cs_machine_types) `ibmcloud ks machine-types`. </td>
</tr>
<tr>
<td><code><em>private-vlan</em></code></td>
<td>Remplacez <code><em>&lt;private_VLAN&gt;</em></code> par l'ID du réseau local virtuel privé que vous souhaitez utiliser pour vos noeuds worker. Pour afficher la liste des VLAN disponibles, exécutez la commande <code>ibmcloud ks vlans --zone <em>&lt;zone&gt;</em></code> et recherchez les routeurs VLAN qui débutent par <code>bcr</code> (routeur de back-end). </td>
</tr>
<tr>
<td><code>public-vlan</code></td>
<td>Remplacez <code>&lt;public_VLAN&gt;</code> par l'ID du réseau local virtuel public que vous souhaitez utiliser pour vos noeuds worker. Pour afficher la liste des VLAN disponibles, exécutez la commande <code>ibmcloud ks vlans --zone &lt;zone&gt;</code> et recherchez les routeurs VLAN qui débutent par <code>fcr</code> (routeur de front-end). <br><strong>Remarque</strong> : si les noeuds worker sont configurés uniquement avec un VLAN privé, vous devez permettre aux noeuds worker et au maître cluster de communiquer en [activant le noeud final de service privé](/docs/containers?topic=containers-cs_network_cluster#set-up-private-se) ou en [configurant un périphérique de passerelle](/docs/containers?topic=containers-plan_clusters#workeruser-master).</td>
</tr>
<tr>
<td><code>hardware</code></td>
<td>Pour les types de machine virtuelle : niveau d'isolement de votre noeud worker. Utilisez un cluster dédié si vous désirez que toutes les ressources physiques vous soient dédiées exclusivement ou un cluster partagé pour permettre leur partage avec d'autres clients IBM. La valeur par défaut est shared.</td>
</tr>
<tr>
<td><code>workerNum</code></td>
<td>Remplacez <code><em>&lt;number_workers&gt;</em></code> par le nombre de noeuds worker que vous souhaitez déployer.</td>
</tr>
<tr>
<td><code>diskEncryption: <em>false</em></code></td>
<td>Par défaut, les noeuds worker disposent du chiffrement de disque avec l'algorithme AES 256 bits. [En savoir plus](/docs/containers?topic=containers-security#encrypted_disk). Pour désactiver le chiffrement, incluez cette option en lui attribuant la valeur <code>false</code>.</td></tr>
</tbody></table></p></dd>

<dt><code>--hardware <em>HARDWARE</em></code></dt>
<dd>Niveau d'isolation du matériel pour votre noeud worker. Utilisez un cluster dédié si vous désirez que toutes les ressources physiques vous soient dédiées exclusivement ou un cluster partagé pour permettre leur partage avec d'autres clients IBM. La valeur par défaut est shared. Cette valeur est facultative. Pour les types de machine bare metal, indiquez `dedicated`.</dd>

<dt><code>--machine-type <em>MACHINE_TYPE</em></code></dt>
<dd>Choisissez un type de machine. Vous pouvez déployer vos noeuds worker en tant que machines virtuelles sur du matériel partagé ou dédié ou en tant que machines physiques sur un serveur bare metal. Les types de machines virtuelles et physiques disponibles varient en fonction de la zone de déploiement du cluster. Pour plus d'informations, voir la documentation de la [commande ](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_machine_types)`ibmcloud ks machine-types`. Cette valeur est obligatoire pour les clusters standard et n'est pas disponible pour les clusters gratuits.</dd>

<dt><code>--workers <em>NUMBER</em></code></dt>
<dd>Entier représentant le nombre de noeuds worker à créer dans le cluster. La valeur par défaut est 1. Cette valeur est facultative.</dd>

<dt><code>--private-vlan <em>PRIVATE_VLAN</em></code></dt>
<dd>VLAN privé spécifié lors de la création du cluster. Cette valeur est obligatoire. Les routeurs de VLAN privé commencent toujours par <code>bcr</code> (routeur de back-end) et les routeurs de VLAN public par <code>fcr</code> (routeur de front-end). Lors de la création d'un cluster et de la spécification des VLAN publics et privés, le nombre et la combinaison de lettres après ces préfixes doivent correspondre.</dd>

<dt><code>--public-vlan <em>PUBLIC_VLAN</em></code></dt>
<dd>VLAN public spécifié lors de la création du cluster. Cette valeur est facultative. Si vous désirez que vos noeuds worker ne soient présents que sur un réseau virtuel local privé, n'indiquez pas d'ID de réseau virtuel local public. Les routeurs de VLAN privé commencent toujours par <code>bcr</code> (routeur de back-end) et les routeurs de VLAN public par <code>fcr</code> (routeur de front-end). Lors de la création d'un cluster et de la spécification des VLAN publics et privés, le nombre et la combinaison de lettres après ces préfixes doivent correspondre.<p class="note">Si les noeuds worker sont configurés uniquement avec un VLAN privé, vous devez permettre aux noeuds worker et au maître cluster de communiquer en [activant le noeud final de service privé](/docs/containers?topic=containers-cs_network_cluster#set-up-private-se) ou en [configurant un périphérique de passerelle](/docs/containers?topic=containers-plan_clusters#workeruser-master).</p></dd>

<dt><code>--disable-disk-encrypt</code></dt>
<dd>Par défaut, les noeuds worker disposent du chiffrement de disque avec l'algorithme AES 256 bits. [En savoir plus](/docs/containers?topic=containers-security#encrypted_disk). Pour désactiver le chiffrement, incluez cette option.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>

</dl>

**Exemple** :
```
ibmcloud ks worker-add --cluster my_cluster --workers 3 --public-vlan my_public_VLAN_ID --private-vlan my_private_VLAN_ID --machine-type b3c.4x16 --hardware shared
```
{: pre}

</br>
### ibmcloud ks worker-get
{: #cs_worker_get}

Affichez les détails d'un noeud worker.
{: shortdesc}

```
ibmcloud ks worker-get --cluster [CLUSTER_NAME_OR_ID] --worker WORKER_NODE_ID [--json] [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Afficheur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER_NAME_OR_ID</em></code></dt>
<dd>Nom ou ID du cluster du noeud worker. Cette valeur est facultative.</dd>

<dt><code>--worker <em>WORKER_NODE_ID</em></code></dt>
<dd>Nom de votre noeud worker. Exécutez la commande <code>ibmcloud ks workers --cluster <em>CLUSTER</em></code> pour afficher les ID des noeuds worker dans un cluster. Cette valeur est obligatoire.</dd>

<dt><code>--json</code></dt>
<dd>Imprime le résultat de la commande au format JSON. Cette valeur est facultative.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks worker-get --cluster my_cluster --worker kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w1
```
{: pre}

**Exemple de sortie** :

  ```
  ID:           kube-dal10-123456789-w1
  State:        normal
  Status:       Ready
  Trust:        disabled
  Private VLAN: 223xxxx
  Public VLAN:  223xxxx
  Private IP:   10.xxx.xx.xxx
  Public IP:    169.xx.xxx.xxx
  Hardware:     shared
  Zone:         dal10
  Version:      1.8.11_1509
  ```
  {: screen}

</br>
### ibmcloud ks worker-reboot
{: #cs_worker_reboot}

Réamorcez un noeud worker dans un cluster. Lors du réamorçage, l'état de votre noeud worker reste inchangé. Par exemple, vous pouvez recourir à un réamorçage si le statut du noeud worker dans l'infrastructure IBM Cloud (SoftLayer) est `Powered Off` et qu'il vous faut mettre le noeud worker sous tension. Un réamorçage efface les répertoires temporaires mais pas le système de fichiers complet et n'entraîne ps le reformatage des disques. L'adresse IP du noeud worker reste la même après l'opération de réamorçage.
{: shortdesc}

Le réamorçage d'un noeud worker peut entraîner des altérations de données sur le noeud worker. Utilisez cette commande avec précaution et lorsque vous savez qu'un réamorçage pourra vous aider à restaurer le noeud worker. Dans tous les autres cas de figure,  [rechargez votre noeud worker](#cs_worker_reload) à la place.
{: important}

Avant de réamorcer votre noeud worker, assurez-vous que les pods sont replanifiés sur d'autres noeuds worker afin d'éviter toute indisponibilité de votre application ou l'altération des données sur votre noeud worker.

1. Répertoriez tous les noeuds worker dans votre cluster et notez le **nom** de celui que vous envisagez de supprimer.
   ```
   kubectl get nodes
   ```
   {: pre}
   Le **nom** renvoyé dans cette commande correspond à l'adresse IP privée affectée à votre noeud worker. Vous pouvez obtenir plus d'informations sur votre noeud worker lorsque vous exécutez la commande `ibmcloud ks workers --cluster <cluster_name_or_ID>` et que vous recherchez le noeud worker avec la même adresse **IP privée**. 

2. Marquez le noeud worker comme non planifiable dans un processus désigné par cordon. Lorsque vous exécutez ce processus sur un noeud worker, vous le rendez indisponible pour toute planification de pod ultérieure. Utilisez le **nom** du noeud worker que vous avez récupéré à l'étape précédente.
   ```
   kubectl cordon <worker_name>
   ```
   {: pre}

3. Vérifiez que la planification de pod est désactivée pour votre noeud worker.
   ```
   kubectl get nodes
   ```
   {: pre}
Votre noeud worker n'est pas activé pour la planification de pod si le statut affiche **SchedulingDisabled**.


4. Imposez le retrait des pods de votre noeud worker et leur replanification sur les noeuds worker restants dans le cluster.
  ```
  kubectl drain <worker_name>
  ```
  {: pre}
   Ce processus peut prendre quelques minutes.
5. Réamorcez le noeud worker. Utilisez l'ID du noeud worker renvoyé par la commande `ibmcloud ks workers --cluster <cluster_name_or_ID>`. 
  ```
  ibmcloud ks worker-reboot --cluster <cluster_name_or_ID> --workers <worker_name_or_ID>
  ```
  {: pre}
6. Patientez environ 5 minutes avant de rendre votre noeud worker disponible pour la planification de pod pour vous assurer que le réamorçage est terminé. Lors du réamorçage, l'état de votre noeud worker reste inchangé. Le réamorçage d'un noeud worker s'effectue en principe en quelques secondes.
7. Rendez votre noeud worker disponible pour la planification de pod. Utilisez le **nom** de noeud worker renvoyé par la commande `kubectl get nodes`.
  ```
  kubectl uncordon <worker_name>
  ```
  {: pre}

  </br>

```
ibmcloud ks worker-reboot [-f] [--hard] --cluster CLUSTER --worker WORKER [WORKER] [--skip-master-healthcheck] [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Opérateur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

<dt><code>-f</code></dt>
<dd>Utilisez cette option pour forcer le redémarrage du noeud worker sans invites utilisateur. Cette valeur est facultative.</dd>

<dt><code>--hard</code></dt>
<dd>Utilisez cette option pour forcer un redémarrage à froid d'un noeud worker en coupant son alimentation. Utilisez cette option si le noeud worker ou l'environnement d'exécution de conteneur du noeud worker ne répond plus. Cette valeur est facultative.</dd>

<dt><code>--workers <em>WORKER</em></code></dt>
<dd>Nom ou ID d'un ou de plusieurs noeuds worker. Utilisez un espace pour répertorier plusieurs noeuds worker. Cette valeur est obligatoire.</dd>

<dt><code>--skip-master-healthcheck</code></dt>
<dd>Ignorer le diagnostic d'intégrité de votre maître avant de recharger et de réamorcer vos noeuds worker.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks worker-reboot --cluster my_cluster --worker kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w1 kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w2
```
{: pre}

</br>
### ibmcloud ks worker-reload
{: #cs_worker_reload}

Recharge les configurations pour un noeud worker. Un rechargement peut s'avérer utile en cas de problème sur votre noeud worker, par exemple une dégradation des performances ou une immobilisation dans un mauvais état de santé. Notez que durant le rechargement, la machine de votre noeud worker est mise à jour avec l'image la plus récente et les données sont supprimées si elles ne sont pas [stockées hors du noeud worker](/docs/containers?topic=containers-storage_planning#persistent_storage_overview). L'adresse IP privée et publique du noeud worker reste la même après l'opération de rechargement.
{: shortdesc}

Le rechargement d'un noeud worker applique des mises à jour de version de correctif à votre noeud worker mais pas de mise à jour principale ou secondaire. Pour voir les modifications d'une version de correctif à une autre, consultez la documentation [Journal des modifications de version](/docs/containers?topic=containers-changelog#changelog).
{: tip}

Avant de recharger le noeud worker, assurez-vous que les pods sont replanifiés sur d'autres noeuds worker afin d'éviter toute indisponibilité de votre application ou l'altération des données sur votre noeud worker.

1. Répertoriez tous les noeuds worker dans votre cluster et notez le **nom** de celui que vous envisagez de recharger.
   ```
   kubectl get nodes
   ```
   Le **nom** renvoyé dans cette commande correspond à l'adresse IP privée affectée à votre noeud worker. Vous pouvez obtenir plus d'informations sur votre noeud worker lorsque vous exécutez la commande `ibmcloud ks workers --cluster <cluster_name_or_ID>` et que vous recherchez le noeud worker avec la même adresse **IP privée**.
2. Marquez le noeud worker comme non planifiable dans un processus désigné par cordon. Lorsque vous exécutez ce processus sur un noeud worker, vous le rendez indisponible pour toute planification de pod ultérieure. Utilisez le **nom** du noeud worker que vous avez récupéré à l'étape précédente.
   ```
   kubectl cordon <worker_name>
   ```
   {: pre}

3. Vérifiez que la planification de pod est désactivée pour votre noeud worker.
   ```
   kubectl get nodes
   ```
   {: pre}
Votre noeud worker n'est pas activé pour la planification de pod si le statut affiche **SchedulingDisabled**.
 4. Imposez le retrait des pods de votre noeud worker et leur replanification sur les noeuds worker restants dans le cluster.
    ```
    kubectl drain <worker_name>
    ```
    {: pre}
    Ce processus peut prendre quelques minutes.
 5. Rechargez le noeud worker. Utilisez l'ID du noeud worker renvoyé par la commande `ibmcloud ks workers --cluster <cluster_name_or_ID>`.
```
    ibmcloud ks worker-reload --cluster <cluster_name_or_ID> --workers <worker_name_or_ID>
    ```
    {: pre}
 6. Patientez jusqu'à la fin du rechargement.
 7. Rendez votre noeud worker disponible pour la planification de pod. Utilisez le **nom** de noeud worker renvoyé par la commande `kubectl get nodes`.
    ```
    kubectl uncordon <worker_name>
    ```
</br>


```
ibmcloud ks worker-reload [-f] --cluster CLUSTER --workers WORKER [WORKER] [--skip-master-healthcheck] [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Opérateur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

<dt><code>-f</code></dt>
<dd>Utilisez cette option pour forcer le rechargement d'un noeud worker sans invites utilisateur. Cette valeur est facultative.</dd>

<dt><code>--worker <em>WORKER</em></code></dt>
<dd>Nom ou ID d'un ou de plusieurs noeuds worker. Utilisez un espace pour répertorier plusieurs noeuds worker. Cette valeur est obligatoire.</dd>

<dt><code>--skip-master-healthcheck</code></dt>
<dd>Ignorer le diagnostic d'intégrité de votre maître avant de recharger et de réamorcer vos noeuds worker.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks worker-reload --cluster my_cluster --workers kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w1 kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w2
```
{: pre}

</br>
### ibmcloud ks worker-rm
{: #cs_worker_rm}

Supprimez un ou plusieurs noeuds worker d'un cluster. Si vous supprimez un noeud worker, votre cluster n'est plus équilibré. Vous pouvez rééquilibrer automatiquement votre pool de noeuds worker en exécutant la [commande](#cs_rebalance) `ibmcloud ks worker-pool-rebalance`.
{: shortdesc}

Avant de supprimer le noeud worker, assurez-vous que les pods sont replanifiés sur d'autres noeuds worker afin d'éviter toute indisponibilité de votre application ou l'altération des données sur votre noeud worker.
{: tip}

1. Répertoriez tous les noeuds worker dans votre cluster et notez le **nom** de celui que vous envisagez de supprimer.
   ```
   kubectl get nodes
   ```
   {: pre}
   Le **nom** renvoyé dans cette commande correspond à l'adresse IP privée affectée à votre noeud worker. Vous pouvez obtenir plus d'informations sur votre noeud worker lorsque vous exécutez la commande `ibmcloud ks workers --cluster <cluster_name_or_ID>` et que vous recherchez le noeud worker avec la même adresse **IP privée**.
2. Marquez le noeud worker comme non planifiable dans un processus désigné par cordon. Lorsque vous exécutez ce processus sur un noeud worker, vous le rendez indisponible pour toute planification de pod ultérieure. Utilisez le **nom** du noeud worker que vous avez récupéré à l'étape précédente.
   ```
   kubectl cordon <worker_name>
   ```
   {: pre}

3. Vérifiez que la planification de pod est désactivée pour votre noeud worker.
   ```
   kubectl get nodes
   ```
   {: pre}
Votre noeud worker n'est pas activé pour la planification de pod si le statut affiche **SchedulingDisabled**.
4. Imposez le retrait des pods de votre noeud worker et leur replanification sur les noeuds worker restants dans le cluster.
   ```
   kubectl drain <worker_name>
   ```
   {: pre}
   Ce processus peut prendre quelques minutes.
5. Supprimez le noeud worker. Utilisez l'ID du noeud worker renvoyé par la commande `ibmcloud ks workers --cluster <cluster_name_or_ID>`. 
   ```
   ibmcloud ks worker-rm --cluster <cluster_name_or_ID> --worker <worker_name_or_ID>
   ```
   {: pre}
6. Vérifiez que le noeud worker est supprimé.
   ```
   ibmcloud ks workers --cluster <cluster_name_or_ID>
   ```
   {: pre}
</br>

```
ibmcloud ks worker-rm [-f] --cluster CLUSTER --workers WORKER[,WORKER] [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Opérateur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

<dt><code>-f</code></dt>
<dd>Utilisez cette option pour forcer la suppression d'un noeud worker sans invites utilisateur. Cette valeur est facultative.</dd>

<dt><code>--workers <em>WORKER</em></code></dt>
<dd>Nom ou ID d'un ou de plusieurs noeuds worker. Utilisez un espace pour répertorier plusieurs noeuds worker. Cette valeur est obligatoire.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks worker-rm --cluster my_cluster --workers kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w1 kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w2
```
{: pre}

</br>
### ibmcloud ks worker-update
{: #cs_worker_update}

Mettez à jour les noeuds worker pour appliquer les correctifs et mises à jour de sécurité les plus récents sur le système d'exploitation et mettre à jour la version de Kubernetes pour qu'elle corresponde à celle du maître Kubernetes. Vous pouvez mettre à jour la version du maître Kubernetes avec la  [commande](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_update) `ibmcloud ks cluster-update`. L'adresse IP du noeud worker reste la même après l'opération de mise à jour.
{: shortdesc}

L'exécution de la commande `ibmcloud ks worker-update` peut entraîner l'indisponibilité de vos services et applications. Lors de la mise à jour, tous les pods sont replanifiés sur d'autres noeuds worker, le noeud worker est réimagé et les données sont supprimées si elles ne sont pas stockées hors du pod. Pour éviter des temps d'indisponibilité, [vérifiez que vous disposez de suffisamment de noeuds worker pour traiter votre charge de travail alors que les noeuds worker sélectionnés sont en cours de mise à jour](/docs/containers?topic=containers-update#worker_node).
{: important}

Vous pourriez devoir modifier vos fichiers YAML en vue des déploiements avant la mise à jour. Consultez cette [note sur l'édition](/docs/containers?topic=containers-cs_versions) pour plus de détails.

```
ibmcloud ks worker-update [-f] --cluster CLUSTER --workers WORKER[,WORKER] [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Opérateur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster sur lequel répertorier les noeuds worker disponibles. Cette valeur est obligatoire.</dd>

<dt><code>-f</code></dt>
<dd>Utilisez cette option pour forcer la mise à jour du maître sans invites utilisateur. Cette valeur est facultative.</dd>

<dt><code>--workers <em>WORKER</em></code></dt>
<dd>ID d'un ou de plusieurs noeuds worker. Utilisez un espace pour répertorier plusieurs noeuds worker. Cette valeur est obligatoire.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks worker-update --cluster my_cluster --worker kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w1 kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w2
```
{: pre}

</br>
### ibmcloud ks workers
{: #cs_workers}

Répertorier tous les noeuds worker dans un cluster.
{: shortdesc}

```
ibmcloud ks workers --cluster CLUSTER [--worker-pool POOL] [--show-pools] [--show-deleted] [--json] [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Afficheur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster pour les noeuds worker disponibles. Cette valeur est obligatoire.</dd>

<dt><code>--worker-pool <em>POOL</em></code></dt>
<dd>Affiche uniquement les noeuds worker appartenant au pool de noeuds worker. Pour afficher la liste des pools de noeuds worker disponibles, exécutez la commande `ibmcloud ks worker-pools --cluster <cluster_name_or_ID>`. Cette valeur est facultative.</dd>

<dt><code>--show-pools</code></dt>
<dd>Répertorie les pools de noeuds worker auquel appartient chaque noeud worker. Cette valeur est facultative.</dd>

<dt><code>--show-deleted</code></dt>
<dd>Affiche les noeuds worker qui ont été supprimés du cluster, y compris la raison de la suppression. Cette valeur est facultative.</dd>

<dt><code>--json</code></dt>
<dd>Imprime le résultat de la commande au format JSON. Cette valeur est facultative.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks workers --cluster my_cluster
```
{: pre}

<br />


## Commandes de pool de noeuds worker
{: #worker-pool}

Utilisez ce groupe de commandes pour afficher et modifier les pools de noeuds worker pour un cluster.
{: shortdesc}

### ibmcloud ks worker-pool-create
{: #cs_worker_pool_create}

Vous pouvez créer un pool de noeuds worker dans votre cluster. Par défaut, lorsque vous ajoutez un pool de noeuds worker, il n'est affecté à aucune zone. Vous spécifiez le nombre de noeuds worker dont vous souhaitez disposer dans chaque zone, ainsi que les types de machine de ces noeuds. Le pool de noeuds worker est fourni avec les versions Kubernetes par défaut. Pour finaliser la création de noeuds worker, [ajoutez une ou plusieurs zones](#cs_zone_add) dans votre pool.
{: shortdesc}

```
ibmcloud ks worker-pool-create --name POOL_NAME --cluster CLUSTER --machine-type MACHINE_TYPE --size-per-zone WORKERS_PER_ZONE --hardware ISOLATION [--labels LABELS] [--disable-disk-encrypt] [-s] [--json]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Opérateur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--name <em>POOL_NAME</em></code></dt>
<dd>Nom que vous souhaitez attribuer à votre pool de noeuds worker.</dd>

<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

<dt><code>--machine-type <em>MACHINE_TYPE</em></code></dt>
<dd>Choisissez un type de machine. Vous pouvez déployer vos noeuds worker en tant que machines virtuelles sur du matériel partagé ou dédié ou en tant que machines physiques sur un serveur bare metal. Les types de machines virtuelles et physiques disponibles varient en fonction de la zone de déploiement du cluster. Pour plus d'informations, voir la documentation de la  [commande](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_machine_types) `ibmcloud ks machine types`. Cette valeur est obligatoire pour les clusters standard et n'est pas disponible pour les clusters gratuits.</dd>

<dt><code>--size-per-zone <em>WORKERS_PER_ZONE</em></code></dt>
<dd>Nombre de noeuds worker à créer dans chaque zone. Cette valeur est obligatoire et doit être supérieure ou égale à 1.</dd>

<dt><code>--hardware <em>ISOLATION</em></code></dt>
<dd>Niveau d'isolation du matériel pour votre noeud worker. Utilisez `dedicated` si vous désirez que toutes les ressources physiques vous soient dédiées exclusivement ou `shared` pour permettre leur partage avec d'autres clients IBM. La valeur par défaut est `shared`. Pour les types de machine bare metal, indiquez `dedicated`. Cette valeur est obligatoire.</dd>

<dt><code>--labels <em>LABELS</em></code></dt>
<dd>Libellés que vous voulez affecter aux noeuds worker dans votre pool. Exemple : `<key1>=<val1>`,`<key2>=<val2>`</dd>

<dt><code>--disable-disk-encrpyt</code></dt>
<dd>Indique si le disque n'est pas chiffré. La valeur par défaut est <code>false</code>.</dd>

<dt><code>--json</code></dt>
<dd>Imprime le résultat de la commande au format JSON. Cette valeur est facultative.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks worker-pool-create --name my_pool --cluster my_cluster --machine-type b3c.4x16 --size-per-zone 6
```
{: pre}

</br>
### ibmcloud ks worker-pool-get
{: #cs_worker_pool_get}

Affichez les détails d'un pool de noeuds worker.
{: shortdesc}

```
ibmcloud ks worker-pool-get --worker-pool WORKER_POOL --cluster CLUSTER [-s] [--json]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Afficheur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--worker-pool <em>WORKER_POOL</em></code></dt>
<dd>Nom du pool de noeuds worker dont vous voulez afficher les détails. Pour afficher la liste des pools de noeuds worker disponibles, exécutez la commande `ibmcloud ks worker-pools --cluster <cluster_name_or_ID>`. Cette valeur est obligatoire.</dd>

<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster dans lequel se trouve le pool de noeuds worker. Cette valeur est obligatoire.</dd>

<dt><code>--json</code></dt>
<dd>Imprime le résultat de la commande au format JSON. Cette valeur est facultative.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks worker-pool-get --worker-pool pool1 --cluster my_cluster
```
{: pre}

**Exemple de sortie** :

  ```
  Name:               pool
  ID:                 a1a11b2222222bb3c33c3d4d44d555e5-f6f777g
  State:              active
  Hardware:           shared
  Zones:              dal10,dal12
  Workers per zone:   3
  Machine type:       b3c.4x16.encrypted
  Labels:             -
  Version:            1.13.6_1512
  ```
  {: screen}

</br>
### ibmcloud ks worker-pool-rebalance
{: #cs_rebalance}

Rééquilibrez votre pool de noeuds worker après la suppression d'un noeud worker. Lorsque vous exécutez cette commande, un ou plusieurs nouveaux noeuds worker sont ajoutés à votre pool de noeuds worker de sorte que celui-ci possède un nombre de noeuds par zone identique à celui que vous avez spécifié.
{: shortdesc}

```
ibmcloud ks worker-pool-rebalance --cluster CLUSTER --worker-pool WORKER_POOL [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Opérateur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code><em>--cluster CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

<dt><code><em>--worker-pool WORKER_POOL</em></code></dt>
<dd>Pool de noeuds worker que vous souhaitez rééquilibrer. Cette valeur est obligatoire.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks worker-pool-rebalance --cluster my_cluster --worker-pool my_pool
```
{: pre}

</br>
### ibmcloud ks worker-pool-resize
{: #cs_worker_pool_resize}

Redimensionnez votre pool de noeuds worker pour augmenter ou réduire le nombre de noeuds worker figurant dans chaque zone de votre cluster. Votre pool de noeuds worker doit comporter au moins un noeud worker.
{: shortdesc}

```
ibmcloud ks worker-pool-resize --worker-pool WORKER_POOL --cluster CLUSTER --size-per-zone WORKERS_PER_ZONE [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Opérateur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--worker-pool <em>WORKER_POOL</em></code></dt>
<dd>Nom du pool de noeuds worker que vous voulez mettre à jour. Cette valeur est obligatoire.</dd>

<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster dont vous voulez redimensionner les pools de noeuds worker. Cette valeur est obligatoire.</dd>

<dt><code>--size-per-zone <em>WORKERS_PER_ZONE</em></code></dt>
<dd>Nombre de noeuds worker dont vous souhaitez disposer dans chaque zone. Cette valeur est obligatoire et doit être supérieure ou égale à 1.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>

</dl>

**Exemple** :
```
ibmcloud ks worker-pool-resize --cluster my_cluster --worker-pool my_pool --size-per-zone 3
```
{: pre}

</br>
### ibmcloud ks worker-pool-rm
{: #cs_worker_pool_rm}

Retirez un pool de noeuds worker de votre cluster. Tous les noeuds worker du pool sont supprimés. Vos pods sont replanifiés lors de la suppression. Pour éviter des interruptions, veillez à disposer d'un nombre suffisant de noeuds worker pour exécuter votre charge de travail.
{: shortdesc}

```
ibmcloud ks worker-pool-rm --worker-pool WORKER_POOL --cluster CLUSTER [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Opérateur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--worker-pool <em>WORKER_POOL</em></code></dt>
<dd>Nom du pool de noeuds worker que vous voulez retirer. Cette valeur est obligatoire.</dd>

<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster duquel vous souhaitez retirer le pool de noeuds worker. Cette valeur est obligatoire.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks worker-pool-rm --cluster my_cluster --worker-pool pool1
```
{: pre}

</br>
### ibmcloud ks worker-pools
{: #cs_worker_pools}

Répertorier tous les pools de noeuds worker dans un cluster.
{: shortdesc}

```
ibmcloud ks worker-pools --cluster CLUSTER [--json] [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Afficheur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--cluster <em>CLUSTER_NAME_OR_ID</em></code></dt>
<dd>Nom ou ID du cluster pour lequel vous souhaitez afficher la liste des pools de noeuds worker. Cette valeur est obligatoire.</dd>

<dt><code>--json</code></dt>
<dd>Imprime le résultat de la commande au format JSON. Cette valeur est facultative.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks worker-pools --cluster my_cluster
```
{: pre}

</br>
### ibmcloud ks zone-add
{: #cs_zone_add}

Après avoir créé un cluster ou un pool de noeuds worker, vous pouvez ajouter une zone. Lorsque vous ajoutez une zone, des noeuds worker sont ajoutés dans la nouvelle zone pour correspondre au nombre de noeuds worker par zone que vous avez indiqué pour le pool de noeuds worker. Vous pouvez ajouter plusieurs zones uniquement si votre cluster se trouve dans une agglomération à zones multiples.
{: shortdesc}

```
ibmcloud ks zone-add --zone ZONE --cluster CLUSTER --worker-pools WORKER_POOL1[,WORKER_POOL2] --private-vlan PRIVATE_VLAN [--public-vlan PUBLIC_VLAN] [--private-only] [--json] [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Opérateur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--zone <em>ZONE</em></code></dt>
<dd>Zone que vous désirez ajouter. Il doit s'agir d'une [zone compatible avec plusieurs zones](/docs/containers?topic=containers-regions-and-zones#zones) présente dans la région du cluster. Cette valeur est obligatoire.</dd>

<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

<dt><code>--worker-pool <em>WORKER_POOLS</em></code></dt>
<dd>Liste séparée par une virgule de pools de noeuds worker auxquels est ajoutée la zone. Il doit y avoir au moins 1 pool de noeuds worker.</dd>

<dt><code>--private-vlan <em>PRIVATE_VLAN</em></code></dt>
<dd><p>ID du VLAN privé. Cette valeur est conditionnelle.</p>
    <p>Si vous disposez d'un VLAN privé dans cette zone, cette valeur doit correspondre à l'ID du VLAN privé d'un ou plusieurs noeuds worker dans le cluster. Pour voir les VLAN à votre disposition, exécutez la commande <code>ibmcloud ks cluster-get --cluster &lt;cluster&gt; --showResources</code>. Les nouveaux noeuds worker sont ajoutés au VLAN que vous spécifiez, mais les VLAN pour les noeuds worker existants restent inchangés.</p>
    <p>Si vous ne disposez pas de VLAN public ou privé dans cette zone, n'indiquez pas cette option. Un VLAN privé et un VLAN public sont automatiquement créés pour vous la première fois que vous ajoutez une nouvelle zone dans votre pool de noeuds worker.</p>
    <p>Si vous disposez de plusieurs VLAN pour un cluster, de plusieurs sous-réseaux sur le même VLAN ou d'un cluster à zones multiples, vous devez activer une fonction [VRF (Virtual Router Function)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) pour votre compte d'infrastructure IBM Cloud (SoftLayer) pour que vos noeuds worker puissent communiquer entre eux sur le réseau privé. Pour activer la fonction VRF, [contactez le représentant de votre compte d'infrastructure IBM Cloud (SoftLayer)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#how-you-can-initiate-the-conversion). Si vous ne parvenez pas à activer la fonction VRF ou si vous ne souhaitez pas le faire, activez la fonction [Spanning VLAN](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning). Pour effectuer cette action, vous devez disposer du [droit d'infrastructure](/docs/containers?topic=containers-users#infra_access) **Réseau > Gérer le spanning VLAN pour réseau**, ou vous pouvez demander au propriétaire du compte de l'activer. Pour vérifier si le spanning VLAN est déjà activé, utilisez la [commande](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get) `ibmcloud ks vlan-spanning-get --region <region>`. </p></dd>

<dt><code>--public-vlan <em>PUBLIC_VLAN</em></code></dt>
<dd><p>ID du VLAN public. Cette valeur est obligatoire si vous souhaitez exposer au public des charges de travail sur les noeuds après avoir créé le cluster. Elle doit correspondre à l'ID du VLAN public d'un ou plusieurs noeuds worker dans le cluster pour la zone. Pour voir les VLAN à votre disposition, exécutez la commande <code>ibmcloud ks cluster-get --cluster &lt;cluster&gt; --showResources</code>. Les nouveaux noeuds worker sont ajoutés au VLAN que vous spécifiez, mais les VLAN pour les noeuds worker existants restent inchangés.</p>
    <p>Si vous ne disposez pas de VLAN public ou privé dans cette zone, n'indiquez pas cette option. Un VLAN privé et un VLAN public sont automatiquement créés pour vous la première fois que vous ajoutez une nouvelle zone dans votre pool de noeuds worker.</p>
    <p>Si vous disposez de plusieurs VLAN pour un cluster, de plusieurs sous-réseaux sur le même VLAN ou d'un cluster à zones multiples, vous devez activer une fonction [VRF (Virtual Router Function)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) pour votre compte d'infrastructure IBM Cloud (SoftLayer) pour que vos noeuds worker puissent communiquer entre eux sur le réseau privé. Pour activer la fonction VRF, [contactez le représentant de votre compte d'infrastructure IBM Cloud (SoftLayer)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#how-you-can-initiate-the-conversion). Si vous ne parvenez pas à activer la fonction VRF ou si vous ne souhaitez pas le faire, activez la fonction [Spanning VLAN](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning). Pour effectuer cette action, vous devez disposer du [droit d'infrastructure](/docs/containers?topic=containers-users#infra_access) **Réseau > Gérer le spanning VLAN pour réseau**, ou vous pouvez demander au propriétaire du compte de l'activer. Pour vérifier si le spanning VLAN est déjà activé, utilisez la [commande](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get) `ibmcloud ks vlan-spanning-get --region <region>`. </p></dd>

<dt><code>--private-only </code></dt>
<dd>Utilisez cette option pour empêcher la création d'un VLAN public. Cette valeur est obligatoire uniquement si vous spécifiez l'indicateur `--private-vlan` sans inclure l'indicateur `--public-vlan`.<p class="note">Si les noeuds worker sont configurés uniquement avec un VLAN privé, vous devez activer le noeud final du service privé ou configurer un périphérique de passerelle. Pour plus d'informations, voir [Planification de votre configuration de cluster privé et de noeud worker](/docs/containers?topic=containers-plan_clusters#private_clusters).</p></dd>

<dt><code>--json</code></dt>
<dd>Imprime le résultat de la commande au format JSON. Cette valeur est facultative.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks zone-add --zone dal10 --cluster my_cluster --worker-pools pool1,pool2,pool3 --private-vlan 2294021
```
{: pre}

</br>
### ibmcloud ks zone-network-set
{: #cs_zone_network_set}

**Clusters à zones multiples uniquement** : définissez les métadonnées du réseau d'un pool de noeuds worker pour utiliser un autre VLAN public ou privé pour la zone que celui qu'elle utilisait auparavant. Les noeuds worker déjà créés dans le pool continuent à utiliser le VLAN public ou privé précédent, mais les nouveaux noeuds worker du pool utilisent les données du nouveau réseau.
{: shortdesc}

```
ibmcloud ks zone-network-set --zone ZONE --cluster CLUSTER --worker-pools WORKER_POOL1[,WORKER_POOL2] --private-vlan PRIVATE_VLAN [--public-vlan PUBLIC_VLAN] [--private-only] [-f] [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Opérateur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--zone <em>ZONE</em></code></dt>
<dd>Zone que vous désirez ajouter. Il doit s'agir d'une [zone compatible avec plusieurs zones](/docs/containers?topic=containers-regions-and-zones#zones) présente dans la région du cluster. Cette valeur est obligatoire.</dd>

<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

<dt><code>--worker-pool <em>WORKER_POOLS</em></code></dt>
<dd>Liste séparée par une virgule de pools de noeuds worker auxquels est ajoutée la zone. Il doit y avoir au moins 1 pool de noeuds worker.</dd>

<dt><code>--private-vlan <em>PRIVATE_VLAN</em></code></dt>
<dd>ID du VLAN privé. Cette valeur est obligatoire, même si vous souhaitez utiliser le même VLAN privé ou un autre VLAN privé que celui que vous avez utilisé pour vos autres noeuds worker. Les nouveaux noeuds worker sont ajoutés au VLAN que vous spécifiez, mais les VLAN pour les noeuds worker existants restent inchangés.<p class="note">Les VLAN public et privé doivent être compatibles, ce que vous pouvez vérifier à partir du préfixe d'ID du **routeur**.</p></dd>

<dt><code>--public-vlan <em>PUBLIC_VLAN</em></code></dt>
<dd>ID du VLAN public. Cette valeur est obligatoire uniquement si vous envisagez de changer de VLAN public pour la zone. Pour changer de VLAN public, vous devez toujours fournir un VLAN privé compatible. Les nouveaux noeuds worker sont ajoutés au VLAN que vous spécifiez, mais les VLAN pour les noeuds worker existants restent inchangés.<p class="note">Les VLAN public et privé doivent être compatibles, ce que vous pouvez vérifier à partir du préfixe d'ID du **routeur**.</p></dd>

<dt><code>--private-only </code></dt>
<dd>Facultatif : Permet d'annuler la définition du VLAN public de sorte que les noeuds worker présents dans cette zone soient connectés à un VLAN privé uniquement. </dd>

<dt><code>-f</code></dt>
<dd>Force la commande à s'exécuter sans invites utilisateur. Cette valeur est facultative.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Syntaxe** :
<ol><li>Vérifiez les VLAN disponibles dans votre cluster. <pre class="pre"><code>ibmcloud ks cluster get --cluster &lt;cluster_name_or_ID&gt; --showResources</code></pre><p>Exemple de sortie :</p>
<pre class="screen"><code>Subnet VLANs
VLAN ID   Subnet CIDR         Public   User-managed
229xxxx   169.xx.xxx.xxx/29   true     false
229xxxx   10.xxx.xx.x/29      false    false</code></pre></li>
<li>Vérifiez si les ID de VLAN public et privé que vous souhaitez utiliser sont compatibles. Pour qu'ils soient compatibles, le routeur (<strong>Router</strong>) doit avoir le même ID de pod. Les routeurs de VLAN privé commencent toujours par <code>bcr</code> (routeur de back-end) et les routeurs de VLAN public par <code>fcr</code> (routeur de front-end). Lors de la création d'un cluster et de la spécification des VLAN publics et privés, le nombre et la combinaison de lettres après ces préfixes doivent correspondre.<pre class="pre"><code>ibmcloud ks vlans --zone &lt;zone&gt;</code></pre><p>Exemple de sortie :</p>
<pre class="screen"><code>ID        Name   Number   Type      Router         Supports Virtual Workers
229xxxx          1234     private   bcr01a.dal12   true
229xxxx          5678     public    fcr01a.dal12   true</code></pre><p>Notez que les ID de pod de la section <strong>Router</strong> correspondent à : `01a` et `01a`. Si un ID de pod était `01a` et que l'autre était `02a`, vous ne pourriez pas définir ces ID de VLAN public et privé pour votre pool de noeuds worker.</p></li>
<li>Si vous n'avez aucun VLAN disponible, vous pouvez <a href="/docs/infrastructure/vlans?topic=vlans-ordering-premium-vlans#ordering-premium-vlans">commander de nouveaux VLAN</a>.</li></ol>

**Exemple** :
```
ibmcloud ks zone-network-set --zone dal10 --cluster my_cluster --worker-pools pool1,pool2,pool3 --private-vlan 2294021
```
{: pre}

</br>
### ibmcloud ks zone-rm
{: #cs_zone_rm}

**Clusters à zones multiples uniquement** : supprimez une zone de tous les pools de noeuds worker dans votre cluster. Tous les noeuds worker du pool correspondant à cette zone sont supprimés.
{: shortdesc}

Avant de supprimer une zone, assurez-vous de disposer d'un nombre de noeuds worker suffisants dans d'autres zones du cluster de sorte que vos pods puissent être replanifiés afin d'éviter toute indisponibilité de votre application ou l'altération des données sur votre noeud worker.
{: tip}

```
ibmcloud ks zone-rm --zone ZONE --cluster CLUSTER [-f] [-s]
```
{: pre}

**Droits minimum requis** : rôle de plateforme **Opérateur** pour le cluster dans {{site.data.keyword.containerlong_notm}}

**Options de commande** :
<dl>
<dt><code>--zone <em>ZONE</em></code></dt>
<dd>Zone que vous désirez ajouter. Il doit s'agir d'une [zone compatible avec plusieurs zones](/docs/containers?topic=containers-regions-and-zones#zones) présente dans la région du cluster. Cette valeur est obligatoire.</dd>

<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

<dt><code>-f</code></dt>
<dd>Forcer la mise à jour sans invites utilisateur. Cette valeur est facultative.</dd>

<dt><code>-s</code></dt>
<dd>Ne pas afficher le message du jour ni les rappels de mise à jour. Cette valeur est facultative.</dd>
</dl>

**Exemple** :
```
ibmcloud ks zone-rm --zone dal10 --cluster my_cluster
```
{: pre}


