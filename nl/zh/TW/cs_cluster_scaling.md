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



# 調整叢集大小
{: #ca}

利用 {{site.data.keyword.containerlong_notm}} `ibm-iks-cluster-autoscaler` 外掛程式，您可以根據所排定之工作負載的調整大小需求，自動調整叢集裡的工作者節點儲存區，以增加或減少工作者節點儲存區中的工作者節點數目。`ibm-iks-cluster-autoscaler` 外掛程式是根據 [Kubernetes Cluster-Autoscaler 專案 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/autoscaler/tree/master/cluster-autoscaler)。
{: shortdesc}

想要改為自動調整 Pod 嗎？請查看[自動調整應用程式](/docs/containers?topic=containers-app#app_scaling)。
{: tip}

叢集 autoscaler 可用於設定為具有公用網路連線功能的標準叢集。如果您的叢集無法存取公用網路，例如防火牆後面的專用叢集，或只啟用專用服務端點的叢集，則您無法在叢集裡使用叢集 autoscaler。
{: important}

## 瞭解擴增及縮減
{: #ca_about}

叢集 autoscaler 會定期掃描叢集來調整它管理的工作者節點儲存區內的工作者節點數目，以回應工作負載資源要求及您配置的任何自訂設定，例如掃描間隔。叢集 autoscaler 會每分鐘檢查下列狀況。
{: shortdesc}

*   **擱置的 Pod 要擴增**：當運算資源不夠而無法在工作者節點上排定 Pod 時，此 Pod 即為擱置。當叢集 autoscaler 偵測到擱置的 Pod 時，autoscaler 會在區域之間平均地擴增工作者節點，以符合工作負載資源要求。
*   **有未充分利用的工作者節點要縮減**：依預設，在 10 分鐘或更長時間內，如果執行的工作者節點所要求的運算資源佔資源總量的比例低於 50%，並且可以將其工作負載重新排定到其他工作者節點上，則這些工作者節點即被視為未充分利用。如果叢集 autoscaler 偵測到未充分利用的工作者節點，它會一次一個來縮減工作者節點，這樣您就只會擁有您需要的運算資源。您也可以[自訂](/docs/containers?topic=containers-ca#ca_chart_values)預設的縮減使用率臨界值為 50% 長達 10 分鐘。

隨著時間推移，掃描與擴增和縮減會定期進行，並根據工作者節點數目而定，有可能需要較長的時間來完成，例如 30 分鐘。

叢集 autoscaler 會考量您為部署所定義的[資源要求 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/)而非實際的工作者節點使用量，來調整工作者節點數目。如果您的 Pod 及部署未要求適當數量的資源，您必須調整其配置檔。叢集 autoscaler 無法為您調整它們。另請記住，工作者節點會將其中一些運算資源用於基本叢集功能、預設和自訂[附加程式](/docs/containers?topic=containers-update#addons)以及[資源保留量](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node)。
{: note}

<br>
**何謂擴增和縮減？**<br>
一般而言，叢集 autoscaler 會計算叢集執行其工作負載所需的工作者節點數目。擴增或縮減叢集取決於諸多因素，其中包括下列因素。
*   您設定的每一區域工作者節點大小的下限及上限。
*   您擱置的 Pod 資源要求和某些與工作負載相關聯的 meta 資料，例如反親緣性、只在某些機型上放置 Pod 的標籤，或是 [Pod 中斷預算 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/concepts/workloads/pods/disruptions/)。
*   叢集 autoscaler 所管理的工作者節點儲存區，可能橫跨[多區域叢集](/docs/containers?topic=containers-ha_clusters#multizone)中的不同區域。
*   所設定的[自訂 Helm 圖表值](#ca_chart_values)，例如跳過使用本端儲存空間的工作者節點不進行刪除。

如需相關資訊，請參閱 Kubernetes 叢集 Autoscaler 常見問題中的[擴增如何運作？![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#how-does-scale-up-work) 和[縮減如何運作？ ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#how-does-scale-down-work)。

<br>

**是否可以變更擴增及縮減運作方式？**<br>
您可以自訂設定或使用其他 Kubernetes 資源來影響擴增及縮減的運作方式。
*   **擴增**：[自訂叢集 Autoscaler Helm 圖表值](#ca_chart_values)，例如 `scanInterval`、`expander`、`skipNodes` 或 `maxNodeProvisionTime`。請檢閱[過度佈建工作者節點](#ca_scaleup)的方式，讓您可以在工作者節點儲存區用盡資源之前擴增工作者節點。您也可以[設定 Kubernetes Pod 預算干擾及 Pod 優先順序截斷](#scalable-practices-apps)，以影響擴增的運作方式。
*   **縮減**：[自訂叢集 Autoscaler Helm 圖表值](#ca_chart_values)，例如 `scaleDownUnneededTime`、`scaleDownDelayAfterAdd`、`scaleDownDelayAfterDelete` 或 `scaleDownUtilizationThreshold`。

<br>
**可以透過設定每個區域的最小大小，立即將叢集擴增到該大小嗎？**<br>
不能，設定 `minSize` 不會自動觸發擴增。`minSize` 是臨界值，它讓叢集 autoscaler 不會調整為低於每個區域特定的工作者節點數目。如果您的叢集每個區域尚未達該數目，叢集 autoscaler 不會擴增，直到您有需要更多資源的工作負載資源要求。例如，如果您有工作者節點儲存區，其中三個區域各一個工作者節點（總計三個工作者節點），並將 `minSize` 設為每個區域 `4`，則叢集 autoscaler 不會立即為每個區域佈建額外三個工作者節點（總計 12 個工作者節點）。擴增會改為由資源要求觸發。如果您建立的工作負載會要求 15 個工作者節點的資源，則叢集 autoscaler 會擴增工作者節點儲存區，以滿足此要求。現在，`minSize` 表示叢集 autoscaler 不會調整為低於每個區域四個工作者節點，即使您移除要求該數量的工作負載也一樣。

<br>
**此行為與不是由叢集 autoscaler 管理的工作者節點儲存區有何不同？**<br>
當您[建立工作者節點儲存區](/docs/containers?topic=containers-add_workers#add_pool)時，您要指定每個區域有多少個工作者節點。工作者節點儲存區會維護工作者節點數目，直到您將它[調整大小](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_resize)或[重新平衡](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_rebalance)為止。工作者節點儲存區不會為您新增或移除工作者節點。如果您有超過可以排定的 Pod，則這些 Pod 會保持擱置狀態，直到您調整工作者節點儲存區的大小為止。

當您針對工作者節點儲存區啟用叢集 autoscaler 後，就會擴增或縮減工作者節點，以回應您的 Pod 規格設定及資源要求。您不需要手動調整工作者節點儲存區的大小或重新平衡。

<br>
**我可以看看叢集 autoscaler 如何擴增或縮減的範例嗎？**<br>
請看下列影像，此即叢集擴增和縮減的範例。

_圖：自動調整叢集大小。_![自動調整叢集大小 GIF](images/cluster-autoscaler-x3.gif){: gif}

1.  叢集有四個工作者節點，位於分散在兩個區域的兩個工作者節點儲存區中。每個區域的每個儲存區有一個工作者節點，但**工作者節點儲存區 A** 的機型為 `u2c.2x4`，**工作者節點儲存區 B** 的機型為 `b2c.4x16`。您的總運算資源大約是 10 個核心（2 個核心 x 2 個工作者節點用於**工作者節點儲存區 A**，4 個核心 x 2 個工作者節點用於**工作者節點儲存區 B**）。您的叢集目前所執行的工作負載要求這 10 個核心當中的 6 個。每個工作者節點上有其他運算資源是由[保留資源](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node)所使用，這是要執行該叢集、工作者節點及任何附加程式（例如叢集 autoscaler）所需要的。
2.  已配置叢集 autoscaler 來管理這兩個工作者節點儲存區，其每個區域的大小下限和大小上限如下：
    *  **工作者節點儲存區 A**：`minSize=1`、`maxSize=5`。
    *  **工作者節點儲存區 B**：`minSize=1`、`maxSize=2`。
3.  您排定的部署需要 14 個額外的應用程式 Pod 抄本，該應用程式要求每個抄本 1 個 CPU 核心。一個 Pod 抄本可以部署在現行資源上，但其他 13 個擱置。
4.  叢集 autoscaler 會在這些限制內擴增您的工作者節點，以支援其他 13 個 Pod 抄本資源要求。
    *  **工作者節點儲存區 A**：以循環式在各區域中盡可能均勻地新增 7 個工作者節點。工作者節點增加叢集運算容量大約 14 個核心（2 個核心 x 7 個工作者節點）。
    *  **工作者節點儲存區 B**：在各區域中均勻新增 2 個工作者節點，以達到 `maxSize`，即每個區域 2 個工作者節點。工作者節點增加叢集容量大約 8 個核心（4 個核心 x 2 工作者節點）。
5.  1 核心 20 個 Pod 的要求將分佈在工作者節點之間，如下所示。由於工作者節點具有資源保留以及為了涵蓋預設叢集特性而執行的 Pod，因此工作負載的 Pod 無法使用工作者節點的所有可用運算資源。例如，雖然 `b2c.4x16` 工作者節點具有 4 個核心數，但只有要求每個 Pod 最低 1 個核心數的 3 個 Pod 可以排定到工作者節點上。
    <table summary="說明工作負載在已調整的叢集裡分佈的表格。">
    <caption>工作負載在已調整的叢集裡的分佈。</caption>
    <thead>
    <tr>
      <th>工作者節點儲存區</th>
      <th>區域</th>
      <th>類型</th>
      <th>工作者節點數目</th>
      <th>Pod 數目</th>
    </tr>
    </thead>
    <tbody>
    <tr>
      <td>A</td>
      <td>dal10</td>
      <td>u2c.2x4</td>
      <td>4 個節點</td>
      <td>3 個 Pod</td>
    </tr>
    <tr>
      <td>A</td>
      <td>dal12</td>
      <td>u2c.2x4</td>
      <td>5 個節點</td>
      <td>5 個 Pod</td>
    </tr>
    <tr>
      <td>B</td>
      <td>dal10</td>
      <td>b2c.4x16</td>
      <td>兩個節點</td>
      <td>六個 Pod</td>
    </tr>
    <tr>
      <td>B</td>
      <td>dal12</td>
      <td>b2c.4x16</td>
      <td>兩個節點</td>
      <td>六個 Pod</td>
    </tr>
    </tbody>
    </table>
6.  您不再需要其他工作負載，因此刪除部署。經過一小段時間之後，叢集 autoscaler 會偵測到您的叢集不再需要全部的運算資源，而一次一個地縮減工作者節點。
7.  您的工作者節點儲存區已縮減。叢集 autoscaler 會定期掃描，檢查是否有擱置的 Pod 資源要求及未充分利用的工作者節點，以便擴增或縮減您的工作者節點儲存區。

## 遵循可擴充部署作法
{: #scalable-practices}

對於工作者節點及工作負載部署策略使用下列策略來善用叢集 Autoscaler。如需相關資訊，請參閱 [Kubernetes Cluster Autoscaler 常見問題 (FAQ) ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md)。
{: shortdesc}

使用一些測試工作負載來[試用叢集 Autoscaler](#ca_helm)，以瞭解[擴增及縮減運作方式](#ca_about)、您要配置的[自訂值](#ca_chart_values)，以及您想要的任何其他部分，例如[過度佈建](#ca_scaleup)工作者節點或[限制應用程式](#ca_limit_pool)。然後，清除測試環境，以及計劃在全新安裝的叢集 Autoscaler 中包括這些自訂值及其他設定。

### 是否可以一次自動調整多個工作者節點儲存區？
{: #scalable-practices-multiple}
是，安裝 Helm 圖表之後，您可以選擇叢集內要[在 configmap 中](#ca_cm)自動調整的工作者節點儲存區。每個叢集只能執行一個 `ibm-iks-cluster-autoscaler` Helm 圖表。
{: shortdesc}

### 如何確定叢集 Autoscaler 會回應應用程式所需的資源？
{: #scalable-practices-resrequests}

叢集 autoscaler 會調整您的叢集，以回應您的工作負載[資源要求 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/)。因此，針對所有部署指定[資源要求 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/)，因為叢集 Autoscaler 會使用資源要求來計算執行工作負載所需的工作者節點數目。請記住，自動調整是根據工作負載配置要求的運算使用量，而不考量其他因素，例如機器成本。
{: shortdesc}

### 是否可以將工作者節點儲存區縮減為零 (0) 個節點？
{: #scalable-practices-zero}

不能，您不能將叢集 Autoscaler 的 `minSize` 設定為 `0`。此外，除非在叢集的每個區域中[停用](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_configure)了所有公用應用程式負載平衡器 (ALB)，否則必須將每個區域的 `minSize` 變更為 `2` 個工作者節點，以便可以分散 ALB Pod 來達到高可用性。
{: shortdesc}

### 是否可以將部署最佳化以進行自動調整？
{: #scalable-practices-apps}

是，您可以將數個 Kubernetes 特性新增至部署，以調整叢集 Autoscaler 如何考量資源要求以進行調整。
{: shortdesc}
*   使用 [Pod 中斷預算 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/concepts/workloads/pods/disruptions/)，來防止 Pod 突然重新排程或遭到刪除。
*   如果您使用 Pod 優先順序，您可以[編輯優先順序截止值 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#how-does-cluster-autoscaler-work-with-pod-priority-and-preemption)，來變更會觸發擴增的優先順序類型。依預設，優先順序截止值為零 (`0`)。

### 是否可以搭配使用污點及容錯與自動調整的工作者節點儲存區？
{: #scalable-practices-taints}

因為不能在工作者節點儲存區層次上套用污點，所以請不要[為工作者節點加上污點](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/)，以避免非預期的結果。例如，當您部署有污點的工作者節點不能容忍的工作負載時，擴增時並不會考量這些工作者節點，而且即使叢集具有足夠容量，也可能訂購更多工作者節點。不過，如果有污點的工作者節點少於其所利用資源的臨界值（依預設為 50%），則仍被認為是未充分利用的工作者節點，因而納入縮減的考量。
{: shortdesc}

### 為何我的自動調整工作者節點儲存區不平衡？
{: #scalable-practices-unbalanced}

在擴增期間，叢集 Autoscaler 會在各區域之間平衡節點，但允許正負一個 (+/- 1) 工作者節點的差異。擱置的工作負載可能沒有要求足夠的容量使每個區域平衡。在此情況下，如果您想要手動平衡工作者節點儲存區，請[更新叢集 autoscaler configmap](#ca_cm)，以移除不平衡的工作者節點儲存區。然後，執行 `ibmcloud ks worker-pool-rebalance` [指令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_rebalance)，並將工作者節點儲存區新增回到叢集 autoscaler configmap。
{: shortdesc}


### 為何我無法將我的工作者節點儲存區調整大小或重新平衡？
{: #scalable-practices-resize}

對工作者節點儲存區啟用叢集 autoscaler 後，您就無法將工作者節點儲存區[調整大小](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_resize)或[重新平衡](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_rebalance)。您必須[編輯 configmap](#ca_cm)，以變更工作者節點儲存區大小下限或大小上限，或停用該工作者節點儲存區的叢集自動調整大小。請勿使用 `ibmcloud ks worker-rm` [指令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_rm)來移除工作者節點儲存區中的個別工作者節點，這樣會使工作者節點儲存區不平衡。
{: shortdesc}

此外，如果您將 `ibm-iks-cluster-autoscaler` Helm 圖表解除安裝之前未停用工作者節點儲存區，則無法手動調整工作者節點儲存區大小。請重新安裝 `ibm-iks-cluster-autoscaler` Helm 圖表，[編輯 configmap](#ca_cm) 以停用工作者節點儲存區，然後再試一次。

<br />


## 將叢集 autoscaler Helm 圖表部署至您的叢集
{: #ca_helm}

安裝具有 Helm 圖表的 {{site.data.keyword.containerlong_notm}} 叢集 autoscaler 外掛程式，以自動調整叢集裡的工作者節點儲存區。
{: shortdesc}

**開始之前**：

1.  [安裝必要的 CLI 及外掛程式](/docs/cli?topic=cloud-cli-getting-started)：
    *  {{site.data.keyword.Bluemix_notm}} CLI (`ibmcloud`)
    *  {{site.data.keyword.containerlong_notm}} 外掛程式 (`ibmcloud ks`)
    *  {{site.data.keyword.registrylong_notm}} 外掛程式 (`ibmcloud cr`)
    *  Kubernetes (`kubectl`)
    *  Helm (`helm`)
2.  [建立標準叢集](/docs/containers?topic=containers-clusters#clusters_ui)，其執行 **Kubernetes 1.12 版或更新版本**。
3.   [登入您的帳戶。適用的話，請將適當的資源群組設為目標。設定叢集的環境定義。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
4.  確認 {{site.data.keyword.Bluemix_notm}} Identity and Access Management 認證儲存在叢集裡。叢集 autoscaler 會使用此密碼對認證進行鑑別。如果遺漏密碼，請[透過重設認證來建立密碼](/docs/containers?topic=containers-cs_troubleshoot_storage#missing_permissions)。
    ```
    kubectl get secrets -n kube-system | grep storage-secret-store
    ```
    {: pre}
5.  叢集 autoscaler 只能調整具有 `ibm-cloud.kubernetes.io/worker-pool-id` 標籤的工作者節點儲存區。
    1.  檢查工作者節點儲存區是否具有必要的標籤。
        ```
        ibmcloud ks worker-pool-get --cluster <cluster_name_or_ID> --worker-pool <worker_pool_name_or_ID> | grep Labels
        ```
        {: pre}

        含有標籤的工作者節點儲存區範例：
        ```
        Labels:             ibm-cloud.kubernetes.io/worker-pool-id=a1aa111111b22b22cc3c3cc444444d44-4d555e5
        ```
        {: screen}
    2.  如果您的工作者節點儲存區沒有必要的標籤，請[新增工作者節點儲存區](/docs/containers?topic=containers-add_workers#add_pool)，並將此工作者節點儲存區與叢集 autoscaler 搭配使用。


<br>
**若要在叢集裡安裝 `ibm-iks-cluster-autoscaler` 外掛程式**，請執行下列動作：

1.  [遵循指示](/docs/containers?topic=containers-helm#public_helm_install)，將 **Helm 2.11 版或更新版本**用戶端安裝在本端機器上，並使用服務帳戶將 Helm 伺服器 (Tiller) 安裝在您的叢集裡。
2.  驗證已使用服務帳戶安裝 tiller。

    ```
    kubectl get serviceaccount -n kube-system tiller
    ```
    {: pre}

    輸出範例：

    ```
    NAME                                 SECRETS   AGE
    tiller                               1         2m
    ```
    {: screen}
3.  新增及更新叢集 autoscaler Helm 圖表所在的 Helm 儲存庫。
    ```
    helm repo add iks-charts https://icr.io/helm/iks-charts
    ```
    {: pre}
    ```
   helm repo update
   ```
    {: pre}
4.  在叢集的 `kube-system` 名稱空間中安裝叢集 autoscaler Helm 圖表。

    在安裝期間，您可以進一步[自訂叢集 Autoscaler 設定](#ca_chart_values)，例如在擴增或縮減工作者節點之前等待的時間數量。
    {: tip}

    ```
    helm install iks-charts/ibm-iks-cluster-autoscaler --namespace kube-system --name ibm-iks-cluster-autoscaler
    ```
    {: pre}

    輸出範例：
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

    如需使用叢集 autoscaler 的相關資訊，請參閱圖表 README.md 檔案。
    ```
    {: screen}

5.  驗證安裝已順利完成。

    1.  檢查叢集 autoscaler Pod 處於**執行中**狀態。
        ```
        kubectl get pods --namespace=kube-system | grep ibm-iks-cluster-autoscaler
        ```
        {: pre}
        輸出範例：
        ```
        ibm-iks-cluster-autoscaler-8497bfc968-dbn7w   1/1       Running   0          9m
        ```
        {: screen}
    2.  檢查是否已建立叢集 autoscaler 服務。
        ```
        kubectl get service --namespace=kube-system | grep ibm-iks-cluster-autoscaler
        ```
        {: pre}
    輸出範例：
    ```
        ibm-iks-cluster-autoscaler   ClusterIP   172.21.xxx.xx    <none>        8085/TCP        9m
        ```
        {: screen}

6.  針對要佈建叢集 autoscaler 的每一個叢集重複這些步驟。

7.  如果若要開始調整您的工作者節點儲存區，請參閱[更新叢集 autoscaler 配置](#ca_cm)。

<br />


## 更新叢集 autoscaler configmap 來啟用調整大小
{: #ca_cm}

更新叢集 autoscaler configmap，以根據您設定的最小值和最大值來自動調整工作者節點儲存區中的工作者節點。
{: shortdesc}

編輯 configmap 以啟用工作者節點儲存區之後，叢集 autoscaler 會調整叢集大小，以回應您的工作負載要求。因此，您不能將工作者節點儲存區[調整大小](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_resize)或[重新平衡](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_rebalance)。隨著時間推移，掃描與擴增和縮減會定期進行，並根據工作者節點數目而定，有可能需要較長的時間來完成，例如 30 分鐘。稍後，如果您想要[移除叢集 autoscaler](#ca_rm)，必須先停用 configmap 中的每一個工作者節點儲存區。
{: note}

**開始之前**：
*  [安裝 `ibm-iks-cluster-autoscaler` 外掛程式](#ca_helm)。
*  [登入您的帳戶。適用的話，請將適當的資源群組設為目標。設定叢集的環境定義。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

**若要更新叢集 autoscaler configmap 和值**，請執行下列動作：

1.  編輯叢集 autoscaler configmap YAML 檔案。
    ```
    kubectl edit cm iks-ca-configmap -n kube-system -o yaml
    ```
    {: pre}
    輸出範例：
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
2.  使用參數編輯 configmap，以定義叢集 autoscaler 如何調整叢集工作者節點儲存區大小。**附註：**除非在標準叢集的每個區域中[停用](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_configure)了所有公用應用程式負載平衡器 (ALB)，否則必須將每個區域的 `minSize` 變更為 `2`，以便可以分散 ALB Pod 來達到高可用性。

    <table>
    <caption>叢集 autoscaler configmap 參數</caption>
    <thead>
    <th id="parameter-with-default">具有預設值的參數</th>
    <th id="parameter-with-description">說明</th>
    </thead>
    <tbody>
    <tr>
    <td id="parameter-name" headers="parameter-with-default">`"name": "default"`</td>
    <td headers="parameter-name parameter-with-description">將 `"default"` 取代為您要調整的工作者節點儲存區的名稱或 ID。若要列出工作者節點儲存區，請執行 `ibmcloud ks worker-pools --cluster <cluster_name_or_ID>`。<br><br>
    若要管理多個工作者節點儲存區，請將 JSON 行複製到以逗點區隔的行，如下所示。<pre class="codeblock">[
     {"name": "default","minSize": 1,"maxSize": 2,"enabled":false},
     {"name": "Pool2","minSize": 2,"maxSize": 5,"enabled":true}
    ]</pre><br><br>
    **附註**：叢集 autoscaler 只能調整具有 `ibm-cloud.kubernetes.io/worker-pool-id` 標籤的工作者節點儲存區。若要檢查工作者節點儲存區是否具有必要的標籤，請執行 `ibmcloud ks worker-pool-get --cluster <cluster_name_or_ID> --worker-pool <worker_pool_name_or_ID> | grep Labels`。如果您的工作者節點儲存區沒有必要的標籤，請[新增工作者節點儲存區](/docs/containers?topic=containers-add_workers#add_pool)，並將此工作者節點儲存區與叢集 autoscaler 搭配使用。</td>
    </tr>
    <tr>
    <td id="parameter-minsize" headers="parameter-with-default">`"minSize": 1`</td>
    <td headers="parameter-minsize parameter-with-description">指定叢集 autoscaler 可將工作者節點儲存區縮減到的每個區域最小工作者節點數。值必須等於或大於 `2`，以便可以分散 ALB Pod 來達到高可用性。如果在標準叢集的每個區域中[停用](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_configure)了所有公用 ALB，則可以將值設定為 `1`。
    <p class="note">設定 `minSize` 不會自動觸發擴增。`minSize` 是臨界值，它讓叢集 autoscaler 不會調整為低於每個區域特定的工作者節點數目。如果您的叢集每個區域尚未達該數目，叢集 autoscaler 不會擴增，直到您有需要更多資源的工作負載資源要求。例如，如果您有工作者節點儲存區，其中三個區域各一個工作者節點（總計三個工作者節點），並將 `minSize` 設為每個區域 `4`，則叢集 autoscaler 不會立即為每個區域佈建額外三個工作者節點（總計 12 個工作者節點）。擴增會改為由資源要求觸發。如果您建立的工作負載會要求 15 個工作者節點的資源，則叢集 autoscaler 會擴增工作者節點儲存區，以滿足此要求。現在，`minSize` 表示叢集 autoscaler 不會調整為低於每個區域四個工作者節點，即使您移除要求該數量的工作負載也一樣。如需相關資訊，請參閱 [Kubernetes 文件 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#when-does-cluster-autoscaler-change-the-size-of-a-cluster)。</p></td>
    </tr>
    <tr>
    <td id="parameter-maxsize" headers="parameter-with-default">`"maxSize": 2`</td>
    <td headers="parameter-maxsize parameter-with-description">指定叢集 autoscaler 可將工作者節點儲存區擴增到的每個區域最大工作者節點數。這個值必須等於或大於您針對 `minSize` 設定的值。</td>
    </tr>
    <tr>
    <td id="parameter-enabled" headers="parameter-with-default">`"enabled": false`</td>
    <td headers="parameter-enabled parameter-with-description">將此值設為 `true`，讓叢集 autoscaler 管理對工作者節點儲存區的調整。將此值設為 `false`，以停止叢集 autoscaler 調整工作者節點儲存區。<br><br>
    稍後，如果您想要[移除叢集 autoscaler](#ca_rm)，必須先停用 configmap 中的每一個工作者節點儲存區。</td>
    </tr>
    </tbody>
    </table>
3.  儲存配置檔。
4.  取得叢集 autoscaler Pod。
    ```
    kubectl get pods -n kube-system
    ```
    {: pre}
5.  檢閱叢集 autoscaler Pod 的 **`Events`** 區段中的 **`ConfigUpdated`** 事件，驗證已順利更新 ConfigMap。configmap 的事件訊息使用下列格式：`minSize:maxSize:PoolName:<SUCCESS|FAILED>:error message`。

    ```
    kubectl describe pod -n kube-system <cluster_autoscaler_pod>
    ```
    {: pre}

    輸出範例：
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

## 自訂叢集 autoscaler Helm 圖表配置值
{: #ca_chart_values}

自訂叢集 autoscaler 設定，例如它在工作者節點擴增或縮減之前等待的時間量。
{: shortdesc}

**開始之前**：
*  [登入您的帳戶。適用的話，請將適當的資源群組設為目標。設定叢集的環境定義。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
*  [安裝 `ibm-iks-cluster-autoscaler` 外掛程式](#ca_helm)。

**若要更新叢集 autoscaler 值**，請執行下列動作：

1.  檢閱叢集 autoscaler Helm 圖表配置值。叢集 autoscaler 附有預設值。不過，您可能想要變更一些值，例如縮減或掃描間隔，視您變更叢集工作負載的頻率而定。
    ```
    helm get values ibm-iks-cluster-autoscaler -a
    ```
    {: pre}

        輸出範例：
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
    <caption>叢集 autoscaler 配置值</caption>
    <thead>
    <th>參數</th>
    <th>說明</th>
    <th>預設值</th>
    </thead>
    <tbody>
    <tr>
    <td>`api_route` 參數</td>
    <td>針對您叢集所在的地區設定 [{{site.data.keyword.containerlong_notm}} API 端點](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cli_api)。</td>
    <td>沒有預設值；使用您叢集所在的目標地區。</td>
    </tr>
    <tr>
    <td>`expander` 參數</td>
    <td>如果您有多個工作者節點儲存區，則指定叢集 autoscaler 如何判定要調整哪一個工作者節點儲存區。可能值為：
    <ul><li>`random`：在 `most-pods` 和 `least-waste` 之間隨機選取。</li>
    <li>`most-pods`：選取當擴增時能夠排定最多 Pod 的工作者節點儲存區。如果您使用 `nodeSelector`，請使用此方法，以確保 Pod 落在特定工作者節點上。</li>
    <li>`least-waste`：選取擴增後具有最少未用 CPU 的工作者節點儲存區。如果擴增後兩個工作者節點儲存區使用的 CPU 資源數量相同，則將選取具有最少未用記憶體的工作者節點儲存區。</li></ul></td>
    <td>random</td>
    </tr>
    <tr>
    <td>`image.repository` 參數</td>
    <td>指定要使用的叢集 autoscaler Docker 映像檔。</td>
    <td>`icr.io/iks-charts/ibm-iks-cluster-autoscaler`</td>
    </tr>
    <tr>
    <td>`image.pullPolicy` 參數</td>
    <td>指定何時取回 Docker 映像檔。可能值為：
    <ul><li>`Always`：每次啟動 Pod 時都取回映像檔。</li>
    <li>`IfNotPresent`：唯有當本端沒有映像檔時才取回映像檔。</li>
    <li>`Never`：假設本端已有影像檔，絕不取回映像檔。</li></ul></td>
    <td>Always</td>
    </tr>
    <tr>
    <td>`maxNodeProvisionTime` 參數</td>
    <td>設定在叢集 autoscaler 取消擴增要求之前，工作者節點可以用來開始佈建的時間量上限（分鐘）。</td>
    <td>`120m`</td>
    </tr>
    <tr>
    <td>`resources.limits.cpu` 參數</td>
    <td>設定 `ibm-iks-cluster-autoscaler` Pod 可以使用的工作者節點 CPU 的數量上限。</td>
    <td>`300m`</td>
    </tr>
    <tr>
    <td>`resources.limits.memory` 參數</td>
    <td>設定 `ibm-iks-cluster-autoscaler` Pod 可以使用的工作者節點記憶體的數量上限。</td>
    <td>`300Mi`</td>
    </tr>
    <tr>
    <td>`resources.requests.cpu` 參數</td>
    <td>設定 `ibm-iks-cluster-autoscaler` Pod 開始的工作者節點 CPU 的數量下限。</td>
    <td>`100m`</td>
    </tr>
    <tr>
    <td>`resources.requests.memory` 參數</td>
    <td>設定 `ibm-iks-cluster-autoscaler` Pod 開始的工作者節點記憶體的數量下限。</td>
    <td>`100Mi`</td>
    </tr>
    <tr>
    <td>`scaleDownUnneededTime` 參數</td>
    <td>設定在可以縮減之前工作者節點必須為非必要的時間量（分鐘）。</td>
    <td>`10m`</td>
    </tr>
    <tr>
    <td>`scaleDownDelayAfterAdd`、`scaleDownDelayAfterDelete` 參數</td>
    <td>設定在擴增 (`add`) 或縮減 (`delete` 之後，叢集 autoscaler 等待再次啟動調整動作的時間量（分鐘）。</td>
    <td>`10m`</td>
    </tr>
    <tr>
    <td>`scaleDownUtilizationThreshold` 參數</td>
    <td>設定工作者節點使用率臨界值。如果工作者節點使用率低於臨界值，則會考慮將工作者節點縮減。工作者節點利用率的計算方法是，工作者節點上執行的所有 Pod 要求的 CPU 和記憶體資源之和除以工作者節點資源容量。</td>
    <td>`0.5`</td>
    </tr>
    <tr>
    <td>`scanInterval` 參數</td>
    <td>設定叢集 autoscaler 掃描觸發擴增或縮減的工作負載用量的頻率（分鐘）。</td>
    <td>`1m`</td>
    </tr>
    <tr>
    <td>`skipNodes.withLocalStorage` 參數</td>
    <td>如果設為 `true`，則具有能將資料儲存至本端儲存空間的 Pod 的工作者節點不會縮減。</td>
    <td>`true`</td>
    </tr>
    <tr>
    <td>`skipNodes.withSystemPods` 參數</td>
    <td>如果設為 `true`，則具有 `kube-system` Pod 的工作者節點不會縮減。請勿將此值設為 `false`，因為縮減 `kube-system` Pod 可能會有非預期的結果。</td>
    <td>`true`</td>
    </tr>
    </tbody>
    </table>
2.  若要變更任何叢集 autoscaler 配置值，請使用新值更新 Helm 圖表。
    包括 `--recreate-pods` 旗標，以重建任何現有叢集 Autoscaler Pod 來挑選自訂設定變更。
    ```
    helm upgrade --set scanInterval=2m ibm-iks-cluster-autoscaler iks-charts/ibm-iks-cluster-autoscaler -i --recreate-pods
    ```
    {: pre}

    若要將圖表重設為預設值：
    ```
    helm upgrade --reset-values ibm-iks-cluster-autoscaler iks-charts/ibm-iks-cluster-autoscaler --recreate-pods
    ```
    {: pre}
3.  若要驗證您的變更，請再次檢閱 Helm 圖表值。
    ```
    helm get values ibm-iks-cluster-autoscaler -a
    ```
    {: pre}


## 限制應用程式只在某些自動調整的工作者節點儲存區上執行
{: #ca_limit_pool}

若要將 Pod 限制至部署到叢集 autoscaler 管理的特定工作者節點儲存區，請使用標籤以及 `nodeSelector` 或 `nodeAffinity`。透過 `nodeAffinity`，可以進一步地控制排程行為的運作方式，以便讓 Pod 與工作者節點相符合。如需將 Pod 指派給工作者節點的相關資訊，請參閱 [Kubernetes 文件 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/)。
{: shortdesc}

**開始之前**：
*  [安裝 `ibm-iks-cluster-autoscaler` 外掛程式](#ca_helm)。
*  [登入您的帳戶。適用的話，請將適當的資源群組設為目標。設定叢集的環境定義。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

**若要限制 Pod 在某些自動調整的工作者節點儲存區上執行**，請執行下列動作：

1.  以您要使用的標籤來建立工作者節點儲存區。
    例如，標籤可能為 `app: nginx`。
    ```
    ibmcloud ks worker-pool-create --name <name> --cluster <cluster_name_or_ID> --machine-type <machine_type> --size-per-zone <number_of_worker_nodes> --labels <key>=<value>
    ```
    {: pre}
2.  [將工作者節點儲存區新增至叢集 autoscaler 配置](#ca_cm)。
3.  在 Pod spec 範本中，將 `nodeSelector` 或 `nodeAffinity` 與工作者節點儲存區中使用的標籤相符合。

    `nodeSelector` 的範例：
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

    `nodeAffinity` 的範例：
    ```
        spec:
          containers:      - name: nginx
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
4.  部署 Pod。因為有相符的標籤，Pod 會排程到含標籤的工作者節點儲存區中的工作者節點。
    ```
      kubectl apply -f pod.yaml
      ```
    {: pre}

<br />


## 在工作者節點儲存區資源不足之前，擴增工作者節點
{: #ca_scaleup}

如[瞭解叢集 autoscaler 的運作方式](#ca_about)主題和 [Kubernetes 叢集 autoscaler 常見問題 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md) 中所述，叢集 autoscaler 可根據工作者節點儲存區的可用資源來擴增工作者節點儲存區，以回應工作負載要求的資源。不過，您可能想要叢集 autoscaler 在工作者節點儲存區用完資源之前就擴增工作者節點。在此情況下，因為工作者節點儲存區已擴增為符合資源要求，所以您的工作負載不需久等工作者節點佈建。
{: shortdesc}

叢集 autoscaler 不支援工作者節點儲存區提早調整（過度佈建）。不過，您可以配置其他 Kubernetes 資源使用叢集 autoscaler 來達到提早調整。

<dl>
  <dt><strong>暫停 Pod</strong></dt>
  <dd>您可以建立部署，以特定的資源要求在 Pod 中部署[暫停容器![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://stackoverflow.com/questions/48651269/what-are-the-pause-containers)，並指派此部署為低 Pod 優先順序。當較高優先順序的工作負載需要這些資源時，暫停 Pod 會被先占，而成為擱置的 Pod。此事件會觸發叢集 autoscaler 擴增。<br><br>如需設定暫停 Pod 部署的相關資訊，請參閱 [Kubernetes 常見問題 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#how-can-i-configure-overprovisioning-with-cluster-autoscaler)。您可以使用[此範例過度佈建配置檔 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/IBM-Cloud/kube-samples/blob/master/ibm-ks-cluster-autoscaler/overprovisioning-autoscaler.yaml)，以建立優先順序類別、服務帳戶及部署。<p class="note">如果您使用此方法，請確定您瞭解 [Pod 優先順序](/docs/containers?topic=containers-pod_priority#pod_priority)如何運作，以及如何設定部署的 Pod 優先順序。例如，如果暫停 Pod 沒有足夠的資源可用於較高優先順序的 Pod，則此 Pod 不會被先占。較高優先順序的工作負載仍處於擱置狀態，因此會觸發叢集 autoscaler 來擴增。但是在這種情況下，擴增動作並不屬於提早擴增，因為您要執行的工作負載由於資源不足而無法排定。</p></dd>

  <dt><strong>水平 Pod 自動調整 (HPA)</strong></dt>
  <dd>因為水平 Pod 自動是根據 Pod 的平均 CPU 使用率，所以在工作者節點儲存區耗盡資源之前，就會達到您設定的 CPU 使用率限制。會要求更多的 Pod，隨後會觸發叢集 autoscaler 來擴增工作者節點儲存區。<br><br>如需設定 HPA 的相關資訊，請參閱 [Kubernetes 文件 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale-walkthrough/)。</dd>
</dl>

<br />


## 更新叢集 autoscaler Helm 圖表
{: #ca_helm_up}

您可以將現有的叢集 autoscaler Helm 圖表更新為最新版本。若要檢查現行 Helm 圖表版本，請執行 `helm ls | grep cluster-autoscaler`。
{: shortdesc}

從 1.0.2 版或更舊版本更新至最新的 Helm 圖表？[請遵循這些指示](#ca_helm_up_102)。
{: note}

開始之前：[登入您的帳戶。適用的話，請將適當的資源群組設為目標。設定叢集的環境定義。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  更新 Helm 儲存庫，以擷取此儲存庫中所有 Helm 圖表的最新版本。
    ```
   helm repo update
   ```
    {: pre}

2.  選用項目：將最新的 Helm 圖表下載至您的本端機器。然後，將套件解壓縮，並檢閱 `release.md` 檔案，以找到最新版本資訊。
    ```
    helm fetch iks-charts/ibm-iks-cluster-autoscaler
    ```
    {: pre}

3.  尋找您已在叢集裡安裝的叢集 autoscaler Helm 圖表的名稱。
    ```
    helm ls | grep cluster-autoscaler
    ```
    {: pre}

        輸出範例：
    ```
    myhelmchart 	1       	Mon Jan  7 14:47:44 2019	DEPLOYED	ibm-ks-cluster-autoscaler-1.0.0  	kube-system
    ```
    {: screen}

4.  將叢集 autoscaler Helm 圖表更新為最新版本。
    ```
    helm upgrade --force --recreate-pods <helm_chart_name>  iks-charts/ibm-iks-cluster-autoscaler
    ```
    {: pre}

5.  驗證[叢集 autoscaler configmap](#ca_cm) `workerPoolsConfig.json` 區段已針對您要調整的工作者節點儲存區設為 `"enabled": true`。
    ```
    kubectl describe cm iks-ca-configmap -n kube-system
    ```
    {: pre}

        輸出範例：
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

### 從 1.0.2 版或更舊版本更新至最新的 Helm 圖表
{: #ca_helm_up_102}

叢集 autoscaler 的最新 Helm 圖表版本需要完整移除先前已安裝的叢集 autoscaler Helm 圖表版本。如果您已安裝 Helm 圖表 1.0.2 版或更舊版本，請先將該版本解除安裝，然後再安裝叢集 autoscaler 的最新 Helm 圖表。
{: shortdesc}

開始之前：[登入您的帳戶。適用的話，請將適當的資源群組設為目標。設定叢集的環境定義。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  取得叢集 autoscaler configmap。
    ```
    kubectl get cm iks-ca-configmap -n kube-system -o yaml > iks-ca-configmap.yaml
    ```
    {: pre}
2.  透過將 `"enabled"` 值設為 `false`，來移除 configmap 中的所有工作者節點儲存區。
    ```
    kubectl edit cm iks-ca-configmap -n kube-system
    ```
    {: pre}
3.  如果您已在 Helm 圖表中套用自訂設定，請記下您的自訂設定。
    ```
    helm get values ibm-ks-cluster-autoscaler -a
    ```
    {: pre}
4.  將現行 Helm 圖表解除安裝。
    ```
    helm delete --purge ibm-ks-cluster-autoscaler
    ```
    {: pre}
5.  更新 Helm 圖表儲存庫，以取得最新叢集 autoscaler Helm 圖表版本。
    ```
   helm repo update
   ```
    {: pre}
6.  安裝最新叢集 autoscaler Helm 圖表。套用您先前與 `--set` 旗標搭配使用的任何自訂設定，例如 `scanInterval=2m`。
    ```
    helm install  iks-charts/ibm-iks-cluster-autoscaler --namespace kube-system --name ibm-iks-cluster-autoscaler [--set <custom_settings>]
    ```
    {: pre}
7.  套用您先前擷取的叢集 autoscaler configmap，以啟用工作者節點儲存區的自動調整。
    ```
    kubectl apply -f iks-ca-configmap.yaml
    ```
    {: pre}
8.  取得叢集 autoscaler Pod。
    ```
    kubectl get pods -n kube-system
    ```
    {: pre}
9.  檢閱叢集 autoscaler Pod 的 **`Events`** 區段來尋找 **`ConfigUpdated`** 事件，驗證已順利更新 configmap。configmap 的事件訊息使用下列格式：`minSize:maxSize:PoolName:<SUCCESS|FAILED>:error message`。
    ```
    kubectl describe pod -n kube-system <cluster_autoscaler_pod>
    ```
    {: pre}

    輸出範例：
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


## 移除叢集 autoscaler
{: #ca_rm}

如果您不想自動調整工作者節點儲存區，可以將叢集 autoscaler Helm 圖表解除安裝。移除之後，如果您要將工作者節點儲存區[調整大小](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_resize)或[重新平衡](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_rebalance)，必須手動進行。
{: shortdesc}

開始之前：[登入您的帳戶。適用的話，請將適當的資源群組設為目標。設定叢集的環境定義。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  在[叢集 autoscaler configmap](#ca_cm) 中，透過將 `"enabled"` 值設為 `false` 來移除工作者節點儲存區。
    ```
    kubectl edit cm iks-ca-configmap -n kube-system
    ```
    {: pre}
    輸出範例：
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
2.  列出現有的 Helm 圖表，並記下叢集 autoscaler 的名稱。
    ```
    helm ls
    ```
    {: pre}
3.  從叢集移除現有的 Helm 圖表。
    ```
    helm delete --purge <ibm-iks-cluster-autoscaler_name>
    ```
    {: pre}
