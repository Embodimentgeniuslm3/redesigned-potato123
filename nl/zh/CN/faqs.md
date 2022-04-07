---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-05"

keywords: kubernetes, iks, compliance, security standards

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
{:download: .download}
{:preview: .preview}
{:faq: data-hd-content-type='faq'}


# 常见问题
{: #faqs}

## 什么是 Kubernetes？
{: #kubernetes}
{: faq}

Kubernetes 是一个开放式源代码平台，用于管理跨多个主机的容器化工作负载和服务，并提供用于部署、自动化、监视和缩放容器化应用程序的管理工具，需要的手动干预极少，甚至不需要手动干预。组成微服务的所有容器都会分组成 pod，pod 是一种逻辑单元，可确保轻松进行管理和发现。这些 pod 在可移植、可扩展并能在发生故障转移时自我复原的 Kubernetes 集群中管理的计算主机上运行。
{: shortdesc}

有关 Kubernetes 的更多信息，请参阅 [Kubernetes 文档 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/home/?path=users&persona=app-developer&level=foundational)。

## IBM Cloud Kubernetes Service 是如何运作的？
{: #kubernetes_service}
{: faq}

通过 {{site.data.keyword.containerlong_notm}}，您可以创建自己的 Kubernetes 集群，以在 {{site.data.keyword.Bluemix_notm}} 上部署和管理容器化应用程序。容器化应用程序在称为工作程序节点的 IBM Cloud Infrastructure (SoftLayer) 计算主机上托管。您可以选择将计算主机作为[虚拟机](/docs/containers?topic=containers-planning_worker_nodes#vm)（使用共享或专用资源）或[裸机机器](/docs/containers?topic=containers-planning_worker_nodes#bm)（可以针对 GPU 和软件定义的存储 (SDS) 使用情况进行优化）进行供应。工作程序节点由 IBM 配置、监视和管理的高可用性 Kubernetes 主节点进行控制。您可以通过 {{site.data.keyword.containerlong_notm}} API 或 CLI 来使用集群基础架构资源，通过 Kubernetes API 或 CLI 来管理部署和服务。

有关如何设置集群资源的更多信息，请参阅[服务体系结构](/docs/containers?topic=containers-ibm-cloud-kubernetes-service-technology#architecture)。要查找功能和优点的列表，请参阅[为什么选择 {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-cs_ov#cs_ov)。

## 为什么应该使用 IBM Cloud Kubernetes Service？
{: #faq_benefits}
{: faq}

{{site.data.keyword.containerlong_notm}} 是一个受管的 Kubernetes 产品，提供功能强大的工具、直观的用户体验和内置安全性，用于快速交付可绑定到与 IBM Watson®、AI、IoT、DevOps、安全性和数据分析相关的云服务的应用程序。作为经过认证的 Kubernetes 提供程序，{{site.data.keyword.containerlong_notm}} 具备智能安排、自我复原、水平缩放、服务发现和负载均衡、自动应用和回滚以及私钥和配置管理功能。该服务还具有与简化的集群管理、容器安全性和隔离策略相关的高级功能，设计您自己的集群的功能，以及用于实现部署一致性的集成操作工具。

有关功能和优点的详细概述，请参阅[为什么选择 {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-cs_ov#cs_ov)。

## 服务随附受管 Kubernetes 主节点和工作程序节点吗？
{: #managed_master_worker}
{: faq}

{{site.data.keyword.containerlong_notm}} 中的每个 Kubernetes 集群都由 IBM 拥有的 {{site.data.keyword.Bluemix_notm}} 基础架构帐户中 IBM 管理的专用 Kubernetes 主节点进行控制。Kubernetes 主节点（包括所有主节点组件）、计算、联网和存储资源由 IBM 现场可靠性工程师 (SRE) 持续监视。SRE 将应用最新的安全标准，检测恶意活动并进行修复，以及设法确保 {{site.data.keyword.containerlong_notm}} 的可性和可用性。IBM 会自动更新在您供应集群时自动安装的附加组件，例如用于日志记录的 Fluentd。但是，您可以选择禁用某些附加组件的自动更新，而单独从主节点和工作程序节点手动更新这些附加组件。
有关更多信息，请参阅[更新集群附加组件](/docs/containers?topic=containers-update#addons)。

Kubernetes 会定期发布[主要更新、次要更新或补丁更新](/docs/containers?topic=containers-cs_versions#version_types)。这些更新可能会影响 Kubernetes 主节点中的 Kubernetes API 服务器版本或其他组件。IBM 会自动更新补丁版本，但主节点的主版本和次版本必须由您进行更新。有关更多信息，请参阅[更新 Kubernetes 主节点](/docs/containers?topic=containers-update#master)。

标准集群中的工作程序节点将供应给 {{site.data.keyword.Bluemix_notm}} 基础架构帐户。工作程序节点专用于您的帐户，您将负责请求及时更新工作程序节点，以确保工作程序节点操作系统和 {{site.data.keyword.containerlong_notm}} 组件应用最新的安全性更新和补丁。安全性更新和补丁由 IBM 现场可靠性工程师 (SRE) 提供，他们会持续监视工作程序节点上安装的 Linux 映像，以检测漏洞和安全合规性问题。有关更多信息，请参阅[更新工作程序节点](/docs/containers?topic=containers-update#worker_node)。

## Kubernetes 主节点和工作程序节点是高可用性节点吗？
{: #faq_ha}
{: faq}

{{site.data.keyword.containerlong_notm}} 体系结构和基础架构旨在确保可靠性、处理等待时间短以及服务正常运行时间最长。缺省情况下，{{site.data.keyword.containerlong_notm}} 中的每个集群都设置为具有多个 Kubernetes 主节点实例，以确保集群资源的可用性和可访问性，即使 Kubernetes 主节点的一个或多个实例不可用也不受影响。

通过将工作负载分布在一个区域的多个专区中的多个工作程序节点上，可以进一步提高集群的高可用性，防止应用程序产生停机时间。此设置称为[多专区集群](/docs/containers?topic=containers-ha_clusters#multizone)，用于确保应用程序可访问，即使某个工作程序节点或整个专区不可用也不受影响。

要防止整个区域发生故障，请创建[多个集群并将其分布在多个 {{site.data.keyword.containerlong_notm}} 区域中](/docs/containers?topic=containers-ha_clusters#multiple_clusters)。通过为集群设置网络负载均衡器 (NLB)，可以实现对集群进行跨区域负载均衡和跨区域联网。

如果您具有即使发生中断也必须可用的数据，请确保将数据存储在[持久性存储器](/docs/containers?topic=containers-storage_planning#storage_planning)上。

有关如何为集群实现高可用性的更多信息，请参阅 [{{site.data.keyword.containerlong_notm}} 的高可用性](/docs/containers?topic=containers-ha#ha)。

## 我有哪些选项可保护集群？
{: #secure_cluster}
{: faq}

您可以使用 {{site.data.keyword.containerlong_notm}} 中的内置安全功能来保护集群中的组件以及数据和应用程序部署，以确保安全合规性和数据完整性。使用这些功能可以保护 Kubernetes API 服务器、etcd 数据存储、工作程序节点、网络、存储器、映像和部署免受恶意攻击。您还可以利用内置日志记录和监视工具来检测恶意攻击和可疑使用模式。

有关集群组件以及如何保护每个组件的更多信息，请参阅 [{{site.data.keyword.containerlong_notm}} 的安全性](/docs/containers?topic=containers-security#security)。

## 我能为集群用户提供哪些访问策略？
{: #faq_access}
{: faq}

{{site.data.keyword.containerlong_notm}} 使用 {{site.data.keyword.iamshort}} (IAM) 通过 IAM 平台角色授予对集群资源的访问权，并且还使用 Kubernetes 基于角色的访问控制 (RBAC) 策略通过 IAM 服务角色来授予对集群资源的访问权。有关访问策略类型的更多信息，请参阅[为用户选取适当的访问策略和角色](/docs/containers?topic=containers-users#access_roles)。
{: shortdesc}

分配给用户的访问策略根据您希望用户能够执行的操作而有所不同。您可以在[用户访问权参考页面](/docs/containers?topic=containers-access_reference)或下表的链接中找到有关哪些角色授权哪些操作类型的更多信息。有关分配策略的步骤，请参阅[通过 {{site.data.keyword.Bluemix_notm}} IAM 授予用户对集群的访问权](/docs/containers?topic=containers-users#platform)。

|用例|示例角色和作用域|
| --- | --- |
|应用程序审计员|[对集群、区域或资源组的查看者平台角色](/docs/containers?topic=containers-access_reference#view-actions)以及[对集群、区域或资源组的读取者服务角色](/docs/containers?topic=containers-access_reference#service)。|
|应用程序开发者|[对集群的编辑者平台角色](/docs/containers?topic=containers-access_reference#editor-actions)、[作用域限定为名称空间的写入者服务角色](/docs/containers?topic=containers-access_reference#service)以及 [Cloud Foundry 开发者空间角色](/docs/containers?topic=containers-access_reference#cloud-foundry)。|
|计费|[对集群、区域或资源组的查看者平台角色](/docs/containers?topic=containers-access_reference#view-actions)。|
|创建集群|对超级用户基础架构凭证的帐户级别许可权，对 {{site.data.keyword.containerlong_notm}} 的管理员平台角色以及对 {{site.data.keyword.registrylong_notm}} 的管理员平台角色。有关更多信息，请参阅[准备创建集群](/docs/containers?topic=containers-clusters#cluster_prepare)。|
|集群管理员|[对集群的管理员平台角色](/docs/containers?topic=containers-access_reference#admin-actions)以及[未将作用域限定为名称空间（对整个集群）的管理者服务角色](/docs/containers?topic=containers-access_reference#service)。|
|DevOps 操作员|[对集群的操作者平台角色](/docs/containers?topic=containers-access_reference#operator-actions)、[未将作用域限定为名称空间（对整个集群）的写入者服务角色](/docs/containers?topic=containers-access_reference#service)以及 [Cloud Foundry 开发者空间角色](/docs/containers?topic=containers-access_reference#cloud-foundry)。|
|操作员或站点可靠性工程师|[对集群、区域或资源组的管理员平台角色](/docs/containers?topic=containers-access_reference#admin-actions)、[对集群或区域的读取者服务角色](/docs/containers?topic=containers-access_reference#service)或[对所有集群名称空间的管理者服务角色](/docs/containers?topic=containers-access_reference#service)，以能够使用 `kubectl top nodes,pods` 命令。|
{: caption="可分配用于满足不同用例的角色类型。" caption-side="top"}

## 在哪里可以找到影响集群的安全公告的列表？
{: #faq_security_bulletins}
{: faq}

如果在 Kubernetes 中发现漏洞，Kubernetes 会在安全公告中发布 CVE 以通知用户，并描述用户必须执行哪些操作来修复漏洞。影响 {{site.data.keyword.containerlong_notm}} 用户或 {{site.data.keyword.Bluemix_notm}} 平台的 Kubernetes 安全公告会在 [{{site.data.keyword.Bluemix_notm}} 安全公告](https://cloud.ibm.com/status?component=containers-kubernetes&selected=security)中发布。

某些 CVE 需要 Kubernetes 版本的最新补丁更新，您可以在 {{site.data.keyword.containerlong_notm}} 中的常规[集群更新过程](/docs/containers?topic=containers-update#update)中安装该更新。确保及时应用安全补丁，以保护集群免受恶意攻击。有关安全补丁中包含哪些内容的信息，请参阅[版本更改日志](/docs/containers?topic=containers-changelog#changelog)。

## 服务提供对裸机和 GPU 的支持吗？
{: #bare_metal_gpu}
{: faq}

是的，您可以将工作程序节点作为单租户物理裸机服务器进行供应。裸机服务器能为工作负载（例如，数据、AI 和 GPU）带来高性能优点。此外，所有硬件资源都专供您的工作负载使用，因此您不必担心“吵闹的邻居”。

有关可用裸机类型模板以及裸机与虚拟机的差异的更多信息，请参阅[物理机器（裸机）](/docs/containers?topic=containers-planning_worker_nodes#bm)。

## 服务支持哪些 Kubernetes 版本？
{: #supported_kube_versions}
{: faq}

{{site.data.keyword.containerlong_notm}} 同时支持多个版本的 Kubernetes。发布最新版本 (n) 时，支持最多低 2 (n-2) 的版本。比最新版本低 2 以上的版本 (n-3) 将首先不推荐使用，然后不再支持。
当前支持以下版本：

*   最新版本：1.14.2
*   缺省版本：1.13.6
*   其他版本：1.12.9

有关支持的版本以及从一个版本移至另一个版本时必须执行的更新操作的更多信息，请参阅[版本信息和更新操作](/docs/containers?topic=containers-cs_versions#cs_versions)。

## 服务在哪里可用？
{: #supported_regions}
{: faq}

{{site.data.keyword.containerlong_notm}} 在全球范围内可用。您可以在每个支持的 {{site.data.keyword.containerlong_notm}} 区域中创建标准集群。免费集群仅在精选区域中可用。

有关支持的区域的更多信息，请参阅[位置](/docs/containers?topic=containers-regions-and-zones#regions-and-zones)。

## 服务符合哪些标准？
{: #standards}
{: faq}

{{site.data.keyword.containerlong_notm}} 实施与以下标准相当的控制措施：
- 欧盟-美国隐私保护和瑞士-美国隐私保护框架
- 健康保险可移植性和责任法案 (HIPAA)
- 服务组织控制标准（SOC 1 和 SOC 2 类型 1）
- 鉴证业务国际准则第 3402 号 (ISAE 3402) - 服务类组织控制鉴证报告
- 国际标准化组织（ISO 27001、ISO 27017 和 ISO 27018）
- 支付卡行业数据安全标准 (PCI DSS)

## 可以将 IBM Cloud 和其他服务用于集群吗？
{: #faq_integrations}
{: faq}

您可以将 {{site.data.keyword.Bluemix_notm}} 平台和基础架构服务以及来自第三方供应商的服务添加到 {{site.data.keyword.containerlong_notm}} 集群，以启用自动化，提高安全性或增强集群中的监视和日志记录功能。

有关支持的服务的列表，请参阅[集成服务](/docs/containers?topic=containers-supported_integrations#supported_integrations)。

## 能将 IBM Cloud Public 中的集群与内部部署数据中心内运行的应用程序相连接吗？
{: #hybrid}
{: faq}

您可以将 {{site.data.keyword.Bluemix_notm}} Public 中的服务与内部部署数据中心相连接，以创建您自己的混合云设置。如何将 {{site.data.keyword.Bluemix_notm}} Public 和 Private 与内部部署数据中心内运行的应用程序配合使用的示例包括：
- 您在 {{site.data.keyword.Bluemix_notm}} Public 中使用 {{site.data.keyword.containerlong_notm}} 创建了集群，但希望将该集群与内部部署数据库相连接。
- 您在自己的数据中心内的 {{site.data.keyword.Bluemix_notm}} Private 中创建了 Kubernetes 集群，并将应用程序部署到该集群。但是，应用程序可能会使用 {{site.data.keyword.Bluemix_notm}} Public 中的 {{site.data.keyword.ibmwatson_notm}} 服务，例如 Tone Analyzer。

要支持在 {{site.data.keyword.Bluemix_notm}} Public 中运行的服务与内部部署运行的服务之间的通信，必须[设置 VPN 连接](/docs/containers?topic=containers-vpn#vpn)。要将 {{site.data.keyword.Bluemix_notm}} Public 或 Dedicated 环境与 {{site.data.keyword.Bluemix_notm}} Private 环境相连接，请参阅[将 {{site.data.keyword.containerlong_notm}} 用于 {{site.data.keyword.Bluemix_notm}} Private](/docs/containers?topic=containers-hybrid_iks_icp#hybrid_iks_icp)。

有关支持的 {{site.data.keyword.containerlong_notm}} 产品的概述，请参阅[产品及其组合的比较](/docs/containers?topic=containers-cs_ov#differentiation)。

## 我可以在自己的数据中心部署 IBM Cloud Kubernetes Service 吗？
{: #private}
{: faq}

如果您不希望将应用程序移至 {{site.data.keyword.Bluemix_notm}} Public 中，但仍希望利用 {{site.data.keyword.containerlong_notm}} 的功能，那么可以安装 {{site.data.keyword.Bluemix_notm}} Private。{{site.data.keyword.Bluemix_notm}} Private 是可以在您自己的机器上本地安装的应用程序平台，可用于在防火墙后您自己的受控环境中开发和管理内部部署容器化应用程序。

有关更多信息，请参阅 [{{site.data.keyword.Bluemix_notm}} Private 产品文档 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_1.2.0/kc_welcome_containers.html)。

## 使用 IBM Cloud Kubernetes Service 时，将向我收取多少费用？
{: #charges}
{: faq}

通过 {{site.data.keyword.containerlong_notm}} 集群，您可以将 IBM Cloud Infrastructure (SoftLayer) 计算、联网和存储资源与 Watson AI 或 Compose 数据库即服务等平台服务配合使用。每个资源都可能产生相应的费用，该费用可能是[固定、计量、分层或保留的](/docs/billing-usage?topic=billing-usage-charges#charges)。
* [工作程序节点](#nodes)
* [出站联网](#bandwidth)
* [子网 IP 地址](#subnet_ips)
* [存储器](#persistent_storage)
* [{{site.data.keyword.Bluemix_notm}} 服务](#services)
* [Red Hat OpenShift on IBM Cloud](#rhos_charges)

<dl>
<dt id="nodes">工作程序节点</dt>
  <dd><p>集群可以有两种主要类型的工作程序节点：虚拟机或物理（裸机）机器。机器类型的可用性和定价随集群部署到的专区而有所不同。</p>
  <p>相对于裸机，使用<strong>虚拟机</strong>能以更具成本效益的价格获得更高灵活性、更短供应时间以及更多自动可扩展性功能。但是，与裸机规范（例如，网络 Gbps、RAM 和内存阈值以及存储选项）相比，VM 的性能有所折衷。请牢记以下影响 VM 成本的因素：</p>
  <ul><li><strong>共享与专用</strong>：如果共享 VM 的底层硬件，那么成本低于专用硬件，但物理资源不专供 VM 使用。</li>
  <li><strong>仅按小时计费</strong>：使用按小时计费可更灵活性地快速订购和取消 VM。
  <li><strong>每月分层小时数</strong>：按小时计费是分层的。由于 VM 始终订购的是计费月份内的小时层，因此按小时收取的费率更低。各小时层如下：0 - 150 小时、151 - 290 小时、291 - 540 小时和 541 小时以上。</li></ul>
  <p><strong>物理机器（裸机）</strong>能为工作负载（例如，数据、AI 和 GPU）带来高性能优点。此外，所有硬件资源都专供您的工作负载使用，因此您没有“吵闹的邻居”。请牢记以下影响裸机成本的因素：</p>
  <ul><li><strong>仅按月计费</strong>：所有裸机均按月计费。</li>
  <li><strong>订购过程更长</strong>：订购或取消裸机服务器后，该过程在 IBM Cloud Infrastructure (SoftLayer) 帐户中手动完成。因此，完成此过程可能需要超过一个工作日的时间。</li></ul>
  <p>有关机器规范的详细信息，请参阅[工作程序节点的可用硬件](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)。</p></dd>

<dt id="bandwidth">公共带宽</dt>
  <dd><p>带宽是指入站和出站网络流量的公共数据传输，包括进出全球数据中心的 {{site.data.keyword.Bluemix_notm}} 资源的数据传输。公共带宽按每 GB 计费。您可以通过登录到 [{{site.data.keyword.Bluemix_notm}} 控制台](https://cloud.ibm.com/)，从菜单 ![“菜单”图标](../icons/icon_hamburger.svg "“菜单”图标") 中选择**经典基础架构**，然后选择**网络 > 带宽 > 摘要**页面来查看当前带宽摘要。
  <p>查看影响公共带宽费用的以下因素：</p>
  <ul><li><strong>位置</strong>：与工作程序节点一样，费用随资源所部署到的专区而有所不同。</li>
  <li><strong>随附带宽或现收现付</strong>：工作程序节点机器可能分配有一定的每月出站联网带宽，例如 250 GB（对于 VM）或 500 GB（对于裸机）。或者，分配可能是根据 GB 使用量现收现付。</li>
  <li><strong>分层包</strong>：超过任何随附的带宽后，会根据按位置变化的分层使用量方案收费。如果超过了某个层分配量，可能还会向您收取标准数据传输费用。</li></ul>
  <p>有关更多信息，请参阅[带宽包 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/cloud/bandwidth)。</p></dd>

<dt id="subnet_ips">子网 IP 地址</dt>
  <dd><p>创建标准集群时，会订购具有 8 个公共 IP 地址的可移植公用子网，并按月向您的帐户收费。</p><p>如果基础架构帐户中有可用的子网，那么可以改为使用这些子网。使用 `--no-subnets` [标志](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_create)创建集群，然后[复用子网](/docs/containers?topic=containers-subnets#subnets_custom)。</p>
  </dd>

<dt id="persistent_storage">存储器</dt>
  <dd>供应存储器时，可以选择适合您用例的存储器类型和存储类。费用随存储器类型、位置和存储器实例的规范而有所不同。某些存储解决方案（例如，File Storage 和 Block Storage）提供了按小时和按月收费的套餐供您选择。要选择正确的存储解决方案，请参阅[规划高可用性持久性存储器](/docs/containers?topic=containers-storage_planning#storage_planning)。有关更多信息，请参阅：
  <ul><li>[NFS 文件存储器定价 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/cloud/file-storage/pricing)</li>
  <li>[块存储器定价 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/cloud/block-storage/pricing)</li>
  <li>[Object Storage 套餐 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api)</li></ul></dd>

<dt id="services">{{site.data.keyword.Bluemix_notm}} 服务</dt>
  <dd>与集群集成的每个服务都有自己的定价模型。请查看每个产品文档，并使用 {{site.data.keyword.Bluemix_notm}} 控制台来[估算成本](/docs/billing-usage?topic=billing-usage-cost#cost)。</dd>

<dt id="rhos_charges">Red Hat OpenShift on IBM Cloud</dt>
  <dd>
  <p class="preview">[Red Hat OpenShift on IBM Cloud](/docs/containers?topic=containers-openshift_tutorial) 作为 Beta 提供，用于测试 OpenShift 集群。</p>如果创建 [Red Hat OpenShift on IBM Cloud 集群](/docs/containers?topic=containers-openshift_tutorial)，那么工作程序节点将随 Red Hat Enterprise Linux 操作系统一起安装，这将增加[工作程序节点机器](#nodes)的价格。您还必须具有 OpenShift 许可证，除了每小时的 VM 成本或每月裸机成本外，还会发生每月成本。OpenShift 许可证适用于工作程序节点类型模板的每 2 个核心。如果在月末之前删除了您的工作程序节点，那么每月许可证可供工作程序池中的其他工作程序节点使用。有关 OpenShift 集群的更多信息，请参阅[创建 Red Hat OpenShift on IBM Cloud 集群](/docs/containers?topic=containers-openshift_tutorial)。</dd>

</dl>
<br><br>

按月计费的资源根据上个月的使用量从当月的第一天开始计费。如果您是在月中订购的按月计费资源，那么会根据该月按比例分配的金额向您收费。但是，如果您在月中取消按月计费资源，那么仍会向您收取该资源的整月费用。
{: note}

## 我的平台和基础架构资源是否会合并在一个帐单中？
{: #bill}
{: faq}

您使用的是计费 {{site.data.keyword.Bluemix_notm}} 帐户时，平台和基础架构资源会汇总到一个帐单中。如果您链接了 {{site.data.keyword.Bluemix_notm}} 和 IBM Cloud Infrastructure (SoftLayer) 帐户，那么将收到 {{site.data.keyword.Bluemix_notm}} 平台和基础架构资源的[合并帐单](/docs/customer-portal?topic=customer-portal-unifybillaccounts#unifybillaccounts)。

## 我能估算成本吗？
{: #cost_estimate}
{: faq}

可以，请参阅[估算成本](/docs/billing-usage?topic=billing-usage-cost#cost)。请记住，某些费用不会反映在估算中，例如针对增加的每小时使用量的分层定价。有关更多信息，请参阅[使用 {{site.data.keyword.containerlong_notm}} 时，将向我收取多少费用？](#charges)。

## 可以查看我的当前使用情况吗？
{: #usage}
{: faq}

您可以检查 {{site.data.keyword.Bluemix_notm}} 平台和基础架构资源的当前使用情况以及每月估算总计。有关更多信息，请参阅[查看使用情况](/docs/billing-usage?topic=billing-usage-viewingusage#viewingusage)。要组织计费，可以使用[资源组](/docs/resources?topic=resources-bp_resourcegroups#bp_resourcegroups)对资源进行分组。
