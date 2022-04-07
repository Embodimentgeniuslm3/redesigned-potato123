---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-06"

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


# 更改服务端点或 VLAN 连接
{: #cs_network_cluster}

[创建集群](/docs/containers?topic=containers-clusters)时初始设置网络后，可以更改可借以访问 Kubernetes 主节点的服务端点，或更改工作程序节点的 VLAN 连接。
{: shortdesc}

## 设置专用服务端点
{: #set-up-private-se}

在运行 Kubernetes V1.11 或更高版本的集群中，启用或禁用集群的专用服务端点。
{: shortdesc}

通过专用服务端点，能使 Kubernetes 主节点可供专用访问。工作程序节点和授权集群用户可以通过专用网络与 Kubernetes 主节点进行通信。要确定是否可以启用专用服务端点，请参阅[工作程序与主节点的通信以及用户与主节点的通信](/docs/containers?topic=containers-plan_clusters#internet-facing)。请注意，在启用专用服务端点后，即无法将其禁用。

在为 [VRF](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) 和[服务端点](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started)启用帐户之前，创建的集群是否仅具有专用服务端点？如果是，请尝试[设置公共服务端点](#set-up-public-se)，以便可以一直使用集群，直到支持案例得到处理，可以更新帐户。
{: tip}

1. 在您的 IBM Cloud Infrastructure (SoftLayer) 帐户中启用 [VRF](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud)。
2. [启用 {{site.data.keyword.Bluemix_notm}} 帐户以使用服务端点](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started)。
3. 启用专用服务端点。
   ```
  ibmcloud ks cluster-feature-enable private-service-endpoint --cluster <cluster_name_or_ID>
  ```
   {: pre}
4. 刷新 Kubernetes 主节点 API 服务器以使用专用服务端点。您可以遵循 CLI 中的提示进行操作，也可以手动运行以下命令。
   ```
  ibmcloud ks apiserver-refresh --cluster <cluster_name_or_ID>
  ```
   {: pre}

5. [创建配置映射](/docs/containers?topic=containers-update#worker-up-configmap)，用于控制集群中可以同时不可用的最大工作程序节点数。更新工作程序节点时，配置映射可帮助防止应用程序产生停机时间，因为应用程序会有序地重新安排到可用的工作程序节点上。
6. 更新集群中的所有工作程序节点以选取专用服务端点配置。

   <p class="important">通过发出更新命令，会重新装入工作程序节点以选取服务端点配置。如果没有工作程序更新可用，那么必须[手动重新装入工作程序节点](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli)。如果重新装入，请确保封锁、放弃和管理工作程序节点，以控制可以同时不可用的最大工作程序节点数。</p>
   ```
  ibmcloud ks worker-update --cluster <cluster_name_or_ID> --workers <worker1,worker2>
  ```
   {: pre}

8. 如果集群位于防火墙后的环境中：
  * [允许授权集群用户运行 `kubectl` 命令，以通过专用服务端点来访问主节点](/docs/containers?topic=containers-firewall#firewall_kubectl)。
  * 针对基础架构资源和计划使用的 {{site.data.keyword.Bluemix_notm}} 服务，[允许流至专用 IP 的出站网络流量](/docs/containers?topic=containers-firewall#firewall_outbound)。

9. 可选：要仅使用专用服务端点，请禁用公共服务端点。
   ```
  ibmcloud ks cluster-feature-disable public-service-endpoint --cluster <cluster_name_or_ID>
  ```
   {: pre}

<br />


## 设置公共服务端点
{: #set-up-public-se}

启用或禁用集群的公共服务端点。
{: shortdesc}

通过公共服务端点，能使 Kubernetes 主节点可供公共访问。工作程序节点和授权集群用户可以安全地通过公用网络与 Kubernetes 主节点进行通信。有关更多信息，请参阅[工作程序与主节点的通信以及用户与主节点的通信](/docs/containers?topic=containers-plan_clusters#internet-facing)。

**启用的步骤**</br>
如果先前禁用了公共端点，那么可以重新启用该端点。
1. 启用公共服务端点。
   ```
  ibmcloud ks cluster-feature-enable public-service-endpoint --cluster <cluster_name_or_ID>
  ```
   {: pre}
2. 刷新 Kubernetes 主节点 API 服务器以使用公共服务端点。您可以遵循 CLI 中的提示进行操作，也可以手动运行以下命令。
   ```
  ibmcloud ks apiserver-refresh --cluster <cluster_name_or_ID>
  ```
   {: pre}

   </br>

**执行禁用的步骤**</br>
要禁用公共服务端点，必须首先启用专用服务端点，以便工作程序节点可以与 Kubernetes 主节点进行通信。
1. 启用专用服务端点。
   ```
  ibmcloud ks cluster-feature-enable private-service-endpoint --cluster <cluster_name_or_ID>
  ```
   {: pre}
2. 通过遵循 CLI 提示进行操作或手动运行以下命令，刷新 Kubernetes 主节点 API 服务器以使用专用服务端点。
   ```
  ibmcloud ks apiserver-refresh --cluster <cluster_name_or_ID>
  ```
   {: pre}
3. [创建配置映射](/docs/containers?topic=containers-update#worker-up-configmap)，用于控制集群中可以同时不可用的最大工作程序节点数。更新工作程序节点时，配置映射可帮助防止应用程序产生停机时间，因为应用程序会有序地重新安排到可用的工作程序节点上。

4. 更新集群中的所有工作程序节点以选取专用服务端点配置。

   <p class="important">通过发出更新命令，会重新装入工作程序节点以选取服务端点配置。如果没有工作程序更新可用，那么必须[手动重新装入工作程序节点](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli)。如果重新装入，请确保封锁、放弃和管理工作程序节点，以控制可以同时不可用的最大工作程序节点数。</p>
   ```
  ibmcloud ks worker-update --cluster <cluster_name_or_ID> --workers <worker1,worker2>
  ```
  {: pre}
5. 禁用公共服务端点。
   ```
  ibmcloud ks cluster-feature-disable public-service-endpoint --cluster <cluster_name_or_ID>
  ```
   {: pre}

## 从公共服务端点切换到专用服务端点
{: #migrate-to-private-se}

在运行 Kubernetes V1.11 或更高版本的集群中，通过启用专用服务端点，使工作程序节点能够通过专用网络而不是公用网络与主节点进行通信。
{: shortdesc}

缺省情况下，连接到公用和专用 VLAN 的所有集群都使用公共服务端点。工作程序节点和授权集群用户可以安全地通过公用网络与 Kubernetes 主节点进行通信。要使工作程序节点能够通过专用网络而不是公用网络与 Kubernetes 主节点进行通信，可以启用专用服务端点。之后，您可以选择禁用公共服务端点。
* 如果启用了专用服务端点，并同时使公共服务端点保持启用状态，那么工作程序始终通过专用网络与主节点进行通信，但用户可以通过公用网络或专用网络与主节点进行通信。
* 如果启用了专用服务端点，而禁用了公共服务端点，那么工作程序和用户必须通过专用网络与主节点进行通信。

请注意，在启用专用服务端点后，即无法将其禁用。

1. 在您的 IBM Cloud Infrastructure (SoftLayer) 帐户中启用 [VRF](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud)。
2. [启用 {{site.data.keyword.Bluemix_notm}} 帐户以使用服务端点](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started)。
3. 启用专用服务端点。
   ```
  ibmcloud ks cluster-feature-enable private-service-endpoint --cluster <cluster_name_or_ID>
  ```
   {: pre}
4. 通过遵循 CLI 提示进行操作或手动运行以下命令，刷新 Kubernetes 主节点 API 服务器以使用专用服务端点。
   ```
  ibmcloud ks apiserver-refresh --cluster <cluster_name_or_ID>
  ```
   {: pre}
5. [创建配置映射](/docs/containers?topic=containers-update#worker-up-configmap)，用于控制集群中可以同时不可用的最大工作程序节点数。更新工作程序节点时，配置映射可帮助防止应用程序产生停机时间，因为应用程序会有序地重新安排到可用的工作程序节点上。

6.  更新集群中的所有工作程序节点以选取专用服务端点配置。

    <p class="important">通过发出更新命令，会重新装入工作程序节点以选取服务端点配置。如果没有工作程序更新可用，那么必须[手动重新装入工作程序节点](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli)。如果重新装入，请确保封锁、放弃和管理工作程序节点，以控制可以同时不可用的最大工作程序节点数。</p>
    ```
    ibmcloud ks worker-update --cluster <cluster_name_or_ID> --workers <worker1,worker2>
    ```
    {: pre}

7. 可选：禁用公共服务端点。
   ```
  ibmcloud ks cluster-feature-disable public-service-endpoint --cluster <cluster_name_or_ID>
  ```
   {: pre}

<br />


## 更改工作程序节点 VLAN 连接
{: #change-vlans}

创建集群时，选择是将工作程序节点连接到专用和公用 VLAN，还是连接到仅专用 VLAN。工作程序节点是工作程序池的一部分，用于存储联网元数据，其中包含要用于供应池中未来工作程序节点的 VLAN。日后遇到如下情况时，您可能希望更改集群的 VLAN 连接设置。
{: shortdesc}

* 专区中的工作程序池 VLAN 的容量不足，您需要供应一个新的 VLAN 以供集群工作程序节点使用。
* 您有一个集群，包含的工作程序节点同时位于公用和专用 VLAN 上，但您希望更改为[仅专用集群](/docs/containers?topic=containers-plan_clusters#private_clusters)。
* 您具有仅专用的集群，但需要某些工作程序节点（例如，公用 VLAN 上的[边缘节点](/docs/containers?topic=containers-edge#edge)的工作程序池）在因特网上公开应用程序。

要尝试转而更改用于主节点与工作程序之间通信的服务端点？请查看设置[公共](#set-up-public-se)和[专用](#set-up-private-se)服务端点的主题。
{: tip}

开始之前：
* [登录到您的帐户。如果适用，请将相应的资源组设定为目标。为集群设置上下文。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
* 如果工作程序节点是独立节点（不是工作程序池的一部分），请[将其更新到工作程序池](/docs/containers?topic=containers-update#standalone_to_workerpool)。

要更改工作程序池用于供应工作程序节点的 VLAN，请执行以下操作：

1. 列出集群中工作程序池的名称。
  ```
  ibmcloud ks worker-pools --cluster <cluster_name_or_ID>
  ```
  {: pre}

2. 确定其中一个工作程序池的专区。在输出中，查找 **Zones** 字段。
  ```
  ibmcloud ks worker-pool-get --cluster <cluster_name_or_ID> --worker-pool <pool_name>
  ```
  {: pre}

3. 对于在上一步中找到的每个专区，获取相互兼容的可用公用和专用 VLAN。

  1. 检查输出中 **Type** 下列出的可用公用和专用 VLAN。
    ```
    ibmcloud ks vlans --zone <zone>
    ```
     {: pre}

  2. 检查专区中的公用和专用 VLAN 是否兼容。要使两者兼容，**Router** 必须具有相同的 pod 标识。在此输出示例中，**Router** pod 标识匹配：`01a` 和 `01a`。如果一个 pod 标识为 `01a`，另一个为 `02a`，那么无法为工作程序池设置公用和专用 VLAN 标识。
    ```
    ID        Name   Number   Type      Router         Supports Virtual Workers
    229xxxx          1234     private   bcr01a.dal12   true
    229xxxx          5678     public    fcr01a.dal12   true
    ```
     {: screen}

  3. 如果需要为专区订购新的公用或专用 VLAN，那么可以在 [{{site.data.keyword.Bluemix_notm}} 控制台](/docs/infrastructure/vlans?topic=vlans-ordering-premium-vlans#ordering-premium-vlans)中进行订购或使用以下命令。请记住，VLAN 必须兼容，并且 **Router** pod 标识匹配，如上一步中所示。如果要创建一对新的公用和专用 VLAN，那么这些 VLAN 必须相互兼容。
    ```
    ibmcloud sl vlan create -t [public|private] -d <zone> -r <compatible_router>
    ```
     {: pre}

  4. 记下兼容 VLAN 的标识。

4. 为每个专区设置具有新的 VLAN 网络元数据的工作程序池。可以创建新的工作程序池，也可以修改现有工作程序池。

  * **创建新的工作程序池**：请参阅[通过创建新的工作程序池来添加工作程序节点](/docs/containers?topic=containers-add_workers#add_pool)。

  * **修改现有工作程序池**：设置工作程序池的网络元数据以将 VLAN 用于每个专区。池中已创建的工作程序节点会继续使用先前的 VLAN，但池中的新工作程序节点将使用设置的新 VLAN 元数据。

    * 添加公用和专用 VLAN 的示例（例如，从仅专用 VLAN 更改为专用和公共 VLAN）：
      ```
      ibmcloud ks zone-network-set --zone <zone> --cluster <cluster_name_or_ID> --worker-pools <pool_name> --private-vlan <private_vlan_id> --public-vlan <public_vlan_id>
      ```
      {: pre}

    * 添加仅专用 VLAN 的示例（例如，在您有使用服务端点的[启用 VRF 的帐户](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started)时，从公用和专用 VLAN 更改为仅专用 VLAN）：
      ```
      ibmcloud ks zone-network-set --zone <zone> --cluster <cluster_name_or_ID> --worker-pools <pool_name> --private-vlan <private_vlan_id> --public-vlan <public_vlan_id>
      ```
      {: pre}

5. 通过调整池大小，向工作程序池添加工作程序节点。
   ```
  ibmcloud ks worker-pool-resize --cluster <cluster_name_or_ID> --worker-pool <pool_name> --size-per-zone <number_of_workers_per_zone>
  ```
   {: pre}

   如果要除去使用先前网络元数据的工作程序节点，请将每个专区的工作程序数更改为先前每个专区的工作程序数的两倍。以后在这些步骤中，可以封锁、放弃和除去先前的工作程序节点。
  {: tip}

6. 验证是否新工作程序节点是使用输出中的相应 **Public IP** 和 **Private IP** 创建的。例如，如果将工作程序池从公用和专用 VLAN 更改为仅专用 VLAN，那么新的工作程序节点仅具有专用 IP。如果将工作程序池从仅专用 VLAN 更改为公用和专用 VLAN，那么新的工作程序节点将同时具有公用和专用 IP。
   ```
   ibmcloud ks workers --cluster <cluster_name_or_ID> --worker-pool <pool_name>
   ```
   {: pre}

7. 可选：从工作程序池中除去具有先前网络元数据的工作程序节点。
  1. 在上一步的输出中，记下要从工作程序池中除去的工作程序节点的 **ID** 和 **Private IP**。
  2. 在称为“封锁”的过程中将工作程序节点标记为不可安排。封锁工作程序节点后，该节点即不可用于未来的 pod 安排。
    ```
    kubectl cordon <worker_private_ip>
    ```
     {: pre}
  3. 验证是否已对工作程序节点禁用了 pod 安排。
      ```
kubectl get nodes
```
     {: pre}
如果阶段状态显示为 **`SchedulingDisabled`**，说明已禁止工作程序节点用于 pod 安排。

  4. 强制从工作程序节点中除去 pod，并将其重新安排到集群中的剩余工作程序节点上。
    ```
    kubectl drain <worker_private_ip>
    ```
     {: pre}
此过程可能需要几分钟时间。
  5. 除去工作程序节点。使用先前检索到的工作程序标识。
    ```
   ibmcloud ks worker-rm --cluster <cluster_name_or_ID> --worker <worker_name_or_ID>
   ```
     {: pre}
  6. 验证工作程序节点是否已除去。
    ```
    ibmcloud ks workers --cluster <cluster_name_or_ID> --worker-pool <pool_name>
    ```
     {: pre}

8. 可选：对于集群中的每个工作程序池，可以重复步骤 2 到 7。完成这些步骤后，集群中的所有工作程序节点都将设置为使用新的 VLAN。

9. 集群中的缺省 ALB 仍绑定到旧 VLAN，因为这些 ALB 的 IP 地址来自该 VLAN 上的子网。由于 ALB 无法在不同 VLAN 之间移动，因此可以改为[在新 VLAN 上创建 ALB，然后禁用旧 VLAN 上的 ALB](/docs/containers?topic=containers-ingress#migrate-alb-vlan)。
