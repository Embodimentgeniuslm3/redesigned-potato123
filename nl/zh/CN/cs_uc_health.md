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


# {{site.data.keyword.cloud_notm}} 的医疗保健用例
{: #cs_uc_health}

这些用例着重说明了 {{site.data.keyword.containerlong_notm}} 上的工作负载如何受益于公共云。它们在隔离的裸机上采用安全计算，可轻松启动集群以实现更快开发，从虚拟机进行迁移，以及共享云数据库中的数据。
{: shortdesc}

## 医疗保健提供者将工作负载从效率低下的 VM 迁移到操作友好型容器，以用于报告和患者系统
{: #uc_migrate}

医疗保健提供者的 IT 主管拥有内部部署的业务报告和患者系统。这些系统的功能增强周期长，因而导致患者服务水平停滞不前。
{: shortdesc}

为什么选择 {{site.data.keyword.cloud_notm}}：为了改善患者服务，提供者希望通过 {{site.data.keyword.containerlong_notm}} 和 {{site.data.keyword.contdelivery_full}} 来减少 IT 支出并加速开发，所有这些工作全部在一个安全平台上完成。提供者的 SaaS 系统用于支持患者记录系统和业务报告应用程序，使用频率高，需要频繁更新。然而，内部部署环境阻碍了敏捷开发工作。提供者还希望能应对不断上升的员工成本，并降低预算。

关键技术：
* [适应各种 CPU、内存和存储器需求的集群](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)
* [水平缩放](/docs/containers?topic=containers-app#highly_available_apps)
* [容器安全性和隔离](/docs/containers?topic=containers-security#security)
* [DevOps 本机工具，包括 {{site.data.keyword.contdelivery_full}} 中的开放式工具链](https://www.ibm.com/cloud/garage/toolchains/)
* [SDK for Node.js](/docs/runtimes/nodejs?topic=Nodejs-nodejs_runtime#nodejs_runtime)
* [使用 {{site.data.keyword.appid_short_notm}} 而不更改应用程序代码进行登录的功能](/docs/services/appid?topic=appid-getting-started)

首先，他们对自己的 SaaS 系统容器化，然后将其放入云中。从第一步开始，他们从专用数据中心内构建过度的硬件转向可定制计算，从而减少了 IT 运营、维护和能源成本。为了托管 SaaS 站点，他们轻松设计了 Kubernetes 集群，以适应自己的 CPU、RAM 和存储器需求。降低员工成本的另一个因素是由 IBM 来管理 Kubernetes，这样提供者就可以集中精力交付更好的客户服务。

加快开发速度是 IT 主管的制胜关键所在。通过移至公共云，开发者可以使用 Node.js SDK 轻松进行试验，将更改推送到开发和测试系统，使系统在不同集群上横向扩展。这些推送通过开放式工具链和 {{site.data.keyword.contdelivery_full}} 自动执行。对 SaaS 系统的更新不用再受困于速度慢、易出错的构建过程。开发者可以每天甚至更频繁地向其用户交付增量更新。此外，SaaS 系统的日志记录和监视功能（尤其是患者前端和后端报告交互方式）可快速集成到系统中。开发者不必浪费时间构建复杂的日志记录系统，只需要有能力对实时系统进行故障诊断即可。

安全第一：通过 {{site.data.keyword.containerlong_notm}} 的裸机，敏感患者工作负载现在可采用熟悉的隔离方法，但仍不脱离公共云的灵活性范围内。裸机提供了可信计算，可以验证底层硬件是否受到篡改。通过该核心，漏洞顾问程序会提供以下扫描：
* 映像漏洞扫描
* 基于 ISO 27000 的政策扫描
* 实时容器扫描
* 针对已知恶意软件的软件包扫描

患者数据安全会使患者体验更愉快。

**背景：医疗保健提供者迁移工作负载**

* 技术债务和发布周期长这两个因素，阻碍了提供者的业务关键型患者管理和报告系统改进。
* 提供者的后台和前台定制应用程序以单体虚拟机映像形式在内部部署中交付。
* 他们需要对其过程、方法和工具进行彻底大修，但却不知道该从何着手。
* 由于无力发布高质量软件来满足市场需求，公司的技术债务非但没有缩减，反而不断增长。
* 安全性成为首要关注的问题，但是此问题加重了交付负担，甚至导致了更多延迟问题。
* 资本开支预算受到严格控制，但 IT 人员认为他们没有预算也没有人手，无法利用其内部系统来创建所需的测试和编译打包环境。

**解决方案模型**

随需应变计算、存储和 I/O 服务在公共云中运行，可安全地对内部部署企业资产进行访问。实施 CI/CD 过程以及 IBM Garage Method 的其他部件可大幅缩短交付周期。

**步骤 1：保护计算平台**
* 用于管理高敏感性患者数据的应用程序可以在支持可信计算的裸机上运行的 {{site.data.keyword.containerlong_notm}} 上重新托管。
* 可信计算可以验证底层硬件是否受到篡改。
* 通过该核心，漏洞顾问程序可提供针对已知恶意软件的映像、策略、容器、打包扫描和漏洞扫描。
* 通过简单的 Ingress 注释，以一致的方式对服务和 API 强制实施策略驱动的认证。通过使用 {{site.data.keyword.appid_short_notm}} 和声明式安全性，可以确保用户认证和令牌验证。

**步骤 2：提升并转换**
* 将虚拟机映像迁移到在公共云的 {{site.data.keyword.containerlong_notm}} 中运行的容器映像。
* 通过 Kubernetes 提供标准化的 DevOps 仪表板和实践。
* 对不频繁运行的批处理和其他后台工作负载的计算启用按需缩放。
* 使用 {{site.data.keyword.SecureGatewayfull}} 来维护与内部部署 DBMS 的安全连接。
* 专用数据中心/内部部署资本成本大幅减少，取而代之的是基于工作负载需求进行缩放的实用工具计算模型。

**步骤 3：微服务和 Garage Method**
* 将应用程序重新构建成一组协作微服务。该组协作微服务基于应用程序具有最多质量问题的功能性区域在 {{site.data.keyword.containerlong_notm}} 中运行。
* 将 {{site.data.keyword.cloudant}} 与客户提供的密钥配合使用，以在云中高速缓存数据。
* 采用持续集成和持续交付 (CI/CD) 实践，使开发者能够根据需要按自己的安排来对微服务进行版本控制和发布。{{site.data.keyword.contdelivery_full}} 提供了用于 CI/CD 过程的工作流程工具链，以及映像创建和容器映像漏洞扫描功能。
* 采用 IBM Garage Method 中敏捷的迭代开发实践，以支持在不停机的情况下频繁发布新功能、补丁和修订。

**技术解决方案**
* {{site.data.keyword.containerlong_notm}}
* {{site.data.keyword.cloudant}}
* {{site.data.keyword.SecureGatewayfull}}
* {{site.data.keyword.appid_short_notm}}

对于最敏感的工作负载，可在 {{site.data.keyword.containerlong_notm}} for Bare Metal 中托管集群。它提供了一个可信计算平台，可自动扫描硬件和运行时代码以查找漏洞。通过使用业界标准容器技术，应用程序最初可在 {{site.data.keyword.containerlong_notm}} 上迅速重新托管，而无需重大的体系结构更改。此更改即时提供了可扩展性的优点。

他们可以使用定义的规则和自动化 Kubernetes 编排器来复制和缩放应用程序。{{site.data.keyword.containerlong_notm}} 提供了可缩放的计算资源和关联的 DevOps 仪表板，可根据需要创建、缩放和拆除应用程序和服务。通过使用 Kubernetes 的部署和运行时对象，提供者可以可靠地监视和管理应用程序升级。

{{site.data.keyword.SecureGatewayfull}} 用于针对要重新托管以在 {{site.data.keyword.containerlong_notm}} 中运行的应用程序，创建通往内部部署数据库和文档的安全管道。

{{site.data.keyword.cloudant}} 是一种现代 NoSQL 数据库，适用于一系列数据驱动的用例，从键/值到复杂的面向文档的数据存储和查询。为了最大限度地减少对后台 RDBMS 的查询，{{site.data.keyword.cloudant}} 用于对应用程序中用户的会话数据进行高速缓存。这些选项改进了 {{site.data.keyword.containerlong_notm}} 上各应用程序中的前端应用程序易用性和性能。

仅仅将计算工作负载移至 {{site.data.keyword.cloud_notm}} 是不够的。提供者还需要实现过程和方法转型。通过采用 IBM Garage Method 的实践，提供者可以实施敏捷的迭代交付过程，支持 CI/CD 等现代 DevOps 实践。

CI/CD 过程本身的大部分内容通过云中的 IBM Continuous Delivery 服务自动执行。提供者可以定义工作流程工具链，以准备容器映像，检查是否有漏洞，然后将其部署到 Kubernetes 集群。

**结果**
* 通过将现有单体 VM 提升到云托管的容器，迈出了支持提供者节省资本成本和开始学习现代 DevOps 实践的第一步。
* 通过将单体应用程序重新构建成一组细颗粒度的微服务，大大缩短了交付补丁、错误修订和新功能的时间。
* 同时，提供者实施了简单的限定时间迭代，以弄明白现有技术债务。

## 随着越来越多地与合作伙伴一起进行研究，非盈利研究机构需要安全地托管敏感数据
{: #uc_research}

非盈利性疾病研究机构的开发主管手下有学术和行业研究人员，但他们无法轻松分享研究数据。他们的工作因区域合规性和集中式数据库的影响，而在全球范围内各自为政。
{: shortdesc}

为什么选择 {{site.data.keyword.cloud_notm}}：{{site.data.keyword.containerlong_notm}} 可交付安全计算，用于在开放式平台上托管敏感数据和高性能数据处理。该全球平台在就近区域中进行托管。因此，它与当地法规密切相关，可帮助树立患者和研究人员的信心，让他们相信自己的数据在本地受到保护，并且对改进医疗成效能起到重要作用。

关键技术：
* [智能安排按需放置工作负载](/docs/containers?topic=containers-regions-and-zones#regions-and-zones)
* [{{site.data.keyword.cloudant}} 可以跨应用程序持久存储和同步数据](/docs/services/Cloudant?topic=cloudant-getting-started#getting-started)
* [工作负载漏洞扫描和隔离](/docs/services/Registry?topic=va-va_index#va_index)
* [DevOps 本机工具，包括 {{site.data.keyword.contdelivery_full}} 中的开放式工具链](https://www.ibm.com/cloud/garage/toolchains/)
* [{{site.data.keyword.openwhisk}}，用于清理数据并通知研究人员关于数据结构更改](/docs/openwhisk?topic=cloud-functions-openwhisk_cloudant#openwhisk_cloudant)

**背景：为非盈利性研究机构安全托管和共享疾病数据**

* 来自各种机构的不同研究者群体没有统一的方法来共享数据，因而减缓了协作速度。
* 安全问题加重了协作负担，甚至导致共享研究减少。
* 开发者和研究人员遍布全球，分别属于不同的组织，这使得 PaaS 和 SaaS 成为每个用户组的最佳选择。
* 健康法规中的区域差异要求某些数据和数据处理工作始终保留在该区域内。

**解决方案**

该非盈利性研究机构希望聚集全球的癌症研究数据。为此，他们创建了一个部门，专门处理针对其研究人员的解决方案：
* 数据获取 - 此应用程序用于获取研究数据。目前研究人员是使用电子表格、文档、商业产品以及专有或内部数据库来记录研究成果。这种情况不太可能随着该非盈利性机构尝试集中化数据分析而改变。
* 匿名化 - 此应用程序用于对数据匿名化。必须除去 SPI 才符合区域健康法规的要求。
* 分析 - 此应用程序用于分析数据。基本模式是以常规格式存储数据，然后使用 AI 和机器学习 (ML) 技术、简单回归等来查询和处理数据。

研究人员需要与区域集群互动，而应用程序用于获取和转换数据并对数据匿名化：
1. 在区域集群之间同步匿名化数据，或将其发送到集中式数据存储
2. 通过在提供 GPU 的裸机工作程序节点上使用 ML（如 PyTorch）来处理数据

**数据获取** - 在存储研究人员的丰富数据文档的每个区域集群中使用 {{site.data.keyword.cloudant}}，并且可以根据需要对其进行查询和处理。{{site.data.keyword.cloudant}} 可加密静态和动态数据，以遵守区域数据隐私法。

{{site.data.keyword.openwhisk}} 用于创建处理函数，以获取研究数据并将其作为结构化数据文档存储在 {{site.data.keyword.cloudant}} 中。通过 {{site.data.keyword.SecureGatewayfull}}，{{site.data.keyword.openwhisk}} 能以安全方式轻松访问内部部署数据。

区域集群中的 Web 应用程序是在 nodeJS 中开发的，用于手动输入结果、模式定义和研究组织从属关系的数据。IBM Key Protect 用于帮助保护对 {{site.data.keyword.cloudant}} 数据的访问，IBM 漏洞顾问程序用于扫描应用程序容器和映像以查找安全漏洞。

**匿名化** - 任何时候在 {{site.data.keyword.cloudant}} 中存储新数据文档，都会触发一个事件，并且 Cloud Function 会对数据匿名化并从数据文档中除去 SPI。这些匿名化数据文档与所获取的“原始”数据分开存储，并且这些文档是在各区域之间共享以供分析的唯一文档。

**分析** - 机器学习框架是高度计算密集型框架，因此非盈利性机构设置了裸机工作程序节点的全局处理集群。与此全局处理集群相关联的是存储匿名化数据的聚集的 {{site.data.keyword.cloudant}} 数据库。定时作业会定期触发 Cloud Function，以将匿名化数据文档从区域中心推送到全局处理集群的 {{site.data.keyword.cloudant}} 实例。

计算集群运行的是 PyTorch ML 框架，而机器学习应用程序是使用 Python 编写的，用于分析聚集的数据。除了 ML 应用程序外，集体中的研究人员还可开发自己的应用程序，这些应用程序可在全局集群上发布并运行。

该非盈利机构还提供了在全局集群的非裸机节点上运行的应用程序。这些应用程序可查看并抽取聚集的数据和 ML 应用程序输出。这些应用程序可通过公共端点进行访问，公共端点由面向全球的 API 网关进行保护。随后，各地的研究人员和数据分析人员可以下载数据集并执行自己的分析。

**在 {{site.data.keyword.containerlong_notm}} 上托管研究工作负载**

开发者首先通过 {{site.data.keyword.containerlong_notm}} 在容器中部署自己的研究共享 SaaS 应用程序。接着，他们为开发环境创建了集群，以支持全球范围的开发者快速以协作方式部署应用程序改进。

安全第一：开发主管选择了裸机的可信计算来托管研究集群。通过 {{site.data.keyword.containerlong_notm}} 的裸机，敏感研究工作负载现在可采用熟悉的隔离方法，但仍不脱离公共云的灵活性范围内。裸机提供了可信计算，可以验证底层硬件是否受到篡改。由于此非盈利机构还与制药公司建立了合作伙伴关系，因此应用程序安全性至关重要。市场竞争十分激烈，还可能有企业间谍活动。通过该安全核心，漏洞顾问程序会提供以下扫描：
* 映像漏洞扫描
* 基于 ISO 27000 的政策扫描
* 实时容器扫描
* 针对已知恶意软件的软件包扫描

通过受保护的研究应用程序，提高了临床试验的参与度。

为了实现全球可用性，在全球多个数据中心部署了开发、测试和生产系统。为了实现 HA，他们使用了多个地理区域的多个集群的组合以及多专区集群。他们可以轻松地将研究应用程序部署到法兰克福集群，以遵守当地的欧洲法规。他们还在美国集群中部署了应用程序，以确保本地可用性和恢复能力。他们还将研究工作负载分布在法兰克福的各个多专区集群中，以确保欧洲应用程序可用，并同时对工作负载进行高效均衡。由于研究人员将使用研究共享应用程序来上传敏感数据，因此应用程序的集群在法规限制更严格的区域中进行托管。

开发者使用现有工具专注于解决领域问题：开发者不用再编写独特的 ML 代码，他们可改为通过将 {{site.data.keyword.cloud_notm}} 服务绑定到集群，从而使 ML 逻辑融入到应用程序中。此外，由于 IBM 负责照管 Kubernetes 和基础架构的升级、安全性等，因此开发者得以从基础架构管理任务中解放出来。

**解决方案**

随需应变计算、存储和 Node 入门模板工具包在公共云中运行，可根据需要安全地对全球范围内的研究数据进行访问。集群中的计算是防篡改的，并与裸机相隔离。

技术解决方案：
* 具有可信计算的 {{site.data.keyword.containerlong_notm}}
* {{site.data.keyword.openwhisk}}
* {{site.data.keyword.cloudant}}
* {{site.data.keyword.SecureGatewayfull}}

**步骤 1：使用微服务对应用程序容器化**
* 使用 IBM 的 Node.js 入门模板工具包开始进行开发。
* 基于应用程序的功能性区域及其依赖关系，将应用程序构造成 {{site.data.keyword.containerlong_notm}} 中的一组协作微服务。
* 将研究应用程序部署到 {{site.data.keyword.containerlong_notm}} 中的容器。
* 通过 Kubernetes 提供标准化的 DevOps 仪表板。
* 对不频繁运行的批处理和其他研究工作负载的计算启用按需缩放。
* 使用 {{site.data.keyword.SecureGatewayfull}} 来维护与现有内部部署数据库的安全连接。

**步骤 2：使用安全和高性能计算**
* 需要更高性能计算的 ML 应用程序在裸机上的 {{site.data.keyword.containerlong_notm}} 中进行托管。此 ML 集群是集中式集群，因此每个区域集群都没有裸机工作程序的费用；而且 Kubernetes 部署也更轻松。
* 用于处理高敏感性临床数据的应用程序可以在支持可信计算的裸机上的 {{site.data.keyword.containerlong_notm}} 中进行托管。
* 可信计算可以验证底层硬件是否受到篡改。通过该核心，漏洞顾问程序可提供针对已知恶意软件的映像、策略、容器、打包扫描和漏洞扫描。

**步骤 3：确保全球可用性**
* 开发者在其开发和测试集群中构建并测试应用程序后，可使用 IBM CI/CD 工具链将应用程序部署到全球范围的集群中。
* {{site.data.keyword.containerlong_notm}} 中的内置 HA 工具对每个地理区域内的工作负载进行均衡，包括自我复原和负载均衡。
* 通过工具链和 Helm 部署工具，应用程序还可部署到全球范围的集群，使工作负载和数据满足区域法规要求。

**步骤 4：数据共享**
* {{site.data.keyword.cloudant}} 是一种现代 NoSQL 数据库，适用于一系列数据驱动的用例，从键/值到复杂的面向文档的数据存储和查询。
* 为了最大限度地减少对区域数据库的查询，{{site.data.keyword.cloudant}} 用于对应用程序中用户的会话数据进行高速缓存。
* 此选择改进了 {{site.data.keyword.containerlong_notm}} 上各应用程序中的前端应用程序易用性和性能。
* {{site.data.keyword.containerlong_notm}} 中的工作程序应用程序分析内部部署数据，并将结果存储在 {{site.data.keyword.cloudant}} 中时，{{site.data.keyword.openwhisk}} 会对更改做出反应，并自动对传入数据订阅源上的数据进行清理。
* 与此类似，可以通过数据上传来触发一个区域中的研究突破通知，以便所有研究人员都可以利用新数据。

**结果**
* 通过入门模板工具包、{{site.data.keyword.containerlong_notm}} 和 IBM CI/CD 工具，全球开发者可使用熟悉的可互操作工具跨机构工作，并以协作方式开发研究应用程序。
* 微服务大大缩短了补丁、错误修订和新功能的交付时间。初始开发速度很快，并且更新频繁。
* 研究人员在遵守当地法规的情况下，有权访问临床数据并可共享临床数据。
* 参与疾病研究的患者相信，自己的数据与大型研究团队共享时是安全的，能起到重要作用。
