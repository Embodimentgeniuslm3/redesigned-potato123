---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: kubernetes, iks, helm, without tiller, private cluster tiller, integrations, helm chart

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



# 使用 Helm 圖表新增服務
{: #helm}

您可以使用 Helm 圖表，將複雜的 Kubernetes 應用程式新增至叢集。
{: shortdesc}

**何謂 Helm 及其使用方式？** </br>
[Helm ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://helm.sh) 是 Kubernetes 套件管理程式，可使用 Helm 圖表來定義、安裝及升級叢集裡的複雜 Kubernetes 應用程式。Helm 圖表會包裝規格，以針對建置應用程式的 Kubernetes 資源產生 YAML 檔案。這些 Kubernetes 資源會在叢集裡自動套用，並由 Helm 指派版本。您也可以使用 Helm 來指定並包裝自己的應用程式，並讓 Helm 針對 Kubernetes 資源產生 YAML 檔案。  

若要在叢集裡使用 Helm，您必須在本端機器上安裝 Helm CLI，並在您要使用 Helm 的每個叢集裡安裝 Helm 伺服器 Tiller。

**{{site.data.keyword.containerlong_notm}} 中支援的 Helm 圖表為何？** </br>
如需可用 Helm 圖表的概觀，請參閱 [Helm 圖表型錄 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/kubernetes/solutions/helm-charts)。此型錄中列出的 Helm chart 分組如下：

- **iks-charts**：針對 {{site.data.keyword.containerlong_notm}} 核准的 Helm 圖表。此儲存庫的名稱已從 `ibm` 變更為 `iks-charts`。
- **ibm-charts**：針對 {{site.data.keyword.containerlong_notm}} 及 {{site.data.keyword.Bluemix_notm}} Private 叢集核准的 Helm 圖表。
- **kubernetes**：Kubernetes 社群所提供並由社群控管視為 `stable` 的 Helm 圖表。這些圖表未經驗證，無法在 {{site.data.keyword.containerlong_notm}} 或 {{site.data.keyword.Bluemix_notm}} Private 叢集裡運作。
- **kubernetes-incubator**：Kubernetes 社群所提供並由社群控管視為 `incubator` 的 Helm 圖表。這些圖表未經驗證，無法在 {{site.data.keyword.containerlong_notm}} 或 {{site.data.keyword.Bluemix_notm}} Private 叢集裡運作。

**iks-charts** 及 **ibm-charts** 儲存庫中的 Helm 圖表已完全整合至 {{site.data.keyword.Bluemix_notm}} 支援組織。如果您有使用這些 Helm 圖表的問題，可以使用其中一個 {{site.data.keyword.containerlong_notm}} 支援頻道。如需相關資訊，請參閱[取得協助及支援](/docs/containers?topic=containers-cs_troubleshoot_clusters#clusters_getting_help)。

**使用 Helm 的必要條件為何，以及是否可以在專用叢集裡使用 Helm？** </br>
若要部署 Helm 圖表，您必須在本端機器上安裝 Helm CLI，並在叢集裡安裝 Helm 伺服器 Tiller。Tiller 的映像檔儲存在公用 Google Container Registry 中。若要在 Tiller 安裝期間存取映像檔，您的叢集必須容許與公用 Google Container Registry 的公用網路連線功能。啟用了公用服務端點的叢集可以自動存取該映像檔。使用自訂防火牆進行受保護的的專用叢集或僅已啟用了專用服務端點的叢集不容許存取 Tiller 映像檔。相反地，您可以[將映像檔取回至本端機器，並將映像檔推送至 {{site.data.keyword.registryshort_notm}} 中的名稱空間](#private_local_tiller)，或[在不使用 Tiller 的情況下安裝 Helm 圖表](#private_install_without_tiller)。



## 在具有公用存取權的叢集裡設定 Helm
{: #public_helm_install}

如果叢集已啟用了公用服務端點，則可以使用 Google Container Registry 中的公用映像檔來安裝 Helm 伺服器 Tiller。
{: shortdesc}

開始之前：
- [登入您的帳戶。適用的話，請將適當的資源群組設為目標。設定叢集的環境定義。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
- 若要使用 `kube-system` 名稱空間中的 Kubernetes 服務帳戶及叢集角色連結來安裝 Tiller，請確定您具有 [`cluster-admin` 角色](/docs/containers?topic=containers-users#access_policies)。

若要在叢集裡安裝具有公用存取權的 Helm，請執行下列動作：

1. 在本端機器上安裝 <a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">Helm CLI <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>。

2. 檢查是否已在叢集裡使用 Kubernetes 服務帳戶安裝 Tiller。
   ```
   kubectl get serviceaccount --all-namespaces | grep tiller
   ```
   {: pre}

   已安裝 Tiller 時的輸出範例：
   ```
   kube-system      tiller                               1         189d
   ```
   {: screen}

   輸出範例包括 Kubernetes 名稱空間及 Tiller 服務帳戶的名稱。如果在您的叢集裡，未使用服務帳戶安裝 Tiller，則不會傳回任何 CLI 輸出。

3. **重要事項**：若要維護叢集安全，請在叢集裡使用服務帳戶及叢集角色連結來設定 Tiller。
   - **如果已使用服務帳戶安裝 Tiller，請執行下列動作：**
     1. 建立用於 Tiller 服務帳戶的叢集角色連結。將 `<namespace>` 取代為叢集裡已安裝 Tiller 的名稱空間。
        ```
        kubectl create clusterrolebinding tiller --clusterrole=cluster-admin --serviceaccount=<namespace>:tiller -n <namespace>
        ```
        {: pre}

     2. 更新 Tiller。將 `<tiller_service_account_name>` 取代為您在前一個步驟中擷取之 Tiller 的 Kubernetes 服務帳戶名稱。
        ```
        helm init --upgrade --service-account <tiller_service_account_name>
        ```
        {: pre}

     3. 驗證 `tiller - deploy` Pod 在叢集裡的 **Status** 為 `Running`。
        ```
        kubectl get pods -n <namespace> -l app=helm
        ```
        {: pre}

        輸出範例：

        ```
        NAME                            READY     STATUS    RESTARTS   AGE
        tiller-deploy-352283156-nzbcm   1/1       Running   0          2m
        ```
        {: screen}

   - **如果未使用服務帳戶來安裝 Tiller，請執行下列動作：**
     1. 在叢集的 `kube-system` 名稱空間中，建立適用於 Tiller 的 Kubernetes 服務帳戶及叢集角色連結。
        ```
        kubectl create serviceaccount tiller -n kube-system
        ```
        {: pre}

        ```
        kubectl create clusterrolebinding tiller --clusterrole=cluster-admin --serviceaccount=kube-system:tiller -n kube-system
        ```
        {: pre}

     2. 驗證已建立 Tiller 服務帳戶。
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

     3. 使用您所建立的服務帳戶，來起始設定 Helm CLI 並在叢集裡安裝 Tiller。
        ```
    helm init --service-account tiller
    ```
        {: pre}

     4. 驗證 `tiller - deploy` Pod 在叢集裡的 **Status** 為 `Running`。
        ```
        kubectl get pods -n kube-system -l app=helm
        ```
        {: pre}

              輸出範例：
      ```
                NAME                            READY     STATUS    RESTARTS   AGE
        tiller-deploy-352283156-nzbcm   1/1       Running   0          2m
        ```
        {: screen}

4. 將 {{site.data.keyword.Bluemix_notm}} Helm 儲存庫新增至 Helm 實例。
   ```
   helm repo add iks-charts https://icr.io/helm/iks-charts
   ```
   {: pre}

   ```
   helm repo add ibm-charts https://icr.io/helm/ibm-charts
   ```
   {: pre}

5. 更新儲存庫，以擷取所有 Helm 圖表的最新版本。
   ```
   helm repo update
   ```
   {: pre}

6. 列出 {{site.data.keyword.Bluemix_notm}} 儲存庫中目前可用的 Helm 圖表。
   ```
   helm search iks-charts
   ```
   {: pre}

   ```
   helm search ibm-charts
   ```
   {: pre}

7. 識別您要安裝的 Helm 圖表，並遵循 Helm 圖表 `README` 中的指示，以在叢集裡安裝 Helm 圖表。


## 專用叢集：將 Tiller 映像檔推送至 IBM Cloud Container Registry 中的名稱空間
{: #private_local_tiller}

您可以將 Tiller 映像檔取回至本端機器、將映像檔推送至 {{site.data.keyword.registryshort_notm}} 中的名稱空間，以及使用 {{site.data.keyword.registryshort_notm}} 中的映像檔在專用叢集裡安裝 Tiller。
{: shortdesc}

如果您要在不使用 Tiller 的情況下安裝 Helm 圖表，請參閱[專用叢集：在不使用 Tiller 的情況下安裝 Helm 圖表](#private_install_without_tiller)。
{: tip}

開始之前：
- 在本端機器上安裝 Docker。如果您安裝了 [{{site.data.keyword.Bluemix_notm}} CLI](/docs/cli?topic=cloud-cli-getting-started)，則已安裝 Docker。
- [安裝 {{site.data.keyword.registryshort_notm}} CLI 外掛程式並設定名稱空間](/docs/services/Registry?topic=registry-getting-started#gs_registry_cli_install)。
- 若要使用 `kube-system` 名稱空間中的 Kubernetes 服務帳戶及叢集角色連結來安裝 Tiller，請確定您具有 [`cluster-admin` 角色](/docs/containers?topic=containers-users#access_policies)。

若要使用 {{site.data.keyword.registryshort_notm}} 來安裝 Tiller，請執行下列動作：

1. 在本端機器上安裝 <a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">Helm CLI <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>。
2. 使用您設定的 {{site.data.keyword.Bluemix_notm}} 基礎架構 VPN 通道，來連接至專用叢集。
3. **重要事項**：若要維護叢集安全，請在 `kube-system` 名稱空間中建立 Tiller 的服務帳戶，以及針對 `tiller-deploy` Pod 建立 Kubernetes RBAC 叢集角色連結，方法是套用 [{{site.data.keyword.Bluemix_notm}} `kube-samples` 儲存庫](https://github.com/IBM-Cloud/kube-samples/blob/master/rbac/serviceaccount-tiller.yaml)中的下列 YAML 檔案。
    1. [取得 Kubernetes 服務帳戶和叢集角色連結 YAML 檔案 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/rbac/serviceaccount-tiller.yaml)。

    2. 在叢集裡建立 Kubernetes 資源。
       ```
       kubectl apply -f service-account.yaml
       ```
       {: pre}

       ```
       kubectl apply -f cluster-role-binding.yaml
       ```
       {: pre}

4. [尋找 Tiller 版本 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://console.cloud.google.com/gcr/images/kubernetes-helm/GLOBAL/tiller?gcrImageListsize=30)，您要在叢集裡安裝該版本。如果您不需要特定版本，請使用最新版本。

5. 將 Tiller 映像檔從公用 Google Container Registry 取回至本端機器。
   ```
   docker pull gcr.io/kubernetes-helm/tiller:v2.11.0
   ```
   {: pre}

   輸出範例：
   ```
   docker pull gcr.io/kubernetes-helm/tiller:v2.13.0
   v2.13.0: Pulling from kubernetes-helm/tiller
   48ecbb6b270e: Pull complete
   d3fa0712c71b: Pull complete
   bf13a43b92e9: Pull complete
   b3f98be98675: Pull complete
   Digest: sha256:c4bf03bb67b3ae07e38e834f29dc7fd43f472f67cad3c078279ff1bbbb463aa6
   Status: Downloaded newer image for gcr.io/kubernetes-helm/tiller:v2.13.0
   ```
   {: screen}

6. [將 Tiller 映像檔推送至 {{site.data.keyword.registryshort_notm}} 中的名稱空間](/docs/services/Registry?topic=registry-getting-started#gs_registry_images_pushing)。

7. 若要從叢集內存取 {{site.data.keyword.registryshort_notm}} 中的映像檔，請[將映像檔取回密碼從 default 名稱空間複製到 `kube-system` 名稱空間](/docs/containers?topic=containers-images#copy_imagePullSecret)。

8. 使用您在 {{site.data.keyword.registryshort_notm}} 的名稱空間中儲存的映像檔，在專用叢集裡安裝 Tiller。
   ```
   helm init --tiller-image <region>.icr.io/<mynamespace>/<myimage>:<tag> --service-account tiller
   ```
   {: pre}

9. 將 {{site.data.keyword.Bluemix_notm}} Helm 儲存庫新增至 Helm 實例。
   ```
   helm repo add iks-charts https://icr.io/helm/iks-charts
   ```
   {: pre}

   ```
   helm repo add ibm-charts https://icr.io/helm/ibm-charts
   ```
   {: pre}

10. 更新儲存庫，以擷取所有 Helm 圖表的最新版本。
    ```
   helm repo update
   ```
    {: pre}

11. 列出 {{site.data.keyword.Bluemix_notm}} 儲存庫中目前可用的 Helm 圖表。
    ```
    helm search iks-charts
    ```
    {: pre}

    ```
    helm search ibm-charts
    ```
    {: pre}

12. 識別您要安裝的 Helm 圖表，並遵循 Helm 圖表 `README` 中的指示，以在叢集裡安裝 Helm 圖表。


## 專用叢集：在不使用 Tiller 的情況下安裝 Helm 圖表
{: #private_install_without_tiller}

如果您不要在專用叢集裡安裝 Tiller，則可以使用 `kubectl` 指令，手動建立 Helm 圖表 YAML 檔案，並套用這些檔案。
{: shortdesc}

此範例中的步驟顯示如何從專用叢集的 {{site.data.keyword.Bluemix_notm}} Helm 圖表儲存庫中安裝 Helm 圖表。如果您要安裝的 Helm 圖表未儲存在其中一個 {{site.data.keyword.Bluemix_notm}} Helm 圖表儲存庫中，則必須遵循此主題中的指示，來建立 Helm 圖表的 YAML 檔案。此外，您還必須從公用 Container Registry 下載 Helm 圖表映像檔，並將它推送至 {{site.data.keyword.registryshort_notm}} 中的名稱空間，然後更新 `values.yaml` 檔案以使用 {{site.data.keyword.registryshort_notm}} 中的映像檔。
{: note}

1. 在本端機器上安裝 <a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">Helm CLI <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>。
2. 使用您設定的 {{site.data.keyword.Bluemix_notm}} 基礎架構 VPN 通道，來連接至專用叢集。
3. 將 {{site.data.keyword.Bluemix_notm}} Helm 儲存庫新增至 Helm 實例。
   ```
   helm repo add iks-charts https://icr.io/helm/iks-charts
   ```
   {: pre}

   ```
   helm repo add ibm-charts https://icr.io/helm/ibm-charts
   ```
   {: pre}

4. 更新儲存庫，以擷取所有 Helm 圖表的最新版本。
   ```
   helm repo update
   ```
   {: pre}

5. 列出 {{site.data.keyword.Bluemix_notm}} 儲存庫中目前可用的 Helm 圖表。
   ```
   helm search iks-charts
   ```
   {: pre}

   ```
   helm search ibm-charts
   ```
   {: pre}

6. 識別您要安裝的 Helm 圖表、將 Helm 圖表下載至本端機器，以及解壓縮 Helm 圖表的檔案。下列範例顯示如何下載 Cluster Autoscaler 1.0.3 版的 Helm 圖表，並解壓縮 `cluster-autoscaler` 目錄中的檔案。
   ```
   helm fetch iks-charts/ibm-iks-cluster-autoscaler --untar --untardir ./cluster-autoscaler --version 1.0.3
   ```
   {: pre}

7. 導覽至您要在其中解壓縮 Helm 圖表檔案的目錄。
   ```
   cd cluster-autoscaler
   ```
   {: pre}

8. 針對您使用 Helm 圖表中的檔案所產生的 YAML 檔案，建立 `output` 目錄。
   ```
   mkdir output
   ```
   {: pre}

9. 開啟 `values.yaml` 檔案，然後進行 Helm 圖表安裝指示所需的全部變更。
   ```
   nano ibm-iks-cluster-autoscaler/values.yaml
   ```
   {: pre}

10. 使用本端 Helm 安裝，以建立 Helm 圖表的所有 Kubernetes YAML 檔案。YAML 檔案會儲存在您先前建立的 `output` 目錄中。
    ```
    helm template --values ./ibm-iks-cluster-autoscaler/values.yaml --output-dir ./output ./ibm-iks-cluster-autoscaler
    ```
    {: pre}

          輸出範例：
      ```
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-configmap.yaml
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-service-account-roles.yaml
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-service.yaml
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-deployment.yaml
    ```
    {: screen}

11. 將所有 YAML 檔案部署至專用叢集。
    ```
    kubectl apply --recursive --filename ./output
    ```
   {: pre}

12. 選用項目：從 `output` 目錄移除所有 YAML 檔案。
    ```
    kubectl delete --recursive --filename ./output
    ```
    {: pre}

## 相關的 Helm 鏈結
{: #helm_links}

請檢閱下列鏈結，以尋找其他 Helm 資訊。
{: shortdesc}

* 在 [Helm 圖表型錄 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/kubernetes/solutions/helm-charts) 中，檢視您可以在 {{site.data.keyword.containerlong_notm}} 中使用的可用 Helm 圖表。
* 進一步瞭解 <a href="https://docs.helm.sh/helm/" target="_blank">Helm 文件 <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a> 中有關可用來設定及管理 Helm 圖表的 Helm 指令。
* 進一步瞭解如何[使用 Kubernetes Helm 圖表來加快部署 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://developer.ibm.com/recipes/tutorials/increase-deployment-velocity-with-kubernetes-helm-charts/)。
