---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: kubernetes, iks, ibmcloud, ic, ks, kubectl

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



# 設定 CLI 及 API
{: #cs_cli_install}

您可以使用 {{site.data.keyword.containerlong}} CLI 或 API 來建立及管理 Kubernetes 叢集。
{:shortdesc}

## 安裝 IBM Cloud CLI 和外掛程式
{: #cs_cli_install_steps}

安裝必要 CLI 以在 {{site.data.keyword.containerlong_notm}} 中建立及管理 Kubernetes 叢集，並且將容器化應用程式部署至叢集。
{:shortdesc}

此作業包括安裝這些 CLI 及外掛程式的資訊：

-   {{site.data.keyword.Bluemix_notm}} CLI 
-   {{site.data.keyword.containerlong_notm}} 外掛程式
-   {{site.data.keyword.registryshort_notm}} 外掛程式

如果您要改用 {{site.data.keyword.Bluemix_notm}} 主控台，則在建立叢集之後，您可以在 [Kubernetes Terminal](#cli_web) 中從 Web 瀏覽器直接執行 CLI 指令。
{: tip}

<br>
若要安裝 CLI，請執行下列動作：



1.  安裝 [{{site.data.keyword.Bluemix_notm}} CLI ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](/docs/cli?topic=cloud-cli-getting-started#idt-prereq)。此安裝包括：
    -   基本 {{site.data.keyword.Bluemix_notm}} CLI (`ibmcloud`)。
    -   {{site.data.keyword.containerlong_notm}} 外掛程式 (`ibmcloud ks`)。
    -   {{site.data.keyword.registryshort_notm}} 外掛程式 (`ibmcloud cr`)。您可以使用此外掛程式，在多方承租戶、高可用性並且由 IBM 所管理的可擴充專用映像檔登錄中設定您自己的名稱空間，以及儲存 Docker 映像檔，並將其與其他使用者共用。需要有 Docker 映像檔，才能將容器部署至叢集。
    -   Kubernetes CLI (`kubectl`)，與預設版本 1.13.6 相符合。<p class="note">如果計劃使用執行其他版本的叢集，則可能需要[個別安裝該版本的 Kubernetes CLI](#kubectl)。如果您有 (OpenShift) 叢集，請[同時安裝 `oc` 和 `kubectl` CLI](#cli_oc)。</p>
    -   Helm CLI (`helm`)。可以將 Helm 用作套件管理程式，以透過 Helm 圖表將 {{site.data.keyword.Bluemix_notm}} 服務和複雜應用程式安裝到叢集。但仍必須在要使用 Helm 的每個叢集裡[設定 Helm](/docs/containers?topic=containers-helm)。

    計劃要使用很多 CLI 嗎？請嘗試[啟用 {{site.data.keyword.Bluemix_notm}} CLI 的 Shell 自動完成（僅限 Linux/MacOS）](/docs/cli/reference/ibmcloud?topic=cloud-cli-shell-autocomplete#shell-autocomplete-linux)。
    {: tip}

2.  登入 {{site.data.keyword.Bluemix_notm}} CLI。系統提示時，請輸入您的 {{site.data.keyword.Bluemix_notm}} 認證。
    ```
    ibmcloud login
    ```
    {: pre}

    如果您具有聯合 ID，請使用 `ibmcloud login --sso` 來登入 {{site.data.keyword.Bluemix_notm}} CLI。請輸入使用者名稱，並使用 CLI 輸出中提供的 URL 來擷取一次性密碼。若沒有 `--sso` 時登入失敗，而有 `--sso` 選項時登入成功，即表示您有聯合 ID。
    {: tip}

3.  驗證 {{site.data.keyword.containerlong_notm}} 外掛程式和 {{site.data.keyword.registryshort_notm}} 外掛程式是否已正確安裝。
    ```
    ibmcloud plugin list
    ```
    {: pre}

    輸出範例：
    ```
    Listing installed plug-ins...

    Plugin Name                            Version   Status        
    container-registry                     0.1.373     
    container-service/kubernetes-service   0.3.23   
    ```
    {: screen}

如需這些 CLI 的相關參考資訊，請參閱那些工具的文件。

-   [`ibmcloud` 指令](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_cli#ibmcloud_cli)
-   [`ibmcloud ks` 指令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli)
-   [`ibmcloud cr` 指令](/docs/services/Registry?topic=container-registry-cli-plugin-containerregcli)

## 安裝 Kubernetes CLI (`kubectl`)
{: #kubectl}

若要檢視 Kubernetes 儀表板的本端版本以及將應用程式部署到叢集，請安裝 Kubernetes CLI (`kubectl`)。最新穩定版本的 `kubectl` 會隨著基本 {{site.data.keyword.Bluemix_notm}} CLI 一起安裝。不過，若要使用叢集，您必須改為安裝與您計劃使用之 Kubernetes 叢集 `major.minor` 版本相符的 Kubernetes CLI `major.minor` 版本。如果您使用的 `kubectl` CLI 版本未至少符合叢集的 `major.minor` 版本，則您可能會看到非預期的結果。例如，[Kubernetes 不支援 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/setup/version-skew-policy/) 比伺服器版本高/低 2 個或更多版本 (n +/- 2) 的 `kubectl` 用戶端版本。請確定 Kubernetes 叢集和 CLI 版本保持最新。
{: shortdesc}

正在使用 OpenShift 叢集嗎？請改為安裝 OpenShift Origin CLI (`oc`)，該 CLI 隨附 `kubectl`。如果同時有 Red Hat OpenShift on IBM Cloud 和 Ubuntu 原生 {{site.data.keyword.containershort_notm}} 叢集，請確保使用與叢集 `major.minor` Kubernetes 版本符合的 `kubectl` 二進位檔。
{: tip}

1.  如果您已具有叢集，請檢查用戶端 `kubectl` CLI 的版本是否與叢集 API 伺服器的版本相符合。
    1.  [登入您的帳戶。適用的話，請將適當的資源群組設為目標。設定叢集的環境定義。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
    2.  比較用戶端和伺服器版本。如果用戶端與伺服器不符合，請繼續執行下一步。如果版本符合，說明您已安裝正確版本的 `kubectl`。
        ```
    kubectl version  --short
    ```
        {: pre}
2.  下載與您計劃使用的 Kubernetes 叢集 `major.minor` 版本相符的 Kubernetes CLI `major.minor` 版本。現行 {{site.data.keyword.containerlong_notm}} 預設 Kubernetes 版本為 1.13.6。
    -   **OS X**：[https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/darwin/amd64/kubectl ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/darwin/amd64/kubectl)
    -   **Linux**：[https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/linux/amd64/kubectl ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/linux/amd64/kubectl)
    -   **Windows**：將 Kubernetes CLI 安裝在與 {{site.data.keyword.Bluemix_notm}} CLI 相同的目錄中。當您稍後執行指令時，這項設定可為您省去一些檔案路徑變更。[https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/windows/amd64/kubectl.exe ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/windows/amd64/kubectl.exe)

3.  如果您使用 OS X 或 Linux，請完成下列步驟。
    1.  將執行檔移至 `/usr/local/bin` 目錄。
      ```
        mv /<filepath>/kubectl /usr/local/bin/kubectl
        ```
        {: pre}

    2.  確定 `/usr/local/bin` 列出在 `PATH` 系統變數中。`PATH` 變數包含作業系統可在其中找到執行檔的所有目錄。`PATH` 變數中所列的目錄用於不同的用途。`/usr/local/bin` 是用來儲存軟體的執行檔，該軟體不是作業系統的一部分，並且已由系統管理者手動安裝。
      ```
      echo $PATH
      ```
        {: pre}
      CLI 輸出範例：
      ```
      /usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
      ```
        {: screen}

    3.  使檔案成為可執行檔。
      ```
      chmod +x /usr/local/bin/kubectl
      ```
        {: pre}
4.  **選用性**：[啟用 `kubectl` 指令的自動完成 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/tasks/tools/install-kubectl/#enabling-shell-autocompletion)。步驟會視您使用的 Shell 而變。

接下來，開始[使用 {{site.data.keyword.containerlong_notm}} 透過 CLI 建立 Kubernetes 叢集](/docs/containers?topic=containers-clusters#clusters_cli_steps)。

如需 Kubernetes CLI 的相關資訊，請參閱 [`kubectl` 參考文件 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubectl.docs.kubernetes.io/)。
{: note}

<br />


## 安裝 OpenShift Origin CLI (`oc`) 預覽測試版
{: #cli_oc}

[Red Hat OpenShift on IBM Cloud](/docs/containers?topic=containers-openshift_tutorial) 以測試版形式提供，用來測試 OpenShift 叢集。
{: preview}

若要檢視 OpenShift 儀表板的本端版本以及將應用程式部署到 Red Hat OpenShift on IBM Cloud 叢集，請安裝 OpenShift CLI (`oc`)。`oc` CLI 包含符合版本的 Kubernetes CLI (`kubectl`)。如需相關資訊，請參閱 [OpenShift 文件 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.openshift.com/container-platform/3.11/cli_reference/get_started_cli.html)。
{: shortdesc}

同時使用 Red Hat OpenShift on IBM Cloud 和 Ubuntu 原生 {{site.data.keyword.containershort_notm}} 叢集嗎？`oc` CLI 隨附 `oc` 和 `kubectl` 二進位檔，但不同叢集可能執行不同版本的 Kubernetes，例如 OpenShift 上執行 1.11，Ubuntu 上執行 1.13.6。確保使用與叢集 `major.minor` Kubernetes 版本符合的 `kubectl` 二進位檔。
{: note}

1.  根據本端作業系統和 OpenShift 版本，[下載 OpenShift Origin CLI ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.okd.io/download.html)。現行預設 OpenShift 版本為 3.11。

2.  如果使用訊息鑑別碼 OS 或 Linux，請完成下列步驟將二進位檔新增到 `PATH` 系統變數。如果使用 Windows，請將 `oc` CLI 安裝在 {{site.data.keyword.Bluemix_notm}} CLI 所在的目錄中。當您稍後執行指令時，這項設定可為您省去一些檔案路徑變更。
    1.  將 `oc` 和 `kubectl` 執行檔移至 `/usr/local/bin` 目錄。
        ```
        mv /<filepath>/oc /usr/local/bin/oc && mv /<filepath>/kubectl /usr/local/bin/kubectl
        ```
        {: pre}

    2.  確定 `/usr/local/bin` 列出在 `PATH` 系統變數中。`PATH` 變數包含作業系統可在其中找到執行檔的所有目錄。`PATH` 變數中所列的目錄用於不同的用途。`/usr/local/bin` 是用來儲存軟體的執行檔，該軟體不是作業系統的一部分，並且已由系統管理者手動安裝。
      ```
      echo $PATH
      ```
        {: pre}
      CLI 輸出範例：
      ```
      /usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
      ```
        {: screen}
3.  **選用性**：[啟用 `kubectl` 指令的自動完成 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/tasks/tools/install-kubectl/#enabling-shell-autocompletion)。步驟會視您使用的 Shell 而變。可以重複這些步驟來啟用 `oc` 指令的自動完成。例如，在 Linux 上的 bash 中，可以執行 `oc completion bash >/etc/bash_completion.d/oc_completion`，以代替 `kubectl completion bash >/etc/bash_completion.d/kubectl`。

接下來，開始[建立 Red Hat OpenShift on IBM Cloud 叢集（預覽）](/docs/containers?topic=containers-openshift_tutorial)。

如需 OpenShift Origin CLI 的相關資訊，請參閱 [`oc` 指令文件 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.openshift.com/container-platform/3.11/cli_reference/basic_cli_operations.html)。
{: note}

<br />


## 在電腦上的容器中執行 CLI
{: #cs_cli_container}

不是在您的電腦上個別安裝每一個 CLI，而是您可以將 CLI 安裝至在您電腦上執行的容器。
{:shortdesc}

開始之前，請[安裝 Docker ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.docker.com/community-edition#/download)，以在本端建置並執行映像檔。如果您使用 Windows 8 或更早版本，則可以改為安裝 [Docker Toolbox ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.docker.com/toolbox/toolbox_install_windows/)。

1. 從提供的 Dockerfile 建立映像檔。

    ```
    docker build -t <image_name> https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/install-clis-container/Dockerfile
    ```
    {: pre}

2. 在本端部署映像檔作為容器，並裝載磁區來存取本端檔案。

    ```
    docker run -it -v /local/path:/container/volume <image_name>
    ```
    {: pre}

3. 從互動式 Shell 開始執行 `ibmcloud ks` 及 `kubectl` 指令。如果您建立了要儲存的資料，請將該資料儲存至您所裝載的磁區。當您結束 Shell 時，容器就會停止。

<br />



## 配置 CLI 以執行 `kubectl`
{: #cs_cli_configure}

您可以使用 Kubernetes CLI 隨附的指令來管理 {{site.data.keyword.Bluemix_notm}} 中的叢集。
{:shortdesc}

支援 Kubernetes 1.13.6 中可用的所有 `kubectl` 指令用於 {{site.data.keyword.Bluemix_notm}} 中的叢集。建立叢集之後，使用環境變數將本端 CLI 的環境定義設定為該叢集。然後，您可以執行 Kubernetes `kubectl` 指令，在 {{site.data.keyword.Bluemix_notm}} 中使用您的叢集。


在可以執行 `kkbectl` 指令之前，請執行下列動作：
* [安裝必要的 CLI](#cs_cli_install)。
* [建立叢集](/docs/containers?topic=containers-clusters#clusters_cli_steps)。
* 請確定已經具有[服務角色](/docs/containers?topic=containers-users#platform)來授與適當的 Kubernetes RBAC 角色，以便您可以使用 Kubernetes 資源。如果您只有服務角色而無平台角色，您需要叢集管理者為您提供叢集名稱和 ID，或提供**檢視者**平台角色來列出叢集。

若要使用 `kubectl` 指令，請執行下列動作：

1.  登入 {{site.data.keyword.Bluemix_notm}} CLI。系統提示時，請輸入您的 {{site.data.keyword.Bluemix_notm}} 認證。

    ```
    ibmcloud login
    ```
    {: pre}

    如果您具有聯合 ID，請使用 `ibmcloud login --sso` 來登入 {{site.data.keyword.Bluemix_notm}} CLI。請輸入使用者名稱，並使用 CLI 輸出中提供的 URL 來擷取一次性密碼。若沒有 `--sso` 時登入失敗，而有 `--sso` 選項時登入成功，即表示您有聯合 ID。
    {: tip}

2.  選取 {{site.data.keyword.Bluemix_notm}} 帳戶。如果您被指派到多個 {{site.data.keyword.Bluemix_notm}} 組織，請選取在其中建立叢集的組織。叢集是組織特有的，但與 {{site.data.keyword.Bluemix_notm}} 空間無關。因此，您不需要選取空間。

3.  若要建立及使用非 default 資源群組中的叢集，請將目標設為該資源群組。若要查看每一個叢集所屬的資源群組，請執行 `ibmcloud ks clusters`。**附註**：您必須對資源群組具有[**檢視者**存取權](/docs/containers?topic=containers-users#platform)。
    ```
    ibmcloud target -g <resource_group_name>
    ```
    {: pre}

4.  列出帳戶中的所有叢集，以取得叢集的名稱。如果您只有 {{site.data.keyword.Bluemix_notm}} IAM 服務角色，無法檢視叢集，請要求叢集管理者提供 IAM 平台**檢視者**角色，或是叢集名稱和 ID。

    ```
    ibmcloud ks clusters
    ```
    {: pre}

5.  將您建立的叢集設為此階段作業的環境定義。請在您每次使用叢集時，完成下列配置步驟。
    1.  取得指令來設定環境變數，並下載 Kubernetes 配置檔。<p class="tip">正在使用 Windows PowerShell 嗎？請包含 `--powershell` 旗標，以取得 Windows PowerShell 格式的環境變數。</p>
        ```
        ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
        ```
        {: pre}

        下載配置檔之後，會顯示一個指令，可讓您用來將本端 Kubernetes 配置檔的路徑設定為環境變數。

        範例：

        ```
        export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/mycluster/kube-config-prod-dal10-mycluster.yml
        ```
        {: screen}

    2.  複製並貼上終端機中顯示的指令，以設定 `KUBECONFIG` 環境變數。

        **Mac 或 Linux 使用者**：不執行 `ibmcloud ks cluster-config` 指令及複製 `KUBECONFIG` 環境變數，您可以改為執行 `ibmcloud ks cluster-config --export <cluster-name>`。根據您的 Shell，您可以透過執行 `eval $(ibmcloud ks cluster-config --export) <cluster-name>)`  來設定 Shell。
        {: tip}

    3.  驗證已適當地設定 `KUBECONFIG` 環境變數。

        範例：

        ```
        echo $KUBECONFIG
        ```
        {: pre}

        輸出：
        ```
        /Users/<user_name>/.bluemix/plugins/container-service/clusters/mycluster/kube-config-prod-dal10-mycluster.yml
        ```
        {: screen}

6.  檢查 Kubernetes CLI 伺服器版本，驗證叢集已適當地執行 `kubectl` 指令。

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

您現在可以執行 `kubectl` 指令，以在 {{site.data.keyword.Bluemix_notm}} 中管理叢集。如需完整指令清單，請參閱 [Kubernetes 文件 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubectl.docs.kubernetes.io/)。

**提示：**如果您使用 Windows，但未在與 {{site.data.keyword.Bluemix_notm}} CLI 相同的目錄中安裝 Kubernetes CLI，則必須將目錄切換至 Kubernetes CLI 安裝所在的路徑，才能順利執行 `kubectl` 指令。


<br />




## 更新 CLI
{: #cs_cli_upgrade}

建議您定期更新 CLI，以使用新特性。
{:shortdesc}

此作業包括更新這些 CLI 的資訊。

-   {{site.data.keyword.Bluemix_notm}} CLI 0.8.0 版或更新版本
-   {{site.data.keyword.containerlong_notm}} 外掛程式
-   Kubernetes CLI 1.13.6 版或更新版本
-   {{site.data.keyword.registryshort_notm}} 外掛程式

<br>
若要更新 CLI，請執行下列動作：

1.  更新 {{site.data.keyword.Bluemix_notm}} CLI。下載[最新版本 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](/docs/cli?topic=cloud-cli-getting-started)，並執行安裝程式。

2. 登入 {{site.data.keyword.Bluemix_notm}} CLI。系統提示時，請輸入您的 {{site.data.keyword.Bluemix_notm}} 認證。

    ```
    ibmcloud login
    ```
    {: pre}

     如果您具有聯合 ID，請使用 `ibmcloud login --sso` 來登入 {{site.data.keyword.Bluemix_notm}} CLI。請輸入使用者名稱，並使用 CLI 輸出中提供的 URL 來擷取一次性密碼。若沒有 `--sso` 時登入失敗，而有 `--sso` 選項時登入成功，即表示您有聯合 ID。
    {: tip}

3.  更新 {{site.data.keyword.containerlong_notm}} 外掛程式。
    1.  從 {{site.data.keyword.Bluemix_notm}} 外掛程式儲存庫中安裝更新。

        ```
        ibmcloud plugin update container-service
        ```
        {: pre}

    2.  執行下列指令，並檢查已安裝的外掛程式清單，以驗證外掛程式安裝。

        ```
        ibmcloud plugin list
        ```
        {: pre}

        在結果中，{{site.data.keyword.containerlong_notm}} 外掛程式會顯示為 container-service。

    3.  起始設定 CLI。

        ```
        ibmcloud ks init
        ```
        {: pre}

4.  [更新 Kubernetes CLI](#kubectl)。

5.  更新 {{site.data.keyword.registryshort_notm}} 外掛程式。
    1.  從 {{site.data.keyword.Bluemix_notm}} 外掛程式儲存庫中安裝更新。

        ```
        ibmcloud plugin update container-registry 
        ```
        {: pre}

    2.  執行下列指令，並檢查已安裝的外掛程式清單，以驗證外掛程式安裝。

        ```
        ibmcloud plugin list
        ```
        {: pre}

        在結果中，登錄外掛程式會顯示為 container-registry。

<br />


## 解除安裝 CLI
{: #cs_cli_uninstall}

如果不再需要 CLI，則可以將它解除安裝。
{:shortdesc}

此作業包括移除這些 CLI 的資訊：


-   {{site.data.keyword.containerlong_notm}} 外掛程式
-   Kubernetes CLI
-   {{site.data.keyword.registryshort_notm}} 外掛程式

若要解除安裝 CLI，請執行下列動作：

1.  解除安裝 {{site.data.keyword.containerlong_notm}} 外掛程式。

    ```
    ibmcloud plugin uninstall container-service
    ```
    {: pre}

2.  解除安裝 {{site.data.keyword.registryshort_notm}} 外掛程式。

    ```
    ibmcloud plugin uninstall container-registry
    ```
    {: pre}

3.  執行下列指令，並檢查已安裝的外掛程式清單，以驗證已解除安裝外掛程式。

    ```
    ibmcloud plugin list
    ```
    {: pre}

    container-service 及 container-registry 外掛程式不會顯示在結果中。

<br />


## 在 Web 瀏覽器中使用 Kubernetes Terminal（測試版）
{: #cli_web}

Kubernetes Terminal 容許您使用 {{site.data.keyword.Bluemix_notm}} CLI 直接從 Web 瀏覽器管理叢集。
{: shortdesc}

Kubernetes Terminal 發行為測試版 {{site.data.keyword.containerlong_notm}} 附加程式，而且可能會因使用者意見及進一步測試而變更。請不要在正式作業叢集裡使用此特性，以避免非預期的負面影響。
{: important}

如果您在 {{site.data.keyword.Bluemix_notm}} 主控台中使用叢集儀表板來管理叢集，但要快速進行更進階的配置變更，則現在可以在 Kubernetes Terminal 中從 Web 瀏覽器直接執行 CLI 指令。Kubernetes Terminal 是使用基礎 [{{site.data.keyword.Bluemix_notm}} CLI ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](/docs/cli?topic=cloud-cli-getting-started)、{{site.data.keyword.containerlong_notm}} 外掛程式及 {{site.data.keyword.registryshort_notm}} 外掛程式所啟用。此外，終端機環境定義已設為您正在使用的叢集，因此，您可以執行 Kubernetes `kubectl` 指令來使用叢集。

您在本端下載及編輯的所有檔案（例如 YAML 檔案）都會暫時儲存在 Kubernetes Terminal 中，而且不會在階段作業之間持續保存。
{: note}

若要安裝及啟動 Kubernetes Terminal，請執行下列動作：

1.  登入 [{{site.data.keyword.Bluemix_notm}} 主控台](https://cloud.ibm.com/)。
2.  從功能表列，選取您要使用的帳戶。
3.  從功能表 ![「功能表」圖示](../icons/icon_hamburger.svg "「功能表」圖示") 中，按一下 **Kubernetes**。
4.  在**叢集**頁面上，按一下您要存取的叢集。
5.  從叢集詳細資料頁面中，按一下**終端機**按鈕。
6.  按一下**安裝**。這可能需要幾分鐘的時間，才能安裝終端機附加程式。
7.  再按一下**終端機**按鈕。即會在瀏覽器中開啟終端機。

下次，只要按一下**終端機**按鈕，您就可以啟動 Kubernetes Terminal。

<br />


## 使用 API 自動化進行叢集部署
{: #cs_api}

您可以使用 {{site.data.keyword.containerlong_notm}} API 來自動化進行 Kubernetes 叢集的建立、部署及管理。
{:shortdesc}

{{site.data.keyword.containerlong_notm}} API 需要標頭資訊，您必須在 API 要求中提供它，且它會視您要使用的 API 而變。若要判斷 API 所需的標頭資訊，請參閱 [{{site.data.keyword.containerlong_notm}} API 文件 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://us-south.containers.cloud.ibm.com/swagger-api)。

若要向 {{site.data.keyword.containerlong_notm}} 進行鑑別，您必須提供以 {{site.data.keyword.Bluemix_notm}} 認證產生且包含建立叢集所在 {{site.data.keyword.Bluemix_notm}} 帳戶 ID 的 {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM) 記號。取決於您向 {{site.data.keyword.Bluemix_notm}} 進行鑑別的方式，您可以在下列選項之間進行選擇，以自動建立 {{site.data.keyword.Bluemix_notm}} IAM 記號。

您也可以使用 [API Swagger JSON 檔案 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://containers.cloud.ibm.com/global/swagger-global-api/swagger.json) 來產生可在自動化工作期間與 API 互動的用戶端。
{: tip}

<table summary="ID 類型及選項，第 1 欄是輸入參數，而第 2 欄是值。">
<caption>ID 類型和選項</caption>
<thead>
<th>{{site.data.keyword.Bluemix_notm}} ID</th>
<th>我的選項</th>
</thead>
<tbody>
<tr>
<td>未聯合 ID</td>
<td><ul><li><strong>產生 {{site.data.keyword.Bluemix_notm}} API 金鑰：</strong>如果不使用 {{site.data.keyword.Bluemix_notm}} 使用者名稱和密碼，您的另一個替代方案是<a href="/docs/iam?topic=iam-userapikey#create_user_key" target="_blank">使用 {{site.data.keyword.Bluemix_notm}} API 金鑰</a>。{{site.data.keyword.Bluemix_notm}} API 金鑰相依於要產生它們的 {{site.data.keyword.Bluemix_notm}} 帳戶。您無法將 {{site.data.keyword.Bluemix_notm}} API 金鑰與不同帳戶 ID 結合在相同的 {{site.data.keyword.Bluemix_notm}} IAM 記號中。若要存取使用您 {{site.data.keyword.Bluemix_notm}} API 金鑰根據帳戶以外之帳戶建立的叢集，您必須登入帳戶才能產生新的 API 金鑰。</li>
<li><strong>{{site.data.keyword.Bluemix_notm}} 使用者名稱及密碼：</strong>您可以遵循本主題的步驟，以完全自動建立 {{site.data.keyword.Bluemix_notm}} IAM 存取記號。</li></ul>
</tr>
<tr>
<td>聯合 ID</td>
<td><ul><li><strong>產生 {{site.data.keyword.Bluemix_notm}} API 金鑰：</strong><a href="/docs/iam?topic=iam-userapikey#create_user_key" target="_blank">{{site.data.keyword.Bluemix_notm}} API 金鑰</a>相依於要產生它們的 {{site.data.keyword.Bluemix_notm}} 帳戶。您無法將 {{site.data.keyword.Bluemix_notm}} API 金鑰與不同帳戶 ID 結合在相同的 {{site.data.keyword.Bluemix_notm}} IAM 記號中。若要存取使用您 {{site.data.keyword.Bluemix_notm}} API 金鑰根據帳戶以外之帳戶建立的叢集，您必須登入帳戶才能產生新的 API 金鑰。</li>
<li><strong>使用一次性密碼：</strong>如果您使用一次性密碼來向 {{site.data.keyword.Bluemix_notm}} 進行鑑別，則無法完全自動建立 {{site.data.keyword.Bluemix_notm}} IAM 記號，因為擷取一次性密碼需要與 Web 瀏覽器進行手動互動。若要完全自動建立 {{site.data.keyword.Bluemix_notm}} IAM 記號，您必須改為建立一個 {{site.data.keyword.Bluemix_notm}} API 金鑰。</ul></td>
</tr>
</tbody>
</table>

1.  建立 {{site.data.keyword.Bluemix_notm}} IAM 存取記號。要求中包含的內文資訊會根據您使用的 {{site.data.keyword.Bluemix_notm}} 鑑別方法而有所不同。

    ```
    POST https://iam.bluemix.net/identity/token
    ```
    {: codeblock}

    <table summary="擷取 IAM 記號的輸入參數，第 1 欄是輸入參數，而第 2 欄是值。">
    <caption>取得 IAM 記號的輸入參數。</caption>
    <thead>
        <th>輸入參數</th>
        <th>值</th>
    </thead>
    <tbody>
    <tr>
    <td>Header</td>
    <td><ul><li>Content-Type: application/x-www-form-urlencoded</li> <li>Authorization: Basic Yng6Yng=<p><strong>附註</strong>：<code>Yng6Yng=</code> 等於使用者名稱 <strong>bx</strong> 及密碼 <strong>bx</strong> 的 URL 編碼授權。</p></li></ul>
    </td>
    </tr>
    <tr>
    <td>{{site.data.keyword.Bluemix_notm}} 使用者名稱和密碼的內文</td>
    <td><ul><li>`grant_type: password`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`username`：您的 {{site.data.keyword.Bluemix_notm}} 使用者名稱。</li>
    <li>`password`：您的 {{site.data.keyword.Bluemix_notm}} 密碼。</li>
    <li>`uaa_client_id: cf`</li>
    <li>`uaa_client_secret:`</br> <strong>附註</strong>：新增未指定任何值的 <code>uaa_client_secret</code> 金鑰。</li></ul></td>
    </tr>
    <tr>
    <td>{{site.data.keyword.Bluemix_notm}} API 金鑰的內文</td>
    <td><ul><li>`grant_type: urn:ibm:params:oauth:grant-type:apikey`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`apikey`：您的 {{site.data.keyword.Bluemix_notm}} API 金鑰</li>
    <li>`uaa_client_id: cf`</li>
    <li>`uaa_client_secret:`</br> <strong>附註</strong>：新增未指定任何值的 <code>uaa_client_secret</code> 金鑰。</li></ul></td>
    </tr>
    <tr>
    <td>{{site.data.keyword.Bluemix_notm}} 一次性密碼的內文</td>
      <td><ul><li><code>grant_type: urn:ibm:params:oauth:grant-type:passcode</code></li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`passcode`：您的 {{site.data.keyword.Bluemix_notm}} 一次性密碼。執行 `ibmcloud login --sso`，並遵循 CLI 輸出中的指示，使用 Web 瀏覽器來擷取一次性密碼。</li>
    <li>`uaa_client_id: cf`</li>
    <li>`uaa_client_secret:`</br> <strong>附註</strong>：新增未指定任何值的 <code>uaa_client_secret</code> 金鑰。</li></ul>
    </td>
    </tr>
    </tbody>
    </table>

    使用 API 金鑰的範例輸出：

    ```
    {
    "access_token": "<iam_access_token>",
    "refresh_token": "<iam_refresh_token>",
    "uaa_token": "<uaa_token>",
    "uaa_refresh_token": "<uaa_refresh_token>",
    "token_type": "Bearer",
    "expires_in": 3600,
    "expiration": 1493747503
    "scope": "ibm openid"
    }

    ```
    {: screen}

    您可以在 API 輸出的 **access_token** 欄位中找到 {{site.data.keyword.Bluemix_notm}} IAM 記號。請記下 {{site.data.keyword.Bluemix_notm}} IAM 記號，以在接下來的步驟中擷取其他標頭資訊。

2.  擷取您要使用的 {{site.data.keyword.Bluemix_notm}} 帳戶 ID。將 `<iam_access_token>` 取代為您在前一個步驟中從 API 輸出的 **access_token** 欄位中擷取的 {{site.data.keyword.Bluemix_notm}} IAM 記號。在 API 輸出中，您可以在 **resources.metadata.guid** 欄位中找到 {{site.data.keyword.Bluemix_notm}} 帳戶的 ID。

    ```
    GET https://accountmanagement.ng.bluemix.net/v1/accounts
    ```
    {: codeblock}

    <table summary="取得 {{site.data.keyword.Bluemix_notm}} 帳戶 ID 的輸入參數，第 1 欄是輸入參數，而第 2 欄是值。">
    <caption>取得 {{site.data.keyword.Bluemix_notm}} 帳戶 ID 的輸入參數。</caption>
    <thead>
  	<th>輸入參數</th>
  	<th>值</th>
    </thead>
    <tbody>
  	<tr>
  		<td>Headers</td>
      <td><ul><li><code>Content-Type：application/json</code></li>
        <li>`Authorization: bearer <iam_access_token>`</li>
        <li><code>Accept: application/json</code></li></ul></td>
  	</tr>
    </tbody>
    </table>

    輸出範例：

    ```
    {
      "total_results": 3,
      "total_pages": 1,
      "prev_url": null,
      "next_url": null,
      "resources":
        {
          "metadata": {
            "guid": "<account_ID>",
            "url": "/v1/accounts/<account_ID>",
            "created_at": "2016-01-07T18:55:09.726Z",
            "updated_at": "2017-04-28T23:46:03.739Z",
            "origin": "BSS"
    ...
    ```
    {: screen}

3.  產生包含 {{site.data.keyword.Bluemix_notm}} 認證及您要使用之帳戶 ID 的新 {{site.data.keyword.Bluemix_notm}} IAM 記號。

    如果您使用 {{site.data.keyword.Bluemix_notm}} API 金鑰，則必須使用為其建立 API 金鑰的 {{site.data.keyword.Bluemix_notm}} 帳戶 ID。若要存取其他帳戶中的叢集，請登入此帳戶，並建立以此帳戶為基礎的 {{site.data.keyword.Bluemix_notm}} API 金鑰。
    {: note}

    ```
    POST https://iam.bluemix.net/identity/token
    ```
    {: codeblock}

    <table summary="擷取 IAM 記號的輸入參數，第 1 欄是輸入參數，而第 2 欄是值。">
    <caption>取得 IAM 記號的輸入參數。</caption>
    <thead>
        <th>輸入參數</th>
        <th>值</th>
    </thead>
    <tbody>
    <tr>
    <td>Header</td>
    <td><ul><li>`Content-Type: application/x-www-form-urlencoded`</li> <li>`Authorization: Basic Yng6Yng=`<p><strong>附註</strong>：<code>Yng6Yng=</code> 等於使用者名稱 <strong>bx</strong> 及密碼 <strong>bx</strong> 的 URL 編碼授權。</p></li></ul>
    </td>
    </tr>
    <tr>
    <td>{{site.data.keyword.Bluemix_notm}} 使用者名稱和密碼的內文</td>
    <td><ul><li>`grant_type: password`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`username`：您的 {{site.data.keyword.Bluemix_notm}} 使用者名稱。</li>
    <li>`password`：您的 {{site.data.keyword.Bluemix_notm}} 密碼。</li>
    <li>`uaa_client_ID: cf`</li>
    <li>`uaa_client_secret:`</br> <strong>附註</strong>：新增未指定任何值的 <code>uaa_client_secret</code> 金鑰。</li>
    <li>`bss_account`：您在前一個步驟中擷取的 {{site.data.keyword.Bluemix_notm}} 帳戶 ID。</li></ul>
    </td>
    </tr>
    <tr>
    <td>{{site.data.keyword.Bluemix_notm}} API 金鑰的內文</td>
    <td><ul><li>`grant_type: urn:ibm:params:oauth:grant-type:apikey`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`apikey`：您的 {{site.data.keyword.Bluemix_notm}} API 金鑰。</li>
    <li>`uaa_client_ID: cf`</li>
    <li>`uaa_client_secret:`</br> <strong>附註</strong>：新增未指定任何值的 <code>uaa_client_secret</code> 金鑰。</li>
    <li>`bss_account`：您在前一個步驟中擷取的 {{site.data.keyword.Bluemix_notm}} 帳戶 ID。</li></ul>
      </td>
    </tr>
    <tr>
    <td>{{site.data.keyword.Bluemix_notm}} 一次性密碼的內文</td>
    <td><ul><li>`grant_type: urn:ibm:params:oauth:grant-type:passcode`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`passcode`：您的 {{site.data.keyword.Bluemix_notm}} 密碼。</li>
    <li>`uaa_client_ID: cf`</li>
    <li>`uaa_client_secret:`</br> <strong>附註</strong>：新增未指定任何值的 <code>uaa_client_secret</code> 金鑰。</li>
    <li>`bss_account`：您在前一個步驟中擷取的 {{site.data.keyword.Bluemix_notm}} 帳戶 ID。</li></ul></td>
    </tr>
    </tbody>
    </table>

    輸出範例：

    ```
    {
      "access_token": "<iam_token>",
      "refresh_token": "<iam_refresh_token>",
      "token_type": "Bearer",
      "expires_in": 3600,
      "expiration": 1493747503
    }

    ```
    {: screen}

    您可以在 API 輸出的 **access_token** 欄位中找到 {{site.data.keyword.Bluemix_notm}} IAM 記號，並在 **refresh_token** 欄位中找到重新整理記號。

4.  列出可用的 {{site.data.keyword.containerlong_notm}} 地區，然後選取您要使用的地區。使用前一個步驟中的 IAM 存取記號及重新整理記號，來建置標頭資訊。
    ```
    GET https://containers.cloud.ibm.com/v1/regions
    ```
    {: codeblock}

    <table summary="擷取 {{site.data.keyword.containerlong_notm}} 地區的輸入參數，第 1 欄是輸入參數，而第 2 欄是值。">
    <caption>擷取 {{site.data.keyword.containerlong_notm}} 地區的輸入參數。</caption>
    <thead>
    <th>輸入參數</th>
    <th>值</th>
    </thead>
    <tbody>
    <tr>
    <td>Header</td>
    <td><ul><li>`Authorization: bearer <iam_token>`</li>
    <li>`X-Auth-Refresh-Token: <refresh_token>`</li></ul></td>
    </tr>
    </tbody>
    </table>

    輸出範例：
    ```
    {
    "regions": [
        {
            "name": "ap-north",
            "alias": "jp-tok",
            "cfURL": "api.au-syd.bluemix.net",
            "freeEnabled": false
        },
        {
            "name": "ap-south",
            "alias": "au-syd",
            "cfURL": "api.au-syd.bluemix.net",
            "freeEnabled": true
        },
        {
            "name": "eu-central",
            "alias": "eu-de",
            "cfURL": "api.eu-de.bluemix.net",
            "freeEnabled": true
        },
        {
            "name": "uk-south",
            "alias": "eu-gb",
            "cfURL": "api.eu-gb.bluemix.net",
            "freeEnabled": true
        },
        {
            "name": "us-east",
            "alias": "us-east",
            "cfURL": "api.ng.bluemix.net",
            "freeEnabled": false
        },
        {
            "name": "us-south",
            "alias": "us-south",
            "cfURL": "api.ng.bluemix.net",
            "freeEnabled": true
        }
    ]
    }
    ```
    {: screen}

5.  列出您選取的 {{site.data.keyword.containerlong_notm}} 地區中的所有叢集。如果您要[針對叢集執行 Kubernetes API 要求](#kube_api)，請務必記下叢集的 **ID** 及**地區**。

     ```
     GET https://containers.cloud.ibm.com/v1/clusters
     ```
     {: codeblock}

     <table summary="使用 {{site.data.keyword.containerlong_notm}} API 的輸入參數，第 1 欄是輸入參數，而第 2 欄是值。">
     <caption>使用 {{site.data.keyword.containerlong_notm}} API 的輸入參數。</caption>
     <thead>
     <th>輸入參數</th>
     <th>值</th>
     </thead>
     <tbody>
     <tr>
     <td>Header</td>
     <td><ul><li>`Authorization: bearer <iam_token>`</li>
       <li>`X-Auth-Refresh-Token: <refresh_token>`</li><li>`X-Region: <region>`</li></ul></td>
     </tr>
     </tbody>
     </table>

5.  請檢閱 [{{site.data.keyword.containerlong_notm}} API 文件 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://containers.cloud.ibm.com/global/swagger-global-api)，以尋找所支援 API 的清單。

<br />


## 使用 Kubernetes API 來使用叢集
{: #kube_api}

您可以使用 [Kubernetes API ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/reference/using-api/api-overview/)，以與 {{site.data.keyword.containerlong_notm}} 中的叢集互動。
{: shortdesc}

下列指示需要叢集裡的公用網路存取，才能連接至 Kubernetes 主節點的公用服務端點。
{: note}

1. 遵循[使用 API 自動化叢集部署](#cs_api)中的步驟，來擷取 {{site.data.keyword.Bluemix_notm}} IAM 存取記號、重新整理記號、您要執行 Kubernetes API 要求的叢集 ID，以及叢集所在的 {{site.data.keyword.containerlong_notm}} 地區。

2. 擷取 {{site.data.keyword.Bluemix_notm}} IAM 委派的重新整理記號。
   ```
   POST https://iam.bluemix.net/identity/token
   ```
   {: codeblock}

   <table summary="取得 IAM 委派的重新整理記號的輸入參數，第 1 欄是輸入參數，而第 2 欄是值。">
   <caption>取得 IAM 委派的重新整理記號的輸入參數。</caption>
   <thead>
   <th>輸入參數</th>
   <th>值</th>
   </thead>
   <tbody>
   <tr>
   <td>Header</td>
   <td><ul><li>`Content-Type: application/x-www-form-urlencoded`</li> <li>`Authorization: Basic Yng6Yng=`</br><strong>附註</strong>：<code>Yng6Yng=</code> 等於使用者名稱 <strong>bx</strong> 及密碼 <strong>bx</strong> 的 URL 編碼授權。</li><li>`cache-control: no-cache`</li></ul>
   </td>
   </tr>
   <tr>
   <td>內文</td>
   <td><ul><li>`delegated_refresh_token_expiry: 600`</li>
   <li>`receiver_client_ids: kube`</li>
   <li>`response_type: delegated_refresh_token` </li>
   <li>`refresh_token`：您的 {{site.data.keyword.Bluemix_notm}} IAM 重新整理記號。</li>
   <li>`grant_type: refresh_token`</li></ul></td>
   </tr>
   </tbody>
   </table>

   輸出範例：
   ```
   {
    "delegated_refresh_token": <delegated_refresh_token>
   }
   ```
   {: screen}

3. 使用前一個步驟中的委派重新整理記號，擷取 {{site.data.keyword.Bluemix_notm}} IAM ID、IAM 存取及 IAM 重新整理記號。在 API 輸出中，您可以在 **id_token** 欄位中找到 IAM ID 記號、在 **access_token** 欄位中找到 IAM 存取記號，以及在 **refresh_token** 欄位中找到 IAM 重新整理記號。
   ```
   POST https://iam.bluemix.net/identity/token
   ```
   {: codeblock}

   <table summary="取得 IAM ID 及 IAM 存取記號的輸入參數，第 1 欄是輸入參數，而第 2 欄是值。">
   <caption>取得 IAM ID 及 IAM 存取記號的輸入參數。</caption>
   <thead>
   <th>輸入參數</th>
   <th>值</th>
   </thead>
   <tbody>
   <tr>
   <td>Header</td>
   <td><ul><li>`Content-Type: application/x-www-form-urlencoded`</li> <li>`Authorization: Basic a3ViZTprdWJl`</br> <strong>附註</strong>：<code>a3ViZTprdWJl</code> 等於使用者名稱 <strong><code>kube</code></strong> 及密碼 <strong><code>kube</code></strong> 的 URL 編碼授權。</li><li>`cache-control: no-cache`</li></ul>
   </td>
   </tr>
   <tr>
   <td>內文</td>
   <td><ul><li>`refresh_token`：您的 {{site.data.keyword.Bluemix_notm}} IAM 委派的重新整理記號。</li>
   <li>`grant_type: urn:ibm:params:oauth:grant-type:delegated-refresh-token`</li></ul></td>
   </tr>
   </tbody>
   </table>

   輸出範例：
   ```
   {
    "access_token": "<iam_access_token>",
    "id_token": "<iam_id_token>",
    "refresh_token": "<iam_refresh_token>",
    "token_type": "Bearer",
    "expires_in": 3600,
    "expiration": 1553629664,
    "scope": "ibm openid containers-kubernetes"
   }
   ```
   {: screen}

4. 使用 IAM 存取記號、IAM ID 記號、IAM 重新整理記號以及您叢集所在的 {{site.data.keyword.containerlong_notm}} 地區，來擷取 Kubernetes 主節點的公用 URL。您可以在 API 輸出的 **`publicServiceEndpointURL`** 中找到 URL。
   ```
   GET https://containers.cloud.ibm.com/v1/clusters/<cluster_ID>
   ```
   {: codeblock}

   <table summary="取得 Kubernetes 主節點的公用服務端點的輸入參數，第 1 欄是輸入參數，而第 2 欄是值。">
   <caption>取得 Kubernetes 主節點的公用服務端點的輸入參數。</caption>
   <thead>
   <th>輸入參數</th>
   <th>值</th>
   </thead>
   <tbody>
   <tr>
   <td>Header</td>
     <td><ul><li>`Authorization`：您的 {{site.data.keyword.Bluemix_notm}} IAM 存取記號。</li><li>`X-Auth-Refresh-Token`：您的 {{site.data.keyword.Bluemix_notm}} IAM 重新整理記號。</li><li>`X-Region`：[使用 API 自動化叢集部署](#cs_api)中使用 `GET https://containers.cloud.ibm.com/v1/clusters` API 所擷取之叢集的 {{site.data.keyword.containerlong_notm}} 地區。</li></ul>
   </td>
   </tr>
   <tr>
   <td>Path</td>
   <td>`<cluster_ID>：`[使用 API 自動化叢集部署](#cs_api)中使用 `GET https://containers.cloud.ibm.com/v1/clusters` API 所擷取之叢集的 ID。</td>
   </tr>
   </tbody>
   </table>

   輸出範例：
   ```
   {
    "location": "Dallas",
    "dataCenter": "dal10",
    "multiAzCapable": true,
    "vlans": null,
    "worker_vlans": null,
    "workerZones": [
        "dal10"
    ],
    "id": "1abc123b123b124ab1234a1a12a12a12",
    "name": "mycluster",
    "region": "us-south",
    ...
    "publicServiceEndpointURL": "https://c7.us-south.containers.cloud.ibm.com:27078"
   }
   ```
   {: screen}

5. 使用您稍早擷取的 IAM ID 記號，針對您的叢集執行 Kubernetes API 要求。例如，列出叢集裡執行的 Kubernetes 版本。

   如果您已在 API 測試架構中啟用 SSL 憑證驗證，則請務必停用此特性。
   {: tip}

   ```
   GET <publicServiceEndpointURL>/version
   ```
   {: codeblock}

   <table summary="檢視叢集裡執行的 Kubernetes 版本的輸入參數，第 1 欄是輸入參數，而第 2 欄是值。">
   <caption>檢視叢集裡執行的 Kubernetes 版本的輸入參數。</caption>
   <thead>
   <th>輸入參數</th>
   <th>值</th>
   </thead>
   <tbody>
   <tr>
   <td>Header</td>
   <td>`Authorization: bearer <id_token>`</td>
   </tr>
   <tr>
   <td>Path</td>
   <td>`<publicServiceEndpointURL>`：您在前一個步驟中擷取的 Kubernetes 主節點的 **`publicServiceEndpointURL`**。</td>
   </tr>
   </tbody>
   </table>

   輸出範例：
   ```
   {
    "major": "1",
    "minor": "13",
    "gitVersion": "v1.13.4+IKS",
    "gitCommit": "c35166bd86eaa91d17af1c08289ffeab3e71e11e",
    "gitTreeState": "clean",
    "buildDate": "2019-03-21T10:08:03Z",
    "goVersion": "go1.11.5",
    "compiler": "gc",
    "platform": "linux/amd64"
   }
   ```
   {: screen}

6. 檢閱 [Kubernetes API 文件 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/reference/kubernetes-api/)，以尋找最新 Kubernetes 版本的受支援 API 清單。請確定使用符合叢集之 Kubernetes 版本的 API 文件。如果您未使用最新的 Kubernetes 版本，則請在 URL 結尾加上您的版本。例如，若要存取 1.12 版的 API 文件，請新增 `v1.12`。


## 使用 API 重新整理 {{site.data.keyword.Bluemix_notm}} IAM 存取記號，並取得新的重新整理記號
{: #cs_api_refresh}

每個透過 API 發出的 {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM) 存取記號都會在一個小時後到期。您必須定期重新整理存取記號，以確保 {{site.data.keyword.Bluemix_notm}} API 的存取權。您可以使用相同的步驟來取得新的重新整理記號。
{:shortdesc}

開始之前，請確定您有可用來要求新存取記號的 {{site.data.keyword.Bluemix_notm}} IAM 重新整理記號或 {{site.data.keyword.Bluemix_notm}} API 金鑰。
- **重新整理記號：**請遵循[使用 {{site.data.keyword.Bluemix_notm}} API 自動化叢集建立及管理處理程序](#cs_api)中的指示。
- **API 金鑰：**如下所示，擷取 [{{site.data.keyword.Bluemix_notm}} ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/) API 金鑰。
   1. 從功能表列，按一下**管理** > **存取權 (IAM)**。
   2. 按一下**使用者**頁面，然後選取自己。
   3. 在 **API 金鑰**窗格中，按一下**建立 IBM Cloud API 金鑰**。
   4. 輸入 API 金鑰的**名稱**和**說明**，然後按一下**建立**。
   4. 按一下**顯示**來查看為您產生的 API 金鑰。
   5. 複製 API 金鑰，您可以用它來擷取新的 {{site.data.keyword.Bluemix_notm}} IAM 存取記號。

如果您想要建立 {{site.data.keyword.Bluemix_notm}} IAM 記號，或想要取得新的重新整理記號，請使用下列步驟。

1.  使用重新整理記號或 {{site.data.keyword.Bluemix_notm}} API 金鑰來產生新的 {{site.data.keyword.Bluemix_notm}} IAM 存取記號。
    ```
    POST https://iam.bluemix.net/identity/token
    ```
    {: codeblock}

    <table summary="新 IAM 記號的輸入參數，第 1 欄是輸入參數，而第 2 欄是值。">
    <caption>新 {{site.data.keyword.Bluemix_notm}} IAM 記號的輸入參數</caption>
    <thead>
    <th>輸入參數</th>
    <th>值</th>
    </thead>
    <tbody>
    <tr>
    <td>Header</td>
    <td><ul><li>`Content-Type: application/x-www-form-urlencoded`</li>
      <li>`Authorization: Basic Yng6Yng=`</br></br><strong>附註：</strong><code>Yng6Yng=</code> 等於使用者名稱 <strong>bx</strong> 及密碼 <strong>bx</strong> 的 URL 編碼授權。</li></ul></td>
    </tr>
    <tr>
    <td>使用重新整理記號時的內文</td>
    <td><ul><li>`grant_type: refresh_token`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`refresh_token:` 您的 {{site.data.keyword.Bluemix_notm}} IAM 重新整理記號。</li>
    <li>`uaa_client_ID: cf`</li>
    <li>`uaa_client_secret:`</li>
    <li>`bss_account:` 您的 {{site.data.keyword.Bluemix_notm}} 帳戶 ID。</li></ul><strong>附註</strong>：新增未指定任何值的 <code>uaa_client_secret</code> 金鑰。</td>
    </tr>
    <tr>
      <td>使用 {{site.data.keyword.Bluemix_notm}} API 金鑰時的內文</td>
      <td><ul><li>`grant_type: urn:ibm:params:oauth:grant-type:apikey`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`apikey:` 您的 {{site.data.keyword.Bluemix_notm}} API 金鑰。</li>
    <li>`uaa_client_ID: cf`</li>
        <li>`uaa_client_secret:`</li></ul><strong>附註</strong>：新增未指定任何值的 <code>uaa_client_secret</code> 金鑰。</td>
    </tr>
    </tbody>
    </table>

    API 輸出範例：

    ```
    {
      "access_token": "<iam_token>",
      "refresh_token": "<iam_refresh_token>",
      "uaa_token": "<uaa_token>",
      "uaa_refresh_token": "<uaa_refresh_token>",
      "token_type": "Bearer",
      "expires_in": 3600,
      "expiration": 1493747503
    }

    ```
    {: screen}

    您可以在 API 輸出的 **access_token** 欄位中找到新的 {{site.data.keyword.Bluemix_notm}} IAM 記號，並在 **refresh_token** 欄位中找到重新整理記號。

2.  使用前一個步驟中的記號，繼續使用 [{{site.data.keyword.containerlong_notm}} API 文件 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://containers.cloud.ibm.com/global/swagger-global-api)。

<br />


## 使用 CLI 重新整理 {{site.data.keyword.Bluemix_notm}} IAM 存取記號，並取得新的重新整理記號
{: #cs_cli_refresh}

啟動新的 CLI 階段作業時，或者如果現行 CLI 階段作業已超過 24 小時，則必須透過執行 `ibmcloud ks cluster-config --cluster <cluster_name>` 來為叢集設定環境定義。當您使用這個指令來設定叢集的環境定義時，會下載 Kubernetes 叢集的 `kubeconfig` 檔案。此外，會發出 {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM) ID 記號及重新整理記號，以提供鑑別。
{: shortdesc}

**ID 記號**：每個透過 CLI 發出的 IAM ID 記號都會在一個小時後到期。當 ID 記號到期時，會將重新整理記號傳送至記號提供者，以重新整理 ID 記號。您的鑑別會重新整理，而且您可以繼續針對叢集執行指令。

**重新整理記號**：重新整理記號每隔 30 天到期。如果重新整理記號過期，則無法重新整理 ID 記號，而且您無法在 CLI 中繼續執行指令。您可以透過執行 `ibmcloud ks cluster-config --cluster <cluster_name>` 來取得新的重新整理記號。這個指令也會重新整理您的 ID 記號。
