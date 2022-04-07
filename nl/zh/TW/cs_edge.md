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


# 限制送至邊緣工作者節點的網路資料流量
{: #edge}

邊緣工作者節點可以藉由容許較少的工作者節點可在外部進行存取，以及隔離 {{site.data.keyword.containerlong}} 中的網路工作負載，來增進 Kubernetes 叢集的安全。
{:shortdesc}

僅在標示這些工作者節點可以進行網路連線時，其他工作負載才無法取用工作者節點的記憶體，並干擾網路連線。


如果您有多區域叢集，並要將網路資料流量限制為傳送至邊緣工作者節點，則每個區域中必須至少已啟用 2 個邊緣工作者節點，才可實現負載平衡器或 Ingress Pod 的高可用性。建立邊緣節點工作者節點儲存區，此儲存區跨叢集裡的所有區域，並且每個區域至少有 2 個工作者節點。
{: tip}

## 將工作者節點標示為邊緣節點
{: #edge_nodes}

將 `dedicated=edge` 標籤新增到叢集裡每個公用或專用 VLAN 上的兩個或更多工作者節點，以確保網路負載平衡器 (NLB) 和 Ingress 應用程式負載平衡器 (ALB) 僅部署到這些工作者節點。
{:shortdesc}

在 Kubernetes 1.14 版以及更新版本中，公用和專用 NLB 和 ALB 都可以部署到邊緣工作者節點。在 Kubernetes 1.13 和較舊版本中，公用和專用 ALB 以及公用 NLB 可以部署到邊緣節點，但專用 NLB 只能部署到叢集裡的非邊緣工作者節點。
{: note}

開始之前：

* 請確定您有下列 [{{site.data.keyword.Bluemix_notm}} IAM 角色](/docs/containers?topic=containers-users#platform)：
  * 用於叢集的任何平台角色
  * 用於所有名稱空間的**撰寫者**或**管理員**服務角色
* [登入您的帳戶。適用的話，請將適當的資源群組設為目標。設定叢集的環境定義。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

</br>若要將工作者節點標示為邊緣節點，請執行下列動作：

1. [建立新的工作者節點儲存區](/docs/containers?topic=containers-add_workers#add_pool)，此儲存區跨叢集裡的所有區域，並且每個區域至少有 2 個工作者節點。在 `ibmcloud ks worker-pool-create` 指令中，包含 `--labels dedicated=edge` 旗標以標示儲存區中的所有工作者節點。此儲存區中的所有工作者節點（包括日後新增的任何工作者節點）都會標示為邊緣節點。
  <p class="tip">如果要使用現有工作者節點儲存區，該儲存區必須跨叢集裡的所有區域，並且每個區域至少有 2 個工作者節點。可以使用 [PATCH 工作者節點儲存區 API](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/PatchWorkerPool) 透過 `dedicated=edge` 來標示工作者節點儲存區。在要求內文中，傳遞下列 JSON。在使用 `dedicated=edge` 標示工作者節點儲存區後，所有現有及後續工作者節點都將獲得此標籤，並且 Ingress 和負載平衡器會部署到邊緣工作者節點。
      <pre class="screen">
      {
        "labels": {"dedicated":"edge"},
        "state": "labels"
      }</pre></p>

2. 驗證工作者節點儲存區和工作者節點是否具有 `dedicated=edge` 標籤。
  * 若要檢查工作者節點儲存區，請執行下列指令：
    ```
    ibmcloud ks worker-pool-get --cluster <cluster_name_or_ID> --worker-pool <worker_pool_name_or_ID>
    ```
    {: pre}

  * 若要檢查個別工作者節點，請檢閱下列指令輸出中的 **Labels** 欄位。
    ```
    kubectl describe node <worker_node_private_IP>
    ```
    {: pre}

3. 擷取叢集裡的所有現有 NLB 和 ALB。
  ```
  kubectl get services --all-namespaces | grep LoadBalancer
  ```
  {: pre}

  記下輸出中的每個 LoadBalancer 服務的 **Namespace** 和 **Name**。例如，在下列輸出中，有四個 LoadBalancer 服務：`default` 名稱空間中的一個公用 NLB 以及 `kube-system` 名稱空間中的一個專用 ALB 和兩個公用 ALB。
  ```
  NAMESPACE     NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP     PORT(S)                                     AGE
  default       webserver-lb                                     LoadBalancer   172.21.190.18    169.46.17.2     80:30597/TCP                                10m
  kube-system   private-crdf253b6025d64944ab99ed63bb4567b6-alb1  LoadBalancer   172.21.158.78    10.185.94.150   80:31015/TCP,443:31401/TCP,9443:32352/TCP   25d
  kube-system   public-crdf253b6025d64944ab99ed63bb4567b6-alb1   LoadBalancer   172.21.84.248    169.48.228.78   80:30286/TCP,443:31363/TCP                  1h
  kube-system   public-crdf253b6025d64944ab99ed63bb4567b6-alb2   LoadBalancer   172.21.229.73    169.46.17.6     80:31104/TCP,443:31138/TCP                  57m
  ```
  {: screen}

4. 使用上一步的輸出，對每個 NLB 和 ALB 執行下列指令。此指令會將 NLB 或 ALB 重新部署到邊緣工作者節點。

  如果叢集執行 Kubernetes 1.14 或更新版本，則可以將公用和專用 NLB 和 ALB 部署到邊緣工作者節點。在 Kubernetes 1.13 和較舊版本中，只有公用和專用 ALB 以及公用 NLB 可以部署到邊緣節點，因此不要重新部署專用 NLB 服務。
  {: note}

  ```
  kubectl get service -n <namespace> <service_name> -o yaml | kubectl apply -f -
  ```
  {: pre}

  輸出範例：
  ```
  service "webserver-lb" configured
  ```
  {: screen}

5. 若要驗證是否將網路連線功能工作負載限制為只能傳送至邊緣節點，請確認是否將 NLB 和 ALB Pod 排定到了邊緣節點上，而未排定到非邊緣節點上。

  * NLB Pod：
    1. 確認是否將 NLB Pod 部署到了邊緣節點。搜尋步驟 3 的輸出中列出的 LoadBalancer 服務的外部 IP 位址。將句點 (`.`) 取代為連字號 (`-`)。外部 IP 位址為 `169.46.17.2` 的 `webserver-lb` NLB 範例：
      ```
      kubectl describe nodes -l dedicated=edge | grep "169-46-17-2"
      ```
      {: pre}

            輸出範例：
      ```
      ibm-system                 ibm-cloud-provider-ip-169-46-17-2-76fcb4965d-wz6dg                 5m (0%)       0 (0%)      10Mi (0%)        0 (0%)
      ibm-system                 ibm-cloud-provider-ip-169-46-17-2-76fcb4965d-2z64r                 5m (0%)       0 (0%)      10Mi (0%)        0 (0%)
      ```
      {: screen}
    2. 確認是否沒有任何 NLB Pod 部署到非邊緣節點。外部 IP 位址為 `169.46.17.2` 的 `webserver-lb` NLB 範例：
      ```
      kubectl describe nodes -l dedicated!=edge | grep "169-46-17-2"
      ```
      {: pre}
      * 如果 NLB Pod 正確部署到了邊緣節點，則不會傳回任何 NLB Pod。NLB 已順利地重新排定到僅邊緣工作者節點上。
      * 如果傳回了 NLB Pod，請繼續執行下一步。

  * ALB Pod：
    1. 確認是否將所有 ALB Pod 都部署到了邊緣節點。搜尋關鍵字 `alb`。每個公用和專用 ALB 都有兩個 Pod。範例：
      ```
      kubectl describe nodes -l dedicated=edge | grep alb
      ```
      {: pre}

      具有兩個區域的叢集的範例輸出，其中叢集已啟用了一個預設專用 ALB 和兩個預設公用 ALB：
      ```
      kube-system                private-crdf253b6025d64944ab99ed63bb4567b6-alb1-d5dd478db-27pv4    0 (0%)        0 (0%)      0 (0%)           0 (0%)
      kube-system                private-crdf253b6025d64944ab99ed63bb4567b6-alb1-d5dd478db-7p9q6    0 (0%)        0 (0%)      0 (0%)           0 (0%)
      kube-system                public-crdf253b6025d64944ab99ed63bb4567b6-alb1-5ff8cdff89-s77z6    0 (0%)        0 (0%)      0 (0%)           0 (0%)
      kube-system                public-crdf253b6025d64944ab99ed63bb4567b6-alb1-5ff8cdff89-kvs9f    0 (0%)        0 (0%)      0 (0%)           0 (0%)
      kube-system                public-crdf253b6025d64944ab99ed63bb4567b6-alb2-57df7c7b5b-tp6xw    0 (0%)        0 (0%)      0 (0%)           0 (0%)
      kube-system                public-crdf253b6025d64944ab99ed63bb4567b6-alb2-57df7c7b5b-z5p2v    0 (0%)        0 (0%)      0 (0%)           0 (0%)
      ```
      {: screen}

    2. 確認是否沒有任何 ALB Pod 部署到非邊緣節點。範例：
    ```
      kubectl describe nodes -l dedicated!=edge | grep alb
      ```
      {: pre}
      * 如果 ALB Pod 正確部署到了邊緣節點，則不會傳回任何 ALB Pod。ALB 已順利地重新排定到僅邊緣工作者節點上。
      * 如果傳回了 ALB Pod，請繼續執行下一步。

6. 如果 NLB 或 ALB Pod 仍部署到非邊緣節點，則可以刪除這些 Pod，以便它們可重新部署到邊緣節點。**重要事項**：一次只能刪除一個 Pod，並且請先確認該 Pod 已重新排定到邊緣節點後，再刪除其他 Pod。
  1. 刪除 Pod。例如，如果其中一個 `webserver-lb` NLB Pod 未排定到邊緣節點，請執行下列指令：
    ```
    kubectl delete pod ibm-cloud-provider-ip-169-46-17-2-76fcb4965d-wz6dg
    ```
    {: pre}

  2. 驗證 Pod 是否已重新排定到邊緣工作者節點上。重新排定是自動執行的，但可能需要幾分鐘時間。外部 IP 位址為 `169.46.17.2` 的 `webserver-lb` NLB 範例：
    ```
    kubectl describe nodes -l dedicated=edge | grep "169-46-17-2"
    ```
    {: pre}

    輸出範例：
    ```
    ibm-system                 ibm-cloud-provider-ip-169-46-17-2-76fcb4965d-wz6dg                 5m (0%)       0 (0%)      10Mi (0%)        0 (0%)
    ibm-system                 ibm-cloud-provider-ip-169-46-17-2-76fcb4965d-2z64r                 5m (0%)       0 (0%)      10Mi (0%)        0 (0%)
    ```
    {: screen}

</br>您已使用 `dedicated=edge` 標示工作者節點儲存區中的工作者節點，並已將所有現有 ALB 和 NLB 重新部署到邊緣節點。新增到叢集的所有後續 ALB 和 NLB 也都會部署到邊緣工作者節點儲存區中的邊緣節點。接下來，請避免其他[工作負載在邊緣工作者節點上執行](#edge_workloads)，以及[封鎖對工作者節點上 NodePort 的入埠資料流量](/docs/containers?topic=containers-network_policies#block_ingress)。

<br />


## 避免工作負載在邊緣工作者節點上執行
{: #edge_workloads}

邊緣工作者節點的一項好處是它們可以指定為僅執行網路服務。
{:shortdesc}

使用 `dedicated=edge` 容忍度意味著所有網路負載平衡器 (NLB) 和 Ingress 應用程式負載平衡器 (ALB) 服務僅部署到已標示的工作者節點。不過，為了避免其他工作負載在邊緣工作者節點上執行，以及取用工作者節點資源，您必須使用 [Kubernetes 污點 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/)。


開始之前：
- 確保您具有[對所有名稱空間的 {{site.data.keyword.Bluemix_notm}} IAM **管理員**服務角色](/docs/containers?topic=containers-users#platform)。
- [登入您的帳戶。適用的話，請將適當的資源群組設為目標。設定叢集的環境定義。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

</br>若要阻止其他工作負載在邊緣工作者節點上執行，請執行下列動作：

1. 套用 `dedicated=edge` 標籤將污點套用於所有工作者節點，這將阻止 Pod 在工作者節點上執行，並且會從工作者節點中移除沒有 `dedicated=edge` 標籤的 Pod。移除的 Pod 會重新部署到具有容量的其他工作者節點。
  ```
  kubectl taint node -l dedicated=edge dedicated=edge:NoSchedule dedicated=edge:NoExecute
  ```
  {: pre}
  現在，僅具有 `dedicated=edge` 容忍的 Pod 才會部署至您的邊緣工作者節點。

2. 驗證已為邊緣節點加上污點。
  ```
  kubectl describe nodes -l dedicated=edge | grep "Taint|Hostname"
  ```
  {: pre}

  輸出範例：
  ```
  Taints:             dedicated=edge:NoExecute
    Hostname:    10.176.48.83
  Taints:             dedicated=edge:NoExecute
    Hostname:    10.184.58.7
  ```
  {: screen}

3. 如果選擇[為 NLB 1.0 服務啟用來源 IP 保留](/docs/containers?topic=containers-loadbalancer#node_affinity_tolerations)，請確保透過[向應用程式 Pod 新增邊緣節點親緣性](/docs/containers?topic=containers-loadbalancer#lb_edge_nodes)，將應用程式 Pod 排定到邊緣工作者節點。必須在邊緣節點上排定應用程式 Pod，才能接收送入要求。

4. 若要移除污點，請執行下列指令。
    ```
    kubectl taint node <node_name> dedicated:NoSchedule- dedicated:NoExecute-
    ```
    {: pre}
