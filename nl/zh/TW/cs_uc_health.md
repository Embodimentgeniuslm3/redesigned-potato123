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


# {{site.data.keyword.cloud_notm}} 的醫療保健使用案例
{: #cs_uc_health}

這些使用案例會強調顯示 {{site.data.keyword.containerlong_notm}} 上的工作負載如何自公用雲端受益。它們具有隔離裸機的安全運算能力、可以輕鬆啟動叢集以更快速地開發、能夠從虛擬機器移轉，以及在雲端資料庫中共用資料。
{: shortdesc}

## 醫療保健提供者會將工作負載從效率不佳的 VM 移轉至 Ops 友善的容器，以用於報告及病患系統
{: #uc_migrate}

醫療保健提供者的 IT Exec 具有內部部署商業報告及病患系統。這些系統經歷慢速增強週期，進而造成停滯的病患服務層次。
{: shortdesc}

為何要使用 {{site.data.keyword.cloud_notm}}：若要改善病患服務，提供者會尋找 {{site.data.keyword.containerlong_notm}} 及 {{site.data.keyword.contdelivery_full}} 以減少 IT 費用並加速開發，而這些全都是在安全平台上執行。提供者的高使用量 SaaS 系統（同時保留病患記錄系統及商業報告應用程式）需要經常更新。此外，內部部署環境會阻礙敏捷開發。提供者也想要抵消不斷增加的人力成本以及減少預算。

重要技術：
* [適合各種 CPU、RAM、儲存空間需求的叢集](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)
* [水平調整](/docs/containers?topic=containers-app#highly_available_apps)
* [容器安全及隔離](/docs/containers?topic=containers-security#security)
* [DevOps 原生工具，包括 {{site.data.keyword.contdelivery_full}} 中的開放式工具鏈](https://www.ibm.com/cloud/garage/toolchains/)
* [SDK for Node.js](/docs/runtimes/nodejs?topic=Nodejs-nodejs_runtime#nodejs_runtime)
* [使用 {{site.data.keyword.appid_short_notm}} 可在不需變更應用程式碼的情況下登入](/docs/services/appid?topic=appid-getting-started)

它們是從容器化其 SaaS 系統並將它們放入雲端開始。從這個第一個步驟，它們已從專用資料中心的過度建置硬體移到可降低 IT 作業、維護及能源的可自訂運算。為了管理 SaaS 系統，它們輕鬆地設計 Kubernetes 叢集，以符合其 CPU、RAM 及儲存空間需求。減少人員成本的另一個因素是 IBM 管理 Kubernetes，因此提供者可以專注於提供更佳的客戶服務。

加速開發是 IT Exec 的重要致勝點。隨著移至公用雲端，「開發人員」可以使用 Node.js SDK 輕鬆地進行實驗，並將變更推送至「開發」及「測試」系統，以在個別叢集上橫向擴充。這些推送已使用開放式工具鏈及 {{site.data.keyword.contdelivery_full}} 自動化。不再以緩慢且容易出錯的建置處理程序來更新 SaaS 系統。「開發人員」可以將漸進式更新提供給其使用者（每日或甚至更為頻繁）。此外，SaaS 系統的記載及監視（特別是病患前端及後端報告如何互動）會快速整合至系統。「開發人員」不會浪費時間來建置複雜記載系統，只能疑難排解即時系統。

安全第一：使用 {{site.data.keyword.containerlong_notm}} 的裸機，敏感病患工作負載現在已具有熟悉的隔離，但在公用雲端的彈性內。裸機提供「授信運算」，以驗證基礎硬體未遭到竄改。從該核心，Vulnerability Advisor 提供以下掃描：
* 映像檔漏洞掃描
* 根據 ISO 27k 的原則掃描
* 即時容器掃描
* 已知惡意軟體的套件掃描

安全的病患資料可以讓患者更愉悅。

**環境定義：醫療保健提供者的工作負載移轉**

* 技術債務（與長期發行週期連結）將會阻礙提供者的重要商業病患管理及報告系統。
* 其後台及前台自訂應用程式使用整合型虛擬機器映像檔在內部部署提供。
* 需要檢修其處理程序、方法及工具，但不知道從哪裡開始。
* 從無法發行品質軟體到跟上市場需求，其技術債務會逐漸成長，而非緊縮。
* 安全是主要考量，而此問題會增加交付負擔，甚至造成更久的延遲。
* 資本費用預算受到嚴格控制，而且 IT 認為他們沒有預算或人員可以使用其內部系統來建立所需的測試及暫置環境。

**解決方案模型**

隨需應變運算、儲存空間及 I/O 服務是在可安全存取內部部署企業資產的公用雲端中執行。實作 CI/CD 處理程序及其他 IBM Garage Method 部分，以大幅縮短交付週期。

**步驟 1：保護運算平台的安全**
* 管理高度敏感病患資料的應用程式可以在於「裸機」上執行以進行「授信運算」的 {{site.data.keyword.containerlong_notm}} 上重新管理。
* 「授信運算」可以驗證基礎硬體未遭到竄改。
* 從該核心，Vulnerability Advisor 提供已知惡意軟體的映像檔、原則、容器及套件掃描漏洞掃描。
* 利用簡單的 Ingress 註釋，對您的服務及 API 一貫地強制執行原則驅動鑑別。藉由宣告式安全，您可以使用 {{site.data.keyword.appid_short_notm}} 來確保使用者鑑別及記號驗證。

**步驟 2：提升及轉移**
* 將虛擬機器映像檔移轉至在公用雲端的 {{site.data.keyword.containerlong_notm}} 中執行的容器映像檔。
* 透過 Kubernetes 提供標準化 DevOps 儀表板及作法。
* 針對批次及其他不常執行的後台工作負載，啟用隨需應變運算調整。
* 使用 {{site.data.keyword.SecureGatewayfull}} 來維護內部部署 DBMS 的安全連線。
* 專用資料中心/內部部署資本成本會大幅減少，並取代為根據工作負載需求調整的公用電腦模型。

**步驟 3：微服務及 Garage Method**
* 將應用程式重新架構成一組共同微服務。這個組合在 {{site.data.keyword.containerlong_notm}} 內執行，而其根據具有最多品質問題之應用程式的功能範圍。
* 使用 {{site.data.keyword.cloudant}} 與客戶提供的金鑰搭配，以在雲端中快取資料。
* 採用持續整合及交付 (CI/CD) 作法，讓「開發人員」視需要依自己的排程來設定版本及發行微服務。{{site.data.keyword.contdelivery_full}} 提供 CI/CD 處理程序的工作流程工具鏈，以及容器映像檔的映像檔建立及漏洞掃描。
* 採用 IBM Garage Method 的敏捷及反覆運算式開發作法，以啟用頻繁發行新的功能、修補程式及修正程式，且不需要關閉。

**技術解決方案**
* {{site.data.keyword.containerlong_notm}}
* {{site.data.keyword.cloudant}}
* {{site.data.keyword.SecureGatewayfull}}
* {{site.data.keyword.appid_short_notm}}

對於最敏感的工作負載，可以在「裸機」的 {{site.data.keyword.containerlong_notm}} 中管理叢集。它提供授信運算平台，以自動掃描硬體及運行環境程式碼來找出漏洞。使用業界標準容器技術，即可一開始就快速在 {{site.data.keyword.containerlong_notm}} 上重新管理應用程式，而不需要變更主要架構。這項變更提供可調整性的直接好處。

它們可以使用已定義的規則及自動化 Kubernetes 編排器，來抄寫及調整應用程式。{{site.data.keyword.containerlong_notm}} 提供可擴充的運算資源，以及關聯的 DevOps 儀表板，依需求建立、調整與關閉應用程式及服務。使用 Kubernetes 的部署及運行環境物件，提供者即可可靠地監視及管理應用程式升級。

{{site.data.keyword.SecureGatewayfull}} 用來建立內部部署資料庫的安全管線，以及重新管理以在 {{site.data.keyword.containerlong_notm}} 中執行之應用程式的文件。

{{site.data.keyword.cloudant}} 是一個現代 NoSQL 資料庫，適用於從金鑰值到複雜文件導向資料儲存空間及查詢的資料驅動使用案例範圍。為了將查詢最小化為後台 RDBMS，會使用 {{site.data.keyword.cloudant}} 跨應用程式來快取使用者的階段作業資料。這些選項可以改善 {{site.data.keyword.containerlong_notm}} 上跨應用程式的前端應用程式可用性及效能。

將運算工作負載移至 {{site.data.keyword.cloud_notm}} 並不足夠。提供者需要逐步完成處理程序及方法轉換。採用 IBM Garage Method 的作法，提供者即可實作敏捷及反覆運算式交付處理程序，以支援現代 DevOps 作法（例如 CI/CD）。

許多 CI/CD 處理程序本身都是透過雲端中的 IBM Continuous Delivery 服務而自動化。提供者可以定義工作流程工具鏈來準備容器映像檔、檢查漏洞，以及將其部署至 Kubernetes 叢集。

**結果**
* 將現有整合型 VM 提升至雲端管理的容器，是容許提供者節省資本成本以及開始學習現代 DevOps 作法的第一個步驟。
* 將整合型應用程式重新架構成一組精細微服務，可大幅減少修補程式、錯誤修正程式及新增特性的交付時間。
* 提供者已平行實作簡單的時間限制反覆運算，以取得現有技術債務的控點。

## 非營利研究可安全地管理敏感資料，同時擴展與夥伴的研究
{: #uc_research}

非營利疾病研究 Development Exec 的學術及產業研究人員並無法輕鬆地共用研究資料。相反地，他們的工作因為地區法規遵循及集中化資料庫而隔離於全球的各區塊中。
{: shortdesc}

為何要使用 {{site.data.keyword.cloud_notm}}：{{site.data.keyword.containerlong_notm}} 提供安全運算，可以在開放式平台上管理敏感及高效能資料處理。該廣域平台會在附近的地區進行管理。因此，它與當地法規相關聯，以提高病患及研究人員在本端保護其資料的信心，並在改善健康成果方面有所不同。

重要技術：
* [智慧型排程會視需要放置工作負載](/docs/containers?topic=containers-regions-and-zones#regions-and-zones)
* [{{site.data.keyword.cloudant}} 可以跨應用程式持續保存及同步處理資料](/docs/services/Cloudant?topic=cloudant-getting-started#getting-started)
* [工作負載的漏洞掃描及隔離](/docs/services/Registry?topic=va-va_index#va_index)
* [DevOps 原生工具，包括 {{site.data.keyword.contdelivery_full}} 中的開放式工具鏈](https://www.ibm.com/cloud/garage/toolchains/)
* [{{site.data.keyword.openwhisk}} 會將資料消毒，並通知研究人員資料結構發生變更](/docs/openwhisk?topic=cloud-functions-openwhisk_cloudant#openwhisk_cloudant)

**環境定義：安全地管理及共用非營利研究的疾病資料**

* 各種機構的不同研究人員群組沒有一致的方式來共用資料，因而讓協同作業變慢。
* 安全考量會增加協同作業負擔，甚至讓共用研究變少。
* 「開發人員」及「研究人員」散佈在全球及組織界限，讓 PaaS 及 SaaS 成為每個使用者群組的最佳選項。
* 健康法規的地區差異要求部分資料及資料處理作業仍在該地區內進行。

**解決方案**

非營利研究想要聚集全球的癌症研究資料。因此，它們建立一個部門，以專用於其研究人員的解決方案：
* INGEST - 汲取研究資料的應用程式。研究人員現今使用試算表、文件、商品，以及專有或自有資料庫，來記錄研究結果。此狀況不可能隨著非營利研究嘗試集中化資料分析而變更。
* ANONYMIZE - 以匿名方式處理資料的應用程式。必須移除 SPI，才能符合地區健康法規。
* ANALYZE - 分析資料的應用程式。基本模式是以一般格式儲存資料，然後使用 AI 及機器學習 (ML) 技術、簡單迴歸等等來查詢及處理資料。

研究人員需要與地區叢集產生連結，而且應用程式會汲取、轉換並以匿名方式處理資料：
1. 將匿名化資料同步到地區叢集，或將它們傳送至集中化資料儲存庫
2. 處理資料，方法是在提供 GPU 的裸機工作者節點上使用 ML（例如 PyTorch）。

**INGEST** {{site.data.keyword.cloudant}} 用於每個地區叢集，以儲存研究人員的豐富資料文件，而且可以視需要進行查詢及處理。{{site.data.keyword.cloudant}} 會加密靜止及傳輸中的資料，以符合地區資料隱私法。

{{site.data.keyword.openwhisk}} 用來建立處理功能，以汲取研究資料並將它們儲存為 {{site.data.keyword.cloudant}} 中的結構化資料文件。{{site.data.keyword.SecureGatewayfull}} 提供簡單的方法，讓 {{site.data.keyword.openwhisk}} 以安全又可靠的方式存取內部部署資料。

地區叢集裡的 Web 應用程式是使用 nodeJS 開發的，適用於結果、綱目定義及研究組織連結的手動資料輸入。IBM Key Protect 協助安全地存取 {{site.data.keyword.cloudant}} 資料，而且 IBM Vulnerability Advisor 會掃描應用程式容器及映像檔以尋找安全惡意探索。

**ANONYMIZE** 只要新的資料文件儲存至 {{site.data.keyword.cloudant}}，就會觸發事件，而且 Cloud Function 會以匿名方式處理資料並移除資料文件中的 SPI。這些匿名化資料文件會與已汲取的「原始」資料分開儲存，而且是跨地區共用以進行分析的唯一文件。

**ANALYZE** 機器學習架構是高度運算密集，因此非營利研究會設定裸機工作者節點的廣域處理叢集。與此廣域處理叢集相關聯的是匿名化資料的聚集 {{site.data.keyword.cloudant}} 資料庫。Cron 工作會定期觸發 Cloud Function，將匿名化資料文件從地區中心推送至廣域處理叢集的 {{site.data.keyword.cloudant}} 實例。

運算叢集會執行 PyTorch ML 架構，而機器學習應用程式是以 Python 撰寫，用來分析聚集資料。除了 ML 應用程式之外，群體群組中的研究人員還會開發其可在廣域叢集裡發佈及執行的專屬應用程式。

非營利研究也會提供在廣域叢集之非裸機節點上執行的應用程式。應用程式會檢視與擷取聚集資料及 ML 應用程式輸出。這些應用程式可以透過公用端點（由「API 閘道」保護）存取。然後，各地的研究人員及資料分析師都可以下載資料集，並執行他們自己的分析。

**在 {{site.data.keyword.containerlong_notm}} 上管理研究工作負載**

「開發人員」會從使用 {{site.data.keyword.containerlong_notm}} 以在容器中部署其研究共用 SaaS 應用程式開始。他們已建立「開發」環境的叢集，讓全球「開發人員」能夠快速協同部署應用程式改善。

安全第一：Development Exec 會選擇裸機的「授信運算」，以管理研究叢集。使用 {{site.data.keyword.containerlong_notm}} 的裸機，敏感研究工作負載現在已具有熟悉的隔離，但在公用雲端的彈性內。裸機提供「授信運算」，以驗證基礎硬體未遭到竄改。因為這項非營利研究與製藥公司也具有夥伴關係，所以應用程式安全十分重要。競爭是殘酷的，而且可能會有企業間諜。從該安全核心，Vulnerability Advisor 提供以下掃描：
* 映像檔漏洞掃描
* 根據 ISO 27k 的原則掃描
* 即時容器掃描
* 已知惡意軟體的套件掃描

安全的研究應用程式會產生不斷增加的臨床試驗參與。

為了達到廣域可用性，會在全球的數個資料中心部署「開發」、「測試」及「正式作業」系統。基於 HA，它們使用多個地理區域中的叢集組合以及多區域叢集。它們可以輕鬆地將研究應用程式部署至「法蘭克福」叢集，以符合當地歐洲法規。它們也會在「美國」叢集內部署應用程式，以確保本端可用性及回復。它們也會將研究工作負載散佈到法蘭克福的多區域叢集，以確保歐洲應用程式可供使用，同時有效率地平衡工作負載。因為研究人員是使用研究共用應用程式來上傳敏感資料，所以應用程式的叢集會在套用較嚴格規範的地區內進行管理。

「開發人員」使用現有工具以專注在網域問題：ML 邏輯會將 {{site.data.keyword.cloud_notm}} 服務連結至叢集，以進入應用程式，而不是撰寫唯一 ML 程式碼。「開發人員」也不需要處理基礎架構管理作業，因為 IBM 會關注 Kubernetes 以及基礎架構更新、安全和其他項目。

**解決方案**

隨需應變運算、儲存空間及 Node 入門範本套件保證是在可安全存取全球研究資料的公用雲端中執行。叢集裡的運算可防竄改，並與裸機隔離。

技術解決方案：
* 使用授信運算的 {{site.data.keyword.containerlong_notm}}
* {{site.data.keyword.openwhisk}}
* {{site.data.keyword.cloudant}}
* {{site.data.keyword.SecureGatewayfull}}

**步驟 1：使用微服務來容器化應用程式**
* 使用 IBM 的 Node.js 入門範本套件來快速開始開發。
* 根據應用程式的功能範圍及其相依關係，將應用程式建構成 {{site.data.keyword.containerlong_notm}} 內的一組共同微服務。
* 將研究應用程式部署至 {{site.data.keyword.containerlong_notm}} 中的容器。
* 透過 Kubernetes 提供標準化 DevOps 儀表板。
* 針對批次及其他不常執行的研究工作負載，啟用隨需應變運算調整。
* 使用 {{site.data.keyword.SecureGatewayfull}} 來維護現有內部部署資料庫的安全連線。

**步驟 2：使用安全及高效能運算**
* 需要較高效能運算的 ML 應用程式是在「裸機」的 {{site.data.keyword.containerlong_notm}} 上進行管理。此 ML 叢集已集中化，因此每個地區叢集都沒有裸機工作者節點的費用；Kubernetes 部署也較為容易。
* 處理高度敏感臨床資料的應用程式可以在「裸機」上進行「授信運算」的 {{site.data.keyword.containerlong_notm}} 上進行管理。
* 「授信運算」可以驗證基礎硬體未遭到竄改。從該核心，Vulnerability Advisor 提供已知惡意軟體的映像檔、原則、容器及套件掃描漏洞掃描。

**步驟 3：確定廣域可用性**
* 「開發人員」在其「開發」及「測試」叢集裡建置及測試應用程式之後，會使用 IBM CI/CD 工具鏈，將應用程式部署至全球叢集。
* {{site.data.keyword.containerlong_notm}} 中的內建 HA 工具會平衡每個地理區域內的工作負載（包括自我修復及負載平衡）。
* 使用工具鏈及 Helm 部署工具，應用程式也會部署至全球叢集，因此工作負載及資料符合地區法規。

**步驟 4：資料共用**
* {{site.data.keyword.cloudant}} 是一個現代 NoSQL 資料庫，適用於從金鑰值到複雜文件導向資料儲存空間及查詢的資料驅動使用案例範圍。
* 為了將查詢最小化為地區資料庫，會使用 {{site.data.keyword.cloudant}} 跨應用程式來快取使用者的階段作業資料。
* 此選項可以改善 {{site.data.keyword.containerlong_notm}} 上跨應用程式的前端應用程式可用性及效能。
* {{site.data.keyword.containerlong_notm}} 中的工作者應用程式分析內部部署資料並將結果儲存至 {{site.data.keyword.cloudant}} 時，{{site.data.keyword.openwhisk}} 會反應變更，並自動消毒送入資訊來源上的資料。
* 同樣地，可以透過資料上傳觸發某個地區中的研究突破通知，讓所有研究人員能夠充分運用新資料。

**結果**
* 使用入門範本套件、{{site.data.keyword.containerlong_notm}} 及 IBM CI/CD 工具，全球「開發人員」可以在各機構運作，並使用熟悉且可交互作業的工具來協同開發研究應用程式。
* 微服務可以大幅減少修補程式、錯誤修正程式及新增特性的交付時間。起始開發十分快速，並且會頻繁更新。
* 研究人員可以存取臨床資料，並且可以在符合當地法規的情況下共用臨床資料。
* 參與疾病研究的病患相信其資料是安全的，並且在與大型研究團隊共用時有所作用。
