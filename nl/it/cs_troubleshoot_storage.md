---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-05"

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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}



# Risoluzione dei problemi dell'archiviazione cluster
{: #cs_troubleshoot_storage}

Mentre utilizzi {{site.data.keyword.containerlong}}, tieni presente queste tecniche per la risoluzione dei problemi dell'archiviazione del cluster.
{: shortdesc}

Se hai un problema più generale, prova a [eseguire il debug del cluster](/docs/containers?topic=containers-cs_troubleshoot).
{: tip}


## Debug di errori degli archivi permanenti
{: #debug_storage}

Rivedi le opzioni per eseguire il debug dell'archiviazione permanente e trovare le cause principali degli errori.
{: shortdesc}

1. Accertati di utilizzare la versione più recente di {{site.data.keyword.Bluemix_notm}} e del plug-in {{site.data.keyword.containerlong_notm}}. 
   ```
   ibmcloud update
   ```
   {: pre}
   
   ```
   ibmcloud plugin repo-plugins
   ```
   {: pre}

2. Verifica che la versione della CLI di `kubectl` eseguita sulla macchina locale corrisponda alla versione Kubernetes installata nel tuo cluster. 
   1. Visualizza la versione della CLI di `kubectl` installata nel tuo cluster e la tua macchina locale.
      ```
      kubectl version
      ```
      {: pre} 
   
      Output di esempio: 
      ```
      Client Version: version.Info{Major:"1", Minor:"13", GitVersion:"v1.13.5", GitCommit:"641856db18352033a0d96dbc99153fa3b27298e5", GitTreeState:"clean", BuildDate:"2019-03-25T15:53:57Z", GoVersion:"go1.12.1", Compiler:"gc", Platform:"darwin/amd64"}
      Server Version: version.Info{Major:"1", Minor:"13", GitVersion:"v1.13.5+IKS", GitCommit:"e15454c2216a73b59e9a059fd2def4e6712a7cf0", GitTreeState:"clean", BuildDate:"2019-04-01T10:08:07Z", GoVersion:"go1.11.5", Compiler:"gc", Platform:"linux/amd64"}
      ```
      {: screen}
    
      Le versioni delle CLI corrispondono se la versione visualizzata per il client e per il server in `GitVersion` è la stessa. Puoi ignorare la parte `+IKS` della versione del server.
   2. Se le versioni della CLI di `kubectl` eseguita nella tua macchina locale e nel tuo cluster non corrispondono, [aggiorna il tuo cluster](/docs/containers?topic=containers-update) o [installa una versione diversa della CLI sulla tua macchina locale](/docs/containers?topic=containers-cs_cli_install#kubectl). 

3. Solo per l'archiviazione blocchi, l'archiviazione oggetti e Portworx: accertati di aver [installato il server Helm Tiller con un account di servizi Kubernetes](/docs/containers?topic=containers-helm#public_helm_install). 

4. Solo per l'archiviazione blocchi, l'archiviazione oggetti e Portworx: accertati di aver installato l'ultima versione dei grafici Helm per il plug-in. 
   
   **Archiviazione blocchi e oggetti**: 
   
   1. Aggiorna i tuoi repository di grafici Helm.
      ```
      helm repo update
      ```
      {: pre}
      
   2. Elenca i grafici Helm del repository `iks-charts`.
      ```
      helm search iks-charts
      ```
      {: pre}
      
      Output di esempio: 
      ```
      iks-charts/ibm-block-storage-attacher          	1.0.2        A Helm chart for installing ibmcloud block storage attach...
      iks-charts/ibm-iks-cluster-autoscaler          	1.0.5        A Helm chart for installing the IBM Cloud cluster autoscaler
      iks-charts/ibm-object-storage-plugin           	1.0.6        A Helm chart for installing ibmcloud object storage plugin  
      iks-charts/ibm-worker-recovery                 	1.10.46      IBM Autorecovery system allows automatic recovery of unhe...
      ...
      ```
      {: screen}
      
   3. Elenca i grafici Helm installati nel tuo cluster e confronta la versione che hai installato con quella disponibile.
      ```
      helm ls
      ```
      {: pre}
      
   4. Se è disponibile una versione più recente, installarla. Per istruzioni, vedi [Aggiornamento del plug-in di archiviazione blocchi {{site.data.keyword.Bluemix_notm}}](/docs/containers?topic=containers-block_storage#updating-the-ibm-cloud-block-storage-plug-in) e [Aggiornamento del plug-in {{site.data.keyword.cos_full_notm}}](/docs/containers?topic=containers-object_storage#update_cos_plugin). 
   
   **Portworx**: 
   
   1. Trova l'[ultima versione del grafico Helm ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/portworx/helm/tree/master/charts/portworx) disponibile. 
   
   2. Elenca i grafici Helm installati nel tuo cluster e confronta la versione che hai installato con quella disponibile.
      ```
      helm ls
      ```
      {: pre}
   
   3. Se è disponibile una versione più recente, installarla. Per istruzioni, vedi [Aggiornamento di Portworx nel tuo cluster](/docs/containers?topic=containers-portworx#update_portworx). 
   
5. Verifica che il driver di archiviazione e i pod del plug-in mostrino lo stato **Running** (In esecuzione). 
   1. Elenca i pod nello spazio dei nomi `kube-system`.
      ```
      kubectl get pods -n kube-system 
      ```
      {: pre}
      
   2. Se i pod non mostrano lo stato **Running**, ottieni ulteriori dettagli del pod per trovare la causa principale. A seconda dello stato del pod, potresti non essere in grado di eseguire tutti i comandi di seguito riportati.
      ```
      kubectl describe pod <pod_name> -n kube-system
      ```
      {: pre}
      
      ```
      kubectl logs <pod_name> -n kube-system
      ```
      {: pre}
      
   3. Analizza la sezione **Events** (Eventi) dell'output della CLI del comando `kubectl describe pod` e i log più recenti per trovare la causa principale per l'errore. 
   
6. Verifica se il provisioning della tua PVC viene eseguito correttamente. 
   1. Controlla lo stato della tua PVC. Il provisioning della PVC viene eseguito correttamente se lo stato della PVC è **Bound** (Collegato).
      ```
      kubectl get pvc
      ```
      {: pre}
      
   2. Se lo stato della PVC è **Pending**, richiama l'errore per cui la PVC resta in sospeso.
      ```
      kubectl describe pvc <pvc_name>
      ```
      {: pre}
      
   3. Esamina gli errori comuni che possono verificarsi durante la creazione della PVC. 
      - [Archiviazione file e archiviazione blocchi: la PVC resta in sospeso](#file_pvc_pending)
      - [Archiviazione oggetti: la PVC resta in sospeso](#cos_pvc_pending)
   
7. Controlla se il pod che monta la tua istanza di archiviazione viene distribuito correttamente. 
   1. Elenca i pod nel tuo cluster. Un pod è stato distribuito correttamente se lo stato del pod è **Running**.
      ```
      kubectl get pods
      ```
      {: pre}
      
   2. Ottieni i dettagli del tuo pod e controlla se vengono visualizzati errori nella sezione **Events** dell'output della tua CLI.
      ```
      kubectl describe pod <pod_name>
      ```
      {: pre}
   
   3. Richiama i log della tua applicazione e controlla se sono presenti messaggi di errore.
      ```
      kubectl logs <pod_name>
      ```
      {: pre}
   
   4. Esamina gli errori comuni che possono verificarsi quando monti una PVC nella tua applicazione. 
      - [Archiviazione file: l'applicazione non riesce ad accedere alla PVC o a scrivere sulla PVC](#file_app_failures)
      - [Archiviazione blocchi: l'applicazione non riesce ad accedere alla PVC o a scrivere sulla PVC](#block_app_failures)
      - [Archiviazione oggetti: l'accesso ai file con un utente non root non riesce](#cos_nonroot_access)
      

## Archiviazione file e archiviazione blocchi: la PVC resta in sospeso
{: #file_pvc_pending}

{: tsSymptoms}
Quando crei una PVC ed esegui `kubectl get pvc <pvc_name>`, lo stato della tua PVC resta **Pending**, anche dopo un po' di tempo. 

{: tsCauses}
Durante la creazione e il bind della PVC, il plug-in di archiviazione file e blocchi esegue varie attività. Ciascuna di esse può non riuscire e causare un messaggio di errore differente.

{: tsResolve}

1. Trova la causa principale per cui lo stato della PVC resta **Pending**. 
   ```
   kubectl describe pvc <pvc_name> -n <namespace>
   ```
   {: pre}
   
2. Esamina le descrizioni e le soluzioni dei messaggi di errore comuni.
   
   <table>
   <thead>
     <th>Messaggio di errore</th>
     <th>Descrizione</th>
     <th>Procedura risolutiva</th>
  </thead>
  <tbody>
    <tr>
      <td><code>User doesn't have permissions to create or manage Storage</code></br></br><code>Failed to find any valid softlayer credentials in configuration file</code></br></br><code>Storage with the order ID %d could not be created after retrying for %d seconds.</code></br></br><code>Unable to locate datacenter with name <datacenter_name>.</code></td>
      <td>La chiave API IAM o la chiave API dell'infrastruttura IBM Cloud (SoftLayer) archiviata nel segreto Kubernetes `storage-secret-store` del tuo cluster non dispone di tutte le autorizzazioni richieste per eseguire il provisioning dell'archiviazione permanente. </td>
      <td>Vedi [Creazione della PVC non riuscita per autorizzazioni mancanti](#missing_permissions). </td>
    </tr>
    <tr>
      <td><code>Your order will exceed the maximum number of storage volumes allowed. Please contact Sales</code></td>
      <td>Ogni account {{site.data.keyword.Bluemix_notm}} viene configurato con un numero massimo di istanze di archiviazione generabili. Con la creazione della PVC, superi il numero massimo di istanze di archiviazione. </td>
      <td>Per creare una PVC, scegli tra le seguenti opzioni. <ul><li>Rimuovi i PVC inutilizzati.</li><li>Chiedi al proprietario dell'account {{site.data.keyword.Bluemix_notm}} di aumentare la tua quota di archiviazione [aprendo un caso di supporto](/docs/get-support?topic=get-support-getting-customer-support).</li></ul> </td>
    </tr>
    <tr>
      <td><code>Unable to find the exact ItemPriceIds(type|size|iops) for the specified storage</code> </br></br><code>Failed to place storage order with the storage provider</code></td>
      <td>La dimensione di archiviazione e gli IOPS che hai specificato nella tua PVC non sono supportati dal tipo di archiviazione che hai scelto e non possono essere utilizzati con la classe di archiviazione specificata. </td>
      <td>Consulta [Decisioni relative alla configurazione dell'archiviazione file](/docs/containers?topic=containers-file_storage#file_predefined_storageclass) e [Decisioni relative alla configurazione dell'archiviazione blocchi](/docs/containers?topic=containers-block_storage#block_predefined_storageclass) per trovare le dimensioni di memoria supportate e gli IOPS per la classe di archiviazione che vuoi utilizzare. Correggi la dimensione e gli IOPS e ricrea la PVC. </td>
    </tr>
    <tr>
  <td><code>Failed to find the datacenter name in configuration file</code></td>
      <td>Il data center che hai specificato nella tua PVC non esiste. </td>
  <td>Esegui <code>ibmcloud ks locations</code> per elencare i data center disponibili. Correggi il data center nella tua PVC e ricrea la PVC.</td>
    </tr>
    <tr>
  <td><code>Failed to place storage order with the storage provider</code></br></br><code>Storage with the order ID 12345 could not be created after retrying for xx seconds. </code></br></br><code>Failed to do subnet authorizations for the storage 12345.</code><code>Storage 12345 has ongoing active transactions and could not be completed after retrying for xx seconds.</code></td>
  <td>La dimensione della memoria, gli IOPS e il tipo di archiviazione potrebbero essere incompatibili con la classe di archiviazione che hai scelto o l'endpoint dell'API dell'infrastruttura {{site.data.keyword.Bluemix_notm}} non è attualmente disponibile. </td>
  <td>Consulta [Decisioni relative alla configurazione dell'archiviazione file](/docs/containers?topic=containers-file_storage#file_predefined_storageclass) e [Decisioni relative alla configurazione dell'archiviazione blocchi](/docs/containers?topic=containers-block_storage#block_predefined_storageclass) per trovare le dimensioni di memoria supportate e gli IOPS per la classe e il tipo di archiviazione che vuoi utilizzare. Quindi, elimina la PVC e ricreala. </td>
  </tr>
  <tr>
  <td><code>Failed to find the storage with storage id 12345. </code></td>
  <td>Vuoi creare una PVC per un'istanza di archiviazione esistente utilizzando il provisioning statico di Kubernetes, ma l'istanza di archiviazione che hai specificato non è stata trovata. </td>
  <td>Segui le istruzioni per eseguire il provisioning dell'[archiviazione file](/docs/containers?topic=containers-file_storage#existing_file) o dell'[archiviazione blocchi](/docs/containers?topic=containers-block_storage#existing_block) esistente nel tuo cluster e assicurarsi di richiamare le informazioni corrette per l'istanza di archiviazione esistente. Quindi, elimina la PVC e ricreala.</td>
  </tr>
  <tr>
  <td><code>Storage type not provided, expected storage type is `Endurance` or `Performance`. </code></td>
  <td>Hai creato una classe di archiviazione personalizzata e hai specificato un tipo di archiviazione non supportato.</td>
  <td>Aggiorna la tua classe di archiviazione personalizzata per specificare `Endurance` o `Performance` come tipo di archiviazione. Per esempi per le classi di archiviazione personalizzate, vedi le classi di archiviazione personalizzate di esempio per l'[archiviazione file](/docs/containers?topic=containers-file_storage#file_custom_storageclass) e l'[archiviazione blocchi](/docs/containers?topic=containers-block_storage#block_custom_storageclass). </td>
  </tr>
  </tbody>
  </table>
  
## Archiviazione file: l'applicazione non riesce ad accedere alla PVC o a scrivere sulla PVC
{: #file_app_failures}

Quando monti una PVC nel tuo pod, potresti riscontrare errori nell'accesso alla PVC o nella scrittura nella PVC.
{: shortdesc}

1. Elenca i pod del tuo cluster e riesamina lo stato del pod. 
   ```
   kubectl get pods
   ```
   {: pre}
   
2. Trova la causa principale per cui non è possibile accedere alla PVC o scrivervi.
   ```
   kubectl describe pod <pod_name>
   ```
   {: pre}
   
   ```
   kubectl logs <pod_name>
   ```
   {: pre}

3. Esamina gli errori comuni che possono verificarsi quando monti una PVC in un pod. 
   <table>
   <thead>
     <th>Sintomo o messaggio di errore</th>
     <th>Descrizione</th>
     <th>Procedura risolutiva</th>
  </thead>
  <tbody>
    <tr>
      <td>Il tuo pod si blocca nello stato <strong>ContainerCreating</strong>. </br></br><code>MountVolume.SetUp failed for volume ... read-only file system</code></td>
      <td>Si sono verificati problemi di rete nel backend dell'infrastruttura {{site.data.keyword.Bluemix_notm}}. Per proteggere i tuoi dati ed evitare la corruzione dei dati, {{site.data.keyword.Bluemix_notm}} ha disconnesso automaticamente il server di archiviazione file per impedire operazioni di scrittura su condivisioni file NFS.  </td>
      <td>Vedi [Archiviazione file: i file system per i nodi di lavoro diventano di sola lettura](#readonly_nodes)</td>
      </tr>
      <tr>
  <td><code>write-permission</code> </br></br><code>do not have required permission</code></br></br><code>cannot create directory '/bitnami/mariadb/data': Permission denied </code></td>
  <td>Nella tua distribuzione, hai specificato che il proprietario del percorso di montaggio dell'archiviazione file NFS deve essere un utente non root. Per impostazione predefinita, gli utenti non root non dispongono dell'autorizzazione di scrittura sul percorso di montaggio del volume per l'archiviazione con supporto NFS. </td>
  <td>Vedi [Archiviazione file: l'applicazione genera un errore quando un utente non root possiede il percorso di montaggio dell'archiviazione file NFS](#nonroot)</td>
  </tr>
  <tr>
  <td>Una volta che specifichi che il proprietario del percorso di montaggio dell'archiviazione file NFS deve essere un utente non root o che distribuisci un grafico Helm con un ID utente non root specificato, l'utente non può scrivere nell'archiviazione montata.</td>
  <td>La configurazione del grafico Helm o della distribuzione specifica il contesto di sicurezza per l'<code>fsGroup</code> (ID gruppo) e il <code>runAsUser</code> (ID utente) del pod</td>
  <td>Vedi [Archiviazione file: l'aggiunta di accesso utente non root all'archiviazione persistente non riesce](#cs_storage_nonroot)</td>
  </tr>
</tbody>
</table>

### Archiviazione file: i file system per i nodi di lavoro diventano di sola lettura
{: #readonly_nodes}

{: tsSymptoms}
{: #stuck_creating_state}
Potresti vedere uno dei seguenti sintomi:
- Quando esegui `kubectl get pods -o wide`, visualizzi che molti pod in esecuzione nello stesso nodo di lavoro sono bloccati nello stato `ContainerCreating`.
- Quando esegui un comando `kubectl describe`, vedi il seguente errore nella sezione **Eventi**: `MountVolume.SetUp failed for volume ... read-only file system`.

{: tsCauses}
Il file system nel nodo di lavoro è in sola lettura.

{: tsResolve}
1.  Esegui il backup di tutti i dati che possono venire archiviati nel nodo di lavoro o nei tuoi contenitori.
2.  Per una correzione a breve termine per il nodo di lavoro esistente, ricarica il nodo di lavoro.
    <pre class="pre"><code>ibmcloud ks worker-reload --cluster &lt;cluster_name&gt; --worker &lt;worker_ID&gt;</code></pre>

Per una correzione a lungo termine, [aggiorna il tipo di macchina del tuo pool di nodi di lavoro](/docs/containers?topic=containers-update#machine_type).

<br />



### Archiviazione file: l'applicazione genera un errore quando un utente non root possiede il percorso di montaggio dell'archiviazione file NFS
{: #nonroot}

{: tsSymptoms}
Dopo aver [aggiunto l'archiviazione NFS](/docs/containers?topic=containers-file_storage#file_app_volume_mount) alla tua distribuzione, la distribuzione del tuo contenitore ha esito negativo. Quando richiami i log per il tuo contenitore, potresti vedere degli errori simili ai seguenti: Il pod genera un errore e si blocca in un ciclo di ricaricamento.

```
write-permission
```
{: screen}

```
do not have required permission
```
{: screen}

```
cannot create directory '/bitnami/mariadb/data': Permission denied
```
{: screen}

{: tsCauses}
Per impostazione predefinita, gli utenti non root non dispongono dell'autorizzazione di scrittura sul percorso di montaggio del volume per l'archiviazione con supporto NFS. Alcune immagini comuni dell'applicazione, come Jenkins e Nexus3, specificano un utente non root che possiede il percorso di montaggio nel Dockerfile. Quando crei un contenitore da questo Dockerfile, la creazione del contenitore non riesce a causa di autorizzazioni insufficienti dell'utente non root sul percorso di montaggio. Per concedere l'autorizzazione di scrittura, puoi modificare il Dockerfile per aggiungere temporaneamente l'utente non root al gruppo di utenti root prima di modificare le autorizzazioni del percorso di montaggio o utilizzare un contenitore init.

Se utilizzi un grafico Helm per distribuire l'immagine, modifica la distribuzione Helm in modo che utilizzi un contenitore init.
{:tip}



{: tsResolve}
Quando includi un [contenitore init![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/) nella tua distribuzione, puoi fornire a un utente non root specificato nel Dockerfile le autorizzazioni di scrittura sul percorso di montaggio del volume all'interno del contenitore. Il contenitore init viene avviato prima del contenitore dell'applicazione. Il contenitore init crea il percorso di montaggio del volume all'interno del contenitore, modifica il percorso di montaggio in modo che sia di proprietà dell'utente corretto (non root) e si chiude. Quindi, viene avviato il contenitore dell'applicazione con l'utente non root che deve scrivere sul percorso di montaggio. Poiché il percorso è già di proprietà dell'utente non root, la scrittura sul percorso di montaggio ha esito positivo. Se non vuoi utilizzare un contenitore init, puoi modificare il Dockerfile per aggiungere l'accesso utente non root all'archiviazione file NFS.


Prima di iniziare: [accedi al tuo account. Se applicabile, specifica il gruppo di risorse appropriato. Imposta il contesto per il tuo cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  Apri il Dockerfile per la tua applicazione e ottieni l'ID utente (UID) e l'ID di gruppo (GID) dell'utente a cui desideri concedere l'autorizzazione di scrittore sul percorso di montaggio del volume. Nell'esempio di un Dockerfile Jenkins, le informazioni sono:
    - UID: `1000`
    - GID: `1000`

    **Esempio**:

    ```
    FROM openjdk:8-jdk

    RUN apt-get update &&apt-get install -y git curl &&rm -rf /var/lib/apt/lists/*

    ARG user=jenkins
    ARG group=jenkins
    ARG uid=1000
    ARG gid=1000
    ARG http_port=8080
    ARG agent_port=50000

    ENV JENKINS_HOME /var/jenkins_home
    ENV JENKINS_SLAVE_AGENT_PORT ${agent_port}
    ...
    ```
    {:screen}

2.  Aggiungi l'archiviazione persistente alla tua applicazione creando un'attestazione del volume persistente (o PVC, persistent volume claim). Questo esempio utilizza la classe di archiviazione `ibmc-file-bronze`. Per esaminare le classi di archiviazione disponibili, esegui `kubectl get storageclasses`.

    ```
    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
      name: mypvc
      annotations:
        volume.beta.kubernetes.io/storage-class: "ibmc-file-bronze"
    spec:
      accessModes:
        - ReadWriteMany
      resources:
        requests:
          storage: 20Gi
    ```
    {: codeblock}

3.  Crea la PVC.

    ```
    kubectl apply -f mypvc.yaml
    ```
    {: pre}

4.  Nel tuo file di distribuzione `.yaml`, aggiungi il contenitore init. Includi l'UID e il GID che hai richiamato in precedenza.

    ```
    initContainers:
    - name: initcontainer # Or replace the name
      image: alpine:latest
      command: ["/bin/sh", "-c"]
      args:
        - chown <UID>:<GID> /mount; # Replace UID and GID with values from the Dockerfile
      volumeMounts:
      - name: volume # Or you can replace with any name
        mountPath: /mount # Must match the mount path in the args line
    ```
    {: codeblock}

    **Esempio** per una distribuzione Jenkins:

    ```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: my_pod
    spec:
      replicas: 1
      selector:
        matchLabels:
          app: jenkins      
      template:
        metadata:
          labels:
            app: jenkins
        spec:
          containers:
          - name: jenkins
            image: jenkins
            volumeMounts:
            - mountPath: /var/jenkins_home
              name: volume
          volumes:
          - name: volume
            persistentVolumeClaim:
              claimName: mypvc
          initContainers:
          - name: permissionsfix
            image: alpine:latest
            command: ["/bin/sh", "-c"]
            args:
              - chown 1000:1000 /mount;
            volumeMounts:
            - name: volume
              mountPath: /mount
    ```
    {: codeblock}

5.  Crea il pod e monta la PVC nel tuo pod.

    ```
    kubectl apply -f my_pod.yaml
    ```
    {: pre}

6. Verifica che il volume sia stato correttamente montato nel tuo pod. Prendi nota del nome del pod e del percorso **Containers/Mounts**.

    ```
    kubectl describe pod <my_pod>
    ```
    {: pre}

    **Output di esempio**:

    ```
    Name:		    mypod-123456789
    Namespace:	default
    ...
    Init Containers:
    ...
    Mounts:
      /mount from volume (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-cp9f0 (ro)
    ...
    Containers:
      jenkins:
        Container ID:
        Image:		jenkins
        Image ID:
        Port:		  <none>
        State:		Waiting
          Reason:		PodInitializing
        Ready:		False
        Restart Count:	0
        Environment:	<none>
        Mounts:
          /var/jenkins_home from volume (rw)
          /var/run/secrets/kubernetes.io/serviceaccount from default-token-cp9f0 (ro)
    ...
    Volumes:
      myvol:
        Type: PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
        ClaimName: mypvc
        ReadOnly: false

    ```
    {: screen}

7.  Accedi al pod utilizzando il nome pod di cui hai preso nota precedentemente.

    ```
    kubectl exec -it <my_pod-123456789> /bin/bash
    ```
    {: pre}

8. Verifica le autorizzazioni del percorso di montaggio del tuo contenitore. Nell'esempio, il percorso di montaggio è `/var/jenkins_home`.

    ```
    ls -ln /var/jenkins_home
    ```
    {: pre}

    **Output di esempio**:

    ```
    jenkins@mypod-123456789:/$ ls -ln /var/jenkins_home
    total 12
    -rw-r--r-- 1 1000 1000  102 Mar  9 19:58 copy_reference_file.log
    drwxr-xr-x 2 1000 1000 4096 Mar  9 19:58 init.groovy.d
    drwxr-xr-x 9 1000 1000 4096 Mar  9 20:16 war
    ```
    {: screen}

    Questo output mostra che il GID e l'UID dal tuo Dockerfile (in questo esempio, `1000` e `1000`) possiedono il percorso di montaggio all'interno del contenitore.

<br />


### Archiviazione file: l'aggiunta di accesso utente non root all'archiviazione persistente non riesce
{: #cs_storage_nonroot}

{: tsSymptoms}
Dopo aver [aggiunto l'accesso utente non root all'archiviazione persistente](#nonroot) o aver distribuito un grafico Helm con un ID utente non root specificato, l'utente non può scrivere nell'archiviazione montata.

{: tsCauses}
La distribuzione o la configurazione del grafico Helm specifica il [contesto di sicurezza](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/) per `fsGroup` (ID gruppo) e `runAsUser` (ID utente) del pod. Attualmente, {{site.data.keyword.containerlong_notm}} non supporta la specifica `fsGroup` e supporta solo `runAsUser` impostato come `0` (autorizzazioni root).

{: tsResolve}
Rimuovi i campi `securityContext` della configurazione per `fsGroup` e `runAsUser` dal file di configurazione dell'immagine, della distribuzione o del grafico Helm e riesegui la distribuzione. Se devi modificare la proprietà del percorso di montaggio da `nobody`, [aggiungi l'accesso utente non root](#nonroot). Dopo che hai aggiunto l'[non-root `initContainer`](#nonroot), imposta `runAsUser` a livello di contenitore, non a livello di pod.

<br />




## Archiviazione blocchi: l'applicazione non riesce ad accedere alla PVC o a scrivere sulla PVC
{: #block_app_failures}

Quando monti una PVC nel tuo pod, potresti riscontrare errori nell'accesso alla PVC o nella scrittura nella PVC.
{: shortdesc}

1. Elenca i pod del tuo cluster e riesamina lo stato del pod. 
   ```
   kubectl get pods
   ```
   {: pre}
   
2. Trova la causa principale per cui non è possibile accedere alla PVC o scrivervi.
   ```
   kubectl describe pod <pod_name>
   ```
   {: pre}
   
   ```
   kubectl logs <pod_name>
   ```
   {: pre}

3. Esamina gli errori comuni che possono verificarsi quando monti una PVC in un pod. 
   <table>
   <thead>
     <th>Sintomo o messaggio di errore</th>
     <th>Descrizione</th>
     <th>Procedura risolutiva</th>
  </thead>
  <tbody>
    <tr>
      <td>Il tuo pod è bloccato nello stato <strong>ContainerCreating</strong> o <strong>CrashLoopBackOff</strong>. </br></br><code>MountVolume.SetUp failed for volume ... read-only.</code></td>
      <td>Si sono verificati problemi di rete nel backend dell'infrastruttura {{site.data.keyword.Bluemix_notm}}. Per proteggere i tuoi dati ed evitarne la corruzione, {{site.data.keyword.Bluemix_notm}} ha disconnesso automaticamente il server di archiviazione blocchi per impedire operazioni di scrittura su istanze di archiviazione blocchi.  </td>
      <td>Vedi [Archiviazione blocchi: l'archiviazione blocchi è modificata a sola lettura](#readonly_block)</td>
      </tr>
      <tr>
  <td><code>failed to mount the volume as "ext4", it already contains xfs. Mount error: mount failed: exit status 32</code> </td>
        <td>Vuoi montare un'istanza di archiviazione blocchi esistente che era stata configurata con un file system <code>XFS</code>. Quando hai creato il PV e la PVC corrispondente, hai specificato un file system <code>ext4</code> o non hai specificato nessun file system. Il file system che specifichi nel tuo PV deve essere lo stesso configurato nella tua istanza di archiviazione blocchi. </td>
  <td>Vedi [Archiviazione blocchi: impossibile montare l'archiviazione blocchi esistente in un pod a causa del file system errato](#block_filesystem)</td>
  </tr>
</tbody>
</table>

### Archiviazione blocchi: l'archiviazione blocchi è modificata a sola lettura
{: #readonly_block}

{: tsSymptoms}
Potresti vedere i seguenti sintomi:
- Quando esegui `kubectl get pods -o wide`, vedi che più pod sullo stesso nodo di lavoro sono bloccati nello stato `ContainerCreating` o `CrashLoopBackOff`. Tutti questi pod utilizzano la stessa istanza di archiviazione blocchi.
- Quando esegui un comando `kubectl describe pod`, vedi il seguente errore nella sezione **Eventi**: `MountVolume.SetUp failed for volume ...
read-only`.

{: tsCauses}
Se si verifica un errore di rete mentre un pod scrive su un volume, l'infrastruttura IBM Cloud (SoftLayer) protegge i dati sul volume per evitarne il danneggiamento modificando il volume a una modalità di sola lettura. I pod che utilizzano questo volume non possono continuare a scrivere sul volume e riscontrano una condizione di errore.

{: tsResolve}
1. Controlla la versione del plugin {{site.data.keyword.Bluemix_notm}} Block Storage installato nel tuo cluster.
   ```
   helm ls
   ```
   {: pre}

2. Verifica che quella che utilizzi è la [versione più recente del plugin {{site.data.keyword.Bluemix_notm}} Block Storage](https://cloud.ibm.com/kubernetes/solutions/helm-charts/ibm/ibmcloud-block-storage-plugin). In caso contrario, [aggiorna il tuo plugin](/docs/containers?topic=containers-block_storage#updating-the-ibm-cloud-block-storage-plug-in).
3. Se hai utilizzato una distribuzione Kubernetes per il tuo pod, riavvia il pod che sta riscontrando la condizione di errore rimuovendolo e lasciando che Kubernetes lo crei nuovamente. Se non hai utilizzato una distribuzione, richiama il file YAML che era stato utilizzato per creare il tuo pod eseguendo `kubectl get pod <pod_name> -o yaml >pod.yaml`. Quindi elimina e crea di nuovo manualmente il pod.
    ```
    kubectl delete pod <pod_name>
    ```
    {: pre}

4. Controlla se creare nuovamente il tuo pod ha risolto il problema. Se non lo ha fatto, ricarica il nodo di lavoro.
   1. Trova il nodo di lavoro dove viene eseguito il tuo pod e annota l'indirizzo IP privato assegnato al tuo nodo di lavoro.
      ```
      kubectl describe pod <pod_name> | grep Node
      ```
      {: pre}

      Output di esempio:
      ```
      Node:               10.75.XX.XXX/10.75.XX.XXX
      Node-Selectors:  <none>
      ```
      {: screen}

   2. Richiama l'**ID** del tuo nodo di lavoro utilizzando l'indirizzo IP privato dal passo precedente.
      ```
      ibmcloud ks workers --cluster <cluster_name_or_ID>
      ```
      {: pre}

   3. Senza forzare l'operazione, [ricarica il nodo di lavoro](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload).


<br />


### Archiviazione blocchi: impossibile montare l'archiviazione blocchi esistente in un pod a causa del file system errato
{: #block_filesystem}

{: tsSymptoms}
Quando esegui `kubectl describe pod <pod_name>`, vedi il seguente errore:
```
failed to mount the volume as "ext4", it already contains xfs. Mount error: mount failed: exit status 32
```
{: screen}

{: tsCauses}
Hai un dispositivo di archiviazione blocchi esistente configurato con un file system `XFS`. Per montare questo dispositivo nel tuo pod, [hai creato un PV](/docs/containers?topic=containers-block_storage#existing_block) che ha specificato `ext4` come tuo file system o nessun file system nella sezione `spec/flexVolume/fsType`. Se non viene definito un file system, viene utilizzato il valore predefinito del PV `ext4`.
Il PV era stato creato correttamente e collegato alla tua istanza dell'archiviazione blocchi esistente. Tuttavia, quando tenti di montare il PV nel tuo cluster utilizzando una PVC corrispondente, il montaggio del volume non riesce. Non puoi montare la tua istanza dell'archiviazione blocchi `XFS` con un file system `ext4` nel pod.

{: tsResolve}
Aggiorna il file system nel PV esistente da `ext4` a `XFS`.

1. Elenca i PV esistenti nel tuo cluster e prendi nota del nome del PV che hai utilizzato per la tua istanza dell'archiviazione blocchi esistente.
   ```
   kubectl get pv
   ```
   {: pre}

2. Salva lo YAML del PV sulla tua macchina locale.
   ```
   kubectl get pv <pv_name> -o yaml > <filepath/xfs_pv.yaml>
   ```
   {: pre}

3. Apri il file YAML e modifica l'`fsType` da `ext4` a `xfs`.
4. Sostituisci il PV nel tuo cluster.
   ```
   kubectl replace --force -f <filepath/xfs_pv.yaml>
   ```
   {: pre}

5. Accedi al pod in cui hai montato il PV.
   ```
   kubectl exec -it <pod_name> sh
   ```
   {: pre}

6. Verifica che il file system sia stato modificato con `XFS`.
   ```
   df -Th
   ```
   {: pre}

   Output di esempio:
   ```
   Filesystem Type Size Used Avail Use% Mounted on /dev/mapper/3600a098031234546d5d4c9876654e35 xfs 20G 33M 20G 1% /myvolumepath
   ```
   {: screen}

<br />



## Archiviazione oggetti: l'installazione del plugin Helm {{site.data.keyword.cos_full_notm}} `ibmc` non riesce
{: #cos_helm_fails}

{: tsSymptoms}
Quando installi il plug-in Helm `ibmc` {{site.data.keyword.cos_full_notm}}, l'installazione non riesce, con uno dei seguenti errori:
```
Error: symlink /Users/ibm/ibmcloud-object-storage-plugin/helm-ibmc /Users/ibm/.helm/plugins/helm-ibmc: file exists
```
{: screen}

```
Error: fork/exec /home/iksadmin/.helm/plugins/helm-ibmc/ibmc.sh: permission denied
```
{: screen}

{: tsCauses}
Quando il plugin Helm `ibmc` è installato, viene creato un collegamento simbolico dalla directory `./helm/plugins/helm-ibmc` alla directory dove si trova il plugin Helm `ibmc` sul tuo sistema locale, che è di norma in `./ibmcloud-object-storage-plugin/helm-ibmc`. Quando rimuovi il plugin Helm `ibmc` dal tuo sistema locale, o sposti la directory del plugin Helm `ibmc` in un'altra ubicazione, il collegamento simbolico non viene rimosso.

Se visualizzi un errore `permission denied`, non possiedi la necessaria autorizzazione `read`, `write`ed `execute` sul file bash `ibmc.sh` che TI consente di eseguire i comandi del plug-in HELM `ibmc`. 

{: tsResolve}

**Per gli errori symlink**: 

1. Rimuovi il plugin Helm {{site.data.keyword.cos_full_notm}}.
   ```
   rm -rf ~/.helm/plugins/helm-ibmc
   ```
   {: pre}

2. Segui la [documentazione](/docs/containers?topic=containers-object_storage#install_cos) per reinstallare il plug-in Helm `ibmc` e il plug-in {{site.data.keyword.cos_full_notm}}.

**Per gli errori di autorizzazione**: 

1. Modifica le autorizzazioni per il plug-in `ibmc`. 
   ```
   chmod 755 ~/.helm/plugins/helm-ibmc/ibmc.sh
   ```
   {: pre}
   
2. Prova il plug-in Helm `ibm`. 
   ```
   helm ibmc --help
   ```
   {: pre}
   
3. [Continua a installare il plug-in {{site.data.keyword.cos_full_notm}}](/docs/containers?topic=containers-object_storage#install_cos). 


<br />


## Archiviazione oggetti: la PVC resta in sospeso
{: #cos_pvc_pending}

{: tsSymptoms}
Quando crei una PVC ed esegui `kubectl get pvc <pvc_name>`, lo stato della tua PVC resta **Pending**, anche dopo un po' di tempo. 

{: tsCauses}
Durante la creazione e il bind della PVC, il plug-in {{site.data.keyword.cos_full_notm}} esegue varie attività. Ciascuna di esse può non riuscire e causare un messaggio di errore differente.

{: tsResolve}

1. Trova la causa principale per cui lo stato della PVC resta **Pending**. 
   ```
   kubectl describe pvc <pvc_name> -n <namespace>
   ```
   {: pre}
   
2. Esamina le descrizioni e le soluzioni dei messaggi di errore comuni.
   
   <table>
   <thead>
     <th>Messaggio di errore</th>
     <th>Descrizione</th>
     <th>Procedura risolutiva</th>
  </thead>
  <tbody>
    <tr>
      <td>`User doesn't have permissions to create or manage Storage`</td>
      <td>La chiave API IAM o la chiave API dell'infrastruttura IBM Cloud (SoftLayer) archiviata nel segreto Kubernetes `storage-secret-store` del tuo cluster non dispone di tutte le autorizzazioni richieste per eseguire il provisioning dell'archiviazione permanente. </td>
      <td>Vedi [Creazione della PVC non riuscita per autorizzazioni mancanti](#missing_permissions). </td>
    </tr>
    <tr>
      <td>`cannot get credentials: cannot get secret <secret_name>: secrets "<secret_name>" not found`</td>
      <td>Il segreto Kubernetes che contiene le credenziali del tuo servizio {{site.data.keyword.cos_full_notm}} non esiste nello stesso spazio dei nomi della PVC o del pod. </td>
      <td>Vedi [La creazione del pod o della PVC non riesce perché non è possibile trovare il segreto Kubernetes](#cos_secret_access_fails).</td>
    </tr>
    <tr>
      <td>`cannot get credentials: Wrong Secret Type.Provided secret of type XXXX.Expected type ibm/ibmc-s3fs`</td>
      <td>Il segreto Kubernetes che hai creato per la tua istanza di servizio {{site.data.keyword.cos_full_notm}} non include il `type: ibm/ibmc-s3fs`.</td>
      <td>Modifica il segreto Kubernetes che contiene le tue credenziali {{site.data.keyword.cos_full_notm}} per aggiungere o modificare il `type` in `ibm/ibmc-s3fs`. </td>
    </tr>
    <tr>
      <td>`Bad value for ibm.io/object-store-endpoint XXXX: scheme is missing. Must be of the form http://<hostname> or https://<hostname>`</br> </br> `Bad value for ibm.io/iam-endpoint XXXX: scheme is missing. Must be of the form http://<hostname< or https://<hostname>`</td>
      <td>Il formato dell'endpoint API s3fs o IAM è errato o non è stato possibile richiamare l'endpoint API s3fs sulla base dell'ubicazione del cluster.  </td>
      <td>Vedi [La creazione della PVC non riesce a causa di un endpoint API s3fs errato](#cos_api_endpoint_failure).</td>
    </tr>
    <tr>
      <td>`object-path cannot be set when auto-create is enabled`</td>
      <td>Hai specificato una sottodirectory esistente nel tuo bucket, che vuoi montare nella tua PVC attraverso l'annotazione `ibm.io/object-path`. Se crei una sottodirectory, devi disabilitare la funzionalità di creazione automatica del bucket.  </td>
      <td>Nella tua PVC, imposta `ibm.io/auto-create-bucket: "false"` e fornisci il nome del bucket esistente in `ibm.io/bucket`.</td>
    </tr>
    <tr>
    <td>`bucket auto-create must be enabled when bucket auto-delete is enabled`</td>
    <td>Nella tua PVC, hai impostato `ibm.io/auto-delete-bucket: true` per eliminare automaticamente i tuoi dati, il bucket e la PV quando rimuovi la PVC. Questa opzione richiede che `ibm.io/auto-create-bucket` sia impostato su <strong>true</strong> e che, al contempo, `ibm.io/bucket` sia impostato su `""`.</td>
    <td>Nella tua PVC, imposta `ibm.io/auto-create-bucket: true` e `ibm.io/bucket: ""` in modo che il tuo bucket venga creato automaticamente con un nome in formato `tmp-s3fs-xxxx`. </td>
    </tr>
    <tr>
    <td>`bucket cannot be set when auto-delete is enabled`</td>
    <td>Nella tua PVC, hai impostato `ibm.io/auto-delete-bucket: true` per eliminare automaticamente i tuoi dati, il bucket e la PV quando rimuovi la PVC. Questa opzione richiede che `ibm.io/auto-create-bucket` sia impostato su <strong>true</strong> e che, al contempo, `ibm.io/bucket` sia impostato su `""`.</td>
    <td>Nella tua PVC, imposta `ibm.io/auto-create-bucket: true` e `ibm.io/bucket: ""` in modo che il tuo bucket venga creato automaticamente con un nome in formato `tmp-s3fs-xxxx`. </td>
    </tr>
    <tr>
    <td>`cannot create bucket using API key without service-instance-id`</td>
    <td>Se vuoi utilizzare le chiavi API IAM per accedere alla tua istanza del servizio {{site.data.keyword.cos_full_notm}}, devi archiviare la chiave API e l'ID dell'istanza del servizio {{site.data.keyword.cos_full_notm}} in un segreto Kubernetes.  </td>
    <td>Vedi [Creazione di un segreto per le credenziali del servizio di archiviazione oggetti](/docs/containers?topic=containers-object_storage#create_cos_secret). </td>
    </tr>
    <tr>
      <td>`object-path “<subdirectory_name>” not found inside bucket <bucket_name>`</td>
      <td>Hai specificato una sottodirectory esistente nel tuo bucket, che vuoi montare nella tua PVC attraverso l'annotazione `ibm.io/object-path`. Questa sottodirectory non è stata trovata nel bucket che hai specificato. </td>
      <td>Verifica che la sottodirectory che hai specificato in `ibm.io/object-path` esista nel bucket che hai specificato in `ibm.io/bucket`. </td>
    </tr>
        <tr>
          <td>`BucketAlreadyExists: The requested bucket name is not available. The bucket namespace is shared by all users of the system. Please select a different name and try again.`</td>
          <td>Hai impostato `ibm.io/auto-create-bucket: true` e specificato contemporaneamente un nome bucket o hai specificato un nome bucket già esistente in {{site.data.keyword.cos_full_notm}}. I nomi bucket devono essere univoci tra tutte le istanze del servizio e le regioni in {{site.data.keyword.cos_full_notm}}.  </td>
          <td>Assicurati di impostare `ibm.io/auto-create-bucket: false` e di fornire un nome bucket univoco in {{site.data.keyword.cos_full_notm}}. Se vuoi utilizzare il plug-in {{site.data.keyword.cos_full_notm}} per creare automaticamente un nome bucket, imposta `ibm.io/auto-create-bucket: true` e `ibm.io/bucket: ""`. Il tuo bucket viene creato con un nome univoco in formato `tmp-s3fs-xxxx`. </td>
        </tr>
        <tr>
          <td>`cannot access bucket <bucket_name>: NotFound: Not Found`</td>
          <td>Hai tentato di accedere a un bucket che non hai creato o la classe di archiviazione e l'endpoint API s3fs che hai specificato non corrispondono alla classe di archiviazione e all'endpoint API s3fs che sono stati utilizzati alla creazione del bucket. </td>
          <td>Vedi [Impossibile accedere a un bucket esistente](#cos_access_bucket_fails). </td>
        </tr>
        <tr>
          <td>`Put https://s3-api.dal-us-geo.objectstorage.service.networklayer.com/<bucket_name>: net/http: invalid header field value "AWS4-HMAC-SHA256 Credential=1234a12a123a123a1a123aa1a123a123\n\n/20190412/us-standard/s3/aws4_request, SignedHeaders=host;x-amz-content-sha256;x-amz-date, Signature=12aa1abc123456aabb12aas12aa123456sb123456abc" for key Authorization`</td>
          <td>I valori presenti nel tuo segreto Kubernetes non sono codificati correttamente su base64.  </td>
          <td>Rivedi i valori del tuo segreto Kubernetes e codifica ogni valore su base64. Puoi anche utilizzare il comando [`kubectl create secret`](/docs/containers?topic=containers-object_storage#create_cos_secret) per creare un nuovo segreto e lasciare che Kubernetes codifichi automaticamente i tuoi valori su base64. </td>
        </tr>
        <tr>
          <td>`cannot access bucket <bucket_name>: Forbidden: Forbidden`</td>
          <td>Hai specificato `ibm.io/auto-create-bucket: false` e hai tentato di accedere a un bucket che non hai creato o la chiave di accesso al servizio o l'ID chiave di accesso delle tue credenziali HMAC {{site.data.keyword.cos_full_notm}} non sono corretti.  </td>
          <td>Non puoi accedere a un bucket che non hai creato. Crea un nuovo bucket invece di impostare `ibm.io/auto-create-bucket: true` e `ibm.io/bucket: ""`. Se sei il proprietario del bucket, vedi [La creazione della PVC non riesce a causa di credenziali errate o di accesso rifiutato](#cred_failure) per controllare le tue credenziali. </td>
        </tr>
        <tr>
          <td>`cannot create bucket <bucket_name>: AccessDenied: Access Denied`</td>
          <td>Hai specificato `ibm.io/auto-create-bucket: true` per creare automaticamente un bucket in {{site.data.keyword.cos_full_notm}}, ma alle credenziali che hai fornito nel segreto Kubernetes è stato assegnato il ruolo di accesso del servizio IAM **Lettore**. Questo ruolo non consente la creazione del bucket in {{site.data.keyword.cos_full_notm}}. </td>
          <td>Vedi [La creazione della PVC non riesce a causa di credenziali errate o di accesso rifiutato](#cred_failure). </td>
        </tr>
        <tr>
          <td>`cannot create bucket <bucket_name>: AccessForbidden: Access Forbidden`</td>
          <td>Hai specificato `ibm.io/auto-create-bucket: true` e hai fornito il nome di un bucket esistente in `ibm.io/bucket`. Inoltre, alle credenziali che hai fornito nel segreto Kubernetes è stato assegnato il ruolo di accesso del servizio IAM **Lettore**. Questo ruolo non consente la creazione del bucket in {{site.data.keyword.cos_full_notm}}. </td>
          <td>Per utilizzare un bucket esistente, imposta `ibm.io/auto-create-bucket: false` e fornisci il nome del tuo bucket esistente in `ibm.io/bucket`. Per creare automaticamente un bucket utilizzando un segreto Kubernetes esistente, imposta `ibm.io/bucket: ""` e segui [La creazione della PVC non riesce a causa di credenziali errate o di accesso rifiutato](#cred_failure) per verificare le credenziali del tuo segreto Kubernetes.</td>
        </tr>
        <tr>
          <td>`cannot create bucket <bucket_name>: SignatureDoesNotMatch: The request signature we calculated does not match the signature you provided. Check your AWS Secret Access Key and signing method. For more information, see REST Authentication and SOAP Authentication for details`</td>
          <td>La chiave di accesso segreta {{site.data.keyword.cos_full_notm}} delle credenziali HMAC che hai fornito nel tuo segreto Kubernetes non è corretta. </td>
          <td>Vedi [La creazione della PVC non riesce a causa di credenziali errate o di accesso rifiutato](#cred_failure).</td>
        </tr>
        <tr>
          <td>`cannot create bucket <bucket_name>: InvalidAccessKeyId: The AWS Access Key ID you provided does not exist in our records`</td>
          <td>L'ID chiave di accesso o la chiave di accesso segreta {{site.data.keyword.cos_full_notm}} delle credenziali HMAC che hai fornito nel tuo segreto Kubernetes non sono corretti.</td>
          <td>Vedi [La creazione della PVC non riesce a causa di credenziali errate o di accesso rifiutato](#cred_failure). </td>
        </tr>
        <tr>
          <td>`cannot create bucket <bucket_name>: CredentialsEndpointError: failed to load credentials` </br> </br> `cannot access bucket <bucket_name>: CredentialsEndpointError: failed to load credentials`</td>
          <td>La chiave API {{site.data.keyword.cos_full_notm}} delle tue credenziali IAM e il GUID della tua istanza del servizio {{site.data.keyword.cos_full_notm}} non sono corretti.</td>
          <td>Vedi [La creazione della PVC non riesce a causa di credenziali errate o di accesso rifiutato](#cred_failure). </td>
        </tr>
  </tbody>
    </table>
    

### Archiviazione oggetti: la creazione del pod o della PVC non riesce perché non è possibile trovare il segreto Kubernetes
{: #cos_secret_access_fails}

{: tsSymptoms}
Quando crei la tua PVC o distribuisci un pod che monta la PVC, la creazione o la distribuzione non riescono.

- Messaggio di errore di esempio per un errore di creazione di PVC:
  ```
  cannot get credentials: cannot get secret tsecret-key: secrets "secret-key" not found
  ```
  {: screen}

- Messaggio di errore di esempio per un errore di creazione di pod:
  ```
  persistentvolumeclaim "pvc-3" not found (repeated 3 times)
  ```
  {: screen}

{: tsCauses}
Il segreto Kubernetes dove archivi le tue credenziali del servizio {{site.data.keyword.cos_full_notm}}, la PVC e il pod non si trovano tutti nello stesso spazio dei nomi Kubernetes, Quando il segreto viene distribuito a uno spazio dei nomi differente rispetto alla tua PVC o al tuo pod, non è possibile accedere al segreto.

{: tsResolve}
Questa attività richiede [**Scrittore** o **Gestore** come ruolo del servizio {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform) per tutti gli spazi dei nomi.

1. Elenca tutti i segreti nel tuo cluster e riesamina lo spazio dei nomi Kubernetes dove viene creato il segreto Kubernetes per la tua istanza del servizio {{site.data.keyword.cos_full_notm}}. Il segreto deve mostrare `ibm/ibmc-s3fs` come tipo (**Type**).
   ```
   kubectl get secrets --all-namespaces
   ```
   {: pre}

2. Controlla il tuo file di configurazione YAML per la tua PVC o il tuo pod per verificare che hai utilizzato lo stesso spazio dei nomi. Se vuoi distribuire un pod in uno spazio dei nomi diverso da quello in cui si trova il tuo segreto, [crea un altro segreto](/docs/containers?topic=containers-object_storage#create_cos_secret) in tale spazio dei nomi.

3. Crea la PVC o distribuisci il pod nello spazio dei nomi appropriato.

<br />


### Archiviazione oggetti: la creazione della PVC non riesce a causa di credenziali errate o di accesso rifiutato
{: #cred_failure}

{: tsSymptoms}
Quando crei la PVC, vedi un messaggio di errore simile al seguente:

```
SignatureDoesNotMatch: The request signature we calculated does not match the signature you provided. Check your AWS Secret Access Key and signing method. For more information, see REST Authentication and SOAP Authentication for details.
```
{: screen}

```
AccessDenied: Access Denied status code: 403
```
{: screen}

```
CredentialsEndpointError: failed to load credentials
```
{: screen}

```
InvalidAccessKeyId: The AWS Access Key ID you provided does not exist in our records`
```
{: screen}

```
cannot access bucket <bucket_name>: Forbidden: Forbidden
```
{: screen}


{: tsCauses}
Le credenziali del servizio {{site.data.keyword.cos_full_notm}} che utilizzi per accedere all'istanza del servizio potrebbero essere errate oppure consentire solo l'accesso in lettura al tuo bucket.

{: tsResolve}
1. Nella navigazione nella pagina dei dettagli del servizio, fai clic su **Credenziali del servizio**.
2. Trova le tue credenziali, quindi fai clic su **Visualizza credenziali**.
3. Nella sezione **iam_role_crn**, verifica di avere il ruolo di scrittore (`Writer`) o di gestore (`Manager`). Se non hai il ruolo corretto, devi creare nuove credenziali del servizio {{site.data.keyword.cos_full_notm}} con l'autorizzazione corretta. 
4. Se il ruolo è corretto, accertati di utilizzare l'**access_key_id** e la **secret_access_key** corretti nel tuo segreto Kubernetes. 
5. [Crea un nuovo segreto con l'**access_key_id** e la **secret_access_key** aggiornati](/docs/containers?topic=containers-object_storage#create_cos_secret). 


<br />


### Archiviazione oggetti: la creazione della PVC non riesce a causa di un endpoint API IAM o s3fs errato
{: #cos_api_endpoint_failure}

{: tsSymptoms}
Quando crei la PVC, lo stato di quest'ultima resta in sospeso. Dopo aver eseguito il comando `kubectl describe pvc <pvc_name>`, visualizzi uno dei seguenti messaggi di errore: 

```
Bad value for ibm.io/object-store-endpoint XXXX: scheme is missing. Must be of the form http://<hostname> or https://<hostname>
```
{: screen}

```
Bad value for ibm.io/iam-endpoint XXXX: scheme is missing. Must be of the form http://<hostname> or https://<hostname>
```
{: screen}

{: tsCauses}
Il formato dell'endpoint API s3fs per il bucket che vuoi usare potrebbe essere errato o il tuo cluster viene distribuito in un'ubicazione supportata da {{site.data.keyword.containerlong_notm}}, ma non ancora dal plug-in {{site.data.keyword.cos_full_notm}}. 

{: tsResolve}
1. Controlla l'endpoint API s3fs assegnato automaticamente dal plug-in Helm `ibmc` alle tue classi di archiviazione durante l'installazione del plug-in {{site.data.keyword.cos_full_notm}}. L'endpoint si basa sull'ubicazione in cui viene distribuito il tuo cluster.  
   ```
   kubectl get sc ibmc-s3fs-standard-regional -o yaml | grep object-store-endpoint
   ```
   {: pre}

   Se il comando restituisce `ibm.io/object-store-endpoint: NA`, il tuo cluster viene distribuito in un'ubicazione supportata da {{site.data.keyword.containerlong_notm}}, ma non ancora dal plug-in {{site.data.keyword.cos_full_notm}}. Per aggiungere l'ubicazione al {{site.data.keyword.containerlong_notm}}, pubblica una domanda nel nostro Slack pubblico o apri un caso di supporto {{site.data.keyword.Bluemix_notm}} . Per ulteriori informazioni, vedi [Come ottenere aiuto e supporto](/docs/containers?topic=containers-cs_troubleshoot#ts_getting_help). 
   
2. Se hai aggiunto manualmente l'endpoint API s3fs con l'annotazione `ibm.io/endpoint` o l'endpoint API IAM con l'annotazione`ibm.io/iam-endpoint` nella tua PVC, accertati di aver aggiunto gli endpoint in formato `https://<s3fs_api_endpoint>` e `https://<iam_api_endpoint>`. L'annotazione sovrascrive gli endpoint API impostati automaticamente dal plug-in `ibmc` nelle classi di archiviazione {{site.data.keyword.cos_full_notm}}. 
   ```
   kubectl describe pvc <pvc_name>
   ```
   {: pre}
   
<br />


### Archiviazione oggetti: impossibile accedere a un bucket esistente
{: #cos_access_bucket_fails}

{: tsSymptoms}
Quando crei la PVC, non è possibile accedere al bucket in {{site.data.keyword.cos_full_notm}}. Vedi un messaggio di errore simile al seguente:

```
Failed to provision volume with StorageClass "ibmc-s3fs-standard-regional": pvc:1b2345678b69175abc98y873e2:cannot access bucket <bucket_name>: NotFound: Not Found
```
{: screen}

{: tsCauses}
Potresti aver utilizzato la classe di archiviazione errata per accedere al tuo bucket esistente oppure hai tentato di accedere a un bucket che non hai creato. Non puoi accedere a un bucket che non hai creato. 

{: tsResolve}
1. Dal [dashboard {{site.data.keyword.Bluemix_notm}}![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cloud.ibm.com/), seleziona la tua istanza del servizio {{site.data.keyword.cos_full_notm}}.
2. Seleziona **Bucket**.
3. Riesamina le informazioni su **Classe** e **Ubicazione** per il tuo bucket esistente.
4. Scegli la [classe di archiviazione](/docs/containers?topic=containers-object_storage#cos_storageclass_reference) appropriata.
5. Assicurati di impostare `ibm.io/auto-create-bucket: false` e di fornire il nome corretto del tuo bucket esistente. 

<br />


## Archiviazione oggetti: l'accesso ai file con un utente non root non riesce
{: #cos_nonroot_access}

{: tsSymptoms}
Hai caricato dei file nella tua istanza del servizio {{site.data.keyword.cos_full_notm}} utilizzando la console o l'API REST. Quando provi ad accedere a questi file con un utente non root che hai definito con `runAsUser` nella tua distribuzione dell'applicazione, l'accesso ai file viene rifiutato.

{: tsCauses}
In Linux, un file o una directory hanno 3 gruppi di accesso: `Owner`, `Group` e `Other`. Quando carichi un file in {{site.data.keyword.cos_full_notm}} utilizzando la console o l'API REST, le autorizzazioni per `Owner`, `Group` e `Other` vengono rimosse. L'autorizzazione di ciascun file si presenta così:

```
d--------- 1 root root 0 Jan 1 1970 <file_name>
```
{: screen}

Quando carichi un file utilizzando il plugin {{site.data.keyword.cos_full_notm}}, le autorizzazioni per il file vengono conservate e non modificate.

{: tsResolve}
Per accedere al file con un utente non root, quest'ultimo deve disporre delle autorizzazioni in lettura e scrittura per il file. La modifica dell'autorizzazione di un file come parte della tua distribuzione del pod richiede un'operazione di scrittura. {{site.data.keyword.cos_full_notm}} non è progettato per i carichi di lavoro di scrittura. L'aggiornamento delle autorizzazioni durante la distribuzione del pod potrebbe impedire al tuo pod di passare a uno stato di `Running`.

Per risolvere questo problema, prima di montare la PVC nel tuo pod dell'applicazione, crea un altro pod per impostare l'autorizzazione corretta per l'utente non root.

1. Controlla le autorizzazioni dei tuoi file nel tuo bucket.
   1. Crea un file di configurazione per il tuo pod `test-permission` e denomina il file `test-permission.yaml`.
      ```
      apiVersion: v1
      kind: Pod
      metadata:
        name: test-permission
      spec:
        containers:
        - name: test-permission
          image: nginx
          volumeMounts:
          - name: cos-vol
            mountPath: /test
        volumes:
        - name: cos-vol
          persistentVolumeClaim:
            claimName: <pvc_name>
      ```
      {: codeblock}

   2. Crea il pod `test-permission`.
      ```
      kubectl apply -f test-permission.yaml
      ```
      {: pre}

   3. Accedi al tuo pod.
      ```
      kubectl exec test-permission -it bash
      ```
      {: pre}

   4. Passa al percorso di montaggio ed elenca le autorizzazioni per i tuoi file.
      ```
      cd test && ls -al
      ```
      {: pre}

      Output di esempio:
      ```
      d--------- 1 root root 0 Jan 1 1970 <file_name>
      ```
      {: screen}

2. Elimina il pod.
   ```
   kubectl delete pod test-permission
   ```
   {: pre}

3. Crea un file di configurazione per il pod che utilizzi per correggere le autorizzazioni dei tuoi file e denominalo `fix-permission.yaml`.
   ```
   apiVersion: v1
   kind: Pod
   metadata:
     name: fix-permission
     namespace: <namespace>
   spec:
     containers:
     - name: fix-permission
       image: busybox
       command: ['sh', '-c']
       args: ['chown -R <nonroot_userID> <mount_path>/*; find <mount_path>/ -type d -print -exec chmod u=+rwx,g=+rx {} \;']
       volumeMounts:
       - mountPath: "<mount_path>"
         name: cos-volume
     volumes:
     - name: cos-volume
       persistentVolumeClaim:
         claimName: <pvc_name>
    ```
    {: codeblock}

3. Crea il pod `fix-permission`.
   ```
   kubectl apply -f fix-permission.yaml
   ```
   {: pre}

4. Attendi che il pod passi a uno stato di `Completed`.  
   ```
   kubectl get pod fix-permission
   ```
   {: pre}

5. Elimina il pod `fix-permission`.
   ```
   kubectl delete pod fix-permission
   ```
   {: pre}

5. Ricrea il pod `test-permission` che hai utilizzato in precedenza per controllare le autorizzazioni.
   ```
   kubectl apply -f test-permission.yaml
   ```
   {: pre}

5. Verifica che le autorizzazioni per i tuoi file siano aggiornate.
   1. Accedi al tuo pod.
      ```
      kubectl exec test-permission -it bash
      ```
      {: pre}

   2. Passa al percorso di montaggio ed elenca le autorizzazioni per i tuoi file.
      ```
      cd test && ls -al
      ```
      {: pre}

      Output di esempio:
      ```
      -rwxrwx--- 1 <nonroot_userID> root 6193 Aug 21 17:06 <file_name>
      ```
      {: screen}

6. Elimina il pod `test-permission`.
   ```
   kubectl delete pod test-permission
   ```
   {: pre}

7. Monta la PVC nell'applicazione con l'utente non root.

   Definisci l'utente non root come `runAsUser` senza impostare al tempo stesso `fsGroup` nel tuo YAML di distribuzione. L'impostazione di `fsGroup` attiva il plugin {{site.data.keyword.cos_full_notm}} per aggiornare le autorizzazioni del gruppo per tutti i file in un bucket quando viene distribuito il pod. L'aggiornamento delle autorizzazioni è un'operazione di scrittura e potrebbe impedire al tuo pod di passare a uno stato di `Running`.
   {: important}

Dopo aver impostato le autorizzazioni file corrette nella tua istanza del servizio {{site.data.keyword.cos_full_notm}}, non caricare i file utilizzando la console o l'API REST. Utilizza il plugin {{site.data.keyword.cos_full_notm}} per aggiungere file alla tua istanza del servizio.
{: tip}

<br />


   
## Creazione della PVC non riuscita per autorizzazioni mancanti
{: #missing_permissions}

{: tsSymptoms}
Quando crei una PVC, quest'ultima resta in sospeso. Quando esegui `kubectl describe pvc <pvc_name>`, visualizzi un messaggio di errore simile al seguente: 

```
User doesn't have permissions to create or manage Storage
```
{: screen}

{: tsCauses}
La chiave API IAM o la chiave API dell'infrastruttura IBM Cloud (SoftLayer) archiviata nel segreto Kubernetes `storage-secret-store` del tuo cluster non dispone di tutte le autorizzazioni richieste per eseguire il provisioning dell'archiviazione permanente. 

{: tsResolve}
1. Richiama la chiave IAM o la chiave API dell'infrastruttura IBM Cloud (SoftLayer) archiviata nel segreto Kubernetes `storage-secret-store` del tuo cluster e verifica che venga utilizzata la chiave API corretta. 
   ```
   kubectl get secret storage-secret-store -n kube-system -o yaml | grep slclient.toml: | awk '{print $2}' | base64 --decode
   ```
   {: pre}
   
   Output di esempio: 
   ```
   [Bluemix]
   iam_url = "https://iam.bluemix.net"
   iam_client_id = "bx"
   iam_client_secret = "bx"
   iam_api_key = "<iam_api_key>"
   refresh_token = ""
   pay_tier = "paid"
   containers_api_route = "https://us-south.containers.bluemix.net"

   [Softlayer]
   encryption = true
   softlayer_username = ""
   softlayer_api_key = ""
   softlayer_endpoint_url = "https://api.softlayer.com/rest/v3"
   softlayer_iam_endpoint_url = "https://api.softlayer.com/mobile/v3"
   softlayer_datacenter = "dal10"
   ```
   {: screen}
   
   La chiave API IAM è elencata nella sezione `Bluemix.iam_api_key` dell'output della tua CLI. Se la `Softlayer.softlayer_api_api_key` è al contempo vuota, la chiave API IAM viene utilizzata per determinare le autorizzazioni della tua infrastruttura. La chiave API IAM viene impostata automaticamente dall'utente che esegue la prima azione che richiede il ruolo della piattaforma**Amministratore** IAM in un gruppo di risorse e nella regione. Se in `Softlayer.softlayer_api_key` viene impostata una chiave differente, quest'ultima ha la precedenza rispetto alla chiave API IAM. La `Softlayer.softlayer_api_key` viene impostata quando un amministratore del cluster esegue il comando `ibmcloud ks credentials-set`. 
   
2. Se vuoi modificare le credenziali, aggiorna la chiave API utilizzata. 
    1.  Per aggiornare la chiave API IAM, utilizza il[comando `ibmcloud ks api-key-reset`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_api_key_reset). Per aggiornare la chiave dell'infrastruttura IBM Cloud (SoftLayer), utilizza il [comando `ibmcloud ks credential-set`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_set).
    2. Attendi circa 10-15 minuti l'aggiornamento del segreto Kubernetes `storage-secret-store`, quindi verifica che la chiave sia aggiornata.
       ```
       kubectl get secret storage-secret-store -n kube-system -o yaml | grep slclient.toml: | awk '{print $2}' | base64 --decode
       ```
       {: pre}
   
3. Se la chiave API è corretta, verifica che abbia l'autorizzazione corretta per eseguire il provisioning dell'archiviazione permanente.
   1. Contatta il proprietario dell'account per verificare l'autorizzazione della chiave API. 
   2. Come proprietario dell'account, seleziona **Gestisci** > **Accesso (IAM)** dalla navigazione nella console {{site.data.keyword.Bluemix_notm}}.
   3. Seleziona **Utenti** e cerca l'utente di cui vuoi usare la chiave API. 
   4. Dal menu delle azioni, seleziona **Gestisci i dettagli utente**. 
   5. Vai alla scheda **Infrastruttura classica**. 
   6. Espandi la categoria **Account** e verifica che sia stata assegnata l'autorizzazione **Aggiungi/esegui upgrade dell'archiviazione (StorageLayer)** . 
   7. Espandi la categoria **Servizi** e verifica che sia stata assegnata l'autorizzazione **Gestione archiviazione**. 
4. Rimuovi la PVC con errori. 
   ```
   kubectl delete pvc <pvc_name>
   ```
   {: pre}
   
5. Ricrea la PVC. 
   ```
   kubectl apply -f pvc.yaml
   ```
   {: pre}


## Come ottenere aiuto e supporto
{: #storage_getting_help}

Stai ancora avendo problemi con il tuo cluster?
{: shortdesc}

-  Nel terminale, ricevi una notifica quando sono disponibili degli aggiornamenti ai plugin e alla CLI `ibmcloud`. Assicurati di mantenere la tua CLI aggiornata in modo che tu possa utilizzare tutti i comandi e gli indicatori disponibili.
-   Per vedere se {{site.data.keyword.Bluemix_notm}} è disponibile, [controlla la pagina sugli stati {{site.data.keyword.Bluemix_notm}} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cloud.ibm.com/status?selected=status).
-   Pubblica una domanda in [{{site.data.keyword.containerlong_notm}} Slack ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://ibm-container-service.slack.com).
    Se non stai utilizzando un ID IBM per il tuo account {{site.data.keyword.Bluemix_notm}}, [richiedi un invito](https://bxcs-slack-invite.mybluemix.net/) a questo Slack.
    {: tip}
-   Rivedi i forum per controllare se altri utenti hanno riscontrato gli stessi problemi. Quando utilizzi i forum per fare una domanda, contrassegna con una tag la tua domanda in modo che sia visualizzabile dai team di sviluppo {{site.data.keyword.Bluemix_notm}}.
    -   Se hai domande tecniche sullo sviluppo o la distribuzione di cluster o applicazioni con
{{site.data.keyword.containerlong_notm}}, inserisci la tua domanda in
[Stack Overflow ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://stackoverflow.com/questions/tagged/ibm-cloud+containers) e contrassegnala con le tag `ibm-cloud`, `kubernetes` e `containers`.
    -   Per domande sul servizio e istruzioni introduttive, utilizza il forum
[IBM Developer Answers ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix). Includi le tag `ibm-cloud`
e `containers`.
    Consulta [Come ottenere supporto](/docs/get-support?topic=get-support-getting-customer-support#using-avatar) per ulteriori dettagli sull'utilizzo dei forum.
-   Contatta il supporto IBM aprendo un caso. Per informazioni su come aprire un caso di supporto IBM o sui livelli di supporto e sulla gravità dei casi, consulta [Come contattare il supporto](/docs/get-support?topic=get-support-getting-customer-support).
Quando riporti un problema, includi il tuo ID del cluster. Per ottenere il tuo ID del cluster, esegui `ibmcloud ks clusters`. Puoi anche utilizzare il [{{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool](/docs/containers?topic=containers-cs_troubleshoot#debug_utility) per raccogliere ed esportare informazioni pertinenti dal tuo cluster da condividere con il supporto IBM.
{: tip}

