---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-03"

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


# 定義 Kubernetes 策略
{: #strategy}

使用 {{site.data.keyword.containerlong}}，您可以在正式作業中快速且安全地為您的應用程式部署容器工作負載。進一步瞭解，讓您在規劃叢集策略時，可以將設定最佳化，以善加利用 [Kubernetes ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/) 自動化部署、調整及編排管理功能。
{:shortdesc}

## 將工作負載移至 {{site.data.keyword.Bluemix_notm}}
{: #cloud_workloads}

有許多原因會讓您將工作負載移至 {{site.data.keyword.Bluemix_notm}}：減少總擁有成本、提高應用程式在安全和相容環境中的高可用性、擴增和縮減以回應使用者需求等等。{{site.data.keyword.containerlong_notm}} 結合容器技術與 Kubernetes 這類開放程式碼工具，因此，您可以建置能夠在不同雲端環境之間移轉的雲本機應用程式，進而避免供應商鎖定。
{:shortdesc}

然而，該如何達到雲端？這一路上的選擇為何？而且，在到達雲端之後，該如何管理工作負載？

請利用這個頁面來瞭解一些在 {{site.data.keyword.containerlong_notm}} 上部署 Kubernetes 的策略。並且，隨時歡迎您在 [Slack ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://ibm-container-service.slack.com) 上與我們的團隊進行交流。

尚未位於 Slack 上？[請申請邀請！](https://bxcs-slack-invite.mybluemix.net/)
{: tip}

### 可以移至 {{site.data.keyword.Bluemix_notm}} 的內容為何？
{: #move_to_cloud}

使用 {{site.data.keyword.Bluemix_notm}} 時，您可以靈活地在[外部部署、內部部署或混合式雲端環境](/docs/containers?topic=containers-cs_ov#differentiation)中建立 Kubernetes 叢集。下表提供一些範例說明使用者一般移至各種雲端類型的工作負載類型。
您還可以選擇使叢集同時在兩種環境中執行的混合方法。
{: shortdesc}

| 工作負載 |{{site.data.keyword.containershort_notm}} 外部部署|內部部署|
| --- | --- | --- |
| DevOps 啟用工具 | <img src="images/confirm.svg" width="32" alt="可用的特性" style="width:32px;" /> | |
| 開發及測試應用程式 | <img src="images/confirm.svg" width="32" alt="可用的特性" style="width:32px;" /> | |
| 應用程式的需求有重大改變，需要快速擴充 | <img src="images/confirm.svg" width="32" alt="可用的特性" style="width:32px;" /> | |
| CRM、HCM、ERP 及電子商務這類商業應用程式 | <img src="images/confirm.svg" width="32" alt="可用的特性" style="width:32px;" /> | |
| 電子郵件這類協同作業及社交工具 | <img src="images/confirm.svg" width="32" alt="可用的特性" style="width:32px;" /> | |
| Linux 及 x86 工作負載 | <img src="images/confirm.svg" width="32" alt="可用的特性" style="width:32px;" /> | |
| 裸機伺服器與 GPU 運算資源 | <img src="images/confirm.svg" width="32" alt="可用的特性" style="width:32px;" /> | <img src="images/confirm.svg" width="32" alt="可用的特性" style="width:32px;" /> |
|符合 PCI 和 HIPAA 的工作負載| <img src="images/confirm.svg" width="32" alt="可用的特性" style="width:32px;" /> | <img src="images/confirm.svg" width="32" alt="可用的特性" style="width:32px;" /> |
| 具有平台和基礎架構限制與相依關係的舊式應用程式 | | <img src="images/confirm.svg" width="32" alt="可用的特性" style="width:32px;" /> |
| 具有嚴格設計、授權或大量法規的專有應用程式 | | <img src="images/confirm.svg" width="32" alt="可用的特性" style="width:32px;" /> |
| 調整公用雲端中的應用程式，並將資料同步化至現場專用資料庫 | <img src="images/confirm.svg" width="32" alt="可用的特性" style="width:32px;" /> | <img src="images/confirm.svg" width="32" alt="可用的特性" style="width:32px;" /> |
{: caption="{{site.data.keyword.Bluemix_notm}} 實作支援您的工作負載" caption-side="top"}

**已準備好在 {{site.data.keyword.containerlong_notm}} 中外部部署執行工作負載？**</br>
太棒了！您已在公用雲端文件中。請持續閱讀更多策略構想，或[立即建立叢集](/docs/containers?topic=containers-getting-started)以馬上開始行動。

**對內部部署雲端感興趣？**</br>
探索 [{{site.data.keyword.Bluemix_notm}} Private 文件 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.1/kc_welcome_containers.html)。如果您已大量投資 WebSphere Application Server 和 Liberty 這類 IBM 技術，則可以利用各種工具來最佳化 {{site.data.keyword.Bluemix_notm}} Private 現代化策略。

**要同時在內部部署雲端和外部部署雲端中執行工作負載？**</br>
請從設定 {{site.data.keyword.Bluemix_notm}} Private 帳戶開始。然後，請參閱[使用 {{site.data.keyword.containerlong_notm}} 與 {{site.data.keyword.Bluemix_notm}} Private 搭配](/docs/containers?topic=containers-hybrid_iks_icp)，以在 {{site.data.keyword.Bluemix_notm}} Public 中連接具有叢集的 {{site.data.keyword.Bluemix_notm}} Private 環境。若要管理多個雲端 Kubernetes 叢集（例如，跨 {{site.data.keyword.Bluemix_notm}} Public 和 {{site.data.keyword.Bluemix_notm}} Private），請查看 [IBM Multicloud Manager ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.1.0/mcm/getting_started/introduction.html)。

### 我可以在 {{site.data.keyword.containerlong_notm}} 中執行哪些類型的應用程式？
{: #app_types}

您的容器化應用程式必須能夠在受支援的作業系統 Ubuntu 16.64、18.64 上執行。您也要考慮應用程式的狀態性。
{: shortdesc}

<dl>
<dt>無狀態的應用程式</dt>
  <dd><p>無狀態的應用程式是 Kubernetes 這類雲端本機環境的首選。它們易於移轉及調整，因為它們會宣告相依關係、將配置與程式碼分開儲存，並將資料庫這類支援服務視為已連接的資源，而不是連結至應用程式。應用程式 Pod 不需要持續資料儲存空間或穩定網路 IP 位址，因此可以終止、重新排程及調整 Pod，以回應工作負載需求。該應用程式使用「資料庫即服務」來持續保存資料，並且使用 NodePort、負載平衡器或 Ingress 服務在穩定 IP 位址上公開工作負載。</p></dd>
<dt>有狀態的應用程式</dt>
  <dd><p>在設定、管理及調整方面，有狀態的應用程式比無狀態的應用程式更為複雜，因為 Pod 需要持續資料及穩定網路身分。有狀態的應用程式通常是資料庫或其他分散式資料密集工作負載，而這些工作負載的處理更有效的接近資料本身。</p>
  <p>如果您要部署有狀態的應用程式，則需要設定持續性儲存空間，並將持續性磁區裝載至由 StatefulSet 物件所控制的 Pod。您可以選擇將[檔案](/docs/containers?topic=containers-file_storage#file_statefulset)、[區塊](/docs/containers?topic=containers-block_storage#block_statefulset)或[物件](/docs/containers?topic=containers-object_storage#cos_statefulset)儲存空間新增為有狀態集合的持續性儲存空間。您也可以在裸機工作者節點上安裝 [Portworx](/docs/containers?topic=containers-portworx)，並使用 Portworx 作為高度可用的軟體定義儲存空間解決方案，來管理有狀態應用程式的持續性儲存空間。如需有狀態集合運作方式的相關資訊，請參閱 [Kubernetes 文件 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/)。</p></dd>
</dl>

### 開發無狀態雲端本機應用程式有哪些準則？
{: #12factor}

請查看 [12 因子應用程式 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://12factor.net/)，這種不限語言的方法考慮如何跨 12 個因子來開發應用程式，總結如下。
{: shortdesc}

1.  **程式碼庫**：在版本控制系統中為您的部署使用單一程式碼庫。當您取回容器部署的映像檔時，請指定已測試的映像檔標籤，而不是使用 `latest`。
2.  **相依關係**：明確地宣告及隔離外部相依關係。
3.  **配置**：將部署特定配置儲存在環境變數中，而不是程式碼。
4.  **支援服務**：將資料儲存或訊息佇列這類支援服務視為已連接或可更換的資源。
5.  **應用程式階段**：在不同的階段（例如 `build`、`release`、`run`）中建置，並且嚴格區隔它們。
6.  **處理程序**：以一個以上的無狀態處理程序執行，這些無狀態處理程序未共用任何項目，且使用[持續性儲存空間](/docs/containers?topic=containers-storage_planning)來儲存資料。
7.  **埠連結**：埠連結為自行包含，並在明確定義的主機和埠上提供服務端點。
8.  **並行**：透過處理程序實例（例如抄本和水平調整）管理及調整應用程式。設定部署的資源要求和限制。請注意，Calico 網路原則無法限制頻寬。請改為考慮 [Istio](/docs/containers?topic=containers-istio)。
9.  **可移除**：將您的應用程式設計為可移除的，具有最小啟動、循序關閉以及對突然處理程序終止的容錯。請記住，容器、Pod 甚至是工作者節點都是可移除的，因此請相應地規劃您的應用程式。
10.  **開發到正式作業保持一致**：為應用程式設定[持續整合](https://www.ibm.com/cloud/garage/content/code/practice_continuous_integration/)和[持續交付](https://www.ibm.com/cloud/garage/content/deliver/practice_continuous_delivery/)管道，盡可能縮小開發環境中的應用程式與正式作業環境中的應用程式之間的差異。
11.  **日誌**：將日誌視為事件串流：外部或管理環境會處理及遞送日誌檔。**重要事項**：在 {{site.data.keyword.containerlong_notm}} 中，依預設不會開啟日誌。若要啟用，請參閱[配置日誌轉遞](/docs/containers?topic=containers-health#configuring)。
12.  **管理處理程序**：將任何一次性管理 Script 與應用程式一起保留，並將這些 Script 作為 [Kubernetes 工作物件 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/concepts/workloads/controllers/jobs-run-to-completion/) 執行，以確保管理 Script 的執行環境與應用程式本身相同。若要編排您要在 Kubernetes 叢集裡執行的較大型套件，請考量使用套件管理程式，例如 [Helm ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://helm.sh/)。

### 我已經有應用程式。如何將它移轉至 {{site.data.keyword.containerlong_notm}}？
{: #migrate_containerize}

您可以採取一些一般步驟，如下所示將您的應用程式容器化。
{: shortdesc}

1.  使用 [12 因子應用程式 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://12factor.net/) 作為指引，來隔離相依關係、將處理程序分成幾個個別服務，以及盡可能減少應用程式的狀態性。
2.  尋找要使用的適當基礎映像檔。您可以使用 [Docker Hub ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://hub.docker.com/) 中公開可用的映像檔、[公用 IBM 映像檔](/docs/services/Registry?topic=registry-public_images#public_images)，或在專用 {{site.data.keyword.registryshort_notm}} 中建置及管理您自己的映像檔。
3.  只將執行應用程式所需的內容新增至 Docker 映像檔。
4.  規劃使用持續性儲存空間或雲端資料庫即服務解決方案來備份應用程式資料，而不依賴本端儲存空間。
5.  一段時間後，將應用程式處理程序重構為微服務。

如需相關資訊，請參閱下列指導教學：
*  [將應用程式從 Cloud Foundry 移轉至叢集](/docs/containers?topic=containers-cf_tutorial#cf_tutorial)
*  [將 VM 型應用程式移至 Kubernetes](/docs/tutorials?topic=solution-tutorials-vm-to-containers-and-kubernetes)



如需移動 Kubernetes 環境、高可用性、服務探索及部署這類工作負載時的考量，請繼續進行下列主題。

<br />


### 在將應用程式移至 {{site.data.keyword.containerlong_notm}} 之前，最好具備哪些知識和技術技能？
{: #knowledge}

Kubernetes 旨在為兩個主要角色提供功能：叢集管理和應用程式開發人員。每個角色使用不同的技術技能，以順利執行應用程式並將應用程式部署到叢集。
{: shortdesc}

**叢集管理的主要作業和需要具備的技術知識是什麼？** </br>
作為叢集管理，您負責設定、操作、保護和管理叢集的 {{site.data.keyword.Bluemix_notm}} 基礎架構。典型作業包括：
- 調整叢集的大小，以便為工作負載提供足夠的容量。
- 設計叢集，以符合公司的高可用性、災難回復和合規標準。
- 透過設定使用者許可權並限制叢集裡的動作來保護運算資源、網路和資料，進而確保叢集安全。
- 規劃和管理基礎架構元件之間的網路通訊，以確保網路安全、分段和合規性。
- 規劃持續性儲存空間選項，以符合資料儲存空間位置和資料保護需求。

叢集管理角色必須具備豐富的知識，包括運算、網路、儲存空間、安全和合規性。在典型的公司中，這些知識一般由多個專家分別掌握，例如系統工程師、系統管理者、網路工程師、網路架構設計師、IT 管理程式或安全與合規專家。請考慮將叢集管理角色指派給公司中的多個人員，以便您具備順利操作叢集所需的知識。

**應用程式開發人員的主要作業和需要具備的技術知識是什麼？** </br>
作為開發人員，您要在 Kubernetes 叢集裡設計、建立、保護、部署、測試、執行和監視雲端本機容器化應用程式。若要建立和執行這些應用程式，您必須熟悉微服務的概念、[12 因子應用程式](#12factor)準則、[Docker 和容器化原則](https://www.docker.com/)以及可用的 [Kubernetes 部署選項](/docs/containers?topic=containers-app#plan_apps)。如果要部署無伺服器應用程式，請熟悉 [Knative](/docs/containers?topic=containers-cs_network_planning)。

Kubernetes 和 {{site.data.keyword.containerlong_notm}} 提供了有關如何[公開應用程式並使應用程式保持專用](/docs/containers?topic=containers-cs_network_planning)、[新增持續性儲存空間](/docs/containers?topic=containers-storage_planning)、[整合其他服務](/docs/containers?topic=containers-ibm-3rd-party-integrations)以及如何[保護工作負載和敏感資料](/docs/containers?topic=containers-security#container)的多個選項。在將應用程式移至 {{site.data.keyword.containerlong_notm}} 中的叢集之前，請驗證是否可以在支援的 Ubuntu 16.64 和 18.64 作業系統上將應用程式作為容器化應用程式執行，以及 Kubernetes 和 {{site.data.keyword.containerlong_notm}} 是否提供了工作負載所需的功能。

**叢集管理者和開發人員之間要彼此互動嗎？**</br>
是。叢集管理者和開發人員必須經常進行互動，透過互動，叢集管理者可瞭解在叢集裡提供此功能的工作負載需求，而開發人員可瞭解其應用程式開發程序中必須考慮的可用限制、整合和安全原則。

## 調整 Kubernetes 叢集大小以支援工作負載
{: #sizing}

找出叢集裡需要多少個工作者節點來支援工作負載並不是一門精確的科學。您可能需要測試不同的配置並進行調整。幸運的是您使用 {{site.data.keyword.containerlong_notm}}，您可在其中新增及移除工作者節點以回應工作負載需求。
{: shortdesc}

若要開始調整叢集大小，請問問自己下列問題。

### 我的應用程式需要多少資源？
{: #sizing_resources}

首先，讓我們從您的現有或專案工作負載使用開始。

1.  計算工作負載的平均 CPU 及記憶體用量。例如，您可以在 Windows 機器上檢視「作業管理程式」，或在 Mac 或 Linux 上執行 `top` 指令。您也可以使用度量值服務及執行報告，來計算工作負載用量。
2.  預期工作負載必須提供的要求數目，讓您可以決定要處理工作負載的應用程式抄本數目。例如，您可以設計應用程式實例每分鐘處理 1000 個要求，並預期您的工作負載每分鐘必須提供 10000 個要求。若是如此，您可以決定建立 12 個應用程式抄本，其中 10 個處理預期的數量，另 2 個則處理激增的容量。

### 除了我的應用程式之外，還有其他項目可能使用叢集裡的資源嗎？
{: #sizing_other}

現在，讓我們新增一些您可能會使用的其他特性。



1.  考量應用程式是否取回大型或許多映像檔，而這些映像檔會佔用工作者節點上的本端儲存空間。
2.  決定是否要[整合服務](/docs/containers?topic=containers-supported_integrations#supported_integrations)至叢集（例如 [Helm](/docs/containers?topic=containers-helm#public_helm_install) 或 [Prometheus ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/coreos/prometheus-operator/tree/master/contrib/kube-prometheus)）。這些整合式服務及附加程式會啟動耗用叢集資源的 Pod。

### 我想要工作負載有何種類型的可用性？
{: #sizing_availability}

不要忘記，您希望您的工作負載盡可能地多！

1.  規劃[高可用性叢集](/docs/containers?topic=containers-ha_clusters#ha_clusters)的策略，例如決定單一叢集或多區域叢集。
2.  檢閱[高可用性部署](/docs/containers?topic=containers-app#highly_available_apps)，以協助決定如何讓您的應用程式可供使用。

### 我需要多少個工作者節點才能處理工作負載？
{: #sizing_workers}

既然，您已經清楚瞭解工作負載是什麼樣子，就讓我們將預估用量對映至可用的叢集配置。

1.  估計工作者節點容量上限（視您的叢集類型而定）。發生激增或其他暫時事件時，您不想要將工作者節點容量最大化。
    *  **單一區域叢集**：規劃在叢集裡至少有 3 個工作者節點。此外，您還希望叢集內有 1 個擁有 CPU 和記憶體容量的額外節點。
    *  **多區域叢集**：規劃每個區域至少有 2 個工作者節點，因此 3 個區域共有 6 個節點。此外，規劃總叢集容量至少為 150% 的總工作負載必要容量，因此，如果 1 個區域關閉，您可以有資源來維護工作負載。
2.  讓應用程式大小和工作者節點容量與其中一個[可用的工作者節點特性](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)一致。若要查看區域中的可用特性，請執行 `ibmcloud ks machine-types <zone>`。
    *   **不要使工作者節點超載**：為了避免 Pod 競爭 CPU 或執行效率不佳，您必須知道應用程式所需的資源，以規劃需要的工作者節點數目。例如，如果您應用程式所需的資源比工作者節點上可用的資源還要少，則可以限制部署至一個工作者節點的 Pod 數目。使您的工作者節點保持在 75% 左右的容量，以保留空間供可能需要排定的其他 Pod 使用。如果您應用程式所需的資源比工作者節點上可用的資源還要多，請使用可滿足這些需求的不同工作者節點特性。如果您的工作者節點經常回報 `NotReady` 狀態，或因缺乏記憶體或其他資源而收回 Pod，則您知道工作者節點已超載。
    *   **較大與較小的工作者節點特性**：較大的節點可能比較小的節點更具成本效益，特別是對於設計用於在高性能機器上處理時提高效率的工作負載。不過，如果大型工作者節點關閉，您需要確保叢集具有足夠的容量，才能循序將所有工作負載 Pod 重新排定到叢集裡的其他工作者節點。較小的工作者節點可協助您更妥善調整。
    *   **應用程式的抄本**：若要判定您想要的工作者節點數目，您也可以考量所要執行之應用程式的抄本數目。例如，如果您知道工作負載需要 32 個 CPU 核心，而且規劃執行應用程式的 16 個抄本，則每個抄本 Pod 都需要 2 個 CPU 核心。如果每個工作者節點只要執行一個應用程式 Pod，您可以為叢集類型訂購適當數目的工作者節點來支援此配置。
3.  執行效能測試，以透過代表性延遲、可調整性、資料集及工作負載需求繼續調整叢集裡所需的工作者節點數目。
4.  對於需要擴增及縮減以回應資源要求的工作負載，設定 [Horizontal Pod Autoscaler](/docs/containers?topic=containers-app#app_scaling) 和[叢集工作者節點儲存區 Autoscaler](/docs/containers?topic=containers-ca#ca)。

<br />


## 建構 Kubernetes 環境
{: #kube_env}

{{site.data.keyword.containerlong_notm}} 僅會鏈結至一個 IBM Cloud 基礎架構 (SoftLayer) 組合。在您的帳戶內，您可以建立由主節點與各種工作者節點組成的叢集。IBM 會管理主節點，而您可以建立一個混合的工作者節點儲存區，將相同特性的個別機器或記憶體與 CPU 規格聚集在一起。在叢集內，您可以使用名稱空間和標籤來進一步組織資源。選擇正確的叢集、機型與組織策略混合，以確定團隊和工作負載可以取得其所需的資源。
{:shortdesc}

### 我應該取得的叢集類型及機型為何？
{: #env_flavors}

**叢集類型**：決定您是要[單一區域、多區域或多重叢集設定](/docs/containers?topic=containers-ha_clusters#ha_clusters)。[全部六個全球 {{site.data.keyword.Bluemix_notm}} 都會地區](/docs/containers?topic=containers-regions-and-zones#zones)都會提供多區域叢集。另外也請記住，工作者節點依區域而有所不同。

**工作者節點類型**：一般而言，密集工作負載更適合在裸機實體機器上執行，而對於具成本效益的測試及開發工作，您可以選擇共用或專用共用硬體上的虛擬機器。使用裸機工作者節點，您的叢集具有 10Gbps 的網路速度以及提供更高傳輸量的超執行緒核心。虛擬機器隨附 1 Gbps 的網路速度，以及未提供超執行緒的一般核心。[請查看機器隔離及可用的特性](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)。

### 我要使用多個叢集，還是只是將更多工作者節點新增至現有叢集？
{: #env_multicluster}

您建立的叢集數目取決於工作負載、公司政策和法規，以及您要如何處理運算資源。您也可以在[容器隔離及安全](/docs/containers?topic=containers-security#container)中檢閱關於此決策的安全資訊。

**多個叢集**：您需要設定[廣域負載平衡器](/docs/containers?topic=containers-ha_clusters#multiple_clusters)，並在每個叢集裡複製及套用相同的配置 YAML 檔案，以將工作負載平衡到各叢集。因此，多個叢集的管理通常較為複雜，但可協助您達成下列重要目標。
*  符合需要您隔離工作負載的安全原則。
*  測試您的應用程式如何在不同版本的 Kubernetes 或其他叢集軟體（例如 Calico）中執行。
*  在另一個地區建立具有您應用程式的叢集，以提高該地理區域中的使用者效能。
*  在叢集實例層次配置使用者存取權，而不是自訂及管理多個 RBAC 原則，以在名稱空間層次控制叢集內的存取權。

**較少或單一叢集**：較少的叢集可協助您減少固定資源的作業工作量及每個叢集成本。您可以為可用於要使用之應用程式和服務元件的不同機型的運算資源新增工作者節點儲存區，而不是建立更多叢集。當您開發應用程式時，所使用的資源位在相同的區域中，或在多區域中緊密地連接，因此您可以針對延遲、頻寬或相關故障做出一些假設。不過，使用名稱空間、資源配額及標籤來組織叢集，變得更為重要。

### 如何在叢集內設定我的資源？
{: #env_resources}

<dl>
<dt>考量工作者節點容量。</dt>
  <dd>若要深入瞭解工作者節點的效能，請考慮下列各項：
  <ul><li><strong>保持核心強度</strong>：每部機器都有特定數量的核心。根據應用程式的工作負載，設定每個核心的 Pod 數目限制，例如 10。</li>
  <li><strong>避免節點超載</strong>：同樣地，僅因為一個節點可以包含超過 100 個 Pod 並不表示這是您想要的。根據應用程式的工作負載，設定每個節點的 Pod 數目限制，例如 40。</li>
  <li><strong>不要耗盡叢集頻寬</strong>：請記住，調整虛擬機器的網路頻寬大約是 1000 Mbps。如果叢集裡需要數百個工作者節點，請將它分割成多個具有較少節點的叢集，或訂購裸機節點。</li>
  <li><strong>分類服務</strong>：在部署之前，請先規劃工作負載所需的服務數目。網路及埠轉遞規則會放入 Iptables 中。如果您預期有大量服務（例如，超過 5,000 個服務），請將叢集分割成多個叢集。</li></ul></dd>
<dt>針對混合的運算資源，佈建不同的機型。</dt>
  <dd>每個人都喜歡選擇，是吧？使用 {{site.data.keyword.containerlong_notm}}，您會有可部署的[混合機型](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)；從用於密集工作負載的裸機機器到用於快速調整的虛擬機器。使用標籤或名稱空間，將部署組織到機器。當您建立部署時，請限制它，讓應用程式的 Pod 只會部署在具有正確混合資源的機器上。例如，建議您將資料庫應用程式限制為具有大量本端磁碟儲存空間（如 `md1c.28x512.4x4tb`）的裸機機器。</dd>
<dt>當您有多個共用叢集的團隊及專案時，請設定多個名稱空間。</dt>
  <dd><p>名稱空間有點像是叢集內的某個叢集。它們是使用[資源配額 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/concepts/policy/resource-quotas/) 及[預設限制 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/tasks/administer-cluster/memory-default-namespace/) 來分割叢集資源的一種方法。當您建立新的名稱空間時，請務必設定適當的 [RBAC 原則](/docs/containers?topic=containers-users#rbac)來控制存取權。如需相關資訊，請參閱 Kubernetes 文件中的[與名稱空間共用叢集 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/tasks/administer-cluster/namespaces/)。</p>
  <p>如果您有一個小型叢集、數十位使用者及類似的資源（例如相同軟體的不同版本），則可能不需要多個名稱空間。您可以改為使用標籤。</p></dd>
<dt>設定資源配額，讓叢集裡的使用者必須使用資源要求及限制</dt>
  <dd>若要確保每個團隊都有所需的資源可以部署服務以及在叢集裡執行應用程式，您必須設定每個名稱空間的[資源配額](https://kubernetes.io/docs/concepts/policy/resource-quotas/)。資源配額決定名稱空間的部署限制項，例如您可以部署的 Kubernetes 資源數目，以及可供那些資源耗用的 CPU 和記憶體數量。設定配額之後，使用者必須在其部署中包括資源要求和限制。</dd>
<dt>使用標籤組織 Kubernetes 物件</dt>
  <dd><p>若要組織並選取 `pods` 或 `nodes` 這類 Kubernetes 資源，請[使用 Kubernetes 標籤 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/)。依預設，{{site.data.keyword.containerlong_notm}} 會套用一些標籤，包括 `arch`、`os`、`region`、`zone` 及 `machine-type`。</p>
  <p>標籤的使用案例範例包括[將網路資料流量限制為邊緣工作者節點](/docs/containers?topic=containers-edge)、[將應用程式部署至 GPU 機器](/docs/containers?topic=containers-app#gpu_app)，以及[限制應用程式工作負載 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/) 以在工作者節點上執行，而這些工作者節點符合特定機型或 SDS 功能（例如裸機工作者節點）。若要查看已套用至資源的標籤，請使用 <code>kubectl get</code> 指令與 <code>--show-labels</code> 旗標搭配。例如：</p>
  <p><pre class="pre"><code>kubectl get node &lt;node_ID&gt; --show-labels</code></pre></p>
  若要將標籤套用至工作者節點，請套用標籤[建立工作者節點儲存區](/docs/containers?topic=containers-add_workers#add_pool)或[更新現有工作者節點儲存區](/docs/containers?topic=containers-add_workers#worker_pool_labels)。</dd>
</dl>




<br />


## 讓資源具有高可用性
{: #kube_ha}

雖然沒有系統完全具有失敗安全功能，但您可以採取幾個步驟來增加 {{site.data.keyword.containerlong_notm}} 中應用程式及服務的高可用性。
{:shortdesc}

檢閱讓資源具有高可用性的相關資訊。
* [減少潛在失敗點](/docs/containers?topic=containers-ha#ha)。
* [建立多區域叢集](/docs/containers?topic=containers-ha_clusters#ha_clusters)。
* [規劃高可用性部署](/docs/containers?topic=containers-app#highly_available_apps)，以跨多區域使用抄本集和 Pod 反親緣性這類特性。
* [執行根據以雲端為基礎的公用登錄中映像檔的容器](/docs/containers?topic=containers-images)。
* [規劃資料儲存空間](/docs/containers?topic=containers-storage_planning#persistent_storage_overview)。特別對於多區域叢集，請考量使用 [{{site.data.keyword.cloudant_short_notm}}](/docs/services/Cloudant?topic=cloudant-getting-started#getting-started) 或 [{{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-about) 這類雲端服務。
* 對於多區域叢集，啟用[負載平衡器服務](/docs/containers?topic=containers-loadbalancer#multi_zone_config)或 Ingress [多區域負載平衡器](/docs/containers?topic=containers-ingress#ingress)，以便對大眾公開應用程式。

<br />


## 設定服務探索
{: #service_discovery}

Kubernetes 叢集裡的每個 Pod 都有 IP 位址。然而，當您將應用程式部署至叢集時，並不想要根據 Pod IP 位址來進行服務探索及網路作業。頻繁且動態地移除及取代 Pod。相反地，請使用 Kubernetes 服務，它代表一組 Pod 並提供透過服務之虛擬 IP 位址（稱為其`叢集 IP`）的穩定進入點。如需相關資訊，請參閱 Kubernetes 文件中關於[服務 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/concepts/services-networking/service/#discovering-services) 的部分。
{:shortdesc}

### 是否可以自訂 Kubernetes 叢集 DNS 提供者？
{: #services_dns}

當您建立服務及 Pod 時，它們會獲指派 DNS 名稱，讓您的應用程式容器可以使用 DNS 服務 IP 來解析 DNS 名稱。您可以自訂 Pod DNS 來指定名稱伺服器、搜尋及物件清單選項。如需相關資訊，請參閱[配置叢集 DNS 提供者](/docs/containers?topic=containers-cluster_dns#cluster_dns)。
{: shortdesc}



### 如何確定我的服務連接至正確的部署，並準備好進行？
{: #services_connected}

對於大部分的服務，請將選取器新增至服務 `.yaml` 檔案，讓它適用於透過該標籤執行應用程式的 Pod。很多時候，當您的應用程式第一次啟動時，您並不希望它立即處理要求。將就緒探測新增至部署，讓資料流量只傳送至視為就緒的 Pod。如需使用標籤並設定就緒探測之服務的部署範例，請查看此 [NGINX YAML](https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/deploy-apps-clusters/nginx_preferredAntiAffinity.yaml)。
{: shortdesc}

有時，您並不希望服務使用標籤。例如，您可能有一個外部資料庫，或想要將服務指向叢集內不同名稱空間中的另一個服務。發生此情況時，您必須手動新增 endpoints 物件，並將它鏈結至服務。


### 如何控制叢集裡執行的服務之間的網路資料流量？
{: #services_network_traffic}

依預設，Pod 可以與叢集裡的其他 Pod 通訊，但是您可以使用網路原則來封鎖對特定 Pod 或名稱空間的資料流量。此外，如果您使用 NodePort、負載平衡器或 Ingress 服務向外部公開應用程式，則建議您設定進階網路原則來封鎖資料流量。在 {{site.data.keyword.containerlong_notm}} 中，您可以使用 Calico 來管理 Kubernetes 和 Calico [網路原則來控制資料流量](/docs/containers?topic=containers-network_policies#network_policies)。

如果有各種微服務在您需要連接、管理及保護網路資料流量的各平台之間執行，請考量使用服務網工具，例如[受管理 Istio 附加程式](/docs/containers?topic=containers-istio)。

您還可以[設定邊緣節點](/docs/containers?topic=containers-edge#edge)，藉由限制網路工作負載來選取工作者節點，以提高叢集的安全和隔離。



### 如何在網際網路上公開服務？
{: #services_expose_apps}

您可以針對外部網路建立三種類型的服務：NodePort、LoadBalancer 及 Ingress。如需相關資訊，請參閱[規劃網路服務](/docs/containers?topic=containers-cs_network_planning#external)。

當您規劃叢集裡需要多少 `Service` 物件時，請記住，Kubernetes 使用 `iptables` 來處理網路及埠轉遞規則。如果您在叢集裡執行大量的服務（例如 5000），則可能會影響效能。



## 將應用程式工作負載部署至叢集
{: #deployments}

使用 Kubernetes，您可以在 YAML 配置檔中宣告多種類型的物件，例如 Pod、部署及工作。這些物件說明下列這類事項：哪些容器化應用程式正在執行、它們所使用的資源，以及哪些原則管理其重新啟動、更新、抄寫等行為。如需相關資訊，請參閱 Kubernetes 文件中關於[配置最佳作法 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/concepts/configuration/overview/) 的部分。
{: shortdesc}

### 我認為需要將應用程式置於容器中。現在，要怎麼處理這些 Pod 相關項目？
{: #deploy_pods}

[Pod ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/concepts/workloads/pods/pod/) 是 Kubernetes 可管理的最小可部署單元。您可以將容器（或容器群組）放入 Pod 中，並使用 Pod 配置檔告知 Pod 如何執行容器以及與其他 Pod 共用資源。放入 Pod 中的所有容器都在共用環境定義中執行，這意味著這些容器共用相同虛擬機或實體機器。
{: shortdesc}

**放入容器中的項目**：當您思考應用程式的元件時，請考量這些元件對於 CPU 和記憶體這類項目是否有明顯不同的資源需求。部分元件能夠以最佳效能執行嗎？在這種情況下，是否可接受關閉一段時間以將資源轉移至其他區域？是否有另一個客戶端適用元件，因此，保持這一點至關重要？請將它們分割至不同的容器。您可以隨時將它們部署至相同的 Pod，讓它們同步一起執行。

**放入 Pod 中的項目**：應用程式的容器不一定都必須位於相同的 Pod。事實上，如果您的元件是有狀態的且難以調整（例如資料庫服務），則請將其放入可排定於工作者節點的不同 Pod 中，該工作者節點具有較多資源可以處理工作負載。如果在不同工作者節點上執行的容器可以正常工作，請使用多個 Pod。如果它們需要在相同的機器上並一起調整，請將容器分組到相同的 Pod 中。

### 因此，如果我只能使用 Pod，為何需要所有這些不同類型的物件？
{: #deploy_objects}

建立 Pod YAML 檔案很容易。只需要幾行就可以撰寫一個檔案，如下所示。

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx
    ports:
    - containerPort: 80
```
{: codeblock}

然而，您不想就這樣停止。如果關閉 Pod 執行所在的節點，則會同時關閉 Pod，且不會重新予以排定。請改用[部署 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) 來支援 Pod 重新排程、抄本集及漸進式更新。基本部署幾乎就像建立 Pod 一樣簡單。不過，您可以在部署 `spec` 中指定 `replicas` 和 `template`，而非自行在 `spec` 中定義容器。範本在其中具有容器的專屬 `spec`，如下所示。

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 80
```
{: codeblock}

您可以在相同的 YAML 檔案中持續新增 Pod 反親緣性或資源限制這類特性。

如需可新增至部署之不同特性的其他詳細說明，請查看[建立應用程式部署 YAML 檔案](/docs/containers?topic=containers-app#app_yaml)。
{: tip}

### 如何組織部署讓它們更容易更新及管理？
{: #deploy_organize}

既然，您已經清楚瞭解部署中要包含的項目，您可能想知道將如何管理所有這些不同的 YAML 檔案？更不用說在 Kubernetes 環境中建立的物件！

組織部署 YAML 檔案的一些提示：
*  使用版本控制系統（例如 Git）。
*  在單一 YAML 檔案內，將緊密相關的 Kubernetes 物件進行分組。例如，如果您要建立 `deployment`，則可能也會將 `service` 檔案新增至 YAML。請使用 `---` 區隔物件，如下所示：
    ```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
    ...
    ---
    apiVersion: v1
    kind: Service
    metadata:
    ...
    ```
    {: codeblock}
*  您可以使用 `kubectl apply -f` 指令來套用至整個目錄，而不只是套用至單一檔案。
*  請試用 [`kusomize` 專案](/docs/containers?topic=containers-app#kustomize)，此專案可用於協助撰寫、自訂和重複使用 Kubernetes 資源 YAML 配置。

在 YAML 檔案內，您可以使用標籤或註釋作為 meta 資料來管理部署。

**標籤**：[標籤 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/) 是可連接至 Pod 和部署這類 Kubernetes 物件的 `key:value` 配對。它們可以是您想要的任何項目，而且有助於根據標籤資訊選取物件。標籤提供用於分組物件的基礎。標籤的一些構想如下：
* `app: nginx`
* `version: v1`
* `env: dev`

**註釋**：[註釋 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/concepts/overview/working-with-objects/annotations/) 與標籤類似的部分在於它們也是 `key:value` 配對。它們適用於可供工具或程式庫運用的非識別資訊，例如保留有關物件來源、如何使用物件、相關追蹤儲存庫的指標，或物件相關原則的額外資訊。您不能根據註釋來選取物件。

### 我還能做什麼來準備應用程式進行部署？
{: #deploy_prep}

還有很多事要做！請參閱[準備容器化應用程式以在叢集裡執行](/docs/containers?topic=containers-app#plan_apps)。該主題包括下列相關資訊：
*  您可以在 Kubernetes 中執行的應用程式類型，包括有狀態及無狀態應用程式的提示。
*  將應用程式移轉至 Kubernetes。
*  根據工作負載需求來調整叢集大小。
*  設定其他應用程式資源，例如 IBM 服務、儲存空間、記載及監視。
*  在部署內使用變數。
*  控制應用程式的存取權。

<br />


## 包裝應用程式
{: #packaging}

如果您要在多個叢集、公用和專用環境或甚至多個雲端提供者中執行應用程式，您可能想知道要如何讓部署策略跨這些環境運作。使用 {{site.data.keyword.Bluemix_notm}} 及其他開放程式碼工具，您可以包裝應用程式，以協助自動化部署。
{: shortdesc}

<dl>
<dt>自動化基礎架構</dt>
  <dd>您可以使用開放程式碼 [Terraform](/docs/terraform?topic=terraform-getting-started#getting-started) 工具來自動佈建 {{site.data.keyword.Bluemix_notm}} 基礎架構，包括 Kubernetes 叢集。遵循本指導教學來[規劃、建立及更新部署環境](/docs/tutorials?topic=solution-tutorials-plan-create-update-deployments#plan-create-update-deployments)。建立叢集之後，您也可以設定 [{{site.data.keyword.containerlong_notm}} 叢集 Autoscaler](/docs/containers?topic=containers-ca)，讓工作者節點儲存區擴增及縮減工作者節點，以回應工作負載的資源要求。</dd>
<dt>設定持續整合及交付 (CI/CD) 管線</dt>
  <dd>使用您在 Git 這類來源控制管理系統中所組織的應用程式配置檔，即可建置管線，以測試程式碼並將其部署至 `test` 及 `prod` 這類不同的環境。請查看[這個有關持續部署至 Kubernetes 的指導教學](/docs/tutorials?topic=solution-tutorials-continuous-deployment-to-kubernetes#continuous-deployment-to-kubernetes)。</dd>
<dt>包裝應用程式配置檔</dt>
  <dd>使用 [Helm ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://helm.sh/docs/) Kubernetes 套件管理程式，您可以指定應用程式在 Helm 圖表中需要的所有 Kubernetes 資源。然後，您可以使用 Helm 來建立 YAML 配置檔，並在叢集裡部署這些檔案。您也可以[整合 {{site.data.keyword.Bluemix_notm}} 提供的 Helm 圖表 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/kubernetes/solutions/helm-charts) 以擴充叢集的功能，例如使用區塊儲存空間外掛程式。<p class="tip">您只是要尋找一個簡單的方式來建立 YAML 檔案範本嗎？有些人會使用 Helm 來做到這一點，或者您可以嘗試 [`ytt` ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://get-ytt.io/) 這類其他社群工具。</p></dd>
</dl>

<br />


## 讓應用程式保持最新
{: #updating}

您為了下一個版本的應用程式做了大量的準備。您可以使用 {{site.data.keyword.Bluemix_notm}} 和 Kubernetes 更新工具，確保應用程式在安全的叢集環境中執行，以及推出不同版本的應用程式。
{: shortdesc}

### 如何將我的叢集保持在受支援狀態？
{: #updating_kube}

請確定您的叢集隨時都能執行[支援的 Kubernetes 版本](/docs/containers?topic=containers-cs_versions#cs_versions)。若發行新的 Kubernetes 次要版本，之後不久就會淘汰舊版本，然後變成不受支援。如需相關資訊，請參閱[更新 Kubernetes 主節點](/docs/containers?topic=containers-update#master)和[工作者節點](/docs/containers?topic=containers-update#worker_node)。

### 我可以使用哪些應用程式更新策略？
{: #updating_apps}

若要更新應用程式，您可以從各種策略中選擇，例如下列各項。在進行更複雜的 Canary 部署之前，您可以從漸進式部署或即時切換開始。

<dl>
<dt>漸進式部署</dt>
  <dd>您可以使用 Kubernetes 原生功能來建立 `v2` 部署，並逐步取代先前的 `v1` 部署。此方式需要應用程式與舊版相容，以讓獲提供 `v2` 應用程式版本的使用者不會遇到任何重大變更。如需相關資訊，請參閱[管理漸進式部署以更新應用程式](/docs/containers?topic=containers-app#app_rolling)。</dd>
<dt>即時切換</dt>
  <dd>即時切換也稱為藍綠部署，需要將運算資源加倍，才能同時執行應用程式的兩個版本。使用此方式，您可以近乎即時地將使用者切換至較新的版本。請確定您使用服務標籤選取器（例如 `version: green` 和 `version: blue`），以確保將要求傳送至正確的應用程式版本。您可以建立新的 `version: green` 部署，並等待它就緒，然後刪除 `version: blue` 部署。或者，您可以執行[漸進式更新](/docs/containers?topic=containers-app#app_rolling)，但將 `maxUnavailable` 參數設為 `0%`，並將 `maxSurge` 參數設為 `100%`。</dd>
<dt>Canary 或 A/B 部署</dt>
  <dd>Canary 部署是更複雜的更新策略，是指您挑選一定比例（例如 5%）的使用者並將他們傳送至新的應用程式版本。您可以在記載及監視工具中收集下列作業的度量值：如何執行新的應用程式版本，執行 A/B 測試，然後將更新項目推出給更多使用者。與所有部署相同，標示應用程式（例如 `version: stable` 和 `version: canary`）十分重要。若要管理 Canary 部署，您可以[安裝受管理 Istio 附加程式服務網](/docs/containers?topic=containers-istio#istio)，[設定叢集的 Sysdig 監視](/docs/services/Monitoring-with-Sysdig/tutorials?topic=Sysdig-kubernetes_cluster#kubernetes_cluster)，然後使用 Istio 服務網進行 A/B 測試，如[本部落格文章 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://sysdig.com/blog/monitor-istio/) 所述。或者，將 Knative 用於金絲雀部署。</dd>
</dl>

<br />


## 監視叢集效能
{: #monitoring_health}

透過有效記載及監視叢集和應用程式，您可以更充分地瞭解環境，以最佳化資源使用率，並對可能發生的問題進行疑難排解。若要為叢集設定記載及監視解決方案，請參閱[記載及監視](/docs/containers?topic=containers-health#health)。
{: shortdesc}

在您設定記載及監視時，請考慮下列考量。

<dl>
<dt>收集日誌及度量值以判定叢集性能</dt>
  <dd>Kubernetes 包括度量值伺服器，以協助判斷基本叢集層次效能。您可以在 [Kubernetes 儀表板](/docs/containers?topic=containers-app#cli_dashboard)或終端機中，執行 `kubectl top (pods | nodes)` 指令來檢閱這些度量值。您可能會在自動化中包括這些指令。<br><br>
  將日誌轉遞至日誌分析工具，讓您可於稍後分析日誌。定義您要記載的日誌詳細程度及層次，以避免儲存超出所需的日誌。日誌可能會快速地佔用大量儲存空間，這會影響應用程式效能，且可能造成日誌分析更加困難。</dd>
<dt>測試應用程式效能</dt>
  <dd>在您設定記載及監視之後，請進行效能測試。在測試環境中，故意建立各種不理想的情境，例如刪除區域中的所有工作者節點以抄寫區域故障。請檢閱日誌及度量值，以檢查應用程式的回復方式。</dd>
<dt>準備審核</dt>
  <dd>除了應用程式日誌及叢集度量值之外，您還要設定活動追蹤，讓您具有可審核的記錄來記錄哪些人執行了哪些叢集和 Kubernetes 動作。如需相關資訊，請參閱 [{{site.data.keyword.cloudaccesstrailshort}}](/docs/containers?topic=containers-at_events#at_events)。</dd>
</dl>
