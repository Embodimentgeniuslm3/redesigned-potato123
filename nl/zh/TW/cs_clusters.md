---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-07"

keywords: kubernetes, iks, clusters, worker nodes, worker pools, delete

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
{:gif: data-image-type='gif'}


# 建立叢集
{: #clusters}

在 {{site.data.keyword.containerlong}} 中建立 Kubernetes 叢集。
{: shortdesc}

仍要開始使用嗎？請試用[建立 Kubernetes 叢集指導教學](/docs/containers?topic=containers-cs_cluster_tutorial#cs_cluster_tutorial)。
不確定要選擇哪種叢集設定？請參閱[規劃叢集網路設定](/docs/containers?topic=containers-plan_clusters)。
{: tip}

您之前是否已建立叢集，只是想要尋找快速指令範例？可試試這些範例。
*  **免費叢集**：
   ```
   ibmcloud ks cluster-create --name my_cluster
   ```
   {: pre}
*  **標準叢集、共用虛擬機器**：
   ```
   ibmcloud ks cluster-create --name my_cluster --zone dal10 --machine-type b3c.4x16 --hardware shared --workers 3 --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID>
   ```
   {: pre}
*  **標準叢集、裸機伺服器**：
   ```
   ibmcloud ks cluster-create --name my_cluster --zone dal10 --machine-type mb2c.4x32 --hardware dedicated --workers 3 --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID>
   ```
   {: pre}
*  **標準叢集（在啟用 VRF 的帳戶中使用公用和專用服務端點的虛擬機器）**：
   ```
   ibmcloud ks cluster-create --name my_cluster --zone dal10 --machine-type b3c.4x16 --hardware shared --workers 3 --public-service-endpoint --private-service-endpoint --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID>
   ```
   {: pre}
*  **標準叢集（僅使用專用 VLAN 和專用服務端點）**：
   ```
    ibmcloud ks cluster-create --name my_cluster --zone dal10 --machine-type b3c.4x16 --hardware shared --workers 3 --private-service-endpoint --private-vlan <private_VLAN_ID> --private-only
    ```
   {: pre}

<br />


## 準備在帳戶層次建立叢集
{: #cluster_prepare}

準備 {{site.data.keyword.Bluemix_notm}} 帳戶以使用 {{site.data.keyword.containerlong_notm}}。這些準備工作由帳戶管理者完成，完成後，您可能無需在每次建立叢集時對其進行變更。不過，每次您建立叢集時，仍然想要驗證現行帳戶層次狀態為您需要其處於的狀態。
{: shortdesc}

1. [建立或將您的帳戶升級至計費帳戶（{{site.data.keyword.Bluemix_notm}}隨收隨付制或訂閱](https://cloud.ibm.com/registration/)）。

2. [在您要建立叢集的地區中設定 {{site.data.keyword.containerlong_notm}}API 金鑰](/docs/containers?topic=containers-users#api_key)。指派具有適當許可權的 API 金鑰來建立叢集：
  * IBM Cloud 基礎架構 (SoftLayer) 的**超級使用者**角色。
  * 帳戶層次中 {{site.data.keyword.containerlong_notm}} 的**管理者**平台管理角色。
  * 帳戶層次中 {{site.data.keyword.registrylong_notm}} 的**管理者**平台管理角色。如果您帳戶的日期是在 2018 年 10 月 4 日之前，您需要[啟用 {{site.data.keyword.registryshort_notm}} 的 {{site.data.keyword.Bluemix_notm}} IAM 原則](/docs/services/Registry?topic=registry-user#existing_users)。有了 IAM 原則，您就可以控制對資源（例如登錄名稱空間）的存取。

  您是帳戶擁有者嗎？您已具有必要的許可權！建立叢集時，會使用您的認證設定該地區及資源群組的 API 金鑰。
    {: tip}

3. 驗證您具有 {{site.data.keyword.containerlong_notm}} 的**管理者**平台角色。若要容許叢集從專用登錄取回映像檔，您還需要 {{site.data.keyword.registrylong_notm}} 的**管理者**平台角色。
  1. 從 [{{site.data.keyword.Bluemix_notm}} 主控台 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/) 功能表列中，按一下**管理 > 存取權 (IAM)**。
  2. 按一下**使用者**頁面，然後從表格中選取您自己。
  3. 從**存取原則**標籤中，確認您的**角色**為**管理者**。您可以是帳戶中所有資源的**管理者**，或至少是 {{site.data.keyword.containershort_notm}} 的管理者。**附註**：如果您只在一個資源群組或地區（而非整個帳戶）中，具有 {{site.data.keyword.containershort_notm}} 的**管理者**角色，則必須在帳戶層次至少具有**檢視者**角色，才能看到帳戶的 VLAN。
  <p class="tip">請確定您的帳戶管理者未同時將**管理者**平台角色與服務角色指派給您。您必須個別指派平台角色及服務角色。</p>

4. 如果您的帳戶使用多個資源群組，請瞭解您帳戶的[管理資源群組](/docs/containers?topic=containers-users#resource_groups)策略。
  * 在您登入 {{site.data.keyword.Bluemix_notm}} 時，會在您設為目標的資源群組中建立叢集。如果您未將目標設為資源群組，則會自動將目標設為 default 資源群組。
  * 如果您要在非 default 資源群組中建立叢集，則至少需要是資源群組的**檢視者**角色。如果您沒有任何資源群組角色，但仍是資源群組內服務的**管理者**，則會在 default 資源群組中建立您的叢集。
  * 您無法變更叢集的資源群組。此外，如果您需要使用 `ibmcloud ks cluster-service-bind` [指令](/docs/containers-cli-plugin?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_service_bind)來進行[與 {{site.data.keyword.Bluemix_notm}} 服務的整合](/docs/containers?topic=containers-service-binding#bind-services)，則該服務必須與叢集位於相同的資源群組中。未使用 {{site.data.keyword.registrylong_notm}} 這類資源群組或不需要 {{site.data.keyword.la_full_notm}} 這類服務連結的服務都會運作，即使叢集位於不同的資源群組中。
  * 如果您計劃使用 [{{site.data.keyword.monitoringlong_notm}} 進行度量](/docs/containers?topic=containers-health#view_metrics)，請計劃提供一個叢集名稱，該名稱在帳戶的所有資源群組和地區中必須是唯一的，以避免發生度量命名衝突。
  * 免費叢集會在 `default` 資源群組中建立。

5. **標準叢集**：規劃叢集[網路設定](/docs/containers?topic=containers-plan_clusters)，以便叢集符合工作負載和環境的需求。然後，設定 IBM Cloud 基礎架構 (SoftLayer) 網路連線功能，以容許工作者節點與主節點的通訊以及使用者與主節點的通訊：
  * 僅使用專用服務端點或使用公用和專用服務端點（執行面對網際網路的工作負載或延伸內部部署資料中心）：
    1. 在您的 IBM Cloud 基礎架構 (SoftLayer) 帳戶中啟用 [VRF](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud)。
    2. [啟用 {{site.data.keyword.Bluemix_notm}} 帳戶，以使用服務端點](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started)。<p class="note">如果授權叢集使用者位於 {{site.data.keyword.Bluemix_notm}} 專用網路，或透過 [VPN 連線](/docs/infrastructure/iaas-vpn?topic=VPN-gettingstarted-with-virtual-private-networking)或 [{{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link) 連接至專用網路，則可以透過專用服務端點私下存取 Kubernetes 主節點。然而，透過專用服務端點與 Kubernetes 主節點的通訊，必須經過 <code>166.X.X.X</code> IP 位址範圍，此範圍無法從 VPN 連線或透過 {{site.data.keyword.Bluemix_notm}} Direct Link 遞送。您可以為叢集使用者，使用專用網路負載平衡器 (NLV) 公開主節點的專用服務端點。專用 NLB 會將主節點的專用服務端點公開為內部 <code>10.X.X.X</code> IP 位址範圍，使用者可以用 VPN 或 {{site.data.keyword.Bluemix_notm}} Direct Link 連線來存取此範圍。如果您只啟用專用服務端點，可以使用 Kubernetes 儀表板或暫時啟用公用服務端點以建立專用 NLB。如需相關資訊，請參閱[透過專用服務端點存取叢集](/docs/containers?topic=containers-clusters#access_on_prem)。</p>

  * 僅使用公用服務端點（執行面對網際網路的工作負載）：
    1. 針對您的 IBM Cloud 基礎架構 (SoftLayer) 帳戶啟用 [VLAN Spanning](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning)，讓您的工作者節點可在專用網路上彼此通訊。若要執行此動作，您需要**網路 > 管理網路 VLAN Spanning** [基礎架構許可權](/docs/containers?topic=containers-users#infra_access)，或者您可以要求帳戶擁有者啟用它。若要確認是否已啟用 VLAN Spanning，請使用 `ibmcloud ks vlan-spanning-get --region <region>` [指令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get)。
  * 使用閘道裝置（延伸內部部署資料中心）：
    1. 針對您的 IBM Cloud 基礎架構 (SoftLayer) 帳戶啟用 [VLAN Spanning](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning)，讓您的工作者節點可在專用網路上彼此通訊。若要執行此動作，您需要**網路 > 管理網路 VLAN Spanning** [基礎架構許可權](/docs/containers?topic=containers-users#infra_access)，或者您可以要求帳戶擁有者啟用它。若要確認是否已啟用 VLAN Spanning，請使用 `ibmcloud ks vlan-spanning-get --region <region>` [指令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get)。
    2. 配置閘道裝置。例如，可以選擇設定[虛擬路由器應用裝置](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-about-the-vra)或 [Fortigate Security Appliance](/docs/services/vmwaresolutions/services?topic=vmware-solutions-fsa_considerations)，以充當防火牆來容許必需的資料流量並封鎖不想要的資料流量。
    3. 為每個地區（以便主節點和工作者節點可以通訊）以及為您計劃使用的 {{site.data.keyword.Bluemix_notm}} 服務[開啟必要的專用 IP 位址和埠](/docs/containers?topic=containers-firewall#firewall_outbound)。

<br />


## 準備在叢集層次建立叢集
{: #prepare_cluster_level}

設定帳戶以建立叢集後，請準備設定叢集。這些是每次建立叢集時會影響叢集的準備工作。
{: shortdesc}

1. 決定[免費或標準叢集](/docs/containers?topic=containers-cs_ov#cluster_types)。您可以建立 1 個免費叢集來試用部分功能 30 天，或建立完全可自訂的標準叢集，並選擇硬體隔離。請建立標準叢集，以獲得更多好處及對您叢集效能的控制。

2. 對於標準叢集，請規劃叢集設定。
  * 判定是要建立[單一區域](/docs/containers?topic=containers-ha_clusters#single_zone)還是[多區域](/docs/containers?topic=containers-ha_clusters#multizone)叢集。請注意，多區域叢集僅適用於選取位置。
  * 為叢集的工作者節點選擇您想要哪種類型的[硬體及隔離](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)，包括虛擬機器與祼機機器之間的決策。

3. 如果是標準叢集，您可以在 {{site.data.keyword.Bluemix_notm}} 主控台中[預估成本](/docs/billing-usage?topic=billing-usage-cost#cost)。如需可能未內含在預估器中的費用相關資訊，請參閱[定價及計費](/docs/containers?topic=containers-faqs#charges)。

4. 如果是在防火牆後的環境中建立叢集，例如用於延伸內部部署資料中心的叢集，請針對計劃使用的 {{site.data.keyword.Bluemix_notm}} 服務，[容許傳送至公用和專用 IP 的出埠網路資料流量](/docs/containers?topic=containers-firewall#firewall_outbound)。

<br />


## 建立免費叢集
{: #clusters_free}

您可以使用 1 個免費叢集來熟悉 {{site.data.keyword.containerlong_notm}} 的運作方式。使用免費叢集，您可以先學習術語、完成指導教學以及瞭解您的方向，再跳至正式作業層級標準叢集。別擔心，就算您擁有計費帳戶，也仍會獲得免費叢集。
{: shortdesc}

免費叢集包含設定有 2 個 vCPU 和 4 GB 記憶體的一個工作者節點，免費叢集的有效期限為 30 天。在該時間之後，叢集會到期，並刪除叢集和其資料。{{site.data.keyword.Bluemix_notm}} 不會備份刪除的資料，因此無法予以還原。請務必備份所有重要資料。
{: note}

### 在主控台中建立免費叢集
{: #clusters_ui_free}

1. 在[型錄 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/catalog?category=containers) 中，選取 **{{site.data.keyword.containershort_notm}}** 來建立叢集。
2. 選取**免費**叢集方案。
3. 選取要部署叢集的地理位置。
4. 在地理位置中選取都會位置。叢集將在此都會內的區域中建立。
5. 提供叢集名稱。此名稱必須以字母開頭，可以包含字母、數字及連字號 (-)，且長度不得超過 35 個字元。請使用在各地區之間都唯一的名稱。
6. 按一下**建立叢集**。依預設，會建立具有一個工作者節點的工作者節點儲存區。您可以在**工作者節點**標籤中查看工作者節點部署的進度。部署完成時，您可以在**概觀**標籤中看到叢集已備妥。請注意，即使叢集已備妥，叢集裡由其他服務使用的某些部分（例如，登錄映像檔取回密碼）可能仍在進行中。

  每個工作者節點都會獲指派唯一的工作者節點 ID 及網域名稱，而在建立叢集之後不得手動變更。變更 ID 或網域名稱會讓 Kubernetes 主節點無法管理叢集。
    {: important}
7. 在您的叢集建立之後，您可以[配置 CLI 階段作業以開始使用叢集](#access_cluster)。

### 在 CLI 中建立免費叢集
{: #clusters_cli_free}

開始之前，請安裝 {{site.data.keyword.Bluemix_notm}} CLI 及 [{{site.data.keyword.containerlong_notm}} 外掛程式](/docs/containers?topic=containers-cs_cli_install#cs_cli_install)。

1. 登入 {{site.data.keyword.Bluemix_notm}} CLI。
  1. 系統提示時，請登入並輸入 {{site.data.keyword.Bluemix_notm}} 認證。
     ```
     ibmcloud login
     ```
     {: pre}

     如果您具有聯合 ID，請使用 `ibmcloud login --sso` 來登入 {{site.data.keyword.Bluemix_notm}} CLI。請輸入使用者名稱，並使用 CLI 輸出中提供的 URL 來擷取一次性密碼。若沒有 `--sso` 時登入失敗，而有 `--sso` 選項時登入成功，即表示您有聯合 ID。
    {: tip}

  2. 如果您有多個 {{site.data.keyword.Bluemix_notm}} 帳戶，請選取您要在其中建立 Kubernetes 叢集的帳戶。

  3. 若要在特定地區中建立免費叢集，必須將該地區設定為目標。可以在 `ap-south`、`eu-central`、`uk-south` 或 `us-south` 中建立免費叢集。叢集將在該地區內的區域中建立。
     ```
    ibmcloud ks region-set
    ```
     {: pre}

2. 建立叢集。
  ```
  ibmcloud ks cluster-create --name my_cluster
  ```
  {: pre}

3. 驗證已要求建立叢集。訂購工作者節點機器可能需要幾分鐘時間。
    ```
    ibmcloud ks clusters
    ```
    {: pre}

    叢集佈建完成之後，叢集的狀態會變更為 **deployed**。
    ```
    Name         ID                                   State      Created          Workers   Zone       Version     Resource Group Name
    my_cluster   paf97e8843e29941b49c598f516de72101   deployed   20170201162433   1         mil01      1.13.6      Default
    ```
    {: screen}

4. 檢查工作者節點節點的狀態。
    ```
    ibmcloud ks workers --cluster <cluster_name_or_ID>
    ```
    {: pre}

    當工作者節點已備妥時，狀態會變更為 **normal**，而狀態為 **Ready**。當節點狀態為 **Ready** 時，您就可以存取叢集。請注意，即使叢集已備妥，其他服務所使用的一些叢集部分（例如 Ingress 密碼或登錄映像檔取回密碼）可能還是在處理中。
    ```
    ID                                                 Public IP       Private IP      Machine Type   State    Status   Zone        Version     Resource Group Name
    kube-mil01-paf97e8843e29941b49c598f516de72101-w1   169.xx.xxx.xxx  10.xxx.xx.xxx   free           normal   Ready    mil01       1.13.6      Default
    ```
    {: screen}

    每個工作者節點都會獲指派唯一的工作者節點 ID 及網域名稱，而在建立叢集之後不得手動變更。變更 ID 或網域名稱會讓 Kubernetes 主節點無法管理叢集。
    {: important}

5. 在您的叢集建立之後，您可以[配置 CLI 階段作業以開始使用叢集](#access_cluster)。

<br />


## 建立標準叢集
{: #clusters_standard}

使用 {{site.data.keyword.Bluemix_notm}} CLI 或 {{site.data.keyword.Bluemix_notm}} 主控台來建立完全可自訂的標準叢集，您可以選擇硬體隔離並且有權存取多種特性，如用於高可用性環境的多個工作者節點。
{: shortdesc}

### 在主控台中建立標準叢集
{: #clusters_ui}

1. 在[型錄 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/catalog?category=containers) 中，選取 **{{site.data.keyword.containershort_notm}}** 來建立叢集。

2. 選取要在其中建立叢集的資源群組。
  **附註**：
    * 一個叢集只能在一個資源群組中建立，而且在建立叢集之後，您無法變更其資源群組。
    * 若要在非 default 資源群組中建立叢集，您必須至少具有該資源群組的[**檢視者**角色](/docs/containers?topic=containers-users#platform)。

3. 選取要部署叢集的地理位置。

4. 為叢集提供唯一名稱。此名稱必須以字母開頭，可以包含字母、數字及連字號 (-)，且長度不得超過 35 個字元。請使用在各地區之間都唯一的名稱。叢集名稱和叢集部署所在的地區會形成 Ingress 子網域的完整網域名稱。為了確保 Ingress 子網域在地區內是唯一的，叢集名稱可能會被截斷，並在 Ingress 網域名稱內附加一個隨機值。
**附註**：變更在建立期間指派的唯一 ID 或網域名稱會封鎖 Kubernetes 主節點，使其無法管理叢集。

5. 選取**單一區域**或**多區域**可用性。在多區域叢集裡，主節點部署於具有多區域功能的區域中，而且您的叢集資源會分散到多個區域。

6. 輸入都會和區域詳細資料。
  * 多區域叢集：
    1. 選取都會位置。要獲得最佳效能，請選取實際離您最近的都會位置。您的選擇可能會受到地理位置的限制。
    2. 選取您要在其中管理叢集的特定區域。您必須至少選取 1 個區域，但可以選取所需數目的區域。如果您選取超過 1 個區域，則工作者節點會分散到您選擇的各區域，讓您具有更高的可用性。如果您只選取 1 個區域，則可以在建立叢集之後[將區域新增至叢集](/docs/containers?topic=containers-add_workers#add_zone)。
  * 單一區域叢集：選取要在其中管理叢集的區域。要獲得最佳效能，請選取實際離您最近的區域。您的選擇可能會受到地理位置的限制。

7. 為每個區域選擇 VLAN。
  * 若要建立可在其中執行面對網際網路之工作負載的叢集，請執行下列動作：
    1. 從 IBM Cloud 基礎架構 (SoftLayer) 帳戶中為每個區域選取公用 VLAN 和專用 VLAN。工作者節點使用專用 VLAN 彼此通訊，並可以使用公用或專用 VLAN 與 Kubernetes 主節點進行通訊。如果在此區域中沒有公用或專用 VLAN，則會自動建立公用和專用 VLAN。您可以將相同的 VLAN 用於多個叢集。
  * 建立用於僅在專用網路上延伸內部部署資料中心的叢集，用於透過日後新增有限公用存取權的選項延伸內部部署資料中心的叢集，或者用於透過閘道裝置延伸內部部署資料中心並提供有限公用存取權的叢集：
    1. 從 IBM Cloud 基礎架構 (SoftLayer) 帳戶中為每個區域選取專用 VLAN。工作者節點使用專用 VLAN 來彼此通訊。如果在某個區域中沒有專用 VLAN，則會自動建立專用 VLAN。您可以將相同的 VLAN 用於多個叢集。
    2. 對於公用 VLAN，選取**無**。

8. 對於**主節點服務端點**，選擇 Kubernetes 主節點和工作者節點的通訊方式。
  * 若要建立可在其中執行面對網際網路之工作負載的叢集，請執行下列動作：
    * 如果在 {{site.data.keyword.Bluemix_notm}} 帳戶中已啟用了 VRF 和服務端點，請選取**專用和公用端點**。
    * 如果無法或不想啟用 VRF，請選取**僅限公用端點**。
  * 若要建立用於僅延伸內部部署資料中心的叢集，或用於透過邊緣工作者節點延伸內部部署資料中心並提供有限公用存取權的叢集，請選取**專用和公用端點**或**僅限專用端點**。確保在 {{site.data.keyword.Bluemix_notm}} 帳戶中已啟用 VRF 和服務端點。請注意，如果僅啟用專用服務端點，則必須[透過專用網路負載平衡器公開主節點端點](#access_on_prem)，以便使用者可以透過 VPN 或 {{site.data.keyword.BluDirectLink}} 連線存取主節點。
  * 若要建立用於透過閘道裝置延伸內部部署資料中心並提供有限公用存取權的叢集，請選取**僅限公用端點**。

9. 配置預設工作者節點儲存區。工作者節點儲存區是共用相同配置的工作者節點群組。您一律可以稍後再將更多工作者節點儲存區新增至叢集。
  1. 為叢集主節點和工作者節點選擇 Kubernetes API 伺服器版本。
  2. 透過選取機型來過濾工作者節點特性。虛擬伺服器是按小時計費，裸機伺服器是按月計費。
    - **Bare Metal Server**：Bare Metal Server 按月計費，訂購後由 IBM Cloud 基礎架構 (SoftLayer) 手動佈建，可能需要一個工作日以上的時間才能完成。裸機伺服器最適合需要更多資源和主機控制的高效能應用程式。您也可以選擇啟用[授信運算](/docs/containers?topic=containers-security#trusted_compute)，來驗證工作者節點是否遭到竄改。「授信運算」可用於選取裸機的機型。例如，`mgXc` GPU 特性不支援「授信運算」。如果您未在建立叢集期間啟用信任，但後來想要啟用，則可以使用 `ibmcloud ks feature-enable` [指令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable)。啟用信任之後，以後您就無法再予以停用。請確定您要佈建裸機機器。因為它是按月計費，如果您不慎下訂之後立即取消，仍會向您收取整個月的費用。
        {:tip}
    - **虛擬 - 共用**：基礎架構資源（例如，系統管理程序和實體硬體）在您與其他 IBM 客戶之間共用，但每個工作者節點只能由您存取。雖然此選項的成本比較低，而且在大部分情況下已夠用，您可能要根據公司原則來驗證效能及基礎架構需求。
    - **虛擬 - 專用**：工作者節點在帳戶專用的基礎架構上管理。實體資源完全被隔離。
  3. 選取特性。特性用於定義在每個工作者節點中設定並可供容器使用的虛擬 CPU 數量、記憶體數量和磁碟空間數量。可用的裸機及虛擬機器類型會因您部署叢集所在的區域而不同。建立叢集之後，您可以藉由將工作者節點或儲存區新增至叢集來新增不同的機型。
  4. 指定您在叢集裡需要的工作者節點數目。您輸入的工作者節點數目會抄寫到您所選取數目的區域。這表示，如果您有 2 個區域並選取 3 個工作者節點，則會佈建 6 個節點，而且每一個區域中都有 3 個節點。

10. 按一下**建立叢集**。即會建立具有所指定工作者數目的工作者節點儲存區。您可以在**工作者節點**標籤中查看工作者節點部署的進度。部署完成時，您可以在**概觀**標籤中看到叢集已備妥。請注意，即使叢集已備妥，其他服務所使用的一些叢集部分（例如 Ingress 密碼或登錄映像檔取回密碼）可能還是在處理中。
    

  每個工作者節點都會獲指派唯一的工作者節點 ID 及網域名稱，而在建立叢集之後不得手動變更。變更 ID 或網域名稱會讓 Kubernetes 主節點無法管理叢集。
    {: important}

11. 在您的叢集建立之後，您可以[配置 CLI 階段作業以開始使用叢集](#access_cluster)。

### 在 CLI 中建立標準叢集
{: #clusters_cli_steps}

開始之前，請安裝 {{site.data.keyword.Bluemix_notm}} CLI 及 [{{site.data.keyword.containerlong_notm}} 外掛程式](/docs/containers?topic=containers-cs_cli_install#cs_cli_install)。

1. 登入 {{site.data.keyword.Bluemix_notm}} CLI。
  1. 系統提示時，請登入並輸入 {{site.data.keyword.Bluemix_notm}} 認證。
     ```
     ibmcloud login
     ```
     {: pre}

     如果您具有聯合 ID，請使用 `ibmcloud login --sso` 來登入 {{site.data.keyword.Bluemix_notm}} CLI。請輸入使用者名稱，並使用 CLI 輸出中提供的 URL 來擷取一次性密碼。若沒有 `--sso` 時登入失敗，而有 `--sso` 選項時登入成功，即表示您有聯合 ID。
    {: tip}

  2. 如果您有多個 {{site.data.keyword.Bluemix_notm}} 帳戶，請選取您要在其中建立 Kubernetes 叢集的帳戶。

  3. 若要在非 default 資源群組中建立叢集，請將目標設為該資源群組。
      **附註**：
      * 一個叢集只能在一個資源群組中建立，而且在建立叢集之後，您無法變更其資源群組。
      * 您必須至少具有資源群組的[**檢視者**角色](/docs/containers?topic=containers-users#platform)。

      ```
      ibmcloud target -g <resource_group_name>
      ```
      {: pre}

2. 檢閱可用的區域。在下列指令的輸出中，區域的**位置類型**為 `dc`。若要跨越區域間的叢集，您必須在[具有多區域功能的區域](/docs/containers?topic=containers-regions-and-zones#zones)中建立叢集。具有多區域功能的區域在**多區域都會**直欄中具有都會值。
    ```
    ibmcloud ks supported-locations
    ```
    {: pre}
    若您選取的區域不在您的國家/地區境內，請謹記，您可能需要合法授權，才能實際將資料儲存在其他國家/地區。
    {: note}

3. 檢閱該區域中可用的工作者節點特性。工作者節點特性指定可供每個工作者節點使用的虛擬或實體運算主機。
  - **虛擬**：按小時計費的虛擬機器是佈建在共用或專用硬體上。
  - **實體**：當您訂購之後，按月計費的裸機伺服器將由 IBM Cloud 基礎架構 (SoftLayer) 進行手動佈建，這可能需要多個營業日才能完成。裸機伺服器最適合需要更多資源和主機控制的高效能應用程式。
  - **使用授信運算的實體機器**：您也可以選擇啟用[授信運算](/docs/containers?topic=containers-security#trusted_compute)，來驗證裸機工作者節點是否遭到竄改。「授信運算」可用於選取裸機的機型。例如，`mgXc` GPU 特性不支援「授信運算」。如果您未在建立叢集期間啟用信任，但後來想要啟用，則可以使用 `ibmcloud ks feature-enable` [指令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable)。啟用信任之後，以後您就無法再予以停用。
  - **機型**：若要決定若要部署的機型，請檢閱[可用工作者節點硬體](/docs/containers?topic=containers-planning_worker_nodes#shared_dedicated_node)的核心、記憶體及儲存空間組合。建立叢集之後，您可以藉由[新增工作者節點儲存區](/docs/containers?topic=containers-add_workers#add_pool)來新增不同的實體或虛擬機器類型。

     請確定您要佈建裸機機器。因為它是按月計費，如果您不慎下訂之後立即取消，仍會向您收取整個月的費用。
        {:tip}

  ```
  ibmcloud ks machine-types --zone <zone>
  ```
  {: pre}

4. 檢查以確定 IBM Cloud 基礎架構 (SoftLayer) 中是否已存在此帳戶的區域的 VLAN。
  ```
  ibmcloud ks vlans --zone <zone>
  ```
  {: pre}

  輸出範例：
  ```
  ID        Name   Number   Type      Router
  1519999   vlan   1355     private   bcr02a.dal10
  1519898   vlan   1357     private   bcr02a.dal10
  1518787   vlan   1252     public    fcr02a.dal10
  1518888   vlan   1254     public    fcr02a.dal10
  ```
  {: screen}
  * 若要建立可以在其中執行面對網際網路的工作負載的叢集，請檢查以確定是否存在公用和專用 VLAN。如果公用及專用 VLAN 已存在，請記下相符的路由器。專用 VLAN 路由器的開頭一律為 <code>bcr</code>（後端路由器），而公用 VLAN 路由器的開頭一律為 <code>fcr</code>（前端路由器）。當建立叢集並指定公用和專用 VLAN 時，那些字首之後的數字和字母組合必須相符。在範例輸出中，任何專用 VLAN 都可以與任何公用 VLAN 搭配使用，因為路由器都會包括 `02a.dal10`。
  * 若要建立用於僅在專用網路上延伸內部部署資料中心的叢集，用於透過日後透過邊緣工作者節點新增有限公用存取權的選項延伸內部部署資料中心的叢集，或者用於透過閘道裝置延伸內部部署資料中心並提供有限公用存取權的叢集，請檢查以確定是否存在專用 VLAN。如果您有專用 VLAN，請記下該 ID。

5. 執行 `cluster-create` 指令。依預設，會對工作者節點磁碟進行 AES 256 位元加密，並且會按使用小時數對叢集進行計費。
  * 若要建立可在其中執行面對網際網路之工作負載的叢集，請執行下列動作：
    ```
    ibmcloud ks cluster-create --zone <zone> --machine-type <machine_type> --hardware <shared_or_dedicated> --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID> --workers <number> --name <cluster_name> --kube-version <major.minor.patch> [--private-service-endpoint] [--public-service-endpoint] [--disable-disk-encrypt] [--trusted]
    ```
    {: pre}
  * 建立用於透過日後透過邊緣工作者節點新增有限公用存取權的選項，在專用網路上延伸內部部署資料中心的叢集：
    ```
    ibmcloud ks cluster-create --zone <zone> --machine-type <machine_type> --hardware <shared_or_dedicated> --private-vlan <private_VLAN_ID> --private-only --workers <number> --name <cluster_name> --kube-version <major.minor.patch> --private-service-endpoint [--public-service-endpoint] [--disable-disk-encrypt] [--trusted]
    ```
    {: pre}
  * 建立用於透過閘道裝置延伸內部部署資料中心並提供有限公用存取權的叢集：
    ```
    ibmcloud ks cluster-create --zone <zone> --machine-type <machine_type> --hardware <shared_or_dedicated> --private-vlan <private_VLAN_ID> --private-only --workers <number> --name <cluster_name> --kube-version <major.minor.patch> --public-service-endpoint [--disable-disk-encrypt] [--trusted]
    ```
    {: pre}

    <table>
    <caption>cluster-create 元件</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解這個指令的元件</th>
    </thead>
    <tbody>
    <tr>
    <td><code>cluster-create</code></td>
    <td>在 {{site.data.keyword.Bluemix_notm}} 組織中建立叢集的指令。</td>
    </tr>
    <tr>
    <td><code>--zone <em>&lt;zone&gt;</em></code></td>
    <td>指定要在其中建立在步驟 4 中所選叢集的 {{site.data.keyword.Bluemix_notm}} 區域 ID。</td>
    </tr>
    <tr>
    <td><code>--machine-type <em>&lt;machine_type&gt;</em></code></td>
    <td>指定在步驟 5 中選擇的機型。</td>
    </tr>
    <tr>
    <td><code>--hardware <em>&lt;shared_or_dedicated&gt;</em></code></td>
    <td>指定工作者節點的硬體隔離層次。若要讓可用的實體資源只供您專用，請使用 dedicated，或者，若要容許與其他 IBM 客戶共用實體資源，請使用 shared。預設值為 shared。此值對於 VM 標準叢集是選用的。若為裸機機型，請指定 `dedicated`。</td>
    </tr>
    <tr>
    <td><code>--public-vlan <em>&lt;public_vlan_id&gt;</em></code></td>
    <td>如果已經在 IBM Cloud 基礎架構 (SoftLayer) 帳戶中為該區域設定了公用 VLAN，請輸入在步驟 4 中找到的該公用 VLAN 的 ID。<p>專用 VLAN 路由器的開頭一律為 <code>bcr</code>（後端路由器），而公用 VLAN 路由器的開頭一律為 <code>fcr</code>（前端路由器）。當建立叢集並指定公用和專用 VLAN 時，那些字首之後的數字和字母組合必須相符。</p></td>
    </tr>
    <tr>
    <td><code>--private-vlan <em>&lt;private_vlan_id&gt;</em></code></td>
    <td>如果已經在 IBM Cloud 基礎架構 (SoftLayer) 帳戶中為該區域設定了專用 VLAN，請輸入在步驟 4 中找到的該專用 VLAN 的 ID。如果帳戶中沒有專用 VLAN，請不要指定此選項。{{site.data.keyword.containerlong_notm}} 會自動為您建立一個專用 VLAN。<p>專用 VLAN 路由器的開頭一律為 <code>bcr</code>（後端路由器），而公用 VLAN 路由器的開頭一律為 <code>fcr</code>（前端路由器）。當建立叢集並指定公用和專用 VLAN 時，那些字首之後的數字和字母組合必須相符。</p></td>
    </tr>
    <tr>
    <td><code>--private-only </code></td>
    <td>建立僅使用專用 VLAN 的叢集。如果包含此選項，請不要包含 <code>--public-vlan</code> 選項。</td>
    </tr>
    <tr>
    <td><code>--name <em>&lt;name&gt;</em></code></td>
    <td>指定叢集的名稱。此名稱必須以字母開頭，可以包含字母、數字及連字號 (-)，且長度不得超過 35 個字元。請使用在各地區之間都唯一的名稱。叢集名稱和叢集部署所在的地區會形成 Ingress 子網域的完整網域名稱。為了確保 Ingress 子網域在地區內是唯一的，叢集名稱可能會被截斷，並在 Ingress 網域名稱內附加一個隨機值。
</td>
    </tr>
    <tr>
    <td><code>--workers <em>&lt;number&gt;</em></code></td>
    <td>指定要包含在叢集裡的工作者節點數。如果未指定 <code>--workers</code> 選項，會建立 1 個工作者節點。</td>
    </tr>
    <tr>
    <td><code>--kube-version <em>&lt;major.minor.patch&gt;</em></code></td>
    <td>叢集主節點的 Kubernetes 版本。這是選用值。未指定版本時，會使用支援的 Kubernetes 版本的預設值來建立叢集。若要查看可用的版本，請執行 <code>ibmcloud ks versions</code>。
</td>
    </tr>
    <tr>
    <td><code>--private-service-endpoint</code></td>
    <td>**在[啟用 VRF 的帳戶](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started)中**：啟用專用服務端點，以便 Kubernetes 主節點和工作者節點可透過專用 VLAN 進行通訊。此外，您可以選擇使用 `--public-service-endpoint` 旗標來啟用公用服務端點，以透過網際網路存取您的叢集。如果您只啟用專用服務端點，則必須連接至專用 VLAN，才能與 Kubernetes 主節點進行通訊。一旦啟用了專用服務端點，之後您將無法停用它。<br><br>建立叢集之後，您可以透過執行 `ibmcloud ks cluster-get --cluster <cluster_name_or_ID>` 來取得端點。</td>
    </tr>
    <tr>
    <td><code>--public-service-endpoint</code></td>
    <td>啟用公用服務端點，以便可以透過公用網路存取 Kubernetes 主節點（例如，透過終端機執行 `kubectl` 指令），以及使 Kubernetes 主節點和工作者節點可以透過公用 VLAN 進行通訊。如果您想要僅限專用叢集，可以稍後再停用公用服務端點。<br><br>建立叢集之後，您可以透過執行 `ibmcloud ks cluster-get --cluster <cluster_name_or_ID>` 來取得端點。</td>
    </tr>
    <tr>
    <td><code>--disable-disk-encrypt</code></td>
    <td>依預設，工作者節點具有 AES 256 位元[磁碟加密](/docs/containers?topic=containers-security#encrypted_disk)功能。如果您要停用加密，請包括此選項。</td>
    </tr>
    <tr>
    <td><code>--trusted</code></td>
    <td>**Bare Metal Server 叢集**：啟用[授信運算](/docs/containers?topic=containers-security#trusted_compute)以驗證 Bare Metal Server 工作者節點是否被篡改。「授信運算」可用於選取裸機的機型。例如，`mgXc` GPU 特性不支援「授信運算」。如果您未在建立叢集期間啟用信任，但後來想要啟用，則可以使用 `ibmcloud ks feature-enable` [指令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable)。啟用信任之後，以後您就無法再予以停用。</td>
    </tr>
    </tbody></table>

6. 驗證已要求建立叢集。若為虛擬機器，要訂購工作者節點機器並且在您的帳戶中設定及佈建叢集，可能需要一些時間。裸機實體機器是透過與 IBM Cloud 基礎架構 (SoftLayer) 之間的手動互動來進行佈建，可能需要多個營業日才能完成。
    ```
    ibmcloud ks clusters
    ```
    {: pre}

    叢集佈建完成之後，叢集的狀態會變更為 **deployed**。
    ```
    Name         ID                                   State      Created          Workers   Zone       Version     Resource Group Name
    my_cluster   paf97e8843e29941b49c598f516de72101   deployed   20170201162433   1         mil01      1.13.6      Default
    ```
    {: screen}

7. 檢查工作者節點的狀態。
    ```
    ibmcloud ks workers --cluster <cluster_name_or_ID>
    ```
    {: pre}

    工作者節點就緒後，狀況會變更為 **Normal**，並且處於 **Ready** 狀態。當節點狀態為 **Ready** 時，您就可以存取叢集。請注意，即使叢集已就緒，其他服務所使用的一些叢集部分（例如 Ingress 密碼或登錄映像檔取回密碼）可能還是在處理中。請注意，如果建立了僅使用專用 VLAN 的叢集，則不會將任何**公用 IP** 位址指派給工作者節點。

    ```
    ID                                                 Public IP       Private IP      Machine Type   State    Status   Zone        Version     Resource Group Name
    kube-mil01-paf97e8843e29941b49c598f516de72101-w1   169.xx.xxx.xxx  10.xxx.xx.xxx   standard       normal   Ready    mil01       1.13.6      Default
    ```
    {: screen}

    每個工作者節點都會獲指派唯一的工作者節點 ID 及網域名稱，而在建立叢集之後不得手動變更。變更 ID 或網域名稱會讓 Kubernetes 主節點無法管理叢集。
    {: important}

8. 在您的叢集建立之後，您可以[配置 CLI 階段作業以開始使用叢集](#access_cluster)。

<br />


## 存取叢集
{: #access_cluster}

建立叢集後，可以透過配置 CLI 階段作業開始使用叢集。
{: shortdesc}

### 透過公用服務端點存取叢集
{: #access_internet}

若要使用叢集，請將所建立的叢集設定為 CLI 階段作業執行 `kubectl` 指令的環境定義。
{: shortdesc}

1. 如果您的網路受到公司防火牆保護，請允許對 {{site.data.keyword.Bluemix_notm}} 及 {{site.data.keyword.containerlong_notm}} API 端點和埠的存取。
  1. [在防火牆中容許存取 `ibmcloud` API 和 `ibmcloud ks` API 的公用端點](/docs/containers?topic=containers-firewall#firewall_bx)。
  2. [容許授權叢集使用者執行 `kubectl` 指令](/docs/containers?topic=containers-firewall#firewall_kubectl)以便透過僅限公用、僅限專用或公用與專用服務端點來存取主節點。
  3. [容許授權叢集使用者執行 `calicotl` 指令](/docs/containers?topic=containers-firewall#firewall_calicoctl)以管理叢集裡的 Calico 網路原則。

2. 將您建立的叢集設為此階段作業的環境定義。請在您每次使用叢集時，完成下列配置步驟。

  如果您要改用 {{site.data.keyword.Bluemix_notm}} 主控台，則您可以在 [Kubernetes Terminal](/docs/containers?topic=containers-cs_cli_install#cli_web) 中從 Web 瀏覽器直接執行 CLI 指令。
  {: tip}
  1. 取得指令來設定環境變數，並下載 Kubernetes 配置檔。
      ```
      ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
      ```
      {: pre}
      配置檔下載完成之後，會顯示一個指令，可讓您用來將本端 Kubernetes 配置檔的路徑設定為環境變數。

      OS X 的範例：
      ```
      export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/mycluster/kube-config-prod-dal10-mycluster.yml
      ```
      {: screen}
  2. 複製並貼上終端機中顯示的指令，以設定 `KUBECONFIG` 環境變數。
  3. 驗證已適當地設定 `KUBECONFIG` 環境變數。OS X 的範例：
      ```
      echo $KUBECONFIG
      ```
      {: pre}

      輸出：
      ```
      /Users/<user_name>/.bluemix/plugins/container-service/clusters/mycluster/kube-config-prod-dal10-mycluster.yml
      ```
      {: screen}

3. 使用預設埠 `8001` 來啟動 Kubernetes 儀表板。
  1. 使用預設埠號來設定 Proxy。
      ```
      kubectl proxy
      ```
      {: pre}

      ```
      Starting to serve on 127.0.0.1:8001
      ```
      {: screen}

  2.  在 Web 瀏覽器中開啟下列 URL，以查看 Kubernetes 儀表板。
      ```
      http://localhost:8001/ui
      ```
      {: codeblock}

### 透過專用服務端點存取叢集
{: #access_on_prem}

如果授權叢集使用者位於 {{site.data.keyword.Bluemix_notm}} 專用網路，或透過 [VPN 連線](/docs/infrastructure/iaas-vpn?topic=VPN-gettingstarted-with-virtual-private-networking)或 [{{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link) 連接至專用網路，則可以透過專用服務端點私下存取 Kubernetes 主節點。然而，透過專用服務端點與 Kubernetes 主節點的通訊，必須經過 <code>166.X.X.X</code> IP 位址範圍，此範圍無法從 VPN 連線或透過 {{site.data.keyword.Bluemix_notm}} Direct Link 遞送。您可以為叢集使用者，使用專用網路負載平衡器 (NLV) 公開主節點的專用服務端點。專用 NLB 會將主節點的專用服務端點公開為內部 <code>10.X.X.X</code> IP 位址範圍，使用者可以用 VPN 或 {{site.data.keyword.Bluemix_notm}} Direct Link 連線來存取此範圍。如果您只啟用專用服務端點，可以使用 Kubernetes 儀表板或暫時啟用公用服務端點以建立專用 NLB。
{: shortdesc}

1. 如果您的網路受到公司防火牆保護，請允許對 {{site.data.keyword.Bluemix_notm}} 及 {{site.data.keyword.containerlong_notm}} API 端點和埠的存取。
  1. [在防火牆中容許存取 `ibmcloud` API 和 `ibmcloud ks` API 的公用端點](/docs/containers?topic=containers-firewall#firewall_bx)。
  2. [容許授權叢集使用者執行 `kubectl` 指令](/docs/containers?topic=containers-firewall#firewall_kubectl)。請注意，在使用專用 NLB 將主節點的專用服務端點公開給叢集之前，無法在步驟 6 中測試與叢集的連線。

2. 取得叢集的專用服務端點 URL 和埠。
  ```
  ibmcloud ks cluster-get --cluster <cluster_name_or_ID>
  ```
  {: pre}

  在此範例輸出中，**專用服務端點 URL** 為 `https://c1.private.us-east.containers.cloud.ibm.com:25073`。
  ```
  Name:                           setest
  ID:                             b8dcc56743394fd19c9f3db7b990e5e3
  State:                          normal
  Created:                        2019-04-25T16:03:34+0000
  Location:                       wdc04
  Master URL:                     https://c1.private.us-east.containers.cloud.ibm.com:25073
  Public Service Endpoint URL:    -
  Private Service Endpoint URL:   https://c1.private.us-east.containers.cloud.ibm.com:25073
  Master Location:                Washington D.C.
  ...
  ```
  {: screen}

3. 建立名為 `kube-api-via-nlb.yaml` 的 YAML 檔案。此 YAML 建立專用 `LoadBalancer` 服務，並透過該 NLB 來公開專用服務端點。將 `<private_service_endpoint_port>` 取代為在上一步中找到的埠。
  ```
  apiVersion: v1
  kind: Service
  metadata:
    name: kube-api-via-nlb
    annotations:
      service.kubernetes.io/ibm-load-balancer-cloud-provider-ip-type: private
    namespace: default
  spec:
    type: LoadBalancer
    ports:
    - protocol: TCP
      port: <private_service_endpoint_port>
      targetPort: <private_service_endpoint_port>
  ---
  kind: Endpoints
  apiVersion: v1
  metadata:
    name: kube-api-via-nlb
  subsets:
    - addresses:
        - ip: 172.20.0.1
      ports:
        - port: 2040
  ```
  {: codeblock}

4. 若要建立專用 NLB，必須連接至叢集主節點。由於尚無法透過專用服務端點經由 VPN 或 {{site.data.keyword.Bluemix_notm}} Direct Link 建立連接，因此必須使用公用服務端點或 Kubernetes 儀表板來連接至叢集主節點並建立 NLB。
  * 如果僅已啟用專用服務端點，則可以使用 Kubernetes 儀表板來建立 NLB。該儀表板會自動將所有要求遞送到主節點的專用服務端點。
    1.  登入 [{{site.data.keyword.Bluemix_notm}} 主控台](https://cloud.ibm.com/)。
    2.  從功能表列，選取您要使用的帳戶。
    3.  從功能表 ![「功能表」圖示](../icons/icon_hamburger.svg "「功能表」圖示") 中，按一下 **Kubernetes**。
    4.  在**叢集**頁面上，按一下您要存取的叢集。
    5.  在叢集詳細資料頁面中，按一下 **Kubernetes 儀表板**。
    6.  按一下 **+ 建立**。
    7.  選取**從檔案建立**，上傳 `kube-api-via-nlb.yaml` 檔案，然後按一下**上傳**。
    8.  在**概觀**頁面中，驗證 `kuba-api-via-nlb` 服務是否已建立。在**外部端點**直欄中，記下 `10.x.x.x` 位址。這個 IP 位址會在您於 YAML 檔中指定的埠上，公開 Kubernetes 主節點的專用服務端點。

  * 如果還已啟用了公用服務端點，則您已有權存取主節點。
    1. 建立 NLB 和端點。
      ```
      kubectl apply -f kube-api-via-nlb.yaml
      ```
      {: pre}
    2. 驗證 `kube-api-via-nlb` NLB 是否已建立。在輸出中，記下 `10.x.x.x` **EXTERNAL-IP** 位址。這個 IP 位址會在您於 YAML 檔中指定的埠上，公開 Kubernetes 主節點的專用服務端點。
      ```
      kubectl get svc -o wide
      ```
      {: pre}

      在此範例輸出中，Kubernetes 主節點的專用服務端點的 IP 位址為 `10.186.92.42`。
      ```
      NAME                     TYPE           CLUSTER-IP       EXTERNAL-IP      PORT(S)          AGE   SELECTOR
      kube-api-via-nlb         LoadBalancer   172.21.150.118   10.186.92.42     443:32235/TCP    10m   <none>
      ...
      ```
      {: screen}

  <p class="note">如果要使用 [strongSwan VPN 服務](/docs/containers?topic=containers-vpn#vpn-setup)連接至主節點，請改為記下 `172.21.x.x` **叢集 IP** 以在下一步中使用。由於 strongSwan VPN Pod 是在叢集內部執行，因此它可以使用內部叢集 IP 服務的 IP 位址來存取 NLB。在 strongSwan Helm 圖表的 `config.yaml` 檔案中，確保 Kubernetes 服務子網路 CIDR `172.21.0.0/16` 列出在 `local.subnet` 設定中。</p>

5. 在您或使用者執行 `kubectl` 指令的用戶端上，將 NLB IP 位址和專用服務端點 URL 新增到 `/etc/hosts` 檔案。不要在 IP 位址和 URL 中包含任何埠，並且不要在 URL 中包含 `https://`。
  * 對於 OSX 和 Linux 使用者：
    ```
    sudo nano /etc/hosts
    ```
    {: pre}
  * 對於 Windows 使用者：
    ```
    notepad C:\Windows\System32\drivers\etc\hosts
    ```
    {: pre}

    根據本端機器許可權，您可能需要以管理者身分執行記事本來編輯 hosts 檔案。
    {: tip}

  要新增的範例文字：
  ```
  10.186.92.42      c1.private.us-east.containers.cloud.ibm.com
  ```
  {: codeblock}

6. 驗證是否已透過 VPN 或 {{site.data.keyword.Bluemix_notm}} Direct Link 連線來連接至專用網路。

7. 透過檢查 Kubernetes CLI 伺服器版本，驗證 `kubectl` 指令是否透過專用服務端點針對您的叢集正常執行。
  ```
  kubectl version --short
  ```
  {: pre}

  輸出範例：
  ```
  Client Version: v1.13.6
  Server Version: v1.13.6
  ```
  {: screen}

<br />


## 後續步驟
{: #next_steps}

當叢集開始執行時，您可以查看下列作業：
- 如果在具有多區域功能的區域中建立了叢集，請[透過向叢集新增區域來展開工作者節點](/docs/containers?topic=containers-add_workers)。
- [在叢集裡部署應用程式。](/docs/containers?topic=containers-app#app_cli)
- [在 {{site.data.keyword.Bluemix_notm}} 中設定您自己的專用登錄，以儲存 Docker 映像檔，並將它與其他使用者共用。](/docs/services/Registry?topic=registry-getting-started)
- [設定叢集 autoscaler](/docs/containers?topic=containers-ca#ca)，以根據工作負載資源要求來自動新增或移除工作者節點儲存區中的工作者節點。
- 控制誰可以使用 [Pod 安全原則](/docs/containers?topic=containers-psp)在您的叢集裡建立 Pod。
- 啟用 [Istio](/docs/containers?topic=containers-istio) 和 [Knative](/docs/containers?topic=containers-serverless-apps-knative) 受管理附加程式以延伸叢集功能。

然後，可以查看用於叢集設定的下列網路配置步驟：

### 在叢集裡執行面對網際網路的應用程式工作負載
{: #next_steps_internet}

* 將網路工作負載隔離至[邊緣工作者節點](/docs/containers?topic=containers-edge)。
* 使用[公用網路連線功能服務](/docs/containers?topic=containers-cs_network_planning#public_access)公開應用程式。
* 透過建立 [Calico DNAT 前原則](/docs/containers?topic=containers-network_policies#block_ingress)（例如，白名單和黑名單原則），控制傳送至用於公開應用程式的網路服務的公用資料流量。
* 透過設定 [strongSwan IPSec VPN 服務](/docs/containers?topic=containers-vpn)，將叢集與 {{site.data.keyword.Bluemix_notm}} 帳戶外部的專用網路中的服務相連接。

### 將內部部署資料中心延伸到叢集，並容許透過邊緣節點和 Calico 網路原則提供有限公用存取權
{: #next_steps_calico}

* 透過設定 [{{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link) 或 [strongSwan IPSec VPN 服務](/docs/containers?topic=containers-vpn#vpn-setup)，將叢集與 {{site.data.keyword.Bluemix_notm}} 帳戶外部的專用網路中的服務相連接。{{site.data.keyword.Bluemix_notm}} Direct Link 容許叢集裡的應用程式和服務透過專用網路與內部部署網路進行通訊，而 strongSwan 容許透過加密的 VPN 通道在公用網路上進行通訊。
* 透過建立連接至公用和專用 VLAN 的工作者節點的[邊緣工作者節點儲存區](/docs/containers?topic=containers-edge)，隔離公用網路連線功能工作負載。
* 使用[專用網路服務](/docs/containers?topic=containers-cs_network_planning#private_access)公開應用程式。
* [建立 Calico 主機網路原則](/docs/containers?topic=containers-network_policies#isolate_workers)，以阻止對 Pod 的公用存取，隔離專用網路上的叢集，並容許存取其他 {{site.data.keyword.Bluemix_notm}} 服務。

### 將內部部署資料中心延伸到叢集，並容許透過閘道裝置提供有限公用存取權
{: #next_steps_gateway}

* 如果您還為專用網路配置了閘道防火牆，則必須[容許工作者節點之間的通訊，並且使叢集透過專用網路存取基礎架構資源](/docs/containers?topic=containers-firewall#firewall_private)。
* 若要將工作者節點和應用程式安全地連接至 {{site.data.keyword.Bluemix_notm}} 帳戶外部的專用網路，請在閘道裝置上設定 IPSec VPN 端點。然後，在叢集裡[配置 strongSwan IPSec VPN 服務](/docs/containers?topic=containers-vpn#vpn-setup)，以在閘道上使用 VPN 端點，或者[直接使用 VRA 設定 VPN 連線功能](/docs/containers?topic=containers-vpn#vyatta)。
* 使用[專用網路服務](/docs/containers?topic=containers-cs_network_planning#private_access)公開應用程式。
* 在閘道裝置防火牆中[開啟必要的埠和 IP 位址](/docs/containers?topic=containers-firewall#firewall_inbound)，以允許傳送至網路連線功能服務的入埠資料流量。

### 將內部部署資料中心延伸到僅限專用網路上的叢集
{: #next_steps_extend}

* 如果您在專用網路上有一個防火牆，則[容許工作者節點之間的通訊，並且使叢集透過專用網路存取基礎架構資源](/docs/containers?topic=containers-firewall#firewall_private)。
* 透過設定 [{{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link)，將叢集與 {{site.data.keyword.Bluemix_notm}} 帳戶外部的專用網路中的服務相連接。
* 使用[專用網路連線功能服務](/docs/containers?topic=containers-cs_network_planning#private_access)在專用網路上公開應用程式。
