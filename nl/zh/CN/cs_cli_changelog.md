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


# CLI 更改日志
{: #cs_cli_changelog}

在终端中，当有 `ibmcloud` CLI 和插件的更新可用时，您会收到通知。务必使 CLI 保持最新，以便您可以使用所有可用的命令和标志。
{:shortdesc}

要安装 {{site.data.keyword.containerlong}} CLI 插件，请参阅[安装 CLI](/docs/containers?topic=containers-cs_cli_install#cs_cli_install_steps)。

请参阅下表以获取每个 {{site.data.keyword.containerlong_notm}} CLI 插件版本的更改摘要。

<table summary="{{site.data.keyword.containerlong_notm}} CLI 插件的版本更改概述">
<caption>{{site.data.keyword.containerlong_notm}} CLI 插件的更改日志</caption>
<thead>
<tr>
<th>版本</th>
<th>发布日期</th>
<th>更改</th>
</tr>
</thead>
<tbody>
<tr>
<td>0.3.34</td>
<td>2019 年 5 月 31 日</td>
<td>添加了对创建 Red Hat OpenShift on IBM Cloud 集群的支持。<ul>
<li>添加了对在 `cluster-create` 命令中使用 `--kube-version` 标志指定 OpenShift 版本的支持。例如，要创建标准 OpenShift 集群，可以在 `cluster-create` 命令中传递 `--kube-version 3.11_openshift`。</li>
<li>添加了 `versions` 命令，用于列出所有支持的 Kubernetes 和 OpenShift 版本。</li>
<li>不推荐使用 `kube-versions` 命令。</li>
</ul></td>
</tr>
<tr>
<td>0.3.33</td>
<td>2019 年 5 月 30 日</td>
<td><ul>
<li>向 `cluster-config` 命令添加了 <code>--powershell</code> 标志，用于检索 Windows PowerShell 格式的 Kubernetes 环境变量。</li>
<li>不推荐使用 `region-get`、`region-set` 和 `regions` 命令。有关更多信息，请参阅[全局端点功能](/docs/containers?topic=containers-regions-and-zones#endpoint)。</li>
</ul></td>
</tr>
<tr>
<td>0.3.28</td>
<td>2019 年 5 月 23 日</td>
<td><ul><li>添加了 [<code>ibmcloud ks alb-create</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_create) 命令，用于创建 Ingress ALB。有关更多信息，请参阅[缩放 ALB](/docs/containers?topic=containers-ingress#scale_albs)。</li>
<li>添加了 [<code>ibmcloud ks infra-permissions-get</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#infra_permissions_get) 命令，用于检查允许[访问 IBM Cloud Infrastructure (SoftLayer) 产品服务组合](/docs/containers?topic=containers-users#api_key)（对于目标资源组和区域）的凭证是否缺少建议或必需的基础架构许可权。</li>
<li>向 `zone-network-set` 命令添加了 <code>--private-only</code> 标志，用于取消设置工作程序池元数据的公用 VLAN，以便该工作程序池专区中的后续工作程序节点仅连接到专用 VLAN。</li>
<li>从 `worker-update` 命令中除去了 <code>--force-update</code> 标志。</li>
<li>向 `albs` 和 `alb-get` 命令的输出添加了 **VLAN ID** 列。</li>
<li>向 `supported-locations` 命令的输出添加了 **Multizone Metro** 列，用于指明专区是否支持多专区。</li>
<li>向 `cluster-get` 命令的输出添加了 **Master State** 和 **Master Health** 字段。有关更多信息，请参阅[主节点状态](/docs/containers?topic=containers-health#states_master)。</li>
<li>更新了帮助文本的翻译。</li>
</ul></td>
</tr>
<tr>
<td>0.3.8</td>
<td>2019 年 4 月 30 日</td>
<td>在 V`0.3` 中，添加了对[全局端点功能](/docs/containers?topic=containers-regions-and-zones#endpoint)的支持。缺省情况下，现在可以查看和管理所有位置中的所有 {{site.data.keyword.containerlong_notm}} 资源。您无需将区域设定为目标就可使用资源。</li>
<ul><li>添加了 [<code>ibmcloud ks supported-locations</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_supported-locations) 命令，用于列出 {{site.data.keyword.containerlong_notm}} 支持的所有位置。</li>
<li>向 `clusters` 和 `zones` 命令添加了 <code>--locations</code> 标志，用于按一个或多个位置过滤资源。</li>
<li>向 `credential-set/unset/get`、`api-key-reset` 和 `vlan-spanning-get` 命令添加了 <code>--region</code> 标志。要运行这些命令，必须在 `--region` 标志中指定区域。</li></ul></td>
</tr>
<tr>
<td>0.2.102</td>
<td>2019 年 4 月 15 日</td>
<td>添加了 [`ibmcloud ks nlb-dns` 命令组](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#nlb-dns)（用于注册和管理网络负载均衡器 (NLB) IP 地址的主机名）和 [`ibmcloud ks nlb-dns-monitor` 命令组](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-monitor)（用于创建和修改 NLB 主机名的运行状况检查监视器）。有关更多信息，请参阅[向 DNS 主机名注册 NLB IP](/docs/containers?topic=containers-loadbalancer#loadbalancer_hostname_dns)。</td>
</tr>
<tr>
<td>0.2.99</td>
<td>2019 年 4 月 9 日</td>
<td><ul>
<li>更新了帮助文本。</li>
<li>将 Go 版本更新为 1.12.2。</li>
</ul></td>
</tr>
<tr>
<td>0.2.95</td>
<td>2019 年 4 月 3 日</td>
<td><ul>
<li>添加了对受管集群附加组件的版本控制支持。</li>
<ul><li>添加了 [<code>ibmcloud ks addon-versions</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_addon_versions) 命令。</li>
<li>向 [ibmcloud ks cluster-addon-enable](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_addon_enable) 命令添加了 <code>--version</code> 标志。</li></ul>
<li>更新了帮助文本的翻译。</li>
<li>更新了帮助文本中文档的短链接。</li>
<li>修复了以不正确的格式显示 JSON 错误消息的错误。</li>
<li>修复了在某些命令上使用静默标志 (`-s`) 会阻止显示错误的错误。</li>
</ul></td>
</tr>
<tr>
<td>0.2.80</td>
<td>2019 年 3 月 19 日</td>
<td><ul>
<li>在[启用 VRF 的帐户](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started)中运行 Kubernetes V1.11 或更高版本的标准集群中，添加了对启用[通过服务端点进行的主节点与工作程序之间通信](/docs/containers?topic=containers-plan_clusters#workeruser-master)的支持。<ul>
<li>向 [<code>ibmcloud ks cluster-create</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_create) 命令添加了 `--private-service-endpoint` 和 `--public-service-endpoint` 标志。</li>
<li>向 <code>ibmcloud ks cluster-get</code> 的输出添加了 **Public Service Endpoint URL** 和 **Private Service Endpoint URL** 字段。</li>
<li>添加了 [<code>ibmcloud ks cluster-feature-enable private-service-endpoint</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable_private_service_endpoint) 命令。</li>
<li>添加了 [<code>ibmcloud ks cluster-feature-enable public-service-endpoint</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable_public_service_endpoint) 命令。</li>
<li>添加了 [<code>ibmcloud ks cluster-feature-disable public-service-endpoint</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_disable) 命令。</li>
</ul></li>
<li>更新了文档和翻译。</li>
<li>将 Go 版本更新为 1.11.6。</li>
<li>解决了 macOS 用户的间歇性联网问题。</li>
</ul></td>
</tr>
<tr>
<td>0.2.75</td>
<td>2019 年 3 月 14 日</td>
<td><ul><li>隐藏了错误输出中的原始 HTML。</li>
<li>修订了帮助文本中的输入错误。</li>
<li>修订了帮助文本的翻译。</li>
</ul></td>
</tr>
<tr>
<td>0.2.61</td>
<td>2019 年 2 月 26 日</td>
<td><ul>
<li>添加了 `cluster-pull-secret-apply` 命令，此命令用于创建集群的 IAM 服务标识、策略、API 密钥和映像拉取私钥，以便在 `default` Kubernetes 名称空间中运行的容器可以从 IBM Cloud Container Registry 中拉取映像。对于新集群，缺省情况下会创建使用 IAM 凭证的映像拉取私钥。要更新现有集群，或者如果集群在创建期间发生映像拉取私钥错误，可使用此命令。有关更多信息，请参阅[文档](https://test.cloud.ibm.com/docs/containers?topic=containers-images#cluster_registry_auth)。</li>
<li>修订了 `ibmcloud ks init` 失败导致打印帮助输出的错误。</li>
</ul></td>
</tr>
<tr>
<td>0.2.53</td>
<td>2019 年 2 月 19 日</td>
<td><ul><li>修正了针对 `ibmcloud ks api-key-reset`、`ibmcloud ks credential-get/set` 和 `ibmcloud ks vlan-spanning-get` 忽略区域的错误。</li>
<li>提高了 `ibmcloud ks worker-update` 的性能。</li>
<li>在 `ibmcloud ks cluster-addon-enable` 提示中添加了附加组件的版本。</li>
</ul></td>
</tr>
<tr>
<td>0.2.44</td>
<td>2019 年 2 月 8 日</td>
<td><ul>
<li>向 `ibmcloud ks cluster-config` 命令添加了 `--skip-rbac` 选项，用于跳过将基于 {{site.data.keyword.Bluemix_notm}} IAM 服务访问角色的用户 Kubernetes RBAC 角色添加到集群配置的操作。仅当您[管理自己的 Kubernetes RBAC 角色](/docs/containers?topic=containers-users#rbac)时，才包含此选项。如果是使用 [{{site.data.keyword.Bluemix_notm}} IAM 服务访问角色](/docs/containers?topic=containers-access_reference#service)来管理所有 RBAC 用户，请不要包含此选项。</li>
<li>将 Go 版本更新为 1.11.5。</li>
</ul></td>
</tr>
<tr>
<td>0.2.40</td>
<td>2019 年 2 月 6 日</td>
<td><ul>
<li>添加了 [<code>ibmcloud ks cluster-addons</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_addons)、[<code>ibmcloud ks cluster-addon-enable</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_addon_enable) 和 [<code>ibmcloud ks cluster-addon-disable</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_addon_disable) 命令，以使用受管集群附加组件，例如 {{site.data.keyword.containerlong_notm}} 的 [Istio](/docs/containers?topic=containers-istio) 和 [Knative](/docs/containers?topic=containers-serverless-apps-knative) 受管附加组件。</li>
<li>改进了向 {{site.data.keyword.Bluemix_dedicated_notm}} 用户提供的 <code>ibmcloud ks vlans</code> 命令的帮助文本。</li></ul></td>
</tr>
<tr>
<td>0.2.30</td>
<td>2019 年 1 月 31 日</td>
<td>将 `ibmcloud ks cluster-config` 的缺省超时值增加到了 `500s`。</td>
</tr>
<tr>
<td>0.2.19</td>
<td>2019 年 1 月 16 日</td>
<td><ul><li>添加了 `IKS_BETA_VERSION` 环境变量以启用 {{site.data.keyword.containerlong_notm}} 插件 CLI 的重新设计的 Beta 版本。要试用重新设计的版本，请参阅[使用 Beta 命令结构](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_beta)。</li>
<li>将 `ibmcloud ks subnets` 的缺省超时值增加到了 `60s`。</li>
<li>次要错误和翻译修订。</li></ul></td>
</tr>
<tr>
<td>0.1.668</td>
<td>2018 年 12 月 18 日</td>
<td><ul><li>将缺省 API 端点从 <code>https://containers.bluemix.net</code> 更改为 <code>https://containers.cloud.ibm.com</code>。</li>
<li>修订了错误，以便能正确显示命令帮助和错误消息的翻译。</li>
<li>更快地显示命令帮助。</li></ul></td>
</tr>
<tr>
<td>0.1.654</td>
<td>2018 年 12 月 5 日</td>
<td>更新了文档和翻译。</td>
</tr>
<tr>
<td>0.1.638</td>
<td>2018 年 11 月 15 日</td>
<td>
<ul><li>向 `apiserver-refresh` 命令添加了 [<code>ibmcloud ks cluster-refresh</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_apiserver_refresh) 别名。</li>
<li>向 <code>ibmcloud ks cluster-get</code> 和 <code>ibmcloud ks clusters</code> 的输出添加了资源组名称。</li></ul>
</td>
</tr>
<tr>
<td>0.1.635</td>
<td>2018 年 11 月 6 日</td>
<td>添加了用于管理 Ingress ALB 集群附加组件自动更新的命令：<ul>
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
<li>添加了 [<code>ibmcloud ks credential-get</code> 命令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credential_get)。</li>
<li>添加了对 <code>storage</code> 日志源用于所有集群日志记录命令的支持。有关更多信息，请参阅<a href="/docs/containers?topic=containers-health#logging">了解集群和应用程序日志转发</a>。</li>
<li>向 [<code>ibmcloud ks cluster-config</code> 命令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_config)添加了 `--network` 标志，用于下载 Calico 配置文件以运行所有 Calico 命令。</li>
<li>小幅错误修订和重构</li></ul>
</td>
</tr>
<tr>
<td>0.1.593</td>
<td>2018 年 10 月 10 日</td>
<td><ul><li>向 <code>ibmcloud ks cluster-get</code> 的输出添加了资源组标识。</li>
<li>[启用 {{site.data.keyword.keymanagementserviceshort}}](/docs/containers?topic=containers-encryption#keyprotect) 作为集群中的密钥管理服务 (KMS) 提供程序时，在 <code>ibmcloud ks cluster-get</code> 的输出中添加了“KMS 已启用”字段。</li></ul></td>
</tr>
<tr>
<td>0.1.591</td>
<td>2018 年 10 月 2 日</td>
<td>添加了对[资源组](/docs/containers?topic=containers-clusters#cluster_prepare)的支持。</td>
</tr>
<tr>
<td>0.1.590</td>
<td>2018 年 10 月 1 日</td>
<td><ul>
<li>添加了 [<code>ibmcloud ks logging-collect</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_collect) 和 [<code>ibmcloud ks logging-collect-status</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_collect_status) 命令，用于收集集群中的 API 服务器日志。</li>
<li>添加了 [<code>ibmcloud ks key-protect-enable</code> 命令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_key_protect)，用于启用 {{site.data.keyword.keymanagementserviceshort}} 作为集群中的密钥管理服务 (KMS) 提供程序。</li>
<li>向 [ibmcloud ks worker-reboot](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reboot) 和 [ibmcloud ks worker-reload](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reboot) 命令添加了 <code>--skip-master-health</code> 标志，用于跳过启动重新引导或重新装入之前的主节点运行状况检查。</li>
<li>将 <code>ibmcloud ks cluster-get</code> 输出中的 <code>Owner Email</code> 重命名为 <code>Owner</code>。</li></ul></td>
</tr>
</tbody>
</table>
