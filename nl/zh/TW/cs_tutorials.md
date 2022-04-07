---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

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



# 指導教學：建立 Kubernetes 叢集
{: #cs_cluster_tutorial}

您可以使用本指導教學，在 {{site.data.keyword.containerlong}} 中部署及管理 Kubernetes 叢集。學習如何在叢集裡，自動進行容器化應用程式的部署、作業、調整及監視。
{:shortdesc}

在本指導教學系列中，您可以看到虛構公關公司如何使用 Kubernetes 功能在 {{site.data.keyword.Bluemix_notm}} 中部署容器化應用程式。利用 {{site.data.keyword.toneanalyzerfull}}，公關公司會分析其新聞稿並收到回饋意見。


## 目標
{: #tutorials_objectives}

在這第一個指導教學中，您擔任公關公司的網路管理者。您可以配置自訂 Kubernetes 叢集，用來在 {{site.data.keyword.containerlong_notm}} 中部署及測試 Hello World 版本的應用程式。
{:shortdesc}

-   建立一個叢集，其包含一個具有 1 個工作者節點的工作者節點儲存區。
-   安裝 CLI 來執行 [Kubernetes 指令 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubectl.docs.kubernetes.io/)，以及管理 {{site.data.keyword.registrylong_notm}} 中的 Docker 映像檔。
-   在 {{site.data.keyword.registrylong_notm}} 中建立專用映像檔儲存庫，以儲存您的映像檔。
-   將 {{site.data.keyword.toneanalyzershort}} 服務新增至叢集，讓叢集裡的任何應用程式都可以使用該服務。


## 所需時間
{: #tutorials_time}

40 分鐘


## 適用對象
{: #tutorials_audience}

本指導教學的適用對象是第一次建立 Kubernetes 叢集的軟體開發人員及網路管理者。
{: shortdesc}

## 必要條件
{: #tutorials_prereqs}

-  請查看您需要採取以[準備建立叢集](/docs/containers?topic=containers-clusters#cluster_prepare)的步驟。
-  確定您具有下列存取原則：
    - {{site.data.keyword.containerlong_notm}} 的[**管理者** {{site.data.keyword.Bluemix_notm}} IAM 平台角色](/docs/containers?topic=containers-users#platform)
    - {{site.data.keyword.registrylong_notm}} 的[**管理者** {{site.data.keyword.Bluemix_notm}} IAM 平台角色](/docs/containers?topic=containers-users#platform)
    - {{site.data.keyword.containerlong_notm}} 的[**撰寫者**或**管理員** {{site.data.keyword.Bluemix_notm}} IAM 服務角色](/docs/containers?topic=containers-users#platform)


## 課程 1：建立叢集並設定 CLI
{: #cs_cluster_tutorial_lesson1}

在 {{site.data.keyword.Bluemix_notm}} 主控台中建立 Kubernetes 叢集，並安裝必要的 CLI。
{: shortdesc}

**建立叢集**

因為需要幾分鐘的時間進行佈建，所以請先建立叢集，再安裝 CLI。

1.  [在 {{site.data.keyword.Bluemix_notm}} 主控台 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/kubernetes/catalog/cluster/create) 中，建立一個免費或標準叢集，其包含一個具有 1 個工作者節點的工作者節點儲存區。

    您也可以[在 CLI 中建立叢集](/docs/containers?topic=containers-clusters#clusters_cli_steps)。
    {: tip}

佈建叢集時，請安裝下列用來管理叢集的 CLI：
-   {{site.data.keyword.Bluemix_notm}} CLI 
-   {{site.data.keyword.containerlong_notm}} 外掛程式
-   Kubernetes CLI
-   {{site.data.keyword.registryshort_notm}} 外掛程式

如果您要改用 {{site.data.keyword.Bluemix_notm}} 主控台，則在建立叢集之後，您可以在 [Kubernetes Terminal](/docs/containers?topic=containers-cs_cli_install#cli_web) 中從 Web 瀏覽器直接執行 CLI 指令。
{: tip}

</br>
**安裝 CLI 及其必備項目**

1. 安裝 [{{site.data.keyword.Bluemix_notm}} CLI ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](/docs/cli?topic=cloud-cli-getting-started)。此安裝包括：
  - 基本 {{site.data.keyword.Bluemix_notm}} CLI。使用 {{site.data.keyword.Bluemix_notm}} CLI 來執行指令的字首是 `ibmcloud`。
  - {{site.data.keyword.containerlong_notm}} 外掛程式。使用 {{site.data.keyword.Bluemix_notm}} CLI 來執行指令的字首是 `ibmcloud ks`。
  - {{site.data.keyword.registryshort_notm}} 外掛程式。使用此外掛程式，在 {{site.data.keyword.registryshort_notm}} 中設定及管理專用映像檔儲存庫。執行登錄指令的字首是 `ibmcloud cr`。

2. 登入 {{site.data.keyword.Bluemix_notm}} CLI。系統提示時，請輸入您的 {{site.data.keyword.Bluemix_notm}} 認證。
  ```
  ibmcloud login
  ```
  {: pre}

  如果您具有聯合 ID，請使用 `--sso` 旗標來登入。請輸入使用者名稱，並使用 CLI 輸出中提供的 URL 來擷取一次性密碼。
    {: tip}

3. 遵循提示來選取帳戶。

5. 驗證已正確安裝外掛程式。
  ```
  ibmcloud plugin list
  ```
  {: pre}

  {{site.data.keyword.containerlong_notm}} 外掛程式會在結果中顯示為 **container-service**，而 {{site.data.keyword.registryshort_notm}} 外掛程式會在結果中顯示為 **container-registry**。

6. 若要將應用程式部署至叢集，請[安裝 Kubernetes CLI ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/tasks/tools/install-kubectl/)。使用 Kubernetes CLI 來執行指令的字首是 `kubectl`。

  1. 下載與您計劃使用的 Kubernetes 叢集 `major.minor` 版本相符的 Kubernetes CLI `major.minor` 版本。現行 {{site.data.keyword.containerlong_notm}} 預設 Kubernetes 版本為 1.13.6。
    - **OS X**：[https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/darwin/amd64/kubectl ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/darwin/amd64/kubectl)
    - **Linux**：[https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/linux/amd64/kubectl ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/linux/amd64/kubectl)
    - **Windows**：[https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/windows/amd64/kubectl.exe ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/windows/amd64/kubectl.exe)

  2. 如果您是使用 OS X 或 Linux，請完成下列步驟。

    1. 將執行檔移至 `/usr/local/bin` 目錄。
      ```
      mv /filepath/kubectl /usr/local/bin/kubectl
      ```
      {: pre}

    2. 確定 `/usr/local/bin` 列在 `PATH` 系統變數中。`PATH` 變數包含作業系統可在其中尋找執行檔的所有目錄。`PATH` 變數中所列的目錄用於不同的用途。`/usr/local/bin` 是用來儲存軟體的執行檔，該軟體不是作業系統的一部分，並且已由系統管理者手動安裝。
      ```
      echo $PATH
      ```
      {: pre}
      CLI 輸出範例：
      ```
      /usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
      ```
      {: screen}

    3. 使檔案成為可執行檔。
      ```
      chmod +x /usr/local/bin/kubectl
      ```
      {: pre}

做得好！您已順利安裝用於下列課程及指導教學的 CLI。接下來，請設定叢集環境並新增 {{site.data.keyword.toneanalyzershort}} 服務。


## 課程 2：設定您的專用登錄
{: #cs_cluster_tutorial_lesson2}

在 {{site.data.keyword.registryshort_notm}} 中設定專用映像檔儲存庫，並且將密碼新增至 Kubernetes 叢集，讓應用程式可以存取 {{site.data.keyword.toneanalyzershort}} 服務。
{: shortdesc}

1.  如果叢集位於 `default` 以外的資源群組中，請將目標設為該資源群組。若要查看每一個叢集所屬的資源群組，請執行 `ibmcloud ks clusters`。
   ```
   ibmcloud target -g <resource_group_name>
   ```
   {: pre}

2.  在 {{site.data.keyword.registryshort_notm}} 中設定您自己的專用映像檔儲存庫，以安全地儲存 Docker 映像檔，並將其與所有叢集使用者共用。{{site.data.keyword.Bluemix_notm}} 中的專用映像檔儲存庫是透過名稱空間識別。名稱空間是用來建立映像檔儲存庫的唯一 URL，而開發人員可以使用映像檔儲存庫來存取專用 Docker 映像檔。

    進一步瞭解使用容器映像檔時如何[保護個人資訊安全](/docs/containers?topic=containers-security#pi)。

    在此範例中，公關公司只要在 {{site.data.keyword.registryshort_notm}} 中建立一個映像檔儲存庫，因此選擇 `pr_firm` 作為名稱空間來分組其帳戶中的所有映像檔。請將 &lt;namespace&gt; 取代為您所選擇的名稱空間，其與指導教學不相關。

    ```
    ibmcloud cr namespace-add <namespace>
    ```
    {: pre}

3.  繼續下一步之前，請驗證工作者節點的部署已完成。

    ```
        ibmcloud ks workers --cluster <cluster_name_or_ID>
        ```
    {: pre}

    當您的工作者節點完成佈建時，狀態會變更為 **Ready**，且您可以開始連結 {{site.data.keyword.Bluemix_notm}} 服務。

    ```
    ID                                                 Public IP       Private IP       Machine Type   State    Status   Zone   Version
    kube-mil01-pafe24f557f070463caf9e31ecf2d96625-w1   169.xx.xxx.xxx   10.xxx.xx.xxx   free           normal   Ready    mil01      1.13.6
    ```
    {: screen}

## 課程 3：設定您的叢集環境
{: #cs_cluster_tutorial_lesson3}

在 CLI 中，設定 Kubernetes 叢集的環境定義。
{: shortdesc}

每次登入 {{site.data.keyword.containerlong}} CLI 以使用叢集時，您都必須執行這些指令，以將叢集配置檔的路徑設為階段作業變數。Kubernetes CLI 會使用此變數來尋找與 {{site.data.keyword.Bluemix_notm}} 中的叢集連接所需的本端配置檔及憑證。

1.  取得指令來設定環境變數，並下載 Kubernetes 配置檔。
    ```
    ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
    ```
    {: pre}

    配置檔下載完成之後，會顯示一個指令，可讓您用來將本端 Kubernetes 配置檔的路徑設定為環境變數。

    OS X 的範例：
    ```
    export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/pr_firm_cluster/kube-config-prod-par02-pr_firm_cluster.yml
    ```
    {: screen}
    使用的是 Windows PowerShell？請包含 `--powershell` 旗標，以取得 Windows PowerShell 格式的環境變數。
    {: tip}

2.  複製並貼上終端機中顯示的指令，以設定 `KUBECONFIG` 環境變數。

3.  驗證已適當地設定 `KUBECONFIG` 環境變數。

    OS X 的範例：

    ```
    echo $KUBECONFIG
    ```
    {: pre}

    輸出：

    ```
    /Users/<user_name>/.bluemix/plugins/container-service/clusters/pr_firm_cluster/kube-config-prod-par02-pr_firm_cluster.yml
    ```
    {: screen}

4.  檢查 Kubernetes CLI 伺服器版本，驗證叢集已適當地執行 `kubectl` 指令。

    ```
    kubectl version  --short
    ```
    {: pre}

    輸出範例：

    ```
    Client Version: v1.13.6
    Server Version: v1.13.6
    ```
    {: screen}

## 課程 4：將服務新增至您的叢集
{: #cs_cluster_tutorial_lesson4}

使用 {{site.data.keyword.Bluemix_notm}} 服務，您可以在應用程式中利用已經開發的功能。任何在該叢集裡部署的應用程式，都可以使用已連結至 Kubernetes 叢集的任何 {{site.data.keyword.Bluemix_notm}} 服務。請針對每個您要與應用程式搭配使用的 {{site.data.keyword.Bluemix_notm}} 服務，重複下列步驟。
{: shortdesc}

1.  將 {{site.data.keyword.toneanalyzershort}} 服務新增至 {{site.data.keyword.Bluemix_notm}} 帳戶。將 <service_name> 取代為您服務實例的名稱。

    當您將 {{site.data.keyword.toneanalyzershort}} 服務新增至帳戶時，會顯示一則訊息指出該服務並非免費。如果您限制 API 呼叫，則此指導教學不會引起 {{site.data.keyword.watson}} 服務的費用。[請檢閱 {{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}} 服務的定價資訊 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/catalog/services/tone-analyzer)。
    {: note}

    ```
    ibmcloud resource service-instance-create <service_name> tone-analyzer standard us-south
    ```
    {: pre}

2.  將 {{site.data.keyword.toneanalyzershort}} 實例連結至叢集的 `default` Kubernetes 名稱空間。稍後，您可以建立自己的名稱空間來管理使用者對 Kubernetes 資源的存取權，但現在請使用 `default` 名稱空間。Kubernetes 名稱空間與您稍早所建立的登錄名稱空間不同。

    ```
    ibmcloud ks cluster-service-bind --cluster <cluster_name> --namespace default --service <service_name>
    ```
    {: pre}

    輸出：

    ```
    ibmcloud ks cluster-service-bind pr_firm_cluster default mytoneanalyzer
    Binding service instance to namespace...
    OK
    Namespace:	default
    Secret name:	binding-mytoneanalyzer
    ```
    {: screen}

3.  驗證已在叢集名稱空間中建立 Kubernetes 密碼。包括機密資訊（例如，{{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM) API 金鑰，以及容器用來取得存取權的 URL）的 JSON 檔案會定義每個 {{site.data.keyword.Bluemix_notm}} 服務。為了安全地儲存此資訊，因此已使用 Kubernetes 密碼。在此範例中，密碼包括可用於存取帳戶中所佈建 {{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}} 實例的 API 金鑰。

    ```
    kubectl get secrets --namespace=default
    ```
    {: pre}

    輸出：

    ```
    NAME                       TYPE                                  DATA      AGE
    binding-mytoneanalyzer     Opaque                                1         1m
    bluemix-default-secret     kubernetes.io/dockercfg               1         1h
    default-token-kf97z        kubernetes.io/service-account-token   3         1h
    ```
    {: screen}

</br>
做得好！已配置叢集，且您的本端環境已就緒，您可以開始將應用程式部署到叢集。

## 下一步為何？
{: #tutorials_next}

* 測試您的知識，並[進行隨堂測驗 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://ibmcloud-quizzes.mybluemix.net/containers/cluster_tutorial/quiz.php)！
* 嘗試[指導教學：將應用程式部署至 Kubernetes 叢集](/docs/containers?topic=containers-cs_apps_tutorial#cs_apps_tutorial)，以將公關公司的應用程式部署至您所建立的叢集。
