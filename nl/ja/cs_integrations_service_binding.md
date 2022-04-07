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



# IBM Cloud サービス・バインディングを使用したサービスの追加
{: #service-binding}

{{site.data.keyword.Bluemix_notm}} サービスを追加して、Watson AI、データ、セキュリティー、モノのインターネット (IoT) などの領域の追加機能によって Kubernetes クラスターを拡張します。
{:shortdesc}

**どのようなタイプのサービスをクラスターにバインドできますか?** </br>
{{site.data.keyword.Bluemix_notm}} サービスをクラスターに追加する際は、{{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM) に対応しているサービスと、Cloud Foundry に基づくサービスのいずれかを選択できます。 IAM 対応サービスは、より細かいアクセス制御を可能にし、{{site.data.keyword.Bluemix_notm}} リソース・グループ内で管理できます。 Cloud Foundry サービスは、Cloud Foundry の組織とスペースに追加する必要があり、リソース・グループに追加することはできません。 Cloud Foundry サービス・インスタンスへのアクセスを制御するには、Cloud Foundry の役割を使用します。 IAM 対応サービスと Cloud Foundry サービスについて詳しくは、[リソースとは](/docs/resources?topic=resources-resource#resource)を参照してください。

サポートされている {{site.data.keyword.Bluemix_notm}} サービスのリストについては、[{{site.data.keyword.Bluemix_notm}} のカタログ](https://cloud.ibm.com/catalog)を参照してください。

**{{site.data.keyword.Bluemix_notm}} サービス・バインディングとは何ですか?**</br>
サービス・バインディングは、{{site.data.keyword.Bluemix_notm}} サービスのサービス資格情報を作成して、これらの資格情報をクラスター内の Kubernetes シークレットに保管するための迅速な方法です。 クラスターにサービスをバインドするには、まずそのサービスのインスタンスをプロビジョンする必要があります。 次に、`ibmcloud ks cluster-service-bind` [コマンド](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_service_bind)を使用して、サービス資格情報と Kubernetes シークレットを作成します。 この Kubernetes シークレットは、データを保護するために、etcd 内で自動的に暗号化されます。

シークレットをさらに保護したいですか? クラスター管理者に連絡してクラスター内の [{{site.data.keyword.keymanagementservicefull}} を有効にして](/docs/containers?topic=containers-encryption#keyprotect)もらい、{{site.data.keyword.Bluemix_notm}} サービス・インスタンスの資格情報を保管するシークレットなどの、新しいシークレットと既存のシークレットを暗号化します。
{: tip}

**すべての {{site.data.keyword.Bluemix_notm}} サービスをクラスター内で使用できますか?**</br>
サービス・バインディングを使用できるのは、サービス・キーをサポートしているサービスのみに対してです。サービス・キーがサポートされている場合は、そのサービスの資格情報を自動的に作成して Kubernetes シークレットに保管できます。 サービス・キーをサポートしているサービスのリストを確認するには、[{{site.data.keyword.Bluemix_notm}} サービスを使用するための外部アプリの使用可能化](/docs/resources?topic=resources-externalapp#externalapp)を参照してください。

サービス・キーをサポートしていないサービスでは、通常、アプリで使用できる API が提供されます。 サービス・バインディング方式では、アプリに対する API アクセスは自動的にセットアップされません。 必ず、ご使用のサービスの API 資料を参照して、アプリに API インターフェースを実装してください。

## IBM Cloud サービスをクラスターに追加する
{: #bind-services}

{{site.data.keyword.Bluemix_notm}} サービス・バインディングを使用して、ご使用の {{site.data.keyword.Bluemix_notm}} サービスのサービス資格情報を自動的に作成して、これらの資格情報を Kubernetes シークレットに保管します。
{: shortdesc}

開始前に、以下のことを行います。
- 以下の役割があることを確認してください。
    - サービスをバインドするクラスターに対する[**エディター**または**管理者**の {{site.data.keyword.Bluemix_notm}} IAM プラットフォーム・アクセス役割](/docs/containers?topic=containers-users#platform)
    - サービスをバインドする Kubernetes 名前空間に対する[**ライター**または**管理者**の {{site.data.keyword.Bluemix_notm}} IAM サービス役割](/docs/containers?topic=containers-users#platform)
    - Cloud Foundry サービスの場合: サービスをプロビジョンするスペースに対する[**開発者**の Cloud Foundry 役割](/docs/iam?topic=iam-mngcf#mngcf)
- [アカウントにログインします。 該当する場合は、適切なリソース・グループをターゲットにします。 クラスターのコンテキストを設定します。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

クラスターに {{site.data.keyword.Bluemix_notm}} サービスを追加するには、以下の手順を実行します。

1. [{{site.data.keyword.Bluemix_notm}} サービスのインスタンスを作成します](/docs/resources?topic=resources-externalapp#externalapp)。
    * 一部の {{site.data.keyword.Bluemix_notm}} サービスは、選択された地域でのみ使用可能です。 サービスがクラスターと同じ地域で使用可能な場合にのみ、サービスをクラスターにバインドできます。 さらに、ワシントン DC ゾーンでサービス・インスタンスを作成する場合は、CLI を使用する必要があります。
    * **IAM 対応サービスの場合**: クラスターと同じリソース・グループ内にサービス・インスタンスを作成する必要があります。 サービスは 1 つのリソース・グループ内にしか作成できず、作成後にそのリソース・グループを変更することはできません。

2. 作成したサービスのタイプを確認し、サービス・インスタンスの**名前**をメモします。
   - **Cloud Foundry サービス:**
     ```
     ibmcloud service list
     ```
     {: pre}

     出力例:
     ```
     name                         service           plan    bound apps   last operation
     <cf_service_instance_name>   <service_name>    spark                create succeeded
     ```
     {: screen}

  - **{{site.data.keyword.Bluemix_notm}}IAM 対応サービス:**
     ```
     ibmcloud resource service-instances
     ```
     {: pre}

     出力例:
     ```
     Name                          Location   State    Type               Tags
     <iam_service_instance_name>   <region>   active   service_instance
     ```
     {: screen}

   {{site.data.keyword.Bluemix_notm}} ダッシュボードに異なるサービス・タイプが**「Cloud Foundry サービス」**および**「サービス」**として表示される場合もあります。

3. サービスを追加するために使用するクラスターの名前空間を識別します。
   ```
   kubectl get namespaces
   ```
   {: pre}

4. サービスをクラスターにバインドして、サービスのサービス資格情報を作成して、それらの資格情報を Kubernetes シークレットに保管します。 既存のサービス資格情報がある場合は、`--key` フラグを使用してそれらの資格情報を指定します。 IAM 対応サービスの場合は、これらの資格情報は**ライター**のサービス・アクセス役割を使用して自動的に作成されますが、`--role` フラグを使用して異なるサービス・アクセス役割を指定できます。 `--key` フラグを使用する場合は、`--role` フラグを組み込まないでください。
   ```
   ibmcloud ks cluster-service-bind --cluster <cluster_name_or_ID> --namespace <namespace> --service <service_instance_name> [--key <service_instance_key>] [--role <IAM_service_role>]
   ```
   {: pre}

   サービス資格情報が正常に作成されると、`binding-<service_instance_name>` という名前の Kubernetes シークレットが作成されます。  

   出力例:
   ```
   ibmcloud ks cluster-service-bind --cluster mycluster --namespace mynamespace --service cleardb
    Binding service instance to namespace...
    OK
    Namespace:	     mynamespace
    Secret name:     binding-<service_instance_name>
   ```
   {: screen}

5. Kubernetes シークレットでサービス資格情報を確認します。
   1. シークレットの詳細を取得し、**binding** 値をメモします。 **binding** 値は base64 エンコードであり、サービス・インスタンスの資格情報を JSON 形式で保持します。
      ```
      kubectl get secrets binding-<service_instance_name> --namespace=<namespace> -o yaml
      ```
      {: pre}

      出力例:
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

   2. binding 値をデコードします
      ```
      echo "<binding>" | base64 -D
      ```
      {: pre}

      出力例:
      ```
      {"apikey":"KL34Ys893284NGJEPFjgrioJ12NElpow","host":"98765aab-9ce1-7tr3-ba87-bfbab6e6d9d6-bluemix.cloudant.com","iam_apikey_description":"Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:cloudantnosqldb:us-south:a/1234g56789cfe8e6388dd2ec098:98746cw-43d7-49ce-947a-d8fe3eebb381::","iam_apikey_name":"auto-generated-apikey-1234abcde-987f-3t64-9d96-d13775ec5663","iam_role_crn":"crn:v1:bluemix:public:iam::::serviceRole:Writer","iam_serviceid_crn":"crn:v1:bluemix:public:iam-identity::a/1234567890brasge5htn2ec098::serviceid:ServiceId-12345vgh-6c4c-ytr12-af6b-467d30d6ef44","password":"jfiavhui12484fnivhuo472nvei23913c3ff","port":443,"url":"https://25c73aac-9ce1-4c24-ba98-bfbab6e6d9d6-bluemix:ugvioev823inreuiegn43donvri29989wiu9t22@25c73aac-9ce1-4c24-ba98-abdrjio123562lnsb-bluemix.cloudant.com","username":"123b45da-9ce1-4c24-ab12-rinwnwub1294-bluemix"}
      ```
      {: screen}

   3. オプション: 前のステップでデコードしたサービス資格情報を、{{site.data.keyword.Bluemix_notm}} ダッシュボードのサービス・インスタンスで検索したサービス資格情報と比較します。

6. これでサービスがクラスターにバインドされたため、[Kubernetes シークレットのサービス資格情報にアクセスする](#adding_app)ために、アプリを構成する必要があります。


## アプリからのサービス資格情報へのアクセス
{: #adding_app}

アプリから {{site.data.keyword.Bluemix_notm}} サービス・インスタンスにアクセスするには、Kubernetes シークレットに保管されているサービス資格情報をアプリで使用可能にする必要があります。
{: shortdesc}

サービス・インスタンスの資格情報は base64 エンコードであり、JSON 形式でシークレットの内部に保管されます。 シークレットにあるデータにアクセスするには、次のオプションの中から選択します。
- [シークレットをボリュームとしてポッドにマウントする](#mount_secret)
- [環境変数でシークレットを参照する](#reference_secret)
<br>

開始前に、以下のことを行います。
-  `kube-system` 名前空間に対する[**ライター**または**管理者**の {{site.data.keyword.Bluemix_notm}} IAM サービス役割](/docs/containers?topic=containers-users#platform)があることを確認してください。
- [アカウントにログインします。 該当する場合は、適切なリソース・グループをターゲットにします。 クラスターのコンテキストを設定します。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
- [{{site.data.keyword.Bluemix_notm}} サービスをクラスターに追加します](#bind-services)。

### シークレットをボリュームとしてポッドにマウントする
{: #mount_secret}

シークレットをボリュームとしてポッドにマウントすると、`binding` という名前のファイルがボリューム・マウント・ディレクトリーに保管されます。 JSON 形式の `binding` ファイルには、{{site.data.keyword.Bluemix_notm}} サービスにアクセスするために必要なすべての情報と資格情報が格納されます。
{: shortdesc}

1.  クラスター内の使用可能なシークレットをリストし、シークレットの **name** をメモします。 **Opaque** タイプのシークレットを探します。 複数のシークレットが存在する場合は、クラスター管理者に連絡して、サービスに対応する正しいシークレットを確認してください。
    ```
    kubectl get secrets
    ```
    {: pre}

    出力例:
    ```
    NAME                                    TYPE                                  DATA      AGE
    binding-<service_instance_name>         Opaque                                1         3m
    ```
    {: screen}

2.  Kubernetes デプロイメントの YAML ファイルを作成し、シークレットをボリュームとしてポッドにマウントします。
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
    <caption>YAML ファイルの構成要素について</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> YAML ファイルの構成要素について</th>
    </thead>
    <tbody>
    <tr>
    <td><code>volumeMounts.mountPath</code></td>
    <td>コンテナー内でボリュームがマウントされるディレクトリーの絶対パス。</td>
    </tr>
    <tr>
    <td><code>volumeMounts.name</code></br><code>volumes.name</code></td>
    <td>ポッドにマウントするボリュームの名前。</td>
    </tr>
    <tr>
    <td><code>secret.defaultMode</code></td>
    <td>シークレットに対する読み取りおよび書き込みアクセス権。 読み取り専用アクセス権を設定するには、`420` を使用します。 </td>
    </tr>
    <tr>
    <td><code>secret.secretName</code></td>
    <td>前のステップでメモしたシークレットの名前。</td>
    </tr></tbody></table>

3.  ポッドを作成して、シークレットをボリュームとしてマウントします。
    ```
    kubectl apply -f secret-test.yaml
    ```
    {: pre}

4.  ポッドが作成されたことを確認します。
    ```
    kubectl get pods
    ```
    {: pre}

    CLI 出力例:

    ```
    NAME                           READY     STATUS    RESTARTS   AGE
    secret-test-1111454598-gfx32   1/1       Running   0          1m
    ```
    {: screen}

5.  サービス資格情報にアクセスします。
    1. ポッドにログインします。
       ```
       kubectl exec <pod_name> -it bash
       ```
       {: pre}

    2. 先ほど定義したボリューム・マウント・パスにナビゲートし、ボリューム・マウント・パスのファイルをリストします。
       ```
       cd <volume_mountpath> && ls
       ```
       {: pre}

       出力例:
       ```
       バインド
       ```
       {: screen}

       `binding` ファイルには、Kubernetes シークレットに保管したサービス資格情報が含まれています。

    4. サービス資格情報を表示します。 資格情報は、JSON 形式でキー値ペアとして保管されています。
       ```
       cat binding
       ```
       {: pre}

       出力例:
       ```
       {"apikey":"KL34Ys893284NGJEPFjgrioJ12NElpow","host":"98765aab-9ce1-7tr3-ba87-bfbab6e6d9d6-bluemix.cloudant.com","iam_apikey_description":"Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:cloudantnosqldb:us-south:a/1234g56789cfe8e6388dd2ec098:98746cw-43d7-49ce-947a-d8fe3eebb381::","iam_apikey_name":"auto-generated-apikey-1234abcde-987f-3t64-9d96-d13775ec5663","iam_role_crn":"crn:v1:bluemix:public:iam::::serviceRole:Writer","iam_serviceid_crn":"crn:v1:bluemix:public:iam-identity::a/1234567890brasge5htn2ec098::serviceid:ServiceId-12345vgh-6c4c-ytr12-af6b-467d30d6ef44","password":"jfiavhui12484fnivhuo472nvei23913c3ff","port":443,"url":"https://25c73aac-9ce1-4c24-ba98-bfbab6e6d9d6-bluemix:ugvioev823inreuiegn43donvri29989wiu9t22@25c73aac-9ce1-4c24-ba98-abdrjio123562lnsb-bluemix.cloudant.com","username":"123b45da-9ce1-4c24-ab12-rinwnwub1294-bluemix"}
       ```
       {: screen}

    5. JSON コンテンツを解析してサービスにアクセスするために必要な情報を取得するようにアプリを構成します。

### 環境変数でシークレットを参照する
{: #reference_secret}

Kubernetes シークレットのサービス資格情報およびその他のキー値ペアを、環境変数としてデプロイメントに追加できます。
{: shortdesc}

1. クラスター内の使用可能なシークレットをリストし、シークレットの **name** をメモします。 **Opaque** タイプのシークレットを探します。 複数のシークレットが存在する場合は、クラスター管理者に連絡して、サービスに対応する正しいシークレットを確認してください。
   ```
   kubectl get secrets
   ```
   {: pre}

   出力例:
   ```
   NAME                                    TYPE                                  DATA      AGE
    binding-<service_instance_name>         Opaque                                1         3m
   ```
   {: screen}

2. シークレットの詳細を入手して、ポッドで環境変数として参照できる潜在的なキー値ペアを検索します。 サービス資格情報は、シークレットの `binding` キーに保管されています。
   ```
   kubectl get secrets binding-<service_instance_name> --namespace=<namespace> -o yaml
   ```
   {: pre}

   出力例:
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

3. Kubernetes デプロイメントの YAML ファイルを作成し、`binding` キーを参照する環境変数を指定します。
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
     <caption>YAML ファイルの構成要素について</caption>
     <thead>
     <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> YAML ファイルの構成要素について</th>
     </thead>
     <tbody>
     <tr>
     <td><code>containers.env.name</code></td>
     <td>環境変数の名前。</td>
     </tr>
     <tr>
     <td><code>env.valueFrom.secretKeyRef.name</code></td>
     <td>前のステップでメモしたシークレットの名前。</td>
     </tr>
     <tr>
     <td><code>env.valueFrom.secretKeyRef.key</code></td>
     <td>シークレットの一部であり、環境変数で参照するキー。 サービス資格情報を参照するには、<strong>binding</strong> キーを使用する必要があります。  </td>
     </tr>
     </tbody></table>

4. シークレットの `binding` キーを環境変数として参照するポッドを作成します。
   ```
   kubectl apply -f secret-test.yaml
   ```
   {: pre}

5. ポッドが作成されたことを確認します。
   ```
   kubectl get pods
   ```
   {: pre}

   CLI 出力例:
   ```
   NAME                           READY     STATUS    RESTARTS   AGE
    secret-test-1111454598-gfx32   1/1       Running   0          1m
   ```
   {: screen}

6. 環境変数が正しく設定されていることを確認します。
   1. ポッドにログインします。
      ```
      kubectl exec <pod_name> -it bash
      ```
      {: pre}

   2. すべての環境変数をポッド内にリストします。
      ```
      env
      ```
      {: pre}

      出力例:
      ```
      BINDING={"apikey":"KL34Ys893284NGJEPFjgrioJ12NElpow","host":"98765aab-9ce1-7tr3-ba87-bfbab6e6d9d6-bluemix.cloudant.com","iam_apikey_description":"Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:cloudantnosqldb:us-south:a/1234g56789cfe8e6388dd2ec098:98746cw-43d7-49ce-947a-d8fe3eebb381::","iam_apikey_name":"auto-generated-apikey-1234abcde-987f-3t64-9d96-d13775ec5663","iam_role_crn":"crn:v1:bluemix:public:iam::::serviceRole:Writer","iam_serviceid_crn":"crn:v1:bluemix:public:iam-identity::a/1234567890brasge5htn2ec098::serviceid:ServiceId-12345vgh-6c4c-ytr12-af6b-467d30d6ef44","password":"jfiavhui12484fnivhuo472nvei23913c3ff","port":443,"url":"https://25c73aac-9ce1-4c24-ba98-bfbab6e6d9d6-bluemix:ugvioev823inreuiegn43donvri29989wiu9t22@25c73aac-9ce1-4c24-ba98-abdrjio123562lnsb-bluemix.cloudant.com","username":"123b45da-9ce1-4c24-ab12-rinwnwub1294-bluemix"}
      ```
      {: screen}

7. 環境変数を読み取り、JSON コンテンツを解析してサービスにアクセスするために必要な情報を取得するようにアプリを構成します。

   Python のコード例:
   ```python
   if os.environ.get('BINDING'):
        credentials = json.loads(os.environ.get('BINDING'))
   ```
   {: codeblock}

8. オプション: 予防措置として、`BINDING` 環境変数が適切に設定されていない場合のエラー処理をアプリに追加します。

   Java のコード例:
   ```java
   if (System.getenv("BINDING") == null) {
    throw new RuntimeException("Environment variable 'SECRET' is not set!");
   }
   ```
   {: codeblock}

   Node.js のコード例:
   ```js
   if (!process.env.BINDING) {
    console.error('ENVIRONMENT variable "BINDING" is not set!');
    process.exit(1);
   }
   ```
   {: codeblock}
