---

copyright:
  years: 2014, 2019
lastupdated: "2019-05-31"

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
{:preview: .preview}


# Partner di IBM Cloud Kubernetes Service
{: #service-partners}

IBM si dedica a rendere {{site.data.keyword.containerlong_notm}} il miglior servizio di Kubernetes che ti aiuti a migrare, utilizzare e amministrare i tuoi carichi di lavoro Kubernetes. Per fornirti tutte le funzionalità di cui hai bisogno per eseguire i carichi di lavoro di produzione nel cloud, {{site.data.keyword.containerlong_notm}} collabora con altri provider dei servizi di terze parti per potenziare il tuo cluster con strumenti di registrazione, monitoraggio e archiviazione di alto livello.
{: shortdesc}

Esamina i nostri partner e i vantaggi di ciascuna soluzione che forniscono. Per trovare altri servizi proprietari di {{site.data.keyword.Bluemix_notm}} e open source di terze parti che puoi utilizzare nel tuo cluster, vedi [Informazioni su {{site.data.keyword.Bluemix_notm}} e integrazioni di terze parti](/docs/containers?topic=containers-ibm-3rd-party-integrations).

## LogDNA
{: #logdna-partner}

{{site.data.keyword.la_full_notm}} fornisce [LogDNA ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://logdna.com/) come servizio di terze parti che puoi utilizzare per aggiungere funzionalità di registrazione intelligente al tuo cluster e alle tue applicazioni.

### Vantaggi
{: #logdna-benefits}

Esamina la seguente tabella per trovare un elenco dei vantaggi chiave che puoi ottenere utilizzando LogDNA.
{: shortdesc}

|Vantaggi|Descrizione|
|-------------|------------------------------|
|Gestione e analisi dei log centralizzata|Quando configuri il tuo cluster come origine log, LogDNA inizia automaticamente a raccogliere le informazioni di registrazione relative ai tuoi nodi di lavoro, pod, applicazioni e rete. I tuoi log vengono automaticamente analizzati, indicizzati, contrassegnati e aggregati da LogDNA e visualizzati nel dashboard LogDNA in modo che tu possa esplorare facilmente le tue risorse del cluster. Puoi utilizzare lo strumento grafico integrato per visualizzare i codici di errore o le voci di log più comuni. |
|Facile reperibilità con sintassi di ricerca simile a Google|LogDNA utilizza la sintassi di ricerca simile a Google che supporta i termini standard, le operazioni `AND` e `OR` e ti consente di escludere o combinare i termini di ricerca per aiutarti a trovare più facilmente i tuoi log. Con l'indicizzazione intelligente dei log, puoi passare a una specifica voce di log in qualsiasi momento. |
|Crittografia di dati in transito e inattivi|LogDNA crittografa automaticamente i tuoi log per proteggerli sia durante il transito che quando sono inattivi. |
|Avvisi personalizzati e viste dei log|Puoi utilizzare il dashboard per trovare i log che corrispondono ai tuoi criteri di ricerca, salvare questi log in una vista e condividere questa vista con altri utenti per semplificare il debug tra i membri del team. Puoi anche utilizzare questa vista per creare un avviso che puoi inviare ai sistemi di downstream, come PagerDuty, Slack o email.   |
|Dashboard predefiniti e personalizzati|Puoi scegliere tra vari dashboard esistenti o creare il tuo proprio dashboard per visualizzare i log nel modo in cui ne hai bisogno. |

### Integrazione con {{site.data.keyword.containerlong_notm}}
{: #logdna-integration}

LogDNA è fornito da {{site.data.keyword.la_full_notm}}, un servizio di piattaforma {{site.data.keyword.Bluemix_notm}} che puoi utilizzare con il tuo cluster. {{site.data.keyword.la_full_notm}} è gestito da LogDNA in collaborazione con IBM.
{: shortdesc}

Per utilizzare LogDNA nel tuo cluster, devi eseguire il provisioning di un'istanza di {{site.data.keyword.la_full_notm}} nel tuo account {{site.data.keyword.Bluemix_notm}} e configurare i tuoi cluster Kubernetes come origine log. Una volta configurato il cluster, i log vengono automaticamente raccolti e inoltrati alla tua istanza del servizio {{site.data.keyword.la_full_notm}}. Puoi utilizzare il dashboard {{site.data.keyword.la_full_notm}} per accedere ai tuoi log.   

Per ulteriori informazioni, vedi [Gestione dei log di cluster Kubernetes con {{site.data.keyword.la_full_notm}}](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-kube).

### Fatturazione e supporto
{: #logdna-billing-support}

{{site.data.keyword.la_full_notm}} è completamente integrato nel sistema di supporto {{site.data.keyword.Bluemix_notm}}. Se riscontri un problema con l'utilizzo di {{site.data.keyword.la_full_notm}}, pubblica una domanda nel canale `logdna-on-iks` in [{{site.data.keyword.containerlong_notm}} Slack ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://ibm-container-service.slack.com/) o apri un [caso di supporto {{site.data.keyword.Bluemix_notm}}](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support). Accedi a Slack utilizzando il tuo ID IBM. Se non utilizzi un ID IBM per il tuo account {{site.data.keyword.Bluemix_notm}}, [richiedi un invito a questo Slack ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://bxcs-slack-invite.mybluemix.net/).

## Sysdig
{: #sydig-partner}

{{site.data.keyword.mon_full_notm}} fornisce [Sysdig Monitor ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://sysdig.com/products/monitor/) come sistema di analisi dei contenitori nativo del cloud di terze parti che puoi utilizzare per ottenere informazioni approfondite sulle prestazioni e sull'integrità di host di calcolo, applicazioni, contenitori e reti.
{: shortdesc}

### Vantaggi
{: #sydig-benefits}

Esamina la seguente tabella per trovare un elenco dei vantaggi chiave che puoi ottenere utilizzando Sysdig.
{: shortdesc}

|Vantaggi|Descrizione|
|-------------|------------------------------|
|Accesso automatico a metriche native del cloud e personalizzate Prometheus|Scegli tra le diverse metriche predefinite native del cloud e personalizzate Prometheus per ottenere informazioni approfondite sulle prestazioni e sull'integrità di host di calcolo, applicazioni, contenitori e reti.|
|Risolvi i problemi con i filtri avanzati|Sysdig Monitor crea delle topologie di rete che mostrano come sono connessi i nodi di lavoro e come i tuoi servizi Kubernetes comunicano tra loro. Puoi passare dai tuoi nodi di lavoro ai contenitori e alle singole chiamate di sistema e raggruppare e visualizzare metriche importanti per ciascuna risorsa lungo il percorso. Ad esempio, utilizza queste metriche per trovare i servizi che ricevono la maggior parte delle richieste o i servizi con query e tempi di risposta lenti. Puoi combinare questi dati con gli eventi Kubernetes, eventi CI/CD personalizzati o commit di codice.
|Rilevamento automatico delle anomalie e avvisi personalizzati|Definisci le regole e le soglie per quando vuoi ricevere una notifica sul rilevamento di anomalie nel tuo cluster o raggruppa le risorse per consentire a Sysdig di avvisarti quando una risorsa agisce in modo diverso rispetto alle altre. Puoi inviare questi avvisi agli strumenti di downstream, come ServiceNow, PagerDuty, Slack, VictorOps o email.|
|Dashboard predefiniti e personalizzati|Puoi scegliere tra vari dashboard esistenti o creare il tuo proprio dashboard per visualizzare le metriche dei tuoi microservizi nel modo in cui ne hai bisogno. |
{: caption="Vantaggi dell'utilizzo di Sysdig Monitor" caption-side="top"}

### Integrazione con {{site.data.keyword.containerlong_notm}}
{: #sysdig-integration}

Sysdig Monitor è fornito da {{site.data.keyword.mon_full_notm}}, un servizio di piattaforma {{site.data.keyword.Bluemix_notm}} che puoi utilizzare con il tuo cluster. {{site.data.keyword.mon_full_notm}} è gestito da Sysdig in collaborazione con IBM.
{: shortdesc}

Per utilizzare Sysdig Monitor nel tuo cluster, devi eseguire il provisioning di un'istanza di {{site.data.keyword.mon_full_notm}} nel tuo account {{site.data.keyword.Bluemix_notm}} e configurare i tuoi cluster Kubernetes come origine delle metriche. Una volta configurato il cluster, le metriche vengono automaticamente raccolte e inoltrate alla tua istanza del servizio {{site.data.keyword.mon_full_notm}}. Puoi utilizzare il dashboard {{site.data.keyword.mon_full_notm}} per accedere alle tue metriche.   

Per ulteriori informazioni, vedi [Analizza le metriche per un'applicazione distribuita in un cluster Kubernetes](/docs/services/Monitoring-with-Sysdig?topic=Sysdig-kubernetes_cluster).

### Fatturazione e supporto
{: #sysdig-billing-support}

Poiché Sysdig Monitor è fornito da {{site.data.keyword.mon_full_notm}}, il tuo utilizzo è incluso nella fattura {{site.data.keyword.Bluemix_notm}} per i servizi di piattaforma. Per informazioni sui prezzi, esamina i piani disponibili nel [catalogo {{site.data.keyword.Bluemix_notm}} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cloud.ibm.com/observe/monitoring/create).

{{site.data.keyword.mon_full_notm}} è completamente integrato nel sistema di supporto {{site.data.keyword.Bluemix_notm}}. Se riscontri un problema con l'utilizzo di {{site.data.keyword.mon_full_notm}}, pubblica una domanda nel canale `sysdig-monitoring` in [{{site.data.keyword.containerlong_notm}} Slack ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://ibm-container-service.slack.com/) o apri un [caso di supporto {{site.data.keyword.Bluemix_notm}}](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support). Accedi a Slack utilizzando il tuo ID IBM. Se non utilizzi un ID IBM per il tuo account {{site.data.keyword.Bluemix_notm}}, [richiedi un invito a questo Slack ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://bxcs-slack-invite.mybluemix.net/).

## Portworx
{: #portworx-parter}

[Portworx ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://portworx.com/products/introduction/) è una soluzione SDS (software-defined storage) altamente disponibile che puoi utilizzare per gestire l'archiviazione persistente locale per i tuoi database inseriti nei contenitori e altre applicazioni con stato oppure per condividere dati tra i pod in più zone.
{: shortdesc}

**Cos'è SDS (software-defined storage)?** </br>
Una soluzione SDS astrae i dispositivi di archiviazione di vari tipi, diverse dimensioni o da fornitori differenti, che sono collegati ai nodi di lavoro nel tuo cluster. I nodi di lavoro con archiviazione disponibile sui dischi rigidi vengono aggiunti come un nodo a un cluster di archiviazione. In questo cluster, l'archiviazione fisica è virtualizzata e presentata come un pool di archiviazione virtuale all'utente. Il cluster di archiviazione è gestito dal software SDS. Se i dati devono essere memorizzati sul cluster di archiviazione, il software SDS decide dove archiviare i dati per la massima disponibilità. La tua archiviazione virtuale viene fornita con una serie comune di funzionalità e servizi di cui puoi avvalerti senza preoccuparti dell'effettiva architettura di archiviazione sottostante.

### Vantaggi
{: #portworx-benefits}

Esamina la seguente tabella per trovare un elenco dei vantaggi chiave che puoi ottenere utilizzando Portworx.
{: shortdesc}

|Vantaggi|Descrizione|
|-------------|------------------------------|
|Gestione dati e archiviazione nativa del cloud per applicazioni con stato|Portworx aggrega l'archiviazione locale disponibile che è collegata ai tuoi nodi di lavoro e che può variare in termini di dimensioni o tipo e crea un livello di archiviazione persistente unificato per i database inseriti nei contenitori o altre applicazioni con stato che desideri eseguire nel cluster. Utilizzando le attestazioni del volume persistente (PVC) di Kubernetes, puoi aggiungere l'archiviazione persistente locale alle tue applicazioni per archiviare i tuoi dati.|
|Dati altamente disponibili con la replica del volume|Portworx replica automaticamente i dati nei tuoi volumi tra i nodi di lavoro e le zone del tuo cluster in modo che i tuoi dati siano accessibili in qualsiasi momento e che la tua applicazione con stato possa essere ripianificata su un altro nodo di lavoro in caso di malfunzionamento o riavvio del nodo di lavoro. |
|Supporto per eseguire `hyper-converged`|Portworx può essere configurato per l'esecuzione di [`hyper-converged` ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://docs.portworx.com/portworx-install-with-kubernetes/storage-operations/hyperconvergence/) per garantire che le tue risorse di calcolo e l'archiviazione siano sempre posizionate sullo stesso nodo di lavoro. Quando la tua applicazione deve essere ripianificata, Portworx sposta l'applicazione su un nodo di lavoro in cui risiede una delle tue repliche del volume per garantire velocità di accesso al disco locale e prestazioni elevate per la tua applicazione con stato. |
|Crittografa i dati con {{site.data.keyword.keymanagementservicelong_notm}}|Puoi [configurare le chiavi di crittografia {{site.data.keyword.keymanagementservicelong_notm}}](/docs/containers?topic=containers-portworx#encrypt_volumes) protette da HSM (hardware security module) basati sul cloud con certificazione FIPS 140-2 Level 2 per proteggere i dati nei tuoi volumi. Puoi scegliere di utilizzare una chiave di crittografia per crittografare tutti i tuoi volumi in un cluster o di utilizzare invece una chiave di crittografia per ciascun volume. Portworx utilizza questa chiave per crittografare i dati inattivi e in fase di transito quando vengono inviati a un altro nodo di lavoro.|
|Istantanee integrate e backup cloud|Puoi salvare lo stato corrente di un volume e i suoi dati creando un'[istantanea Portworx ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://docs.portworx.com/portworx-install-with-kubernetes/storage-operations/create-snapshots/). Le istantanee possono essere archiviate sul tuo cluster Portworx locale o nel cloud.|
|Monitoraggio integrato con Lighthouse|[Lighthouse ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://docs.portworx.com/reference/lighthouse/) è uno strumento grafico molto facile da utilizzare per aiutarti a gestire e monitorare le tue istantanee di volume e i tuoi cluster Portworx. Con Lighthouse, puoi visualizzare lo stato del tuo cluster Portworx, compresi il numero di nodi di archiviazione disponibili, i volumi e la capacità disponibile e analizzare i tuoi dati in Prometheus, Grafana o Kibana.|
{: caption="Vantaggi dell'utilizzo di Portworx" caption-side="top"}

### Integrazione con {{site.data.keyword.containerlong_notm}}
{: #portworx-integration}

{{site.data.keyword.containerlong_notm}} fornisce dei tipi di nodo di lavoro che sono ottimizzati per l'utilizzo SDS e che sono forniti con uno o più dischi locali non elaborati, non formattati e non montati che puoi utilizzare per archiviare i tuoi dati. Portworx offre delle prestazioni ottimali quando utilizzi le [macchine di nodo di lavoro SDS](/docs/containers?topic=containers-planning_worker_nodes#sds) fornite con la velocità di rete di 10Gbps. Puoi tuttavia installare Portworx su tipi di nodo di lavoro non SDS ma potresti non ottenere i vantaggi prestazionali richiesti dalla tua applicazione.
{: shortdesc}

Portworx viene installato utilizzando un [grafico Helm](/docs/containers?topic=containers-portworx#install_portworx). Quando installi il grafico Helm, Portworx analizza automaticamente l'archiviazione persistente locale disponibile nel tuo cluster e aggiunge l'archiviazione al livello di archiviazione Portworx. Per aggiungere l'archiviazione dal livello di archiviazione Portworx alle tue applicazioni, devi utilizzare le [attestazioni del volume persistente di Kubernetes](/docs/containers?topic=containers-portworx#add_portworx_storage).

Per installare Portworx nel tuo cluster, devi avere una licenza Portworx. Se sei un utente principiante, puoi utilizzare l'edizione `px-enterprise` come versione di prova. La versione di prova ti offre la piena funzionalità di Portworx che puoi testare per 30 giorni. Una volta scaduta la versione di prova, devi [acquistare una licenza Portworx ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](/docs/containers?topic=containers-portworx#portworx_license) per continuare a utilizzare il tuo cluster Portworx.

Per ulteriori informazioni su come installare e utilizzare Portworx con {{site.data.keyword.containerlong_notm}}, vedi [Archiviazione di dati su SDS (software-defined storage) con Portworx](/docs/containers?topic=containers-portworx).

### Fatturazione e supporto
{: #portworx-billing-support}

Le macchine del nodo di lavoro SDS fornite con i dischi locali e le macchine virtuali che utilizzi per Portworx sono incluse nella tua fattura mensile di {{site.data.keyword.containerlong_notm}}. Per informazioni sui prezzi, vedi il [catalogo {{site.data.keyword.Bluemix_notm}} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cloud.ibm.com/kubernetes/catalog/cluster). La licenza Portworx comporta un costo separato e non è inclusa nella tua fattura mensile.
{: shortdesc}

Se riscontri un problema con l'utilizzo di Portworx o se vuoi comunicare mediante chat in merito alle configurazioni di Portworx per il tuo specifico caso d'utilizzo, pubblica una domanda nel canale `portworx-on-iks` in [{{site.data.keyword.containerlong_notm}} Slack ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://ibm-container-service.slack.com/). Accedi a Slack utilizzando il tuo ID IBM. Se non utilizzi un ID IBM per il tuo account {{site.data.keyword.Bluemix_notm}}, [richiedi un invito a questo Slack ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://bxcs-slack-invite.mybluemix.net/).
