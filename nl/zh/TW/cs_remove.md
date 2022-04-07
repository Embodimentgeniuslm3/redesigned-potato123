---

copyright:
  years: 2014, 2019
lastupdated: "2019-05-31"

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


# 移除叢集
{: #remove}

不再需要使用計費帳戶所建立的免費及標準叢集時，必須手動移除這些叢集，讓它們不再耗用資源。
{:shortdesc}

<p class="important">
不會在持續性儲存空間中建立叢集或資料的備份。當您刪除叢集時，您可以選擇刪除持續性儲存空間。如果您選擇刪除持續性儲存空間，則在 IBM Cloud 基礎架構 (SoftLayer) 中，會永久刪除您使用 `delete` 儲存空間類別所佈建的持續性儲存空間。如果您是使用 `retain` 儲存空間類別來佈建持續性儲存空間，且您選擇要刪除該儲存空間，則也會刪除叢集、PV 及 PVC，但是 IBM Cloud 基礎架構 (SoftLayer) 帳戶中的持續性儲存空間實例會保留下來。</br>
</br>移除叢集時，您也會移除在建立叢集時自動佈建的所有子網路，以及您使用 `ibmcloud ks cluster-subnet-create` 指令所建立的所有子網路。不過，如果您使用 `ibmcloud ks cluster-subnet-add` 指令手動將現有子網路新增至叢集，則不會從 IBM Cloud 基礎架構 (SoftLayer) 帳戶移除這些子網路，並且可以在其他叢集裡重複使用它們。</p>

開始之前：
* 記下叢集 ID。您可能需要叢集 ID，才能調查及移除未隨著叢集自動刪除的相關 IBM Cloud 基礎架構 (SoftLayer) 資源。
* 如果您要刪除持續性儲存空間中的資料，請[瞭解 delete 選項](/docs/containers?topic=containers-cleanup#cleanup)。
* 確定您具有[**管理者** {{site.data.keyword.Bluemix_notm}} IAM 平台角色](/docs/containers?topic=containers-users#platform)。

若要移除叢集，請執行下列動作：

1. 選用：在 CLI 中，將叢集裡所有資料的副本儲存到本端 YAML 檔案。
  ```
  kubectl get all --all-namespaces -o yaml
  ```
  {: pre}

2. 移除叢集。
  - 從 {{site.data.keyword.Bluemix_notm}} 主控台
    1.  選取叢集，然後按一下**其他動作...** 功能表中的**刪除**。

  - 從 {{site.data.keyword.Bluemix_notm}} CLI
    1.  列出可用的叢集。

        ```
        ibmcloud ks clusters
        ```
        {: pre}

    2.  刪除叢集。

        ```
        ibmcloud ks cluster-rm --cluster <cluster_name_or_ID>
        ```
        {: pre}

    3.  遵循提示，然後選擇是否刪除叢集資源，其中包括容器、Pod、連結服務、持續性儲存空間及密碼。
      - **持續性儲存空間**：持續性儲存空間為您的資料提供高可用性。如果您使用[現有的檔案共用](/docs/containers?topic=containers-file_storage#existing_file)建立了持續性磁區要求，則在刪除叢集時，無法刪除檔案共用。您必須稍後從 IBM Cloud 基礎架構 (SoftLayer) 組合中手動刪除檔案共用。

          基於每月計費週期，不能在月底最後一天刪除持續性磁區宣告。如果您在該月份的最後一天刪除持續性磁區要求，則刪除會保持擱置，直到下個月開始為止。
          {: note}

後續步驟：
- 當您執行 `ibmcloud ks clusters` 指令時，在可用的叢集清單中不再列出已移除的叢集之後，您就可以重複使用該叢集的名稱。
- 如果保留子網路，則可以[在新的叢集裡重複使用它們](/docs/containers?topic=containers-subnets#subnets_custom)，或稍後從 IBM Cloud 基礎架構 (SoftLayer) 組合中手動刪除它們。
- 如果保留持續性儲存空間，則稍後可以透過 {{site.data.keyword.Bluemix_notm}} 主控台中的 IBM Cloud 基礎架構 (SoftLayer) 儀表板來[刪除儲存空間](/docs/containers?topic=containers-cleanup#cleanup)。
