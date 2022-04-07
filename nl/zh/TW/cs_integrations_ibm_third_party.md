---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-07"

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


# IBM Cloud 服務和協力廠商整合
{: #ibm-3rd-party-integrations}

您可以使用 {{site.data.keyword.Bluemix_notm}} 平台和基礎架構服務以及其他協力廠商整合來為叢集新增額外功能。
{: shortdesc}

## IBM Cloud 服務
{: #ibm-cloud-services}

請檢閱下列資訊，以瞭解 {{site.data.keyword.Bluemix_notm}} 平台和基礎架構服務如何與 {{site.data.keyword.containerlong_notm}} 整合以及如何在叢集裡使用這些服務。
{: shortdesc}

### IBM Cloud 平台服務
{: #platform-services}

支援服務金鑰的所有 {{site.data.keyword.Bluemix_notm}} 平台服務都可以使用 {{site.data.keyword.containerlong_notm}} [服務連結](/docs/containers?topic=containers-service-binding)進行整合。
{: shortdesc}

服務連結是一種快速方式，用來建立 {{site.data.keyword.Bluemix_notm}} 服務的服務認證，並將這些認證儲存在叢集的 Kubernetes 密碼中。在 etcd 中會將 Kubernetes 密碼自動加密，以保護您的資料。應用程式可以使用密碼中的認證來存取 {{site.data.keyword.Bluemix_notm}} 服務實例。

不支援服務金鑰的服務通常會提供可直接在應用程式中使用的 API。

若要尋找常用 {{site.data.keyword.Bluemix_notm}} 服務的概觀，請參閱[常用整合](/docs/containers?topic=containers-supported_integrations#popular_services)。

### IBM Cloud 基礎架構服務
{: #infrastructure-services}

由於 {{site.data.keyword.containerlong_notm}} 允許您建立根據 {{site.data.keyword.Bluemix_notm}} 基礎架構的 Kubernetes 叢集，因此某些基礎架構服務（例如，Virtual Servers、Bare Metal Servers 或 VLAN）已完全整合到 {{site.data.keyword.containerlong_notm}} 中。您可以使用 {{site.data.keyword.containerlong_notm}} API、CLI 或主控台來建立和使用這些服務實例。
{: shortdesc}

支援的持續儲存解決方案（例如，{{site.data.keyword.Bluemix_notm}} File Storage、{{site.data.keyword.Bluemix_notm}} Block Storage 或 {{site.data.keyword.cos_full}}）作為 Kubernetes Flex 驅動程式整合，並且可以使用 [Helm 圖表](/docs/containers?topic=containers-helm)進行設定。Helm 圖表會自動設定叢集裡的 Kubernetes 儲存類別、儲存空間提供者和儲存空間驅動程式。可以使用儲存類別來透過持續性磁區要求 (PVC) 佈建持續性儲存空間。

若要保護叢集網路或連接至內部部署資料中心，可以配置下列其中一個選項：
- [strongSwan IPSec VPN 服務](/docs/containers?topic=containers-vpn#vpn-setup)
- [{{site.data.keyword.BluDirectLink}}](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link)
- [虛擬路由器應用裝置 (VRA)](/docs/containers?topic=containers-vpn#vyatta)
- [Fortigate Security Appliance (FSA)](/docs/services/vmwaresolutions/services?topic=vmware-solutions-fsa_considerations)

## Kubernetes 社群和開放程式碼整合
{: #kube-community-tools}

由於您擁有在 {{site.data.keyword.containerlong_notm}} 中建立的標準叢集，因此可以選擇安裝協力廠商解決方案以向叢集新增額外功能。
{: shortdesc}

某些開放程式碼技術（例如，Knative、Istio、LogDNA、Sysdig 或 Portworx）已經過 IBM 測試，並作為受管理附加程式、Helm 圖表或 {{site.data.keyword.Bluemix_notm}} 服務（由與 IBM 合作的服務提供者運作）提供。這些開放程式碼工具完全整合到 {{site.data.keyword.Bluemix_notm}} 計費和支援系統中。

可以在叢集裡安裝其他開放程式碼工具，但這些工具可能無法在 {{site.data.keyword.containerlong_notm}} 中進行管理、支援或驗證。

### 透過合作運作的整合
{: #open-source-partners}

如需 {{site.data.keyword.containerlong_notm}} 夥伴及其提供的每種解決方案的優點的相關資訊，請參閱 [{{site.data.keyword.containerlong_notm}} 夥伴](/docs/containers?topic=containers-service-partners)。

### 受管理附加程式
{: #cluster-add-ons}
{{site.data.keyword.containerlong_notm}} 使用[受管理附加程式](/docs/containers?topic=containers-managed-addons)整合了常用開放程式碼整合，例如 [Knative](/docs/containers?topic=containers-serverless-apps-knative) 或 [Istio](/docs/containers?topic=containers-istio)。透過受管理附加程式，可在叢集裡輕鬆安裝已經過 IBM 測試並已核准在 {{site.data.keyword.containerlong_notm}} 中使用的開放程式碼工具。

受管理附加程式已完全整合至 {{site.data.keyword.Bluemix_notm}} 支援組織。如果您在使用受管理附加程式時有疑問或遇到問題，可以使用其中一個 {{site.data.keyword.containerlong_notm}} 支援頻道。如需相關資訊，請參閱[取得協助及支援](/docs/containers?topic=containers-cs_troubleshoot_clusters#clusters_getting_help)。

如果您新增至叢集的工具產生成本需求，則這些成本會自動整合並列在您的按月 {{site.data.keyword.Bluemix_notm}} 計費中。計費週期由 {{site.data.keyword.Bluemix_notm}} 決定，視您在叢集裡啟用附加程式的時間而定。

### 其他協力廠商整合
{: #kube-community-helm}

您可以安裝任何與 Kubernetes 整合的協力廠商開放程式碼工具。例如，Kubernetes 社群指定了某些 Helm 圖表`stable` 或 `incubator`。請注意，這些圖表或工具是否適合用於 {{site.data.keyword.containerlong_notm}} 並未經過驗證。如果工具需要授權，則必須先購買授權，然後才能使用該工具。如需 Kubernetes 社群的可用 Helm 圖表的概觀，請參閱 [Helm 圖表![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/kubernetes/solutions/helm-charts) 目錄中的 `kubernetes` 和 `kubernetes-incubator` 儲存庫。
{: shortdesc}

使用協力廠商開放程式碼整合所發生的任何成本都不會包含在按月 {{site.data.keyword.Bluemix_notm}} 帳單中。

從 Kubernetes 社群安裝協力廠商開放程式碼整合或 Helm 圖表可能會變更預設叢集配置，並且會使叢集進入不受支援的狀態。如果使用其中任何工具時遇到問題，請直接諮詢 Kubernetes 社群或服務提供者。
{: important}
