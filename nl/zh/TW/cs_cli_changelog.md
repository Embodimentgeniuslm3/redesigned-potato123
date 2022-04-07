---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-05"

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


# CLI 變更日誌
{: #cs_cli_changelog}

在終端機中，有 `ibmcloud` CLI 及外掛程式的更新可用時，就會通知您。請務必保持最新的 CLI，讓您可以使用所有可用的指令及旗標。
{:shortdesc}

若要安裝 {{site.data.keyword.containerlong}} CLI 外掛程式，請參閱[安裝 CLI](/docs/containers?topic=containers-cs_cli_install#cs_cli_install_steps)。

如需每一個 {{site.data.keyword.containerlong_notm}} CLI 外掛程式版本的變更摘要，請參閱下表。

<table summary="針對 {{site.data.keyword.containerlong_notm}} CLI 外掛程式的版本變更概觀">
<caption>{{site.data.keyword.containerlong_notm}} CLI 外掛程式的變更日誌</caption>
<thead>
<tr>
<th>版本</th>
<th>發行日期</th>
<th>變更</th>
</tr>
</thead>
<tbody>
<tr>
<td>0.3.34</td>
<td>2019 年 5 月 31 日</td>
<td>新增了對建立 Red Hat OpenShift on IBM Cloud 叢集的支援。<ul>
<li>新增了對在 `cluster-create` 指令中使用 `--kube-version` 旗標指定 OpenShift 版本的支援。例如，要建立標準 OpenShift 叢集，可以在 `cluster-create` 指令中傳遞 `--kube-version 3.11_openshift`。</li>
<li>新增了 `versions` 指令，用於列出所有支援的 Kubernetes 和 OpenShift 版本。</li>
<li>已淘汰 `kube-versions` 指令。</li>
</ul></td>
</tr>
<tr>
<td>0.3.33</td>
<td>2019 年 5 月 30 日</td>
<td><ul>
<li>向 `cluster-config` 指令新增了 <code>--powershell</code> 旗標，用於擷取 Windows PowerShell 格式的 Kubernetes 環境變數。</li>
<li>已淘汰 `region-get`、`region-set` 和 `regions` 指令。如需相關資訊，請參閱[廣域端點功能](/docs/containers?topic=containers-regions-and-zones#endpoint)。</li>
</ul></td>
</tr>
<tr>
<td>0.3.28</td>
<td>2019 年 5 月 23 日</td>
<td><ul><li>新增了 [<code>ibmcloud ks alb-create</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_create) 指令，用於建立 Ingress ALB。如需相關資訊，請參閱[調整 ALB](/docs/containers?topic=containers-ingress#scale_albs)。</li>
<li>新增了 [<code>ibmcloud ks infra-permissions-get</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#infra_permissions_get) 指令，用於檢查容許[存取 IBM Cloud 基礎架構 (SoftLayer) 組合](/docs/containers?topic=containers-users#api_key)（對於目標資源群組和地區）的認證是否缺少建議或必要的基礎架構許可權。</li>
<li>向 `zone-network-set` 指令新增了 <code>--private-only</code> 旗標，用於取消設定工作者節點儲存區 meta 資料的公用 VLAN，以便該工作者節點儲存區區域中的後續工作者節點僅連接至專用 VLAN。</li>
<li>從 `worker-update` 指令中移除了 <code>--force-update</code> 旗標。</li>
<li>向 `albs` 和 `alb-get` 指令的輸出新增了 **VLAN ID** 直欄。</li>
<li>向 `supported-locations` 指令的輸出新增了**多區域都會**直欄，用於指明區域是否具有多區域功能。</li>
<li>向 `cluster-get` 指令的輸出新增了**主節點狀態**和**主節點性能**欄位。如需相關資訊，請參閱[主節點狀態](/docs/containers?topic=containers-health#states_master)。</li>
<li>更新說明文字的翻譯。</li>
</ul></td>
</tr>
<tr>
<td>0.3.8</td>
<td>2019 年 4 月 30 日</td>
<td>在 `0.3` 版中，新增了對[廣域端點功能](/docs/containers?topic=containers-regions-and-zones#endpoint)的支援。依預設，現在可以檢視和管理所有位置中的所有 {{site.data.keyword.containerlong_notm}} 資源。您無需將地區設定為目標就可使用資源。</li>
<ul><li>新增了 [<code>ibmcloud ks supported-locations</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_supported-locations) 指令，用於列出 {{site.data.keyword.containerlong_notm}} 支援的所有位置。</li>
<li>向 `clusters` 和 `zones` 指令新增了 <code>--locations</code> 旗標，用於按一個以上位置過濾資源。</li>
<li>向 `credential-set/unset/get`、`api-key-reset` 和 `vlan-spanning-get` 指令新增了 <code>--region</code> 旗標。若要執行這些指令，必須在 `--region` 旗標中指定地區。</li></ul></td>
</tr>
<tr>
<td>0.2.102</td>
<td>2019 年 4 月 15 日</td>
<td>新增 [`ibmcloud ks nlb-dns` 指令群組](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#nlb-dns)來登錄及管理網路負載平衡器 (NLB) IP 位址的主機名稱，以及新增 [`ibmcloud ks nlb-dns-monitor` 指令群組](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-monitor)來建立及修改 NLB 主機名稱的性能檢查監視器。如需相關資訊，請參閱[使用 DNS 主機名稱登錄 NLB IP](/docs/containers?topic=containers-loadbalancer#loadbalancer_hostname_dns)。
</td>
</tr>
<tr>
<td>0.2.99</td>
<td>2019 年 4 月 9 日</td>
<td><ul>
<li>更新說明文字。</li>
<li>將 Go 版本更新為 1.12.2。</li>
</ul></td>
</tr>
<tr>
<td>0.2.95</td>
<td>2019 年 4 月 3 日</td>
<td><ul>
<li>新增受管理叢集附加程式的版本化支援。</li>
<ul><li>新增 [<code>ibmcloud ks addon-versions</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_addon_versions) 指令。</li>
<li>將 <code>--version</code> 旗標新增至 [ibmcloud ks cluster-addon-enable](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_addon_enable) 指令。</li></ul>
<li>更新說明文字的翻譯。</li>
<li>更新說明文字中文件的簡短鏈結。</li>
<li>修正以不正確格式列印 JSON 錯誤訊息的錯誤。</li>
<li>修正在部分指令上使用 silent 旗標 (`-s`) 以防止列印錯誤的錯誤。</li>
</ul></td>
</tr>
<tr>
<td>0.2.80</td>
<td>2019 年 3 月 19 日</td>
<td><ul>
<li>在[已啟用 VRF 的帳戶](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started)中新增支援，能夠在標準叢集（執行 Kubernetes 1.11 版或更新版本）中[與服務端點進行主節點到工作者節點的通訊](/docs/containers?topic=containers-plan_clusters#workeruser-master)。<ul>
<li>將 `--private-service-endpoint` 和 `--public-service-endpoint` 旗標新增至 [<code>ibmcloud s cluster-create</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_create) 指令。</li>
<li>將**公用服務端點 URL** 和**專用服務端點 URL** 欄位新增至 <code>ibmcloud ks cluster-get</code> 的輸出中。</li>
<li>新增 [<code>ibmcloud ks cluster-feature-enable private-service-endpoint</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable_private_service_endpoint) 指令。</li>
<li>新增 [<code>ibmcloud ks cluster-feature-enable public-service-endpoint</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable_public_service_endpoint) 指令。</li>
<li>新增 [<code>ibmcloud ks cluster-feature-disable public-service-endpoint</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_disable) 指令。</li>
</ul></li>
<li>更新文件和翻譯。</li>
<li>將 Go 版本更新為 1.11.6。</li>
<li>解決 macOS 使用者的間歇性網路問題。</li>
</ul></td>
</tr>
<tr>
<td>0.2.75</td>
<td>2019 年 3 月 14 日</td>
<td><ul><li>在錯誤輸出中隱藏原始 HTML。</li>
<li>修正說明文字中的錯字。</li>
<li>修正說明文字的翻譯。</li>
</ul></td>
</tr>
<tr>
<td>0.2.61</td>
<td>2019 年 2 月 26 日</td>
<td><ul>
<li>新增 `cluster-pull-secret-apply` 指令，這會建立叢集、原則、API 金鑰及映像檔取回密碼的 IAM 服務 ID，以便在 `default` Kubernetes 名稱空間中執行的容器可以從 IBM Cloud Container Registry 取回映像檔。對於新的叢集，依預設，會建立使用 IAM 認證的映像檔取回密碼。不論是現有的叢集，或者，如果您的叢集在建立期間發生映像檔取回密碼錯誤，都可以使用這個指令來更新叢集。如需相關資訊，請參閱[文件](https://test.cloud.ibm.com/docs/containers?topic=containers-images#cluster_registry_auth)。</li>
<li>修正錯誤，其中 `ibmcloud ks init` 失敗導致將列印說明輸出。</li>
</ul></td>
</tr>
<tr>
<td>0.2.53</td>
<td>2019 年 2 月 19 日</td>
<td><ul><li>修正 `ibmcloud ks api-key-reset`、`ibmcloud ks credential-get/set` 及 `ibmcloud ks vlan-spanning-get` 忽略地區的錯誤。</li>
<li>改善 `ibmcloud ks worker-update` 的效能。</li>
<li>在 `ibmcloud ks cluster-addon-enable` 提示中，新增附加程式的版本。</li>
</ul></td>
</tr>
<tr>
<td>0.2.44</td>
<td>2019 年 2 月 08 日</td>
<td><ul>
<li>將 `--skip-rbac` 選項新增至 `ibmcloud ks cluster-config` 指令，以跳過根據 {{site.data.keyword.Bluemix_notm}} IAM 服務存取角色將使用者 Kubernetes RBAC 角色新增至叢集配置。唯有當您[管理自己的 Kubernetes RBAC 角色](/docs/containers?topic=containers-users#rbac)時，才要包含此選項。如果您使用 [{{site.data.keyword.Bluemix_notm}} IAM 服務存取角色](/docs/containers?topic=containers-access_reference#service)來管理所有 RBAC 使用者，請勿包含此選項。</li>
<li>將 Go 版本更新為 1.11.5。</li>
</ul></td>
</tr>
<tr>
<td>0.2.40</td>
<td>2019 年 2 月 6 日</td>
<td><ul>
<li>新增 [<code>ibmcloud ks cluster-addons</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_addons)、[<code>ibmcloud ks cluster-addon-enable</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_addon_enable) 及 [<code>ibmcloud ks cluster-addon-disable</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_addon_disable) 指令，以使用 {{site.data.keyword.containerlong_notm}} 的受管理叢集附加程式，例如 [Istio](/docs/containers?topic=containers-istio) 和 [Knative](/docs/containers?topic=containers-serverless-apps-knative) 管理的附加程式。</li>
<li>改善 <code>ibmcloud ks vlans</code> 指令的 {{site.data.keyword.Bluemix_dedicated_notm}} 使用者的說明文字。</li></ul></td>
</tr>
<tr>
<td>0.2.30</td>
<td>2019 年 1 月 31 日</td>
<td>將 `ibmcloud ks cluster-config` 的預設逾時值增加至 `500s`。</td>
</tr>
<tr>
<td>0.2.19</td>
<td>2019 年 1 月 16 日</td>
<td><ul><li>新增 `IKS_BETA_VERSION` 環境變數，以啟用重新設計的 {{site.data.keyword.containerlong_notm}} 外掛程式 CLI 測試版。若要試用重新設計的版本，請參閱[使用測試版指令結構](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_beta)。</li>
<li>將 `ibmcloud ks subnets` 的預設逾時值增加至 `60s`。</li>
<li>次要錯誤和翻譯修正。</li></ul></td>
</tr>
<tr>
<td>0.1.668</td>
<td>2018 年 12 月 18 日</td>
<td><ul><li>將預設 API 端點從 <code>https://containers.bluemix.net</code> 變更為 <code>https://containers.cloud.ibm.com</code>。</li>
<li>修正錯誤，使指令說明和錯誤訊息的翻譯能夠正確顯示。</li>
<li>更快顯示指令說明。</li></ul></td>
</tr>
<tr>
<td>0.1.654</td>
<td>2018 年 12 月 5 日</td>
<td>更新文件和翻譯。</td>
</tr>
<tr>
<td>0.1.638</td>
<td>2018 年 11 月 15 日</td>
<td>
<ul><li>向 `apiserver-refresh` 指令新增了 [<code>ibmcloud ks cluster-refresh</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_apiserver_refresh) 別名。</li>
<li>將資源群組名稱新增至 <code>ibmcloud ks cluster-get</code> 和 <code>ibmcloud ks clusters</code> 的輸出。</li></ul>
</td>
</tr>
<tr>
<td>0.1.635</td>
<td>2018 年 11 月 6 日</td>
<td>新增指令來管理 Ingress ALB 叢集附加程式的自動更新：<ul>
<li>[<code>ibmcloud ks alb-autoupdate-disable</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_autoupdate_disable)</li>
<li>[<code>ibmcloud ks alb-autoupdate-enable</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_autoupdate_enable)</li>
<li>[<code>ibmcloud ks alb-autoupdate-get</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_autoupdate_get)</li>
<li>[<code>ibmcloud ks alb-rollback</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_rollback)</li>
<li>[<code>ibmcloud ks alb-update</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_update)</li>
</ul></td>
</tr>
<tr>
<td>0.1.621</td>
<td>2018 年 10 月 30 日</td>
<td><ul>
<li>新增 [<code>ibmcloud ks credential-get</code> 指令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credential_get)。</li>
<li>將 <code>storage</code> 日誌來源的支援新增至所有叢集記載指令。如需相關資訊，請參閱<a href="/docs/containers?topic=containers-health#logging">瞭解叢集和應用程式日誌轉遞</a>。</li>
<li>將 `--network` 旗標新增至 [<code>ibmcloud k cluster-config</code> 指令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_config)，其會下載 Calico 配置檔以執行所有 Calico 指令。</li>
<li>次要錯誤修正程式及重構</li></ul>
</td>
</tr>
<tr>
<td>0.1.593</td>
<td>2018 年 10 月 10 日</td>
<td><ul><li>將資源群組 ID 新增至 <code>ibmcloud s cluster-get</code> 的輸出。</li>
<li>已啟用 [{{site.data.keyword.keymanagementserviceshort}}](/docs/containers?topic=containers-encryption#keyprotect) 作為叢集裡的金鑰管理服務 (KMS) 提供者時，會在 <code>ibmcloud s cluster-get</code> 的輸出中新增已啟用 KMS 的欄位。</li></ul></td>
</tr>
<tr>
<td>0.1.591</td>
<td>2018 年 10 月 2 日</td>
<td>新增[資源群組的支援](/docs/containers?topic=containers-clusters#cluster_prepare)。</td>
</tr>
<tr>
<td>0.1.590</td>
<td>2018 年 10 月 1 日</td>
<td><ul>
<li>新增 [<code>ibmcloud ks logging-collect</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_collect) 及 [<code>ibmcloud ks logging-collect-status</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_collect_status) 指令，用於收集叢集裡的 API 伺服器日誌。</li>
<li>新增 [<code>ibmcloud ks key-protect-enable</code> 指令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_key_protect)，以啟用 {{site.data.keyword.keymanagementserviceshort}} 作為叢集裡的金鑰管理服務 (KMS) 提供者。</li>
<li>在起始重新開機或重新載入之前，將 <code>--skip-master-health</code> 旗標新增至 [ibmcloud ks worker-reboot](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reboot) 及 [ibmcloud ks worker-reload](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reboot) 指令，以跳過主節點性能檢查。</li>
<li>在 <code>ibmcloud ks cluster-get</code> 的輸出中，將 <code>Owner Email</code> 重新命名為 <code>Owner</code>。</li></ul></td>
</tr>
</tbody>
</table>
