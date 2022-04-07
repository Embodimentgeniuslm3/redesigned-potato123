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



# 지속적 볼륨의 데이터 백업 및 복원
{: #backup_restore}

파일 공유 및 블록 스토리지는 클러스터와 동일한 구역으로 프로비저닝됩니다. 이 스토리지는 서버가 작동을 중지하는 경우 가용성을 제공하기 위해 {{site.data.keyword.IBM_notm}}에 의해 클러스터된 서버에서 호스팅됩니다. 그러나 파일 공유 및 블록 스토리지는 자동으로 백업되지 않으며, 전체 구역에서 장애가 발생하면 액세스가 불가능해질 수 있습니다. 데이터가 유실되거나 손상되지 않도록 하기 위해, 필요한 경우 데이터를 복원하는 데 사용할 수 있는 주기적 백업을 설정할 수 있습니다.
{: shortdesc}

NFS 파일 공유 및 블록 스토리지에 대한 다음 백업 및 복원 옵션을 검토하십시오.

<dl>
  <dt>주기적 스냅샷 설정</dt>
  <dd><p>특정 시점의 인스턴스 상태를 캡처하는 읽기 전용 이미지인 NFS 파일 공유 또는 블록 스토리지의 주기적 스냅샷을 설정할 수 있습니다. 스냅샷을 저장하려면 NFS 파일 공유 또는 블록 스토리지에 대한 스냅샷 영역을 요청해야 합니다. 스냅샷은 동일한 구역 내의 기본 스토리지 인스턴스에 저장됩니다. 사용자가 실수로 볼륨에서 중요한 데이터를 제거한 경우 스냅샷에서 데이터를 복원할 수 있습니다. </br></br> <strong>볼륨에 대한 스냅샷을 작성하려면 다음을 수행하십시오. </strong><ol><li>클러스터에 있는 기존 PV를 나열하십시오. <pre class="pre"><code>kubectl get pv</code></pre></li><li>스냅샷 영역을 작성할 PV에 대한 세부사항을 가져오고 볼륨 ID, 크기 및 IOPS를 기록해 두십시오. <pre class="pre"><code>kubectl describe pv &lt;pv_name&gt;</code></pre> 파일 스토리지의 경우 CLI 출력의 <strong>Labels</strong> 섹션에서 볼륨 ID, 크기 및 IOPS를 찾을 수 있습니다. 블록 스토리지의 경우 크기 및 IOPS가 CLI 출력의 <strong>Labels</strong> 섹션에 표시됩니다. 볼륨 ID을 찾으려면 CLI 출력의 <code>ibm.io/network-storage-id</code> 어노테이션을 검토하십시오. </li><li>이전 단계에서 검색한 매개변수를 사용하여 기존 볼륨의 스냅샷 크기를 작성하십시오. <pre class="pre"><code>slcli file snapshot-order --capacity &lt;size&gt; --tier &lt;iops&gt; &lt;volume_id&gt;</code></pre><pre class="pre"><code>slcli block snapshot-order --capacity &lt;size&gt; --tier &lt;iops&gt; &lt;volume_id&gt;</code></pre></li><li>스냅샷 크기가 작성될 때까지 기다리십시오. <pre class="pre"><code>slcli file volume-detail &lt;volume_id&gt;</code></pre><pre class="pre"><code>slcli block volume-detail &lt;volume_id&gt;</code></pre>CLI 출력의 <strong>Snapshot Capacity (GB)</strong>가 0에서 주문한 크기로 변경된 경우 스냅샷 크기가 성공적으로 프로비저닝된 것입니다. </li><li>볼륨에 대한 스냅샷을 작성하고 작성된 스냅샷의 ID를 기록해 두십시오. <pre class="pre"><code>slcli file snapshot-create &lt;volume_id&gt;</code></pre><pre class="pre"><code>slcli block snapshot-create &lt;volume_id&gt;</code></pre></li><li>스냅샷이 작성되었는지 확인하십시오. <pre class="pre"><code>slcli file volume-detail &lt;snapshot_id&gt;</code></pre><pre class="pre"><code>slcli block volume-detail &lt;snapshot_id&gt;</code></pre></li></ol></br><strong>스냅샷의 데이터를 기본 볼륨에 복원하려면 다음을 수행하십시오. </strong><pre class="pre"><code>slcli file snapshot-restore -s &lt;snapshot_id&gt; &lt;volume_id&gt;</code></pre><pre class="pre"><code>slcli block snapshot-restore -s &lt;snapshot_id&gt; &lt;volume_id&gt;</code></pre></br>자세한 정보는 다음을 참조하십시오.<ul><li>[NFS 주기적 스냅샷](/docs/infrastructure/FileStorage?topic=FileStorage-snapshots)</li><li>[블록 주기적 스냅샷](/docs/infrastructure/BlockStorage?topic=BlockStorage-snapshots#snapshots)</li></ul></p></dd>
  <dt>다른 구역으로 스냅샷 복제</dt>
 <dd><p>구역 장애로부터 데이터를 보호하기 위해 다른 구역에서 설정된 NFS 파일 공유 또는 블록 스토리지 인스턴스로 [스냅샷을 복제](/docs/infrastructure/FileStorage?topic=FileStorage-replication#replication)할 수 있습니다. 데이터는 기본 스토리지에서 백업 스토리지로만 복제할 수 있습니다. 복제된 NFS 파일 공유 또는 블록 스토리지 인스턴스를 클러스터에 마운트할 수는 없습니다. 기본 스토리지에서 장애가 발생하는 경우에는 복제된 백업 스토리지가 기본 스토리지가 되도록 수동으로 설정할 수 있습니다. 그런 다음 클러스터에 이를 추가할 수 있습니다. 기본 스토리지가 복원되고 나면 백업 스토리지로부터 데이터를 복원할 수 있습니다.</p>
 <p>자세한 정보는 다음을 참조하십시오.<ul><li>[NFS에 대한 스냅샷 복제](/docs/infrastructure/FileStorage?topic=FileStorage-replication)</li><li>[블록에 대한 스냅샷 복제](/docs/infrastructure/BlockStorage?topic=BlockStorage-replication#replication)</li></ul></p></dd>
 <dt>스토리지 복제(duplicate)</dt>
 <dd><p>원본 스토리지 인스턴스와 동일한 구역에서 NFS 파일 공유나 블록 스토리지 인스턴스를 복제할 수 있습니다. 복제본(duplicate)에는 복제본(duplicate)을 작성한 시점의 원본 스토리지 인스턴스와 동일한 데이터가 저장되어 있습니다. 복제본(replica)과 다르게 복제본(duplicate)은 원본과 별개인 스토리지 인스턴스로 사용하십시오. 복제(duplicate)하려면 먼저 볼륨의 스냅샷을 설정하십시오.</p>
 <p>자세한 정보는 다음을 참조하십시오.<ul><li>[NFS 복제(duplicate) 스냅샷](/docs/infrastructure/FileStorage?topic=FileStorage-duplicatevolume#duplicatevolume)</li><li>[블록 복제(duplicate) 스냅샷](/docs/infrastructure/BlockStorage?topic=BlockStorage-duplicatevolume#duplicatevolume)</li></ul></p></dd>
  <dt>데이터를 Object Storage에 백업</dt>
  <dd><p>[**ibm-backup-restore 이미지**](/docs/services/RegistryImages/ibm-backup-restore?topic=RegistryImages-ibmbackup_restore_starter#ibmbackup_restore_starter)를 사용하여 클러스터에서 백업을 회전하고 팟(Pod)을 복원할 수 있습니다. 이 팟(Pod)에는 클러스터의 지속적 볼륨 클레임(PVC)에 대한 일회성 또는 주기적 백업을 실행하는 스크립트가 포함되어 있습니다. 데이터는 구역에 설정된 {{site.data.keyword.objectstoragefull}} 인스턴스에 저장됩니다.</p>
  <p>데이터의 고가용성을 개선하고 구역 장애로부터 앱을 보호하려면 두 번째 {{site.data.keyword.objectstoragefull}} 인스턴스를 설정하고 구역 간에 데이터를 복제하십시오. {{site.data.keyword.objectstoragefull}} 인스턴스에서 데이터를 복원해야 하는 경우 이미지와 함께 제공된 복원 스크립트를 사용하십시오.</p></dd>
<dt>팟(Pod) 및 컨테이너에서 데이터 복사</dt>
<dd><p>`kubectl cp` [명령 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/reference/kubectl/overview/#cp)을 사용하여 클러스터의 팟(Pod) 또는 특정 컨테이너에서 파일 및 디렉토리를 복사할 수 있습니다.</p>
<p>시작하기 전에: [계정에 로그인하십시오. 적절한 지역을 대상으로 지정하고, 해당되는 경우에는 리소스 그룹도 지정하십시오. 클러스터의 컨텍스트를 설정하십시오](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure). <code>-c</code>를 사용하여 컨테이너를 지정하지 않는 경우 이 명령은 팟(Pod)의 사용 가능한 첫 번째 컨테이너를 사용합니다.</p>
<p>이 명령은 다양한 방식으로 사용할 수 있습니다.</p>
<ul>
<li>로컬 머신에서 클러스터의 팟(Pod)으로 데이터 복사: <pre class="pre"><code>kubectl cp <var>&lt;local_filepath&gt;/&lt;filename&gt;</var> <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;</var></code></pre></li>
<li>클러스터의 팟(Pod)에서 로컬 머신으로 데이터 복사: <pre class="pre"><code>kubectl cp <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;/&lt;filename&gt;</var> <var>&lt;local_filepath&gt;/&lt;filename&gt;</var></code></pre></li>
<li>클러스터의 팟(Pod)에서 다른 팟(Pod)의 특정 컨테이너로 데이터 복사: <pre class="pre"><code>kubectl cp <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;</var> <var>&lt;namespace&gt;/&lt;other_pod&gt;:&lt;pod_filepath&gt;</var> -c <var>&lt;container></var></code></pre></li>
</ul></dd>
  </dl>
