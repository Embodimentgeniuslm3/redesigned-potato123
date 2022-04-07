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



# 叢集除錯
{: #cs_troubleshoot}

在您使用 {{site.data.keyword.containerlong}} 時，請考慮使用這些技術來進行一般疑難排解以及叢集的除錯。您也可以檢查 [{{site.data.keyword.Bluemix_notm}} 系統的狀態 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://developer.ibm.com/bluemix/support/#status)。
{: shortdesc}

您可以採取這些一般步驟來確保叢集保持最新：
- 每月檢查可用的安全及作業系統修補程式，以[更新工作者節點](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update)。
- [將叢集更新](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_update)為 {{site.data.keyword.containerlong_notm}} 的最新預設 [Kubernetes 版本](/docs/containers?topic=containers-cs_versions)。<p class="important">確保 [`kubectl` CLI](/docs/containers?topic=containers-cs_cli_install#kubectl) 用戶端的 Kubernetes 版本與叢集伺服器的相符合。[Kubernetes 不支援 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/setup/version-skew-policy/) 比伺服器版本高/低 2 個或更多版本 (n +/- 2) 的 `kubectl` 用戶端版本。</p>

## 使用 {{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool 執行測試
{: #debug_utility}

疑難排解時，您可以使用 {{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool 來執行測試，並從叢集收集相關資訊。若要使用除錯工具，請安裝 [`ibmcloud-iks-debug` Helm 圖表 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/kubernetes/helm/iks-charts/ibmcloud-iks-debug)：
{: shortdesc}


1. [在叢集裡設定 Helm，建立 Tiller 的服務帳戶，並將 `ibm` 儲存庫新增至 Helm 實例](/docs/containers?topic=containers-helm)。

2. 將 Helm 圖表安裝至您的叢集。
        
  ```
  helm install ibm/ibmcloud-iks-debug --name debug-tool
  ```
  {: pre}


3. 啟動 Proxy 伺服器以顯示除錯工具介面。
  ```
  kubectl proxy --port 8080
  ```
  {: pre}

4. 在 Web 瀏覽器中，開啟除錯工具介面 URL： http://localhost:8080/api/v1/namespaces/default/services/debug-tool-ibmcloud-iks-debug:8822/proxy/page

5. 選取要執行個別測試或一群測試。有些測試會檢查潛在警告、錯誤或問題，有些測試僅收集您在疑難排解時可以參照的資訊。如需每個測試的功能相關資訊，請按一下測試名稱旁邊的資訊圖示。

6. 按一下**執行**。

7. 檢查每個測試的結果。
  * 如果有任何測試失敗，請按一下左側直欄中測試名稱旁的資訊圖示，以取得如何解決問題的相關資訊。
  * 您也可以使用測試結果來收集資訊，例如，完整的 YAML，其會在下列各節中協助您對叢集進行除錯。

## 叢集除錯
{: #debug_clusters}

請檢閱選項以進行叢集除錯，並找出失敗的主要原因。

1.  列出叢集，並尋找叢集的 `State`。

  ```
  ibmcloud ks clusters
  ```
  {: pre}

2.  檢閱叢集的 `State`。如果叢集處於 **Critical**、**Delete failed** 或 **Warning** 狀況，或停留在 **Pending** 狀況很長一段時間，請開始[針對工作者節點進行除錯](#debug_worker_nodes)。

您可以執行 `ibmcloud ks clusters` 指令並找出**狀況**欄位，以檢視現行叢集狀況。
{: shortdesc}

<table summary="每個表格列都應該從左到右閱讀，第一欄為叢集狀況，第二欄則為說明。">
<caption>叢集狀況</caption>
   <thead>
   <th>叢集狀況</th>
   <th>說明</th>
   </thead>
   <tbody>
<tr>
   <td>`Aborted`</td>
   <td>在部署 Kubernetes 主節點之前，使用者要求刪除叢集。叢集刪除完成之後，即會從儀表板移除該叢集。如果叢集停留在此狀況很長一段時間，請開立 [{{site.data.keyword.Bluemix_notm}} 支援案例](/docs/containers?topic=containers-cs_troubleshoot#ts_getting_help)。</td>
   </tr>
 <tr>
     <td>`Critical`</td>
     <td>無法聯繫 Kubernetes 主節點，或叢集裡的所有工作者節點都已關閉。</td>
    </tr>
   <tr>
     <td>`Delete failed`</td>
     <td>無法刪除 Kubernetes 主節點或至少一個工作者節點。</td>
   </tr>
   <tr>
     <td>`Deleted`</td>
     <td>叢集已刪除，但尚未從儀表板移除。如果叢集停留在此狀況很長一段時間，請開立 [{{site.data.keyword.Bluemix_notm}} 支援案例](/docs/containers?topic=containers-cs_troubleshoot#ts_getting_help)。</td>
   </tr>
   <tr>
   <td>`Deleting`</td>
   <td>正在刪除叢集，且正在拆除叢集基礎架構。您無法存取該叢集。</td>
   </tr>
   <tr>
     <td>`Deploy failed`</td>
     <td>無法完成 Kubernetes 主節點的部署。您無法解決此狀況。請開立 [{{site.data.keyword.Bluemix_notm}} 支援案例](/docs/containers?topic=containers-cs_troubleshoot#ts_getting_help)，以與 IBM Cloud 支援中心聯絡。</td>
   </tr>
     <tr>
       <td>`Deploying`</td>
       <td>Kubernetes 主節點尚未完整部署。您無法存取叢集。請等到叢集完成部署後，再檢閱叢集的性能。</td>
      </tr>
      <tr>
       <td>`Normal`</td>
       <td>叢集裡的所有工作者節點都已開始執行。您可以存取叢集，並將應用程式部署至叢集。此狀態被視為健全，您不需要採取動作。<p class="note">雖然工作者節點可能是正常的，但也可能需要注意其他的基礎架構資源（例如[網路](/docs/containers?topic=containers-cs_troubleshoot_network)和[儲存空間](/docs/containers?topic=containers-cs_troubleshoot_storage)）。如果您才剛建立叢集，則其他服務所使用的一些叢集部分（例如 Ingress 密碼或登錄映像檔取回密碼）可能還在處理中。</p></td>
    </tr>
      <tr>
       <td>`Pending`</td>
       <td>已部署 Kubernetes 主節點。正在佈建工作者節點，因此還無法在叢集裡使用。您可以存取叢集，但無法將應用程式部署至叢集。</td>
     </tr>
   <tr>
     <td>`Requested`</td>
     <td>已傳送要建立叢集和訂購 Kubernetes 主節點和工作者節點之基礎架構的要求。當開始部署叢集時，叢集狀況會變更為 <code>Deploying</code>。如果叢集停留在 <code>Requested</code> 狀況很長一段時間，請開立 [{{site.data.keyword.Bluemix_notm}} 支援案例](/docs/containers?topic=containers-cs_troubleshoot#ts_getting_help)。</td>
   </tr>
   <tr>
     <td>`Updating`</td>
     <td>在 Kubernetes 主節點中執行的 Kubernetes API 伺服器正更新為新的 Kubernetes API 版本。在更新期間，您無法存取或變更叢集。使用者所部署的工作者節點、應用程式及資源不會遭到修改，並會繼續執行。請等到更新完成後，再檢閱叢集的性能。</td>
   </tr>
   <tr>
    <td>`Unsupported`</td>
    <td>叢集執行的 [Kubernetes 版本](/docs/containers?topic=containers-cs_versions#cs_versions)不再受支援。您的叢集性能不再受到積極監視或報告。此外，您無法新增或重新載入工作者節點。若要繼續接收重要的安全更新及支援，您必須更新您的叢集。請檢閱[版本更新準備動作](/docs/containers?topic=containers-cs_versions#prep-up)，然後[更新叢集](/docs/containers?topic=containers-update#update)至支援的 Kubernetes 版本。<br><br><p class="note">比最舊的支援版本還早三個以上版本的叢集將無法更新。若要避免此狀況，您可以將叢集更新至現行版本之前少於 3 版本的支援版本，例如 1.12 更新至 1.14。此外，如果您的叢集執行 1.5、1.7 或 1.8 版，則版本已經太落後而無法更新。相反地，您必須[建立叢集](/docs/containers?topic=containers-clusters#clusters)，然後[部署應用程式](/docs/containers?topic=containers-app#app)至該叢集。</p></td>
   </tr>
    <tr>
       <td>`Warning`</td>
       <td>叢集裡至少有一個工作者節點無法使用，但有其他工作者節點可用，並且可以接管工作負載。</td>
    </tr>
   </tbody>
 </table>


[Kubernetes 主節點](/docs/containers?topic=containers-ibm-cloud-kubernetes-service-technology#architecture)是保持叢集運作的主要元件。主節點將叢集資源及其配置儲存在 etcd 資料庫中，作為叢集的單點真實資料 (SPOT)。Kubernetes API 伺服器是從工作者節點到主節點之所有叢集管理要求的主要進入點，或您要與叢集資源互動時。<br><br>如果發生主節點失敗，則工作負載會繼續在工作者節點上執行，但無法使用 `kubectl` 指令來使用叢集資源，或檢視叢集性能，直到備份主節點中的 Kubernetes API 伺服器為止。如果 Pod 在主節點中斷期間關閉，則除非工作者節點再次到達 Kubernetes API 伺服器，否則無法重新排定 Pod。<br><br>在主節點中斷期間，您仍然可以針對 {{site.data.keyword.containerlong_notm}} API 執行 `ibmcloud ks` 指令，以使用您的基礎架構資源（例如工作者節點或 VLAN）。如果您透過在叢集裡新增或移除工作者節點來變更現行叢集配置，則除非備份主節點，否則您的變更不會發生。

在主節點中斷期間，請不要將工作者節點重新啟動或重新開機。此動作會從您的工作者節點移除 Pod。因為 Kubernetes API 伺服器無法使用，所以無法將 Pod 重新排程至叢集裡的其他工作者節點。
{: important}


<br />


## 針對工作者節點進行除錯
{: #debug_worker_nodes}

檢閱針對工作者節點進行除錯的選項，並尋找失敗的主要原因。

<ol><li>如果叢集處於 **Critical**、**Delete failed** 或 **Warning** 狀況，或停留在 **Pending** 狀況很長一段時間，請檢閱工作者節點的狀況。<p class="pre">ibmcloud ks workers --cluster <cluster_name_or_id></p></li>
  <li>請檢閱 CLI 輸出中每個工作者節點的 **State** 及 **Status** 欄位。<p>您可以執行 `ibmcloud ks workers --cluster <cluster_name_or_ID` 指令並找到**狀況**和**狀態**欄位，以檢視現行的工作者節點狀況。
{: shortdesc}

<table summary="每個表格列都應該從左到右閱讀，第一欄為工作者節點狀況，第二欄則為說明。">
    <caption>工作者節點狀況</caption>
  <thead>
  <th>工作者節點狀況</th>
  <th>說明</th>
  </thead>
  <tbody>
<tr>
    <td>`Critical`</td>
    <td>工作者節點可能因為各種原因而進入 Critical 狀況：<ul><li>您針對工作者節點起始重新啟動，但未隔離及排除您的工作者節點。重新啟動工作者節點會導致 <code>containerd</code>、<code>kubelet</code>、<code>kube-proxy</code> 及 <code>calico</code> 中的資料毀損。</li>
    <li>部署至工作者節點的 Pod 未使用[記憶體 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/tasks/configure-pod-container/assign-memory-resource/) 及 [CPU ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/tasks/configure-pod-container/assign-cpu-resource/) 的資源限制。沒有資源限制，Pod 可能會取用所有可用的資源，而未留下任何資源給在此工作者節點上執行的其他 Pod。工作負載的過度使用會導致工作者節點失敗。</li>
    <li>在一段時間內執行數百或數千個容器之後，<code>containerd</code>、<code>kubelet</code> 或 <code>calico</code> 會進入無法復原的狀況。</li>
    <li>您為工作者節點設定 Virtual Router Appliance，而工作者節點關閉並中斷工作者節點與 Kubernetes 主節點之間的通訊。</li><li> {{site.data.keyword.containerlong_notm}} 或 IBM Cloud 基礎架構 (SoftLayer) 中的現行網路問題，導致工作者節點與 Kubernetes 主節點之間的通訊失敗。</li>
    <li>工作者節點容量不足。請檢查工作者節點的<strong>狀態</strong> (Status)，以查看它是否顯示<strong>磁碟不足</strong> (Out of disk) 或<strong>記憶體不足</strong> (Out of memory)。如果工作者節點容量不足，請考慮減少工作者節點上的工作負載，或將工作者節點新增至叢集，以協助對工作負載進行負載平衡。</li>
    <li>已從 [{{site.data.keyword.Bluemix_notm}} 主控台資源清單 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/resources) 關閉裝置電源。開啟資源清單，並在**裝置**清單中尋找您的工作者節點 ID。在動作功能表中，按一下**開啟電源**。</li></ul>
在許多情況下，[重新載入](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)工作者節點可以解決此問題。當您重新載入工作者節點時，最新的[修補程式版本](/docs/containers?topic=containers-cs_versions#version_types)會套用至您的工作者節點。主要版本和次要版本不會變更。在您重新載入工作者節點之前，請確定隔離及排除工作者節點，以確保現有 Pod 會循序終止並重新排定其餘的工作者節點。</br></br>如果重新載入工作者節點未解決此問題，請移至下一步，以繼續進行工作者節點的疑難排解。</br></br><strong>提示：</strong>您可以[為工作者節點配置性能檢查，並啟用自動回復](/docs/containers?topic=containers-health#autorecovery)。如果「自動回復」根據配置的檢查，偵測到性能不佳的工作者節點，則「自動回復」會觸發更正動作，如在工作者節點上重新載入 OS。如需自動回復運作方式的相關資訊，請參閱[自動回復部落格![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/blogs/bluemix/2017/12/autorecovery-utilizes-consistent-hashing-high-availability/)。</td>
   </tr>
   <tr>
   <td>`已部署`</td>
   <td>更新項目已順利部署至您的工作者節點。部署更新項目之後，{{site.data.keyword.containerlong_notm}} 會啟動工作者節點上的性能檢查。性能檢查成功之後，工作者節點會進入 <code>Normal</code> 狀態。處於 <code>Deployed</code> 狀態的工作者節點通常已準備好接收工作負載，而檢查方式是執行 <code>kubectl get nodes</code> 並確認狀態顯示 <code>Normal</code>。</td>
   </tr>
    <tr>
      <td>`Deploying`</td>
      <td>當您更新工作者節點的 Kubernetes 版本時，會重新部署工作者節點以安裝更新。如果您重新載入或重新開機工作者節點，則會重新部署工作者節點，以自動安裝最新的修補程式版本。如果工作者節點停留在此狀況很長一段時間，請繼續下一步，以查看部署期間是否發生問題。</td>
   </tr>
      <tr>
      <td>`Normal`</td>
      <td>工作者節點已完整佈建，並已準備好用於叢集。此狀況被視為健全，不需要使用者採取動作。**附註**：雖然工作者節點可能是正常的，但可能仍需注意其他的基礎架構資源（例如[網路](/docs/containers?topic=containers-cs_troubleshoot_network)和[儲存空間](/docs/containers?topic=containers-cs_troubleshoot_storage)）。</td>
   </tr>
 <tr>
      <td>`Provisioning`</td>
      <td>正在佈建工作者節點，因此還無法在叢集裡使用。您可以在 CLI 輸出的 <strong>Status</strong> 直欄中監視佈建處理程序。如果工作者節點停留在此狀況很長一段時間，請繼續下一步，以查看佈建期間是否發生問題。</td>
    </tr>
    <tr>
      <td>`Provision_failed`</td>
      <td>無法佈建工作者節點。請繼續進行下一步，以尋找失敗的詳細資料。</td>
    </tr>
 <tr>
      <td>`Reloading`</td>
      <td>正在重新載入工作者節點，因此無法在叢集裡使用。您可以在 CLI 輸出的 <strong>Status</strong> 直欄中監視重新載入處理程序。如果工作者節點停留在此狀況很長一段時間，請繼續下一步，以查看重新載入期間是否發生問題。</td>
     </tr>
     <tr>
      <td>`Reloading_failed`</td>
      <td>無法重新載入工作者節點。請繼續進行下一步，以尋找失敗的詳細資料。</td>
    </tr>
    <tr>
      <td>`Reload_pending `</td>
      <td>傳送重新載入或更新工作者節點的 Kubernetes 版本的要求。重新載入工作者節點時，State 會變更為 <code>Reloading</code>。</td>
    </tr>
    <tr>
     <td>`Unknown`</td>
     <td>因下列其中一個原因而無法聯繫 Kubernetes 主節點：<ul><li>您已要求更新 Kubernetes 主節點。在更新期間，無法擷取工作者節點的狀況。如果工作者節點即使在 Kubernetes 主節點順利更新之後仍長期處於此狀態，請嘗試[重新載入](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)工作者節點。</li><li>您可能有另一個防火牆保護工作者節點，或最近變更過防火牆設定。{{site.data.keyword.containerlong_notm}} 需要開啟特定 IP 位址及埠，以容許從工作者節點到 Kubernetes 主節點的通訊，反之亦然。如需相關資訊，請參閱[防火牆阻止工作者節點連接](/docs/containers?topic=containers-cs_troubleshoot_clusters#cs_firewall)。</li><li>Kubernetes 主節點已關閉。請開立 [{{site.data.keyword.Bluemix_notm}} 支援案例](/docs/containers?topic=containers-cs_troubleshoot#ts_getting_help)，以與 {{site.data.keyword.Bluemix_notm}} 支援中心聯絡。</li></ul></td>
</tr>
   <tr>
      <td>`Warning`</td>
      <td>工作者節點將達到記憶體或磁碟空間的限制。您可以減少工作者節點上的工作負載，或將工作者節點新增至叢集，以協助對工作負載進行負載平衡。</td>
</tr>
  </tbody>
</table>
</p></li>
<li>列出工作者節點的詳細資料。如果詳細資料包含錯誤訊息，請檢閱[工作者節點的一般錯誤訊息](#common_worker_nodes_issues)清單，以瞭解如何解決問題。<p class="pre">ibmcloud ks worker-get --cluster <cluster_name_or_id> --worker <worker_node_id></li>
  </ol>

<br />


## 工作者節點的一般問題
{: #common_worker_nodes_issues}

檢閱一般錯誤訊息，並瞭解其解決方式。

  <table>
  <caption>一般錯誤訊息</caption>
    <thead>
    <th>錯誤訊息</th>
    <th>說明及解決方式
    </thead>
    <tbody>
      <tr>
        <td>{{site.data.keyword.Bluemix_notm}} 基礎架構異常狀況：您的帳戶目前被禁止訂購「運算實例」。</td>
        <td>您的 IBM Cloud 基礎架構 (SoftLayer) 帳戶可能無法訂購運算資源。請開立 [{{site.data.keyword.Bluemix_notm}} 支援案例](#ts_getting_help)，以與 {{site.data.keyword.Bluemix_notm}} 支援中心聯絡。</td>
      </tr>
      <tr>
      <td>{{site.data.keyword.Bluemix_notm}} 基礎架構異常狀況：無法下訂單。<br><br>
      {{site.data.keyword.Bluemix_notm}} 基礎架構異常狀況：無法下訂單。路由器 'router_name' 的資源不足，無法滿足下列來賓的要求：'worker_id'。</td>
      <td>您所選取區域的基礎架構容量可能不足，無法佈建您的工作者節點。或者，您可能已超出 IBM Cloud 基礎架構 (SoftLayer) 帳戶的限制。若要解決，請嘗試下列其中一個選項：
      <ul><li>區域中的基礎架構資源可用性可能經常變動。請等待數分鐘，然後再試一次。</li>
      <li>若為單一區域叢集，請在不同的區域中建立叢集。若為多區域叢集，請將區域新增至叢集。</li>
      <li>為您 IBM Cloud 基礎架構 (SoftLayer) 帳戶中的工作者節點，指定不同的公用及專用 VLAN 配對。對於工作者節點儲存區中的工作者節點，您可以使用 <code>ibmcloud ks zone-network-set</code> [指令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_zone_network_set)。</li>
      <li>與您的 IBM Cloud 基礎架構 (SoftLayer) 帳戶管理員聯絡，以驗證您未超出帳戶限制（例如廣域配額）。</li>
      <li>開立 [IBM Cloud 基礎架構 (SoftLayer) 支援案例](#ts_getting_help)</li></ul></td>
      </tr>
      <tr>
        <td>{{site.data.keyword.Bluemix_notm}} 基礎架構異常狀況：無法取得 ID 為 <code>&lt;vlan id&gt;</code> 的網路 VLAN。</td>
        <td>無法佈建工作者節點，因為因下列其中一個原因而找不到選取的 VLAN ID：<ul><li>您可能已指定 VLAN 號碼，而非 VLAN ID。VLAN 號碼的長度是 3 或 4 位數，而 VLAN ID 的長度是 7 位數。執行 <code>ibmcloud ks vlans --zone &lt;zone&gt;</code>，以擷取 VLAN ID。<li>VLAN ID 可能未與您使用的 IBM Cloud 基礎架構 (SoftLayer) 帳戶相關聯。執行 <code>ibmcloud ks vlans --zone &lt;zone&gt;</code>，以列出帳戶的可用 VLAN ID。若要變更 IBM Cloud 基礎架構 (SoftLayer) 帳戶，請參閱 [`ibmcloud ks credential-set`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_set)。</ul></td>
      </tr>
      <tr>
        <td>SoftLayer_Exception_Order_InvalidLocation：針對此訂單提供的位置無效。(HTTP 500)</td>
        <td>您的 IBM Cloud 基礎架構 (SoftLayer) 未設定成訂購所選取資料中心內的運算資源。請與 [{{site.data.keyword.Bluemix_notm}} 支援中心](#ts_getting_help)聯絡，驗證已正確設定帳戶。</td>
       </tr>
       <tr>
        <td>{{site.data.keyword.Bluemix_notm}} 基礎架構異常狀況：使用者沒有新增伺服器的必要「{{site.data.keyword.Bluemix_notm}} 基礎架構」許可權
        </br></br>
        {{site.data.keyword.Bluemix_notm}} 基礎架構異常狀況：必須要有許可權才能訂購「項目」。
        </br></br>
                無法驗證 {{site.data.keyword.Bluemix_notm}} 基礎架構認證。</td>
        <td>您可能沒有在 IBM Cloud 基礎架構 (SoftLayer) 組合中執行動作的必要許可權，或您使用錯誤的基礎架構認證。請參閱[設定 API 金鑰以啟用存取基礎架構組合](/docs/containers?topic=containers-users#api_key)。</td>
      </tr>
      <tr>
       <td>工作者節點無法與 {{site.data.keyword.containerlong_notm}} 伺服器交談。請驗證您的防火牆設定容許來自此工作者節點的資料流量。
       <td><ul><li>如果您有防火牆，請[配置防火牆設定，以容許將送出的資料流量傳送至適當的埠和 IP 位址](/docs/containers?topic=containers-firewall#firewall_outbound)。</li>
       <li>執行 `ibmcloud ks workers --cluster &lt;mycluster&gt;`，來檢查您的叢集是否沒有公用 IP。如果未列出任何公用 IP，則您的叢集只有專用 VLAN。<ul><li>如果您要叢集只有專用 VLAN，則請設定 [VLAN 連線](/docs/containers?topic=containers-plan_clusters#private_clusters)及[防火牆](/docs/containers?topic=containers-firewall#firewall_outbound)。</li>
       <li>如果已啟用帳戶以使用 [VRF](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) 和[服務端點](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started)之前，建立的叢集僅具有專用服務端點，則工作者節點無法連接至主節點。如果是，請嘗試[設定公用服務端點](/docs/containers?topic=containers-cs_network_cluster#set-up-public-se)，以便可以一直使用叢集，直到支援案例得到處理，可以更新帳戶。
如果在更新帳戶後仍只想要專用服務端點叢集，則可以停用公用服務端點。</li>
       <li>如果您想要叢集具有公用 IP，請同時使用公用及專用 VLAN 來[新增工作者節點](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_add)。</li></ul></li></ul></td>
     </tr>
      <tr>
  <td>無法建立 IMS 入口網站記號，因為沒有任何 IMS 帳戶鏈結到所選取的 BSS 帳戶</br></br>找不到提供的使用者，或是提供的使用者不在作用中</br></br>SoftLayer_Exception_User_Customer_InvalidUserStatus：使用者帳戶目前為 cancel_pending。</br></br>等待使用者看見機器</td>
  <td>用來存取 IBM Cloud 基礎架構 (SoftLayer) 組合的 API 金鑰擁有者沒有執行此動作的必要許可權，或可能處於擱置刪除狀態。</br></br><strong>身為使用者</strong>，請遵循下列步驟：
  <ol><li>如果您可以存取多個帳戶，請確定您已登入想要使用 {{site.data.keyword.containerlong_notm}} 的帳戶。</li>
  <li>執行 <code>ibmcloud ks api-key-info --cluster &lt;cluster_name_or_ID&gt;</code> 以檢視用於存取 IBM Cloud 基礎架構 (SoftLayer) 組合的現行 API 金鑰擁有者。</li>
  <li>執行 <code>ibmcloud account list</code>，以檢視您目前使用的 {{site.data.keyword.Bluemix_notm}} 帳戶的擁有者。</li>
  <li>請聯絡 {{site.data.keyword.Bluemix_notm}} 帳戶的擁有者，並報告 API 金鑰擁有者對 IBM Cloud 基礎架構 (SoftLayer) 的許可權不足，或可能處於擱置刪除狀態。</li></ol>
  </br><strong>身為帳戶擁有者</strong>，請遵循下列步驟：<ol><li>請檢閱 [IBM Cloud 基礎架構 (SoftLayer) 中的必要許可權](/docs/containers?topic=containers-users#infra_access)，以執行先前失敗的動作。</li>
  <li>使用 [<code>ibmcloud ks api-key-reset --region <region></code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_api_key_reset) 指令來修正 API 金鑰擁有者的許可權或建立新的 API 金鑰。</li>
  <li>如果您或其他帳戶管理在您的帳戶中手動設定了 IBM Cloud 基礎架構 (SoftLayer) 認證，請執行 [<code>ibmcloud ks credential-unset --region <region></code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_unset) 以從您的帳戶中移除這些認證。</li></ol></td>
  </tr>
    </tbody>
  </table>

<br />


## 檢閱主節點性能
{: #debug_master}

您的 {{site.data.keyword.containerlong_notm}} 包含 IBM 管理的主節點，且主節點具有高可用性抄本、已套用自動安全修補程式，並實施自動化以便在發生突發事件時進行回復。您可以執行 `ibmcloud ks cluster-get --cluster <cluster_name_or_ID>`，檢查叢集主節點的性能、狀態及狀況。
{: shortdesc} 

**主節點性能**<br>
**主節點性能**會反映主節點元件的狀況，並且在有事情需要您注意時通知您。性能可能是下列其中一項：
*   `error`：主節點不在作業中。IBM 會自動收到通知，並採取動作來解決此問題。您可以繼續監視性能，直到主節點為 `normal` 為止。
*   `normal`：主節點在作業中及性能健全。無需採取任何動作。
*   `unavailable`：主節點可能無法存取，這表示部分動作（例如重新調整工作者節點儲存區大小）暫時無法使用。IBM 會自動收到通知，並採取動作來解決此問題。您可以繼續監視性能，直到主節點為 `normal` 為止。 
*   `unsupported`：主節點執行不受支援的 Kubernetes 版本。您必須[更新叢集](/docs/containers?topic=containers-update)，以讓主節點回到 `normal` 性能。

**主節點狀態及狀況**<br>
**主節點狀態**提供來自主節點狀況的何種作業正在進行中的詳細資料。狀態包含時間戳記，它記錄了主節點已在相同狀況中（例如 `Ready (1 month ago)`）多長的時間。**主節點狀況**反映可以對主節點執行之可能作業的生命週期，例如 deploying、updating 及 deleting。各個狀況說明於下表。

<table summary="每個表格列都應該從左到右閱讀，第一欄為主節點狀況，第二欄則為說明。">
<caption>主節點狀況</caption>
   <thead>
   <th>主節點狀況</th>
   <th>說明</th>
   </thead>
   <tbody>
<tr>
   <td>`deployed`</td>
   <td>已順利部署主節點。請檢查狀態以驗證主節點為 `Ready`，或是查看是否有更新可用。</td>
   </tr>
 <tr>
     <td>`deploying`</td>
     <td>主節點目前正在部署。請等待狀況變成 `deployed` 再使用叢集，例如新增工作者節點。</td>
    </tr>
   <tr>
     <td>`deploy_failed`</td>
     <td>主節點無法部署。IBM 支援中心已收到通知，並努力解決問題。如需相關資訊，請檢查**主節點狀態**欄位，或等待狀況變成 `deployed`。</td>
   </tr>
   <tr>
   <td>`deleting`</td>
   <td>目前正在刪除主節點，因為您已刪除叢集。您無法復原刪除。刪除叢集之後，您便無法再檢查主節點狀況，因為已完全移除叢集。</td>
   </tr>
     <tr>
       <td>`delete_failed`</td>
       <td>主節點無法刪除。IBM 支援中心已收到通知，並努力解決問題。您無法藉由嘗試重新刪除叢集來解決問題。相反地，如需相關資訊，請檢查**主節點狀態**欄位，或等待刪除叢集。</td>
      </tr>
      <tr>
       <td>`updating`</td>
       <td>主節點正在更新其 Kubernetes 版本。更新可能是自動套用的修補程式更新，或是您藉由更新叢集而套用的次要或主要版本。在更新期間，您的高可用性主節點可以繼續處理要求，且您的應用程式工作負載及工作者節點會繼續執行。主節點更新完成之後，您可以[更新工作者節點](/docs/containers?topic=containers-update#worker_node)。<br><br>
       如果更新不成功，主節點會回到 `deployed` 狀況並繼續執行舊版。IBM 支援中心已收到通知，並努力解決問題。您可以在**主節點狀態**欄位中檢查更新是否失敗。</td>
    </tr>
   </tbody>
 </table>


<br />



## 應用程式部署除錯
{: #debug_apps}

請檢閱您既有的選項以進行應用程式部署除錯，並找出失敗的主要原因。

開始之前，請確定您具有已在其中部署應用程式之名稱空間的[**撰寫者**或**管理員** {{site.data.keyword.Bluemix_notm}} IAM 服務角色](/docs/containers?topic=containers-users#platform)。

1. 執行 `describe` 指令，以尋找服務或部署資源中的異常狀況。

 範例：
 <pre class="pre"><code>kubectl describe service &lt;service_name&gt; </code></pre>

2. [檢查容器是否停留在 `ContainerCreating` 狀態](/docs/containers?topic=containers-cs_troubleshoot_storage#stuck_creating_state)。

3. 檢查叢集是否處於 `Critical` 狀況。如果叢集處於 `Critical` 狀況中，請檢查防火牆規則，確認主節點可以與工作者節點通訊。

4. 驗證服務正在接聽正確的埠。
   1. 取得 Pod 的名稱。
     <pre class="pre"><code>kubectl get pods</code></pre>
   2. 登入容器。
     <pre class="pre"><code>kubectl exec -it &lt;pod_name&gt; -- /bin/bash</code></pre>
   3. 從容器內 Curl 應用程式。如果無法存取埠，服務可能未接聽正確的埠，或是應用程式可能有問題。請使用正確的埠來更新服務的配置檔，並重新部署或調查應用程式的潛在問題。
     <pre class="pre"><code>curl localhost: &lt;port&gt;</code></pre>

5. 驗證服務已正確鏈結至 Pod。
   1. 取得 Pod 的名稱。
     <pre class="pre"><code>kubectl get pods</code></pre>
   2. 登入容器。
     <pre class="pre"><code>kubectl exec -it &lt;pod_name&gt; -- /bin/bash</code></pre>
   3. Curl 服務的叢集 IP 位址及埠。如果無法存取 IP 位址及埠，請查看服務的端點。如果未列出端點，則服務的選取器與 Pod 不符。如果列出端點，請查看服務上的目標埠欄位，並確定目標埠與用於 Pod 的埠相同。
     <pre class="pre"><code>curl &lt;cluster_IP&gt;:&lt;port&gt;</code></pre>

6. 對於 Ingress 服務，驗證可以從叢集內存取服務。
   1. 取得 Pod 的名稱。
     <pre class="pre"><code>kubectl get pods</code></pre>
   2. 登入容器。
     <pre class="pre"><code>kubectl exec -it &lt;pod_name&gt; -- /bin/bash</code></pre>
   2. Curl 為 Ingress 服務指定的 URL。如果無法存取 URL，請檢查叢集與外部端點之間是否有防火牆問題。
     <pre class="pre"><code>curl &lt;host_name&gt;.&lt;domain&gt;</code></pre>

<br />



## 取得協助及支援
{: #ts_getting_help}

叢集仍有問題？
{: shortdesc}

-  在終端機中，有 `ibmcloud` CLI 及外掛程式的更新可用時，就會通知您。請務必保持最新的 CLI，讓您可以使用所有可用的指令及旗標。
-   若要查看 {{site.data.keyword.Bluemix_notm}} 是否可用，請[檢查 {{site.data.keyword.Bluemix_notm}} 狀態頁面 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/status?selected=status)。
-   將問題張貼到 [{{site.data.keyword.containerlong_notm}} Slack ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://ibm-container-service.slack.com)。如果您的 {{site.data.keyword.Bluemix_notm}} 帳戶未使用 IBM ID，請[要求邀請](https://bxcs-slack-invite.mybluemix.net/)以加入此 Slack。
    {: tip}
-   檢閱討論區，以查看其他使用者是否發生過相同的問題。使用討論區提問時，請標記您的問題，以便 {{site.data.keyword.Bluemix_notm}} 開發團隊能看到它。
    -   如果您在使用 {{site.data.keyword.containerlong_notm}} 開發或部署叢集或應用程式時有技術方面的問題，請將問題張貼到 [Stack Overflow ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://stackoverflow.com/questions/tagged/ibm-cloud+containers)，並使用 `ibm-cloud`、`kubernetes` 及 `containers` 來標記問題。
    -   若為服務及開始使用指示的相關問題，請使用 [IBM Developer Answers ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix) 討論區。請包含 `ibm-cloud` 及 `containers` 標籤。如需使用討論區的詳細資料，請參閱[取得協助](/docs/get-support?topic=get-support-getting-customer-support#using-avatar)。
-   開立案例，以與「IBM 支援中心」聯絡。若要瞭解如何開立 IBM 支援中心案例，或是瞭解支援層次與案例嚴重性，請參閱[與支援中心聯絡](/docs/get-support?topic=get-support-getting-customer-support)。當您報告問題時，請包含您的叢集 ID。若要取得叢集 ID，請執行 `ibmcloud ks clusters`。您也可以使用 [{{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool](/docs/containers?topic=containers-cs_troubleshoot#debug_utility)，來收集及匯出叢集裡的相關資訊，以與 IBM 支援中心共用。
{: tip}

