---

copyright:
  years: 2014, 2019
lastupdated: "2019-05-31"

keywords: kubernetes, iks, helm

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


# IBM Cloud Kubernetes Service 夥伴
{: #service-partners}

IBM 致力於使 {{site.data.keyword.containerlong_notm}} 成為最佳 Kubernetes 服務，以協助您移轉、操作和管理 Kubernetes 工作負載。為向您提供在雲端中執行正式作業工作負載所需的所有功能，{{site.data.keyword.containerlong_notm}} 與其他協力廠商服務提供者合作，利用一流的記載、監視和儲存空間工具來增強叢集功能。
{: shortdesc}

請檢閱我們的合作夥伴及其提供的每種解決方案的優點。若要尋找可在叢集裡使用的其他專有 {{site.data.keyword.Bluemix_notm}} 和協力廠商開放程式碼服務，請參閱[瞭解 {{site.data.keyword.Bluemix_notm}} 和協力廠商整合](/docs/containers?topic=containers-ibm-3rd-party-integrations)。

## LogDNA
{: #logdna-partner}

{{site.data.keyword.la_full_notm}} 將 [LogDNA ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://logdna.com/) 作為協力廠商服務提供，可用於將智慧型記載功能新增到叢集和 APP。

### 優點
{: #logdna-benefits}

請檢閱下表，以尋找使用 LogDNA 可以獲得的重要優點清單。
{: shortdesc}

|優點|說明|
|-------------|------------------------------|
|集中日誌管理和日誌分析|將叢集配置為日誌來源後，LogDNA 會自動開始收集工作者節點、pod、APP 應用程式和網路的記載資訊。日誌由 LogDNA 自動剖析、編製索引、進行標籤和聚集，並在 LogDNA 儀表板中直觀視覺化，以便您可以輕鬆地深入研究叢集資源。可以使用內建的圖形工具來直觀視覺化最常見的錯誤碼或日誌項目。|
|可使用類似 Google 的搜尋語法輕鬆尋找|LogDNA 使用類似 Google 的搜尋語法，支援標準術語、`AND` 和 `OR` 運算，並允許您排除或結合搜尋詞彙，以協助您更輕鬆地尋找日誌。透過對日誌建立智慧型索引，可以隨時跳至特定日誌項目。|
|動態和靜態加密|LogDNA 會自動加密日誌，以保護傳輸中的日誌和靜態日誌。|
|自訂警示和日誌視圖|可以使用儀表板來尋找與搜尋準則符合的日誌，在視圖中儲存這些日誌，並與其他使用者共用此視圖，以簡化團隊成員之間的除錯。您還可以使用此視圖來建立可以傳送到下遊系統（如 PagerDuty、Slack 或電子郵件）的警示。|
|現成及自訂儀表板|您可以在各種現有儀表板之間進行選擇，或者建立您自己的儀表板，以依照您需要的方式來直觀視覺化日誌。|

### 與 {{site.data.keyword.containerlong_notm}} 整合
{: #logdna-integration}

LogDNA 由 {{site.data.keyword.la_full_notm}} 提供，這是一個可用於叢集的 {{site.data.keyword.Bluemix_notm}} 平台服務。{{site.data.keyword.la_full_notm}} 由 LogDNA 與 IBM 合作執行中。
{: shortdesc}

若要在叢集裡使用 LogDNA，必須在 {{site.data.keyword.Bluemix_notm}} 帳戶中佈建 {{site.data.keyword.la_full_notm}} 的實例，並將 Kubernetes 叢集配置為日誌來源。配置叢集後，會自動收集日誌並將其轉遞到 {{site.data.keyword.la_full_notm}} 服務實例。可以使用 {{site.data.keyword.la_full_notm}} 儀表板來存取日誌。   

如需相關資訊，請參閱[使用 {{site.data.keyword.la_full_notm}} 管理 Kubernetes 叢集日誌](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-kube)。

### 計費及支援
{: #logdna-billing-support}

{{site.data.keyword.la_full_notm}} 完全整合到 {{site.data.keyword.Bluemix_notm}} 支援系統中。如果使用 {{site.data.keyword.la_full_notm}} 時遇到問題，請在 [{{site.data.keyword.containerlong_notm}} Slack ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://ibm-container-service.slack.com/) 中的 `logdna-on-iks` 頻道中發帖提問，或者具有 [{{site.data.keyword.Bluemix_notm}} 支援案例](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support)。使用 IBM ID 登入 Slack。如果您未使用 IBM ID 作為 {{site.data.keyword.Bluemix_notm}} 帳戶，請[要求邀請以加入此 Slack ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://bxcs-slack-invite.mybluemix.net/)。

## Sysdig
{: #sydig-partner}

{{site.data.keyword.mon_full_notm}} 將 [Sysdig 監視器 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://sysdig.com/products/monitor/) 作為協力廠商雲端本機容器分析系統提供，可用於瞭解計算主機、APP、容器和網路的效能和性能。
{: shortdesc}

### 優點
{: #sydig-benefits}

請檢閱下表，以尋找使用 Sysdig 可以獲得的重要優點清單。
{: shortdesc}

|優點|說明|
|-------------|------------------------------|
|自動存取雲端本機和 Prometheus 定制度量|從各種預先定義的雲端本機和 Prometheus 定制度量中進行選擇，以瞭解計算主機、應用程式、容器和網路的效能和性能。|
|使用進階過濾器進行疑難排解|Sysdig 監視器會建立網路拓蹼，用於顯示工作者節點如何連接以及 Kubernetes 服務如何相互通訊。可以從工作者節點導覽至容器和單一系統呼叫，並在整個過程中對每個資源的重要度量進行分組和檢視。例如，使用這些度量可尋找接收要求最多的服務，或查詢和回應速度慢的服務。可以將這些資料與 Kubernetes 事件、自訂 CI/CD 事件或程式碼確定結合使用。
|自動異常偵測和自訂警示|定義關於您希望在叢集或群組資源中偵測到異常的情況下何時收到通知的規則和臨界值，以允許 Sysdig 在一個資源的行為不同於其他資源時通知您。可以將這些警示傳送到下遊工具，例如 ServiceNow、PagerDuty、Slack、VictorOps 或電子郵件。|
|現成及自訂儀表板|您可以在各種現有儀表板之間進行選擇，或者建立您自己的儀表板，以依照您需要的方式來直觀視覺化微服務的度量。|
{: caption="使用 Sysdig 監視器的優點" caption-side="top"}

### 與 {{site.data.keyword.containerlong_notm}} 整合
{: #sysdig-integration}

Sysdig 監視器由 {{site.data.keyword.mon_full_notm}} 提供，後者是一個可用於叢集的 {{site.data.keyword.Bluemix_notm}} 平台服務。{{site.data.keyword.mon_full_notm}} 由 Sysdig 與 IBM 合作執行中。
{: shortdesc}

若要在叢集裡使用 Sysdig 監視器，必須在 {{site.data.keyword.Bluemix_notm}} 帳戶中佈建 {{site.data.keyword.mon_full_notm}} 的實例，並將 Kubernetes 叢集配置為度量來源。配置叢集後，會自動收集度量並將其轉遞到 {{site.data.keyword.mon_full_notm}} 服務實例。可以使用 {{site.data.keyword.mon_full_notm}} 儀表板來存取度量。   

如需相關資訊，請參閱[分析在 Kubernetes 叢集裡部署的應用程式的度量](/docs/services/Monitoring-with-Sysdig?topic=Sysdig-kubernetes_cluster)。

### 計費及支援
{: #sysdig-billing-support}

由於 Sysdig 監視器由 {{site.data.keyword.mon_full_notm}} 提供，因此使用情況包含在 {{site.data.keyword.Bluemix_notm}} 的平台服務帳單中。如需定價資訊，請在 [{{site.data.keyword.Bluemix_notm}}「型錄」![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/observe/monitoring/create) 中檢閱可用方案。

{{site.data.keyword.mon_full_notm}} 完全整合到 {{site.data.keyword.Bluemix_notm}} 支援系統中。如果使用 {{site.data.keyword.mon_full_notm}} 時遇到問題，請在 [{{site.data.keyword.containerlong_notm}} Slack ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://ibm-container-service.slack.com/) 中的 `sysdig-monitoring` 頻道中發帖提問，或者開立 [{{site.data.keyword.Bluemix_notm}} 支援案例](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support)。使用 IBM ID 登入 Slack。如果您未使用 IBM ID 作為 {{site.data.keyword.Bluemix_notm}} 帳戶，請[要求邀請以加入此 Slack ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://bxcs-slack-invite.mybluemix.net/)。

## Portworx
{: #portworx-parter}

[Portworx ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://portworx.com/products/introduction/) 是一個高可用性軟體定義儲存空間解決方案，可用來管理容器化資料庫及其他有狀態應用程式的本端持續性儲存空間，或在多個區域的 Pod 之間共用資料。
{: shortdesc}

**何謂軟體定義儲存空間 (SDS)？** </br> SDS 解決方案會將各種類型、大小或不同供應商的儲存裝置抽象化，這些裝置連接至您叢集裡的工作者節點。硬碟上具有可用儲存空間的工作者節點會被當成節點新增至儲存空間叢集。在此叢集裡，已虛擬化實體儲存空間，並當成虛擬儲存區呈現給使用者。儲存空間叢集由 SDS 軟體所管理。如果資料必須儲存在儲存空間叢集，則 SDS 軟體會決定資料的儲存位置，以取得最高可用性。虛擬儲存空間隨附一組共同的功能和服務，讓您可以在不需考慮實際基礎儲存空間架構的情況下加以運用。

### 優點
{: #portworx-benefits}

請檢閱下表，以尋找使用 Portworx 可以獲得的重要優點清單。
{: shortdesc}

|優點|說明|
|-------------|------------------------------|
|屬於任一國家應用程式的雲端本機儲存空間和資料管理|Portworx 將連接到工作者節點的可用本端儲存空間聚集在一起（儲存空間的大小或類型可以各不相同），並建立統一的持續儲存層，供要在叢集裡執行的容器化資料庫或其他有狀態應用程式使用。透過使用 Kubernetes 持續性磁區要求 (PVC)，可以將本端持續性儲存空間新增至應用程式以儲存空間資料。|
|使用磁區抄寫的高可用性資料|Portworx 會自動抄寫叢集裡不同工作者節點和區域的磁區中的資料，以便可以隨時存取資料，並且在工作者節點發生故障或重新開機時，有狀態應用程式可以重新排定到其他工作者節點。|
|支援執行 `hyper-converged`|可以將 Portworx 配置為執行 [`hyper-converged` ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.portworx.com/portworx-install-with-kubernetes/storage-operations/hyperconvergence/)，以確保始終將運算資源和儲存空間放在相同工作者節點上。必須對應用程式進行重新排定時，Portworx 會將應用程式移至其中一個磁區抄本所在的工作者節點，以確保有狀態應用程式的本端磁碟存取速度和高效能。|
|使用 {{site.data.keyword.keymanagementservicelong_notm}} 加密資料|可以[設定 {{site.data.keyword.keymanagementservicelong_notm}} 加密金鑰](/docs/containers?topic=containers-portworx#encrypt_volumes)，這些金鑰由透過了 FIPS 140-2 二級認證的以雲端為基礎的硬體安全模組 (HSM) 進行保護，以保護磁區中的資料。您可以選擇使用一個加密金鑰來加密叢集裡的所有磁區，或是為每個磁區使用一個加密金鑰。將資料傳送至不同的工作者節點時，Portworx 會使用此金鑰來加密靜態資料和傳輸期間的資料。|
|內建 Snapshot 和雲端備份|可以透過建立 [Portworx Snapshot ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.portworx.com/portworx-install-with-kubernetes/storage-operations/create-snapshots/) 來儲存磁區及其資料的現行狀態。Snapshot 可以儲存在本端 Portworx 叢集上或雲端中。|
|使用 Lighthouse 進行整合監視|[Lighthouse ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.portworx.com/reference/lighthouse/) 是一個直覺式的圖形工具，可協助您管理及監視 Portworx 叢集和磁區 Snapshot。使用 Lighthouse，您可以檢視 Portworx 叢集的性能，包括可用的儲存空間節點數目、磁區及可用容量，以及在 Prometheus、Grafana 或 Kibana 中分析資料。|
{: caption="使用 Portworx 的優點" caption-side="top"}

### 與 {{site.data.keyword.containerlong_notm}} 整合
{: #portworx-integration}

{{site.data.keyword.containerlong_notm}} 提供工作者節點特性，這些特性已針對 SDS 用法最佳化，並且隨附一個以上可用來儲存資料的原始、未格式化及未裝載的本端磁碟。當您使用隨附 10Gbps 網路速度的 [SDS 工作者節點機器](/docs/containers?topic=containers-planning_worker_nodes#sds)時，Portworx 會提供最佳效能。不過，您可以在非 SDS 工作者節點特性上安裝 Portworx，但可能無法獲得應用程式所需的效能優點。
{: shortdesc}

Portworx 是使用 [Helm chart](/docs/containers?topic=containers-portworx#install_portworx) 安裝的。安裝 Helm chart 時，Portworx 會自動分析叢集裡可用的本端持續性儲存空間，並將儲存空間新增到 Portworx 儲存空間層。若要將儲存空間從 Portworx 儲存空間層新增至應用程式，必須使用 [Kubernetes 持續性磁區要求](/docs/containers?topic=containers-portworx#add_portworx_storage)。

若要在叢集裡安裝 Portworx，您必須具有 Portworx 授權。如果您是新手使用者，則可以將 `px-enterprise` 版本用作試用版本。試用版為您提供完整的 Portworx 功能，您可以測試 30 天。試用版本到期後，必須[購買 Portworx 授權 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](/docs/containers?topic=containers-portworx#portworx_license) 才能繼續使用 Portworx 叢集。


如需如何安裝 Portworx 並用於 {{site.data.keyword.containerlong_notm}} 的相關資訊，請參閱[使用 Portworx 在軟體定義的儲存空間 (SDS) 上儲存空間資料](/docs/containers?topic=containers-portworx)。

### 計費及支援
{: #portworx-billing-support}

隨附本端磁碟的 SDS 工作者節點機器以及用於 Portworx 的虛擬機器會包含在依月 {{site.data.keyword.containerlong_notm}} 帳單中。如需定價資訊，請參閱 [{{site.data.keyword.Bluemix_notm}}「型錄」![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/kubernetes/catalog/cluster)。Portworx 授權個別收費，不包含在依月帳單中。
{: shortdesc}

如果使用 Portworx 時遇到問題，或要聊聊特定使用案例的 Portworx 配置，請將問題張貼到 [{{site.data.keyword.containerlong_notm}} Slack ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://ibm-container-service.slack.com/) 中的 `portworx-on-iks` 頻道。使用 IBM ID 登入 Slack。如果您未使用 IBM ID 作為 {{site.data.keyword.Bluemix_notm}} 帳戶，請[要求邀請以加入此 Slack ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://bxcs-slack-invite.mybluemix.net/)。
