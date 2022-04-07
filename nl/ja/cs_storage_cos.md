---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-03"

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


# IBM Cloud Object Storage へのデータの保管
{: #object_storage}

[{{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-about) は、{{site.data.keyword.cos_full_notm}} プラグインを使用することで Kubernetes クラスター内で実行されているアプリにマウントできる永続的な高可用性ストレージです。 このプラグインは、Cloud {{site.data.keyword.cos_short}} バケットをクラスター内のポッドに接続する Kubernetes Flex-Volume プラグインです。 {{site.data.keyword.cos_full_notm}} を使用して保管される情報は、暗号化された状態で転送および保存され、複数の地理的位置にまたがって分散されて、REST API を使用して HTTP 経由でアクセスされます。
{: shortdesc}

{{site.data.keyword.cos_full_notm}} に接続するには、ご使用のクラスターは {{site.data.keyword.Bluemix_notm}} Identity and Access Management から認証を受けるためにパブリック・ネットワークにアクセスできる必要があります。 プライベート専用クラスターを使用している場合は、{{site.data.keyword.cos_full_notm}} のプライベート・サービス・エンドポイントと通信するには、プラグイン・バージョン `1.0.3` 以降をインストールして、{{site.data.keyword.cos_full_notm}} サービス・インスタンスを HMAC 認証に対応するようにセットアップする必要があります。 HMAC 認証の使用を希望しない場合は、このプラグインがプライベート・クラスター内で適切に機能するために、ポート 443 上のすべてのアウトバウンド・ネットワーク・トラフィックを開放する必要があります。
{: important}

バージョン 1.0.5 では、{{site.data.keyword.cos_full_notm}} プラグインの名前が `ibmcloud-object-storage-plugin` から `ibm-object-storage-plugin` に変更されました。 新しいバージョンのプラグインをインストールするには、[古い Helm チャート・インストールをアンインストール](#remove_cos_plugin)し、[{{site.data.keyword.cos_full_notm}} プラグインの新しいバージョンで Helm チャートを再インストール](#install_cos)する必要があります。
{: note}

## オブジェクト・ストレージ・サービス・インスタンスの作成
{: #create_cos_service}

クラスターでオブジェクト・ストレージの使用を開始する前に、アカウントに {{site.data.keyword.cos_full_notm}} サービス・インスタンスをプロビジョンする必要があります。
{: shortdesc}

{{site.data.keyword.cos_full_notm}} プラグインは、任意の s3 API エンドポイントで動作するように構成されています。 例えば、希望に応じて、[Minio](https://cloud.ibm.com/kubernetes/solutions/helm-charts/ibm-charts/ibm-minio-objectstore) などのローカル Cloud Object Storage サーバーを使用することも、{{site.data.keyword.cos_full_notm}} サービス・インスタンスを使用する代わりに、異なるクラウド・プロバイダーでセットアップした s3 API エンドポイントに接続することもできます。

以下の手順に従って {{site.data.keyword.cos_full_notm}} サービス・インスタンスを作成します。 ローカル Cloud Object Storage サーバーまたは異なる s3 API エンドポイントを使用する予定の場合は、プロバイダーの資料を参照して、Cloud Object Storage インスタンスをセットアップしてください。

1. {{site.data.keyword.cos_full_notm}} サービス・インスタンスをデプロイします。
   1.  [{{site.data.keyword.cos_full_notm}} カタログ・ページ](https://cloud.ibm.com/catalog/services/cloud-object-storage)を開きます。
   2.  サービス・インスタンスの名前 (`cos-backup` など) を入力し、クラスターが属するリソース・グループを選択します。 ご使用のクラスターのリソース・グループを表示するには、`ibmcloud ks cluster-get --cluster <cluster_name_or_ID>` を実行します。   
   3.  [プランのオプション ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api) で料金情報を確認し、プランを選択します。
   4.  **「作成」**をクリックします。 サービス詳細ページが開きます。
2. {: #service_credentials}{{site.data.keyword.cos_full_notm}} サービス資格情報を取得します。
   1.  サービス詳細ページのナビゲーションで、**「サービス資格情報」**をクリックします。
   2.  **「新規資格情報」**をクリックします。 ダイアログ・ボックスが表示されます。
   3.  資格情報に対する名前を入力します。
   4.  **「役割」**ドロップダウンから、`Writer` または `Manager` を選択します。 `Reader` を選択した場合、資格情報を使用して {{site.data.keyword.cos_full_notm}} にバケットを作成してデータを書き込むことはできません。
   5.  オプション: **「インラインの構成パラメーターの追加 (オプション)」**で、`{"HMAC":true}` と入力して、{{site.data.keyword.cos_full_notm}} サービスの追加の HMAC 資格情報を作成します。 HMAC 認証は、期限切れになったかまたはランダムに作成された OAuth2 トークンの誤用を防ぐことによって、OAuth2 認証にセキュリティー・レイヤーを追加します。 **重要**: パブリック・アクセスに対応していないプライベート専用クラスターを使用している場合は、プライベート・ネットワーク経由で {{site.data.keyword.cos_full_notm}} サービスにアクセス可能にするために、HMAC 認証を使用する必要があります。
   6.  **「追加 (Add)」**をクリックします。 新しい資格情報が**「サービス資格情報」**の表にリストされます。
   7.  **「資格情報の表示」**をクリックします。
   8.  {{site.data.keyword.cos_full_notm}} サービスで認証を受けるための OAuth2 トークンを使用する **apikey** をメモします。 HMAC 認証の場合は、**cos_hmac_keys** セクションにある **access_key_id** と **secret_access_key** をメモします。
3. [サービス資格情報をクラスター内の Kubernetes シークレットに保管](#create_cos_secret)して、{{site.data.keyword.cos_full_notm}} サービス・インスタンスへのアクセスを有効にします。

## オブジェクト・ストレージ・サービス資格情報用のシークレットの作成
{: #create_cos_secret}

データの読み取りおよび書き込みを行うために {{site.data.keyword.cos_full_notm}} サービス・インスタンスにアクセスするには、サービス資格情報を Kubernetes シークレットに安全に保管する必要があります。 {{site.data.keyword.cos_full_notm}} プラグインでは、バケットに対する読み取り操作または書き込み操作ごとに、これらの資格情報が使用されます。
{: shortdesc}

以下の手順に従って、{{site.data.keyword.cos_full_notm}} サービス・インスタンスの資格情報用の Kubernetes シークレットを作成します。 ローカル Cloud Object Storage サーバーまたは異なる s3 API エンドポイントを使用する予定の場合は、適切な資格情報を使用して Kubernetes シークレットを作成してください。

開始前に、以下のことを行います。 [アカウントにログインします。 該当する場合は、適切なリソース・グループをターゲットにします。 クラスターのコンテキストを設定します。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1. [{{site.data.keyword.cos_full_notm}} サービス資格情報](#service_credentials)の **apikey**、または **access_key_id** と **secret_access_key** を取得します。

2. {{site.data.keyword.cos_full_notm}} サービス・インスタンスの **GUID** を取得します。
   ```
   ibmcloud resource service-instance <service_name> | grep GUID
   ```
   {: pre}

3. サービス資格情報を保管する Kubernetes シークレットを作成します。 シークレットを作成すると、すべての値が自動的に base64 にエンコードされます。

   **API キーの使用例:**
   ```
   kubectl create secret generic cos-write-access --type=ibm/ibmc-s3fs --from-literal=api-key=<api_key> --from-literal=service-instance-id=<service_instance_guid>
   ```
   {: pre}

   **HMAC 認証の例:**
   ```
   kubectl create secret generic cos-write-access --type=ibm/ibmc-s3fs --from-literal=access-key=<access_key_ID> --from-literal=secret-key=<secret_access_key>    
   ```
   {: pre}

   <table>
   <caption>コマンドの構成要素について</caption>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> コマンドの構成要素について</th>
   </thead>
   <tbody>
   <tr>
   <td><code>api-key</code></td>
   <td>以前に {{site.data.keyword.cos_full_notm}} サービス資格情報から取得した API キーを入力します。 HMAC 認証を使用する場合は、代わりに <code>access-key</code> および <code>secret-key</code> を指定します。  </td>
   </tr>
   <tr>
   <td><code>access-key</code></td>
   <td>以前に {{site.data.keyword.cos_full_notm}} サービス資格情報から取得したアクセス・キー ID を入力します。 OAuth2 認証を使用する場合は、代わりに <code>api-key</code> を指定します。  </td>
   </tr>
   <tr>
   <td><code>secret-key</code></td>
   <td>以前に {{site.data.keyword.cos_full_notm}} サービス資格情報から取得したシークレット・アクセス・キーを入力します。 OAuth2 認証を使用する場合は、代わりに <code>api-key</code> を指定します。</td>
   </tr>
   <tr>
   <td><code>service-instance-id</code></td>
   <td>以前に取得した {{site.data.keyword.cos_full_notm}} サービス・インスタンスの GUID を入力します。 </td>
   </tr>
   </tbody>
   </table>

4. 名前空間内にシークレットが作成されたことを確認します。
   ```
   kubectl get secret
   ```
   {: pre}

5. [{{site.data.keyword.cos_full_notm}} プラグインをインストール](#install_cos)します。プラグインをインストール済みの場合は、{{site.data.keyword.cos_full_notm}} バケットの[構成を決定]( #configure_cos)します。

## IBM Cloud Object Storage プラグインのインストール
{: #install_cos}

{{site.data.keyword.cos_full_notm}} 用に事前定義されたストレージ・クラスをセットアップするには、{{site.data.keyword.cos_full_notm}} プラグインと Helm チャートをインストールします。 これらのストレージ・クラスを使用すると、アプリ用に {{site.data.keyword.cos_full_notm}} をプロビジョンするための PVC を作成できます。
{: shortdesc}

{{site.data.keyword.cos_full_notm}} プラグインを更新または削除する方法の説明をお探しですか? [IBM Cloud Object Storage プラグインの更新](#update_cos_plugin)と [IBM Cloud Object Storage プラグインの削除](#remove_cos_plugin)を参照してください。
{: tip}

開始前に、以下のことを行います。 [アカウントにログインします。 該当する場合は、適切なリソース・グループをターゲットにします。 クラスターのコンテキストを設定します。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1. ワーカー・ノードで、ご使用のマイナー・バージョンに対する最新パッチが適用されていることを確認します。
   1. ワーカー・ノードの現在のパッチ・バージョンをリストします。
      ```
      ibmcloud ks workers --cluster <cluster_name_or_ID>
      ```
      {: pre}

      出力例:
      ```
      OK
      ID                                                  Public IP        Private IP     Machine Type           State    Status   Zone    Version
      kube-dal10-crb1a23b456789ac1b20b2nc1e12b345ab-w26   169.xx.xxx.xxx    10.xxx.xx.xxx   b3c.4x16.encrypted     normal   Ready    dal10   1.13.6_1523*
      ```
      {: screen}

      ワーカー・ノードで、最新のパッチ・バージョンが適用されていない場合は、CLI 出力の **Version** 列にアスタリスク (`*`) が表示されます。

   2. [version changelog](/docs/containers?topic=containers-changelog#changelog) を参照して、最新のパッチ・バージョンに含まれる変更内容を確認してください。

   3. ワーカー・ノードを再ロードして、最新のパッチ・バージョンを適用します。 [ibmcloud ks worker-reload コマンド](/docs/containers?topic=containers-cs_cli_reference#cs_worker_reload)の説明に従って、ワーカー・ノード上で実行されているポッドを安全な方法でスケジュール変更した後に、ワーカー・ノードを再ロードしてください。 再ロード中に、ワーカー・ノード・マシンが最新のイメージで更新されるので、[ワーカー・ノードの外部に保管](/docs/containers?topic=containers-storage_planning#persistent_storage_overview)していないデータは削除されることに注意してください。

2.  {{site.data.keyword.cos_full_notm}} プラグインを Helm サーバー (Tiller) を使用してインストールするか、使用せずにインストールするかを選択します。 次に、[手順に従って](/docs/containers?topic=containers-helm#public_helm_install)、ローカル・マシン上に Helm クライアントをインストールし、Tiller を使用する場合には、サービス・アカウントで Tiller をクラスター内にインストールします。

3. Tiller を使用してプラグインをインストールする場合は、Tiller がサービス・アカウントでインストールされていることを確認します。
   ```
   kubectl get serviceaccount -n kube-system tiller
   ```
   {: pre}
   
   出力例:
   ```
   NAME                                 SECRETS   AGE
    tiller                               1         2m
   ```
   {: screen}

4. {{site.data.keyword.Bluemix_notm}} Helm リポジトリーをクラスターに追加します。
   ```
   helm repo add iks-charts https://icr.io/helm/iks-charts
   ```
   {: pre}

4. Helm リポジトリーを更新して、このリポジトリーにあるすべての Helm チャートの最新バージョンを取得します。
   ```
   helm repo update
   ```
   {: pre}

5. Helm チャートをダウンロードして、現行ディレクトリーにチャートをアンパックします。
   ```
   helm fetch --untar iks-charts/ibm-object-storage-plugin
   ```
   {: pre}

7. OS X または Linux ディストリビューションを使用する場合は、{{site.data.keyword.cos_full_notm}} Helm プラグイン `ibmc` をインストールします。 このプラグインは、クラスターの場所を自動的に取得し、ストレージ・クラスの {{site.data.keyword.cos_full_notm}} バケットに API エンドポイントを設定するために使用されます。 Windows をオペレーティング・システムとして使用する場合は、次のステップに進みます。
   1. Helm プラグインをインストールします。
      ```
      helm plugin install ./ibm-object-storage-plugin/helm-ibmc
      ```
      {: pre}

      出力例:
      ```
      Installed plugin: ibmc
      ```
      {: screen}
      
      エラー `Error: plugin already exists` が表示された場合は、`rm -rf ~/.helm/plugins/helm-ibmc` を実行して、`ibmc` Helm プラグインを削除します。
      {: tip}

   2. `ibmc` プラグインが正常にインストールされていることを確認します。
      ```
      helm ibmc --help
      ```
      {: pre}

      出力例:
      ```
      IBM K8S Service (IKS) および IBM Cloud Private (ICP) で Helm チャートをインストールまたはアップグレードします。

      Available Commands:
         helm ibmc install [CHART][flags]                      Install a Helm chart
         helm ibmc upgrade [RELEASE][CHART] [flags]            Upgrade the release to a new version of the Helm chart
         helm ibmc template [CHART][flags] [--apply|--delete]  Install/uninstall a Helm chart without tiller

      Available Flags:
         -h, --help                    (Optional) This text.
         -u, --update                  (Optional) Update this plugin to the latest version

      Example Usage:
         With Tiller:
             Install:   helm ibmc install iks-charts/ibm-object-storage-plugin --name ibm-object-storage-plugin
         Without Tiller:
             Install:   helm ibmc template iks-charts/ibm-object-storage-plugin --apply
             Dry-run:   helm ibmc template iks-charts/ibm-object-storage-plugin
             Uninstall: helm ibmc template iks-charts/ibm-object-storage-plugin --delete

      注:
         1. 常に、最新バージョンの ibm-object-storage-plugin チャートをインストールすることをお勧めします。
         2. 常に、「kubectl」クライアントを最新の状態にしておくことをお勧めします。
      ```
      {: screen}

      出力で `Error: fork/exec /home/iksadmin/.helm/plugins/helm-ibmc/ibmc.sh: permission denied` というエラーが表示される場合は、`chmod 755 ~/.helm/plugins/helm-ibmc/ibmc.sh` を実行します。 その後、`helm ibmc --help` を再実行します。
      {: tip}

8. オプション: {{site.data.keyword.cos_full_notm}} サービス資格情報を保持する Kubernetes シークレットのみにアクセスするように {{site.data.keyword.cos_full_notm}} プラグインを制限します。 デフォルトでは、このプラグインは、クラスター内のすべての Kubernetes シークレットへのアクセスが許可されています。
   1. [{{site.data.keyword.cos_full_notm}} サービス・インスタンスを作成します](#create_cos_service)。
   2. [{{site.data.keyword.cos_full_notm}} サービス資格情報を Kubernetes シークレットに保管します](#create_cos_secret)。
   3. `templates` ディレクトリーにナビゲートし、使用可能なファイルをリストします。  
      ```
      cd ibm-object-storage-plugin/templates && ls
      ```
      {: pre}

   4. `provisioner-sa.yaml` ファイルを開き、`ibmcloud-object-storage-secret-reader` `ClusterRole` 定義を探します。
   6. `resourceNames` セクションで、以前に作成したシークレットの名前を、プラグインがアクセスを許可されたシークレットのリストに追加します。
      ```
      kind: ClusterRole
      apiVersion: rbac.authorization.k8s.io/v1beta1
      metadata:
        name: ibmcloud-object-storage-secret-reader
      rules:
      - apiGroups: [""]
        resources: ["secrets"]
        resourceNames: ["<secret_name1>","<secret_name2>"]
        verbs: ["get"]
      ```
      {: codeblock}
   7. 変更を保存します。

9. {{site.data.keyword.cos_full_notm}} プラグインをインストールします。 このプラグインをインストールすると、事前定義されたストレージ・クラスがクラスターに追加されます。

   - **OS X および Linux の場合:**
     - 前述のステップをスキップした場合は、特定の Kubernetes シークレットに制限せずにインストールします。</br>
       **Tiller なし**:
       ```
       helm ibmc template iks-charts/ibm-object-storage-plugin --apply
       ```
       {: pre}
       
       **Tiller あり**:
       ```
       helm ibmc install iks-charts/ibm-object-storage-plugin --name ibm-object-storage-plugin
       ```
       {: pre}

     - 前述のステップを完了した場合は、特定の Kubernetes シークレットに制限してインストールします。</br>
       **Tiller なし**:
       ```
       cd ../..
       helm ibmc template ./ibm-object-storage-plugin --apply 
       ```
       {: pre}
       
       **Tiller あり**:
       ```
       cd ../..
       helm ibmc install ./ibm-object-storage-plugin --name ibm-object-storage-plugin
       ```
       {: pre}

   - **Windows の場合は、以下のようにします。**
     1. クラスターのデプロイ先であるゾーンを取得し、そのゾーンを環境変数に格納します。
        ```
        export DC_NAME=$(kubectl get cm cluster-info -n kube-system -o jsonpath='{.data.cluster-config\.json}' | grep datacenter | awk -F ': ' '{print $2}' | sed 's/\"//g' |sed 's/,//g')
        ```
        {: pre}

     2. 環境変数が設定されていることを確認します。
        ```
        printenv
        ```
        {: pre}

     3. Helm チャートをインストールします。
        - 前述のステップをスキップした場合は、特定の Kubernetes シークレットに制限せずにインストールします。</br>
          **Tiller なし**:
          ```
          helm ibmc template iks-charts/ibm-object-storage-plugin --apply
          ```
          {: pre}
          
          **Tiller あり**:
          ```
          helm ibmc install iks-charts/ibm-object-storage-plugin --name ibm-object-storage-plugin
          ```
          {: pre}

        - 前述のステップを完了した場合は、特定の Kubernetes シークレットに制限してインストールします。</br>
          **Tiller なし**:
          ```
          cd ../..
          helm ibmc template ./ibm-object-storage-plugin --apply 
          ```
          {: pre}
          
          **Tiller あり**:
          ```
          cd ../..
          helm ibmc install ./ibm-object-storage-plugin --name ibm-object-storage-plugin
          ```
          {: pre}


   Tiller なしでインストールする場合の出力例:
   ```
   Rendering the Helm chart templates...
   DC: dal10
   Chart: iks-charts/ibm-object-storage-plugin
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-cold-cross-region.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-cold-regional.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-flex-cross-region.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-flex-perf-cross-region.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-flex-perf-regional.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-flex-regional.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-standard-cross-region.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-standard-perf-cross-region.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-standard-perf-regional.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-standard-regional.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-vault-cross-region.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-vault-regional.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/flex-driver-sa.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/provisioner-sa.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/flex-driver.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/tests/check-driver-install.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/provisioner.yaml
   Installing the Helm chart...
   serviceaccount/ibmcloud-object-storage-driver created
   daemonset.apps/ibmcloud-object-storage-driver created
   storageclass.storage.k8s.io/ibmc-s3fs-cold-cross-region created
   storageclass.storage.k8s.io/ibmc-s3fs-cold-regional created
   storageclass.storage.k8s.io/ibmc-s3fs-flex-cross-region created
   storageclass.storage.k8s.io/ibmc-s3fs-flex-perf-cross-region created
   storageclass.storage.k8s.io/ibmc-s3fs-flex-perf-regional created
   storageclass.storage.k8s.io/ibmc-s3fs-flex-regional created
   storageclass.storage.k8s.io/ibmc-s3fs-standard-cross-region created
   storageclass.storage.k8s.io/ibmc-s3fs-standard-perf-cross-region created
   storageclass.storage.k8s.io/ibmc-s3fs-standard-perf-regional created
   storageclass.storage.k8s.io/ibmc-s3fs-standard-regional created
   storageclass.storage.k8s.io/ibmc-s3fs-vault-cross-region created
   storageclass.storage.k8s.io/ibmc-s3fs-vault-regional created
   serviceaccount/ibmcloud-object-storage-plugin created
   clusterrole.rbac.authorization.k8s.io/ibmcloud-object-storage-plugin created
   clusterrole.rbac.authorization.k8s.io/ibmcloud-object-storage-secret-reader created
   clusterrolebinding.rbac.authorization.k8s.io/ibmcloud-object-storage-plugin created
   clusterrolebinding.rbac.authorization.k8s.io/ibmcloud-object-storage-secret-reader created
   deployment.apps/ibmcloud-object-storage-plugin created
   pod/ibmcloud-object-storage-driver-test created
   ```
   {: screen}

10. プラグインが正しくインストールされていることを確認します。
    ```
    kubectl get pod -n kube-system -o wide | grep object
    ```
    {: pre}

    出力例:
    ```
    ibmcloud-object-storage-driver-9n8g8                              1/1       Running   0          2m
   ibmcloud-object-storage-plugin-7c774d484b-pcnnx                   1/1       Running   0          2m
    ```
    {: screen}

    1 つの `ibmcloud-object-storage-plugin` ポッドと 1 つ以上の `ibmcloud-object-storage-driver` ポッドが表示されたら、インストールは成功しています。 `ibmcloud-object-storage-driver` ポッドの数は、クラスター内のワーカー・ノードの数と等しくなります。 プラグインが正しく機能するためには、すべてのポッドが `Running` 状態である必要があります。 ポッドが失敗した場合は、障害の根本原因を見つけるために `kubectl describe pod -n kube-system <pod_name>` を実行してください。

11. ストレージ・クラスが正常に作成されたことを確認します。
    ```
    kubectl get storageclass | grep s3
    ```
    {: pre}

    出力例:
    ```
    ibmc-s3fs-cold-cross-region            ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-cold-regional                ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-flex-cross-region            ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-flex-perf-cross-region       ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-flex-perf-regional           ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-flex-regional                ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-standard-cross-region        ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-standard-perf-cross-region   ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-standard-perf-regional       ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-standard-regional            ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-vault-cross-region           ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-vault-regional               ibm.io/ibmc-s3fs   8m
    ```
    {: screen}

12. {{site.data.keyword.cos_full_notm}} バケットにアクセスするすべてのクラスターに対してこのステップを繰り返します。

### IBM Cloud Object Storage プラグインの更新
{: #update_cos_plugin}

既存の {{site.data.keyword.cos_full_notm}} プラグインを最新バージョンにアップグレードできます。
{: shortdesc}

1. 以前に `ibmcloud-object-storage-plugin` という名前のバージョン 1.0.4 以前の Helm チャートをインストールした場合は、この Helm インストールをクラスターから削除します。 次に、Helm チャートを再インストールします。
   1. 古いバージョンの {{site.data.keyword.cos_full_notm}} Helm チャートがクラスターにインストールされているかどうかを確認します。  
      ```
      helm ls | grep ibmcloud-object-storage-plugin
      ```
      {: pre}

      出力例:
      ```
      ibmcloud-object-storage-plugin	1       	Mon Sep 18 15:31:40 2017	DEPLOYED	ibmcloud-object-storage-plugin-1.0.4	default
      ```
      {: screen}

   2. `ibmcloud-object-storage-plugin` という名前のバージョン 1.0.4 以前の Helm チャートが存在する場合は、クラスターから Helm チャートを削除します。 `ibm-object-storage-plugin` という名前のバージョン 1.0.5 以降の Helm チャートが存在する場合は、ステップ 2 に進みます。
      ```
      helm delete --purge ibmcloud-object-storage-plugin
      ```
      {: pre}

   3. [{{site.data.keyword.cos_full_notm}} プラグインのインストール](#install_cos)の手順に従って、最新バージョンの {{site.data.keyword.cos_full_notm}} プラグインをインストールします。

2. {{site.data.keyword.Bluemix_notm}} Helm リポジトリーを更新して、このリポジトリーにあるすべての Helm チャートの最新バージョンを取得します。
   ```
   helm repo update
   ```
   {: pre}

3. OS X または Linux ディストリビューションを使用する場合は、{{site.data.keyword.cos_full_notm}} `ibmc` Helm プラグインを最新バージョンに更新します。
   ```
   helm ibmc --update
   ```
   {: pre}

4. 最新の {{site.data.keyword.cos_full_notm}} Helm チャートをローカル・マシンにダウンロードしてパッケージを解凍して、`release.md` ファイルを参照して最新のリリース情報を確認します。
   ```
   helm fetch --untar iks-charts/ibm-object-storage-plugin
   ```
   {: pre}

5. プラグインをアップグレードします。 </br>
   **Tiller なし**: 
   ```
   helm ibmc template iks-charts/ibm-object-storage-plugin --update
   ```
   {: pre}
     
   **Tiller あり**: 
   1. Helm チャートのインストール名を検索します。
      ```
      helm ls | grep ibm-object-storage-plugin
      ```
      {: pre}

      出力例:
      ```
      <helm_chart_name> 	1       	Mon Sep 18 15:31:40 2017	DEPLOYED	ibm-object-storage-plugin-1.0.5	default
      ```
      {: screen}

   2. {{site.data.keyword.cos_full_notm}} Helm チャートを最新バージョンにアップグレードします。
      ```   
      helm ibmc upgrade <helm_chart_name> iks-charts/ibm-object-storage-plugin --force --recreate-pods -f
      ```
      {: pre}

6. `ibmcloud-object-storage-plugin` が正常にアップグレードされたことを確認します。  
   ```
   kubectl rollout status deployment/ibmcloud-object-storage-plugin -n kube-system
   ```
   {: pre}

   CLI 出力で `deployment "ibmcloud-object-storage-plugin" successfully rolled out` と表示された場合、プラグインのアップグレードは成功しています。

7. `ibmcloud-object-storage-driver` が正常にアップグレードされたことを確認します。
   ```
   kubectl rollout status ds/ibmcloud-object-storage-driver -n kube-system
   ```
   {: pre}

   CLI 出力で `daemon set "ibmcloud-object-storage-driver" successfully rolled out` と表示された場合、アップグレードは成功しています。

8. {{site.data.keyword.cos_full_notm}} ポッドが `Running` 状態であることを確認します。
   ```
   kubectl get pods -n kube-system -o wide | grep object-storage
   ```
   {: pre}


### IBM Cloud Object Storage プラグインの削除
{: #remove_cos_plugin}

クラスターで {{site.data.keyword.cos_full_notm}} をプロビジョンして使用する必要がない場合は、プラグインをアンインストールできます。
{: shortdesc}

このプラグインを削除しても、既存の PVC、PV、データは削除されません。 このプラグインを削除すると、関連するすべてのポッドとデーモン・セットがクラスターから削除されます。 プラグインを削除した後では、{{site.data.keyword.cos_full_notm}} API を直接使用するようにアプリを構成しない限り、クラスターで新しい {{site.data.keyword.cos_full_notm}} をプロビジョンしたり、既存の PVC および PV を使用したりすることはできません。
{: important}

開始前に、以下のことを行います。

- [CLI のターゲットを自分のクラスターに設定します](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)。
- {{site.data.keyword.cos_full_notm}} を使用する PVC と PV がクラスターにないことを確認してください。 特定の PVC をマウントするすべてのポッドをリストするには、`kubectl get pods --all-namespaces -o=jsonpath='{range .items[*]}{"\n"}{.metadata.name}{":\t"}{range .spec.volumes[*]}{.persistentVolumeClaim.claimName}{" "}{end}{end}' | grep "<pvc_name>"` を実行します。

プラグインを削除するには、以下のようにします。

1. クラスターからプラグインを削除します。 </br>
   **Tiller あり**: 
   1. Helm チャートのインストール名を検索します。
      ```
      helm ls | grep object-storage-plugin
      ```
      {: pre}

      出力例:
      ```
      <helm_chart_name> 	1       	Mon Sep 18 15:31:40 2017	DEPLOYED	ibmcloud-object-storage-plugin-1.0.0	default
      ```
      {: screen}

   2. Helm チャートを削除することによって、{{site.data.keyword.cos_full_notm}} プラグインを削除します。
      ```
      helm delete --purge <helm_chart_name>
      ```
      {: pre}

   **Tiller なし**: 
   ```
   helm ibmc template iks-charts/ibm-object-storage-plugin --delete
   ```
   {: pre}

2. {{site.data.keyword.cos_full_notm}} ポッドが削除されたことを確認します。
   ```
   kubectl get pod -n kube-system | grep object-storage
   ```
   {: pre}

   CLI 出力にポッドが表示されていなければ、ポッドは正常に削除されています。

3. ストレージ・クラスが削除されたことを確認します。
   ```
   kubectl get storageclasses | grep s3
   ```
   {: pre}

   CLI 出力にストレージ・クラスが表示されていなければ、ストレージ・クラスは正常に削除されています。

4. OS X または Linux ディストリビューションを使用する場合は、`ibmc` Helm プラグインを削除します。 Windows を使用する場合、このステップは不要です。
   1. `ibmc` プラグインを削除します。
      ```
      rm -rf ~/.helm/plugins/helm-ibmc
      ```
      {: pre}

   2. `ibmc` プラグインが削除されたことを確認します。
      ```
      helm plugin list
      ```
      {: pre}

      出力例:
     ```
     NAME	VERSION	DESCRIPTION
     ```
     {: screen}

     CLI 出力に `ibmc` プラグインがリストされない場合、`ibmc` プラグインは正常に削除されています。


## オブジェクト・ストレージ構成の決定
{: #configure_cos}

{{site.data.keyword.containerlong_notm}} には、特定の構成でバケットを作成するために使用できる事前定義ストレージ・クラスが用意されています。
{: shortdesc}

1. {{site.data.keyword.containerlong_notm}} で使用可能なストレージ・クラスをリストします。
   ```
   kubectl get storageclasses | grep s3
   ```
   {: pre}

   出力例:
   ```
   ibmc-s3fs-cold-cross-region            ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-cold-regional                ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-flex-cross-region            ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-flex-perf-cross-region       ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-flex-perf-regional           ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-flex-regional                ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-standard-cross-region        ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-standard-perf-cross-region   ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-standard-perf-regional       ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-standard-regional            ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-vault-cross-region           ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-vault-regional               ibm.io/ibmc-s3fs   8m
   ```
   {: screen}

2. データ・アクセス要件に合ったストレージ・クラスを選択します。 ストレージ・クラスによって、バケットのストレージ容量、読み取り/書き込み操作、およびアウトバウンド帯域幅の[料金設定 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api) が決まります。 どのオプションが適切かは、サービス・インスタンスに対するデータの読み取りと書き込みの頻度に基づきます。
   - **Standard**: このオプションは、頻繁にアクセスされるホット・データに使用されます。 Web アプリやモバイル・アプリが一般的なユース・ケースです。
   - **Vault**: このオプションは、アクセス頻度の低い (月 1 回以下程度) ワークロードまたはクール・データに使用されます。 アーカイブ、短期データ保存、デジタル資産の保持、テープの交換、および災害復旧が一般的なユース・ケースです。
   - **Cold**: このオプションは、めったにアクセスされない (90 日に 1 回以下程度) コールド・データ、または非アクティブ・データに使用されます。 アーカイブ、長期バックアップ、コンプライアンスのために保持する履歴データ、またはめったにアクセスされないワークロードとアプリが一般的なユース・ケースです。
   - **Flex**: このオプションは、特定の使用パターンに従わない、または使用パターンを判別または予測するには大量すぎるワークロードおよびデータに使用されます。 **ヒント:** この[ブログ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/blogs/bluemix/2017/03/interconnect-2017-changing-rules-storage/) を参照し、従来のストレージ層と比較して Flex ストレージ・クラスがどのように機能するかを確認してください。   

3. バケットに格納されるデータの回復力のレベルを決定します。
   - **Cross-region**: このオプションを使用すると、地理位置情報内の 3 つの地域にまたがってデータが格納されるため、最大限の可用性が実現します。 複数の地域に分散されたワークロードがある場合、要求は最も近い地域のエンドポイントにルーティングされます。 地理位置情報の API エンドポイントは、クラスターの場所に基づいて、以前にインストールした `ibmc` Helm プラグインによって自動的に設定されます。 例えば、クラスターが `US South` にある場合、ストレージ・クラスはバケットの `US GEO` API エンドポイントを使用するように構成されます。 詳しくは、[地域とエンドポイント](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-endpoints#endpoints)を参照してください。  
   - **Regional**: このオプションを使用すると、データが 1 つの地域内の複数のゾーンに複製されます。 同じ地域に複数のワークロードがある場合は、Cross-region のセットアップよりも待ち時間が低くなり、パフォーマンスが向上します。 地域エンドポイントは、クラスターの場所に基づいて、以前にインストールした `ibm` Helm プラグインによって自動的に設定されます。 例えば、クラスターが `US South` にある場合、ストレージ・クラスはバケットの地域エンドポイントとして `US South` を使用するように構成されます。 詳しくは、[地域とエンドポイント](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-endpoints#endpoints)を参照してください。

4. ストレージ・クラスの詳細な {{site.data.keyword.cos_full_notm}} バケット構成を確認します。
   ```
   kubectl describe storageclass <storageclass_name>
   ```
   {: pre}

   出力例:
   ```
   Name:                  ibmc-s3fs-standard-cross-region
   IsDefaultClass:        No
   Annotations:           <none>
   Provisioner:           ibm.io/ibmc-s3fs
   Parameters:            ibm.io/chunk-size-mb=16,ibm.io/curl-debug=false,ibm.io/debug-level=warn,ibm.io/iam-endpoint=https://iam.bluemix.net,ibm.io/kernel-cache=true,ibm.io/multireq-max=20,ibm.io/object-store-endpoint=https://s3-api.dal-us-geo.objectstorage.service.networklayer.com,ibm.io/object-store-storage-class=us-standard,ibm.io/parallel-count=2,ibm.io/s3fs-fuse-retry-count=5,ibm.io/stat-cache-size=100000,ibm.io/tls-cipher-suite=AESGCM
   AllowVolumeExpansion:  <unset>
   MountOptions:          <none>
   ReclaimPolicy:         Delete
   VolumeBindingMode:     Immediate
   Events:                <none>
   ```
   {: screen}

   <table>
   <caption>ストレージ・クラスの詳細について</caption>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> YAML ファイルの構成要素について</th>
   </thead>
   <tbody>
   <tr>
   <td><code>ibm.io/chunk-size-mb</code></td>
   <td>{{site.data.keyword.cos_full_notm}} に対して読み取りまたは書き込みが行われるデータ・チャンクのサイズ (メガバイト単位)。 名前に <code>perf</code> が指定されたストレージ・クラスは、52 メガバイトでセットアップされます。 名前に <code>perf</code> が指定されていないストレージ・クラスは、16 メガバイトのチャンクを使用します。 例えば、1 GB のファイルを読み取る場合、プラグインは、このファイルを複数の 16 メガバイトまたは 52 メガバイトのチャンクで読み取ります。 </td>
   </tr>
   <tr>
   <td><code>ibm.io/curl-debug</code></td>
   <td>{{site.data.keyword.cos_full_notm}} サービス・インスタンスに送信される要求のロギングを有効にします。 有効にすると、ログが `syslog` に送信され、[ログを外部ロギング・サーバーに転送する](/docs/containers?topic=containers-health#logging)ことができます。 デフォルトでは、すべてのストレージ・クラスが <strong>false</strong> に設定され、このロギング機能が無効になっています。 </td>
   </tr>
   <tr>
   <td><code>ibm.io/debug-level</code></td>
   <td>{{site.data.keyword.cos_full_notm}} プラグインによって設定されるロギング・レベル。 すべてのストレージ・クラスは、<strong>WARN</strong> ロギング・レベルでセットアップされます。 </td>
   </tr>
   <tr>
   <td><code>ibm.io/iam-endpoint</code></td>
   <td>{{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM) の API エンドポイント。 </td>
   </tr>
   <tr>
   <td><code>ibm.io/kernel-cache</code></td>
   <td>ボリューム・マウント・ポイントのカーネル・バッファー・キャッシュを有効または無効にします。 有効にすると、{{site.data.keyword.cos_full_notm}} から読み取られるデータがカーネル・キャッシュに格納され、データへの高速読み取りアクセスが可能になります。 無効の場合、データはキャッシュされず、常に {{site.data.keyword.cos_full_notm}} から読み取られます。 カーネル・キャッシュは、<code>standard</code> および <code>flex</code> ストレージ・クラスに対しては有効で、<code>cold</code> および <code>vault</code> ストレージ・クラスに対しては無効です。 </td>
   </tr>
   <tr>
   <td><code>ibm.io/multireq-max</code></td>
   <td>1 つのディレクトリー内のファイルをリストするために {{site.data.keyword.cos_full_notm}} サービス・インスタンスに送信できる並行要求の最大数。 すべてのストレージ・クラスは、最大 20 個の並行要求でセットアップされます。  </td>
   </tr>
   <tr>
   <td><code>ibm.io/object-store-endpoint</code></td>
   <td>{{site.data.keyword.cos_full_notm}} サービス・インスタンス内のバケットにアクセスするために使用する API エンドポイント。 エンドポイントは、クラスターの地域に基づいて自動的に設定されます。 **注**: クラスターのある地域とは異なる地域の既存のバケットにアクセスする場合は、[カスタム・ストレージ・クラス](/docs/containers?topic=containers-kube_concepts#customized_storageclass)を作成して、バケットの API エンドポイントを使用する必要があります。</td>
   </tr>
   <tr>
   <td><code>ibm.io/object-store-storage-class</code></td>
   <td>ストレージ・クラスの名前。 </td>
   </tr>
   <tr>
   <td><code>ibm.io/parallel-count</code></td>
   <td>単一の読み取りまたは書き込み操作のために {{site.data.keyword.cos_full_notm}} サービス・インスタンスに送信できる並行要求の最大数。 名前に <code>perf</code> が指定されたストレージ・クラスは、最大 20 個の並行要求でセットアップされます。 <code>perf</code> が指定されていないストレージ・クラスは、デフォルトで 2 つの並行要求でセットアップされます。  </td>
   </tr>
   <tr>
   <td><code>ibm.io/s3fs-fuse-retry-count</code></td>
   <td>操作が失敗したと見なされるまでの読み取りまたは書き込み操作の最大再試行回数。 すべてのストレージ・クラスは、最大 5 回の再試行でセットアップされます。  </td>
   </tr>
   <tr>
   <td><code>ibm.io/stat-cache-size</code></td>
   <td>{{site.data.keyword.cos_full_notm}} メタデータ・キャッシュ内に保持されるレコードの最大数。 各レコードのサイズの上限は 0.5 キロバイトです。 すべてのストレージ・クラスで、レコードの最大数はデフォルトで 100000 に設定されています。 </td>
   </tr>
   <tr>
   <td><code>ibm.io/tls-cipher-suite</code></td>
   <td>{{site.data.keyword.cos_full_notm}} への接続が HTTPS エンドポイント経由で確立されるときに使用する必要がある TLS 暗号スイート。 暗号スイートの値は [OpenSSL 形式 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.openssl.org/docs/man1.0.2/apps/ciphers.html) に従う必要があります。 すべてのストレージ・クラスで、デフォルトで <strong><code>AESGCM</code></strong> 暗号スイートが使用されます。  </td>
   </tr>
   </tbody>
   </table>

   各ストレージ・クラスについて詳しくは、[ストレージ・クラス・リファレンス](#cos_storageclass_reference)を参照してください。 事前設定値のいずれかを変更する場合は、独自の[カスタマイズしたストレージ・クラス](/docs/containers?topic=containers-kube_concepts#customized_storageclass)を作成します。
   {: tip}

5. バケットの名前を決定します。 バケットの名前は、{{site.data.keyword.cos_full_notm}} において固有でなければなりません。 {{site.data.keyword.cos_full_notm}} プラグインによってバケットの名前を自動的に作成するように選択することもできます。 1 つのバケット内のデータを編成するために、サブディレクトリーを作成できます。

   以前に選択したストレージ・クラスによって、バケット全体の料金設定が決定します。 サブディレクトリーに対して異なるストレージ・クラスを定義することはできません。 異なるアクセス要件を持つデータを格納する場合は、複数の PVC を使用して複数のバケットを作成することを検討してください。
   {: note}

6. クラスターまたは永続ボリューム請求 (PVC) が削除された後もデータとバケットを保持するかどうかを選択します。 PVC を削除すると、必ず PV が削除されます。 PVC の削除時にデータとバケットを自動的に削除するかどうかを選択できます。 {{site.data.keyword.cos_full_notm}} サービス・インスタンスは、データに対して選択した保存ポリシーとは独立しているため、PVC の削除時に削除されることはありません。

これで、必要な構成を決定したので、[PVC を作成](#add_cos)して {{site.data.keyword.cos_full_notm}} をプロビジョンする準備ができました。

## アプリへのオブジェクト・ストレージの追加
{: #add_cos}

クラスターで {{site.data.keyword.cos_full_notm}} をプロビジョンするために、永続ボリューム請求 (PVC) を作成します。
{: shortdesc}

PVC で選択した設定に応じて、以下の方法で {{site.data.keyword.cos_full_notm}} をプロビジョンすることができます。
- [動的プロビジョニング](/docs/containers?topic=containers-kube_concepts#dynamic_provisioning): PVC を作成すると、{{site.data.keyword.cos_full_notm}} サービス・インスタンスの一致する永続ボリューム (PV) とバケットが自動的に作成されます。
- [静的プロビジョニング](/docs/containers?topic=containers-kube_concepts#static_provisioning): PVC の {{site.data.keyword.cos_full_notm}} サービス・インスタンスの既存のバケットを参照できます。 PVC を作成すると、一致する PV のみが自動的に作成され、{{site.data.keyword.cos_full_notm}} の既存のバケットにリンクされます。

開始前に、以下のことを行います。
- [{{site.data.keyword.cos_full_notm}} サービス・インスタンスを作成して準備します](#create_cos_service)。
- [{{site.data.keyword.cos_full_notm}} サービス資格情報を保管するシークレットを作成します](#create_cos_secret)。
- [{{site.data.keyword.cos_full_notm}} の構成を決定します](#configure_cos)。

クラスターに {{site.data.keyword.cos_full_notm}} を追加するには、以下のようにします。

1. 永続ボリューム請求 (PVC) を定義した構成ファイルを作成します。
   ```
   kind: PersistentVolumeClaim
   apiVersion: v1
   metadata:
     name: <pvc_name>
     namespace: <namespace>
     annotations:
       ibm.io/auto-create-bucket: "<true_or_false>"
       ibm.io/auto-delete-bucket: "<true_or_false>"
       ibm.io/bucket: "<bucket_name>"
       ibm.io/object-path: "<bucket_subdirectory>"
       ibm.io/secret-name: "<secret_name>"
       ibm.io/endpoint: "https://<s3fs_service_endpoint>"
   spec:
     accessModes:
       - ReadWriteOnce
     resources:
       requests:
         storage: 8Gi # Enter a fictitious value
     storageClassName: <storage_class>
   ```
   {: codeblock}

   <table>
   <caption>YAML ファイルの構成要素について</caption>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> YAML ファイルの構成要素について</th>
   </thead>
   <tbody>
   <tr>
   <td><code>metadata.name</code></td>
   <td>PVC の名前を入力します。</td>
   </tr>
   <tr>
   <td><code>metadata.namespace</code></td>
   <td>PVC を作成する名前空間を入力します。 PVC は、{{site.data.keyword.cos_full_notm}} サービス資格情報の Kubernetes シークレットを作成した、ポッドを実行する名前空間に作成する必要があります。 </td>
   </tr>
   <tr>
   <td><code>ibm.io/auto-create-bucket</code></td>
   <td>次のいずれかのオプションを選択します。 <ul><li><strong>true</strong>: PVC を作成すると、{{site.data.keyword.cos_full_notm}} サービス・インスタンスの PV とバケットが自動的に作成されます。 {{site.data.keyword.cos_full_notm}} サービス・インスタンスに新規バケットを作成するには、このオプションを選択します。 </li><li><strong>false</strong>: 既存のバケットのデータにアクセスする場合は、このオプションを選択します。 PVC を作成すると、PV が自動的に作成され、<code>ibm.io/bucket</code> で指定したバケットにリンクされます。</td>
   </tr>
   <tr>
   <td><code>ibm.io/auto-delete-bucket</code></td>
   <td>次のいずれかのオプションを選択します。 <ul><li><strong>true</strong>: データ、バケット、および PV が、PVC の削除時に自動的に削除されます。 {{site.data.keyword.cos_full_notm}} サービス・インスタンスは維持され、削除されません。 このオプションを <strong>true</strong> に設定する場合は、<code>ibm.io/auto-create-bucket: true</code> および <code>ibm.io/bucket: ""</code> を設定して、<code>tmp-s3fs-xxxx</code> の形式の名前でバケットが自動的に作成されるようにする必要があります。 </li><li><strong>false</strong>: PVC を削除すると、PV は自動的に削除されますが、{{site.data.keyword.cos_full_notm}} サービス・インスタンスのデータとバケットは維持されます。 データにアクセスするには、既存のバケットの名前を持つ新しい PVC を作成する必要があります。 </li></ul>
   <tr>
   <td><code>ibm.io/bucket</code></td>
   <td>次のいずれかのオプションを選択します。 <ul><li><code>ibm.io/auto-create-bucket</code> が <strong>true</strong> に設定されている場合: {{site.data.keyword.cos_full_notm}} に作成するバケットの名前を入力します。 さらに <code>ibm.io/auto-delete-bucket</code> が <strong>true</strong> に設定されている場合は、<code>tmp-s3fs-xxxx</code> の形式の名前が自動的にバケットに割り当てられるように、このフィールドをブランクのままにする必要があります。 この名前は、{{site.data.keyword.cos_full_notm}} において固有でなければなりません。 </li><li><code>ibm.io/auto-create-bucket</code> が <strong>false</strong> に設定されている場合: クラスター内でアクセスする既存のバケットの名前を入力します。 </li></ul> </td>
   </tr>
   <tr>
   <td><code>ibm.io/object-path</code></td>
   <td>オプション: マウントするバケットの既存のサブディレクトリーの名前を入力します。 このオプションは、1 つのサブディレクトリーのみをマウントし、バケット全体をマウントしない場合に使用します。 サブディレクトリーをマウントするには、<code>ibm.io/auto-create-bucket: "false"</code> を設定し、<code>ibm.io/bucket</code> にバケットの名前を指定する必要があります。 </li></ul> </td>
   </tr>
   <tr>
   <td><code>ibm.io/secret-name</code></td>
   <td>以前に作成した {{site.data.keyword.cos_full_notm}} 資格情報を保持するシークレットの名前を入力します。 </td>
   </tr>
   <tr>
  <td><code>ibm.io/endpoint</code></td>
  <td>クラスターとは異なるロケーションに {{site.data.keyword.cos_full_notm}} サービス・インスタンスを作成した場合は、使用する {{site.data.keyword.cos_full_notm}} サービス・インスタンスのプライベート・サービス・エンドポイントまたはパブリック・サービス・エンドポイントを入力します。 使用可能なサービス・エンドポイントの概要については、[追加のエンドポイント情報](/docs/services/cloud-object-storage?topic=cloud-object-storage-advanced-endpoints)を参照してください。 デフォルトでは、<code>ibmc</code> Helm プラグインにより、クラスター・ロケーションが自動的に取得され、クラスター・ロケーションに一致する {{site.data.keyword.cos_full_notm}} プライベート・サービス・エンドポイントを使用して、ストレージ・クラスが作成されます。 クラスターがいずれかの大都市ゾーン (`dal10` など) に存在する場合、その大都市 (この場合にはダラス) の {{site.data.keyword.cos_full_notm}} プライベート・サービス・エンドポイントが使用されます。 ストレージ・クラス内のサービス・エンドポイントがサービス・インスタンスのサービス・エンドポイントと一致することを確認するには、`kubectl describe storageclass <storageclassname>` を実行します。 サービス・エンドポイントは、プライベート・サービス・エンドポイントの場合には `https://<s3fs_private_service_endpoint>` の形式で、パブリック・サービス・エンドポイントの場合には `http://<s3fs_public_service_endpoint>` の形式で、入力するようにしてください。 ストレージ・クラス内のサービス・エンドポイントが {{site.data.keyword.cos_full_notm}} サービス・インスタンスのサービス・エンドポイントと一致する場合は、PVC YAML ファイルに <code>ibm.io/endpoint</code> オプションを含めないでください。 </td>
  </tr>
   <tr>
   <td><code>resources.requests.storage</code></td>
   <td>ご使用の {{site.data.keyword.cos_full_notm}} バケットの架空のサイズ (ギガバイト単位)。 このサイズは Kubernetes で必要になりますが、{{site.data.keyword.cos_full_notm}} では考慮されません。 希望するサイズを入力できます。 {{site.data.keyword.cos_full_notm}} で使用する実際のスペースは異なる場合があり、[料金設定表 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api) に基づいて請求されます。 </td>
   </tr>
   <tr>
   <td><code>spec.storageClassName</code></td>
   <td>次のいずれかのオプションを選択します。 <ul><li><code>ibm.io/auto-create-bucket</code> が <strong>true</strong> に設定されている場合: 新規バケットに使用するストレージ・クラスを入力します。 </li><li><code>ibm.io/auto-create-bucket</code> が <strong>false</strong> に設定されている場合: 既存のバケットの作成に使用したストレージ・クラスを入力します。 </br></br>{{site.data.keyword.cos_full_notm}} サービス・インスタンスに手動でバケットを作成した場合、または使用したストレージ・クラスを思い出せない場合は、{{site.data.keyword.Bluemix}} ダッシュボードでサービス・インスタンスを見つけて、既存のバケットの<strong>クラス</strong>と<strong>場所</strong>を確認します。 次に、該当する[ストレージ・クラス](#cos_storageclass_reference)を使用します。 <p class="note">ご使用のストレージ・クラスに設定されている {{site.data.keyword.cos_full_notm}} API エンドポイントは、クラスターのある地域に基づいています。 クラスターのある地域とは異なる地域のバケットにアクセスする場合は、[カスタム・ストレージ・クラス](/docs/containers?topic=containers-kube_concepts#customized_storageclass)を作成して、バケットの適切な API エンドポイントを使用する必要があります。</p></li></ul>  </td>
   </tr>
   </tbody>
   </table>

2. PVC を作成します。
   ```
   kubectl apply -f filepath/pvc.yaml
   ```
   {: pre}

3. PVC が作成され、PV にバインドされたことを確認します。
   ```
   kubectl get pvc
   ```
   {: pre}

   出力例:
   ```
   NAME                  STATUS    VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS                     AGE
   s3fs-test-pvc         Bound     pvc-b38b30f9-1234-11e8-ad2b-t910456jbe12   8Gi        RWO            ibmc-s3fs-standard-cross-region  1h
   ```
   {: screen}

4. オプション: 非 root ユーザーとしてデータにアクセスする場合、または既存の {{site.data.keyword.cos_full_notm}} バケットに追加されたファイルにコンソールまたは API を使用して直接アクセスする場合は、アプリが正常にファイルを読み取り、必要に応じて更新できるように、[ファイルに正しい権限が割り当てられている](/docs/containers?topic=containers-cs_troubleshoot_storage#cos_nonroot_access)ことを確認します。

4.  {: #cos_app_volume_mount}PV をデプロイメントにマウントするには、構成 `.yaml` ファイルを作成し、PV をバインドする PVC を指定します。

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
            securityContext:
              runAsUser: <non_root_user>
              fsGroup: <non_root_user> #only applicable for clusters that run Kubernetes version 1.13 or later
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
    <caption>YAML ファイルの構成要素について</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> YAML ファイルの構成要素について</th>
    </thead>
    <tbody>
        <tr>
    <td><code>metadata.labels.app</code></td>
    <td>デプロイメントのラベル。</td>
      </tr>
      <tr>
        <td><code>spec.selector.matchLabels.app</code> <br/> <code>spec.template.metadata.labels.app</code></td>
        <td>アプリのラベル。</td>
      </tr>
    <tr>
    <td><code>template.metadata.labels.app</code></td>
    <td>デプロイメントのラベル。</td>
      </tr>
    <tr>
    <td><code>spec.containers.image</code></td>
    <td>使用するイメージの名前。 {{site.data.keyword.registryshort_notm}} アカウント内の使用可能なイメージをリストするには、`ibmcloud cr image-list` を実行します。</td>
    </tr>
    <tr>
    <td><code>spec.containers.name</code></td>
    <td>クラスターにデプロイするコンテナーの名前。</td>
    </tr>
    <tr>
    <td><code>spec.containers.securityContext.runAsUser</code></td>
    <td>オプション: Kubernetes バージョン 1.12 以前が実行されているクラスター内で非 root ユーザーとしてアプリを実行するには、デプロイメント YAML で同時に `fsGroup` を設定せずに非 root ユーザーを定義して、ポッドの[セキュリティー・コンテキスト ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/) を指定します。 `fsGroup` を設定すると、ポッドのデプロイ時に、{{site.data.keyword.cos_full_notm}} プラグインによるバケット内のすべてのファイルのグループ権限の更新がトリガーされます。 権限の更新は書き込み操作であり、パフォーマンスに影響を与えます。 所有するファイルの数によっては、権限を更新すると、ポッドが起動しなくなり、<code>Running</code> 状態にならなくなる場合があります。 </br></br>クラスターで Kubernetes バージョン 1.13 以降および {{site.data.keyword.Bluemix_notm}} Object Storage プラグインのバージョン 1.0.4 以降を実行する場合は、s3fs マウント・ポイントの所有者を変更できます。 所有者を変更するには、`runAsUser` および `fsGroup` を s3fs マウント・ポイントを所有する同じ非 root ユーザーの ID に設定して、セキュリティー・コンテキストを指定します。 これら 2 つの値が一致しない場合、マウント・ポイントは自動的に `root` ユーザーが所有します。  </td>
    </tr>
    <tr>
    <td><code>spec.containers.volumeMounts.mountPath</code></td>
    <td>コンテナー内でボリュームがマウントされるディレクトリーの絶対パス。 複数のアプリ間で 1 つのボリュームを共有する場合は、アプリごとに[ボリューム・サブパス ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/concepts/storage/volumes/#using-subpath) を指定できます。</td>
    </tr>
    <tr>
    <td><code>spec.containers.volumeMounts.name</code></td>
    <td>ポッドにマウントするボリュームの名前。</td>
    </tr>
    <tr>
    <td><code>volumes.name</code></td>
    <td>ポッドにマウントするボリュームの名前。 通常、この名前は <code>volumeMounts/name</code> と同じです。</td>
    </tr>
    <tr>
    <td><code>volumes.persistentVolumeClaim.claimName</code></td>
    <td>使用する PV をバインドする PVC の名前。 </td>
    </tr>
    </tbody></table>

5.  デプロイメントを作成します。
     ```
     kubectl apply -f <local_yaml_path>
     ```
     {: pre}

6.  PV が正常にマウントされたことを確認します。

     ```
     kubectl describe deployment <deployment_name>
     ```
     {: pre}

     マウント・ポイントは **Volume Mounts** フィールドにあり、ボリュームは **Volumes** フィールドにあります。

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

7. {{site.data.keyword.cos_full_notm}} サービス・インスタンスにデータを書き込み可能であることを確認します。
   1. PV をマウントするポッドにログインします。
      ```
      kubectl exec <pod_name> -it bash
      ```
      {: pre}

   2. アプリ・デプロイメントで定義したボリューム・マウント・パスにナビゲートします。
   3. テキスト・ファイルを作成します。
      ```
      echo "This is a test" > test.txt
      ```
      {: pre}

   4. {{site.data.keyword.Bluemix}} ダッシュボードから、{{site.data.keyword.cos_full_notm}} サービス・インスタンスにナビゲートします。
   5. メニューから、**「バケット」**を選択します。
   6. バケットを開いて、作成した `test.txt` を表示できることを確認します。


## ステートフル・セットでのオブジェクト・ストレージの使用
{: #cos_statefulset}

データベースなどのステートフルなアプリがある場合は、そのアプリのデータを保管するために、{{site.data.keyword.cos_full_notm}} を使用するステートフル・セットを作成することができます。 別の方法として、{{site.data.keyword.Bluemix_notm}} Database as a Service ({{site.data.keyword.cloudant_short_notm}} など) を使用し、クラウドにデータを保管することもできます。
{: shortdesc}

開始前に、以下のことを行います。
- [{{site.data.keyword.cos_full_notm}} サービス・インスタンスを作成して準備します](#create_cos_service)。
- [{{site.data.keyword.cos_full_notm}} サービス資格情報を保管するシークレットを作成します](#create_cos_secret)。
- [{{site.data.keyword.cos_full_notm}} の構成を決定します](#configure_cos)。

オブジェクト・ストレージを使用するステートフル・セットをデプロイするには、以下のようにします。

1. ステートフル・セットと、そのステートフル・セットを公開するために使用するサービスに関する、構成ファイルを作成します。 下記の例は、3 つのレプリカを伴うステートフル・セットとして NGINX をデプロイする方法を示しています。レプリカごとに別個のバケットを使用する場合と、すべてのレプリカで同じバケットを共有する場合を示します。

   **レプリカごとに別個のバケットを使用する、3 つのレプリカを伴うステートフル・セットを作成する例**:
   ```
   apiVersion: v1
   kind: Service
   metadata:
     name: nginx-v01
     namespace: default
     labels:
       app: nginx-v01 # must match spec.template.metadata.labels and spec.selector.matchLabels in stateful set YAML
   spec:
     ports:
     - port: 80
       name: web
     clusterIP: None
     selector:
       app: nginx-v01 # must match spec.template.metadata.labels and spec.selector.matchLabels in stateful set YAML
   ---
   apiVersion: apps/v1
   kind: StatefulSet
   metadata:
     name: web-v01
     namespace: default
   spec:
     selector:
       matchLabels:
         app: nginx-v01 # must match spec.template.metadata.labels in stateful set YAML and metadata.labels in service YAML
     serviceName: "nginx-v01"
     replicas: 3 
     template:
       metadata:
         labels:
           app: nginx-v01 # must match spec.selector.matchLabels in stateful set YAML and metadata.labels in service YAML
       spec:
         terminationGracePeriodSeconds: 10
         containers:
         - name: nginx
           image: k8s.gcr.io/nginx-slim:0.8
           ports:
           - containerPort: 80
             name: web
           volumeMounts:
           - name: mypvc
             mountPath: /usr/share/nginx/html
     volumeClaimTemplates:
     - metadata:
         name: mypvc
         annotations:
           ibm.io/auto-create-bucket: "true"
           ibm.io/auto-delete-bucket: "true"
           ibm.io/bucket: ""
           ibm.io/secret-name: mysecret 
           volume.beta.kubernetes.io/storage-class: ibmc-s3fs-standard-perf-cross-region 
           volume.beta.kubernetes.io/storage-provisioner: ibm.io/ibmc-s3fs
       spec:
         accessModes: [ "ReadWriteOnce" ]
         storageClassName: "ibmc-s3fs-standard-perf-cross-region"
         resources:
           requests:
             storage: 1Gi
   ```
   {: codeblock}

   **すべてのレプリカで同じバケット `mybucket`** を共有する、3 つのレプリカを伴うステートフル・セットを作成する例:
   ```
   apiVersion: v1
   kind: Service
   metadata:
     name: nginx-v01
     namespace: default
     labels:
       app: nginx-v01 # must match spec.template.metadata.labels and spec.selector.matchLabels in stateful set YAML
   spec:
     ports:
     - port: 80
       name: web
     clusterIP: None
     selector:
       app: nginx-v01 # must match spec.template.metadata.labels and spec.selector.matchLabels in stateful set YAML
   --- 
   apiVersion: apps/v1
   kind: StatefulSet
   metadata:
     name: web-v01
     namespace: default
   spec:
     selector:
       matchLabels:
         app: nginx-v01 # must match spec.template.metadata.labels in stateful set YAML and metadata.labels in service YAML
     serviceName: "nginx-v01"
     replicas: 3 
     template:
       metadata:
         labels:
           app: nginx-v01 # must match spec.selector.matchLabels in stateful set YAML and metadata.labels in service YAML
       spec:
         terminationGracePeriodSeconds: 10
         containers:
         - name: nginx
           image: k8s.gcr.io/nginx-slim:0.8
           ports:
           - containerPort: 80
             name: web
           volumeMounts:
           - name: mypvc
             mountPath: /usr/share/nginx/html
     volumeClaimTemplates:
     - metadata:
         name: mypvc
         annotations:
           ibm.io/auto-create-bucket: "false"
           ibm.io/auto-delete-bucket: "false"
           ibm.io/bucket: mybucket
           ibm.io/secret-name: mysecret
           volume.beta.kubernetes.io/storage-class: ibmc-s3fs-standard-perf-cross-region 
           volume.beta.kubernetes.io/storage-provisioner: ibm.io/ibmc-s3fs
       spec:
         accessModes: [ "ReadOnlyMany" ]
         storageClassName: "ibmc-s3fs-standard-perf-cross-region"
         resources:
           requests:
             storage: 1Gi
   ```
   {: codeblock}


   <table>
    <caption>ステートフル・セット YAML ファイルの構成要素について</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> ステートフル・セット YAML ファイルの構成要素について</th>
    </thead>
    <tbody>
    <tr>
    <td style="text-align:left"><code>metadata.name</code></td>
    <td style="text-align:left">ステートフル・セットに対する名前を入力します。 入力した名前が使用されて、PVC の名前が次の形式で作成されます: <code>&lt;volume_name&gt;-&lt;statefulset_name&gt;-&lt;replica_number&gt;</code>。 </td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.serviceName</code></td>
    <td style="text-align:left">ステートフル・セットを公開するために使用するサービスの名前を入力します。 </td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.replicas</code></td>
    <td style="text-align:left">ステートフル・セットのレプリカの数を入力します。 </td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.selector.matchLabels</code></td>
    <td style="text-align:left">ステートフル・セットと PVC に含めるすべてのラベルを入力します。 ステートフル・セットの <code>volumeClaimTemplates</code> に含めたラベルは、Kubernetes によって認識されません。 代わりに、それらのラベルをステートフル・セット YAML の <code>spec.selector.matchLabels</code> および <code>spec.template.metadata.labels</code> セクションで定義する必要があります。 すべてのステートフル・セット・レプリカをサービスのロード・バランシングに確実に含めるため、サービス YAML の <code>spec.selector</code> セクションで使用したラベルと同じラベルを含めます。 </td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.template.metadata.labels</code></td>
    <td style="text-align:left">ステートフル・セット YAML の <code>spec.selector.matchLabels</code> セクションに追加したラベルと同じラベルを入力します。 </td>
    </tr>
    <tr>
    <td><code>spec.template.spec.</code></br><code>terminationGracePeriodSeconds</code></td>
    <td>ステートフル・セット・レプリカを実行するポッドを安全な方法で終了するために <code>kubelet</code> に付与する秒数を入力します。 詳しくは、[Delete Pods ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/tasks/run-application/force-delete-stateful-set-pod/#delete-pods) を参照してください。 </td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.volumeClaimTemplates.</code></br><code>metadata.name</code></td>
    <td style="text-align:left">ボリュームの名前を入力します。 <code>spec.containers.volumeMount.name</code> セクションで定義した名前と同じ名前を使用します。 ここに入力した名前が使用されて、PVC の名前が次の形式で作成されます: <code>&lt;volume_name&gt;-&lt;statefulset_name&gt;-&lt;replica_number&gt;</code>。 </td>
    </tr>
    <tr>
    <td><code>spec.volumeClaimTemplates.metadata</code></br><code>annotions.ibm.io/auto-create-bucket</code></td>
    <td>次のいずれかのオプションを選択します。 <ul><li><strong>true: </strong>ステートフル・セット・レプリカごとにバケットを自動的に作成するには、このオプションを選択します。 </li><li><strong>false: </strong>既存のバケットの 1 つをステートフル・セット・レプリカ間で共有する場合は、このオプションを選択します。 ステートフル・セット YAML の <code>spec.volumeClaimTemplates.metadata.annotions.ibm.io/bucket</code> セクションでバケットの名前を定義してください。</li></ul></td>
    </tr>
    <tr>
    <td><code>spec.volumeClaimTemplates.metadata</code></br><code>annotions.ibm.io/auto-delete-bucket</code></td>
    <td>次のいずれかのオプションを選択します。 <ul><li><strong>true: </strong>データ、バケット、および PV が、PVC の削除時に自動的に削除されます。 {{site.data.keyword.cos_full_notm}} サービス・インスタンスは維持され、削除されません。 このオプションを true に設定する場合は、<code>ibm.io/auto-create-bucket: true</code> および <code>ibm.io/bucket: ""</code> を設定して、<code>tmp-s3fs-xxxx</code> の形式の名前でバケットが自動的に作成されるようにする必要があります。 </li><li><strong>false: </strong>PVC を削除すると、PV は自動的に削除されますが、{{site.data.keyword.cos_full_notm}} サービス・インスタンスのデータとバケットは維持されます。 データにアクセスするには、既存のバケットの名前を持つ新しい PVC を作成する必要があります。</li></ul></td>
    </tr>
    <tr>
    <td><code>spec.volumeClaimTemplates.metadata</code></br><code>annotions.ibm.io/bucket</code></td>
    <td>次のいずれかのオプションを選択します。 <ul><li><strong><code>ibm.io/auto-create-bucket</code> が true に設定されている場合:</strong> {{site.data.keyword.cos_full_notm}} に作成するバケットの名前を入力します。 さらに <code>ibm.io/auto-delete-bucket</code> が <strong>true</strong> に設定されている場合は、tmp-s3fs-xxxx の形式の名前が自動的にバケットに割り当てられるように、このフィールドをブランクのままにする必要があります。 この名前は、{{site.data.keyword.cos_full_notm}} において固有でなければなりません。</li><li><strong><code>ibm.io/auto-create-bucket</code> が false に設定されている場合:</strong> クラスター内でアクセスする既存のバケットの名前を入力します。</li></ul></td>
    </tr>
    <tr>
    <td><code>spec.volumeClaimTemplates.metadata</code></br><code>annotions.ibm.io/secret-name</code></td>
    <td>以前に作成した {{site.data.keyword.cos_full_notm}} 資格情報を保持するシークレットの名前を入力します。</td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.volumeClaimTemplates.metadata.</code></br><code>annotations.volume.beta.</code></br><code>kubernetes.io/storage-class</code></td>
    <td style="text-align:left">使用するストレージ・クラスを入力します。 次のいずれかのオプションを選択します。 <ul><li><strong><code>ibm.io/auto-create-bucket</code> が true に設定されている場合:</strong> 新規バケットに使用するストレージ・クラスを入力します。</li><li><strong><code>ibm.io/auto-create-bucket</code> が false に設定されている場合:</strong> 既存のバケットの作成に使用したストレージ・クラスを入力します。 </li></ul></br>  既存のストレージ・クラスをリストするには、<code>kubectl get storageclasses | grep s3</code> を実行します。 ストレージ・クラスを指定しなかった場合は、クラスターで設定されているデフォルト・ストレージ・クラスを使用して PVC が作成されます。 オブジェクト・ストレージを使用してステートフル・セットがプロビジョンされるようにするために、デフォルト・ストレージ・クラスで <code>ibm.io/ibmc-s3fs</code> プロビジョナーが使用されていることを確認してください。</td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.volumeClaimTemplates.</code></br><code>spec.storageClassName</code></td>
    <td>ステートフル・セット YAML の <code>spec.volumeClaimTemplates.metadata.annotations.volume.beta.kubernetes.io/storage-class</code> セクションに入力したストレージ・クラスを入力します。  </td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.volumeClaimTemplates.spec.</code></br><code>resource.requests.storage</code></td>
    <td>ご使用の {{site.data.keyword.cos_full_notm}} バケットの架空のサイズ (ギガバイト単位) を入力します。 このサイズは Kubernetes で必要になりますが、{{site.data.keyword.cos_full_notm}} では考慮されません。 希望するサイズを入力できます。 {{site.data.keyword.cos_full_notm}} で使用する実際のスペースは異なる場合があり、[料金設定表 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api) に基づいて請求されます。</td>
    </tr>
    </tbody></table>


## データのバックアップとリストア
{: #cos_backup_restore}

{{site.data.keyword.cos_full_notm}} は、データが損失しないように保護するために、データに高い耐久性を提供するようにセットアップされています。 [{{site.data.keyword.cos_full_notm}} サービスのご利用条件 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www-03.ibm.com/software/sla/sladb.nsf/sla/bm-7857-03) で SLA を確認できます。
{: shortdesc}

{{site.data.keyword.cos_full_notm}} は、データのバージョン履歴を提供しません。 旧バージョンのデータを保持してアクセスする必要がある場合は、アプリをセットアップして、データの履歴を管理するか、または代替のバックアップ・ソリューションを実装する必要があります。 例えば、{{site.data.keyword.cos_full_notm}} データをオンプレミス・データベースに保管したり、テープを使用してデータをアーカイブしたりすることができます。
{: note}

## ストレージ・クラス・リファレンス
{: #cos_storageclass_reference}

### 標準
{: #standard}

<table>
<caption>オブジェクト・ストレージ・クラス: standard</caption>
<thead>
<th>特性</th>
<th>設定</th>
</thead>
<tbody>
<tr>
<td>名前</td>
<td><code>ibmc-s3fs-standard-cross-region</code></br><code>ibmc-s3fs-standard-perf-cross-region</code></br><code>ibmc-s3fs-standard-regional</code></br><code>ibmc-s3fs-standard-perf-regional</code></td>
</tr>
<tr>
<td>デフォルトの回復力エンドポイント</td>
<td>回復力エンドポイントは、クラスターの場所に基づいて自動的に設定されます。 詳しくは、[地域とエンドポイント](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-endpoints#endpoints)を参照してください。 </td>
</tr>
<tr>
<td>チャンク・サイズ</td>
<td>`perf` が指定されていないストレージ・クラス: 16 MB</br>`perf` が指定されているストレージ・クラス: 52 MB</td>
</tr>
<tr>
<td>カーネル・キャッシュ</td>
<td>有効</td>
</tr>
<tr>
<td>課金</td>
<td>月単位</td>
</tr>
<tr>
<td>料金設定</td>
<td>[料金設定 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api)</td>
</tr>
</tbody>
</table>

### Vault
{: #Vault}

<table>
<caption>オブジェクト・ストレージ・クラス: vault</caption>
<thead>
<th>特性</th>
<th>設定</th>
</thead>
<tbody>
<tr>
<td>名前</td>
<td><code>ibmc-s3fs-vault-cross-region</code></br><code>ibmc-s3fs-vault-regional</code></td>
</tr>
<tr>
<td>デフォルトの回復力エンドポイント</td>
<td>回復力エンドポイントは、クラスターの場所に基づいて自動的に設定されます。 詳しくは、[地域とエンドポイント](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-endpoints#endpoints)を参照してください。 </td>
</tr>
<tr>
<td>チャンク・サイズ</td>
<td>16 MB</td>
</tr>
<tr>
<td>カーネル・キャッシュ</td>
<td>無効化</td>
</tr>
<tr>
<td>課金</td>
<td>月単位</td>
</tr>
<tr>
<td>料金設定</td>
<td>[料金設定 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api)</td>
</tr>
</tbody>
</table>

### Cold
{: #cold}

<table>
<caption>オブジェクト・ストレージ・クラス: cold</caption>
<thead>
<th>特性</th>
<th>設定</th>
</thead>
<tbody>
<tr>
<td>名前</td>
<td><code>ibmc-s3fs-flex-cross-region</code></br><code>ibmc-s3fs-flex-perf-cross-region</code></br><code>ibmc-s3fs-flex-regional</code></br><code>ibmc-s3fs-flex-perf-regional</code></td>
</tr>
<tr>
<td>デフォルトの回復力エンドポイント</td>
<td>回復力エンドポイントは、クラスターの場所に基づいて自動的に設定されます。 詳しくは、[地域とエンドポイント](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-endpoints#endpoints)を参照してください。 </td>
</tr>
<tr>
<td>チャンク・サイズ</td>
<td>16 MB</td>
</tr>
<tr>
<td>カーネル・キャッシュ</td>
<td>無効化</td>
</tr>
<tr>
<td>課金</td>
<td>月単位</td>
</tr>
<tr>
<td>料金設定</td>
<td>[料金設定 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api)</td>
</tr>
</tbody>
</table>

### Flex
{: #flex}

<table>
<caption>オブジェクト・ストレージ・クラス: flex</caption>
<thead>
<th>特性</th>
<th>設定</th>
</thead>
<tbody>
<tr>
<td>名前</td>
<td><code>ibmc-s3fs-cold-cross-region</code></br><code>ibmc-s3fs-flex-perf-cross-region</code></br><code>ibmc-s3fs-cold-regional</code></br><code>ibmc-s3fs-flex-perf-regional</code></td>
</tr>
<tr>
<td>デフォルトの回復力エンドポイント</td>
<td>回復力エンドポイントは、クラスターの場所に基づいて自動的に設定されます。 詳しくは、[地域とエンドポイント](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-endpoints#endpoints)を参照してください。 </td>
</tr>
<tr>
<td>チャンク・サイズ</td>
<td>`perf` が指定されていないストレージ・クラス: 16 MB</br>`perf` が指定されているストレージ・クラス: 52 MB</td>
</tr>
<tr>
<td>カーネル・キャッシュ</td>
<td>有効</td>
</tr>
<tr>
<td>課金</td>
<td>月単位</td>
</tr>
<tr>
<td>料金設定</td>
<td>[料金設定 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api)</td>
</tr>
</tbody>
</table>
