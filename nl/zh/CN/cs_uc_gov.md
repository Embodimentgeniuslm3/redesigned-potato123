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


# {{site.data.keyword.Bluemix_notm}} 的政府用例
{: #cs_uc_gov}

这些用例着重说明了 {{site.data.keyword.containerlong_notm}} 上的工作负载如何受益于公共云。这些工作负载通过可信计算进行隔离，位于全球多个区域中以获得数据主权，使用 Watson 机器学习替代了全新代码，并连接到内部部署数据库。
{: shortdesc}

## 区域政府通过组合使用公共/专用数据的社区开发者来改进协作并加快速度
{: #uc_data_mashup}

开放式政府数据项目主管需要与社区和私营部门共享公共数据，但这些数据被禁锢在内部部署单体系统中。
{: shortdesc}

为什么选择 {{site.data.keyword.Bluemix_notm}}：通过 {{site.data.keyword.containerlong_notm}}，主管可发挥组合公共/专用数据的转化价值。同样，该服务会提供公共云平台，以基于单体内部部署应用程序来重构并公开微服务。此外，公共云还支持政府和公共合作伙伴关系使用外部云服务及适合协作的开放式源代码工具。

关键技术：    
* [适应各种 CPU、内存和存储器需求的集群](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)
* [DevOps 本机工具，包括 {{site.data.keyword.contdelivery_full}} 中的开放式工具链](https://www.ibm.com/cloud/garage/toolchains/)
* [通过 {{site.data.keyword.cos_full_notm}} 提供对公共数据的访问](/docs/services/cloud-object-storage?topic=cloud-object-storage-about)
* [即插即用的 IBM Cloud Analytics 服务](https://www.ibm.com/cloud/analytics)

**背景：政府通过组合使用公共/专用数据的社区开发者来改进协作并加快速度**
* “开放式政府”是未来的政府模式，但此区域政府机构无法通过其内部部署系统实现这一飞跃。
* 他们希望支持创新并促进私营部门、公民和公共机构联合进行开发。
* 来自政府和私营组织的不同开发者群体没有一个统一的开放式源代码平台可用于轻松共享 API 和数据。
* 政府数据被禁锢在内部部署系统中，无法轻松进行公共访问。

**解决方案**

开放式政府转型必须在能提供性能、弹性、业务连续性和安全性的基础上进行构建。随着创新和联合开发的发展，各机构和公民都依赖于软件、服务和基础设施公司来“保护和提供服务”。

为了破除官僚主义，转变政府与选民之间的关系，他们采用开放式标准构建了一个可实现共同创造的平台：

* 开放数据 - 数据存储器，公民、政府机构和企业可自由访问、共享和改进其中的数据
* 开放式 API - 一个开发平台，在其中 API 由所有社区合作伙伴提供并复用
* 开放创新 - 一组云服务，支持开发者进行插件创新，而不必手动对其进行编码

首先，政府使用 {{site.data.keyword.cos_full_notm}} 将其公共数据存储在云中。此存储器可免费使用和复用，可由任何人共享，仅受归属和共享的约束。敏感数据在推送到云之前，可以先进行清理。此外，会设置访问控制，以便云可设置新数据存储的上限，其中社区可以演示对增强的现有免费数据的 POC。

政府针对公共/私营合作伙伴关系开展的下一步工作是建立一个在 {{site.data.keyword.apiconnect_long}} 中托管的 API 经济体。在其中，社区和企业开发者使数据能通过 API 形式轻松访问。他们的目标是拥有公共可用的 REST API，以实现互操作性，并加速应用程序集成。他们使用 IBM {{site.data.keyword.SecureGateway}} 连接回内部部署的专用数据源。

最后，基于这些共享 API 的应用程序在 {{site.data.keyword.containerlong_notm}} 中进行托管，在其中可轻松启动集群。随后，社区、私营部门和政府中的开发者可以轻松联合创建应用程序。总之，开发者需要专注于编码，而不是管理基础架构。因此他们选择了 {{site.data.keyword.containerlong_notm}}，因为 IBM 简化了基础架构管理：
* 管理 Kubernetes 主节点、IaaS 和操作组件，例如 Ingress 和存储器
* 监视工作程序节点的运行状况和恢复
* 提供全球计算，使开发者不必在工作负载和数据需要位于的全球各区域中支持基础架构

仅仅将计算工作负载移至 {{site.data.keyword.Bluemix_notm}} 是不够的。政府还需要实现过程和方法转型。通过采用 IBM Garage Method 的实践，提供者可以实施敏捷的迭代交付过程，以支持持续集成和持续交付 (CI/CD) 等现代 DevOps 实践。

大部分 CI/CD 过程本身可通过云中的 {{site.data.keyword.contdelivery_full}} 自动执行。提供者可以定义工作流程工具链，以准备容器映像，检查是否有漏洞，然后将其部署到 Kubernetes 集群。

**解决方案模型**

随需应变计算、存储和 API 工具在公共云中运行，可安全地对内部部署数据源执行进出访问。

技术解决方案：
* {{site.data.keyword.containerlong_notm}}
* {{site.data.keyword.cos_full_notm}} 和 {{site.data.keyword.cloudant}}
* {{site.data.keyword.apiconnect_long}}
* IBM {{site.data.keyword.SecureGateway}}
* {{site.data.keyword.contdelivery_full}}

**步骤 1：将数据存储在云中**
* {{site.data.keyword.cos_full_notm}} 提供历史数据存储器，可供公共云上的所有人访问。
* 将 {{site.data.keyword.cloudant}} 与开发者提供的密钥配合使用，以在云中高速缓存数据。
* 使用 IBM {{site.data.keyword.SecureGateway}} 来维护与现有内部部署数据库的安全连接。

**步骤 2：通过 API 提供对数据的访问**
* 将 {{site.data.keyword.apiconnect_long}} 用于 API 经济体平台。API 支持公共和私营部门将数据合并到其应用程序中。
* 为通过 API 驱动的公共/专用应用程序创建集群。
* 基于应用程序的功能性区域及其依赖关系，将应用程序构造成一组在 {{site.data.keyword.containerlong_notm}} 中运行的协作微服务。
* 将应用程序部署到在 {{site.data.keyword.containerlong_notm}} 中运行的容器。{{site.data.keyword.containerlong_notm}} 中的内置 HA 工具对工作负载进行均衡，包括自我复原和负载均衡。
* 通过所有类型开发者都熟悉的 Kubernetes 等开放式源代码工具，提供标准化的 DevOps 仪表板。

**步骤 3：通过 IBM Garage 和云服务实现创新**
* 采用 IBM Garage Method 中敏捷的迭代开发实践，以支持在不停机的情况下频繁发布功能、补丁和修订。
* 无论是公共部门还是私营部门的开发者，{{site.data.keyword.contdelivery_full}} 都可以帮助他们使用可定制、可共享的模板来快速供应集成工具链。
* 开发者在其开发和测试集群中构建并测试应用程序后，可使用 {{site.data.keyword.contdelivery_full}} 工具链将应用程序部署到生产集群中。
* 借助 Watson AI、机器学习以及 {{site.data.keyword.Bluemix_notm}}“目录”中提供的深入学习工具，开发者可以专注于解决领域问题。定制的独特 ML 代码被 ML 逻辑所取代；ML 逻辑通过服务绑定融入到应用程序中。

**结果**
* 现在，通常步调缓慢的公共/私营合作伙伴关系在数周内就可以启动应用程序，而不需要数个月的时间。这些开发合作伙伴关系现在每周可交付多达 10 次功能和错误修订。
* 所有参与者都使用熟知的开放式源代码工具（如 Kubernetes）时，开发速度将加快。长学习曲线不再是障碍。
* 公民和私营部门可透明地了解政府的活动、信息和规划。公民被整合到政府过程、服务和支持中。
* 借助公共/私营合作伙伴关系，可攻克极为艰巨的任务，如寨卡病毒跟踪、智能配电、犯罪统计信息分析以及大学“新领”人才教育。

## 大型公共港口确保港口数据和装运载货单交换的安全性，以此将公共和私营组织连接在一起
{: #uc_port}

私营运输公司和政府运营的港口的 IT 主管需要建立连接，提供可视性，并安全地交换港口信息。但是，并没有统一的系统可用于连接公共港口信息和私营装运载货单。
{: shortdesc}

为什么选择 {{site.data.keyword.Bluemix_notm}}：{{site.data.keyword.containerlong_notm}} 支持政府和公共合作伙伴关系使用外部云服务及适合协作的开放式源代码工具。容器提供了一个可共享平台，港口和运输公司对于共享信息在该安全平台上进行托管都很放心。此外，这些公司从小型开发/测试系统发展到生产规模的系统时，该平台也会相应进行缩放。开放式工具链通过自动执行构建、测试和部署，进一步加速了开发工作。

关键技术：    
* [适应各种 CPU、内存和存储器需求的集群](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)
* [容器安全性和隔离](/docs/containers?topic=containers-security#security)
* [DevOps 本机工具，包括 {{site.data.keyword.contdelivery_full}} 中的开放式工具链](https://www.ibm.com/cloud/garage/toolchains/)
* [SDK for Node.js](/docs/runtimes/nodejs?topic=Nodejs-nodejs_runtime#nodejs_runtime)

**背景：港口确保港口数据和装运载货单交换的安全性，以此将公共和私营组织连接在一起。**

* 来自政府和运输公司的不同开发者群体没有一个统一的平台可用于进行协作，因而降低了部署更新和功能的速度。
* 开发者遍布全球，分别属于不同的组织，这意味着开放式源代码和 PaaS 是最佳选择。
* 安全性是首要关注的问题，但是对安全性的关注加重了协作负担，因而影响到软件的功能和更新，在应用程序处于生产环境后这一问题尤为突出。

* 即时数据意味着全球范围的系统必须高度可用，以减少运输运营中的延迟问题。运输码头的时间表受到严格控制，在某些情况下没有灵活性。Web 使用量在增长，因此不稳定性可能会导致用户体验差。

**解决方案**

港口和运输公司联合开发了一个统一的交易系统，以电子方式一次性提交合规相关信息以对货物和船舶清关，而不用向多个机构提供这些信息。载货单和报关应用程序可以快速共享特定装运的内容，并确保所有文书都由港口的代理以电子方式传输并处理。

由此，他们建立了专注于贸易系统解决方案的合作伙伴关系：
* 报关 - 此应用程序用于接收装运载货单，并以数字方式处理典型的报关文书，并标记不符合政策的项目，以供调查和执法
* 关税 - 此应用程序用于计算关税，以电子方式向发货人提交费用，以及接收数字付款
* 法规 - 这是灵活且可配置的应用程序，用于为先前两个应用程序提供影响进口、出口和关税处理的不断变化的政策和法规

开发者首先通过 {{site.data.keyword.containerlong_notm}} 在容器中部署自己的应用程序。接着，他们为共享开发环境创建了集群，以支持全球范围的开发者快速以协作方式部署应用程序改进。容器支持每个开发团队使用自己选择的语言。

安全第一：IT 主管选择了裸机的可信计算来托管集群。通过 {{site.data.keyword.containerlong_notm}} 的裸机，敏感报关工作负载现在可采用熟悉的隔离方法，但仍不脱离公共云的灵活性范围内。裸机提供了可信计算，可以验证底层硬件是否受到篡改。

由于运输公司希望同时与其他港口合作，因此应用程序安全性至关重要。装运载货单和报关信息高度保密。通过该安全核心，漏洞顾问程序会提供以下扫描：
* 映像漏洞扫描
* 基于 ISO 27000 的政策扫描
* 实时容器扫描
* 针对已知恶意软件的软件包扫描

同时，{{site.data.keyword.iamlong}} 还有助于控制谁对资源具有哪种访问级别。

开发者使用现有工具专注于解决领域问题：开发者不用再编写独特日志记录和监视代码，他们可改为通过将 {{site.data.keyword.Bluemix_notm}} 服务绑定到集群，从而使其融入到应用程序中。此外，由于 IBM 负责照管 Kubernetes 和基础架构的升级、安全性等，因此开发者得以从基础架构管理任务中解放出来。

**解决方案模型**

随需应变计算、存储和 Node 入门模板工具包在公共云中运行，可根据需要安全地对全球范围内的装运数据进行访问。集群中的计算是防篡改的，并与裸机相隔离。  

技术解决方案：
* 具有可信计算的 {{site.data.keyword.containerlong_notm}}
* {{site.data.keyword.openwhisk}}
* {{site.data.keyword.cloudant}}
* IBM {{site.data.keyword.SecureGateway}}

**步骤 1：使用微服务对应用程序容器化**
* 使用 IBM 的 Node.js 入门模板工具包开始进行开发。
* 基于应用程序的功能性区域及其依赖关系，将应用程序构造成一组在 {{site.data.keyword.containerlong_notm}} 中运行的协作微服务。
* 将载货单和装运应用程序部署到在 {{site.data.keyword.containerlong_notm}} 中运行的容器。
* 通过 Kubernetes 提供标准化的 DevOps 仪表板。
* 使用 IBM {{site.data.keyword.SecureGateway}} 来维护与现有内部部署数据库的安全连接。

**步骤 2：确保全球可用性**
* 开发者在其开发和测试集群中部署应用程序后，可使用 {{site.data.keyword.contdelivery_full}} 工具链和 Helm 将特定于国家或地区的应用程序部署到全球范围的集群中。
* 然后，工作负载和数据可满足区域法规的要求。
* {{site.data.keyword.containerlong_notm}} 中的内置 HA 工具对每个地理区域内的工作负载进行均衡，包括自我复原和负载均衡。

**步骤 3：数据共享**
* {{site.data.keyword.cloudant}} 是一种现代 NoSQL 数据库，适用于一系列数据驱动的用例，从键/值到复杂的面向文档的数据存储和查询。
* 为了最大限度地减少对区域数据库的查询，{{site.data.keyword.cloudant}} 用于对应用程序中用户的会话数据进行高速缓存。
* 此配置改进了 {{site.data.keyword.containershort}} 上各应用程序中的前端应用程序易用性和性能。
* {{site.data.keyword.containerlong_notm}} 中的工作程序应用程序分析内部部署数据，并将结果存储在 {{site.data.keyword.cloudant}} 中时，{{site.data.keyword.openwhisk}} 会对更改做出反应，并自动对传入数据订阅源上的数据进行清理。
* 与此类似，可以通过数据上传来触发一个区域中的装运通知，以便所有下游使用者都可以访问新数据。

**结果**
* 通过 IBM 入门模板工具包、{{site.data.keyword.containerlong_notm}} 和 {{site.data.keyword.contdelivery_full}} 工具，跨组织和政府的全球开发者可开展合作。可使用熟悉的可互操作工具，以协作方式开发定制应用程序。
* 微服务大大缩短了补丁、错误修订和新功能的交付时间。初始开发速度很快，更新频率为 10 次/周。
* 运输客户和政府官员在遵守当地法规的情况下，有权访问载货单数据并可共享报关数据。
* 运输公司受益于供应链中改进的物流管理：成本更低，清关速度更快。
* 99% 为数字报关，90% 的进口在没有人为干预的情况下完成处理。
