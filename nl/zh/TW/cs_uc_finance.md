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


# {{site.data.keyword.cloud_notm}} 的金融服務使用案例
{: #cs_uc_finance}

這些使用案例強調顯示 {{site.data.keyword.containerlong_notm}} 上的工作負載如何充分運用高可用性、高效能運算、輕鬆啟動叢集以更快速地開發，以及來自 {{site.data.keyword.ibmwatson}} 的 AI。
{: shortdesc}

## 抵押貸款公司刪減成本並加速法規遵循
{: #uc_mortgage}

房貸公司的「風險管理副總裁」每天都要處理 7 千萬筆記錄，但內部部署系統的速度緩慢，也不精確。IT 費用會急速增加，因為硬體會快速老舊而且未完全利用。在等待硬體佈建時，法規遵循的進度便會變慢。
{: shortdesc}

為何要使用 {{site.data.keyword.Bluemix_notm}}：若要改善風險分析，該公司會查看 {{site.data.keyword.containerlong_notm}} 及 IBM Cloud Analytic 服務以降低成本、增加全球可用性，最後加速法規遵循。在多個地區中使用 {{site.data.keyword.containerlong_notm}}，可以在全球容器化及部署其分析應用程式，並改善可用性及處理當地法規。這些部署透過已是 {{site.data.keyword.containerlong_notm}} 一部分的熟悉開放程式碼工具予以加速。

{{site.data.keyword.containerlong_notm}} 及重要技術：
* [水平調整](/docs/containers?topic=containers-app#highly_available_apps)
* [多個地區的高可用性](/docs/containers?topic=containers-regions-and-zones#regions-and-zones)
* [適合各種 CPU、RAM、儲存空間需求的叢集](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)
* [容器安全及隔離](/docs/containers?topic=containers-security#security)
* [{{site.data.keyword.cloudant}} 可以跨應用程式持續保存及同步處理資料](/docs/services/Cloudant?topic=cloudant-getting-started#getting-started)
* [SDK for Node.js](/docs/runtimes/nodejs?topic=Nodejs-nodejs_runtime#nodejs_runtime)

**解決方案**

它們是從容器化分析應用程式並將它們放入雲端開始。它們的硬體問題一瞬間就會消失。它們能夠輕鬆設計 Kubernetes 叢集，以符合其高效能 CPU、RAM、儲存空間及安全需求。而且它們的分析應用程式變更時，不需要大量硬體投資就可以新增或縮減運算。使用 {{site.data.keyword.containerlong_notm}} 水平調整，其應用程式會隨著不斷成長的記錄數目進行調整，而加速產生法規報告。{{site.data.keyword.containerlong_notm}} 提供全世界的彈性運算資源，而這些資源是安全且高效能的，可完全使用現代運算資源。

現在，這些應用程式會從 {{site.data.keyword.cloudant}} 的資料倉儲接收大量資料。{{site.data.keyword.cloudant}} 中的雲端型儲存空間確保可用性高於在內部部署系統中鎖定它時。因為可用性十分重要，所以會跨全球資料中心部署應用程式：適用於 DR 及延遲。

它們也會加快其風險分析及法規遵循。其預測及風險分析功能（例如 Monte Carlo 計算）現在會透過反覆運算式敏捷部署持續更新。容器編排由受管理的 Kubernetes 所處理，因此作業成本也會減少。最後，抵押貸款的風險分析對市場中的快節奏變更更具回應力。

**環境定義：房貸的法規遵循及財務建模**

* 需要較好財務風險管理的需求增加，法規監督也會增加。相同的需求會驅動風險評量處理程序的關聯檢閱，並揭露更精細、整合式且豐富的法規報告。
* 「高效能運算網格」是財務建模的關鍵基礎架構元件。

公司的問題現在是規模和交付時間。

其現行環境為 7 年以上、內部部署，並且具有有限的運算、儲存空間及 I/O 容量。伺服器重新整理成本很高，需要很長的時間才能完成。軟體及應用程式更新遵循非正式處理程序，而且不可重複。實際 HPC 網格很難對其進行程式設計。API 對於加入的新「開發人員」而言太過複雜，因此需要未記載的知識。主要應用程式升級需要 6 - 9 個月的時間才能完成。

**解決方案模型：隨需應變運算、儲存空間及 I/O 服務是視需要在可安全存取內部部署企業資產的公用雲端中執行**

* 支援結構化及未結構化文件查詢的安全且可擴充文件儲存空間
* 「提升及轉移」現有企業資產及應用程式，同時將它們整合到一些未移轉的內部部署系統
* 縮短部署時間解決方案，以及實作標準 DevOps 和監視處理程序來處理可影響報告正確性的錯誤

**詳細解決方案**

* {{site.data.keyword.containerlong_notm}}
* {{site.data.keyword.cos_full_notm}}
* {{site.data.keyword.sqlquery_notm}} (Spark)
* {{site.data.keyword.cloudant}}
* {{site.data.keyword.SecureGateway}}

{{site.data.keyword.containerlong_notm}} 提供可擴充的運算資源，以及關聯的 DevOps 儀表板，依需求建立、調整與關閉應用程式及服務。使用業界標準容器，即可一開始就快速在 {{site.data.keyword.containerlong_notm}} 上重新管理應用程式，而不需要變更主要架構。

此解決方案提供可調整性的直接好處。使用 Kubernetes 的一組豐富部署及運行環境物件，抵押貸款公司即可可靠地監視及管理應用程式升級。它們也可以抄寫及調整使用已定義的規則及自動化 Kubernetes 編排器的應用程式。

{{site.data.keyword.SecureGateway}} 用來建立內部部署資料庫的安全管線，以及重新管理以在 {{site.data.keyword.containerlong_notm}} 中執行之應用程式的文件。

{{site.data.keyword.cos_full_notm}} 適用於所有原始文件及資料儲存空間，因為它們會持續增加。對於 Monte Carlo 模擬，會將工作流程管線放在定位，而模擬資料位在儲存於 {{site.data.keyword.cos_full_notm}} 的結構化檔案中。開始模擬的觸發程式會在 {{site.data.keyword.containerlong_notm}} 中調整運算服務，以將檔案資料分割為 N 個事件儲存區來處理模擬。{{site.data.keyword.containerlong_notm}} 會自動調整為 N 個相關聯的服務執行，並將中間結果寫入 {{site.data.keyword.cos_full_notm}}。這些結果由另一組 {{site.data.keyword.containerlong_notm}} 運算服務處理，以產生最終結果。

{{site.data.keyword.cloudant}} 是一個適用於許多資料驅動使用案例的現代 NoSQL 資料庫：從金鑰值到複雜文件導向資料儲存空間及查詢。為了管理不斷成長的一組法規及管理報告規則，抵押貸款公司會使用 {{site.data.keyword.cloudant}} 來儲存與進入公司之原始法規資料相關聯的文件。會觸發 {{site.data.keyword.containerlong_notm}} 上的運算處理程序，以編譯、處理及發佈各種報告格式的資料。報告中共用的中間結果會儲存為 {{site.data.keyword.cloudant}} 文件，因此，範本驅動處理程序可以用來產生必要的報告。

**結果**

* 複雜財務模擬的完成比先前使用現有內部部署系統要快 25% 的時間。
* 部署時間平均已從先前的 6 - 9 個月改善為 1 - 3 週。發生這項改善的原因是 {{site.data.keyword.containerlong_notm}} 容許使用已控管的受控處理程序來升高應用程式容器，並將它們取代為較新的版本。報告錯誤可以快速修正、解決問題（例如正確性）。
* 使用 {{site.data.keyword.containerlong_notm}} 及 {{site.data.keyword.cloudant}} 所帶來的一組一致且可擴充的儲存空間及運算服務，降低法規報告成本。
* 一段時間之後，一開始「提升及轉移」至雲端的原始應用程式會重新架構成 {{site.data.keyword.containerlong_notm}} 上所執行的協同微服務。此動作進一步加速開發及部署時間，並且因相對簡單的實驗而容許更多的創新。它們還利用新版微服務發行創新應用程式，以充分運用市場與業務狀況（即所謂的狀況應用程式及微服務）。

## 支付技術公司可簡化開發人員生產力，並以 4 倍的速度將啟用 AI 功能的工具部署至其夥伴
{: #uc_payment_tech}

Development Exec 的「開發人員」使用可關閉原型的內部部署傳統工具，同時等待硬體採購。
{: shortdesc}

為何要使用 {{site.data.keyword.Bluemix_notm}}：{{site.data.keyword.containerlong_notm}} 使用開放程式碼標準技術來提供啟動運算。公司移至 {{site.data.keyword.containerlong_notm}} 之後，「開發人員」可以存取 DevOps 友善的工具（例如可攜式及輕鬆共用的容器）。

然後，「開發人員」可以輕鬆地進行實驗，並使用開放式工具鏈將變更快速推送至「開發」及「測試」系統。他們的傳統軟體開發工具透過按一下將 AI 雲端服務新增至應用程式時，會出現新的外觀。

重要技術：
* [適合各種 CPU、RAM、儲存空間需求的叢集](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)
* [使用 {{site.data.keyword.watson}} AI 防止詐騙](https://www.ibm.com/cloud/watson-studio)
* [DevOps 原生工具，包括 {{site.data.keyword.contdelivery_full}} 中的開放式工具鏈](https://www.ibm.com/cloud/garage/toolchains/)
* [SDK for Node.js](/docs/runtimes/nodejs?topic=Nodejs-nodejs_runtime#nodejs_runtime)
* [使用 {{site.data.keyword.appid_short_notm}} 可在不需變更應用程式碼的情況下登入](/docs/services/appid?topic=appid-getting-started)

**環境定義：簡化開發人員生產力，並以 4 倍的速度將 AI 工具部署至夥伴**

* 在消費者及企業消費型商務區段中也大幅成長的支付產業中，會經常發生中斷。但是，支付工具的更新會變慢。
* 需要認知特性，以全新且更快的方式將目標設為詐騙交易。
* 透過不斷成長的夥伴及其交易數目，工具資料流量會增加，但需要減少其基礎架構預算，方法是將資源的效率最大化。
* 從無法發行品質軟體到跟上市場需求，其技術債務會逐漸成長，而非緊縮。
* 資本費用預算受到嚴格控制，而且 IT 認為他們沒有預算或人員可以使用其內部系統來建立測試及暫置環境。
* 安全逐漸變成主要考量，而此問題只會增加交付負擔，甚至全部都會造成更久的延遲。

**解決方案**

Development Exec 在動態支付產業面臨許多挑戰。法規、消費者行為、詐騙、競爭者及市場基礎架構都會不斷地發展。快速開發是未來支付世界的重要部分。

其商業模型是向事業夥伴提供支付工具，因此可協助這些財務機構及其他組織提供安全且豐富的數位支付體驗。

它們需要一個解決方案來協助「開發人員」及其事業夥伴：
* 支付工具的前端：費用系統、付款追蹤（包括跨界、法規遵循、生物識別技術、匯款等等）
* 法規特定特性：每個國家/地區都有獨特的法規，因此整體工具集看起來可能很類似，但會顯示國家/地區的特定權益
* 開發人員友善的工具：加速推出特性及錯誤修正程式
* 「詐騙偵測即服務 (FDaaS)」使用 {{site.data.keyword.watson}} AI 早一步防止頻繁且不斷成長的詐騙動作

**解決方案模型**

在可存取後端支付系統之公用雲端中執行的隨需應變運算、DevOps 工具及 AI。實作 CI/CD 處理程序，以大幅縮短交付週期。

技術解決方案：
* {{site.data.keyword.containerlong_notm}}
* {{site.data.keyword.contdelivery_full}}
* IBM Cloud Logging and Monitoring
* {{site.data.keyword.ibmwatson_notm}} for Financial Services
* {{site.data.keyword.appid_full_notm}}

它們是從容器化支付工具 VM 並將它們放入雲端開始。它們的硬體問題一瞬間就會消失。它們能夠輕鬆設計 Kubernetes 叢集，以符合其 CPU、RAM、儲存空間及安全需求。而且當它們的支付工具需要變更時，不需要昂貴且緩慢的硬體採購過程就可以新增或縮減運算。

使用 {{site.data.keyword.containerlong_notm}} 水平調整，其應用程式會隨著不斷成長的夥伴數目進行調整，進而加速成長。{{site.data.keyword.containerlong_notm}} 提供全世界的彈性運算資源，而這些資源是安全的，可完全使用現代運算資源。

加速開發是 Exec 的重要致勝點。使用現代容器，「開發人員」可以使用選擇的語言輕鬆地進行實驗，並將變更推送至「開發」及「測試」系統，以在個別叢集上橫向擴充。這些推送已使用開放式工具鏈及 {{site.data.keyword.contdelivery_full}} 自動化。不再以緩慢且容易出錯的建置處理程序來更新工具。他們可以將漸進式更新提供給其工具（每日或甚至更為頻繁）。

此外，工具的記載及監視（特別是使用 {{site.data.keyword.watson}} AI 的位置）會快速整合至系統。「開發人員」不會浪費時間來建置複雜記載系統，只能疑難排解其即時系統。減少人員配置成本的重要因素是 IBM 管理 Kubernetes，因此「開發人員」可以專注於更佳的支付工具。

安全第一：使用 {{site.data.keyword.containerlong_notm}} 的裸機，敏感支付工具現在已具有熟悉的隔離，但在公用雲端的彈性內。裸機提供「授信運算」，以驗證基礎硬體未遭到竄改。會持續執行漏洞及惡意軟體的掃描。

**步驟 1：提升及轉移至安全運算**
* 管理高度敏感資料的應用程式可以在於「裸機」上執行以進行「授信運算」的 {{site.data.keyword.containerlong_notm}} 上重新管理。「授信運算」可以驗證基礎硬體未遭到竄改。
* 將虛擬機器映像檔移轉至在公用 {{site.data.keyword.Bluemix_notm}} 的 {{site.data.keyword.containerlong_notm}} 中執行的容器映像檔。
* 從該核心中，Vulnerability Advisor 提供已知惡意軟體的映像檔、原則、容器及套件漏洞掃描。
* 專用資料中心/內部部署資本成本會大幅減少，並取代為根據工作負載需求調整的公用電腦模型。
* 利用簡單的 Ingress 註釋，對您的服務及 API 一貫地強制執行原則驅動鑑別。藉由宣告式安全，您可以使用 {{site.data.keyword.appid_short_notm}} 來確保使用者鑑別及記號驗證。

**步驟 2：現有支付系統後端的作業及連線**
* 使用 IBM {{site.data.keyword.SecureGateway}} 來維護內部部署工具系統的安全連線。
* 透過 Kubernetes 提供標準化 DevOps 儀表板及作法。
* 「開發人員」在「開發」及「測試」叢集裡建置及測試應用程式之後，會使用 {{site.data.keyword.contdelivery_full}} 工具鏈，將應用程式部署至全球的 {{site.data.keyword.containerlong_notm}} 叢集。
* {{site.data.keyword.containerlong_notm}} 中的內建 HA 工具會平衡每個地理區域內的工作負載（包括自我修復及負載平衡）。

**步驟 3：分析及防止詐騙**
* 部署 IBM {{site.data.keyword.watson}} for Financial Services，以預防及偵測詐騙。
* 使用工具鏈及 Helm 部署工具，應用程式也會部署至全球的 {{site.data.keyword.containerlong_notm}} 叢集。然後，工作負載及資料會符合地區法規。

**結果**
* 將現有整合型 VM 提升至雲端管理的容器，是容許 Development Exec 節省資本及作業成本的第一個步驟。
* 使用 IBM 所關注的基礎架構管理，開發團隊就不需要每天提供 10 次更新。
* 提供者已平行實作簡單的時間限制反覆運算，以取得現有技術債務的控點。
* 隨著它們處理的交易數目，它們可以透過指數方式來調整其作業。
* 同時，使用 {{site.data.keyword.watson}} 的新詐騙分析已增加偵測及防止速度，讓詐騙平均比該地區減少 4 倍以上。
