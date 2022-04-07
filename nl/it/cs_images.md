---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-04"

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



# Creazione dei contenitori dalle immagini
{: #images}

Un'immagine Docker è la base per ogni contenitore che crei con {{site.data.keyword.containerlong}}.
{:shortdesc}

Un'immagine viene creata da un
Dockerfile, che è un file che contiene le istruzioni per creare l'immagine. Nelle sue istruzioni, un Dockerfile potrebbe
fare riferimento alle risorse di build che vengono memorizzate separatamente, quali ad esempio un'applicazione, la configurazione
dell'applicazione e le relative dipendenze.

## Pianificazione dei registri di immagini
{: #planning_images}

Le immagini normalmente vengono archiviate in un registro ed è possibile accedervi pubblicamente (registro pubblico)
o configurarle con un accesso limitato a un piccolo gruppo di utenti (registro privato).
{:shortdesc}

I registri pubblici,
come Docker Hub possono essere utilizzati come un'introduzione di Docker e Kubernetes per creare la tua prima
applicazione inserita in un contenitore in un cluster. Nel caso però di applicazioni aziendali, utilizza un registro privato,
come quello fornito in {{site.data.keyword.registryshort_notm}}, per proteggere le tue immagini dall'essere utilizzate e modificate da utenti non autorizzati. I registri privati
devono essere configurati dall'amministratore del cluster per assicurare che le credenziali di accesso al registro privato
siano disponibili agli utenti del cluster.


Puoi utilizzare più registri con {{site.data.keyword.containerlong_notm}} per distribuire le applicazioni al tuo cluster.

|Registro|Descrizione|Vantaggi|
|--------|-----------|-------|
|[{{site.data.keyword.registryshort_notm}}](/docs/services/Registry?topic=registry-getting-started#getting-started)|Con questa opzione, puoi configurare il tuo proprio repository delle immagini Docker protetto in {{site.data.keyword.registryshort_notm}}
dove puoi archiviare e condividere le immagini in sicurezza tra gli utenti del cluster.|<ul><li>Gestisci l'accesso alle immagini nel tuo account.</li><li>Utilizza le applicazioni di esempio e le immagini fornite da
{{site.data.keyword.IBM_notm}}, come {{site.data.keyword.IBM_notm}} Liberty, come immagine principale e aggiungi il tuo proprio codice
ad esse.</li><li>Scansione automatica delle immagini per vulnerabilità potenziali dal Controllo vulnerabilità, incluse
le raccomandazioni di correzione specifiche del sistema operativo.</li></ul>|
|Ogni altro registro privato|Connetti qualsiasi registro privato esistente al tuo cluster creando un [segreto di pull dell'immagine ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/concepts/containers/images/). Il segreto viene utilizzato
per salvare in modo sicuro le tue credenziali e il tuo URL del registro in un segreto Kubernetes.|<ul><li>Utilizza i registri privati esistenti indipendentemente dalle loro origini (Docker Hub, registri di proprietà dell'organizzazione
o altri registri cloud privati).</li></ul>|
|[Docker Hub pubblico![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://hub.docker.com/){: #dockerhub}|Utilizza questa opzione per utilizzare le immagini pubbliche esistenti da Docker Hub direttamente nella tua [distribuzione Kubernetes![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) quando non è necessaria alcuna modifica al Dockerfile. <p>**Nota:** tieni presente che questa opzione non rispetta i requisiti di sicurezza della tua organizzazione,
come la gestione dell'accesso, la scansione di vulnerabilità o la privacy dell'applicazione.</p>|<ul><li>Non è necessaria alcuna ulteriore configurazione per il tuo cluster.</li><li>Include varie applicazioni open source.</li></ul>|
{: caption="Opzioni del registro delle immagini privato e pubblico" caption-side="top"}

Dopo che hai configurato un registro immagini, gli utenti del cluster possono utilizzare le immagini per distribuire le applicazioni nel cluster.

Ulteriori informazioni sulla [protezione delle tue informazioni personali](/docs/containers?topic=containers-security#pi) quando utilizzi le immagini del contenitore.

<br />


## Configurazione dei contenuti attendibili per le immagini di contenitore
{: #trusted_images}

Puoi creare dei contenitori dalle immagini attendibili che sono firmate e memorizzate in {{site.data.keyword.registryshort_notm}} e impedire le distribuzioni da immagini non firmate o vulnerabili.
{:shortdesc}

1.  [Firma le immagini per i contenuti attendibili](/docs/services/Registry?topic=registry-registry_trustedcontent#registry_trustedcontent). Una volta configurata l'attendibilità delle tue immagini, puoi gestire i contenuti attendibili e i firmatari che possono inserire le immagini nel tuo registro.
2.  Per applicare una politica in cui possono essere utilizzate solo immagini firmate per creare i contenitori nel tuo cluster, [aggiungi Container Image Security Enforcement (beta)](/docs/services/Registry?topic=registry-security_enforce#security_enforce).
3.  Distribuisci la tua applicazione.
    1. [Distribuisci nello spazio dei nomi Kubernetes `predefinito`](#namespace).
    2. [Distribuisci in uno spazio dei nomi Kubernetes diverso oppure da {{site.data.keyword.Bluemix_notm}} una regione o da un account](#other) diversi.

<br />


## Distribuzione dei contenitori da un'immagine {{site.data.keyword.registryshort_notm}} nello spazio dei nomi Kubernetes `predefinito`
{: #namespace}

Puoi distribuire i contenitori al tuo cluster da un'immagine pubblica fornita da IBM o da un'immagine privata memorizzata nel tuo spazio dei nomi {{site.data.keyword.registryshort_notm}}. Per ulteriori informazioni sul modo in cui il tuo cluster accede alle immagini del registro, vedi [Informazioni su come il tuo cluster è autorizzato a estrarre immagini da {{site.data.keyword.registrylong_notm}}](#cluster_registry_auth).
{:shortdesc}

Prima di iniziare:
1. [Configura uno spazio dei nomi in {{site.data.keyword.registryshort_notm}} e inserisci le immagini in questo spazio dei nomi](/docs/services/Registry?topic=registry-getting-started#gs_registry_namespace_add).
2. [Crea un
cluster](/docs/containers?topic=containers-clusters#clusters_ui).
3. Se disponi di un cluster esistente creato prima del **25 febbraio 2019**, [aggiorna il tuo cluster per utilizzare l'`imagePullSecret` della chiave API](#imagePullSecret_migrate_api_key).
4. [Accedi al tuo account. Se applicabile, specifica il gruppo di risorse appropriato. Imposta il contesto per il tuo cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

Per distribuire un contenitore nello spazio dei nomi **predefinito** del tuo cluster:

1.  Crea un file di configurazione di distribuzione denominato `mydeployment.yaml`.
2.  Definisci la distribuzione e l'immagine da utilizzare dal tuo spazio dei nomi in {{site.data.keyword.registryshort_notm}}.

    ```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: <app_name>-deployment
    spec:
      replicas: 3
      selector:
        matchLabels:
          app: <app_name>
      template:
        metadata:
          labels:
            app: <app_name>
        spec:
          containers:
          - name: <app_name>
            image: <region>.icr.io/<namespace>/<my_image>:<tag>
    ```
    {: codeblock}

    Sostituisci le variabili dell'URL dell'immagine con le informazioni relative alla tua immagine:
    *  **`<app_name>`**: il nome della tua applicazione.
    *  **`<region>`**: l'endpoint dell'API {{site.data.keyword.registryshort_notm}} della regione per il dominio del registro. Per elencare il dominio per la regione in cui hai effettuato l'accesso, esegui `ibmcloud cr api`.
    *  **`<namespace>`**: lo spazio dei nomi del registro. Per ottenere le informazioni sul tuo spazio dei nomi, esegui `ibmcloud cr namespace-list`.
    *  **`<my_image>:<tag>`**: l'immagine e la tag che desideri utilizzare per creare il contenitore. Per ottenere le immagini disponibili nel tuo registro, esegui `ibmcloud cr images`.

3.  Crea la distribuzione nel tuo cluster.

    ```
    kubectl apply -f mydeployment.yaml
    ```
    {: pre}

<br />


## Descrizione di come autorizzare il tuo cluster ad eseguire il pull delle immagini da un registro
{: #cluster_registry_auth}

Per eseguire il pull di immagini da un registro, il tuo cluster {{site.data.keyword.containerlong_notm}} utilizza un tipo speciale di segreto Kubernetes, un `imagePullSecret`. Questo segreto di pull dell'immagine archivia le credenziali per accedere a un registro del contenitore. Il registro del contenitore può essere il tuo spazio dei nomi in {{site.data.keyword.registrylong_notm}}, uno spazio dei nomi in {{site.data.keyword.registrylong_notm}} che appartiene a un account {{site.data.keyword.Bluemix_notm}} differente o qualsiasi altro registro privato, come Docker. Il tuo cluster è configurato per eseguire il pull delle immagini dal tuo spazio dei nomi {{site.data.keyword.registrylong_notm}} e distribuisce contenitori da queste immagini allo spazio dei nomi Kubernetes `default` del tuo cluster. Se hai bisogno di eseguire il pull di immagini in altri spazi dei nomi Kubernetes del cluster o altri registri, devi configurare il segreto di pull dell'immagine.
{:shortdesc}

**Qual'è la configurazione del mio cluster per l'esecuzione del pull delle immagini dallo spazio dei nomi Kubernetes `default`?**<br>
Quando crei un cluster, tale cluster dispone di un ID del servizio {{site.data.keyword.Bluemix_notm}} IAM a cui viene fornita una politica di ruolo di accesso al servizio IAM **Lettore** per {{site.data.keyword.registrylong_notm}}. Le credenziali dell'ID del servizio vengono rappresentate in una chiave API senza scadenza che viene memorizzata nei segreti di pull dell'immagine nel tuo cluster. I segreti di pull dell'immagine vengono aggiunti allo spazio dei nomi Kubernetes di `default` e all'elenco di segreti nell'account di servizio `default` per questo spazio dei nomi. Utilizzando i segreti di pull dell'immagine, le tue distribuzioni possono estrarre (accesso in sola lettura) le immagini contenute nel tuo [registro globale e regionale](/docs/services/Registry?topic=registry-registry_overview#registry_regions) per creare contenitori nello spazio dei nomi Kubernetes `default`. Il registro globale archivia in modo sicuro le immagini fornite da IBM pubbliche a cui puoi fare riferimento nelle tue distribuzioni invece di avere riferimenti differenti per le immagini archiviate in ogni registro regionale. Il registro regionale archivia in modo sicuro le tue proprie immagini Docker private.

**Posso limitare l'accesso pull a un determinato registro regionale?**<br>
Sì, puoi [modificare la politica IAM esistente dell'ID servizio](/docs/iam?topic=iam-serviceidpolicy#access_edit) che limita il
ruolo di accesso al servizio **Lettore** a tale registro locale o a una risorsa di registro, come ad esempio uno spazio dei nomi. Prima di poter personalizzare le politiche IAM del registro, devi [abilitare le politiche {{site.data.keyword.Bluemix_notm}} IAM per {{site.data.keyword.registrylong_notm}}](/docs/services/Registry?topic=registry-user#existing_users).

  Vuoi rendere le tue credenziali del registro ancora più sicure? Chiedi al tuo amministratore cluster di [abilitare {{site.data.keyword.keymanagementservicefull}}](/docs/containers?topic=containers-encryption#keyprotect) nel tuo cluster per crittografare i segreti Kubernetes nel cluster, come ad esempio `imagePullSecret` che memorizza le tue credenziali del registro.
  {: tip}

**Posso eseguire il pull delle immagini in spazi dei nomi Kubernetes diversi da quello `default`?**<br>
Non per impostazione predefinita. Utilizzando la configurazione predefinita del cluster, puoi distribuire contenitori da qualsiasi immagine archiviata nel tuo spazio dei nomi {{site.data.keyword.registrylong_notm}} nello spazio dei nomi Kubernetes `default` del tuo cluster. Per utilizzare queste immagini in altri spazi dei nomi Kubernetes o in altri account {{site.data.keyword.Bluemix_notm}}, [puoi scegliere di copiare o creare il tuo segreto di pull dell'immagine](#other).

**Posso eseguire il pull di immagini da un account {{site.data.keyword.Bluemix_notm}} differente?**<br>
Sì, crea una chiave API nell'account {{site.data.keyword.Bluemix_notm}} che desideri utilizzare. Quindi, crea un segreto di pull dell'immagine che archivi le credenziali della chiave API in ciascun cluster e spazio dei nomi del cluster da cui desideri eseguire il pull. [Segui questo esempio che utilizza una chiave API
ID servizio autorizzato](#other_registry_accounts).

Per utilizzare un registro non {{site.data.keyword.Bluemix_notm}}, come Docker, vedi [Accesso alle immagini memorizzate in altri registri privati](#private_images).

**La chiave API deve essere destinata a un ID servizio? Cosa succede se raggiungo il limite di ID servizio per il mio account?**<br>
La configurazione predefinita del cluster crea un ID servizio per memorizzare le credenziali della chiave API {{site.data.keyword.Bluemix_notm}} IAM nel segreto di pull dell'immagine. Tuttavia, puoi anche creare una chiave API per un singolo utente e archiviare le credenziali in un segreto di pull dell'immagine. Se raggiungi il [limite IAM per gli ID servizio](/docs/iam?topic=iam-iam_limits#iam_limits), il tuo cluster viene creato senza l'ID servizio e il segreto di pull dell'immagine non può eseguire il pull delle immagini dai domini di registro `icr.io` per impostazione predefinita. Devi[creare il tuo segreto di pull dell'immagine](#other_registry_accounts), ma utilizzando una chiave API per un singolo utente, come un ID funzionale, e non un ID servizio {{site.data.keyword.Bluemix_notm}} IAM.

**Il segreto di pull dell'immagine del tuo cluster utilizza un token di registro. Il token funziona ancora?**<br>

Il precedente metodo di autorizzazione dell'accesso cluster a {{site.data.keyword.registrylong_notm}} creando automaticamente un [token](/docs/services/Registry?topic=registry-registry_access#registry_tokens) e memorizzando tale token in un segreto di pull dell'immagine è supportato ma obsoleto.
{: deprecated}

I token autorizzano l'accesso ai domini di registro `registry.bluemix.net` obsoleti, mentre le chiavi API autorizzano l'accesso ai domini di registro `icr.io`. Durante il periodo di transizione dall'autenticazione basata su token all'autenticazione basata su chiave API, per un certo periodo vengono creati sia segreti di pull dell'immagine basati su token che segreti di pull dell'immagine basati su chiave API. Con i segreti di pull dell'immagine basati su token e su chiave API, il tuo cluster può eseguire il pull delle immagini dai domini `registry.bluemix.net` o `icr.io` nello spazio dei nomi Kubernetes `default`.

Prima che i token obsoleti e i domini `registry.bluemix.net` non siano più supportati, aggiorna i segreti di pull dell'immagine del tuo cluster, in modo da utilizzare il metodo dello [spazio dei nomi Kubernetes `default` ](#imagePullSecret_migrate_api_key) e [qualsiasi altro spazio dei nomi o account](#other) utilizzabile. Quindi, aggiorna le tue distribuzioni in modo da eseguire il pull dai domini di registro `icr.io`.

**Dopo che ho copiato o creato un segreto di pull dell'immagine in un altro spazio dei nomi Kubernetes, ho finito?**<br>
Non proprio. I tuoi contenitori devono essere autorizzati ad eseguire il pull delle immagini utilizzando il segreto che hai creato. Puoi aggiungere il segreto di pull delle immagini all'account del servizio per lo spazio dei nomi oppure fare riferimento al segreto in ciascuna distribuzione. Per le istruzioni, vedi [Utilizzo del segreto di pull dell'immagine per la distribuzione dei contenitori](/docs/containers?topic=containers-images#use_imagePullSecret).

<br />


## Aggiornamento dei cluster esistenti per utilizzare il segreto di pull dell'immagine della chiave API
{: #imagePullSecret_migrate_api_key}

I nuovi cluster {{site.data.keyword.containerlong_notm}} memorizzano una chiave API in [un segreto di pull dell'immagine per autorizzare l'accesso a {{site.data.keyword.registrylong_notm}}](#cluster_registry_auth). Con questi segreti di pull dell'immagine, puoi distribuire i contenitori dalle immagini che sono memorizzate nei domini del registro `icr.io`. Per i cluster creati prima del **25 febbraio 2019**, devi aggiornare il tuo cluster per memorizzare una chiave API anziché un token del registro nel segreto di pull dell'immagine.
{: shortdesc}

**Prima di iniziare**:
*   [Accedi al tuo account. Se applicabile, specifica il gruppo di risorse appropriato. Imposta il contesto per il tuo cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
*   Assicurati di disporre delle seguenti autorizzazioni:
    *   Ruolo della piattaforma {{site.data.keyword.Bluemix_notm}} IAM **Operatore o Amministratore** per {{site.data.keyword.containerlong_notm}}. Il proprietario dell'account può assegnarti il ruolo eseguendo:
        ```
        ibmcloud iam user-policy-create <your_user_email> --service-name containers-kubernetes --roles Administrator,Operator
        ```
        {: pre}
    *   Ruolo della piattaforma {{site.data.keyword.Bluemix_notm}} IAM **Amministratore** per {{site.data.keyword.registrylong_notm}}, in tutte le regioni e i gruppi di risorse. Il proprietario dell'account può assegnarti il ruolo eseguendo:
        ```
        ibmcloud iam user-policy-create <your_user_email> --service-name container-registry --roles Administrator
        ```
        {: pre}

**Per aggiornare il segreto di pull dell'immagine del tuo cluster nello spazio dei nomi Kubernetes `default`**:
1.  Ottieni il tuo ID cluster.
    ```
    ibmcloud ks clusters
    ```
    {: pre}
2.  Esegui questo comando per creare un ID del servizio per il cluster, assegnare all'ID del servizio un ruolo del servizio IAM **Lettore** per {{site.data.keyword.registrylong_notm}}, creare una chiave API per rappresentare le credenziali dell'ID del servizio e memorizzare la chiave API in un segreto di pull dell'immagine Kubernetes all'interno del cluster. Il segreto di pull dell'immagine si trova nello spazio dei nomi Kubernetes di `default`.
    ```
    ibmcloud ks cluster-pull-secret-apply --cluster <cluster_name_or_ID>
    ```
    {: pre}

    Quando esegui questo comando, viene avviata la creazione delle credenziali IAM e dei segreti di pull dell'immagine il cui completamento potrebbe richiedere alcuni minuti. Non puoi distribuire contenitori che estraggono un'immagine dai domini `icr.io` di {{site.data.keyword.registrylong_notm}} finché non vengono creati i segreti di pull dell'immagine.
    {: important}

3.  Verifica che i segreti di pull dell'immagine siano stati creati nel tuo cluster. Nota che hai un segreto di pull dell'immagine separato per ogni regione {{site.data.keyword.registrylong_notm}}.
    ```
    kubectl get secrets
    ```
    {: pre}
    Output di esempio:
    ```
    default-us-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-uk-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-de-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-au-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-jp-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-icr-io                             kubernetes.io/dockerconfigjson        1         16d
    ```
    {: screen}
4.  Aggiorna le tue distribuzioni del contenitore per estrarre le immagini dal nome di dominio `icr.io`.
5.  Facoltativo: se hai un firewall, assicurati di [consentire il traffico di rete in uscita alle sottoreti del registro](/docs/containers?topic=containers-firewall#firewall_outbound) per i domini che utilizzi.

**Operazioni successive**
*   Per estrarre le immagini in spazi dei nomi Kubernetes diversi da quello di `default` o da altri account {{site.data.keyword.Bluemix_notm}}, [copia o crea un altro segreto di pull dell'immagine](/docs/containers?topic=containers-images#other).
*   Per limitare l'accesso del segreto di pull dell'immagine a determinate risorse del registro come spazi dei nomi o regioni:
    1.  Assicurati che le [politiche {{site.data.keyword.Bluemix_notm}} IAM per {{site.data.keyword.registrylong_notm}} siano abilitate](/docs/services/Registry?topic=registry-user#existing_users).
    2.  [Modifica le politiche {{site.data.keyword.Bluemix_notm}} IAM](/docs/iam?topic=iam-serviceidpolicy#access_edit) per l'ID del servizio o [crea un altro segreto di pull dell'immagine](/docs/containers?topic=containers-images#other_registry_accounts).

<br />


## Utilizzo di un segreto di pull dell'immagine per accedere ad altri spazi dei nomi Kubernetes del cluster, ad altri account {{site.data.keyword.Bluemix_notm}} o a registri privati esterni
{: #other}

Configura il tuo segreto di pull dell'immagine del tuo cluster per distribuire contenitori in spazi dei nomi Kubernetes diversi da`default`, utilizzare immagini archiviate in altri account {{site.data.keyword.Bluemix_notm}} o utilizzare le immagini archiviate in registri privati esterni. Inoltre, potresti creare il tuo segreto di pull dell'immagine per applicare politiche di accesso IAM che limitano le autorizzazioni a specifici spazi dei nomi delle immagini di registro o azioni (ad esempio, `push` o `pull`).
{:shortdesc}

Dopo che hai creato il segreto di pull dell'immagine, i tuoi contenitori devono utilizzare il segreto per essere autorizzati a eseguire il pull di un'immagine dal registro. Puoi aggiungere il segreto di pull delle immagini all'account del servizio per lo spazio dei nomi oppure fare riferimento al segreto in ciascuna distribuzione. Per le istruzioni, vedi [Utilizzo del segreto di pull dell'immagine per la distribuzione dei contenitori](/docs/containers?topic=containers-images#use_imagePullSecret).

I segreti di pull dell'immagine sono validi solo per gli spazi dei nomi Kubernetes per i quali sono stati creati. Ripeti questa procedura
per ogni spazio dei nomi in cui vuoi distribuire i contenitori. Le immagini da [DockerHub](#dockerhub) non richiedono segreti di pull dell'immagine.
{: tip}

Prima di iniziare:

1.  [Configura uno spazio dei nomi in {{site.data.keyword.registryshort_notm}} e inserisci le immagini in questo spazio dei nomi](/docs/services/Registry?topic=registry-getting-started#gs_registry_namespace_add).
2.  [Crea un
cluster](/docs/containers?topic=containers-clusters#clusters_ui).
3.  Se disponi di un cluster esistente creato prima del **25 febbraio 2019**, [aggiorna il tuo cluster per utilizzare il segreto di pull dell'immagine basato su chiave API](#imagePullSecret_migrate_api_key).
4.  [Accedi al tuo account. Se applicabile, specifica il gruppo di risorse appropriato. Imposta il contesto per il tuo cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

<br/>
Per utilizzare il tuo proprio segreto di pull dell'immagine, scegli tra le seguenti opzioni:
- [Copia il segreto di pull dell'immagine](#copy_imagePullSecret) dallo spazio dei nomi Kubernetes predefinito ad altri spazi dei nomi nel tuo cluster.
- [Crea nuove credenziali della chiave API IAM e archiviale in un segreto di pull dell'immagine](#other_registry_accounts) per accedere alle immagini in altri account {{site.data.keyword.Bluemix_notm}} o applicare le politiche IAM che limitano l'accesso a determinati domini di registro o spazi dei nomi.
- [Crea un segreto di pull dell'immagine per accedere alle immagini in registri privati esterni](#private_images).

<br/>
Se nel tuo spazio dei nomi hai già creato un segreto di pull dell'immagine che desideri utilizzare nella tua distribuzione, vedi [Distribuzione dei contenitori utilizzando il `segreto di pull dell'immagine` creato](#use_imagePullSecret).

### Copia di un segreto di pull dell'immagine esistente
{: #copy_imagePullSecret}

Puoi copiare un segreto di pull dell'immagine, come quello che viene creato automaticamente per lo spazio dei nomi Kubernetes `default`, in altri spazi dei nomi del tuo cluster. Se desideri utilizzare credenziali delle chiavi API {{site.data.keyword.Bluemix_notm}} IAM differenti per questo spazio dei nomi, ad esempio per limitare l'accesso a spazi dei nomi specifici o eseguire il pull delle immagini da altri account {{site.data.keyword.Bluemix_notm}}, [crea invece un segreto di pull dell'immagine](#other_registry_accounts).
{: shortdesc}

1.  Elenca gli spazi dei nomi Kubernetes disponibili nel tuo cluster o crea uno spazio dei nomi da utilizzare.
    ```
    kubectl get namespaces
    ```
    {: pre}

    Output di esempio:
    ```
    default          Active    79d
   ibm-cert-store   Active    79d
   ibm-system       Active    79d
   istio-system     Active    34d
   kube-public      Active    79d
   kube-system      Active    79d
    ```
    {: screen}

    Per creare uno spazio dei nomi:
    ```
    kubectl create namespace <namespace_name>
    ```
    {: pre}
2.  Elenca i segreti di pull dell'immagine esistenti nello spazio dei nomi Kubernetes di `default` per {{site.data.keyword.registrylong_notm}}.
    ```
    kubectl get secrets -n default | grep icr
    ```
    {: pre}
    Output di esempio:
    ```
    default-us-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-uk-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-de-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-au-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-jp-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-icr-io                             kubernetes.io/dockerconfigjson        1         16d
    ```
    {: screen}
3.  Copia ogni segreto di pull dell'immagine dallo spazio dei nomi di `default` allo spazio dei nomi di tua scelta. I nuovi segreti
di pull dell'immagine sono denominati `<namespace_name>-icr-<region>-io`.
    ```
    kubectl get secret default-us-icr-io -o yaml | sed 's/default/<new-namespace>/g' | kubectl -n <new-namespace> create -f -
    ```
    {: pre}
    ```
    kubectl get secret default-uk-icr-io -o yaml | sed 's/default/<new-namespace>/g' | kubectl -n <new-namespace> create -f -
    ```
    {: pre}
    ```
    kubectl get secret default-de-icr-io -o yaml | sed 's/default/<new-namespace>/g' | kubectl -n <new-namespace> create -f -
    ```
    {: pre}
    ```
    kubectl get secret default-au-icr-io -o yaml | sed 's/default/<new-namespace>/g' | kubectl -n <new-namespace> create -f -
    ```
    {: pre}
    ```
    kubectl get secret default-jp-icr-io -o yaml | sed 's/default/<new-namespace>/g' | kubectl -n <new-namespace> create -f -
    ```
    {: pre}
    ```
    kubectl get secret default-icr-io -o yaml | sed 's/default/<new-namespace>/g' | kubectl -n <new-namespace> create -f -
    ```
    {: pre}
4.  Verifica che i segreti siano stati creati correttamente.
    ```
    kubectl get secrets -n <namespace_name>
    ```
    {: pre}
5.  [Aggiungi il segreto di pull dell'immagine a un account di servizio Kubernetes in modo che qualsiasi pod nello spazio dei nomi possa utilizzare tale segreto quando distribuisci un contenitore](#use_imagePullSecret).

### Creazione di un segreto di pull dell'immagine con credenziali chiavi API IAM differenti, per un maggior controllo o accedere a immagini in altri account {{site.data.keyword.Bluemix_notm}}
{: #other_registry_accounts}

Puoi assegnare politiche di accesso {{site.data.keyword.Bluemix_notm}} IAM a utenti o a un ID servizio per limitare le autorizzazioni a specifici spazi dei nomi delle immagini di registro o azioni (quali `push` o `pull`).
Quindi, crea una chiave API e archivia le credenziali del registro in un segreto di pull dell'immagine per il tuo cluster.
{: shortdesc}

Ad esempio, per accedere alle immagini in altri account {{site.data.keyword.Bluemix_notm}}, crea una chiave API per archiviare le {{site.data.keyword.registryshort_notm}} credenziali di un utente o di un ID servizio in tale account. Quindi, nell'account del tuo cluster, salva le credenziali della chiave API in un segreto di pull dell'immagine per ciascun cluster e spazio dei nomi del cluster.

La seguente procedura crea una chiave API che archivia le credenziali di un ID servizio {{site.data.keyword.Bluemix_notm}} IAM. Anziché utilizzare un ID del servizio, potresti voler creare una chiave API per un ID utente che abbia una politica di accesso al servizio {{site.data.keyword.Bluemix_notm}} IAM per {{site.data.keyword.registryshort_notm}}. Tuttavia, assicurati che l'utente sia un ID funzionale o di avere un piano nel caso in cui l'utente se ne vada, in modo che il cluster possa ancora accedere al registro.
{: note}

1.  Elenca gli spazi dei nomi Kubernetes disponibili nel tuo cluster o crea uno spazio dei nomi da utilizzare dove desideri distribuire i contenitori dalle immagini del registro.
    ```
    kubectl get namespaces
    ```
    {: pre}

    Output di esempio:
    ```
    default          Active    79d
   ibm-cert-store   Active    79d
   ibm-system       Active    79d
   istio-system     Active    34d
   kube-public      Active    79d
   kube-system      Active    79d
    ```
    {: screen}

    Per creare uno spazio dei nomi:
    ```
    kubectl create namespace <namespace_name>
    ```
    {: pre}
2.  Crea un ID del servizio {{site.data.keyword.Bluemix_notm}} IAM per il tuo cluster che viene utilizzato per le politiche IAM e per le credenziali della chiave API nel segreto di pull dell'immagine. Assicurati di fornire all'ID del servizio una descrizione che ti aiuti a richiamare l'ID in un secondo momento, ad esempio includendo il nome del cluster e dello spazio dei nomi.
    ```
    ibmcloud iam service-id-create <cluster_name>-<kube_namespace>-id --description "Service ID for IBM Cloud Container Registry in Kubernetes cluster <cluster_name> namespace <kube_namespace>"
    ```
    {: pre}
3.  Crea una politica {{site.data.keyword.Bluemix_notm}} IAM personalizzata per l'ID del servizio cluster che conceda l'accesso a {{site.data.keyword.registryshort_notm}}.
    ```
    ibmcloud iam service-policy-create <cluster_service_ID> --roles <service_access_role> --service-name container-registry [--region <IAM_region>] [--resource-type namespace --resource <registry_namespace>]
    ```
    {: pre}
    <table>
    <caption>Descrizione dei componenti di questo comando</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dei componenti di questo comando</th>
    </thead>
    <tbody>
    <tr>
    <td><code><em>&lt;cluster_service_ID&gt;</em></code></td>
    <td>Obbligatoria. Sostituisci con l'ID servizio `<cluster_name>-<kube_namespace>-id` che hai creato in precedenza per il tuo cluster
Kubernetes.</td>
    </tr>
    <tr>
    <td><code>--service-name <em>container-registry</em></code></td>
    <td>Obbligatoria. Immetti `container-registry` in modo che la politica IAM venga applicata a {{site.data.keyword.registrylong_notm}}.</td>
    </tr>
    <tr>
    <td><code>--roles <em>&lt;service_access_role&gt;</em></code></td>
    <td>Obbligatoria. Immetti il [ruolo di accesso al servizio per {{site.data.keyword.registrylong_notm}}](/docs/services/Registry?topic=registry-iam#service_access_roles) a cui vuoi estendere l'accesso all'ID servizio. I valori possibili sono `Reader`, `Writer` e `Manager`.</td>
    </tr>
    <tr>
    <td><code>--region <em>&lt;IAM_region&gt;</em></code></td>
    <td>Facoltativa. Se vuoi applicare la politica di accesso a determinate regioni IAM, immetti le regioni in un elenco separato da virgole. I valori possibili sono `au-syd`, `eu-gb`, `eu-de`, `jp-tok`, `us-south` e `global`.</td>
    </tr>
    <tr>
    <td><code>--resource-type <em>namespace</em> --resource <em>&lt;registry_namespace&gt;</em></code></td>
    <td>Facoltativa. Se vuoi limitare l'accesso solo alle immagini di determinati [spazi dei nomi {{site.data.keyword.registrylong_notm}}](/docs/services/Registry?topic=registry-registry_overview#registry_namespaces), immetti il `namespace` del tipo di risorsa e specifica il `<registry_namespace>`. Per elencare gli spazi dei nomi del registro, esegui `ibmcloud cr namespaces`.</td>
    </tr>
    </tbody></table>
4.  Crea una chiave API per l'ID del servizio. Assegna alla chiave API un nome simile all'ID servizio e includi l'ID servizio che hai creato in precedenza, ``<cluster_name>-<kube_namespace>-id`. Assicurati di fornire alla chiave API una descrizione che ti aiuti a richiamare la chiave in un secondo momento.
    ```
    ibmcloud iam service-api-key-create <cluster_name>-<kube_namespace>-key <cluster_name>-<kube_namespace>-id --description "API key for service ID <service_id> in Kubernetes cluster <cluster_name> namespace <kube_namespace>"
    ```
    {: pre}
5.  Richiama il valore della tua **Chiave API** dall'output del comando precedente.
    ```
    Conserva la chiave API. Non è possibile recuperarla dopo la creazione.

    Name          <cluster_name>-<kube_namespace>-key   
    Description   key_for_registry_for_serviceid_for_kubernetes_cluster_multizone_namespace_test   
    Bound To      crn:v1:bluemix:public:iam-identity::a/1bb222bb2b33333ddd3d3333ee4ee444::serviceid:ServiceId-ff55555f-5fff-6666-g6g6-777777h7h7hh   
    Created At    2019-02-01T19:06+0000   
    API Key       i-8i88ii8jjjj9jjj99kkkkkkkkk_k9-llllll11mmm1   
    Locked        false   
    UUID          ApiKey-222nn2n2-o3o3-3o3o-4p44-oo444o44o4o4   
    ```
    {: screen}
6.  Crea un segreto di pull dell'immagine Kubernetes per memorizzare le credenziali della chiave API nello spazio dei nomi del cluster. Ripeti questo passo per ogni dominio `icr.io`, spazio dei nomi Kubernetes e cluster per cui vuoi estrarre le immagini dal registro con le credenziali IAM di questo ID del servizio.
    ```
    kubectl --namespace <kubernetes_namespace> create secret docker-registry <secret_name> --docker-server=<registry_URL> --docker-username=iamapikey --docker-password=<api_key_value> --docker-email=<docker_email>
    ```
    {: pre}

    <table>
    <caption>Descrizione dei componenti di questo comando</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dei componenti di questo comando</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace <em>&lt;kubernetes_namespace&gt;</em></code></td>
    <td>Obbligatoria. Specifica lo spazio dei nomi Kubernetes del tuo cluster utilizzato per il nome dell'ID del servizio.</td>
    </tr>
    <tr>
    <td><code><em>&lt;secret_name&gt;</em></code></td>
    <td>Obbligatoria. Immetti un nome per il tuo segreto di pull dell'immagine.</td>
    </tr>
    <tr>
    <td><code>--docker-server <em>&lt;registry_URL&gt;</em></code></td>
    <td>Obbligatoria. Imposta l'URL del registro delle immagini in cui è configurato il tuo spazio dei nomi del registro. Domini di registro disponibili:<ul>
    <li>Asia Pacifico Nord (Tokyo): `jp.icr.io`</li>
    <li>Asia Pacifico Sud (Sydney): `au.icr.io`</li>
    <li>Europa Centrale (Francoforte): `de.icr.io`</li>
    <li>Regno Unito Sud (Londra): `uk.icr.io`</li>
    <li>Stati Uniti Sud (Dallas): `us.icr.io`</li></ul></td>
    </tr>
    <tr>
    <td><code>--docker-username iamapikey</code></td>
    <td>Obbligatoria. Immetti il nome utente per accedere al tuo registro privato. Per {{site.data.keyword.registryshort_notm}}, il nome utente è impostato sul valore <strong><code>iamapikey</code></strong>.</td>
    </tr>
    <tr>
    <td><code>--docker-password <em>&lt;token_value&gt;</em></code></td>
    <td>Obbligatoria. Immetti il valore della tua **Chiave API** che hai richiamato in precedenza.</td>
    </tr>
    <tr>
    <td><code>--docker-email <em>&lt;docker-email&gt;</em></code></td>
    <td>Obbligatoria. Se ne hai uno, immetti il tuo indirizzo e-mail Docker. Altrimenti, immetti un indirizzo e-mail fittizio, ad esempio `a@b.c`. Questa e-mail è necessaria per creare un segreto Kubernetes, ma non viene utilizzata dopo la creazione.</td>
    </tr>
    </tbody></table>
7.  Verifica che il segreto sia stato creato correttamente. Sostituisci <em>&lt;kubernetes_namespace&gt;</em> con lo spazio dei nomi in cui hai creato il segreto di pull dell'immagine.

    ```
    kubectl get secrets --namespace <kubernetes_namespace>
    ```
    {: pre}
8.  [Aggiungi il segreto di pull dell'immagine a un account di servizio Kubernetes in modo che qualsiasi pod nello spazio dei nomi possa utilizzare tale segreto quando distribuisci un contenitore](#use_imagePullSecret).

### Accesso alle immagini memorizzate in altri registri privati
{: #private_images}

Se hai già un registro privato, devi memorizzare le credenziali del registro in un segreto di pull dell'immagine Kubernetes e fare riferimento a questo segreto dal tuo file di configurazione.
{:shortdesc}

Prima di iniziare:

1.  [Crea un
cluster](/docs/containers?topic=containers-clusters#clusters_ui).
2.  [Indirizza la tua CLI al tuo cluster](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).

Per creare un segreto di pull dell'immagine:

1.  Crea il segreto Kubernetes per memorizzare le credenziali del tuo registro privato.

    ```
    kubectl --namespace <kubernetes_namespace> create secret docker-registry <secret_name>  --docker-server=<registry_URL> --docker-username=<docker_username> --docker-password=<docker_password> --docker-email=<docker_email>
    ```
    {: pre}

    <table>
    <caption>Descrizione dei componenti di questo comando</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dei componenti di questo comando</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace <em>&lt;kubernetes_namespace&gt;</em></code></td>
    <td>Obbligatoria. Lo spazio dei nomi Kubernetes del cluster in cui vuoi utilizzare il segreto e a cui distribuire i contenitori. Esegui <code>kubectl get namespaces</code> per elencare tutti gli spazi dei nomi nel tuo cluster.</td>
    </tr>
    <tr>
    <td><code><em>&lt;secret_name&gt;</em></code></td>
    <td>Obbligatoria. Il nome che vuoi utilizzare per il tuo <code>imagePullSecret</code>.</td>
    </tr>
    <tr>
    <td><code>--docker-server <em>&lt;registry_URL&gt;</em></code></td>
    <td>Obbligatoria. L'URL del registro in cui sono memorizzate le tue immagini private.</td>
    </tr>
    <tr>
    <td><code>--docker-username <em>&lt;docker_username&gt;</em></code></td>
    <td>Obbligatoria. Il nome utente per accedere al tuo registro privato.</td>
    </tr>
    <tr>
    <td><code>--docker-password <em>&lt;token_value&gt;</em></code></td>
    <td>Obbligatoria. Il valore del tuo token di registro che hai richiamato in precedenza.</td>
    </tr>
    <tr>
    <td><code>--docker-email <em>&lt;docker-email&gt;</em></code></td>
    <td>Obbligatoria. Se ne hai uno, immetti il tuo indirizzo e-mail Docker. Altrimenti, immetti un indirizzo e-mail fittizio, come ad esempio `a@b.c`. Questa e-mail è necessaria per creare un segreto Kubernetes, ma non viene utilizzata dopo la creazione.</td>
    </tr>
    </tbody></table>

2.  Verifica che il segreto sia stato creato correttamente. Sostituisci <em>&lt;kubernetes_namespace&gt;</em> con il nome dello spazio in cui hai creato l'`imagePullSecret`.

    ```
    kubectl get secrets --namespace <kubernetes_namespace>
    ```
    {: pre}

3.  [Crea un pod che fa riferimento al segreto di pull dell'immagine](#use_imagePullSecret).

<br />


## Utilizzo del segreto di pull dell'immagine per la distribuzione dei contenitori
{: #use_imagePullSecret}

Puoi definire un segreto di pull dell'immagine nella tua distribuzione del pod oppure memorizzare il segreto nel tuo account di servizio Kubernetes in modo che sia disponibile per tutte le distribuzioni che non specificano un account di servizio.
{: shortdesc}

Scegli tra le seguenti opzioni:
* [Riferimento al segreto di pull dell'immagine nella tua distribuzione del pod](#pod_imagePullSecret): utilizza questa opzione se non desideri concedere l'accesso al tuo registro per tutti i pod presenti nel tuo spazio dei nomi per impostazione predefinita.
* [Memorizzazione del segreto di pull dell'immagine nell'account di servizio Kubernetes](#store_imagePullSecret): utilizza questa opzione per concedere l'accesso alle immagini presenti nel tuo registro per le tutte le distribuzioni degli spazi dei nomi Kubernetes selezionati.

Prima di iniziare:
* [Crea un segreto di pull dell'immagine](#other) per accedere alle immagini che si trovano in altri registri o in spazi dei nomi Kubernetes diversi da `default`.
* [Indirizza la tua CLI al tuo cluster](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).

### Riferimento al segreto di pull dell'immagine nella tua distribuzione del pod
{: #pod_imagePullSecret}

Quando fai riferimento al segreto di pull dell'immagine in una distribuzione del pod, il segreto è valido solo per questo pod e non può essere condiviso tra i pod nello spazio dei nomi.
{:shortdesc}

1.  Crea un file di configurazione del pod denominato `mypod.yaml`.
2.  Definisci il pod e il segreto di pull dell'immagine per l'accesso alle immagini in {{site.data.keyword.registrylong_notm}}.

    Per accedere a un'immagine privata:
    ```
    apiVersion: v1
    kind: Pod
    metadata:
      name: mypod
    spec:
      containers:
        - name: <container_name>
          image: <region>.icr.io/<namespace_name>/<image_name>:<tag>
      imagePullSecrets:
        - name: <secret_name>
    ```
    {: codeblock}

    Per accedere ad un'immagine pubblica {{site.data.keyword.Bluemix_notm}}:
    ```
    apiVersion: v1
    kind: Pod
    metadata:
      name: mypod
    spec:
      containers:
        - name: <container_name>
          image: icr.io/<image_name>:<tag>
      imagePullSecrets:
        - name: <secret_name>
    ```
    {: codeblock}

    <table>
    <caption>Descrizione dei componenti del file YAML</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dei componenti del file YAML</th>
    </thead>
    <tbody>
    <tr>
    <td><code><em>&lt;container_name&gt;</em></code></td>
    <td>Il nome del contenitore da distribuire al tuo cluster.</td>
    </tr>
    <tr>
    <td><code><em>&lt;namespace_name&gt;</em></code></td>
    <td>Lo spazio dei nomi in cui è memorizzata l'immagine. Per elencare gli spazi dei nomi disponibili, esegui `ibmcloud cr namespace-list`.</td>
    </tr>
    <tr>
    <td><code><em>&lt;image_name&gt;</em></code></td>
    <td>Il nome dell'immagine da utilizzare. Per elencare le immagini disponibili in un account {{site.data.keyword.Bluemix_notm}}, esegui `ibmcloud cr image-list`.</td>
    </tr>
    <tr>
    <td><code><em>&lt;tag&gt;</em></code></td>
    <td>La versione dell'immagine che vuoi utilizzare. Se non si specifica una tag, viene utilizzata l'immagine contrassegnata con <strong>latest</strong> per impostazione predefinita.</td>
    </tr>
    <tr>
    <td><code><em>&lt;secret_name&gt;</em></code></td>
    <td>Il nome del segreto di pull dell'immagine che hai creato in precedenza.</td>
    </tr>
    </tbody></table>

3.  Salva le modifiche.
4.  Crea la distribuzione nel tuo cluster.
    ```
    kubectl apply -f mypod.yaml
    ```
    {: pre}

### Memorizzazione del segreto di pull dell'immagine nell'account di servizio Kubernetes per lo spazio dei nomi selezionato
{:#store_imagePullSecret}

Ogni spazio dei nomi ha un account di servizio Kubernetes denominato `default`. Puoi aggiungere il segreto di pull dell'immagine a questo account di servizio per concedere l'accesso alle immagini nel tuo registro. Le distribuzioni che non specificano un account di servizio utilizzano automaticamente l'account di servizio `default` per questo spazio dei nomi.
{:shortdesc}

1. Controlla se esiste già un segreto di pull dell'immagine per il tuo account di servizio predefinito.
   ```
   kubectl describe serviceaccount default -n <namespace_name>
   ```
   {: pre}
   Se nella voce **Segreti di pull dell'immagine** viene visualizzato `<none>`, non esiste alcun segreto di pull dell'immagine.  
2. Aggiungi il segreto di pull dell'immagine al tuo account di servizio predefinito.
   - **Per aggiungere il segreto di pull dell'immagine quando non è definito alcun segreto:**
       ```
       kubectl patch -n <namespace_name> serviceaccount/default -p '{"imagePullSecrets":[{"name": "<image_pull_secret_name>"}]}'
       ```
       {: pre}
   - **Per aggiungere il segreto di pull dell'immagine quando è già definito un segreto:**
       ```
       kubectl patch -n <namespace_name> serviceaccount/default --type='json' -p='[{"op":"add","path":"/imagePullSecrets/-","value":{"name":"<image_pull_secret_name>"}}]'
       ```
       {: pre}
3. Verifica che il tuo segreto di pull dell'immagine sia stato aggiunto al tuo account di servizio predefinito.
   ```
   kubectl describe serviceaccount default -n <namespace_name>
   ```
   {: pre}

   Output di esempio:
   ```
   Name:                default
   Namespace:           <namespace_name>
   Labels:              <none>
   Annotations:         <none>
   Image pull secrets:  <image_pull_secret_name>
   Mountable secrets:   default-token-sh2dx
   Tokens:              default-token-sh2dx
   Events:              <none>
   ```
   {: pre}

4. Distribuisci un contenitore da un'immagine nel tuo registro.
   ```
   apiVersion: v1
   kind: Pod
   metadata:
     name: mypod
   spec:
     containers:
       - name: <container_name>
         image: <region>.icr.io/<namespace_name>/<image_name>:<tag>
   ```
   {: codeblock}

5. Crea la distribuzione nel cluster.
   ```
   kubectl apply -f mypod.yaml
   ```
   {: pre}

<br />


## Obsoleto: utilizzo di un token del registro per distribuire i contenitori da un'immagine {{site.data.keyword.registrylong_notm}}
{: #namespace_token}

Puoi distribuire i contenitori al tuo cluster da un'immagine pubblica fornita da IBM o da un'immagine privata
memorizzata nel tuo spazio dei nomi in {{site.data.keyword.registryshort_notm}}. I cluster esistenti utilizzano un [token](/docs/services/Registry?topic=registry-registry_access#registry_tokens) del registro memorizzato in un `imagePullSecret` del cluster per autorizzare l'accesso per l'estrazione di immagini dai nomi di dominio `registry.bluemix.net`.
{:shortdesc}

Quando crei un cluster, i segreti e i token del registro senza scadenza vengono creati automaticamente per [il registro regionale più vicino e il registro globale](/docs/services/Registry?topic=registry-registry_overview#registry_regions). Il registro globale archivia in modo sicuro le immagini fornite da IBM pubbliche a cui puoi fare riferimento nelle tue distribuzioni invece di avere riferimenti differenti per le immagini archiviate in ogni registro regionale. Il registro regionale archivia in modo sicuro le tue proprie immagini Docker private. I token sono utilizzati per autorizzare l'accesso in sola lettura a tutti i tuoi spazi dei nomi che hai configurato in {{site.data.keyword.registryshort_notm}} in modo che tu possa lavorare con queste immagini pubbliche (registro globale) e private (registro regionale).

Ogni token deve essere memorizzato in un `imagePullSecret` Kubernetes per poter essere accessibile da un cluster Kubernetes
quando distribuisci un'applicazione inserita in un contenitore. Quando il tuo cluster viene creato, {{site.data.keyword.containerlong_notm}} memorizza automaticamente i token per il registro globale (immagini pubbliche fornite da IBM) e il regionale nei segreti di pull dell'immagine Kubernetes. I segreti di pull dell'immagine vengono aggiunti allo spazio dei nomi Kubernetes di `default`, allo spazio dei nomi `kube-system` e all'elenco di segreti nell'account di servizio `default` per questi spazi dei nomi.

Questo metodo di utilizzo di un token per autorizzare il cluster ad accedere a {{site.data.keyword.registrylong_notm}} è supportato per i nomi di dominio `registry.bluemix.net` ma è obsoleto. Invece, [utilizza il metodo della chiave API](#cluster_registry_auth) per autorizzare il cluster ad accedere ai nuovi nomi di dominio del registro `icr.io`.
{: deprecated}

A seconda di dove si trova l'immagine e di dove si trova il contenitore, devi distribuire i contenitori seguendo passi diversi.
*   [Distribuisci un contenitore nello spazio dei nomi Kubernetes di `default` con un'immagine che si trova nella stessa regione del tuo cluster](#token_default_namespace)
*   [Distribuisci un contenitore in uno spazio dei nomi Kubernetes diverso da quello di `default`](#token_copy_imagePullSecret)
*   [Distribuisci un contenitore con un'immagine che si trova in una regione o in un account {{site.data.keyword.Bluemix_notm}} diversi rispetto a quelli del tuo cluster](#token_other_regions_accounts)
*   [Distribuisci un contenitore con un'immagine proveniente da un registro privato non IBM](#private_images)

Utilizzando questa configurazione iniziale, puoi distribuire i contenitori da qualsiasi immagine disponibile in uno spazio dei nomi del tuo account {{site.data.keyword.Bluemix_notm}} allo spazio dei nomi **predefinito** del tuo cluster. Per distribuire un contenitore in altri spazi dei nomi del tuo cluster o per utilizzare un'immagine memorizzata in un'altra regione {{site.data.keyword.Bluemix_notm}} o in un altro account {{site.data.keyword.Bluemix_notm}}, devi [creare il tuo proprio segreto di pull dell'immagine per il cluster](#other).
{: note}

### Obsoleto: distribuzione di immagini nello spazio dei nomi Kubernetes di `default` con un token del registro
{: #token_default_namespace}

Con il token del registro memorizzato nel segreto di pull dell'immagine, puoi distribuire un contenitore da qualsiasi immagine disponibile nel tuo {{site.data.keyword.registrylong_notm}} regionale allo spazio dei nomi **predefinito** del tuo cluster.
{: shortdesc}

Prima di iniziare:
1. [Configura uno spazio dei nomi in {{site.data.keyword.registryshort_notm}} e inserisci le immagini in questo spazio dei nomi](/docs/services/Registry?topic=registry-getting-started#gs_registry_namespace_add).
2. [Crea un
cluster](/docs/containers?topic=containers-clusters#clusters_ui).
3. [Indirizza la tua CLI al tuo cluster](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).

Per distribuire un contenitore nello spazio dei nomi **predefinito** del tuo
cluster, crea un file di configurazione.

1.  Crea un file di configurazione di distribuzione denominato `mydeployment.yaml`.
2.  Definisci la distribuzione e l'immagine che vuoi utilizzare dal tuo spazio dei nomi in {{site.data.keyword.registryshort_notm}}.

    Per
utilizzare un'immagine privata da uno spazio dei nomi in {{site.data.keyword.registryshort_notm}}:

    ```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: <app_name>-deployment
    spec:
      replicas: 3
      selector:
        matchLabels:
          app: <app_name>
      template:
        metadata:
          labels:
            app: <app_name>
        spec:
          containers:
          - name: <app_name>
            image: registry.<region>.bluemix.net/<namespace>/<my_image>:<tag>
    ```
    {: codeblock}

    **Suggerimento:** per richiamare le informazioni sul tuo spazio dei nomi, esegui `ibmcloud cr namespace-list`.

3.  Crea la distribuzione nel tuo cluster.

    ```
    kubectl apply -f mydeployment.yaml
    ```
    {: pre}

    **Suggerimento:** puoi anche distribuire un file di configurazione esistente, come una delle immagini pubbliche fornite da IBM. Questo esempio utilizza l'immagine **ibmliberty** nella regione stati uniti sud.

    ```
    kubectl apply -f https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/deploy-apps-clusters/deploy-ibmliberty.yaml
    ```
    {: pre}

### Obsoleto: copia del segreto di pull dell'immagine basato su token dallo spazio dei nomi predefinito in altri spazi dei nomi nel tuo cluster
{: #token_copy_imagePullSecret}

Puoi copiare il segreto di pull dell'immagine con le credenziali del token del registro che viene creato automaticamente per lo spazio dei nomi Kubernetes di `default` in altri spazi dei nomi nel tuo cluster.
{: shortdesc}

1. Elenca gli spazi dei nomi disponibili nel tuo cluster.
   ```
   kubectl get namespaces
   ```
   {: pre}

   Output di esempio:
   ```
   default          Active    79d
   ibm-cert-store   Active    79d
   ibm-system       Active    79d
   istio-system     Active    34d
   kube-public      Active    79d
   kube-system      Active    79d
   ```
   {: screen}

2. Facoltativo: crea uno spazio dei nomi nel tuo cluster.
   ```
   kubectl create namespace <namespace_name>
   ```
   {: pre}

3. Copia i segreti di pull dell'immagine dallo spazio dei nomi di `default` allo spazio dei nomi di tua scelta. I nuovi segreti di pull dell'immagine sono denominati `bluemix-<namespace_name>-secret-regional` e `bluemix-<namespace_name>-secret-international`
   ```
   kubectl get secret bluemix-default-secret-regional -o yaml | sed 's/default/<namespace_name>/g' | kubectl -n <namespace_name> create -f -
   ```
   {: pre}

   ```
   kubectl get secret bluemix-default-secret-international -o yaml | sed 's/default/<namespace_name>/g' | kubectl -n <namespace_name> create -f -
   ```
   {: pre}

4.  Verifica che i segreti siano stati creati correttamente.
    ```
    kubectl get secrets --namespace <namespace_name>
    ```
    {: pre}

5. [Distribuisci un contenitore utilizzando l'`imagePullSecret`](#use_imagePullSecret) nel tuo spazio dei nomi.


### Obsoleto: creazione di un segreto di pull dell'immagine basato su token per accedere alle immagini in altre regioni o in altri account {{site.data.keyword.Bluemix_notm}}
{: #token_other_regions_accounts}

Per accedere alle immagini in altre regioni o in altri account {{site.data.keyword.Bluemix_notm}}, devi creare un token del registro e salvare le tue credenziali in un segreto di pull dell'immagine.
{: shortdesc}

1.  Se non hai un token, [crea un token per il registro a cui vuoi accedere.](/docs/services/Registry?topic=registry-registry_access#registry_tokens_create)
2.  Elenca i token nel tuo account {{site.data.keyword.Bluemix_notm}}.

    ```
    ibmcloud cr token-list
    ```
    {: pre}

3.  Prendi nota dell'ID token che vuoi utilizzare.
4.  Richiama il valore per il tuo token. Sostituisci <em>&lt;token_ID&gt;</em> con l'ID del token che hai richiamato nel passo precedente.

    ```
    ibmcloud cr token-get <token_id>
    ```
    {: pre}

    Il valore del tuo token viene visualizzato
nel campo **Token** dell'output della tua CLI.

5.  Crea il segreto Kubernetes per memorizzare le informazioni sul token.

    ```
    kubectl --namespace <kubernetes_namespace> create secret docker-registry <secret_name>  --docker-server=<registry_URL> --docker-username=token --docker-password=<token_value> --docker-email=<docker_email>
    ```
    {: pre}

    <table>
    <caption>Descrizione dei componenti di questo comando</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dei componenti di questo comando</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace <em>&lt;kubernetes_namespace&gt;</em></code></td>
    <td>Obbligatoria. Lo spazio dei nomi Kubernetes del cluster in cui vuoi utilizzare il segreto e a cui distribuire i contenitori. Esegui <code>kubectl get namespaces</code> per elencare tutti gli spazi dei nomi nel tuo cluster.</td>
    </tr>
    <tr>
    <td><code><em>&lt;secret_name&gt;</em></code></td>
    <td>Obbligatoria. Il nome che vuoi utilizzare per il tuo segreto di pull dell'immagine.</td>
    </tr>
    <tr>
    <td><code>--docker-server <em>&lt;registry_URL&gt;</em></code></td>
    <td>Obbligatoria. L'URL del registro delle immagini in cui è configurato il tuo spazio dei nomi.<ul><li>Per gli spazi dei nomi configurati in Stati Uniti Sud e Stati Uniti Est <code>registry.ng.bluemix.net</code></li><li>Per gli spazi dei nomi configurati in Regno Unito Sud <code>registry.eu-gb.bluemix.net</code></li><li>Per gli spazi dei nomi configurati in Europa centrale (Francoforte) <code>registry.eu-de.bluemix.net</code></li><li>Per gli spazi dei nomi configurati in Australia (Sydney) <code>registry.au-syd.bluemix.net</code></li></ul></td>
    </tr>
    <tr>
    <td><code>--docker-username <em>&lt;docker_username&gt;</em></code></td>
    <td>Obbligatoria. Il nome utente per accedere al tuo registro privato. Per {{site.data.keyword.registryshort_notm}}, il nome utente è impostato sul valore <strong><code>token</code></strong>.</td>
    </tr>
    <tr>
    <td><code>--docker-password <em>&lt;token_value&gt;</em></code></td>
    <td>Obbligatoria. Il valore del tuo token di registro che hai richiamato in precedenza.</td>
    </tr>
    <tr>
    <td><code>--docker-email <em>&lt;docker-email&gt;</em></code></td>
    <td>Obbligatoria. Se ne hai uno, immetti il tuo indirizzo e-mail Docker. Altrimenti, immetti un indirizzo e-mail fittizio, come ad esempio a@b.c. Questa e-mail è obbligatoria per creare un segreto Kubernetes, ma non viene utilizzata dopo la creazione.</td>
    </tr>
    </tbody></table>

6.  Verifica che il segreto sia stato creato correttamente. Sostituisci <em>&lt;kubernetes_namespace&gt;</em> con lo spazio dei nomi in cui hai creato il segreto di pull dell'immagine.

    ```
    kubectl get secrets --namespace <kubernetes_namespace>
    ```
    {: pre}

7.  [Distribuisci un contenitore utilizzando il segreto di pull dell'immagine](#use_imagePullSecret) nel tuo spazio dei nomi.
