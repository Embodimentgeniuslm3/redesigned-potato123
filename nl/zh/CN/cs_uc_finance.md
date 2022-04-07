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


# {{site.data.keyword.cloud_notm}} 的金融服务用例
{: #cs_uc_finance}

这些用例着重说明了 {{site.data.keyword.containerlong_notm}} 上的工作负载可以如何利用高可用性和高性能计算，如何轻松启动集群来加快开发速度，以及如何利用 {{site.data.keyword.ibmwatson}} 中的 AI。
{: shortdesc}

## 抵押贷款公司削减成本并加快实现合规性
{: #uc_mortgage}

一家住宅抵押贷款公司的风险管理副总裁每天要处理 7000 万条记录，但内部部署系统的速度较慢，而且也不准确。IT 费用迅速攀升，原因是硬件很快过时，而且未得到充分利用。在等待硬件供应期间，公司的合规性脚步会因此而放慢。
{: shortdesc}

为什么选择 {{site.data.keyword.Bluemix_notm}}：为了改进风险分析，该公司期望 {{site.data.keyword.containerlong_notm}} 和 IBM Cloud Analytic 服务可帮助降低成本，提高全球范围的可用性，并最终加快实现合规性。通过多个区域中的 {{site.data.keyword.containerlong_notm}}，可以对该公司的分析应用程序容器化并将其部署到全球范围，从而提高可用性并满足当地法规要求。这些部署通过人们熟悉的开放式源代码工具进行加速，这些工具已经是 {{site.data.keyword.containerlong_notm}} 的组成部分。

{{site.data.keyword.containerlong_notm}} 和关键技术：
* [水平缩放](/docs/containers?topic=containers-app#highly_available_apps)
* [利用多个区域实现高可用性](/docs/containers?topic=containers-regions-and-zones#regions-and-zones)
* [适应各种 CPU、内存和存储器需求的集群](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)
* [容器安全性和隔离](/docs/containers?topic=containers-security#security)
* [{{site.data.keyword.cloudant}} 可以跨应用程序持久存储和同步数据](/docs/services/Cloudant?topic=cloudant-getting-started#getting-started)
* [SDK for Node.js](/docs/runtimes/nodejs?topic=Nodejs-nodejs_runtime#nodejs_runtime)

**解决方案**

首先，该公司对分析应用程序容器化，然后将其放入云中。他们头疼的硬件问题瞬间就解决了。他们能够轻松设计 Kubernetes 集群，以适应自己的高性能 CPU、RAM、存储器和安全性需求。当分析应用程序发生变化时，他们不用进行大量硬件投资就能添加或收缩计算。利用 {{site.data.keyword.containerlong_notm}} 水平缩放，他们的应用程序可随着记录数的增长而缩放，从而能更快生成法规报告。{{site.data.keyword.containerlong_notm}} 提供了全球范围的弹性计算资源，这些资源安全、性能高，可充分利用现代计算资源。

现在，这些应用程序可从 {{site.data.keyword.cloudant}} 上的数据仓库接收大量数据。{{site.data.keyword.cloudant}} 中基于云的存储器可确保数据可用性高于原先禁锢在内部部署系统中的数据的可用性。由于可用性至关重要，因此应用程序会跨全球数据中心进行部署：解决 DR 和等待时间问题。

此外，该公司还加快了风险分析速度和合规性脚步。现在，他们通过迭代式敏捷部署，持续更新预测和风险分析功能，如蒙特卡洛计算。容器编排由受管 Kubernetes 进行处理，因此运营成本也随之下降。最终，抵押贷款风险分析可更快响应市场上的快速变化。

**背景：住宅抵押贷款的合规性和财务建模**

* 对改善财务风险管理的迫切需求，促使监管力度加大。这些需求还促使在风险评估过程中进行关联的复查，并披露更为详细、综合与丰富的监管报告。
* 高性能计算网格是用于财务建模的关键基础架构组件。

公司目前的问题是交付规模和交付时间

该公司目前采用的是内部部署环境，已经使用了 7 年多，计算、存储和 I/O 容量都很有限。服务器刷新成本很高，而且需要很长时间才能完成。软件和应用程序更新遵循非正式流程，并且这些更新不可重复。实际 HPC 网格难以进行编程。API 对于新入职的开发者来说过于复杂，并且需要非书面知识。此外，完成重大应用程序升级需要 6 到 9 个月时间。

**解决方案模型：随需应变计算、存储和 I/O 服务在公共云中运行，可根据需要安全地访问内部部署企业资产**

* 安全且可扩展的文档存储，支持结构化和非结构化文档查询
* “提升并转换”现有企业资产和应用程序，同时支持集成到某些无法迁移的内部部署系统中
* 缩短了解决方案部署时间，并实施了标准 DevOps 和监视过程，以解决影响报告准确性的错误

**详细解决方案**

* {{site.data.keyword.containerlong_notm}}
* {{site.data.keyword.cos_full_notm}}
* {{site.data.keyword.sqlquery_notm}} (Spark)
* {{site.data.keyword.cloudant}}
* {{site.data.keyword.SecureGateway}}

{{site.data.keyword.containerlong_notm}} 提供了可缩放的计算资源和关联的 DevOps 仪表板，可根据需要创建、缩放和拆除应用程序和服务。通过使用业界标准容器，应用程序最初可在 {{site.data.keyword.containerlong_notm}} 上迅速重新托管，而无需重大的体系结构更改。

此解决方案即时提供了可扩展性的优点。通过使用 Kubernetes 的一组丰富的部署和运行时对象，该抵押贷款公司可以可靠地监视和管理应用程序升级。他们还能够复制和缩放应用程序，以使用定义的规则和自动化 Kubernetes 编排器。

{{site.data.keyword.SecureGateway}} 用于针对要重新托管以在 {{site.data.keyword.containerlong_notm}} 中运行的应用程序，创建通往内部部署数据库和文档的安全管道。

{{site.data.keyword.cos_full_notm}} 在其推进过程中适用于所有原始文档和数据存储器。为了进行蒙特卡洛模拟，实施了一条工作流程管道，其中模拟数据位于在 {{site.data.keyword.cos_full_notm}} 中存储的结构化文件中。启动模拟的触发器在 {{site.data.keyword.containerlong_notm}} 中扩展计算服务，用于将文件的数据拆分成 N 个事件存储区以进行模拟处理。{{site.data.keyword.containerlong_notm}} 会自动缩放到 N 个关联的服务执行，然后将中间结果写入 {{site.data.keyword.cos_full_notm}}。这些结果由另一组 {{site.data.keyword.containerlong_notm}} 计算服务进行处理，以生成最终结果。

{{site.data.keyword.cloudant}} 是一种现代 NoSQL 数据库，对于许多数据驱动的用例非常有用：从键/值到复杂的面向文档的数据存储和查询。为了管理不断增长的监管和管理报告规则集，抵押贷款公司使用 {{site.data.keyword.cloudant}} 来存储与传入公司的原始监管数据关联的文档。这将触发 {{site.data.keyword.containerlong_notm}} 上的计算过程来汇集、处理和发布各种报告格式的数据。通用于各个报告的中间结果会存储为 {{site.data.keyword.cloudant}} 文档，以便可以使用模板驱动的过程来生成必要的报告。

**结果**

* 完成复杂财务模拟所用时间仅为先前使用现有内部部署系统可能实现的时间的 25%。
* 部署时间从先前的 6 到 9 个月缩短为平均 1 到 3 周。实现此改进的原因是，{{site.data.keyword.containerlong_notm}} 支持通过规范的受控过程来提升应用程序容器，并将其替换为更新的版本。可以快速修正报告错误，从而解决问题（例如，准确性）。
* 通过 {{site.data.keyword.containerlong_notm}} 和 {{site.data.keyword.cloudant}} 提供的一组一致、可扩展的存储和计算服务，降低了监管报告成本。
* 一段时间后，会将最初“提升并转换”到云的原始应用程序重新构造成在 {{site.data.keyword.containerlong_notm}} 上运行的协作微服务。此操作进一步加快了开发，缩短了部署时间，支持通过相对简单的试验来实现更多创新。他们还使用更新版本的微服务发布了创新的应用程序，以利用市场和业务条件（即，所谓的情境应用程序和微服务）。

## 支付技术公司简化了开发者的工作，提高了开发者的工作效率，使得将支持 AI 的工具部署到其合作伙伴所需的时间缩短为原来的 1/4
{: #uc_payment_tech}

在等待硬件采购期间，开发主管手下有开发者使用的是内部部署传统工具，这些工具会使原型设计速度变慢。
{: shortdesc}

为什么选择 {{site.data.keyword.Bluemix_notm}}：{{site.data.keyword.containerlong_notm}} 提供了使用开放式源代码标准技术来加速计算的功能。在该公司移至 {{site.data.keyword.containerlong_notm}} 后，开发者有权访问 DevOps 友好工具，例如可移植且轻松共享的容器。

开发者还可以轻松进行试验，利用开放式工具链，快速将更改推送到开发和测试系统。在 AI 云服务上，通过一次单击将他们传统的软件开发工具添加到应用程序时，这些工具将采用全新外观。

关键技术：
* [适应各种 CPU、内存和存储器需求的集群](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)
* [通过 {{site.data.keyword.watson}} AI 预防欺诈](https://www.ibm.com/cloud/watson-studio)
* [DevOps 本机工具，包括 {{site.data.keyword.contdelivery_full}} 中的开放式工具链](https://www.ibm.com/cloud/garage/toolchains/)
* [SDK for Node.js](/docs/runtimes/nodejs?topic=Nodejs-nodejs_runtime#nodejs_runtime)
* [使用 {{site.data.keyword.appid_short_notm}} 而不更改应用程序代码进行登录的功能](/docs/services/appid?topic=appid-getting-started)

**背景：简化开发者工作，提高开发者工作效率，并使将 AI 工具部署到合作伙伴的时间缩短为原来的 1/4**

* 颠覆性在支付行业中随处可见，在消费者和企业到企业细分市场中也呈明显增长趋势。但是，支付工具的更新却很慢。
* 需要认知功能以全新方式更快地准确识别欺诈性交易。
* 随着合作伙伴及其交易量的不断增加，工具的流量在增加，但其基础架构预算却需要减少，这可通过最大程度地提高资源效率来实现。
* 由于无力发布高质量软件来满足市场需求，公司的技术债务非但没有缩减，反而不断增长。
* 资本开支预算受到严格控制，但 IT 人员认为他们没有预算也没有人手，无法利用其内部系统来创建测试和编译打包环境。
* 安全性日益成为首要关注的问题，但是对安全性的关注只是加重了交付负担，甚至导致了更多延迟问题。

**解决方案**

开发主管面临着动态支付行业中的诸多难题。法规、消费者行为、欺诈、竞争对手和市场基础设施都在快速变化。要在未来支付领域占有一席之地，快节奏的开发至关重要。

该公司的业务模型是为业务合作伙伴提供支付工具，以便可以帮助这些金融机构和其他组织提供高度安全的数字支付体验。

他们需要能在以下方面帮助开发者及其业务合作伙伴的解决方案：
* 支付工具前端：收费系统、付款跟踪（包括跨境）、合规性、生物统计学、汇款等
* 特定于法规的功能：每个国家或地区都有其独特的法规，因此整体工具集可能看起来类似，但实际显示特定于国家或地区的优点
* 开发者友好工具：加快功能和错误修订的应用
* 欺诈检测即服务 (FDaaS)：使用 {{site.data.keyword.watson}} AI 领先一步，以防范日益频繁、不断增长的欺诈行为

**解决方案模型**

在公共云中运行的随需应变计算、DevOps 工具和 AI 有权访问后端支付系统。实施 CI/CD 过程可大幅缩短交付周期。

技术解决方案：
* {{site.data.keyword.containerlong_notm}}
* {{site.data.keyword.contdelivery_full}}
* IBM Cloud Logging and Monitoring
* {{site.data.keyword.ibmwatson_notm}} for Financial Services
* {{site.data.keyword.appid_full_notm}}

首先，该公司对支付工具 VM 容器化，然后将其放入云中。他们头疼的硬件问题瞬间就解决了。他们能够轻松设计 Kubernetes 集群，以适应自己的 CPU、RAM、存储器和安全性需求。此外，当支付工具需要更改时，他们不用采购昂贵的硬件，也不必等待缓慢的硬件采购流程，直接就能添加或收缩计算。

利用 {{site.data.keyword.containerlong_notm}} 水平缩放，他们的应用程序可随着合作伙伴数的增长而缩放，从而能实现更快的成长。{{site.data.keyword.containerlong_notm}} 提供了全球范围的弹性计算资源，这些资源十分安全，可充分利用现代计算资源。

加快开发速度是主管的制胜关键所在。通过使用现代容器，开发者可以轻松使用自己所选的语言进行试验，将更改推送到开发和测试系统，以及在不同集群上横向扩展。这些推送通过开放式工具链和 {{site.data.keyword.contdelivery_full}} 自动执行。对工具的更新不用再受困于速度慢、易出错的构建过程。他们可以每天或更频繁地向其工具交付增量更新。

此外，工具的日志记录和监视功能（尤其是如果使用了 {{site.data.keyword.watson}} AI）可快速集成到系统中。开发者不必浪费时间构建复杂的日志记录系统，只需要有能力对实时系统进行故障诊断即可。降低员工成本的一个关键因素是由 IBM 来管理 Kubernetes，这样开发者就可以集中精力改进支付工具。

安全第一：通过 {{site.data.keyword.containerlong_notm}} 的裸机，敏感支付工具现在可采用熟悉的隔离方法，但仍不脱离公共云的灵活性范围内。裸机提供了可信计算，可以验证底层硬件是否受到篡改。扫描漏洞和恶意软件将持续运行。

**步骤 1：提升和转换为安全计算**
* 用于管理高敏感性数据的应用程序可以在裸机上运行的 {{site.data.keyword.containerlong_notm}} 上重新托管，以实现可信计算。可信计算可以验证底层硬件是否受到篡改。
* 将虚拟机映像迁移到在公共 {{site.data.keyword.Bluemix_notm}} 的 {{site.data.keyword.containerlong_notm}} 中运行的容器映像。
* 通过该核心，漏洞顾问程序可提供针对已知恶意软件的映像、策略、容器和打包漏洞扫描。
* 专用数据中心/内部部署资本成本大幅减少，取而代之的是基于工作负载需求进行缩放的实用工具计算模型。
* 通过简单的 Ingress 注释，以一致的方式对服务和 API 强制实施策略驱动的认证。通过使用 {{site.data.keyword.appid_short_notm}} 和声明式安全性，可以确保用户认证和令牌验证。

**步骤 2：操作现有支付系统后端以及与其连接**
* 使用 IBM {{site.data.keyword.SecureGateway}} 来维护与内部部署工具系统的安全连接。
* 通过 Kubernetes 提供标准化的 DevOps 仪表板和实践。
* 开发者在开发和测试集群中构建并测试应用程序后，可使用 {{site.data.keyword.contdelivery_full}} 工具链将应用程序部署到全球范围的 {{site.data.keyword.containerlong_notm}} 集群中。
* {{site.data.keyword.containerlong_notm}} 中的内置 HA 工具对每个地理区域内的工作负载进行均衡，包括自我复原和负载均衡。

**步骤 3：分析并预防欺诈**
* 部署 IBM {{site.data.keyword.watson}} for Financial Services 以预防和检测欺诈。
* 通过使用工具链和 Helm 部署工具，应用程序还可部署到全球范围的 {{site.data.keyword.containerlong_notm}} 集群。然后，工作负载和数据可满足区域法规的要求。

**结果**
* 通过将现有整体 VM 提升到云托管的容器，迈出了支持开发主管节省资本和运营成本的第一步。
* 通过由 IBM 来照管基础架构管理，开发团队得以腾出时间每天交付 10 次更新。
* 同时，提供者实施了简单的限定时间迭代，以弄明白现有技术债务。
* 根据处理的事务数，能以指数方式缩放运营。
* 同时，使用 {{site.data.keyword.watson}} 进行新的欺诈分析提高了检测和预防速度，使欺诈事件降低到区域平均水平的 1/4。
