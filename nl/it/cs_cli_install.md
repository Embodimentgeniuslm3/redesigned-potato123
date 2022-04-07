---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: kubernetes, iks, ibmcloud, ic, ks, kubectl

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



# Configurazione della CLI e dell'API
{: #cs_cli_install}

Puoi usare l'API o la CLI {{site.data.keyword.containerlong}} per creare e gestire i tuoi cluster Kubernetes.
{:shortdesc}

## Installazione dei plug-in e della CLI IBM Cloud
{: #cs_cli_install_steps}

Installa le CLI richieste per creare e gestire i tuoi cluster Kubernetes in {{site.data.keyword.containerlong_notm}} e per distribuire le applicazioni inserite nel contenitore al tuo cluster.
{:shortdesc}

Questa attività include le informazioni per l'installazione di queste CLI e questi plugin:

-   CLI {{site.data.keyword.Bluemix_notm}}
-   Plugin {{site.data.keyword.containerlong_notm}}
-   Plugin {{site.data.keyword.registryshort_notm}}

Se invece desideri utilizzare la console {{site.data.keyword.Bluemix_notm}}, al termine della creazione del tuo cluster puoi eseguire i comandi CLI direttamente dal tuo browser Web nel [terminale Kubernetes](#cli_web).
{: tip}

<br>
Per installare le CLI:

1.  Installa la [CLI {{site.data.keyword.Bluemix_notm}} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](/docs/cli?topic=cloud-cli-getting-started#idt-prereq). L'installazione include:
    -   La CLI {{site.data.keyword.Bluemix_notm}} di base (`ibmcloud`).
    -   Il plug-in {{site.data.keyword.containerlong_notm}} (`ibmcloud ks`).
    -   Il plug-in {{site.data.keyword.registryshort_notm}} (`ibmcloud cr`). Utilizza questo plugin per configurare il tuo proprio spazio dei nomi in un multi-tenant, altamente disponibile e il registro delle immagini privato scalabile ospitato da IBM e per archiviare e condividere le immagini Docker con altri utenti. Le immagini Docker sono richieste per distribuire i contenitori in un cluster.
    -   La CLI Kubernetes (`kubectl`) che corrisponde alla versione predefinita: 1.13.6.<p class="note">Se intendi utilizzare un cluster che viene eseguito su una versione differente, potresti dover [installare tale versione della CLI Kubernetes separatamente](#kubectl). Se hai un cluster (OpenShift), [installi le CLI `oc` e `kubectl` insieme](#cli_oc).</p>
    -   La CLI Helm (`helm`). Potresti utilizzare Helm come un gestore pacchetti per installare i servizi {{site.data.keyword.Bluemix_notm}} e le applicazioni complesse nel tuo cluster tramite i grafici Helm. Devi comunque [configurare Helm](/docs/containers?topic=containers-helm) in ciascun cluster dove vuoi utilizzare Helm.

    Prevedi di utilizzare molto la CLI? Prova [Abilitazione del completamento automatico della shell per la CLI {{site.data.keyword.Bluemix_notm}} (solo Linux/MacOS)](/docs/cli/reference/ibmcloud?topic=cloud-cli-shell-autocomplete#shell-autocomplete-linux).
    {: tip}

2.  Accedi alla CLI {{site.data.keyword.Bluemix_notm}}. Immetti le tue credenziali {{site.data.keyword.Bluemix_notm}} quando richiesto.
    ```
    ibmcloud login
    ```
    {: pre}

    Se disponi di un ID federato, utilizza `ibmcloud login --sso` per accedere alla CLI {{site.data.keyword.Bluemix_notm}}. Immetti il tuo nome utente e usa l'URL fornito nell'output della CLI per richiamare la tua passcode monouso. Sai che hai un ID federato quando l'accesso ha esito negativo senza `--sso` e positivo con l'opzione `--sso`.
    {: tip}

3.  Verifica che il plug-in {{site.data.keyword.containerlong_notm}} e i plug-in {{site.data.keyword.registryshort_notm}} siano installati correttamente.
    ```
    ibmcloud plugin list
    ```
    {: pre}

    Output di esempio:
    ```
    Listing installed plug-ins...

    Plugin Name                            Version   Status        
    container-registry                     0.1.373     
    container-service/kubernetes-service   0.3.23   
    ```
    {: screen}

Per informazioni di riferimento su queste CLI, consulta la documentazione per questi strumenti.

-   [Comandi `ibmcloud`](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_cli#ibmcloud_cli)
-   [Comandi `ibmcloud ks`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli)
-   [Comandi `ibmcloud cr`](/docs/services/Registry?topic=container-registry-cli-plugin-containerregcli)

## Installazione della CLI Kubernetes (`kubectl`)
{: #kubectl}

Per visualizzare una versione locale del dashboard Kubernetes e per distribuire le applicazioni nei tuoi cluster, installa la CLI Kubernetes (`kubectl`). L'ultima versione stabile di `kubectl` viene installata con la CLI {{site.data.keyword.Bluemix_notm}} di base. Tuttavia, per utilizzare il tuo cluster devi installare la versione `major.minor` della CLI di Kubernetes che corrisponde alla versione `major.minor` del cluster Kubernetes che vuoi utilizzare. Se utilizzi una versione della CLI `kubectl` che non corrisponde almeno alla versione `major.minor` dei tuoi cluster, potresti riscontrare risultati imprevisti. Ad esempio, [Kubernetes non supporta ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/setup/version-skew-policy/) le versioni client `kubectl` con uno scarto di 2 o più versioni dalla versione del server (n +/- 2). Assicurati di mantenere aggiornate le versioni della CLI e dei cluster Kubernetes.
{: shortdesc}

Stai utilizzando un cluster OpenShift? Installa invece la CLI OpenShift Origin (`oc`), che viene fornita con `kubectl`. Se hai sia cluster Red Hat OpenShift on IBM Cloud che {{site.data.keyword.containershort_notm}} nativi Ubuntu, assicurati di utilizzare il file binario `kubectl` che corrisponde alla versione Kubernetes `major.minor` del tuo cluster.
{: tip}

1.  Se già hai un cluster, controlla che la versione della tua CLI `kubectl` client corrisponda alla versione del server API cluster.
    1.  [Accedi al tuo account. Se applicabile, specifica il gruppo di risorse appropriato. Imposta il contesto per il tuo cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
    2.  Confronta le versioni di client e server. Se il client non corrisponde al server, continua con il passo successivo. Se le versioni corrispondono, hai già installato la versione appropriata di `kubectl`.
        ```
        kubectl version --short
        ```
        {: pre}
2.  Scarica la versione `major.minor` della CLI Kubernetes che corrisponde alla versione `major.minor` del cluster Kubernetes che vuoi utilizzare. L'attuale versione Kubernetes predefinita per {{site.data.keyword.containerlong_notm}} è la 1.13.6.
    -   **OS X**:   [https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/darwin/amd64/kubectl ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/darwin/amd64/kubectl)
    -   **Linux**:   [https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/linux/amd64/kubectl ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/linux/amd64/kubectl)
    -   **Windows**: installa la CLI Kubernetes nella stessa directory della CLI {{site.data.keyword.Bluemix_notm}}. Questa configurazione ti consente di ridurre le modifiche al percorso file quando esegui i comandi in un secondo momento. [ https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/windows/amd64/kubectl.exe ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/windows/amd64/kubectl.exe)

3.  Se utilizzi OS X o Linux, completa la seguente procedura.
    1.  Sposta il file eseguibile nella directory `/usr/local/bin`.
        ```
        mv /<filepath>/kubectl /usr/local/bin/kubectl
        ```
        {: pre}

    2.  Assicurati che `/usr/local/bin` sia elencato nella tua variabile di sistema `PATH`. La variabile `PATH` contiene tutte le directory in cui il tuo sistema operativo può trovare i file eseguibili. Le directory elencate nella variabile `PATH` servono a diversi scopi. `/usr/local/bin` viene utilizzata per memorizzare i file eseguibili per il software che non fa parte del sistema operativo e che viene installato manualmente dall'amministratore di sistema.
        ```
        echo $PATH
        ```
        {: pre}
        Output CLI di esempio:
        ```
        /usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
        ```
        {: screen}

    3.  Rendi il file eseguibile.
        ```
        chmod +x /usr/local/bin/kubectl
        ```
        {: pre}
4.  **Facoltativo**: [abilita il completamento automatico per i comandi `kubectl` ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/tasks/tools/install-kubectl/#enabling-shell-autocompletion). La procedura varia a seconda della shell che utilizzi. 

Successivamente, avvia la [Creazione dei cluster Kubernetes dalla CLI con {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-clusters#clusters_cli_steps).

Per ulteriori informazioni sulla CLI Kubernetes, vedi la [documentazione di riferimento `kubectl` ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubectl.docs.kubernetes.io/).
{: note}

<br />


## Installazione dell'anteprima della CLI OpenShift Origin (`oc`) beta
{: #cli_oc}

[Red Hat OpenShift on IBM Cloud](/docs/containers?topic=containers-openshift_tutorial) è disponibile come versione beta per testare i cluster OpenShift.
{: preview}

Per visualizzare una versione locale del dashboard OpenShift e per distribuire le applicazioni nei tuoi cluster Red Hat OpenShift on IBM Cloud, installa la CLI OpenShift Origin (`oc`). La CLI `oc` include una versione corrispondente della CLI Kubernetes (`kubectl`). Per ulteriori informazioni, vedi la [documentazione di OpenShift ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://docs.openshift.com/container-platform/3.11/cli_reference/get_started_cli.html).
{: shortdesc}

Stai utilizzando sia cluster Red Hat OpenShift on IBM Cloud che {{site.data.keyword.containershort_notm}} nativi Ubuntu? La CLI `oc` viene fornita con entrambi i file binari `oc` e `kubectl` ma i tuoi cluster differenti potrebbero eseguire versioni differenti di Kubernetes, come ad esempio 1.11 su OpenShift e 1.13.6 su Ubuntu. Assicurati di utilizzare il file binario `kubectl` che corrisponde alla versione Kubernetes `major.minor` del tuo cluster.
{: note}

1.  [Scarica la CLI OpenShift Origin ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.okd.io/download.html) per la tua versione OpenShift e il tuo sistema operativo locali. La versione OpenShift predefinita corrente è 3.11.

2.  Se utilizzi Mac OS o Linux, completa la seguente procedura per aggiungere i file binari alla tua variabile di sistema `PATH`. Se utilizzi Windows, installa la CLI `oc` nella stessa directory della CLI {{site.data.keyword.Bluemix_notm}}. Questa configurazione ti consente di ridurre le modifiche al percorso file quando esegui i comandi in un secondo momento.
    1.  Sposta i file eseguibili `oc` e `kubectl` nella directory `/usr/local/bin`.
        ```
        mv /<filepath>/oc /usr/local/bin/oc && mv /<filepath>/kubectl /usr/local/bin/kubectl
        ```
        {: pre}

    2.  Assicurati che `/usr/local/bin` sia elencato nella tua variabile di sistema `PATH`. La variabile `PATH` contiene tutte le directory in cui il tuo sistema operativo può trovare i file eseguibili. Le directory elencate nella variabile `PATH` servono a diversi scopi. `/usr/local/bin` viene utilizzata per memorizzare i file eseguibili per il software che non fa parte del sistema operativo e che viene installato manualmente dall'amministratore di sistema.
        ```
        echo $PATH
        ```
        {: pre}
        Output CLI di esempio:
        ```
        /usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
        ```
        {: screen}
3.  **Facoltativo**: [abilita il completamento automatico per i comandi `kubectl` ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/tasks/tools/install-kubectl/#enabling-shell-autocompletion). La procedura varia a seconda della shell che utilizzi. Puoi ripetere la procedura per abilitare il completamento automatico per i comandi `oc`. Ad esempio, in bash su Linux, invece di `kubectl completion bash >/etc/bash_completion.d/kubectl`, puoi eseguire `oc completion bash >/etc/bash_completion.d/oc_completion`.

Avvia quindi [Creazione di un cluster Red Hat OpenShift on IBM Cloud (anteprima)](/docs/containers?topic=containers-openshift_tutorial).

Per ulteriori informazioni sulla CLI OpenShift Origin, vedi la [documentazione dei comandi `oc`![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://docs.openshift.com/container-platform/3.11/cli_reference/basic_cli_operations.html).
{: note}

<br />


## Esecuzione della CLI in un contenitore nel tuo computer
{: #cs_cli_container}

Invece di installare ogni CLI individualmente sul tuo computer, puoi installare le CLI in un contenitore che viene eseguito sul tuo computer.
{:shortdesc}

Prima di iniziare, [installa Docker ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.docker.com/community-edition#/download) per creare ed eseguire immagini localmente. Se stai utilizzando Windows 8 o precedenti, puoi invece installare [Docker Toolbox ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://docs.docker.com/toolbox/toolbox_install_windows/).

1. Crea un'immagine dal Dockerfile fornito.

    ```
    docker build -t <image_name> https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/install-clis-container/Dockerfile
    ```
    {: pre}

2. Distribuisci l'immagine localmente come un contenitore e monta un volume per accedere ai file locali.

    ```
    docker run -it -v /local/path:/container/volume <image_name>
    ```
    {: pre}

3. Inizia eseguendo i comandi `ibmcloud ks` e `kubectl` dalla shell interattiva. Se crei dati che vuoi salvare, salvali nel volume che hai montato. Quando esci dalla shell, il contenitore si arresta.

<br />



## Configurazione della CLI per eseguire `kubectl`
{: #cs_cli_configure}

Puoi utilizzare i comandi forniti con la CLI Kubernetes per gestire i cluster in {{site.data.keyword.Bluemix_notm}}.
{:shortdesc}

Tutti i comandi `kubectl` disponibili in Kubernetes 1.13.6 sono supportati per l'utilizzo con i cluster in {{site.data.keyword.Bluemix_notm}}. Dopo che hai creato un cluster, imposta il contesto per la tua CLI locale su tale cluster con una variabile di ambiente. Puoi quindi eseguire i comandi `kubectl` Kubernetes per lavorare con il tuo cluster in {{site.data.keyword.Bluemix_notm}}.

Prima di poter eseguire i comandi `kubectl`:
* [Installa le CLI richieste](#cs_cli_install).
* [Crea un
cluster](/docs/containers?topic=containers-clusters#clusters_cli_steps).
* Assicurati di avere un [ruolo del servizio](/docs/containers?topic=containers-users#platform) che concede il ruolo RBAC Kubernetes appropriato in modo che tu possa lavorare con le risorse Kubernetes. Se hai solo un ruolo del servizio ma nessun ruolo della piattaforma, è necessario che l'amministratore del cluster ti fornisca il nome e l'ID del cluster oppure il ruolo della piattaforma **Visualizzatore** per elencare i cluster.

Per utilizzare i comandi `kubectl`:

1.  Accedi alla CLI {{site.data.keyword.Bluemix_notm}}. Immetti le tue credenziali {{site.data.keyword.Bluemix_notm}} quando richiesto.

    ```
    ibmcloud login
    ```
    {: pre}

    Se disponi di un ID federato, utilizza `ibmcloud login --sso` per accedere alla CLI {{site.data.keyword.Bluemix_notm}}. Immetti il tuo nome utente e usa l'URL fornito nell'output della CLI per richiamare la tua passcode monouso. Sai che hai un ID federato quando l'accesso ha esito negativo senza `--sso` e positivo con l'opzione `--sso`.
    {: tip}

2.  Seleziona un account {{site.data.keyword.Bluemix_notm}}. Se sei assegnato a più organizzazioni {{site.data.keyword.Bluemix_notm}}, seleziona l'organizzazione
dove è stato creato il cluster. I cluster sono specifici di un'organizzazione, ma sono indipendenti da uno spazio {{site.data.keyword.Bluemix_notm}}. Pertanto, non devi selezionare uno spazio.

3.  Per creare e gestire i cluster in un gruppo di risorse diverso da quello predefinito, specifica tale gruppo di risorse. Per visualizzare il gruppo di risorse a cui appartiene ciascun cluster, esegui `ibmcloud ks clusters`. **Nota**: devi disporre dell'[accesso come **Visualizzatore**](/docs/containers?topic=containers-users#platform) al gruppo di risorse.
    ```
    ibmcloud target -g <resource_group_name>
    ```
    {: pre}

4.  Elenca tutti i cluster nell'account per ottenere il nome del cluster. Se hai solo un ruolo del servizio {{site.data.keyword.Bluemix_notm}} IAM e non puoi visualizzare i cluster, chiedi al tuo amministratore cluster di fornirti il ruolo di **Visualizzatore** della piattaforma IAM oppure il nome e l'ID del cluster.

    ```
    ibmcloud ks clusters
    ```
    {: pre}

5.  Configura il cluster che hai creato come il contesto per questa sessione. Completa questi passi di configurazione ogni volta che lavori con il tuo cluster.
    1.  Richiama il comando per impostare la variabile di ambiente e scaricare i file di configurazione Kubernetes. <p class="tip">Stai utilizzando Windows PowerShell? Includi l'indicatore `--powershell` per ottenere le variabili di ambiente in formato Windows PowerShell.</p>
        ```
        ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
        ```
        {: pre}

        Dopo aver scaricato i file di configurazione, viene visualizzato un comando che puoi utilizzare per impostare il percorso al file di configurazione di Kubernetes locale come una variabile di ambiente.

        Esempio:

        ```
        export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/mycluster/kube-config-prod-dal10-mycluster.yml
        ```
        {: screen}

    2.  Copia e incolla il comando visualizzato nel tuo terminale per impostare la variabile di ambiente `KUBECONFIG`.

        **Utente Mac o Linux**: invece di eseguire il comando `ibmcloud ks cluster-config` e di copiare la variabile di ambiente `KUBECONFIG`, puoi eseguire `ibmcloud ks cluster-config --export <cluster-name>`. A seconda della tua shell, puoi configurarla eseguendo `eval $(ibmcloud ks cluster-config --export <cluster-name>)`.
        {: tip}

    3.  Verifica che la variabile di ambiente `KUBECONFIG` sia impostata correttamente.

        Esempio:

        ```
        echo $KUBECONFIG
        ```
        {: pre}

        Output:
        ```
        /Users/<user_name>/.bluemix/plugins/container-service/clusters/mycluster/kube-config-prod-dal10-mycluster.yml
        ```
        {: screen}

6.  Verifica che i comandi `kubectl` siano eseguiti correttamente con il tuo cluster controllando la versione server della CLI Kubernetes.

    ```
    kubectl version --short
    ```
    {: pre}

    Output di esempio:

    ```
    Client Version: v1.13.6
    Server Version: v1.13.6
    ```
    {: screen}

Ora puoi eseguire i comandi `kubectl` per gestire i tuoi cluster in {{site.data.keyword.Bluemix_notm}}. Per un elenco completo dei comandi, consulta la [Documentazione Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubectl.docs.kubernetes.io/).

**Suggerimento:** se stai utilizzando Windows e la CLI Kubernetes non è installata nella stessa directory della CLI {{site.data.keyword.Bluemix_notm}}, devi modificare le directory al percorso in cui è installata la CLI Kubernetes per eseguire correttamente i comandi `kubectl`.


<br />




## Aggiornamento della CLI
{: #cs_cli_upgrade}

Per utilizzare le nuove funzioni, ti consigliamo di aggiornare periodicamente le CLI.
{:shortdesc}

Questa attività include le informazioni per aggiornare queste CLI.

-   Versione CLI {{site.data.keyword.Bluemix_notm}} 0.8.0 o successive
-   Plugin {{site.data.keyword.containerlong_notm}}
-   CLI Kubernetes versione 1.13.6 o successive
-   Plugin {{site.data.keyword.registryshort_notm}}

<br>
Per aggiornare le CLI:

1.  Aggiorna la CLI {{site.data.keyword.Bluemix_notm}}. Scarica la [versione più recente ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](/docs/cli?topic=cloud-cli-getting-started) ed esegui il programma di installazione.

2. Accedi alla CLI {{site.data.keyword.Bluemix_notm}}. Immetti le tue credenziali {{site.data.keyword.Bluemix_notm}} quando richiesto.

    ```
    ibmcloud login
    ```
    {: pre}

     Se disponi di un ID federato, utilizza `ibmcloud login --sso` per accedere alla CLI {{site.data.keyword.Bluemix_notm}}. Immetti il tuo nome utente e usa l'URL fornito nell'output della CLI per richiamare la tua passcode monouso. Sai che hai un ID federato quando l'accesso ha esito negativo senza `--sso` e positivo con l'opzione `--sso`.
     {: tip}

3.  Aggiorna il plugin {{site.data.keyword.containerlong_notm}}.
    1.  Installa l'aggiornamento dal repository di plugin {{site.data.keyword.Bluemix_notm}}.

        ```
        ibmcloud plugin update container-service 
        ```
        {: pre}

    2.  Verifica l'installazione del plugin immettendo il seguente comando e controllando l'elenco dei plugin installati.

        ```
        ibmcloud plugin list
        ```
        {: pre}

        Il plugin {{site.data.keyword.containerlong_notm}} viene visualizzato nei risultati come container-service.

    3.  Inizializza la CLI.

        ```
        ibmcloud ks init
        ```
        {: pre}

4.  [Aggiorna la CLI Kubernetes](#kubectl).

5.  Aggiorna il plugin {{site.data.keyword.registryshort_notm}}.
    1.  Installa l'aggiornamento dal repository di plugin {{site.data.keyword.Bluemix_notm}}.

        ```
        ibmcloud plugin update container-registry 
        ```
        {: pre}

    2.  Verifica l'installazione del plugin immettendo il seguente comando e controllando l'elenco dei plugin installati.

        ```
        ibmcloud plugin list
        ```
        {: pre}

        Il plugin registro viene visualizzato nei risultati come container-registry.

<br />


## Disinstallazione della CLI
{: #cs_cli_uninstall}

Se non hai più bisogno della CLI, puoi disinstallarla.
{:shortdesc}

Questa attività include le informazioni per rimuovere queste CLI:


-   Plugin {{site.data.keyword.containerlong_notm}}
-   CLI Kubernetes
-   Plugin {{site.data.keyword.registryshort_notm}}

Per disinstallare le CLI:

1.  Disinstalla il plugin {{site.data.keyword.containerlong_notm}}.

    ```
    ibmcloud plugin uninstall container-service
    ```
    {: pre}

2.  Disinstalla il plugin {{site.data.keyword.registryshort_notm}}.

    ```
    ibmcloud plugin uninstall container-registry
    ```
    {: pre}

3.  Verifica che i plugin siano stati disinstallati eseguendo questo comando e controllando l'elenco dei plugin installati.

    ```
    ibmcloud plugin list
    ```
    {: pre}

    I plugin container-service e container-registry non sono visualizzati nei risultati.

<br />


## Utilizzo del terminale Kubernetes nel tuo browser Web (beta)
{: #cli_web}

Il terminale Kubernetes ti consente di utilizzare la CLI {{site.data.keyword.Bluemix_notm}} per gestire il tuo cluster direttamente dal tuo browser Web.
{: shortdesc}

Il terminale Kubernetes viene rilasciato come componente aggiuntivo {{site.data.keyword.containerlong_notm}} beta e potrebbe essere modificato a seguito dei feedback degli utenti e di ulteriori test. Non utilizzare questa funzione nei cluster di produzione per evitare effetti secondari imprevisti.
{: important}

Se utilizzi il dashboard cluster nella console {{site.data.keyword.Bluemix_notm}} per gestire i tuoi cluster, ma vuoi apportare velocemente modifiche di configurazione più avanzate, puoi ora eseguire i comandi CLI direttamente dal tuo browser Web nel terminale Kubernetes. Il terminale Kubernetes è abilitato con la CLI [{{site.data.keyword.Bluemix_notm}}{{site.data.keyword.Bluemix_notm}} di base ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](/docs/cli?topic=cloud-cli-getting-started) e i plugin {{site.data.keyword.containerlong_notm}} e {{site.data.keyword.registryshort_notm}}. Inoltre, il contesto del terminale è già impostato sul cluster che stai utilizzando, cosicché tu possa eseguire i comandi Kubernetes `kubectl` per utilizzare il cluster.

Tutti i file che scarichi e modifichi localmente, come i file YAML, vengono memorizzati temporaneamente in Kubernetes Terminal e non vengono conservati tra le sessioni.
{: note}

Per installare e avviare il terminale Kubernetes:

1.  Accedi alla [console {{site.data.keyword.Bluemix_notm}}](https://cloud.ibm.com/).
2.  Dalla barra dei menu, seleziona l'account che vuoi utilizzare.
3.  Dal menu ![Icona Menu](../icons/icon_hamburger.svg "Icona Menu"), fai clic su **Kubernetes**.
4.  Nella pagina **Cluster**, fai clic sul cluster a cui vuoi accedere.
5.  Dalla pagina dei dettagli del cluster, fai clic sul pulsante **Terminale**.
6.  Fai clic su **Installa**. L'installazione del componente aggiuntivo del terminale potrebbe richiedere alcuni minuti.
7.  Fai nuovamente clic sul pulsante **Terminale**. Il terminale si apre nel tuo browser.

La prossima volta puoi avviare il terminale Kubernetes semplicemente facendo clic sul pulsante **Terminale**.

<br />


## Automazione delle distribuzioni dei cluster con l'API
{: #cs_api}

Puoi utilizzare l'API {{site.data.keyword.containerlong_notm}} per automatizzare la creazione, la distribuzione e la gestione dei tuoi cluster Kubernetes.
{:shortdesc}

L'API {{site.data.keyword.containerlong_notm}} richiede informazioni di intestazione che devi fornire nella tua richiesta API e che possono variare in base all'API che vuoi utilizzare. Per determinare quali informazioni di intestazione sono necessarie per la tua API, vedi la [documentazione dell'API {{site.data.keyword.containerlong_notm}} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://us-south.containers.cloud.ibm.com/swagger-api).

Per l'autenticazione con {{site.data.keyword.containerlong_notm}}, devi fornire un token {{site.data.keyword.Bluemix_notm}} IAM (Identity and Access Management) che viene generato con le tue credenziali {{site.data.keyword.Bluemix_notm}} e che include l'ID dell'account {{site.data.keyword.Bluemix_notm}} in cui è stato creato il cluster. A seconda del modo con cui ti autentichi con {{site.data.keyword.Bluemix_notm}}, puoi scegliere tra le seguenti opzioni per automatizzare la creazione del tuo token {{site.data.keyword.Bluemix_notm}} IAM.

Puoi anche utilizzare il [file JSON API swagger ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://containers.cloud.ibm.com/global/swagger-global-api/swagger.json) per generare un client che possa interagire con l'API come parte del tuo lavoro di automazione.
{: tip}

<table summary="Tipi di ID e opzioni con parametro di input nella colonna 1 e valore nella colonna 2.">
<caption>Opzioni e tipi di ID</caption>
<thead>
<th>ID {{site.data.keyword.Bluemix_notm}}</th>
<th>Opzioni personali</th>
</thead>
<tbody>
<tr>
<td>ID non federato</td>
<td><ul><li><strong>Genera una chiave API {{site.data.keyword.Bluemix_notm}}:</strong> come alternativa all'utilizzo di nome utente e password {{site.data.keyword.Bluemix_notm}}, puoi <a href="/docs/iam?topic=iam-userapikey#create_user_key" target="_blank">utilizzare le chiavi API {{site.data.keyword.Bluemix_notm}}</a>. Le chiavi API {{site.data.keyword.Bluemix_notm}} dipendono dall'account {{site.data.keyword.Bluemix_notm}} per il quale vengono generate. Non puoi combinare la tua chiave API {{site.data.keyword.Bluemix_notm}} con un ID account diverso nello stesso token {{site.data.keyword.Bluemix_notm}} IAM. Per accedere ai cluster che sono stati creati con un account diverso da quello su cui si basa la tua chiave API {{site.data.keyword.Bluemix_notm}}, devi accedere all'account per generare una nuova chiave API.</li>
<li><strong>Nome utente e password {{site.data.keyword.Bluemix_notm}}:</strong> puoi seguire i passi di questo argomento per automatizzare completamente la creazione del tuo token di accesso {{site.data.keyword.Bluemix_notm}} IAM.</li></ul>
</tr>
<tr>
<td>ID federato</td>
<td><ul><li><strong>Genera una chiave API {{site.data.keyword.Bluemix_notm}}:</strong> <a href="/docs/iam?topic=iam-userapikey#create_user_key" target="_blank">le chiavi API {{site.data.keyword.Bluemix_notm}}</a> dipendono dall'account {{site.data.keyword.Bluemix_notm}} per il quale vengono generate. Non puoi combinare la tua chiave API {{site.data.keyword.Bluemix_notm}} con un ID account diverso nello stesso token {{site.data.keyword.Bluemix_notm}} IAM. Per accedere ai cluster che sono stati creati con un account diverso da quello su cui si basa la tua chiave API {{site.data.keyword.Bluemix_notm}}, devi accedere all'account per generare una nuova chiave API.</li>
<li><strong>Utilizza un passcode monouso: </strong> se ti autentichi con {{site.data.keyword.Bluemix_notm}} utilizzando un passcode monouso, non puoi automatizzare completamente la creazione del tuo token {{site.data.keyword.Bluemix_notm}} IAM perché il recupero del tuo passcode monouso richiede un'interazione manuale con il tuo browser web. Per automatizzare completamente la creazione del tuo token {{site.data.keyword.Bluemix_notm}} IAM, devi creare invece una chiave API {{site.data.keyword.Bluemix_notm}}.</ul></td>
</tr>
</tbody>
</table>

1.  Crea il tuo token di accesso {{site.data.keyword.Bluemix_notm}} IAM. Le informazioni sul corpo incluse nella tue richiesta variano in base al metodo di autenticazione {{site.data.keyword.Bluemix_notm}} che utilizzi.

    ```
    POST https://iam.bluemix.net/identity/token
    ```
    {: codeblock}

    <table summary="Parametri di input per richiamare i token IAM con il parametro di input della colonna 1 e il valore della colonna 2.">
    <caption>Parametri di input per ottenere i token IAM.</caption>
    <thead>
        <th>Parametri di input</th>
        <th>Valori</th>
    </thead>
    <tbody>
    <tr>
    <td>Intestazione</td>
    <td><ul><li>Content-Type: application/x-www-form-urlencoded</li> <li>Authorization: Basic Yng6Yng=<p><strong>Nota</strong>: <code>Yng6Yng=</code> è uguale all'autorizzazione con codifica URL per il nome utente <strong>bx</strong> e la password <strong>bx</strong>.</p></li></ul>
    </td>
    </tr>
    <tr>
    <td>Corpo per il nome utente e la password {{site.data.keyword.Bluemix_notm}}.</td>
    <td><ul><li>`grant_type: password`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`username`: il tuo nome utente {{site.data.keyword.Bluemix_notm}}.</li>
    <li>`password`: la tua password {{site.data.keyword.Bluemix_notm}}.</li>
    <li>`uaa_client_id: cf`</li>
    <li>`uaa_client_secret:`</br><strong>Nota</strong>: aggiungi la chiave <code>uaa_client_secret</code> senza specificare alcun valore.</li></ul></td>
    </tr>
    <tr>
    <td>Corpo per le chiavi API {{site.data.keyword.Bluemix_notm}}</td>
    <td><ul><li>`grant_type: urn:ibm:params:oauth:grant-type:apikey`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`apikey`: la tua chiave API {{site.data.keyword.Bluemix_notm}}</li>
    <li>`uaa_client_id: cf`</li>
    <li>`uaa_client_secret:` </br><strong>Nota</strong>: aggiungi la chiave <code>uaa_client_secret</code> senza specificare alcun valore.</li></ul></td>
    </tr>
    <tr>
    <td>Corpo per il passcode monouso {{site.data.keyword.Bluemix_notm}}</td>
      <td><ul><li><code>grant_type: urn:ibm:params:oauth:grant-type:passcode</code></li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`passcode`: il tuo passcode monouso {{site.data.keyword.Bluemix_notm}}. Esegui `ibmcloud login --sso` e segui le istruzioni nel tuo output della CLI per richiamare il tuo passcode monouso utilizzando il tuo browser web.</li>
    <li>`uaa_client_id: cf`</li>
    <li>`uaa_client_secret:` </br><strong>Nota</strong>: aggiungi la chiave <code>uaa_client_secret</code> senza specificare alcun valore.</li></ul>
    </td>
    </tr>
    </tbody>
    </table>

    Output di esempio per l'utilizzo di una chiave API:

    ```
    {
    "access_token": "<iam_access_token>",
    "refresh_token": "<iam_refresh_token>",
    "uaa_token": "<uaa_token>",
    "uaa_refresh_token": "<uaa_refresh_token>",
    "token_type": "Bearer",
    "expires_in": 3600,
    "expiration": 1493747503
    "scope": "ibm openid"
    }

    ```
    {: screen}

    Puoi trovare il token {{site.data.keyword.Bluemix_notm}} IAM nel campo **access_token** dell'output dell'API. Prendi nota del token {{site.data.keyword.Bluemix_notm}} IAM per richiamare ulteriori informazioni di intestazione nei passi successivi.

2.  Richiama l'ID dell'account {{site.data.keyword.Bluemix_notm}} che desideri utilizzare. Sostituisci `<iam_access_token>` con il token IAM {{site.data.keyword.Bluemix_notm}} che hai richiamato dal campo **access_token** del tuo output API nel passo precedente. Nel
tuo output API, puoi trovare l'ID del tuo account {{site.data.keyword.Bluemix_notm}} nel campo **resources.metadata.guid**.

    ```
    GET https://accountmanagement.ng.bluemix.net/v1/accounts
    ```
    {: codeblock}

    <table summary="Parametri di input per ottenere l'ID account {{site.data.keyword.Bluemix_notm}} coni l parametro di input della colonna 1 e il valore della colonna 2.">
    <caption>Parametri di input per ottenere un ID account {{site.data.keyword.Bluemix_notm}}.</caption>
    <thead>
  	<th>Parametri di input</th>
  	<th>Valori</th>
    </thead>
    <tbody>
  	<tr>
  		<td>Intestazioni</td>
      <td><ul><li><code>Content-Type: application/json</code></li>
        <li>`Authorization: bearer <iam_access_token>`</li>
        <li><code>Accept: application/json</code></li></ul></td>
  	</tr>
    </tbody>
    </table>

    Output di esempio:

    ```
    {
      "total_results": 3,
      "total_pages": 1,
      "prev_url": null,
      "next_url": null,
      "resources":
        {
          "metadata": {
            "guid": "<account_ID>",
            "url": "/v1/accounts/<account_ID>",
            "created_at": "2016-01-07T18:55:09.726Z",
            "updated_at": "2017-04-28T23:46:03.739Z",
            "origin": "BSS"
    ...
    ```
    {: screen}

3.  Genera un nuovo token {{site.data.keyword.Bluemix_notm}} IAM che include le tue credenziali {{site.data.keyword.Bluemix_notm}} e l'ID account che desideri utilizzare.

    Se utilizzi una chiave API {{site.data.keyword.Bluemix_notm}}, devi usare l'ID account {{site.data.keyword.Bluemix_notm}} per il quale è stata creata la chiave API. Per accedere ai cluster in altri account, collegati a questo account e crea una chiave API {{site.data.keyword.Bluemix_notm}} basata su questo account.
    {: note}

    ```
    POST https://iam.bluemix.net/identity/token
    ```
    {: codeblock}

    <table summary="Parametri di input per richiamare i token IAM con il parametro di input della colonna 1 e il valore della colonna 2.">
    <caption>Parametri di input per ottenere i token IAM.</caption>
    <thead>
        <th>Parametri di input</th>
        <th>Valori</th>
    </thead>
    <tbody>
    <tr>
    <td>Intestazione</td>
    <td><ul><li>`Content-Type: application/x-www-form-urlencoded`</li> <li>`Authorization: Basic Yng6Yng=`<p><strong>Nota</strong>: <code>Yng6Yng=</code> è uguale all'autorizzazione con codifica URL per il nome utente <strong>bx</strong> e la password <strong>bx</strong>.</p></li></ul>
    </td>
    </tr>
    <tr>
    <td>Corpo per il nome utente e la password {{site.data.keyword.Bluemix_notm}}.</td>
    <td><ul><li>`grant_type: password`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`username`: il tuo nome utente {{site.data.keyword.Bluemix_notm}}. </li>
    <li>`password`: la tua password {{site.data.keyword.Bluemix_notm}}. </li>
    <li>`uaa_client_ID: cf`</li>
    <li>`uaa_client_secret:` </br><strong>Nota</strong>: aggiungi la chiave <code>uaa_client_secret</code> senza specificare alcun valore.</li>
    <li>`bss_account`: l'ID account {{site.data.keyword.Bluemix_notm}} che hai richiamato nel passo precedente.</li></ul>
    </td>
    </tr>
    <tr>
    <td>Corpo per le chiavi API {{site.data.keyword.Bluemix_notm}}</td>
    <td><ul><li>`grant_type: urn:ibm:params:oauth:grant-type:apikey`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`apikey`: la tua chiave API {{site.data.keyword.Bluemix_notm}}.</li>
    <li>`uaa_client_ID: cf`</li>
    <li>`uaa_client_secret:` </br><strong>Nota</strong>: aggiungi la chiave <code>uaa_client_secret</code> senza specificare alcun valore.</li>
    <li>`bss_account`: l'ID account {{site.data.keyword.Bluemix_notm}} che hai richiamato nel passo precedente.</li></ul>
      </td>
    </tr>
    <tr>
    <td>Corpo per il passcode monouso {{site.data.keyword.Bluemix_notm}}</td>
    <td><ul><li>`grant_type: urn:ibm:params:oauth:grant-type:passcode`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`passcode`: il tuo passcode {{site.data.keyword.Bluemix_notm}}. </li>
    <li>`uaa_client_ID: cf`</li>
    <li>`uaa_client_secret:` </br><strong>Nota</strong>: aggiungi la chiave <code>uaa_client_secret</code> senza specificare alcun valore.</li>
    <li>`bss_account`: l'ID account {{site.data.keyword.Bluemix_notm}} che hai richiamato nel passo precedente.</li></ul></td>
    </tr>
    </tbody>
    </table>

    Output di esempio:

    ```
    {
      "access_token": "<iam_token>",
      "refresh_token": "<iam_refresh_token>",
      "token_type": "Bearer",
      "expires_in": 3600,
      "expiration": 1493747503
    }

    ```
    {: screen}

    Puoi trovare il token {{site.data.keyword.Bluemix_notm}} IAM nel campo **access_token** e il token di aggiornamento nel campo **refresh_token** dell'output dell'API.

4.  Elenca le regioni {{site.data.keyword.containerlong_notm}} disponibili e seleziona quella che vuoi utilizzare. Utilizza il token di accesso IAM e il token di aggiornamento dal passo precedente per creare le informazioni di intestazione.
    ```
    GET https://containers.cloud.ibm.com/v1/regions
    ```
    {: codeblock}

    <table summary="Parametri di input per richiamare le regioni {{site.data.keyword.containerlong_notm}} con il parametro di input della colonna 1 e il valore della colonna 2.">
    <caption>Parametri di input per richiamare le regioni {{site.data.keyword.containerlong_notm}}.</caption>
    <thead>
    <th>Parametri di input</th>
    <th>Valori</th>
    </thead>
    <tbody>
    <tr>
    <td>Intestazione</td>
    <td><ul><li>`Authorization: bearer <iam_token>`</li>
    <li>`X-Auth-Refresh-Token: <refresh_token>`</li></ul></td>
    </tr>
    </tbody>
    </table>

    Output di esempio:
    ```
    {
    "regions": [
        {
            "name": "ap-north",
            "alias": "jp-tok",
            "cfURL": "api.au-syd.bluemix.net",
            "freeEnabled": false
        },
        {
            "name": "ap-south",
            "alias": "au-syd",
            "cfURL": "api.au-syd.bluemix.net",
            "freeEnabled": true
        },
        {
            "name": "eu-central",
            "alias": "eu-de",
            "cfURL": "api.eu-de.bluemix.net",
            "freeEnabled": true
        },
        {
            "name": "uk-south",
            "alias": "eu-gb",
            "cfURL": "api.eu-gb.bluemix.net",
            "freeEnabled": true
        },
        {
            "name": "us-east",
            "alias": "us-east",
            "cfURL": "api.ng.bluemix.net",
            "freeEnabled": false
        },
        {
            "name": "us-south",
            "alias": "us-south",
            "cfURL": "api.ng.bluemix.net",
            "freeEnabled": true
        }
    ]
    }
    ```
    {: screen}

5.  Elenca tutti i cluster della regione {{site.data.keyword.containerlong_notm}} che hai selezionato. Se desideri [eseguire le richieste API Kubernetes per il tuo cluster](#kube_api), assicurati di annotare l'**id** e la **regione** del cluster.

     ```
     GET https://containers.cloud.ibm.com/v1/clusters
     ```
     {: codeblock}

     <table summary="Parametri di input per l'utilizzo dell'API {{site.data.keyword.containerlong_notm}} con il parametro di input della colonna 1 e il valore della colonna 2.">
     <caption>Parametri di input per l'utilizzo dell'API {{site.data.keyword.containerlong_notm}}.</caption>
     <thead>
     <th>Parametri di input</th>
     <th>Valori</th>
     </thead>
     <tbody>
     <tr>
     <td>Intestazione</td>
     <td><ul><li>`Authorization: bearer <iam_token>`</li>
       <li>`X-Auth-Refresh-Token: <refresh_token>`</li><li>`X-Region: <region>`</li></ul></td>
     </tr>
     </tbody>
     </table>

5.  Consulta la [Documentazione API {{site.data.keyword.containerlong_notm}} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://containers.cloud.ibm.com/global/swagger-global-api) per trovare un elenco di API supportate.

<br />


## Utilizzo del tuo cluster attraverso l'API Kubernetes
{: #kube_api}

Puoi utilizzare l'[API Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/reference/using-api/api-overview/) per interagire con il tuo cluster in {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

Le seguenti istruzioni richiedono l'accesso alla rete pubblica nel tuo cluster per connettersi all'endpoint del servizio pubblico del tuo master Kubernetes.
{: note}

1. Segui la procedura descritta in [Automazione delle distribuzioni dei cluster con l'API](#cs_api) per richiamare i tuoi token di accesso {{site.data.keyword.Bluemix_notm}} IAM e di aggiornamento, l'ID del cluster in cui desideri eseguire le richieste API Kubernetes e la regione {{site.data.keyword.containerlong_notm}} in cui è ubicato il cluster.

2. Richiama un token di aggiornamento delegato {{site.data.keyword.Bluemix_notm}} IAM.
   ```
   POST https://iam.bluemix.net/identity/token
   ```
   {: codeblock}

   <table summary="Parametri di input per richiamare un token di aggiornamento delegato IAM con il parametro di input della colonna 1 e il valore della colonna 2.">
   <caption>Parametri di input per ottenere un token di aggiornamento delegato IAM. </caption>
   <thead>
   <th>Parametri di input</th>
   <th>Valori</th>
   </thead>
   <tbody>
   <tr>
   <td>Intestazione</td>
   <td><ul><li>`Content-Type: application/x-www-form-urlencoded`</li> <li>`Authorization: Basic Yng6Yng=`</br><strong>Nota</strong>: <code>Yng6Yng=</code> è uguale all'autorizzazione con codifica URL per il nome utente <strong>bx</strong> e la password <strong>bx</strong>.</li><li>`cache-control: no-cache`</li></ul>
   </td>
   </tr>
   <tr>
   <td>Corpo</td>
   <td><ul><li>`delegated_refresh_token_expiry: 600`</li>
   <li>`receiver_client_ids: kube`</li>
   <li>`response_type: delegated_refresh_token` </li>
   <li>`refresh_token`: il tuo token di aggiornamento {{site.data.keyword.Bluemix_notm}} IAM. </li>
   <li>`grant_type: refresh_token`</li></ul></td>
   </tr>
   </tbody>
   </table>

   Output di esempio:
   ```
   {
    "delegated_refresh_token": <delegated_refresh_token>
   }
   ```
   {: screen}

3. Richiama un token ID {{site.data.keyword.Bluemix_notm}} IAM, un token di accesso IAM e un token di aggiornamento IAM utilizzando il token di aggiornamento delegato del passo precedente. Nell'output dell'API sono riportati il token ID IAM, nel campo **id_token**, il token di accesso IAM, nel campo **access_token**, e il token di aggiornamento IAM, nel campo **refresh_token**.
   ```
   POST https://iam.bluemix.net/identity/token
   ```
   {: codeblock}

   <table summary="Parametri di input per l'ottenimento dei token di accesso IAM e ID IAM con il parametro di input della colonna 1 e il valore della colonna 2.">
   <caption>Parametri di input per l'ottenimento dei token di accesso IAM e ID IAM.</caption>
   <thead>
   <th>Parametri di input</th>
   <th>Valori</th>
   </thead>
   <tbody>
   <tr>
   <td>Intestazione</td>
   <td><ul><li>`Content-Type: application/x-www-form-urlencoded`</li> <li>`Authorization: Basic a3ViZTprdWJl`</br><strong>Nota</strong>: <code>a3ViZTprdWJl</code> è uguale all'autorizzazione con codifica URL per il nome utente <strong><code>kube</code></strong> e la password <strong><code>kube</code></strong>.</li><li>`cache-control: no-cache`</li></ul>
   </td>
   </tr>
   <tr>
   <td>Corpo</td>
   <td><ul><li>`refresh_token`: il tuo token di aggiornamento delegato {{site.data.keyword.Bluemix_notm}} IAM. </li>
   <li>`grant_type: urn:ibm:params:oauth:grant-type:delegated-refresh-token`</li></ul></td>
   </tr>
   </tbody>
   </table>

   Output di esempio:
   ```
   {
    "access_token": "<iam_access_token>",
    "id_token": "<iam_id_token>",
    "refresh_token": "<iam_refresh_token>",
    "token_type": "Bearer",
    "expires_in": 3600,
    "expiration": 1553629664,
    "scope": "ibm openid containers-kubernetes"
   }
   ```
   {: screen}

4. Richiama l'URL pubblico del tuo master Kubernetes utilizzando il token di accesso IAM, il token ID IAM, il token di aggiornamento IAM e la regione {{site.data.keyword.containerlong_notm}} in cui si trova il cluster. L'URL è riportato nel **`publicServiceEndpointURL`** del tuo output dell'API.
   ```
   GET https://containers.cloud.ibm.com/v1/clusters/<cluster_ID>
   ```
   {: codeblock}

   <table summary="Parametri di input per l'ottenimento dell'endpoint del servizio pubblico per il tuo master Kubernetes con il parametro di input della colonna 1 e il valore della colonna 2.">
   <caption>Parametri di input per l'ottenimento dell'endpoint del servizio pubblico per il tuo master Kubernetes.</caption>
   <thead>
   <th>Parametri di input</th>
   <th>Valori</th>
   </thead>
   <tbody>
   <tr>
   <td>Intestazione</td>
     <td><ul><li>`Authorization`: il tuo token di accesso {{site.data.keyword.Bluemix_notm}} IAM.</li><li>`X-Auth-Refresh-Token`: il tuo token di aggiornamento {{site.data.keyword.Bluemix_notm}} IAM.</li><li>`X-Region`: la regione {{site.data.keyword.containerlong_notm}} del tuo cluster che hai richiamato con l'API `GET https://containers.cloud.ibm.com/v1/clusters` in [Automazione delle distribuzioni dei cluster con l'API](#cs_api). </li></ul>
   </td>
   </tr>
   <tr>
   <td>Percorso</td>
   <td>`<cluster_ID>:` L'ID del tuo cluster che hai richiamato con l'API `GET https://containers.cloud.ibm.com/v1/clusters` in [Automazione delle distribuzioni dei cluster con l'API](#cs_api).      </td>
   </tr>
   </tbody>
   </table>

   Output di esempio:
   ```
   {
    "location": "Dallas",
    "dataCenter": "dal10",
    "multiAzCapable": true,
    "vlans": null,
    "worker_vlans": null,
    "workerZones": [
        "dal10"
    ],
    "id": "1abc123b123b124ab1234a1a12a12a12",
    "name": "mycluster",
    "region": "us-south",
    ...
    "publicServiceEndpointURL": "https://c7.us-south.containers.cloud.ibm.com:27078"
   }
   ```
   {: screen}

5. Esegui le richieste API Kubernetes per il tuo cluster utilizzando il token ID IAM che hai richiamato in precedenza. Ad esempio, elenca la versione Kubernetes eseguita nel tuo cluster.

   Se hai abilitato la verifica del certificato SSL nel framework di test API, assicurati di disabilitare questa funzione.
   {: tip}

   ```
   GET <publicServiceEndpointURL>/version
   ```
   {: codeblock}

   <table summary="Parametri di input per la visualizzazione della versione Kubernetes eseguita nel tuo cluster con il parametro di input della colonna 1 e il valore della colonna 2.">
   <caption>Parametri di input per la visualizzazione della versione Kubernetes eseguita nel tuo cluster. </caption>
   <thead>
   <th>Parametri di input</th>
   <th>Valori</th>
   </thead>
   <tbody>
   <tr>
   <td>Intestazione</td>
   <td>`Authorization: bearer <id_token>`</td>
   </tr>
   <tr>
   <td>Percorso</td>
   <td>`<publicServiceEndpointURL>`:ul **`publicServiceEndpointURL`** del tuo master Kubernetes
che hai richiamato nel passo precedente      </td>
   </tr>
   </tbody>
   </table>

   Output di esempio:
   ```
   {
    "major": "1",
    "minor": "13",
    "gitVersion": "v1.13.4+IKS",
    "gitCommit": "c35166bd86eaa91d17af1c08289ffeab3e71e11e",
    "gitTreeState": "clean",
    "buildDate": "2019-03-21T10:08:03Z",
    "goVersion": "go1.11.5",
    "compiler": "gc",
    "platform": "linux/amd64"
   }
   ```
   {: screen}

6. Consulta la [documentazione dell'API Kubernetes![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/reference/kubernetes-api/) per trovare un elenco di API supportate per la versione più recente di Kubernetes. Assicurati di utilizzare la documentazione API che corrisponde alla versione Kubernetes del tuo cluster. Se non utilizzi la versione più recente di Kubernetes, aggiungi la tua versione alla fine dell'URL. Ad esempio, per accedere alla documentazione API per la versione 1.12, aggiungi `v1.12`.


## Aggiornamento dei token di accesso {{site.data.keyword.Bluemix_notm}} IAM e ottenimento di nuovi token di aggiornamento con l'API
{: #cs_api_refresh}

Ogni token di accesso {{site.data.keyword.Bluemix_notm}} IAM (Identity and Access Management) che viene emesso tramite API scade dopo un'ora. Devi aggiornare regolarmente il tuo token di accesso per garantire l'accesso all'API {{site.data.keyword.Bluemix_notm}}. Puoi utilizzare la stessa procedura per ottenere un nuovo token di aggiornamento.
{:shortdesc}

Prima di iniziare, assicurati di disporre di un token di aggiornamento {{site.data.keyword.Bluemix_notm}} IAM o di una chiave API {{site.data.keyword.Bluemix_notm}} che puoi utilizzare per richiedere un nuovo token di accesso.
- **Token di aggiornamento:** segui le istruzioni presenti in [Automazione del processo di creazione e gestione dei cluster con l'API {{site.data.keyword.Bluemix_notm}}](#cs_api).
- **Chiave API** richiama la tua chiave API [{{site.data.keyword.Bluemix_notm}} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cloud.ibm.com/) come segue.
   1. Dalla barra dei menu, fai clic su **Gestisci** > **Accesso (IAM)**.
   2. Fai clic sulla pagina **Utenti** e quindi seleziona te stesso.
   3. Nel riquadro **Chiavi API**, fai clic su **Crea una chiave API IBM Cloud**.
   4. Immetti un **Nome** e una **Descrizione** per la tua chiave API e fai clic su **Crea**.
   4. Fai clic su **Mostra** per vedere la chiave API che è stata generata per te.
   5. Copia la chiave API in modo che tu possa utilizzarla per richiamare il tuo nuovo token di accesso {{site.data.keyword.Bluemix_notm}} IAM.

Utilizza la seguente procedura se vuoi creare un token {{site.data.keyword.Bluemix_notm}} IAM o se vuoi ottenere un nuovo token di aggiornamento.

1.  Genera un nuovo token di accesso {{site.data.keyword.Bluemix_notm}} IAM utilizzando il token di aggiornamento oppure la chiave API {{site.data.keyword.Bluemix_notm}}.
    ```
    POST https://iam.bluemix.net/identity/token
    ```
    {: codeblock}

    <table summary="Parametri di input per il nuovo token IAM con il parametro di input della colonna 1 e il valore della colonna 2.">
    <caption>Parametri di input per un nuovo token {{site.data.keyword.Bluemix_notm}} IAM</caption>
    <thead>
    <th>Parametri di input</th>
    <th>Valori</th>
    </thead>
    <tbody>
    <tr>
    <td>Intestazione</td>
    <td><ul><li>`Content-Type: application/x-www-form-urlencoded`</li>
      <li>`Authorization: Basic Yng6Yng=`</br></br><strong>Nota:</strong> <code>Yng6Yng=</code> è uguale all'autorizzazione con codifica URL per il nome utente <strong>bx</strong> e la password <strong>bx</strong>.</li></ul></td>
    </tr>
    <tr>
    <td>Corpo quando si utilizza il token di aggiornamento</td>
    <td><ul><li>`grant_type: refresh_token`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`refresh_token:` il tuo token di aggiornamento {{site.data.keyword.Bluemix_notm}} IAM. </li>
    <li>`uaa_client_ID: cf`</li>
    <li>`uaa_client_secret:`</li>
    <li>`bss_account:` il tuo ID account {{site.data.keyword.Bluemix_notm}}. </li></ul><strong>Nota</strong>: aggiungi la chiave <code>uaa_client_secret</code> senza alcun valore specificato.</td>
    </tr>
    <tr>
      <td>Corpo quando si utilizza la chiave API {{site.data.keyword.Bluemix_notm}}</td>
      <td><ul><li>`grant_type: urn:ibm:params:oauth:grant-type:apikey`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`apikey:` la tua chiave API {{site.data.keyword.Bluemix_notm}}. </li>
    <li>`uaa_client_ID: cf`</li>
        <li>`uaa_client_secret:`</li></ul><strong>Nota:</strong> aggiungi la chiave <code>uaa_client_secret</code> senza alcun valore specificato.</td>
    </tr>
    </tbody>
    </table>

    Output API di esempio:

    ```
    {
      "access_token": "<iam_token>",
      "refresh_token": "<iam_refresh_token>",
      "uaa_token": "<uaa_token>",
      "uaa_refresh_token": "<uaa_refresh_token>",
      "token_type": "Bearer",
      "expires_in": 3600,
      "expiration": 1493747503
    }

    ```
    {: screen}

    Puoi trovare il tuo nuovo token {{site.data.keyword.Bluemix_notm}} IAM nel campo **access_token** e il token di aggiornamento nel campo **refresh_token** dell'output dell'API.

2.  Continua a lavorare con la [documentazione API {{site.data.keyword.containerlong_notm}} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://containers.cloud.ibm.com/global/swagger-global-api) utilizzando il token dal passo precedente.

<br />


## Aggiornamento dei token di accesso {{site.data.keyword.Bluemix_notm}} IAM e ottenimento di nuovi token di aggiornamento con la CLI
{: #cs_cli_refresh}

Quando avvii una nuova sessione della CLI o se sono trascorse 24 ore nella tua sessione corrente della CLI, devi impostare il contesto per il tuo cluster eseguendo `ibmcloud ks cluster-config --cluster <cluster_name>`. Quando imposti il contesto per il cluster con questo comando, viene scaricato il file `kubeconfig` per il tuo cluster Kubernetes. Inoltre, vengono emessi un token ID {{site.data.keyword.Bluemix_notm}} IAM (Identity and Access Management) e un token di aggiornamento per fornire l'autenticazione.
{: shortdesc}

**Token ID**: ogni token ID IAM che viene emesso tramite la CLI scade dopo un'ora. Quando il token ID scade, il token di aggiornamento viene inviato al provider di token per aggiornare il token ID. L'autenticazione viene aggiornata e puoi continuare a eseguire comandi sul tuo cluster.

**Token di aggiornamento**: i token di aggiornamento scadono ogni 30 giorni. Se il token di aggiornamento è scaduto, il token ID non può essere aggiornato e non puoi continuare a eseguire i comandi nella CLI. Puoi ottenere un nuovo token di aggiornamento eseguendo `ibmcloud ks cluster-config --cluster <cluster_name>`. Questo comando aggiorna anche il tuo token ID.
