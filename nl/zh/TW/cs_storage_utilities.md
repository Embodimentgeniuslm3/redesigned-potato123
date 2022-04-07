---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: kubernetes, iks, local persistent storage

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



# IBM Cloud 儲存空間公用程式
{: #utilities}

## 安裝 IBM Cloud Block Storage Attacher 外掛程式（測試版）
{: #block_storage_attacher}

使用 {{site.data.keyword.Bluemix_notm}} Block Storage Attacher 外掛程式，將原始、未格式化且未裝載的區塊儲存空間連接至叢集裡的工作者節點。
  
{: shortdesc}

例如，您想要使用 [Portworx](/docs/containers?topic=containers-portworx) 等軟體定義儲存空間解決方案(SDS) 來儲存資料，但不想要使用針對 SDS 用法而進行最佳化，且具有額外本端磁碟的裸機工作者節點。若要新增本端磁碟至您的非 SDS工作者節點，您必須在 {{site.data.keyword.Bluemix_notm}} 基礎架構帳戶中，手動建立區塊儲存裝置，然後使用 {{site.data.keyword.Bluemix_notm}}Block Volume Attacher 將該儲存空間連接至您的非 SDS 工作者節點。

{{site.data.keyword.Bluemix_notm}} Block Volume Attacher 外掛程式會在叢集裡的每個工作者節點上建立 Pod，作為常駐程式集的一部分，並設定 Kubernetes 儲存空間類別，您稍後會使用該類別將區塊儲存裝置連接至非 SDS 工作者節點。

您在尋找如何更新或移除 {{site.data.keyword.Bluemix_notm}} Block Volume Attacher 外掛程式的指示嗎？請參閱[更新外掛程式](#update_block_attacher)及[移除外掛程式](#remove_block_attacher)。
{: tip}

1.  [遵循指示](/docs/containers?topic=containers-helm#public_helm_install)，將 Helm 用戶端安裝在本端機器上，並在叢集裡使用服務帳戶安裝 Helm 伺服器 (Tiller)。

2.  驗證已使用服務帳戶安裝 Tiller。

    ```
    kubectl get serviceaccount -n kube-system | grep tiller
    ```
    {: pre}

    輸出範例：

    ```
    NAME                                 SECRETS   AGE
    tiller                               1         2m
    ```
    {: screen}

3. 更新 Helm 儲存庫，以擷取此儲存庫中所有 Helm 圖表的最新版本。
   ```
   helm repo update
   ```
   {: pre}

4. 安裝 {{site.data.keyword.Bluemix_notm}} Block Volume Attacher 外掛程式。安裝外掛程式時，會將預先定義的區塊儲存空間類別新增至叢集。
   ```
   helm install iks-charts/ibm-block-storage-attacher --name block-attacher
   ```
   {: pre}

   輸出範例：
   ```
   NAME:   block-volume-attacher
   LAST DEPLOYED: Thu Sep 13 22:48:18 2018
   NAMESPACE: default
   STATUS: DEPLOYED

   RESOURCES:
   ==> v1beta1/ClusterRoleBinding
   NAME                             AGE
   ibmcloud-block-storage-attacher  1s

   ==> v1beta1/DaemonSet
   NAME                             DESIRED  CURRENT  READY  UP-TO-DATE  AVAILABLE  NODE SELECTOR  AGE
   ibmcloud-block-storage-attacher  0        0        0      0           0          <none>         1s

   ==> v1/StorageClass
   NAME                 PROVISIONER                AGE
   ibmc-block-attacher  ibm.io/ibmc-blockattacher  1s

   ==> v1/ServiceAccount
   NAME                             SECRETS  AGE
   ibmcloud-block-storage-attacher  1        1s

   ==> v1beta1/ClusterRole
   NAME                             AGE
   ibmcloud-block-storage-attacher  1s

   NOTES:
   Thank you for installing: ibmcloud-block-storage-attacher.   Your release is named: block-volume-attacher

   Please refer Chart README.md file for attaching a block storage
   Please refer Chart RELEASE.md to see the release details/fixes
   ```
   {: screen}

5. 驗證是否已順利安裝 {{site.data.keyword.Bluemix_notm}} Block Volume Attacher 常駐程式集。
   ```
   kubectl get pod -n kube-system -o wide | grep attacher
   ```
   {: pre}

   輸出範例：
   ```
   ibmcloud-block-storage-attacher-z7cv6           1/1       Running            0          19m
   ```
   {: screen}

   當您看到一個以上的 **ibmcloud-block-storage-attacher** Pod 時，安裝即已成功。Pod 的數目會等於叢集裡的工作者節點數目。所有 Pod 都必須處於 **Running** 狀況。

6. 驗證是否已順利建立 {{site.data.keyword.Bluemix_notm}} Block Volume Attacher 的儲存空間類別。
   ```
   kubectl get storageclasses | grep attacher
   ```
   {: pre}

   輸出範例：
   ```
   ibmc-block-attacher       ibm.io/ibmc-blockattacher   11m
   ```
   {: screen}

### 更新 IBM Cloud Block Storage Attacher 外掛程式
{: #update_block_attacher}

您可以將現有的 {{site.data.keyword.Bluemix_notm}} Block Storage Attacher 外掛程式升級至最新版本。
{: shortdesc}

1. 更新 Helm 儲存庫，以擷取此儲存庫中所有 Helm 圖表的最新版本。
   ```
   helm repo update
   ```
   {: pre}

2. 選用項目：將最新的 Helm 圖表下載至您的本端機器。然後，將套件解壓縮，並檢閱 `release.md` 檔案，以找到最新版本資訊。
   ```
   helm fetch iks-charts/ibmcloud-block-storage-plugin
   ```
   {: pre}

3. 尋找 {{site.data.keyword.Bluemix_notm}} Block Storage Attacher 外掛程式的 Helm 圖表名稱。
   ```
   helm ls | grep ibm-block-storage-attacher
   ```
   {: pre}

   輸出範例：
   ```
   <helm_chart_name>	1       	Wed Aug  1 14:55:15 2018	DEPLOYED	ibm-block-storage-attacher-1.0.0	default
   ```
   {: screen}

4. 將 {{site.data.keyword.Bluemix_notm}} Block Storage Attacher 升級至最新版本。
   ```
   helm upgrade --force --recreate-pods <helm_chart_name> ibm-block-storage-attacher
   ```
   {: pre}

### 移除 IBM Cloud Block Volume Attacher 外掛程式
{: #remove_block_attacher}

如果您不想要在叢集裡佈建及使用 {{site.data.keyword.Bluemix_notm}} Block Storage Attacher 外掛程式，您可以解除安裝 Helm 圖表。
{: shortdesc}

1. 尋找 {{site.data.keyword.Bluemix_notm}} Block Storage Attacher 外掛程式的 Helm 圖表名稱。
   ```
   helm ls | grep ibm-block-storage-attacher
   ```
   {: pre}

   輸出範例：
   ```
   <helm_chart_name>	1       	Wed Aug  1 14:55:15 2018	DEPLOYED	ibm-block-storage-attacher-1.0.0	default
   ```
   {: screen}

2. 移除 Helm 圖表，以刪除 {{site.data.keyword.Bluemix_notm}} Block Storage Attacher 外掛程式。
   ```
   helm delete <helm_chart_name> --purge
   ```
   {: pre}

3. 驗證是否已移除 {{site.data.keyword.Bluemix_notm}} Block Storage Attacher 外掛程式 Pod。
   ```
   kubectl get pod -n kube-system -o wide | grep attacher
   ```
   {: pre}

   如果 CLI 輸出中沒有顯示任何 Pod，則表示已順利移除 Pod。

4. 驗證是否已移除 {{site.data.keyword.Bluemix_notm}} Block Storage Attacher 儲存空間類別。
   ```
   kubectl get storageclasses | grep attacher
   ```
   {: pre}

   如果 CLI 輸出中沒有顯示任何儲存空間類別，則表示已順利移除儲存空間類別。

## 自動佈建未格式化的區塊儲存空間，並授權工作者節點存取儲存空間
{: #automatic_block}

您可以使用 {{site.data.keyword.Bluemix_notm}} Block Volume Attacher 外掛程式，將含有相同配置的原始、未格式化且未裝載的區塊儲存空間，自動新增至叢集裡的所有工作者節點。
{: shortdesc}

內含在 {{site.data.keyword.Bluemix_notm}} Block Volume Attacher 外掛程式中的 `mkpvyaml` 容器已配置為執行 Script，該 Script 會尋找叢集裡的所有工作者節點、在 {{site.data.keyword.Bluemix_notm}} 基礎架構入口網站中建立原始區塊儲存空間，然後授權工作者節點存取儲存空間。

若要新增不同的區塊儲存空間配置、將區塊儲存空間僅新增至部分工作者節點，或擁有更多的佈建處理程序控制權，請選擇[手動新增區塊儲存空間](#manual_block)。
{: tip}


1. 登入 {{site.data.keyword.Bluemix_notm}}，並將目標設定為您的叢集所在的資源群組。
   ```
   ibmcloud login
   ```
   {: pre}

2.  複製「{{site.data.keyword.Bluemix_notm}} 儲存空間公用程式」儲存庫。
    ```
    git clone https://github.com/IBM/ibmcloud-storage-utilities.git
    ```
    {: pre}

3. 導覽至 `block-storage-utilities` 目錄。
   ```
   cd ibmcloud-storage-utilities/block-storage-provisioner
   ```
   {: pre}

4. 開啟 `yamlgen.yaml` 檔，並指定要新增至叢集裡每個工作者節點的區塊儲存空間配置。
   ```
   #
   # Can only specify 'performance' OR 'endurance' and associated clause
   #
   cluster:  <cluster_name>    #  Enter the name of your cluster
   region:   <region>          #  Enter the IBM Cloud Kubernetes Service region where you created your cluster
   type:  <storage_type>       #  Enter the type of storage that you want to provision. Choose between "performance" or "endurance"
   offering: storage_as_a_service
   # performance:
   #    - iops:  <iops>
   endurance:
     - tier:  2                #   [0.25|2|4|10]
   size:  [ 20 ]               #   Enter the size of your storage in gigabytes
   ```
   {: codeblock}

   <table>
   <caption>瞭解 YAML 檔案元件</caption>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解 YAML 檔案元件</th>
   </thead>
   <tbody>
   <tr>
   <td><code>cluster</code></td>
   <td>輸入要在其中新增原始區塊儲存空間的叢集名稱。</td>
   </tr>
   <tr>
   <td><code>region</code></td>
   <td>輸入您已在其中建立叢集的 {{site.data.keyword.containerlong_notm}} 地區。執行 <code>[bxcs] cluster-get --cluster &lt;cluster_name_or_ID&gt;</code>，以尋找您叢集的地區。</td>
   </tr>
   <tr>
   <td><code>type</code></td>
   <td>輸入您要佈建的儲存空間類型。請選擇 <code>performance</code> 或 <code>endurance</code>。如需相關資訊，請參閱[決定區塊儲存空間配置](/docs/containers?topic=containers-block_storage#block_predefined_storageclass)。</td>
   </tr>
   <tr>
   <td><code>performance.iops</code></td>
   <td>如果您要佈建 `performance` 儲存空間，請輸入 IOPS 的數目。如需相關資訊，請參閱[決定區塊儲存空間配置](/docs/containers?topic=containers-block_storage#block_predefined_storageclass)。如果您要佈建 `endurance` 儲存空間，請移除此區段，或將 `#` 新增至每一行的開頭，以註銷此區段。</tr>
   <tr>
   <td><code>endurance.tier</code></td>
   <td>如果您要佈建 `endurance` 儲存空間，請輸入每 GB 的 IOPS 數目。例如，若您要如 `ibmc-block-bronze` 儲存空間類別中所定義，來佈建區塊儲存空間，請輸入 2。如需相關資訊，請參閱[決定區塊儲存空間配置](/docs/containers?topic=containers-block_storage#block_predefined_storageclass)。如果您要佈建 `performance` 儲存空間，請移除此區段，或將 `#` 新增至每一行的開頭，以註銷此區段。</td>
   </tr>
   <tr>
   <td><code>size</code></td>
   <td>輸入儲存空間的大小（以 GB 為單位）。請參閱[決定區塊儲存空間配置](/docs/containers?topic=containers-block_storage#block_predefined_storageclass)，尋找您的儲存空間層級所支援的大小。</td>
   </tr>
   </tbody>
   </table>  

5. 擷取您的 IBM Cloud 基礎架構 (SoftLayer) 使用者名稱及 API 金鑰。`mkpvyaml` Script 會使用該使用者名稱及 API 金鑰來存取叢集。
   1. 登入 [{{site.data.keyword.Bluemix_notm}} 主控台 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/)。
   2. 從功能表 ![「功能表」圖示](../icons/icon_hamburger.svg "「功能表」圖示")，選取**基礎架構**。
   3. 從功能表列，選取**帳戶** > **使用者** > **使用者清單**。
   4. 尋找您要擷取其使用者名稱及 API 金鑰的使用者。
   5. 按一下**產生**以產生 API 金鑰，或按一下**檢視**以檢視您現有的 API 金鑰。即會開啟一個蹦現視窗，顯示基礎架構使用者名稱及 API 金鑰。

6. 將認證儲存在環境變數中。
   1. 新增環境變數。
      ```
      export SL_USERNAME=<infrastructure_username>
      ```
      {: pre}

      ```
      export SL_API_KEY=<infrastructure_api_key>
      ```
      {: pre}

   2. 驗證是否已順利建立環境變數。
      ```
        printenv
        ```
      {: pre}

7.  建置並執行 `mkpvyaml` 容器。當您從映像檔執行容器時，會執行 `mkpvyaml.py` Script。該 Script 會將區塊儲存裝置新增至叢集裡的每個工作者節點，並授權每個工作者節點存取該區塊儲存裝置。在 Script 結束時，會產生 `pv-<cluster_name>.yaml` YAML 檔案，您稍後可以使用它在叢集裡建立持續性磁區。
    1.  建置 `mkpvyaml` 容器。
        ```
        docker build -t mkpvyaml .
        ```
        {: pre}
        輸出範例：
        ```
        Sending build context to Docker daemon   29.7kB
        Step 1/16 : FROM ubuntu:18.10
        18.10: Pulling from library/ubuntu
        5940862bcfcd: Pull complete
        a496d03c4a24: Pull complete
        5d5e0ccd5d0c: Pull complete
        ba24b170ddf1: Pull complete
        Digest: sha256:20b5d52b03712e2ba8819eb53be07612c67bb87560f121cc195af27208da10e0
        Status: Downloaded newer image for ubuntu:18.10
         ---> 0bfd76efee03
        Step 2/16 : RUN apt-get update
         ---> Running in 85cedae315ce
        Get:1 http://security.ubuntu.com/ubuntu cosmic-security InRelease [83.2 kB]
        Get:2 http://archive.ubuntu.com/ubuntu cosmic InRelease [242 kB]
        ...
        Step 16/16 : ENTRYPOINT [ "/docker-entrypoint.sh" ]
         ---> Running in 9a6842f3dbe3
        Removing intermediate container 9a6842f3dbe3
         ---> 7926f5384fc7
        Successfully built 7926f5384fc7
        Successfully tagged mkpvyaml:latest
        ```
        {: screen}
    2.  執行容器，以執行 `mkpvyaml.py` Script。
        ```
        docker run --rm -v `pwd`:/data -v ~/.bluemix:/config -e SL_API_KEY=$SL_API_KEY -e SL_USERNAME=$SL_USERNAME mkpvyaml
        ```
        {: pre}

        輸出範例：
    ```
        Unable to find image 'portworx/iks-mkpvyaml:latest' locally
        latest: Pulling from portworx/iks-mkpvyaml
        72f45ff89b78: Already exists
        9f034a33b165: Already exists
        386fee7ab4d3: Already exists
        f941b4ac6aa8: Already exists
        fe93e194fcda: Pull complete
        f29a13da1c0a: Pull complete
        41d6e46c1515: Pull complete
        e89af7a21257: Pull complete
        b8a7a212d72e: Pull complete
        5e07391a6f39: Pull complete
        51539879626c: Pull complete
        cdbc4e813dcb: Pull complete
        6cc28f4142cf: Pull complete
        45bbaad87b7c: Pull complete
        05b0c8595749: Pull complete
        Digest: sha256:43ac58a8e951994e65f89ed997c173ede1fa26fb41e5e764edfd3fc12daf0120
        Status: Downloaded newer image for portworx/iks-mkpvyaml:latest

        kube-dal10-abc1d23e123456ac8b12a3c1e12b123a4 10.XXX.XX.XX
        Creating Vol of size 20 with type:  endurance
                  Ordering block storage of size: 20 for host: kube-dal10-abc1d23e123456ac8b12a3c1e12b123a4
                  ORDER ID =  30087291
        kube-dal10-abc1d23e123456ac8b12a3c1e12b123a4 10.XXX.XX.XX
        Creating Vol of size 20 with type:  endurance
                  Ordering block storage of size: 20 for host: kube-dal10-abc1d23e123456ac8b12a3c1e12b123a4
                  ORDER ID =  30087293
        kube-dal12-abc1d23e123456ac8b12a3c1e12b456v1 10.XXX.XX.XX
        Creating Vol of size 20 with type:  endurance
                  Ordering block storage of size: 20 for host: kube-dal12-abc1d23e123456ac8b12a3c1e12b456v1
                  ORDER ID =  30085655
                  No volume yet ... for orderID :  30087291
                  No volume yet ... for orderID :  30087291
                  No volume yet ... for orderID :  30087291
                  No volume yet ... for orderID :  30087291
                  Order ID =  30087291 has created VolId =  12345678
                  Granting access to volume: 12345678 for HostIP: 10.XXX.XX.XX
                  Order ID =  30087293 has created VolId =  87654321
                  Granting access to volume: 87654321 for HostIP: 10.XXX.XX.XX
                  Vol 53002831 is not yet ready ...
                  Granting access to volume: 87654321 for HostIP: 10.XXX.XX.XX
                  Order ID =  30085655 has created VolId =  98712345
                  Granting access to volume: 98712345 for HostIP: 10.XXX.XX.XX
        Output file created as :  pv-<cluster_name>.yaml
        ```
        {: screen}

7. [將區塊儲存裝置連接至您的工作者節點](#attach_block)。

## 手動新增區塊儲存空間至特定的工作者節點
{: #manual_block}

如果您要新增不同的區塊儲存空間配置、將區塊儲存空間僅新增至部分工作者節點，或擁有更多的佈建處理程序控制權，請使用此選項。
{: shortdesc}

1. 列出叢集裡的工作者節點，並記下您要新增區塊儲存裝置之非 SDS 工作者節點的專用 IP 位址及區域。
   ```
   ibmcloud ks workers --cluster <cluster_name_or_ID>
   ```
   {: pre}

2. 檢閱[決定區塊儲存空間配置](/docs/containers?topic=containers-block_storage#block_predefined_storageclass)中的步驟 3 和 4，針對您要新增至非 SDS 工作者節點的區塊儲存裝置，選擇類型、大小及 IOPS 數目。    

3. 在非 SDS 工作者節點所在的相同區域中建立區塊儲存裝置。

   **佈建 20 GB、2 IOPS/GB 耐久性區塊儲存空間的範例：**
   ```
   ibmcloud sl block volume-order --storage-type endurance --size 20 --tier 2 --os-type LINUX --datacenter dal10
   ```
   {: pre}

   **佈建 20 GB、100 IOPS 效能區塊儲存空間的範例：**
   ```
   ibmcloud sl block volume-order --storage-type performance --size 20 --iops 100 --os-type LINUX --datacenter dal10
   ```
   {: pre}

4. 驗證是否已建立區塊儲存裝置，並記下磁區的 **`id`**。**附註：**如果您沒有立即看到您的區塊儲存裝置，請等待幾分鐘。然後，重新執行這個指令。
   ```
   ibmcloud sl block volume-list
   ```
   {: pre}

   輸出範例：
   ```
   id         username          datacenter   storage_type                capacity_gb   bytes_used   ip_addr         lunId   active_transactions
   123456789  IBM02SL1234567-8  dal10        performance_block_storage   20            -            161.12.34.123   0       0
   ```
   {: screen}

5. 請檢閱磁區的詳細資料，並記下 **`Target IP`** 及 **`LUN Id`**。
   ```
   ibmcloud sl block volume-detail <volume_ID>
   ```
   {: pre}

   輸出範例：
   ```
   Name                       Value
   ID                         1234567890
   User name                  IBM123A4567890-1
   Type                       performance_block_storage
   Capacity (GB)              20
   LUN Id                     0
   IOPs                       100
   Datacenter                 dal10
   Target IP                  161.12.34.123
   # of Active Transactions   0
   Replicant Count            0
   ```
   {: screen}

6. 授權非 SDS 工作者節點，以存取區塊儲存裝置。將 `<volume_ID>` 取代為您先前擷取之區塊儲存裝置的磁區 ID，以及將 `<private_worker_IP>` 取代為您要連接裝置的非 SDS 工作者節點的專用 IP 位址。

   ```
   ibmcloud sl block access-authorize <volume_ID> -p <private_worker_IP>
   ```
   {: pre}

   輸出範例：
   ```
   The IP address 123456789 was authorized to access <volume_ID>.
   ```
   {: screen}

7. 驗證您的非 SDS 工作者節點是否已順利授權，並記下 **`host_iqn`**、**`username`** 及 **`password`**。
   ```
   ibmcloud sl block access-list <volume_ID>
   ```
   {: pre}

   輸出範例：
   ```
   ID          name                 type   private_ip_address   source_subnet   host_iqn                                      username   password           allowed_host_id
   123456789   <private_worker_IP>  IP     <private_worker_IP>  -               iqn.2018-09.com.ibm:ibm02su1543159-i106288771   IBM02SU1543159-I106288771   R6lqLBj9al6e2lbp   1146581
   ```
   {: screen}

   當指派 **`host_iqn`**、**`username`** 及 **`password`** 時，表示授權已成功。

8. [將區塊儲存裝置連接至您的工作者節點](#attach_block)。


## 將原始區塊儲存空間連接至非 SDS 工作者節點
{: #attach_block}

若要將區塊儲存裝置連接到非 SDS 工作者節點，必須使用 {{site.data.keyword.Bluemix_notm}} Block Volume Attacher 儲存類別和區塊儲存裝置詳細資料來建立持續性磁區 (PV)。
{: shortdesc}

**開始之前**：
- 確保已[自動](#automatic_block)或[手動](#manual_block)建立要用於非 SDS 工作者節點的未格式化且未裝載的原始區塊儲存空間。
- [登入您的帳戶。適用的話，請將適當的資源群組設為目標。設定叢集的環境定義。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

**若要將原始區塊儲存空間連接至非 SDS 工作者節點，請執行下列動作：**
1. 準備建立 PV。  
   - **如果您使用 `mkpvyaml` 容器，請執行下列動作：**
     1. 開啟 `pv-<cluster_name>.yaml` 檔案。
        ```
        nano pv-<cluster_name>.yaml
        ```
        {: pre}

     2. 檢閱 PV 的配置。

   - **如果您手動新增區塊儲存空間，請執行下列動作：**
     1. 建立 `pv.yaml` 檔案。下列指令會使用 `nano` 編輯器來建立檔案。
        ```
        nano pv.yaml
        ```
        {: pre}

     2. 將區塊儲存裝置的詳細資料新增至 PV。
        ```
        apiVersion: v1
        kind: PersistentVolume
        metadata:
          name: <pv_name>
          annotations:
            ibm.io/iqn: "<IQN_hostname>"
            ibm.io/username: "<username>"
            ibm.io/password: "<password>"
            ibm.io/targetip: "<targetIP>"
            ibm.io/lunid: "<lunID>"
            ibm.io/nodeip: "<private_worker_IP>"
            ibm.io/volID: "<volume_ID>"
        spec:
          capacity:
            storage: <size>
          accessModes:
            - ReadWriteOnce
          hostPath:
              path: /
          storageClassName: ibmc-block-attacher
        ```
        {: codeblock}

        <table>
        <caption>瞭解 YAML 檔案元件</caption>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解 YAML 檔案元件</th>
        </thead>
        <tbody>
      	<tr>
          <td><code>metadata.name</code></td>
      	<td>輸入 PV 的名稱。</td>
      	</tr>
        <tr>
        <td><code>ibm.io/iqn</code></td>
        <td>輸入您先前擷取的 IQN 主機名稱。</td>
        </tr>
        <tr>
        <td><code>ibm.io/username</code></td>
        <td>輸入您先前擷取的 IBM Cloud 基礎架構 (SoftLayer) 使用者名稱。</td>
        </tr>
        <tr>
        <td><code>ibm.io/password</code></td>
        <td>輸入您先前擷取的 IBM Cloud 基礎架構 (SoftLayer) 密碼。</td>
        </tr>
        <tr>
        <td><code>ibm.io/targetip</code></td>
        <td>輸入您先前擷取的目標 IP。</td>
        </tr>
        <tr>
        <td><code>ibm.io/lunid</code></td>
        <td>輸入先前擷取到的區塊儲存裝置的 LUN ID。</td>
        </tr>
        <tr>
        <td><code>ibm.io/nodeip</code></td>
        <td>針對要在其中連接區塊儲存裝置，且您先前已授權存取區塊儲存裝置的工作者節點，輸入其專用 IP 位址。</td>
        </tr>
        <tr>
          <td><code>ibm.io/volID</code></td>
        <td>輸入您先前擷取的區塊儲存空間磁區 ID。</td>
        </tr>
        <tr>
        <td><code>storage</code></td>
        <td>輸入您先前所建立區塊儲存裝置的大小。例如，若您的區塊儲存裝置為 20 GB，請輸入 <code>20Gi</code>。</td>
        </tr>
        </tbody>
        </table>
2. 建立 PV，以將區塊儲存裝置連接至您的非 SDS 工作者節點。
   - **如果您使用 `mkpvyaml` 容器，請執行下列動作：**
     ```
     kubectl apply -f pv-<cluster_name>.yaml
     ```
     {: pre}

   - **如果您手動新增區塊儲存空間，請執行下列動作：**
     ```
     kubectl apply -f  block-volume-attacher/pv.yaml
     ```
     {: pre}

3. 驗證區塊儲存空間是否已順利連接至您的工作者節點。
   ```
   kubectl describe pv <pv_name>
   ```
   {: pre}

   輸出範例：
   ```
   Name:            kube-wdc07-cr398f790bc285496dbeb8e9137bc6409a-w1-pv1
   Labels:          <none>
   Annotations:     ibm.io/attachstatus=attached
                    ibm.io/dm=/dev/dm-1
                    ibm.io/iqn=iqn.2018-09.com.ibm:ibm02su1543159-i106288771
                    ibm.io/lunid=0
                    ibm.io/mpath=3600a09803830445455244c4a38754c66
                    ibm.io/nodeip=10.176.48.67
                    ibm.io/password=R6lqLBj9al6e2lbp
                    ibm.io/targetip=161.26.98.114
                    ibm.io/username=IBM02SU1543159-I106288771
                    kubectl.kubernetes.io/last-applied-configuration={"apiVersion":"v1","kind":"PersistentVolume","metadata":{"annotations":{"ibm.io/iqn":"iqn.2018-09.com.ibm:ibm02su1543159-i106288771","ibm.io/lunid":"0"...
   Finalizers:      []
   StorageClass:    ibmc-block-attacher
   Status:          Available
   Claim:
   Reclaim Policy:  Retain
   Access Modes:    RWO
   Capacity:        20Gi
   Node Affinity:   <none>
   Message:
   Source:
       Type:          HostPath (bare host directory volume)
       Path:          /
       HostPathType:
   Events:            <none>
   ```
   {: screen}

   當 **ibm.io/dm** 設為裝置 ID（例如 `/dev/dm/1`），且您可以在 CLI 輸出的**註釋**區段中，看到 **ibm.io/attachstatus=attached**，表示已順利連接區塊儲存裝置。

如果您要分離磁區，請刪除 PV。已分離的磁區仍有授權可供特定工作者節點存取，並且在您使用 {{site.data.keyword.Bluemix_notm}} BlockVolume Attacher 儲存空間類別來建立新的 PV，將不同的磁區連接至相同的工作者節點時，會重新連接。若要避免再次連接舊的分離磁區，請使用 `ibmcloud sl block access-revoke` 指令，解除工作者節點存取已分離磁區的授權。分離磁區並不會將磁區從您的 IBM Cloud 基礎架構 (SoftLayer) 帳戶移除。若要取消磁區的計費，您必須手動[從 IBM Cloud 基礎架構 (SoftLayer) 帳戶移除儲存空間](/docs/containers?topic=containers-cleanup)。
{: note}


