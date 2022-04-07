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



# Copia de seguridad y restauración de datos en volúmenes persistentes
{: #backup_restore}

Las comparticiones de archivos y el almacenamiento en bloque se suministran en la misma zona que su clúster. {{site.data.keyword.IBM_notm}} aloja el almacenamiento en servidores en clúster para proporcionar disponibilidad en el caso de que el servidor deje de funcionar. Sin embargo, no se realizan automáticamente copias de seguridad de las comparticiones de archivo ni del almacenamiento en bloque, y pueden no ser accesibles si falla toda la zona. Para evitar que los datos que se pierdan o se dañen, puede configurar copias de seguridad periódicas que puede utilizar para restaurar los datos cuando sea necesario.
{: shortdesc}

Consulte las opciones siguientes de copia de seguridad y restauración para las comparticiones de archivo NFS y el almacenamiento en bloque:

<dl>
  <dt>Configurar instantáneas periódicas</dt>
  <dd><p>Configure instantáneas periódicas para su almacenamiento en bloque o de compartición de archivos NFS. Se trata de imágenes de solo lectura que capturan el estado de la instancia en un instante dado. Para almacenar la instantánea, debe solicitar espacio de instantáneas en el almacenamiento en bloques o en el almacenamiento compartido de archivos NFS. Las instantáneas se almacenan en la instancia de almacenamiento existente dentro de la misma zona. Puede restaurar datos desde una instantánea si un usuario elimina accidentalmente datos importantes del volumen. </br></br> <strong>Para crear una instantánea para su volumen: </strong><ol><li>Liste los PV en su clúster. <pre class="pre"><code>kubectl get pv</code></pre></li><li>Obtenga los detalles de los PV para los que desea crear espacio de instantáneas y anote el ID de volumen, el tamaño y las IOPS. <pre class="pre"><code>kubectl describe pv &lt;pv_name&gt;</code></pre> Para almacenamiento de archivos, encontrará el ID de volumen, el tamaño y las IOPS en la sección <strong>Labels</strong> de la salida de la CLI. Para el almacenamiento de bloques, el tamaño y las IOPS se muestran en la sección <strong>Labels</strong> de la salida de la CLI. Para encontrar el ID de volumen, revise la anotación <code>ibm.io/network-storage-id</code> de la salida de la CLI. </li><li>Cree el tamaño de instantánea para el volumen existente con los parámetros que ha recuperado en el paso anterior. <pre class="pre"><code>slcli file snapshot-order --capacity &lt;size&gt; --tier &lt;iops&gt; &lt;volume_id&gt;</code></pre><pre class="pre"><code>slcli block snapshot-order --capacity &lt;size&gt; --tier &lt;iops&gt; &lt;volume_id&gt;</code></pre></li><li>Espere a que se haya creado el tamaño de la instantánea. <pre class="pre"><code>slcli file volume-detail &lt;volume_id&gt;</code></pre><pre class="pre"><code>slcli block volume-detail &lt;volume_id&gt;</code></pre>El tamaño de la instantánea se suministra de forma correcta cuando la <strong>Snapshot Capacity (GB)</strong> en la salida de la CLI cambia de 0 al tamaño solicitado. </li><li>Cree la instantánea para el volumen y anote el ID de la instantánea que se crea para usted. <pre class="pre"><code>slcli file snapshot-create &lt;volume_id&gt;</code></pre><pre class="pre"><code>slcli block snapshot-create &lt;volume_id&gt;</code></pre></li><li>Verifique que la instantánea se haya creado correctamente. <pre class="pre"><code>slcli file volume-detail &lt;snapshot_id&gt;</code></pre><pre class="pre"><code>slcli block volume-detail &lt;snapshot_id&gt;</code></pre></li></ol></br><strong>Para restaurar los datos desde una instantánea en un volumen existente: </strong><pre class="pre"><code>slcli file snapshot-restore -s &lt;snapshot_id&gt; &lt;volume_id&gt;</code></pre><pre class="pre"><code>slcli block snapshot-restore -s &lt;snapshot_id&gt; &lt;volume_id&gt;</code></pre></br>Para obtener más información, consulte:<ul><li>[Instantáneas periódicas NFS](/docs/infrastructure/FileStorage?topic=FileStorage-snapshots)</li><li>[Instantáneas periódicas en bloque](/docs/infrastructure/BlockStorage?topic=BlockStorage-snapshots#snapshots)</li></ul></p></dd>
  <dt>Realice una réplica de las instantáneas en otra zona</dt>
 <dd><p>Para proteger los datos ante un error de zona, puede [replicar instantáneas](/docs/infrastructure/FileStorage?topic=FileStorage-replication#replication) a una compartición de archivos NFS o un almacenamiento en bloque que configurados en otra ubicación. Los datos únicamente se pueden replicar desde el almacenamiento primario al almacenamiento de copia de seguridad. No puede montar una instancia de almacenamiento en bloque o compartición de archivos NFS replicados en un clúster. Cuando el almacenamiento primario falla, puede establecer de forma manual el almacenamiento de copia de seguridad replicado para que sea el primario. A continuación, puede montarla en el clúster. Una vez restaurado el almacenamiento primario, puede restaurar los datos del almacenamiento de copia de seguridad.</p>
 <p>Para obtener más información, consulte:<ul><li>[Replicar instantáneas para NFS](/docs/infrastructure/FileStorage?topic=FileStorage-replication)</li><li>[Replicar instantáneas para Block](/docs/infrastructure/BlockStorage?topic=BlockStorage-replication#replication)</li></ul></p></dd>
 <dt>Duplicar almacenamiento</dt>
 <dd><p>La instancia de almacenamiento en bloque o compartición de archivos NFS se puede duplicar en la misma zona que la instancia de almacenamiento original. La instancia duplicada tiene los mismos datos que la instancia de almacenamiento original en el momento de duplicarla. A diferencia de las réplicas, utilice los duplicados como una instancia de almacenamiento independiente de la original. Para duplicar, primero configure instantáneas para el volumen.</p>
 <p>Para obtener más información, consulte:<ul><li>[Instantáneas duplicadas NFS](/docs/infrastructure/FileStorage?topic=FileStorage-duplicatevolume#duplicatevolume)</li><li>[Instantáneas duplicadas de bloque](/docs/infrastructure/BlockStorage?topic=BlockStorage-duplicatevolume#duplicatevolume)</li></ul></p></dd>
  <dt>Copia de seguridad de datos a Object Storage</dt>
  <dd><p>Puede utilizar la [**imagen ibm-backup-restore**](/docs/services/RegistryImages/ibm-backup-restore?topic=RegistryImages-ibmbackup_restore_starter#ibmbackup_restore_starter) para utilizar un pod de copia de seguridad y restauración en el clúster. Este pod contiene un script para ejecutar una copia de seguridad puntual o periódico para cualquier reclamación de volumen persistente (PVC) en el clúster. Los datos se almacenan en la instancia de {{site.data.keyword.objectstoragefull}} que ha configurado en una zona.</p>
  <p>Para aumentar la alta disponibilidad de los datos y proteger la app ante un error de la zona, configure una segunda instancia de {{site.data.keyword.objectstoragefull}} y replique los datos entre las zonas. Si necesita restaurar datos desde la instancia de {{site.data.keyword.objectstoragefull}}, utilice el script de restauración que se proporciona con la imagen.</p></dd>
<dt>Copiar datos a y desde pods y contenedores</dt>
<dd><p>Utilice el [mandato ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/reference/kubectl/overview/#cp) `kubectl cp` para copiar archivos y directorios a y desde pods o contenedores específicos en el clúster.</p>
<p>Antes de empezar: [Inicie la sesión en su cuenta. Elija como destino la región adecuada y, si procede, el grupo de recursos. Establezca el contexto para el clúster](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure). Si no especifica un contenedor con <code>-c</code>, el mandato utiliza el primer contenedor disponible en el pod.</p>
<p>El mandato se puede utilizar de varias maneras:</p>
<ul>
<li>Copiar datos desde su máquina local a un pod en su clúster: <pre class="pre"><code>kubectl cp <var>&lt;local_filepath&gt;/&lt;filename&gt;</var> <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;</var></code></pre></li>
<li>Copiar datos desde un pod en su clúster a su máquina local: <pre class="pre"><code>kubectl cp <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;/&lt;filename&gt;</var> <var>&lt;local_filepath&gt;/&lt;filename&gt;</var></code></pre></li>
<li>Copiar datos desde un pod en su clúster a un contenedor específico en otro pod: <pre class="pre"><code>kubectl cp <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;</var> <var>&lt;namespace&gt;/&lt;other_pod&gt;:&lt;pod_filepath&gt;</var> -c <var>&lt;container></var></code></pre></li>
</ul></dd>
  </dl>
