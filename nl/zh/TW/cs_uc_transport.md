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


# {{site.data.keyword.cloud_notm}} 的交通工具使用案例
{: #cs_uc_transport}

這些使用案例強調顯示 {{site.data.keyword.containerlong_notm}} 上的工作負載如何充分運用工具鏈，以在全球實現快速應用程式更新及多地區部署。同時，這些工作負載可以連接至現有後端系統、使用 Watson AI 進行個人化，以及使用 {{site.data.keyword.messagehub_full}} 來存取 IOT 資料。

{: shortdesc}

## 貨運公司提高商業夥伴生態系統的全球系統可用性
{: #uc_shipping}

IT Exec 具有夥伴與之互動的全球船隻路線及排程系統。夥伴需要這些存取 IoT 裝置資料之系統中的最新資訊。但是，這些系統無法使用足夠的 HA 擴充到全球。
{: shortdesc}

為何要使用 {{site.data.keyword.cloud_notm}}：{{site.data.keyword.containerlong_notm}} 會使用五個 9s 可用性來調整容器化應用程式，以符合不斷成長的需求。如果「開發人員」可輕鬆地進行實驗，並將變更快速推送至「開發」及「測試」系統，則應用程式部署每天會發生 40 次。IoT Platform 可讓您輕鬆地存取 IoT 資料。

重要技術：    
* [商業夥伴生態系統的多地區](/docs/containers?topic=containers-regions-and-zones#regions-and-zones)
* [水平調整](/docs/containers?topic=containers-app#highly_available_apps)
* [{{site.data.keyword.contdelivery_full}} 中的開放式工具鏈](https://www.ibm.com/cloud/garage/toolchains/)
* [創新的雲端服務](https://www.ibm.com/cloud/products/#analytics)
* [{{site.data.keyword.messagehub_full}}以將事件資料提供給應用程式](/docs/services/EventStreams?topic=eventstreams-about#about)

**環境定義：貨運公司提高商業夥伴生態系統的全球系統可用性**

* 由於出貨物流的地區差異，造成很難跟上多個國家/地區中不斷成長的夥伴數目。範例是唯一的法規及運輸物流，其中公司必須跨界維護一致的記錄。
* 即時資料表示全球系統必須高度可用，才能減少傳輸作業的落差。船隻碼頭的時間表受高度管制，因此，在某些情況下不具彈性。Web 用量不斷成長，因此不穩定可能會導致不好的使用者體驗。
* 「開發人員」需要持續發展應用程式，但傳統工具會讓經常部署更新及特性的能力變慢。  

**解決方案**

貨運公司需要統一管理出貨時間表、庫存及海關文書工作。然後，它們可以正確地與其客戶共用出貨位置、出貨內容及交付排程。它們可以準確掌握貨物（例如應用裝置、衣物或農產品）的到達時間，這樣它們的貨運客戶就可以將該資訊傳達給他們自己的客戶。

解決方案由以下主要元件組成：
1. 每個出貨貨櫃之 IoT 裝置的串流資料：貨單及位置
2. 以數位方式與適合港口及運輸夥伴共用的海關文書工作（包括存取控制）
3. 聚集及傳達已出貨貨品送達資訊的貨運客戶應用程式，包括貨運客戶的 API 以在其自己的零售及企業消費型商務應用程式中重複使用出貨資料

為了讓貨運公司與全球夥伴合作，路線及排程系統需要進行當地修改，以符合每個地區的語言、法規及唯一港口物流。{{site.data.keyword.containerlong_notm}} 提供多個地區的全球涵蓋範圍（包括北美洲、歐洲、亞洲及澳洲），讓應用程式反映其夥伴在每個國家/地區中的需求。

IoT 裝置會串流處理 {{site.data.keyword.messagehub_full}} 配送至地區港口應用程式及關聯「海關」及「貨櫃」貨單資料儲存庫的資料。{{site.data.keyword.messagehub_full}} 是 IoT 事件的登入點。它會根據 Watson IoT Platform 提供給 {{site.data.keyword.messagehub_full}} 的受管理連線來配送事件。

事件位於 {{site.data.keyword.messagehub_full}} 之後，即會持續予以保存，以供港口運輸應用程式立即使用，並可於未來的任何時間點立即使用。需要最低延遲的應用程式會直接從事件串流 ({{site.data.keyword.messagehub_full}}) 即時使用。其他未來的應用程式（例如分析工具）可以選擇使用 {{site.data.keyword.cos_full}} 並以批次模式從事件儲存庫使用。

因為出貨資料是與公司的客戶共用，所以「開發人員」可確保客戶可以使用 API 在自己的應用程式中顯示出貨資料。這些應用程式的範例是行動追蹤應用程式或 Web 電子商務解決方案。「開發人員」也忙於建置及維護地區港口應用程式，而地區港口應用程式會收集及散播海關記錄及出貨貨單。簡言之，他們需要專注在進行程式編碼，而不是管理基礎架構。因此，他們選擇 {{site.data.keyword.containerlong_notm}}，因為 IBM 簡化了基礎架構管理：
* 管理 Kubernetes 主節點、IaaS 及作業元件（例如 Ingress 及儲存空間）
* 監視工作者節點的性能及回復
* 提供廣域運算，因此「開發人員」不負責所需工作負載及資料所在之數個地區中的基礎架構

為了達到廣域可用性，會在全球的數個資料中心部署「開發」、「測試」及「正式作業」系統。基於 HA，它們使用不同地理區域中多個叢集的組合以及多區域叢集。它們可以輕鬆地部署港口應用程式，以符合商業需要：
* 在「法蘭克福」叢集裡，以符合當地歐洲法規
* 在「美國」叢集裡，以確保本端可用性及失敗回復

它們也會將工作負載散佈到法蘭克福的多區域叢集，以確保歐洲版本的應用程式可供使用，同時有效率地平衡工作負載。因為每個地區都是使用港口應用程式來上傳唯一資料，所以應用程式的叢集會在低延遲的地區內進行管理。

對於「開發人員」，可以使用 {{site.data.keyword.contdelivery_full}} 自動化大部分的持續整合及交付 (CI/CD) 處理程序。公司可以定義工作流程工具鏈來準備容器映像檔、檢查漏洞，以及將其部署至 Kubernetes 叢集。

**解決方案模型**

隨需應變運算、儲存空間及事件管理是視需要在可存取全球出貨資料的公用雲端中執行

技術解決方案：
* {{site.data.keyword.containerlong_notm}}
* {{site.data.keyword.messagehub_full}}
* {{site.data.keyword.cos_full}}
* {{site.data.keyword.contdelivery_full}}

**步驟 1：使用微服務來容器化應用程式**

* 根據應用程式的功能範圍及其相依關係，將應用程式整合至 {{site.data.keyword.containerlong_notm}} 中的一組共同微服務。
* 將應用程式部署至 {{site.data.keyword.containerlong_notm}} 中的容器。
* 透過 Kubernetes 提供標準化 DevOps 儀表板。
* 針對批次及其他不常執行的庫存工作負載，啟用隨需應變運算調整。
* 使用 {{site.data.keyword.messagehub_full}} 以從 IoT 裝置管理串流資料。

**步驟 2：確定廣域可用性**
* {{site.data.keyword.containerlong_notm}} 中的內建 HA 工具會平衡每個地理區域內的工作負載（包括自我修復及負載平衡）。
* 由 IBM Cloud Internet Services 來處理負載平衡、防火牆及 DNS。
* 使用工具鏈及 Helm 部署工具，應用程式也會部署至全球叢集，因此工作負載及資料符合地區需求。

**步驟 3：共用資料**
* {{site.data.keyword.cos_full}} 與 {{site.data.keyword.messagehub_full}} 提供即時及歷程資料儲存空間。
* API 容許貨運公司客戶將資料共用至其應用程式。

**步驟 4：持續交付**
* {{site.data.keyword.contdelivery_full}} 協助「開發人員」搭配使用可自訂且可共用範本與 IBM、協力廠商及開放程式碼的工具，快速佈建整合式工具鏈。自動化建置及測試，並使用分析來控制品質。
* 「開發人員」在其「開發」及「測試」叢集裡建置及測試應用程式之後，會使用 IBM CI/CD 工具鏈，將應用程式部署至全球叢集。
* {{site.data.keyword.containerlong_notm}} 可輕鬆地推出及回復應用程式；透過 Istio 的智慧型遞送及負載平衡部署自訂應用程式，以符合地區需求。

**結果**

* 使用 {{site.data.keyword.containerlong_notm}} 及 IBM CI/CD 工具，會在它們從中收集資料的實體裝置附近管理應用程式的地區版本。
* 微服務可以大幅減少修補程式、錯誤修正程式及新增特性的交付時間。起始開發十分快速，並且會頻繁更新。
* 貨運客戶可以即時存取出貨位置、交付排程，甚至核准的港口記錄。
* 各種船隻碼頭的運輸夥伴可以察覺貨單及出貨詳細資料，以改善站上物流，而不會延遲。
* 與此案例不同，[Maersk 及 IBM 是合資企業](https://www.ibm.com/press/us/en/pressrelease/53602.wss)，可使用 Blockchain 改善國際供應鏈。

## 航空公司提供 3 週內的創新「人力資源 (HR)」權益網站
{: #uc_airline}

HR Exec (CHRO) 需要具有創新聊天機器人的新 HR 權益網站，但現行「開發」工具及平台平均要有較久的時間，應用程式才能上線。此狀況包括長期等待硬體採購。
{: shortdesc}

為何要使用 {{site.data.keyword.cloud_notm}}：{{site.data.keyword.containerlong_notm}} 提供輕鬆地啟動運算。然後，「開發人員」可以輕鬆地進行實驗，並使用開放式工具鏈將變更快速推送至「開發」及「測試」系統。他們的傳統軟體開發工具會在加入 IBM Watson Assistant 時得到提升。新的權益網站是在 3 週內建立。

重要技術：    
* [適合各種 CPU、RAM、儲存空間需求的叢集](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)
* [採用 Watson 技術的聊天機器人服務](https://developer.ibm.com/code/patterns/create-cognitive-banking-chatbot/)
* [DevOps 原生工具，包括 {{site.data.keyword.contdelivery_full}} 中的開放式工具鏈](https://www.ibm.com/cloud/garage/toolchains/)
* [SDK for Node.js](/docs/runtimes/nodejs?topic=Nodejs-nodejs_runtime#nodejs_runtime)

**環境定義：在 3 週內快速建置及部署創新的 HR 權益網站**
* 員工成長及變更 HR 原則，表示每年登記需要一個全新網站。
* 預期有互動式特性（例如聊天機器人），協助向現有員工傳達新的 HR 原則。
* 由於員工數目的成長，網站資料流量會不斷增加，但其基礎架構預算保持相同。
* HR 團隊面臨更快速移動的壓力：快速推出新的網站特性，並頻繁張貼最新的權益變更。
* 註冊期間會持續兩週，因此不接受關閉新應用程式。

**解決方案**

航空公司想要設計先放入人員的開放式文化。「人力資源高級管理人員」十分瞭解著重在獎勵及保留人才，會影響航空公司利潤。因此，每年推出權益是促進員工導向文化的關鍵層面。

它們需要一個解決方案來協助「開發人員」及其使用者：
* 現有權益前端：保險、教育供應項目、健康等等
* 地區特定特性：每個國家/地區都有唯一的 HR 原則，因此整體網站看起來可能很類似，但會顯示地區的特定權益
* 開發人員友善的工具：加速推出特性及錯誤修正程式
* 「聊天機器人」：用來提供有關權益的確實交談，並且有效率地解決使用者要求及問題。

技術解決方案：
* {{site.data.keyword.containerlong_notm}}
* IBM Watson Assistant 及 Tone Analyzer
* {{site.data.keyword.contdelivery_full}}
* IBM 記載及監視
* {{site.data.keyword.SecureGatewayfull}}
* {{site.data.keyword.appid_full_notm}}

加速開發是 HR Exec 的重要致勝點。團隊是從容器化其應用程式並將它們放入雲端開始。使用現代容器，「開發人員」可以使用 Node.js SDK 輕鬆地進行實驗，並將變更推送至「開發」及「測試」系統，以在個別叢集上橫向擴充。這些推送已使用開放式工具鏈及 {{site.data.keyword.contdelivery_full}} 自動化。不再以緩慢且容易出錯的建置處理程序來更新 HR 網站。他們可以將漸進式更新提供給其網站（每日或甚至更為頻繁）。此外，還會快速整合 HR 網站的記載及監視，特別是網站如何從後端權益系統取回個人化資料。「開發人員」不會浪費時間來建置複雜記載系統，只能疑難排解即時系統。開發人員不需要成為雲端安全的專家，他們可以透過 {{site.data.keyword.appid_full_notm}} 輕鬆地強制執行原則驅動的鑑別。

使用 {{site.data.keyword.containerlong_notm}}，它們已從專用資料中心的過度建置硬體移到可降低 IT 作業、維護及能源的可自訂運算。為了管理 HR 網站，它們可以輕鬆地設計 Kubernetes 叢集，以符合其 CPU、RAM 及儲存空間需求。減少人員成本的另一個因素是 IBM 管理 Kubernetes，因此「開發人員」可以專注於提供權益註冊的更佳員工體驗。

{{site.data.keyword.containerlong_notm}} 提供可擴充的運算資源，以及關聯的 DevOps 儀表板，依需求建立、調整與關閉應用程式及服務。使用業界標準容器技術應用程式，可以跨多個「開發」、「測試」及「正式作業」環境快速開發及共用。此設定提供可調整性的直接好處。使用 Kubernetes 的一組豐富部署及運行環境物件，HR 團隊即可可靠地監視及管理應用程式升級。它們也可以使用已定義的規則及自動化 Kubernetes 編排器，來抄寫及調整應用程式。

**步驟 1：容器、微服務及 Garage Method**
* 應用程式內建於 {{site.data.keyword.containerlong_notm}} 中所執行的一組共同微服務。架構代表具有最多品質問題之應用程式的功能範圍。
* 將應用程式部署至 {{site.data.keyword.containerlong_notm}} 中的容器，並使用 IBM Vulnerability Advisor 持續掃描。
* 透過 Kubernetes 提供標準化 DevOps 儀表板。
* 採用 IBM Garage Method 內的基本敏捷及反覆運算式開發作法，以啟用頻繁發行新的功能、修補程式及修正程式，且不需要關閉。

**步驟 2：現有權益的後端連線**
* {{site.data.keyword.SecureGatewayfull}} 用來為管理權益系統的內部部署系統建立安全通道。  
* 內部部署資料與 {{site.data.keyword.containerlong_notm}} 的組合可讓它們在遵守法規時存取敏感資料。
* 聊天機器人交談會反應至 HR 原則，並容許權益網站反映最受歡迎及最不受歡迎的權益（包括效能不佳方案的目標增進功能）。

**步驟 3：聊天機器人及個人化**
* IBM Watson Assistant 提供工具來快速支撐聊天機器人，以向使用者提供正確的權益資訊。
* Watson Tone Analyzer 確保客戶滿意聊天機器人交談，並在必要時採取人為介入。

**步驟 4：全球持續交付**
* {{site.data.keyword.contdelivery_full}} 協助「開發人員」搭配使用可自訂且可共用範本與 IBM、協力廠商及開放程式碼的工具，快速佈建整合式工具鏈。自動化建置及測試，並使用分析來控制品質。
* 「開發人員」在其「開發」及「測試」叢集裡建置及測試應用程式之後，會使用 IBM CI/CD 工具鏈，將應用程式部署至全球的「正式作業」叢集。
* {{site.data.keyword.containerlong_notm}} 可輕鬆地推出及回復應用程式。透過 Istio 的智慧型遞送及負載平衡部署自訂應用程式，以符合地區需求。
* {{site.data.keyword.containerlong_notm}} 中的內建 HA 工具會平衡每個地理區域內的工作負載（包括自我修復及負載平衡）。

**結果**
* 使用聊天機器人這類工具，HR 團隊已證明對其人力而言創新是企業文化一部分，而不只是行話。
* 網站中的個人化與確實性，解決了現今航空公司人力不斷變更的預期。
* HR 網站的最新更新（包括員工聊天機器人交談所驅動的更新）會很快上線，因為「開發人員」每天至少推送變更 10 次。
* 使用 IBM 所關注的基礎架構管理，「開發」團隊就不需要只在 3 週內提供網站。
