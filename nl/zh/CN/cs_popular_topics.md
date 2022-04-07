---

copyright:
  years: 2014, 2019
lastupdated: "2019-04-15"

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



# {{site.data.keyword.containerlong_notm}} 的热门主题
{: #cs_popular_topics}

与 {{site.data.keyword.containerlong}} 中的内容保持同步。了解要探索的新功能、试用技巧或其他开发者目前在产品中发现有用的一些热门主题。
{:shortdesc}



## 2019 年 4 月的热门主题
{: #apr19}

<table summary="此表显示热门主题。各行都应从左到右阅读，其中第一列是日期，第二列是功能标题，第三列是描述。">
<caption>2019 年 4 月容器和 Kubernetes 集群的热门主题</caption>
<thead>
<th>日期</th>
<th>标题</th>
<th>描述</th>
</thead>
<tbody>
<tr>
<td>2019 年 4 月 15 日</td>
<td>[注册网络负载均衡器 (NLB) 主机名](/docs/containers?topic=containers-loadbalancer#loadbalancer_hostname)</td>
<td>现在，设置公共网络负载均衡器 (NLB) 以向因特网公开应用程序后，可以通过创建主机名来为 NLB IP 创建 DNS 条目。{{site.data.keyword.Bluemix_notm}} 负责为主机名生成和维护通配符 SSL 证书。您还可以设置 TCP/HTTP(S) 监视器，以对每个主机名后面的 NLB IP 地址执行运行状况检查。</td>
</tr>
<tr>
<td>2019 年 4 月 8 日</td>
<td>[Web 浏览器中的 Kubernetes 终端 (beta)](/docs/containers?topic=containers-cs_cli_install#cli_web)</td>
<td>如果在 {{site.data.keyword.Bluemix_notm}} 控制台中使用集群仪表板来管理集群，但希望快速进行更高级的配置更改，现在可以在 Kubernetes 终端的 Web 浏览器中直接运行 CLI 命令。在集群的详细信息页面上，通过单击**终端**按钮来启动 Kubernetes 终端。请注意，Kubernetes 终端作为 Beta 附加组件发布，不适合用于生产集群。</td>
</tr>
<tr>
<td>2019 年 4 月 4 日</td>
<td>[现在高可用性主节点在悉尼可用](/docs/containers?topic=containers-regions-and-zones#zones)</td>
<td>在多专区大城市位置（现在包括悉尼）中[创建集群](/docs/containers?topic=containers-clusters#clusters_ui)时，Kubernetes 主节点副本会跨专区分布以实现高可用性。</td>
</tr>
</tbody></table>

## 2019 年 3 月的热门主题
{: #mar19}

<table summary="此表显示热门主题。各行都应从左到右阅读，其中第一列是日期，第二列是功能标题，第三列是描述。">
<caption>2019 年 3 月容器和 Kubernetes 集群的热门主题</caption>
<thead>
<th>日期</th>
<th>标题</th>
<th>描述</th>
</thead>
<tbody>
<tr>
<td>2019 年 3 月 21 日</td>
<td>为 Kubernetes 集群主节点引入了专用服务端点</td>
<td>缺省情况下，{{site.data.keyword.containerlong_notm}} 为集群设置了对公用 VLAN 和专用 VLAN 的访问权。先前，如果要使用[仅专用 VLAN 集群](/docs/containers?topic=containers-plan_clusters#private_clusters)，您需要设置网关设备来将集群的工作程序节点与主节点相连接。现在，您可以使用专用服务端点。启用了专用服务端点后，工作程序节点与主节点之间的所有流量都位于专用网络上，而无需网关设备。除了此增强的安全性外，专用网络上的入站和出站流量[不受限制且免费 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/cloud/bandwidth)。您仍可以保留公共服务端点，以通过因特网安全地访问 Kubernetes 主节点，例如在不位于专用网络上时运行 `kubectl` 命令。<br><br>
要使用专用服务端点，必须启用 IBM Cloud Infrastructure (SoftLayer) 帐户的 [VRF](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) 和[服务端点](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started)。集群必须运行 Kubernetes V1.11 或更高版本。如果集群运行的是较低的 Kubernetes 版本，请[至少更新到 1.11](/docs/containers?topic=containers-update#update)。有关更多信息，请查看以下链接：<ul>
<li>[了解通过服务端点进行的主节点与工作程序节点的通信](/docs/containers?topic=containers-cs_network_ov#cs_network_ov_master)</li>
<li>[设置专用服务端点](/docs/containers?topic=containers-cs_network_cluster#set-up-private-se)</li>
<li>[从公共服务端点切换到专用服务端点](/docs/containers?topic=containers-cs_network_cluster#migrate-to-private-se)</li>
<li>如果在专用网络上有防火墙，请[添加用于 {{site.data.keyword.containerlong_notm}}、{{site.data.keyword.registrylong_notm}} 和其他 {{site.data.keyword.Bluemix_notm}} 服务的专用 IP 地址](/docs/containers?topic=containers-firewall#firewall_outbound)。</li>
</ul>
<p class="important">如果切换到仅专用服务端点集群，请确保集群仍可与您使用的其他 {{site.data.keyword.Bluemix_notm}} 服务进行通信。[Portworx 软件定义的存储 (SDS)](/docs/containers?topic=containers-portworx#portworx) 和[集群自动缩放器](/docs/containers?topic=containers-ca#ca)不支持仅专用服务端点。请改为使用具有公共和专用服务端点的集群。如果集群运行的是 Kubernetes V1.13.4_1513、V1.12.6_1544、V1.11.8_1550、V1.10.13_1551 或更高版本，那么支持[基于 NFS 的文件存储器](/docs/containers?topic=containers-file_storage#file_storage)。</p>
</td>
</tr>
<tr>
<td>2019 年 3 月 7 日</td>
<td>[集群自动缩放器已从 Beta 移至 GA](/docs/containers?topic=containers-ca#ca)</td>
<td>现在，集群自动缩放器已一般可用。安装 Helm 插件后，即可开始根据安排的工作负载的大小调整需求，自动缩放集群中的工作程序池，以增加或减少工作程序节点数。<br><br>
针对集群自动缩放器需要帮助或有反馈意见？如果您是外部用户，请[在公共 Slack 中注册 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://bxcs-slack-invite.mybluemix.net/)，然后在 [#cluster-autoscaler ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://ibm-container-service.slack.com/messages/CF6APMLBB) 通道中发布帖子。如果您是 IBM 员工，请在[内部 Slack ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://ibm-argonauts.slack.com/messages/C90D3KZUL) 通道中发布帖子。</td>
</tr>
</tbody></table>




## 2019 年 2 月的热门主题
{: #feb19}

<table summary="此表显示热门主题。各行都应从左到右阅读，其中第一列是日期，第二列是功能标题，第三列是描述。">
<caption>2019 年 2 月容器和 Kubernetes 集群的热门主题</caption>
<thead>
<th>日期</th>
<th>标题</th>
<th>描述</th>
</thead>
<tbody>
<tr>
<td>2 月 25 日</td>
<td>对从 {{site.data.keyword.registrylong_notm}} 拉取映像有更细颗粒度的控制权</td>
<td>在 {{site.data.keyword.containerlong_notm}} 集群中部署工作负载时，容器现在可以从 [{{site.data.keyword.registrylong_notm}} 的新 `icr.io`](/docs/services/Registry?topic=registry-registry_overview#registry_regions) 域名中拉取映像。此外，还可以使用 {{site.data.keyword.Bluemix_notm}} IAM 中的细颗粒度访问策略来控制对映像的访问权。有关更多信息，请参阅[了解如何授权集群拉取映像](/docs/containers?topic=containers-images#cluster_registry_auth)。</td>
</tr>
<tr>
<td>2 月 21 日</td>
<td>[在墨西哥提供了专区](/docs/containers?topic=containers-regions-and-zones#regions-and-zones)</td>
<td>欢迎使用墨西哥专区 (`mex01`)，这是用于美国南部区域中集群的新专区。如果您有防火墙，请确保[打开防火墙端口](/docs/containers?topic=containers-firewall#firewall_outbound)以用于此专区以及您集群所在区域内的其他专区。</td>
</tr>
<tr><td>2019 年 2 月 6 日</td>
<td>[Istio on {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-istio)</td>
<td>Istio on {{site.data.keyword.containerlong_notm}} 提供了 Istio 的无缝安装，可对 Istio 控制平面组件进行自动更新和生命周期管理，并可与平台日志记录和监视工具相集成。只需单击一次，您就可以获取所有 Istio 核心组件，执行其他跟踪、监视和可视化，并使 BookInfo 样本应用程序启动并运行。Istio on {{site.data.keyword.containerlong_notm}} 作为受管附加组件提供，因此 {{site.data.keyword.Bluemix_notm}} 会自动使所有 Istio 组件保持最新。</td>
</tr>
<tr>
<td>2019 年 2 月 6 日</td>
<td>[Knative on {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-knative_tutorial)</td>
<td>Knative 是一种开放式源代码平台，扩展了 Kubernetes 的功能，帮助您在 Kubernetes 集群上创建以源代码为中心的现代容器化无服务器应用程序。Managed Knative on {{site.data.keyword.containerlong_notm}} 是一个受管附加组件，用于将 Istio 与 Kubernetes 集群直接集成。附加组件中的 Knative 和 Istio 版本已由 IBM 进行测试，支持在 {{site.data.keyword.containerlong_notm}} 中使用。</td>
</tr>
</tbody></table>


## 2019 年 1 月的热门主题
{: #jan19}

<table summary="此表显示热门主题。各行都应从左到右阅读，其中第一列是日期，第二列是功能标题，第三列是描述。">
<caption>2019 年 1 月容器和 Kubernetes 集群的热门主题</caption>
<thead>
<th>日期</th>
<th>标题</th>
<th>描述</th>
</thead>
<tbody>
<tr>
<td>1 月 30 日</td>
<td>{{site.data.keyword.Bluemix_notm}} IAM 服务访问角色和 Kubernetes 名称空间</td>
<td>现在，{{site.data.keyword.containerlong_notm}} 支持 {{site.data.keyword.Bluemix_notm}} IAM [服务访问角色](/docs/containers?topic=containers-access_reference#service)。 这些服务访问角色与 [Kubernetes RBAC](/docs/containers?topic=containers-users#role-binding) 相对应，用于授权用户在集群中执行 `kubectl` 操作来管理 Kubernetes 资源，例如 pod 或部署。此外，您还可以使用 {{site.data.keyword.Bluemix_notm}} IAM 服务访问角色，[将用户的访问权限制为只能访问集群中的特定 Kubernetes 名称空间](/docs/containers?topic=containers-users#platform)。现在，[平台访问角色](/docs/containers?topic=containers-access_reference#iam_platform)用于授权用户执行 `ibmcloud ks` 操作来管理集群基础架构，例如工作程序节点。<br><br>{{site.data.keyword.Bluemix_notm}} IAM 服务访问角色会根据用户先前通过 IAM 平台访问角色具有的许可权，自动添加到现有 {{site.data.keyword.containerlong_notm}} 帐户和集群。此外，还可以使用 IAM 来创建访问组，将用户添加到访问组，以及为组分配平台或服务访问角色。有关更多信息，请查看博客 [Introducing service roles and namespaces in IAM for more granular control of cluster access ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/blogs/bluemix/2019/02/introducing-service-roles-and-namespaces-in-iam-for-more-granular-control-of-cluster-access/)。</td>
</tr>
<tr>
<td>1 月 8 日</td>
<td>[集群自动缩放器预览 Beta](/docs/containers?topic=containers-ca#ca)</td>
<td>根据安排的工作负载的大小调整需求，自动缩放集群中的工作程序池，以增加或减少工作程序池中的工作程序节点数。要测试此 Beta，您必须请求对白名单的访问权。</td>
</tr>
<tr>
<td>1 月 8 日</td>
<td>[{{site.data.keyword.containerlong_notm}} 诊断和调试工具](/docs/containers?topic=containers-cs_troubleshoot#debug_utility)</td>
<td>您是否有时会发现，要获取所有 YAML 文件以及帮助对问题进行故障诊断时所需的其他信息很不方便？请试用 {{site.data.keyword.containerlong_notm}} 诊断和调试工具，以通过图形用户界面帮助您收集集群、部署和网络信息。</td>
</tr>
</tbody></table>

## 2018 年 12 月的热门主题
{: #dec18}

<table summary="此表显示热门主题。各行都应从左到右阅读，其中第一列是日期，第二列是功能标题，第三列是描述。">
<caption>2018 年 12 月容器和 Kubernetes 集群的热门主题</caption>
<thead>
<th>日期</th>
<th>标题</th>
<th>描述</th>
</thead>
<tbody>
<tr>
<td>12 月 11 日</td>
<td>使用 Portworx 支持 SDS</td>
<td>使用 Portworx 管理容器化数据库和其他有状态应用程序的持久存储，或者在多个专区的 pod 之间共享数据。[Portworx ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://portworx.com/products/introduction/) 是一种高可用性软件定义的存储解决方案 (SDS)，用于管理工作程序节点的本地块存储器，以便为应用程序创建统一的持久性存储层。通过在多个工作程序节点上使用每个容器级别卷的卷复制，Portworx 可确保跨专区的数据持久性和数据可访问性。有关更多信息，请参阅[使用 Portworx 在软件定义的存储 (SDS) 上存储数据](/docs/containers?topic=containers-portworx#portworx)。</td>
</tr>
<tr>
<td>12 月 6 日</td>
<td>{{site.data.keyword.mon_full_notm}}</td>
<td>通过将 Sysdig 作为第三方服务部署到工作程序节点，以将度量值转发到 {{site.data.keyword.monitoringlong}}，从而从运营角度了解应用程序的性能和运行状况。有关更多信息，请参阅[分析在 Kubernetes 集群中部署的应用程序的度量值](/docs/services/Monitoring-with-Sysdig/tutorials?topic=Sysdig-kubernetes_cluster#kubernetes_cluster)。**注**：如果将 {{site.data.keyword.mon_full_notm}} 用于运行 Kubernetes V1.11 或更高版本的集群，那么不会收集所有容器度量值，因为 Sysdig 当前不支持 `containerd`。</td>
</tr>
<tr>
<td>12 月 4 日</td>
<td>[工作程序节点资源保留量](/docs/containers?topic=containers-plan_clusters#resource_limit_node)</td>
<td>您是否因部署了大量应用程序而担心超过集群容量？工作程序节点资源保留量和 Kubernetes 逐出功能可保护集群的计算能力，使集群有足够的资源来保持运行。只需再添加几个工作程序节点，pod 就会自动开始重新安排到这些节点。在最新的[版本补丁更改](/docs/containers?topic=containers-changelog#changelog)中，更新了工作程序节点资源保留量。</td>
</tr>
</tbody></table>

## 2018 年 11 月的热门主题
{: #nov18}

<table summary="此表显示热门主题。各行都应从左到右阅读，其中第一列是日期，第二列是功能标题，第三列是描述。">
<caption>2018 年 11 月容器和 Kubernetes 集群的热门主题</caption>
<thead>
<th>日期</th>
<th>标题</th>
<th>描述</th>
</thead>
<tbody>
<tr>
<td>11 月 29 日</td>
<td>[在金奈提供了专区](/docs/containers?topic=containers-regions-and-zones)</td>
<td>欢迎使用印度金奈专区，这是用于亚太北部区域中集群的新专区。如果您有防火墙，请确保[打开防火墙端口](/docs/containers?topic=containers-firewall#firewall)以用于此专区以及您集群所在区域内的其他专区。</td>
</tr>
<tr>
<td>11 月 27 日</td>
<td>{{site.data.keyword.la_full_notm}}</td>
<td>通过将 LogDNA 作为第三方服务部署到工作程序节点来管理 pod 容器中的日志，从而将日志管理功能添加到集群。有关更多信息，请参阅[使用 {{site.data.keyword.loganalysisfull_notm}} 通过 LogDNA 管理 Kubernetes 集群日志](/docs/services/Log-Analysis-with-LogDNA/tutorials?topic=LogDNA-kube#kube)。</td>
</tr>
<tr>
<td>11 月 7 日</td>
<td>网络负载均衡器 2.0 (beta)</td>
<td>现在，可以选择 [NLB 1.0 或 2.0](/docs/containers?topic=containers-loadbalancer#planning_ipvs)，以向公众安全地公开集群应用程序。</td>
</tr>
<tr>
<td>11 月 7 日</td>
<td>Kubernetes V1.12 可用</td>
<td>现在，可以更新或创建运行 [Kubernetes V1.12](/docs/containers?topic=containers-cs_versions#cs_v112) 的集群！缺省情况下，1.12 集群随附高可用性 Kubernetes 主节点。</td>
</tr>
<tr>
<td>11 月 7 日</td>
<td>高可用性主节点</td>
<td>运行 Kubernetes V1.10 的集群中提供了高可用性主节点。在前面 1.11 集群的条目中描述的所有优点也同样适用于 1.10 集群，此外必须执行的[准备步骤](/docs/containers?topic=containers-cs_versions#110_ha-masters)也一样。</td>
</tr>
<tr>
<td>11 月 1 日</td>
<td>运行 Kubernetes V1.11 的集群中的高可用性主节点</td>
<td>在单个专区中，主节点具有高可用性，在分别用于 Kubernetes API 服务器、etcd、调度程序和控制器管理器的不同物理主机上包含多个副本，以防止发生中断，例如在集群更新期间。如果集群位于支持多专区的专区中，那么高可用性主节点还将在各专区中进行分布，以帮助保护集群不受专区故障的影响。<br>有关必须执行的操作，请参阅[更新为高可用性集群主节点](/docs/containers?topic=containers-cs_versions#ha-masters)。这些准备操作适用于以下情况：<ul>
<li>如果具有防火墙或定制 Calico 网络策略。</li>
<li>如果在工作程序节点上使用的主机端口是 `2040` 或 `2041`。</li>
<li>如果使用了集群主节点 IP 地址对主节点进行集群内访问。</li>
<li>如果具有调用 Calico API 或 CLI (`calicoctl`) 的自动化操作，例如创建 Calico 策略。</li>
<li>如果使用 Kubernetes 或 Calico 网络策略来控制对主节点的 pod 流出访问。</li></ul></td>
</tr>
</tbody></table>

## 2018 年 10 月的热门主题
{: #oct18}

<table summary="此表显示热门主题。各行都应从左到右阅读，其中第一列是日期，第二列是功能标题，第三列是描述。">
<caption>2018 年 10 月容器和 Kubernetes 集群的热门主题</caption>
<thead>
<th>日期</th>
<th>标题</th>
<th>描述</th>
</thead>
<tbody>
<tr>
<td>10 月 25 日</td>
<td>[在米兰提供了专区](/docs/containers?topic=containers-regions-and-zones)</td>
<td>欢迎使用适用于付费集群的欧洲中部区域意大利米兰的新专区。先前，米兰仅可用于免费集群。如果您有防火墙，请确保[打开防火墙端口](/docs/containers?topic=containers-firewall#firewall)以用于此专区以及您集群所在区域内的其他专区。</td>
</tr>
<tr>
<td>10 月 22 日</td>
<td>[新的伦敦多专区位置 `lon05`](/docs/containers?topic=containers-regions-and-zones#zones)</td>
<td>伦敦多专区大城市位置的 `lon02` 专区替换为新的 `lon05` 专区，新专区的基础架构资源比 `lon02` 更丰富。请使用 `lon05` 专区创建新的多专区集群。支持使用 `lon02` 的现有集群，但新的多专区集群必须改为使用 `lon05`。</td>
</tr>
<tr>
<td>10 月 5 日</td>
<td>与 {{site.data.keyword.keymanagementservicefull}} 集成 (beta)</td>
<td>您可以通过[启用 {{site.data.keyword.keymanagementserviceshort}} (beta)](/docs/containers?topic=containers-encryption#keyprotect) 对集群中的 Kubernetes 私钥进行加密。</td>
</tr>
<tr>
<td>10 月 4 日</td>
<td>现在，[{{site.data.keyword.registrylong}} 已与 {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM)](/docs/services/Registry?topic=registry-iam#iam) 集成</td>
<td>您可以使用 {{site.data.keyword.Bluemix_notm}} IAM 来控制对注册表资源的访问，例如拉取、推送和构建映像。创建集群时，还会创建注册表令牌，以便集群可以使用注册表。因此，您需要帐户级别注册表的**管理员**平台管理角色才能创建集群。要对注册表帐户启用 {{site.data.keyword.Bluemix_notm}} IAM，请参阅[对现有用户启用策略强制实施](/docs/services/Registry?topic=registry-user#existing_users)。</td>
</tr>
<tr>
<td>10 月 1 日</td>
<td>[资源组](/docs/containers?topic=containers-users#resource_groups)</td>
<td>您可以使用资源组将 {{site.data.keyword.Bluemix_notm}} 资源分成管道、部门或其他分组，以帮助分配访问权和计量表用途。现在，{{site.data.keyword.containerlong_notm}} 支持在 `default` 组或您创建的其他任何资源组中创建集群！</td>
</tr>
</tbody></table>

## 2018 年 9 月的热门主题
{: #sept18}

<table summary="此表显示热门主题。各行都应从左到右阅读，其中第一列是日期，第二列是功能标题，第三列是描述。">
<caption>2018 年 9 月容器和 Kubernetes 集群的热门主题</caption>
<thead>
<th>日期</th>
<th>标题</th>
<th>描述</th>
</thead>
<tbody>
<tr>
<td>9 月 25 日</td>
<td>[提供了新专区](/docs/containers?topic=containers-regions-and-zones)</td>
<td>现在，甚至有更多应用程序部署位置选项可供您选择！
<ul><li>欢迎使用美国南部区域圣何塞的两个新专区：`sjc03` 和 `sjc04`。如果您有防火墙，请确保[打开防火墙端口](/docs/containers?topic=containers-firewall#firewall)以用于此专区以及您集群所在区域内的其他专区。</li>
<li>现在，通过两个新的 `tok04` 和 `tok05` 专区，可以在亚太地区北部区域的东京[创建多专区集群](/docs/containers?topic=containers-plan_clusters#multizone)。</li></ul></td>
</tr>
<tr>
<td>9 月 5 日</td>
<td>[在奥斯陆提供了专区](/docs/containers?topic=containers-regions-and-zones)</td>
<td>欢迎使用欧洲中部区域挪威奥斯陆的新专区。如果您有防火墙，请确保[打开防火墙端口](/docs/containers?topic=containers-firewall#firewall)以用于此专区以及您集群所在区域内的其他专区。</td>
</tr>
</tbody></table>

## 2018 年 8 月的热门主题
{: #aug18}

<table summary="此表显示热门主题。各行都应从左到右阅读，其中第一列是日期，第二列是功能标题，第三列是描述。">
<caption>2018 年 8 月容器和 Kubernetes 集群的热门主题</caption>
<thead>
<th>日期</th>
<th>标题</th>
<th>描述</th>
</thead>
<tbody>
<tr>
<td>8 月 31 日</td>
<td>{{site.data.keyword.cos_full_notm}} 现在与 {{site.data.keyword.containerlong}} 相集成</td>
<td>使用 Kubernetes 本机持久卷声明 (PVC) 以向集群供应 {{site.data.keyword.cos_full_notm}}。{{site.data.keyword.cos_full_notm}} 最适合用于读取密集型工作负载，以及在多专区集群中跨多个专区存储数据。从在集群上[创建 {{site.data.keyword.cos_full_notm}} 服务实例](/docs/containers?topic=containers-object_storage#create_cos_service)和[安装 {{site.data.keyword.cos_full_notm}} 插件](/docs/containers?topic=containers-object_storage#install_cos)开始。</br></br>不确定哪种存储解决方案适合您？请从[此处](/docs/containers?topic=containers-storage_planning#choose_storage_solution)开始分析数据并针对数据选择相应的存储解决方案。</td>
</tr>
<tr>
<td>8 月 14 日</td>
<td>将集群更新为 Kubernetes V1.11 以分配 pod 优先级</td>
<td>在将集群更新为 [Kubernetes V1.11](/docs/containers?topic=containers-cs_versions#cs_v111) 后，可利用新功能，例如，使用 `containerd` 提高的容器运行时性能或[分配 pod 优先级](/docs/containers?topic=containers-pod_priority#pod_priority)。</td>
</tr>
</tbody></table>

## 2018 年 7 月的热门主题
{: #july18}

<table summary="此表显示热门主题。各行都应从左到右阅读，其中第一列是日期，第二列是功能标题，第三列是描述。">
<caption>2018 年 7 月容器和 Kubernetes 集群的热门主题</caption>
<thead>
<th>日期</th>
<th>标题</th>
<th>描述</th>
</thead>
<tbody>
<tr>
<td>7 月 30 日</td>
<td>[自带 Ingress 控制器](/docs/containers?topic=containers-ingress#user_managed)</td>
<td>您对集群的 Ingress 控制器有非常具体的安全性或其他定制需求吗？如果有，那么您可能希望运行自己的 Ingress 控制器，而不是缺省 Ingress 控制器。</td>
</tr>
<tr>
<td>7 月 10 日</td>
<td>引入多专区集群</td>
<td>想要提高集群可用性吗？现在，您可以使集群跨精选大城市区域中的多个专区。有关更多信息，请参阅[在 {{site.data.keyword.containerlong_notm}} 中创建多专区集群](/docs/containers?topic=containers-plan_clusters#multizone)。</td>
</tr>
</tbody></table>

## 2018 年 6 月的热门主题
{: #june18}

<table summary="此表显示热门主题。各行都应从左到右阅读，其中第一列是日期，第二列是功能标题，第三列是描述。">
<caption>2018 年 6 月容器和 Kubernetes 集群的热门主题</caption>
<thead>
<th>日期</th>
<th>标题</th>
<th>描述</th>
</thead>
<tbody>
<tr>
<td>6 月 13 日</td>
<td>`bx` CLI 命令名称将更改为 `ic` CLI</td>
<td>下载最新版本的 {{site.data.keyword.Bluemix_notm}} CLI 后，现在是使用 `ic` 前缀而不是 `bx` 来运行命令。例如，通过运行 `ibmcloud ks clusters` 列出集群。</td>
</tr>
<tr>
<td>6 月 12 日</td>
<td>[pod 安全策略](/docs/containers?topic=containers-psp)</td>
<td>对于运行 Kubernetes 1.10.3 或更高版本的集群，可以配置 pod 安全策略以授权谁可以在 {{site.data.keyword.containerlong_notm}} 中创建和更新 pod。</td>
</tr>
<tr>
<td>6 月 6 日</td>
<td>[对 IBM 提供的 Ingress 通配符子域的 TLS 支持](/docs/containers?topic=containers-ingress#wildcard_tls)</td>
<td>对于在 2018 年 6 月 6 日或之后创建的集群，现在 IBM 提供的 Ingress 子域 TLS 证书是通配符证书，可用于已注册的通配符子域。对于在 2018 年 6 月 6 日之前创建的集群，在更新当前 TLS 证书时，TLS 证书会更新为通配符证书。</td>
</tr>
</tbody></table>

## 2018 年 5 月的热门主题
{: #may18}


<table summary="此表显示热门主题。各行都应从左到右阅读，其中第一列是日期，第二列是功能标题，第三列是描述。">
<caption>2018 年 5 月容器和 Kubernetes 集群的热门主题</caption>
<thead>
<th>日期</th>
<th>标题</th>
<th>描述</th>
</thead>
<tbody>
<tr>
<td>5 月 24 日</td>
<td>[新的 Ingress 子域格式](/docs/containers?topic=containers-ingress)</td>
<td>对于 5 月 24 日之后创建的集群，将为其分配新格式的 Ingress 子域：<code>&lt;cluster_name&gt;.&lt;region&gt;.containers.appdomain.cloud</code>。使用 Ingress 公开应用程序时，可以使用新的子域从因特网访问应用程序。</td>
</tr>
<tr>
<td>5 月 14 日</td>
<td>[更新：在全球范围内的 GPU 裸机上部署工作负载](/docs/containers?topic=containers-app#gpu_app)</td>
<td>如果集群中有[裸机图形处理单元 (GPU) 机器类型](/docs/containers?topic=containers-plan_clusters#shared_dedicated_node)，那么可以安排数学密集型应用程序。GPU 工作程序节点可以处理 CPU 和 GPU 上的应用程序工作负载以提高性能。</td>
</tr>
<tr>
<td>5 月 3 日</td>
<td>[Container Image Security Enforcement (beta)](/docs/services/Registry?topic=registry-security_enforce#security_enforce)</td>
<td>您的团队需要一些额外的帮助来了解要在应用程序容器中拉入哪个映像吗？请试用 Container Image Security Enforcement (beta)，以便在部署容器映像之前验证这些映像。可用于运行 Kubernetes 1.9 或更高版本的集群。</td>
</tr>
<tr>
<td>5 月 1 日</td>
<td>[通过控制台部署 Kubernetes 仪表板](/docs/containers?topic=containers-app#cli_dashboard)</td>
<td>您曾想过仅单击一次就访问 Kubernetes 仪表板吗？请使用 {{site.data.keyword.Bluemix_notm}} 控制台中的 **Kubernetes 仪表板**按钮。</td>
</tr>
</tbody></table>




## 2018 年 4 月的热门主题
{: #apr18}

<table summary="此表显示热门主题。各行都应从左到右阅读，其中第一列是日期，第二列是功能标题，第三列是描述。">
<caption>2018 年 4 月容器和 Kubernetes 集群的热门主题</caption>
<thead>
<th>日期</th>
<th>标题</th>
<th>描述</th>
</thead>
<tbody>
<tr>
<td>4 月 17 日</td>
<td>{{site.data.keyword.Bluemix_notm}} Block Storage</td>
<td>安装 {{site.data.keyword.Bluemix_notm}} Block Storage [插件](/docs/containers?topic=containers-block_storage#install_block)以在块存储器中保存持久数据。然后，可以为集群[创建新的](/docs/containers?topic=containers-block_storage#add_block)块存储器或[使用现有](/docs/containers?topic=containers-block_storage#existing_block)块存储器。</td>
</tr>
<tr>
<td>4 月 13 日</td>
<td>[用于将 Cloud Foundry 应用程序迁移到集群的新教程](/docs/containers?topic=containers-cf_tutorial#cf_tutorial)</td>
<td>您有 Cloud Foundry 应用程序吗？请了解如何将该应用程序中的相同代码部署到在 Kubernetes 集群中运行的容器。</td>
</tr>
<tr>
<td>4 月 5 日</td>
<td>[过滤日志](/docs/containers?topic=containers-health#filter-logs)</td>
<td>过滤掉特定日志，使其不会转发。可以针对特定名称空间、容器名称、日志级别和消息字符串过滤掉日志。</td>
</tr>
</tbody></table>

## 2018 年 3 月的热门主题
{: #mar18}

<table summary="此表显示热门主题。各行都应从左到右阅读，其中第一列是日期，第二列是功能标题，第三列是描述。">
<caption>2018 年 3 月容器和 Kubernetes 集群的热门主题</caption>
<thead>
<th>日期</th>
<th>标题</th>
<th>描述</th>
</thead>
<tbody>
<tr>
<td>3 月 16 日</td>
<td>[供应具有可信计算的裸机集群](/docs/containers?topic=containers-plan_clusters#shared_dedicated_node)</td>
<td>创建运行 [Kubernetes V1.9](/docs/containers?topic=containers-cs_versions#cs_v19) 或更高版本的裸机集群，并启用“可信计算”来验证工作程序节点是否被篡改。</td>
</tr>
<tr>
<td>3 月 14 日</td>
<td>[使用 {{site.data.keyword.appid_full}} 安全登录](/docs/containers?topic=containers-supported_integrations#appid)</td>
<td>通过要求用户登录，增强了在 {{site.data.keyword.containerlong_notm}} 中运行的应用程序的功能。</td>
</tr>
<tr>
<td>3 月 13 日</td>
<td>[在圣保罗提供了专区](/docs/containers?topic=containers-regions-and-zones)</td>
<td>欢迎巴西圣保罗成为美国南部区域的一个新专区。如果您有防火墙，请确保[打开防火墙端口](/docs/containers?topic=containers-firewall#firewall)以用于此专区以及您集群所在区域内的其他专区。</td>
</tr>
</tbody></table>

## 2018 年 2 月的热门主题
{: #feb18}

<table summary="此表显示热门主题。各行都应从左到右阅读，其中第一列是日期，第二列是功能标题，第三列是描述。">
<caption>2018 年 2 月容器和 Kubernetes 集群的热门主题</caption>
<thead>
<th>日期</th>
<th>标题</th>
<th>描述</th>
</thead>
<tbody>
<tr>
<td>2 月 27 日</td>
<td>工作程序节点的硬件虚拟机 (HVM) 映像</td>
<td>通过 HVM 映像来提高工作负载的 I/O 性能。使用 `ibmcloud ks worker-reload` [命令](/docs/containers?topic=containers-cs_cli_reference#cs_worker_reload)或 `ibmcloud ks worker-update` [命令](/docs/containers?topic=containers-cs_cli_reference#cs_worker_update)在每个现有工作程序节点上进行激活。</td>
</tr>
<tr>
<td>2 月 26 日</td>
<td>[KubeDNS 自动缩放](https://kubernetes.io/docs/tasks/administer-cluster/dns-horizontal-autoscaling/)</td>
<td>现在，KubeDNS 可随着集群的增长而缩放。可以使用以下命令来调整缩放配额：`kubectl -n kube-system edit cm kube-dns-autoscaler`。</td>
</tr>
<tr>
<td>2 月 23 日</td>
<td>在 Web 控制台中查看[日志记录](/docs/containers?topic=containers-health#view_logs)和[度量值](/docs/containers?topic=containers-health#view_metrics)</td>
<td>通过改进的 Web UI，轻松查看集群及其组件的日志和度量数据。有关访问权的信息，请参阅“集群详细信息”页面。</td>
</tr>
<tr>
<td>2 月 20 日</td>
<td>加密映像和[签名的可信内容](/docs/services/Registry?topic=registry-registry_trustedcontent#registry_trustedcontent)</td>
<td>在 {{site.data.keyword.registryshort_notm}} 中，可以对映像进行签名和加密，以确保这些映像在注册表名称空间中存储时的完整性。仅使用可信内容来运行容器实例。</td>
</tr>
<tr>
<td>2 月 19 日</td>
<td>[设置 strongSwan IPSec VPN](/docs/containers?topic=containers-vpn#vpn-setup)</td>
<td>快速部署 strongSwan IPSec VPN Helm 图表，无需虚拟路由器设备就能将 {{site.data.keyword.containerlong_notm}} 集群安全地连接到内部部署数据中心。</td>
</tr>
<tr>
<td>2 月 14 日</td>
<td>[在首尔提供了专区](/docs/containers?topic=containers-regions-and-zones)</td>
<td>将 Kubernetes 集群部署到亚太地区北部区域的首尔，刚好赶上奥运会。如果您有防火墙，请确保[打开防火墙端口](/docs/containers?topic=containers-firewall#firewall)以用于此专区以及您集群所在区域内的其他专区。</td>
</tr>
<tr>
<td>2 月 8 日</td>
<td>[更新 Kubernetes 1.9](/docs/containers?topic=containers-cs_versions#cs_v19)</td>
<td>在更新到 Kubernetes 1.9 之前，请查看要对集群进行的更改。</td>
</tr>
</tbody></table>

## 2018 年 1 月的热门主题
{: #jan18}

<table summary="此表显示热门主题。各行都应从左到右阅读，其中第一列是日期，第二列是功能标题，第三列是描述。">
<caption>2018 年 1 月容器和 Kubernetes 集群的热门主题</caption>
<thead>
<th>日期</th>
<th>标题</th>
<th>描述</th>
</thead>
<tbody>
<td>1 月 25 日</td>
<td>[提供了全局注册表](/docs/services/Registry?topic=registry-registry_overview#registry_regions)</td>
<td>通过 {{site.data.keyword.registryshort_notm}}，可以使用全局 `registry.bluemix.net` 来拉取 IBM 提供的公共映像。</td>
</tr>
<tr>
<td>1 月 23 日</td>
<td>[在新加坡和加拿大蒙特利尔提供了专区](/docs/containers?topic=containers-regions-and-zones)</td>
<td>新加坡和蒙特利尔是 {{site.data.keyword.containerlong_notm}} 亚太地区北部和美国东部区域中可用的专区。如果您有防火墙，请确保[打开防火墙端口](/docs/containers?topic=containers-firewall#firewall)以用于这些专区以及您集群所在区域内的其他专区。</td>
</tr>
<tr>
<td>1 月 8 日</td>
<td>[提供增强的类型模板](/docs/containers?topic=containers-cs_cli_reference#cs_machine_types)</td>
<td>系列 2 虚拟机类型包括本地 SSD 存储和磁盘加密。[迁移工作负载](/docs/containers?topic=containers-update#machine_type)至这些类型模板，以提高性能和稳定性。</td>
</tr>
</tbody></table>

## 在 Slack 上与志趣相投的开发人员交谈
{: #slack}

您可以在 [{{site.data.keyword.containerlong_notm}} Slack ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://ibm-container-service.slack.com) 中查看其他人正在讨论的内容并提出自己的问题。
{:shortdesc}

如果未将 IBM 标识用于 {{site.data.keyword.Bluemix_notm}} 帐户，请针对此 Slack [请求邀请](https://bxcs-slack-invite.mybluemix.net/)。
{: tip}
