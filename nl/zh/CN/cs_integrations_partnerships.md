---

copyright:
  years: 2014, 2019
lastupdated: "2019-05-31"

keywords: kubernetes, iks, helm

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


# IBM Cloud Kubernetes Service 合作伙伴
{: #service-partners}

IBM 致力于使 {{site.data.keyword.containerlong_notm}} 成为最佳 Kubernetes 服务，以帮助您迁移、操作和管理 Kubernetes 工作负载。为向您提供在云中运行生产工作负载所需的所有功能，{{site.data.keyword.containerlong_notm}} 与其他第三方服务提供者合作，利用一流的日志记录、监视和存储工具来增强集群功能。
{: shortdesc}

查看合作伙伴及其提供的每种解决方案的优点。要查找可在集群中使用的其他专有 {{site.data.keyword.Bluemix_notm}} 和第三方开放式源代码服务，请参阅[了解 {{site.data.keyword.Bluemix_notm}} 和第三方集成](/docs/containers?topic=containers-ibm-3rd-party-integrations)。

## LogDNA
{: #logdna-partner}

{{site.data.keyword.la_full_notm}} 将 [LogDNA ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://logdna.com/) 作为第三方服务提供，可用于将智能日志记录功能添加到集群和应用程序。

### 优点
{: #logdna-benefits}

查看下表以了解使用 LogDNA 可以获得的主要优点的列表。
{: shortdesc}

|优点|描述|
|-------------|------------------------------|
|集中日志管理和日志分析|将集群配置为日志源后，LogDNA 会自动开始收集工作程序节点、pod、应用程序和网络的日志记录信息。日志由 LogDNA 自动解析、建立索引、进行标记和聚集，并在 LogDNA 仪表板中直观显示，以便您可以轻松地深入研究集群资源。可以使用内置的图形工具来直观显示最常见的错误代码或日志条目。|
|可使用类似 Google 的搜索语法轻松查找|LogDNA 使用类似 Google 的搜索语法，支持标准术语、`AND` 和 `OR` 运算，并允许您排除或组合搜索项，以帮助更轻松地查找日志。通过对日志建立智能索引，可以随时跳转至特定日志条目。|
|动态和静态加密|LogDNA 会自动加密日志，以保护传输中的日志和静态日志。|
|定制警报和日志视图|可以使用仪表板来查找与搜索条件匹配的日志，在视图中保存这些日志，并与其他用户共享此视图，以简化团队成员之间的调试。您还可以使用此视图来创建可以发送到下游系统（如 PagerDuty、Slack 或电子邮件）的警报。|
|开箱即用的仪表板和定制仪表板|您可以在各种现有仪表板之间进行选择，或者创建您自己的仪表板，以按照您需要的方式来直观显示日志。|

### 与 {{site.data.keyword.containerlong_notm}} 集成
{: #logdna-integration}

LogDNA 由 {{site.data.keyword.la_full_notm}} 提供，这是一个可用于集群的 {{site.data.keyword.Bluemix_notm}} 平台服务。{{site.data.keyword.la_full_notm}} 由 LogDNA 与 IBM 合作运行。
{: shortdesc}

要在集群中使用 LogDNA，必须在 {{site.data.keyword.Bluemix_notm}} 帐户中供应 {{site.data.keyword.la_full_notm}} 的实例，并将 Kubernetes 集群配置为日志源。配置集群后，会自动收集日志并将其转发到 {{site.data.keyword.la_full_notm}} 服务实例。可以使用 {{site.data.keyword.la_full_notm}} 仪表板来访问日志。   

有关更多信息，请参阅[使用 {{site.data.keyword.la_full_notm}} 管理 Kubernetes 集群日志](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-kube)。

### 计费和支持
{: #logdna-billing-support}

{{site.data.keyword.la_full_notm}} 完全集成到 {{site.data.keyword.Bluemix_notm}} 支持系统中。如果使用 {{site.data.keyword.la_full_notm}} 时遇到问题，请在 [{{site.data.keyword.containerlong_notm}} Slack ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://ibm-container-service.slack.com/) 中的 `logdna-on-iks` 通道中发帖提问，或者开具 [{{site.data.keyword.Bluemix_notm}} 支持案例](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support)。使用您的 IBM 标识登录到 Slack。如果未将 IBM 标识用于 {{site.data.keyword.Bluemix_notm}} 帐户，可[请求加入此 Slack 的邀请 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://bxcs-slack-invite.mybluemix.net/)。

## Sysdig
{: #sydig-partner}

{{site.data.keyword.mon_full_notm}} 将 [Sysdig 监视器 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://sysdig.com/products/monitor/) 作为第三方云本机容器分析系统提供，可用于了解计算主机、应用程序、容器和网络的性能和运行状况。
{: shortdesc}

### 优点
{: #sydig-benefits}

查看下表以了解使用 Sysdig 可以获得的主要优点的列表。
{: shortdesc}

|优点|描述|
|-------------|------------------------------|
|自动访问云本机和 Prometheus 定制度量值|从各种预定义的云本机和 Prometheus 定制度量值中进行选择，以了解计算主机、应用程序、容器和网络的性能和运行状况。|
|使用高级过滤器进行故障诊断|Sysdig 监视器会创建网络拓扑，用于显示工作程序节点如何连接以及 Kubernetes 服务如何相互通信。可以从工作程序节点导航至容器和单个系统调用，并在整个过程中对每个资源的重要度量值进行分组和查看。例如，使用这些度量值可查找接收请求最多的服务，或查询和响应速度慢的服务。可以将这些数据与 Kubernetes 事件、定制 CI/CD 事件或代码落实组合使用。
|自动异常检测和定制警报|定义关于您希望在集群或组资源中检测到异常的情况下何时收到通知的规则和阈值，以允许 Sysdig 在一个资源的行为不同于其他资源时通知您。可以将这些警报发送到下游工具，例如 ServiceNow、PagerDuty、Slack、VictorOps 或电子邮件。|
|开箱即用的仪表板和定制仪表板|您可以在各种现有仪表板之间进行选择，或者创建您自己的仪表板，以按照您需要的方式来直观显示微服务的度量值。|
{: caption="使用 Sysdig 监视器的优点" caption-side="top"}

### 与 {{site.data.keyword.containerlong_notm}} 集成
{: #sysdig-integration}

Sysdig 监视器由 {{site.data.keyword.mon_full_notm}} 提供，后者是一个可用于集群的 {{site.data.keyword.Bluemix_notm}} 平台服务。{{site.data.keyword.mon_full_notm}} 由 Sysdig 与 IBM 合作运行。
{: shortdesc}

要在集群中使用 Sysdig 监视器，必须在 {{site.data.keyword.Bluemix_notm}} 帐户中供应 {{site.data.keyword.mon_full_notm}} 的实例，并将 Kubernetes 集群配置为度量源。配置集群后，会自动收集度量值并将其转发到 {{site.data.keyword.mon_full_notm}} 服务实例。可以使用 {{site.data.keyword.mon_full_notm}} 仪表板来访问度量值。   

有关更多信息，请参阅[分析在 Kubernetes 集群中部署的应用程序的度量值](/docs/services/Monitoring-with-Sysdig?topic=Sysdig-kubernetes_cluster)。

### 计费和支持
{: #sysdig-billing-support}

由于 Sysdig 监视器由 {{site.data.keyword.mon_full_notm}} 提供，因此使用情况包含在 {{site.data.keyword.Bluemix_notm}} 的平台服务帐单中。有关定价信息，请在 [{{site.data.keyword.Bluemix_notm}}“目录”![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/observe/monitoring/create) 中查看可用套餐。

{{site.data.keyword.mon_full_notm}} 完全集成到 {{site.data.keyword.Bluemix_notm}} 支持系统中。如果使用 {{site.data.keyword.mon_full_notm}} 时遇到问题，请在 [{{site.data.keyword.containerlong_notm}} Slack ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://ibm-container-service.slack.com/) 中的 `sysdig-monitoring` 通道中发帖提问，或者开具 [{{site.data.keyword.Bluemix_notm}} 支持案例](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support)。使用您的 IBM 标识登录到 Slack。如果未将 IBM 标识用于 {{site.data.keyword.Bluemix_notm}} 帐户，可[请求加入此 Slack 的邀请 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://bxcs-slack-invite.mybluemix.net/)。

## Portworx
{: #portworx-parter}

[Portworx ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://portworx.com/products/introduction/) 是一种高可用性软件定义的存储解决方案，可用于管理容器化数据库和其他有状态应用程序的本地持久性存储器，或者在多个专区的 pod 之间共享数据。
{: shortdesc}

**什么是软件定义的存储 (SDS)？**</br>SDS 解决方案会对连接到集群中工作程序节点的各种类型、大小或来自不同供应商的存储设备进行抽象化处理。在硬盘上具有可用存储器的工作程序节点会添加为存储集群的节点。在此集群中，会对物理存储器进行虚拟化，并将其作为虚拟存储池显示给用户。存储集群由 SDS 软件进行管理。如果数据必须存储在存储集群上，那么 SDS 软件会决定在何处存储数据以实现最高可用性。虚拟存储器随附一组常用的功能和服务，您可以利用这些功能和服务，而无需关注实际底层存储体系结构。

### 优点
{: #portworx-benefits}

查看下表以了解使用 Portworx 可以获得的主要优点的列表。
{: shortdesc}

|优点|描述|
|-------------|------------------------------|
|有状态应用程序的云本机存储和数据管理|Portworx 将连接到工作程序节点的可用本地存储器聚集在一起（存储器的大小或类型可以各不相同），并创建统一的持久存储层，供要在集群中运行的容器化数据库或其他有状态应用程序使用。通过使用 Kubernetes 持久卷声明 (PVC)，可以将本地持久性存储器添加到应用程序以存储数据。|
|使用卷复制的高可用性数据|Portworx 会自动复制集群中不同工作程序节点和专区的卷中的数据，以便可以随时访问数据，并且在工作程序节点发生故障或重新引导时，有状态应用程序可以重新安排到其他工作程序节点。|
|支持运行 `hyper-converged`|可以将 Portworx 配置为运行 [`hyper-converged` ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://docs.portworx.com/portworx-install-with-kubernetes/storage-operations/hyperconvergence/)，以确保始终将计算资源和存储器放在同一工作程序节点上。必须对应用程序进行重新安排时，Portworx 会将应用程序移至其中一个卷副本所在的工作程序节点，以确保有状态应用程序的本地磁盘访问速度和高性能。|
|使用 {{site.data.keyword.keymanagementservicelong_notm}} 加密数据|可以[设置 {{site.data.keyword.keymanagementservicelong_notm}} 加密密钥](/docs/containers?topic=containers-portworx#encrypt_volumes)，这些密钥由通过了 FIPS 140-2 二级认证的基于云的硬件安全模块 (HSM) 进行保护，以保护卷中的数据。您可以选择使用一个加密密钥来加密集群中的所有卷，或者对每个卷使用一个加密密钥。数据发送到其他工作程序节点时，Portworx 会使用此密钥对静态数据和传输期间的数据进行加密。|
|内置快照和云备份|可以通过创建 [Portworx 快照 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://docs.portworx.com/portworx-install-with-kubernetes/storage-operations/create-snapshots/) 来保存卷及其数据的当前状态。快照可以存储在本地 Portworx 集群上或云中。|
|使用 Lighthouse 进行集成监视|[Lighthouse ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://docs.portworx.com/reference/lighthouse/) 是一种直观的图形工具，可帮助您管理和监视 Portworx 集群和卷快照。通过 Lighthouse，可以查看 Portworx 集群的运行状况，包括可用存储节点数、卷数和可用容量，并在 Prometheus、Grafana 或 Kibana 中分析数据。|
{: caption="使用 Portworx 的优点" caption-side="top"}

### 与 {{site.data.keyword.containerlong_notm}} 集成
{: #portworx-integration}

{{site.data.keyword.containerlong_notm}} 提供了针对 SDS 使用情况进行优化的工作程序节点类型模板，这些类型模板随附一个或多个可用于存储数据的未格式化且未安装的原始本地磁盘。您使用随附 10 Gbps 网络速度的 [SDS 工作程序节点机器](/docs/containers?topic=containers-planning_worker_nodes#sds)时，Portworx 的性能最佳。然而，您可以在非 SDS 工作程序节点类型模板上安装 Portworx，但可能无法获得应用程序所需的性能优点。
{: shortdesc}

Portworx 是使用 [Helm chart](/docs/containers?topic=containers-portworx#install_portworx) 安装的。安装 Helm chart 时，Portworx 会自动分析集群中可用的本地持久性存储器，并将存储器添加到 Portworx 存储层。要将存储器从 Portworx 存储层添加到应用程序，必须使用 [Kubernetes 持久卷声明](/docs/containers?topic=containers-portworx#add_portworx_storage)。

要在集群中安装 Portworx，您必须具有 Portworx 许可证。如果您是新手用户，那么可以将 `px-enterprise` 版本用作试用版本。该试用版提供了完整的 Portworx 功能，可以试用 30 天。试用版本到期后，必须[购买 Portworx 许可证 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](/docs/containers?topic=containers-portworx#portworx_license) 才能继续使用 Portworx 集群。


有关如何安装 Portworx 并用于 {{site.data.keyword.containerlong_notm}} 的更多信息，请参阅[使用 Portworx 在软件定义的存储 (SDS) 上存储数据](/docs/containers?topic=containers-portworx)。

### 计费和支持
{: #portworx-billing-support}

随附本地磁盘的 SDS 工作程序节点机器以及用于 Portworx 的虚拟机会包含在每月 {{site.data.keyword.containerlong_notm}} 帐单中。有关定价信息，请参阅 [{{site.data.keyword.Bluemix_notm}}“目录”![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/kubernetes/catalog/cluster)。Portworx 许可证单独收费，不包含在每月帐单中。
{: shortdesc}

如果使用 Portworx 时遇到问题，或者希望就特定用例的 Portworx 配置进行交谈，请在 [{{site.data.keyword.containerlong_notm}} Slack ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://ibm-container-service.slack.com/) 中的 `portworx-on-iks` 通道中发帖提问。使用您的 IBM 标识登录到 Slack。如果未将 IBM 标识用于 {{site.data.keyword.Bluemix_notm}} 帐户，可[请求加入此 Slack 的邀请 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://bxcs-slack-invite.mybluemix.net/)。
