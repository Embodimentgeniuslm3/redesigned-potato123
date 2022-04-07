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



# クラスターのストレージのトラブルシューティング
{: #cs_troubleshoot_storage}

{{site.data.keyword.containerlong}} を使用する際は、ここに示すクラスターのストレージのトラブルシューティング手法を検討してください。
{: shortdesc}

より一般的な問題が起きている場合は、[クラスターのデバッグ](/docs/containers?topic=containers-cs_troubleshoot)を試してください。
{: tip}


## 永続ストレージの障害のデバッグ
{: #debug_storage}

永続ストレージをデバッグするためのオプションを確認し、障害の根本原因を探します。
{: shortdesc}

1. {{site.data.keyword.Bluemix_notm}} と {{site.data.keyword.containerlong_notm}} のプラグインの最新バージョンを使用していることを確認します。 
   ```
   ibmcloud update
   ```
   {: pre}
   
   ```
   ibmcloud plugin repo-plugins
   ```
   {: pre}

2. ローカル・マシンで実行する `kubectl` CLI バージョンが、クラスターにインストールされている Kubernetes のバージョンと一致することを確認します。 
   1. クラスターおよびローカル・マシンにインストールされている `kubectl` CLI バージョンを表示します。
      ```
      kubectl version
      ```
      {: pre} 
   
      出力例: 
      ```
      Client Version: version.Info{Major:"1", Minor:"13", GitVersion:"v1.13.5", GitCommit:"641856db18352033a0d96dbc99153fa3b27298e5", GitTreeState:"clean", BuildDate:"2019-03-25T15:53:57Z", GoVersion:"go1.12.1", Compiler:"gc", Platform:"darwin/amd64"}
      Server Version: version.Info{Major:"1", Minor:"13", GitVersion:"v1.13.5+IKS", GitCommit:"e15454c2216a73b59e9a059fd2def4e6712a7cf0", GitTreeState:"clean", BuildDate:"2019-04-01T10:08:07Z", GoVersion:"go1.11.5", Compiler:"gc", Platform:"linux/amd64"}
      ```
      {: screen}
    
      CLI バージョンが一致するのは、クライアントとサーバーの `GitVersion` に同じバージョンが表示される場合です。サーバーのバージョンの `+IKS` 部分は無視できます。
   2. ローカル・マシンとクラスターの `kubectl` CLI バージョンが一致しない場合は、[クラスターを更新](/docs/containers?topic=containers-update)するか、[ローカル・マシンに別の CLI バージョンをインストール](/docs/containers?topic=containers-cs_cli_install#kubectl)します。 

3. ブロック・ストレージ、オブジェクト・ストレージ、および Portworx の場合のみ: [Kubernetes サービス・アカウントを使用して Helm サーバー Tiller をインストールした](/docs/containers?topic=containers-helm#public_helm_install)ことを確認します。 

4. ブロック・ストレージ、オブジェクト・ストレージ、および Portworx の場合のみ: プラグインの Helm チャートの最新バージョンをインストールしたことを確認します。 
   
   **ブロック・ストレージおよびオブジェクト・ストレージ**: 
   
   1. Helm チャート・リポジトリーを更新します。
      ```
      helm repo update
      ```
      {: pre}
      
   2. `iks-charts` リポジトリーの Helm チャートをリストします。
      ```
      helm search iks-charts
      ```
      {: pre}
      
      出力例: 
      ```
      iks-charts/ibm-block-storage-attacher          	1.0.2        A Helm chart for installing ibmcloud block storage attach...
      iks-charts/ibm-iks-cluster-autoscaler          	1.0.5        A Helm chart for installing the IBM Cloud cluster autoscaler
      iks-charts/ibm-object-storage-plugin           	1.0.6        A Helm chart for installing ibmcloud object storage plugin  
      iks-charts/ibm-worker-recovery                 	1.10.46      IBM Autorecovery system allows automatic recovery of unhe...
      ...
      ```
      {: screen}
      
   3. クラスターにインストールされた Helm チャートをリストし、インストールしたバージョンと使用可能なバージョンを比較します。
      ```
      helm ls
      ```
      {: pre}
      
   4. より新しいバージョンが使用可能な場合は、そのバージョンをインストールします。手順については、[{{site.data.keyword.Bluemix_notm}} Block Storage プラグインの更新](/docs/containers?topic=containers-block_storage#updating-the-ibm-cloud-block-storage-plug-in)および [{{site.data.keyword.cos_full_notm}} プラグインの更新](/docs/containers?topic=containers-object_storage#update_cos_plugin)を参照してください。 
   
   **Portworx**: 
   
   1. 使用可能な[最新バージョンの Helm チャート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/portworx/helm/tree/master/charts/portworx) を探します。 
   
   2. クラスターにインストールされた Helm チャートをリストし、インストールしたバージョンと使用可能なバージョンを比較します。
      ```
      helm ls
      ```
      {: pre}
   
   3. より新しいバージョンが使用可能な場合は、そのバージョンをインストールします。手順については、[クラスター内の Portworx の更新](/docs/containers?topic=containers-portworx#update_portworx)を参照してください。 
   
5. ストレージ・ドライバーおよびプラグインのポッドで、**Running** の状況が示されることを確認します。 
   1. `kube-system` 名前空間のポッドをリストします。
      ```
      kubectl get pods -n kube-system 
      ```
      {: pre}
      
   2. ポッドで **Running** の状況が示されない場合は、ポッドの詳細を取得して、根本原因を探します。ポッドの状況によっては、以下のコマンドの一部を実行できない場合があります。
      ```
      kubectl describe pod <pod_name> -n kube-system
      ```
      {: pre}
      
      ```
      kubectl logs <pod_name> -n kube-system
      ```
      {: pre}
      
   3. `kubectl describe pod` コマンドの CLI 出力の **Events** セクションと最新のログを分析して、エラーの根本原因を探します。 
   
6. PVC が正常にプロビジョンされているかどうかを確認します。 
   1. PVC の状態を確認します。PVC で **Bound** の状況が示される場合、PVC は正常にプロビジョンされています。
      ```
      kubectl get pvc
      ```
      {: pre}
      
   2. PVC の状態で **Pending** が示される場合は、エラーを取得して、PVC が保留中のままである理由を確認します。
      ```
      kubectl describe pvc <pvc_name>
      ```
      {: pre}
      
   3. PVC の作成中に発生する可能性がある一般的なエラーを確認します。 
      - [ファイル・ストレージおよびブロック・ストレージ: PVC が保留状態のままになる](#file_pvc_pending)
      - [オブジェクト・ストレージ: PVC が保留状態のままになる](#cos_pvc_pending)
   
7. ストレージ・インスタンスをマウントするポッドが正常にデプロイされているかどうかを確認します。 
   1. クラスター内のポッドをリストします。 ポッドで **Running** の状況が示される場合、ポッドは正常にデプロイされています。
      ```
      kubectl get pods
      ```
      {: pre}
      
   2. ポッドの詳細を取得し、CLI 出力の **Events** セクションにエラーが表示されているかどうかを確認します。
      ```
      kubectl describe pod <pod_name>
      ```
      {: pre}
   
   3. アプリケーションのログを取得し、エラー・メッセージが表示されるかどうかを確認します。
      ```
      kubectl logs <pod_name>
      ```
      {: pre}
   
   4. PVC をアプリケーションにマウントするときに発生する可能性がある一般的なエラーを確認します。 
      - [ファイル・ストレージ: アプリケーションが PVC へのアクセスまたは書き込みを行うことができない](#file_app_failures)
      - [ブロック・ストレージ: アプリケーションが PVC へのアクセスまたは書き込みを行うことができない](#block_app_failures)
      - [オブジェクト・ストレージ: 非 root ユーザーとしてのファイルへのアクセスが失敗する](#cos_nonroot_access)
      

## ファイル・ストレージおよびブロック・ストレージ: PVC が保留状態のままになる
{: #file_pvc_pending}

{: tsSymptoms}
PVC を作成して `kubectl get pvc <pvc_name>` を実行すると、しばらく待っても PVC が **Pending** の状態のままになります。 

{: tsCauses}
PVC の作成時およびバインド時に、ファイル・ストレージおよびブロック・ストレージのプラグインによってさまざまなタスクが実行されます。各タスクが失敗し、異なるエラー・メッセージが表示されることがあります。

{: tsResolve}

1. PVC が **Pending** の状態のままになっている根本原因を探します。 
   ```
   kubectl describe pvc <pvc_name> -n <namespace>
   ```
   {: pre}
   
2. 一般的なエラー・メッセージの説明と解決方法を確認します。
   
   <table>
   <thead>
     <th>エラー・メッセージ</th>
     <th>説明</th>
     <th>解決するための手順</th>
  </thead>
  <tbody>
    <tr>
      <td><code>User doesn't have permissions to create or manage Storage</code></br></br><code>Failed to find any valid softlayer credentials in configuration file</code></br></br><code>Storage with the order ID %d could not be created after retrying for %d seconds.</code></br></br><code>Unable to locate datacenter with name <datacenter_name>.</code></td>
      <td>クラスターの `storage-secret-store` Kubernetes シークレットに保管されている IAM API キーまたは IBM Cloud インフラストラクチャー (SoftLayer) API キーに、永続ストレージをプロビジョンするために必要な許可の一部がありません。</td>
      <td>[許可がないことが原因で PVC の作成が失敗する](#missing_permissions)を参照してください。</td>
    </tr>
    <tr>
      <td><code>Your order will exceed the maximum number of storage volumes allowed. Please contact Sales</code></td>
      <td>すべての {{site.data.keyword.Bluemix_notm}} アカウントは、作成可能なストレージ・インスタンスの最大数でセットアップされます。PVC を作成することにより、ストレージ・インスタンスの最大数を超えます。</td>
      <td>PVC を作成するには、以下のいずれかのオプションを選択します。<ul><li>使用されていない PVC をすべて削除します。</li><li>[サポート・ケースを開いて](/docs/get-support?topic=get-support-getting-customer-support)、{{site.data.keyword.Bluemix_notm}} アカウント所有者にストレージ割り当て量を増やすよう依頼します。</li></ul> </td>
    </tr>
    <tr>
      <td><code>Unable to find the exact ItemPriceIds(type|size|iops) for the specified storage</code> </br></br><code>Failed to place storage order with the storage provider</code></td>
      <td>PVC で指定したストレージ・サイズと IOPS は、選択したストレージ・タイプではサポートされていないため、指定されたストレージ・クラスでは使用できません。</td>
      <td>[ファイル・ストレージ構成の決定](/docs/containers?topic=containers-file_storage#file_predefined_storageclass)および[ブロック・ストレージ構成の決定](/docs/containers?topic=containers-block_storage#block_predefined_storageclass)を参照して、使用するストレージ・クラスのサポートされるストレージ・サイズと IOPS を確認してください。サイズと IOPS を修正し、PVC を再作成します。</td>
    </tr>
    <tr>
  <td><code>Failed to find the datacenter name in configuration file</code></td>
      <td>PVC で指定したデータ・センターが存在しません。</td>
  <td><code>ibmcloud ks locations</code> を実行して、使用可能なデータ・センターをリストします。PVC のデータ・センターを修正し、PVC を再作成します。</td>
    </tr>
    <tr>
  <td><code>Failed to place storage order with the storage provider</code></br></br><code>Storage with the order ID 12345 could not be created after retrying for xx seconds. </code></br></br><code>Failed to do subnet authorizations for the storage 12345.</code><code>Storage 12345 has ongoing active transactions and could not be completed after retrying for xx seconds.</code></td>
  <td>ストレージ・サイズ、IOPS、およびストレージ・タイプが、選択したストレージ・クラスと互換性がないか、{{site.data.keyword.Bluemix_notm}} インフラストラクチャー API エンドポイントが現在使用できません。</td>
  <td>[ファイル・ストレージ構成の決定](/docs/containers?topic=containers-file_storage#file_predefined_storageclass)および[ブロック・ストレージ構成の決定](/docs/containers?topic=containers-block_storage#block_predefined_storageclass)を参照して、使用するストレージ・クラスおよびストレージ・タイプのサポートされるストレージ・サイズと IOPS を確認してください。その後、PVC を削除し、PVC を再作成します。</td>
  </tr>
  <tr>
  <td><code>Failed to find the storage with storage id 12345. </code></td>
  <td>Kubernetes 静的プロビジョニングを使用して既存のストレージ・インスタンスの PVC を作成する必要がありますが、指定したストレージ・インスタンスが見つかりませんでした。</td>
  <td>指示に従って、クラスター内の既存の[ファイル・ストレージ](/docs/containers?topic=containers-file_storage#existing_file)または[ブロック・ストレージ](/docs/containers?topic=containers-block_storage#existing_block)をプロビジョンして、既存のストレージ・インスタンスの正しい情報を取得します。その後、PVC を削除し、PVC を再作成します。</td>
  </tr>
  <tr>
  <td><code>Storage type not provided, expected storage type is `Endurance` or `Performance`. </code></td>
  <td>カスタム・ストレージ・クラスを作成し、サポートされていないストレージ・タイプを指定しました。</td>
  <td>カスタム・ストレージ・クラスを更新して、ストレージ・タイプとして `Endurance` または `Performance` を指定します。カスタム・ストレージ・クラスの例については、[ファイル・ストレージ](/docs/containers?topic=containers-file_storage#file_custom_storageclass)および[ブロック・ストレージ](/docs/containers?topic=containers-block_storage#block_custom_storageclass)のサンプル・カスタム・ストレージ・クラスを参照してください。</td>
  </tr>
  </tbody>
  </table>
  
## ファイル・ストレージ: アプリケーションが PVC へのアクセスまたは書き込みを行うことができない
{: #file_app_failures}

PVC をポッドにマウントすると、PVC へのアクセス時または PVC への書き込み時にエラーが発生することがあります。
{: shortdesc}

1. クラスター内のポッドをリストし、ポッドの状況を確認します。 
   ```
   kubectl get pods
   ```
   {: pre}
   
2. アプリケーションが PVC へのアクセスまたは書き込みを行うことができない根本原因を探します。
   ```
   kubectl describe pod <pod_name>
   ```
   {: pre}
   
   ```
   kubectl logs <pod_name>
   ```
   {: pre}

3. PVC をポッドにマウントするときに発生する可能性がある一般的なエラーを確認します。 
   <table>
   <thead>
     <th>症状またはエラー・メッセージ</th>
     <th>説明</th>
     <th>解決するための手順</th>
  </thead>
  <tbody>
    <tr>
      <td>ポッドが <strong>ContainerCreating</strong> の状態で停滞しています。</br></br><code>MountVolume.SetUp failed for volume ... read-only file system</code></td>
      <td>{{site.data.keyword.Bluemix_notm}} インフラストラクチャーのバックエンドでネットワークの問題が発生しました。データを保護し、データ破損を回避するために、{{site.data.keyword.Bluemix_notm}} によって、NFS ファイル共有での書き込み操作を防止するために自動的にファイル・ストレージ・サーバーが切断されました。</td>
      <td>[ファイル・ストレージ: ワーカー・ノードのファイル・システムが読み取り専用に変更される](#readonly_nodes)を参照</td>
      </tr>
      <tr>
  <td><code>write-permission</code> </br></br><code>do not have required permission</code></br></br><code>cannot create directory '/bitnami/mariadb/data': Permission denied </code></td>
  <td>デプロイメントで、NFS ファイル・ストレージのマウント・パスを所有する非 root ユーザーを指定しました。デフォルトでは、非 root ユーザーには、NFS ベースのストレージのボリューム・マウント・パスに対する書き込み権限がありません。 </td>
  <td>[ファイル・ストレージ: 非 root ユーザーが NFS ファイル・ストレージのマウント・パスを所有しているとアプリが失敗する](#nonroot)を参照</td>
  </tr>
  <tr>
  <td>NFS ファイル・ストレージのマウント・パスを所有する非 root ユーザーを指定した後、または、非 root ユーザーの ID を指定して Helm チャートをデプロイした後に、そのユーザーがマウントされたストレージに書き込めません。</td>
  <td>デプロイメント構成または Helm チャートの構成で、ポッドの <code>fsGroup</code> (グループ ID) および <code>runAsUser</code> (ユーザー ID) のセキュリティー・コンテキストが指定されています。</td>
  <td>[ファイル・ストレージ: 永続ストレージに対する非 root ユーザー・アクセスの追加が失敗する](#cs_storage_nonroot)を参照</td>
  </tr>
</tbody>
</table>

### ファイル・ストレージ: ワーカー・ノードのファイル・システムが読み取り専用に変更される
{: #readonly_nodes}

{: tsSymptoms}
{: #stuck_creating_state}
以下のいずれかの症状が見られることがあります。
- `kubectl get pods -o wide` を実行すると、同じワーカー・ノードで稼働している複数のポッドが `ContainerCreating` 状態で停滞していることが判明します。
- `kubectl describe` コマンドを実行すると、**Events** セクションに `MountVolume.SetUp failed for volume ... read-only file system` というエラーが表示されます。

{: tsCauses}
ワーカー・ノード上のファイル・システムは読み取り専用です。

{: tsResolve}
1.  ワーカー・ノード上またはコンテナー内に保管されている可能性のあるデータをバックアップします。
2.  既存のワーカー・ノードに対する短期的な解決策としては、ワーカー・ノードを再ロードします。
    <pre class="pre"><code>ibmcloud ks worker-reload --cluster &lt;cluster_name&gt; --worker &lt;worker_ID&gt;</code></pre>

長期的な修正としては、[ワーカー・プールのマシン・タイプを更新します](/docs/containers?topic=containers-update#machine_type)。

<br />



### ファイル・ストレージ: 非 root ユーザーが NFS ファイル・ストレージのマウント・パスを所有しているとアプリが失敗する
{: #nonroot}

{: tsSymptoms}
デプロイメントに [NFS ストレージを追加](/docs/containers?topic=containers-file_storage#file_app_volume_mount)した後に、コンテナーのデプロイメントが失敗します。 コンテナーのログを取得すると、以下のようなエラーが表示されることがあります。 ポッドが失敗し、再ロードが繰り返されます。

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
デフォルトでは、非 root ユーザーには、NFS ベースのストレージのボリューム・マウント・パスに対する書き込み権限がありません。 一部の一般的なアプリのイメージ (Jenkins や Nexus3 など) は、マウント・パスを所有する非 root ユーザーを Dockerfile に指定しています。 この Dockerfile からコンテナーを作成すると、マウント・パスに対する非 root ユーザーの権限が不十分なために、コンテナーの作成は失敗します。 書き込み権限を付与するには、Dockerfile を変更して非 root ユーザーを root ユーザー・グループに一時的に追加してから、マウント・パスの権限を変更するか、または init コンテナーを使用します。

Helm チャートを使用してイメージをデプロイする場合は、init コンテナーを使用するように Helm デプロイメントを編集します。
{:tip}



{: tsResolve}
デプロイメントに [init コンテナー ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/) を指定すると、Dockerfile に指定された非 root ユーザーに、コンテナー内のボリューム・マウント・パスに対する書き込み権限を付与できます。 init コンテナーは、アプリ・コンテナーが開始される前に開始されます。 init コンテナーは、コンテナーの内部にボリューム・マウント・パスを作成し、そのマウント・パスを正しい (非 root) ユーザーが所有するように変更してから、クローズします。 その後、マウント・パスに書き込む必要がある非 root ユーザーでアプリ・コンテナーが開始されます。 パスは既に非 root ユーザーによって所有されているため、マウント・パスへの書き込みは成功します。 init コンテナーを使用したくない場合は、Dockerfile を変更して、非 root ユーザーに NFS ファイル・ストレージへのアクセス権限を追加できます。


開始前に、以下のことを行います。 [アカウントにログインします。 該当する場合は、適切なリソース・グループをターゲットにします。 クラスターのコンテキストを設定します。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  アプリの Dockerfile を開き、ボリューム・マウント・パスに対する書き込み権限を付与するユーザーのユーザー ID (UID) とグループ ID (GID) を取得します。 Jenkins Dockerfile の例では、情報は以下のとおりです。
    - UID: `1000`
    - GID: `1000`

    **例**:

    ```
    FROM openjdk:8-jdk

    RUN apt-get update && apt-get install -y git curl && rm -rf /var/lib/apt/lists/*

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

2.  永続ボリューム請求 (PVC) を作成して、アプリに永続ストレージを追加します。 この例では、`ibmc-file-bronze` ストレージ・クラスを使用します。 使用可能なストレージ・クラスを確認するには、`kubectl get storageclasses` を実行します。

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

3.  PVC を作成します。

    ```
    kubectl apply -f mypvc.yaml
    ```
    {: pre}

4.  デプロイメント `.yaml` ファイルに、init コンテナーを追加します。 先ほど取得した UID と GID を指定します。

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

    Jenkins デプロイメントの**例**:

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

5.  ポッドを作成して、PVC をポッドにマウントします。

    ```
    kubectl apply -f my_pod.yaml
    ```
    {: pre}

6. ボリュームがポッドに正常にマウントされたことを確認します。 ポッド名と **Containers/Mounts** パスをメモします。

    ```
    kubectl describe pod <my_pod>
    ```
    {: pre}

    **出力例**:

    ```
    Name:       mypod-123456789
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
        Type:	PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
        ClaimName:	mypvc
        ReadOnly:	  false

    ```
    {: screen}

7.  前にメモしたポッド名を使用して、ポッドにログインします。

    ```
    kubectl exec -it <my_pod-123456789> /bin/bash
    ```
    {: pre}

8. コンテナーのマウント・パスの権限を確認します。 この例では、マウント・パスは `/var/jenkins_home` です。

    ```
    ls -ln /var/jenkins_home
    ```
    {: pre}

    **出力例**:

    ```
    jenkins@mypod-123456789:/$ ls -ln /var/jenkins_home
    total 12
    -rw-r--r-- 1 1000 1000  102 Mar  9 19:58 copy_reference_file.log
    drwxr-xr-x 2 1000 1000 4096 Mar  9 19:58 init.groovy.d
    drwxr-xr-x 9 1000 1000 4096 Mar  9 20:16 war
    ```
    {: screen}

    この出力は、Dockerfile の GID と UID (この例では `1000` と `1000`) がコンテナー内部のマウント・パスを所有していることを示しています。

<br />


### ファイル・ストレージ: 永続ストレージに対する非 root ユーザー・アクセスの追加が失敗する
{: #cs_storage_nonroot}

{: tsSymptoms}
[永続ストレージに対する非 root ユーザーのアクセスを追加した](#nonroot)後、または、非 root ユーザーの ID を指定して Helm チャートをデプロイした後に、そのユーザーがマウントされたストレージに書き込めません。

{: tsCauses}
デプロイメント構成または Helm チャートの構成で、ポッドの `fsGroup` (グループ ID) と `runAsUser` (ユーザー ID) に対して[セキュリティー・コンテキスト](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/)が指定されています。 現在、{{site.data.keyword.containerlong_notm}} では `fsGroup` の指定をサポートしていません。`0` (ルート権限) に設定された `runAsUser` だけをサポートしています。

{: tsResolve}
イメージ、デプロイメント、または Helm チャートの構成ファイルから構成の `fsGroup` と `runAsUser` の `securityContext` フィールドを削除してから、再デプロイします。 マウント・パスの所有権を `nobody` から変更する必要がある場合は、[非 root ユーザー・アクセスを追加します](#nonroot)。 [非 root の `initContainer`](#nonroot) を追加した後、ポッド・レベルではなくコンテナー・レベルで `runAsUser` を設定します。

<br />




## ブロック・ストレージ: アプリケーションが PVC へのアクセスまたは書き込みを行うことができない
{: #block_app_failures}

PVC をポッドにマウントすると、PVC へのアクセス時または PVC への書き込み時にエラーが発生することがあります。
{: shortdesc}

1. クラスター内のポッドをリストし、ポッドの状況を確認します。 
   ```
   kubectl get pods
   ```
   {: pre}
   
2. アプリケーションが PVC へのアクセスまたは書き込みを行うことができない根本原因を探します。
   ```
   kubectl describe pod <pod_name>
   ```
   {: pre}
   
   ```
   kubectl logs <pod_name>
   ```
   {: pre}

3. PVC をポッドにマウントするときに発生する可能性がある一般的なエラーを確認します。 
   <table>
   <thead>
     <th>症状またはエラー・メッセージ</th>
     <th>説明</th>
     <th>解決するための手順</th>
  </thead>
  <tbody>
    <tr>
      <td>ポッドが <strong>ContainerCreating</strong> または <strong>CrashLoopBackOff</strong> の状態で停滞しています。</br></br><code>MountVolume.SetUp failed for volume ... read-only.</code></td>
      <td>{{site.data.keyword.Bluemix_notm}} インフラストラクチャーのバックエンドでネットワークの問題が発生しました。データを保護し、データ破損を回避するために、{{site.data.keyword.Bluemix_notm}} によって、ブロック・ストレージ・インスタンスでの書き込み操作を防止するために自動的にブロック・ストレージ・サーバーが切断されました。</td>
      <td>[ブロック・ストレージ: ブロック・ストレージが読み取り専用に変更される](#readonly_block)を参照</td>
      </tr>
      <tr>
  <td><code>failed to mount the volume as "ext4", it already contains xfs. Mount error: mount failed: exit status 32</code> </td>
        <td><code>XFS</code> ファイル・システムでセットアップされた、既存のブロック・ストレージ・インスタンスをマウントする必要があります。PV および一致する PVC を作成したときに、<code>ext4</code> を指定したか、ファイル・システムを指定しませんでした。PV で指定するファイル・システムは、ブロック・ストレージ・インスタンスでセットアップされているファイル・システムと同じでなければなりません。</td>
  <td>[ブロック・ストレージ: ファイル・システムが正しくないため、既存のブロック・ストレージをポッドにマウントできない](#block_filesystem)を参照</td>
  </tr>
</tbody>
</table>

### ブロック・ストレージ: ブロック・ストレージが読み取り専用に変更される
{: #readonly_block}

{: tsSymptoms}
以下の症状が見られることがあります。
- `kubectl get pods -o wide` を実行すると、同じワーカー・ノードの複数のポッドが `ContainerCreating` または `CrashLoopBackOff` 状態で停滞していることが判明します。 これらのポッドはすべて、同じブロック・ストレージ・インスタンスを使用しています。
- `kubectl describe pod` コマンドを実行すると、**Events** セクションに `MountVolume.SetUp failed for volume ... read-only` というエラーが表示されます。

{: tsCauses}
ポッドのボリュームへの書き込み中にネットワーク・エラーが発生した場合、IBM Cloud インフラストラクチャー (SoftLayer) によってボリュームが読み取り専用モードに変更され、ボリュームのデータが破損から保護されます。 このボリュームを使用しているポッドは、ボリュームへの書き込みを継続できず、失敗します。

{: tsResolve}
1. クラスターにインストールされている {{site.data.keyword.Bluemix_notm}} Block Storage プラグインのバージョンを確認します。
   ```
   helm ls
   ```
   {: pre}

2. [{{site.data.keyword.Bluemix_notm}} Block Storage プラグインの最新バージョン](https://cloud.ibm.com/kubernetes/solutions/helm-charts/ibm/ibmcloud-block-storage-plugin)を使用していることを確認します。 そうでない場合は、[プラグインを更新します](/docs/containers?topic=containers-block_storage#updating-the-ibm-cloud-block-storage-plug-in)。
3. ポッドに Kubernetes デプロイメントを使用している場合は、失敗するポッドを削除して Kubernetes でポッドを再作成し、ポッドを再始動します。 デプロイメントを使用していない場合は、`kubectl get pod <pod_name> -o yaml >pod.yaml` を実行して、ポッドの作成に使用された YAML ファイルを取得します。 その後、ポッドを削除して、手動でポッドを再作成します。
    ```
    kubectl delete pod <pod_name>
    ```
    {: pre}

4. ポッドの再作成によって問題が解決されたかどうかを確認します。 解決されていない場合は、ワーカー・ノードを再ロードします。
   1. ポッドが実行されているワーカー・ノードを見つけて、ワーカー・ノードに割り当てられているプライベート IP アドレスをメモします。
      ```
      kubectl describe pod <pod_name> | grep Node
      ```
      {: pre}

      出力例:
      ```
      Node:               10.75.XX.XXX/10.75.XX.XXX
      Node-Selectors:  <none>
      ```
      {: screen}

   2. 前のステップでメモしたプライベート IP アドレスを使用して、ワーカー・ノードの **ID** を取得します。
      ```
      ibmcloud ks workers --cluster <cluster_name_or_ID>
      ```
      {: pre}

   3. 安全な方法で、[ワーカー・ノードを再ロードします](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)。


<br />


### ブロック・ストレージ: ファイル・システムが正しくないため、既存のブロック・ストレージをポッドにマウントできない
{: #block_filesystem}

{: tsSymptoms}
`kubectl describe pod<pod_name>` を実行すると、以下のエラーが表示されます。
```
failed to mount the volume as "ext4", it already contains xfs. Mount error: mount failed: exit status 32
```
{: screen}

{: tsCauses}
`XFS` ファイル・システムでセットアップされている、既存のブロック・ストレージ・デバイスがあります。 このデバイスをポッドにマウントするために、`spec/flexVolume/fsType` セクションでファイル・システムとして `ext4` を指定しているか、またはファイル・システムを指定していない [PV を作成](/docs/containers?topic=containers-block_storage#existing_block)しました。 ファイル・システムが定義されていない場合、PV はデフォルトで `ext4` に設定されます。
PV は正常に作成され、既存のブロック・ストレージ・インスタンスにリンクされました。 ところが、対応する PVC を使用してクラスターに PV をマウントしようとすると、ボリュームのマウントが失敗します。 `ext4` ファイル・システムが指定された `XFS` ブロック・ストレージ・インスタンスをポッドにマウントすることができません。

{: tsResolve}
既存の PV のファイル・システムを `ext4` から `XFS` に更新します。

1. クラスター内の既存の PV をリストし、既存のブロック・ストレージ・インスタンスに使用している PV の名前をメモします。
   ```
   kubectl get pv
   ```
   {: pre}

2. ローカル・マシンに PV YAML を保存します。
   ```
   kubectl get pv <pv_name> -o yaml > <filepath/xfs_pv.yaml>
   ```
   {: pre}

3. YAML ファイルを開き、`fsType` を `ext4` から `xfs` に変更します。
4. クラスター内の PV を置き換えます。
   ```
   kubectl replace --force -f <filepath/xfs_pv.yaml>
   ```
   {: pre}

5. PV をマウントしたポッドにログインします。
   ```
   kubectl exec -it <pod_name> sh
   ```
   {: pre}

6. ファイル・システムが `XFS` に変更されたことを確認します。
   ```
   df -Th
   ```
   {: pre}

   出力例:
   ```
   Filesystem Type Size Used Avail Use% Mounted on /dev/mapper/3600a098031234546d5d4c9876654e35 xfs 20G 33M 20G 1% /myvolumepath
   ```
   {: screen}

<br />



## オブジェクト・ストレージ: {{site.data.keyword.cos_full_notm}} `ibmc` Helm プラグインのインストールが失敗する
{: #cos_helm_fails}

{: tsSymptoms}
{{site.data.keyword.cos_full_notm}} `ibmc` Helm プラグインをインストールすると、インストールが失敗し、以下のいずれかのエラーが発生します。
```
Error: symlink /Users/ibm/ibmcloud-object-storage-plugin/helm-ibmc /Users/ibm/.helm/plugins/helm-ibmc: file exists
```
{: screen}

```
Error: fork/exec /home/iksadmin/.helm/plugins/helm-ibmc/ibmc.sh: permission denied
```
{: screen}

{: tsCauses}
`ibmc` Helm プラグインがインストールされると、`./helm/plugins/helm-ibmc` ディレクトリーから、`ibmc` Helm プラグインがローカル・システムに置かれているディレクトリー (通常は `./ibmcloud-object-storage-plugin/helm-ibmc` にあります) へのシンボリック・リンクが作成されます。 ローカル・システムから `ibmc` Helm プラグインを削除するか、または `ibmc` Helm プラグイン・ディレクトリーを別の場所に移動すると、シンボリック・リンクは削除されません。

`permission denied` エラーが表示される場合、`ibmc` Helm プラグイン・コマンドを実行できるようにするために必要な `read`、`write`、および `execute` の許可が `ibmc.sh` bash ファイルにありません。 

{: tsResolve}

**シンボリック・リンク・エラーの場合**: 

1. {{site.data.keyword.cos_full_notm}} Helm プラグインを削除します。
   ```
   rm -rf ~/.helm/plugins/helm-ibmc
   ```
   {: pre}

2. [資料](/docs/containers?topic=containers-object_storage#install_cos)に従って、`ibmc` Helm プラグインおよび {{site.data.keyword.cos_full_notm}} プラグインを再インストールします。

**許可エラーの場合**: 

1. `ibmc` プラグインのアクセス許可を変更します。 
   ```
   chmod 755 ~/.helm/plugins/helm-ibmc/ibmc.sh
   ```
   {: pre}
   
2. `ibm` Helm プラグインを試行します。 
   ```
   helm ibmc --help
   ```
   {: pre}
   
3. [{{site.data.keyword.cos_full_notm}} プラグインのインストールを続行します](/docs/containers?topic=containers-object_storage#install_cos)。 


<br />


## オブジェクト・ストレージ: PVC が保留状態のままになる
{: #cos_pvc_pending}

{: tsSymptoms}
PVC を作成して `kubectl get pvc <pvc_name>` を実行すると、しばらく待っても PVC が **Pending** の状態のままになります。 

{: tsCauses}
PVC の作成時およびバインド時に、{{site.data.keyword.cos_full_notm}} プラグインによってさまざまなタスクが実行されます。各タスクが失敗し、異なるエラー・メッセージが表示されることがあります。

{: tsResolve}

1. PVC が **Pending** の状態のままになっている根本原因を探します。 
   ```
   kubectl describe pvc <pvc_name> -n <namespace>
   ```
   {: pre}
   
2. 一般的なエラー・メッセージの説明と解決方法を確認します。
   
   <table>
   <thead>
     <th>エラー・メッセージ</th>
     <th>説明</th>
     <th>解決するための手順</th>
  </thead>
  <tbody>
    <tr>
      <td>`User doesn't have permissions to create or manage Storage`</td>
      <td>クラスターの `storage-secret-store` Kubernetes シークレットに保管されている IAM API キーまたは IBM Cloud インフラストラクチャー (SoftLayer) API キーに、永続ストレージをプロビジョンするために必要な許可の一部がありません。</td>
      <td>[許可がないことが原因で PVC の作成が失敗する](#missing_permissions)を参照してください。</td>
    </tr>
    <tr>
      <td>`cannot get credentials: cannot get secret <secret_name>: secrets "<secret_name>" not found`</td>
      <td>{{site.data.keyword.cos_full_notm}} サービス資格情報を保持する Kubernetes シークレットが、PVC またはポッドと同じ名前空間に存在しません。</td>
      <td>[Kubernetes シークレットが見つからないため、PVC またはポッドの作成が失敗する](#cos_secret_access_fails)を参照してください。</td>
    </tr>
    <tr>
      <td>`cannot get credentials: Wrong Secret Type.Provided secret of type XXXX.Expected type ibm/ibmc-s3fs`</td>
      <td>{{site.data.keyword.cos_full_notm}} サービス・インスタンス用に作成した Kubernetes シークレットには、`type: ibm/ibmc-s3fs` は含まれていません。</td>
      <td>{{site.data.keyword.cos_full_notm}} 資格情報を保持する Kubernetes シークレットを編集して、`type` を `ibm/ibmc-s3fs` に追加または変更します。</td>
    </tr>
    <tr>
      <td>`Bad value for ibm.io/object-store-endpoint XXXX: scheme is missing. Must be of the form http://<hostname> or https://<hostname>`</br> </br> `Bad value for ibm.io/iam-endpoint XXXX: scheme is missing. Must be of the form http://<hostname< or https://<hostname>`</td>
      <td>s3fs API エンドポイントまたは IAM API エンドポイントの形式が正しくないか、またはクラスターの場所に基づいて s3fs API エンドポイントを取得できませんでした。</td>
      <td>[誤った s3fs API エンドポイントが原因で PVC の作成が失敗する](#cos_api_endpoint_failure)を参照してください。</td>
    </tr>
    <tr>
      <td>`object-path cannot be set when auto-create is enabled`</td>
      <td>`ibm.io/object-path` アノテーションを使用して、PVC にマウントするバケットの既存のサブディレクトリーを指定しました。サブディレクトリーを設定する場合は、バケット自動作成機能を無効にする必要があります。</td>
      <td>PVC で、`ibm.io/auto-create-bucket: "false"` を設定し、`ibm.io/bucket` に既存のバケットの名前を指定します。</td>
    </tr>
    <tr>
    <td>`bucket auto-create must be enabled when bucket auto-delete is enabled`</td>
    <td>PVC で、PVC を削除するときにデータ、バケット、および PV を自動的に削除するように `ibm.io/auto-delete-bucket: true` を設定しました。このオプションでは、`ibm.io/auto-create-bucket` を <strong>true</strong> に設定し、同時に `ibm.io/bucket` を `""` に設定する必要があります。</td>
    <td>PVC で、`ibm.io/auto-create-bucket: true` および `ibm.io/bucket: ""` を設定して、`tmp-s3fs-xxxx` の形式の名前でバケットが自動的に作成されるようにします。</td>
    </tr>
    <tr>
    <td>`bucket cannot be set when auto-delete is enabled`</td>
    <td>PVC で、PVC を削除するときにデータ、バケット、および PV を自動的に削除するように `ibm.io/auto-delete-bucket: true` を設定しました。このオプションでは、`ibm.io/auto-create-bucket` を <strong>true</strong> に設定し、同時に `ibm.io/bucket` を `""` に設定する必要があります。</td>
    <td>PVC で、`ibm.io/auto-create-bucket: true` および `ibm.io/bucket: ""` を設定して、`tmp-s3fs-xxxx` の形式の名前でバケットが自動的に作成されるようにします。</td>
    </tr>
    <tr>
    <td>`cannot create bucket using API key without service-instance-id`</td>
    <td>IAM API キーを使用して {{site.data.keyword.cos_full_notm}} サービス・インスタンスにアクセスする場合は、{{site.data.keyword.cos_full_notm}} サービス・インスタンスの API キーと ID を Kubernetes シークレットに保管する必要があります。</td>
    <td>[オブジェクト・ストレージ・サービス資格情報用のシークレットの作成](/docs/containers?topic=containers-object_storage#create_cos_secret)を参照してください。</td>
    </tr>
    <tr>
      <td>`object-path “<subdirectory_name>” not found inside bucket <bucket_name>`</td>
      <td>`ibm.io/object-path` アノテーションを使用して、PVC にマウントするバケットの既存のサブディレクトリーを指定しました。このサブディレクトリーが、指定したバケットに見つかりませんでした。</td>
      <td>`ibm.io/bucket` で指定したバケットに、`ibm.io/object-path` で指定したサブディレクトリーが存在することを確認します。</td>
    </tr>
        <tr>
          <td>`BucketAlreadyExists: The requested bucket name is not available. The bucket namespace is shared by all users of the system. Please select a different name and try again.`</td>
          <td>`ibm.io/auto-create-bucket: true` を設定し、同時にバケット名を指定したか、{{site.data.keyword.cos_full_notm}} に既に存在しているバケット名を指定しました。バケット名は、{{site.data.keyword.cos_full_notm}} 内のすべてのサービス・インスタンスおよび地域で固有でなければなりません。</td>
          <td>`ibm.io/auto-create-bucket: false` を設定し、{{site.data.keyword.cos_full_notm}} で固有のバケット名を指定するようにします。{{site.data.keyword.cos_full_notm}} プラグインを使用して自動的にバケット名を作成する場合は、`ibm.io/auto-create-bucket: true` および `ibm.io/bucket: ""` を設定します。バケットは、`tmp-s3fs-xxxx` の形式で固有の名前で作成されます。</td>
        </tr>
        <tr>
          <td>`cannot access bucket <bucket_name>: NotFound: Not Found`</td>
          <td>作成しなかったバケットにアクセスしようとしたか、または指定したストレージ・クラスおよび s3fs API エンドポイントが、バケットの作成時に使用されたストレージ・クラスおよび s3fs API エンドポイントと一致しません。</td>
          <td>[既存のバケットにアクセスできない](#cos_access_bucket_fails)を参照してください。</td>
        </tr>
        <tr>
          <td>`Put https://s3-api.dal-us-geo.objectstorage.service.networklayer.com/<bucket_name>: net/http: invalid header field value "AWS4-HMAC-SHA256 Credential=1234a12a123a123a1a123aa1a123a123\n\n/20190412/us-standard/s3/aws4_request, SignedHeaders=host;x-amz-content-sha256;x-amz-date, Signature=12aa1abc123456aabb12aas12aa123456sb123456abc" for key Authorization`</td>
          <td>Kubernetes シークレットの値が base64 に正しくエンコードされていません。</td>
          <td>Kubernetes シークレットの値を確認し、すべての値を base64 にエンコードします。[`kubectl create secret`](/docs/containers?topic=containers-object_storage#create_cos_secret) コマンドを使用して新しいシークレットを作成し、Kubernetes によって自動的に値が base64 にエンコードされるようにすることもできます。</td>
        </tr>
        <tr>
          <td>`cannot access bucket <bucket_name>: Forbidden: Forbidden`</td>
          <td>`ibm.io/auto-create-bucket: false` を指定して、作成しなかったバケットにアクセスしようとしたか、{{site.data.keyword.cos_full_notm}} HMAC 資格情報のサービス・アクセス・キーまたはアクセス・キー ID が正しくありません。</td>
          <td>作成していないバケットにはアクセスできません。代わりに `ibm.io/auto-create-bucket: true` および `ibm.io/bucket: ""` を設定して、新しいバケットを作成します。バケットの所有者である場合は、[資格情報が正しくないかアクセスが拒否されたために PVC の作成が失敗する](#cred_failure)を参照して、資格情報を確認します。</td>
        </tr>
        <tr>
          <td>`cannot create bucket <bucket_name>: AccessDenied: Access Denied`</td>
          <td>{{site.data.keyword.cos_full_notm}} で自動的にバケットを作成するように `ibm.io/auto-create-bucket: true` を指定しましたが、Kubernetes シークレットで指定した資格情報に**リーダー**の IAM サービス・アクセス役割が割り当てられています。この役割では、{{site.data.keyword.cos_full_notm}} でのバケットの作成は許可されません。</td>
          <td>[資格情報が正しくないかアクセスが拒否されたために PVC の作成が失敗する](#cred_failure)を参照してください。</td>
        </tr>
        <tr>
          <td>`cannot create bucket <bucket_name>: AccessForbidden: Access Forbidden`</td>
          <td>`ibm.io/auto-create-bucket: true` を指定して、`ibm.io/bucket` の既存のバケットの名前を指定しました。さらに、Kubernetes シークレットで指定した資格情報には、**リーダー**の IAM サービス・アクセス役割が割り当てられています。この役割では、{{site.data.keyword.cos_full_notm}} でのバケットの作成は許可されません。</td>
          <td>既存のバケットを使用するには、`ibm.io/auto-create-bucket: false` を設定し、`ibm.io/bucket` の既存のバケットの名前を指定します。既存の Kubernetes シークレットを使用してバケットを自動的に作成するには、`ibm.io/bucket: ""` を設定して、[資格情報が正しくないかアクセスが拒否されたために PVC の作成が失敗する](#cred_failure)に従って、Kubernetes シークレットの資格情報を確認します。</td>
        </tr>
        <tr>
          <td>`cannot create bucket <bucket_name>: SignatureDoesNotMatch: The request signature we calculated does not match the signature you provided. Check your AWS Secret Access Key and signing method. For more information, see REST Authentication and SOAP Authentication for details`</td>
          <td>Kubernetes シークレットに指定した HMAC 資格情報の {{site.data.keyword.cos_full_notm}} シークレット・アクセス・キーが正しくありません。</td>
          <td>[資格情報が正しくないかアクセスが拒否されたために PVC の作成が失敗する](#cred_failure)を参照してください。</td>
        </tr>
        <tr>
          <td>`cannot create bucket <bucket_name>: InvalidAccessKeyId: The AWS Access Key ID you provided does not exist in our records`</td>
          <td>Kubernetes シークレットに指定した HMAC 資格情報の {{site.data.keyword.cos_full_notm}} アクセス・キー ID またはシークレット・アクセス・キーが正しくありません。</td>
          <td>[資格情報が正しくないかアクセスが拒否されたために PVC の作成が失敗する](#cred_failure)を参照してください。</td>
        </tr>
        <tr>
          <td>`cannot create bucket <bucket_name>: CredentialsEndpointError: failed to load credentials` </br> </br> `cannot access bucket <bucket_name>: CredentialsEndpointError: failed to load credentials`</td>
          <td>IAM 資格情報の {{site.data.keyword.cos_full_notm}} API キーと {{site.data.keyword.cos_full_notm}} サービス・インスタンスの GUID が正しくありません。</td>
          <td>[資格情報が正しくないかアクセスが拒否されたために PVC の作成が失敗する](#cred_failure)を参照してください。</td>
        </tr>
  </tbody>
    </table>
    

### オブジェクト・ストレージ: Kubernetes シークレットが見つからないため、PVC またはポッドの作成が失敗する
{: #cos_secret_access_fails}

{: tsSymptoms}
PVC を作成するか、PVC をマウントするポッドをデプロイすると、作成またはデプロイメントが失敗します。

- PVC の作成が失敗した場合のエラー・メッセージの例:
  ```
  cannot get credentials: cannot get secret tsecret-key: secrets "secret-key" not found
  ```
  {: screen}

- ポッドの作成が失敗した場合のエラー・メッセージの例:
  ```
  persistentvolumeclaim "pvc-3" not found (repeated 3 times)
  ```
  {: screen}

{: tsCauses}
{{site.data.keyword.cos_full_notm}} サービス資格情報を保管する Kubernetes シークレット、PVC、およびポッドのすべてが、同じ Kubernetes 名前空間に含まれていません。 シークレットが PVC またはポッドとは異なる名前空間にデプロイされた場合、そのシークレットにはアクセスできません。

{: tsResolve}
このタスクには、すべての名前空間に対する[**ライター**または**管理者**の {{site.data.keyword.Bluemix_notm}} IAM サービス役割](/docs/containers?topic=containers-users#platform)が必要です。

1. クラスター内のシークレットをリストし、{{site.data.keyword.cos_full_notm}} サービス・インスタンスの Kubernetes シークレットが作成される Kubernetes 名前空間を確認します。 シークレットには、`ibm/ibmc-s3fs` が **Type** として表示される必要があります。
   ```
   kubectl get secrets --all-namespaces
   ```
   {: pre}

2. ご使用の PVC およびポッドの YAML 構成ファイルを確認して、同じ名前空間を使用したことを確認します。 シークレットが存在する名前空間とは異なる名前空間にポッドをデプロイする場合は、その名前空間に[別のシークレットを作成](/docs/containers?topic=containers-object_storage#create_cos_secret)します。

3. 該当の名前空間に PVC を作成するか、ポッドをデプロイします。

<br />


### オブジェクト・ストレージ: 資格情報が正しくないかアクセスが拒否されたために PVC の作成が失敗する
{: #cred_failure}

{: tsSymptoms}
PVC を作成すると、次のいずれかのようなエラー・メッセージが表示されます。

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
サービス・インスタンスへのアクセスに使用する {{site.data.keyword.cos_full_notm}} サービス資格情報が正しくないか、バケットに対する読み取りアクセスのみを許可している可能性があります。

{: tsResolve}
1. サービス詳細ページのナビゲーションで、**「サービス資格情報」**をクリックします。
2. 資格情報を見つけて、**「資格情報の表示」**をクリックします。
3. **iam_role_crn** セクションで、`Writer` または `Manager` の役割があることを確認します。 正しい役割を持っていない場合は、正しい権限で新しい {{site.data.keyword.cos_full_notm}} サービス資格情報を作成する必要があります。 
4. 役割が正しい場合は、Kubernetes シークレットで正しい **access_key_id** と **secret_access_key** を使用していることを確認します。 
5. [更新された **access_key_id** と **secret_access_key** を使用して新しいシークレットを作成します](/docs/containers?topic=containers-object_storage#create_cos_secret)。 


<br />


### オブジェクト・ストレージ: 誤った s3fs API エンドポイントまたは IAM API エンドポイントが原因で PVC の作成が失敗する
{: #cos_api_endpoint_failure}

{: tsSymptoms}
PVC を作成した場合に、PVC が保留状態のままになります。`kubectl describe pvc <pvc_name>` コマンドを実行すると、次のエラー・メッセージのいずれかが表示されます。 

```
Bad value for ibm.io/object-store-endpoint XXXX: scheme is missing. Must be of the form http://<hostname> or https://<hostname>
```
{: screen}

```
Bad value for ibm.io/iam-endpoint XXXX: scheme is missing. Must be of the form http://<hostname> or https://<hostname>
```
{: screen}

{: tsCauses}
使用するバケットの s3fs API エンドポイントの形式が誤っているか、{{site.data.keyword.containerlong_notm}} でサポートされているが {{site.data.keyword.cos_full_notm}} プラグインではまだサポートされていないロケーションにクラスターがデプロイされています。 

{: tsResolve}
1. {{site.data.keyword.cos_full_notm}} プラグインのインストール時に `ibmc` Helm プラグインによってストレージ・クラスに自動的に割り当てられた s3fs API エンドポイントを確認します。エンドポイントは、クラスターがデプロイされたロケーションに基づきます。  
   ```
   kubectl get sc ibmc-s3fs-standard-regional -o yaml | grep object-store-endpoint
   ```
   {: pre}

   コマンドで `ibm.io/object-store-endpoint: NA` が返される場合は、{{site.data.keyword.containerlong_notm}} でサポートされているが {{site.data.keyword.cos_full_notm}} プラグインではまだサポートされていないロケーションにクラスターがデプロイされています。ロケーションを {{site.data.keyword.containerlong_notm}} に追加するには、公開されている Slack に質問を投稿するか、{{site.data.keyword.Bluemix_notm}} サポート・ケースを開きます。詳しくは、[ヘルプとサポートの取得](/docs/containers?topic=containers-cs_troubleshoot#ts_getting_help)を参照してください。 
   
2. PVC に `ibm.io/endpoint` アノテーションを使用して s3fs API エンドポイントを手動で追加したか、`ibm.io/iam-endpoint` アノテーションを使用して IAM API エンドポイントを手動で追加した場合は、`https://<s3fs_api_endpoint>` と `https://<iam_api_endpoint>` の形式でエンドポイントを追加したことを確認してください。アノテーションは、{{site.data.keyword.cos_full_notm}} ストレージ・クラスの `ibmc` プラグインによって自動的に設定される API エンドポイントを上書きします。 
   ```
   kubectl describe pvc <pvc_name>
   ```
   {: pre}
   
<br />


### オブジェクト・ストレージ: 既存のバケットにアクセスできない
{: #cos_access_bucket_fails}

{: tsSymptoms}
PVC を作成すると、{{site.data.keyword.cos_full_notm}} のバケットにアクセスできません。 次のようなエラー・メッセージが表示されます。

```
Failed to provision volume with StorageClass "ibmc-s3fs-standard-regional": pvc:1b2345678b69175abc98y873e2:cannot access bucket <bucket_name>: NotFound: Not Found
```
{: screen}

{: tsCauses}
既存のバケットへのアクセスに正しくないストレージ・クラスを使用していたか、作成していないバケットにアクセスしようとした可能性があります。 作成していないバケットにはアクセスできません。 

{: tsResolve}
1. [{{site.data.keyword.Bluemix_notm}} ダッシュボード ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/) から、{{site.data.keyword.cos_full_notm}} サービス・インスタンスを選択します。
2. **「バケット」**を選択します。
3. 既存のバケットの**クラス**および**場所**の情報を確認します。
4. 該当する[ストレージ・クラス](/docs/containers?topic=containers-object_storage#cos_storageclass_reference)を選択します。
5. `ibm.io/auto-create-bucket: false` を設定し、既存のバケットの正しい名前を指定するようにします。 

<br />


## オブジェクト・ストレージ: 非 root ユーザーとしてのファイルへのアクセスが失敗する
{: #cos_nonroot_access}

{: tsSymptoms}
コンソールまたは REST API を使用して、{{site.data.keyword.cos_full_notm}} サービス・インスタンスにファイルをアップロードしました。 アプリ・デプロイメントで `runAsUser` で定義した非 root ユーザーを使用してこれらのファイルにアクセスしようとすると、ファイルへのアクセスは拒否されます。

{: tsCauses}
Linux では、ファイルまたはディレクトリーには `Owner`、`Group`、および `Other` の 3 つのアクセス・グループがあります。 コンソールまたは REST API を使用してファイルを {{site.data.keyword.cos_full_notm}} にアップロードする場合、`Owner`、`Group`、および `Other` の権限が削除されます。 各ファイルの権限は、以下のようになります。

```
d--------- 1 root root 0 Jan 1 1970 <file_name>
```
{: screen}

{{site.data.keyword.cos_full_notm}} プラグインを使用してファイルをアップロードすると、ファイルの権限が保持され、変更されません。

{: tsResolve}
非 root ユーザーとしてファイルにアクセスするために、非 root ユーザーには、そのファイルに対する読み取り権限と書き込み権限が必要です。 ポッドのデプロイメントの一部としてファイルに対するアクセス権を変更するには、書き込み操作が必要です。 {{site.data.keyword.cos_full_notm}} は、書き込みワークロード用には設計されていません。 ポッドのデプロイメント中に権限を更新すると、ポッドが `Running` 状態にならない場合があります。

この問題を解決するには、PVC をアプリ・ポッドにマウントする前に、別のポッドを作成して非 root ユーザーに正しい権限を設定します。

1. バケット内のファイルのアクセス権を確認します。
   1. `test-permission` ポッドの構成ファイルを作成し、ファイルに `test-permission.yaml` という名前を付けます。
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

   2. `test-permission` ポッドを作成します。
      ```
      kubectl apply -f test-permission.yaml
      ```
      {: pre}

   3. ポッドにログインします。
      ```
      kubectl exec test-permission -it bash
      ```
      {: pre}

   4. マウント・パスにナビゲートし、ファイルのアクセス権をリストします。
      ```
      cd test && ls -al
      ```
      {: pre}

      出力例:
      ```
      d--------- 1 root root 0 Jan 1 1970 <file_name>
      ```
      {: screen}

2. ポッドを削除します。
   ```
   kubectl delete pod test-permission
   ```
   {: pre}

3. ファイルのアクセス権を修正するために使用するポッドの構成ファイルを作成し、`fix-permission.yaml` という名前を付けます。
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

3. `fix-permission` ポッドを作成します。
   ```
   kubectl apply -f fix-permission.yaml
   ```
   {: pre}

4. ポッドが `Completed` 状態になるまで待機します。  
   ```
   kubectl get pod fix-permission
   ```
   {: pre}

5. `fix-permission` ポッドを削除します。
   ```
   kubectl delete pod fix-permission
   ```
   {: pre}

5. 以前に使用した `test-permission` ポッドを再作成して、アクセス権を確認します。
   ```
   kubectl apply -f test-permission.yaml
   ```
   {: pre}

5. ファイルのアクセス権が更新されていることを確認します。
   1. ポッドにログインします。
      ```
      kubectl exec test-permission -it bash
      ```
      {: pre}

   2. マウント・パスにナビゲートし、ファイルのアクセス権をリストします。
      ```
      cd test && ls -al
      ```
      {: pre}

      出力例:
      ```
      -rwxrwx--- 1 <nonroot_userID> root 6193 Aug 21 17:06 <file_name>
      ```
      {: screen}

6. `test-permission` ポッドを削除します。
   ```
   kubectl delete pod test-permission
   ```
   {: pre}

7. 非 root ユーザーとしてアプリに PVC をマウントします。

   デプロイメント YAML で同時に `fsGroup` を設定することなく、非 root ユーザーを `runAsUser` として定義します。 `fsGroup` を設定すると、ポッドのデプロイ時に、{{site.data.keyword.cos_full_notm}} プラグインによるバケット内のすべてのファイルのグループ権限の更新がトリガーされます。 権限の更新は書き込み操作であり、これによってポッドが `Running` 状態にならない場合があります。
   {: important}

{{site.data.keyword.cos_full_notm}} サービス・インスタンスで正しいファイルのアクセス権を設定した後は、コンソールまたは REST API を使用してファイルをアップロードしないでください。 {{site.data.keyword.cos_full_notm}} プラグインを使用して、サービス・インスタンスにファイルを追加します。
{: tip}

<br />


   
## 許可がないことが原因で PVC の作成が失敗する
{: #missing_permissions}

{: tsSymptoms}
PVC を作成した場合に、PVC が保留中のままになります。`kubectl describe pvc <pvc_name>` を実行すると、以下のようなエラー・メッセージが表示されます。 

```
User doesn't have permissions to create or manage Storage
```
{: screen}

{: tsCauses}
クラスターの `storage-secret-store` Kubernetes シークレットに保管されている IAM API キーまたは IBM Cloud インフラストラクチャー (SoftLayer) API キーに、永続ストレージをプロビジョンするために必要な許可の一部がありません。 

{: tsResolve}
1. クラスターの `storage-secret-store` Kubernetes シークレットに保管されている IAM キーまたは IBM Cloud インフラストラクチャー (SoftLayer) API キーを取得して、正しい API キーが使用されていることを確認します。 
   ```
   kubectl get secret storage-secret-store -n kube-system -o yaml | grep slclient.toml: | awk '{print $2}' | base64 --decode
   ```
   {: pre}
   
   出力例: 
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
   
   CLI 出力の `Bluemix.iam_api_key` セクションに、IAM API キーがリストされます。同時に `Softlayer.softlayer_api_key` が空の場合は、IAM API キーを使用してインフラストラクチャー許可が決定します。IAM API キーは、リソース・グループおよび地域の IAM の**管理者**プラットフォーム役割を必要とする最初のアクションを実行するユーザーによって自動的に設定されます。`Softlayer.softlayer_api_key` に別の API キーが設定されている場合、このキーが IAM API キーよりも優先されます。`Softlayer.softlayer_api_key` は、クラスター管理者が `ibmcloud ks credentials-set` コマンドを実行すると設定されます。 
   
2. 資格情報を変更する場合は、使用される API キーを更新します。 
    1.  IAM API キーを更新するには、`ibmcloud ks api-key-reset` [コマンド](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_api_key_reset)を使用します。IBM Cloud インフラストラクチャー (SoftLayer) のキーを更新する場合は、`ibmcloud ks credential-set` [コマンド](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_set)を使用します。
    2. `storage-secret-store` Kubernetes シークレットが更新されるまで約 10 分から 15 分待機して、キーが更新されたことを確認します。
       ```
       kubectl get secret storage-secret-store -n kube-system -o yaml | grep slclient.toml: | awk '{print $2}' | base64 --decode
       ```
       {: pre}
   
3. API キーが正しい場合は、キーに永続ストレージをプロビジョンするための正しい許可があることを確認します。
   1. API キーの許可を確認のために、アカウント所有者に連絡します。 
   2. アカウント所有者として、{{site.data.keyword.Bluemix_notm}} コンソールのナビゲーションから**「管理」**>**「アクセス (IAM)」**を選択します。
   3. **「ユーザー」**を選択し、使用する API キーを持つユーザーを確認します。 
   4. アクション・メニューから、**「ユーザーの詳細の管理」**を選択します。 
   5. **「クラシック・インフラストラクチャー」**タブに移動します。 
   6. **「アカウント」**カテゴリーを展開し、**「ストレージの追加/アップグレード (StorageLayer)」**の許可が割り当てられていることを確認します。 
   7. **「サービス」**カテゴリーを展開し、**「ストレージ管理」**の許可が割り当てられていることを確認します。 
4. 失敗した PVC を削除します。 
   ```
   kubectl delete pvc <pvc_name>
   ```
   {: pre}
   
5. PVC を再作成します。 
   ```
   kubectl apply -f pvc.yaml
   ```
   {: pre}


## ヘルプとサポートの取得
{: #storage_getting_help}

まだクラスターに問題がありますか?
{: shortdesc}

-  `ibmcloud` CLI およびプラグインの更新が使用可能になると、端末に通知が表示されます。 使用可能なすべてのコマンドおよびフラグを使用できるように、CLI を最新の状態に保つようにしてください。
-   {{site.data.keyword.Bluemix_notm}} が使用可能かどうかを確認するために、[{{site.data.keyword.Bluemix_notm}} 状況ページ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン") を確認します](https://cloud.ibm.com/status?selected=status)。
-   [{{site.data.keyword.containerlong_notm}} Slack ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://ibm-container-service.slack.com) に質問を投稿します。
    {{site.data.keyword.Bluemix_notm}} アカウントに IBM ID を使用していない場合は、この Slack への[招待を要求](https://bxcs-slack-invite.mybluemix.net/)してください。
    {: tip}
-   フォーラムを確認して、同じ問題が他のユーザーで起こっているかどうかを調べます。 フォーラムを使用して質問するときは、{{site.data.keyword.Bluemix_notm}} 開発チームの目に止まるように、質問にタグを付けてください。
    -   {{site.data.keyword.containerlong_notm}} を使用したクラスターまたはアプリの開発やデプロイに関する技術的な質問がある場合は、[Stack Overflow![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://stackoverflow.com/questions/tagged/ibm-cloud+containers) に質問を投稿し、`ibm-cloud`、`kubernetes`、`containers` のタグを付けてください。
    -   サービスや概説の説明について質問がある場合は、[IBM Developer Answers Answers ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix) フォーラムを使用してください。 `ibm-cloud` と `containers` のタグを含めてください。
    フォーラムの使用について詳しくは、[ヘルプの取得](/docs/get-support?topic=get-support-getting-customer-support#using-avatar)を参照してください。
-   ケースを開いて、IBM サポートに連絡してください。 IBM サポート・ケースを開く方法や、サポート・レベルとケースの重大度については、[サポートへのお問い合わせ](/docs/get-support?topic=get-support-getting-customer-support)を参照してください。
問題を報告する際に、クラスター ID も報告してください。 クラスター ID を取得するには、`ibmcloud ks clusters` を実行します。 また、[{{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool](/docs/containers?topic=containers-cs_troubleshoot#debug_utility) を使用して、クラスターから関連情報を収集してエクスポートし、IBM サポートと情報を共有することができます。
{: tip}

