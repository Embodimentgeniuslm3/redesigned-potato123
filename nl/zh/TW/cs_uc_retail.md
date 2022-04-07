---

copyright:
  years: 2014, 2019
lastupdated: "2019-05-31"

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


# {{site.data.keyword.cloud_notm}} 的零售使用案例
{: #cs_uc_retail}

這些使用案例強調顯示 {{site.data.keyword.containerlong_notm}} 上的工作負載如何充分運用市場見解分析、全球的多地區部署，以及具有 {{site.data.keyword.messagehub_full}} 及物件儲存空間的庫存管理。
{: shortdesc}

## 實體零售商會搭配使用 API 與全球商業夥伴以驅動全通路銷售來共用資料
{: #uc_data-share}

Line-of-Business (LOB) Exec 需要增加銷售通道，但內部部署資料中心已關閉零售系統。競爭擁有全球商業夥伴，可交叉銷售及向上銷售其貨品排列：跨實體和線上網站。
{: shortdesc}

為何要使用 {{site.data.keyword.cloud_notm}}：{{site.data.keyword.containerlong_notm}} 提供一種公用雲端生態系統，其中，容器可讓新商業夥伴及其他外部各方透過 API 共同開發應用程式及資料。現在零售系統位於公用雲端上，API 也會簡化資料共用以及快速開始新的應用程式開發。如果「開發人員」可輕鬆地進行實驗，並使用工具鏈將變更快速推送至「開發」及「測試」系統，則應用程式部署會增加。

{{site.data.keyword.containerlong_notm}} 及重要技術：
* [適合各種 CPU、RAM、儲存空間需求的叢集](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)
* [{{site.data.keyword.cos_full}} 可以跨應用程式持續保存及同步處理資料](/docs/tutorials?topic=solution-tutorials-pub-sub-object-storage#pub-sub-object-storage)
* [DevOps 原生工具，包括 {{site.data.keyword.contdelivery_full}} 中的開放式工具鏈](https://www.ibm.com/cloud/garage/toolchains/)

**環境定義：零售商會搭配使用 API 與全球商業夥伴以驅動全通路銷售來共用資料**

* 零售商面臨強大的競爭壓力。首先，他們需要遮罩跨入新產品及新通路的複雜性。例如，他們需要擴充產品複雜度。同時，它需要更為簡單，讓其客戶可以跳到各品牌。
* 這個跳躍品牌的能力表示零售生態系統需要連線至商業夥伴。然後，雲端可以帶出商業夥伴、客戶及其他外部各方的新價值。
* 激增的使用者事件（例如黑色星期五）會濫用現有線上系統，並強制零售商過度佈建運算基礎架構。
* 零售商的「開發人員」需要持續發展應用程式，但傳統工具會讓經常部署更新及特性的能力變慢，特別是在與商業夥伴團隊分工合作時更是如此。  

**解決方案**

需要有更聰明的購物體驗，才能增加留客率和毛利率。零售商的傳統銷售模型會因為缺乏商業夥伴庫存無法進行交叉銷售及向上銷售而不佳。它們的購物者正在尋找提高便利性的機會，因此他們可以一起快速尋找相關項目（例如瑜伽褲及瑜珈墊）。

零售商也必須為客戶提供有用的內容（例如產品資訊、替代產品資訊、檢閱及即時庫存可見性）。而這些客戶想要透過個人行動裝置及配備行動裝置的商店店員在線上及店內購買。

解決方案由以下主要元件組成：
* 庫存：可聚集及傳達庫存的商業夥伴生態系統應用程式，特別是新產品簡介（包括商業夥伴可在其自己的零售及 B2B 應用程式中重複使用的 API）
* 交叉及向上銷售：使用 API 呈現交叉銷售及向上銷售機會的應用程式，而 API 可以用於各種電子商務及行動應用程式
* 開發環境：「開發」、「測試」及「正式作業」系統的 Kubernetes 叢集可提高零售商與其商業夥伴之間的協同作業及資料共用

為了讓零售商與全球商業夥伴合作，庫存 API 需要變更，以符合每個地區的語言及市場喜好設定。{{site.data.keyword.containerlong_notm}} 提供多個地區的涵蓋範圍（包括北美洲、歐洲、亞洲及澳洲），讓 API 反映每個國家/地區中的需求，並且確保 API 呼叫具有低延遲。

另一項要求是，庫存資料必須可與公司的事業夥伴和客戶共享。使用庫存 API，「開發人員」可以使用應用程式（例如行動庫存應用程式或 Web 電子商務解決方案）來呈現資訊。「開發人員」也忙於建置及維護主要電子商務網站。簡言之，他們需要專注在進行程式編碼，而不是管理基礎架構。

因此，他們選擇 {{site.data.keyword.containerlong_notm}}，因為 IBM 簡化了基礎架構管理：
* 管理 Kubernetes 主節點、IaaS 及作業元件（例如 Ingress 及儲存空間）
* 監視工作者節點的性能及回復
* 提供廣域運算，因此「開發人員」擁有所需工作負載及資料所在之地區中的硬體基礎架構

此外，API 微服務的記載及監視（特別是它們如何從後端系統取回個人化資料）可以輕鬆地與 {{site.data.keyword.containerlong_notm}} 整合。「開發人員」不會浪費時間來建置複雜記載系統，只能疑難排解即時系統。

{{site.data.keyword.messagehub_full}} 用來作為即時事件平台，以將商業夥伴庫存系統中快速變更的資訊帶入 {{site.data.keyword.cos_full}}。

**解決方案模型**

隨需應變運算、儲存空間及事件管理是視需要在可存取全球零售庫存的公用雲端中執行

技術解決方案：
* {{site.data.keyword.containerlong_notm}}
* {{site.data.keyword.messagehub_full}}
* {{site.data.keyword.cos_full}}
* {{site.data.keyword.contdelivery_full}}
* IBM Cloud Logging and Monitoring
* {{site.data.keyword.appid_short_notm}}

**步驟 1：使用微服務來容器化應用程式**
* 根據應用程式的功能範圍及其相依關係，將應用程式建構成在 {{site.data.keyword.containerlong_notm}} 內執行的一組共同微服務。
* 將應用程式部署至 {{site.data.keyword.containerlong_notm}} 中執行的容器映像檔。
* 透過 Kubernetes 提供標準化 DevOps 儀表板。
* 針對批次及其他不常執行的庫存工作負載，啟用隨需應變運算調整。

**步驟 2：確定廣域可用性**
* {{site.data.keyword.containerlong_notm}} 中的內建 HA 工具會平衡每個地理區域內的工作負載（包括自我修復及負載平衡）。
* 由 IBM Cloud Internet Services 來處理負載平衡、防火牆及 DNS。
* 使用工具鏈及 Helm 部署工具，應用程式也會部署至全球叢集，因此工作負載及資料符合地區需求（特別是個人化）。

**步驟 3：瞭解使用者**
* {{site.data.keyword.appid_short_notm}} 提供登入功能，而不需要變更應用程式碼。
* 使用者已登入之後，您可以使用 {{site.data.keyword.appid_short_notm}} 來建立設定檔，並將使用者的應用程式體驗個人化。

**步驟 4：共用資料**
* {{site.data.keyword.cos_full}} 及 {{site.data.keyword.messagehub_full}} 提供即時及歷程資料儲存空間，因此，交叉銷售方案代表商業夥伴所提供的庫存。
* API 容許零售商的商業夥伴將資料共用至其電子商務及 B2B 應用程式。

**步驟 5：持續交付**
* 在 IBM Cloud Logging and Monitoring 工具（各種「開發人員」可以存取的雲端型工具）上新增共同開發的 API 時，除錯共同開發的 API 會變得更為簡單。
* {{site.data.keyword.contdelivery_full}} 協助「開發人員」搭配使用可自訂且可共用範本與 IBM、協力廠商及開放程式碼的工具，快速佈建整合式工具鏈。自動化建置及測試，並使用分析來控制品質。
* 「開發人員」在其「開發」及「測試」叢集裡建置及測試應用程式之後，會使用 IBM 持續整合及交付（CI 及 CD）工具鏈，將應用程式部署至全球叢集。
* {{site.data.keyword.containerlong_notm}} 可輕鬆地推出及回復應用程式；透過 Istio 的智慧型遞送及負載平衡部署自訂應用程式，以測試行銷活動。

**結果**
* 微服務可以大幅減少修補程式、錯誤修正程式及新增特性的交付時間。起始全世界開發十分快速，而且每週會頻繁更新 40 次。
* 零售商及其商業夥伴可以使用 API 來立即存取可供貨庫存及交付排程。
* 使用 {{site.data.keyword.containerlong_notm}} 以及 IBM CI 和 CD 工具，A-B 版本的應用程式已備妥可測試行銷活動。
* {{site.data.keyword.containerlong_notm}} 提供可擴充運算，因此庫存及交叉銷售 API 工作負載可能會在一年的高峰期間成長（例如秋天的假日）。

## 傳統食品雜貨店透過數位見解增加客戶資料流量及銷售
{: #uc_grocer}

「行銷長 (CMO)」需要藉由讓商店成為差異化資產，以將店內的客戶資料流量增加 20%。大型零售競爭者及線上零售商都會竊取銷售。同時，CMO 需要在不降價促銷的情況下減少庫存，因為持有庫存太久會鎖住數百萬的資金。
{: shortdesc}

為何要使用 {{site.data.keyword.cloud_notm}}：{{site.data.keyword.containerlong_notm}} 提供輕鬆地啟動更多運算，其中，「開發人員」會快速新增「雲端分析」服務，以取得銷售行為見解及數位市場適應性。

重要技術：    
* [水平調整以加速開發](/docs/containers?topic=containers-app#highly_available_apps)
* [適合各種 CPU、RAM、儲存空間需求的叢集](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)
* [使用 Watson Discovery 的市場趨勢見解](https://www.ibm.com/watson/services/discovery/)
* [DevOps 原生工具，包括 {{site.data.keyword.contdelivery_full}} 中的開放式工具鏈](https://www.ibm.com/cloud/garage/toolchains/)
* [使用 {{site.data.keyword.messagehub_full}} 進行庫存管理](/docs/services/EventStreams?topic=eventstreams-about#about)

**環境定義：傳統食品雜貨店透過數位見解增加客戶資料流量及銷售**

* 線上零售商及大型零售商店的競爭壓力會干擾傳統雜貨零售模型。銷售額下降，實體商店中的低客流量證明了這一點。
* 其忠誠度方案需要在結帳時利用現代化的紙本優待券來提供一臂之力。因此，「開發人員」必須持續發展相關的應用程式，但傳統工具會讓經常部署更新及特性的能力變慢。  
* 某些高價值庫存不會如預期般移動，但「美食家」移動似乎會在大都會市場中不斷成長。

**解決方案**

食品雜貨店需要一個應用程式，才能在可重複使用的雲端分析平台中增加轉換，以及儲存資料流量來產生新的銷售與建置客戶忠誠度。店內目標體驗可以是事件，以及根據特定事件親緣性來吸引忠誠度及新客戶的服務或產品供應商。然後，商店及商業夥伴會提供獎勵以進入事件，以及從商店或商業夥伴購買產品。  

在事件之後，會引導客戶購買必要的產品，讓他們可以在日後自行重複示範的活動。目標客戶體驗的測量方式是獎勵兌換及新的忠誠度客戶註冊。超個人化行銷事件以及用來追蹤店內採購之工具的組合，可以將目標體驗帶至產品採購。所有這些動作都會導致更高的資料流量及轉換。

作為範例事件，會將當地主廚帶入商店中，以顯示如何做出美味的餐點。商店會提供來店禮。例如，提供主廚餐廳的免費開胃菜，而購買示範餐點材料則會提供額外獎勵（例如，滿 $150 的購物車減價 $20）。

解決方案由以下主要元件組成：
1. 庫存分析：自訂店內事件（食譜、材料清單及產品位置）來行銷低流動性庫存。
2. 「忠誠度行動應用程式」透過數位優待券、購物清單、商店地圖上的產品庫存（價格、可用性）及社交分享來提供目標行銷。
3. 「社交媒體分析」根據趨勢偵測客戶喜好設定來提供個人化：菜餚、主廚及材料。分析會連接地區趨勢與個別的 Twitter、Pinterest 及 Instagram 活動。
4. 「開發人員友善的工具」可加速推出特性及錯誤修正程式。

產品庫存、商店補貨及產品預測的後端庫存系統具有豐富的資訊，但現代分析可以解開如何更充分地移動高端產品的新見解。使用 {{site.data.keyword.cloudant}} 與 IBM Streaming Analytics 的組合，CMO 即可尋找材料的甜蜜點，以符合自訂店內事件。

{{site.data.keyword.messagehub_full}} 用來作為即時事件平台，以將庫存系統中快速變更的資訊帶入 IBM Streaming Analytics。

具有 Watson Discovery（特質及語氣見解）的社交媒體分析也會提供庫存分析的趨勢，以改善產品預測。

忠誠度行動應用程式提供詳細的個人化資訊，特別是客戶使用其社交分享特性（例如張貼食譜）時。

除了行動應用程式之外，「開發人員」忙於建置及維護關聯至傳統結帳優待券的現有忠誠度應用程式。簡言之，他們需要專注在進行程式編碼，而不是管理基礎架構。因此，他們選擇 {{site.data.keyword.containerlong_notm}}，因為 IBM 簡化了基礎架構管理：
* 管理 Kubernetes 主節點、IaaS 及作業元件（例如 Ingress 及儲存空間）
* 監視工作者節點的性能及回復
* 提供廣域運算，因此「開發人員」不負責資料中心內的基礎架構設定

**解決方案模型**

隨需應變運算、儲存空間及事件管理是在可存取後端 ERP 系統的公用雲端中執行

技術解決方案：
* {{site.data.keyword.containerlong_notm}}
* {{site.data.keyword.messagehub_full}}
* {{site.data.keyword.cloudant}}
* IBM Streaming Analytics
* IBM Watson Discovery

**步驟 1：使用微服務來容器化應用程式**

* 將庫存分析及行動應用程式建構成微服務，並將它們部署至 {{site.data.keyword.containerlong_notm}} 中的容器。
* 透過 Kubernetes 提供標準化 DevOps 儀表板。
* 針對批次及其他不常執行的庫存工作負載，隨需應變調整運算。

**步驟 2：分析庫存及趨勢**
* {{site.data.keyword.messagehub_full}} 用來作為即時事件平台，以將庫存系統中快速變更的資訊帶入 IBM Streaming Analytics。
* 具有 Watson Discovery 及庫存系統資料的社交媒體分析會與 IBM Streaming Analytics 整合，以提供推銷及行銷建議。

**步驟 3：使用行動忠誠度應用程式來交付促銷活動**
* 使用 IBM Mobile Starter Kit 及其他 IBM Mobile 服務（例如 {{site.data.keyword.appid_full_notm}}）快速開始開發行動應用程式。
* 優待券形式的促銷活動及其他授權都會傳送至使用者的行動應用程式。促銷活動是使用庫存及社交分析（以及其他後端系統）所識別。
* 儲存行動應用程式上的促銷秘訣及兌換（兌換結帳優待券）會反應至 ERP 系統，以進一步分析。

**結果**
* 使用 {{site.data.keyword.containerlong_notm}}，微服務可以大幅減少修補程式、錯誤修正程式及新增特性的交付時間。起始開發十分快速，並且會頻繁更新。
* 店內的客流量及銷售會增加，其方式是讓商店本身成為差異化資產。
* 同時，社交及認知分析的新見解已改善減少的庫存 OpEx（作業費用）。
* 行動應用程式中的社交分享也有助於識別及行銷給新客戶。
