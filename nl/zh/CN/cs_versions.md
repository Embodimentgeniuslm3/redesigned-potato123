---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-11"

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
{:note: .note}


# 版本信息和更新操作
{: #cs_versions}

## Kubernetes 版本类型
{: #version_types}

{{site.data.keyword.containerlong}} 同时支持多个版本的 Kubernetes。发布最新版本 (n) 时，支持最多低 2 (n-2) 的版本。比最新版本低 2 以上的版本 (n-3) 将首先不推荐使用，然后不再支持。
{:shortdesc}

**支持的 Kubernetes 版本**：
*   最新版本：1.14.2 
*   缺省版本：1.13.6
*   其他版本：1.12.9

**不推荐和不支持的 Kubernetes 版本**：
*   不推荐的版本：1.11
*   不支持的版本：1.5、1.7、1.8、1.9 和 1.10 

</br>

**不推荐的版本**：集群在不推荐的 Kubernetes 版本上运行时，您至少有 30 天的时间来复查并更新到支持的 Kubernetes 版本，30 天后此版本变为不受支持。在不推荐期间，集群仍可正常工作，但可能需要更新为支持的发行版以修复安全漏洞。例如，距离不支持的日期等于或短于 30 天时，可以添加和重新装入工作程序节点，但不能创建使用不推荐版本的新集群。

**不支持的版本**：如果集群在不支持的 Kubernetes 版本上运行，请查看以下潜在更新影响，然后立即[更新集群](/docs/containers?topic=containers-update#update)以继续接收重要的安全性更新和支持。不支持的集群无法添加或重新装入现有工作程序节点。通过在 `ibmcloud ks clusters` 命令的输出中查看 **State** 字段，或在 [{{site.data.keyword.containerlong_notm}} 控制台 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/kubernetes/clusters) 中查看该字段，可以了解集群是否为**不受支持**。

如果您等到集群低于最旧受支持版本三个或更多次版本时才更新，那么无法更新集群。请改为[创建新集群](/docs/containers?topic=containers-clusters#clusters)，[部署应用程序](/docs/containers?topic=containers-app#app)至新集群，然后[删除](/docs/containers?topic=containers-remove)不支持的集群。<br><br>要避免此问题，请将不推荐使用的集群更新为比当前版本高最多两个版本的受支持的版本，例如先从 1.11 更新到 1.12，然后再更新到最新版本 1.14。如果工作程序节点运行的版本低于主节点三个或更多版本，那么您可能会看到 pod 失败并进入某个状态，例如 `MatchNodeSelector`、`CrashLoopBackOff` 或 `ContainerCreating`，直到您将工作程序节点更新为与主节点相同的版本。从不推荐的版本更新为受支持的版本后，集群可以恢复正常运行并继续接收支持。
{: important}

</br>

要检查集群的服务器版本，请运行以下命令。
```
kubectl version  --short | grep -i server
```
{: pre}

输出示例：
```
Server Version: v1.13.6+IKS
```
{: screen}


## 更新类型
{: #update_types}

Kubernetes 集群有三种类型的更新：主要更新、次要更新和补丁更新。
{:shortdesc}

|更新类型|版本标签示例|更新者|影响
|-----|-----|-----|-----|
|主要|1.x.x|您|集群的操作更改，包括脚本或部署。|
|次要|x.9.x|您|集群的操作更改，包括脚本或部署。|
|补丁|x.x.4_1510|IBM 和您|Kubernetes 补丁以及其他 {{site.data.keyword.Bluemix_notm}} Provider 组件更新，例如安全性和操作系统补丁。IBM 会自动更新主节点，但由您将补丁应用于工作程序节点。请参阅以下部分中有关补丁的更多信息。|
{: caption="Kubernetes 更新的影响" caption-side="top"}

更新可用时，您在查看有关工作程序节点的信息时（例如，使用 `ibmcloud ks workers --cluster <cluster>` 或 `ibmcloud ks worker-get --cluster <cluster> --worker <worker>` 命令），会收到相应通知。
-  **主要更新和次要更新 (1.x)**：首先[更新主节点](/docs/containers?topic=containers-update#master)，然后[更新工作程序节点](/docs/containers?topic=containers-update#worker_node)。工作程序节点运行的 Kubernetes 主版本或次版本不能高于主节点。
   - 最多只能跨 Kubernetes 主节点的两个次版本进行更新。例如，如果当前主节点的版本是 1.11，而您要更新到 1.14，那么必须先更新到 1.12。
   - 您使用的 `kubectl` CLI 版本至少应该与集群的 `major.minor` 版本相匹配，否则可能会遇到意外的结果。务必使 Kubernetes 集群版本和 [CLI 版本](/docs/containers?topic=containers-cs_cli_install#kubectl)保持最新。

-  **补丁更新 (x.x.4_1510)**：各补丁之间的更改会记录在[版本更改日志](/docs/containers?topic=containers-changelog)中。主节点补丁会自动应用，但工作程序节点补丁更新须由您来启动。此外，工作程序节点运行的补丁版本可以高于主节点。更新可用时，您在 {{site.data.keyword.Bluemix_notm}} 控制台或 CLI 中查看有关主节点和工作程序节点的信息时（例如，使用 `ibmcloud ks clusters`、`cluster-get`、`workers` 或 `worker-get` 命令），会收到相应通知。
   - **工作程序节点补丁**：每月检查以了解更新是否可用，并使用 `ibmcloud ks worker-update` [命令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update)或 `ibmcloud ks worker-reload` [命令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)来应用这些安全性和操作系统补丁。在更新或重新装入期间，将重新创建工作程序节点机器的映像，并且如果数据未[存储在工作程序节点外部](/docs/containers?topic=containers-storage_planning#persistent_storage_overview)，那么将删除数据。
   - **主节点补丁**：主节点补丁会自动应用，但需要若干天时间，因此主节点补丁版本在应用于主节点之前可能会显示为可用。自动更新还会跳过运行状况欠佳或当前有操作正在执行的集群。有时，IBM 可能会对特定主节点修订包禁用自动更新（如更改日志中所注释），例如仅当主节点从一个次版本更新到另一个次版本时才需要的补丁。在上述任何情况下，您都可以选择自行安全地使用 `ibmcloud ks cluster-update` [命令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_update)，而无需等待应用自动更新。

</br>

{: #prep-up}
以下信息总结了在将集群从先前版本更新到新版本时，可能会对已部署应用程序产生影响的更新。
-  V1.14 [准备操作](#cs_v114)。
-  V1.13 [准备操作](#cs_v113)。
-  V1.12 [准备操作](#cs_v112)。
-  **不推荐**：V1.11 [准备操作](#cs_v111)。
-  对不支持的版本[归档](#k8s_version_archive)。

<br/>

有关完整的更改列表，请查看以下信息：
* [Kubernetes 更改日志 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG.md)。
* [IBM 版本更改日志](/docs/containers?topic=containers-changelog)。

</br>

## 发布历史记录
{: #release-history}

下表记录了 {{site.data.keyword.containerlong_notm}} 版本发布历史记录。您可以将这些信息用于规划目的，例如估算在某个发行版可能变为不受支持时的常规时间范围。Kubernetes 社区发布版本更新后，IBM 团队即会开始针对 {{site.data.keyword.containerlong_notm}} 环境对该发行版进行强化和测试。可用和不受支持的发布日期取决于这些测试的结果、社区更新、安全补丁和版本之间的技术更改。请根据 `n-2` 版本支持策略来计划更新，以使集群主节点和工作程序节点版本保持最新。
{: shortdesc}

{{site.data.keyword.containerlong_notm}} 最早是在 Kubernetes V1.5 中一般可用。预计的发布或不受支持的日期会随时更改。要转至版本更新准备步骤，请单击相应版本号。

标记有短剑 (`†`) 的日期表示这是暂定时间，会随时更改。
{: important}

<table summary="此表显示了 {{site.data.keyword.containerlong_notm}} 的发布历史记录。">
<caption>{{site.data.keyword.containerlong_notm}} 的发布历史记录。</caption>
<col width="20%" align="center">
<col width="20%">
<col width="30%">
<col width="30%">
<thead>
<tr>
<th>是否受支持？</th>
<th>版本</th>
<th>{{site.data.keyword.containerlong_notm}}<br>发布日期</th>
<th>{{site.data.keyword.containerlong_notm}}<br>不受支持的日期</th>
</tr>
</thead>
<tbody>
<tr>
  <td><img src="images/checkmark-filled.png" align="left" width="32" style="width:32px;" alt="此版本受支持。"/></td>
  <td>[1.14](#cs_v114)</td>
  <td>2019 年 5 月 7 日</td>
  <td>2020 年 3 月 `†`</td>
</tr>
<tr>
  <td><img src="images/checkmark-filled.png" align="left" width="32" style="width:32px;" alt="此版本受支持。"/></td>
  <td>[1.13](#cs_v113)</td>
  <td>2019 年 2 月 5 日</td>
  <td>2019 年 12 月 `†`</td>
</tr>
<tr>
  <td><img src="images/checkmark-filled.png" align="left" width="32" style="width:32px;" alt="此版本受支持。"/></td>
  <td>[1.12](#cs_v112)</td>
  <td>2018 年 11 月 7 日</td>
  <td>2019 年 9 月 `†`</td>
</tr>
<tr>
  <td><img src="images/warning-filled.png" align="left" width="32" style="width:32px;" alt="此版本不推荐使用。"/></td>
  <td>[1.11](#cs_v111)</td>
  <td>2018 年 8 月 14 日</td>
  <td>2019 年 6 月 27 日 `†`</td>
</tr>
<tr>
  <td><img src="images/close-filled.png" align="left" width="32" style="width:32px;" alt="此版本不受支持。"/></td>
  <td>[1.10](#cs_v110)</td>
  <td>2018 年 5 月 1 日</td>
  <td>2019 年 5 月 16 日</td>
</tr>
<tr>
  <td><img src="images/close-filled.png" align="left" width="32" style="width:32px;" alt="此版本不受支持。"/></td>
  <td>[1.9](#cs_v19)</td>
  <td>2018 年 2 月 8 日</td>
  <td>2018 年 12 月 27 日</td>
</tr>
<tr>
  <td><img src="images/close-filled.png" align="left" width="32" style="width:32px;" alt="此版本不受支持。"/></td>
  <td>[1.8](#cs_v18)</td>
  <td>2017 年 11 月 8 日</td>
  <td>2018 年 9 月 22 日</td>
</tr>
<tr>
  <td><img src="images/close-filled.png" align="left" width="32" style="width:32px;" alt="此版本不受支持。"/></td>
  <td>[1.7](#cs_v17)</td>
  <td>2017 年 9 月 19 日</td>
  <td>2018 年 6 月 21 日</td>
</tr>
<tr>
  <td><img src="images/close-filled.png" align="left" width="32" style="width:32px;" alt="此版本不受支持。"/></td>
  <td>1.6</td>
  <td>不适用</td>
  <td>不适用</td>
</tr>
<tr>
  <td><img src="images/close-filled.png" align="left" width="32" style="width:32px;" alt="此版本不受支持。"/></td>
  <td>[1.5](#cs_v1-5)</td>
  <td>2017 年 5 月 23 日</td>
  <td>2018 年 4 月 4 日</td>
</tr>
</tbody>
</table>

<br />


## V1.14
{: #cs_v114}

<p><img src="images/certified_kubernetes_1x14.png" style="padding-right: 10px;" align="left" alt="此角标指示 {{site.data.keyword.containerlong_notm}} 的 Kubernetes V1.14 证书。"/> {{site.data.keyword.containerlong_notm}} 是 CNCF Kubernetes Software Conformance Certification 计划下经认证的 V1.14 的 Kubernetes 产品。_Kubernetes® 是 Linux Foundation 在美国和其他国家或地区的注册商标，并根据 Linux Foundation 的许可证进行使用。_</p>

查看 Kubernetes 从先前版本更新到 1.14 时可能需要进行的更改。
{: shortdesc}

Kubernetes 1.14 引入了新的功能供您探索。请试用新的 [`kustomize` 项目 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes-sigs/kustomize)，此项目可用于帮助编写、定制和复用 Kubernetes 资源 YAML 配置。或者，请查看新的 [`kubectl` CLI 文档 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubectl.docs.kubernetes.io/)。
{: tip}

### 在更新主节点之前更新
{: #114_before}

下表说明了在更新 Kubernetes 主节点之前必须执行的操作。
{: shortdesc}

<table summary="适用于 V1.14 的 Kubernetes 更新">
<caption>在将主节点更新到 Kubernetes 1.14 之前要进行的更改</caption>
<thead>
<tr>
<th>类型</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>CRI pod 日志目录结构更改</td>
<td>容器运行时接口 (CRI) 将 pod 日志目录结构从 `/var/log/pods/<UID>` 更改为 `/var/log/pods/<NAMESPACE_NAME_UID>`。如果应用程序绕过 Kubernetes 和 CRI 直接访问工作程序节点上的 pod 日志，请更新应用程序以处理这两个目录结构。经由 Kubernetes 访问 pod 日志（例如，通过运行 `kubectl logs`）不受此更改的影响。</td>
</tr>
<tr>
<td>运行状况检查不再执行重定向</td>
<td>使用 `HTTPGetAction` 的运行状况检查活性和就绪性探测器不再执行转至与原始探测器请求不同的主机名的重定向。这些非本地重定向会改为返回 `Success` 响应，并且会生成原因为 `ProbeWarning` 的事件，以指示忽略了重定向。如果先前依赖重定向来针对不同的主机名端点运行运行状况检查，那么必须在 `kubelet` 外部执行运行状况检查逻辑。例如，可以代理外部端点，而不是重定向探测器请求。</td>
</tr>
<tr>
<td>不受支持：KubeDNS 集群 DNS 提供程序</td>
<td>现在，对于运行 Kubernetes V1.14 和更高版本的集群，CoreDNS 是唯一支持的集群 DNS 提供程序。如果将使用 KubeDNS 作为集群 DNS 提供程序的现有集群更新为 V1.14，那么在更新期间会自动将 KubeDNS 迁移到 CoreDNS。因此，在更新集群之前，请考虑[将 CoreDNS 设置为集群 DNS 提供程序](/docs/containers?topic=containers-cluster_dns#set_coredns)，并对其进行测试。<br><br>CoreDNS 支持[集群 DNS 规范 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/dns/blob/master/docs/specification.md#25---records-for-external-name-services) 以输入域名作为 Kubernetes 服务 `ExternalName` 字段。先前的集群 DNS 提供程序 KubeDNS 不遵循集群 DNS 规范，因此允许将 IP 地址用于 `ExternalName`。如果任何 Kubernetes 服务使用的是 IP 地址，而不是 DNS，那么必须将 `ExternalName` 更新为 DNS 才能继续使用此功能。</td>
</tr>
<tr>
<td>不受支持：Kubernetes `Initializers` Alpha 功能</td>
<td>除去了 Kubernetes `Initializers` Alpha 功能、`admissionregistration.k8s.io/v1alpha` API 版本、`Initializers` 许可控制器插件以及 `metadata.initializers` API 字段的使用。如果使用 `Initializers`，请在更新集群之前，先切换为使用 [Kubernetes 许可 Webhook ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/reference/access-authn-authz/extensible-admission-controllers/)，并删除所有现有 `InitializerConfiguration` API 对象。</td>
</tr>
<tr>
<td>不受支持：Node Alpha 污点</td>
<td>不再支持使用 `node.alpha.kubernetes.io/notReady` 和 `node.alpha.kubernetes.io/unreachable` 污点。如果您依赖于这些污点，请更新应用程序以改为使用 `node.kubernetes.io/not-ready` 和 `node.kubernetes.io/unreachable` 污点。</td>
</tr>
<tr>
<td>不受支持：Kubernetes API Swagger 文档</td>
<td>现在，除去了 `swagger/*`、`/swagger.json` 和 `/swagger-2.0.0.pb-v1` 模式 API 文档，而支持使用 `/openapi/v2` 模式 API 文档。OpenAPI 文档在 Kubernetes V1.10 中可用后，已不推荐使用 Swagger 文档。此外，现在 Kubernetes API 服务器仅从聚集的 API 服务器的 `/openapi/v2` 端点中聚集 OpenAPI 模式。除去了从 `/swagger.json` 进行聚集的回退。如果已安装提供 Kubernetes API 扩展的应用程序，请确保应用程序支持 `/openapi/v2` 模式 API 文档。</td>
</tr>
<tr>
<td>不受支持和不推荐使用：选择度量值</td>
<td>查看[已除去和不推荐使用的 Kubernetes 度量值 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG-1.14.md#removed-and-deprecated-metrics)。如果要使用其中任何不推荐的度量值，请更改为使用可用的替换度量值。</td>
</tr>
</tbody>
</table>

### 在更新主节点之后更新
{: #114_after}

下表说明了在更新 Kubernetes 主节点之后必须执行的操作。
{: shortdesc}

<table summary="适用于 V1.14 的 Kubernetes 更新">
<caption>在将主节点更新到 Kubernetes 1.14 之后要进行的更改</caption>
<thead>
<tr>
<th>类型</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>不受支持：`kubectl --show-all`</td>
<td>不再支持 `--show-all` 和简写的 `-a` 标志。如果脚本依赖于这两个标志，请更新这些脚本。</td>
</tr>
<tr>
<td>用于未认证用户的 Kubernetes 缺省 RBAC 策略</td>
<td>Kubernetes 缺省基于角色的访问控制 (RBAC) 策略不再[授予未经认证的用户对发现和许可权检查 API 的访问权 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#discovery-roles)。此更改仅适用于新的 V1.14 集群。如果从先前版本更新集群，那么未经认证的用户仍有权访问发现和许可权检查 API。如果要更新为对未经认证的用户使用更安全的缺省值，请从 `system:basic-user` 和 `system:discovery` 集群角色绑定中除去 `system:unauthenticated` 组。</td>
</tr>
<tr>
<td>不推荐：使用 `pod_name` 和 `container_name` 标签的 Prometheus 查询</td>
<td>更新与 `pod_name` 或 `container_name` 标签相匹配的任何 Prometheus 查询，以改为使用 `pod` 或 `container` 标签。可能使用这些不推荐标签的示例查询包括 kubelet 探测器度量值。不推荐使用的 `pod_name` 和 `container_name` 标签在下一个 Kubernetes 发行版中即不再予以支持。</td>
</tr>
</tbody>
</table>

<br />


## V1.13
{: #cs_v113}

<p><img src="images/certified_kubernetes_1x13.png" style="padding-right: 10px;" align="left" alt="此角标指示 {{site.data.keyword.containerlong_notm}} 的 Kubernetes V1.13 证书。"/> {{site.data.keyword.containerlong_notm}} 是 CNCF Kubernetes Software Conformance Certification 计划下经认证的 V1.13 的 Kubernetes 产品。_Kubernetes® 是 Linux Foundation 在美国和其他国家或地区的注册商标，并根据 Linux Foundation 的许可证进行使用。_</p>

查看 Kubernetes 从先前版本更新到 V1.13 时可能需要进行的更改。
{: shortdesc}

### 在更新主节点之前更新
{: #113_before}

下表说明了在更新 Kubernetes 主节点之前必须执行的操作。
{: shortdesc}

<table summary="适用于 V1.13 的 Kubernetes 更新">
<caption>在将主节点更新到 Kubernetes 1.13 之前要进行的更改</caption>
<thead>
<tr>
<th>类型</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>不适用</td>
<td></td>
</tr>
</tbody>
</table>

### 在更新主节点之后更新
{: #113_after}

下表说明了在更新 Kubernetes 主节点之后必须执行的操作。
{: shortdesc}

<table summary="适用于 V1.13 的 Kubernetes 更新">
<caption>在将主节点更新到 Kubernetes 1.13 之后要进行的更改</caption>
<thead>
<tr>
<th>类型</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>CoreDNS 作为新的缺省集群 DNS 提供程序可用</td>
<td>现在，在 Kubernetes 1.13 和更高版本中，CoreDNS 是新集群的缺省集群 DNS 提供程序。如果将使用 KubeDNS 作为集群 DNS 提供程序的现有集群更新为 1.13，那么 KubeDNS 会继续作为集群 DNS 提供程序。但是，您可以选择[改为使用 CoreDNS](/docs/containers?topic=containers-cluster_dns#dns_set)。<br><br>CoreDNS 支持[集群 DNS 规范 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/dns/blob/master/docs/specification.md#25---records-for-external-name-services) 以输入域名作为 Kubernetes 服务 `ExternalName` 字段。先前的集群 DNS 提供程序 KubeDNS 不遵循集群 DNS 规范，因此允许将 IP 地址用于 `ExternalName`。如果任何 Kubernetes 服务使用的是 IP 地址，而不是 DNS，那么必须将 `ExternalName` 更新为 DNS 才能继续使用此功能。</td>
</tr>
<tr>
<td>`Deployment` 和 `StatefulSet` 的 `kubectl` 输出</td>
<td>现在，`Deployment` 和 `StatefulSet` 的 `kubectl` 输出包含 `Ready` 列，并且更容易供人类阅读。如果脚本依赖于先前的行为，请更新这些脚本。</td>
</tr>
<tr>
<td>`PriorityClass` 的 `kubectl` 输出</td>
<td>现在，`PriorityClass` 的 `kubectl` 输出包含 `Value` 列。如果脚本依赖于先前的行为，请更新这些脚本。</td>
</tr>
<tr>
<td>`kubectl get componentstatuses`</td>
<td>`kubectl get componentstatuses` 命令未正确报告某些 Kubernetes 主节点组件的运行状况，因为既然 `localhost` 和不安全 (HTTP) 端口已禁用，因此这些组件不再可通过 Kubernetes API 服务器进行访问。在 Kubernetes V1.10 中引入高可用性 (HA) 主节点后，每个 Kubernetes 主节点都设置有多个 `apiserver`、`controller-manager`、`scheduler` 和 `etcd` 实例。请改为通过检查 [{{site.data.keyword.Bluemix_notm}} 控制台 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/kubernetes/landing) 或使用 `ibmcloud ks cluster-get` [命令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_get)来查看集群运行状况。</td>
</tr>
<tr>
<tr>
<td>不受支持：`kubectl run-container`</td>
<td>`kubectl run-container` 命令已除去。请改为使用 `kubectl run` 命令。</td>
</tr>
<tr>
<td>`kubectl rollout undo`</td>
<td>对不存在的修订版运行 `kubectl rollout undo` 时，将返回错误。如果脚本依赖于先前的行为，请更新这些脚本。</td>
</tr>
<tr>
<td>不推荐：`scheduler.alpha.kubernetes.io/critical-pod` 注释</td>
<td>现在，不推荐使用 `scheduler.alpha.kubernetes.io/critical-pod` 注释。请将依赖于此注释的任何 pod 更改为使用 [pod 优先级](/docs/containers?topic=containers-pod_priority#pod_priority)。</td>
</tr>
</tbody>
</table>

### 在更新工作程序节点之后的更新
{: #113_after_workers}

下表说明了在更新工作程序节点之后必须执行的操作。
{: shortdesc}

<table summary="适用于 V1.13 的 Kubernetes 更新">
<caption>在将工作程序节点更新到 Kubernetes 1.13 之后要进行的更改</caption>
<thead>
<tr>
<th>类型</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>containerd `cri` 流服务器</td>
<td>在 containerd V1.2 中，现在 `cri` 插件流服务器在随机端口 `http://localhost:0` 上提供服务。此更改支持 `kubelet` 流式代理，并为容器 `exec` 和 `logs` 操作提供了更安全的流接口。先前，`cri` 流服务器使用端口 10010 侦听工作程序节点的专用网络接口。如果应用程序使用的是 containerd `cri` 插件并依赖于先前的行为，请对其进行更新。</td>
</tr>
</tbody>
</table>

<br />


## V1.12
{: #cs_v112}

<p><img src="images/certified_kubernetes_1x12.png" style="padding-right: 10px;" align="left" alt="此角标指示 IBM Cloud Container Service 的 Kubernetes V1.12 证书。"/> {{site.data.keyword.containerlong_notm}} 是 CNCF Kubernetes Software Conformance Certification 计划下经认证的 V11.2 的 Kubernetes 产品。_Kubernetes® 是 Linux Foundation 在美国和其他国家或地区的注册商标，并根据 Linux Foundation 的许可证进行使用。_</p>

查看 Kubernetes 从先前版本更新到 V1.12 时可能需要进行的更改。
{: shortdesc}

### 在更新主节点之前更新
{: #112_before}

下表说明了在更新 Kubernetes 主节点之前必须执行的操作。
{: shortdesc}

<table summary="适用于 V1.12 的 Kubernetes 更新">
<caption>在将主节点更新到 Kubernetes 1.12 之前要进行的更改</caption>
<thead>
<tr>
<th>类型</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes Metrics Server</td>
<td>如果当前已在集群中部署了 Kubernetes `metric-server`，那么在将集群更新为 Kubernetes 1.12 之前，必须先除去 `metric-server`。此除去操作可防止与更新期间部署的 `metric-server` 发生冲突。</td>
</tr>
<tr>
<td>`kube-system` `default` 服务帐户的角色绑定</td>
<td>`kube-system` `default` 服务帐户不再具有对 Kubernetes API 的 **cluster-admin** 访问权。如果部署需要访问集群中的进程的功能或附加组件（例如 [Helm](/docs/containers?topic=containers-helm#public_helm_install)），请设置[服务帐户 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/reference/access-authn-authz/service-accounts-admin/)。如果您需要时间来创建和设置具有相应许可权的单个服务帐户，那么可以使用以下集群角色绑定临时授予 **cluster-admin** 角色：`kubectl create clusterrolebinding kube-system:default --clusterrole=cluster-admin --serviceaccount=kube-system:default`</td>
</tr>
</tbody>
</table>

### 在更新主节点之后更新
{: #112_after}

下表说明了在更新 Kubernetes 主节点之后必须执行的操作。
{: shortdesc}

<table summary="适用于 V1.12 的 Kubernetes 更新">
<caption>在将主节点更新到 Kubernetes 1.12 之后要进行的更改</caption>
<thead>
<tr>
<th>类型</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes API</td>
<td>Kubernetes API 替换了如下不推荐使用的 API：
<ul><li><strong>apps/v1</strong>：`apps/v1` Kubernetes API 替换了 `apps/v1beta1` 和 `apps/v1alpha` API。此外，`apps/v1` API 还替换了用于 `daemonset`、`deployment`、`replicaset` 和 `statefulset` 资源的 `extensions/v1beta1` API。Kubernetes 项目不推荐使用对 Kubernetes `apiserver` 和 `kubectl` 客户机的先前 API 的支持，此支持将分阶段停止。</li>
<li><strong>networking.k8s.io/v1</strong>：`networking.k8s.io/v1` API 替换了用于 NetworkPolicy 资源的 `extensions/v1beta1` API。</li>
<li><strong>policy/v1beta1</strong>：`policy/v1beta1` API 替换了用于 `podsecuritypolicy` 资源的 `extensions/v1beta1` API。</li></ul>
<br><br>在不推荐使用的 API 变为不受支持之前，请更新所有 YAML `apiVersion` 字段以使用相应的 Kubernetes API。此外，请查看 [Kubernetes 文档 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)，以了解与 `apps/v1` 相关的更改，例如：<ul><li>创建部署后，`.spec.selector` 字段是不可变的。</li>
<li>不推荐使用 `.spec.rollbackTo` 字段。请改为使用 `kubectl rollout undo` 命令。</li></ul></td>
</tr>
<tr>
<td>作为集群 DNS 提供程序提供的 CoreDNS</td>
<td>Kubernetes 项目正在过渡为支持 CoreDNS，以取代当前的 Kubernetes DNS (KubeDNS)。在 V1.12 中，缺省集群 DNS 仍然为 KubeDNS，但您可以[选择使用 CoreDNS](/docs/containers?topic=containers-cluster_dns#dns_set)。</td>
</tr>
<tr>
<td>`kubectl apply --force`</td>
<td>现在，对无法更新的资源（例如，YAML 文件中的不可变字段）强制执行应用操作 (`kubectl apply --force`) 时，会改为重新创建这些资源。如果脚本依赖于先前的行为，请更新这些脚本。</td>
</tr>
<tr>
<td>`kubectl get componentstatuses`</td>
<td>`kubectl get componentstatuses` 命令未正确报告某些 Kubernetes 主节点组件的运行状况，因为既然 `localhost` 和不安全 (HTTP) 端口已禁用，因此这些组件不再可通过 Kubernetes API 服务器进行访问。在 Kubernetes V1.10 中引入高可用性 (HA) 主节点后，每个 Kubernetes 主节点都设置有多个 `apiserver`、`controller-manager`、`scheduler` 和 `etcd` 实例。请改为通过检查 [{{site.data.keyword.Bluemix_notm}} 控制台 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/kubernetes/landing) 或使用 `ibmcloud ks cluster-get` [命令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_get)来查看集群运行状况。</td>
</tr>
<tr>
<td>`kubectl logs --interactive`</td>
<td>`kubectl logs` 不再支持 `--interactive` 标志。请更新使用此标志的任何自动化。</td>
</tr>
<tr>
<td>`kubectl patch`</td>
<td>如果 `patch` 命令未实现任何更改（冗余补丁），那么该命令不会再以返回码 `1` 退出。如果脚本依赖于先前的行为，请更新这些脚本。</td>
</tr>
<tr>
<td>`kubectl version -c`</td>
<td>不再支持 `-c` 简写标志。请改为使用完整的 `--client` 标志。请更新使用此标志的任何自动化。</td>
</tr>
<tr>
<td>`kubectl wait`</td>
<td>现在，如果找不到匹配的选择器，那么该命令将显示错误消息并以返回码 `1` 退出。如果脚本依赖于先前的行为，请更新这些脚本。</td>
</tr>
<tr>
<td>kubelet cAdvisor port</td>
<td>从 Kubernetes 1.12 中除去了 kubelet 通过启动 `--cadvisor-port` 来使用的 [Container Advisor (cAdvisor) ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/google/cadvisor) Web UI。如果仍然需要运行 cAdvisor，请[将 cAdvisor 部署为守护程序集 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/google/cadvisor/tree/master/deploy/kubernetes)。<br><br>在守护程序集内，指定 ports 部分，以便可通过 `http://node-ip:4194` 来访问 cAdvisor，如下所示。在将工作程序节点更新为 1.12 之前，cAdvisor pod 始终会发生故障，因为更低版本的 kuelet 是将主机端口 4194 用于 cAdvisor。
<pre class="screen"><code>ports:
          - name: http
            containerPort: 8080
            hostPort: 4194
            protocol: TCP</code></pre></td>
</tr>
<tr>
<td>Kubernetes 仪表板</td>
<td>如果您通过 `kubectl proxy` 来访问仪表板，那么会除去登录页面上的**跳过**按钮。请改为[使用**令牌**进行登录](/docs/containers?topic=containers-app#cli_dashboard)。</td>
</tr>
<tr>
<td id="metrics-server">Kubernetes Metrics Server</td>
<td>Kubernetes Metrics Server 将替换作为集群度量值提供程序的 Kubernetes Heapster（自 Kubernetes V1.8 开始不推荐使用）。如果在集群中每个工作程序节点上运行的 pod 超过 30 个，请[调整 `metrics-server` 配置以提高性能](/docs/containers?topic=containers-kernel#metrics)。
<p>Kubernetes 仪表板无法用于 `metrics-server`。如果要在仪表板中显示度量值，请从以下选项中进行选择。</p>
<ul><li>通过使用集群监视仪表板，[设置 Grafana 来分析度量值](/docs/services/cloud-monitoring/tutorials?topic=cloud-monitoring-container_service_metrics#container_service_metrics)。</li>
<li>将 [Heapster ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/heapster) 部署到集群。
<ol><li>复制 `heapster-rbac` [YAML ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/kubernetes/blob/release-1.12/cluster/addons/cluster-monitoring/heapster-rbac.yaml)、`heapster-service` [YAML ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/kubernetes/blob/release-1.12/cluster/addons/cluster-monitoring/standalone/heapster-service.yaml) 和 `heapster-controller` [YAML ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/kubernetes/blob/release-1.12/cluster/addons/cluster-monitoring/standalone/heapster-controller.yaml) 文件。</li>
<li>编辑 `heapter-controller` YAML 并替换以下字符串。
<ul><li>将 `{{ nanny_memory }}` 替换为 `90Mi`</li>
<li>将 `{{ base_metrics_cpu }}` 替换为 `80m`</li>
<li>将 `{{ metrics_cpu_per_node }}` 替换为 `0.5m`</li>
<li>将 `{{ base_metrics_memory }}` 替换为 `140Mi`</li>
<li>将 `{{ metrics_memory_per_node }}` 替换为 `4Mi`</li>
<li>将 `{{ heapster_min_cluster_size }}` 替换为 `16`</li></ul></li>
<li>通过运行 `kubectl apply -f` 命令，将 `heapster-rbac`、`heapster-service` 和 `heapster-controller` YAML 文件应用于集群。</li></ol></li></ul></td>
</tr>
<tr>
<td>`rbac.authorization.k8s.io/v1` Kubernetes API</td>
<td>`rbac.authorization.k8s.io/v1` Kubernetes API（自 Kubernetes 1.8 开始受支持）将替换 `rbac.authorization.k8s.io/v1alpha1` 和 `rbac.authorization.k8s.io/v1beta1` API。您不能再使用不受支持的 `v1alpha` API 来创建 RBAC 对象，例如角色或角色绑定。现有 RBAC 对象将转换为 `v1` API。</td>
</tr>
</tbody>
</table>

<br />


## 不推荐：V1.11
{: #cs_v111}

<p><img src="images/certified_kubernetes_1x11.png" style="padding-right: 10px;" align="left" alt="此角标指示 IBM Cloud Container Service 的 Kubernetes V1.11 证书。"/> {{site.data.keyword.containerlong_notm}} 是 CNCF Kubernetes Software Conformance Certification 计划下经认证的 V11.1 的 Kubernetes 产品。_Kubernetes® 是 Linux Foundation 在美国和其他国家或地区的注册商标，并根据 Linux Foundation 的许可证进行使用。_</p>

查看 Kubernetes 从先前版本更新到 V1.11 时可能需要进行的更改。
{: shortdesc}

Kubernetes V1.11 已不推荐使用，到 2019 年 6 月 27 日（暂定）即不再予以支持。对于每个 Kubernetes 版本更新，请[查看潜在影响](/docs/containers?topic=containers-cs_versions#cs_versions)，然后立即[更新集群](/docs/containers?topic=containers-update#update)，并且至少更新到 1.12。
{: deprecated}

必须执行[准备更新到 Calico V3](#111_calicov3) 中列出的步骤后，才能成功将集群从 Kubernetes V1.9 或更低版本更新到 V1.11。
{: important}

### 在更新主节点之前更新
{: #111_before}

下表说明了在更新 Kubernetes 主节点之前必须执行的操作。
{: shortdesc}

<table summary="适用于 V1.11 的 Kubernetes 更新">
<caption>在将主节点更新到 Kubernetes 1.11 之前要进行的更改</caption>
<thead>
<tr>
<th>类型</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>集群主节点高可用性 (HA) 配置</td>
<td>更新了集群主节点配置以提高高可用性 (HA)。集群现在设置有三个 Kubernetes 主节点副本，其中每个主节点副本部署到单独的物理主机上。此外，如果集群位于支持多专区的专区中，那么这些主节点还将在各专区中进行分布。<br><br>有关必须执行的操作，请参阅[更新为高可用性集群主节点](#ha-masters)。这些准备操作适用于以下情况：<ul>
<li>如果具有防火墙或定制 Calico 网络策略。</li>
<li>如果在工作程序节点上使用的主机端口是 `2040` 或 `2041`。</li>
<li>如果使用了集群主节点 IP 地址对主节点进行集群内访问。</li>
<li>如果具有调用 Calico API 或 CLI (`calicoctl`) 的自动化操作，例如创建 Calico 策略。</li>
<li>如果使用 Kubernetes 或 Calico 网络策略来控制对主节点的 pod 流出访问。</li></ul></td>
</tr>
<tr>
<td>`containerd`：新的 Kubernetes 容器运行时</td>
<td><p class="important">`containerd` 将替换 Docker 来作为 Kubernetes 的新容器运行时。有关必须执行的操作，请参阅[更新为使用 `containerd` 作为容器运行时](#containerd)。</p></td>
</tr>
<tr>
<td>加密 etcd 中的数据</td>
<td>先前，etcd 数据存储在静态加密的主节点 NFS 文件存储器实例上。现在，etcd 数据存储在主节点的本地磁盘上，并备份到 {{site.data.keyword.cos_full_notm}}。数据在传输到 {{site.data.keyword.cos_full_notm}} 期间和处于静态时会进行加密。但是，不会对主节点本地磁盘上的 etcd 数据进行加密。如果要对主节点的本地 etcd 数据进行加密，请[在集群中启用 {{site.data.keyword.keymanagementservicelong_notm}}](/docs/containers?topic=containers-encryption#keyprotect)。</td>
</tr>
<tr>
<td>Kubernetes 容器卷安装传播</td>
<td>容器 `VolumeMount` 的 [`mountPropagation` 字段 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/storage/volumes/#mount-propagation) 的缺省值已从 `HostToContainer` 更改为 `None`。此更改复原在 Kubernetes V1.9 以及更低版本中存在的行为。如果 pod 规范依赖于 `HostToContainer` 成为缺省值，请进行更新。</td>
</tr>
<tr>
<td>Kubernetes API 服务器 JSON 反序列化器</td>
<td>Kubernetes API 服务器 JSON 反序列化器现在区分大小写。此更改复原在 Kubernetes V1.7 以及更低版本中存在的行为。如果 JSON 资源定义使用不正确的大小写，请进行更新。<br><br>仅影响直接 Kubernetes API 服务器请求。`kubectl` CLI 继续在 Kubernetes V1.7 以及更高版本中强制实施区分大小写的密钥，因此如果使用 `kubectl` 严格管理资源，那么您不会受到影响。</td>
</tr>
</tbody>
</table>

### 在更新主节点之后更新
{: #111_after}

下表说明了在更新 Kubernetes 主节点之后必须执行的操作。
{: shortdesc}

<table summary="适用于 V1.11 的 Kubernetes 更新">
<caption>在将主节点更新到 Kubernetes 1.11 之后要进行的更改</caption>
<thead>
<tr>
<th>类型</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>集群日志记录配置</td>
<td>`fluentd` 集群附加组件自动更新为 V1.11，即使禁用了 `logging-autoupdate`。<br><br>
容器日志目录已从 `/var/lib/docker/` 更改为 `/var/log/pods/`。如果使用自己的日志记录解决方案来监视先前目录，请相应更新。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM) 支持</td>
<td>运行 Kubernetes V1.11 或更高版本的集群支持 IAM [访问组](/docs/iam?topic=iam-groups#groups)和[服务标识](/docs/iam?topic=iam-serviceids#serviceids)。现在，可以使用这些功能来[授予对集群的访问权](/docs/containers?topic=containers-users#users)。</td>
</tr>
<tr>
<td>刷新 Kubernetes 配置</td>
<td>更新集群的 Kubernetes API 服务器的 OpenID Connect 配置以支持 {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM) 访问组。因此，在通过运行 `ibmcloud ks cluster-config --cluster <cluster_name_or_ID>` 更新主 Kubernetes v1.11 后，必须刷新集群的 Kubernetes 配置。使用此命令时，配置将应用于 `default` 名称空间中的角色绑定。<br><br>如果不刷新配置，那么集群操作将失败，并显示以下错误消息：`You must be logged in to the server (Unauthorized).`</td>
</tr>
<tr>
<td>Kubernetes 仪表板</td>
<td>如果您通过 `kubectl proxy` 来访问仪表板，那么会除去登录页面上的**跳过**按钮。请改为[使用**令牌**进行登录](/docs/containers?topic=containers-app#cli_dashboard)。</td>
</tr>
<tr>
<td>`kubectl` CLI</td>
<td>`kubectl` CLI for Kubernetes V1.11 需要 `apps/v1` API。因此，V1.11 `kubectl` CLI 不适用于运行 Kubernetes V1.8 或更低版本的集群。使用与集群的 Kubernetes API 服务器版本相匹配的 `kubectl` CLI 的版本。</td>
</tr>
<tr>
<td>`kubectl auth can-i`</td>
<td>现在，如果未授权用户，那么 `kubectl auth can-i` 命令失败，并返回 `exit code 1`。如果脚本依赖于先前的行为，请进行更新。</td>
</tr>
<tr>
<td>`kubectl delete`</td>
<td>现在，在使用选择条件（例如，标签）删除资源时，缺省情况下，`kubectl delete` 命令忽略 `not found` 错误。如果脚本依赖于先前的行为，请更新这些脚本。</td>
</tr>
<tr>
<td>Kubernetes `sysctls` 功能</td>
<td>现在会忽略 `security.alpha.kubernetes.io/sysctls` 注释。相反，Kubernetes 向 `PodSecurityPolicy` 和 `Pod` 对象添加字段以指定和控制 `sysctls`。有关更多信息，请参阅[在 Kubernetes 中使用 sysctls ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/tasks/administer-cluster/sysctl-cluster/)。<br><br>在更新集群主节点和工作程序后，更新 `PodSecurityPolicy` 和 `Pod` 对象以使用新的 `sysctls` 字段。</td>
</tr>
</tbody>
</table>

### 在 Kubernetes 1.11 中更新为高可用性集群主节点
{: #ha-masters}

对于运行 Kubernetes V1.10.8_1530、V1.11.3_1531 或更高版本的集群，将更新集群主节点配置以提高高可用性 (HA)。集群现在设置有三个 Kubernetes 主节点副本，其中每个主节点副本部署到单独的物理主机上。此外，如果集群位于支持多专区的专区中，那么这些主节点还将在各专区中进行分布。
{: shortdesc}

可以通过在控制台中检查集群的主节点 URL，或者通过运行 `ibmcloud ks cluster-get --cluster <cluster_name_or_ID`，检查集群是否具有 HA 主节点配置。如果主节点 URL 具有主机名（例如，`https://c2.us-south.containers.cloud.ibm.com:xxxxx`），而不是 IP 地址（例如，`https://169.xx.xx.xx:xxxxx`），说明集群具有 HA 主节点配置。您可能会因为自动主节点补丁更新或手动应用更新而获得 HA 主节点配置。在任一情况下，您仍必须复查以下各项，以确保集群网络设置为充分利用配置。

* 如果具有防火墙或定制 Calico 网络策略。
* 如果在工作程序节点上使用的主机端口是 `2040` 或 `2041`。
* 如果使用了集群主节点 IP 地址对主节点进行集群内访问。
* 如果具有调用 Calico API 或 CLI (`calicoctl`) 的自动化操作，例如创建 Calico 策略。
* 如果使用 Kubernetes 或 Calico 网络策略来控制对主节点的 pod 流出访问。

<br>
**针对 HA 主节点更新防火墙或定制 Calico 主机网络策略**：</br>
{: #ha-firewall}
如果使用防火墙或定制 Calico 主机网络策略来控制来自工作程序节点的流出流量，请允许出局流量流至集群所在区域内所有专区的端口和 IP 地址。请参阅[允许集群访问基础架构资源和其他服务](/docs/containers?topic=containers-firewall#firewall_outbound)。

<br>
**在工作程序节点上保留主机端口 `2040` 和 `2041`**：</br>
{: #ha-ports}
要允许访问高可用性配置中的集群主节点，必须使所有工作程序节点上的主机端口 `2040` 和 `2041` 保持可用。
* 将 `hostPort` 设置为 `2040` 或 `2041` 的任何 pod 更新为使用其他端口。
* 对于将 `hostNetwork` 设置为 `true` 且侦听端口 `2040` 或 `2041` 的任何 pod，将这些 pod 更新为使用其他端口。

要检查 pod 当前是否在使用端口 `2040` 或 `2041`，请将集群设定为目标，然后运行以下命令。

```
kubectl get pods --all-namespaces -o yaml | grep -B 3 "hostPort: 204[0,1]"
```
{: pre}

如果您已经具有 HA 主节点配置，那么会在 `kube-system` 名称空间中看到 `ibm-master-proxy-*` 的结果，例如以下示例中所示。如果返回其他 pod，请更新其端口。

```
name: ibm-master-proxy-static
ports:
- containerPort: 2040
  hostPort: 2040
  name: apiserver
  protocol: TCP
- containerPort: 2041
  hostPort: 2041
...
```
{: screen}


<br>
**将 `kubernetes` 服务集群 IP 或域用于对主节点的集群内访问**：</br>
{: #ha-incluster}
要从集群内访问 HA 配置中的集群主节点，请使用下列其中一项：
* `kubernetes` 服务集群 IP 地址，缺省情况下为：`https://172.21.0.1`
* `kubernetes` 服务域名，缺省情况下为：`https://kubernetes.default.svc.cluster.local`

如果先前使用了集群主节点 IP 地址，那么此方法将继续有效。但是，为了提高可用性，请更新为使用 `kubernetes` 服务集群 IP 地址或域名。

<br>
**配置 Calico 以对使用 HA 配置的主节点进行集群外访问**：</br>
{: #ha-outofcluster}
存储在 `kube-system` 名称空间的 `calico-config` 配置映射中的数据更改为支持 HA 主配置。特别是，`etcd_endpoints` 值现在仅支持集群内访问。使用此值来配置 Calico CLI 以支持从集群外部进行访问不再有效。

请改为使用存储在 `kube-system` 名称空间的 `cluster-info` 配置映射中的数据。特别是，使用 `etcd_host` 和 `etcd_port` 值来配置 [Calico CLI](/docs/containers?topic=containers-network_policies#cli_install) 的端点，以便从集群外部访问使用 HA 配置的主节点。

<br>
**更新 Kubernetes 或 Calico 网络策略**：</br>
{: #ha-networkpolicies}
如果使用 [Kubernetes 或 Calico 网络策略](/docs/containers?topic=containers-network_policies#network_policies)来控制对集群主节点的 pod 流出访问，并且当前正在使用以下各项，那么需要执行其他操作：
*  Kubernetes 服务集群 IP，此 IP 可以通过运行 `kubectl get service kubernetes -o yaml | grep clusterIP` 来获取。
*  Kubernetes 服务域名，缺省情况下为：`https://kubernetes.default.svc.cluster.local`。
*  集群主节点 IP，此 IP 可以通过运行 `kubectl cluster-info | grep Kubernetes` 来获取。

以下步骤描述了如何更新 Kubernetes 网络策略。如果要更新 Calico 网络策略，可重复这些步骤，但需要做一些微小的策略语法更改，并使用 `calicoctl` 来搜索策略以了解影响。
{: note}

开始之前：[登录到您的帐户。如果适用，请将相应的资源组设定为目标。为集群设置上下文。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  获取集群主节点 IP 地址。
    ```
    kubectl cluster-info | grep Kubernetes
    ```
    {: pre}

2.  搜索 Kubernetes 网络策略以了解影响。如果未返回任何 YAML，说明您的集群不受影响，您不需要进行其他更改。
    ```
    kubectl get networkpolicies --all-namespaces -o yaml | grep <cluster-master-ip>
    ```
    {: pre}

3.  复查 YAML。例如，如果集群使用以下 Kubernetes 网络策略来允许 `default` 名称空间中的 pod 通过 `kubernetes` 服务集群 IP 或集群主节点 IP 来访问集群主节点，那么必须更新该策略。
    ```
    apiVersion: networking.k8s.io/v1
    kind: NetworkPolicy
    metadata:
      name: all-master-egress
      namespace: default
    spec:
      egress:
      # Allow access to cluster master using kubernetes service cluster IP address
      # or domain name or cluster master IP address.
      -   ports:

        - protocol: TCP
        to:
        - ipBlock:
            cidr: 161.202.126.210/32
      # Allow access to Kubernetes DNS in order to resolve the kubernetes service
      # domain name.
      -   ports:

        - protocol: TCP
          port: 53
        - protocol: UDP
          port: 53
      podSelector: {}
      policyTypes:
      - Egress
    ```
    {: screen}

4.  修改 Kubernetes 网络策略，以允许流量流出到集群内主代理 IP 地址 `172.20.0.1`。目前，请保留集群主节点 IP 地址。例如，先前的网络策略示例将更改为以下值。

    如果先前设置了流出策略，以针对一个 Kubernetes 主节点，仅打开一个 IP 地址和端口，那么现在请使用集群内主代理 IP 地址范围 172.20.0.1/32 和端口 2040。
    {: tip}

    ```
    apiVersion: networking.k8s.io/v1
    kind: NetworkPolicy
    metadata:
      name: all-master-egress
      namespace: default
    spec:
      egress:
      # Allow access to cluster master using kubernetes service cluster IP address
      # or domain name.
      -   ports:

        - protocol: TCP
        to:
        - ipBlock:
            cidr: 172.20.0.1/32
        - ipBlock:
            cidr: 161.202.126.210/32
      # Allow access to Kubernetes DNS in order to resolve the kubernetes service domain name.
      -   ports:

        - protocol: TCP
          port: 53
        - protocol: UDP
          port: 53
      podSelector: {}
      policyTypes:
      - Egress
    ```
    {: screen}

5.  将修改后的网络策略应用于集群。
    ```
    kubectl apply -f all-master-egress.yaml
    ```
    {: pre}

6.  完成所有[准备操作](#ha-masters)（包括这些步骤）后，[更新集群主节点](/docs/containers?topic=containers-update#master)至 HA 主修订包。

7.  更新完成后，从网络策略中除去集群主节点 IP 地址。例如，从先前的网络策略中除去以下行，然后重新应用策略。

    ```
    - ipBlock:
        cidr: 161.202.126.210/32
    ```
    {: screen}

    ```
    kubectl apply -f all-master-egress.yaml
    ```
    {: pre}

### 更新为使用 `containerd` 作为容器运行时
{: #containerd}

对于运行 Kubernetes V1.11 或更高版本的集群，`containerd` 将替换 Docker 来作为 Kubernetes 的新容器运行时，以增强性能。如果 pod 依赖于 Docker 作为 Kubernetes 容器运行时，那么必须更新它们以将 `containerd` 作为容器运行时进行处理。有关更多信息，请参阅 [Kubernetes containerd 声明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/blog/2018/05/24/kubernetes-containerd-integration-goes-ga/)。
{: shortdesc}

**如何知道应用程序是否依赖于 `docker` 而不是 `containerd`？**<br>
依赖于 Docker 作为容器运行时的情况的示例：
*  如果使用特权容器直接访问 Docker 引擎或 API，那么更新 pod 以支持 `containerd` 作为运行时。例如，您可以直接调用 Docker 套接字以启动容器或执行其他 Docker 操作。Docker 套接字已从 `/var/run/docker.sock` 更改为 `/run/containerd/containerd.sock`。在 `containerd` 套接字中使用的协议与 Docker 中的协议略有不同。请尝试将应用程序更新为 `containerd` 套接字。如果要继续使用 Docker 套接字，请查看 [Docker-inside-Docker (DinD) ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://hub.docker.com/_/docker/) 的具体使用情况。
*  集群中安装的某些第三方附加组件（例如，日志记录和监视工具）可能依赖于 Docker 引擎。请检查提供程序以确保工具兼容 containerd。可能的用例包括：
   - 日志记录工具可能使用容器 `stderr/stdout` 目录 `/var/log/pods/<pod_uuid>/<container_name>/*.log` 来访问日志。在 Docker 中，此目录是 `/var/data/cripersistentstorage/containers/<container_uuid>/<container_uuid>-json.log` 的符号链接，而在 `containerd` 中，无需符号链接就可以直接访问此目录。
   - 监视工具直接访问 Docker 套接字。Docker 套接字已从 `/var/run/docker.sock` 更改为 `/run/containerd/containerd.sock`。

<br>

**除了依赖于运行时，是否还需要执行其他准备操作？**<br>

**清单工具**：如果您具有使用 Docker V18.06 之前的试验性 `docker manifest` [工具 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://docs.docker.com/edge/engine/reference/commandline/manifest/) 构建的多平台映像，那么无法使用 `containerd` 从 DockerHub 拉取映像。

在检查 pod 事件时，可能会看到如下错误。
```
failed size validation
```
{: screen}

要使用通过清单工具以及 `containerd` 构建的映像，请从以下选项中进行选择。

*  使用[清单工具 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/estesp/manifest-tool) 重新构建映像。
*  在更新为 Docker V18.06 或更高版本后，使用 `docker-manifest` 工具重新构建映像。

<br>

**什么不受影响？是否需要更改部署容器的方式？**<br>
通常，容器部署过程不会更改。您仍可以使用 Dockerfile 来定义 Docker 映像并针对应用程序构建 Docker 容器。如果使用 `docker` 命令来构建映像并将其推送到注册表，那么可继续使用 `docker` 或者改为使用 `ibmcloud cr` 命令。

### 准备更新到 Calico V3
{: #111_calicov3}

如果要将集群从 Kubernetes V1.9 或更低版本更新到 V1.11，请在更新主节点之前准备 Calico V3 更新。在将主节点升级到 Kubernetes V1.11 期间，不会安排新的 pod 和新的 Kubernetes 或 Calico 网络策略。更新阻止新安排的时间长短不同。小型集群可能需要几分钟，每 10 个节点需要额外增加几分钟时间。现有网络策略和 pod 会继续运行。
{: shortdesc}

如果要将集群从 Kubernetes V1.10 更新到 V1.11，请跳过这些步骤，因为在更新到 1.10 时已完成这些步骤。
{: note}

开始之前，集群主节点和所有工作程序节点都必须运行的是 Kubernetes V1.8 或 V1.9，并且必须至少有一个工作程序节点。

1.  验证 Calico pod 是否运行正常。
    ```
    kubectl get pods -n kube-system -l k8s-app=calico-node -o wide
    ```
    {: pre}

2.  如果任何 pod 未处于**正在运行**状态，请删除该 pod，并等到它处于**正在运行**状态后再继续。如果 pod 未恢复为**正在运行**状态：
    1.  检查工作程序节点的**状态**和**阶段状态**。
        ```
   ibmcloud ks workers --cluster <cluster_name_or_ID>
   ```
        {: pre}
    2.  如果工作程序节点的状态不是**正常**，请执行[调试工作程序节点](/docs/containers?topic=containers-cs_troubleshoot#debug_worker_nodes)步骤。例如，**临界**或**未知**状态通常可通过[重新装入工作程序节点](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)来解决。

3.  如果是自动生成的 Calico 策略或其他 Calico 资源，请更新自动化工具，以使用 [Calico V3 语法 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://docs.projectcalico.org/v3.1/reference/calicoctl/resources/) 生成这些资源。

4.  如果是将 [strongSwan](/docs/containers?topic=containers-vpn#vpn-setup) 用于 VPN 连接，那么 strongSwan 2.0.0 Helm chart 不适用于 Calico V3 或 Kubernetes 1.11。请[更新 strongSwan](/docs/containers?topic=containers-vpn#vpn_upgrade) 至 2.1.0 Helm chart，此版本向后兼容 Calico 2.6 以及 Kubernetes 1.7、1.8 和 1.9。

5.  [将集群主节点更新到 Kubernetes V1.11](/docs/containers?topic=containers-update#master)。

<br />


## 归档
{: #k8s_version_archive}

查找 {{site.data.keyword.containerlong_notm}} 中不支持的 Kubernetes 版本的概述。
{: shortdesc}

### V1.10（不受支持）
{: #cs_v110}

自 2019 年 5 月 16 日开始，不支持使用运行 [Kubernetes V1.10](/docs/containers?topic=containers-changelog#changelog_archive) 的 {{site.data.keyword.containerlong_notm}} 集群。V1.10 集群无法接收安全性更新或支持，除非更新到下一个最新版本。
{: shortdesc}

对于每个 Kubernetes 版本更新，请[查看潜在影响](/docs/containers?topic=containers-cs_versions#cs_versions)，然后[更新集群](/docs/containers?topic=containers-update#update)至 [Kubernetes 1.12](#cs_v112)，因为 Kubernetes 1.11 已不推荐使用。


### V1.9（不受支持）
{: #cs_v19}

自 2018 年 12 月 27 日开始，不支持使用运行 [Kubernetes V1.9](/docs/containers?topic=containers-changelog#changelog_archive) 的 {{site.data.keyword.containerlong_notm}} 集群。V1.9 集群无法接收安全性更新或支持，除非更新到下一个最新版本。
{: shortdesc}

对于每个 Kubernetes 版本更新，请[查看潜在影响](/docs/containers?topic=containers-cs_versions#cs_versions)，然后[更新集群](/docs/containers?topic=containers-update#update)，首先更新到[不推荐的 Kubernetes 1.11](#cs_v111)，然后立即更新到 [Kubernetes 1.12](#cs_v112)。


### V1.8（不受支持）
{: #cs_v18}

自 2018 年 9 月 22 日开始，不支持使用运行 [Kubernetes V1.8](/docs/containers?topic=containers-changelog#changelog_archive) 的 {{site.data.keyword.containerlong_notm}} 集群。V1.8 集群无法接收安全性更新或支持。
{: shortdesc}

要继续在 {{site.data.keyword.containerlong_notm}} 中运行应用程序，请[创建新集群](/docs/containers?topic=containers-clusters#clusters)并[部署应用程序](/docs/containers?topic=containers-app#app)至新集群。

### V1.7（不支持）
{: #cs_v17}

自 2018 年 6 月 21 日开始，不支持使用运行 [Kubernetes V1.7](/docs/containers?topic=containers-changelog#changelog_archive) 的 {{site.data.keyword.containerlong_notm}} 集群。V1.7 集群无法接收安全性更新或支持。
{: shortdesc}

要继续在 {{site.data.keyword.containerlong_notm}} 中运行应用程序，请[创建新集群](/docs/containers?topic=containers-clusters#clusters)并[部署应用程序](/docs/containers?topic=containers-app#app)至新集群。

### V1.5（不受支持）
{: #cs_v1-5}

自 2018 年 4 月 4 日开始，不支持使用运行 [Kubernetes V1.5](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG-1.5.md) 的 {{site.data.keyword.containerlong_notm}} 集群。V1.5 集群无法接收安全性更新或支持。
{: shortdesc}

要继续在 {{site.data.keyword.containerlong_notm}} 中运行应用程序，请[创建新集群](/docs/containers?topic=containers-clusters#clusters)并[部署应用程序](/docs/containers?topic=containers-app#app)至新集群。
