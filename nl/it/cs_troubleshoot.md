---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-06"

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



# Debug del tuo cluster
{: #cs_troubleshoot}

Mentre utilizzi {{site.data.keyword.containerlong}}, tieni presenti queste tecniche per procedure generiche sulla risoluzione dei problemi e sull'esecuzione del debug per i tuoi cluster. Puoi anche verificare lo [stato del sistema {{site.data.keyword.Bluemix_notm}} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://developer.ibm.com/bluemix/support/#status).
{: shortdesc}

Puoi eseguire questi passi generali per assicurarti che i tuoi cluster siano aggiornati:
- Controlla mensilmente la disponibilità delle patch di sicurezza e del sistema operativo per [aggiornare i tuoi nodi di lavoro](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update).
- [Aggiorna il tuo cluster](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_update) alla [versione di Kubernetes](/docs/containers?topic=containers-cs_versions) più recente predefinita per {{site.data.keyword.containerlong_notm}}<p class="important">Assicurati che la versione del [tuo client CLI `kubectl`](/docs/containers?topic=containers-cs_cli_install#kubectl) corrisponda a quella del server del tuo cluster. [Kubernetes non supporta ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/setup/version-skew-policy/) le versioni client `kubectl` che precedono o seguono di oltre due versioni quella del server (n +/- 2).</p>

## Esecuzione di test con {{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool
{: #debug_utility}

Mentre risolvi i problemi, puoi utilizzare il {{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool per eseguire dei test e raccogliere informazioni pertinenti dal tuo cluster. Per utilizzare lo strumento di debug, installa il [grafico Helm `ibmcloud-iks-debug` ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cloud.ibm.com/kubernetes/helm/iks-charts/ibmcloud-iks-debug):
{: shortdesc}


1. [Configura Helm nel tuo cluster, crea un account di servizio per Tiller e aggiungi il repository `ibm` alla tua istanza Helm](/docs/containers?topic=containers-helm).

2. Installa il grafico Helm nel tuo cluster.
  ```
  helm install ibm/ibmcloud-iks-debug --name debug-tool
  ```
  {: pre}


3. Avvia un server proxy per visualizzare l'interfaccia dello strumento di debug.
  ```
  kubectl proxy --port 8080
  ```
  {: pre}

4. In un browser web, apri l'URL dell'interfaccia dello strumento di debug: http://localhost:8080/api/v1/namespaces/default/services/debug-tool-ibmcloud-iks-debug:8822/proxy/page

5. Seleziona i singoli test oppure un gruppo di test da eseguire. Alcuni test controllano la presenza di potenziali avvertenze, errori o problemi e alcuni testi si limitano a raccogliere informazioni a cui puoi fare riferimento mentre risolvi i problemi. Per ulteriori informazioni sulla funzione di ciascun test, fai clic sull'icona delle informazioni accanto al nome del test.

6. Fai clic su **Run**.

7. Controlla i risultati di ciascun test.
  * Se qualche test non riesce, fai clic sull'icona di informazioni accanto al nome del test nella colonna di sinistra per informazioni su come risolvere il problema.
  * Puoi anche utilizzare i risultati dei test per raccogliere informazioni, come ad esempio gli YAML completi, che possono aiutarti ad eseguire il debug del tuo cluster nelle successive sezioni.

## Debug dei cluster
{: #debug_clusters}

Rivedi le opzioni per il debug dei tuoi cluster e per trovare le cause principali dei malfunzionamenti.

1.  Elenca il tuo cluster e trova lo `Stato` del cluster.

  ```
  ibmcloud ks clusters
  ```
  {: pre}

2.  Esamina lo `Stato` del cluster. Se il tuo cluster si trova in uno stato **Critico**, **Eliminazione non riuscita** o **Avvertenza** o rimane bloccato nello stato **In sospeso** per molto tempo, avvia il [debug dei nodi di lavoro](#debug_worker_nodes).

    Puoi visualizzare lo stato del cluster corrente eseguendo il comando `ibmcloud ks clusters` e individuando il campo **Stato**. 
{: shortdesc}

<table summary="Ogni riga della tabella deve essere letta da sinistra verso destra, con lo stato del cluster nella prima colonna e un descrizione nella seconda colonna.">
<caption>Stati cluster</caption>
   <thead>
   <th>Stato cluster</th>
   <th>Descrizione</th>
   </thead>
   <tbody>
<tr>
   <td>`Aborted`</td>
   <td>L'eliminazione del cluster viene richiesta dall'utente prima che il master Kubernetes venga distribuito. Al termine dell'eliminazione del cluster, questo viene rimosso dal tuo dashboard. Se il tuo cluster rimane bloccato in questo stato per molto tempo, apri un [caso di supporto {{site.data.keyword.Bluemix_notm}}](/docs/containers?topic=containers-cs_troubleshoot#ts_getting_help).</td>
   </tr>
 <tr>
     <td>`Critical`</td>
     <td>Il master Kubernetes non può essere raggiunto o tutti i nodi di lavoro nel cluster sono inattivi. </td>
    </tr>
   <tr>
     <td>`Delete failed`</td>
     <td>Non è possibile eliminare il master Kubernetes o almeno uno dei nodi di lavoro.  </td>
   </tr>
   <tr>
     <td>`Deleted`</td>
     <td>Il cluster viene eliminato ma non ancora rimosso dal tuo dashboard. Se il tuo cluster rimane bloccato in questo stato per molto tempo, apri un [caso di supporto {{site.data.keyword.Bluemix_notm}}](/docs/containers?topic=containers-cs_troubleshoot#ts_getting_help). </td>
   </tr>
   <tr>
   <td>`Deleting`</td>
   <td>Il cluster viene eliminato e la relativa infrastruttura viene smantellata. Non puoi accedere al cluster.  </td>
   </tr>
   <tr>
     <td>`Deploy failed`</td>
     <td>Non è stato possibile completare la distribuzione del master Kubernetes. Non puoi risolvere questo stato. Contatta il supporto IBM Cloud aprendo un [caso di supporto {{site.data.keyword.Bluemix_notm}}](/docs/containers?topic=containers-cs_troubleshoot#ts_getting_help).</td>
   </tr>
     <tr>
       <td>`Deploying`</td>
       <td>Il master Kubernetes non è ancora stato completamente distribuito. Non puoi accedere al tuo cluster. Attendi fino alla completa distribuzione del cluster per verificarne l'integrità.</td>
      </tr>
      <tr>
       <td>`Normal`</td>
       <td>Tutti i nodi di lavoro in un cluster sono attivi e in esecuzione. Puoi accedere al cluster e distribuire le applicazioni al cluster. Questo stato è considerato come integro e non richiede un'azione da parte tua.<p class="note">Anche se i nodi di lavoro possono essere normali, le altre risorse dell'infrastruttura, come [rete](/docs/containers?topic=containers-cs_troubleshoot_network) e [archiviazione](/docs/containers?topic=containers-cs_troubleshoot_storage), potrebbero necessitare ancora di attenzione. Se hai appena creato il cluster, alcune delle sue parti utilizzate da altri servizi, quali i segreti Ingress o i segreti di pull dell'immagine di registro, potrebbero essere ancora in elaborazione.</p></td>
    </tr>
      <tr>
       <td>`Pending`</td>
       <td>Il master Kubernetes è stato distribuito. Sta venendo eseguito il provisioning dei nodi di lavoro e non sono ancora disponibili nel cluster. Puoi accedere al cluster, ma non puoi distribuire le applicazioni al cluster.  </td>
     </tr>
   <tr>
     <td>`Requested`</td>
     <td>Viene inviata una richiesta per creare il cluster e ordinare l'infrastruttura per il master Kubernetes e i nodi di lavoro. Quando viene avviata la distribuzione del cluster, lo stato del cluster cambia in <code>Deploying</code>. Se il tuo cluster rimane bloccato nello stato <code>Requested</code> per molto tempo, apri un [caso di supporto {{site.data.keyword.Bluemix_notm}}](/docs/containers?topic=containers-cs_troubleshoot#ts_getting_help). </td>
   </tr>
   <tr>
     <td>`Updating`</td>
     <td>Il server API Kubernetes eseguito nel master Kubernetes viene aggiornato a una nuova versione dell'API Kubernetes. Durante l'aggiornamento, non è possibile accedere o modificare il cluster. I nodi di lavoro, le applicazioni e le risorse che sono state distribuite dall'utente non vengono modificate e continuano a essere eseguite. Attendi il completamento dell'aggiornamento per verificare l'integrità del tuo cluster. </td>
   </tr>
   <tr>
    <td>`Unsupported`</td>
    <td>La [versione di Kubernetes](/docs/containers?topic=containers-cs_versions#cs_versions) eseguita dal cluster non è più supportata. L'integrità del tuo cluster non è più monitorata attivamente o segnalata. Inoltre, non puoi aggiungere né ricaricare i nodi di lavoro. Per continuare a ricevere importanti aggiornamenti di sicurezza e supporto, devi aggiornare il cluster. Consulta le [azioni di preparazione dell'aggiornamento della versione](/docs/containers?topic=containers-cs_versions#prep-up), quindi [aggiorna il tuo cluster](/docs/containers?topic=containers-update#update) a una versione supportata di Kubernetes.<br><br><p class="note">I cluster la cui versione precede di tre o più versioni la versione più vecchia supportata non possono essere aggiornati. Per evitare questa situazione, puoi aggiornare il cluster a una versione di Kubernetes che precede di non oltre tre versioni quella corrente, come ad esempio dalla 1.12 alla 1.14. Inoltre, se il tuo cluster esegue la versione 1.5, 1.7 o 1.8, la versione è troppo obsoleta per procedere all'aggiornamento. Devi invece [creare un cluster](/docs/containers?topic=containers-clusters#clusters) e [distribuire le tue applicazioni](/docs/containers?topic=containers-app#app) al cluster.</p></td>
   </tr>
    <tr>
       <td>`Warning`</td>
       <td>Almeno un nodo di lavoro nel cluster non è disponibile, ma altri lo sono e possono subentrare nel carico di lavoro. </td>
    </tr>
   </tbody>
 </table>


Il [master Kubernetes](/docs/containers?topic=containers-ibm-cloud-kubernetes-service-technology#architecture) è il componente principale che mantiene operativo il tuo cluster. Il master archivia le risorse del cluster e le loro configurazioni nel database etcd che funge da SPoT (single point of truth) per il tuo cluster. Il server API Kubernetes funge da punto di ingresso principale per tutte le richieste di gestione del cluster dai nodo di lavoro al master oppure quando vuoi interagire con le tue risorse cluster.<br><br>Se si verifica un malfunzionamento del master, i tuoi carichi di lavoro continuano a essere eseguiti sui nodi di lavoro ma non puoi utilizzare i comandi `kubectl` per gestire le tue risorse cluster o visualizzare l'integrità del cluster finché il server API Kubernetes nel master non torna a essere attivo. Se un pod viene disattivato durante l'interruzione del master, non è possibile ripianificarlo finché il nodo di lavoro non potrà raggiungere nuovamente il server API Kubernetes.<br><br>Durante un'interruzione del master, puoi continuare a eseguire i comandi `ibmcloud ks` sull'API {{site.data.keyword.containerlong_notm}} per gestire le tue risorse dell'infrastruttura, quali i nodi di lavoro o le VLAN. Se modifichi la configurazione del cluster corrente aggiungendo o rimuovendo nodi di lavoro nel cluster, le tue modifiche diventeranno effettive solo dopo che il master sarà tornato attivo.

Non riavviare un nodo di lavoro durante un'interruzione del master. Questa azione rimuove i pod dal tuo nodo di lavoro. Poiché il server API Kubernetes non è disponibile, i pod non possono essere ripianificati su altri nodi di lavoro nel cluster.
{: important}


<br />


## Debug dei nodi di lavoro
{: #debug_worker_nodes}

Rivedi le opzioni per eseguire il debug dei nodi di lavoro e trovare le cause principali degli errori.

<ol><li>Se il tuo cluster si trova in uno stato **Critico**, **Eliminazione non riuscita** o **Avvertenza** o rimane bloccato nello stato **In sospeso** per molto tempo, controlla lo stato dei tuoi nodi di lavoro.<p class="pre">ibmcloud ks workers --cluster <cluster_name_or_id></p></li>
<li>Controlla il campo **State** e **Status** per ogni nodo di lavoro nell'output della CLI.<p>Puoi visualizzare lo stato del nodo di lavoro corrente eseguendo il comando `ibmcloud ks workers --cluster <cluster_name_or_ID` e individuando i campi **State** e **Status**.
{: shortdesc}

<table summary="Ogni riga della tabella deve essere letta da sinistra verso destra, con lo stato del cluster nella prima colonna e un descrizione nella seconda colonna.">
<caption>Stati del nodo di lavoro</caption>
  <thead>
  <th>Stato nodo di lavoro</th>
  <th>Descrizione</th>
  </thead>
  <tbody>
<tr>
    <td>`Critical`</td>
    <td>Un nodo di lavoro può entrare in uno stato Critical per diversi motivi: <ul><li>Hai effettuato un riavvio per il tuo nodo di lavoro senza prima delimitarlo e svuotarlo. Il riavvio di un nodo di lavoro può causare il danneggiamento dei dati in <code>containerd</code>, <code>kubelet</code>, <code>kube-proxy</code> e <code>calico</code>. </li>
    <li>I pod distribuiti sul tuo nodo di lavoro non utilizzano limiti di risorse per [memoria ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/tasks/configure-pod-container/assign-memory-resource/) e [CPU ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/tasks/configure-pod-container/assign-cpu-resource/). Senza limiti di risorse, i pod possono consumare tutte le risorse disponibili, senza lasciare risorse per altri pod da eseguire su questo nodo di lavoro. Questo eccesso del carico di lavoro causa un errore per il nodo di lavoro. </li>
    <li><code>containerd</code>, <code>kubelet</code> o <code>calico</code> sono entrati in uno stato irreversibile dopo aver eseguito centinaia o migliaia di contenitori nel tempo. </li>
    <li>Hai configurato una VRA (Virtual Router Appliance) per il tuo nodo di lavoro che è diventato inattivo e ha interrotto le comunicazioni tra il nodo di lavoro e il master Kubernetes. </li><li> Problemi di rete correnti in {{site.data.keyword.containerlong_notm}} o nell'infrastruttura IBM Cloud (SoftLayer) che comportano un errore di comunicazione tra il tuo nodo di lavoro e il master Kubernetes.</li>
    <li>Il tuo nodo di lavoro ha esaurito la capacità. Controlla lo stato (<strong>Status</strong>) del nodo di lavoro per vedere se indica una condizione di <strong>Out of disk</strong> o <strong>Out of memory</strong>. Se il tuo nodo di lavoro ha esaurito la capacità, puoi ridurre il carico di lavoro sul nodo di lavoro o aggiungere un nodo di lavoro al tuo cluster per bilanciare il carico di lavoro.</li>
    <li>Il dispositivo è stato spento dall'[elenco di risorse della console {{site.data.keyword.Bluemix_notm}} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cloud.ibm.com/resources). Apri l'elenco risorse e trova il tuo ID nodo di lavoro nell'elenco **Dispositivi**. Nel menu delle azioni, fai clic su **Accendi**.</li></ul>
    In molti casi, [ricaricare](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) il nodo di lavoro può risolvere il problema. Quando ricarichi il tuo nodo di lavoro, verrà applicata la [versione patch](/docs/containers?topic=containers-cs_versions#version_types)
più recente. La versione principale e quella secondaria non cambiano. Prima di ricaricare il tuo nodo di lavoro, assicurati di delimitarlo e svuotarlo per garantire che i pod esistenti vengano terminati normalmente e ripianificati sui nodi di lavoro rimanenti. </br></br> Se il ricaricamento del nodo di lavoro non risolve il problema, vai al passo successivo per continuare a risolvere il problema. </br></br><strong>Suggerimento:</strong> puoi [configurare i controlli dell'integrità per il nodo di lavoro e abilitare l'Autorecovery](/docs/containers?topic=containers-health#autorecovery). Se Autorecovery rileva un nodo di lavoro non integro in base ai controlli configurati, attiva un'azione correttiva come un ricaricamento del sistema operativo sul nodo di lavoro. Per ulteriori informazioni su come funziona Autorecovery, vedi il [blog Autorecovery ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/blogs/bluemix/2017/12/autorecovery-utilizes-consistent-hashing-high-availability/).
    </td>
   </tr>
   <tr>
   <td>`Deployed`</td>
   <td>Gli aggiornamenti sono stati distribuiti correttamente al tuo nodo di lavoro. Dopo la distribuzione degli aggiornamenti, {{site.data.keyword.containerlong_notm}} avvia un controllo dell'integrità sul nodo di lavoro. Dopo che il controllo dell'integrità è stato eseguito con esito positivo, il nodo di lavoro passa a uno stato di <code>Normal</code>. I nodi di lavoro in uno stato <code>Deployed</code> di norma sono pronti a ricevere carichi di lavoro, che puoi controllare eseguendo <code>kubectl get nodes</code> e confermando che lo stato si presenta come <code>Normal</code>. </td>
   </tr>
    <tr>
      <td>`Deploying`</td>
      <td>Quando aggiorni la versione di Kubernetes del tuo nodo di lavoro, il nodo di lavoro viene ridistribuito per installare gli aggiornamenti. Se ricarichi o riavvii il tuo nodo di lavoro, esso viene ridistribuito per installare automaticamente la versione patch più recente. Se il tuo nodo di lavoro rimane bloccato in questo stato per molto tempo, vai al passo successivo per controllare se si è verificato un problema durante la distribuzione. </td>
   </tr>
      <tr>
      <td>`Normal`</td>
      <td>È stato eseguito il provisioning completo del tuo nodo di lavoro ed è pronto per essere utilizzato nel cluster. Questo stato è considerato come integro e non richiede un'azione da parte dell'utente. **Nota**: anche se i nodi di lavoro possono essere normali, le altre risorse dell'infrastruttura, come [rete](/docs/containers?topic=containers-cs_troubleshoot_network) e [archiviazione](/docs/containers?topic=containers-cs_troubleshoot_storage), potrebbero necessitare ancora di attenzione.</td>
   </tr>
 <tr>
      <td>`Provisioning`</td>
      <td>Sta venendo eseguito il provisioning del tuo nodo di lavoro e non è ancora disponibile nel cluster. Puoi monitorare il processo di provisioning nella colonna <strong>Status</strong> del tuo output CLI. Se il tuo nodo di lavoro rimane bloccato in questo stato per molto tempo, vai al passo successivo per controllare se si è verificato un problema durante il provisioning.</td>
    </tr>
    <tr>
      <td>`Provision_failed`</td>
      <td>Non è stato possibile eseguire il provisioning del tuo nodo di lavoro. Continua con il passo successivo per trovare i dettagli del malfunzionamento.</td>
    </tr>
 <tr>
      <td>`Reloading`</td>
      <td>Il tuo nodo di lavoro sta venendo ricaricato e non è disponibile nel cluster. Puoi monitorare il processo di ricaricamento nella colonna <strong>Status</strong> del tuo output CLI. Se il tuo nodo di lavoro rimane bloccato in questo stato per molto tempo, vai al passo successivo per controllare se si è verificato un problema durante il ricaricamento.</td>
     </tr>
     <tr>
      <td>`Reloading_failed`</td>
      <td>Impossibile ricaricare il tuo nodo di lavoro. Continua con il passo successivo per trovare i dettagli del malfunzionamento.</td>
    </tr>
    <tr>
      <td>`Reload_pending `</td>
      <td>Viene inviata una richiesta per ricaricare o aggiornare la versione Kubernetes del tuo nodo di lavoro. Quando il nodo di lavoro viene ricaricato, lo stato cambia in <code>Reloading</code>. </td>
    </tr>
    <tr>
     <td>`Unknown`</td>
     <td>Il master Kubernetes non è raggiungibile per uno dei seguenti motivi:<ul><li>Hai richiesto un aggiornamento del tuo master Kubernetes. Lo stato del nodo di lavoro non può essere richiamato durante l'aggiornamento. Se il nodo di lavoro rimane in questo stato per un periodo di tempo prolungato anche dopo che il master Kubernetes è stato aggiornato correttamente, prova a [ricaricare](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) il nodo di lavoro.</li><li>Potresti avere un altro firewall che protegge i tuoi nodi di lavoro o hai modificato le impostazioni del firewall recentemente. {{site.data.keyword.containerlong_notm}}
richiede che alcuni indirizzi IP e porte siano aperti per consentire le comunicazioni dal nodo di lavoro al master Kubernetes e viceversa. Per ulteriori informazioni, vedi [Il firewall impedisce la connessione dei nodi di lavoro](/docs/containers?topic=containers-cs_troubleshoot_clusters#cs_firewall).</li><li>Il master Kubernetes è inattivo. Contatta il supporto {{site.data.keyword.Bluemix_notm}} aprendo un [caso di supporto {{site.data.keyword.Bluemix_notm}}](/docs/containers?topic=containers-cs_troubleshoot#ts_getting_help).</li></ul></td>
</tr>
   <tr>
      <td>`Warning`</td>
      <td>Il tuo nodo di lavoro sta raggiungendo il limite di memoria o spazio disco. Puoi ridurre il carico di lavoro nel tuo nodo di lavoro o aggiungere un nodo di lavoro al tuo cluster per bilanciare il carico del carico di lavoro.</td>
</tr>
  </tbody>
</table>
</p></li>
<li>Elenca i dettagli del nodo di lavoro. Se i dettagli includono un messaggio di errore, rivedi l'elenco di [messaggi di errore comuni per i nodi di lavoro](#common_worker_nodes_issues) per imparare come risolvere il problema.<p class="pre">ibmcloud ks worker-get --cluster <cluster_name_or_id> --worker <worker_node_id></li>
</ol>

<br />


## Problemi comuni con i nodi di lavoro
{: #common_worker_nodes_issues}

Controlla i messaggi di errore comuni e impara come risolverli.

  <table>
  <caption>Messaggi di errore comuni</caption>
    <thead>
    <th>Messaggio di errore</th>
    <th>Descrizione e risoluzione
    </thead>
    <tbody>
      <tr>
        <td>Eccezione infrastruttura {{site.data.keyword.Bluemix_notm}}: al tuo account è al momento vietato ordinare le 'Istanze di calcolo'.</td>
        <td>Al tuo account dell'infrastruttura IBM Cloud (SoftLayer) potrebbe essere stato impedito di ordinare le risorse di calcolo. Contatta il supporto {{site.data.keyword.Bluemix_notm}} aprendo un [caso di supporto {{site.data.keyword.Bluemix_notm}}](#ts_getting_help).</td>
      </tr>
      <tr>
      <td>Eccezione infrastruttura {{site.data.keyword.Bluemix_notm}}: impossibile inserire l'ordine.<br><br>
      Eccezione infrastruttura {{site.data.keyword.Bluemix_notm}}: impossibile inserire l'ordine. Le risorse dietro il router 'router_name' non sono sufficienti a soddisfare la richiesta per i seguenti guest: 'worker_id'.</td>
      <td>La zona che hai selezionato potrebbe non avere sufficiente capacità dell'infrastruttura per eseguire il provisioning dei tuoi nodi di lavoro. È anche possibile che tu abbia superato un limite nel tuo account dell'infrastruttura IBM Cloud (SoftLayer). Per risolvere questa condizione, prova una delle seguenti opzioni:
      <ul><li>La disponibilità di risorse dell'infrastruttura può fluttuare spesso. Attendi qualche minuto e riprova.</li>
      <li>Per un cluster a zona singola, crea il cluster in una zona differente. Per un cluster multizona, aggiungi una zona al cluster.</li>
      <li>Specifica una coppia differente di VLAN pubblica e privata per i tuoi nodi di lavoro nel tuo account dell'infrastruttura IBM Cloud (SoftLayer). Per i nodi di lavoro che si trovano in un pool di nodi di lavoro, puoi utilizzare il [comando](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_zone_network_set) <code>ibmcloud ks zone-network-set</code>.</li>
      <li>Contatta il responsabile del tuo account dell'infrastruttura IBM Cloud (SoftLayer) per verificare che non superi un limite dell'account, come ad esempio una quota globale.</li>
      <li>Apri un [caso di supporto dell'infrastruttura IBM Cloud (SoftLayer)](#ts_getting_help)</li></ul></td>
      </tr>
      <tr>
        <td>Eccezione infrastruttura {{site.data.keyword.Bluemix_notm}}: impossibile ottenere la VLAN di rete con ID: <code>&lt;vlan id&gt;</code>.</td>
        <td>Non è stato possibile eseguire il provisioning del tuo nodo di lavoro perché non è stato possibile trovare l'ID della VLAN selezionato per uno dei seguenti motivi:<ul><li>Potresti aver specificato un numero della VLAN invece dell'ID. Il numero della VLAN è di 3 o 4 cifre, mentre l'ID è di 7. Esegui <code>ibmcloud ks vlans --zone &lt;zone&gt;</code> per richiamare l'ID VLAN.<li>L'ID VLAN potrebbe non essere associato all'account dell'infrastruttura IBM Cloud (SoftLayer) da te utilizzato. Esegui <code>ibmcloud ks vlans --zone &lt;zone&gt;</code> per elencare gli ID VLAN disponibili per il tuo account. Per modificare l'account dell'infrastruttura IBM Cloud (SoftLayer) , vedi [`ibmcloud ks credential-set`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_set). </ul></td>
      </tr>
      <tr>
        <td>SoftLayer_Exception_Order_InvalidLocation: l'ubicazione fornita per questo ordine non è valida. (HTTP 500)</td>
        <td>La tua infrastruttura IBM Cloud (SoftLayer) non è configurata per ordinare le risorse di calcolo nel data center selezionato. Contatta il [supporto {{site.data.keyword.Bluemix_notm}}](#ts_getting_help) per verificare che il tuo account sia configurato correttamente.</td>
       </tr>
       <tr>
        <td>Eccezione infrastruttura {{site.data.keyword.Bluemix_notm}}: l'utente non dispone delle autorizzazioni dell'infrastruttura {{site.data.keyword.Bluemix_notm}} necessarie per aggiungere i server
        </br></br>
        Eccezione dell'infrastruttura {{site.data.keyword.Bluemix_notm}}: 'Elemento' deve essere ordinato con l'autorizzazione.
        </br></br>
        Non è stato possibile convalidare le credenziali dell'infrastruttura {{site.data.keyword.Bluemix_notm}}.</td>
        <td>Potresti non disporre delle autorizzazioni necessarie per eseguire l'azione nel tuo portfolio dell'infrastruttura IBM Cloud (SoftLayer) oppure stai utilizzando le credenziali dell'infrastruttura errate. Vedi [Configurazione della chiave API per consentire l'accesso al portfolio dell'infrastruttura](/docs/containers?topic=containers-users#api_key).</td>
      </tr>
      <tr>
       <td>Il nodo di lavoro non riesce a comunicare con i server {{site.data.keyword.containerlong_notm}}. Verifica che la configurazione del firewall consenta il traffico da questo nodo di lavoro.
       <td><ul><li>Se hai un firewall, [configura le impostazioni del tuo firewall per consentire il traffico in uscita alle porte e agli indirizzi IP appropriati](/docs/containers?topic=containers-firewall#firewall_outbound).</li>
       <li>Controlla se il tuo cluster non ha un IP pubblico eseguendo `ibmcloud ks workers --cluster &lt;mycluster&gt;`. Se non viene elencato alcun IP pubblico, il tuo cluster ha solo VLAN private.
       <ul><li>Se vuoi che il cluster abbia solo VLAN private, configura la tua [connessione VLAN](/docs/containers?topic=containers-plan_clusters#private_clusters) e il tuo [firewall](/docs/containers?topic=containers-firewall#firewall_outbound).</li>
       <li>Se hai creato il cluster utilizzando solo l'endpoint servizio privato prima di abilitare il tuo account per [VRF](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) e gli [endpoint del sevizio](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started), i tuoi nodi di lavoro non possono più connettersi al master. Prova a [configurare l'endpoint del servizio pubblico](/docs/containers?topic=containers-cs_network_cluster#set-up-public-se) in modo da poter utilizzare il tuo cluster fino all'elaborazione dei tuoi casi di supporto per aggiornare il tuo account. Se, dopo l'aggiornamento del tuo account, desideri ancora un cluster con solo un endpoint del servizio privato, puoi disabilitare l'endpoint del servizio pubblico.</li>
       <li>Se vuoi che il cluster abbia un IP pubblico, [aggiungi nuovi nodi di lavoro](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_add) con VLAN sia pubbliche che private.</li></ul></li></ul></td>
     </tr>
      <tr>
  <td>Impossibile creare il token del portale IMS e nessun account IMS è collegato all'account BSS selezionato</br></br>L'utente fornito non è stato trovato o non è attivo</br></br>SoftLayer_Exception_User_Customer_InvalidUserStatus: lo stato dell'account utente è attualmente cancel_pending.</br></br>In attesa che la macchina sia visibile all'utente</td>
  <td>Il proprietario della chiave API utilizzata per accedere al portfolio dell'infrastruttura IBM Cloud (SoftLayer) non dispone delle autorizzazioni necessarie per eseguire l'azione o potrebbe essere in attesa di eliminazione.</br></br><strong>Come utente</strong>, attieniti alla seguente procedura:
  <ol><li>Se hai accesso a più account, assicurati di essere collegato all'account in cui desideri lavorare con {{site.data.keyword.containerlong_notm}}. </li>
  <li>Esegui <code>ibmcloud ks api-key-info --cluster &lt;cluster_name_or_ID&gt;</code> per visualizzare il proprietario della chiave API attuale utilizzata per accedere al portfolio dell'infrastruttura IBM Cloud (SoftLayer). </li>
  <li>Esegui <code>ibmcloud account list</code> per visualizzare il proprietario dell'account {{site.data.keyword.Bluemix_notm}} che usi attualmente. </li>
  <li>Contatta il proprietario dell'account {{site.data.keyword.Bluemix_notm}} e segnala che il proprietario della chiave API non dispone di autorizzazioni sufficienti nell'infrastruttura IBM Cloud (SoftLayer) o potrebbe essere in attesa di eliminazione. </li></ol>
  </br><strong>Come proprietario dell'account</strong>, attieniti alla seguente procedura:
  <ol><li>Rivedi le [autorizzazioni necessarie nell'infrastruttura IBM Cloud (SoftLayer)](/docs/containers?topic=containers-users#infra_access) per eseguire l'azione precedentemente non riuscita. </li>
  <li>Correggi le autorizzazioni del proprietario della chiave API o crea una nuova chiave API utilizzando il comando [<code>ibmcloud ks api-key-reset -- region <region></code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_api_key_reset). </li>
  <li>Se tu o un altro amministratore dell'account impostate manualmente le credenziali dell'infrastruttura IBM Cloud (SoftLayer) nel tuo account, esegui [<code>ibmcloud ks credential-unset --region <region></code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_unset) per rimuovere le credenziali dall'account.</li></ol></td>
  </tr>
    </tbody>
  </table>

<br />


## Revisione dell'integrità del master
{: #debug_master}

Il tuo {{site.data.keyword.containerlong_notm}} include un master gestito da IBM con repliche altamente disponibili, aggiornamenti della patch di sicurezza automatici e ripristino automatico in caso di incidente. Puoi controllare l'integrita e lo stato del master cluster eseguendo `ibmcloud ks cluster-get --cluster <cluster_name_or_ID>`.
{: shortdesc} 

**Master Health**<br>
L'integrità del master (**Master Health**) rispecchia lo stato dei componenti del master e segnala l'eventuale necessità di intervento dell'utente. Lo stato di integrità può essere uno dei seguenti:
*   `error`: il master non è operativo. IBM viene avvisato automaticamente e interviene per risolvere il problema. Puoi continuare a monitorare lo stato di integrazione del master finché non diventa `normal`.
*   `normal`: il master è operativo e integro. Non è richiesta alcuna azione.
*   `unavailable`: il master potrebbe non essere accessibile, il che significa che alcune azioni come il ridimensionamento di un pool di nodi di lavoro è temporaneamente non disponibile. IBM viene avvisato automaticamente e interviene per risolvere il problema. Puoi continuare a monitorare lo stato di integrazione del master finché non diventa `normal`. 
*   `unsupported`: il master esegue una versione non supportata di Kubernetes. Devi[aggiornare il tuo cluster](/docs/containers?topic=containers-update) per riportare il master allo stato di integrità `normal`.

**Master Status e Master State**<br>
Il **Master Status** fornisce dettagli sull'operazione in corso da quando è cambiato lo stato del master. Questo stato indica da quanto tempo il master è nello stesso stato, ad esempio `Ready (1 month ago)`. Il **Master State** rispecchia il ciclo di vita delle operazioni che potrebbero essere eseguite sul master, quali la distribuzione, l'aggiornamento e l'eliminazione. La seguente tabella descrive ciascuno stato.

<table summary="Ogni riga della tabella deve essere letta da sinistra a destra, con lo stato del master nella prima colonna e una descrizione nella seconda colonna.">
<caption>Stati del master</caption>
   <thead>
   <th>Stato del master</th>
   <th>Descrizione</th>
   </thead>
   <tbody>
<tr>
   <td>`deployed`</td>
   <td>Il master è stato distribuito correttamente. Controlla lo stato per verificare che il master sia `Ready` o per vedere se è disponibile un aggiornamento.</td>
   </tr>
 <tr>
     <td>`deploying`</td>
     <td>La distribuzione del master è in corso. Attendi che lo stato diventi `deployed` prima di utilizzare il cluster, ad esempio per aggiungere nodi di lavoro.</td>
    </tr>
   <tr>
     <td>`deploy_failed`</td>
     <td>Il master non è stato distribuito. Il supporto IBM riceve una notifica dell'errore e lavora alla risoluzione del problema. Controlla il campo **Master Status** per ulteriori informazioni o attendi che lo stato diventi `deployed`.</td>
   </tr>
   <tr>
   <td>`deleting`</td>
   <td>È in corso l'eliminazione del master a causa dell'eliminazione del cluster. Non puoi annullare un'eliminazione. Una volta eliminato il cluster, non puoi più controllare lo stato del master perché il cluster viene completamente rimosso.</td>
   </tr>
     <tr>
       <td>`delete_failed`</td>
       <td>Il master non è riuscito a effettuare un'eliminazione. Il supporto IBM riceve una notifica dell'errore e lavora alla risoluzione del problema. Non puoi risolvere il problema tentando di eliminare di nuovo il cluster. Verifica, invece, il campo **Master status** per ulteriori informazioni o attendi l'eliminazione del cluster.</td>
      </tr>
      <tr>
       <td>`updating`</td>
       <td>Il master sta aggiornando la sua versione di Kubernetes. L'aggiornamento potrebbe essere un aggiornamento patch applicato automaticamente o una versione principale o secondaria che applichi con l'aggiornamento del cluster. Durante l'aggiornamento, il tuo master altamente disponibile può continuare l'elaborazione delle richieste e i nodi e i carichi di lavoro della tua applicazione continuano a essere eseguiti. Una volta completato l'aggiornamento del master, puoi [aggiornare i tuoi nodi di lavoro](/docs/containers?topic=containers-update#worker_node).<br><br>
       Se l'aggiornamento non ha esito positivo, il master ritorna allo stato `deployed` e continua a essere eseguito nella versione precedente. Il supporto IBM riceve una notifica dell'errore e lavora alla risoluzione del problema. Puoi verificare se l'aggiornamento non è riuscito, nel campo **Master Status**.</td>
    </tr>
   </tbody>
 </table>


<br />



## Debug delle distribuzioni dell'applicazione
{: #debug_apps}

Rivedi le opzioni di cui disponi per il debug delle tue distribuzioni dell'applicazione e per trovare le cause principali degli errori.

Prima di iniziare, assicurati di disporre di [**Scrittore** o **Gestore** come ruolo del servizio {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform) per lo spazio dei nomi dove è distribuita la tua applicazione.

1. Ricerca anomalie nelle risorse di distribuzione o nel servizio eseguendo il comando `describe`.

 Esempio:
 <pre class="pre"><code>kubectl describe service &lt;service_name&gt; </code></pre>

2. [Controlla se i contenitori sono bloccati nello stato `ContainerCreating`](/docs/containers?topic=containers-cs_troubleshoot_storage#stuck_creating_state).

3. Verifica se il cluster è nello stato `Critico`. Se il cluster è in uno stato `Critico`, controlla le regole del firewall e verifica che il master possa comunicare con i nodi di lavoro.

4. Verifica che il servizio sia in ascolto sulla porta corretta.
   1. Richiama il nome del pod.
     <pre class="pre"><code>kubectl get pods</code></pre>
   2. Accedi al contenitore.
     <pre class="pre"><code>kubectl exec -it &lt;pod_name&gt; -- /bin/bash</code></pre>
   3. Esegui curl per l'applicazione dall'interno del contenitore. Se la porta non è accessibile, il servizio potrebbe non essere in ascolto sulla porta corretta o l'applicazione potrebbe avere dei problemi. Aggiorna il file di configurazione per il servizio con la porta corretta e ridistribuisci o investiga sui problemi potenziali dell'applicazione.
     <pre class="pre"><code>curl localhost: &lt;port&gt;</code></pre>

5. Verifica che il servizio sia collegato correttamente ai pod.
   1. Richiama il nome del pod.
     <pre class="pre"><code>kubectl get pods</code></pre>
   2. Accedi al contenitore.
     <pre class="pre"><code>kubectl exec -it &lt;pod_name&gt; -- /bin/bash</code></pre>
   3. Esegui curl per la porta e l'indirizzo IP del cluster del servizio. Se la porta o l'indirizzo IP non sono accessibili, ricerca gli endpoint del servizio. Se non vengono elencati degli endpoint, il selettore del servizio non corrisponde ai pod. Se vengono elencati degli endpoint, ricerca il campo della porta di destinazione nel servizio e assicurati che sia la stessa di quella utilizzata dai pod.
     <pre class="pre"><code>curl &lt;cluster_IP&gt;:&lt;port&gt;</code></pre>

6. Per i servizi Ingress, verifica che il servizio sia accessibile dall'interno del cluster.
   1. Richiama il nome del pod.
     <pre class="pre"><code>kubectl get pods</code></pre>
   2. Accedi al contenitore.
     <pre class="pre"><code>kubectl exec -it &lt;pod_name&gt; -- /bin/bash</code></pre>
   2. Esegui curl dell'URL specificato per il servizio Ingress. Se l'URL non è accessibile, controlla se è presente un errore con il firewall tra il cluster e l'endpoint esterno. 
     <pre class="pre"><code>curl &lt;host_name&gt;.&lt;domain&gt;</code></pre>

<br />



## Come ottenere aiuto e supporto
{: #ts_getting_help}

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

