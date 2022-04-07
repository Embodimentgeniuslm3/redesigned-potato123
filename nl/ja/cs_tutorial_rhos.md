---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: kubernetes, iks, oks, iro, openshift, red hat, red hat openshift, rhos

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

# チュートリアル: Red Hat OpenShift on IBM Cloud クラスター (ベータ版) の作成
{: #openshift_tutorial}

Red Hat OpenShift on IBM Cloud は、OpenShift クラスターをテストするためのベータ版として使用できます。ベータ版では、{{site.data.keyword.containerlong}} のすべての機能を使用できるわけではありません。また、作成した OpenShift ベータ版クラスターは、ベータ版が終了して Red Hat OpenShift on IBM Cloud が一般出荷可能になった後は、30 日間のみ存続します。
{: preview}

**Red Hat OpenShift on IBM Cloud のベータ版**を使用すると、OpenShift コンテナー・オーケストレーション・プラットフォーム・ソフトウェアとともにインストールされるワーカー・ノードを持つ {{site.data.keyword.containerlong_notm}} クラスターを作成できます。アプリのデプロイメント用に Red Hat Enterprise Linux 上で実行される [OpenShift ツールおよびカタログ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.openshift.com/container-platform/3.11/welcome/index.html) を使用しているときは、クラスター・インフラストラクチャー環境の[管理対象の {{site.data.keyword.containerlong_notm}} のすべての利点](/docs/containers?topic=containers-responsibilities_iks)を享受できます。
{: shortdesc}

OpenShift ワーカー・ノードは、標準クラスターでのみ使用可能です。Red Hat OpenShift on IBM Cloud は、OpenShift バージョン 3.11 のみをサポートします。これには Kubernetes バージョン 1.11 が含まれます。
{: note}

## 達成目標
{: #openshift_objectives}

チュートリアルのレッスンでは、標準の Red Hat OpenShift on IBM Cloud クラスターを作成し、OpenShift コンソールを開き、組み込みの OpenShift コンポーネントにアクセスし、OpenShift プロジェクトに {{site.data.keyword.Bluemix_notm}} サービスを使用するアプリをデプロイし、外部ユーザーがサービスにアクセスできるように OpenShift 経路でアプリを公開します。
{: shortdesc}

このページには、OpenShift クラスター・アーキテクチャー、ベータ版の制限、およびフィードバックの送信方法とサポートの利用方法に関する情報も含まれます。

## 所要時間
{: #openshift_time}
45 分

## 対象読者
{: #openshift_audience}

このチュートリアルは、初めて Red Hat OpenShift on IBM Cloud クラスターを作成する方法を学習するクラスター管理者を対象としています。
{: shortdesc}

## 前提条件
{: #openshift_prereqs}

*   以下の {{site.data.keyword.Bluemix_notm}} IAM アクセス・ポリシーを用意しておきます。
    *   {{site.data.keyword.containerlong_notm}} に対する[**管理者**のプラットフォーム役割](/docs/containers?topic=containers-users#platform)
    *   {{site.data.keyword.containerlong_notm}} に対する[**ライター**または**管理者**のサービス役割](/docs/containers?topic=containers-users#platform)
    *   {{site.data.keyword.registrylong_notm}} に対する[**管理者**のプラットフォーム役割](/docs/containers?topic=containers-users#platform)
*    {{site.data.keyword.Bluemix_notm}} の地域およびリソース・グループの [API キー](/docs/containers?topic=containers-users#api_key)が、クラスターを作成するための正しいインフラストラクチャー許可、**スーパーユーザー**、または[最小限の役割](/docs/containers?topic=containers-access_reference#infra)を使用してセットアップされていることを確認します。
*   コマンド・ライン・ツールをインストールします。
    *   [{{site.data.keyword.Bluemix_notm}} CLI (`ibmcloud`)、{{site.data.keyword.containershort_notm}} プラグイン (`ibmcloud ks`)、および {{site.data.keyword.registryshort_notm}} プラグイン (`ibmcloud cr`) をインストールします](/docs/containers?topic=containers-cs_cli_install#cs_cli_install_steps)。
    *   [OpenShift Origin (`oc`) CLI および Kubernetes (`kubectl`) CLI をインストールします](/docs/containers?topic=containers-cs_cli_install#cli_oc)。

<br />


## アーキテクチャーの概要
{: #openshift_architecture}

以下の図および表は、Red Hat OpenShift on IBM Cloud アーキテクチャーでセットアップされるデフォルト・コンポーネントを示しています。
{: shortdesc}

![Red Hat OpenShift on IBM Cloud クラスター・アーキテクチャー](images/cs_org_ov_both_ses_rhos.png)

| マスターのコンポーネント| 説明 |
|:-----------------|:-----------------|
| レプリカ | OpenShift Kubernetes API サーバーと etcd データ・ストアを含むマスターのコンポーネントは 3 つのレプリカを持ち、複数ゾーンの大都市に配置された場合は複数のゾーンに分散されて、さらに高い可用性を実現します。マスターのコンポーネントは 8 時間ごとにバックアップされます。|
| `rhos-api` | OpenShift Kubernetes API サーバーは、ワーカー・ノードからマスターへのすべてのクラスター管理要求のメインエントリー・ポイントとして機能します。API サーバーは、ポッドやサービスなどの Kubernetes リソースの状態を変更する要求を検証および処理してから、この状態を etcd データ・ストアに保管します。|
| `openvpn-server` | OpenVPN サーバーは、マスターをワーカー・ノードに安全に接続するために OpenVPN クライアントと連携します。 この接続は、ポッドやサービスに対する `apiserver proxy` 呼び出しと、kubelet に対する `kubectl exec`、`attach`、`logs` 呼び出しをサポートしています。|
| `etcd` | etcd は、クラスターのすべての Kubernetes リソース (サービス、デプロイメント、ポッドなど) の状態を保管する、可用性の高いキー値ストアです。 etcd 内のデータは、IBM が管理する暗号化ストレージ・インスタンスにバックアップされます。|
| `rhos-controller` | OpenShift コントローラー・マネージャーは、新しく作成されたポッドを監視し、容量、パフォーマンスのニーズ、ポリシー制約、アンチアフィニティー仕様、およびワークロード要件に基づいて、それらのポッドをデプロイする場所を決定します。要件に合致するワーカー・ノードが見つからなければ、ポッドはクラスターにデプロイされません。 コントローラーでは、レプリカ・セットなどのクラスター・リソースの状態も監視されます。リソースの状態が変化する (例えば、レプリカ・セット内のポッドがダウンする) と、コントローラー・マネージャーは、必要な状態を実現するための修正アクションを開始します。 `rhos-controller` は、ネイティブの Kubernetes 構成でスケジューラーとコントローラー・マネージャーの両方として機能します。|
| `cloud-controller-manager` | クラウド・コントローラー・マネージャーは、{{site.data.keyword.Bluemix_notm}} ロード・バランサーなどのクラウド・プロバイダー固有コンポーネントを管理します。|
{: caption="表 1. OpenShift マスターのコンポーネント" caption-side="top"}
{: #rhos-components-1}
{: tab-title="Master"}
{: tab-group="rhos-components"}
{: class="simple-tab-table"}

| ワーカー・ノードのコンポーネント| 説明 |
|:-----------------|:-----------------|
| オペレーティング・システム | Red Hat OpenShift on IBM Cloud ワーカー・ノードは、Red Hat Enterprise Linux 7 (RHEL 7) オペレーティング・システム上で稼働します。|
| プロジェクト | OpenShift は、リソースをプロジェクト (アノテーション付きの Kubernetes 名前空間) に編成し、カタログなどの OpenShift 機能を実行するためにネイティブの Kubernetes クラスターよりも多くのコンポーネントを含みます。以下の行で、プロジェクトの厳選されたコンポーネントについて説明します。詳しくは、[Projects and users ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.openshift.com/container-platform/3.11/architecture/core_concepts/projects_and_users.html) を参照してください。|
| `kube-system` | この名前空間には、ワーカー・ノードで Kubernetes を実行するために使用される多くのコンポーネントが含まれています。<ul><li>**`ibm-master-proxy`**: `ibm-master-proxy` は、ワーカー・ノードからの要求を、可用性の高い複数のマスター・レプリカの各 IP アドレスに転送するデーモン・セットです。単一ゾーン・クラスターのマスターの場合は、別々のホスト上に 3 つのレプリカが存在します。複数ゾーン対応ゾーンにあるクラスターの場合、マスターの 3 つのレプリカがゾーン間に分散されます。 高可用性ロード・バランサーは、マスター・ドメイン・ネームへの要求をマスター・レプリカに転送します。</li><li>**`openvpn-client`**: OpenVPN クライアントは、マスターをワーカー・ノードに安全に接続するために OpenVPN サーバーと連携します。この接続は、ポッドやサービスに対する `apiserver proxy` 呼び出しと、kubelet に対する `kubectl exec`、`attach`、`logs` 呼び出しをサポートしています。</li><li>**`kubelet`**: kubelet はワーカー・ノードごとに実行されるワーカー・ノード・エージェントであり、ワーカー・ノードで実行される各ポッドの正常性のモニタリングと、Kubernetes API サーバーから送信されるイベントの監視を行います。イベントに基づいて、kubelet は、ポッドを作成または削除し、Liveness Probe と Readiness Probe を確保し、Kubernetes API サーバーに応答としてポッドの状況を報告します。</li><li>**`calico`**: Calico は、クラスターのネットワーク・ポリシーを管理し、コンテナー・ネットワーク接続、IP アドレス割り当て、およびネットワーク・トラフィック制御を管理するいくつかのコンポーネントを含みます。</li><li>**その他のコンポーネント**: `kube-system` 名前空間には、IBM 提供のリソース (ファイルおよびブロック・ストレージ用のストレージ・プラグイン、Ingress アプリケーション・ロード・バランサー (ALB)、`fluentd` ロギング、および `keepalived` など) を管理するコンポーネントも含まれています。</li></ul>|
| `ibm-system` | この名前空間には、`ibm-cloud-provider-ip` デプロイメントが含まれています。このデプロイメントは、`keepalived` と連携して、アプリ・ポッドへの要求のヘルス・チェックおよびレイヤー 4 ロード・バランシングを提供します。|
| `kube-proxy-and-dns`| この名前空間には、ワーカー・ノードでセットアップされた `iptables` ルールに照らして着信ネットワーク・トラフィックを検証し、クラスターへの出入りを許可された要求をプロキシー処理するためのコンポーネントが含まれています。|
| `default` | この名前空間は、名前空間を指定しない場合、または Kubernetes リソース用のプロジェクトを作成しない場合に使用されます。さらに、デフォルトの名前空間には、OpenShift クラスターをサポートする以下のコンポーネントが含まれています。<ul><li>**`router`**: OpenShift では[経路 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.openshift.com/container-platform/3.11/architecture/networking/routes.html) を使用して、外部クライアントがサービスにアクセスできるようにホスト名でアプリのサービスを公開します。router によって、サービスがホスト名にマップされます。</li><li>**`docker-registry`** および **`registry-console`**: OpenShift では、コンソールを使用してイメージをローカルで管理および表示するために使用できる内部[コンテナー・イメージ・レジストリー ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.openshift.com/container-platform/3.11/install_config/registry/index.html) が提供されます。プライベート {{site.data.keyword.registrylong_notm}} をセットアップすることもできます。</li></ul>|
| その他のプロジェクト | その他のコンポーネントは、ロギング、モニタリング、OpenShift コンソールなどの機能を有効にするために、デフォルトでさまざまな名前空間にインストールされます。<ul><li><code>ibm-cert-store</code></li><li><code>kube-public</code></li><li><code>kube-service-catalog</code></li><li><code>openshift</code></li><li><code>openshift-ansible-service-broker</code></li><li><code>openshift-console</code></li><li><code>openshift-infra</code></li><li><code>openshift-monitoring</code></li><li><code>openshift-node</code></li><li><code>openshift-template-service-broker</code></li><li><code>openshift-web-console</code></li></ul>|
{: caption="表 2. OpenShift ワーカー・ノードのコンポーネント。" caption-side="top"}
{: #rhos-components-2}
{: tab-title="Worker nodes"}
{: tab-group="rhos-components"}
{: class="simple-tab-table"}

<br />


## レッスン 1: Red Hat OpenShift on IBM Cloud クラスターを作成する
{: #openshift_create_cluster}

[コンソール](#openshift_create_cluster_console)または [CLI](#openshift_create_cluster_cli) を使用して、{{site.data.keyword.containerlong_notm}} で Red Hat OpenShift on IBM Cloud クラスターを作成できます。クラスターの作成時にセットアップされるコンポーネントについては、[アーキテクチャーの概要](#openshift_architecture)を参照してください。OpenShift は、標準クラスターでのみ使用可能です。標準クラスターの価格について詳しくは、[よくある質問](/docs/containers?topic=containers-faqs#charges)を参照してください。
{:shortdesc}

クラスターは、**デフォルト**のリソース・グループでのみ作成できます。ベータ版で作成した OpenShift クラスターは、ベータ版が終了して Red Hat OpenShift on IBM Cloud が一般出荷可能になった後は、30 日間存続します。
{: important}

### コンソールを使用したクラスターの作成
{: #openshift_create_cluster_console}

{{site.data.keyword.containerlong_notm}} コンソールで標準の OpenShift クラスターを作成します。
{: shortdesc}

始める前に、[前提条件を完了し](#openshift_prereqs)、クラスターを作成するための適切な権限があることを確認します。

1.  クラスターを作成します。
    1.  [{{site.data.keyword.Bluemix_notm}} アカウント ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/) にログインします。
    2.  ハンバーガー・メニュー ![ハンバーガー・メニュー・アイコン](../icons/icon_hamburger.svg "ハンバーガー・メニュー・アイコン") から、**「Kubernetes」**を選択して**「クラスターの作成」**をクリックします。
    3.  クラスターのセットアップの詳細と名前を選択します。ベータ版の OpenShift クラスターは、ワシントン DC およびロンドンのデータ・センターにある標準クラスターとしてのみ使用可能です。
        *   **「プランの選択 (Select a plan)」**で、**「標準」**を選択します。
        *   **「リソース・グループ」**では**デフォルト**を使用する必要があります。
        *   **「場所」**で、地域を**「北アメリカ」**または**「ヨーロッパ」**に設定して、**「単一ゾーン (Single zone)」**または**「複数ゾーン (Multizone)」**の可用性を選択して、**「ワシントン DC (Washington, DC)」**または**「ロンドン」**のワーカー・ゾーンを選択します。
        *   **「デフォルトのワーカー・プール (Default worker pool)」**で **OpenShift** クラスターのバージョンを選択します。Red Hat OpenShift on IBM Cloud は、OpenShift バージョン 3.11 のみをサポートします。これには Kubernetes バージョン 1.11 が含まれます。理想的には、ワーカー・ノードには、4 コア 16 GB RAM 以上の使用可能なフレーバーを選択してください。
        *   ゾーンごとに作成するワーカー・ノードの数 (3 など) を設定します。
    4.  完了するには、**「クラスターの作成」**をクリックします。<p class="note">クラスターの作成が完了するまで、しばらく時間がかかる可能性があります。クラスターの状態で **Normal** が示されてから、クラスター・ネットワークおよびロード・バランシング・コンポーネントで、OpenShift Web コンソールおよびその他の経路に使用するクラスター・ドメインをデプロイおよび更新するのに約 10 分かかります。クラスターの準備が整うまで待機してから、**Ingress サブドメイン**が `<cluster_name>.<region>.containers.appdomain.cloud` のパターンに従っていることを確認して、次のステップに進みます。</p>
2.  クラスターの詳細ページで、**「OpenShift Web コンソール (OpenShift web console)」**をクリックします。
3.  OpenShift コンテナー・プラットフォームのメニュー・バーのドロップダウン・メニューから、**「アプリケーション・コンソール (Application Console)」**をクリックします。アプリケーション・コンソールに、クラスター内のすべてのプロジェクト名前空間がリストされます。名前空間にナビゲートして、アプリケーション、ビルド、およびその他の Kubernetes リソースを表示できます。
4.  端末で操作して次のレッスンを実行するには、自分のプロファイルの**「IAM#user.name@email.com」>「ログイン・コマンドのコピー (Copy Login Command)」**をクリックします。CLI を使用して、コピーした `oc` ログイン・コマンドを端末に貼り付けて認証します。

### CLI を使用したクラスターの作成
{: #openshift_create_cluster_cli}

{{site.data.keyword.Bluemix_notm}} CLI を使用して標準の OpenShift クラスターを作成します。
{: shortdesc}

始める前に、[前提条件を完了し](#openshift_prereqs)、クラスターを作成するための適切な権限があることと、`ibmcloud` CLI とプラグイン、および `oc` CLI と `kubectl` CLI があることを確認します。

1.  OpenShift クラスターを作成するためにセットアップしたアカウントにログインします。**us-east** または **eu-gb** の地域と **default** のリソース・グループをターゲットにします。統合されたアカウントがある場合は、`--sso` フラグを含めます。
```
    ibmcloud login -r (us-east|eu-gb) -g default [--sso]
    ```
    {: pre}
2.  クラスターを作成します。
    ```
    ibmcloud ks cluster-create --name <name> --location <zone> --kube-version <openshift_version> --machine-type <worker_node_flavor> --workers <number_of_worker_nodes_per_zone> --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID>
    ```
    {: pre}

    ワシントン DC に、4 つのコアと 16 GB のメモリーがある 3 つのワーカー・ノードを持つクラスターを作成するコマンドの例。

    ```
    ibmcloud ks cluster-create --name my_openshift --location wdc04 --kube-version 3.11_openshift --machine-type b3c.4x16.encrypted  --workers 3 --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID>
    ```
    {: pre}

    <table summary="表の 1 行目は両方の列にまたがっています。残りの行は左から右に読みます。1 列目はコマンドの構成要素、2 列目は対応する説明です。">
    <caption>cluster-create コンポーネント</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> このコマンドの構成要素について</th>
    </thead>
    <tbody>
    <tr>
    <td><code>cluster-create</code></td>
    <td>{{site.data.keyword.Bluemix_notm}} アカウント内にクラシック・インフラストラクチャー・クラスターを作成するためのコマンド。</td>
    </tr>
    <tr>
    <td><code>--name <em>&lt;name&gt;</em></code></td>
    <td>クラスターの名前を入力します。 名前は先頭が文字でなければならず、文字、数字、およびハイフン (-) を使用できます。35 文字以内でなければなりません。 {{site.data.keyword.Bluemix_notm}} 地域全体で固有の名前を使用してください。</td>
    </tr>
    <tr>
    <td><code>--location <em>&lt;zone&gt;</em></code></td>
    <td>クラスターを作成するゾーンを指定します。ベータ版では、使用可能なゾーンは `wdc04、wdc06、wdc07、lon04、lon05、`または `lon06` です。</p></td>
    </tr>
    <tr>
      <td><code>--kube-version <em>&lt;openshift_version&gt;</em></code></td>
      <td>サポートされる OpenShift バージョンを選択する必要があります。OpenShift バージョンには、ネイティブの Kubernetes Ubuntu クラスターで使用可能な Kubernetes バージョンとは異なる Kubernetes バージョンが含まれています。使用可能な OpenShift バージョンをリストするには、`ibmcloud ks versions` を実行します。最新バージョンのパッチでクラスターを作成するには、`3.11_openshift` など、メジャー・バージョンとマイナー・バージョンのみを指定できます。<br><br>Red Hat OpenShift on IBM Cloud は、OpenShift バージョン 3.11 のみをサポートします。これには Kubernetes バージョン 1.11 が含まれます。</td>
    </tr>
    <tr>
    <td><code>--machine-type <em>&lt;worker_node_flavor&gt;</em></code></td>
    <td>マシン・タイプを選択します。 ワーカー・ノードは、共有または専用ハードウェア上に仮想マシンとしてデプロイすることも、ベア・メタル上に物理マシンとしてデプロイすることもできます。 使用可能な物理マシンと仮想マシンのタイプは、クラスターをデプロイするゾーンによって異なります。 使用可能なマシン・タイプをリストするには、`ibmcloud ks machine-types --zone <zone>` を実行します。</td>
    </tr>
    <tr>
    <td><code>--workers <em>&lt;number_of_worker_nodes_per_zone&gt;</em></code></td>
    <td>クラスターに含めるワーカー・ノードの数。クラスターに、デフォルトのコンポーネントを実行して高可用性を実現するための十分なリソースがあるように、少なくとも 3 つのワーカー・ノードを指定することをお勧めします。<code>--workers</code> オプションが指定されていない場合は、ワーカー・ノードが 1 つ作成されます。</td>
    </tr>
    <tr>
    <td><code>--public-vlan <em>&lt;public_vlan_id&gt;</em></code></td>
    <td>IBM Cloud インフラストラクチャー (SoftLayer) アカウントでそのゾーン用にパブリック VLAN を既にセットアップしている場合には、そのパブリック VLAN の ID を入力します。 使用可能な VLAN を確認するには、`ibmcloud ks vlans --zone <zone>` を実行します。<br><br>ご使用のアカウントでパブリック VLAN がない場合は、このオプションを指定しないでください。{{site.data.keyword.containerlong_notm}} が自動的にパブリック VLAN を作成します。</td>
    </tr>
    <tr>
    <td><code>--private-vlan <em>&lt;private_vlan_id&gt;</em></code></td>
    <td>IBM Cloud インフラストラクチャー (SoftLayer) アカウントでそのゾーン用にプライベート VLAN を既にセットアップしている場合には、そのプライベート VLAN の ID を入力します。 使用可能な VLAN を確認するには、`ibmcloud ks vlans --zone <zone>` を実行します。<br><br>ご使用のアカウントでプライベート VLAN がない場合は、このオプションを指定しないでください。 {{site.data.keyword.containerlong_notm}} が自動的にプライベート VLAN を作成します。</td>
    </tr>
    </tbody></table>
3.  クラスターの詳細をリストします。クラスターの**状態**を確認し、**Ingress サブドメイン**を確認し、**マスター URL** をメモします。<p class="note">クラスターの作成が完了するまで、しばらく時間がかかる可能性があります。クラスターの状態で **Normal** が示されてから、クラスター・ネットワークおよびロード・バランシング・コンポーネントで、OpenShift Web コンソールおよびその他の経路に使用するクラスター・ドメインをデプロイおよび更新するのに約 10 分かかります。クラスターの準備が整うまで待機してから、**Ingress サブドメイン**が `<cluster_name>.<region>.containers.appdomain.cloud` のパターンに従っていることを確認して、次のステップに進みます。</p>
    ```
    ibmcloud ks cluster-get --cluster <cluster_name_or_ID>
    ```
    {: pre}
4.  クラスターに接続するための構成ファイルをダウンロードします。
    ```
    ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
    ```
    {: pre}

    構成ファイルのダウンロードが完了すると、そのローカルの Kubernetes 構成ファイルのパスを環境変数として設定するためにコピーして貼り付けることができるコマンドが表示されます。

    OS X の場合の例:

    ```
    export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-<datacenter>-<cluster_name>.yml
    ```
    {: screen}
5.  ブラウザーで、**マスター URL** のアドレスに `/console` を付加してナビゲートします。例えば、`https://c0.containers.cloud.ibm.com:23652/console` などです。
6.  自分のプロファイルの**「IAM#user.name@email.com」>「ログイン・コマンドのコピー (Copy Login Command)」**をクリックします。CLI を使用して、コピーした `oc` ログイン・コマンドを端末に貼り付けて認証します。<p class="tip">後で OpenShift コンソールにアクセスするために、クラスターのマスター URL を保存します。今後のセッションでは、`cluster-config` のステップをスキップして、代わりにコンソールからログイン・コマンドをコピーすることができます。</p>
7.  バージョンを調べて、ご使用のクラスターで `oc` コマンドが正常に実行されることを確認します。

    ```
    oc version
    ```
    {: pre}

    出力例:

    ```
    oc v3.11.0+0cbc58b
    kubernetes v1.11.0+d4cacc0
    features: Basic-Auth

    Server https://c0.containers.cloud.ibm.com:23652
    openshift v3.11.98
    kubernetes v1.11.0+d4cacc0
    ```
    {: screen}

    クラスター内のすべてのワーカー・ノードまたはポッドのリストなどの、管理者の許可を必要とする操作を実行できない場合は、`ibmcloud ks cluster-config --cluster <cluster_name_or_ID> --admin` コマンドを実行して、クラスター管理者の TLS 証明書および許可ファイルをダウンロードします。
    {: tip}

<br />


## レッスン 2: 組み込みの OpenShift サービスにアクセスする
{: #openshift_access_oc_services}

Red Hat OpenShift on IBM Cloud には、OpenShift コンソール、Prometheus、Grafana などのクラスターの操作に役立つ組み込みサービスが付属しています。ベータ版では、これらのサービスにアクセスするために、[経路 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.openshift.com/container-platform/3.11/architecture/networking/routes.html) のローカル・ホストを使用できます。デフォルトの経路のドメイン・ネームは、`<service_name>-<namespace>.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud` のクラスター固有のパターンに従います。
{:shortdesc}

組み込み OpenShift サービスの経路には、[コンソール](#openshift_services_console)または [CLI](#openshift_services_cli) からアクセスできます。1 つのプロジェクト内の Kubernetes リソースをナビゲートする場合は、コンソールを使用することをお勧めします。CLI を使用すると、複数のプロジェクトにまたがる経路などのリソースをリストできます。

### コンソールからの組み込み OpenShift サービスへのアクセス
{: #openshift_services_console}
1.  OpenShift Web コンソールの OpenShift コンテナー・プラットフォームのメニュー・バーのドロップダウン・メニューで、**「アプリケーション・コンソール (Application Console)」**をクリックします。
2.  **デフォルト**のプロジェクトを選択して、ナビゲーション・ペインで、**「アプリケーション」>「ポッド (Pods)」**をクリックします。
3.  **router** のポッドが **Running** の状況であることを確認します。router は、外部ネットワーク・トラフィックを受信するポイントとして機能します。router を使用して、経路を使用することにより、router の外部 IP アドレスでクラスター内のサービスをパブリックに公開できます。router は、プライベート IP のみで listen するアプリ・ポッドとは異なり、パブリック・ホスト・ネットワーク・インターフェースで listen します。router により、経路のホスト名の外部要求が、経路のホスト名に関連付けたサービスによって識別されるアプリ・ポッドの IP にプロキシー処理されます。
4.  **デフォルト**のプロジェクトのナビゲーション・ペインで、**「アプリケーション」>「デプロイメント (Deployments)」**をクリックし、**registry-console** デプロイメントをクリックします。内部レジストリー・コンソールを開くには、外部からアクセスできるようにプロバイダー URL を更新する必要があります。
    1.  **registry-console** の詳細ページの**「環境」**タブで、**OPENSHIFT_OAUTH_PROVIDER_URL** フィールドを見つけます。 
    2. 値フィールドで、`c1` の後に `-e` を追加します (`https://c1-e.eu-gb.containers.cloud.ibm.com:20399` など)。 
    3. **「保存」**をクリックします。 これで、クラスター・マスターのパブリック API エンドポイントを介してレジストリー・コンソール・デプロイメントにアクセスできるようになりました。
    4.  **デフォルト**のプロジェクトのナビゲーション・ペインで、**「アプリケーション」>「経路」**をクリックします。レジストリー・コンソールを開くには、**「ホスト名」**の値 (`https://registry-console-default.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud` など) をクリックします。<p class="note">ベータ版の場合、レジストリー・コンソールで自己署名 TLS 証明書が使用されるため、続行してレジストリー・コンソールに移動することを選択する必要があります。Google Chrome で、**「Advanced」>「Proceed to <cluster_master_URL>」**をクリックします。その他のブラウザーに、同様のオプションがあります。この設定に進むことができない場合は、プライベート・ブラウザーで URL を開いてみてください。</p>
5.  OpenShift コンテナー・プラットフォームのメニュー・バーのドロップダウン・メニューで、**「クラスター・コンソール (Cluster Console)」**をクリックします。
6.  ナビゲーション・ペインで**「モニタリング」**を展開します。
7.  アクセスする組み込みのモニタリング・ツール (**「ダッシュボード」**など) をクリックします。Grafana 経路が開きます (`https://grafana-openshift-monitoring.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud`)。<p class="note">初めてホスト名にアクセスするときに、**「OpenShift でログイン (Log in with OpenShift)」**をクリックして IAM Identity へのアクセスを許可するなど、認証が必要になる場合があります。</p>

### CLI からの組み込み OpenShift サービスへのアクセス
{: #openshift_services_cli}

1.  OpenShift Web コンソールから、自分のプロファイルの**「IAM#user.name@email.com」>「ログイン・コマンドのコピー (Copy Login Command)」**をクリックして、ログイン・コマンドを端末に貼り付けて認証します。
    ```
    oc login https://c1-e.<region>.containers.cloud.ibm.com:<port> --token=<access_token>
    ```
    {: pre}
2.  router がデプロイされていることを確認します。router は、外部ネットワーク・トラフィックを受信するポイントとして機能します。router を使用して、経路を使用することにより、router の外部 IP アドレスでクラスター内のサービスをパブリックに公開できます。router は、プライベート IP のみで listen するアプリ・ポッドとは異なり、パブリック・ホスト・ネットワーク・インターフェースで listen します。router により、経路のホスト名の外部要求が、経路のホスト名に関連付けたサービスによって識別されるアプリ・ポッドの IP にプロキシー処理されます。
    ```
    oc get svc router -n default
    ```
    {: pre}

    出力例:
    ```
    NAME      TYPE           CLUSTER-IP               EXTERNAL-IP     PORT(S)                      AGE
    router    LoadBalancer   172.21.xxx.xxx   169.xx.xxx.xxx   80:30399/TCP,443:32651/TCP                      5h
    ```
    {: screen}
2.  アクセスするサービス経路の **Host/Port** のホスト名を取得します。例えば、Grafana ダッシュボードにアクセスして、クラスターのリソース使用量に関するメトリックを確認できます。デフォルトの経路のドメイン・ネームは、`<service_name>-<namespace>.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud` のクラスター固有のパターンに従います。
    ```
    oc get route --all-namespaces
    ```
    {: pre}

    出力例:
    ```
    NAMESPACE                          NAME                HOST/PORT                                                                    PATH                  SERVICES            PORT               TERMINATION          WILDCARD
    default                            registry-console    registry-console-default.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud                              registry-console    registry-console   passthrough          None
    kube-service-catalog               apiserver           apiserver-kube-service-catalog.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud                        apiserver           secure             passthrough          None
    openshift-ansible-service-broker   asb-1338            asb-1338-openshift-ansible-service-broker.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud            asb                 1338               reencrypt            None
    openshift-console                  console             console.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud                                              console             https              reencrypt/Redirect   None
    openshift-monitoring               alertmanager-main   alertmanager-main-openshift-monitoring.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud                alertmanager-main   web                reencrypt            None
    openshift-monitoring               grafana             grafana-openshift-monitoring.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud                          grafana             https              reencrypt            None
    openshift-monitoring               prometheus-k8s      prometheus-k8s-openshift-monitoring.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud                   prometheus-k8s      web                reencrypt
    ```
    {: screen}
3.  **レジストリーのワンタイム更新**: 内部レジストリー・コンソールをインターネットからアクセス可能にするには、クラスター・マスターのパブリック API エンドポイントを OpenShift プロバイダー URL として使用するように `registry-console` デプロイメントを編集します。パブリック API エンドポイントの形式はプライベート API エンドポイントと同じですが、URL に追加の `-e` が含まれています。
    ```
    oc edit deploy registry-console -n default
    ```
    {: pre}
    
    `Pod Template.Containers.registry-console.Environment.OPENSHIFT_OAUTH_PROVIDER_URL` フィールドで、`c1` の後に `-e` を追加します (`https://ce.eu-gb.containers.cloud.ibm.com:20399` など)。
    ```
    Name:                   registry-console
    Namespace:              default
    ...
    Pod Template:
      Labels:  name=registry-console
      Containers:
       registry-console:
        Image:      registry.eu-gb.bluemix.net/armada-master/iksorigin-registrconsole:v3.11.98-6
        ...
        Environment:
          OPENSHIFT_OAUTH_PROVIDER_URL:  https://c1-e.eu-gb.containers.cloud.ibm.com:20399
          ...
    ```
    {: screen}
4.  Web ブラウザーで、アクセスする経路 (例えば、`https://grafana-openshift-monitoring.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud`) を開きます。初めてホスト名にアクセスするときに、**「OpenShift でログイン (Log in with OpenShift)」**をクリックして IAM Identity へのアクセスを許可するなど、認証が必要になる場合があります。

<br>
これで、組み込みの OpenShift アプリが表示されます。例えば、Grafana を使用している場合は、名前空間の CPU 使用量やその他のグラフを確認できます。他の組み込みツールにアクセスするには、その経路のホスト名を開きます。

<br />


## レッスン 3: OpenShift クラスターにアプリをデプロイする
{: #openshift_deploy_app}

Red Hat OpenShift on IBM Cloud を使用すると、新しいアプリを作成したり、外部のユーザーが使用できるように OpenShift ルーターからアプリ・サービスを公開したりすることができます。
{: shortdesc}

前回のレッスンを中断して、新しい端末を開始した場合は、必ずクラスターに再度ログインしてください。OpenShift コンソール (`https://<master_URL>/console`) を開きます。例えば、`https://c0.containers.cloud.ibm.com:23652/console` などです。次に、自分のプロファイルの**「IAM#user.name@email.com」>「ログイン・コマンドのコピー (Copy Login Command)」**をクリックして、CLI を使用して、コピーした `oc` ログイン・コマンドを端末に貼り付けて認証します。
{: tip}

1.  Hello World アプリのプロジェクトを作成します。プロジェクトは、追加のアノテーションを持つ OpenShift バージョンの Kubernetes 名前空間です。
    ```
    oc new-project hello-world
    ```
    {: pre}
2.  [ソース・コードから ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/IBM/container-service-getting-started-wt) サンプル・アプリをビルドします。OpenShift の `new-app` コマンドを使用すると、イメージをビルドするための Dockerfile とアプリのコードを含むリモート・リポジトリー内のディレクトリーを参照できます。このコマンドでは、イメージをビルドして、イメージをローカルの Docker レジストリーに保管し、アプリ・デプロイメントの構成 (`dc`) およびサービス (`svc`) を作成します。新しいアプリの作成について詳しくは、[OpenShift の資料 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン") を参照してください](https://docs.openshift.com/container-platform/3.11/dev_guide/application_lifecycle/new_app.html)。
    ```
    oc new-app --name hello-world https://github.com/IBM/container-service-getting-started-wt --context-dir="Lab 1"
    ```
    {: pre}
3.  サンプルの Hello World アプリのコンポーネントが作成されたことを確認します。
    1.  ブラウザーでレジストリー・コンソールにアクセスして、クラスターの組み込みの Docker レジストリーで **hello-world** イメージを確認します。前のレッスンの説明に従って、レジストリー・コンソールのプロバイダー URL を `-e` で更新したことを確認します。
        ```
        https://registry-console-default.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud/
        ```
        {: codeblock}
    2.  **hello-world** サービスをリストし、サービス名をメモします。ルーターが外部トラフィック要求をアプリに転送できるようにサービスの経路を作成しない限り、アプリはこれらの内部クラスター IP アドレスでトラフィックを listen します。
        ```
        oc get svc -n hello-world
        ```
        {: pre}

        出力例:
        ```
        NAME          TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)    AGE
        hello-world   ClusterIP   172.21.xxx.xxx   <nones>       8080/TCP   31m
        ```
        {: screen}
    3.  ポッドをリストします。名前に `build` が含まれているポッドは、新規アプリのビルド・プロセスの一部として**完了した**ジョブです。**hello-world** ポッドの状況が **Running** であることを確認します。
        ```
        oc get pods -n hello-world
        ```
        {: pre}

        出力例:
        ```
        NAME                READY     STATUS             RESTARTS   AGE
        hello-world-9cv7d   1/1       Running            0          30m
        hello-world-build   0/1       Completed          0          31m
        ```
        {: screen}
4.  {{site.data.keyword.toneanalyzershort}} サービスにパブリックにアクセスできるように経路をセットアップします。デフォルトでは、ホスト名の形式は `<service_name>-<namespace>.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud` です。ホスト名をカスタマイズする場合は、`--hostname=<hostname>` フラグを含めます。
    1.  **hello-world** サービスの経路を作成します。
        ```
        oc create route edge --service=hello-world -n hello-world
        ```
        {: pre}
    2.  **Host/Port** の出力から、経路のホスト名アドレスを取得します。
        ```
        oc get route -n hello-world
        ```
        {: pre}
        出力例:
        ```
        NAME          HOST/PORT                         PATH                                        SERVICES      PORT       TERMINATION   WILDCARD
        hello-world   hello-world-hello.world.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud    hello-world   8080-tcp   edge/Allow    None
        ```
        {: screen}
5.  アプリにアクセスします。
    ```
    curl https://hello-world-hello-world.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud
    ```
    {: pre}
    
    出力例:
    ```
    Hello world from hello-world-9cv7d! Your app is up and running in a cluster!
    ```
    {: screen}
6.  **オプション** このレッスンで作成したリソースをクリーンアップする場合は、各アプリに割り当てられたラベルを使用できます。
    1.  `hello-world` プロジェクト内の各アプリのすべてのリソースをリストします。
        ```
        oc get all -l app=hello-world -o name -n hello-world
        ```
        {: pre}
        出力例:
        ```
        pod/hello-world-1-dh2ff
        replicationcontroller/hello-world-1
        service/hello-world
        deploymentconfig.apps.openshift.io/hello-world
        buildconfig.build.openshift.io/hello-world
        build.build.openshift.io/hello-world-1
        imagestream.image.openshift.io/hello-world
        imagestream.image.openshift.io/node
        route.route.openshift.io/hello-world
        ```
        {: screen}
    2.  作成したすべてのリソースを削除します。
        ```
        oc delete all -l app=hello-world -n hello-world
        ```
        {: pre}



<br />


## レッスン 4: クラスターの正常性をモニターするために LogDNA および Sysdig アドオンをセットアップする
{: #openshift_logdna_sysdig}

OpenShift はデフォルトでネイティブの Kubernetes より制限された [Security Context Constraints (SCC) ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.openshift.com/container-platform/3.11/admin_guide/manage_scc.html) をセットアップするため、ネイティブの Kubernetes で使用する一部のアプリまたはクラスター・アドオンを同じ方法で OpenShift にデプロイすることはできません。特に、多くのイメージは、`root` ユーザーまたは特権コンテナーとして実行する必要があり、デフォルトでは OpenShift で使用できません。このレッスンでは、特権セキュリティー・アカウントを作成し、{{site.data.keyword.la_full_notm}} と {{site.data.keyword.mon_full_notm}} の 2 つの一般的な {{site.data.keyword.containerlong_notm}} アドオンを使用するようにポッド仕様で `securityContext` を更新して、デフォルトの SCC を変更する方法を学習します。
{: shortdesc}

始める前に、管理者としてクラスターにログインします。
1.  OpenShift コンソール (`https://<master_URL>/console`) を開きます。例えば、`https://c0.containers.cloud.ibm.com:23652/console` などです。
2.  自分のプロファイルの**「IAM#user.name@email.com」>「ログイン・コマンドのコピー (Copy Login Command)」**をクリックして、CLI を使用して、コピーした `oc` ログイン・コマンドを端末に貼り付けて認証します。
3.  ご使用のクラスター用の管理構成ファイルをダウンロードします。
    ```
    ibmcloud ks cluster-config --cluster <cluster_name_or_ID> --admin
    ```
    {: pre}
    
    構成ファイルのダウンロードが完了すると、そのローカルの Kubernetes 構成ファイルのパスを環境変数として設定するためにコピーして貼り付けることができるコマンドが表示されます。

    OS X の場合の例:

    ```
    export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-<datacenter>-<cluster_name>.yml
    ```
    {: screen}
4.  レッスンを続行して、[{{site.data.keyword.la_short}}](#openshift_logdna) および [{{site.data.keyword.mon_short}}](#openshift_sysdig) をセットアップします。

### レッスン 4a: LogDNA をセットアップする
{: #openshift_logdna}

{{site.data.keyword.la_full_notm}} のプロジェクトおよび特権サービス・アカウントをセットアップします。次に、ご使用の {{site.data.keyword.Bluemix_notm}} アカウントで {{site.data.keyword.la_short}} インスタンスを作成します。{{site.data.keyword.la_short}}インスタンスを OpenShift クラスターと統合するには、デプロイされたデーモン・セットを、特権サービス・アカウントを使用して root として実行するように変更する必要があります。
{: shortdesc}

1.  LogDNA のプロジェクトおよび特権サービス・アカウントをセットアップします。
    1.  クラスター管理者として、`logdna` プロジェクトを作成します。
        ```
        oc adm new-project logdna
        ```
        {: pre}
    2.  作成する後続のリソースが `logdna` プロジェクト名前空間に入るように、プロジェクトをターゲットにします。
        ```
        oc project logdna
        ```
        {: pre}
    3.  `logdna` プロジェクトのサービス・アカウントを作成します。
        ```
        oc create serviceaccount logdna
        ```
        {: pre}
    4.  特権セキュリティー・コンテキスト制約を `logdna` プロジェクトのサービス・アカウントに追加します。<p class="note">`privileged` SCC ポリシーがサービス・アカウントに与える許可を確認する場合は、`oc describe scc privileged` を実行します。SCC について詳しくは、[OpenShift の資料 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.openshift.com/container-platform/3.11/admin_guide/manage_scc.html) を参照してください。</p>
        ```
        oc adm policy add-scc-to-user privileged -n logdna -z logdna
        ```
        {: pre}
2.  クラスターと同じリソース・グループに {{site.data.keyword.la_full_notm}} インスタンスを作成します。ログの保存期間を決定する価格プラン (例えば、ログを 0 日間保持する `lite`) を選択します。この地域は、クラスターの地域と一致する必要はありません。詳しくは、[インスタンスのプロビジョニング](/docs/services/Log-Analysis-with-LogDNA/tutorials?topic=LogDNA-provision)および[価格プラン](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-about#overview_pricing_plans)を参照してください。
    ```
    ibmcloud resource service-instance-create <service_instance_name> logdna (lite|7-days|14-days|30-days) <region> [-g <resource_group>]
    ```
    {: pre}
    
    コマンド例:
    ```
    ibmcloud resource service-instance-create logdna-openshift logdna lite us-south
    ```
    {: pre}
    
    出力に含まれる、`crn:v1:bluemix:public:logdna:<region>:<ID_string>::` の形式のサービス・インスタンス **ID** をメモします。
    ```
    Service instance <name> was created.
                 
    Name:         <name>   
    ID:            crn:v1:bluemix:public:logdna:<region>:<ID_string>::   
    GUID:         <guid>   
    Location:     <region>   
    ...
    ```
    {: screen}    
3.  {{site.data.keyword.la_short}} インスタンス取り込み鍵を取得します。LogDNA 取り込みサーバーに対してセキュア Web ソケットを開く際、およびロギング・エージェントを {{site.data.keyword.la_short}} サービスで認証する際に、LogDNA 取り込み鍵が使用されます。
    1.  LogDNA インスタンスのサービス・キーを作成します。
        ```
        ibmcloud resource service-key-create <key_name> Administrator --instance-id <logdna_instance_ID>
        ```
        {: pre}
    2.  サービス・キーの **ingestion_key** をメモします。
        ```
        ibmcloud resource service-key <key_name>
        ```
        {: pre}
        
        出力例:
        ```
        Name:          <key_name>  
        ID:            crn:v1:bluemix:public:logdna:<region>:<ID_string>::    
        Created At:    Thu Jun  6 21:31:25 UTC 2019   
        State:         active   
        Credentials:                                   
                       apikey:                   <api_key_value>      
                       iam_apikey_description:   Auto-generated for key <ID>     
                       iam_apikey_name:          <key_name>       
                       iam_role_crn:             crn:v1:bluemix:public:iam::::role:Administrator      
                       iam_serviceid_crn:        crn:v1:bluemix:public:iam-identity::<ID_string>     
                       ingestion_key:            111a11aa1a1a11a1a11a1111aa1a1a11    
        ```
        {: screen}
4.  サービス・インスタンスの LogDNA 取り込み鍵を保管する Kubernetes シークレットを作成します。
    ```
     oc create secret generic logdna-agent-key --from-literal=logdna-agent-key=<logDNA_ingestion_key>
    ```
    {: pre}
5.  Kubernetes デーモン・セットを作成して、Kubernetes クラスターのすべてのワーカー・ノードに LogDNA エージェントをデプロイします。LogDNA エージェントは、ポッドの `/var/log` ディレクトリーに保管されている拡張子のないファイルと拡張子が `*.log` のログを収集します。デフォルトでは、`kube-system` を含めすべての名前空間からログが収集され、{{site.data.keyword.la_short}} サービスに自動的に転送されます。
    ```
    oc create -f https://assets.us-south.logging.cloud.ibm.com/clients/logdna-agent-ds.yaml
    ```
    {: pre}
6.  LogDNA エージェントのデーモン・セット構成を編集して、以前に作成したサービス・アカウントを参照し、セキュリティー・コンテキストを privileged に設定します。
    ```
    oc edit ds logdna-agent
    ```
    {: pre}
    
    構成ファイルで、以下の仕様を追加します。
    *   `spec.template.spec` で、`serviceAccount: logdna` を追加します。
    *   `spec.template.spec.containers` で、`securityContext: privileged: true` を追加します。
    *   {{site.data.keyword.la_short}} インスタンスを `us-south` 以外の地域に作成した場合は、`LDAPIHOST` および `LDLOGHOST` の `spec.template.spec.containers.env` 環境変数値を `<region>` で更新します。

    出力例:
    ```
    apiVersion: extensions/v1beta1
    kind: DaemonSet
    spec:
      ...
      template:
        ...
        spec:
          serviceAccount: logdna
          containers:
          - securityContext:
              privileged: true
            ...
            env:
            - name: LOGDNA_AGENT_KEY
              valueFrom:
                secretKeyRef:
                  key: logdna-agent-key
                  name: logdna-agent-key
            - name: LDAPIHOST
              value: api.<region>.logging.cloud.ibm.com
            - name: LDLOGHOST
              value: logs.<region>.logging.cloud.ibm.com
          ...
    ```
    {: screen}
7.  各ノードの `logdna-agent` ポッドが **Running** の状況であることを確認します。
    ```
    oc get pods
    ```
    {: pre}
8.  [{{site.data.keyword.Bluemix_notm}} のプログラム識別情報 > ロギング・コンソール](https://cloud.ibm.com/observe/logging)から、{{site.data.keyword.la_short}} インスタンスの行で、**「LogDNA の表示」**をクリックします。LogDNA ダッシュボードが開き、ログの分析を開始できます。

{{site.data.keyword.la_short}} の使用方法について詳しくは、[次のステップの資料](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-kube#kube_next_steps)を参照してください。

### レッスン 4b: Sysdig をセットアップする
{: #openshift_sysdig}

{{site.data.keyword.mon_full_notm}} インスタンスを {{site.data.keyword.Bluemix_notm}} アカウントで作成します。{{site.data.keyword.mon_short}}インスタンスを OpenShift クラスターと統合するには、Sysdig エージェント用のプロジェクトと特権サービス・アカウントを作成するスクリプトを実行する必要があります。
{: shortdesc}

1.  クラスターと同じリソース・グループに {{site.data.keyword.mon_full_notm}} インスタンスを作成します。ログの保存期間を決定する価格プラン (例えば、`lite`) を選択します。この地域は、クラスターの地域と一致する必要はありません。詳しくは、[インスタンスのプロビジョニング](/docs/services/Monitoring-with-Sysdig?topic=Sysdig-provision)を参照してください。
    ```
    ibmcloud resource service-instance-create <service_instance_name> sysdig-monitor (lite|graduated-tier) <region> [-g <resource_group>]
    ```
    {: pre}
    
    コマンド例:
    ```
    ibmcloud resource service-instance-create sysdig-openshift sysdig-monitor lite us-south
    ```
    {: pre}
    
    出力に含まれる、`crn:v1:bluemix:public:logdna:<region>:<ID_string>::` の形式のサービス・インスタンス **ID** をメモします。
    ```
    Service instance <name> was created.
                 
    Name:         <name>   
    ID:            crn:v1:bluemix:public:sysdig-monitor:<region>:<ID_string>::   
    GUID:         <guid>   
    Location:     <region>   
    ...
    ```
    {: screen}    
2.  {{site.data.keyword.mon_short}} インスタンスのアクセス・キーを取得します。Sysdig 取り込みサーバーに対してセキュア Web ソケットを開く際、およびモニター・エージェントを {{site.data.keyword.mon_short}} サービスで認証する際に、Sysdig アクセス・キーが使用されます。
    1.  Sysdig インスタンスのサービス・キーを作成します。
        ```
        ibmcloud resource service-key-create <key_name> Administrator --instance-id <sysdig_instance_ID>
        ```
        {: pre}
    2.  サービス・キーの **Sysdig Access Key** と **Sysdig Collector Endpoint** をメモします。
        ```
        ibmcloud resource service-key <key_name>
        ```
        {: pre}
        
        出力例:
        ```
        Name:          <key_name>  
        ID:            crn:v1:bluemix:public:sysdig-monitor:<region>:<ID_string>::    
        Created At:    Thu Jun  6 21:31:25 UTC 2019   
        State:         active   
        Credentials:                                   
                       Sysdig Access Key:           11a1aa11-aa1a-1a1a-a111-11a11aa1aa11      
                       Sysdig Collector Endpoint:   ingest.<region>.monitoring.cloud.ibm.com      
                       Sysdig Customer Id:          11111      
                       Sysdig Endpoint:             https://<region>.monitoring.cloud.ibm.com  
                       apikey:                   <api_key_value>      
                       iam_apikey_description:   Auto-generated for key <ID>     
                       iam_apikey_name:          <key_name>       
                       iam_role_crn:             crn:v1:bluemix:public:iam::::role:Administrator      
                       iam_serviceid_crn:        crn:v1:bluemix:public:iam-identity::<ID_string>       
        ```
        {: screen}
3.  特権サービス・アカウントと Kubernetes デーモン・セットで `ibm-observe` プロジェクトをセットアップして、Kubernetes クラスターのすべてのワーカー・ノードに Sysdig エージェントをデプロイするスクリプトを実行します。Sysdig エージェントは、ワーカー・ノードの CPU 使用量、ワーカー・ノードのメモリー使用量、コンテナーとの間の HTTP トラフィック、いくつかのインフラストラクチャー・コンポーネントに関するデータなどのメトリックを収集します。 

    以下のコマンドで、<sysdig_access_key> と <sysdig_collector_endpoint> を、以前に作成したサービス・キーの値に置き換えます。<tag> で、タグを Sysdig エージェントに関連付けることができます。例えば、`role:service,location:us-south` は、メトリックの取得元の環境を識別するのに役立ちます。

    ```
    curl -sL https://ibm.biz/install-sysdig-k8s-agent | bash -s -- -a <sysdig_access_key> -c <sysdig_collector_endpoint> -t <tag> -ac 'sysdig_capture_enabled: false' --openshift
    ```
    {: pre}
    
    出力例: 
    ```
    * Detecting operating system
    * Downloading Sysdig cluster role yaml
    * Downloading Sysdig config map yaml
    * Downloading Sysdig daemonset v2 yaml
    * Creating project: ibm-observe
    * Creating sysdig-agent serviceaccount in project: ibm-observe
    * Creating sysdig-agent access policies
    * Creating sysdig-agent secret using the ACCESS_KEY provided
    * Retreiving the IKS Cluster ID and Cluster Name
    * Setting cluster name as <cluster_name>
    * Setting ibm.containers-kubernetes.cluster.id 1fbd0c2ab7dd4c9bb1f2c2f7b36f5c47
    * Updating agent configmap and applying to cluster
    * Setting tags
    * Setting collector endpoint
    * Adding additional configuration to dragent.yaml
    * Enabling Prometheus
    configmap/sysdig-agent created
    * Deploying the sysdig agent
    daemonset.extensions/sysdig-agent created
    ```
    {: screen}
        
4.  各ノードの `sydig-agent` ポッドで、**1/1** ポッドの準備ができており、各ポッドの状況が **Running** であることが示されていることを確認します。
    ```
    oc get pods
    ```
    {: pre}
    
    出力例:
    ```
    NAME                 READY     STATUS    RESTARTS   AGE
    sysdig-agent-qrbcq   1/1       Running   0          1m
    sysdig-agent-rhrgz   1/1       Running   0          1m
    ```
    {: screen}
5.  [{{site.data.keyword.Bluemix_notm}} のプログラム識別情報 > モニタリング・コンソール](https://cloud.ibm.com/observe/logging)から、{{site.data.keyword.mon_short}} インスタンスの行で、**「Sysdig の表示」**をクリックします。Sysdig ダッシュボードが開き、クラスター・メトリックの分析を開始できます。

{{site.data.keyword.mon_short}} の使用方法について詳しくは、[次のステップの資料](/docs/services/Monitoring-with-Sysdig?topic=Sysdig-kubernetes_cluster#kubernetes_cluster_next_steps)を参照してください。

### オプション: クリーンアップ
{: #openshift_logdna_sysdig_cleanup}

クラスターおよび {{site.data.keyword.Bluemix_notm}} アカウントから {{site.data.keyword.la_short}} インスタンスと {{site.data.keyword.mon_short}} インスタンスを削除します。[永続ストレージ](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-archiving)にログとメトリックを保管しない限り、アカウントからインスタンスを削除した後にこの情報にアクセスすることはできません。
{: shortdesc}

1.  クラスター内の {{site.data.keyword.la_short}} インスタンスおよび {{site.data.keyword.mon_short}} インスタンスを、それらに対して作成したプロジェクトを削除することによってクリーンアップします。プロジェクトを削除すると、そのリソース (サービス・アカウントやデーモン・セットなど) も削除されます。
    ```
    oc delete project logdna
    ```
    {: pre}
    ```
    oc delete project ibm-observe
    ```
    {: pre}
2.  {{site.data.keyword.Bluemix_notm}} アカウントからインスタンスを削除します。
    *   [{{site.data.keyword.la_short}} インスタンスを削除します](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-remove)。
    *   [{{site.data.keyword.mon_short}} インスタンスを削除します](/docs/services/Monitoring-with-Sysdig?topic=Sysdig-remove)。

<br />


## 制限
{: #openshift_limitations}

Red Hat OpenShift on IBM Cloud のベータ版は、以下の制限付きでリリースされます。
{: shortdesc}

**クラスター**:
*   標準クラスターのみを作成できます。フリー・クラスターは作成できません。
*   ロケーションは、2 つの複数ゾーンの大都市領域 (ワシントン DC およびロンドン) で使用可能です。サポートされるゾーンは `wdc04、wdc06、wdc07、lon04、lon05、`および `lon06` です。
*   Red Hat Enterprise Linux の OpenShift と Ubuntu のネイティブの Kubernetes など、複数のオペレーティング・システムで稼働するワーカー・ノードを含むクラスターを作成することはできません。
*   [クラスター自動スケーリング機能](/docs/containers?topic=containers-ca)は、Kubernetes バージョン 1.12 以降を必要とするため、サポートされません。OpenShift 3.11 には、Kubernetes バージョン 1.11 のみが含まれています。



**ストレージ**:
*   {{site.data.keyword.Bluemix_notm}} ファイル、ブロック、およびクラウド・オブジェクト・ストレージがサポートされています。Portworx ソフトウェア定義ストレージ (SDS) はサポートされていません。
*   {{site.data.keyword.Bluemix_notm}} NFS ファイル・ストレージが Linux ユーザー許可を構成する方法によって、ファイル・ストレージの使用時にエラーが発生する場合があります。その場合は、[OpenShift Security Context Constraints ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.openshift.com/container-platform/3.11/admin_guide/manage_scc.html) を構成するか、別のストレージ・タイプを使用する必要がある場合があります。

**ネットワーキング**:
*   Calico は、OpenShift SDN の代わりにネットワーキング・ポリシー・プロバイダーとして使用されます。

**アドオン、統合、およびその他のサービス**:
*   Istio、Knative、Kubernetes Terminal などの {{site.data.keyword.containerlong_notm}} アドオンは使用できません。
*   {{site.data.keyword.Bluemix_notm}} Object Storage を除いて、OpenShift クラスターでの Helm チャートの動作は保証されていません。
*   クラスターは、{{site.data.keyword.registryshort_notm}} `icr.io` ドメインのイメージ・プル・シークレットでは、デプロイされません。[独自のイメージ・プル・シークレットを作成](/docs/containers?topic=containers-images#other_registry_accounts)することも、代わりに OpenShift クラスター用の組み込みの Docker レジストリーを使用することもできます。

**アプリ**:
*   OpenShift は、デフォルトでは、ネイティブの Kubernetes よりも制限されたセキュリティー設定をセットアップします。詳しくは、[Managing Security Context Constraints (SCC) ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.openshift.com/container-platform/3.11/admin_guide/manage_scc.html) の OpenShift の資料を参照してください。
*   例えば、root として実行するように構成されているアプリは、ポッドが `CrashLoopBackOff` の状況のときに失敗する可能性があります。この問題を解決するには、デフォルトのセキュリティー・コンテキスト制約を変更するか、root として実行されないイメージを使用します。
*   OpenShift は、デフォルトでは、ローカルの Docker レジストリーを使用してセットアップされます。リモートのプライベート {{site.data.keyword.registrylong_notm}} の `icr.io` ドメイン・ネームに保管されているイメージを使用する場合は、グローバル・レジストリーおよび地域レジストリーごとのシークレットを自分で作成する必要があります。`default` の名前空間から、イメージをプルする名前空間に [`default-<region>-icr-io` シークレットをコピー](/docs/containers?topic=containers-images#copy_imagePullSecret)することも、[独自のシークレットを作成](/docs/containers?topic=containers-images#other_registry_accounts)することもできます。その後、デプロイメント構成または名前空間サービス・アカウントに[イメージ・プル・シークレットを追加](/docs/containers?topic=containers-images#use_imagePullSecret)します。
*   Kubernetes ダッシュボードの代わりに OpenShift コンソールが使用されます。

<br />


## 次の作業
{: #openshift_next}

アプリおよびルーティング・サービスの操作について詳しくは、[OpenShift Developer Guide](https://docs.openshift.com/container-platform/3.11/dev_guide/index.html) を参照してください。

<br />


## フィードバックおよび質問
{: #openshift_support}

ベータ版では、Red Hat OpenShift on IBM Cloud クラスターは、IBM サポートおよび Red Hat サポートの対象外です。提供されるサポートは、製品の一般出荷可能日に備えて製品を評価する場合に役立ちます。
{: important}

質問やフィードバックについては、Slack に投稿してください。 
*   外部ユーザーの場合は、[#openshift ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://ibm-container-service.slack.com/messages/CKCJLJCH4) チャネルに投稿してください。 
*   IBM 登録ユーザーの場合は、[#iks-openshift-users ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://ibm-argonauts.slack.com/messages/CJH0UPN2D) チャネルを使用してください。

自身の {{site.data.keyword.Bluemix_notm}} アカウントで IBMid を使用していない場合は、この Slack への[招待を要求](https://bxcs-slack-invite.mybluemix.net/)してください。
{: tip}
