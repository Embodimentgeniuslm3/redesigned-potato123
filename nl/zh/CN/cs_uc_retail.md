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


# {{site.data.keyword.cloud_notm}} 的零售用例
{: #cs_uc_retail}

这些用例着重说明了 {{site.data.keyword.containerlong_notm}} 上的工作负载可以如何利用分析来获取市场洞察、全球范围的多区域部署以及使用 {{site.data.keyword.messagehub_full}} 和对象存储器进行的库存管理。
{: shortdesc}

## 实体零售商使用 API 与全球业务合作伙伴共享数据，以推动全渠道销售
{: #uc_data-share}

业务线 (LOB) 主管需要增加销售渠道，但零售系统却限制在内部部署数据中心。竞争对手依靠全球业务合作伙伴在实体店和网店交叉销售和追加销售各类商品。
{: shortdesc}

为什么选择 {{site.data.keyword.cloud_notm}}：{{site.data.keyword.containerlong_notm}} 提供了公共云生态系统，其中容器支持新的业务合作伙伴和其他外部参与者通过 API 共同开发应用程序和数据。既然零售系统位于公共云上，因此 API 还可简化数据共享以及快速启动新的应用程序开发。在开发者轻松试验时，应用程序部署会增加，从而使用工具链快速推动开发和测试系统的更改。

{{site.data.keyword.containerlong_notm}} 和关键技术：
* [适应各种 CPU、内存和存储器需求的集群](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)
* [用于跨应用程序持久存储和同步数据的 {{site.data.keyword.cos_full}}](/docs/tutorials?topic=solution-tutorials-pub-sub-object-storage#pub-sub-object-storage)
* [DevOps 本机工具，包括 {{site.data.keyword.contdelivery_full}} 中的开放式工具链](https://www.ibm.com/cloud/garage/toolchains/)

**背景：零售商使用 API 与全球业务合作伙伴共享数据，以推动全渠道销售**

* 零售商面临着巨大的竞争压力。首先，他们需要克服涉足新产品和新渠道的复杂性。例如，他们需要提高产品的复杂性。同时，其客户需要能够更轻松地选择不同品牌。
* 这种选择不同品牌的能力意味着零售生态系统需要连接到业务合作伙伴。随后，云可以通过业务合作伙伴、客户和其他外部参与者创造新的价值。
* 突发性用户事件（如黑色星期五）会使现有在线系统承受极大压力，迫使零售商过量供应计算基础架构。
* 零售商的开发者需要持续发展应用程序，但传统工具却降低了他们部署更新和功能的频率，在与业务合作伙伴团队进行协作时尤其如此。  

**解决方案**

需要一种更智能的购物体验来提高客户保留率和毛利率。零售商传统销售模式之所以不理想，是因为缺乏用于交叉销售和追加销售的业务合作伙伴库存。零售商的购物者寻求的是更高便利性，以便他们可以快速同时找到相关商品，例如瑜伽裤和瑜伽垫。

零售商还必须为客户提供有用的内容，例如产品信息、替代产品信息、评论以及实时库存可视性。而且这些客户希望通过个人移动设备和配备移动设备的店铺助理在线上和门店获得这些信息。

解决方案由以下主要部分组成：
* 库存：针对业务合作伙伴生态系统的应用程序，用于聚集和显示库存，尤其是新产品介绍，包括供业务合作伙伴在其自己的零售和 B2B 应用程序中复用的 API
* 交叉销售和追加销售：利用可以在各种电子商务和移动应用程序中使用的 API 来显示交叉销售和追加销售商机的应用程序
* 开发环境：用于开发、测试和生产系统的 Kubernetes 集群，可增进零售商及其业务合作伙伴之间的协作和数据共享

要使零售商能够与全球业务合作伙伴协作，库存 API 需要更改以适应每个区域的语言和市场偏好。{{site.data.keyword.containerlong_notm}} 覆盖多个区域（包括北美、欧洲、亚洲和澳大利亚），以便 API 反映每个国家或地区的需求并确保 API 调用的等待时间较短。

另一项要求是，库存数据必须可与公司的业务合作伙伴和客户共享。通过库存 API，开发者可以在应用程序（例如，移动库存应用程序或 Web 电子商务解决方案）中显示信息。开发者还忙于构建和维护主电子商务站点。总之，他们需要专注于编码，而不是管理基础架构。

因此他们选择了 {{site.data.keyword.containerlong_notm}}，因为 IBM 简化了基础架构管理：
* 管理 Kubernetes 主节点、IaaS 和操作组件，例如 Ingress 和存储器
* 监视工作程序节点的运行状况和恢复
* 提供全球计算，以便开发者在需要工作负载和数据位于的区域中拥有硬件基础架构

此外，对 API 微服务进行日志记录和监视，尤其是这些服务如何从后端系统中拉出个性化数据，以及如何轻松地与 {{site.data.keyword.containerlong_notm}} 集成。开发者不必浪费时间构建复杂的日志记录系统，只需要有能力对实时系统进行故障诊断即可。

{{site.data.keyword.messagehub_full}} 充当即时事件平台，将业务合作伙伴库存系统中快速变化的信息传递给 {{site.data.keyword.cos_full}}。

**解决方案模型**

随需应变计算、存储和事件管理在公共云中运行，有权根据需要对全球范围的零售库存进行访问

技术解决方案：
* {{site.data.keyword.containerlong_notm}}
* {{site.data.keyword.messagehub_full}}
* {{site.data.keyword.cos_full}}
* {{site.data.keyword.contdelivery_full}}
* IBM Cloud Logging and Monitoring
* {{site.data.keyword.appid_short_notm}}

**步骤 1：使用微服务对应用程序容器化**
* 基于应用程序的功能性区域及其依赖关系，将应用程序构造成一组在 {{site.data.keyword.containerlong_notm}} 中运行的协作微服务。
* 将应用程序部署到在 {{site.data.keyword.containerlong_notm}} 中运行的容器映像。
* 通过 Kubernetes 提供标准化的 DevOps 仪表板。
* 对不频繁运行的批处理和其他库存工作负载启用按需缩放。

**步骤 2：确保全球可用性**
* {{site.data.keyword.containerlong_notm}} 中的内置 HA 工具对每个地理区域内的工作负载进行均衡，包括自我复原和负载均衡。
* 负载均衡、防火墙和 DNS 由 IBM Cloud Internet Services 处理。
* 通过使用工具链和 Helm 部署工具，应用程序还会部署到全球范围的集群，使工作负载和数据满足区域需求，尤其是个性化需求。

**步骤 3：了解用户**
* {{site.data.keyword.appid_short_notm}} 提供了登录功能，而无需更改应用程序代码。
* 用户登录后，可以使用 {{site.data.keyword.appid_short_notm}} 来创建概要文件并个性化用户的应用程序体验。

**步骤 4：共享数据**
* {{site.data.keyword.cos_full}} 与 {{site.data.keyword.messagehub_full}} 一起提供了实时和历史数据存储，以便交叉销售产品代表了来自业务合作伙伴的可用库存。
* 通过 API，零售商的业务合作伙伴可以将数据共享到其电子商务和 B2B 应用程序中。

**步骤 5：持续交付**
* 在 IBM Cloud Logging and Monitoring 工具（基于云的工具，可供各种开发者访问）上添加联合开发的 API 后，调试这些 API 会变得更加简单。
* {{site.data.keyword.contdelivery_full}} 将可定制、可共享的模板与来自 IBM、第三方以及开放式源代码的工具配合使用，帮助开发者快速供应集成工具链。自动构建和测试，以通过分析控制质量。
* 开发者在其开发和测试集群中构建并测试应用程序后，使用 IBM 持续集成和交付（CI 和 CD）工具链将应用程序部署到全球范围的集群中。
* 通过 {{site.data.keyword.containerlong_notm}} 可轻松应用和回滚应用程序；定制应用程序可通过 Istio 的智能路由和负载均衡进行部署，以测试竞销活动。

**结果**
* 微服务大大缩短了补丁、错误修订和新功能的交付时间。初始全球范围开发的速度很快，更新频率高达 40 次/周。
* 通过使用 API，零售商及其业务合作伙伴可以直接访问库存可用性和交货安排。
* 通过 {{site.data.keyword.containerlong_notm}} 以及 IBM CI 和 CD 工具，A-B 版本的应用程序准备就绪，可以测试竞销活动。
* {{site.data.keyword.containerlong_notm}} 提供了可扩展的计算，使得库存和交叉销售 API 工作负载可以在一年中的销售高峰期（例如，秋季假期）内增长。

## 传统的杂货商通过数字洞察来提高客流量和销售量
{: #uc_grocer}

首席市场营销官 (CMO) 需要通过使门店成为差异化资产，使门店的客流量提高 20%。大型零售竞争对手和在线零售商都在争夺销售量。同时，CMO 需要在不降价的情况下减少库存，因为持有库存太久会导致数百万美元的资本无法流动。
{: shortdesc}

为什么选择 {{site.data.keyword.cloud_notm}}：通过 {{site.data.keyword.containerlong_notm}}，可以轻松启动更多计算能力，开发者可快速添加 Cloud Analytics 服务，以获取销售行为洞察并适应数字市场。

关键技术：    
* [水平缩放以加速开发](/docs/containers?topic=containers-app#highly_available_apps)
* [适应各种 CPU、内存和存储器需求的集群](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)
* [利用 Watson Discovery 来洞察市场趋势](https://www.ibm.com/watson/services/discovery/)
* [DevOps 本机工具，包括 {{site.data.keyword.contdelivery_full}} 中的开放式工具链](https://www.ibm.com/cloud/garage/toolchains/)
* [使用 {{site.data.keyword.messagehub_full}} 进行库存管理](/docs/services/EventStreams?topic=eventstreams-about#about)

**背景：传统的杂货商通过数字洞察来提高客流量和销售量**

* 来自在线零售商和大型零售店的竞争压力打乱了传统的杂货零售模式。销售量在不断下降，这通过实体店中稀少的客流量就可见一斑。
* 零售商的忠诚度计划需要通过在结帐时使用印刷优惠券的现代版来激励客户。因此，开发者必须持续发展相关应用程序，但传统工具却降低了他们部署更新和功能的频率。  
* 某些高价值库存并未按预期那样顺利流动，而“美食家”运动在主要大都市的市场中似乎呈现增长趋势。

**解决方案**

杂货商需要应用程序来增加转化率和门店流量，从而在可复用的云分析平台中生成新的销售额并构建客户忠诚度。店内有针对性的体验可以是某个服务或产品供应商举办的一项活动，借此基于与特定活动的亲缘关系来吸引忠诚客户和新客户。随后，门店和业务合作伙伴提供了一些激励计划，以吸引客户参加活动以及购买门店或业务合作伙伴的产品。  

在活动后，将指导客户购买必要的产品，这样他们就可以在未来自行重复演示的活动。有针对性的客户体验可通过激励性换购和新的忠诚度客户注册来度量。一个超个性化的市场营销活动和一个用于跟踪店内购买情况的工具相组合，可以将有针对性的体验一路转化为产品购买。所有这些措施都会带来更高的流量和转换率。

例如，举办的活动是安排一位当地厨师在门店展示如何制作一道美食。门店采用了吸引观众的激励计划。例如，他们在厨师的餐厅提供免费开胃菜，并对购买展示的菜肴所用食材的客户给予额外奖励（例如，购物车满 150 美元减 20 美元）。

解决方案由以下主要部分组成：
1. 库存分析：定制店内活动（食谱、食材清单和产品位置），以推广销售流动缓慢的库存。
2. 忠诚度移动应用程序：通过数字优惠券、购物清单、门店地图上的产品库存（价格和可用性）和社交共享，提供有针对性的市场营销。
3. 社交媒体分析：通过检测客户对各种趋势（烹饪法、厨师和食材）的偏好来提供个性化服务。分析通过个人的推特、Pinterest 和 Instagram 活动来连接区域趋势。
4. 开发者友好工具：加快功能和错误修订的应用。

用于产品库存、门店补货和产品预测的后端库存系统具有丰富的信息，但通过现代分析功能，可以揭示有关如何更好地使高端产品流动的新洞察。通过使用 {{site.data.keyword.cloudant}} 和 IBM Streaming Analytics 的组合，CMO 可以发现与定制店内活动相匹配的成功要素完美组合。

{{site.data.keyword.messagehub_full}} 充当即时事件平台，将库存系统中快速变化的信息传递给 IBM Streaming Analytics。

通过 Watson Discovery 进行的社交媒体分析（个性和语气洞察）也会将趋势传递到库存分析中，以改善产品预测。

忠诚度移动应用程序提供了详细的个性化信息，尤其是客户使用其社交共享功能（例如，发布食谱）时。

除了移动应用程序外，开发者还忙于构建和维护与传统结帐优惠券相关的现有忠诚度应用程序。总之，他们需要专注于编码，而不是管理基础架构。因此他们选择了 {{site.data.keyword.containerlong_notm}}，因为 IBM 简化了基础架构管理：
* 管理 Kubernetes 主节点、IaaS 和操作组件，例如 Ingress 和存储器
* 监视工作程序节点的运行状况和恢复
* 提供全球计算，因此开发者不必负责在数据中心设置基础架构

**解决方案模型**

随需应变计算、存储和事件管理在公共云中运行，有权访问后端 ERP 系统

技术解决方案：
* {{site.data.keyword.containerlong_notm}}
* {{site.data.keyword.messagehub_full}}
* {{site.data.keyword.cloudant}}
* IBM Streaming Analytics
* IBM Watson Discovery

**步骤 1：使用微服务对应用程序容器化**

* 将库存分析和移动应用程序构造成微服务，然后将其部署到 {{site.data.keyword.containerlong_notm}} 中的容器。
* 通过 Kubernetes 提供标准化的 DevOps 仪表板。
* 对于不频繁运行的批处理和其他库存工作负载，按需缩放计算。

**步骤 2：分析库存和趋势**
* {{site.data.keyword.messagehub_full}} 充当即时事件平台，将库存系统中快速变化的信息传递给 IBM Streaming Analytics。
* 将使用 Watson Discovery 进行的社交媒体分析和库存系统数据与 IBM Streaming Analytics 相集成，以提供推销和市场营销建议。

**步骤 3：通过移动忠诚度应用程序交付促销活动**
* 利用 IBM Mobile 入门模板工具包和其他 IBM Mobile 服务（例如 {{site.data.keyword.appid_full_notm}}）来快速启动移动应用程序的开发。
* 以优惠券和其他权利形式进行的促销活动将发送到用户的移动应用程序。促销活动是使用库存和社交分析以及其他后端系统来识别的。
* 将移动应用程序上存储的促销食谱和转化率（已兑换的结帐优惠券）反馈到 ERP 系统中，以供进一步分析。

**结果**
* 通过 {{site.data.keyword.containerlong_notm}}，微服务大大缩短了补丁、错误修订和新功能的交付时间。初始开发速度很快，并且更新频繁。
* 通过使门店本身成为差异化资产，提高了门店的客流量和销售量。
* 与此同时，通过社交和认知分析获得的新洞察降低了库存 OpEx（运营支出）。
* 移动应用程序中的社交共享也有助于识别新客户并向其进行市场营销。
