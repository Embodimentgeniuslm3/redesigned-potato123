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



# 備份及還原持續性磁區中的資料
{: #backup_restore}

檔案共用及區塊儲存空間會佈建至與叢集相同的區域。儲存空間是在叢集化伺服器上由 {{site.data.keyword.IBM_notm}} 管理，以在伺服器關閉時提供可用性。不過，如果整個區域失敗，則不會自動備份檔案共用及區塊儲存空間，且可能無法存取它們。若要避免資料遺失或損壞，您可以設定定期備份，以在需要時使用它們來還原您的資料。
{: shortdesc}

檢閱 NFS 檔案共用及區塊儲存空間的下列備份及還原選項：

<dl>
  <dt>設定定期 Snapshot</dt>
  <dd><p>您可以針對 NFS 檔案共用或區塊儲存空間設定定期 Snapshot，這是唯讀映像檔，會擷取實例在某個時間點的狀況。若要儲存 Snapshot，您必須在 NFS 檔案共用或區塊儲存空間上要求 Snapshot 空間。Snapshot 儲存於相同區域內的現有儲存空間實例上。如果使用者不小心從磁區移除重要資料，您可以從 Snapshot 還原資料。</br></br> <strong>若要建立磁區的 Snapshot，請執行下列動作：</strong><ol><li>列出叢集裡的現有 PV。<pre class="pre"><code>    kubectl get pv
    </code></pre></li><li>取得您要建立 Snapshot 空間之 PV 的詳細資料，並記下磁區 ID、大小及 IOPS。<pre class="pre"><code>kubectl describe pv &lt;pv_name&gt;</code></pre> 若為檔案儲存空間，可以在 CLI 輸出的 <strong>Labels</strong> 區段中找到磁區 ID、大小及 IOPS。若為區塊儲存空間，CLI 輸出的 <strong>Labels</strong> 區段中則會顯示大小及 IOPS。若要尋找磁區 ID，請檢閱 CLI 輸出的 <code>ibm.io/network-storage-id</code> 註釋。</li><li>使用您在前一個步驟中擷取的參數，建立現有磁區的 Snapshot 大小。<pre class="pre"><code>slcli file snapshot-order --capacity &lt;size&gt; --tier &lt;iops&gt; &lt;volume_id&gt;</code></pre><pre class="pre"><code>slcli block snapshot-order --capacity &lt;size&gt; --tier &lt;iops&gt; &lt;volume_id&gt;</code></pre></li><li>等待要建立的 Snapshot 大小。<pre class="pre"><code>slcli file volume-detail &lt;volume_id&gt;</code></pre><pre class="pre"><code>slcli block volume-detail &lt;volume_id&gt;</code></pre>CLI 輸出中的 <strong>Snapshot 容量 (GB)</strong> 從 0 變更為您訂購的大小時，即已順利佈建 Snapshot 大小。</li><li>為您的磁區建立 Snapshot，並記下為您建立的 Snapshot ID。<pre class="pre"><code>slcli file snapshot-create &lt;volume_id&gt;</code></pre><pre class="pre"><code>slcli block snapshot-create &lt;volume_id&gt;</code></pre></li><li>驗證已順利建立 Snapshot。<pre class="pre"><code>slcli file volume-detail &lt;snapshot_id&gt;</code></pre><pre class="pre"><code>slcli block volume-detail &lt;snapshot_id&gt;</code></pre></li></ol></br><strong>若要將資料從 Snapshot 還原至現有磁區，請執行下列動作：</strong><pre class="pre"><code>slcli file snapshot-restore -s &lt;snapshot_id&gt; &lt;volume_id&gt;</code></pre><pre class="pre"><code>slcli block snapshot-restore -s &lt;snapshot_id&gt; &lt;volume_id&gt;</code></pre></br>如需相關資訊，請參閱：<ul><li>[NFS 定期 Snapshot](/docs/infrastructure/FileStorage?topic=FileStorage-snapshots)</li><li>[區塊定期 Snapshot](/docs/infrastructure/BlockStorage?topic=BlockStorage-snapshots#snapshots)</li></ul></p></dd>
  <dt>將 Snapshot 抄寫至另一個區域</dt>
 <dd><p>若要在發生區域失敗時保護資料，您可以[將 Snapshot 抄寫](/docs/infrastructure/FileStorage?topic=FileStorage-replication#replication)至另一個區域中設定的 NFS 檔案共用或區塊儲存空間實例。資料只能從主要儲存空間抄寫至備份儲存空間。您無法將抄寫的 NFS 檔案共用或區塊儲存空間實例裝載至叢集。當主要儲存空間失敗時，您可以手動將抄寫的備份儲存空間設為主要儲存空間。然後，您可以將它裝載至叢集。還原主要儲存空間之後，您可以從備份儲存空間中還原資料。</p>
 <p>如需相關資訊，請參閱：<ul><li>[抄寫 NFS 的 Snapshot](/docs/infrastructure/FileStorage?topic=FileStorage-replication)</li><li>[抄寫區塊的 Snapshot](/docs/infrastructure/BlockStorage?topic=BlockStorage-replication#replication)</li></ul></p></dd>
 <dt>複製儲存空間</dt>
 <dd><p>您可以在與原始儲存空間實例相同的區域中，複製 NFS 檔案共用或區塊儲存空間實例。在建立複本的時間點，複本具有與原始儲存空間實例相同的資料。與抄本不同，請使用複本作為獨立於原始儲存空間實例外的儲存空間實例。若要複製，請先設定磁區的 Snapshot。</p>
 <p>如需相關資訊，請參閱：<ul><li>[NFS 複製 Snapshot](/docs/infrastructure/FileStorage?topic=FileStorage-duplicatevolume#duplicatevolume)</li><li>[區塊複製 Snapshot](/docs/infrastructure/BlockStorage?topic=BlockStorage-duplicatevolume#duplicatevolume)</li></ul></p></dd>
  <dt>將資料備份至物件儲存空間</dt>
  <dd><p>您可以使用 [**ibm-backup-restore image**](/docs/services/RegistryImages/ibm-backup-restore?topic=RegistryImages-ibmbackup_restore_starter#ibmbackup_restore_starter)，在叢集裡啟動一個備份及還原 Pod。這個 Pod 包含一個 Script，它會針對叢集裡的任何持續性磁區要求 (PVC) 執行一次性或定期備份。資料會儲存在您於區域中設定的 {{site.data.keyword.objectstoragefull}} 實例。</p>
  <p>若要讓資料有更高的可用性，並在發生區域失敗時保護應用程式，請設定第二個 {{site.data.keyword.objectstoragefull}} 實例，並在區域之間抄寫資料。如果您需要從 {{site.data.keyword.objectstoragefull}} 實例還原資料，請使用隨該映像檔所提供的還原 Script。</p></dd>
<dt>在 Pod 與容器之間複製資料</dt>
<dd><p>您可以使用 `kubectl cp` [指令 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/reference/kubectl/overview/#cp)，在叢集的 Pod 或特定容器之間複製檔案及目錄。</p>
<p>開始之前：[登入您的帳戶。將目標設為適當的地區及（如果適用的話）資源群組。設定叢集的環境定義](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)。如果未使用 <code>-c</code> 來指定容器，則指令會使用 Pod 中第一個可用的容器。</p>
<p>您可以透過下列各種方式來使用指令：</p>
<ul>
<li>將資料從本端機器複製到叢集裡的 Pod：<pre class="pre"><code>kubectl cp <var>&lt;local_filepath&gt;/&lt;filename&gt;</var> <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;</var></code></pre></li>
<li>將資料從叢集裡的 Pod 複製到本端機器：<pre class="pre"><code>kubectl cp <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;/&lt;filename&gt;</var> <var>&lt;local_filepath&gt;/&lt;filename&gt;</var></code></pre></li>
<li>將資料從叢集裡的 Pod 複製到另一個 Pod 中的特定容器：<pre class="pre"><code>kubectl cp <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;</var> <var>&lt;namespace&gt;/&lt;other_pod&gt;:&lt;pod_filepath&gt;</var> -c <var>&lt;container></var></code></pre></li>
</ul></dd>
  </dl>
