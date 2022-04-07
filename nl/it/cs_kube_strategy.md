---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-03"

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


# Definizione della tua strategia Kubernetes
{: #strategy}

Con {{site.data.keyword.containerlong}}, puoi distribuire in modo rapido e sicuro i carichi di lavoro del contenitore delle tue applicazioni produttive. Le seguenti informazioni aggiuntive ti consentono di pianificare la strategia del tuo cluster, ottimizzando la tua configurazione per ottenere il massimo dalle funzioni di distribuzione automatizzata, ridimensionamento e gestione dell'orchestrazione di [Kubernetes![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/).
{:shortdesc}

## Spostamento dei carichi di lavoro in {{site.data.keyword.Bluemix_notm}}
{: #cloud_workloads}

Sono numerosi i motivi per cui spostare i tuoi carichi di lavoro in {{site.data.keyword.Bluemix_notm}}: riduzione del costo totale di proprietà, aumento dell'alta disponibilità delle tue applicazioni in un ambiente sicuro e conforme, ampliamento e riduzione in risposta alla domanda degli utenti e molti altri ancora. {{site.data.keyword.containerlong_notm}} combina la tecnologia dei contenitori con strumenti open source, quale Kubernetes, per consentirti di creare un'applicazione nativa del cloud capace di migrare in diversi ambienti cloud, evitando il vendor lock-in.
{:shortdesc}

Ma come si arriva al cloud? Quali opzioni incontri lungo il percorso? Come gestisci i carichi di lavoro una volta nel cloud?

Utilizza questa pagina per apprendere alcune strategie sulle distribuzioni Kubernetes su {{site.data.keyword.containerlong_notm}}. In qualsiasi momento, non esitare a contattarci su [Slack. ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://ibm-container-service.slack.com)

Non hai ancora accesso a Slack? [Richiedi un invito!](https://bxcs-slack-invite.mybluemix.net/)
{: tip}

### Cosa posso spostare in {{site.data.keyword.Bluemix_notm}}?
{: #move_to_cloud}

Con {{site.data.keyword.Bluemix_notm}}, hai la flessibilità di creare dei cluster Kubernetes in [ambienti cloud non in loco, in loco o ibridi](/docs/containers?topic=containers-cs_ov#differentiation). La seguente tabella fornisce alcuni esempi dei tipi di carico di lavoro che gli utenti generalmente spostano nei vari tipi di cloud. Puoi anche scegliere un approccio ibrido laddove hai dei cluster in esecuzione in entrambi gli ambienti.
{: shortdesc}

| Carico di lavoro | {{site.data.keyword.containershort_notm}} non in loco | in loco |
| --- | --- | --- |
| Strumenti di abilitazione DevOps | <img src="images/confirm.svg" width="32" alt="Funzione disponibile" style="width:32px;" /> | |
| Applicazioni di sviluppo e test | <img src="images/confirm.svg" width="32" alt="Funzione disponibile" style="width:32px;" /> | |
| Applicazioni con variazioni importanti nella domanda che necessitano di un ridimensionamento rapido | <img src="images/confirm.svg" width="32" alt="Funzione disponibile" style="width:32px;" /> | |
| Applicazioni aziendali quali CRM, HCM, ERP ed e-commerce | <img src="images/confirm.svg" width="32" alt="Funzione disponibile" style="width:32px;" /> | |
| Strumenti di collaborazione e social media quali l'e-mail | <img src="images/confirm.svg" width="32" alt="Funzione disponibile" style="width:32px;" /> | |
| Carichi di lavoro Linux e x86 | <img src="images/confirm.svg" width="32" alt="Funzione disponibile" style="width:32px;" /> | |
| Risorse di calcolo GPU e bare metal | <img src="images/confirm.svg" width="32" alt="Funzione disponibile" style="width:32px;" /> | <img src="images/confirm.svg" width="32" alt="Funzione disponibile" style="width:32px;" /> |
| Carichi di lavoro conformi a PCI e HIPAA | <img src="images/confirm.svg" width="32" alt="Funzione disponibile" style="width:32px;" /> | <img src="images/confirm.svg" width="32" alt="Funzione disponibile" style="width:32px;" /> |
| Applicazioni legacy con vincoli e dipendenze legati alla piattaforma e all'infrastruttura | | <img src="images/confirm.svg" width="32" alt="Funzione disponibile" style="width:32px;" /> |
| Applicazioni proprietarie con progettazioni o concessione in licenza rigidi o normative severe | | <img src="images/confirm.svg" width="32" alt="Funzione disponibile" style="width:32px;" /> |
| Ridimensionamento delle applicazioni nel cloud pubblico e sincronizzazione dei dati con un database privato in loco | <img src="images/confirm.svg" width="32" alt="Funzione disponibile" style="width:32px;" />  | <img src="images/confirm.svg" width="32" alt="Funzione disponibile" style="width:32px;" /> |
{: caption="Le implementazioni {{site.data.keyword.Bluemix_notm}} supportano i tuoi carichi di lavoro" caption-side="top"}

**Pronto a eseguire carichi di lavoro non in loco in {{site.data.keyword.containerlong_notm}}?**</br>
Ottimo! Ti trovi già nella nostra documentazione sul cloud pubblico. Continua a leggere per conoscere ulteriori idee sulle strategie e metterti subito al lavoro attraverso la [creazione immediata di un cluster](/docs/containers?topic=containers-getting-started).

**Interessato a un cloud in loco?**</br>
Esplora la documentazione di [{{site.data.keyword.Bluemix_notm}} Private![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.1/kc_welcome_containers.html). Se hai già effettuato investimenti importanti nella tecnologia IBM, ad esempio per prodotti quali WebSphere Application Server e Liberty, puoi ottimizzare la tua strategia di modernizzazione di {{site.data.keyword.Bluemix_notm}} Private con vari strumenti.

**Vuoi eseguire i carichi di lavoro sia in cloud in loco che non in loco?**</br>
Comincia con la configurazione di un account {{site.data.keyword.Bluemix_notm}} Private. Quindi, vedi [Utilizzo di {{site.data.keyword.containerlong_notm}} con {{site.data.keyword.Bluemix_notm}} Private](/docs/containers?topic=containers-hybrid_iks_icp) per connettere il tuo ambiente {{site.data.keyword.Bluemix_notm}} Privato con un cluster in {{site.data.keyword.Bluemix_notm}} Public. Per gestire più cluster cloud Kubernetes, come ad esempio tra {{site.data.keyword.Bluemix_notm}} Public e {{site.data.keyword.Bluemix_notm}} Private, consulta [IBM Multicloud Manager ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.1.0/mcm/getting_started/introduction.html).

### Che tipo di applicazioni posso eseguire in {{site.data.keyword.containerlong_notm}}?
{: #app_types}

La tua applicazione inserita in un contenitore deve essere in grado di funzionare sul sistema operativo supportato, Ubuntu 16.64, 18.64. Vuoi anche valutare la presenza di uno stato nella tua applicazione.
{: shortdesc}

<dl>
<dt>Applicazioni senza stato</dt>
  <dd><p>Le applicazioni senza stato sono preferibili per ambienti nativi del cloud, quale Kubernetes. Sono semplici da migrare e ridimensionare, poiché dichiarano le dipendenze, archiviano le configurazioni separatamente dal codice e gestiscono i servizi di backup, quali i database, come risorse collegate piuttosto che abbinate all'applicazione. I pod dell'applicazione non richiedono un'archiviazione dati persistente o un indirizzo IP di rete stabile e, in quanto tali, possono essere terminati, ripianificati e ridimensionati in base alle domande di carico di lavoro. L'applicazione utilizza un Database-as-a-Service per i dati persistenti, e servizi NodePort, di bilanciamento del carico o Ingress per esporre il carico di lavoro su un indirizzo IP stabile.</p></dd>
<dt>Applicazioni con stato</dt>
  <dd><p>Rispetto alle applicazioni senza stato, le applicazioni con stato sono più difficili da configurare, gestire e ridimensionare, poiché i pod richiedono dati persistenti e un'identità di rete stabile. Le applicazioni con stato sono spesso costituite da database o altri carichi di lavoro distribuiti con utilizzo intensivo di dati, in cui l'elaborazione è più efficiente vicino ai dati stessi.</p>
  <p>Se desideri distribuire un'applicazione con stato, devi configurare l'archiviazione persistente e montare un volume persistente per il pod controllato da un oggetto StatefulSet. Puoi scegliere di aggiungere un'archiviazione [file](/docs/containers?topic=containers-file_storage#file_statefulset), [blocchi](/docs/containers?topic=containers-block_storage#block_statefulset) od [oggetti](/docs/containers?topic=containers-object_storage#cos_statefulset) come archiviazione persistente per la tua serie con stato. Puoi anche installare [Portworx](/docs/containers?topic=containers-portworx) sui tuoi nodi di lavoro bare metal e utilizzarlo come soluzione SDS (software-defined storage) altamente disponibile per gestire l'archiviazione persistente delle tue applicazioni con stato. Per ulteriori informazioni sul funzionamento delle serie con stato, vedi la [documentazione di Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/).</p></dd>
</dl>

### Quali sono le linee guida per lo sviluppo di applicazioni native del cloud senza stato?
{: #12factor}

Vedi [Twelve-Factor App ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://12factor.net/), una metodologia indipendente dal linguaggio utilizzata per valutare come sviluppare la tua app alla luce di 12 fattori, riassunti come segue.
{: shortdesc}

1.  **Codebase**: utilizza una sola codebase in un sistema di controllo della versione per le tue distribuzioni. Quando estrai un'immagine per la distribuzione del tuo contenitore, specifica una tag dell'immagine testata invece di utilizzare `latest`.
2.  **Dipendenze**: dichiara esplicitamente e isola le dipendenze esterne.
3.  **Configurazione**: memorizza la configurazione specifica della distribuzione nelle variabili di ambiente, non nel codice.
4.  **Servizi di supporto**: gestisci i servizi di supporto, quali archivi dati o code di messaggi, come risorse collegate o sostituibili.
5.  **Fasi dell'applicazione**: crea in fasi distinte quali `build`, `release`, `run`, con una separazione netta delle fasi.
6.  **Processi**: esegui come uno o più processi senza stato che non condividono nulla e utilizzano l'[archiviazione persistente](/docs/containers?topic=containers-storage_planning) per il salvataggio dei dati.
7.  **Bind delle porte**: i bind delle porte sono autonomi e forniscono un endpoint del servizio su un host e una porta ben definiti.
8.  **Simultaneità**: gestisci e ridimensiona la tua applicazione attraverso istanze di processo quali repliche e adattamento orizzontale. Imposta limiti o richieste di risorse per le tue distribuzioni. Nota che le politiche di rete Calico non possono limitare la larghezza di banda. Valuta, invece, [Istio](/docs/containers?topic=containers-istio).
9.  **Disponibilità**: progetta la tua applicazione in modo che sia disponibile, con avvio minimo, arresto normale e tolleranza delle chiusure improvvise dei processi. Ricorda che contenitori, pod e nodi di lavoro sono fatti per essere disponibili, quindi pianifica la tua applicazione di conseguenza.
10.  **Parità da sviluppo a produzione**: configura una pipeline di [integrazione
continua](https://www.ibm.com/cloud/garage/content/code/practice_continuous_integration/) e [fornitura continua](https://www.ibm.com/cloud/garage/content/deliver/practice_continuous_delivery/) per la tua applicazione,
con differenze minime tra l'applicazione in fase di sviluppo e l'applicazione in produzione.
11.  **Log**: gestisci i log come flussi di eventi: processi degli ambienti esterni o di hosting e file di log degli instradamenti. **Importante**: in {{site.data.keyword.containerlong_notm}}, i log non vengono attivati per impostazione predefinita. Per abilitarli, vedi [Configurazione dell'inoltro dei log](/docs/containers?topic=containers-health#configuring).
12.  **Processi di amministrazione**: conserva gli script di amministrazione una tantum della tua applicazione ed eseguili come [oggetto job Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/concepts/workloads/controllers/jobs-run-to-completion/) per garantire che gli script di amministrazione vengano eseguiti con lo stesso ambiente dell'applicazione stessa. Per l'orchestrazione dei pacchetti più grandi che desideri eseguire nei tuoi cluster Kubernetes, valuta l'utilizzo di un gestore pacchetti quale [Helm ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://helm.sh/).

### Ho già un'applicazione. Come posso eseguirne la migrazione a {{site.data.keyword.containerlong_notm}}?
{: #migrate_containerize}

Puoi eseguire alcuni passi generali per inserire le tue applicazioni in contenitori, come di seguito descritto.
{: shortdesc}

1.  Utilizza [Twelve-Factor App ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://12factor.net/) come guida per isolare le dipendenze, separare i processi in servizi distinti e ridurre il più possibile la presenza di uno stato nella tua applicazione.
2.  Trova un'immagine di base appropriata da utilizzare. Puoi utilizzare immagini le immagini pubbliche di [Docker Hub ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://hub.docker.com/), [immagini pubbliche IBM](/docs/services/Registry?topic=registry-public_images#public_images), o creare e gestire le tue immagini nel tuo {{site.data.keyword.registryshort_notm}} privato.
3.  Aggiungi alla tua immagine Docker solo ciò che occorre per eseguire l'applicazione.
4.  Invece di affidarti all'archiviazione locale, pianifica di utilizzare soluzioni DBaaS (database-as-a-service) cloud o di archiviazione persistente per eseguire il backup dei dati della tua applicazione.
5.  Nel tempo, esegui il refactoring dei processi della tua applicazione in microservizi.

Per ulteriori informazioni, vedi le seguenti esercitazioni:
*  [Migrazione di un'applicazione da Cloud Foundry a un cluster](/docs/containers?topic=containers-cf_tutorial#cf_tutorial)
*  [Spostamento di un'applicazione basata su VM a Kubernetes](/docs/tutorials?topic=solution-tutorials-vm-to-containers-and-kubernetes)



Prosegui con i seguenti argomenti per ulteriori valutazioni da eseguire durante lo spostamento dei carichi di lavoro, quali gli ambienti Kubernetes, l'alta disponibilità, il rilevamento dei servizi e le distribuzioni.

<br />


### Quali conoscenze e competenze tecniche mi conviene avere prima che sposti le mie applicazioni su {{site.data.keyword.containerlong_notm}}?
{: #knowledge}

Kubernetes è progettato per fornire funzionalità a due figure principali, l'amministratore del cluster e lo sviluppatore dell'applicazione. Ciascuna di queste figure utilizza competenze tecniche differenti per eseguire e distribuire correttamente le applicazioni a un cluster.
{: shortdesc}

**Quali sono i compiti principali e le conoscenze tecniche di un amministratore del cluster?** </br>
In qualità di amministratore del cluster, sei responsabile della configurazione, del funzionamento, della protezione e della gestione dell'infrastruttura {{site.data.keyword.Bluemix_notm}} del tuo cluster.  Le attività tipiche includono quanto segue:
- Dimensionare il cluster per fornire sufficiente capacità per i tuoi carichi di lavoro.
- Progettare un cluster per soddisfare gli standard di alta disponibilità, ripristino di emergenza e conformità della tua azienda.
- Proteggere il cluster configurando le autorizzazioni utente e limitando le azioni all'interno del cluster per proteggere le tue risorse di calcolo, la tua rete e i dati.
- Pianificare e gestire le comunicazioni di rete tra i componenti dell'infrastruttura per garantire la sicurezza, la segmentazione e la conformità della rete.
- Pianificare le opzioni di archiviazione persistente per soddisfare i requisiti di protezione e residenza dei dati.

L'amministratore del cluster deve avere un'ampia conoscenza che includa calcolo, rete, archiviazione, sicurezza e conformità. In un'azienda tipica, questa conoscenza è distribuita tra più specialisti, come ad esempio gli ingegneri di sistema, gli amministratori del sistema, gli ingegneri di rete, gli architetti di rete, il responsabile del settore IT o gli specialisti di sicurezza e conformità. Considera di assegnare il ruolo di amministratore del cluster a più persone nella tua azienda in modo da avere la conoscenza necessaria per gestire correttamente il tuo cluster.

**Quali sono i compiti principali e le competenze tecniche di uno sviluppatore di applicazioni?** </br>
In qualità di sviluppatore, progetti, crei, proteggi, distribuisci, verifichi, esegui e monitori le applicazioni inserite nel contenitore e native del cloud in un cluster Kubernetes. Per creare ed eseguire queste applicazioni, devi avere dimestichezza con il concetto di microservizi, con le linee guida dell'[applicazione a 12 fattori](#12factor), con i [principi di Docker e inserimento nei contenitori](https://www.docker.com/) e con le [opzioni di distribuzione Kubernetes](/docs/containers?topic=containers-app#plan_apps) disponibili. Se vuoi distribuire applicazioni senza server, acquisisci dimestichezza con [Knative](/docs/containers?topic=containers-cs_network_planning).

Kubernetes e {{site.data.keyword.containerlong_notm}} forniscono molteplici opzioni di come [esporre un'applicazione e mantenere privata un'applicazione](/docs/containers?topic=containers-cs_network_planning), [aggiungere dell'archiviazione persistente](/docs/containers?topic=containers-storage_planning), [integrare altri servizi](/docs/containers?topic=containers-ibm-3rd-party-integrations) e poter [proteggere i tuoi carichi di lavoro e i dati sensibili](/docs/containers?topic=containers-security#container). Prima di spostare la tua applicazione in un cluster in {{site.data.keyword.containerlong_notm}}, verifica che puoi eseguire la tua applicazione come un'applicazione inserita nel contenitore sul sistema operativo Ubuntu 16.64, 18.64 supportato e che Kubernetes e {{site.data.keyword.containerlong_notm}} forniscano le funzionalità di cui ha bisogno il tuo carico di lavoro.

**Gli amministratori e gli sviluppatori del cluster interagiscono tra loro?** </br>
Sì. Gli amministratori e gli sviluppatori del cluster devono interagire frequentemente in modo che gli amministratori del cluster comprendano i requisiti del carico di lavoro per fornire questa capacità nel cluster e in modo che gli sviluppatori conoscano le limitazioni, le integrazioni e i principi di sicurezza disponibili che devono prendere in considerazione nel loro processo di sviluppo delle applicazioni.

## Dimensionamento del tuo cluster Kubernetes affinché possa supportare il tuo carico di lavoro
{: #sizing}

Il calcolo dei nodi di lavoro necessari nel tuo cluster per supportare il tuo carico di lavoro non è una scienza esatta. Potresti aver bisogno di testare varie configurazioni e adattarle. È positivo che utilizzi {{site.data.keyword.containerlong_notm}}, perché ti consente di aggiungere e rimuovere i nodi di lavoro in base alle domande di carico di lavoro.
{: shortdesc}

Per iniziare a dimensionare il tuo cluster, poniti le seguenti domande.

### Di quante risorse necessita la mia applicazione?
{: #sizing_resources}

Innanzitutto, partiamo dall'utilizzo del carico di lavoro del progetto o esistente.

1.  Calcola l'utilizzo medio di memoria e CPU del tuo carico di lavoro. Ad esempio, potresti accedere a Gestione attività su Windows o eseguire il comando `top` su Mac o Linux. Potresti anche utilizzare un servizio metriche ed eseguire dei report per calcolare l'utilizzo del carico di lavoro.
2.  Prevedi la quantità di richieste che il tuo carico di lavoro dovrà servire, in modo da poter decidere quante repliche delle applicazioni vuoi che gestiscano il carico di lavoro. Ad
esempio, potresti progettare un'istanza dell'applicazione che gestisca 1000 richieste al minuto e prevedere che il tuo carico di lavoro dovrà servire 10000 richieste al minuto. In questo caso, potresti decidere di effettuare 12 repliche dell'applicazione: 10 per gestire la quantità prevista e 2 aggiuntive per eventuali picchi.

### Cosa, oltre la mia applicazione, potrebbe utilizzare le risorse del cluster?
{: #sizing_other}

Aggiungiamo ora delle altre funzioni che potresti utilizzare.



1.  Valuta se la tua applicazione potrebbe eseguire il pull di numerose immagini o di immagini di dimensioni elevate, occupando così le risorse di archiviazione locale nel nodo di lavoro.
2.  Decidi se desideri [integrare i servizi](/docs/containers?topic=containers-supported_integrations#supported_integrations) nel tuo cluster (ad esempio [Helm](/docs/containers?topic=containers-helm#public_helm_install) o [Prometheus ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/coreos/prometheus-operator/tree/master/contrib/kube-prometheus)). Questi servizi integrati e i componenti aggiuntivi eseguono lo spin-up dei pod che utilizzano le risorse del cluster.

### Che tipo di disponibilità desidero per il mio carico di lavoro?
{: #sizing_availability}

Non dimenticare che desideri che il tuo carico di lavoro sia il più possibile attivo!

1.  Pianifica la tua strategia per i [cluster ad alta disponibilità](/docs/containers?topic=containers-ha_clusters#ha_clusters), scegliendo, ad esempio, tra cluster singolo o multizona.
2.  Consulta l'argomento sulle [distribuzioni altamente disponibili](/docs/containers?topic=containers-app#highly_available_apps) per decidere come puoi rendere disponibile la tua applicazione.

### Quanti nodi di lavoro mi occorrono per gestire il mio carico di lavoro?
{: #sizing_workers}

Ora che hai un'idea precisa del tuo carico di lavoro, mappiamone il presunto utilizzo sulle tue configurazioni cluster disponibili.

1.  Stima la capacità massima del nodo di lavoro, che dipende dal tipo di cluster di cui disponi. Evita di sfruttare al massimo la capacità del nodo di lavoro nel caso in cui si verifichi un picco o un altro evento temporaneo.
    *  **Cluster a zona singola**: pianifica almeno 3 nodi di lavoro nel tuo cluster. Ti conviene disporre, all'interno del cluster, di una capacità di memoria e CPU equivalente a un nodo supplementare.
    *  **Cluster multizona**: pianifica almeno 2 nodi di lavoro per zona, per un totale di 6 nodi in 3 zone. Inoltre, pianifica una capacità totale del cluster pari ad almeno il 150% della capacità richiesta da tuo carico di lavoro totale, in modo che se 1 zona diventa inattiva, disponi delle risorse necessarie per mantenere il carico di lavoro.
2.  Adatta la dimensione dell'applicazione e la capacità del nodo di lavoro a uno dei [tipi di nodo di lavoro disponibili](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes). Per visualizzare i tipi disponibili in una zona, esegui `ibmcloud ks machine-types <zone>`.
    *   **Non sovraccaricare i nodi di lavoro**: per evitare che i tuoi pod si contendano la CPU o vengano eseguiti in modo inefficiente, devi sapere di quali risorse necessitano le tue applicazioni, così da poter pianificare il numero di nodi di lavoro di cui hai bisogno. Ad esempio, se le tue applicazioni richiedono una quantità di risorse inferiore a quella disponibile nel nodo di lavoro, puoi limitare il numero di pod che distribuisci su un unico nodo di lavoro. Mantieni il tuo nodo di lavoro a circa il 75% della capacità, al fine di lasciare spazio per eventuali altri pod da pianificare. Se le tue applicazioni richiedono più risorse di quelle disponibili sul tuo nodo di lavoro, utilizza un tipo di nodo di lavoro differente in grado di soddisfare questi requisiti. Sai che i tuoi nodi di lavoro sono sovraccarichi se riportano spesso lo stato `NotReady` o rimuovono pod per mancanza di memoria o di altre risorse.
    *   **Confronto tra nodi di lavoro più grandi e più piccoli**: i nodi più grandi possono essere più convenienti rispetto a quelli più piccoli, soprattutto per i carichi di lavoro progettati per diventare efficienti quando elaborati in una macchina ad alte prestazioni. Tuttavia, se un nodo di lavoro di grandi dimensioni si interrompe, devi essere certo che la capacità del tuo cluster sia sufficiente per ripianificare normalmente tutti i pod del carico di lavoro in altri nodi di lavoro del cluster. I nodi di lavoro più piccoli possono aiutarti a effettuare un ridimensionamento più normalmente.
    *   **Repliche della tua applicazione**: per determinare il numero di nodi di lavoro desiderati, puoi anche valutare quante repliche della tua applicazione vuoi eseguire. Ad esempio, se sai che il tuo carico di lavoro richiede 32 core CPU e prevedi di eseguire 16 repliche della tua applicazione, ogni pod di replica necessita di 2 core CPU. Se desideri eseguire un solo pod dell'applicazione per ciascun nodo di lavoro, puoi ordinare un numero adeguato di nodi di lavoro affinché il tuo tipo di cluster possa supportare questa configurazione.
3.  Esegui test delle prestazioni per continuare a perfezionare il numero di nodi di lavoro necessari nel tuo cluster, con requisiti di latenza, scalabilità, dataset e requisiti del carico di lavoro.
4.  Per i carichi di lavoro che devono essere ampliati o ridimensionati in risposta alle richieste di risorse, configura l'[autoscaler di pod orizzontale](/docs/containers?topic=containers-app#app_scaling) e l'[autoscaler del pool di nodi di lavoro](/docs/containers?topic=containers-ca#ca).

<br />


## Strutturare il tuo ambiente Kubernetes
{: #kube_env}

Il tuo {{site.data.keyword.containerlong_notm}} è collegato a un solo portfolio dell'infrastruttura IBM Cloud (SoftLayer). All'interno del tuo account, puoi creare cluster composti da un master e vari nodi di lavoro. IBM gestisce il master e puoi creare una combinazione di pool di nodi di lavoro che raggruppano singole macchine dello stesso tipo, o con la stessa memoria e le stesse specifiche CPU. All'interno del cluster, puoi organizzare ulteriormente le risorse utilizzando spazi dei nomi ed etichette. Scegli la giusta combinazione di cluster, tipi di macchina e strategie organizzative, in modo da poter garantire che i tuoi team e carichi di lavoro ottengano le risorse di cui hanno bisogno.
{:shortdesc}

### Che tipo di cluster e di macchine devo ottenere?
{: #env_flavors}

**Tipi di cluster**: decidi se desideri una[configurazione cluster a zona singola, multizona o multicluster](/docs/containers?topic=containers-ha_clusters#ha_clusters). I cluster multizona sono disponibili in [tutte e sei le regioni metropolitane {{site.data.keyword.Bluemix_notm}} di tutto il mondo](/docs/containers?topic=containers-regions-and-zones#zones). Inoltre, tieni presente che i nodi di lavoro variano in base alla zona.

**Tipi di nodi di lavoro**: in generale, i tuoi carichi di lavoro intensivi sono più adatti per l'esecuzione su macchine fisiche bare metal, mentre per operazioni di test e sviluppo più convenienti, potresti scegliere macchine virtuali su hardware condiviso o condiviso dedicato. Con i nodi di lavoro bare metal, il tuo cluster ha una velocità di rete pari a 10Gbps e core con hyperthreading che offrono una velocità di elaborazione più elevata. Le macchine virtuali vengono fornite con una velocità di rete pari a 1 Gbps e core standard che non offrono l'hyperthreading. [Controlla l'isolamento e i tipi di macchina disponibili](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes).

### Devo utilizzare più cluster o solo aggiungere più nodi di lavoro a un cluster esistente?
{: #env_multicluster}

Il numero di cluster che crei dipende dal tuo carico di lavoro, dalle politiche e dai regolamenti aziendali e da quello che desideri fare con le risorse di calcolo. Puoi consultare anche le informazioni sulla sicurezza pertinenti riportate in [Isolamento e sicurezza del contenitore](/docs/containers?topic=containers-security#container).

**Più cluster**: per bilanciare i carichi di lavoro tra i cluster, devi configurare [un programma di bilanciamento del carico globale](/docs/containers?topic=containers-ha_clusters#multiple_clusters) e copiare e applicare gli stessi file YAML di configurazione in ognuno. Pertanto, la configurazione con più cluster è generalmente più complessa da gestire, ma può aiutare a raggiungere obbiettivi quali i seguenti.
*  Conformarsi alle politiche di sicurezza che ti richiedono l'isolamento dei carichi di lavoro.
*  Verificare in che modo viene eseguita la tua applicazione in una versione diversa di Kubernetes o di altri software cluster quale Calico.
*  Crea un cluster con la tua applicazione in un'altra regione, per ottenere prestazioni più elevate per gli utenti in tale area geografica.
*  Configurare l'accesso utente a livello di istanza del cluster invece di personalizzare e gestire più politiche RBAC per controllare l'accesso all'interno di un cluster a livello di spazio dei nomi.

**Pochi o singoli cluster**: l'utilizzo di pochi cluster può aiutarti a ridurre gli sforzi operativi e i costi per cluster per le risorse fisse. Invece di creare più cluster, puoi aggiungere pool di nodi di lavoro per differenti tipi di macchina delle risorse di calcolo disponibili per i componenti dell'applicazione e dei servizi che desideri utilizzare. Quando sviluppi l'applicazione, le risorse che quest'ultima utilizza si trovano nella stessa zona, o strettamente connesse in una multizona, cosicché tu possa formulare ipotesi sulla latenza, sulla larghezza di banda o sugli errori correlati. Tuttavia, diventa ancora più importante per te organizzare il tuo cluster utilizzando spazi dei nomi, quote delle risorse ed etichette.

### Come posso configurare le mie risorse all'interno del cluster?
{: #env_resources}

<dl>
<dt>Valuta la capacità del tuo nodo di lavoro.</dt>
  <dd>Per ottenere il massimo dalle prestazioni del tuo nodo di lavoro, valuta quanto segue:
  <ul><li><strong>Stai al passo con la potenza dei tuoi core</strong>: ogni macchina ha un certo numero di core. A seconda del carico di lavoro della tua applicazione, configura un limite per il numero di pod per core, ad esempio 10.</li>
  <li><strong>Evita di sovraccaricare i nodi</strong>: allo stesso modo, solo perché un nodo può contenere più di 100 pod non significa che debba farlo. A seconda del carico di lavoro della tua applicazione, imposta un limite per il numero di pod per nodo, ad esempio 40.</li>
  <li><strong>Non mettere al tappeto la larghezza di banda del tuo cluster</strong>: tieni a mente che la larghezza di banda della rete utilizzata durante il ridimensionamento delle macchine virtuali è di circa 1000 Mbps. Se ti occorrono centinaia di nodi di lavoro in un cluster, dividilo in più cluster con meno nodi o ordina nodi bare metal.</li>
  <li><strong>Predisposizione dei tuoi servizi</strong>: pianifica il numero di servizi di cui hai bisogno per il tuo carico di lavoro prima di effettuare la distribuzione. Le regole di rete e inoltro della porta vengono messe in Iptables. Se prevedi un numero maggiore di servizi, ad esempio più di 5.000 servizi,
suddividi il cluster in più cluster.</li></ul></dd>
<dt>Effettua il provisioning di differenti tipi di macchina per una combinazione di risorse di calcolo.</dt>
  <dd>A tutti piace poter scegliere, giusto? Con {{site.data.keyword.containerlong_notm}}, disponi di [una combinazione di tipi di macchina](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes) distribuibili: dal bare metal per i carichi di lavoro intensivi alle macchine virtuali per una scalabilità rapida. Utilizza etichette o spazi dei nomi per organizzare le distribuzioni nelle tue macchine. Quando crei una distribuzione, limitala in modo che il pod della tua applicazione venga distribuito solo su macchine con la giusta combinazione di risorse. Ad esempio, potresti voler limitare un'applicazione database a una macchina bare metal con una quantità significativa di archiviazione disco locale, quale `md1c.28x512.4x4tb`.</dd>
<dt>Configura più spazi dei nomi, se il cluster è condiviso da più team e progetti.</dt>
  <dd><p>Gli spazi dei nomi sono un po' come un cluster dentro il cluster. Rappresentano un modo per suddividere le risorse del cluster utilizzando [quote delle risorse ![Icona link esterno](../icons/launch-glyph.svg "Icona linkesterno")](https://kubernetes.io/docs/concepts/policy/resource-quotas/) e [limiti predefiniti ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/tasks/administer-cluster/memory-default-namespace/). Quando crei nuovi spazi dei nomi, assicurati di configurare le [Politiche RBAC](/docs/containers?topic=containers-users#rbac) appropriate per controllare l'accesso. Per ulteriori informazioni, vedi [Share a cluster with namespaces ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/tasks/administer-cluster/namespaces/) nella documentazione di Kubernetes.</p>
  <p>Se hai un piccolo cluster, un paio di dozzine di utenti e risorse simili (quali versioni diverse dello stesso software), probabilmente non hai bisogno di più spazi dei nomi. Puoi utilizzare invece le etichette.</p></dd>
<dt>Imposta le quote delle risorse in modo che gli utenti del cluster debbano utilizzare richieste di risorse e limiti</dt>
  <dd>Per garantire che ogni team disponga delle risorse necessarie per distribuire i servizi ed eseguire le applicazioni nel cluster, devi configurare le [quote delle risorse](https://kubernetes.io/docs/concepts/policy/resource-quotas/) per ciascuno spazio dei nomi. Le quote delle risorse determinano i vincoli di distribuzione per uno spazio dei nomi, come il numero di risorse Kubernetes che puoi distribuire e la quantità di CPU e memoria che può essere utilizzata da tali risorse. Dopo che hai impostato una quota, gli utenti devono includere le richieste di risorse e i limiti nelle loro distribuzioni.</dd>
<dt>Organizza i tuoi oggetti Kubernetes con etichette</dt>
  <dd><p>Per organizzare e selezionare le tue risorse Kubernetes, quali `pods` o `nodes`, [utilizza le etichette Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/). Per impostazione predefinita, {{site.data.keyword.containerlong_notm}} applica alcune etichette, tra cui `arch`, `os`, `region`, `zone` e `machine-type`.</p>
  <p>I casi d'uso di esempio per le etichette includono la [limitazione del traffico di rete per i nodi di lavoro edge](/docs/containers?topic=containers-edge), la [distribuzione di un'applicazione su una macchina GPU](/docs/containers?topic=containers-app#gpu_app) e la[limitazione dei carichi di lavoro della tua applicazione![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/) per l'esecuzione sui nodi di lavoro che soddisfano determinati tipi di macchina o capacità SDS, come i nodi di lavoro bare metal. Per visualizzare quali etichette sono già applicate a una risorsa, utilizza il comando <code>kubectl get</code> con l'indicatore <code>-- show-label</code>. Ad esempio:</p>
  <p><pre class="pre"><code>kubectl get node &lt;node_ID&gt; --show-labels</code></pre></p>
  Per applicare etichette ai nodi di lavoro, [crea il tuo pool di nodi di lavoro](/docs/containers?topic=containers-add_workers#add_pool) con le etichette oppure [aggiorna un pool di nodi di lavoro esistente](/docs/containers?topic=containers-add_workers#worker_pool_labels)</dd>
</dl>




<br />


## Come rendere le tue risorse altamente disponibili
{: #kube_ha}

Benché nessun sistema sia del tutto a prova di errore, puoi adottare delle misure per aumentare l'alta disponibilità delle tue applicazioni e dei tuoi servizi in {{site.data.keyword.containerlong_notm}}.
{:shortdesc}

Sono disponibili ulteriori informazioni su come rendere le risorse altamente disponibili.
* [Reduci i potenziali punti di errore](/docs/containers?topic=containers-ha#ha).
* [Crea cluster multizona](/docs/containers?topic=containers-ha_clusters#ha_clusters).
* [Pianifica distribuzioni altamente disponibili](/docs/containers?topic=containers-app#highly_available_apps) che utilizzano funzioni quali le serie di repliche e l'anti-affinità dei pod tra multizona.
* [Esegui contenitori basati su immagini in un registro pubblico basato su cloud](/docs/containers?topic=containers-images).
* [Pianifica l'archiviazione dei dati](/docs/containers?topic=containers-storage_planning#persistent_storage_overview). Soprattutto per i cluster multizona, valuta l'utilizzo di un servizio cloud quale[{{site.data.keyword.cloudant_short_notm}}](/docs/services/Cloudant?topic=cloudant-getting-started#getting-started) o [{{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-about).
* Per i cluster multizona, abilita un [servizio del programma di bilanciamento del carico](/docs/containers?topic=containers-loadbalancer#multi_zone_config) o il [programma di bilanciamento del carico multizona](/docs/containers?topic=containers-ingress#ingress) Ingress per esporre pubblicamente le tue applicazioni.

<br />


## Configurazione del rilevamento di servizi
{: #service_discovery}

Ognuno dei tuoi pod nel tuo cluster Kubernetes ha un indirizzo IP. Tuttavia, quando distribuisci un'applicazione nel tuo cluster non vuoi affidarti all'indirizzo IP del pod per il rilevamento dei servizi e le reti. I pod vengono rimossi e sostituiti frequentemente e dinamicamente. Utilizza invece un servizio Kubernetes, che rappresenta un gruppo di pod e fornisce un punto di ingresso stabile tramite l'indirizzo IP virtuale del servizio, denominato `cluster IP`. Per ulteriori informazioni, consulta la documentazione di Kubernetes sui[Servizi ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/concepts/services-networking/service/#discovering-services).
{:shortdesc}

### Posso personalizzare il provider DNS del cluster Kubernetes?
{: #services_dns}

Quando crei servizi e pod, viene assegnato loro un nome DNS, cosicché i contenitori delle tue applicazioni possano utilizzare l'IP del servizio DNS per risolvere i nomi DNS. Puoi personalizzare il DNS del pod per specificare i server dei nomi, le ricerche e le opzioni dell'elenco di oggetti. Per ulteriori informazioni, vedi [Configurazione del provider DNS del cluster](/docs/containers?topic=containers-cluster_dns#cluster_dns).
{: shortdesc}



### Come posso assicurarmi che i miei servizi siano collegati alle distribuzioni corrette e pronti per l'uso?
{: #services_connected}

Per la maggior parte dei servizi, aggiungi un selettore al file `.yaml` del tuo servizio, cosicché venga applicato ai pod che eseguono le tue applicazioni in base a quell'etichetta. Spesso, al primo avvio dell'applicazione non vuoi che elabori immediatamente delle richieste. Aggiungi un'analisi di disponibilità alla tua distribuzione, in modo che il traffico venga inviato a un pod solo se considerato pronto. Per un esempio di distribuzione con un servizio che utilizza le etichette e imposta un'analisi di disponibilità, vedi questo [NGINX YAML](https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/deploy-apps-clusters/nginx_preferredAntiAffinity.yaml).
{: shortdesc}

A volte, non desideri che il servizio utilizzi un'etichetta. Ad esempio, potresti avere un database esterno o voler puntare il servizio verso un altro servizio in uno spazio dei nomi differente all'interno del cluster. Quando ciò accade, devi aggiungere manualmente un oggetto endpoint e collegarlo al servizio.


### Come posso controllare il traffico di rete tra i servizi eseguiti nel mio cluster?
{: #services_network_traffic}

Per impostazione predefinita, i pod possono comunicare con altri pod nel cluster, ma puoi bloccare il traffico verso determinati pod o spazi dei nomi attraverso le politiche di rete. Inoltre, se esponi la tua applicazione esternamente attraverso una NodePort, un programma di bilanciamento del carico o un servizio Ingress, potresti voler configurare politiche di rete avanzate per bloccare il traffico. In {{site.data.keyword.containerlong_notm}}, puoi utilizzare Calico per gestire le [politiche di rete Kubernetes e Calico per controllare il traffico](/docs/containers?topic=containers-network_policies#network_policies).

Se hai vari microservizi eseguiti su piattaforme, per cui hai bisogno di connettere, gestire e proteggere il traffico di rete, valuta l'utilizzo di uno strumento di rete di servizi quale il [componente aggiuntivo gestito Istio](/docs/containers?topic=containers-istio).

Puoi anche [configurare nodi edge](/docs/containers?topic=containers-edge#edge) per aumentare la sicurezza e l'isolamento del tuo cluster, limitando il carico di lavoro di rete per selezionare i nodi di lavoro.



### Come posso esporre i miei servizi su Internet?
{: #services_expose_apps}

Puoi creare tre tipi di servizi per reti esterne: NodePort, LoadBalancer e Ingress. Per ulteriori informazioni, vedi [Pianificazione dei servizi di rete](/docs/containers?topic=containers-cs_network_planning#external).

Quando pianifichi il numero di oggetti `Service` di cui necessiti nel tuo cluster, ricorda che Kubernetes utilizza `iptables` per gestire le regole di rete e inoltro della porta. Se esegui un numero elevato di servizi nel tuo cluster, ad esempio 5000, ciò potrebbe influire sulle prestazioni.



## Distribuzione dei carichi di lavoro delle applicazioni ai cluster
{: #deployments}

Con Kubernetes, dichiari numerosi tipi di oggetti nei file di configurazione YAML, quali pod, distribuzioni e job. Questi oggetti descrivono cose quali le applicazioni inserite nei contenitori in esecuzione, le risorse che esse utilizzano e le politiche che ne gestiscono il funzionamento per il riavvio, l'aggiornamento, la replica e altro ancora. Per ulteriori informazioni, consulta la documentazione Kubernetes per la [Configurazione delle procedure consigliate ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/concepts/configuration/overview/).
{: shortdesc}

### Pensavo di dover mettere la mia applicazione in un contenitore. Ora cosa c'entrano i pod?
{: #deploy_pods}

Un [pod ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/concepts/workloads/pods/pod/) è la più piccola unità distribuibile gestibile da Kubernetes. Collochi il tuo contenitore (o un gruppo di contenitori) in un pod e utilizzi il file di configurazione del pod per dire al pod come eseguire il contenitore e condividere le risorse con altri pod. Tutti i contenitori che metti in un pod vengono eseguiti in un contesto condiviso, il che significa che condividono la stessa macchina virtuale o fisica.
{: shortdesc}

**Cosa mettere in un contenitore**: quando rifletti sui componenti della tua applicazione, valuta se hanno esigenze significativamente differenti in termini di risorse per cose come la CPU e la memoria. Ci sono componenti che possono essere eseguiti con il massimo delle risorse? È accettabile che vengano disattivati temporaneamente per deviare delle risorse verso altre aree? Ci sono componenti rivolti agli utenti che devono dunque essere sempre attivi? Suddividili in contenitori distinti. Puoi sempre distribuirli sullo stesso pod, in modo che vengano eseguiti insieme in sincrono.

**Cosa mettere in un pod**: non sempre è necessario che i contenitori della tua applicazione si trovino nello stesso pod. Infatti, se hai un componente senza stato e difficile da ridimensionare, come ad esempio un servizio di database, collocalo in un pod differente pianificabile su un nodo di lavoro che dispone di maggiori risorse per la gestione del carico di lavoro. Se i tuoi contenitori funzionano correttamente se eseguiti su nodi di lavoro diversi, utilizza più pod. Se devono trovarsi nella stessa macchina ed essere ridimensionati insieme,
raggruppa i contenitori nello stesso pod.

### Quindi se posso usare un pod, perché ho bisogno di tutti questi tipi di oggetto differenti?
{: #deploy_objects}

È facile creare un file YAML pod. Puoi scriverne uno di poche righe come di seguito indicato.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx
    ports:
    - containerPort: 80
```
{: codeblock}

Poniamo che tu non voglia limitarti a questo. Se il nodo eseguito dal tuo pod si disattiva, la fa anche il tuo pod e non viene ripianificato. Utilizza, invece, una [distribuzione ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) per supportare la ripianificazione del pod, le serie di replica e gli aggiornamenti continui. Creare una distribuzione di base è quasi tanto semplice quanto creare un pod. Invece di definire il contenitore nella stessa `spec`, specifichi `replicas` e `template` nella distribuzione `spec`. Il modello dispone di una propria `spec` per i contenitori al suo interno, come di seguito indicato.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 80
```
{: codeblock}

Puoi continuare ad aggiungere funzioni, come ad esempio l'anti-affinità pod e limiti delle risorse, tutti nello stesso file YAML.

Per una spiegazione più dettagliata delle varie funzioni che puoi aggiungere alla tua distribuzione, dai un'occhiata a [Creazione del file YAML di distribuzione della tua applicazione](/docs/containers?topic=containers-app#app_yaml).
{: tip}

### Come posso organizzare le mie distribuzioni per renderle più facili da aggiornare e gestire?
{: #deploy_organize}

Ora che hai un'idea precisa di cosa includere nella tua distribuzione, potresti chiederti come gestirai tutti questi file YAML differenti. Per non parlare degli oggetti che creano nel tuo ambiente Kubernetes!

Ecco alcuni suggerimenti per organizzare i file YAML di distribuzione:
*  Utilizza un sistema di controllo delle versioni, come ad esempio Git.
*  Raggruppa gli oggetti Kubernetes strettamente correlati in un unico file YAML. Ad esempio, se stai creando una `deployment`, potresti voler aggiungere al file YAML anche il file `service`. Separa gli oggetti con `---` come segue:
    ```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
    ...
    ---
    apiVersion: v1
    kind: Service
    metadata:
    ...
    ```
    {: codeblock}
*  Puoi utilizzare il comando `kubectl apply -f` da applicare a un'intera directory, non solo a un singolo file.
*  Prova il [progetto `kustomize`](/docs/containers?topic=containers-app#kustomize) che puoi utilizzare come ausilio per scrivere, personalizzare e riutilizzare le configurazioni YALM delle tue risorse Kubernetes.

All'interno del file YAML, puoi utilizzare etichette o annotazioni come metadati per gestire le tue distribuzioni.

**Etichette**: le [etichette ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/) sono coppie `key:value` che possono essere collegate a oggetti Kubernetes, quali pod e distribuzioni. Possono essere qualsiasi cosa desideri e sono utili per selezionare gli oggetti in base alle informazioni dell'etichetta. Le etichette forniscono le basi per il raggruppamento degli oggetti. Alcune idee per le etichette:
* `app: nginx`
* `version: v1`
* `env: dev`

**Annotazioni**: [Annotazioni ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/concepts/overview/working-with-objects/annotations/) sono simili alle etichette per il fatto che sono anche coppie `key:value`. Sono migliori per le informazioni non identificative che possono essere sfruttate da strumenti o librerie, come ad esempio l'organizzazione di ulteriori informazioni sull'origine di un oggetto, come utilizzare l'oggetto, i puntatori ai repository di traccia correlati o una politica sull'oggetto. Non selezioni gli oggetti in base alle annotazioni.

### Cos'altro posso fare per preparare la mia applicazione per la distribuzione?
{: #deploy_prep}

Molte cose! Vedi [Preparazione della tua applicazione inserita in un contenitore per l'esecuzione nei cluster](/docs/containers?topic=containers-app#plan_apps). L'argomento include
informazioni su:
*  I tipi di applicazioni che si possono eseguire in Kubernetes, inclusi suggerimenti per applicazioni con stato e senza stato.
*  Migrazione delle applicazioni a Kubernetes.
*  Dimensionamento del tuo cluster in base ai requisiti del tuo carico di lavoro.
*  Configurazione di risorse aggiuntive dell'applicazione quali servizi IBM, archiviazione, registrazione e monitoraggio.
*  Utilizzo di variabili all'interno della tua distribuzione.
*  Controllo dell'accesso alla tua applicazione.

<br />


## Creazione del pacchetto della tua applicazione
{: #packaging}

Se intendi eseguire la tua applicazione in più cluster, ambienti pubblici e privati e persino in più provider cloud, potresti chiederti come puoi fare in modo che la tua strategia di distribuzione funzioni in tutti questi ambienti. Con {{site.data.keyword.Bluemix_notm}} e altri strumenti open source, puoi creare pacchetti della tua applicazione per facilitare l'automazione delle distribuzioni.
{: shortdesc}

<dl>
<dt>Automazione dell'infrastruttura</dt>
  <dd>È possibile utilizzare lo strumento open source [Terraform](/docs/terraform?topic=terraform-getting-started#getting-started) per automatizzare il provisioning dell'infrastruttura {{site.data.keyword.Bluemix_notm}}, inclusi i cluster Kubernetes. Segui questa esercitazione per [pianificare, creare e aggiornare gli ambienti di distribuzione](/docs/tutorials?topic=solution-tutorials-plan-create-update-deployments#plan-create-update-deployments). Dopo aver creato un cluster, puoi anche configurare il [{{site.data.keyword.containerlong_notm}} cluster autoscaler](/docs/containers?topic=containers-ca) in modo che il tuo pool di nodi di lavoro ampli e ridimensioni i nodi di lavoro in risposta alle richieste di risorse del tuo carico di lavoro.</dd>
<dt>Configurazione di una pipeline di fornitura e integrazione continue (CI/CD)</dt>
  <dd>Con i file di configurazione della tua applicazione organizzati in un sistema di gestione del controllo di origine, come Git, puoi creare la tua pipeline per testare e distribuire il codice in diversi ambienti, ad esempio `test` e `prod`. Vedi [questa esercitazione sulla distribuzione continua in Kubernetes](/docs/tutorials?topic=solution-tutorials-continuous-deployment-to-kubernetes#continuous-deployment-to-kubernetes).</dd>
<dt>Creazione del pacchetto dei file di configurazione dell'applicazione</dt>
  <dd>Con il gestore pacchetti Kubernetes [Helm ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://helm.sh/docs/), puoi specificare tutte le risorse Kubernetes di cui necessita la tua applicazione in un grafico Helm. Quindi, puoi utilizzare Helm per creare i file di configurazione YAML e distribuirli nel tuo cluster. Puoi anche[integrare i grafici Helm forniti da {{site.data.keyword.Bluemix_notm}}![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cloud.ibm.com/kubernetes/solutions/helm-charts) per estendere le funzionalità del tuo cluster, ad esempio con un plugin di archiviazione blocchi.<p class="tip">Cerchi solo un modo semplice per creare modelli di file YAML? Alcune persone usano Helm per fare solo questo. Potresti provare altri strumenti della community quale [`ytt`![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://get-ytt.io/).</p></dd>
</dl>

<br />


## Come mantenere aggiornata la tua app
{: #updating}

Ti sei prodigato per preparare la prossima versione della tua applicazione. Puoi utilizzare gli strumenti di aggiornamento di{{site.data.keyword.Bluemix_notm}} e Kubernetes per assicurarti che la tua applicazione venga eseguita in un ambiente cluster sicuro, oltre che per eseguire il rollout delle varie versioni della tua applicazione.
{: shortdesc}

### Come posso far sì che il mio cluster sia sempre supportato?
{: #updating_kube}

Assicurati che il tuo cluster esegua sempre una [versione supportata di Kubernetes](/docs/containers?topic=containers-cs_versions#cs_versions). Quando viene rilasciata una nuova versione secondaria di Kubernetes, le versioni precedenti diventano presto obsolete e diventano non supportate. Per ulteriori informazioni, vedi [Aggiornamento del master Kubernetes](/docs/containers?topic=containers-update#master) e [nodi di lavoro](/docs/containers?topic=containers-update#worker_node).

### Quali strategie di aggiornamento dell'applicazione posso utilizzare?
{: #updating_apps}

Per aggiornare la tua applicazione, puoi scegliere tra varie strategie come di seguito indicato. Puoi iniziare con una distribuzione graduale o con uno switch istantaneo prima di passare a una distribuzione canary più complicata.

<dl>
<dt>Distribuzione graduale</dt>
  <dd>Puoi utilizzare la funzionalità nativa di Kubernetes per creare una distribuzione `v2` e sostituire gradualmente la tua distribuzione `v1` precedente. Questo approccio richiede che la compatibilità delle applicazioni con le versioni precedenti, in modo che le operazioni degli utenti a cui viene fornita la versione dell'applicazione `v2` non riscontrino modifiche improvvise. Per ulteriori informazioni, vedi [Gestione delle distribuzioni graduali per aggiornare le tue applicazioni](/docs/containers?topic=containers-app#app_rolling).</dd>
<dt>Passaggio istantaneo</dt>
  <dd>Denominata anche distribuzione blu-verde: un passaggio istantaneo richiede il doppio delle risorse di calcolo per avere due versioni di un'applicazione in esecuzione contemporaneamente. Con questo approccio, puoi far passare i tuoi utenti alla versione più recente in tempo reale. Assicurati di utilizzare i selettori di etichetta del servizio (ad esempio `version: green` e `version: blue`) per accertarti che le richieste vengano inviate alla versione corretta dell'applicazione. Puoi creare la nuova distribuzione `version: green`, attendere finché non è pronta e infine eliminare la distribuzione `version: blue`. In alternativa, puoi eseguire un [aggiornamento continuo](/docs/containers?topic=containers-app#app_rolling) ma imposti il parametro `maxUnavailable` su `0%` e il parametro `maxSurge` su `100%`.</dd>
<dt>Distribuzione canary o A/B</dt>
  <dd>Si tratta di una strategia di aggiornamento più complessa: la distribuzione canary si verifica quando selezioni una percentuale di utenti quale il 5% e la invii alla nuova versione dell'applicazione. Raccogli le metriche nei tuoi strumenti di registrazione e monitoraggio delle prestazioni della nuova versione dell'applicazione, effettui test A/B, quindi esegui il rollout dell'aggiornamento per più utenti. Come per tutte le distribuzioni, l'etichettatura dell'applicazione (come `version: stable` e `version: canary`) è fondamentale. Per gestire le distribuzioni canary, puoi [installare la rete di servizi del componente aggiuntivo Istio gestito](/docs/containers?topic=containers-istio#istio), [configurare il monitoraggio per il tuo cluster](/docs/services/Monitoring-with-Sysdig/tutorials?topic=Sysdig-kubernetes_cluster#kubernetes_cluster) e utilizzare quindi la rete di servizi Istio per il test A/B come descritto [in questo post del blog ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://sysdig.com/blog/monitor-istio/). In alternativa, utilizza Knative per le distribuzioni canary.</dd>
</dl>

<br />


## Monitoraggio delle prestazioni del tuo cluster
{: #monitoring_health}

Con la registrazione e il monitoraggio efficaci del tuo cluster e delle applicazioni, puoi comprendere meglio il tuo ambiente allo scopo di ottimizzare l'utilizzo delle risorse e risolvere i problemi che potrebbero presentarsi. Per configurare le soluzioni di registrazione e monitoraggio per il tuo cluster, vedi [Registrazione e monitoraggio](/docs/containers?topic=containers-health#health).
{: shortdesc}

Mentre configuri la tua registrazione e il tuo monitoraggio, rifletti sui seguenti punti.

<dl>
<dt>Raccogli i log e le metriche per determinare lo stato di integrità del cluster</dt>
  <dd>Kubernetes include un server di metriche che aiuta a determinare le prestazioni di base a livello di cluster. Puoi riesaminare queste metriche nel tuo [dashboard Kubernetes](/docs/containers?topic=containers-app#cli_dashboard) o in un terminale eseguendo i comandi `kubectl top (pods | nodes)` . Puoi includere questi comandi nella tua automazione.<br><br>
  Inoltra i log a uno strumento di analisi dei log, in modo da poterli analizzare in un secondo momento. Definisci la verbosità e il livello dei log che vuoi registrare per evitare di archiviare più log del necessario. I log possono consumare rapidamente una grossa quantità di memoria, il che può influire sulle prestazioni della tua applicazione e rendere più difficile l'analisi del log.</dd>
<dt>Testa le prestazioni dell'applicazione</dt>
  <dd>Dopo aver configurato la registrazione e il monitoraggio, esegui i test sulle prestazioni. In un ambiente di test, crea appositamente una serie di scenari non ideali, quale l'eliminazione di tutti i nodi di lavoro in una zona, per replicare un errore di zona. Esamina i log e le metriche per verificare come viene ripristinata la tua applicazione.</dd>
<dt>Prepara i controlli</dt>
  <dd>Oltre ai log delle applicazioni e alle metriche dei cluster, vuoi configurare la traccia delle attività in modo da avere un record verificabile di chi ha eseguito quali azioni cluster e Kubernetes. Per ulteriori informazioni, vedi [{{site.data.keyword.cloudaccesstrailshort}}](/docs/containers?topic=containers-at_events#at_events).</dd>
</dl>
