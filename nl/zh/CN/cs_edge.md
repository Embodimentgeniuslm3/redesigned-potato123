---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-11"

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


# 将网络流量限于边缘工作程序节点
{: #edge}

边缘工作程序节点通过在 {{site.data.keyword.containerlong}} 中减少允许外部访问的工作程序节点，并隔离联网工作负载，可以提高 Kubernetes 集群的安全性。
{:shortdesc}

当这些工作程序节点标记为仅用于联网时，其他工作负载无法使用工作程序节点的 CPU 或内存，也不会干扰联网。


如果您有多专区集群，并要将网络流量限制为流至边缘工作程序节点，那么每个专区中必须至少启用 2 个边缘工作程序节点，才可实现负载均衡器或 Ingress pod 的高可用性。创建边缘节点工作程序池，此池跨集群中的所有专区，并且每个专区至少有 2 个工作程序节点。
{: tip}

## 将工作程序节点标注为边缘节点
{: #edge_nodes}

将 `dedicated=edge` 标签添加到集群中每个公用或专用 VLAN 上的两个或更多工作程序节点，以确保网络负载均衡器 (NLB) 和 Ingress 应用程序负载均衡器 (ALB) 仅部署到这些工作程序节点。
{:shortdesc}

在 Kubernetes V1.14 和更高版本中，公共和专用 NLB 和 ALB 都可以部署到边缘工作程序节点。在 Kubernetes 1.13 和更低版本中，公共和专用 ALB 以及公共 NLB 可以部署到边缘节点，但专用 NLB 只能部署到集群中的非边缘工作程序节点。
{: note}

开始之前：

* 确保您具有以下 [{{site.data.keyword.Bluemix_notm}} IAM 角色](/docs/containers?topic=containers-users#platform)：
  * 对集群的任何平台角色
  * 对所有名称空间的**写入者**或**管理者**服务角色
* [登录到您的帐户。如果适用，请将相应的资源组设定为目标。为集群设置上下文。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

</br>要将工作程序节点标注为边缘节点，请执行以下操作：

1. [创建新的工作程序池](/docs/containers?topic=containers-add_workers#add_pool)，此池跨集群中的所有专区，并且每个专区至少有 2 个工作程序。在 `ibmcloud ks worker-pool-create` 命令中，包含 `--labels dedicated=edge` 标志以标注池中的所有工作程序节点。此池中的所有工作程序节点（包括日后添加的任何工作程序节点）都会标注为边缘节点。
  <p class="tip">如果要使用现有工作程序池，该池必须跨集群中的所有专区，并且每个专区至少有 2 个工作程序。可以使用 [PATCH 工作程序池 API](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/PatchWorkerPool) 通过 `dedicated=edge` 来标注工作程序池。在请求的主体中，传递以下 JSON。在使用 `dedicated=edge` 标记工作程序池后，所有现有及后续工作程序节点都将获得此标签，并且 Ingress 和负载均衡器会部署到边缘工作程序节点。
      <pre class="screen">
      {
        "labels": {"dedicated":"edge"},
        "state": "labels"
      }</pre></p>

2. 验证工作程序池和工作程序节点是否具有 `dedicated=edge` 标签。
  * 要检查工作程序池，请运行以下命令：
    ```
    ibmcloud ks worker-pool-get --cluster <cluster_name_or_ID> --worker-pool <worker_pool_name_or_ID>
    ```
    {: pre}

  * 要检查单个工作程序节点，请查看以下命令输出中的 **Labels** 字段。
    ```
    kubectl describe node <worker_node_private_IP>
    ```
    {: pre}

3. 检索集群中的所有现有 NLB 和 ALB。
  ```
  kubectl get services --all-namespaces | grep LoadBalancer
  ```
  {: pre}

  记下输出中的每个 LoadBalancer 服务的 **Namespace** 和 **Name**。例如，在以下输出中，有四个 LoadBalancer 服务：`default` 名称空间中的一个公共 NLB 以及 `kube-system` 名称空间中的一个专用 ALB 和两个公共 ALB。
  ```
  NAMESPACE     NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP     PORT(S)                                     AGE
  default       webserver-lb                                     LoadBalancer   172.21.190.18    169.46.17.2     80:30597/TCP                                10m
  kube-system   private-crdf253b6025d64944ab99ed63bb4567b6-alb1  LoadBalancer   172.21.158.78    10.185.94.150   80:31015/TCP,443:31401/TCP,9443:32352/TCP   25d
  kube-system   public-crdf253b6025d64944ab99ed63bb4567b6-alb1   LoadBalancer   172.21.84.248    169.48.228.78   80:30286/TCP,443:31363/TCP                  1h
  kube-system   public-crdf253b6025d64944ab99ed63bb4567b6-alb2   LoadBalancer   172.21.229.73    169.46.17.6     80:31104/TCP,443:31138/TCP                  57m
  ```
  {: screen}

4. 使用上一步的输出，对每个 NLB 和 ALB 运行以下命令。此命令会将 NLB 或 ALB 重新部署到边缘工作程序节点。

  如果集群运行的是 Kubernetes 1.14 或更高版本，那么可以将公共和专用 NLB 和 ALB 部署到边缘工作程序节点。在 Kubernetes 1.13 和更低版本中，只有公共和专用 ALB 以及公共 NLB 可以部署到边缘节点，因此不要重新部署专用 NLB 服务。
  {: note}

  ```
  kubectl get service -n <namespace> <service_name> -o yaml | kubectl apply -f -
  ```
  {: pre}

  输出示例：
  ```
  service "webserver-lb" configured
  ```
  {: screen}

5. 要验证是否将联网工作负载限制为只能流至边缘节点，请确认是否将 NLB 和 ALB pod 安排到了边缘节点上，而未安排到非边缘节点上。

  * NLB pod：
    1. 确认是否将 NLB pod 部署到了边缘节点。搜索步骤 3 的输出中列出的 LoadBalancer 服务的外部 IP 地址。将句点 (`.`) 替换为连字符 (`-`)。具有外部 IP 地址 `169.46.17.2` 的 `webserver-lb` NLB 的示例：
      ```
      kubectl describe nodes -l dedicated=edge | grep "169-46-17-2"
      ```
      {: pre}

      输出示例：
        ```
      ibm-system                 ibm-cloud-provider-ip-169-46-17-2-76fcb4965d-wz6dg                 5m (0%)       0 (0%)      10Mi (0%)        0 (0%)
      ibm-system                 ibm-cloud-provider-ip-169-46-17-2-76fcb4965d-2z64r                 5m (0%)       0 (0%)      10Mi (0%)        0 (0%)
      ```
      {: screen}
    2. 确认是否没有任何 NLB pod 部署到非边缘节点。具有外部 IP 地址 `169.46.17.2` 的 `webserver-lb` NLB 的示例：
      ```
      kubectl describe nodes -l dedicated!=edge | grep "169-46-17-2"
      ```
      {: pre}
      * 如果 NLB pod 正确部署到了边缘节点，那么不会返回任何 NLB pod。这说明 NLB 已成功地重新安排到仅边缘工作程序节点上。
      * 如果返回了 NLB pod，请继续执行下一步。

  * ALB pod：
    1. 确认是否将所有 ALB pod 都部署到了边缘节点。搜索关键字 `alb`。每个公共和专用 ALB 都有两个 pod。示例：
    ```
      kubectl describe nodes -l dedicated=edge | grep alb
      ```
      {: pre}

      具有两个专区的集群的示例输出，其中集群启用了一个缺省专用 ALB 和两个缺省公共 ALB：
      ```
      kube-system                private-crdf253b6025d64944ab99ed63bb4567b6-alb1-d5dd478db-27pv4    0 (0%)        0 (0%)      0 (0%)           0 (0%)
      kube-system                private-crdf253b6025d64944ab99ed63bb4567b6-alb1-d5dd478db-7p9q6    0 (0%)        0 (0%)      0 (0%)           0 (0%)
      kube-system                public-crdf253b6025d64944ab99ed63bb4567b6-alb1-5ff8cdff89-s77z6    0 (0%)        0 (0%)      0 (0%)           0 (0%)
      kube-system                public-crdf253b6025d64944ab99ed63bb4567b6-alb1-5ff8cdff89-kvs9f    0 (0%)        0 (0%)      0 (0%)           0 (0%)
      kube-system                public-crdf253b6025d64944ab99ed63bb4567b6-alb2-57df7c7b5b-tp6xw    0 (0%)        0 (0%)      0 (0%)           0 (0%)
      kube-system                public-crdf253b6025d64944ab99ed63bb4567b6-alb2-57df7c7b5b-z5p2v    0 (0%)        0 (0%)      0 (0%)           0 (0%)
      ```
      {: screen}

    2. 确认是否没有任何 ALB pod 部署到非边缘节点。示例：
    ```
      kubectl describe nodes -l dedicated!=edge | grep alb
      ```
      {: pre}
      * 如果 ALB pod 正确部署到了边缘节点，那么不会返回任何 ALB pod。这说明 ALB 已成功地重新安排到仅边缘工作程序节点上。
      * 如果返回了 ALB pod，请继续执行下一步。

6. 如果 NLB 或 ALB pod 仍部署到非边缘节点，那么可以删除这些 pod，以便它们可重新部署到边缘节点。**重要信息**：一次只能删除一个 pod，并且请先确认该 pod 已重新安排到边缘节点后，再删除其他 pod。
  1. 删除 pod。例如，如果其中一个 `webserver-lb` NLB pod 未安排到边缘节点，请运行以下命令：
    ```
    kubectl delete pod ibm-cloud-provider-ip-169-46-17-2-76fcb4965d-wz6dg
    ```
    {: pre}

  2. 验证 pod 是否已重新安排到边缘工作程序节点上。重新安排是自动执行的，但可能需要几分钟时间。具有外部 IP 地址 `169.46.17.2` 的 `webserver-lb` NLB 的示例：
      ```
    kubectl describe nodes -l dedicated=edge | grep "169-46-17-2"
    ```
    {: pre}

    输出示例：
    ```
    ibm-system                 ibm-cloud-provider-ip-169-46-17-2-76fcb4965d-wz6dg                 5m (0%)       0 (0%)      10Mi (0%)        0 (0%)
    ibm-system                 ibm-cloud-provider-ip-169-46-17-2-76fcb4965d-2z64r                 5m (0%)       0 (0%)      10Mi (0%)        0 (0%)
    ```
    {: screen}

</br>您已使用 `dedicated=edge` 标注工作程序池中的工作程序节点，并已将所有现有 ALB 和 NLB 重新部署到边缘节点。添加到集群的所有后续 ALB 和 NLB 也都会部署到边缘工作程序池中的边缘节点。接下来，请[阻止其他工作负载在边缘工作程序节点上运行](#edge_workloads)并[阻止流至工作程序节点上 NodePort 的入站流量](/docs/containers?topic=containers-network_policies#block_ingress)。

<br />


## 阻止工作负载在边缘工作程序节点上运行
{: #edge_workloads}

边缘工作程序节点的优点是可以将其指定为仅运行联网服务。
{:shortdesc}

使用 `dedicated=edge` 容忍度意味着所有网络负载均衡器 (NLB) 和 Ingress 应用程序负载均衡器 (ALB) 服务仅部署到已标注的工作程序节点。但是，要阻止其他工作负载在边缘工作程序节点上运行并使用工作程序节点资源，必须使用 [Kubernetes 污点 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/)。


开始之前：
- 确保您具有[对所有名称空间的 {{site.data.keyword.Bluemix_notm}} IAM **管理者**服务角色](/docs/containers?topic=containers-users#platform)。
- [登录到您的帐户。如果适用，请将相应的资源组设定为目标。为集群设置上下文。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

</br>要阻止其他工作负载在边缘工作程序节点上运行，请执行以下操作：

1. 使用 `dedicated=edge` 标签将污点应用于所有工作程序节点，这将阻止 pod 在工作程序节点上运行，并且会从工作程序节点中除去没有 `dedicated=edge` 标签的 pod。除去的 pod 会重新部署到具有容量的其他工作程序节点。
  ```
  kubectl taint node -l dedicated=edge dedicated=edge:NoSchedule dedicated=edge:NoExecute
  ```
  {: pre}
现在，仅具有 `dedicated=edge` 容忍度的 pod 会部署到边缘工作程序节点。

2. 验证边缘节点是否有污点。
  ```
  kubectl describe nodes -l dedicated=edge | grep "Taint|Hostname"
  ```
  {: pre}

  输出示例：
  ```
  Taints:             dedicated=edge:NoExecute
    Hostname:    10.176.48.83
  Taints:             dedicated=edge:NoExecute
    Hostname:    10.184.58.7
  ```
  {: screen}

3. 如果选择[为 NLB 1.0 服务启用源 IP 保留](/docs/containers?topic=containers-loadbalancer#node_affinity_tolerations)，请确保通过[向应用程序 pod 添加边缘节点亲缘关系](/docs/containers?topic=containers-loadbalancer#lb_edge_nodes)，将应用程序 pod 安排到边缘工作程序节点。应用程序 pod 必须安排到边缘节点才能接收入局请求。

4. 要除去污点，请运行以下命令。
    ```
    kubectl taint node <node_name> dedicated:NoSchedule- dedicated:NoExecute-
    ```
    {: pre}
