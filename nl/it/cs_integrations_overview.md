---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

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
{:preview: .preview}f


# Integrazioni di terze parti e IBM Cloud supportate
{: #supported_integrations}

Puoi utilizzare vari servizi esterni e servizi di catalogo con un cluster Kubernetes standard in {{site.data.keyword.containerlong}}.
{:shortdesc}

## Integrazioni popolari
{: #popular_services}

<table summary="La tabella mostra i servizi disponibili, popolari tra gli utenti {{site.data.keyword.containerlong_notm}}, che puoi aggiungere al tuo cluster. Le righe devono essere lette da sinistra a destra, con il nome del servizio nella prima colonna e la descrizione del servizio nella seconda colonna.">
<caption>Servizi popolari</caption>
<thead>
<tr>
<th>Servizio</th>
<th>Categoria</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.cloudaccesstrailfull}}</td>
<td>Log delle attività del cluster</td>
<td>Monitora le attività di amministrazione eseguite nel tuo cluster, analizzando i log tramite Grafana. Per ulteriori informazioni sul servizio, vedi la documentazione di [Activity Tracker](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started). Per ulteriori informazioni sui tipi di eventi che puoi tracciare, vedi [Eventi Activity Tracker](/docs/containers?topic=containers-at_events).</td>
</tr>
<tr>
<td>{{site.data.keyword.appid_full}}</td>
<td>Autenticazione</td>
<td>Aggiungi un livello di sicurezza alle tue applicazioni con [{{site.data.keyword.appid_short}}](/docs/services/appid?topic=appid-getting-started) richiedendo agli utenti di effettuare l'accesso. Per autenticare le richieste HTTP/HTTPS web o API nella tua applicazione, puoi integrare {{site.data.keyword.appid_short_notm}} con il tuo servizio Ingress utilizzando l'[annotazione Ingress per l'autenticazione {{site.data.keyword.appid_short_notm}}](/docs/containers?topic=containers-ingress_annotation#appid-auth).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix}} Block Storage</td>
<td>Archiviazione blocchi</td>
<td>[{{site.data.keyword.Bluemix_notm}} Block Storage](/docs/infrastructure/BlockStorage?topic=BlockStorage-getting-started#getting-started) è un'archiviazione iSCSI persistente e ad alte prestazioni che puoi aggiungere alle tue applicazioni utilizzando i volumi persistenti (PV, persistent volume) Kubernetes. Utilizza l'archiviazione blocchi per distribuire applicazioni con stato in una singola zona o come archiviazione ad alte prestazioni per i singoli pod. Per ulteriori informazioni su come eseguire il provisioning dell'archiviazione blocchi nel tuo cluster, vedi [Archiviazione dei dati su {{site.data.keyword.Bluemix_notm}} Block Storage](/docs/containers?topic=containers-block_storage#block_storage)</td>
</tr>
<tr>
<td>{{site.data.keyword.cloudcerts_full}}</td>
<td>Certificati TLS</td>
<td>Puoi utilizzare <a href="/docs/services/certificate-manager?topic=certificate-manager-getting-started#getting-started" target="_blank">{{site.data.keyword.cloudcerts_long}} <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a> per archiviare e gestire i certificati SSL per le tue applicazioni. Per ulteriori informazioni, consulta <a href="https://www.ibm.com/blogs/bluemix/2018/01/use-ibm-cloud-certificate-manager-ibm-cloud-container-service-deploy-custom-domain-tls-certificates/" target="_blank">Use {{site.data.keyword.cloudcerts_long_notm}} with {{site.data.keyword.containerlong_notm}} to deploy custom domain TLS Certificates <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>. </td>
</tr>
<tr>
<td>{{site.data.keyword.registrylong}}</td>
<td>Immagini contenitore</td>
<td>Imposta il tuo repository delle immagini Docker protetto in cui puoi memorizzare e condividere in modo sicuro le immagini tra gli utenti cluster. Per ulteriori informazioni, vedi la <a href="/docs/services/Registry?topic=registry-getting-started" target="_blank">{{site.data.keyword.registrylong}} documentazione <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>.</td>
</tr>
<tr>
<td>{{site.data.keyword.contdelivery_full}}</td>
<td>Automazione delle build</td>
<td>Automatizza le build delle tue applicazioni e le distribuzioni del contenitore ai cluster Kubernetes utilizzando una toolchain. Per ulteriori informazioni sulla configurazione, consulta il blog <a href="https://developer.ibm.com/recipes/tutorials/deploy-kubernetes-pods-to-the-bluemix-container-service-using-devops-pipelines/" target="_blank">Deploy Kubernetes pod to the {{site.data.keyword.containerlong_notm}} using DevOps Pipelines <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>. </td>
</tr>
<tr>
<td>{{site.data.keyword.datashield_full}} (Beta)</td>
<td>Crittografia della memoria</td>
<td>Puoi utilizzare <a href="/docs/services/data-shield?topic=data-shield-getting-started#getting-started" target="_blank">{{site.data.keyword.datashield_short}} <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a> per crittografare la tua memoria di dati. {{site.data.keyword.datashield_short}} è integrato con la tecnologia di Intel® Software Guard Extensions (SGX) e Fortanix® in modo che il codice e i dati dei carichi di lavoro del tuo contenitore {{site.data.keyword.Bluemix_notm}} siano protetti durante l'utilizzo. Il codice e i dati dell'applicazione vengono eseguiti in enclavi con protezione avanzata della CPU, che sono aree di memoria attendibili sul nodo di lavoro che proteggono aspetti critici dell'applicazione, aiutando a mantenere il codice e i dati riservati e invariati.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix}} File Storage</td>
<td>Archiviazione file</td>
<td>[{{site.data.keyword.Bluemix_notm}} File Storage](/docs/infrastructure/FileStorage?topic=FileStorage-getting-started#getting-started) è un'archiviazione file basata su NFS persistente, rapida, collegata alla rete e flessibile che puoi aggiungere alle tue applicazioni utilizzando i volumi persistenti (PV, persistent volume) Kubernetes. Puoi scegliere tra i livelli di archiviazione predefiniti con dimensioni in GB e IOPS che soddisfano i requisiti dei tuoi carichi di lavoro. Per ulteriori informazioni su come eseguire il provisioning dell'archiviazione file nel tuo cluster, vedi [Archiviazione dei dati su {{site.data.keyword.Bluemix_notm}} File Storage](/docs/containers?topic=containers-file_storage#file_storage).</td>
</tr>
<tr>
<td>{{site.data.keyword.keymanagementservicefull}}</td>
<td>Crittografia dati</td>
<td>Crittografa i segreti Kubernetes che si trovano nel tuo cluster abilitando {{site.data.keyword.keymanagementserviceshort}}. La crittografia dei tuoi segreti Kubernetes impedisce agli utenti non autorizzati di accedere a informazioni riservate sui cluster.<br>Per la configurazione, vedi <a href="/docs/containers?topic=containers-encryption#keyprotect">Crittografia dei segreti Kubernetes mediante {{site.data.keyword.keymanagementserviceshort}}</a>.<br>Per ulteriori informazioni, vedi la <a href="/docs/services/key-protect?topic=key-protect-getting-started-tutorial" target="_blank">{{site.data.keyword.keymanagementserviceshort}} documentazione <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>.</td>
</tr>
<tr>
<td>{{site.data.keyword.la_full}}</td>
<td>Log di cluster e applicazioni</td>
<td>Aggiungi funzionalità di gestione dei log al tuo cluster distribuendo LogDNA come servizio di terze parti ai tuoi nodi di lavoro per gestire i log dai contenitori di pod. Per ulteriori informazioni, vedi [Gestione dei log di cluster Kubernetes con {{site.data.keyword.loganalysisfull_notm}} with LogDNA](/docs/services/Log-Analysis-with-LogDNA/tutorials?topic=LogDNA-kube#kube).</td>
</tr>
<tr>
<td>{{site.data.keyword.mon_full}}</td>
<td>Metriche di cluster e applicazioni</td>
<td>Ottieni visibilità operativa sulle prestazioni e sull'integrità delle tue applicazioni distribuendo Sysdig come servizio di terze parti ai tuoi nodi di lavoro per inoltrare le metriche a {{site.data.keyword.monitoringlong}}. Per ulteriori informazioni, vedi [Analizza le metriche per un'applicazione distribuita in un cluster Kubernetes](/docs/services/Monitoring-with-Sysdig/tutorials?topic=Sysdig-kubernetes_cluster#kubernetes_cluster). </td>
</tr>
<tr>
<td>{{site.data.keyword.cos_full}}</td>
<td>Archiviazione oggetti</td>
<td>I dati memorizzati con {{site.data.keyword.cos_short}} vengono crittografati e diffusi tra più posizioni geografiche e vi si accede su HTTP utilizzando un'API REST. Puoi utilizzare l'[immagine ibm-backup-restore](/docs/services/RegistryImages/ibm-backup-restore?topic=RegistryImages-ibmbackup_restore_starter) per configurare il servizio per creare backup monouso o pianificati per i dati nei tuoi cluster. Per ulteriori informazioni sul servizio, vedi la <a href="/docs/services/cloud-object-storage?topic=cloud-object-storage-about" target="_blank">documentazione {{site.data.keyword.cos_short}} <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>.</td>
</tr>
<tr>
<td>Istio su {{site.data.keyword.containerlong_notm}}</td>
<td>Gestione dei microservizi</td>
<td><a href="https://www.ibm.com/cloud/info/istio" target="_blank">Istio <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a> è un servizio open source che offre agli sviluppatori un modo per connettere, proteggere, gestire e monitorare una rete di microservizi, nota anche come rete (mesh) di servizi, su piattaforme di orchestrazione cloud. Istio su {{site.data.keyword.containerlong}} fornisce un'istallazione a fase unica di Istio nel tuo cluster tramite un componente aggiuntivo gestito. Con un solo clic, puoi attivare tutti i componenti core di Istio, ulteriori funzioni di traccia, monitoraggio e visualizzazione e l'applicazione di esempio BookInfo. Per iniziare, vedi [Utilizzo del componente aggiuntivo Istio gestito (beta)](/docs/containers?topic=containers-istio).</td>
</tr>
<tr>
<td>Knative</td>
<td>Applicazioni senza server</td>
<td>[Knative ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/knative/docs) è una piattaforma open source che è stata sviluppata da IBM, Google, Pivotal, Red Hat, Cisco e altri con l'obiettivo di estendere le funzionalità di Kubernetes per aiutarti a creare applicazioni senza server e inserite in contenitori moderne e basate sul sorgente sul tuo cluster Kubernetes. La piattaforma utilizza un approccio congruente tra i linguaggi di programmazione e i framework per astrarre il carico operativo derivante dal creare, distribuire e gestire carichi di lavoro in Kubernetes in modo che gli sviluppatori possano concentrarsi su quello che conta di più per loro: il codice sorgente. Per ulteriori informazioni, vedi [Distribuzione di applicazioni senza server con Knative](/docs/containers?topic=containers-serverless-apps-knative). </td>
</tr>
<tr>
<td>Portworx</td>
<td>Archiviazione per applicazioni con stato</td>
<td>[Portworx ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://portworx.com/products/introduction/) è una soluzione SDS (software-defined storage) altamente disponibile che puoi utilizzare per gestire l'archiviazione persistente per i tuoi database inseriti nei contenitori e altre applicazioni con stato oppure per condividere dati tra i pod in più zone. Puoi installare Portworx con un grafico Helm ed eseguire il provisioning dell'archiviazione per le tue applicazioni utilizzando i volumi persistenti Kubernetes. Per ulteriori informazioni su come configurare Portworx nel tuo cluster, vedi [Archiviazione di dati su SDS (software-defined storage) con Portworx](/docs/containers?topic=containers-portworx#portworx).</td>
</tr>
<tr>
<td>Razee</td>
<td>Automazione della distribuzione</td>
<td>[Razee ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://razee.io/) è un progetto open source che automatizza e gestisce la distribuzione delle risorse Kubernetes tra cluster, ambienti e provider cloud e ti aiuta a visualizzare le informazioni sulla distribuzione per le tue risorse, consentendoti di monitorare il processo di rollout e trovare i problemi di distribuzione in modo più rapido. Per ulteriori informazioni su Razee e su come configurarlo nel tuo cluster per automatizzare il tuo processo di distribuzione, vedi la [documentazione di Razee ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/razee-io/Razee).</td>
</tr>
</tbody>
</table>

<br />


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
<td> <a href="https://helm.sh" target="_blank">Helm <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a> è un gestore del pacchetto Kubernetes. Puoi creare nuovi grafici Helm o utilizzare grafici Helm preesistenti per definire, installare e aggiornare applicazioni Kubernetes complesse eseguite nei cluster {{site.data.keyword.containerlong_notm}}. <p>Per ulteriori informazioni, vedi [Configurazione di Helm in {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-helm).</p></td>
</tr>
<tr>
<td>{{site.data.keyword.contdelivery_full}}</td>
<td>Automatizza le build delle tue applicazioni e le distribuzioni del contenitore ai cluster Kubernetes utilizzando una toolchain. Per ulteriori informazioni sulla configurazione, consulta il blog <a href="https://developer.ibm.com/recipes/tutorials/deploy-kubernetes-pods-to-the-bluemix-container-service-using-devops-pipelines/" target="_blank">Deploy Kubernetes pod to the {{site.data.keyword.containerlong_notm}} using DevOps Pipelines <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>. </td>
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
<td>[Knative ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/knative/docs) è una piattaforma open source che è stata sviluppata da IBM, Google, Pivotal, Red Hat, Cisco e altri con l'obiettivo di estendere le funzionalità di Kubernetes per aiutarti a creare applicazioni senza server e inserite in contenitori moderne e basate sul sorgente sul tuo cluster Kubernetes. La piattaforma utilizza un approccio congruente tra i linguaggi di programmazione e i framework per astrarre il carico operativo derivante dal creare, distribuire e gestire carichi di lavoro in Kubernetes in modo che gli sviluppatori possano concentrarsi su quello che conta di più per loro: il codice sorgente. Per ulteriori informazioni, vedi [Distribuzione di applicazioni senza server con Knative](/docs/containers?topic=containers-serverless-apps-knative). </td>
</tr>
<tr>
<td>Razee</td>
<td>[Razee ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://razee.io/) è un progetto open source che automatizza e gestisce la distribuzione delle risorse Kubernetes tra cluster, ambienti e provider cloud e ti aiuta a visualizzare le informazioni sulla distribuzione per le tue risorse, consentendoti di monitorare il processo di rollout e trovare i problemi di distribuzione in modo più rapido. Per ulteriori informazioni su Razee e su come configurarlo nel tuo cluster per automatizzare il tuo processo di distribuzione, vedi la [documentazione di Razee ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/razee-io/Razee).</td>
</tr>
</tbody>
</table>

<br />


## Servizi cloud ibridi
{: #hybrid_cloud_services}

<table summary="La tabella mostra i servizi disponibili che puoi utilizzare per connettere il tuo cluster ai data center in loco. Le righe devono essere lette da sinistra a destra, con il nome del servizio nella prima colonna e la descrizione del servizio nella seconda colonna.">
<caption>Servizi cloud ibridi</caption>
<thead>
<tr>
<th>Servizio</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
  <tr>
    <td>{{site.data.keyword.BluDirectLink}}</td>
    <td>[{{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-about-ibm-cloud-direct-link) consente di creare una connessione privata diretta tra i tuoi ambienti di rete remoti e {{site.data.keyword.containerlong_notm}} senza instradamento sull'Internet pubblico. Le offerte {{site.data.keyword.Bluemix_notm}} Direct Link sono utili quando devi implementare carichi di lavoro ibridi, carichi di lavoro tra provider, trasferimenti di dati di grandi dimensioni o frequenti oppure carichi di lavoro privati. Per scegliere un'offerta {{site.data.keyword.Bluemix_notm}} Direct Link e configurare una connessione {{site.data.keyword.Bluemix_notm}} Direct Link, vedi [Introduzione a {{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link#how-do-i-know-which-type-of-ibm-cloud-direct-link-i-need-) nella documentazione di {{site.data.keyword.Bluemix_notm}} Direct Link.</td>
  </tr>
<tr>
  <td>Servizio VPN IPSec strongSwan</td>
  <td>Configura un [Servizio VPN IPSec strongSwan![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.strongswan.org/about.html) che collega in modo sicuro il tuo cluster Kubernetes con una rete in loco. Il servizio VPN IPSec strongSwan fornisce un canale di comunicazione end-to-end protetto su Internet basato sulla suite di protocolli IPSec (Internet Protocol Security) standard del settore. Per configurare una connessione protetta tra il tuo cluster e una rete in loco, [configura e distribuisci il servizio VPN IPSec strongSwan](/docs/containers?topic=containers-vpn#vpn-setup) direttamente in un pod nel tuo cluster.</td>
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
<td>Monitora le attività di amministrazione eseguite nel tuo cluster, analizzando i log tramite Grafana. Per ulteriori informazioni sul servizio, vedi la documentazione di [Activity Tracker](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started). Per ulteriori informazioni sui tipi di eventi che puoi tracciare, vedi [Eventi Activity Tracker](/docs/containers?topic=containers-at_events).</td>
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
<td>Prometheus è uno strumento di monitoraggio, registrazione e avviso open source progettato per Kubernetes. Prometheus richiama informazioni dettagliate sul cluster, sui nodi di lavoro e sull'integrità della distribuzione in base alle informazioni di registrazione di Kubernetes. Le attività di rete, I/O, memoria e CPU vengono raccolte per ciascun contenitore in esecuzione in un cluster. Puoi utilizzare i dati raccolti in query o avvisi personalizzati per monitorare le prestazioni e i carichi di lavoro nel tuo cluster.

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
<td>[Weave Scope ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.weave.works/oss/scope/) fornisce un diagramma visivo delle tue risorse di un cluster Kubernetes, inclusi i servizi, i pod, i contenitori, i processi, i nodi e altro. Scope fornisce metriche interattive
per la CPU e la memoria e inoltre fornisce strumenti per inserire ed eseguire in un contenitore.</li></ol>
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
  <tr>
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
  <td>I dati memorizzati con {{site.data.keyword.cos_short}} vengono crittografati e diffusi tra più posizioni geografiche e vi si accede su HTTP utilizzando un'API REST. Puoi utilizzare l'[immagine ibm-backup-restore](/docs/services/RegistryImages/ibm-backup-restore?topic=RegistryImages-ibmbackup_restore_starter) per configurare il servizio per creare backup monouso o pianificati per i dati nei tuoi cluster. Per ulteriori informazioni sul servizio, vedi la <a href="/docs/services/cloud-object-storage?topic=cloud-object-storage-about" target="_blank">documentazione {{site.data.keyword.cos_short}} <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>.</td>
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
  <td>Puoi scegliere tra vari servizi di database {{site.data.keyword.Bluemix_notm}}, come {{site.data.keyword.composeForMongoDB_full}} o {{site.data.keyword.cloudantfull}}, per distribuire soluzioni di database altamente disponibili e scalabili nel tuo cluster. Per un elenco di database cloud disponibili, consulta il [catalogo {{site.data.keyword.Bluemix_notm}} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cloud.ibm.com/catalog?category=databases).  </td>
  </tr>
  </tbody>
  </table>
