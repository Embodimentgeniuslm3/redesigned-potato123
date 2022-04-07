---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: kubernetes, iks, oks, iro, openshift, red hat, red hat openshift, rhos

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

# 教程：创建 Red Hat OpenShift on IBM Cloud 集群 (beta)
{: #openshift_tutorial}

Red Hat OpenShift on IBM Cloud 作为 Beta 提供，用于测试 OpenShift 集群。在处于 Beta 阶段期间，并非 {{site.data.keyword.containerlong}} 的所有功能都可用。此外，在 Beta 阶段结束并且 Red Hat OpenShift on IBM Cloud 正式发布后，您创建的任何 OpenShift Beta 集群都只保留 30 天。
{: preview}

通过 **Red Hat OpenShift on IBM Cloud beta**，可以创建 {{site.data.keyword.containerlong_notm}} 集群，其中包含随 OpenShift 容器编排平台软件一起安装的工作程序节点。使用在 Red Hat Enterprise Linux 上运行的 [OpenShift 工具和目录 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://docs.openshift.com/container-platform/3.11/welcome/index.html) 来部署应用程序时，您将获得集群基础架构环境的所有[受管 {{site.data.keyword.containerlong_notm}} 的优点](/docs/containers?topic=containers-responsibilities_iks)。
{: shortdesc}

OpenShift 工作程序节点仅可用于标准集群。Red Hat OpenShift on IBM Cloud 仅支持 OpenShift V3.11，此版本包含 Kubernetes V1.11。
{: note}

## 目标
{: #openshift_objectives}

在本教程的各课程中，您将创建标准 Red Hat OpenShift on IBM Cloud 集群，打开 OpenShift 控制台，访问内置 OpenShift 组件，部署在 OpenShift 项目中使用 {{site.data.keyword.Bluemix_notm}} 服务的应用程序，以及在 OpenShift 路径上公开应用程序，以便外部用户可以访问该服务。
{: shortdesc}

此页面还包含有关 OpenShift 集群体系结构、Beta 限制以及如何提供反馈和获取支持的信息。

## 所需时间
{: #openshift_time}
45 分钟

## 受众
{: #openshift_audience}

本教程适用于希望了解如何首次创建 Red Hat OpenShift on IBM Cloud 集群的集群管理员。
{: shortdesc}

## 先决条件
{: #openshift_prereqs}

*   确保您具有以下 {{site.data.keyword.Bluemix_notm}} IAM 访问策略。
    *   对 {{site.data.keyword.containerlong_notm}} 的[**管理员**平台角色](/docs/containers?topic=containers-users#platform)
    *   对 {{site.data.keyword.containerlong_notm}} 的[**写入者**或**管理者**服务角色](/docs/containers?topic=containers-users#platform)
    *   对 {{site.data.keyword.registrylong_notm}} 的[**管理员**平台角色](/docs/containers?topic=containers-users#platform)
*    确保为 {{site.data.keyword.Bluemix_notm}} 区域和资源组的 [API 密钥](/docs/containers?topic=containers-users#api_key)设置了正确的基础架构许可权、**超级用户**或创建集群的[最低角色](/docs/containers?topic=containers-access_reference#infra)。
*   安装命令行工具。
    *   [安装 {{site.data.keyword.Bluemix_notm}} CLI (`ibmcloud`)、{{site.data.keyword.containershort_notm}} 插件 (`ibmcloud ks`) 和 {{site.data.keyword.registryshort_notm}} 插件 (`ibmcloud cr`)](/docs/containers?topic=containers-cs_cli_install#cs_cli_install_steps)。
    *   [安装 OpenShift Origin (`oc`) 和 Kubernetes (`kubectl`) CLI](/docs/containers?topic=containers-cs_cli_install#cli_oc)。

<br />


## 体系结构概述
{: #openshift_architecture}

下图和表描述了在 Red Hat OpenShift on IBM Cloud 体系结构中设置的缺省组件。
{: shortdesc}

![Red Hat OpenShift on IBM Cloud 集群体系结构](images/cs_org_ov_both_ses_rhos.png)

|主节点组件|描述|
|:-----------------|:-----------------|
|副本|主节点组件（包括 OpenShift Kubernetes API 服务器和 etcd 数据存储）有三个副本，如果位于多专区大城市中，那么这三个副本会跨专区分布，以实现更高可用性。主节点组件每 8 小时备份一次。|
|`rhos-api`|OpenShift Kubernetes API 服务器充当从工作程序节点到主节点的所有集群管理请求的主入口点。该 API 服务器会验证并处理会更改 Kubernetes 资源（例如，pod 或服务）的状态的请求，并将此状态存储在 etcd 数据存储中。|
|`openvpn-server`|OpenVPN 服务器与 OpenVPN 客户机配合使用，以安全地将主节点连接到工作程序节点。此连接支持对 pod 和服务的 `apiserver proxy` 调用，还支持对 kubelet 的 `kubectl exec`、`attach` 和 `logs` 调用。|
|`etcd`|etcd 是一种高可用性键值存储，用于存储集群的所有 Kubernetes 资源（例如，服务、部署和 pod）的状态。etcd 中的数据会备份到 IBM 管理的加密存储器实例中。|
|`rhos-controller`|OpenShift 控制器管理器监视新创建的 pod，并根据容量、性能需求、策略约束、反亲缘关系规范和工作负载需求来决定这些 pod 的部署位置。如果找不到与这些需求相匹配的工作程序节点，那么不会在集群中部署 pod。控制器还会监视集群资源（例如，副本集）的状态。当资源的状态更改时（例如，如果副本集内的 pod 停止运行），控制器管理器会开始更正操作以实现所需的状态。`rhos-controller` 在本机 Kubernetes 配置中充当调度程序和控制器管理器。|
|`cloud-controller-manager`|云控制器管理器管理特定于云提供者的组件，例如 {{site.data.keyword.Bluemix_notm}} 负载均衡器。|
{: caption="表 1. OpenShift 主节点组件。" caption-side="top"}
{: #rhos-components-1}
{: tab-title="Master"}
{: tab-group="rhos-components"}
{: class="simple-tab-table"}

|工作程序节点组件|描述|
|:-----------------|:-----------------|
|操作系统|Red Hat OpenShift on IBM Cloud 工作程序节点在 Red Hat Enterprise Linux 7 (RHEL 7) 操作系统上运行。|
|项目|OpenShift 会将资源组织成项目（项目是具有注释的 Kubernetes 名称空间），并且包含的组件比本机 Kubernetes 集群多得多，以用于运行 OpenShift 功能（如目录）。以下各行中描述了项目的精选组件。有关更多信息，请参阅[项目和用户 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://docs.openshift.com/container-platform/3.11/architecture/core_concepts/projects_and_users.html)。|
|`kube-system`|此名称空间包含用于在工作程序节点上运行 Kubernetes 的许多组件。<ul><li>**`ibm-master-proxy`**：`ibm-master-proxy` 是一个守护程序集，用于将请求从工作程序节点转发到高可用性主节点副本的 IP 地址。在单专区集群中，主节点有三个副本，每个副本位于单独的主机上。对于位于支持多专区的专区中的集群，主节点的三个副本在各专区中进行分布。高可用性负载均衡器会将向主节点域名发出的请求转发到主节点副本。</li><li>**`openvpn-client`**：OpenVPN 客户机与 OpenVPN 服务器配合使用，以安全地将主节点连接到工作程序节点。此连接支持对 pod 和服务的 `apiserver proxy` 调用，还支持对 kubelet 的 `kubectl exec`、`attach` 和 `logs` 调用。</li><li>**`kubelet`**：kubelet 是一个工作程序节点代理程序，在每个工作程序节点上运行，负责监视在工作程序节点上运行的 pod 的运行状况，以及监视 Kubernetes API 服务器发送的事件。根据事件，kubelet 会创建或除去 pod，确保活性和就绪性探测，并向 Kubernetes API 服务器报告 pod 的阶段状态。</li><li>**`calico`**：Calico 管理集群的网络策略，并包含多个组件，用于管理容器网络连接、IP 地址分配和网络流量控制。</li><li>**其他组件**：`kube-system` 名称空间还包含多个组件，用于管理 IBM 提供的资源，例如文件存储器和块存储器的存储器插件、Ingress 应用程序负载均衡器 (ALB)、`fluentd` 日志记录和 `keepalived`。</li></ul>|
|`ibm-system`|此名称空间包含 `ibm-cloud-provider-ip` 部署，此部署使用 `keepalived` 为向应用程序 pod 发出的请求提供运行状况检查和第 4 层负载均衡。|
|`kube-proxy-and-dns`|此名称空间包含多个组件，用于根据在工作程序节点上设置的 `iptables` 规则来验证入局网络流量，以及作为代理传递允许进入或离开集群的请求。|
|`default`|如果未指定名称空间或为 Kubernetes 资源创建项目，将使用此名称空间。此外，default 名称空间还包含以下组件，用于支持 OpenShift 集群。<ul><li>**`router`**：OpenShift 使用[路径 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://docs.openshift.com/container-platform/3.11/architecture/networking/routes.html) 在主机名上公开应用程序的服务，以便外部客户机可以访问该服务。router 会将服务映射到主机名。</li><li>**`docker-registry`** 和 **`registry-console`**：OpenShift 提供了内部[容器映像注册表 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://docs.openshift.com/container-platform/3.11/install_config/registry/index.html)，可用于通过控制台在本地管理和查看映像。或者，也可以设置专用 {{site.data.keyword.registrylong_notm}}。</li></ul>|
|其他项目|缺省情况下，其他组件安装在各种名称空间中，以启用日志记录、监视和 OpenShift 控制台等功能。<ul><li><code>ibm-cert-store</code></li><li><code>kube-public</code></li><li><code>kube-service-catalog</code></li><li><code>openshift</code></li><li><code>openshift-ansible-service-broker</code></li><li><code>openshift-console</code></li><li><code>openshift-infra</code></li><li><code>openshift-monitoring</code></li><li><code>openshift-node</code></li><li><code>openshift-template-service-broker</code></li><li><code>openshift-web-console</code></li></ul>|
{: caption="表 2. OpenShift 工作程序节点组件。" caption-side="top"}
{: #rhos-components-2}
{: tab-title="Worker nodes"}
{: tab-group="rhos-components"}
{: class="simple-tab-table"}

<br />


## 第 1 课：创建 Red Hat OpenShift on IBM Cloud 集群
{: #openshift_create_cluster}

可以使用[控制台](#openshift_create_cluster_console)或 [CLI](#openshift_create_cluster_cli) 在 {{site.data.keyword.containerlong_notm}} 中创建 Red Hat OpenShift on IBM Cloud 集群。要了解在创建集群时设置哪些组件，请参阅[体系结构概述](#openshift_architecture)。OpenShift 仅可用于标准集群。您可以在[常见问题](/docs/containers?topic=containers-faqs#charges)中了解有关标准集群价格的更多信息。
{:shortdesc}

只能在 **default** 资源组中创建集群。在 Beta 阶段结束并且 Red Hat OpenShift on IBM Cloud 正式发布后，您在 Beta 阶段期间创建的任何 OpenShift 集群都只保留 30 天。
{: important}

### 使用控制台创建集群
{: #openshift_create_cluster_console}

在 {{site.data.keyword.containerlong_notm}} 控制台中创建标准 OpenShift 集群。
{: shortdesc}

开始之前，请[完成先决条件](#openshift_prereqs)，以确保您具有创建集群的相应许可权。

1.  创建集群。
    1.  登录到 [{{site.data.keyword.Bluemix_notm}} 帐户 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/)。
    2.  从汉堡菜单 ![“汉堡菜单”图标](../icons/icon_hamburger.svg "“汉堡菜单”图标") 中，选择 **Kubernetes**，然后单击**创建集群**。
    3.  选择集群设置详细信息和名称。对于 Beta，OpenShift 集群仅作为位于华盛顿和伦敦数据中心的标准集群可用。
        *   对于**选择套餐**，选择**标准**。
        *   对于**资源组**，必须使用 **default**。
        *   对于**位置**，将地理位置设置为**北美**或**欧洲**，选择**单专区**或**多专区**可用性，然后选择 **华盛顿**或**伦敦**工作程序专区。
        *   对于**缺省工作程序池**，选择 **OpenShift** 集群版本。Red Hat OpenShift on IBM Cloud 仅支持 OpenShift V3.11，此版本包含 Kubernetes V1.11。为工作程序节点选择可用的类型模板，理想情况下工作程序节点至少有 4 个核心 16 GB RAM。
        *   设置每个专区要创建的工作程序节点数，例如 3。
    4.  最后，单击**创建集群**。<p class="note">创建集群可能需要一些时间才能完成。集群状态显示**正常**后，集群网络和负载均衡组件还需要大约 10 分钟时间来部署和更新用于 OpenShift Web 控制台和其他路径的集群域。等待集群就绪后再继续执行下一步，确定就绪的方法是检查 **Ingress Subdomain** 是否遵循 `<cluster_name>.<region>.containers.appdomain.cloud` 模式。</p>
2.  在集群详细信息页面中，单击 **OpenShift Web 控制台**。
3.  在 OpenShift 容器平台菜单栏的下拉菜单中，单击**应用程序控制台**。应用程序控制台将列出集群中的所有项目名称空间。您可以导航至某个名称空间来查看应用程序、构建和其他 Kubernetes 资源。
4.  要通过在终端中工作来完成下一课，请单击概要文件 **IAM#user.name@email.com > 复制登录命令**。将复制的 `oc` 登录命令粘贴到终端以通过 CLI 进行认证。

### 使用 CLI 创建集群
{: #openshift_create_cluster_cli}

使用 {{site.data.keyword.Bluemix_notm}} CLI 创建标准 OpenShift 集群。
{: shortdesc}

开始之前，请[完成先决条件](#openshift_prereqs)，以确保您具有创建集群的相应许可权，并且具有 `ibmcloud` CLI 和插件以及 `oc` 和 `kubectl` CLI。

1.  登录到为创建 OpenShift 集群而设置的帐户。将 **us-east** 或 **eu-gb** 区域以及 **default** 资源组设定为目标。如果您有联合帐户，请包括 `--sso` 标志。
    ```
    ibmcloud login -r (us-east|eu-gb) -g default [--sso]
    ```
    {: pre}
2.  创建集群。
    ```
    ibmcloud ks cluster-create --name <name> --location <zone> --kube-version <openshift_version> --machine-type <worker_node_flavor> --workers <number_of_worker_nodes_per_zone> --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID>
    ```
    {: pre}

    用于创建具有三个工作程序节点的集群的示例命令，这些节点位于华盛顿，具有四个核心和 16 GB 内存。

    ```
    ibmcloud ks cluster-create --name my_openshift --location wdc04 --kube-version 3.11_openshift --machine-type b3c.4x16.encrypted  --workers 3 --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID>
    ```
    {: pre}

    <table summary="表中第一行占两列。其余行都应从左到右阅读，其中第一列是命令组成部分，第二列是相应的描述。">
    <caption>cluster-create 组成部分</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解此命令的组成部分</th>
    </thead>
    <tbody>
    <tr>
    <td><code>cluster-create</code></td>
    <td>此命令在 {{site.data.keyword.Bluemix_notm}} 帐户中创建经典基础架构集群。</td>
    </tr>
    <tr>
    <td><code>--name <em>&lt;name&gt;</em></code></td>
    <td>输入集群的名称。名称必须以字母开头，可以包含字母、数字和连字符 (-)，并且不能超过 35 个字符。请使用在各 {{site.data.keyword.Bluemix_notm}} 区域中唯一的名称。</td>
    </tr>
    <tr>
    <td><code>--location <em>&lt;zone&gt;</em></code></td>
    <td>指定要在其中创建集群的专区。对于 Beta，可用专区为 `wdc04、wdc06、wdc07、lon04、lon05` 或 `lon06`。</p></td>
    </tr>
    <tr>
      <td><code>--kube-version <em>&lt;openshift_version&gt;</em></code></td>
      <td>必须选择支持的 OpenShift 版本。OpenShift 版本包含 Kubernetes 版本，此版本不同于本机 Kubernetes Ubuntu 集群上提供的 Kubernetes 版本。要列出可用的 OpenShift 版本，请运行 `ibmcloud ks versions`。要创建具有最新补丁版本的集群，可以仅指定主版本和次版本，例如 `3.11_openshift`。<br><br>Red Hat OpenShift on IBM Cloud 仅支持 OpenShift V3.11，此版本包含 Kubernetes V1.11。</td>
    </tr>
    <tr>
    <td><code>--machine-type <em>&lt;worker_node_flavor&gt;</em></code></td>
    <td>选择机器类型。可以将工作程序节点作为虚拟机部署在共享或专用硬件上，也可以作为物理机器部署在裸机上。可用的物理和虚拟机类型随集群的部署专区而变化。要列出可用的机器类型，请运行 `ibmcloud ks machine-types --zone <zone>`。</td>
    </tr>
    <tr>
    <td><code>--workers <em>&lt;number_of_worker_nodes_per_zone&gt;</em></code></td>
    <td>要包含在集群中的工作程序节点数。您可能需要至少指定三个工作程序节点，以便集群具有足够的资源来运行缺省组件并实现高可用性。如果未指定 <code>--workers</code> 选项，那么会创建一个工作程序节点。</td>
    </tr>
    <tr>
    <td><code>--public-vlan <em>&lt;public_vlan_id&gt;</em></code></td>
    <td>如果已经在 IBM Cloud Infrastructure (SoftLayer) 帐户中为该专区设置了公用 VLAN，请输入该公用 VLAN 的标识。要列出可用的 VLAN，请运行 `ibmcloud ks vlans --zone <zone>`。<br><br>如果帐户中没有公用 VLAN，请不要指定此选项。{{site.data.keyword.containerlong_notm}} 会自动为您创建公用 VLAN。</td>
    </tr>
    <tr>
    <td><code>--private-vlan <em>&lt;private_vlan_id&gt;</em></code></td>
    <td>如果已经在 IBM Cloud Infrastructure (SoftLayer) 帐户中为该专区设置了专用 VLAN，请输入该专用 VLAN 的标识。要列出可用的 VLAN，请运行 `ibmcloud ks vlans --zone <zone>`。<br><br>如果帐户中没有专用 VLAN，请不要指定此选项。{{site.data.keyword.containerlong_notm}} 会自动为您创建专用 VLAN。</td>
    </tr>
    </tbody></table>
3.  列出集群详细信息。查看集群 **State**，检查 **Ingress Subdomain**，并记下 **Master URL**。<p class="note">创建集群可能需要一些时间才能完成。集群状态显示**正常**后，集群网络和负载均衡组件还需要大约 10 分钟时间来部署和更新用于 OpenShift Web 控制台和其他路径的集群域。等待集群就绪后再继续执行下一步，确定就绪的方法是检查 **Ingress Subdomain** 是否遵循 `<cluster_name>.<region>.containers.appdomain.cloud` 模式。</p>
    ```
   ibmcloud ks cluster-get --cluster <cluster_name_or_ID>
   ```
    {: pre}
4.  下载配置文件以连接到集群。
    ```
    ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
    ```
    {: pre}

    配置文件下载完成后，会显示一个命令，您可以复制并粘贴该命令以将本地 Kubernetes 配置文件的路径设置为环境变量。

    OS X 的示例：

    ```
    export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-<datacenter>-<cluster_name>.yml
    ```
    {: screen}
5.  在浏览器中，导航至 **Master URL** 的地址并附加 `/console`。例如，`https://c0.containers.cloud.ibm.com:23652/console`。
6.  单击概要文件 **IAM#user.name@email.com > 复制登录命令**。将复制的 `oc` 登录命令粘贴到终端以通过 CLI 进行认证。<p class="tip">保存集群主节点 URL，以便日后访问 OpenShift 控制台。在未来的会话中，可以跳过 `cluster-config` 步骤，而改为通过控制台复制登录命令。</p>
7.  通过检查版本，验证 `oc` 命令是否针对您的集群正常运行。

    ```
    oc version
    ```
    {: pre}

    输出示例：

    ```
    oc v3.11.0+0cbc58b
    kubernetes v1.11.0+d4cacc0
    features: Basic-Auth

    Server https://c0.containers.cloud.ibm.com:23652
    openshift v3.11.98
    kubernetes v1.11.0+d4cacc0
    ```
    {: screen}

    如果无法执行需要管理员许可权的操作（例如，列出集群中的所有工作程序节点或 pod），请通过运行 `ibmcloud ks cluster-config --cluster <cluster_name_or_ID> --admin` 命令来下载集群管理员的 TLS 证书和许可权文件。
    {: tip}

<br />


## 第 2 课：访问内置 OpenShift 服务
{: #openshift_access_oc_services}

Red Hat OpenShift on IBM Cloud 随附可用于帮助操作集群的内置服务，例如 OpenShift 控制台、Prometheus 和 Grafana。对于 Beta，要访问这些服务，可以使用[路径 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://docs.openshift.com/container-platform/3.11/architecture/networking/routes.html) 的本地主机。缺省路径域名遵循特定于集群的模式 `<service_name>-<namespace>.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud`。
{:shortdesc}

可以通过[控制台](#openshift_services_console)或 [CLI](#openshift_services_cli) 来访问内置 OpenShift 服务路径。您可能希望使用控制台在一个项目中的 Kubernetes 资源中进行导航。通过使用 CLI，可以列出资源，例如跨项目的路径。

### 通过控制台访问内置 OpenShift 服务
{: #openshift_services_console}
1.  在 OpenShift Web 控制台的 OpenShift 容器平台菜单栏的下拉菜单中，单击**应用程序控制台**。
2.  选择 **default** 项目，然后在导航窗格中，单击**应用程序 > pod**。
3.  验证**路由器** pod 是否处于**正在运行**状态。路由器充当外部网络流量的入口点。可以使用路由器在路由器的外部 IP 地址上通过路径以公共方式公开集群中的服务。路由器会侦听公共主机网络接口，这与仅侦听专用 IP 的应用程序 pod 不同。路由器作为代理将路径主机名的外部请求传递到服务所标识的与路径主机名关联的应用程序 pod。
4.  在 **default** 项目导航窗格中，单击**应用程序 > 部署**，然后单击 **registry-console** 部署。要打开内部注册表控制台，必须更新提供程序 URL，以便可以在外部对其进行访问。
    1.  在 **registry-console** 详细信息页面的**环境**选项卡中，找到 **OPENSHIFT_OAUTH_PROVIDER_URL** 字段。 
    2. 在值字段中，在 `c1` 后添加 `-e`，例如 `https://c1-e.eu-gb.containers.cloud.ibm.com:20399`。 
    3. 单击**保存**。现在，可以通过集群主节点的公共 API 端点来访问注册表控制台部署。
    4.  在 **default** 项目导航窗格中，单击**应用程序 > 路径**。要打开注册表控制台，请单击**主机名**值，例如 `https://registry-console-default.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud`。
<p class="note">对于 Beta，注册表控制台使用自签名 TLS 证书，因此必须选择继续访问注册表控制台。在 Google Chrome 中，单击**高级 > 继续到 <cluster_master_URL>**。其他浏览器具有类似的选项。如果无法使用此设置继续，请尝试在专用浏览器中打开该 URL。</p>
5.  在 OpenShift 容器平台菜单栏的下拉菜单中，单击**集群控制台**。
6.  在导航窗格中，展开**监视**。
7.  单击要访问的内置监视工具，例如**仪表板**。这将打开 Grafana 路径 `https://grafana-openshift-monitoring.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud`。<p class="note">第一次访问主机名时，您可能需要进行认证，例如通过单击**登录到 OpenShift** 并授予对 IAM 身份的访问权。</p>

### 通过 CLI 访问内置 OpenShift 服务
{: #openshift_services_cli}

1.  在 OpenShift Web 控制台中，单击概要文件 **IAM#user.name@email.com > 复制登录命令**，然后将登录命令粘贴到终端以进行认证。
    ```
    oc login https://c1-e.<region>.containers.cloud.ibm.com:<port> --token=<access_token>
    ```
    {: pre}
2.  验证路由器是否已部署。路由器充当外部网络流量的入口点。可以使用路由器在路由器的外部 IP 地址上通过路径以公共方式公开集群中的服务。路由器会侦听公共主机网络接口，这与仅侦听专用 IP 的应用程序 pod 不同。路由器作为代理将路径主机名的外部请求传递到服务所标识的与路径主机名关联的应用程序 pod。
    ```
    oc get svc router -n default
    ```
    {: pre}

输出示例：
        ```
    NAME      TYPE           CLUSTER-IP               EXTERNAL-IP     PORT(S)                      AGE
    router    LoadBalancer   172.21.xxx.xxx   169.xx.xxx.xxx   80:30399/TCP,443:32651/TCP                      5h
    ```
    {: screen}
2.  获取要访问的服务路径的 **Host/Port** 主机名。例如，您可能希望访问 Grafana 仪表板，以查看集群资源使用情况的度量值。缺省路径域名遵循特定于集群的模式 `<service_name>-<namespace>.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud`。
    ```
    oc get route --all-namespaces
    ```
    {: pre}

输出示例：
        ```
    NAMESPACE                          NAME                HOST/PORT                                                                    PATH                  SERVICES            PORT               TERMINATION          WILDCARD
    default                            registry-console    registry-console-default.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud                              registry-console    registry-console   passthrough          None
    kube-service-catalog               apiserver           apiserver-kube-service-catalog.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud                        apiserver           secure             passthrough          None
    openshift-ansible-service-broker   asb-1338            asb-1338-openshift-ansible-service-broker.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud            asb                 1338               reencrypt            None
    openshift-console                  console             console.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud                                              console             https              reencrypt/Redirect   None
    openshift-monitoring               alertmanager-main   alertmanager-main-openshift-monitoring.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud                alertmanager-main   web                reencrypt            None
    openshift-monitoring               grafana             grafana-openshift-monitoring.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud                          grafana             https              reencrypt            None
    openshift-monitoring               prometheus-k8s      prometheus-k8s-openshift-monitoring.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud                   prometheus-k8s      web                reencrypt
    ```
    {: screen}
3.  **注册表一次性更新**：要使内部注册表控制台可供从因特网访问，请编辑 `registry-console` 部署以将集群主节点的公共 API 端点用作 OpenShift 提供程序 URL。公共 API 端点的格式与专用 API 端点的格式相同，但在 URL 中会包含额外的 `-e`。
    ```
    oc edit deploy registry-console -n default
    ```
    {: pre}
    
    在 `Pod Template.Containers.registry-console.Environment.OPENSHIFT_OAUTH_PROVIDER_URL` 字段中，在 `c1` 后添加 `-e`，例如 `https://ce.eu-gb.containers.cloud.ibm.com:20399`。
    ```
    Name:                   registry-console
    Namespace:              default
    ...
    Pod Template:
      Labels:  name=registry-console
      Containers:
       registry-console:
        Image:      registry.eu-gb.bluemix.net/armada-master/iksorigin-registrconsole:v3.11.98-6
        ...
        Environment:
          OPENSHIFT_OAUTH_PROVIDER_URL:  https://c1-e.eu-gb.containers.cloud.ibm.com:20399
          ...
    ```
    {: screen}
4.  在 Web 浏览器中，打开要访问的路径，例如：`https://grafana-openshift-monitoring.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud`。第一次访问主机名时，您可能需要进行认证，例如通过单击**登录到 OpenShift** 并授予对 IAM 身份的访问权。

<br>
现在，您已位于内置 OpenShift 应用程序中！例如，如果您位于 Grafana 中，那么可查看名称空间 CPU 使用率或其他图形。要访问其他内置工具，请打开其路径主机名。

<br />


## 第 3 课：将应用程序部署到 OpenShift 集群
{: #openshift_deploy_app}

通过 Red Hat OpenShift on IBM Cloud，可以创建新的应用程序，并可利用 OpenShift 路由器来公开应用程序服务以供外部用户使用。
{: shortdesc}

如果在学习了上一课后稍作休息，现在启动了新的终端，请确保您已重新登录到集群。打开 OpenShift 控制台：`https://<master_URL>/console`。例如，`https://c0.containers.cloud.ibm.com:23652/console`。接着，单击概要文件 **IAM#user.name@email.com > 复制登录命令**，然后将复制的 `oc` 登录命令粘贴到终端以通过 CLI 进行认证。
{: tip}

1.  为 Hello World 应用程序创建项目。项目是具有其他注释的 Kubernetes 名称空间的 OpenShift 版本。
    ```
    oc new-project hello-world
    ```
    {: pre}
2.  [通过源代码 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/IBM/container-service-getting-started-wt) 构建样本应用程序。使用 OpenShift `new-app` 命令，可以引用远程存储库中包含 Dockerfile 和应用程序代码的目录来构建映像。该命令将在本地 Docker 注册表中构建映像，并创建应用程序部署配置 (`dc`) 和服务 (`svc`)。有关创建新应用程序的更多信息，请参阅 [OpenShift 文档 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://docs.openshift.com/container-platform/3.11/dev_guide/application_lifecycle/new_app.html)。
    ```
    oc new-app --name hello-world https://github.com/IBM/container-service-getting-started-wt --context-dir="Lab 1"
    ```
    {: pre}
3.  验证是否已创建样本 Hello World 应用程序组件。
    1.  通过在浏览器中访问注册表控制台，检查集群的内置 Docker 注册表中是否有 **hello-world** 映像。确保使用 `-e` 来更新注册表控制台提供程序 URL，如上一课中所述。
        ```
        https://registry-console-default.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud/
        ```
        {: codeblock}
    2.  列出 **hello-world** 服务并记下服务名称。除非您为服务创建了路径，以便路由器可以将外部流量请求转发到应用程序，否则应用程序会侦听这些内部集群 IP 地址上的流量。
        ```
        oc get svc -n hello-world
        ```
        {: pre}

        输出示例：
        ```
        NAME          TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)    AGE
        hello-world   ClusterIP   172.21.xxx.xxx   <nones>       8080/TCP   31m
        ```
        {: screen}
    3.  列出 pod。名称中带有 `build` 的 pod 是在新应用程序构建过程中**已完成**的作业。确保 **hello-world** pod 的状态为 **Running**。
        ```
        oc get pods -n hello-world
        ```
        {: pre}

输出示例：
        ```
        NAME                READY     STATUS             RESTARTS   AGE
        hello-world-9cv7d   1/1       Running            0          30m
        hello-world-build   0/1       Completed          0          31m
        ```
        {: screen}
4.  设置路径，以便可以公共访问 {{site.data.keyword.toneanalyzershort}} 服务。缺省情况下，主机名的格式为 `<service_name>-<namespace>.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud`。如果要定制主机名，请包含 `--hostname=<hostname>` 标志。
    1.  为 **hello-world** 服务创建路径。
        ```
        oc create route edge --service=hello-world -n hello-world
        ```
        {: pre}
    2.  从 **Host/Port** 输出获取路径主机名地址。
        ```
        oc get route -n hello-world
        ```
        {: pre}
输出示例：
        ```
        NAME          HOST/PORT                         PATH                                        SERVICES      PORT       TERMINATION   WILDCARD
        hello-world   hello-world-hello.world.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud    hello-world   8080-tcp   edge/Allow    None
        ```
        {: screen}
5.  访问应用程序。
    ```
    curl https://hello-world-hello-world.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud
    ```
    {: pre}
    
输出示例：
        ```
    Hello world from hello-world-9cv7d! Your app is up and running in a cluster!
    ```
    {: screen}
6.  **可选**：要清除在本课程中创建的资源，可以使用分配给每个应用程序的标签。
    1.  列出 `hello-world` 项目中每个应用程序的所有资源。
        ```
        oc get all -l app=hello-world -o name -n hello-world
        ```
        {: pre}
输出示例：
        ```
        pod/hello-world-1-dh2ff
        replicationcontroller/hello-world-1
        service/hello-world
        deploymentconfig.apps.openshift.io/hello-world
        buildconfig.build.openshift.io/hello-world
        build.build.openshift.io/hello-world-1
        imagestream.image.openshift.io/hello-world
        imagestream.image.openshift.io/node
        route.route.openshift.io/hello-world
        ```
        {: screen}
    2.  删除创建的所有资源。
        ```
        oc delete all -l app=hello-world -n hello-world
        ```
        {: pre}



<br />


## 第 4 课：设置 LogDNA 和 Sysdig 附加组件以监视集群运行状况
{: #openshift_logdna_sysdig}

由于缺省情况下 OpenShift 设置的[安全上下文约束 (SCC) ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://docs.openshift.com/container-platform/3.11/admin_guide/manage_scc.html) 比本机 Kubernetes 更严格，因此您可能会发现在本机 Kubernetes 上使用的某些应用程序或集群附加组件无法以相同方式部署在 OpenShift 上。尤其是，许多映像需要以 `root` 用户身份运行或作为特权容器运行，而在 OpenShift 中缺省情况下会阻止这些这些行为。在本课程中，您将学习如何通过创建特权安全帐户，并更新 pod 规范中的 `securityContext` 来修改缺省 SCC，以使用两个常用的 {{site.data.keyword.containerlong_notm}} 附加组件：{{site.data.keyword.la_full_notm}} 和 {{site.data.keyword.mon_full_notm}}。
{: shortdesc}

开始之前，请以管理员身份登录到集群。
1.  打开 OpenShift 控制台：`https://<master_URL>/console`。例如，`https://c0.containers.cloud.ibm.com:23652/console`。
2.  单击概要文件 **IAM#user.name@email.com > 复制登录命令**，然后将复制的 `oc` 登录命令粘贴到终端以通过 CLI 进行认证。
3.  下载集群的管理配置文件。
    ```
    ibmcloud ks cluster-config --cluster <cluster_name_or_ID> --admin
    ```
    {: pre}
    
    配置文件下载完成后，会显示一个命令，您可以复制并粘贴该命令以将本地 Kubernetes 配置文件的路径设置为环境变量。

    OS X 的示例：

    ```
    export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-<datacenter>-<cluster_name>.yml
    ```
    {: screen}
4.  继续本课程以设置 [{{site.data.keyword.la_short}}](#openshift_logdna) 和 [{{site.data.keyword.mon_short}}](#openshift_sysdig)。

### 第 4a 课：设置 LogDNA
{: #openshift_logdna}

为 {{site.data.keyword.la_full_notm}} 设置项目和特权服务帐户。然后，在 {{site.data.keyword.Bluemix_notm}} 帐户中创建 {{site.data.keyword.la_short}} 实例。要将 {{site.data.keyword.la_short}} 实例与 OpenShift 集群集成，必须修改部署的守护程序集，以使用特权服务帐户以 root 用户身份运行。
{: shortdesc}

1.  为 LogDNA 设置项目和特权服务帐户。
    1.  以集群管理员身份，创建 `logdna` 项目。
        ```
        oc adm new-project logdna
        ```
        {: pre}
    2.  将该项目设定为目标，以便后续创建的资源位于 `logdna` 项目名称空间中。
        ```
        oc project logdna
        ```
        {: pre}
    3.  为 `logdna` 项目创建服务帐户。
        ```
        oc create serviceaccount logdna
        ```
        {: pre}
    4.  将特权安全上下文约束添加到 `logdna` 项目的服务帐户。<p class="note">如果要检查 `privileged` SCC 策略授予服务帐户的权限，请运行 `oc describe scc privileged`。有关 SCC 的更多信息，请参阅 [OpenShift 文档 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://docs.openshift.com/container-platform/3.11/admin_guide/manage_scc.html)。</p>
        ```
        oc adm policy add-scc-to-user privileged -n logdna -z logdna
        ```
        {: pre}
2.  在集群所在的资源组中创建 {{site.data.keyword.la_full_notm}} 实例。选择价格套餐，套餐用于确定日志的保留期，例如 `lite` 将日志保留 0 天。区域不必与集群的区域相匹配。有关更多信息，请参阅[供应实例](/docs/services/Log-Analysis-with-LogDNA/tutorials?topic=LogDNA-provision)和[价格套餐](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-about#overview_pricing_plans)。
    ```
    ibmcloud resource service-instance-create <service_instance_name> logdna (lite|7-days|14-days|30-days) <region> [-g <resource_group>]
    ```
    {: pre}
    
    示例命令：
    ```
    ibmcloud resource service-instance-create logdna-openshift logdna lite us-south
    ```
    {: pre}
    
    在输出中，记下服务实例 **ID**，其格式为 `crn:v1:bluemix:public:logdna:<region>:<ID_string>::`。
    ```
    Service instance <name> was created.
                 
    Name:         <name>   
    ID:           crn:v1:bluemix:public:logdna:<region>:<ID_string>::   
    GUID:         <guid>   
    Location:     <region>   
    ...
    ```
    {: screen}    
3.  获取 {{site.data.keyword.la_short}} 实例摄入密钥。LogDNA 摄入密钥用于将安全的 Web 套接字打开到 LogDNA 摄入服务器，并使用 {{site.data.keyword.la_short}} 服务来认证日志记录代理程序。
    1.  为 LogDNA 实例创建服务密钥。
        ```
        ibmcloud resource service-key-create <key_name> Administrator --instance-id <logdna_instance_ID>
        ```
        {: pre}
    2.  记下服务密钥的 **ingestion_key**。
        ```
        ibmcloud resource service-key <key_name>
        ```
        {: pre}
        
输出示例：
        ```
        Name:          <key_name>  
        ID:            crn:v1:bluemix:public:logdna:<region>:<ID_string>::    
        Created At:    Thu Jun  6 21:31:25 UTC 2019   
        State:         active   
        Credentials:                                   
                       apikey:                   <api_key_value>      
                       iam_apikey_description:   Auto-generated for key <ID>     
                       iam_apikey_name:          <key_name>       
                       iam_role_crn:             crn:v1:bluemix:public:iam::::role:Administrator      
                       iam_serviceid_crn:        crn:v1:bluemix:public:iam-identity::<ID_string>     
                       ingestion_key:            111a11aa1a1a11a1a11a1111aa1a1a11    
        ```
        {: screen}
4.  创建 Kubernetes 私钥以用于存储服务实例的 LogDNA 摄入密钥。
    ```
     oc create secret generic logdna-agent-key --from-literal=logdna-agent-key=<logDNA_ingestion_key>
    ```
    {: pre}
5.  创建 Kubernetes 守护程序集，以用于在 Kubernetes 集群的每个工作程序节点上部署 LogDNA 代理程序。LogDNA 代理程序会收集在 pod 的 `/var/log` 目录中存储的扩展名为 `*.log` 的日志以及无扩展名的文件。缺省情况下，将从所有名称空间（包括 `kube-system`）收集日志，并自动将日志转发到 {{site.data.keyword.la_short}} 服务。
    ```
    oc create -f https://assets.us-south.logging.cloud.ibm.com/clients/logdna-agent-ds.yaml
    ```
    {: pre}
6.  编辑 LogDNA 代理程序守护程序集配置，以引用先前创建的服务帐户，并将安全上下文设置为 privileged。
    ```
    oc edit ds logdna-agent
    ```
    {: pre}
    
    在配置文件中，添加以下规范。
    *   在 `spec.template.spec` 中，添加 `serviceAccount: logdna`。
    *   在 `spec.template.spec.containers` 中，添加 `securityContext: privileged: true`。
    *   如果在除 `us-south` 以外的区域中创建了 {{site.data.keyword.la_short}} 实例，请使用 `<region>` 更新 `LDAPIHOST` 和 `LDLOGHOST` 的 `spec.template.spec.containers.env` 环境变量值。

输出示例：
        ```
    apiVersion: extensions/v1beta1
    kind: DaemonSet
    spec:
      ...
      template:
...
        spec:
          serviceAccount: logdna
          containers:
          - securityContext:
              privileged: true
            ...
            env:
            - name: LOGDNA_AGENT_KEY
              valueFrom:
                secretKeyRef:
                  key: logdna-agent-key
                  name: logdna-agent-key
            - name: LDAPIHOST
              value: api.<region>.logging.cloud.ibm.com
            - name: LDLOGHOST
              value: logs.<region>.logging.cloud.ibm.com
          ...
    ```
    {: screen}
7.  验证每个节点上的 `logdna-agent` pod 是否处于 **Running** 状态。
    ```
    oc get pods
    ```
    {: pre}
8.  在 [{{site.data.keyword.Bluemix_notm}} 可观察性 > 日志记录控制台](https://cloud.ibm.com/observe/logging)的 {{site.data.keyword.la_short}} 实例所在行中，单击**查看 LogDNA**。这将打开 LogDNA 仪表板，在其中可以开始分析日志。

有关如何使用 {{site.data.keyword.la_short}} 的更多信息，请参阅[后续步骤文档](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-kube#kube_next_steps)。

### 第 4b 课：设置 Sysdig
{: #openshift_sysdig}

在 {{site.data.keyword.Bluemix_notm}} 帐户中创建 {{site.data.keyword.mon_full_notm}} 实例。要将 {{site.data.keyword.mon_short}} 实例与 OpenShift 集群集成，必须运行脚本来为 Sysdig 代理程序创建项目和特权服务帐户。
{: shortdesc}

1.  在集群所在的资源组中创建 {{site.data.keyword.mon_full_notm}} 实例。选择价格套餐，套餐用于确定日志的保留期，例如 `lite`。区域不必与集群的区域相匹配。有关更多信息，请参阅[供应实例](/docs/services/Monitoring-with-Sysdig?topic=Sysdig-provision)。
    ```
    ibmcloud resource service-instance-create <service_instance_name> sysdig-monitor (lite|graduated-tier) <region> [-g <resource_group>]
    ```
    {: pre}
    
    示例命令：
    ```
    ibmcloud resource service-instance-create sysdig-openshift sysdig-monitor lite us-south
    ```
    {: pre}
    
    在输出中，记下服务实例 **ID**，其格式为 `crn:v1:bluemix:public:logdna:<region>:<ID_string>::`。
    ```
    Service instance <name> was created.
                 
    Name:         <name>   
    ID:           crn:v1:bluemix:public:sysdig-monitor:<region>:<ID_string>::   
    GUID:         <guid>   
    Location:     <region>   
    ...
    ```
    {: screen}    
2.  获取 {{site.data.keyword.mon_short}} 实例访问密钥。Sysdig 访问密钥用于将安全的 Web 套接字打开到 Sysdig 摄入服务器，并使用 {{site.data.keyword.mon_short}} 服务来认证监视代理程序。
    1.  为 Sysdig 实例创建服务密钥。
        ```
        ibmcloud resource service-key-create <key_name> Administrator --instance-id <sysdig_instance_ID>
        ```
        {: pre}
    2.  记下服务密钥的 **Sysdig Access Key** 和 **Sysdig Collector Endpoint**。
        ```
        ibmcloud resource service-key <key_name>
        ```
        {: pre}
        
输出示例：
        ```
        Name:          <key_name>  
        ID:            crn:v1:bluemix:public:sysdig-monitor:<region>:<ID_string>::    
        Created At:    Thu Jun  6 21:31:25 UTC 2019   
        State:         active   
        Credentials:                                   
                       Sysdig Access Key:           11a1aa11-aa1a-1a1a-a111-11a11aa1aa11      
                       Sysdig Collector Endpoint:   ingest.<region>.monitoring.cloud.ibm.com      
                       Sysdig Customer Id:          11111      
                       Sysdig Endpoint:             https://<region>.monitoring.cloud.ibm.com  
                       apikey:                   <api_key_value>      
                       iam_apikey_description:   Auto-generated for key <ID>     
                       iam_apikey_name:          <key_name>       
                       iam_role_crn:             crn:v1:bluemix:public:iam::::role:Administrator      
                       iam_serviceid_crn:        crn:v1:bluemix:public:iam-identity::<ID_string>       
        ```
        {: screen}
3.  运行脚本来为 `ibm-observe` 项目设置特权服务帐户和 Kubernetes 守护程序集，以在 Kubernetes 集群的每个工作程序节点上部署 Sysdig 代理程序。Sysdig 代理程序会收集工作程序节点 CPU 使用率、工作程序节点内存使用情况、与容器之间的 HTTP 流量以及多个基础架构组件相关数据等度量值。 

    在以下命令中，将 <sysdig_access_key> 和 <sysdig_collector_endpoint> 替换为先前创建的服务密钥中的值。对于 <tag>，可以将标记与 Sysdig 代理程序相关联，例如 `role:service,location:us-south`，以帮助确定度量值所来自的环境。

    ```
    curl -sL https://ibm.biz/install-sysdig-k8s-agent | bash -s -- -a <sysdig_access_key> -c <sysdig_collector_endpoint> -t <tag> -ac 'sysdig_capture_enabled: false' --openshift
    ```
    {: pre}
    
    输出示例：
        ```
    * Detecting operating system
    * Downloading Sysdig cluster role yaml
    * Downloading Sysdig config map yaml
    * Downloading Sysdig daemonset v2 yaml
    * Creating project: ibm-observe
    * Creating sysdig-agent serviceaccount in project: ibm-observe
    * Creating sysdig-agent access policies
    * Creating sysdig-agent secret using the ACCESS_KEY provided
    * Retreiving the IKS Cluster ID and Cluster Name
    * Setting cluster name as <cluster_name>
    * Setting ibm.containers-kubernetes.cluster.id 1fbd0c2ab7dd4c9bb1f2c2f7b36f5c47
    * Updating agent configmap and applying to cluster
    * Setting tags
    * Setting collector endpoint
    * Adding additional configuration to dragent.yaml
    * Enabling Prometheus
    configmap/sysdig-agent created
    * Deploying the sysdig agent
    daemonset.extensions/sysdig-agent created
    ```
    {: screen}
        
4.  验证每个节点上的 `sysdig-agent` pod 是否显示 **1/1** pod 已就绪，以及每个 pod 的状态是否为 **Running**。
    ```
    oc get pods
    ```
    {: pre}
    
输出示例：
        ```
    NAME                 READY     STATUS    RESTARTS   AGE
    sysdig-agent-qrbcq   1/1       Running   0          1m
    sysdig-agent-rhrgz   1/1       Running   0          1m
    ```
    {: screen}
5.  在 [{{site.data.keyword.Bluemix_notm}} 可观察性 > 监视控制台](https://cloud.ibm.com/observe/logging)的 {{site.data.keyword.mon_short}} 实例所在行中，单击**查看 Sysdig**。这将打开 Sysdig 仪表板，在其中可以开始分析集群度量值。

有关如何使用 {{site.data.keyword.mon_short}} 的更多信息，请参阅[后续步骤文档](/docs/services/Monitoring-with-Sysdig?topic=Sysdig-kubernetes_cluster#kubernetes_cluster_next_steps)。

### 可选：清除
{: #openshift_logdna_sysdig_cleanup}

从集群和 {{site.data.keyword.Bluemix_notm}} 帐户中除去 {{site.data.keyword.la_short}} 和 {{site.data.keyword.mon_short}} 实例。请注意，除非您在[持久性存储器](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-archiving)中存储了日志和度量值，否则在从帐户中删除实例后，即无法访问这些信息。
{: shortdesc}

1.  通过除去为集群中的 {{site.data.keyword.la_short}} 和 {{site.data.keyword.mon_short}} 实例创建的项目来清除这些实例。删除项目时，还会同时除去其资源，例如服务帐户和守护程序集。
    ```
    oc delete project logdna
    ```
    {: pre}
    ```
    oc delete project ibm-observe
    ```
    {: pre}
2.  从 {{site.data.keyword.Bluemix_notm}} 帐户中除去实例。
    *   [除去 {{site.data.keyword.la_short}} 实例](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-remove)。
    *   [除去 {{site.data.keyword.mon_short}} 实例](/docs/services/Monitoring-with-Sysdig?topic=Sysdig-remove)。

<br />


## 限制

{: #openshift_limitations}

发布的 Red Hat OpenShift on IBM Cloud Beta 存在以下限制。
{: shortdesc}

**集群**：
*   只能创建标准集群，而不能创建免费集群。
*   位置在两个多专区大城市区域（华盛顿和伦敦）中可用。支持的专区为 `wdc04、wdc06、wdc07、lon04、lon05` 和 `lon06`。
*   创建的集群不能包含运行多个操作系统的工作程序节点，例如，Red Hat Enterprise Linux 上的 OpenShift 和 Ubuntu 上的本机 Kubernetes。
*   不支持[集群自动缩放器](/docs/containers?topic=containers-ca)，因为此工具需要 Kubernetes V1.12 或更高版本。OpenShift 3.11 仅包含 Kubernetes V1.11。



**存储器**：
*   支持 {{site.data.keyword.Bluemix_notm}} File Storage、Block Storage 和 Cloud Object Storage。不支持软件定义的存储 (SDS)。
*   受 {{site.data.keyword.Bluemix_notm}} NFS 文件存储器配置 Linux 用户许可权的方式的影响，在使用文件存储器时可能会遇到错误。如果发生这种情况，可能需要配置 [OpenShift 安全上下文约束 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://docs.openshift.com/container-platform/3.11/admin_guide/manage_scc.html) 或使用其他存储类型。

**联网**：
*   用作联网策略提供程序的是 Calico，而不是 OpenShift SDN。

**附加组件、集成和其他服务**：
*   {{site.data.keyword.containerlong_notm}} 附加组件（例如，Istio、Knative 和 Kubernetes Terminal）不可用。
*   Helm chart 未经过认证，无法在 OpenShift 集群中使用，但 {{site.data.keyword.Bluemix_notm}} Object Storage 除外。
*   集群未部署为使用 {{site.data.keyword.registryshort_notm}} `icr.io` 域的映像拉取私钥。可以[创建您自己的映像拉取私钥](/docs/containers?topic=containers-images#other_registry_accounts)，或者改为使用 OpenShift 集群的内置 Docker 注册表。

**应用程序**：
*   缺省情况下，OpenShift 设置的安全设置比本机 Kubernetes 更严格。有关更多信息，请参阅有关[管理安全上下文约束 (SCC) ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://docs.openshift.com/container-platform/3.11/admin_guide/manage_scc.html) 的 OpenShift 文档。
*   例如，配置为以 root 用户身份运行的应用程序可能失败，其中 pod 处于 `CrashLoopBackOff` 状态。要解决此问题，可以修改缺省安全上下文约束，或使用不以 root 用户身份运行的映像。
*   缺省情况下，OpenShift 设置为使用本地 Docker 注册表。如果要使用存储在远程专用 {{site.data.keyword.registrylong_notm}} `icr.io` 域名中的映像，那么必须自行为每个全局和区域注册表创建私钥。可以使用[复制 `default-<region>-icr-io` 私钥](/docs/containers?topic=containers-images#copy_imagePullSecret)将这些私钥从 `default` 名称空间复制到要从中拉取映像的名称空间，或者可以[创建您自己的私钥](/docs/containers?topic=containers-images#other_registry_accounts)。然后，向部署配置或名称空间服务帐户[添加映像拉取私钥](/docs/containers?topic=containers-images#use_imagePullSecret)。
*   将使用 OpenShift 控制台，而不是 Kubernetes 仪表板。

<br />


## 接下来要做什么？
{: #openshift_next}

有关使用应用程序和路由服务的更多信息，请参阅 [OpenShift Developer Guide](https://docs.openshift.com/container-platform/3.11/dev_guide/index.html)。

<br />


## 反馈和问题
{: #openshift_support}

在处于 Beta 阶段期间，IBM 支持人员和 Red Hat 支持人员的服务范围均不包括 Red Hat OpenShift on IBM Cloud 集群。提供的任何支持都旨在帮助您评估该产品，为其正式发布做准备。
{: important}

有关任何问题或反馈，请在 Slack 中发布帖子。 
*   如果您是外部用户，请在 [#openshift ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://ibm-container-service.slack.com/messages/CKCJLJCH4) 通道中发布帖子。 
*   如果您是 IBM 员工，请使用 [#iks-openshift-users ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://ibm-argonauts.slack.com/messages/CJH0UPN2D) 通道。

如果未使用 {{site.data.keyword.Bluemix_notm}} 帐户的 IBM 标识，请针对此 Slack [请求邀请](https://bxcs-slack-invite.mybluemix.net/)。
{: tip}
