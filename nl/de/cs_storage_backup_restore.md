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



# Sichern und Wiederherstellen von Daten auf persistenten Datenträgern
{: #backup_restore}

Dateifreigaben und Blockspeicher werden in derselben Zone bereitgestellt wie Ihr Cluster. Der Speicher wird auf in Gruppen zusammengefassten Servern von {{site.data.keyword.IBM_notm}} gehostet, um Verfügbarkeit sicherzustellen, falls ein Server ausfallen sollte. Dateifreigaben und Blockspeicher werden jedoch nicht automatisch gesichert und sind möglicherweise nicht zugänglich, wenn die gesamte Zone fehlschlägt. Um Ihre Daten vor Verlust oder Beschädigung zu schützen, können Sie regelmäßige Sicherungen konfigurieren, mit denen Sie bei Bedarf Daten wiederherstellen können.
{: shortdesc}

Überprüfen Sie die folgenden Optionen zum Sichern und Wiederherstellen Ihrer NFS-Dateifreigaben und des Blockspeichers:

<dl>
  <dt>Regelmäßige Snapshots konfigurieren</dt>
  <dd><p>Sie können regelmäßige Snapshots für Ihre NFS-Dateifreigaben oder Ihren Blockspeicher konfigurieren. Dies ist ein schreibgeschütztes Image, das den Status des Datenträgers zu einem bestimmten Zeitpunkt erfasst. Um den Snapshot zu speichern, müssen Sie Speicherplatz für den Snapshot auf der NFS-Dateifreigabe oder im Blockspeicher anfordern. Snapshots werden in der in derselben Zone vorhandenen Speicherinstanz gespeichert. Sie können Daten von einem Snapshot wiederherstellen, falls ein Benutzer versehentlich wichtige Daten von dem Datenträger entfernt. </br></br> <strong>Gehen Sie wie folgt vor, um einen Snapshot für den Datenträger zu erstellen: </strong><ol><li>Listen Sie alle vorhandenen PVs in Ihrem Cluster auf. <pre class="pre"><code>    kubectl get pv
    </code></pre></li><li>Rufen Sie die Details für das PV ab, für das Snapshotspeicherplatz angefordert werden soll, und notieren Sie sich die Datenträger-ID, die Größe und die E/A-Operationen pro Sekunde (IOPS). <pre class="pre"><code>kubectl describe pv &lt;pv-name&gt;</code></pre> Für Dateispeicher finden Sie die Datenträger-ID, die Größe und den Wert für IOPS im Abschnitt <strong>Labels</strong> der CLI-Ausgabe. Für Blockspeicher werden die Größe und der Wert für IOPS im Abschnitt <strong>Labels</strong> der CLI-Ausgabe angezeigt. Um die Datenträger-ID zu finden, überprüfen Sie die Annotation <code>ibm.io/network-storage-id</code> der CLI-Ausgabe. </li><li>Erstellen Sie die Snapshotgröße für den vorhandenen Datenträger mit den Parametern, die Sie im vorherigen Schritt abgerufen haben. <pre class="pre"><code>slcli file snapshot-order --capacity &lt;größe&gt; --tier &lt;iops&gt; &lt;datenträger-id&gt;</code></pre><pre class="pre"><code>slcli block snapshot-order --capacity &lt;größe&gt; --tier &lt;iops&gt; &lt;datenträger-id&gt;</code></pre></li><li>Warten Sie, bis die Snapshotgröße erstellt wurde. <pre class="pre"><code>slcli file volume-detail &lt;datenträger-id&gt;</code></pre><pre class="pre"><code>slcli block volume-detail &lt;datenträger-id&gt;</code></pre>Die Snapshotgröße wird erfolgreich bereitgestellt, wenn der Wert für <strong>Snapshot Capacity (GB)</strong> in der CLI-Ausgabe von '0' in die von Ihnen angeforderte Größe geändert wird. </li><li>Erstellen Sie einen Snapshot für den Datenträger und notieren Sie die ID des von Sie erstellten Snapshots. <pre class="pre"><code>slcli file snapshot-create &lt;datenträger-id&gt;</code></pre><pre class="pre"><code>slcli block snapshot-create &lt;datenträger-id&gt;</code></pre></li><li>Überprüfen Sie, dass der Snapshot erfolgreich erstellt wurde. <pre class="pre"><code>slcli file volume-detail &lt;snapshot-id&gt;</code></pre><pre class="pre"><code>slcli block volume-detail &lt;snapshot-id&gt;</code></pre></li></ol></br><strong>Gehen Sie wie folgt vor, um Daten aus einem Snapshot auf einem vorhandenen Datenträger wiederherzustellen: </strong><pre class="pre"><code>slcli file snapshot-restore -s &lt;snapshot_id&gt; &lt;datenträger-id&gt;</code></pre><pre class="pre"><code>slcli block snapshot-restore -s &lt;snapshot-id&gt; &lt;datenträger-id&gt;</code></pre></br>Weitere Informationen finden Sie unter:<ul><li>[Regelmäßige NFS-Snapshots](/docs/infrastructure/FileStorage?topic=FileStorage-snapshots)</li><li>[Regelmäßige Blockspeicher-Snapshots](/docs/infrastructure/BlockStorage?topic=BlockStorage-snapshots#snapshots)</li></ul></p></dd>
  <dt>Snapshots in eine andere Zone replizieren</dt>
 <dd><p>Um Daten vor einem Zonenausfall zu schützen, können Sie in einer NFS-Dateifreigabe oder einer Blockspeicherinstanz, die in einer anderen Zone konfiguriert ist, [Snapshots replizieren](/docs/infrastructure/FileStorage?topic=FileStorage-replication#replication). Daten können nur aus dem primären Speicher an den Sicherungsspeicher repliziert werden. Sie können eine replizierte NFS-Dateifreigabe oder Blockspeicherinstanz nicht an einen Cluster anhängen. Wenn Ihr primärer Speicher fehlschlägt, können Sie Ihren replizierten Sicherungsspeicher manuell als primären Speicher festlegen. Anschließend können Sie ihn an den Cluster anhängen. Nachdem Ihr primärer Speicher wiederhergestellt wurde, können Sie die Daten aus dem Sicherungsspeicher wiederherstellen.</p>
 <p>Weitere Informationen finden Sie unter:<ul><li>[Snapshots für NFS replizieren](/docs/infrastructure/FileStorage?topic=FileStorage-replication)</li><li>[Snapshots für Blockspeicher replizieren](/docs/infrastructure/BlockStorage?topic=BlockStorage-replication#replication)</li></ul></p></dd>
 <dt>Speicher duplizieren</dt>
 <dd><p>Sie können Ihre NFS-Dateifreigabe oder Blockspeicherinstanz in derselben Zone duplizieren, in der sich auch die Originalspeicherinstanz befindet. Ein Duplikat hat dieselben Daten wie die Originalspeicherinstanz zu dem Zeitpunkt, an dem das Duplikat erstellt wurde. Verwenden Sie das Duplikat - im Gegensatz zu den Replikaten - als unabhängige Speicherinstanz. Erstellen Sie zur Vorbereitung einer Duplizierung zunächst Snapshots für den Datenträger.</p>
 <p>Weitere Informationen finden Sie unter:<ul><li>[NFS-Snapshots duplizieren](/docs/infrastructure/FileStorage?topic=FileStorage-duplicatevolume#duplicatevolume)</li><li>[Blockspeicher-Snapshots duplizieren](/docs/infrastructure/BlockStorage?topic=BlockStorage-duplicatevolume#duplicatevolume)</li></ul></p></dd>
  <dt>Daten in Object Storage sichern</dt>
  <dd><p>Sie können den Befehl [**ibm-backup-restore image**](/docs/services/RegistryImages/ibm-backup-restore?topic=RegistryImages-ibmbackup_restore_starter#ibmbackup_restore_starter) verwenden, damit ein Pod für Sicherung und Wiederherstellung in Ihrem Cluster den Betrieb aufnimmt. Dieser Pod enthält ein Script zur Ausführung einer einmaligen oder regelmäßigen Sicherung für alle PVCs (Persistent Volume Claims) in Ihrem Cluster. Die Daten werden in Ihrer {{site.data.keyword.objectstoragefull}}-Instanz gespeichert, die Sie in einer Zone konfiguriert haben.</p>
  <p>Damit Ihre Daten noch besser hoch verfügbar sind und um Ihre App vor einem Zonenausfall zu schützen, konfigurieren Sie eine zweite {{site.data.keyword.objectstoragefull}}-Instanz und replizieren Sie die Daten zonenübergreifend. Falls Sie Daten von Ihrer {{site.data.keyword.objectstoragefull}}-Instanz wiederherstellen müssen, verwenden Sie das Wiederherstellungsscript, das mit dem Image bereitgestellt wird.</p></dd>
<dt>Daten in und aus Pods und Containern kopieren</dt>
<dd><p>Sie können den [Befehl ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/reference/kubectl/overview/#cp) `kubectl cp` verwenden, um Dateien und Verzeichnisse in und aus Pods oder spezifischen Containern in Ihrem Cluster zu kopieren.</p>
<p>Vorbereitende Schritte: [Melden Sie sich an Ihrem Konto an. Geben Sie als Ziel die entsprechende Region und, sofern zutreffend, die Ressourcengruppe an. Legen Sie den Kontext für den Cluster fest.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure) Wenn Sie keinen Container mit <code>-c</code> angeben, verwendet der Befehl den ersten verfügbaren Container im Pod.</p>
<p>Sie können den Befehl auf verschiedene Weisen verwenden:</p>
<ul>
<li>Kopieren Sie Daten von Ihrer lokalen Maschine in einen Pod in Ihrem Cluster: <pre class="pre"><code>kubectl cp <var>&lt;lokaler_dateipfad&gt;/&lt;dateiname&gt;</var> <var>&lt;namensbereich&gt;/&lt;pod&gt;:&lt;dateipfad_des_pods&gt;</var></code></pre></li>
<li>Kopieren Sie Daten von einem Pod in Ihrem Cluster auf Ihre lokale Maschine: <pre class="pre"><code>kubectl cp <var>&lt;namensbereich&gt;/&lt;pod&gt;:&lt;dateipfad_des_pods&gt;/&lt;dateiname&gt;</var> <var>&lt;lokaler_dateipfad&gt;/&lt;dateiname&gt;</var></code></pre></li>
<li>Kopieren Sie Daten von einem Pod in Ihrem Cluster in einen spezifischen Container in einem anderen Pod: <pre class="pre"><code>kubectl cp <var>&lt;namensbereich&gt;/&lt;pod&gt;:&lt;dateipfad_des_pods&gt;</var> <var>&lt;namensbereich&gt;/&lt;anderer_pod&gt;:&lt;dateipfad_des_pods&gt;</var> -c <var>&lt;container></var></code></pre></li>
</ul></dd>
  </dl>
