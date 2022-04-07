---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-07"

keywords: kubernetes, iks, helm

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


# IBM Cloud 服务和第三方集成
{: #ibm-3rd-party-integrations}

您可以使用 {{site.data.keyword.Bluemix_notm}} 平台和基础架构服务以及其他第三方集成来为集群添加额外功能。
{: shortdesc}

## IBM Cloud 服务
{: #ibm-cloud-services}

查看以下信息，以了解 {{site.data.keyword.Bluemix_notm}} 平台和基础架构服务如何与 {{site.data.keyword.containerlong_notm}} 集成以及如何在集群中使用这些服务。
{: shortdesc}

### IBM Cloud 平台服务
{: #platform-services}

支持服务密钥的所有 {{site.data.keyword.Bluemix_notm}} 平台服务都可以使用 {{site.data.keyword.containerlong_notm}} [服务绑定](/docs/containers?topic=containers-service-binding)进行集成。
{: shortdesc}

通过服务绑定，可以快速为 {{site.data.keyword.Bluemix_notm}} 服务创建服务凭证，并将这些凭证存储在集群的 Kubernetes 私钥中。Kubernetes 私钥在 etcd 中会自动加密，以保护您的数据。应用程序可以使用私钥中的凭证来访问 {{site.data.keyword.Bluemix_notm}} 服务实例。

不支持服务密钥的服务通常会提供可直接在应用程序中使用的 API。

要查找常用 {{site.data.keyword.Bluemix_notm}} 服务的概述，请参阅[常用集成](/docs/containers?topic=containers-supported_integrations#popular_services)。

### IBM Cloud 基础架构服务
{: #infrastructure-services}

由于 {{site.data.keyword.containerlong_notm}} 允许您创建基于 {{site.data.keyword.Bluemix_notm}} 基础架构的 Kubernetes 集群，因此某些基础架构服务（例如，Virtual Servers、Bare Metal Servers 或 VLAN）已完全集成到 {{site.data.keyword.containerlong_notm}} 中。您可以使用 {{site.data.keyword.containerlong_notm}} API、CLI 或控制台来创建和使用这些服务实例。
{: shortdesc}

支持的持久存储解决方案（例如，{{site.data.keyword.Bluemix_notm}} File Storage、{{site.data.keyword.Bluemix_notm}} Block Storage 或 {{site.data.keyword.cos_full}}）作为 Kubernetes Flex 驱动程序集成，并且可以使用 [Helm chart](/docs/containers?topic=containers-helm) 进行设置。Helm chart 会自动设置集群中的 Kubernetes 存储类、存储器提供者和存储器驱动程序。可以使用存储类来通过持久卷声明 (PVC) 供应持久性存储器。

要保护集群网络或连接到内部部署数据中心，可以配置下列其中一个选项：
- [strongSwan IPSec VPN 服务](/docs/containers?topic=containers-vpn#vpn-setup)
- [{{site.data.keyword.BluDirectLink}}](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link)
- [虚拟路由器设备 (VRA)](/docs/containers?topic=containers-vpn#vyatta)
- [Fortigate Security Appliance (FSA)](/docs/services/vmwaresolutions/services?topic=vmware-solutions-fsa_considerations)

## Kubernetes 社区和开放式源代码集成
{: #kube-community-tools}

由于您拥有在 {{site.data.keyword.containerlong_notm}} 中创建的标准集群，因此可以选择安装第三方解决方案以向集群添加额外功能。
{: shortdesc}

某些开放式源代码技术（例如，Knative、Istio、LogDNA、Sysdig 或 Portworx）已经过 IBM 测试，并作为受管附加组件、Helm chart 或 {{site.data.keyword.Bluemix_notm}} 服务（由与 IBM 合作的服务提供者运行）提供。这些开放式源代码工具完全集成到 {{site.data.keyword.Bluemix_notm}} 计费和支持系统中。

可以在集群中安装其他开放式源代码工具，但这些工具可能无法在 {{site.data.keyword.containerlong_notm}} 中进行管理、支持或验证。

### 通过合作运行集成
{: #open-source-partners}

有关 {{site.data.keyword.containerlong_notm}} 合作伙伴及其提供的每种解决方案的优点的更多信息，请参阅 [{{site.data.keyword.containerlong_notm}} 合作伙伴](/docs/containers?topic=containers-service-partners)。

### 受管附加组件
{: #cluster-add-ons}
{{site.data.keyword.containerlong_notm}} 使用[受管附加组件](/docs/containers?topic=containers-managed-addons)集成了常用开放式源代码集成，例如 [Knative](/docs/containers?topic=containers-serverless-apps-knative) 或 [Istio](/docs/containers?topic=containers-istio)。通过受管附加组件，可在集群中轻松安装已经过 IBM 测试并核准在 {{site.data.keyword.containerlong_notm}} 中使用的开放式源代码工具。

受管附加组件完全集成到 {{site.data.keyword.Bluemix_notm}} 支持组织中。如果您在使用受管附加组件时有疑问或遇到问题，可以使用其中一个 {{site.data.keyword.containerlong_notm}} 支持通道。有关更多信息，请参阅[获取帮助和支持](/docs/containers?topic=containers-cs_troubleshoot_clusters#clusters_getting_help)。

如果添加到集群的工具发生了成本，这些成本会自动合并，并列入每月 {{site.data.keyword.Bluemix_notm}} 计费中。计费周期由 {{site.data.keyword.Bluemix_notm}} 根据您在集群中启用附加组件的时间来确定。

### 其他第三方集成
{: #kube-community-helm}

您可以安装任何与 Kubernetes 集成的第三方开放式源代码工具。例如，Kubernetes 社区指定了某些 Helm chart `stable` 或 `incubator`。请注意，这些 chart 或工具是否适合用于 {{site.data.keyword.containerlong_notm}} 并未经过验证。如果工具需要许可证，那么必须先购买许可证，然后才能使用该工具。有关 Kubernetes 社区的可用 Helm chart 的概述，请参阅 [Helm chart ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/kubernetes/solutions/helm-charts) 目录中的 `kubernetes` 和 `kubernetes-incubator` 存储库。
{: shortdesc}

使用第三方开放式源代码集成所发生的任何成本都不会包含在每月 {{site.data.keyword.Bluemix_notm}} 帐单中。

从 Kubernetes 社区安装第三方开放式源代码集成或 Helm chart 可能会更改缺省集群配置，并且会使集群进入不受支持的状态。如果使用其中任何工具时遇到问题，请直接咨询 Kubernetes 社区或服务提供者。
{: important}
