---

copyright:
  years: 2014, 2019
lastupdated: "2019-05-31"

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


# 为什么使用 {{site.data.keyword.containerlong_notm}}
{: #cs_ov}

{{site.data.keyword.containerlong}} 通过组合 Docker 容器、Kubernetes 技术、直观的用户体验以及内置安全性和隔离，提供功能强大的工具来自动对计算主机集群中的容器化应用程序进行部署、操作、缩放和监视。有关证书信息，请参阅 [Compliance on the {{site.data.keyword.Bluemix_notm}} ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/cloud/compliance)。
{:shortdesc}


## 使用服务的优点
{: #benefits}

集群会部署在提供本机 Kubernetes 和特定于 {{site.data.keyword.IBM_notm}} 的功能的计算主机上。
{:shortdesc}

|优点|描述|
|-------|-----------|
|隔离了计算、网络和存储基础架构的单租户 Kubernetes 集群|<ul><li>创建您自己的定制基础架构，以满足组织的需求。</li><li>使用 IBM Cloud Infrastructure (SoftLayer) 提供的资源来供应专用而安全的 Kubernetes 主节点、工作程序节点、虚拟网络和存储器。</li><li>由 {{site.data.keyword.IBM_notm}} 持续监视和更新的完全受管 Kubernetes 主节点，使您的集群可用。</li><li>用于将工作程序节点作为具有可信计算的裸机服务器供应的选项。</li><li>存储持久数据，在 Kubernetes pod 之间共享数据，以及在需要时使用集成和安全卷服务复原数据。</li><li>受益于对所有本机 Kubernetes API 的完全支持。</li></ul>|
|多专区集群可提高高可用性| <ul><li>通过工作程序池轻松管理同一机器类型（CPU、内存、虚拟或物理）的工作程序节点。</li><li>通过在精选多专区中均匀分布节点并对应用程序使用反亲缘关系 pod 部署，以防止专区故障。</li><li>通过使用多专区集群而不是在单独的集群中复制资源，从而降低成本。</li><li>通过在集群的每个专区中自动设置的多专区负载均衡器 (MZLB)，受益于跨应用程序的自动负载均衡。</li></ul>|
|高可用性主节点| <ul><li>缩短集群停机时间，例如在对创建集群时自动供应的高可用性主节点进行主节点更新期间。</li><li>在[多专区集群](/docs/containers?topic=containers-ha_clusters#multizone)中的各个专区中分布主节点，以防止集群发生专区故障。</li></ul> |
|使用漏洞顾问程序确保映像安全合规性|<ul><li>在安全的 Docker 专用映像注册表中设置您自己的存储库，映像会存储在该存储库中，并供组织中的所有用户共享。</li><li>受益于自动扫描专用 {{site.data.keyword.Bluemix_notm}} 注册表中的映像。</li><li>查看特定于映像中所用操作系统的建议，以修复潜在漏洞。</li></ul>|
|持续监视集群运行状况|<ul><li>使用集群仪表板可快速查看和管理集群、工作程序节点和容器部署的运行状况。</li><li>使用 {{site.data.keyword.monitoringlong}}，找到详细的使用量度量值，并快速扩展集群以满足工作负载需求。</li><li>使用 {{site.data.keyword.loganalysislong}} 复查日志记录信息，以查看详细的集群活动。</li></ul>|
|安全地向公众公开应用程序|<ul><li>在公共 IP 地址、{{site.data.keyword.IBM_notm}} 提供的路径或您自己的定制域之间进行选择，以通过因特网访问集群中的服务。</li></ul>|
|{{site.data.keyword.Bluemix_notm}} 服务集成|<ul><li>通过集成 {{site.data.keyword.Bluemix_notm}} 服务（例如，Watson API、Blockchain、数据服务或 Internet of Things）向应用程序添加额外的功能。</li></ul>|
{: caption="{{site.data.keyword.containerlong_notm}} 的优点" caption-side="top"}

准备好开始了吗？可尝试使用[创建 Kubernetes 集群教程](/docs/containers?topic=containers-cs_cluster_tutorial#cs_cluster_tutorial)。


<br />


## 比较产品及其组合
{: #differentiation}

可以在 {{site.data.keyword.Bluemix_notm}} Public、{{site.data.keyword.Bluemix_notm}} Private 或混合设置中运行 {{site.data.keyword.containerlong_notm}}。
{:shortdesc}


<table>
<caption>{{site.data.keyword.containershort_notm}} 设置之间的差异</caption>
<col width="22%">
<col width="78%">
 <thead>
 <th>{{site.data.keyword.containershort_notm}} 设置</th>
 <th>描述</th>
 </thead>
 <tbody>
 <tr>
 <td>{{site.data.keyword.Bluemix_notm}} Public（外部部署）</td>
 <td>使用位于[共享或专用硬件或者位于裸机机器上](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)的 {{site.data.keyword.Bluemix_notm}} Public 时，可以使用 {{site.data.keyword.containerlong_notm}} 在云上的集群中托管应用程序。您还可以使用多个专区中的工作程序池来创建集群，以提高应用程序的高可用性。{{site.data.keyword.containerlong_notm}} on {{site.data.keyword.Bluemix_notm}} Public 通过组合 Docker 容器、Kubernetes 技术、直观的用户体验以及内置安全性和隔离，提供功能强大的工具来自动对计算主机集群中的容器化应用程序进行部署、操作、缩放和监视。<br><br>有关更多信息，请参阅 [{{site.data.keyword.containerlong_notm}} 技术](/docs/containers?topic=containers-ibm-cloud-kubernetes-service-technology)。
 </td>
 </tr>
 <tr>
 <td>{{site.data.keyword.Bluemix_notm}} Private（内部部署）</td>
 <td>{{site.data.keyword.Bluemix_notm}} Private 是可以在您自己的机器上本地安装的应用程序平台。如果需要在防火墙后您自己的受控环境中开发和管理内部部署容器化应用程序，那么可以选择在 {{site.data.keyword.Bluemix_notm}} Private 中使用 Kubernetes。<br><br>有关更多信息，请参阅 [{{site.data.keyword.Bluemix_notm}} Private 产品文档 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_1.2.0/kc_welcome_containers.html)。</td>
 </tr>
 <tr>
 <td>混合设置
 </td>
 <td>混合是将 {{site.data.keyword.Bluemix_notm}} Public 外部部署中运行的服务与内部部署运行的其他服务（例如，{{site.data.keyword.Bluemix_notm}} Private 中的应用程序）组合使用。混合设置的示例：<ul><li>在 {{site.data.keyword.Bluemix_notm}} Public 中使用 {{site.data.keyword.containerlong_notm}} 供应集群，但将该集群连接到内部部署数据库。</li><li>在 {{site.data.keyword.Bluemix_notm}} Private 中使用 {{site.data.keyword.containerlong_notm}} 供应集群，并将应用程序部署到该集群中。但是，此应用程序可能会使用 {{site.data.keyword.Bluemix_notm}} Public 中的 {{site.data.keyword.ibmwatson}} 服务，例如 {{site.data.keyword.toneanalyzershort}}。</li></ul><br>要启用在 {{site.data.keyword.Bluemix_notm}} Public 或 Dedicated 中运行的服务与内部部署运行的服务之间的通信，必须[设置 VPN 连接](/docs/containers?topic=containers-vpn)。有关更多信息，请参阅[将 {{site.data.keyword.containerlong_notm}} 用于 {{site.data.keyword.Bluemix_notm}} Private](/docs/containers?topic=containers-hybrid_iks_icp)。
 </td>
 </tr>
 </tbody>
</table>

<br />


## 比较免费和标准集群
{: #cluster_types}

您可以创建一个免费集群或创建任意数量的标准集群。试用免费集群以熟悉一些 Kubernetes 功能，或者创建标准集群以使用完整的 Kubernetes 功能来部署应用程序。
免费集群在 30 天后会被自动删除。
{:shortdesc}

如果您有免费集群并希望升级到标准集群，那么可以[创建标准集群](/docs/containers?topic=containers-clusters#clusters_ui)。然后，将使用免费集群创建的 Kubernetes 资源的任何 YAML 部署到标准集群中。

|特征|免费集群|标准集群|
|---------------|-------------|-----------------|
|[集群内联网](/docs/containers?topic=containers-security#network)|<img src="images/confirm.svg" width="32" alt="功能可用" style="width:32px;" />|<img src="images/confirm.svg" width="32" alt="功能可用" style="width:32px;" />|
|[NodePort 服务对非稳定 IP 地址的公用网络应用程序访问权](/docs/containers?topic=containers-nodeport)|<img src="images/confirm.svg" width="32" alt="功能可用" style="width:32px;" />|<img src="images/confirm.svg" width="32" alt="功能可用" style="width:32px;" />|
|[用户访问管理](/docs/containers?topic=containers-users#access_policies)|<img src="images/confirm.svg" width="32" alt="功能可用" style="width:32px;" />|<img src="images/confirm.svg" width="32" alt="功能可用" style="width:32px;" />|
|[从集群和应用程序访问 {{site.data.keyword.Bluemix_notm}} 服务](/docs/containers?topic=containers-service-binding#bind-services)|<img src="images/confirm.svg" width="32" alt="功能可用" style="width:32px;" />|<img src="images/confirm.svg" width="32" alt="功能可用" style="width:32px;" />|
|[工作程序节点上用于非持久性存储的磁盘空间](/docs/containers?topic=containers-storage_planning#non_persistent_overview)|<img src="images/confirm.svg" width="32" alt="功能可用" style="width:32px;" />|<img src="images/confirm.svg" width="32" alt="功能可用" style="width:32px;" />|
|[能够在每个 {{site.data.keyword.containerlong_notm}} 区域中创建集群](/docs/containers?topic=containers-regions-and-zones)| | <img src="images/confirm.svg" width="32" alt="功能可用" style="width:32px;" />|
|[用于提高应用程序高可用性的多专区集群](/docs/containers?topic=containers-ha_clusters#multizone)| |<img src="images/confirm.svg" width="32" alt="功能可用" style="width:32px;" />|
|用于实现更高可用性的复制主节点| | <img src="images/confirm.svg" width="32" alt="功能可用" style="width:32px;" />|
|[用于增加容量的可缩放工作程序节点数](/docs/containers?topic=containers-app#app_scaling)| |<img src="images/confirm.svg" width="32" alt="功能可用" style="width:32px;" />|
|[基于 NFS 文件的持久性存储器（带有卷）](/docs/containers?topic=containers-file_storage#file_storage)| |<img src="images/confirm.svg" width="32" alt="功能可用" style="width:32px;" />|
|[网络负载均衡器 (NLB) 服务对稳定 IP 地址的公用或专用网络应用程序访问权](/docs/containers?topic=containers-loadbalancer)| |<img src="images/confirm.svg" width="32" alt="功能可用" style="width:32px;" />|
|[Ingress 服务对稳定 IP 地址和可定制 URL 的公用网络应用程序访问权](/docs/containers?topic=containers-ingress#planning)| |<img src="images/confirm.svg" width="32" alt="功能可用" style="width:32px;" />|
|[可移植公共 IP 地址](/docs/containers?topic=containers-subnets#review_ip)| |<img src="images/confirm.svg" width="32" alt="功能可用" style="width:32px;" />|
|[日志记录和监视](/docs/containers?topic=containers-health#logging)| |<img src="images/confirm.svg" width="32" alt="功能可用" style="width:32px;" />|
|[用于在物理（裸机服务器）服务器上供应工作程序节点的选项](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)| |<img src="images/confirm.svg" width="32" alt="功能可用" style="width:32px;" />|
|[用于供应具有可信计算的裸机工作程序的选项](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)| |<img src="images/confirm.svg" width="32" alt="功能可用" style="width:32px;" />|
{: caption="免费和标准集群的特征" caption-side="top"}
