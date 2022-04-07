---

copyright:
  years: 2014, 2019
lastupdated: "2019-03-21"

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



# Backup e ripristino di dati nei volumi persistenti
{: #backup_restore}

Il provisioning delle condivisioni file e dell'archiviazione blocchi viene eseguito nella stessa zona del tuo cluster. L'archiviazione viene ospitata sui server in cluster da {{site.data.keyword.IBM_notm}} per offrire la disponibilità in caso di arresto di un server. Tuttavia, il backup delle condivisioni file e dell'archiviazione blocchi non viene eseguito automaticamente e potrebbero essere inaccessibili se si verifica un malfunzionamento dell'intera zona. Per evitare che i tuoi dati vengano persi o danneggiati, puoi configurare dei backup periodici che puoi utilizzare per ripristinare i dati quando necessario.
{: shortdesc}

Esamina le seguenti opzioni di backup e ripristino per le tue condivisioni file NFS e la tua archiviazione blocchi:

<dl>
  <dt>Configura istantanee periodiche</dt>
  <dd><p>Puoi configurare istantanee periodiche per la tua archiviazione blocchi o di condivisione file NFS, ossia un'immagine di sola lettura che acquisisce lo stato dell'istanza in un punto nel tempo. Per archiviare l'istantanea, devi richiedere lo spazio per l'istantanea nella tua archiviazione blocchi o di condivisione file NFS. Le istantanee vengono archiviate nell'istanza di archiviazione esistente all'interno della stessa zona. Puoi ripristinare i dati da un'istantanea se un utente rimuove accidentalmente dati importanti dal volume. </br></br> <strong>Per creare un'istantanea per il tuo volume:</strong><ol><li>Elenca i PV esistenti nel tuo cluster. <pre class="pre"><code>    kubectl get pv
    </code></pre></li><li>Ottieni i dettagli del PV per cui vuoi creare uno spazio per l'istantanea e prendi nota dell'ID volume, della dimensione e dell'IOPS. <pre class="pre"><code>kubectl describe pv &lt;pv_name&gt;</code></pre> Per l'archiviazione file, l'ID volume, la dimensione e l'IOPS possono essere trovati nella sezione <strong>Etichette</strong> del tuo output della CLI. Per l'archiviazione blocchi, la dimensione e l'IOPS vengono visualizzati nella sezione <strong>Etichette</strong> del tuo output della CLI. Per trovare l'ID volume, controlla l'annotazione <code>ibm.io/network-storage-id</code> del tuo output della CLI. </li><li>Crea la dimensione dell'istantanea per il tuo volume esistente con i parametri che hai richiamato nel passo precedente. <pre class="pre"><code>slcli file snapshot-order --capacity &lt;size&gt; --tier &lt;iops&gt; &lt;volume_id&gt;</code></pre><pre class="pre"><code>slcli block snapshot-order --capacity &lt;size&gt; --tier &lt;iops&gt; &lt;volume_id&gt;</code></pre></li><li>Attendi che la dimensione dell'istantanea venga creata. <pre class="pre"><code>slcli file volume-detail &lt;volume_id&gt;</code></pre><pre class="pre"><code>slcli block volume-detail &lt;volume_id&gt;</code></pre>La dimensione dell'istantanea viene fornita correttamente quando la <strong>Capacità dell'istantanea (GB)</strong> nel tuo output della CLI viene modificata da 0 con la dimensione che hai ordinato. </li><li>Crea l'istantanea per il tuo volume e prendi nota dell'ID dell'istantanea che ti viene creata. <pre class="pre"><code>slcli file snapshot-create &lt;volume_id&gt;</code></pre><pre class="pre"><code>slcli block snapshot-create &lt;volume_id&gt;</code></pre></li><li>Verifica che l'istantanea sia stata creata correttamente. <pre class="pre"><code>slcli file volume-detail &lt;snapshot_id&gt;</code></pre><pre class="pre"><code>slcli block volume-detail &lt;snapshot_id&gt;</code></pre></li></ol></br><strong>Per ripristinare i dati da un'istantanea in un volume esistente: </strong><pre class="pre"><code>slcli file snapshot-restore -s &lt;snapshot_id&gt; &lt;volume_id&gt;</code></pre><pre class="pre"><code>slcli block snapshot-restore -s &lt;snapshot_id&gt; &lt;volume_id&gt;</code></pre></br>Per ulteriori informazioni, consulta:<ul><li>[Istantanee periodiche NFS](/docs/infrastructure/FileStorage?topic=FileStorage-snapshots)</li><li>[Istantanee periodiche blocchi](/docs/infrastructure/BlockStorage?topic=BlockStorage-snapshots#snapshots)</li></ul></p></dd>
  <dt>Replica le istantanee in un'altra zona</dt>
 <dd><p>Per proteggere i tuoi dati da un malfunzionamento della zona, puoi [replicare le istantanee](/docs/infrastructure/FileStorage?topic=FileStorage-replication#replication) in un'istanza di condivisione file NFS o di archiviazione blocchi configurata in un'altra zona. I dati possono essere replicati solo dall'archiviazione primaria a quella di backup. Non puoi montare un'istanza di condivisione file NFS o di archiviazione blocchi replicata in un cluster. Quando la tua archiviazione primaria non funziona più, puoi impostare manualmente la tua archiviazione di backup replicata in modo che sia quella primaria. Quindi, puoi montarla nel tuo cluster. Una volta ripristinata la tua archiviazione primaria, puoi ripristinare i dati dall'archiviazione di backup.</p>
 <p>Per ulteriori informazioni, consulta:<ul><li>[Replica le istantanee per NFS](/docs/infrastructure/FileStorage?topic=FileStorage-replication)</li><li>[Replica le istantanee per Block](/docs/infrastructure/BlockStorage?topic=BlockStorage-replication#replication)</li></ul></p></dd>
 <dt>Duplica l'archiviazione</dt>
 <dd><p>Puoi duplicare la tua istanza di condivisione file NFS o di archiviazione blocchi nella stessa zona dell'istanza di archiviazione originale. Un duplicato contiene gli stessi dati dell'istanza di archiviazione originale nel momento in cui è stato creato il duplicato. A differenza delle repliche, puoi utilizzare il duplicato come un'istanza di archiviazione indipendente dall'originale. Per eseguire la duplicazione, configura innanzitutto le istantanee per il volume.</p>
 <p>Per ulteriori informazioni, consulta:<ul><li>[Istantanee duplicate NFS](/docs/infrastructure/FileStorage?topic=FileStorage-duplicatevolume#duplicatevolume)</li><li>[Istantanee duplicate blocchi](/docs/infrastructure/BlockStorage?topic=BlockStorage-duplicatevolume#duplicatevolume)</li></ul></p></dd>
  <dt>Esegui il backup dei dati in Object Storage</dt>
  <dd><p>Puoi utilizzare l'[**immagine ibm-backup-restore**](/docs/services/RegistryImages/ibm-backup-restore?topic=RegistryImages-ibmbackup_restore_starter#ibmbackup_restore_starter) per avviare un pod di backup e ripristino nel tuo cluster. Questo pod contiene uno script per eseguire un backup una tantum o periodico per qualsiasi attestazione del volume persistente (PVC) nel tuo cluster. I dati vengono archiviati nella tua istanza {{site.data.keyword.objectstoragefull}} che hai configurato in una zona.</p>
  <p>Per rendere i tuoi dati ancora più disponibili e proteggere la tua applicazione da un errore di zona, configura una seconda istanza {{site.data.keyword.objectstoragefull}} e replica i dati tra le varie zone. Se devi ripristinare i dati dalla tua istanza {{site.data.keyword.objectstoragefull}}, utilizza lo script di ripristino fornito con l'immagine.</p></dd>
<dt>Copia i dati nei/dai pod e contenitori</dt>
<dd><p>Puoi utilizzare il comando `kubectl cp` [![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/reference/kubectl/overview/#cp) per copiare i file e le directory in/da pod o specifici contenitori nel tuo cluster.</p>
<p>Prima di iniziare: [accedi al tuo account. Specifica la regione appropriata e, se applicabile, il gruppo di risorse. Imposta il contesto per il tuo cluster](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure). Se non specifichi un contenitore con <code>-c</code>, il comando utilizza il primo contenitore disponibile nel pod.</p>
<p>Puoi utilizzare il comando in diversi modi:</p>
<ul>
<li>Copiare i dati dalla tua macchina locale in un pod nel tuo cluster: <pre class="pre"><code>kubectl cp <var>&lt;local_filepath&gt;/&lt;filename&gt;</var> <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;</var></code></pre></li>
<li>Copiare i dati da un pod nel tuo cluster nella tua macchina locale: <pre class="pre"><code>kubectl cp <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;/&lt;filename&gt;</var> <var>&lt;local_filepath&gt;/&lt;filename&gt;</var></code></pre></li>
<li>Copiare i dati da un pod nel tuo cluster in uno specifico contenitore in un altro pod: <pre class="pre"><code>kubectl cp <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;</var> <var>&lt;namespace&gt;/&lt;other_pod&gt;:&lt;pod_filepath&gt;</var> -c <var>&lt;container></var></code></pre></li>
</ul></dd>
  </dl>
