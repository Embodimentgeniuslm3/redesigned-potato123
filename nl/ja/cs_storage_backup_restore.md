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



# 永続ボリュームのデータのバックアップとリストア
{: #backup_restore}

ファイル共有およびブロック・ストレージは、クラスターと同じゾーンにプロビジョンされます。 サーバーがダウンした場合の可用性を確保するために、{{site.data.keyword.IBM_notm}} は、クラスター化したサーバーでストレージをホストしています。 ただし、ファイル共有およびブロック・ストレージは自動的にはバックアップされないため、ゾーン全体で障害が発生した場合はアクセス不能になる可能性があります。 データの損失や損傷を防止するために、定期バックアップをセットアップすると、必要時にバックアップを使用してデータをリストアできます。
{: shortdesc}

NFS ファイル共有およびブロック・ストレージのバックアップとリストアのための以下のオプションを検討してください。

<dl>
  <dt>定期的なスナップショットをセットアップする</dt>
  <dd><p>NFS ファイル共有またはブロック・ストレージの定期的なスナップショットをセットアップできます。スナップショットとは、特定の時点のインスタンスの状態をキャプチャーした読み取り専用のイメージです。 スナップショットを保管するには、NFS ファイル共有またはブロック・ストレージでスナップショット・スペースを要求する必要があります。 スナップショットは、同じゾーン内の既存のストレージ・インスタンスに保管されます。 ユーザーが誤って重要なデータをボリュームから削除した場合に、スナップショットからデータをリストアできます。 </br></br> <strong>ボリュームのスナップショットを作成するには、以下のようにします。</strong><ol><li>クラスター内の既存の PV をリストします。 <pre class="pre"><code>kubectl get pv</code></pre></li><li>スナップショット・スペースを作成する PV の詳細を取得し、ボリューム ID、サイズ、および IOPS をメモします。 <pre class="pre"><code>kubectl describe pv &lt;pv_name&gt;</code></pre> ファイル・ストレージの場合、ボリューム ID、サイズ、および IOPS は CLI 出力の <strong>Labels</strong> セクションにあります。 ブロック・ストレージの場合、サイズと IOPS は CLI 出力の <strong>Labels</strong> セクションに表示されます。 ボリューム ID は、CLI 出力の <code>ibm.io/network-storage-id</code> アノテーションで確認します。 </li><li>前のステップで取得したパラメーターを使用して、既存のボリュームのスナップショット・サイズを作成します。 <pre class="pre"><code>slcli file snapshot-order --capacity &lt;size&gt; --tier &lt;iops&gt; &lt;volume_id&gt;</code></pre><pre class="pre"><code>slcli block snapshot-order --capacity &lt;size&gt; --tier &lt;iops&gt; &lt;volume_id&gt;</code></pre></li><li>スナップショット・サイズが作成されるまで待ちます。 <pre class="pre"><code>slcli file volume-detail &lt;volume_id&gt;</code></pre><pre class="pre"><code>slcli block volume-detail &lt;volume_id&gt;</code></pre>CLI 出力の <strong>Snapshot Capacity (GB)</strong> が 0 から注文したサイズに変更されていれば、スナップショット・サイズは正常にプロビジョンされています。 </li><li>ボリュームのスナップショットを作成し、作成されたスナップショットの ID をメモします。 <pre class="pre"><code>slcli file snapshot-create &lt;volume_id&gt;</code></pre><pre class="pre"><code>slcli block snapshot-create &lt;volume_id&gt;</code></pre></li><li>スナップショットが正常に作成されたことを確認します。 <pre class="pre"><code>slcli file volume-detail &lt;snapshot_id&gt;</code></pre><pre class="pre"><code>slcli block volume-detail &lt;snapshot_id&gt;</code></pre></li></ol></br><strong>スナップショットから既存のボリュームにデータをリストアするには、以下のようにします。</strong><pre class="pre"><code>slcli file snapshot-restore -s &lt;snapshot_id&gt; &lt;volume_id&gt;</code></pre><pre class="pre"><code>slcli block snapshot-restore -s &lt;snapshot_id&gt; &lt;volume_id&gt;</code></pre></br>詳しくは、以下を参照してください。<ul><li>[NFS の定期的なスナップショット](/docs/infrastructure/FileStorage?topic=FileStorage-snapshots)</li><li>[ブロックの定期的なスナップショット](/docs/infrastructure/BlockStorage?topic=BlockStorage-snapshots#snapshots)</li></ul></p></dd>
  <dt>スナップショットを別のゾーンにレプリケーションする</dt>
 <dd><p>ゾーンの障害からデータを保護するために、別のゾーンにセットアップした NFS ファイル共有またはブロック・ストレージのインスタンスに[スナップショットをレプリケーション](/docs/infrastructure/FileStorage?topic=FileStorage-replication#replication)することができます。 データは、1 次ストレージからバックアップ・ストレージにのみレプリケーションできます。 レプリケーションされた NFS ファイル共有またはブロック・ストレージのインスタンスを、クラスターにマウントすることはできません。 1 次ストレージに障害が発生した場合には、レプリケーションされたバックアップ・ストレージを 1 次ストレージに手動で設定できます。 すると、そのファイル共有をクラスターにマウントできます。 1 次ストレージがリストアされたら、バックアップ・ストレージからデータをリストアできます。</p>
 <p>詳しくは、以下を参照してください。<ul><li>[NFS のスナップショットのレプリケーション](/docs/infrastructure/FileStorage?topic=FileStorage-replication)</li><li>[ブロックのスナップショットのレプリケーション](/docs/infrastructure/BlockStorage?topic=BlockStorage-replication#replication)</li></ul></p></dd>
 <dt>ストレージを複製する</dt>
 <dd><p>NFS ファイル共有またはブロック・ストレージのインスタンスを、元のストレージ・インスタンスと同じゾーンに複製できます。 複製インスタンスのデータは、それを作成した時点の元のストレージ・インスタンスと同じです。 レプリカとは異なり、複製インスタンスは、元のインスタンスから独立したストレージ・インスタンスとして使用します。 複製するには、まず、ボリュームのスナップショットをセットアップします。</p>
 <p>詳しくは、以下を参照してください。<ul><li>[NFS の複製スナップショット](/docs/infrastructure/FileStorage?topic=FileStorage-duplicatevolume#duplicatevolume)</li><li>[ブロックの複製スナップショット](/docs/infrastructure/BlockStorage?topic=BlockStorage-duplicatevolume#duplicatevolume)</li></ul></p></dd>
  <dt>Object Storage にデータをバックアップする</dt>
  <dd><p>[**ibm-backup-restore image**](/docs/services/RegistryImages/ibm-backup-restore?topic=RegistryImages-ibmbackup_restore_starter#ibmbackup_restore_starter) を使用して、クラスター内にバックアップとリストアのポッドをスピンアップできます。 このポッドには、クラスター内の任意の永続ボリューム請求 (PVC) のために 1 回限りのバックアップまたは定期バックアップを実行するスクリプトが含まれています。 データは、ゾーンにセットアップした {{site.data.keyword.objectstoragefull}} インスタンスに保管されます。</p>
  <p>データを可用性をさらに高め、アプリをゾーン障害から保護するには、2 つ目の {{site.data.keyword.objectstoragefull}} インスタンスをセットアップして、ゾーン間でデータを複製します。 {{site.data.keyword.objectstoragefull}} インスタンスからデータをリストアする必要がある場合は、イメージに付属するリストア・スクリプトを使用します。</p></dd>
<dt>ポッドおよびコンテナーとの間でデータをコピーする</dt>
<dd><p>`kubectl cp` [コマンド ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/reference/kubectl/overview/#cp) を使用して、クラスター内のポッドまたは特定のコンテナーとの間でファイルとディレクトリーをコピーできます。</p>
<p>開始前に、以下のことを行います。 [アカウントにログインします。 該当する地域とリソース・グループ (該当する場合) をターゲットとして設定します。 クラスターのコンテキストを設定します](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)。 <code>-c</code> を使用してコンテナーを指定しない場合、コマンドはポッド内で最初に使用可能なコンテナーを使用します。</p>
<p>このコマンドは、以下のようにさまざまな方法で使用できます。</p>
<ul>
<li>ローカル・マシンからクラスター内のポッドにデータをコピーする: <pre class="pre"><code>kubectl cp <var>&lt;local_filepath&gt;/&lt;filename&gt;</var> <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;</var></code></pre></li>
<li>クラスター内のポッドからローカル・マシンにデータをコピーする: <pre class="pre"><code>kubectl cp <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;/&lt;filename&gt;</var> <var>&lt;local_filepath&gt;/&lt;filename&gt;</var></code></pre></li>
<li>クラスター内のあるポッドから別のポッドの特定のコンテナーにデータをコピーする: <pre class="pre"><code>kubectl cp <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;</var> <var>&lt;namespace&gt;/&lt;other_pod&gt;:&lt;pod_filepath&gt;</var> -c <var>&lt;container></var></code></pre></li>
</ul></dd>
  </dl>
