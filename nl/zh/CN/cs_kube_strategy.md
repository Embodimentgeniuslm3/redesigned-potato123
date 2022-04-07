---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-03"

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


# 定义 Kubernetes 策略
{: #strategy}

通过 {{site.data.keyword.containerlong}}，您可以快速、安全地在生产环境中为应用程序部署容器工作负载。请了解更多信息，以便在规划集群策略时，可优化设置以充分利用 [Kubernetes ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/) 的自动部署、缩放和编排管理功能。
{:shortdesc}

## 将工作负载移至 {{site.data.keyword.Bluemix_notm}}
{: #cloud_workloads}

出于多种原因，您需要将工作负载移至 {{site.data.keyword.Bluemix_notm}}：降低总体拥有成本，在安全且合规的环境中提高应用程序的高可用性，根据用户的需求进行扩展和缩减等。{{site.data.keyword.containerlong_notm}} 将容器技术与开放式源代码工具（如 Kubernetes）相组合，以便您可以构建能在不同云环境之间迁移的云本机应用程序，避免被供应商套牢。
{:shortdesc}

但是如何进入云呢？在此过程中，您有哪些选择？在进入云之后，如何管理工作负载？

使用此页面可了解 {{site.data.keyword.containerlong_notm}} 上针对 Kubernetes 部署的一些策略。请在 [Slack ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://ibm-container-service.slack.com) 上随时与我们的团队联系。

还没有使用过 Slack？可以[请求邀请！](https://bxcs-slack-invite.mybluemix.net/)
{: tip}

### 可以将哪些内容移至 {{site.data.keyword.Bluemix_notm}}？
{: #move_to_cloud}

使用 {{site.data.keyword.Bluemix_notm}} 时，您可以灵活地在[外部部署、内部部署或混合云环境](/docs/containers?topic=containers-cs_ov#differentiation)中创建 Kubernetes 集群。下表就用户通常会移至各种类型云的工作负载类型提供了一些示例。
您还可以选择使集群同时在两种环境中运行的混合方法。
{: shortdesc}

|工作负载|{{site.data.keyword.containershort_notm}} 外部部署|内部部署|
| --- | --- | --- |
|DevOps 支持工具| <img src="images/confirm.svg" width="32" alt="功能可用" style="width:32px;" />| |
|开发和测试应用程序| <img src="images/confirm.svg" width="32" alt="功能可用" style="width:32px;" />| |
|应用程序的需求发生重大变化，需要快速缩放| <img src="images/confirm.svg" width="32" alt="功能可用" style="width:32px;" />| |
|业务应用程序（例如，CRM、HCM、ERP 和电子商务）| <img src="images/confirm.svg" width="32" alt="功能可用" style="width:32px;" />| |
|协作和社交工具（例如，电子邮件）| <img src="images/confirm.svg" width="32" alt="功能可用" style="width:32px;" />| |
|Linux 和 x86 工作负载| <img src="images/confirm.svg" width="32" alt="功能可用" style="width:32px;" />| |
|裸机和 GPU 计算资源| <img src="images/confirm.svg" width="32" alt="功能可用" style="width:32px;" />| <img src="images/confirm.svg" width="32" alt="功能可用" style="width:32px;" />|
|符合 PCI 和 HIPAA 的工作负载| <img src="images/confirm.svg" width="32" alt="功能可用" style="width:32px;" />| <img src="images/confirm.svg" width="32" alt="功能可用" style="width:32px;" />|
|具有平台和基础架构约束和依赖项的旧应用程序| | <img src="images/confirm.svg" width="32" alt="功能可用" style="width:32px;" />|
|采用严格设计、许可或遵循严密监管的专有应用程序| | <img src="images/confirm.svg" width="32" alt="功能可用" style="width:32px;" />|
|在公共云中缩放应用程序，并将数据同步到现场专用数据库| <img src="images/confirm.svg" width="32" alt="功能可用" style="width:32px;" />| <img src="images/confirm.svg" width="32" alt="功能可用" style="width:32px;" />|
{: caption="{{site.data.keyword.Bluemix_notm}} 实现支持工作负载" caption-side="top"}

**已准备好在 {{site.data.keyword.containerlong_notm}} 中外部部署运行工作负载？**</br>
太好了！您正在阅读的是公共云文档。请继续阅读以了解更多策略构想，或者通过[立即创建集群](/docs/containers?topic=containers-getting-started)来开始运作。

**对内部部署云感兴趣？**</br>
请探索 [{{site.data.keyword.Bluemix_notm}} Private 文档 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.1/kc_welcome_containers.html)。如果您已经对 IBM 技术（例如，WebSphere Application Server 和 Liberty）进行了大量投资，那么可以使用各种工具来优化 {{site.data.keyword.Bluemix_notm}} Private 现代化策略。

**要同时在内部部署云和外部部署云中运行工作负载？**</br>
首先设置 {{site.data.keyword.Bluemix_notm}} Private 帐户。然后，请参阅[将 {{site.data.keyword.containerlong_notm}} 与 {{site.data.keyword.Bluemix_notm}} Private 配合使用](/docs/containers?topic=containers-hybrid_iks_icp)，以将 {{site.data.keyword.Bluemix_notm}} Private 环境与 {{site.data.keyword.Bluemix_notm}} Public 中的集群相连接。要管理多个云 Kubernetes 集群（如跨 {{site.data.keyword.Bluemix_notm}} Public 和 {{site.data.keyword.Bluemix_notm}} Private），请查看 [IBM Multicloud Manager ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.1.0/mcm/getting_started/introduction.html)。

### 可以在 {{site.data.keyword.containerlong_notm}} 中运行哪种应用程序？
{: #app_types}

容器化应用程序必须能够在支持的操作系统（Ubuntu 16.64 和 18.64）上运行。您还需要考虑应用程序的有状态性。
{: shortdesc}

<dl>
<dt>无状态应用程序</dt>
  <dd><p>对于云本机环境（如 Kubernetes），首选无状态应用程序。这种应用程序的迁移和缩放都很简单，因为它们声明依赖项，将配置与代码分开存储，并将后备服务（例如，数据库）视为连接的资源，而不是耦合到应用程序。应用程序 pod 无需持久数据存储或稳定的网络 IP 地址，因此可以根据工作负载需求来终止、重新安排和缩放 pod。应用程序使用数据库即服务来持久存储数据，使用 NodePort、LoadBalancer 或 Ingress 服务在稳定的 IP 地址上公开工作负载。</p></dd>
<dt>有状态应用程序</dt>
  <dd><p>相比无状态应用程序，有状态应用程序的设置、管理和缩放都更加复杂，因为 pod 需要持久数据和稳定的网络身份。有状态应用程序通常是数据库或其他分布式数据密集型工作负载，在这些工作负载中，处理的效率更接近数据本身。</p>
  <p>如果要部署有状态应用程序，那么需要设置持久存储器，并将持久卷安装到由 StatefulSet 对象控制的 pod。可以选择将[文件](/docs/containers?topic=containers-file_storage#file_statefulset)存储器、[块](/docs/containers?topic=containers-block_storage#block_statefulset)存储器或[对象](/docs/containers?topic=containers-object_storage#cos_statefulset)存储器添加为有状态集的持久存储器。您还可以在裸机工作程序节点上安装 [Portworx](/docs/containers?topic=containers-portworx)，然后使用 Portworx 作为高可用性软件定义的存储解决方案来管理有状态应用程序的持久存储器。有关有状态集如何运作的更多信息，请参阅 [Kubernetes 文档 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/)。</p></dd>
</dl>

### 开发无状态云本机应用程序的一些准则是什么？
{: #12factor}

请查看 [Twelve-Factor App ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://12factor.net/)，这是一种与语言无关的方法，用于考虑如何根据十二要素开发应用程序，具体概述如下。
{: shortdesc}

1.  **代码库**：在部署的版本控制系统中使用单个代码库。拉取用于容器部署的映像时，指定已测试映像标记，而不是使用 `latest`。
2.  **依赖项**：显式声明和隔离外部依赖项。
3.  **配置**：在环境变量中（而不是在代码中）存储特定于部署的配置。
4.  **后备服务**：将后备服务（例如，数据存储或消息队列）视为已连接或可替换的资源。
5.  **应用程序阶段**：在不同的阶段（例如，`build`、`release` 和 `run`）中进行构建，并严格分离各个阶段。
6.  **进程**：作为不共享任何内容的一个或多个无状态进程运行，并使用[持久存储器](/docs/containers?topic=containers-storage_planning)来保存数据。
7.  **端口绑定**：端口绑定是自包含的，并且在定义明确的主机和端口上提供服务端点。
8.  **并行**：通过进程实例（例如，副本和水平缩放）来管理和缩放应用程序。为部署设置资源请求和限制。请注意，Calico 网络策略无法限制带宽。请改为考虑使用 [Istio](/docs/containers?topic=containers-istio)。
9.  **一次性**：将应用程序设计为一次性的，启动工作极少，可正常关闭，并能容忍进程突然终止。请记住，容器、pod 甚至工作程序节点都应该是一次性的，因此请相应地规划应用程序。
10.  **开发到生产保持一致**：为应用程序设置[持续集成](https://www.ibm.com/cloud/garage/content/code/practice_continuous_integration/)和[持续交付](https://www.ibm.com/cloud/garage/content/deliver/practice_continuous_delivery/)管道，尽可能减小开发环境中的应用程序与生产环境中的应用程序之间的差异。
11.  **日志**：将日志视为事件流：外部或托管环境会处理和路由日志文件。**重要信息**：在 {{site.data.keyword.containerlong_notm}} 中，缺省情况下不会开启日志。要启用，请参阅[配置日志转发](/docs/containers?topic=containers-health#configuring)。
12.  **管理进程**：将任何一次性管理脚本与应用程序一起保留，并将这些脚本作为 [Kubernetes 作业对象 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/workloads/controllers/jobs-run-to-completion/) 运行，以确保管理脚本的运行环境与应用程序本身相同。要对您希望在 Kubernetes 集群中运行的较大型软件包进行编排，请考虑使用软件包管理器，例如 [Helm ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://helm.sh/)。

### 我已有应用程序。如何将其迁移到 {{site.data.keyword.containerlong_notm}}？
{: #migrate_containerize}

您可以执行一些常规步骤来对应用程序进行容器化，如下所示。
{: shortdesc}

1.  使用 [Twelve-Factor App ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://12factor.net/) 作为指南来隔离依赖项，将进程分隔成单独的服务，以及尽可能减少应用程序的有状态性。
2.  查找要使用的相应基本映像。可以使用 [Docker Hub ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://hub.docker.com/) 中公开可用的映像以及[公共 IBM 映像](/docs/services/Registry?topic=registry-public_images#public_images)，也可以在专用 {{site.data.keyword.registryshort_notm}} 中构建和管理您自己的映像。
3.  仅将运行应用程序所必需的内容添加到 Docker 映像。
4.  不要依赖本地存储器，而是改为计划使用持久存储器或云数据库即服务解决方案来备份应用程序的数据。
5.  随着时间的推移，将应用程序进程重构为微服务。

有关更多信息，请参阅以下教程：
*  [将应用程序从 Cloud Foundry 迁移到集群](/docs/containers?topic=containers-cf_tutorial#cf_tutorial)
*  [将基于 VM 的应用程序移至 Kubernetes](/docs/tutorials?topic=solution-tutorials-vm-to-containers-and-kubernetes)



请继续阅读以下主题，了解移动工作负载时的更多注意事项，例如 Kubernetes 环境、高可用性、服务发现和部署。

<br />


### 在将应用程序移至 {{site.data.keyword.containerlong_notm}} 之前，最好具备哪些知识和技术技能？
{: #knowledge}

Kubernetes 旨在为两个主要角色提供功能：集群管理员和应用程序开发者。每个角色使用不同的技术技能，以成功运行应用程序并将应用程序部署到集群
。
{: shortdesc}

**集群管理员的主要任务和需要具备的技术知识是什么？** </br>
作为集群管理员，您负责设置、操作、保护和管理集群的 {{site.data.keyword.Bluemix_notm}} 基础架构。典型任务包括：
- 调整集群的大小，以便为工作负载提供足够的容量。
- 设计集群，以满足公司的高可用性、灾难恢复和合规标准。
- 通过设置用户许可权并限制集群中的操作来保护计算资源、网络和数据，从而确保集群安全。
- 规划和管理基础架构组件之间的网络通信，以确保网络安全性、分段和合规性。
- 规划持久性存储器选项，以满足数据存储位置和数据保护需求。

集群管理员角色必须具备丰富的知识，包括计算、网络、存储、安全性和合规性。在典型的公司中，这些知识一般由多个专家分别掌握，例如系统工程师、系统管理员、网络工程师、网络架构设计师、IT 经理或安全与合规专家。请考虑将集群管理员角色分配给公司中的多个人员，以便您具备成功操作集群所需的知识。

**应用程序开发者的主要任务和需要具备的技术知识是什么？** </br>
作为开发者，您要在 Kubernetes 集群中设计、创建、保护、部署、测试、运行和监视云本机容器化应用程序。要创建和运行这些应用程序，您必须熟悉微服务的概念、[12 因子应用程序](#12factor)准则、[Docker 和容器化原则](https://www.docker.com/)以及可用的 [Kubernetes 部署选项](/docs/containers?topic=containers-app#plan_apps)。如果要部署无服务器应用程序，请熟悉 [Knative](/docs/containers?topic=containers-cs_network_planning)。

Kubernetes 和 {{site.data.keyword.containerlong_notm}} 提供了有关如何[公开应用程序并使应用程序保持专用](/docs/containers?topic=containers-cs_network_planning)、[添加持久性存储器](/docs/containers?topic=containers-storage_planning)、[集成其他服务](/docs/containers?topic=containers-ibm-3rd-party-integrations)以及如何[保护工作负载和敏感数据](/docs/containers?topic=containers-security#container)的多个选项。在将应用程序移至 {{site.data.keyword.containerlong_notm}} 中的集群之前，请验证是否可以在支持的 Ubuntu 16.64 和 18.64 操作系统上将应用程序作为容器化应用程序运行，以及 Kubernetes 和 {{site.data.keyword.containerlong_notm}} 是否提供了工作负载所需的功能。

**集群管理员和开发者之间要相互交互吗？**</br>
是的。集群管理员和开发者必须经常进行交互，通过交互，集群管理员可了解在集群中提供此功能的工作负载需求，而开发者可了解其应用程序开发过程中必须考虑的可用限制、集成和安全性原则。

## 调整 Kubernetes 集群的大小以支持工作负载
{: #sizing}

了解集群中需要多个工作程序节点来支持工作负载并不是精密科学。您可能需要测试其他配置并进行相应调整。但使用 {{site.data.keyword.containerlong_notm}} 的优点是，可以根据工作负载需求来添加和除去工作程序节点。
{: shortdesc}

要开始调整集群的大小，请先自问以下问题。

### 应用程序需要多少资源？
{: #sizing_resources}

首先，了解现有工作负载使用情况或进行预估。

1.  计算工作负载的平均 CPU 和内存使用量。例如，可以在 Windows 计算机上查看“任务管理器”，或者在 Mac 或 Linux 上运行 `top` 命令。 还可以使用度量值服务并运行报告来计算工作负载使用情况。
2.  预测工作负载必须处理的请求数，以便决定需要多少应用程序副本来处理工作负载。例如，您可能将应用程序实例设计为每分钟处理 1000 个请求，并预测工作负载必须每分钟处理 10000 个请求。对于此情况，您可能决定创建 12 个应用程序副本，其中 10 个用于处理预测量，另外 2 个用于处理激增容量。

### 除了应用程序外，还有其他哪些对象可能会使用集群中的资源？
{: #sizing_other}

现在我们来添加您可能会使用的其他一些功能。



1.  考虑应用程序是否会拉取大型映像或大量映像，这可能会占用工作程序节点上的本地存储器。
2.  决定是否要将服务[集成](/docs/containers?topic=containers-supported_integrations#supported_integrations)到集群中，例如 [Helm](/docs/containers?topic=containers-helm#public_helm_install) 或 [Prometheus ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/coreos/prometheus-operator/tree/master/contrib/kube-prometheus)。这些集成的服务和附加组件会启动使用集群资源的 pod。

### 我希望工作负载具有哪种类型的可用性？
{: #sizing_availability}

请记住，您希望工作负载尽可能持续可用！

1.  规划针对[高可用性集群](/docs/containers?topic=containers-ha_clusters#ha_clusters)的策略，例如决定是使用单专区集群还是多专区集群。
2.  查看[高可用性部署](/docs/containers?topic=containers-app#highly_available_apps)，以帮助决定如何使应用程序可用。

### 需要多少个工作程序节点来处理工作负载？
{: #sizing_workers}

既然您已非常了解工作负载的情况，下面可将估算的使用量与可用集群配置对应起来。

1.  估算最大工作程序节点容量，这取决于您拥有的集群类型。您并不希望达到最大工作程序节点容量，以防发生激增或其他临时事件。
    *  **单专区集群**：计划在集群中至少有 3 个工作程序节点。此外，需要在集群中提供可容纳 1 个额外节点的 CPU 和内存容量。
    *  **多专区集群**：计划每个专区至少有 2 个工作程序节点，因此在 3 个专区中总共有 6 个节点。此外，将集群的总容量计划为至少等于总工作负载所需容量的 150%，这样，如果一个专区停止运行，您仍有资源可用于维持工作负载。
2.  将应用程序大小和工作程序节点容量对应于其中一个[可用工作程序节点类型模板](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)。要查看专区中的可用类型模板，请运行 `ibmcloud ks machine-types <zone>`。
    *   **不要使工作程序节点超负荷**：为了避免 pod 争用 CPU 或低效运行，您必须了解应用程序需要的资源，以便可以规划所需的工作程序节点数。例如，如果应用程序需要的资源少于工作程序节点上的可用资源，那么可以限制部署到一个工作程序节点的 pod 数。使工作程序节点保持在约 75% 容量的水平，以便为可能需要安排的其他 pod 留出空间。如果应用程序需要的资源超过工作程序节点上的可用资源，请使用可以满足这些需求的其他工作程序节点类型模板。当工作程序节点由于内存或其他资源不足而频繁回报 `NotReady` 状态或逐出 pod 时，即表明工作程序节点超负荷了。
    *   **大型与小型工作程序节点类型模板**：大型节点可能比小型节点更有成本效益，对于设计为在高性能机器上进行处理时获取高效率的工作负载尤其如此。但是，如果大型工作程序节点停止运行，您需要确保集群有足够的容量，可将所有工作负载 pod 正常重新安排到集群中的其他工作程序节点上。小型工作程序有助于更正常地进行缩放。
    *   **应用程序副本数**：要确定所需的工作程序节点数，您还可以考虑要运行的应用程序副本数。例如，如果您知道工作负载需要 32 个 CPU 核心，并且计划运行 16 个应用程序副本，那么每个副本 pod 需要 2 个 CPU 核心。如果希望每个工作程序节点仅运行一个应用程序 pod，那么可以为集群类型订购相应数量的工作程序节点以支持此配置。
3.  对典型等待时间、可伸缩性、数据集和工作负载需求运行性能测试，以持续优化集群中需要的工作程序节点数。
4.  对于需要根据资源请求进行扩展和缩减的工作负载，请设置[水平 pod 自动缩放器](/docs/containers?topic=containers-app#app_scaling)和[集群工作程序池自动缩放器](/docs/containers?topic=containers-ca#ca)。

<br />


## 设计 Kubernetes 环境结构
{: #kube_env}

{{site.data.keyword.containerlong_notm}} 仅链接到一个 IBM Cloud Infrastructure (SoftLayer) 产品服务组合。在帐户中，可以创建由主节点和各种工作程序节点组成的集群。IBM 负责管理主节点，您可以创建工作程序池（将同一类型模板的不同机器汇聚在一起）或内存和 CPU 规范的组合。在集群中，可以使用名称空间和标签来进一步组织资源。选择集群、机器类型和组织策略的正确组合，以便可以确保团队和工作负载获得所需的资源。
{:shortdesc}

### 应该获取哪种集群类型和机器类型？
{: #env_flavors}

**集群类型**：决定是需要[单专区集群、多专区集群还是多集群设置](/docs/containers?topic=containers-ha_clusters#ha_clusters)。多专区集群在[所有六个全球 {{site.data.keyword.Bluemix_notm}} 大城市区域](/docs/containers?topic=containers-regions-and-zones#zones)都可用。另请记住，工作程序节点因专区而异。

**工作程序节点类型**：一般而言，密集型工作负载更适合在裸机物理机器上运行，而对于有成本效益的测试和开发工作，您可选择共享或专用共享硬件上的虚拟机。使用裸机工作程序节点时，集群的网络速度为 10 Gbps，具有超线程核心，可提供更高吞吐量。虚拟机随附的网络速度为 1 Gbps，具有不提供超线程的常规核心。[查看可用的机器隔离和类型模板](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)。

### 我应该使用多个集群，还是只需向现有集群添加更多工作程序？
{: #env_multicluster}

您创建的集群数取决于工作负载、公司政策和条例以及要对计算资源执行的操作。您还可以在[容器隔离和安全性](/docs/containers?topic=containers-security#container)中查看有关此决策的安全信息。

**多个集群**：需要设置[全局负载均衡器](/docs/containers?topic=containers-ha_clusters#multiple_clusters)，并在每个集群中复制并应用相同的配置 YAML 文件，以在各集群中均衡工作负载。因此，多个集群的管理工作通常更复杂，但可以帮助您实现重要目标，如以下目标。
*  符合要求隔离工作负载的安全策略。
*  测试应用程序如何在不同版本的 Kubernetes 或其他集群软件（如 Calico）中运行。
*  创建应用程序位于另一个区域中的集群，以提高该地理区域中用户的性能。
*  在集群实例级别配置用户访问权，而不是在名称空间级别定制和管理多个 RBAC 策略来控制集群中的访问权。

**更少或单个集群**：在资源固定不变的情况下，集群越少，操作工作量越少，每个集群的成本越低。您可以不创建更多集群，而改为针对可用于要使用的应用程序和服务组件的不同机器类型的计算资源，添加工作程序池。在开发应用程序时，应用程序使用的资源位于同一专区中，或者在多专区中以其他方式密切关联，以便您可以对等待时间、带宽或相关故障进行假设。不过正因如此，使用名称空间、资源配额和标签来组织集群变得更加重要。

### 如何在集群中设置资源？
{: #env_resources}

<dl>
<dt>请考虑工作程序节点容量。</dt>
  <dd>要最充分地利用工作程序节点的性能，请考虑以下各项：
  <ul><li><strong>保持核心力量</strong>：每个机器都有一定数量的核心。根据应用程序的工作负载，为每个核心的 pod 数设置限制，例如 10 个。</li>
  <li><strong>避免节点超负荷</strong>：同样，虽然一个节点可以包含 100 个以上的 pod，但这并不意味着您希望出现这种情况。根据应用程序的工作负载，为每个节点的 pod 数设置限制，例如 40 个。</li>
  <li><strong>不要过度利用集群带宽</strong>：请记住，缩放虚拟机上的网络带宽大约为 1000 Mbps。如果集群中需要数百个工作程序节点，请将其拆分成多个集群，每个集群具有较少的节点，或者订购裸机节点。</li>
  <li><strong>对服务分类</strong>：部署之前，规划工作负载需要多少服务。联网和端口转发规则会放入 Iptables 中。如果预测有更多数量的服务（例如，超过 5,000 个服务)，请将集群拆分为多个集群。</li></ul></dd>
<dt>针对计算资源的组合，供应不同类型的机器。</dt>
  <dd>每个人都希望有权进行选择，对吧？通过 {{site.data.keyword.containerlong_notm}}，您可以部署[机器类型的组合](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)：从针对密集型工作负载的裸机到针对快速缩放的虚拟机。使用标签或名称空间来将部署组织到您的各个机器。创建部署时，请对其进行限制，以便应用程序的 pod 只能在具有正确资源组合的机器上进行部署。例如，您可能希望将数据库应用程序限制为部署到具有大容量本地磁盘存储器的裸机机器，如 `md1c.28x512.4x4tb`。</dd>
<dt>如果您有共享集群的多个团队和项目，请设置多个名称空间。</dt>
  <dd><p>名称空间有点类似于集群中的集群。名称空间可用于使用[资源配额 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/policy/resource-quotas/) 和[缺省限制 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/tasks/administer-cluster/memory-default-namespace/) 来划分集群资源。创建新的名称空间时，请确保设置正确的 [RBAC 策略](/docs/containers?topic=containers-users#rbac)来控制访问权。有关更多信息，请参阅 Kubernetes 文档中的[使用名称空间共享集群 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/tasks/administer-cluster/namespaces/)。</p>
  <p>如果您有一个小型集群、数十个用户以及类似的资源（例如，同一软件的不同版本），那么可能并不需要多个名称空间。您可以改为使用标签。</p></dd>
<dt>设置资源配额，以便集群中的用户必须使用资源请求和限制</dt>
  <dd>要确保每个团队都有必需的资源来部署服务并在集群中运行应用程序，必须为每个名称空间设置[资源配额](https://kubernetes.io/docs/concepts/policy/resource-quotas/)。资源配额确定了名称空间的部署约束，例如可以部署的 Kubernetes 资源数以及这些资源可使用的 CPU 数和内存量。设置配额后，用户必须在其部署中包含资源请求和限制。</dd>
<dt>使用标签组织 Kubernetes 对象</dt>
  <dd><p>要组织并选择 Kubernetes 资源（例如，`pod` 或`节点`），请[使用 Kubernetes 标签 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/)。缺省情况下，{{site.data.keyword.containerlong_notm}} 会应用一些标签，包括 `arch`、`os`、`region`、`zone` 和 `machine-type`。</p>
  <p>标签的示例用例包括[将网络流量限制为流至边缘工作程序节点](/docs/containers?topic=containers-edge)、[将应用程序部署到 GPU 机器](/docs/containers?topic=containers-app#gpu_app)，以及[限制应用程序工作负载 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/)，使其只能在满足特定机器类型或 SDS 功能的工作程序节点（例如，裸机工作程序节点）上运行。要查看已应用于资源的标签，请使用带有 <code>--show-labels</code> 标志的 <code>kubectl get</code> 命令。例如：</p>
  <p><pre class="pre"><code>kubectl get node &lt;node_ID&gt; --show-labels</code></pre></p>
  要将标签应用于工作程序节点，请使用标签[创建工作程序池](/docs/containers?topic=containers-add_workers#add_pool)或[更新现有工作程序池](/docs/containers?topic=containers-add_workers#worker_pool_labels)。</dd>
</dl>




<br />


## 使资源具有高可用性
{: #kube_ha}

虽然没有任何系统能完全防止故障发生，但您可以采取措施来提高 {{site.data.keyword.containerlong_notm}} 中应用程序和服务的高可用性。
{:shortdesc}

查看有关使资源具有高可用性的更多信息。
* [减少潜在故障点](/docs/containers?topic=containers-ha#ha)。
* [创建多专区集群](/docs/containers?topic=containers-ha_clusters#ha_clusters)。
* [规划高可用性部署](/docs/containers?topic=containers-app#highly_available_apps)，此类部署在多专区中使用副本集和 pod 反亲缘关系等功能。
* [运行基于云端公共注册表中映像的容器](/docs/containers?topic=containers-images)。
* [规划数据存储](/docs/containers?topic=containers-storage_planning#persistent_storage_overview)。尤其是对于多专区集群，请考虑使用云服务，例如 [{{site.data.keyword.cloudant_short_notm}}](/docs/services/Cloudant?topic=cloudant-getting-started#getting-started) 或 [{{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-about)。
* 对于多专区集群，启用 [LoadBalancer 服务](/docs/containers?topic=containers-loadbalancer#multi_zone_config)或 Ingress [多专区负载均衡器](/docs/containers?topic=containers-ingress#ingress)来以公共方式公开应用程序。

<br />


## 设置服务发现
{: #service_discovery}

Kubernetes 集群中的每个 pod 都具有一个 IP 地址。但是，将应用程序部署到集群时，您并不希望依赖 pod IP 地址来执行服务发现和联网。pod 会以动态方式频繁被除去和替换。因此，请改为使用 Kubernetes 服务，该服务表示一组 pod，并通过服务的虚拟 IP 地址（称为其`集群 IP`）提供稳定的入口点。有关更多信息，请参阅关于[服务 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/services-networking/service/#discovering-services) 的 Kubernetes 文档。
{:shortdesc}

### 可以定制 Kubernetes 集群 DNS 提供程序吗？
{: #services_dns}

创建服务和 pod 时，会为其分配 DNS 名称，以便应用程序容器可以使用 DNS 服务 IP 来解析 DNS 名称。可以定制 pod DNS 来指定名称服务器、搜索和对象列表选项。有关更多信息，请参阅[配置集群 DNS 提供程序](/docs/containers?topic=containers-cluster_dns#cluster_dns)。
{: shortdesc}



### 如何确保服务连接到正确的部署并准备好运行？
{: #services_connected}

对于大多数服务，请将选择器添加到服务 `.yaml` 文件，以便它应用于通过该标签运行应用程序的 pod。很多时候在应用程序首次启动时，您并不希望它立即处理请求。向部署添加就绪性探测器，以便将流量仅发送到被视为准备就绪的 pod。有关具有使用标签和设置就绪性探测器的服务的部署示例，请查看此 [NGINX YAML](https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/deploy-apps-clusters/nginx_preferredAntiAffinity.yaml)。
{: shortdesc}

有时，您不希望服务使用标签。例如，您可能有外部数据库，或者希望将服务指向集群内其他名称空间中的其他服务。发生这种情况时，您必须手动添加端点对象，并将其链接到服务。


### 如何控制集群中运行的服务之间的网络流量？
{: #services_network_traffic}

缺省情况下，pod 可以与集群中的其他 pod 进行通信，但您可以使用网络策略来阻止流量流至特定 pod 或名称空间。此外，如果使用 NodePort、LoadBalancer、Ingress 服务向外部公开了应用程序，那么您可能希望设置高级网络策略来阻止流量。在 {{site.data.keyword.containerlong_notm}} 中，可以使用 Calico 来管理 Kubernetes 和 Calico [网络策略以控制流量](/docs/containers?topic=containers-network_policies#network_policies)。

如果您有跨平台运行的各种微服务，需要连接、管理和保护这些平台的网络流量，请考虑使用服务网工具，例如[受管 Istio 附加组件](/docs/containers?topic=containers-istio)。

您还可以[设置边缘节点](/docs/containers?topic=containers-edge#edge)，以通过将联网工作负载限制为分布到所选工作程序节点，提高集群的安全性和隔离性。



### 如何在因特网上公开服务？
{: #services_expose_apps}

可以为外部联网创建三种类型的服务：NodePort、LoadBalancer 和 Ingress。有关更多信息，请参阅[规划联网服务](/docs/containers?topic=containers-cs_network_planning#external)。

在规划集群中需要多少`服务`对象时，请记住，Kubernetes 是使用 `iptables` 来处理联网和端口转发规则。如果在集群中运行大量服务，如 5000 个服务，那么性能可能会受到影响。



## 将应用程序工作负载部署到集群
{: #deployments}

使用 Kubernetes 时，可在 YAML 配置文件中声明许多类型的对象，例如 pod、部署和作业。这些对象描述多种内容，如正在运行的容器化应用程序、这些应用程序使用的资源以及管理其重新启动、更新、复制等操作行为的策略。有关更多信息，请参阅 Kubernetes 文档以了解[配置最佳实践 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/configuration/overview/)。
{: shortdesc}

### 我以为自己需要把应用程序放入容器中。现在，pod 中都有哪些相关内容？
{: #deploy_pods}

[pod ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/workloads/pods/pod/) 是 Kubernetes 可以管理的最小可部署单元。将容器（或一组容器）放入 pod 中，并使用 pod 配置文件来指示 pod 如何运行容器以及与其他 pod 共享资源。放入 pod 中的所有容器都在共享上下文中运行，这意味着这些容器共享同一虚拟机或物理机器。
{: shortdesc}

**放入容器中的内容**：考虑应用程序的组件时，请考虑这些组件对于 CPU 和内存等是否有明显不同的资源需求。对于以最佳性能运行的某些组件，在将资源转移到其他区域期间，可以接受这些组件停止运行一小会儿吗？是否有面向客户的其他组件，使其保持运行至关重要？如果有这类组件，请将其拆分放入不同的容器中。您始终可以将这些组件部署到同一 pod，使其同步运行。

**放入 pod 中的内容**：应用程序的容器不一定必须位于同一 pod 中。事实上，如果有一个有状态且难以缩放的组件（例如，数据库服务），请将其放入其他 pod 中，然后可以将该 pod 安排在使用更多资源来处理工作负载的工作程序节点上。如果在不同工作程序节点上运行的容器可以正常工作，请使用多个 pod。如果容器需要位于同一机器上并一起缩放，请将容器分组到同一 pod 中。

### 那么，如果可以只使用 pod，为什么还需要所有这些不同类型的对象呢？
{: #deploy_objects}

创建 pod YAML 文件非常简单。您可以编写只包含下面几行的 pod YAML 文件。

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx
    ports:
    - containerPort: 80
```
{: codeblock}

但是，您并不希望就此止步。如果运行 pod 所在的节点停止运行，那么 pod 会随之停止运行，并且不会重新安排该 pod。请改为使用[部署 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) 来支持 pod 重新安排、副本集和滚动更新。创建基本部署几乎与创建 pod 一样容易。但是，不要在 `spec` 中定义容器本身，请改为在部署 `spec` 中指定 `replicas` 和 `template`。对于模板中的容器，模板具有自己的 `spec`，如下所示。

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 80
```
{: codeblock}

可以持续添加功能，例如 pod 反亲缘关系或资源限制，所有这些都在同一 YAML 文件中添加。

有关可以添加到部署的不同功能的更详细说明，请参阅[创建应用程序部署 YAML 文件](/docs/containers?topic=containers-app#app_yaml)。
{: tip}

### 如何组织部署以使其更容易更新和管理？
{: #deploy_organize}

既然您已非常了解要在部署中包含哪些内容，接下来您可能会想了解如何管理所有这些不同的 YAML 文件？当然还包括这些文件在 Kubernetes 环境中创建的对象！

组织部署 YAML 文件的一些提示：
*  使用版本控制系统，例如 Git。
*  将紧密相关的 Kubernetes 对象分组在单个 YAML 文件中。例如，如果要创建 `deployment`，那么还可以将 `service` 文件添加到 YAML。请使用 `---` 来分隔对象，如下所示：
    ```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
    ...
    ---
apiVersion: v1
    kind: Service
    metadata:
      ...
    ```
    {: codeblock}
*  可以使用 `kubectl apply -f` 命令来应用于整个目录，而不仅仅是单个文件。
*  请试用 [`kusomize` 项目](/docs/containers?topic=containers-app#kustomize)，此项目可用于帮助编写、定制和复用 Kubernetes 资源 YAML 配置。

在 YAML 文件中，可以使用标签或注释作为元数据来管理部署。

**标签**：[标签 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/) 是可连接到 Kubernetes 对象（例如，pod 和部署）的 `key:value` 对。标签可以是您所需的任何内容，并且在根据标签信息选择对象时非常有用。标签是对对象分组的基础。下面是关于标签的一些构想：
* `app: nginx`
* `version: v1`
* `env: dev`

**注释**：[注释 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/overview/working-with-objects/annotations/) 类似于标签，因为它们也是 `key:value` 对。注释更适用于工具或库可以利用的非标识信息，例如保存有关对象来源的额外信息、如何使用对象的信息、指向相关跟踪存储库的指针或有关对象的策略。您不可基于注释来选择对象。

### 我还能做什么来准备应用程序进行部署？
{: #deploy_prep}

有很多工作可做！请参阅[准备容器化应用程序以在集群中运行](/docs/containers?topic=containers-app#plan_apps)。该主题包含有关以下内容的信息：
*  可以在 Kubernetes 中运行的应用程序类型，包括有状态和无状态应用程序的提示。
*  将应用程序迁移到 Kubernetes。
*  根据工作负载需求调整集群大小。
*  设置其他应用程序资源，例如 IBM 服务、存储器、日志记录和监视。
*  在部署中使用变量。
*  控制对应用程序的访问权。

<br />


## 打包应用程序
{: #packaging}

如果要在多个集群中、公共和专用环境中，或者甚至在多个云提供者中运行应用程序，那么您可能会想知道如何使部署策略在所有这些环境中都有效。借助 {{site.data.keyword.Bluemix_notm}} 和其他开放式源代码工具，您可以打包应用程序以帮助自动执行部署。
{: shortdesc}

<dl>
<dt>自动执行基础架构</dt>
  <dd>可以使用开放式源代码 [Terraform](/docs/terraform?topic=terraform-getting-started#getting-started) 工具来自动供应 {{site.data.keyword.Bluemix_notm}} 基础架构，包括 Kubernetes 集群。请遵循本教程来[规划、创建和更新部署环境](/docs/tutorials?topic=solution-tutorials-plan-create-update-deployments#plan-create-update-deployments)。创建集群后，还可以设置 [{{site.data.keyword.containerlong_notm}} 集群自动缩放器](/docs/containers?topic=containers-ca)，以便工作程序池可根据工作负载的资源请求来扩展和缩减工作程序节点。</dd>
<dt>设置持续集成和交付 (CI/CD) 管道</dt>
  <dd>通过在源代码控制管理系统（如 Git）中组织的应用程序配置文件，可以构建管道来测试代码，并将代码部署到不同的环境，例如 `test` 和 `prod`。请查看[有关持续部署到 Kubernetes 的本教程](/docs/tutorials?topic=solution-tutorials-continuous-deployment-to-kubernetes#continuous-deployment-to-kubernetes)。</dd>
<dt>打包应用程序配置文件</dt>
  <dd>通过 [Helm ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://helm.sh/docs/) Kubernetes 软件包管理器，可以在 Helm chart 中指定应用程序需要的所有 Kubernetes 资源。然后，可以使用 Helm 来创建 YAML 配置文件，并在集群中部署这些文件。您还可以[集成 {{site.data.keyword.Bluemix_notm}} 提供的 Helm chart ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/kubernetes/solutions/helm-charts)，以扩展集群的功能，例如使用 Block Storage 插件。<p class="tip">您只是在寻找创建 YAML 文件模板的简单方法？有些人是使用 Helm 来创建 YAML 文件模板的，或者您也可以尝试使用其他社区工具，例如 [`ytt` ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://get-ytt.io/)。</p></dd>
</dl>

<br />


## 使应用程序保持最新
{: #updating}

您投入了大量工作来准备使用应用程序的下一个版本。可以使用 {{site.data.keyword.Bluemix_notm}} 和 Kubernetes 更新工具来确保应用程序在安全集群环境中运行，以及对应用程序的不同版本进行应用。
{: shortdesc}

### 如何使集群保持受支持状态？
{: #updating_kube}

确保集群始终运行[支持的 Kubernetes 版本](/docs/containers?topic=containers-cs_versions#cs_versions)。发布新的 Kubernetes 次版本时，较旧的版本很快就会不推荐使用，然后变为不受支持。有关更多信息，请参阅[更新 Kubernetes 主节点](/docs/containers?topic=containers-update#master)和[工作程序节点](/docs/containers?topic=containers-update#worker_node)。

### 可以使用哪些应用程序更新策略？
{: #updating_apps}

要更新应用程序，可以从多种策略中进行选择，例如以下策略。在执行更复杂的金丝雀部署之前，您可能要首先执行滚动部署或即时切换。

<dl>
<dt>滚动部署</dt>
  <dd>可以使用 Kubernetes 本机功能来创建 `v2` 部署，并逐渐替换先前的 `v1` 部署。此方法需要应用程序具有向后兼容性，以便提供 `v2` 应用程序版本的用户不会经历任何重大更改。有关更多信息，请参阅[管理滚动部署来更新应用程序](/docs/containers?topic=containers-app#app_rolling)。</dd>
<dt>即时切换</dt>
  <dd>即时切换也称为蓝绿部署，这种方法需要将计算资源加倍，以同时运行两个版本的应用程序。通过此方法，可以近乎实时地将用户切换到更高版本。确保使用服务标签选择器（例如，`version: green` 和 `version: blue`）来确保向正确的应用程序版本发送请求。可以创建新的 `version: green` 部署，等待它准备就绪，然后删除 `version: blue` 部署。或者，可以执行[滚动更新](/docs/containers?topic=containers-app#app_rolling)，但将 `maxUnavailable` 参数设置为 `0%`，将 `maxSurge` 参数设置为 `100%`。</dd>
<dt>金丝雀或 A/B 部署</dt>
  <dd>金丝雀部署是更复杂的更新策略，使用此方法时，要求您选取用户百分比（例如，5%），然后将这一比例的用户发送到新的应用程序版本。可以在日志记录和监视工具中收集有关新应用程序版本执行情况的度量值，执行 A/B 测试，然后将更新应用到更多用户。与所有部署一样，标注应用程序（例如，`version: stable` 和 `version: canary`）至关重要。要管理金丝雀部署，可以[安装受管 Istio 附加组件服务网](/docs/containers?topic=containers-istio#istio)，[为集群设置 Sysdig 监视](/docs/services/Monitoring-with-Sysdig/tutorials?topic=Sysdig-kubernetes_cluster#kubernetes_cluster)，然后如[此博客帖子 ![ 外部链接图标](../icons/launch-glyph.svg "外部链接图标 ")](https://sysdig.com/blog/monitor-istio/) 中所述，使用 Istio 服务网执行 A/B 测试。或者，将 Knative 用于金丝雀部署。</dd>
</dl>

<br />


## 监视集群性能
{: #monitoring_health}

通过对集群和应用程序进行有效的日志记录和监视，可以更好地了解环境，以优化资源利用率，并对可能出现的问题进行故障诊断。要为集群设置日志记录和监视解决方案，请参阅[日志记录和监视](/docs/containers?topic=containers-health#health)。
{: shortdesc}

设置日志记录和监视时，请考虑以下注意事项。

<dl>
<dt>收集日志和度量值以确定集群运行状况</dt>
  <dd>Kubernetes 包含度量值服务器，可帮助确定基本集群级别的性能。可以在 [Kubernetes 仪表板](/docs/containers?topic=containers-app#cli_dashboard)中查看这些度量值，也可以在终端中通过运行 `kubectl top (pods | nodes)` 命令来进行查看。您可以在自动化中包含这些命令。<br><br>
  将日志转发到日志分析工具，以便稍后可以分析日志。定义要记录的日志的详细程度和级别，以避免存储的日志多于需要的日志。日志会迅速占用大量存储空间，这可能会影响应用程序的性能，并使日志分析更加困难。</dd>
<dt>测试应用程序性能</dt>
  <dd>设置日志记录和监视后，执行性能测试。在测试环境中，有意创建各种非理想场景，例如删除专区中的所有工作程序节点以重现专区故障。查看日志和度量值，以检查应用程序如何进行恢复。</dd>
<dt>为审计做好准备</dt>
  <dd>除了应用程序日志和集群度量值外，您还希望设置活动跟踪，以便拥有关于谁执行了哪些集群和 Kubernetes 操作的可审计记录。有关更多信息，请参阅 [{{site.data.keyword.cloudaccesstrailshort}}](/docs/containers?topic=containers-at_events#at_events)。</dd>
</dl>
