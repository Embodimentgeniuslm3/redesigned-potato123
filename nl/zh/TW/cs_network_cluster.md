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


# 變更服務端點或 VLAN 連線
{: #cs_network_cluster}

[建立叢集](/docs/containers?topic=containers-clusters)時初始設定網路後，可以變更可借以存取 Kubernetes 主節點的服務端點，或變更工作者節點的 VLAN 連線。
{: shortdesc}

## 設定專用服務端點
{: #set-up-private-se}

在執行 Kubernetes 1.11 版或更新版本的叢集裡，啟用或停用叢集的專用服務端點。
{: shortdesc}

專用服務端點讓您的 Kubernetes 主節點可供專用存取。工作者節點和授權叢集使用者可以透過專用網路與 Kubernetes 主節點通訊。若要確定是否可以啟用專用服務端點，請參閱[工作者節點到主節點的通訊以及使用者到主節點的通訊](/docs/containers?topic=containers-plan_clusters#internet-facing)。請注意，專用服務端點在啟用之後，就無法予以停用。

在為 [VRF](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) 和[服務端點](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started)已啟用帳戶之前，建立的叢集是否僅具有專用服務端點？如果是，請嘗試[設定公用服務端點](#set-up-public-se)，以便可以一直使用叢集，直到支援案例得到處理，可以更新帳戶。
{: tip}

1. 在您的 IBM Cloud 基礎架構 (SoftLayer) 帳戶中啟用 [VRF](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud)。
2. [啟用 {{site.data.keyword.Bluemix_notm}} 帳戶，以使用服務端點](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started)。
3. 啟用專用服務端點。
   ```
  ibmcloud ks cluster-feature-enable private-service-endpoint --cluster <cluster_name_or_ID>
  ```
   {: pre}
4. 重新整理 Kubernetes 主節點 API 伺服器，以使用專用服務端點。您可以遵循 CLI 中的提示，或手動執行下列指令。
   ```
        ibmcloud ks apiserver-refresh --cluster <cluster_name_or_ID>
        ```
   {: pre}

5. [建立 ConfigMap](/docs/containers?topic=containers-update#worker-up-configmap)，以控制叢集裡可能在某個時間無法使用的工作者節點數目上限。更新工作者節點時，ConfigMap 會協助避免應用程式的關閉，因為會依序將應用程式重新排定至可用的工作者節點。
6. 更新叢集裡的所有工作者節點，以反映專用服務端點配置。

   <p class="important">透過發出 update 指令，會重新載入工作者節點，以反映服務端點配置。如果沒有工作者節點更新項目可供使用，則必須[手動重新載入工作者節點](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli)。如果您重新載入，則務必隔離、排除及管理訂購，以控制在某個時間無法使用的工作者節點數目上限。</p>
   ```
ibmcloud ks worker-update --cluster <cluster_name_or_ID> --workers <worker1,worker2>
    ```
   {: pre}

8. 如果叢集位於受防火牆保護的環境，請執行下列動作：
  * [容許授權叢集使用者執行 `kubectl` 指令，以透過專用服務端點來存取主節點。](/docs/containers?topic=containers-firewall#firewall_kubectl)
  * 針對基礎架構資源以及您計劃使用的 {{site.data.keyword.Bluemix_notm}} 服務，[容許專用 IP 的出埠網路資料流量](/docs/containers?topic=containers-firewall#firewall_outbound)。

9. 選用項目：若只要使用專用服務端點，請停用公用服務端點。
   ```
  ibmcloud ks cluster-feature-disable public-service-endpoint --cluster <cluster_name_or_ID>
  ```
   {: pre}

<br />


## 設定公用服務端點
{: #set-up-public-se}

啟用或停用叢集的公用服務端點。
{: shortdesc}

公用服務端點讓您的 Kubernetes 主節點可供公開存取。工作者節點和授權叢集使用者可以透過公用網路與 Kubernetes 主節點安全地通訊。如需相關資訊，請參閱[工作者節點到主節點的通訊以及使用者到主節點的通訊](/docs/containers?topic=containers-plan_clusters#internet-facing)。

**啟用的步驟**</br>
如果先前停用了公用端點，則可以重新啟用該端點。
1. 啟用公用服務端點。
   ```
  ibmcloud ks cluster-feature-enable public-service-endpoint --cluster <cluster_name_or_ID>
  ```
   {: pre}
2. 重新整理 Kubernetes 主節點 API 伺服器，以使用公用服務端點。您可以遵循 CLI 中的提示，或手動執行下列指令。
   ```
        ibmcloud ks apiserver-refresh --cluster <cluster_name_or_ID>
        ```
   {: pre}

   </br>

**停用的步驟**</br>
若要停用公用服務端點，您必須先啟用專用服務端點，讓工作者節點可以與 Kubernetes 主節點進行通訊。
1. 啟用專用服務端點。
   ```
  ibmcloud ks cluster-feature-enable private-service-endpoint --cluster <cluster_name_or_ID>
  ```
   {: pre}
2. 遵循 CLI 提示，或手動執行下列指令，以重新整理 Kubernetes 主節點 API 伺服器以使用專用服務端點。
   ```
        ibmcloud ks apiserver-refresh --cluster <cluster_name_or_ID>
        ```
   {: pre}
3. [建立 ConfigMap](/docs/containers?topic=containers-update#worker-up-configmap)，以控制叢集裡可能在某個時間無法使用的工作者節點數目上限。更新工作者節點時，ConfigMap 會協助避免應用程式的關閉，因為會依序將應用程式重新排定至可用的工作者節點。

4. 更新叢集裡的所有工作者節點，以反映專用服務端點配置。

   <p class="important">透過發出 update 指令，會重新載入工作者節點，以反映服務端點配置。如果沒有工作者節點更新項目可供使用，則必須[手動重新載入工作者節點](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli)。如果您重新載入，則務必隔離、排除及管理訂購，以控制在某個時間無法使用的工作者節點數目上限。</p>
   ```
ibmcloud ks worker-update --cluster <cluster_name_or_ID> --workers <worker1,worker2>
    ```
  {: pre}
5. 停用公用服務端點。
   ```
  ibmcloud ks cluster-feature-disable public-service-endpoint --cluster <cluster_name_or_ID>
  ```
   {: pre}

## 從公用服務端點切換至專用服務端點
{: #migrate-to-private-se}

在執行 Kubernetes 1.11 版或更新版本的叢集裡，啟用工作者節點透過專用網路以與主節點通訊，而非啟用專用服務端點以透過公用網路與主節點通訊。
{: shortdesc}

依預設，所有連接至公用和專用 VLAN 的叢集都會使用公用服務端點。工作者節點和授權叢集使用者可以透過公用網路與 Kubernetes 主節點安全地通訊。若要啟用工作者節點透過專用網路來與 Kubernetes 主節點通訊，而非透過公用網路，則可以啟用專用服務端點。然後，您可以選擇性地停用公用服務端點。
* 如果您啟用專用服務端點，並一併保持公用服務端點的啟用狀態，則工作者節點一律會透過專用網路與主節點進行通訊，但使用者可以透過公用或專用網路與主節點進行通訊。
* 如果您啟用專用服務端點，但停用公用服務端點，則工作者節點和使用者必須透過專用網路與主節點進行通訊。

請注意，專用服務端點在啟用之後，就無法予以停用。

1. 在您的 IBM Cloud 基礎架構 (SoftLayer) 帳戶中啟用 [VRF](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud)。
2. [啟用 {{site.data.keyword.Bluemix_notm}} 帳戶，以使用服務端點](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started)。
3. 啟用專用服務端點。
   ```
  ibmcloud ks cluster-feature-enable private-service-endpoint --cluster <cluster_name_or_ID>
  ```
   {: pre}
4. 遵循 CLI 提示，或手動執行下列指令，以重新整理 Kubernetes 主節點 API 伺服器以使用專用服務端點。
   ```
        ibmcloud ks apiserver-refresh --cluster <cluster_name_or_ID>
        ```
   {: pre}
5. [建立 configmap](/docs/containers?topic=containers-update#worker-up-configmap)，以控制叢集裡可能在某個時間無法使用的工作者節點數目上限。更新工作者節點時，ConfigMap 會協助避免應用程式的關閉，因為會依序將應用程式重新排定至可用的工作者節點。

6.  更新叢集裡的所有工作者節點，以反映專用服務端點配置。

    <p class="important">透過發出 update 指令，會重新載入工作者節點，以反映服務端點配置。如果沒有工作者節點更新項目可供使用，則必須[手動重新載入工作者節點](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli)。如果您重新載入，則務必隔離、排除及管理訂購，以控制在某個時間無法使用的工作者節點數目上限。</p>
    ```
    ibmcloud ks worker-update --cluster <cluster_name_or_ID> --workers <worker1,worker2>
    ```
    {: pre}

7. 選用項目：停用公用服務端點。
   ```
  ibmcloud ks cluster-feature-disable public-service-endpoint --cluster <cluster_name_or_ID>
  ```
   {: pre}

<br />


## 變更工作者節點 VLAN 連線
{: #change-vlans}

建立叢集時，您可以選擇要將工作者節點連接至專用和公用 VLAN，還是要連接至僅限專用 VLAN。工作者節點是儲存網路 meta 資料的工作者節點儲存區一部分，而網路 meta 資料包括用來佈建儲存區中未來工作者節點的 VLAN。建議您稍後變更叢集的 VLAN 連線功能設定，如下所示。
{: shortdesc}

* 區域中的工作者節點儲存區 VLAN 已耗盡容量，而且您需要針對要使用的叢集工作者節點佈建新的 VLAN。
* 您叢集的工作者節點同時位於公用和專用 VLAN，但您要變更為[僅限專用叢集](/docs/containers?topic=containers-plan_clusters#private_clusters)。
* 您具有僅限專用叢集，但想要一些工作者節點（例如公用 VLAN 上[邊緣節點](/docs/containers?topic=containers-edge#edge)的工作者節點儲存區），以在網際網路上公開應用程式。

嘗試改為變更服務端點以進行主節點與工作者節點的通訊嗎？請參閱[公用](#set-up-public-se)和[專用](#set-up-private-se)服務端點的設定主題。
{: tip}

開始之前：
* [登入您的帳戶。適用的話，請將適當的資源群組設為目標。設定叢集的環境定義。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
* 如果工作者節點是獨立式（不屬於工作者節點儲存區的一部分），請[將它們更新為工作者節點儲存區](/docs/containers?topic=containers-update#standalone_to_workerpool)。

若要變更工作者節點儲存區用來佈建工作者節點的 VLAN，請執行下列動作：

1. 列出叢集裡工作者節點儲存區的名稱。
  ```
   ibmcloud ks worker-pools --cluster <cluster_name_or_ID>
   ```
  {: pre}

2. 判定其中一個工作者節點儲存區的區域。在輸出中，尋找**區域**欄位。
  ```
  ibmcloud ks worker-pool-get --cluster <cluster_name_or_ID> --worker-pool <pool_name>
  ```
  {: pre}

3. 針對您在前一個步驟中找到的每個區域，取得彼此相容的可用公用和專用 VLAN。

  1. 檢查輸出中**類型**下所列出的可用公用和專用 VLAN。
    ```
   ibmcloud ks vlans --zone <zone>
   ```
     {: pre}

  2. 確認區域中的公用和專用 VLAN 是相容的。為了能夠相容，**路由器**必須具有相同的 Pod ID。在此範例輸出中，**路由器** Pod ID 相符：`01a` 和 `01a`。如果有一個 Pod ID 是 `01a`，而另一個是 `02a`，則您無法對工作者節點儲存區設定這些公用及專用 VLAN ID。
    ```
    ID        Name   Number   Type      Router         Supports Virtual Workers
    229xxxx          1234     private   bcr01a.dal12   true
    229xxxx          5678     public    fcr01a.dal12   true
    ```
     {: screen}

  3. 如果您需要訂購區域的新公用或專用 VLAN，則可以在 [{{site.data.keyword.Bluemix_notm}} 主控台](/docs/infrastructure/vlans?topic=vlans-ordering-premium-vlans#ordering-premium-vlans)中訂購，或使用下列指令。請記住，VLAN 必須與前一個步驟中的相符**路由器** Pod ID 相容。如果您建立一對新的公用和專用 VLAN，則它們必須彼此相容。
    ```
    ibmcloud sl vlan create -t [public|private] -d <zone> -r <compatible_router>
    ```
     {: pre}

  4. 記下相容 VLAN 的 ID。

4. 針對每個區域，使用新的 VLAN 網路 meta 資料來設定工作者節點儲存區。您可以建立新的工作者節點儲存區，或修改現有的工作者節點儲存區。

  * **建立新的工作者節點儲存區**：請參閱[建立新工作者節點儲存區來新增工作者節點](/docs/containers?topic=containers-add_workers#add_pool).

  * **修改現有的工作者節點儲存區**：針對每個區域，設定工作者節點儲存區的網路 meta 資料以使用 VLAN。已在儲存區中建立的工作者節點會繼續使用前一個 VLAN，但儲存區中的新工作者節點會使用您設定的新 VLAN meta 資料。

    * 同時新增公用和專用 VLAN 的範例，例如，如果您從僅限專用變更為專用及公用：
      ```
      ibmcloud ks zone-network-set --zone <zone> --cluster <cluster_name_or_ID> --worker-pools <pool_name> --private-vlan <private_vlan_id> --public-vlan <public_vlan_id>
      ```
      {: pre}

    * 僅新增專用 VLAN 的範例，例如，如果您在具有[已啟用 VRF 且使用服務端點的帳戶](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started)時從公用和專用 VLAN 變更為僅限專用：
      ```
      ibmcloud ks zone-network-set --zone <zone> --cluster <cluster_name_or_ID> --worker-pools <pool_name> --private-vlan <private_vlan_id> --public-vlan <public_vlan_id>
      ```
      {: pre}

5. 重新調整工作者節點儲存區大小，以將工作者節點新增至該儲存區。
   ```
    ibmcloud ks worker-pool-resize --cluster <cluster_name_or_ID> --worker-pool <pool_name>  --size-per-zone <number_of_workers_per_zone>
    ```
   {: pre}

   如果您要移除使用前一個網路 meta 資料的工作者節點，請將每個區域的工作者節點數目變更為每個區域的先前工作者節點數量的兩倍。稍後，您可以在這些步驟中隔離、排除及移除先前的工作者節點。
  {: tip}

6. 驗證使用輸出中的適當**公用 IP** 和**專用 IP** 來建立新的工作者節點。例如，如果您將工作者節點儲存區從公用和專用 VLAN 變更為僅限專用，則新的工作者節點只有專用 IP。如果您將工作者節點儲存區從僅限專用變更為公用和專用 VLAN，則新的工作者節點同時具有公用和專用 IP。
   ```
   ibmcloud ks workers --cluster <cluster_name_or_ID> --worker-pool <pool_name>
   ```
   {: pre}

7. 選用項目：從工作者節點儲存區移除具有先前網路 meta 資料的工作者節點。
  1. 在前一個步驟的輸出中，記下您要從工作者節點儲存區移除之工作者節點的 **ID** 和**專用 IP**。
  2. 在一個稱為隔離的處理程序中，將工作者節點標示為無法排程。當您隔離工作者節點時，會使它無法用於未來 Pod 排程。
    ```
    kubectl cordon <worker_private_ip>
    ```
     {: pre}
  3. 驗證已停用工作者節點的 Pod 排程。
    ```
    kubectl get nodes
    ```
     {: pre}
     如果狀態顯示 **`SchedulingDisabled`**，表示工作者節點已停用 Pod 排程。
  4. 強制從工作者節點移除 Pod，並將其重新排程至叢集裡的其餘工作者節點。
    ```
    kubectl drain <worker_private_ip>
    ```
     {: pre}
      此處理程序可能需要幾分鐘的時間。
  5. 移除工作者節點。使用您先前擷取的工作者節點 ID。
   ```
   ibmcloud ks worker-rm --cluster <cluster_name_or_ID> --worker <worker_name_or_ID>
   ```
     {: pre}
  6. 驗證已移除工作者節點。
    ```
        ibmcloud ks workers --cluster <cluster_name_or_ID> --worker-pool <pool_name>
        ```
     {: pre}

8. 選用項目：您可以針對叢集裡的每個工作者節點儲存區重複步驟 2 - 7。完成這些步驟之後，就會使用新的 VLAN 來設定叢集裡的所有工作者節點。

9. 叢集裡的預設 ALB 仍然會連結至舊的 VLAN，因為其 IP 位址來自該 VLAN 上的子網路。因為無法跨 VLAN 移動 ALB，所以您可以改為[在新的 VLAN 上建立 ALB，並在舊的 VLAN 上停用 ALB](/docs/containers?topic=containers-ingress#migrate-alb-vlan)。
