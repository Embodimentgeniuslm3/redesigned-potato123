---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: kubernetes, iks

subcollection: containers

---

{:new_window: target="blank"}
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



# Autorizzazioni di accesso utente
{: #access_reference}

Quando [assegni le autorizzazioni del cluster](/docs/containers?topic=containers-users), può essere difficile valutare quale ruolo devi assegnare a un utente. Utilizza le tabelle riportate nelle seguenti sezioni per determinare il livello minimo di autorizzazioni necessarie per eseguire attività comuni in {{site.data.keyword.containerlong}}.
{: shortdesc}

A partire dal 30 gennaio 2019, {{site.data.keyword.containerlong_notm}} ha introdotto un nuovo modo per autorizzare gli utenti con {{site.data.keyword.Bluemix_notm}} IAM: [i ruoli di accesso al servizio](#service). Questi ruoli del servizio vengono utilizzati per concedere l'accesso alle risorse all'interno del cluster, come gli spazi dei nomi Kubernetes. Per ulteriori informazioni, consulta il blog [Introducing service roles and namespaces in IAM for more granular control of cluster access ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/blogs/bluemix/2019/02/introducing-service-roles-and-namespaces-in-iam-for-more-granular-control-of-cluster-access/).
{: note}

## Ruoli della piattaforma {{site.data.keyword.Bluemix_notm}} IAM
{: #iam_platform}

{{site.data.keyword.containerlong_notm}} è configurato per utilizzare i ruoli {{site.data.keyword.Bluemix_notm}} IAM (Identity and Access Management). I ruoli della piattaforma {{site.data.keyword.Bluemix_notm}} IAM determinano le azioni che gli utenti possono eseguire sulle risorse {{site.data.keyword.Bluemix_notm}} come i cluster, i nodi di lavoro e gli ALB (application load balancer) Ingress. I ruoli della piattaforma {{site.data.keyword.Bluemix_notm}} IAM inoltre impostano automaticamente le autorizzazioni di infrastruttura di base per gli utenti. Per impostare i ruoli della piattaforma, vedi [Assegnazione delle autorizzazioni della piattaforma {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform).
{: shortdesc}

<p class="tip">Non assegnare contemporaneamente i ruoli della piattaforma {{site.data.keyword.Bluemix_notm}} IAM come ruoli del servizio. Devi assegnare i ruoli della piattaforma e del servizio separatamente.</p>

In ciascuna delle seguenti sezioni, le tabelle mostrano le autorizzazioni di gestione del cluster, di registrazione e di Ingress concesse da ciascun ruolo della piattaforma {{site.data.keyword.Bluemix_notm}} IAM. Le tabelle sono organizzate in ordine alfabetico in base al nome del comando della CLI.

* [Azioni che non richiedono autorizzazioni](#none-actions)
* [Azioni del visualizzatore](#view-actions)
* [Azioni dell'editor](#editor-actions)
* [Azioni dell'operatore](#operator-actions)
* [Azioni dell'amministratore](#admin-actions)

### Azioni che non richiedono autorizzazioni
{: #none-actions}

Qualsiasi utente nel tuo account che esegue il comando della CLI o effettua la chiamata API per l'azione indicata nella seguente tabella vede il risultato, anche se l'utente non ha autorizzazioni assegnate.
{: shortdesc}

<table>
<caption>Panoramica dei comandi della CLI e delle chiamate API che non richiedono autorizzazioni in {{site.data.keyword.containerlong_notm}}</caption>
<thead>
<th id="none-actions-action">Azione</th>
<th id="none-actions-cli">Comando della CLI</th>
<th id="none-actions-api">Chiamata API</th>
</thead>
<tbody>
<tr>
<td>Visualizza un elenco di versioni supportate per i componenti aggiuntivi gestiti in {{site.data.keyword.containerlong_notm}}.</td>
<td><code>[ibmcloud ks addon-versions](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_addon_versions)</code></td>
<td><code>[GET /v1/addon](https://containers.cloud.ibm.com/global/swagger-global-api/#/util/GetAddons)</code></td>
</tr>
<tr>
<td>Specifica o visualizza l'endpoint API per {{site.data.keyword.containerlong_notm}}.</td>
<td><code>[ibmcloud ks api](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cli_api)</code></td>
<td>-</td>
</tr>
<tr>
<td>Visualizza un elenco di comandi e parametri supportati.</td>
<td><code>[ibmcloud ks help](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_help)</code></td>
<td>-</td>
</tr>
<tr>
<td>Inizializza il plugin {{site.data.keyword.containerlong_notm}} o specifica la regione in cui desideri creare o accedere ai cluster Kubernetes.</td>
<td><code>[ibmcloud ks init](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_init)</code></td>
<td>-</td>
</tr>
<tr>
<td>Obsoleto: Visualizza un elenco di versioni di Kubernetes supportate in {{site.data.keyword.containerlong_notm}}.</td>
<td><code>[ibmcloud ks kube-versions](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_kube_versions)</code></td>
<td><code>[GET /v1/kube-versions](https://containers.cloud.ibm.com/global/swagger-global-api/#/util/GetKubeVersions)</code></td>
</tr>
<tr>
<td>Visualizza un elenco di tipi di macchina disponibili per i tuoi nodi di lavoro.</td>
<td><code>[ibmcloud ks machine-types](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_machine_types)</code></td>
<td><code>[GET /v1/datacenters/{datacenter}/machine-types](https://containers.cloud.ibm.com/global/swagger-global-api/#/util/GetDatacenterMachineTypes)</code></td>
</tr>
<tr>
<td>Visualizza i messaggi correnti dell'utente ID IBM.</td>
<td><code>[ibmcloud ks messages](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_messages)</code></td>
<td><code>[GET /v1/messages](https://containers.cloud.ibm.com/global/swagger-global-api/#/util/GetMessages)</code></td>
</tr>
<tr>
<td>Obsoleto: Trova la regione {{site.data.keyword.containerlong_notm}} in cui sei al momento.</td>
<td><code>[ibmcloud ks region](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_region)</code></td>
<td>-</td>
</tr>
<tr>
<td>Obsoleto: Imposta la regione per {{site.data.keyword.containerlong_notm}}.</td>
<td><code>[ibmcloud ks region-set](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_region-set)</code></td>
<td>-</td>
</tr>
<tr>
<td>Obsoleto: Elenca le regioni disponibili.</td>
<td><code>[ibmcloud ks regions](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_regions)</code></td>
<td><code>[GET /v1/regions](https://containers.cloud.ibm.com/global/swagger-global-api/#/util/GetRegions)</code></td>
</tr>
<tr>
<td>Visualizza un elenco di ubicazioni supportate in {{site.data.keyword.containerlong_notm}}.</td>
<td><code>[ibmcloud ks supported-locations](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_supported-locations)</code></td>
<td><code>[GET /v1/locations](https://containers.cloud.ibm.com/global/swagger-global-api/#/util/ListLocations)</code></td>
</tr>
<tr>
<td>Visualizza un elenco di versioni supportate in {{site.data.keyword.containerlong_notm}}.</td>
<td><code>[ibmcloud ks versions](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_versions)</code></td>
<td>-</td>
</tr>
<tr>
<td>Visualizza un elenco delle zone disponibili in cui puoi creare un cluster.</td>
<td><code>[ibmcloud ks zones](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_datacenters)</code></td>
<td><code>[GET /v1/zones](https://containers.cloud.ibm.com/global/swagger-global-api/#/util/GetZones)</code></td>
</tr>
</tbody>
</table>

### Azioni del visualizzatore
{: #view-actions}

Il ruolo della piattaforma **Visualizzatore** include le [azioni che non richiedono autorizzazioni](#none-actions), oltre alle autorizzazioni mostrate nella seguente tabella. Con il ruolo **Visualizzatore**, utenti quali revisori o responsabili della fatturazione possono visualizzare i dettagli del cluster, ma non modificare l'infrastruttura.
{: shortdesc}

<table>
<caption>Panoramica dei comandi della CLI e delle chiamate API che richiedono il ruolo della piattaforma Visualizzatore in {{site.data.keyword.containerlong_notm}}</caption>
<thead>
<th id="view-actions-mngt">Azione</th>
<th id="view-actions-cli">Comando della CLI</th>
<th id="view-actions-api">Chiamata API</th>
</thead>
<tbody>
<tr>
<td>Visualizza le informazioni per un ALB Ingress.</td>
<td><code>[ibmcloud ks alb-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_get)</code></td>
<td><code>[GET /albs/{albId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/GetClusterALB)</code></td>
</tr>
<tr>
<td>Visualizza i tipi di ALB supportati nella regione.</td>
<td><code>[ibmcloud ks alb-types](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_types)</code></td>
<td><code>[GET /albtypes](https://containers.cloud.ibm.com/global/swagger-global-api/#/util/GetAvailableALBTypes)</code></td>
</tr>
<tr>
<td>Elenca tutti gli ALB Ingress in un cluster.</td>
<td><code>[ibmcloud ks albs](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_albs)</code></td>
<td><code>[GET /clusters/{idOrName}](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/GetClusterALBs)</code></td>
</tr>
<tr>
<td>Visualizza il nome e l'indirizzo e-mail per il proprietario della chiave API {{site.data.keyword.Bluemix_notm}} IAM per un gruppo di risorse e una regione.</td>
<td><code>[ibmcloud ks api-key-info](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_api_key_info)</code></td>
<td><code>[GET /v1/logging/{idOrName}/clusterkeyowner](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/GetClusterKeyOwner)</code></td>
</tr>
<tr>
<td>Scarica i dati di configurazione e i certificati di Kubernetes per connetterti al tuo cluster ed eseguire i comandi `kubectl`.</td>
<td><code>[ibmcloud ks cluster-config](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_config)</code></td>
<td><code>[GET /v1/clusters/{idOrName}/config](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetClusterConfig)</code></td>
</tr>
<tr>
<td>Visualizza le informazioni per un cluster.</td>
<td><code>[ibmcloud ks cluster-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_get)</code></td>
<td><code>[GET /v1/clusters/{idOrName}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetCluster)</code></td>
</tr>
<tr>
<td>Elenca tutti i servizi in tutti gli spazi dei nomi associati a un cluster.</td>
<td><code>[ibmcloud ks cluster-services](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_services)</code></td>
<td><code>[GET /v1/clusters/{idOrName}/services](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/ListServicesForAllNamespaces)</code></td>
</tr>
<tr>
<td>Elenca tutti i cluster.</td>
<td><code>[ibmcloud ks clusters](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_clusters)</code></td>
<td><code>[GET /v1/clusters](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetClusters)</code></td>
</tr>
<tr>
<td>Ottieni le credenziali dell'infrastruttura per l'account {{site.data.keyword.Bluemix_notm}} per accedere a un diverso portfolio dell'infrastruttura IBM Cloud (SoftLayer).</td>
<td><code>[        ibmcloud ks credential-get
        ](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credential_get)</code></td><td><code>[GET /v1/credentials](https://containers.cloud.ibm.com/global/swagger-global-api/#/accounts/GetUserCredentials)</code></td>
</tr>
<tr>
<td>Controlla se alle credenziali che consentono l'accesso al portfolio dell'infrastruttura IBM Cloud (SoftLayer) per la regione e il gruppo di risorse indicati come destinazione mancano le autorizzazioni dell'infrastruttura consigliate o richieste.</td>
<td><code>[ibmcloud ks infra-permissions-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#infra_permissions_get)</code></td>
<td><code>[GET /v1/infra-permissions](https://containers.cloud.ibm.com/global/swagger-global-api/#/accounts/GetInfraPermissions)</code></td>
</tr>
<tr>
<td>Visualizza lo stato per gli aggiornamenti automatici del componente aggiuntivo Fluentd.</td>
<td><code>[ibmcloud ks logging-autoupdate-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_autoupdate_get)</code></td>
<td><code>[GET /v1/logging/{idOrName}/updatepolicy](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/GetUpdatePolicy)</code></td>
</tr>
<tr>
<td>Visualizza l'endpoint di registrazione predefinito per la regione indicata come destinazione.</td>
<td>-</td>
<td><code>[GET /v1/logging/{idOrName}/default](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/GetDefaultLoggingEndpoint)</code></td>
</tr>
<tr>
<td>Elenca tutte le configurazioni di inoltro dei log nel cluster o per una specifica origine del log nel cluster.</td>
<td><code>[ibmcloud ks logging-config-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_logging_get)</code></td>
<td><code>[GET /v1/logging/{idOrName}/loggingconfig](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/FetchLoggingConfigs) and [GET /v1/logging/{idOrName}/loggingconfig/{logSource}](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/FetchLoggingConfigsForSource)</code></td>
</tr>
<tr>
<td>Visualizza le informazioni per una configurazione di filtraggio log.</td>
<td><code>[ibmcloud ks logging-filter-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_filter_view)</code></td>
<td><code>[GET /v1/logging/{idOrName}/filterconfigs/{id}](https://containers.cloud.ibm.com/global/swagger-global-api/#/filter/FetchFilterConfig)</code></td>
</tr>
<tr>
<td>Elenca tutte le configurazione del filtro di registrazione nel cluster</td>
<td><code>[ibmcloud ks logging-filter-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_filter_view)</code></td>
<td><code>[GET /v1/logging/{idOrName}/filterconfigs](https://containers.cloud.ibm.com/global/swagger-global-api/#/filter/FetchFilterConfigs)</code></td>
</tr>
<tr>
<td>Elenca tutti i servizi associati a uno specifico spazio dei nomi.</td>
<td>-</td>
<td><code>[GET /v1/clusters/{idOrName}/services/{namespace}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/ListServicesInNamespace)</code></td>
</tr>
<tr>
<td>Elenca tutte le sottoreti gestite dall'utente associate a un cluster.</td>
<td>-</td>
<td><code>[GET /v1/clusters/{idOrName}/usersubnets](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetClusterUserSubnet)</code></td>
</tr>
<tr>
<td>Elenca le sottoreti disponibili nell'account dell'infrastruttura.</td>
<td><code>[ibmcloud ks subnets](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_subnets)</code></td>
<td><code>[GET /v1/subnets](https://containers.cloud.ibm.com/global/swagger-global-api/#/properties/ListSubnets)</code></td>
</tr>
<tr>
<td>Visualizza lo stato di spanning della VLAN per l'account dell'infrastruttura.</td>
<td><code>[ibmcloud ks vlan-spanning-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get)</code></td>
<td><code>[GET /v1/subnets/vlan-spanning](https://containers.cloud.ibm.com/global/swagger-global-api/#/accounts/GetVlanSpanning)</code></td>
</tr>
<tr>
<td>Quando impostato per un solo cluster: elenca le VLAN a cui è collegato il cluster in una zona.</br>Quando impostato per tutti i cluster nell'account: elenca tutte le VLAN disponibili in una zona.</td>
<td><code>[ibmcloud ks vlans](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlans)</code></td>
<td><code>[GET /v1/datacenters/{datacenter}/vlans](https://containers.cloud.ibm.com/global/swagger-global-api/#/properties/GetDatacenterVLANs)</code></td>
</tr>
<tr>
<td>Elenca tutti i webhook per un cluster.</td>
<td>-</td>
<td><code>[GET /v1/clusters/{idOrName}/webhooks](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetClusterWebhooks)</code></td>
</tr>
<tr>
<td>Visualizza le informazioni per un nodo di lavoro.</td>
<td><code>[ibmcloud ks worker-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_get)</code></td>
<td><code>[GET /v1/clusters/{idOrName}/workers/{workerId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetWorkers)</code></td>
</tr>
<tr>
<td>Visualizza le informazioni per un pool di nodi di lavoro.</td>
<td><code>[ibmcloud ks worker-pool-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_get)</code></td>
<td><code>[GET /v1/clusters/{idOrName}/workerpools/{poolidOrName}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetWorkerPool)</code></td>
</tr>
<tr>
<td>Elenca tutti i pool di nodi di lavoro in un cluster.</td>
<td><code>[ibmcloud ks worker-pools](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pools)</code></td>
<td><code>[GET /v1/clusters/{idOrName}/workerpools](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetWorkerPools)</code></td>
</tr>
<tr>
<td>Elenca tutti i nodi di lavoro in un cluster.</td>
<td><code>[ibmcloud ks workers](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_workers)</code></td>
<td><code>[GET /v1/clusters/{idOrName}/workers](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetClusterWorkers)</code></td>
</tr>
</tbody>
</table>

### Azioni dell'editor
{: #editor-actions}

Il ruolo della piattaforma **Editor** include le autorizzazioni concesse dal **Visualizzatore**, oltre alle seguenti. Con il ruolo **Editor**, utenti quali gli sviluppatori possono eseguire il bind dei servizi, utilizzare le risorse Ingress e configurare l'inoltro dei log per le proprie applicazioni, ma non modificare l'infrastruttura. **Suggerimento**: utilizza questo ruolo per gli sviluppatori dell'applicazione e assegna il ruolo <a href="#cloud-foundry">Cloud Foundry</a> di **Sviluppatore**.
{: shortdesc}

<table>
<caption>Panoramica dei comandi della CLI e delle chiamate API che richiedono il ruolo della piattaforma Editor in {{site.data.keyword.containerlong_notm}}</caption>
<thead>
<th id="editor-actions-mngt">Azione</th>
<th id="editor-actions-cli">Comando della CLI</th>
<th id="editor-actions-api">Chiamata API</th>
</thead>
<tbody>
<tr>
<td>Disabilita gli aggiornamenti automatici per il componente aggiuntivo ALB Ingress.</td>
<td><code>[ibmcloud ks alb-autoupdate-disable](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_autoupdate_disable)</code></td>
<td><code>[PUT /clusters/{idOrName}/updatepolicy](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/ChangeUpdatePolicy)</code></td>
</tr>
<tr>
<td>Abilita gli aggiornamenti automatici per il componente aggiuntivo ALB Ingress.</td>
<td><code>[ibmcloud ks alb-autoupdate-enable](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_autoupdate_enable)</code></td>
<td><code>[PUT /clusters/{idOrName}/updatepolicy](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/ChangeUpdatePolicy)</code></td>
</tr>
<tr>
<td>Controlla se sono abilitati gli aggiornamenti automatici per il componente aggiuntivo ALB Ingress.</td>
<td><code>[ibmcloud ks alb-autoupdate-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_autoupdate_get)</code></td>
<td><code>[GET /clusters/{idOrName}/updatepolicy](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/GetUpdatePolicy)</code></td>
</tr>
<tr>
<td>Abilita o disabilita un ALB Ingress.</td>
<td><code>[ibmcloud ks alb-configure](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_configure)</code></td>
<td><code>[POST /albs](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/EnableALB) and [DELETE /albs/{albId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/)</code></td>
</tr>
<tr>
<td>Crea un ALB Ingress.</td>
<td><code>[ibmcloud ks alb-create](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_create)</code></td>
<td><code>[POST /clusters/{idOrName}/zone/{zoneId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/CreateALB)</code></td>
</tr>
<tr>
<td>Esegui il rollback dell'aggiornamento del componente aggiuntivo ALB Ingress alla build in cui i tuoi pod ALB erano precedentemente in esecuzione.</td>
<td><code>[ibmcloud ks alb-rollback](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_rollback)</code></td>
<td><code>[PUT /clusters/{idOrName}/updaterollback](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/RollbackUpdate)</code></td>
</tr>
<tr>
<td>Forza un aggiornamento una tantum dei tuoi pod ALB aggiornando manualmente il componente aggiuntivo ALB Ingress.</td>
<td><code>[ibmcloud ks alb-update](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_update)</code></td>
<td><code>[PUT /clusters/{idOrName}/update](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/UpdateALBs)</code></td>
</tr>
<tr>
<td>Crea un webhook di controllo del server API.</td>
<td><code>[ibmcloud ks apiserver-config-set](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_apiserver_config_set)</code></td>
<td><code>[PUT /v1/clusters/{idOrName}/apiserverconfigs/auditwebhook](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/apiserverconfigs/UpdateAuditWebhook)</code></td>
</tr>
<tr>
<td>Elimina un webhook di controllo del server API.</td>
<td><code>[ibmcloud ks apiserver-config-unset](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_apiserver_config_unset)</code></td>
<td><code>[DELETE /v1/clusters/{idOrName}/apiserverconfigs/auditwebhook](https://containers.cloud.ibm.com/global/swagger-global-api/#/apiserverconfigs/DeleteAuditWebhook)</code></td>
</tr>
<tr>
<td>Esegui il bind di un servizio a un cluster. **Nota**: devi disporre del ruolo di sviluppatore Cloud Foundry per lo spazio in cui si trova la tua istanza del servizio.</td>
<td><code>[ibmcloud ks cluster-service-bind](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_service_bind)</code></td>
<td><code>[POST /v1/clusters/{idOrName}/services](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/BindServiceToNamespace)</code></td>
</tr>
<tr>
<td>Annulla il bind di un servizio da un cluster. **Nota**: devi disporre del ruolo di sviluppatore Cloud Foundry per lo spazio in cui si trova la tua istanza del servizio.</td>
<td><code>[ibmcloud ks cluster-service-unbind](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_service_unbind)</code></td>
<td><code>[DELETE /v1/clusters/{idOrName}/services/{namespace}/{serviceInstanceId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/UnbindServiceFromNamespace)</code></td>
</tr>
<tr>
<td>Crea una configurazione di inoltro dei log per tutte le origini log eccetto <code>kube-audit</code>.</td>
<td><code>[ibmcloud ks logging-config-create](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_logging_create)</code></td>
<td><code>[POST /v1/logging/{idOrName}/loggingconfig/{logSource}](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/CreateLoggingConfig)</code></td>
</tr>
<tr>
<td>Aggiorna una configurazione di inoltro dei log.</td>
<td><code>[ibmcloud ks logging-config-refresh](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_logging_refresh)</code></td>
<td><code>[PUT /v1/logging/{idOrName}/refresh](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/RefreshLoggingConfig)</code></td>
</tr>
<tr>
<td>Elimina una configurazione di inoltro dei log per tutte le origini log eccetto <code>kube-audit</code>.</td>
<td><code>[ibmcloud ks logging-config-rm](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_logging_rm)</code></td>
<td><code>[DELETE /v1/logging/{idOrName}/loggingconfig/{logSource}/{id}](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/DeleteLoggingConfig)</code></td>
</tr>
<tr>
<td>Elimina tutte le configurazioni di inoltro dei log per un cluster.</td>
<td>-</td>
<td><code>[DELETE /v1/logging/{idOrName}/loggingconfig](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/DeleteLoggingConfigs)</code></td>
</tr>
<tr>
<td>Aggiorna una configurazione di inoltro dei log.</td>
<td><code>[ibmcloud ks logging-config-update](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_logging_update)</code></td>
<td><code>[PUT /v1/logging/{idOrName}/loggingconfig/{logSource}/{id}](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/UpdateLoggingConfig)</code></td>
</tr>
<tr>
<td>Crea una configurazione di filtraggio log.</td>
<td><code>[ibmcloud ks logging-filter-create](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_filter_create)</code></td>
<td><code>[POST /v1/logging/{idOrName}/filterconfigs](https://containers.cloud.ibm.com/global/swagger-global-api/#/filter/CreateFilterConfig)</code></td>
</tr>
<tr>
<td>Elimina una configurazione di filtraggio log.</td>
<td><code>[ibmcloud ks logging-filter-rm](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_filter_delete)</code></td>
<td><code>[DELETE /v1/logging/{idOrName}/filterconfigs/{id}](https://containers.cloud.ibm.com/global/swagger-global-api/#/filter/DeleteFilterConfig)</code></td>
</tr>
<tr>
<td>Elimina tutte le configurazione del filtro di registrazione per il cluster Kubernetes.</td>
<td>-</td>
<td><code>[DELETE /v1/logging/{idOrName}/filterconfigs](https://containers.cloud.ibm.com/global/swagger-global-api/#/filter/DeleteFilterConfigs)</code></td>
</tr>
<tr>
<td>Aggiorna una configurazione di filtraggio log.</td>
<td><code>[ibmcloud ks logging-filter-update](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_filter_update)</code></td>
<td><code>[PUT /v1/logging/{idOrName}/filterconfigs/{id}](https://containers.cloud.ibm.com/global/swagger-global-api/#/filter/UpdateFilterConfig)</code></td>
</tr>
<tr>
<td>Aggiungi un indirizzo IP NLB a un nome host NLB esistente.</td>
<td><code>[ibmcloud ks nlb-dns-add](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-add)</code></td>
<td><code>[PUT /clusters/{idOrName}/add](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb45dns/UpdateDNSWithIP)</code></td>
</tr>
<tr>
<td>Crea un nome host DNS per registrare un indirizzo IP NLB.</td>
<td><code>[ibmcloud ks nlb-dns-create](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-create)</code></td>
<td><code>[POST /clusters/{idOrName}/register](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb45dns/RegisterDNSWithIP)</code></td>
</tr>
<tr>
<td>Elenca i nomi host NLB e gli indirizzi IP registrati in un cluster.</td>
<td><code>[ibmcloud ks nlb-dnss](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-ls)</code></td>
<td><code>[GET /clusters/{idOrName}/list](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb45dns/ListNLBIPsForSubdomain)</code></td>
</tr>
<tr>
<td>Rimuovi un indirizzo IP NLB da un nome host.</td>
<td><code>[ibmcloud ks nlb-dns-rm](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-rm)</code></td>
<td><code>[DELETE /clusters/{idOrName}/host/{nlbHost}/ip/{nlbIP}/remove](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb45dns/UnregisterDNSWithIP)</code></td>
</tr>
<tr>
<td>Configura e abilita, facoltativamente, un monitoraggio del controllo di integrità per un nome host NLB esistente in un cluster.</td>
<td><code>[ibmcloud ks nlb-dns-monitor-configure](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-monitor-configure)</code></td>
<td><code>[POST /health/clusters/{idOrName}/config](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb45health45monitor/AddNlbDNSHealthMonitor)</code></td>
</tr>
<tr>
<td>Visualizza le impostazioni per un monitoraggio del controllo di integrità esistente.</td>
<td><code>[ibmcloud ks nlb-dns-monitor-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-monitor-get)</code></td>
<td><code>[GET /health/clusters/{idOrName}/host/{nlbHost}/config](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb45health45monitor/GetNlbDNSHealthMonitor)</code></td>
</tr>
<tr>
<td>Disabilita un monitoraggio del controllo di integrità esistente per un nome host in un cluster.</td>
<td><code>[ibmcloud ks nlb-dns-monitor-disable](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-monitor-disable)</code></td>
<td><code>[PUT /clusters/{idOrName}/health](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb45health45monitor/UpdateNlbDNSHealthMonitor)</code></td>
</tr>
<tr>
<td>Abilita un monitoraggio del controllo di integrità che hai configurato.</td>
<td><code>[ibmcloud ks nlb-dns-monitor-enable](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-monitor-enable)</code></td>
<td><code>[PUT /clusters/{idOrName}/health](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb45health45monitor/UpdateNlbDNSHealthMonitor)</code></td>
</tr>
<tr>
<td>Elenca le impostazioni del monitoraggio del controllo di integrità per ciascun nome host NLB di un cluster.</td>
<td><code>[ibmcloud ks nlb-dns-monitor-ls](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-monitor-ls)</code></td>
<td><code>[GET /health/clusters/{idOrName}/list](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb45health45monitor/ListNlbDNSHealthMonitors)</code></td>
</tr>
<tr>
<td>Elenca lo stato del controllo di integrità di ogni indirizzo IP registrato con un nome host NLB in un cluster.</td>
<td><code>[ibmcloud ks nlb-dns-monitor-status](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-monitor-status)</code></td>
<td><code>[GET /health/clusters/{idOrName}/status](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb45health45monitor/ListNlbDNSHealthMonitorStatus)</code></td>
</tr>
<tr>
<td>Crea un webhook in un cluster.</td>
<td><code>[ibmcloud ks webhook-create](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_webhook_create)</code></td>
<td><code>[POST /v1/clusters/{idOrName}/webhooks](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/AddClusterWebhooks)</code></td>
</tr>
</tbody>
</table>

### Azioni dell'operatore
{: #operator-actions}

Il ruolo della piattaforma **Operatore** include le autorizzazioni concesse dal **Visualizzatore**, oltre alle autorizzazioni mostrate nella seguente tabella. Con il ruolo **Operatore**, utenti quali site reliability engineer, ingegneri DevOps e amministratori cluster possono aggiungere nodi di lavoro e risolvere i problemi relativi all'infrastruttura, quali il ricaricamento di un nodo di lavoro; non possono invece creare o eliminare il cluster, modificare le credenziali o configurare funzioni a livello di cluster, quali endpoint del servizio o componenti aggiuntivi gestiti.
{: shortdesc}

<table>
<caption>Panoramica dei comandi della CLI e delle chiamate API che richiedono il ruolo della piattaforma Operatore in {{site.data.keyword.containerlong_notm}}</caption>
<thead>
<th id="operator-mgmt">Azione</th>
<th id="operator-cli">Comando della CLI</th>
<th id="operator-api">Chiamata API</th>
</thead>
<tbody>
<tr>
<td>Aggiorna il master Kubernetes.</td>
<td><code>[ibmcloud ks apiserver-refresh](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_apiserver_refresh) (cluster-refresh)</code></td>
<td><code>[PUT /v1/clusters/{idOrName}/masters](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/HandleMasterAPIServer)</code></td>
</tr>
<tr>
<td>Crea un ID del servizio {{site.data.keyword.Bluemix_notm}} IAM per il cluster, crea una politica per l'ID servizio che assegna il ruolo di accesso al servizio **Lettore** in {{site.data.keyword.registrylong_notm}}, quindi crea una chiave API per l'ID servizio.</td>
<td><code>[ibmcloud ks cluster-pull-secret-apply](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_pull_secret_apply)</code></td>
<td>-</td>
</tr>
<tr>
<td>Aggiungi una sottorete a un cluster.</td>
<td><code>[ibmcloud ks cluster-subnet-add](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_subnet_add)</code></td>
<td><code>[PUT /v1/clusters/{idOrName}/subnets/{subnetId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/AddClusterSubnet)</code></td>
</tr>
<tr>
<td>Crea una sottorete e aggiungila a un cluster.</td>
<td><code>[ibmcloud ks cluster-subnet-create](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_subnet_create)</code></td>
<td><code>[POST /v1/clusters/{idOrName}/vlans/{vlanId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/CreateClusterSubnet)</code></td>
</tr>
<tr>
<td>Aggiorna un cluster.</td>
<td><code>[ibmcloud ks cluster-update](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_update)</code></td>
<td><code>[PUT /v1/clusters/{idOrName}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/UpdateCluster)</code></td>
</tr>
<tr>
<td>Aggiungi una sottorete gestita dall'utente a un cluster.</td>
<td><code>[ibmcloud ks cluster-user-subnet-add](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_user_subnet_add)</code></td>
<td><code>[POST /v1/clusters/{idOrName}/usersubnets](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/AddClusterUserSubnet)</code></td>
</tr>
<tr>
<td>Rimuovi una sottorete gestita dall'utente da un cluster.</td>
<td><code>[ibmcloud ks cluster-user-subnet-rm](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_user_subnet_rm)</code></td>
<td><code>[DELETE /v1/clusters/{idOrName}/usersubnets/{subnetId}/vlans/{vlanId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/RemoveClusterUserSubnet)</code></td>
</tr>
<tr>
<td>Aggiungi nodi di lavoro.</td>
<td><code>[ibmcloud ks worker-add (deprecated)](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_add)</code></td>
<td><code>[POST /v1/clusters/{idOrName}/workers](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/AddClusterWorkers)</code></td>
</tr>
<tr>
<td>Crea un pool di nodi di lavoro.</td>
<td><code>[ibmcloud ks worker-pool-create](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_create)</code></td>
<td><code>[POST /v1/clusters/{idOrName}/workerpools](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/CreateWorkerPool)</code></td>
</tr>
<tr>
<td>Ribilancia un pool di nodi di lavoro.</td>
<td><code>[ibmcloud ks worker-pool-rebalance](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_rebalance)</code></td>
<td><code>[PATCH /v1/clusters/{idOrName}/workerpools/{poolidOrName}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/PatchWorkerPool)</code></td>
</tr>
<tr>
<td>Ridimensiona un pool di nodi di lavoro.</td>
<td><code>[ibmcloud ks worker-pool-resize](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_resize)</code></td>
<td><code>[PATCH /v1/clusters/{idOrName}/workerpools/{poolidOrName}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/PatchWorkerPool)</code></td>
</tr>
<tr>
<td>Elimina un pool di nodi di lavoro.</td>
<td><code>[ibmcloud ks worker-pool-rm](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_rm)</code></td>
<td><code>[DELETE /v1/clusters/{idOrName}/workerpools/{poolidOrName}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/RemoveWorkerPool)</code></td>
</tr>
<tr>
<td>Riavvia un nodo di lavoro.</td>
<td><code>[ibmcloud ks worker-reboot](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reboot)</code></td>
<td><code>[PUT /v1/clusters/{idOrName}/workers/{workerId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/UpdateClusterWorker)</code></td>
</tr>
<tr>
<td>Ricarica un nodo di lavoro.</td>
<td><code>[ibmcloud ks worker-reload](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)</code></td>
<td><code>[PUT /v1/clusters/{idOrName}/workers/{workerId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/UpdateClusterWorker)</code></td>
</tr>
<tr>
<td>Rimuovi un nodo di lavoro.</td>
<td><code>[ibmcloud ks worker-rm](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_rm)</code></td>
<td><code>[DELETE /v1/clusters/{idOrName}/workers/{workerId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/RemoveClusterWorker)</code></td>
</tr>
<tr>
<td>Aggiorna un nodo di lavoro.</td>
<td><code>[ibmcloud ks worker-update](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update)</code></td>
<td><code>[PUT /v1/clusters/{idOrName}/workers/{workerId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/UpdateClusterWorker)</code></td>
</tr>
<tr>
<td>Aggiungi una zona a un pool di nodi di lavoro.</td>
<td><code>[ibmcloud ks zone-add](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_zone_add)</code></td>
<td><code>[POST /v1/clusters/{idOrName}/workerpools/{poolidOrName}/zones](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/AddWorkerPoolZone)</code></td>
</tr>
<tr>
<td>Aggiorna la configurazione di rete per una determinata zona in un pool di nodi di lavoro.</td>
<td><code>[ibmcloud ks zone-network-set](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_zone_network_set)</code></td>
<td><code>[PATCH /v1/clusters/{idOrName}/workerpools/{poolidOrName}/zones/{zoneid}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/AddWorkerPoolZoneNetwork)</code></td>
</tr>
<tr>
<td>Rimuovi una zona da un pool di nodi di lavoro.</td>
<td><code>[ibmcloud ks zone-rm](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_zone_rm)</code></td>
<td><code>[DELETE /v1/clusters/{idOrName}/workerpools/{poolidOrName}/zones/{zoneid}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/RemoveWorkerPoolZone)</code></td>
</tr>
</tbody>
</table>

### Azioni dell'amministratore
{: #admin-actions}

Il ruolo della piattaforma **Amministratore** include tutte le autorizzazioni concesse dai ruoli **Visualizzatore**, **Editor** e **Operatore**, oltre alle seguenti. Con il ruolo **Amministratore**, utenti quali amministratori di cluster o account possono creare ed eliminare i cluster o configurare funzioni a livello di cluster, quali endpoint del servizio o componenti aggiuntivi gestiti. Per creare un ordine tra le risorse dell'infrastruttura, quali le macchine del nodo di lavoro, le VLAN e le sottoreti, gli utenti Amministratore hanno bisogno del <a href="#infra">ruolo dell'infrastruttura</a> **Super utente** o la chiave API della regione deve essere impostata con le autorizzazioni appropriate.
{: shortdesc}

<table>
<caption>Panoramica dei comandi della CLI e delle chiamate API che richiedono il ruolo della piattaforma Amministratore in {{site.data.keyword.containerlong_notm}}</caption>
<thead>
<th id="admin-mgmt">Azione</th>
<th id="admin-cli">Comando della CLI</th>
<th id="admin-api">Chiamata API</th>
</thead>
<tbody>
<tr>
<td>Beta: Distribuisci o aggiorna un certificato dalla tua istanza {{site.data.keyword.cloudcerts_long_notm}} a un ALB.</td>
<td><code>[ibmcloud ks alb-cert-deploy](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_cert_deploy)</code></td>
<td><code>[POST /albsecrets](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/CreateALBSecret) or [PUT /albsecrets](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/UpdateALBSecret)</code></td>
</tr>
<tr>
<td>Beta: Visualizza i dettagli per un segreto ALB in un cluster.</td>
<td><code>[ibmcloud ks alb-cert-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_cert_get)</code></td>
<td><code>[GET /clusters/{idOrName}/albsecrets](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/ViewClusterALBSecrets)</code></td>
</tr>
<tr>
<td>Beta: Rimuovi un segreto ALB da un cluster.</td>
<td><code>[ibmcloud ks alb-cert-rm](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_cert_rm)</code></td>
<td><code>[DELETE /clusters/{idOrName}/albsecrets](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/DeleteClusterALBSecrets)</code></td>
</tr>
<tr>
<td>Elenca tutti i segreti ALB in un cluster.</td>
<td><code>[ibmcloud ks alb-certs](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_certs)</code></td>
<td>-</td>
</tr>
<tr>
<td>Imposta la chiave API per l'account {{site.data.keyword.Bluemix_notm}} per accedere al portfolio dell'infrastruttura IBM Cloud (SoftLayer) collegato.</td>
<td><code>[ibmcloud ks api-key-reset](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_api_key_reset)</code></td>
<td><code>[POST /v1/keys](https://containers.cloud.ibm.com/global/swagger-global-api/#/accounts/ResetUserAPIKey)</code></td>
</tr>
<tr>
<td>Disabilita un componente aggiuntivo gestito, come Istio o Knative, in un cluster.</td>
<td><code>[ibmcloud ks cluster-addon-disable](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_addon_disable)</code></td>
<td><code>[PATCH /v1/clusters/{idOrName}/addons](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/ManageClusterAddons)</code></td>
</tr>
<tr>
<td>Abilita un componente aggiuntivo gestito, come Istio o Knative, in un cluster.</td>
<td><code>[ibmcloud ks cluster-addon-enable](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_addon_enable)</code></td>
<td><code>[PATCH /v1/clusters/{idOrName}/addons](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/ManageClusterAddons)</code></td>
</tr>
<tr>
<td>Elenca i componenti aggiuntivi gestiti, come Istio o Knative, che sono abilitati in un cluster.</td>
<td><code>[ibmcloud ks cluster-addons](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_addons)</code></td>
<td><code>[GET /v1/clusters/{idOrName}/addons](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetClusterAddons)</code></td>
</tr>
<tr>
<td>Crea un cluster gratuito o standard. **Nota**: sono richiesti anche il ruolo della piattaforma di Amministratore per {{site.data.keyword.registrylong_notm}} e il ruolo dell'infrastruttura Super utente.</td>
<td><code>[ibmcloud ks cluster-create](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_create)</code></td>
<td><code>[POST /v1/clusters](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/CreateCluster)</code></td>
</tr>
<tr>
<td>Disabilita una funzione specificata per un cluster, come l'endpoint del servizio pubblico per il master cluster.</td>
<td><code>[ibmcloud ks cluster-feature-disable](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_disable)</code></td>
<td>-</td>
</tr>
<tr>
<td>Abilita una funzione specificata per un cluster, come l'endpoint del servizio privato per il master cluster.</td>
<td><code>[ibmcloud ks cluster-feature-enable](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable)</code></td>
<td>-</td>
</tr>
<tr>
<td>Elimina un cluster.</td>
<td><code>[ibmcloud ks cluster-rm](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_rm)</code></td>
<td><code>[DELETE /v1/clusters/{idOrName}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/RemoveCluster)</code></td>
</tr>
<tr>
<td>Imposta le credenziali dell'infrastruttura per l'account {{site.data.keyword.Bluemix_notm}} per accedere a un diverso portfolio dell'infrastruttura IBM Cloud (SoftLayer).</td>
<td><code>[ibmcloud ks credential-set](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_set)</code></td>
<td><code>[POST /v1/credentials](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/accounts/StoreUserCredentials)</code></td>
</tr>
<tr>
<td>Rimuovi le credenziali dell'infrastruttura per l'account {{site.data.keyword.Bluemix_notm}} per accedere a un diverso portfolio dell'infrastruttura IBM Cloud (SoftLayer).</td>
<td><code>[ibmcloud ks credential-unset](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_unset)</code></td>
<td><code>[DELETE /v1/credentials](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/accounts/RemoveUserCredentials)</code></td>
</tr>
<tr>
<td>Beta: Crittografa i segreti Kubernetes utilizzando {{site.data.keyword.keymanagementservicefull}}.</td>
<td><code>[ibmcloud ks key-protect-enable](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_messages)</code></td>
<td><code>[POST /v1/clusters/{idOrName}/kms](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/CreateKMSConfig)</code></td>
</tr>
<tr>
<td>Disabilita gli aggiornamenti automatici per il componente aggiuntivo del cluster Fluentd.</td>
<td><code>[ibmcloud ks logging-autoupdate-disable](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_autoupdate_disable)</code></td>
<td><code>[PUT /v1/logging/{idOrName}/updatepolicy](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/ChangeUpdatePolicy)</code></td>
</tr>
<tr>
<td>Abilita gli aggiornamenti automatici per il componente aggiuntivo del cluster Fluentd.</td>
<td><code>[ibmcloud ks logging-autoupdate-enable](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_autoupdate_enable)</code></td>
<td><code>[PUT /v1/logging/{idOrName}/updatepolicy](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/ChangeUpdatePolicy)</code></td>
</tr>
<tr>
<td>Raccogli un'istantanea dei log del server API in un bucket di {{site.data.keyword.cos_full_notm}}.</td>
<td><code>[ibmcloud ks logging-collect](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_collect)</code></td>
<td>[POST /v1/log-collector/{idOrName}/masterlogs](https://containers.cloud.ibm.com/global/swagger-global-api/#/log45collector/CreateMasterLogCollection)</td>
</tr>
<tr>
<td>Visualizza lo stato della richiesta di istantanea del log del server API.</td>
<td><code>[ibmcloud ks logging-collect-status](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_collect_status)</code></td>
<td>[GET /v1/log-collector/{idOrName}/masterlogs](https://containers.cloud.ibm.com/global/swagger-global-api/#/log45collector/GetMasterLogCollectionStatus)</td>
</tr>
<tr>
<td>Crea una configurazione di inoltro dei log per l'origine log <code>kube-audit</code>.</td>
<td><code>[ibmcloud ks logging-config-create](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_logging_create)</code></td>
<td><code>[POST /v1/logging/{idOrName}/loggingconfig/{logSource}](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/CreateLoggingConfig)</code></td>
</tr>
<tr>
<td>Elimina una configurazione di inoltro dei log per l'origine log <code>kube-audit</code>.</td>
<td><code>[ibmcloud ks logging-config-rm](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_logging_rm)</code></td>
<td><code>[DELETE /v1/logging/{idOrName}/loggingconfig/{logSource}/{id}](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/DeleteLoggingConfig)</code></td>
</tr>
</tbody>
</table>

<br />


## Ruoli del servizio {{site.data.keyword.Bluemix_notm}} IAM
{: #service}

A ogni utente a cui viene assegnato un ruolo di accesso al servizio {{site.data.keyword.Bluemix_notm}} IAM viene assegnato automaticamente anche un ruolo RBAC (role-based access control) di Kubernetes corrispondente in uno specifico spazio dei nomi. Per ulteriori informazioni sui ruoli di accesso al servizio, vedi [Ruoli del servizio {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform). Non assegnare contemporaneamente i ruoli della piattaforma {{site.data.keyword.Bluemix_notm}} IAM come ruoli del servizio. Devi assegnare i ruoli della piattaforma e del servizio separatamente.
{: shortdesc}

Stai cercando le azioni Kubernetes concesse da ogni ruolo del servizio tramite RBAC? Vedi [Autorizzazioni delle risorse Kubernetes per ogni ruolo RBAC](#rbac_ref). Per ulteriori informazioni sui ruoli RBAC, vedi [Assegnazione delle autorizzazioni RBAC](/docs/containers?topic=containers-users#role-binding) e [Estensione delle autorizzazioni esistenti aggregando i ruoli cluster](https://cloud.ibm.com/docs/containers?topic=containers-users#rbac_aggregate)
{: tip}

La seguente tabella mostra le autorizzazioni delle risorse Kubernetes concesse da ciascun ruolo del servizio e il ruolo RBAC corrispondente.

<table>
<caption>Autorizzazioni delle risorse Kubernetes per servizio e ruoli RBAC corrispondenti</caption>
<thead>
    <th id="service-role">Ruolo del servizio</th>
    <th id="rbac-role">Ruolo RBAC corrispondente, bind e ambito</th>
    <th id="kube-perm">Autorizzazioni delle risorse Kubernetes</th>
</thead>
<tbody>
  <tr>
    <td id="service-role-reader" headers="service-role">Ruolo lettore</td>
    <td headers="service-role-reader rbac-role">Quando l'ambito è delimitato a un solo spazio dei nomi: ruolo cluster <strong><code>view</code></strong> applicato dal bind del ruolo
<strong><code>ibm-view</code></strong> in quello spazio dei nomi</br><br>Quando l'ambito è delimitato a tutti gli spazi dei nomi: ruolo cluster <strong><code>view</code></strong> applicato dal bind del ruolo <strong><code>ibm-view</code></strong> in ogni spazio dei nomi del cluster</td>
    <td headers="service-role-reader kube-perm"><ul>
      <li>Accesso in lettura alle risorse in uno spazio dei nomi</li>
      <li>Nessun accesso in lettura ai ruoli e ai bind del ruolo o ai segreti Kubernetes</li>
      <li>Accesso al dashboard Kubernetes per visualizzare le risorse in uno spazio dei nomi</li></ul>
    </td>
  </tr>
  <tr>
    <td id="service-role-writer" headers="service-role">Ruolo scrittore</td>
    <td headers="service-role-writer rbac-role">Quando l'ambito è delimitato a un solo spazio dei nomi: ruolo cluster <strong><code>edit</code></strong> applicato dal bind del ruolo <strong><code>ibm-edit</code></strong> in quello spazio dei nomi</br><br>Quando l'ambito è delimitato a tutti gli spazi dei nomi: ruolo cluster <strong><code>edit</code></strong> applicato dal bind del ruolo <strong><code>ibm-edit</code></strong> in ogni spazio dei nomi del cluster</td>
    <td headers="service-role-writer kube-perm"><ul><li>Accesso in lettura/scrittura alle risorse in uno spazio dei nomi</li>
    <li>Nessun accesso in lettura/scrittura ai ruoli e ai bind del ruolo</li>
    <li>Accesso al dashboard Kubernetes per visualizzare le risorse in uno spazio dei nomi</li></ul>
    </td>
  </tr>
  <tr>
    <td id="service-role-manager" headers="service-role">Ruolo gestore</td>
    <td headers="service-role-manager rbac-role">Quando l'ambito è delimitato a un solo spazio dei nomi: ruolo cluster <strong><code>admin</code></strong> applicato dal bind del ruolo <strong><code>ibm-operate</code></strong> in quello spazio dei nomi</br><br>Quando l'ambito è delimitato a tutti gli spazi dei nomi: ruolo cluster <strong><code>cluster-admin</code></strong> applicato dal bind del ruolo cluster <strong><code>ibm-admin</code></strong></td> che si applica a tutti gli spazi dei nomi
    <td headers="service-role-manager kube-perm">Quando l'ambito è delimitato a un solo spazio dei nomi:
      <ul><li>Accesso in lettura/scrittura a tutte le risorse in uno spazio dei nomi ma non alla quota della risorsa o allo spazio dei nomi stesso</li>
      <li>Creare ruoli RBAC e bind del ruolo in uno spazio dei nomi</li>
      <li>Accedere al dashboard Kubernetes per visualizzare tutte le risorse in uno spazio dei nomi</li></ul>
    </br>Quando l'ambito è delimitato a tutti gli spazi dei nomi:
        <ul><li>Accesso in lettura/scrittura a tutte risorse in ogni spazio dei nomi</li>
        <li>Creare ruoli RBAC e bind del ruolo in uno spazio dei nomi oppure ruoli cluster e bind del ruolo cluster in tutti gli spazi dei nomi</li>
        <li>Accedere al dashboard Kubernetes</li>
        <li>Creare una risorsa Ingress che rende le applicazioni disponibili al pubblico</li>
        <li>Esaminare le metriche del cluster, ad esempio, con i comandi <code>kubectl top pods</code>, <code>kubectl top nodes</code> o <code>kubectl get nodes</code></li></ul>
    </td>
  </tr>
</tbody>
</table>

<br />


## Autorizzazioni delle risorse Kubernetes per ogni ruolo RBAC
{: #rbac_ref}

A ogni utente a cui viene assegnato un ruolo di accesso al servizio {{site.data.keyword.Bluemix_notm}} IAM viene assegnato automaticamente anche un ruolo RBAC (role-based access control) di Kubernetes predefinito corrispondente. Se intendi gestire i tuoi propri ruoli RBAC Kubernetes personalizzati, vedi [Creazione di autorizzazioni RBAC personalizzate per utenti, gruppi o account di servizio](/docs/containers?topic=containers-users#rbac).
{: shortdesc}

Ti chiedi se hai le autorizzazioni corrette per eseguire un determinato comando `kubectl` su una risorsa in uno spazio dei nomi? Prova il comando [`kubectl auth can-i` ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#-em-can-i-em-).
{: tip}

La seguente tabella mostra le autorizzazioni concesse da ciascun ruolo RBAC alle singole risorse Kubernetes. Le autorizzazioni sono mostrate come verbi che un utente con quel ruolo può completare sulla risorsa, ad esempio "get", "list", "describe", "create" o "delete".

<table>
 <caption>Autorizzazioni delle risorse Kubernetes concesse da ciascun ruolo RBAC predefinito</caption>
 <thead>
  <th>Risorsa Kubernetes</th>
  <th><code>view</code></th>
  <th><code>edit</code></th>
  <th><code>admin</code> e <code>cluster-admin</code></th>
 </thead>
<tbody>
<tr>
  <td><code>bindings</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
</tr><tr>
  <td><code>configmaps</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>cronjobs.batch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>daemonsets.apps </code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>daemonsets.extensions</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>deployments.apps</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>deployments.apps/rollback</code></td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>deployments.apps/scale</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>deployments.extensions</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>deployments.extensions/rollback</code></td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>deployments.extensions/scale</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>endpoints</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>events</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
</tr><tr>
  <td><code>horizontalpodautoscalers.autoscaling</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>ingresses.extensions</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>jobs.batch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>limitranges</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
</tr><tr>
  <td><code>localsubjectaccessreviews</code></td>
  <td>-</td>
  <td>-</td>
  <td><code>create</code></td>
</tr><tr>
  <td><code>namespaces</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></br>**Solo amministratore cluster:** <code>create</code>, <code>delete</code></td>
</tr><tr>
  <td><code>namespaces/status</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
</tr><tr>
  <td><code>networkpolicies</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>networkpolicies.extensions</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>persistentvolumeclaims</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>poddisruptionbudgets.policy</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>pods</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>top</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>pods/attach</code></td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>pods/exec</code></td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>pods/log</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
</tr><tr>
  <td><code>pods/portforward</code></td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>pods/proxy</code></td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>pods/status</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
</tr><tr>
  <td><code>replicasets.apps</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>replicasets.apps/scale</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>replicasets.extensions</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>replicasets.extensions/scale</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>replicationcontrollers</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>replicationcontrollers/scale</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>replicationcontrollers/status</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
</tr><tr>
  <td><code>replicationcontrollers.extensions/scale</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>resourcequotas</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
</tr><tr>
  <td><code>resourcequotas/status</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
</tr><tr>
  <td><code>rolebindings</code></td>
  <td>-</td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>roles</code></td>
  <td>-</td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>secrets</code></td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>serviceaccounts</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code>, <code>impersonate</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code>, <code>impersonate</code></td>
</tr><tr>
  <td><code>services</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>services/proxy</code></td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>statefulsets.apps</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>statefulsets.apps/scale</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr>
</tbody>
</table>

<br />


## Ruoli Cloud Foundry
{: #cloud-foundry}

I ruoli di Cloud Foundry concedono l'accesso a organizzazioni e spazi all'interno dell'account. Per visualizzare l'elenco dei servizi basati su Cloud Foundry in {{site.data.keyword.Bluemix_notm}}, esegui `ibmcloud service list`. Per ulteriori informazioni, vedi tutti i [ruoli disponibili per l'organizzazione e lo spazio](/docs/iam?topic=iam-cfaccess) o la procedura per la [gestione dell'accesso a Cloud Foundry](/docs/iam?topic=iam-mngcf) nella documentazione di {{site.data.keyword.Bluemix_notm}} IAM.
{: shortdesc}

La seguente tabella mostra i ruoli di Cloud Foundry richiesti per le autorizzazioni all'azione nel cluster.

<table>
  <caption>Autorizzazioni di gestione del cluster per ruolo Cloud Foundry</caption>
  <thead>
    <th>Ruolo Cloud Foundry</th>
    <th>Autorizzazioni di gestione del cluster</th>
  </thead>
  <tbody>
  <tr>
    <td>Ruolo spazio: gestore</td>
    <td>Gestire l'accesso utente a uno spazio {{site.data.keyword.Bluemix_notm}}</td>
  </tr>
  <tr>
    <td>Ruolo spazio: sviluppatore</td>
    <td>
      <ul><li>Creare le istanze del servizio {{site.data.keyword.Bluemix_notm}}</li>
      <li>Eseguire il bind delle istanze del servizio {{site.data.keyword.Bluemix_notm}}
ai cluster</li>
      <li>Visualizzare i log dalla configurazione di inoltro log di un cluster a livello di spazio</li></ul>
    </td>
  </tr>
  </tbody>
</table>

## Ruoli dell'infrastruttura
{: #infra}

Un utente con il ruolo di accesso dell'infrastruttura **Super utente** [imposta la chiave API per una regione e un gruppo di risorse](/docs/containers?topic=containers-users#api_key) in modo che possano essere eseguite le azioni dell'infrastruttura (o, più raramente, [imposta manualmente credenziali dell'account differenti](/docs/containers?topic=containers-users#credentials)). Quindi, le azioni di infrastruttura che altri utenti nell'account possono eseguire sono autorizzate tramite i ruoli della piattaforma IAM {{site.data.keyword.Bluemix_notm}}. Non dovrai modificare le autorizzazioni dell'infrastruttura IBM Cloud (SoftLayer) degli altri utenti. Utilizza la seguente tabella per personalizzare le autorizzazioni dell'infrastruttura IBM Cloud (SoftLayer) degli utenti solo quando non puoi assegnare il ruolo **Super utente** all'utente che imposta la chiave API. Per le istruzioni per assegnare le autorizzazioni, vedi [Personalizzazione delle autorizzazioni di infrastruttura](/docs/containers?topic=containers-users#infra_access).
{: shortdesc}



La seguente tabella mostra le autorizzazioni dell'infrastruttura richieste per completare gruppi di attività comuni.

<table>
<caption>Autorizzazioni dell'infrastruttura comunemente richieste per {{site.data.keyword.containerlong_notm}}</caption>
<thead>
  <th>Attività comuni in {{site.data.keyword.containerlong_notm}}</th>
  <th>Autorizzazioni dell'infrastruttura richieste per categoria</th>
</thead>
<tbody>
<tr>
<td>
  <strong>Autorizzazioni minime</strong>: <ul>
  <li>Crea un cluster.</li></ul></td>
<td>
<strong>Account</strong>: <ul>
<li>Aggiungi server</li></ul>
  <strong>Dispositivi</strong>:<ul>
  <li>Per i nodi di lavoro bare metal: Visualizza i dettagli hardware</li>
  <li>Gestione remota IPMI</li>
  <li>Ricaricamenti del sistema operativo e kernel di salvataggio</li>
  <li>Per i nodi di lavoro VM: Visualizza i dettagli del Virtual Server</li></ul></td>
</tr>
<tr>
<td>
<strong>Amministrazione cluster</strong>:<ul>
  <li>Crea, aggiorna ed elimina i cluster.</li>
  <li>Aggiungi, ricarica e riavvia i nodi di lavoro.</li>
  <li>Visualizza le VLAN.</li>
  <li>Crea sottoreti.</li>
  <li>Distribuisci i pod e i servizi di bilanciamento del carico.</li></ul>
  </td><td>
<strong>Account</strong>:<ul>
  <li>Aggiungi server</li>
  <li>Annulla server</li></ul>
<strong>Dispositivi</strong>:<ul>
  <li>Per i nodi di lavoro bare metal: Visualizza i dettagli hardware</li>
  <li>Gestione remota IPMI</li>
  <li>Ricaricamenti del sistema operativo e kernel di salvataggio</li>
  <li>Per i nodi di lavoro VM: Visualizza i dettagli del Virtual Server</li></ul>
<strong>Rete</strong>:<ul>
  <li>Aggiungi calcoli con la porta di rete pubblica</li></ul>
<p class="important">Devi anche assegnare all'utente la capacità di gestire i casi di supporto. Vedi il passo 8 di [Personalizzazione delle autorizzazioni di infrastruttura](/docs/containers?topic=containers-users#infra_access).</p>
</td>
</tr>
<tr>
<td>
  <strong>Archiviazione</strong>: <ul>
  <li>Crea attestazioni del volume persistente per il provisioning di volumi persistenti.</li>
  <li>Crea e gestisci risorse dell'infrastruttura di archiviazione.</li></ul></td>
<td>
<strong>Account</strong>:<ul>
  <li>Aggiungi/esegui upgrade dell'archiviazione (StorageLayer)</li></ul>
<strong>Servizi</strong>:<ul>
  <li>Gestione archiviazione</li></ul></td>
</tr>
<tr>
<td>
  <strong>Rete privata</strong>: <ul>
  <li>Gestisci le VLAN private per la rete in cluster</li>
  <li>Configura la connettività VPN per le reti private.</li></ul></td>
<td>
  <strong>Rete</strong>:<ul>
  <li>Gestisci le rotte di sottorete della rete</li></ul></td>
</tr>
<tr>
<td>
  <strong>Rete pubblica</strong>:<ul>
  <li>Configura la rete Ingress o programma di bilanciamento del carico pubblica per esporre le applicazioni.</li></ul></td>
<td>
<strong>Dispositivi</strong>:<ul>
<li>Gestisci controllo porta</li>
  <li>Modifica nome host/dominio</li></ul>
<strong>Rete</strong>:<ul>
  <li>Aggiungi indirizzi IP</li>
  <li>Gestisci le rotte di sottorete della rete</li>
  <li>Aggiungi calcoli con la porta di rete pubblica</li></ul>
<strong>Servizi</strong>:<ul>
  <li>Gestisci DNS</li>
  <li>Visualizza certificati (SSL)</li>
  <li>Gestisci certificati (SSL)</li></ul></td>
</tr>
</tbody>
</table>
