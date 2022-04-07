---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-05"

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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}



# 叢集儲存空間的疑難排解
{: #cs_troubleshoot_storage}

在您使用 {{site.data.keyword.containerlong}} 時，請考慮使用這些技術來進行叢集儲存空間的疑難排解。
{: shortdesc}

如果您有更一般性的問題，請嘗試[叢集除錯](/docs/containers?topic=containers-cs_troubleshoot)。
{: tip}


## 除錯持續性儲存空間故障
{: #debug_storage}

檢閱可用於除錯持續性儲存空間的選項，並尋找失敗的主要原因。
{: shortdesc}

1. 驗證是否使用的是最新的 {{site.data.keyword.Bluemix_notm}} 和 {{site.data.keyword.containerlong_notm}} 外掛程式版本。 
   ```
   ibmcloud update
   ```
   {: pre}
   
   ```
   ibmcloud plugin repo-plugins
   ```
   {: pre}

2. 驗證在本端機器上執行的 `kubectl` CLI 版本是否與叢集裡安裝的 Kubernetes 版本相符合。 
   1. 顯示在叢集裡和本端機器上安裝的 `kubectl` CLI 版本。
      ```
      kubectl version
      ```
      {: pre} 
   
      輸出範例：
      ```
      Client Version: version.Info{Major:"1", Minor:"13", GitVersion:"v1.13.5", GitCommit:"641856db18352033a0d96dbc99153fa3b27298e5", GitTreeState:"clean", BuildDate:"2019-03-25T15:53:57Z", GoVersion:"go1.12.1", Compiler:"gc", Platform:"darwin/amd64"}
      Server Version: version.Info{Major:"1", Minor:"13", GitVersion:"v1.13.5+IKS", GitCommit:"e15454c2216a73b59e9a059fd2def4e6712a7cf0", GitTreeState:"clean", BuildDate:"2019-04-01T10:08:07Z", GoVersion:"go1.11.5", Compiler:"gc", Platform:"linux/amd64"}
      ```
      {: screen}
    
      如果在 `GitVersion` 中看到用戶端和伺服器的版本相同，說明 CLI 版本相符。可以忽略伺服器版本中的 `+IKS` 部分。
   2. 如果本端機器上和叢集裡的 `kubectl` CLI 版本不相符，請[更新叢集](/docs/containers?topic=containers-update)或[在本端機器上安裝其他 CLI 版本](/docs/containers?topic=containers-cs_cli_install#kubectl)。 

3. 僅限區塊儲存空間、物件儲存空間和 Portworx：確保已[使用 Kubernetes 服務帳戶安裝 Helm 伺服器 Tiller](/docs/containers?topic=containers-helm#public_helm_install)。 

4. 僅限區塊儲存空間、物件儲存空間和 Portworx：確保已為外掛程式安裝最新的 Helm chart 版本。 
   
   **區塊儲存空間和物件儲存空間**： 
   
   1. 更新 Helm chart 儲存庫。
      ```
    helm repo update
    ```
      {: pre}
      
   2. 列出 `iks-charts` 儲存庫中的 Helm chart。
      ```
    helm search iks-charts
    ```
      {: pre}
      
      輸出範例：
      ```
      iks-charts/ibm-block-storage-attacher          	1.0.2        A Helm chart for installing ibmcloud block storage attach...
      iks-charts/ibm-iks-cluster-autoscaler          	1.0.5        A Helm chart for installing the IBM Cloud cluster autoscaler
      iks-charts/ibm-object-storage-plugin           	1.0.6        A Helm chart for installing ibmcloud object storage plugin  
      iks-charts/ibm-worker-recovery                 	1.10.46      IBM Autorecovery system allows automatic recovery of unhe...
      ...
      ```
      {: screen}
      
   3. 列出叢集裡已安裝的 Helm 圖表，並比較您安裝的版本與可用的版本。
      ```
      helm ls
      ```
      {: pre}
      
   4. 如果有較新的版本可用，請安裝此版本。如需指示，請參閱[更新 {{site.data.keyword.Bluemix_notm}} Block Storage 外掛程式](/docs/containers?topic=containers-block_storage#updating-the-ibm-cloud-block-storage-plug-in)和[更新 {{site.data.keyword.cos_full_notm}} 外掛程式](/docs/containers?topic=containers-object_storage#update_cos_plugin)。 
   
   **Portworx**： 
   
   1. 尋找可用的[最新 Helm chart 版本 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/portworx/helm/tree/master/charts/portworx)。 
   
   2. 列出叢集裡已安裝的 Helm 圖表，並比較您安裝的版本與可用的版本。
      ```
      helm ls
      ```
      {: pre}
   
   3. 如果有較新的版本可用，請安裝此版本。如需指示，請參閱[更新叢集裡的 Portworx](/docs/containers?topic=containers-portworx#update_portworx)。 
   
5. 驗證儲存空間驅動程式和外掛程式 Pod 的狀態是否為 **Running**。 
   1. 列出 `kube-system` 名稱空間中的 Pod。
      ```
    kubectl get pods -n kube-system
    ```
      {: pre}
      
   2. 如果 Pod 未顯示 **Running** 狀態，請取得 Pod 的詳細資料以尋找主要原因。根據 Pod 的狀態，您可能無法執行下列所有指令。
      ```
      kubectl describe pod <pod_name> -n kube-system
      ```
      {: pre}
      
      ```
      kubectl logs <pod_name> -n kube-system
      ```
      {: pre}
      
   3. 分析 `kubectl describe pod` 指令的 CLI 輸出中的 **Events** 區段以及最新日誌，以尋找錯誤的主要原因。 
   
6. 檢查 PVC 是否已順利佈建。 
   1. 檢查 PVC 的狀態。如果 PVC 顯示的狀態為 **Bound**，說明 PVC 已順利佈建。
      ```
      kubectl get pvc
      ```
      {: pre}
      
   2. 如果 PVC 的狀態顯示為 **Pending**，請擷取導致 PVC 保持擱置狀態的錯誤。
      ```
      kubectl describe pvc <pvc_name>
      ```
      {: pre}
      
   3. 檢閱 PVC 建立期間可能發生的常見錯誤。 
      - [檔案儲存空間和區塊儲存空間：PVC 保持擱置狀態](#file_pvc_pending)
      - [物件儲存空間：PVC 保持擱置狀態](#cos_pvc_pending)
   
7. 檢查裝載儲存空間實例的 Pod 是否已順利部署。 
   1. 列出叢集裡的 Pod。如果 Pod 顯示的狀態為 **Running**，則 Pod 已順利部署。
      ```
      kubectl get pods
      ```
      {: pre}
      
   2. 取得 Pod 的詳細資料，並檢查 CLI 輸出的 **Events** 區段中是否顯示有錯誤。
      ```
      kubectl describe pod <pod_name>
      ```
      {: pre}
   
   3. 擷取應用程式的日誌，並檢查是否可以看到任何錯誤訊息。
      ```
      kubectl logs <pod_name>
      ```
      {: pre}
   
   4. 檢閱將 PVC 裝載到應用程式時可能發生的常見錯誤。 
      - [檔案儲存空間：應用程式無法存取或寫入 PVC](#file_app_failures)
      - [區塊儲存空間：應用程式無法存取或寫入 PVC](#block_app_failures)
      - [物件儲存空間：使用非 root 使用者身分存取檔案失敗](#cos_nonroot_access)
      

## 檔案儲存空間和區塊儲存空間：PVC 保持擱置狀態
{: #file_pvc_pending}

{: tsSymptoms}
當您建立 PVC 且執行 `kubectl get pvc <pvc_name>` 時，PVC 會維持在 **Pending** 狀況，即使是等待了一段時間。 

{: tsCauses}
在 PVC 建立和連結期間，File Storage 和 Block Storage 外掛程式會執行許多不同的作業。每個作業都可能失敗並導致不同的錯誤訊息。

{: tsResolve}

1. 尋找 PVC 維持在 **Pending** 狀況的主要原因。 
   ```
   kubectl describe pvc <pvc_name> -n <namespace>
   ```
   {: pre}
   
2. 檢閱一般錯誤訊息說明及解決方法。
   
   <table>
   <thead>
     <th>錯誤訊息</th>
     <th>說明</th>
     <th>解決的步驟</th>
  </thead>
  <tbody>
    <tr>
      <td><code>User doesn't have permissions to create or manage Storage</code></br></br><code>Failed to find any valid softlayer credentials in configuration file</code></br></br><code>Storage with the order ID %d could not be created after retrying for %d seconds.</code></br></br><code>Unable to locate datacenter with name <datacenter_name>.</code></td>
      <td>儲存在您叢集 `storage-secret-store` Kubernetes 密碼中的 IAM API 金鑰或 IBM Cloud 基礎架構 (SoftLayer) API 金鑰，沒有佈建持續性儲存空間的所有必要許可權。</td>
      <td>請參閱 [PVC 建立因為遺漏許可權而失敗](#missing_permissions)。</td>
    </tr>
    <tr>
      <td><code>Your order will exceed the maximum number of storage volumes allowed. Please contact Sales</code></td>
      <td>每個 {{site.data.keyword.Bluemix_notm}} 帳戶都設定有最多可建立的儲存空間實例數。透過建立 PVC，建立的實例數可超過上限儲存空間實例數。</td>
      <td>若要建立 PVC，請從下列選項中進行選擇。<ul><li>移除所有未使用的 PVC。</li><li>要求 {{site.data.keyword.Bluemix_notm}} 帳戶擁有者透過[開立支援案例](/docs/get-support?topic=get-support-getting-customer-support)來增加儲存空間配額。</li></ul> </td>
    </tr>
    <tr>
      <td><code>Unable to find the exact ItemPriceIds(type|size|iops) for the specified storage</code> </br></br><code>Failed to place storage order with the storage provider</code></td>
      <td>PVC 中指定的儲存空間大小和 IOPS 不受您選擇的儲存空間類型支援，因此無法用於指定的儲存空間類別。</td>
      <td>檢閱[決定檔案儲存空間配置](/docs/containers?topic=containers-file_storage#file_predefined_storageclass)和[決定區塊儲存空間配置](/docs/containers?topic=containers-block_storage#block_predefined_storageclass)，以尋找要使用的儲存空間類別支援的儲存空間大小和 IOPS。更正大小和 IOPS，然後重建 PVC。</td>
    </tr>
    <tr>
  <td><code>Failed to find the datacenter name in configuration file</code></td>
      <td>在 PVC 中指定的資料中心不存在。</td>
  <td>執行 <code>ibmcloud ks locations</code> 以列出可用的資料中心。更正 PVC 中的資料中心，然後重建 PVC。</td>
    </tr>
    <tr>
  <td><code>Failed to place storage order with the storage provider</code></br></br><code>Storage with the order ID 12345 could not be created after retrying for xx seconds. </code></br></br><code>Failed to do subnet authorizations for the storage 12345.</code><code>Storage 12345 has ongoing active transactions and could not be completed after retrying for xx seconds.</code></td>
  <td>儲存空間大小、IOPS 和儲存空間類別可能與您選擇的儲存空間類別不相容，或者 {{site.data.keyword.Bluemix_notm}} 基礎架構 API 端點目前無法使用。</td>
  <td>檢閱[決定檔案儲存空間配置](/docs/containers?topic=containers-file_storage#file_predefined_storageclass)和[決定區塊儲存空間配置](/docs/containers?topic=containers-block_storage#block_predefined_storageclass)，以尋找要使用的儲存空間類別和儲存空間類型支援的儲存空間大小和 IOPS。然後，刪除 PVC 並重建 PVC。</td>
  </tr>
  <tr>
  <td><code>Failed to find the storage with storage id 12345. </code></td>
  <td>您希望使用 Kubernetes 靜態佈建來為現有儲存空間實例建立 PVC，但找不到指定的儲存空間實例。</td>
  <td>遵循指示在叢集裡佈建現有[檔案儲存空間](/docs/containers?topic=containers-file_storage#existing_file)或[區塊儲存空間](/docs/containers?topic=containers-block_storage#existing_block)，並確保擷取到現有儲存空間實例的正確資訊。然後，刪除 PVC 並重建 PVC。</td>
  </tr>
  <tr>
  <td><code>Storage type not provided, expected storage type is `Endurance` or `Performance`. </code></td>
  <td>您建立了自訂儲存空間類別，但指定的儲存空間類型不受支援。</td>
  <td>更新自訂儲存空間類別，以指定 `Endurance` 或 `Performance` 作為儲存空間類型。若要尋找自訂儲存空間類別的範例，請參閱[檔案儲存空間](/docs/containers?topic=containers-file_storage#file_custom_storageclass)和[區塊儲存空間](/docs/containers?topic=containers-block_storage#block_custom_storageclass)的範例自訂儲存空間類別。</td>
  </tr>
  </tbody>
  </table>
  
## 檔案儲存空間：應用程式無法存取或寫入 PVC
{: #file_app_failures}

當您將 PVC 裝載至 Pod 時，可能在存取或寫入 PVC 時遭遇錯誤。
{: shortdesc}

1. 列出叢集裡的 Pod 並檢閱 Pod 的狀態。 
   ```
   kubectl get pods
   ```
   {: pre}
   
2. 尋找應用程式為何無法存取或寫入 PVC 的主要原因。
   ```
   kubectl describe pod <pod_name>
   ```
   {: pre}
   
   ```
   kubectl logs <pod_name>
   ```
   {: pre}

3. 檢閱當您將 PVC 裝載至 Pod 時可能發生的一般錯誤。 
   <table>
   <thead>
     <th>症狀或錯誤訊息</th>
     <th>說明</th>
     <th>解決的步驟</th>
  </thead>
  <tbody>
    <tr>
      <td>Pod 卡在 <strong>ContainerCreating</strong> 狀態。</br></br><code>MountVolume.SetUp failed for volume ... read-only file system</code></td>
      <td>{{site.data.keyword.Bluemix_notm}} 基礎架構後端遭遇了網路問題。為了保護資料和避免資料毀損，{{site.data.keyword.Bluemix_notm}} 會自動中斷檔案儲存空間伺服器的連接，以阻止對 NFS 檔案共用執行撰寫作業。</td>
      <td>請參閱[檔案儲存空間：工作者節點的檔案系統變更為唯讀](#readonly_nodes)</td>
      </tr>
      <tr>
  <td><code>write-permission</code> </br></br><code>do not have required permission</code></br></br><code>cannot create directory '/bitnami/mariadb/data': Permission denied </code></td>
  <td>在部署中，指定了非 root 使用者來擁有 NFS 檔案儲存空間裝載路徑。依預設，非 root 使用者對於支援 NFS 的儲存空間的磁區裝載路徑沒有寫入權。</td>
  <td>請參閱[檔案儲存空間：非 root 使用者擁有 NFS 檔案儲存空間裝載路徑時，應用程式發生故障](#nonroot)</td>
  </tr>
  <tr>
  <td>在指定非 root 使用者來擁有 NFS 檔案儲存空間裝載路徑或使用指定的非 root 使用者 ID 部署 Helm chart 後，使用者無法寫入裝載的儲存空間。</td>
  <td>部署或 Helm chart 配置為 Pod 的 <code>fsGroup</code>（群組 ID）和 <code>runAsUser</code>（使用者 ID）指定了安全環境定義。</td>
  <td>請參閱[檔案儲存空間：新增持續性儲存空間的非 root 使用者存取權失敗](#cs_storage_nonroot)</td>
  </tr>
</tbody>
</table>

### 檔案儲存空間：工作者節點的檔案系統變更為唯讀
{: #readonly_nodes}

{: tsSymptoms}
{: #stuck_creating_state}
您可能會看到下列其中一個症狀：
- 當您執行 `kubectl get pods -o wide` 時，您會看到在同一個工作者節點上執行的多個位置停留在 `ContainerCreating` 狀況中。
- 當您執行 `kubectl describe` 指令時，會在 **Events** 區段中看到下列錯誤：`MountVolume.SetUp failed for volume ... read-only file system`。

{: tsCauses}
工作者節點上的檔案系統是唯讀的。

{: tsResolve}
1.  備份可能儲存在工作者節點或容器上的任何資料。
2.  若要對現有工作者節點進行短期修正，請重新載入工作者節點。<pre class="pre"><code>ibmcloud ks worker-reload --cluster &lt;cluster_name&gt; --worker &lt;worker_ID&gt;</code></pre>

如需長期修正，請[更新工作者節點儲存區的機型](/docs/containers?topic=containers-update#machine_type)。

<br />



### 檔案儲存空間：當非 root 使用者擁有 NFS 檔案儲存空間裝載路徑時，應用程式會失敗
{: #nonroot}

{: tsSymptoms}
[新增 NFS 儲存空間](/docs/containers?topic=containers-file_storage#file_app_volume_mount)至您的部署之後，您的容器部署失敗。當您擷取容器的日誌時，可能會看到如下錯誤。Pod 失敗，並停留在重新載入的循環中。

```
write-permission
```
{: screen}

```
do not have required permission
```
{: screen}

```
cannot create directory '/bitnami/mariadb/data': Permission denied
```
{: screen}

{: tsCauses}
依預設，非 root 使用者對於支援 NFS 的儲存空間的磁區裝載路徑沒有寫入權。部分一般應用程式映像檔（例如 Jenkins 及 Nexus3）會在 Dockerfile 中指定擁有裝載路徑的非 root 使用者。當您從這個 Dockerfile 建立容器時，會由於非 root 使用者對裝載路徑的權限不足，而造成建立容器失敗。若要授與寫入權，您可以修改 Dockerfile，以在變更裝載路徑許可權之前，暫時將非 root 使用者新增至 root 使用者群組，或使用 init 容器。

如果您使用 Helm 圖表來部署映像檔，則請編輯 Helm 部署以使用 init 容器。
{:tip}



{: tsResolve}
當您在部署中包括 [init 容器 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/) 時，可以授與 Dockerfile 中所指定的非 root 使用者對於容器內的磁區裝載路徑的寫入權。init 容器會在您的應用程式容器啟動之前啟動。init 容器會在容器內建立磁區裝載路徑，將裝載路徑變更為由正確的（非 root）使用者所擁有，然後關閉。然後，會使用必須寫入至裝載路徑的非 root 使用者來啟動您的應用程式容器。因為路徑已由非 root 使用者擁有，所以會成功寫入至裝載路徑。如果您不想要使用 init 容器，則可以修改 Dockerfile，以新增非 root 使用者對 NFS 檔案儲存空間的存取權。


開始之前：[登入您的帳戶。適用的話，請將適當的資源群組設為目標。設定叢集的環境定義。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  開啟應用程式的 Dockerfile，然後取得您要授與磁區裝載路徑寫入權的使用者的使用者 ID (UID) 及群組 ID (GID)。在 Jenkins Dockerfile 的範例中，其資訊如下：
    - UID：`1000`
    - GID：`1000`

    **範例**：

    ```
    FROM openjdk:8-jdk

    

    RUN apt-get update &&apt-get install -y git curl &&rm -rf /var/lib/apt/lists/*

    

    ARG user=jenkins
    ARG group=jenkins
    ARG uid=1000
    ARG gid=1000
    ARG http_port=8080
    ARG agent_port=50000

    

    ENV JENKINS_HOME /var/jenkins_home
    ENV JENKINS_SLAVE_AGENT_PORT ${agent_port}
    ...
    ```
    {:screen}

2.  藉由建立持續性磁區要求 (PVC)，將持續性儲存空間新增至應用程式。這個範例使用 `ibmc-file-bronze` 儲存空間類別。若要檢閱可用的儲存空間類別，請執行 `kubectl get storageclasses`。

    ```
    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
      name: mypvc
      annotations:
        volume.beta.kubernetes.io/storage-class: "ibmc-file-bronze"
    spec:
      accessModes:
        - ReadWriteMany
      resources:
        requests:
          storage: 20Gi
    ```
    {: codeblock}

3.  建立 PVC。

    ```
    kubectl apply -f mypvc.yaml
    ```
    {: pre}

4.  在您的部署 `.yaml` 檔案中，新增 init 容器。包括您先前擷取的 UID 及 GID。

    ```
    initContainers:
    - name: initcontainer # Or replace the name
      image: alpine:latest
      command: ["/bin/sh", "-c"]
      args:
        - chown <UID>:<GID> /mount; # Replace UID and GID with values from the Dockerfile
      volumeMounts:
      - name: volume # Or you can replace with any name
        mountPath: /mount # Must match the mount path in the args line
    ```
    {: codeblock}

    Jenkins 部署的**範例**：

    ```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: my_pod
    spec:
      replicas: 1
      selector:
        matchLabels:
          app: jenkins      
      template:
        metadata:
          labels:
            app: jenkins
        spec:
          containers:
          - name: jenkins
            image: jenkins
            volumeMounts:
            - mountPath: /var/jenkins_home
              name: volume
          volumes:
          - name: volume
            persistentVolumeClaim:
              claimName: mypvc
          initContainers:
          - name: permissionsfix
            image: alpine:latest
            command: ["/bin/sh", "-c"]
            args:
              - chown 1000:1000 /mount;
            volumeMounts:
            - name: volume
              mountPath: /mount
    ```
    {: codeblock}

5.  建立 Pod，並將 PVC 裝載至 Pod。

    ```
    kubectl apply -f my_pod.yaml
    ```
    {: pre}

6. 驗證磁區已順利裝載至 Pod。請記下 Pod 名稱及 **Containers/Mounts** 路徑。

    ```
    kubectl describe pod <my_pod>
    ```
    {: pre}

    **輸出範例**：

    ```
    Name:		    mypod-123456789
    Namespace:	default
    ...
    Init Containers:
    ...
    Mounts:
      /mount from volume (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-cp9f0 (ro)
    ...
    Containers:
      jenkins:
        Container ID:
        Image:		jenkins
        Image ID:
        Port:		  <none>
        State:		Waiting
          Reason:		PodInitializing
        Ready:		False
        Restart Count:	0
        Environment:	<none>
        Mounts:
          /var/jenkins_home from volume (rw)
          /var/run/secrets/kubernetes.io/serviceaccount from default-token-cp9f0 (ro)
    ...
    Volumes:
      myvol:
        Type: PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
        ClaimName: mypvc
        ReadOnly: false

    

    ```
    {: screen}

7.  使用您先前記下的 Pod 名稱來登入 Pod。

    ```
    kubectl exec -it <my_pod-123456789> /bin/bash
    ```
    {: pre}

8. 驗證容器裝載路徑的許可權。在此範例中，裝載路徑為 `/var/jenkins_home`。

    ```
    ls -ln /var/jenkins_home
    ```
    {: pre}

    **輸出範例**：

    ```
    jenkins@mypod-123456789:/$ ls -ln /var/jenkins_home
    total 12
    -rw-r--r-- 1 1000 1000  102 Mar  9 19:58 copy_reference_file.log
    drwxr-xr-x 2 1000 1000 4096 Mar  9 19:58 init.groovy.d
    drwxr-xr-x 9 1000 1000 4096 Mar  9 20:16 war
    ```
    {: screen}

    此輸出顯示 Dockerfile 中的 GID 及 UID（在此範例中是 `1000` 及 `1000`）擁有容器內的裝載路徑。

<br />


### 檔案儲存空間：新增非 root 使用者對持續性儲存空間的存取權失敗
{: #cs_storage_nonroot}

{: tsSymptoms}
在您[新增非 root 使用者對持續性儲存空間的存取權](#nonroot)之後，或在指定非 root 使用者 ID 的情況下部署 Helm 圖表之後，使用者無法寫入已裝載的儲存空間。

{: tsCauses}
部署或 Helm 圖表配置為 Pod 的 `fsGroup`（群組 ID）及 `runAsUser`（使用者 ID）指定[安全環境定義](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/)。目前，{{site.data.keyword.containerlong_notm}} 不支援 `fsGroup` 規格，僅支援 `runAsUser` 設為 `0`（root 許可權）。

{: tsResolve}
請從映像檔、部署或 Helm 圖表配置檔中，移除 `fsGroup` 及 `runAsUser` 的配置的 `securityContext` 欄位，然後重新部署。如果您需要變更 `nobody` 的裝載路徑的所有權，請[新增非 root 使用者存取權](#nonroot)。在您新增[非 root `initContainer`](#nonroot) 之後，請在容器層次設定 `runAsUser`，而非 Pod 層次。

<br />




## 區塊儲存空間：應用程式無法存取或寫入 PVC
{: #block_app_failures}

當您將 PVC 裝載至 Pod 時，可能在存取或寫入 PVC 時遭遇錯誤。
{: shortdesc}

1. 列出叢集裡的 Pod 並檢閱 Pod 的狀態。 
   ```
   kubectl get pods
   ```
   {: pre}
   
2. 尋找應用程式為何無法存取或寫入 PVC 的主要原因。
   ```
   kubectl describe pod <pod_name>
   ```
   {: pre}
   
   ```
   kubectl logs <pod_name>
   ```
   {: pre}

3. 檢閱當您將 PVC 裝載至 Pod 時可能發生的一般錯誤。 
   <table>
   <thead>
     <th>症狀或錯誤訊息</th>
     <th>說明</th>
     <th>解決的步驟</th>
  </thead>
  <tbody>
    <tr>
      <td>Pod 卡在 <strong>ContainerCreating</strong> 或 <strong>CrashLoopBackOff</strong> 狀態。</br></br><code>MountVolume.SetUp failed for volume ... read-only.</code></td>
      <td>{{site.data.keyword.Bluemix_notm}} 基礎架構後端遭遇了網路問題。為了保護資料和避免資料毀損，{{site.data.keyword.Bluemix_notm}} 自動中斷了區塊儲存空間伺服器的連接，以阻止對區塊儲存空間實例執行撰寫作業。</td>
      <td>請參閱[區塊儲存空間：區塊儲存空間變更為唯讀](#readonly_block)</td>
      </tr>
      <tr>
  <td><code>failed to mount the volume as "ext4", it already contains xfs. Mount error: mount failed: exit status 32</code> </td>
        <td>您要裝載設定為使用 <code>XFS</code> 檔案系統的現有區塊儲存空間實例。建立 PV 和相符的 PVC 時，指定了 <code>ext4</code> 或未指定檔案系統。在 PV 中指定的檔案系統必須與區塊儲存空間實例中設定的檔案系統相同。</td>
  <td>請參閱[區塊儲存空間：由於檔案系統不正確，將現有區塊儲存空間裝載到 Pod 失敗](#block_filesystem)</td>
  </tr>
</tbody>
</table>

### 區塊儲存空間：區塊儲存空間變更為唯讀
{: #readonly_block}

{: tsSymptoms}
您可能會看到下列狀況：
- 當您執行 `kubectl get pods -o wide` 時，您會看到同一個工作者節點上有多個 Pod 停留在 `ContainerCreating` 或 `CrashLoopBackOff` 狀態中。這些 Pod 全部都使用相同的區塊儲存空間實例。
- 當您執行 `kubectl describe pod` 指令時，您會在**事件**區段中看到下列錯誤：`MountVolume.SetUp failed for volume ... read-only`。

{: tsCauses}
如果在 Pod 寫入磁區時發生網路錯誤，IBM Cloud 基礎架構 (SoftLayer) 會將磁區變更為唯讀模式，以保護磁區上的資料免於毀損。使用此磁區的 Pod 無法繼續寫入磁區，並發生失敗。

{: tsResolve}
1. 檢查叢集裡已安裝的 {{site.data.keyword.Bluemix_notm}} Block Storage 外掛程式版本。
   ```
   helm ls
   ```
   {: pre}

2. 驗證您使用[最新版本的 {{site.data.keyword.Bluemix_notm}} Block Storage 外掛程式](https://cloud.ibm.com/kubernetes/solutions/helm-charts/ibm/ibmcloud-block-storage-plugin)。如果不是，請[更新外掛程式](/docs/containers?topic=containers-block_storage#updating-the-ibm-cloud-block-storage-plug-in)。
3. 如果您的 Pod 使用 Kubernetes 部署，請移除失敗的 Pod 並讓 Kubernetes 重建它，以重新啟動該 Pod。如果您未使用部署，請執行下列指令以擷取用來建立 Pod 的 YAML 檔案：`kubectl get pod <pod_name> -o yaml >pod.yaml`。然後，刪除並手動重建 Pod。
    ```
    kubectl delete pod <pod_name>
    ```
    {: pre}

4. 檢查重建 Pod 是否已解決問題。如果沒有，請重新載入工作者節點。
   1. 尋找 Pod 執行所在的工作者節點，並記下指派給工作者節點的專用 IP 位址。
      ```
      kubectl describe pod <pod_name> | grep Node
      ```
      {: pre}

      輸出範例：
      ```
      Node:               10.75.XX.XXX/10.75.XX.XXX
      Node-Selectors:  <none>
      ```
      {: screen}

   2. 透過使用前一個步驟的專用 IP 位址，來擷取工作者節點的 **ID**。
      ```
      ibmcloud ks workers --cluster <cluster_name_or_ID>
      ```
      {: pre}

   3. 溫和地[重新載入工作者節點](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)。


<br />


### 區塊儲存空間：將現有區塊儲存空間裝載至 Pod 失敗，因為檔案系統錯誤
{: #block_filesystem}

{: tsSymptoms}
當您執行 `kubectl describe pod <pod_name>` 時，會看到下列錯誤：
```
failed to mount the volume as "ext4", it already contains xfs. Mount error: mount failed: exit status 32
```
{: screen}

{: tsCauses}
您的現有區塊儲存裝置已設定 `XFS` 檔案系統。若要將此裝置裝載至您的 Pod，您已[建立 PV](/docs/containers?topic=containers-block_storage#existing_block)，其將 `ext4` 指定為檔案系統或 `spec/flexVolume/fsType` 區段中未指定檔案系統。如果未定義任何檔案系統，則 PV 預設為 `ext4`。PV 已順利建立並鏈結至現有區塊儲存空間實例。不過，當您嘗試使用相符的 PVC 將 PV 裝載到叢集時，會無法裝載磁區。您無法將具有 `ext4` 檔案系統的 `XFS` 區塊儲存空間實例裝載到 Pod。

{: tsResolve}
將現有 PV 中的檔案系統從 `ext4` 更新至 `XFS`。

1. 列出叢集裡的現有 PV，並記下用於現有區塊儲存空間實例的 PV 名稱。
   ```
   kubectl get pv
   ```
   {: pre}

2. 將 PV YAML 儲存在您的本端機器上。
   ```
   kubectl get pv <pv_name> -o yaml > <filepath/xfs_pv.yaml>
   ```
   {: pre}

3. 開啟 YAML 檔案，並將 `fsType` 從 `ext4` 變更為 `xfs`。
4. 取代叢集裡的 PV。
   ```
   kubectl replace --force -f <filepath/xfs_pv.yaml>
   ```
   {: pre}

5. 登入已裝載 PV 的 Pod。
   ```
   kubectl exec -it <pod_name> sh
   ```
   {: pre}

6. 驗證檔案系統已變更為 `XFS`。
   ```
   df -Th
   ```
   {: pre}

   輸出範例：
   ```
   Filesystem Type Size Used Avail Use% Mounted on /dev/mapper/3600a098031234546d5d4c9876654e35 xfs 20G 33M 20G 1% /myvolumepath
   ```
   {: screen}

<br />



## 物件儲存空間：安裝 {{site.data.keyword.cos_full_notm}} `ibmc` Helm 外掛程式失敗
{: #cos_helm_fails}

{: tsSymptoms}
在安裝 {{site.data.keyword.cos_full_notm}} `ibmc` Helm 外掛程式時，安裝失敗，發生下列其中一個錯誤：
```
Error: symlink /Users/ibm/ibmcloud-object-storage-plugin/helm-ibmc /Users/ibm/.helm/plugins/helm-ibmc: file exists
```
{: screen}

```
Error: fork/exec /home/iksadmin/.helm/plugins/helm-ibmc/ibmc.sh: permission denied
```
{: screen}

{: tsCauses}
安裝 `ibmc` Helm 外掛程式時，會建立從 `./helm/plugins/helm-ibmc` 目錄到本端系統上 `ibmc` Helm 外掛程式所在目錄（通常在 `./ibmcloud-object-storage-plugin/helm-ibmc` 中）的符號鏈結。當您從本端系統移除 `ibmc` Helm 外掛程式時，或將 `ibmc` Helm 外掛程式目錄移至不同的位置時，不會移除符號鏈結。

如果您看到 `permission denied` 錯誤，表示您對 `ibmc.sh` bash 檔案沒有必要的 `read`、`write` 和 `execute` 許可權，因此無法執行 `ibmc` Helm 外掛程式指令。 

{: tsResolve}

**對於符號鏈接錯誤**： 

1. 移除 {{site.data.keyword.cos_full_notm}} Helm 外掛程式。
   ```
   rm -rf ~/.helm/plugins/helm-ibmc
   ```
   {: pre}

2. 遵循[文件](/docs/containers?topic=containers-object_storage#install_cos)以重新安裝 `ibmc` Helm 外掛程式和 {{site.data.keyword.cos_full_notm}} 外掛程式。

**對於許可權錯誤**： 

1. 變更 `ibmc` 外掛程式的許可權。 
   ```
   chmod 755 ~/.helm/plugins/helm-ibmc/ibmc.sh
   ```
   {: pre}
   
2. 試用 `ibm` Helm 外掛程式。 
   ```
      helm ibmc --help
      ```
   {: pre}
   
3. [繼續安裝 {{site.data.keyword.cos_full_notm}} 外掛程式](/docs/containers?topic=containers-object_storage#install_cos)。 


<br />


## 物件儲存空間：PVC 保持擱置狀態
{: #cos_pvc_pending}

{: tsSymptoms}
當您建立 PVC 且執行 `kubectl get pvc <pvc_name>` 時，PVC 會維持在 **Pending** 狀況，即使是等待了一段時間。 

{: tsCauses}
在 PVC 建立和連結期間，{{site.data.keyword.cos_full_notm}} 外掛程式會執行許多不同的作業。每個作業都可能失敗並導致不同的錯誤訊息。

{: tsResolve}

1. 尋找 PVC 維持在 **Pending** 狀況的主要原因。 
   ```
   kubectl describe pvc <pvc_name> -n <namespace>
   ```
   {: pre}
   
2. 檢閱一般錯誤訊息說明及解決方法。
   
   <table>
   <thead>
     <th>錯誤訊息</th>
     <th>說明</th>
     <th>解決的步驟</th>
  </thead>
  <tbody>
    <tr>
      <td>`User doesn't have permissions to create or manage Storage`</td>
      <td>儲存在您叢集 `storage-secret-store` Kubernetes 密碼中的 IAM API 金鑰或 IBM Cloud 基礎架構 (SoftLayer) API 金鑰，沒有佈建持續性儲存空間的所有必要許可權。</td>
      <td>請參閱 [PVC 建立因為遺漏許可權而失敗](#missing_permissions)。</td>
    </tr>
    <tr>
      <td>`cannot get credentials: cannot get secret <secret_name>: secrets "<secret_name>" not found`</td>
      <td>用於保存 {{site.data.keyword.cos_full_notm}} 服務認證的 Kubernetes 密碼不在 PVC 或 Pod 所在的名稱空間中。</td>
      <td>請參閱[由於找不到 Kubernetes 密碼，PVC 或 Pod 建立失敗](#cos_secret_access_fails)。</td>
    </tr>
    <tr>
      <td>`cannot get credentials: Wrong Secret Type.Provided secret of type XXXX.Expected type ibm/ibmc-s3fs`</td>
      <td>為 {{site.data.keyword.cos_full_notm}} 服務實例建立的 Kubernetes 密碼不包含 `type: ibm/ibmc-s3fs`。</td>
      <td>編輯用於保存 {{site.data.keyword.cos_full_notm}} 認證的 Kubernetes 密碼，以新增或變更 `type` 為 `ibm/ibmc-s3fs`。</td>
    </tr>
    <tr>
      <td>`Bad value for ibm.io/object-store-endpoint XXXX: scheme is missing. Must be of the form http://<hostname> or https://<hostname>`</br> </br> `Bad value for ibm.io/iam-endpoint XXXX: scheme is missing. Must be of the form http://<hostname< or https://<hostname>`</td>
      <td>s3fs API 或 IAM API 端點的格式錯誤，或者無法根據叢集位置擷取到 s3fs API 端點。</td>
      <td>請參閱[由於錯誤的 s3fs API 端點，PVC 建立失敗](#cos_api_endpoint_failure)。</td>
    </tr>
    <tr>
      <td>`object-path cannot be set when auto-create is enabled`</td>
      <td>您使用 `ibm.io/object-path` 註釋指定了儲存區中您要裝載至 PVC 的現有子目錄。如果設定了子目錄，則必須停用儲存區自動建立特性。</td>
      <td>在 PVC 中，設定 `ibm.io/auto-create-bucket: "false"`，並在 `ibm.io/bucket` 中提供現有儲存區的名稱。</td>
    </tr>
    <tr>
    <td>`bucket auto-create must be enabled when bucket auto-delete is enabled`</td>
    <td>在 PVC 中，您設定 `ibm.io/auto-delete-bucket: true` 以便在移除 PVC 時自動刪除資料、儲存區及 PV。這個選項需要將 `ibm.io/auto-create-bucket` 設為 <strong>true</strong>，且同時將 `ibm.io/bucket` 設為 `""`。</td>
    <td>在 PVC 中，設定 `ibm.io/auto-create-bucket: true` 及 `ibm.io/bucket: ""`，以便自動以 `tmp-s3fs-xxxx` 格式的名稱建立儲存區。</td>
    </tr>
    <tr>
    <td>`bucket cannot be set when auto-delete is enabled`</td>
    <td>在 PVC 中，您設定 `ibm.io/auto-delete-bucket: true` 以便在移除 PVC 時自動刪除資料、儲存區及 PV。這個選項需要將 `ibm.io/auto-create-bucket` 設為 <strong>true</strong>，且同時將 `ibm.io/bucket` 設為 `""`。</td>
    <td>在 PVC 中，設定 `ibm.io/auto-create-bucket: true` 及 `ibm.io/bucket: ""`，以便自動以 `tmp-s3fs-xxxx` 格式的名稱建立儲存區。</td>
    </tr>
    <tr>
    <td>`cannot create bucket using API key without service-instance-id`</td>
    <td>如果要使用 IAM API 金鑰來存取 {{site.data.keyword.cos_full_notm}} 服務實例，您必須在 Kubernetes 密碼中儲存 API 金鑰和 {{site.data.keyword.cos_full_notm}} 服務實例 ID。</td>
    <td>請參閱[為 Object Storage 服務認證建立密碼](/docs/containers?topic=containers-object_storage#create_cos_secret)。</td>
    </tr>
    <tr>
      <td>`object-path "<subdirectory_name>"  not found inside bucket <bucket_name>`</td>
      <td>您使用 `ibm.io/object-path` 註釋指定了儲存區中您要裝載至 PVC 的現有子目錄。在指定的儲存區中找不到此子目錄。</td>
      <td>驗證在 `ibm.io/bucket` 中指定的儲存區中是否存在 `ibm.io/object-path` 中指定的子目錄。</td>
    </tr>
        <tr>
          <td>`BucketAlreadyExists: The requested bucket name is not available. The bucket namespace is shared by all users of the system. Please select a different name and try again.`</td>
          <td>您設定了 `ibm.io/auto-create-bucket: true` 並同時指定了儲存區名稱，或者指定了 {{site.data.keyword.cos_full_notm}} 中已存在的儲存區名稱。儲存區名稱在 {{site.data.keyword.cos_full_notm}} 中的所有服務實例和地區中必須是唯一的。</td>
          <td>確保設定 `ibm.io/auto-create-bucket: false`，並確保提供在 {{site.data.keyword.cos_full_notm}} 中唯一的儲存區名稱。如果要使用 {{site.data.keyword.cos_full_notm}} 外掛程式自動建立儲存區名稱，請設定 `ibm.io/auto-create-bucket: true` 和 `ibm.io/bucket: ""`。這將使用格式為 `tmp-s3fs-xxxx` 的唯一名稱來建立儲存區。</td>
        </tr>
        <tr>
          <td>`cannot access bucket <bucket_name>: NotFound: Not Found`</td>
          <td>您嘗試存取的是未建立的儲存區，或者指定的儲存空間類別和 s3fs API 端點與建立儲存區時使用的儲存空間類別和 s3fs API 端點不相符。</td>
          <td>請參閱[無法存取現有儲存區](#cos_access_bucket_fails)。</td>
        </tr>
        <tr>
          <td>`Put https://s3-api.dal-us-geo.objectstorage.service.networklayer.com/<bucket_name>: net/http: invalid header field value "AWS4-HMAC-SHA256 Credential=1234a12a123a123a1a123aa1a123a123\n\n/20190412/us-standard/s3/aws4_request, SignedHeaders=host;x-amz-content-sha256;x-amz-date, Signature=12aa1abc123456aabb12aas12aa123456sb123456abc" for key Authorization`</td>
          <td>Kubernetes 密碼中的值未正確編碼為 Base64。</td>
          <td>檢閱 Kubernetes 密碼中的值，並將每個值編碼為 Base64。您還可以使用 [`kubectl create secret`](/docs/containers?topic=containers-object_storage#create_cos_secret) 指令來建立新密碼，並讓 Kubernetes 自動將值編碼為 Base64。</td>
        </tr>
        <tr>
          <td>`cannot access bucket <bucket_name>: Forbidden: Forbidden`</td>
          <td>您指定了 `ibm.io/auto-create-bucket: false`，並且嘗試存取並未建立的儲存區，或者 {{site.data.keyword.cos_full_notm}} HMAC 認證的服務存取金鑰或存取金鑰 ID 不正確。</td>
          <td>您無法存取未由您建立的儲存區。請透過設定 `ibm.io/auto-create-bucket: true` 和 `ibm.io/bucket: ""` 來建立新的儲存區。如果您是儲存區的擁有者，請參閱[PVC 建立失敗，因為認證錯誤或拒絕存取](#cred_failure)來檢查認證。</td>
        </tr>
        <tr>
          <td>`cannot create bucket <bucket_name>: AccessDenied: Access Denied`</td>
          <td>您指定了 `ibm.io/auto-create-bucket: true` 以在 {{site.data.keyword.cos_full_notm}} 中自動建立儲存區，但為 Kubernetes 密碼中提供的認證指派的是 IAM **讀者**服務存取角色。這個角色不容許在 {{site.data.keyword.cos_full_notm}} 建立儲存區。</td>
          <td>請參閱 [PVC 建立失敗，因為認證錯誤或拒絕存取](#cred_failure)。</td>
        </tr>
        <tr>
          <td>`cannot create bucket <bucket_name>: AccessForbidden: Access Forbidden`</td>
          <td>您指定了 `ibm.io/auto-create-bucket: true`，並在 `ibm.io/bucket` 中提供了現有儲存區的名稱。此外，還為 Kubernetes 密碼中提供的認證指派了 IAM **讀者**服務存取角色。這個角色不容許在 {{site.data.keyword.cos_full_notm}} 建立儲存區。</td>
          <td>若要使用現有儲存區，請設定 `ibm.io/auto-create-bucket: false`，並在 `ibm.io/bucket` 中提供現有儲存區的名稱。若要使用現有 Kubernetes 密碼自動建立儲存區，請設定 `ibm.io/bucket: ""`，並遵循[由於錯誤的認證或存取遭拒，PVC 建立失敗](#cred_failure)，以驗證 Kubernetes 密碼中的認證。</td>
        </tr>
        <tr>
          <td>`cannot create bucket <bucket_name>: SignatureDoesNotMatch: The request signature we calculated does not match the signature you provided. Check your AWS Secret Access Key and signing method. For more information, see REST Authentication and SOAP Authentication for details`</td>
          <td>Kubernetes 密碼中提供的 HMAC 認證的 {{site.data.keyword.cos_full_notm}} 秘密存取金鑰不正確。</td>
          <td>請參閱 [PVC 建立失敗，因為認證錯誤或拒絕存取](#cred_failure)。</td>
        </tr>
        <tr>
          <td>`cannot create bucket <bucket_name>: InvalidAccessKeyId: The AWS Access Key ID you provided does not exist in our records`</td>
          <td>Kubernetes 密碼中提供的 HMAC 認證的 {{site.data.keyword.cos_full_notm}} 存取金鑰 ID 或秘密存取金鑰不正確。</td>
          <td>請參閱 [PVC 建立失敗，因為認證錯誤或拒絕存取](#cred_failure)。</td>
        </tr>
        <tr>
          <td>`cannot create bucket <bucket_name>: CredentialsEndpointError: failed to load credentials` </br> </br> `cannot access bucket <bucket_name>: CredentialsEndpointError: failed to load credentials`</td>
          <td>IAM 認證的 {{site.data.keyword.cos_full_notm}} API 金鑰和 {{site.data.keyword.cos_full_notm}} 服務實例的 GUID 不正確。</td>
          <td>請參閱 [PVC 建立失敗，因為認證錯誤或拒絕存取](#cred_failure)。</td>
        </tr>
  </tbody>
    </table>
    

### 物件儲存空間：因為找不到 Kubernetes 密碼，所以無法建立 PVC 或 Pod
{: #cos_secret_access_fails}

{: tsSymptoms}
當您建立 PVC 或部署可裝載 PVC 的 Pod 時，建立或部署會失敗。

- PVC 建立失敗的錯誤訊息範例：
  ```
  cannot get credentials: cannot get secret tsecret-key: secrets "secret-key" not found
  ```
  {: screen}

- Pod 建立失敗的錯誤訊息範例：
  ```
  persistentvolumeclaim "pvc-3" not found (repeated 3 times)
  ```
  {: screen}

{: tsCauses}
儲存 {{site.data.keyword.cos_full_notm}} 服務認證的 Kubernetes 的密碼、PVC 及 Pod 不是都在相同的 Kubernetes 名稱空間中。將密碼部署至與 PVC 或 Pod 不同的名稱空間時，無法存取密碼。

{: tsResolve}
此作業需要所有名稱空間的[**撰寫者**或**管理員** {{site.data.keyword.Bluemix_notm}} IAM 服務角色](/docs/containers?topic=containers-users#platform)。

1. 列出叢集裡的密碼，並檢閱已建立 {{site.data.keyword.cos_full_notm}} 服務實例之 Kubernetes 密碼的 Kubernetes 名稱空間。密碼必須將 `ibm/ibmc-s3fs` 顯示為**類型**。
   ```
   kubectl get secrets --all-namespaces
   ```
   {: pre}

2. 檢查 PVC 及 Pod 的 YAML 配置檔，驗證您已使用相同的名稱空間。如果您要將 Pod 部署至非密碼所在的名稱空間中，則請在該名稱空間中[建立另一個密碼](/docs/containers?topic=containers-object_storage#create_cos_secret)。

3. 在適當的名稱空間中建立 PVC 或部署 Pod。

<br />


### 物件儲存空間：PVC 建立失敗，因為認證錯誤或拒絕存取
{: #cred_failure}

{: tsSymptoms}
當您建立 PVC 時，會看到與下列其中一則類似的錯誤訊息：

```
SignatureDoesNotMatch: The request signature we calculated does not match the signature you provided. Check your AWS Secret Access Key and signing method. For more information, see REST Authentication and SOAP Authentication for details.
```
{: screen}

```
AccessDenied: Access Denied status code: 403 
```
{: screen}

```
CredentialsEndpointError: failed to load credentials
```
{: screen}

```
InvalidAccessKeyId: The AWS Access Key ID you provided does not exist in our records`
```
{: screen}

```
cannot access bucket <bucket_name>: Forbidden: Forbidden
```
{: screen}


{: tsCauses}
您用來存取服務實例的 {{site.data.keyword.cos_full_notm}} 服務認證可能錯誤，或只容許對儲存區進行讀取。

{: tsResolve}
1. 在「服務詳細資料」頁面的導覽中，按一下**服務認證**。
2. 尋找您的認證，然後按一下**檢視認證**。
3. 在 **iam_role_crn** 區段中，驗證您具有 `Writer` 或 `Manager` 角色。如果沒有正確的角色，則必須使用正確的許可權建立新的 {{site.data.keyword.cos_full_notm}} 服務認證。 
4. 如果角色正確，請驗證在 Kubernetes 密碼中是否已使用正確的 **access_key_id** 和 **secret_access_key**。 
5. [使用更新的 **access_key_id** 和 **secret_access_key** 建立新密碼](/docs/containers?topic=containers-object_storage#create_cos_secret)。 


<br />


### 物件儲存空間：由於錯誤的 s3fs 或 IAM API 端點，PVC 建立失敗
{: #cos_api_endpoint_failure}

{: tsSymptoms}
建立 PVC 時，PVC 保持擱置狀態。執行 `kubectl describe pvc <pvc_name>` 指令後，您將看到下列其中一條錯誤訊息： 

```
Bad value for ibm.io/object-store-endpoint XXXX: scheme is missing. Must be of the form http://<hostname> or https://<hostname>
```
{: screen}

```
Bad value for ibm.io/iam-endpoint XXXX: scheme is missing. Must be of the form http://<hostname> or https://<hostname>
```
{: screen}

{: tsCauses}
要使用的儲存區的 s3fs API 端點的格式可能錯誤，或者叢集部署在 {{site.data.keyword.containerlong_notm}} 中支援，但 {{site.data.keyword.cos_full_notm}} 外掛程式尚不支援的位置。 

{: tsResolve}
1. 檢查在 {{site.data.keyword.cos_full_notm}} 外掛程式安裝期間 `ibmc` Helm 外掛程式自動指派給儲存空間類別的 s3fs API 端點。根據叢集所在的位置，來部署端點。  
   ```
   kubectl get sc ibmc-s3fs-standard-regional -o yaml | grep object-store-endpoint
   ```
   {: pre}

   如果指令傳回 `ibm.io/object-store-endpoint: NA`，表示叢集部署在 {{site.data.keyword.containerlong_notm}} 中支援，但 {{site.data.keyword.cos_full_notm}} 外掛程式尚不支援的位置。若要將該位置新增到 {{site.data.keyword.containerlong_notm}}，請在公用 Slack 中張貼問題，或開立 {{site.data.keyword.Bluemix_notm}} 支援案例。如需相關資訊，請參閱[取得協助及支援](/docs/containers?topic=containers-cs_troubleshoot#ts_getting_help)。 
   
2. 如果在 PVC 中手動已新增具有 `ibm.io/endpoint` 註釋的 s3fs API 端點或具有 `ibm.io/iam-endpoint` 註釋的 IAM API 端點，請確保已新增格式為 `https://<s3fs_api_endpoint>` 和 `https://<iam_api_endpoint>` 的端點。註釋會改寫 {{site.data.keyword.cos_full_notm}} 儲存空間類別中的 `ibmc` 外掛程式自動設定的 API 端點。 
   ```
   kubectl describe pvc <pvc_name>
   ```
   {: pre}
   
<br />


### 物件儲存空間：無法存取現有儲存區
{: #cos_access_bucket_fails}

{: tsSymptoms}
當您建立 PVC 時，無法存取 {{site.data.keyword.cos_full_notm}} 中的儲存區。您會看到與下列內容類似的錯誤訊息：

```
Failed to provision volume with StorageClass "ibmc-s3fs-standard-regional": pvc:1b2345678b69175abc98y873e2:cannot access bucket <bucket_name>: NotFound: Not Found
```
{: screen}

{: tsCauses}
您可能使用錯誤的儲存空間類別來存取現有儲存區，或嘗試存取您未建立的儲存區。您無法存取未由您建立的儲存區。 

{: tsResolve}
1. 從 [{{site.data.keyword.Bluemix_notm}} 儀表板 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/) 中，選取 {{site.data.keyword.cos_full_notm}} 服務實例。
2. 選取**儲存區**。
3. 檢閱現有儲存區的**類別**及**位置**資訊。
4. 選擇適當的[儲存空間類別](/docs/containers?topic=containers-object_storage#cos_storageclass_reference)。
5. 確保設定 `ibm.io/auto-create-bucket: false`，並確保提供現有儲存區的正確名稱。 

<br />


## 物件儲存空間：使用非 root 使用者身分存取檔案失敗
{: #cos_nonroot_access}

{: tsSymptoms}
您已使用主控台或 REST API，將檔案上傳至 {{site.data.keyword.cos_full_notm}} 服務實例。當您嘗試在應用程式部署中以使用 `runAsUser` 所定義的非 root 使用者身分存取這些檔案時，會拒絕對檔案的存取。

{: tsCauses}
在 Linux 中，檔案或目錄有 3 個存取群組：`Owner`、`Group` 及 `Other`。當您使用主控台或 REST API 將檔案上傳至 {{site.data.keyword.cos_full_notm}} 時，會移除 `Owner`、`Group` 及 `Other` 的許可權。每個檔案的許可權都如下所示：

```
d--------- 1 root root 0 Jan 1 1970 <file_name>
```
{: screen}

當您使用 {{site.data.keyword.cos_full_notm}} 外掛程式上傳檔案時，會保留檔案的許可權而且不會變更。

{: tsResolve}
若要使用非 root 使用者身分存取檔案，非 root 使用者必須具有該檔案的讀取及寫入權。在 Pod 部署期間變更檔案的許可權，需要寫入作業。{{site.data.keyword.cos_full_notm}} 不是針對寫入工作負載所設計。在 Pod 部署期間更新許可權可能會讓您的 Pod 無法進入 `Running` 狀態。

若要解決此問題，在將 PVC 裝載至應用程式 Pod 之前，請建立另一個 Pod 以設定非 root 使用者的正確許可權。

1. 檢查儲存區中您檔案的許可權。
   1. 建立 `test-permission` Pod 的配置檔，並將檔案命名為 `test-permission.yaml`。
      ```
      apiVersion: v1
      kind: Pod
      metadata:
        name: test-permission
      spec:
        containers:
        - name: test-permission
          image: nginx
          volumeMounts:
          - name: cos-vol
            mountPath: /test
        volumes:
        - name: cos-vol
          persistentVolumeClaim:
            claimName: <pvc_name>
      ```
      {: codeblock}

   2. 建立 `test-permission` Pod。
      ```
   kubectl apply -f test-permission.yaml
   ```
      {: pre}

   3. 登入 Pod。
      ```
      kubectl exec test-permission -it bash
      ```
      {: pre}

   4. 導覽至您的裝載路徑，並列出檔案的許可權。
      ```
      cd test && ls -al
      ```
      {: pre}

      輸出範例：
      ```
      d--------- 1 root root 0 Jan 1 1970 <file_name>
      ```
      {: screen}

2. 刪除 Pod。
   ```
   kubectl delete pod test-permission
   ```
   {: pre}

3. 建立您用來更正檔案許可權之 Pod 的配置檔，並將它命名為 `fix-permission.yaml`。
   ```
   apiVersion: v1
   kind: Pod
   metadata:
     name: fix-permission
     namespace: <namespace>
   spec:
     containers:
     - name: fix-permission
       image: busybox
       command: ['sh', '-c']
       args: ['chown -R <nonroot_userID> <mount_path>/*; find <mount_path>/ -type d -print -exec chmod u=+rwx,g=+rx {} \;']
       volumeMounts:
       - mountPath: "<mount_path>"
         name: cos-volume
     volumes:
     - name: cos-volume
       persistentVolumeClaim:
         claimName: <pvc_name>
    ```
    {: codeblock}

3. 建立 `fix-permission` Pod。
   ```
   kubectl apply -f fix-permission.yaml
   ```
   {: pre}

4. 等待 Pod 進入 `Completed` 狀態。  
   ```
   kubectl get pod fix-permission
   ```
   {: pre}

5. 刪除 `fix-permission` Pod。
   ```
   kubectl delete pod fix-permission
   ```
   {: pre}

5. 重建您稍早用來檢查許可權的 `test-permission` Pod。
   ```
   kubectl apply -f test-permission.yaml
   ```
   {: pre}

5. 驗證已更新檔案的許可權。
   1. 登入 Pod。
      ```
      kubectl exec test-permission -it bash
      ```
      {: pre}

   2. 導覽至您的裝載路徑，並列出檔案的許可權。
      ```
      cd test && ls -al
      ```
      {: pre}

      輸出範例：
      ```
      -rwxrwx--- 1 <nonroot_userID> root 6193 Aug 21 17:06 <file_name>
      ```
      {: screen}

6. 刪除 `test-permission` Pod。
   ```
   kubectl delete pod test-permission
   ```
   {: pre}

7. 使用非 root 使用者身分，將 PVC 裝載至應用程式。

   將非 root 使用者定義為 `runAsUser`，而不需要同時在部署 YAML 中設定 `fsGroup`。設定 `fsGroup`，即會在部署 Pod 時觸發 {{site.data.keyword.cos_full_notm}} 外掛程式更新儲存區中所有檔案的群組許可權。更新許可權是一項寫入作業，而且可能會讓您的 Pod 無法進入 `Running` 狀態。
   {: important}

在您於 {{site.data.keyword.cos_full_notm}} 服務實例中設定正確的檔案許可權之後，請不要使用主控台或 REST API 來上傳檔案。請使用 {{site.data.keyword.cos_full_notm}} 外掛程式，將檔案新增至服務實例。
{: tip}

<br />


   
## 由於遺漏許可權，PVC 建立失敗
{: #missing_permissions}

{: tsSymptoms}
建立 PVC 時，PVC 保持擱置狀態。執行 `kubectl describe pvc <pvc_name>` 時，您會看到類似於下列內容的錯誤訊息： 

```
User doesn't have permissions to create or manage Storage
```
{: screen}

{: tsCauses}
儲存在您叢集 `storage-secret-store` Kubernetes 密碼中的 IAM API 金鑰或 IBM Cloud 基礎架構 (SoftLayer) API 金鑰，沒有佈建持續性儲存空間的所有必要許可權。 

{: tsResolve}
1. 擷取儲存在叢集的 `storage-secret-store` Kubernetes 密碼中的 IAM 金鑰或 IBM Cloud 基礎架構 (SoftLayer) API 金鑰，並驗證是否已使用正確的 API 金鑰。 
   ```
   kubectl get secret storage-secret-store -n kube-system -o yaml | grep slclient.toml: | awk '{print $2}' | base64 --decode
   ```
   {: pre}
   
   輸出範例： 
   ```
   [Bluemix]
   iam_url = "https://iam.bluemix.net"
   iam_client_id = "bx"
   iam_client_secret = "bx"
   iam_api_key = "<iam_api_key>"
   refresh_token = ""
   pay_tier = "paid"
   containers_api_route = "https://us-south.containers.bluemix.net"

   [Softlayer]
   encryption = true
   softlayer_username = ""
   softlayer_api_key = ""
   softlayer_endpoint_url = "https://api.softlayer.com/rest/v3"
   softlayer_iam_endpoint_url = "https://api.softlayer.com/mobile/v3"
   softlayer_datacenter = "dal10"
   ```
   {: screen}
   
   IAM API 金鑰會列在 CLI 輸出的 `Bluemix.iam_api_key` 區段中。如果此時 `Softlayer.softlayer_api_key` 是空的，則會使用 IAM API 金鑰來確定您的基礎架構許可權。IAM API 金鑰由執行第一個動作的使用者自動設定，該使用者需要在資源群組和地區中具有 IAM **管理者**平台角色。如果在 `Softlayer.softlayer_api_key` 中設定了不同的 API 金鑰，則此金鑰優先於 IAM API 金鑰。叢集管理執行 `ibmcloud ks credentials-set` 指令時，將設定 `Softlayer.softlayer_api_key`。 
   
2. 如果要變更認證，請更新使用的 API 金鑰。 
    1.  若要更新 IAM API 金鑰，請使用 `ibmcloud ks api-key-reset` [指令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_api_key_reset)。若要更新 IBM Cloud 基礎架構 (SoftLayer) 金鑰，請使用 `ibmcloud ks credential-set` [指令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_set)。
    2. 等待 `storage-secret-store` Kubernetes 密碼更新，此過程大約需要 10 到 15 分鐘，然後驗證金鑰是否已更新。
       ```
   kubectl get secret storage-secret-store -n kube-system -o yaml | grep slclient.toml: | awk '{print $2}' | base64 --decode
   ```
       {: pre}
   
3. 如果 API 金鑰正確，請驗證該金鑰是否具有佈建持續性儲存空間的正確許可權。
   1. 聯絡帳戶擁有者以驗證 API 金鑰的許可權。 
   2. 作為帳戶擁有者，從 {{site.data.keyword.Bluemix_notm}} 主控台的導覽中，選取**管理** > **存取權 (IAM)**。
   3. 選取**使用者**，並尋找要使用其 API 金鑰的使用者。 
   4. 從「動作」功能表中，選取**管理使用者詳細資料**。 
   5. 移至**標準基礎架構**標籤。 
   6. 展開**帳戶**種類，並驗證是否已指派**新增/升級儲存空間 (StorageLayer)** 許可權。 
   7. 展開**服務**種類，並驗證是否已指派**儲存空間管理**許可權。 
4. 移除失敗的 PVC。 
   ```
   kubectl delete pvc <pvc_name>
   ```
   {: pre}
   
5. 重建 PVC。 
   ```
   kubectl apply -f pvc.yaml
   ```
   {: pre}


## 取得協助及支援
{: #storage_getting_help}

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

