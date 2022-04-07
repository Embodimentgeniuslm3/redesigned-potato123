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



# チュートリアル: Kubernetes クラスターの作成
{: #cs_cluster_tutorial}

このチュートリアルを使用すると、{{site.data.keyword.containerlong}} で Kubernetes クラスターをデプロイおよび管理することができます。 クラスターでのコンテナー化アプリのデプロイメント、操作、スケーリング、およびモニタリングを自動化する方法について説明します。
{:shortdesc}

このチュートリアル・シリーズでは、架空の PR 会社が Kubernetes 機能を使用してコンテナー化アプリを {{site.data.keyword.Bluemix_notm}} にデプロイする方法を示します。 この PR 会社は、{{site.data.keyword.toneanalyzerfull}} を利用してプレス・リリースを分析し、フィードバックを受け取ります。


## 達成目標
{: #tutorials_objectives}

この最初のチュートリアルでは、あなたがこの PR 会社のネットワーキング管理者になったつもりで学習を進めていきます。 あなたは、{{site.data.keyword.containerlong_notm}} でアプリの Hello World バージョンをデプロイしてテストするために使用されるカスタムの Kubernetes クラスターを構成します。
{:shortdesc}

-   1 つのワーカー・ノードがある 1 つのワーカー・プールを持つクラスターを作成します。
-   [Kubernetes コマンド ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubectl.docs.kubernetes.io/) を実行し、{{site.data.keyword.registrylong_notm}} で Docker イメージを管理するための CLI をインストールします。
-   イメージを格納するためのプライベート・イメージ・リポジトリーを {{site.data.keyword.registrylong_notm}} で作成します。
-   {{site.data.keyword.toneanalyzershort}} サービスをクラスターに追加して、そのサービスをクラスター内のすべてのアプリが使用できるようにします。


## 所要時間
{: #tutorials_time}

40 分


## 対象読者
{: #tutorials_audience}

このチュートリアルは、Kubernetes クラスターを初めて作成するソフトウェア開発者やネットワーク管理者を対象にしています。
{: shortdesc}

## 前提条件
{: #tutorials_prereqs}

-  [クラスター作成の準備](/docs/containers?topic=containers-clusters#cluster_prepare)に必要な手順を確認します。
-  以下のアクセス・ポリシーがあることを確認します。
    - {{site.data.keyword.containerlong_notm}} に対する[**管理者**の {{site.data.keyword.Bluemix_notm}} IAM プラットフォーム役割](/docs/containers?topic=containers-users#platform)
    - {{site.data.keyword.registrylong_notm}} に対する[**管理者**の {{site.data.keyword.Bluemix_notm}} IAM プラットフォーム役割](/docs/containers?topic=containers-users#platform)
    - {{site.data.keyword.containerlong_notm}} に対する[**ライター**または**管理者**の {{site.data.keyword.Bluemix_notm}} IAM サービス役割](/docs/containers?topic=containers-users#platform)


## レッスン 1: クラスターを作成して CLI をセットアップする
{: #cs_cluster_tutorial_lesson1}

{{site.data.keyword.Bluemix_notm}} コンソール内に Kubernetes クラスターを作成して必要な CLI をインストールします。
{: shortdesc}

**クラスターを作成するには、以下のようにします。**

クラスターがプロビジョンされるまでには数分かかることがあるため、CLI をインストールする前にクラスターを作成します。

1.  [{{site.data.keyword.Bluemix_notm}} コンソール ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン") で](https://cloud.ibm.com/kubernetes/catalog/cluster/create)、1 つのワーカー・ノードがある 1 つのワーカー・プールを持つフリー・クラスターまたは標準クラスターを作成します。

    [CLI でクラスター](/docs/containers?topic=containers-clusters#clusters_cli_steps)を作成することもできます。
    {: tip}

クラスターがプロビジョンされたら、クラスターを管理するために使用する以下の CLI をインストールします。
-   {{site.data.keyword.Bluemix_notm}} CLI
-   {{site.data.keyword.containerlong_notm}} プラグイン
-   Kubernetes CLI
-   {{site.data.keyword.registryshort_notm}} プラグイン

{{site.data.keyword.Bluemix_notm}} コンソールを代わりに使用する場合は、クラスターが作成された後、[Kubernetes Terminal](/docs/containers?topic=containers-cs_cli_install#cli_web) の Web ブラウザーから直接 CLI コマンドを実行できます。
{: tip}

</br>
**CLI とその前提条件をインストールするには、以下のようにします。**

1. [{{site.data.keyword.Bluemix_notm}} CLI ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン") をインストールします](/docs/cli?topic=cloud-cli-getting-started)。 このインストールには、以下が含まれます。
  - 基本 {{site.data.keyword.Bluemix_notm}} CLI。 {{site.data.keyword.Bluemix_notm}} CLI を使用してコマンドを実行するための接頭部は、`ibmcloud` です。
  - {{site.data.keyword.containerlong_notm}} プラグイン。 {{site.data.keyword.Bluemix_notm}} CLI を使用してコマンドを実行するための接頭部は、`ibmcloud ks` です。
  - {{site.data.keyword.registryshort_notm}} プラグイン。 このプラグインを使用して、{{site.data.keyword.registryshort_notm}} でプライベート・イメージ・リポジトリーをセットアップして管理します。 レジストリー・コマンドを実行するための接頭部は、`ibmcloud cr` です。

2. {{site.data.keyword.Bluemix_notm}} CLI にログインします。 プロンプトが出されたら、{{site.data.keyword.Bluemix_notm}} 資格情報を入力します。
  ```
  ibmcloud login
  ```
  {: pre}

  フェデレーテッド ID がある場合、`--sso` フラグを使用してログインします。 ユーザー名を入力し、CLI 出力に示された URL を使用して、ワンタイム・パスコードを取得してください。
  {: tip}

3. プロンプトに従ってアカウントを選択します。

5. プラグインが正しくインストールされていることを確認します。
  ```
  ibmcloud plugin list
  ```
  {: pre}

  {{site.data.keyword.containerlong_notm}} プラグインは **container-service** として結果に表示され、{{site.data.keyword.registryshort_notm}} プラグインは **container-registry** として結果に表示されます。

6. クラスターにアプリをデプロイするには、[Kubernetes CLI をインストールします ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/tasks/tools/install-kubectl/)。 Kubernetes CLI を使用してコマンドを実行するための接頭部は、`kubectl` です。

  1. 使用する予定の Kubernetes クラスターの `major.minor` バージョンと一致する Kubernetes CLI の `major.minor` バージョンをダウンロードします。 現在の {{site.data.keyword.containerlong_notm}} のデフォルト Kubernetes バージョンは 1.13.6 です。
    - **OS X**: [https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/darwin/amd64/kubectl ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/darwin/amd64/kubectl)
    - **Linux**: [https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/linux/amd64/kubectl ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/linux/amd64/kubectl)
    - **Windows**: [https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/windows/amd64/kubectl.exe ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/windows/amd64/kubectl.exe)

  2. OS X または Linux を使用している場合、以下の手順を実行します。

    1. 実行可能ファイルを `/usr/local/bin` ディレクトリーに移動します。
      ```
      mv /filepath/kubectl /usr/local/bin/kubectl
      ```
      {: pre}

    2. `/usr/local/bin` が `PATH` システム変数にリストされていることを確認します。 `PATH` 変数には、オペレーティング・システムが実行可能ファイルを見つけることのできるすべてのディレクトリーが含まれています。 `PATH` 変数にリストされた複数のディレクトリーには、それぞれ異なる目的があります。 `/usr/local/bin` は実行可能ファイルを保管するために使用されますが、保管対象となるのは、オペレーティング・システムの一部ではなく、システム管理者によって手動でインストールされたソフトウェアです。
      ```
      echo $PATH
      ```
      {: pre}
      CLI 出力例:
      ```
      /usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
      ```
      {: screen}

    3. ファイルを実行可能にします。
      ```
      chmod +x /usr/local/bin/kubectl
      ```
      {: pre}

お疲れさまでした。 CLI のインストールを正常に行うことができたので、次のレッスンとチュートリアルに進むことができます。 次に、クラスター環境をセットアップして {{site.data.keyword.toneanalyzershort}} サービスを追加します。


## レッスン 2: プライベート・レジストリーをセットアップする
{: #cs_cluster_tutorial_lesson2}

{{site.data.keyword.registryshort_notm}} でプライベート・イメージ・リポジトリーをセットアップし、シークレットを Kubernetes クラスターに追加して、アプリが {{site.data.keyword.toneanalyzershort}} サービスにアクセスできるようにします。
{: shortdesc}

1.  `default` 以外のリソース・グループ内にクラスターがある場合は、そのリソース・グループをターゲットとして設定します。 各クラスターが属するリソース・グループを表示するには、`ibmcloud ks clusters` を実行します。
   ```
   ibmcloud target -g <resource_group_name>
   ```
   {: pre}

2.  独自のプライベート・イメージ・リポジトリーを {{site.data.keyword.registryshort_notm}} にセットアップすることによって、Docker イメージを安全に保管し、すべてのクラスター・ユーザーと共有します。 {{site.data.keyword.Bluemix_notm}} 内のプライベート・イメージ・リポジトリーは、名前空間によって識別されます。 イメージ・リポジトリーの固有の URL を作成するために名前空間が使用されます。開発者はこれを使用してプライベート Dockerイメージにアクセスできます。

    コンテナー・イメージを使用する際の[個人情報の保護](/docs/containers?topic=containers-security#pi)の詳細を確認してください。

    この例で PR 会社はイメージ・リポジトリーを {{site.data.keyword.registryshort_notm}} に 1 つだけを作成するので、アカウント内のすべてのイメージをグループする名前空間として `pr_firm` を選択します。 &lt;namespace&gt; を、このチュートリアルに関係のない任意の名前空間に置き換えてください。

    ```
    ibmcloud cr namespace-add <namespace>
    ```
    {: pre}

3.  次のステップに進む前に、ワーカー・ノードのデプロイメントが完了したことを確認します。

    ```
    ibmcloud ks workers --cluster <cluster_name_or_ID>
    ```
    {: pre}

    ワーカー・ノードのプロビジョニングが終了すると、状況が **Ready** に変わり、{{site.data.keyword.Bluemix_notm}} サービスのバインドを開始できます。

    ```
    ID                                                 Public IP       Private IP       Machine Type   State    Status   Zone   Version
    kube-mil01-pafe24f557f070463caf9e31ecf2d96625-w1   169.xx.xxx.xxx   10.xxx.xx.xxx   free           normal   Ready    mil01      1.13.6
    ```
    {: screen}

## レッスン 3: クラスター環境をセットアップする
{: #cs_cluster_tutorial_lesson3}

CLI で Kubernetes クラスターのコンテキストを設定します。
{: shortdesc}

クラスターの作業を行うために {{site.data.keyword.containerlong}} CLI にログインするたびに、これらのコマンドを実行して、クラスターの構成ファイルのパスをセッション変数として設定する必要があります。 Kubernetes CLI はこの変数を使用して、{{site.data.keyword.Bluemix_notm}} 内のクラスターと接続するために必要なローカル構成ファイルと証明書を検索します。

1.  環境変数を設定して Kubernetes 構成ファイルをダウンロードするためのコマンドを取得します。
    ```
    ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
    ```
    {: pre}

    構成ファイルのダウンロードが完了すると、そのローカルの Kubernetes 構成ファイルのパスを環境変数として設定するために使用できるコマンドが表示されます。

    OS X の場合の例:
    ```
    export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/pr_firm_cluster/kube-config-prod-par02-pr_firm_cluster.yml
    ```
    {: screen}
    Windows PowerShell を使用していますか? Windows PowerShell 形式の環境変数を取得するには、`--powershell` フラグを含めます。
    {: tip}

2.  `KUBECONFIG` 環境変数を設定するためのコマンドとしてターミナルに表示されたものを、コピーして貼り付けます。

3.  `KUBECONFIG` 環境変数が適切に設定されたことを確認します。

    OS X の場合の例:

    ```
    echo $KUBECONFIG
    ```
    {: pre}

    出力:

    ```
    /Users/<user_name>/.bluemix/plugins/container-service/clusters/pr_firm_cluster/kube-config-prod-par02-pr_firm_cluster.yml
    ```
    {: screen}

4.  Kubernetes CLI サーバーのバージョンを調べて、ご使用のクラスターで `kubectl` コマンドが正常に実行されることを確認します。

    ```
    kubectl version  --short
    ```
    {: pre}

    出力例:

    ```
    Client Version: v1.13.6
    Server Version: v1.13.6
    ```
    {: screen}

## レッスン 4: クラスターにサービスを追加する
{: #cs_cluster_tutorial_lesson4}

{{site.data.keyword.Bluemix_notm}} サービスを使用すると、既に開発された機能をアプリで活用できます。 Kubernetes クラスターにバインドされているすべての {{site.data.keyword.Bluemix_notm}} サービスは、そのクラスターにデプロイされたアプリで使用できます。 アプリで使用する {{site.data.keyword.Bluemix_notm}} サービスごとに、以下の手順を繰り返してください。
{: shortdesc}

1.  {{site.data.keyword.toneanalyzershort}} サービスを {{site.data.keyword.Bluemix_notm}} アカウントに追加します。 <service_name> をサービス・インスタンスの名前に置き換えます。

    {{site.data.keyword.toneanalyzershort}} サービスをアカウントに追加すると、そのサービスが無料ではないことを示すメッセージが表示されます。 API 呼び出しを制限している場合には、このチュートリアルによって {{site.data.keyword.watson}} サービスからの課金は発生しません。 [{{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}} サービスの料金情報を確認します ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/catalog/services/tone-analyzer)。
    {: note}

    ```
    ibmcloud resource service-instance-create <service_name> tone-analyzer standard us-south
    ```
    {: pre}

2.  {{site.data.keyword.toneanalyzershort}} インスタンスをクラスターの `default` の Kubernetes 名前空間にバインドします。 あとで独自の名前空間を作成して Kubernetes リソースへのユーザー・アクセスを管理できますが、現時点では `default` 名前空間を使用します。 Kubernetes 名前空間は、以前に作成したレジストリー名前空間とは異なります。

    ```
    ibmcloud ks cluster-service-bind --cluster <cluster_name> --namespace default --service <service_name>
    ```
    {: pre}

    出力:

    ```
    ibmcloud ks cluster-service-bind pr_firm_cluster default mytoneanalyzer
    Binding service instance to namespace...
    OK
    Namespace:	default
    Secret name:	binding-mytoneanalyzer
    ```
    {: screen}

3.  クラスターの名前空間内に Kubernetes シークレットが作成されたことを確認します。 すべての {{site.data.keyword.Bluemix_notm}} サービスは、 {{site.data.keyword.Bluemix_notm}} IAM (ID およびアクセス管理) の API キーや、コンテナーがアクセスするために使用する URL などの機密情報を含む、 JSON ファイルによって定義されます。 この情報を安全に保管するために、Kubernetes シークレットが使用されます。 この例では、アカウントにプロビジョンされた {{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}} インスタンスにアクセスするための API キーがシークレットに格納されます。

    ```
    kubectl get secrets --namespace=default
    ```
    {: pre}

    出力:

    ```
    NAME                       TYPE                                  DATA      AGE
    binding-mytoneanalyzer     Opaque                                1         1m
    bluemix-default-secret     kubernetes.io/dockercfg               1         1h
    default-token-kf97z        kubernetes.io/service-account-token   3         1h
    ```
    {: screen}

</br>
おつかれさまでした。 クラスターが構成され、ローカル環境でアプリをクラスターにデプロイする準備が整いました。

## 次の作業
{: #tutorials_next}

* [クイズに答えて ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://ibmcloud-quizzes.mybluemix.net/containers/cluster_tutorial/quiz.php) 知識を確認しましょう!
* [チュートリアル: Kubernetes クラスターにアプリをデプロイする方法](/docs/containers?topic=containers-cs_apps_tutorial#cs_apps_tutorial)を試して、作成したクラスターに PR 会社のアプリをデプロイします。
