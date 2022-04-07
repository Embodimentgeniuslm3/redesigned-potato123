---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-05"

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


# 版本更改日志
{: #changelog}

查看可用于 {{site.data.keyword.containerlong}} Kubernetes 集群的主要更新、次要更新和补丁更新的版本更改信息。更改包括对 Kubernetes 和 {{site.data.keyword.Bluemix_notm}} Provider 组件的更新。
{:shortdesc}

除非在更改日志中另有说明，否则 {{site.data.keyword.containerlong_notm}} Provider 版本支持处于 Beta 阶段的 Kubernetes API 和功能。会随时更改的 Kubernetes Alpha 功能均已禁用。

有关主版本、次版本和补丁版本以及次版本之间的准备操作的更多信息，请参阅 [Kubernetes 版本](/docs/containers?topic=containers-cs_versions)。
{: tip}

有关自上一个版本以来的更改的信息，请参阅以下更改日志。
-  V1.14 [更改日志](#114_changelog)。
-  V1.13 [更改日志](#113_changelog)。
-  V1.12 [更改日志](#112_changelog)。
-  **不推荐**：V1.11 [更改日志](#111_changelog)。
-  对不受支持版本的更改日志[归档](#changelog_archive)。

一些更改日志针对_工作程序节点修订包_，仅适用于工作程序节点。您必须[应用这些补丁](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update)才能确保工作程序节点的安全合规性。这些工作程序节点修订包的版本可能高于主节点，因为某些构建修订包特定于工作程序节点。另一些更改日志针对_主节点修订包_，仅适用于集群主节点。主节点修订包可能不会自动应用。您可以选择[手动进行应用](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_update)。有关补丁类型的更多信息，请参阅[更新类型](/docs/containers?topic=containers-cs_versions#update_types)。
{: note}

</br>

## V1.14 更改日志
{: #114_changelog}

### 2019 年 6 月 4 日发布的 1.14.2_1521 的更改日志
{: #1142_1521}

下表显示了补丁 1.14.2_1521 中包含的更改。
{: shortdesc}

<table summary="自 V1.14.1_1519 以来进行的更改">
<caption>自 V1.14.1_1519 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>集群 DNS 配置</td>
<td>不适用</td>
<td>不适用</td>
<td>修复了可能导致 Kubernetes DNS 和 CoreDNS pod 在执行集群创建 (`create`) 或更新 (`update`) 操作之后同时运行的错误。</td>
</tr>
<tr>
<td>集群主节点 HA 配置</td>
<td>不适用</td>
<td>不适用</td>
<td>更新了配置，以最大程度地减少在主节点更新期间发生的间歇性主节点网络连接失败。</td>
</tr>
<tr>
<td>etcd</td>
<td>V3.3.11</td>
<td>V3.3.13</td>
<td>请参阅 [etcd 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/coreos/etcd/releases/v3.3.13)。</td>
</tr>
<tr>
<td>GPU 设备插件和安装程序</td>
<td>55c1f66</td>
<td>32257d3</td>
<td>针对 [CVE-2018-10844 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844)、[CVE-2018-10845 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845)、[CVE-2018-10846 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846)、[CVE-2019-3829 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829)、[CVE-2019-3836 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836)、[CVE-2019-9893 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893)、[CVE-2019-5435 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435) 和 [CVE-2019-5436 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436)，更新了映像。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>V1.14.1-71</td>
<td>V1.14.2-100</td>
<td>更新为支持 Kubernetes 1.14.2 发行版。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>V1.14.1</td>
<td>V1.14.2</td>
<td>请参阅 [Kubernetes 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/kubernetes/releases/tag/v1.14.2)。</td>
</tr>
<tr>
<td>Kubernetes Metrics Server</td>
<td>V0.3.1</td>
<td>V0.3.3</td>
<td>请参阅 [Kubernetes Metrics Server 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes-incubator/metrics-server/releases/tag/v0.3.3)。</td>
</tr>
<tr>
<td>可信计算代理程序</td>
<td>13c7ef0</td>
<td>e8c6d72</td>
<td>针对 [CVE-2018-10844 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844)、[CVE-2018-10845 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845)、[CVE-2018-10846 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846)、[CVE-2019-3829 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829)、[CVE-2019-3836 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836)、[CVE-2019-9893 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893)、[CVE-2019-5435 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435) 和 [CVE-2019-5436 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436)，更新了映像。</td>
</tr>
</tbody>
</table>

### 2019 年 5 月 20 日发布的工作程序节点 FP1.14.1_1519 的更改日志
{: #1141_1519}

下表显示了补丁 1.14.1_1519 中包含的更改。
{: shortdesc}

<table summary="自 V1.14.1_1518 以来进行的更改">
<caption>自 V1.14.1_1518 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 16.04 内核</td>
<td>4.4.0-145-generic</td>
<td>4.4.0-148-generic</td>
<td>使用 [CVE-2018-12126 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html)、[CVE-2018-12127 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html) 和 [CVE-2018-12130 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html) 的内核更新对工作程序节点映像进行了更新。</td>
</tr>
<tr>
<td>Ubuntu 18.04 内核</td>
<td>4.15.0-47-generic</td>
<td>4.15.0-50-generic</td>
<td>使用 [CVE-2018-12126 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html)、[CVE-2018-12127 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html) 和 [CVE-2018-12130 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html) 的内核更新对工作程序节点映像进行了更新。</td>
</tr>
</tbody>
</table>

### 2019 年 5 月 13 日发布的 1.14.1_1518 的更改日志
{: #1141_1518}

下表显示了补丁 1.14.1_1518 中包含的更改。
{: shortdesc}

<table summary="自 V1.14.1_1516 以来进行的更改">
<caption>自 V1.14.1_1516 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>集群主节点 HA 代理</td>
<td>1.9.6-alpine</td>
<td>1.9.7-alpine</td>
<td>请参阅 [HAProxy 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.haproxy.org/download/1.9/src/CHANGELOG)。更新解决了 [CVE-2019-6706 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6706)。</td>
</tr>
<tr>
<td>Kubernetes 配置</td>
<td>不适用</td>
<td>不适用</td>
<td>Kubernetes API 服务器审计策略配置更新为不记录 `/openapi/v2*` 只读 URL。此外，Kubernetes 控制器管理器配置将签署的 `kubelet` 证书的有效期从 1 年延长到 3 年。</td>
</tr>
<tr>
<td>OpenVPN 客户机配置</td>
<td>不适用</td>
<td>不适用</td>
<td>现在，`kube-system` 名称空间中的 OpenVPN 客户机 `vpn-*` pod 将 `dnsPolicy` 设置为 `Default`，以避免在集群 DNS 停止运行时 pod 发生故障。</td>
</tr>
<tr>
<td>可信计算代理程序</td>
<td>e7182c7</td>
<td>13c7ef0</td>
<td>针对 [CVE-2016-7076 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-7076) 和 [CVE-2017-1000368 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1000368)，更新了映像。</td>
</tr>
</tbody>
</table>

### 2019 年 5 月 7 日发布的 1.14.1_1516 的更改日志
{: #1141_1516}

下表显示了补丁 1.14.1_1516 中包含的更改。
{: shortdesc}

<table summary="自 V1.13.5_1519 以来进行的更改">
<caption>自 V1.13.5_1519 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>V3.4.4</td>
<td>V3.6.1</td>
<td>请参阅 [Calico 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://docs.projectcalico.org/v3.6/release-notes/)。</td>
</tr>
<tr>
<td>CoreDNS</td>
<td>1.2.6</td>
<td>1.3.1</td>
<td>请参阅 [CoreDNS 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://coredns.io/2019/01/13/coredns-1.3.1-release/)。更新包括在集群 DNS 服务上添加了[度量值端口 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://coredns.io/plugins/metrics/)。<br><br>现在，CoreDNS 是唯一支持的集群 DNS 提供程序。如果将集群从 Kubernetes 的较早版本更新到 V1.14，并且使用的是 KubeDNS，那么在集群更新期间，KubeDNS 会自动迁移到 CoreDNS。有关更多信息或要在更新之前测试 CoreDNS，请参阅[配置集群 DNS 提供程序](https://cloud.ibm.com/docs/containers?topic=containers-cluster_dns#cluster_dns)。</td>
</tr>
<tr>
<td>GPU 设备插件和安装程序</td>
<td>9ff3fda</td>
<td>ed0dafc</td>
<td>针对 [CVE-2019-1543 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543)，更新了映像。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>V1.13.5-107</td>
<td>V1.14.1-71</td>
<td>更新为支持 Kubernetes 1.14.1 发行版。此外，`calicoctl` 版本更新为 3.6.1。修复了对只有一个工作程序节点可用于负载均衡器 pod 的 V2.0 负载均衡器的更新。现在，专用负载均衡器支持在[专用边缘工作程序节点](/docs/containers?topic=containers-edge#edge)上运行。</td>
</tr>
<tr>
<td>IBM pod 安全策略</td>
<td>不适用</td>
<td>不适用</td>
<td>[IBM pod 安全策略](/docs/containers?topic=containers-psp#ibm_psp)更新为支持 Kubernetes [RunAsGroup ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/policy/pod-security-policy/#users-and-groups) 功能。</td>
</tr>
<tr>
<td>`kubelet` 配置</td>
<td>不适用</td>
<td>不适用</td>
<td>将 `--pod-max-pids` 选项设置为 `14336`，以阻止单个 pod 使用工作程序节点上的所有进程标识。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>V1.13.5</td>
<td>V1.14.1</td>
<td>请参阅 [Kubernetes 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/kubernetes/releases/tag/v1.14.1) 和 [Kubernetes 1.14 博客 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/blog/2019/03/25/kubernetes-1-14-release-announcement/)。<br><br>Kubernetes 缺省基于角色的访问控制 (RBAC) 策略不再[授予未经认证的用户对发现和许可权检查 API 的访问权 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#discovery-roles)。此更改仅适用于新的 V1.14 集群。如果从先前版本更新集群，那么未经认证的用户仍有权访问发现和许可权检查 API。</td>
</tr>
<tr>
<td>Kubernetes 许可控制器配置</td>
<td>不适用</td>
<td>不适用</td>
<td><ul>
<li>向集群的 Kubernetes API 服务器的 `--enable-admission-plugins` 选项添加了 `NodeRestriction`，并将相关集群资源配置为支持此安全增强功能。</li>
<li>从集群的 Kubernetes API 服务器的 `--enable-admission-plugins` 选项中除去了 `Initializers`，从 `--runtime-config` 选项中除去了 `admissionregistration.k8s.io/v1alpha1=true`，因为不再支持这些 API。可以改为使用 [Kubernetes 许可 Webhook ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/reference/access-authn-authz/extensible-admission-controllers/)。</li></ul></td>
</tr>
<tr>
<td>Kubernetes DNS Autoscaler</td>
<td>1.3.0</td>
<td>1.4.0</td>
<td>请参阅 [Kubernetes DNS Autoscaler 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes-incubator/cluster-proportional-autoscaler/releases/tag/1.4.0)。</td>
</tr>
<tr>
<td>Kubernetes 功能检测点配置</td>
<td>不适用</td>
<td>不适用</td>
<td><ul>
  <li>添加了 `RuntimeClass=false`，用于禁止选择容器运行时配置。</li>
  <li>除去了 `ExperimentalCriticalPodAnnotation=true`，因为不再支持 `scheduler.alpha.kubernetes.io/critical-pod` pod 注释。可以改为使用 [Kubernetes pod 优先级 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/docs/containers?topic=containers-pod_priority#pod_priority)。</li></ul></td>
</tr>
<tr>
<td>可信计算代理程序</td>
<td>e132aa4</td>
<td>e7182c7</td>
<td>针对 [CVE-2019-11068 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-11068)，更新了映像。</td>
</tr>
</tbody>
</table>

<br />


## V1.13 更改日志
{: #113_changelog}

### 2019 年 6 月 4 日发布的 1.13.6_1524 的更改日志
{: #1136_1524}

下表显示了补丁 1.13.6_1524 中包含的更改。
{: shortdesc}

<table summary="自 V1.13.6_1522 以来进行的更改">
<caption>自 V1.13.6_1522 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>集群 DNS 配置</td>
<td>不适用</td>
<td>不适用</td>
<td>修复了可能导致 Kubernetes DNS 和 CoreDNS pod 在执行集群创建 (`create`) 或更新 (`update`) 操作之后同时运行的错误。</td>
</tr>
<tr>
<td>集群主节点 HA 配置</td>
<td>不适用</td>
<td>不适用</td>
<td>更新了配置，以最大程度地减少在主节点更新期间发生的间歇性主节点网络连接失败。</td>
</tr>
<tr>
<td>etcd</td>
<td>V3.3.11</td>
<td>V3.3.13</td>
<td>请参阅 [etcd 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/coreos/etcd/releases/v3.3.13)。</td>
</tr>
<tr>
<td>GPU 设备插件和安装程序</td>
<td>55c1f66</td>
<td>32257d3</td>
<td>针对 [CVE-2018-10844 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844)、[CVE-2018-10845 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845)、[CVE-2018-10846 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846)、[CVE-2019-3829 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829)、[CVE-2019-3836 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836)、[CVE-2019-9893 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893)、[CVE-2019-5435 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435) 和 [CVE-2019-5436 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436)，更新了映像。</td>
</tr>
<tr>
<td>Kubernetes Metrics Server</td>
<td>V0.3.1</td>
<td>V0.3.3</td>
<td>请参阅 [Kubernetes Metrics Server 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes-incubator/metrics-server/releases/tag/v0.3.3)。</td>
</tr>
<tr>
<td>可信计算代理程序</td>
<td>13c7ef0</td>
<td>e8c6d72</td>
<td>针对 [CVE-2018-10844 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844)、[CVE-2018-10845 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845)、[CVE-2018-10846 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846)、[CVE-2019-3829 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829)、[CVE-2019-3836 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836)、[CVE-2019-9893 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893)、[CVE-2019-5435 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435) 和 [CVE-2019-5436 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436)，更新了映像。</td>
</tr>
</tbody>
</table>

### 2019 年 5 月 20 日发布的工作程序节点 FP1.13.6_1522 的更改日志
{: #1136_1522}

下表显示了补丁 1.13.6_1522 中包含的更改。
{: shortdesc}

<table summary="自 V1.13.6_1521 以来进行的更改">
<caption>自 V1.13.6_1521 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 16.04 内核</td>
<td>4.4.0-145-generic</td>
<td>4.4.0-148-generic</td>
<td>使用 [CVE-2018-12126 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html)、[CVE-2018-12127 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html) 和 [CVE-2018-12130 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html) 的内核更新对工作程序节点映像进行了更新。</td>
</tr>
<tr>
<td>Ubuntu 18.04 内核</td>
<td>4.15.0-47-generic</td>
<td>4.15.0-50-generic</td>
<td>使用 [CVE-2018-12126 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html)、[CVE-2018-12127 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html) 和 [CVE-2018-12130 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html) 的内核更新对工作程序节点映像进行了更新。</td>
</tr>
</tbody>
</table>

### 2019 年 5 月 13 日发布的 1.13.6_1521 的更改日志
{: #1136_1521}

下表显示了补丁 1.13.6_1521 中包含的更改。
{: shortdesc}

<table summary="自 V1.13.5_1519 以来进行的更改">
<caption>自 V1.13.5_1519 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>集群主节点 HA 代理</td>
<td>1.9.6-alpine</td>
<td>1.9.7-alpine</td>
<td>请参阅 [HAProxy 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.haproxy.org/download/1.9/src/CHANGELOG)。更新解决了 [CVE-2019-6706 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6706)。</td>
</tr>
<tr>
<td>GPU 设备插件和安装程序</td>
<td>9ff3fda</td>
<td>55c1f66</td>
<td>针对 [CVE-2019-1543 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543)，更新了映像。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>V1.13.5-107</td>
<td>V1.13.6-139</td>
<td>更新为支持 Kubernetes 1.13.6 发行版。此外，修复了只有一个工作程序节点可用于负载均衡器 pod 的 V2.0 负载均衡器的更新过程。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>V1.13.5</td>
<td>V1.13.6</td>
<td>请参阅 [Kubernetes 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/kubernetes/releases/tag/v1.13.6)。</td>
</tr>
<tr>
<td>Kubernetes 配置</td>
<td>不适用</td>
<td>不适用</td>
<td>Kubernetes API 服务器审计策略配置更新为不记录 `/openapi/v2*` 只读 URL。此外，Kubernetes 控制器管理器配置将签署的 `kubelet` 证书的有效期从 1 年延长到 3 年。</td>
</tr>
<tr>
<td>OpenVPN 客户机配置</td>
<td>不适用</td>
<td>不适用</td>
<td>现在，`kube-system` 名称空间中的 OpenVPN 客户机 `vpn-*` pod 将 `dnsPolicy` 设置为 `Default`，以避免在集群 DNS 停止运行时 pod 发生故障。</td>
</tr>
<tr>
<td>可信计算代理程序</td>
<td>e132aa4</td>
<td>13c7ef0</td>
<td>针对 [CVE-2016-7076 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-7076)、[CVE-2017-1000368 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1000368) 和 [CVE-2019-11068 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-11068)，更新了映像。</td>
</tr>
</tbody>
</table>

### 2019 年 4 月 29 日发布的工作程序节点 FP1.13.5_1519 的更改日志
{: #1135_1519}

下表显示了工作程序节点 FP1.13.5_1519 中包含的更改。
{: shortdesc}

<table summary="自 V1.13.5_1518 以来进行的更改">
<caption>自 V1.13.5_1518 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 软件包</td>
<td>不适用</td>
<td>不适用</td>
<td>已安装的 Ubuntu 软件包的更新。</td>
</tr>
<tr>
<td>containerd</td>
<td>1.2.5</td>
<td>1.2.6</td>
<td>请参阅 [containerd 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/containerd/containerd/releases/tag/v1.2.6)。</td>
</tr>
</tbody>
</table>

### 2019 年 4 月 15 日发布的工作程序节点 FP1.13.5_1518 的更改日志
{: #1135_1518}

下表显示了工作程序节点 FP1.13.5_1518 中包含的更改。
{: shortdesc}

<table summary="自 V1.13.5_1517 以来进行的更改">
<caption>自 V1.13.5_1517 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 软件包</td>
<td>不适用</td>
<td>不适用</td>
<td>更新了安装的 Ubuntu 软件包，包括针对 [CVE-2019-3842 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-3842.html)，更新了 `systemd`。</td>
</tr>
</tbody>
</table>

### 2019 年 4 月 8 日发布的 1.13.5_1517 的更改日志
{: #1135_1517}

下表显示了补丁 1.13.5_1517 中包含的更改。
{: shortdesc}

<table summary="自 V1.13.4_1516 以来进行的更改">
<caption>自 V1.13.4_1516 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>V3.4.0</td>
<td>V3.4.4</td>
<td>请参阅 [Calico 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://docs.projectcalico.org/v3.4/releases/#v344)。更新解决了 [CVE-2019-9946 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9946)。</td>
</tr>
<tr>
<td>集群主节点 HA 代理</td>
<td>1.8.12-alpine</td>
<td>1.9.6-alpine</td>
<td>请参阅 [HAProxy 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.haproxy.org/download/1.9/src/CHANGELOG)。更新解决了 [CVE-2018-0732 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732)、[CVE-2018-0734 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734)、[CVE-2018-0737 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737)、[CVE-2018-5407 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407)、[CVE-2019-1543 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543) 和 [CVE-2019-1559 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559)。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>V1.13.4-86</td>
<td>V1.13.5-107</td>
<td>更新为支持 Kubernetes 1.13.5 和 Calico 3.4.4 发行版。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>V1.13.4</td>
<td>V1.13.5</td>
<td>请参阅 [Kubernetes 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/kubernetes/releases/tag/v1.13.5)。</td>
</tr>
<tr>
<td>可信计算代理程序</td>
<td>a02f765</td>
<td>e132aa4</td>
<td>针对 [CVE-2017-12447 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-12447)，更新了映像。</td>
</tr>
<tr>
<td>Ubuntu 16.04 内核</td>
<td>4.4.0-143-generic</td>
<td>4.4.0-145-generic</td>
<td>通过针对 [CVE-2019-9213 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html) 的内核更新，更新了工作程序节点映像。</td>
</tr>
<tr>
<td>Ubuntu 18.04 内核</td>
<td>4.15.0-46-generic</td>
<td>4.15.0-47-generic</td>
<td>通过针对 [CVE-2019-9213 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html) 的内核更新，更新了工作程序节点映像。</td>
</tr>
</tbody>
</table>

### 2019 年 4 月 1 日发布的工作程序节点 FP1.13.4_1516 的更改日志
{: #1134_1516}

下表显示了工作程序节点 FP1.13.4_1516 中包含的更改。
{: shortdesc}

<table summary="自 V1.13.4_1515 以来进行的更改">
<caption>自 V1.13.4_1515 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>工作程序节点资源利用率</td>
<td>不适用</td>
<td>不适用</td>
<td>增大了针对 kubelet 和 containerd 的内存保留量，以防止这些组件耗尽资源。有关更多信息，请参阅[工作程序节点资源保留](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node)。</td>
</tr>
</tbody>
</table>

### 2019 年 3 月 26 日发布的主节点 FP1.13.4_1515 的更改日志
{: #1134_1515}

下表显示了主节点 FP1.13.4_1515 中包含的更改。
{: shortdesc}

<table summary="自 V1.13.4_1513 以来进行的更改">
<caption>自 V1.13.4_1513 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>集群 DNS 配置</td>
<td>不适用</td>
<td>不适用</td>
<td>修复了 Kubernetes V1.11 中的更新过程，以阻止更新将集群 DNS 提供程序切换到 CoreDNS。在更新后，您仍可以[将 CoreDNS 设置为集群 DNS 提供程序](/docs/containers?topic=containers-cluster_dns#set_coredns)。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 插件</td>
<td>345</td>
<td>346</td>
<td>针对 [CVE-2019-9741 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741)，更新了映像。</td>
</tr>
<tr>
<td>密钥管理服务提供程序</td>
<td>166</td>
<td>167</td>
<td>修复了针对管理 Kubernetes 私钥，间歇性发生的`超过了上下文截止期限`和`超时`错误。此外，还修复了对密钥管理服务的更新，原先更新可能会使现有 Kubernetes 私钥处于未加密状态。更新包含针对 [CVE-2019-9741 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741) 的修订。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider 的负载均衡器和负载均衡器监视器</td>
<td>143</td>
<td>146</td>
<td>针对 [CVE-2019-9741 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741)，更新了映像。</td>
</tr>
</tbody>
</table>

### 2019 年 3 月 20 日发布的 1.13.4_1513 的更改日志
{: #1134_1513}

下表显示了补丁 1.13.4_1513 中包含的更改。
{: shortdesc}

<table summary="自 V1.13.4_1510 以来进行的更改">
<caption>自 V1.13.4_1510 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>集群 DNS 配置</td>
<td>不适用</td>
<td>不适用</td>
<td>修复了必须缩减未使用的集群 DNS 时，可能导致集群主节点操作（例如，`refresh` 或 `update`）失败的错误。</td>
</tr>
<tr>
<td>集群主节点 HA 代理配置</td>
<td>不适用</td>
<td>不适用</td>
<td>更新了配置，以更好地处理集群主节点的间歇性连接失败。</td>
</tr>
<tr>
<td>CoreDNS 配置</td>
<td>不适用</td>
<td>不适用</td>
<td>从 1.12 更新集群 Kubernetes 版本后，CoreDNS 配置更新为支持[多个 Corefile ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://coredns.io/2017/07/23/corefile-explained/)。</td>
</tr>
<tr>
<td>containerd</td>
<td>1.2.4</td>
<td>1.2.5</td>
<td>请参阅 [containerd 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/containerd/containerd/releases/tag/v1.2.5)。更新包含针对 [CVE-2019-5736 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5736) 的改进修订。</td>
</tr>
<tr>
<td>GPU 设备插件和安装程序</td>
<td>e32d51c</td>
<td>9ff3fda</td>
<td>GPU 驱动程序更新为 [418.43 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.nvidia.com/object/unix.html)。更新包含针对 [CVE-2019-9741 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9741.html) 的修订。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 插件</td>
<td>344</td>
<td>345</td>
<td>添加了对[专用服务端点](/docs/containers?topic=containers-cs_network_cluster#set-up-private-se)的支持。</td>
</tr>
<tr>
<td>内核</td>
<td>4.4.0-141</td>
<td>4.4.0-143</td>
<td>通过 [CVE-2019-6133 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-6133.html) 的内核更新，更新了工作程序节点映像。</td>
</tr>
<tr>
<td>密钥管理服务提供程序</td>
<td>136</td>
<td>166</td>
<td>针对 [CVE-2018-16890 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16890)、[CVE-2019-3822 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3822) 和 [CVE-2019-3823 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3823)，更新了映像。</td>
</tr>
<tr>
<td>可信计算代理程序</td>
<td>5f3d092</td>
<td>a02f765</td>
<td>针对 [CVE-2018-10779 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10779)、[CVE-2018-12900 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-12900)、[CVE-2018-17000 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-17000)、[CVE-2018-19210 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-19210)、[CVE-2019-6128 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6128) 和 [CVE-2019-7663 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-7663)，更新了映像。</td>
</tr>
</tbody>
</table>

### 2019 年 3 月 4 日发布的 1.13.4_1510 的更改日志
{: #1134_1510}

下表显示了补丁 1.13.4_1510 中包含的更改。
{: shortdesc}

<table summary="自 V1.13.2_1509 以来进行的更改">
<caption>自 V1.13.2_1509 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>集群 DNS 提供程序</td>
<td>不适用</td>
<td>不适用</td>
<td>将 Kubernetes DNS 和 CoreDNS pod 内存限制从 `170Mi` 增加到了 `400Mi`，以便处理更多集群服务。</td>
</tr>
<tr>
<td>GPU 设备插件和安装程序</td>
<td>eb3a259</td>
<td>e32d51c</td>
<td>针对 [CVE-2019-6454 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454)，更新了映像。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>V1.13.2-62</td>
<td>V1.13.4-86</td>
<td>更新为支持 Kubernetes 1.13.4 发行版。修复了 V1.0 负载均衡器将 `externalTrafficPolicy` 设置为 `local` 时的定期连接问题。更新了 LoadBalancer V1.0 和 V2.0 事件，以使用最新的 {{site.data.keyword.Bluemix_notm}} 文档链接。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 插件</td>
<td>342</td>
<td>344</td>
<td>将映像的基本操作系统从 Fedora 更改为 Alpine。针对 [CVE-2019-6486 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486)，更新了映像。</td>
</tr>
<tr>
<td>密钥管理服务提供程序</td>
<td>122</td>
<td>136</td>
<td>将客户机超时增加到 {{site.data.keyword.keymanagementservicefull_notm}}。针对 [CVE-2019-6486 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486)，更新了映像。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>V1.13.2</td>
<td>V1.13.4</td>
<td>请参阅 [Kubernetes 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/kubernetes/releases/tag/v1.13.4)。更新解决了 [CVE-2019-6486 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) 和 [CVE-2019-1002100 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1002100)。</td>
</tr>
<tr>
<td>Kubernetes 配置</td>
<td>不适用</td>
<td>不适用</td>
<td>向 `--feature-gates` 选项添加了 `ExperimentalCriticalPodAnnotation=true`。此设置可帮助将 pod 从不推荐使用的 `scheduler.alpha.kubernetes.io/critical-pod` 注释迁移到 [Kubernetes pod 优先级支持](/docs/containers?topic=containers-pod_priority#pod_priority)。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider 的负载均衡器和负载均衡器监视器</td>
<td>132</td>
<td>143</td>
<td>针对 [CVE-2019-6486 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486)，更新了映像。</td>
</tr>
<tr>
<td>OpenVPN 客户机和服务器</td>
<td>2.4.6-r3-IKS-13</td>
<td>2.4.6-r3-IKS-25</td>
<td>针对 [CVE-2019-1559 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559)，更新了映像。</td>
</tr>
<tr>
<td>可信计算代理程序</td>
<td>1ea5ad3</td>
<td>5f3d092</td>
<td>针对 [CVE-2019-6454 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454)，更新了映像。</td>
</tr>
</tbody>
</table>

### 2019 年 2 月 27 日发布的工作程序节点 FP1.13.2_1509 的更改日志
{: #1132_1509}

下表显示了工作程序节点 FP1.13.2_1509 中包含的更改。
{: shortdesc}

<table summary="自 V1.13.2_1508 以来进行的更改">
<caption>自 V1.13.2_1508 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>内核</td>
<td>4.4.0-141</td>
<td>4.4.0-142</td>
<td>通过 [CVE-2018-19407 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-142.168/changelog) 的内核更新，更新了工作程序节点映像。</td>
</tr>
</tbody>
</table>

### 2019 年 2 月 15 日发布的工作程序节点 FP1.13.2_1508 的更改日志
{: #1132_1508}

下表显示了工作程序节点 FP1.13.2_1508 中包含的更改。
{: shortdesc}

<table summary="自 V1.13.2_1507 以来进行的更改">
<caption>自 V1.13.2_1507 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>集群主节点 HA 代理配置</td>
<td>不适用</td>
<td>不适用</td>
<td>将 pod 配置 `spec.priorityClassName` 值更改为 `system-node-critical`，并将 `spec.priority` 值设置为 `2000001000`。</td>
</tr>
<tr>
<td>containerd</td>
<td>1.2.2</td>
<td>1.2.4</td>
<td>请参阅 [containerd 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/containerd/containerd/releases/tag/v1.2.4)。更新解决了 [CVE-2019-5736 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5736)。</td>
</tr>
<tr>
<td>Kubernetes `kubelet` 配置</td>
<td>不适用</td>
<td>不适用</td>
<td>启用了 `ExperimentalCriticalPodAnnotation` 功能检测点，以防止关键静态 pod 逐出。将 `event-qps` 选项设置为 `0` 可阻止创建速率限制事件。</td>
</tr>
</tbody>
</table>

### 2019 年 2 月 5 日发布的 1.13.2_1507 的更改日志
{: #1132_1507}

下表显示了补丁 1.13.2_1507 中包含的更改。
{: shortdesc}

<table summary="自 V1.12.4_1535 以来进行的更改">
<caption>自 V1.12.4_1535 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>V3.3.1</td>
<td>V3.4.0</td>
<td>请参阅 [Calico 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://docs.projectcalico.org/v3.4/releases/#v340)。</td>
</tr>
<tr>
<td>集群 DNS 提供程序</td>
<td>不适用</td>
<td>不适用</td>
<td>现在，CoreDNS 是新集群的缺省集群 DNS 提供程序。如果将使用 KubeDNS 作为集群 DNS 提供程序的现有集群更新为 1.13，那么 KubeDNS 会继续作为集群 DNS 提供程序。但是，您可以选择[改为使用 CoreDNS](/docs/containers?topic=containers-cluster_dns#dns_set)。</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.5</td>
<td>1.2.2</td>
<td>请参阅 [containerd 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/containerd/containerd/releases/tag/v1.2.2)。</td>
</tr>
<tr>
<td>CoreDNS</td>
<td>1.2.2</td>
<td>1.2.6</td>
<td>请参阅 [CoreDNS 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/coredns/coredns/releases/tag/v1.2.6)。此外，CoreDNS 配置更新为[支持多个 Corefile ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://coredns.io/2017/07/23/corefile-explained/)。</td>
</tr>
<tr>
<td>etcd</td>
<td>V3.3.1</td>
<td>V3.3.11</td>
<td>请参阅 [etcd 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/coreos/etcd/releases/v3.3.11)。此外，支持用于 etcd 的密码套件现在仅限于具有高强度加密的子集（128 位或更多位）。</td>
</tr>
<tr>
<td>GPU 设备插件和安装程序</td>
<td>13fdc0d</td>
<td>eb3a259</td>
<td>针对 [CVE-2019-3462 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) 和 [CVE-2019-6486 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486)，更新了映像。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>V1.12.4-118</td>
<td>V1.13.2-62</td>
<td>更新为支持 Kubernetes 1.13.2 发行版。此外，`calicoctl` 版本更新为 3.4.0。修复了在发生工作程序节点状态更改时，对 V2.0 负载均衡器的不必要的配置更新。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 插件</td>
<td>338</td>
<td>342</td>
<td>File Storage 插件按如下所示进行更新：<ul><li>支持使用[卷拓扑感知安排](/docs/containers?topic=containers-file_storage#file-topology)进行动态供应。</li>
<li>忽略存储器已删除时的持久卷声明 (PVC) 删除错误。</li>
<li>向失败的 PVC 添加了失败消息注释。</li>
<li>优化了存储供应程序控制器的领导者选举和再同步时间段设置，并将供应超时从 30 分钟增加到 1 小时。</li>
<li>在开始供应之前检查用户许可权。</li>
<li>向 `kube-system` 名称空间中的 `ibm-file-plugin` 和 `ibm-storage-watcher` 部署添加了 `CriticalAddonsOnly` 容忍度。</li></ul></td>
</tr>
<tr>
<td>密钥管理服务提供程序</td>
<td>111</td>
<td>122</td>
<td>添加了重试逻辑，以避免在 {{site.data.keyword.keymanagementservicefull_notm}} 管理 Kubernetes 私钥时发生临时故障。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>V1.12.4</td>
<td>V1.13.2</td>
<td>请参阅 [Kubernetes 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/kubernetes/releases/tag/v1.13.2)。</td>
</tr>
<tr>
<td>Kubernetes 配置</td>
<td>不适用</td>
<td>不适用</td>
<td>Kubernetes API 服务器审计策略配置更新为包含 `cluster-admin` 请求的日志记录元数据，并记录工作负载 `create`、`update` 和 `patch` 请求的请求主体。</td>
</tr>
<tr>
<td>Kubernetes DNS Autoscaler</td>
<td>1.2.0</td>
<td>1.3.0</td>
<td>请参阅 [Kubernetes DNS Autoscaler 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes-incubator/cluster-proportional-autoscaler/releases/tag/1.3.0)。</td>
</tr>
<tr>
<td>OpenVPN 客户机</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>针对 [CVE-2018-0734 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) 和 [CVE-2018-5407 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407)，更新了映像。向 `kube-system` 名称空间中的 `vpn` 部署添加了 `CriticalAddonsOnly` 容忍度。此外，现在 pod 配置将从私钥获取，而不是从配置映射获取。</td>
</tr>
<tr>
<td>OpenVPN 服务器</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>针对 [CVE-2018-0734 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) 和 [CVE-2018-5407 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407)，更新了映像。</td>
</tr>
<tr>
<td>systemd</td>
<td>230</td>
<td>229</td>
<td>[CVE-2018-16864 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16864) 的安全补丁。</td>
</tr>
</tbody>
</table>

<br />


## V1.12 更改日志
{: #112_changelog}

查看 V1.12 更改日志。
{: shortdesc}

### 2019 年 6 月 4 日发布的 1.12.9_1555 的更改日志
{: #1129_1555}

下表显示了补丁 1.12.9_1555 中包含的更改。
{: shortdesc}

<table summary="自 V1.12.8_1553 以来进行的更改">
<caption>自 V1.12.8_1553 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>集群 DNS 配置</td>
<td>不适用</td>
<td>不适用</td>
<td>修复了可能导致 Kubernetes DNS 和 CoreDNS pod 在执行集群创建 (`create`) 或更新 (`update`) 操作之后同时运行的错误。</td>
</tr>
<tr>
<td>集群主节点 HA 配置</td>
<td>不适用</td>
<td>不适用</td>
<td>更新了配置，以最大程度地减少在主节点更新期间发生的间歇性主节点网络连接失败。</td>
</tr>
<tr>
<td>etcd</td>
<td>V3.3.11</td>
<td>V3.3.13</td>
<td>请参阅 [etcd 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/coreos/etcd/releases/v3.3.13)。</td>
</tr>
<tr>
<td>GPU 设备插件和安装程序</td>
<td>55c1f66</td>
<td>32257d3</td>
<td>针对 [CVE-2018-10844 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844)、[CVE-2018-10845 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845)、[CVE-2018-10846 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846)、[CVE-2019-3829 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829)、[CVE-2019-3836 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836)、[CVE-2019-9893 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893)、[CVE-2019-5435 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435) 和 [CVE-2019-5436 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436)，更新了映像。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>V1.12.8-210</td>
<td>V1.12.9-227</td>
<td>更新为支持 Kubernetes 1.12.9 发行版。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>V1.12.8</td>
<td>V1.12.9</td>
<td>请参阅 [Kubernetes 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.9)。</td>
</tr>
<tr>
<td>Kubernetes Metrics Server</td>
<td>V0.3.1</td>
<td>V0.3.3</td>
<td>请参阅 [Kubernetes Metrics Server 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes-incubator/metrics-server/releases/tag/v0.3.3)。</td>
</tr>
<tr>
<td>可信计算代理程序</td>
<td>13c7ef0</td>
<td>e8c6d72</td>
<td>针对 [CVE-2018-10844 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844)、[CVE-2018-10845 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845)、[CVE-2018-10846 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846)、[CVE-2019-3829 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829)、[CVE-2019-3836 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836)、[CVE-2019-9893 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893)、[CVE-2019-5435 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435) 和 [CVE-2019-5436 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436)，更新了映像。</td>
</tr>
</tbody>
</table>

### 2019 年 5 月 20 日发布的工作程序节点 FP1.12.8_1553 的更改日志
{: #1128_1533}

下表显示了补丁 1.12.8_1553 中包含的更改。
{: shortdesc}

<table summary="自 V1.12.8_1553 以来进行的更改">
<caption>自 V1.12.8_1553 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 16.04 内核</td>
<td>4.4.0-145-generic</td>
<td>4.4.0-148-generic</td>
<td>使用 [CVE-2018-12126 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html)、[CVE-2018-12127 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html) 和 [CVE-2018-12130 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html) 的内核更新对工作程序节点映像进行了更新。</td>
</tr>
<tr>
<td>Ubuntu 18.04 内核</td>
<td>4.15.0-47-generic</td>
<td>4.15.0-50-generic</td>
<td>使用 [CVE-2018-12126 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html)、[CVE-2018-12127 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html) 和 [CVE-2018-12130 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html) 的内核更新对工作程序节点映像进行了更新。</td>
</tr>
</tbody>
</table>

### 2019 年 5 月 13 日发布的 1.12.8_1552 的更改日志
{: #1128_1552}

下表显示了补丁 1.12.8_1552 中包含的更改。
{: shortdesc}

<table summary="自 V1.12.7_1550 以来进行的更改">
<caption>自 V1.12.7_1550 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>集群主节点 HA 代理</td>
<td>1.9.6-alpine</td>
<td>1.9.7-alpine</td>
<td>请参阅 [HAProxy 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.haproxy.org/download/1.9/src/CHANGELOG)。更新解决了 [CVE-2019-6706 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6706)。</td>
</tr>
<tr>
<td>GPU 设备插件和安装程序</td>
<td>9ff3fda</td>
<td>55c1f66</td>
<td>针对 [CVE-2019-1543 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543)，更新了映像。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>V1.12.7-180</td>
<td>V1.12.8-210</td>
<td>更新为支持 Kubernetes 1.12.8 发行版。此外，修复了只有一个工作程序节点可用于负载均衡器 pod 的 V2.0 负载均衡器的更新过程。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>V1.12.7</td>
<td>V1.12.8</td>
<td>请参阅 [Kubernetes 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.8)。</td>
</tr>
<tr>
<td>Kubernetes 配置</td>
<td>不适用</td>
<td>不适用</td>
<td>Kubernetes API 服务器审计策略配置更新为不记录 `/openapi/v2*` 只读 URL。此外，Kubernetes 控制器管理器配置将签署的 `kubelet` 证书的有效期从 1 年延长到 3 年。</td>
</tr>
<tr>
<td>OpenVPN 客户机配置</td>
<td>不适用</td>
<td>不适用</td>
<td>现在，`kube-system` 名称空间中的 OpenVPN 客户机 `vpn-*` pod 将 `dnsPolicy` 设置为 `Default`，以避免在集群 DNS 停止运行时 pod 发生故障。</td>
</tr>
<tr>
<td>可信计算代理程序</td>
<td>e132aa4</td>
<td>13c7ef0</td>
<td>针对 [CVE-2016-7076 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-7076)、[CVE-2017-1000368 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1000368) 和 [CVE-2019-11068 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-11068)，更新了映像。</td>
</tr>
</tbody>
</table>

### 2019 年 4 月 29 日发布的工作程序节点 FP1.12.7_1550 的更改日志
{: #1127_1550}

下表显示了工作程序节点 FP1.12.7_1550 中包含的更改。
{: shortdesc}

<table summary="自 V1.12.7_1549 以来进行的更改">
<caption>自 V1.12.7_1549 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 软件包</td>
<td>不适用</td>
<td>不适用</td>
<td>已安装的 Ubuntu 软件包的更新。</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.6</td>
<td>1.1.7</td>
<td>请参阅 [containerd 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/containerd/containerd/releases/tag/v1.1.7)。</td>
</tr>
</tbody>
</table>


### 2019 年 4 月 15 日发布的工作程序节点 FP1.12.7_1549 的更改日志
{: #1127_1549}

下表显示了工作程序节点 FP1.12.7_1549 中包含的更改。
{: shortdesc}

<table summary="自 V1.12.7_1548 以来进行的更改">
<caption>自 V1.12.7_1548 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 软件包</td>
<td>不适用</td>
<td>不适用</td>
<td>更新了安装的 Ubuntu 软件包，包括针对 [CVE-2019-3842 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-3842.html)，更新了 `systemd`。</td>
</tr>
</tbody>
</table>

### 2019 年 4 月 8 日发布的 1.12.7_1548 的更改日志
{: #1127_1548}

下表显示了补丁 1.12.7_1548 中包含的更改。
{: shortdesc}

<table summary="自 V1.12.6_1547 以来进行的更改">
<caption>自 V1.12.6_1547 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>V3.3.1</td>
<td>V3.3.6</td>
<td>请参阅 [Calico 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://docs.projectcalico.org/v3.3/releases/#v336)。更新解决了 [CVE-2019-9946 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9946)。</td>
</tr>
<tr>
<td>集群主节点 HA 代理</td>
<td>1.8.12-alpine</td>
<td>1.9.6-alpine</td>
<td>请参阅 [HAProxy 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.haproxy.org/download/1.9/src/CHANGELOG)。更新解决了 [CVE-2018-0732 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732)、[CVE-2018-0734 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734)、[CVE-2018-0737 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737)、[CVE-2018-5407 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407)、[CVE-2019-1543 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543) 和 [CVE-2019-1559 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559)。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>V1.12.6-157</td>
<td>V1.12.7-180</td>
<td>更新为支持 Kubernetes 1.12.7 和 Calico 3.3.6 发行版。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>V1.12.6</td>
<td>V1.12.7</td>
<td>请参阅 [Kubernetes 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.7)。</td>
</tr>
<tr>
<td>可信计算代理程序</td>
<td>a02f765</td>
<td>e132aa4</td>
<td>针对 [CVE-2017-12447 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-12447)，更新了映像。</td>
</tr>
<tr>
<td>Ubuntu 16.04 内核</td>
<td>4.4.0-143-generic</td>
<td>4.4.0-145-generic</td>
<td>通过针对 [CVE-2019-9213 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html) 的内核更新，更新了工作程序节点映像。</td>
</tr>
<tr>
<td>Ubuntu 18.04 内核</td>
<td>4.15.0-46-generic</td>
<td>4.15.0-47-generic</td>
<td>通过针对 [CVE-2019-9213 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html) 的内核更新，更新了工作程序节点映像。</td>
</tr>
</tbody>
</table>

### 2019 年 4 月 1 日发布的工作程序节点 FP1.12.6_1547 的更改日志
{: #1126_1547}

下表显示了工作程序节点 FP1.12.6_1547 中包含的更改。
{: shortdesc}

<table summary="自 V1.12.6_1546 以来进行的更改">
<caption>自 V1.12.6_1546 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>工作程序节点资源利用率</td>
<td>不适用</td>
<td>不适用</td>
<td>增大了针对 kubelet 和 containerd 的内存保留量，以防止这些组件耗尽资源。有关更多信息，请参阅[工作程序节点资源保留](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node)。</td>
</tr>
</tbody>
</table>


### 2019 年 3 月 26 日发布的主节点 FP1.12.6_1546 的更改日志
{: #1126_1546}

下表显示了主节点 FP1.12.6_1546 中包含的更改。
{: shortdesc}

<table summary="自 V1.12.6_1544 以来进行的更改">
<caption>自 V1.12.6_1544 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 插件</td>
<td>345</td>
<td>346</td>
<td>针对 [CVE-2019-9741 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741)，更新了映像。</td>
</tr>
<tr>
<td>密钥管理服务提供程序</td>
<td>166</td>
<td>167</td>
<td>修复了针对管理 Kubernetes 私钥，间歇性发生的`超过了上下文截止期限`和`超时`错误。此外，还修复了对密钥管理服务的更新，原先更新可能会使现有 Kubernetes 私钥处于未加密状态。更新包含针对 [CVE-2019-9741 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741) 的修订。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider 的负载均衡器和负载均衡器监视器</td>
<td>143</td>
<td>146</td>
<td>针对 [CVE-2019-9741 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741)，更新了映像。</td>
</tr>
</tbody>
</table>

### 2019 年 3 月 20 日发布的 1.12.6_1544 的更改日志
{: #1126_1544}

下表显示了补丁 1.12.6_1544 中包含的更改。
{: shortdesc}

<table summary="自 V1.12.6_1541 以来进行的更改">
<caption>自 V1.12.6_1541 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>集群 DNS 配置</td>
<td>不适用</td>
<td>不适用</td>
<td>修复了必须缩减未使用的集群 DNS 时，可能导致集群主节点操作（例如，`refresh` 或 `update`）失败的错误。</td>
</tr>
<tr>
<td>集群主节点 HA 代理配置</td>
<td>不适用</td>
<td>不适用</td>
<td>更新了配置，以更好地处理集群主节点的间歇性连接失败。</td>
</tr>
<tr>
<td>CoreDNS 配置</td>
<td>不适用</td>
<td>不适用</td>
<td>CoreDNS 配置更新为支持[多个 Corefile ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://coredns.io/2017/07/23/corefile-explained/)。</td>
</tr>
<tr>
<td>GPU 设备插件和安装程序</td>
<td>e32d51c</td>
<td>9ff3fda</td>
<td>GPU 驱动程序更新为 [418.43 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.nvidia.com/object/unix.html)。更新包含针对 [CVE-2019-9741 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9741.html) 的修订。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 插件</td>
<td>344</td>
<td>345</td>
<td>添加了对[专用服务端点](/docs/containers?topic=containers-cs_network_cluster#set-up-private-se)的支持。</td>
</tr>
<tr>
<td>内核</td>
<td>4.4.0-141</td>
<td>4.4.0-143</td>
<td>通过 [CVE-2019-6133 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-6133.html) 的内核更新，更新了工作程序节点映像。</td>
</tr>
<tr>
<td>密钥管理服务提供程序</td>
<td>136</td>
<td>166</td>
<td>针对 [CVE-2018-16890 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16890)、[CVE-2019-3822 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3822) 和 [CVE-2019-3823 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3823)，更新了映像。</td>
</tr>
<tr>
<td>可信计算代理程序</td>
<td>5f3d092</td>
<td>a02f765</td>
<td>针对 [CVE-2018-10779 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10779)、[CVE-2018-12900 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-12900)、[CVE-2018-17000 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-17000)、[CVE-2018-19210 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-19210)、[CVE-2019-6128 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6128) 和 [CVE-2019-7663 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-7663)，更新了映像。</td>
</tr>
</tbody>
</table>

### 2019 年 3 月 4 日发布的 1.12.6_1541 的更改日志
{: #1126_1541}

下表显示了补丁 1.12.6_1541 中包含的更改。
{: shortdesc}

<table summary="自 V1.12.5_1540 以来进行的更改">
<caption>自 V1.12.5_1540 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>集群 DNS 提供程序</td>
<td>不适用</td>
<td>不适用</td>
<td>将 Kubernetes DNS 和 CoreDNS pod 内存限制从 `170Mi` 增加到了 `400Mi`，以便处理更多集群服务。</td>
</tr>
<tr>
<td>GPU 设备插件和安装程序</td>
<td>eb3a259</td>
<td>e32d51c</td>
<td>针对 [CVE-2019-6454 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454)，更新了映像。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>V1.12.5-137</td>
<td>V1.12.6-157</td>
<td>更新为支持 Kubernetes 1.12.6 发行版。修复了 V1.0 负载均衡器将 `externalTrafficPolicy` 设置为 `local` 时的定期连接问题。更新了 LoadBalancer V1.0 和 V2.0 事件，以使用最新的 {{site.data.keyword.Bluemix_notm}} 文档链接。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 插件</td>
<td>342</td>
<td>344</td>
<td>将映像的基本操作系统从 Fedora 更改为 Alpine。针对 [CVE-2019-6486 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486)，更新了映像。</td>
</tr>
<tr>
<td>密钥管理服务提供程序</td>
<td>122</td>
<td>136</td>
<td>将客户机超时增加到 {{site.data.keyword.keymanagementservicefull_notm}}。针对 [CVE-2019-6486 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486)，更新了映像。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>V1.12.5</td>
<td>V1.12.6</td>
<td>请参阅 [Kubernetes 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.6)。更新解决了 [CVE-2019-6486 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) 和 [CVE-2019-1002100 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1002100)。</td>
</tr>
<tr>
<td>Kubernetes 配置</td>
<td>不适用</td>
<td>不适用</td>
<td>向 `--feature-gates` 选项添加了 `ExperimentalCriticalPodAnnotation=true`。此设置可帮助将 pod 从不推荐使用的 `scheduler.alpha.kubernetes.io/critical-pod` 注释迁移到 [Kubernetes pod 优先级支持](/docs/containers?topic=containers-pod_priority#pod_priority)。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider 的负载均衡器和负载均衡器监视器</td>
<td>132</td>
<td>143</td>
<td>针对 [CVE-2019-6486 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486)，更新了映像。</td>
</tr>
<tr>
<td>OpenVPN 客户机和服务器</td>
<td>2.4.6-r3-IKS-13</td>
<td>2.4.6-r3-IKS-25</td>
<td>针对 [CVE-2019-1559 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559)，更新了映像。</td>
</tr>
<tr>
<td>可信计算代理程序</td>
<td>1ea5ad3</td>
<td>5f3d092</td>
<td>针对 [CVE-2019-6454 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454)，更新了映像。</td>
</tr>
</tbody>
</table>

### 2019 年 2 月 27 日发布的工作程序节点 FP1.12.5_1540 的更改日志
{: #1125_1540}

下表显示了工作程序节点 FP1.12.5_1540 中包含的更改。
{: shortdesc}

<table summary="自 V1.12.5_1538 以来进行的更改">
<caption>自 V1.12.5_1538 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>内核</td>
<td>4.4.0-141</td>
<td>4.4.0-142</td>
<td>通过 [CVE-2018-19407 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-142.168/changelog) 的内核更新，更新了工作程序节点映像。</td>
</tr>
</tbody>
</table>

### 2019 年 2 月 15 日发布的工作程序节点 FP1.12.5_1538 的更改日志
{: #1125_1538}

下表显示了工作程序节点 FP1.12.5_1538 中包含的更改。
{: shortdesc}

<table summary="自 V1.12.5_1537 以来进行的更改">
<caption>自 V1.12.5_1537 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>集群主节点 HA 代理配置</td>
<td>不适用</td>
<td>不适用</td>
<td>将 pod 配置 `spec.priorityClassName` 值更改为 `system-node-critical`，并将 `spec.priority` 值设置为 `2000001000`。</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.5</td>
<td>1.1.6</td>
<td>请参阅 [containerd 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/containerd/containerd/releases/tag/v1.1.6)。更新解决了 [CVE-2019-5736 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5736)。</td>
</tr>
<tr>
<td>Kubernetes `kubelet` 配置</td>
<td>不适用</td>
<td>不适用</td>
<td>启用了 `ExperimentalCriticalPodAnnotation` 功能检测点，以防止关键静态 pod 逐出。</td>
</tr>
</tbody>
</table>

### 2019 年 2 月 5 日发布的 1.12.5_1537 的更改日志
{: #1125_1537}

下表显示了补丁 1.12.5_1537 中包含的更改。
{: shortdesc}

<table summary="自 V1.12.4_1535 以来进行的更改">
<caption>自 V1.12.4_1535 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>etcd</td>
<td>V3.3.1</td>
<td>V3.3.11</td>
<td>请参阅 [etcd 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/coreos/etcd/releases/v3.3.11)。此外，支持用于 etcd 的密码套件现在仅限于具有高强度加密的子集（128 位或更多位）。</td>
</tr>
<tr>
<td>GPU 设备插件和安装程序</td>
<td>13fdc0d</td>
<td>eb3a259</td>
<td>针对 [CVE-2019-3462 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) 和 [CVE-2019-6486 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486)，更新了映像。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>V1.12.4-118</td>
<td>V1.12.5-137</td>
<td>更新为支持 Kubernetes 1.12.5 发行版。此外，`calicoctl` 版本更新为 3.3.1。修复了在发生工作程序节点状态更改时，对 V2.0 负载均衡器的不必要的配置更新。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 插件</td>
<td>338</td>
<td>342</td>
<td>File Storage 插件按如下所示进行更新：<ul><li>支持使用[卷拓扑感知安排](/docs/containers?topic=containers-file_storage#file-topology)进行动态供应。</li>
<li>忽略存储器已删除时的持久卷声明 (PVC) 删除错误。</li>
<li>向失败的 PVC 添加了失败消息注释。</li>
<li>优化了存储供应程序控制器的领导者选举和再同步时间段设置，并将供应超时从 30 分钟增加到 1 小时。</li>
<li>在开始供应之前检查用户许可权。</li></ul></td>
</tr>
<tr>
<td>密钥管理服务提供程序</td>
<td>111</td>
<td>122</td>
<td>添加了重试逻辑，以避免在 {{site.data.keyword.keymanagementservicefull_notm}} 管理 Kubernetes 私钥时发生临时故障。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>V1.12.4</td>
<td>V1.12.5</td>
<td>请参阅 [Kubernetes 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.5)。</td>
</tr>
<tr>
<td>Kubernetes 配置</td>
<td>不适用</td>
<td>不适用</td>
<td>Kubernetes API 服务器审计策略配置更新为包含 `cluster-admin` 请求的日志记录元数据，并记录工作负载 `create`、`update` 和 `patch` 请求的请求主体。</td>
</tr>
<tr>
<td>OpenVPN 客户机</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>针对 [CVE-2018-0734 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) 和 [CVE-2018-5407 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407)，更新了映像。此外，现在 pod 配置将从私钥获取，而不是从配置映射获取。</td>
</tr>
<tr>
<td>OpenVPN 服务器</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>针对 [CVE-2018-0734 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) 和 [CVE-2018-5407 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407)，更新了映像。</td>
</tr>
<tr>
<td>systemd</td>
<td>230</td>
<td>229</td>
<td>[CVE-2018-16864 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16864) 的安全补丁。</td>
</tr>
</tbody>
</table>

### 2019 年 1 月 28 日发布的工作程序节点 FP1.12.4_1535 的更改日志
{: #1124_1535}

下表显示了工作程序节点 FP1.12.4_1535 中包含的更改。
{: shortdesc}

<table summary="自 V1.12.4_1534 以来进行的更改">
<caption>自 V1.12.4_1534 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 软件包</td>
<td>不适用</td>
<td>不适用</td>
<td>更新了安装的 Ubuntu 软件包，包括针对 [CVE-2019-3462 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) 和 [USN-3863-1 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://usn.ubuntu.com/3863-1)，更新了 `apt`。</td>
</tr>
</tbody>
</table>


### 2019 年 1 月 21 日发布的 1.12.4_1534 的更改日志
{: #1124_1534}

下表显示了补丁 1.12.3_1534 中包含的更改。
{: shortdesc}

<table summary="自 V1.12.3_1533 以来进行的更改">
<caption>自 V1.12.3_1533 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>V1.12.3-91</td>
<td>V1.12.4-118</td>
<td>更新为支持 Kubernetes 1.12.4 发行版。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>V1.12.3</td>
<td>V1.12.4</td>
<td>请参阅 [Kubernetes 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.4)。</td>
</tr>
<tr>
<td>Kubernetes add-on resizer</td>
<td>1.8.1</td>
<td>1.8.4</td>
<td>请参阅 [Kubernetes add-on resizer 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/autoscaler/releases/tag/addon-resizer-1.8.4)。</td>
</tr>
<tr>
<td>Kubernetes 仪表板</td>
<td>V1.8.3</td>
<td>V1.10.1</td>
<td>请参阅 [Kubernetes 仪表板发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/dashboard/releases/tag/v1.10.1)。更新解决了 [CVE-2018-18264 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-18264)。</td>
</tr>
<tr>
<td>GPU 安装程序</td>
<td>390.12</td>
<td>410.79</td>
<td>将安装的 GPU 驱动程序更新为 410.79。</td>
</tr>
</tbody>
</table>

### 2019 年 1 月 7 日发布的工作程序节点 FP1.12.3_1533 的更改日志
{: #1123_1533}

下表显示了工作程序节点 FP1.12.3_1533 中包含的更改。
{: shortdesc}

<table summary="自 V1.12.3_1532 以来进行的更改">
<caption>自 V1.12.3_1532 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>内核</td>
<td>4.4.0-139</td>
<td>4.4.0-141</td>
<td>使用 [CVE-2017-5753 和 CVE-2018-18690 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-141.167/changelog) 的内核更新，更新了工作程序节点映像。</td>
</tr>
</tbody>
</table>

### 2018 年 12 月 17 日发布的工作程序节点 FP1.12.3_1532 的更改日志
{: #1123_1532}

下表显示了工作程序节点 FP1.12.2_1532 中包含的更改。
{: shortdesc}

<table summary="自 V1.12.3_1531 以来进行的更改">
<caption>自 V1.12.3_1531 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 软件包</td>
<td>不适用</td>
<td>不适用</td>
<td>已安装的 Ubuntu 软件包的更新。</td>
</tr>
</tbody>
</table>


### 2018 年 12 月 5 日发布的 1.12.3_1531 的更改日志
{: #1123_1531}

下表显示了补丁 1.12.3_1531 中包含的更改。
{: shortdesc}

<table summary="自 V1.12.2_1530 以来进行的更改">
<caption>自 V1.12.2_1530 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>V1.12.2-68</td>
<td>V1.12.3-91</td>
<td>更新为支持 Kubernetes 1.12.3 发行版。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>V1.12.2</td>
<td>V1.12.3</td>
<td>请参阅 [Kubernetes 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.3)。更新解决了 [CVE-2018-1002105 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/kubernetes/issues/71411)。</td>
</tr>
</tbody>
</table>

### 2018 年 12 月 4 日发布的工作程序节点 FP1.12.2_1530 的更改日志
{: #1122_1530}

下表显示了工作程序节点 FP1.12.2_1530 中包含的更改。
{: shortdesc}

<table summary="自 V1.12.2_1529 以来进行的更改">
<caption>自 V1.12.2_1529 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>工作程序节点资源利用率</td>
<td>不适用</td>
<td>不适用</td>
<td>添加了针对 kubelet 和 containerd 的专用 cgroup，以防止这些组件耗尽资源。有关更多信息，请参阅[工作程序节点资源保留](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node)。</td>
</tr>
</tbody>
</table>



### 2018 年 11 月 27 日发布的 1.12.2_1529 的更改日志
{: #1122_1529}

下表显示了补丁 1.12.2_1529 中包含的更改。
{: shortdesc}

<table summary="自 V1.12.2_1528 以来进行的更改">
<caption>自 V1.12.2_1528 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>V3.2.1</td>
<td>V3.3.1</td>
<td>请参阅 [Calico 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://docs.projectcalico.org/v3.3/releases/#v331)。更新解决了 [Tigera Technical Advisory TTA-2018-001 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.projectcalico.org/security-bulletins/)。</td>
</tr>
<tr>
<td>集群 DNS 配置</td>
<td>不适用</td>
<td>不适用</td>
<td>修复了可能导致 Kubernetes DNS 和 CoreDNS pod 在集群创建或更新操作之后同时运行的错误。</td>
</tr>
<tr>
<td>containerd</td>
<td>V1.2.0</td>
<td>V1.1.5</td>
<td>请参阅 [containerd 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/containerd/containerd/releases/tag/v1.1.5)。更新了 containerd，以修复会[阻止 pod 终止 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/containerd/containerd/issues/2744) 的死锁。</td>
</tr>
<tr>
<td>OpenVPN 客户机和服务器</td>
<td>2.4.4-r1-6</td>
<td>2.4.6-r3-IKS-8</td>
<td>针对 [CVE-2018-0732 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732) 和 [CVE-2018-0737 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737)，更新了映像。</td>
</tr>
</tbody>
</table>

### 2018 年 11 月 19 日发布的工作程序节点 FP1.12.2_1528 的更改日志
{: #1122_1528}

下表显示了工作程序节点 FP1.12.2_1528 中包含的更改。
{: shortdesc}

<table summary="自 V1.12.2_1527 以来进行的更改">
<caption>自 V1.12.2_1527 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>内核</td>
<td>4.4.0-137</td>
<td>4.4.0-139</td>
<td>通过 [CVE-2018-7755 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-139.165/changelog) 的内核更新，更新了工作程序节点映像。</td>
</tr>
</tbody>
</table>


### 2018 年 11 月 7 日发布的 1.12.2_1527 的更改日志
{: #1122_1527}

下表显示了补丁 1.12.2_1527 中包含的更改。
{: shortdesc}

<table summary="自 V1.11.3_1533 以来进行的更改">
<caption>自 V1.11.3_1533 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico 配置</td>
<td>不适用</td>
<td>不适用</td>
<td>现在，`kube-system` 名称空间中的 Calico `calco-*` pod 设置对所有容器的 CPU 和内存资源请求。</td>
</tr>
<tr>
<td>集群 DNS 提供程序</td>
<td>不适用</td>
<td>不适用</td>
<td>Kubernetes DNS (KubeDNS) 仍然是缺省集群 DNS 提供程序。但是，现在可以选择[将集群 DNS 提供程序更改为 CoreDNS](/docs/containers?topic=containers-cluster_dns#dns_set)。</td>
</tr>
<tr>
<td>集群度量值提供程序</td>
<td>不适用</td>
<td>不适用</td>
<td>Kubernetes Metrics Server 将替换作为集群度量值提供程序的 Kubernetes Heapster（自 Kubernetes V1.8 开始不推荐使用）。有关操作项，请参阅 [`metrics-server` 准备操作](/docs/containers?topic=containers-cs_versions#metrics-server)。</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.4</td>
<td>1.2.0</td>
<td>请参阅 [containerd 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/containerd/containerd/releases/tag/v1.2.0)。</td>
</tr>
<tr>
<td>CoreDNS</td>
<td>不适用</td>
<td>1.2.2</td>
<td>请参阅 [CoreDNS 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/coredns/coredns/releases/tag/v1.2.2)。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>V1.11.3</td>
<td>V1.12.2</td>
<td>请参阅 [Kubernetes 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.2)。</td>
</tr>
<tr>
<td>Kubernetes 配置</td>
<td>不适用</td>
<td>不适用</td>
<td>添加了三个新的 IBM pod 安全策略及其关联的集群角色。有关更多信息，请参阅[了解 IBM 集群管理的缺省资源](/docs/containers?topic=containers-psp#ibm_psp)。</td>
</tr>
<tr>
<td>Kubernetes 仪表板</td>
<td>V1.8.3</td>
<td>V1.10.0</td>
<td>请参阅 [Kubernetes 仪表板发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/dashboard/releases/tag/v1.10.0)。<br><br>
如果您通过 `kubectl proxy` 来访问仪表板，那么会除去登录页面上的**跳过**按钮。请改为[使用**令牌**进行登录](/docs/containers?topic=containers-app#cli_dashboard)。此外，现在可以通过运行 `kubectl -n kube-system scale deploy kubernetes-dashboard --replicas=3` 来缩放 Kubernetes 仪表板 pod 的数量。</td>
</tr>
<tr>
<td>Kubernetes DNS</td>
<td>1.14.10</td>
<td>1.14.13</td>
<td>请参阅 [Kubernetes DNS 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/dns/releases/tag/1.14.13)。</td>
</tr>
<tr>
<td>Kubernetes Metrics Server</td>
<td>不适用</td>
<td>V0.3.1</td>
<td>请参阅 [Kubernetes Metrics Server 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes-incubator/metrics-server/releases/tag/v0.3.1)。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>V1.11.3-118</td>
<td>V1.12.2-68</td>
<td>更新为支持 Kubernetes 1.12 发行版。其他更改包括以下内容：
<ul><li>现在，负载均衡器 pod（在 `ibm-system` 名称空间中为 `ibm-cloud-provider-ip-*`）可设置 CPU 和内存资源请求。</li>
<li>添加了 `service.kubernetes.io/ibm-load-balancer-cloud-provider-vlan` 注释，用于指定 LoadBalancer 服务部署到的 VLAN。要查看集群中的可用 VLAN，请运行 `ibmcloud ks vlans --zone <zone>`。</li>
<li>新的[负载均衡器 2.0](/docs/containers?topic=containers-loadbalancer#planning_ipvs) 作为 Beta 提供。</li></ul></td>
</tr>
<tr>
<td>OpenVPN 客户机配置</td>
<td>不适用</td>
<td>不适用</td>
<td>现在，`kube-system` 名称空间中的 OpenVPN 客户机 `vpn-* pod` 可设置 CPU 和内存资源请求。</td>
</tr>
</tbody>
</table>

## 不推荐：V1.11 更改日志
{: #111_changelog}

查看 V1.11 更改日志。
{: shortdesc}

Kubernetes V1.11 已不推荐使用，到 2019 年 6 月 27 日（暂定）即不再予以支持。对于每个 Kubernetes 版本更新，请[查看潜在影响](/docs/containers?topic=containers-cs_versions#cs_versions)，然后立即[更新集群](/docs/containers?topic=containers-update#update)，并且至少更新到 1.12。
{: deprecated}

### 2019 年 6 月 4 日发布的 1.11.10_1561 的更改日志
{: #11110_1561}

下表显示了补丁 1.11.10_1561 中包含的更改。
{: shortdesc}

<table summary="自 V1.11.10_1559 以来进行的更改">
<caption>自 V1.11.10_1559 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>集群主节点 HA 配置</td>
<td>不适用</td>
<td>不适用</td>
<td>更新了配置，以最大程度地减少在主节点更新期间发生的间歇性主节点网络连接失败。</td>
</tr>
<tr>
<td>etcd</td>
<td>V3.3.11</td>
<td>V3.3.13</td>
<td>请参阅 [etcd 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/coreos/etcd/releases/v3.3.13)。</td>
</tr>
<tr>
<td>GPU 设备插件和安装程序</td>
<td>55c1f66</td>
<td>32257d3</td>
<td>针对 [CVE-2018-10844 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844)、[CVE-2018-10845 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845)、[CVE-2018-10846 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846)、[CVE-2019-3829 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829)、[CVE-2019-3836 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836)、[CVE-2019-9893 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893)、[CVE-2019-5435 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435) 和 [CVE-2019-5436 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436)，更新了映像。</td>
</tr>
<tr>
<td>可信计算代理程序</td>
<td>13c7ef0</td>
<td>e8c6d72</td>
<td>针对 [CVE-2018-10844 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844)、[CVE-2018-10845 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845)、[CVE-2018-10846 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846)、[CVE-2019-3829 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829)、[CVE-2019-3836 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836)、[CVE-2019-9893 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893)、[CVE-2019-5435 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435) 和 [CVE-2019-5436 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436)，更新了映像。</td>
</tr>
</tbody>
</table>

### 2019 年 5 月 20 日发布的工作程序节点 FP1.11.10_1559 的更改日志
{: #11110_1559}

下表显示了补丁包 1.11.10_1559 中包含的更改。
{: shortdesc}

<table summary="自 V1.11.10_1558 以来进行的更改">
<caption>自 V1.11.10_1558 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 16.04 内核</td>
<td>4.4.0-145-generic</td>
<td>4.4.0-148-generic</td>
<td>使用 [CVE-2018-12126 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html)、[CVE-2018-12127 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html) 和 [CVE-2018-12130 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html) 的内核更新对工作程序节点映像进行了更新。</td>
</tr>
<tr>
<td>Ubuntu 18.04 内核</td>
<td>4.15.0-47-generic</td>
<td>4.15.0-50-generic</td>
<td>使用 [CVE-2018-12126 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html)、[CVE-2018-12127 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html) 和 [CVE-2018-12130 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html) 的内核更新对工作程序节点映像进行了更新。</td>
</tr>
</tbody>
</table>

### 2019 年 5 月 13 日发布的 1.11.10_1558 的更改日志
{: #11110_1558}

下表显示了补丁 1.11.10_1558 中包含的更改。
{: shortdesc}

<table summary="自 V1.11.9_1556 以来进行的更改">
<caption>自 V1.11.9_1556 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>集群主节点 HA 代理</td>
<td>1.9.6-alpine</td>
<td>1.9.7-alpine</td>
<td>请参阅 [HAProxy 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.haproxy.org/download/1.9/src/CHANGELOG)。更新解决了 [CVE-2019-6706 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6706)。</td>
</tr>
<tr>
<td>GPU 设备插件和安装程序</td>
<td>9ff3fda</td>
<td>55c1f66</td>
<td>针对 [CVE-2019-1543 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543)，更新了映像。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>V1.11.9-241</td>
<td>V1.11.10-270</td>
<td>更新为支持 Kubernetes 1.11.10 发行版。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>V1.11.9</td>
<td>V1.11.10</td>
<td>请参阅 [Kubernetes 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.10)。</td>
</tr>
<tr>
<td>Kubernetes 配置</td>
<td>不适用</td>
<td>不适用</td>
<td>Kubernetes API 服务器审计策略配置更新为不记录 `/openapi/v2*` 只读 URL。此外，Kubernetes 控制器管理器配置将签署的 `kubelet` 证书的有效期从 1 年延长到 3 年。</td>
</tr>
<tr>
<td>OpenVPN 客户机配置</td>
<td>不适用</td>
<td>不适用</td>
<td>现在，`kube-system` 名称空间中的 OpenVPN 客户机 `vpn-*` pod 将 `dnsPolicy` 设置为 `Default`，以避免在集群 DNS 停止运行时 pod 发生故障。</td>
</tr>
<tr>
<td>可信计算代理程序</td>
<td>e132aa4</td>
<td>13c7ef0</td>
<td>针对 [CVE-2016-7076 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-7076)、[CVE-2017-1000368 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1000368) 和 [CVE-2019-11068 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-11068)，更新了映像。</td>
</tr>
</tbody>
</table>

### 2019 年 4 月 29 日发布的工作程序节点 FP1.11.9_1556 的更改日志
{: #1119_1556}

下表显示了工作程序节点 FP1.11.9_1556 中包含的更改。
{: shortdesc}

<table summary="自 V1.11.9_1555 以来进行的更改">
<caption>自 V1.11.9_1555 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 软件包</td>
<td>不适用</td>
<td>不适用</td>
<td>已安装的 Ubuntu 软件包的更新。</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.6</td>
<td>1.1.7</td>
<td>请参阅 [containerd 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/containerd/containerd/releases/tag/v1.1.7)。</td>
</tr>
</tbody>
</table>


### 2019 年 4 月 15 日发布的工作程序节点 FP1.11.9_1555 的更改日志
{: #1119_1555}

下表显示了工作程序节点 FP1.11.9_1555 中包含的更改。
{: shortdesc}

<table summary="自 V1.11.9_1554 以来进行的更改">
<caption>自 V1.11.9_1554 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 软件包</td>
<td>不适用</td>
<td>不适用</td>
<td>更新了安装的 Ubuntu 软件包，包括针对 [CVE-2019-3842 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-3842.html)，更新了 `systemd`。</td>
</tr>
</tbody>
</table>

### 2019 年 4 月 8 日发布的 1.11.9_1554 的更改日志
{: #1119_1554}

下表显示了补丁 1.11.9_1554 中包含的更改。
{: shortdesc}

<table summary="自 V1.11.8_1553 以来进行的更改">
<caption>自 V1.11.8_1553 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>V3.3.1</td>
<td>V3.3.6</td>
<td>请参阅 [Calico 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://docs.projectcalico.org/v3.3/releases/#v336)。更新解决了 [CVE-2019-9946 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9946)。</td>
</tr>
<tr>
<td>集群主节点 HA 代理</td>
<td>1.8.12-alpine</td>
<td>1.9.6-alpine</td>
<td>请参阅 [HAProxy 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.haproxy.org/download/1.9/src/CHANGELOG)。更新解决了 [CVE-2018-0732 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732)、[CVE-2018-0734 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734)、[CVE-2018-0737 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737)、[CVE-2018-5407 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407)、[CVE-2019-1543 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543) 和 [CVE-2019-1559 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559)。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>V1.11.8-219</td>
<td>V1.11.9-241</td>
<td>更新为支持 Kubernetes 1.11.9 和 Calico 3.3.6 发行版。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>V1.11.8</td>
<td>V1.11.9</td>
<td>请参阅 [Kubernetes 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.9)。</td>
</tr>
<tr>
<td>Kubernetes DNS</td>
<td>1.14.10</td>
<td>1.14.13</td>
<td>请参阅 [Kubernetes DNS 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/dns/releases/tag/1.14.13)。</td>
</tr>
<tr>
<td>可信计算代理程序</td>
<td>a02f765</td>
<td>e132aa4</td>
<td>针对 [CVE-2017-12447 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-12447)，更新了映像。</td>
</tr>
<tr>
<td>Ubuntu 16.04 内核</td>
<td>4.4.0-143-generic</td>
<td>4.4.0-145-generic</td>
<td>通过针对 [CVE-2019-9213 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html) 的内核更新，更新了工作程序节点映像。</td>
</tr>
<tr>
<td>Ubuntu 18.04 内核</td>
<td>4.15.0-46-generic</td>
<td>4.15.0-47-generic</td>
<td>通过针对 [CVE-2019-9213 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html) 的内核更新，更新了工作程序节点映像。</td>
</tr>
</tbody>
</table>

### 2019 年 4 月 1 日发布的工作程序节点 FP1.11.8_1553 的更改日志
{: #1118_1553}

下表显示了工作程序节点 FP1.11.8_1553 中包含的更改。
{: shortdesc}

<table summary="自 V1.11.8_1552 以来进行的更改">
<caption>自 V1.11.8_1552 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>工作程序节点资源利用率</td>
<td>不适用</td>
<td>不适用</td>
<td>增大了针对 kubelet 和 containerd 的内存保留量，以防止这些组件耗尽资源。有关更多信息，请参阅[工作程序节点资源保留](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node)。</td>
</tr>
</tbody>
</table>

### 2019 年 3 月 26 日发布的主节点 FP1.11.8_1552 的更改日志
{: #1118_1552}

下表显示了主节点 FP1.11.8_1552 中包含的更改。
{: shortdesc}

<table summary="自 V1.11.8_1550 以来进行的更改">
<caption>自 V1.11.8_1550 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 插件</td>
<td>345</td>
<td>346</td>
<td>针对 [CVE-2019-9741 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741)，更新了映像。</td>
</tr>
<tr>
<td>密钥管理服务提供程序</td>
<td>166</td>
<td>167</td>
<td>修复了针对管理 Kubernetes 私钥，间歇性发生的`超过了上下文截止期限`和`超时`错误。此外，还修复了对密钥管理服务的更新，原先更新可能会使现有 Kubernetes 私钥处于未加密状态。更新包含针对 [CVE-2019-9741 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741) 的修订。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider 的负载均衡器和负载均衡器监视器</td>
<td>143</td>
<td>146</td>
<td>针对 [CVE-2019-9741 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741)，更新了映像。</td>
</tr>
</tbody>
</table>

### 2019 年 3 月 20 日发布的 1.11.8_1550 的更改日志
{: #1118_1550}

下表显示了补丁 1.11.8_1550 中包含的更改。
{: shortdesc}

<table summary="自 V1.11.8_1547 以来进行的更改">
<caption>自 V1.11.8_1547 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>集群主节点 HA 代理配置</td>
<td>不适用</td>
<td>不适用</td>
<td>更新了配置，以更好地处理集群主节点的间歇性连接失败。</td>
</tr>
<tr>
<td>GPU 设备插件和安装程序</td>
<td>e32d51c</td>
<td>9ff3fda</td>
<td>GPU 驱动程序更新为 [418.43 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.nvidia.com/object/unix.html)。更新包含针对 [CVE-2019-9741 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9741.html) 的修订。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 插件</td>
<td>344</td>
<td>345</td>
<td>添加了对[专用服务端点](/docs/containers?topic=containers-cs_network_cluster#set-up-private-se)的支持。</td>
</tr>
<tr>
<td>内核</td>
<td>4.4.0-141</td>
<td>4.4.0-143</td>
<td>通过 [CVE-2019-6133 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-6133.html) 的内核更新，更新了工作程序节点映像。</td>
</tr>
<tr>
<td>密钥管理服务提供程序</td>
<td>136</td>
<td>166</td>
<td>针对 [CVE-2018-16890 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16890)、[CVE-2019-3822 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3822) 和 [CVE-2019-3823 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3823)，更新了映像。</td>
</tr>
<tr>
<td>可信计算代理程序</td>
<td>5f3d092</td>
<td>a02f765</td>
<td>针对 [CVE-2018-10779 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10779)、[CVE-2018-12900 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-12900)、[CVE-2018-17000 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-17000)、[CVE-2018-19210 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-19210)、[CVE-2019-6128 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6128) 和 [CVE-2019-7663 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-7663)，更新了映像。</td>
</tr>
</tbody>
</table>

### 2019 年 3 月 4 日发布的 1.11.8_1547 的更改日志
{: #1118_1547}

下表显示了补丁 1.11.8_1547 中包含的更改。
{: shortdesc}

<table summary="自 V1.11.7_1546 以来进行的更改">
<caption>自 V1.11.7_1546 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>GPU 设备插件和安装程序</td>
<td>eb3a259</td>
<td>e32d51c</td>
<td>针对 [CVE-2019-6454 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454)，更新了映像。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>V1.11.7-198</td>
<td>V1.11.8-219</td>
<td>更新为支持 Kubernetes 1.11.8 发行版。修复了负载均衡器将 `externalTrafficPolicy` 设置为 `local` 时的定期连接问题。更新了负载均衡器事件，以使用最新的 {{site.data.keyword.Bluemix_notm}} 文档链接。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 插件</td>
<td>342</td>
<td>344</td>
<td>将映像的基本操作系统从 Fedora 更改为 Alpine。针对 [CVE-2019-6486 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486)，更新了映像。</td>
</tr>
<tr>
<td>密钥管理服务提供程序</td>
<td>122</td>
<td>136</td>
<td>将客户机超时增加到 {{site.data.keyword.keymanagementservicefull_notm}}。针对 [CVE-2019-6486 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486)，更新了映像。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>V1.11.7</td>
<td>V1.11.8</td>
<td>请参阅 [Kubernetes 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.8)。更新解决了 [CVE-2019-6486 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) 和 [CVE-2019-1002100 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1002100)。</td>
</tr>
<tr>
<td>Kubernetes 配置</td>
<td>不适用</td>
<td>不适用</td>
<td>向 `--feature-gates` 选项添加了 `ExperimentalCriticalPodAnnotation=true`。此设置可帮助将 pod 从不推荐使用的 `scheduler.alpha.kubernetes.io/critical-pod` 注释迁移到 [Kubernetes pod 优先级支持](/docs/containers?topic=containers-pod_priority#pod_priority)。</td>
</tr>
<tr>
<td>Kubernetes DNS</td>
<td>不适用</td>
<td>不适用</td>
<td>将 Kubernetes DNS pod 内存限制从 `170Mi` 增加到了 `400Mi`，以便处理更多集群服务。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider 的负载均衡器和负载均衡器监视器</td>
<td>132</td>
<td>143</td>
<td>针对 [CVE-2019-6486 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486)，更新了映像。</td>
</tr>
<tr>
<td>OpenVPN 客户机和服务器</td>
<td>2.4.6-r3-IKS-13</td>
<td>2.4.6-r3-IKS-25</td>
<td>针对 [CVE-2019-1559 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559)，更新了映像。</td>
</tr>
<tr>
<td>可信计算代理程序</td>
<td>1ea5ad3</td>
<td>5f3d092</td>
<td>针对 [CVE-2019-6454 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454)，更新了映像。</td>
</tr>
</tbody>
</table>

### 2019 年 2 月 27 日发布的工作程序节点 FP1.11.7_1546 的更改日志
{: #1117_1546}

下表显示了工作程序节点 FP1.11.7_1546 中包含的更改。
{: shortdesc}

<table summary="自 V1.11.7_1546 以来进行的更改">
<caption>自 V1.11.7_1546 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>内核</td>
<td>4.4.0-141</td>
<td>4.4.0-142</td>
<td>通过 [CVE-2018-19407 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-142.168/changelog) 的内核更新，更新了工作程序节点映像。</td>
</tr>
</tbody>
</table>

### 2019 年 2 月 15 日发布的工作程序节点 FP1.11.7_1544 的更改日志
{: #1117_1544}

下表显示了工作程序节点 FP1.11.7_1544 中包含的更改。
{: shortdesc}

<table summary="自 V1.11.7_1543 以来进行的更改">
<caption>自 V1.11.7_1543 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>集群主节点 HA 代理配置</td>
<td>不适用</td>
<td>不适用</td>
<td>将 pod 配置 `spec.priorityClassName` 值更改为 `system-node-critical`，并将 `spec.priority` 值设置为 `2000001000`。</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.5</td>
<td>1.1.6</td>
<td>请参阅 [containerd 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/containerd/containerd/releases/tag/v1.1.6)。更新解决了 [CVE-2019-5736 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5736)。</td>
</tr>
<tr>
<td>Kubernetes `kubelet` 配置</td>
<td>不适用</td>
<td>不适用</td>
<td>启用了 `ExperimentalCriticalPodAnnotation` 功能检测点，以防止关键静态 pod 逐出。</td>
</tr>
</tbody>
</table>

### 2019 年 2 月 5 日发布的 1.11.7_1543 的更改日志
{: #1117_1543}

下表显示了补丁 1.11.7_1543 中包含的更改。
{: shortdesc}

<table summary="自 V1.11.6_1541 以来进行的更改">
<caption>自 V1.11.6_1541 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>etcd</td>
<td>V3.3.1</td>
<td>V3.3.11</td>
<td>请参阅 [etcd 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/coreos/etcd/releases/v3.3.11)。此外，支持用于 etcd 的密码套件现在仅限于具有高强度加密的子集（128 位或更多位）。</td>
</tr>
<tr>
<td>GPU 设备插件和安装程序</td>
<td>13fdc0d</td>
<td>eb3a259</td>
<td>针对 [CVE-2019-3462 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) 和 [CVE-2019-6486 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486)，更新了映像。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>V1.11.6-180</td>
<td>V1.11.7-198</td>
<td>更新为支持 Kubernetes 1.11.7 发行版。此外，`calicoctl` 版本更新为 3.3.1。修复了在发生工作程序节点状态更改时，对 V2.0 负载均衡器的不必要的配置更新。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 插件</td>
<td>338</td>
<td>342</td>
<td>File Storage 插件按如下所示进行更新：<ul><li>支持使用[卷拓扑感知安排](/docs/containers?topic=containers-file_storage#file-topology)进行动态供应。</li>
<li>忽略存储器已删除时的持久卷声明 (PVC) 删除错误。</li>
<li>向失败的 PVC 添加了失败消息注释。</li>
<li>优化了存储供应程序控制器的领导者选举和再同步时间段设置，并将供应超时从 30 分钟增加到 1 小时。</li>
<li>在开始供应之前检查用户许可权。</li></ul></td>
</tr>
<tr>
<td>密钥管理服务提供程序</td>
<td>111</td>
<td>122</td>
<td>添加了重试逻辑，以避免在 {{site.data.keyword.keymanagementservicefull_notm}} 管理 Kubernetes 私钥时发生临时故障。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>V1.11.6</td>
<td>V1.11.7</td>
<td>请参阅 [Kubernetes 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.7)。</td>
</tr>
<tr>
<td>Kubernetes 配置</td>
<td>不适用</td>
<td>不适用</td>
<td>Kubernetes API 服务器审计策略配置更新为包含 `cluster-admin` 请求的日志记录元数据，并记录工作负载 `create`、`update` 和 `patch` 请求的请求主体。</td>
</tr>
<tr>
<td>OpenVPN 客户机</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>针对 [CVE-2018-0734 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) 和 [CVE-2018-5407 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407)，更新了映像。此外，现在 pod 配置将从私钥获取，而不是从配置映射获取。</td>
</tr>
<tr>
<td>OpenVPN 服务器</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>针对 [CVE-2018-0734 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) 和 [CVE-2018-5407 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407)，更新了映像。</td>
</tr>
<tr>
<td>systemd</td>
<td>230</td>
<td>229</td>
<td>[CVE-2018-16864 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16864) 的安全补丁。</td>
</tr>
</tbody>
</table>

### 2019 年 1 月 28 日发布的工作程序节点 FP1.11.6_1541 的更改日志
{: #1116_1541}

下表显示了工作程序节点 FP1.11.6_1541 中包含的更改。
{: shortdesc}

<table summary="自 V1.11.6_1540 以来进行的更改">
<caption>自 V1.11.6_1540 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 软件包</td>
<td>不适用</td>
<td>不适用</td>
<td>更新了安装的 Ubuntu 软件包，包括针对 [CVE-2019-3462 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462)/[USN-3863-1 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://usn.ubuntu.com/3863-1)，更新了 `apt`。</td>
</tr>
</tbody>
</table>

### 2019 年 1 月 21 日发布的 1.11.6_1540 的更改日志
{: #1116_1540}

下表显示了补丁 1.11.6_1540 中包含的更改。
{: shortdesc}

<table summary="自 V1.11.5_1539 以来进行的更改">
<caption>自 V1.11.5_1539 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>V1.11.5-152</td>
<td>V1.11.6-180</td>
<td>更新为支持 Kubernetes 1.11.6 发行版。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>V1.11.5</td>
<td>V1.11.6</td>
<td>请参阅 [Kubernetes 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.6)。</td>
</tr>
<tr>
<td>Kubernetes add-on resizer</td>
<td>1.8.1</td>
<td>1.8.4</td>
<td>请参阅 [Kubernetes add-on resizer 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/autoscaler/releases/tag/addon-resizer-1.8.4)。</td>
</tr>
<tr>
<td>Kubernetes 仪表板</td>
<td>V1.8.3</td>
<td>V1.10.1</td>
<td>请参阅 [Kubernetes 仪表板发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/dashboard/releases/tag/v1.10.1)。更新解决了 [CVE-2018-18264 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-18264)。<br><br>如果您通过 `kubectl proxy` 来访问仪表板，那么会除去登录页面上的**跳过**按钮。请改为[使用**令牌**进行登录](/docs/containers?topic=containers-app#cli_dashboard)。</td>
</tr>
<tr>
<td>GPU 安装程序</td>
<td>390.12</td>
<td>410.79</td>
<td>将安装的 GPU 驱动程序更新为 410.79。</td>
</tr>
</tbody>
</table>

### 2019 年 1 月 7 日发布的工作程序节点 FP1.11.5_1539 的更改日志
{: #1115_1539}

下表显示了工作程序节点 FP1.11.5_1539 中包含的更改。
{: shortdesc}

<table summary="自 V1.11.5_1538 以来进行的更改">
<caption>自 V1.11.5_1538 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>内核</td>
<td>4.4.0-139</td>
<td>4.4.0-141</td>
<td>使用 [CVE-2017-5753 和 CVE-2018-18690 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-141.167/changelog) 的内核更新，更新了工作程序节点映像。</td>
</tr>
</tbody>
</table>

### 2018 年 12 月 17 日发布的工作程序节点 FP1.11.5_1538 的更改日志
{: #1115_1538}

下表显示了工作程序节点 FP1.11.5_1538 中包含的更改。
{: shortdesc}

<table summary="自 V1.11.5_1537 以来进行的更改">
<caption>自 V1.11.5_1537 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 软件包</td>
<td>不适用</td>
<td>不适用</td>
<td>已安装的 Ubuntu 软件包的更新。</td>
</tr>
</tbody>
</table>

### 2018 年 12 月 5 日发布的 1.11.5_1537 的更改日志
{: #1115_1537}

下表显示了补丁 1.11.5_1537 中包含的更改。
{: shortdesc}

<table summary="自 V1.11.4_1536 以来进行的更改">
<caption>自 V1.11.4_1536 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>V1.11.4-142</td>
<td>V1.11.5-152</td>
<td>更新为支持 Kubernetes 1.11.5 发行版。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>V1.11.4</td>
<td>V1.11.5</td>
<td>请参阅 [Kubernetes 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.5)。更新解决了 [CVE-2018-1002105 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/kubernetes/issues/71411)。</td>
</tr>
</tbody>
</table>

### 2018 年 12 月 4 日发布的工作程序节点 FP1.11.4_1536 的更改日志
{: #1114_1536}

下表显示了工作程序节点 FP1.11.4_1536 中包含的更改。
{: shortdesc}

<table summary="自 V1.11.4_1535 以来进行的更改">
<caption>自 V1.11.4_1535 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>工作程序节点资源利用率</td>
<td>不适用</td>
<td>不适用</td>
<td>添加了针对 kubelet 和 containerd 的专用 cgroup，以防止这些组件耗尽资源。有关更多信息，请参阅[工作程序节点资源保留](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node)。</td>
</tr>
</tbody>
</table>

### 2018 年 11 月 27 日发布的 1.11.4_1535 的更改日志
{: #1114_1535}

下表显示了补丁 1.11.4_1535 中包含的更改。
{: shortdesc}

<table summary="自 V1.11.3_1534 以来进行的更改">
<caption>自 V1.11.3_1534 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>V3.2.1</td>
<td>V3.3.1</td>
<td>请参阅 [Calico 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://docs.projectcalico.org/v3.3/releases/#v331)。更新解决了 [Tigera Technical Advisory TTA-2018-001 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.projectcalico.org/security-bulletins/)。</td>
</tr>
<tr>
<td>containerd</td>
<td>V1.1.4</td>
<td>V1.1.5</td>
<td>请参阅 [containerd 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/containerd/containerd/releases/tag/v1.1.5)。更新了 containerd，以修复会[阻止 pod 终止 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/containerd/containerd/issues/2744) 的死锁。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>V1.11.3-127</td>
<td>V1.11.4-142</td>
<td>更新为支持 Kubernetes 1.11.4 发行版。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>V1.11.3</td>
<td>V1.11.4</td>
<td>请参阅 [Kubernetes 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.4)。</td>
</tr>
<tr>
<td>OpenVPN 客户机和服务器</td>
<td>2.4.4-r1-6</td>
<td>2.4.6-r3-IKS-8</td>
<td>针对 [CVE-2018-0732 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732) 和 [CVE-2018-0737 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737)，更新了映像。</td>
</tr>
</tbody>
</table>

### 2018 年 11 月 19 日发布的工作程序节点 FP1.11.3_1534 的更改日志
{: #1113_1534}

下表显示了工作程序节点 FP1.11.3_1534 中包含的更改。
{: shortdesc}

<table summary="自 V1.11.3_1533 以来进行的更改">
<caption>自 V1.11.3_1533 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>内核</td>
<td>4.4.0-137</td>
<td>4.4.0-139</td>
<td>通过 [CVE-2018-7755 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-139.165/changelog) 的内核更新，更新了工作程序节点映像。</td>
</tr>
</tbody>
</table>


### 2018 年 11 月 7 日发布的 1.11.3_1533 的更改日志
{: #1113_1533}

下表显示了补丁 1.11.3_1533 中包含的更改。
{: shortdesc}

<table summary="自 V1.11.3_1531 以来进行的更改">
<caption>自 V1.11.3_1531 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>集群主节点 HA 更新</td>
<td>不适用</td>
<td>不适用</td>
<td>对于使用许可 Webhook（例如，`initializerconfigurations`、`mutatingwebhookconfigurations` 或 `validatingwebhookconfigurations`）的集群，修复了对其高可用性 (HA) 主节点的更新。您可以将这些 Webhook 与 Helm chart 配合使用，例如用于 [Container Image Security Enforcement](/docs/services/Registry?topic=registry-security_enforce#security_enforce)。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>V1.11.3-100</td>
<td>V1.11.3-127</td>
<td>添加了 `service.kubernetes.io/ibm-load-balancer-cloud-provider-vlan` 注释，用于指定 LoadBalancer 服务部署到的 VLAN。要查看集群中的可用 VLAN，请运行 `ibmcloud ks vlans --zone <zone>`。</td>
</tr>
<tr>
<td>启用 TPM 的内核</td>
<td>不适用</td>
<td>不适用</td>
<td>对于将 TPM 芯片用于可信计算的裸机工作程序节点，在信任启用之前，会一直使用缺省 Ubuntu 内核。如果在现有集群上[启用信任](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable)，那么需要[重新装入](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)任何具有 TPM 芯片的现有裸机工作程序节点。要检查裸机工作程序节点是否具有 TPM 芯片，请在运行 `ibmcloud ks machine-types --zone` [命令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_machine_types)后查看 **Trustable** 字段。</td>
</tr>
</tbody>
</table>

### 2018 年 11 月 1 日发布的主节点 FP1.11.3_1531 的更改日志
{: #1113_1531_ha-master}

下表显示了主节点 FP1.11.3_1531 中包含的更改。
{: shortdesc}

<table summary="自 V1.11.3_1527 以来进行的更改">
<caption>自 V1.11.3_1527 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>集群主节点</td>
<td>不适用</td>
<td>不适用</td>
<td>更新了集群主节点配置以提高高可用性 (HA)。集群现在有三个 Kubernetes 主节点副本，设置为使用高可用性 (HA) 配置，其中每个主节点副本部署到单独的物理主机上。此外，如果集群位于支持多专区的专区中，那么这些主节点还将在各专区中进行分布。<br>有关必须执行的操作，请参阅[更新为高可用性集群主节点](/docs/containers?topic=containers-cs_versions#ha-masters)。这些准备操作适用于以下情况：<ul>
<li>如果具有防火墙或定制 Calico 网络策略。</li>
<li>如果在工作程序节点上使用的主机端口是 `2040` 或 `2041`。</li>
<li>如果使用了集群主节点 IP 地址对主节点进行集群内访问。</li>
<li>如果具有调用 Calico API 或 CLI (`calicoctl`) 的自动化操作，例如创建 Calico 策略。</li>
<li>如果使用 Kubernetes 或 Calico 网络策略来控制对主节点的 pod 流出访问。</li></ul></td>
</tr>
<tr>
<td>集群主节点 HA 代理</td>
<td>不适用</td>
<td>1.8.12-alpine</td>
<td>在所有工作程序节点上添加了用于客户机端负载均衡的 `ibm-master-proxy-*` pod，以便每个工作程序节点客户机都可以将请求路由到可用的 HA 主节点副本。</td>
</tr>
<tr>
<td>etcd</td>
<td>V3.2.18</td>
<td>V3.3.1</td>
<td>请参阅 [etcd 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/coreos/etcd/releases/v3.3.1)。</td>
</tr>
<tr>
<td>加密 etcd 中的数据</td>
<td>不适用</td>
<td>不适用</td>
<td>先前，etcd 数据存储在静态加密的主节点 NFS 文件存储器实例上。现在，etcd 数据存储在主节点的本地磁盘上，并备份到 {{site.data.keyword.cos_full_notm}}。数据在传输到 {{site.data.keyword.cos_full_notm}} 期间和处于静态时会进行加密。但是，不会对主节点本地磁盘上的 etcd 数据进行加密。如果要对主节点的本地 etcd 数据进行加密，请[在集群中启用 {{site.data.keyword.keymanagementservicelong_notm}}](/docs/containers?topic=containers-encryption#keyprotect)。</td>
</tr>
</tbody>
</table>

### 2018 年 10 月 26 日发布的工作程序节点 FP1.11.3_1531 的更改日志
{: #1113_1531}

下表显示了工作程序节点 FP1.11.3_1531 中包含的更改。
{: shortdesc}

<table summary="自 V1.11.3_1525 以来进行的更改">
<caption>自 V1.11.3_1525 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>操作系统中断处理</td>
<td>不适用</td>
<td>不适用</td>
<td>用更高性能的中断处理程序替换了中断请求 (IRQ) 系统守护程序。</td>
</tr>
</tbody>
</table>

### 2018 年 10 月 15 日发布的主节点 FP1.11.3_1527 的更改日志
{: #1113_1527}

下表显示了主节点 FP1.11.3_1527 中包含的更改。
{: shortdesc}

<table summary="自 V1.11.3_1524 以来进行的更改">
<caption>自 V1.11.3_1524 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico 配置</td>
<td>不适用</td>
<td>不适用</td>
<td>修订了 `calico-node` 容器就绪性探测器，以更好地处理节点故障。</td>
</tr>
<tr>
<td>集群更新</td>
<td>不适用</td>
<td>不适用</td>
<td>解决了从不支持的版本更新主节点时，更新集群附加组件时发生的问题。</td>
</tr>
</tbody>
</table>

### 2018 年 10 月 10 日发布的工作程序节点 FP1.11.3_1525 的更改日志
{: #1113_1525}

下表显示了工作程序节点 FP1.11.3_1525 中包含的更改。
{: shortdesc}

<table summary="自 V1.11.3_1524 以来进行的更改">
<caption>自 V1.11.3_1524 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>内核</td>
<td>4.4.0-133</td>
<td>4.4.0-137</td>
<td>使用 [CVE-2018-14633 和 CVE-2018-17182 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-137.163/changelog) 的内核更新对工作程序节点映像进行了更新。</td>
</tr>
<tr>
<td>不活动会话超时</td>
<td>不适用</td>
<td>不适用</td>
<td>由于合规性原因，将不活动会话超时设置为 5 分钟。</td>
</tr>
</tbody>
</table>


### 2018 年 10 月 2 日发布的 1.11.3_1524 的更改日志
{: #1113_1524}

下表显示了补丁 1.11.3_1524 中包含的更改。
{: shortdesc}

<table summary="自 V1.11.3_1521 以来进行的更改">
<caption>自 V1.11.3_1521 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>containerd</td>
<td>1.1.3</td>
<td>1.1.4</td>
<td>请参阅 [containerd 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/containerd/containerd/releases/tag/v1.1.4)。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>V1.11.3-91</td>
<td>V1.11.3-100</td>
<td>更新了负载均衡器错误消息中的文档链接。</td>
</tr>
<tr>
<td>IBM File Storage 类</td>
<td>不适用</td>
<td>不适用</td>
<td>除去了 IBM File Storage 类中重复的 `reclaimPolicy` 参数。<br><br>
此外，现在更新集群主节点时，缺省 IBM File Storage 类会保持不变。如果要更改缺省存储类，请运行 `kubectl patch storageclass <storageclass> -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'`，并将 `<storageclass>` 替换为存储类的名称。</td>
</tr>
</tbody>
</table>

### 2018 年 9 月 20 日发布的 1.11.3_1521 的更改日志
{: #1113_1521}

下表显示了补丁 1.11.3_1521 中包含的更改。
{: shortdesc}

<table summary="自 V1.11.2_1516 以来进行的更改">
<caption>自 V1.11.2_1516 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>V1.11.2-71</td>
<td>V1.11.3-91</td>
<td>更新为支持 Kubernetes 1.11.3 发行版。</td>
</tr>
<tr>
<td>IBM File Storage 类</td>
<td>不适用</td>
<td>不适用</td>
<td>除去了 IBM File Storage 类中的 `mountOptions`，以使用工作程序节点提供的缺省值。<br><br>
此外，现在更新集群主节点时，缺省 IBM File Storage 类会保持为 `ibmc-file-bronze`。如果要更改缺省存储类，请运行 `kubectl patch storageclass <storageclass> -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'`，并将 `<storageclass>` 替换为存储类的名称。</td>
</tr>
<tr>
<td>密钥管理服务提供程序</td>
<td>不适用</td>
<td>不适用</td>
<td>添加了在集群中使用 Kubernetes 密钥管理服务 (KMS) 提供程序的功能，以支持 {{site.data.keyword.keymanagementservicefull}}。[在集群中启用 {{site.data.keyword.keymanagementserviceshort}}](/docs/containers?topic=containers-encryption#keyprotect) 时，会对所有 Kubernetes 私钥进行加密。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>V1.11.2</td>
<td>V1.11.3</td>
<td>请参阅 [Kubernetes 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.3)。</td>
</tr>
<tr>
<td>Kubernetes DNS Autoscaler</td>
<td>1.1.2-r2</td>
<td>1.2.0</td>
<td>请参阅 [Kubernetes DNS Autoscaler 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes-incubator/cluster-proportional-autoscaler/releases/tag/1.2.0)。</td>
</tr>
<tr>
<td>日志循环</td>
<td>不适用</td>
<td>不适用</td>
<td>切换到使用 `systemd` 计时器，而不使用 `cronjobs`，以防止 `logrotate` 因工作程序节点 90 天内未重新装入或更新而失败。**注**：在该次发行版的所有更早版本中，由于日志不循环，因此在定时作业失败后，主磁盘会填满。在工作程序节点 90 天内处于活动状态而未更新或重新装入后，定时作业会失败。如果日志填满整个主磁盘，那么工作程序节点将进入故障状态。使用 `ibmcloud ks worker-reload` [命令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)或 `ibmcloud ks worker-update` [命令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update)可以修复工作程序节点。</td>
</tr>
<tr>
<td>root 用户密码到期</td>
<td>不适用</td>
<td>不适用</td>
<td>由于合规性原因，工作程序节点的 root 用户密码会在 90 天后到期。如果自动化工具需要以 root 用户身份登录到工作程序节点，或者依赖于以 root 用户身份运行的定时作业，那么可以通过登录到工作程序节点并运行 `chage -M -1 root` 来禁用密码到期。**注**：如果您有阻止以 root 用户身份运行或阻止除去到期密码的安全合规性需求，请不要禁用到期。您可以改为至少每 90 天[更新](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update)或[重新装入](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)一次工作程序节点。</td>
</tr>
<tr>
<td>工作程序节点运行时组件（`kubelet`、`kube-proxy` 和 `containerd`）</td>
<td>不适用</td>
<td>不适用</td>
<td>除去了运行时组件对主磁盘的依赖关系。此增强功能可防止工作程序节点在主磁盘填满时发生故障。</td>
</tr>
<tr>
<td>systemd</td>
<td>不适用</td>
<td>不适用</td>
<td>定期清除瞬态安装单元，以防止它们变得不受控制。此操作解决了 [Kubernetes 问题 57345 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/kubernetes/issues/57345)。</td>
</tr>
</tbody>
</table>

### 2018 年 9 月 4 日发布的 1.11.2_1516 的更改日志
{: #1112_1516}

下表显示了补丁 1.11.2_1516 中包含的更改。
{: shortdesc}

<table summary="自 V1.11.2_1514 以来进行的更改">
<caption>自 V1.11.2_1514 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>V3.1.3</td>
<td>V3.2.1</td>
<td>请参阅 [Calico 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://docs.projectcalico.org/v3.2/releases/#v321)。</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.2</td>
<td>1.1.3</td>
<td>请参阅 [`containerd` 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/containerd/containerd/releases/tag/v1.1.3)。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>V1.11.2-60</td>
<td>V1.11.2-71</td>
<td>通过将 `externalTrafficPolicy` 设置为 `local`，更改了 Cloud Provider 配置以更好地处理负载均衡器服务的更新。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 插件配置</td>
<td>不适用</td>
<td>不适用</td>
<td>已从 IBM 提供的文件存储类的安装选项中除去缺省 NFS 版本。主机的操作系统现在与 IBM Cloud infrastructure (SoftLayer) NFS 服务器协商 NFS 版本。要手动设置特定 NFS 版本，或者更改主机操作系统协商的 PV 的 NFS 版本，请参阅[更改缺省 NFS 版本](/docs/containers?topic=containers-file_storage#nfs_version_class)。</td>
</tr>
</tbody>
</table>

### 2018 年 8 月 23 日发布的工作程序节点 FP1.11.2_1514 的更改日志
{: #1112_1514}

下表显示了工作程序节点 FP1.11.2_1514 中包含的更改。
{: shortdesc}

<table summary="自 V1.11.2_1513 以来进行的更改">
<caption>自 V1.11.2_1513 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>`systemd`</td>
<td>229</td>
<td>230</td>
<td>更新了 `systemd` 以修复 `cgroup` 泄漏。</td>
</tr>
<tr>
<td>内核</td>
<td>4.4.0-127</td>
<td>4.4.0-133</td>
<td>使用 [CVE-2018-3620、CVE-2018-3646 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://usn.ubuntu.com/3741-1/) 的内核更新对工作程序节点映像进行了更新。</td>
</tr>
</tbody>
</table>

### 2018 年 8 月 14 日发布的 1.11.2_1513 的更改日志
{: #1112_1513}

下表显示了补丁 1.11.2_1513 中包含的更改。
{: shortdesc}

<table summary="自 V1.10.5_1518 以来进行的更改">
<caption>自 V1.10.5_1518 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>containerd</td>
<td>不适用</td>
<td>1.1.2</td>
<td>`containerd` 将替换 Docker 来作为 Kubernetes 的新容器运行时。请参阅 [`containerd` 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/containerd/containerd/releases/tag/v1.1.2)。有关必须执行的操作，请参阅[更新为使用 `containerd` 作为容器运行时](/docs/containers?topic=containers-cs_versions#containerd)。</td>
</tr>
<tr>
<td>Docker</td>
<td>不适用</td>
<td>不适用</td>
<td>`containerd` 将替换 Docker 来作为 Kubernetes 的新容器运行时，以增强性能。有关必须执行的操作，请参阅[更新为使用 `containerd` 作为容器运行时](/docs/containers?topic=containers-cs_versions#containerd)。</td>
</tr>
<tr>
<td>etcd</td>
<td>V3.2.14</td>
<td>V3.2.18</td>
<td>请参阅 [etcd 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/coreos/etcd/releases/v3.2.18)。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>V1.10.5-118</td>
<td>V1.11.2-60</td>
<td>更新为支持 Kubernetes 1.11 发行版。此外，负载均衡器 pod 现在使用新的 `ibm-app-cluster-critical` pod 优先级类。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 插件</td>
<td>334</td>
<td>338</td>
<td>已将 `incubator` 版本更新为 1.8。将向选择的特定专区供应文件存储。您无法更新现有（静态）PV 实例标签，除非使用多专区集群并且需要[添加区域和专区标签](/docs/containers?topic=containers-kube_concepts#storage_multizone)。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>V1.10.5</td>
<td>V1.11.2</td>
<td>请参阅 [Kubernetes 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.2)。</td>
</tr>
<tr>
<td>Kubernetes 配置</td>
<td>不适用</td>
<td>不适用</td>
<td>更新了集群的 Kubernetes API 服务器的 OpenID Connect 配置以支持 {{site.data.keyword.Bluemix_notm}} Identity Access and Management (IAM) 访问组。向集群的 Kubernetes API 服务器的 `--enable-admission-plugins` 选项添加了 `Priority`，并将集群配置为支持 pod 优先级。有关更多信息，请参阅：<ul><li>[{{site.data.keyword.Bluemix_notm}}IAM 访问组](/docs/containers?topic=containers-users#rbac)</li>
<li>[配置 pod 优先级](/docs/containers?topic=containers-pod_priority#pod_priority)</li></ul></td>
</tr>
<tr>
<td>Kubernetes Heapster</td>
<td>V1.5.2</td>
<td>V1.5.4</td>
<td>提高了 `heapster-nanny` 容器的资源限制。请参阅 [Kubernetes Heapster 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/heapster/releases/tag/v1.5.4)。</td>
</tr>
<tr>
<td>日志记录配置</td>
<td>不适用</td>
<td>不适用</td>
<td>容器日志目录现在为 `/var/log/pods/`，而不是先前的 `/var/lib/docker/containers/`。</td>
</tr>
</tbody>
</table>

<br />


## 归档
{: #changelog_archive}

不支持的 Kubernetes 版本：
*  [V1.10](#110_changelog)
*  [V1.9](#19_changelog)
*  [V1.8](#18_changelog)
*  [V1.7](#17_changelog)

### V1.10 更改日志（自 2019 年 5 月 16 日起不再支持）
{: #110_changelog}

查看 V1.10 更改日志。
{: shortdesc}

*   [2019 年 5 月 13 日发布的工作程序节点 FP1.10.13_1558 的更改日志](#11013_1558)
*   [2019 年 4 月 29 日发布的工作程序节点 FP1.10.13_1557 的更改日志](#11013_1557)
*   [2019 年 4 月 15 日发布的工作程序节点 FP1.10.13_1556 的更改日志](#11013_1556)
*   [2019 年 4 月 8 日发布的 1.10.13_1555 的更改日志](#11013_1555)
*   [2019 年 4 月 1 日发布的工作程序节点 FP1.10.13_1554 的更改日志](#11013_1554)
*   [2019 年 3 月 26 日发布的主节点 FP1.10.13_1553 的更改日志](#11118_1553)
*   [2019 年 3 月 20 日发布的 1.10.13_1551 的更改日志](#11013_1551)
*   [2019 年 3 月 4 日发布的 1.10.13_1548 的更改日志](#11013_1548)
*   [2019 年 2 月 27 日发布的工作程序节点 FP1.10.12_1546 的更改日志](#11012_1546)
*   [2019 年 2 月 15 日发布的工作程序节点 FP1.10.12_1544 的更改日志](#11012_1544)
*   [2019 年 2 月 5 日发布的 1.10.12_1543 的更改日志](#11012_1543)
*   [2019 年 1 月 28 日发布的工作程序节点 FP1.10.12_1541 的更改日志](#11012_1541)
*   [2019 年 1 月 21 日发布的 1.10.12_1540 的更改日志](#11012_1540)
*   [2019 年 1 月 7 日发布的工作程序节点 FP1.10.11_1538 的更改日志](#11011_1538)
*   [2018 年 12 月 17 日发布的工作程序节点 FP1.10.11_1537 的更改日志](#11011_1537)
*   [2018 年 12 月 4 日发布的 1.10.11_1536 的更改日志](#11011_1536)
*   [2018 年 11 月 27 日发布的工作程序节点 FP1.10.8_1532 的更改日志](#1108_1532)
*   [2018 年 11 月 19 日发布的工作程序节点 FP1.10.8_1531 的更改日志](#1108_1531)
*   [2018 年 11 月 7 日发布的 1.10.8_1530 的更改日志](#1108_1530_ha-master)
*   [2018 年 10 月 26 日发布的工作程序节点 FP1.10.8_1528 的更改日志](#1108_1528)
*   [2018 年 10 月 10 日发布的工作程序节点 FP1.10.8_1525 的更改日志](#1108_1525)
*   [2018 年 10 月 2 日发布的 1.10.8_1524 的更改日志](#1108_1524)
*   [2018 年 9 月 20 日发布的工作程序节点 FP1.10.7_1521 的更改日志](#1107_1521)
*   [2018 年 9 月 4 日发布的 1.10.7_1520 的更改日志](#1107_1520)
*   [2018 年 8 月 23 日发布的工作程序节点 FP1.10.5_1519 的更改日志](#1105_1519)
*   [2018 年 8 月 13 日发布的工作程序节点 FP1.10.5_1518 的更改日志](#1105_1518)
*   [2018 年 7 月 27 日发布的 1.10.5_1517 的更改日志](#1105_1517)
*   [2018 年 7 月 3 日发布的工作程序节点 FP1.10.3_1514 的更改日志](#1103_1514)
*   [2018 年 6 月 21 日发布的工作程序节点 FP1.10.3_1513 的更改日志](#1103_1513)
*   [2018 年 6 月 12 日发布的 1.10.3_1512 的更改日志](#1103_1512)
*   [2018 年 5 月 18 日发布的工作程序节点 FP1.10.1_1510 的更改日志](#1101_1510)
*   [2018 年 5 月 16 日发布的工作程序节点 FP1.10.1_1509 的更改日志](#1101_1509)
*   [2018 年 5 月 1 日发布的 1.10.1_1508 的更改日志](#1101_1508)

#### 2019 年 5 月 13 日发布的工作程序节点 FP1.10.13_1558 的更改日志
{: #11013_1558}

下表显示了工作程序节点 FP1.10.13_1558 中包含的更改。
{: shortdesc}

<table summary="自 V1.10.13_1557 以来进行的更改">
<caption>自 V1.10.13_1557 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>集群主节点 HA 代理</td>
<td>1.9.6-alpine</td>
<td>1.9.7-alpine</td>
<td>请参阅 [HAProxy 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.haproxy.org/download/1.9/src/CHANGELOG)。更新解决了 [CVE-2019-6706 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6706)。</td>
</tr>
</tbody>
</table>

#### 2019 年 4 月 29 日发布的工作程序节点 FP1.10.13_1557 的更改日志
{: #11013_1557}

下表显示了工作程序节点 FP1.10.13_1557 中包含的更改。
{: shortdesc}

<table summary="自 V1.10.13_1556 以来进行的更改">
<caption>自 V1.10.13_1556 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 软件包</td>
<td>不适用</td>
<td>不适用</td>
<td>已安装的 Ubuntu 软件包的更新。</td>
</tr>
</tbody>
</table>


#### 2019 年 4 月 15 日发布的工作程序节点 FP1.10.13_1556 的更改日志
{: #11013_1556}

下表显示了工作程序节点 FP1.10.13_1556 中包含的更改。
{: shortdesc}

<table summary="自 V1.10.13_1555 以来进行的更改">
<caption>自 V1.10.13_1555 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 软件包</td>
<td>不适用</td>
<td>不适用</td>
<td>更新了安装的 Ubuntu 软件包，包括针对 [CVE-2019-3842 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-3842.html)，更新了 `systemd`。</td>
</tr>
</tbody>
</table>

#### 2019 年 4 月 8 日发布的 1.10.13_1555 的更改日志
{: #11013_1555}

下表显示了补丁 1.10.13_1555 中包含的更改。
{: shortdesc}

<table summary="自 V1.10.13_1554 以来进行的更改">
<caption>自 V1.10.13_1554 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>集群主节点 HA 代理</td>
<td>1.8.12-alpine</td>
<td>1.9.6-alpine</td>
<td>请参阅 [HAProxy 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.haproxy.org/download/1.9/src/CHANGELOG)。更新解决了 [CVE-2018-0732 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732)、[CVE-2018-0734 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734)、[CVE-2018-0737 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737)、[CVE-2018-5407 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407)、[CVE-2019-1543 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543) 和 [CVE-2019-1559 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559)。</td>
</tr>
<tr>
<td>Kubernetes DNS</td>
<td>1.14.10</td>
<td>1.14.13</td>
<td>请参阅 [Kubernetes DNS 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/dns/releases/tag/1.14.13)。</td>
</tr>
<tr>
<td>可信计算代理程序</td>
<td>a02f765</td>
<td>e132aa4</td>
<td>针对 [CVE-2017-12447 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-12447)，更新了映像。</td>
</tr>
<tr>
<td>Ubuntu 16.04 内核</td>
<td>4.4.0-143-generic</td>
<td>4.4.0-145-generic</td>
<td>通过针对 [CVE-2019-9213 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html) 的内核更新，更新了工作程序节点映像。</td>
</tr>
<tr>
<td>Ubuntu 18.04 内核</td>
<td>4.15.0-46-generic</td>
<td>4.15.0-47-generic</td>
<td>通过针对 [CVE-2019-9213 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html) 的内核更新，更新了工作程序节点映像。</td>
</tr>
</tbody>
</table>

#### 2019 年 4 月 1 日发布的工作程序节点 FP1.10.13_1554 的更改日志
{: #11013_1554}

下表显示了工作程序节点 FP1.10.13_1554 中包含的更改。
{: shortdesc}

<table summary="自 V1.10.13_1553 以来进行的更改">
<caption>自 V1.10.13_1553 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>工作程序节点资源利用率</td>
<td>不适用</td>
<td>不适用</td>
<td>增大了针对 kubelet 和 containerd 的内存保留量，以防止这些组件耗尽资源。有关更多信息，请参阅[工作程序节点资源保留](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node)。</td>
</tr>
</tbody>
</table>


#### 2019 年 3 月 26 日发布的主节点 FP1.10.13_1553 的更改日志
{: #11118_1553}

下表显示了主节点 FP1.10.13_1553 中包含的更改。
{: shortdesc}

<table summary="自 V1.10.13_1551 以来进行的更改">
<caption>自 V1.10.13_1551 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 插件</td>
<td>345</td>
<td>346</td>
<td>针对 [CVE-2019-9741 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741)，更新了映像。</td>
</tr>
<tr>
<td>密钥管理服务提供程序</td>
<td>166</td>
<td>167</td>
<td>修复了针对管理 Kubernetes 私钥，间歇性发生的`超过了上下文截止期限`和`超时`错误。此外，还修复了对密钥管理服务的更新，原先更新可能会使现有 Kubernetes 私钥处于未加密状态。更新包含针对 [CVE-2019-9741 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741) 的修订。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider 的负载均衡器和负载均衡器监视器</td>
<td>143</td>
<td>146</td>
<td>针对 [CVE-2019-9741 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741)，更新了映像。</td>
</tr>
</tbody>
</table>

#### 2019 年 3 月 20 日发布的 1.10.13_1551 的更改日志
{: #11013_1551}

下表显示了补丁 1.10.13_1551 中包含的更改。
{: shortdesc}

<table summary="自 V1.10.13_1548 以来进行的更改">
<caption>自 V1.10.13_1548 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>集群主节点 HA 代理配置</td>
<td>不适用</td>
<td>不适用</td>
<td>更新了配置，以更好地处理集群主节点的间歇性连接失败。</td>
</tr>
<tr>
<td>GPU 设备插件和安装程序</td>
<td>e32d51c</td>
<td>9ff3fda</td>
<td>GPU 驱动程序更新为 [418.43 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.nvidia.com/object/unix.html)。更新包含针对 [CVE-2019-9741 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9741.html) 的修订。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 插件</td>
<td>344</td>
<td>345</td>
<td>添加了对[专用服务端点](/docs/containers?topic=containers-cs_network_cluster#set-up-private-se)的支持。</td>
</tr>
<tr>
<td>内核</td>
<td>4.4.0-141</td>
<td>4.4.0-143</td>
<td>通过 [CVE-2019-6133 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-6133.html) 的内核更新，更新了工作程序节点映像。</td>
</tr>
<tr>
<td>密钥管理服务提供程序</td>
<td>136</td>
<td>166</td>
<td>针对 [CVE-2018-16890 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16890)、[CVE-2019-3822 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3822) 和 [CVE-2019-3823 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3823)，更新了映像。</td>
</tr>
<tr>
<td>可信计算代理程序</td>
<td>5f3d092</td>
<td>a02f765</td>
<td>针对 [CVE-2018-10779 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10779)、[CVE-2018-12900 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-12900)、[CVE-2018-17000 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-17000)、[CVE-2018-19210 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-19210)、[CVE-2019-6128 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6128) 和 [CVE-2019-7663 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-7663)，更新了映像。</td>
</tr>
</tbody>
</table>

#### 2019 年 3 月 4 日发布的 1.10.13_1548 的更改日志
{: #11013_1548}

下表显示了补丁 1.10.13_1548 中包含的更改。
{: shortdesc}

<table summary="自 V1.10.12_1546 以来进行的更改">
<caption>自 V1.10.12_1546 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>GPU 设备插件和安装程序</td>
<td>eb3a259</td>
<td>e32d51c</td>
<td>针对 [CVE-2019-6454 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454)，更新了映像。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>V1.10.12-252</td>
<td>V1.10.13-288</td>
<td>更新为支持 Kubernetes 1.10.13 发行版。修复了负载均衡器将 `externalTrafficPolicy` 设置为 `local` 时的定期连接问题。更新了负载均衡器事件，以使用最新的 {{site.data.keyword.Bluemix_notm}} 文档链接。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 插件</td>
<td>342</td>
<td>344</td>
<td>将映像的基本操作系统从 Fedora 更改为 Alpine。针对 [CVE-2019-6486 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486)，更新了映像。</td>
</tr>
<tr>
<td>密钥管理服务提供程序</td>
<td>122</td>
<td>136</td>
<td>将客户机超时增加到 {{site.data.keyword.keymanagementservicefull_notm}}。针对 [CVE-2019-6486 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486)，更新了映像。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>V1.10.12</td>
<td>V1.10.13</td>
<td>请参阅 [Kubernetes 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.13)。</td>
</tr>
<tr>
<td>Kubernetes DNS</td>
<td>不适用</td>
<td>不适用</td>
<td>将 Kubernetes DNS pod 内存限制从 `170Mi` 增加到了 `400Mi`，以便处理更多集群服务。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider 的负载均衡器和负载均衡器监视器</td>
<td>132</td>
<td>143</td>
<td>针对 [CVE-2019-6486 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486)，更新了映像。</td>
</tr>
<tr>
<td>OpenVPN 客户机和服务器</td>
<td>2.4.6-r3-IKS-13</td>
<td>2.4.6-r3-IKS-25</td>
<td>针对 [CVE-2019-1559 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559)，更新了映像。</td>
</tr>
<tr>
<td>可信计算代理程序</td>
<td>1ea5ad3</td>
<td>5f3d092</td>
<td>针对 [CVE-2019-6454 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454)，更新了映像。</td>
</tr>
</tbody>
</table>

#### 2019 年 2 月 27 日发布的工作程序节点 FP1.10.12_1546 的更改日志
{: #11012_1546}

下表显示了工作程序节点 FP1.10.12_1546 中包含的更改。
{: shortdesc}

<table summary="自 V1.10.12_1544 以来进行的更改">
<caption>自 V1.10.12_1544 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>内核</td>
<td>4.4.0-141</td>
<td>4.4.0-142</td>
<td>通过 [CVE-2018-19407 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-142.168/changelog) 的内核更新，更新了工作程序节点映像。</td>
</tr>
</tbody>
</table>

#### 2019 年 2 月 15 日发布的工作程序节点 FP1.10.12_1544 的更改日志
{: #11012_1544}

下表显示了工作程序节点 FP1.10.12_1544 中包含的更改。
{: shortdesc}

<table summary="自 V1.10.12_1543 以来进行的更改">
<caption>自 V1.10.12_1543 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Docker</td>
<td>18.06.1-ce</td>
<td>18.06.2-ce</td>
<td>请参阅 [Docker Community Edition 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/docker/docker-ce/releases/tag/v18.06.2-ce)。更新解决了 [CVE-2019-5736 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5736)。</td>
</tr>
<tr>
<td>Kubernetes `kubelet` 配置</td>
<td>不适用</td>
<td>不适用</td>
<td>启用了 `ExperimentalCriticalPodAnnotation` 功能检测点，以防止关键静态 pod 逐出。</td>
</tr>
</tbody>
</table>

#### 2019 年 2 月 5 日发布的 1.10.12_1543 的更改日志
{: #11012_1543}

下表显示了补丁 1.10.12_1543 中包含的更改。
{: shortdesc}

<table summary="自 V1.10.12_1541 以来进行的更改">
<caption>自 V1.10.12_1541 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>etcd</td>
<td>V3.3.1</td>
<td>V3.3.11</td>
<td>请参阅 [etcd 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/coreos/etcd/releases/v3.3.11)。此外，支持用于 etcd 的密码套件现在仅限于具有高强度加密的子集（128 位或更多位）。</td>
</tr>
<tr>
<td>GPU 设备插件和安装程序</td>
<td>13fdc0d</td>
<td>eb3a259</td>
<td>针对 [CVE-2019-3462 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) 和 [CVE-2019-6486 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486)，更新了映像。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 插件</td>
<td>338</td>
<td>342</td>
<td>File Storage 插件按如下所示进行更新：<ul><li>支持使用[卷拓扑感知安排](/docs/containers?topic=containers-file_storage#file-topology)进行动态供应。</li>
<li>忽略存储器已删除时的持久卷声明 (PVC) 删除错误。</li>
<li>向失败的 PVC 添加了失败消息注释。</li>
<li>优化了存储供应程序控制器的领导者选举和再同步时间段设置，并将供应超时从 30 分钟增加到 1 小时。</li>
<li>在开始供应之前检查用户许可权。</li></ul></td>
</tr>
<tr>
<td>密钥管理服务提供程序</td>
<td>111</td>
<td>122</td>
<td>添加了重试逻辑，以避免在 {{site.data.keyword.keymanagementservicefull_notm}} 管理 Kubernetes 私钥时发生临时故障。</td>
</tr>
<tr>
<td>Kubernetes 配置</td>
<td>不适用</td>
<td>不适用</td>
<td>Kubernetes API 服务器审计策略配置更新为包含 `cluster-admin` 请求的日志记录元数据，并记录工作负载 `create`、`update` 和 `patch` 请求的请求主体。</td>
</tr>
<tr>
<td>OpenVPN 客户机</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>针对 [CVE-2018-0734 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) 和 [CVE-2018-5407 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407)，更新了映像。此外，现在 pod 配置将从私钥获取，而不是从配置映射获取。</td>
</tr>
<tr>
<td>OpenVPN 服务器</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>针对 [CVE-2018-0734 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) 和 [CVE-2018-5407 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407)，更新了映像。</td>
</tr>
<tr>
<td>systemd</td>
<td>230</td>
<td>229</td>
<td>[CVE-2018-16864 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16864) 的安全补丁。</td>
</tr>
</tbody>
</table>

#### 2019 年 1 月 28 日发布的工作程序节点 FP1.10.12_1541 的更改日志
{: #11012_1541}

下表显示了工作程序节点 FP1.10.12_1541 中包含的更改。
{: shortdesc}

<table summary="自 V1.10.12_1540 以来进行的更改">
<caption>自 V1.10.12_1540 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 软件包</td>
<td>不适用</td>
<td>不适用</td>
<td>更新了安装的 Ubuntu 软件包，包括针对 [CVE-2019-3462 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) 和 [USN-3863-1 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://usn.ubuntu.com/3863-1)，更新了 `apt`。</td>
</tr>
</tbody>
</table>

#### 2019 年 1 月 21 日发布的 1.10.12_1540 的更改日志
{: #11012_1540}

下表显示了补丁 1.10.12_1540 中包含的更改。
{: shortdesc}

<table summary="自 V1.10.11_1538 以来进行的更改">
<caption>自 V1.10.11_1538 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>V1.10.11-219</td>
<td>V1.10.12-252</td>
<td>更新为支持 Kubernetes 1.10.12 发行版。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>V1.10.11</td>
<td>V1.10.12</td>
<td>请参阅 [Kubernetes 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.12)。</td>
</tr>
<tr>
<td>Kubernetes add-on resizer</td>
<td>1.8.1</td>
<td>1.8.4</td>
<td>请参阅 [Kubernetes add-on resizer 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/autoscaler/releases/tag/addon-resizer-1.8.4)。</td>
</tr>
<tr>
<td>Kubernetes 仪表板</td>
<td>V1.8.3</td>
<td>V1.10.1</td>
<td>请参阅 [Kubernetes 仪表板发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/dashboard/releases/tag/v1.10.1)。更新解决了 [CVE-2018-18264 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-18264)。<br><br>如果您通过 `kubectl proxy` 来访问仪表板，那么会除去登录页面上的**跳过**按钮。请改为[使用**令牌**进行登录](/docs/containers?topic=containers-app#cli_dashboard)。</td>
</tr>
<tr>
<td>GPU 安装程序</td>
<td>390.12</td>
<td>410.79</td>
<td>将安装的 GPU 驱动程序更新为 410.79。</td>
</tr>
</tbody>
</table>

#### 2019 年 1 月 7 日发布的工作程序节点 FP1.10.11_1538 的更改日志
{: #11011_1538}

下表显示了工作程序节点 FP1.10.11_1538 中包含的更改。
{: shortdesc}

<table summary="自 V1.10.11_1537 以来进行的更改">
<caption>自 V1.10.11_1537 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>内核</td>
<td>4.4.0-139</td>
<td>4.4.0-141</td>
<td>使用 [CVE-2017-5753 和 CVE-2018-18690 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-141.167/changelog) 的内核更新，更新了工作程序节点映像。</td>
</tr>
</tbody>
</table>

#### 2018 年 12 月 17 日发布的工作程序节点 FP1.10.11_1537 的更改日志
{: #11011_1537}

下表显示了工作程序节点 FP1.10.11_1537 中包含的更改。
{: shortdesc}

<table summary="自 V1.10.11_1536 以来进行的更改">
<caption>自 V1.10.11_1536 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 软件包</td>
<td>不适用</td>
<td>不适用</td>
<td>已安装的 Ubuntu 软件包的更新。</td>
</tr>
</tbody>
</table>


#### 2018 年 12 月 4 日发布的 1.10.11_1536 的更改日志
{: #11011_1536}

下表显示了补丁 1.10.11_1536 中包含的更改。
{: shortdesc}

<table summary="自 V1.10.8_1532 以来进行的更改">
<caption>自 V1.10.8_1532 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>V3.2.1</td>
<td>V3.3.1</td>
<td>请参阅 [Calico 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://docs.projectcalico.org/v3.3/releases/#v331)。更新解决了 [Tigera Technical Advisory TTA-2018-001 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.projectcalico.org/security-bulletins/)。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>V1.10.8-197</td>
<td>V1.10.11-219</td>
<td>更新为支持 Kubernetes 1.10.11 发行版。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>V1.10.8</td>
<td>V1.10.11</td>
<td>请参阅 [Kubernetes 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.11)。更新解决了 [CVE-2018-1002105 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/kubernetes/issues/71411)。</td>
</tr>
<tr>
<td>OpenVPN 客户机和服务器</td>
<td>2.4.4-r1-6</td>
<td>2.4.6-r3-IKS-8</td>
<td>针对 [CVE-2018-0732 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732) 和 [CVE-2018-0737 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737)，更新了映像。</td>
</tr>
<tr>
<td>工作程序节点资源利用率</td>
<td>不适用</td>
<td>不适用</td>
<td>添加了针对 kubelet 和 Docker 的专用 cgroup，以防止这些组件耗尽资源。有关更多信息，请参阅[工作程序节点资源保留](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node)。</td>
</tr>
</tbody>
</table>

#### 2018 年 11 月 27 日发布的工作程序节点 FP1.10.8_1532 的更改日志
{: #1108_1532}

下表显示了工作程序节点 FP1.10.8_1532 中包含的更改。
{: shortdesc}

<table summary="自 V1.10.8_1531 以来进行的更改">
<caption>自 V1.10.8_1531 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Docker</td>
<td>17.06.2</td>
<td>18.06.1</td>
<td>请参阅 [Docker 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://docs.docker.com/engine/release-notes/#18061-ce)。</td>
</tr>
</tbody>
</table>

#### 2018 年 11 月 19 日发布的工作程序节点 FP1.10.8_1531 的更改日志
{: #1108_1531}

下表显示了工作程序节点 FP1.10.8_1531 中包含的更改。
{: shortdesc}

<table summary="自 V1.10.8_1530 以来进行的更改">
<caption>自 V1.10.8_1530 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>内核</td>
<td>4.4.0-137</td>
<td>4.4.0-139</td>
<td>通过 [CVE-2018-7755 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-139.165/changelog) 的内核更新，更新了工作程序节点映像。</td>
</tr>
</tbody>
</table>

#### 2018 年 11 月 7 日发布的 1.10.8_1530 的更改日志
{: #1108_1530_ha-master}

下表显示了补丁 1.10.8_1530 中包含的更改。
{: shortdesc}

<table summary="自 V1.10.8_1528 以来进行的更改">
<caption>自 V1.10.8_1528 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>集群主节点</td>
<td>不适用</td>
<td>不适用</td>
<td>更新了集群主节点配置以提高高可用性 (HA)。集群现在有三个 Kubernetes 主节点副本，设置为使用高可用性 (HA) 配置，其中每个主节点副本部署到单独的物理主机上。此外，如果集群位于支持多专区的专区中，那么这些主节点还将在各专区中进行分布。<br>有关必须执行的操作，请参阅[更新为高可用性集群主节点](/docs/containers?topic=containers-cs_versions#ha-masters)。这些准备操作适用于以下情况：<ul>
<li>如果具有防火墙或定制 Calico 网络策略。</li>
<li>如果在工作程序节点上使用的主机端口是 `2040` 或 `2041`。</li>
<li>如果使用了集群主节点 IP 地址对主节点进行集群内访问。</li>
<li>如果具有调用 Calico API 或 CLI (`calicoctl`) 的自动化操作，例如创建 Calico 策略。</li>
<li>如果使用 Kubernetes 或 Calico 网络策略来控制对主节点的 pod 流出访问。</li></ul></td>
</tr>
<tr>
<td>集群主节点 HA 代理</td>
<td>不适用</td>
<td>1.8.12-alpine</td>
<td>在所有工作程序节点上添加了用于客户机端负载均衡的 `ibm-master-proxy-*` pod，以便每个工作程序节点客户机都可以将请求路由到可用的 HA 主节点副本。</td>
</tr>
<tr>
<td>etcd</td>
<td>V3.2.18</td>
<td>V3.3.1</td>
<td>请参阅 [etcd 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/coreos/etcd/releases/v3.3.1)。</td>
</tr>
<tr>
<td>加密 etcd 中的数据</td>
<td>不适用</td>
<td>不适用</td>
<td>先前，etcd 数据存储在静态加密的主节点 NFS 文件存储器实例上。现在，etcd 数据存储在主节点的本地磁盘上，并备份到 {{site.data.keyword.cos_full_notm}}。数据在传输到 {{site.data.keyword.cos_full_notm}} 期间和处于静态时会进行加密。但是，不会对主节点本地磁盘上的 etcd 数据进行加密。如果要对主节点的本地 etcd 数据进行加密，请[在集群中启用 {{site.data.keyword.keymanagementservicelong_notm}}](/docs/containers?topic=containers-encryption#keyprotect)。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>V1.10.8-172</td>
<td>V1.10.8-197</td>
<td>添加了 `service.kubernetes.io/ibm-load-balancer-cloud-provider-vlan` 注释，用于指定 LoadBalancer 服务部署到的 VLAN。要查看集群中的可用 VLAN，请运行 `ibmcloud ks vlans --zone <zone>`。</td>
</tr>
<tr>
<td>启用 TPM 的内核</td>
<td>不适用</td>
<td>不适用</td>
<td>对于将 TPM 芯片用于可信计算的裸机工作程序节点，在信任启用之前，会一直使用缺省 Ubuntu 内核。如果在现有集群上[启用信任](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable)，那么需要[重新装入](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)任何具有 TPM 芯片的现有裸机工作程序节点。要检查裸机工作程序节点是否具有 TPM 芯片，请在运行 `ibmcloud ks machine-types --zone` [命令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_machine_types)后查看 **Trustable** 字段。</td>
</tr>
</tbody>
</table>

#### 2018 年 10 月 26 日发布的工作程序节点 FP1.10.8_1528 的更改日志
{: #1108_1528}

下表显示了工作程序节点 FP1.10.8_1528 中包含的更改。
{: shortdesc}

<table summary="自 V1.10.8_1527 以来进行的更改">
<caption>自 V1.10.8_1527 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>操作系统中断处理</td>
<td>不适用</td>
<td>不适用</td>
<td>用更高性能的中断处理程序替换了中断请求 (IRQ) 系统守护程序。</td>
</tr>
</tbody>
</table>

#### 2018 年 10 月 15 日发布的主节点 FP1.10.8_1527 的更改日志
{: #1108_1527}

下表显示了主节点 FP1.10.8_1527 中包含的更改。
{: shortdesc}

<table summary="自 V1.10.8_1524 以来进行的更改">
<caption>自 V1.10.8_1524 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico 配置</td>
<td>不适用</td>
<td>不适用</td>
<td>修订了 `calico-node` 容器就绪性探测器，以更好地处理节点故障。</td>
</tr>
<tr>
<td>集群更新</td>
<td>不适用</td>
<td>不适用</td>
<td>解决了从不支持的版本更新主节点时，更新集群附加组件时发生的问题。</td>
</tr>
</tbody>
</table>

#### 2018 年 10 月 10 日发布的工作程序节点 FP1.10.8_1525 的更改日志
{: #1108_1525}

下表显示了工作程序节点 FP1.10.8_1525 中包含的更改。
{: shortdesc}

<table summary="自 V1.10.8_1524 以来进行的更改">
<caption>自 V1.10.8_1524 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>内核</td>
<td>4.4.0-133</td>
<td>4.4.0-137</td>
<td>使用 [CVE-2018-14633 和 CVE-2018-17182 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-137.163/changelog) 的内核更新对工作程序节点映像进行了更新。</td>
</tr>
<tr>
<td>不活动会话超时</td>
<td>不适用</td>
<td>不适用</td>
<td>由于合规性原因，将不活动会话超时设置为 5 分钟。</td>
</tr>
</tbody>
</table>


#### 2018 年 10 月 2 日发布的 1.10.8_1524 的更改日志
{: #1108_1524}

下表显示了补丁 1.10.8_1524 中包含的更改。
{: shortdesc}

<table summary="自 V1.10.7_1520 以来进行的更改">
<caption>自 V1.10.7_1520 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>密钥管理服务提供程序</td>
<td>不适用</td>
<td>不适用</td>
<td>添加了在集群中使用 Kubernetes 密钥管理服务 (KMS) 提供程序的功能，以支持 {{site.data.keyword.keymanagementservicefull}}。[在集群中启用 {{site.data.keyword.keymanagementserviceshort}}](/docs/containers?topic=containers-encryption#keyprotect) 时，会对所有 Kubernetes 私钥进行加密。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>V1.10.7</td>
<td>V1.10.8</td>
<td>请参阅 [Kubernetes 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.8)。</td>
</tr>
<tr>
<td>Kubernetes DNS Autoscaler</td>
<td>1.1.2-r2</td>
<td>1.2.0</td>
<td>请参阅 [Kubernetes DNS Autoscaler 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes-incubator/cluster-proportional-autoscaler/releases/tag/1.2.0)。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>V1.10.7-146</td>
<td>V1.10.8-172</td>
<td>更新为支持 Kubernetes 1.10.8 发行版。此外，还更新了负载均衡器错误消息中的文档链接。</td>
</tr>
<tr>
<td>IBM File Storage 类</td>
<td>不适用</td>
<td>不适用</td>
<td>除去了 IBM File Storage 类中的 `mountOptions`，以使用工作程序节点提供的缺省值。除去了 IBM File Storage 类中重复的 `reclaimPolicy` 参数。<br><br>
此外，现在更新集群主节点时，缺省 IBM File Storage 类会保持不变。如果要更改缺省存储类，请运行 `kubectl patch storageclass <storageclass> -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'`，并将 `<storageclass>` 替换为存储类的名称。</td>
</tr>
</tbody>
</table>

#### 2018 年 9 月 20 日发布的工作程序节点 FP1.10.7_1521 的更改日志
{: #1107_1521}

下表显示了工作程序节点 FP1.10.7_1521 中包含的更改。
{: shortdesc}

<table summary="自 V1.10.7_1520 以来进行的更改">
<caption>自 V1.10.7_1520 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>日志循环</td>
<td>不适用</td>
<td>不适用</td>
<td>切换到使用 `systemd` 计时器，而不使用 `cronjobs`，以防止 `logrotate` 因工作程序节点 90 天内未重新装入或更新而失败。**注**：在该次发行版的所有更早版本中，由于日志不循环，因此在定时作业失败后，主磁盘会填满。在工作程序节点 90 天内处于活动状态而未更新或重新装入后，定时作业会失败。如果日志填满整个主磁盘，那么工作程序节点将进入故障状态。使用 `ibmcloud ks worker-reload` [命令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)或 `ibmcloud ks worker-update` [命令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update)可以修复工作程序节点。</td>
</tr>
<tr>
<td>工作程序节点运行时组件（`kubelet`、`kube-proxy` 和 `docker`）</td>
<td>不适用</td>
<td>不适用</td>
<td>除去了运行时组件对主磁盘的依赖关系。此增强功能可防止工作程序节点在主磁盘填满时发生故障。</td>
</tr>
<tr>
<td>root 用户密码到期</td>
<td>不适用</td>
<td>不适用</td>
<td>由于合规性原因，工作程序节点的 root 用户密码会在 90 天后到期。如果自动化工具需要以 root 用户身份登录到工作程序节点，或者依赖于以 root 用户身份运行的定时作业，那么可以通过登录到工作程序节点并运行 `chage -M -1 root` 来禁用密码到期。**注**：如果您有阻止以 root 用户身份运行或阻止除去到期密码的安全合规性需求，请不要禁用到期。您可以改为至少每 90 天[更新](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update)或[重新装入](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)一次工作程序节点。</td>
</tr>
<tr>
<td>systemd</td>
<td>不适用</td>
<td>不适用</td>
<td>定期清除瞬态安装单元，以防止它们变得不受控制。此操作解决了 [Kubernetes 问题 57345 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/kubernetes/issues/57345)。</td>
</tr>
<tr>
<td>Docker</td>
<td>不适用</td>
<td>不适用</td>
<td>禁用了缺省 Docker 网桥，以便 `172.17.0.0/16` IP 范围现在可用于专用路由。如果通过直接在主机上执行 `docker` 命令或使用安装了 Docker 套接字的 pod 来依赖于在工作程序节点中构建 Docker 容器，请从以下选项中进行选择。<ul><li>要确保构建容器时的外部网络连接，请运行 `docker build . --network host`。</li>
<li>要显式创建在构建容器时要使用的网络，请运行 `docker network create`，然后使用此网络。</li></ul>
**注**：是否对 Docker 套接字或 Docker 有直接依赖关系？请[更新为使用 `containerd`（而不是 `docker`）作为容器运行时](/docs/containers?topic=containers-cs_versions#containerd)，以便准备好集群来运行 Kubernetes V1.11 或更高版本。</td>
</tr>
</tbody>
</table>

#### 2018 年 9 月 4 日发布的 1.10.7_1520 的更改日志
{: #1107_1520}

下表显示了补丁 1.10.7_1520 中包含的更改。
{: shortdesc}

<table summary="自 V1.10.5_1519 以来进行的更改">
<caption>自 V1.10.5_1519 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>V3.1.3</td>
<td>V3.2.1</td>
<td>请参阅 Calico [发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://docs.projectcalico.org/v3.2/releases/#v321)。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>V1.10.5-118</td>
<td>V1.10.7-146</td>
<td>更新为支持 Kubernetes 1.10.7 发行版。此外，通过将 `externalTrafficPolicy` 设置为 `local`，更改了 Cloud Provider 配置以更好地处理负载均衡器服务的更新。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 插件</td>
<td>334</td>
<td>338</td>
<td>已将 incubator 版本更新为 1.8。将向选择的特定专区供应文件存储。您无法更新现有（静态）PV 实例的标签，除非使用多专区集群并且需要添加区域和专区标签。<br><br> 已从 IBM 提供的文件存储类的安装选项中除去缺省 NFS 版本。主机的操作系统现在与 IBM Cloud infrastructure (SoftLayer) NFS 服务器协商 NFS 版本。要手动设置特定 NFS 版本，或者更改主机操作系统协商的 PV 的 NFS 版本，请参阅[更改缺省 NFS 版本](/docs/containers?topic=containers-file_storage#nfs_version_class)。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>V1.10.5</td>
<td>V1.10.7</td>
<td>请参阅 Kubernetes [发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.7)。</td>
</tr>
<tr>
<td>Kubernetes Heapster 配置</td>
<td>不适用</td>
<td>不适用</td>
<td>提高了 `heapster-nanny` 容器的资源限制。</td>
</tr>
</tbody>
</table>

#### 2018 年 8 月 23 日发布的工作程序节点 FP1.10.5_1519 的更改日志
{: #1105_1519}

下表显示了工作程序节点 FP1.10.5_1519 中包含的更改。
{: shortdesc}

<table summary="自 V1.10.5_1518 以来进行的更改">
<caption>自 V1.10.5_1518 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>`systemd`</td>
<td>229</td>
<td>230</td>
<td>更新了 `systemd` 以修复 `cgroup` 泄漏。</td>
</tr>
<tr>
<td>内核</td>
<td>4.4.0-127</td>
<td>4.4.0-133</td>
<td>使用 [CVE-2018-3620、CVE-2018-3646 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://usn.ubuntu.com/3741-1/) 的内核更新对工作程序节点映像进行了更新。</td>
</tr>
</tbody>
</table>


#### 2018 年 8 月 13 日发布的工作程序节点 FP1.10.5_1518 的更改日志
{: #1105_1518}

下表显示了工作程序节点 FP1.10.5_1518 中包含的更改。
{: shortdesc}

<table summary="自 V1.10.5_1517 以来进行的更改">
<caption>自 V1.10.5_1517 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 软件包</td>
<td>不适用</td>
<td>不适用</td>
<td>已安装的 Ubuntu 软件包的更新。</td>
</tr>
</tbody>
</table>

#### 2018 年 7 月 27 日发布的 1.10.5_1517 的更改日志
{: #1105_1517}

下表显示了补丁 1.10.5_1517 中包含的更改。
{: shortdesc}

<table summary="自 V1.10.3_1514 以来进行的更改">
<caption>自 V1.10.3_1514 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>V3.1.1</td>
<td>V3.1.3</td>
<td>请参阅 Calico [发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://docs.projectcalico.org/v3.1/releases/#v313)。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>V1.10.3-85</td>
<td>V1.10.5-118</td>
<td>更新为支持 Kubernetes 1.10.5 发行版。此外，LoadBalancer 服务 `create failure` 事件现在包含任何可移植子网错误。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 插件</td>
<td>320</td>
<td>334</td>
<td>将创建持久卷的超时从 15 分钟增加到 30 分钟。将缺省计费类型更改为 `hourly`。向预定义存储类添加了安装选项。在 IBM Cloud Infrastructure (SoftLayer) 帐户的 NFS 文件存储器实例中，已将**注释**字段更改为 JSON 格式，并添加了 PV 部署到的 Kubernetes 名称空间。为了支持多专区集群，向持久卷添加了专区和区域标签。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>V1.10.3</td>
<td>V1.10.5</td>
<td>请参阅 Kubernetes [发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.5)。</td>
</tr>
<tr>
<td>内核</td>
<td>不适用</td>
<td>不适用</td>
<td>针对高性能联网工作负载对工作程序节点网络设置进行了小幅改进。</td>
</tr>
<tr>
<td>OpenVPN 客户机</td>
<td>不适用</td>
<td>不适用</td>
<td>在 `kube-system` 名称空间中运行的 OpenVPN 客户机 `vpn` 部署现在由 Kubernetes `addon-manager` 进行管理。</td>
</tr>
</tbody>
</table>

#### 2018 年 7 月 3 日发布的工作程序节点 FP1.10.3_1514 的更改日志
{: #1103_1514}

下表显示了工作程序节点 FP1.10.3_1514 中包含的更改。
{: shortdesc}

<table summary="自 V1.10.3_1513 以来进行的更改">
<caption>自 V1.10.3_1513 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>内核</td>
<td>不适用</td>
<td>不适用</td>
<td>针对高性能联网工作负载优化了 `sysctl`。</td>
</tr>
</tbody>
</table>


#### 2018 年 6 月 21 日发布的工作程序节点 FP1.10.3_1513 的更改日志
{: #1103_1513}

下表显示了工作程序节点 FP1.10.3_1513 中包含的更改。
{: shortdesc}

<table summary="自 V1.10.3_1512 以来进行的更改">
<caption>自 V1.10.3_1512 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Docker</td>
<td>不适用</td>
<td>不适用</td>
<td>对于未加密的机器类型，重新装入或更新工作程序节点时，将通过获取全新文件系统来清除辅助磁盘。</td>
</tr>
</tbody>
</table>

#### 2018 年 6 月 12 日发布的 1.10.3_1512 的更改日志
{: #1103_1512}

下表显示了补丁 1.10.3_1512 中包含的更改。
{: shortdesc}

<table summary="自 V1.10.1_1510 以来进行的更改">
<caption>自 V1.10.1_1510 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>V1.10.1</td>
<td>V1.10.3</td>
<td>请参阅 Kubernetes [发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.3)。</td>
</tr>
<tr>
<td>Kubernetes 配置</td>
<td>不适用</td>
<td>不适用</td>
<td>向集群的 Kubernetes API 服务器的 `--enable-admission-plugins` 选项添加了 `PodSecurityPolicy`，并将集群配置为支持 pod 安全策略。有关更多信息，请参阅[配置 pod 安全策略](/docs/containers?topic=containers-psp)。</td>
</tr>
<tr>
<td>Kubelet 配置</td>
<td>不适用</td>
<td>不适用</td>
<td>启用了 `--authentication-token-webhook` 选项以支持 API 不记名令牌和服务帐户令牌认证到 `kubelet` HTTPS 端点。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>V1.10.1-52</td>
<td>V1.10.3-85</td>
<td>更新为支持 Kubernetes 1.10.3 发行版。</td>
</tr>
<tr>
<td>OpenVPN 客户机</td>
<td>不适用</td>
<td>不适用</td>
<td>向在 `kube-system` 名称空间中运行的 OpenVPN 客户机 `vpn` 部署添加了 `livenessProbe`。</td>
</tr>
<tr>
<td>内核更新</td>
<td>4.4.0-116</td>
<td>4.4.0-127</td>
<td>新工作程序节点映像具有 [CVE-2018-3639 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-3639) 的内核更新。</td>
</tr>
</tbody>
</table>



#### 2018 年 5 月 18 日发布的工作程序节点 FP1.10.1_1510 的更改日志
{: #1101_1510}

下表显示了工作程序节点 FP1.10.1_1510 中包含的更改。
{: shortdesc}

<table summary="自 V1.10.1_1509 以来进行的更改">
<caption>自 V1.10.1_1509 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>不适用</td>
<td>不适用</td>
<td>修订解决了使用块存储器插件时发生的错误。</td>
</tr>
</tbody>
</table>

#### 2018 年 5 月 16 日发布的工作程序节点 FP1.10.1_1509 的更改日志
{: #1101_1509}

下表显示了工作程序节点 FP1.10.1_1509 中包含的更改。
{: shortdesc}

<table summary="自 V1.10.1_1508 以来进行的更改">
<caption>自 V1.10.1_1508 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>不适用</td>
<td>不适用</td>
<td>存储在 `kubelet` 根目录中的数据现在保存在工作程序节点机器中更大的辅助磁盘上。</td>
</tr>
</tbody>
</table>

#### 2018 年 5 月 1 日发布的 1.10.1_1508 的更改日志
{: #1101_1508}

下表显示了补丁 1.10.1_1508 中包含的更改。
{: shortdesc}

<table summary="自 V1.9.7_1510 以来进行的更改">
<caption>自 V1.9.7_1510 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>V2.6.5</td>
<td>V3.1.1</td>
<td>请参阅 Calico [发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://docs.projectcalico.org/v3.1/releases/#v311)。</td>
</tr>
<tr>
<td>Kubernetes Heapster</td>
<td>V1.5.0</td>
<td>V1.5.2</td>
<td>请参阅 Kubernetes Heapster [发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/heapster/releases/tag/v1.5.2)。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>V1.9.7</td>
<td>V1.10.1</td>
<td>请参阅 Kubernetes [发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.1)。</td>
</tr>
<tr>
<td>Kubernetes 配置</td>
<td>不适用</td>
<td>不适用</td>
<td>向集群的 Kubernetes API 服务器的 <code>--enable-admission-plugins</code> 选项添加了 <code>StorageObjectInUseProtection</code>。</td>
</tr>
<tr>
<td>Kubernetes DNS</td>
<td>1.14.8</td>
<td>1.14.10</td>
<td>请参阅 Kubernetes DNS [发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/dns/releases/tag/1.14.10)。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>V1.9.7-102</td>
<td>V1.10.1-52</td>
<td>更新为支持 Kubernetes 1.10 发行版。</td>
</tr>
<tr>
<td>GPU 支持</td>
<td>不适用</td>
<td>不适用</td>
<td>现在，可支持[图形处理单元 (GPU) 容器工作负载](/docs/containers?topic=containers-app#gpu_app)进行安排和执行。有关可用 GPU 机器类型的列表，请参阅[工作程序节点的硬件](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)。有关更多信息，请参阅 Kubernetes 文档以[安排 GPU ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/tasks/manage-gpus/scheduling-gpus/)。</td>
</tr>
</tbody>
</table>

<br />


### V1.9 更改日志（自 2018 年 12 月 27 日起不再支持）
{: #19_changelog}

查看 V1.9 更改日志。
{: shortdesc}

*   [2018 年 12 月 17 日发布的工作程序节点 FP1.9.11_1539 的更改日志](#1911_1539)
*   [2018 年 12 月 4 日发布的工作程序节点 FP1.9.11_1538 的更改日志](#1911_1538)
*   [2018 年 11 月 27 日发布的工作程序节点 FP1.9.11_1537 的更改日志](#1911_1537)
*   [2018 年 11 月 19 日发布的 1.9.11_1536 的更改日志](#1911_1536)
*   [2018 年 11 月 7 日发布的工作程序节点 FP1.9.10_1532 的更改日志](#1910_1532)
*   [2018 年 10 月 26 日发布的工作程序节点 FP1.9.10_1531 的更改日志](#1910_1531)
*   [2018 年 10 月 15 日发布的主节点 FP1.9.10_1530 的更改日志](#1910_1530)
*   [2018 年 10 月 10 日发布的工作程序节点 FP1.9.10_1528 的更改日志](#1910_1528)
*   [2018 年 10 月 2 日发布的 1.9.10_1527 的更改日志](#1910_1527)
*   [2018 年 9 月 20 日发布的工作程序节点 FP1.9.10_1524 的更改日志](#1910_1524)
*   [2018 年 9 月 4 日发布的 1.9.10_1523 的更改日志](#1910_1523)
*   [2018 年 8 月 23 日发布的工作程序节点 FP1.9.9_1522 的更改日志](#199_1522)
*   [2018 年 8 月 13 日发布的工作程序节点 FP1.9.9_1521 的更改日志](#199_1521)
*   [2018 年 7 月 27 日发布的 1.9.9_1520 的更改日志](#199_1520)
*   [2018 年 7 月 3 日发布的工作程序节点 FP1.9.8_1517 的更改日志](#198_1517)
*   [2018 年 6 月 21 日发布的工作程序节点 FP1.9.8_1516 的更改日志](#198_1516)
*   [2018 年 6 月 19 日发布的 1.9.8_1515 的更改日志](#198_1515)
*   [2018 年 6 月 11 日发布的工作程序节点 FP1.9.7_1513 的更改日志](#197_1513)
*   [2018 年 5 月 18 日发布的工作程序节点 FP1.9.7_1512 的更改日志](#197_1512)
*   [2018 年 5 月 16 日发布的工作程序节点 FP1.9.7_1511 的更改日志](#197_1511)
*   [2018 年 4 月 30 日发布的 1.9.7_1510 的更改日志](#197_1510)

#### 2018 年 12 月 17 日发布的工作程序节点 FP1.9.11_1539 的更改日志
{: #1911_1539}

下表显示了工作程序节点 FP1.9.11_1539 中包含的更改。
{: shortdesc}

<table summary="自 V1.9.11_1538 以来进行的更改">
<caption>自 V1.9.11_1538 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 软件包</td>
<td>不适用</td>
<td>不适用</td>
<td>已安装的 Ubuntu 软件包的更新。</td>
</tr>
</tbody>
</table>

#### 2018 年 12 月 4 日发布的工作程序节点 FP1.9.11_1538 的更改日志
{: #1911_1538}

下表显示了工作程序节点 FP1.9.11_1538 中包含的更改。
{: shortdesc}

<table summary="自 V1.9.11_1537 以来进行的更改">
<caption>自 V1.9.11_1537 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>工作程序节点资源利用率</td>
<td>不适用</td>
<td>不适用</td>
<td>添加了针对 kubelet 和 Docker 的专用 cgroup，以防止这些组件耗尽资源。有关更多信息，请参阅[工作程序节点资源保留](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node)。</td>
</tr>
</tbody>
</table>

#### 2018 年 11 月 27 日发布的工作程序节点 FP1.9.11_1537 的更改日志
{: #1911_1537}

下表显示了工作程序节点 FP1.9.11_1537 中包含的更改。
{: shortdesc}

<table summary="自 V1.9.11_1536 以来进行的更改">
<caption>自 V1.9.11_1536 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Docker</td>
<td>17.06.2</td>
<td>18.06.1</td>
<td>请参阅 [Docker 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://docs.docker.com/engine/release-notes/#18061-ce)。</td>
</tr>
</tbody>
</table>

#### 2018 年 11 月 19 日发布的 1.9.11_1536 的更改日志
{: #1911_1536}

下表显示了补丁 1.9.11_1536 中包含的更改。
{: shortdesc}

<table summary="自 V1.9.10_1532 以来进行的更改">
<caption>自 V1.9.10_1532 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>V2.6.5</td>
<td>V2.6.12</td>
<td>请参阅 [Calico 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://docs.projectcalico.org/v2.6/releases/#v2612)。更新解决了 [Tigera Technical Advisory TTA-2018-001 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.projectcalico.org/security-bulletins/)。</td>
</tr>
<tr>
<td>内核</td>
<td>4.4.0-137</td>
<td>4.4.0-139</td>
<td>通过 [CVE-2018-7755 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-139.165/changelog) 的内核更新，更新了工作程序节点映像。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>V1.9.10</td>
<td>V1.9.11</td>
<td>请参阅 [Kubernetes 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.11)。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}}</td>
<td>V1.9.10-219</td>
<td>V1.9.11-249</td>
<td>更新为支持 Kubernetes 1.9.11 发行版。</td>
</tr>
<tr>
<td>OpenVPN 客户机和服务器</td>
<td>2.4.4-r2</td>
<td>2.4.6-r3-IKS-8</td>
<td>针对 [CVE-2018-0732 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732) 和 [CVE-2018-0737 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737)，更新了映像。</td>
</tr>
</tbody>
</table>

#### 2018 年 11 月 7 日发布的工作程序节点 FP1.9.10_1532 的更改日志
{: #1910_1532}

下表显示了工作程序节点 FP1.9.11_1532 中包含的更改。
{: shortdesc}

<table summary="自 V1.9.10_1531 以来进行的更改">
<caption>自 V1.9.10_1531 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>启用 TPM 的内核</td>
<td>不适用</td>
<td>不适用</td>
<td>对于将 TPM 芯片用于可信计算的裸机工作程序节点，在信任启用之前，会一直使用缺省 Ubuntu 内核。如果在现有集群上[启用信任](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable)，那么需要[重新装入](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)任何具有 TPM 芯片的现有裸机工作程序节点。要检查裸机工作程序节点是否具有 TPM 芯片，请在运行 `ibmcloud ks machine-types --zone` [命令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_machine_types)后查看 **Trustable** 字段。</td>
</tr>
</tbody>
</table>

#### 2018 年 10 月 26 日发布的工作程序节点 FP1.9.10_1531 的更改日志
{: #1910_1531}

下表显示了工作程序节点 FP1.9.10_1531 中包含的更改。
{: shortdesc}

<table summary="自 V1.9.10_1530 以来进行的更改">
<caption>自 V1.9.10_1530 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>操作系统中断处理</td>
<td>不适用</td>
<td>不适用</td>
<td>用更高性能的中断处理程序替换了中断请求 (IRQ) 系统守护程序。</td>
</tr>
</tbody>
</table>

#### 2018 年 10 月 15 日发布的主节点 FP1.9.10_1530 的更改日志
{: #1910_1530}

下表显示了工作程序节点 FP1.9.10_1530 中包含的更改。
{: shortdesc}

<table summary="自 V1.9.10_1527 以来进行的更改">
<caption>自 V1.9.10_1527 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>集群更新</td>
<td>不适用</td>
<td>不适用</td>
<td>解决了从不支持的版本更新主节点时，更新集群附加组件时发生的问题。</td>
</tr>
</tbody>
</table>

#### 2018 年 10 月 10 日发布的工作程序节点 FP1.9.10_1528 的更改日志
{: #1910_1528}

下表显示了工作程序节点 FP1.9.10_1528 中包含的更改。
{: shortdesc}

<table summary="自 V1.9.10_1527 以来进行的更改">
<caption>自 V1.9.10_1527 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>内核</td>
<td>4.4.0-133</td>
<td>4.4.0-137</td>
<td>使用 [CVE-2018-14633 和 CVE-2018-17182 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-137.163/changelog) 的内核更新对工作程序节点映像进行了更新。</td>
</tr>
<tr>
<td>不活动会话超时</td>
<td>不适用</td>
<td>不适用</td>
<td>由于合规性原因，将不活动会话超时设置为 5 分钟。</td>
</tr>
</tbody>
</table>


#### 2018 年 10 月 2 日发布的 1.9.10_1527 的更改日志
{: #1910_1527}

下表显示了补丁 1.9.10_1527 中包含的更改。
{: shortdesc}

<table summary="自 V1.9.10_1523 以来进行的更改">
<caption>自 V1.9.10_1523 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>V1.9.10-192</td>
<td>V1.9.10-219</td>
<td>更新了负载均衡器错误消息中的文档链接。</td>
</tr>
<tr>
<td>IBM File Storage 类</td>
<td>不适用</td>
<td>不适用</td>
<td>除去了 IBM File Storage 类中的 `mountOptions`，以使用工作程序节点提供的缺省值。除去了 IBM File Storage 类中重复的 `reclaimPolicy` 参数。<br><br>
此外，现在更新集群主节点时，缺省 IBM File Storage 类会保持不变。如果要更改缺省存储类，请运行 `kubectl patch storageclass <storageclass> -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'`，并将 `<storageclass>` 替换为存储类的名称。</td>
</tr>
</tbody>
</table>

#### 2018 年 9 月 20 日发布的工作程序节点 FP1.9.10_1524 的更改日志
{: #1910_1524}

下表显示了工作程序节点 FP1.9.10_1524 中包含的更改。
{: shortdesc}

<table summary="自 V1.9.10_1523 以来进行的更改">
<caption>自 V1.9.10_1523 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>日志循环</td>
<td>不适用</td>
<td>不适用</td>
<td>切换到使用 `systemd` 计时器，而不使用 `cronjobs`，以防止 `logrotate` 因工作程序节点 90 天内未重新装入或更新而失败。**注**：在该次发行版的所有更早版本中，由于日志不循环，因此在定时作业失败后，主磁盘会填满。在工作程序节点 90 天内处于活动状态而未更新或重新装入后，定时作业会失败。如果日志填满整个主磁盘，那么工作程序节点将进入故障状态。使用 `ibmcloud ks worker-reload` [命令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)或 `ibmcloud ks worker-update` [命令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update)可以修复工作程序节点。</td>
</tr>
<tr>
<td>工作程序节点运行时组件（`kubelet`、`kube-proxy` 和 `docker`）</td>
<td>不适用</td>
<td>不适用</td>
<td>除去了运行时组件对主磁盘的依赖关系。此增强功能可防止工作程序节点在主磁盘填满时发生故障。</td>
</tr>
<tr>
<td>root 用户密码到期</td>
<td>不适用</td>
<td>不适用</td>
<td>由于合规性原因，工作程序节点的 root 用户密码会在 90 天后到期。如果自动化工具需要以 root 用户身份登录到工作程序节点，或者依赖于以 root 用户身份运行的定时作业，那么可以通过登录到工作程序节点并运行 `chage -M -1 root` 来禁用密码到期。**注**：如果您有阻止以 root 用户身份运行或阻止除去到期密码的安全合规性需求，请不要禁用到期。您可以改为至少每 90 天[更新](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update)或[重新装入](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)一次工作程序节点。</td>
</tr>
<tr>
<td>systemd</td>
<td>不适用</td>
<td>不适用</td>
<td>定期清除瞬态安装单元，以防止它们变得不受控制。此操作解决了 [Kubernetes 问题 57345 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/kubernetes/issues/57345)。</td>
</tr>
<tr>
<td>Docker</td>
<td>不适用</td>
<td>不适用</td>
<td>禁用了缺省 Docker 网桥，以便 `172.17.0.0/16` IP 范围现在可用于专用路由。如果通过直接在主机上执行 `docker` 命令或使用安装了 Docker 套接字的 pod 来依赖于在工作程序节点中构建 Docker 容器，请从以下选项中进行选择。<ul><li>要确保构建容器时的外部网络连接，请运行 `docker build . --network host`。</li>
<li>要显式创建在构建容器时要使用的网络，请运行 `docker network create`，然后使用此网络。</li></ul>
**注**：是否对 Docker 套接字或 Docker 有直接依赖关系？请[更新为使用 `containerd`（而不是 `docker`）作为容器运行时](/docs/containers?topic=containers-cs_versions#containerd)，以便准备好集群来运行 Kubernetes V1.11 或更高版本。</td>
</tr>
</tbody>
</table>

#### 2018 年 9 月 4 日发布的 1.9.10_1523 的更改日志
{: #1910_1523}

下表显示了补丁 1.9.10_1523 中包含的更改。
{: shortdesc}

<table summary="自 V1.9.9_1522 以来进行的更改">
<caption>自 V1.9.9_1522 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>V1.9.9-167</td>
<td>V1.9.10-192</td>
<td>更新为支持 Kubernetes 1.9.10 发行版。此外，通过将 `externalTrafficPolicy` 设置为 `local`，更改了 Cloud Provider 配置以更好地处理负载均衡器服务的更新。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 插件</td>
<td>334</td>
<td>338</td>
<td>已将 incubator 版本更新为 1.8。将向选择的特定专区供应文件存储。您无法更新现有（静态）PV 实例的标签，除非使用多专区集群并且需要添加区域和专区标签。<br><br>已从 IBM 提供的文件存储类的安装选项中除去缺省 NFS 版本。主机的操作系统现在与 IBM Cloud infrastructure (SoftLayer) NFS 服务器协商 NFS 版本。要手动设置特定 NFS 版本，或者更改主机操作系统协商的 PV 的 NFS 版本，请参阅[更改缺省 NFS 版本](/docs/containers?topic=containers-file_storage#nfs_version_class)。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>V1.9.9</td>
<td>V1.9.10</td>
<td>请参阅 Kubernetes [发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.10)。</td>
</tr>
<tr>
<td>Kubernetes Heapster 配置</td>
<td>不适用</td>
<td>不适用</td>
<td>提高了 `heapster-nanny` 容器的资源限制。</td>
</tr>
</tbody>
</table>

#### 2018 年 8 月 23 日发布的工作程序节点 FP1.9.9_1522 的更改日志
{: #199_1522}

下表显示了工作程序节点 FP1.9.9_1522 中包含的更改。
{: shortdesc}

<table summary="自 V1.9.9_1521 以来进行的更改">
<caption>自 V1.9.9_1521 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>`systemd`</td>
<td>229</td>
<td>230</td>
<td>更新了 `systemd` 以修复 `cgroup` 泄漏。</td>
</tr>
<tr>
<td>内核</td>
<td>4.4.0-127</td>
<td>4.4.0-133</td>
<td>使用 [CVE-2018-3620、CVE-2018-3646 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://usn.ubuntu.com/3741-1/) 的内核更新对工作程序节点映像进行了更新。</td>
</tr>
</tbody>
</table>


#### 2018 年 8 月 13 日发布的工作程序节点 FP1.9.9_1521 的更改日志
{: #199_1521}

下表显示了工作程序节点 FP1.9.9_1521 中包含的更改。
{: shortdesc}

<table summary="自 V1.9.9_1520 以来进行的更改">
<caption>自 V1.9.9_1520 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 软件包</td>
<td>不适用</td>
<td>不适用</td>
<td>已安装的 Ubuntu 软件包的更新。</td>
</tr>
</tbody>
</table>

#### 2018 年 7 月 27 日发布的 1.9.9_1520 的更改日志
{: #199_1520}

下表显示了补丁 1.9.9_1520 中包含的更改。
{: shortdesc}

<table summary="自 V1.9.8_1517 以来进行的更改">
<caption>自 V1.9.8_1517 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>V1.9.8-141</td>
<td>V1.9.9-167</td>
<td>更新为支持 Kubernetes 1.9.9 发行版。此外，LoadBalancer 服务 `create failure` 事件现在包含任何可移植子网错误。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 插件</td>
<td>320</td>
<td>334</td>
<td>将创建持久卷的超时从 15 分钟增加到 30 分钟。将缺省计费类型更改为 `hourly`。向预定义存储类添加了安装选项。在 IBM Cloud Infrastructure (SoftLayer) 帐户的 NFS 文件存储器实例中，已将**注释**字段更改为 JSON 格式，并添加了 PV 部署到的 Kubernetes 名称空间。为了支持多专区集群，向持久卷添加了专区和区域标签。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>V1.9.8</td>
<td>V1.9.9</td>
<td>请参阅 Kubernetes [发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.9)。</td>
</tr>
<tr>
<td>内核</td>
<td>不适用</td>
<td>不适用</td>
<td>针对高性能联网工作负载对工作程序节点网络设置进行了小幅改进。</td>
</tr>
<tr>
<td>OpenVPN 客户机</td>
<td>不适用</td>
<td>不适用</td>
<td>在 `kube-system` 名称空间中运行的 OpenVPN 客户机 `vpn` 部署现在由 Kubernetes `addon-manager` 进行管理。</td>
</tr>
</tbody>
</table>

#### 2018 年 7 月 3 日发布的工作程序节点 FP1.9.8_1517 的更改日志
{: #198_1517}

下表显示了工作程序节点 FP1.9.8_1517 中包含的更改。
{: shortdesc}

<table summary="自 V1.9.8_1516 以来进行的更改">
<caption>自 V1.9.8_1516 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>内核</td>
<td>不适用</td>
<td>不适用</td>
<td>针对高性能联网工作负载优化了 `sysctl`。</td>
</tr>
</tbody>
</table>


#### 2018 年 6 月 21 日发布的工作程序节点 FP1.9.8_1516 的更改日志
{: #198_1516}

下表显示了工作程序节点 FP1.9.8_1516 中包含的更改。
{: shortdesc}

<table summary="自 V1.9.8_1515 以来进行的更改">
<caption>自 V1.9.8_1515 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Docker</td>
<td>不适用</td>
<td>不适用</td>
<td>对于未加密的机器类型，重新装入或更新工作程序节点时，将通过获取全新文件系统来清除辅助磁盘。</td>
</tr>
</tbody>
</table>

#### 2018 年 6 月 19 日发布的 1.9.8_1515 的更改日志
{: #198_1515}

下表显示了补丁 1.9.8_1515 中包含的更改。
{: shortdesc}

<table summary="自 V1.9.7_1513 以来进行的更改">
<caption>自 V1.9.7_1513 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>V1.9.7</td>
<td>V1.9.8</td>
<td>请参阅 [Kubernetes 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.8)。</td>
</tr>
<tr>
<td>Kubernetes 配置</td>
<td>不适用</td>
<td>不适用</td>
<td>向集群的 Kubernetes API 服务器的 `--admission-control` 选项添加了 `PodSecurityPolicy`，并将集群配置为支持 pod 安全策略。有关更多信息，请参阅[配置 pod 安全策略](/docs/containers?topic=containers-psp)。</td>
</tr>
<tr>
<td>IBM Cloud Provider</td>
<td>V1.9.7-102</td>
<td>V1.9.8-141</td>
<td>更新为支持 Kubernetes 1.9.8 发行版。</td>
</tr>
<tr>
<td>OpenVPN 客户机</td>
<td>不适用</td>
<td>不适用</td>
<td>向在 <code>kube-system</code> 名称空间中运行的 OpenVPN 客户机 <code>vpn</code> 部署添加了 <code>livenessProbe</code>。</td>
</tr>
</tbody>
</table>


#### 2018 年 6 月 11 日发布的工作程序节点 FP1.9.7_1513 的更改日志
{: #197_1513}

下表显示了工作程序节点 FP1.9.7_1513 中包含的更改。
{: shortdesc}

<table summary="自 V1.9.7_1512 以来进行的更改">
<caption>自 V1.9.7_1512 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>内核更新</td>
<td>4.4.0-116</td>
<td>4.4.0-127</td>
<td>新工作程序节点映像具有 [CVE-2018-3639 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-3639) 的内核更新。</td>
</tr>
</tbody>
</table>

#### 2018 年 5 月 18 日发布的工作程序节点 FP1.9.7_1512 的更改日志
{: #197_1512}

下表显示了工作程序节点 FP1.9.7_1512 中包含的更改。
{: shortdesc}

<table summary="自 V1.9.7_1511 以来进行的更改">
<caption>自 V1.9.7_1511 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>不适用</td>
<td>不适用</td>
<td>修订解决了使用块存储器插件时发生的错误。</td>
</tr>
</tbody>
</table>

#### 2018 年 5 月 16 日发布的工作程序节点 FP1.9.7_1511 的更改日志
{: #197_1511}

下表显示了工作程序节点 FP1.9.7_1511 中包含的更改。
{: shortdesc}

<table summary="自 V1.9.7_1510 以来进行的更改">
<caption>自 V1.9.7_1510 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>不适用</td>
<td>不适用</td>
<td>存储在 `kubelet` 根目录中的数据现在保存在工作程序节点机器中更大的辅助磁盘上。</td>
</tr>
</tbody>
</table>

#### 2018 年 4 月 30 日发布的 1.9.7_1510 的更改日志
{: #197_1510}

下表显示了补丁 1.9.7_1510 中包含的更改。
{: shortdesc}

<table summary="自 V1.9.3_1506 以来进行的更改">
<caption>自 V1.9.3_1506 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>V1.9.3</td>
<td>V1.9.7</td>
<td><p>请参阅 [Kubernetes 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.7)。此发行版解决了 [CVE-2017-1002101 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002101) 和 [CVE-2017-1002102 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002102) 漏洞。</p><p><strong>注</strong>：现在，`secret`、`configMap`、`downwardAPI` 和投影卷均安装为只读。先前，应用程序可以将数据写入这些卷，但系统可能会自动还原数据。如果应用程序依赖于先前的不安全行为，请相应地对其进行修改。</p></td>
</tr>
<tr>
<td>Kubernetes 配置</td>
<td>不适用</td>
<td>不适用</td>
<td>向集群的 Kubernetes API 服务器的 `--runtime-config` 选项添加了 `admissionregistration.k8s.io/v1alpha1=true`。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>V1.9.3-71</td>
<td>V1.9.7-102</td>
<td>`NodePort` 和 `LoadBalancer` 服务现在通过将 `service.spec.externalTrafficPolicy` 设置为 `Local` 来支持[保留客户机源 IP](/docs/containers?topic=containers-loadbalancer#node_affinity_tolerations)。</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>修正旧集群的[边缘节点](/docs/containers?topic=containers-edge#edge)容忍度设置。</td>
</tr>
</tbody>
</table>

<br />


### V1.8 更改日志（不受支持）
{: #18_changelog}

查看 V1.8 更改日志。
{: shortdesc}

*   [2018 年 9 月 20 日发布的工作程序节点 FP1.8.15_1521 的更改日志](#1815_1521)
*   [2018 年 8 月 23 日发布的工作程序节点 FP1.8.15_1520 的更改日志](#1815_1520)
*   [2018 年 8 月 13 日发布的工作程序节点 FP1.8.15_1519 的更改日志](#1815_1519)
*   [2018 年 7 月 27 日发布的 1.8.15_1518 的更改日志](#1815_1518)
*   [2018 年 7 月 3 日发布的工作程序节点 FP1.8.13_1516 的更改日志](#1813_1516)
*   [2018 年 6 月 21 日发布的工作程序节点 FP1.8.13_1515 的更改日志](#1813_1515)
*   [2018 年 6 月 19 日发布的 1.8.13_1514 的更改日志](#1813_1514)
*   [2018 年 6 月 11 日发布的工作程序节点 FP1.8.11_1512 的更改日志](#1811_1512)
*   [2018 年 5 月 18 日发布的工作程序节点 FP1.8.11_1511 的更改日志](#1811_1511)
*   [2018 年 5 月 16 日发布的工作程序节点 FP1.8.11_1510 的更改日志](#1811_1510)
*   [2018 年 4 月 19 日发布的 1.8.11_1509 的更改日志](#1811_1509)

#### 2018 年 9 月 20 日发布的工作程序节点 FP1.8.15_1521 的更改日志
{: #1815_1521}

<table summary="自 V1.8.15_1520 以来进行的更改">
<caption>自 V1.8.15_1520 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>日志循环</td>
<td>不适用</td>
<td>不适用</td>
<td>切换到使用 `systemd` 计时器，而不使用 `cronjobs`，以防止 `logrotate` 因工作程序节点 90 天内未重新装入或更新而失败。**注**：在该次发行版的所有更早版本中，由于日志不循环，因此在定时作业失败后，主磁盘会填满。在工作程序节点 90 天内处于活动状态而未更新或重新装入后，定时作业会失败。如果日志填满整个主磁盘，那么工作程序节点将进入故障状态。使用 `ibmcloud ks worker-reload` [命令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)或 `ibmcloud ks worker-update` [命令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update)可以修复工作程序节点。</td>
</tr>
<tr>
<td>工作程序节点运行时组件（`kubelet`、`kube-proxy` 和 `docker`）</td>
<td>不适用</td>
<td>不适用</td>
<td>除去了运行时组件对主磁盘的依赖关系。此增强功能可防止工作程序节点在主磁盘填满时发生故障。</td>
</tr>
<tr>
<td>root 用户密码到期</td>
<td>不适用</td>
<td>不适用</td>
<td>由于合规性原因，工作程序节点的 root 用户密码会在 90 天后到期。如果自动化工具需要以 root 用户身份登录到工作程序节点，或者依赖于以 root 用户身份运行的定时作业，那么可以通过登录到工作程序节点并运行 `chage -M -1 root` 来禁用密码到期。**注**：如果您有阻止以 root 用户身份运行或阻止除去到期密码的安全合规性需求，请不要禁用到期。您可以改为至少每 90 天[更新](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update)或[重新装入](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)一次工作程序节点。</td>
</tr>
<tr>
<td>systemd</td>
<td>不适用</td>
<td>不适用</td>
<td>定期清除瞬态安装单元，以防止它们变得不受控制。此操作解决了 [Kubernetes 问题 57345 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/kubernetes/issues/57345)。</td>
</tr>
</tbody>
</table>

#### 2018 年 8 月 23 日发布的工作程序节点 FP1.8.15_1520 的更改日志
{: #1815_1520}

<table summary="自 V1.8.15_1519 以来进行的更改">
<caption>自 V1.8.15_1519 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>`systemd`</td>
<td>229</td>
<td>230</td>
<td>更新了 `systemd` 以修复 `cgroup` 泄漏。</td>
</tr>
<tr>
<td>内核</td>
<td>4.4.0-127</td>
<td>4.4.0-133</td>
<td>使用 [CVE-2018-3620、CVE-2018-3646 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://usn.ubuntu.com/3741-1/) 的内核更新对工作程序节点映像进行了更新。</td>
</tr>
</tbody>
</table>

#### 2018 年 8 月 13 日发布的工作程序节点 FP1.8.15_1519 的更改日志
{: #1815_1519}

<table summary="自 V1.8.15_1518 以来进行的更改">
<caption>自 V1.8.15_1518 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 软件包</td>
<td>不适用</td>
<td>不适用</td>
<td>已安装的 Ubuntu 软件包的更新。</td>
</tr>
</tbody>
</table>

#### 2018 年 7 月 27 日发布的 1.8.15_1518 的更改日志
{: #1815_1518}

<table summary="自 V1.8.13_1516 以来进行的更改">
<caption>自 V1.8.13_1516 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>V1.8.13-176</td>
<td>V1.8.15-204</td>
<td>更新为支持 Kubernetes 1.8.15 发行版。此外，LoadBalancer 服务 `create failure` 事件现在包含任何可移植子网错误。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 插件</td>
<td>320</td>
<td>334</td>
<td>将创建持久卷的超时从 15 分钟增加到 30 分钟。将缺省计费类型更改为 `hourly`。向预定义存储类添加了安装选项。在 IBM Cloud Infrastructure (SoftLayer) 帐户的 NFS 文件存储器实例中，已将**注释**字段更改为 JSON 格式，并添加了 PV 部署到的 Kubernetes 名称空间。为了支持多专区集群，向持久卷添加了专区和区域标签。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>V1.8.13</td>
<td>V1.8.15</td>
<td>请参阅 Kubernetes [发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/kubernetes/releases/tag/v1.8.15)。</td>
</tr>
<tr>
<td>内核</td>
<td>不适用</td>
<td>不适用</td>
<td>针对高性能联网工作负载对工作程序节点网络设置进行了小幅改进。</td>
</tr>
<tr>
<td>OpenVPN 客户机</td>
<td>不适用</td>
<td>不适用</td>
<td>在 `kube-system` 名称空间中运行的 OpenVPN 客户机 `vpn` 部署现在由 Kubernetes `addon-manager` 进行管理。</td>
</tr>
</tbody>
</table>

#### 2018 年 7 月 3 日发布的工作程序节点 FP1.8.13_1516 的更改日志
{: #1813_1516}

<table summary="自 V1.8.13_1515 以来进行的更改">
<caption>自 V1.8.13_1515 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>内核</td>
<td>不适用</td>
<td>不适用</td>
<td>针对高性能联网工作负载优化了 `sysctl`。</td>
</tr>
</tbody>
</table>


#### 2018 年 6 月 21 日发布的工作程序节点 FP1.8.13_1515 的更改日志
{: #1813_1515}

<table summary="自 V1.8.13_1514 以来进行的更改">
<caption>自 V1.8.13_1514 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Docker</td>
<td>不适用</td>
<td>不适用</td>
<td>对于未加密的机器类型，重新装入或更新工作程序节点时，将通过获取全新文件系统来清除辅助磁盘。</td>
</tr>
</tbody>
</table>

#### 2018 年 6 月 19 日发布的 1.8.13_1514 的更改日志
{: #1813_1514}

<table summary="自 V1.8.11_1512 以来进行的更改">
<caption>自 V1.8.11_1512 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>V1.8.11</td>
<td>V1.8.13</td>
<td>请参阅 [Kubernetes 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/kubernetes/releases/tag/v1.8.13)。</td>
</tr>
<tr>
<td>Kubernetes 配置</td>
<td>不适用</td>
<td>不适用</td>
<td>向集群的 Kubernetes API 服务器的 `--admission-control` 选项添加了 `PodSecurityPolicy`，并将集群配置为支持 pod 安全策略。有关更多信息，请参阅[配置 pod 安全策略](/docs/containers?topic=containers-psp)。</td>
</tr>
<tr>
<td>IBM Cloud Provider</td>
<td>V1.8.11-126</td>
<td>V1.8.13-176</td>
<td>更新为支持 Kubernetes 1.8.13 发行版。</td>
</tr>
<tr>
<td>OpenVPN 客户机</td>
<td>不适用</td>
<td>不适用</td>
<td>向在 <code>kube-system</code> 名称空间中运行的 OpenVPN 客户机 <code>vpn</code> 部署添加了 <code>livenessProbe</code>。</td>
</tr>
</tbody>
</table>


#### 2018 年 6 月 11 日发布的工作程序节点 FP1.8.11_1512 的更改日志
{: #1811_1512}

<table summary="自 V1.8.11_1511 以来进行的更改">
<caption>自 V1.8.11_1511 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>内核更新</td>
<td>4.4.0-116</td>
<td>4.4.0-127</td>
<td>新工作程序节点映像具有 [CVE-2018-3639 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-3639) 的内核更新。</td>
</tr>
</tbody>
</table>


#### 2018 年 5 月 18 日发布的工作程序节点 FP1.8.11_1511 的更改日志
{: #1811_1511}

<table summary="自 V1.8.11_1510 以来进行的更改">
<caption>自 V1.8.11_1510 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>不适用</td>
<td>不适用</td>
<td>修订解决了使用块存储器插件时发生的错误。</td>
</tr>
</tbody>
</table>

#### 2018 年 5 月 16 日发布的工作程序节点 FP1.8.11_1510 的更改日志
{: #1811_1510}

<table summary="自 V1.8.11_1509 以来进行的更改">
<caption>自 V1.8.11_1509 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>不适用</td>
<td>不适用</td>
<td>存储在 `kubelet` 根目录中的数据现在保存在工作程序节点机器中更大的辅助磁盘上。</td>
</tr>
</tbody>
</table>


#### 2018 年 4 月 19 日发布的 1.8.11_1509 的更改日志
{: #1811_1509}

<table summary="自 V1.8.8_1507 以来进行的更改">
<caption>自 V1.8.8_1507 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>V1.8.8</td>
<td>V1.8.11</td>
<td><p>请参阅 [Kubernetes 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/kubernetes/releases/tag/v1.8.11)。此发行版解决了 [CVE-2017-1002101 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002101) 和 [CVE-2017-1002102 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002102) 漏洞。</p><p>现在，`secret`、`configMap`、`downwardAPI` 和投影卷均安装为只读。先前，应用程序可以将数据写入这些卷，但系统可能会自动还原数据。如果应用程序依赖于先前的不安全行为，请相应地对其进行修改。</p></td>
</tr>
<tr>
<td>暂停容器映像</td>
<td>3.0</td>
<td>3.1</td>
<td>除去继承的孤立 zombie 进程。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>V1.8.8-86</td>
<td>V1.8.11-126</td>
<td>`NodePort` 和 `LoadBalancer` 服务现在通过将 `service.spec.externalTrafficPolicy` 设置为 `Local` 来支持[保留客户机源 IP](/docs/containers?topic=containers-loadbalancer#node_affinity_tolerations)。</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>修正旧集群的[边缘节点](/docs/containers?topic=containers-edge#edge)容忍度设置。</td>
</tr>
</tbody>
</table>

<br />


### V1.7 更改日志（不受支持）
{: #17_changelog}

查看 V1.7 更改日志。
{: shortdesc}

*   [2018 年 6 月 11 日发布的工作程序节点 FP1.7.16_1514 的更改日志](#1716_1514)
*   [2018 年 5 月 18 日发布的工作程序节点 FP1.7.16_1513 的更改日志](#1716_1513)
*   [2018 年 5 月 16 日发布的工作程序节点 FP1.7.16_1512 的更改日志](#1716_1512)
*   [2018 年 4 月 19 日发布的 1.7.16_1511 的更改日志](#1716_1511)

#### 2018 年 6 月 11 日发布的工作程序节点 FP1.7.16_1514 的更改日志
{: #1716_1514}

<table summary="自 V1.7.16_1513 以来进行的更改">
<caption>自 V1.7.16_1513 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>内核更新</td>
<td>4.4.0-116</td>
<td>4.4.0-127</td>
<td>新工作程序节点映像具有 [CVE-2018-3639 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-3639) 的内核更新。</td>
</tr>
</tbody>
</table>

#### 2018 年 5 月 18 日发布的工作程序节点 FP1.7.16_1513 的更改日志
{: #1716_1513}

<table summary="自 V1.7.16_1512 以来进行的更改">
<caption>自 V1.7.16_1512 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>不适用</td>
<td>不适用</td>
<td>修订解决了使用块存储器插件时发生的错误。</td>
</tr>
</tbody>
</table>

#### 2018 年 5 月 16 日发布的工作程序节点 FP1.7.16_1512 的更改日志
{: #1716_1512}

<table summary="自 V1.7.16_1511 以来进行的更改">
<caption>自 V1.7.16_1511 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>不适用</td>
<td>不适用</td>
<td>存储在 `kubelet` 根目录中的数据现在保存在工作程序节点机器中更大的辅助磁盘上。</td>
</tr>
</tbody>
</table>

#### 2018 年 4 月 19 日发布的 1.7.16_1511 的更改日志
{: #1716_1511}

<table summary="自 V1.7.4_1509 以来进行的更改">
<caption>自 V1.7.4_1509 以来的更改</caption>
<thead>
<tr>
<th>组件</th>
<th>上一个版本</th>
<th>当前版本</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>V1.7.4</td>
<td>V1.7.16</td>
<td><p>请参阅 [Kubernetes 发行说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/kubernetes/releases/tag/v1.7.16)。此发行版解决了 [CVE-2017-1002101 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002101) 和 [CVE-2017-1002102 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002102) 漏洞。</p><p>现在，`secret`、`configMap`、`downwardAPI` 和投影卷均安装为只读。先前，应用程序可以将数据写入这些卷，但系统可能会自动还原数据。如果应用程序依赖于先前的不安全行为，请相应地对其进行修改。</p></td>
</tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>V1.7.4-133</td>
<td>V1.7.16-17</td>
<td>`NodePort` 和 `LoadBalancer` 服务现在通过将 `service.spec.externalTrafficPolicy` 设置为 `Local` 来支持[保留客户机源 IP](/docs/containers?topic=containers-loadbalancer#node_affinity_tolerations)。</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>修正旧集群的[边缘节点](/docs/containers?topic=containers-edge#edge)容忍度设置。</td>
</tr>
</tbody>
</table>
