---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: kubernetes, iks, oks, iro, openshift, red hat, red hat openshift, rhos

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

# 指導教學：建立 Red Hat OpenShift on IBM Cloud 叢集（測試版）
{: #openshift_tutorial}

Red Hat OpenShift on IBM Cloud 作為測試版提供，用於測試 OpenShift 叢集。在處於測試版期間，並非 {{site.data.keyword.containerlong}} 的所有特性皆提供使用。此外，在測試版結束並且 Red Hat OpenShift on IBM Cloud 正式發佈後，您建立的任何 OpenShift 測試版叢集都只保留 30 天。
{: preview}

使用 **Red Hat OpenShift on IBM Cloud 測試版**，可以建立 {{site.data.keyword.containerlong_notm}} 叢集，其中包含隨 OpenShift 容器編排平台軟體一起安裝的工作者節點。使用在 Red Hat Enterprise Linux 上執行的 [OpenShift 工具和型錄 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.openshift.com/container-platform/3.11/welcome/index.html) 來部署應用程式時，您將取得叢集基礎架構環境的所有[受管理 {{site.data.keyword.containerlong_notm}} 的優點](/docs/containers?topic=containers-responsibilities_iks)。
{: shortdesc}

OpenShift 工作者節點僅可用於標準叢集。Red Hat OpenShift on IBM Cloud 只支援 OpenShift 3.11 版，這包含 Kubernetes 1.11 版。
{: note}

## 目標
{: #openshift_objectives}

在本指導教學的各課程中，您將建立標準 Red Hat OpenShift on IBM Cloud 叢集，開啟 OpenShift 主控台，存取內建 OpenShift 元件，部署在 OpenShift 專案中使用 {{site.data.keyword.Bluemix_notm}} 服務的應用程式，以及在 OpenShift 路徑上公開應用程式，以便外部使用者可以存取該服務。
{: shortdesc}

此頁面還包含有關 OpenShift 叢集架構、測試版限制以及如何提供意見和取得支援的資訊。

## 所需時間
{: #openshift_time}
45 分鐘

## 適用對象
{: #openshift_audience}

本指導教學適用於希望瞭解如何首次建立 Red Hat OpenShift on IBM Cloud 叢集的叢集管理者。
{: shortdesc}

## 必要條件
{: #openshift_prereqs}

*   確保您具有下列 {{site.data.keyword.Bluemix_notm}} IAM 存取原則。
    *   對 {{site.data.keyword.containerlong_notm}} 的[**管理者**平台角色](/docs/containers?topic=containers-users#platform)
    *   對 {{site.data.keyword.containerlong_notm}} 的[**撰寫者**或**管理員**服務角色](/docs/containers?topic=containers-users#platform)
    *   對 {{site.data.keyword.registrylong_notm}} 的[**管理者**平台角色](/docs/containers?topic=containers-users#platform)
*    確保為 {{site.data.keyword.Bluemix_notm}} 地區和資源群組的 [API 金鑰](/docs/containers?topic=containers-users#api_key)設定了正確的基礎架構許可權、**超級使用者**或建立叢集的[最低角色](/docs/containers?topic=containers-access_reference#infra)。
*   安裝指令行工具。
    *   [安裝 {{site.data.keyword.Bluemix_notm}} CLI (`ibmcloud`)、{{site.data.keyword.containershort_notm}} 外掛程式 (`ibmcloud ks`) 和 {{site.data.keyword.registryshort_notm}} 外掛程式 (`ibmcloud cr`)](/docs/containers?topic=containers-cs_cli_install#cs_cli_install_steps)。
    *   [安裝 OpenShift Origin (`oc`) 和 Kubernetes (`kubectl`) CLI](/docs/containers?topic=containers-cs_cli_install#cli_oc)。

<br />


## 架構概觀
{: #openshift_architecture}

下圖和表格說明了在 Red Hat OpenShift on IBM Cloud 架構中設定的預設元件。
{: shortdesc}

![Red Hat OpenShift on IBM Cloud 叢集架構](images/cs_org_ov_both_ses_rhos.png)

|主節點元件|說明|
|:-----------------|:-----------------|
|抄本|主節點元件（包括 OpenShift Kubernetes API 伺服器和 etcd 資料儲存庫）有三個抄本，如果位於多區域都會中，則這三個抄本會跨區域分佈，以實現更高可用性。主節點元件每 8 小時備份一次。|
|`rhos-api`|OpenShift Kubernetes API 伺服器作為從工作者節點到主節點的所有叢集管理要求的主進入點。該 API 伺服器會驗證並處理會變更 Kubernetes 資源（例如，pod 或服務）的狀態的要求，並將此狀態儲存在 etcd 資料儲存庫中。|
|`openvpn-server`|OpenVPN 伺服器會使用 OpenVPN 用戶端，將主節點安全地連接至工作者節點。此連線支援對 Pod 及服務的 `apiserver proxy` 呼叫，以及對 kubelet 的 `kubectl exec`、`attach` 及 `logs` 呼叫。|
|`etcd`|etcd 是高度可用的鍵值儲存庫，其中儲存叢集的所有 Kubernetes 資源（例如服務、部署及 Pod）的狀況。etcd 中的資料會備份至 IBM 管理的已加密儲存空間實例。|
|`rhos-controller`|OpenShift 控制器管理程式監視新建立的 Pod，並根據容量、效能需求、原則限制、反親緣關係規格和工作負載需求來決定這些 Pod 的部署位置。如果找不到相符需求的工作者節點，則不會在叢集裡部署 Pod。控制器還會監視叢集資源（例如，抄本集）的狀態。當資源的狀況變更時，例如，若抄本集中的 Pod 關閉，則控制器管理程式會起始更正動作以達到所需的狀況。`rhos-controller` 在原生 Kubernetes 配置中作為排程器和控制器管理程式。|
|`cloud-controller-manager`|雲端控制器管理程式管理雲端提供者特定的元件，例如 {{site.data.keyword.Bluemix_notm}} 負載平衡器。|
{: caption="表格 1. OpenShift 主節點元件。" caption-side="top"}
{: #rhos-components-1}
{: tab-title="Master"}
{: tab-group="rhos-components"}
{: class="simple-tab-table"}

|工作者節點元件|說明|
|:-----------------|:-----------------|
|作業系統|Red Hat OpenShift on IBM Cloud 工作者節點在 Red Hat Enterprise Linux 7 (RHEL 7) 作業系統上執行。|
|專案|OpenShift 會將資源組織成專案（專案是具有註釋的 Kubernetes 名稱空間），並且包含的元件比原生 Kubernetes 叢集多得多，以用於執行 OpenShift 特性（如型錄）。選取的專案元件將在下列各行中說明。如需相關資訊，請參閱[專案和使用者 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.openshift.com/container-platform/3.11/architecture/core_concepts/projects_and_users.html)。|
| `kube-system` |此名稱空間包含用於在工作者節點上執行 Kubernetes 的許多元件。<ul><li>**`ibm-master-proxy`**：`ibm-master-proxy` 是一個常駐程式集，用於將要求從工作者節點轉遞到高可用性主節點抄本的 IP 位址。在單一區域叢集裡，主節點有三個抄本，每個抄本位於個別的主機上。對於具有多區域功能之區域中的叢集，主節點具有三個分散在各區域之中的抄本。高可用性負載平衡器會將向主節點網域名稱發出的要求轉遞到主節點抄本。</li><li>**`openvpn-client`**：OpenVPN 用戶端與 OpenVPN 伺服器配合使用，以安全地將主節點連接至工作者節點。此連線支援對 Pod 及服務的 `apiserver proxy` 呼叫，以及對 kubelet 的 `kubectl exec`、`attach` 及 `logs` 呼叫。</li><li>**`kubelet`**：kubelet 是一個工作者節點代理程式，在每個工作者節點上執行，負責監視在工作者節點上執行的 Pod 的性能，以及監視 Kubernetes API 伺服器傳送的事件。根據這些事件，kubelet 會建立或移除 Pod、確保存活性及就緒探測，以及向 Kubernetes API 伺服器回報 Pod 的狀態。</li><li>**`calico`**：Calico 管理叢集的網路原則，並包含多個元件，用於管理容器網路連線功能、IP 位址指派和網路資料流量控制。</li><li>**其他元件**：`kube-system` 名稱空間還包含多個元件，用於管理 IBM 提供的資源，例如檔案及區塊儲存空間的儲存空間外掛程式、Ingress 應用程式負載平衡器 (ALB)、`fluentd` 記載和 `keepalived`。</li></ul>|
| `ibm-system` |此名稱空間包含 `ibm-cloud-provider-ip` 部署，此部署使用 `keepalived` 為向應用程式 Pod 發出的要求提供性能檢查和第 4 層負載平衡。|
|`kube-proxy-and-dns`|此名稱空間包含多個元件，用於根據在工作者節點上設定的 `iptables` 規則來驗證送入網路資料流量，以及作為 Proxy 傳遞容許進入或離開叢集的要求。|
|`default`|如果未指定名稱空間或為 Kubernetes 資源建立專案，將使用此名稱空間。此外，default 名稱空間還包含下列元件，用於支援 OpenShift 叢集。<ul><li>**`router`**：OpenShift 使用[路徑 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.openshift.com/container-platform/3.11/architecture/networking/routes.html) 在主機名稱上公開應用程式的服務，以便外部用戶端可以使用該服務。router 會將服務對映到主機名稱。</li><li>**`docker-registry`** 和 **`registry-console`**：OpenShift 提供了內部[容器映像檔登錄 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.openshift.com/container-platform/3.11/install_config/registry/index.html)，可用於透過主控台在本端管理和檢視映像檔。或者，也可以設定專用 {{site.data.keyword.registrylong_notm}}。</li></ul>|
|其他專案|依預設，其他元件安裝在各種名稱空間中，以啟用記載、監視和 OpenShift 主控台等功能。<ul><li><code>ibm-cert-store</code></li><li><code>kube-public</code></li><li><code>kube-service-catalog</code></li><li><code>openshift</code></li><li><code>openshift-ansible-service-broker</code></li><li><code>openshift-console</code></li><li><code>openshift-infra</code></li><li><code>openshift-monitoring</code></li><li><code>openshift-node</code></li><li><code>openshift-template-service-broker</code></li><li><code>openshift-web-console</code></li></ul>|
{: caption="表格 2. OpenShift 工作者節點元件。" caption-side="top"}
{: #rhos-components-2}
{: tab-title="Worker nodes"}
{: tab-group="rhos-components"}
{: class="simple-tab-table"}

<br />


## 課程 1：建立 Red Hat OpenShift on IBM Cloud 叢集
{: #openshift_create_cluster}

可以使用[主控台](#openshift_create_cluster_console)或 [CLI](#openshift_create_cluster_cli) 在 {{site.data.keyword.containerlong_notm}} 中建立 Red Hat OpenShift on IBM Cloud 叢集。若要瞭解在建立叢集時設定哪些元件，請參閱[架構概觀](#openshift_architecture)。OpenShift 僅可用於標準叢集。您可以在[常見問題集](/docs/containers?topic=containers-faqs#charges)中瞭解有關標準叢集價格的更多資訊。
{:shortdesc}

只能在 **default** 資源群組中建立叢集。在測試版結束並且 Red Hat OpenShift on IBM Cloud 正式發佈後，您在測試版階段期間建立的任何 OpenShift 叢集都只保留 30 天。
{: important}

### 使用主控台建立叢集
{: #openshift_create_cluster_console}

在 {{site.data.keyword.containerlong_notm}} 主控台中建立標準 OpenShift 叢集。
{: shortdesc}

開始之前，請[完成必要條件](#openshift_prereqs)，以確保您具有建立叢集的適當許可權。

1.  建立叢集。
    1.  登入到 [{{site.data.keyword.Bluemix_notm}} 帳戶 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/)。
    2.  從漢堡功能表 ![「漢堡功能表」圖示](../icons/icon_hamburger.svg "「漢堡功能表」圖示") 中，選取 **Kubernetes**，然後按一下**建立叢集**。
    3.  選擇叢集設定詳細資料和名稱。對於測試版，OpenShift 叢集僅作為位於華盛頓特區和倫敦資料中心的標準叢集提供使用。
        *   對於**選取方案**，選擇**標準**。
        *   對於**資源群組**，必須使用 **default**。
        *   對於**位置**，將地理位置設定為**北美**或**歐洲**，選取可用的**單一區域**或**多個區域**，然後選取**華盛頓特區**或**倫敦**工作者節點區域。
        *   對於**預設工作者節點儲存區**，選取 **OpenShift** 叢集版本。Red Hat OpenShift on IBM Cloud 只支援 OpenShift 3.11 版，這包含 Kubernetes 1.11 版。
為工作者節點選擇可用的特性，理想情況下工作者節點至少有 4 個核心 16 GB RAM。
        *   設定每個區域要建立的工作者節點數，例如 3。
    4.  最後，按一下**建立叢集**。<p class="note">叢集建立可能需要一點時間才能完成。叢集狀況顯示 **Normal** 之後，叢集網路及負載平衡元件需要大約再多 10 分鐘才能部署及更新您用於 OpenShift Web 主控台和其他路徑叢集網域。等待叢集就緒後再繼續執行下一步，確定就緒的方法是檢查 **Ingress Subdomain** 是否遵循 `<cluster_name>.<region>.containers.appdomain.cloud` 型樣。</p>
2.  在叢集詳細資料頁面中，按一下 **OpenShift Web 主控台**。
3.  在 OpenShift 容器平台功能表列的下拉功能表中，按一下**應用程式主控台**。應用程式主控台將列出叢集裡的所有專案名稱空間。您可以導覽至某個名稱空間來檢視應用程式、建置和其他 Kubernetes 資源。
4.  若要透過在終端機中工作來完成下一課，請按一下設定檔 **IAM#user.name@email.com > 複製登入指令**。將複製的 `oc` login 指令貼到終端機，以便透過 CLI 進行鑑別。

### 使用 CLI 建立叢集
{: #openshift_create_cluster_cli}

使用 {{site.data.keyword.Bluemix_notm}} CLI 建立標準 OpenShift 叢集。
{: shortdesc}

開始之前，請[完成必要條件](#openshift_prereqs)，以確保您具有建立叢集的適當許可權，並且具有 `ibmcloud` CLI 和外掛程式以及 `oc` 和 `kubectl` CLI。

1.  登入到為建立 OpenShift 叢集而設定的帳戶。將 **us-east** 或 **eu-gb** 地區以及 **default** 資源群組設定為目標。如果您有聯合帳戶，請包括 `--sso` 旗標。
    ```
    ibmcloud login -r (us-east|eu-gb) -g default [--sso]
    ```
    {: pre}
2.  建立叢集。
    ```
    ibmcloud ks cluster-create --name <name> --location <zone> --kube-version <openshift_version> --machine-type <worker_node_flavor> --workers <number_of_worker_nodes_per_zone> --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID>
    ```
    {: pre}

    用於建立具有三個工作者節點的叢集的範例指令，這些節點位於華盛頓特區，具有四個核心和 16 GB 記憶體。

    ```
    ibmcloud ks cluster-create --name my_openshift --location wdc04 --kube-version 3.11_openshift --machine-type b3c.4x16.encrypted  --workers 3 --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID>
    ```
    {: pre}

    <table summary="表格中第一行佔兩直欄。其餘行都應從左到右閱讀，其中第一直欄是指令組成部分，第二直欄是相應的說明。">
    <caption>cluster-create 元件</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解這個指令的元件</th>
    </thead>
    <tbody>
    <tr>
    <td><code>cluster-create</code></td>
    <td>此指令在 {{site.data.keyword.Bluemix_notm}} 帳戶中建立標準基礎架構叢集。</td>
    </tr>
    <tr>
    <td><code>--name <em>&lt;name&gt;</em></code></td>
    <td>輸入叢集的名稱。此名稱必須以字母開頭，可以包含字母、數字及連字號 (-)，且長度不得超過 35 個字元。請使用在各 {{site.data.keyword.Bluemix_notm}} 地區中唯一的名稱。</td>
    </tr>
    <tr>
    <td><code>--location <em>&lt;zone&gt;</em></code></td>
    <td>指定要在其中建立叢集的區域。對於測試版，可用區域為 `wdc04、wdc06、wdc07、lon04、lon05` 或 `lon06`。</p></td>
    </tr>
    <tr>
      <td><code>--kube-version <em>&lt;openshift_version&gt;</em></code></td>
      <td>必須選擇支援的 OpenShift 版本。OpenShift 版本包含 Kubernetes 版本，此版本不同於原生 Kubernetes Ubuntu 叢集上提供的 Kubernetes 版本。若要列出可用的 OpenShift 版本，請執行 `ibmcloud ks versions`。若要建立具有最新修補程式版本的叢集，可以僅指定主版本和次要版本，例如 `3.11_openshift`。<br><br>Red Hat OpenShift on IBM Cloud 只支援 OpenShift 3.11 版，這包含 Kubernetes 1.11 版。
</td>
    </tr>
    <tr>
    <td><code>--machine-type <em>&lt;worker_node_flavor&gt;</em></code></td>
    <td>選擇機型。您可以將工作者節點部署為共用或專用硬體上的虛擬機器，或部署為裸機上的實體機器。可用的實體及虛擬機器類型會因您部署叢集所在的區域而不同。若要列出可用的機器類型，請執行 `ibmcloud ks machine-types --zone <zone>`。</td>
    </tr>
    <tr>
    <td><code>--workers <em>&lt;number_of_worker_nodes_per_zone&gt;</em></code></td>
    <td>要包含在叢集裡的工作者節點數。您可能需要至少指定三個工作者節點，以便叢集具有足夠的資源來執行預設元件並實現高可用性。如果未指定 <code>--workers</code> 選項，則會建立一個工作者節點。</td>
    </tr>
    <tr>
    <td><code>--public-vlan <em>&lt;public_vlan_id&gt;</em></code></td>
    <td>如果已經在 IBM Cloud 基礎架構 (SoftLayer) 帳戶中為該區域設定了公用 VLAN，請輸入該公用 VLAN 的 ID。若要檢查可用的 VLAN，請執行 `ibmcloud ks vlans --zone <zone>`。<br><br>如果帳戶中沒有公用 VLAN，請不要指定此選項。{{site.data.keyword.containerlong_notm}} 會為您自動建立公用 VLAN。</td>
    </tr>
    <tr>
    <td><code>--private-vlan <em>&lt;private_vlan_id&gt;</em></code></td>
    <td>如果已經在 IBM Cloud 基礎架構 (SoftLayer) 帳戶中為該區域設定了專用 VLAN，請輸入該專用 VLAN 的 ID。若要檢查可用的 VLAN，請執行 `ibmcloud ks vlans --zone <zone>`。<br><br>如果您的帳戶中沒有專用 VLAN，請不要指定此選項。{{site.data.keyword.containerlong_notm}} 會自動為您建立一個專用 VLAN。</td>
    </tr>
    </tbody></table>
3.  列出叢集詳細資料。檢閱叢集 **State**、檢查 **Ingress Subdomain**，並記下 **Master URL**。<p class="note">叢集建立可能需要一點時間才能完成。叢集狀況顯示 **Normal** 之後，叢集網路及負載平衡元件需要大約再多 10 分鐘才能部署及更新您用於 OpenShift Web 主控台和其他路徑叢集網域。等待叢集就緒後再繼續執行下一步，確定就緒的方法是檢查 **Ingress Subdomain** 是否遵循 `<cluster_name>.<region>.containers.appdomain.cloud` 型樣。</p>
    ```
    ibmcloud ks cluster-get --cluster <cluster_name_or_ID>
    ```
    {: pre}
4.  下載配置檔以連接至叢集。
    ```
    ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
    ```
    {: pre}

    配置檔下載完成之後，會顯示一個指令，可讓您複製並貼上，將本端 Kubernetes 配置檔的路徑設定為環境變數。

    OS X 的範例：

    ```
    export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-<datacenter>-<cluster_name>.yml
    ```
    {: screen}
5.  在瀏覽器中，導覽至 **Master URL** 的位址並附加 `/console`。例如 `https://c0.containers.cloud.ibm.com:23652/console`。
6.  按一下設定檔 **IAM#user.name@email.com > 複製登入指令**。將複製的 `oc` login 指令貼到終端機，以便透過 CLI 進行鑑別。<p class="tip">儲存叢集主節點 URL，以便日後存取 OpenShift 主控台。在未來的階段作業中，可以跳過 `cluster-config` 步驟，而改為使用主控台複製登入指令。</p>
7.  透過檢查版本，驗證 `oc` 指令是否針對您的叢集正常執行。

    ```
    oc version
    ```
    {: pre}

    輸出範例：

    ```
    oc v3.11.0+0cbc58b
    kubernetes v1.11.0+d4cacc0
    features: Basic-Auth

    Server https://c0.containers.cloud.ibm.com:23652
    openshift v3.11.98
    kubernetes v1.11.0+d4cacc0
    ```
    {: screen}

    如果無法執行需要管理者許可權的作業（例如，列出叢集裡的所有工作者節點或 Pod），請透過執行 `ibmcloud ks cluster-config --cluster <cluster_name_or_ID> --admin` 指令來下載叢集管理者的 TLS 憑證和許可權檔案。
    {: tip}

<br />


## 課程 2：存取內建 OpenShift 服務
{: #openshift_access_oc_services}

Red Hat OpenShift on IBM Cloud 隨附可使用的內建服務（例如 OpenShift 主控台、Prometheus 和 Grafana），以協助操作叢集。對於測試版，要存取這些服務，可以使用[路徑 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.openshift.com/container-platform/3.11/architecture/networking/routes.html) 的本端主機。預設路徑網域名稱遵循叢集特有的型樣：`<service_name>-<namespace>.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud`。
{:shortdesc}

可以透過[主控台](#openshift_services_console)或 [CLI](#openshift_services_cli) 來存取內建 OpenShift 服務路徑。您可能希望使用主控台透過專案中的 Kubernetes 資源進行導覽。透過使用 CLI，可以列出資源，例如跨專案的路徑。

### 透過主控台存取內建 OpenShift 服務
{: #openshift_services_console}
1.  在 OpenShift Web 主控台的 OpenShift 容器平台功能表列的下拉功能表中，按一下**應用程式主控台**。
2.  選取 **default** 專案，然後在導覽窗格中，按一下**應用程式 > Pod**。
3.  驗證**路由器** Pod 是否處於**執行中**狀態。路由器會作為外部網路資料流量的進入點。您可以使用路由器，利用路徑將叢集裡的服務公開地在路由器的外部 IP 位址上公開。路由器會接聽公用主機網路介面，而不像應用程式 Pod 是只接聽專用 IP。路由器會將路徑主機名稱的外部要求代理到由服務所識別、您已和路徑主機名稱建立關聯的應用程式 Pod IP。
4.  在 **default** 專案導覽窗格中，按一下**應用程式 > 部署**，然後按一下 **registry-console** 部署。若要開啟內部登錄主控台，必須更新提供者 URL，以便可以在外部對其進行存取。
    1.  在 **registry-console** 詳細資料頁面的**環境**標籤中，尋找 **OPENSHIFT_OAUTH_PROVIDER_URL** 欄位。 
    2. 在值欄位中，在 `c1` 後新增 `-e`，例如 `https://c1-e.eu-gb.containers.cloud.ibm.com:20399`。 
    3. 按一下**儲存**。現在，可以透過叢集主節點的公用 API 端點來存取登錄主控台部署。
    4.  在 **default** 專案導覽窗格中，按一下**應用程式 > 路徑**。若要開啟登錄主控台，請按一下**主機名稱**值，例如 `https://registry-console-default.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud`。
<p class="note">對於測試版，登錄主控台使用自簽 TLS 憑證，因此必須選擇繼續存取登錄主控台。在 Google Chrome 中，按一下**進階 > 繼續到 <cluster_master_URL>**。其他瀏覽器具有類似的選項。如果無法使用此設定繼續，請嘗試在專用瀏覽器中開啟該 URL。</p>
5.  在 OpenShift 容器平台功能表列的下拉功能表中，按一下**叢集主控台**。
6.  在導覽窗格中，展開**監視**。
7.  按一下要存取的內建監視工具，例如**儀表板**。這將開啟 Grafana 路徑 `https://grafana-openshift-monitoring.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud`。
<p class="note">您第一次存取主機名稱時，可能需要進行鑑別，例如按一下**使用 OpenShift 登入**並授與存取權給您的 IAM 身分。</p>

### 透過 CLI 存取內建 OpenShift 服務
{: #openshift_services_cli}

1.  在 OpenShift Web 主控台中，按一下設定檔 **IAM#user.name@email.com > 複製登入指令**，然後將登入指令貼到終端機以進行鑑別。
    ```
    oc login https://c1-e.<region>.containers.cloud.ibm.com:<port> --token=<access_token>
    ```
    {: pre}
2.  驗證路由器是否已部署。路由器會作為外部網路資料流量的進入點。您可以使用路由器，利用路徑將叢集裡的服務公開地在路由器的外部 IP 位址上公開。路由器會接聽公用主機網路介面，而不像應用程式 Pod 是只接聽專用 IP。路由器會將路徑主機名稱的外部要求代理到由服務所識別、您已和路徑主機名稱建立關聯的應用程式 Pod IP。
    ```
    oc get svc router -n default
    ```
    {: pre}

    輸出範例：
    ```
    NAME      TYPE           CLUSTER-IP               EXTERNAL-IP     PORT(S)                      AGE
    router    LoadBalancer   172.21.xxx.xxx   169.xx.xxx.xxx   80:30399/TCP,443:32651/TCP                      5h
    ```
    {: screen}
2.  取得要存取服務路徑的 **Host/Port** 主機名稱。例如，您可能希望存取 Grafana 儀表板，以查看叢集資源使用情況的度量。預設路徑網域名稱遵循叢集特有的型樣：`<service_name>-<namespace>.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud`。
```
    oc get route --all-namespaces
    ```
    {: pre}

    輸出範例：
    ```
    NAMESPACE                          NAME                HOST/PORT                                                                    PATH                  SERVICES            PORT               TERMINATION          WILDCARD
    default                            registry-console    registry-console-default.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud                              registry-console    registry-console   passthrough          None
    kube-service-catalog               apiserver           apiserver-kube-service-catalog.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud                        apiserver           secure             passthrough          None
    openshift-ansible-service-broker   asb-1338            asb-1338-openshift-ansible-service-broker.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud            asb                 1338               reencrypt            None
    openshift-console                  console             console.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud                                              console             https              reencrypt/Redirect   None
    openshift-monitoring               alertmanager-main   alertmanager-main-openshift-monitoring.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud                alertmanager-main   web                reencrypt            None
    openshift-monitoring               grafana             grafana-openshift-monitoring.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud                          grafana             https              reencrypt            None
    openshift-monitoring               prometheus-k8s      prometheus-k8s-openshift-monitoring.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud                   prometheus-k8s      web                reencrypt
    ```
    {: screen}
3.  **登錄一次性更新**：若要使內部登錄主控台可從網際網路存取，請編輯 `registry-console` 部署以將叢集主節點的公用 API 端點用作 OpenShift 提供者 URL。公用 API 端點的格式與專用 API 端點的格式相同，但在 URL 中會包含額外的 `-e`。
    ```
    oc edit deploy registry-console -n default
    ```
    {: pre}
    
    在 `Pod Template.Containers.registry-console.Environment.OPENSHIFT_OAUTH_PROVIDER_URL` 欄位中，在 `c1` 後新增 `-e`，例如 `https://ce.eu-gb.containers.cloud.ibm.com:20399`。
    ```
    Name:                   registry-console
    Namespace:              default
    ...
    Pod Template:
      Labels:  name=registry-console
      Containers:
       registry-console:
        Image:      registry.eu-gb.bluemix.net/armada-master/iksorigin-registrconsole:v3.11.98-6
        ...
        Environment:
          OPENSHIFT_OAUTH_PROVIDER_URL:  https://c1-e.eu-gb.containers.cloud.ibm.com:20399
          ...
    ```
    {: screen}
4.  在 Web 瀏覽器中，開啟要存取的路徑，例如：`https://grafana-openshift-monitoring.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud`。
您第一次存取主機名稱時，可能需要進行鑑別，例如按一下**使用 OpenShift 登入**並授與存取權給您的 IAM 身分。

<br>
現在，您已位於內建 OpenShift 應用程式中！例如，如果您位於 Grafana 中，則可查看名稱空間 CPU 使用率或其他圖形。若要存取其他內建工具，請開啟其路徑主機名稱。

<br />


## 課程 3：將應用程式部署到 OpenShift 叢集
{: #openshift_deploy_app}

透過 Red Hat OpenShift on IBM Cloud，可以建立新的應用程式，並可利用 OpenShift 路由器來公開應用程式服務以供外部使用者使用。
{: shortdesc}

如果在學習了上一課後稍作休息，現在啟動了新的終端機，請確保您已重新登入到叢集。在 `https://<master_URL>/console` 開啟 OpenShift 主控台。例如 `https://c0.containers.cloud.ibm.com:23652/console`。然後按一下您的設定檔 **IAM#user.name@email.com > 複製登入指令**，再將複製的 `oc` login 指令貼到終端機，以便透過 CLI 進行鑑別。
{: tip}

1.  為 Hello World 應用程式建立專案。專案是具有其他註釋的 Kubernetes 名稱空間的 OpenShift 版本。
    ```
    oc new-project hello-world
    ```
    {: pre}
2.  [透過原始碼 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/IBM/container-service-getting-started-wt) 建置範例應用程式。使用 OpenShift `new-app` 指令，可以參照遠端儲存庫中包含 Dockerfile 和應用程式代碼的目錄來建置映像檔。該指令將在本端 Docker 登錄中建置映像檔，並建立應用程式部署配置 (`dc`) 和服務 (`svc`)。如需建立新應用程式的相關資訊，請參閱 [OpenShift 文件 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.openshift.com/container-platform/3.11/dev_guide/application_lifecycle/new_app.html)。
    ```
    oc new-app --name hello-world https://github.com/IBM/container-service-getting-started-wt --context-dir="Lab 1"
    ```
    {: pre}
3.  驗證是否已建立範例 Hello World 應用程式元件。
    1.  透過在瀏覽器中存取登錄主控台，檢查叢集的內建 Docker 登錄中是否有 **hello-world** 映像檔。確保使用 `-e` 來更新登錄主控台提供者 URL，如上一課中所述。
        ```
        https://registry-console-default.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud/
        ```
        {: codeblock}
    2.  列出 **hello-world** 服務並記下服務名稱。除非您為服務建立了路徑，讓路由器可以將外部資料流量要求轉遞到應用程式，否則應用程式會接聽這些內部叢集 IP 位址上的資料流量。
        ```
        oc get svc -n hello-world
        ```
        {: pre}

                輸出範例：
        ```
        NAME          TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)    AGE
        hello-world   ClusterIP   172.21.xxx.xxx   <nones>       8080/TCP   31m
        ```
        {: screen}
    3.  列出 Pod。名稱中具有 `build` 的 Pod 是在新應用程式建置程序中**已完成**的工作。確保 **hello-world** Pod 的狀態為 **Running**。
        ```
        oc get pods -n hello-world
        ```
        {: pre}

        輸出範例：
        ```
        NAME                READY     STATUS             RESTARTS   AGE
        hello-world-9cv7d   1/1       Running            0          30m
        hello-world-build   0/1       Completed          0          31m
        ```
        {: screen}
4.  設定路徑，以便可使用公用方式存取 {{site.data.keyword.toneanalyzershort}} 服務。依預設，主機名稱的格式為 `<service_name>-<namespace>.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud`。
如果要自訂主機名稱，請包含 `--hostname=<hostname>` 旗標。
    1.  為 **hello-world** 服務建立路徑。
        ```
        oc create route edge --service=hello-world -n hello-world
        ```
        {: pre}
    2.  從 **Host/Port** 輸出取得路徑主機名稱位址。
        ```
        oc get route -n hello-world
        ```
        {: pre}
        輸出範例：
        ```
        NAME          HOST/PORT                         PATH                                        SERVICES      PORT       TERMINATION   WILDCARD
        hello-world   hello-world-hello.world.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud    hello-world   8080-tcp   edge/Allow    None
        ```
        {: screen}
5.  存取應用程式。
    ```
    curl https://hello-world-hello-world.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud
    ```
    {: pre}
    
    輸出範例：
    ```
    Hello world from hello-world-9cv7d! Your app is up and running in a cluster!
    ```
    {: screen}
6.  **選用**：若要清除在本課程中建立的資源，可以使用指派給每個應用程式的標籤。
    1.  列出 `hello-world` 項目中每個應用程式的所有資源。
        ```
        oc get all -l app=hello-world -o name -n hello-world
        ```
        {: pre}
        輸出範例：
        ```
        pod/hello-world-1-dh2ff
        replicationcontroller/hello-world-1
        service/hello-world
        deploymentconfig.apps.openshift.io/hello-world
        buildconfig.build.openshift.io/hello-world
        build.build.openshift.io/hello-world-1
        imagestream.image.openshift.io/hello-world
        imagestream.image.openshift.io/node
        route.route.openshift.io/hello-world
        ```
        {: screen}
    2.  刪除建立的所有資源。
        ```
        oc delete all -l app=hello-world -n hello-world
        ```
        {: pre}



<br />


## 課程 4：設定 LogDNA 和 Sysdig 附加程式以監視叢集性能
{: #openshift_logdna_sysdig}

由於依預設 OpenShift 設定的[安全環境定義限制 (SCC) ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.openshift.com/container-platform/3.11/admin_guide/manage_scc.html) 比原生 Kubernetes 更嚴格，因此您可能會發現在原生 Kubernetes 上使用的某些應用程式或叢集附加程式無法以相同方式部署在 OpenShift 上。尤其是，許多映像檔需要以 `root` 使用者身分執行或作為特許容器執行，而在 OpenShift 中依預設會阻止這些行為。在本課程中，您將瞭解如何透過建立特許安全帳戶，並更新 Pod 規格中的 `securityContext` 來修改預設 SCC，以使用兩個常用的 {{site.data.keyword.containerlong_notm}} 附加程式：{{site.data.keyword.la_full_notm}} 和 {{site.data.keyword.mon_full_notm}}。
{: shortdesc}

開始之前，請以管理者身分登入到叢集。
1.  在 `https://<master_URL>/console` 開啟 OpenShift 主控台。例如 `https://c0.containers.cloud.ibm.com:23652/console`。
2.  按一下設定檔 **IAM#user.name@email.com > 複製登入指令**，然後將複製的 `oc` 登入指令貼到終端機以透過 CLI 進行鑑別。
3.  下載叢集的管理配置檔。
    ```
    ibmcloud ks cluster-config --cluster <cluster_name_or_ID> --admin
    ```
    {: pre}
    
    配置檔下載完成之後，會顯示一個指令，可讓您複製並貼上，將本端 Kubernetes 配置檔的路徑設定為環境變數。

    OS X 的範例：

    ```
    export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-<datacenter>-<cluster_name>.yml
    ```
    {: screen}
4.  繼續本課程以設定 [{{site.data.keyword.la_short}}](#openshift_logdna) 和 [{{site.data.keyword.mon_short}}](#openshift_sysdig)。

### 課程 4a：設定 LogDNA
{: #openshift_logdna}

為 {{site.data.keyword.la_full_notm}} 設定專案和特許服務程式帳戶。然後，在 {{site.data.keyword.Bluemix_notm}} 帳戶中建立 {{site.data.keyword.la_short}} 實例。若要將 {{site.data.keyword.la_short}} 實例與 OpenShift 叢集整合，必須修改部署的常駐程式集，以使用特許服務程式帳戶以 root 使用者身分執行。
{: shortdesc}

1.  為 LogDNA 設定專案和特許服務程式帳戶。
    1.  以叢集管理者身分，建立 `logdna` 項目。
        ```
        oc adm new-project logdna
        ```
        {: pre}
    2.  將該專案設定為目標，以便後續建立的資源位於 `logdna` 專案名稱空間中。
        ```
        oc project logdna
        ```
        {: pre}
    3.  為 `logdna` 專案建立服務帳戶。
        ```
        oc create serviceaccount logdna
        ```
        {: pre}
    4.  將特許安全環境定義限制新增到 `logdna` 專案的服務帳戶。<p class="note">如果要檢查 `privileged` SCC 原則授予服務帳戶的專用權，請執行 `oc describe scc privileged`。如需 SCC 的相關資訊，請參閱 [OpenShift 文件 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.openshift.com/container-platform/3.11/admin_guide/manage_scc.html)。</p>
        ```
        oc adm policy add-scc-to-user privileged -n logdna -z logdna
        ```
        {: pre}
2.  在叢集所在的資源群組中建立 {{site.data.keyword.la_full_notm}} 實例。選取定價方案，方案用於確定日誌的保留期，例如 `lite` 將日誌保留 0 天。地區不必符合您叢集的地區。如需相關資訊，請參閱[佈建實例](/docs/services/Log-Analysis-with-LogDNA/tutorials?topic=LogDNA-provision)和[定價方案](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-about#overview_pricing_plans)。
    ```
    ibmcloud resource service-instance-create <service_instance_name> logdna (lite|7-days|14-days|30-days) <region> [-g <resource_group>]
    ```
    {: pre}
    
    指令範例：
    ```
    ibmcloud resource service-instance-create logdna-openshift logdna lite us-south
    ```
    {: pre}
    
    在輸出中，記下服務實例 **ID**，其格式為 `crn:v1:bluemix:public:logdna:<region>:<ID_string>::`。
    ```
    Service instance <name> was created.
                 
    Name:         <name>   
    ID:           crn:v1:bluemix:public:logdna:<region>:<ID_string>::   
    GUID:         <guid>   
    Location:     <region>   
    ...
    ```
    {: screen}    
3.  取得 {{site.data.keyword.la_short}} 實例汲取金鑰。LogDNA 汲取金鑰用於將安全的 Web Socket 開啟到 LogDNA 汲取伺服器，並使用 {{site.data.keyword.la_short}} 服務來鑑別記載代理程式。
    1.  為 LogDNA 實例建立服務金鑰。
        ```
        ibmcloud resource service-key-create <key_name> Administrator --instance-id <logdna_instance_ID>
        ```
        {: pre}
    2.  記下服務金鑰的 **ingestion_key**。
        ```
        ibmcloud resource service-key <key_name>
        ```
        {: pre}
        
        輸出範例：
        ```
        Name:          <key_name>  
        ID:            crn:v1:bluemix:public:logdna:<region>:<ID_string>::    
        Created At:    Thu Jun  6 21:31:25 UTC 2019   
        State:         active   
        Credentials:                                   
                       apikey:                   <api_key_value>      
                       iam_apikey_description:   Auto-generated for key <ID>     
                       iam_apikey_name:          <key_name>       
                       iam_role_crn:             crn:v1:bluemix:public:iam::::role:Administrator      
                       iam_serviceid_crn:        crn:v1:bluemix:public:iam-identity::<ID_string>     
                       ingestion_key:            111a11aa1a1a11a1a11a1111aa1a1a11    
        ```
        {: screen}
4.  建立 Kubernetes 密碼以用於儲存服務實例的 LogDNA 汲取金鑰。
    ```
     oc create secret generic logdna-agent-key --from-literal=logdna-agent-key=<logDNA_ingestion_key>
    ```
    {: pre}
5.  建立 Kubernetes 常駐程式集，以用於在 Kubernetes 叢集的每個工作者節點上部署 LogDNA 代理程式。LogDNA 代理程式會收集在 Pod 的 `/var/log` 目錄中儲存的副檔名為 `*.log` 的日誌以及無副檔名的檔案。依預設，將從所有名稱空間（包括 `kube-system`）收集日誌，並自動將日誌轉遞到 {{site.data.keyword.la_short}} 服務。
    ```
    oc create -f https://assets.us-south.logging.cloud.ibm.com/clients/logdna-agent-ds.yaml
    ```
    {: pre}
6.  編輯 LogDNA 代理程式常駐程式集配置，以參照先前建立的服務帳戶，並將安全環境定義設定為 privileged。
    ```
    oc edit ds logdna-agent
    ```
    {: pre}
    
    在配置檔中，新增下列規格。
    *   在 `spec.template.spec` 中，新增 `serviceAccount: logdna`。
    *   在 `spec.template.spec.containers` 中，新增 `securityContext: privileged: true`。
    *   如果在除 `us-south` 以外的地區中建立了 {{site.data.keyword.la_short}} 實例，請使用 `<region>` 更新 `LDAPIHOST` 和 `LDLOGHOST` 的 `spec.template.spec.containers.env` 環境變數值。

    輸出範例：
    ```
    apiVersion: extensions/v1beta1
    kind: DaemonSet
    spec:
      ...
        template:
...
        spec:
          serviceAccount: logdna
          containers:
          - securityContext:
              privileged: true
            ...
            env:
            - name: LOGDNA_AGENT_KEY
              valueFrom:
                secretKeyRef:
                  key: logdna-agent-key
                  name: logdna-agent-key
            - name: LDAPIHOST
              value: api.<region>.logging.cloud.ibm.com
            - name: LDLOGHOST
              value: logs.<region>.logging.cloud.ibm.com
          ...
    ```
    {: screen}
7.  驗證每個節點上的 `logdna-agent` Pod 是否處於 **Running** 狀態。
    ```
    oc get pods
    ```
    {: pre}
8.  在 [{{site.data.keyword.Bluemix_notm}} 觀察 > 記載主控台](https://cloud.ibm.com/observe/logging)的 {{site.data.keyword.la_short}} 實例所在列中，按一下**檢視 LogDNA**。這將開啟 LogDNA 儀表板，在其中可以開始分析日誌。

如需如何使用 {{site.data.keyword.la_short}} 的相關資訊，請參閱[後續步驟文件](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-kube#kube_next_steps)。

### 課程 4b：設定 Sysdig
{: #openshift_sysdig}

在 {{site.data.keyword.Bluemix_notm}} 帳戶中建立 {{site.data.keyword.mon_full_notm}} 實例。若要將 {{site.data.keyword.mon_short}} 實例與 OpenShift 叢集整合，必須執行 Script 來為 Sysdig 代理程序建立專案和特許服務程式帳戶。
{: shortdesc}

1.  在叢集所在的資源群組中建立 {{site.data.keyword.mon_full_notm}} 實例。選取定價方案，方案用於確定日誌的保留期，例如 `lite`。地區不必符合您叢集的地區。如需相關資訊，請參閱[佈建實例](/docs/services/Monitoring-with-Sysdig?topic=Sysdig-provision)。
    ```
    ibmcloud resource service-instance-create <service_instance_name> sysdig-monitor (lite|graduated-tier) <region> [-g <resource_group>]
    ```
    {: pre}
    
    指令範例：
    ```
    ibmcloud resource service-instance-create sysdig-openshift sysdig-monitor lite us-south
    ```
    {: pre}
    
    在輸出中，記下服務實例 **ID**，其格式為 `crn:v1:bluemix:public:logdna:<region>:<ID_string>::`。
    ```
    Service instance <name> was created.
                 
    Name:         <name>   
    ID:           crn:v1:bluemix:public:sysdig-monitor:<region>:<ID_string>::   
    GUID:         <guid>   
    Location:     <region>   
    ...
    ```
    {: screen}    
2.  取得 {{site.data.keyword.mon_short}} 實例存取金鑰。Sysdig 存取金鑰用於將安全的 Web Socket 開啟到 Sysdig 汲取伺服器，並使用 {{site.data.keyword.mon_short}} 服務來鑑別監視代理程式。
    1.  為 Sysdig 實例建立服務金鑰。
        ```
        ibmcloud resource service-key-create <key_name> Administrator --instance-id <sysdig_instance_ID>
        ```
        {: pre}
    2.  記下服務金鑰的 **Sysdig Access Key** 和 **Sysdig Collector Endpoint**。
        ```
        ibmcloud resource service-key <key_name>
        ```
        {: pre}
        
        輸出範例：
        ```
        Name:          <key_name>  
    ID:           crn:v1:bluemix:public:sysdig-monitor:<region>:<ID_string>::    
        Created At:    Thu Jun  6 21:31:25 UTC 2019   
        State:         active   
        Credentials:                                   
                       Sysdig Access Key:           11a1aa11-aa1a-1a1a-a111-11a11aa1aa11      
                       Sysdig Collector Endpoint:   ingest.<region>.monitoring.cloud.ibm.com      
                       Sysdig Customer Id:          11111      
                       Sysdig Endpoint:             https://<region>.monitoring.cloud.ibm.com  
                       apikey:                   <api_key_value>      
                       iam_apikey_description:   Auto-generated for key <ID>     
                       iam_apikey_name:          <key_name>       
                       iam_role_crn:             crn:v1:bluemix:public:iam::::role:Administrator      
                       iam_serviceid_crn:        crn:v1:bluemix:public:iam-identity::<ID_string>       
        ```
        {: screen}
3.  執行 Script 來為 `ibm-observe` 專案設定特許服務程式帳戶和 Kubernetes 常駐程式集，以在 Kubernetes 叢集的每個工作者節點上部署 Sysdig 代理程式。Sysdig 代理程式會收集工作者節點 CPU 使用率、工作者節點記憶體用量、與容器之間的 HTTP 資料流量以及多個基礎架構元件相關資料等度量。 

    在下列指令中，將 <sysdig_access_key> 和 <sysdig_collector_endpoint> 取代為先前建立的服務金鑰中的值。對於 <tag>，可以將標籤與 Sysdig 代理程式相關聯，例如 `role:service,location:us-south`，以協助確定度量所來自的環境。

    ```
    curl -sL https://ibm.biz/install-sysdig-k8s-agent | bash -s -- -a <sysdig_access_key> -c <sysdig_collector_endpoint> -t <tag> -ac 'sysdig_capture_enabled: false' --openshift
    ```
    {: pre}
    
            輸出範例：
        ```
    * Detecting operating system
    * Downloading Sysdig cluster role yaml
    * Downloading Sysdig config map yaml
    * Downloading Sysdig daemonset v2 yaml
    * Creating project: ibm-observe
    * Creating sysdig-agent serviceaccount in project: ibm-observe
    * Creating sysdig-agent access policies
    * Creating sysdig-agent secret using the ACCESS_KEY provided
    * Retreiving the IKS Cluster ID and Cluster Name
    * Setting cluster name as <cluster_name>
    * Setting ibm.containers-kubernetes.cluster.id 1fbd0c2ab7dd4c9bb1f2c2f7b36f5c47
    * Updating agent configmap and applying to cluster
    * Setting tags
    * Setting collector endpoint
    * Adding additional configuration to dragent.yaml
    * Enabling Prometheus
    configmap/sysdig-agent created
    * Deploying the sysdig agent
    daemonset.extensions/sysdig-agent created
    ```
    {: screen}
        
4.  驗證每個節點上的 `sysdig-agent` Pod 是否顯示 **1/1** Pod 就緒，以及每個 Pod 的狀態是否為 **Running**。
    ```
    oc get pods
    ```
    {: pre}
    
    輸出範例：
    ```
    NAME                 READY     STATUS    RESTARTS   AGE
    sysdig-agent-qrbcq   1/1       Running   0          1m
    sysdig-agent-rhrgz   1/1       Running   0          1m
    ```
    {: screen}
5.  在 [{{site.data.keyword.Bluemix_notm}} 觀察 > 監視主控台](https://cloud.ibm.com/observe/logging)的 {{site.data.keyword.mon_short}} 實例所在列中，按一下**檢視 Sysdig**。這將開啟 Sysdig 儀表板，在其中可以開始分析叢集度量。

如需如何使用 {{site.data.keyword.mon_short}} 的相關資訊，請參閱[後續步驟文件](/docs/services/Monitoring-with-Sysdig?topic=Sysdig-kubernetes_cluster#kubernetes_cluster_next_steps)。

### 選用：清除
{: #openshift_logdna_sysdig_cleanup}

從叢集和 {{site.data.keyword.Bluemix_notm}} 帳戶中移除 {{site.data.keyword.la_short}} 和 {{site.data.keyword.mon_short}} 實例。請注意，除非您在[持續性儲存空間](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-archiving)中儲存日誌和度量，否則在從帳戶中刪除實例後，就無法存取這些資訊。
{: shortdesc}

1.  透過移除為叢集裡的 {{site.data.keyword.la_short}} 和 {{site.data.keyword.mon_short}} 實例建立的專案來清除這些實例。刪除專案時，還會同時移除其資源，例如服務帳戶和常駐程式集。
    ```
    oc delete project logdna
    ```
    {: pre}
    ```
    oc delete project ibm-observe
    ```
    {: pre}
2.  從 {{site.data.keyword.Bluemix_notm}} 帳戶中移除實例。
    *   [移除 {{site.data.keyword.la_short}} 實例](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-remove)。
    *   [移除 {{site.data.keyword.mon_short}} 實例](/docs/services/Monitoring-with-Sysdig?topic=Sysdig-remove)。

<br />


## 限制

{: #openshift_limitations}

發佈的 Red Hat OpenShift on IBM Cloud 測試版存在下列限制。
{: shortdesc}

**叢集**：
*   只能建立標準叢集，而不能建立免費叢集。
*   位置在兩個多區域都會區域（華盛頓特區和倫敦）中提供使用。支援的區域為 `wdc04、wdc06、wdc07、lon04、lon05` 和 `lon06`。
*   建立的叢集不能包含執行多個作業系統的工作者節點，例如，Red Hat Enterprise Linux 上的 OpenShift 和 Ubuntu 上的原生 Kubernetes。
*   不支援[叢集自動縮放器](/docs/containers?topic=containers-ca)，因為此工具需要 Kubernetes 1.12 版或更高版本。OpenShift 3.11 僅包含 Kubernetes 1.11 版。



**儲存空間**：
*   支援 {{site.data.keyword.Bluemix_notm}} File Storage、Block Storage 和 Cloud Object Storage。不支援軟體定義的儲存空間 (SDS)。
*   受 {{site.data.keyword.Bluemix_notm}} NFS 檔案儲存空間配置 Linux 使用者許可權的方式的影響，在使用檔案儲存空間時可能會遇到錯誤。如果發生這種情況，可能需要配置 [OpenShift 安全環境定義限制 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.openshift.com/container-platform/3.11/admin_guide/manage_scc.html) 或使用其他儲存空間類型。

**網路連線功能**：
*   用作網路連線功能原則提供者的是 Calico，而不是 OpenShift SDN。

**附加程式、整合和其他服務**：
*   {{site.data.keyword.containerlong_notm}} 附加程式（例如，Istio、Knative 和 Kubernetes Terminal）無法使用。
*   Helm chart 未經過認證，無法在 OpenShift 叢集裡使用，但 {{site.data.keyword.Bluemix_notm}} Object Storage 除外。
*   叢集未部署為使用 {{site.data.keyword.registryshort_notm}} `icr.io` 網域的映像檔取回密碼。可以[建立您自己的映像檔取回密碼](/docs/containers?topic=containers-images#other_registry_accounts)，或者改為使用 OpenShift 叢集的內建 Docker 登錄。

**應用程式**：
*   依預設，OpenShift 設定的安全設定比原生 Kubernetes 更嚴格。如需相關資訊，請參閱[管理安全環境定義限制 (SCC) ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.openshift.com/container-platform/3.11/admin_guide/manage_scc.html) 的 OpenShift 文件。
*   例如，配置為以 root 使用者身分執行的應用程式可能失敗，其中 Pod 處於 `CrashLoopBackOff` 狀態。若要解決此問題，可以修改預設安全環境定義限制，或使用不以 root 使用者身分執行的映像檔。
*   依預設，OpenShift 設定為使用本端 Docker 登錄。如果要使用儲存在遠端專用 {{site.data.keyword.registrylong_notm}} `icr.io` 網域名稱中的映像檔，則必須自行為每個廣域和區網域登錄建立密碼。可以使用[複製 `default-<region>-icr-io` 密碼](/docs/containers?topic=containers-images#copy_imagePullSecret)將這些密碼從 `default` 名稱空間複製到要從中取回映像檔的名稱空間，或者可以[建立您自己的密碼](/docs/containers?topic=containers-images#other_registry_accounts)。然後，向部署配置或名稱空間服務帳戶[新增映像檔取回密碼](/docs/containers?topic=containers-images#use_imagePullSecret)。
*   將使用 OpenShift 主控台，而不是 Kubernetes 儀表板。

<br />


## 下一步為何？
{: #openshift_next}

如需使用應用程式和遞送服務的相關資訊，請參閱 [OpenShift Developer Guide](https://docs.openshift.com/container-platform/3.11/dev_guide/index.html)。

<br />


## 意見和問題
{: #openshift_support}

在處於測試版期間，IBM 支援中心和 Red Hat 支援中心的服務範圍均不包括 Red Hat OpenShift on IBM Cloud 叢集。提供的任何支援都以協助您評估該產品，為其正式發佈做準備。
{: important}

如有任何問題或意見，請在 Slack 中張貼。 
*   如果您是外部使用者，請在 [#openshift ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://ibm-container-service.slack.com/messages/CKCJLJCH4) 頻道中張貼。 
*   如果您是 IBM 員工，請使用 [#iks-openshift-users ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://ibm-argonauts.slack.com/messages/CJH0UPN2D) 頻道。

如果未使用 {{site.data.keyword.Bluemix_notm}} 帳戶的 IBM ID，請針對此 Slack [要求邀請](https://bxcs-slack-invite.mybluemix.net/)。
{: tip}
