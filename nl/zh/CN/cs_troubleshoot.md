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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}



# 调试集群
{: #cs_troubleshoot}

在使用 {{site.data.keyword.containerlong}} 时，请考虑对集群进行常规故障诊断和调试的以下方法。您还可以检查 [{{site.data.keyword.Bluemix_notm}} 系统的状态 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://developer.ibm.com/bluemix/support/#status)。
{: shortdesc}

您可以执行以下常规步骤来确保集群是最新的：
- 每月检查可用安全性和操作系统补丁以[更新工作程序节点](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update)。
- [更新集群](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_update)到 {{site.data.keyword.containerlong_notm}} 的最新缺省 [Kubernetes 版本](/docs/containers?topic=containers-cs_versions)<p class="important">确保 [`kubectl` CLI](/docs/containers?topic=containers-cs_cli_install#kubectl) 客户机的 Kubernetes 版本与集群服务器的相匹配。[Kubernetes 不支持 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/setup/version-skew-policy/) 比服务器版本高/低 2 个或更多版本 (n +/- 2) 的 `kubectl` 客户机版本。</p>

## 使用 {{site.data.keyword.containerlong_notm}} 诊断和调试工具运行测试
{: #debug_utility}

进行故障诊断时，可以使用 {{site.data.keyword.containerlong_notm}} 诊断和调试工具来运行测试并从集群收集相关信息。要使用调试工具，请安装 [`ibmcloud-iks-debug` Helm chart ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/kubernetes/helm/iks-charts/ibmcloud-iks-debug)：
{: shortdesc}


1. [在集群中设置 Helm，为 Tiller 创建服务帐户，然后将 `ibm` 存储库添加到 Helm 实例](/docs/containers?topic=containers-helm)。

2. 将 Helm chart 安装到集群。
  ```
  helm install ibm/ibmcloud-iks-debug --name debug-tool
  ```
  {: pre}


3. 启动代理服务器以显示调试工具接口。
  ```
  kubectl proxy --port 8080
  ```
  {: pre}

4. 在 Web 浏览器中，打开调试工具接口 URL：http://localhost:8080/api/v1/namespaces/default/services/debug-tool-ibmcloud-iks-debug:8822/proxy/page

5. 选择单个测试或一组测试来运行。一些测试检查是否存在潜在警告、错误或问题，一些测试仅收集在故障诊断期间可以参考的信息。有关每个测试的功能的更多信息，请单击测试名称旁边的“信息”图标。

6. 单击**运行**。

7. 检查每个测试的结果。
  * 如果任何测试失败，请单击左侧列中测试名称旁边的“信息”图标，以获取有关如何解决此问题的信息。
  * 您还可以使用测试结果来收集信息（例如，完整的 YAML），这些信息可以帮助您在以下各部分中调试集群。

## 调试集群
{: #debug_clusters}

复查可用于调试集群并查找失败根本原因的选项。

1.  列出集群并找到集群的 `State`。

  ```
  ibmcloud ks clusters
  ```
  {: pre}

2.  复查集群的 `State`。如果集群处于 **Critical**、**Delete failed** 或 **Warning** 状态，或者长时间卡在 **Pending** 状态，请开始[调试工作程序节点](#debug_worker_nodes)。

您可以通过运行 `ibmcloud ks clusters` 命令并找到 **State** 字段，查看当前集群状态。
{: shortdesc}

<table summary="每个表行都应从左到右阅读，其中第一列是集群状态，第二列是描述。">
  <caption>集群状态</caption>
   <thead>
   <th>集群状态</th>
   <th>描述</th>
   </thead>
   <tbody>
<tr>
   <td>`Aborted`</td>
   <td>在部署 Kubernetes 主节点之前，用户请求删除集群。在集群删除完成后，将从仪表板中除去集群。如果集群长时间卡在此状态，请打开 [{{site.data.keyword.Bluemix_notm}} 支持案例](/docs/containers?topic=containers-cs_troubleshoot#ts_getting_help)。</td>
   </tr>
 <tr>
     <td>`Critical`</td>
     <td>无法访问 Kubernetes 主节点，或者集群中的所有工作程序节点都已停止运行。</td>
    </tr>
   <tr>
     <td>`Delete failed`</td>
     <td>Kubernetes 主节点或至少一个工作程序节点无法删除。</td>
   </tr>
   <tr>
     <td>`Deleted`</td>
     <td>集群已删除，但尚未从仪表板中除去。如果集群长时间卡在此状态，请打开 [{{site.data.keyword.Bluemix_notm}} 支持案例](/docs/containers?topic=containers-cs_troubleshoot#ts_getting_help)。</td>
   </tr>
   <tr>
   <td>`Deleting`</td>
   <td>正在删除集群，并且正在拆除集群基础架构。无法访问集群。</td>
   </tr>
   <tr>
     <td>`Deploy failed`</td>
     <td>无法完成 Kubernetes 主节点的部署。您无法解决此状态。请通过打开 [{{site.data.keyword.Bluemix_notm}} 支持案例](/docs/containers?topic=containers-cs_troubleshoot#ts_getting_help)来联系 IBM Cloud 支持人员。</td>
   </tr>
     <tr>
       <td>`Deploying`</td>
       <td>Kubernetes 主节点尚未完全部署。无法访问集群。请等待集群完全部署后，再复查集群的运行状况。</td>
      </tr>
      <tr>
       <td>`Normal`</td>
       <td>集群中的所有工作程序节点都已启动并正在运行。您可以访问集群，并将应用程序部署到集群。此状态视为正常运行，不需要您执行操作。<p class="note">虽然工作程序节点可能是正常的，但其他基础架构资源（例如，[联网](/docs/containers?topic=containers-cs_troubleshoot_network)和[存储](/docs/containers?topic=containers-cs_troubleshoot_storage)）可能仍然需要注意。如果刚创建了集群，集群中由其他服务使用的某些部分（例如，Ingress 私钥或注册表映像拉取私钥）可能仍在进行中。</p></td>
    </tr>
      <tr>
       <td>`Pending`</td>
       <td>Kubernetes 主节点已部署。正在供应工作程序节点，这些节点在集群中尚不可用。您可以访问集群，但无法将应用程序部署到集群。</td>
     </tr>
   <tr>
     <td>`Requested`</td>
     <td>发送了用于创建集群并为 Kubernetes 主节点和工作程序节点订购基础架构的请求。集群部署启动后，集群状态将更改为 <code>Deploying</code>。如果集群长时间卡在 <code>Requested</code> 状态，请打开 [{{site.data.keyword.Bluemix_notm}} 支持案例](/docs/containers?topic=containers-cs_troubleshoot#ts_getting_help)。</td>
   </tr>
   <tr>
     <td>`Updating`</td>
     <td>在 Kubernetes 主节点中运行的 Kubernetes API 服务器正在更新到新的 Kubernetes API 版本。在更新期间，您无法访问或更改集群。用户已部署的工作程序节点、应用程序和资源不会被修改，并且将继续运行。等待更新完成后，再复查集群的运行状况。</td>
   </tr>
   <tr>
    <td>`Unsupported`</td>
    <td>不再支持集群运行的 [Kubernetes 版本](/docs/containers?topic=containers-cs_versions#cs_versions)。不再主动监视或报告集群的运行状况。此外，无法添加或重新装入工作程序节点。要继续接收重要的安全性更新和支持，必须更新集群。请查看[版本更新准备操作](/docs/containers?topic=containers-cs_versions#prep-up)，然后[更新集群](/docs/containers?topic=containers-update#update)，以将集群更新为支持的 Kubernetes 版本。<br><br><p class="note">无法更新比最旧受支持版本低三个或更多版本的集群。要避免此情况，可以将集群更新为比当前版本高最多两个版本的 Kubernetes 版本，例如从 1.12 更新到 1.14。此外，如果集群运行的是 V1.5、1.7 或 1.8，那么此版本落后太多，无法更新。必须改为[创建集群](/docs/containers?topic=containers-clusters#clusters)，并[部署应用程序](/docs/containers?topic=containers-app#app)至该集群。</p></td>
   </tr>
    <tr>
       <td>`Warning`</td>
       <td>集群中至少有一个工作程序节点不可用，但其他工作程序节点可用，并且可以接管工作负载。</td>
    </tr>
   </tbody>
 </table>


[Kubernetes 主节点](/docs/containers?topic=containers-ibm-cloud-kubernetes-service-technology#architecture)是用于保持集群正常启动并运行的主组件。主节点将集群资源及其配置存储在充当集群单个事实点的 etcd 数据库中。Kubernetes API 服务器是从工作程序节点到主节点的所有集群管理请求或者想要与集群资源交互时的主入口点。<br><br>如果主节点发生故障，那么工作负载将继续在工作程序节点上运行，但是无法使用 `kubectl` 命令来处理集群资源或查看集群运行状况，直至主节点中的 Kubernetes API 服务器恢复运行。如果在主节点停运期间 pod 停止运行，那么在工作程序节点可再次访问 Kubernetes API 服务器之前，将无法重新调度 pod。<br><br>在主节点停运期间，您仍可以针对 {{site.data.keyword.containerlong_notm}} API 运行 `ibmcloud ks` 命令以处理基础架构资源，例如，工作程序节点或 VLAN。如果通过向集群添加或从中除去工作程序节点来更改当前集群配置，那么在主节点恢复运行前，更改不会发生。

在主节点停运期间，请勿重新启动或重新引导工作程序节点。此操作会从工作程序节点中除去 pod。因为 Kubernetes API 服务器不可用，因此无法将 pod 重新调度到集群中的其他工作程序节点。
{: important}


<br />


## 调试工作程序节点
{: #debug_worker_nodes}

复查可用于调试工作程序节点并查找故障根本原因的选项。

<ol><li>如果集群处于 **Critical**、**Delete failed** 或 **Warning** 状态，或者长时间卡在 **Pending** 状态，请复查工作程序节点的状态。<p class="pre">ibmcloud ks workers --cluster <cluster_name_or_id></p></li>
  <li>复查 CLI 输出中每个工作程序节点的 **State** 和 **Status** 字段。<p>您可以通过运行 `ibmcloud ks workers --cluster <cluster_name_or_ID` 命令并找到 **State** 和 **Status** 字段，查看当前工作程序节点状态。
{: shortdesc}

<table summary="每个表行都应从左到右阅读，其中第一列是集群状态，第二列是描述。">
  <caption>工作程序节点状态</caption>
  <thead>
  <th>工作程序节点状态</th>
  <th>描述</th>
  </thead>
  <tbody>
<tr>
    <td>`Critical`</td>
    <td>工作程序节点可能由于许多原因而进入 Critical 状态：<ul><li>对工作程序节点启动了重新引导，但并未对工作程序节点执行封锁和放弃操作。重新引导工作程序节点可能会导致 <code>containerd</code>、<code>kubelet</code>、<code>kube-proxy</code> 和 <code>calico</code> 中发生数据损坏。</li>
    <li>部署到工作程序节点的 pod 不会对[内存 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/tasks/configure-pod-container/assign-memory-resource/) 和 [CPU ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/tasks/configure-pod-container/assign-cpu-resource/) 使用资源限制。如果没有资源限制，那么 pod 可以使用所有可用资源，这样就没有资源可供其他 pod 在此工作程序节点上运行。这一过度落实工作负载的情况会导致工作程序节点失败。</li>
    <li>在运行数百个或数千个容器一段时间后，<code>containerd</code>、<code>kubelet</code> 或 <code>calico</code> 进入了不可恢复的状态。</li>
    <li>为工作程序节点设置了虚拟路由器设备，但虚拟路由器设备已停止运行并切断了工作程序节点与 Kubernetes 主节点之间的通信。</li><li> {{site.data.keyword.containerlong_notm}} 或 IBM Cloud Infrastructure (SoftLayer) 中导致工作程序节点与 Kubernetes 主节点之间通信失败的当前网络问题。</li>
    <li>工作程序节点的容量不足。检查工作程序节点的 <strong>Status</strong>，以查看它是显示 <strong>Out of disk</strong> 还是 <strong>Out of memory</strong>。如果工作程序节点的容量不足，请考虑减少工作程序节点上的工作负载，或者向集群添加工作程序节点来帮助对工作负载进行负载均衡。</li>
    <li>设备已通过 [{{site.data.keyword.Bluemix_notm}} 控制台资源列表 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/resources) 关闭电源。打开资源列表，并在**设备**列表中找到工作程序节点标识。在“操作”菜单中，单击**打开电源**。</li></ul>
在许多情况下，[重新装入](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)工作程序节点可以解决此问题。重新装入工作程序节点时，最新的[补丁版本](/docs/containers?topic=containers-cs_versions#version_types)会应用于工作程序节点。主版本和次版本不会更改。在重新装入工作程序节点之前，请务必对工作程序节点执行封锁和放弃操作，以确保正常终止现有 pod 并将其重新安排到剩余的工作程序节点上。</br></br>如果重新装入工作程序节点无法解决此问题，请转至下一步以继续对工作程序节点进行故障诊断。</br></br><strong>提示：</strong>可以[为工作程序节点配置运行状况检查并启用自动恢复](/docs/containers?topic=containers-health#autorecovery)。如果自动恢复根据配置的检查，检测到运行状况欠佳的工作程序节点，那么自动恢复会触发更正操作，例如在工作程序节点上重装操作系统。有关自动恢复的工作方式的更多信息，请参阅[自动恢复博客 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/blogs/bluemix/2017/12/autorecovery-utilizes-consistent-hashing-high-availability/)。</td>
   </tr>
   <tr>
   <td>`已部署`</td>
   <td>已成功将更新部署到工作程序节点。在部署更新后，{{site.data.keyword.containerlong_notm}} 在工作程序节点上启动运行状况检查。在运行状况检查成功后，工作程序节点进入 <code>Normal</code> 状态。<code>Deployed</code> 状态的工作程序节点通常已准备好接收工作负载，可通过运行 <code>kubectl get nodes</code> 并确认状态显示 <code>Normal</code> 进行检查。</td>
   </tr>
    <tr>
      <td>`Deploying`</td>
      <td>更新工作程序节点的 Kubernetes 版本时，将重新部署工作程序节点以安装更新。如果重新装入或重新引导工作程序节点，那么将重新部署工作程序节点以自动安装最新的补丁版本。如果工作程序节点长时间卡在此状态，请继续执行下一步，以查看在部署期间是否发生了问题。</td>
   </tr>
      <tr>
      <td>`Normal`</td>
      <td>工作程序节点已完全供应并准备就绪，可以在集群中使用。此状态视为正常运行，不需要用户执行操作。**注**：虽然工作程序节点可能是正常的，但其他基础架构资源（例如，[联网](/docs/containers?topic=containers-cs_troubleshoot_network)和[存储](/docs/containers?topic=containers-cs_troubleshoot_storage)）可能仍然需要注意。</td>
   </tr>
 <tr>
      <td>`Provisioning`</td>
      <td>正在供应工作程序节点，该节点在集群中尚不可用。您可以在 CLI 输出的 <strong>Status</strong> 列中监视供应过程。如果工作程序节点长时间卡在此状态，请继续执行下一步，以查看在供应期间是否发生了问题。</td>
    </tr>
    <tr>
      <td>`Provision_failed`</td>
      <td>无法供应工作程序节点。继续执行下一步以找到失败的详细信息。</td>
    </tr>
 <tr>
      <td>`Reloading`</td>
      <td>正在重新装入工作程序节点，该节点在集群中不可用。您可以在 CLI 输出的 <strong>Status</strong> 列中监视重新装入过程。如果工作程序节点长时间卡在此状态，请继续执行下一步，以查看在重新装入期间是否发生了问题。</td>
     </tr>
     <tr>
      <td>`Reloading_failed`</td>
      <td>无法重新装入工作程序节点。继续执行下一步以找到失败的详细信息。</td>
    </tr>
    <tr>
      <td>`Reload_pending `</td>
      <td>发送了用于重新装入或更新工作程序节点的 Kubernetes 版本的请求。正在重新装入工作程序节点时，状态将更改为 <code>Reloading</code>。</td>
    </tr>
    <tr>
     <td>`Unknown`</td>
     <td>由于以下某种原因，Kubernetes 主节点不可访问：<ul><li>您请求了更新 Kubernetes 主节点。在更新期间无法检索到工作程序节点的状态。如果工作程序节点长时间保持此状态，即使在成功更新 Kubernetes 主节点之后也是如此，请尝试[重新装入](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)工作程序节点。</li><li>您可能有其他防火墙在保护工作程序节点，或者最近更改了防火墙设置。{{site.data.keyword.containerlong_notm}} 需要打开特定 IP 地址和端口，以允许工作程序节点与 Kubernetes 主节点之间进行通信。有关更多信息，请参阅[防火墙阻止工作程序节点进行连接](/docs/containers?topic=containers-cs_troubleshoot_clusters#cs_firewall)。</li><li>Kubernetes 主节点已停止运行。请通过打开 [{{site.data.keyword.Bluemix_notm}} 支持案例](/docs/containers?topic=containers-cs_troubleshoot#ts_getting_help)来联系 {{site.data.keyword.Bluemix_notm}} 支持人员。</li></ul></td>
</tr>
   <tr>
      <td>`Warning`</td>
      <td>工作程序节点将达到内存或磁盘空间的限制。您可以减少工作程序节点上的工作负载，或者向集群添加一个工作程序节点以帮助均衡工作负载。</td>
</tr>
  </tbody>
</table>
</p></li>
<li>列出工作程序节点的详细信息。如果详细信息包含错误消息，请查看[工作程序节点的常见错误消息](#common_worker_nodes_issues)列表以了解如何解决问题。<p class="pre">ibmcloud ks worker-get --cluster <cluster_name_or_id> --worker <worker_node_id></li>
  </ol>

<br />


## 工作程序节点的常见问题
{: #common_worker_nodes_issues}

查看常见错误消息并了解如何解决这些错误。

  <table>
  <caption>常见错误消息</caption>
    <thead>
    <th>错误消息</th>
    <th>描述和解决方法
    </thead>
    <tbody>
      <tr>
        <td>{{site.data.keyword.Bluemix_notm}} Infrastructure 异常：当前禁止您的帐户订购“计算实例”。</td>
        <td>您的 IBM Cloud Infrastructure (SoftLayer) 帐户可能受到限制，无法订购计算资源。请通过打开 [{{site.data.keyword.Bluemix_notm}} 支持案例](#ts_getting_help)来联系 {{site.data.keyword.Bluemix_notm}} 支持人员。</td>
      </tr>
      <tr>
      <td>{{site.data.keyword.Bluemix_notm}} Infrastructure 异常：无法下单。<br><br>
      {{site.data.keyword.Bluemix_notm}} Infrastructure 异常：无法下单。路由器“router_name”后的资源不足，无法实现以下访客的请求：“worker_id”。</td>
      <td>选择的专区可能没有足够的基础架构容量来供应工作程序节点。或者，您可能已超出 IBM Cloud Infrastructure (SoftLayer) 帐户中的限制。要解决此问题，请尝试以下某个选项：<ul><li>专区中的基础架构资源可用性可能经常波动。请稍等几分钟，然后重试。</li>
      <li>对于单专区集群，请在另一个专区中创建集群。对于多专区集群，请将专区添加到集群。</li>
      <li>在 IBM Cloud infrastructure (SoftLayer) 帐户中，为工作程序节点指定不同的公共和专用 VLAN 对。对于位于工作程序池中的工作程序节点，您可以使用 <code>ibmcloud ks zone-network-set</code> [命令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_zone_network_set)。</li>
      <li>请联系 IBM Cloud Infrastructure (SoftLayer) 帐户管理者以验证未超过帐户限制，例如，全球配额。</li>
      <li>打开 [IBM Cloud Infrastructure (SoftLayer) 支持案例](#ts_getting_help)</li></ul></td>
      </tr>
      <tr>
        <td>{{site.data.keyword.Bluemix_notm}} 基础架构异常：无法获取标识为 <code>&lt;vlan id&gt;</code> 的网络 VLAN。</td>
        <td>由于以下某种原因而找不到所选的 VLAN 标识，因此无法供应工作程序节点：<ul><li>您可能指定的是 VLAN 编号，而不是 VLAN 标识。VLAN 编号的长度为 3 位或 4 位，而 VLAN 标识的长度为 7 位。运行 <code>ibmcloud ks vlans --zone &lt;zone&gt;</code> 以检索 VLAN 标识。<li>该 VLAN 标识可能未与所使用的 IBM Cloud Infrastructure (SoftLayer) 帐户相关联。运行 <code>ibmcloud ks vlans --zone &lt;zone&gt;</code> 以列出您帐户的可用 VLAN 标识。要更改 IBM Cloud Infrastructure (SoftLayer) 帐户，请参阅 [`ibmcloud ks credential-set`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_set)。</ul></td>
      </tr>
      <tr>
        <td>SoftLayer_Exception_Order_InvalidLocation：为此订单提供的位置无效。(HTTP 500)</td>
        <td>IBM Cloud Infrastructure (SoftLayer) 未设置为订购所选数据中心内的计算资源。请联系 [{{site.data.keyword.Bluemix_notm}} 支持](#ts_getting_help)，以验证您的帐户是否正确设置。</td>
       </tr>
       <tr>
        <td>{{site.data.keyword.Bluemix_notm}} Infrastructure 异常：用户没有必需的 {{site.data.keyword.Bluemix_notm}} Infrastructure 许可权来添加服务器</br></br>
        {{site.data.keyword.Bluemix_notm}} Infrastructure 异常：必须具有许可权才能订购“项”。
        </br></br>
        无法验证 {{site.data.keyword.Bluemix_notm}} infrastructure 凭证。</td>
        <td>您可能没有必需的许可权在 IBM Cloud Infrastructure (SoftLayer) 产品服务组合中执行操作，或者使用的基础架构凭证不正确。请参阅[设置 API 密钥以启用对基础架构产品服务组合的访问](/docs/containers?topic=containers-users#api_key)。</td>
      </tr>
      <tr>
       <td>工作程序无法与 {{site.data.keyword.containerlong_notm}} 服务器通信。请验证防火墙设置是否允许来自此工作程序的流量。
       <td><ul><li>如果您有防火墙，请[配置防火墙设置以允许出局流量流至相应的端口和 IP 地址](/docs/containers?topic=containers-firewall#firewall_outbound)。</li>
       <li>通过运行 `ibmcloud ks workers --cluster &lt;mycluster&gt;` 来检查集群是否没有公共 IP。如果未列出任何公共 IP，说明集群仅具有专用 VLAN。<ul><li>如果希望集群仅具有专用 VLAN，请设置 [VLAN 连接](/docs/containers?topic=containers-plan_clusters#private_clusters)和[防火墙](/docs/containers?topic=containers-firewall#firewall_outbound)。</li>
       <li>如果启用帐户以使用 [VRF](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) 和[服务端点](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started)之前，创建的集群仅具有专用服务端点，那么工作程序无法连接到主节点。如果是，请尝试[设置公共服务端点](/docs/containers?topic=containers-cs_network_cluster#set-up-public-se)，以便可以一直使用集群，直到支持案例得到处理，可以更新帐户。
如果在更新帐户后仍需要仅专用服务端点集群，那么可以禁用公共服务端点。</li>
       <li>如果希望集群具有公共 IP，请[添加新的工作程序节点](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_add)（具有公用和专用 VLAN）。</li></ul></li></ul></td>
     </tr>
      <tr>
  <td>无法创建 IMS 门户网站令牌，因为没有 IMS 帐户链接到所选的 BSS 帐户</br></br>提供的用户找不到或不处于活动状态</br></br>SoftLayer_Exception_User_Customer_InvalidUserStatus：用户帐户当前处于 cancel_pending 状态。</br></br>等待机器对用户可见</td>
  <td>用于访问 IBM Cloud Infrastructure (SoftLayer) 产品服务组合的 API 密钥的所有者没有执行此操作的必需许可权，或者可能处于暂挂待删除状态。</br></br><strong>以该用户身份</strong>，执行以下步骤：
  <ol><li>如果您有权访问多个帐户，请确保您已登录到要使用 {{site.data.keyword.containerlong_notm}} 的帐户。</li>
  <li>运行 <code>ibmcloud ks api-key-info --cluster &lt;cluster_name_or_ID&gt;</code> 以查看用于访问 IBM Cloud Infrastructure (SoftLayer) 产品服务组合的当前 API 密钥所有者。</li>
  <li>运行 <code>ibmcloud account list</code> 以查看当前使用的 {{site.data.keyword.Bluemix_notm}} 帐户的所有者。</li>
  <li>请与 {{site.data.keyword.Bluemix_notm}} 帐户的所有者联系，并报告 API 密钥所有者在 IBM Cloud Infrastructure (SoftLayer) 中的许可权不足，或者可能暂挂待删除。</li></ol>
  </br><strong>以帐户所有者身份</strong>，执行以下步骤：<ol><li>复查 [IBM Cloud Infrastructure (SoftLayer) 中的必需许可权](/docs/containers?topic=containers-users#infra_access)，即执行先前失败的操作所需的许可权。</li>
  <li>使用 [<code>ibmcloud ks api-key-reset --region <region></code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_api_key_reset) 命令来修正 API 密钥所有者的许可权或创建新的 API 密钥。</li>
  <li>如果您或其他帐户管理员在您的帐户中手动设置了 IBM Cloud Infrastructure (SoftLayer) 凭证，请运行 [<code>ibmcloud ks credential-unset --region <region></code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_unset) 以从您的帐户中除去这些凭证。</li></ol></td>
  </tr>
    </tbody>
  </table>

<br />


## 查看主节点运行状况
{: #debug_master}

{{site.data.keyword.containerlong_notm}} 包含 IBM 管理的主节点（具有高可用性副本）、应用的自动安全补丁更新，以及实施的用于在发生事件时进行恢复的自动化功能。您可以通过运行 `ibmcloud ks cluster-get --cluster <cluster_name_or_ID>` 来检查集群主节点的运行状况、阶段状态和状态。
{: shortdesc} 

**主节点运行状况**<br>
**主节点运行状况**反映了主组件的状态，并在发生需要您注意的事件时通知您。运行状况可能为下列其中一项：
*   `error`：主节点不可运行。IBM 会自动收到通知，并采取行动来解决此问题。您可以继续监视运行状况，直到主节点为 `normal`。
*   `normal`：主节点正常运行。无需任何操作。
*   `unavailable`：主节点可能不可访问，这意味着某些操作（例如，调整工作程序池的大小）暂时不可用。IBM 会自动收到通知，并采取行动来解决此问题。您可以继续监视运行状况，直到主节点为 `normal`。 
*   `unsupported`：主节点运行的是不支持的 Kubernetes 版本。必须[更新集群](/docs/containers?topic=containers-update)，才能使主节点恢复为 `normal` 运行状况。

**主节点阶段状态和状态**<br>
**主节点阶段状态**提供主节点状态中正在执行哪个操作的详细信息。阶段状态包括主节点处于同一状态的时间长度的时间戳记，例如 `Ready (1 month ago)`。**主节点状态**反映的是可以在主节点上执行的可能操作的生命周期，例如 deploying、updating 和 deleting。下表中描述了每个状态。

<table summary="每个表行应从左到右阅读，其中第一列是主节点状态，第二列是描述。">
<caption>主节点状态</caption>
   <thead>
   <th>主节点状态</th>
   <th>描述</th>
   </thead>
   <tbody>
<tr>
   <td>`deployed`</td>
   <td>主节点已成功部署。检查阶段状态以验证主节点是否处于 `Ready` 状态，或者查看更新是否可用。</td>
   </tr>
 <tr>
     <td>`deploying`</td>
     <td>当前正在部署主节点。在使用集群（例如，添加工作程序节点）之前，等待状态变为 `deployed`。</td>
    </tr>
   <tr>
     <td>`deploy_failed`</td>
     <td>主节点部署失败。IBM 支持人员会收到相关通知，并努力解决此问题。查看 **Master Status** 字段以获取更多信息，或等待状态变为 `deployed`。</td>
   </tr>
   <tr>
   <td>`deleting`</td>
   <td>由于删除了集群，因此当前正在删除主节点。无法撤销删除操作。删除集群后，即无法再检查主节点状态，因为已完全除去了集群。</td>
   </tr>
     <tr>
       <td>`delete_failed`</td>
       <td>主节点删除失败。IBM 支持人员会收到相关通知，并努力解决此问题。无法通过重试删除集群来解决此问题。请改为查看 **Master Status** 字段以获取更多信息，或等待集群删除。</td>
      </tr>
      <tr>
       <td>`更新`</td>
       <td>主节点正在更新其 Kubernetes 版本。更新可能是自动应用的补丁更新，或者是通过更新集群应用的次版本或主版本。更新期间，高可用性主节点可以继续处理请求，并且应用程序工作负载和工作程序节点将继续运行。主节点更新完成后，可以[更新工作程序节点](/docs/containers?topic=containers-update#worker_node)。<br><br>
       如果更新失败，那么主节点会恢复为 `deployed` 状态，并继续运行先前的版本。IBM 支持人员会收到相关通知，并努力解决此问题。可以在 **Master Status** 字段中查看更新是否失败。</td>
    </tr>
   </tbody>
 </table>


<br />



## 调试应用程序部署
{: #debug_apps}

复查您可用于调试应用程序部署并查找失败根本原因的选项。

开始之前，请确保您具有对部署了应用程序的名称空间的 [{{site.data.keyword.Bluemix_notm}} IAM **写入者**或**管理者**服务角色](/docs/containers?topic=containers-users#platform)。

1. 通过运行 `describe` 命令，在服务或部署资源中查找异常情况。

 示例：
 <pre class="pre"><code>kubectl describe service &lt;service_name&gt; </code></pre>

2. [检查容器是否卡在 `ContainerCreating` 状态](/docs/containers?topic=containers-cs_troubleshoot_storage#stuck_creating_state)。

3. 检查集群是否处于 `Critical` 状态。如果集群处于 `Critical` 状态，请检查防火墙规则，并验证主节点是否能与工作程序节点进行通信。

4. 验证该服务是否正在侦听正确的端口。
   1. 获取 pod 的名称。
<pre class="pre"><code>kubectl get pods</code></pre>
   2. 登录到容器。
<pre class="pre"><code>kubectl exec -it &lt;pod_name&gt; -- /bin/bash</code></pre>
   3. 从容器中 Curl 应用程序。如果该端口不可访问，那么该服务可能未在侦听正确的端口，或者应用程序可能存在问题。使用正确的端口更新服务的配置文件，并重新部署或调查应用程序的潜在问题。<pre class="pre"><code>curl localhost: &lt;port&gt;</code></pre>

5. 验证服务是否已正确链接到 Pod。
   1. 获取 pod 的名称。
<pre class="pre"><code>kubectl get pods</code></pre>
   2. 登录到容器。
<pre class="pre"><code>kubectl exec -it &lt;pod_name&gt; -- /bin/bash</code></pre>
   3. Curl 服务的集群 IP 地址和端口。如果 IP 地址和端口不可访问，请查看服务的端点。如果没有列出任何端点，说明服务的选择器与 pod 不匹配。如果列出了端点，请查看服务上的“目标端口”字段，并确保目标端口与用于这些 pod 的目标端口相同。
     <pre class="pre"><code>curl &lt;cluster_IP&gt;:&lt;port&gt;</code></pre>

6. 对于 Ingress 服务，请验证可从集群内访问该服务。
   1. 获取 pod 的名称。
<pre class="pre"><code>kubectl get pods</code></pre>
   2. 登录到容器。
<pre class="pre"><code>kubectl exec -it &lt;pod_name&gt; -- /bin/bash</code></pre>
   2. Curl 为 Ingress 服务指定的 URL。如果该 URL 不可访问，请检查集群和外部端点之间是否存在防火墙问题。
<pre class="pre"><code>curl &lt;host_name&gt;.&lt;domain&gt;</code></pre>

<br />



## 获取帮助和支持
{: #ts_getting_help}

集群仍然有问题吗？
{: shortdesc}

-  在终端中，当有 `ibmcloud` CLI 和插件的更新可用时，您会收到通知。务必使 CLI 保持最新，以便您可以使用所有可用的命令和标志。
-   要查看 {{site.data.keyword.Bluemix_notm}} 是否可用，请[检查 {{site.data.keyword.Bluemix_notm}} 状态页面 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/status?selected=status)。
-   在 [{{site.data.keyword.containerlong_notm}} Slack ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://ibm-container-service.slack.com) 中发布问题。
    如果未将 IBM 标识用于 {{site.data.keyword.Bluemix_notm}} 帐户，请针对此 Slack [请求邀请](https://bxcs-slack-invite.mybluemix.net/)。
    {: tip}
-   请复查论坛，以查看是否有其他用户遇到相同的问题。使用论坛进行提问时，请使用适当的标记来标注您的问题，以方便 {{site.data.keyword.Bluemix_notm}} 开发团队识别。
    -   如果您有关于使用 {{site.data.keyword.containerlong_notm}} 开发或部署集群或应用程序的技术问题，请在 [Stack Overflow ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://stackoverflow.com/questions/tagged/ibm-cloud+containers) 上发布您的问题，并使用 `ibm-cloud`、`kubernetes` 和 `containers` 标记您的问题。
    -   有关服务的问题和入门指示信息，请使用 [IBM Developer Answers ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix) 论坛。请加上 `ibm-cloud` 和 `containers` 标记。
    有关使用论坛的更多详细信息，请参阅[获取帮助](/docs/get-support?topic=get-support-getting-customer-support#using-avatar)。
-   通过打开案例来联系 IBM 支持人员。要了解有关打开 IBM 支持案例或有关支持级别和案例严重性的信息，请参阅[联系支持人员](/docs/get-support?topic=get-support-getting-customer-support)。报告问题时，请包含集群标识。要获取集群标识，请运行 `ibmcloud ks clusters`。您还可以使用 [{{site.data.keyword.containerlong_notm}} 诊断和调试工具](/docs/containers?topic=containers-cs_troubleshoot#debug_utility)从集群收集相关信息并导出这些信息，以便与 IBM 支持人员共享。
{: tip}

