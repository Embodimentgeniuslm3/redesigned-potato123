---

copyright:
  years: 2014, 2019
lastupdated: "2019-05-31"

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



# 將資料儲存在 IBM File Storage for IBM Cloud
{: #file_storage}

{{site.data.keyword.Bluemix_notm}} File Storage 是一種持續性、快速且具彈性的網路連結的 NFS 型檔案儲存空間，可以使用 Kubernetes 持續性磁區 (PV) 將其新增至應用程式。您可以從具有符合您工作負載需求之 GB 大小及 IOPS 的預先定義儲存空間層級中進行選擇。若要瞭解 {{site.data.keyword.Bluemix_notm}} File Storage 是否為正確的儲存空間選項，請參閱[選擇儲存空間解決方案](/docs/containers?topic=containers-storage_planning#choose_storage_solution)。如需定價資訊，請參閱[計費](/docs/infrastructure/FileStorage?topic=FileStorage-about#billing)。
{: shortdesc}

{{site.data.keyword.Bluemix_notm}} File Storage 僅適用於已設定公用網路連線功能的標準叢集。如果叢集無法存取公用網路（例如，防火牆後面的專用叢集或僅已啟用了專用服務端點的叢集），則可以在叢集執行 Kubernetes 1.13.4_1513、1.12.6_1544、1.11.8_1550 版或更高版本時佈建檔案儲存空間。NFS 檔案儲存空間實例是單一區域特有的。如果您有多區域叢集，請考慮[多區域持續性儲存空間選項](/docs/containers?topic=containers-storage_planning#persistent_storage_overview)。
{: important}

## 決定檔案儲存空間配置
{: #file_predefined_storageclass}

{{site.data.keyword.containerlong}} 為檔案儲存空間提供預先定義的儲存空間類別，您可以使用此類別搭配特定配置來佈建檔案儲存空間。
{: shortdesc}

每個儲存空間類別都指定您佈建的檔案儲存空間類型，包括可用的大小、IOPS、檔案系統及保留原則。  

在使用儲存空間類別佈建特定類型的儲存空間之後，您就無法變更儲存裝置的類型或保留原則。不過，如果想要增加您的儲存空間容量及效能，您可以[變更大小及 IOPS](#file_change_storage_configuration)。若要變更儲存空間的類型及保留原則，您必須[建立新的儲存空間實例，並將資料](/docs/containers?topic=containers-kube_concepts#update_storageclass)從舊的儲存空間實例複製到新的儲存空間實例。
{: important}

開始之前：[登入您的帳戶。適用的話，請將適當的資源群組設為目標。設定叢集的環境定義。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

若要決定儲存空間配置，請執行下列動作：

1. 列出 {{site.data.keyword.containerlong}} 中可用的儲存空間類別。
   ```
    kubectl get storageclasses | grep file
    ```
   {: pre}

   輸出範例：
   ```
$ kubectl get storageclasses
    NAME                         TYPE
    ibmc-file-bronze (default)   ibm.io/ibmc-file
    ibmc-file-custom             ibm.io/ibmc-file
    ibmc-file-gold               ibm.io/ibmc-file
    ibmc-file-retain-bronze      ibm.io/ibmc-file
    ibmc-file-retain-custom      ibm.io/ibmc-file
    ibmc-file-retain-gold        ibm.io/ibmc-file
    ibmc-file-retain-silver      ibm.io/ibmc-file
    ibmc-file-silver             ibm.io/ibmc-file
    ```
   {: screen}

2. 檢閱儲存空間類別的配置。
  ```
   kubectl describe storageclass <storageclass_name>
   ```
  {: pre}

   如需每一個儲存空間類別的相關資訊，請參閱[儲存空間類別參照](#file_storageclass_reference)。如果您找不到要尋找的項目，請考慮建立自己的自訂儲存空間類別。若要開始使用，請參閱[自訂的儲存空間類別範例](#file_custom_storageclass)。
   {: tip}

3. 選擇您要佈建的檔案儲存空間類型。
   - **銅級、銀級和金級儲存空間類別：**這些儲存空間類別會佈建[耐久性儲存空間](/docs/infrastructure/FileStorage?topic=FileStorage-about#provisioning-with-endurance-tiers)。耐久性儲存空間可讓您在預先定義的 IOPS 層級選擇儲存空間的大小（以 GB 為單位）。
   - **自訂儲存空間類別：**此儲存空間類別會佈建[效能儲存空間](/docs/infrastructure/FileStorage?topic=FileStorage-about#provisioning-with-performance)。使用效能儲存空間，您更能控制儲存空間大小及 IOPS。

4. 選擇檔案儲存空間的大小及 IOPS。大小及 IOPS 數目定義 IOPS（每秒的輸入/輸出作業數）總數，此 IOPS 可作為儲存空間有多快的指示器。您的儲存空間的 IOPS 總數越大，其處理讀取及寫入作業的速度就越快。
   - **銅級、銀級和金級儲存空間類別：**這些儲存空間類別隨附每 GB 固定數目的 IOPS，並佈建在 SSD 硬碟上。IOPS 總數取決於您選擇的儲存空間大小。您可以選取所容許大小範圍內的任何整數的 GB 大小，例如 20 Gi、256 Gi 或 11854 Gi。若要決定 IOPS 總數，您必須將 IOPS 乘以選取的大小。例如，如果您在隨附每 GB 4 個 IOPS 的銀級儲存空間類別中選取 1000Gi 檔案儲存空間大小，則您的儲存空間總共有 4000 個 IOPS。
     <table>
         <caption>儲存空間類別大小範圍及每 GB IOPS 數目的表格</caption>
         <thead>
         <th>儲存空間類別</th>
         <th>每 GB 的 IOPS 數目</th>
         <th>大小範圍（以 GB 為單位）</th>
         </thead>
         <tbody>
         <tr>
         <td>銅級</td>
         <td>2 IOPS/GB</td>
         <td>20-12000 Gi</td>
         </tr>
         <tr>
         <td>銀級</td>
         <td>4 IOPS/GB</td>
         <td>20-12000 Gi</td>
         </tr>
         <tr>
         <td>金級</td>
         <td>10 IOPS/GB</td>
         <td>20-4000 Gi</td>
         </tr>
         </tbody></table>
   - **自訂儲存空間類別：**當您選擇此儲存空間類別時，更能控制您想要的大小及 IOPS。對於大小，您可以選取所容許大小範圍內的任何整數的 GB 大小。您選擇的大小決定了可供您使用的 IOPS 範圍。您可以選擇在指定範圍內的 100 的倍數的 IOPS。您選擇的 IOPS 是靜態的，不會隨著儲存空間大小一起調整。例如，如果您選擇具有 100 IOPS 的 40Gi，則 IOPS 總數會保留 100。</br></br>IOPS 與 GB 的比例也會決定為您佈建之硬碟的類型。例如，如果您有 100 IOPS 的 500Gi，則您的 IOPS 與 GB 的比例為 0.2。比例小於或等於 0.3 的儲存空間會佈建在 SATA 硬碟上。如果您的比例大於 0.3，則您的儲存空間會佈建在 SSD 硬碟上。
       
     <table>
         <caption>自訂儲存空間類別大小範圍及 IOPS 的表格</caption>
         <thead>
         <th>大小範圍（以 GB 為單位）</th>
         <th>IOPS 範圍（以 100 的倍數表示）</th>
         </thead>
         <tbody>
         <tr>
         <td>20-39 Gi</td>
         <td>100-1000 IOPS</td>
         </tr>
         <tr>
         <td>40-79 Gi</td>
         <td>100-2000 IOPS</td>
         </tr>
         <tr>
         <td>80-99 Gi</td>
         <td>100-4000 IOPS</td>
         </tr>
         <tr>
         <td>100-499 Gi</td>
         <td>100-6000 IOPS</td>
         </tr>
         <tr>
         <td>500-999 Gi</td>
         <td>100-10000 IOPS</td>
         </tr>
         <tr>
         <td>1000-1999 Gi</td>
         <td>100-20000 IOPS</td>
         </tr>
         <tr>
         <td>2000-2999 Gi</td>
         <td>200-40000 IOPS</td>
         </tr>
         <tr>
         <td>3000-3999 Gi</td>
         <td>200-48000 IOPS</td>
         </tr>
         <tr>
         <td>4000-7999 Gi</td>
         <td>300-48000 IOPS</td>
         </tr>
         <tr>
         <td>8000-9999 Gi</td>
         <td>500-48000 IOPS</td>
         </tr>
         <tr>
         <td>10000-12000 Gi</td>
         <td>1000-48000 IOPS</td>
         </tr>
         </tbody></table>

5. 選擇是否要在刪除叢集或持續性磁區要求 (PVC) 之後保留您的資料。
   - 如果要保留資料，則請選擇 `retain` 儲存空間類別。當您刪除 PVC 時，只會刪除 PVC。PV、IBM Cloud 基礎架構 (SoftLayer) 帳戶中的實體儲存裝置，以及您的資料仍然存在。若要收回儲存空間，並再次在您的叢集裡使用它，您必須移除 PV，並遵循[使用現有檔案儲存空間](#existing_file)的步驟。
   - 如果您想要在刪除 PVC 時一併刪除 PV、資料及實體檔案儲存裝置，請選擇沒有 `retain` 的儲存空間類別。

6. 選擇您要依小時還是依月計費。如需相關資訊，請檢查[定價 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/cloud/file-storage/pricing)。依預設，所有檔案儲存裝置都是搭配每小時計費類型進行佈建。如果您選擇依月計費類型，則移除持續性儲存空間時，仍然需要支付它的一個月費用，即使您只是短時間使用也是一樣。
   {: note}

<br />



## 將檔案儲存空間新增至應用程式
{: #add_file}

建立持續性磁區要求 (PVC)，為叢集[動態佈建](/docs/containers?topic=containers-kube_concepts#dynamic_provisioning)檔案儲存空間。動態佈建會自動建立相符的持續性磁區 (PV)，並以 IBM Cloud 基礎架構 (SoftLayer) 帳戶訂購實體儲存裝置。
{:shortdesc}

開始之前：
- 如果您有防火牆，請對叢集所在區域的 IBM Cloud 基礎架構 (SoftLayer) IP 範圍[容許進行 Egress 存取](/docs/containers?topic=containers-firewall#pvc)，這樣您才可以建立 PVC。
- [決定預先定義的儲存空間類別](#file_predefined_storageclass)或建立[自訂的儲存空間類別](#file_custom_storageclass)。

希望在有狀態集合中部署檔案儲存空間嗎？如需相關資訊，請參閱[在有狀態集合中使用檔案儲存空間](#file_statefulset)。
{: tip}

若要新增檔案儲存空間，請執行下列動作：

1.  建立配置檔來定義持續性磁區要求 (PVC)，以及將配置儲存為 `.yaml` 檔案。

    - **銅級、銀級、金級儲存空間類別的範例**：下列 `.yaml` 檔案會建立一個名稱為 `mypvc` 的要求，其儲存空間類別為 `"ibmc-file-sliver"`，計費方式為 `"monthly"`，GB 大小為 `24Gi`。

       ```
       apiVersion: v1
       kind: PersistentVolumeClaim
       metadata:
         name: mypvc
         labels:
           billingType: "monthly"
           region: us-south
           zone: dal13
       spec:
         accessModes:
           - ReadWriteMany
         resources:
           requests:
             storage: 24Gi
         storageClassName: ibmc-file-silver
       ```
       {: codeblock}

    -  **使用自訂儲存空間類別的範例**：下列 `.yaml` 檔案會建立一個名稱為 `mypvc` 的要求，其儲存空間類別為 `ibmc-file-retain-custom`，計費方式為 `"hourly"`，GB 大小為 `45Gi`，IOPS 為 `"300"`。

       ```
       apiVersion: v1
       kind: PersistentVolumeClaim
       metadata:
         name: mypvc
         labels:
           billingType: "hourly"
           region: us-south
           zone: dal13
       spec:
         accessModes:
           - ReadWriteMany
         resources:
           requests:
             storage: 45Gi
             iops: "300"
         storageClassName: ibmc-file-retain-custom
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
       <td>輸入 PVC 名稱。</td>
       </tr>
       <tr>
         <td><code>metadata.labels.billingType</code></td>
          <td>指定計算儲存空間費用的頻率為 "monthly" 或 "hourly"。如果未指定計費類型，則會佈建計費類型為每小時的儲存空間。</td>
       </tr>
       <tr>
       <td><code>metadata.labels.region</code></td>
       <td>選用項目：指定您要在其中佈建檔案儲存空間的地區。若要連接至儲存空間，請在叢集所在的相同地區中建立儲存空間。如果指定地區，則也須指定區域。如果未指定地區或找不到指定的地區，則會在與叢集相同的地區中建立儲存空間。</br></br>若要取得叢集的地區，請執行 `ibmcloud ks cluster-get --cluster <cluster_name_or_ID>`，並在 **Master URL** 中尋找地區字首，例如 `https://c2.eu-de.containers.cloud.ibm.com:11111` 中的 `eu-de`。
       </br></br><strong>提示：</strong>您也可以在[自訂的儲存空間類別](#file_multizone_yaml)中指定這些值，而不是在 PVC 中指定地區及區域。然後，在 PVC 的 <code>metadata.annotations.volume.beta.kubernetes.io/storage-class</code> 區段中使用您的儲存空間類別。如果在儲存空間類別和 PVC 中指定了地區和區域，則會優先採用 PVC 中的值。</td>
       </tr>
       <tr>
       <td><code>metadata.labels.zone</code></td>
       <td>選用項目：指定您要在其中佈建檔案儲存空間的區域。若要在應用程式中使用儲存空間，請在工作者節點所在的相同區域中建立儲存空間。若要檢視工作者節點的區域，請執行 <code>ibmcloud ks workers --cluster &lt;cluster_name_or_ID&gt;</code>，並檢閱 CLI 輸出中的 <strong>Zone</strong> 直欄。如果指定區域，則也須指定地區。如果未指定區域或找不到指定的區域，則會根據循環式基準選取區域。</br></br><strong>提示：</strong>您也可以在[自訂的儲存空間類別](#file_multizone_yaml)中指定這些值，而不是在 PVC 中指定地區及區域。然後，在 PVC 的 <code>metadata.annotations.volume.beta.kubernetes.io/storage-class</code> 區段中使用您的儲存空間類別。如果在儲存空間類別和 PVC 中指定了地區和區域，則會優先採用 PVC 中的值。</td>
       </tr>
       <tr>
       <td><code>spec.accessMode</code></td>
       <td>指定下列其中一個選項：<ul><li><strong>ReadWriteMany：</strong>多個 Pod 可以裝載 PVC。所有 Pod 都可以讀取及寫入至磁區。</li><li><strong>ReadOnlyMany：</strong>多個 Pod 可以裝載 PVC。所有 Pod 都具有唯讀存取權。<li><strong>ReadWriteOnce：</strong>只有一個 Pod 可以裝載 PVC。此 Pod 可以讀取及寫入至磁區。</li></ul></td>
       </tr>
       <tr>
       <td><code>spec.resources.requests.storage</code></td>
       <td>輸入檔案儲存空間大小，以 GB 為單位 (Gi)。在佈建儲存空間之後，您就無法變更檔案儲存空間的大小。請確定指定符合您要儲存的資料量的大小。</td>
       </tr>
       <tr>
       <td><code>spec.resources.requests.iops</code></td>
       <td>此選項僅供自訂儲存空間類別使用 (`ibmc-file-custom / ibmc-file-retain-custom`)。選取容許範圍內的 100 的倍數，來指定儲存空間的 IOPS 總數。如果您選擇的 IOPS 不是所列出的 IOPS，則會將 IOPS 無條件進位。</td>
       </tr>
       <tr>
       <td><code>spec.storageClassName</code></td>
       <td>您要用來佈建檔案儲存空間之儲存空間類別的名稱。您可以選擇使用其中一個 [IBM 提供的儲存空間類別](#file_storageclass_reference)或[建立自己的儲存空間類別](#file_custom_storageclass)。</br>如果您未指定儲存空間類別，則會建立預設儲存空間類別為 <code>ibmc-file-bronze</code> 的 PV。</br></br><strong>提示：</strong>如果您要變更預設儲存空間類別，請執行 <code>kubectl patch storageclass &lt;storageclass&gt; -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'</code>，並將 <code>&lt;storageclass&gt;</code> 取代為儲存空間類別的名稱。</td>
       </tr>
       </tbody></table>

    如果您要使用自訂的儲存空間類別，則請建立具有對應儲存空間類別名稱、有效 IOPS 及大小的 PVC。   
    {: tip}

2.  建立 PVC。

    ```
    kubectl apply -f mypvc.yaml
    ```
    {: pre}

3.  驗證您的 PVC 已建立並已連結至 PV。此處理程序可能需要幾分鐘的時間。

    ```
    kubectl describe pvc mypvc
    ```
    {: pre}

    輸出範例：

    ```
    Name: mypvc
    Namespace: default
    StorageClass: ""
    Status:		Bound
    Volume:		pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2
    Labels:		<none>
    Capacity:	20Gi
    Access Modes:	RWX
    Events:
      FirstSeen	LastSeen	Count	From								SubObjectPath	Type		Reason			Message
      ---------	--------	-----	----								-------------	--------	------			-------
      3m		3m		1	{ibm.io/ibmc-file 31898035-3011-11e7-a6a4-7a08779efd33 }			Normal		Provisioning		External provisioner is provisioning volume for claim "default/my-persistent-volume-claim"
      3m		1m		10	{persistentvolume-controller }							Normal		ExternalProvisioning	cannot find provisioner "ibm.io/ibmc-file", expecting that a volume for the claim is provisioned either manually or via external software
      1m		1m		1	{ibm.io/ibmc-file 31898035-3011-11e7-a6a4-7a08779efd33 }			Normal		ProvisioningSucceeded	Successfully provisioned volume pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2

    ```
    {: screen}

4.  {: #file_app_volume_mount}若要將儲存空間裝載至部署，請建立配置 `.yaml` 檔，並指定連結 PV 的 PVC。

    如果您具有需要非 root 使用者才能寫入至持續性儲存空間的應用程式，或需要裝載路徑由 root 使用者擁有的應用程式，請參閱[將非 root 使用者存取新增至 NFS 檔案儲存空間](/docs/containers?topic=containers-cs_troubleshoot_storage#nonroot)或[對 NFS 檔案儲存空間啟用 root 使用者權限](/docs/containers?topic=containers-cs_troubleshoot_storage#nonroot)。
    {: tip}

    ```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: <deployment_name>
      labels:
        app: <deployment_label>
    spec:
      selector:
        matchLabels:
          app: <app_name>
      template:
        metadata:
          labels:
            app: <app_name>
        spec:
          containers:
          - image: <image_name>
            name: <container_name>
            volumeMounts:
            - name: <volume_name>
              mountPath: /<file_path>
          volumes:
          - name: <volume_name>
            persistentVolumeClaim:
              claimName: <pvc_name>
    ```
    {: codeblock}

    <table>
    <caption>瞭解 YAML 檔案元件</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解 YAML 檔案元件</th>
    </thead>
    <tbody>
        <tr>
    <td><code>metadata.labels.app</code></td>
    <td>部署的標籤。</td>
      </tr>
      <tr>
        <td><code>spec.selector.matchLabels.app</code> <br/> <code>spec.template.metadata.labels.app</code></td>
        <td>應用程式的標籤。</td>
      </tr>
    <tr>
    <td><code>template.metadata.labels.app</code></td>
    <td>部署的標籤。</td>
      </tr>
    <tr>
    <td><code>spec.containers.image</code></td>
    <td>您要使用的映像檔的名稱。若要列出 {{site.data.keyword.registryshort_notm}} 帳戶中的可用映像檔，請執行 `ibmcloud cr image-list`。</td>
    </tr>
    <tr>
    <td><code>spec.containers.name</code></td>
    <td>您要部署至叢集的容器的名稱。</td>
    </tr>
    <tr>
    <td><code>spec.containers.volumeMounts.mountPath</code></td>
    <td>容器內裝載磁區之目錄的絕對路徑。寫入裝載路徑的資料儲存在實體檔案儲存空間實例的 <code>root</code> 目錄下。如果您要在不同的應用程式之間共用磁區，則可以指定每個應用程式的[磁區子路徑 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/concepts/storage/volumes/#using-subpath)。</td>
    </tr>
    <tr>
    <td><code>spec.containers.volumeMounts.name</code></td>
    <td>要裝載至 Pod 之磁區的名稱。</td>
    </tr>
    <tr>
    <td><code>volumes.name</code></td>
    <td>要裝載至 Pod 之磁區的名稱。此名稱通常與 <code>volumeMounts.name</code> 相同。</td>
    </tr>
    <tr>
    <td><code>volumes.persistentVolumeClaim.claimName</code></td>
    <td>連結您要使用之 PV 的 PVC 名稱。</td>
    </tr>
    </tbody></table>

5.  建立部署。
     ```
     kubectl apply -f <local_yaml_path>
     ```
     {: pre}

6.  驗證已順利裝載 PV。

     ```
     kubectl describe deployment <deployment_name>
     ```
     {: pre}

     裝載點在 **Volume Mounts**（磁區裝載）欄位中，而磁區在 **Volumes**（磁區）欄位中。

     ```
      Volume Mounts:
           /var/run/secrets/kubernetes.io/serviceaccount from default-token-tqp61 (ro)
           /volumemount from myvol (rw)
     ...
     Volumes:
       myvol:
         Type:	PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
         ClaimName:	mypvc
         ReadOnly:	false
     ```
     {: screen}

<br />


## 在叢集裡使用現有的檔案儲存空間
{: #existing_file}

如果您具有想要在叢集裡使用的現有實體儲存裝置，則可以手動建立 PV 及 PVC，以[靜態佈建](/docs/containers?topic=containers-kube_concepts#static_provisioning)儲存空間。
{: shortdesc}

開始之前：
- 請確定您至少具有一個工作者節點，存在於與現有檔案儲存空間實例相同的區域中。
- [登入您的帳戶。適用的話，請將適當的資源群組設為目標。設定叢集的環境定義。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

### 步驟1：準備現有的儲存空間。
{: #existing-file-1}

在可以開始將現有儲存空間裝載至應用程式之前，您必須擷取所有適用於 PV 的必要資訊，並準備可在叢集裡存取的儲存空間。  
{: shortdesc}

**若為已使用 `retain` 儲存空間類別佈建的儲存空間：** </br>
如果您已使用 `retain` 儲存空間類別佈建儲存空間，並移除 PVC，則不會自動移除 PV 及實體儲存裝置。若要在叢集裡重複使用儲存空間，您必須先移除剩餘的 PV。

若要在不同的叢集（不是您佈建儲存空間的叢集）中使用現有儲存空間，請遵循[已在叢集外建立的儲存空間](#external_storage)的步驟，將儲存空間新增至工作者節點的子網路。
{: tip}

1. 列出現有 PV。
   ```
   kubectl get pv
   ```
   {: pre}

   尋找屬於持續性儲存空間的 PV。PV 處於 `released` 狀況。

2. 取得 PV 的詳細資料。
   ```
   kubectl describe pv <pv_name>
   ```
   {: pre}

3. 記下 `CapacityGb`、`storageClass`、`failure-domain.beta.kubernetes.io/region`、`failure-domain.beta.kubernetes.io/zone`、`server` 及 `path`。

4. 移除 PV。
   ```
   kubectl delete pv <pv_name>
   ```
   {: pre}

5. 驗證已移除 PV。
   ```
   kubectl get pv
   ```
   {: pre}

</br>

**若為在叢集外佈建的持續性儲存空間：** </br>
如果您想要使用先前佈建但之前從未在叢集裡使用過的現有儲存空間，則必須讓儲存空間可在與工作者節點相同的子網路中使用。

1.  {: #external_storage}從 [IBM Cloud 基礎架構 (SoftLayer) 入口網站 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/classic?) 中，按一下**儲存空間**。
2.  按一下**檔案儲存空間**，並從**動作**功能表中選取**授權主機**。
3.  選取**子網路**。
4.  從下拉清單，選取您的工作者節點所連接的專用 VLAN 子網路。若要尋找工作者節點的子網路，請執行 `ibmcloud ks workers --cluster <cluster_name>`，並且比較工作者節點的 `Private IP` 與您在下拉清單中找到的子網路。
5.  按一下**提交**。
6.  按一下檔案儲存空間的名稱。
7.  記下 `Mount Point`、`size` 及 `Location` 欄位。`Mount Point` 欄位會顯示為 `<nfs_server>:<file_storage_path>`。

### 步驟 2：建立持續性磁區 (PV) 及相符的持續性磁區要求 (PVC)
{: #existing-file-2}

1.  建立 PV 的儲存空間配置檔。包括您先前擷取的值。

    ```
    apiVersion: v1
    kind: PersistentVolume
    metadata:
     name: mypv
     labels:
        failure-domain.beta.kubernetes.io/region: <region>
        failure-domain.beta.kubernetes.io/zone: <zone>
    spec:
     capacity:
       storage: "<size>"
     accessModes:
       - ReadWriteMany
     nfs:
       server: "<nfs_server>"
       path: "<file_storage_path>"
    ```
    {: codeblock}

    <table>
    <caption>瞭解 YAML 檔案元件</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解 YAML 檔案元件</th>
    </thead>
    <tbody>
    <tr>
    <td><code>name</code></td>
    <td>輸入要建立的 PV 物件的名稱。</td>
    </tr>
    <tr>
    <td><code>metadata.labels</code></td>
    <td>輸入您先前擷取的地區及區域。您必須在與持續性儲存空間相同的地區及區域中至少具有一個工作者節點，才能在叢集裡裝載儲存空間。如果儲存空間的 PV 已存在，請[將區域及地區標籤新增](/docs/containers?topic=containers-kube_concepts#storage_multizone)至 PV。</tr>
    <tr>
    <td><code>spec.capacity.storage</code></td>
    <td>輸入您先前擷取之現有 NFS 檔案共用的儲存空間大小。儲存空間大小必須以 GB 為單位寫入（例如，20Gi (20 GB) 或 1000Gi (1 TB)），而且大小必須符合現有檔案共用的大小。</td>
    </tr>
    <tr>
    <td><code>spec.accessMode</code></td>
    <td>指定下列其中一個選項：<ul><li><strong>ReadWriteMany：</strong>多個 Pod 可以裝載 PVC。所有 Pod 都可以讀取及寫入至磁區。</li><li><strong>ReadOnlyMany：</strong>多個 Pod 可以裝載 PVC。所有 Pod 都具有唯讀存取權。<li><strong>ReadWriteOnce：</strong>只有一個 Pod 可以裝載 PVC。此 Pod 可以讀取及寫入至磁區。</li></ul></td>
    </tr>
    <tr>
    <td><code>spec.nfs.server</code></td>
    <td>輸入您先前擷取的 NFS 檔案共用伺服器 ID。</td>
    </tr>
    <tr>
    <td><code>path</code></td>
    <td>輸入您先前擷取之 NFS 檔案共用的路徑。</td>
    </tr>
    </tbody></table>

3.  在叢集裡建立 PV。

    ```
    kubectl apply -f mypv.yaml
    ```
    {: pre}

4.  驗證已建立 PV。

    ```
    kubectl get pv
    ```
    {: pre}

5.  建立另一個配置檔來建立您的 PVC。為了讓 PVC 符合您先前建立的 PV，您必須對 `storage` 及 `accessMode` 選擇相同的值。`storage-class` 欄位必須是空的。如果其中有任何欄位不符合 PV，則會[自動佈建](/docs/containers?topic=containers-kube_concepts#dynamic_provisioning)新 PV 及新的實體儲存空間實例。

    ```
    kind: PersistentVolumeClaim
    apiVersion: v1
    metadata:
     name: mypvc
    spec:
     accessModes:
       - ReadWriteMany
     resources:
       requests:
         storage: "<size>"
     storageClassName:
    ```
    {: codeblock}

6.  建立您的 PVC。

    ```
    kubectl apply -f mypvc.yaml
    ```
    {: pre}

7.  驗證您的 PVC 已建立並已連結至 PV。此處理程序可能需要幾分鐘的時間。

    ```
    kubectl describe pvc mypvc
    ```
    {: pre}

    輸出範例：

    ```
    Name: mypvc
    Namespace: default
    StorageClass: ""
    Status: Bound
    Volume: pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2
    Labels: <none>
    Capacity: 20Gi
    Access Modes: RWX
    Events:
      FirstSeen LastSeen Count From        SubObjectPath Type Reason Message
      --------- -------- ----- ----        ------------- -------- ------ -------
      3m 3m 1 {ibm.io/ibmc-file 31898035-3011-11e7-a6a4-7a08779efd33 } Normal Provisioning External provisioner is provisioning volume for claim "default/my-persistent-volume-claim"
      3m 1m	 10 {persistentvolume-controller } Normal ExternalProvisioning cannot find provisioner "ibm.io/ibmc-file", expecting that a volume for the claim is provisioned either manually or via external software
      1m 1m 1 {ibm.io/ibmc-file 31898035-3011-11e7-a6a4-7a08779efd33 } Normal ProvisioningSucceeded	Successfully provisioned volume pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2
    ```
    {: screen}


您已順利建立 PV，並將它連結至 PVC。現在，叢集使用者可以[裝載 PVC](#file_app_volume_mount) 至其部署，並開始對 PV 物件進行讀寫。

<br />



## 在有狀態集合中使用檔案儲存空間
{: #file_statefulset}

如果您具有有狀態應用程式（例如資料庫），則可以建立有狀態集合，以使用檔案儲存空間來儲存應用程式資料。或者，您也可以使用 {{site.data.keyword.Bluemix_notm}} 資料庫即服務，並將資料儲存在雲端。
{: shortdesc}

**將檔案儲存空間新增至有狀態集合時，需要注意的事項為何？** </br> 若要將儲存空間新增至有狀態集合，您可以在有狀態集合 YAML 的 `volumeClaimTemplates` 區段中指定儲存空間配置。`volumeClaimTemplates` 是您 PVC 的基礎，可以包括儲存空間類別以及您要佈建的檔案儲存空間大小或 IOPS。不過，如果您要在 `volumeClaimTemplates` 中包括標籤，則在建立 PVC 時，Kubernetes 不會包括這些標籤。相反地，您必須將標籤直接新增至有狀態集合。

您無法同時部署兩個有狀態集合。如果您嘗試在完整部署不同的有狀態集合之前建立有狀態集合，則有狀態集合的部署可能會導致非預期的結果。
{: important}

**如何在特定區域中建立有狀態集合？** </br> 在多區域叢集裡，您可以指定要在有狀態集合 YAML 的 `spec.selector.matchLabels` 及 `spec.template.metadata.labels` 區段中建立有狀態集合的區域及地區。或者，您也可以將這些標籤新增至[自訂的儲存空間類別](/docs/containers?topic=containers-kube_concepts#customized_storageclass)，並在有狀態集合的 `volumeClaimTemplates` 區段中使用此儲存空間類別。

**可以將連結 PV 至有狀態 Pod 延遲到 Pod 就緒嗎？**<br>
是，您可以為 PVC [建立自訂儲存空間類別](#file-topology)，其中包括 [`volumeBindingMode: WaitForFirstConsumer` ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/concepts/storage/storage-classes/#volume-binding-mode) 欄位。

**將檔案儲存空間新增至有狀態集合時有哪些選擇？** </br> 如果您要在建立有狀態集合時自動建立 PVC，請使用[動態佈建](#file_dynamic_statefulset)。您也可以選擇使用有狀態集合來[預先佈建 PVC 或使用現有 PVC](#file_static_statefulset)。  

### 動態佈建：在建立有狀態集合時建立 PVC
{: #file_dynamic_statefulset}

如果您要在建立有狀態集合時自動建立 PVC，請使用此選項。
{: shortdesc}

開始之前：[登入您的帳戶。適用的話，請將適當的資源群組設為目標。設定叢集的環境定義。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1. 驗證已完整部署叢集裡的所有現有有狀態集合。如果仍在部署有狀態集合，則無法開始建立有狀態集合。您必須等到叢集裡的所有有狀態集合皆已完整部署，才能避免非預期的結果。
   1. 列出叢集裡的現有有狀態集合。
      ```
      kubectl get statefulset --all-namespaces
      ```
      {: pre}

      輸出範例：
    ```
      NAME              DESIRED   CURRENT   AGE
      mystatefulset     3         3         6s
      ```
      {: screen}

   2. 檢視每個有狀態集合的 **Pod 狀態**，確定已完成有狀態集合的部署。  
      ```
      kubectl describe statefulset <statefulset_name>
      ```
      {: pre}

      輸出範例：
    ```
      Name:               nginx
      Namespace:          default
      CreationTimestamp:  Fri, 05 Oct 2018 13:22:41 -0400
      Selector:           app=nginx,billingType=hourly,region=us-south,zone=dal10
      Labels:             app=nginx
                          billingType=hourly
                          region=us-south
                          zone=dal10
      Annotations:        kubectl.kubernetes.io/last-applied-configuration={"apiVersion":"apps/v1","kind":"StatefulSet","metadata":{"annotations":{},"name":"nginx","namespace":"default"},"spec":{"podManagementPolicy":"Par...
      Replicas:           3 desired | 3 total
      Pods Status:        0 Running / 3 Waiting / 0 Succeeded / 0 Failed
      Pod Template:
        Labels:  app=nginx
                 billingType=hourly
                 region=us-south
                 zone=dal10
      ...
      ```
      {: screen}

      當您在 CLI 輸出的 **Replicas** 區段中找到的抄本數目等於 **Pods Status** 區段中的 **Running** Pod 數目時，即已完整部署有狀態集合。如果尚未完整部署有狀態集合，請先等到部署完成之後，再繼續。

2. 建立有狀態集合的配置檔，以及您用來公開有狀態集合的服務。

  - **用於指定區域的有狀態集合範例：**

    下列範例顯示如何將 NGINX 部署為具有 3 個抄本的有狀態集合。對於每個抄本，都會根據 `ibmc-file-retain-bronze` 儲存空間類別中的規格，來佈建 20 GB 的檔案儲存裝置。所有儲存裝置都會佈建在 `dal10` 區域。因為無法從其他區域存取檔案儲存空間，所以也會將有狀態集合的所有抄本都部署至位於 `dal10` 的工作者節點。

    ```
    apiVersion: v1
    kind: Service
    metadata:
     name: nginx
     labels:
       app: nginx
    spec:
     ports:
     - port: 80
       name: web
     clusterIP: None
     selector:
       app: nginx
    ---
    apiVersion: apps/v1
    kind: StatefulSet
    metadata:
     name: nginx
    spec:
     serviceName: "nginx"
     replicas: 3
     podManagementPolicy: Parallel
     selector:
       matchLabels:
         app: nginx
         billingType: "hourly"
         region: "us-south"
         zone: "dal10"
     template:
       metadata:
         labels:
           app: nginx
           billingType: "hourly"
           region: "us-south"
           zone: "dal10"
       spec:
         containers:
         - name: nginx
           image: k8s.gcr.io/nginx-slim:0.8
           ports:
           - containerPort: 80
             name: web
           volumeMounts:
           - name: myvol
             mountPath: /usr/share/nginx/html
     volumeClaimTemplates:
     - metadata:
         name: myvol
       spec:
         accessModes:
         - ReadWriteOnce
         resources:
           requests:
             storage: 20Gi
             iops: "300" #required only for performance storage
         storageClassName: ibmc-file-retain-bronze
    ```
    {: codeblock}

  - **具有反親緣性規則和延遲檔案儲存空間建立的有狀態集合範例：**

    下列範例顯示如何將 NGINX 部署為具有 3 個抄本的有狀態集合。有狀態集合未指定檔案儲存空間建立所在的地區及區域。相反地，有狀態集合會使用反親緣性規則，確保 Pod 分散到各工作者節點和區域。定義 `app: nginx` 標籤，即可達成工作者節點反親緣性。此標籤指示 Kubernetes 排程器在此工作者節點上已執行具有相同標籤的 Pod 時，不在工作者節點上排定 Pod。`topologykey: failure-domain.beta.kubernetes.io/zone` 標籤會進一步限制此反親緣性規則，以及防止在工作者節點上排定 Pod，該工作者節點與已執行具有 `app: nginx` 標籤的 Pod 的工作者節點位於相同的區域。對於每個有狀態集合 Pod，會建立兩個 PVC（如 `volumeClaimTemplates` 區段中所定義），但會將建立檔案儲存空間實例延遲到排定用於使用儲存空間的有狀態集合 Pod。此設定稱為[拓蹼察覺磁區排程](https://kubernetes.io/blog/2018/10/11/topology-aware-volume-provisioning-in-kubernetes/)。

    ```
    apiVersion: storage.k8s.io/v1
    kind: StorageClass
    metadata:
      name: ibmc-file-bronze-delayed
    parameters:
      billingType: hourly
      classVersion: "2"
      iopsPerGB: "2"
      sizeRange: '[20-12000]Gi'
      type: Endurance
    provisioner: ibm.io/ibmc-file
    reclaimPolicy: Delete
    volumeBindingMode: WaitForFirstConsumer
    ---
    apiVersion: v1
    kind: Service
    metadata:
      name: nginx
      labels:
        app: nginx
    spec:
      ports:
      - port: 80
        name: web
      clusterIP: None
      selector:
        app: nginx
    ---
    apiVersion: apps/v1
    kind: StatefulSet
    metadata:
      name: web
    spec:
      serviceName: "nginx"
      replicas: 3
      podManagementPolicy: "Parallel"
      selector:
        matchLabels:
          app: nginx
      template:
        metadata:
          labels:
            app: nginx
        spec:
          affinity:
            podAntiAffinity:
              preferredDuringSchedulingIgnoredDuringExecution:
              - weight: 100
            podAffinityTerm:
              labelSelector:
                matchExpressions:
                    - key: app
                  operator: In
                  values:
                      - nginx
                  topologyKey: failure-domain.beta.kubernetes.io/zone
          containers:
          - name: nginx
            image: k8s.gcr.io/nginx-slim:0.8
            ports:
            - containerPort: 80
              name: web
            volumeMounts:
            - name: www
              mountPath: /usr/share/nginx/html
            - name: wwwww
              mountPath: /tmp1
      volumeClaimTemplates:
      - metadata:
          name: myvol1
        spec:
          accessModes:
          - ReadWriteMany # access mode
          resources:
            requests:
              storage: 20Gi
          storageClassName: ibmc-file-bronze-delayed
      - metadata:
          name: myvol2
        spec:
          accessModes:
          - ReadWriteMany # access mode
          resources:
            requests:
              storage: 20Gi
          storageClassName: ibmc-file-bronze-delayed
    ```
    {: codeblock}

    <table>
    <caption>瞭解有狀態集合 YAML 檔案元件</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解有狀態集合 YAML 檔案元件</th>
    </thead>
    <tbody>
    <tr>
    <td style="text-align:left"><code>metadata.name</code></td>
    <td style="text-align:left">輸入有狀態集合的名稱。您輸入的名稱用來建立 PVC 名稱，格式如下：<code>&lt;volume_name&gt;-&lt;statefulset_name&gt;-&lt;replica_number&gt;</code>。</td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.serviceName</code></td>
    <td style="text-align:left">輸入您要用來公開有狀態集合的服務名稱。</td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.replicas</code></td>
    <td style="text-align:left">輸入有狀態集合的抄本數目。</td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.podManagementPolicy</code></td>
    <td style="text-align:left">輸入您要用於有狀態集合的 Pod 管理原則。請從下列選項中進行選擇：<ul><li><strong><code>OrderedReady</code></strong>：使用此選項，逐一部署有狀態集合抄本。例如，如果您已指定 3 個抄本，則 Kubernetes 會為您的第一個抄本建立 PVC、等待 PVC 連結、部署有狀態集合抄本，以及將 PVC 裝載至抄本。部署完成之後，會部署第二個抄本。如需此選項的相關資訊，請參閱 [`OrderedReady` Pod 管理 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/#orderedready-pod-management)。</li><li><strong>Parallel：</strong>使用此選項，會同時開始部署所有有狀態集合抄本。如果您的應用程式支援平行部署抄本，則請使用此選項來儲存您 PVC 及有狀態集合抄本的部署時間。</li></ul></td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.selector.matchLabels</code></td>
    <td style="text-align:left">輸入您要內含在有狀態集合及 PVC 的所有標籤。Kubernetes 無法辨識有狀態集合的 <code>volumeClaimTemplates</code> 中所內含的標籤。您可能想要包含的範例標籤如下：<ul><li><code><strong>region</strong></code> 及 <code><strong>zone</strong></code>：如果您要在某個特定區域中建立所有有狀態集合抄本及 PVC，請新增這兩個標籤。您也可以在所使用的儲存空間類別中指定區域及地區。如果您未指定區域及地區，而且具有多區域叢集，則會根據循環式基準選取儲存空間佈建所在的區域，以在所有區域均勻地平衡磁區要求。</li><li><code><strong>billingType</strong></code>：輸入您要用於 PVC 的計費類型。請選擇 <code>hourly</code> 或 <code>monthly</code>。如果您未指定此標籤，則會使用依小時計費類型來建立所有 PVC。</li></ul></td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.template.metadata.labels</code></td>
    <td style="text-align:left">輸入您已新增至 <code>spec.selector.matchLabels</code> 區段的相同標籤。</td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.template.spec.affinity</code></td>
    <td style="text-align:left">指定反親緣性規則，確定有狀態集合 Pod 分散到各工作者節點和區域。此範例顯示偏好不要在工作者節點上排定有狀態集合 Pod 的反親緣性規則，其中 Pod 具有 `app: nginx` 標籤。如果工作者節點與具有 `app: nginx` 標籤的 Pod 位於相同的區域，則 `topologykey: failure-domain.beta.kubernetes.io/zone` 會進一步限制此反親緣性規則，以及防止在工作者節點上排定 Pod。使用此反親緣性規則，您可以跨各工作者節點和區域達到反親緣性。</td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.volumeClaimTemplates.metadata.name</code></td>
    <td style="text-align:left">輸入磁區的名稱。使用您在 <code>spec.containers.volumeMount.name</code> 區段中定義的相同名稱。您在這裡輸入的名稱用來建立 PVC 名稱，格式如下：<code>&lt;volume_name&gt;-&lt;statefulset_name&gt;-&lt;replica_number&gt;</code>。</td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.volumeClaimTemplates.spec.resources.</code></br><code>requests.storage</code></td>
    <td style="text-align:left">輸入檔案儲存空間大小，以 GB 為單位 (Gi)。</td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.volumeClaimTemplates.spec.resources.</code></br><code>requests.iops</code></td>
    <td style="text-align:left">如果您要佈建[效能儲存空間](#file_predefined_storageclass)，請輸入 IOPS 數目。如果您使用耐久性儲存空間類別，並指定 IOPS 數目，則會忽略 IOPS 數目。相反地，會使用儲存空間類別中所指定的 IOPS。</td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.volumeClaimTemplates.</code></br><code>spec.storageClassName</code></td>
    <td style="text-align:left">輸入您要使用的儲存空間類別。若要列出現有儲存空間類別，請執行 <code>kubectl get storageclasses | grep file</code>。如果您未指定儲存空間類別，則會建立 PVC，其具有叢集裡所設定的預設儲存空間類別。請確定預設儲存空間類別使用 <code>ibm.io/ibmc-file</code> 佈建者，以使用檔案儲存空間佈建有狀態集合。</td>
    </tr>
    </tbody></table>

4. 建立有狀態集合。
   ```
   kubectl apply -f statefulset.yaml
   ```
   {: pre}

5. 等待部署有狀態集合。
   ```
   kubectl describe statefulset <statefulset_name>
   ```
   {: pre}

   若要查看 PVC 的現行狀態，請執行 `kubectl get pvc`。PVC 名稱的格式為 `<volume_name>-<statefulset_name>-<replica_number>`。
   {: tip}

### 靜態佈建：搭配使用現有 PVC 與有狀態集合
{: #file_static_statefulset}

您可以在使用有狀態集合來建立有狀態集合或使用現有 PVC 之前，預先佈建 PVC。
{: shortdesc}

如果您[在建立有狀態集合時動態佈建 PVC](#file_dynamic_statefulset)，則會根據有狀態集合 YAML 檔案中所使用的值來指派 PVC 名稱。若要讓有狀態集合使用現有 PVC，則 PVC 名稱必須符合使用動態佈建時所自動建立的名稱。

開始之前：[登入您的帳戶。適用的話，請將適當的資源群組設為目標。設定叢集的環境定義。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1. 如果您要在建立有狀態集合之前預先佈建 PVC，請遵循[將檔案儲存空間新增至應用程式](#add_file)中的步驟 1-3，以建立每個有狀態集合抄本的 PVC。請確定您建立名稱遵循下列格式的 PVC：`<volume_name>-<statefulset_name>-<replica_number>`。
   - **`<volume_name>`**：使用您要在有狀態集合的 `spec.volumeClaimTemplates.metadata.name` 區段中指定的名稱，例如 `nginxvol`。
   - **`<statefulset_name>`**：使用您要在有狀態集合的 `metadata.name` 區段中指定的名稱，例如 `nginx_statefulset`。
   - **`<replica_number>`**：輸入抄本數目，從 0 開始。

   例如，如果您必須建立 3 個有狀態集合抄本，請建立具有下列名稱的 3 個 PVC：`nginxvol-nginx_statefulset-0`、`nginxvol-nginx_statefulset-1` 及 `nginxvol-nginx_statefulset-2`。  

   要為現有檔案儲存空間實例建立 PVC 和 PV 嗎？使用[靜態佈建](#existing_file)，以建立 PVC 和 PV。
   {: tip}

2. 遵循[動態佈建：在建立有狀態集合時建立 PVC](#file_dynamic_statefulset) 中的步驟，以建立有狀態集合。PVC 名稱的格式為 `<volume_name>-<statefulset_name>-<replica_number>`。請務必使用有狀態集合規格中的下列 PVC 名稱值：
   - **`spec.volumeClaimTemplates.metadata.name`**：輸入 PVC 名稱的 `<volume_name>`。
   - **`metadata.name`**：輸入 PVC 名稱的 `<statefulset_name>`。
   - **`spec.replicas`**：輸入您要針對有狀態集合建立的抄本數目。抄本數目必須等於您先前所建立的 PVC 數目。

   如果 PVC 位於不同的區域，請不要在有狀態集合中包含地區或區域標籤。
   {: note}

3. 驗證已在有狀態集合抄本 Pod 中使用 PVC。
   1. 列出叢集裡的 Pod。識別屬於有狀態集合的 Pod。
      ```
      kubectl get pods
      ```
      {: pre}

   2. 驗證現有 PVC 已裝載至有狀態集合抄本。請檢閱 CLI 輸出之 **`Volumes`** 區段中的 **`ClaimName`**。
      ```
      kubectl describe pod <pod_name>
      ```
      {: pre}

      輸出範例：
      ```
      Name:           nginx-0
      Namespace:      default
      Node:           10.xxx.xx.xxx/10.xxx.xx.xxx
      Start Time:     Fri, 05 Oct 2018 13:24:59 -0400
      ...
      Volumes:
        myvol:
          Type:       PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
          ClaimName:  myvol-nginx-0
     ...
      ```
      {: screen}

<br />


## 變更現有儲存裝置的大小及 IOPS
{: #file_change_storage_configuration}

如果您要增加儲存空間容量或效能，則可以修改現有磁區。
{: shortdesc}

如需計費的相關問題，以及尋找如何使用 {{site.data.keyword.Bluemix_notm}} 主控台來修改儲存空間的步驟，請參閱[擴充檔案共用容量](/docs/infrastructure/FileStorage?topic=FileStorage-expandCapacity#expandCapacity)。
{: tip}

1. 列出叢集裡的 PVC，並記下 **VOLUME** 直欄中關聯 PV 的名稱。
   ```
   kubectl get pvc
   ```
   {: pre}

   輸出範例：
   ```
   NAME             STATUS    VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS        AGE
   myvol            Bound     pvc-01ac123a-123b-12c3-abcd-0a1234cb12d3   20Gi       RWX            ibmc-file-bronze    147d
   ```
   {: screen}

2. 列出 PVC 所連結之 PV 的詳細資料，以擷取與 PVC 相關聯之實體檔案儲存空間的 **`StorageType`**、**`volumeId`** 及 **`server`**。將 `<pv_name>` 取代為您在前一個步驟中擷取的 PV 名稱。儲存空間類型、磁區 ID 及伺服器名稱會顯示在 CLI 輸出的 **`Labels`** 區段中。
   ```
   kubectl describe pv <pv_name>
   ```
   {: pre}

   輸出範例：
   ```
   Name:            pvc-4b62c704-5f77-11e8-8a75-b229c11ba64a
   Labels:          CapacityGb=20
                    Datacenter=dal10
                    Iops=2
                    StorageType=ENDURANCE
                    Username=IBM02SEV1543159_6
                    billingType=hourly
                    failure-domain.beta.kubernetes.io/region=us-south
                    failure-domain.beta.kubernetes.io/zone=dal10
                    path=IBM01SEV1234567_8ab12t
                    server=fsf-dal1001g-fz.adn.networklayer.com
                    volumeId=12345678
   ...
   ```
   {: screen}

3. 修改 IBM Cloud 基礎架構 (SoftLayer) 帳戶中磁區的大小或 IOPS。

   效能儲存空間的範例：
   ```
   ibmcloud sl file volume-modify <volume_ID> --new-size <size> --new-iops <iops>
   ```
   {: pre}

   耐久性儲存空間的範例：
   ```
   ibmcloud sl file volume-modify <volume_ID> --new-size <size> --new-tier <iops>
   ```
   {: pre}

   <table>
   <caption>瞭解指令的元件</caption>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解 YAML 檔案元件</th>
   </thead>
   <tbody>
   <tr>
   <td><code>&lt;volume_ID&gt;</code></td>
   <td>輸入您先前擷取的磁區 ID。</td>
   </tr>
   <tr>
   <td><code>&lt;new-size&gt;</code></td>
   <td>以 GB 為單位輸入磁區的新大小。如需有效的大小，請參閱[決定檔案儲存空間配置](#file_predefined_storageclass)。您輸入的大小必須大於或等於磁區的現行大小。如果您未指定新大小，則會使用磁區的現行大小。</td>
   </tr>
   <tr>
   <td><code>&lt;new-iops&gt;</code></td>
   <td>僅限效能儲存空間。輸入您想要的新 IOPS 數。如需有效的 IOPS，請參閱[決定檔案儲存空間配置](#file_predefined_storageclass)。如果您未指定 IOPS，則會使用現行 IOPS。<p class="note">如果磁區的原始 IOPS/GB 比例小於 0.3，則新的 IOPS/GB 比例必須小於 0.3。如果磁區的原始 IOPS/GB 比例大於或等於 0.3，則磁區的新 IOPS/GB 比例必須大於或等於 0.3。</p> </td>
   </tr>
   <tr>
   <td><code>&lt;new-tier&gt;</code></td>
   <td>僅限耐久性儲存空間。輸入您想要的每 GB 新 IOPS 數。如需有效的 IOPS，請參閱[決定檔案儲存空間配置](#file_predefined_storageclass)。如果您未指定 IOPS，則會使用現行 IOPS。<p class="note">如果磁區的原始 IOPS/GB 比例小於 0.25，則新的 IOPS/GB 比例必須小於 0.25。如果磁區的原始 IOPS/GB 比例大於或等於 0.25，則磁區的新 IOPS/GB 比例必須大於或等於 0.25。</p> </td>
   </tr>
   </tbody>
   </table>

   輸出範例：
   ```
   Order 31020713 was placed successfully!.
   > Storage as a Service

   > 40 GBs

   > 2 IOPS per GB

   > 20 GB Storage Space (Snapshot Space)

   You may run 'ibmcloud sl file volume-list --order 12345667' to find this file volume after it is ready.
   ```
   {: screen}

4. 如果您已變更磁區的大小，並使用 Pod 中的磁區，請登入 Pod 以驗證新的大小。
   1. 列出所有使用 PVC 的 Pod。
      ```
      kubectl get pods --all-namespaces -o=jsonpath='{range .items[*]}{"\n"}{.metadata.name}{":\t"}{range .spec.volumes[*]}{.persistentVolumeClaim.claimName}{" "}{end}{end}' | grep "<pvc_name>"
      ```
      {: pre}

      Pod 會以下列格式傳回：`<pod_name>: <pvc_name>`。
   2. 登入 Pod。
      ```
      kubectl exec -it <pod_name> bash
      ```
      {: pre}

   3. 顯示磁碟用量統計資料，並尋找您先前擷取之磁區的伺服器路徑。
      ```
      df -h
      ```
      {: pre}

      輸出範例：
    ```
      Filesystem                                                      Size  Used Avail Use% Mounted on
      overlay                                                          99G  4.8G   89G   6% /
      tmpfs                                                            64M     0   64M   0% /dev
      tmpfs                                                           7.9G     0  7.9G   0% /sys/fs/cgroup
      fsf-dal1001g-fz.adn.networklayer.com:/IBM01SEV1234567_6/data01   40G     0   40G   0% /myvol
      ```
      {: screen}


## 變更預設 NFS 版本
{: #nfs_version}

檔案儲存空間的版本會決定用來與 {{site.data.keyword.Bluemix_notm}} 檔案儲存空間伺服器通訊的通訊協定。依預設，所有檔案儲存空間實例都已設定 NFS 第 4 版。如果您的應用程式需要特定版本才能適當地運作，則您可以將現有的 PV 變更為較舊的 NFS 版本。
{: shortdesc}

若要變更預設 NFS 版本，您可以建立新的儲存空間類別，以在叢集裡動態佈建檔案儲存空間，或選擇變更已裝載至 Pod 的現有 PV。

若要套用最新的安全更新項目，並提高效能，請使用預設 NFS 版本，而且不要變更為較舊的 NFS 版本。
{: important}

**若要建立具有特定 NFS 版本的自訂儲存空間類別，請執行下列動作：**
1. 使用您要佈建的 NFS 版本來建立[自訂的儲存空間類別](#nfs_version_class)。
2. 在叢集裡建立儲存空間類別。
   ```
   kubectl apply -f nfsversion_storageclass.yaml
   ```
   {: pre}

3. 驗證已建立自訂的儲存空間類別。
   ```
   kubectl get storageclasses
   ```
   {: pre}

4. 使用自訂的儲存空間類別來佈建[檔案儲存空間](#add_file)。

**若要變更現有 PV 以使用不同的 NFS 版本，請執行下列動作：**

1. 取得您要在其中變更 NFS 版本之檔案儲存空間的 PV，並記下 PV 的名稱。
   ```
   kubectl get pv
   ```
   {: pre}

2. 將註釋新增至 PV。將 `<version_number>` 取代為您要使用的 NFS 版本。例如，若要變更為 NFS 3.0 版，請輸入 **3**。  
   ```
   kubectl patch pv <pv_name> -p '{"metadata": {"annotations":{"volume.beta.kubernetes.io/mount-options":"vers=<version_number>"}}}'
   ```
   {: pre}

3. 刪除使用檔案儲存空間的 Pod，然後重建 Pod。
   1. 將 Pod YAML 儲存至本端機器。
      ```
      kubect get pod <pod_name> -o yaml > <filepath/pod.yaml>
      ```
      {: pre}

   2. 刪除 Pod。
      ```
      kubectl deleted pod <pod_name>
      ```
      {: pre}

   3. 重建 Pod。
      ```
      kubectl apply -f pod.yaml
      ```
      {: pre}

4. 等待要部署的 Pod。
   ```
   kubectl get pods
   ```
   {: pre}

   狀態變更為 `Running` 時，即已完整部署 Pod。

5. 登入 Pod。
   ```
   kubectl exec -it <pod_name> sh
   ```
   {: pre}

6. 驗證檔案儲存空間已裝載您先前指定的 NFS 版本。
   ```
   mount | grep "nfs" | awk -F" |," '{ print $5, $8 }'
   ```
   {: pre}

   輸出範例：
   ```
   nfs vers=3.0
   ```
   {: screen}

<br />


## 備份及還原資料
{: #file_backup_restore}

檔案儲存空間會佈建至與叢集裡工作者節點相同的位置。儲存空間是在叢集化伺服器上由 IBM 管理，以在伺服器關閉時提供可用性。不過，如果整個位置失敗，則不會自動備份檔案儲存空間，且可能無法存取它們。若要避免資料遺失或損壞，您可以設定定期備份，以在需要時使用它們來還原您的資料。
{: shortdesc}

檢閱檔案儲存空間的下列備份及還原選項：

<dl>
  <dt>設定定期 Snapshot</dt>
  <dd><p>您可以[針對檔案儲存空間設定定期 Snapshot](/docs/infrastructure/FileStorage?topic=FileStorage-snapshots)，這是唯讀映像檔，會擷取實例在某個時間點的狀況。若要儲存 Snapshot，您必須在檔案儲存空間上要求 Snapshot 空間。Snapshot 儲存於相同區域內的現有儲存空間實例上。如果使用者不小心從磁區移除重要資料，您可以從 Snapshot 還原資料。</br><strong>若要建立磁區的 Snapshot，請執行下列動作：</strong><ol><li>[登入您的帳戶。適用的話，請將適當的資源群組設為目標。設定叢集的環境定義。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)</li><li>登入 `ibmcloud sl` CLI。<pre class="pre"><code>ibmcloud sl init</code></pre></li><li>列出叢集裡的現有 PV。<pre class="pre"><code>kubectl get pv</code></pre></li><li>取得您要建立 Snapshot 空間之 PV 的詳細資料，並記下磁區 ID、大小及 IOPS。<pre class="pre"><code>kubectl describe pv &lt;pv_name&gt;</code></pre> 可在 CLI 輸出的 <strong>Labels</strong> 區段中找到磁區 ID、大小及 IOPS。</li><li>使用您在前一個步驟中擷取的參數，建立現有磁區的 Snapshot 大小。<pre class="pre"><code>ibmcloud sl file snapshot-order &lt;volume_ID&gt; --size &lt;size&gt; --tier &lt;iops&gt;</code></pre></li><li>等待要建立的 Snapshot 大小。<pre class="pre"><code>ibmcloud sl file volume-detail &lt;volume_ID&gt;</code></pre>CLI 輸出中的 <strong>Snapshot Size (GB)</strong> 從 0 變更為您所訂購的大小時，即已順利佈建 Snapshot 大小。</li><li>為您的磁區建立 Snapshot，並記下為您建立的 Snapshot ID。<pre class="pre"><code>ibmcloud sl file snapshot-create &lt;volume_ID&gt;</code></pre></li><li>驗證已順利建立 Snapshot。<pre class="pre"><code>ibmcloud sl file snapshot-list &lt;volume_ID&gt;</code></pre></li></ol></br><strong>若要將資料從 Snapshot 還原至現有磁區，請執行下列動作：</strong><pre class="pre"><code>ibmcloud sl file snapshot-restore &lt;volume_ID&gt; &lt;snapshot_ID&gt;</code></pre></p></dd>
  <dt>將 Snapshot 抄寫至另一個區域</dt>
 <dd><p>若要在發生區域故障時保護資料，您可以[將 Snapshot 抄寫](/docs/infrastructure/FileStorage?topic=FileStorage-replication#replication)至另一個區域中設定的檔案儲存空間實例。資料只能從主要儲存空間抄寫至備份儲存空間。您無法將抄寫的檔案儲存空間實例裝載至叢集。當主要儲存空間失敗時，您可以手動將抄寫的備份儲存空間設為主要儲存空間。然後，您可以將它裝載至叢集。還原主要儲存空間之後，您可以從備份儲存空間中還原資料。</p></dd>
 <dt>複製儲存空間</dt>
 <dd><p>您可以在與原始儲存空間實例相同的區域中[複製檔案儲存空間實例](/docs/infrastructure/FileStorage?topic=FileStorage-duplicatevolume#duplicatevolume)。在建立複本的時間點，複本具有與原始儲存空間實例相同的資料。與抄本不同，請使用複本作為獨立於原始儲存空間實例外的儲存空間實例。若要複製，請先[設定磁區的 Snapshot](/docs/infrastructure/FileStorage?topic=FileStorage-snapshots)。</p></dd>
  <dt>將資料備份至 {{site.data.keyword.cos_full}}</dt>
  <dd><p>您可以使用 [**ibm-backup-restore image**](/docs/services/RegistryImages/ibm-backup-restore?topic=RegistryImages-ibmbackup_restore_starter#ibmbackup_restore_starter)，在叢集裡啟動一個備份及還原 Pod。這個 Pod 包含一個 Script，它會針對叢集裡的任何持續性磁區要求 (PVC) 執行一次性或定期備份。資料會儲存在您於區域中設定的 {{site.data.keyword.cos_full}} 實例中。</p>
  <p>若要讓資料有更高的可用性，並在發生區域故障時保護應用程式，請設定第二個 {{site.data.keyword.cos_full}} 實例，並在區域之間抄寫資料。如果您需要從 {{site.data.keyword.cos_full}} 實例還原資料，請使用隨該映像檔所提供的還原 Script。</p></dd>
<dt>在 Pod 與容器之間複製資料</dt>
<dd><p>您可以使用 `kubectl cp` [指令 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/reference/kubectl/overview/#cp)，在叢集的 Pod 或特定容器之間複製檔案及目錄。</p>
<p>開始之前：[登入您的帳戶。適用的話，請將適當的資源群組設為目標。設定叢集的環境定義。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)如果未使用 <code>-c</code> 來指定容器，則指令會使用 Pod 中第一個可用的容器。</p>
<p>您可以透過下列各種方式來使用指令：</p>
<ul>
<li>將資料從本端機器複製到叢集裡的 Pod：<pre class="pre"><code>kubectl cp <var>&lt;local_filepath&gt;/&lt;filename&gt;</var> <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;</var></code></pre></li>
<li>將資料從叢集裡的 Pod 複製到本端機器：<pre class="pre"><code>kubectl cp <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;/&lt;filename&gt;</var> <var>&lt;local_filepath&gt;/&lt;filename&gt;</var></code></pre></li>
<li>將資料從本端機器複製到叢集裡的 Pod 中執行的特定容器：<pre class="pre"><code>kubectl cp <var>&lt;local_filepath&gt;/&lt;filename&gt;</var> <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;</var> -c <var>&lt;container></var></code></pre></li>
</ul></dd>
  </dl>

<br />


## 儲存空間類別參照
{: #file_storageclass_reference}

### 銅級
{: #file_bronze}

<table>
<caption>檔案儲存空間類別：銅級</caption>
<thead>
<th>特徵</th>
<th>設定</th>
</thead>
<tbody>
<tr>
<td>名稱</td>
<td><code>ibmc-file-bronze</code></br><code>ibmc-file-retain-bronze</code></td>
</tr>
<tr>
<td>類型</td>
<td>[耐久性儲存空間](/docs/infrastructure/FileStorage?topic=FileStorage-about#provisioning-with-endurance-tiers)</td>
</tr>
<tr>
<td>檔案系統</td>
<td>NFS</td>
</tr>
<tr>
<td>每 GB 的 IOPS 數目</td>
<td>2</td>
</tr>
<tr>
<td>大小範圍（以 GB 為單位）</td>
<td>20-12000 Gi</td>
</tr>
<tr>
<td>硬碟</td>
<td>SSD</td>
</tr>
<tr>
<td>計費</td>
<td>每小時</td>
</tr>
<tr>
<td>定價</td>
<td>[定價資訊 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/cloud/file-storage/pricing)</td>
</tr>
</tbody>
</table>


### 銀級
{: #file_silver}

<table>
<caption>檔案儲存空間類別：銀級</caption>
<thead>
<th>特徵</th>
<th>設定</th>
</thead>
<tbody>
<tr>
<td>名稱</td>
<td><code>ibmc-file-silver</code></br><code>ibmc-file-retain-silver</code></td>
</tr>
<tr>
<td>類型</td>
<td>[耐久性儲存空間](/docs/infrastructure/FileStorage?topic=FileStorage-about#provisioning-with-endurance-tiers)</td>
</tr>
<tr>
<td>檔案系統</td>
<td>NFS</td>
</tr>
<tr>
<td>每 GB 的 IOPS 數目</td>
<td>4</td>
</tr>
<tr>
<td>大小範圍（以 GB 為單位）</td>
<td>20-12000 Gi</td>
</tr>
<tr>
<td>硬碟</td>
<td>SSD</td>
</tr>
<tr>
<td>計費</td>
<td>每小時</li></ul></td>
</tr>
<tr>
<td>定價</td>
<td>[定價資訊 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/cloud/file-storage/pricing)</td>
</tr>
</tbody>
</table>

### 金級
{: #file_gold}

<table>
<caption>檔案儲存空間類別：金級</caption>
<thead>
<th>特徵</th>
<th>設定</th>
</thead>
<tbody>
<tr>
<td>名稱</td>
<td><code>ibmc-file-gold</code></br><code>ibmc-file-retain-gold</code></td>
</tr>
<tr>
<td>類型</td>
<td>[耐久性儲存空間](/docs/infrastructure/FileStorage?topic=FileStorage-about#provisioning-with-endurance-tiers)</td>
</tr>
<tr>
<td>檔案系統</td>
<td>NFS</td>
</tr>
<tr>
<td>每 GB 的 IOPS 數目</td>
<td>10</td>
</tr>
<tr>
<td>大小範圍（以 GB 為單位）</td>
<td>20-4000 Gi</td>
</tr>
<tr>
<td>硬碟</td>
<td>SSD</td>
</tr>
<tr>
<td>計費</td>
<td>每小時</li></ul></td>
</tr>
<tr>
<td>定價</td>
<td>[定價資訊 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/cloud/file-storage/pricing)</td>
</tr>
</tbody>
</table>

### 自訂
{: #file_custom}

<table>
<caption>檔案儲存空間類別：自訂</caption>
<thead>
<th>特徵</th>
<th>設定</th>
</thead>
<tbody>
<tr>
<td>名稱</td>
<td><code>ibmc-file-custom</code></br><code>ibmc-file-retain-custom</code></td>
</tr>
<tr>
<td>類型</td>
<td>[效能](/docs/infrastructure/FileStorage?topic=FileStorage-about#provisioning-with-performance)</td>
</tr>
<tr>
<td>檔案系統</td>
<td>NFS</td>
</tr>
<tr>
<td>IOPS 及大小</td>
<td><p><strong>大小範圍（以 GB 為單位）/ IOPS 範圍（以 100 的倍數表示）</strong></p><ul><li>20-39 Gi / 100-1000 IOPS</li><li>40-79 Gi / 100-2000 IOPS</li><li>80-99 Gi / 100-4000 IOPS</li><li>100-499 Gi / 100-6000 IOPS</li><li>500-999 Gi / 100-10000 IOPS</li><li>1000-1999 Gi / 100-20000 IOPS</li><li>2000-2999 Gi / 200-40000 IOPS</li><li>3000-3999 Gi / 200-48000 IOPS</li><li>4000-7999 Gi / 300-48000 IOPS</li><li>8000-9999 Gi / 500-48000 IOPS</li><li>10000-12000 Gi / 1000-48000 IOPS</li></ul></td>
</tr>
<tr>
<td>硬碟</td>
<td>IOPS 與 GB 的比例決定佈建之硬碟的類型。若要判定 IOPS 與 GB 的比例，您可將 IOPS 除以儲存空間的大小。</br></br>範例：</br>您選擇具有 100 IOPS 的 500Gi 儲存空間。您的比例為 0.2 (100 IOPS/500Gi)。</br></br><strong>每個比例之硬碟類型的概觀</strong><ul><li>小於或等於 0.3：SATA</li><li>大於 0.3：SSD</li></ul></td>
</tr>
<tr>
<td>計費</td>
<td>每小時</li></ul></td>
</tr>
<tr>
<td>定價</td>
<td>[定價資訊 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/cloud/file-storage/pricing)</td>
</tr>
</tbody>
</table>

<br />


## 自訂的儲存空間類別範例
{: #file_custom_storageclass}

您可以建立自訂的儲存空間類別，並使用 PVC 中的儲存空間類別。
{: shortdesc}

{{site.data.keyword.containerlong_notm}} 提供[預先定義的儲存空間類別](#file_storageclass_reference)，以佈建具有特定層級及配置的檔案儲存空間。在某些情況下，建議您使用預先定義儲存空間類別中未涵蓋的不同配置來佈建儲存空間。您可以使用本主題中的範例，來尋找範例自訂儲存空間類別。

若要建立自訂的儲存空間類別，請參閱[自訂儲存空間類別](/docs/containers?topic=containers-kube_concepts#customized_storageclass)。然後，[在 PVC 中使用自訂的儲存空間類別](#add_file)。

### 建立拓蹼察覺儲存空間
{: #file-topology}

若要在多區域叢集裡使用檔案儲存空間，必須將 Pod 排定至與檔案儲存空間實例相同的區域，以讀取及寫入磁區。在 Kubernetes 引進拓蹼察覺磁區排程之前，儲存空間的動態佈建會在建立 PVC 時自動建立檔案儲存空間實例。然後，在建立 Pod 時，Kubernetes 排程器會嘗試將 Pod 部署至與檔案儲存空間實例相同的資料中心內的工作者節點。
{: shortdesc}

在不瞭解 Pod 限制的情況下，建立檔案儲存空間實例可能會導致不想要的結果。例如，Pod 可能無法排定至與儲存空間相同的工作者節點，因為工作者節點的資源不足，或者工作者節點有污點，而且不容許排定 Pod。使用拓蹼察覺磁區排程時，除非建立可使用儲存空間的第一個 Pod，否則會延遲檔案儲存空間實例。

只有在執行 Kubernetes 1.12 版或更新版本的叢集上，才支援拓蹼察覺磁區排程。
{: note}

下列範例顯示如何建立儲存空間類別，以將檔案儲存空間實例的建立延遲到備妥要排定使用此儲存空間的第一個 Pod。若要延遲建立，您必須包括 `volumeBindingMode: WaitForFirstConsumer` 選項。如果您未包括此選項，則 `volumeBindingMode` 會自動設為 `Immediate`，而且會在建立 PVC 時建立檔案儲存空間實例。

- **耐久性檔案儲存空間範例：**
  ```
  apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
    name: ibmc-file-bronze-delayed
  parameters:
    billingType: hourly
    classVersion: "2"
    iopsPerGB: "2"
    sizeRange: '[20-12000]Gi'
    type: Endurance
  provisioner: ibm.io/ibmc-file
  reclaimPolicy: Delete
  volumeBindingMode: WaitForFirstConsumer
  ```
  {: codeblock}

- **效能檔案儲存空間範例：**
  ```
  apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
   name: ibmc-file-performance-storageclass
   labels:
     kubernetes.io/cluster-service: "true"
  provisioner: ibm.io/ibmc-file
  parameters:
   billingType: "hourly"
   classVersion: "2"
   sizeIOPSRange: |-
     "[20-39]Gi:[100-1000]"
     "[40-79]Gi:[100-2000]"
     "[80-99]Gi:[100-4000]"
     "[100-499]Gi:[100-6000]"
     "[500-999]Gi:[100-10000]"
     "[1000-1999]Gi:[100-20000]"
     "[2000-2999]Gi:[200-40000]"
     "[3000-3999]Gi:[200-48000]"
     "[4000-7999]Gi:[300-48000]"
     "[8000-9999]Gi:[500-48000]"
     "[10000-12000]Gi:[1000-48000]"
   type: "Performance"
  reclaimPolicy: Delete
  volumeBindingMode: WaitForFirstConsumer
  ```
  {: codeblock}

### 指定多區域叢集的區域
{: #file_multizone_yaml}

如果您要在特定區域中建立檔案儲存空間，則可以在自訂的儲存空間類別中指定區域及地區。
{: shortdesc}

如果您要在特定區域中[靜態佈建檔案儲存空間](#existing_file)，請使用自訂的儲存空間類別。在所有其他情況下，請[直接在 PVC 中指定區域](#add_file)。
{: note}

建立自訂的儲存空間類別時，請指定叢集和工作者節點所在的相同地區和區域。若要取得叢集的地區，請執行 `ibmcloud ks cluster-get --cluster <cluster_name_or_ID>`，並在 **Master URL** 中尋找地區字首，例如 `https://c2.eu-de.containers.cloud.ibm.com:11111` 中的 `eu-de`。若要取得工作者節點的區域，請執行 `ibmcloud ks workers --cluster <cluster_name_or_ID>`。

- **耐久性檔案儲存空間範例：**
  ```
  apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
    name: ibmc-file-silver-mycustom-storageclass
    labels:
      kubernetes.io/cluster-service: "true"
  provisioner: ibm.io/ibmc-file
  parameters:
    zone: "dal12"
    region: "us-south"
    type: "Endurance"
    iopsPerGB: "4"
    sizeRange: "[20-12000]Gi"
    reclaimPolicy: "Delete"
    classVersion: "2"
  reclaimPolicy: Delete
  volumeBindingMode: Immediate
  ```
  {: codeblock}

- **效能檔案儲存空間範例：**
  ```
  apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
   name: ibmc-file-performance-storageclass
   labels:
     kubernetes.io/cluster-service: "true"
  provisioner: ibm.io/ibmc-file
  parameters:
   zone: "dal12"
   region: "us-south"
   billingType: "hourly"
   classVersion: "2"
   sizeIOPSRange: |-
     "[20-39]Gi:[100-1000]"
     "[40-79]Gi:[100-2000]"
     "[80-99]Gi:[100-4000]"
     "[100-499]Gi:[100-6000]"
     "[500-999]Gi:[100-10000]"
     "[1000-1999]Gi:[100-20000]"
     "[2000-2999]Gi:[200-40000]"
     "[3000-3999]Gi:[200-48000]"
     "[4000-7999]Gi:[300-48000]"
     "[8000-9999]Gi:[500-48000]"
     "[10000-12000]Gi:[1000-48000]"
   type: "Performance"
  reclaimPolicy: Delete
  volumeBindingMode: Immediate
  ```
  {: codeblock}

### 變更預設 NFS 版本
{: #nfs_version_class}

下列自訂的儲存空間類別可讓您定義您要佈建的 NFS 版本。例如，若要佈建 NFS 3.0 版，請將 `<nfs_version>` 取代為 **3.0**。
{: shortdesc}

- **耐久性檔案儲存空間範例：**
  ```
  apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
    name: ibmc-file-mount
    labels:
      kubernetes.io/cluster-service: "true"
  provisioner: ibm.io/ibmc-file
  parameters:
    type: "Endurance"
    iopsPerGB: "2"
    sizeRange: "[1-12000]Gi"
    reclaimPolicy: "Delete"
    classVersion: "2"
    mountOptions: nfsvers=<nfs_version>
  ```
  {: codeblock}

- **效能檔案儲存空間範例：**
  ```
  apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
    name: ibmc-file-mount
    labels:
      kubernetes.io/cluster-service: "true"
  provisioner: ibm.io/ibmc-file
  parameters:
   type: "Performance"
   classVersion: "2"
   sizeIOPSRange: |-
     "[20-39]Gi:[100-1000]"
     "[40-79]Gi:[100-2000]"
     "[80-99]Gi:[100-4000]"
     "[100-499]Gi:[100-6000]"
     "[500-999]Gi:[100-10000]"
     "[1000-1999]Gi:[100-20000]"
     "[2000-2999]Gi:[200-40000]"
     "[3000-3999]Gi:[200-48000]"
     "[4000-7999]Gi:[300-48000]"
     "[8000-9999]Gi:[500-48000]"
     "[10000-12000]Gi:[1000-48000]"
   mountOptions: nfsvers=<nfs_version>
  ```
  {: codeblock}
