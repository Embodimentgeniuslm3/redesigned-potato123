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


# {{site.data.keyword.Bluemix_notm}} 的政府使用案例
{: #cs_uc_gov}

這些使用案例會強調顯示 {{site.data.keyword.containerlong_notm}} 上的工作負載如何自公用雲端受益。這些工作負載與「授信運算」隔離、位於資料自主性的廣域地區中、使用 Watson 機器學習而非全新程式碼，並且連接至內部部署資料庫。
{: shortdesc}

## 地區政府改善與結合公用-專用資料之社群「開發人員」的協同作業及速度
{: #uc_data_mashup}

Open-Government Data Program Executive 需要與社群及私人部門共用公用資料，但會在內部部署整合型系統中鎖定資料。
{: shortdesc}

為何要使用 {{site.data.keyword.Bluemix_notm}}：使用 {{site.data.keyword.containerlong_notm}}，Exec 會提供已結合公用-專用資料的變革價值。同樣地，此服務提供公用雲端平台，以從整合型內部部署應用程式重構並公開微服務。此外，公用雲端還容許政府及公用夥伴關係使用外部雲端服務及協同作業友善的開放程式碼工具。

重要技術：    
* [適合各種 CPU、RAM、儲存空間需求的叢集](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)
* [DevOps 原生工具，包括 {{site.data.keyword.contdelivery_full}} 中的開放式工具鏈](https://www.ibm.com/cloud/garage/toolchains/)
* [透過 {{site.data.keyword.cos_full_notm}} 提供對公用資料的存取](/docs/services/cloud-object-storage?topic=cloud-object-storage-about)
* [隨插即用 IBM Cloud Analytics 服務](https://www.ibm.com/cloud/analytics)

**環境定義：政府改善與結合公用-專用資料之社群「開發人員」的協同作業及速度**
* 「開放式政府」模型是未來模型，但此地區政府機構無法利用其內部部署系統而有大幅躍進。
* 它們想要支援創新，並促進私人部門、居民與公共機構之間的共同開發。
* 政府及私人組織的不同「開發人員」群組，沒有可輕鬆共用 API 及資料的統一開放程式碼平台。
* 政府資料鎖定在內部部署系統中，無法輕鬆進行公用存取。

**解決方案**

開放式政府轉換的建置基礎，必須提供效能、備援、企業永續及安全。隨著創新及共同開發的進行，機構及居民依賴軟體、服務及基礎架構公司來「保護並服務」。

為了打破官僚主義及改變政府與其選舉區的關係，它們轉向開放式標準，以建置共用建立的平台：

* 開放式資料 – 資料儲存空間，讓居民、政府機構及企業自由地存取、共用與加強資料
* 開放式 API – 開發平台，讓 API 由所有社群夥伴提供並重複使用
* 開放式創新 – 一組雲端服務，讓「開發人員」插入創新，而不是手動進行編碼

若要開始，政府使用 {{site.data.keyword.cos_full_notm}} 將其公用資料儲存在雲端。此儲存空間可以免費使用及重複使用、可由任何人共用，而且僅限歸屬及共用。可以先使機密資料為安全的，再將它推送至雲端。除此之外，還會設定存取控制，讓雲端可以涵蓋新的資料儲存空間，而社群可以在其中示範加強型現有可用資料的 POC。

政府公用-私人夥伴關係的下一個步驟是建立 {{site.data.keyword.apiconnect_long}} 中所管理的 API 經濟。社群及企業「開發人員」可以在 API 表單中輕鬆存取資料。其目標是具有公開可用的 REST API、啟用交互作業能力，以及加速應用程式整合。它們使用 IBM {{site.data.keyword.SecureGateway}}，透過內部部署連接回專用資料來源。

最後，根據這些共用 API 的應用程式會在 {{site.data.keyword.containerlong_notm}} 中進行管理，以輕鬆啟動叢集。然後，跨社群、私人部門及政府的「開發人員」可以輕鬆地共同建立應用程式。簡言之，「開發人員」需要專注在進行程式編碼，而不是管理基礎架構。因此，他們選擇 {{site.data.keyword.containerlong_notm}}，因為 IBM 簡化了基礎架構管理：
* 管理 Kubernetes 主節點、IaaS 及作業元件（例如 Ingress 及儲存空間）
* 監視工作者節點的性能及回復
* 提供廣域運算，因此「開發人員」不需要負責所需工作負載及資料所在之全球地區中的基礎架構

將運算工作負載移至 {{site.data.keyword.Bluemix_notm}} 並不足夠。政府需要逐步完成處理程序及方法轉換。採用 IBM Garage Method 的作法，提供者即可實作敏捷及反覆運算式交付處理程序，以支援現代 DevOps 作法（例如持續整合及交付 (CI/CD)）。

許多 CI/CD 處理程序本身都是透過雲端中的 {{site.data.keyword.contdelivery_full}} 而自動化。提供者可以定義工作流程工具鏈來準備容器映像檔、檢查漏洞，以及將其部署至 Kubernetes 叢集。

**解決方案模型**

隨需應變運算、儲存空間及 API 工具是在公用雲端中執行，並且可安全地存取內部部署資料來源，或從內部部署資料來源進行存取。

技術解決方案：
* {{site.data.keyword.containerlong_notm}}
* {{site.data.keyword.cos_full_notm}} 及 {{site.data.keyword.cloudant}}
* {{site.data.keyword.apiconnect_long}}
* IBM {{site.data.keyword.SecureGateway}}
* {{site.data.keyword.contdelivery_full}}

**步驟 1：將資料儲存在雲端**
* {{site.data.keyword.cos_full_notm}} 提供公用雲端上所有項目都可以存取的歷程資料儲存空間。
* 使用 {{site.data.keyword.cloudant}} 與開發人員提供的金鑰搭配，以快取雲端中的資料。
* 使用 IBM {{site.data.keyword.SecureGateway}} 來維護現有內部部署資料庫的安全連線。

**步驟 2：使用 API 提供存取資料**
* 使用 API 經濟平台的 {{site.data.keyword.apiconnect_long}}。API 容許公用及私人部門將資料結合至其應用程式。
* 建立由 API 驅動之公用-私人應用程式的叢集。
* 根據應用程式的功能範圍及其相依關係，將應用程式建構成在 {{site.data.keyword.containerlong_notm}} 內執行的一組共同微服務。
* 將應用程式部署至 {{site.data.keyword.containerlong_notm}} 中執行的容器。{{site.data.keyword.containerlong_notm}} 中的內建 HA 工具會平衡工作負載（包括自我修復及負載平衡）。
* 透過 Kubernetes 提供標準化 DevOps 儀表板（所有類型「開發人員」熟悉的開放程式碼工具）。

**步驟 3：使用 IBM Garage 及雲端服務創新**
* 採用 IBM Garage Method 的敏捷及反覆運算式開發作法，以啟用頻繁發行特性、修補程式及修正程式，且不需要關閉。
* 不論開發人員位在公用還是私人部門，{{site.data.keyword.contdelivery_full}} 都可以協助他們使用可自訂且可共用的範本，快速佈建整合式工具鏈。
* 「開發人員」在其「開發」及「測試」叢集裡建置及測試應用程式之後，會使用 {{site.data.keyword.contdelivery_full}} 工具鏈，將應用程式部署至正式作業叢集。
* 使用 {{site.data.keyword.Bluemix_notm}} 型錄中提供的 Watson AI、機器學習及深度學習工具，「開發人員」可專注在網域問題。ML 邏輯透過服務連結進入應用程式，而不是自訂唯一的 ML 程式碼。

**結果**
* 通常，緩慢的公用-私人夥伴關係現在會在數週（而非數個月）內快速啟動應用程式。這些開發夥伴關係現在每週最多提供 10 次的特性及錯誤修正程式。
* 所有參與者都使用已知開放程式碼工具（例如 Kubernetes）時，會加速開發。長時間的學習曲線不再是阻礙。
* 將活動、資訊及方案中的透明度提供給居民及私人部門。而且，居民會整合到政府處理程序、服務及支援。
* 公用-私人夥伴關係可克服艱巨任務（例如 Zika 病毒追蹤、智慧型配電、犯罪統計資料分析，及大學「新領階級」教育）。

## 大型公共港口可保護連接公用與私人組織之港口資料及出貨貨單的交換
{: #uc_port}

私人貨運公司及政府所運作港口的 IT Exec 需要連接、提供可見性，以及安全地交換港口資訊。但是，沒有任何統一的系統可以連接公共港口資訊及私人出貨貨單。
{: shortdesc}

為何要使用 {{site.data.keyword.Bluemix_notm}}：{{site.data.keyword.containerlong_notm}} 容許政府及公用夥伴關係使用外部雲端服務及協同作業友善的開放程式碼工具。這些容器提供可共用的平台，其中，港口及貨運公司都保證共用資訊是在一個安全的平台上進行管理。而該平台會隨著從小型「開發-測試」系統到正式作業大小系統而進行調整。透過自動化建置、測試及部署，開放式工具鏈可進一步加速開發。

重要技術：    
* [適合各種 CPU、RAM、儲存空間需求的叢集](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)
* [容器安全及隔離](/docs/containers?topic=containers-security#security)
* [DevOps 原生工具，包括 {{site.data.keyword.contdelivery_full}} 中的開放式工具鏈](https://www.ibm.com/cloud/garage/toolchains/)
* [SDK for Node.js](/docs/runtimes/nodejs?topic=Nodejs-nodejs_runtime#nodejs_runtime)

**環境定義：港口可保護連接公用與私人組織之港口資料及出貨貨單的交換。**

* 政府及貨運公司的不同「開發人員」群組沒有可分工合作的統一平台，這會減緩更新及特性部署的速度。
* 「開發人員」散佈在全球及組織界限，這表示開放程式碼及 PaaS 是最佳選項。
* 安全是主要考量，而此問題會增加影響軟體特性及更新（特別是在應用程式處於正式作業之後）的協同作業負擔。
* 即時資料表示全球系統必須高度可用，才能減少傳輸作業的落差。船隻碼頭的時間表受高度管制，因此，在某些情況下不具彈性。Web 用量不斷成長，因此不穩定可能會導致不好的使用者體驗。

**解決方案**

港口及貨運公司共同開發統一的交易系統，以電子方式提交貨品及船隻出入港許可的合規性相關資訊一次，而不是提交給多個機構。貨單及海關應用程式可以快速共用特定貨運的內容，並確保所有文書工作都是以電子方式傳送並由港口的機構進行處理。

因此，它們建立夥伴關係，以專用於交易系統的解決方案：
* 宣告 - 此應用程式可在出貨貨單中採用，以及數位處理一般海關文書工作，並標示「不符合政策項目」來進行調查及強制執行
* 價目表 – 此應用程式可計算價目表、透過電子方式向貨主提交費用，並接收數位付款
* 法規 – 彈性且可配置的應用程式，可將不斷變更且影響進口、出口及價目表處理的原則及法規提供給前兩個應用程式

「開發人員」會從使用 {{site.data.keyword.containerlong_notm}} 以在容器中部署其應用程式開始。他們已建立共用「開發」環境的叢集，讓全球「開發人員」能夠快速協同部署應用程式改善。容器容許每個開發團隊使用其選擇的語言。

安全第一：IT Execs 會選擇裸機的「授信運算」，以管理叢集。使用 {{site.data.keyword.containerlong_notm}} 的裸機，敏感海關工作負載現在已具有熟悉的隔離，但在公用雲端的彈性內。裸機提供「授信運算」，以驗證基礎硬體未遭到竄改。

因為貨運公司也想要與其他港口合作，所以應用程式安全十分重要。出貨貨單及海關資訊高度機密。從該安全核心，Vulnerability Advisor 提供下列掃描：
* 映像檔漏洞掃描
* 根據 ISO 27k 的原則掃描
* 即時容器掃描
* 已知惡意軟體的套件掃描

同時，{{site.data.keyword.iamlong}} 有助於控制誰具有資源的哪種存取層次。

「開發人員」使用現有工具以專注在網域問題：「開發人員」會將 {{site.data.keyword.Bluemix_notm}} 服務連結至叢集，以讓其進入應用程式，而不是撰寫唯一記載及監視程式碼。「開發人員」也不需要處理基礎架構管理作業，因為 IBM 會關注 Kubernetes 以及基礎架構更新、安全和其他項目。

**解決方案模型**

隨需應變運算、儲存空間及 Node 入門範本套件是視需要在可安全存取全球出貨資料的公用雲端中執行。叢集裡的運算可防竄改，並與裸機隔離。  

技術解決方案：
* 使用授信運算的 {{site.data.keyword.containerlong_notm}}
* {{site.data.keyword.openwhisk}}
* {{site.data.keyword.cloudant}}
* IBM {{site.data.keyword.SecureGateway}}

**步驟 1：使用微服務來容器化應用程式**
* 使用 IBM 的 Node.js 入門範本套件來快速開始開發。
* 根據應用程式的功能範圍及其相依關係，將應用程式建構成在 {{site.data.keyword.containerlong_notm}} 內執行的一組共同微服務。
* 將貨單及出貨應用程式部署至 {{site.data.keyword.containerlong_notm}} 中所執行的容器。
* 透過 Kubernetes 提供標準化 DevOps 儀表板。
* 使用 IBM {{site.data.keyword.SecureGateway}} 來維護現有內部部署資料庫的安全連線。

**步驟 2：確定廣域可用性**
* 「開發人員」在其「開發」及「測試」叢集裡部署應用程式之後，會使用 {{site.data.keyword.contdelivery_full}} 工具鏈及 Helm，將國家/地區特定的應用程式部署至全球叢集。
* 然後，工作負載及資料可符合地區法規。
* {{site.data.keyword.containerlong_notm}} 中的內建 HA 工具會平衡每個地理區域內的工作負載（包括自我修復及負載平衡）。

**步驟 3：資料共用**
* {{site.data.keyword.cloudant}} 是一個現代 NoSQL 資料庫，適用於從金鑰值到複雜文件導向資料儲存空間及查詢的資料驅動使用案例範圍。
* 為了將查詢最小化為地區資料庫，會使用 {{site.data.keyword.cloudant}} 跨應用程式來快取使用者的階段作業資料。
* 此配置可以改善 {{site.data.keyword.containershort}} 上跨應用程式的前端應用程式可用性及效能。
* {{site.data.keyword.containerlong_notm}} 中的工作者節點應用程式分析內部部署資料並將結果儲存至 {{site.data.keyword.cloudant}} 時，{{site.data.keyword.openwhisk}} 會反應變更，並自動消毒送入資訊來源上的資料。
* 同樣地，可以透過資料上傳觸發某個地區中的出貨通知，讓所有下游消費者可以存取新資料。

**結果**
* 使用 IBM 入門範本套件、{{site.data.keyword.containerlong_notm}} 及 {{site.data.keyword.contdelivery_full}} 工具，全球「開發人員」可以跨組織及政府合作。他們透過熟悉且可交互作業的工具，協同開發海關應用程式。
* 微服務可以大幅減少修補程式、錯誤修正程式及新增特性的交付時間。起始開發十分快速，而且每週會頻繁更新 10 次。
* 貨運客戶及政府人員可以存取貨單資料，並且可以在符合當地法規的情況下共用海關資料。
* 貨運公司的好處是改善供應鏈中的物流管理：減少成本及縮短出入港許可時間。
* 99% 是數位宣告，而 90% 的進口是在沒有人為介入的情況下進行處理。
