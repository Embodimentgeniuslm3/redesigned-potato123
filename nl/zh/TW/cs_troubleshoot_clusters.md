---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-06"

keywords: kubernetes, iks, ImagePullBackOff, registry, image, failed to pull image,

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


# 叢集和工作者節點的疑難排解
{: #cs_troubleshoot_clusters}

在您使用 {{site.data.keyword.containerlong}} 時，請考慮使用這些技術來進行叢集和工作者節點的疑難排解。
{: shortdesc}

<p class="tip">如果您有更一般性的問題，請嘗試[叢集除錯](/docs/containers?topic=containers-cs_troubleshoot)。
<br>此外，疑難排解時，您可以使用 [{{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool](/docs/containers?topic=containers-cs_troubleshoot#debug_utility) 來執行測試，並從叢集收集相關資訊。</p>

## 許可權錯誤導致無法建立叢集或管理工作者節點
{: #cs_credentials}

{: tsSymptoms}
您嘗試透過執行下列其中一個指令，管理新叢集或現有叢集的工作者節點。
* 佈建工作者節點：`ibmcloud ks cluster-create`、`ibmcloud ks worker-pool-rebalance` 或 `ibmcloud ks worker-pool-resize`
* 重新載入工作者節點：`ibmcloud ks worker-reload` 或 `ibmcloud ks worker-update`
* 重新開機工作者節點：`ibmcloud ks worker-reboot`
* 刪除工作者節點：`ibmcloud ks cluster-rm`、`ibmcloud ks worker-rm`、`ibmcloud ks worker-pool-rebalance` 或 `ibmcloud ks worker-pool-resize`

不過，您會收到與下列其中一則類似的錯誤訊息。

```
We were unable to connect to your IBM Cloud infrastructure (SoftLayer) account.
Creating a standard cluster requires that you have either a
Pay-As-You-Go account that is linked to an IBM Cloud infrastructure (SoftLayer)
account term or that you have used the {{site.data.keyword.containerlong_notm}}
CLI to set your {{site.data.keyword.Bluemix_notm}} Infrastructure API keys.
```
{: screen}

```
{{site.data.keyword.Bluemix_notm}} Infrastructure Exception:
'Item' must be ordered with permission.
```
{: screen}

```
找不到工作者節點。請檢閱 {{site.data.keyword.Bluemix_notm}} 基礎架構許可權。
```
{: screen}

```
{{site.data.keyword.Bluemix_notm}} Infrastructure Exception:
The user does not have the necessary {{site.data.keyword.Bluemix_notm}}
Infrastructure permissions to add servers
```
{: screen}

```
IAM token exchange request failed: Cannot create IMS portal token, as no IMS account is linked to the selected BSS account
```
{: screen}

```
無法使用登錄來配置叢集。請確定您具有 {{site.data.keyword.registrylong_notm}} 的「管理者」角色。
```
{: screen}

{: tsCauses}
對地區和資源群組設定的基礎架構認證遺漏適當的[基礎架構許可權](/docs/containers?topic=containers-access_reference#infra)。使用者的基礎架構許可權最常儲存為地區和資源群組的 [API 金鑰](/docs/containers?topic=containers-users#api_key)。較少見的情況是，如果使用[其他 {{site.data.keyword.Bluemix_notm}} 帳戶類型](/docs/containers?topic=containers-users#understand_infra)，則您可能已 [手動設定基礎架構認證](/docs/containers?topic=containers-users#credentials)。如果您使用不同的 IBM Cloud 基礎架構 (SoftLayer) 帳戶來佈建基礎架構資源，則您的帳戶中也可能有[孤立叢集](#orphaned)。

{: tsResolve}
帳戶擁有者必須適當地設定基礎架構認證帳戶。認證取決於您所使用的基礎架構帳戶類型。

開始之前，[登入到您的帳戶。適用的話，請將適當的資源群組設為目標。為叢集設定環境定義。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  確定用於地區和資源群組的基礎架構許可權的使用者認證。
    1.  檢查叢集的地區和資源群組的 API 金鑰。
        ```
        ibmcloud ks api-key-info --cluster <cluster_name_or_ID>
        ```
        {: pre}

        輸出範例：
        ```
        Getting information about the API key owner for cluster <cluster_name>...
        OK
        Name                Email   
        <user_name>         <name@email.com>
        ```
        {: screen}
    2.  檢查地區和資源群組的基礎架構帳戶是否已手動設定為使用其他 IBM Cloud 基礎架構 (SoftLayer) 帳戶。
        ```
        ibmcloud ks credential-get --region <us-south>
        ```
        {: pre}

        **認證設定為使用其他帳戶時的輸出範例**。在這種情況下，使用者的基礎架構認證將用於目標地區和資源群組，即使在上一步中擷取到的 API 金鑰中已儲存其他使用者的認證也是如此。
        ```
        OK
        Infrastructure credentials for user name <1234567_name@email.com> set for resource group <resource_group_name>.
        ```
        {: screen}

        **認證未設定為使用其他帳戶時的輸出範例**。在這種情況下，在上一步中擷取到的 API 金鑰擁有者具有用於地區和資源群組的基礎架構認證。
        ```
        FAILED
        No credentials set for resource group <resource_group_name>.: The user credentials could not be found. (E0051)
        ```
        {: screen}
2.  驗證使用者擁有的基礎架構許可權。
    1.  列出地區和資源群組的建議和必要的基礎架構許可權。
        ```
        ibmcloud ks infra-permissions-get --region <region>
        ```
        {: pre}
    2.  確保 [API 金鑰的基礎架構認證擁有者或手動設定的帳戶具有正確的許可權](/docs/containers?topic=containers-users#owner_permissions)。
    3.  如果需要，可以變更地區和資源群組的 [API 金鑰](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_api_key_reset)或[手動設定](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_set)的基礎架構認證擁有者。
3.  測試變更後的許可權是否允許授權使用者對叢集執行基礎架構作業。
    1.  例如，您可嘗試刪除工作者節點。
        ```
        ibmcloud ks worker-rm --cluster <cluster_name_or_ID> --worker <worker_node_ID>
        ```
        {: pre}
    2.  檢查以確定是否已移除工作者節點。
        ```
        ibmcloud ks worker-get --cluster <cluster_name_or_ID> --worker <worker_node_ID>
        ```
        {: pre}

        順利移除工作者節點時的輸出範例。由於刪除了工作者節點，因此 `worker-get` 作業會失敗。這說明基礎架構許可權已正確設定。
        ```
        FAILED
        The specified worker node could not be found. (E0011)
        ```
        {: screen}

    3.  如果未移除工作者節點，請檢閱[**狀況**和**狀態**欄位](/docs/containers?topic=containers-cs_troubleshoot#debug_worker_nodes)以及[工作者節點的常見問題](/docs/containers?topic=containers-cs_troubleshoot#common_worker_nodes_issues)，以繼續進行除錯。
    4.  如果已手動設定認證，但在基礎架構帳戶中仍然看不到叢集的工作者節點，則可以檢查是否[叢集為孤立叢集](#orphaned)。

<br />


## 防火牆阻止執行 CLI 指令
{: #ts_firewall_clis}

{: tsSymptoms}
當您從 CLI 執行 `ibmcloud`、`kubectl` 或 `calicoctl` 指令時，它們會失敗。

{: tsCauses}
您的組織網路原則可能會導致無法透過 Proxy 或防火牆從本端系統存取公用端點。

{: tsResolve}
[容許 TCP 存取以讓 CLI 指令運作](/docs/containers?topic=containers-firewall#firewall_bx)。此作業需要叢集的[**管理者** {{site.data.keyword.Bluemix_notm}} IAM 平台角色](/docs/containers?topic=containers-users#platform)。


## 無法存取我的叢集裡的資源
{: #cs_firewall}

{: tsSymptoms}
當您叢集裡的工作者節點無法在專用網路上進行通訊時，您可能會看到各種不同的症狀。

- 當您執行 `kubectl exec`、`attach`、`logs`、`proxy` 或 `port-forward` 時的錯誤訊息範例：
  ```
  Error from server: error dialing backend: dial tcp XXX.XXX.XXX:10250: getsockopt: connection timed out
  ```
  {: screen}

- `kubectl proxy` 成功，但 Kubernetes 儀表板無法使用時的錯誤訊息範例：
  ```
  timeout on 172.xxx.xxx.xxx
  ```
  {: screen}

- `kubectl proxy` 失敗或服務連線失敗時的錯誤訊息範例：
  ```
  Connection refused
  ```
  {: screen}

  ```
  Connection timed out
  ```
  {: screen}

  ```
  Unable to connect to the server: net/http: TLS handshake timeout
  ```
  {: screen}


{: tsCauses}
若要存取叢集裡的資源，您的工作者節點必須能夠在專用網路上進行通訊。您可能已在 IBM Cloud 基礎架構 (SoftLayer) 帳戶中設定 Vyatta 或另一個防火牆，或自訂現有的防火牆設定。{{site.data.keyword.containerlong_notm}} 需要開啟特定 IP 位址及埠，以容許從工作者節點到 Kubernetes 主節點的通訊，反之亦然。如果您的工作者節點分散在多個區域，您必須透過啟用 VLAN Spanning 來容許專用網路通訊。如果您的工作者節點停留在重新載入的迴圈中，則也可能無法在工作者節點之間進行通訊。

{: tsResolve}
1. 列出叢集裡的工作者節點，並驗證您的工作者節點未停留在 `Reloading` 狀態中。
   ```
  ibmcloud ks workers --cluster <cluster_name_or_id>
  ```
   {: pre}

2. 如果您具有多區域叢集，且未針對 VRF 啟用您的帳戶，請驗證您的帳戶是否 [已啟用 VLAN Spanning](/docs/containers?topic=containers-subnets#subnet-routing)。
3. 如果您有 Vyatta 或自訂防火牆設定，請確定[已開啟必要的埠](/docs/containers?topic=containers-firewall#firewall_outbound)，以容許叢集存取基礎架構資源和服務。

<br />



## 無法檢視或使用叢集
{: #cs_cluster_access}

{: tsSymptoms}
* 您找不到叢集。當您執行 `ibmcloud ks clusters` 時，該叢集未列在輸出中。
* 您無法使用叢集。當您執行 `ibmcloud ks cluster-config` 或其他叢集特定指令時，找不到叢集。


{: tsCauses}
在 {{site.data.keyword.Bluemix_notm}} 中，每個資源都必須位於資源群組中。例如，叢集 `mycluster` 可能存在於 `default` 資源群組中。當帳戶擁有者藉由向您指派 {{site.data.keyword.Bluemix_notm}} IAM 平台角色來將資源存取權授與您時，該存取權可以是資源特有的或屬於資源群組。當您獲授與特定資源的存取權時，則無法存取資源群組。在此情況下，您不需要將目標設為資源群組，即可使用您可以存取的叢集。如果您的目標資源群組與叢集所在的群組不同，則該叢集的動作可能會失敗。反之，當您在存取資源群組過程中獲授與資源的存取權時，必須將目標設為資源群組，才能使用該群組中的叢集。如果您未將 CLI 階段作業的目標設為叢集所在的資源群組，則該叢集的動作可能會失敗。

如果您找不到或無法使用叢集，則可能遭遇下列其中一個問題：
* 您可以存取叢集及叢集所在的資源群組，但 CLI 階段作業的目標不是叢集所在的資源群組。
* 您可以存取叢集，但不是叢集所在資源群組的一部分。CLI 階段作業的目標設為這個或另一個資源群組。
* 您沒有叢集的存取權。

{: tsResolve}
若要檢查使用者存取權，請執行下列動作：

1. 列出所有使用者許可權。
    ```
    ibmcloud iam user-policies <your_user_name>
    ```
    {: pre}

2. 檢查您是否可以存取叢集及叢集所在的資源群組。
    1. 尋找具有下列項目的原則：**Resource Group Name** 值為叢集資源群組，且 **Memo** 值為 `Policy applies to the resource group`。如果您具有此原則，則可以存取資源群組。例如，此原則指出使用者可以存取 `test-rg` 資源群組：
        ```
        Policy ID:   3ec2c069-fc64-4916-af9e-e6f318e2a16c
        Roles:       Viewer
        Resources:
                     Resource Group ID     50c9b81c983e438b8e42b2e8eca04065
                     Resource Group Name   test-rg
                     Memo                  Policy applies to the resource group
        ```
        {: screen}
    2. 尋找具有下列項目的原則：**Resource Group Name** 值為叢集資源群組、**Service Name** 值為 `containers-kubernetes` 或沒有值，且 **Memo** 值為 `Policy applies to the resource(s) within the resource group`。如果您具有此原則，則可以存取叢集或是資源群組內的所有資源。例如，此原則指出使用者可以存取 `test-rg` 資源群組中的叢集：
        ```
        Policy ID:   e0ad889d-56ba-416c-89ae-a03f3cd8eeea
        Roles:       Administrator
        Resources:
                     Resource Group ID     a8a12accd63b437bbd6d58fb6a462ca7
                     Resource Group Name   test-rg
                     Service Name          containers-kubernetes
                     Service Instance
                     Region
                     Resource Type
                     Resource
                     Memo                  Policy applies to the resource(s) within the resource group
        ```
        {: screen}
    3. 如果您具有這兩個原則，則請跳到步驟 4（第一個項目符號）。如果您沒有步驟 2a 中的原則，但您具有步驟 2b 中的原則，則請跳到步驟 4（第二個項目符號）。如果您沒有其中任一個原則，請繼續進行步驟 3。

3. 檢查您是否可以存取叢集，但不是作為叢集所在資源群組存取權的一部分。
    1. 尋找沒有 **Policy ID** 及 **Roles** 欄位以外值的原則。如果您具有此原則，則可以在存取整個帳戶的過程中存取叢集。例如，此原則指出使用者可以存取帳戶中的所有資源：
        ```
        Policy ID:   8898bdfd-d520-49a7-85f8-c0d382c4934e
        Roles:       Administrator, Manager
        Resources:
                     Service Name
                     Service Instance
                     Region
                     Resource Type
                     Resource
        ```
        {: screen}
    2. 尋找具有下列項目的原則：**Service Name** 值為 `containers-kubernetes`，且 **Service Instance** 值為叢集 ID。您可以執行 `ibmcloud ks cluster-get --cluster <cluster_name>` 來尋找叢集 ID。例如，此原則指出使用者可以存取特定叢集：
        ```
        Policy ID:   140555ce-93ac-4fb2-b15d-6ad726795d90
        Roles:       Administrator
        Resources:
                     Service Name       containers-kubernetes
                     Service Instance   df253b6025d64944ab99ed63bb4567b6
                     Region
                     Resource Type
                     Resource
        ```
        {: screen}
    3. 如果您具有其中任一個原則，請跳到步驟 4 的第二個項目符號點。如果您沒有其中任一個原則，請跳到步驟 4 的第三個項目符號點。

4. 根據您的存取原則，選擇下列其中一個選項。
    * 如果您可以存取叢集及叢集所在的資源群組，請執行下列動作：
      1. 將目標設為資源群組。**附註**：除非您取消將此資源群組設為目標，否則無法使用其他資源群組中的叢集。
          ```
          ibmcloud target -g <resource_group>
          ```
          {: pre}

      2. 將目標設為叢集。
          ```
          ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
          ```
          {: pre}

    * 如果您可以存取叢集但無法存取叢集所在的資源群組，請執行下列動作：
      1. 不要將資源群組設為目標。如果您已將資源群組設為目標，則請取消將它設為目標：
        ```
        ibmcloud target --unset-resource-group
        ```
        {: pre}

      2. 將目標設為叢集。
          ```
          ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
          ```
        {: pre}

    * 如果您沒有叢集的存取權，請執行下列動作：
        1. 要求帳戶擁有者將該叢集的 [{{site.data.keyword.Bluemix_notm}} IAM 平台角色](/docs/containers?topic=containers-users#platform)指派給您。
        2. 不要將資源群組設為目標。如果您已將資源群組設為目標，則請取消將它設為目標：
        ```
          ibmcloud target --unset-resource-group
          ```
          {: pre}
        3. 將目標設為叢集。
          ```
          ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
          ```
          {: pre}

<br />


## 使用 SSH 存取工作者節點失敗
{: #cs_ssh_worker}

{: tsSymptoms}
您無法使用 SSH 連線來存取工作者節點。

{: tsCauses}
工作者節點上無法使用透過密碼的 SSH。

{: tsResolve}
針對您必須在每個節點上執行的動作使用 Kubernetes [`DaemonSet` ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/)，或針對您必須執行的一次性動作使用工作。

<br />


## 裸機實例 ID 與工作者節點記錄不一致
{: #bm_machine_id}

{: tsSymptoms}
使用 `ibmcloud ks worker` 指令與裸機工作者節點搭配時，您會看到與下列內容類似的訊息。

```
Instance ID inconsistent with worker records
```
{: screen}

{: tsCauses}
當機器遭遇硬體問題時，機器 ID 可能變成與 {{site.data.keyword.containerlong_notm}} 工作者節點記錄不一致。當 IBM Cloud 基礎架構 (SoftLayer) 解決此問題時，元件可在服務未識別的系統內變更。

{: tsResolve}
若要讓 {{site.data.keyword.containerlong_notm}} 重新識別機器，請[重新載入裸機工作者節點](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)。**附註**：重新載入也會更新機器的[修補程式版本](/docs/containers?topic=containers-changelog)。

您也可以 [刪除裸機工作者節點](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_rm)。**附註**：裸機實例依月計費。

<br />


## 無法修改或刪除孤立叢集裡的基礎架構
{: #orphaned}

{: tsSymptoms}
您無法在叢集上執行基礎架構相關指令，例如：
* 新增或移除工作者節點
* 重新載入或重新啟動工作者節點
* 重新調整工作者節點儲存區大小
* 更新叢集

您無法使用 IBM Cloud 基礎架構 (SoftLayer) 帳戶檢視叢集工作者節點。不過，您可以更新及管理帳戶中的其他叢集。

甚至，您已驗證具有[適當的基礎架構認證](#cs_credentials)。

{: tsCauses}
叢集可能是使用不再鏈結至 {{site.data.keyword.containerlong_notm}} 帳戶的 IBM Cloud 基礎架構 (SoftLayer) 帳戶所佈建。叢集是孤立的。因為資源位於不同的帳戶中，所以您沒有可修改資源的基礎架構認證。

請考量下列情境，以瞭解叢集如何變成孤立的。
1.  您具有 {{site.data.keyword.Bluemix_notm}}「隨收隨付制」帳戶。
2.  您建立名稱為 `Cluster1` 的叢集。工作者節點及其他基礎架構資源佈建至「隨收隨付制」帳戶隨附的基礎架構帳戶。
3.  稍後，您發現您的團隊使用舊式或共用的 IBM Cloud 基礎架構 (SoftLayer) 帳戶。您使用 `ibmcloud ks credential-set` 指令，將 IBM Cloud 基礎架構 (SoftLayer) 認證變更為使用您的團隊帳戶。
4.  您建立另一個名稱為 `Cluster2` 的叢集。工作者節點及其他基礎架構資源佈建至團隊基礎架構帳戶。
5.  您注意到 `Cluster1` 需要工作者節點更新、工作者節點重新載入，或您只想要藉由刪除它來予以清除。不過，因為 `Cluster1` 佈建至不同的基礎架構帳戶，所以您無法修改其基礎架構資源。`Cluster1` 是孤立的。
6.  您遵循下節中的解決步驟，但未將基礎架構認證設回團隊帳戶。您可以刪除 `Cluster1`，但現在 `Cluster2` 是孤立的。
7.  您將基礎架構認證變更回建立 `Cluster2` 的團隊帳戶。現在，您不再具有孤立叢集！

<br>

{: tsResolve}
1.  檢查您叢集所在的地區目前使用哪個基礎架構帳戶來佈建叢集。
    1.  登入 [{{site.data.keyword.containerlong_notm}} 叢集主控台 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/kubernetes/clusters)。
    2.  從表格中，選取您的叢集。
    3.  在**概觀**標籤中，檢查**基礎架構使用者**欄位。此欄位可協助您判斷 {{site.data.keyword.containerlong_notm}} 帳戶是否使用與預設值不同的基礎架構帳戶。
        * 如果您看不到**基礎架構使用者**欄位，表示您的已鏈結「隨收隨付制」帳戶會將相同的認證用於基礎架構及平台帳戶。無法修改的叢集可能佈建至不同的基礎架構帳戶。
        * 如果您看到**基礎架構使用者**欄位，表示使用不同的基礎架構帳戶，而不是「隨收隨付制」帳戶隨附的基礎架構帳戶。這些不同的認證適用於地區內的所有叢集。無法修改的叢集可能佈建至「隨收隨付制」或不同的基礎架構帳戶。
2.  檢查已使用哪個基礎架構帳戶來佈建叢集。
    1.  在**工作者節點**標籤中，選取工作者節點，並記下其 **ID**。
    2.  開啟功能表 ![功能表圖示](../icons/icon_hamburger.svg "功能表圖示")，然後按一下**標準基礎架構**。
    3.  從基礎架構導覽窗格中，按一下**裝置 > 裝置清單**。
    4.  搜尋您先前記下的工作者節點 ID。
    5.  如果您找不到工作者節點 ID，則未將工作者節點佈建至此基礎架構帳戶。請切換至不同的基礎架構帳戶，然後再試一次。
3.  使用 `ibmcloud ks credential-set` [指令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_set)，將基礎架構認證變更為在其中佈建叢集工作者節點的帳戶，而您可以在前一個步驟找到此帳戶。
    如果您無法再存取而且無法取得基礎架構認證，則必須開立 {{site.data.keyword.Bluemix_notm}} 支援案例以移除孤立叢集。
    {: note}
4.  [刪除叢集](/docs/containers?topic=containers-remove)。
5.  如果想要的話，請將基礎架構認證重設為前一個帳戶。請注意，如果您建立叢集所用的基礎架構帳戶與您切換至的帳戶不同，則可能會孤立那些叢集。
    * 若要將認證設為不同的基礎架構帳戶，請使用 `ibmcloud ks credential-set` [指令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_set)。
    * 若要使用 {{site.data.keyword.Bluemix_notm}} 隨收隨付制帳戶隨附的預設認證，請使用 `ibmcloud ks credential-unset --region <region>` [指令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_unset)。

<br />


## `kubectl` 指令逾時
{: #exec_logs_fail}

{: tsSymptoms}
如果您執行 `kubectl exec`、`kubectl attach`、`kubectl proxy`、`kubectl port-forward` 或 `kubectl logs` 這類指令，則會看到下列訊息。

  ```
  <workerIP>:10250: getsockopt: connection timed out
  ```
  {: screen}

{: tsCauses}
主節點與工作者節點之間的 OpenVPN 連線未正常運作。

{: tsResolve}
1. 如果您的叢集具有多個 VLAN、同一個 VLAN 上有多個子網路，或者是您具有多區域叢集，則必須為您的 IBM Cloud 基礎架構 (SoftLayer) 帳戶啟用[虛擬路由器功能 (VRF)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud)，讓工作者節點可以在專用網路上彼此通訊。若要啟用 VRF，[請與 IBM Cloud 基礎架構 (SoftLayer) 客戶代表聯絡](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#how-you-can-initiate-the-conversion)。如果您無法或不想要啟用 VRF，請啟用 [VLAN Spanning](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning)。若要執行此動作，您需要**網路 > 管理網路 VLAN Spanning** [基礎架構許可權](/docs/containers?topic=containers-users#infra_access)，或者您可以要求帳戶擁有者啟用它。若要確認是否已啟用 VLAN Spanning，請使用 `ibmcloud ks vlan-spanning-get --region <region>` [指令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get)。
2. 重新啟動 OpenVPN 用戶端 Pod。
  ```
  kubectl delete pod -n kube-system -l app=vpn
  ```
  {: pre}
3. 如果您仍看到相同的錯誤訊息，則 VPN Pod 所在的工作者節點可能性能不佳。若要重新啟動 VPN Pod 並將它排定到不同的工作者節點，[請隔離、排除及重新啟動工作者節點](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reboot)，而 VPN Pod 位於該工作者節點上。

<br />


## 將服務連結至叢集導致相同名稱錯誤
{: #cs_duplicate_services}

{: tsSymptoms}
當您執行 `ibmcloud ks cluster-service-bind --cluster <cluster_name> --namespace <namespace> --service <service_instance_name>` 時，會看到下列訊息。

```
Multiple services with the same name were found.
Run 'ibmcloud service list' to view available Bluemix service instances...
```
{: screen}

{: tsCauses}
多個服務實例可能在不同的地區中具有相同的名稱。

{: tsResolve}
請在 `ibmcloud ks cluster-service-bind` 指令中使用服務 GUID，而不要使用服務實例名稱。

1. [登入包含要連結之服務實例的 {{site.data.keyword.Bluemix_notm}} 地區。](/docs/containers?topic=containers-regions-and-zones#bluemix_regions)

2. 取得服務實例的 GUID。
  ```
  ibmcloud service show <service_instance_name> --guid
  ```
  {: pre}

  輸出：
  ```
  Invoking 'cf service <service_instance_name> --guid'...
  <service_instance_GUID>
  ```
  {: screen}
3. 再次將服務連結至叢集。
  ```
  ibmcloud ks cluster-service-bind --cluster <cluster_name> --namespace <namespace> --service <service_instance_GUID>
  ```
  {: pre}

<br />


## 將服務連結至叢集導致「找不到服務」錯誤
{: #cs_not_found_services}

{: tsSymptoms}
當您執行 `ibmcloud ks cluster-service-bind --cluster <cluster_name> --namespace <namespace> --service <service_instance_name>` 時，會看到下列訊息。

```
Binding service to a namespace...
FAILED

The specified IBM Cloud service could not be found. If you just created the service, wait a little and then try to bind it again. To view available IBM Cloud service instances, run 'ibmcloud service list'. (E0023)
```
{: screen}

{: tsCauses}
若要將服務連結至叢集，您必須具有佈建服務實例之空間的 Cloud Foundry 開發人員使用者角色。此外，您還必須對 {{site.data.keyword.containerlong}} 具有 {{site.data.keyword.Bluemix_notm}} IAM「編輯者」平台存取權。若要存取服務實例，您必須登入佈建服務實例的空間。

{: tsResolve}

**身為使用者：**

1. 登入 {{site.data.keyword.Bluemix_notm}}。
   ```
   ibmcloud login
   ```
   {: pre}

2. 將佈建服務實例的組織及空間設為目標。
   ```
   ibmcloud target -o <org> -s <space>
   ```
   {: pre}

3. 列出您的服務實例，驗證您位在正確的空間中。
   ```
   ibmcloud service list
   ```
   {: pre}

4. 嘗試重新連結服務。如果您收到相同的錯誤，則請聯絡帳戶管理者，並驗證您有足夠的許可權可連結服務（請參閱下列帳戶管理者步驟）。

**身為帳戶管理者：**

1. 驗證發生此問題的使用者具有 [{{site.data.keyword.containerlong}} 的編輯者權限](/docs/iam?topic=iam-iammanidaccser#edit_existing)。

2. 驗證發生此問題的使用者具有佈建服務之[空間的 Cloud Foundry 開發人員角色](/docs/iam?topic=iam-mngcf#update_cf_access)。

3. 如果有正確的許可權，則請嘗試指派不同的許可權，然後重新指派必要的許可權。

4. 等待幾分鐘，然後讓使用者嘗試重新連結服務。

5. 如果這樣做無法解決問題，則 {{site.data.keyword.Bluemix_notm}} IAM 許可權會不同步，且您無法自行解決問題。請開立支援案例，以[與 IBM 支援中心聯絡](/docs/get-support?topic=get-support-getting-customer-support)。請務必提供叢集 ID、使用者 ID 及服務實例 ID。
   1. 擷取叢集 ID。
      ```
      ibmcloud ks clusters
      ```
      {: pre}

   2. 擷取服務實例 ID。
      ```
      ibmcloud service show <service_name> --guid
      ```
      {: pre}


<br />


## 將服務連結至叢集導致服務不支援服務金鑰錯誤
{: #cs_service_keys}

{: tsSymptoms}
當您執行 `ibmcloud ks cluster-service-bind --cluster <cluster_name> --namespace <namespace> --service <service_instance_name>` 時，會看到下列訊息。

```
This service doesn't support creation of keys
```
{: screen}

{: tsCauses}
{{site.data.keyword.Bluemix_notm}} 中的部分服務（例如 {{site.data.keyword.keymanagementservicelong}}）不支援建立服務認證（也稱為服務金鑰）。如果不支援服務金鑰，則服務無法連結至叢集。若要尋找支援建立服務金鑰的服務清單，請參閱[啟用外部應用程式以使用 {{site.data.keyword.Bluemix_notm}} 服務](/docs/resources?topic=resources-externalapp#externalapp)。

{: tsResolve}
若要整合不支援服務金鑰的服務，請確認服務是否提供您可用來從應用程式直接存取服務的 API。例如，如果您要使用 {{site.data.keyword.keymanagementservicelong}}，請參閱 [API 參考資料 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/apidocs/kms?language=curl)。

<br />


## 工作者節點更新或重新載入之後，出現重複的節點及 Pod
{: #cs_duplicate_nodes}

{: tsSymptoms}
當您執行 `kubectl get nodes` 時，會看到重複的工作者節點，並顯示狀態 **`NotReady`**。具有 **`NotReady`** 的工作者節點具有公用 IP 位址，而具有 **`Ready`** 的工作者節點則具有專用 IP 位址。

{: tsCauses}
較舊的叢集會依叢集的公用 IP 位址列出工作者節點。現在，工作者節點會依叢集的專用 IP 位址列出。當您重新載入或更新節點時，IP 位址會變更，但對公用 IP 位址的參照則照舊。

{: tsResolve}
不會因這些重複項目而中斷服務，但您可以從 API 伺服器移除舊的工作者節點參照。

  ```
  kubectl delete node <node_name1> <node_name2>
  ```
  {: pre}

<br />


## 在新工作者節點上存取 Pod 因逾時而失敗
{: #cs_nodes_duplicate_ip}

{: tsSymptoms}
您已刪除叢集裡的工作者節點，然後新增工作者節點。若您已部署 Pod 或 Kubernetes 服務，資源即無法存取新建立的工作者節點，且連線逾時。

{: tsCauses}
如果您從叢集刪除工作者節點，然後新增工作者節點，則可能會將已刪除工作者節點的專用 IP 位址指派給新的工作者節點。Calico 使用此專用 IP 位址作為標籤，並繼續嘗試連接已刪除的節點。

{: tsResolve}
手動更新專用 IP 位址的參照，以指向正確的節點。

1.  確認您有兩個具有相同**專用 IP** 位址的工作者節點。請記下已刪除之工作者節點的**專用 IP** 及 **ID**。

  ```
  ibmcloud ks workers --cluster <cluster_name_or_id>
  ```
  {: pre}

  ```
  ID                                                 Public IP       Private IP       Machine Type   State     Status   Zone   Version
  kube-dal10-cr9b7371a7fcbe46d08e04f046d5e6d8b4-w1   169.xx.xxx.xxx  10.xxx.xx.xxx    b3c.4x16       normal    Ready    dal10      1.13.6
  kube-dal10-cr9b7371a7fcbe46d08e04f046d5e6d8b4-w2   169.xx.xxx.xxx  10.xxx.xx.xxx    b3c.4x16       deleted    -       dal10      1.13.6
  ```
  {: screen}

2.  安裝 [Calico CLI](/docs/containers?topic=containers-network_policies#adding_network_policies)。
3.  列出 Calico 中的可用工作者節點。請將 <path_to_file> 取代為 Calico 配置檔的本端路徑。

  ```
  calicoctl get nodes --config=filepath/calicoctl.cfg
  ```
  {: pre}

  ```
  NAME
  kube-dal10-cr9b7371a7faaa46d08e04f046d5e6d8b4-w1
  kube-dal10-cr9b7371a7faaa46d08e04f046d5e6d8b4-w2
  ```
  {: screen}

4.  刪除 Calico 中的重複工作者節點。請將 NODE_ID 取代為工作者節點 ID。

  ```
  calicoctl delete node NODE_ID --config=<path_to_file>/calicoctl.cfg
  ```
  {: pre}

5.  將未刪除的工作者節點重新開機。

  ```
  ibmcloud ks worker-reboot --cluster <cluster_name_or_id> --worker <worker_id>
  ```
  {: pre}


已刪除的節點不再列於 Calico 中。

<br />




## 因為 Pod 安全原則，Pod 無法部署
{: #cs_psp}

{: tsSymptoms}
在建立 Pod 或執行 `kubectl get events` 來檢查 Pod 部署之後，您會看到與下列內容類似的錯誤訊息。

```
unable to validate against any Pod security policy
```
{: screen}

{: tsCauses}
[`PodSecurityPolicy` 許可控制器](/docs/containers?topic=containers-psp)會檢查嘗試建立 Pod 之使用者或服務帳戶（例如，開發或 Helm Tiller）的授權。如果沒有 Pod 安全原則支援使用者或服務帳戶，`PodSecurityPolicy` 許可控制器會阻止建立 Pod。

如果您已刪除 [{{site.data.keyword.IBM_notm}} 叢集管理](/docs/containers?topic=containers-psp#ibm_psp)的其中一個 Pod 安全原則資源，則可能遭遇類似問題。

{: tsResolve}
確定使用者或服務帳戶已獲得 Pod 安全原則授權。您可能需要[修改現有原則](/docs/containers?topic=containers-psp#customize_psp)。

如果您已刪除 {{site.data.keyword.IBM_notm}} 叢集管理資源，請重新整理 Kubernetes 主節點來還原它。

1.  [登入您的帳戶。適用的話，請將適當的資源群組設為目標。設定叢集的環境定義。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
2.  重新整理 Kubernetes 主節點來還原它。

    ```
    ibmcloud ks apiserver-refresh
    ```
    {: pre}


<br />




## 叢集保持在擱置狀況
{: #cs_cluster_pending}

{: tsSymptoms}
當您部署叢集時，它保持在擱置狀況，且未啟動。

{: tsCauses}
如果您才剛建立叢集，則可能仍在配置工作者節點。如果您已等待一段時間，則可能會有無效的 VLAN。

{: tsResolve}

您可以嘗試下列其中一種解決方案：
  - 執行 `ibmcloud ks clusters`，來檢查叢集的狀態。然後，執行 `ibmcloud ks workers --cluster <cluster_name>`，來檢查並確定已部署工作者節點。
  - 查看您的 VLAN 是否有效。VLAN 若要有效，則必須與基礎架構相關聯，且該基礎架構可以管理具有本端磁碟儲存空間的工作者節點。您可以執行 `ibmcloud ks vlans --zone <zone>` 來[列出 VLAN](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlans)，如果 VLAN 未顯示在清單中，則它是無效的。請選擇其他 VLAN。

<br />


## 叢集建立錯誤，無法從登錄中取回映像檔
{: #ts_image_pull_create}

{: tsSymptoms}
建立叢集時，收到了類似於下列內容的錯誤訊息。


```
由於無法建立 IAM 存取原則，因此叢集無法從 IBM Cloud Container Registry "icr.io" 網域中取回映像檔。請確保您具有對 IBM Cloud Container Registry 的 IAM 管理者平台角色。然後，透過執行 "ibmcloud ks cluster-pre-secret-apply" 以使用登錄的 IAM 認證建立映像檔取回密碼。
```
{: screen}

{: tsCauses}
在叢集建立期間，會為叢集建立服務 ID，並為該 ID 指派對 {{site.data.keyword.registrylong_notm}} 的**讀者**服務存取原則。然後，會產生此服務 ID 的 API 金鑰，並將其儲存在[映像檔取回密碼](/docs/containers?topic=containers-images#cluster_registry_auth)中，以授權叢集從 {{site.data.keyword.registrylong_notm}} 取回映像檔。

若要在叢集建立期間為服務 ID 順利指派**讀者**服務存取原則，您必須具有對 {{site.data.keyword.registrylong_notm}} 的**管理者**平台存取原則。

{: tsResolve}

步驟：
1.  確保帳戶擁有者授予您 {{site.data.keyword.registrylong_notm}} 的**管理者**角色。
    ```
        ibmcloud iam user-policy-create <your_user_email> --service-name container-registry --roles Administrator
        ```
    {: pre}
2.  [使用 `ibmcloud ks cluster-pull-secret-apply` 指令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_pull_secret_apply)以使用適當的登錄認證重建映像檔取回密碼。

<br />


## 未能從登錄中取回映像檔，發生 `ImagePullBackOff` 或授權錯誤
{: #ts_image_pull}

{: tsSymptoms}

當您部署工作負載從 {{site.data.keyword.registrylong_notm}} 取回映像檔時，您的 Pod 會失敗並處於 **`ImagePullBackOff`** 狀態。

```
kubectl get pods
```
{: pre}

```
NAME         READY     STATUS             RESTARTS   AGE
<pod_name>   0/1       ImagePullBackOff   0          2m
```
{: screen}

當您說明 Pod 時，您會看到類似下列的鑑別錯誤。

```
kubectl describe pod <pod_name>
```
{: pre}

```
Failed to pull image "<region>.icr.io/<namespace>/<image>:<tag>" ... unauthorized: authentication required
Failed to pull image "<region>.icr.io/<namespace>/<image>:<tag>" ... 401 Unauthorized
```
{: screen}

```
Failed to pull image "registry.ng.bluemix.net/<namespace>/<image>:<tag>" ... unauthorized: authentication required
Failed to pull image "registry.ng.bluemix.net/<namespace>/<image>:<tag>" ... 401 Unauthorized
```
{: screen}

{: tsCauses}
您的叢集使用儲存在[映像檔取回密碼](/docs/containers?topic=containers-images#cluster_registry_auth)中的 API 金鑰或記號，來授權叢集，以從 {{site.data.keyword.registrylong_notm}} 取回映像檔。依預設，新叢集具有使用 API 金鑰的映像檔取回密碼，以讓叢集可從已部署至 `default` Kubernetes 名稱空間之容器的任何地區 `icr.io` 登錄中取回映像檔。如果叢集具有使用記號的映像檔取回密碼，則預設的 {{site.data.keyword.registrylong_notm}} 存取權會進一步受限於使用已淘汰 `<region>.registry.bluemix.net` 網域的特定地區登錄。

{: tsResolve}

1.  驗證您在部署 YAML 檔案中使用映像檔的正確名稱和標籤。
    ```
    ibmcloud cr images
    ```
    {: pre}
2.  取得失敗 Pod 的 Pod 配置檔，並尋找 `imagePullSecrets` 區段。
    ```
    kubectl get pod <pod_name> -o yaml
    ```
    {: pre}

    輸出範例：
    ```
    ...
    imagePullSecrets:
    - name: bluemix-default-secret
    - name: bluemix-default-secret-regional
    - name: bluemix-default-secret-international
    - name: default-us-icr-io
    - name: default-uk-icr-io
    - name: default-de-icr-io
    - name: default-au-icr-io
    - name: default-jp-icr-io
    - name: default-icr-io
    ...
    ```
    {: screen}
3.  如果未列出任何映像檔取回密碼，請在名稱空間中設定映像檔取回密碼。
    1.  [將映像檔取回密碼從 `default` Kubernetes 名稱空間，複製到您要在其中部署工作負載的名稱空間中](/docs/containers?topic=containers-images#copy_imagePullSecret)。
    2.  [將映像檔取回密碼新增至此 Kubernetes 名稱空間的服務帳戶](/docs/containers?topic=containers-images#store_imagePullSecret)，以便該名稱空間中的所有 Pod 都能使用映像檔取回密碼認證。
4.  如果列出映像檔取回密碼，請判定您用於存取容器登錄的認證類型。
    *   **已淘汰**：如果密碼的名稱中有 `bluemix`，您可以使用登錄記號來鑑別已淘汰的 `registry.<region>.bluemix.net` 網域名稱。繼續[對使用記號的映像檔取回密碼進行疑難排解](#ts_image_pull_token)。
    *   如果密碼的名稱中有 `icr`，您可以使用 API 金鑰來鑑別 `icr.io` 網域名稱。繼續[對使用 API 金鑰的映像檔取回密碼進行疑難排解](#ts_image_pull_apikey)。
    *   如果您有兩種類型的密碼，則會使用這兩種鑑別方法。接下來，針對容器映像檔，使用部署 YAML 中的 `icr.io` 網域名稱。繼續[對使用 API 金鑰的映像檔取回密碼進行疑難排解](#ts_image_pull_apikey)。

<br>
<br>

**對使用 API 金鑰的映像檔取回密碼進行疑難排解**</br>
{: #ts_image_pull_apikey}

如果您的 Pod 配置具有使用 API 金鑰的映像檔取回密碼，請檢查是否正確設定 API 金鑰認證。
{: shortdesc}

下列步驟假設 API 金鑰會儲存服務 ID 的認證。如果您將映像檔取回密碼設為使用個別使用者的 API 金鑰，則必須驗證使用者的 {{site.data.keyword.Bluemix_notm}} IAM 許可權及認證。
{: note}

1.  檢閱**說明**，以尋找 API 金鑰針對映像檔取回密碼所使用的服務 ID。隨叢集建立的服務 ID 表示 `ID for <cluster_name>`，且用於 `default` Kubernetes 名稱空間。如果您建立另一個服務 ID 來存取不同的 Kubernetes 名稱空間，或修改 {{site.data.keyword.Bluemix_notm}} IAM 許可權，則要自訂說明。
    ```
    ibmcloud iam service-ids
    ```
    {: pre}

    輸出範例：
    ```
    UUID                Name               Created At              Last Updated            Description                                                                                                                                                                                         Locked     
    ServiceId-aa11...   <service_ID_name>  2019-02-01T19:01+0000   2019-02-01T19:01+0000   ID for <cluster_name>                                                                                                                                         false   
    ServiceId-bb22...   <service_ID_name>  2019-02-01T19:01+0000   2019-02-01T19:01+0000   Service ID for IBM Cloud Container Registry in Kubernetes cluster <cluster_name> namespace <kube_namespace>                                                                                                                                         false    
    ```
    {: screen}
2.  驗證已至少指派給該服務 ID 一個對 [{{site.data.keyword.registryshort_notm}} 的 {{site.data.keyword.Bluemix_notm}} IAM **讀者**服務存取角色原則](/docs/services/Registry?topic=registry-user#create)。如果服務 ID 沒有**讀者**服務角色，請[編輯 IAM 原則](/docs/iam?topic=iam-serviceidpolicy#access_edit)。如果原則正確，請繼續下一步，以查看認證是否有效。
    ```
    ibmcloud iam service-policies <service_ID_name>
    ```
    {: pre}

    輸出範例：
    ```              
    Policy ID:   a111a111-b22b-333c-d4dd-e555555555e5   
    Roles:       Reader   
    Resources:                            
                  Service Name       container-registry      
                  Service Instance         
                  Region                  
                  Resource Type      namespace      
                  Resource           <registry_namespace>  
    ```
    {: screen}  
3.  檢查映像檔取回密碼認證是否有效。
    1.  取得映像檔取回密碼配置。如果 Pod 不在 `default` 名稱空間中，請包含 `-n` 旗標。
        ```
        kubectl get secret <image_pull_secret_name> -o yaml [-n <namespace>]
        ```
        {: pre}
    2.  在輸出中，複製 `.dockercfg` 欄位的 base64 編碼值。
        ```
        apiVersion: v1
        kind: Secret
        data:
          .dockercfg: eyJyZWdp...==
        ...
        ```
        {: screen}
    3.  解碼 base64 字串。例如，在 OS X 上，您可以執行下列指令。
        ```
        echo -n "<base64_string>" | base64 --decode
        ```
        {: pre}

        輸出範例：
        ```
        {"auths":{"<region>.icr.io":{"username":"iamapikey","password":"<password_string>","email":"<name@abc.com>","auth":"<auth_string>"}}}
        ```
        {: screen}
    4.  比較映像檔取回密碼地區登錄網域名稱與您在容器映像檔中指定的網域名稱。依預設，對於在 `default` Kubernetes 名稱空間中執行的容器，新叢集具有其中每個地區登錄網域名稱的映像檔取回密碼。不過，如果您修改了預設值，或使用不同的 Kubernetes 名稱空間，則可能沒有地區登錄的映像檔取回密碼。針對地區登錄網域名稱，[複製映像檔取回密碼](/docs/containers?topic=containers-images#copy_imagePullSecret)。
    5.  從您的本端機器，使用映像檔取回密碼中的 `username` 及 `password` 來登入登錄。如果您無法登入，則可能需要修正服務 ID。
        ```
        docker login -u iamapikey -p <password_string> <region>.icr.io
        ```
        {: pre}
        1.  針對在 `default` Kubernetes 名稱空間中執行的容器，重建其叢集服務 ID、{{site.data.keyword.Bluemix_notm}} IAM原則、API 金鑰及映像檔取回密碼。
            ```
            ibmcloud ks cluster-pull-secret-apply --cluster <cluster_name_or_ID>
            ```
            {: pre}
        2.  在 `default` Kubernetes 名稱空間中，重建部署。如果您仍看到授權錯誤訊息，請使用新的映像檔取回密碼，重複步驟 1-5。如果您仍無法登入，[請在 Slack 上聯絡 IBM 團隊，或開立 {{site.data.keyword.Bluemix_notm}} 支援案例](#clusters_getting_help)。
    6.  如果登入成功，請在本端取回映像檔。如果指令失敗且發生 `access denied` 錯誤，則登錄帳戶所在的 {{site.data.keyword.Bluemix_notm}} 帳戶不同於叢集所在的帳戶。[建立映像檔取回密碼以在其他帳戶中存取映像檔](/docs/containers?topic=containers-images#other_registry_accounts)。如果您可以將映像檔取回至本端機器，則 API 金鑰具有正確的許可權，但叢集裡的 API 設定不正確。您無法解決此問題。[請在 Slack 上聯絡 IBM 團隊，或開立 {{site.data.keyword.Bluemix_notm}} 支援案例](#clusters_getting_help)。
        ```
        docker pull <region>icr.io/<namespace>/<image>:<tag>
        ```
        {: pre}

<br>
<br>

**已淘汰：對使用記號的映像檔取回密碼進行疑難排解**</br>
{: #ts_image_pull_token}

如果您的 Pod 配置具有使用記號的映像檔取回密碼，請檢查記號認證是否有效。
{: shortdesc}

`registry.bluemix.net` 網域名稱支援使用記號來授權叢集對 {{site.data.keyword.registrylong_notm}} 的存取權，但是這個方法已淘汰。改為[使用 API 金鑰方法](/docs/containers?topic=containers-images#cluster_registry_auth)，來授權叢集對新 `icr.io` 登錄網域名稱的存取權。
{: deprecated}

1.  取得映像檔取回密碼配置。如果 Pod 不在 `default` 名稱空間中，請包含 `-n` 旗標。
    ```
    kubectl get secret <image_pull_secret_name> -o yaml [-n <namespace>]
    ```
    {: pre}
2.  在輸出中，複製 `.dockercfg` 欄位的 base64 編碼值。
    ```
    apiVersion: v1
    kind: Secret
    data:
      .dockercfg: eyJyZWdp...==
    ...
    ```
    {: screen}
3.  解碼 base64 字串。例如，在 OS X 上，您可以執行下列指令。
    ```
    echo -n "<base64_string>" | base64 --decode
    ```
    {: pre}

    輸出範例：
    ```
    {"auths":{"registry.<region>.bluemix.net":{"username":"token","password":"<password_string>","email":"<name@abc.com>","auth":"<auth_string>"}}}
    ```
    {: screen}
4.  比較登錄網域名稱與您在容器映像檔中指定的網域名稱。例如，若映像檔取回密碼授權對 `registry.ng.bluemix.net` 網域的存取權，但您已指定儲存在 `registry.eu-de.bluemix.net` 中的映像檔，則必須針對 `registry.eu-de.bluemix.net`，[建立要在映像檔取回密碼中使用的記號](/docs/containers?topic=containers-images#token_other_regions_accounts)。
5.  從您的本端機器，使用映像檔取回密碼中的 `username` 及 `password` 來登入登錄。如果您無法登入，則記號具有無法解決的問題。[請在 Slack 上聯絡 IBM 團隊，或開立 {{site.data.keyword.Bluemix_notm}} 支援案例](#clusters_getting_help)。
    ```
    docker login -u token -p <password_string> registry.<region>.bluemix.net
    ```
    {: pre}
6.  如果登入成功，請在本端取回映像檔。如果指令失敗且發生 `access denied` 錯誤，則登錄帳戶所在的 {{site.data.keyword.Bluemix_notm}} 帳戶不同於叢集所在的帳戶。[建立映像檔取回密碼以在其他帳戶中存取映像檔](/docs/containers?topic=containers-images#token_other_regions_accounts)。如果指令成功，[請在 Slack 上聯絡 IBM 團隊，或開立 {{site.data.keyword.Bluemix_notm}} 支援案例](#clusters_getting_help)。
    ```
    docker pull registry.<region>.bluemix.net/<namespace>/<image>:<tag>
    ```
    {: pre}

<br />


## Pod 保持擱置狀況
{: #cs_pods_pending}

{: tsSymptoms}
當您執行 `kubectl get pods` 時，可以看到保持 **Pending** 狀況的 Pod。

{: tsCauses}
如果您才剛剛建立 Kubernetes 叢集，則可能仍在配置工作者節點。

如果此叢集是現有叢集，請執行下列動作：
*  您的叢集裡可能沒有足夠的容量可部署 Pod。
*  Pod 可能已超出資源要求或限制。

{: tsResolve}
此作業需要叢集的{{site.data.keyword.Bluemix_notm}} IAM [**管理者**平台角色](/docs/containers?topic=containers-users#platform)，以及所有名稱空間的[**管理員**服務角色](/docs/containers?topic=containers-users#platform)。

如果您才剛剛建立 Kubernetes 叢集，請執行下列指令，並等待起始設定工作者節點。

```
kubectl get nodes
```
{: pre}

如果此叢集是現有叢集，請檢查叢集容量。

1.  使用預設埠號來設定 Proxy。

  ```
  kubectl proxy
  ```
   {: pre}

2.  開啟 Kubernetes 儀表板。

  ```
  http://localhost:8001/ui
  ```
  {: pre}

3.  確認您的叢集裡是否有足夠的容量可部署 Pod。

4.  如果您的叢集裡沒有足夠容量，請調整工作者節點儲存區的大小來新增更多的節點。

    1.  檢閱工作者節點儲存區的現行大小及機型，來決定要調整哪個工作者節點儲存區的大小。

        ```
        ibmcloud ks worker-pools
        ```
        {: pre}

    2.  調整工作者節點儲存區的大小，以將更多節點新增至儲存區跨越的每一個區域。

        ```
        ibmcloud ks worker-pool-resize --worker-pool <worker_pool> --cluster <cluster_name_or_ID> --size-per-zone <workers_per_zone>
        ```
        {: pre}

5.  選用項目：檢查您的 Pod 資源要求。

    1.  確認 `resources.requests` 值不大於工作者節點的容量。例如，如果 Pod 要求 `cpu: 4000m` 或 4 個核心，但工作者節點大小只有 2 個核心，則無法部署 Pod。

        ```
        kubectl get pod <pod_name> -o yaml
        ```
        {: pre}

    2.  如果要求超出可用的容量，則請[新增工作者節點儲存區](/docs/containers?topic=containers-add_workers#add_pool)，而其具有可滿足要求的工作者節點。

6.  如果您的 Pod 在工作者節點完整部署之後仍然保持 **pending** 狀況，請檢閱 [Kubernetes 文件 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/tasks/debug-application-cluster/debug-pod-replication-controller/#my-pod-stays-pending)，以進一步對 Pod 的擱置中狀況進行疑難排解。

<br />


## 容器未啟動
{: #containers_do_not_start}

{: tsSymptoms}
Pod 順利部署至叢集，但容器未啟動。

{: tsCauses}
在達到登錄配額時，容器可能無法啟動。

{: tsResolve}
[請釋放 {{site.data.keyword.registryshort_notm}} 中的儲存空間。](/docs/services/Registry?topic=registry-registry_quota#registry_quota_freeup)

<br />


## Pod 反覆地無法重新啟動或非預期地被移除
{: #pods_fail}

{: tsSymptoms}
您的 Pod 性能良好，但非預期地被移除或停滯在重新啟動迴圈中。

{: tsCauses}
您的容器可能超出其資源限制，或您的 Pod 可能被取代為較高優先順序的 Pod。

{: tsResolve}
若要查看是否因資源限制而刪除容器，請執行下列動作：
<ol><li>取得 Pod 的名稱。如果您已使用標籤，則可以包括它來過濾結果。<pre class="pre"><code>kubectl get pods --selector='app=wasliberty'</code></pre></li>
<li>說明 Pod，並尋找**重新啟動計數**。<pre class="pre"><code>kubectl describe pod</code></pre></li>
<li>如果 Pod 在短期間內重新啟動多次，則會提取其狀態。<pre class="pre"><code>kubectl get pod -o go-template={{range.status.containerStatuses}}{{"Container Name: "}}{{.name}}{{"\r\nLastState: "}}{{.lastState}}{{end}}</code></pre></li>
<li>檢閱原因。例如，`OOM Killed` 表示「記憶體不足」，指出容器因資源限制而損毀。</li>
<li>將容量新增至叢集，以滿足資源。</li></ol>

<br>

若要查看是否將 Pod 取代為較高優先順序的 Pod，請執行下列動作：
1.  取得 Pod 的名稱。

    ```
    kubectl get pods
    ```
    {: pre}

2.  說明 Pod YAML。

    ```
    kubectl get pod <pod_name> -o yaml
    ```
    {: pre}

3.  檢查 `priorityClassName` 欄位。

    1.  如果沒有 `priorityClassName` 欄位值，則 Pod 會有 `globalDefault` 優先順序類別。如果您的叢集管理者未設定 `globalDefault` 優先順序類別，則預設值為零 (0) 或最低優先順序。任何具有較高優先順序類別的 Pod 都可以先占或移除您的 Pod。

    2.  如果有 `priorityClassName` 欄位值，則請取得優先順序類別。

        ```
        kubectl get priorityclass <priority_class_name> -o yaml
        ```
        {: pre}

    3.  記下 `value` 欄位，以檢查您的 Pod 優先順序。

4.  列出叢集裡的現有優先順序類別。

    ```
    kubectl get priorityclasses
    ```
    {: pre}

5.  對於每個優先順序類別，取得 YAML 檔案，並記下 `value` 欄位。

    ```
    kubectl get priorityclass <priority_class_name> -o yaml
    ```
    {: pre}

6.  將 Pod 的優先順序類別值與其他優先順序類別值比較，以查看其優先順序較高或較低。

7.  對於叢集裡的其他 Pod 重複步驟 1 到 3，以檢查它們使用的優先順序類別。如果這些其他 Pod 的優先順序類別高於您的 Pod，則未佈建您 Pod，除非您的 Pod 有足夠的資源而且每個 Pod 都有較高的優先順序。

8.  聯絡叢集管理者，為您的叢集新增更多容量，並確認已指派正確的優先順序類別。

<br />


## 無法安裝具有已更新配置值的 Helm 圖表
{: #cs_helm_install}

{: tsSymptoms}
當您嘗試執行 `helm install -f config.yaml --namespace=kube-system --name=<release_name> ibm/<chart_name>` 來安裝已更新的 Helm 圖表時，您看到 `Error: failed to download "ibm/<chart_name>"` 錯誤訊息。

{: tsCauses}
在 Helm 實例中，{{site.data.keyword.Bluemix_notm}} 儲存庫的 URL 可能不正確。

{: tsResolve}
若要對 Helm 圖表進行疑難排解，請執行下列動作：

1. 列出 Helm 實例中目前可用的儲存庫。

    ```
    helm repo list
    ```
    {: pre}

2. 在輸出中，驗證 {{site.data.keyword.Bluemix_notm}} 儲存庫 `ibm` 的 URL 是否為 `https://icr.io/helm/iks-charts`。

    ```
    NAME    URL
    stable  https://kubernetes-charts.storage.googleapis.com
    local   http://127.0.0.1:8888/charts
    ibm     https://icr.io/helm/iks-charts
    ```
    {: screen}

    * 如果 URL 不正確，請執行下列動作：

        1. 移除 {{site.data.keyword.Bluemix_notm}} 儲存庫。

            ```
            helm repo remove ibm
            ```
            {: pre}

        2. 再次新增 {{site.data.keyword.Bluemix_notm}} 儲存庫。

            ```
   helm repo add iks-charts https://icr.io/helm/iks-charts
   ```
            {: pre}

    * 如果 URL 正確無誤，則從儲存庫取得最新的更新。

        ```
        helm repo update
        ```
        {: pre}

3. 安裝含有更新的 Helm 圖表。

    ```
    helm install -f config.yaml --namespace=kube-system --name=<release_name> ibm/<chart_name>
    ```
    {: pre}

<br />


## 無法從叢集裡的公用映像檔安裝 Helm Tiller 或部署容器
{: #cs_tiller_install}

{: tsSymptoms}

當您嘗試安裝 Helm Tiller，或想要從公用登錄（例如 DockerHub）部署映像檔時，安裝失敗並傳回類似下列的錯誤：

```
Failed to pull image "gcr.io/kubernetes-helm/tiller:v2.12.0": rpc error: code = Unknown desc = failed to resolve image "gcr.io/kubernetes-helm/tiller:v2.12.0": no available registry endpoint:
```
{: screen}

{: tsCauses}
您可能已設定自訂防火牆、指定自訂 Calico 原則，或使用專用服務端點來建立僅專用叢集，該端點會封鎖公用網路連線至儲存映像檔的容器登錄。

{: tsResolve}
- 如果您有自訂防火牆或設定自訂 Calico 原則，請容許工作者節點與儲存映像檔之容器儲存庫之間的出埠及入埠網路資料流量。如果映像檔儲存在 {{site.data.keyword.registryshort_notm}} 中，請檢閱[容許叢集存取基礎架構資源及其他服務](/docs/containers?topic=containers-firewall#firewall_outbound)中的必要埠。
- 如果您利用只啟用專用服務端點的方式來建立專用叢集，則可以針對您的叢集，[啟用公用服務端點](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_disable)。如果想要在專用叢集裡安裝 Helm 圖表，而不需開啟公用連線，則可安裝 Helm（[含 Tiller](/docs/containers?topic=containers-helm#private_local_tiller) 或[不含 Tiller](/docs/containers?topic=containers-helm#private_install_without_tiller)）。

<br />


## 取得協助及支援
{: #clusters_getting_help}

叢集仍有問題？
{: shortdesc}

-  在終端機中，有 `ibmcloud` CLI 及外掛程式的更新可用時，就會通知您。請務必保持最新的 CLI，讓您可以使用所有可用的指令及旗標。
-   若要查看 {{site.data.keyword.Bluemix_notm}} 是否可用，請[檢查 {{site.data.keyword.Bluemix_notm}} 狀態頁面 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/status?selected=status)。
-   將問題張貼到 [{{site.data.keyword.containerlong_notm}} Slack ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://ibm-container-service.slack.com)。如果您的 {{site.data.keyword.Bluemix_notm}} 帳戶未使用 IBM ID，請[要求邀請](https://bxcs-slack-invite.mybluemix.net/)以加入此 Slack。
    {: tip}
-   檢閱討論區，以查看其他使用者是否發生過相同的問題。使用討論區提問時，請標記您的問題，以便 {{site.data.keyword.Bluemix_notm}} 開發團隊能看到它。
    -   如果您在使用 {{site.data.keyword.containerlong_notm}} 開發或部署叢集或應用程式時有技術方面的問題，請將問題張貼到 [Stack Overflow ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://stackoverflow.com/questions/tagged/ibm-cloud+containers)，並使用 `ibm-cloud`、`kubernetes` 及 `containers` 來標記問題。
    -   若為服務及開始使用指示的相關問題，請使用 [IBM Developer Answers ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix) 討論區。請包含 `ibm-cloud` 及 `containers` 標籤。如需使用討論區的詳細資料，請參閱[取得協助](/docs/get-support?topic=get-support-getting-customer-support#using-avatar)。
-   開立案例，以與「IBM 支援中心」聯絡。若要瞭解如何開立 IBM 支援中心案例，或是瞭解支援層次與案例嚴重性，請參閱[與支援中心聯絡](/docs/get-support?topic=get-support-getting-customer-support)。當您報告問題時，請包含您的叢集 ID。若要取得叢集 ID，請執行 `ibmcloud ks clusters`。您也可以使用 [{{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool](/docs/containers?topic=containers-cs_troubleshoot#debug_utility)，來收集及匯出叢集裡的相關資訊，以與 IBM 支援中心共用。
{: tip}

