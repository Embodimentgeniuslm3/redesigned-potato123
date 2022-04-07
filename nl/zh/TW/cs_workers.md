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



# 將工作者節點及區域新增至叢集
{: #add_workers}

若要增加應用程式的可用性，您可以將工作者節點新增至叢集裡的現有區域或多個現有區域。為了協助保護應用程式使其免受區域故障影響，您可以將區域新增至叢集。
{:shortdesc}

當您建立叢集時，會在工作者節點儲存區中佈建工作者節點。在建立叢集之後，您可以藉由調整其大小或新增更多工作者節點儲存區，來將更多工作者節點新增至儲存區。依預設，工作者節點儲存區存在於一個區域中。一個區域中只有一個工作者節點儲存區的叢集稱為單一區域叢集。當您將更多區域新增至叢集時，工作者節點儲存區會存在於多個區域。具有分散到多個區域之工作者節點儲存區的叢集稱為多區域叢集。

如果您有多區域叢集，請保持其工作者節點資源的平衡。請確定所有工作者節點儲存區都分散到相同區域，並藉由調整儲存區大小來新增或移除工作者節點，而不是新增個別節點。
{: tip}

開始之前，請確定您具有[**操作員**或**管理者** {{site.data.keyword.Bluemix_notm}} IAM 平台角色](/docs/containers?topic=containers-users#platform)。然後，選擇下列其中一個區段：
  * [調整叢集裡現有工作者節點儲存區的大小來新增工作者節點](#resize_pool)
  * [將工作者節點儲存區新增至叢集來新增工作者節點](#add_pool)
  * [將區域新增至叢集，並將工作者節點儲存區中的工作者節點抄寫到多個區域](#add_zone)
  * [已淘汰：將獨立式工作者節點新增至叢集](#standalone)

設定工作者節點儲存區之後，您可以[設定叢集 autoscaler](/docs/containers?topic=containers-ca#ca)，以根據工作負載資源要求來自動新增或移除工作者節點儲存區中的工作者節點。
{:tip}

## 調整現有工作者節點儲存區的大小來新增工作者節點
{: #resize_pool}

不論工作者節點儲存區是在一個區域還是分散到多個區域，您都可以藉由調整現有工作者節點儲存區的大小來新增或減少叢集裡的工作者節點數目。
{: shortdesc}

例如，請考慮使用一個工作者節點儲存區的每個區域有三個工作者節點的叢集。
* 如果叢集是單一區域，並且存在於 `dal10` 中，則工作者節點儲存區在 `dal10` 中有三個工作者節點。叢集共有三個工作者節點。
* 如果叢集是多區域，並且存在於 `dal10` 及 `dal12` 中，則工作者節點儲存區在 `dal10` 中有三個工作者節點，而且三個工作者節點都在 `dal12` 中。叢集共有六個工作者節點。

針對裸機工作者節點儲存區，請謹記是按月計費。如果您向上或向下調整大小，則它會影響該月的成本。
{: tip}

若要調整工作者節點儲存區的大小，請變更工作者節點儲存區在每一個區域中部署的工作者節點數目：

1. 取得您要調整大小的工作者節點儲存區的名稱。
    ```
    ibmcloud ks worker-pools --cluster <cluster_name_or_ID>
    ```
    {: pre}

2. 指定您要在每一個區域中部署的工作者節點數目，以調整工作者節點儲存區的大小。最小值為 1。
    ```
    ibmcloud ks worker-pool-resize --cluster <cluster_name_or_ID> --worker-pool <pool_name>  --size-per-zone <number_of_workers_per_zone>
    ```
    {: pre}

3. 驗證已調整工作者節點儲存區的大小。
    ```
    ibmcloud ks workers --cluster <cluster_name_or_ID> --worker-pool <pool_name>
    ```
    {: pre}

    位於兩個區域（`dal10` 及 `dal12`）中且調整大小為每個區域有兩個工作者節點之工作者節點儲存區的輸出範例：
    ```
    ID                                                 Public IP        Private IP      Machine Type      State    Status  Zone    Version
    kube-dal10-crb20b637238ea471f8d4a8b881aae4962-w7   169.xx.xxx.xxx   10.xxx.xx.xxx   b3c.4x16          normal   Ready   dal10   1.8.6_1504
    kube-dal10-crb20b637238ea471f8d4a8b881aae4962-w8   169.xx.xxx.xxx   10.xxx.xx.xxx   b3c.4x16          normal   Ready   dal10   1.8.6_1504
    kube-dal12-crb20b637238ea471f8d4a8b881aae4962-w9   169.xx.xxx.xxx   10.xxx.xx.xxx   b3c.4x16          normal   Ready   dal12   1.8.6_1504
    kube-dal12-crb20b637238ea471f8d4a8b881aae4962-w10  169.xx.xxx.xxx   10.xxx.xx.xxx   b3c.4x16          normal   Ready   dal12   1.8.6_1504
    ```
    {: screen}

## 建立新工作者節點儲存區來新增工作者節點
{: #add_pool}

您可以建立新的工作者節點儲存區，以將工作者節點新增至叢集。
{:shortdesc}

1. 擷取叢集的**工作者節點區域**，並選擇要在工作者節點儲存區中部署工作者節點的區域。如果您具有單一區域叢集，則必須使用您在**工作者節點區域**欄位中看到的區域。對於多區域叢集，您可以選擇叢集的任何現有**工作者節點區域**，或為叢集所在的地區新增其中一個[多區域都會位置](/docs/containers?topic=containers-regions-and-zones#zones)。您可以藉由執行 `ibmcloud s zones` 來列出可用的區域。
   ```
   ibmcloud ks cluster-get --cluster <cluster_name_or_ID>
   ```
   {: pre}

   輸出範例：
   ```
   ...
   Worker Zones: dal10, dal12, dal13
   ```
   {: screen}

2. 針對每一個區域，列出可用的專用及公用 VLAN。請記下您要使用的專用及公用 VLAN。如果您沒有專用或公用 VLAN，則會在將區域新增至工作者節點儲存區時自動為您建立 VLAN。
   ```
   ibmcloud ks vlans --zone <zone>
   ```
   {: pre}

3.  針對每一個區域，檢閱[工作者節點的可用機型](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)。

    ```
    ibmcloud ks machine-types <zone>
    ```
    {: pre}

4. 建立工作者節點儲存區。包含 `--labels` 選項以使用 `key=value` 標籤自動標示儲存區中的工作者節點。如果您佈建祼機工作者節點儲存區，請指定 `--hardware dedicated`。
   ```
   ibmcloud ks worker-pool-create --name <pool_name> --cluster <cluster_name_or_ID> --machine-type <machine_type> --size-per-zone <number_of_workers_per_zone> --hardware <dedicated_or_shared> --labels <key=value>
   ```
   {: pre}

5. 驗證已建立工作者節點儲存區。
   ```
   ibmcloud ks worker-pools --cluster <cluster_name_or_ID>
   ```
   {: pre}

6. 依預設，新增工作者節點儲存區會建立沒有區域的儲存區。若要在區域中部署工作者節點，您必須將先前擷取的區域新增至工作者節點儲存區。如果您要將工作者節點分散到多個區域，請對每一個區域重複這個指令。
   ```
   ibmcloud ks zone-add --zone <zone> --cluster <cluster_name_or_ID> --worker-pools <pool_name> --private-vlan <private_VLAN_ID> --public-vlan <public_VLAN_ID>
   ```
   {: pre}

7. 驗證工作者節點佈建於您所新增的區域中。當狀態從 **provision_pending** 變更為 **normal** 時，表示您的工作者節點已備妥。
   ```
   ibmcloud ks workers --cluster <cluster_name_or_ID> --worker-pool <pool_name>
   ```
   {: pre}

   輸出範例：
   ```
   ID                                                 Public IP        Private IP      Machine Type      State    Status  Zone    Version
   kube-dal10-crb20b637238ea471f8d4a8b881aae4962-w7   169.xx.xxx.xxx   10.xxx.xx.xxx   b3c.4x16          provision_pending   Ready   dal10   1.8.6_1504
   kube-dal10-crb20b637238ea471f8d4a8b881aae4962-w8   169.xx.xxx.xxx   10.xxx.xx.xxx   b3c.4x16          provision_pending   Ready   dal10   1.8.6_1504
   ```
   {: screen}

## 將區域新增至工作者節點儲存區來新增工作者節點
{: #add_zone}

您可以跨越一個地區內跨多個區域的叢集，方法是將區域新增至現有工作者節點儲存區。
{:shortdesc}

當您將區域新增至工作者節點儲存區時，會在新的區域中佈建工作者節點儲存區中所定義的工作者節點，並考慮用於排定未來的工作負載。{{site.data.keyword.containerlong_notm}} 會自動將地區的 `failure-domain.beta.kubernetes.io/region` 標籤以及區域的 `failure-domain.beta.kubernetes.io/zone` 標籤新增至每一個工作者節點。Kubernetes 排程器使用這些標籤，以將 Pod 分散到相同地區內的區域。

如果您的叢集裡有多個工作者節點儲存區，請將該區域新增至所有這些儲存區，以將工作者節點平均地分散到叢集。

開始之前：
*  若要將區域新增至工作者節點儲存區，該工作者節點儲存區必須位於[具有多區域功能的區域](/docs/containers?topic=containers-regions-and-zones#zones)中。如果您的工作者節點儲存區不在具有多區域功能的區域中，請考慮[建立新工作者節點儲存區](#add_pool)。
*  如果您的叢集具有多個 VLAN、同一個 VLAN 上有多個子網路，或者是您具有多區域叢集，則必須為您的 IBM Cloud 基礎架構 (SoftLayer) 帳戶啟用[虛擬路由器功能 (VRF)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud)，讓工作者節點可以在專用網路上彼此通訊。若要啟用 VRF，[請與 IBM Cloud 基礎架構 (SoftLayer) 客戶代表聯絡](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#how-you-can-initiate-the-conversion)。如果您無法或不想要啟用 VRF，請啟用 [VLAN Spanning](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning)。若要執行此動作，您需要**網路 > 管理網路 VLAN Spanning** [基礎架構許可權](/docs/containers?topic=containers-users#infra_access)，或者您可以要求帳戶擁有者啟用它。若要確認是否已啟用 VLAN Spanning，請使用 `ibmcloud ks vlan-spanning-get --region <region>` [指令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get)。

若要將具有工作者節點的區域新增至工作者節點儲存區，請執行下列動作：

1. 列出可用的區域，並挑選您要新增至工作者節點儲存區的區域。您選擇的區域必須是具有多區域功能的區域。
   ```
   ibmcloud ks zones
   ```
   {: pre}

2. 列出該區域中的可用 VLAN。如果您沒有專用或公用 VLAN，則會在將區域新增至工作者節點儲存區時自動為您建立 VLAN。
   ```
   ibmcloud ks vlans --zone <zone>
   ```
   {: pre}

3. 列出叢集裡的工作者節點儲存區，並記下其名稱。
   ```
   ibmcloud ks worker-pools --cluster <cluster_name_or_ID>
   ```
   {: pre}

4. 將區域新增至工作者節點儲存區。如果您有多個工作者節點儲存區，請將區域新增至所有工作者節點儲存區，以平衡所有區域中的叢集。請將 `<pool1_id_or_name,pool2_id_or_name,...>` 取代為所有工作者節點儲存區的名稱（以逗點區隔的清單）。

    必須要有專用及公用 VLAN，才能將區域新增至多個工作者節點儲存區。如果您在該區域中沒有專用及公用 VLAN，請先將該區域新增至一個工作者節點儲存區，以為您建立專用及公用 VLAN。然後，您可以指定為您所建立的專用及公用 VLAN，以將該區域新增至其他工作者節點儲存區。
    {: note}

   如果您要對不同的工作者節點儲存區使用不同的 VLAN，則請針對每一個 VLAN 及其對應工作者節點儲存區重複這個指令。任何新的工作者節點都會新增至您指定的 VLAN，但不會變更任何現有工作者節點的 VLAN。
   {: tip}
   ```
   ibmcloud ks zone-add --zone <zone> --cluster <cluster_name_or_ID> --worker-pools <pool1_name,pool2_name,...> --private-vlan <private_VLAN_ID> --public-vlan <public_VLAN_ID>
   ```
   {: pre}

5. 驗證已將區域新增至叢集。在輸出的**工作者節點區域**欄位中，尋找已新增的區域。請注意，隨著在已新增的區域中佈建新工作者節點，**工作者節點**欄位中的工作者節點總數會增加。
  ```
  ibmcloud ks cluster-get --cluster <cluster_name_or_ID>
  ```
  {: pre}

  輸出範例：
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

## 已淘汰：新增獨立式工作者節點
{: #standalone}

如果您的叢集是在引進工作者節點儲存區之前所建立，則可以使用已淘汰的指令來新增獨立式工作者節點。
{: deprecated}

如果您的叢集是在引進工作者節點儲存區之後所建立，則無法新增獨立式工作者節點。相反地，您可以[建立工作者節點儲存區](#add_pool)、[調整現有工作者節點儲存區的大小](#resize_pool)，或[將區域新增至工作者節點儲存區](#add_zone)，以將工作者節點新增至叢集。
{: note}

1. 列出可用的區域，並挑選您要新增工作者節點的區域。
   ```
   ibmcloud ks zones
   ```
   {: pre}

2. 列出該區域中的可用 VLAN，並記下其 ID。
   ```
   ibmcloud ks vlans --zone <zone>
   ```
   {: pre}

3. 列出該區域中的可用機型。
   ```
   ibmcloud ks machine-types --zone <zone>
   ```
   {: pre}

4. 將獨立式工作者節點新增至叢集。若為裸機機型，請指定 `dedicated`。
   ```
   ibmcloud ks worker-add --cluster <cluster_name_or_ID> --workers <number_of_worker_nodes> --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID> --machine-type <machine_type> --hardware <shared_or_dedicated>
   ```
   {: pre}

5. 驗證已建立工作者節點。
   ```
   ibmcloud ks workers --cluster <cluster_name_or_ID>
   ```
   {: pre}

<br />


## 向現有工作者節點儲存區新增標籤
{: #worker_pool_labels}

可以在[建立工作者節點儲存區](#add_pool)時為工作者節點儲存區指派標籤，也可以日後透過執行下列步驟來指派標籤。在標示工作者節點儲存區後，所有現有及後續工作者節點都將獲得此標籤。您可使用標籤將特定工作負載僅部署到該工作者節點儲存區中的工作者節點，例如[用於負載平衡器網路資料流量的邊緣節點](/docs/containers?topic=containers-edge)。
{: shortdesc}

開始之前：[登入您的帳戶。適用的話，請將適當的資源群組設為目標。設定叢集的環境定義。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  列出叢集裡的工作者節點儲存區。
    ```
    ibmcloud ks worker-pools --cluster <cluster_name_or_ID>
    ```
    {: pre}
2.  若要使用 `key=value` 標籤來標示工作者節點儲存區，請使用 [PATCH 工作者節點儲存區 API ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/PatchWorkerPool)。按照下列 JSON 範例的格式，設定要求內文的格式。
    ```
    {
      "labels": {"key":"value"},
      "state": "labels"
    }
    ```
    {: codeblock}
3.  驗證工作者節點儲存區和工作者節點是否具有指派的 `key=value` 標籤。
    *   若要檢查工作者節點儲存區，請執行下列指令：
        ```
        ibmcloud ks worker-pool-get --cluster <cluster_name_or_ID> --worker-pool <worker_pool_name_or_ID>
        ```
        {: pre}
    *   若要檢查工作者節點，請執行下列動作：
        1.  列出工作者節點儲存區中的工作者節點，並記下**專用 IP**。
            ```
            ibmcloud ks workers --cluster <cluster_name_or_ID> --worker-pool <worker_pool_name_or_ID>
            ```
            {: pre}
        2.  檢閱輸出的 **Labels** 欄位。
            ```
            kubectl describe node <worker_node_private_IP>
            ```
            {: pre}

標示工作者節點儲存區後，可以[在應用程式部署中使用標籤](/docs/containers?topic=containers-app#label)，以便工作負載只能在這些工作者節點上執行，或者可以使用[污點 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/)，以阻止部署在這些工作者節點上執行。

<br />


## 工作者節點的自動回復
{: #planning_autorecovery}

重要元件（例如 `containerd`、`kubelet`、`kube-proxy` 及 `calico`）必須能夠運作，才能有健全的 Kubernetes 工作者節點。經過一段時間後，這些元件可能會中斷，而讓工作者節點處於無功能的狀況。無功能工作者節點會降低叢集的總容量，並可能導致應用程式關閉。
{:shortdesc}

您可以[為工作者節點配置性能檢查，並啟用自動回復](/docs/containers?topic=containers-health#autorecovery)。如果「自動回復」根據配置的檢查，偵測到性能不佳的工作者節點，則「自動回復」會觸發更正動作，如在工作者節點上重新載入 OS。如需自動回復運作方式的相關資訊，請參閱[自動回復部落格![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/blogs/bluemix/2017/12/autorecovery-utilizes-consistent-hashing-high-availability/)。
