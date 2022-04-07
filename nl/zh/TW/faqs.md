---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-05"

keywords: kubernetes, iks, compliance, security standards

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
{:download: .download}
{:preview: .preview}
{:faq: data-hd-content-type='faq'}


# 常見問題
{: #faqs}

## 何謂 Kubernetes？
{: #kubernetes}
{: faq}

Kubernetes 是一個開放程式碼平台，用於管理跨多個主機的容器化工作負載和服務，並提供管理工具來部署、自動執行、監視及調整容器化應用程式，只需最少或無需人為介入。構成微服務的所有容器都會分組成 Pod，這是一個可確保輕鬆管理及探索的邏輯單元。這些 Pod 會在 Kubernetes 叢集裡所管理的運算主機上執行，而此叢集是可攜式、可延伸，並且在失敗時可自我修復。
{: shortdesc}

如需 Kubernetes 的相關資訊，請參閱 [Kubernetes 文件 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/home/?path=users&persona=app-developer&level=foundational)。

## 「IBM Cloud Kubernetes 服務」如何運作？
{: #kubernetes_service}
{: faq}

使用 {{site.data.keyword.containerlong_notm}}，您可以建立自己的 Kubernetes 叢集，以在 {{site.data.keyword.Bluemix_notm}} 上部署及管理容器化應用程式。您的容器化應用程式是在稱為工作者節點的 IBM Cloud 基礎架構 (SoftLayer) 運算主機上進行管理。您可以選擇將運算主機佈建為具有共用或專用資源的[虛擬機器](/docs/containers?topic=containers-planning_worker_nodes#vm)，也可以佈建為[裸機機器](/docs/containers?topic=containers-planning_worker_nodes#bm)，其可最佳化以供 GPU 與軟體定義的儲存空間 (SDS) 使用。您的工作者節點是由 IBM 配置、監視及管理的高可用性 Kubernetes 主節點來控制的。您可以使用 {{site.data.keyword.containerlong_notm}} API 或 CLI 來處理叢集基礎架構資源，以及使用 Kubernetes API 或 CLI 來管理您的部署與服務。

如需如何設定叢集資源的相關資訊，請參閱[服務架構](/docs/containers?topic=containers-ibm-cloud-kubernetes-service-technology#architecture)。若要尋找功能和好處的清單，請參閱[為何要用 {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-cs_ov#cs_ov)。

## 為何應該使用 IBM Cloud Kubernetes Service？
{: #faq_benefits}
{: faq}

{{site.data.keyword.containerlong_notm}} 是一個受管理的 Kubernetes 供應項目，可提供功能強大的工具、直覺式使用者體驗及內建安全，以快速遞送各種應用程式，您可以將其連結至與 IBM Watson®、AI、IoT、DevOps、安全及資料分析相關的雲端服務。作為認證的 Kubernetes 提供者，{{site.data.keyword.containerlong_notm}} 提供智慧型排程、自我修復、水平調整、服務探索與負載平衡、自動化推出與回復，以及密碼與配置管理。該服務還具有依循簡化叢集管理、容器安全及隔離原則的進階功能、設計專屬叢集的能力，以及用於部署一致性的整合作業工具。

如需功能和好處的詳細概觀，請參閱[為何要用 {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-cs_ov#cs_ov)。

## 服務是否隨附受管理的 Kubernetes 主節點和工作者節點？
{: #managed_master_worker}
{: faq}

{{site.data.keyword.containerlong_notm}} 中的每個 Kubernetes 叢集都是由 IBM 透過 IBM 擁有的 {{site.data.keyword.Bluemix_notm}} 基礎架構帳戶所管理的專用 Kubernetes 主節點予以控制。IBM Site Reliability Engineer (SRE) 會持續監視 Kubernetes 主節點（包括所有主節點元件、運算、網路及儲存空間資源）。SRE 會套用最新的安全標準、偵測並重新修補惡意活動，並且努力確保 {{site.data.keyword.containerlong_notm}} 的可靠性及可用性。當您佈建叢集時自動安裝的附加程式（例如 Fluentd 以進行記載）是由 IBM 自動更新。不過，您可以選擇停用部分附加程式的自動更新，並從主節點及工作者節點分別進行手動更新。如需相關資訊，請參閱[更新叢集附加程式](/docs/containers?topic=containers-update#addons)。

Kubernetes 會定期發行[主要、次要或修補程式更新](/docs/containers?topic=containers-cs_versions#version_types)。這些更新會影響 Kubernetes API 伺服器版本或 Kubernetes 主節點的其他元件。IBM 會自動更新修補程式版本，但您必須更新主節點的主要及次要版本。
如需相關資訊，請參閱[更新 Kubernetes 主節點](/docs/containers?topic=containers-update#master)。

標準叢集裡的工作者節點會佈建至您的 {{site.data.keyword.Bluemix_notm}} 基礎架構帳戶。工作者節點為您的帳戶所專用，而且您負責向工作者節點要求及時更新，以確保工作者節點 OS 和 {{site.data.keyword.containerlong_notm}} 元件套用最新的安全更新及修補程式。IBM Site Reliability Engineer (SRE) 會提供安全更新項目與修補程式，也會持續監視工作者節點上所安裝的 Linux 映像檔，以偵測漏洞及安全規範問題。如需相關資訊，請參閱[更新工作者節點](/docs/containers?topic=containers-update#worker_node)。

## Kubernetes 主節點與工作者節點具有高可用性嗎？
{: #faq_ha}
{: faq}

{{site.data.keyword.containerlong_notm}} 架構及基礎架構的設計旨在確保可靠性、縮短處理延遲時間及服務執行時間最大化。依預設，{{site.data.keyword.containerlong_notm}} 中的每個叢集都是使用多個 Kubernetes 主節點實例來設定，以確保叢集資源的可用性及可存取性，即使 Kubernetes 主節點的一個以上實例無法使用。

您可以讓叢集具有更高可用性，並藉由將工作負載分散在某個地區的多個區域中的多個工作者節點之中，來保護應用程式免於關閉。此設定稱為[多區域叢集](/docs/containers?topic=containers-ha_clusters#multizone)，並確保您的應用程式可存取，即使工作者節點或整個區域無法使用。

若要防範整個地區失敗，請建立[多個叢集，並將它們分散在各 {{site.data.keyword.containerlong_notm}} 地區](/docs/containers?topic=containers-ha_clusters#multiple_clusters)之中。設定叢集的網路負載平衡器 (NLB)，即可達到叢集的跨地區負載平衡及跨地區網路。

如果您具有必須可供使用的資料，則即使運作中斷，也請務必將您的資料儲存在[持續性儲存空間](/docs/containers?topic=containers-storage_planning#storage_planning)中。

如需如何達到叢集高可用性的相關資訊，請參閱 [{{site.data.keyword.containerlong_notm}} 的高可用性](/docs/containers?topic=containers-ha#ha)。

## 我有哪些可用來保護叢集的選項？
{: #secure_cluster}
{: faq}

您可以使用 {{site.data.keyword.containerlong_notm}} 中的內建安全特性，來保護叢集裡的元件、資料及應用程式部署，以確保安全規範及資料完整性。請使用這些特性來保護 Kubernetes API 伺服器、etcd 資料儲存庫、工作者節點、儲存空間、映像檔及部署，免於惡意攻擊。您也可以運用內建的記載和監視工具，來偵測惡意攻擊和可疑的使用模式。

如需叢集元件以及如何保護每個元件的相關資訊，請參閱 [{{site.data.keyword.containerlong_notm}} 的安全](/docs/containers?topic=containers-security#security)。

## 我能為叢集使用者提供哪些存取原則？
{: #faq_access}
{: faq}

{{site.data.keyword.containerlong_notm}} 使用 {{site.data.keyword.iamshort}} (IAM) 透過 IAM 平台角色授與對叢集資源的存取權，並且還使用 Kubernetes 以角色為基礎的存取控制 (RBAC) 原則透過 IAM 服務角色來授與對叢集資源的存取權。如需存取原則類型的相關資訊，請參閱[為使用者選取適當的存取原則和角色](/docs/containers?topic=containers-users#access_roles)。
{: shortdesc}

指派給使用者的存取原則根據您想要使用者能夠執行的操作而有所不同。您可以在[使用者存取權參考頁面](/docs/containers?topic=containers-access_reference)或下表格的鏈結中找到有關哪些角色授權哪些動作類型的相關資訊。如需指派原則的步驟，請參閱[透過 {{site.data.keyword.Bluemix_notm}} IAM 授與使用者對叢集的存取權](/docs/containers?topic=containers-users#platform)。

|使用案例|範例角色和範圍|
| --- | --- |
|應用程式審核員|[對叢集、地區或資源群組的檢視者平台角色](/docs/containers?topic=containers-access_reference#view-actions)以及[對叢集、地區或資源群組的讀者服務角色](/docs/containers?topic=containers-access_reference#service)。|
|應用程式開發人員|[對叢集的編輯器平台角色](/docs/containers?topic=containers-access_reference#editor-actions)、[範圍限定為名稱空間的撰寫者服務角色](/docs/containers?topic=containers-access_reference#service)以及 [Cloud Foundry 開發人員空間角色](/docs/containers?topic=containers-access_reference#cloud-foundry)。|
|計費|[對叢集、地區或資源群組的檢視者平台角色](/docs/containers?topic=containers-access_reference#view-actions)。|
|建立叢集|對超級使用者基礎架構認證的帳戶層次許可權，對 {{site.data.keyword.containerlong_notm}} 的管理者平台角色以及對 {{site.data.keyword.registrylong_notm}} 的管理者平台角色。如需相關資訊，請參閱[準備建立叢集](/docs/containers?topic=containers-clusters#cluster_prepare)。|
|叢集管理者|[對叢集的管理者平台角色](/docs/containers?topic=containers-access_reference#admin-actions)以及[未將範圍限定為名稱空間（對整個叢集）的管理員服務角色](/docs/containers?topic=containers-access_reference#service)。|
|DevOps 操作員|[對叢集的操作者平台角色](/docs/containers?topic=containers-access_reference#operator-actions)、[未將範圍限定為名稱空間（對整個叢集）的撰寫者服務角色](/docs/containers?topic=containers-access_reference#service)以及 [Cloud Foundry 開發人員空間角色](/docs/containers?topic=containers-access_reference#cloud-foundry)。|
|操作員或網站可靠性工程師|[對叢集、地區或資源群組的管理者平台角色](/docs/containers?topic=containers-access_reference#admin-actions)、[對叢集或地區的讀者服務角色](/docs/containers?topic=containers-access_reference#service)或[對所有叢集名稱空間的管理員服務角色](/docs/containers?topic=containers-access_reference#service)，以能夠使用 `kubectl top nodes,pods` 指令。|
{: caption="可指派用於符合不同使用案例的角色類型。" caption-side="top"}

## 何處可以找到影響我的叢集的安全性公告清單？
{: #faq_security_bulletins}
{: faq}

如果在 Kubernetes 中找到漏洞，則 Kubernetes 會在安全性公告中發行 CVE，以通知使用者並且說明使用者為補救漏洞必須採取的動作。影響 {{site.data.keyword.containerlong_notm}} 使用者或 {{site.data.keyword.Bluemix_notm}} 平台的 Kubernetes 安全性公告則會在 [{{site.data.keyword.Bluemix_notm}} 安全性公告](https://cloud.ibm.com/status?component=containers-kubernetes&selected=security)中發佈。

部分 CVE 需要 Kubernetes 版本的最新修補程式更新，您可以在 {{site.data.keyword.containerlong_notm}} 中的一般[叢集更新處理程序](/docs/containers?topic=containers-update#update)過程中安裝。請務必及時套用安全修補程式，以保護叢集不受惡意攻擊。如需安全修補程式中所含內容的相關資訊，請參閱[版本變更日誌](/docs/containers?topic=containers-changelog#changelog)。

## 服務是否提供裸機和 GPU 的支援？
{: #bare_metal_gpu}
{: faq}

是，您可以將工作者節點佈建為單一承租戶實體裸機伺服器。裸機伺服器隨附工作負載的高效能好處，例如資料、AI 及 GPU。此外，所有硬體資源都為您的工作負載專用，因此您不必擔心「吵雜的鄰居」。

如需可用裸機特性以及裸機與虛擬機器有何不同的相關資訊，請參閱[實體機器（裸機）](/docs/containers?topic=containers-planning_worker_nodes#bm)。

## 服務支援哪些 Kubernetes 版本？
{: #supported_kube_versions}
{: faq}

{{site.data.keyword.containerlong_notm}} 同時支援多個 Kubernetes 版本。發行最新版本 (n) 時，最多支援到前 2 個 (n-2) 版本。超過最新版本前 2 個版本的版本 (n-3) 會先被淘汰，然後不受支援。
目前支援下列版本：

*   最新：1.14.2
*   預設：1.13.6
*   其他：1.12.9

如需支援的版本，以及您必須採取以從某個版本移至另一個版本之更新動作的相關資訊，請參閱[版本資訊與更新動作](/docs/containers?topic=containers-cs_versions#cs_versions)。

## 可在何處使用服務？
{: #supported_regions}
{: faq}

{{site.data.keyword.containerlong_notm}} 可在全球使用。您可以在每個支援的 {{site.data.keyword.containerlong_notm}} 地區建立標準叢集。免費叢集僅適用於選取地區。

如需支援的地區的相關資訊，請參閱[位置](/docs/containers?topic=containers-regions-and-zones#regions-and-zones)。

## 服務符合哪些標準？
{: #standards}
{: faq}

{{site.data.keyword.containerlong_notm}} 實作控制項與下列標準相稱：
- 歐盟-美國隱私權防護及瑞士-美國隱私權防護架構 (EU-US Privacy Shield and Swiss-US Privacy Shield Framework)
- 醫療保險轉移和責任法 (Health Insurance Portability and Accountability Act, HIPAA)
- Service Organization Control 標準（SOC 1、SOC 2 Type 1）
- International Standard on Assurance Engagements 3402 (ISAE 3402)、Assurance Reports on Controls at a Service Organization
- 國際標準組織 (International Organization for Standardization, ISO 27001, ISO 27017, ISO 27018)
- 支付卡產業資料安全標準 (Payment Card Industry Data Security Standard, PCI DSS)

## 我可以使用 IBM Cloud 及其他服務與叢集搭配嗎？
{: #faq_integrations}
{: faq}

您可以將 {{site.data.keyword.Bluemix_notm}} 平台及基礎架構服務，以及來自協力廠商供應商的服務新增至 {{site.data.keyword.containerlong_notm}}叢集，以啟用自動化、提高安全，或加強叢集裡的監視和記載功能。

如需支援服務的清單，請參閱[整合服務](/docs/containers?topic=containers-supported_integrations#supported_integrations)。

## 我可以連接 IBM Cloud Public 中的叢集與在內部部署資料中心內執行的應用程式嗎？
{: #hybrid}
{: faq}

您可以連接 {{site.data.keyword.Bluemix_notm}} Public 中的服務與內部部署資料中心，以建立自己的混合式雲端設定。如何運用 {{site.data.keyword.Bluemix_notm}} Public 及 Private 與在內部部署資料中心內執行的應用程式搭配的範例包括：
- 您在 {{site.data.keyword.Bluemix_notm}} Public 中使用 {{site.data.keyword.containerlong_notm}} 來建立叢集，但想要連接叢集與內部部署資料庫。
- 您可以在自己的資料中心的 {{site.data.keyword.Bluemix_notm}} Private 中建立 Kubernetes 叢集，並將應用程式部署至您的叢集。不過，您的應用程式可能會在 {{site.data.keyword.Bluemix_notm}} Public 中使用 {{site.data.keyword.ibmwatson_notm}} 服務，例如 Tone Analyzer。

若要啟用在 {{site.data.keyword.Bluemix_notm}} Public 執行的服務與執行內部部署的服務之間的通訊，您必須[設定 VPN 連線](/docs/containers?topic=containers-vpn#vpn)。若要連接 {{site.data.keyword.Bluemix_notm}} Public 或 Dedicated 環境與 {{site.data.keyword.Bluemix_notm}} Private 環境，請參閱[使用 {{site.data.keyword.containerlong_notm}} 與 {{site.data.keyword.Bluemix_notm}} Private 搭配](/docs/containers?topic=containers-hybrid_iks_icp#hybrid_iks_icp)。

如需支援的 {{site.data.keyword.containerlong_notm}} 供應項目的概觀，請參閱[供應項目與其組合的比較](/docs/containers?topic=containers-cs_ov#differentiation)。

## 我可以在自己的資料中心內部署 IBM Cloud Kubernetes Service 嗎？
{: #private}
{: faq}

如果您不想將應用程式移至 {{site.data.keyword.Bluemix_notm}} Public，但仍想要運用 {{site.data.keyword.containerlong_notm}} 的特性，則可以安裝 {{site.data.keyword.Bluemix_notm}} Private。{{site.data.keyword.Bluemix_notm}} Private 是可在您自己機器本端安裝的應用程式平台，而且您可以將其用於在防火牆後面的專屬控制環境中開發及管理內部部署的容器化應用程式。

如需相關資訊，請參閱 [{{site.data.keyword.Bluemix_notm}} Private 產品說明文件 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_1.2.0/kc_welcome_containers.html)。
 

## 使用 IBM Cloud Kubernetes Service 時，我要支付哪些項目的費用？
{: #charges}
{: faq}

使用 {{site.data.keyword.containerlong_notm}} 叢集，您可以搭配使用 IBM Cloud 基礎架構 (SoftLayer) 運算、網路及儲存空間資源與平台服務（例如 Watson AI 或 Compose Database-as-a-Service）。每個資源都可能會有自己的費用，其可以是[固定、計量、分層或保留](/docs/billing-usage?topic=billing-usage-charges#charges)。
* [工作者節點](#nodes)
* [出埠網路](#bandwidth)
* [子網路 IP 位址](#subnet_ips)
* [儲存空間](#persistent_storage)
* [{{site.data.keyword.Bluemix_notm}} 服務](#services)
* [Red Hat OpenShift on IBM Cloud](#rhos_charges)

<dl>
<dt id="nodes">工作者節點</dt>
  <dd><p>叢集可以有兩種主要類型的工作者節點：虛擬或實體（裸機）機器。機型可用性及定價會因您部署叢集的區域而異。</p>
  <p><strong>虛擬機器</strong>具備比裸機更大的彈性、更快速的佈建時間，以及更多的自動可擴充性特性，而且價格比裸機更加划算。不過，與裸機規格（例如網路 Gbps、RAM 和記憶體臨界值，以及儲存空間選項）比較時，VM 會做出效能取捨。請記住下列影響 VM 成本的因素：</p>
  <ul><li><strong>共用與專用</strong>：如果您共用 VM 的基礎硬體，則成本會低於專用硬體，但實體資源並非專供您的 VM 使用。</li>
  <li><strong>僅限按小時計費</strong>：「按小時」提供更多的彈性來快速訂購及取消 VM。
  <li><strong>每個月的分層時數</strong>：將按小時計費分層。因為針對計費月份內的小時層級訂購您的 VM，所以向您收取的每小時費率會較低。小時的層級如下：0 - 150 小時、151 - 290 小時、291 - 540 小時及 541+ 小時。</li></ul>
  <p><strong>實體機器（裸機）</strong>會為資料、AI 及 GPU 等工作負載帶來高效能優點。此外，所有硬體資源都為您的工作負載專用，因此您沒有「吵雜的鄰居」。請記住下列影響裸機成本的因素：</p>
  <ul><li><strong>僅限按月計費</strong>：所有裸機都是按月計費。</li>
  <li><strong>較長訂購程序</strong>：訂購或取消裸機伺服器之後，會以手動方式，在您的 IBM Cloud 基礎架構 (SoftLayer) 帳戶中完成處理程序。因此，可能需要多個營業日才能完成。</li></ul>
  <p>如需機器規格的詳細資料，請參閱[工作者節點的可用硬體](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)。</p></dd>

<dt id="bandwidth">公用頻寬</dt>
  <dd><p>頻寬是指入埠及出埠網路資料流量的公用資料傳送，即進出全球資料中心之 {{site.data.keyword.Bluemix_notm}} 資源的網路資料流量。公用頻寬是按每 GB 收費。您可以檢閱目前的頻寬摘要，只要登入 [{{site.data.keyword.Bluemix_notm}} 主控台](https://cloud.ibm.com/)，從功能表 ![功能表圖示](../icons/icon_hamburger.svg "功能表圖示") 中選取**標準基礎架構**，然後選取**網路 > 頻寬 > 摘要**頁面即可。<p>檢閱下列影響公用頻寬費用的因素：</p>
  <ul><li><strong>位置</strong>：與使用工作者節點相同，費用會視資源部署所在的區域而不同。</li>
  <li><strong>已包括頻寬或隨收隨付制</strong>：您的工作者節點機器可能會隨附每個月出埠網路的特定配置（例如 250GB 用於 VM 或 500GB 用於裸機）。或者，根據 GB 用量，配置可能是「隨收隨付制」。</li>
  <li><strong>分層套件</strong>：在您超出任何已包括的頻寬之後，會根據依位置而異的分層用量方法向您收費。如果您超出層級分配，則也可能會向您收取標準資料傳送費用。</li></ul>
  <p>如需相關資訊，請參閱[頻寬套件 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/cloud/bandwidth)。</p></dd>

<dt id="subnet_ips">子網路 IP 位址</dt>
  <dd><p>當您建立標準叢集時，會訂購 1 個可攜式公用子網路（含 8 個公用 IP 位址），並每月向您的帳戶收費。</p><p>如果您的基礎架構帳戶中已有可用的子網路，則您可以改為使用這些子網路。使用 `--no-subnets` [旗標](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_create)建立叢集，然後[重複使用子網路](/docs/containers?topic=containers-subnets#subnets_custom)。</p>
  </dd>

<dt id="persistent_storage">儲存空間</dt>
  <dd>當您佈建儲存空間時，可以為您的使用案例選擇適合的儲存空間類型及儲存空間類別。費用會視儲存空間類型、位置及儲存空間實例規格而不同。部分儲存空間解決方案（例如檔案及區塊儲存空間）提供您可以從中選擇的每小時及每月方案。若要選擇正確的儲存空間解決方案，請參閱[規劃高度可用的持續性儲存空間](/docs/containers?topic=containers-storage_planning#storage_planning)。如需相關資訊，請參閱：<ul><li>[NFS 檔案儲存空間定價 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/cloud/file-storage/pricing)</li>
  <li>[區塊儲存空間定價 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/cloud/block-storage/pricing)</li>
  <li>[物件儲存空間方案 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api)</li></ul></dd>

<dt id="services">{{site.data.keyword.Bluemix_notm}} 服務</dt>
  <dd>與您的叢集整合的每個服務都有自己的定價模型。請檢閱每個產品文件，並使用 {{site.data.keyword.Bluemix_notm}} 主控台來[預估成本](/docs/billing-usage?topic=billing-usage-cost#cost)。</dd>

<dt id="rhos_charges">Red Hat OpenShift on IBM Cloud</dt>
  <dd>
  <p class="preview">[Red Hat OpenShift on IBM Cloud](/docs/containers?topic=containers-openshift_tutorial) 以測試版形式提供，用來測試 OpenShift 叢集。
</p>如果建立 [Red Hat OpenShift on IBM Cloud 叢集](/docs/containers?topic=containers-openshift_tutorial)，則工作者節點將隨 Red Hat Enterprise Linux 作業系統一起安裝，這將增加[工作者節點機器](#nodes)的價格。您還必須具有 OpenShift 授權，除了按小時的 VM 成本或按月 Bare Metal Server 成本外，還會發生按月成本。OpenShift 授權適用於工作者節點特性的每 2 個核心數。如果在月末之前刪除了您的工作者節點，則按月授權可供工作者節點儲存區中的其他工作者節點使用。如需 OpenShift 叢集的相關資訊，請參閱[建立 Red Hat OpenShift on IBM Cloud 叢集](/docs/containers?topic=containers-openshift_tutorial)。</dd>

</dl>
<br><br>

每月資源是根據該月的第一天收取前一個月用量的費用。如果您在月中訂購每月資源，則會向您收取該月份的按比例分配金額。不過，如果您在月中取消資源，則仍會向您收取每月資源的完整金額。
{: note}

## 我的平台和基礎架構資源是否合併在一個帳單中？
{: #bill}
{: faq}

使用 {{site.data.keyword.Bluemix_notm}} 計費帳戶時，會在一個帳單中彙總平台及基礎架構資源。如果您已鏈結 {{site.data.keyword.Bluemix_notm}} 及 IBM Cloud 基礎架構 (SoftLayer) 帳戶，則會收到 {{site.data.keyword.Bluemix_notm}} 平台與基礎架構資源的[合併帳單](/docs/customer-portal?topic=customer-portal-unifybillaccounts#unifybillaccounts)。

## 是否可以預估成本？
{: #cost_estimate}
{: faq}

是，請參閱[預估成本](/docs/billing-usage?topic=billing-usage-cost#cost)。請記住，某些費用不會反映在預估中，例如，已增加之每小時用量的分層計價。如需相關資訊，請參閱[使用 {{site.data.keyword.containerlong_notm}} 時，我要支付哪些項目的費用？](#charges)。

## 是否可以檢視現行用量？
{: #usage}
{: faq}

您可以檢查 {{site.data.keyword.Bluemix_notm}} 平台及基礎架構資源的現行用量及每月預估總計。如需相關資訊，請參閱[檢視用量](/docs/billing-usage?topic=billing-usage-viewingusage#viewingusage)。若要組織您的計費，您可以使用[資源群組](/docs/resources?topic=resources-bp_resourcegroups#bp_resourcegroups)將資源分組。
