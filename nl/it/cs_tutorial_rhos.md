---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: kubernetes, iks, oks, iro, openshift, red hat, red hat openshift, rhos

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

# Esercitazione: Creazione di un cluster Red Hat OpenShift on IBM Cloud (beta)
{: #openshift_tutorial}

Red Hat OpenShift on IBM Cloud è disponibile come versione beta per testare i cluster OpenShift. Non tutte le funzioni di {{site.data.keyword.containerlong}} sono disponibili durante il periodo beta. Inoltre, qualsiasi cluster beta OpenShift che crei rimane per soli 30 giorni dopo la fine della versione beta e Red Hat OpenShift on IBM Cloud diventa generalmente disponibile.
{: preview}

Con **Red Hat OpenShift on IBM Cloud beta**, puoi creare cluster {{site.data.keyword.containerlong_notm}} con i nodi di lavoro che vengono installati con il software della piattaforma di orchestrazione dei contenitori OpenShift. Ottieni tutti i [vantaggi del servizio {{site.data.keyword.containerlong_notm}}gestito](/docs/containers?topic=containers-responsibilities_iks) per l'ambiente della tua infrastruttura cluster, mentre utilizzi gli [strumenti e il catalogo OpenShift ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://docs.openshift.com/container-platform/3.11/welcome/index.html) eseguiti su Red Hat Enterprise Linux per le tue distribuzioni dell'applicazione.
{: shortdesc}

I nodi di lavoro OpenShift sono disponibili solo per i cluster standard. Red Hat OpenShift on IBM Cloud supporta solo OpenShift versione 3.11, che include Kubernetes versione 1.11.
{: note}

## Obiettivi
{: #openshift_objectives}

Nelle lezioni dell'esercitazione, crei un cluster Red Hat OpenShift on IBM Cloud standard, apri la console OpenShift, accedi ai componenti OpenShift integrati, distribuisci un'applicazione che utilizza i servizi {{site.data.keyword.Bluemix_notm}} in un progetto OpenShift ed esponi l'applicazione su un instradamento OpenShift in modo che gli utenti esterni possano accedere al servizio.
{: shortdesc}

Questa pagina include anche informazioni sull'architettura del cluster OpenShift, sui limiti della versione beta e su come fornire feedback e ottenere supporto.

## Tempo richiesto
{: #openshift_time}
45 minuti

## Destinatari
{: #openshift_audience}

Questa esercitazione è rivolta agli amministratori del cluster che vogliono imparare come creare un cluster Red Hat OpenShift on IBM Cloud per la prima volta.
{: shortdesc}

## Prerequisiti
{: #openshift_prereqs}

*   Assicurati di disporre delle seguenti politiche di accesso {{site.data.keyword.Bluemix_notm}} IAM.
    *   Il [ruolo della piattaforma **Amministratore**](/docs/containers?topic=containers-users#platform) per {{site.data.keyword.containerlong_notm}}
    *   Il [ruolo del servizio **Scrittore** o **Gestore**](/docs/containers?topic=containers-users#platform) per {{site.data.keyword.containerlong_notm}}
    *   Il [ruolo della piattaforma **Amministratore**](/docs/containers?topic=containers-users#platform) per {{site.data.keyword.registrylong_notm}}
*    Assicurati che la [chiave API](/docs/containers?topic=containers-users#api_key) per la regione e il gruppo di risorse {{site.data.keyword.Bluemix_notm}} sia configurata con le autorizzazioni dell'infrastruttura corrette, **Super utente**, o con i [ruoli minimi](/docs/containers?topic=containers-access_reference#infra) per creare un cluster.
*   Installa gli strumenti di riga di comando.
    *   [Installa la CLI {{site.data.keyword.Bluemix_notm}} (`ibmcloud`), il plugin {{site.data.keyword.containershort_notm}} (`ibmcloud ks`) e il plugin {{site.data.keyword.registryshort_notm}} (`ibmcloud cr`)](/docs/containers?topic=containers-cs_cli_install#cs_cli_install_steps).
    *   [Installa le CLI OpenShift Origin (`oc`) e Kubernetes (`kubectl`)](/docs/containers?topic=containers-cs_cli_install#cli_oc).

<br />


## Panoramica dell'architettura
{: #openshift_architecture}

Il diagramma e la tabella che seguono descrivono i componenti predefiniti che sono configurati in un'architettura Red Hat OpenShift on IBM Cloud.
{: shortdesc}

![Architettura del cluster Red Hat OpenShift on IBM Cloud](images/cs_org_ov_both_ses_rhos.png)

| Componenti master| Descrizione |
|:-----------------|:-----------------|
| Repliche | I componenti master, tra cui il server API Kubernetes e l'archivio dati etcd di OpenShift, hanno tre repliche e, se situati in un'area metropolitana multizona, vengono estesi tra le zone per una disponibilità ancora maggiore. I componenti master vengono sottoposti a backup ogni 8 ore.|
| `rhos-api` | Il server API Kubernetes di OpenShift funge da punto di ingresso principale per tutte le richieste di gestione del cluster dal nodo di lavoro al master. Il server API convalida ed elabora le richieste che modificano lo stato delle risorse Kubernetes, come i pod o i servizi, e archivia questo stato nell'archivio dati etcd.|
| `openvpn-server` | Il server OpenVPN funziona con il client OpenVPN per connettere in modo protetto il master al nodo di lavoro. Questa connessione supporta le chiamate `apiserver proxy` ai tuoi pod e servizi e le chiamate `kubectl exec`, `attach` e `logs` a kubelet.|
| `etcd` | etcd è un archivio di valori chiave altamente disponibile che archivia lo stato di tutte le risorse Kubernetes di un cluster, quali servizi, distribuzioni e pod. I dati in etcd sono sottoposti a backup su un'istanza di archiviazione crittografata gestita da IBM.|
| `rhos-controller` | Il gestore controller OpenShift verifica i pod appena creati e decide dove distribuirli in base a capacità, esigenze di prestazioni, vincoli delle politiche, specifiche di anti-affinità e requisiti del carico di lavoro. Se non è possibile trovare alcun nodo di lavoro che corrisponda ai requisiti, il pod non viene distribuito nel cluster. Il controller verifica anche lo stato delle risorse del cluster, come le serie di repliche. Quando lo stato di una risorsa viene modificato, ad esempio se si verifica un'interruzione di un pod in una serie di repliche, il gestore controller avvia le azioni correttive per raggiungere lo stato richiesto. `rhos-controller` funziona sia come programma di pianificazione (scheduler) che come gestore controller in una configurazione Kubernetes nativa. |
| `cloud-controller-manager` | Il gestore controller cloud gestisce i componenti specifici del provider cloud, come ad esempio il programma di bilanciamento del carico {{site.data.keyword.Bluemix_notm}}.|
{: caption="Tabella 1. Componenti master OpenShift." caption-side="top"}
{: #rhos-components-1}
{: tab-title="Master"}
{: tab-group="rhos-components"}
{: class="simple-tab-table"}

| Componenti di nodo di lavoro| Descrizione |
|:-----------------|:-----------------|
| Sistema operativo | I nodi di lavoro Red Hat OpenShift on IBM Cloud vengono eseguiti sul sistema operativo Red Hat Enterprise Linux 7 (RHEL 7). |
| Progetti | OpenShift organizza le tue risorse in progetti, che sono spazi dei nomi Kubernetes con annotazioni, e include molti più componenti rispetto ai cluster nativi Kubernetes per eseguire funzioni OpenShift come il catalogo. I componenti selezionati dei progetti sono descritti nelle seguenti righe. Per ulteriori informazioni, vedi [Progetti e utenti ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://docs.openshift.com/container-platform/3.11/architecture/core_concepts/projects_and_users.html).|
| `kube-system` | Questo spazio dei nomi include molti componenti che vengono utilizzati per eseguire Kubernetes sul nodo di lavoro.<ul><li>**`ibm-master-proxy`**: `ibm-master-proxy` è una serie di daemon che inoltra le richieste dal nodo di lavoro agli indirizzi IP delle repliche master altamente disponibili. Nei cluster a zona singola, il master ha tre repliche su host separati. Per i cluster che si trovano in una zona che supporta il multizona, il master ha tre repliche che vengono estese tra le zone. Un programma di bilanciamento del carico altamente disponibile inoltra le richieste effettuate al nome di dominio del master alle repliche del master.</li><li>**`openvpn-client`**: il client OpenVPN funziona con il server OpenVPN per connettere in modo protetto il master al nodo di lavoro. Questa connessione supporta le chiamate `apiserver proxy` ai tuoi pod e servizi e le chiamate `kubectl exec`, `attach` e `logs` a kubelet.</li><li>**`kubelet`**: kubelet è un agent del nodo di lavoro che viene eseguito su ogni nodo di lavoro ed è responsabile del monitoraggio dell'integrità dei pod in esecuzione sul nodo di lavoro e del controllo degli eventi inviati dal server API Kubernetes. In base agli eventi, il kubelet crea o rimuove i pod, garantisce i probe di attività e disponibilità e segnala in risposta lo stato dei pod al server API Kubernetes.</li><li>**`calico`**: Calico gestisce le politiche di rete per il tuo cluster e include alcuni componenti per gestire la connettività di rete dei contenitori, l'assegnazione di indirizzi IP e il controllo del traffico di rete.</li><li>**Altri componenti**: lo spazio dei nomi `kube-system` include anche dei componenti per gestire le risorse fornite da IBM come i plugin di archiviazione per l'archiviazione file e blocchi, l'ALB (application load balancer) ingress, la registrazione `fluentd` e `keepalived`.</li></ul>|
| `ibm-system` | Questo spazio dei nomi include la distribuzione `ibm-cloud-provider-ip` che funziona con `keepalived` per fornire il controllo dell'integrità e il bilanciamento del carico di livello 4 per le richieste ai pod dell'applicazione.|
| `kube-proxy-and-dns`| Questo spazio dei nomi include i componenti per convalidare il traffico di rete in entrata rispetto alle regole `iptables` configurate sul nodo di lavoro e le richieste di proxy che possono entrare o uscire dal cluster.|
| `default` | Questo spazio dei nomi viene utilizzato se non specifichi uno spazio dei nomi o crei un progetto per le tue risorse Kubernetes. Inoltre, lo spazio dei nomi predefinito include i seguenti componenti per supportare i tuoi cluster OpenShift.<ul><li>**`router`**: OpenShift utilizza gli [instradamenti ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://docs.openshift.com/container-platform/3.11/architecture/networking/routes.html) per esporre il servizio di un'applicazione su un nome host in modo che i client esterni possano raggiungere il servizio. Il router associa il servizio al nome host.</li><li>**`docker-registry`** e **`registry-console`**: OpenShift fornisce un [registro di immagini contenitore ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://docs.openshift.com/container-platform/3.11/install_config/registry/index.html) interno che puoi utilizzare per gestire e visualizzare localmente le immagini attraverso la console. In alternativa puoi configurare l'{{site.data.keyword.registrylong_notm}} privato.</li></ul>|
| Altri progetti | Altri componenti sono installati in vari spazi dei nomi per impostazione predefinita per abilitare funzionalità come la registrazione, il monitoraggio e la console OpenShift.<ul><li><code>ibm-cert-store</code></li><li><code>kube-public</code></li><li><code>kube-service-catalog</code></li><li><code>openshift</code></li><li><code>openshift-ansible-service-broker</code></li><li><code>openshift-console</code></li><li><code>openshift-infra</code></li><li><code>openshift-monitoring</code></li><li><code>openshift-node</code></li><li><code>openshift-template-service-broker</code></li><li><code>openshift-web-console</code></li></ul>|
{: caption="Tabella 2. Componenti di nodo di lavoro OpenShift." caption-side="top"}
{: #rhos-components-2}
{: tab-title="Worker nodes"}
{: tab-group="rhos-components"}
{: class="simple-tab-table"}

<br />


## Lezione 1: Creazione di un cluster Red Hat OpenShift on IBM Cloud
{: #openshift_create_cluster}

Puoi creare un cluster Red Hat OpenShift on IBM Cloud in {{site.data.keyword.containerlong_notm}} utilizzando la [console](#openshift_create_cluster_console) o la [CLI](#openshift_create_cluster_cli). Per informazioni su quali componenti vengono configurati quando crei un cluster, vedi la [Panoramica dell'architettura](#openshift_architecture). OpenShift è disponibile solo per i cluster standard. Puoi trovare ulteriori informazioni sul prezzo dei cluster standard nelle [domande frequenti (FAQ)](/docs/containers?topic=containers-faqs#charges).
{:shortdesc}

Puoi creare i cluster solo nel gruppo di risorse **predefinito**. Tutti i cluster OpenShift che crei durante il periodo beta rimangono per 30 giorni dopo la fine della versione beta e Red Hat OpenShift on IBM Cloud diventa generalmente disponibile.
{: important}

### Creazione di un cluster con la console
{: #openshift_create_cluster_console}

Crea un cluster OpenShift standard nella console {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

Prima di iniziare, [completa i prerequisiti](#openshift_prereqs) per assicurarti di disporre delle autorizzazioni appropriate per creare un cluster.

1.  Crea un cluster.
    1.  Accedi al tuo [account {{site.data.keyword.Bluemix_notm}} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cloud.ibm.com/).
    2.  Dal menu a tre linee ![icona di menu a tre linee](../icons/icon_hamburger.svg "icona di menu a tre linee"), seleziona **Kubernetes** e fai clic su **Create cluster**.
    3.  Scegli i dettagli di configurazione e il nome per il tuo cluster. Per la versione beta, i cluster OpenShift sono disponibili solo come cluster standard ubicati nei data center di Washington, DC e Londra.
        *   Per **Select a plan**, scegli **Standard**.
        *   Per **Resource Group**, devi utilizzare **default**.
        *   Per **Location**, imposta l'area geografica su **North America** o **Europe**, seleziona una disponibilità **Single zone** o **Multizone** e seleziona quindi le zone di lavoro **Washington, DC** o **London**.
        *   Per **Default worker pool**, seleziona la versione cluster **OpenShift**. Red Hat OpenShift on IBM Cloud supporta solo OpenShift versione 3.11, che include Kubernetes versione 1.11. Scegli una varietà disponibile per i tuoi nodi di lavoro idealmente con almeno quattro core e 16 GB di RAM.
        *   Imposta un numero di nodi di lavoro da creare per ogni zona, ad esempio 3.
    4.  Per finire, fai clic su **Create cluster**.<p class="note">Il completamento del processo di creazione del tuo cluster potrebbe richiedere alcuni minuti. Una volta che lo stato del cluster diventa **Normal**, i componenti di rete e di bilanciamento del carico del cluster impiegano circa altri 10 minuti per distribuire e aggiornare il dominio cluster che utilizzi per la console web OpenShift e per altri instradamenti. Attendi che il cluster sia pronto prima di proseguire con il passo successivo controllando che il **dominio secondario Ingress** segua un modello di `<cluster_name>.<region>.containers.appdomain.cloud`.</p>
2.  Dalla pagina dei dettagli del cluster, fai clic su **OpenShift web console**.
3.  Dal menu a discesa nella barra dei menu della piattaforma del contenitore OpenShift, fai clic su **Application Console**. La console dell'applicazione elenca tutti gli spazi dei nomi del progetto nel tuo cluster. Puoi passare a uno spazio dei nomi per visualizzare le tue applicazioni, le tue build e altre risorse Kubernetes.
4.  Per completare la prossima lezione lavorando nel terminale, fai clic sul tuo profilo **IAM#user.name@email.com > Copy Login Command**. Incolla nel tuo terminale il comando di login `oc` copiato per eseguire l'autenticazione tramite la CLI.

### Creazione di un cluster con la CLI
{: #openshift_create_cluster_cli}

Crea un cluster OpenShift standard utilizzando la CLI {{site.data.keyword.Bluemix_notm}}.
{: shortdesc}

Prima di iniziare, [completa i prerequisiti](#openshift_prereqs) per assicurarti di avere le autorizzazioni appropriate per creare un cluster, la CLI e i plugin `ibmcloud` e le CLI `oc` e `kubectl`.

1.  Accedi all'account che hai configurato per creare i cluster OpenShift. Specifica la regione **us-east** o **eu-gb** e il gruppo di risorse **default**. Se hai un account federato, includi l'indicatore `--sso`.
    ```
    ibmcloud login -r (us-east|eu-gb) -g default [--sso]
    ```
    {: pre}
2.  Crea un cluster.
    ```
    ibmcloud ks cluster-create --name <name> --location <zone> --kube-version <openshift_version> --machine-type <worker_node_flavor> --workers <number_of_worker_nodes_per_zone> --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID>
    ```
    {: pre}

    Comando di esempio per creare un cluster con tre nodi di lavoro che hanno quattro core e 16 GB di memoria a Washington, DC.

    ```
    ibmcloud ks cluster-create --name my_openshift --location wdc04 --kube-version 3.11_openshift --machine-type b3c.4x16.encrypted  --workers 3 --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID>
    ```
    {: pre}

    <table summary="La prima riga nella tabella si estende su entrambe le colonne. Le restanti righe devono essere lette da sinistra a destra, con i componenti del comando nella colonna uno e la descrizione corrispondente nella colonna due.">
    <caption>Componenti di cluster-create</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dei componenti di questo comando</th>
    </thead>
    <tbody>
    <tr>
    <td><code>cluster-create</code></td>
    <td>Il comando per creare un cluster dell'infrastruttura classica nel tuo account {{site.data.keyword.Bluemix_notm}}.</td>
    </tr>
    <tr>
    <td><code>--name <em>&lt;name&gt;</em></code></td>
    <td>Immetti un nome per il tuo cluster. Il nome deve iniziare con una lettera, può contenere lettere, numeri e un trattino (-) e non può contenere più di 35 caratteri. Utilizza un nome che sia univoco tra tutte le regioni {{site.data.keyword.Bluemix_notm}}.</td>
    </tr>
    <tr>
    <td><code>--location <em>&lt;zone&gt;</em></code></td>
    <td>Specifica la zona in cui vuoi creare il cluster. Per la versione beta, le zone disponibili sono `wdc04, wdc06, wdc07, lon04, lon05` o `lon06`.</p></td>
    </tr>
    <tr>
      <td><code>--kube-version <em>&lt;openshift_version&gt;</em></code></td>
      <td>Devi scegliere una versione OpenShift supportata. Le versioni OpenShift includono una versione di Kubernetes che differisce dalle versioni Kubernetes disponibili sui cluster Ubuntu nativi di Kubernetes. Per elencare le versioni OpenShift disponibili, esegui `ibmcloud ks versions`. Per creare un cluster con la versione patch più recente, puoi specificare solo la versione principale e secondaria, ad esempio ` 3.11_openshift`.<br><br>Red Hat OpenShift on IBM Cloud supporta solo OpenShift versione 3.11, che include Kubernetes versione 1.11.</td>
    </tr>
    <tr>
    <td><code>--machine-type <em>&lt;worker_node_flavor&gt;</em></code></td>
    <td>Scegli un tipo di macchina. Puoi distribuire i tuoi nodi di lavoro come macchine virtuali su hardware condiviso o dedicato o come macchine fisiche su bare metal. I tipi di macchine fisiche e virtuali disponibili variano in base alla zona in cui distribuisci il cluster. Per elencare i tipi di macchina disponibili, esegui `ibmcloud ks machine-types --zone <zone>`.</td>
    </tr>
    <tr>
    <td><code>--workers <em>&lt;number_of_worker_nodes_per_zone&gt;</em></code></td>
    <td>Il numero di nodi di lavoro da includere nel cluster. Potresti voler specificare almeno tre nodi di lavoro in modo che il tuo cluster disponga di risorse sufficienti per eseguire i componenti predefiniti e per l'alta disponibilità. Se non viene specificata l'opzione <code>--workers</code>, viene creato un nodo di lavoro.</td>
    </tr>
    <tr>
    <td><code>--public-vlan <em>&lt;public_vlan_id&gt;</em></code></td>
    <td>Se hai già una VLAN pubblica configurata nel tuo account dell'infrastruttura IBM Cloud (SoftLayer) per quella zona, immetti l'ID della VLAN pubblica. Per controllare le VLAN disponibili, esegui `ibmcloud ks vlans --zone <zone>`. <br><br>Se non hai una VLAN pubblica nel tuo account, non specificare questa opzione. {{site.data.keyword.containerlong_notm}} crea automaticamente una VLAN pubblica per tuo conto.</td>
    </tr>
    <tr>
    <td><code>--private-vlan <em>&lt;private_vlan_id&gt;</em></code></td>
    <td>Se hai già una VLAN privata configurata nel tuo account dell'infrastruttura IBM Cloud (SoftLayer) per quella zona, immetti l'ID della VLAN privata. Per controllare le VLAN disponibili, esegui `ibmcloud ks vlans --zone <zone>`. <br><br>Se non hai una VLAN privata nel tuo account, non specificare questa opzione. {{site.data.keyword.containerlong_notm}} crea automaticamente una VLAN privata per te.</td>
    </tr>
    </tbody></table>
3.  Elenca i dettagli del tuo cluster. Esamina lo **Stato** del cluster, controlla il **Dominio secondario Ingress** e prendi nota dell'**URL master**.<p class="note">Il completamento del processo di creazione del tuo cluster potrebbe richiedere alcuni minuti. Una volta che lo stato del cluster diventa **Normal**, i componenti di rete e di bilanciamento del carico del cluster impiegano circa altri 10 minuti per distribuire e aggiornare il dominio cluster che utilizzi per la console web OpenShift e per altri instradamenti. Attendi che il cluster sia pronto prima di proseguire con il passo successivo controllando che il **Dominio secondario Ingress** segua un modello di `<cluster_name>.<region>.containers.appdomain.cloud`.</p>
    ```
    ibmcloud ks cluster-get --cluster <cluster_name_or_ID>
    ```
    {: pre}
4.  Scarica i file di configurazione per connetterti al tuo cluster.
    ```
    ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
    ```
    {: pre}

    Al termine del download dei file di configurazione, viene visualizzato un comando che puoi copiare e incollare per impostare il percorso del file di configurazione Kubernetes locale come variabile di ambiente.

    Esempio per OS X:

    ```
    export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-<datacenter>-<cluster_name>.yml
    ```
    {: screen}
5.  Nel tuo browser, passa all'indirizzo del tuo **URL master URL** e aggiungi `/console`. Ad esempio, `https://c0.containers.cloud.ibm.com:23652/console`.
6.  Fai clic sul tuo profilo **IAM#user.name@email.com > Copy Login Command**. Incolla nel tuo terminale il comando di login `oc` copiato per eseguire l'autenticazione tramite la CLI.<p class="tip">Salva l'URL master del tuo cluster per accedere successivamente alla console OpenShift. Nelle sessioni future, puoi saltare il passo `cluster-config` e copiare invece il comando di login dalla console.</p>
7.  Verifica che i comandi `oc` vengano eseguiti correttamente con il tuo cluster controllando la versione.

    ```
    oc version
    ```
    {: pre}

    Output di esempio:

    ```
    oc v3.11.0+0cbc58b
    kubernetes v1.11.0+d4cacc0
    features: Basic-Auth

    Server https://c0.containers.cloud.ibm.com:23652
    openshift v3.11.98
    kubernetes v1.11.0+d4cacc0
    ```
    {: screen}

    Se non puoi eseguire operazioni che richiedono le autorizzazioni di Amministratore, come elencare tutti i nodi di lavoro o i pod in un cluster, scarica i certificati TLS e i file di autorizzazione per l'amministratore del cluster eseguendo il comando `ibmcloud ks cluster-config --cluster <cluster_name_or_ID> --admin`.
    {: tip}

<br />


## Lezione 2: Accesso ai servizi OpenShift integrati
{: #openshift_access_oc_services}

Red Hat OpenShift on IBM Cloud viene fornito con dei servizi integrati che puoi utilizzare per gestire il tuo cluster, come ad esempio la console OpenShift, Prometheus e Grafana. Per la versione beta, per accedere a questi servizi, puoi utilizzare l'host locale di un [instradamento ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://docs.openshift.com/container-platform/3.11/architecture/networking/routes.html). I nomi di dominio di instradamento predefiniti seguono un modello specifico per il cluster di `<service_name>-<namespace>.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud`.
{:shortdesc}

Puoi accedere agli instradamenti dei servizi OpenShift integrati dalla [console](#openshift_services_console) o dalla [CLI](#openshift_services_cli). Potresti voler utilizzare la console per spostarti tra le risorse Kubernetes in un unico progetto. Utilizzando la CLI, puoi elencare le risorse, come gli instradamenti, in più progetti.

### Accesso ai servizi OpenShift integrati dalla console
{: #openshift_services_console}
1.  Dalla console web OpenShift, nel menu a discesa nella barra dei menu della piattaforma del contenitore OpenShift, fai clic su **Application Console**.
2.  Seleziona il progetto di **default**, quindi, nel riquadro di navigazione, fai clic su **Applications > Pods**.
3.  Verifica che i pod del **router** siano in uno stato **Running**. Il router funge da punto di ingresso per il traffico di rete esterno. Puoi utilizzare il router per esporre pubblicamente i servizi nel tuo cluster sull'indirizzo IP esterno del router utilizzando un instradamento. Il router rimane in ascolto sull'interfacci di rete host pubblica, a differenza dei tuoi pod dell'applicazione che rimangono in ascolto solo sugli IP privati. Il router trasmette tramite proxy le richieste esterne per i nomi host di instradamento agli IP dei pod dell'applicazione identificati dal servizio che hai associato al nome host di instradamento.
4.  Dal riquadro di navigazione del progetto di **default**, fai clic su **Applications > Deployments** e quindi sulla distribuzione **registry-console**. Per aprire la console del registro interno, devi aggiornare l'URL del provider in modo da poter accedervi esternamente.
    1.  Nella scheda **Environment** della pagina dei dettagli di **registry-console**, cerca il campo **OPENSHIFT_OAUTH_PROVIDER_URL**. 
    2. Nel campo del valore, aggiungi `-e` dopo `c1` come in `https://c1-e.eu-gb.containers.cloud.ibm.com:20399`. 
    3. Fai clic su **Salva**. Ora è possibile accedere alla distribuzione della console del registro tramite l'endpoint API pubblico del master cluster.
    4.  Dal riquadro di navigazione del progetto di **default**, fai clic su **Applications > Routes**. Per aprire la console del registro, fai clic sul valore **Hostname**, ad esempio `https://registry-console-default.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud`.<p class="note">Per la versione beta, la console del registro utilizza certificati TLS autofirmati, quindi devi scegliere di procedere per accedere alla console del registro. In Google Chrome, fai clic su **Advanced > Proceed to <cluster_master_URL>**. Gli altri browser hanno delle opzioni simili. Se non puoi procedere con questa impostazione, prova ad aprire l'URL in un browser privato.</p>
5.  Dal menu a discesa nella barra dei menu della piattaforma del contenitore OpenShift, fai clic su **Cluster Console**.
6.  Dal riquadro di navigazione, espandi **Monitoring**.
7.  Fai clic sullo strumento di monitoraggio integrato a cui vuoi accedere, ad esempio **Dashboards**. Si apre l'instradamento di Grafana, `https://grafana-openshift-monitoring.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud`.<p class="note">La prima volta che accedi al nome host potresti dover eseguire l'autenticazione, ad esempio facendo clic su **Log in with OpenShift** e autorizzando l'accesso alla tua identità IAM.</p>

### Accesso ai servizi OpenShift integrati dalla CLI
{: #openshift_services_cli}

1.  Dalla console web OpenShift, fai clic sul tuo profilo **IAM#user.name@email.com > Copy Login Command** e incolla il comando di login nel tuo terminale per eseguire l'autenticazione.
    ```
    oc login https://c1-e.<region>.containers.cloud.ibm.com:<port> --token=<access_token>
    ```
    {: pre}
2.  Verifica che il tuo router sia stato distribuito. Il router funge da punto di ingresso per il traffico di rete esterno. Puoi utilizzare il router per esporre pubblicamente i servizi nel tuo cluster sull'indirizzo IP esterno del router utilizzando un instradamento. Il router rimane in ascolto sull'interfacci di rete host pubblica, a differenza dei tuoi pod dell'applicazione che rimangono in ascolto solo sugli IP privati. Il router trasmette tramite proxy le richieste esterne per i nomi host di instradamento agli IP dei pod dell'applicazione identificati dal servizio che hai associato al nome host di instradamento.
    ```
    oc get svc router -n default
    ```
    {: pre}

    Output di esempio:
    ```
    NAME      TYPE           CLUSTER-IP               EXTERNAL-IP     PORT(S)                      AGE
    router    LoadBalancer   172.21.xxx.xxx   169.xx.xxx.xxx   80:30399/TCP,443:32651/TCP                      5h
    ```
    {: screen}
2.  Ottieni il nome host **Host/Port** dell'instradamento del servizio a cui vuoi accedere. Ad esempio, potresti voler accedere al tuo dashboard Grafana per controllare le metriche relative all'utilizzo delle risorse del tuo cluster. I nomi di dominio di instradamento predefiniti seguono un modello specifico per il cluster di `<service_name>-<namespace>.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud`.
    ```
    oc get route --all-namespaces
    ```
    {: pre}

    Output di esempio:
    ```
    NAMESPACE                          NAME                HOST/PORT                                                                    PATH                  SERVICES            PORT               TERMINATION          WILDCARD
    default                            registry-console    registry-console-default.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud                              registry-console    registry-console   passthrough          None
    kube-service-catalog               apiserver           apiserver-kube-service-catalog.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud                        apiserver           secure             passthrough          None
    openshift-ansible-service-broker   asb-1338            asb-1338-openshift-ansible-service-broker.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud            asb                 1338               reencrypt            None
    openshift-console                  console             console.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud                                              console             https              reencrypt/Redirect   None
    openshift-monitoring               alertmanager-main   alertmanager-main-openshift-monitoring.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud                alertmanager-main   web                reencrypt            None
    openshift-monitoring               grafana             grafana-openshift-monitoring.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud                          grafana             https              reencrypt            None
    openshift-monitoring               prometheus-k8s      prometheus-k8s-openshift-monitoring.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud                   prometheus-k8s      web                reencrypt
    ```
    {: screen}
3.  **Aggiornamento una tantum del registro**: per rendere accessibile la tua console del registro interno da Internet, modifica la distribuzione `registry-console` per utilizzare l'endpoint API pubblico del tuo master cluster come URL del provider OpenShift. L'endpoint API pubblico ha lo stesso formato dell'endpoint API privato, ma include un ulteriore `-e` nell'URL.
    ```
    oc edit deploy registry-console -n default
    ```
    {: pre}
    
    Nel campo `Pod Template.Containers.registry-console.Environment.OPENSHIFT_OAUTH_PROVIDER_URL`, aggiungi `-e` dopo `c1` come in `https://ce.eu-gb.containers.cloud.ibm.com:20399`.
    ```
    Name:                   registry-console
    Namespace:              default
    ...
    Pod Template:
      Labels:  name=registry-console
      Containers:
       registry-console:
        Image:      registry.eu-gb.bluemix.net/armada-master/iksorigin-registrconsole:v3.11.98-6
        ...
        Environment:
          OPENSHIFT_OAUTH_PROVIDER_URL:  https://c1-e.eu-gb.containers.cloud.ibm.com:20399
          ...
    ```
    {: screen}
4.  Nel tuo browser web, apri l'instradamento a cui vuoi accedere, ad esempio: `https://grafana-openshift-monitoring.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud`. La prima volta che accedi al nome host potresti dover eseguire l'autenticazione, ad esempio facendo clic su **Log in with OpenShift** e autorizzando l'accesso alla tua identità IAM.

<br>
Ora sei nell'applicazione OpenShift integrata. Ad esempio, se sei in Grafana, potresti controllare l'utilizzo della CPU dello spazio dei nomi o altri grafici. Per accedere ad altri strumenti integrati, apri i loro nomi host di instradamento.

<br />


## Lezione 3: Distribuzione di un'applicazione al tuo cluster OpenShift
{: #openshift_deploy_app}

Con Red Hat OpenShift on IBM Cloud, puoi creare una nuova applicazione ed esporre il tuo servizio dell'applicazione tramite un router OpenShift per consentirne l'utilizzo ad utenti esterni.
{: shortdesc}

Se hai fatto una pausa dall'ultima lezione e hai avviato un nuovo terminale, assicurati di accedere di nuovo al tuo cluster. Apri la tua console OpenShift all'indirizzo `https://<master_URL>/console`. Ad esempio, `https://c0.containers.cloud.ibm.com:23652/console`. Quindi, fai clic sul tuo profilo **IAM#user.name@email.com > Copy Login Command** e incolla nel tuo terminale il comando di login `oc` copiato per eseguire l'autenticazione tramite la CLI.
{: tip}

1.  Crea un progetto per la tua applicazione Hello World. Un progetto è una versione OpenShift di uno spazio dei nomi Kubernetes con annotazioni aggiuntive.
    ```
    oc new-project hello-world
    ```
    {: pre}
2.  Crea l'applicazione di esempio [dal codice sorgente ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/IBM/container-service-getting-started-wt). Con il comando OpenShift `new-app`, puoi fare riferimento a una directory in un repository remoto che contiene il Dockerfile e il codice dell'applicazione per creare la tua immagine. Il comando crea l'immagine, la archivia nel registro Docker locale e crea le configurazioni di distribuzione dell'applicazione (`dc`) e i servizi (`svc`). Per ulteriori informazioni sulla creazione di nuove applicazioni, [consulta la documentazione OpenShift ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://docs.openshift.com/container-platform/3.11/dev_guide/application_lifecycle/new_app.html).
    ```
    oc new-app --name hello-world https://github.com/IBM/container-service-getting-started-wt --context-dir="Lab 1"
    ```
    {: pre}
3.  Verifica che i componenti dell'applicazione Hello World di esempio siano stato creati.
    1.  Controlla l'immagine **hello-world** nel registro Docker integrato del cluster accedendo alla console del registro nel tuo browser. Assicurati di aver aggiornato l'URL del provider della console del registro con `-e` come descritto nella lezione precedente.
        ```
        https://registry-console-default.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud/
        ```
        {: codeblock}
    2.  Elenca i servizi **hello-world** e prendi nota del nome servizio. La tua applicazione resta in ascolto del traffico su questi indirizzi IP interni del cluster a meno che non crei un instradamento per il servizio in modo che il router possa inoltrare le richieste di traffico esterno all'applicazione.
        ```
        oc get svc -n hello-world
        ```
        {: pre}

        Output di esempio:
        ```
        NAME          TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)    AGE
        hello-world   ClusterIP   172.21.xxx.xxx   <nones>       8080/TCP   31m
        ```
        {: screen}
    3.  Elenca i pod. I pod che hanno `build` nel nome sono lavori che sono stati **completati** come parte de processo di build della nuova applicazione. Assicura che lo stato del pod **hello-world** sia **Running**.
        ```
        oc get pods -n hello-world
        ```
        {: pre}

        Output di esempio:
        ```
        NAME                READY     STATUS             RESTARTS   AGE
        hello-world-9cv7d   1/1       Running            0          30m
        hello-world-build   0/1       Completed          0          31m
        ```
        {: screen}
4.  Configura un instradamento in modo che tu possa accedere pubblicamente al servizio {{site.data.keyword.toneanalyzershort}}. Per impostazione predefinita, il nome host è in formato `<service_name>-<namespace>.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud`. Se vuoi personalizzare il nome host, includi l'indicatore `--hostname=<hostname>`.
    1.  Crea un instradamento per il servizio **hello-world**.
        ```
        oc create route edge --service=hello-world -n hello-world
        ```
        {: pre}
    2.  Ottieni l'indirizzo del nome host di instradamento dall'output **Host/Port**.
        ```
        oc get route -n hello-world
        ```
        {: pre}
        Output di esempio:
        ```
        NAME          HOST/PORT                         PATH                                        SERVICES      PORT       TERMINATION   WILDCARD
        hello-world   hello-world-hello.world.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud    hello-world   8080-tcp   edge/Allow    None
        ```
        {: screen}
5.  Accedi alla tua applicazione.
    ```
    curl https://hello-world-hello-world.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud
    ```
    {: pre}
    
    Output di esempio:
    ```
    Hello world da hello-world-9cv7d! La tua applicazione è attiva e in esecuzione in un cluster!
    ```
    {: screen}
6.  **Facoltativo** Per ripulire le risorse che hai creato in questa lezione, puoi utilizzare le etichette assegnate a ciascuna applicazione.
    1.  Elenca tutte le risorse per ogni applicazione nel progetto `hello-world`.
        ```
        oc get all -l app=hello-world -o name -n hello-world
        ```
        {: pre}
        Output di esempio:
        ```
        pod/hello-world-1-dh2ff
        replicationcontroller/hello-world-1
        service/hello-world
        deploymentconfig.apps.openshift.io/hello-world
        buildconfig.build.openshift.io/hello-world
        build.build.openshift.io/hello-world-1
        imagestream.image.openshift.io/hello-world
        imagestream.image.openshift.io/node
        route.route.openshift.io/hello-world
        ```
        {: screen}
    2.  Elimina tutte le risorse che hai creato.
        ```
        oc delete all -l app=hello-world -n hello-world
        ```
        {: pre}



<br />


## Lezione 4: Configurazione dei componenti aggiuntivi LogDNA e Sysdig per monitorare l'integrità del cluster
{: #openshift_logdna_sysdig}

Poiché OpenShift configura per impostazione predefinita dei [vincoli del contesto di sicurezza (o SCC, Security Context Constraint) ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://docs.openshift.com/container-platform/3.11/admin_guide/manage_scc.html) più rigidi rispetto al sistema Kubernetes nativo, potresti notare che alcune applicazioni o alcuni componenti aggiuntivi del cluster che utilizzi sul Kubernetes nativo non possono essere distribuiti su OpenShift nello stesso modo. In particolare, molte immagini richiedono di essere eseguite come utente `root` o come contenitore privilegiato, cosa che viene impedita in OpenShift per impostazione predefinita. In questa lezione, imparerai come modificare gli SCC predefiniti creando account di sicurezza privilegiati e aggiornando il `securityContext` nella specifica del pod per utilizzare due popolari componenti aggiuntivi di {{site.data.keyword.containerlong_notm}}: {{site.data.keyword.la_full_notm}} e {{site.data.keyword.mon_full_notm}}.
{: shortdesc}

Prima di iniziare, accedi al tuo cluster come amministratore.
1.  Apri la tua console OpenShift all'indirizzo `https://<master_URL>/console`. Ad esempio, `https://c0.containers.cloud.ibm.com:23652/console`.
2.  Fai clic sul tuo profilo **IAM#user.name@email.com > Copy Login Command** e incolla nel tuo terminale il comando di login `oc` copiato per eseguire l'autenticazione tramite la CLI.
3.  Scarica i file di configurazione dell'amministratore per il tuo cluster.
    ```
    ibmcloud ks cluster-config --cluster <cluster_name_or_ID> --admin
    ```
    {: pre}
    
    Al termine del download dei file di configurazione, viene visualizzato un comando che puoi copiare e incollare per impostare il percorso del file di configurazione Kubernetes locale come variabile di ambiente.

    Esempio per OS X:

    ```
    export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-<datacenter>-<cluster_name>.yml
    ```
    {: screen}
4.  Continua la lezione per configurare [{{site.data.keyword.la_short}}](#openshift_logdna) e [{{site.data.keyword.mon_short}}](#openshift_sysdig).

### Lezione 4a: Configurazione di LogDNA
{: #openshift_logdna}

Configura un progetto e un account di servizio privilegiato per {{site.data.keyword.la_full_notm}}. Quindi, crea un'istanza {{site.data.keyword.la_short}} nel tuo account {{site.data.keyword.Bluemix_notm}}. Per integrare la tua istanza {{site.data.keyword.la_short}} con il cluster OpenShift, devi modificare la serie di daemon che viene distribuita per utilizzare l'account di servizio privilegiato per l'esecuzione come root.
{: shortdesc}

1.  Configura il progetto e l'account di servizio privilegiato per LogDNA.
    1.  Come amministratore del cluster, crea un progetto `logdna`.
        ```
        oc adm new-project logdna
        ```
        {: pre}
    2.  Indica il progetto come destinazione in modo che le successive risorse che crei siano nello spazio dei nomi del progetto `logdna`.
        ```
        oc project logdna
        ```
        {: pre}
    3.  Crea un account di servizio per il progetto `logdna`.
        ```
        oc create serviceaccount logdna
        ```
        {: pre}
    4.  Aggiungi un vincolo del contesto di sicurezza privilegiato all'account di servizio per il progetto `logdna`.<p class="note">Se vuoi controllare quale autorizzazione viene fornita dalla politica SCC `privileged` all'account di servizio, esegui `oc describe scc privileged`. Per ulteriori informazioni sui SCC, vedi la [documentazione di OpenShift ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://docs.openshift.com/container-platform/3.11/admin_guide/manage_scc.html).</p>
        ```
        oc adm policy add-scc-to-user privileged -n logdna -z logdna
        ```
        {: pre}
2.  Crea la tua istanza {{site.data.keyword.la_full_notm}} nello stesso gruppo di risorse del tuo cluster. Seleziona un piano dei prezzi che determina il periodo di conservazione per i tuoi log, ad esempio `lite` che conserva i log per 0 giorni. Non è necessario che la regione corrisponda alla regione del tuo cluster. Per ulteriori informazioni, vedi [Provisioning di un'istanza](/docs/services/Log-Analysis-with-LogDNA/tutorials?topic=LogDNA-provision) e [Piani dei prezzi](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-about#overview_pricing_plans).
    ```
    ibmcloud resource service-instance-create <service_instance_name> logdna (lite|7-days|14-days|30-days) <region> [-g <resource_group>]
    ```
    {: pre}
    
    Comando di esempio:
    ```
    ibmcloud resource service-instance-create logdna-openshift logdna lite us-south
    ```
    {: pre}
    
    Nell'output, prendi nota dell'**ID** dell'istanza del servizio, che è nel formato `crn:v1:bluemix:public:logdna:<region>:<ID_string>::`.
    ```
    Service instance <name> was created.
                 
    Name:         <name>   
    ID:           crn:v1:bluemix:public:logdna:<region>:<ID_string>::   
    GUID:         <guid>   
    Location:     <region>   
    ...
    ```
    {: screen}    
3.  Ottieni la chiave di inserimento della tua istanza {{site.data.keyword.la_short}}. La chiave di inserimento LogDNA viene utilizzata per aprire un socket web sicuro al server di inserimento LogDNA e per autenticare l'agent di registrazione presso il servizio {{site.data.keyword.la_short}}.
    1.  Crea una chiave di servizio per la tua istanza LogDNA.
        ```
        ibmcloud resource service-key-create <key_name> Administrator --instance-id <logdna_instance_ID>
        ```
        {: pre}
    2.  Prendi nota della **ingestion_key** della tua chiave di servizio.
        ```
        ibmcloud resource service-key <key_name>
        ```
        {: pre}
        
        Output di esempio:
        ```
        Name:          <key_name>  
        ID:            crn:v1:bluemix:public:logdna:<region>:<ID_string>::    
        Created At:    Thu Jun  6 21:31:25 UTC 2019   
        State:         active   
        Credentials:                                   
                       apikey:                   <api_key_value>      
                       iam_apikey_description:   Auto-generated for key <ID>     
                       iam_apikey_name:          <key_name>       
                       iam_role_crn:             crn:v1:bluemix:public:iam::::role:Administrator      
                       iam_serviceid_crn:        crn:v1:bluemix:public:iam-identity::<ID_string>     
                       ingestion_key:            111a11aa1a1a11a1a11a1111aa1a1a11    
        ```
        {: screen}
4.  Crea un segreto Kubernetes per archiviare la chiave di inserimento LogDNA per la tua istanza del servizio.
    ```
     oc create secret generic logdna-agent-key --from-literal=logdna-agent-key=<logDNA_ingestion_key>
    ```
    {: pre}
5.  Crea una serie di daemon Kubernetes per distribuire l'agent LogDNA su ogni nodo di lavoro del tuo cluster Kubernetes. L'agent LogDNA raccoglie i log con l'estensione `*.log` e i file senza estensione archiviati nella directory `/var/log` del tuo pod. Per impostazione predefinita, i log vengono raccolti da tutti gli spazi dei nomi, incluso `kube-system`, e vengono inoltrati automaticamente al servizio {{site.data.keyword.la_short}}.
    ```
    oc create -f https://assets.us-south.logging.cloud.ibm.com/clients/logdna-agent-ds.yaml
    ```
    {: pre}
6.  Modifica la configurazione della serie di daemon dell'agent LogDNA per fare riferimento all'account di servizio che hai creato in precedenza e per impostare il contesto di sicurezza su privilegiato.
    ```
    oc edit ds logdna-agent
    ```
    {: pre}
    
    Nel file di configurazione, aggiungi le seguenti specifiche.
    *   In `spec.template.spec`, aggiungi `serviceAccount: logdna`.
    *   In `spec.template.spec.containers`, aggiungi `securityContext: privileged: true`.
    *   Se hai creato la tua istanza {{site.data.keyword.la_short}} in una regione diversa da `us-south`, aggiorna i valori della variabile di ambiente `spec.template.spec.containers.env` relativi a `LDAPIHOST` e `LDLOGHOST` con `<region>`.

    Output di esempio:
    ```
    apiVersion: extensions/v1beta1
    kind: DaemonSet
    spec:
      ...
      template:
        ...
        spec:
          serviceAccount: logdna
          containers:
          - securityContext:
              privileged: true
            ...
            env:
            - name: LOGDNA_AGENT_KEY
              valueFrom:
                secretKeyRef:
                  key: logdna-agent-key
                  name: logdna-agent-key
            - name: LDAPIHOST
              value: api.<region>.logging.cloud.ibm.com
            - name: LDLOGHOST
              value: logs.<region>.logging.cloud.ibm.com
          ...
    ```
    {: screen}
7.  Verifica che il pod `logdna-agent` su ogni nodo sia in uno stato **Running**.
    ```
    oc get pods
    ```
    {: pre}
8.  Dalla [console {{site.data.keyword.Bluemix_notm}} Observability > Logging](https://cloud.ibm.com/observe/logging), nella riga relativa alla tua istanza {{site.data.keyword.la_short}}, fai clic su **View LogDNA**. Si apre il dashboard LogDNA e puoi iniziare ad analizzare i tuoi log.

Per ulteriori informazioni su come utilizzare {{site.data.keyword.la_short}}, vedi la [documentazione sui passi successivi](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-kube#kube_next_steps).

### Lezione 4b: Configurazione di Sysdig
{: #openshift_sysdig}

Crea un'istanza {{site.data.keyword.mon_full_notm}} nel tuo account {{site.data.keyword.Bluemix_notm}}. Per integrare la tua istanza {{site.data.keyword.mon_short}} con il cluster OpenShift, devi eseguire uno script che crea un progetto e un account di servizio privilegiato per l'agent Sysdig.
{: shortdesc}

1.  Crea la tua istanza {{site.data.keyword.mon_full_notm}} nello stesso gruppo di risorse del tuo cluster. Seleziona un piano dei prezzi che determina il periodo di conservazione per i tuoi log, ad esempio `lite`. Non è necessario che la regione corrisponda alla regione del tuo cluster. Per ulteriori informazioni, vedi [Provisioning di un'istanza](/docs/services/Monitoring-with-Sysdig?topic=Sysdig-provision).
    ```
    ibmcloud resource service-instance-create <service_instance_name> sysdig-monitor (lite|graduated-tier) <region> [-g <resource_group>]
    ```
    {: pre}
    
    Comando di esempio:
    ```
    ibmcloud resource service-instance-create sysdig-openshift sysdig-monitor lite us-south
    ```
    {: pre}
    
    Nell'output, prendi nota dell'**ID** dell'istanza del servizio, che è nel formato `crn:v1:bluemix:public:logdna:<region>:<ID_string>::`.
    ```
    Service instance <name> was created.
                 
    Name:         <name>   
    ID:           crn:v1:bluemix:public:sysdig-monitor:<region>:<ID_string>::   
    GUID:         <guid>   
    Location:     <region>   
    ...
    ```
    {: screen}    
2.  Ottieni la chiave di accesso della tua istanza {{site.data.keyword.mon_short}}. La chiave di accesso Sysdig viene utilizzata per aprire un socket web sicuro al server di inserimento Sysdig e per autenticare l'agent di monitoraggio presso il servizio {{site.data.keyword.mon_short}}.
    1.  Crea una chiave di servizio per la tua istanza Sysdig.
        ```
        ibmcloud resource service-key-create <key_name> Administrator --instance-id <sysdig_instance_ID>
        ```
        {: pre}
    2.  Prendi nota della **Chiave di accesso Sysdig** e dell'**Endpoint raccoglitore Sysdig** della tua chiave di servizio.
        ```
        ibmcloud resource service-key <key_name>
        ```
        {: pre}
        
        Output di esempio:
        ```
        Name:          <key_name>  
        ID:            crn:v1:bluemix:public:sysdig-monitor:<region>:<ID_string>::    
        Created At:    Thu Jun  6 21:31:25 UTC 2019   
        State:         active   
        Credentials:                                   
                       Sysdig Access Key:           11a1aa11-aa1a-1a1a-a111-11a11aa1aa11      
                       Sysdig Collector Endpoint:   ingest.<region>.monitoring.cloud.ibm.com      
                       Sysdig Customer Id:          11111      
                       Sysdig Endpoint:             https://<region>.monitoring.cloud.ibm.com  
                       apikey:                   <api_key_value>      
                       iam_apikey_description:   Auto-generated for key <ID>     
                       iam_apikey_name:          <key_name>       
                       iam_role_crn:             crn:v1:bluemix:public:iam::::role:Administrator      
                       iam_serviceid_crn:        crn:v1:bluemix:public:iam-identity::<ID_string>       
        ```
        {: screen}
3.  Esegui lo script per configurare un progetto `ibm-observe` con un account di servizio privilegiato e una serie di daemon Kubernetes per distribuire l'agent Sysdig su ogni nodo di lavoro del tuo cluster Kubernetes. L'agent Sysdig raccoglie metriche quali, ad esempio, l'utilizzo della CPU del nodo di lavoro, l'utilizzo della memoria del nodo di lavoro, il traffico HTTP da e verso i tuoi contenitori e i dati sui diversi componenti dell'infrastruttura. 

    Nel seguente comando, sostituisci <sysdig_access_key> e <sysdig_collector_endpoint> con i valori dalla chiave di servizio che hai creato in precedenza. Per <tag>, puoi associare le tag con il tuo agent Sysdig, ad esempio `role:service,location:us-south` per aiutarti ad identificare l'ambiente da cui provengono le metriche.

    ```
    curl -sL https://ibm.biz/install-sysdig-k8s-agent | bash -s -- -a <sysdig_access_key> -c <sysdig_collector_endpoint> -t <tag> -ac 'sysdig_capture_enabled: false' --openshift
    ```
    {: pre}
    
    Output di esempio: 
    ```
    * Detecting operating system
    * Downloading Sysdig cluster role yaml
    * Downloading Sysdig config map yaml
    * Downloading Sysdig daemonset v2 yaml
    * Creating project: ibm-observe
    * Creating sysdig-agent serviceaccount in project: ibm-observe
    * Creating sysdig-agent access policies
    * Creating sysdig-agent secret using the ACCESS_KEY provided
    * Retreiving the IKS Cluster ID and Cluster Name
    * Setting cluster name as <cluster_name>
    * Setting ibm.containers-kubernetes.cluster.id 1fbd0c2ab7dd4c9bb1f2c2f7b36f5c47
    * Updating agent configmap and applying to cluster
    * Setting tags
    * Setting collector endpoint
    * Adding additional configuration to dragent.yaml
    * Enabling Prometheus
    configmap/sysdig-agent created
    * Deploying the sysdig agent
    daemonset.extensions/sysdig-agent created
    ```
    {: screen}
        
4.  Verifica che i pod `sydig-agent` su ciascun nodo mostrino che i pod **1/1** sono pronti e che ogni pod abbia uno stato **Running**.
    ```
    oc get pods
    ```
    {: pre}
    
    Output di esempio:
    ```
    NAME                 READY     STATUS    RESTARTS   AGE
    sysdig-agent-qrbcq   1/1       Running   0          1m
    sysdig-agent-rhrgz   1/1       Running   0          1m
    ```
    {: screen}
5.  Dalla [console {{site.data.keyword.Bluemix_notm}} Observability > Monitoring](https://cloud.ibm.com/observe/logging), nella riga relativa alla tua istanza {{site.data.keyword.mon_short}}, fai clic su **View Sysdig**. Si apre il dashboard Sysdig e puoi iniziare ad analizzare le metriche del tuo cluster.

Per ulteriori informazioni su come utilizzare {{site.data.keyword.mon_short}}, vedi la [documentazione sui passi successivi](/docs/services/Monitoring-with-Sysdig?topic=Sysdig-kubernetes_cluster#kubernetes_cluster_next_steps).

### Facoltativo: ripulitura
{: #openshift_logdna_sysdig_cleanup}

Rimuovi le istanze {{site.data.keyword.la_short}} e {{site.data.keyword.mon_short}} dal tuo cluster e dall'account {{site.data.keyword.Bluemix_notm}}. Tieni presente che, a meno che non archivi i log e le metriche nell'[archiviazione persistente](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-archiving), non puoi accedere a queste informazioni dopo aver eliminato le istanze dal tuo account.
{: shortdesc}

1.  Ripulisci le istanze {{site.data.keyword.la_short}} e {{site.data.keyword.mon_short}} nel tuo cluster rimuovendo i progetti che hai creato per loro. Quando elimini un progetto vengono rimosse anche le sue risorse, come gli account di servizio e le serie di daemon.
    ```
    oc delete project logdna
    ```
    {: pre}
    ```
    oc delete project ibm-observe
    ```
    {: pre}
2.  Rimuovi l'istanza dal tuo account {{site.data.keyword.Bluemix_notm}}.
    *   [Rimozione di un'istanza {{site.data.keyword.la_short}}](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-remove).
    *   [Rimozione di un'istanza {{site.data.keyword.mon_short}}](/docs/services/Monitoring-with-Sysdig?topic=Sysdig-remove).

<br />


## Limitazioni
{: #openshift_limitations}

Red Hat OpenShift on IBM Cloud beta viene rilasciato con le seguenti limitazioni.
{: shortdesc}

**Cluster**:
*   Puoi creare solo i cluster standard e non i cluster gratuiti.
*   Le ubicazioni sono disponibili in due aree metropolitane multizona, Washington, DC e Londra. Le zone supportate sono `wdc04, wdc06, wdc07, lon04, lon05` e `lon06`.
*   Non puoi creare un cluster con nodi di lavoro che eseguono più sistemi operativi, come OpenShift su Red Hat Enterprise Linux e Kubernetes nativo su Ubuntu.
*   Il [cluster autoscaler](/docs/containers?topic=containers-ca) non è supportato perché richiede Kubernetes versione 1.12 o successive. OpenShift 3.11 include solo Kubernetes versione 1.11.



**Archiviazione**:
*   Sono supportate le archiviazioni di file, blocchi e oggetti cloud di {{site.data.keyword.Bluemix_notm}}. L'archiviazione definita dal software (o SDS, software-defined storage) Portworx non è supportata.
*   A causa del modo in cui l'archiviazione file NFS {{site.data.keyword.Bluemix_notm}} configura le autorizzazioni utente di Linux, potresti riscontrare degli errori quando utilizzi l'archiviazione file. In tal caso, potresti dover configurare i [Vincoli del contesto di sicurezza OpenShift ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://docs.openshift.com/container-platform/3.11/admin_guide/manage_scc.html) o utilizzare un tipo di archiviazione diverso.

**Rete**:
*   Calico viene utilizzato come provider delle politiche di rete al posto di OpenShift SDN.

**Componenti aggiuntivi, integrazioni e altri servizi**:
*   I componenti aggiuntivi di {{site.data.keyword.containerlong_notm}} come Istio, Knative e il terminale Kubernetes non sono disponibili.
*   I grafici Helm non sono certificati per funzionare nei cluster OpenShift, ad eccezione di {{site.data.keyword.Bluemix_notm}} Object Storage.
*   I cluster non vengono distribuiti con i segreti di pull dell'immagine per i domini `icr.io` di {{site.data.keyword.registryshort_notm}}. Puoi [creare i tuoi propri segreti di pull dell'immagine](/docs/containers?topic=containers-images#other_registry_accounts) oppure utilizzare il registro Docker integrato per i cluster OpenShift.

**Applicazioni**:
*   Per impostazione predefinita, OpenShift configura impostazioni di sicurezza più rigide rispetto al sistema Kubernetes nativo. Per ulteriori informazioni, consulta la documentazione di OpenShift relativa alla [Gestione dei vincoli del contesto di sicurezza (SCC) ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://docs.openshift.com/container-platform/3.11/admin_guide/manage_scc.html).
*   Ad esempio, le applicazioni configurate per l'esecuzione come root potrebbero non riuscire, con i pod in uno stato `CrashLoopBackOff`. Per risolvere questo problema, puoi modificare i vincoli del contesto di sicurezza predefiniti o utilizzare un'immagine che non viene eseguita come root.
*   OpenShift è configurato con un registro Docker locale per impostazione predefinita. Se vuoi utilizzare le immagini archiviate nei tuoi nomi di dominio `icr.io` {{site.data.keyword.registrylong_notm}} privati remoti, devi creare manualmente i segreti per ogni registro globale e regionale. Puoi [copiare i segreti `default-<region>-icr-io`](/docs/containers?topic=containers-images#copy_imagePullSecret) dallo spazio dei nomi `default` allo spazio dei nomi da cui vuoi estrarre le immagini o [creare il tuo proprio segreto](/docs/containers?topic=containers-images#other_registry_accounts). Quindi, [aggiungi il segreto di pull dell'immagine](/docs/containers?topic=containers-images#use_imagePullSecret) alla tua configurazione di distribuzione o all'account di servizio dello spazio dei nomi.
*   La console OpenShift viene utilizzata al posto del dashboard Kubernetes.

<br />


## Operazioni successive
{: #openshift_next}

Per ulteriori informazioni su come lavorare con le tue applicazioni e i tuoi servizi di instradamento, vedi la [Guida per sviluppatori OpenShift](https://docs.openshift.com/container-platform/3.11/dev_guide/index.html).

<br />


## Feedback e domande
{: #openshift_support}

Durante il periodo beta, i cluster Red Hat OpenShift on IBM Cloud non sono coperti dal supporto IBM né dal supporto Red Hat. Qualsiasi supporto fornito viene offerto per aiutarti a valutare il prodotto in preparazione per la sua disponibilità generale.
{: important}

Per qualsiasi domanda o feedback, pubblica il messaggio in Slack. 
*   Se sei un utente esterno, pubblica nel canale [#openshift ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://ibm-container-service.slack.com/messages/CKCJLJCH4). 
*   Se sei un dipendente IBM, utilizza il canale [#iks-openshift-users ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://ibm-argonauts.slack.com/messages/CJH0UPN2D).

Se non utilizzi un ID IBM per il tuo account {{site.data.keyword.Bluemix_notm}}, [richiedi un invito](https://bxcs-slack-invite.mybluemix.net/) a questo Slack.
{: tip}
