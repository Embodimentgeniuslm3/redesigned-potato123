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



# 從叢集移除持續性儲存空間
{: #cleanup}

當您在叢集裡設定持續性儲存空間時，有三個主要元件：要求儲存空間的 Kubernetes 持續性磁區要求 (PVC)、裝載至 Pod 並在 PVC 中說明的 Kubernetes 持續性磁區 (PV)，以及 IBM Cloud 基礎架構 (SoftLayer) 實例，例如 NFS 檔案或區塊儲存空間。所有這三個項目可能需要分別刪除，具體取決於儲存空間的建立方式。
{:shortdesc}

## 清除持續性儲存空間
{: #storage_remove}

瞭解您的刪除選項：

**我已刪除叢集。必須刪除任何其他項目才能移除持續性儲存空間嗎？**</br>
視情況而定。刪除叢集時，也會刪除 PVC 及 PV。不過，您可以選擇是否要移除 IBM Cloud 基礎架構 (SoftLayer) 中的相關聯儲存空間實例。如果您選擇不移除，儲存空間實例仍會存在。此外，如果您已刪除處於性能不佳狀況的叢集，則即使您選擇要移除儲存空間，此儲存空間可能仍會存在。請遵循 IBM Cloud 基礎架構 (SoftLayer) 中的指示，尤其是[刪除儲存空間實例](#sl_delete_storage)的步驟。

**我可以藉由刪除 PVC 來移除我的所有儲存空間嗎？**</br>
有時可以。如果您[動態建立持續性儲存空間](/docs/containers?topic=containers-kube_concepts#dynamic_provisioning)，並選取其名稱中沒有 `retain` 的儲存空間類別，則當您刪除 PVC 時，也會刪除 PV 及 IBM Cloud 基礎架構 (SoftLayer) 儲存空間實例。

在所有其他情況下，請遵循指示來檢查 PVC、PV 及實體儲存裝置的狀態，並在需要時個別刪除它們。

**刪除儲存空間之後，我是否仍然需要為其付費？**</br>
這取決於您刪除的項目和計費類型。如果您刪除 PVC 及 PV，但未刪除 IBM Cloud 基礎架構 (SoftLayer) 帳戶中的實例，則該實例仍會存在，並且您仍要為其付費。您必須刪除所有項目，才能避免費用。此外，在 PVC 中指定 `billingType` 時，您可以選擇 `hourly` 或 `monthly`。如果您選擇 `monthly`，則您的實例是依月計費。刪除實例時，您需要為當月的剩餘時間付費。


<p class="important">清除持續性儲存空間時，也會刪除其中儲存的所有資料。如果您需要資料的副本，請備份[檔案儲存空間](/docs/containers?topic=containers-file_storage#file_backup_restore)或[區塊儲存空間](/docs/containers?topic=containers-block_storage#block_backup_restore)。</p>

開始之前：[登入您的帳戶。適用的話，請將適當的資源群組設為目標。設定叢集的環境定義。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

若要清除持續性資料，請執行下列動作：

1.  列出叢集裡的 PVC，並記下 PVC 的 **`NAME`**、**`STORAGECLASS`**，以及連結至 PVC 並顯示為 **`VOLUME`** 的 PV 名稱。
    ```
    kubectl get pvc
    ```
    {: pre}

    輸出範例：
    ```
    NAME                  STATUS    VOLUME                                     CAPACITY   ACCESSMODES   STORAGECLASS            AGE
    claim1-block-bronze   Bound     pvc-06886b77-102b-11e8-968a-f6612bb731fb   20Gi       RWO           ibmc-block-bronze       78d
    claim-file-bronze     Bound     pvc-457a2b96-fafc-11e7-8ff9-b6c8f770356c   4Gi        RWX           ibmc-file-bronze-retain 105d
    claim-file-silve      Bound     pvc-1efef0ba-0c48-11e8-968a-f6612bb731fb   24Gi       RWX           ibmc-file-silver        83d
    ```
    {: screen}

2. 檢閱儲存空間類別的 **`ReclaimPolicy`** 及 **`billingType`**。
   ```
   kubectl describe storageclass <storageclass_name>
   ```
   {: pre}

   如果收回原則指明為 `Delete`，則在您移除 PVC 時，也會移除 PV 及實體儲存空間。如果收回原則指明為 `Retain`，或如果您已佈建儲存空間，但沒有儲存空間類別，則在您移除 PVC 時，不會移除 PV 及實體儲存空間。您必須個別移除 PVC、PV 及實體儲存空間。

   如果儲存空間依月收費，即使在計費週期結束之前已移除儲存空間，也仍需要依整月付費。
   {: important}

3. 移除任何裝載 PVC 的 Pod。
   1. 列出裝載 PVC 的 Pod。
      ```
      kubectl get pods --all-namespaces -o=jsonpath='{range .items[*]}{"\n"}{.metadata.name}{":\t"}{range .spec.volumes[*]}{.persistentVolumeClaim.claimName}{" "}{end}{end}' | grep "<pvc_name>"
      ```
      {: pre}

      輸出範例：
      ```
      blockdepl-12345-prz7b:	claim1-block-bronze  
      ```
      {: screen}

      如果您的 CLI 輸出中未傳回任何 Pod，則您沒有使用 PVC 的 Pod。

   2. 移除使用 PVC 的 Pod。如果 Pod 是部署的一部分，便移除部署。
      ```
      kubectl delete pod <pod_name>
      ```
      {: pre}

   3. 驗證已移除 Pod。
      ```
      kubectl get pods
      ```
      {: pre}

4. 移除 PVC。
   ```
   kubectl delete pvc <pvc_name>
   ```
   {: pre}

5. 檢閱 PV 的狀態。請使用您先前擷取的 PV 名稱 **`VOLUME`**。
   ```
   kubectl get pv <pv_name>
   ```
   {: pre}

   移除 PVC 時，會釋放連結至 PVC 的 PV。視佈建儲存空間的方式而定，如果自動刪除 PV，您的 PV 會進入 `Deleting` 狀況，或者如果您必須手動刪除 PV，則 PV 會進入 `Released` 狀況。**附註**：若為自動刪除的 PV，則在刪除它之前，狀況可能短暫地顯示為 `Released`。請在幾分鐘後重新執行該指令以查看該 PV 是否已移除。

6. 如果 PV 未刪除，請手動移除 PV。
   ```
   kubectl delete pv <pv_name>
   ```
   {: pre}

7. 驗證已移除 PV。
   ```
    kubectl get pv
    ```
   {: pre}

8. {: #sl_delete_storage}列出 PV 指向的實體儲存空間實例，並記下實體儲存空間實例的 **`id`**。

   **檔案儲存空間：**
   ```
   ibmcloud sl file volume-list --columns id  --columns notes | grep <pv_name>
   ```
   {: pre}
   **區塊儲存空間：**
   ```
   ibmcloud sl block volume-list --columns id --columns notes | grep <pv_name>
   ```
   {: pre}

   如果您已移除叢集而無法擷取 PV 的名稱，請將 `grep <pv_name>` 取代為 `grep <cluster_id>`，以列出與叢集相關聯的所有儲存裝置。
   {: tip}

   輸出範例：
   ```
   id         notes   
   12345678   {"plugin":"ibm-file-plugin-5b55b7b77b-55bb7","region":"us-south","cluster":"aa1a11a1a11b2b2bb22b22222c3c3333","type":"Endurance","ns":"default","pvc":"mypvc","pv":"pvc-d979977d-d79d-77d9-9d7d-d7d97ddd99d7","storageclass":"ibmc-file-gold"}
   ```
   {: screen}

   瞭解 **Notes** 欄位資訊：
   *  **`"plugin":"ibm-file-plugin-5b55b7b77b-55bb7"`**：叢集使用的儲存空間外掛程式。
   *  **`"region":"us-south"`**：叢集所在的地區。
   *  **`"cluster":"aa1a11a1a11b2b2bb22b22222c3c3333"`**：與儲存空間實例相關聯的叢集 ID。
   *  **`"type":"Endurance"`**：檔案或區塊儲存空間的類型，此為 `Endurance` 或 `Performance`。
   *  **`"ns":"default"`**：在其中部署儲存空間實例的名稱空間。
   *  **`"pvc":"mypvc"`**：與儲存空間實例相關聯的 PVC 名稱。
   *  **`"pv":"pvc-d979977d-d79d-77d9-9d7d-d7d97ddd99d7"`**：與儲存空間實例相關聯的 PV。
   *  **`"storageclass":"ibmc-file-gold"`**：儲存空間類別的類型：銅級、銀級、金級或自訂。

9. 移除實體儲存空間實例。

   **檔案儲存空間：**
   ```
   ibmcloud sl file volume-cancel <filestorage_id>
   ```
   {: pre}

   **區塊儲存空間：**
   ```
   ibmcloud sl block volume-cancel <blockstorage_id>
   ```
   {: pre}

9. 驗證已移除實體儲存空間實例。刪除程序最長可能需要幾天時間才能完成。

   **檔案儲存空間：**
   ```
   ibmcloud sl file volume-list
   ```
   {: pre}
   **區塊儲存空間：**
   ```
   ibmcloud sl block volume-list
   ```
   {: pre}
