---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-07"

keywords: kubernetes, iks

subcollection: containers

---

{:new_window: target="blank"}
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


# 已淘汰：專用雲端
{: #dedicated}

已淘汰 {{site.data.keyword.Bluemix_dedicated_notm}} 中的 {{site.data.keyword.containerlong}}。您無法在 {{site.data.keyword.Bluemix_dedicated_notm}} 環境中建立叢集。若要在 {{site.data.keyword.Bluemix_notm}} Public 中建立叢集，請參閱[開始使用 {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-getting-started)。
{: deprecated}

如果您具有 {{site.data.keyword.Bluemix_dedicated_notm}} 帳戶，則可以在專用雲端環境 (`https://<my-dedicated-cloud-instance>.bluemix.net`) 中部署 Kubernetes 叢集，並與也在該處執行的預先選取 {{site.data.keyword.Bluemix_notm}} 服務連接。
{:shortdesc}

如果您沒有 {{site.data.keyword.Bluemix_dedicated_notm}} 帳戶，則可以在公用 {{site.data.keyword.Bluemix_notm}} 帳戶中[開始使用 {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-getting-started)。

## 關於專用雲端環境
{: #dedicated_environment}

使用 {{site.data.keyword.Bluemix_dedicated_notm}} 帳戶，可用的實體資源只供您的叢集專用，無法與其他 {{site.data.keyword.IBM_notm}} 客戶的叢集共用。當想要隔離您的叢集，而且需要隔離您所使用的其他 {{site.data.keyword.Bluemix_notm}} 服務時，您可以選擇設定 {{site.data.keyword.Bluemix_dedicated_notm}} 環境。如果您沒有「專用」帳戶，則可以[在 {{site.data.keyword.Bluemix_notm}} Public 建立具有專用硬體的叢集](/docs/containers?topic=containers-clusters#clusters_ui)。
{: shortdesc}

使用 {{site.data.keyword.Bluemix_dedicated_notm}}，您可以在「專用」主控台中從型錄建立叢集，或使用 {{site.data.keyword.containerlong_notm}} CLI 來建立叢集。若要使用「專用」主控台，請同時使用 IBM ID 來登入「專用」及公用帳戶。您可以使用雙重登入，使用「專用」主控台來存取公用叢集。若要使用 CLI，請使用 Dedicated 端點 (`api.<my-dedicated-cloud-instance>.bluemix.net.`) 進行登入。然後，將目標設為與「專用」環境相關聯之公用地區的 {{site.data.keyword.containerlong_notm}} API 端點。

{{site.data.keyword.Bluemix_notm}} Public 與 Dedicated 之間的最重要差異如下。

*   在 {{site.data.keyword.Bluemix_dedicated_notm}} 中，{{site.data.keyword.IBM_notm}} 擁有並管理在其中部署工作者節點、VLAN 及子網路的 IBM Cloud 基礎架構 (SoftLayer) 帳戶。在 {{site.data.keyword.Bluemix_notm}} Public 環境中，您擁有 IBM Cloud 基礎架構 (SoftLayer) 帳戶。
*   在 {{site.data.keyword.Bluemix_dedicated_notm}} 中，會在啟用「專用」環境時，決定 {{site.data.keyword.IBM_notm}} 管理的 IBM Cloud 基礎架構 (SoftLayer) 帳戶中的 VLAN 及子網路的規格。在 {{site.data.keyword.Bluemix_notm}} Public 中，VLAN 及子網路的規格是在建立叢集時決定的。

### 雲端環境之間的叢集管理差異
{: #dedicated_env_differences}

<table>
<caption>叢集管理的差異</caption>
<col width="20%">
<col width="40%">
<col width="40%">
 <thead>
 <th>範疇</th>
 <th>{{site.data.keyword.Bluemix_notm}} Public</th>
 <th>{{site.data.keyword.Bluemix_dedicated_notm}}</th>
 </thead>
 <tbody>
 <tr>
 <td>叢集建立</td>
 <td>建立免費叢集或標準叢集。</td>
 <td>建立標準叢集。</td>
 </tr>
 <tr>
 <td>叢集硬體及所有權</td>
 <td>在標準叢集裡，硬體可以與其他 {{site.data.keyword.IBM_notm}} 客戶共用，或者只供您專用。在您的 IBM Cloud 基礎架構 (SoftLayer) 帳戶中，是由您擁有及管理公用及專用 VLAN。</td>
 <td>在 {{site.data.keyword.Bluemix_dedicated_notm}} 的叢集裡，硬體一律是專用的。用於建立叢集的公用及專用 VLAN 是在設定 {{site.data.keyword.Bluemix_dedicated_notm}} 環境時預先定義的，而且為 IBM 所擁有，並為您管理。建立叢集期間可用的區域也是針對 {{site.data.keyword.Bluemix_notm}} 環境預先定義的。</td>
 </tr>
 <tr>
 <td>負載平衡器及 Ingress 網路</td>
 <td>在佈建標準叢集期間，會自動執行下列動作。<ul><li>一個可攜式公用子網路及一個可攜式專用子網路會連結至叢集，並指派給您的 IBM Cloud 基礎架構 (SoftLayer) 帳戶。您可以透過 IBM Cloud 基礎架構 (SoftLayer) 帳戶要求其他子網路。</li></li><li>一個可攜式公用 IP 位址用於高可用性 Ingress 應用程式負載平衡器 (ALB)，並指派了格式為 <code>&lt;cluster_name&gt;. containers.appdomain.cloud</code> 的唯一公共路徑。您可以使用此路徑將多個應用程式公開給大眾使用。一個可攜式專用 IP 位址用於專用 ALB。</li><li>四個可攜式公用及四個可攜式專用 IP 位址會指派給可用於負載平衡器服務的叢集。</ul></td>
 <td>當您建立「專用」帳戶時，您會決定要如何公開及存取叢集服務的連線功能。若要使用自己的企業 IP 範圍（使用者管理的 IP），則必須在[設定 {{site.data.keyword.Bluemix_dedicated_notm}} 環境](/docs/dedicated?topic=dedicated-dedicated#setupdedicated)時提供它們。<ul><li>依預設，不會將任何可攜式公用子網路連結至您在「專用」帳戶中建立的叢集。相反地，您可以彈性地選擇最適合您企業的連線功能模型。</li><li>在您建立叢集之後，請針對負載平衡器或 Ingress 連線功能，選擇您要連結以及與叢集搭配使用的子網路類型。<ul><li>針對公用或專用可攜式子網路，您可以[將子網路新增至叢集](/docs/containers?topic=containers-subnets#subnets)</li><li>針對您在「專用」上線時提供給 IBM 之使用者管理的 IP 位址，您可以[將使用者管理的子網路新增至叢集](#dedicated_byoip_subnets)。</li></ul></li><li>將子網路連結至叢集之後，即會建立 Ingress ALB。只有在您使用可攜式公用子網路時，才會建立公用 Ingress 路徑。</li></ul></td>
 </tr>
 <tr>
 <td>NodePort 網路</td>
 <td>公開工作者節點上的公用埠，並使用工作者節點的公用 IP 位址來公開存取您在叢集裡的服務。</td>
 <td>工作者節點的所有公用 IP 位址都會遭到防火牆封鎖。不過，針對新增至叢集的 {{site.data.keyword.Bluemix_notm}} 服務，可以透過公用 IP 位址或專用 IP 位址存取 NodePort。</td>
 </tr>
 <tr>
 <td>持續性儲存空間</td>
 <td>使用磁區的[動態佈建](/docs/containers?topic=containers-kube_concepts#dynamic_provisioning)或[靜態佈建](/docs/containers?topic=containers-kube_concepts#static_provisioning)。</td>
 <td>使用磁區的[動態佈建](/docs/containers?topic=containers-kube_concepts#dynamic_provisioning)。[開立支援案例](/docs/get-support?topic=get-support-getting-customer-support)以要求備份磁區、要求從磁區還原，以及執行其他儲存空間功能。</li></ul></td>
 </tr>
 <tr>
 <td>{{site.data.keyword.registryshort_notm}} 中的映像檔登錄 URL</td>
 <td><ul><li>美國南部及美國東部：<code>registry.ng bluemix.net</code></li><li>英國南部：<code>registry.eu-gb.bluemix.net</code></li><li>歐盟中部（法蘭克福）：<code>registry.eu-de.bluemix.net</code></li><li>澳洲（雪梨）：<code>registry.au-syd.bluemix.net</code></li></ul></td>
 <td><ul><li>若為新的名稱空間，請使用針對 {{site.data.keyword.Bluemix_notm}} 公用所定義的相同地區型登錄。</li><li>若為已針對 {{site.data.keyword.Bluemix_dedicated_notm}} 中的單一及可擴充容器所設定的名稱空間，請使用 <code>registry.&lt;dedicated_domain&gt;</code></li></ul></td>
 </tr>
 <tr>
 <td>存取登錄</td>
 <td>請參閱[搭配使用專用及公用映像檔登錄與 {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-images) 中的選項。</td>
 <td><ul><li>若為新的名稱空間，請參閱[搭配使用專用及公用映像檔登錄與 {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-images) 中的選項。</li><li>若為已針對單一及可擴充群組所設定的名稱空間，請[使用記號及建立 Kubernetes 密碼](#cs_dedicated_tokens)來進行鑑別。</li></ul></td>
 </tr>
 <tr>
 <td>多區域叢集</td>
 <td>藉由將其他區域新增至工作者節點儲存區，來建立[多區域叢集](/docs/containers?topic=containers-ha_clusters#multizone)。</td>
 <td>建立[單一區域叢集](/docs/containers?topic=containers-ha_clusters#single_zone)。在設定 {{site.data.keyword.Bluemix_dedicated_notm}} 環境時，已預先定義可用區域。依預設，單一區域叢集已設定名稱為 `default` 的工作者節點儲存區。工作者節點儲存區會將具有您在建立叢集期間所定義之相同配置（例如機型）的工作者節點分組在一起。您可以藉由[調整現有工作者節點儲存區大小](/docs/containers?topic=containers-add_workers#resize_pool)或[新增工作者節點儲存區](/docs/containers?topic=containers-add_workers#add_pool)，來將更多工作者節點新增至叢集裡。當您新增工作者節點儲存區時，必須將可用區域新增至工作者節點儲存區，讓工作者節點可以部署至該區域。不過，您無法將其他區域新增至工作者節點儲存區。</td>
 </tr>
</tbody></table>
{: caption="{{site.data.keyword.Bluemix_notm}} 公用與 {{site.data.keyword.Bluemix_dedicated_notm}} 的特性差異" caption-side="top"}

<br />



### 服務架構
{: #dedicated_ov_architecture}

每一個工作者節點都已設定不同的運算資源、網路及磁區服務。
{:shortdesc}

內建安全特性可提供隔離、資源管理功能及工作者節點安全規範。工作者節點會使用安全 TLS 憑證及 OpenVPN 連線來與主節點進行通訊。


*{{site.data.keyword.Bluemix_dedicated_notm}} 中的 Kubernetes 架構及網路*

![{{site.data.keyword.containerlong_notm}} {{site.data.keyword.Bluemix_dedicated_notm}}](images/cs_dedicated_arch.png)

<br />


## 在專用上設定 {{site.data.keyword.containerlong_notm}}
{: #dedicated_setup}

每一個 {{site.data.keyword.Bluemix_dedicated_notm}} 環境都具有 {{site.data.keyword.Bluemix_notm}} 中的公用、用戶端擁有的公司帳戶。為了讓「專用」環境中的使用者建立叢集，管理者必須將使用者新增至公用公司帳戶。
{:shortdesc}

開始之前：
  * [設定 {{site.data.keyword.Bluemix_dedicated_notm}} 環境](/docs/dedicated?topic=dedicated-dedicated#setupdedicated)。
  * 如果您的本端系統或組織網路使用 Proxy 或防火牆控制公用網際網路端點，則必須[在您的防火牆中開啟必要的埠和 IP 位址](/docs/containers?topic=containers-firewall#firewall)。
  * [下載 Cloud Foundry CLI ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/cloudfoundry/cli/releases)。

若要容許 {{site.data.keyword.Bluemix_dedicated_notm}} 使用者存取叢集，請執行下列動作：

1.  公用 {{site.data.keyword.Bluemix_notm}} 帳戶的擁有者必須產生 API 金鑰。
    1.  登入 {{site.data.keyword.Bluemix_dedicated_notm}} 實例的端點。輸入公用帳戶擁有者的 {{site.data.keyword.Bluemix_notm}} 認證，並在系統提示時選取您的帳戶。

        ```
        ibmcloud login -a api.<my-dedicated-cloud-instance>.<region>.bluemix.net
        ```
        {: pre}

        如果您具有聯合 ID，請使用 `ibmcloud login -a api.<my-dedicated-cloud-instance>.<region>.bluemix.net --sso` 來登入 {{site.data.keyword.Bluemix_notm}} CLI。請輸入使用者名稱，並使用 CLI 輸出中提供的 URL 來擷取一次性密碼。若沒有 `--sso` 時登入失敗，而有 `--sso` 選項時登入順利，即表示您有聯合 ID。
        {: tip}

    2.  產生用於邀請使用者加入公用帳戶的 API 金鑰。請記下 API 金鑰值，因為「專用」帳戶管理者必須在下一步中使用它。

        ```
        ibmcloud iam api-key-create <key_name> -d "Key to invite users to <dedicated_account_name>"
        ```
        {: pre}

    3.  記下您要邀請使用者加入的公用帳戶組織的 GUID，因為「專用」帳戶管理者必須在下一步中使用它。

        ```
        ibmcloud account orgs
        ```
        {: pre}

2.  {{site.data.keyword.Bluemix_dedicated_notm}} 帳戶的擁有者可以邀請單一或多位使用者加入您的「公用」帳戶。
    1.  登入 {{site.data.keyword.Bluemix_dedicated_notm}} 實例的端點。輸入「專用」帳戶擁有者的 {{site.data.keyword.Bluemix_notm}} 認證，並在系統提示時選取您的帳戶。

        ```
        ibmcloud login -a api.<my-dedicated-cloud-instance>.<region>.bluemix.net
        ```
        {: pre}

        如果您具有聯合 ID，請使用 `ibmcloud login -a api.<my-dedicated-cloud-instance>.<region>.bluemix.net --sso` 來登入 {{site.data.keyword.Bluemix_notm}} CLI。請輸入使用者名稱，並使用 CLI 輸出中提供的 URL 來擷取一次性密碼。若沒有 `--sso` 時登入失敗，而有 `--sso` 選項時登入順利，即表示您有聯合 ID。
        {: tip}

    2.  邀請使用者加入公用帳戶。
        * 若要邀請單一使用者，請執行下列指令：

            ```
            ibmcloud cf bluemix-admin invite-users-to-public -userid=<user_email> -apikey=<public_API_key> -public_org_id=<public_org_ID>
            ```
            {: pre}

            將 <em>&lt;user_IBMid&gt;</em> 取代為您要邀請之使用者的電子郵件、將 <em>&lt;public_API_key&gt;</em> 取代為前一個步驟中所產生的 API 金鑰，並將 <em>&lt;public_org_ID&gt;</em> 取代為公用帳戶組織的 GUID。 

        * 若要邀請目前在「專用」帳戶組織中的所有使用者，請執行下列指令：

            ```
            ibmcloud cf bluemix-admin invite-users-to-public -organization=<dedicated_org_ID> -apikey=<public_API_key> -public_org_id=<public_org_ID>
            ```

            將 <em>&lt;dedicated_org_ID&gt;</em> 取代為「專用」帳戶組織 ID、將 <em>&lt;public_API_key&gt;</em> 取代為前一個步驟中所產生的 API 金鑰，並將 <em>&lt;public_org_ID&gt;</em> 取代為公用帳戶組織的 GUID。 

    3.  如果使用者有 IBM ID，則會自動將使用者新增至公用帳戶中的指定組織。如果使用者沒有 IBM ID，則會將邀請傳送到使用者的電子郵件位址。在使用者接受邀請之後，就會為使用者建立 IBM ID，並將使用者新增至公用帳戶中的指定組織。

    4.  驗證已將使用者新增至帳戶。

        ```
        ibmcloud cf bluemix-admin invite-users-status -apikey=<public_API_key>
        ```
        {: pre}

        具有現有 IBM ID 的受邀使用者具有 `ACTIVE` 狀態。在接受邀請之前，沒有現有 IBM ID 的受邀使用者具有 `PENDING` 狀態，而在接受邀請之後，則具有 `ACTIVE` 狀態。

3.  如果任何使用者需要叢集建立專用權，您必須授與管理者角色給該位使用者。

    1.  從公用主控台的功能表列，按一下**管理 > 安全 > 身分及存取**，然後按一下**使用者**。

    2.  在要為其指派存取權的使用者所在的行中，選取**動作**功能表，然後按一下**指派存取權**。

    3.  選取**指派對資源的存取權**。

    4.  從**服務**清單，選取 **{{site.data.keyword.containerlong}}**。

    5.  從**地區**清單，選取**所有現行地區**或特定的地區（如果您收到提示的話）。

    6. 在**選取角色**下，選取「管理者」。

    7. 按一下**指派**。

4.  使用者現在可以登入「專用」帳戶端點來開始建立叢集。

    1.  登入 {{site.data.keyword.Bluemix_dedicated_notm}} 實例的端點。當系統提示時，請輸入 IBM ID。

        ```
        ibmcloud login -a api.<my-dedicated-cloud-instance>.<region>.bluemix.net
        ```
        {: pre}

        如果您具有聯合 ID，請使用 `ibmcloud login -a api.<my-dedicated-cloud-instance>.<region>.bluemix.net --sso` 來登入 {{site.data.keyword.Bluemix_notm}} CLI。請輸入使用者名稱，並使用 CLI 輸出中提供的 URL 來擷取一次性密碼。若沒有 `--sso` 時登入失敗，而有 `--sso` 選項時登入順利，即表示您有聯合 ID。
        {: tip}

    2.  如果您是第一次登入，請在系統提示時提供您的「專用」使用者 ID 及密碼。您的「專用」帳戶會進行鑑別，而且「專用」與公用帳戶會鏈結在一起。在第一次登入後的每次登入，都只需使用您的 IBM ID 來登入。如需相關資訊，請參閱[將專用 ID 連接至公用 IBM ID](/docs/iam?topic=iam-connect_dedicated_id#connect_dedicated_id)。

        您必須同時登入「專用」帳戶及公用帳戶，才能建立叢集。如果您只要登入「專用」帳戶，請在登入「專用」端點時使用 `--no-iam` 旗標。
        {: note}

    3.  若要在專用環境中建立或存取叢集，您必須設定與該環境相關聯的地區。**附註**：您無法在 `default` 以外的資源群組中建立叢集。

        ```
        ibmcloud ks region-set
        ```
        {: pre}

5.  如果您要解除鏈結您的帳戶，則可以中斷 IBM ID 與「專用」使用者 ID 的連線。如需相關資訊，請參閱[中斷專用 ID 與公用 IBM ID 的連線](/docs/iam?topic=iam-connect_dedicated_id#disconnect_id)。

    ```
    ibmcloud iam dedicated-id-disconnect
    ```
    {: pre}

<br />


## 建立叢集
{: #dedicated_administering}

設計最大可用性及容量的 {{site.data.keyword.Bluemix_dedicated_notm}} 叢集設定。
{:shortdesc}

### 使用 {{site.data.keyword.Bluemix_notm}} 主控台建立叢集
{: #dedicated_creating_ui}

1. 開啟「專用」主控台：`https://<my-dedicated-cloud-instance>.bluemix.net`。

2. 選取**也登入 {{site.data.keyword.Bluemix_notm}} Public** 勾選框，然後按一下**登入**。

3. 遵循提示，以使用 IBM ID 登入。如果您是第一次登入「專用」帳戶，請遵循提示來登入 {{site.data.keyword.Bluemix_dedicated_notm}}。

4. 從型錄中，選取**容器**，然後按一下 **Kubernetes 叢集**。

5. 配置叢集詳細資料。

    1. 輸入**叢集名稱**。此名稱必須以字母開頭，可以包含字母、數字及連字號 (-)，且長度不得超過 35 個字元。請使用在各地區之間都唯一的名稱。叢集名稱和叢集部署所在的地區會形成 Ingress 子網域的完整網域名稱。為了確保 Ingress 子網域在地區內是唯一的，叢集名稱可能會被截斷，並在 Ingress 網域名稱內附加一個隨機值。

    2. 選取要在其中部署叢集的**區域**。在設定 {{site.data.keyword.Bluemix_dedicated_notm}} 環境時，已預先定義可用區域。

    3. 選擇叢集主節點的 Kubernetes API 伺服器版本。

    4. 選取硬體隔離的類型。虛擬伺服器是依小時計費，裸機伺服器是依月計費。

        - **虛擬 - 專用**：工作者節點是在您帳戶專用的基礎架構上進行管理。實體資源完全被隔離。

        - **裸機**：當您訂購之後，依月計費的裸機伺服器將由 IBM Cloud 基礎架構 (SoftLayer) 進行手動佈建，這可能需要多個營業日才能完成。裸機伺服器最適合需要更多資源和主機控制的高效能應用程式。對於所選機型，還可以選取啟用[授信運算](/docs/containers?topic=containers-security#trusted_compute)來驗證工作者節點是否被篡改。如果您未在建立叢集期間啟用信任，但後來想要啟用，則可以使用 `ibmcloud ks feature-enable` [指令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable)。啟用信任之後，以後您就無法再予以停用。

        請確定您要佈建裸機機器。因為它是依月計費，如果您不慎下訂之後立即取消，仍會向您收取整個月的費用。
        {:tip}

    5. 選取**機型**。機型會定義設定於每一個工作者節點中，且可供容器使用的虛擬 CPU、記憶體及磁碟空間量。可用的裸機及虛擬機器類型會因您部署叢集所在的區域而不同。如需相關資訊，請參閱 `ibmcloud ks machine-type` [指令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_machine_types)的文件。建立叢集之後，您可以藉由將工作者節點新增至叢集來新增不同的機型。

    6. 選擇您需要的**工作者節點數目**。選取 `3`，確保叢集的高可用性。

    7. 選取**公用 VLAN**（選用）和**專用 VLAN**（必要）。在設定 {{site.data.keyword.Bluemix_dedicated_notm}} 環境時，會預先定義可用的公用和專用 VLAN。兩個 VLAN 會在工作者節點之間進行通訊，但公用 VLAN 也與 IBM 管理的 Kubernetes 主節點通訊。您可以將相同的 VLAN 用於多個叢集。如果工作者節點設定為具有僅限專用 VLAN，則您必須[啟用專用服務端點](/docs/containers?topic=containers-cs_network_cluster#set-up-private-se)或[配置閘道裝置](/docs/containers?topic=containers-plan_clusters#workeruser-master)，以容許工作者節點與叢集主節點通訊。
        {: note}

    8. 依預設，會選取**加密本端磁碟**。如果您選擇清除這個勾選框，則主機的容器運行環境資料不會加密。[進一步瞭解加密](/docs/containers?topic=containers-security#encrypted_disk)。

6. 按一下**建立叢集**。您可以在**工作者節點**標籤中查看工作者節點部署的進度。部署完成時，您可以在**概觀**標籤中看到叢集已備妥。每個工作者節點都會獲指派唯一的工作者節點 ID 及網域名稱，其在建立叢集之後即不得手動變更。變更 ID 或網域名稱會讓 Kubernetes 主節點無法管理叢集。
    {: important}

### 使用 CLI 建立叢集
{: #dedicated_creating_cli}

1.  安裝 {{site.data.keyword.Bluemix_notm}} CLI 及 [{{site.data.keyword.containerlong_notm}} 外掛程式](/docs/containers?topic=containers-cs_cli_install#cs_cli_install)。
2.  登入 {{site.data.keyword.Bluemix_dedicated_notm}} 實例的端點。當系統提示時，請輸入 {{site.data.keyword.Bluemix_notm}} 認證，然後選取帳戶。

    ```
    ibmcloud login -a api.<my-dedicated-cloud-instance>.<region>.bluemix.net
    ```
    {: pre}

    如果您具有聯合 ID，請使用 `ibmcloud login -a api.<my-dedicated-cloud-instance>.<region>.bluemix.net --sso` 來登入 {{site.data.keyword.Bluemix_notm}} CLI。請輸入使用者名稱，並使用 CLI 輸出中提供的 URL 來擷取一次性密碼。若沒有 `--sso` 時登入失敗，而有 `--sso` 選項時登入順利，即表示您有聯合 ID。
        {: tip}

3.  將地區端點設定為目標。支援下列地區端點：
  * 達拉斯（美國南部、us-south）：`https://us-south.containers.cloud.ibm.com`
  * 法蘭克福（歐盟中部、eu-de）：`https://eu-de.containers.cloud.ibm.com`
  * 倫敦（英國南部、eu-gb）：`https://eu-gb.containers.cloud.ibm.com`
  * 雪梨（亞太地區南部、au-syd）：`https://au-syd.containers.cloud.ibm.com`
  * 東京（亞太地區北部、jp-tok）：`https://jp-tok.containers.cloud.ibm.com`
  * 華盛頓特區（美國東部、us-east）：`https://us-east.containers.cloud.ibm.com`
  ```
  ibmcloud ks init --host <endpoint>
  ```
  {: pre}
  不能使用廣域端點 `https://containers.cloud.ibm.com`。必須將地區端點設定為目標，才能建立或使用該地區中的叢集。
  {: important}

4.  使用 `cluster-create` 指令來建立叢集。當您建立標準叢集時，工作者節點的硬體會依小時計費。

    範例：

    ```
    ibmcloud ks cluster-create --zone <zone> --machine-type <machine_type> --name <cluster_name> --workers <number>
    ```
    {: pre}

    <table>
    <caption>瞭解這個指令的元件</caption>
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
    <td>輸入您配置「專用」環境使用的 {{site.data.keyword.Bluemix_notm}} 區域 ID。</td>
    </tr>
    <tr>
    <td><code>--machine-type <em>&lt;machine_type&gt;</em></code></td>
    <td>輸入機型。您可以將工作者節點部署為專用硬體上的虛擬機器，或部署為裸機上的實體機器。可用的實體及虛擬機器類型會因您部署叢集所在的區域而不同。如需相關資訊，請參閱 `ibmcloud ks machine-type` [指令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_machine_types)的文件。</td>
    </tr>
    <tr>
    <td><code>--public-vlan <em>&lt;machine_type&gt;</em></code></td>
    <td>輸入您配置「專用」環境使用之公用 VLAN 的 ID。如果您只要將工作者節點連接至專用 VLAN，請不要指定此選項。<p class="note">如果工作者節點設定為具有僅限專用 VLAN，則您必須[啟用專用服務端點](/docs/containers?topic=containers-cs_network_cluster#set-up-private-se)或[配置閘道裝置](/docs/containers?topic=containers-plan_clusters#workeruser-master)，以容許工作者節點與叢集主節點通訊。</p></td>
    </tr>
    <tr>
    <td><code>--private-vlan <em>&lt;machine_type&gt;</em></code></td>
    <td>輸入您配置「專用」環境使用之專用 VLAN 的 ID。</td>
    </tr>  
    <tr>
    <td><code>--name <em>&lt;name&gt;</em></code></td>
    <td>輸入叢集的名稱。此名稱必須以字母開頭，可以包含字母、數字及連字號 (-)，且長度不得超過 35 個字元。請使用在各地區之間都唯一的名稱。叢集名稱和叢集部署所在的地區會形成 Ingress 子網域的完整網域名稱。為了確保 Ingress 子網域在地區內是唯一的，叢集名稱可能會被截斷，並在 Ingress 網域名稱內附加一個隨機值。</td>
    </tr>
    <tr>
    <td><code>--workers <em>&lt;number&gt;</em></code></td>
    <td>輸入要包含在叢集裡的工作者節點數目。如果未指定 <code>--workers</code> 選項，則會建立一個工作者節點。</td>
    </tr>
    <tr>
    <td><code>--kube-version <em>&lt;major.minor.patch&gt;</em></code></td>
    <td>叢集主節點的 Kubernetes 版本。這是選用值。未指定版本時，會使用支援的 Kubernetes 版本的預設值來建立叢集。若要查看可用的版本，請執行 <code>ibmcloud ks versions</code>。
</td>
    </tr>
    <tr>
    <td><code>--disable-disk-encrypt</code></td>
    <td>依預設，工作者節點具有 AES 256 位元[磁碟加密](/docs/containers?topic=containers-security#encrypted_disk)功能。如果您要停用加密，請包括此選項。</td>
    </tr>
    <tr>
    <td><code>--trusted</code></td>
    <td>啟用[授信運算](/docs/containers?topic=containers-security#trusted_compute)，以驗證裸機工作者節點是否遭到竄改。如果您未在建立叢集期間啟用信任，但後來想要啟用，則可以使用 `ibmcloud ks feature-enable` [指令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable)。啟用信任之後，以後您就無法再予以停用。</td>
    </tr>
    </tbody></table>

5.  驗證已要求建立叢集。

    ```
    ibmcloud ks clusters
    ```
    {: pre}

    * 若為虛擬機器，要訂購工作者節點機器並且在您的帳戶中設定及佈建叢集，可能需要一些時間。裸機實體機器是透過與 IBM Cloud 基礎架構 (SoftLayer) 之間的手動互動來進行佈建，可能需要多個營業日才能完成。
    * 如果您看到下列錯誤訊息，請[開立支援案例](/docs/get-support?topic=get-support-getting-customer-support)。
        ```
        {{site.data.keyword.Bluemix_notm}} Infrastructure Exception: Could not place order. There are insufficient resources behind router 'router_name' to fulfill the request for the following guests: 'worker_id'.
        ```

    叢集佈建完成之後，叢集的狀態會變更為 **deployed**。

    ```
    Name         ID                                   State      Created          Workers    Zone      Version     Resource Group Name
    my_cluster   paf97e8843e29941b49c598f516de72101   deployed   20170201162433   1          mil01     1.13.6      Default
    ```
    {: screen}

6.  檢查工作者節點的狀態。

    ```
        ibmcloud ks workers --cluster <cluster_name_or_ID>
        ```
    {: pre}

    工作者節點就緒後，狀況會變更為 **Normal**，並且處於 **Ready** 狀態。當節點狀態為 **Ready** 時，您就可以存取叢集。

    每個工作者節點都會獲指派唯一的工作者節點 ID 及網域名稱，而在建立叢集之後不得手動變更。變更 ID 或網域名稱會讓 Kubernetes 主節點無法管理叢集。
    {: important}

    ```
    ID                                                 Public IP       Private IP       Machine Type   State    Status   Zone   Version
    kube-mil01-paf97e8843e29941b49c598f516de72101-w1   169.xx.xxx.xxx  10.xxx.xx.xxx    free           normal   Ready    mil01      1.13.6
    ```
    {: screen}

7.  將您建立的叢集設為此階段作業的環境定義。請在您每次使用叢集時，完成下列配置步驟。

    1.  讓指令設定環境變數，並下載 Kubernetes 配置檔。

        ```
        ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
        ```
        {: pre}

        配置檔下載完成之後，會顯示一個指令，可讓您用來將本端 Kubernetes 配置檔的路徑設定為環境變數。

        OS X 的範例：

        ```
        export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-prod-dal10-<cluster_name>.yml
        ```
        {: screen}

    2.  複製並貼上輸出中的指令，以設定 `KUBECONFIG` 環境變數。
    3.  驗證已適當地設定 `KUBECONFIG` 環境變數。

        OS X 的範例：

        ```
        echo $KUBECONFIG
        ```
        {: pre}

        輸出：

        ```
        /Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-prod-dal10-<cluster_name>.yml

        ```
        {: screen}

8.  使用預設埠 8001 來存取 Kubernetes 儀表板。
    1.  使用預設埠號來設定 Proxy。

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

### 新增工作者節點
{: #add_workers_dedicated}

使用 {{site.data.keyword.Bluemix_dedicated_notm}}，您只能建立[單一區域叢集](/docs/containers?topic=containers-ha_clusters#single_zone)。依預設，單一區域叢集已設定名稱為 `default` 的工作者節點儲存區。工作者節點儲存區會將具有您在建立叢集期間所定義之相同配置（例如機型）的工作者節點分組在一起。您可以藉由[調整現有工作者節點儲存區大小](/docs/containers?topic=containers-add_workers#resize_pool)或[新增工作者節點儲存區](/docs/containers?topic=containers-add_workers#add_pool)，來將更多工作者節點新增至叢集裡。當您新增工作者節點儲存區時，必須將可用區域新增至工作者節點儲存區，讓工作者節點可以部署至該區域。不過，您無法將其他區域新增至工作者節點儲存區。
{: shortdesc}

### 使用專用及公用映像檔登錄
{: #dedicated_images}

進一步瞭解使用容器映像檔時如何[保護個人資訊安全](/docs/containers?topic=containers-security#pi)。

若為新的名稱空間，請參閱[搭配使用專用及公用映像檔登錄與 {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-images) 中的選項。若為已針對單一及可擴充群組所設定的名稱空間，請[使用記號及建立 Kubernetes 密碼](#cs_dedicated_tokens)來進行鑑別。

### 將子網路新增至叢集
{: #dedicated_cluster_subnet}

將子網路新增至叢集，以變更可用的可攜式公用 IP 位址的儲存區。如需相關資訊，請參閱[將子網路新增至叢集](/docs/containers?topic=containers-subnets#subnets)。請檢閱下列將子網路新增至「專用」叢集的差異。
{: shortdesc}

#### 將其他使用者管理的子網路及 IP 位址新增至 Kubernetes 叢集
{: #dedicated_byoip_subnets}

從您要用來存取 {{site.data.keyword.containerlong_notm}} 的內部部署網路，提供更多自己的子網路。您可以將專用 IP 位址從這些子網路新增至 Kubernetes 叢集裡的 Ingress 及負載平衡器服務。根據您要使用的子網路格式，使用兩種方式中的其中一種來配置使用者管理的子網路。
{: shortdesc}

需求：
- 使用者管理的子網路只能新增至專用 VLAN。
- 子網路字首長度限制為 /24 到 /30。例如，`203.0.113.0/24` 指定 253 個可用的專用 IP 位址，而 `203.0.113.0/30` 指定 1 個可用的專用 IP 位址。
- 子網路中的第一個 IP 位址必須用來作為子網路的閘道。

開始之前：請配置進入您企業網路的網路資料流量，以及離開您的企業網路到將利用使用者管理子網路之 {{site.data.keyword.Bluemix_dedicated_notm}} 網路的網路資料流量遞送。

1. 若要使用您自己的子網路，請[開立支援案例](/docs/get-support?topic=get-support-getting-customer-support)，並提供您要使用的子網路 CIDR 清單。**附註**：針對內部部署及內部帳戶連線功能管理 ALB 及負載平衡器的方式會不同（視子網路 CIDR 的格式而定）。請參閱最終步驟，以瞭解配置差異。

2. 在 {{site.data.keyword.IBM_notm}} 佈建使用者管理的子網路之後，讓子網路可供 Kubernetes 叢集使用。

    ```
    ibmcloud ks cluster-user-subnet-add --cluster <cluster_name> --subnet-cidr <subnet_CIDR> --private-vlan <private_VLAN>
    ```
    {: pre}
    將 <em>&lt;cluster_name&gt;</em> 取代為叢集的名稱或 ID、將 <em>&lt;subnet_CIDR&gt;</em> 取代為您在支援案例中所提供的其中一個子網路 CIDR，並將 <em>&lt;private_VLAN&gt;</em> 取代為可用的專用 VLAN ID。您可以藉由執行 `ibmcloud ks vlans`，來尋找可用的專用 VLAN ID。

3. 驗證子網路已新增至叢集。使用者所提供子網路的 **User-managed** 欄位為 _`true`_。

    ```
    ibmcloud ks cluster-get --showResources <cluster_name>
    ```
    {: pre}

    ```
    Subnet VLANs
    VLAN ID   Subnet CIDR         Public       User-managed
    1555503   169.xx.xxx.xxx/24   true         false
    1555505   10.xxx.xx.xxx/24    false        false
    1555505   10.xxx.xx.xxx/24    false        true
    ```
    {: screen}

4. **重要事項**：如果您的叢集具有多個 VLAN、同一個 VLAN 上有多個子網路，或者是您具有多區域叢集，則必須為您的 IBM Cloud 基礎架構 (SoftLayer) 帳戶啟用[虛擬路由器功能 (VRF)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud)，讓工作者節點可以在專用網路上彼此通訊。若要啟用 VRF，[請與 IBM Cloud 基礎架構 (SoftLayer) 客戶代表聯絡](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#how-you-can-initiate-the-conversion)。如果您無法或不想要啟用 VRF，請啟用 [VLAN Spanning](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning)。若要執行此動作，您需要**網路 > 管理網路 VLAN Spanning** [基礎架構許可權](/docs/containers?topic=containers-users#infra_access)，或者您可以要求帳戶擁有者啟用它。若要確認是否已啟用 VLAN Spanning，請使用 `ibmcloud ks vlan-spanning-get --region <region>` [指令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get)。

5. 若要配置內部部署及內部帳戶連線功能，請從下列選項中進行選擇：
  - 如果您已使用子網路的 10.x.x.x 專用 IP 位址範圍，請使用該範圍中的有效 IP，以使用 Ingress 及負載平衡器來配置內部部署及內部帳戶連線功能。如需相關資訊，請參閱[規劃搭配 NodePort、負載平衡器或 Ingress 服務的網路](/docs/containers?topic=containers-cs_network_planning#external)。
  - 如果您未使用子網路的 10.x.x.x 專用 IP 位址範圍，請使用該範圍中的有效 IP，以使用 Ingress 及負載平衡器來配置內部部署連線功能。如需相關資訊，請參閱[規劃搭配 NodePort、負載平衡器或 Ingress 服務的網路](/docs/containers?topic=containers-cs_network_planning#external)。不過，您必須使用 IBM Cloud 基礎架構 (SoftLayer) 可攜式專用子網路，來配置叢集與其他 Cloud Foundry 型服務之間的內部帳戶連線功能。您可以使用 [`ibmcloud ks cluster-subnet-add`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_subnet_add) 指令來建立可攜式專用子網路。在此情境中，您的叢集同時具有用於內部部署連線功能的使用者管理子網路，以及用於內部帳戶連線功能的 IBM Cloud 基礎架構 (SoftLayer) 可攜式專用子網路。

### 其他叢集配置
{: #dedicated_other}

請檢閱下列用於其他叢集配置的選項：
  * [管理叢集存取](/docs/containers?topic=containers-users#access_policies)
  * [更新 Kubernetes 主節點](/docs/containers?topic=containers-update#master)
  * [更新工作者節點](/docs/containers?topic=containers-update#worker_node)
  * [配置叢集記載](/docs/containers?topic=containers-health#logging)。「專用」端點不支援「日誌」啟用。您必須登入公用 {{site.data.keyword.cloud_notm}} 端點，並將您的公用組織及空間設為目標，才能啟用日誌轉遞。
  * [配置叢集監視](/docs/containers?topic=containers-health#view_metrics)。每一個 {{site.data.keyword.Bluemix_dedicated_notm}} 帳戶內都會有 `ibm-monitoring` 叢集。此叢集會持續監視「專用」環境中的 {{site.data.keyword.containerlong_notm}} 性能，並檢查環境的穩定性及連線功能。請不要從環境移除此叢集。
  * [移除叢集](/docs/containers?topic=containers-remove)

<br />


## 在叢集裡部署應用程式
{: #dedicated_apps}

您可以使用 Kubernetes 技術，在 {{site.data.keyword.Bluemix_dedicated_notm}} 叢集裡部署應用程式，並確保應用程式隨時都已啟動並在執行中。
{:shortdesc}

若要在叢集裡部署應用程式，您可以遵循[在 {{site.data.keyword.Bluemix_notm}} 公用叢集裡部署應用程式](/docs/containers?topic=containers-app#app)的指示。請檢閱下列 {{site.data.keyword.Bluemix_dedicated_notm}} 叢集的差異。

進一步瞭解使用 Kubernetes 資源時如何[保護個人資訊安全](/docs/containers?topic=containers-security#pi)。

### 容許對應用程式的公用存取
{: #dedicated_apps_public}

對於 {{site.data.keyword.Bluemix_dedicated_notm}} 環境，防火牆會封鎖公用主要 IP 位址。若要讓應用程式可公開使用，請使用[負載平衡器服務](#dedicated_apps_public_load_balancer)或 [Ingress](#dedicated_apps_public_ingress)，而非 NodePort 服務。如果您需要存取可攜式公用 IP 位址的負載平衡器服務或 Ingress，請在服務上線時提供企業防火牆白名單給 IBM。
{: shortdesc}

#### 使用負載平衡器服務類型來配置應用程式的存取
{: #dedicated_apps_public_load_balancer}

如果您要將公用 IP 位址用於負載平衡器，請確定已向 IBM 提供企業防火牆白名單，或[開立支援案例](/docs/get-support?topic=get-support-getting-customer-support)來配置防火牆白名單。然後，遵循[使用網路負載平衡器 (NLB) 的基本及 DSR 負載平衡](/docs/containers?topic=containers-loadbalancer)中的步驟進行。
{: shortdesc}

#### 使用 Ingress 來配置應用程式的公用存取
{: #dedicated_apps_public_ingress}

如果您要將公用 IP 位址用於 Ingress ALB，請確定已向 IBM 提供企業防火牆白名單，或[開立支援案例](/docs/get-support?topic=get-support-getting-customer-support)來配置防火牆白名單。然後，遵循[將應用程式公開給大眾使用](/docs/containers?topic=containers-ingress#ingress_expose_public)中的步驟。
{: shortdesc}

### 建立持續性儲存空間
{: #dedicated_apps_volume_claim}

若要檢閱用於建立持續性儲存空間的選項，請參閱[實現高可用性的持續性資料儲存空間選項](/docs/containers?topic=containers-storage_planning#persistent_storage_overview)。若要求備份磁區、從磁區還原、刪除磁區或產生檔案儲存空間的定期 Snapshot，您必須[開立支援案例](/docs/get-support?topic=get-support-getting-customer-support)。
{: shortdesc}

如果您選擇佈建[檔案儲存空間](/docs/containers?topic=containers-file_storage#file_predefined_storageclass)，請選擇不保留儲存類別。選擇不保留儲存類別有助於防止 IBM Cloud 基礎架構 (SoftLayer) 中產生孤立的持續性儲存空間實例，您只能藉由開立支援案例來移除它們。

## 建立 {{site.data.keyword.Bluemix_dedicated_notm}} 映像檔登錄的 {{site.data.keyword.registryshort_notm}} 記號
{: #cs_dedicated_tokens}

針對您用於單一及可擴充群組並與 {{site.data.keyword.containerlong}} 中的叢集搭配使用的映像檔登錄，建立不會過期的記號。
{:shortdesc}

1.  要求現行階段作業的永久性登錄記號。此記號會授與對現行名稱空間中映像檔的存取權。
    ```
    ibmcloud cr token-add --description "<description>" --non-expiring -q
    ```
    {: pre}

2.  驗證 Kubernetes 密碼。

    ```
    kubectl describe secrets
    ```
    {: pre}

    您可以利用此密碼來使用 {{site.data.keyword.containerlong}}。

3.  建立用來儲存記號資訊的 Kubernetes 密碼。

    ```
    kubectl --namespace <kubernetes_namespace> create secret docker-registry <secret_name>  --docker-server=<registry_url> --docker-username=token --docker-password=<token_value> --docker-email=<docker_email>
    ```
    {: pre}

    <table>
    <caption>瞭解這個指令的元件</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解這個指令的元件</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace &lt;kubernetes_namespace&gt;</code></td>
    <td>必要。您要使用密碼並在其中部署容器之叢集的 Kubernetes 名稱空間。執行 <code>kubectl get namespaces</code>，以列出叢集裡的所有名稱空間。</td>
    </tr>
    <tr>
    <td><code>&lt;secret_name&gt;</code></td>
    <td>必要。您要用於映像檔取回密碼的名稱。</td>
    </tr>
    <tr>
    <td><code>--docker-server=&lt;registry_url&gt;</code></td>
    <td>必要。已設定名稱空間的映像檔登錄的 URL：<code>registry.&lt;dedicated_domain&gt;</code></li></ul></td>
    </tr>
    <tr>
    <td><code>--docker-username=token</code></td>
    <td>必要。請不要變更此值。</td>
    </tr>
    <tr>
    <td><code>--docker-password=&lt;token_value&gt;</code></td>
    <td>必要。先前所擷取的登錄記號值。</td>
    </tr>
    <tr>
    <td><code>--docker-email=&lt;docker-email&gt;</code></td>
    <td>必要。如果您有 Docker 電子郵件位址，請輸入它。否則，請輸入虛構的電子郵件位址，例如 a@b.c。此電子郵件是建立 Kubernetes 密碼的必要項目，但在建立之後就不再使用。</td>
    </tr>
    </tbody></table>

4.  建立參照映像檔取回密碼的 Pod。

    1.  開啟偏好的文字編輯器，並建立名稱為 `mypod.yaml` 的 Pod 配置 Script。
    2.  定義您要用來存取登錄的 Pod 及映像檔取回密碼。若要使用名稱空間中的專用映像檔：

        ```
        apiVersion: v1
        kind: Pod
        metadata:
          name: <pod_name>
        spec:
          containers:
            - name: <container_name>
              image: registry.<dedicated_domain>/<my_namespace>/<my_image>:<tag>
          imagePullSecrets:
            - name: <secret_name>
        ```
        {: codeblock}

        <table>
        <caption>瞭解 YAML 檔案元件</caption>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解 YAML 檔案元件</th>
        </thead>
        <tbody>
        <tr>
        <td><code>&lt;pod_name&gt;</code></td>
        <td>您要建立的 Pod 的名稱。</td>
        </tr>
        <tr>
        <td><code>&lt;container_name&gt;</code></td>
        <td>您要部署至叢集的容器的名稱。</td>
        </tr>
        <tr>
        <td><code>&lt;my_namespace&gt;</code></td>
        <td>在其中儲存映像檔的名稱空間。若要列出可用的名稱空間，請執行 `ibmcloud cr namespace-list`。</td>
        </tr>
        <td><code>&lt;my_image&gt;</code></td>
        <td>您要使用的映像檔的名稱。若要列出 {{site.data.keyword.Bluemix_notm}} 帳戶中的可用映像檔，請執行 <code>ibmcloud cr image-list</code>。</td>
        </tr>
        <tr>
        <td><code>&lt;tag&gt;</code></td>
        <td>您要使用的映像檔的版本。如果未指定任何標籤，預設會使用標記為<strong>最新</strong>的映像檔。</td>
        </tr>
        <tr>
        <td><code>&lt;secret_name&gt;</code></td>
        <td>您先前建立的映像檔取回密碼的名稱。</td>
        </tr>
        </tbody></table>

    3.  儲存變更。

    4.  在叢集裡建立部署。

          ```
          kubectl apply -f mypod.yaml -n <namespace>
          ```
          {: pre}
