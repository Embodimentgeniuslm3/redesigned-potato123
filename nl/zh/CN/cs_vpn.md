---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-10"

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


# 设置 VPN 连接
{: #vpn}

通过 VPN 连接，您可以将 {{site.data.keyword.containerlong}} 上 Kubernetes 集群中的应用程序安全地连接到内部部署网络。您还可以将集群外部的应用程序连接到正在集群内部运行的应用程序。
{:shortdesc}

要将工作程序节点和应用程序连接到内部部署数据中心，可以配置下列其中一个选项。

- **strongSwan IPSec VPN 服务**：您可以设置 [strongSwan IPSec VPN 服务 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.strongswan.org/about.html)，以将 Kubernetes 集群与内部部署网络安全连接。strongSwan IPSec VPN 服务基于业界标准因特网协议安全性 (IPSec) 协议组，通过因特网提供安全的端到端通信信道。要在集群与内部部署网络之间设置安全连接，请在集群的 pod 中直接[配置和部署 strongSwan IPSec VPN 服务](#vpn-setup)。

- **{{site.data.keyword.BluDirectLink}}**：[{{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-about-ibm-cloud-direct-link) 允许您在远程网络环境和 {{site.data.keyword.containerlong_notm}} 之间创建直接专用连接，而无需通过公用因特网进行路由。必须实现混合工作负载、跨提供者工作负载、大型或频繁数据传输或者专用工作负载时，{{site.data.keyword.Bluemix_notm}} Direct Link 产品非常有用。要选择 {{site.data.keyword.Bluemix_notm}} Direct Link 产品并设置 {{site.data.keyword.Bluemix_notm}} Direct Link 连接，请参阅 {{site.data.keyword.Bluemix_notm}} Direct Link 文档中的 [{{site.data.keyword.Bluemix_notm}} Direct Link 入门](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link#how-do-i-know-which-type-of-ibm-cloud-direct-link-i-need-)。

- **虚拟路由器设备 (VRA) 或 Fortigate Security Appliance (FSA)**：您可选择设置 [VRA (Vyatta)](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-about-the-vra) 或 [FSA](/docs/services/vmwaresolutions/services?topic=vmware-solutions-fsa_considerations) 来配置 IPSec VPN 端点。如果您具有更大的集群，希望通过单个 VPN 访问多个集群，或者需要基于路径的 VPN，那么此选项会非常有用。要配置 VRA，请参阅[使用 VRA 设置 VPN 连接](#vyatta)。

## 使用 strongSwan IPSec VPN 服务 Helm chart
{: #vpn-setup}

使用 Helm chart 在 Kubernetes pod 内配置并部署 strongSwan IPSec VPN 服务。
{:shortdesc}

由于 strongSwan 已在集群中集成，因此无需外部网关设备。建立 VPN 连接时，会在集群中的所有工作程序节点上自动配置路径。这些路径允许在任何工作程序节点和远程系统上的 pod 之间通过 VPN 隧道进行双向连接。例如，下图显示了 {{site.data.keyword.containerlong_notm}} 中的应用程序可以如何通过 strongSwan VPN 连接与内部部署服务器进行通信：

<img src="images/cs_vpn_strongswan.png" width="700" alt="通过使用网络负载均衡器 (NLB) 在 {{site.data.keyword.containerlong_notm}} 中公开应用程序" style="width:700px; border-style: none"/>

1. 集群中的应用程序 `myapp` 接收来自 Ingress 或 LoadBalancer 服务的请求，并且需要安全地连接到内部部署网络中的数据。

2. 对内部部署数据中心的请求将转发到 IPSec strongSwan VPN pod。目标 IP 地址用于确定将哪些网络包发送到 IPSec strongSwan VPN pod。

3. 该请求已加密，并通过 VPN 隧道发送到内部部署数据中心。

4. 入局请求通过内部部署防火墙传递，并传递到将在其中进行解密的 VPN 隧道端点（路由器）。

5. VPN 隧道端点（路由器）将该请求转发到内部部署服务器或大型机，具体取决于步骤 2 中指定的目标 IP 地址。必需的数据通过相同的过程经由 VPN 连接发送回 `myapp`。

## strongSwan VPN 服务注意事项
{: #strongswan_limitations}

使用 strongSwan Helm chart 之前，请查看以下注意事项和限制。
{: shortdesc}

* strongSwan Helm chart 需要远程 VPN 端点启用 NAT 遍历。除了缺省 IPSec UDP 端口 500 之外，NAT 遍历还需要 UDP 端口 4500。需要允许这两个 UDP 端口通过任何配置的防火墙。
* strongSwan Helm chart 不支持基于路径的 IPSec VPN。
* strongSwan Helm chart 支持使用预共享密钥的 IPSec VPN，但不支持需要证书的 IPSec VPN。
* strongSwan Helm chart 不允许多个集群和其他 IaaS 资源共享单个 VPN 连接。
* strongSwan Helm chart 作为集群内部的 Kubernetes pod 运行。VPN 性能受集群中运行的 Kubernetes 和其他 pod 的内存和网络使用情况的影响。如果您的环境中性能很重要，请考虑使用在集群外部的专用硬件上运行的 VPN 解决方案。
* strongSwan Helm chart 将单个 VPN pod 作为 IPSec 隧道端点运行。如果 pod 发生故障，集群将重新启动该 pod。但是，在新 pod 启动并重新建立 VPN 连接时，您可能会遇到较短的停机时间。如果需要从错误更快恢复，或需要更详细的高可用性解决方案，请考虑使用在集群外部的专用硬件上运行的 VPN 解决方案。
* strongSwan Helm chart 不提供对通过 VPN 连接传递的网络流量的度量或监视。有关受支持监视工具的列表，请参阅[日志记录和监视服务](/docs/containers?topic=containers-supported_integrations#health_services)。

集群用户可以使用 strongSwan VPN 服务通过专用服务端点连接到 Kubernetes 主节点。但是，与 Kubernetes 主节点通过专用服务端点进行的通信必须经过 <code>166.X.X.X</code> IP 地址范围，这不能通过 VPN 连接进行路由。可以通过[使用专用网络负载均衡器 (NLB)](/docs/containers?topic=containers-clusters#access_on_prem) 来公开集群用户的主节点的专用服务端点。专用 NLB 将主节点的专用服务端点作为 strongSwan VPN pod 可以访问的内部 `172.21.x.x` 集群 IP 地址公开。如果仅启用专用服务端点，那么可以使用 Kubernetes 仪表板或临时启用公共服务端点来创建专用 NLB。
{: tip}

<br />


## 在多专区集群中配置 strongSwan VPN
{: #vpn_multizone}

多专区集群通过使应用程序实例在多个专区中的工作程序节点上可用，可在发生中断时为应用程序提供高可用性。但是，在多专区集群中配置 strongSwan VPN 服务比在单专区集群中配置 strongSwan 更复杂。
{: shortdesc}

在多专区集群中配置 strongSwan 之前，请首先尝试将 strongSwan Helm chart 部署到单专区集群中。首次在单专区集群与内部部署网络之间建立 VPN 连接时，可以更轻松地确定对于多专区 strongSwan 配置非常重要的远程网络防火墙设置：
* 某些远程 VPN 端点在 `ipsec.conf` 文件中具有 `leftid` 或 `rightid` 等设置。如果您有这些设置，请检查是否必须将 `leftid` 设置为 VPN IPSec 隧道的 IP 地址。
*	如果连接是从远程网络到集群的入站连接，请检查远程 VPN 端点是否可以在一个专区中的负载均衡器发生故障时，与其他 IP 地址重新建立 VPN 连接。

要开始在多专区集群中使用 strongSwan，请选择下列其中一个选项。
* 如果可以使用出站 VPN 连接，那么可以选择仅配置一个 strongSwan VPN 部署。请参阅[从多专区集群配置一个出站 VPN 连接](#multizone_one_outbound)。
* 如果需要入站 VPN 连接，那么根据远程 VPN 端点是否可以配置为在检测到中断时与其他公共负载均衡器 IP 重新建立 VPN 连接，您可以使用的配置设置有所不同。
  * 如果远程 VPN 端点可以自动与其他 IP 重新建立 VPN 连接，那么可以选择仅配置一个 strongSwan VPN 部署。请参阅[配置与多专区集群的一个入站 VPN 连接](#multizone_one_inbound)。
  * 如果远程 VPN 端点无法自动与其他 IP 重新建立 VPN 连接，那么必须在每个专区中部署单独的入站 strongSwan VPN 服务。请参阅[在多专区集群的每个专区中配置 VPN 连接](#multizone_multiple)。

请尝试将环境设置为针对多专区集群的出站或入站 VPN 连接仅需要一个 strongSwan VPN 部署。如果必须在每个专区中设置单独的 strongSwan VPN，请确保计划如何管理这一增加的复杂性和提高的资源使用量。
{: note}

### 从多专区集群配置一个出站 VPN 连接
{: #multizone_one_outbound}

要在多专区集群中配置 strongSwan VPN 服务，最简单的解决方案是使用可在集群中所有可用性专区的不同工作程序节点之间切换的单个出站 VPN 连接。
{: shortdesc}

VPN 连接是来自多专区集群的出站连接时，只需要一个 strongSwan 部署。如果某个工作程序节点已除去或遇到停机时间，`kubelet` 会将 VPN pod 重新安排到新的工作程序节点上。如果某个可用性专区遇到中断，`kubelet` 会将 VPN pod 重新安排到其他专区中的新工作程序节点上。

1. [配置一个 strongSwan VPN Helm chart](/docs/containers?topic=containers-vpn#vpn_configure)。执行该部分中的步骤时，请确保指定以下设置：
    - `ipsec.auto`：更改为 `start`。连接是来自集群的出站连接。
    - `loadBalancerIP`：不要指定 IP 地址。请将此设置保留为空。
    - `zoneLoadBalancer`：为工作程序节点所在的每个专区指定一个公共负载均衡器 IP 地址。[可以检查以确定可用的公共 IP 地址](/docs/containers?topic=containers-subnets#review_ip)或[释放使用的 IP 地址](/docs/containers?topic=containers-subnets#free)。由于可以将 strongSwan VPN pod 安排到任何专区中的工作程序节点，因此该 IP 列表可确保在其中安排了 VPN pod 的任何专区中都可以使用负载均衡器 IP。
    - `connectUsingLoadBalancerIP`：设置为 `true`。将 strongSwan VPN pod 安排到工作程序节点上时，strongSwan 服务会选择同一专区中的负载均衡器 IP 地址，并使用此 IP 来建立出站连接。
    - `local.id`：指定远程 VPN 端点支持的固定值。如果远程 VPN 端点需要将 `local.id` 选项（`ipsec.conf` 中的 `leftid` 值）设置为 VPN IPSec 隧道的公共 IP 地址，请将 `local.id` 设置为 `%loadBalancerIP`。此值会自动将 `ipsec.conf` 中的 `leftid` 值配置为用于连接的负载均衡器 IP 地址。

2. 在远程网络防火墙中，允许来自 `zoneLoadBalancer` 设置中列出的公共 IP 地址的入局 IPSec VPN 连接。

3. 配置远程 VPN 端点，以允许来自 `zoneLoadBalancer` 设置中列出的每个可能的负载均衡器 IP 的入局 VPN 连接。

### 配置与多专区集群的一个入站 VPN 连接
{: #multizone_one_inbound}

如果需要入局 VPN 连接，并且远程 VPN 端点可以在检测到故障时自动与其他 IP 重新建立 VPN 连接，那么可以使用可在集群中所有可用性专区的不同工作程序节点之间切换的单个入站 VPN 连接。
{: shortdesc}

远程 VPN 端点可以与任何专区中的任何 strongSwan 负载均衡器建立 VPN 连接。无论 VPN pod 位于哪个专区中，都会将入局请求发送到该 VPN pod。来自 VPN pod 的响应会通过原始负载均衡器发送回远程 VPN 端点。此选项可确保高可用性，因为如果某个工作程序节点已除去或遇到停机时间，`kubelet` 会将 VPN pod 重新安排到新的工作程序节点上。此外，如果某个可用性专区遇到中断，那么远程 VPN 端点可以与其他专区中的负载均衡器 IP 地址重新建立 VPN 连接，以便仍能访问 VPN pod。

1. [配置一个 strongSwan VPN Helm chart](/docs/containers?topic=containers-vpn#vpn_configure)。执行该部分中的步骤时，请确保指定以下设置：
    - `ipsec.auto`：更改为 `add`。连接是集群的入站连接。
    - `loadBalancerIP`：不要指定 IP 地址。请将此设置保留为空。
    - `zoneLoadBalancer`：为工作程序节点所在的每个专区指定一个公共负载均衡器 IP 地址。[可以检查以确定可用的公共 IP 地址](/docs/containers?topic=containers-subnets#review_ip)或[释放使用的 IP 地址](/docs/containers?topic=containers-subnets#free)。
    - `local.id`：如果远程 VPN 端点需要将 `local.id` 选项（`ipsec.conf` 中的 `leftid` 值）设置为 VPN IPSec 隧道的公共 IP 地址，请将 `local.id` 设置为 `%loadBalancerIP`。此值会自动将 `ipsec.conf` 中的 `leftid` 值配置为用于连接的负载均衡器 IP 地址。

2. 在远程网络防火墙中，允许连至 `zoneLoadBalancer` 设置中列出的公共 IP 地址的出局 IPSec VPN 连接。

### 在多专区集群的每个专区中配置入站 VPN 连接
{: #multizone_multiple}

需要入局 VPN 连接，但远程 VPN 端点无法与其他 IP 重新建立 VPN 连接时，必须在每个专区中部署单独的 strongSwan VPN 服务。
{: shortdesc}

远程 VPN 端点必须更新为与每个专区中的负载均衡器建立单独的 VPN 连接。此外，必须在远程 VPN 端点上配置特定于专区的设置，以便其中每个 VPN 连接都是唯一的。确保这些多个入局 VPN 连接始终处于活动状态。

部署每个 Helm chart 后，每个 strongSwan VPN 部署会作为正确专区中的 Kubernetes Load Balancer 服务启动。对该公共 IP 的入局请求会转发到也在同一专区中分配的 VPN pod。如果该专区遇到中断，不会影响其他专区中建立的 VPN 连接。

1. 针对每个专区[配置 strongSwan VPN Helm chart](/docs/containers?topic=containers-vpn#vpn_configure)。执行该部分中的步骤时，请确保指定以下设置：
    - `loadBalancerIP`：指定在其中部署此 strongSwan 服务的专区中的可用公共负载均衡器 IP 地址。[可以检查以确定可用的公共 IP 地址](/docs/containers?topic=containers-subnets#review_ip)或[释放使用的 IP 地址](/docs/containers?topic=containers-subnets#free)。
    - `zoneSelector`：指定要在其中安排 VPN pod 的专区。
    - 可能需要其他设置，例如 `zoneSpecificRoutes`、`remoteSubnetNAT`、`localSubnetNAT` 或 `enableSingleSourceIP`，具体取决于哪些资源必须可通过 VPN 进行访问。请参阅下一步以了解更多详细信息。

2. 在 VPN 隧道的两端配置特定于专区的设置，以确保每个 VPN 连接都是唯一的。根据哪些资源必须可通过 VPN 进行访问，您有两个选项可用于区分连接：
    * 如果集群中的 pod 必须访问远程内部部署网络上的服务：
      - `zoneSpecificRoutes`：设置为 `true`。此设置将 VPN 连接限制为集群中的单个专区。特定专区中的 pod 仅使用针对该特定专区设置的 VPN 连接。此解决方案减少了在多专区集群中支持多个 VPN 所需的 strongSwan pod 数，提高了 VPN 性能（因为 VPN 流量仅流至位于当前专区中的工作程序节点），并可确保每个专区的 VPN 连接不受其他专区中的 VPN 连接、崩溃的 pod 或专区中断的影响。请注意，无需配置 `remoteSubnetNAT`。使用 `zoneSpecificRoutes` 设置的多个 VPN 可以具有相同的 `remote.subnet`，因为路由是按专区设置的。
      - `enableSingleSourceIP`：设置为 `true`，并将 `local.subnet` 设置为单个 /32 IP 地址。此设置组合将隐藏单个 /32 IP 地址后面的所有集群专用 IP 地址。此唯一的 /32 IP 地址允许远程内部部署网络通过正确的 VPN 连接，将回复发送回集群中启动请求的正确 pod。请注意，为 `local.subnet` 选项配置的单个 /32 IP 地址在每个 strongSwan VPN 配置中必须唯一。
    * 如果远程内部部署网络中的应用程序必须访问集群中的服务：    
      - `localSubnetNAT`：确保内部部署远程网络中的应用程序可以选择特定的 VPN 连接来发送和接收流至集群的流量。在每个 strongSwan Helm 配置中，使用 `localSubnetNAT` 来唯一地标识可由远程内部部署应用程序访问的集群资源。由于从远程内部部署网络到集群建立了多个 VPN，因此必须将逻辑添加到内部部署网络上的应用程序，以便可以选择要在访问集群中的服务时使用的 VPN。请注意，集群中的服务可通过多个不同的子网进行访问，具体取决于在每个 strongSwan VPN 配置中为 `localSubnetNAT` 配置的内容。
      - `remoteSubnetNAT`：确保集群中的 pod 使用相同的 VPN 连接将流量返回到远程网络。在每个 strongSwan 部署文件中，使用 `remoteSubnetNAT` 设置将远程内部部署子网映射到唯一子网。集群中的 pod 从特定于 VPN 的 `remoteSubnetNAT` 收到的流量会发送回特定于 VPN 的这一相同 `remoteSubnetNAT`，然后通过这一相同 VPN 连接传递。

3. 配置远程 VPN 端点软件，以与每个专区中的负载均衡器 IP 建立单独的 VPN 连接。

<br />


## 配置 strongSwan Helm chart
{: #vpn_configure}

安装 strongSwan Helm chart 之前，您必须决定 strongSwan 配置。
{: shortdesc}

开始之前：
* 在内部部署数据中心内安装 IPSec VPN 网关。
* 确保您具有对 `default` 名称空间的 [{{site.data.keyword.Bluemix_notm}} IAM **写入者**或**管理者**服务角色](/docs/containers?topic=containers-users#platform)。
* [登录到您的帐户。如果适用，请将相应的资源组设定为目标。为集群设置上下文。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
  * **注**：标准集群中允许所有 strongSwan 配置。如果使用的是免费集群，那么只能在[步骤 3](#strongswan_3) 中选择出站 VPN 连接。入站 VPN 连接需要集群中的负载均衡器，但负载均衡器对于免费集群不可用。

### 步骤 1：获取 strongSwan Helm chart
{: #strongswan_1}

安装 Helm 并获取 strongSwan Helm chart 以查看可能的配置。
{: shortdesc}

1.  [遵循指示信息](/docs/containers?topic=containers-helm#public_helm_install)在本地计算机上安装 Helm 客户机，使用服务帐户安装 Helm 服务器 (Tiller)，然后添加 {{site.data.keyword.Bluemix_notm}} Helm 存储库。请注意，需要 Helm V2.8 或更高版本。

2.  验证是否已使用服务帐户安装 Tiller。

    ```
    kubectl get serviceaccount -n kube-system | grep tiller
    ```
    {: pre}

    输出示例：

    ```
    NAME                                 SECRETS   AGE
    tiller                               1         2m
    ```
    {: screen}

3. 在本地 YAML 文件中保存 strongSwan Helm chart 的缺省配置设置。

    ```
    helm inspect values iks-charts/strongswan > config.yaml
    ```
    {: pre}

4. 打开 `config.yaml` 文件。

### 步骤 2：配置基本 IPSec 设置
{: #strongswan_2}

要控制 VPN 连接的建立，请修改以下基本 IPSec 设置。
{: shortdesc}

有关每个设置的更多信息，请阅读 Helm chart 的 `config.yaml` 文件中提供的文档。
{: tip}

1. 如果内部部署 VPN 隧道端点不支持 `ikev2` 作为初始化连接的协议，请将 `ipsec.keyexchange` 的值更改为 `ikev1`。
2. 将 `ipsec.esp` 设置为内部部署 VPN 隧道端点用于连接的 ESP 加密和认证算法的列表。
    * 如果 `ipsec.keyexchange` 设置为 `ikev1`，那么必须指定此设置。
    * 如果 `ipsec.keyexchange` 设置为 `ikev2`，那么此设置是可选的。
    * 如果将此设置保留为空，那么会将缺省 strongSwan 算法 `aes128-sha1,3des-sha1` 用于连接。
3. 将 `ipsec.ike` 设置为内部部署 VPN 隧道端点用于连接的 IKE/ISAKMP SA 加密和认证算法的列表。算法必须明确采用格式 `encryption-integrity[-prf]-dhgroup`。
    * 如果 `ipsec.keyexchange` 设置为 `ikev1`，那么必须指定此设置。
    * 如果 `ipsec.keyexchange` 设置为 `ikev2`，那么此设置是可选的。
    * 如果将此设置保留为空，那么会将缺省 strongSwan 算法 `aes128-sha1-modp2048,3des-sha1-modp1536` 用于连接。
4. 将 `local.id` 的值更改为要用于标识 VPN 隧道端点使用的本地 Kubernetes 集群端的任何字符串。缺省值为 `ibm-cloud`。某些 VPN 实现需要使用本地端点的公共 IP 地址。
5. 将 `remote.id` 的值更改为要用于标识 VPN 隧道端点使用的远程内部部署端的任何字符串。缺省值为 `on-prem`。某些 VPN 实现需要使用远程端点的公共 IP 地址。
6. 将 `preshared.secret` 的值更改为内部部署 VPN 隧道端点网关用于连接的预共享密钥。此值存储在 `ipsec.secrets` 中。
7. 可选：将 `remote.privateIPtoPing` 设置为远程子网中作为 Helm 连接验证测试的一部分对其执行 ping 操作的任何专用 IP 地址。

### 步骤 3：选择入站或出站 VPN 连接
{: #strongswan_3}

配置 strongSwan VPN 连接时，请选择 VPN 连接是入站到集群还是从集群出站。
{: shortdesc}

<dl>
<dt>入站</dt>
<dd>远程网络中的内部部署 VPN 端点启动 VPN 连接，集群侦听连接。</dd>
<dt>出站</dt>
<dd>集群启动 VPN 连接，远程网络中的内部部署 VPN 端点侦听连接。</dd>
</dl>

如果使用的是免费集群，那么只能选择出站 VPN 连接。入站 VPN 连接需要集群中的负载均衡器，但负载均衡器对于免费集群不可用。

要建立入站 VPN 连接，请修改以下设置：
1. 验证 `ipsec.auto` 是否设置为 `add`。
2. 可选：将 `loadBalancerIP` 设置为 strongSwan VPN 服务的可移植公共 IP 地址。需要稳定的 IP 地址时（例如，必须指定允许哪些 IP 地址通过内部部署防火墙时），指定 IP 地址很有用。该集群必须至少具有一个可用的公共负载均衡器 IP 地址。[可以检查以确定可用的公共 IP 地址](/docs/containers?topic=containers-subnets#review_ip)或[释放使用的 IP 地址](/docs/containers?topic=containers-subnets#free)。
    * 如果将此设置保留为空，那么将使用其中一个可用的可移植公共 IP 地址。
    * 还必须配置为内部部署 VPN 端点上的集群 VPN 端点选择或分配给该端点的公共 IP 地址。

要建立出站 VPN 连接，请修改以下设置：
1. 将 `ipsec.auto` 更改为 `start`。
2. 将 `remote.gateway` 设置为远程网络中内部部署 VPN 端点的公共 IP 地址。
3. 对于集群 VPN 端点的 IP 地址，选择下列其中一个选项：
    * **集群专用网关的公共 IP 地址**：如果工作程序节点仅连接到专用 VLAN，那么出站 VPN 请求会通过专用网关进行路由，以访问因特网。专用网关的公共 IP 地址用于 VPN 连接。
    * **运行 strongSwan pod 的工作程序节点的公共 IP 地址**：如果运行 strongSwan pod 的工作程序节点连接到公用 VLAN，那么该工作程序节点的公共 IP 地址将用于 VPN 连接。<br>
        * 如果删除了 strongSwan pod 并将其重新安排到集群中的其他工作程序节点上，那么 VPN 的公共 IP 地址会更改。远程网络的内部部署 VPN 端点必须允许从任何集群工作程序节点的公共 IP 地址建立 VPN 连接。
        * 如果远程 VPN 端点无法处理来自多个公共 IP 地址的 VPN 连接，请限制 strongSwan VPN pod 部署到的节点。将 `nodeSelector` 设置为特定工作程序节点的 IP 地址或工作程序节点标签。例如，值 `kubernetes.io/hostname: 10.232.xx.xx` 允许将 VPN pod 仅部署到该工作程序节点。值 `strongswan: vpn` 将 VPN pod 限制为在具有该标签的任何工作程序节点上运行。可以使用任何工作程序节点标签。要允许不同的工作程序节点用于不同的 Helm chart 部署，请使用 `strongswan: <release_name>`。为实现高可用性，请至少选择两个工作程序节点。
    * **strongSwan 服务的公共 IP 地址**：要使用 strongSwan VPN 服务的 IP 地址来建立连接，请将 `connectUsingLoadBalancerIP` 设置为 `true`。strongSwan 服务 IP 地址是可以在 `loadBalancerIP` 设置中指定的可移植公共 IP 地址，也可以是自动分配给服务的可用可移植公共 IP 地址。
        <br>
        * 如果选择使用 `loadBalancerIP` 设置来选择 IP 地址，那么集群必须至少具有一个可用的公共负载均衡器 IP 地址。[可以检查以确定可用的公共 IP 地址](/docs/containers?topic=containers-subnets#review_ip)或[释放使用的 IP 地址](/docs/containers?topic=containers-subnets#free)。
        * 所有集群工作程序节点必须位于同一公用 VLAN 上。否则，必须使用 `nodeSelector` 设置来确保 VPN pod 部署到 `loadBalancerIP` 所在的公用 VLAN 上的工作程序节点。
        * 如果 `connectUsingLoadBalancerIP` 设置为 `true`，并且 `ipsec.keyexchange` 设置为 `ikev1`，那么必须将 `enableServiceSourceIP` 设置为 `true`。

### 步骤 4：通过 VPN 连接访问集群资源
{: #strongswan_4}

确定哪些集群资源必须可由远程网络通过 VPN 连接进行访问。
{: shortdesc}

1. 将一个或多个集群子网的 CIDR 添加到 `local.subnet` 设置。必须在内部部署 VPN 端点上配置本地子网 CIDR。此列表可包含以下子网：  
    * Kubernetes pod 子网 CIDR：`172.30.0.0/16`。将启用所有集群 pod 与 `remote.subnet` 设置中列出的远程网络子网中的任何主机之间的双向通信。如果出于安全原因，必须阻止任何 `remote.subnet` 主机访问集群 pod，请不要将 Kubernetes pod 子网添加到 `local.subnet` 设置。
    * Kubernetes 服务子网 CIDR：`172.21.0.0/16`。服务 IP 地址提供了一种方法，用于公开在单个 IP 后面的多个工作程序节点上部署的多个应用程序 pod。
    * 如果应用程序由专用网络上的 NodePort 服务或专用 Ingress ALB 公开，请添加工作程序节点的专用子网 CIDR。通过运行 `ibmcloud ks worker <cluster_name>`，检索您工作程序的专用 IP 地址的前三个八位元。例如，如果检索到的是 `10.176.48.xx`，请记下 `10.176.48`。接下来，通过运行以下命令来获取工作程序专用子网 CIDR，并将 `<xxx.yyy.zz>` 替换为先前检索到的八位元：`ibmcloud sl subnet list | grep <xxx.yyy.zzz>`。**注**：如果在新的专用子网上添加了工作程序节点，那么必须将新的专用子网 CIDR 添加到 `local.subnet` 设置和内部部署 VPN 端点。然后，必须重新启动 VPN 连接。
    * 如果应用程序由专用网络上的 LoadBalancer 服务公开，请添加集群的由用户管理的专用子网 CIDR。要查找这些值，请运行 `ibmcloud ks cluster-get --cluster <cluster_name> --showResources`。在 **VLANs** 部分中，查找 **Public** 值为 `false` 的 CIDR。**注**：如果 `ipsec.keyexchange` 设置为 `ikev1`，那么只能指定一个子网。但是，可以使用 `localSubnetNAT` 设置将多个集群子网组合成单个子网。

2. 可选：使用 `localSubnetNAT` 设置重新映射集群子网。子网的网络地址转换 (NAT) 提供了针对集群网络与内部部署远程网络之间子网冲突的变通方法。可以使用 NAT 将集群的专用本地 IP 子网、pod 子网 (172.30.0.0/16) 或 pod 服务子网 (172.21.0.0/16) 重新映射到其他专用子网。VPN 隧道看到的是重新映射的 IP 子网，而不是原始子网。在通过 VPN 隧道发送包之前以及来自 VPN 隧道的包到达之后，会发生重新映射。可以通过 VPN 来同时公开重新映射和未重新映射的子网。要启用 NAT，可以添加整个子网，也可以添加单个 IP 地址。
    * 如果是添加整个子网（格式为 `10.171.42.0/24=10.10.10.0/24`），那么重新映射方式为 1 对 1：内部网络子网中的所有 IP 地址都会映射到外部网络子网，反之亦然。
    * 如果是添加单个 IP 地址（格式为 `10.171.42.17/32=10.10.10.2/32,10.171.42.29/32=10.10.10.3/32`），那么只有这些内部 IP 地址会映射到指定的外部 IP 地址。

3. 对于 V2.2.0 和更高版本的 strongSwan Helm chart 为可选：通过将 `enableSingleSourceIP` 设置为 `true`，将所有集群 IP 地址隐藏在单个 IP 地址后面。此选项为 VPN 连接提供了其中一种最安全的配置，因为不允许远程网络向后连接到集群。
    <br>
    * 此设置要求不管 VPN 连接是从集群还是从远程网络建立的，通过 VPN 连接传递的所有数据流都必须为出站。
    * `local.subnet` 只能设置为一个 /32 子网。

4. 对于 V2.2.0 和更高版本的 strongSwan Helm chart 为可选：通过使用 `localNonClusterSubnet` 设置，允许 strongSwan 服务将来自远程网络的入局请求路由到存在于集群外部的服务。
    <br>
    * 非集群服务必须存在于同一专用网络上，或存在于工作程序节点可访问的专用网络上。
    * 非集群工作程序节点无法通过 VPN 连接来启动流至远程网络的流量，但非集群节点可以是来自远程网络的入局请求的目标。
    * 必须在 `local.subnet` 设置中列出非集群子网的 CIDR。

### 步骤 5：通过 VPN 连接访问远程网络资源
{: #strongswan_5}

确定哪些远程网络资源必须可由集群通过 VPN 连接进行访问。
{: shortdesc}

1. 将一个或多个内部部署专用子网的 CIDR 添加到 `remote.subnet` 设置。
    **注**：如果 `ipsec.keyexchange` 设置为 `ikev1`，那么只能指定一个子网。
2. 对于 V2.2.0 和更高版本的 strongSwan Helm chart 为可选：使用 `remoteSubnetNAT` 设置重新映射远程网络子网。子网的网络地址转换 (NAT) 提供了针对集群网络与内部部署远程网络之间子网冲突的变通方法。可以使用 NAT 将远程网络的 IP 子网重新映射到其他专用子网。在通过 VPN 隧道发送包之前，会发生重新映射。集群中的 pod 看到的是重新映射的 IP 子网，而不是原始子网。在 pod 通过 VPN 隧道发送数据之前，重新映射的 IP 子网将切换回远程网络正在使用的实际子网。可以通过 VPN 来同时公开重新映射和未重新映射的子网。

### 步骤 6（可选）：通过 Slack Webhook 集成启用监视
{: #strongswan_6}

要监视 strongSwan VPN 的状态，您可以设置 Webhook 以自动向 Slack 通道发布 VPN 连接消息。
{: shortdesc}

1. 登录到 Slack 工作空间。

2. 转至 [Incoming WebHooks 应用程序页面 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://slack.com/apps/A0F7XDUAZ-incoming-webhooks)。

3. 单击**请求安装**。如果此应用程序未列在 Slack 设置中，请联系 Slack 工作空间所有者。

4. 安装请求得到核准后，请单击**添加配置**。

5. 选择 Slack 通道或创建新通道来接收 VPN 消息。

6. 复制生成的 Webhook URL。URL 格式类似于以下内容：
  ```
  https://hooks.slack.com/services/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
  ```
  {: screen}

7. 要验证是否已安装 Slack Webhook，请通过运行以下命令向 Webhook URL 发送测试消息：
    ```
    curl -X POST -H 'Content-type: application/json' -d '{"text":"VPN test message"}' <webhook_URL>
    ```
    {: pre}

8. 转至选择的 Slack 通道来验证测试消息是否成功。

9. 在 Helm chart 的 `config.yaml` 文件中，配置 Webhook 以监视 VPN 连接。
    1. 将 `monitoring.enable` 更改为 `true`。
    2. 将专用 IP 地址或要确保可通过 VPN 连接访问的远程子网中的 HTTP 端点添加到 `monitoring.privateIPs` 或 `monitoring.httpEndpoints`。例如，可以将 `remote.privateIPtoPing` 设置中的 IP 添加到 `monitoring.privateIPs`。
    3. 将 Webhook URL 添加到 `monitoring.slackWebhook`。
    4. 根据需要更改其他可选 `monitoring` 设置。

### 步骤 7：部署 Helm chart
{: #strongswan_7}

使用您先前选择的配置在集群中部署 strongSwan Helm chart。
{: shortdesc}

1. 如果需要配置更高级的设置，请遵循为 Helm chart 中的每个设置提供的文档。

3. 保存更新的 `config.yaml` 文件。

4. 使用更新的 `config.yaml` 文件将 Helm chart 安装到集群。

    如果在单个集群中有多个 VPN 部署，那么可以通过选择比 `vpn` 描述性更强的发行版名称，以避免命名冲突并区分部署。为了避免截断发行版名称，请将发行版名称限制为不超过 35 个字符。
    {: tip}

    ```
    helm install -f config.yaml --name=vpn iks-charts/strongswan
    ```
    {: pre}

5. 检查 chart 部署状态。当 chart 就绪时，输出顶部附近的 **STATUS** 字段的值为 `DEPLOYED`。

    ```
    helm status vpn
    ```
    {: pre}

6. 部署图表后，请验证是否使用了 `config.yaml` 文件中的已更新设置。

    ```
    helm get values vpn
    ```
    {: pre}

## 测试并验证 strongSwan VPN 连接
{: #vpn_test}

部署 Helm chart 后，请测试 VPN 连接。
{:shortdesc}

1. 如果内部部署网关上的 VPN 处于不活动状态，请启动 VPN。

2. 设置 `STRONGSWAN_POD` 环境变量。

    ```
export STRONGSWAN_POD=$(kubectl get pod -l app=strongswan,release=vpn -o jsonpath='{ .items[0].metadata.name }')
    ```
    {: pre}

3. 检查 VPN 的状态。状态 `ESTABLISHED` 表示 VPN 连接成功。

    ```
kubectl exec $STRONGSWAN_POD -- ipsec status
    ```
    {: pre}

    输出示例：

    ```
Security Associations (1 up, 0 connecting):
    k8s-conn[1]: ESTABLISHED 17 minutes ago, 172.30.xxx.xxx[ibm-cloud]...192.xxx.xxx.xxx[on-premises]
    k8s-conn{2}: INSTALLED, TUNNEL, reqid 12, ESP in UDP SPIs: c78cb6b1_i c5d0d1c3_o
    k8s-conn{2}: 172.21.0.0/16 172.30.0.0/16 === 10.91.152.xxx/26
    ```
    {: screen}

    * 尝试使用 strongSwan Helm chart 建立 VPN 连接时，很有可能 VPN 阶段状态一开始不是 `ESTABLISHED`。您可能需要检查内部部署 VPN 端点设置，并多次更改配置文件，连接才能成功：
        1. 运行 `helm delete --purge <release_name>`
        2. 修正配置文件中的错误值。
        3. 运行 `helm install -f config.yaml --name=<release_name> ibm/strongswan`
     您还可以在下一步中运行更多检查。

    * 如果 VPN pod 处于 `ERROR` 状态或继续崩溃并重新启动，那么可能是因为在图表的配置映射中对 `ipsec.conf` 设置的参数验证问题。
        1. 通过运行 `kubectl logs $STRONGSWAN_POD` 来检查 strongSwan pod 日志中是否存在任何验证错误。
        2. 如果存在验证错误，请运行 `helm delete --purge <release_name>`
        3. 修正配置文件中的错误值。
        4. 运行 `helm install -f config.yaml --name=<release_name> ibm/strongswan`

4. 可以通过运行 strongSwan chart 定义中包含的五个 Helm 测试来进一步测试 VPN 连接。

    ```
    helm test vpn
    ```
    {: pre}

    * 如果所有测试均通过，说明 strongSwan VPN 连接已成功建立。
    * 如果任何测试失败，请继续执行下一步。

5. 通过查看测试 pod 的日志来查看失败测试的输出。

    ```
kubectl logs <test_program>
    ```
    {: pre}

    某些测试有一些要求，而这些要求在 VPN 配置中是可选设置。如果某些测试失败，失败可能是可接受的，具体取决于您是否指定了这些可选设置。有关每个测试的信息以及测试可能失败的原因，请参阅下表。
    {: note}

    {: #vpn_tests_table}
    <table>
    <caption>了解 Helm VPN 连接测试</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解 Helm VPN 连接测试</th>
    </thead>
    <tbody>
    <tr>
    <td><code>vpn-strongswan-check-config</code></td>
    <td>验证通过 <code>config.yaml</code> 文件生成的 <code>ipsec.conf</code> 文件的语法。此测试可能会因为 <code>config.yaml</code> 文件中的值不正确而失败。</td>
    </tr>
    <tr>
    <td><code>vpn-strongswan-check-state</code></td>
    <td>检查 VPN 连接的阶段状态是否为 <code>ESTABLISHED</code>。此测试可能因为以下原因而失败：<ul><li><code>config.yaml</code> 文件中的值与内部部署 VPN 端点设置不同。</li><li>如果集群处于“侦听”方式（<code>ipsec.auto</code> 设置为 <code>add</code>），那么不会在内部部署端建立连接。</li></ul></td>
    </tr>
    <tr>
    <td><code>vpn-strongswan-ping-remote-gw</code></td>
    <td>对 <code>config.yaml</code> 文件中配置的 <code>remote.dgateway</code> 公共 IP 地址执行 ping 操作。如果 VPN 连接的状态为 <code>ESTABLISHED</code>，那么可以忽略此测试的结果。如果 VPN 连接的状态不是 <code>ESTABLISHED</code>，说明此测试可能由于以下原因而失败：<ul><li>未指定内部部署 VPN 网关 IP 地址。如果 <code>ipsec.auto</code> 设置为 <code>start</code>，那么 <code>remote.dgateway</code> IP 地址是必需的。</li><li>防火墙阻塞了 ICMP (ping) 包。</li></ul></td>
    </tr>
    <tr>
    <td><code>vpn-strongswan-ping-remote-ip-1</code></td>
    <td>通过集群中的 VPN pod，对内部部署 VPN 网关的 <code>remote.privateIPtoPing</code> 专用 IP 地址执行 ping 操作。此测试可能因为以下原因而失败：<ul><li>未指定 <code>remote.privateIPtoPing</code> IP 地址。如果您是有意不指定 IP 地址，那么此失败是可接受的。</li><li>未在 <code>local.subnet</code> 列表中指定集群 pod 子网 CIDR <code>172.30.0.0/16</code>。</li></ul></td>
    </tr>
    <tr>
    <td><code>vpn-strongswan-ping-remote-ip-2</code></td>
    <td>通过集群中的工作程序节点，对内部部署 VPN 网关的 <code>remote.privateIPtoPing</code> 专用 IP 地址执行 ping 操作。此测试可能因为以下原因而失败：<ul><li>未指定 <code>remote.privateIPtoPing</code> IP 地址。如果您是有意不指定 IP 地址，那么此失败是可接受的。</li><li>未在 <code>local.subnet</code> 列表中指定集群工作程序节点专用子网 CIDR。</li></ul></td>
    </tr>
    </tbody></table>

6. 删除当前 Helm chart。

    ```
    helm delete --purge vpn
    ```
    {: pre}

7. 打开 `config.yaml` 文件并修正不正确的值。

8. 保存更新的 `config.yaml` 文件。

9. 使用更新的 `config.yaml` 文件将 Helm chart 安装到集群。更新的属性会存储在 chart 的配置映射中。

    ```
    helm install -f config.yaml --name=<release_name> ibm/strongswan
    ```
    {: pre}

10. 检查 chart 部署状态。当 chart 就绪时，输出顶部附近的 **STATUS** 字段的值为 `DEPLOYED`。

    ```
    helm status vpn
    ```
    {: pre}

11. 部署图表后，请验证是否使用了 `config.yaml` 文件中的已更新设置。

    ```
    helm get values vpn
    ```
    {: pre}

12. 清除当前测试 pod。

    ```
kubectl get pods -a -l app=strongswan-test
    ```
    {: pre}

    ```
kubectl delete pods -l app=strongswan-test
    ```
    {: pre}

13. 重新运行测试。

    ```
    helm test vpn
    ```
    {: pre}

<br />


## 通过名称空间或工作程序节点限制 strongSwan VPN 流量
{: #limit}

如果您有单租户集群，或者如果有多租户集群，其中的集群资源在租户之间共享，那么可以[将每个 strongSwan 部署的 VPN 流量限制为特定名称空间中的 pod](#limit_namespace)。如果多租户集群中的集群资源专用于租户，那么可以[将每个 strongSwan 部署的 VPN 流量限制为每个租户专用的工作程序节点](#limit_worker)。
{: shortdesc}

### 通过名称空间限制 strongSwan VPN 流量
{: #limit_namespace}

您有单租户或多租户集群时，可以将 VPN 流量限制为仅特定名称空间中的 pod。
{: shortdesc}

例如，假设您希望仅特定名称空间 `my-secure-namespace` 中的 pod 可通过 VPN 发送和接收数据。您不希望其他名称空间（例如，`kube-system`、`ibm-system` 或 `default`）中的 pod 访问内部部署网络。要将 VPN 流量限制为仅 `my-secure-namespace`，可以创建 Calico 全局网络策略。

使用此解决方案之前，请查看以下注意事项和限制。
* 您无需将 strongSwan Helm chart 部署到指定的名称空间中。strongSwan VPN pod 和路径守护程序集可以部署到 `kube-system` 或其他任何名称空间中。如果 strongSwan VPN 未部署到指定的名称空间中，那么 `vpn-strongswan-ping-remote-ip-1` Helm 测试会失败。这是预期故障，可以接受。测试会通过不位于对远程子网具有直接访问权的名称空间中的 pod，对内部部署 VPN 网关的 `remote.privateIPtoPing` 专用 IP 地址执行 ping 操作。但是，VPN pod 仍能够将流量转发到具有到远程子网的路径的名称空间中的 pod，并且流量仍可正常流动。VPN 状态仍为 `ESTABLISHED`，并且指定名称空间中的 pod 可以通过 VPN 进行连接。

* 以下步骤中的 Calico 全局网络策略不会阻止使用主机联网的 Kubernetes pod 通过 VPN 发送和接收数据。pod 配置为使用主机联网，在该 pod 中运行的应用程序可以侦听其所在的工作程序节点的网络接口。这些主机联网 pod 可以在任何名称空间中存在。要确定哪些 pod 具有主机联网，请运行 `kubectl get pods --all-namespaces -o wide`，并查找没有 `172.30.0.0/16` pod IP 地址的任何 pod。如果要阻止主机联网 pod 通过 VPN 发送和接收数据，那么可以在 `values.yaml` 部署文件中设置以下选项：`local.subnet: 172.30.0.0/16` 和 `enablePodSNAT: false`。这些配置设置通过 VPN 连接向远程网络公开所有 Kubernetes pod。但是，只有位于指定安全名称空间中的 pod 可通过 VPN 进行访问。

开始之前：
* [部署 strongSwan Helm chart](#vpn_configure)，并[确保 VPN 连接正常工作](#vpn_test)。
* [安装和配置 Calico CLI](/docs/containers?topic=containers-network_policies#cli_install)。

要将 VPN 流量限制为特定名称空间，请执行以下操作：

1. 创建名为 `allow-non-vpn-outbound.yaml` 的 Calico 全局网络策略。此策略允许所有名称空间继续将出站流量发送到所有目标，但 strongSwan VPN 访问的远程子网除外。将 `<remote.subnet>` 替换为您在 Helm `values.yaml` 配置文件中指定的 `remote.subnet`。要指定多个远程子网，请参阅 [Calico 文档 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://docs.projectcalico.org/v3.3/reference/calicoctl/resources/globalnetworkpolicy)。
    ```yaml
    apiVersion: projectcalico.org/v3
    kind: GlobalNetworkPolicy
    metadata:
      name: allow-non-vpn-outbound
    spec:
      selector: has(projectcalico.org/namespace)
      egress:
      - action: Allow
        destination:
          notNets:
          - <remote.subnet>
      order: 900
      types:
      - Egress
    ```
    {: codeblock}

2. 应用该策略。

    ```
    calicoctl apply -f allow-non-vpn-outbound.yaml --config=filepath/calicoctl.cfg
    ```
    {: pre}

3. 创建另一个名为 `allow-vpn-from-namespace.yaml` 的 Calico 全局网络策略。此策略仅允许指定名称空间将出站流量发送到 strongSwan VPN访问的远程子网。将 `<namespace>` 替换为可以访问 VPN 的名称空间，并将 `<remote.subnet>` 替换为在 Helm `values.yaml` 配置文件中指定的 `remote.subnet`。要指定多个名称空间或远程子网，请参阅 [Calico 文档 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://docs.projectcalico.org/v3.3/reference/calicoctl/resources/globalnetworkpolicy)。
    ```yaml
    apiVersion: projectcalico.org/v3
    kind: GlobalNetworkPolicy
    metadata:
      name: allow-vpn-from-namespace
    spec:
      selector: projectcalico.org/namespace == "<namespace>"
      egress:
      - action: Allow
        destination:
          nets:
          - <remote.subnet>
      order: 900
      types:
      - Egress
    ```
    {: codeblock}

4. 应用该策略。

    ```
    calicoctl apply -f allow-vpn-from-namespace.yaml --config=filepath/calicoctl.cfg
    ```
    {: pre}

5. 验证是否在集群中创建了全局网络策略。
    ```
calicoctl get GlobalNetworkPolicy -o wide --config=filepath/calicoctl.cfg
    ```
    {: pre}

### 通过工作程序节点限制 strongSwan VPN 流量
{: #limit_worker}

在多租户集群中具有多个 strongSwan VPN 部署时，可以将每个部署的 VPN 流量限制为专用于每个租户的特定工作程序节点。
{: shortdesc}

部署 strongSwan Helm chart 时，会创建 strongSwan VPN 部署。strongSwan VPN pod 会部署到任何无污点工作程序节点。此外，还会创建 Kubernetes 守护程序集。此守护程序集会将集群中所有无污点工作程序节点上的路径自动配置为连接到每个远程子网。在内部部署网络中，strongSwan VPN pod 使用工作程序节点上的路径将请求转发到远程子网。

除非在 `value.yaml` 文件的 `tolerations` 设置中指定了污点，否则不会在有污点的节点上配置路径。通过污染工作程序节点，可以防止在这些工作程序上配置任何 VPN 路径。然后，可以仅针对确实希望在有污点工作程序上允许的 VPN 部署，在 `tolerations` 设置中指定污点。这样，用于一个租户的 Helm chart 部署的 strongSwan VPN 将仅使用该租户的工作程序节点上的路径来通过 VPN 连接将流量转发到远程子网。

使用此解决方案之前，请查看以下注意事项和限制。
* 缺省情况下，Kubernetes 会将应用程序 pod 置于可用的任何无污点工作程序节点上。要确保此解决方案正常工作，每个租户必须首先确保仅将其应用程序 pod 部署到正确租户的有污点工作程序。此外，每个有污点工作程序节点还必须具有容忍度，以允许将应用程序 pod 放置在该节点上。有关污点和容忍度的更多信息，请参阅 [Kubernetes 文档 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/)。
* 由于任一租户均无法将应用程序 pod 置于共享的无污点节点上，因此集群资源可能未以最佳方式进行利用。

以下用于通过工作程序节点限制 strongSwan VPN 流量的步骤使用此示例方案：假设您有一个多租户 {{site.data.keyword.containerlong_notm}} 集群，该集群有六个工作程序节点。该集群支持租户 A 和租户 B。您通过以下方式污染了工作程序节点：
* 污染了两个工作程序节点，以便仅将租户 A pod 安排在这两个工作程序上。
* 污染了两个工作程序节点，以便仅将租户 B pod 安排在这两个工作程序上。
* 有两个工作程序节点未污染，因为至少需要 2 个工作程序节点来运行 strongSwan VPN pod 和负载均衡器 IP。

要将 VPN 流量限制为每个租户的有污点节点，请执行以下操作：

1. 要将 VPN 流量限制为仅专用于本示例中租户 A 的工作程序，请在租户 A 的 strongSwan Helm chart 的 `values.yaml` 文件中指定以下 `toleration`：
    ```
    tolerations:
     - key: dedicated
       operator: "Equal"
       value: "tenantA"
       effect: "NoSchedule"
    ```
    {: codeblock}
    此容忍度允许路径守护程序集在有 `dedicated="tenantA"` 污点的两个工作程序节点上以及两个无污点工作程序节点上运行。此部署的 strongSwan VPN 会在两个无污点工作程序节点上运行。

2. 要将 VPN 流量限制为仅专用于本示例中租户 B 的工作程序，请在租户 B 的 strongSwan Helm chart 的 `values.yaml` 文件中指定以下 `toleration`：
    ```
    tolerations:
     - key: dedicated
       operator: "Equal"
       value: "tenantB"
       effect: "NoSchedule"
    ```
    {: codeblock}
    此容忍度允许路径守护程序集在有 `dedicated="tenantB"` 污点的两个工作程序节点上以及两个无污点工作程序节点上运行。此部署的 strongSwan VPN 同样会在两个无污点工作程序节点上运行。

<br />


## 升级 strongSwan Helm chart
{: #vpn_upgrade}

通过升级 stronSwan Helm chart 以确保该图表是最新的。
{:shortdesc}

要将 strongSwan Helm chart 升级到最新版本，请执行以下操作：

  ```
  helm upgrade -f config.yaml <release_name> ibm/strongswan
  ```
  {: pre}

## 禁用 strongSwan IPSec VPN 服务
{: vpn_disable}

可以通过删除 Helm chart 来禁用 VPN 连接。
{:shortdesc}

  ```
  helm delete --purge <release_name>
  ```
  {: pre}

<br />


## 使用虚拟路由器设备
{: #vyatta}

[虚拟路由器设备 (VRA)](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-about-the-vra) 为 x86 裸机服务器提供最新的 Vyatta 5600 操作系统。可以使用 VRA 作为 VPN 网关来安全地连接到内部部署网络。
{:shortdesc}

所有进出集群 VLAN 的公用和专用网络流量都将通过 VRA 进行路由。可以使用 VRA 作为 VPN 端点，以在 IBM Cloud Infrastructure (SoftLayer) 和内部部署资源中的服务器之间创建加密的 IPSec 隧道。例如，下图显示了 {{site.data.keyword.containerlong_notm}} 中仅限专用的工作程序节点上的应用程序可以如何通过 VRA VPN 连接与内部部署服务器进行通信：

<img src="images/cs_vpn_vyatta.png" width="725" alt="使用负载均衡器在 {{site.data.keyword.containerlong_notm}} 中公开应用程序" style="width:725px; border-style: none"/>

1. 集群中的应用程序 `myapp2` 接收来自 Ingress 或 LoadBalancer 服务的请求，并且需要安全地连接到内部部署网络中的数据。

2. 因为 `myapp2` 位于仅在专用 VLAN 上的工作程序节点上，所以 VRA 充当工作程序节点与内部部署网络之间的安全连接。VRA 使用目标 IP 地址来确定将哪些网络包发送到内部部署网络。

3. 该请求已加密，并通过 VPN 隧道发送到内部部署数据中心。

4. 入局请求通过内部部署防火墙传递，并传递到将在其中进行解密的 VPN 隧道端点（路由器）。

5. VPN 隧道端点（路由器）将该请求转发到内部部署服务器或大型机，具体取决于步骤 2 中指定的目标 IP 地址。必需的数据通过相同的过程经由 VPN 连接发送回 `myapp2`。

要设置虚拟路由器设备，请执行以下操作：

1. [订购 VRA](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-getting-started)。

2. [在 VRA 上配置专用 VLAN](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-managing-your-vlans)。

3. 要使用 VRA 来启用 VPN 连接，请[在 VRA 上配置 VRRP](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-working-with-high-availability-and-vrrp#high-availability-vpn-with-vrrp)。

如果您有现有路由器设备，然后添加了集群，那么不会在该路由器设备上配置为集群订购的新可移植子网。要使用联网服务，必须通过[启用 VLAN 生成](/docs/containers?topic=containers-subnets#subnet-routing)来启用同一 VLAN 上子网之间的路由。要检查是否已启用 VLAN 生成，请使用 `ibmcloud ks vlan-spanning-get --region <region>` [命令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get)。
{: important}
