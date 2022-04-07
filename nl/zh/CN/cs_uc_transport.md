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


# {{site.data.keyword.cloud_notm}} 的运输用例
{: #cs_uc_transport}

这些用例着重说明了 {{site.data.keyword.containerlong_notm}} 上的工作负载可以如何利用工具链来快速更新应用程序以及在全球范围进行多区域部署。同时，这些工作负载可以连接到现有后端系统，使用 Watson AI 进行个性化，并使用 {{site.data.keyword.messagehub_full}} 来访问 IOT 数据。

{: shortdesc}

## 运输公司提高针对业务合作伙伴生态系统的全球范围系统可用性
{: #uc_shipping}

IT 主管拥有与合作伙伴进行交互的全球运输路线和调度系统。合作伙伴需要访问这些 IoT 设备数据的系统中的最新信息。然而，这些系统无法以充分的 HA 在全球范围进行缩放。
{: shortdesc}

为什么选择 {{site.data.keyword.cloud_notm}}：{{site.data.keyword.containerlong_notm}} 可缩放可用性达 99.999% 的容器化应用程序，以满足不断增长的需求。开发者可以轻松进行试验，从而将更改快速推送到开发和测试系统时，应用程序部署每天可执行 40 次。通过 IoT Platform，可轻松访问 IoT 数据。

关键技术：    
* [针对业务合作伙伴生态系统的多个区域](/docs/containers?topic=containers-regions-and-zones#regions-and-zones)
* [水平缩放](/docs/containers?topic=containers-app#highly_available_apps)
* [{{site.data.keyword.contdelivery_full}} 中的开放式工具链](https://www.ibm.com/cloud/garage/toolchains/)
* [面向创新的云服务](https://www.ibm.com/cloud/products/#analytics)
* [{{site.data.keyword.messagehub_full}} 用于将事件数据提供给应用程序](/docs/services/EventStreams?topic=eventstreams-about#about)

**背景：运输公司提高针对业务合作伙伴生态系统的全球范围系统可用性**

* 由于运输物流的区域性差异，难以应对越来越多的合作伙伴分布在多个国家或地区中的情况。示例是独特的法规和运输物流，该公司在其中必须跨境维护一致的记录。
* 即时数据意味着全球范围的系统必须高度可用，以减少运输运营中的延迟问题。运输码头的时间表受到严格控制，在某些情况下没有灵活性。Web 使用量在增长，因此不稳定性可能会导致用户体验差。
* 开发者需要持续发展应用程序，但传统工具却降低了他们部署更新和功能的频率。  

**解决方案**

运输公司需要以统一的方式来管理发货时间表、库存和报关文书。随后，他们才能将装运位置、装运内容和交货时间表准确地与客户共享。他们不必再推测货物（例如，设备、服装或农产品）到达时间，而是能将准确的时间告知其运输客户，供后者向自己的客户传达这些信息。

解决方案由以下主要部分组成：
1. 流式传输 IoT 设备中每个装运集装箱的数据：载货单和位置
2. 以数字方式与相应的港口和运输合作伙伴共享报关文书，包括访问控制
3. 供聚集并传达装运货物到达信息的运输客户使用的应用程序，包括供运输客户在其自己的零售和企业到企业应用程序中复用装运数据的 API

对于与全球合作伙伴进行合作的运输公司，需要对路线和调度系统进行本地修改，以适应每个区域的语言、法规和独特的港口物流。{{site.data.keyword.containerlong_notm}} 为多个区域（包括北美、欧洲、亚洲和澳大利亚）提供了全局覆盖，以便应用程序可以反映每个国家或地区合作伙伴的需求。

IoT 设备流式传输的数据由 {{site.data.keyword.messagehub_full}} 分发到区域港口应用程序以及关联的海关和集装箱载货单数据存储。{{site.data.keyword.messagehub_full}} 是 IoT 事件的着陆点。它会根据 Watson IoT Platform 提供给 {{site.data.keyword.messagehub_full}} 的受管连接来分发事件。

事件位于 {{site.data.keyword.messagehub_full}} 中后，会持久存储以直接供港口运输应用程序使用，并且未来可随时使用。需要最短等待时间的应用程序直接实时使用事件流 ({{site.data.keyword.messagehub_full}}) 中的事件。其他未来应用程序（例如，分析工具）可以选择通过 {{site.data.keyword.cos_full}} 以批处理方式来使用事件存储中的事件。

由于装运数据会与公司的客户共享，因此开发者应确保客户可以使用 API 在其自己的应用程序中显示装运数据。例如，这些应用程序包括移动跟踪应用程序或 Web 电子商务解决方案。开发者还忙于构建和维护区域港口应用程序，以收集和分发海关记录和装运载货单。总之，他们需要专注于编码，而不是管理基础架构。因此他们选择了 {{site.data.keyword.containerlong_notm}}，因为 IBM 简化了基础架构管理：
* 管理 Kubernetes 主节点、IaaS 和操作组件，例如 Ingress 和存储器
* 监视工作程序节点的运行状况和恢复
* 提供全球计算，使开发者不必负责工作负载和数据需要位于的多个区域中的基础架构

为了实现全球可用性，在全球多个数据中心部署了开发、测试和生产系统。为了实现 HA，他们使用了不同地理区域多个集群的组合以及多专区集群。他们可以轻松在以下集群中部署港口应用程序以满足业务需求：
* 在法兰克福集群中，以符合当地的欧洲法规
* 在美国集群中，以确保本地可用性和故障恢复能力

他们还将工作负载分布在法兰克福的多专区集群中，以确保应用程序的欧洲版本可用，并同时对工作负载进行高效均衡。由于每个区域都会使用港口应用程序来上传独特的数据，因此应用程序的集群在等待时间较短的区域中进行托管。

对于开发者，大部分持续集成和交付 (CI/CD) 过程可通过 {{site.data.keyword.contdelivery_full}} 自动执行。公司可以定义工作流程工具链，以准备容器映像，检查是否有漏洞，然后将其部署到 Kubernetes 集群。

**解决方案模型**

随需应变计算、存储和事件管理在公共云中运行，有权根据需要对全球范围的装运数据进行访问

技术解决方案：
* {{site.data.keyword.containerlong_notm}}
* {{site.data.keyword.messagehub_full}}
* {{site.data.keyword.cos_full}}
* {{site.data.keyword.contdelivery_full}}

**步骤 1：使用微服务对应用程序容器化**

* 基于应用程序的功能区域及其依赖关系，将应用程序集成到 {{site.data.keyword.containerlong_notm}} 中的一组协作微服务。
* 将应用程序部署到 {{site.data.keyword.containerlong_notm}} 中的容器。
* 通过 Kubernetes 提供标准化的 DevOps 仪表板。
* 对不频繁运行的批处理和其他库存工作负载的计算启用按需缩放。
* 使用 {{site.data.keyword.messagehub_full}} 来管理来自 IoT 设备的流式数据。

**步骤 2：确保全球可用性**
* {{site.data.keyword.containerlong_notm}} 中的内置 HA 工具对每个地理区域内的工作负载进行均衡，包括自我复原和负载均衡。
* 负载均衡、防火墙和 DNS 由 IBM Cloud Internet Services 处理。
* 通过使用工具链和 Helm 部署工具，应用程序还可部署到全球范围的集群，使工作负载和数据满足区域需求。

**步骤 3：共享数据**
* {{site.data.keyword.cos_full}} 以及 {{site.data.keyword.messagehub_full}} 可提供实时数据存储和历史数据存储。
* API 支持装运公司的客户将数据共享到其应用程序中。

**步骤 4：持续交付**
* {{site.data.keyword.contdelivery_full}} 将可定制、可共享的模板与来自 IBM、第三方以及开放式源代码的工具配合使用，帮助开发者快速供应集成工具链。自动构建和测试，以通过分析控制质量。
* 开发者在其开发和测试集群中构建并测试应用程序后，可使用 IBM CI/CD 工具链将应用程序部署到全球范围的集群中。
* 通过 {{site.data.keyword.containerlong_notm}} 可轻松应用和回滚应用程序；部署了定制应用程序，以通过 Istio 的智能路由和负载均衡来满足区域需求。

**结果**

* 利用 {{site.data.keyword.containerlong_notm}} 和 IBM CI/CD 工具，应用程序的区域版本在从中收集数据的物理设备附近进行托管。
* 微服务大大缩短了补丁、错误修订和新功能的交付时间。初始开发速度很快，并且更新频繁。
* 运输客户可实时访问货物的位置、交货时间表，甚至是已批准的港口记录。
* 各个装运码头的运输伙伴都了解载货单和装运详细信息，因此改进了现场物流，减少了延迟。
* 与本案例不同的是，[马士基和 IBM 组建了一家合资企业](https://www.ibm.com/press/us/en/pressrelease/53602.wss)，以通过 Blockchain 来改进国际市场供应链。

## 航空公司在 3 周内交付创新的人力资源 (HR) 福利站点
{: #uc_airline}

人力资源主管 (CHRO) 需要采用创新的聊天机器人的新 HR 福利站点，但当前的开发工具和平台意味着应用程序上线的提前时间很长。这种情况包括须长时间等待硬件采购。
{: shortdesc}

为什么选择 {{site.data.keyword.cloud_notm}}：{{site.data.keyword.containerlong_notm}} 提供了易于启动的计算。开发者还可以轻松进行试验，利用开放式工具链，快速将更改推送到开发和测试系统。他们的传统软件开发工具添加到 IBM Watson Assistant 后性能得到提升。新的福利站点在 3 周内就创建完成。

关键技术：    
* [适应各种 CPU、内存和存储器需求的集群](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)
* [基于 Watson 技术的聊天机器人服务](https://developer.ibm.com/code/patterns/create-cognitive-banking-chatbot/)
* [DevOps 本机工具，包括 {{site.data.keyword.contdelivery_full}} 中的开放式工具链](https://www.ibm.com/cloud/garage/toolchains/)
* [SDK for Node.js](/docs/runtimes/nodejs?topic=Nodejs-nodejs_runtime#nodejs_runtime)

**背景：在 3 周内快速构建并部署创新的 HR 福利站点**
* 员工的增长和不断变化的 HR 政策意味着每年都需要注册一个全新的站点。
* 互动功能（例如，聊天机器人）有望帮助将新的 HR 政策传达给现有员工。
* 由于员工数量的增长，站点流量不断增加，但其基础架构的预算仍保持不变。
* HR 团队面临着快速移动的压力：快速推出新的站点功能，并频繁发布最新的福利变更内容。
* 注册时间段会持续两周，因此不允许新应用程序发生停机时间。

**解决方案**

该航空公司希望设计一种以人为本的开放型文化。人力资源主管充分意识到，专注于奖励和留住人才对于航空公司的盈利能力有很大影响。因此，每年推出的福利是培养以员工为中心的文化的关键方面。

他们需要能在以下方面帮助开发者及其用户的解决方案：
* 现有福利的前端：保险、教育产品、社会福利等
* 特定于区域的功能：每个国家或地区都有其独特的 HR 政策，因此总体站点可能看起来类似，但实际显示特定于区域的福利
* 开发者友好工具：加快功能和错误修订的应用
* 聊天机器人：提供有关福利的可信对话，并高效地解决用户请求和问题。

技术解决方案：
* {{site.data.keyword.containerlong_notm}}
* IBM Watson Assistant 和 Tone Analyzer
* {{site.data.keyword.contdelivery_full}}
* IBM Logging and Monitoring
* {{site.data.keyword.SecureGatewayfull}}
* {{site.data.keyword.appid_full_notm}}

加快开发速度是 HR 主管的制胜关键所在。首先，团队对其应用程序容器化，然后将其放入云中。通过使用现代容器，开发者可以使用 Node.js SDK 轻松进行试验，将更改推送到开发和测试系统，这些系统可在不同集群上横向扩展。这些推送通过开放式工具链和 {{site.data.keyword.contdelivery_full}} 自动执行。对 HR 站点的更新不用再受困于速度慢、易出错的构建过程。他们可以每天或更频繁地向其站点交付增量更新。此外，还可以快速集成 HR 站点的日志记录和监视，尤其是对于站点从后端福利系统中拉取个性化数据的方式。开发者不必浪费时间构建复杂的日志记录系统，只需要有能力对实时系统进行故障诊断即可。开发者无需成为云安全领域的专家，他们可以使用 {{site.data.keyword.appid_full_notm}} 轻松强制实施策略驱动的认证。

通过 {{site.data.keyword.containerlong_notm}}，开发者从专用数据中心内构建过度的硬件转向可定制计算，从而减少了 IT 运营、维护和能源成本。为了托管 HR 站点，他们能够轻松设计 Kubernetes 集群，以适应自己的 CPU、RAM 和存储器需求。降低人员成本的另一个因素是由 IBM 来管理 Kubernetes，这样开发者就可以集中精力交付更好的员工福利注册体验。

{{site.data.keyword.containerlong_notm}} 提供了可缩放的计算资源和关联的 DevOps 仪表板，可根据需要创建、缩放和拆除应用程序和服务。通过使用业界标准容器技术，应用程序可以在多个开发、测试和生产环境中进行快速开发和共享。此设置即时提供了可扩展性的优点。通过使用 Kubernetes 的一组丰富的部署和运行时对象，HR 团队可以可靠地监视和管理应用程序升级。他们还可以使用定义的规则和自动化 Kubernetes 编排器来复制和缩放应用程序。

**步骤 1：容器、微服务和 Garage Method**
* 将应用程序构建成一组在 {{site.data.keyword.containerlong_notm}} 中运行的协作微服务。该体系结构表示应用程序具有最多质量问题的功能性区域。
* 使用 IBM 漏洞顾问程序将应用程序部署到 {{site.data.keyword.containerlong_notm}} 中的容器。
* 通过 Kubernetes 提供标准化的 DevOps 仪表板。
* 采用 IBM Garage Method 中的基本敏捷和迭代开发实践，在不停机的情况下频繁发布新的功能、补丁和修订。

**步骤 2：连接到现有福利后端**
* {{site.data.keyword.SecureGatewayfull}} 用于为托管福利系统的内部部署系统创建安全隧道。  
* 通过组合使用内部部署数据和 {{site.data.keyword.containerlong_notm}}，可在遵循法规的情况下访问敏感数据。
* 聊天机器人对话会反馈到 HR 政策，使福利站点能够反映出最受欢迎和最不受欢迎的福利，包括有针对性地改进绩效欠佳的计划。

**步骤 3：聊天机器人和个性化**
* IBM Watson Assistant 提供了一些工具，可用于快速构建聊天机器人，用于向用户提供正确的福利信息。
* Watson Tone Analyzer 确保客户对聊天机器人的对话感到满意，并在必要时采取人为干预。

**步骤 4：在全球范围内持续交付**
* {{site.data.keyword.contdelivery_full}} 将可定制、可共享的模板与来自 IBM、第三方以及开放式源代码的工具配合使用，帮助开发者快速供应集成工具链。自动构建和测试，以通过分析控制质量。
* 开发者在其开发和测试集群中构建并测试应用程序后，可使用 IBM CI/CD 工具链将应用程序部署到全球范围的生产集群中。
* {{site.data.keyword.containerlong_notm}} 可轻松应用和回滚应用程序。部署了定制应用程序，以通过 Istio 的智能路由和负载均衡来满足区域需求。
* {{site.data.keyword.containerlong_notm}} 中的内置 HA 工具对每个地理区域内的工作负载进行均衡，包括自我复原和负载均衡。

**结果**
* 借助聊天机器人等工具，HR 团队向员工证明了创新是企业文化的一部分，而不仅仅是时髦用语。
* 通过在站点中真正实现个性化，满足了如今航空公司劳动力的不断变化的期望。
* 对 HR 站点的最新更新（包括由员工聊天机器人对话驱动的最新更新）会快速上线，因为开发者每天至少推送 10 次更改。
* 通过由 IBM 来照管基础架构管理，开发团队得以腾出时间在 3 周内完成站点交付。
