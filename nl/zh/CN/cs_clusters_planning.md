---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: kubernetes, iks, multi az, multi-az, szr, mzr

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


# 规划集群网络设置
{: #plan_clusters}

为 {{site.data.keyword.containerlong}} 集群设计网络设置，以满足工作负载和环境的需求。
{: shortdesc}

在 {{site.data.keyword.containerlong_notm}} 集群中，容器化应用程序在称为工作程序节点的计算主机上托管。工作程序节点由 Kubernetes 主节点进行管理。工作程序节点与 Kubernetes 主节点、其他服务、因特网或其他专用网络之间的通信设置取决于如何设置 IBM Cloud Infrastructure (SoftLayer) 网络。

是首次创建集群？请首先试用[教程](/docs/containers?topic=containers-cs_cluster_tutorial)，然后在准备好规划生产就绪型集群后返回此处。
{: tip}

要规划集群网络设置，请首先[了解集群网络基本信息](#plan_basics)。然后，可以查看适用于基于环境的场景的三个潜在集群网络设置，包括[运行面向因特网的应用程序工作负载](#internet-facing)、[使用受限公共访问权扩展内部部署数据中心](#limited-public)和[仅在专用网络上扩展内部部署数据中心](#private_clusters)。

## 了解集群网络基本信息
{: #plan_basics}

创建集群时，必须选择联网设置，以便特定集群组件可以相互通信，并且能与集群外部的网络或服务进行通信。
{: shortdesc}

* [工作程序到工作程序的通信](#worker-worker)：所有工作程序节点都必须能够在专用网络上相互通信。在许多情况下，必须允许跨多个专用 VLAN 进行通信，以支持不同 VLAN 和不同专区上的工作程序相互连接。
* [工作程序与主节点的通信以及用户与主节点的通信](#workeruser-master)：工作程序节点和授权集群用户可以通过 TLS 在公用网络上或通过专用服务端点在专用网络上安全地与 Kubernetes 主节点进行通信。
* [工作程序与其他 {{site.data.keyword.Bluemix_notm}} 服务或内部部署网络的通信](#worker-services-onprem)：允许工作程序节点安全地与其他 {{site.data.keyword.Bluemix_notm}} 服务（例如，{{site.data.keyword.registrylong}}）以及与内部部署网络进行通信。
* [与在工作程序节点上运行的应用程序的外部通信](#external-workers)：允许向集群发出公共或专用请求，以及从集群向公共端点发出请求。

### 工作程序到工作程序的通信
{: #worker-worker}

创建集群时，集群的工作程序节点会自动连接到专用 VLAN，并且可以选择连接到公用 VLAN。VLAN 会将一组工作程序节点和 pod 视为连接到同一物理连线那样进行配置，并为工作程序之间的连接提供通道。
{: shortdesc}

**工作程序节点的 VLAN 连接**</br>
所有工作程序节点都必须连接到专用 VLAN，以便每个工作程序节点都可以向其他工作程序节点发送信息以及接收来自其他工作程序节点的信息。创建具有同时连接到公用 VLAN 的工作程序节点的集群时，工作程序节点可以通过公用 VLAN 和专用 VLAN（如果启用了专用服务端点）自动与 Kubernetes 主节点进行通信。公用 VLAN 还会提供公用网络连接，以便可以将集群中的应用程序公开到因特网。但是，如果需要保护应用程序不被公共接口访问，那么有若干选项可用于保护集群，例如使用 Calico 网络策略或将外部网络工作负载隔离到边缘工作程序节点。
* 免费集群：在免费集群中，缺省情况下集群的工作程序节点会连接到 IBM 拥有的公用 VLAN 和专用 VLAN。因为是 IBM 控制 VLAN、子网和 IP 地址，所以无法创建多专区集群或向集群添加子网，而只能使用 NodePort 服务来公开应用程序。</dd>
* 标准集群：在标准集群中，首次在某个专区中创建集群时，会自动在 IBM Cloud Infrastructure (SoftLayer) 帐户中供应该专区中的公用 VLAN 和专用 VLAN。如果指定工作程序节点必须仅连接到专用 VLAN，那么将自动供应该专区中的专用 VLAN。对于在该专区中创建的每个后续集群，可以指定要使用的 VLAN 对。可以复用为您创建的相同公用和专用 VLAN，因为多个集群可以共享 VLAN。
有关 VLAN、子网和 IP 地址的更多信息，请参阅 [{{site.data.keyword.containerlong_notm}} 中的联网概述](/docs/containers?topic=containers-subnets#basics)。

**工作程序节点跨子网和 VLAN 进行通信**</br>
在多种情况下，必须允许集群中的组件跨多个专用 VLAN 进行通信。例如，如果要创建多专区集群，如果集群有多个 VLAN，或者如果同一 VLAN 上有多个子网，那么同一 VLAN 中不同子网上的工作程序节点或不同 VLAN 中的工作程序节点无法自动相互通信。您必须为 IBM Cloud Infrastructure (SoftLayer) 帐户启用虚拟路由和转发 (VRF) 或 VLAN 生成。

* [虚拟路由和转发 (VRF)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud)：VRF 支持基础架构帐户中的所有专用 VLAN 和子网相互通信。此外，需要 VRF 来允许工作程序与主节点通过专用服务端点进行通信，以及与支持专用服务端点的其他 {{site.data.keyword.Bluemix_notm}} 实例进行通信。要启用 VRF，请运行 `ibmcloud account update --service-endpoint-enable true`。此命令输出将提示您开具支持案例，以便为帐户启用 VRF 和服务端点。VRF 会使帐户的 VLAN 生成选项失效，因为所有 VLAN 都能够进行通信。</br></br>启用 VRF 后，任何连接到同一 {{site.data.keyword.Bluemix_notm}} 帐户中的任何专用 VLAN 的系统都可以与集群工作程序节点进行通信。您可以通过应用 [Calico 专用网络策略](/docs/containers?topic=containers-network_policies#isolate_workers)，将集群与专用网络上的其他系统相隔离。</dd>
* [VLAN 生成](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning)：如果无法或者不希望启用 VRF（例如，如果不需要主节点在专用网络上可供访问，或者如果是使用网关设备通过公用 VLAN 来访问主节点），请启用 VLAN 生成。例如，如果您有现有网关设备，然后添加了集群，那么不会在该网关设备上配置为集群订购的新可移植子网，但 VLAN 生成会启用子网之间的路由。要启用 VLAN 生成，您需要**网络 > 管理网络 VLAN 生成**[基础架构许可权](/docs/containers?topic=containers-users#infra_access)，或者可以请求帐户所有者来启用 VLAN 生成。要检查是否已启用 VLAN 生成，请使用 `ibmcloud ks vlan-spanning-get` [命令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get)。如果选择启用 VLAN 生成而不是 VRF，那么无法启用专用服务端点。

</br>

### 工作程序与主节点的通信以及用户与主节点的通信
{: #workeruser-master}

必须设置通信信道，以便工作程序节点可以与 Kubernetes 主节点建立连接。您可通过启用仅公共服务端点、公共和专用服务端点或仅专用服务端点，允许工作程序节点与 Kubernetes 主节点进行通信。
{: shortdesc}

为了保护通过公共和专用服务端点进行的通信，在创建集群时，{{site.data.keyword.containerlong_notm}} 会自动设置 Kubernetes 主节点与工作程序节点之间的 OpenVPN 连接。工作程序通过 TLS 证书与主节点进行安全对话，主节点通过 OpenVPN 连接与工作程序进行对话。

**仅公共服务端点**</br>
如果您不希望或无法为帐户启用 VRF，那么工作程序节点可以通过公共服务端点在公用 VLAN 上自动连接到 Kubernetes 主节点。
* 工作程序节点和主节点之间的通信通过公共服务端点在公用网络上安全地建立。
* 主节点仅可供授权集群用户通过公共服务端点公共访问。例如，集群用户可以安全地通过因特网访问 Kubernetes 主节点，以运行 `kubectl` 命令。

**公共和专用服务端点**</br>
要使主节点可供集群用户公共或专用访问，可以启用公共和专用服务端点。{{site.data.keyword.Bluemix_notm}} 帐户中需要 VRF，因此必须为帐户启用服务端点。要启用 VRF 和服务端点，请运行 `ibmcloud account update --service-endpoint-enable true`。
* 如果工作程序节点连接到公用和专用 VLAN，那么工作程序节点与主节点之间的通信将通过专用服务端点在专用网络上建立，以及通过公共服务端点在公用网络上建立。通过将工作程序与主节点的流量一半通过公共端点路由，一半通过专用端点路由，可保护主节点到工作程序的通信不受公共或专用网络潜在中断的影响。如果工作程序节点仅连接到专用 VLAN，那么工作程序节点与主节点之间的通信将仅通过专用服务端点在专用网络上建立。
* 主节点可供授权集群用户通过公共服务端点公共访问。如果授权集群用户位于 {{site.data.keyword.Bluemix_notm}} 专用网络中，或者通过 VPN 连接或 {{site.data.keyword.Bluemix_notm}} Direct Link 与专用网络连接，那么主节点可通过专用服务端点供专用访问。请注意，您必须[通过专用负载均衡器公开主节点端点](/docs/containers?topic=containers-clusters#access_on_prem)，以便用户可以通过 VPN 或 {{site.data.keyword.Bluemix_notm}} Direct Link 连接访问主节点。

**仅专用服务端点**</br>
要使主节点仅可供专用访问，可以启用专用服务端点。{{site.data.keyword.Bluemix_notm}} 帐户中需要 VRF，因此必须为帐户启用服务端点。要启用 VRF 和服务端点，请运行 `ibmcloud account update --service-endpoint-enable true`。请注意，使用仅专用服务端点不会发生任何计费或计量的带宽费用。
* 工作程序节点与主节点之间的通信通过专用服务端点在专用网络上建立。
* 如果授权集群用户位于 {{site.data.keyword.Bluemix_notm}} 专用网络中，或者通过 VPN 连接或 DirectLink 与专用网络连接，那么主节点可供专用访问。请注意，您必须[通过专用负载均衡器公开主节点端点](/docs/containers?topic=containers-clusters#access_on_prem)，以便用户可以通过 VPN 或 DirectLink 连接访问主节点。

</br>

### 工作程序与其他 {{site.data.keyword.Bluemix_notm}} 服务或内部部署网络的通信
{: #worker-services-onprem}

允许工作程序节点安全地与其他 {{site.data.keyword.Bluemix_notm}} 服务（例如，{{site.data.keyword.registrylong}}）以及与内部部署网络进行通信。
{: shortdesc}

**与其他 {{site.data.keyword.Bluemix_notm}} 服务通过专用或公用网络进行通信**</br>
工作程序节点可以通过 IBM Cloud Infrastructure (SoftLayer) 专用网络，自动、安全地与支持专用服务端点的其他 {{site.data.keyword.Bluemix_notm}} 服务（例如，{{site.data.keyword.registrylong}}）进行通信。如果 {{site.data.keyword.Bluemix_notm}} 服务不支持专用服务端点，那么工作程序节点必须连接到公用 VLAN，这样才能通过公用网络安全地与服务进行通信。

如果使用 Calico 网络策略来锁定集群中的公用网络，那么可能需要允许访问要在 Calico 策略中使用的服务的公共和专用 IP 地址。如果使用的是网关设备，例如虚拟路由器设备 (Vyatta)，那么必须[允许访问要使用的服务的专用 IP 地址](/docs/containers?topic=containers-firewall#firewall_outbound)（这是要在网关设备防火墙中使用的服务）。
{: note}

**使用 {{site.data.keyword.BluDirectLink}} 通过专用网络与内部部署数据中心内的资源进行通信**</br>
要将集群与内部部署数据中心（例如，与 {{site.data.keyword.icpfull_notm}}）相连接，可以设置 [{{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link)。通过 {{site.data.keyword.Bluemix_notm}} Direct Link，您可以在远程网络环境和 {{site.data.keyword.containerlong_notm}} 之间创建直接专用连接，而无需通过公用因特网进行路由。

**使用 strongSwan IPSec VPN 连接通过公用网络与内部部署数据中心内的资源进行通信**
* 连接到公用和专用 VLAN 的工作程序节点：直接在集群中设置 [strongSwan IPSec VPN 服务 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.strongswan.org/about.html)。strongSwan IPSec VPN 服务基于业界标准因特网协议安全性 (IPSec) 协议组，通过因特网提供安全的端到端通信信道。要在集群与内部部署网络之间设置安全连接，请在集群的 pod 中直接[配置和部署 strongSwan IPSec VPN 服务](/docs/containers?topic=containers-vpn#vpn-setup)。
* 仅连接到专用 VLAN 的工作程序节点：在网关设备上设置 IPSec VPN 端点，例如虚拟路由器设备 (Vyatta)。然后，在集群中[配置 strongSwan IPSec VPN 服务](/docs/containers?topic=containers-vpn#vpn-setup)，以在网关上使用 VPN 端点。如果不想使用 strongSwan，那么可以[直接使用 VRA 设置 VPN 连接](/docs/containers?topic=containers-vpn#vyatta)。


</br>

### 与在工作程序节点上运行的应用程序的外部通信
{: #external-workers}

允许从集群外部向在工作程序节点上运行的应用程序发出公共或专用流量请求。
{: shortdesc}

**流至集群应用程序的专用流量**</br>
在集群中部署应用程序后，您可能希望使应用程序仅可供位于集群所在专用网络上的用户和服务访问。专用负载均衡非常适用于使应用程序可供集群外部的请求使用，而无需向一般公众公开应用程序。还可以使用专用负载均衡来测试访问，请求路由以及对应用程序进行其他配置后，再使用公用网络服务向公众公开应用程序。要允许从集群外部向应用程序发出专用流量请求，可以创建专用 Kubernetes 联网服务，例如专用 NodePort、NLB 和 Ingress ALB。然后可以使用 Calico DNAT 前策略来阻止流至专用联网服务的公共 NodePort 的流量。有关更多信息，请参阅[规划专用外部负载均衡](/docs/containers?topic=containers-cs_network_planning#private_access)。

**流至集群应用程序的公共流量**</br>
要使应用程序可供从公共因特网进行外部访问，可以创建公共 NodePort、网络负载均衡器 (NLB) 和 Ingress 应用程序负载均衡器 (ALB)。公用联网服务通过向应用程序提供公共 IP 地址和（可选）公共 URL 来连接到此公用网络接口。应用程序以公共方式公开时，具有公共服务 IP 地址或为应用程序设置的 URL 的任何人都可以向应用程序发送请求。然后，可以使用 Calico DNAT 前策略来控制流至公用联网服务的流量，例如仅将来自特定源 IP 地址或 CIDR 的流量列入白名单，而阻塞其他所有流量。有关更多信息，请参阅[规划公共外部负载均衡](/docs/containers?topic=containers-cs_network_planning#private_access)。

要获得额外的安全性，请将联网工作负载隔离到边缘工作程序节点。边缘工作程序节点通过减少允许外部访问的连接到公用 VLAN 的工作程序节点，并隔离联网工作负载，可以提高集群的安全性。[将工作程序节点标注为边缘节点](/docs/containers?topic=containers-edge#edge_nodes)时，NLB 和 ALB pod 会仅部署到这些指定的工作程序节点。此外，要阻止其他工作负载在边缘节点上运行，可以[感染边缘节点](/docs/containers?topic=containers-edge#edge_workloads)。在 Kubernetes V1.14 和更高版本中，可以将公共和专用 NLB 和 ALB 部署到边缘节点。例如，如果工作程序节点仅连接到专用 VLAN，但您需要允许对集群中的应用程序进行公共访问，那么可以创建边缘工作程序池，其中边缘节点连接到公用和专用 VLAN。可以将公共 NLB 和 ALB 部署到这些边缘节点，以确保只有这些工作程序可处理公共连接。

如果工作程序节点仅连接到专用 VLAN，并且您使用网关设备来提供工作程序节点和集群主节点之间的通信，那么还可以将该设备配置为公共或专用防火墙。要允许从集群外部向应用程序发出公共或专用流量请求，可以创建公共或专用 NodePort、NLB 和 Ingress ALB。然后，必须在网关设备防火墙中[打开必需的端口和 IP 地址](/docs/containers?topic=containers-firewall#firewall_inbound)，以允许通过公用或专用网络流至这些服务的入站流量。
{: note}

<br />


## 场景：在集群中运行面向因特网的应用程序工作负载
{: #internet-facing}

在此场景中，您希望在集群中运行的工作负载可供来自因特网的请求访问，以便最终用户可以访问应用程序。您希望使用的选项是在集群中隔离公共访问，并控制允许向集群发出哪些公共请求。此外，工作程序对要连接到集群的任何 {{site.data.keyword.Bluemix_notm}} 服务都具有自动访问权。
{: shortdesc}

<p>
<figure>
 <img src="images/cs_clusters_planning_internet.png" alt="运行面向因特网的工作负载的集群的体系结构图"/>
 <figcaption>运行面向因特网的工作负载的集群的体系结构</figcaption>
</figure>
</p>

要实现此设置，请通过将工作程序节点连接到公用和专用 VLAN 来创建集群。

如果创建了使用公用和专用 VLAN 的集群，那么日后无法从该集群中除去所有公用 VLAN。从集群除去所有公用 VLAN 将导致多个集群组件停止工作。请改为创建仅连接到专用 VLAN 的新工作程序池。
{: note}

可以选择允许通过公用和专用网络或仅通过公用网络进行工作程序与主节点的通信以及用户与主节点的通信。
* 公共和专用服务端点：帐户必须启用了 VRF 并支持使用服务端点。工作程序节点与主节点之间的通信通过专用服务端点在专用网络上建立，以及通过公共服务端点在公用网络上建立。主节点可供授权集群用户通过公共服务端点公共访问。
* 公共服务端点：如果您不希望或无法为帐户启用 VRF，那么工作程序节点和授权集群用户可以通过公共服务端点在公用网络上自动连接到 Kubernetes 主节点。

工作程序节点可以自动、安全地与支持专用服务端点的其他 {{site.data.keyword.Bluemix_notm}} 服务通过 IBM Cloud Infrastructure (SoftLayer) 专用网络进行通信。如果 {{site.data.keyword.Bluemix_notm}} 服务不支持专用服务端点，那么工作程序可以通过公用网络安全地与服务进行通信。可以将 Calico 网络策略用于公用网络或专用网络隔离，以锁定工作程序节点的公共或专用接口。您可能需要允许访问要在这些 Calico 隔离策略中使用的服务的公共和专用 IP 地址。

要将集群中的应用程序公开到因特网，可以创建公共网络负载均衡器 (NLB) 或 Ingress 应用程序负载均衡器 (ALB) 服务。通过创建标注为边缘节点的工作程序节点的池，可以提高集群的安全性。公用网络服务的 pod 会部署到边缘节点，以便将外部流量工作负载隔离到集群中的仅少数几个工作程序。您可以通过创建 Calico DNAT 前策略（例如，白名单和黑名单策略），进一步控制流至用于公开应用程序的网络服务的公共流量。

如果工作程序节点需要访问 {{site.data.keyword.Bluemix_notm}} 帐户外部的专用网络中的服务，那么可以在集群中配置并部署 strongSwan IPSec VPN 服务，或者利用 {{site.data.keyword.Bluemix_notm}} Direct Link 服务来连接到这些网络。

准备好开始将集群用于此方案了吗？规划[高可用性](/docs/containers?topic=containers-ha_clusters)和[工作程序节点](/docs/containers?topic=containers-planning_worker_nodes)设置后，请参阅[创建集群](/docs/containers?topic=containers-clusters#cluster_prepare)。

<br />


## 场景：将内部部署数据中心扩展到专用网络上的集群，并添加受限公共访问权
{: #limited-public}

在此场景中，您希望在集群中运行的工作负载可供内部部署数据中心内的服务、数据库或其他资源访问。但是，您可能需要提供对集群的受限公共访问权，并且希望确保在集群中控制和隔离任何公共访问。例如，您可能需要工作程序访问不支持专用服务端点而必须通过公用网络进行访问的 {{site.data.keyword.Bluemix_notm}} 服务。或者，您可能需要提供对集群中运行的应用程序的受限公共访问权。
{: shortdesc}

要实现此集群设置，可以[使用边缘节点和 Calico 网络策略](#calico-pc)或[使用网关设备](#vyatta-gateway)来创建防火墙。

### 使用边缘节点和 Calico 网络策略
{: #calico-pc}

通过将边缘节点用作公共网关，并将 Calico 网络策略用作公共防火墙，允许与集群建立受限公共连接。
{: shortdesc}

<p>
<figure>
 <img src="images/cs_clusters_planning_calico.png" alt="使用边缘节点和 Calico 网络策略进行安全公共访问的集群的体系结构图"/>
 <figcaption>使用边缘节点和 Calico 网络策略进行安全公共访问的集群的体系结构</figcaption>
</figure>
</p>

使用此设置时，可通过将工作程序节点仅连接到专用 VLAN 来创建集群。帐户必须启用了 VRF 并支持使用专用服务端点。

如果授权集群用户位于 {{site.data.keyword.Bluemix_notm}} 专用网络中，或者通过 [VPN 连接](/docs/infrastructure/iaas-vpn?topic=VPN-gettingstarted-with-virtual-private-networking)或 [{{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link) 与专用网络连接，那么 Kubernetes 主节点可通过专用服务端点进行访问。但是，与 Kubernetes 主节点通过专用服务端点进行的通信必须经过 <code>166.X.X.X</code> IP 地址范围，这不能通过 VPN 连接或 {{site.data.keyword.Bluemix_notm}} Direct Link 进行路由。可以通过使用专用网络负载均衡器 (NLB) 来为集群用户公开主节点的专用服务端点。专用 NLB 将主节点的专用服务端点作为内部 <code>10.X.X.X</code> IP 地址范围公开，用户可以使用 VPN 或 {{site.data.keyword.Bluemix_notm}} Direct Link 连接对这些地址进行访问。如果仅启用专用服务端点，那么可以使用 Kubernetes 仪表板或临时启用公共服务端点来创建专用 NLB。

接下来，可以创建连接到公用和专用 VLAN 并标注为边缘节点的工作程序节点的池。边缘节点通过仅允许外部访问少数几个工作程序节点，并将联网工作负载隔离到这些工作程序，可以提高集群的安全性。

工作程序节点可以自动、安全地与支持专用服务端点的其他 {{site.data.keyword.Bluemix_notm}} 服务通过 IBM Cloud Infrastructure (SoftLayer) 专用网络进行通信。如果 {{site.data.keyword.Bluemix_notm}} 服务不支持专用服务端点，那么连接到公用 VLAN 的边缘节点可以通过公用网络安全地与服务进行通信。可以将 Calico 网络策略用于公用网络或专用网络隔离，以锁定工作程序节点的公共或专用接口。您可能需要允许访问要在这些 Calico 隔离策略中使用的服务的公共和专用 IP 地址。

要提供对集群中应用程序的专用访问权，可以创建专用网络负载均衡器 (NLB) 或 Ingress 应用程序负载均衡器 (ALB)，以仅将应用程序公开到专用网络。通过创建 Calico DNAT 前策略（例如，用于阻止工作程序节点上的公共 NodePort 的策略），可以阻止流至用于公开应用程序的这些网络服务的所有公共流量。如果需要提供对集群中应用程序的受限公共访问权，那么可以创建公共 NLB 或 ALB 来公开应用程序。然后，必须将应用程序部署到这些边缘节点，以便 NLB 或 ALB 可以将公共流量定向到应用程序 pod。您可以通过创建 Calico DNAT 前策略（例如，白名单和黑名单策略），进一步控制流至用于公开应用程序的网络服务的公共流量。专用和公用网络服务的 pod 会部署到边缘节点，以便将外部流量工作负载限制为仅流至集群中的少数几个工作程序。  

要安全地访问 {{site.data.keyword.Bluemix_notm}} 外部和其他内部部署网络上的服务，可以在集群中配置并部署 strongSwan IPSec VPN 服务。strongSwan 负载均衡器 pod 会部署到边缘池中的一个工作程序，其中 pod 会通过加密 VPN 隧道在公用网络上建立与内部部署网络的安全连接。或者，可以使用 {{site.data.keyword.Bluemix_notm}} Direct Link 服务仅通过专用网络将集群连接到内部部署数据中心。

准备好开始将集群用于此方案了吗？规划[高可用性](/docs/containers?topic=containers-ha_clusters)和[工作程序节点](/docs/containers?topic=containers-planning_worker_nodes)设置后，请参阅[创建集群](/docs/containers?topic=containers-clusters#cluster_prepare)。

</br>

### 使用网关设备
{: #vyatta-gateway}

通过将网关设备（例如，虚拟路由器设备 (Vyatta)）配置为公共网关和防火墙，允许与集群建立受限公共连接。
{: shortdesc}

<p>
<figure>
 <img src="images/cs_clusters_planning_gateway.png" alt="使用网关设备进行安全公共访问的集群的体系结构图"/>
 <figcaption>使用网关设备进行安全公共访问的集群的体系结构</figcaption>
</figure>
</p>

如果仅在专用 VLAN 上设置了工作程序节点，并且您不希望或无法为帐户启用 VRF，那么必须配置网关设备，以通过公用网络在工作程序节点与主节点之间提供网络连接。例如，可以选择设置[虚拟路由器设备](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-about-the-vra)或 [Fortigate Security Appliance](/docs/services/vmwaresolutions/services?topic=vmware-solutions-fsa_considerations)。

可以使用定制网络策略来设置网关设备，以便为集群提供专用网络安全性，检测网络侵入并进行补救。在公用网络上设置防火墙时，必须为每个区域打开必需的端口和专用 IP 地址，以便主节点和工作程序节点可以通信。如果还为专用网络配置了此防火墙，那么还必须打开必需的端口和专用 IP 地址，以允许工作程序节点之间的通信，并且使集群通过专用网络访问基础架构资源。此外，还必须为帐户启用 VLAN 生成，以便子网可以在同一 VLAN 上以及不同 VLAN 之间进行路由。

要将工作程序节点和应用程序安全地连接到内部部署网络或 {{site.data.keyword.Bluemix_notm}} 外部的服务，请在网关设备上设置 IPSec VPN 端点，并在集群中设置 strongSwan IPSec VPN 服务以使用网关 VPN 端点。如果不想使用 strongSwan，那么可以使用 VRA 直接设置 VPN 连接。

工作程序节点可以通过网关设备与其他 {{site.data.keyword.Bluemix_notm}} 服务和 {{site.data.keyword.Bluemix_notm}} 外部的公共服务安全地进行通信。可以将防火墙配置为仅允许访问要使用的服务的公共和专用 IP 地址。

要提供对集群中应用程序的专用访问权，可以创建专用网络负载均衡器 (NLB) 或 Ingress 应用程序负载均衡器 (ALB)，以仅将应用程序公开到专用网络。如果需要提供对集群中应用程序的受限公共访问权，那么可以创建公共 NLB 或 ALB 来公开应用程序。由于所有流量都流经网关设备防火墙，因此可以通过在防火墙中打开服务的端口和 IP 地址来允许流至用于公开应用程序的网络服务的入站流量，以控制流至这些服务的公共和专用流量。

准备好开始将集群用于此方案了吗？规划[高可用性](/docs/containers?topic=containers-ha_clusters)和[工作程序节点](/docs/containers?topic=containers-planning_worker_nodes)设置后，请参阅[创建集群](/docs/containers?topic=containers-clusters#cluster_prepare)。

<br />


## 场景：将内部部署数据中心扩展到专用网络上的集群
{: #private_clusters}

在此场景中，您希望在 {{site.data.keyword.containerlong_notm}} 集群中运行工作负载。但是，您希望这些工作负载仅可供内部部署数据中心内的服务、数据库或其他资源（例如，{{site.data.keyword.icpfull_notm}}）访问。集群工作负载可能需要访问支持通过专用网络进行通信的其他若干 {{site.data.keyword.Bluemix_notm}} 服务，例如 {{site.data.keyword.cos_full_notm}}。
{: shortdesc}

<p>
<figure>
 <img src="images/cs_clusters_planning_extend.png" alt="在专用网络上连接到内部部署数据中心的集群的体系结构图"/>
 <figcaption>在专用网络上连接到内部部署数据中心的集群的体系结构</figcaption>
</figure>
</p>

要实现此设置，可通过将工作程序节点仅连接到专用 VLAN 来创建集群。要仅通过专用服务端点在专用网络上提供集群主节点和工作程序节点之间的连接，帐户必须启用 VRF 并支持使用服务端点。由于启用了 VRF 时专用网络上的任何资源都可看到集群，因此可以通过应用 Calico 专用网络策略，将集群与专用网络上的其他系统相隔离。

如果授权集群用户位于 {{site.data.keyword.Bluemix_notm}} 专用网络中，或者通过 [VPN 连接](/docs/infrastructure/iaas-vpn?topic=VPN-gettingstarted-with-virtual-private-networking)或 [{{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link) 与专用网络连接，那么 Kubernetes 主节点可通过专用服务端点进行访问。但是，与 Kubernetes 主节点通过专用服务端点进行的通信必须经过 <code>166.X.X.X</code> IP 地址范围，这不能通过 VPN 连接或 {{site.data.keyword.Bluemix_notm}} Direct Link 进行路由。可以通过使用专用网络负载均衡器 (NLB) 来为集群用户公开主节点的专用服务端点。专用 NLB 将主节点的专用服务端点作为内部 <code>10.X.X.X</code> IP 地址范围公开，用户可以使用 VPN 或 {{site.data.keyword.Bluemix_notm}} Direct Link 连接对这些地址进行访问。如果仅启用专用服务端点，那么可以使用 Kubernetes 仪表板或临时启用公共服务端点来创建专用 NLB。

工作程序节点可以自动、安全地与支持专用服务端点的其他 {{site.data.keyword.Bluemix_notm}} 服务（例如，{{site.data.keyword.registrylong}}）通过 IBM Cloud Infrastructure (SoftLayer) 专用网络进行通信。例如，{{site.data.keyword.cloudant_short_notm}} 的所有标准套餐实例的专用硬件环境都支持专用服务端点。如果 {{site.data.keyword.Bluemix_notm}} 服务不支持专用服务端点，那么集群无法访问该服务。

要提供对集群中应用程序的专用访问权，可以创建专用网络负载均衡器 (NLB) 或 Ingress 应用程序负载均衡器 (ALB)。这些 Kubernetes 网络服务仅将应用程序公开到专用网络，以便连接到该 NLB IP 所在子网的任何内部部署系统都可以访问应用程序。

准备好开始将集群用于此方案了吗？规划[高可用性](/docs/containers?topic=containers-ha_clusters)和[工作程序节点](/docs/containers?topic=containers-planning_worker_nodes)设置后，请参阅[创建集群](/docs/containers?topic=containers-clusters#cluster_prepare)。
