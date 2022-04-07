---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: kubernetes, iks, node scaling

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



# 缩放集群
{: #ca}

通过 {{site.data.keyword.containerlong_notm}} `ibm-iks-cluster-autoscaler` 插件，您可以根据安排的工作负载的大小调整需求，自动缩放集群中的工作程序池，以增加或减少工作程序池中的工作程序节点数。`ibm-iks-cluster-autoscaler` 插件基于 [Kubernetes Cluster-Autoscaler 项目 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/autoscaler/tree/master/cluster-autoscaler)。
{: shortdesc}

要改为自动缩放 pod 吗？请查看[缩放应用程序](/docs/containers?topic=containers-app#app_scaling)。
{: tip}

集群自动缩放器可用于设置为使用公用网络连接的标准集群。如果集群无法访问公用网络（例如，防火墙后面的专用集群或仅启用了专用服务端点的集群），那么无法在集群中使用集群自动缩放器。
{: important}

## 了解扩展和缩减
{: #ca_about}

集群自动缩放器会定期扫描集群，以根据工作负载资源请求以及配置的任何定制设置（如扫描时间间隔），调整它所管理的工作程序池中的工作程序节点数。集群自动缩放器每分钟会检查一次以下情况。
{: shortdesc}

*   **有暂挂 pod 要扩展**：因计算资源不足而无法在工作程序节点上安排 pod 时，该 pod 即被视为暂挂。集群自动缩放器检测到暂挂 pod 时，自动缩放器会跨专区均匀扩展工作程序节点，以满足工作负载资源请求。
*   **有未充分利用的工作程序节点要缩减**：缺省情况下，在 10 分钟或更长时间内，如果运行的工作程序节点所请求的计算资源占资源总量的比例低于 50%，并且可以将其工作负载重新安排到其他工作程序节点上，那么这些工作程序节点即被视为未充分利用。如果集群自动缩放器检测到未充分利用的工作程序节点，它会对工作程序节点进行缩减，一次缩减一个，以便您只拥有所需的计算资源。如果需要，可以对 10 分钟内的缺省缩减利用率阈值 50% 进行[定制](/docs/containers?topic=containers-ca#ca_chart_values)。

随着时间的推移，会定期进行扫描并执行扩展和缩减，根据工作程序节点数，此操作可能需要更长时间才能完成，例如 30 分钟。

集群自动缩放器调整工作程序节点数时，考虑的是您为部署定义的[资源请求 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/)，而不是实际工作程序节点使用量。如果 pod 和部署都未请求正确的资源量，那么您必须调整其配置文件。集群自动缩放器无法为您进行调整。另请记住，工作程序节点会将其中一些计算资源用于基本集群功能、缺省和定制[附加组件](/docs/containers?topic=containers-update#addons)以及[资源保留量](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node)。
{: note}

<br>
**扩展和缩减是如何运作的？**<br>
通常，集群自动缩放器会计算集群运行其工作负载时需要的工作程序节点数。扩展或缩减集群取决于诸多因素，其中包括以下因素。
*   设置的每个专区的工作程序节点最小大小和最大大小。
*   暂挂 pod 资源请求数以及与工作负载关联的特定元数据，例如，反亲缘关系、用于将 pod 仅放在特定机器类型上的标签，或者 [pod 中断预算 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/workloads/pods/disruptions/)。
*   集群自动缩放器管理的工作程序池可能会跨[多专区集群](/docs/containers?topic=containers-ha_clusters#multizone)中的多个专区。
*   设置的[定制 Helm chart 值](#ca_chart_values)，例如删除操作跳过使用本地存储器的工作程序节点。

有关更多信息，请参阅 Kubernetes 集群自动缩放器常见问题中的 [How does scale-up work? ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#how-does-scale-up-work) 和 [How does scale-down work? ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#how-does-scale-down-work)。

<br>

**可以更改扩展和缩减的运作方式吗？**<br>
可以定制设置或使用其他 Kubernetes 资源来影响扩展和缩减运作方式。
*   **扩展**：[定制集群自动缩放器 Helm chart 值](#ca_chart_values)，例如 `scanInterval`、`expander`、`skipNodes` 或 `maxNodeProvisionTime`。查看[过量供应工作程序节点](#ca_scaleup)的方式，以便可以扩展工作程序节点而避免工作程序池耗尽资源。此外，还可以[设置 Kubernetes pod 预算中断和 pod 优先级分界值](#scalable-practices-apps)，以影响扩展运作方式。
*   **缩减**：[定制集群自动缩放器 Helm chart 值](#ca_chart_values)，例如 `scaleDownUnneededTime`、`scaleDownDelayAfterAdd`、`scaleDownDelayAfterDelete` 或 `scaleDownUtilizationThreshold`。

<br>
**可以通过设置每个专区的最小大小，立即将集群扩展到该大小吗？**<br>
不能，设置 `minSize` 不会自动触发扩展。`minSize` 是阈值，以便集群自动缩放器不会将每个专区的工作程序节点数缩减到低于特定数量。如果集群的每个专区的工作程序节点尚未达到此数量，那么仅当您的工作负载资源请求需要更多资源时，集群自动缩放器才会进行扩展。例如，如果您有一个工作程序池，其中在三个专区中分别有一个工作程序节点（共计三个工作程序节点），并将每个专区的 `minSize` 设置为 `4`，那么集群自动缩放器不会立即为每个专区额外供应三个工作程序节点（共计 12 个工作程序节点），而是在有资源请求时才会触发扩展。如果创建工作负载来请求 15 个工作程序节点的资源，那么集群自动缩放器会扩展工作程序池以满足此请求。现在，`minSize` 意味着即使除去请求该数量的工作负载，集群自动缩放器也不会将每个专区的工作程序节点数缩减为低于四个。

<br>
**此行为与非集群自动缩放器管理的工作程序池有何不同？**<br>
[创建工作程序池](/docs/containers?topic=containers-add_workers#add_pool)时，您会指定每个专区的工作程序节点数。工作程序池将保持该数量的工作程序节点，直到您对其[调整大小](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_resize)或[重新均衡](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_rebalance)。工作程序池不会添加或除去工作程序节点。如果拥有的 pod 数多于可以安排的数量，那么多出的这些 pod 会保持暂挂状态，直到您调整工作程序池的大小。

对工作程序池启用集群自动缩放器后，可根据 pod 规范设置和资源请求，对工作程序节点进行扩展或缩减。您无需手动对工作程序池调整大小或重新均衡。

<br>
**我可以查看集群自动缩放器如何扩展和缩减的示例吗？**<br>
请参阅下图，以了解集群扩展和缩减的示例。

_图：自动扩展和缩减集群。_
![自动扩展和缩减集群 GIF](images/cluster-autoscaler-x3.gif){: gif}

1.  集群有 2 个工作程序池，池分布在 2 个专区中，共有 4 个工作程序节点。每个池在每个专区中有一个工作程序节点，但**工作程序池 A** 的机器类型为 `u2c.2x4`，**工作程序池 B** 的机器类型为 `b2c.4x16`。总计算资源约为 10 个核心（对于**工作程序池 A**，为 2 个核心 x 2 个工作程序节点，对于**工作程序池 B**，为 4 个核心 x 2 个工作程序节点）。集群当前运行的工作负载请求了这 10 个核心中的 6 个核心。运行集群、工作程序节点和任何附加组件（如集群自动缩放器）所需的[保留资源](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node)会占用每个工作程序节点上的额外计算资源。
2.  集群自动缩放器配置为管理这两个工作程序池，这两个池的每个专区的最小大小和最大大小如下：
    *  **工作程序池 A**：`minSize=1`，`maxSize=5`。
    *  **工作程序池 B**：`minSize=1`，`maxSize=2`。
3.  您安排的部署需要 14 个额外的应用程序 pod 副本，该应用程序请求每个副本 1 个 CPU 核心。在当前资源上可以部署一个 pod 副本，但其他 13 个 pod 副本处于暂挂状态。
4.  集群自动缩放器在这些约束内扩展工作程序节点，以支持这其他 13 个 pod 副本资源请求。
    *  **工作程序池 A**：以循环法在各专区中尽可能均匀地添加 7 个工作程序节点。工作程序节点将集群计算能力增加了约 14 个核心（2 个核心 x 7 个工作程序节点）。
    *  **工作程序池 B**：在各专区中均匀添加 2 个工作程序节点，以达到 `maxSize`，即每个专区 2 个工作程序节点。工作程序节点将集群容量增加了约 8 个核心（4 个核心 x 2 个工作程序节点）。
5.  具有单核心请求的 20 个 pod 分布在工作程序节点上，如下所示。由于工作程序节点具有资源保留量以及为了支持缺省集群功能而运行的 pod，因此工作负载的 pod 无法使用工作程序节点的所有可用计算资源。例如，虽然 `b2c.4x16` 工作程序节点具有 4 个核心，但只有请求每个 pod 最低 1 个核心的 3 个 pod 可以安排到工作程序节点上。
    <table summary="描述缩放集群中工作负载分布的表。">
    <caption>缩放集群中的工作负载分布。</caption>
    <thead>
    <tr>
      <th>工作程序池</th>
      <th>专区</th>
      <th>类型</th>
      <th>工作程序节点数</th>
      <th>pod 数</th>
    </tr>
    </thead>
    <tbody>
    <tr>
      <td>A</td>
      <td>dal10</td>
      <td>u2c.2x4</td>
      <td>4 个节点</td>
      <td>3 个 pod</td>
    </tr>
    <tr>
      <td>A</td>
      <td>dal12</td>
      <td>u2c.2x4</td>
      <td>5 个节点</td>
      <td>5 个 pod</td>
    </tr>
    <tr>
      <td>B</td>
      <td>dal10</td>
      <td>b2c.4x16</td>
      <td>2 个节点</td>
      <td>6 个 pod</td>
    </tr>
    <tr>
      <td>B</td>
      <td>dal12</td>
      <td>b2c.4x16</td>
      <td>2 个节点</td>
      <td>6 个 pod</td>
    </tr>
    </tbody>
    </table>
6.  您不再需要额外的工作负载，因此可删除部署。经过很短的一段时间后，集群自动缩放器会检测到集群不再需要其所有计算资源，随即会缩减工作程序节点，一次缩减一个。
7.  工作程序池已缩减。集群自动缩放器会定期扫描，以检查暂挂 pod 资源请求和未充分利用的工作程序节点，并相应地扩展或缩减工作程序池。

## 遵循可缩放部署实践
{: #scalable-practices}

使用以下工作程序节点策略以及工作负载部署策略，以最充分地利用集群自动缩放器。有关更多信息，请参阅 [Kubernetes 集群自动缩放器常见问题 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md)。
{: shortdesc}

通过若干测试工作负载[试用集群自动缩放器](#ca_helm)，以了解[扩展和缩减运作方式](#ca_about)、可能需要配置的[定制值](#ca_chart_values)以及可能需要的其他任何方面，如[过量供应](#ca_scaleup)工作程序节点或[限制应用程序](#ca_limit_pool)。然后，清除测试环境，并规划在集群自动缩放器全新安装中包含这些定制值和其他设置。

### 可以一次自动缩放多个工作程序池吗？
{: #scalable-practices-multiple}
可以，安装 Helm chart 后，可以在[配置映射](#ca_cm)中选择集群中哪些工作程序池进行自动缩放。每个集群只能运行一个 `ibm-iks-cluster-autoscaler` Helm chart。
{: shortdesc}

### 如何确保集群自动缩放器可响应应用程序需要的资源？
{: #scalable-practices-resrequests}

集群自动缩放器会根据工作负载[资源请求 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/) 来缩放集群。因此，请为所有部署指定[资源请求 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/)，因为集群自动缩放器是使用资源请求来计算运行工作负载所需的工作程序节点数。请记住，自动缩放基于的是工作负载配置请求的计算使用量，不考虑其他因素，例如机器成本。
{: shortdesc}

### 可以将工作程序池缩减到零 (0) 个节点吗？
{: #scalable-practices-zero}

不能，您不能将集群自动缩放器的 `minSize` 设置为 `0`。此外，除非在集群的每个专区中[禁用](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_configure)了所有公共应用程序负载均衡器 (ALB)，否则必须将每个专区的 `minSize` 更改为 `2` 个工作程序节点，以便可以分布 ALB pod 以实现高可用性。
{: shortdesc}

### 可以优化部署以进行自动缩放吗？
{: #scalable-practices-apps}

可以，您可以将多个 Kubernetes 功能添加到部署中，以调整集群自动缩放器如何考虑资源请求以进行缩放。
{: shortdesc}
*   使用 [pod 中断预算 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/workloads/pods/disruptions/) 以防止突然重新安排或删除 pod。
*   如果使用了 pod 优先级，那么可以[编辑优先级分界值 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#how-does-cluster-autoscaler-work-with-pod-priority-and-preemption)，以更改何种类型的优先级会触发扩展。缺省情况下，优先级分界值为零 (`0`)。

### 可以将污点和容忍度用于自动缩放的工作程序池吗？
{: #scalable-practices-taints}

由于无法在工作程序池级别应用污点，因此不要[污染工作程序节点](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/)，以避免产生意外结果。例如，部署有污点的工作程序节点不容忍的工作负载时，不会考虑对工作程序节点进行扩展，并且即使集群有足够的容量，也可能会订购更多工作程序节点。但是，如果有污点工作程序节点的资源利用率低于阈值（缺省情况下为 50%），并因此要考虑对其进行缩减，那么这些工作程序节点仍会被视为未充分利用。
{: shortdesc}

### 为什么自动缩放工作程序池不均衡?
{: #scalable-practices-unbalanced}

在扩展期间，集群自动缩放器会在各专区中均衡节点，允许正/负一个 (+/- 1) 工作程序节点的差异。暂挂工作负载可能无法请求足够的容量使每个专区均衡。在这种情况下，如果要手动均衡工作程序池，请[更新集群自动缩放器配置映射](#ca_cm)，以除去不均衡的工作程序池。然后，运行 `ibmcloud ks worker-pool-rebalance` [命令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_rebalance)，并将该工作程序池添加回集群自动缩放器配置映射。
{: shortdesc}


### 为什么无法对工作程序池调整大小或重新均衡？
{: #scalable-practices-resize}

对工作程序池启用了集群自动缩放器时，无法对工作程序池[调整大小](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_resize)或[重新均衡](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_rebalance)。您必须[编辑配置映射](#ca_cm)来更改工作程序池的最小大小或最大大小，或者禁用对该工作程序池的集群自动缩放。不要使用 `ibmcloud ks worker-rm` [命令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_rm)从工作程序池中除去单个工作程序节点，这可能会导致工作程序池不均衡。
{: shortdesc}

此外，如果未禁用工作程序池就卸载了 `ibm-iks-cluster-autoscaler` Helm chart，那么也无法手动调整工作程序池的大小。请重新安装 `ibm-iks-cluster-autoscaler` Helm chart，[编辑配置映射](#ca_cm)以禁用工作程序池，然后重试。

<br />


## 将集群自动缩放器 Helm chart 部署到集群
{: #ca_helm}

使用 Helm chart 安装 {{site.data.keyword.containerlong_notm}} 集群自动缩放器插件，以在集群中自动缩放工作程序池。
{: shortdesc}

**开始之前**：

1.  [安装必需的 CLI 和插件](/docs/cli?topic=cloud-cli-getting-started)：
    *  {{site.data.keyword.Bluemix_notm}} CLI (`ibmcloud`)
    *  {{site.data.keyword.containerlong_notm}} 插件 (`ibmcloud ks`)
    *  {{site.data.keyword.registrylong_notm}} 插件 (`ibmcloud cr`)
    *  Kubernetes (`kubectl`)
    *  Helm (`helm`)
2.  [创建标准集群](/docs/containers?topic=containers-clusters#clusters_ui)（运行的是 **Kubernetes V1.12 或更高版本**）。
3.   [登录到您的帐户。如果适用，请将相应的资源组设定为目标。为集群设置上下文。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
4.  确认您的 {{site.data.keyword.Bluemix_notm}} Identity and Access Management 凭证是否存储在集群中。集群自动缩放器会使用此私钥对凭证进行认证。如果缺少私钥，请[通过重置凭证来创建私钥](/docs/containers?topic=containers-cs_troubleshoot_storage#missing_permissions)。
    ```
    kubectl get secrets -n kube-system | grep storage-secret-store
    ```
    {: pre}
5.  集群自动缩放器只能缩放具有 `ibm-cloud.kubernetes.io/worker-pool-id` 标签的工作程序池。
    1.  检查工作程序池是否具有必需的标签。
        ```
        ibmcloud ks worker-pool-get --cluster <cluster_name_or_ID> --worker-pool <worker_pool_name_or_ID> | grep Labels
        ```
        {: pre}

        具有标签的工作程序池的输出示例：
        ```
        Labels:             ibm-cloud.kubernetes.io/worker-pool-id=a1aa111111b22b22cc3c3cc444444d44-4d555e5
        ```
        {: screen}
    2.  如果工作程序池没有必需的标签，请[添加新的工作程序池](/docs/containers?topic=containers-add_workers#add_pool)，并将此工作程序池与集群自动缩放器配合使用。


<br>
**要在集群中安装 `ibm-iks-cluster-autoscaler` 插件**，请执行以下操作：

1.  [遵循指示信息](/docs/containers?topic=containers-helm#public_helm_install)在本地计算机上安装 **Helm V2.11 或更高版本**客户机，然后在集群中使用服务帐户安装 Helm 服务器 (Tiller)。
2.  验证是否已使用服务帐户安装 Tiller。

    ```
    kubectl get serviceaccount -n kube-system tiller
    ```
    {: pre}

    输出示例：

    ```
    NAME                                 SECRETS   AGE
    tiller                               1         2m
    ```
    {: screen}
3.  添加和更新集群自动缩放器 Helm chart 所在的 Helm 存储库。
    ```
    helm repo add iks-charts https://icr.io/helm/iks-charts
    ```
    {: pre}
    ```
        helm repo update
        ```
    {: pre}
4.  在集群的 `kube-system` 名称空间中安装集群自动缩放器 Helm chart。

    在安装期间，您可以进一步[定制集群自动缩放器设置](#ca_chart_values)，例如在扩展或缩减工作程序节点之前等待的时间量。
    {: tip}

    ```
    helm install iks-charts/ibm-iks-cluster-autoscaler --namespace kube-system --name ibm-iks-cluster-autoscaler
    ```
    {: pre}

    输出示例：
    ```
    NAME:   ibm-iks-cluster-autoscaler
    LAST DEPLOYED: Thu Nov 29 13:43:46 2018
    NAMESPACE: kube-system
    STATUS: DEPLOYED

    RESOURCES:
    ==> v1/ClusterRole
    NAME                       AGE
    ibm-iks-cluster-autoscaler  1s

    ==> v1/Pod(related)

    NAME                                        READY  STATUS             RESTARTS  AGE
    ibm-iks-cluster-autoscaler-67c8f87b96-qbb6c  0/1    ContainerCreating  0         1s

    ==> v1/ConfigMap

    NAME              AGE
    iks-ca-configmap  1s

    ==> v1/ClusterRoleBinding
    ibm-iks-cluster-autoscaler  1s

    ==> v1/Role
    ibm-iks-cluster-autoscaler  1s

    ==> v1/RoleBinding
    ibm-iks-cluster-autoscaler  1s

    ==> v1/Service
    ibm-iks-cluster-autoscaler  1s

    ==> v1beta1/Deployment
    ibm-iks-cluster-autoscaler  1s

    ==> v1/ServiceAccount
    ibm-iks-cluster-autoscaler  1s

    NOTES:
    Thank you for installing: ibm-iks-cluster-autoscaler. Your release is named: ibm-iks-cluster-autoscaler

    For more information about using the cluster autoscaler, refer to the chart README.md file.
    ```
    {: screen}

5.  验证安装是否成功。

    1.  检查集群自动缩放器 pod 是否处于 **Running** 状态。
        ```
        kubectl get pods --namespace=kube-system | grep ibm-iks-cluster-autoscaler
        ```
        {: pre}
        输出示例：
        ```
        ibm-iks-cluster-autoscaler-8497bfc968-dbn7w   1/1       Running   0          9m
        ```
        {: screen}
    2.  检查集群自动缩放器服务是否已创建。
        ```
        kubectl get service --namespace=kube-system | grep ibm-iks-cluster-autoscaler
        ```
        {: pre}
        输出示例：
        ```
        ibm-iks-cluster-autoscaler   ClusterIP   172.21.xxx.xx    <none>        8085/TCP        9m
        ```
        {: screen}

6.  对要供应集群自动缩放器的每个集群，重复这些步骤。

7.  要开始缩放工作程序池，请参阅[更新集群自动缩放器配置](#ca_cm)。

<br />


## 更新集群自动缩放器配置映射以启用缩放
{: #ca_cm}

更新集群自动缩放器配置映射，以支持根据设置的最小值和最大值，自动缩放工作程序池中的工作程序节点。
{: shortdesc}

编辑配置映射以启用工作程序池后，集群自动缩放器即会根据工作负载请求来缩放集群。因此，您不能对工作程序池[调整大小](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_resize)或[重新均衡](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_rebalance)。随着时间的推移，会定期进行扫描并执行扩展和缩减，根据工作程序节点数，此操作可能需要更长时间才能完成，例如 30 分钟。如果日后要[除去集群自动缩放器](#ca_rm)，必须先在配置映射中禁用每个工作程序池。
{: note}

**开始之前**：
*  [安装 `ibm-iks-cluster-autoscaler` 插件](#ca_helm)。
*  [登录到您的帐户。如果适用，请将相应的资源组设定为目标。为集群设置上下文。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

**要更新集群自动缩放器配置映射和值**，请执行以下操作：

1.  编辑集群自动缩放器配置映射 YAML 文件。
    ```
    kubectl edit cm iks-ca-configmap -n kube-system -o yaml
    ```
    {: pre}
        输出示例：
        ```
    apiVersion: v1
    data:
      workerPoolsConfig.json: |
        [
         {"name": "default","minSize": 1,"maxSize": 2,"enabled":false}
        ]
    kind: ConfigMap
    metadata:
      creationTimestamp: 2018-11-29T18:43:46Z
      name: iks-ca-configmap
      namespace: kube-system
      resourceVersion: "2227854"
      selfLink: /api/v1/namespaces/kube-system/configmaps/iks-ca-configmap
      uid: b45d047b-f406-11e8-b7f0-82ddffc6e65e
    ```
    {: screen}
2.  使用参数编辑配置映射，以定义集群自动缩放器如何缩放集群工作程序池。**注：**除非在标准集群的每个专区中[禁用](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_configure)了所有公共应用程序负载均衡器 (ALB)，否则必须将每个专区的 `minSize` 更改为 `2`，以便可以分布 ALB pod 以实现高可用性。

    <table>
    <caption>集群自动缩放器配置映射参数</caption>
    <thead>
    <th id="parameter-with-default">具有缺省值的参数</th>
    <th id="parameter-with-description">描述</th>
    </thead>
    <tbody>
    <tr>
    <td id="parameter-name" headers="parameter-with-default">`"name": "default"`</td>
    <td headers="parameter-name parameter-with-description">将 `"default"` 替换为要缩放的工作程序池的名称或标识。要列出工作程序池，请运行 `ibmcloud ks worker-pools --cluster <cluster_name_or_ID>`。<br><br>
    要管理多个工作程序池，请将该 JSON 行复制到逗号分隔行，如下所示。<pre class="codeblock">[
     {"name": "default","minSize": 1,"maxSize": 2,"enabled":false},
     {"name": "Pool2","minSize": 2,"maxSize": 5,"enabled":true}
    ]</pre><br><br>
    **注**：集群自动缩放器只能缩放具有 `ibm-cloud.kubernetes.io/worker-pool-id` 标签的工作程序池。要检查工作程序池是否具有必需的标签，请运行 `ibmcloud ks worker-pool-get --cluster <cluster_name_or_ID> --worker-pool <worker_pool_name_or_ID> | grep Labels`。如果工作程序池没有必需的标签，请[添加新的工作程序池](/docs/containers?topic=containers-add_workers#add_pool)，并将此工作程序池与集群自动缩放器配合使用。</td>
    </tr>
    <tr>
    <td id="parameter-minsize" headers="parameter-with-default">`"minSize": 1`</td>
    <td headers="parameter-minsize parameter-with-description">指定集群自动缩放器可将工作程序池缩减到的每个专区最小工作程序节点数。值必须等于或大于 `2`，以便可以分布 ALB pod 以实现高可用性。如果在标准集群的每个专区中[禁用](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_configure)了所有公共 ALB，那么可以将值设置为 `1`。
    <p class="note">设置 `minSize` 不会自动触发扩展。`minSize` 是阈值，以便集群自动缩放器不会将每个专区的工作程序节点数缩减到低于特定数量。如果集群的每个专区的工作程序节点尚未达到此数量，那么仅当您的工作负载资源请求需要更多资源时，集群自动缩放器才会进行扩展。例如，如果您有一个工作程序池，其中在三个专区中分别有一个工作程序节点（共计三个工作程序节点），并将每个专区的 `minSize` 设置为 `4`，那么集群自动缩放器不会立即为每个专区额外供应三个工作程序节点（共计 12 个工作程序节点），而是在有资源请求时才会触发扩展。如果创建工作负载来请求 15 个工作程序节点的资源，那么集群自动缩放器会扩展工作程序池以满足此请求。现在，`minSize` 意味着即使除去请求该数量的工作负载，集群自动缩放器也不会将每个专区的工作程序节点数缩减为低于四个。有关更多信息，请参阅 [Kubernetes 文档 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#when-does-cluster-autoscaler-change-the-size-of-a-cluster)。</p></td>
    </tr>
    <tr>
    <td id="parameter-maxsize" headers="parameter-with-default">`"maxSize": 2`</td>
    <td headers="parameter-maxsize parameter-with-description">指定集群自动缩放器可将工作程序池扩展到的每个专区最大工作程序节点数。值必须等于或大于为 `minSize` 设置的值。</td>
    </tr>
    <tr>
    <td id="parameter-enabled" headers="parameter-with-default">`"enabled": false`</td>
    <td headers="parameter-enabled parameter-with-description">将值设置为 `true`，表示由集群自动缩放器来管理工作程序池的缩放。将值设置为 `false` 可阻止集群自动缩放器对工作程序池进行缩放。<br><br>
    如果日后要[除去集群自动缩放器](#ca_rm)，必须先在配置映射中禁用每个工作程序池。</td>
    </tr>
    </tbody>
    </table>
3.  保存配置文件。
4.  获取集群自动缩放器 pod。
    ```
    kubectl get pods -n kube-system
    ```
    {: pre}
5.  查看集群自动缩放器 pod 的 **`Events`** 部分中是否有 **`ConfigUpdated`** 事件，以验证配置映射是否已成功更新。配置映射的事件消息采用以下格式：`minSize:maxSize:PoolName:<SUCCESS|FAILED>:error message`。

    ```
    kubectl describe pod -n kube-system <cluster_autoscaler_pod>
    ```
    {: pre}

    输出示例：
    ```
		Name:               ibm-iks-cluster-autoscaler-857c4d9d54-gwvc6
		Namespace:          kube-system
		...
		Events:
		Type     Reason         Age   From                                        Message
		----     ------         ----  ----                                        -------

		Normal  ConfigUpdated  3m    ibm-iks-cluster-autoscaler-857c4d9d54-gwvc6  {"1:3:default":"SUCCESS:"}
    ```
    {: screen}

## 定制集群自动缩放器 Helm chart 配置值
{: #ca_chart_values}

定制集群自动缩放器设置，例如在扩展或缩减工作程序节点之前等待的时间量。
{: shortdesc}

**开始之前**：
*  [登录到您的帐户。如果适用，请将相应的资源组设定为目标。为集群设置上下文。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
*  [安装 `ibm-iks-cluster-autoscaler` 插件](#ca_helm)。

**要更新集群自动缩放器值，请执行以下操作：**

1.  查看集群自动缩放器 Helm chart 配置值。集群自动缩放器随附缺省设置。但是，您可能希望更改某些值，例如根据集群工作负载的更改频率，更改缩减或扫描时间间隔。
    ```
    helm get values ibm-iks-cluster-autoscaler -a
    ```
    {: pre}

    输出示例：
    ```
    expander: least-waste
    image:
      pullPolicy: Always
      repository: icr.io/iks-charts/ibm-iks-cluster-autoscaler
      tag: dev1
    maxNodeProvisionTime: 120m
    resources:
      limits:
        cpu: 300m
        memory: 300Mi
      requests:
        cpu: 100m
        memory: 100Mi
    scaleDownDelayAfterAdd: 10m
    scaleDownDelayAfterDelete: 10m
    scaleDownUtilizationThreshold: 0.5
    scaleDownUnneededTime: 10m
    scanInterval: 1m
    skipNodes:
      withLocalStorage: true
      withSystemPods: true
    ```
    {: codeblock}

    <table>
    <caption>集群自动缩放器配置值</caption>
    <thead>
    <th>参数</th>
    <th>描述</th>
    <th>缺省值</th>
    </thead>
    <tbody>
    <tr>
    <td>`api_route` 参数</td>
    <td>为集群所在的区域设置 [{{site.data.keyword.containerlong_notm}} API 端点](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cli_api)。</td>
    <td>无缺省值；使用集群所在的目标区域。</td>
    </tr>
    <tr>
    <td>`expander` 参数</td>
    <td>指定集群自动缩放器如何确定哪个工作程序池要进行缩放（如果有多个工作程序池）。可能的值为：
    <ul><li>`random`：在 `most-pods` 和 `least-waste` 之间随机选择。</li>
    <li>`most-pods`：选择在扩展时能够安排最多 pod 的工作程序池。如果要使用 `nodeSelector` 来确保 pod 位于特定工作程序节点上，请使用此方法。</li>
    <li>`least-waste`：选择扩展后未使用 CPU 最少的工作程序池。如果扩展后两个工作程序池使用的 CPU 资源量相同，那么将选择具有最少未使用内存的工作程序池。</li></ul></td>
    <td>random</td>
    </tr>
    <tr>
    <td>`image.repository` 参数</td>
    <td>指定要使用的集群自动缩放器 Docker 映像。</td>
    <td>`icr.io/iks-charts/ibm-iks-cluster-autoscaler`</td>
    </tr>
    <tr>
    <td>`image.pullPolicy` 参数</td>
    <td>指定何时拉取 Docker 映像。可能的值为：
    <ul><li>`Always`：每次启动 pod 时都拉取映像。</li>
    <li>`IfNotPresent`：仅当映像在本地尚不存在时，才拉取映像。</li>
    <li>`Never`：假定映像在本地存在，因而从不拉取映像。</li></ul></td>
    <td>Always</td>
    </tr>
    <tr>
    <td>`maxNodeProvisionTime` 参数</td>
    <td>设置工作程序节点在开始供应后，最长经过多长时间（以分钟为单位），集群自动缩放器会取消扩展请求。</td>
    <td>`120m`</td>
    </tr>
    <tr>
    <td>`resources.limits.cpu` 参数</td>
    <td>设置 `ibm-iks-cluster-autoscaler` pod 可以使用的最大工作程序节点 CPU 量。</td>
    <td>`300m`</td>
    </tr>
    <tr>
    <td>`resources.limits.memory` 参数</td>
    <td>设置 `ibm-iks-cluster-autoscaler` pod 可以使用的最大工作程序节点内存量。</td>
    <td>`300Mi`</td>
    </tr>
    <tr>
    <td>`resources.requests.cpu` 参数</td>
    <td>设置 `ibm-iks-cluster-autoscaler` pod 开始使用的最小工作程序节点 CPU 量。</td>
    <td>`100m`</td>
    </tr>
    <tr>
    <td>`resources.requests.memory` 参数</td>
    <td>设置 `ibm-iks-cluster-autoscaler` pod 开始使用的最小工作程序节点内存量。</td>
    <td>`100Mi`</td>
    </tr>
    <tr>
    <td>`scaleDownUnneededTime` 参数</td>
    <td>设置工作程序节点变为不必要后，必须经过多长时间（以分钟为单位）才能缩减该工作程序节点。</td>
    <td>`10m`</td>
    </tr>
    <tr>
    <td>`scaleDownDelayAfterAdd` 和 `scaleDownDelayAfterDelete` 参数</td>
    <td>设置集群自动缩放器在扩展 (`add`) 或缩减 ( `delete`) 后再次启动缩放操作之前要等待的时间量（以分钟为单位）。</td>
    <td>`10m`</td>
    </tr>
    <tr>
    <td>`scaleDownUtilizationThreshold` 参数</td>
    <td>设置工作程序节点利用率阈值。如果工作程序节点利用率低于阈值，那么会考虑缩减该工作程序节点。工作程序节点利用率的计算方法是，工作程序节点上运行的所有 pod 请求的 CPU 和内存资源之和除以工作程序节点资源容量。</td>
    <td>`0.5`</td>
    </tr>
    <tr>
    <td>`scanInterval` 参数</td>
    <td>设置集群自动缩放器对会触发扩展或缩减的工作负载使用情况进行扫描的频率（以分钟为单位）。</td>
    <td>`1m`</td>
    </tr>
    <tr>
    <td>`skipNodes.withLocalStorage` 参数</td>
    <td>设置为 `true` 时，具有将数据保存到本地存储器的 pod 的工作程序节点不会缩减。</td>
    <td>`true`</td>
    </tr>
    <tr>
    <td>`skipNodes.withSystemPods` 参数</td>
    <td>设置为 `true` 时，不会缩减具有 `kube-system` pod 的工作程序节点。不要将值设置为 `false`，因为缩减 `kube-system` pod 可能会产生意外结果。</td>
    <td>`true`</td>
    </tr>
    </tbody>
    </table>
2.  要更改任何集群自动缩放器配置值，请使用新值更新 Helm chart。
    包含 `--recreate-pods` 标志，以便重新创建任何现有集群自动缩放器 pod，从而获取定制设置更改。
    ```
    helm upgrade --set scanInterval=2m ibm-iks-cluster-autoscaler iks-charts/ibm-iks-cluster-autoscaler -i --recreate-pods
    ```
    {: pre}

    要将图表重置为缺省值，请运行以下命令：
    ```
    helm upgrade --reset-values ibm-iks-cluster-autoscaler iks-charts/ibm-iks-cluster-autoscaler --recreate-pods
    ```
    {: pre}
3.  要验证更改，请再次查看 Helm chart 值。
    ```
    helm get values ibm-iks-cluster-autoscaler -a
    ```
    {: pre}


## 将应用程序限制为仅在特定自动缩放的工作程序池上运行
{: #ca_limit_pool}

要将 pod 限制为部署到集群自动缩放器管理的特定工作程序池，请使用标签以及 `nodeSelector` 或 `nodeAffinity`。通过 `nodeAffinity`，可以对安排行为的运作方式有更大的控制力，以将 pod 与工作程序节点相匹配。有关将 pod 分配给工作程序节点的更多信息，请参阅 [Kubernetes 文档 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/)。
{: shortdesc}

**开始之前**：
*  [安装 `ibm-iks-cluster-autoscaler` 插件](#ca_helm)。
*  [登录到您的帐户。如果适用，请将相应的资源组设定为目标。为集群设置上下文。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

**要将 pod 限制为在特定自动缩放的工作程序池上运行，请执行以下操作：**

1.  通过要使用的标签来创建工作程序池。
    例如，标签可能为 `app: nginx`。
    ```
    ibmcloud ks worker-pool-create --name <name> --cluster <cluster_name_or_ID> --machine-type <machine_type> --size-per-zone <number_of_worker_nodes> --labels <key>=<value>
    ```
    {: pre}
2.  [将工作程序池添加到集群自动缩放器配置](#ca_cm)。
3.  在 pod spec 模板中，将 `nodeSelector` 或 `nodeAffinity` 与工作程序池中使用的标签相匹配。

    `nodeSelector` 的示例：
    ```
    ...
    spec:
      containers:
      - name: nginx
        image: nginx
        imagePullPolicy: IfNotPresent
      nodeSelector:
        app: nginx
    ...
    ```
    {: codeblock}

    `nodeAffinity` 的示例：
    ```
    spec:
      containers:
      - name: nginx
        image: nginx
        imagePullPolicy: IfNotPresent
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: app
                       operator: In
                       values:
                - nginx
    ```
    {: codeblock}
4.  部署 pod。由于标签匹配，因此 pod 会安排到标记有该标签的工作程序池中的某个工作程序节点上。
    ```
      kubectl apply -f pod.yaml
      ```
    {: pre}

<br />


## 在工作程序池资源不足之前扩展工作程序节点
{: #ca_scaleup}

如[了解集群自动缩放器的运作方式](#ca_about)主题和 [Kubernetes 集群自动缩放器常见问题 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md) 中所述，集群自动缩放器可根据工作程序池的可用资源来扩展工作程序池，以响应工作负载请求的资源。但是，您可能希望集群自动缩放器在工作程序池资源不足之前就能扩展工作程序节点。在这种情况下，工作负载就无需等待供应工作程序节点，因为工作程序池已扩展来满足资源请求。
{: shortdesc}

集群自动缩放器不支持对工作程序池提早缩放（过量供应）。但是，可以配置其他 Kubernetes 资源来使用集群自动缩放器，从而实现提早缩放。

<dl>
  <dt><strong>暂停 pod</strong></dt>
  <dd>可以创建部署，用于通过特定资源请求在 pod 中部署[暂停容器 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://stackoverflow.com/questions/48651269/what-are-the-pause-containers)，并为该部署分配低 pod 优先级。当优先级较高的工作负载需要这些资源时，会抢占暂停 pod，使其变成暂挂 pod。此事件会触发集群自动缩放器进行扩展。<br><br>有关设置暂停 pod 部署的更多信息，请参阅 [Kubernetes 常见问题 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#how-can-i-configure-overprovisioning-with-cluster-autoscaler)。您可以使用[此示例过量供应配置文件 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/IBM-Cloud/kube-samples/blob/master/ibm-ks-cluster-autoscaler/overprovisioning-autoscaler.yaml) 来创建优先级类、服务帐户和部署。<p class="note">如果使用此方法，请确保了解 [pod 优先级](/docs/containers?topic=containers-pod_priority#pod_priority)的运作方式，以及如何为部署设置 pod 优先级。例如，如果暂停 pod 没有足够的资源可用于更高优先级的 pod，那么不会抢占该 pod。优先级较高的工作负载会保持暂挂状态，因此会触发集群自动缩放器进行扩展。但是在这种情况下，扩展操作并不属于提早扩展，因为您要运行的工作负载由于资源不足而无法安排。</p></dd>

  <dt><strong>水平 pod 自动缩放 (HPA)</strong></dt>
  <dd>因为水平 pod 自动缩放基于的是 pod 的平均 CPU 使用率，所以在工作程序池耗尽资源之前，就会达到您设置的 CPU 使用率限制。这时会请求更多 pod，从而触发集群自动缩放器扩展工作程序池。<br><br>有关设置 HPA 的更多信息，请参阅 [Kubernetes 文档 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale-walkthrough/)。</dd>
</dl>

<br />


## 更新集群自动缩放器 Helm chart
{: #ca_helm_up}

可以将现有集群自动缩放器 Helm chart 更新到最新版本。要检查当前 Helm chart 版本，请运行 `helm ls | grep cluster-autoscaler`。
{: shortdesc}

要从 Helm chart V1.0.2 或更低版本更新到最新版本？请[遵循以下指示信息](#ca_helm_up_102)进行操作。
{: note}

开始之前：[登录到您的帐户。如果适用，请将相应的资源组设定为目标。为集群设置上下文。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  更新 Helm 存储库以在此存储库中检索所有最新版本的 Helm chart。
    ```
        helm repo update
        ```
    {: pre}

2.  可选：将最新 Helm chart 下载到本地计算机。然后，解压缩包并查看 `release.md` 文件，以了解最新的发行版信息。
    ```
    helm fetch iks-charts/ibm-iks-cluster-autoscaler
    ```
    {: pre}

3.  查找已安装在集群中的集群自动缩放器 Helm chart 的名称。
    ```
    helm ls | grep cluster-autoscaler
    ```
    {: pre}

    输出示例：
    ```
    myhelmchart 	1       	Mon Jan  7 14:47:44 2019	DEPLOYED	ibm-ks-cluster-autoscaler-1.0.0  	kube-system
    ```
    {: screen}

4.  将集群自动缩放器 Helm chart 更新到最新版本。
    ```
    helm upgrade --force --recreate-pods <helm_chart_name>  iks-charts/ibm-iks-cluster-autoscaler
    ```
    {: pre}

5.  验证[集群自动缩放器配置映射](#ca_cm)的 `workerPoolsConfig.json` 部分是否已针对要缩放的工作程序池设置为 `"enabled": true`。
    ```
    kubectl describe cm iks-ca-configmap -n kube-system
    ```
    {: pre}

    输出示例：
    ```
    Name:         iks-ca-configmap
    Namespace:    kube-system
    Labels:       <none>
    Annotations:  kubectl.kubernetes.io/last-applied-configuration={"apiVersion":"v1","data":{"workerPoolsConfig.json":"[\n {\"name\": \"docs\",\"minSize\": 1,\"maxSize\": 3,\"enabled\":true}\n]\n"},"kind":"ConfigMap",...

    Data
    ====
    workerPoolsConfig.json:
    ----
    [
     {"name": "docs","minSize": 1,"maxSize": 3,"enabled":true}
    ]

    Events:  <none>
    ```
    {: screen}

### 从 Helm chart V1.0.2 或更低版本更新到最新版本
{: #ca_helm_up_102}

集群自动缩放器的最新 Helm chart 版本需要完全除去先前安装的集群自动缩放器 Helm chart 版本。如果已安装 Helm chart V1.0.2 或更低版本，请先卸载该版本，然后再安装集群自动缩放器的最新 Helm chart。
{: shortdesc}

开始之前：[登录到您的帐户。如果适用，请将相应的资源组设定为目标。为集群设置上下文。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  获取集群自动缩放器配置映射。
    ```
    kubectl get cm iks-ca-configmap -n kube-system -o yaml > iks-ca-configmap.yaml
    ```
    {: pre}
2.  在配置映射中，通过将 `"enabled"` 值设置为 `false` 以除去所有工作程序池。
    ```
    kubectl edit cm iks-ca-configmap -n kube-system
    ```
    {: pre}
3.  如果将定制设置应用于 Helm chart，请记下这些定制设置。
    ```
    helm get values ibm-ks-cluster-autoscaler -a
    ```
    {: pre}
4.  卸载当前 Helm chart。
    ```
    helm delete --purge ibm-ks-cluster-autoscaler
    ```
    {: pre}
5.  更新 Helm chart 存储库以获取最新的集群自动缩放器 Helm chart 版本。
    ```
        helm repo update
        ```
    {: pre}
6.  安装最新的集群自动缩放器 Helm chart。应用先前与 `--set` 标志一起使用的任何定制设置，例如 `scanInterval=2m`。
    ```
    helm install  iks-charts/ibm-iks-cluster-autoscaler --namespace kube-system --name ibm-iks-cluster-autoscaler [--set <custom_settings>]
    ```
    {: pre}
7.  应用先前检索到的集群自动缩放器配置映射，以启用对工作程序池的自动缩放。
    ```
    kubectl apply -f iks-ca-configmap.yaml
    ```
    {: pre}
8.  获取集群自动缩放器 pod。
    ```
    kubectl get pods -n kube-system
    ```
    {: pre}
9.  查看集群自动缩放器 pod 的 **`Events`** 部分并查找 **`ConfigUpdated`** 事件，以验证配置映射是否已成功更新。配置映射的事件消息采用以下格式：`minSize:maxSize:PoolName:<SUCCESS|FAILED>:error message`。
    ```
    kubectl describe pod -n kube-system <cluster_autoscaler_pod>
    ```
    {: pre}

    输出示例：
    ```
		Name:               ibm-iks-cluster-autoscaler-857c4d9d54-gwvc6
		Namespace:          kube-system
		...
		Events:
		Type     Reason         Age   From                                        Message
		----     ------         ----  ----                                        -------

		Normal  ConfigUpdated  3m    ibm-iks-cluster-autoscaler-857c4d9d54-gwvc6  {"1:3:default":"SUCCESS:"}
    ```
    {: screen}

<br />


## 除去集群自动缩放器
{: #ca_rm}

如果不想自动缩放工作程序池，那么可以卸载集群自动缩放器 Helm chart。除去后，您必须手动对工作程序池[调整大小](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_resize)或[重新均衡](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_rebalance)。
{: shortdesc}

开始之前：[登录到您的帐户。如果适用，请将相应的资源组设定为目标。为集群设置上下文。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  在[集群自动缩放器配置映射](#ca_cm)中，通过将 `"enabled"` 值设置为 `false` 来除去工作程序池。
    ```
    kubectl edit cm iks-ca-configmap -n kube-system
    ```
    {: pre}
        输出示例：
        ```
    apiVersion: v1
    data:
      workerPoolsConfig.json: |
        [
         {"name": "default","minSize": 1,"maxSize": 2,"enabled":false},
         {"name":"mypool","minSize":1,"maxSize":5,"enabled":false}
        ]
    kind: ConfigMap
    ...
    ```
2.  列出现有 Helm chart，并记下集群自动缩放器的名称。
    ```
    helm ls
    ```
    {: pre}
3.  从集群中除去现有 Helm chart。
    ```
    helm delete --purge <ibm-iks-cluster-autoscaler_name>
    ```
    {: pre}
