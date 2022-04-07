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


# 使用 {{site.data.keyword.containerlong_notm}} 的责任
{: #responsibilities_iks}

了解使用 {{site.data.keyword.containerlong}} 时，您的集群管理责任以及相关条款和条件。
{:shortdesc}

## 集群管理责任
{: #responsibilities}

IBM 为您提供了一个企业云平台，供您部署应用程序以及 {{site.data.keyword.Bluemix_notm}} DevOps、AI、数据和安全服务。您可以选择如何设置、集成和运行云中的应用程序和服务。
{:shortdesc}

<table summary="此表显示 IBM 和您的责任。各行都应从左到右阅读，其中第一列是表示各责任的图标，第二列是描述。">
<caption>IBM 和您的责任</caption>
  <thead>
  <th colspan=2>按类型列出的责任</th>
  </thead>
  <tbody>
    <tr>
    <td align="center"><img src="images/icon_clouddownload.svg" alt="包含向下箭头的云的图标"/><br>云基础架构</td>
    <td>
    **IBM 的责任**：
    <ul><li>在 IBM 拥有的安全基础架构帐户中，为每个集群部署完全受管的高可用性专用主节点。</li>
    <li>在 IBM Cloud Infrastructure (SoftLayer) 帐户中供应工作程序节点。</li>
    <li>设置集群管理组件，例如 VLAN 和负载均衡器。</li>
    <li>满足增加基础架构的请求，例如添加和除去工作程序节点，创建缺省子网，以及根据持久卷声明来供应存储卷。</li>
    <li>集成订购的基础架构资源，使其自动使用集群体系结构，并可用于已部署的应用程序和工作负载。</li></ul>
    <br><br>
    **您的责任**：
    <ul><li>使用提供的 API、CLI 或控制台工具来调整[计算](/docs/containers?topic=containers-clusters#clusters)和[存储](/docs/containers?topic=containers-storage_planning#storage_planning)容量，并调整[联网配置](/docs/containers?topic=containers-cs_network_cluster#cs_network_cluster)以满足工作负载的需求。</li></ul><br><br>
    </td>
     </tr>
     <tr>
     <td align="center"><img src="images/icon_tools.svg" alt="扳手图标"/><br>受管集群</td>
     <td>
     **IBM 的责任**：
    <ul><li>提供一套用于自动执行集群管理的工具，例如 {{site.data.keyword.containerlong_notm}} [API ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://containers.cloud.ibm.com/global/swagger-global-api/)、[CLI 插件](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli)和[控制台 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/kubernetes/clusters)。</li>
     <li>自动应用 Kubernetes 主节点补丁操作系统、版本和安全更新。使主要更新和次要更新可供您应用。</li>
     <li>更新和恢复集群中运行的 {{site.data.keyword.containerlong_notm}} 和 Kubernetes 组件，例如 Ingress 应用程序负载均衡器和 File Storage 插件。</li>
     <li>备份和恢复 etcd 中的数据，例如 Kubernetes 工作负载配置文件。</li>
     <li>创建集群时，在主节点和工作程序节点之间设置 OpenVPN 连接。</li>
     <li>监视并报告各种接口中主节点和工作程序节点的运行状况。</li>
     <li>提供工作程序节点主要、次要以及补丁操作系统、版本和安全更新。</li>
     <li>执行自动化请求以更新和恢复工作程序节点。提供可选的[工作程序节点自动恢复](/docs/containers?topic=containers-health#autorecovery)。</li>
     <li>提供多种工具（例如，[集群自动缩放器](/docs/containers?topic=containers-ca#ca)）用于扩展集群基础架构。</li>
     </ul>
     <br><br>
     **您的责任**：
    <ul>
     <li>使用 API、CLI 或控制台工具来[应用](/docs/containers?topic=containers-update#update)提供的主要和次要 Kubernetes 主节点更新以及主要、次要和补丁工作程序节点更新。</li>
     <li>使用 API、CLI 或控制台工具来[恢复](/docs/containers?topic=containers-cs_troubleshoot#cs_troubleshoot)基础架构资源，或者设置并配置可选的[工作程序节点自动恢复](/docs/containers?topic=containers-health#autorecovery)。</li></ul>
     <br><br></td>
      </tr>
    <tr>
      <td align="center"><img src="images/icon_locked.svg" alt="锁图标"/><br>安全功能丰富的环境</td>
      <td>
      **IBM 的责任**：
      <ul>
      <li>维护与[各种行业合规标准](/docs/containers?topic=containers-faqs#standards)（例如，PCI DSS）相应的控制措施。</li>
      <li>监视、隔离和恢复集群主节点。</li>
      <li>提供 Kubernetes 主节点 API 服务器、etcd、调度程序和控制器管理器组件的高可用性副本，以防止主节点发生中断。</li>
      <li>自动应用主要安全补丁更新，并提供工作程序节点安全补丁更新。</li>
      <li>启用某些安全设置，例如工作程序节点上的加密磁盘。</li>
      <li>对工作程序节点禁用某些不安全操作，例如不允许用户通过 SSH 登录到主机。</li>
      <li>使用 TLS 加密主节点和工作程序节点之间的通信。</li>
      <li>为工作程序节点操作系统提供符合 CIS 的 Linux 映像。</li>
      <li>持续监视主节点和工作程序节点映像，以检测漏洞和安全合规性问题。</li>
      <li>为工作程序节点供应两个本地 SSD AES 256 位加密数据分区。</li>
      <li>为集群网络连接提供选项，例如公共和专用服务端点。</li>
      <li>提供用于计算隔离的选项，例如专用虚拟机、裸机和具有可信计算的裸机。</li>
      <li>将 Kubernetes 基于角色的访问控制 (RBAC) 与 {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM) 相集成。</li>
      </ul>
      <br><br>
      **您的责任**：
      <ul>
      <li>使用 API、CLI 或控制台工具将提供的[安全补丁更新](/docs/containers?topic=containers-changelog#changelog)应用于工作程序节点。</li>
      <li>选择如何设置[集群网络](/docs/containers?topic=containers-plan_clusters)，并配置进一步的[安全设置](/docs/containers?topic=containers-security#security)，以满足工作负载的安全性和合规性需求。如果适用，请配置[防火墙](/docs/containers?topic=containers-firewall#firewall)。</li></ul>
      <br><br></td>
      </tr>

      <tr>
        <td align="center"><img src="images/icon_code.svg" alt="代码方括号的图标"/><br>应用程序编排</td>
        <td>
        **IBM 的责任**：
      <ul>
        <li>供应已安装 Kubernetes 组件的集群，以便您可以访问 Kubernetes API。</li>
        <li>提供若干受管附加组件以扩展应用程序的功能，例如 [Istio](/docs/containers?topic=containers-istio#istio) 和 [Knative](/docs/containers?topic=containers-serverless-apps-knative)。为您简化了维护工作，因为 IBM 为受管附加组件提供了安装和更新。</li>
        <li>提供了集群与精选第三方合作伙伴关系技术（例如，{{site.data.keyword.la_short}}、{{site.data.keyword.mon_short}} 和 Portworx）的集成。</li>
        <li>提供自动化以支持与其他 {{site.data.keyword.Bluemix_notm}} 服务的服务绑定。</li>
        <li>使用映像拉取私钥创建集群，以便在 `default` Kubernetes 名称空间中的部署可以从 {{site.data.keyword.registrylong_notm}} 中拉取映像。</li>
        <li>提供存储类和插件，以支持持久卷用于应用程序。</li>
        <li>创建其子网 IP 地址保留用于在外部公开应用程序的集群。</li>
        <li>支持本机 Kubernetes 公共和专用负载均衡器和 Ingress 路径用于在外部公开服务。</li>
        </ul>
        <br><br>
        **您的责任**：
        <ul>
        <li>使用提供的工具和功能来[配置和部署](/docs/containers?topic=containers-app#app)；[设置许可权](/docs/containers?topic=containers-users#users)；[与其他服务集成](/docs/containers?topic=containers-supported_integrations#supported_integrations)；[在外部处理](/docs/containers?topic=containers-cs_network_planning#cs_network_planning)；[监视运行状况](/docs/containers?topic=containers-health#health)；[保存、备份和复原数据](/docs/containers?topic=containers-storage_planning#storage_planning)；以及在其他方面管理[高可用性](/docs/containers?topic=containers-ha#ha)和弹性工作负载。</li>
        </ul>
        </td>
        </tr>
  </tbody>
  </table>

<br />


## 滥用 {{site.data.keyword.containerlong_notm}}
{: #terms}

客户机不能滥用 {{site.data.keyword.containerlong_notm}}。
{:shortdesc}

滥用包括：

*   任何非法活动
*   分发或执行恶意软件
*   损害 {{site.data.keyword.containerlong_notm}} 或干扰任何人使用 {{site.data.keyword.containerlong_notm}}
*   损害或干扰任何人使用任何其他服务或系统
*   对任何服务或系统进行未经授权的访问
*   对任何服务或系统进行未经授权的修改
*   侵犯他人的权利

请参阅 [Cloud Services 条款](/docs/overview/terms-of-use?topic=overview-terms#terms)，以获取总体使用条款。
