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



# 用户访问许可权
{: #access_reference}

[分配集群许可权](/docs/containers?topic=containers-users)时，很难判断需要将哪个角色分配给用户。请使用以下各部分中的表来确定在 {{site.data.keyword.containerlong}} 中执行常见任务所需的最低许可权级别。
{: shortdesc}

自 2019 年 1 月 30 日开始，{{site.data.keyword.containerlong_notm}} 采用新的方式通过 {{site.data.keyword.Bluemix_notm}} IAM：[服务访问角色](#service)来对用户授权。这些服务角色用于授予对集群中资源（例如，Kubernetes 名称空间）的访问权。有关更多信息，请查看博客 [Introducing service roles and namespaces in IAM for more granular control of cluster access ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/blogs/bluemix/2019/02/introducing-service-roles-and-namespaces-in-iam-for-more-granular-control-of-cluster-access/)。
{: note}

## {{site.data.keyword.Bluemix_notm}} IAM 平台角色
{: #iam_platform}

{{site.data.keyword.containerlong_notm}} 已配置为使用 {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM) 角色。{{site.data.keyword.Bluemix_notm}} IAM 平台角色确定用户可对集群、工作程序节点和 Ingress 应用程序负载均衡器 (ALB) 等 {{site.data.keyword.Bluemix_notm}} 资源执行的操作。此外，{{site.data.keyword.Bluemix_notm}} IAM 平台角色还会自动为用户设置基本基础架构许可权。要设置平台角色，请参阅[分配 {{site.data.keyword.Bluemix_notm}} IAM 平台许可权](/docs/containers?topic=containers-users#platform)。
{: shortdesc}

<p class="tip">不要在分配 {{site.data.keyword.Bluemix_notm}} IAM 平台角色的同时分配服务角色。您必须单独分配平台角色和服务角色。</p>

以下每个部分中的各表显示了每个 {{site.data.keyword.Bluemix_notm}} IAM 平台角色授予的集群管理、日志记录和 Ingress 许可权。这些表按 CLI 命令名称以字母顺序进行组织。

* [无需许可权的操作](#none-actions)
* [查看者操作](#view-actions)
* [编辑者操作](#editor-actions)
* [操作员操作](#operator-actions)
* [管理员操作](#admin-actions)

### 无需许可权的操作
{: #none-actions}

您帐户中运行 CLI 命令或对下表中的操作发出 API 调用的任何用户都会看到相应结果，即使没有为该用户分配任何许可权也是如此。
{: shortdesc}

<table>
<caption>在 {{site.data.keyword.containerlong_notm}} 中无需许可权的 CLI 命令和 API 调用概述</caption>
<thead>
<th id="none-actions-action">操作</th>
<th id="none-actions-cli">CLI 命令</th>
<th id="none-actions-api">API 调用</th>
</thead>
<tbody>
<tr>
<td>查看 {{site.data.keyword.containerlong_notm}} 中支持的受管附加组件版本的列表。</td>
<td><code>[ibmcloud ks addon-versions](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_addon_versions)</code></td>
<td><code>[GET /v1/addon](https://containers.cloud.ibm.com/global/swagger-global-api/#/util/GetAddons)</code></td>
</tr>
<tr>
<td>查看 {{site.data.keyword.containerlong_notm}} 的 API 端点或将其设定为目标。</td>
<td><code>[ibmcloud ks api](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cli_api)</code></td>
<td>-</td>
</tr>
<tr>
<td>查看支持的命令和参数的列表。</td>
<td><code>[ibmcloud ks help](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_help)</code></td>
<td>-</td>
</tr>
<tr>
<td>初始化 {{site.data.keyword.containerlong_notm}} 插件或指定要在其中创建或访问 Kubernetes 集群的区域。</td>
<td><code>[ibmcloud ks init](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_init)</code></td>
<td>-</td>
</tr>
<tr>
<td>不推荐：查看 {{site.data.keyword.containerlong_notm}} 中支持的 Kubernetes 版本的列表。</td>
<td><code>[ibmcloud ks kube-versions](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_kube_versions)</code></td>
<td><code>[GET /v1/kube-versions](https://containers.cloud.ibm.com/global/swagger-global-api/#/util/GetKubeVersions)</code></td>
</tr>
<tr>
<td>查看可用于工作程序节点的机器类型的列表。</td>
<td><code>[ibmcloud ks machine-types](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_machine_types)</code></td>
<td><code>[GET /v1/datacenters/{datacenter}/machine-types](https://containers.cloud.ibm.com/global/swagger-global-api/#/util/GetDatacenterMachineTypes)</code></td>
</tr>
<tr>
<td>查看 IBM 标识用户的当前消息。</td>
<td><code>[ibmcloud ks messages](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_messages)</code></td>
<td><code>[GET /v1/messages](https://containers.cloud.ibm.com/global/swagger-global-api/#/util/GetMessages)</code></td>
</tr>
<tr>
<td>不推荐：查找您当前所在的 {{site.data.keyword.containerlong_notm}} 区域。</td>
<td><code>[ibmcloud ks region](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_region)</code></td>
<td>-</td>
</tr>
<tr>
<td>不推荐：设置 {{site.data.keyword.containerlong_notm}} 的区域。</td>
<td><code>[ibmcloud ks region-set](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_region-set)</code></td>
<td>-</td>
</tr>
<tr>
<td>不推荐：列出可用的区域。</td>
<td><code>[ibmcloud ks regions](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_regions)</code></td>
<td><code>[GET /v1/regions](https://containers.cloud.ibm.com/global/swagger-global-api/#/util/GetRegions)</code></td>
</tr>
<tr>
<td>查看 {{site.data.keyword.containerlong_notm}} 中支持的位置的列表。</td>
<td><code>[ibmcloud ks supported-locations](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_supported-locations)</code></td>
<td><code>[GET /v1/locations](https://containers.cloud.ibm.com/global/swagger-global-api/#/util/ListLocations)</code></td>
</tr>
<tr>
<td>查看 {{site.data.keyword.containerlong_notm}} 中支持的版本的列表。</td>
<td><code>[ibmcloud ks versions](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_versions)</code></td>
<td>-</td>
</tr>
<tr>
<td>查看可在其中创建集群的可用专区的列表。</td>
<td><code>[ibmcloud ks zones](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_datacenters)</code></td>
<td><code>[GET /v1/zones](https://containers.cloud.ibm.com/global/swagger-global-api/#/util/GetZones)</code></td>
</tr>
</tbody>
</table>

### 查看者操作
{: #view-actions}

**查看者**平台角色包含[无需许可权的操作](#none-actions)以及下表中显示的许可权。
具有**查看者**角色（例如，审计员或计费）的用户可以查看集群详细信息，但无法修改基础架构。
{: shortdesc}

<table>
<caption>在 {{site.data.keyword.containerlong_notm}} 中需要查看者平台角色的 CLI 命令和 API 调用概述</caption>
<thead>
<th id="view-actions-mngt">操作</th>
<th id="view-actions-cli">CLI 命令</th>
<th id="view-actions-api">API 调用</th>
</thead>
<tbody>
<tr>
<td>查看 Ingress ALB 的信息。</td>
<td><code>[ibmcloud ks alb-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_get)</code></td>
<td><code>[GET /albs/{albId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/GetClusterALB)</code></td>
</tr>
<tr>
<td>查看区域中支持的 ALB 类型。</td>
<td><code>[ibmcloud ks alb-types](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_types)</code></td>
<td><code>[GET /albtypes](https://containers.cloud.ibm.com/global/swagger-global-api/#/util/GetAvailableALBTypes)</code></td>
</tr>
<tr>
<td>列出一个集群中的所有 Ingress ALB。</td>
<td><code>[ibmcloud ks albs](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_albs)</code></td>
<td><code>[GET /clusters/{idOrName}](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/GetClusterALBs)</code></td>
</tr>
<tr>
<td>查看资源组和区域的 {{site.data.keyword.Bluemix_notm}} IAM API 密钥所有者的姓名和电子邮件地址。</td>
<td><code>[ibmcloud ks api-key-info](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_api_key_info)</code></td>
<td><code>[GET /v1/logging/{idOrName}/clusterkeyowner](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/GetClusterKeyOwner)</code></td>
</tr>
<tr>
<td>下载 Kubernetes 配置数据和证书，以连接到集群并运行 `kubectl` 命令。</td>
<td><code>[ibmcloud ks cluster-config](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_config)</code></td>
<td><code>[GET /v1/clusters/{idOrName}/config](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetClusterConfig)</code></td>
</tr>
<tr>
<td>查看集群的信息。</td>
<td><code>[ibmcloud ks cluster-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_get)</code></td>
<td><code>[GET /v1/clusters/{idOrName}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetCluster)</code></td>
</tr>
<tr>
<td>列出绑定到某个集群的所有名称空间中的所有服务。</td>
<td><code>[ibmcloud ks cluster-services](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_services)</code></td>
<td><code>[GET /v1/clusters/{idOrName}/services](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/ListServicesForAllNamespaces)</code></td>
</tr>
<tr>
<td>列出所有集群。</td>
<td><code>[ibmcloud ks clusters](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_clusters)</code></td>
<td><code>[GET /v1/clusters](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetClusters)</code></td>
</tr>
<tr>
<td>获取为 {{site.data.keyword.Bluemix_notm}} 帐户设置用于访问其他 IBM Cloud Infrastructure (SoftLayer) 产品服务组合的基础架构凭证。</td>
<td><code>[ibmcloud ks credential-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credential_get)</code></td><td><code>[GET /v1/credentials](https://containers.cloud.ibm.com/global/swagger-global-api/#/accounts/GetUserCredentials)</code></td>
</tr>
<tr>
<td>检查允许访问目标区域和资源组的 IBM Cloud Infrastructure (SoftLayer) 产品服务组合的凭证是否缺少建议或必需的基础架构许可权。</td>
<td><code>[ibmcloud ks infra-permissions-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#infra_permissions_get)</code></td>
<td><code>[GET /v1/infra-permissions](https://containers.cloud.ibm.com/global/swagger-global-api/#/accounts/GetInfraPermissions)</code></td>
</tr>
<tr>
<td>查看 Fluentd 附加组件自动更新的状态。</td>
<td><code>[ibmcloud ks logging-autoupdate-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_autoupdate_get)</code></td>
<td><code>[GET /v1/logging/{idOrName}/updatepolicy](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/GetUpdatePolicy)</code></td>
</tr>
<tr>
<td>查看目标区域的缺省日志记录端点。</td>
<td>-</td>
<td><code>[GET /v1/logging/{idOrName}/default](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/GetDefaultLoggingEndpoint)</code></td>
</tr>
<tr>
<td>列出集群中或集群中特定日志源的所有日志转发配置。</td>
<td><code>[ibmcloud ks logging-config-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_logging_get)</code></td>
<td><code>[GET /v1/logging/{idOrName}/loggingconfig](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/FetchLoggingConfigs) 和 [GET /v1/logging/{idOrName}/loggingconfig/{logSource}](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/FetchLoggingConfigsForSource)</code></td>
</tr>
<tr>
<td>查看日志过滤配置的信息。</td>
<td><code>[ibmcloud ks logging-filter-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_filter_view)</code></td>
<td><code>[GET /v1/logging/{idOrName}/filterconfigs/{id}](https://containers.cloud.ibm.com/global/swagger-global-api/#/filter/FetchFilterConfig)</code></td>
</tr>
<tr>
<td>列出集群中的所有日志记录过滤器配置。</td>
<td><code>[ibmcloud ks logging-filter-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_filter_view)</code></td>
<td><code>[GET /v1/logging/{idOrName}/filterconfigs](https://containers.cloud.ibm.com/global/swagger-global-api/#/filter/FetchFilterConfigs)</code></td>
</tr>
<tr>
<td>列出绑定到特定名称空间的所有服务。</td>
<td>-</td>
<td><code>[GET /v1/clusters/{idOrName}/services/{namespace}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/ListServicesInNamespace)</code></td>
</tr>
<tr>
<td>列出绑定到某个集群的所有由用户管理的子网。</td>
<td>-</td>
<td><code>[GET /v1/clusters/{idOrName}/usersubnets](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetClusterUserSubnet)</code></td>
</tr>
<tr>
<td>列出基础架构帐户中的可用子网。</td>
<td><code>[ibmcloud ks subnets](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_subnets)</code></td>
<td><code>[GET /v1/subnets](https://containers.cloud.ibm.com/global/swagger-global-api/#/properties/ListSubnets)</code></td>
</tr>
<tr>
<td>查看基础架构帐户的 VLAN 生成状态。</td>
<td><code>[ibmcloud ks vlan-spanning-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get)</code></td>
<td><code>[GET /v1/subnets/vlan-spanning](https://containers.cloud.ibm.com/global/swagger-global-api/#/accounts/GetVlanSpanning)</code></td>
</tr>
<tr>
<td>为一个集群设置时：列出该集群在专区中连接到的 VLAN。</br>为帐户中的所有集群设置时：列出专区中的所有可用 VLAN。</td>
<td><code>[ibmcloud ks vlans](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlans)</code></td>
<td><code>[GET /v1/datacenters/{datacenter}/vlans](https://containers.cloud.ibm.com/global/swagger-global-api/#/properties/GetDatacenterVLANs)</code></td>
</tr>
<tr>
<td>列出一个集群的所有 Webhook。</td>
<td>-</td>
<td><code>[GET /v1/clusters/{idOrName}/webhooks](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetClusterWebhooks)</code></td>
</tr>
<tr>
<td>查看工作程序节点的信息。</td>
<td><code>[ibmcloud ks worker-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_get)</code></td>
<td><code>[GET /v1/clusters/{idOrName}/workers/{workerId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetWorkers)</code></td>
</tr>
<tr>
<td>查看工作程序池的信息。</td>
<td><code>[ibmcloud ks worker-pool-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_get)</code></td>
<td><code>[GET /v1/clusters/{idOrName}/workerpools/{poolidOrName}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetWorkerPool)</code></td>
</tr>
<tr>
<td>列出一个集群中的所有工作程序池。</td>
<td><code>[ibmcloud ks worker-pools](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pools)</code></td>
<td><code>[GET /v1/clusters/{idOrName}/workerpools](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetWorkerPools)</code></td>
</tr>
<tr>
<td>列出一个集群中的所有工作程序节点。</td>
<td><code>[ibmcloud ks workers](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_workers)</code></td>
<td><code>[GET /v1/clusters/{idOrName}/workers](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetClusterWorkers)</code></td>
</tr>
</tbody>
</table>

### 编辑者操作
{: #editor-actions}

**编辑者**平台角色包含**查看者**授予的许可权以及以下许可权。具有**编辑者**角色（例如，开发者）的用户可以绑定服务，使用 Ingress 资源，并为其应用程序设置日志转发，但无法修改基础架构。**提示**：将此角色用于应用程序开发者，并分配 <a href="#cloud-foundry">Cloud Foundry</a> **开发者**角色。
{: shortdesc}

<table>
<caption>在 {{site.data.keyword.containerlong_notm}} 中需要编辑者平台角色的 CLI 命令和 API 调用概述</caption>
<thead>
<th id="editor-actions-mngt">操作</th>
<th id="editor-actions-cli">CLI 命令</th>
<th id="editor-actions-api">API 调用</th>
</thead>
<tbody>
<tr>
<td>禁用 Ingress ALB 附加组件的自动更新。</td>
<td><code>[ibmcloud ks alb-autoupdate-disable](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_autoupdate_disable)</code></td>
<td><code>[PUT /clusters/{idOrName}/updatepolicy](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/ChangeUpdatePolicy)</code></td>
</tr>
<tr>
<td>启用 Ingress ALB 附加组件的自动更新。</td>
<td><code>[ibmcloud ks alb-autoupdate-enable](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_autoupdate_enable)</code></td>
<td><code>[PUT /clusters/{idOrName}/updatepolicy](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/ChangeUpdatePolicy)</code></td>
</tr>
<tr>
<td>检查是否启用了 Ingress ALB 附加组件的自动更新。</td>
<td><code>[ibmcloud ks alb-autoupdate-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_autoupdate_get)</code></td>
<td><code>[GET /clusters/{idOrName}/updatepolicy](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/GetUpdatePolicy)</code></td>
</tr>
<tr>
<td>启用或禁用 Ingress ALB。</td>
<td><code>[ibmcloud ks alb-configure](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_configure)</code></td>
<td><code>[POST /albs](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/EnableALB) 和 [DELETE /albs/{albId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/)</code></td>
</tr>
<tr>
<td>创建 Ingress ALB。</td>
<td><code>[ibmcloud ks alb-create](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_create)</code></td>
<td><code>[POST /clusters/{idOrName}/zone/{zoneId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/CreateALB)</code></td>
</tr>
<tr>
<td>将 Ingress ALB 附加组件更新回滚到 ALB pod 先前在运行的构建。</td>
<td><code>[ibmcloud ks alb-rollback](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_rollback)</code></td>
<td><code>[PUT /clusters/{idOrName}/updaterollback](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/RollbackUpdate)</code></td>
</tr>
<tr>
<td>通过手动更新 Ingress ALB 附加组件，强制一次性更新 ALB pod。</td>
<td><code>[ibmcloud ks alb-update](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_update)</code></td>
<td><code>[PUT /clusters/{idOrName}/update](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/UpdateALBs)</code></td>
</tr>
<tr>
<td>创建 API 服务器审计 Webhook。</td>
<td><code>[ibmcloud ks apiserver-config-set](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_apiserver_config_set)</code></td>
<td><code>[PUT /v1/clusters/{idOrName}/apiserverconfigs/auditwebhook](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/apiserverconfigs/UpdateAuditWebhook)</code></td>
</tr>
<tr>
<td>删除 API 服务器审计 Webhook。</td>
<td><code>[ibmcloud ks apiserver-config-unset](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_apiserver_config_unset)</code></td>
<td><code>[DELETE /v1/clusters/{idOrName}/apiserverconfigs/auditwebhook](https://containers.cloud.ibm.com/global/swagger-global-api/#/apiserverconfigs/DeleteAuditWebhook)</code></td>
</tr>
<tr>
<td>将服务绑定到集群。 **注**：您必须具有对服务实例所在空间的 Cloud Foundry 开发者角色。</td>
<td><code>[ibmcloud ks cluster-service-bind](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_service_bind)</code></td>
<td><code>[POST /v1/clusters/{idOrName}/services](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/BindServiceToNamespace)</code></td>
</tr>
<tr>
<td>从集群取消绑定服务。**注**：您必须具有对服务实例所在空间的 Cloud Foundry 开发者角色。</td>
<td><code>[ibmcloud ks cluster-service-unbind](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_service_unbind)</code></td>
<td><code>[DELETE /v1/clusters/{idOrName}/services/{namespace}/{serviceInstanceId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/UnbindServiceFromNamespace)</code></td>
</tr>
<tr>
<td>为除 <code>kube-audit</code> 之外的其他所有日志源创建日志转发配置。</td>
<td><code>[ibmcloud ks logging-config-create](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_logging_create)</code></td>
<td><code>[POST /v1/logging/{idOrName}/loggingconfig/{logSource}](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/CreateLoggingConfig)</code></td>
</tr>
<tr>
<td>刷新日志转发配置。</td>
<td><code>[ibmcloud ks logging-config-refresh](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_logging_refresh)</code></td>
<td><code>[PUT /v1/logging/{idOrName}/refresh](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/RefreshLoggingConfig)</code></td>
</tr>
<tr>
<td>针对除 <code>kube-audit</code> 之外的其他所有日志源，删除日志转发配置。</td>
<td><code>[ibmcloud ks logging-config-rm](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_logging_rm)</code></td>
<td><code>[DELETE /v1/logging/{idOrName}/loggingconfig/{logSource}/{id}](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/DeleteLoggingConfig)</code></td>
</tr>
<tr>
<td>删除集群的所有日志转发配置。</td>
<td>-</td>
<td><code>[DELETE /v1/logging/{idOrName}/loggingconfig](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/DeleteLoggingConfigs)</code></td>
</tr>
<tr>
<td>更新日志转发配置。</td>
<td><code>[ibmcloud ks logging-config-update](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_logging_update)</code></td>
<td><code>[PUT /v1/logging/{idOrName}/loggingconfig/{logSource}/{id}](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/UpdateLoggingConfig)</code></td>
</tr>
<tr>
<td>创建日志过滤配置。</td>
<td><code>[ibmcloud ks logging-filter-create](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_filter_create)</code></td>
<td><code>[POST /v1/logging/{idOrName}/filterconfigs](https://containers.cloud.ibm.com/global/swagger-global-api/#/filter/CreateFilterConfig)</code></td>
</tr>
<tr>
<td>删除日志过滤配置。</td>
<td><code>[ibmcloud ks logging-filter-rm](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_filter_delete)</code></td>
<td><code>[DELETE /v1/logging/{idOrName}/filterconfigs/{id}](https://containers.cloud.ibm.com/global/swagger-global-api/#/filter/DeleteFilterConfig)</code></td>
</tr>
<tr>
<td>删除 Kubernetes 集群的所有日志记录过滤器配置。</td>
<td>-</td>
<td><code>[DELETE /v1/logging/{idOrName}/filterconfigs](https://containers.cloud.ibm.com/global/swagger-global-api/#/filter/DeleteFilterConfigs)</code></td>
</tr>
<tr>
<td>更新日志过滤配置。</td>
<td><code>[ibmcloud ks logging-filter-update](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_filter_update)</code></td>
<td><code>[PUT /v1/logging/{idOrName}/filterconfigs/{id}](https://containers.cloud.ibm.com/global/swagger-global-api/#/filter/UpdateFilterConfig)</code></td>
</tr>
<tr>
<td>将一个 NLB IP 添加到现有 NLB 主机名。</td>
<td><code>[ibmcloud ks nlb-dns-add](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-add)</code></td>
<td><code>[PUT /clusters/{idOrName}/add](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb45dns/UpdateDNSWithIP)</code></td>
</tr>
<tr>
<td>创建 DNS 主机名以注册 NLB IP 地址。</td>
<td><code>[ibmcloud ks nlb-dns-create](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-create)</code></td>
<td><code>[POST /clusters/{idOrName}/register](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb45dns/RegisterDNSWithIP)</code></td>
</tr>
<tr>
<td>列出集群中注册的 NLB 主机名和 IP 地址。</td>
<td><code>[ibmcloud ks nlb-dnss](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-ls)</code></td>
<td><code>[GET /clusters/{idOrName}/list](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb45dns/ListNLBIPsForSubdomain)</code></td>
</tr>
<tr>
<td>从主机名中除去 NLB IP 地址。</td>
<td><code>[ibmcloud ks nlb-dns-rm](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-rm)</code></td>
<td><code>[DELETE /clusters/{idOrName}/host/{nlbHost}/ip/{nlbIP}/remove](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb45dns/UnregisterDNSWithIP)</code></td>
</tr>
<tr>
<td>为集群中的现有 NLB 主机名配置运行状况检查监视器，并可选择启用该监视器。</td>
<td><code>[ibmcloud ks nlb-dns-monitor-configure](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-monitor-configure)</code></td>
<td><code>[POST /health/clusters/{idOrName}/config](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb45health45monitor/AddNlbDNSHealthMonitor)</code></td>
</tr>
<tr>
<td>查看现有运行状况检查监视器的设置。</td>
<td><code>[ibmcloud ks nlb-dns-monitor-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-monitor-get)</code></td>
<td><code>[GET /health/clusters/{idOrName}/host/{nlbHost}/config](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb45health45monitor/GetNlbDNSHealthMonitor)</code></td>
</tr>
<tr>
<td>对集群中的主机名禁用现有运行状况检查监视器。</td>
<td><code>[ibmcloud ks nlb-dns-monitor-disable](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-monitor-disable)</code></td>
<td><code>[PUT /clusters/{idOrName}/health](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb45health45monitor/UpdateNlbDNSHealthMonitor)</code></td>
</tr>
<tr>
<td>启用配置的运行状况检查监视器。</td>
<td><code>[ibmcloud ks nlb-dns-monitor-enable](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-monitor-enable)</code></td>
<td><code>[PUT /clusters/{idOrName}/health](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb45health45monitor/UpdateNlbDNSHealthMonitor)</code></td>
</tr>
<tr>
<td>列出集群中每个 NLB 主机名的运行状况检查监视器设置。</td>
<td><code>[ibmcloud ks nlb-dns-monitor-ls](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-monitor-ls)</code></td>
<td><code>[GET /health/clusters/{idOrName}/list](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb45health45monitor/ListNlbDNSHealthMonitors)</code></td>
</tr>
<tr>
<td>列出向集群中的 NLB 主机名注册的每个 IP 地址的运行状况检查状态。</td>
<td><code>[ibmcloud ks nlb-dns-monitor-status](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-monitor-status)</code></td>
<td><code>[GET /health/clusters/{idOrName}/status](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb45health45monitor/ListNlbDNSHealthMonitorStatus)</code></td>
</tr>
<tr>
<td>在集群中创建 Webhook。</td>
<td><code>[ibmcloud ks webhook-create](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_webhook_create)</code></td>
<td><code>[POST /v1/clusters/{idOrName}/webhooks](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/AddClusterWebhooks)</code></td>
</tr>
</tbody>
</table>

### 操作员操作
{: #operator-actions}

**操作员**平台角色包含**查看者**授予的许可权以及下表中显示的许可权。
具有**操作员**角色（例如，站点可靠性工程师、DevOps 工程师或集群管理员）的用户可以添加工作程序节点，并对基础架构进行故障诊断（如通过重新装入工作程序节点），但无法创建或删除集群，更改凭证或设置集群范围的功能，如服务端点或受管附加组件。
{: shortdesc}

<table>
<caption>在 {{site.data.keyword.containerlong_notm}} 中需要操作者平台角色的 CLI 命令和 API 调用概述</caption>
<thead>
<th id="operator-mgmt">操作</th>
<th id="operator-cli">CLI 命令</th>
<th id="operator-api">API 调用</th>
</thead>
<tbody>
<tr>
<td>刷新 Kubernetes 主节点。</td>
<td><code>[ibmcloud ks apiserver-refresh](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_apiserver_refresh) (cluster-refresh)</code></td>
<td><code>[PUT /v1/clusters/{idOrName}/masters](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/HandleMasterAPIServer)</code></td>
</tr>
<tr>
<td>为集群创建 {{site.data.keyword.Bluemix_notm}} IAM 服务标识，为该服务标识创建策略，以用于在 {{site.data.keyword.registrylong_notm}} 中分配**读者**服务访问角色，然后为该服务标识创建 API 密钥。</td>
<td><code>[ibmcloud ks cluster-pull-secret-apply](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_pull_secret_apply)</code></td>
<td>-</td>
</tr>
<tr>
<td>向集群添加子网。</td>
<td><code>[ibmcloud ks cluster-subnet-add](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_subnet_add)</code></td>
<td><code>[PUT /v1/clusters/{idOrName}/subnets/{subnetId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/AddClusterSubnet)</code></td>
</tr>
<tr>
<td>创建子网并将其添加到集群。</td>
<td><code>[ibmcloud ks cluster-subnet-create](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_subnet_create)</code></td>
<td><code>[POST /v1/clusters/{idOrName}/vlans/{vlanId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/CreateClusterSubnet)</code></td>
</tr>
<tr>
<td>更新集群。</td>
<td><code>[ibmcloud ks cluster-update](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_update)</code></td>
<td><code>[PUT /v1/clusters/{idOrName}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/UpdateCluster)</code></td>
</tr>
<tr>
<td>向集群添加用户管理的子网。</td>
<td><code>[ibmcloud ks cluster-user-subnet-add](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_user_subnet_add)</code></td>
<td><code>[POST /v1/clusters/{idOrName}/usersubnets](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/AddClusterUserSubnet)</code></td>
</tr>
<tr>
<td>从集群中除去用户管理的子网。</td>
<td><code>[ibmcloud ks cluster-user-subnet-rm](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_user_subnet_rm)</code></td>
<td><code>[DELETE /v1/clusters/{idOrName}/usersubnets/{subnetId}/vlans/{vlanId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/RemoveClusterUserSubnet)</code></td>
</tr>
<tr>
<td>添加工作程序节点。</td>
<td><code>[ibmcloud ks worker-add（不推荐）](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_add)</code></td>
<td><code>[POST /v1/clusters/{idOrName}/workers](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/AddClusterWorkers)</code></td>
</tr>
<tr>
<td>创建工作程序池。</td>
<td><code>[ibmcloud ks worker-pool-create](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_create)</code></td>
<td><code>[POST /v1/clusters/{idOrName}/workerpools](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/CreateWorkerPool)</code></td>
</tr>
<tr>
<td>重新均衡工作程序池。</td>
<td><code>[ibmcloud ks worker-pool-rebalance](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_rebalance)</code></td>
<td><code>[PATCH /v1/clusters/{idOrName}/workerpools/{poolidOrName}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/PatchWorkerPool)</code></td>
</tr>
<tr>
<td>调整工作程序池的大小。</td>
<td><code>[ibmcloud ks worker-pool-resize](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_resize)</code></td>
<td><code>[PATCH /v1/clusters/{idOrName}/workerpools/{poolidOrName}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/PatchWorkerPool)</code></td>
</tr>
<tr>
<td>删除工作程序池。</td>
<td><code>[ibmcloud ks worker-pool-rm](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_rm)</code></td>
<td><code>[DELETE /v1/clusters/{idOrName}/workerpools/{poolidOrName}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/RemoveWorkerPool)</code></td>
</tr>
<tr>
<td>重新引导工作程序节点。</td>
<td><code>[ibmcloud ks worker-reboot](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reboot)</code></td>
<td><code>[PUT /v1/clusters/{idOrName}/workers/{workerId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/UpdateClusterWorker)</code></td>
</tr>
<tr>
<td>重新装入工作程序节点。</td>
<td><code>[ibmcloud ks worker-reload](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)</code></td>
<td><code>[PUT /v1/clusters/{idOrName}/workers/{workerId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/UpdateClusterWorker)</code></td>
</tr>
<tr>
<td>除去工作程序节点。</td>
<td><code>[ibmcloud ks worker-rm](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_rm)</code></td>
<td><code>[DELETE /v1/clusters/{idOrName}/workers/{workerId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/RemoveClusterWorker)</code></td>
</tr>
<tr>
<td>更新工作程序节点。</td>
<td><code>[ibmcloud ks worker-update](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update)</code></td>
<td><code>[PUT /v1/clusters/{idOrName}/workers/{workerId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/UpdateClusterWorker)</code></td>
</tr>
<tr>
<td>向工作程序池添加专区。</td>
<td><code>[ibmcloud ks zone-add](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_zone_add)</code></td>
<td><code>[POST /v1/clusters/{idOrName}/workerpools/{poolidOrName}/zones](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/AddWorkerPoolZone)</code></td>
</tr>
<tr>
<td>更新工作程序池中给定专区的网络配置。</td>
<td><code>[ibmcloud ks zone-network-set](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_zone_network_set)</code></td>
<td><code>[PATCH /v1/clusters/{idOrName}/workerpools/{poolidOrName}/zones/{zoneid}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/AddWorkerPoolZoneNetwork)</code></td>
</tr>
<tr>
<td>从工作程序池中除去专区。</td>
<td><code>[ibmcloud ks zone-rm](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_zone_rm)</code></td>
<td><code>[DELETE /v1/clusters/{idOrName}/workerpools/{poolidOrName}/zones/{zoneid}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/RemoveWorkerPoolZone)</code></td>
</tr>
</tbody>
</table>

### 管理员操作
{: #admin-actions}

**管理员**平台角色包含由**查看者**、**编辑者**和**操作员**角色授予的所有许可权以及以下许可权。具有**管理员**角色（例如，集群或帐户管理员）的用户可以创建和删除集群，或设置集群范围的功能，如服务端点或受管附加组件。要创建订单来订购工作程序节点机器、VLAN 和子网之类的基础架构资源，管理员用户需要**超级用户**<a href="#infra">基础架构角色</a>，或者区域的 API 密钥必须设置有相应的许可权。
{: shortdesc}

<table>
<caption>在 {{site.data.keyword.containerlong_notm}} 中需要管理员平台角色的 CLI 命令和 API 调用概述</caption>
<thead>
<th id="admin-mgmt">操作</th>
<th id="admin-cli">CLI 命令</th>
<th id="admin-api">API 调用</th>
</thead>
<tbody>
<tr>
<td>Beta：将证书从 {{site.data.keyword.cloudcerts_long_notm}} 实例部署到 ALB 或更新该证书。</td>
<td><code>[ibmcloud ks alb-cert-deploy](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_cert_deploy)</code></td>
<td><code>[POST /albsecrets](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/CreateALBSecret) 或 [PUT /albsecrets](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/UpdateALBSecret)</code></td>
</tr>
<tr>
<td>Beta：查看有关集群中 ALB 私钥的详细信息。</td>
<td><code>[ibmcloud ks alb-cert-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_cert_get)</code></td>
<td><code>[GET /clusters/{idOrName}/albsecrets](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/ViewClusterALBSecrets)</code></td>
</tr>
<tr>
<td>Beta：从集群中除去 ALB 私钥。</td>
<td><code>[ibmcloud ks alb-cert-rm](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_cert_rm)</code></td>
<td><code>[DELETE /clusters/{idOrName}/albsecrets](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/DeleteClusterALBSecrets)</code></td>
</tr>
<tr>
<td>列出一个集群中的所有 ALB 私钥。</td>
<td><code>[ibmcloud ks alb-certs](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_certs)</code></td>
<td>-</td>
</tr>
<tr>
<td>为 {{site.data.keyword.Bluemix_notm}} 帐户设置 API 密钥以访问链接的 IBM Cloud Infrastructure (SoftLayer) 产品服务组合。</td>
<td><code>[ibmcloud ks api-key-reset](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_api_key_reset)</code></td>
<td><code>[POST /v1/keys](https://containers.cloud.ibm.com/global/swagger-global-api/#/accounts/ResetUserAPIKey)</code></td>
</tr>
<tr>
<td>在集群中禁用受管附加组件（例如，Istio 或 Knative）。</td>
<td><code>[ibmcloud ks cluster-addon-disable](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_addon_disable)</code></td>
<td><code>[PATCH /v1/clusters/{idOrName}/addons](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/ManageClusterAddons)</code></td>
</tr>
<tr>
<td>在集群中启用受管附加组件（例如，Istio 或 Knative）。</td>
<td><code>[ibmcloud ks cluster-addon-enable](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_addon_enable)</code></td>
<td><code>[PATCH /v1/clusters/{idOrName}/addons](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/ManageClusterAddons)</code></td>
</tr>
<tr>
<td>列出在集群中启用的受管附加组件（例如，Istio 或 Knative）。</td>
<td><code>[ibmcloud ks cluster-addons](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_addons)</code></td>
<td><code>[GET /v1/clusters/{idOrName}/addons](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetClusterAddons)</code></td>
</tr>
<tr>
<td>创建免费或标准集群。**注**：还需要对 {{site.data.keyword.registrylong_notm}} 的管理员平台角色以及超级用户基础架构角色。</td>
<td><code>[ibmcloud ks cluster-create](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_create)</code></td>
<td><code>[POST /v1/clusters](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/CreateCluster)</code></td>
</tr>
<tr>
<td>禁用集群的指定功能，例如集群主节点的公共服务端点。</td>
<td><code>[ibmcloud ks cluster-feature-disable](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_disable)</code></td>
<td>-</td>
</tr>
<tr>
<td>启用集群的指定功能，例如集群主节点的专用服务端点。</td>
<td><code>[ibmcloud ks cluster-feature-enable](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable)</code></td>
<td>-</td>
</tr>
<tr>
<td>删除集群。</td>
<td><code>[ibmcloud ks cluster-rm](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_rm)</code></td>
<td><code>[DELETE /v1/clusters/{idOrName}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/RemoveCluster)</code></td>
</tr>
<tr>
<td>为 {{site.data.keyword.Bluemix_notm}} 帐户设置用于访问其他 IBM Cloud Infrastructure (SoftLayer) 产品服务组合的基础架构凭证。</td>
<td><code>[ibmcloud ks credential-set](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_set)</code></td>
<td><code>[POST /v1/credentials](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/accounts/StoreUserCredentials)</code></td>
</tr>
<tr>
<td>除去 {{site.data.keyword.Bluemix_notm}} 帐户用于访问其他 IBM Cloud Infrastructure (SoftLayer) 产品服务组合的基础架构凭证。</td>
<td><code>[ibmcloud ks credential-unset](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_unset)</code></td>
<td><code>[DELETE /v1/credentials](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/accounts/RemoveUserCredentials)</code></td>
</tr>
<tr>
<td>Beta：使用 {{site.data.keyword.keymanagementservicefull}} 加密 Kubernetes 私钥。</td>
<td><code>[ibmcloud ks key-protect-enable](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_messages)</code></td>
<td><code>[POST /v1/clusters/{idOrName}/kms](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/CreateKMSConfig)</code></td>
</tr>
<tr>
<td>禁用 Fluentd 集群附加组件的自动更新。</td>
<td><code>[ibmcloud ks logging-autoupdate-disable](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_autoupdate_disable)</code></td>
<td><code>[PUT /v1/logging/{idOrName}/updatepolicy](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/ChangeUpdatePolicy)</code></td>
</tr>
<tr>
<td>启用 Fluentd 集群附加组件的自动更新。</td>
<td><code>[ibmcloud ks logging-autoupdate-enable](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_autoupdate_enable)</code></td>
<td><code>[PUT /v1/logging/{idOrName}/updatepolicy](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/ChangeUpdatePolicy)</code></td>
</tr>
<tr>
<td>在 {{site.data.keyword.cos_full_notm}} 存储区中收集 API 服务器日志的快照。</td>
<td><code>[ibmcloud ks logging-collect](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_collect)</code></td>
<td>[POST /v1/log-collector/{idOrName}/masterlogs](https://containers.cloud.ibm.com/global/swagger-global-api/#/log45collector/CreateMasterLogCollection)</td>
</tr>
<tr>
<td>查看 API 服务器日志快照请求的状态。</td>
<td><code>[ibmcloud ks logging-collect-status](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_collect_status)</code></td>
<td>[GET /v1/log-collector/{idOrName}/masterlogs](https://containers.cloud.ibm.com/global/swagger-global-api/#/log45collector/GetMasterLogCollectionStatus)</td>
</tr>
<tr>
<td>为 <code>kube-audit</code> 日志源创建日志转发配置。</td>
<td><code>[ibmcloud ks logging-config-create](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_logging_create)</code></td>
<td><code>[POST /v1/logging/{idOrName}/loggingconfig/{logSource}](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/CreateLoggingConfig)</code></td>
</tr>
<tr>
<td>删除 <code>kube-audit</code> 日志源的日志转发配置。</td>
<td><code>[ibmcloud ks logging-config-rm](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_logging_rm)</code></td>
<td><code>[DELETE /v1/logging/{idOrName}/loggingconfig/{logSource}/{id}](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/DeleteLoggingConfig)</code></td>
</tr>
</tbody>
</table>

<br />


## {{site.data.keyword.Bluemix_notm}} IAM 服务角色
{: #service}

分配有 {{site.data.keyword.Bluemix_notm}} IAM 服务访问角色的每个用户还会在特定名称空间中自动分配有相应的 Kubernetes 基于角色的访问控制 (RBAC) 角色。要了解有关服务访问角色的更多信息，请参阅 [{{site.data.keyword.Bluemix_notm}} IAM 服务角色](/docs/containers?topic=containers-users#platform)。不要在分配 {{site.data.keyword.Bluemix_notm}} IAM 平台角色的同时分配服务角色。您必须单独分配平台角色和服务角色。
{: shortdesc}

需要了解每个服务角色通过 RBAC 授权的 Kubernetes 操作？请参阅[每个 RBAC 角色的 Kubernetes 资源许可权](#rbac_ref)。
要了解有关 RBAC 角色的更多信息，请参阅[分配 RBAC 许可权](/docs/containers?topic=containers-users#role-binding)和[通过聚集集群角色扩展现有许可权](https://cloud.ibm.com/docs/containers?topic=containers-users#rbac_aggregate)。
{: tip}

下表显示了每个服务角色及其相应的 RBAC 角色授予的 Kubernetes 资源许可权。

<table>
<caption>服务角色和相应的 RBAC 角色授予的 Kubernetes 资源许可权</caption>
<thead>
    <th id="service-role">服务角色</th>
    <th id="rbac-role">相应的 RBAC 角色、绑定和作用域</th>
    <th id="kube-perm">Kubernetes 资源许可权</th>
</thead>
<tbody>
  <tr>
    <td id="service-role-reader" headers="service-role">读取者角色</td>
    <td headers="service-role-reader rbac-role">作用域限定为一个名称空间时：由该名称空间中的 <strong><code>ibm-view</code></strong> 角色绑定所应用的 <strong><code>view</code></strong> 集群角色</br><br>作用域限定为所有名称空间时：由集群的每个名称空间中的 <strong><code>ibm-view</code></strong> 角色绑定所应用的 <strong><code>view</code></strong> 集群角色</td>
    <td headers="service-role-reader kube-perm"><ul>
      <li>对名称空间中资源的读访问权</li>
      <li>没有对角色和角色绑定的读访问权，也没有对 Kubernetes 私钥的读访问权</li>
      <li>访问 Kubernetes 仪表板可查看名称空间中的资源</li></ul>
    </td>
  </tr>
  <tr>
    <td id="service-role-writer" headers="service-role">写入者角色</td>
    <td headers="service-role-writer rbac-role">作用域限定为一个名称空间时：由该名称空间中的 <strong><code>ibm-edit</code></strong> 角色绑定所应用的 <strong><code>edit</code></strong> 集群角色</br><br>作用域限定为所有名称空间时：由集群中每个名称空间中的 <strong><code>ibm-edit</code></strong> 角色绑定所应用的 <strong><code>edit</code></strong> 集群角色</td>
    <td headers="service-role-writer kube-perm"><ul><li>对名称空间中资源的读/写访问权</li>
    <li>没有对角色和角色绑定的读/写访问权。</li>
    <li>访问 Kubernetes 仪表板可查看名称空间中的资源</li></ul>
    </td>
  </tr>
  <tr>
    <td id="service-role-manager" headers="service-role">管理者角色</td>
    <td headers="service-role-manager rbac-role">作用域限定为一个名称空间时：由该名称空间中的 <strong><code>ibm-operate</code></strong> 角色绑定所应用的 <strong><code>admin</code></strong> 集群角色</br><br>作用域限定为所有名称空间时：由应用于所有名称空间的 <strong><code>ibm-admin</code></strong> 集群角色绑定所应用的 <strong><code>cluster-admin</code></strong> 集群角色</td>
    <td headers="service-role-manager kube-perm">作用域限定为一个名称空间时：
      <ul><li>对名称空间中所有资源的读/写访问权，但没有对资源配额或名称空间本身的读/写访问权</li>
      <li>在名称空间中创建 RBAC 角色和角色绑定</li>
      <li>访问 Kubernetes 仪表板可查看名称空间中的所有资源</li></ul>
    </br>作用域限定为所有名称空间时：
      <ul><li>对每个名称空间中所有资源的读/写访问权</li>
        <li>在名称空间中创建 RBAC 角色和角色绑定，或在所有名称空间中创建集群角色和集群角色绑定</li>
        <li>访问 Kubernetes 仪表板</li>
        <li>创建使应用程序公共可用的 Ingress 资源</li>
        <li>查看集群度量值，例如使用 <code>kubectl top pods</code>、<code>kubectl top nodes</code> 或 <code>kubectl get nodes</code> 命令</li></ul>
    </td>
  </tr>
</tbody>
</table>

<br />


## 每个 RBAC 角色的 Kubernetes 资源许可权
{: #rbac_ref}

分配有 {{site.data.keyword.Bluemix_notm}} IAM 服务访问角色的每个用户还会自动分配有相应的预定义 Kubernetes 基于角色的访问控制 (RBAC) 角色。如果计划管理您自己的定制 Kubernetes RBAC 角色，请参阅[为用户、组或服务帐户创建定制 RBAC 许可权](/docs/containers?topic=containers-users#rbac)。
{: shortdesc}

想了解您是否具有对名称空间中的某个资源运行特定 `kubectl` 命令的正确许可权？请尝试使用 [`kubectl auth can-i` 命令 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#-em-can-i-em-)。
{: tip}

下表显示了每个 RBAC 角色授予的对单个 Kubernetes 资源的许可权。许可权显示为具有该角色的用户可以对资源完成的操作动词，例如“get”、“list”、“describe”、“create”或“delete”。

<table>
 <caption>每个预定义 RBAC 角色授予的 Kubernetes 资源许可权</caption>
 <thead>
  <th>Kubernetes 资源</th>
  <th><code>view</code></th>
  <th><code>edit</code></th>
  <th><code>admin</code> 和 <code>cluster-admin</code></th>
 </thead>
<tbody>
<tr>
  <td><code>bindings</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
</tr><tr>
  <td><code>configmaps</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td><code>cronjobs.batch</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td><code>daemonsets.apps</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td><code>daemonsets.extensions</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td><code>deployments.apps</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td><code>deployments.apps/rollback</code></td>
  <td>-</td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td><code>deployments.apps/scale</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td><code>deployments.extensions</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td><code>deployments.extensions/rollback</code></td>
  <td>-</td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td><code>deployments.extensions/scale</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td><code>endpoints</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td><code>events</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
</tr><tr>
  <td><code>horizontalpodautoscalers.autoscaling</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td><code>ingresses.extensions</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td><code>jobs.batch</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td><code>limitranges</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
</tr><tr>
  <td><code>localsubjectaccessreviews</code></td>
  <td>-</td>
  <td>-</td>
  <td><code>create</code></td>
</tr><tr>
  <td><code>namespaces</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></br>**仅限集群管理员：**<code>create</code> 和 <code>delete</code></td>
</tr><tr>
  <td><code>namespaces/status</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
</tr><tr>
  <td><code>networkpolicies</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td><code>networkpolicies.extensions</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td><code>persistentvolumeclaims</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td><code>poddisruptionbudgets.policy</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td><code>pods</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>top</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td><code>pods/attach</code></td>
  <td>-</td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td><code>pods/exec</code></td>
  <td>-</td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td><code>pods/log</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
</tr><tr>
  <td><code>pods/portforward</code></td>
  <td>-</td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td><code>pods/proxy</code></td>
  <td>-</td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td><code>pods/status</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
</tr><tr>
  <td><code>replicasets.apps</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td><code>replicasets.apps/scale</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td><code>replicasets.extensions</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td><code>replicasets.extensions/scale</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td><code>replicationcontrollers</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td><code>replicationcontrollers/scale</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td><code>replicationcontrollers/status</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
</tr><tr>
  <td><code>replicationcontrollers.extensions/scale</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td><code>resourcequotas</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
</tr><tr>
  <td><code>resourcequotas/status</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
</tr><tr>
  <td><code>rolebindings</code></td>
  <td>-</td>
  <td>-</td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td><code>角色
</code></td>
  <td>-</td>
  <td>-</td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td><code>私钥</code></td>
  <td>-</td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td><code> serviceaccounts                               </code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code>、<code>watch</code> 和 <code>impersonate</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code>、<code>watch</code> 和 <code>impersonate</code></td>
</tr><tr>
  <td><code> 服务</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td><code>services/proxy</code></td>
  <td>-</td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td><code>statefulsets.apps</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td><code>statefulsets.apps/scale</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr>
</tbody>
</table>

<br />


## Cloud Foundry 角色
{: #cloud-foundry}

Cloud Foundry 角色会授予对帐户内组织和空间的访问权。要在 {{site.data.keyword.Bluemix_notm}} 中查看基于 Cloud Foundry 的服务的列表，请运行 `ibmcloud service list`。要了解更多信息，请参阅 {{site.data.keyword.Bluemix_notm}} IAM 文档中的所有可用[组织和空间角色](/docs/iam?topic=iam-cfaccess)或[管理 Cloud Foundry 访问权](/docs/iam?topic=iam-mngcf)的步骤。
{: shortdesc}

下表显示了集群操作许可权所需的 Cloud Foundry 角色。

<table>
  <caption>集群管理许可权（按 Cloud Foundry 角色）</caption>
  <thead>
    <th>Cloud Foundry 角色</th>
    <th>集群管理许可权</th>
  </thead>
  <tbody>
  <tr>
    <td>空间角色：管理员</td>
    <td>管理用户对 {{site.data.keyword.Bluemix_notm}} 空间的访问权</td>
  </tr>
  <tr>
    <td>空间角色：开发者</td>
    <td>
      <ul><li>创建 {{site.data.keyword.Bluemix_notm}} 服务实例</li>
      <li>将 {{site.data.keyword.Bluemix_notm}} 服务实例绑定到集群</li>
      <li>通过集群日志转发配置在空间级别查看日志</li></ul>
    </td>
  </tr>
  </tbody>
</table>

## 基础架构角色
{: #infra}

具有**超级用户**基础架构访问角色的用户可[为区域和资源组设置 API 密钥](/docs/containers?topic=containers-users#api_key)，以便可以执行基础架构操作（或者较少见的情况下，[手动设置其他帐户凭证](/docs/containers?topic=containers-users#credentials)）。然后，帐户中其他用户可以执行的基础架构操作将通过 {{site.data.keyword.Bluemix_notm}} IAM 平台角色进行授权。您无需编辑其他用户的 IBM Cloud Infrastructure (SoftLayer) 许可权。仅当无法将**超级用户**分配给设置 API 密钥的用户时，才可以使用下表来定制用户的 IBM Cloud Infrastructure (SoftLayer) 许可权。有关分配许可权的指示信息，请参阅[定制基础架构许可权](/docs/containers?topic=containers-users#infra_access)。
{: shortdesc}



下表显示了完成多组常见任务所必需的基础架构许可权。

<table>
<caption>{{site.data.keyword.containerlong_notm}} 通常必需的基础架构许可权</caption>
<thead>
  <th>{{site.data.keyword.containerlong_notm}} 中的常见任务</th>
  <th>必需的基础架构许可权（按类别）</th>
</thead>
<tbody>
<tr>
<td>
  <strong>最低许可权</strong>：<ul>
  <li>创建集群。</li></ul></td>
<td>
<strong>帐户</strong>：<ul>
<li>添加服务器</li></ul>
  <strong>设备</strong>：<ul>
  <li>对于裸机工作程序节点：查看硬件详细信息</li>
  <li>IPMI 远程管理</li>
  <li>操作系统重装和急救内核</li>
  <li>对于 VM 工作程序节点：查看虚拟服务器详细信息</li></ul></td>
</tr>
<tr>
<td>
<strong>集群管理</strong>：<ul>
  <li>创建、更新和删除集群。</li>
  <li>添加、重新装入和重新引导工作程序节点。</li>
  <li>查看 VLAN。</li>
  <li>创建子网。</li>
  <li>部署 pod 和 LoadBalancer 服务。</li></ul>
  </td><td>
<strong>帐户</strong>：<ul>
  <li>添加服务器</li>
  <li>取消服务器</li></ul>
<strong>设备</strong>：<ul>
  <li>对于裸机工作程序节点：查看硬件详细信息</li>
  <li>IPMI 远程管理</li>
  <li>操作系统重装和急救内核</li>
  <li>对于 VM 工作程序节点：查看虚拟服务器详细信息</li></ul>
<strong>网络</strong>：<ul>
  <li>使用公用网络端口添加计算</li></ul>
<p class="important">您还必须为用户分配管理支持案例的能力。请参阅[定制基础架构许可权](/docs/containers?topic=containers-users#infra_access)中的步骤 8。</p>
</td>
</tr>
<tr>
<td>
  <strong>存储器</strong>：<ul>
  <li>创建持久卷声明以供应持久卷。</li>
  <li>创建和管理存储器基础架构资源。</li></ul></td>
<td>
<strong>帐户</strong>：<ul>
  <li>添加/升级存储器 (StorageLayer)</li></ul>
<strong>服务</strong>：<ul>
  <li>存储管理</li></ul></td>
</tr>
<tr>
<td>
  <strong>专用联网</strong>：<ul>
  <li>管理用于集群内联网的专用 VLAN。</li>
  <li>设置与专用网络的 VPN 连接。</li></ul></td>
<td>
  <strong>网络</strong>：<ul>
  <li>管理网络子网路径</li></ul></td>
</tr>
<tr>
<td>
  <strong>公用网络</strong>：<ul>
  <li>设置公共负载均衡器或 Ingress 联网以公开应用程序。</li></ul></td>
<td>
<strong>设备</strong>：<ul>
<li>管理端口控制</li>
  <li>编辑主机名/域</li></ul>
<strong>网络</strong>：<ul>
  <li>添加 IP 地址</li>
  <li>管理网络子网路径</li>
  <li>使用公用网络端口添加计算</li></ul>
<strong>服务</strong>：<ul>
  <li>管理 DNS</li>
  <li>查看证书 (SSL)</li>
  <li>管理证书 (SSL)</li></ul></td>
</tr>
</tbody>
</table>
