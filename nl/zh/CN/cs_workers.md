---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-06"

keywords: kubernetes, iks, clusters, worker nodes, worker pools, delete

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
{:gif: data-image-type='gif'}



# 向集群添加工作程序节点和专区
{: #add_workers}

要提高应用程序的可用性，可以将工作程序节点添加到集群中的一个或多个现有专区中。为了帮助保护应用程序免受专区故障的影响，您可以向集群添加专区。
{:shortdesc}

创建集群时，会在工作程序池中供应工作程序节点。创建集群后，可以通过调整其大小或添加更多工作程序池，将更多工作程序节点添加到池。缺省情况下，工作程序池存在于一个专区中。仅在一个专区中有工作程序池的集群称为单专区集群。向集群添加更多专区时，该工作程序池会跨多个专区。具有跨多个专区分布的工作程序池的集群称为多专区集群。

如果您有多专区集群，请使其工作程序节点资源保持均衡。确保所有工作程序池跨相同专区进行分布，并通过调整池大小（而不采用添加单个节点的方式）来添加或除去工作程序。
{: tip}

开始之前，请确保您具有 [{{site.data.keyword.Bluemix_notm}} IAM **操作员**或**管理员**平台角色](/docs/containers?topic=containers-users#platform)。然后，选择下列其中一个部分：
  * [通过调整集群中现有工作程序池的大小来添加工作程序节点](#resize_pool)
  * [通过向集群添加工作程序池来添加工作程序节点](#add_pool)
  * [向集群添加专区并在跨多个专区的工作程序池中复制工作程序节点](#add_zone)
  * [不推荐：向集群添加独立工作程序节点](#standalone)

设置工作程序池后，可以[设置集群自动缩放器](/docs/containers?topic=containers-ca#ca)，以根据工作负载资源请求，自动在工作程序池中添加或除去工作程序节点。
{:tip}

## 通过调整集群中现有工作程序池的大小来添加工作程序节点
{: #resize_pool}

您可以通过调整现有工作程序池的大小来添加或减少集群中的工作程序节点数，而不管工作程序池是位于一个专区中还是或跨多个专区分布。
{: shortdesc}

例如，假设一个集群有一个工作程序池，该工作程序池在每个专区有 3 个工作程序节点。
* 如果该集群是单专区集群，并且存在于 `dal10` 中，那么工作程序池在 `dal10` 中有 3 个工作程序节点。集群共有 3 个工作程序节点。
* 如果该集群是多专区集群，并且存在于 `dal10` 和 `dal12` 中，那么工作程序池在 `dal10` 中有 3 个工作程序节点，在 `dal12` 中有 3 个工作程序节点。集群共有 6 个工作程序节点。

请记住，裸机工作程序池是按月计费的。如果向上或向下调整大小，都会影响您当月的成本。
{: tip}

要调整工作程序池大小，请更改工作程序池在每个专区中部署的工作程序节点数：

1. 获取要调整其大小的工作程序池的名称。
    ```
    ibmcloud ks worker-pools --cluster <cluster_name_or_ID>
    ```
    {: pre}

2. 通过指定要在每个专区中部署的工作程序节点数来调整工作程序池的大小。最小值为 1。
    ```
    ibmcloud ks worker-pool-resize --cluster <cluster_name_or_ID> --worker-pool <pool_name>  --size-per-zone <number_of_workers_per_zone>
    ```
    {: pre}

3. 验证工作程序池的大小是否已调整。
    ```
    ibmcloud ks workers --cluster <cluster_name_or_ID> --worker-pool <pool_name>
    ```
    {: pre}

    两个专区（`dal10` 和 `dal12`）中的工作程序池的输出示例，此池的大小已调整为每个专区 2 个工作程序节点：
    ```
    ID                                                 Public IP        Private IP      Machine Type      State    Status  Zone    Version
    kube-dal10-crb20b637238ea471f8d4a8b881aae4962-w7   169.xx.xxx.xxx   10.xxx.xx.xxx   b3c.4x16          normal   Ready   dal10   1.8.6_1504
    kube-dal10-crb20b637238ea471f8d4a8b881aae4962-w8   169.xx.xxx.xxx   10.xxx.xx.xxx   b3c.4x16          normal   Ready   dal10   1.8.6_1504
    kube-dal12-crb20b637238ea471f8d4a8b881aae4962-w9   169.xx.xxx.xxx   10.xxx.xx.xxx   b3c.4x16          normal   Ready   dal12   1.8.6_1504
    kube-dal12-crb20b637238ea471f8d4a8b881aae4962-w10  169.xx.xxx.xxx   10.xxx.xx.xxx   b3c.4x16          normal   Ready   dal12   1.8.6_1504
    ```
    {: screen}

## 通过创建新的工作程序池来添加工作程序节点
{: #add_pool}

可以通过创建新的工作程序池，向集群添加工作程序节点。
{:shortdesc}

1. 检索集群的**工作程序专区**，并选择要在其中部署工作程序池中工作程序节点的专区。如果您具有单专区集群，那么必须使用在 **Worker Zones** 字段中看到的专区。对于多专区集群，可以选择集群的任何现有**工作程序专区**，也可以为集群所在的区域添加其中一个[多专区大城市位置](/docs/containers?topic=containers-regions-and-zones#zones)。可以通过运行 `ibmcloud ks zones` 来列出可用专区。
   ```
   ibmcloud ks cluster-get --cluster <cluster_name_or_ID>
   ```
   {: pre}

   输出示例：
   ```
   ...
   Worker Zones: dal10, dal12, dal13
   ```
   {: screen}

2. 对于每个专区，列出可用的专用和公用 VLAN。请记下要使用的专用和公用 VLAN。如果没有专用或公用 VLAN，那么在向工作程序池添加专区时，会自动创建 VLAN。
   ```
   ibmcloud ks vlans --zone <zone>
   ```
   {: pre}

3.  对于每个专区，请查看[可用于工作程序节点的机器类型](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)。

    ```
    ibmcloud ks machine-types <zone>
    ```
    {: pre}

4. 创建工作程序池。包含 `--labels` 选项以使用 `key=value` 标签自动标注池中的工作程序节点。如果供应的是裸机工作程序池，请指定 `--hardware dedicated`。
   ```
   ibmcloud ks worker-pool-create --name <pool_name> --cluster <cluster_name_or_ID> --machine-type <machine_type> --size-per-zone <number_of_workers_per_zone> --hardware <dedicated_or_shared> --labels <key=value>
   ```
   {: pre}

5. 验证工作程序池是否已创建。
   ```
   ibmcloud ks worker-pools --cluster <cluster_name_or_ID>
   ```
   {: pre}

6. 缺省情况下，添加工作程序池将创建不包含专区的池。要在专区中部署工作程序节点，必须将先前检索到的专区添加到工作程序池。如果要跨多个专区分布工作程序节点，请对每个专区重复此命令。
   ```
ibmcloud ks zone-add --zone <zone> --cluster <cluster_name_or_ID> --worker-pools <pool_name> --private-vlan <private_VLAN_ID> --public-vlan <public_VLAN_ID>
      ```
   {: pre}

7. 验证工作程序节点是否在添加的专区中供应。当状态从 **provision_pending** 更改为 **normal** 时，说明工作程序节点已就绪。
   ```
   ibmcloud ks workers --cluster <cluster_name_or_ID> --worker-pool <pool_name>
   ```
   {: pre}

   输出示例：
   ```
   ID                                                 Public IP        Private IP      Machine Type      State    Status  Zone    Version
   kube-dal10-crb20b637238ea471f8d4a8b881aae4962-w7   169.xx.xxx.xxx   10.xxx.xx.xxx   b3c.4x16          provision_pending   Ready   dal10   1.8.6_1504
   kube-dal10-crb20b637238ea471f8d4a8b881aae4962-w8   169.xx.xxx.xxx   10.xxx.xx.xxx   b3c.4x16          provision_pending   Ready   dal10   1.8.6_1504
   ```
   {: screen}

## 通过向工作程序池添加专区来添加工作程序节点
{: #add_zone}

可以通过向现有工作程序池添加专区，使集群跨一个区域内的多个专区。
{:shortdesc}

将专区添加到工作程序池时，将在新专区中供应工作程序池中定义的工作程序节点数，并考虑用于未来的工作负载安排。{{site.data.keyword.containerlong_notm}} 会自动将区域的 `failure-domain.beta.kubernetes.io/region` 标签和专区的 `failure-domain.beta.kubernetes.io/zone` 标签添加到每个工作程序节点。Kubernetes 调度程序使用这些标签在同一区域内的各个专区之间分布 pod。

如果集群中有多个工作程序池，请将该专区添加到所有这些工作程序池，以便工作程序节点在集群中均匀分布。

开始之前：
*  要将专区添加到工作程序池，工作程序池必须位于[支持多专区的专区](/docs/containers?topic=containers-regions-and-zones#zones)中。如果工作程序池不位于支持多专区的专区中，请考虑[创建新的工作程序池](#add_pool)。
*  如果有多个 VLAN 用于一个集群，有多个子网位于同一 VLAN 上或有一个多专区集群，那么必须针对 IBM Cloud Infrastructure (SoftLayer) 帐户启用[虚拟路由器功能 (VRF)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud)，从而使工作程序节点可以在专用网络上相互通信。要启用 VRF，请[联系 IBM Cloud Infrastructure (SoftLayer) 客户代表](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#how-you-can-initiate-the-conversion)。如果无法或不想启用 VRF，请启用 [VLAN 生成](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning)。要执行此操作，您需要有**网络 > 管理网络 VLAN 生成**[基础架构许可权](/docs/containers?topic=containers-users#infra_access)，也可以请求帐户所有者来启用 VLAN 生成。要检查是否已启用 VLAN 生成，请使用 `ibmcloud ks vlan-spanning-get --region <region>` [命令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get)。

要将具有工作程序节点的专区添加到工作程序池，请执行以下操作：

1. 列出可用专区，然后选取要添加到工作程序池的专区。选择的专区必须是支持多专区的专区。
   ```
   ibmcloud ks zones
   ```
   {: pre}

2. 列出该专区中可用的 VLAN。如果没有专用或公用 VLAN，那么在向工作程序池添加专区时，会自动创建 VLAN。
   ```
   ibmcloud ks vlans --zone <zone>
   ```
   {: pre}

3. 列出集群中的工作程序池并记下其名称。
   ```
   ibmcloud ks worker-pools --cluster <cluster_name_or_ID>
   ```
   {: pre}

4. 将专区添加到工作程序池。如果您有多个工作程序池，请将专区添加到所有工作程序池，以便在所有专区中均衡集群。将 `<pool1_id_or_name,pool2_id_or_name,...>` 替换为所有工作程序池的名称（采用逗号分隔列表形式）。

    必须存在专用和公用 VLAN，才能将专区添加到多个工作程序池。如果在该专区中没有专用和公用 VLAN，请首先将该专区添加到一个工作程序池，以便创建专用和公用 VLAN。然后，可以通过指定创建的专用和公用 VLAN，将该专区添加到其他工作程序池。
    {: note}

   如果要对不同工作程序池使用不同的 VLAN，请对每个 VLAN 及其相应的工作程序池重复此命令。任何新的工作程序节点都会添加到指定的 VLAN，但不会更改任何现有工作程序节点的 VLAN。
      {: tip}
   ```
   ibmcloud ks zone-add --zone <zone> --cluster <cluster_name_or_ID> --worker-pools <pool1_name,pool2_name,...> --private-vlan <private_VLAN_ID> --public-vlan <public_VLAN_ID>
   ```
   {: pre}

5. 验证是否已将专区添加到集群。在输出的 **Worker zones** 字段中查找添加的专区。请注意，在添加的专区中供应了新的工作程序节点，因此 **Workers** 字段中的工作程序总数已增加。
    
  ```
   ibmcloud ks cluster-get --cluster <cluster_name_or_ID>
   ```
  {: pre}

  输出示例：
  ```
  Name:                           mycluster
  ID:                             df253b6025d64944ab99ed63bb4567b6
  State:                          normal
  Created:                        2018-09-28T15:43:15+0000
  Location:                       dal10
  Master URL:                     https://c3.<region>.containers.cloud.ibm.com:30426
  Public Service Endpoint URL:    https://c3.<region>.containers.cloud.ibm.com:30426
  Private Service Endpoint URL:   https://c3-private.<region>.containers.cloud.ibm.com:31140
  Master Location:                Dallas
  Master Status:                  Ready (21 hours ago)
  Ingress Subdomain:              mycluster.us-south.containers.appdomain.cloud
  Ingress Secret:                 mycluster
  Workers:                        6
  Worker Zones:                   dal10, dal12
  Version:                        1.11.3_1524
  Owner:                          owner@email.com
  Resource Group ID:              a8a12accd63b437bbd6d58fb6a462ca7
  Resource Group Name:            Default
  ```
  {: screen}

## 不推荐：添加独立工作程序节点
{: #standalone}

如果您的集群是在引入工作程序池之前创建的，那么可以使用不推荐的命令来添加独立工作程序节点。
{: deprecated}

如果您的集群是在引入工作程序池后创建的，那么无法添加独立工作程序节点。您可以改为通过[创建工作程序池](#add_pool)、[调整现有工作程序池大小](#resize_pool)或[向工作程序池添加专区](#add_zone)，将工作程序节点添加到集群。
{: note}

1. 列出可用专区，然后选取要添加工作程序节点的专区。
   ```
   ibmcloud ks zones
   ```
   {: pre}

2. 列出该专区中的可用 VLAN，并记下其标识。
   ```
   ibmcloud ks vlans --zone <zone>
   ```
   {: pre}

3. 列出该专区中的可用机器类型。
   ```
   ibmcloud ks machine-types --zone <zone>
   ```
   {: pre}

4. 向集群添加独立工作程序节点。对于裸机机器类型，请指定 `dedicated`。
   ```
   ibmcloud ks worker-add --cluster <cluster_name_or_ID> --workers <number_of_worker_nodes> --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID> --machine-type <machine_type> --hardware <shared_or_dedicated>
   ```
   {: pre}

5. 验证工作程序节点是否已创建。
   ```
   ibmcloud ks workers --cluster <cluster_name_or_ID>
   ```
   {: pre}

<br />


## 向现有工作程序池添加标签
{: #worker_pool_labels}

可以在[创建工作程序池](#add_pool)时为工作程序池分配标签，也可以日后通过执行以下步骤来分配标签。在标注工作程序池后，所有现有及后续工作程序节点都将获得此标签。您可使用标签将特定工作负载仅部署到该工作程序池中的工作程序节点，例如[用于负载均衡器网络流量的边缘节点](/docs/containers?topic=containers-edge)。
{: shortdesc}

开始之前：[登录到您的帐户。如果适用，请将相应的资源组设定为目标。为集群设置上下文。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  列出集群中的工作程序池。
    ```
    ibmcloud ks worker-pools --cluster <cluster_name_or_ID>
    ```
    {: pre}
2.  要使用 `key=value` 标签来标注工作程序池，请使用 [PATCH 工作程序池 API ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/PatchWorkerPool)。按照以下 JSON 示例的格式，设置请求主体的格式。
    ```
    {
      "labels": {"key":"value"},
      "state": "labels"
    }
    ```
    {: codeblock}
3.  验证工作程序池和工作程序节点是否具有分配的 `key=value` 标签。
    *   要检查工作程序池，请运行以下命令：
        ```
        ibmcloud ks worker-pool-get --cluster <cluster_name_or_ID> --worker-pool <worker_pool_name_or_ID>
        ```
        {: pre}
    *   要检查工作程序节点，请执行以下操作：
        1.  列出工作程序池中的工作程序节点，并记下 **Private IP**。
            ```
        ibmcloud ks workers --cluster <cluster_name_or_ID> --worker-pool <worker_pool_name_or_ID>
        ```
            {: pre}
        2.  查看输出的 **Labels** 字段。
            ```
    kubectl describe node <worker_node_private_IP>
    ```
            {: pre}

标注工作程序池后，可以[在应用程序部署中使用标签](/docs/containers?topic=containers-app#label)，以便工作负载只能在这些工作程序节点上运行，或者可以使用[污点 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/)，以阻止部署在这些工作程序节点上运行。

<br />


## 自动恢复工作程序节点
{: #planning_autorecovery}

关键组件（例如，`containerd`、`kubelet`、`kube-proxy` 和 `calico`）必须正常运行才能拥有正常运行的 Kubernetes 工作程序节点。随着时间变化，这些组件可能会中断，这可能使工作程序节点处于非正常运行状态。非正常运行的工作程序节点会使集群总容量下降，并可能导致应用程序产生停机时间。
{:shortdesc}

可以[为工作程序节点配置运行状况检查并启用自动恢复](/docs/containers?topic=containers-health#autorecovery)。如果自动恢复根据配置的检查，检测到运行状况欠佳的工作程序节点，那么自动恢复会触发更正操作，例如在工作程序节点上重装操作系统。有关自动恢复的工作方式的更多信息，请参阅[自动恢复博客 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/blogs/bluemix/2017/12/autorecovery-utilizes-consistent-hashing-high-availability/)。
