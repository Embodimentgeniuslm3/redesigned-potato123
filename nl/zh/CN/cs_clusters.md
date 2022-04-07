---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-07"

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


# 创建集群
{: #clusters}

在 {{site.data.keyword.containerlong}} 中创建 Kubernetes 集群。
{: shortdesc}

您已经开始设置集群了吗？可尝试使用[创建 Kubernetes 集群教程](/docs/containers?topic=containers-cs_cluster_tutorial#cs_cluster_tutorial)。
不确定要选择哪种集群设置？请参阅[规划集群网络设置](/docs/containers?topic=containers-plan_clusters)。
{: tip}

是否之前已创建集群，现在只需要了解快速示例命令？请尝试以下示例。
*  **免费集群**：
   ```
        ibmcloud ks cluster-create --name my_cluster
        ```
   {: pre}
*  **标准集群（共享虚拟机）**：
   ```
   ibmcloud ks cluster-create --name my_cluster --zone dal10 --machine-type b3c.4x16 --hardware shared --workers 3 --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID>
   ```
   {: pre}
*  **标准集群（裸机）**：
   ```
   ibmcloud ks cluster-create --name my_cluster --zone dal10 --machine-type mb2c.4x32 --hardware dedicated --workers 3 --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID>
   ```
   {: pre}
*  **标准集群（在启用 VRF 的帐户中使用公共和专用服务端点的虚拟机）**：
   ```
   ibmcloud ks cluster-create --name my_cluster --zone dal10 --machine-type b3c.4x16 --hardware shared --workers 3 --public-service-endpoint --private-service-endpoint --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID>
   ```
   {: pre}
*  **标准集群（仅使用专用 VLAN 和专用服务端点）**：
   ```
    ibmcloud ks cluster-create --name my_cluster --zone dal10 --machine-type b3c.4x16 --hardware shared --workers 3 --private-service-endpoint --private-vlan <private_VLAN_ID> --private-only
    ```
   {: pre}

<br />


## 准备在帐户级别创建集群
{: #cluster_prepare}

准备 {{site.data.keyword.Bluemix_notm}} 帐户以使用 {{site.data.keyword.containerlong_notm}}。这些准备工作由帐户管理员完成，完成后，您可能无需在每次创建集群时对其进行更改。但是，每次创建集群时，您仍希望验证当前帐户级别状态是否为所需的状态。
{: shortdesc}

1. [创建计费帐户或将帐户升级到计费帐户（{{site.data.keyword.Bluemix_notm}} 现收现付或预订帐户）](https://cloud.ibm.com/registration/)。

2. [在要创建集群的区域中设置 {{site.data.keyword.containerlong_notm}} API 密钥](/docs/containers?topic=containers-users#api_key)。为 API 密钥分配可创建集群的相应许可权：
  * IBM Cloud Infrastructure (SoftLayer) 的**超级用户**角色。
  * 帐户级别的 {{site.data.keyword.containerlong_notm}} 的**管理员**平台管理角色。
  * 帐户级别的 {{site.data.keyword.registrylong_notm}} 的**管理员**平台管理角色。如果您的帐户是在 2018 年 10 月 4 日之前注册的，那么您需要[为 {{site.data.keyword.registryshort_notm}} 启用 {{site.data.keyword.Bluemix_notm}} IAM 策略](/docs/services/Registry?topic=registry-user#existing_users)。通过 IAM 策略，您可以控制对资源（例如，注册表名称空间）的访问。

  您是帐户所有者吗？如果是，那么您已具有必需的许可权！创建集群时，该区域和资源组的 API 密钥将使用您的凭证进行设置。
    {: tip}

3. 验证您是否具有 {{site.data.keyword.containerlong_notm}} 的**管理员**平台角色。要允许集群从专用注册表中拉取映像，您还需要 {{site.data.keyword.registrylong_notm}} 的**管理员**平台角色。
  1. 在 [{{site.data.keyword.Bluemix_notm}} 控制台 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/) 菜单栏中，选择**管理 > 访问权 (IAM)**。
  2. 单击**用户**页面，然后从表中选择您自己。
  3. 在**访问策略**选项卡中，确认您的**角色**为**管理员**。您可以是帐户中所有资源的**管理员**，也可以至少是 {{site.data.keyword.containershort_notm}} 的管理员。**注**：如果您仅具有一个资源组或区域（而不是整个帐户）中 {{site.data.keyword.containershort_notm}} 的**管理员**角色，那么您必须至少在帐户级别具有**查看者**角色才能查看帐户的 VLAN。
  <p class="tip">确保帐户管理员未在向您分配**管理员**平台角色的同时分配服务角色。您必须单独分配平台角色和服务角色。</p>

4. 如果帐户使用了多个资源组，请确定[管理资源组](/docs/containers?topic=containers-users#resource_groups)的帐户策略。
  * 在您登录到 {{site.data.keyword.Bluemix_notm}} 后，将在您设定为目标的资源组中创建集群。如果未将某个资源组设定为目标，那么会自动将缺省资源组设定为目标。
  * 如果要在非缺省资源组中创建集群，那么您至少需要该资源组的**查看者**角色。如果您不具有该资源组的任何角色，但仍然是该资源组中服务的**管理员**，那么将在缺省资源组中创建集群。
  * 无法更改集群的资源组。此外，如果您需要使用 `ibmcloud ks cluster-service-bind` [命令](/docs/containers-cli-plugin?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_service_bind)[与 {{site.data.keyword.Bluemix_notm}} 服务集成](/docs/containers?topic=containers-service-binding#bind-services)，那么该服务必须与集群位于同一资源组中。对于不使用资源组的服务（如 {{site.data.keyword.registrylong_notm}}）或不需要服务绑定的服务（如 {{site.data.keyword.la_full_notm}}），即使集群位于其他资源组中，这些服务也可正常工作。
  * 如果计划将 [{{site.data.keyword.monitoringlong_notm}} 用于度量值](/docs/containers?topic=containers-health#view_metrics)，请计划为集群提供在您帐户中的所有资源组和区域之间唯一的名称，以避免发生度量值命名冲突。
  * 免费集群会在 `default` 资源组中创建。

5. **标准集群**：规划集群[网络设置](/docs/containers?topic=containers-plan_clusters)，以便集群满足工作负载和环境的需求。然后，设置 IBM Cloud Infrastructure (SoftLayer) 联网，以允许工作程序与主节点的通信以及用户与主节点的通信：
  * 仅使用专用服务端点或使用公共和专用服务端点（运行面向因特网的工作负载或扩展内部部署数据中心）：
    1. 在您的 IBM Cloud Infrastructure (SoftLayer) 帐户中启用 [VRF](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud)。
    2. [启用 {{site.data.keyword.Bluemix_notm}} 帐户以使用服务端点](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started)。<p class="note">如果授权集群用户位于 {{site.data.keyword.Bluemix_notm}} 专用网络中，或者通过 [VPN 连接](/docs/infrastructure/iaas-vpn?topic=VPN-gettingstarted-with-virtual-private-networking)或 [{{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link) 与专用网络连接，那么 Kubernetes 主节点可通过专用服务端点进行访问。但是，与 Kubernetes 主节点通过专用服务端点进行的通信必须经过 <code>166.X.X.X</code> IP 地址范围，这不能通过 VPN 连接或 {{site.data.keyword.Bluemix_notm}} Direct Link 进行路由。可以通过使用专用网络负载均衡器 (NLB) 来为集群用户公开主节点的专用服务端点。专用 NLB 将主节点的专用服务端点作为内部 <code>10.X.X.X</code> IP 地址范围公开，用户可以使用 VPN 或 {{site.data.keyword.Bluemix_notm}} Direct Link 连接对这些地址进行访问。如果仅启用专用服务端点，那么可以使用 Kubernetes 仪表板或临时启用公共服务端点来创建专用 NLB。有关更多信息，请参阅[通过专用服务端点访问集群](/docs/containers?topic=containers-clusters#access_on_prem)。</p>

  * 仅使用公共服务端点（运行面向因特网的工作负载）：
    1. 为 IBM Cloud Infrastructure (SoftLayer) 帐户启用 [VLAN 生成](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning)，以便工作程序节点可以在专用网络上彼此通信。要执行此操作，您需要有**网络 > 管理网络 VLAN 生成**[基础架构许可权](/docs/containers?topic=containers-users#infra_access)，也可以请求帐户所有者来启用 VLAN 生成。要检查是否已启用 VLAN 生成，请使用 `ibmcloud ks vlan-spanning-get --region <region>` [命令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get)。
  * 使用网关设备（扩展内部部署数据中心）：
    1. 为 IBM Cloud Infrastructure (SoftLayer) 帐户启用 [VLAN 生成](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning)，以便工作程序节点可以在专用网络上彼此通信。要执行此操作，您需要有**网络 > 管理网络 VLAN 生成**[基础架构许可权](/docs/containers?topic=containers-users#infra_access)，也可以请求帐户所有者来启用 VLAN 生成。要检查是否已启用 VLAN 生成，请使用 `ibmcloud ks vlan-spanning-get --region <region>` [命令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get)。
    2. 配置网关设备。例如，可以选择设置[虚拟路由器设备](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-about-the-vra)或 [Fortigate Security Appliance](/docs/services/vmwaresolutions/services?topic=vmware-solutions-fsa_considerations)，以充当防火墙来允许必需的流量并阻止不需要的流量。
    3. 为每个区域（以便主节点和工作程序节点可以通信）以及为您计划使用的 {{site.data.keyword.Bluemix_notm}} 服务[打开必需的专用 IP 地址和端口](/docs/containers?topic=containers-firewall#firewall_outbound)。

<br />


## 准备在集群级别创建集群
{: #prepare_cluster_level}

设置帐户以创建集群后，请准备设置集群。这些是每次创建集群时会影响集群的准备工作。
{: shortdesc}

1. 决定是[免费还是标准集群](/docs/containers?topic=containers-cs_ov#cluster_types)。您可以创建 1 个免费集群来试用部分功能 30 天，或者创建具有所选硬件隔离的可完全定制的标准集群。创建标准集群可获取更多收益并控制集群性能。

2. 对于标准集群，请规划集群设置。
  * 确定是创建[单专区](/docs/containers?topic=containers-ha_clusters#single_zone)还是[多专区](/docs/containers?topic=containers-ha_clusters#multizone)集群。请注意，多专区集群仅在精选位置可用。
  * 选择要用于集群的工作程序节点的[硬件和隔离](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)类型，包括决定是使用虚拟机还是裸机机器。

3. 对于标准集群，可以在 {{site.data.keyword.Bluemix_notm}} 控制台中[估算成本](/docs/billing-usage?topic=billing-usage-cost#cost)。有关估算工具中可能不包含的费用的更多信息，请参阅[定价和计费](/docs/containers?topic=containers-faqs#charges)。

4. 如果是在防火墙后的环境中创建集群，例如用于扩展内部部署数据中心的集群，请针对计划使用的 {{site.data.keyword.Bluemix_notm}} 服务，[允许流至公共和专用 IP 的出站网络流量](/docs/containers?topic=containers-firewall#firewall_outbound)。

<br />


## 创建免费集群
{: #clusters_free}

可以使用 1 个免费集群来熟悉 {{site.data.keyword.containerlong_notm}} 的工作方式。通过免费集群，您可以了解术语，完成教程，弄清状况，然后再跃升到生产级别的标准集群。别担心，就算您拥有的是计费帐户，也仍会获得免费集群。
{: shortdesc}

免费集群包含设置有 2 个 vCPU 和 4 GB 内存的一个工作程序节点，免费集群的生命周期为 30 天。在此时间之后，免费集群将到期，并且会删除该集群及其数据。{{site.data.keyword.Bluemix_notm}} 不会备份删除的数据，因此无法复原这些数据。请确保备份任何重要数据。
{: note}

### 在控制台中创建免费集群
{: #clusters_ui_free}

1. 在[目录 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/catalog?category=containers) 中，选择 **{{site.data.keyword.containershort_notm}}** 以创建集群。
2. 选择**免费**集群套餐。
3. 选择要在其中部署集群的地理位置。
4. 在地理位置中选择大城市位置。集群将在此大城市内的专区中进行创建。
5. 为集群提供名称。名称必须以字母开头，可以包含字母、数字和连字符 (-)，并且不能超过 35 个字符。请使用在各区域中唯一的名称。
6. 单击**创建集群**。缺省情况下，将创建包含一个工作程序节点的工作程序池。您可以在**工作程序节点**选项卡中查看工作程序节点部署的进度。完成部署后，您可以在**概述**选项卡中看到集群已就绪。请注意，即使集群已准备就绪，集群中由其他服务使用的某些部分（例如，注册表映像拉取私钥）可能仍在进行中。

  系统会为每个工作程序节点分配唯一的工作程序节点标识和域名，在创建集群后，不得手动更改该标识和域名。更改标识或域名会阻止 Kubernetes 主节点管理集群。
    {: important}
7. 创建集群后，可以[通过配置 CLI 会话开始使用集群](#access_cluster)。

### 在 CLI 中创建免费集群
{: #clusters_cli_free}

开始之前，请安装 {{site.data.keyword.Bluemix_notm}} CLI 和 [{{site.data.keyword.containerlong_notm}} 插件](/docs/containers?topic=containers-cs_cli_install#cs_cli_install)。

1. 登录到 {{site.data.keyword.Bluemix_notm}} CLI。
  1. 登录并根据提示输入 {{site.data.keyword.Bluemix_notm}} 凭证。
     ```
     ibmcloud login
     ```
     {: pre}

     如果您有联合标识，请使用 `ibmcloud login --sso` 登录到 {{site.data.keyword.Bluemix_notm}} CLI。输入您的用户名，并使用 CLI 输出中提供的 URL 来检索一次性密码。如果不使用 `--sso` 时登录失败，而使用 `--sso` 选项时登录成功，说明您拥有的是联合标识。
        {: tip}

  2. 如果有多个 {{site.data.keyword.Bluemix_notm}} 帐户，请选择要在其中创建 Kubernetes 集群的帐户。

  3. 要在特定区域中创建免费集群，必须将该区域设定为目标。可以在 `ap-south`、`eu-central`、`uk-south` 或 `us-south` 中创建免费集群。集群将在该区域内的专区中进行创建。
     ```
ibmcloud ks region-set
```
     {: pre}

2. 创建集群。
  ```
  ibmcloud ks cluster-create --name my_cluster
  ```
  {: pre}

3. 验证是否请求了创建集群。订购工作程序节点机器可能需要几分钟时间。
    ```
    ibmcloud ks clusters
    ```
    {: pre}

    当完成集群供应时，集群的状态会更改为**已部署**。
    ```
    Name         ID                                   State      Created          Workers   Zone       Version     Resource Group Name
    my_cluster   paf97e8843e29941b49c598f516de72101   deployed   20170201162433   1         mil01      1.13.6      Default
    ```
    {: screen}

4. 检查工作程序节点的状态。
    ```
   ibmcloud ks workers --cluster <cluster_name_or_ID>
   ```
    {: pre}

    当工作程序节点已准备就绪时，状态会更改为 **normal**，而阶段状态为 **Ready**。节点阶段状态为 **Ready** 时，可以访问集群。请注意，即使集群已准备就绪，集群中由其他服务使用的某些部分（例如，Ingress 私钥或注册表映像拉取私钥）可能仍在进行中。
    ```
    ID                                                 Public IP       Private IP      Machine Type   State    Status   Zone        Version     Resource Group Name
    kube-mil01-paf97e8843e29941b49c598f516de72101-w1   169.xx.xxx.xxx  10.xxx.xx.xxx   free           normal   Ready    mil01       1.13.6      Default
    ```
    {: screen}

    系统会为每个工作程序节点分配唯一的工作程序节点标识和域名，在创建集群后，不得手动更改该标识和域名。更改标识或域名会阻止 Kubernetes 主节点管理集群。
    {: important}

5. 创建集群后，可以[通过配置 CLI 会话开始使用集群](#access_cluster)。

<br />


## 创建标准集群
{: #clusters_standard}

使用 {{site.data.keyword.Bluemix_notm}} CLI 或 {{site.data.keyword.Bluemix_notm}} 控制台来创建完全可定制的标准集群，您可以选择硬件隔离并且有权访问多种功能，如用于高可用性环境的多个工作程序节点。
{: shortdesc}

### 在控制台中创建标准集群
{: #clusters_ui}

1. 在[目录 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/catalog?category=containers) 中，选择 **{{site.data.keyword.containershort_notm}}** 以创建集群。

2. 选择要在其中创建集群的资源组。
  **注**：
    * 只能在一个资源组中创建集群，在创建集群后，即无法更改其资源组。
    * 要在非缺省资源组中创建集群，您必须至少具有该资源组的[**查看者**角色](/docs/containers?topic=containers-users#platform)。

3. 选择要在其中部署集群的地理位置。

4. 为集群提供唯一名称。名称必须以字母开头，可以包含字母、数字和连字符 (-)，并且不能超过 35 个字符。请使用在各区域中唯一的名称。集群名称和部署集群的区域构成了 Ingress 子域的标准域名。为了确保 Ingress 子域在区域内是唯一的，可能会截断 Ingress 域名中的集群名称并附加随机值。
**注**：更改创建期间分配的唯一标识或域名，会导致 Kubernetes 主节点无法管理集群。

5. 选择**单专区**或**多专区**可用性。在多专区集群中，主节点部署在支持多专区的专区中，并且集群的资源会跨多个专区进行分布。

6. 输入大城市和专区详细信息。
  * 多专区集群：
    1. 选择大城市位置。要获得最佳性能，请选择实际离您最近的大城市位置。根据地理位置的不同，您的选择可能会受到限制。
    2. 选择要在其中托管集群的特定专区。必须选择至少一个专区，但您可以选择任意多数量的专区。如果选择多个专区，那么工作程序节点会跨您选择的专区分布，从而为您提供更高的可用性。如果仅选择 1 个专区，那么在创建该专区后，可以[向集群添加专区](/docs/containers?topic=containers-add_workers#add_zone)。
  * 单专区集群：选择要在其中托管集群的专区。要获得最佳性能，请选择实际离您最近的专区。根据地理位置的不同，您的选择可能会受到限制。

7. 为每个专区选择 VLAN。
  * 创建可以在其中运行面向因特网的工作负载的集群：
    1. 从 IBM Cloud Infrastructure (SoftLayer) 帐户中为每个专区选择公用 VLAN 和专用 VLAN。工作程序节点使用专用 VLAN 相互通信，并可以使用公用或专用 VLAN 与 Kubernetes 主节点进行通信。如果在此专区中没有公用或专用 VLAN，那么会自动创建公用和专用 VLAN。可以对多个集群使用相同的 VLAN。

  * 创建用于仅在专用网络上扩展内部部署数据中心的集群，用于通过日后添加受限公共访问权的选项扩展内部部署数据中心的集群，或者用于通过网关设备扩展内部部署数据中心并提供受限公共访问权的集群：
    1. 从 IBM Cloud Infrastructure (SoftLayer) 帐户中为每个专区选择专用 VLAN。工作程序节点使用专用 VLAN 相互通信。如果在某个专区中没有专用 VLAN，那么会自动创建专用 VLAN。可以对多个集群使用相同的 VLAN。

    2. 对于公用 VLAN，选择**无**。

8. 对于**主节点服务端点**，选择 Kubernetes 主节点和工作程序节点的通信方式。
  * 创建可以在其中运行面向因特网的工作负载的集群：
    * 如果在 {{site.data.keyword.Bluemix_notm}} 帐户中启用了 VRF 和服务端点，请选择**专用和公共端点**。
    * 如果无法或不想启用 VRF，请选择**仅公共端点**。
  * 要创建用于仅扩展内部部署数据中心的集群，或用于通过边缘工作程序节点扩展内部部署数据中心并提供受限公共访问权的集群，请选择**专用和公共端点**或**仅专用端点**。确保已在 {{site.data.keyword.Bluemix_notm}} 帐户中启用 VRF 和服务端点。请注意，如果仅启用专用服务端点，那么必须[通过专用网络负载均衡器公开主节点端点](#access_on_prem)，以便用户可以通过 VPN 或 {{site.data.keyword.BluDirectLink}} 连接访问主节点。
  * 要创建用于通过网关设备扩展内部部署数据中心并提供受限公共访问权的集群，请选择**仅公共端点**。

9. 配置缺省工作程序池。工作程序池是共享相同配置的成组的工作程序节点。日后，始终可以向集群添加更多工作程序池。
  1. 为集群主节点和工作程序节点选择 Kubernetes API 服务器版本。
  2. 通过选择机器类型来过滤工作程序类型模板。“虚拟”按小时计费，“裸机”按月计费。
    - **裸机**：裸机服务器按月计费，订购后由 IBM Cloud Infrastructure (SoftLayer) 手动供应，可能需要一个工作日以上的时间才能完成。裸机最适用于需要更多资源和主机控制的高性能应用程序。还可以选择启用[可信计算](/docs/containers?topic=containers-security#trusted_compute)来验证工作程序节点是否被篡改。可信计算可用于精选的裸机机器类型。例如，`mgXc` GPU 类型模板不支持可信计算。如果在创建集群期间未启用信任，但希望日后启用，那么可以使用 `ibmcloud ks feature-enable` [命令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable)。启用信任后，日后无法将其禁用。确保要供应裸机机器。因为裸机机器是按月计费的，所以如果在错误下单后立即将其取消，也仍然会按整月向您收费。
        {:tip}
    - **虚拟 - 共享**：基础架构资源（例如，系统管理程序和物理硬件）在您与其他 IBM 客户之间共享，但每个工作程序节点只能由您访问。虽然此选项更便宜，并且足以满足大多数情况，但您可能希望使用公司策略来验证性能和基础架构需求。
    - **虚拟 - 专用**：工作程序节点在帐户专用的基础架构上托管。物理资源完全隔离。
  3. 选择类型模板。类型模板用于定义在每个工作程序节点中设置并可供容器使用的虚拟 CPU 量、内存量和磁盘空间量。可用的裸机和虚拟机类型随部署集群的专区而变化。创建集群后，可以通过将工作程序或池添加到集群来添加不同的机器类型。
  4. 指定集群中需要的工作程序节点数。输入的工作程序数将在所选数量的专区之间进行复制。这意味着如果您有 2 个专区并选择了 3 个工作程序节点，那么会供应 6 个节点，并且每个专区中存在 3 个节点。

10. 单击**创建集群**。这将创建具有指定工作程序数的工作程序池。您可以在**工作程序节点**选项卡中查看工作程序节点部署的进度。完成部署后，您可以在**概述**选项卡中看到集群已就绪。请注意，即使集群已准备就绪，集群中由其他服务使用的某些部分（例如，Ingress 私钥或注册表映像拉取私钥）可能仍在进行中。

  系统会为每个工作程序节点分配唯一的工作程序节点标识和域名，在创建集群后，不得手动更改该标识和域名。更改标识或域名会阻止 Kubernetes 主节点管理集群。
    {: important}

11. 创建集群后，可以[通过配置 CLI 会话开始使用集群](#access_cluster)。

### 在 CLI 中创建标准集群
{: #clusters_cli_steps}

开始之前，请安装 {{site.data.keyword.Bluemix_notm}} CLI 和 [{{site.data.keyword.containerlong_notm}} 插件](/docs/containers?topic=containers-cs_cli_install#cs_cli_install)。

1. 登录到 {{site.data.keyword.Bluemix_notm}} CLI。
  1. 登录并根据提示输入 {{site.data.keyword.Bluemix_notm}} 凭证。
     ```
     ibmcloud login
     ```
     {: pre}

     如果您有联合标识，请使用 `ibmcloud login --sso` 登录到 {{site.data.keyword.Bluemix_notm}} CLI。输入您的用户名，并使用 CLI 输出中提供的 URL 来检索一次性密码。如果不使用 `--sso` 时登录失败，而使用 `--sso` 选项时登录成功，说明您拥有的是联合标识。
        {: tip}

  2. 如果有多个 {{site.data.keyword.Bluemix_notm}} 帐户，请选择要在其中创建 Kubernetes 集群的帐户。

  3. 要在非缺省资源组中创建集群，请将该资源组设定为目标。
      **注**：
      * 只能在一个资源组中创建集群，在创建集群后，即无法更改其资源组。
      * 您必须至少具有该资源组的[**查看者**角色](/docs/containers?topic=containers-users#platform)。

      ```
      ibmcloud target -g <resource_group_name>
      ```
      {: pre}

2. 查看可用的专区。在以下命令的输出中，专区的 **Location Type** 为 `dc`。要使集群跨多个专区，必须在[支持多专区的专区](/docs/containers?topic=containers-regions-and-zones#zones)中创建集群。支持多专区的专区在 **Multizone Metro** 列中具有大城市值。
    ```
    ibmcloud ks supported-locations
    ```
    {: pre}
    选择您所在国家或地区以外的专区时，请记住，您可能需要法律授权才能将数据实际存储在国外。
    {: note}

3. 查看该专区中可用的工作程序节点类型模板。工作程序类型模板指定可供每个工作程序节点使用的虚拟或物理计算主机。
  - **虚拟**：虚拟机按小时计费，在共享或专用硬件上供应。
  - **物理**：裸机服务器按月计费，订购后由 IBM Cloud Infrastructure (SoftLayer) 手动供应，可能需要一个工作日以上的时间才能完成。裸机最适用于需要更多资源和主机控制的高性能应用程序。
  - **具有可信计算的物理机器**：您还可以选择启用[可信计算](/docs/containers?topic=containers-security#trusted_compute)来验证裸机工作程序节点是否被篡改。可信计算可用于精选的裸机机器类型。例如，`mgXc` GPU 类型模板不支持可信计算。如果在创建集群期间未启用信任，但希望日后启用，那么可以使用 `ibmcloud ks feature-enable` [命令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable)。启用信任后，日后无法将其禁用。
  - **机器类型**：要确定需要部署的机器类型，请查看[可用工作程序节点硬件](/docs/containers?topic=containers-planning_worker_nodes#shared_dedicated_node)的核心、内存和存储器组合。创建集群后，可以通过[添加工作程序池](/docs/containers?topic=containers-add_workers#add_pool)来添加不同的物理或虚拟机类型。

     确保要供应裸机机器。因为裸机机器是按月计费的，所以如果在错误下单后立即将其取消，也仍然会按整月向您收费。
        {:tip}

  ```
   ibmcloud ks machine-types --zone <zone>
   ```
  {: pre}

4. 检查以确定 IBM Cloud Infrastructure (SoftLayer) 中是否已存在此帐户的专区的 VLAN。
  ```
        ibmcloud ks vlans --zone <zone>
        ```
  {: pre}

  输出示例：
  ```
        ID        Name                Number   Type      Router  
        1519999   vlan   1355     private   bcr02a.dal10  
        1519898   vlan   1357     private   bcr02a.dal10 
        1518787   vlan   1252     public   fcr02a.dal10 
        1518888   vlan   1254     public    fcr02a.dal10 
        ```
  {: screen}
  * 要创建可以在其中运行面向因特网的工作负载的集群，请检查以确定是否存在公用和专用 VLAN。如果公用和专用 VLAN 已经存在，请记下匹配的路由器。专用 VLAN 路由器始终以 <code>bcr</code>（后端路由器）开头，而公用 VLAN 路由器始终以 <code>fcr</code>（前端路由器）开头。创建集群并指定公用和专用 VLAN 时，在这些前缀之后的数字和字母组合必须匹配。在输出示例中，任一专用 VLAN 都可以与任一公用 VLAN 一起使用，因为路由器全都包含 `02a.dal10`。
  * 要创建用于仅在专用网络上扩展内部部署数据中心的集群，用于通过日后借助边缘工作程序节点添加受限公共访问权的选项扩展内部部署数据中心的集群，或者用于通过网关设备扩展内部部署数据中心并提供受限公共访问权的集群，请检查以确定是否存在专用 VLAN。如果您有专用 VLAN，请记下该标识。

5. 运行 `cluster-create` 命令。缺省情况下，会对工作程序节点磁盘进行 AES 256 位加密，并且会按使用小时数对集群进行计费。
  * 创建可以在其中运行面向因特网的工作负载的集群：
    ```
    ibmcloud ks cluster-create --zone <zone> --machine-type <machine_type> --hardware <shared_or_dedicated> --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID> --workers <number> --name <cluster_name> --kube-version <major.minor.patch> [--private-service-endpoint] [--public-service-endpoint] [--disable-disk-encrypt] [--trusted]
    ```
    {: pre}
  * 创建用于通过日后借助边缘工作程序节点添加受限公共访问权的选项，在专用网络上扩展内部部署数据中心的集群：
    ```
    ibmcloud ks cluster-create --zone <zone> --machine-type <machine_type> --hardware <shared_or_dedicated> --private-vlan <private_VLAN_ID> --private-only --workers <number> --name <cluster_name> --kube-version <major.minor.patch> --private-service-endpoint [--public-service-endpoint] [--disable-disk-encrypt] [--trusted]
    ```
    {: pre}
  * 创建用于通过网关设备扩展内部部署数据中心并提供受限公共访问权的集群：
    ```
    ibmcloud ks cluster-create --zone <zone> --machine-type <machine_type> --hardware <shared_or_dedicated> --private-vlan <private_VLAN_ID> --private-only --workers <number> --name <cluster_name> --kube-version <major.minor.patch> --public-service-endpoint [--disable-disk-encrypt] [--trusted]
    ```
    {: pre}

    <table>
    <caption>cluster-create 组成部分</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解此命令的组成部分</th>
    </thead>
    <tbody>
    <tr>
    <td><code>cluster-create</code></td>
    <td>此命令在 {{site.data.keyword.Bluemix_notm}} 组织中创建集群。</td>
    </tr>
    <tr>
    <td><code>--zone <em>&lt;zone&gt;</em></code></td>
    <td>指定要在其中创建在步骤 4 中所选集群的 {{site.data.keyword.Bluemix_notm}} 专区标识。</td>
    </tr>
    <tr>
    <td><code>--machine-type <em>&lt;machine_type&gt;</em></code></td>
    <td>指定在步骤 5 中选择的机器类型。</td>
    </tr>
    <tr>
    <td><code>--hardware <em>&lt;shared_or_dedicated&gt;</em></code></td>
    <td>指定工作程序节点的硬件隔离级别。如果希望可用的物理资源仅供您专用，请使用 dedicated，或者要允许物理资源与其他 IBM 客户共享，请使用 shared。缺省值为 shared。此值对于 VM 标准集群是可选的。对于裸机机器类型，请指定 `dedicated`。</td>
    </tr>
    <tr>
    <td><code>--public-vlan <em>&lt;public_vlan_id&gt;</em></code></td>
    <td>如果已经在 IBM Cloud Infrastructure (SoftLayer) 帐户中为该专区设置了公用 VLAN，请输入在步骤 4 中找到的该公用 VLAN 的标识。<p>专用 VLAN 路由器始终以 <code>bcr</code>（后端路由器）开头，而公用 VLAN 路由器始终以 <code>fcr</code>（前端路由器）开头。创建集群并指定公用和专用 VLAN 时，在这些前缀之后的数字和字母组合必须匹配。</p></td>
    </tr>
    <tr>
    <td><code>--private-vlan <em>&lt;private_vlan_id&gt;</em></code></td>
    <td>如果已经在 IBM Cloud Infrastructure (SoftLayer) 帐户中为该专区设置了专用 VLAN，请输入在步骤 4 中找到的该专用 VLAN 的标识。如果帐户中没有专用 VLAN，请不要指定此选项。{{site.data.keyword.containerlong_notm}} 会自动为您创建专用 VLAN。<p>专用 VLAN 路由器始终以 <code>bcr</code>（后端路由器）开头，而公用 VLAN 路由器始终以 <code>fcr</code>（前端路由器）开头。创建集群并指定公用和专用 VLAN 时，在这些前缀之后的数字和字母组合必须匹配。</p></td>
    </tr>
    <tr>
    <td><code>--private-only </code></td>
    <td>创建仅使用专用 VLAN 的集群。如果包含此选项，请不要包含 <code>--public-vlan</code> 选项。</td>
    </tr>
    <tr>
    <td><code>--name <em>&lt;name&gt;</em></code></td>
    <td>指定集群的名称。名称必须以字母开头，可以包含字母、数字和连字符 (-)，并且不能超过 35 个字符。请使用在各区域中唯一的名称。集群名称和部署集群的区域构成了 Ingress 子域的标准域名。为了确保 Ingress 子域在区域内是唯一的，可能会截断 Ingress 域名中的集群名称并附加随机值。
</td>
    </tr>
    <tr>
    <td><code>--workers <em>&lt;number&gt;</em></code></td>
    <td>指定要包含在集群中的工作程序节点数。如果未指定 <code>--workers</code> 选项，那么会创建 1 个工作程序节点。</td>
    </tr>
    <tr>
    <td><code>--kube-version <em>&lt;major.minor.patch&gt;</em></code></td>
    <td>集群主节点的 Kubernetes 版本。此值是可选的。未指定版本时，会使用受支持 Kubernetes 版本的缺省值来创建集群。要查看可用版本，请运行 <code>ibmcloud ks versions</code>。</td>
    </tr>
    <tr>
    <td><code>--private-service-endpoint</code></td>
    <td>**在[启用 VRF 的帐户](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started)中**：启用专用服务端点，以便 Kubernetes 主节点和工作程序节点可通过专用 VLAN 进行通信。此外，可以选择使用 `--public-service-endpoint` 标志来启用公共服务端点，以通过因特网访问集群。如果仅启用专用服务端点，那么必须连接到专用 VLAN 才能与 Kubernetes 主节点进行通信。启用专用服务端点后，日后无法将其禁用。<br><br>创建集群后，可以通过运行 `ibmcloud ks cluster-get --cluster <cluster_name_or_ID>` 来获取端点。</td>
    </tr>
    <tr>
    <td><code>--public-service-endpoint</code></td>
    <td>启用公共服务端点，以便可以通过公用网络访问 Kubernetes 主节点（例如，通过终端运行 `kubectl` 命令），以及使 Kubernetes 主节点和工作程序节点可以通过公用 VLAN 进行通信。如果日后希望使用仅专用集群，那么可以禁用公共服务端点。<br><br>创建集群后，可以通过运行 `ibmcloud ks cluster-get --cluster <cluster_name_or_ID>` 来获取端点。</td>
    </tr>
    <tr>
    <td><code>--disable-disk-encrypt</code></td>
    <td>缺省情况下，工作程序节点具有 AES 256 位[磁盘加密](/docs/containers?topic=containers-security#encrypted_disk)功能。如果要禁用加密，请包括此选项。</td>
    </tr>
    <tr>
    <td><code>--trusted</code></td>
    <td>**裸机集群**：启用[可信计算](/docs/containers?topic=containers-security#trusted_compute)以验证裸机工作程序节点是否被篡改。可信计算可用于精选的裸机机器类型。例如，`mgXc` GPU 类型模板不支持可信计算。如果在创建集群期间未启用信任，但希望日后启用，那么可以使用 `ibmcloud ks feature-enable` [命令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable)。启用信任后，日后无法将其禁用。</td>
    </tr>
    </tbody></table>

6. 验证是否请求了创建集群。对于虚拟机，可能需要几分钟时间，才能订购好工作程序节点机器并且在帐户中设置并供应集群。裸机物理机器通过与 IBM Cloud Infrastructure (SoftLayer) 进行手动交互来供应，可能需要一个工作日以上的时间才能完成。
    ```
    ibmcloud ks clusters
    ```
    {: pre}

    当完成集群供应时，集群的状态会更改为**已部署**。
    ```
    Name         ID                                   State      Created          Workers   Zone       Version     Resource Group Name
    my_cluster   paf97e8843e29941b49c598f516de72101   deployed   20170201162433   1         mil01      1.13.6      Default
    ```
    {: screen}

7. 检查工作程序节点的状态。
    ```
    ibmcloud ks workers --cluster <cluster_name_or_ID>
    ```
    {: pre}

    当工作程序节点已准备就绪时，状态会更改为 **normal**，而阶段状态为 **Ready**。节点阶段状态为 **Ready** 时，可以访问集群。请注意，即使集群已准备就绪，集群中由其他服务使用的某些部分（例如，Ingress 私钥或注册表映像拉取私钥）可能仍在进行中。请注意，如果创建了仅使用专用 VLAN 的集群，那么不会将任何**公共 IP** 地址分配给工作程序节点。

    ```
    ID                                                 Public IP       Private IP      Machine Type   State    Status   Zone        Version     Resource Group Name
    kube-mil01-paf97e8843e29941b49c598f516de72101-w1   169.xx.xxx.xxx  10.xxx.xx.xxx   standard       normal   Ready    mil01       1.13.6      Default
    ```
    {: screen}

    系统会为每个工作程序节点分配唯一的工作程序节点标识和域名，在创建集群后，不得手动更改该标识和域名。更改标识或域名会阻止 Kubernetes 主节点管理集群。
    {: important}

8. 创建集群后，可以[通过配置 CLI 会话开始使用集群](#access_cluster)。

<br />


## 访问集群
{: #access_cluster}

创建集群后，可以通过配置 CLI 会话开始使用集群。
{: shortdesc}

### 通过公共服务端点访问集群
{: #access_internet}

要使用集群，请将所创建的集群设置为 CLI 会话运行 `kubectl` 命令的上下文。
{: shortdesc}

1. 如果网络受到公司防火墙的保护，请允许访问 {{site.data.keyword.Bluemix_notm}} 和 {{site.data.keyword.containerlong_notm}} API 端点和端口。
  1. [在防火墙中允许访问 `ibmcloud` API 和 `ibmcloud ks` API 的公共端点](/docs/containers?topic=containers-firewall#firewall_bx)。
  2. [允许授权集群用户运行 `kubectl` 命令](/docs/containers?topic=containers-firewall#firewall_kubectl)，以通过仅公共、仅专用或公共和专用服务端点来访问主节点。
  3. [允许授权集群用户运行 `calicotl` 命令](/docs/containers?topic=containers-firewall#firewall_calicoctl)，以管理集群中的 Calico 网络策略。

2. 将所创建的集群设置为此会话的上下文。每次使用集群时都完成这些配置步骤。

  如果要改为使用 {{site.data.keyword.Bluemix_notm}} 控制台，那么可以在 [Kubernetes 终端](/docs/containers?topic=containers-cs_cli_install#cli_web)中直接通过 Web 浏览器来运行 CLI 命令。
  {: tip}
  1. 获取命令以设置环境变量并下载 Kubernetes 配置文件。
      ```
      ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
      ```
      {: pre}
配置文件下载完成后，会显示一个命令，您可以使用该命令将本地 Kubernetes 配置文件的路径设置为环境变量。

      OS X 的示例：
      ```
      export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/mycluster/kube-config-prod-dal10-mycluster.yml
      ```
      {: screen}
  2. 复制并粘贴终端中显示的命令，以设置 `KUBECONFIG` 环境变量。
  3. 验证是否已正确设置 `KUBECONFIG` 环境变量。OS X 的示例：
      ```
        echo $KUBECONFIG
        ```
      {: pre}

      输出：
      ```
              /Users/<user_name>/.bluemix/plugins/container-service/clusters/mycluster/kube-config-prod-dal10-mycluster.yml

        ```
      {: screen}

3. 使用缺省端口 `8001` 启动 Kubernetes 仪表板。
  1. 使用缺省端口号设置代理。
      ```
        kubectl proxy
        ```
      {: pre}

      ```
        Starting to serve on 127.0.0.1:8001
        ```
      {: screen}

  2.  在 Web 浏览器中打开以下 URL 以查看 Kubernetes 仪表板。
      ```
      http://localhost:8001/ui
      ```
      {: codeblock}

### 通过专用服务端点访问集群
{: #access_on_prem}

如果授权集群用户位于 {{site.data.keyword.Bluemix_notm}} 专用网络中，或者通过 [VPN 连接](/docs/infrastructure/iaas-vpn?topic=VPN-gettingstarted-with-virtual-private-networking)或 [{{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link) 与专用网络连接，那么 Kubernetes 主节点可通过专用服务端点进行访问。但是，与 Kubernetes 主节点通过专用服务端点进行的通信必须经过 <code>166.X.X.X</code> IP 地址范围，这不能通过 VPN 连接或 {{site.data.keyword.Bluemix_notm}} Direct Link 进行路由。可以通过使用专用网络负载均衡器 (NLB) 来为集群用户公开主节点的专用服务端点。专用 NLB 将主节点的专用服务端点作为内部 <code>10.X.X.X</code> IP 地址范围公开，用户可以使用 VPN 或 {{site.data.keyword.Bluemix_notm}} Direct Link 连接对这些地址进行访问。如果仅启用专用服务端点，那么可以使用 Kubernetes 仪表板或临时启用公共服务端点来创建专用 NLB。
{: shortdesc}

1. 如果网络受到公司防火墙的保护，请允许访问 {{site.data.keyword.Bluemix_notm}} 和 {{site.data.keyword.containerlong_notm}} API 端点和端口。
  1. [在防火墙中允许访问 `ibmcloud` API 和 `ibmcloud ks` API 的公共端点](/docs/containers?topic=containers-firewall#firewall_bx)。
  2. [允许授权集群用户运行 `kubectl` 命令](/docs/containers?topic=containers-firewall#firewall_kubectl)。请注意，在使用专用 NLB 将主节点的专用服务端点公开给集群之前，无法在步骤 6 中测试与集群的连接。

2. 获取集群的专用服务端点 URL 和端口。
  ```
   ibmcloud ks cluster-get --cluster <cluster_name_or_ID>
   ```
  {: pre}

  在此示例输出中，**Private Service Endpoint URL** 为 `https://c1.private.us-east.containers.cloud.ibm.com:25073`。
  ```
  Name:                           setest
  ID:                             b8dcc56743394fd19c9f3db7b990e5e3
  State:                          normal
  Created:                        2019-04-25T16:03:34+0000
  Location:                       wdc04
  Master URL:                     https://c1.private.us-east.containers.cloud.ibm.com:25073
  Public Service Endpoint URL:    -
  Private Service Endpoint URL:   https://c1.private.us-east.containers.cloud.ibm.com:25073
  Master Location:                Washington D.C.
  ...
  ```
  {: screen}

3. 创建名为 `kube-api-via-nlb.yaml` 的 YAML 文件。此 YAML 创建专用 `LoadBalancer` 服务，并通过该 NLB 来公开专用服务端点。将 `<private_service_endpoint_port>` 替换为在上一步中找到的端口。
  ```
  apiVersion: v1
  kind: Service
  metadata:
    name: kube-api-via-nlb
    annotations:
      service.kubernetes.io/ibm-load-balancer-cloud-provider-ip-type: private
    namespace: default
  spec:
    type: LoadBalancer
    ports:
    - protocol: TCP
      port: <private_service_endpoint_port>
      targetPort: <private_service_endpoint_port>
  ---
  kind: Endpoints
  apiVersion: v1
  metadata:
    name: kube-api-via-nlb
  subsets:
    - addresses:
        - ip: 172.20.0.1
      ports:
        - port: 2040
  ```
  {: codeblock}

4. 要创建专用 NLB，必须连接到集群主节点。由于尚无法通过专用服务端点经由 VPN 或 {{site.data.keyword.Bluemix_notm}} Direct Link 建立连接，因此必须使用公共服务端点或 Kubernetes 仪表板来连接到集群主节点并创建 NLB。
  * 如果仅启用专用服务端点，那么可以使用 Kubernetes 仪表板来创建 NLB。该仪表板会自动将所有请求路由到主节点的专用服务端点。
    1.  登录到 [{{site.data.keyword.Bluemix_notm}} 控制台](https://cloud.ibm.com/)。
    2.  在菜单栏中，选择要使用的帐户。
    3.  在菜单 ![“菜单”图标](../icons/icon_hamburger.svg "“菜单”图标") 中，单击 **Kubernetes**。
    4.  在**集群**页面上，单击要访问的集群。
    5.  在集群详细信息页面中，单击 **Kubernetes 仪表板**。
    6.  单击 **+ 创建**。
    7.  选择**通过文件创建**，上传 `kube-api-via-nlb.yaml` 文件，然后单击**上传**。
    8.  在**概述**页面中，验证 `kuba-api-via-nlb` 服务是否已创建。在**外部端点**列中，记下 `10.x.x.x` 地址。此 IP 地址会在 YAML 文件中指定的端口上公开 Kubernetes 主节点的专用服务端点。

  * 如果还启用了公共服务端点，那么您已有权访问主节点。
    1. 创建 NLB 和端点。
      ```
      kubectl apply -f kube-api-via-nlb.yaml
      ```
      {: pre}
    2. 验证 `kube-api-via-nlb` NLB 是否已创建。在输出中，记下 `10.x.x.x` **EXTERNAL-IP** 地址。此 IP 地址会在 YAML 文件中指定的端口上公开 Kubernetes 主节点的专用服务端点。
      ```
        kubectl get svc -o wide
        ```
      {: pre}

      在此示例输出中，Kubernetes 主节点的专用服务端点的 IP 地址为 `10.186.92.42`。
      ```
      NAME                     TYPE           CLUSTER-IP       EXTERNAL-IP      PORT(S)          AGE   SELECTOR
      kube-api-via-nlb         LoadBalancer   172.21.150.118   10.186.92.42     443:32235/TCP    10m   <none>
      ...
      ```
      {: screen}

  <p class="note">如果要使用 [strongSwan VPN 服务](/docs/containers?topic=containers-vpn#vpn-setup)连接到主节点，请改为记下 `172.21.x.x` **集群 IP** 以在下一步中使用。由于 strongSwan VPN pod 是在集群内部运行，因此它可以使用内部集群 IP 服务的 IP 地址来访问 NLB。在 strongSwan Helm chart 的 `config.yaml` 文件中，确保 Kubernetes 服务子网 CIDR `172.21.0.0/16` 列在 `local.subnet` 设置中。</p>

5. 在您或用户运行 `kubectl` 命令的客户机上，将 NLB IP 地址和专用服务端点 URL 添加到 `/etc/hosts` 文件。不要在 IP 地址和 URL 中包含任何端口，并且不要在 URL 中包含 `https://`。
  * 对于 OSX 和 Linux 用户：
    ```
    sudo nano /etc/hosts
    ```
    {: pre}
  * 对于 Windows 用户：
    ```
    notepad C:\Windows\System32\drivers\etc\hosts
    ```
    {: pre}

    根据本地计算机许可权，您可能需要以管理员身份运行记事本来编辑 hosts 文件。
    {: tip}

  要添加的示例文本：
  ```
  10.186.92.42      c1.private.us-east.containers.cloud.ibm.com
  ```
  {: codeblock}

6. 验证是否已通过 VPN 或 {{site.data.keyword.Bluemix_notm}} Direct Link 连接来连接到专用网络。

7. 通过检查 Kubernetes CLI 服务器版本，验证 `kubectl` 命令是否通过专用服务端点针对您的集群正常运行。
  ```
        kubectl version  --short
        ```
  {: pre}

  输出示例：
  ```
  Client Version: v1.13.6
  Server Version: v1.13.6
  ```
  {: screen}

<br />


## 后续步骤
{: #next_steps}

集群启动并开始运行后，可查看以下任务：
- 如果在支持多专区的专区中创建了集群，请[通过向集群添加专区来分布工作程序节点](/docs/containers?topic=containers-add_workers)。
- [在集群中部署应用程序。](/docs/containers?topic=containers-app#app_cli)
- [在 {{site.data.keyword.Bluemix_notm}} 中设置您自己的专用注册表，以存储 Docker 映像并与其他用户共享这些映像。](/docs/services/Registry?topic=registry-getting-started)
- [设置集群自动缩放器](/docs/containers?topic=containers-ca#ca)，以根据工作负载资源请求，自动在工作程序池中添加或除去工作程序节点。
- 使用 [pod 安全策略](/docs/containers?topic=containers-psp)来控制谁可以在集群中创建 pod。
- 启用 [Istio](/docs/containers?topic=containers-istio) 和 [Knative](/docs/containers?topic=containers-serverless-apps-knative) 受管附加组件以扩展集群功能。

然后，可以查看用于集群设置的以下网络配置步骤：

### 在集群中运行面向因特网的应用程序工作负载
{: #next_steps_internet}

* 将联网工作负载隔离到[边缘工作程序节点](/docs/containers?topic=containers-edge)。
* 使用[公用联网服务](/docs/containers?topic=containers-cs_network_planning#public_access)公开应用程序。
* 通过创建 [Calico DNAT 前策略](/docs/containers?topic=containers-network_policies#block_ingress)（例如，白名单和黑名单策略），控制流至用于公开应用程序的网络服务的公共流量。
* 通过设置 [strongSwan IPSec VPN 服务](/docs/containers?topic=containers-vpn)，将集群与 {{site.data.keyword.Bluemix_notm}} 帐户外部的专用网络中的服务相连接。

### 将内部部署数据中心扩展到集群，并允许通过边缘节点和 Calico 网络策略提供受限公共访问权
{: #next_steps_calico}

* 通过设置 [{{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link) 或 [strongSwan IPSec VPN 服务](/docs/containers?topic=containers-vpn#vpn-setup)，将集群与 {{site.data.keyword.Bluemix_notm}} 帐户外部的专用网络中的服务相连接。{{site.data.keyword.Bluemix_notm}} Direct Link 允许集群中的应用程序和服务通过专用网络与内部部署网络进行通信，而 strongSwan 允许通过加密的 VPN 隧道在公用网络上进行通信。
* 通过创建连接到公用和专用 VLAN 的工作程序节点的[边缘工作程序池](/docs/containers?topic=containers-edge)，隔离公共联网工作负载。
* 使用[专用联网服务](/docs/containers?topic=containers-cs_network_planning#private_access)公开应用程序。
* [创建 Calico 主机网络策略](/docs/containers?topic=containers-network_policies#isolate_workers)，以阻止对 pod 的公共访问，隔离专用网络上的集群，并允许访问其他 {{site.data.keyword.Bluemix_notm}} 服务。

### 将内部部署数据中心扩展到集群，并允许通过网关设备提供受限公共访问权
{: #next_steps_gateway}

* 如果您还为专用网络配置了网关防火墙，那么必须[允许工作程序节点之间的通信，并且使集群通过专用网络访问基础架构资源](/docs/containers?topic=containers-firewall#firewall_private)。
* 要将工作程序节点和应用程序安全地连接到 {{site.data.keyword.Bluemix_notm}} 帐户外部的专用网络，请在网关设备上设置 IPSec VPN 端点。然后，在集群中[配置 strongSwan IPSec VPN 服务](/docs/containers?topic=containers-vpn#vpn-setup)，以在网关上使用 VPN 端点，或者[直接使用 VRA 设置 VPN 连接](/docs/containers?topic=containers-vpn#vyatta)。
* 使用[专用联网服务](/docs/containers?topic=containers-cs_network_planning#private_access)公开应用程序。
* 在网关设备防火墙中[打开必需的端口和 IP 地址](/docs/containers?topic=containers-firewall#firewall_inbound)，以允许流至联网服务的入站流量。

### 将内部部署数据中心扩展到仅专用网络上的集群
{: #next_steps_extend}

* 如果您在专用网络上有一个防火墙，那么[允许工作程序节点之间的通信，并且使集群通过专用网络访问基础架构资源](/docs/containers?topic=containers-firewall#firewall_private)。
* 通过设置 [{{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link)，将集群与 {{site.data.keyword.Bluemix_notm}} 帐户外部的专用网络中的服务相连接。
* 使用[专用联网服务](/docs/containers?topic=containers-cs_network_planning#private_access)在专用网络上公开应用程序。
