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



# 使用 IBM Cloud 服务绑定添加服务
{: #service-binding}

添加 {{site.data.keyword.Bluemix_notm}} 服务以使用区域中的额外功能增强 Kubernetes 集群，例如，Watson AI、数据、安全性和物联网 (IoT)。
{:shortdesc}

**可以将哪些类型的服务绑定到集群？**</br>
将 {{site.data.keyword.Bluemix_notm}} 服务添加到集群时，可以在启用了 {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM) 的服务和基于 Cloud Foundry 的服务之间进行选择。启用 IAM 的服务提供更细颗粒度的访问控制，并且可以在 {{site.data.keyword.Bluemix_notm}} 资源组中进行管理。Cloud Foundry 服务必须添加到 Cloud Foundry 组织和空间，并且无法添加到资源组。要控制对 Cloud Foundry 服务实例的访问权，可使用 Cloud Foundry 角色。有关启用 IAM 的服务和 Cloud Foundry 服务的更多信息，请参阅[什么是资源？](/docs/resources?topic=resources-resource#resource)。

要查找受支持 {{site.data.keyword.Bluemix_notm}} 服务的列表，请参阅 [{{site.data.keyword.Bluemix_notm}} 目录](https://cloud.ibm.com/catalog)。

**什么是 {{site.data.keyword.Bluemix_notm}} 服务绑定？**</br>
通过服务绑定，可以快速为 {{site.data.keyword.Bluemix_notm}} 服务创建服务凭证，并将这些凭证存储在集群的 Kubernetes 私钥中。要将服务绑定到集群，必须首先供应服务的实例。然后，使用 `ibmcloud ks cluster-service-bind` [命令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_service_bind)来创建服务凭证和 Kubernetes 私钥。Kubernetes 私钥在 etcd 中会自动加密，以保护您的数据。

想要使私钥更安全吗？请要求集群管理员在集群中[启用 {{site.data.keyword.keymanagementservicefull}}](/docs/containers?topic=containers-encryption#keyprotect)，以加密新私钥和现有私钥，例如用于存储 {{site.data.keyword.Bluemix_notm}} 服务实例凭证的私钥。
{: tip}

**可以将所有 {{site.data.keyword.Bluemix_notm}} 服务用于集群吗？**</br>
只能将服务绑定用于支持服务密钥的服务，这样才能自动创建服务凭证，并将其存储在 Kubernetes 私钥中。要查找支持服务密钥的服务的列表，请参阅[使外部应用程序能够使用 {{site.data.keyword.Bluemix_notm}} 服务](/docs/resources?topic=resources-externalapp#externalapp)。

不支持服务密钥的服务通常会提供可在应用程序中使用的 API。服务绑定方法不会自动为应用程序设置 API 访问权。请确保查看服务的 API 文档，并在应用程序中实现 API 接口。

## 向集群添加 IBM Cloud 服务
{: #bind-services}

使用 {{site.data.keyword.Bluemix_notm}} 服务绑定可自动为 {{site.data.keyword.Bluemix_notm}} 服务创建服务凭证，并将这些凭证存储在 Kubernetes 私钥中。
{: shortdesc}

开始之前：
- 确保您具有以下角色：
    - 对要绑定服务的集群的 [{{site.data.keyword.Bluemix_notm}} IAM **编辑者**或**管理员**平台访问角色](/docs/containers?topic=containers-users#platform)
    - 对要绑定服务的 Kubernetes 名称空间的 [{{site.data.keyword.Bluemix_notm}} IAM **写入者**或**管理者**服务角色](/docs/containers?topic=containers-users#platform)
    - 对于 Cloud Foundry 服务：对要供应服务的空间的 [Cloud Foundry **开发者**角色](/docs/iam?topic=iam-mngcf#mngcf)
- [登录到您的帐户。如果适用，请将相应的资源组设定为目标。为集群设置上下文。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

要将 {{site.data.keyword.Bluemix_notm}} 服务添加到集群：

1. [创建 {{site.data.keyword.Bluemix_notm}} 服务的实例](/docs/resources?topic=resources-externalapp#externalapp)。
    * 某些 {{site.data.keyword.Bluemix_notm}} 服务仅在精选区域中可用。仅当服务在与您的集群相同的区域中可用时，才可将服务绑定到集群。此外，如果想要在华盛顿专区中创建服务实例，必须使用 CLI。
    * **对于启用 IAM 的服务**：必须在集群所在的资源组中创建服务实例。一个服务只能在一个资源组中进行创建，并且在此之后无法进行更改。

2. 检查创建的服务类型，并记下服务实例**名称**。
   - **Cloud Foundry 服务：**
     ```
    ibmcloud service list
    ```
     {: pre}

     输出示例：
     ```
     name                         service           plan    bound apps   last operation
     <cf_service_instance_name>   <service_name>    spark                create succeeded
     ```
     {: screen}

  - **{{site.data.keyword.Bluemix_notm}}支持 IAM 的服务：**
     ```
    ibmcloud resource service-instances
    ```
     {: pre}

     输出示例：
     ```
     Name                          Location   State    Type               Tags
     <iam_service_instance_name>   <region>   active   service_instance
     ```
     {: screen}

   您还可以在 {{site.data.keyword.Bluemix_notm}}“仪表板”中查看作为 **Cloud Foundry 服务**和**服务**的不同服务类型。

3. 确定要用于添加服务的集群名称空间。
   ```
        kubectl get namespaces
        ```
   {: pre}

4. 将服务绑定到集群以便为服务创建服务凭证，并将凭证存储在 Kubernetes 私钥中。如果您具有现有服务凭证，请使用 `--key` 标志来指定凭证。对于启用 IAM 的服务，将使用**写入者**服务访问角色来自动创建凭证，但可以使用 `--role` 标志来指定其他服务访问角色。如果使用 `--key` 标志，请不要包含 `--role` 标志。
    
   ```
    ibmcloud ks cluster-service-bind --cluster <cluster_name_or_ID> --namespace <namespace> --service <service_instance_name> [--key <service_instance_key>] [--role <IAM_service_role>]
    ```
   {: pre}

   服务凭证创建成功时，将创建名为 `binding-<service_instance_name>` 的 Kubernetes 私钥。  

   输出示例：
   ```
ibmcloud ks cluster-service-bind --cluster mycluster --namespace mynamespace --service cleardb
    Binding service instance to namespace...
    OK
    Namespace:	     mynamespace
    Secret name:     binding-<service_instance_name>
    ```
   {: screen}

5. 验证 Kubernetes 私钥中的服务凭证。
   1. 获取私钥的详细信息并记录 **binding** 值。**binding** 值采用 base64 编码，并且以 JSON 格式保存服务实例的凭证。
       ```
       kubectl get secrets binding-<service_instance_name> --namespace=<namespace> -o yaml
       ```
      {: pre}

      输出示例：
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

   2. 对绑定值进行解码。
       ```
       echo "<binding>" | base64 -D
       ```
      {: pre}

      输出示例：
        ```
      {"apikey":"KL34Ys893284NGJEPFjgrioJ12NElpow","host":"98765aab-9ce1-7tr3-ba87-bfbab6e6d9d6-bluemix.cloudant.com","iam_apikey_description":"Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:cloudantnosqldb:us-south:a/1234g56789cfe8e6388dd2ec098:98746cw-43d7-49ce-947a-d8fe3eebb381::","iam_apikey_name":"auto-generated-apikey-1234abcde-987f-3t64-9d96-d13775ec5663","iam_role_crn":"crn:v1:bluemix:public:iam::::serviceRole:Writer","iam_serviceid_crn":"crn:v1:bluemix:public:iam-identity::a/1234567890brasge5htn2ec098::serviceid:ServiceId-12345vgh-6c4c-ytr12-af6b-467d30d6ef44","password":"jfiavhui12484fnivhuo472nvei23913c3ff","port":443,"url":"https://25c73aac-9ce1-4c24-ba98-bfbab6e6d9d6-bluemix:ugvioev823inreuiegn43donvri29989wiu9t22@25c73aac-9ce1-4c24-ba98-abdrjio123562lnsb-bluemix.cloudant.com","username":"123b45da-9ce1-4c24-ab12-rinwnwub1294-bluemix"}
       ```
      {: screen}

   3. 可选：在 {{site.data.keyword.Bluemix_notm}} 仪表板中，将在先前步骤中解码的服务凭证与针对服务实例找到的服务凭证进行比较。

6. 现在，您的服务已绑定到集群，必须配置应用程序以[访问 Kubernetes 私钥中的服务凭证](#adding_app)。


## 从应用程序访问服务凭证
{: #adding_app}

要从应用程序访问 {{site.data.keyword.Bluemix_notm}} 服务实例，必须使存储在 Kubernetes 私钥中的服务凭证可用于应用程序。
{: shortdesc}

服务实例凭证采用 base64 编码并且以 JSON 格式存储在私钥中。要访问私钥中的数据，请从以下选项中进行选择：
- [将私钥作为卷安装到 pod](#mount_secret)
- [在环境变量中引用私钥](#reference_secret)
<br>

开始之前：
-  确保您具有对 `kube-system` 名称空间的 [{{site.data.keyword.Bluemix_notm}} IAM **写入者**或**管理者**服务角色](/docs/containers?topic=containers-users#platform)。
- [登录到您的帐户。如果适用，请将相应的资源组设定为目标。为集群设置上下文。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
- [向集群添加 {{site.data.keyword.Bluemix_notm}} 服务](#bind-services)。

### 将私钥作为卷安装到 pod
{: #mount_secret}

将私钥作为卷安装到 pod 时，会将名为 `binding` 的文件存储在卷安装目录中。JSON 格式的 `binding` 文件包含访问 {{site.data.keyword.Bluemix_notm}} 服务所需的全部信息和凭证。
{: shortdesc}

1.  列出集群中的可用私钥并记下私钥的 **name**。查找类型为 **Opaque** 的私钥。如果存在多个私钥，请联系集群管理员来确定正确的服务私钥。
    ```
    kubectl get secrets
    ```
    {: pre}

    输出示例：
    ```
        NAME                                    TYPE                                  DATA      AGE
    binding-<service_instance_name>         Opaque                                1         3m

    ```
    {: screen}

2.  针对 Kubernetes 部署创建 YAML 文件并将私钥作为卷安装到 pod。
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
    <caption>了解 YAML 文件的组成部分</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解 YAML 文件的组成部分</th>
    </thead>
    <tbody>
    <tr>
    <td><code>volumeMounts.mountPath</code></td>
    <td>在容器中安装卷的目录的绝对路径。</td>
    </tr>
    <tr>
    <td><code>volumeMounts.name</code></br><code>volumes.name</code></td>
    <td>要安装到 pod 中的卷的名称。</td>
    </tr>
    <tr>
    <td><code>secret.defaultMode</code></td>
    <td>私钥的读写许可权。使用 `420` 可设置只读许可权。</td>
    </tr>
    <tr>
    <td><code>secret.secretName</code></td>
    <td>先前步骤中记录的私钥的名称。</td>
    </tr></tbody></table>

3.  创建 pod 并安装私钥作为卷。
    ```
    kubectl apply -f secret-test.yaml
    ```
    {: pre}

4.  验证 pod 是否已创建。
    ```
    kubectl get pods
    ```
    {: pre}

    示例 CLI 输出：

    ```
    NAME                           READY     STATUS    RESTARTS   AGE
    secret-test-1111454598-gfx32   1/1       Running   0          1m
    ```
    {: screen}

5.  访问服务凭证。
    1. 登录到 pod。
       ```
       kubectl exec <pod_name> -it bash
       ```
       {: pre}

    2. 浏览至先前定义的卷安装路径并列出卷安装路径中的文件。
       ```
       cd <volume_mountpath> && ls
       ```
       {: pre}

       输出示例：
       ```
       binding
       ```
       {: screen}

       `binding` 文件包含存储在 Kubernetes 私钥中的服务凭证。

    4. 查看服务凭证。凭证以 JSON 格式存储为键值对。
       ```
       cat binding
       ```
       {: pre}

       输出示例：
       ```
       {"apikey":"KL34Ys893284NGJEPFjgrioJ12NElpow","host":"98765aab-9ce1-7tr3-ba87-bfbab6e6d9d6-bluemix.cloudant.com","iam_apikey_description":"Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:cloudantnosqldb:us-south:a/1234g56789cfe8e6388dd2ec098:98746cw-43d7-49ce-947a-d8fe3eebb381::","iam_apikey_name":"auto-generated-apikey-1234abcde-987f-3t64-9d96-d13775ec5663","iam_role_crn":"crn:v1:bluemix:public:iam::::serviceRole:Writer","iam_serviceid_crn":"crn:v1:bluemix:public:iam-identity::a/1234567890brasge5htn2ec098::serviceid:ServiceId-12345vgh-6c4c-ytr12-af6b-467d30d6ef44","password":"jfiavhui12484fnivhuo472nvei23913c3ff","port":443,"url":"https://25c73aac-9ce1-4c24-ba98-bfbab6e6d9d6-bluemix:ugvioev823inreuiegn43donvri29989wiu9t22@25c73aac-9ce1-4c24-ba98-abdrjio123562lnsb-bluemix.cloudant.com","username":"123b45da-9ce1-4c24-ab12-rinwnwub1294-bluemix"}
       ```
       {: screen}

    5. 配置应用程序以解析 JSON 内容，并检索访问服务所需的信息。

### 在环境变量中引用私钥
{: #reference_secret}

您可以将来自 Kubernetes 私钥的服务凭证和其他键值对作为环境变量添加到部署中。
{: shortdesc}

1. 列出集群中的可用私钥并记下私钥的 **name**。查找类型为 **Opaque** 的私钥。如果存在多个私钥，请联系集群管理员来确定正确的服务私钥。
   ```
    kubectl get secrets
    ```
   {: pre}

   输出示例：
   ```
    NAME                                    TYPE                                  DATA      AGE
    binding-<service_instance_name>         Opaque                                1         3m

    ```
   {: screen}

2. 获取私钥的详细信息，以查找可以引用为 pod 中的环境变量的潜在键值对。服务凭证存储在私钥的 `binding` 密钥中。
   ```
   kubectl get secrets binding-<service_instance_name> --namespace=<namespace> -o yaml
   ```
   {: pre}

   输出示例：
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

3. 为 Kubernetes 部署创建 YAML 文件，并指定引用 `binding` 密钥的环境变量。
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
     <caption>了解 YAML 文件的组成部分</caption>
     <thead>
     <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解 YAML 文件的组成部分</th>
     </thead>
     <tbody>
     <tr>
     <td><code>containers.env.name</code></td>
     <td>环境变量的名称。</td>
     </tr>
     <tr>
     <td><code>env.valueFrom.secretKeyRef.name</code></td>
     <td>先前步骤中记录的私钥的名称。</td>
     </tr>
     <tr>
     <td><code>env.valueFrom.secretKeyRef.key</code></td>
     <td>属于私钥的一部分并且您想要在环境变量中引用的密钥。要引用服务凭证，必须使用 <strong>binding</strong> 密钥。</td>
     </tr>
     </tbody></table>

4. 创建引用私钥的 `binding` 密钥作为环境变量的 pod。
   ```
    kubectl apply -f secret-test.yaml
    ```
   {: pre}

5. 验证 pod 是否已创建。
   ```
   kubectl get pods
   ```
   {: pre}

   示例 CLI 输出：
   ```
   NAME                           READY     STATUS    RESTARTS   AGE
   secret-test-1111454598-gfx32   1/1       Running   0          1m
   ```
   {: screen}

6. 验证是否已正确设置环境变量。
   1. 登录到 pod。
      ```
      kubectl exec <pod_name> -it bash
      ```
      {: pre}

   2. 列出 pod 中的所有环境变量。
      ```
      env
      ```
      {: pre}

      输出示例：
        ```
      BINDING={"apikey":"KL34Ys893284NGJEPFjgrioJ12NElpow","host":"98765aab-9ce1-7tr3-ba87-bfbab6e6d9d6-bluemix.cloudant.com","iam_apikey_description":"Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:cloudantnosqldb:us-south:a/1234g56789cfe8e6388dd2ec098:98746cw-43d7-49ce-947a-d8fe3eebb381::","iam_apikey_name":"auto-generated-apikey-1234abcde-987f-3t64-9d96-d13775ec5663","iam_role_crn":"crn:v1:bluemix:public:iam::::serviceRole:Writer","iam_serviceid_crn":"crn:v1:bluemix:public:iam-identity::a/1234567890brasge5htn2ec098::serviceid:ServiceId-12345vgh-6c4c-ytr12-af6b-467d30d6ef44","password":"jfiavhui12484fnivhuo472nvei23913c3ff","port":443,"url":"https://25c73aac-9ce1-4c24-ba98-bfbab6e6d9d6-bluemix:ugvioev823inreuiegn43donvri29989wiu9t22@25c73aac-9ce1-4c24-ba98-abdrjio123562lnsb-bluemix.cloudant.com","username":"123b45da-9ce1-4c24-ab12-rinwnwub1294-bluemix"}
      ```
      {: screen}

7. 配置应用程序以读取环境变量，并解析 JSON 内容以检索访问服务所需的信息。

   Python 中的示例代码：
   ```python
   if os.environ.get('BINDING'):
        credentials = json.loads(os.environ.get('BINDING'))
   ```
   {: codeblock}

8. 可选：作为预防措施，将错误处理添加到应用程序，以应对 `BINDING` 环境变量未正确设置的情况。

   Java 示例代码：
   ```java
   if (System.getenv("BINDING") == null) {
    throw new RuntimeException("Environment variable 'SECRET' is not set!");
   }
   ```
   {: codeblock}

   Node.js 示例代码：
   ```js
   if (!process.env.BINDING) {
    console.error('ENVIRONMENT variable "BINDING" is not set!');
    process.exit(1);
   }
   ```
   {: codeblock}
