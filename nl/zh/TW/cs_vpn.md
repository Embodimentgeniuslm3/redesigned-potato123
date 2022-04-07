---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-10"

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


# 設定 VPN 連線功能
{: #vpn}

使用 VPN 連線功能，您可以在 {{site.data.keyword.containerlong}} 上將 Kubernetes 叢集裡的應用程式安全地連接至內部部署網路。您也可以將叢集以外的應用程式連接至叢集內執行的應用程式。
{:shortdesc}

若要將工作者節點及應用程式連接至內部部署資料中心，您可以配置下列其中一個選項。

- **strongSwan IPSec VPN 服務**：您可以設定 [strongSwan IPSec VPN 服務 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.strongswan.org/about.html)，以安全地連接 Kubernetes 叢集與內部部署網路。在根據業界標準網際網路通訊協定安全 (IPSec) 通訊協定套組的網際網路上，strongsWan IPSec VPN 服務提供安全的端對端通訊通道。若要設定叢集與內部部署網路之間的安全連線，請直接在叢集的 Pod 中[配置及部署 strongSwan IPSec VPN 服務](#vpn-setup)。

- **{{site.data.keyword.BluDirectLink}}**：[{{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-about-ibm-cloud-direct-link) 容許您在遠端網路環境和 {{site.data.keyword.containerlong_notm}} 之間建立直接專用連線，而無需透過公用網際網路進行遞送。{{site.data.keyword.Bluemix_notm}} Direct Link 供應項目適用於您必須實作混合式工作負載、跨提供者工作負載、大型或頻繁的資料傳送，或是專用工作負載等情況。若要選擇 {{site.data.keyword.Bluemix_notm}} Direct Link 供應項目並設定 {{site.data.keyword.Bluemix_notm}} Direct Link 連線，請參閱 {{site.data.keyword.Bluemix_notm}} Direct Link 文件中的 [{{site.data.keyword.Bluemix_notm}} Direct Link 開始使用](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link#how-do-i-know-which-type-of-ibm-cloud-direct-link-i-need-)。

- **Virtual Router Appliance (VRA) 或 Fortigate Security Appliance (FSA)**：您可以選擇設定 [VRA (Vyatta)](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-about-the-vra) 或 [FSA](/docs/services/vmwaresolutions/services?topic=vmware-solutions-fsa_considerations)，以配置 IPSec VPN 端點。當您有較大的叢集、想要透過單一 VPN 存取多個叢集，或需要以路徑為基礎的 VPN 時，此選項十分有用。若要配置 VRA，請參閱[使用 VRA 設定 VPN 連線功能](#vyatta)。

## 使用 strongSwan IPSec VPN 服務 Helm 圖表
{: #vpn-setup}

使用 Helm 圖表，可以在 Kubernetes Pod 中配置及部署 strongSwan IPSec VPN 服務。
{:shortdesc}

因為 strongSwan 已整合在您的叢集內，所以您不需要外部閘道裝置。建立 VPN 連線功能時，會在叢集裡的所有工作者節點上自動配置路徑。這些路線容許透過 VPN 通道在任何工作者節點與遠端系統上的 Pod 之間進行雙向連線。例如，下圖顯示 {{site.data.keyword.containerlong_notm}} 中的應用程式如何透過 strongSwan VPN 連線與內部部署伺服器通訊：

<img src="images/cs_vpn_strongswan.png" width="700" alt="使用網路負載平衡器 (NLB)，在 {{site.data.keyword.containerlong_notm}} 中公開應用程式" style="width:700px; border-style: none"/>

1. 叢集裡的應用程式 (`myapp`) 會接收來自 Ingress 或 LoadBalancer 服務的要求，且需要安全地連接至內部部署網路中的資料。

2. 向內部部署資料中心發出的要求會轉遞至 IPSec strongSwan VPN Pod。目的地 IP 位址是用來判定要傳送至 IPSec strongSwan VPN Pod 的網路封包。

3. 此要求會加密並透過 VPN 通道傳送至內部部署資料中心。

4. 送入的要求會透過內部部署防火牆，並遞送至解密所在的 VPN 通道端點（路由器）。

5. VPN 通道端點（路由器）會將要求轉遞至內部部署伺服器或大型主機，視步驟 2 中指定的目的地 IP 位址而定。必要資料會藉由相同的處理程序，透過 VPN 連線傳回至 `myapp`。

## strongSwan VPN 服務考量
{: #strongswan_limitations}

在使用 strongSwan Helm 圖表之前，請檢閱下列考量及限制。
{: shortdesc}

* strongSwan Helm 圖表需要遠端 VPN 端點啟用 NAT 遍訪。除了預設 IPSec UDP 埠 500 外，NAT 遍訪還需要 UDP 埠 4500。這兩個 UDP 埠都需要容許透過配置的任何防火牆。
* strongSwan Helm 圖表不支援以路徑為基礎的 IPSec VPN。
* strongSwan Helm 圖表支援使用預先共用金鑰的 IPSec VPN，但不支援需要憑證的 IPSec VPN。
* strongSwan Helm 圖表不容許多個叢集和其他 IaaS 資源共用單一 VPN 連線。
* strongSwan Helm 圖表會在叢集內以 Kubernetes Pod 形式執行。Kubernetes 的記憶體及網路用量，以及在叢集裡執行的其他 Pod，都會影響 VPN 效能。如果您具有效能關鍵環境，請考慮使用在叢集以外的專用硬體上執行的 VPN 解決方案。
* strongSwan Helm 圖表會執行單一 VPN Pod，作為 IPSec 通道端點。如果 Pod 失敗，叢集會重新啟動 Pod。不過，當新 Pod 啟動並重新建立 VPN 連線時，您可能遭遇短暫的關閉時間。如果您需要更快速的錯誤回復，或更精心製作的高可用性解決方案，請考慮使用在叢集以外的專用硬體上執行的 VPN 解決方案。
* strongSwan Helm 圖表不會針對流過 VPN 連線的網路資料流量提供度量值或監視。如需所支援監視工具的清單，請參閱[記載及監視服務](/docs/containers?topic=containers-supported_integrations#health_services)。

叢集使用者可以使用 strongSwan VPN 服務透過專用服務端點連接至 Kubernetes 主節點。但是，與 Kubernetes 主節點透過專用服務端點進行的通訊必須經過 <code>166.X.X.X</code> IP 位址範圍，這不能透過 VPN 連線進行遞送。可以透過[使用專用網路負載平衡器 (NLB)](/docs/containers?topic=containers-clusters#access_on_prem) 來公開叢集使用者的主節點的專用服務端點。專用 NLB 將主節點的專用服務端點作為 strongSwan VPN Pod 可以存取的內部 `172.21.x.x` 叢集 IP 位址公開。如果您只啟用專用服務端點，可以使用 Kubernetes 儀表板或暫時啟用公用服務端點以建立專用 NLB。
{: tip}

<br />


## 在多區域叢集裡配置 strongSwan VPN
{: #vpn_multizone}

藉由讓應用程式實例可用於多個區域的工作者節點，多區域叢集便可在發生中斷時提供應用程式高可用性。不過，在多區域叢集裡配置 strongSwan VPN 服務比在單一區域叢集裡配置 strongSwan 更為複雜。
{: shortdesc}

在多區域叢集裡配置 strongSwan 之前，請先嘗試將 strongSwan Helm 圖表部署至單一區域叢集。第一次在單一區域叢集與內部部署網路之間建立 VPN 連線時，您可以更輕鬆地判斷對於多區域 strongSwan 配置而言非常重要的遠端網路防火牆設定：
* 有些遠端 VPN 端點在 `ipsec.conf` 檔案中，有 `leftid` 或 `rightid` 等設定。如果您有這些設定，請檢查是否必須將 `leftid` 設為 VPN IPSec 通道的 IP 位址。
*	如果連線是從遠端網路入埠連至叢集，請檢查遠端 VPN 端點是否可以在一個區域發生負載平衡器故障時，重新建立連至不同 IP 位址的 VPN 連線。

若要在多區域叢集裡開始使用 strongSwan，請選擇下列其中一個選項。
* 如果您可以使用出埠 VPN 連線，則可以選擇僅配置一個 strongSwan VPN 部署。請參閱[配置來自多區域叢集的出埠 VPN 連線](#multizone_one_outbound)。
* 如果您需要入埠 VPN 連線，則您可以使用的配置設定，會根據遠端 VPN 端點是否可以配置為在偵測到中斷時，重新建立連至不同公用負載平衡器 IP 的 VPN 連線而改變。
  * 如果遠端 VPN 端點可以自動重新建立連至不同 IP 的 VPN 連線，則您可以選擇僅配置一個 strongSwan VPN 部署。請參閱[配置連至多區域叢集的入埠 VPN 連線](#multizone_one_inbound)。
  * 如果遠端 VPN 端點無法自動重新建立連至不同 IP 的 VPN 連線，則您必須在每個區域中部署個別的入埠 strongSwan VPN 服務。請參閱[在多區域叢集的每個區域中配置 VPN 連線](#multizone_multiple)。

嘗試設定您的環境，以便您只需要一個 strongSwan VPN 部署，便可建立連至多區域叢集的出埠或入埠 VPN 連線。如果您必須在每個區域中設定個別的 strongSwan VPN，請務必計劃如何管理因此而增加的複雜度以及提高的資源用量。
{: note}

### 配置來自多區域叢集的單一出埠 VPN 連線
{: #multizone_one_outbound}

在多區域叢集裡配置 strongSwan VPN 服務最簡單的解決方案，是使用單一出埠 VPN 連線，該連線會在叢集裡所有可用性區域中的不同工作者節點之間浮動。
{: shortdesc}

當 VPN 連線從多區域叢集出埠時，只需要一個 strongSwan 部署。如果工作者節點已移除或遇到關閉時間，則 `kubelet` 會將 VPN Pod 重新排定到新的工作者節點。如果可用性區域遇到中斷，則 `kubelet` 會將 VPN Pod 重新排定到不同區域的新工作者節點上。

1. [配置 strongSwan VPN Helm 圖表](/docs/containers?topic=containers-vpn#vpn_configure)。當您遵循該區段中的步驟時，請務必指定下列設定：
    - `ipsec.auto`：變更為 `start`。連線是來自叢集的出埠連線。
    - `loadBalancerIP`：請勿指定 IP 位址。將此設定保留空白。
    - `zoneLoadBalancer`：為您有工作者節點的每個區域，指定公用負載平衡器 IP 位址。[您可以查看可用的公用 IP 位址](/docs/containers?topic=containers-subnets#review_ip)或[釋放已使用的 IP 位址](/docs/containers?topic=containers-subnets#free)。因為 strongSwan VPN Pod 可以排定到任何區域中的工作者節點，因此這份 IP 清單可確保負載平衡器 IP 可用於已排定 VPN Pod 的任何區域。
    - `connectUsingLoadBalancerIP`：設為 `true`。當 strongSwan VPN Pod 排定到工作者節點時，strongSwan 服務會選取位於相同區域的負載平衡器 IP 位址，並使用此 IP 來建立出埠連線。
    - `local.id`：指定遠端 VPN 端點所支援的固定值。如果遠端 VPN 端點需要您將 `local.id` 選項（`ipsec.conf` 中的 `leftid` 值）設為 VPN IPSec 通道的公用 IP 位址，請將 `local.id` 設為 `%loadBalancerIP`。此值會自動將 `ipsec.conf` 中的 `leftid` 值配置為用於連線的負載平衡器 IP 位址。

2. 在遠端網路防火牆中，容許送入的 IPSec VPN 連線，該連線來自您在 `zoneLoadBalancer` 設定中所列出的公用 IP 位址。

3. 將遠端 VPN 端點配置為容許送入的 VPN 連線，該連線來自您在 `zoneLoadBalancer` 設定中所列出的每個可能負載平衡器 IP。

### 配置連至多區域叢集的單一入埠 VPN 連線
{: #multizone_one_inbound}

當您需要送入的 VPN 連線，且遠端 VPN 端點在偵測到失敗時，可以自動重新建立連至不同 IP 的 VPN 連線時，您可以使用在叢集裡所有可用性區域中的不同工作者節點之間浮動的單一入埠 VPN 連線。
{: shortdesc}

遠端 VPN 端點可以建立連至任何區域中任何 strongSwan 負載平衡器的 VPN 連線。不管 VPN Pod 位於哪一個區域，送入的要求都會傳送至 VPN Pod。來自 VPN Pod 的回應會透過原始負載平衡器傳回遠端 VPN 端點。此選項可確保高可用性，因為 `kubelet` 會在工作者節點已移除或遇到關閉時間時，將 VPN Pod 重新排定到新的工作者節點。此外，如果可用性區域遇到中斷，遠端 VPN 端點可以重新建立連至不同區域中負載平衡器 IP 位址的 VPN 連線，以便仍然可以連接到 VPN Pod。

1. [配置 strongSwan VPN Helm 圖表](/docs/containers?topic=containers-vpn#vpn_configure)。當您遵循該區段中的步驟時，請務必指定下列設定：
    - `ipsec.auto`：變更為 `add`。連線是入埠連至叢集。
    - `loadBalancerIP`：請勿指定 IP 位址。將此設定保留空白。
    - `zoneLoadBalancer`：為您有工作者節點的每個區域，指定公用負載平衡器 IP 位址。[您可以查看可用的公用 IP 位址](/docs/containers?topic=containers-subnets#review_ip)或[釋放已使用的 IP 位址](/docs/containers?topic=containers-subnets#free)。
    - `local.id`：如果遠端 VPN 端點需要您將 `local.id` 選項（`ipsec.conf` 中的 `leftid` 值）設為 VPN IPSec 通道的公用 IP 位址，請將 `local.id` 設為 `%loadBalancerIP`。此值會自動將 `ipsec.conf` 中的 `leftid` 值配置為用於連線的負載平衡器 IP 位址。

2. 在遠端網路防火牆中，容許送出的 IPSec VPN 連線連至您在 `zoneLoadBalancer` 設定中所列出的公用 IP 位址。

### 在多區域叢集的每個區域中配置入埠 VPN 連線
{: #multizone_multiple}

當您需要送入的 VPN 連線，且遠端 VPN 端點無法重新建立連至不同 IP 的 VPN 連線時，您必須在每個區域中部署個別的 strongSwan VPN 服務。
{: shortdesc}

遠端 VPN 端點必須更新，才能建立連至每個區域中負載平衡器的個別 VPN 連線。此外，您必須在遠端 VPN 端點上配置區域特定的設定，讓每個 VPN 連線都是唯一的。請確保這些送入的 VPN 連線隨時保持作用中狀態。

部署每個 Helm 圖表之後，每個 strongSwan VPN 部署都會在正確的區域中，啟動成為 Kubernetes 負載平衡器服務。對該公用 IP 的送入要求會轉遞至也配置在相同區域中的 VPN Pod。如果區域遇到中斷，並不會影響在其他區域中建立的 VPN 連線。

1. 針對每個區域[配置 strongSwan VPN Helm 圖表](/docs/containers?topic=containers-vpn#vpn_configure)。當您遵循該區段中的步驟時，請務必指定下列設定：
    - `loadBalancerIP`：指定可用的公用負載平衡器 IP 位址，其位於您在其中部署此 strongSwan 服務的區域。[您可以查看可用的公用 IP 位址](/docs/containers?topic=containers-subnets#review_ip)或[釋放已使用的 IP 位址](/docs/containers?topic=containers-subnets#free)。
    - `zoneSelector`：指定您要在其中排定 VPN Pod 的區域。
    - 可能需要其他設定，例如 `zoneSpecificRoutes`、`remoteSubnetNAT`、`localSubnetNAT` 或 `enableSingleSourceIP`，視必須可以透過 VPN 進行存取的資源而定。如需詳細資料，請參閱下一步。

2. 在 VPN 通道的兩端配置區域特定的設定，以確保每個 VPN 連線都是唯一的。視必須可以透過 VPN 存取哪些資源而定，您有兩個選項可以讓連線可供區分：
    * 如果叢集裡的 Pod 必須存取遠端內部部署網路上的服務，請執行下列動作：
      - `zoneSpecificRoutes`：設為 `true`。此設定會將 VPN 連線限制為針對叢集裡的單一區域。特定區域中的 Pod 只會使用針對該特定區域設定的 VPN 連線。此解決方案可以減少在多區域叢集裡支援多個 VPN 所需的 strongSwan Pod 數量，進而改善 VPN 效能，因為 VPN 資料流量只會流向位於現行區域中的工作者節點，此解決方案也能確保每個區域的 VPN 連線功能不受 VPN 連線功能、Pod 損毀或其他區域的區域中斷等所影響。請注意，您不需要配置 `remoteSubnetNAT`。因為遞送是以每個區域為基準進行設定，所以使用 `zoneSpecificRoutes` 設定的多個 VPN 可以有相同的 `remote.subnet`。
      - `enableSingleSourceIP`：設為 `true`，並將 `local.subnet` 設為單一 /32 IP 位址。此設定組合會將所有叢集專用 IP 位址隱藏在單一 /32 IP 位址後面。這個唯一的 /32 IP 位址容許遠端內部部署網路透過正確的 VPN 連線，將回覆傳送回叢集裡起始要求的正確 Pod。請注意，針對 `local.subnet` 選項所配置的單一 /32 IP 位址，在每個 strongSwan VPN 配置中都必須是唯一的。
    * 如果遠端內部部署網路中的應用程式必須存取叢集裡的服務，請執行下列動作：    
      - `localSubnetNAT`：確保遠端內部部署網路中的應用程式可以選取特定的 VPN 連線，以傳送資料流量到叢集和從叢集接收資料流量。在每個 strongSwan Helm 配置中，使用 `localSubnetNAT` 唯一地識別可由遠端內部部署應用程式存取的叢集資源。因為已建立多個從遠端內部部署網路連至叢集的 VPN，所以您必須在內部部署網路上將邏輯新增至應用程式，讓它可以選取在存取叢集裡的服務時要使用哪個 VPN。請注意，叢集裡的服務可透過多個不同的子網路進行存取，具體取決於在每個 strongSwan VPN 配置中為 `localSubnetNAT` 配置的內容。
      - `remoteSubnetNAT`：請確定叢集裡的 Pod 使用相同的 VPN 連線，將資料流量傳回遠端網路。在每個 strongSwan 部署檔案中，使用 `remoteSubnetNAT` 設定將遠端內部部署子網路對映到唯一子網路。叢集裡的 Pod 從 VPN 特定的 `remoteSubnetNAT` 收到的資料流量會傳送回相同的 VPN 特定 `remoteSubnetNAT`，然後透過該相同 VPN 連線傳送。

3. 配置遠端 VPN 端點軟體，以建立連至每個區域中負載平衡器 IP 的個別 VPN 連線。

<br />


## 配置 strongSwan Helm 圖表
{: #vpn_configure}

在安裝 strongSwan Helm 圖表之前，您必須決定 strongSwan 配置。
{: shortdesc}

開始之前：
* 在內部部署資料中心內安裝 IPSec VPN 閘道。
* 確定您具有 `default` 名稱空間的[**撰寫者**或**管理員** {{site.data.keyword.Bluemix_notm}} IAM 服務角色](/docs/containers?topic=containers-users#platform)。
* [登入您的帳戶。適用的話，請將適當的資源群組設為目標。設定叢集的環境定義。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
  * **附註**：標準叢集裡允許所有 strongSwan 配置。如果您使用免費叢集，則在[步驟 3](#strongswan_3) 只能選擇出埠 VPN 連線。入埠 VPN 連線需要叢集裡的負載平衡器，而免費叢集無法使用負載平衡器。

### 步驟 1：取得 strongSwan Helm 圖表
{: #strongswan_1}

安裝 Helm 並取得 strongSwan Helm 圖表，以檢視可能的配置。
{: shortdesc}

1.  [遵循指示](/docs/containers?topic=containers-helm#public_helm_install)，在您的本端機器上安裝 Helm 用戶端、使用服務帳戶安裝 Helm 伺服器 (Tiller)，以及新增 {{site.data.keyword.Bluemix_notm}} Helm 儲存庫。請注意，需要 Helm 2.8 版或更新版本。

2.  驗證已使用服務帳戶安裝 Tiller。

    ```
    kubectl get serviceaccount -n kube-system | grep tiller
    ```
    {: pre}

    輸出範例：

    ```
    NAME                                 SECRETS   AGE
    tiller                               1         2m
    ```
    {: screen}

3. 將 strongSwan Helm 圖表的預設配置設定儲存在本端 YAML 檔案中。

    ```
    helm inspect values iks-charts/strongswan > config.yaml
    ```
    {: pre}

4. 開啟 `config.yaml` 檔案。

### 步驟 2：配置基本 IPSec 設定
{: #strongswan_2}

若要控制 VPN 連線的建立，請修改下列基本 IPSec 設定。
{: shortdesc}

如需每一個設定的相關資訊，請閱讀 `config.yaml` 檔案內提供的 Helm 圖表文件。
{: tip}

1. 如果您的內部部署 VPN 通道端點不支援使用 `ikev2` 作為起始設定連線的通訊協定，請將 `ipsec.keyexchange` 的值變更為 `ikev1`。
2. 將 `ipsec.esp` 設為內部部署 VPN 通道端點用於連線之 ESP 加密及鑑別演算法的清單。
    * 如果將 `ipsec.keyexchange` 設為 `ikev1`，則必須指定此設定。
    * 如果將 `ipsec.keyexchange` 設為 `ikev2`，則此設定為選用項目。
    * 如果您將此設定保留空白，則連線會使用預設 strongSwan 演算法 `aes128-sha1,3des-sha1`。
3. 將 `ipsec.ike` 設為內部部署 VPN 通道端點用於連線之 IKE/ISAKMP SA 加密及鑑別演算法的清單。這些演算法必須以特定格式 `encryption-integrity[-prf]-dhgroup` 表示。
    * 如果將 `ipsec.keyexchange` 設為 `ikev1`，則必須指定此設定。
    * 如果將 `ipsec.keyexchange` 設為 `ikev2`，則此設定為選用項目。
    * 如果您將此設定保留空白，則連線會使用預設 strongSwan 演算法 `aes128-sha1-modp2048,3des-sha1-modp1536`。
4. 將 `local.id` 的值變更為您要用來識別 VPN 通道端點所用本端 Kubernetes 叢集端的任何字串。預設值為 `ibm-cloud`。部分 VPN 實作需要您針對本端端點使用公用 IP 位址。
5. 將 `remote.id` 的值變更為您要用來識別 VPN 通道端點所用遠端內部部署端的任何字串。預設值為 `on-prem`。部分 VPN 實作需要您針對遠端端點使用公用 IP 位址。
6. 將 `preshared.secret` 的值變更為內部部署 VPN 通道端點閘道用於連線的預先共用密碼。此值儲存在 `ipsec.secrets` 中。
7. 選用項目：將 `remote.privateIPtoPing` 設為遠端子網路中的任何專用 IP 位址，以便在 Helm 連線功能驗證測試之中進行連線測試。

### 步驟 3：選取入埠或出埠 VPN 連線
{: #strongswan_3}

配置 strongSwan VPN 連線時，您可以選擇 VPN 連線是要入埠連至叢集，還是從叢集出埠。
{: shortdesc}

<dl>
<dt>入埠</dt>
<dd>來自遠端網路的內部部署 VPN 端點會起始 VPN 連線，而叢集會接聽連線。</dd>
<dt>出埠</dt>
<dd>叢集會起始 VPN 連線，而來自遠端網路的內部部署 VPN 端點會接聽連線。</dd>
</dl>

如果您使用免費叢集，則只能選擇出埠 VPN 連線。入埠 VPN 連線需要叢集裡的負載平衡器，而免費叢集無法使用負載平衡器。

若要建立入埠 VPN 連線，請修改下列設定：
1. 驗證 `ipsec.auto` 已設為 `add`。
2. 選用項目：將 `loadBalancerIP` 設為 strongSwan VPN 服務的可攜式公用 IP 位址。當您需要穩定的 IP 位址時（例如，當您必須指定允許哪些 IP 位址透過內部部署防火牆時），指定 IP 位址很有用。叢集必須至少有一個可用的公用負載平衡器 IP 位址。[您可以查看可用的公用 IP 位址](/docs/containers?topic=containers-subnets#review_ip)或[釋放已使用的 IP 位址](/docs/containers?topic=containers-subnets#free)。
    * 如果將此設定保留空白，則會使用其中一個可用的可攜式公用 IP 位址。
    * 您也必須配置針對內部部署 VPN 端點上的叢集 VPN 端點選取的公用 IP 位址，或指派給它的公用 IP 位址。

若要建立出埠 VPN 連線，請修改下列設定：
1. 將 `ipsec.auto` 變更為 `start`。
2. 將 `remote.gateway` 設為遠端網路中內部部署 VPN 端點的公用 IP 位址。
3. 為叢集 VPN 端點的 IP 位址選擇下列其中一個選項：
    * **叢集專用閘道的公用 IP 位址**：如果您的工作者節點僅連接至專用 VLAN，則出埠 VPN 要求會透過專用閘道來遞送，以到達網際網路。專用閘道的公用 IP 位址用於 VPN 連線。
    * **strongSwan Pod 執行所在之工作者節點的公用 IP 位址**：如果 strongSwan Pod 執行所在的工作者節點已連接至公用 VLAN，則工作者節點的公用 IP 位址會用於 VPN 連線。<br>
        * 如果 strongSwan Pod 已遭刪除，並重新排定至叢集裡的不同工作者節點，則 VPN 的公用 IP 位址會變更。遠端網路的內部部署 VPN 端點必須容許從任何叢集工作者節點的公用 IP 位址建立 VPN 連線。
        * 如果遠端 VPN 端點無法處理來自多個公用 IP 位址的 VPN 連線，請限制 strongSwan VPN Pod 部署至的節點。將 `nodeSelector` 設為特定工作者節點的 IP 位址或工作者節點標籤。例如，值 `kubernetes.io/hostname: 10.232.xx.xx` 容許 VPN Pod 僅部署至該工作者節點。`strongswan: vpn` 值將 VPN Pod 限制為只能在具有該標籤的任何工作者節點上執行。您可以使用任何工作者節點標籤。若要容許不同的工作者節點與不同的 Helm 圖表部署搭配使用，請使用 `strongswan: <release_name>`。如需高可用性，請至少選取兩個工作者節點。
    * **strongSwan 服務的公用 IP 位址**：若要使用 strongSwan VPN 服務的 IP 位址來建立連線，請將 `connectUsingLoadBalancerIP` 設為 `true`。strongSwan 服務 IP 位址是您可在 `loadBalancerIP` 設定中指定的可攜式公用 IP 位址，或是自動指派給服務的可用可攜式公用 IP 位址。<br>
        * 如果您選擇使用 `loadBalancerIP` 設定來選取 IP 位址，則叢集必須至少具有一個可用的公用負載平衡器 IP 位址。[您可以查看可用的公用 IP 位址](/docs/containers?topic=containers-subnets#review_ip)或[釋放已使用的 IP 位址](/docs/containers?topic=containers-subnets#free)。
        * 所有叢集工作者節點都必須位於相同的公用 VLAN 上。否則，您必須使用 `nodeSelector` 設定，以確保 VPN Pod 部署至與 `loadBalancerIP` 相同公用 VLAN 上的工作者節點。
        * 如果 `connectUsingLoadBalancerIP` 設為 `true`，且 `ipsec.keyexchange` 設為 `ikev1`，則您必須將 `enableServiceSourceIP` 設為 `true`。

### 步驟 4：透過 VPN 連線存取叢集資源
{: #strongswan_4}

決定哪些叢集資源必須可由遠端網路透過 VPN 連線存取。
{: shortdesc}

1. 將一個以上的叢集子網路的 CIDR 新增至 `local.subnet` 設定。您必須在內部部署 VPN 端點上配置本端子網路 CIDR。此清單可能包括下列子網路：  
    * Kubernetes Pod 子網路 CIDR：`172.30.0.0/16`。在所有叢集 Pod 與遠端網路子網路（列在 `remote.subnet` 設定中）中的任何主機之間，會啟用雙向通訊。如果基於安全考量，您必須防止任何 `remote.subnet` 主機存取叢集 Pod，請不要將 Kubernetes Pod 子網路新增至 `local.subnet` 設定。
    * Kubernetes 服務子網路 CIDR：`172.21.0.0/16`。服務 IP 位址會提供一種公開多個應用程式 Pod 的方法，而這些應用程式 Pod 是部署在單一 IP 後面的數個工作者節點上。
    * 如果您的應用程式是由專用網路上的 NodePort 服務或專用 Ingress ALB 公開，請新增工作者節點的專用子網路 CIDR。藉由執行 `ibmcloud ks worker <cluster_name>`，來擷取工作者節點專用 IP 位址的前三個八位元組。比方說，如果它是 `10.176.48.xx`，請注意 `10.176.48`。接下來，執行下列指令，將 `<xxx.yyy.zz>` 取代為您先前擷取的八位元組，來取得工作者節點專用子網路 CIDR：`ibmcloud sl subnet list | grep <xxx.yyy.zzz>`。**附註**：如果在新的專用子網路上新增工作者節點，則您必須將新的專用子網路 CIDR 新增至 `local.subnet` 設定及內部部署 VPN 端點。然後，必須重新啟動 VPN 連線。
    * 如果您的應用程式是由專用網路上的 LoadBalancer 服務公開，請新增叢集的專用使用者管理子網路 CIDR。若要尋找這些值，請執行 `ibmcloud ks cluster-get --cluster <cluster_name> --showResources`。在 **VLAN** 區段中，尋找 **Public** 值為 `false` 的 CIDR。**附註**：如果將 `ipsec.keyexchange` 設為 `ikev1`，則您只能指定一個子網路。不過，您可以使用 `localSubnetNAT` 設定，將多個叢集子網路結合成單一子網路。

2. 選用項目：使用 `localSubnetNAT` 設定來重新對映叢集子網路。子網路的「網址轉換 (NAT)」能針對叢集網路與遠端內部部署網路之間的子網路衝突，提供暫行解決方法。您可以使用 NAT，將叢集的專用本端 IP 子網路、Pod 子網路 (172.30.0.0/16) 或 Pod 服務子網路 (172.21.0.0/16) 重新對映至不同的專用子網路。VPN 通道會看到重新對映的 IP 子網路，而非原始子網路。重新對映發生在封包透過 VPN 通道傳送之前，以及封包從 VPN 通道抵達之後。您可以透過 VPN 同時公開重新對映及未重新對映的子網路。若要啟用 NAT，您可以新增整個子網路或新增個別 IP 位址。
    * 如果您以格式 `10.171.42.0/24=10.10.10.0/24` 新增整個子網路，則重新對映是 1 對 1：內部網路子網路中的所有 IP 位址都會對映到外部網路子網路，反之亦然。
    * 如果您以格式 `10.171.42.17/32=10.10.10.2/32,10.171.42.29/32=10.10.10.3/32` 新增個別 IP 位址，則只有那些內部 IP 位址會對映到指定的外部 IP 位址。

3. 2.2.0 版以及更新版本 strongSwan Helm 圖表的選用項目：藉由將 `enableSingleSourceIP` 設為 `true`，將所有叢集 IP 位址隱藏在單一 IP 位址之後。此選項為 VPN 連線提供其中一個最安全的配置，因為不允許從遠端網路回到叢集的任何連線。<br>
    * 此設定要求流經 VPN 連線的所有資料流程都必須是出埠流程，無論 VPN 連線是從叢集建立，還是從遠端網路建立。
    * `local.subnet` 必須僅設為某個 /32 子網路。

4. 2.2.0 版以及更新版本 strongSwan Helm 圖表的選用項目：使用 `localNonClusterSubnet` 設定，讓 strongSwan 服務可將送入的要求從遠端網路遞送至存在於叢集以外的服務。<br>
    * 非叢集服務必須存在於相同的專用網路上，或工作者節點可以連接的專用網路上。
    * 非叢集工作者節點無法起始透過 VPN 連線送至遠端網路的資料流量，但非叢集節點可以是來自遠端網路的送入要求目標。
    * 您必須在 `local.subnet` 設定中列出非叢集子網路的 CIDR。

### 步驟 5：透過 VPN 連線存取遠端網路資源
{: #strongswan_5}

決定哪些遠端網路資源必須可由叢集透過 VPN 連線存取。
{: shortdesc}

1. 將一個以上的內部部署專用子網路的 CIDR 新增至 `remote.subnet` 設定。
    **附註**：如果將 `ipsec.keyexchange` 設為 `ikev1`，則您只能指定一個子網路。
2. 2.2.0 版以及更新版本 strongSwan Helm 圖表的選用項目：使用 `remoteSubnetNAT` 設定來重新對映遠端網路子網路。子網路的「網址轉換 (NAT)」能針對叢集網路與遠端內部部署網路之間的子網路衝突，提供暫行解決方法。您可以使用 NAT，將遠端網路的 IP 子網路重新對映至不同的專用子網路。重新對映發生在封包透過 VPN 通道傳送之前。叢集裡的 Pod 會看到重新對映的 IP 子網路，而非原始子網路。在 Pod 透過 VPN 通道傳回資料之前，重新對映的 IP 子網路會切換回遠端網路正在使用的實際子網路。您可以透過 VPN 同時公開重新對映及未重新對映的子網路。

### 步驟 6（選用）：使用 Slack Webhook 整合啟用監視
{: #strongswan_6}

若要監視 strongSwan VPN 的狀態，您可以設定 Webhook 將 VPN 連線功能訊息自動張貼到 Slack 頻道。
{: shortdesc}

1. 登入 Slack 工作區。

2. 移至[送入的 WebHooks 應用程式頁面 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://slack.com/apps/A0F7XDUAZ-incoming-webhooks)。

3. 按一下**要求安裝**。如果此應用程式未列在 Slack 設定中，請聯絡 Slack 工作區擁有者。

4. 在核准安裝要求之後，請按一下**新增配置**。

5. 選擇 Slack 頻道，或建立新通道以將 VPN 訊息傳送至其中。

6. 複製所產生的 Webhook URL。此 URL 格式與下列類似：
  ```
  https://hooks.slack.com/services/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
  ```
  {: screen}

7. 若要驗證已安裝 Slack Webhook，請執行下列指令，將測試訊息傳送至 Webhook URL：
    ```
    curl -X POST -H 'Content-type: application/json' -d '{"text":"VPN test message"}' <webhook_URL>
    ```
    {: pre}

8. 移至您選擇的 Slack 頻道，以驗證測試訊息已成功。

9. 在 Helm 圖表的 `config.yaml` 檔案中，配置 Webhook 來監視您的 VPN 連線。
    1. 將 `monitoring.enable` 變更為 `true`。
    2. 在遠端子網路中，新增專用 IP 位址或 HTTP 端點，您想要確保它們可透過 `monitoring.privateIPs` 或 `monitoring.httpEndpoints` 的 VPN 連線來聯絡。例如，您可以將 `remote.privateIPtoPing` 設定中的 IP 新增至 `monitoring.privateIPs`。
    3. 將 Webhook URL 新增至 `monitoring.slackWebhook`。
    4. 視需要變更其他選用的 `monitoring` 設定。

### 步驟 7：部署 Helm 圖表
{: #strongswan_7}

在您的叢集裡，使用您先前選擇的配置來部署 Helm 圖表。
{: shortdesc}

1. 如果您需要配置其他進階設定，請遵循提供的文件以瞭解 Helm 圖表中的每一個設定。

3. 儲存已更新的 `config.yaml` 檔案。

4. 使用已更新的 `config.yaml` 檔案，將 Helm 圖表安裝至叢集。

    如果單一叢集裡有多個 VPN 部署，您可以選擇比 `vpn` 更具描述性的版本名稱，以避免命名衝突並區分您的部署。為了避免截斷版本名稱，請將版本名稱限制為 35 個字元以內。
    {: tip}

    ```
    helm install -f config.yaml --name=vpn iks-charts/strongswan
    ```
    {: pre}

5. 檢查圖表部署狀態。圖表就緒時，輸出頂端附近的 **STATUS** 欄位會具有 `DEPLOYED` 值。

    ```
    helm status vpn
    ```
    {: pre}

6. 部署圖表之後，請驗證已使用 `config.yaml` 檔案中的已更新設定。

    ```
    helm get values vpn
    ```
    {: pre}

## 測試並驗證 strongSwan VPN 連線功能
{: #vpn_test}

部署 Helm 圖表之後，請測試 VPN 連線功能。
{:shortdesc}

1. 如果內部部署閘道上的 VPN 不在作用中，請啟動 VPN。

2. 設定 `STRONGSWAN_POD` 環境變數。

    ```
    export STRONGSWAN_POD=$(kubectl get pod -l app=strongswan,release=vpn -o jsonpath='{ .items[0].metadata.name }')
    ```
    {: pre}

3. 檢查 VPN 的狀態。`ESTABLISHED` 狀態表示 VPN 連線成功。

    ```
    kubectl exec $STRONGSWAN_POD -- ipsec status
    ```
    {: pre}

    輸出範例：

    ```
    Security Associations (1 up, 0 connecting):
    k8s-conn[1]: ESTABLISHED 17 minutes ago, 172.30.xxx.xxx[ibm-cloud]...192.xxx.xxx.xxx[on-premises]
    k8s-conn{2}: INSTALLED, TUNNEL, reqid 12, ESP in UDP SPIs: c78cb6b1_i c5d0d1c3_o
    k8s-conn{2}: 172.21.0.0/16 172.30.0.0/16 === 10.91.152.xxx/26
    ```
    {: screen}

    * 當您嘗試使用 strongSwan Helm 圖表建立 VPN 連線功能時，有可能第一次的 VPN 狀態不是 `ESTABLISHED`。您可能需要檢查內部部署 VPN 端點設定，並數次變更配置檔，連線才會成功：
        1. 執行 `helm delete --purge <release_name>`
        2. 修正配置檔中不正確的值。
        3. 執行 `helm install -f config.yaml --name=<release_name> ibm/strongswan`
      您也可以在下一步執行其他檢查。

    * 如果 VPN Pod 處於 `ERROR` 狀況，或持續當機並重新啟動，則可能是由於圖表的 ConfigMap 中 `ipsec.conf` 設定的參數驗證所造成。
        1. 執行 `kubectl logs $STRONGSWAN_POD`，檢查 strongSwan Pod 日誌以尋找任何驗證錯誤。
        2. 如果有驗證錯誤，請執行 `helm delete --purge <release_name>`
        3. 修正配置檔中不正確的值。
        4. 執行 `helm install -f config.yaml --name=<release_name> ibm/strongswan`

4. 您可以藉由執行內含在 strongSwan 圖表定義中的五個 Helm 測試，來進一步測試 VPN 連線功能。

    ```
    helm test vpn
    ```
    {: pre}

    * 如果所有的測試都透過，您的 strongSwan VPN 連線便已順利設定。
    * 如果有任何測試失敗，請繼續下一步。

5. 查看測試 Pod 的日誌，以檢視失敗測試的輸出。

    ```
    kubectl logs <test_program>
    ```
    {: pre}

    部分測試的需求是 VPN 配置中的選用設定。如果某些測試失敗，則根據您是否指定這些選用設定，失敗也許是可接受的。如需每一個測試及其失敗原因的相關資訊，請參閱下表。
    {: note}

    {: #vpn_tests_table}
    <table>
    <caption>瞭解 Helm VPN 連線功能測試</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解 Helm VPN 連線功能測試</th>
    </thead>
    <tbody>
    <tr>
    <td><code>vpn-strongswan-check-config</code></td>
    <td>驗證從 <code>config.yaml</code> 檔案產生的 <code>ipsec.conf</code> 檔案的語法。此測試可能由於 <code>config.yaml</code> 檔案中的值不正確而失敗。</td>
    </tr>
    <tr>
    <td><code>vpn-strongswan-check-state</code></td>
    <td>確認 VPN 連線的狀態為 <code>ESTABLISHED</code>。此測試可能因下列原因而失敗：<ul><li><code>config.yaml</code> 檔案中的值與內部部署 VPN 端點設定之間的差異。</li><li>如果叢集處於「接聽」模式（<code>ipsec.auto</code> 設為 <code>add</code>），則不會在內部部署端建立連線。</li></ul></td>
    </tr>
    <tr>
    <td><code>vpn-strongswan-ping-remote-gw</code></td>
    <td>對您在 <code>config.yaml</code> 檔案中配置的 <code>remote.gateway</code> 公用 IP 位址進行連線測試。如果 VPN 連線具有 <code>ESTABLISHED</code> 狀態，則您可以忽略此測試的結果。如果 VPN 連線沒有 <code>ESTABLISHED</code> 狀態，則此測試可能由於下列原因而失敗：<ul><li>您未指定內部部署 VPN 閘道 IP 位址。如果 <code>ipsec.auto</code> 是設為 <code>start</code>，則需要 <code>remote.gateway</code> IP 位址。</li><li>ICMP（連線測試）封包遭到防火牆封鎖。</li></ul></td>
    </tr>
    <tr>
    <td><code>vpn-strongswan-ping-remote-ip-1</code></td>
    <td>從叢集裡的 VPN Pod，對內部部署 VPN 閘道的 <code>remote.privateIPtoPing</code> 專用 IP 位址進行連線測試。此測試可能因下列原因而失敗：<ul><li>您未指定 <code>remote.privateIPtoPing</code> IP 位址。如果您是故意不指定 IP 位址，則此失敗是可接受的。</li><li>您未在 <code>local.subnet</code> 清單中指定叢集 Pod 子網路 CIDR <code>172.30.0.0/16</code>。</li></ul></td>
    </tr>
    <tr>
    <td><code>vpn-strongswan-ping-remote-ip-2</code></td>
    <td>從叢集裡的工作者節點，對內部部署 VPN 閘道的 <code>remote.privateIPtoPing</code> 專用 IP 位址進行連線測試。此測試可能因下列原因而失敗：<ul><li>您未指定 <code>remote.privateIPtoPing</code> IP 位址。如果您是故意不指定 IP 位址，則此失敗是可接受的。</li><li>您未在 <code>local.subnet</code> 清單中指定叢集工作者節點專用子網路 CIDR。</li></ul></td>
    </tr>
    </tbody></table>

6. 刪除現行 Helm 圖表。

    ```
    helm delete --purge vpn
    ```
    {: pre}

7. 開啟 `config.yaml` 檔案，並修正不正確的值。

8. 儲存已更新的 `config.yaml` 檔案。

9. 使用已更新的 `config.yaml` 檔案，將 Helm 圖表安裝至叢集。更新的內容會儲存在您圖表的 ConfigMap 中。

    ```
    helm install -f config.yaml --name=<release_name> ibm/strongswan
    ```
    {: pre}

10. 檢查圖表部署狀態。圖表就緒時，輸出頂端附近的 **STATUS** 欄位會具有 `DEPLOYED` 值。

    ```
    helm status vpn
    ```
    {: pre}

11. 部署圖表之後，請驗證已使用 `config.yaml` 檔案中的已更新設定。

    ```
    helm get values vpn
    ```
    {: pre}

12. 清除現行測試 Pod。

    ```
    kubectl get pods -a -l app=strongswan-test
    ```
    {: pre}

    ```
    kubectl delete pods -l app=strongswan-test
    ```
    {: pre}

13. 重新執行測試。

    ```
    helm test vpn
    ```
    {: pre}

<br />


## 依名稱空間或工作者節點限制 strongSwan VPN 資料流量
{: #limit}

如果您具有單一承租戶叢集，或者如果您具有可在承租戶之間共用叢集資源的多方承租戶叢集，則您可以[將每個 strongSwan 部署的 VPN 資料流量限制為特定名稱空間中的 Pod](#limit_namespace)。如果您具有叢集資源專用於承租戶的多方承租戶叢集，則您可以[將每個 strongSwan 部署的 VPN 資料流量限制為專用於每個承租戶的工作者節點](#limit_worker)。
{: shortdesc}

### 依名稱空間限制 strongSwan VPN 資料流量
{: #limit_namespace}

當您具有單一承租戶或多方承租戶叢集時，可以將 VPN 資料流量限制為僅在特定名稱空間中的 Pod。
{: shortdesc}

例如，假設您只要特定名稱空間 (`my-secure-namespace`) 中的 Pod 透過 VPN 傳送及接收資料。您不要其他名稱空間（例如 `kube-system`、`ibm-system` 或 `default`）中的 Pod 存取您的內部部署網路。若要將 VPN 資料流量限制為僅限 `my-secure-namespace`，您可以建立 Calico 廣域網路原則。

在使用此解決方案之前，請檢閱下列考量及限制。
* 您不需要將 strongSwan Helm 圖表部署到指定的名稱空間。strongSwan VPN Pod 及遞送常駐程式集可以部署至 `kube-system` 或任何其他名稱空間。如果 strongSwan VPN 未部署至指定的名稱空間，則 `vpn-strongswan-ping-remote-ip-1` Helm 測試會失敗。這是預期且可接受的失敗。此測試會從 Pod 對內部部署 VPN 閘道的 `remote.privateIPtoPing` 專用 IP 位址進行連線測試，而此 Pod 不在可以直接存取遠端子網路的名稱空間中。不過，VPN Pod 仍然能夠將資料流量轉遞至名稱空間中對遠端子網路具有路徑的 Pod，而且資料流量仍然可以正確地流動。VPN 狀況仍是 `ESTABLISHED`，而且所指定之名稱空間中的 Pod 可以透過 VPN 進行連接。

* 下列步驟中的 Calico 廣域網路原則不會防止使用主機網路的 Kubernetes Pod，透過 VPN 傳送及接收資料。使用主機網路配置 Pod 時，在 Pod 中執行的應用程式可以接聽其所在工作者節點的網路介面。這些主機網路 Pod 都可以存在於任何名稱空間中。若要判斷哪些 Pod 具有主機網路，請執行 `kubectl get pods --all-namespaces -o wide`，並尋找所有沒有 `172.30.0.0/16` Pod IP 位址的 Pod。如果您想要防止主機網路 Pod 透過 VPN 傳送及接收資料，您可以在 `values.yaml` 部署檔案中設定下列選項：`local.subnet: 172.30.0.0/16` 及 `enablePodSNAT: false`。這些配置設定會透過 VPN 連線將所有 Kubernetes Pod 公開至遠端網路。不過，只有位於所指定之安全名稱空間的 Pod 才能透過 VPN 進行聯絡。

開始之前：
* [部署 strongSwan Helm 圖表](#vpn_configure)，並[確保 VPN 連線功能運作正常](#vpn_test)。
* [安裝並配置 Calico CLI](/docs/containers?topic=containers-network_policies#cli_install)。

若要將 VPN 資料流量限制為特定名稱空間，請執行下列動作：

1. 建立名稱為 `allow-nonvpn-outbound.yaml` 的 Calico 廣域網路原則。此原則容許所有名稱空間繼續將出埠資料流量傳送至所有目的地，但 strongSwan VPN 存取的遠端子網路除外。將 `<remote.subnet>` 取代為您在 Helm `values.yaml` 配置檔中指定的 `remote.subnet`。若要指定多個遠端子網路，請參閱 [Calico 文件 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.projectcalico.org/v3.3/reference/calicoctl/resources/globalnetworkpolicy)。
    ```yaml
    apiVersion: projectcalico.org/v3
    kind: GlobalNetworkPolicy
    metadata:
      name: allow-non-vpn-outbound
    spec:
      selector: has(projectcalico.org/namespace)
      egress:
      - action: Allow
        destination:
          notNets:
          - <remote.subnet>
      order: 900
      types:
      - Egress
    ```
    {: codeblock}

2. 套用原則。

    ```
    calicoctl apply -f allow-non-vpn-outbound.yaml --config=filepath/calicoctl.cfg
    ```
    {: pre}

3. 建立另一個名稱為 `allow-vpn-from-namespace.yaml` 的 Calico 廣域網路原則。此原則僅容許指定的名稱空間將出埠資料流量傳送至 strongSwan VPN 存取的遠端子網路。將 `<namespace>` 取代為可以存取 VPN 的名稱空間，並將 `<remote.subnet>` 取代為您在 Helm `values.yaml` 配置檔中指定的 `remote.subnet`。若要指定多個名稱空間或遠端子網路，請參閱 [Calico 文件 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.projectcalico.org/v3.3/reference/calicoctl/resources/globalnetworkpolicy)。
    ```yaml
    apiVersion: projectcalico.org/v3
    kind: GlobalNetworkPolicy
    metadata:
      name: allow-vpn-from-namespace
    spec:
      selector: projectcalico.org/namespace == "<namespace>"
      egress:
      - action: Allow
        destination:
          nets:
          - <remote.subnet>
      order: 900
      types:
      - Egress
    ```
    {: codeblock}

4. 套用原則。

    ```
    calicoctl apply -f allow-vpn-from-namespace.yaml --config=filepath/calicoctl.cfg
    ```
    {: pre}

5. 驗證已在叢集裡建立廣域網路原則。
    ```
    calicoctl get GlobalNetworkPolicy -o wide --config=filepath/calicoctl.cfg
    ```
    {: pre}

### 依工作者節點限制 strongSwan VPN 資料流量
{: #limit_worker}

在多方承租戶叢集裡具有多個 strongSwan VPN 部署時，您可以將每個部署的 VPN 資料流量限制為專用於每個承租戶的特定工作者節點。
{: shortdesc}

部署 strongSwan Helm 圖表時，會建立 strongSwan VPN 部署。strongSwan VPN Pod 會部署至任何無污點的工作者節點。此外，還會建立 Kubernetes 常駐程式集。此常駐程式集會自動將叢集裡所有無污點之工作者節點上的路徑配置為每個遠端子網路。strongSwan VPN Pod 會使用工作者節點上的路徑，將要求轉遞至內部部署網路中的遠端子網路。

除非您在 `value.yaml` 檔案的 `tolerations` 設定中指定污點，否則路徑不會配置在有污點的節點上。藉由為工作者節點加上污點，您可以阻止在這些工作者節點上配置任何 VPN 路徑。然後，在 `tolerations` 設定中，您可以只針對您要在有污點工作者節點上允許的 VPN 部署指定污點。如此，某個承租戶的 Helm 圖表部署的 strongSwan VPN Pod 只會使用該承租戶的工作者節點上的路徑，透過 VPN 連線將資料流量轉遞至遠端子網路。

在使用此解決方案之前，請檢閱下列考量及限制。
* 依預設，Kubernetes 會將應用程式 Pod 放置在任何可用的無污點工作者節點上。若要確定此解決方案運作正常，每個承租戶首先必須確定，他們只針對正確的承租戶將其應用程式 Pod 部署至有污點的工作者節點。此外，每個有污點的工作者節點也都必須要有一個容錯，才能讓應用程式 Pod 放置在節點上。如需污點及容錯的相關資訊，請參閱 [Kubernetes 文件 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/)。
* 叢集資源可能未以最佳化方式使用，因為任一承租戶都無法將應用程式 Pod 放置在共用的無污點節點上。

依工作者節點限制 strongSwan VPN 資料流量的下列步驟會使用此範例情境：假設您有一個多方承租戶 {{site.data.keyword.containerlong_notm}} 叢集，具有六個工作者節點。叢集支援承租戶 A 和承租戶 B。您以下列方式為工作者節點加上污點：
* 兩個工作者節點有污點，因此只有承租戶 A Pod 才會排定在工作者節點上。
* 兩個工作者節點有污點，因此只有承租戶 B Pod 才會排定在工作者節點上。
* 兩個工作者節點沒有污點，因為 strongSwan VPN Pod 及執行所在的負載平衡器 IP 至少需要 2 個工作者節點。

若要將 VPN 資料流量限制為每個承租戶的有污點節點，請執行下列動作：

1. 若要在此範例中，將 VPN 資料流量限制為僅專用於承租戶 A 的工作者節點，您可以在 `values.yaml` 檔案中，針對承租戶 A strongSwan Helm 圖表指定下列 `toleration`：
    ```
    tolerations:
     - key: dedicated
       operator: "Equal"
       value: "tenantA"
       effect: "NoSchedule"
    ```
    {: codeblock}
    此容忍容許遞送常駐程式集在兩個具有 `dedicated="tenantA"` 污點的工作者節點上執行，以及在兩個無污點的工作者節點上執行。此部署的 strongSwan VPN Pod 會在兩個無污點的工作者節點上執行。

2. 若要在此範例中，將 VPN 資料流量限制為僅專用於承租戶 B 的工作者節點，您可以在 `values.yaml` 檔案中，針對承租戶 B strongSwan Helm 圖表指定下列 `toleration`：
    ```
    tolerations:
     - key: dedicated
       operator: "Equal"
       value: "tenantB"
       effect: "NoSchedule"
    ```
    {: codeblock}

    此容忍容許遞送常駐程式集在兩個具有 `dedicated="tenantB"` 污點的工作者節點上執行，以及在兩個無污點的工作者節點上執行。此部署的 strongSwan VPN Pod 也會在兩個無污點的工作者節點上執行。

<br />


## 升級 strongSwan Helm 圖表
{: #vpn_upgrade}

藉由升級 strongSwan Helm 圖表，確定它保持最新。
{:shortdesc}

若要將 strongSwan Helm 圖表升級至最新版本，請執行下列動作：

  ```
  helm upgrade -f config.yaml <release_name> ibm/strongswan
  ```
  {: pre}

## 停用 strongSwan IPSec VPN 服務
{: vpn_disable}

您可以藉由刪除 Helm 圖表來停用 VPN 連線。
{:shortdesc}

  ```
  helm delete --purge <release_name>
  ```
  {: pre}

<br />


## 使用 Virtual Router Appliance
{: #vyatta}

[Virtual Router Appliance (VRA)](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-about-the-vra) 提供適用於 x86 裸機伺服器的最新 Vyatta 5600 作業系統。您可以使用 VRA 作為 VPN 閘道，以安全地連接至內部部署網路。
{:shortdesc}

進入或離開叢集 VLAN 的所有公用及專用網路資料流量都會透過 VRA 遞送。您可以使用 VRA 作為 VPN 端點，在 IBM Cloud 基礎架構 (SoftLayer) 及內部部署資源中的伺服器之間建立已加密的 IPSec 通道。例如，下圖顯示 {{site.data.keyword.containerlong_notm}} 中僅限專用工作者節點上的應用程式如何透過 VRA VPN 連線與內部部署伺服器通訊：

<img src="images/cs_vpn_vyatta.png" width="725" alt="使用負載平衡器，在 {{site.data.keyword.containerlong_notm}} 中公開應用程式" style="width:725px; border-style: none"/>

1. 叢集裡的應用程式 (`myapp2`) 會接收來自 Ingress 或 LoadBalancer 服務的要求，且需要安全地連接至內部部署網路中的資料。

2. 因為 `myapp2` 位於僅限專用 VLAN 的工作者節點上，所以 VRA 在工作者節點與內部部署網路之間作為安全連線。VRA 會使用目的地 IP 位址，來判定要傳送至內部部署網路的網路封包。

3. 此要求會加密並透過 VPN 通道傳送至內部部署資料中心。

4. 送入的要求會透過內部部署防火牆，並遞送至解密所在的 VPN 通道端點（路由器）。

5. VPN 通道端點（路由器）會將要求轉遞至內部部署伺服器或大型主機，視步驟 2 中指定的目的地 IP 位址而定。必要資料會藉由相同的處理程序，透過 VPN 連線傳回至 `myapp2`。

若要設定 Virtual Router Appliance，請執行下列動作：

1. [訂購 VRA](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-getting-started)。

2. [在 VRA 上配置專用 VLAN](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-managing-your-vlans)。

3. 若要使用 VRA 啟用 VPN 連線，請[在 VRA 上配置 VRRP](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-working-with-high-availability-and-vrrp#high-availability-vpn-with-vrrp)。

如果您具有現有的路由器應用裝置，然後新增叢集，則系統不會在路由器應用裝置上配置針對該叢集所訂購的新可攜式子網路。為了能夠使用網路服務，您必須[啟用 VLAN Spanning](/docs/containers?topic=containers-subnets#subnet-routing)，在相同 VLAN 上的子網路之間啟用遞送。若要確認是否已啟用 VLAN Spanning，請使用 `ibmcloud ks vlan-spanning-get --region <region>` [指令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get)。
{: important}
