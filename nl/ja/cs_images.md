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



# イメージからのコンテナーのビルド
{: #images}

Docker イメージが、{{site.data.keyword.containerlong}} で作成するすべてのコンテナーの基礎です。
{:shortdesc}

イメージは、Dockerfile (イメージをビルドするための指示が入ったファイル) から作成されます。 Dockerfile の別個に保管されている指示の中で、ビルド成果物 (アプリ、アプリの構成、その従属関係) が参照されることもあります。

## イメージ・レジストリーの計画
{: #planning_images}

イメージは通常、レジストリーに保管されます。だれでもアクセスできるレジストリー (パブリック・レジストリー) を使用することも、小さなグループのユーザーだけにアクセスを限定したレジストリー (プライベート・レジストリー) をセットアップすることもできます。
{:shortdesc}

Docker Hub などのパブリック・レジストリーは、Docker および Kubernetes の入門として最初のコンテナー化アプリをクラスター内に作成する時に使用できます。 しかしエンタープライズ・アプリケーションを作成する場合は、許可されていないユーザーが勝手にイメージを使用したり変更したりすることがないように、 {{site.data.keyword.registryshort_notm}} で提供されているようなプライベート・レジストリーを使用してください。 プライベート・レジストリーはクラスター管理者がセットアップする必要があります。プライベート・レジストリーにアクセスするための資格情報をクラスター・ユーザーに提供する必要があるためです。


{{site.data.keyword.containerlong_notm}} では、複数のレジストリーを使用してアプリをクラスターにデプロイできます。

|レジストリー|説明|利点|
|--------|-----------|-------|
|[{{site.data.keyword.registryshort_notm}}](/docs/services/Registry?topic=registry-getting-started#getting-started)|このオプションを使用すると、保護された独自の Docker イメージ・リポジトリーを {{site.data.keyword.registryshort_notm}} 内にセットアップできます。そこでイメージを安全に保管してクラスター・ユーザー間で共有することができます。|<ul><li>アカウント内でイメージへのアクセス権限を管理できる。</li><li>{{site.data.keyword.IBM_notm}} 提供のイメージとサンプル・アプリ ({{site.data.keyword.IBM_notm}} Liberty など) を親イメージとして使用し、それに独自のアプリ・コードを追加できる。</li><li>Vulnerability Advisor によるイメージの潜在的脆弱性の自動スキャン (それらの脆弱性を修正するための OS 固有の推奨事項を含む)。</li></ul>|
|他のプライベート・レジストリー|[イメージ・プル・シークレット ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/concepts/containers/images/) を作成して、既存のプライベート・レジストリーをクラスターに接続します。 このシークレットは、レジストリーの URL と資格情報を Kubernetes シークレットに安全に保存するために使用されます。|<ul><li>ソース (Docker Hub、組織が所有するレジストリー、または他のプライベート・クラウド・レジストリー) と無関係に既存のプライベート・レジストリーを使用できる。</li></ul>|
|[パブリック Docker ハブ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://hub.docker.com/){: #dockerhub}|このオプションを使用すると、Dockerfile に変更を加える必要がない場合に、Docker Hub にある既存のパブリック・イメージを [Kubernetes デプロイメント ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) で直接使用できます。 <p>**注:** このオプションは組織のセキュリティー要件 (アクセス管理、脆弱性スキャン、アプリ・プライバシーなど) を満たさない可能性があるということに留意してください。</p>|<ul><li>クラスターに関して追加のセットアップは不要です。</li><li>さまざまなオープン・ソース・アプリケーションを含めることができる。</li></ul>|
{: caption="パブリック・イメージ・レジストリーおよびプライベート・イメージ・レジストリーのオプション" caption-side="top"}

イメージ・レジストリーをセットアップすると、クラスター・ユーザーがそれらのイメージを使用してクラスターにアプリをデプロイできるようになります。

コンテナー・イメージを使用する際の[個人情報の保護](/docs/containers?topic=containers-security#pi)の詳細を確認してください。

<br />


## コンテナー・イメージのための信頼できるコンテンツのセットアップ
{: #trusted_images}

署名ありで {{site.data.keyword.registryshort_notm}} に保管されている信頼できるイメージからコンテナーを構築できます。また、署名なしのイメージや脆弱なイメージのデプロイを防止できます。
{:shortdesc}

1.  [信頼できるコンテンツとしてイメージに署名します](/docs/services/Registry?topic=registry-registry_trustedcontent#registry_trustedcontent)。 イメージに信頼をセットアップしたら、信頼できるコンテンツと、レジストリーにイメージをプッシュできる署名者を管理できるようになります。
2.  署名されたイメージしかクラスターのコンテナー作成に使用できないようにするポリシーを適用するには、[Container Image Security Enforcement (ベータ) を追加します](/docs/services/Registry?topic=registry-security_enforce#security_enforce)。
3.  アプリをデプロイします。
    1. [`default` の Kubernetes 名前空間にデプロイします](#namespace)。
    2. [別の Kubernetes 名前空間にデプロイするか、別の {{site.data.keyword.Bluemix_notm}} 地域またはアカウントからデプロイします](#other)。

<br />


## {{site.data.keyword.registryshort_notm}} イメージから `default` の Kubernetes 名前空間へのコンテナーのデプロイ
{: #namespace}

IBM 提供のパブリック・イメージや {{site.data.keyword.registryshort_notm}} 名前空間に保管されているプライベート・イメージからクラスターにコンテナーをデプロイできます。 クラスターがレジストリー・イメージにアクセスする方法について詳しくは、[{{site.data.keyword.registrylong_notm}} からイメージをプルする権限がクラスターに与えられる仕組みについて](#cluster_registry_auth)を参照してください。
{:shortdesc}

開始前に、以下のことを行います。
1. [{{site.data.keyword.registryshort_notm}} に名前空間をセットアップして、その名前空間にイメージをプッシュします](/docs/services/Registry?topic=registry-getting-started#gs_registry_namespace_add)。
2. [クラスターを作成します](/docs/containers?topic=containers-clusters#clusters_ui)。
3. **2019 年 2 月 25 日**より前に作成された既存のクラスターを使用している場合は、[`imagePullSecret` という API キーを使用するようにそのクラスターを更新](#imagePullSecret_migrate_api_key)します。
4. [アカウントにログインします。 該当する場合は、適切なリソース・グループをターゲットにします。 クラスターのコンテキストを設定します。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

コンテナーをクラスターの **default** 名前空間内にデプロイするには、以下のステップを実行します。

1.  `mydeployment.yaml` という名前のデプロイメント構成ファイルを作成します。
2.  {{site.data.keyword.registryshort_notm}} 内の名前空間から使用するイメージおよびデプロイメントを定義します。

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

    イメージの URL の変数は、対象イメージの情報に置き換えます。
    *  **`<app_name>`**: アプリの名前。
    *  **`<region>`**: レジストリー・ドメインの地域 {{site.data.keyword.registryshort_notm}} API エンドポイント。 ログイン先の地域のドメインをリストするには、`ibmcloud cr api` を実行します。
    *  **`<namespace>`**: レジストリー名前空間。 名前空間の情報を取得するには、`ibmcloud cr namespace-list` を実行します。
    *  **`<my_image>:<tag>`**: コンテナーを作成するために使用するイメージとタグ。 レジストリー内にあるイメージを取得するには、`ibmcloud cr images` を実行します。

3.  クラスター内にデプロイメントを作成します。

    ```
    kubectl apply -f mydeployment.yaml
    ```
    {: pre}

<br />


## レジストリーからイメージをプルすることをクラスターに許可する方法について
{: #cluster_registry_auth}

レジストリーからイメージをプルするために、{{site.data.keyword.containerlong_notm}} クラスターは特別なタイプの Kubernetes シークレットである `imagePullSecret` を使用します。 このイメージ・プル・シークレットには、コンテナー・レジストリーにアクセスするための資格情報が保管されます。 このコンテナー・レジストリーは、{{site.data.keyword.registrylong_notm}} 内の独自の名前空間、異なる {{site.data.keyword.Bluemix_notm}} アカウントに属する {{site.data.keyword.registrylong_notm}} 内の名前空間、または Docker などの他の任意のプライベート・レジストリーのいずれかにできます。 クラスターは、{{site.data.keyword.registrylong_notm}} 内の独自の名前空間からイメージをプルして、これらのイメージからコンテナーをクラスター内の `default` Kubernetes 名前空間にデプロイするようにセットアップされています。 他のクラスター Kubernetes 名前空間内のイメージまたは他のレジストリー内のイメージをプルする必要がある場合は、イメージ・プル・シークレットをセットアップする必要があります。
{:shortdesc}

**`default` Kubernetes 名前空間からイメージをプルするようにクラスターをセットアップするには、どのようにしますか?**<br>
クラスターを作成すると、そのクラスターの {{site.data.keyword.Bluemix_notm}} IAM サービス ID に、{{site.data.keyword.registrylong_notm}} に対する **リーダー**の IAM サービス・アクセス役割ポリシーが与えられます。 このサービス ID の資格情報が擬人化され、有効期限のない API キーの中に含められ、API キーがクラスターのイメージ・プル・シークレットに保管されます。 そのイメージ・プル・シークレットが、Kubernetes 名前空間 `default` に追加され、その名前空間の `default` サービス・アカウントのシークレットのリストに含められます。 イメージ・プル・シークレットを使用すれば、デプロイメントから[グローバル・レジストリーと地域レジストリー](/docs/services/Registry?topic=registry-registry_overview#registry_regions)にあるイメージをプルして (読み取り専用アクセスを実行して)、Kubernetes 名前空間 `default` にコンテナーを作成できます。 グローバル・レジストリーには、ユーザーが各地域レジストリーに保管されたイメージを別個に参照する代わりにデプロイメント全体で参照することのできる、IBM 提供のパブリック・イメージが安全に保管されます。 地域レジストリーには、独自のプライベート Docker イメージが安全に保管されます。

**特定の地域レジストリーへのプル・アクセスを制限できますか?**<br>
はい。[サービス ID の既存の IAM ポリシーを編集](/docs/iam?topic=iam-serviceidpolicy#access_edit)して、**リーダー**のサービス・アクセス役割をその地域レジストリーやレジストリー・リソース (名前空間など) に制限できます。 レジストリーの IAM ポリシーをカスタマイズするには、[{{site.data.keyword.registrylong_notm}}](/docs/services/Registry?topic=registry-user#existing_users) で {{site.data.keyword.Bluemix_notm}} IAM ポリシーを有効にしておく必要があります。

  レジストリー資格情報の安全性をさらに高めたいですか? クラスター管理者に連絡してクラスター内の [{{site.data.keyword.keymanagementservicefull}} を有効にして](/docs/containers?topic=containers-encryption#keyprotect)もらい、レジストリー資格情報を保管する `imagePullSecret` などの、クラスター内の Kubernetes シークレットを暗号化します。
  {: tip}

**`default` 以外の Kubernetes 名前空間内のイメージをプルできますか?**<br>
デフォルトではできません。 デフォルトのクラスター・セットアップを使用することで、{{site.data.keyword.registrylong_notm}} 名前空間内に保管されている任意のイメージからのコンテナーを、クラスターの `default` Kubernetes 名前空間にデプロイできます。 これらのイメージを他の Kubernetes 名前空間や他の {{site.data.keyword.Bluemix_notm}} アカウントで使用する場合は、[既存のイメージ・プル・シークレットをコピーするか、独自のイメージ・プル・シークレットを作成するかを選択できます](#other)。

**異なる {{site.data.keyword.Bluemix_notm}} アカウントからイメージをプルできますか?**<br>
はい。使用する {{site.data.keyword.Bluemix_notm}} アカウントで API キーを作成してください。 次に、プル元となる各クラスター内と各クラスター名前空間内のこれらの API キー資格情報を保管するイメージ・プル・シークレットを作成します。 [許可済みのサービス ID API キーが使用されるこちら例の手順に従ってください](#other_registry_accounts)。

Docker などの非 {{site.data.keyword.Bluemix_notm}} レジストリーを使用する場合は、[他のプライベート・レジストリーに格納されたイメージへのアクセス](#private_images)を参照してください。

**API キーはサービス ID 用である必要がありますか? 自分のアカウントのサービス ID の制限に達した場合はどうなりますか?**<br>
デフォルトのクラスター・セットアップでは、{{site.data.keyword.Bluemix_notm}} IAM API キー資格情報をイメージ・プル・シークレットに保管するためのサービス ID が作成されます。 ただし、個別ユーザー用の API キーを作成して、それらの資格情報をイメージ・プル・シークレットに保管することもできます。 [サービス ID の IAM 制限](/docs/iam?topic=iam-iam_limits#iam_limits)に達した場合は、クラスターはサービス ID とイメージ・プル・シークレットなしで作成されるため、デフォルトではそのクラスターは `icr.io` レジストリー・ドメインからイメージをプルすることはできません。 [独自のイメージ・プル・シークレットを作成](#other_registry_accounts)する必要がありますが、そのためには、{{site.data.keyword.Bluemix_notm}} IAM サービス ID ではなく、部門 ID などの個別ユーザー用の API キーを使用します。

**使用しているクラスターのイメージ・プル・シークレットではレジストリー・トークンが使用されます。 トークンは現在でも機能しますか?**<br>

{{site.data.keyword.registrylong_notm}} にアクセスする権限をクラスターに与えるために、以前は、自動で[トークン](/docs/services/Registry?topic=registry-registry_access#registry_tokens)が生成され、イメージ・プル・シークレットに保管されていました。現在、この方式はサポートされますが、非推奨になっています。
{: deprecated}

トークンによって、非推奨となった `registry.bluemix.net` レジストリー・ドメインへのアクセスが許可されるのに対して、API キーによって、`icr.io` レジストリー・ドメインへのアクセスが許可されます。 トークン・ベースの認証から API キー・ベースの認証への移行期間中は、トークン・ベースと API キー・ベースの両方のイメージ・プル・シークレットが当面は作成されます。 トークン・ベースと API キー・ベースの両方のイメージ・プル・シークレットを使用した場合、クラスターは、`default` Kubernetes 名前空間内の `registry.bluemix.net` ドメインまたは `icr.io` ドメインからイメージをプルできます。

非推奨のトークンと `registry.bluemix.net` ドメインがサポート対象外になる前に、クラスターのイメージ・プル・シークレットを更新して、[`default` Kubernetes 名前空間](#imagePullSecret_migrate_api_key)および使用する可能性のある[他の任意の名前空間またはアカウント](#other)で API キー方式が使用されるようにしてください。 次に、`icr.io` レジストリー・ドメインからプルするようにデプロイメントを更新してください。

**イメージ・プル・シークレットを別の Kubernetes 名前空間にコピーまたは作成したら、完了ですか?**<br>
十分ではありません。コンテナーが、作成したシークレットを使用してイメージをプルすることを許可されている必要があります。イメージ・プル・シークレットを名前空間のサービス・アカウントに追加することも、各デプロイメントでシークレットを参照することもできます。手順については、[イメージ・プル・シークレットを使用したコンテナーのデプロイ](/docs/containers?topic=containers-images#use_imagePullSecret)を参照してください。

<br />


## API キーのイメージ・プル・シークレットを使用するように既存のクラスターを更新する
{: #imagePullSecret_migrate_api_key}

新しい {{site.data.keyword.containerlong_notm}} クラスターでは、[{{site.data.keyword.registrylong_notm}} へのアクセス権限を与えるために、イメージ・プル・シークレット](#cluster_registry_auth)に API キーを保管します。 それらのイメージ・プル・シークレットを使用すれば、`icr.io` レジストリー・ドメインに保管されているイメージからコンテナーをデプロイできます。 **2019 年 2 月 25 日**より前に作成されたクラスターは、レジストリー・トークンではなく API キーをイメージ・プル・シークレットに保管するように更新する必要があります。
{: shortdesc}

**始める前に**:
*   [アカウントにログインします。 該当する場合は、適切なリソース・グループをターゲットにします。 クラスターのコンテキストを設定します。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
*   以下のアクセス権を持っていることを確認します。
    *   {{site.data.keyword.containerlong_notm}} に対する {{site.data.keyword.Bluemix_notm}} IAM の**オペレーターまたは管理者**のプラットフォーム役割。 アカウント所有者は、次のコマンドを実行することでユーザーにこの役割を付与できます。
        ```
        ibmcloud iam user-policy-create <your_user_email> --service-name containers-kubernetes --roles Administrator,Operator
        ```
        {: pre}
    *   すべての地域とリソース・グループにまたがる、{{site.data.keyword.registrylong_notm}} に対する {{site.data.keyword.Bluemix_notm}} IAM の**管理者**プラットフォーム役割。 アカウント所有者は、次のコマンドを実行することでユーザーにこの役割を付与できます。
        ```
        ibmcloud iam user-policy-create <your_user_email> --service-name container-registry --roles Administrator
        ```
        {: pre}

**`default` Kubernetes 名前空間内のクラスターのイメージ・プル・シークレットを更新するには、以下のようにします**。
1.  クラスター ID を取得します。
    ```
    ibmcloud ks clusters
    ```
    {: pre}
2.  以下のコマンドを実行して、クラスターのサービス ID を作成し、{{site.data.keyword.registrylong_notm}} に対する IAM の**リーダー**のサービス役割をそのサービス ID に割り当て、そのサービス ID の資格情報を擬人化するための API キーを作成し、その API キーをクラスターの Kubernetes イメージ・プル・シークレットに保管します。 このイメージ・プル・シークレットは、Kubernetes 名前空間 `default` にあります。
    ```
    ibmcloud ks cluster-pull-secret-apply --cluster <cluster_name_or_ID>
    ```
    {: pre}

    このコマンドを実行すると、IAM 資格情報とイメージ・プル・シークレットの作成が開始されます。これが完了するまで、しばらく時間がかかる場合があります。 イメージ・プル・シークレットが作成されるまでは、{{site.data.keyword.registrylong_notm}} `icr.io` ドメインからイメージをプルするコンテナーをデプロイできません。
    {: important}

3.  クラスター内にイメージ・プル・シークレットが作成されていることを確認します。 {{site.data.keyword.registrylong_notm}} 地域ごとに別々のイメージ・プル・シークレットが作成されます。
    ```
    kubectl get secrets
    ```
    {: pre}
    出力例:
    ```
    default-us-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-uk-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-de-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-au-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-jp-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-icr-io                             kubernetes.io/dockerconfigjson        1         16d
    ```
    {: screen}
4.  `icr.io` ドメイン名からイメージをプルするようコンテナー・デプロイメントを更新します。
5.  オプション: ファイアウォールがある場合は、使用するドメインで[レジストリー・サブネットへのアウトバウンド・ネットワーク・トラフィックを許可](/docs/containers?topic=containers-firewall#firewall_outbound)してください。

**次の作業**
*   `default` 以外の Kubernetes 名前空間にイメージをプルしたり、他の {{site.data.keyword.Bluemix_notm}} アカウントからイメージをプルしたりする場合は、[別のイメージ・プル・シークレットをコピーまたは作成します](/docs/containers?topic=containers-images#other)。
*   イメージ・プル・シークレットのアクセスを特定のレジストリー・リソース (名前空間や地域など) に制限する場合は、以下のようにします。
    1.  [{{site.data.keyword.registrylong_notm}} に対する {{site.data.keyword.Bluemix_notm}} IAM ポリシーが有効になっている](/docs/services/Registry?topic=registry-user#existing_users)ことを確認します。
    2.  サービス ID の [{{site.data.keyword.Bluemix_notm}} IAM ポリシーを編集](/docs/iam?topic=iam-serviceidpolicy#access_edit)するか、[別のイメージ・プル・シークレットを作成](/docs/containers?topic=containers-images#other_registry_accounts)します。

<br />


## イメージ・プル・シークレットを使用して、他のクラスター Kubernetes 名前空間、他の {{site.data.keyword.Bluemix_notm}} アカウント、または外部プライベート・レジストリーにアクセスする方法
{: #other}

クラスター内の独自のイメージ・プル・シークレットのセットアップ内容に応じて、`default` 以外の Kubernetes 名前空間にコンテナーをデプロイしたり、他の {{site.data.keyword.Bluemix_notm}} アカウントに保管されているイメージを使用したり、外部プライベート・レジストリーに保管されているイメージを使用したりできます。 さらに、独自のイメージ・プル・シークレットを作成して、特定のレジストリー・イメージ名前空間または操作 (`push` や `pull` など) にアクセス権を制限する IAM アクセス・ポリシーを適用することも可能です。
{:shortdesc}

イメージ・プル・シークレットを作成したら、コンテナーがそのシークレットを使用してレジストリーからイメージをプルすることを許可する必要があります。イメージ・プル・シークレットを名前空間のサービス・アカウントに追加することも、各デプロイメントでシークレットを参照することもできます。手順については、[イメージ・プル・シークレットを使用したコンテナーのデプロイ](/docs/containers?topic=containers-images#use_imagePullSecret)を参照してください。

イメージ・プル・シークレットは、それらが作成された対象の Kubernetes 名前空間に対してのみ有効です。 コンテナーをデプロイする名前空間ごとに、以下の手順を繰り返してください。 [DockerHub](#dockerhub) のイメージの場合はイメージ・プル・シークレットは必要ありません。
{: tip}

開始前に、以下のことを行います。

1.  [{{site.data.keyword.registryshort_notm}} に名前空間をセットアップして、その名前空間にイメージをプッシュします](/docs/services/Registry?topic=registry-getting-started#gs_registry_namespace_add)。
2.  [クラスターを作成します](/docs/containers?topic=containers-clusters#clusters_ui)。
3.  **2019 年 2 月 25 日**より前に作成された既存のクラスターを使用している場合は、[API キーのイメージ・プル・シークレットを使用するようにそのクラスターを更新](#imagePullSecret_migrate_api_key)します。
4.  [アカウントにログインします。 該当する場合は、適切なリソース・グループをターゲットにします。 クラスターのコンテキストを設定します。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

<br/>
独自のイメージ・プル・シークレットを使用する方法としては、以下の選択肢があります。
- default の Kubernetes 名前空間からクラスター内の他の名前空間に[イメージ・プル・シークレットをコピーする](#copy_imagePullSecret)。
- [新しい IAM API キー資格情報を作成して、それらをイメージ・プル・シークレット内に保管](#other_registry_accounts)することで、他の {{site.data.keyword.Bluemix_notm}} アカウント内のイメージにアクセスしたり、特定のレジストリー・ドメインやレジストリー名前空間へのアクセスを制限する IAM ポリシーを適用したりする。
- [外部プライベート・レジストリーのイメージにアクセスするためのイメージ・プル・シークレットを作成する](#private_images)。

<br/>
デプロイメントで使用する名前空間にイメージ・プル・シークレットを既に作成した場合は、[作成済みの `imagePullSecret` を使用したコンテナーのデプロイ](#use_imagePullSecret)を参照してください。

### 既存のイメージ・プル・シークレットのコピー
{: #copy_imagePullSecret}

イメージ・プル・シークレットをクラスター内の他の名前空間にコピーできます (`default` Kubernetes 名前空間用に自動的に作成されたイメージ・プル・シークレットなど)。 例えば、特定の名前空間へのアクセスを制限するために、または他の {{site.data.keyword.Bluemix_notm}} アカウントからイメージをプルするために、この名前空間で別の {{site.data.keyword.Bluemix_notm}} IAM API キー資格情報を使用する必要がある場合は、代わりに、[イメージ・プル・シークレットを作成します](#other_registry_accounts)。
{: shortdesc}

1.  クラスター内に存在する Kubernetes 名前空間をリストするか、使用する名前空間を作成します。
    ```
    kubectl get namespaces
    ```
    {: pre}

    出力例:
    ```
    default          Active    79d
   ibm-cert-store   Active    79d
   ibm-system       Active    79d
   istio-system     Active    34d
   kube-public      Active    79d
   kube-system      Active    79d
    ```
    {: screen}

    名前空間を作成するには、以下のようにします。
    ```
    kubectl create namespace <namespace_name>
    ```
    {: pre}
2.  {{site.data.keyword.registrylong_notm}} の Kubernetes 名前空間 `default` にある既存のイメージ・プル・シークレットをリストします。
    ```
    kubectl get secrets -n default | grep icr
    ```
    {: pre}
    出力例:
    ```
    default-us-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-uk-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-de-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-au-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-jp-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-icr-io                             kubernetes.io/dockerconfigjson        1         16d
    ```
    {: screen}
3.  `default` 名前空間から任意の名前空間に各イメージ・プル・シークレットをコピーします。 新しいイメージ・プル・シークレットは `<namespace_name>-icr-<region>-io` という名前になります。
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
4.  シークレットが正常に作成されたことを確認します。
    ```
    kubectl get secrets -n <namespace_name>
    ```
    {: pre}
5.  [コンテナーをデプロイする際に、名前空間内の任意のポッドでそのイメージ・プル・シークレットを使用できるよう、イメージ・プル・シークレットを Kubernetes サービス・アカウントに追加します](#use_imagePullSecret)。

### 異なる IAM API キー資格情報を使用してイメージ・プル・シークレットを作成することで、より細かい制御を可能にしたり、他の {{site.data.keyword.Bluemix_notm}} アカウント内のイメージへのアクセスを可能にしたりする方法
{: #other_registry_accounts}

{{site.data.keyword.Bluemix_notm}} の IAM アクセス・ポリシーをユーザーまたはサービス ID に割り当てて、特定のレジストリー・イメージ名前空間または操作 (`push` や `pull` など) にアクセス権を制限できます。次に、API キーを作成して、これらのレジストリー資格情報をクラスターのイメージ・プル・シークレットに保管します。
{: shortdesc}

例えば、他の {{site.data.keyword.Bluemix_notm}} アカウント内のイメージにアクセスするには、そのアカウント内のユーザーまたはサービス ID の {{site.data.keyword.registryshort_notm}} 資格情報を保管する API キーを作成します。 次に、クラスターのアカウント内で、それぞれのクラスターとクラスター名前空間のイメージ・プル・シークレットにその API キー資格情報を保存します。

以下のステップでは、{{site.data.keyword.Bluemix_notm}} の IAM サービス ID の資格情報を保管する API キーを作成します。 サービス ID を使用する代わりに、{{site.data.keyword.registryshort_notm}} に対する {{site.data.keyword.Bluemix_notm}} IAM サービス・アクセス・ポリシーを割り当てたユーザー ID の API キーを作成することもできます。 ただし、そのユーザー ID は部門 ID にしてください。あるいは、その特定のユーザーがいなくなってもクラスターがレジストリーにアクセスできるように計画しておいてください。
{: note}

1.  クラスター内に存在する Kubernetes 名前空間をリストするか、レジストリー・イメージからコンテナーをデプロイする場所として使用する名前空間を作成します。
    ```
    kubectl get namespaces
    ```
    {: pre}

    出力例:
    ```
    default          Active    79d
   ibm-cert-store   Active    79d
   ibm-system       Active    79d
   istio-system     Active    34d
   kube-public      Active    79d
   kube-system      Active    79d
    ```
    {: screen}

    名前空間を作成するには、以下のようにします。
    ```
    kubectl create namespace <namespace_name>
    ```
    {: pre}
2.  イメージ・プル・シークレットの IAM ポリシーと API キー資格情報のために使用するクラスターの {{site.data.keyword.Bluemix_notm}} IAM サービス ID を作成します。 サービス ID には、後からサービス ID を特定できるように、必ず説明を付けておいてください。例えば、クラスター名と名前空間名の両方を記載してください。
    ```
    ibmcloud iam service-id-create <cluster_name>-<kube_namespace>-id --description "Service ID for IBM Cloud Container Registry in Kubernetes cluster <cluster_name> namespace <kube_namespace>"
    ```
    {: pre}
3.  {{site.data.keyword.registryshort_notm}} へのアクセス権を与えるカスタム {{site.data.keyword.Bluemix_notm}} IAM ポリシーを、クラスター・サービス ID に対して作成します。
    ```
    ibmcloud iam service-policy-create <cluster_service_ID> --roles <service_access_role> --service-name container-registry [--region <IAM_region>] [--resource-type namespace --resource <registry_namespace>]
    ```
    {: pre}
    <table>
    <caption>このコマンドの構成要素について</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> このコマンドの構成要素について</th>
    </thead>
    <tbody>
    <tr>
    <td><code><em>&lt;cluster_service_ID&gt;</em></code></td>
    <td>必須。 Kubernetes クラスターのために前に作成した `<cluster_name>-<kube_namespace>-id` のサービス ID に置き換えます。</td>
    </tr>
    <tr>
    <td><code>--service-name <em>container-registry</em></code></td>
    <td>必須。 `container-registry` を入力して、IAM ポリシーの対象を {{site.data.keyword.registrylong_notm}} にします。</td>
    </tr>
    <tr>
    <td><code>--roles <em>&lt;service_access_role&gt;</em></code></td>
    <td>必須。 サービス ID のアクセス権の有効範囲を指定するために、[{{site.data.keyword.registrylong_notm}} の サービス・アクセス役割](/docs/services/Registry?topic=registry-iam#service_access_roles)を入力します。 指定できる値は、`Reader`、`Writer`、`Manager` です。</td>
    </tr>
    <tr>
    <td><code>--region <em>&lt;IAM_region&gt;</em></code></td>
    <td>オプション。 アクセス・ポリシーの有効範囲を特定の IAM 地域に制限する場合は、コンマ区切りリスト形式で地域を入力します。 指定できる値は、`au-syd`、`eu-gb`、`eu-de`、`jp-tok`、`us-south`、`global` です。</td>
    </tr>
    <tr>
    <td><code>--resource-type <em>namespace</em> --resource <em>&lt;registry_namespace&gt;</em></code></td>
    <td>オプション。 特定の [{{site.data.keyword.registrylong_notm}}名前空間](/docs/services/Registry?topic=registry-registry_overview#registry_namespaces)にあるイメージだけにアクセス権を制限する場合は、リソース・タイプとして `namespace` と入力し、`<registry_namespace>` を指定します。 レジストリーの名前空間をリストするには、`ibmcloud cr namespaces` を実行します。</td>
    </tr>
    </tbody></table>
4.  サービス ID の API キーを作成します。 サービス ID と似た名前になるように、前に作成したサービス ID (``<cluster_name>-<kube_namespace>-id) が入った名前を API キーに設定します。 API キーには、後からそのキーを特定できるように、必ず説明を付けておいてください。
    ```
    ibmcloud iam service-api-key-create <cluster_name>-<kube_namespace>-key <cluster_name>-<kube_namespace>-id --description "API key for service ID <service_id> in Kubernetes cluster <cluster_name> namespace <kube_namespace>"
    ```
    {: pre}
5.  前のコマンドの出力から **API キー**の値を取得します。
    ```
    API キーを保存してください。 作成後は表示できなくなります。

    Name          <cluster_name>-<kube_namespace>-key   
    Description   key_for_registry_for_serviceid_for_kubernetes_cluster_multizone_namespace_test   
    Bound To      crn:v1:bluemix:public:iam-identity::a/1bb222bb2b33333ddd3d3333ee4ee444::serviceid:ServiceId-ff55555f-5fff-6666-g6g6-777777h7h7hh   
    Created At    2019-02-01T19:06+0000   
    API Key       i-8i88ii8jjjj9jjj99kkkkkkkkk_k9-llllll11mmm1   
    Locked        false   
    UUID          ApiKey-222nn2n2-o3o3-3o3o-4p44-oo444o44o4o4   
    ```
    {: screen}
6.  クラスターの名前空間に、API キーの資格情報を保管するための Kubernetes イメージ・プル・シークレットを作成します。 このサービス ID の IAM 資格情報を使用してレジストリーからイメージをプルしたい `icr.io` ドメイン、Kubernetes 名前空間、クラスターごとに、この手順を繰り返します。
    ```
    kubectl --namespace <kubernetes_namespace> create secret docker-registry <secret_name> --docker-server=<registry_URL> --docker-username=iamapikey --docker-password=<api_key_value> --docker-email=<docker_email>
    ```
    {: pre}

    <table>
    <caption>このコマンドの構成要素について</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> このコマンドの構成要素について</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace <em>&lt;kubernetes_namespace&gt;</em></code></td>
    <td>必須。 サービス ID 名に使用したクラスターの Kubernetes 名前空間を指定します。</td>
    </tr>
    <tr>
    <td><code><em>&lt;secret_name&gt;</em></code></td>
    <td>必須。 イメージ・プル・シークレットの名前を入力します。</td>
    </tr>
    <tr>
    <td><code>--docker-server <em>&lt;registry_URL&gt;</em></code></td>
    <td>必須。 レジストリーの名前空間がセットアップされているイメージ・レジストリーの URL を設定します。 使用可能なレジストリー・ドメインは以下のとおりです。<ul>
    <li>北アジア太平洋地域 (東京): `jp.icr.io`</li>
    <li>南アジア太平洋地域 (シドニー): `au.icr.io`</li>
    <li>中欧 (フランクフルト): `de.icr.io`</li>
    <li>英国南部 (ロンドン): `uk.icr.io`</li>
    <li>米国南部 (ダラス): `us.icr.io`</li></ul></td>
    </tr>
    <tr>
    <td><code>--docker-username iamapikey</code></td>
    <td>必須。 プライベート・レジストリーにログインするためのユーザー名を入力します。 {{site.data.keyword.registryshort_notm}} の場合、ユーザー名は値 <strong><code>iamapikey</code></strong> に設定されます。</td>
    </tr>
    <tr>
    <td><code>--docker-password <em>&lt;token_value&gt;</em></code></td>
    <td>必須。 前に取得した **API キー**の値を入力します。</td>
    </tr>
    <tr>
    <td><code>--docker-email <em>&lt;docker-email&gt;</em></code></td>
    <td>必須。 Docker E メール・アドレスがある場合は、その値を入力します。 ない場合は、`a@b.c` のような架空の E メール・アドレスを入力します。 この E メールは、Kubernetes シークレットを作成する際には必須ですが、作成後は使用されません。</td>
    </tr>
    </tbody></table>
7.  シークレットが正常に作成されたことを確認します。 <em>&lt;kubernetes_namespace&gt;</em> を、イメージ・プル・シークレットを作成した名前空間に置き換えます。

    ```
    kubectl get secrets --namespace <kubernetes_namespace>
    ```
    {: pre}
8.  [コンテナーをデプロイする際に、名前空間内の任意のポッドでそのイメージ・プル・シークレットを使用できるよう、イメージ・プル・シークレットを Kubernetes サービス・アカウントに追加します](#use_imagePullSecret)。

### 他のプライベート・レジストリー内に保管されているイメージへのアクセス
{: #private_images}

プライベート・レジストリーが既にある場合は、そのレジストリーの資格情報を Kubernetes イメージ・プル・シークレットに保管し、構成ファイルからそのシークレットを参照する必要があります。
{:shortdesc}

開始前に、以下のことを行います。

1.  [クラスターを作成します](/docs/containers?topic=containers-clusters#clusters_ui)。
2.  [CLI のターゲットを自分のクラスターに設定します](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)。

イメージ・プル・シークレットを作成するには、以下のようにします。

1.  プライベート・レジストリーの資格情報を保管する Kubernetes シークレットを作成します。

    ```
    kubectl --namespace <kubernetes_namespace> create secret docker-registry <secret_name>  --docker-server=<registry_URL> --docker-username=<docker_username> --docker-password=<docker_password> --docker-email=<docker_email>
    ```
    {: pre}

    <table>
    <caption>このコマンドの構成要素について</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> このコマンドの構成要素について</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace <em>&lt;kubernetes_namespace&gt;</em></code></td>
    <td>必須。 シークレットを使用してコンテナーをデプロイする、クラスターの Kubernetes 名前空間。 クラスター内の名前空間をすべてリストするには、<code>kubectl get namespaces</code> を実行します。</td>
    </tr>
    <tr>
    <td><code><em>&lt;secret_name&gt;</em></code></td>
    <td>必須。 <code>imagePullSecret</code> に使用する名前。</td>
    </tr>
    <tr>
    <td><code>--docker-server <em>&lt;registry_URL&gt;</em></code></td>
    <td>必須。 プライベート・イメージが保管されているレジストリーの URL。</td>
    </tr>
    <tr>
    <td><code>--docker-username <em>&lt;docker_username&gt;</em></code></td>
    <td>必須。 プライベート・レジストリーにログインするためのユーザー名。</td>
    </tr>
    <tr>
    <td><code>--docker-password <em>&lt;token_value&gt;</em></code></td>
    <td>必須。 以前に取得したレジストリー・トークンの値。</td>
    </tr>
    <tr>
    <td><code>--docker-email <em>&lt;docker-email&gt;</em></code></td>
    <td>必須。 Docker E メール・アドレスがある場合は、その値を入力します。 ない場合は、`a@b.c` のような架空の E メール・アドレスを入力します。 この E メールは、Kubernetes シークレットを作成する際には必須ですが、作成後は使用されません。</td>
    </tr>
    </tbody></table>

2.  シークレットが正常に作成されたことを確認します。 <em>&lt;kubernetes_namespace&gt;</em> を、`imagePullSecret` を作成した名前空間の名前に置き換えます。

    ```
    kubectl get secrets --namespace <kubernetes_namespace>
    ```
    {: pre}

3.  [イメージ・プル・シークレットを参照するポッドを作成します](#use_imagePullSecret)。

<br />


## イメージ・プル・シークレットを使用したコンテナーのデプロイ
{: #use_imagePullSecret}

ポッド・デプロイメントにイメージ・プル・シークレットを定義するか、Kubernetes サービス・アカウントにイメージ・プル・シークレットを保管すると、サービス・アカウントを指定しないすべてのデプロイメントでイメージ・プル・シークレットを使用できるようになります。
{: shortdesc}

次のいずれかのオプションを選択します。
* [ポッド・デプロイメントでイメージ・プル・シークレットを参照する](#pod_imagePullSecret): 名前空間内のすべてのポッドにデフォルトでレジストリーへのアクセス権限を付与したくない場合は、このオプションを使用します。
* [Kubernetes サービス・アカウントにイメージ・プル・シークレットを保管する](#store_imagePullSecret): 選択した Kubernetes 名前空間内のすべてのデプロイメントについて、レジストリー内のイメージへのアクセス権限を付与する場合は、このオプションを使用します。

開始前に、以下のことを行います。
* 他のレジストリー内や `default` 以外の Kubernetes 名前空間内のイメージにアクセスするための[イメージ・プル・シークレットを作成します](#other)。
* [CLI のターゲットを自分のクラスターに設定します](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)。

### ポッド・デプロイメントでイメージ・プル・シークレットを参照する
{: #pod_imagePullSecret}

ポッド・デプロイメントでイメージ・プル・シークレットを参照する場合、そのイメージ・プル・シークレットはそのポッドでのみ有効です。名前空間内の複数のポッドで共有することはできません。
{:shortdesc}

1.  `mypod.yaml` という名前のポッド構成ファイルを作成します。
2.  {{site.data.keyword.registrylong_notm}} 内のイメージにアクセスするためのイメージ・プル・シークレットとポッドを定義します。

    プライベート・イメージにアクセスするには、以下のようにします。
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

    {{site.data.keyword.Bluemix_notm}} パブリック・イメージにアクセスするには、以下のようにします。
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
    <caption>YAML ファイルの構成要素について</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> YAML ファイルの構成要素について</th>
    </thead>
    <tbody>
    <tr>
    <td><code><em>&lt;container_name&gt;</em></code></td>
    <td>クラスターにデプロイするコンテナーの名前。</td>
    </tr>
    <tr>
    <td><code><em>&lt;namespace_name&gt;</em></code></td>
    <td>イメージが保管されている名前空間。 使用可能な名前空間をリストするには、`ibmcloud cr namespace-list` を実行します。</td>
    </tr>
    <tr>
    <td><code><em>&lt;image_name&gt;</em></code></td>
    <td>使用するイメージの名前。 {{site.data.keyword.Bluemix_notm}} アカウント内の使用可能なイメージをリストするには、`ibmcloud cr image-list` を実行します。</td>
    </tr>
    <tr>
    <td><code><em>&lt;tag&gt;</em></code></td>
    <td>使用するイメージのバージョン。 タグを指定しないと、デフォルトでは <strong>latest</strong> のタグが付いたイメージが使用されます。</td>
    </tr>
    <tr>
    <td><code><em>&lt;secret_name&gt;</em></code></td>
    <td>以前に作成したイメージ・プル・シークレットの名前。</td>
    </tr>
    </tbody></table>

3.  変更を保存します。
4.  クラスター内にデプロイメントを作成します。
    ```
    kubectl apply -f mypod.yaml
    ```
    {: pre}

### 選択した名前空間の Kubernetes サービス・アカウントにイメージ・プル・シークレットを保管する
{:#store_imagePullSecret}

すべての名前空間に、`default` という名前の Kubernetes サービス・アカウントがあります。 イメージ・プル・シークレットをこのサービス・アカウントに追加すると、レジストリー内のイメージへのアクセス権限を付与できます。 サービス・アカウントを指定しないデプロイメントでは、その名前空間の `default` サービス・アカウントが自動的に使用されます。
{:shortdesc}

1. default サービス・アカウントにイメージ・プル・シークレットが既に存在しているかどうかを確認します。
   ```
   kubectl describe serviceaccount default -n <namespace_name>
   ```
   {: pre}
   **イメージ・プル・シークレット**の項目に `<none>` が表示された場合、イメージ・プル・シークレットは存在しません。  
2. イメージ・プル・シークレットを default サービス・アカウントに追加します。
   - **イメージ・プル・シークレットが定義されていない場合に、イメージ・プル・シークレットを追加するには、以下のようにします。**
       ```
       kubectl patch -n <namespace_name> serviceaccount/default -p '{"imagePullSecrets":[{"name": "<image_pull_secret_name>"}]}'
       ```
       {: pre}
   - **イメージ・プル・シークレットが既に定義されている場合にイメージ・プル・シークレットを追加するには、以下のようにします。**
       ```
       kubectl patch -n <namespace_name> serviceaccount/default --type='json' -p='[{"op":"add","path":"/imagePullSecrets/-","value":{"name":"<image_pull_secret_name>"}}]'
       ```
       {: pre}
3. イメージ・プル・シークレットが default サービス・アカウントに追加されたことを確認します。
   ```
   kubectl describe serviceaccount default -n <namespace_name>
   ```
   {: pre}

   出力例:
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

4. レジストリーのイメージからコンテナーをデプロイします。
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

5. クラスター内にデプロイメントを作成します。
   ```
   kubectl apply -f mypod.yaml
   ```
   {: pre}

<br />


## 非推奨: レジストリー・トークンを使用して {{site.data.keyword.registrylong_notm}} イメージからコンテナーをデプロイする方法
{: #namespace_token}

{{site.data.keyword.registryshort_notm}} 内の名前空間に保管されている IBM 提供のパブリック・イメージやプライベート・イメージからクラスターにコンテナーをデプロイできます。 既存のクラスターでは、`registry.bluemix.net` ドメイン名からイメージをプルするアクセス権限を与えるために、クラスターの `imagePullSecret` に保管されたレジストリー・[トークン](/docs/services/Registry?topic=registry-registry_access#registry_tokens)を使用しています。
{:shortdesc}

クラスターを作成すると、有効期限のないレジストリー・トークンおよびシークレットが、[最も近い地域レジストリーとグローバル・レジストリー](/docs/services/Registry?topic=registry-registry_overview#registry_regions)の両方に対して自動的に作成されます。 グローバル・レジストリーには、ユーザーが各地域レジストリーに保管されたイメージを別個に参照する代わりにデプロイメント全体で参照することのできる、IBM 提供のパブリック・イメージが安全に保管されます。 地域レジストリーには、独自のプライベート Docker イメージが安全に保管されます。 トークンは、{{site.data.keyword.registryshort_notm}} 内にセットアップした名前空間への読み取り専用アクセスを許可するために使用されるもので、これにより、パブリック (グローバル・レジストリー) およびプライベート (地域レジストリー) のイメージの処理が可能になります。

コンテナー化アプリのデプロイ時に Kubernetes クラスターがトークンにアクセスできるように、各トークンは Kubernetes の `imagePullSecret` 内に保管されている必要があります。 クラスターが作成されると、{{site.data.keyword.containerlong_notm}} により、グローバル・レジストリー (IBM 提供のパブリック・イメージ) および地域レジストリーのトークンが Kubernetes イメージ・プル・シークレット内に自動的に保管されます。 そのイメージ・プル・シークレットは、Kubernetes の `default` 名前空間と `kube-system` 名前空間に追加され、それらの名前空間の `default` サービス・アカウントのシークレットのリストに追加されます。

トークンを使用して {{site.data.keyword.registrylong_notm}} へのアクセス権限をクラスターに与えるこの方式は、`registry.bluemix.net` ドメイン名ではサポートされていますが、非推奨になっています。 新しい `icr.io` レジストリー・ドメイン名へのアクセス権限をクラスターに与える場合は、代わりに [API キーの方式を使用](#cluster_registry_auth)してください。
{: deprecated}

コンテナーをデプロイする手順は、イメージの場所やコンテナーの場所によって次のように異なります。
*   [クラスターと同じ地域にあるイメージを使用して Kubernetes 名前空間 `default` にコンテナーをデプロイする手順](#token_default_namespace)
*   [`default` 以外の Kubernetes 名前空間にコンテナーをデプロイする手順](#token_copy_imagePullSecret)
*   [クラスターとは別の地域や {{site.data.keyword.Bluemix_notm}} アカウントにあるイメージを使用してコンテナーをデプロイする手順](#token_other_regions_accounts)
*   [IBM 以外のプライベート・レジストリーにあるイメージを使用してコンテナーをデプロイする手順](#private_images)

この初期セットアップを使用すると、{{site.data.keyword.Bluemix_notm}} アカウントの名前空間にある任意のイメージのコンテナーを、クラスターの **default** 名前空間にデプロイできます。 クラスターのその他の名前空間にコンテナーをデプロイする場合や、別の {{site.data.keyword.Bluemix_notm}} 地域や別の {{site.data.keyword.Bluemix_notm}} アカウントに保管されているイメージを使用する場合は、[クラスターで使用する独自のイメージ・プル・シークレットを作成](#other)する必要があります。
{: note}

### 非推奨: レジストリー・トークンを使用して Kubernetes 名前空間 `default` にイメージをデプロイする方法
{: #token_default_namespace}

イメージ・プル・シークレットに保管されたレジストリー・トークンを使用して、地域の {{site.data.keyword.registrylong_notm}} にあるイメージからクラスターの **default** 名前空間にコンテナーをデプロイできます。
{: shortdesc}

開始前に、以下のことを行います。
1. [{{site.data.keyword.registryshort_notm}} に名前空間をセットアップして、その名前空間にイメージをプッシュします](/docs/services/Registry?topic=registry-getting-started#gs_registry_namespace_add)。
2. [クラスターを作成します](/docs/containers?topic=containers-clusters#clusters_ui)。
3. [CLI のターゲットを自分のクラスターに設定します](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)。

コンテナーをクラスターの **default** 名前空間にデプロイするために、構成ファイルを作成します。

1.  `mydeployment.yaml` という名前のデプロイメント構成ファイルを作成します。
2.  デプロイメントと、{{site.data.keyword.registryshort_notm}}内の名前空間にある、使用するイメージを定義します。

    {{site.data.keyword.registryshort_notm}}内の名前空間にあるプライベート・イメージを使用するには、次のようにします。

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

    **ヒント:** 名前空間の情報を取得するには、`ibmcloud cr namespace-list` を実行します。

3.  クラスター内にデプロイメントを作成します。

    ```
    kubectl apply -f mydeployment.yaml
    ```
    {: pre}

    **ヒント:** IBM 提供のパブリック・イメージなど、既存の構成ファイルをデプロイすることもできます。 この例では、米国南部地域の **ibmliberty** イメージを使用しています。

    ```
    kubectl apply -f https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/deploy-apps-clusters/deploy-ibmliberty.yaml
    ```
    {: pre}

### 非推奨: クラスターの default 名前空間からその他の名前空間にトークン・ベースのイメージ・プル・シークレットをコピーする方法
{: #token_copy_imagePullSecret}

Kubernetes 名前空間 `default` のために自動的に生成されたレジストリー・トークンの資格情報が含まれているイメージ・プル・シークレットをクラスターの他の名前空間にコピーできます。
{: shortdesc}

1. クラスターで使用可能な名前空間をリストします。
   ```
   kubectl get namespaces
   ```
   {: pre}

   出力例:
   ```
   default          Active    79d
   ibm-cert-store   Active    79d
   ibm-system       Active    79d
   istio-system     Active    34d
   kube-public      Active    79d
   kube-system      Active    79d
   ```
   {: screen}

2. オプション: クラスターに名前空間を作成します。
   ```
   kubectl create namespace <namespace_name>
   ```
   {: pre}

3. `default` の名前空間から任意の名前空間にイメージ・プル・シークレットをコピーします。 新しいイメージ・プル・シークレットの名前は、`bluemix-<namespace_name>-secret-regional` と `bluemix-<namespace_name>-secret-international` になります。
   ```
   kubectl get secret bluemix-default-secret-regional -o yaml | sed 's/default/<namespace_name>/g' | kubectl -n <namespace_name> create -f -
   ```
   {: pre}

   ```
   kubectl get secret bluemix-default-secret-international -o yaml | sed 's/default/<namespace_name>/g' | kubectl -n <namespace_name> create -f -
   ```
   {: pre}

4.  シークレットが正常に作成されたことを確認します。
    ```
    kubectl get secrets --namespace <namespace_name>
    ```
    {: pre}

5. その [`imagePullSecret` を名前空間で使用してコンテナーをデプロイします](#use_imagePullSecret)。


### 非推奨: 他の {{site.data.keyword.Bluemix_notm}} 地域やアカウントのイメージにアクセスするためにトークン・ベースのイメージ・プル・シークレットを作成する方法
{: #token_other_regions_accounts}

他の {{site.data.keyword.Bluemix_notm}} 地域またはアカウントのイメージにアクセスするには、レジストリー・トークンを作成し、資格情報をイメージ・プル・シークレットに保存する必要があります。
{: shortdesc}

1.  トークンがない場合は、[アクセスするレジストリーのトークンを作成します。](/docs/services/Registry?topic=registry-registry_access#registry_tokens_create)
2.  {{site.data.keyword.Bluemix_notm}} アカウント内のトークンのリストを表示します。

    ```
    ibmcloud cr token-list
    ```
    {: pre}

3.  使用するトークン ID をメモします。
4.  トークンの値を取得します。 <em>&lt;token_ID&gt;</em> を、前のステップで取得したトークンの ID に置き換えます。

    ```
    ibmcloud cr token-get <token_id>
    ```
    {: pre}

    CLI 出力の **Token** フィールドに、トークン値が表示されます。

5.  トークン情報を保管する Kubernetes シークレットを作成します。

    ```
    kubectl --namespace <kubernetes_namespace> create secret docker-registry <secret_name>  --docker-server=<registry_URL> --docker-username=token --docker-password=<token_value> --docker-email=<docker_email>
    ```
    {: pre}

    <table>
    <caption>このコマンドの構成要素について</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> このコマンドの構成要素について</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace <em>&lt;kubernetes_namespace&gt;</em></code></td>
    <td>必須。 シークレットを使用してコンテナーをデプロイする、クラスターの Kubernetes 名前空間。 クラスター内の名前空間をすべてリストするには、<code>kubectl get namespaces</code> を実行します。</td>
    </tr>
    <tr>
    <td><code><em>&lt;secret_name&gt;</em></code></td>
    <td>必須。 イメージ・プル・シークレットに使用する名前。</td>
    </tr>
    <tr>
    <td><code>--docker-server <em>&lt;registry_URL&gt;</em></code></td>
    <td>必須。 名前空間をセットアップするイメージ・レジストリーの URL。<ul><li>米国南部と米国東部でセットアップした名前空間の場合: <code>registry.ng.bluemix.net</code></li><li>英国南部でセットアップした名前空間の場合: <code>registry.eu-gb.bluemix.net</code></li><li>中欧 (フランクフルト) でセットアップした名前空間の場合: <code>registry.eu-de.bluemix.net</code></li><li>オーストラリア (シドニー) でセットアップした名前空間の場合: <code>registry.au-syd.bluemix.net</code></li></ul></td>
    </tr>
    <tr>
    <td><code>--docker-username <em>&lt;docker_username&gt;</em></code></td>
    <td>必須。 プライベート・レジストリーにログインするためのユーザー名。 {{site.data.keyword.registryshort_notm}} の場合、ユーザー名は値 <strong><code>token</code></strong> に設定されます。</td>
    </tr>
    <tr>
    <td><code>--docker-password <em>&lt;token_value&gt;</em></code></td>
    <td>必須。 以前に取得したレジストリー・トークンの値。</td>
    </tr>
    <tr>
    <td><code>--docker-email <em>&lt;docker-email&gt;</em></code></td>
    <td>必須。 Docker E メール・アドレスがある場合は、その値を入力します。 ない場合は、a@b.c のような架空の E メール・アドレスを入力します。 この E メールは、Kubernetes シークレットを作成する際には必須ですが、作成後は使用されません。</td>
    </tr>
    </tbody></table>

6.  シークレットが正常に作成されたことを確認します。 <em>&lt;kubernetes_namespace&gt;</em> を、イメージ・プル・シークレットを作成した名前空間に置き換えます。

    ```
    kubectl get secrets --namespace <kubernetes_namespace>
    ```
    {: pre}

7.  その[イメージ・プル・シークレットを名前空間で使用してコンテナーをデプロイします](#use_imagePullSecret)。
