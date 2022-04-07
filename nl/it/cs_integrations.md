---

copyright:
  years: 2014, 2019
lastupdated: "2019-04-09"

keywords: kubernetes, iks, helm

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


# Integrazione dei servizi
{: #integrations}

Puoi utilizzare vari servizi esterni e servizi di catalogo con un cluster Kubernetes standard in {{site.data.keyword.containerlong}}.
{:shortdesc}


## Servizi DevOps
{: #devops_services}
<table summary="La tabella mostra i servizi disponibili che puoi aggiungere al tuo cluster per aggiungere ulteriori funzionalità DevOps. Le righe devono essere lette da sinistra a destra, con il nome del servizio nella colonna uno e una descrizione del servizio nella colonna due.">
<caption>Servizi DevOps</caption>
<thead>
<tr>
<th>Servizio</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.cfee_full_notm}}</td>
<td>Distribuisci e gestisci la tua piattaforma Cloud Foundry su un cluster Kubernetes per sviluppare, assemblare, distribuire e gestire applicazioni native del cloud e sfruttare l'ecosistema {{site.data.keyword.Bluemix_notm}} per associare ulteriori servizi alle tue applicazioni. Quando crei un'istanza {{site.data.keyword.cfee_full_notm}}, devi configurare il tuo cluster Kubernetes scegliendo il tipo di macchina e le VLAN per i tuoi nodi di lavoro. Il tuo cluster viene quindi fornito con {{site.data.keyword.containerlong_notm}} e {{site.data.keyword.cfee_full_notm}} viene distribuito automaticamente nel tuo cluster. Per ulteriori informazioni su come configurare {{site.data.keyword.cfee_full_notm}}, vedi l'[Esercitazione introduttiva](/docs/cloud-foundry?topic=cloud-foundry-getting-started#getting-started). </td>
</tr>
<tr>
<td>Codeship</td>
<td>Puoi utilizzare <a href="https://codeship.com" target="_blank">Codeship <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a> per l'integrazione e la consegna continua dei contenitori. Per ulteriori informazioni, consulta <a href="https://www.ibm.com/blogs/bluemix/2017/10/using-codeship-pro-deploy-workloads-ibm-container-service/" target="_blank">Using Codeship Pro To Deploy Workloads to {{site.data.keyword.containerlong_notm}} <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>. </td>
</tr>
<tr>
<td>Grafeas</td>
<td>[Grafeas ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://grafeas.io) è un servizio CI/CD open source che fornisce un metodo comune per richiamare, memorizzare e scambiare i metadati durante il processo della catena di fornitura del software. Ad esempio, se integri Grafeas nel processo di build dell'applicazione, Grafeas può memorizzare informazioni sull'initiator della richiesta di build, sui risultati della scansione delle vulnerabilità e sulla convalida del controllo qualità in modo che tu possa decidere in maniera consapevole se un'applicazione può essere distribuita in produzione. Puoi utilizzare questi metadati nei controlli o per dimostrare la conformità per la tua catena di fornitura del software. </td>
</tr>
<tr>
<td>Helm</td>
<td> <a href="https://helm.sh" target="_blank">Helm <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a> è un gestore del pacchetto Kubernetes. Puoi creare nuovi grafici Helm o utilizzare grafici Helm preesistenti per definire, installare e aggiornare applicazioni Kubernetes complesse eseguite nei cluster {{site.data.keyword.containerlong_notm}}. <p>Per ulteriori informazioni, vedi [Configurazione di Helm in {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-integrations#helm).</p></td>
</tr>
<tr>
<td>{{site.data.keyword.contdelivery_full}}</td>
<td>Automatizza le creazioni delle tue applicazioni e le distribuzioni del contenitore ai cluster Kubernetes utilizzando una toolchain. Per informazioni sulla configurazione, consulta il blog <a href="https://developer.ibm.com/recipes/tutorials/deploy-kubernetes-pods-to-the-bluemix-container-service-using-devops-pipelines/" target="_blank">Deploy Kubernetes pods to the {{site.data.keyword.containerlong_notm}} using DevOps Pipelines <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>. </td>
</tr>
<tr>
<td>Istio su {{site.data.keyword.containerlong_notm}}</td>
<td><a href="https://www.ibm.com/cloud/info/istio" target="_blank">Istio <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a> è un servizio open source che offre agli sviluppatori un modo per connettere, proteggere, gestire e monitorare una rete di microservizi, nota anche come rete (mesh) di servizi, su piattaforme di orchestrazione cloud. Istio su {{site.data.keyword.containerlong}} fornisce un'istallazione a fase unica di Istio nel tuo cluster tramite un componente aggiuntivo gestito. Con un solo clic, puoi attivare tutti i componenti core di Istio, ulteriori funzioni di traccia, monitoraggio e visualizzazione e l'applicazione di esempio BookInfo. Per iniziare, vedi [Utilizzo del componente aggiuntivo Istio gestito (beta)](/docs/containers?topic=containers-istio).</td>
</tr>
<tr>
<td>Jenkins X</td>
<td>Jenkins X è una piattaforma di integrazione e fornitura continua nativa di Kubernetes che puoi utilizzare per automatizzare il tuo processo di build. Per ulteriori informazioni su come installarlo su {{site.data.keyword.containerlong_notm}}, vedi [Introducing the Jenkins X open source project ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/blogs/bluemix/2018/08/installing-jenkins-x-on-ibm-cloud-kubernetes-service/).</td>
</tr>
<tr>
<td>Knative</td>
<td>[Knative ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/knative/docs) è una piattaforma open source che è stata sviluppata da IBM, Google, Pivotal, Red Hat, Cisco e altri con l'obiettivo di estendere le funzionalità di Kubernetes per aiutarti a creare applicazioni senza server e inserite in contenitori moderne e basate sul sorgente sul tuo cluster Kubernetes. La piattaforma utilizza un approccio congruente tra i linguaggi di programmazione e i framework per astrarre il carico operativo derivante dal creare, distribuire e gestire carichi di lavoro in Kubernetes in modo che gli sviluppatori possano concentrarsi su quello che conta di più per loro: il codice sorgente. Per ulteriori informazioni, vedi [Esercitazione: Utilizzo di Knative gestito per eseguire le applicazioni senza server nei cluster Kubernetes](/docs/containers?topic=containers-knative_tutorial#knative_tutorial). </td>
</tr>
</tbody>
</table>

<br />



## Servizi di registrazione e monitoraggio
{: #health_services}
<table summary="La tabella mostra i servizi disponibili che puoi aggiungere al tuo cluster per aggiungere ulteriori funzionalità di registrazione e monitoraggio. Le righe devono essere lette da sinistra a destra, con il nome del servizio nella colonna uno e una descrizione del servizio nella colonna due.">
<caption>Servizi di registrazione e monitoraggio</caption>
<thead>
<tr>
<th>Servizio</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>CoScale</td>
<td>Monitora i nodi di lavoro, i contenitori, le serie di repliche, i controller della replica e i servizi con <a href="https://www.newrelic.com/coscale" target="_blank">CoScale <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>. Per ulteriori informazioni, consulta <a href="https://www.ibm.com/blogs/bluemix/2017/06/monitoring-ibm-bluemix-container-service-coscale/" target="_blank">Monitoring {{site.data.keyword.containerlong_notm}} with CoScale <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>. </td>
</tr>
<tr>
<td>Datadog</td>
<td>Monitora il tuo cluster e visualizza le metriche delle prestazioni dell'applicazione e dell'infrastruttura con <a href="https://www.datadoghq.com/" target="_blank">Datadog <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>. Per ulteriori informazioni, consulta <a href="https://www.ibm.com/blogs/bluemix/2017/07/monitoring-ibm-bluemix-container-service-datadog/" target="_blank">Monitoring {{site.data.keyword.containerlong_notm}} with Datadog <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>. </td>
</tr>
<tr>
<td>{{site.data.keyword.cloudaccesstrailfull}}</td>
<td>Monitora l'attività amministrativa eseguita nel tuo cluster analizzando i log tramite Grafana. Per ulteriori informazioni sul servizio, vedi la documentazione di [Activity Tracker](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started-with-cla). Per ulteriori informazioni sui tipi di eventi che puoi tracciare, vedi [Eventi Activity Tracker](/docs/containers?topic=containers-at_events).</td>
</tr>
<tr>
<td>{{site.data.keyword.la_full_notm}}</td>
<td>Aggiungi funzionalità di gestione dei log al tuo cluster distribuendo LogDNA come servizio di terze parti ai tuoi nodi di lavoro per gestire i log dai contenitori di pod. Per ulteriori informazioni, vedi [Gestione dei log di cluster Kubernetes con {{site.data.keyword.loganalysisfull_notm}} with LogDNA](/docs/services/Log-Analysis-with-LogDNA/tutorials?topic=LogDNA-kube#kube).</td>
</tr>
<tr>
<td>{{site.data.keyword.mon_full_notm}}</td>
<td>Ottieni visibilità operativa sulle prestazioni e sull'integrità delle tue applicazioni distribuendo Sysdig come servizio di terze parti ai tuoi nodi di lavoro per inoltrare le metriche a {{site.data.keyword.monitoringlong}}. Per ulteriori informazioni, vedi [Analizza le metriche per un'applicazione distribuita in un cluster Kubernetes](/docs/services/Monitoring-with-Sysdig/tutorials?topic=Sysdig-kubernetes_cluster#kubernetes_cluster). </td>
</tr>
<tr>
<td>Instana</td>
<td> <a href="https://www.instana.com/" target="_blank">Instana <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a> fornisce il monitoraggio delle prestazioni di infrastruttura e applicazioni con una GUI che rileva e associa automaticamente le tue applicazioni. Instana cattura ogni richiesta alle tue applicazioni, che puoi utilizzare per risolvere i problemi ed eseguire l'analisi delle cause principali per evitare che i problemi si ripetano. Per ulteriori informazioni, controlla il post del blog relativo alla <a href="https://www.instana.com/blog/precise-visibility-applications-ibm-bluemix-container-service/" target="_blank">distribuzione di Instana in {{site.data.keyword.containerlong_notm}} <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>.</td>
</tr>
<tr>
<td>Prometheus</td>
<td>Prometheus è uno strumento di avviso, registrazione e monitoraggio open source che è stato specificatamente progettato per Kubernetes. Prometheus richiama informazioni dettagliate sul cluster, sui nodi di lavoro e sull'integrità della distribuzione in base alle informazioni di registrazione di Kubernetes. La CPU, la memoria, I/O e l'attività di rete vengono raccolti per ogni contenitore in esecuzione in un cluster. Puoi utilizzare i dati raccolti in query o avvisi personalizzati per monitorare le prestazioni e i carichi di lavoro nel tuo cluster.

<p>Per utilizzare Prometheus, segui le <a href="https://github.com/coreos/prometheus-operator/tree/master/contrib/kube-prometheus" target="_blank">istruzioni CoreOS <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>.</p>
</td>
</tr>
<tr>
<td>Sematext</td>
<td>Visualizza le metriche e i log per le tue applicazioni inserite in un contenitore utilizzando <a href="https://sematext.com/" target="_blank">Sematext <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>. Per ulteriori informazioni, consulta <a href="https://www.ibm.com/blogs/bluemix/2017/09/monitoring-logging-ibm-bluemix-container-service-sematext/" target="_blank">Monitoring and logging for containers with Sematext <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>. </td>
</tr>
<tr>
<td>Splunk</td>
<td>Importa e cerca i dati di registrazione, oggetti e metriche di Kubernetes in Splunk usando Splunk Connect per Kubernetes. Splunk Connect per Kubernetes è una raccolta di grafici Helm che implementano una distribuzione supportata da Splunk di Fluentd nel tuo cluster Kubernetes, un plugin HEC (HTTP Event Collector) Fluentd basato su Splunk per inviare log e metadati e una distribuzione di metriche che cattura le metriche del tuo cluster. Per ulteriori informazioni, vedi <a href="https://www.ibm.com/blogs/bluemix/2019/02/solving-business-problems-with-splunk-on-ibm-cloud-kubernetes-service/" target="_blank">Solving Business Problems with Splunk on {{site.data.keyword.containerlong_notm}} <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>.</td>
</tr>
<tr>
<td>Weave Scope</td>
<td>Weave Scope fornisce un diagramma visivo delle risorse in un cluster Kubernetes,
inclusi i servizi, i pod, i contenitori, i processi, i nodi e altro. Scope fornisce metriche interattive
per la CPU e la memoria e inoltre fornisce strumenti per inserire ed eseguire in un contenitore.<p>Per ulteriori informazioni, consulta [Visualizzazione delle risorse del cluster Kubernetes con Weave Scope e {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-integrations#weavescope).</p></li></ol>
</td>
</tr>
</tbody>
</table>

<br />



## Servizi di sicurezza
{: #security_services}

Vuoi una visione completa di come integrare i servizi di sicurezza {{site.data.keyword.Bluemix_notm}} con il tuo cluster? Controlla l'[esercitazione su come applicare la sicurezza end-to-end a un'applicazione cloud](/docs/tutorials?topic=solution-tutorials-cloud-e2e-security).
{: shortdesc}

<table summary="La tabella mostra i servizi disponibili che puoi aggiungere al tuo cluster per aggiungere ulteriori funzionalità di sicurezza. Le righe devono essere lette da sinistra a destra, con il nome del servizio nella colonna uno e una descrizione del servizio nella colonna due.">
<caption>Servizi di sicurezza</caption>
<thead>
<tr>
<th>Servizio</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
  <tr id="appid">
    <td>{{site.data.keyword.appid_full}}</td>
    <td>Aggiungi un livello di sicurezza alle tue applicazioni con [{{site.data.keyword.appid_short}}](/docs/services/appid?topic=appid-getting-started) richiedendo agli utenti di effettuare l'accesso. Per autenticare le richieste HTTP/HTTPS web o API nella tua applicazione, puoi integrare {{site.data.keyword.appid_short_notm}} con il tuo servizio Ingress utilizzando l'[annotazione Ingress per l'autenticazione {{site.data.keyword.appid_short_notm}}](/docs/containers?topic=containers-ingress_annotation#appid-auth).</td>
  </tr>
<tr>
<td>Aqua Security</td>
  <td>Come un supplemento al <a href="/docs/services/va?topic=va-va_index" target="_blank">Controllo vulnerabilità</a>, puoi utilizzare <a href="https://www.aquasec.com/" target="_blank">Aqua Security <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a> per migliorare la sicurezza delle distribuzioni del contenitore riducendo cosa è consentito fare alla tua applicazione. Per ulteriori informazioni, consulta <a href="https://blog.aquasec.com/securing-container-deployments-on-bluemix-with-aqua-security" target="_blank">Securing container deployments on {{site.data.keyword.Bluemix_notm}} with Aqua Security <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>. </td>
</tr>
<tr>
<td>{{site.data.keyword.cloudcerts_full}}</td>
<td>Puoi utilizzare <a href="/docs/services/certificate-manager?topic=certificate-manager-getting-started#getting-started" target="_blank">{{site.data.keyword.cloudcerts_long}} <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a> per archiviare e gestire i certificati SSL per le tue applicazioni. Per ulteriori informazioni, consulta <a href="https://www.ibm.com/blogs/bluemix/2018/01/use-ibm-cloud-certificate-manager-ibm-cloud-container-service-deploy-custom-domain-tls-certificates/" target="_blank">Use {{site.data.keyword.cloudcerts_long_notm}} with {{site.data.keyword.containerlong_notm}} to deploy custom domain TLS Certificates <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>. </td>
</tr>
<tr>
  <td>{{site.data.keyword.datashield_full}} (Beta)</td>
  <td>Puoi utilizzare <a href="/docs/services/data-shield?topic=data-shield-getting-started#getting-started" target="_blank">{{site.data.keyword.datashield_short}} <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a> per crittografare la tua memoria di dati. {{site.data.keyword.datashield_short}} è integrato con la tecnologia di Intel® Software Guard Extensions (SGX) e Fortanix® in modo che il codice e i dati dei carichi di lavoro del tuo contenitore {{site.data.keyword.Bluemix_notm}} siano protetti durante l'utilizzo. Il codice e i dati dell'applicazione vengono eseguiti in enclavi con protezione avanzata della CPU, che sono aree di memoria attendibili sul nodo di lavoro che proteggono aspetti critici dell'applicazione, aiutando a mantenere il codice e i dati riservati e invariati.</td>
</tr>
<tr>
  <td>{{site.data.keyword.registrylong}}</td>
  <td>Imposta il tuo repository delle immagini Docker protetto in cui puoi memorizzare e condividere in modo sicuro le immagini tra gli utenti cluster. Per ulteriori informazioni, vedi la <a href="/docs/services/Registry?topic=registry-getting-started" target="_blank">{{site.data.keyword.registrylong}} documentazione <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>.</td>
</tr>
<tr>
  <td>{{site.data.keyword.keymanagementservicefull}}</td>
  <td>Crittografa i segreti Kubernetes che si trovano nel tuo cluster abilitando {{site.data.keyword.keymanagementserviceshort}}. La crittografia dei tuoi segreti Kubernetes impedisce agli utenti non autorizzati di accedere a informazioni riservate sui cluster.<br>Per la configurazione, vedi <a href="/docs/containers?topic=containers-encryption#keyprotect">Crittografia dei segreti Kubernetes mediante {{site.data.keyword.keymanagementserviceshort}}</a>.<br>Per ulteriori informazioni, vedi la <a href="/docs/services/key-protect?topic=key-protect-getting-started-tutorial" target="_blank">{{site.data.keyword.keymanagementserviceshort}} documentazione <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>.</td>
</tr>
<tr>
<td>NeuVector</td>
<td>Proteggi i contenitori con un firewall nativo nel cloud utilizzando <a href="https://neuvector.com/" target="_blank">NeuVector <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>. Per ulteriori informazioni, consulta <a href="https://www.ibm.com/us-en/marketplace/neuvector-container-security" target="_blank">NeuVector Container Security <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>. </td>
</tr>
<tr>
<td>Twistlock</td>
<td>Come un supplemento al <a href="/docs/services/va?topic=va-va_index" target="_blank">Controllo vulnerabilità</a>, puoi utilizzare <a href="https://www.twistlock.com/" target="_blank">Twistlock <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a> per gestire i firewall, la protezione dalle minacce e la risposta agli incidenti. Per ulteriori informazioni, consulta <a href="https://www.ibm.com/blogs/bluemix/2017/07/twistlock-ibm-bluemix-container-service/" target="_blank">Twistlock on {{site.data.keyword.containerlong_notm}} <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>. </td>
</tr>
</tbody>
</table>

<br />



## Servizi di archiviazione
{: #storage_services}
<table summary="La tabella mostra i servizi disponibili che puoi aggiungere al tuo cluster per aggiungere funzionalità di archiviazione persistente. Le righe devono essere lette da sinistra a destra, con il nome del servizio nella colonna uno e una descrizione del servizio nella colonna due.">
<caption>Servizi di archiviazione</caption>
<thead>
<tr>
<th>Servizio</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
  <td>Heptio Velero</td>
  <td>Puoi utilizzare <a href="https://github.com/heptio/velero" target="_blank">Heptio Velero <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a> per eseguire il backup e il ripristino delle risorse del cluster e dei volumi persistenti. Per ulteriori informazioni, vedi Heptio Velero <a href="https://github.com/heptio/velero/blob/release-0.9/docs/use-cases.md" target="_blank">Use cases for disaster recovery and cluster migration <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>.</td>
</tr>
<tr>
  <td>{{site.data.keyword.Bluemix_notm}} Block Storage</td>
  <td>[{{site.data.keyword.Bluemix_notm}} Block Storage](/docs/infrastructure/BlockStorage?topic=BlockStorage-getting-started#getting-started) è un'archiviazione iSCSI persistente e ad alte prestazioni che puoi aggiungere alle tue applicazioni utilizzando i volumi persistenti (PV, persistent volume) Kubernetes. Utilizza l'archiviazione blocchi per distribuire applicazioni con stato in una singola zona o come archiviazione ad alte prestazioni per i singoli pod. Per ulteriori informazioni su come eseguire il provisioning dell'archiviazione blocchi nel tuo cluster, vedi [Archiviazione dei dati su {{site.data.keyword.Bluemix_notm}} Block Storage](/docs/containers?topic=containers-block_storage#block_storage)</td>
  </tr>
<tr>
  <td>{{site.data.keyword.cos_full}}</td>
  <td>I dati memorizzati con {{site.data.keyword.cos_short}} vengono crittografati e diffusi tra più posizioni geografiche e vi si accede su HTTP utilizzando un'API REST. Puoi utilizzare l'[immagine ibm-backup-restore](/docs/services/RegistryImages/ibm-backup-restore?topic=RegistryImages-ibmbackup_restore_starter) per configurare il servizio per creare backup monouso o pianificati per i dati nei tuoi cluster. Per informazioni generali sul servizio, vedi la <a href="/docs/services/cloud-object-storage?topic=cloud-object-storage-about-ibm-cloud-object-storage" target="_blank">{{site.data.keyword.cos_short}} documentazione <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>.</td>
</tr>
  <tr>
  <td>{{site.data.keyword.Bluemix_notm}} File Storage</td>
  <td>[{{site.data.keyword.Bluemix_notm}} File Storage](/docs/infrastructure/FileStorage?topic=FileStorage-getting-started#getting-started) è un'archiviazione file basata su NFS persistente, rapida, collegata alla rete e flessibile che puoi aggiungere alle tue applicazioni utilizzando i volumi persistenti (PV, persistent volume) Kubernetes. Puoi scegliere tra i livelli di archiviazione predefiniti con dimensioni in GB e IOPS che soddisfano i requisiti dei tuoi carichi di lavoro. Per ulteriori informazioni su come eseguire il provisioning dell'archiviazione file nel tuo cluster, vedi [Archiviazione dei dati su {{site.data.keyword.Bluemix_notm}} File Storage](/docs/containers?topic=containers-file_storage#file_storage).</td>
  </tr>
  <tr>
    <td>Portworx</td>
    <td>[Portworx ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://portworx.com/products/introduction/) è una soluzione SDS (software-defined storage) altamente disponibile che puoi utilizzare per gestire l'archiviazione persistente per i tuoi database inseriti nei contenitori e altre applicazioni con stato oppure per condividere dati tra i pod in più zone. Puoi installare Portworx con un grafico Helm ed eseguire il provisioning dell'archiviazione per le tue applicazioni utilizzando i volumi persistenti Kubernetes. Per ulteriori informazioni su come configurare Portworx nel tuo cluster, vedi [Archiviazione di dati su SDS (software-defined storage) con Portworx](/docs/containers?topic=containers-portworx#portworx).</td>
  </tr>
</tbody>
</table>

<br />


## Servizi di database
{: #database_services}

<table summary="La tabella mostra i servizi disponibili che puoi aggiungere al tuo cluster per aggiungere funzionalità di database. Le righe devono essere lette da sinistra a destra, con il nome del servizio nella colonna uno e una descrizione del servizio nella colonna due.">
<caption>Servizi di database</caption>
<thead>
<tr>
<th>Servizio</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
  <tr>
    <td>{{site.data.keyword.blockchainfull_notm}} Platform 2.0 beta</td>
    <td>Distribuisci e gestisci il tuo proprio {{site.data.keyword.blockchainfull_notm}} Platform su {{site.data.keyword.containerlong_notm}}. Con {{site.data.keyword.blockchainfull_notm}} Platform 2.0, puoi ospitare le reti {{site.data.keyword.blockchainfull_notm}} o creare organizzazioni che possono unirsi ad altre reti {{site.data.keyword.blockchainfull_notm}} 2.0. Per ulteriori informazioni su come configurare {{site.data.keyword.blockchainfull_notm}} in {{site.data.keyword.containerlong_notm}}, vedi [Informazioni su {{site.data.keyword.blockchainfull_notm}} Platform free 2.0 beta](/docs/services/blockchain?topic=blockchain-ibp-console-overview#ibp-console-overview).</td>
  </tr>
<tr>
  <td>Database cloud</td>
  <td>Puoi scegliere tra una varietà di servizi di database {{site.data.keyword.Bluemix_notm}}, come {{site.data.keyword.composeForMongoDB_full}} o {{site.data.keyword.cloudantfull}}, per distribuire soluzioni di database altamente disponibili e scalabili nel tuo cluster. Per un elenco di database cloud disponibili, consulta il [catalogo {{site.data.keyword.Bluemix_notm}} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cloud.ibm.com/catalog?category=databases).  </td>
  </tr>
  </tbody>
  </table>


## Aggiunta dei servizi {{site.data.keyword.Bluemix_notm}}
ai cluster
{: #adding_cluster}

Aggiungi i servizi {{site.data.keyword.Bluemix_notm}} per migliorare il tuo cluster Kubernetes con funzionalità supplementari in aree quali l'intelligenza artificiale Watson, i dati, la sicurezza e IoT (Internet of Things).
{:shortdesc}

Puoi eseguire il bind solo di servizi che supportano le chiavi del servizio. Per trovare un elenco con i servizi che supportano le chiavi del servizio, vedi [Abilitazione di applicazioni esterne a utilizzare servizi {{site.data.keyword.Bluemix_notm}}](/docs/resources?topic=resources-externalapp#externalapp).
{: note}

Prima di iniziare:
- Assicurati di avere i seguenti ruoli:
    - [Ruolo del servizio {{site.data.keyword.Bluemix_notm}} IAM **Editor** o **Amministratore**](/docs/containers?topic=containers-users#platform) per il cluster.
    - [Ruolo del servizio {{site.data.keyword.Bluemix_notm}} IAM **Scrittore** o **Gestore**](/docs/containers?topic=containers-users#platform) per lo spazio dei nomi in cui vuoi associare il servizio
    - [Ruolo Cloud Foundry **Sviluppatore**](/docs/iam?topic=iam-mngcf#mngcf) per lo spazio che vuoi utilizzare
- [Accedi al tuo account. Specifica la regione appropriata e, se applicabile, il gruppo di risorse. Imposta il contesto per il tuo cluster](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).

Per aggiungere un servizio {{site.data.keyword.Bluemix_notm}} al tuo cluster:

1. [Crea un'istanza del servizio {{site.data.keyword.Bluemix_notm}}](/docs/resources?topic=resources-externalapp#externalapp).
    * Alcuni servizi {{site.data.keyword.Bluemix_notm}} sono disponibili solo in determinate regioni. Puoi eseguire il bind di un servizio al tuo cluster solo se il servizio è disponibile nella stessa regione del tuo cluster. Inoltre, se vuoi creare un'istanza del servizio nella zona Washington DC, devi utilizzare la CLI.
    * Devi creare l'istanza del servizio nello stesso gruppo di risorse del tuo cluster. Una risorsa può essere creata in un solo gruppo di risorse che non puoi modificare in seguito.

2. Controlla il tipo di servizio che hai creato e prendi nota del nome (**Name**) dell'istanza del servizio.
   - **Servizi Cloud Foundry:**
     ```
     ibmcloud service list
     ```
     {: pre}

     Output di esempio:
     ```
     name                         service           plan    bound apps   last operation
     <cf_service_instance_name>   <service_name>    spark                create succeeded
     ```
     {: screen}

  - **Servizi abilitati a {{site.data.keyword.Bluemix_notm}} IAM:**
     ```
     ibmcloud resource service-instances
     ```
     {: pre}

     Output di esempio:
     ```
     Name                          Location   State    Type               Tags
     <iam_service_instance_name>   <region>   active   service_instance
     ```
     {: screen}

   Puoi anche vedere i diversi tipi di servizio nel tuo dashboard come **Servizi Cloud Foundry** e **Servizi**.

3. Identifica lo spazio dei nomi del cluster che desideri utilizzare per l'aggiunta del tuo servizio. Scegli tra le seguenti opzioni.
     ```
     kubectl get namespaces
     ```
     {: pre}

4.  Aggiungi il servizio al tuo cluster utilizzando il [comando](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_service_bind) `ibmcloud ks cluster-service-bind`. Per i servizi abilitati a {{site.data.keyword.Bluemix_notm}} IAM, assicurati di utilizzare l'alias Cloud Foundry che hai creato in precedenza. Per i servizi abilitati a IAM, puoi anche utilizzare il ruolo predefinito di accesso al servizio **Scrittore** o specificare il ruolo di accesso al servizio con l'indicatore `--role`. Il comando crea una chiave di servizio per l'istanza del servizio oppure puoi includere l'indicatore `--key` per utilizzare le credenziali della chiave di servizio esistente. Se utilizzi l'indicatore `--key`, non includere l'indicatore `--role`.
    ```
    ibmcloud ks cluster-service-bind --cluster <cluster_name_or_ID> --namespace <namespace> --service <service_instance_name> [--key <service_instance_key>] [--role <IAM_service_role>]
    ```
    {: pre}

    Dopo che
il servizio è stato correttamente associato al tuo cluster, viene creato un segreto cluster che contiene le credenziali
della tua istanza del servizio. I segreti vengono crittografati automaticamente in etcd per proteggere i tuoi dati.

    Output di esempio:
    ```
    ibmcloud ks cluster-service-bind --cluster mycluster --namespace mynamespace --service cleardb
    Binding service instance to namespace...
    OK
    Namespace:	     mynamespace
    Secret name:     binding-<service_instance_name>
    ```
    {: screen}

5.  Verifica le credenziali del servizio nel tuo segreto Kubernetes.
    1. Ottieni i dettagli del segreto e prendi nota del valore **binding**. Il valore **binding** ha una codifica base64 e contiene le credenziali per la tua istanza del servizio in formato JSON.
       ```
       kubectl get secrets binding-<service_instance_name> --namespace=<namespace> -o yaml
       ```
       {: pre}

       Output di esempio:
       ```
       apiVersion: v1
       data:
         binding: <binding>
       kind: Secret
       metadata:
         annotations:
           service-instance-id: 1111aaaa-a1aa-1aa1-1a11-111aa111aa11
           service-key-id: 2b22bb2b-222b-2bb2-2b22-b22222bb2222
         creationTimestamp: 2018-08-07T20:47:14Z
         name: binding-<service_instance_name>
         namespace: <namespace>
         resourceVersion: "6145900"
         selfLink: /api/v1/namespaces/default/secrets/binding-mycloudant
         uid: 33333c33-3c33-33c3-cc33-cc33333333c
       type: Opaque
       ```
       {: screen}

    2. Decodifica il valore di bind.
       ```
       echo "<binding>" | base64 -D
       ```
       {: pre}

       Output di esempio:
       ```
       {"apikey":"KL34Ys893284NGJEPFjgrioJ12NElpow","host":"98765aab-9ce1-7tr3-ba87-bfbab6e6d9d6-bluemix.cloudant.com","iam_apikey_description":"Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:cloudantnosqldb:us-south:a/1234g56789cfe8e6388dd2ec098:98746cw-43d7-49ce-947a-d8fe3eebb381::","iam_apikey_name":"auto-generated-apikey-1234abcde-987f-3t64-9d96-d13775ec5663","iam_role_crn":"crn:v1:bluemix:public:iam::::serviceRole:Writer","iam_serviceid_crn":"crn:v1:bluemix:public:iam-identity::a/1234567890brasge5htn2ec098::serviceid:ServiceId-12345vgh-6c4c-ytr12-af6b-467d30d6ef44","password":"jfiavhui12484fnivhuo472nvei23913c3ff","port":443,"url":"https://25c73aac-9ce1-4c24-ba98-bfbab6e6d9d6-bluemix:ugvioev823inreuiegn43donvri29989wiu9t22@25c73aac-9ce1-4c24-ba98-abdrjio123562lnsb-bluemix.cloudant.com","username":"123b45da-9ce1-4c24-ab12-rinwnwub1294-bluemix"}
       ```
       {: screen}

    3. Facoltativo: confronta le credenziali del servizio che hai decodificato nel passo precedente con le credenziali del servizio che trovi per la tua istanza del servizio nel dashboard {{site.data.keyword.Bluemix_notm}}.

6. Ora che è stato eseguito il bind del tuo servizio al tuo cluster, devi configurare la tua applicazione per [accedere alle credenziali del servizio nel segreto Kubernetes](#adding_app).


## Accesso alle credenziali del servizio dalle tue applicazioni
{: #adding_app}

Per accedere ad un'istanza del servizio {{site.data.keyword.Bluemix_notm}} dalla tua applicazione, devi rendere le credenziali del servizio archiviate nel segreto di Kubernetes disponibili per la tua applicazione.
{: shortdesc}

Le credenziali di un'istanza del servizio hanno una codifica base64 e sono archiviate all'interno del tuo segreto in formato JSON. Per accedere ai dati nel tuo segreto, scegli tra le seguenti opzioni:
- [Monta il segreto come un volume al tuo pod](#mount_secret)
- [Fai riferimento al segreto nelle variabili di ambiente](#reference_secret)
<br>
Vuoi rendere i tuoi segreti ancora più sicuri? Chiedi al tuo amministratore cluster di [abilitare {{site.data.keyword.keymanagementservicefull}}](/docs/containers?topic=containers-encryption#keyprotect) nel tuo cluster per crittografare i segreti nuovi ed esistenti, come ad esempio il segreto che memorizza le credenziali delle tue istanze del servizio {{site.data.keyword.Bluemix_notm}}.
{: tip}

Prima di iniziare:
-  Assicurati di disporre del [ruolo del servizio {{site.data.keyword.Bluemix_notm}} IAM **Scrittore** o **Gestore**](/docs/containers?topic=containers-users#platform) per lo spazio dei nomi `kube-system`.
- [Accedi al tuo account. Specifica la regione appropriata e, se applicabile, il gruppo di risorse. Imposta il contesto per il tuo cluster](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).
- [Aggiungi un servizio {{site.data.keyword.Bluemix_notm}} al tuo cluster](#adding_cluster).

### Montaggio del segreto come un volume al tuo pod
{: #mount_secret}

Quando monti un segreto come un volume al tuo pod, un file denominato `binding` viene archiviato nella directory di montaggio del volume. Il file `binding` in formato JSON include tutte le informazioni e le credenziali di cui hai bisogno per accedere al servizio {{site.data.keyword.Bluemix_notm}}.
{: shortdesc}

1.  Elenca i segreti disponibili nel tuo cluster e prendi nota del **nome** del tuo segreto. Cerca un segreto di tipo **Opaque**. Se sono presenti più segreti, contatta il tuo amministratore cluster per identificare il segreto del servizio
corretto.

    ```
    kubectl get secrets
    ```
    {: pre}

    Output di esempio:

    ```
    NAME                                    TYPE                                  DATA      AGE
    binding-<service_instance_name>         Opaque                                1         3m

    ```
    {: screen}

2.  Crea un file YAML per la tua distribuzione Kubernetes e monta il segreto come un volume al tuo pod.
    ```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      labels:
        app: secret-test
      name: secret-test
      namespace: <my_namespace>
    spec:
      selector:
        matchLabels:
          app: secret-test
      replicas: 1
      template:
        metadata:
          labels:
            app: secret-test
        spec:
          containers:
          - image: registry.bluemix.net/ibmliberty:latest
            name: secret-test
            volumeMounts:
            - mountPath: <mount_path>
              name: <volume_name>
          volumes:
          - name: <volume_name>
            secret:
              defaultMode: 420
              secretName: binding-<service_instance_name>
    ```
    {: codeblock}

    <table>
    <caption>Descrizione dei componenti del file YAML</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dei componenti del file YAML</th>
    </thead>
    <tbody>
    <tr>
    <td><code>volumeMounts.mountPath</code></td>
    <td>Il percorso assoluto della directory in cui viene montato il volume nel contenitore.</td>
    </tr>
    <tr>
    <td><code>volumeMounts.name</code></br><code>volumes.name</code></td>
    <td>Il nome del volume per montare il tuo pod.</td>
    </tr>
    <tr>
    <td><code>secret.defaultMode</code></td>
    <td>Le autorizzazioni di lettura e scrittura sul segreto. Utilizza `420` per impostare le autorizzazioni di sola lettura. </td>
    </tr>
    <tr>
    <td><code>secret.secretName</code></td>
    <td>Il nome del segreto di cui hai preso nota nel passo precedente.</td>
    </tr></tbody></table>

3.  Crea il pod e monta il segreto come un volume.
    ```
    kubectl apply -f secret-test.yaml
    ```
    {: pre}

4.  Verifica che il pod sia stato creato.
    ```
    kubectl get pods
    ```
    {: pre}

    Output CLI di esempio:

    ```
    NAME                           READY     STATUS    RESTARTS   AGE
    secret-test-1111454598-gfx32   1/1       Running   0          1m
    ```
    {: screen}

5.  Accedi alle credenziali del servizio.
    1. Accedi al tuo pod.
       ```
       kubectl exec <pod_name> -it bash
       ```
       {: pre}

    2. Vai al tuo percorso di montaggio del volume che hai definito in precedenza ed elenca i file in esso contenuti.
       ```
       cd <volume_mountpath> && ls
       ```
       {: pre}

       Output di esempio:
       ```
       binding
       ```
       {: screen}

       Il file `binding` include le credenziali del servizio che hai archiviato nel segreto Kubernetes.

    4. Visualizza le credenziali del servizio. Le credenziali vengono archiviate come coppie chiave-valore in formato JSON.
       ```
       cat binding
       ```
       {: pre}

       Output di esempio:
       ```
       {"apikey":"KL34Ys893284NGJEPFjgrioJ12NElpow","host":"98765aab-9ce1-7tr3-ba87-bfbab6e6d9d6-bluemix.cloudant.com","iam_apikey_description":"Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:cloudantnosqldb:us-south:a/1234g56789cfe8e6388dd2ec098:98746cw-43d7-49ce-947a-d8fe3eebb381::","iam_apikey_name":"auto-generated-apikey-1234abcde-987f-3t64-9d96-d13775ec5663","iam_role_crn":"crn:v1:bluemix:public:iam::::serviceRole:Writer","iam_serviceid_crn":"crn:v1:bluemix:public:iam-identity::a/1234567890brasge5htn2ec098::serviceid:ServiceId-12345vgh-6c4c-ytr12-af6b-467d30d6ef44","password":"jfiavhui12484fnivhuo472nvei23913c3ff","port":443,"url":"https://25c73aac-9ce1-4c24-ba98-bfbab6e6d9d6-bluemix:ugvioev823inreuiegn43donvri29989wiu9t22@25c73aac-9ce1-4c24-ba98-abdrjio123562lnsb-bluemix.cloudant.com","username":"123b45da-9ce1-4c24-ab12-rinwnwub1294-bluemix"}
       ```
       {: screen}

    5. Configura la tua applicazione per analizzare il contenuto JSON e richiamare le informazioni di cui hai bisogno per accedere al tuo servizio.


### Riferimento al segreto nelle variabili di ambiente
{: #reference_secret}

Puoi aggiungere le credenziali del servizio e altre coppie chiave-valore dal tuo segreto Kubernetes come variabili di ambiente alla tua distribuzione.
{: shortdesc}

1. Elenca i segreti disponibili nel tuo cluster e prendi nota del **nome** del tuo segreto. Cerca un segreto di tipo **Opaque**. Se sono presenti più segreti, contatta il tuo amministratore cluster per identificare il segreto del servizio
corretto.

    ```
    kubectl get secrets
    ```
    {: pre}

    Output di esempio:

    ```
    NAME                                    TYPE                                  DATA      AGE
    binding-<service_instance_name>         Opaque                                1         3m
    ```
    {: screen}

2. Ottieni i dettagli del tuo segreto per trovare le potenziali coppie chiave-valore a cui puoi fare riferimento come variabili di ambiente nel tuo pod. Le credenziali del servizio sono archiviate nella chiave `binding` del tuo segreto.
   ```
   kubectl get secrets binding-<service_instance_name> --namespace=<namespace> -o yaml
   ```
   {: pre}

   Output di esempio:
   ```
   apiVersion: v1
   data:
     binding: <binding>
   kind: Secret
   metadata:
     annotations:
       service-instance-id: 7123acde-c3ef-4ba2-8c52-439ac007fa70
       service-key-id: 9h30dh8a-023f-4cf4-9d96-d12345ec7890
     creationTimestamp: 2018-08-07T20:47:14Z
     name: binding-<service_instance_name>
     namespace: <namespace>
     resourceVersion: "6145900"
     selfLink: /api/v1/namespaces/default/secrets/binding-mycloudant
     uid: 12345a31-9a83-11e8-ba83-cd49014748f
   type: Opaque
   ```
   {: screen}

3. Crea un file YAML per la tua distribuzione Kubernetes e specifica una variabile di ambiente che fa riferimento alla chiave `binding`.
   ```
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     labels:
       app: secret-test
     name: secret-test
     namespace: <my_namespace>
   spec:
     selector:
       matchLabels:
         app: secret-test
     template:
       metadata:
         labels:
           app: secret-test
       spec:
         containers:
         - image: registry.bluemix.net/ibmliberty:latest
           name: secret-test
           env:
           - name: BINDING
             valueFrom:
               secretKeyRef:
                 name: binding-<service_instance_name>
                 key: binding
     ```
     {: codeblock}

     <table>
     <caption>Descrizione dei componenti del file YAML</caption>
     <thead>
     <th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dei componenti del file YAML</th>
     </thead>
     <tbody>
     <tr>
     <td><code>containers.env.name</code></td>
     <td>Il nome della tua variabile di ambiente.</td>
     </tr>
     <tr>
     <td><code>env.valueFrom.secretKeyRef.name</code></td>
     <td>Il nome del segreto di cui hai preso nota nel passo precedente.</td>
     </tr>
     <tr>
     <td><code>env.valueFrom.secretKeyRef.key</code></td>
     <td>La chiave che fa parte del tuo segreto e a cui vuoi fare riferimento nella tua variabile di ambiente. Per fare riferimento alle credenziali del servizio, devi utilizzare la chiave <strong>binding</strong> .  </td>
     </tr>
     </tbody></table>

4. Crea il pod che fa riferimento alla chiave `binding` del tuo segreto come una variabile di ambiente.
   ```
   kubectl apply -f secret-test.yaml
   ```
   {: pre}

5. Verifica che il pod sia stato creato.
   ```
   kubectl get pods
   ```
   {: pre}

   Output CLI di esempio:
   ```
   NAME                           READY     STATUS    RESTARTS   AGE
    secret-test-1111454598-gfx32   1/1       Running   0          1m
   ```
   {: screen}

6. Verifica che la variabile di ambiente sia impostata correttamente.
   1. Accedi al tuo pod.
      ```
      kubectl exec <pod_name> -it bash
      ```
      {: pre}

   2. Elenca tutte le variabili di ambiente nel pod.
      ```
      env
      ```
      {: pre}

      Output di esempio:
      ```
      BINDING={"apikey":"KL34Ys893284NGJEPFjgrioJ12NElpow","host":"98765aab-9ce1-7tr3-ba87-bfbab6e6d9d6-bluemix.cloudant.com","iam_apikey_description":"Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:cloudantnosqldb:us-south:a/1234g56789cfe8e6388dd2ec098:98746cw-43d7-49ce-947a-d8fe3eebb381::","iam_apikey_name":"auto-generated-apikey-1234abcde-987f-3t64-9d96-d13775ec5663","iam_role_crn":"crn:v1:bluemix:public:iam::::serviceRole:Writer","iam_serviceid_crn":"crn:v1:bluemix:public:iam-identity::a/1234567890brasge5htn2ec098::serviceid:ServiceId-12345vgh-6c4c-ytr12-af6b-467d30d6ef44","password":"jfiavhui12484fnivhuo472nvei23913c3ff","port":443,"url":"https://25c73aac-9ce1-4c24-ba98-bfbab6e6d9d6-bluemix:ugvioev823inreuiegn43donvri29989wiu9t22@25c73aac-9ce1-4c24-ba98-abdrjio123562lnsb-bluemix.cloudant.com","username":"123b45da-9ce1-4c24-ab12-rinwnwub1294-bluemix"}
      ```
      {: screen}

7. Configura la tua applicazione per leggere la variabile di ambiente e per analizzare il contenuto JSON per richiamare le informazioni di cui hai bisogno per accedere al tuo servizio.

   Codice di esempio in Python:
   ```
   if os.environ.get('BINDING'):
        credentials = json.loads(os.environ.get('BINDING'))
   ```
   {: codeblock}

## Configurazione di Helm in {{site.data.keyword.containerlong_notm}}
{: #helm}

[Helm ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://helm.sh) è un gestore pacchetti di Kubernetes. Puoi creare grafici Helm o utilizzare grafici Helm preesistenti per definire, installare e aggiornare applicazioni Kubernetes complesse eseguite nei cluster {{site.data.keyword.containerlong_notm}}.
{:shortdesc}

Per distribuire i grafici Helm, devi installare la CLI Helm sulla tua macchina locale e installare il server Helm Tiller nel tuo cluster. L'immagine per Tiller è memorizzata nel Google Container Registry pubblico. Per accedere all'immagine durante l'installazione di Tiller, il tuo cluster deve consentire la connettività di rete pubblica al Google Container Registry pubblico. I cluster che hanno abilitato l'endpoint del servizio pubblico possono accedere automaticamente all'immagine. I cluster privati protetti con un firewall personalizzato o i cluster che hanno abilitato solo l'endpoint del servizio privato, non consentono l'accesso all'immagine Tiller. Puoi invece [estrarre l'immagine nella tua macchina locale e inviarla al tuo spazio dei nomi in {{site.data.keyword.registryshort_notm}}](#private_local_tiller) oppure [installare i grafici Helm senza utilizzare Tiller](#private_install_without_tiller).
{: note}

### Configurazione di Helm in un cluster con accesso pubblico
{: #public_helm_install}

Se il tuo cluster ha abilitato l'endpoint del servizio pubblico, puoi installare Tiller utilizzando l'immagine pubblica memorizzata in Google Container Registry.
{: shortdesc}

Prima di iniziare: [accedi al tuo account. Specifica la regione appropriata e, se applicabile, il gruppo di risorse. Imposta il contesto per il tuo cluster](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).

1. Installa la <a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">CLI Helm <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>.

2. **Importante**: per mantenere la sicurezza del cluster, crea un account di servizio per Tiller nello spazio dei nomi `kube-system` e un bind del ruolo cluster RBAC Kubernetes per il pod `tiller-deploy` applicando il seguente file `.yaml` dal [repository {{site.data.keyword.Bluemix_notm}} `kube-samples`](https://github.com/IBM-Cloud/kube-samples/blob/master/rbac/serviceaccount-tiller.yaml). **Nota**: per installare Tiller con l'account di servizio e il bind del ruolo cluster nello spazio dei nomi `kube-system`, devi avere il [ruolo `cluster-admin`](/docs/containers?topic=containers-users#access_policies).
    ```
    kubectl apply -f https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/rbac/serviceaccount-tiller.yaml
    ```
    {: pre}

3. Inizializza Helm e installa Tiller con l'account di servizio che hai creato.

    ```
    helm init --service-account tiller
    ```
    {: pre}

4.  Verifica che l'installazione sia riuscita correttamente.
    1.  Verifica che l'account di servizio Tiller sia stato creato.
        ```
        kubectl get serviceaccount -n kube-system tiller
        ```
        {: pre}

        Output di esempio:

        ```
        NAME                                 SECRETS   AGE
        tiller                               1         2m
        ```
        {: screen}

    2.  Verifica che il pod `tiller-deploy` abbia uno **Status** di `Running` nel tuo cluster.
        ```
        kubectl get pods -n kube-system -l app=helm
        ```
        {: pre}

        Output di esempio:

        ```
        NAME                            READY     STATUS    RESTARTS   AGE
    tiller-deploy-352283156-nzbcm   1/1       Running   0          2m
        ```
        {: screen}

5. Aggiungi i repository Helm {{site.data.keyword.Bluemix_notm}} alla tua istanza Helm.
   ```
   helm repo add ibm https://registry.bluemix.net/helm/ibm
   ```
   {: pre}

   ```
   helm repo add ibm-charts https://registry.bluemix.net/helm/ibm-charts
   ```
   {: pre}

6. Aggiorna i repository per richiamare le versioni più recenti di tutti i grafici Helm.
   ```
   helm repo update
   ```
   {: pre}

7. Elenca i grafici Helm attualmente disponibili nei repository {{site.data.keyword.Bluemix_notm}}.
   ```
   helm search ibm
   ```
   {: pre}

   ```
   helm search ibm-charts
   ```
   {: pre}

8. Identifica il grafico Helm che vuoi installare e segui le istruzioni nel file `README` del grafico Helm per installarlo nel tuo cluster.


### Cluster privati: esecuzione del push dell'immagine Tiller per il tuo registro privato in {{site.data.keyword.registryshort_notm}}
{: #private_local_tiller}

Puoi estrarre l'immagine Tiller nella tua macchina locale, inviarla al tuo spazio dei nomi in {{site.data.keyword.registryshort_notm}} e lasciare che Helm installi Tiller utilizzando l'immagine in {{site.data.keyword.registryshort_notm}}.
{: shortdesc}

Prima di iniziare:
- Installa Docker sulla tua macchina locale. Se hai installato la [CLI {{site.data.keyword.Bluemix_notm}}](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli), Docker è già installato.
- [Installa il plugin della CLI {{site.data.keyword.registryshort_notm}} e configura uno spazio dei nomi](/docs/services/Registry?topic=registry-getting-started#gs_registry_cli_install).

Per installare Tiller utilizzando {{site.data.keyword.registryshort_notm}}:

1. Installa la <a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">CLI Helm <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a> sulla tua macchina locale.
2. Connettiti al tuo cluster privato utilizzando il tunnel VPN dell'infrastruttura {{site.data.keyword.Bluemix_notm}} che hai configurato.
3. **Importante**: per mantenere la sicurezza del cluster, crea un account di servizio per Tiller nello spazio dei nomi `kube-system` e un bind del ruolo cluster RBAC Kubernetes per il pod `tiller-deploy` applicando il seguente file `.yaml` dal [repository {{site.data.keyword.Bluemix_notm}} `kube-samples`](https://github.com/IBM-Cloud/kube-samples/blob/master/rbac/serviceaccount-tiller.yaml). **Nota**: per installare Tiller con l'account di servizio e il bind del ruolo cluster nello spazio dei nomi `kube-system`, devi avere il [ruolo `cluster-admin`](/docs/containers?topic=containers-users#access_policies).
    1. [Ottieni i file YALM per l'account di servizio e il bind del ruolo cluster di Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/rbac/serviceaccount-tiller.yaml).

    2. Crea le risorse Kubernetes nel tuo cluster.
       ```
       kubectl apply -f service-account.yaml
       ```
       {: pre}

       ```
       kubectl apply -f cluster-role-binding.yaml
       ```
       {: pre}

4. [Trova la versione di Tiller ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://console.cloud.google.com/gcr/images/kubernetes-helm/GLOBAL/tiller?gcrImageListsize=30)] che vuoi installare nel tuo cluster. Se non hai bisogno di una versione specifica, usa quella più recente.

5. Estrai l'immagine Tiller nella tua macchina locale.
   ```
   docker pull gcr.io/kubernetes-helm/tiller:v2.11.0
   ```
   {: pre}

   Output di esempio:
   ```
   docker pull gcr.io/kubernetes-helm/tiller:v2.13.0
   v2.13.0: Pulling from kubernetes-helm/tiller
   48ecbb6b270e: Pull complete
   d3fa0712c71b: Pull complete
   bf13a43b92e9: Pull complete
   b3f98be98675: Pull complete
   Digest: sha256:c4bf03bb67b3ae07e38e834f29dc7fd43f472f67cad3c078279ff1bbbb463aa6
   Status: Downloaded newer image for gcr.io/kubernetes-helm/tiller:v2.13.0
   ```
   {: screen}

6. [Invia l'immagine Tiller al tuo spazio dei nomi in {{site.data.keyword.registryshort_notm}}](/docs/services/Registry?topic=registry-getting-started#gs_registry_images_pushing).

7. [Copia il segreto di pull dell'immagine per accedere a {{site.data.keyword.registryshort_notm}} dallo spazio dei nomi predefinito allo spazio dei nomi `kube-system`](/docs/containers?topic=containers-images#copy_imagePullSecret).

8. Installa Tiller nel tuo cluster privato utilizzando l'immagine che hai memorizzato nel tuo spazio dei nomi in {{site.data.keyword.registryshort_notm}}.
   ```
   helm init --tiller-image <region>.icr.io/<mynamespace>/<myimage>:<tag> --service-account tiller
   ```
   {: pre}

9. Aggiungi i repository Helm {{site.data.keyword.Bluemix_notm}} alla tua istanza Helm.
   ```
   helm repo add ibm https://registry.bluemix.net/helm/ibm
   ```
   {: pre}

   ```
   helm repo add ibm-charts https://registry.bluemix.net/helm/ibm-charts
   ```
   {: pre}

10. Aggiorna i repository per richiamare le versioni più recenti di tutti i grafici Helm.
    ```
    helm repo update
    ```
    {: pre}

11. Elenca i grafici Helm attualmente disponibili nei repository {{site.data.keyword.Bluemix_notm}}.
    ```
    helm search ibm
    ```
    {: pre}

    ```
    helm search ibm-charts
    ```
    {: pre}

12. Identifica il grafico Helm che vuoi installare e segui le istruzioni nel file `README` del grafico Helm per installarlo nel tuo cluster.

### Cluster privati: installazione dei grafici Helm senza utilizzare Tiller
{: #private_install_without_tiller}

Se non vuoi installare Tiller nel tuo cluster privato, puoi creare manualmente i file YAML del grafico Helm e applicarli utilizzando i comandi `kubectl`.
{: shortdesc}

I passi in questo esempio mostrano come installare i grafici Helm dai repository di grafici Helm {{site.data.keyword.Bluemix_notm}} nel tuo cluster privato. Se vuoi installare un grafico Helm che non è memorizzato in uno dei repository di grafici Helm {{site.data.keyword.Bluemix_notm}}, devi seguire le istruzioni presentate in questo argomento per creare i file YAML per il tuo grafico. Inoltre, devi scaricare l'immagine del grafico Helm dal registro del contenitore pubblico, estrarla nel tuo spazio dei nomi in {{site.data.keyword.registryshort_notm}} e aggiornare il file `values.yaml` per utilizzare l'immagine in {{site.data.keyword.registryshort_notm}}.
{: note}

1. Installa la <a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">CLI Helm <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a> sulla tua macchina locale.
2. Connettiti al tuo cluster privato utilizzando il tunnel VPN dell'infrastruttura {{site.data.keyword.Bluemix_notm}} che hai configurato.
3. Aggiungi i repository Helm {{site.data.keyword.Bluemix_notm}} alla tua istanza Helm.
   ```
   helm repo add ibm https://registry.bluemix.net/helm/ibm
   ```
   {: pre}

   ```
   helm repo add ibm-charts https://registry.bluemix.net/helm/ibm-charts
   ```
   {: pre}

4. Aggiorna i repository per richiamare le versioni più recenti di tutti i grafici Helm.
   ```
   helm repo update
   ```
   {: pre}

5. Elenca i grafici Helm attualmente disponibili nei repository {{site.data.keyword.Bluemix_notm}}.
   ```
   helm search ibm
   ```
   {: pre}

   ```
   helm search ibm-charts
   ```
   {: pre}

6. Identifica il grafico Helm che vuoi installare, scarica il grafico Helm nella tua macchina locale e decomprimi i file del tuo grafico Helm. Il seguente esempio mostra come scaricare il grafico Helm per il cluster autoscaler versione 1.0.3 e decomprimere i file in una directory `cluster-autoscaler`.
   ```
   helm fetch ibm/ibm-iks-cluster-autoscaler --untar --untardir ./cluster-autoscaler --version 1.0.3
   ```
   {: pre}

7. Passa alla directory in cui hai decompresso i file del grafico Helm.
   ```
   cd cluster-autoscaler
   ```
   {: pre}

8. Crea una directory `output` per i file YAML che generi utilizzando i file nel tuo grafico Helm.
   ```
   mkdir output
   ```
   {: pre}

9. Apri il file `values.yaml` e apporta tutte le modifiche richieste dalle istruzioni di installazione del grafico Helm.
   ```
   nano ibm-iks-cluster-autoscaler/values.yaml
   ```
   {: pre}

10. Utilizza la tua installazione Helm locale per creare tutti i file YALM di Kubernetes per il tuo grafico Helm. I file YAML vengono memorizzati nella directory `output` che hai creato in precedenza.
    ```
    helm template --values ./ibm-iks-cluster-autoscaler/values.yaml --output-dir ./output ./ibm-iks-cluster-autoscaler
    ```
    {: pre}

    Output di esempio:
    ```
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-configmap.yaml
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-service-account-roles.yaml
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-service.yaml
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-deployment.yaml
    ```
    {: screen}

11. Distribuisci tutti i file YAML nel tuo cluster privato.
    ```
    kubectl apply --recursive --filename ./output
    ```
   {: pre}

12. Facoltativo: rimuovi tutti i file YAML dalla directory `output`.
    ```
    kubectl delete --recursive --filename ./output
    ```
    {: pre}


### Link Helm correlati
{: #helm_links}

* Per utilizzare il grafico Helm strongSwan, vedi [Configurazione della connettività VPN con il grafico Helm del servizio VPN IPSec strongSwan](/docs/containers?topic=containers-vpn#vpn-setup).
* Visualizza i grafici Helm disponibili che puoi utilizzare con {{site.data.keyword.Bluemix_notm}} nel [Catalogo dei grafici Helm ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cloud.ibm.com/kubernetes/solutions/helm-charts) della console.
* Ulteriori informazioni sui comandi Helm utilizzati per configurare e gestire i grafici Helm sono disponibili nella <a href="https://docs.helm.sh/helm/" target="_blank">documentazione Helm <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>.
* Ulteriori informazioni su come puoi [aumentare la velocità di distribuzione con i grafici Helm Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://developer.ibm.com/recipes/tutorials/increase-deployment-velocity-with-kubernetes-helm-charts/).

## Visualizzazione delle risorse del cluster Kubernetes
{: #weavescope}

Weave Scope fornisce un diagramma visivo delle tue risorse all'interno di un cluster Kubernetes, inclusi servizi, pod, contenitori e altro. Weave Scope fornisce metriche interattive per CPU e memoria e strumenti per l'accodamento e l'esecuzione in un contenitore.
{:shortdesc}

Prima di iniziare:

-   Ricorda di non esporre le informazioni del tuo cluster pubblicamente su Internet. Completa questa procedura per distribuire in sicurezza Weave Scope e accedervi da un browser web
localmente.
-   Se non ne hai già uno, [crea un cluster standard](/docs/containers?topic=containers-clusters#clusters_ui). Weave Scope può essere intensivo per la CPU, specialmente l'applicazione. Esegui Weave Scope con grandi cluster a pagamento, non con cluster gratuito.
-  Assicurati di disporre del [ruolo del servizio {{site.data.keyword.Bluemix_notm}} IAM **Gestore**](/docs/containers?topic=containers-users#platform) per tutti gli spazi dei nomi.
-   [Accedi al tuo account. Specifica la regione appropriata e, se applicabile, il gruppo di risorse. Imposta il contesto per il tuo cluster](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).


Per utilizzare Weave Scope con un cluster:
2.  Distribuisci uno dei file di configurazione delle autorizzazioni RBAC fornite nel
cluster.

    Per abilitare le autorizzazioni di lettura/scrittura:

    ```
    kubectl apply -f "https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/weave-scope/weave-scope-rbac.yaml"
    ```
    {: pre}

    Per abilitare le autorizzazioni di
sola
lettura:

    ```
    kubectl apply --namespace weave -f "https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/weave-scope/weave-scope-rbac-readonly.yaml"
    ```
    {: pre}

    Output:

    ```
    clusterrole "weave-scope-mgr" created
    clusterrolebinding "weave-scope-mgr-role-binding" created
    ```
    {: screen}

3.  Distribuisci il servizio Weave Scope, che è accessibile privatamente dall'indirizzo IP del cluster.

    <pre class="pre">
    <code>kubectl apply --namespace weave -f "https://cloud.weave.works/k8s/scope.yaml?k8s-version=$(kubectl version | base64 | tr -d '&bsol;n')"</code>
    </pre>

    Output:

    ```
    serviceaccount "weave-scope" created
    deployment "weave-scope-app" created
    service "weave-scope-app" created
    daemonset "weave-scope-agent" created
    ```
    {: screen}

4.  Esegui un comando di inoltro della porta per aprire il servizio nel tuo computer. La volta successiva che accedi a Weave Scope, puoi eseguire questo comando di inoltro della porta senza dover completare nuovamente i precedenti passi di configurazione.

    ```
    kubectl port-forward -n weave "$(kubectl get -n weave pod --selector=weave-scope-component=app -o jsonpath='{.items..metadata.name}')" 4040
    ```
    {: pre}

    Output:

    ```
    Forwarding from 127.0.0.1:4040 -> 4040
    Forwarding from [::1]: :1]:4040 -> 4040
    Handling connection for 4040
    ```
    {: screen}

5.  Apri il tuo browser all'indirizzo `http://localhost:4040`. Senza i componenti predefiniti distribuiti, viene visualizzato il seguente diagramma. Puoi scegliere di visualizzare i diagrammi della topologia o le tabelle delle risorse Kubernetes nel cluster.

     <img src="images/weave_scope.png" alt="Topologia di esempio da Weave Scope" style="width:357px;" />


[Ulteriori informazioni sulle funzioni Weave Scope ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.weave.works/docs/scope/latest/features/).

<br />

