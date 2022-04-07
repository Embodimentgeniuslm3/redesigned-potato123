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



# 备份和复原持久卷中的数据
{: #backup_restore}

文件共享和块存储器会供应到集群所在的专区。存储器由 {{site.data.keyword.IBM_notm}} 在集群服务器上托管，以在其中某个服务器停止运行时提供可用性。但是，文件共享和块存储器不会自动进行备份，因此它们在整个专区发生故障时可能无法进行访问。为了防止数据丢失或损坏，可以设置定期备份，以便在需要时可用于复原数据。
{: shortdesc}

查看 NFS 文件共享和块存储器的以下备份和复原选项：

<dl>
  <dt>设置定期快照</dt>
  <dd><p>可以为 NFS 文件共享或块存储器设置定期快照，这是捕获某个时间点的实例状态的只读映像。要存储快照，必须在 NFS 文件共享或块存储器上请求快照空间。快照会存储在同一专区的现有存储器实例上。如果用户意外地从卷中除去了重要数据，那么可以通过快照来复原数据。</br></br> <strong>要为卷创建快照，请执行以下操作：</strong><ol><li>列出集群中的现有 PV。<pre class="pre"><code>    kubectl get pv
    </code></pre></li><li>获取要为其创建快照空间的 PV 的详细信息，并记下卷标识、大小和 IOPS。<pre class="pre"><code>kubectl describe pv &lt;pv_name&gt;</code></pre> 对于文件存储器，可以在 CLI 输出的 <strong>Labels</strong> 部分中找到卷标识、大小和 IOPS。对于块存储器，大小和 IOPS 会显示在 CLI 输出的 <strong>Labels</strong> 部分中。要查找卷标识，请查看 CLI 输出的 <code>ibm.io/network-storage-id</code> 注释。</li><li>使用您在先前步骤中检索到的参数为现有卷创建快照大小。<pre class="pre"><code>slcli file snapshot-order --capacity &lt;size&gt; --tier &lt;iops&gt; &lt;volume_id&gt;</code></pre><pre class="pre"><code>slcli block snapshot-order --capacity &lt;size&gt; --tier &lt;iops&gt; &lt;volume_id&gt;</code></pre></li><li>等待快照大小创建。<pre class="pre"><code>slcli file volume-detail &lt;volume_id&gt;</code></pre><pre class="pre"><code>slcli block volume-detail &lt;volume_id&gt;</code></pre>CLI 输出中的 <strong>Snapshot Capacity (GB)</strong> 从 0 更改为您所订购的大小时，说明已成功供应快照大小。</li><li>为卷创建快照，并记下创建的快照的标识。<pre class="pre"><code>slcli file snapshot-create &lt;volume_id&gt;</code></pre><pre class="pre"><code>slcli block snapshot-create &lt;volume_id&gt;</code></pre></li><li>验证快照是否已成功创建。<pre class="pre"><code>slcli file volume-detail &lt;snapshot_id&gt;</code></pre><pre class="pre"><code>slcli block volume-detail &lt;snapshot_id&gt;</code></pre></li></ol></br><strong>要将数据从快照复原到现有卷，请运行以下命令：</strong><pre class="pre"><code>slcli file snapshot-restore -s &lt;snapshot_id&gt; &lt;volume_id&gt;</code></pre><pre class="pre"><code>slcli block snapshot-restore -s &lt;snapshot_id&gt; &lt;volume_id&gt;</code></pre></br>有关更多信息，请参阅：<ul><li>[NFS 定期快照](/docs/infrastructure/FileStorage?topic=FileStorage-snapshots)</li><li>[块定期快照](/docs/infrastructure/BlockStorage?topic=BlockStorage-snapshots#snapshots)</li></ul></p></dd>
  <dt>将快照复制到其他专区</dt>
 <dd><p>为了保护数据不受专区故障的影响，可以[复制快照](/docs/infrastructure/FileStorage?topic=FileStorage-replication#replication)到其他专区中设置的 NFS 文件共享或块存储器实例。数据只能从主存储器复制到备份存储器。不能将复制的 NFS 文件共享或块存储器实例安装到集群。主存储器发生故障时，可以手动将复制的备份存储器设置为主存储器。然后，可以将其安装到集群。复原主存储器后，可以从备份存储器复原数据。</p>
 <p>有关更多信息，请参阅：<ul><li>[复制 NFS 的快照](/docs/infrastructure/FileStorage?topic=FileStorage-replication)</li><li>[复制块的快照](/docs/infrastructure/BlockStorage?topic=BlockStorage-replication#replication)</li></ul></p></dd>
 <dt>复制存储器</dt>
 <dd><p>可以在原始存储器实例所在的专区中，复制 NFS 文件共享或块存储器实例。复制项采用原始存储器实例在创建该复制项的时间点的数据。与副本不同，复制项用作独立于原始项的存储器实例。要进行复制，请首先为该卷设置快照。</p>
 <p>有关更多信息，请参阅：<ul><li>[NFS 复制快照](/docs/infrastructure/FileStorage?topic=FileStorage-duplicatevolume#duplicatevolume)</li><li>[块复制快照](/docs/infrastructure/BlockStorage?topic=BlockStorage-duplicatevolume#duplicatevolume)</li></ul></p></dd>
  <dt>将数据备份到 Object Storage</dt>
  <dd><p>可以使用 [**ibm-backup-restore 映像**](/docs/services/RegistryImages/ibm-backup-restore?topic=RegistryImages-ibmbackup_restore_starter#ibmbackup_restore_starter)来启动集群中的备份和复原 pod。此 pod 包含一个脚本，用于对集群中的任何持久卷声明 (PVC) 运行一次性或定期备份。数据存储在您在某个专区中设置的 {{site.data.keyword.objectstoragefull}} 实例中。</p>
  <p>要使数据具有更高可用性，并保护应用程序不受专区故障的影响，请设置第二个 {{site.data.keyword.objectstoragefull}} 实例，并在各个专区之间复制数据。如果需要从 {{site.data.keyword.objectstoragefull}} 实例复原数据，请使用随映像一起提供的复原脚本。</p></dd>
<dt>将数据复制到 pod 和容器，以及从 pod 和容器中复制数据</dt>
<dd><p>可以使用 `kubectl cp` [命令 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/reference/kubectl/overview/#cp) 将文件和目录复制到集群中的 pod 或特定容器，或者从集群中的 pod 或特定容器中复制文件和目录。</p>
<p>开始之前：[登录到您的帐户。将相应的区域和（如果适用）资源组设定为目标。设置集群的上下文](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)。如果未使用 <code>-c</code> 来指定容器，那么此命令将使用 pod 中的第一个可用容器。</p>
<p>可以通过以下各种方法来使用此命令：</p>
<ul>
<li>将本地机器中的数据复制到集群中的 pod：<pre class="pre"><code>kubectl cp <var>&lt;local_filepath&gt;/&lt;filename&gt;</var> <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;</var></code></pre></li>
<li>将集群的 pod 中的数据复制到本地机器：<pre class="pre"><code>kubectl cp <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;/&lt;filename&gt;</var> <var>&lt;local_filepath&gt;/&lt;filename&gt;</var></code></pre></li>
<li>将集群的 pod 中的数据复制到其他集群的其他 pod 中的特定容器：<pre class="pre"><code>kubectl cp <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;</var> <var>&lt;namespace&gt;/&lt;other_pod&gt;:&lt;pod_filepath&gt;</var> -c <var>&lt;container></var></code></pre></li>
</ul></dd>
  </dl>
