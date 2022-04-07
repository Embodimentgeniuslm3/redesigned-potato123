---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-05"

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



# 使用 IBM Cloud 服務連結新增服務
{: #service-binding}

新增 {{site.data.keyword.Bluemix_notm}} 服務以加強您的 Kubernetes 叢集，其在 Watson AI、資料、安全及物聯網 (IoT) 等方面有額外功能。
{:shortdesc}

**哪些類型的服務可以連結至叢集？** </br>
當您將 {{site.data.keyword.Bluemix_notm}} 服務新增至叢集時，可以從已啟用 {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM) 的服務，與根據 Cloud Foundry 的服務，兩者之間進行選擇。已啟用 IAM 的服務提供更精細的存取控制，並可在 {{site.data.keyword.Bluemix_notm}} 資源群組中進行管理。Cloud Foundry 服務必須新增至 Cloud Foundry 組織及空間，且無法新增至資源群組。若要控制對 Cloud Foundry 服務實例的存取，您可以使用 Cloud Foundry 角色。如需已啟用 IAM 的服務及 Cloud Foundry 服務的相關資訊，請參閱[何謂資源？](/docs/resources?topic=resources-resource#resource)。

若要尋找支援的 {{site.data.keyword.Bluemix_notm}} 服務清單，請參閱 [{{site.data.keyword.Bluemix_notm}} 型錄](https://cloud.ibm.com/catalog)。

**何謂 {{site.data.keyword.Bluemix_notm}} 服務連結？**</br>
服務連結是一種快速方式，用來建立 {{site.data.keyword.Bluemix_notm}} 服務的服務認證，並將這些認證儲存在叢集的 Kubernetes 密碼中。若要將服務連結至叢集，您必須先佈建服務實例。然後，您可以使用 `ibmcloud ks cluster-service-bind` [指令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_service_bind)來建立服務認證及 Kubernetes 密碼。在 etcd 中會將 Kubernetes 密碼自動加密，以保護您的資料。

想要讓您的密碼更加安全嗎？請要求叢集管理者在您的叢集裡[啟用 {{site.data.keyword.keymanagementservicefull}}](/docs/containers?topic=containers-encryption#keyprotect)，以加密新的及現有的密碼，例如儲存 {{site.data.keyword.Bluemix_notm}} 服務實例之認證的密碼。
{: tip}

**是否可以使用叢集裡的所有 {{site.data.keyword.Bluemix_notm}} 服務？**</br>
您只能對支援服務金鑰的服務使用服務連結，以自動建立服務認證，並將其儲存在 Kubernetes 密碼中。若要找出支援服務金鑰的服務清單，請參閱[啟用外部應用程式以使用 {{site.data.keyword.Bluemix_notm}} 服務](/docs/resources?topic=resources-externalapp#externalapp)。

不支援服務金鑰的服務通常會提供您可在應用程式中使用的 API。服務連結方法不會自動為您的應用程式設定 API 存取權。請務必檢閱服務的 API 文件，並在應用程式中實作 API 介面。

## 將 IBM Cloud 服務新增至叢集
{: #bind-services}

使用 {{site.data.keyword.Bluemix_notm}} 服務連結來自動建立 {{site.data.keyword.Bluemix_notm}} 服務的服務認證，並將這些認證儲存在 Kubernetes 密碼中。
{: shortdesc}

開始之前：
- 確定您具有下列角色：
    - 您要連結服務的叢集的[**編輯者**或**管理者** {{site.data.keyword.Bluemix_notm}} IAM 平台存取角色](/docs/containers?topic=containers-users#platform)
    - 您要連結服務的 Kubernetes 名稱空間的[**撰寫者**或**管理員** {{site.data.keyword.Bluemix_notm}} IAM 服務角色](/docs/containers?topic=containers-users#platform)
    - 對於 Cloud Foundry 服務：您要佈建服務的空間的[**開發人員** Cloud Foundry 角色](/docs/iam?topic=iam-mngcf#mngcf)
- [登入您的帳戶。適用的話，請將適當的資源群組設為目標。設定叢集的環境定義。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

若要將 {{site.data.keyword.Bluemix_notm}} 服務新增至叢集裡，請執行下列動作：

1. [建立 {{site.data.keyword.Bluemix_notm}} 服務的實例](/docs/resources?topic=resources-externalapp#externalapp)。
    * 部分 {{site.data.keyword.Bluemix_notm}} 服務僅特定地區才有提供。唯有與您的叢集相同的地區中有提供某服務時，您才可以將該服務連結至您的叢集。此外，如果您要在華盛頓特區中建立服務實例，則必須使用 CLI。
    * **對於已啟用 IAM 的服務**：您必須在與叢集相同的資源群組中建立服務實例。只能在一個資源群組中建立一個服務，之後就無法進行變更。

2. 請檢查您所建立的服務類型，並記下服務實例**名稱**。
   - **Cloud Foundry 服務：**
     ```
    ibmcloud service list
    ```
     {: pre}

           輸出範例：
      ```
     name                         service           plan    bound apps   last operation
     <cf_service_instance_name>   <service_name>    spark                create succeeded
     ```
     {: screen}

  - **{{site.data.keyword.Bluemix_notm}}已啟用 IAM 的服務：**
     ```
    ibmcloud resource service-instances
    ```
     {: pre}

           輸出範例：
      ```
     Name                          Location   State    Type               Tags
     <iam_service_instance_name>   <region>   active   service_instance
     ```
     {: screen}

   您也可以在 {{site.data.keyword.Bluemix_notm}} 儀表板中看到不同的服務類型：**Cloud Foundry 服務**及**服務**。

3. 識別您要用來新增服務的叢集名稱空間。
   ```
   kubectl get namespaces
   ```
   {: pre}

4. 將服務連結至叢集以建立服務的服務認證，並將認證儲存在 Kubernetes 密碼中。如果您有現有的服務認證，請使用 `--key` 旗標來指定認證。對於已啟用 IAM 的服務，會自動以**撰寫者**服務存取角色來建立認證，但您可以使用 `--role` 旗標來指定不同的服務存取角色。如果您使用 `--key` 旗標，則不要包括 `--role` 旗標。
    
   ```
    ibmcloud ks cluster-service-bind --cluster <cluster_name_or_ID> --namespace <namespace> --service <service_instance_name> [--key <service_instance_key>] [--role <IAM_service_role>]
    ```
   {: pre}

   成功建立服務認證時，會建立名稱為 `binding-<service_instance_name>` 的 Kubernetes 密碼。  

   輸出範例：
   ```
ibmcloud ks cluster-service-bind --cluster mycluster --namespace mynamespace --service cleardb
    Binding service instance to namespace...
    OK
    Namespace:	     mynamespace
    Secret name:     binding-<service_instance_name>
    ```
   {: screen}

5. 驗證 Kubernetes 密碼中的服務認證。
   1. 取得密碼的詳細資料，並記下**binding** 值。**binding** 值以 base64 編碼，並以 JSON 格式保留服務實例的認證。
       ```
       kubectl get secrets binding-<service_instance_name> --namespace=<namespace> -o yaml
       ```
      {: pre}

      輸出範例：
        ```
      apiVersion: v1
       data:
         binding: <binding>
       kind: Secret
       metadata:
         annotations:
           service-instance-id: 1111aaaa-a1aa-1aa1-1a11-111aa111aa11
           service-key-id: 2b22bb2b-222b-2bb2-2b22-b22222bb2222
         creationTimestamp: 2018-08-07T20:47:14Z
         name: binding-<service_instance_name>
         namespace: <namespace>
         resourceVersion: "6145900"
         selfLink: /api/v1/namespaces/default/secrets/binding-mycloudant
         uid: 33333c33-3c33-33c3-cc33-cc33333333c
       type: Opaque
       ```
      {: screen}

   2. 將連結值解碼。
       ```
       echo "<binding>" | base64 -D
       ```
      {: pre}

      輸出範例：
        ```
      {"apikey":"KL34Ys893284NGJEPFjgrioJ12NElpow","host":"98765aab-9ce1-7tr3-ba87-bfbab6e6d9d6-bluemix.cloudant.com","iam_apikey_description":"Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:cloudantnosqldb:us-south:a/1234g56789cfe8e6388dd2ec098:98746cw-43d7-49ce-947a-d8fe3eebb381::","iam_apikey_name":"auto-generated-apikey-1234abcde-987f-3t64-9d96-d13775ec5663","iam_role_crn":"crn:v1:bluemix:public:iam::::serviceRole:Writer","iam_serviceid_crn":"crn:v1:bluemix:public:iam-identity::a/1234567890brasge5htn2ec098::serviceid:ServiceId-12345vgh-6c4c-ytr12-af6b-467d30d6ef44","password":"jfiavhui12484fnivhuo472nvei23913c3ff","port":443,"url":"https://25c73aac-9ce1-4c24-ba98-bfbab6e6d9d6-bluemix:ugvioev823inreuiegn43donvri29989wiu9t22@25c73aac-9ce1-4c24-ba98-abdrjio123562lnsb-bluemix.cloudant.com","username":"123b45da-9ce1-4c24-ab12-rinwnwub1294-bluemix"}
       ```
      {: screen}

   3. 選用項目：將您在前一個步驟中所解碼的服務認證與您在 {{site.data.keyword.Bluemix_notm}} 儀表板中針對服務實例所找到的服務認證進行比較。

6. 既然您的服務已連結至叢集，您必須配置應用程式[存取 Kubernetes 密碼中的服務認證](#adding_app)。


## 從應用程式存取服務認證
{: #adding_app}

若要從應用程式存取 {{site.data.keyword.Bluemix_notm}} 服務實例，您必須把儲存在 Kubernetes 密碼中的服務認證提供給應用程式。
{: shortdesc}

服務實例的認證會以 base64 編碼，並以 JSON 格式儲存在您的密碼內。若要存取您密碼中的資料，請在下列選項之間進行選擇：
- [以磁區將密碼裝載至您的 Pod](#mount_secret)
- [在環境變數中參照該密碼](#reference_secret)
<br>

開始之前：
-  確定您具有 `kube-system` 名稱空間的[**撰寫者**或**管理員** {{site.data.keyword.Bluemix_notm}} IAM 服務角色](/docs/containers?topic=containers-users#platform)。
- [登入您的帳戶。適用的話，請將適當的資源群組設為目標。設定叢集的環境定義。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
- [將 {{site.data.keyword.Bluemix_notm}} 服務新增至叢集](#bind-services)。

### 以磁區將密碼裝載至您的 Pod
{: #mount_secret}

以磁區將密碼裝載至 Pod 時，名稱為 `binding` 的檔案會儲存在磁區裝載目錄中。JSON 格式的 `binding` 檔案包含存取 {{site.data.keyword.Bluemix_notm}} 服務所需的所有資訊和認證。
{: shortdesc}

1.  列出叢集裡的可用密碼，並記下您密碼的**名稱**。尋找類型為 **Opaque** 的密碼。如果有多個密碼，請與叢集管理者聯絡，以識別正確的服務密碼。
    ```
    kubectl get secrets
    ```
    {: pre}

    輸出範例：
    ```
    NAME                              TYPE            DATA      AGE
    binding-<service_instance_name>   Opaque          1         3m
    ```
    {: screen}

2.  為您的 Kubernetes 部署建立 YAML 檔案，並以磁區將該密碼裝載至您的 Pod。
    ```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      labels:
        app: secret-test
      name: secret-test
      namespace: <my_namespace>
    spec:
      selector:
        matchLabels:
          app: secret-test
      replicas: 1
      template:
        metadata:
          labels:
            app: secret-test
        spec:
          containers:
          - image: icr.io/ibmliberty:latest
            name: secret-test
            volumeMounts:
            - mountPath: <mount_path>
              name: <volume_name>
          volumes:
          - name: <volume_name>
            secret:
              defaultMode: 420
              secretName: binding-<service_instance_name>
    ```
    {: codeblock}

    <table>
    <caption>瞭解 YAML 檔案元件</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解 YAML 檔案元件</th>
    </thead>
    <tbody>
    <tr>
    <td><code>volumeMounts.mountPath</code></td>
    <td>容器內裝載磁區之目錄的絕對路徑。</td>
    </tr>
    <tr>
    <td><code>volumeMounts.name</code></br><code>volumes.name</code></td>
    <td>要裝載至 Pod 之磁區的名稱。</td>
    </tr>
    <tr>
    <td><code>secret.defaultMode</code></td>
    <td>密碼的讀取權和寫入權。請使用 `420` 來設定唯讀許可權。</td>
    </tr>
    <tr>
    <td><code>secret.secretName</code></td>
    <td>您在前一個步驟記下的密碼名稱。</td>
    </tr></tbody></table>

3.  建立 Pod，並以磁區裝載密碼。
    ```
    kubectl apply -f secret-test.yaml
    ```
    {: pre}

4.  驗證已建立 Pod。
    ```
    kubectl get pods
    ```
    {: pre}

    CLI 輸出範例：

    ```
    NAME                           READY     STATUS    RESTARTS   AGE
    secret-test-1111454598-gfx32   1/1       Running   0          1m
    ```
    {: screen}

5.  存取服務認證。
    1. 登入 Pod。
       ```
       kubectl exec <pod_name> -it bash
       ```
       {: pre}

    2. 導覽至您稍早定義的磁區裝載路徑，並列出磁區裝載路徑中的檔案。
       ```
       cd <volume_mountpath> && ls
       ```
       {: pre}

       輸出範例：
       ```
       binding
       ```
       {: screen}

       `binding` 檔案包含您儲存在 Kubernetes 密碼中的服務認證。

    4. 檢視服務認證。認證會以 JSON 格式儲存為鍵值組。
       ```
       cat binding
       ```
       {: pre}

       輸出範例：
       ```
       {"apikey":"KL34Ys893284NGJEPFjgrioJ12NElpow","host":"98765aab-9ce1-7tr3-ba87-bfbab6e6d9d6-bluemix.cloudant.com","iam_apikey_description":"Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:cloudantnosqldb:us-south:a/1234g56789cfe8e6388dd2ec098:98746cw-43d7-49ce-947a-d8fe3eebb381::","iam_apikey_name":"auto-generated-apikey-1234abcde-987f-3t64-9d96-d13775ec5663","iam_role_crn":"crn:v1:bluemix:public:iam::::serviceRole:Writer","iam_serviceid_crn":"crn:v1:bluemix:public:iam-identity::a/1234567890brasge5htn2ec098::serviceid:ServiceId-12345vgh-6c4c-ytr12-af6b-467d30d6ef44","password":"jfiavhui12484fnivhuo472nvei23913c3ff","port":443,"url":"https://25c73aac-9ce1-4c24-ba98-bfbab6e6d9d6-bluemix:ugvioev823inreuiegn43donvri29989wiu9t22@25c73aac-9ce1-4c24-ba98-abdrjio123562lnsb-bluemix.cloudant.com","username":"123b45da-9ce1-4c24-ab12-rinwnwub1294-bluemix"}
       ```
       {: screen}

    5. 配置應用程式以剖析 JSON 內容，並擷取您存取服務所需的資訊。

### 在環境變數中參照該密碼
{: #reference_secret}

您可以將 Kubernetes 密碼中的服務認證及其他鍵值組當成環境變數新增至部署中。
{: shortdesc}

1. 列出叢集裡的可用密碼，並記下您密碼的**名稱**。尋找類型為 **Opaque** 的密碼。如果有多個密碼，請與叢集管理者聯絡，以識別正確的服務密碼。
   ```
    kubectl get secrets
    ```
   {: pre}

   輸出範例：
   ```
    NAME                              TYPE            DATA      AGE
    binding-<service_instance_name>   Opaque          1         3m
    ```
   {: screen}

2. 取得您密碼的詳細資料，以尋找您可以在 Pod 中作為環境變數參照的可能鍵值組。服務認證儲存在您密碼的 `binding` 索引鍵中。
   ```
   kubectl get secrets binding-<service_instance_name> --namespace=<namespace> -o yaml
   ```
   {: pre}

   輸出範例：
   ```
   apiVersion: v1
   data:
     binding: <binding>
   kind: Secret
   metadata:
     annotations:
       service-instance-id: 7123acde-c3ef-4ba2-8c52-439ac007fa70
       service-key-id: 9h30dh8a-023f-4cf4-9d96-d12345ec7890
     creationTimestamp: 2018-08-07T20:47:14Z
     name: binding-<service_instance_name>
     namespace: <namespace>
     resourceVersion: "6145900"
     selfLink: /api/v1/namespaces/default/secrets/binding-mycloudant
     uid: 12345a31-9a83-11e8-ba83-cd49014748f
   type: Opaque
   ```
   {: screen}

3. 為您的 Kubernetes 部署建立 YAML 檔案，並指定參照 `binding` 索引鍵的環境變數。
   ```
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     labels:
       app: secret-test
     name: secret-test
     namespace: <my_namespace>
   spec:
     selector:
       matchLabels:
         app: secret-test
     template:
       metadata:
         labels:
           app: secret-test
       spec:
         containers:
         - image: icr.io/ibmliberty:latest
           name: secret-test
           env:
           - name: BINDING
             valueFrom:
               secretKeyRef:
                 name: binding-<service_instance_name>
                 key: binding
     ```
     {: codeblock}

     <table>
     <caption>瞭解 YAML 檔案元件</caption>
     <thead>
     <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解 YAML 檔案元件</th>
     </thead>
     <tbody>
     <tr>
     <td><code>containers.env.name</code></td>
     <td>環境變數的名稱。</td>
     </tr>
     <tr>
     <td><code>env.valueFrom.secretKeyRef.name</code></td>
     <td>您在前一個步驟記下的密碼名稱。</td>
     </tr>
     <tr>
     <td><code>env.valueFrom.secretKeyRef.key</code></td>
     <td>此索引鍵為您密碼的一部分，您想在環境變數中參照它。若要參照服務認證，您必須使用 <strong>binding</strong> 索引鍵。</td>
     </tr>
     </tbody></table>

4. 建立 Pod，其以環境變數參照您密碼的 `binding` 索引鍵。
   ```
    kubectl apply -f secret-test.yaml
    ```
   {: pre}

5. 驗證已建立 Pod。
   ```
   kubectl get pods
   ```
   {: pre}

   CLI 輸出範例：
   ```
    NAME                           READY     STATUS    RESTARTS   AGE
    secret-test-1111454598-gfx32   1/1       Running   0          1m
    ```
   {: screen}

6. 驗證是否已正確設定環境變數。
   1. 登入 Pod。
      ```
      kubectl exec <pod_name> -it bash
      ```
      {: pre}

   2. 列出 Pod 中的所有環境變數。
      ```
      env
      ```
      {: pre}

      輸出範例：
        ```
      BINDING={"apikey":"KL34Ys893284NGJEPFjgrioJ12NElpow","host":"98765aab-9ce1-7tr3-ba87-bfbab6e6d9d6-bluemix.cloudant.com","iam_apikey_description":"Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:cloudantnosqldb:us-south:a/1234g56789cfe8e6388dd2ec098:98746cw-43d7-49ce-947a-d8fe3eebb381::","iam_apikey_name":"auto-generated-apikey-1234abcde-987f-3t64-9d96-d13775ec5663","iam_role_crn":"crn:v1:bluemix:public:iam::::serviceRole:Writer","iam_serviceid_crn":"crn:v1:bluemix:public:iam-identity::a/1234567890brasge5htn2ec098::serviceid:ServiceId-12345vgh-6c4c-ytr12-af6b-467d30d6ef44","password":"jfiavhui12484fnivhuo472nvei23913c3ff","port":443,"url":"https://25c73aac-9ce1-4c24-ba98-bfbab6e6d9d6-bluemix:ugvioev823inreuiegn43donvri29989wiu9t22@25c73aac-9ce1-4c24-ba98-abdrjio123562lnsb-bluemix.cloudant.com","username":"123b45da-9ce1-4c24-ab12-rinwnwub1294-bluemix"}
      ```
      {: screen}

7. 配置應用程式來讀取環境變數並剖析 JSON 內容，以擷取您存取該服務所需的資訊。

   Python 程式碼範例：
   ```python
   if os.environ.get('BINDING'):
        credentials = json.loads(os.environ.get('BINDING'))
   ```
   {: codeblock}

8. 選用項目：為了小心起見，若未適當設定 `BINDING` 環境變數，請對應用程式新增錯誤處理。

   Java 程式碼範例：
   ```java
   if (System.getenv("BINDING") == null) {
    throw new RuntimeException("Environment variable 'SECRET' is not set!");
   }
   ```
   {: codeblock}

   Node.js 程式碼範例：
   ```js
   if (!process.env.BINDING) {
    console.error('ENVIRONMENT variable "BINDING" is not set!');
    process.exit(1);
   }
   ```
   {: codeblock}
