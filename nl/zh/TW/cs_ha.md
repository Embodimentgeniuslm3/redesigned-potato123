---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-05"

keywords: kubernetes, iks, disaster recovery, dr, ha, hadr

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



# {{site.data.keyword.containerlong_notm}} 的高可用性
{: #ha}

使用內建的 Kubernetes 及 {{site.data.keyword.containerlong}} 特性，讓叢集具有更高的可用性，並在叢集裡的元件失敗時避免應用程式關閉。
{: shortdesc}

高可用性是 IT 基礎架構中的核心原則，可讓您的應用程式保持執行中，即使是網站局部或全部故障。高可用性的主要目的是要消除 IT 基礎架構中的潛在故障點。例如，您可以藉由新增備援並設定失效接手機制，為系統故障做好準備。

您可以在 IT 基礎架構及叢集的不同元件內，達到不同層次的高可用性。您適合的可用性層次視多個因素而定，例如您的業務需求、您與客戶之間的「服務水準合約」，以及您要花費的金額。

## {{site.data.keyword.containerlong_notm}} 中的潛在故障點的概觀
{: #fault_domains}

{{site.data.keyword.containerlong_notm}} 架構及基礎架構的設計旨在確保可靠性、縮短處理延遲時間及服務執行時間最大化。不過，可能會發生故障。取決於您在 {{site.data.keyword.Bluemix_notm}} 中管理的服務，您可能無法容忍故障，即使故障只持續幾分鐘而已。
{: shortdesc}

{{site.data.keyword.containerlong_notm}} 提供數種方法，藉由新增備援及反親緣性，以在叢集裡新增更多可用性。請檢閱下列映像檔，以瞭解潛在的故障點，以及其消除方式。

<img src="images/cs_failure_ov.png" alt="對於 {{site.data.keyword.containerlong_notm}} 地區內的高可用性叢集裡的錯誤網域的概觀。" width="250" style="width:250px; border-style: none"/>

<dl>
<dt> 1. 容器或 Pod 故障。</dt>
  <dd><p>容器及 Pod 在設計上是短暫存在的，且可能非預期地故障。例如，如果應用程式發生錯誤，則容器或 Pod 可能會當機。若要讓應用程式具有高可用性，您必須確保具有足夠的應用程式實例能夠處理工作負載，以及有更多實例來因應故障情況。理論上，這些實例會分散在多個工作者節點之間，以在發生工作者節點故障時保護您的應用程式。</p>
  <p>請參閱[部署高可用性應用程式](/docs/containers?topic=containers-app#highly_available_apps)。</p></dd>
<dt> 2. 工作者節點故障。</dt>
  <dd><p>工作者節點是在實體硬體上執行的 VM。工作者節點故障包括硬體中斷，例如電源、散熱或網路問題，以及 VM 本身的問題。您可以藉由在叢集裡設定多個工作者節點，來說明工作者節點故障情況。</p><p class="note">不保證其中一個區域的工作者節點位於個別的實體運算主機上。例如，一個叢集可能有 3 個工作者節點，但這 3 個工作者節點全都建立在 IBM 區域的相同實體運算主機上。如果此實體運算主機關閉，則所有工作者節點都關閉。若要避免此故障情況，您必須在不同的區域中[設定多區域叢集或建立多個單一區域叢集](/docs/containers?topic=containers-ha_clusters#ha_clusters)。</p>
  <p>請參閱[建立具有多個工作者節點的叢集。](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_create)</p></dd>
<dt> 3. 叢集故障。</dt>
  <dd><p>[Kubernetes 主節點](/docs/containers?topic=containers-ibm-cloud-kubernetes-service-technology#architecture)是保持叢集運作的主要元件。主節點將叢集資源及其配置儲存在 etcd 資料庫中，作為叢集的單點真實資料 (SPOT)。Kubernetes API 伺服器是從工作者節點到主節點之所有叢集管理要求的主要進入點，或您要與叢集資源互動時。<br><br>如果發生主節點失敗，則工作負載會繼續在工作者節點上執行，但無法使用 `kubectl` 指令來使用叢集資源，或檢視叢集性能，直到備份主節點中的 Kubernetes API 伺服器為止。如果 Pod 在主節點中斷期間關閉，則除非工作者節點再次到達 Kubernetes API 伺服器，否則無法重新排定 Pod。<br><br>在主節點中斷期間，您仍然可以針對 {{site.data.keyword.containerlong_notm}} API 執行 `ibmcloud ks` 指令，以使用您的基礎架構資源（例如工作者節點或 VLAN）。如果您透過在叢集裡新增或移除工作者節點來變更現行叢集配置，則除非備份主節點，否則您的變更不會發生。

在主節點中斷期間，請不要將工作者節點重新啟動或重新開機。此動作會從您的工作者節點移除 Pod。因為 Kubernetes API 伺服器無法使用，所以無法將 Pod 重新排程至叢集裡的其他工作者節點。
{: important}
主要叢集具有高可用性，且會在個別主機上包含 Kubernetes API 伺服器、etcd、排程器及控制器管理程式的抄本，來防範在主節點更新這類期間發生運作中斷。</p><p>若要防範叢集主節點發生區域失敗，您可以：<ul><li>在[多區域都會位置](/docs/containers?topic=containers-regions-and-zones#zones)中建立叢集，這會將主節點分散在各區域之中。</li><li>在另一個區域設定第二個叢集。</li></ul></p>
  <p>請參閱[設定高可用性叢集。](/docs/containers?topic=containers-ha_clusters#ha_clusters)</p></dd>
<dt> 4. 區域故障。</dt>
  <dd><p>區域故障會影響所有實體運算主機及 NFS 儲存空間。故障包括電源、散熱、網路連線或儲存空間運作中斷，以及自然災害，例如洪水、地震和颶風等。若要避免發生區域故障，您必須讓叢集位於兩個不同區域，然後由外部負載平衡器進行負載平衡。</p>
  <p>請參閱[設定高可用性叢集](/docs/containers?topic=containers-ha_clusters#ha_clusters)。</p></dd>    
<dt> 5. 地區故障。</dt>
  <dd><p>每一個地區都有設定高可用性的負載平衡器，可從地區專用的 API 端點進行存取。負載平衡器會將送入及送出要求遞送至地區區域中的叢集。整個地區故障的可能性很低。不過，若要說明此故障，您可以在不同地區設定多個叢集，然後使用外部負載平衡器連接它們。如果整個地區發生故障，另一個地區中的叢集可以接管工作負載。</p><p class="note">多地區叢集需要數個「雲端」資源，視您的應用程式而定，它們可能既複雜又昂貴。檢查您是否需要多地區設定，或您是否可以容忍潛在的服務中斷。如果要設定多地區叢集，請確定您的應用程式和資料可在另一個地區管理，而且您的應用程式可以處理廣域資料抄寫。</p>
  <p>請參閱[設定高可用性叢集](/docs/containers?topic=containers-ha_clusters#ha_clusters)。</p></dd>   
<dt> 6a、6b. 儲存空間故障。</dt>
  <dd><p>在有狀態的應用程式中，資料扮演能讓您應用程式保持執行中的重要角色。請確定資料具有高可用性，以在發生潛在故障時能夠回復。在 {{site.data.keyword.containerlong_notm}} 中，您可以從數個選項中選擇，以持續保存您的資料。例如，您可以使用 Kubernetes 原生持續性磁區來佈建 NFS 儲存空間，或使用 {{site.data.keyword.Bluemix_notm}} 資料庫服務來儲存資料。</p>
  <p>請參閱[規劃高可用性資料](/docs/containers?topic=containers-storage_planning#persistent_storage_overview)。</p></dd>
</dl>
