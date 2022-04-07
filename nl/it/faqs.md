---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-05"

keywords: kubernetes, iks, compliance, security standards

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
{:download: .download}
{:preview: .preview}
{:faq: data-hd-content-type='faq'}


# Domande frequenti (FAQ)
{: #faqs}

## Cos'è Kubernetes?
{: #kubernetes}
{: faq}

Kubernetes è una piattaforma open source per la gestione di carichi di lavoro e servizi inseriti in un contenitore su più host e offre strumenti di gestione per la distribuzione, l'automazione, il monitoraggio e il ridimensionamento di applicazioni inserite in un contenitore con un intervento manuale minimo o nullo. Tutti i contenitori che costituiscono il tuo microservizio sono raggruppati in pod, un'unità logica per garantire una facile gestione e rilevazione. Questi pod vengono eseguiti su host di calcolo gestiti in un cluster Kubernetes che è portatile, estensibile e con riparazione automatica in caso di guasti.
{: shortdesc}

Per ulteriori informazioni su Kubernetes, vedi la [documentazione di Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/home/?path=users&persona=app-developer&level=foundational).

## Come funziona il servizio IBM Cloud Kubernetes?
{: #kubernetes_service}
{: faq}

Con {{site.data.keyword.containerlong_notm}}, puoi creare il tuo proprio cluster Kubernetes per distribuire e gestire le applicazioni inserite in un contenitore su {{site.data.keyword.Bluemix_notm}}. Le tue applicazioni inserite in un contenitore sono ospitate su host di calcolo dell'infrastruttura IBM Cloud (SoftLayer) denominati nodi di lavoro. Puoi scegliere di eseguire il provisioning dei tuoi host di calcolo come [macchine virtuali](/docs/containers?topic=containers-planning_worker_nodes#vm) con risorse condivise o dedicate oppure come [macchine bare metal](/docs/containers?topic=containers-planning_worker_nodes#bm) che possono essere ottimizzate per l'utilizzo di GPU ed SDS (software-defined storage). I tuoi nodi di lavoro sono controllati da un master Kubernetes altamente disponibile che è configurato, monitorato e gestito da IBM. Puoi utilizzare l'API o la CLI di {{site.data.keyword.containerlong_notm}} per lavorare con le risorse dell'infrastruttura del cluster e l'API o la CLI di Kubernetes per gestire le tue distribuzioni e i tuoi servizi.

Per ulteriori informazioni su come vengono configurate le tue risorse del cluster, vedi [Architettura del servizio](/docs/containers?topic=containers-ibm-cloud-kubernetes-service-technology#architecture). Per trovare un elenco di funzionalità e vantaggi, vedi [Perché {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-cs_ov#cs_ov).

## Perché dovrei utilizzare il servizio IBM Cloud Kubernetes?
{: #faq_benefits}
{: faq}

{{site.data.keyword.containerlong_notm}} è un'offerta di Kubernetes gestito che offre potenti strumenti, un'esperienza utente intuitiva e sicurezza integrata per la rapida consegna di applicazioni che puoi associare ai servizi cloud relativi a IBM Watson®, AI, IoT, DevOps, sicurezza e analisi dei dati. Come fornitore certificato di Kubernetes, {{site.data.keyword.containerlong_notm}} fornisce funzioni di pianificazione intelligente, riparazione automatica, adattamento orizzontale, rilevamento di servizi e bilanciamento del carico, rollout e rollback automatizzati e gestione dei segreti e della configurazione. Il servizio ha anche funzionalità avanzate per la gestione semplificata dei cluster, le politiche di sicurezza e isolamento dei contenitori, la capacità di progettare il tuo proprio cluster e strumenti operativi integrati per la coerenza nella distribuzione.

Per una panoramica dettagliata di funzionalità e vantaggi, vedi [Perché {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-cs_ov#cs_ov).

## Il servizio viene fornito con un master Kubernetes gestito e nodi di lavoro?
{: #managed_master_worker}
{: faq}

Ogni cluster Kubernetes in {{site.data.keyword.containerlong_notm}} è controllato da un master Kubernetes dedicato che viene gestito da IBM in un account dell'infrastruttura {{site.data.keyword.Bluemix_notm}} di proprietà di IBM. Il master Kubernetes, inclusi tutti componenti del master, le risorse di calcolo, di rete e di archiviazione, viene continuamente monitorato dagli SRE (Site Reliability Engineer) IBM. Gli SRE applicano i più recenti standard di sicurezza, rilevano e correggono le attività dannose e operano per garantire l'affidabilità e la disponibilità di {{site.data.keyword.containerlong_notm}}. I componenti aggiuntivi, come Fluentd per la registrazione, che vengono installati automaticamente quando esegui il provisioning del cluster vengono aggiornati automaticamente da IBM. Tuttavia, puoi scegliere di disabilitare gli aggiornamenti automatici per alcuni componenti aggiuntivi e aggiornarli manualmente e separatamente dai nodi master e di lavoro. Per ulteriori informazioni, vedi [Aggiornamento dei componenti aggiuntivi del cluster](/docs/containers?topic=containers-update#addons).

Periodicamente, Kubernetes rilascia [aggiornamenti principali, secondari o patch](/docs/containers?topic=containers-cs_versions#version_types). Questi aggiornamenti possono influire sulla versione del server API Kubernetes o su altri componenti nel tuo master Kubernetes. IBM aggiorna automaticamente la versione patch, ma devi aggiornare la versione principale e quella secondaria del master. Per ulteriori informazioni, vedi [Aggiornamento del master Kubernetes](/docs/containers?topic=containers-update#master).

Il provisioning dei nodi di lavoro nei cluster standard viene eseguito nel tuo account dell'infrastruttura {{site.data.keyword.Bluemix_notm}}. I nodi di lavoro sono dedicati al tuo account ed è una tua responsabilità richiedere degli aggiornamenti tempestivi ai nodi di lavoro per garantire che i componenti {{site.data.keyword.containerlong_notm}} e il sistema operativo dei nodi di lavoro siano applicati con gli aggiornamenti e le patch di sicurezza più recenti. Gli aggiornamenti e le patch di sicurezza sono resi disponibili dagli SRE (Site Reliability Engineer) IBM che monitorano continuamente l'immagine Linux installata sui tuoi nodi di lavoro per rilevare vulnerabilità e problemi di conformità della sicurezza. Per ulteriori informazioni, vedi [Aggiornamento dei nodi di lavoro](/docs/containers?topic=containers-update#worker_node).

## I nodi master e di lavoro Kubernetes sono altamente disponibili?
{: #faq_ha}
{: faq}

L'architettura e l'infrastruttura di {{site.data.keyword.containerlong_notm}} sono progettate per garantire affidabilità, bassa latenza di elaborazione e una massima operatività del servizio. Per impostazione predefinita, ogni cluster in {{site.data.keyword.containerlong_notm}} è configurato con più istanze del master Kubernetes per garantire la disponibilità e l'accessibilità delle risorse del cluster, anche se una o più istanze del master Kubernetes non sono disponibili.

Puoi rendere il tuo cluster ancora più altamente disponibile e proteggere la tua applicazione da tempi di inattività, estendendo i tuoi carichi di lavoro tra più nodi di lavoro in più zone di una regione. Questa configurazione viene chiamata [cluster multizona](/docs/containers?topic=containers-ha_clusters#multizone) e garantisce che la tua applicazione sia accessibile, anche se un nodo di lavoro o un'intera zona non è disponibile.

Per proteggerti da un malfunzionamento di un'intera regione, crea [più cluster e diffondili tra le regioni {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-ha_clusters#multiple_clusters). Configurando un NLB (network load balancer) per i tuoi cluster, puoi ottenere il bilanciamento del carico tra regioni e la rete inter-regionale per i cluster.

Se hai dei dati che devono essere disponibili, anche se si verifica un'interruzione, assicurati di memorizzare i tuoi dati nell'[archiviazione persistente](/docs/containers?topic=containers-storage_planning#storage_planning).

Per ulteriori informazioni su come ottenere l'alta disponibilità per il tuo cluster, vedi [Alta disponibilità per {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-ha#ha).

## Quali opzioni ho per proteggere il mio cluster?
{: #secure_cluster}
{: faq}

Puoi utilizzare le funzioni di sicurezza integrate in {{site.data.keyword.containerlong_notm}} per proteggere i componenti nel tuo cluster, i tuoi dati e le distribuzioni dell'applicazione per garantire la conformità della sicurezza e l'integrità dei dati. Utilizza queste funzioni per proteggere il server API Kubernetes, l'archivio dati etcd, il nodo di lavoro, la rete, l'archiviazione, le immagini e le distribuzioni da attacchi dolosi. Puoi anche avvalerti degli strumenti di registrazione e monitoraggio integrati per rilevare attacchi dolosi e modelli di utilizzo sospetti.

Per ulteriori informazioni sui componenti del tuo cluster e su come proteggere ciascun componente, vedi [Sicurezza per {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-security#security).

## Quali politiche di accesso assegno agli utenti del mio cluster?
{: #faq_access}
{: faq}

{{site.data.keyword.containerlong_notm}} utilizza {{site.data.keyword.iamshort}} (IAM) per concedere l'accesso alle risorse del cluster tramite i ruoli di piattaforma IAM e le politiche ruolo RBAC (role-based access control) di Kubernetes tramite i ruoli del servizio IAM. Per ulteriori informazioni sui tipi di politiche di accesso, vedi [Scegli la politica e il ruolo di accesso appropriati per i tuoi utenti](/docs/containers?topic=containers-users#access_roles).
{: shortdesc}

Le politiche di accesso che assegni agli utenti dipendono da ciò che vuoi che i tuoi utenti possano fare. Per ulteriori informazioni sui tipi di azioni autorizzate per ciascun ruolo, consulta la [pagina di riferimento sugli accessi utente](/docs/containers?topic=containers-access_reference) o i link della seguente tabella. Per la procedura di assegnazione delle politiche, vedi [Concessione dell'accesso utente al tuo cluster tramite {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform)

| Caso d'uso | Ruoli e ambito di esempio |
| --- | --- |
| Revisore applicazione | [Ruolo della piattaforma Visualizzatore per un cluster, una regione o un gruppo di risorse](/docs/containers?topic=containers-access_reference#view-actions), [Ruolo del servizio di lettore per un cluster, una regione o un gruppo di risorse](/docs/containers?topic=containers-access_reference#service). |
| Sviluppatori di applicazioni | [Ruolo della piattaforma Editor per un cluster](/docs/containers?topic=containers-access_reference#editor-actions), [Ruolo del servizio di scrittore con ambito delimitato a uno spazio dei nomi](/docs/containers?topic=containers-access_reference#service), [Ruolo dello spazio Sviluppatore](/docs/containers?topic=containers-access_reference#cloud-foundry). |
| Fatturazione | [Ruolo della piattaforma Visualizzatore per un cluster, una regione o un gruppo di risorse](/docs/containers?topic=containers-access_reference#view-actions). |
| Crea un cluster | Autorizzazioni a livello di account per le credenziali dell'infrastruttura Super utente, ruolo della piattaforma Amministratore per {{site.data.keyword.containerlong_notm}} e ruolo della piattaforma Amministratore per {{site.data.keyword.registrylong_notm}}. Per ulteriori informazioni, vedi [Preparazione alla creazione dei cluster](/docs/containers?topic=containers-clusters#cluster_prepare).|
| Amministratore cluster | [Ruolo della piattaforma Amministratore per un cluster](/docs/containers?topic=containers-access_reference#admin-actions), [Ruolo di servizio Gestore senza ambito delimitato a uno spazio dei nomi (per l'intero cluster)](/docs/containers?topic=containers-access_reference#service).|
| Operatore DevOps | [Ruolo della piattaforma Operatore per un cluster](/docs/containers?topic=containers-access_reference#operator-actions), [Ruolo del servizio di scrittore senza ambito delimitato a uno spazio dei nomi (per l'intero cluster)](/docs/containers?topic=containers-access_reference#service), [Ruolo dello spazio Sviluppatore Cloud Foundry](/docs/containers?topic=containers-access_reference#cloud-foundry).  |
| Operatore o ingegnere dell'affidabilità del sito | [Ruolo della piattaforma Visualizzatore per un cluster, una regione o un gruppo di risorse](/docs/containers?topic=containers-access_reference#admin-actions), [Ruolo del servizio di lettore per un cluster o una regione](/docs/containers?topic=containers-access_reference#service) o [Ruolo di servizio Gestore per tutti gli spazi dei nomi dei cluster](/docs/containers?topic=containers-access_reference#service) per poter utilizzare i comandi `kubectl top nodes,pods`. |
{: caption="Tipi di ruoli che puoi assegnare per soddisfare i diversi casi di utilizzo." caption-side="top"}

## Dove posso trovare un elenco dei bollettini di sicurezza che interessano il mio cluster?
{: #faq_security_bulletins}
{: faq}

Se vengono rilevate delle vulnerabilità in Kubernetes, quest'ultima rilascia delle CVE in bollettini di sicurezza per informare gli utenti e descrivere le azioni che devono eseguire per ovviare alla vulnerabilità. I bollettini di sicurezza di Kubernetes che interessano gli utenti {{site.data.keyword.containerlong_notm}} o la piattaforma {{site.data.keyword.Bluemix_notm}} sono pubblicati nei [Bollettini di sicurezza di {{site.data.keyword.Bluemix_notm}}](https://cloud.ibm.com/status?component=containers-kubernetes&selected=security).

Alcune CVE richiedono l'aggiornamento patch più recente per una versione Kubernetes che puoi installare come parte del regolare [processo di aggiornamento del cluster](/docs/containers?topic=containers-update#update) in {{site.data.keyword.containerlong_notm}}. Assicurati di applicare le patch di sicurezza in tempo per proteggere il tuo cluster da attacchi dolosi. Per informazioni su cosa è incluso in una patch di sicurezza, fai riferimento a [Changelog versione](/docs/containers?topic=containers-changelog#changelog).

## Il servizio offre supporto per bare metal e GPU?
{: #bare_metal_gpu}
{: faq}

Sì, puoi eseguire il provisioning del tuo nodo di lavoro come server bare metal fisico a singolo tenant. I server bare metal offrono vantaggi ad alte prestazioni per carichi di lavoro come dati, intelligenza artificiale e GPU. Inoltre, tutte le risorse hardware sono dedicate ai tuoi carichi di lavoro, quindi non devi preoccuparti degli "elementi di disturbo".

Per ulteriori informazioni sulle varietà bare metal disponibili e su come il bare metal è diverso dalle macchine virtuali, vedi [Macchine fisiche (bare metal)](/docs/containers?topic=containers-planning_worker_nodes#bm).

## Quali versioni di Kubernetes sono supportate dal servizio?
{: #supported_kube_versions}
{: faq}

{{site.data.keyword.containerlong_notm}} supporta contemporaneamente più versioni di Kubernetes. Quando viene rilasciata una versione più recente (n), sono supportate fino a 2 versioni precedenti (n-2). Le versioni che sono più di 2 precedenti rispetto all'ultima (n-3) sono prima dichiarate obsolete e quindi non più supportate. Attualmente sono supportate le seguenti versioni:

*   Più recente: 1.14.2
*   Predefinita: 1.13.6
*   Altro: 1.12.9

Per ulteriori informazioni sulle versioni supportate e sulle azioni di aggiornamento che devi eseguire per passare da una versione all'altra, vedi [Informazioni sulla versione e azioni di aggiornamento](/docs/containers?topic=containers-cs_versions#cs_versions).

## Dove è disponibile il servizio?
{: #supported_regions}
{: faq}

{{site.data.keyword.containerlong_notm}} è disponibile in tutto il mondo. Puoi creare dei cluster standard in ogni regione {{site.data.keyword.containerlong_notm}} supportata. I cluster gratuiti sono disponibili solo in regioni selezionate.

Per ulteriori informazioni sulle regioni supportate, vedi [Ubicazioni](/docs/containers?topic=containers-regions-and-zones#regions-and-zones).

## A quali standard è conforme il servizio?
{: #standards}
{: faq}

{{site.data.keyword.containerlong_notm}} implementa controlli commisurati ai seguenti standard:
- Framework Scudo UE-USA per la privacy (EU-US Privacy Shield) e Scudo per la privacy Svizzera-USA (Swiss-US Privacy Shield)
- HIPAA (Health Insurance Portability and Accountability Act)
- Standard Service Organization Control (SOC 1, SOC 2 Type 1)
- International Standard on Assurance Engagements 3402 (ISAE 3402), Assurance Reports on Controls at a Service Organization
- International Organization for Standardization (ISO 27001, ISO 27017, ISO 27018)
- Payment Card Industry Data Security Standard (PCI DSS)

## Posso utilizzare IBM Cloud e altri servizi con il mio cluster?
{: #faq_integrations}
{: faq}

Puoi aggiungere i servizi di infrastruttura e piattaforma {{site.data.keyword.Bluemix_notm}} e i servizi di fornitori di terze parti al tuo cluster {{site.data.keyword.containerlong_notm}} per abilitare l'automazione, aumentare la sicurezza o migliorare le capacità di monitoraggio e registrazione nel cluster.

Per un elenco di servizi supportati, vedi [Integrazione dei servizi](/docs/containers?topic=containers-supported_integrations#supported_integrations).

## Posso connettere il mio cluster in IBM Cloud pubblico con applicazioni eseguite nel mio data center in loco?
{: #hybrid}
{: faq}

Puoi connettere i servizi in {{site.data.keyword.Bluemix_notm}} pubblico con il tuo data center in loco per creare la tua propria configurazione di cloud ibrido. Gli esempi su come sfruttare {{site.data.keyword.Bluemix_notm}} pubblico e privato con le applicazioni eseguite nel tuo data center in loco includono:
- Crei un cluster con {{site.data.keyword.containerlong_notm}} in {{site.data.keyword.Bluemix_notm}} pubblico, ma vuoi connettere il tuo cluster con un database installato in loco.
- Crei un cluster Kubernetes in {{site.data.keyword.Bluemix_notm}} privato nel tuo data center e distribuisci le applicazioni nel tuo cluster. Tuttavia, la tua applicazione potrebbe utilizzare un servizio {{site.data.keyword.ibmwatson_notm}}, ad esempio Tone Analyzer, in {{site.data.keyword.Bluemix_notm}} pubblico.

Per abilitare la comunicazione tra i servizi eseguiti in {{site.data.keyword.Bluemix_notm}} pubblico e i servizi eseguiti in loco, devi [configurare una connessione VPN](/docs/containers?topic=containers-vpn#vpn). Per connettere il tuo ambiente {{site.data.keyword.Bluemix_notm}} pubblico o dedicato con un ambiente {{site.data.keyword.Bluemix_notm}} privato, vedi [Utilizzo di {{site.data.keyword.containerlong_notm}} con {{site.data.keyword.Bluemix_notm}} privato](/docs/containers?topic=containers-hybrid_iks_icp#hybrid_iks_icp).

Per una panoramica delle offerte {{site.data.keyword.containerlong_notm}} supportate, vedi [Confronto delle offerte e le loro combinazioni](/docs/containers?topic=containers-cs_ov#differentiation).

## Posso distribuire il servizio IBM Cloud Kubernetes nel mio data center?
{: #private}
{: faq}

Se non vuoi spostare le tue applicazioni in {{site.data.keyword.Bluemix_notm}} pubblico, ma vuoi comunque avvalerti delle funzioni di {{site.data.keyword.containerlong_notm}}, puoi installare {{site.data.keyword.Bluemix_notm}} privato. {{site.data.keyword.Bluemix_notm}} privato è una piattaforma applicativa che può essere installata localmente sulle tue macchine e che puoi utilizzare per sviluppare e gestire applicazioni inserite in un contenitore in loco nel tuo ambiente controllato dietro un firewall.

Per ulteriori informazioni, consulta la [documentazione del prodotto {{site.data.keyword.Bluemix_notm}} privato ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_1.2.0/kc_welcome_containers.html).

## Cosa mi viene addebitato quando utilizzo il servizio IBM Cloud Kubernetes?
{: #charges}
{: faq}

Con i cluster {{site.data.keyword.containerlong_notm}}, puoi utilizzare le risorse di calcolo, rete e archiviazione dell'infrastruttura IBM Cloud (SoftLayer) con i servizi di piattaforma come Watson AI o Compose Database-as-a-Service. Ogni risorsa potrebbe comportare i propri addebiti che possono essere [fissi, a consumo, a livelli o riservati](/docs/billing-usage?topic=billing-usage-charges#charges).
* [Nodi di lavoro](#nodes)
* [Rete in uscita](#bandwidth)
* [Indirizzi IP della sottorete](#subnet_ips)
* [Archiviazione](#persistent_storage)
* [Servizi {{site.data.keyword.Bluemix_notm}}](#services)
* [Red Hat OpenShift on IBM Cloud](#rhos_charges)

<dl>
<dt id="nodes">Nodi di lavoro</dt>
  <dd><p>I cluster possono avere due tipi principali di nodi di lavoro: le macchine virtuali o fisiche (bare metal). La disponibilità del tipo di macchina e il prezzo variano in base alla zona in cui distribuisci il tuo cluster.</p>
  <p>Le <strong>macchine virtuali</strong> offrono una maggiore flessibilità, tempi di provisioning più rapidi e più funzioni di scalabilità automatica rispetto alle macchine bare metal, ad un prezzo più conveniente. Tuttavia, le VM hanno un compromesso in termini di prestazioni rispetto alle specifiche bare metal, come le reti Gbps, le soglie di RAM e di memoria e le opzioni di archiviazione. Tieni presente questi fattori che influiscono sui costi della tua VM:</p>
  <ul><li><strong>Condiviso o dedicato</strong>: se condividi l'hardware sottostante della VM, il costo è inferiore rispetto all'hardware dedicato, ma le risorse fisiche non sono dedicate alla VM.</li>
  <li><strong>Solo fatturazione oraria</strong>: la fatturazione oraria offre maggiore flessibilità nell'ordinare e annullare rapidamente le VM.
  <li><strong>Livelli di ore al mese</strong>: la fatturazione oraria è a livelli. Poiché la tua VM rimane ordinata per un certo livello di ore entro un mese di fatturazione, la tariffa oraria che ti viene addebitata diminuisce. I livelli di ore sono i seguenti: 0 - 150 ore, 151 - 290 ore, 291 - 540 ore e 541+ ore.</li></ul>
  <p>Le <strong>macchine fisiche (bare metal)</strong> offrono prestazioni elevate per carichi di lavoro come dati, intelligenza artificiale e GPU. Inoltre, tutte le risorse hardware sono dedicate ai tuoi carichi di lavoro, quindi non hai "elementi di disturbo". Tieni presente questi fattori che influiscono sui costi della tua macchina bare metal:</p>
  <ul><li><strong>Solo fatturazione mensile</strong>: tutte le macchine bare metal vengono addebitate mensilmente.</li>
  <li><strong>Processo di ordinazione più lungo</strong>:  dopo che hai ordinato o annullato un server bare metal, il processo viene completato manualmente nel tuo account dell'infrastruttura IBM Cloud (SoftLayer). Pertanto, ci vuole più di un giorno lavorativo per completare questo processo.</li></ul>
  <p>Per i dettagli sulle specifiche delle macchine, vedi [Hardware disponibile per i nodi di lavoro](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes).</p></dd>

<dt id="bandwidth">Larghezza di banda pubblica</dt>
  <dd><p>La larghezza di banda si riferisce al trasferimento di dati pubblici del traffico di rete in entrata e in uscita, sia da che verso le risorse {{site.data.keyword.Bluemix_notm}} nei data center in tutto il mondo. La larghezza di banda pubblica viene addebitata per GB. Puoi rivedere il tuo riepilogo di larghezza di banda corrente accedendo alla [console {{site.data.keyword.Bluemix_notm}}](https://cloud.ibm.com/), dal menu ![Icona menu](../icons/icon_hamburger.svg "Icona Menu") selezionando **Infrastruttura classica** e selezionando quindi la pagina **Rete > Larghezza di banda> Riepilogo**.
  <p>Esamina i seguenti fattori che influiscono sui costi della larghezza di banda pubblica:</p>
  <ul><li><strong>Ubicazione</strong>: come per i nodi di lavoro, gli addebiti variano a seconda della zona in cui vengono distribuite le tue risorse.</li>
  <li><strong>Larghezza di banda inclusa o a pagamento</strong>: le macchine del tuo nodo di lavoro potrebbero essere dotate di una determinata assegnazione di rete in uscita al mese, come 250 GB per le VM o 500 GB per bare metal. Oppure, l'assegnazione potrebbe essere a consumo, in base all'utilizzo di GB.</li>
  <li><strong>Pacchetti a più livelli</strong>: dopo aver superato qualsiasi larghezza di banda inclusa, vieni addebitato in base a uno schema di utilizzo a più livelli che varia a seconda dell'ubicazione. Se superi l'assegnazione di un livello, ti potrebbe anche essere addebitata una tariffa di trasferimento dati standard.</li></ul>
  <p>Per ulteriori informazioni, vedi [Pacchetti di larghezza di banda ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/cloud/bandwidth).</p></dd>

<dt id="subnet_ips">Indirizzi IP della sottorete</dt>
  <dd><p>Quando crei un cluster standard, viene ordinata una sottorete pubblica portatile con 8 indirizzi IP pubblici che viene addebitata mensilmente al tuo account.</p><p>Se hai già sottoreti disponibili nel tuo account dell'infrastruttura, puoi invece utilizzare queste sottoreti. Crea il cluster con l'[indicatore](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_create) `--no-subnets`, quindi [riutilizza le tue sottoreti](/docs/containers?topic=containers-subnets#subnets_custom).</p>
  </dd>

<dt id="persistent_storage">Archiviazione</dt>
  <dd>Quando esegui il provisioning dell'archiviazione, puoi scegliere il tipo di archiviazione e la classe di archiviazione più adatti al tuo caso di utilizzo. Gli addebiti variano in base al tipo di archiviazione, all'ubicazione e alle specifiche dell'istanza di archiviazione. Alcune soluzioni di archiviazione, come l'archiviazione file e blocchi, offrono piani orari e mensili tra cui puoi scegliere. Per scegliere la giusta soluzione di archiviazione, vedi [Pianificazione di archiviazione persistente altamente disponibile](/docs/containers?topic=containers-storage_planning#storage_planning). Per ulteriori informazioni, consulta:
  <ul><li>[Prezzi dell'archiviazione file NFS![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/cloud/file-storage/pricing)</li>
  <li>[Prezzi dell'archiviazione blocchi![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/cloud/block-storage/pricing)</li>
  <li>[Piani di archiviazione oggetti![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api)</li></ul></dd>

<dt id="services">Servizi {{site.data.keyword.Bluemix_notm}}</dt>
  <dd>Ogni servizio che integri con il tuo cluster ha il proprio modello di prezzo. Consulta la documentazione di ciascun prodotto e utilizza la console {{site.data.keyword.Bluemix_notm}} per [stimare i costi](/docs/billing-usage?topic=billing-usage-cost#cost).</dd>

<dt id="rhos_charges">Red Hat OpenShift on IBM Cloud</dt>
  <dd>
  <p class="preview">[Red Hat OpenShift on IBM Cloud](/docs/containers?topic=containers-openshift_tutorial) è disponibile come versione beta per testare i cluster OpenShift.</p>Se crei un [Red Hat OpenShift su cluster IBM Cloud](/docs/containers?topic=containers-openshift_tutorial), i tuoi nodi di lavoro vengono installati con il sistema operativo Red Hat Enterprise Linux, con un aumento del prezzo delle [macchine del nodo di lavoro](#nodes). Devi avere anche una licenza OpenShift, che prevede un costo mensile oltre al costo orario della VM o al costo mensile del bare metal. Occorre una licenza OpenShift ogni 2 core del tipo di nodo di lavoro. Se elimini il tuo nodo di lavoro prima della fine del mese, la tua licenza mensile è disponibile per gli altri nodi di lavoro del pool di nodi di lavoro. Per ulteriori informazioni sui cluster OpenShift, vedi [Creazione di un Red Hat OpenShift su cluster IBM Cloud](/docs/containers?topic=containers-openshift_tutorial).</dd>

</dl>
<br><br>

Le risorse mensili vengono fatturate in base al primo giorno del mese per l'utilizzo nel mese precedente. Se ordini una risorsa mensile a metà mese, ti verrà addebitato un importo proporzionale per quel mese. Tuttavia, se annulli una risorsa a metà del mese, ti verrà comunque addebitato l'intero importo per la risorsa mensile.
{: note}

## Le mie risorse di piattaforma e infrastruttura sono consolidate in un'unica fattura?
{: #bill}
{: faq}

Quando utilizzi un account {{site.data.keyword.Bluemix_notm}} fatturabile, le risorse di piattaforma e infrastruttura sono riepilogate in una sola fattura.
Se hai collegato i tuoi account di {{site.data.keyword.Bluemix_notm}} e dell'infrastruttura IBM Cloud (SoftLayer), ricevi una [fattura consolidata](/docs/customer-portal?topic=customer-portal-unifybillaccounts#unifybillaccounts) per le tue risorse di piattaforma e infrastruttura {{site.data.keyword.Bluemix_notm}}.

## Posso stimare i miei costi?
{: #cost_estimate}
{: faq}

Sì, vedi [Stima dei costi](/docs/billing-usage?topic=billing-usage-cost#cost). Tieni presente che alcuni costi non sono riflessi nella stima, come ad esempio il prezzo a livelli per un utilizzo orario aumentato. Per ulteriori informazioni, vedi [Cosa mi viene addebitato quando utilizzo {{site.data.keyword.containerlong_notm}}?](#charges).

## Posso visualizzare il mio utilizzo corrente?
{: #usage}
{: faq}

Puoi controllare il tuo utilizzo corrente e i totali mensili stimati per le tue risorse di piattaforma e infrastruttura {{site.data.keyword.Bluemix_notm}}. Per ulteriori informazioni, vedi [Visualizzazione del tuo utilizzo](/docs/billing-usage?topic=billing-usage-viewingusage#viewingusage). Per organizzare la tua fatturazione, puoi raggruppare le tue risorse con i [gruppi di risorse](/docs/resources?topic=resources-bp_resourcegroups#bp_resourcegroups).
