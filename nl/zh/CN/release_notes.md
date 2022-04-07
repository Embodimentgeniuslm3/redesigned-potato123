---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

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

# 发行说明
{: #iks-release}

使用发行说明可了解对 {{site.data.keyword.containerlong}} 文档的最新更改，这些更改按月份分组。
{:shortdesc}

## 2019 年 6 月
{: #jun19}

<table summary="该表显示了发行说明。每行从左到右阅读，其中第一列是日期，第二列是功能标题，第三列是描述。">
<caption>2019 年 6 月的 {{site.data.keyword.containerlong_notm}} 文档更新</caption>
<thead>
<th>日期</th>
<th>描述</th>
</thead>
<tbody>
<tr>
  <td>2019 年 6 月 7 日</td>
  <td><ul>
  <li><strong>通过专用服务端点访问 Kubernetes 主节点</strong>：添加了[步骤](/docs/containers?topic=containers-clusters#access_on_prem)，用于通过专用负载均衡器公开专用服务端点。完成这些步骤后，授权集群用户可以通过 VPN 或 {{site.data.keyword.Bluemix_notm}} Direct Link 连接来访问 Kubernetes 主节点。</li>
  <li><strong>{{site.data.keyword.BluDirectLink}}</strong>：向 [VPN 连接](/docs/containers?topic=containers-vpn)和[混合云](/docs/containers?topic=containers-hybrid_iks_icp)页面添加了 {{site.data.keyword.Bluemix_notm}} Direct Link，用于在远程网络环境和 {{site.data.keyword.containerlong_notm}} 之间创建直接专用连接，而无需通过公用因特网进行路由。</li>
  <li><strong>Ingress ALB 更改日志</strong>：已将 [ALB `ingress-auth` 映像更新至构建 330](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog)。</li>
  <li><strong>OpenShift beta</strong>：[添加了一个课程](/docs/containers?topic=containers-openshift_tutorial#openshift_logdna_sysdig)，用于说明如何修改 {{site.data.keyword.la_full_notm}} 和 {{site.data.keyword.mon_full_notm}} 附加组件的特权安全上下文约束的应用程序部署。</li>
  </ul></td>
</tr>
<tr>
  <td>2019 年 6 月 6 日</td>
  <td><ul>
  <li><strong>Fluentd 更改日志</strong>：添加了 [Fluentd 版本更改日志](/docs/containers?topic=containers-cluster-add-ons-changelog#fluentd_changelog)。</li>
  <li><strong>Ingress ALB 更改日志</strong>：已将 [ALB `nginx-ingress` 映像更新至构建 470](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog)。</li>
  </ul></td>
</tr>
<tr>
  <td>2019 年 6 月 5 日</td>
  <td><ul>
  <li><strong>CLI 参考</strong>：更新了 [CLI 参考页面](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli)，以反映 {{site.data.keyword.containerlong_notm}} CLI 插件 [V0.3.34 发行版](/docs/containers?topic=containers-cs_cli_changelog)的多项更改。</li>
  <li><strong>新！Red Hat OpenShift on IBM Cloud 集群 (beta)</strong>：通过 Red Hat OpenShift on IBM Cloud beta，可以创建 {{site.data.keyword.containerlong_notm}} 集群，其中包含随 OpenShift 容器编排平台软件一起安装的工作程序节点。您将获得集群基础架构环境的受管 {{site.data.keyword.containerlong_notm}} 的所有优点，以及在 Red Hat Enterprise Linux 上运行以用于应用程序部署的 [OpenShift 工具和目录 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://docs.openshift.com/container-platform/3.11/welcome/index.html)。首先，请参阅[教程：创建 Red Hat OpenShift on IBM Cloud 集群 (beta)](/docs/containers?topic=containers-openshift_tutorial)。</li>
  </ul></td>
</tr>
<tr>
  <td>2019 年 6 月 4 日</td>
  <td><ul>
  <li><strong>版本更改日志</strong>：更新了 [1.14.2_1521](/docs/containers?topic=containers-changelog#1142_1521)、[1.13.6_1524](/docs/containers?topic=containers-changelog#1136_1524)、[1.12.9_1555](/docs/containers?topic=containers-changelog#1129_1555) 和 [1.11.10_1561](/docs/containers?topic=containers-changelog#11110_1561) 补丁发行版的更改日志。</li></ul>
  </td>
</tr>
<tr>
  <td>2019 年 6 月 3 日</td>
  <td><ul>
  <li><strong>自带 Ingress 控制器</strong>：更新了[步骤](/docs/containers?topic=containers-ingress#user_managed)，以反映对缺省社区控制器的更改，以及需要对多专区集群中的控制器 IP 地址进行运行状况检查。</li>
  <li><strong>{{site.data.keyword.cos_full_notm}}</strong>：更新了[步骤](/docs/containers?topic=containers-object_storage#install_cos)，以安装使用或不使用 Helm 服务器 Tiller 的 {{site.data.keyword.cos_full_notm}} 插件。</li>
  <li><strong>Ingress ALB 更改日志</strong>：已将 [ALB `nginx-ingress` 映像更新至构建 467](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog)。</li>
  <li><strong>Kustomize</strong>：添加了[通过 Kustomize 在多个环境中复用 Kubernetes 配置文件](/docs/containers?topic=containers-app#kustomize)的示例。</li>
  <li><strong>Razee</strong>：向支持的集成添加了 [Razee ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/razee-io/Razee)，以直观显示集群中的部署信息以及自动部署 Kubernetes 资源。</li></ul>
  </td>
</tr>
</tbody></table>

## 2019 年 5 月
{: #may19}

<table summary="该表显示了发行说明。每行从左到右阅读，其中第一列是日期，第二列是功能标题，第三列是描述。">
<caption>2019 年 5 月的 {{site.data.keyword.containerlong_notm}} 文档更新</caption>
<thead>
<th>日期</th>
<th>描述</th>
</thead>
<tbody>
<tr>
  <td>2019 年 5 月 30 日</td>
  <td><ul>
  <li><strong>CLI 参考</strong>：更新了 [CLI 参考页面](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli)，以反映 {{site.data.keyword.containerlong_notm}} CLI 插件 [V0.3.33 发行版](/docs/containers?topic=containers-cs_cli_changelog)的多项更改。</li>
  <li><strong>有关存储器的故障诊断</strong>：<ul>
  <li>添加了有关 [PVC 暂挂状态](/docs/containers?topic=containers-cs_troubleshoot_storage#file_pvc_pending)的文件存储器和块存储器故障诊断主题。</li>
  <li>添加了有关[应用程序无法访问或写入 PVC](/docs/containers?topic=containers-cs_troubleshoot_storage#block_app_failures) 时的块存储器故障诊断主题。</li>
  </ul></li>
  </ul></td>
</tr>
<tr>
  <td>2019 年 5 月 28 日</td>
  <td><ul>
  <li><strong>集群附加组件更改日志</strong>：已将 [ALB `nginx-ingress` 映像更新至构建 462](/docs/containers?topic=containers-cluster-add-ons-changelog)。</li>
  <li><strong>有关注册表的故障诊断</strong>：添加了[故障诊断主题](/docs/containers?topic=containers-cs_troubleshoot_clusters#ts_image_pull_create)，用于说明在集群创建期间集群由于发生错误而无法从 {{site.data.keyword.registryfull}} 拉取映像的情况。</li>
  <li><strong>有关存储器的故障诊断</strong>：<ul>
  <li>添加了有关[调试持久性存储器故障](/docs/containers?topic=containers-cs_troubleshoot_storage#debug_storage)的主题。</li>
  <li>添加了有关[由于缺少许可权，PVC 创建失败](/docs/containers?topic=containers-cs_troubleshoot_storage#missing_permissions)的故障诊断主题。</li>
  </ul></li>
  </ul></td>
</tr>
<tr>
  <td>2019 年 5 月 23 日</td>
  <td><ul>
  <li><strong>CLI 参考</strong>：更新了 [CLI 参考页面](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli)，以反映 {{site.data.keyword.containerlong_notm}} CLI 插件 [V0.3.28 发行版](/docs/containers?topic=containers-cs_cli_changelog)的多项更改。</li>
  <li><strong>集群附加组件更改日志</strong>：已将 [ALB `nginx-ingress` 映像更新至构建 457](/docs/containers?topic=containers-cluster-add-ons-changelog)。</li>
  <li><strong>集群和工作程序状态</strong>：更新了[日志记录和监视页面](/docs/containers?topic=containers-health#states)，以添加有关集群和工作程序节点状态的参考表。</li>
  <li><strong>集群规划和创建</strong>：现在，可以在以下页面中找到有关规划、创建和除去集群以及网络规划的信息：
    <ul><li>[规划集群网络设置](/docs/containers?topic=containers-plan_clusters)</li>
    <li>[规划集群以实现高可用性](/docs/containers?topic=containers-ha_clusters)</li>
    <li>[规划工作程序节点设置](/docs/containers?topic=containers-planning_worker_nodes)</li>
    <li>[创建集群](/docs/containers?topic=containers-clusters)</li>
    <li>[向集群添加工作程序节点和专区](/docs/containers?topic=containers-add_workers)</li>
    <li>[除去集群](/docs/containers?topic=containers-remove)</li>
    <li>[更改服务端点或 VLAN 连接](/docs/containers?topic=containers-cs_network_cluster)</li></ul>
  </li>
  <li><strong>集群版本更新</strong>：更新了[不支持的版本策略](/docs/containers?topic=containers-cs_versions)，以指出如果主节点版本比最旧的受支持版本低三个或更多版本，那么无法更新集群。可以通过在列出集群时查看**状态**字段来查看集群是否**不受支持**。</li>
  <li><strong>Istio</strong>：更新了 [Istio 页面](/docs/containers?topic=containers-istio)，以除去 Istio 在仅连接到专用 VLAN 的集群中不工作的限制。添加了[更新受管附加组件主题](/docs/containers?topic=containers-managed-addons#updating-managed-add-ons)的步骤，以在 Istio 管理的附加组件更新完成后，重新创建使用 TLS 部分的 Istio 网关。</li>
  <li><strong>热门主题</strong>：已将热门主题替换为此发行说明页面，此页面包含特定于 {{site.data.keyword.containershort_notm}} 的新功能和更新。有关 {{site.data.keyword.Bluemix_notm}} 产品的最新信息，请查看[公告](https://www.ibm.com/cloud/blog/announcements)。</li>
  </ul></td>
</tr>
<tr>
  <td>2019 年 5 月 20 日</td>
  <td><ul>
  <li><strong>版本更改日志</strong>：添加了[工作程序节点修订包更改日志](/docs/containers?topic=containers-changelog)。</li>
  </ul></td>
</tr>
<tr>
  <td>2019 年 5 月 16 日</td>
  <td><ul>
  <li><strong>CLI 参考</strong>：更新了 [CLI 参考页面](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli)，以添加用于 `logging-collect` 命令的 COS 端点，并且说明了 `apiserver-refresh` 用于重新启动 Kubernetes 主节点组件。</li>
  <li><strong>不受支持：Kubernetes V1.10</strong>：[Kubernetes V1.10](/docs/containers?topic=containers-cs_versions#cs_v114) 现在不受支持。</li>
  </ul></td>
</tr>
<tr>
  <td>2019 年 5 月 15 日</td>
  <td><ul>
  <li><strong>缺省 Kubernetes 版本</strong>：缺省 Kubernetes 版本现在为 1.13.6。</li>
  <li><strong>服务限制</strong>：添加了[服务限制主题](/docs/containers?topic=containers-ibm-cloud-kubernetes-service-technology#tech_limits)。</li>
  </ul></td>
</tr>
<tr>
  <td>2019 年 5 月 13 日</td>
  <td><ul>
  <li><strong>版本更改日志</strong>：添加了可用于 [1.14.1_1518](/docs/containers?topic=containers-changelog#1141_1518)、[1.13.6_1521](/docs/containers?topic=containers-changelog#1136_1521)、[1.12.8_1552](/docs/containers?topic=containers-changelog#1128_1552)、[1.11.10_1558](/docs/containers?topic=containers-changelog#11110_1558) 和 [1.10.13_1558](/docs/containers?topic=containers-changelog#11013_1558) 的新补丁更新。</li>
  <li><strong>工作程序节点类型模板</strong>：除去了每种[云状态 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/status?component=containers-kubernetes&selected=status) 48 个或更多核心的所有[虚拟机工作程序节点类型模板](/docs/containers?topic=containers-planning_worker_nodes#vm)。您仍可以供应使用 48 个或更多核心的[裸机工作程序节点](/docs/containers?topic=containers-plan_clusters#bm)。</li></ul></td>
</tr>
<tr>
  <td>2019 年 5 月 8 日</td>
  <td><ul>
  <li><strong>API</strong>：添加了[全局 API Swagger 文档 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://containers.cloud.ibm.com/global/swagger-global-api/#/) 的链接。</li>
  <li><strong>Cloud Object Storage</strong>：在 {{site.data.keyword.containerlong_notm}} 集群中[添加了有关 Cloud Object Storage 的故障诊断指南](/docs/containers?topic=containers-cs_troubleshoot_storage#cos_pvc_pending)。</li>
  <li><strong>Kubernetes 策略</strong>：添加了有关[在将应用程序移至 {{site.data.keyword.containerlong_notm}} 之前，最好具备哪些知识和技术技能？](/docs/containers?topic=containers-strategy#knowledge)的主题。</li>
  <li><strong>Kubernetes V1.14</strong>：添加了经过认证的 [Kubernetes 1.14 发行版](/docs/containers?topic=containers-cs_versions#cs_v114)。</li>
  <li><strong>参考主题</strong>：更新了[用户访问权](/docs/containers?topic=containers-access_reference)和 [CLI](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli) 参考页面中各种服务绑定、`logging` 和 `nlb` 操作的信息。</li></ul></td>
</tr>
<tr>
  <td>2019 年 5 月 7 日</td>
  <td><ul>
  <li><strong>集群 DNS 提供程序</strong>：[说明了 CoreDNS 的优点](/docs/containers?topic=containers-cluster_dns)，现在运行 Kubernetes 1.14 和更高版本的集群仅支持 CoreDNS。</li>
  <li><strong>边缘节点</strong>：添加了对[边缘节点](/docs/containers?topic=containers-edge)的专用负载均衡器支持。</li>
  <li><strong>免费集群</strong>：说明了支持[免费集群](/docs/containers?topic=containers-regions-and-zones#regions_free)的位置。</li>
  <li><strong>新！集成</strong>：添加并重构了有关 [{{site.data.keyword.Bluemix_notm}} 服务和第三方集成](/docs/containers?topic=containers-ibm-3rd-party-integrations)、[常用集成](/docs/containers?topic=containers-supported_integrations)和[合作伙伴关系](/docs/containers?topic=containers-service-partners)的信息。</li>
  <li><strong>新！Kubernetes V1.14</strong>：创建集群或将集群更新为 [Kubernetes 1.14](/docs/containers?topic=containers-cs_versions#cs_v114)。</li>
  <li><strong>不推荐使用 Kubernetes V1.11</strong>：在不支持 [Kubernetes 1.11](/docs/containers?topic=containers-cs_versions#cs_v111) 之前，[更新](/docs/containers?topic=containers-update)运行 Kubernetes 1.11 的所有集群。</li>
  <li><strong>许可权</strong>：添加了常见问题 - [我能为集群用户提供哪些访问策略？](/docs/containers?topic=containers-faqs#faq_access)</li>
  <li><strong>工作程序池</strong>：添加了有关如何[将标签应用于现有工作程序池](/docs/containers?topic=containers-add_workers#worker_pool_labels)的指示信息。</li>
  <li><strong>参考主题</strong>：为支持新功能，例如 Kubernetes 1.14，更新了[更改日志](/docs/containers?topic=containers-changelog#changelog)参考页面。</li></ul></td>
</tr>
<tr>
  <td>2019 年 5 月 1 日</td>
  <td><strong>分配基础架构访问权</strong>：修改了[分配开具支持案例的 IAM 许可权的步骤](/docs/containers?topic=containers-users#infra_access)。</td>
</tr>
</tbody></table>


