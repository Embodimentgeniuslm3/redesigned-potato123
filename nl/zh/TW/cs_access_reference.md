---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

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



# 使用者存取許可權
{: #access_reference}

當您[指派叢集許可權](/docs/containers?topic=containers-users)時，很難判斷您需要指派給使用者的角色。使用下列各節中的表格，來判定在 {{site.data.keyword.containerlong}} 中執行一般作業所需的最低許可權層次。
{: shortdesc}

自 2019 年 1 月 30 日起，{{site.data.keyword.containerlong_notm}} 提供一個使用 {{site.data.keyword.Bluemix_notm}} IAM 來授權使用者的新方法：[服務存取角色](#service)。這些服務角色用來授與對叢集裡資源（例如 Kubernetes 名稱空間）的存取權。如需相關資訊，請參閱部落格 [Introducing service roles and namespaces in IAM for more granular control of cluster access ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/blogs/bluemix/2019/02/introducing-service-roles-and-namespaces-in-iam-for-more-granular-control-of-cluster-access/)。
{: note}

## {{site.data.keyword.Bluemix_notm}} IAM 平台角色
{: #iam_platform}

{{site.data.keyword.containerlong_notm}} 已配置為使用 {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM) 角色。{{site.data.keyword.Bluemix_notm}} IAM 平台角色決定使用者可對 {{site.data.keyword.Bluemix_notm}} 資源（例如叢集、工作者節點及 Ingress 應用程式負載平衡器 (ALB)）執行的動作。{{site.data.keyword.Bluemix_notm}} IAM 平台角色也為使用者自動設定基本的基礎架構許可權。若要設定平台角色，請參閱[指派 {{site.data.keyword.Bluemix_notm}} IAM 平台許可權](/docs/containers?topic=containers-users#platform)。
{: shortdesc}

<p class="tip">不要同時指派 {{site.data.keyword.Bluemix_notm}} IAM 平台角色與服務角色。您必須個別指派平台角色及服務角色。</p>

下列每一節的表格顯示每個 {{site.data.keyword.Bluemix_notm}} IAM 平台角色所授與的叢集管理、記載和 Ingress 許可權。這些表格是依 CLI 指令名稱的字母順序來排列。

* [不需要許可權的動作](#none-actions)
* [檢視者動作](#view-actions)
* [編輯者動作](#editor-actions)
* [操作員動作](#operator-actions)
* [管理者動作](#admin-actions)

### 不需要許可權的動作
{: #none-actions}

您帳戶中執行 CLI 指令或進行 API 呼叫以進行下表動作的任何使用者，都會看到結果（即使該使用者沒有獲得指派許可權）。
{: shortdesc}

<table>
<caption>{{site.data.keyword.containerlong_notm}} 中不需要許可權的 CLI 指令及 API 呼叫的概觀</caption>
<thead>
<th id="none-actions-action">動作</th>
<th id="none-actions-cli">CLI 指令</th>
<th id="none-actions-api">API 呼叫</th>
</thead>
<tbody>
<tr>
<td>檢視 {{site.data.keyword.containerlong_notm}} 中支援的受管理附加程式版本的清單。</td>
<td><code>[ibmcloud ks addon-versions](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_addon_versions)</code></td>
<td><code>[GET /v1/addon](https://containers.cloud.ibm.com/global/swagger-global-api/#/util/GetAddons)</code></td>
</tr>
<tr>
<td>為 {{site.data.keyword.containerlong_notm}} 設定目標或檢視 API 端點。</td>
<td><code>[ibmcloud ks api
](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cli_api)</code></td>
<td>-</td>
</tr>
<tr>
<td>檢視所支援指令及參數的清單。</td>
<td><code>[ibmcloud ks help](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_help)</code></td>
<td>-</td>
</tr>
<tr>
<td>起始設定 {{site.data.keyword.containerlong_notm}} 外掛程式，或指定您要建立或存取 Kubernetes 叢集的地區。</td>
<td><code>[ibmcloud ks init](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_init)</code></td>
<td>-</td>
</tr>
<tr>
<td>已淘汰：檢視 {{site.data.keyword.containerlong_notm}} 中支援的 Kubernetes 版本的清單。</td>
<td><code>[ibmcloud ks kube-versions](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_kube_versions)</code></td>
<td><code>[GET /v1/kube-versions](https://containers.cloud.ibm.com/global/swagger-global-api/#/util/GetKubeVersions)</code></td>
</tr>
<tr>
<td>檢視工作者節點的可用機型清單。</td>
<td><code>[ibmcloud ks machine-types](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_machine_types)</code></td>
<td><code>[GET /v1/datacenters/{datacenter}/machine-types](https://containers.cloud.ibm.com/global/swagger-global-api/#/util/GetDatacenterMachineTypes)</code></td>
</tr>
<tr>
<td>檢視 IBM ID 使用者的現行訊息。</td>
<td><code>[ibmcloud ks messages](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_messages)</code></td>
<td><code>[GET /v1/messages](https://containers.cloud.ibm.com/global/swagger-global-api/#/util/GetMessages)</code></td>
</tr>
<tr>
<td>已淘汰：尋找您現行所在的 {{site.data.keyword.containerlong_notm}} 地區。</td>
<td><code>[ibmcloud ks region](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_region)</code></td>
<td>-</td>
</tr>
<tr>
<td>已淘汰：設定 {{site.data.keyword.containerlong_notm}} 的地區。</td>
<td><code>[ibmcloud ks region-set](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_region-set)</code></td>
<td>-</td>
</tr>
<tr>
<td>已淘汰：列出可用的地區。</td>
<td><code>[ibmcloud ks regions](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_regions)</code></td>
<td><code>[GET /v1/regions](https://containers.cloud.ibm.com/global/swagger-global-api/#/util/GetRegions)</code></td>
</tr>
<tr>
<td>檢視 {{site.data.keyword.containerlong_notm}} 中支援的位置的清單。</td>
<td><code>[ibmcloud ks supported-locations](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_supported-locations)</code></td>
<td><code>[GET /v1/locations](https://containers.cloud.ibm.com/global/swagger-global-api/#/util/ListLocations)</code></td>
</tr>
<tr>
<td>檢視 {{site.data.keyword.containerlong_notm}} 中支援的版本的清單。</td>
<td><code>[ibmcloud ks versions](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_versions)</code></td>
<td>-</td>
</tr>
<tr>
<td>檢視您可在其中建立叢集的可用區域清單。</td>
<td><code>[ibmcloud ks zones](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_datacenters)</code></td>
<td><code>[GET /v1/zones](https://containers.cloud.ibm.com/global/swagger-global-api/#/util/GetZones)</code></td>
</tr>
</tbody>
</table>

### 檢視者動作
{: #view-actions}

**檢視者**平台角色包括[不需要許可權的動作](#none-actions)，以及下表所顯示的許可權。
使用**檢視者**角色時，審核員或帳單這類使用者可以看到叢集詳細資料，但無法修改基礎架構。
{: shortdesc}

<table>
<caption>在 {{site.data.keyword.containerlong_notm}} 中需要檢視者平台角色的 CLI 指令和 API 呼叫概觀</caption>
<thead>
<th id="view-actions-mngt">動作</th>
<th id="view-actions-cli">CLI 指令</th>
<th id="view-actions-api">API 呼叫</th>
</thead>
<tbody>
<tr>
<td>檢視 Ingress ALB 的資訊。</td>
<td><code>[ibmcloud ks alb-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_get)</code></td>
<td><code>[GET /albs/{albId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/GetClusterALB)</code></td>
</tr>
<tr>
<td>檢視地區中支援的 ALB 類型。</td>
<td><code>[ibmcloud ks alb-types](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_types)</code></td>
<td><code>[GET /albtypes](https://containers.cloud.ibm.com/global/swagger-global-api/#/util/GetAvailableALBTypes)</code></td>
</tr>
<tr>
<td>列出叢集裡的所有 Ingress ALB。</td>
<td><code>[ibmcloud ks albs](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_albs)</code></td>
<td><code>[GET /clusters/{idOrName}](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/GetClusterALBs)</code></td>
</tr>
<tr>
<td>檢視資源群組及地區之 {{site.data.keyword.Bluemix_notm}} IAM API 金鑰擁有者的名稱及電子郵件位址。</td>
<td><code>[ibmcloud ks api-key-info](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_api_key_info)</code></td>
<td><code>[GET /v1/logging/{idOrName}/clusterkeyowner](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/GetClusterKeyOwner)</code></td>
</tr>
<tr>
<td>下載 Kubernetes 配置資料及憑證，以便連接至叢集並執行 `kubectl` 指令。</td>
<td><code>[ibmcloud ks cluster-config](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_config)</code></td>
<td><code>[GET /v1/clusters/{idOrName}/config](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetClusterConfig)</code></td>
</tr>
<tr>
<td>檢視叢集的資訊。</td>
<td><code>[ibmcloud ks cluster-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_get)</code></td>
<td><code>[GET /v1/clusters/{idOrName}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetCluster)</code></td>
</tr>
<tr>
<td>列出連結至叢集之所有名稱空間中的所有服務。</td>
<td><code>[ibmcloud ks cluster-services](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_services)</code></td>
<td><code>[GET /v1/clusters/{idOrName}/services](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/ListServicesForAllNamespaces)</code></td>
</tr>
<tr>
<td>列出所有叢集。</td>
<td><code>[ibmcloud ks clusters](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_clusters)</code></td>
<td><code>[GET /v1/clusters](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetClusters)</code></td>
</tr>
<tr>
<td>取得針對 {{site.data.keyword.Bluemix_notm}} 帳戶設定的基礎架構認證，以存取不同的 IBM Cloud 基礎架構 (SoftLayer) 組合。</td>
<td><code>[ibmcloud ks credential-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credential_get)</code></td><td><code>[GET /v1/credentials](https://containers.cloud.ibm.com/global/swagger-global-api/#/accounts/GetUserCredentials)</code></td>
</tr>
<tr>
<td>檢查容許存取目標地區和資源群組的 IBM Cloud 基礎架構 (SoftLayer) 組合的認證是否遺漏建議或必要的基礎架構許可權。</td>
<td><code>[ibmcloud ks infra-permissions-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#infra_permissions_get)</code></td>
<td><code>[GET /v1/infra-permissions](https://containers.cloud.ibm.com/global/swagger-global-api/#/accounts/GetInfraPermissions)</code></td>
</tr>
<tr>
<td>檢視 Fluentd 附加程式的自動更新狀態。</td>
<td><code>[ibmcloud ks logging-autoupdate-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_autoupdate_get)</code></td>
<td><code>[GET /v1/logging/{idOrName}/updatepolicy](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/GetUpdatePolicy)</code></td>
</tr>
<tr>
<td>檢視目標地區的預設記載端點。</td>
<td>-</td>
<td><code>[GET /v1/logging/{idOrName}/default](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/GetDefaultLoggingEndpoint)</code></td>
</tr>
<tr>
<td>列出叢集裡所有日誌轉遞配置，或叢集裡特定日誌來源的日誌轉遞配置。</td>
<td><code>[ibmcloud ks logging-config-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_logging_get)</code></td>
<td><code>[GET /v1/logging/{idOrName}/loggingconfig](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/FetchLoggingConfigs) 及 [GET /v1/logging/{idOrName}/loggingconfig/{logSource}](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/FetchLoggingConfigsForSource)</code></td>
</tr>
<tr>
<td>檢視日誌過濾配置的資訊。</td>
<td><code>[ibmcloud ks logging-filter-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_filter_view)</code></td>
<td><code>[GET /v1/logging/{idOrName}/filterconfigs/{id}](https://containers.cloud.ibm.com/global/swagger-global-api/#/filter/FetchFilterConfig)</code></td>
</tr>
<tr>
<td>列出叢集裡的所有日誌過濾配置。</td>
<td><code>[ibmcloud ks logging-filter-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_filter_view)</code></td>
<td><code>[GET /v1/logging/{idOrName}/filterconfigs](https://containers.cloud.ibm.com/global/swagger-global-api/#/filter/FetchFilterConfigs)</code></td>
</tr>
<tr>
<td>列出連結至特定名稱空間的所有服務。</td>
<td>-</td>
<td><code>[GET /v1/clusters/{idOrName}/services/{namespace}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/ListServicesInNamespace)</code></td>
</tr>
<tr>
<td>列出所有連結至叢集的使用者管理的子網路。</td>
<td>-</td>
<td><code>[GET /v1/clusters/{idOrName}/usersubnets](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetClusterUserSubnet)</code></td>
</tr>
<tr>
<td>列出基礎架構帳戶中的可用子網路。</td>
<td><code>[ibmcloud ks subnets](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_subnets)</code></td>
<td><code>[GET /v1/subnets](https://containers.cloud.ibm.com/global/swagger-global-api/#/properties/ListSubnets)</code></td>
</tr>
<tr>
<td>檢視基礎架構帳戶的 VLAN Spanning 狀態。</td>
<td><code>[ibmcloud ks vlan-spanning-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get)</code></td>
<td><code>[GET /v1/subnets/vlan-spanning](https://containers.cloud.ibm.com/global/swagger-global-api/#/accounts/GetVlanSpanning)</code></td>
</tr>
<tr>
<td>針對某個叢集設定時：列出區域中叢集所連接的 VLAN。</br>針對帳戶中的所有叢集設定時：列出區域中所有可用的 VLAN。</td>
<td><code>[ibmcloud ks vlans](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlans)</code></td>
<td><code>[GET /v1/datacenters/{datacenter}/vlans](https://containers.cloud.ibm.com/global/swagger-global-api/#/properties/GetDatacenterVLANs)</code></td>
</tr>
<tr>
<td>列出叢集的所有 Webhook。</td>
<td>-</td>
<td><code>[GET /v1/clusters/{idOrName}/webhooks](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetClusterWebhooks)</code></td>
</tr>
<tr>
<td>檢視工作者節點的資訊。</td>
<td><code>[ibmcloud ks worker-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_get)</code></td>
<td><code>[GET /v1/clusters/{idOrName}/workers/{workerId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetWorkers)</code></td>
</tr>
<tr>
<td>檢視工作者節點儲存區的資訊。</td>
<td><code>[ibmcloud ks worker-pool-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_get)</code></td>
<td><code>[GET /v1/clusters/{idOrName}/workerpools/{poolidOrName}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetWorkerPool)</code></td>
</tr>
<tr>
<td>列出叢集裡的所有工作者節點儲存區。</td>
<td><code>[ibmcloud ks worker-pools](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pools)</code></td>
<td><code>[GET /v1/clusters/{idOrName}/workerpools](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetWorkerPools)</code></td>
</tr>
<tr>
<td>列出叢集裡的所有工作者節點。</td>
<td><code>[ibmcloud ks workers](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_workers)</code></td>
<td><code>[GET /v1/clusters/{idOrName}/workers](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetClusterWorkers)</code></td>
</tr>
</tbody>
</table>

### 編輯者動作
{: #editor-actions}

**編輯者**平台角色包括**檢視者**授與的許可權，以及下列許可權。使用**編輯者**角色，開發人員這類使用者可以連結服務、使用 Ingress 資源，以及設定其應用程式的日誌轉遞，但無法修改基礎架構。**提示**：請對應用程式開發人員使用此角色，並指派 <a href="#cloud-foundry">Cloud Foundry</a> **開發人員**角色。
{: shortdesc}

<table>
<caption>在 {{site.data.keyword.containerlong_notm}} 中需要編輯器平台角色的 CLI 指令和 API 呼叫概觀</caption>
<thead>
<th id="editor-actions-mngt">動作</th>
<th id="editor-actions-cli">CLI 指令</th>
<th id="editor-actions-api">API 呼叫</th>
</thead>
<tbody>
<tr>
<td>停用 Ingress ALB 附加程式的自動更新。</td>
<td><code>[ibmcloud ks alb-autoupdate-disable](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_autoupdate_disable)</code></td>
<td><code>[PUT /clusters/{idOrName}/updatepolicy](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/ChangeUpdatePolicy)</code></td>
</tr>
<tr>
<td>啟用 Ingress ALB 附加程式的自動更新。</td>
<td><code>[ibmcloud ks alb-autoupdate-enable](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_autoupdate_enable)</code></td>
<td><code>[PUT /clusters/{idOrName}/updatepolicy](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/ChangeUpdatePolicy)</code></td>
</tr>
<tr>
<td>檢查是否已啟用了 Ingress ALB 附加程式的自動更新。</td>
<td><code>[ibmcloud ks alb-autoupdate-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_autoupdate_get)</code></td>
<td><code>[GET /clusters/{idOrName}/updatepolicy](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/GetUpdatePolicy)</code></td>
</tr>
<tr>
<td>啟用或停用 Ingress ALB。</td>
<td><code>[ibmcloud ks alb-configure](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_configure)</code></td>
<td><code>[POST /albs](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/EnableALB) and [DELETE /albs/{albId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/)</code></td>
</tr>
<tr>
<td>建立 Ingress ALB。</td>
<td><code>[ibmcloud ks alb-create](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_create)</code></td>
<td><code>[POST /clusters/{idOrName}/zone/{zoneId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/CreateALB)</code></td>
</tr>
<tr>
<td>將 Ingress ALB 附加程式更新回復到 ALB Pod 先前在執行的建置。</td>
<td><code>[ibmcloud ks alb-rollback](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_rollback)</code></td>
<td><code>[PUT /clusters/{idOrName}/updaterollback](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/RollbackUpdate)</code></td>
</tr>
<tr>
<td>手動更新 Ingress ALB 附加程式，以強制一次性更新您的 ALB Pod。</td>
<td><code>[ibmcloud ks alb-update](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_update)</code></td>
<td><code>[PUT /clusters/{idOrName}/update](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/UpdateALBs)</code></td>
</tr>
<tr>
<td>建立 API 伺服器審核 Webhook。</td>
<td><code>[ibmcloud ks apiserver-config-set](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_apiserver_config_set)</code></td>
<td><code>[PUT /v1/clusters/{idOrName}/apiserverconfigs/auditwebhook](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/apiserverconfigs/UpdateAuditWebhook)</code></td>
</tr>
<tr>
<td>刪除 API 伺服器審核 Webhook。</td>
<td><code>[ibmcloud ks apiserver-config-unset](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_apiserver_config_unset)</code></td>
<td><code>[DELETE /v1/clusters/{idOrName}/apiserverconfigs/auditwebhook](https://containers.cloud.ibm.com/global/swagger-global-api/#/apiserverconfigs/DeleteAuditWebhook)</code></td>
</tr>
<tr>
<td>將服務連結至叢集。**附註**：您必須具有服務實例所在空間的 Cloud Foundry 開發人員角色。</td>
<td><code>[ibmcloud ks cluster-service-bind](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_service_bind)</code></td>
<td><code>[POST /v1/clusters/{idOrName}/services](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/BindServiceToNamespace)</code></td>
</tr>
<tr>
<td>將服務與叢集取消連結。**附註**：您必須具有服務實例所在空間的 Cloud Foundry 開發人員角色。</td>
<td><code>[ibmcloud ks cluster-service-unbind](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_service_unbind)</code></td>
<td><code>[DELETE /v1/clusters/{idOrName}/services/{namespace}/{serviceInstanceId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/UnbindServiceFromNamespace)</code></td>
</tr>
<tr>
<td>為所有日誌來源（<code>kube-audit</code> 除外）建立日誌轉遞配置。</td>
<td><code>[ibmcloud ks logging-config-create](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_logging_create)</code></td>
<td><code>[POST /v1/logging/{idOrName}/loggingconfig/{logSource}](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/CreateLoggingConfig)</code></td>
</tr>
<tr>
<td>重新整理日誌轉遞配置。</td>
<td><code>[ibmcloud ks logging-config-refresh](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_logging_refresh)</code></td>
<td><code>[PUT /v1/logging/{idOrName}/refresh](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/RefreshLoggingConfig)</code></td>
</tr>
<tr>
<td>為所有日誌來源（<code>kube-audit</code> 除外）刪除日誌轉遞配置。</td>
<td><code>[ibmcloud ks logging-config-rm](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_logging_rm)</code></td>
<td><code>[DELETE /v1/logging/{idOrName}/loggingconfig/{logSource}/{id}](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/DeleteLoggingConfig)</code></td>
</tr>
<tr>
<td>刪除叢集的所有日誌轉遞配置。</td>
<td>-</td>
<td><code>[DELETE /v1/logging/{idOrName}/loggingconfig](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/DeleteLoggingConfigs)</code></td>
</tr>
<tr>
<td>更新日誌轉遞配置。
    </td>
<td><code>[ibmcloud ks logging-config-update](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_logging_update)</code></td>
<td><code>[PUT /v1/logging/{idOrName}/loggingconfig/{logSource}/{id}](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/UpdateLoggingConfig)</code></td>
</tr>
<tr>
<td>建立日誌過濾配置。</td>
<td><code>[ibmcloud ks logging-filter-create](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_filter_create)</code></td>
<td><code>[POST /v1/logging/{idOrName}/filterconfigs](https://containers.cloud.ibm.com/global/swagger-global-api/#/filter/CreateFilterConfig)</code></td>
</tr>
<tr>
<td>刪除日誌過濾配置。</td>
<td><code>[ibmcloud ks logging-filter-rm](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_filter_delete)</code></td>
<td><code>[DELETE /v1/logging/{idOrName}/filterconfigs/{id}](https://containers.cloud.ibm.com/global/swagger-global-api/#/filter/DeleteFilterConfig)</code></td>
</tr>
<tr>
<td>刪除 Kubernetes 叢集的所有日誌過濾配置。</td>
<td>-</td>
<td><code>[DELETE /v1/logging/{idOrName}/filterconfigs](https://containers.cloud.ibm.com/global/swagger-global-api/#/filter/DeleteFilterConfigs)</code></td>
</tr>
<tr>
<td>更新日誌過濾配置。</td>
<td><code>[ibmcloud ks logging-filter-update](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_filter_update)</code></td>
<td><code>[PUT /v1/logging/{idOrName}/filterconfigs/{id}](https://containers.cloud.ibm.com/global/swagger-global-api/#/filter/UpdateFilterConfig)</code></td>
</tr>
<tr>
<td>將某個 NLB IP 位址新增至現有的 NLB 主機名稱。</td>
<td><code>[ibmcloud ks nlb-dns-add](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-add)</code></td>
<td><code>[PUT /clusters/{idOrName}/add](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb45dns/UpdateDNSWithIP)</code></td>
</tr>
<tr>
<td>建立 DNS 主機名稱以登錄 NLB IP 位址。</td>
<td><code>[ibmcloud ks nlb-dns-create](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-create)</code></td>
<td><code>[POST /clusters/{idOrName}/register](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb45dns/RegisterDNSWithIP)</code></td>
</tr>
<tr>
<td>列出在叢集裡登錄的 NLB 主機名稱及 IP 位址。</td>
<td><code>[ibmcloud ks nlb-dnss](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-ls)</code></td>
<td><code>[GET /clusters/{idOrName}/list](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb45dns/ListNLBIPsForSubdomain)</code></td>
</tr>
<tr>
<td>從主機名稱移除 NLB IP 位址。</td>
<td><code>[ibmcloud ks nlb-dns-rm](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-rm)</code></td>
<td><code>[DELETE /clusters/{idOrName}/host/{nlbHost}/ip/{nlbIP}/remove](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb45dns/UnregisterDNSWithIP)</code></td>
</tr>
<tr>
<td>配置及選擇性地啟用叢集裡現有 NLB 主機名稱的性能檢查監視器。</td>
<td><code>[ibmcloud ks nlb-dns-monitor-configure](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-monitor-configure)</code></td>
<td><code>[POST /health/clusters/{idOrName}/config](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb45health45monitor/AddNlbDNSHealthMonitor)</code></td>
</tr>
<tr>
<td>檢視現有性能檢查監視器的設定。</td>
<td><code>[ibmcloud ks nlb-dns-monitor-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-monitor-get)</code></td>
<td><code>[GET /health/clusters/{idOrName}/host/{nlbHost}/config](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb45health45monitor/GetNlbDNSHealthMonitor)</code></td>
</tr>
<tr>
<td>停用叢集裡主機名稱的現有性能檢查監視器。</td>
<td><code>[ibmcloud ks nlb-dns-monitor-disable](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-monitor-disable)</code></td>
<td><code>[PUT /clusters/{idOrName}/health](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb45health45monitor/UpdateNlbDNSHealthMonitor)</code></td>
</tr>
<tr>
<td>啟用您已配置的性能檢查監視器。</td>
<td><code>[ibmcloud ks nlb-dns-monitor-enable](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-monitor-enable)</code></td>
<td><code>[PUT /clusters/{idOrName}/health](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb45health45monitor/UpdateNlbDNSHealthMonitor)</code></td>
</tr>
<tr>
<td>列出叢集裡每個 NLB 主機名稱的性能檢查監視器設定。</td>
<td><code>[ibmcloud ks nlb-dns-monitor-ls](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-monitor-ls)</code></td>
<td><code>[GET /health/clusters/{idOrName}/list](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb45health45monitor/ListNlbDNSHealthMonitors)</code></td>
</tr>
<tr>
<td>列出使用叢集裡 NLB 主機名稱登錄的每個 IP 位址的性能檢查狀態。</td>
<td><code>[ibmcloud ks nlb-dns-monitor-status](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-monitor-status)</code></td>
<td><code>[GET /health/clusters/{idOrName}/status](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb45health45monitor/ListNlbDNSHealthMonitorStatus)</code></td>
</tr>
<tr>
<td>在叢集裡建立 Webhook。</td>
<td><code>[ibmcloud ks webhook-create](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_webhook_create)</code></td>
<td><code>[POST /v1/clusters/{idOrName}/webhooks](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/AddClusterWebhooks)</code></td>
</tr>
</tbody>
</table>

### 操作員動作
{: #operator-actions}

**操作員**平台角色包括**檢視者**授與的許可權，以及下表所顯示的許可權。
使用**操作員**角色，網站可靠性工程師、DevOps 工程師或叢集管理者這類使用者可以新增工作者節點以及進行基礎架構疑難排解（例如，重新載入工作者節點），但無法建立或刪除叢集、變更認證，或設定服務端點或受管理附加程式這類全叢集特性。
{: shortdesc}

<table>
<caption>在 {{site.data.keyword.containerlong_notm}} 中需要操作者平台角色的 CLI 指令和 API 呼叫概觀</caption>
<thead>
<th id="operator-mgmt">動作</th>
<th id="operator-cli">CLI 指令</th>
<th id="operator-api">API 呼叫</th>
</thead>
<tbody>
<tr>
<td>重新整理 Kubernetes 主節點。</td>
<td><code>[ibmcloud ks apiserver-refresh](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_apiserver_refresh) (cluster-refresh)</code></td>
<td><code>[PUT /v1/clusters/{idOrName}/masters](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/HandleMasterAPIServer)</code></td>
</tr>
<tr>
<td>建立叢集的 {{site.data.keyword.Bluemix_notm}} IAM 服務 ID、為在 {{site.data.keyword.registrylong_notm}} 中指派**讀者**服務存取角色的服務 ID 建立一個原則，然後建立該服務 ID 的 API 金鑰。</td>
<td><code>[ibmcloud ks cluster-pull-secret-apply](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_pull_secret_apply)</code></td>
<td>-</td>
</tr>
<tr>
<td>將子網路新增至叢集。</td>
<td><code>[ibmcloud ks cluster-subnet-add](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_subnet_add)</code></td>
<td><code>[PUT /v1/clusters/{idOrName}/subnets/{subnetId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/AddClusterSubnet)</code></td>
</tr>
<tr>
<td>建立子網路並將其新增到叢集。</td>
<td><code>[ibmcloud ks cluster-subnet-create](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_subnet_create)</code></td>
<td><code>[POST /v1/clusters/{idOrName}/vlans/{vlanId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/CreateClusterSubnet)</code></td>
</tr>
<tr>
<td>更新叢集。</td>
<td><code>[ibmcloud ks cluster-update](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_update)</code></td>
<td><code>[PUT /v1/clusters/{idOrName}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/UpdateCluster)</code></td>
</tr>
<tr>
<td>將使用者管理的子網路新增至叢集。</td>
<td><code>[ibmcloud ks cluster-user-subnet-add](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_user_subnet_add)</code></td>
<td><code>[POST /v1/clusters/{idOrName}/usersubnets](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/AddClusterUserSubnet)</code></td>
</tr>
<tr>
<td>從叢集移除使用者管理的子網路。</td>
<td><code>[ibmcloud ks cluster-user-subnet-rm](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_user_subnet_rm)</code></td>
<td><code>[DELETE /v1/clusters/{idOrName}/usersubnets/{subnetId}/vlans/{vlanId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/RemoveClusterUserSubnet)</code></td>
</tr>
<tr>
<td>新增工作者節點。</td>
<td><code>[ibmcloud ks worker-add (deprecated)](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_add)</code></td>
<td><code>[POST /v1/clusters/{idOrName}/workers](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/AddClusterWorkers)</code></td>
</tr>
<tr>
<td>建立工作者節點儲存區。</td>
<td><code>[ibmcloud ks worker-pool-create](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_create)</code></td>
<td><code>[POST /v1/clusters/{idOrName}/workerpools](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/CreateWorkerPool)</code></td>
</tr>
<tr>
<td>重新平衡工作者節點儲存區。</td>
<td><code>[ibmcloud ks worker-pool-rebalance](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_rebalance)</code></td>
<td><code>[PATCH /v1/clusters/{idOrName}/workerpools/{poolidOrName}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/PatchWorkerPool)</code></td>
</tr>
<tr>
<td>調整工作者節點儲存區的大小。</td>
<td><code>[ibmcloud ks worker-pool-resize](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_resize)</code></td>
<td><code>[PATCH /v1/clusters/{idOrName}/workerpools/{poolidOrName}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/PatchWorkerPool)</code></td>
</tr>
<tr>
<td>刪除工作者節點儲存區。</td>
<td><code>[ibmcloud ks worker-pool-rm](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_rm)</code></td>
<td><code>[DELETE /v1/clusters/{idOrName}/workerpools/{poolidOrName}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/RemoveWorkerPool)</code></td>
</tr>
<tr>
<td>重新啟動工作者節點。</td>
<td><code>[ibmcloud ks worker-reboot](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reboot)</code></td>
<td><code>[PUT /v1/clusters/{idOrName}/workers/{workerId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/UpdateClusterWorker)</code></td>
</tr>
<tr>
<td>重新載入工作者節點。</td>
<td><code>[ibmcloud ks worker-reload](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)</code></td>
<td><code>[PUT /v1/clusters/{idOrName}/workers/{workerId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/UpdateClusterWorker)</code></td>
</tr>
<tr>
<td>移除工作者節點。</td>
<td><code>[ibmcloud ks worker-rm](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_rm)</code></td>
<td><code>[DELETE /v1/clusters/{idOrName}/workers/{workerId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/RemoveClusterWorker)</code></td>
</tr>
<tr>
<td>更新工作者節點。</td>
<td><code>[ibmcloud ks worker-update](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update)</code></td>
<td><code>[PUT /v1/clusters/{idOrName}/workers/{workerId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/UpdateClusterWorker)</code></td>
</tr>
<tr>
<td>將區域新增至工作者節點儲存區。</td>
<td><code>[ibmcloud ks zone-add](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_zone_add)</code></td>
<td><code>[POST /v1/clusters/{idOrName}/workerpools/{poolidOrName}/zones](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/AddWorkerPoolZone)</code></td>
</tr>
<tr>
<td>更新工作者節點儲存區中給定區域的網路配置。</td>
<td><code>[ibmcloud ks zone-network-set](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_zone_network_set)</code></td>
<td><code>[PATCH /v1/clusters/{idOrName}/workerpools/{poolidOrName}/zones/{zoneid}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/AddWorkerPoolZoneNetwork)</code></td>
</tr>
<tr>
<td>從工作者節點儲存區移除區域。</td>
<td><code>[ibmcloud ks zone-rm](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_zone_rm)</code></td>
<td><code>[DELETE /v1/clusters/{idOrName}/workerpools/{poolidOrName}/zones/{zoneid}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/RemoveWorkerPoolZone)</code></td>
</tr>
</tbody>
</table>

### 管理者動作
{: #admin-actions}

**管理者**平台角色包括**檢視者**、**編輯者**及**操作員**角色授與的所有許可權，以及下列許可權。使用**管理者**角色，叢集或帳戶管理者這類使用者可以建立及刪除叢集，或設定服務端點或受管理附加程式這類全叢集特性。若要建立工作者節點機器、VLAN 及子網路這類基礎架構資源的訂購，「管理者」使用者需要**超級使用者**<a href="#infra">基礎架構角色</a>，或必須使用適當的許可權來設定地區的 API 金鑰。
{: shortdesc}

<table>
<caption>在 {{site.data.keyword.containerlong_notm}} 中需要管理者平台角色的 CLI 指令和 API 呼叫概觀</caption>
<thead>
<th id="admin-mgmt">動作</th>
<th id="admin-cli">CLI 指令</th>
<th id="admin-api">API 呼叫</th>
</thead>
<tbody>
<tr>
<td>測試版：將憑證從 {{site.data.keyword.cloudcerts_long_notm}} 實例部署到 ALB 或更新該憑證。</td>
<td><code>[ibmcloud ks alb-cert-deploy](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_cert_deploy)</code></td>
<td><code>[POST /albsecrets](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/CreateALBSecret) or [PUT /albsecrets](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/UpdateALBSecret)</code></td>
</tr>
<tr>
<td>測試版：檢視有關叢集裡 ALB 密碼的詳細資料。</td>
<td><code>[ibmcloud ks alb-cert-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_cert_get)</code></td>
<td><code>[GET /clusters/{idOrName}/albsecrets](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/ViewClusterALBSecrets)</code></td>
</tr>
<tr>
<td>測試版：從叢集移除 ALB 密碼。</td>
<td><code>[ibmcloud ks alb-cert-rm](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_cert_rm)</code></td>
<td><code>[DELETE /clusters/{idOrName}/albsecrets](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/DeleteClusterALBSecrets)</code></td>
</tr>
<tr>
<td>列出叢集裡的所有 ALB 密碼。</td>
<td><code>[ibmcloud ks alb-certs](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_certs)</code></td>
<td>-</td>
</tr>
<tr>
<td>設定 {{site.data.keyword.Bluemix_notm}} 帳戶的 API 金鑰，以存取鏈結的 IBM Cloud 基礎架構 (SoftLayer) 組合。</td>
<td><code>[ibmcloud ks api-key-reset](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_api_key_reset)</code></td>
<td><code>[POST /v1/keys](https://containers.cloud.ibm.com/global/swagger-global-api/#/accounts/ResetUserAPIKey)</code></td>
</tr>
<tr>
<td>在叢集裡停用受管理的附加程式，例如 Istio 或 Knative。</td>
<td><code>[ibmcloud ks cluster-addon-disable](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_addon_disable)</code></td>
<td><code>[PATCH /v1/clusters/{idOrName}/addons](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/ManageClusterAddons)</code></td>
</tr>
<tr>
<td>在叢集裡啟用受管理的附加程式，例如 Istio 或 Knative。</td>
<td><code>[ibmcloud ks cluster-addon-enable](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_addon_enable)</code></td>
<td><code>[PATCH /v1/clusters/{idOrName}/addons](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/ManageClusterAddons)</code></td>
</tr>
<tr>
<td>列出在叢集裡已啟用的受管理附加程式（例如，Istio 或 Knative）。</td>
<td><code>[ibmcloud ks cluster-addons](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_addons)</code></td>
<td><code>[GET /v1/clusters/{idOrName}/addons](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetClusterAddons)</code></td>
</tr>
<tr>
<td>建立免費或標準叢集。**附註**：也需要 {{site.data.keyword.registrylong_notm}} 的「管理者」平台角色以及「超級使用者」基礎架構角色。</td>
<td><code>[ibmcloud ks cluster-create](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_create)</code></td>
<td><code>[POST /v1/clusters](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/CreateCluster)</code></td>
</tr>
<tr>
<td>停用叢集的指定特性，例如叢集主節點的公用服務端點。</td>
<td><code>[ibmcloud ks cluster-feature-disable](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_disable)</code></td>
<td>-</td>
</tr>
<tr>
<td>啟用叢集的指定特性，例如叢集主節點的專用服務端點。</td>
<td><code>[ibmcloud ks cluster-feature-enable](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable)</code></td>
<td>-</td>
</tr>
<tr>
<td>刪除叢集。</td>
<td><code>[ibmcloud ks cluster-rm](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_rm)</code></td>
<td><code>[DELETE /v1/clusters/{idOrName}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/RemoveCluster)</code></td>
</tr>
<tr>
<td>設定 {{site.data.keyword.Bluemix_notm}} 帳戶的基礎架構認證，以存取不同的 IBM Cloud 基礎架構 (SoftLayer) 組合。</td>
<td><code>[ibmcloud ks credential-set](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_set)</code></td>
<td><code>[POST /v1/credentials](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/accounts/StoreUserCredentials)</code></td>
</tr>
<tr>
<td>移除 {{site.data.keyword.Bluemix_notm}} 帳戶的基礎架構認證，以存取不同的 IBM Cloud 基礎架構 (SoftLayer) 組合。</td>
<td><code>[ibmcloud ks credential-unset](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_unset)</code></td>
<td><code>[DELETE /v1/credentials](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/accounts/RemoveUserCredentials)</code></td>
</tr>
<tr>
<td>測試版：使用 {{site.data.keyword.keymanagementservicefull}} 加密 Kubernetes 密碼。</td>
<td><code>[ibmcloud ks key-protect-enable](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_messages)</code></td>
<td><code>[POST /v1/clusters/{idOrName}/kms](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/CreateKMSConfig)</code></td>
</tr>
<tr>
<td>停用 Fluentd 叢集附加程式的自動更新。</td>
<td><code>[ibmcloud ks logging-autoupdate-disable](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_autoupdate_disable)</code></td>
<td><code>[PUT /v1/logging/{idOrName}/updatepolicy](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/ChangeUpdatePolicy)</code></td>
</tr>
<tr>
<td>啟用 Fluentd 叢集附加程式的自動更新。</td>
<td><code>[ibmcloud ks logging-autoupdate-enable](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_autoupdate_enable)</code></td>
<td><code>[PUT /v1/logging/{idOrName}/updatepolicy](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/ChangeUpdatePolicy)</code></td>
</tr>
<tr>
<td>收集 {{site.data.keyword.cos_full_notm}} 儲存區中 API 伺服器日誌的 Snapshot。</td>
<td><code>[ibmcloud ks logging-collect](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_collect)</code></td>
<td>[POST /v1/log-collector/{idOrName}/masterlogs](https://containers.cloud.ibm.com/global/swagger-global-api/#/log45collector/CreateMasterLogCollection)</td>
</tr>
<tr>
<td>查看 API 伺服器日誌 Snapshot 要求的狀態。</td>
<td><code>[ibmcloud ks logging-collect-status](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_collect_status)</code></td>
<td>[GET /v1/log-collector/{idOrName}/masterlogs](https://containers.cloud.ibm.com/global/swagger-global-api/#/log45collector/GetMasterLogCollectionStatus)</td>
</tr>
<tr>
<td>建立 <code>kube-audit</code> 日誌來源的日誌轉遞配置。</td>
<td><code>[ibmcloud ks logging-config-create](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_logging_create)</code></td>
<td><code>[POST /v1/logging/{idOrName}/loggingconfig/{logSource}](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/CreateLoggingConfig)</code></td>
</tr>
<tr>
<td>刪除 <code>kube-audit</code> 日誌來源的日誌轉遞配置。</td>
<td><code>[ibmcloud ks logging-config-rm](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_logging_rm)</code></td>
<td><code>[DELETE /v1/logging/{idOrName}/loggingconfig/{logSource}/{id}](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/DeleteLoggingConfig)</code></td>
</tr>
</tbody>
</table>

<br />


## {{site.data.keyword.Bluemix_notm}} IAM 服務角色
{: #service}

獲指派 {{site.data.keyword.Bluemix_notm}} IAM 服務存取角色的每一位使用者，也會在特定名稱空間中自動獲指派一個對應的 Kubernetes 角色型存取控制 (RBAC) 角色。若要進一步瞭解服務存取角色，請參閱 [{{site.data.keyword.Bluemix_notm}} IAM 服務角色](/docs/containers?topic=containers-users#platform)。不要同時指派 {{site.data.keyword.Bluemix_notm}} IAM 平台角色與服務角色。您必須個別指派平台角色及服務角色。
{: shortdesc}

想要知道每個服務角色透過 RBAC 授與哪些 Kubernetes 動作？請參閱[每個 RBAC 角色的 Kubernetes 資源許可權](#rbac_ref)。若要進一步瞭解 RBAC 角色的相關資訊，請參閱[指派 RBAC 許可權](/docs/containers?topic=containers-users#role-binding)和[藉由聚集叢集角色延伸現有許可權](https://cloud.ibm.com/docs/containers?topic=containers-users#rbac_aggregate)。
{: tip}

下表顯示了每個服務角色及其對應的 RBAC 角色授權的 Kubernetes 資源許可權。

<table>
<caption>依服務及對應 RBAC 角色的 Kubernetes 資源許可權</caption>
<thead>
    <th id="service-role">服務角色</th>
    <th id="rbac-role">對應的 RBAC 角色、連結及範圍</th>
    <th id="kube-perm">Kubernetes 資源許可權</th>
</thead>
<tbody>
  <tr>
    <td id="service-role-reader" headers="service-role">讀者角色</td>
    <td headers="service-role-reader rbac-role">範圍限定為某個名稱空間時：由該名稱空間中的 <strong><code>ibm-view</code></strong> 角色連結所套用的 <strong><code>view</code></strong> 叢集角色</br><br>範圍限定為所有名稱空間時：由叢集的每個名稱空間中的 <strong><code>ibm-view</code></strong> 角色連結所套用的 <strong><code>view</code></strong> 叢集角色</td>
    <td headers="service-role-reader kube-perm"><ul>
      <li>對名稱空間中的資源的讀取權</li>
      <li>對角色和角色連結或對 Kubernetes 密碼沒有讀取權</li>
      <li>存取 Kubernetes 儀表板以檢視名稱空間中的資源</li></ul>
    </td>
  </tr>
  <tr>
    <td id="service-role-writer" headers="service-role">撰寫者角色</td>
    <td headers="service-role-writer rbac-role">範圍限定為某個名稱空間時：由該名稱空間中的 <strong><code>ibm-edit</code></strong> 角色連結所套用的 <strong><code>edit</code></strong> 叢集角色</br><br>範圍限定為所有名稱空間時：由叢集的每個名稱空間中的 <strong><code>ibm-edit</code></strong> 角色連結所套用的 <strong><code>edit</code></strong> 叢集角色</td>
    <td headers="service-role-writer kube-perm"><ul><li>對名稱空間中資源的讀寫權</li>
    <li>對角色和角色連結沒有讀寫權</li>
    <li>存取 Kubernetes 儀表板以檢視名稱空間中的資源</li></ul>
    </td>
  </tr>
  <tr>
    <td id="service-role-manager" headers="service-role">管理者角色</td>
    <td headers="service-role-manager rbac-role">範圍限定為某個名稱空間時：由該名稱空間中的 <strong><code>ibm-operate</code></strong> 角色連結所套用的 <strong><code>admin</code></strong> 叢集角色</br><br>範圍限定為所有名稱空間時：由套用至所有名稱空間的 <strong><code>ibm-admin</code></strong> 叢集角色連結所套用的 <strong><code>cluster-admin</code></strong> 叢集角色</td>
    <td headers="service-role-manager kube-perm">範圍限定為某個名稱空間時：
      <ul><li>對名稱空間中的所有資源有讀寫權，但對資源配額或名稱空間本身沒有讀寫權</li>
      <li>在名稱空間中建立 RBAC 角色及角色連結</li>
      <li>存取 Kubernetes 儀表板以檢視名稱空間中的所有資源</li></ul>
    </br>範圍限定為所有名稱空間時：
        <ul><li>對每個名稱空間中所有資源的讀寫權</li>
        <li>在一個名稱空間中建立 RBAC 角色及角色連結，或在所有名稱空間中建立叢集角色及叢集角色連結</li>
        <li>存取 Kubernetes 儀表板</li>
        <li>建立 Ingress 資源，讓應用程式成為公開可用</li>
        <li>檢閱叢集度量值，例如使用 <code>kubectl top pods</code>、<code>kubectl top nodes</code> 或 <code>kubectl get nodes</code> 指令</li></ul>
    </td>
  </tr>
</tbody>
</table>

<br />


## 每個 RBAC 角色的 Kubernetes 資源許可權
{: #rbac_ref}

獲指派 {{site.data.keyword.Bluemix_notm}} IAM 服務存取角色的每一位使用者，也會自動獲指派一個對應的 Kubernetes 角色型存取控制 (RBAC) 角色。如果您計劃自行管理自訂 Kubernetes RBAC 角色，請參閱[針對使用者、群組或服務帳戶建立自訂 RBAC 許可權](/docs/containers?topic=containers-users#rbac)。
{: shortdesc}

想知道您是否有正確的許可權，可對名稱空間中的資源執行特定 `kubectl` 指令？請嘗試 [`kubectl auth can-i` 指令 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#-em-can-i-em-)。
{: tip}

下表顯示每個 RBAC 角色所授與針對個別 Kubernetes 資源的許可權。許可權顯示為具有該角色的使用者可以針對資源完成的動詞，例如 "get"、"list"、"describe"、"create" 或 "delete"。

<table>
 <caption>每一個預先定義的 RBAC 角色所授與的 Kubernetes 資源許可權</caption>
 <thead>
  <th>Kubernetes 資源</th>
  <th><code>view</code></th>
  <th><code>edit</code></th>
  <th><code>admin</code> 及 <code>cluster-admin</code></th>
 </thead>
<tbody>
<tr>
  <td><code>bindings</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
</tr><tr>
  <td><code>configmaps</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>cronjobs.batch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>daemonsets.apps </code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>daemonsets.extensions</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>deployments.apps</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>deployments.apps/rollback</code></td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>deployments.apps/scale</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>deployments.extensions</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>deployments.extensions/rollback</code></td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>deployments.extensions/scale</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>endpoints</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>events</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
</tr><tr>
  <td><code>horizontalpodautoscalers.autoscaling</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>ingresses.extensions</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>jobs.batch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>limitranges</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
</tr><tr>
  <td><code>localsubjectaccessreviews</code></td>
  <td>-</td>
  <td>-</td>
  <td><code>create</code></td>
</tr><tr>
  <td><code>namespaces</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></br>**cluster-admin only:** <code>create</code>, <code>delete</code></td>
</tr><tr>
  <td><code>namespaces/status</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
</tr><tr>
  <td><code>networkpolicies</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>networkpolicies.extensions</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>persistentvolumeclaims</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>poddisruptionbudgets.policy</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>pods</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>top</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>pods/attach</code></td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>pods/exec</code></td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>pods/log</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
</tr><tr>
  <td><code>pods/portforward</code></td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>pods/proxy</code></td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>pods/status</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
</tr><tr>
  <td><code>replicasets.apps</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>replicasets.apps/scale</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>replicasets.extensions</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>replicasets.extensions/scale</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>replicationcontrollers</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>replicationcontrollers/scale</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>replicationcontrollers/status</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
</tr><tr>
  <td><code>replicationcontrollers.extensions/scale</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>resourcequotas</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
</tr><tr>
  <td><code>resourcequotas/status</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
</tr><tr>
  <td><code>rolebindings</code></td>
  <td>-</td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>roles</code></td>
  <td>-</td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>secrets</code></td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>serviceaccounts</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code>, <code>impersonate</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code>, <code>impersonate</code></td>
</tr><tr>
  <td><code>services</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>services/proxy</code></td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>statefulsets.apps</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>statefulsets.apps/scale</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr>
</tbody>
</table>

<br />


## Cloud Foundry 角色
{: #cloud-foundry}

Cloud Foundry 角色會授與對帳戶內組織及空間的存取權。若要查看 {{site.data.keyword.Bluemix_notm}} 中的 Cloud Foundry 型服務清單，請執行 `ibmcloud service list`。若要進一步瞭解，請參閱 {{site.data.keyword.Bluemix_notm}} IAM 文件中的所有可用[組織和空間角色](/docs/iam?topic=iam-cfaccess)或[管理 Cloud Foundry 存取權](/docs/iam?topic=iam-mngcf)的步驟。
{: shortdesc}

下表顯示了叢集動作權限所需的 Cloud Foundry 角色。

<table>
  <caption>依 Cloud Foundry 角色的叢集管理許可權</caption>
  <thead>
    <th>Cloud Foundry 角色</th>
    <th>叢集管理許可權</th>
  </thead>
  <tbody>
  <tr>
    <td>空間角色：管理員</td>
    <td>管理對 {{site.data.keyword.Bluemix_notm}} 空間的使用者存取權</td>
  </tr>
  <tr>
    <td>空間角色：開發人員</td>
    <td>
      <ul><li>建立 {{site.data.keyword.Bluemix_notm}} 服務實例</li>
      <li>將 {{site.data.keyword.Bluemix_notm}} 服務實例連結至叢集</li>
      <li>檢視來自空間層次之叢集日誌轉遞配置的日誌</li></ul>
    </td>
  </tr>
  </tbody>
</table>

## 基礎架構角色
{: #infra}

具有**超級使用者**基礎架構存取角色的使用者可[為地區和資源群組設定 API 金鑰](/docs/containers?topic=containers-users#api_key)，以便可以執行基礎架構動作（或者較少見的情況下，[手動設定其他帳戶認證](/docs/containers?topic=containers-users#credentials)）。然後，帳戶中其他使用者可以執行的基礎架構動作將透過 {{site.data.keyword.Bluemix_notm}} IAM 平台角色進行授權。您不需要編輯其他使用者的 IBM Cloud 基礎架構 (SoftLayer) 許可權。僅當無法將**超級使用者**指派給設定 API 金鑰的使用者時，才可以使用下表來自訂使用者的 IBM Cloud 基礎架構 (SoftLayer) 許可權。如需指派許可權的指示，請參閱[自訂基礎架構許可權](/docs/containers?topic=containers-users#infra_access)。
{: shortdesc}



下表會顯示要完成群組一般作業所需的基礎架構許可權。

<table>
<caption>{{site.data.keyword.containerlong_notm}} 一般需要的基礎架構許可權</caption>
<thead>
  <th>{{site.data.keyword.containerlong_notm}} 中的一般作業</th>
  <th>依種類列出的必要基礎架構許可權</th>
</thead>
<tbody>
<tr>
<td>
  <strong>最少許可權</strong>：<ul>
  <li>建立叢集。</li></ul></td>
<td>
<strong>帳戶</strong>：<ul>
<li>新增伺服器</li></ul>
  <strong>裝置</strong>：<ul>
  <li>針對裸機工作者節點：檢視硬體詳細資料</li>
  <li>IPMI 遠端管理</li>
  <li>作業系統重新載入及救援核心</li>
  <li>針對 VM 工作者節點：檢視虛擬伺服器詳細資料</li></ul></td>
</tr>
<tr>
<td>
<strong>叢集管理</strong>：<ul>
  <li>建立、更新及刪除叢集。</li>
  <li>新增、重新載入及重新啟動工作者節點。</li>
  <li>檢視 VLAN。</li>
  <li>建立子網路。</li>
  <li>部署 Pod 及負載平衡器服務。</li></ul>
  </td><td>
<strong>帳戶</strong>：<ul>
  <li>新增伺服器</li>
  <li>取消伺服器</li></ul>
<strong>裝置</strong>：<ul>
  <li>針對裸機工作者節點：檢視硬體詳細資料</li>
  <li>IPMI 遠端管理</li>
  <li>作業系統重新載入及救援核心</li>
  <li>針對 VM 工作者節點：檢視虛擬伺服器詳細資料</li></ul>
<strong>網路</strong>：<ul>
  <li>新增運算與公用網路埠</li></ul>
<p class="important">您還必須為使用者指派管理支援案例的能力。請參閱[ 自訂基礎架構許可權](/docs/containers?topic=containers-users#infra_access)的步驟 8。</p>
</td>
</tr>
<tr>
<td>
  <strong>儲存空間</strong>：<ul>
  <li>建立持續性磁區要求，以佈建持續性磁區。</li>
  <li>建立及管理儲存空間基礎架構資源。</li></ul></td>
<td>
<strong>帳戶</strong>：<ul>
  <li>新增/升級儲存空間 (StorageLayer)</li></ul>
<strong>服務</strong>：<ul>
  <li>儲存空間管理</li></ul></td>
</tr>
<tr>
<td>
  <strong>專用網路</strong>：<ul>
  <li>管理專用 VLAN 來建立叢集內網路連線。</li>
  <li>設定專用網路的 VPN 連線功能。</li></ul></td>
<td>
  <strong>網路</strong>：<ul>
  <li>管理網路子網路路徑</li></ul></td>
</tr>
<tr>
<td>
  <strong>公用網路</strong>：<ul>
  <li>設定公用負載平衡器或 Ingress 網路來公開應用程式。</li></ul></td>
<td>
<strong>裝置</strong>：<ul>
<li>管理埠控制</li>
  <li>編輯主機名稱/網域</li></ul>
<strong>網路</strong>：<ul>
  <li>新增 IP 位址</li>
  <li>管理網路子網路路徑</li>
  <li>新增運算與公用網路埠</li></ul>
<strong>服務</strong>：<ul>
  <li>管理 DNS</li>
  <li>檢視憑證 (SSL)</li>
  <li>管理憑證 (SSL)</li></ul></td>
</tr>
</tbody>
</table>
