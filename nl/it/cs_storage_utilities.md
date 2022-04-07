---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: kubernetes, iks, local persistent storage

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



# Programmi di utilità di archiviazione IBM Cloud
{: #utilities}

## Installazione del plugin IBM Cloud Block Storage Attacher (beta)
{: #block_storage_attacher}

Utilizza il plugin {{site.data.keyword.Bluemix_notm}} Block Storage Attacher per collegare l'archiviazione blocchi non elaborata, non formattata e non montata a un nodo di lavoro nel tuo cluster.  
{: shortdesc}

Ad esempio, vuoi archiviare i tuoi dati con una soluzione SDS (software-defined storage), come ad esempio [Portworx](/docs/containers?topic=containers-portworx), ma non vuoi utilizzare i nodi di lavoro bare metal che sono ottimizzati per l'utilizzo di SDS e che sono forniti con dischi locali supplementari. Per aggiungere dischi locali al tuo nodo di lavoro non SDS, devi creare manualmente i tuoi dispositivi di archiviazione blocchi nel tuo account dell'infrastruttura {{site.data.keyword.Bluemix_notm}} e utilizzare il {{site.data.keyword.Bluemix_notm}} Block Volume Attacher per collegare l'archiviazione al tuo nodo di lavoro non SDS.

Il plugin {{site.data.keyword.Bluemix_notm}} Block Volume Attacher crea i pod su ogni nodo di lavoro nel tuo cluster come parte di una serie di daemon e configura una classe di archiviazione Kubernetes che utilizzi successivamente per collegare il dispositivo di archiviazione blocchi al tuo nodo di lavoro non SDS.

Stai cercando le istruzioni su come aggiornare o rimuovere il plugin {{site.data.keyword.Bluemix_notm}} Block Volume Attacher? Vedi [Aggiornamento del plugin](#update_block_attacher) e [Rimozione del plugin](#remove_block_attacher).
{: tip}

1.  [Attieniti alle istruzioni](/docs/containers?topic=containers-helm#public_helm_install) per installare il client Helm sulla tua macchina locale e installare il server Helm (tiller) con un account di servizio nel tuo cluster.

2.  Verifica che tiller sia installato con un account di servizio.

    ```
    kubectl get serviceaccount -n kube-system | grep tiller
    ```
    {: pre}

    Output di esempio:

    ```
    NAME                                 SECRETS   AGE
    tiller                               1         2m
    ```
    {: screen}

3. Aggiorna il repository Helm per richiamare la versione più recente di tutti i grafici Helm in questo repository.
   ```
   helm repo update
   ```
   {: pre}

4. Installa il plugin {{site.data.keyword.Bluemix_notm}} Block Volume Attacher. Quando installi il plugin, al tuo cluster vengono aggiunte le classi di archiviazione blocchi predefinite.
   ```
   helm install iks-charts/ibm-block-storage-attacher --name block-attacher
   ```
   {: pre}

   Output di esempio:
   ```
   NAME:   block-volume-attacher
   LAST DEPLOYED: Thu Sep 13 22:48:18 2018
   NAMESPACE: default
   STATUS: DEPLOYED

   RESOURCES:
   ==> v1beta1/ClusterRoleBinding
   NAME                             AGE
   ibmcloud-block-storage-attacher  1s

   ==> v1beta1/DaemonSet
   NAME                             DESIRED  CURRENT  READY  UP-TO-DATE  AVAILABLE  NODE SELECTOR  AGE
   ibmcloud-block-storage-attacher  0        0        0      0           0          <none>         1s

   ==> v1/StorageClass
   NAME                 PROVISIONER                AGE
   ibmc-block-attacher  ibm.io/ibmc-blockattacher  1s

   ==> v1/ServiceAccount
   NAME                             SECRETS  AGE
   ibmcloud-block-storage-attacher  1        1s

   ==> v1beta1/ClusterRole
   NAME                             AGE
   ibmcloud-block-storage-attacher  1s

   NOTES:
   Thank you for installing: ibmcloud-block-storage-attacher.   Your release is named: block-volume-attacher

   Please refer Chart README.md file for attaching a block storage
   Please refer Chart RELEASE.md to see the release details/fixes
   ```
   {: screen}

5. Verifica che la serie di daemon {{site.data.keyword.Bluemix_notm}} Block Volume Attacher sia stata installata correttamente.
   ```
   kubectl get pod -n kube-system -o wide | grep attacher
   ```
   {: pre}

   Output di esempio:
   ```
   ibmcloud-block-storage-attacher-z7cv6           1/1       Running            0          19m
   ```
   {: screen}

   L'installazione ha avuto esito positivo quando vedi uno o più pod **ibmcloud-block-storage-attacher**. Il numero di pod è uguale al numero di nodi di lavoro nel tuo cluster. Tutti i pod devono essere in uno stato **In esecuzione**.

6. Verifica che la classe di archiviazione per il {{site.data.keyword.Bluemix_notm}} Block Volume Attacher sia stata creata correttamente.
   ```
   kubectl get storageclasses | grep attacher
   ```
   {: pre}

   Output di esempio:
   ```
   ibmc-block-attacher       ibm.io/ibmc-blockattacher   11m
   ```
   {: screen}

### Aggiornamento del plugin IBM Cloud Block Storage Attacher
{: #update_block_attacher}

Puoi eseguire un upgrade del plugin {{site.data.keyword.Bluemix_notm}} Block Storage Attacher alla versione più recente.
{: shortdesc}

1. Aggiorna il repository Helm per richiamare la versione più recente di tutti i grafici helm in questo repository.
   ```
   helm repo update
   ```
   {: pre}

2. Facoltativo: scarica il grafico helm più recente sulla tua macchina locale. Estrai quindi il pacchetto ed esamina il file `release.md` per trovare le informazioni di release più aggiornate.
   ```
   helm fetch iks-charts/ibmcloud-block-storage-plugin
   ```
   {: pre}

3. Trova il nome del grafico Helm per il plugin {{site.data.keyword.Bluemix_notm}} Block Storage Attacher.
   ```
   helm ls | grep ibm-block-storage-attacher
   ```
   {: pre}

   Output di esempio:
   ```
   <helm_chart_name>	1       	Wed Aug  1 14:55:15 2018	DEPLOYED	ibm-block-storage-attacher-1.0.0	default
   ```
   {: screen}

4. Esegui l'upgrade del {{site.data.keyword.Bluemix_notm}} Block Storage Attacher alla versione più recente.
   ```
   helm upgrade --force --recreate-pods <helm_chart_name> ibm-block-storage-attacher
   ```
   {: pre}

### Rimozione del plugin IBM Cloud Block Volume Attacher
{: #remove_block_attacher}

Se non vuoi eseguire il provisioning del plugin {{site.data.keyword.Bluemix_notm}} Block Storage e farne uso nel tuo cluster, puoi disinstallare il grafico Helm.
{: shortdesc}

1. Trova il nome del grafico Helm per il plugin {{site.data.keyword.Bluemix_notm}} Block Storage Attacher.
   ```
   helm ls | grep ibm-block-storage-attacher
   ```
   {: pre}

   Output di esempio:
   ```
   <helm_chart_name>	1       	Wed Aug  1 14:55:15 2018	DEPLOYED	ibm-block-storage-attacher-1.0.0	default
   ```
   {: screen}

2. Elimina il plugin {{site.data.keyword.Bluemix_notm}} Block Storage Attacher rimuovendo il grafico Helm.
   ```
   helm delete <helm_chart_name> --purge
   ```
   {: pre}

3. Verifica che i pod del plugin {{site.data.keyword.Bluemix_notm}} Block Storage Attacher siano stati rimossi.
   ```
   kubectl get pod -n kube-system -o wide | grep attacher
   ```
   {: pre}

   La rimozione dei pod è riuscita se nel tuo output CLI non vengono visualizzati pod.

4. Verifica che la classe di archiviazione {{site.data.keyword.Bluemix_notm}} Block Storage Attacher sia stata rimossa.
   ```
   kubectl get storageclasses | grep attacher
   ```
   {: pre}

   La rimozione della classe di archiviazione è stata eseguita correttamente se nel tuo output della CLI non viene visualizzata alcuna classe di archiviazione.

## Provisioning automatico dell'archiviazione blocchi non formattata e autorizzazione dei tuoi nodi di lavoro ad accedere all'archiviazione
{: #automatic_block}

Puoi utilizzare il plugin {{site.data.keyword.Bluemix_notm}} Block Volume Attacher per aggiungere automaticamente l'archiviazione blocchi non elaborata, non formattata e non montata con la stessa configurazione a tutti i nodi di lavoro nel tuo cluster.
{: shortdesc}

Il contenitore `mkpvyaml` incluso nel plugin {{site.data.keyword.Bluemix_notm}} Block Volume Attacher è configurato per eseguire uno script che trova tutti i nodi di lavoro nel tuo cluster, crea dell'archiviazione blocchi non elaborata nel portale dell'infrastruttura {{site.data.keyword.Bluemix_notm}} e autorizza quindi i nodi di lavoro ad accedere all'archiviazione.

Per aggiungere configurazioni di archiviazione blocchi differenti, aggiungere l'archiviazione blocchi solo a un sottoinsieme di nodi di lavoro oppure per avere maggiore controllo sul processo di provisioning, scegli di [aggiungere manualmente l'archiviazione blocchi](#manual_block).
{: tip}


1. Esegui l'accesso a {{site.data.keyword.Bluemix_notm}} e indica come destinazione il gruppo di risorse in cui si trova il tuo cluster.
   ```
   ibmcloud login
   ```
   {: pre}

2.  Clona il repository {{site.data.keyword.Bluemix_notm}} Storage Utilities.
    ```
    git clone https://github.com/IBM/ibmcloud-storage-utilities.git
    ```
    {: pre}

3. Vai alla directory `block-storage-utilities`.
   ```
   cd ibmcloud-storage-utilities/block-storage-provisioner
   ```
   {: pre}

4. Apri il file `yamlgen.yaml` e specifica la configurazione dell'archiviazione blocchi che desideri aggiungere a ogni nodo di lavoro nel cluster.
   ```
   #
   # Can only specify 'performance' OR 'endurance' and associated clause
   #
   cluster:  <cluster_name>    #  Enter the name of your cluster
   region:   <region>          #  Enter the IBM Cloud Kubernetes Service region where you created your cluster
   type:  <storage_type>       #  Enter the type of storage that you want to provision. Choose between "performance" or "endurance"
   offering: storage_as_a_service
   # performance:
   #    - iops:  <iops>
   endurance:
     - tier:  2                #   [0.25|2|4|10]
   size:  [ 20 ]               #   Enter the size of your storage in gigabytes
   ```
   {: codeblock}

   <table>
   <caption>Descrizione dei componenti del file YAML</caption>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dei componenti del file YAML</th>
   </thead>
   <tbody>
   <tr>
   <td><code>cluster</code></td>
   <td>Immetti il nome del tuo cluster dove desideri aggiungere l'archiviazione blocchi non elaborata. </td>
   </tr>
   <tr>
   <td><code>region</code></td>
   <td>Immetti la regione {{site.data.keyword.containerlong_notm}} dove hai creato il tuo cluster. Esegui <code>[bxcs] cluster-get --cluster &lt;cluster_name_or_ID&gt;</code> per trovare la regione del tuo cluster.  </td>
   </tr>
   <tr>
   <td><code>type</code></td>
   <td>Immetti il tipo di archiviazione di cui desideri eseguire il provisioning. Scegli tra <code>performance</code> o <code>endurance</code>. Per ulteriori informazioni, vedi [Decisioni relative alla tua configurazione dell'archiviazione blocchi](/docs/containers?topic=containers-block_storage#block_predefined_storageclass).  </td>
   </tr>
   <tr>
   <td><code>performance.iops</code></td>
   <td>Se desideri eseguire il provisioning di archiviazione `performance`, immetti il numero di IOPS. Per ulteriori informazioni, vedi [Decisioni relative alla tua configurazione dell'archiviazione blocchi](/docs/containers?topic=containers-block_storage#block_predefined_storageclass). Se desideri eseguire il provisioning di archiviazione `endurance`, rimuovi questa sezione o indicala come commento aggiungendo `#` all'inizio di ciascuna riga.
   </tr>
   <tr>
   <td><code>endurance.tier</code></td>
   <td>Se desideri eseguire il provisioning di archiviazione `endurance`, immetti il numero di IOPS per ogni gigabyte. Ad esempio, se vuoi eseguire il provisioning dell'archiviazione blocchi come è definita nella classe di archiviazione `ibmc-block-bronze`, immetti 2. Per ulteriori informazioni, vedi[Decisioni relative alla tua configurazione dell'archiviazione blocchi](/docs/containers?topic=containers-block_storage#block_predefined_storageclass). Se vuoi eseguire il provisioning di archiviazione `performance`, rimuovi questa sezione o indicala come commento aggiungendo `#` all'inizio di ciascuna riga. </td>
   </tr>
   <tr>
   <td><code>size</code></td>
   <td>Immetti la dimensione della tua archiviazione in gigabyte. Vedi [Decisioni relative alla tua configurazione dell'archiviazione blocchi](/docs/containers?topic=containers-block_storage#block_predefined_storageclass) per trovare dimensioni supportate per il tuo livello di archiviazione. </td>
   </tr>
   </tbody>
   </table>  

5. Richiama il tuo nome utente e la tua chiave API dell'infrastruttura IBM Cloud (SoftLayer). Il nome utente e la chiave API sono utilizzati dallo script `mkpvyaml` per accedere al cluster.
   1. Accedi alla [console {{site.data.keyword.Bluemix_notm}} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cloud.ibm.com/).
   2. Dal menu ![Icona Menu](../icons/icon_hamburger.svg "Icona Menu"), seleziona **Infrastruttura**.
   3. Dalla barra dei menu, seleziona **Account** > **Utenti** > **Elenco utenti**.
   4. Trova il nome di cui desideri richiamare il nome utente e la chiave API.
   5. Fai clic su **Genera** per generare una chiave API o **Visualizza** per visualizzare la tua chiave API esistente. Viene aperta una finestra a comparsa che mostra il nome utente e la chiave API dell'infrastruttura.

6. Memorizza le credenziali in una variabile di ambiente.
   1. Aggiungi le variabili di ambiente.
      ```
      export SL_USERNAME=<infrastructure_username>
      ```
      {: pre}

      ```
      export SL_API_KEY=<infrastructure_api_key>
      ```
      {: pre}

   2. Verifica che le tue variabili di ambiente vengano create correttamente.
      ```
      printenv
      ```
      {: pre}

7.  Crea ed esegui il contenitore `mkpvyaml`. Quando esegui il contenitore dall'immagine, viene eseguito lo script `mkpvyaml.py`. Lo script aggiunge un dispositivo di archiviazione blocchi per ogni nodo di lavoro nel cluster e autorizza ciascun nodo di lavoro ad accedere al dispositivo di archiviazione blocchi. Alla fine dello script, viene generato un file YAML `pv-<cluster_name>.yaml` per te che puoi utilizzare successivamente per creare i volumi persistenti nel cluster.
    1.  Crea il contenitore `mkpvyaml`.
        ```
        docker build -t mkpvyaml .
        ```
        {: pre}
        Output di esempio:
        ```
        Sending build context to Docker daemon   29.7kB
        Step 1/16 : FROM ubuntu:18.10
        18.10: Pulling from library/ubuntu
        5940862bcfcd: Pull complete
        a496d03c4a24: Pull complete
        5d5e0ccd5d0c: Pull complete
        ba24b170ddf1: Pull complete
        Digest: sha256:20b5d52b03712e2ba8819eb53be07612c67bb87560f121cc195af27208da10e0
        Status: Downloaded newer image for ubuntu:18.10
         ---> 0bfd76efee03
        Step 2/16 : RUN apt-get update
         ---> Running in 85cedae315ce
        Get:1 http://security.ubuntu.com/ubuntu cosmic-security InRelease [83.2 kB]
        Get:2 http://archive.ubuntu.com/ubuntu cosmic InRelease [242 kB]
        ...
        Step 16/16 : ENTRYPOINT [ "/docker-entrypoint.sh" ]
         ---> Running in 9a6842f3dbe3
        Removing intermediate container 9a6842f3dbe3
         ---> 7926f5384fc7
        Successfully built 7926f5384fc7
        Successfully tagged mkpvyaml:latest
        ```
        {: screen}
    2.  Esegui il contenitore per eseguire lo script `mkpvyaml.py`.
        ```
        docker run --rm -v `pwd`:/data -v ~/.bluemix:/config -e SL_API_KEY=$SL_API_KEY -e SL_USERNAME=$SL_USERNAME mkpvyaml
        ```
        {: pre}

        Output di esempio:
        ```
        Unable to find image 'portworx/iks-mkpvyaml:latest' locally
        latest: Pulling from portworx/iks-mkpvyaml
        72f45ff89b78: Already exists
        9f034a33b165: Already exists
        386fee7ab4d3: Already exists
        f941b4ac6aa8: Already exists
        fe93e194fcda: Pull complete
        f29a13da1c0a: Pull complete
        41d6e46c1515: Pull complete
        e89af7a21257: Pull complete
        b8a7a212d72e: Pull complete
        5e07391a6f39: Pull complete
        51539879626c: Pull complete
        cdbc4e813dcb: Pull complete
        6cc28f4142cf: Pull complete
        45bbaad87b7c: Pull complete
        05b0c8595749: Pull complete
        Digest: sha256:43ac58a8e951994e65f89ed997c173ede1fa26fb41e5e764edfd3fc12daf0120
        Status: Downloaded newer image for portworx/iks-mkpvyaml:latest

        kube-dal10-abc1d23e123456ac8b12a3c1e12b123a4 10.XXX.XX.XX
        Creating Vol of size 20 with type:  endurance
                  Ordering block storage of size: 20 for host: kube-dal10-abc1d23e123456ac8b12a3c1e12b123a4
                  ORDER ID =  30087291
        kube-dal10-abc1d23e123456ac8b12a3c1e12b123a4 10.XXX.XX.XX
        Creating Vol of size 20 with type:  endurance
                  Ordering block storage of size: 20 for host: kube-dal10-abc1d23e123456ac8b12a3c1e12b123a4
                  ORDER ID =  30087293
        kube-dal12-abc1d23e123456ac8b12a3c1e12b456v1 10.XXX.XX.XX
        Creating Vol of size 20 with type:  endurance
                  Ordering block storage of size: 20 for host: kube-dal12-abc1d23e123456ac8b12a3c1e12b456v1
                  ORDER ID =  30085655
                  No volume yet ... for orderID :  30087291
                  No volume yet ... for orderID :  30087291
                  No volume yet ... for orderID :  30087291
                  No volume yet ... for orderID :  30087291
                  Order ID =  30087291 has created VolId =  12345678
                  Granting access to volume: 12345678 for HostIP: 10.XXX.XX.XX
                  Order ID =  30087293 has created VolId =  87654321
                  Granting access to volume: 87654321 for HostIP: 10.XXX.XX.XX
                  Vol 53002831 is not yet ready ...
                  Granting access to volume: 87654321 for HostIP: 10.XXX.XX.XX
                  Order ID =  30085655 has created VolId =  98712345
                  Granting access to volume: 98712345 for HostIP: 10.XXX.XX.XX
        Output file created as :  pv-<cluster_name>.yaml
        ```
        {: screen}

7. [Collega i dispositivi di archiviazione blocchi ai tuoi nodi di lavoro](#attach_block).

## Aggiunta manuale dell'archiviazione blocchi a specifici nodi di lavoro
{: #manual_block}

Utilizza questa opzione se vuoi aggiungere configurazioni di archiviazione blocchi differenti, aggiungere l'archiviazione blocchi solo a un sottoinsieme di nodi di lavoro oppure per avere maggior controllo sul processo di provisioning.
{: shortdesc}

1. Elenca i nodi di lavoro nel tuo cluster e annota l'indirizzo IP privato e la zona dei nodi di lavoro non SDS dove desideri aggiungere un dispositivo di archiviazione blocchi.
   ```
   ibmcloud ks workers --cluster <cluster_name_or_ID>
   ```
   {: pre}

2. Ripeti i passi 3 e 4 in [Decisioni relative alla tua configurazione dell'archiviazione blocchi](/docs/containers?topic=containers-block_storage#block_predefined_storageclass) per scegliere il tipo, la dimensione e il numero di IOPS per il dispositivo di archiviazione blocchi che vuoi aggiungere al tuo nodo di lavoro non SDS.    

3. Crea il dispositivo di archiviazione blocchi nella stessa zona in cui si trova il tuo nodo di lavoro non SDS.

   **Esempio per il provisioning di un'archiviazione blocchi endurance da 20 GB con due IOPS per ogni GB:**
   ```
   ibmcloud sl block volume-order --storage-type endurance --size 20 --tier 2 --os-type LINUX --datacenter dal10
   ```
   {: pre}

   **Esempio per il provisioning di un'archiviazione blocchi performance da 20 GB con 100 IOPS:**
   ```
   ibmcloud sl block volume-order --storage-type performance --size 20 --iops 100 --os-type LINUX --datacenter dal10
   ```
   {: pre}

4. Verifica che il dispositivo di archiviazione blocchi venga creato e annota l'**`id`** del volume. **Nota:** se non vedi subito il dispositivo di archiviazione blocchi, attendi qualche minuto. Esegui quindi nuovamente questo comando.
   ```
   ibmcloud sl block volume-list
   ```
   {: pre}

   Output di esempio:
   ```
   id         username          datacenter   storage_type                capacity_gb   bytes_used   ip_addr         lunId   active_transactions
   123456789  IBM02SL1234567-8  dal10        performance_block_storage   20            -            161.12.34.123   0       0   
   ```
   {: screen}

5. Esamina i dettagli per il tuo volume e annota **`Target IP`** e **`LUN Id`**.
   ```
   ibmcloud sl block volume-detail <volume_ID>
   ```
   {: pre}

   Output di esempio:
   ```
   Name                       Value
   ID                         1234567890
   User name                  IBM123A4567890-1
   Type                       performance_block_storage
   Capacity (GB)              20
   LUN Id                     0
   IOPs                       100
   Datacenter                 dal10
   Target IP                  161.12.34.123
   # of Active Transactions   0
   Replicant Count            0
   ```
   {: screen}

6. Autorizza il nodo di lavoro non SDS ad accedere al dispositivo di archiviazione blocchi. Sostituisci `<volume_ID>` con l'ID volume del tuo dispositivo di archiviazione blocchi che hai richiamato in precedenza e `<private_worker_IP>` con l'indirizzo IP privato del nodo di lavoro non SDS dove desideri collegare il dispositivo.

   ```
   ibmcloud sl block access-authorize <volume_ID> -p <private_worker_IP>
   ```
   {: pre}

   Output di esempio:
   ```
   The IP address 123456789 was authorized to access <volume_ID>.
   ```
   {: screen}

7. Verifica che il tuo nodo di lavoro non SDS sia correttamente autorizzato e annota i valori di **`host_iqn`**, **`username`** e **`password`**.
   ```
   ibmcloud sl block access-list <volume_ID>
   ```
   {: pre}

   Output di esempio:
   ```
   ID          name                 type   private_ip_address   source_subnet   host_iqn                                      username   password           allowed_host_id
   123456789   <private_worker_IP>  IP     <private_worker_IP>  -               iqn.2018-09.com.ibm:ibm02su1543159-i106288771   IBM02SU1543159-I106288771   R6lqLBj9al6e2lbp   1146581   
   ```
   {: screen}

   L'autorizzazione ha esito positivo quando vengono assegnati **`host_iqn`**, **`username`** e **`password`**.

8. [Collega i dispositivi di archiviazione blocchi ai tuoi nodi di lavoro](#attach_block).


## Collegamento di archiviazione blocchi non elaborata a nodi di lavoro non SDS
{: #attach_block}

Per collegare il dispositivo di archiviazione blocchi a un nodo di lavoro non SDS, devi creare un volume persistente (PV, persistent volume) con la classe di archiviazione {{site.data.keyword.Bluemix_notm}} Block Volume Attacher e i dettagli del tuo dispositivo di archiviazione blocchi.
{: shortdesc}

**Prima di iniziare**:
- Assicurati di aver creato [automaticamente](#automatic_block) o [manualmente](#manual_block) l'archiviazione blocchi non elaborata, non formattata e non montata per i tuoi nodi di lavoro non SDS.
- [Accedi al tuo account. Se applicabile, specifica il gruppo di risorse appropriato. Imposta il contesto per il tuo cluster:](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

**Per collegare l'archiviazione blocchi non elaborata a nodi di lavoro non SDS**:
1. Prepara la creazione del PV.  
   - **Se hai utilizzato il contenitore `mkpvyaml`:**
     1. Apri il file `pv-<cluster_name>.yaml`.
        ```
        nano pv-<cluster_name>.yaml
        ```
        {: pre}

     2. Esamina la configurazione per i tuoi PV.

   - **Se hai aggiunto manualmente l'archiviazione blocchi:**
     1. Crea un file `pv.yaml`. Il seguente comando crea il file con l'editor `nano`.
        ```
        nano pv.yaml
        ```
        {: pre}

     2. Aggiungi i dettagli del tuo dispositivo di archiviazione blocchi al PV.
        ```
        apiVersion: v1
        kind: PersistentVolume
        metadata:
          name: <pv_name>
          annotations:
            ibm.io/iqn: "<IQN_hostname>"
            ibm.io/username: "<username>"
            ibm.io/password: "<password>"
            ibm.io/targetip: "<targetIP>"
            ibm.io/lunid: "<lunID>"
            ibm.io/nodeip: "<private_worker_IP>"
            ibm.io/volID: "<volume_ID>"
        spec:
          capacity:
            storage: <size>
          accessModes:
            - ReadWriteOnce
          hostPath:
              path: /
          storageClassName: ibmc-block-attacher
        ```
        {: codeblock}

        <table>
        <caption>Descrizione dei componenti del file YAML</caption>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dei componenti del file YAML</th>
        </thead>
        <tbody>
      	<tr>
          <td><code>metadata.name</code></td>
      	<td>Immetti un nome per il tuo PV.</td>
      	</tr>
        <tr>
        <td><code>ibm.io/iqn</code></td>
        <td>Immetti il nome host IQN che hai richiamato in precedenza. </td>
        </tr>
        <tr>
        <td><code>ibm.io/username</code></td>
        <td>Immetti il nome utente dell'infrastruttura IBM Cloud (SoftLayer) che hai richiamato in precedenza. </td>
        </tr>
        <tr>
        <td><code>ibm.io/password</code></td>
        <td>Immetti la password dell'infrastruttura IBM Cloud (SoftLayer) che hai richiamato in precedenza. </td>
        </tr>
        <tr>
        <td><code>ibm.io/targetip</code></td>
        <td>Immetti l'IP di destinazione che hai richiamato in precedenza. </td>
        </tr>
        <tr>
        <td><code>ibm.io/lunid</code></td>
        <td>Immetti l'ID LUN del tuo dispositivo di archiviazione blocchi che hai richiamato in precedenza. </td>
        </tr>
        <tr>
        <td><code>ibm.io/nodeip</code></td>
        <td>Immetti l'indirizzo IP privato del nodo di lavoro dove desideri collegare il dispositivo di archiviazione blocchi e che hai autorizzato in precedenza ad accedere al tuo dispositivo di archiviazione blocchi. </td>
        </tr>
        <tr>
          <td><code>ibm.io/volID</code></td>
        <td>Immetti l'ID del volume di archiviazione blocchi che hai richiamato in precedenza. </td>
        </tr>
        <tr>
        <td><code>storage</code></td>
        <td>Immetti la dimensione del dispositivo di archiviazione blocchi che hai creato in precedenza. Ad esempio, se il tuo dispositivo di archiviazione blocchi è 20 gigabyte, immetti <code>20Gi</code>.  </td>
        </tr>
        </tbody>
        </table>
2. Crea il PV per collegare il dispositivo di archiviazione blocchi al tuo nodo di lavoro non SDS.
   - **Se hai utilizzato il contenitore `mkpvyaml`:**
     ```
     kubectl apply -f pv-<cluster_name>.yaml
     ```
     {: pre}

   - **Se hai aggiunto manualmente l'archiviazione blocchi:**
     ```
     kubectl apply -f  block-volume-attacher/pv.yaml
     ```
     {: pre}

3. Verifica che l'archiviazione blocchi sia collegata correttamente al tuo nodo di lavoro.
   ```
   kubectl describe pv <pv_name>
   ```
   {: pre}

   Output di esempio:
   ```
   Name:            kube-wdc07-cr398f790bc285496dbeb8e9137bc6409a-w1-pv1
   Labels:          <none>
   Annotations:     ibm.io/attachstatus=attached
                    ibm.io/dm=/dev/dm-1
                    ibm.io/iqn=iqn.2018-09.com.ibm:ibm02su1543159-i106288771
                    ibm.io/lunid=0
                    ibm.io/mpath=3600a09803830445455244c4a38754c66
                    ibm.io/nodeip=10.176.48.67
                    ibm.io/password=R6lqLBj9al6e2lbp
                    ibm.io/targetip=161.26.98.114
                    ibm.io/username=IBM02SU1543159-I106288771
                    kubectl.kubernetes.io/last-applied-configuration={"apiVersion":"v1","kind":"PersistentVolume","metadata":{"annotations":{"ibm.io/iqn":"iqn.2018-09.com.ibm:ibm02su1543159-i106288771","ibm.io/lunid":"0"...
   Finalizers:      []
   StorageClass:    ibmc-block-attacher
   Status:          Available
   Claim:
   Reclaim Policy:  Retain
   Access Modes:    RWO
   Capacity:        20Gi
   Node Affinity:   <none>
   Message:
   Source:
       Type:          HostPath (bare host directory volume)
       Path:          /
       HostPathType:
   Events:            <none>
   ```
   {: screen}

   Il dispositivo di archiviazione blocchi è collegato correttamente quando **ibm.io/dm** è impostato su un ID dispositivo, come ad esempio `/dev/dm/1`, e puoi vedere **ibm.io/attachstatus=attached** nella sezione **Annotations** dell'output CLI.

Se desideri scollegare un volume, elimina il PV. L'accesso da parte di uno specifico nodo di lavoro ai volumi scollegati continua a essere autorizzato e vengono collegati nuovamente quando crei un nuovo PV con la classe di archiviazione {{site.data.keyword.Bluemix_notm}} Block Volume Attacher per collegare un volume differente allo stesso nodo di lavoro. Per evitare di collegare nuovamente il vecchio volume scollegato, annulla l'autorizzazione del nodo di lavoro ad accedere al volume scollegato utilizzando il comando `ibmcloud sl block access-revoke`. Scollegare il volume non rimuove quest'ultimo dal tuo account dell'infrastruttura IBM Cloud (SoftLayer). Per annullare la fatturazione per il tuo volume, devi [rimuovere l'archiviazione dal tuo account dell'infrastruttura IBM Cloud (SoftLayer)](/docs/containers?topic=containers-cleanup) manualmente.
{: note}


