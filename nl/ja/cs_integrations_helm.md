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



# Helm チャートを使用したサービスの追加
{: #helm}

Helm チャートを使用して、複雑な Kubernetes アプリをクラスターに追加できます。
{: shortdesc}

**Helm とは何ですか? どのようにして使用しますか?** </br>
[Helm ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://helm.sh) は Kubernetes パッケージ・マネージャーであり、Helm チャートを使用してクラスター内の複雑な Kubernetes アプリを定義、インストール、およびアップグレードします。 Helm チャートには、アプリを作成する Kubernetes リソースの YAML ファイルを生成するための指定内容がパッケージ化されています。 これらの Kubernetes リソースはクラスター内で自動的に適用され、Helm によってバージョンを割り当てられます。Helm を使用して独自のアプリを指定およびパッケージ化して、Kubernetes リソースの YAML ファイルを Helm に生成させることもできます。  

クラスターで Helm を使用するには、Helm CLI をローカル・マシンにインストールして、Helm を使用するすべてのクラスターに Helm サーバー Tiller をインストールする必要があります。

**{{site.data.keyword.containerlong_notm}} ではどのような Helm チャートがサポートされていますか?** </br>
使用可能な Helm チャートの概要については、[Helm チャートのカタログ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/kubernetes/solutions/helm-charts) を参照してください。 このカタログにリストされている Helm チャートは、以下のようにグループ分けされています。

- **iks-charts**: これらの Helm チャートは、{{site.data.keyword.containerlong_notm}} 用として承認されています。 このリポジトリーの名前は、`ibm` から `iks-charts` に変更されました。
- **ibm-charts**: これらの Helm チャートは、{{site.data.keyword.containerlong_notm}} 用および {{site.data.keyword.Bluemix_notm}} Private クラスター用として承認されています。
- **kubernetes**: これらの Helm チャートは Kubernetes コミュニティーによって提供されており、コミュニティー・ガバナンスによって `stable` と見なされています。 これらのチャートは、{{site.data.keyword.containerlong_notm}} や {{site.data.keyword.Bluemix_notm}} Private クラスターで機能することが確認されていません。
- **kubernetes-incubator**: これらの Helm チャートは Kubernetes コミュニティーによって提供されており、コミュニティー・ガバナンスによって `incubator` と見なされています。 これらのチャートは、{{site.data.keyword.containerlong_notm}} や {{site.data.keyword.Bluemix_notm}} Private クラスターで機能することが確認されていません。

**iks-charts** リポジトリー内と **ibm-charts** リポジトリー内の Helm チャートは、{{site.data.keyword.Bluemix_notm}} のサポート組織に完全に統合されています。 これらの Helm チャートの使用に関する質問や問題がある場合は、いずれかの {{site.data.keyword.containerlong_notm}} サポート・チャネルを使用できます。 詳しくは、[ヘルプとサポートの取得](/docs/containers?topic=containers-cs_troubleshoot_clusters#clusters_getting_help)を参照してください。

**Helm を使用するための前提条件は何ですか? プライベート・クラスターで Helm を使用できますか?** </br>
Helm チャートをデプロイするためには、ローカル・マシンに Helm CLI をインストールし、クラスターに Helm サーバー Tiller をインストールする必要があります。 Tiller のイメージはパブリック Google Container Registry に保管されます。 Tiller のインストール時にイメージにアクセスするためには、パブリック Google Container Registry へのパブリック・ネットワーク接続がクラスターで許可されていなければなりません。 パブリック・サービス・エンドポイントが有効になっているクラスターは、自動的にイメージにアクセスできます。カスタム・ファイアウォールで保護されているプライベート・クラスターや、プライベート・サービス・エンドポイントのみを有効にしているクラスターでは、Tiller イメージへのアクセスが許可されません。代わりに、[イメージをローカル・マシンにプルしてそれを {{site.data.keyword.registryshort_notm}} 内の名前空間にプッシュする](#private_local_tiller)か、[Tiller を使用せずに Helm チャートをインストールする](#private_install_without_tiller)ことができます。


## パブリック・アクセスが可能なクラスターでの Helm のセットアップ
{: #public_helm_install}

クラスターでパブリック・サービス・エンドポイントが有効になっている場合は、Google Container Registry 内のパブリック・イメージを使用して Helm サーバー Tiller をインストールできます。
{: shortdesc}

開始前に、以下のことを行います。
- [アカウントにログインします。 該当する場合は、適切なリソース・グループをターゲットにします。 クラスターのコンテキストを設定します。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
- Kubernetes サービス・アカウントとクラスター役割バインディングを使用して Tiller を `kube-system` 名前空間にインストールするには、[`cluster-admin` 役割](/docs/containers?topic=containers-users#access_policies)を持っていることを確認してください。

パブリック・アクセスが可能なクラスターに Helm をインストールするには、次のようにします。

1. ローカル・マシンに <a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">Helm CLI <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> をインストールします。

2. Kubernetes サービス・アカウントを使用して Tiller をクラスター内に既にインストール済みかどうかを確認します。
   ```
   kubectl get serviceaccount --all-namespaces | grep tiller
   ```
   {: pre}

   Tiller がインストールされている場合の出力例:
   ```
   kube-system      tiller                               1         189d
   ```
   {: screen}

   この出力例には、Kubernetes 名前空間と、Tiller 用のサービス・アカウントの名前が含まれています。 サービス・アカウントを使用して Tiller がクラスターにインストールされていない場合は、CLI の出力は返されません。

3. **重要**: クラスターのセキュリティーを維持するには、サービス・アカウントとクラスター役割バインディングを使用してクラスター内に Tiller をセットアップしてください。
   - **サービス・アカウントを使用して Tiller がインストールされている場合:**
     1. Tiller サービス・アカウントのクラスター役割バインディングを作成します。 `<namespace>` をクラスター内で Tiller がインストールされている名前空間に置き換えてください。
        ```
        kubectl create clusterrolebinding tiller --clusterrole=cluster-admin --serviceaccount=<namespace>:tiller -n <namespace>
        ```
        {: pre}

     2. Tiller を更新します。 `<tiller_service_account_name>` を前のステップで取得した Tiller の Kubernetes サービス・アカウントの名前に置き換えてください。
        ```
        helm init --upgrade --service-account <tiller_service_account_name>
        ```
        {: pre}

     3. クラスター内の `tiller-deploy` ポッドの**「状況」**が`「実行中」`になっていることを確認します。
        ```
        kubectl get pods -n <namespace> -l app=helm
        ```
        {: pre}

        出力例:

        ```
        NAME                            READY     STATUS    RESTARTS   AGE
    tiller-deploy-352283156-nzbcm   1/1       Running   0          2m
        ```
        {: screen}

   - **サービス・アカウントを使用して Tiller がインストールされていない場合:**
     1. クラスターの `kube-system` 名前空間内に、Tiller の Kubernetes サービス・アカウントとクラスター役割バインディングを作成します。
        ```
        kubectl create serviceaccount tiller -n kube-system
        ```
        {: pre}

        ```
        kubectl create clusterrolebinding tiller --clusterrole=cluster-admin --serviceaccount=kube-system:tiller -n kube-system
        ```
        {: pre}

     2. Tiller サービス・アカウントが作成されたことを確認します。
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

     3. 作成したサービス・アカウントを使用して、Helm CLI を初期化して、Tiller をクラスター内にインストールします。
        ```
        helm init --service-account tiller
        ```
        {: pre}

     4. クラスター内の `tiller-deploy` ポッドの**「状況」**が`「実行中」`になっていることを確認します。
        ```
        kubectl get pods -n kube-system -l app=helm
        ```
        {: pre}

        出力例:
        ```
        NAME                            READY     STATUS    RESTARTS   AGE
    tiller-deploy-352283156-nzbcm   1/1       Running   0          2m
        ```
        {: screen}

4. {{site.data.keyword.Bluemix_notm}} Helm リポジトリーを Helm インスタンスに追加します。
   ```
   helm repo add iks-charts https://icr.io/helm/iks-charts
   ```
   {: pre}

   ```
   helm repo add ibm-charts https://icr.io/helm/ibm-charts
   ```
   {: pre}

5. すべての Helm チャートの最新バージョンを取得するようにリポジトリーを更新します。
   ```
   helm repo update
   ```
   {: pre}

6. {{site.data.keyword.Bluemix_notm}} リポジトリーで現在使用可能な Helm チャートをリストします。
   ```
   helm search iks-charts
   ```
   {: pre}

   ```
   helm search ibm-charts
   ```
   {: pre}

7. インストールする Helm チャートを特定し、Helm チャートの `README` に記載されている手順に従って Helm チャートをクラスターにインストールします。


## プライベート・クラスター: IBM Cloud Container Registry 内の名前空間への Tiller イメージのプッシュ
{: #private_local_tiller}

Tiller イメージをローカル・マシンにプルして、そのイメージを {{site.data.keyword.registryshort_notm}} 内の名前空間にプッシュし、{{site.data.keyword.registryshort_notm}} 内のそのイメージを使用して Tiller をプライベート・クラスターにインストールできます。
{: shortdesc}

Tiller を使用せずに Helm チャートをインストールする場合は、[プライベート・クラスター: Tiller を使用せずに Helm チャートをインストールする](#private_install_without_tiller)を参照してください。
{: tip}

開始前に、以下のことを行います。
- ローカル・マシンに Docker をインストールします。 [{{site.data.keyword.Bluemix_notm}} CLI](/docs/cli?topic=cloud-cli-getting-started) がインストールされていれば、Docker は既にインストールされています。
- [{{site.data.keyword.registryshort_notm}} CLI プラグインをインストールして、名前空間をセットアップします](/docs/services/Registry?topic=registry-getting-started#gs_registry_cli_install)。
- Kubernetes サービス・アカウントとクラスター役割バインディングを使用して Tiller を `kube-system` 名前空間にインストールするには、[`cluster-admin` 役割](/docs/containers?topic=containers-users#access_policies)を持っていることを確認してください。

{{site.data.keyword.registryshort_notm}} を使用して Tiller をインストールするには、以下のようにします。

1. ローカル・マシンに <a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">Helm CLI <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> をインストールします。
2. セットアップした {{site.data.keyword.Bluemix_notm}} インフラストラクチャー VPN トンネルを使用して、プライベート・クラスターに接続します。
3. **重要**: クラスターのセキュリティーを維持するためには、[{{site.data.keyword.Bluemix_notm}}`kube-samples` リポジトリー](https://github.com/IBM-Cloud/kube-samples/blob/master/rbac/serviceaccount-tiller.yaml)から以下の YAML ファイルを適用することによって、Tiller のサービス・アカウントを `kube-system` 名前空間内に作成し、`tiller-deploy` ポッドに対する Kubernetes RBAC クラスター役割バインディングを作成します。
    1. [Kubernetes サービス・アカウントとクラスター役割バインディングの YAML ファイル ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン") を取得します](https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/rbac/serviceaccount-tiller.yaml)。

    2. クラスター内に Kubernetes リソースを作成します。
       ```
       kubectl apply -f service-account.yaml
       ```
       {: pre}

       ```
       kubectl apply -f cluster-role-binding.yaml
       ```
       {: pre}

4. クラスターにインストールする[バージョンの Tiller を見つけます ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://console.cloud.google.com/gcr/images/kubernetes-helm/GLOBAL/tiller?gcrImageListsize=30)。 特定のバージョンを必要としない場合は、最新のものを使用してください。

5. パブリック Google Container Registry からローカル・マシンに Tiller イメージをプルします。
   ```
   docker pull gcr.io/kubernetes-helm/tiller:v2.11.0
   ```
   {: pre}

   出力例:
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

6. [Tiller イメージを {{site.data.keyword.registryshort_notm}} 内の名前空間にプッシュします](/docs/services/Registry?topic=registry-getting-started#gs_registry_images_pushing)。

7. クラスター内から {{site.data.keyword.registryshort_notm}} 内のこのイメージにアクセスするには、[デフォルトの名前空間から `kube-system` 名前空間にイメージ・プル・シークレットをコピー](/docs/containers?topic=containers-images#copy_imagePullSecret)します。

8. {{site.data.keyword.registryshort_notm}} 内の名前空間に保管したイメージを使用して、プライベート・クラスターに Tiller をインストールします。
   ```
   helm init --tiller-image <region>.icr.io/<mynamespace>/<myimage>:<tag> --service-account tiller
   ```
   {: pre}

9. {{site.data.keyword.Bluemix_notm}} Helm リポジトリーを Helm インスタンスに追加します。
   ```
   helm repo add iks-charts https://icr.io/helm/iks-charts
   ```
   {: pre}

   ```
   helm repo add ibm-charts https://icr.io/helm/ibm-charts
   ```
   {: pre}

10. すべての Helm チャートの最新バージョンを取得するようにリポジトリーを更新します。
    ```
    helm repo update
    ```
    {: pre}

11. {{site.data.keyword.Bluemix_notm}} リポジトリーで現在使用可能な Helm チャートをリストします。
    ```
    helm search iks-charts
    ```
    {: pre}

    ```
    helm search ibm-charts
    ```
    {: pre}

12. インストールする Helm チャートを特定し、Helm チャートの `README` に記載されている手順に従って Helm チャートをクラスターにインストールします。


## プライベート・クラスター: Tiller を使用せずに Helm チャートをインストールする
{: #private_install_without_tiller}

プライベート・クラスターに Tiller をインストールしない場合は、Helm チャートの YAML ファイルを手動で作成し、`kubectl` コマンドを使用してそれらの YAML ファイルを適用できます。
{: shortdesc}

この例のステップは、{{site.data.keyword.Bluemix_notm}} Helm チャート・リポジトリーからプライベート・クラスターに Helm チャートをインストールする方法を示しています。 {{site.data.keyword.Bluemix_notm}} Helm チャート・リポジトリーのいずれにも保管されていない Helm チャートをインストールする場合は、このトピックの手順に従って Helm チャート用の YAML ファイルを作成する必要があります。 さらに、パブリック・コンテナー・リポジトリーから Helm チャート・イメージをダウンロードし、それを {{site.data.keyword.registryshort_notm}} 内の名前空間にプッシュして、{{site.data.keyword.registryshort_notm}} 内のイメージを使用するように `values.yaml` ファイルを更新する必要があります。
{: note}

1. ローカル・マシンに <a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">Helm CLI <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> をインストールします。
2. セットアップした {{site.data.keyword.Bluemix_notm}} インフラストラクチャー VPN トンネルを使用して、プライベート・クラスターに接続します。
3. {{site.data.keyword.Bluemix_notm}} Helm リポジトリーを Helm インスタンスに追加します。
   ```
   helm repo add iks-charts https://icr.io/helm/iks-charts
   ```
   {: pre}

   ```
   helm repo add ibm-charts https://icr.io/helm/ibm-charts
   ```
   {: pre}

4. すべての Helm チャートの最新バージョンを取得するようにリポジトリーを更新します。
   ```
   helm repo update
   ```
   {: pre}

5. {{site.data.keyword.Bluemix_notm}} リポジトリーで現在使用可能な Helm チャートをリストします。
   ```
   helm search iks-charts
   ```
   {: pre}

   ```
   helm search ibm-charts
   ```
   {: pre}

6. インストールする Helm チャートを特定し、その Helm チャートをローカル・マシンにダウンロードして、Helm チャートのファイルをアンパックします。 次の例は、cluster autoscaler バージョン 1.0.3 用の Helm チャートをダウンロードして、`cluster-autoscaler` ディレクトリーにファイルをアンパックする方法を示しています。
   ```
   helm fetch iks-charts/ibm-iks-cluster-autoscaler --untar --untardir ./cluster-autoscaler --version 1.0.3
   ```
   {: pre}

7. Helm チャート・ファイルをアンパックしたディレクトリーにナビゲートします。
   ```
   cd cluster-autoscaler
   ```
   {: pre}

8. Helm チャート内のファイルを使用して生成する YAML ファイル用の `output` ディレクトリーを作成します。
   ```
   mkdir output
   ```
   {: pre}

9. `values.yaml` ファイルを開き、Helm チャートのインストール手順で必要とされる変更を行います。
   ```
   nano ibm-iks-cluster-autoscaler/values.yaml
   ```
   {: pre}

10. ローカルにインストールされている Helm を使用して、Helm チャート用のすべての Kubernetes YAML ファイルを作成します。 既に作成した `output` ディレクトリーに YAML ファイルが保管されます。
    ```
    helm template --values ./ibm-iks-cluster-autoscaler/values.yaml --output-dir ./output ./ibm-iks-cluster-autoscaler
    ```
    {: pre}

    出力例:
    ```
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-configmap.yaml
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-service-account-roles.yaml
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-service.yaml
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-deployment.yaml
    ```
    {: screen}

11. すべての YAML ファイルをプライベート・クラスターにデプロイします。
    ```
    kubectl apply --recursive --filename ./output
    ```
   {: pre}

12. オプション: `output` ディレクトリーからすべての YAML ファイルを削除します。
    ```
    kubectl delete --recursive --filename ./output
    ```
    {: pre}

## Helm の関連リンク
{: #helm_links}

Helm について詳しくは、以下のリンクを参照してください。
{: shortdesc}

* {{site.data.keyword.containerlong_notm}} で使用できる Helm チャートについては、[Helm チャートのカタログ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/kubernetes/solutions/helm-charts) を参照してください。
* Helm チャートのセットアップと管理に使用できる Helm コマンドについて詳しくは、<a href="https://docs.helm.sh/helm/" target="_blank">Helm の資料 <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を参照してください。
* Kubernetes Helm チャートを使用して開発速度を上げる方法について詳しくは、[こちら ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://developer.ibm.com/recipes/tutorials/increase-deployment-velocity-with-kubernetes-helm-charts/) を参照してください。
