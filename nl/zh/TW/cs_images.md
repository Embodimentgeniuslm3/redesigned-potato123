---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-04"

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



# 從映像檔建置容器
{: #images}

Docker 映像檔是您使用 {{site.data.keyword.containerlong}} 建立的每個容器的基礎。
{:shortdesc}

映像檔是從 Dockerfile 所建立的，該 Dockerfile 檔案包含建置映像檔的指示。Dockerfile 可能會參照其指示中個別儲存的建置構件（例如應用程式、應用程式的配置及其相依關係）。


## 規劃映像檔登錄
{: #planning_images}

映像檔通常會儲存在登錄中，而該登錄可供公開存取（公用登錄）或已設定一小組使用者的有限存取（專用登錄）。
{:shortdesc}

公用登錄（例如 Docker Hub）可用來開始使用 Docker 及 Kubernetes 在叢集裡建立第一個容器化應用程式。但是，如果是企業應用程式，則請使用專用登錄（例如 {{site.data.keyword.registryshort_notm}} 中提供的登錄）來防止未授權使用者使用及變更映像檔。叢集管理者必須設定專用登錄，以確保叢集使用者可以使用存取專用登錄的認證。


您可以搭配使用多個登錄與 {{site.data.keyword.containerlong_notm}}，以將應用程式部署至叢集。

|登錄|說明|優點|
|--------|-----------|-------|
|[{{site.data.keyword.registryshort_notm}}](/docs/services/Registry?topic=registry-getting-started#getting-started)|使用此選項，您可以在 {{site.data.keyword.registryshort_notm}} 中設定您自己的安全 Docker 映像檔儲存庫，您可以在其中放心地儲存映像檔並且在叢集使用者之間進行共用。|<ul><li>管理帳戶中的映像檔存取。</li><li>使用 {{site.data.keyword.IBM_notm}} 所提供的映像檔及範例應用程式（例如 {{site.data.keyword.IBM_notm}} Liberty）作為主映像檔，並在其中新增您自己的應用程式碼。</li><li>Vulnerability Advisor 會自動掃描映像檔的潛在漏洞（包括修正它們的 OS 特定建議）。</li></ul>|
|任何其他專用登錄|建立[映像檔取回密碼 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/concepts/containers/images/)，以將任何現有專用登錄連接至叢集。密碼是用來將登錄 URL 及認證安全地儲存在 Kubernetes 密碼中。|<ul><li>使用現有專用登錄，而不管其來源（Docker Hub、組織所擁有的登錄或其他專用 Cloud 登錄）。</li></ul>|
|[公用 Docker Hub![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://hub.docker.com/){: #dockerhub}|使用此選項，即可在不需要 Dockerfile 變更時，於 [Kubernetes 部署 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) 直接使用 Docker Hub 中的現有公用映像檔。<p>**附註：**請記住，此選項可能不符合組織的安全需求（例如存取管理、漏洞掃描或應用程式保密）。</p>|<ul><li>您的叢集不需要其他設定。</li><li>包括各種開放程式碼應用程式。</li></ul>|
{: caption="公用及專用映像檔登錄選項" caption-side="top"}

在您設定映像檔登錄之後，叢集使用者可以使用映像檔，以將應用程式部署至叢集。

進一步瞭解使用容器映像檔時如何[保護個人資訊安全](/docs/containers?topic=containers-security#pi)。

<br />


## 設定容器映像檔的授信內容
{: #trusted_images}

您可以從已簽署並儲存在 {{site.data.keyword.registryshort_notm}} 中的授信映像檔中建置容器，並防止從未簽署或有漏洞的映像檔進行部署。
{:shortdesc}

1.  [簽署映像檔以取得授信內容](/docs/services/Registry?topic=registry-registry_trustedcontent#registry_trustedcontent)。在設定映像檔的信任之後，您可以管理可將映像檔推送至登錄的授信內容及簽章者。
2.  若要施行僅簽署的映像檔可以用來在叢集裡建置容器的原則，請[新增 Container Image Security Enforcement（測試版）](/docs/services/Registry?topic=registry-security_enforce#security_enforce)。
3.  部署應用程式。
    1. [部署至 `default` Kubernetes 名稱空間](#namespace)。
    2. [部署至不同的 Kubernetes 名稱空間，或從不同的 {{site.data.keyword.Bluemix_notm}} 地區或帳戶進行部署](#other)。

<br />


## 將容器從 {{site.data.keyword.registryshort_notm}} 映像檔部署至 `default` Kubernetes 名稱空間
{: #namespace}

您可以將容器從 IBM 提供的公用映像檔或 {{site.data.keyword.registryshort_notm}} 名稱空間中所儲存的專用映像檔部署至叢集裡。如需叢集如何存取登錄映像檔的相關資訊，請參閱[瞭解如何授權叢集以從 {{site.data.keyword.registrylong_notm}} 取回映像檔](#cluster_registry_auth)。
{:shortdesc}

開始之前：
1. [在 {{site.data.keyword.registryshort_notm}} 中設定名稱空間，並將映像檔推送至此名稱空間](/docs/services/Registry?topic=registry-getting-started#gs_registry_namespace_add)。
2. [建立叢集](/docs/containers?topic=containers-clusters#clusters_ui)。
3. 如果您的現有叢集是在 **2019 年 2 月 25 日**之前建立的，請[更新叢集以使用 API 金鑰 `imagePullSecret`](#imagePullSecret_migrate_api_key)。
4. [登入您的帳戶。適用的話，請將適當的資源群組設為目標。設定叢集的環境定義。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

若要將容器部署至叢集的 **default** 名稱空間，請執行下列動作：

1.  建立名稱為 `mydeployment.yaml` 的部署配置檔。
2.  從 {{site.data.keyword.registryshort_notm}} 中的名稱空間，定義要使用的部署及映像檔。

    ```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: <app_name>-deployment
    spec:
      replicas: 3
      selector:
        matchLabels:
          app: <app_name>
      template:
        metadata:
          labels:
            app: <app_name>
        spec:
          containers:
          - name: <app_name>
            image: <region>.icr.io/<namespace>/<my_image>:<tag>
    ```
    {: codeblock}

    將映像檔 URL 變數取代為映像檔的資訊：
    *  **`<app_name>`**：應用程式的名稱。
    *  **`<region>`**：登錄網域的地區 {{site.data.keyword.registryshort_notm}} API 端點。若要列出您所登入之地區的網域，請執行 `ibmcloud cr api`。
    *  **`<namespace>`**：登錄名稱空間。若要取得名稱空間資訊，請執行 `ibmcloud cr namespace-list`。
    *  **`<my_image>:<tag>`**：您要用來建置容器的映像檔及標籤。若要取得登錄中可用的映像檔，請執行 `ibmcloud cr images`。

3.  在叢集裡建立部署。

    ```
    kubectl apply -f mydeployment.yaml
    ```
    {: pre}

<br />


## 瞭解如何授權叢集以從登錄中取回映像檔
{: #cluster_registry_auth}

若要從登錄中取回映像檔，您的 {{site.data.keyword.containerlong_notm}} 叢集會使用特殊類型的 Kubernetes 密碼 `imagePullSecret`。此映像檔取回密碼會儲存用來存取容器登錄的認證。容器登錄可以是您在 {{site.data.keyword.registrylong_notm}} 中的名稱空間、{{site.data.keyword.registrylong_notm}} 中屬於不同 {{site.data.keyword.Bluemix_notm}} 帳戶的名稱空間，或任何其他專用登錄（例如 Docker）。您的叢集設定成從 {{site.data.keyword.registrylong_notm}} 中的名稱空間取回映像檔，並將容器從這些映像檔部署至叢集裡的 `default` Kubernetes 名稱空間。如果您需要取回其他叢集 Kubernetes 名稱空間或其他登錄中的映像檔，則必須設定映像檔取回密碼。
{:shortdesc}

**如何設定我的叢集以從 `default` Kubernetes 名稱空間中取回映像檔？**<br>
當您建立叢集時，叢集具有 {{site.data.keyword.Bluemix_notm}} IAM 服務 ID，而此服務 ID 獲提供 {{site.data.keyword.registrylong_notm}} 的 IAM **Reader** 服務存取角色原則。在叢集的映像檔取回密碼所儲存的未到期 API 金鑰中，模擬服務 ID 認證。映像檔取回密碼會新增至 `default` Kubernetes 名稱空間，以及此名稱空間的 `default` 服務帳戶中的密碼清單。透過使用映像檔取回密碼，部署可以取回（唯讀存取）[廣域及地區登錄](/docs/services/Registry?topic=registry-registry_overview#registry_regions)中的映像檔，以在 `default` Kubernetes 名稱空間中建置容器。全球登錄會安全地儲存公用、IBM 提供的映像檔，您可以在各部署之間參照它們，而不必針對每個地區登錄中儲存的映像檔有不同的參照。地區登錄會安全地儲存您自己的專用 Docker 映像檔。


**是否可以限制對特定地區登錄的取回存取？**<br>
是，您可以[編輯服務 ID 的現有 IAM 原則](/docs/iam?topic=iam-serviceidpolicy#access_edit)，以將 **Reader** 服務存取角色限制為該地區登錄或登錄資源（例如名稱空間）。您必須先[針對 {{site.data.keyword.registrylong_notm}} 啟用 {{site.data.keyword.Bluemix_notm}} IAM 原則](/docs/services/Registry?topic=registry-user#existing_users)，才能自訂登錄 IAM 原則。

  想要讓您的登錄認證更加安全嗎？請要求叢集管理者在您的叢集裡[啟用 {{site.data.keyword.keymanagementservicefull}}](/docs/containers?topic=containers-encryption#keyprotect)，以加密叢集裡的 Kubernetes 密碼，例如儲存登錄認證的 `imagePullSecret`。
  {: tip}

**是否可以取回 `default` 以外的 Kubernetes 名稱空間中的映像檔？**<br>
依預設，不可以。使用預設叢集設定，您可以將容器從儲存在 {{site.data.keyword.registrylong_notm}} 名稱空間的任何映像檔中部署至叢集的 `default` Kubernetes 名稱空間。若要在其他 Kubernetes 名稱空間或其他 {{site.data.keyword.Bluemix_notm}} 帳戶中使用這些映像檔，[您可以選擇複製或建立自己的映像檔取回密碼](#other)。

**是否可以從不同的 {{site.data.keyword.Bluemix_notm}} 帳戶中取回映像檔？**<br>
是，在您要使用的 {{site.data.keyword.Bluemix_notm}} 帳戶中建立 API 金鑰。然後，在您要從中取回的每個叢集和叢集名稱空間中，建立映像檔取回密碼，用來儲存那些 API 金鑰認證。[請遵循這個使用授權服務 ID API 金鑰的範例](#other_registry_accounts)。

若要使用 Docker 這類非 {{site.data.keyword.Bluemix_notm}} 登錄，請參閱[存取儲存在其他專用登錄中的映像檔](#private_images)。

**服務 ID 需要有 API 金鑰嗎？如果我達到帳戶的服務 ID 限制，會發生什麼情況？**<br>
預設叢集設定會建立一個服務 ID，以將 {{site.data.keyword.Bluemix_notm}} IAM API 金鑰認證儲存在映像檔取回密碼中。不過，您也可以建立個別使用者的 API 金鑰，並將那些認證儲存在映像檔取回密碼中。如果您達到[服務 ID 的 IAM 限制](/docs/iam?topic=iam-iam_limits#iam_limits)，則會建立不含服務 ID 和映像檔取回密碼的叢集，而且依預設無法從 `icr.io` 登錄網域中取回映像檔。您必須[建立自己的映像檔取回密碼](#other_registry_accounts)，但使用個別使用者的 API 金鑰（例如功能 ID），而非 {{site.data.keyword.Bluemix_notm}} IAM 服務 ID。

**我的叢集映像檔取回密碼使用登錄記號。記號是否仍然有效？**<br>

支援但已淘汰先前授權叢集存取 {{site.data.keyword.registrylong_notm}} 的方法，先前的方法是自動建立[記號](/docs/services/Registry?topic=registry-registry_access#registry_tokens)，並將記號儲存在映像檔取回密碼中。
{: deprecated}

記號會授權對已淘汰 `registry.bluemix.net` 登錄網域的存取，而 API 金鑰會授權對 `icr.io` 登錄網域的存取。在從記號到 API 金鑰型鑑別的轉移期間，會建立某個時間的記號和 API 金鑰型映像檔取回密碼。使用記號及 API 金鑰型映像檔取回密碼時，您的叢集可以從 `default` Kubernetes 名稱空間的 `registry.bluemix.net` 或 `icr.io` 網域中取回映像檔。

已淘汰的記號和 `registry.bluemix.net` 網域變成不受支援之前，請更新叢集映像檔取回密碼，以使用 [`default` Kubernetes 名稱空間](#imagePullSecret_migrate_api_key)和您可能使用之[任何其他名稱空間或帳戶](#other)的 API 金鑰方法。然後，更新部署以從 `icr.io` 登錄網域中取回。

**在另一個 Kubernetes 名稱空間中複製或建立映像檔取回密碼之後，即完成操作了嗎？**<br>
還沒完。容器必須有權使用所建立的密碼來取回映像檔。您可以將映像檔取回密碼新增至名稱空間的服務帳戶，或在每次部署時參照密碼。如需指示，請參閱[使用映像檔取回密碼部署容器](/docs/containers?topic=containers-images#use_imagePullSecret)。

<br />


## 更新現有叢集以使用 API 金鑰映像檔取回密碼
{: #imagePullSecret_migrate_api_key}

新的 {{site.data.keyword.containerlong_notm}} 叢集會將 API 金鑰儲存在[映像檔取回密碼中，以授權存取 {{site.data.keyword.registrylong_notm}}](#cluster_registry_auth)。使用這些映像檔取回密碼，您可以從 `icr.io` 登錄網域中所儲存的映像檔部署容器。對於在 **2019 年 2 月 25 日**之前建立的叢集，您必須更新叢集以儲存 API 金鑰，而非映像檔取回密碼中的登錄記號。
{: shortdesc}

**開始之前**：
*   [登入您的帳戶。適用的話，請將適當的資源群組設為目標。設定叢集的環境定義。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
*   確定您具有下列許可權：
    *   {{site.data.keyword.containerlong_notm}} 的 {{site.data.keyword.Bluemix_notm}} IAM **操作員或管理者**平台角色。帳戶擁有者可以執行下列指令來提供角色：
        ```
        ibmcloud iam user-policy-create <your_user_email> --service-name containers-kubernetes --roles Administrator,Operator
        ```
        {: pre}
    *   {{site.data.keyword.registrylong_notm}} 的 {{site.data.keyword.Bluemix_notm}} IAM **Administrator** 平台角色，跨所有地區及資源群組。帳戶擁有者可以執行下列指令來提供角色：
        ```
        ibmcloud iam user-policy-create <your_user_email> --service-name container-registry --roles Administrator
        ```
        {: pre}

**若要更新 `default` Kubernetes 名稱空間中的叢集映像檔取回密碼**，請執行下列動作：
1.  取得叢集 ID。
    ```
    ibmcloud ks clusters
    ```
    {: pre}
2.  執行下列指令以建立叢集的服務 ID、將 {{site.data.keyword.registrylong_notm}} 的 IAM **Reader** 服務角色指派給服務 ID、建立 API 金鑰以模擬服務 ID 認證，並且將 API 金鑰儲存在叢集的 Kubernetes 映像檔取回密碼中。映像檔取回密碼位於 `default` Kubernetes 名稱空間中。
    ```
    ibmcloud ks cluster-pull-secret-apply --cluster <cluster_name_or_ID>
    ```
    {: pre}

    當您執行這個指令時，會起始建立 IAM 認證及映像檔取回密碼，這可能需要一點時間才能完成。在建立映像檔取回密碼之前，您無法部署從 {{site.data.keyword.registrylong_notm}} `icr.io` 網域取回映像檔的容器。
    {: important}

3.  驗證已在叢集裡建立映像檔取回密碼。請注意，每個 {{site.data.keyword.registrylong_notm}} 地區各有一個映像檔取回密碼。
    ```
    kubectl get secrets
    ```
    {: pre}
    輸出範例：
    ```
    default-us-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-uk-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-de-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-au-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-jp-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-icr-io                             kubernetes.io/dockerconfigjson        1         16d
    ```
    {: screen}
4.  更新容器部署，以從 `icr.io` 網域名稱取回映像檔。
5.  選用項目：如果您有防火牆，請確定[容許登錄子網路（您所使用的網域）的出埠網路資料流量](/docs/containers?topic=containers-firewall#firewall_outbound)。

**下一步為何？**
*   若要取回 `default` 以外的 Kubernetes 名稱空間中的映像檔，或從其他 {{site.data.keyword.Bluemix_notm}} 帳戶中取回映像檔，請[複製或建立另一個映像檔取回密碼](/docs/containers?topic=containers-images#other)。
*   若要限制對特定登錄資源（例如，名稱空間或地區）的映像檔取回密碼存取權，請執行下列動作：
    1.  確定[已啟用 {{site.data.keyword.registrylong_notm}} 的 {{site.data.keyword.Bluemix_notm}} IAM 原則](/docs/services/Registry?topic=registry-user#existing_users)。
    2.  [編輯 {{site.data.keyword.Bluemix_notm}} IAM 原則](/docs/iam?topic=iam-serviceidpolicy#access_edit)（針對服務 ID），或[建立另一個映像檔取回密碼](/docs/containers?topic=containers-images#other_registry_accounts)。

<br />


## 使用映像檔取回密碼來存取其他叢集 Kubernetes 名稱空間、其他 {{site.data.keyword.Bluemix_notm}} 帳戶或外部專用登錄
{: #other}

在叢集裡設定您自己的映像檔取回密碼，以將容器部署至 `default` 以外的 Kubernetes 名稱空間、使用其他 {{site.data.keyword.Bluemix_notm}} 帳戶中所儲存的映像檔，或使用外部專用登錄中所儲存的映像檔。此外，您還可以建立自己的映像檔取回密碼來套用 IAM 存取原則，以限制對特定登錄映像檔、名稱空間或動作（例如 `push` 或 `pull`）的許可權。
{:shortdesc}

建立映像檔取回密碼後，容器必須使用該密碼才有權從登錄中取回映像檔。您可以將映像檔取回密碼新增至名稱空間的服務帳戶，或在每次部署時參照密碼。如需指示，請參閱[使用映像檔取回密碼部署容器](/docs/containers?topic=containers-images#use_imagePullSecret)。

映像檔取回密碼僅適用於建立它們的 Kubernetes 名稱空間。請針對您要部署容器的每個名稱空間，重複這些步驟。來自 [DockerHub](#dockerhub) 的映像檔不需要映像檔取回密碼。
{: tip}

開始之前：

1.  [在 {{site.data.keyword.registryshort_notm}} 中設定名稱空間，並將映像檔推送至此名稱空間](/docs/services/Registry?topic=registry-getting-started#gs_registry_namespace_add)。
2.  [建立叢集](/docs/containers?topic=containers-clusters#clusters_ui)。
3.  如果您的現有叢集是在 **2019 年 2 月 25 日**之前建立的，請[更新叢集以使用 API 金鑰映像檔取回密碼](#imagePullSecret_migrate_api_key)。
4.  [登入您的帳戶。適用的話，請將適當的資源群組設為目標。設定叢集的環境定義。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

<br/>
若要使用自己的映像檔取回密碼，請在下列選項之間進行選擇：
- 從 default Kubernetes 名稱空間中[將映像檔取回密碼複製](#copy_imagePullSecret)到叢集裡的其他名稱空間。
- [建立新的 IAM API 金鑰認證，並將其儲存在映像檔取回密碼中](#other_registry_accounts)，以存取其他 {{site.data.keyword.Bluemix_notm}} 帳戶中的映像檔，或是套用 IAM 原則來限制對特定登錄網域或名稱空間的存取權。
- [建立映像檔取回密碼，以存取外部專用登錄中的映像檔](#private_images)。

<br/>
如果您已在名稱空間中建立要在部署中使用的映像檔取回密碼，請參閱[使用已建立的 `imagePullSecret` 來部署容器](#use_imagePullSecret)。

### 複製現有的映像檔取回密碼
{: #copy_imagePullSecret}

您可以將映像檔取回密碼（例如針對 `default` Kubernetes 名稱空間自動建立的映像檔取回密碼）複製到叢集裡的其他名稱空間。如果您要針對此名稱空間使用不同的 {{site.data.keyword.Bluemix_notm}} IAM API 金鑰認證（例如限制對特定名稱空間的存取），或要從其他 {{site.data.keyword.Bluemix_notm}} 帳戶取回映像檔，請改為[建立映像檔取回密碼](#other_registry_accounts)。
{: shortdesc}

1.  列出叢集裡可用的 Kubernetes 名稱空間，或建立要使用的名稱空間。
    ```
    kubectl get namespaces
    ```
    {: pre}

        輸出範例：
    ```
    default          Active    79d
    ibm-cert-store   Active    79d
    ibm-system       Active    79d
    istio-system     Active    34d
    kube-public      Active    79d
    kube-system      Active    79d
    ```
    {: screen}

    若要建立名稱空間，請執行下列指令：
    ```
    kubectl create namespace <namespace_name>
    ```
    {: pre}
2.  列出 {{site.data.keyword.registrylong_notm}} 之 `default` Kubernetes 名稱空間中的現有映像檔取回密碼。
    ```
    kubectl get secrets -n default | grep icr
    ```
    {: pre}
    輸出範例：
    ```
    default-us-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-uk-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-de-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-au-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-jp-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-icr-io                             kubernetes.io/dockerconfigjson        1         16d
    ```
    {: screen}
3.  將每個映像檔取回密碼從 `default` 名稱空間複製到您選擇的名稱空間。新的映像檔取回密碼命名為 `<namespace_name>-icr-<region>-io`。
    ```
    kubectl get secret default-us-icr-io -o yaml | sed 's/default/<new-namespace>/g' | kubectl -n <new-namespace> create -f -
    ```
    {: pre}
    ```
    kubectl get secret default-uk-icr-io -o yaml | sed 's/default/<new-namespace>/g' | kubectl -n <new-namespace> create -f -
    ```
    {: pre}
    ```
    kubectl get secret default-de-icr-io -o yaml | sed 's/default/<new-namespace>/g' | kubectl -n <new-namespace> create -f -
    ```
    {: pre}
    ```
    kubectl get secret default-au-icr-io -o yaml | sed 's/default/<new-namespace>/g' | kubectl -n <new-namespace> create -f -
    ```
    {: pre}
    ```
    kubectl get secret default-jp-icr-io -o yaml | sed 's/default/<new-namespace>/g' | kubectl -n <new-namespace> create -f -
    ```
    {: pre}
    ```
    kubectl get secret default-icr-io -o yaml | sed 's/default/<new-namespace>/g' | kubectl -n <new-namespace> create -f -
    ```
    {: pre}
4.  驗證已順利建立密碼。
    ```
    kubectl get secrets -n <namespace_name>
    ```
    {: pre}
5.  [將映像檔取回密碼新增至 Kubernetes 服務帳戶，讓名稱空間中的任何 Pod 在部署容器時都可以使用映像檔取回密碼](#use_imagePullSecret)。

### 建立具有不同 IAM API 金鑰認證的映像檔取回密碼，進一步控制或存取其他 {{site.data.keyword.Bluemix_notm}} 帳戶中的映像檔
{: #other_registry_accounts}

可以將 {{site.data.keyword.Bluemix_notm}} IAM 存取原則指派給使用者或服務 ID，以將許可權作用對象限制為特定登錄映像檔名稱空間或動作（例如，`push` 或 `pull`）。然後，建立 API 金鑰，並將這些登錄認證儲存在叢集的映像檔取回密碼中。
{: shortdesc}

例如，若要存取其他 {{site.data.keyword.Bluemix_notm}} 帳戶中的映像檔，請建立 API 金鑰，以將使用者或服務 ID 的 {{site.data.keyword.registryshort_notm}} 認證儲存在該帳戶中。然後，在叢集帳戶中，將 API 金鑰認證儲存在每個叢集和叢集名稱空間的映像檔取回密碼中。

下列步驟會建立 API 金鑰，用來儲存 {{site.data.keyword.Bluemix_notm}} IAM 服務 ID 的認證。建議您針對具有 {{site.data.keyword.registryshort_notm}} 之 {{site.data.keyword.Bluemix_notm}} IAM 服務存取原則的使用者 ID 建立 API 金鑰，而不是使用服務 ID。不過，請確定使用者是一個功能 ID，或具有可在使用者離開時讓叢集仍可存取登錄的方案。
{: note}

1.  列出叢集裡可用的 Kubernetes 名稱空間，或建立要使用的名稱空間，而您要在其中從登錄映像檔部署容器。
    ```
    kubectl get namespaces
    ```
    {: pre}

        輸出範例：
    ```
    default          Active    79d
    ibm-cert-store   Active    79d
    ibm-system       Active    79d
    istio-system     Active    34d
    kube-public      Active    79d
    kube-system      Active    79d
    ```
    {: screen}

    若要建立名稱空間，請執行下列指令：
    ```
    kubectl create namespace <namespace_name>
    ```
    {: pre}
2.  建立叢集的 {{site.data.keyword.Bluemix_notm}} IAM 服務 ID，以用於映像檔取回密碼中的 IAM 原則及 API 金鑰認證。請務必提供服務 ID 的說明，以協助您稍後擷取服務 ID，例如包括叢集和名稱空間名稱。
    ```
    ibmcloud iam service-id-create <cluster_name>-<kube_namespace>-id --description "Service ID for IBM Cloud Container Registry in Kubernetes cluster <cluster_name> namespace <kube_namespace>"
    ```
    {: pre}
3.  建立叢集服務 ID 的自訂 {{site.data.keyword.Bluemix_notm}} IAM 原則，以授與 {{site.data.keyword.registryshort_notm}} 的存取權。
    ```
    ibmcloud iam service-policy-create <cluster_service_ID> --roles <service_access_role> --service-name container-registry [--region <IAM_region>] [--resource-type namespace --resource <registry_namespace>]
    ```
    {: pre}
    <table>
    <caption>瞭解這個指令的元件</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解這個指令的元件</th>
    </thead>
    <tbody>
    <tr>
    <td><code><em>&lt;cluster_service_ID&gt;</em></code></td>
    <td>必要。取代為先前針對 Kubernetes 叢集所建立的 `<cluster_name>-<kube_namespace>-id` 服務 ID。</td>
    </tr>
    <tr>
    <td><code>--service-name <em>container-registry</em></code></td>
    <td>必要。輸入 `container-registry`，讓 IAM 原則適用於 {{site.data.keyword.registrylong_notm}}。</td>
    </tr>
    <tr>
    <td><code>--roles <em>&lt;service_access_role&gt;</em></code></td>
    <td>必要。輸入 [{{site.data.keyword.registrylong_notm}} 的服務存取角色](/docs/services/Registry?topic=registry-iam#service_access_roles)，以限定服務 ID 存取權範圍。可能值為 `Reader`、`Writer` 及 `Manager`。</td>
    </tr>
    <tr>
    <td><code>--region <em>&lt;IAM_region&gt;</em></code></td>
    <td>選用。如果您要將存取原則範圍限定為特定 IAM 地區，請以逗點區隔清單輸入地區。可能的值為 `au-syd`、`eu-gb`、`eu-de`、`jp-tok`、`us-south` 及 `global`。</td>
    </tr>
    <tr>
    <td><code>--resource-type <em>namespace</em> --resource <em>&lt;registry_namespace&gt;</em></code></td>
    <td>選用。如果您要限制存取特定 [{{site.data.keyword.registrylong_notm}} 名稱空間](/docs/services/Registry?topic=registry-registry_overview#registry_namespaces)中的映像檔，請輸入 `namespace` 作為資源類型，並指定 `<registry_namespace>`。若要列出登錄名稱空間，請執行 `ibmcloud cr namespaces`。</td>
    </tr>
    </tbody></table>
4.  建立服務 ID 的 API 金鑰。請命名與服務 ID 類似的 API 金鑰，並包括您先前建立的服務 ID：``<cluster_name>-<kube_namespace>-id`。請務必提供 API 金鑰的說明，以協助您稍後擷取該金鑰。
    ```
    ibmcloud iam service-api-key-create <cluster_name>-<kube_namespace>-key <cluster_name>-<kube_namespace>-id --description "API key for service ID <service_id> in Kubernetes cluster <cluster_name> namespace <kube_namespace>"
    ```
    {: pre}
5.  從前一個指令的輸出中擷取 **API 金鑰**值。
    ```
    請保留 API 金鑰！建立之後就無法再擷取它。

    Name          <cluster_name>-<kube_namespace>-key   
    Description   key_for_registry_for_serviceid_for_kubernetes_cluster_multizone_namespace_test   
    Bound To      crn:v1:bluemix:public:iam-identity::a/1bb222bb2b33333ddd3d3333ee4ee444::serviceid:ServiceId-ff55555f-5fff-6666-g6g6-777777h7h7hh   
    Created At    2019-02-01T19:06+0000   
    API Key       i-8i88ii8jjjj9jjj99kkkkkkkkk_k9-llllll11mmm1   
    Locked        false   
    UUID          ApiKey-222nn2n2-o3o3-3o3o-4p44-oo444o44o4o4   
    ```
    {: screen}
6.  建立 Kubernetes 映像檔取回密碼，以將 API 金鑰認證儲存在叢集的名稱空間中。針對每個 `icr.io` 網域、Kubernetes 名稱空間以及您要使用此服務 ID 的 IAM 認證從登錄取回映像檔的叢集，重複此步驟。
    ```
    kubectl --namespace <kubernetes_namespace> create secret docker-registry <secret_name> --docker-server=<registry_URL> --docker-username=iamapikey --docker-password=<api_key_value> --docker-email=<docker_email>
    ```
    {: pre}

    <table>
    <caption>瞭解這個指令的元件</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解這個指令的元件</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace <em>&lt;kubernetes_namespace&gt;</em></code></td>
    <td>必要。指定您用於服務 ID 名稱之叢集的 Kubernetes 名稱空間。</td>
    </tr>
    <tr>
    <td><code><em>&lt;secret_name&gt;</em></code></td>
    <td>必要。輸入映像檔取回密碼的名稱。</td>
    </tr>
    <tr>
    <td><code>--docker-server <em>&lt;registry_URL&gt;</em></code></td>
    <td>必要。設定在其中設定登錄名稱空間的映像檔登錄 URL。可用的登錄網域：<ul>
    <li>亞太地區北部（東京）：`jp.icr.io`</li>
    <li>亞太地區南部（雪梨）：`au.icr.io`</li>
    <li>歐盟中部（法蘭克福）：`de.icr.io`</li>
    <li>英國南部（倫敦）：`uk.icr.io`</li>
    <li>美國南部（達拉斯）：`us.icr.io`</li></ul></td>
    </tr>
    <tr>
    <td><code>--docker-username iamapikey</code></td>
    <td>必要。輸入用來登入專用登錄的使用者名稱。對於 {{site.data.keyword.registryshort_notm}}，使用者名稱會設為 <strong><code>iamapikey</code></strong> 值。</td>
    </tr>
    <tr>
    <td><code>--docker-password <em>&lt;token_value&gt;</em></code></td>
    <td>必要。輸入您先前擷取的 **API 金鑰**值。</td>
    </tr>
    <tr>
    <td><code>--docker-email <em>&lt;docker-email&gt;</em></code></td>
    <td>必要。如果您有 Docker 電子郵件位址，請輸入它。否則，請輸入虛構的電子郵件位址，例如 `a@b.c`。需要有此電子郵件才能建立 Kubernetes 密碼，但在建立之後就不再使用。</td>
    </tr>
    </tbody></table>
7.  驗證已順利建立 Secret。
    將 <em>&lt;kubernetes_namespace&gt;</em> 取代為已在其中建立映像檔取回密碼的名稱空間。

    ```
    kubectl get secrets --namespace <kubernetes_namespace>
    ```
    {: pre}
8.  [將映像檔取回密碼新增至 Kubernetes 服務帳戶，讓名稱空間中的任何 Pod 在部署容器時都可以使用映像檔取回密碼](#use_imagePullSecret)。

### 存取儲存在其他專用登錄中的映像檔
{: #private_images}

如果您已有專用登錄，則必須將登錄認證儲存在 Kubernetes 映像檔取回密碼中，然後從配置檔參照此密碼。
{:shortdesc}

開始之前：

1.  [建立叢集](/docs/containers?topic=containers-clusters#clusters_ui)。
2.  [將 CLI 的目標設為叢集](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)。

若要建立映像檔取回密碼，請執行下列動作：

1.  建立用來儲存專用登錄認證的 Kubernetes 密碼。

    ```
    kubectl --namespace <kubernetes_namespace> create secret docker-registry <secret_name>  --docker-server=<registry_URL> --docker-username=<docker_username> --docker-password=<docker_password> --docker-email=<docker_email>
    ```
    {: pre}

    <table>
    <caption>瞭解這個指令的元件</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解這個指令的元件</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace <em>&lt;kubernetes_namespace&gt;</em></code></td>
    <td>必要。您要使用密碼並在其中部署容器之叢集的 Kubernetes 名稱空間。執行 <code>kubectl get namespaces</code>，以列出叢集裡的所有名稱空間。</td>
    </tr>
    <tr>
    <td><code><em>&lt;secret_name&gt;</em></code></td>
    <td>必要。您要用於 <code>imagePullSecret</code> 的名稱。</td>
    </tr>
    <tr>
    <td><code>--docker-server <em>&lt;registry_URL&gt;</em></code></td>
    <td>必要。儲存專用映像檔的登錄的 URL。</td>
    </tr>
    <tr>
    <td><code>--docker-username <em>&lt;docker_username&gt;</em></code></td>
    <td>必要。登入專用登錄的使用者名稱。</td>
    </tr>
    <tr>
    <td><code>--docker-password <em>&lt;token_value&gt;</em></code></td>
    <td>必要。先前所擷取的登錄記號值。</td>
    </tr>
    <tr>
    <td><code>--docker-email <em>&lt;docker-email&gt;</em></code></td>
    <td>必要。如果您有 Docker 電子郵件位址，請輸入它。否則，請輸入虛構的電子郵件位址，例如 `a@b.c`。需要有此電子郵件才能建立 Kubernetes 密碼，但在建立之後就不再使用。</td>
    </tr>
    </tbody></table>

2.  驗證已順利建立 Secret。
    將 <em>&lt;kubernetes_namespace&gt;</em> 取代為已在其中建立 `imagePullSecret` 的名稱空間名稱。

    ```
kubectl get secrets --namespace <kubernetes_namespace>
    ```
    {: pre}

3.  [建立一個參照映像檔取回密碼的 Pod](#use_imagePullSecret)。

<br />


## 使用映像檔取回密碼部署容器
{: #use_imagePullSecret}

您可以在 Pod 部署中定義映像檔取回密碼，或將映像檔取回密碼儲存在 Kubernetes 服務帳戶中，使其可用於未指定服務帳戶的所有部署。
{: shortdesc}

請從下列選項中進行選擇：
* [在 Pod 部署中參照映像檔取回密碼](#pod_imagePullSecret)：如果您不想要依預設對名稱空間中的所有 Pod 授與登錄存取權，請使用此選項。
* [在 Kubernetes 服務帳戶中儲存映像檔取回密碼](#store_imagePullSecret)：使用此選項，可以為所選取 Kubernetes 名稱空間中的所有部署授與對您登錄中映像檔的存取權。

開始之前：
* [建立映像檔取回密碼](#other)，以存取其他登錄或 `default` 以外之 Kubernetes 名稱空間中的映像檔。
* [將 CLI 的目標設為叢集](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)。

### 在 Pod 部署中參照映像檔取回密碼
{: #pod_imagePullSecret}

當您在 Pod 部署中參照映像檔取回密碼時，映像檔取回密碼僅適用於此 Pod，且無法跨名稱空間中的 Pod 共用。
{:shortdesc}

1.  建立名稱為 `mypod.yaml` 的 Pod 配置檔。
2.  定義 Pod 及映像檔取回密碼，以存取 {{site.data.keyword.registrylong_notm}} 中的映像檔。

    若要存取專用映像檔，請執行下列程式碼：
    ```
    apiVersion: v1
    kind: Pod
    metadata:
      name: mypod
    spec:
      containers:
        - name: <container_name>
          image: <region>.icr.io/<namespace_name>/<image_name>:<tag>
      imagePullSecrets:
        - name: <secret_name>
    ```
    {: codeblock}

    若要存取 {{site.data.keyword.Bluemix_notm}} 公用映像檔，請執行下列程式碼︰
    ```
    apiVersion: v1
    kind: Pod
    metadata:
      name: mypod
    spec:
      containers:
        - name: <container_name>
          image: icr.io/<image_name>:<tag>
      imagePullSecrets:
        - name: <secret_name>
    ```
    {: codeblock}

    <table>
    <caption>瞭解 YAML 檔案元件</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解 YAML 檔案元件</th>
    </thead>
    <tbody>
    <tr>
    <td><code><em>&lt;container_name&gt;</em></code></td>
    <td>要部署至叢集的容器名稱。</td>
    </tr>
    <tr>
    <td><code><em>&lt;namespace_name&gt;</em></code></td>
    <td>在其中儲存映像檔的名稱空間。若要列出可用的名稱空間，請執行 `ibmcloud cr namespace-list`。</td>
    </tr>
    <tr>
    <td><code><em>&lt;image_name&gt;</em></code></td>
    <td>要使用的映像檔名稱。若要列出 {{site.data.keyword.Bluemix_notm}} 帳戶中的可用映像檔，請執行 `ibmcloud cr image-list`。</td>
    </tr>
    <tr>
    <td><code><em>&lt;tag&gt;</em></code></td>
    <td>您要使用的映像檔的版本。如果未指定任何標籤，預設會使用標記為<strong>最新</strong>的映像檔。</td>
    </tr>
    <tr>
    <td><code><em>&lt;secret_name&gt;</em></code></td>
    <td>您先前建立的映像檔取回密碼的名稱。</td>
    </tr>
    </tbody></table>

3.  儲存變更。
4.  在叢集裡建立部署。
    ```
   kubectl apply -f mypod.yaml
   ```
    {: pre}

### 在所選取名稱空間的 Kubernetes 服務帳戶中儲存映像檔取回密碼
{:#store_imagePullSecret}

每個名稱空間都有一個稱為 `default` 的 Kubernetes 服務帳戶。您可以將映像檔取回密碼新增至此服務帳戶，以授與對登錄中映像檔的存取權。未指定服務帳戶的部署會自動對此名稱空間使用 `default` 服務帳戶。
{:shortdesc}

1. 檢查預設服務帳戶是否已有映像檔取回密碼。
   ```
   kubectl describe serviceaccount default -n <namespace_name>
   ```
   {: pre}
   **Image pull secrets** 項目中顯示 `<none>` 時，則沒有映像檔取回密碼。  
2. 將映像檔取回密碼新增至預設服務帳戶。
   - **若要在未定義任何映像檔取回密碼時新增映像檔取回密碼，請執行下列指令：**
       ```
       kubectl patch -n <namespace_name> serviceaccount/default -p '{"imagePullSecrets":[{"name": "<image_pull_secret_name>"}]}'
       ```
       {: pre}
   - **若要在已定義映像檔取回密碼時新增映像檔取回密碼，請執行下列指令：**
       ```
       kubectl patch -n <namespace_name> serviceaccount/default --type='json' -p='[{"op":"add","path":"/imagePullSecrets/-","value":{"name":"<image_pull_secret_name>"}}]'
       ```
       {: pre}
3. 驗證映像檔取回密碼已新增至預設服務帳戶。
   ```
   kubectl describe serviceaccount default -n <namespace_name>
   ```
   {: pre}

   輸出範例：
   ```
   Name:                default
   Namespace:           <namespace_name>
   Labels:              <none>
   Annotations:         <none>
   Image pull secrets:  <image_pull_secret_name>
   Mountable secrets:   default-token-sh2dx
   Tokens:              default-token-sh2dx
   Events:              <none>
   ```
   {: pre}

4. 從登錄中的映像檔部署容器。
   ```
   apiVersion: v1
   kind: Pod
   metadata:
     name: mypod
   spec:
     containers:
       - name: <container_name>
         image: <region>.icr.io/<namespace_name>/<image_name>:<tag>
   ```
   {: codeblock}

5. 在叢集裡建立部署。
   ```
   kubectl apply -f mypod.yaml
   ```
   {: pre}

<br />


## 已淘汰：使用登錄記號從 {{site.data.keyword.registrylong_notm}} 映像檔部署容器
{: #namespace_token}

您可以將容器從 IBM 提供的公用映像檔或 {{site.data.keyword.registryshort_notm}} 的名稱空間中所儲存的專用映像檔部署至叢集裡。現有叢集使用叢集 `imagePullSecret` 中所儲存的登錄[記號](/docs/services/Registry?topic=registry-registry_access#registry_tokens)，以授權從 `registry.bluemix.net` 網域名稱中取回映像檔的存取。
{:shortdesc}

當您建立叢集時，會自動為[最近的地區登錄和全球登錄](/docs/services/Registry?topic=registry-registry_overview#registry_regions)建立不會過期的登錄記號與密碼。全球登錄會安全地儲存公用、IBM 提供的映像檔，您可以在各部署之間參照它們，而不必針對每個地區登錄中儲存的映像檔有不同的參照。地區登錄會安全地儲存您自己的專用 Docker 映像檔。記號用來授權對 {{site.data.keyword.registryshort_notm}} 中所設定之任何名稱空間的唯讀存取，因此您可以使用這些公用（全球登錄）和專用（地區登錄）映像檔。

當您部署容器化應用程式時，每個記號必須儲存在 Kubernetes `imagePullSecret` 中，才能供 Kubernetes 叢集存取。建立叢集時，{{site.data.keyword.containerlong_notm}} 會自動將全球（IBM 提供之公用映像檔）與地區登錄的記號儲存在 Kubernetes 映像檔取回密碼中。映像檔取回密碼會新增至 `default` Kubernetes 名稱空間、`kube-system` 名稱空間，以及這些名稱空間的 `default` 服務帳戶中的密碼清單。

`registry.bluemix.net` 網域名稱支援使用記號來授權叢集對 {{site.data.keyword.registrylong_notm}} 的存取權，但是這個方法已淘汰。相反地，請[使用 API 金鑰方法](#cluster_registry_auth)來授權對新 `icr.io` 登錄網域名稱的叢集存取。
{: deprecated}

根據映像檔所在位置及容器所在位置，您必須遵循不同的步驟來部署容器。
*   [使用與叢集位在相同地區的映像檔，將容器部署至 `default` Kubernetes 名稱空間](#token_default_namespace)
*   [將容器部署至 `default` 以外的不同 Kubernetes 名稱空間](#token_copy_imagePullSecret)
*   [使用與叢集位於不同地區或 {{site.data.keyword.Bluemix_notm}} 帳戶的映像檔來部署容器](#token_other_regions_accounts)
*   [使用來自專用之非 IBM 登錄的映像檔來部署容器](#private_images)

使用此起始設定，即可將容器從 {{site.data.keyword.Bluemix_notm}} 帳戶之名稱空間中可用的任何映像檔，部署至叢集的 **default** 名稱空間。若要將容器部署至叢集的其他名稱空間，或者若要使用儲存在另一個 {{site.data.keyword.Bluemix_notm}} 地區或另一個 {{site.data.keyword.Bluemix_notm}} 帳戶中的映像檔，則必須[為叢集建立您自己的映像檔取回密碼](#other)。
{: note}

### 已淘汰：使用登錄記號將映像檔部署至 `default` Kubernetes 名稱空間
{: #token_default_namespace}

使用映像檔取回密碼中所儲存的登錄記號，您可以將容器從地區 {{site.data.keyword.registrylong_notm}} 中可用的任何映像檔部署至叢集的 **default** 名稱空間。
{: shortdesc}

開始之前：
1. [在 {{site.data.keyword.registryshort_notm}} 中設定名稱空間，並將映像檔推送至此名稱空間](/docs/services/Registry?topic=registry-getting-started#gs_registry_namespace_add)。
2. [建立叢集](/docs/containers?topic=containers-clusters#clusters_ui)。
3. [將 CLI 的目標設為叢集](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)。

若要將容器部署至叢集的 **default** 名稱空間，請建立配置檔。

1.  建立名稱為 `mydeployment.yaml` 的部署配置檔。
2.  從 {{site.data.keyword.registryshort_notm}} 中的名稱空間，定義您要使用的部署及映像檔。

    若要使用 {{site.data.keyword.registryshort_notm}} 的名稱空間中的專用映像檔：

    ```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: <app_name>-deployment
    spec:
      replicas: 3
      selector:
        matchLabels:
          app: <app_name>
      template:
        metadata:
          labels:
            app: <app_name>
        spec:
          containers:
          - name: <app_name>
            image: registry.<region>.bluemix.net/<namespace>/<my_image>:<tag>
    ```
    {: codeblock}

    **提示：**若要擷取名稱空間資訊，請執行 `ibmcloud cr namespace-list`。

3.  在叢集裡建立部署。

    ```
    kubectl apply -f mydeployment.yaml
    ```
    {: pre}

    **提示：**您也可以部署現有配置檔，例如其中一個 IBM 提供的公用映像檔。此範例在美國南部地區使用 **ibmliberty** 映像檔。

    ```
    kubectl apply -f https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/deploy-apps-clusters/deploy-ibmliberty.yaml
    ```
    {: pre}

### 已淘汰：將記號型映像檔取回密碼從 default 名稱空間複製到叢集裡的其他名稱空間
{: #token_copy_imagePullSecret}

使用登錄記號認證，您可以將自動針對 `default` Kubernetes 名稱空間建立的映像檔取回密碼複製到叢集裡的其他名稱空間。
{: shortdesc}

1. 列出叢集裡可用的名稱空間。
   ```
   kubectl get namespaces
   ```
   {: pre}

   輸出範例：
   ```
   default          Active    79d
   ibm-cert-store   Active    79d
   ibm-system       Active    79d
   istio-system     Active    34d
   kube-public      Active    79d
   kube-system      Active    79d
   ```
   {: screen}

2. 選用項目：在叢集裡建立名稱空間。
   ```
   kubectl create namespace <namespace_name>
   ```
   {: pre}

3. 將映像檔取回密碼從 `default` 名稱空間複製到您選擇的名稱空間。新的映像檔取回密碼命名為 `bluemix-<namespace_name>-secret-regional` 和 `bluemix-<namespace_name>-secret-international`。
   ```
   kubectl get secret bluemix-default-secret-regional -o yaml | sed 's/default/<namespace_name>/g' | kubectl -n <namespace_name> create -f -
   ```
   {: pre}

   ```
   kubectl get secret bluemix-default-secret-international -o yaml | sed 's/default/<namespace_name>/g' | kubectl -n <namespace_name> create -f -
   ```
   {: pre}

4.  驗證已順利建立密碼。
    ```
    kubectl get secrets --namespace <namespace_name>
    ```
    {: pre}

5. 在名稱空間中[使用 `imagePullSecret` 來部署容器](#use_imagePullSecret)。


### 已淘汰：建立記號型映像檔取回密碼，以存取其他 {{site.data.keyword.Bluemix_notm}} 地區和帳戶中的映像檔
{: #token_other_regions_accounts}

若要存取其他 {{site.data.keyword.Bluemix_notm}} 地區或帳戶中的映像檔，您必須建立登錄記號，並在映像檔取回密碼中儲存您的認證。
{: shortdesc}

1.  如果您沒有記號，請[為您要存取的登錄建立記號](/docs/services/Registry?topic=registry-registry_access#registry_tokens_create)。
2.  列出 {{site.data.keyword.Bluemix_notm}} 帳戶中的記號。

    ```
    ibmcloud cr token-list
    ```
    {: pre}

3.  記下您要使用的記號 ID。
4.  擷取記號的值。將 <em>&lt;token_ID&gt;</em> 取代為您在前一個步驟中所擷取的記號 ID。

    ```
    ibmcloud cr token-get <token_id>
    ```
    {: pre}

    您的記號值會顯示在 CLI 輸出的**記號**欄位中。

5.  建立用來儲存記號資訊的 Kubernetes 密碼。

    ```
    kubectl --namespace <kubernetes_namespace> create secret docker-registry <secret_name>  --docker-server=<registry_URL> --docker-username=token --docker-password=<token_value> --docker-email=<docker_email>
    ```
    {: pre}

    <table>
    <caption>瞭解這個指令的元件</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解這個指令的元件</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace <em>&lt;kubernetes_namespace&gt;</em></code></td>
    <td>必要。您要使用密碼並在其中部署容器之叢集的 Kubernetes 名稱空間。執行 <code>kubectl get namespaces</code>，以列出叢集裡的所有名稱空間。</td>
    </tr>
    <tr>
    <td><code><em>&lt;secret_name&gt;</em></code></td>
    <td>必要。您要用於映像檔取回密碼的名稱。</td>
    </tr>
    <tr>
    <td><code>--docker-server <em>&lt;registry_URL&gt;</em></code></td>
    <td>必要。已設定名稱空間的映像檔登錄的 URL。<ul><li>對於美國南部及美國東部所設定的名稱空間：<code>registry.ng.bluemix.net</code></li><li>對於英國南部所設定的名稱空間：<code>registry.eu-gb.bluemix.net</code></li><li>對於歐盟中部（法蘭克福）所設定的名稱空間：<code>registry.eu-de.bluemix.net</code></li><li>對於澳洲（雪梨）所設定的名稱空間：<code>registry.au-syd.bluemix.net</code></li></ul></td>
    </tr>
    <tr>
    <td><code>--docker-username <em>&lt;docker_username&gt;</em></code></td>
    <td>必要。登入專用登錄的使用者名稱。對於 {{site.data.keyword.registryshort_notm}}，使用者名稱會設為 <strong><code>token</code></strong> 值。</td>
    </tr>
    <tr>
    <td><code>--docker-password <em>&lt;token_value&gt;</em></code></td>
    <td>必要。先前所擷取的登錄記號值。</td>
    </tr>
    <tr>
    <td><code>--docker-email <em>&lt;docker-email&gt;</em></code></td>
    <td>必要。如果您有 Docker 電子郵件位址，請輸入它。否則，請輸入虛構的電子郵件位址，例如 a@b.c。此電子郵件是建立 Kubernetes 密碼的必要項目，但在建立之後就不再使用。</td>
    </tr>
    </tbody></table>

6.  驗證已順利建立 Secret。
    將 <em>&lt;kubernetes_namespace&gt;</em> 取代為已在其中建立映像檔取回密碼的名稱空間。

    ```
kubectl get secrets --namespace <kubernetes_namespace>
    ```
    {: pre}

7.  在名稱空間中[使用映像檔取回密碼來部署容器](#use_imagePullSecret)。
