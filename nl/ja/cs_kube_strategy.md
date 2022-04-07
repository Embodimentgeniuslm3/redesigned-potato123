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


# Kubernetes 戦略の定義
{: #strategy}

{{site.data.keyword.containerlong}} を使用すると、実動しているアプリのコンテナー・ワークロードを素早く安全にデプロイできます。 クラスター戦略を計画する場合は、詳細情報を確認して、[Kubernetes![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/) によって自動化されたデプロイ、スケーリング、およびオーケストレーションの管理機能を最大限に活用するようにセットアップを最適化してください。
{:shortdesc}

## {{site.data.keyword.Bluemix_notm}} へのワークロードの移動
{: #cloud_workloads}

ワークロードを {{site.data.keyword.Bluemix_notm}} に移動する理由はさまざまですが、その例としては、総所有コストの削減、各種の要件に準拠しているセキュアな環境でのアプリの可用性向上、ユーザーの要求に応じたスケールアップとスケールダウンなどが挙げられます。 {{site.data.keyword.containerlong_notm}} では、コンテナー・テクノロジーとオープン・ソース・ツール (Kubernetes など) を組み合わせているため、異なるクラウド環境間でマイグレーションできるクラウド・ネイティブ・アプリを作成して、ベンダーの囲い込みを回避できます。
{:shortdesc}

では、どのようにクラウドを導入しますか? そこに至る過程で、どのようなオプションがあるでしょうか? そして、クラウドの導入後にワークロードをどのようにして管理しますか?

このページでは、{{site.data.keyword.containerlong_notm}} 上の Kubernetes デプロイメントに関するいくつかの戦略について説明します。 いつでも、[Slack ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://ibm-container-service.slack.com) を通じて IBM のチームと自由にやり取りしてください。

Slack をまだご利用でない場合は、 [招待を要求してください。](https://bxcs-slack-invite.mybluemix.net/)
{: tip}

### {{site.data.keyword.Bluemix_notm}} には何を移動できますか?
{: #move_to_cloud}

{{site.data.keyword.Bluemix_notm}} を使用すると、[オフプレミス、オンプレミス、またはハイブリッド・クラウドの各環境](/docs/containers?topic=containers-cs_ov#differentiation)で、Kubernetes クラスターを柔軟に作成できます。 次の表では、ユーザーがさまざまなタイプのクラウドに一般に移動するワークロードのタイプを例示しています。 両方の環境でクラスターが実行されるハイブリッド・アプローチを選択することもできます。
{: shortdesc}

| ワークロード | {{site.data.keyword.containershort_notm}} オフプレミス | on-prem |
| --- | --- | --- |
| DevOps 実現ツール | <img src="images/confirm.svg" width="32" alt="機能は使用可能" style="width:32px;" /> | |
| アプリの開発とテスト | <img src="images/confirm.svg" width="32" alt="機能は使用可能" style="width:32px;" /> | |
| アプリの需要が大きく変化したため迅速なスケーリングが必要 | <img src="images/confirm.svg" width="32" alt="機能は使用可能" style="width:32px;" /> | |
| CRM、HCM、ERP、e-コマースなどのビジネス・アプリ | <img src="images/confirm.svg" width="32" alt="機能は使用可能" style="width:32px;" /> | |
| E メールなどのコラボレーション・ツールとソーシャル・ツール | <img src="images/confirm.svg" width="32" alt="機能は使用可能" style="width:32px;" /> | |
| Linux と x86 のワークロード | <img src="images/confirm.svg" width="32" alt="機能は使用可能" style="width:32px;" /> | |
| ベアメタルおよび GPU のコンピュート・リソース | <img src="images/confirm.svg" width="32" alt="機能は使用可能" style="width:32px;" /> | <img src="images/confirm.svg" width="32" alt="機能は使用可能" style="width:32px;" /> |
| PCI 準拠および HIPAA 準拠のワークロード | <img src="images/confirm.svg" width="32" alt="機能は使用可能" style="width:32px;" /> | <img src="images/confirm.svg" width="32" alt="機能は使用可能" style="width:32px;" /> |
| プラットフォームやインフラストラクチャーに関する制約や依存関係のあるレガシー・アプリ | | <img src="images/confirm.svg" width="32" alt="機能は使用可能" style="width:32px;" /> |
| 厳格な設計、ライセンス交付、または厳しい規制を伴う独自開発アプリ | | <img src="images/confirm.svg" width="32" alt="機能は使用可能" style="width:32px;" /> |
| パブリック・クラウド内のアプリのスケーリングと、オンサイト・プライベート・データベースとの間のデータの同期 | <img src="images/confirm.svg" width="32" alt="機能は使用可能" style="width:32px;" />  | <img src="images/confirm.svg" width="32" alt="機能は使用可能" style="width:32px;" /> |
{: caption="{{site.data.keyword.Bluemix_notm}} の実装ではワークロードがサポートされています" caption-side="top"}

**{{site.data.keyword.containerlong_notm}} でワークロードをオフプレミスで実行する準備は整いましたか?**</br>
ここまでに、既にパブリック・クラウドの資料をお読みいただいています。 さらに読み続けてその他の戦略アイデアを確認するか、[今すぐクラスターを作成](/docs/containers?topic=containers-getting-started)して作業を開始してください。

**オンプレミス・クラウドに関心をお持ちですか?**</br>
[{{site.data.keyword.Bluemix_notm}} Private の資料 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.1/kc_welcome_containers.html) を参照してください。 WebSphere Application Server や Liberty などの IBM テクノロジーに既に多大な投資をしている場合は、各種のツールを使用して {{site.data.keyword.Bluemix_notm}} Private のモダナイズ戦略を最適化できます。

**オンプレミスとオフプレミスの両方のクラウドでワークロードを実行しますか?**</br>
まず {{site.data.keyword.Bluemix_notm}} Private アカウントをセットアップしてください。 次に、[{{site.data.keyword.Bluemix_notm}} Private と組み合わせた {{site.data.keyword.containerlong_notm}} の使用](/docs/containers?topic=containers-hybrid_iks_icp)を参照して、使用する {{site.data.keyword.Bluemix_notm}} Private 環境を {{site.data.keyword.Bluemix_notm}} Public 内のクラスターと接続します。 複数のクラウド Kubernetes クラスターを管理するには ({{site.data.keyword.Bluemix_notm}} Public と {{site.data.keyword.Bluemix_notm}} Private にまたがる場合など)、[IBM Multicloud Manager ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.1.0/mcm/getting_started/introduction.html) を確認してください。

### {{site.data.keyword.containerlong_notm}} ではどのようなアプリを実行できますか?
{: #app_types}

コンテナー化されたアプリは、サポートされているオペレーティング・システム Ubuntu 16.64/18.64 上で実行できる必要があります。 アプリのステートフル性も考慮してください。
{: shortdesc}

<dl>
<dt>ステートレス・アプリ</dt>
  <dd><p>ステートレス・アプリは、Kubernetes のようなクラウド・ネイティブ環境に適しています。 これらのアプリはマイグレーションとスケーリングが簡単です。これは、これらのアプリでは、依存関係が宣言され、構成がコードとは別に保管され、データベースなどのバッキング・サービスが、アプリに結合されたものではなく接続されたリソースとして扱われるためです。 アプリ・ポッドは永続データ・ストレージや安定したネットワーク IP アドレスを必要としないため、ワークロードの需要に応じてポッドを終了、スケジュール変更、およびスケーリングできます。 このアプリでは、永続データに対して Database-as-a-Service が使用されるとともに、NodePort サービス、ロード・バランサー・サービス、または Ingress サービスを使用して安定した IP アドレス上でワークロードが公開されます。</p></dd>
<dt>ステートフルなアプリ</dt>
  <dd><p>ステートフル・アプリは、ステートレス・アプリよりも、セットアップ、管理、およびスケーリングが複雑になります。これは、ポッドが永続データと安定したネットワーク ID を必要とするためです。 ステートフル・アプリは、多くの場合、データベース、またはその他の分散型でデータ集中型のワークロードであり、データ自体に近ければ近いほど、処理効率が高くなります。</p>
  <p>ステートフル・アプリをデプロイする場合は、永続ストレージをセットアップして、StatefulSet オブジェクトによって制御されるポッドに永続ボリュームをマウントする必要があります。 ステートフル・セット用の永続ストレージとして、[ファイル](/docs/containers?topic=containers-file_storage#file_statefulset)・ストレージ、[ブロック](/docs/containers?topic=containers-block_storage#block_statefulset)・ストレージ、または[オブジェクト](/docs/containers?topic=containers-object_storage#cos_statefulset)・ストレージを追加することを選択できます。 また、ベアメタル・ワーカー・ノード上に [Portworx](/docs/containers?topic=containers-portworx) をインストールし、高可用性のソフトウェア定義ストレージ・ソリューションとして Portworx を使用して、ステートフル・アプリ用の永続ストレージを管理することができます。 ステートフル・セットの仕組みについて詳しくは、[Kubernetes の資料 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/) を参照してください。</p></dd>
</dl>

### ステートレスなクラウド・ネイティブ・アプリを開発するためのガイドラインはどのようなものですか?
{: #12factor}

[Twelve-Factor App ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://12factor.net/) を確認してください。これは、以下に要約している 12 個の要素を考慮してアプリを開発する方法を検討するための言語非依存の手法です。
{: shortdesc}

1.  **コードベース**: デプロイメントのバージョン管理システムで単一のコードベースを使用します。 コンテナー・デプロイメントのイメージをプルする際は、`latest` を使用する代わりにテスト済みのイメージ・タグを指定します。
2.  **依存関係**: 外部的な依存関係を明示的に宣言して分離します。
3.  **構成**: デプロイメント固有の構成をコード内ではなく環境変数に保管します。
4.  **バッキング・サービス**: データ・ストアやメッセージ・キューなどのバッキング・サービスを、接続されたリソースまたは交換可能なリソースとして扱います。
5.  **アプリのステージ**: `build`、`release`、`run` などの独立したステージを取り入れて、これらのステージを厳格に分離します。
6.  何も共有しない 1 つ以上のステートレス・プロセスとして実行します。また、データ保存用に [永続ストレージ](/docs/containers?topic=containers-storage_planning)を使用します。
7.  **ポート・バインディング**: ポート・バインディングは自己完結型であり、明確に定義されたホストとポート上でサービス・エンドポイントを提供します。
8.  **並行性**: レプリカや水平スケーリングなどのプロセス・インスタンスを通じてアプリを管理およびスケーリングします。 デプロイメントに対するリソースの要求と制限を設定します。 Calico ネットワーク・ポリシーによって帯域幅を制限することはできません。 代わりに、[Istio](/docs/containers?topic=containers-istio) を検討してください。
9.  **廃棄容易性**: 最小の起動時間、安全なシャットダウン、突然のプロセス終了への耐性を備えた、廃棄可能なアプリを設計します。 コンテナー、ポッド、さらにはワーカー・ノードも廃棄可能であることを意図しているため、アプリもこれらに従って計画してください。
10.  **開発と実運用の一致**: アプリに対する[継続的統合](https://www.ibm.com/cloud/garage/content/code/practice_continuous_integration/)と[継続的デリバリー](https://www.ibm.com/cloud/garage/content/deliver/practice_continuous_delivery/)のパイプラインをセットアップして、開発時のアプリと実運用時のアプリの差異を最小化してください。
11.  **ログ**: ログをイベント・ストリームとして扱います。外部環境またはホスティング環境によってログ・ファイルが処理されてルーティングされます。 **重要**: {{site.data.keyword.containerlong_notm}} では、ログはデフォルトで有効になっていません。 ログを有効にするには、[ログ転送の構成](/docs/containers?topic=containers-health#configuring)を参照してください。
12.  **管理プロセス**: 一回限りの管理スクリプトをアプリとともに保持して、これらを 1 つの [Kubernetes ジョブ・オブジェクト ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/concepts/workloads/controllers/jobs-run-to-completion/) として実行することで、それらの管理スクリプトがアプリ自体と同じ環境で実行されるようにします。 Kubernetes クラスターで実行する大規模なパッケージのオーケストレーションを実現するには、[Helm ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://helm.sh/) などのパッケージ・マネージャーの使用を検討してください。

### 既にアプリがあります。 どうすれば {{site.data.keyword.containerlong_notm}} にマイグレーションできますか?
{: #migrate_containerize}

以下の一般的な手順を実行することで、アプリをコンテナー化できます。
{: shortdesc}

1.  [Twelve-Factor App ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://12factor.net/) をガイドとして使用し、依存関係を分離して、複数のプロセスを別々のサービスに分けて、アプリのステートフル性をできる限り低減します。
2.  使用する適切な基本イメージを見つけます。 [Docker Hub ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://hub.docker.com/) で一般公開されているイメージを使用することも、[パブリック IBM イメージ](/docs/services/Registry?topic=registry-public_images#public_images)を使用することも、独自のイメージを作成してプライベート {{site.data.keyword.registryshort_notm}} 内で管理することもできます。
3.  アプリを実行するために必要なもののみを Docker イメージに追加します。
4.  ローカル・ストレージを利用する代わりに、永続ストレージまたはクラウド Database as a Service のソリューションを使用してアプリのデータをバックアップすることを計画してください。
5.  時間をかけて、アプリのプロセスをリファクタリングしてマイクロサービスに変換します。

詳しくは、以下のチュートリアルを参照してください。
*  [Cloud Foundry からクラスターへのアプリのマイグレーション](/docs/containers?topic=containers-cf_tutorial#cf_tutorial)
*  [Kubernetes への VM ベース・アプリの移動](/docs/tutorials?topic=solution-tutorials-vm-to-containers-and-kubernetes)



以下のトピックを引き続き参照して、ワークロードを移動する際の考慮事項を確認してください。これらの考慮事項としては、Kubernetes 環境、高可用性、サービス・ディスカバリー、デプロイメントなどが挙げられます。

<br />


### アプリを {{site.data.keyword.containerlong_notm}} に移動するために役立つ知識と技術スキルは何ですか?
{: #knowledge}

Kubernetes は、クラスター管理者とアプリ開発者という 2 人の主要な個人に対して機能を提供するために設計されています。 各個人は、異なる技術スキルを使用してアプリを正常に実行し、クラスターにデプロイします。
{: shortdesc}

**クラスター管理者のメインタスクと技術知識は何ですか?** </br>
クラスター管理者は、クラスターの {{site.data.keyword.Bluemix_notm}} インフラストラクチャーのセットアップ、操作、保護、および管理を担当します。 標準的なタスクは、以下のとおりです。
- ワークロードに十分な容量を提供できるよう、クラスターのサイズを設定します。
- 高可用性、災害復旧、および会社のコンプライアンスの規格を満たすよう、クラスターを設計します。
- コンピュート・リソース、ネットワーク、およびデータを保護するためにユーザー許可をセットアップし、クラスター内の操作を制限することで、クラスターを保護します。
- ネットワーク・セキュリティー、ネットワーク・セグメンテーション、ネットワーク・コンプライアンスを確保できるよう、インフラストラクチャー・コンポーネント間のネットワーク通信を計画および管理します。
- データの常駐およびデータの保護の要件を満たすよう、永続ストレージ・オプションを計画します。

クラスター管理者には、コンピュート、ネットワーク、ストレージ、セキュリティー、およびコンプライアンスなど、幅広い知識が必要です。 標準的な会社では、この知識はシステム・エンジニア、システム管理者、ネットワーク・エンジニア、ネットワーク設計者、IT マネージャー、セキュリティー・スペシャリスト、およびコンプライアンス・スペシャリストなど、複数のスペシャリスト間で分散されています。 クラスター管理の役割を会社内の複数の個人に割り当てて、クラスターを正常に運用するために必要な知識を得ることを検討してください。

**アプリ開発者のメインタスクおよび技術スキルは何ですか?** </br>
開発者は、Kubernetes クラスター内のクラウド・ネイティブのコンテナー化アプリを設計、作成、保護、デプロイ、テスト、実行、およびモニターします。 これらのアプリを作成および実行するには、マイクロサービスの概念、[12-Factor App](#12factor) のガイドライン、[Docker とコンテナー化の原則](https://www.docker.com/)、および使用可能な [Kubernetes デプロイメント・オプション](/docs/containers?topic=containers-app#plan_apps)に精通している必要があります。 サーバーレス・アプリをデプロイする場合は、[Knative](/docs/containers?topic=containers-cs_network_planning) をよく理解してください。

Kubernetes および {{site.data.keyword.containerlong_notm}} では、[アプリの公開とアプリの非公開化](/docs/containers?topic=containers-cs_network_planning)、[永続ストレージの追加](/docs/containers?topic=containers-storage_planning)、[他のサービスの統合](/docs/containers?topic=containers-ibm-3rd-party-integrations)、および[ワークロードの保護と機密データの保護](/docs/containers?topic=containers-security#container)を行う方法について、複数のオプションが提供されています。 {{site.data.keyword.containerlong_notm}} でアプリをクラスターに移動する前に、サポート対象の Ubuntu 16.64、18.64 のオペレーティング・システムでアプリをコンテナー化アプリとして実行できること、および Kubernetes と {{site.data.keyword.containerlong_notm}} にワークロードに必要な機能が備わっていることを確認してください。

**クラスター管理者と開発者は相互に対話しますか?** </br>
はい。 クラスター管理者と開発者は、クラスター管理者がワークロード要件を理解してクラスターにこの機能を提供し、開発者がアプリ開発プロセスで考慮する必要がある使用可能な制限、統合、およびセキュリティー原則について把握できるように、頻繁に対話する必要があります。

## ワークロードをサポートするための Kubernetes クラスターのサイズ設定
{: #sizing}

現在のワークロードをサポートするためにクラスター内で必要なワーカー・ノードの数を精密に算出することはできません。 さまざまな構成のテストと適応が必要となる場合があります。 幸いにも {{site.data.keyword.containerlong_notm}} を使用しているため、ワークロードの需要に応じてワーカー・ノードを追加および削除できます。
{: shortdesc}

クラスターのサイズ設定を開始するには、以下の質問に回答してください。

### アプリにはリソースがいくつ必要ですか?
{: #sizing_resources}

まず、既存のワークロードまたはプロジェクト・ワークロードの使用状況から始めましょう。

1.  ワークロードの平均 CPU 使用量または平均メモリー使用量を算出します。 例えば、Windows マシンではタスク・マネージャーを参照して、Mac や Linux では `top` コマンドを実行します。 メトリック・サービスを使用してレポートを実行することで、ワークロードの使用状況を計算することもできます。
2.  ワークロードによって対処する必要のある要求の数を予測することで、そのワークロードを処理するために必要なアプリ・レプリカの数を決定できるようにします。 例えば、毎分 1000 件の要求を処理するようにアプリ・インスタンスを設計して、ワークロードによって毎分 10000 件の要求に対処する必要があると予測するとします。 その場合は、12 個のアプリ・レプリカを作成することを決定して、そのうちの 10 個のレプリカによって予測される数の要求を処理して、残りの 2 個のレプリカによって要求される能力の急増に対応できます。

### アプリ以外に、クラスター内のリソースを使用するものは何ですか?
{: #sizing_other}

ここでは、使用する可能性のあるその他の機能を追加しましょう。



1.  ワーカー・ノード上のローカル・ストレージを占有する可能性のある大きいイメージや多数のイメージが、アプリによってプルされるかどうかを検討します。
2.  クラスターに [Helm](/docs/containers?topic=containers-helm#public_helm_install) や [Prometheus ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/coreos/prometheus-operator/tree/master/contrib/kube-prometheus) などの[サービスを統合](/docs/containers?topic=containers-supported_integrations#supported_integrations)するかどうかを決定します。 これらの統合されたサービスとアドオンによって、クラスターのリソースを消費するポッドがスピンアップされます。

### どのようなタイプの可用性がワークロードに必要ですか?
{: #sizing_availability}

ワークロードはできる限り稼働状態を維持する必要があります。

1.  [高可用性のクラスター](/docs/containers?topic=containers-ha_clusters#ha_clusters)を実現するための戦略を綿密に計画します (単一ゾーン・クラスターか複数ゾーン・クラスターかの決定など)。
2.  [高可用性のデプロイメント](/docs/containers?topic=containers-app#highly_available_apps)を参考にして、アプリの可用性を確保する方法を決定します。

### ワークロードを処理するには、いくつのワーカー・ノードが必要ですか?
{: #sizing_workers}

ワークロードがどのようなものか把握したので、推定される使用状況を使用可能なクラスター構成に対応付けましょう。

1.  ワーカー・ノードの最大容量を推定します。これは、使用するクラスターのタイプによって異なります。 負荷の急増などの一時イベントが発生した場合に、ワーカー・ノード容量の上限に達しないようにしてください。
    *  **単一ゾーン・クラスター**: 少なくとも 3 つのワーカー・ノードをクラスター内に配置することを計画します。 さらに、余分の 1 ノード分の CPU とメモリーの容量をクラスター内に確保することをお勧めします。
    *  **複数ゾーン・クラスター**: ゾーンあたり少なくとも 2 つのワーカー・ノードを配置することを計画します。したがって、3 つのゾーンにまたがって合計 6 つのノードを配置することになります。 さらに、クラスターの合計容量が、必要な合計ワークロード容量の 150% 以上になるように計画します。そうすることで、1 つのゾーンがダウンした場合でも、ワークロードを維持するためのリソースが使用可能になります。
2.  アプリのサイズとワーカー・ノードの容量を、[使用可能なワーカー・ノード・フレーバー](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)の 1 つと合致させます。 ゾーン内で使用可能なフレーバーを表示するには、`ibmcloud ks machine-types <zone>` コマンドを実行します。
    *   **ワーカー・ノードを過負荷にしないこと**: ポッドが CPU に対して競合したり、非効率的に実行されたりしないようにするには、必要なワーカー・ノードの数を計画できるように、アプリが必要とするリソースを把握する必要があります。 例えば、アプリで必要なリソースが、ワーカー・ノード上で使用可能なリソースより少ない場合は、1 つのワーカー・ノードにデプロイするポッドの数を制限できます。 ワーカー・ノードを約 75% の容量で維持することで、スケジュールされる必要が生じる可能性のある他のポッド用のスペースを残しておいてください。 アプリで必要なリソースが、ワーカー・ノード上で使用可能なリソースより多い場合は、これらの要件を満たすことができる異なるワーカー・ノード・フレーバーを使用してください。 ワーカー・ノードによって `NotReady` という状況が頻繁に報告されたり、メモリーなどのリソース不足が原因でワーカー・ノードによってポッドが頻繁に強制除去されたりする場合は、ワーカー・ノードが過負荷になっていることがわかります。
    *   **大規模および小規模のワーカー・ノード・フレーバー**: 大規模ノードは小規模ノードよりもコスト効率が高くなることがあります。特に、高性能マシン上での処理時に効率化されるように設計されているワークロードにこのことが当てはまります。 ただし、大規模ワーカー・ノードがダウンした場合は、すべてのワークロード・ポッドをクラスター内の他のワーカー・ノードに移すスケジュール変更を正常に行うために十分な能力をクラスターが備えていることを確認する必要があります。 小規模ワーカーは、より安全にスケーリングするために役立ちます。
    *   **アプリのレプリカ**: 必要となるワーカー・ノードの数を決定するために、実行するアプリ・レプリカの数を検討することもできます。 例えば、ワークロードで 32 基の CPU コアが必要となることがわかっており、16 個のアプリ・レプリカを実行することを計画している場合は、各レプリカ・ポッドに 2 基の CPU コアが必要となります。 ワーカー・ノードあたり 1 つのアプリ・ポッドのみを実行する場合は、使用するクラスター・タイプでこの構成をサポートするための適切な数のワーカー・ノードを注文できます。
3.  パフォーマンス・テストを実行して、待ち時間、スケーラビリティー、データ・セット、およびワークロードに関する代表的な要件に基づいて、クラスター内で必要なワーカー・ノードの数をさらに絞り込みます。
4.  リソース要求に応じてスケールアップおよびスケールダウンする必要のあるワークロードについては、[水平ポッド自動スケーリング機能](/docs/containers?topic=containers-app#app_scaling)と[クラスター・ワーカー・プール自動スケーリング機能](/docs/containers?topic=containers-ca#ca)をセットアップします。

<br />


## Kubernetes 環境の構造化
{: #kube_env}

{{site.data.keyword.containerlong_notm}} は、1 つの IBM Cloud インフラストラクチャー (SoftLayer) ポートフォリオのみにリンクされています。 自分のアカウント内で、1 つのマスターと各種のワーカー・ノードで構成されるクラスターを作成できます。 マスターは IBM によって管理され、ユーザーは、同じフレーバー (または同じメモリー仕様と CPU 仕様) の個別マシンをまとめてプールするワーカー・プールの組み合わせを作成できます。 クラスター内で、名前空間とラベルを使用することによって、リソースをさらに細かく編成できます。 自分のチームとワークロードが必要なリソースを確実に得られるように、クラスター、マシン・タイプ、および編成戦略の適切な組み合わせを選択してください。
{:shortdesc}

### どのようなタイプのクラスターとマシン・タイプを取得する必要がありますか?
{: #env_flavors}

**クラスターのタイプ**: [単一ゾーン・クラスター・セットアップ、複数ゾーン・クラスター・セットアップ、または複数クラスター・セットアップ](/docs/containers?topic=containers-ha_clusters#ha_clusters)のどれにするかを決定します。 複数ゾーン・クラスターは、[世界各国にまたがる {{site.data.keyword.Bluemix_notm}} の 6 大都市地域のすべて](/docs/containers?topic=containers-regions-and-zones#zones)で使用できます。 また、ワーカー・ノードはゾーンごとに異なることも留意してください。

**ワーカー・ノードのタイプ**: 一般的に、集中型のワークロードはベアメタルの物理マシン上で実行することが適する一方、コスト効率が高いテスト作業や開発作業には、共有ハードウェアまたは専用の共有ハードウェア上の仮想マシンを選択します。 ベアメタルのワーカー・ノードを使用すると、クラスターは、その10 Gbps のネットワーク速度とハイパー・スレッディング対応コアによってスループットが向上します。 仮想マシンは、1 Gbps のネットワーク速度と、ハイパー・スレッディングに対応していない標準コアをサポートしています。 [マシンの分離と使用可能なフレーバーを確認してください](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)。

### 複数のクラスターを使用しますか? または既存のクラスターに単にワーカーを追加しますか?
{: #env_multicluster}

作成するクラスターの数は、ワークロード、企業のポリシーと規則、およびコンピューティング・リソースを使用して実行する内容に応じて異なります。 [コンテナーの分離とセキュリティー](/docs/containers?topic=containers-security#container)を参照して、この決定に関するセキュリティー情報を確認することもできます。

**複数のクラスター**: [グローバル・ロード・バランサー](/docs/containers?topic=containers-ha_clusters#multiple_clusters)をセットアップして、各クラスターに同じ構成 YAML ファイルをコピーして適用することで、これらのクラスター間でワークロードのバランスを取る必要があります。 したがって、複数のクラスターは一般的に管理が複雑になるものの、以下のような重要な目標を達成するために役立ちます。
*  ワークロードの分離を求めるセキュリティー・ポリシーに準拠する。
*  異なるバージョンの Kubernetes や Calico などの他のクラスター・ソフトウェアで、アプリがどのように実行されるかをテストする。
*  別の地域のアプリが含まれたクラスターを作成することで、その地域のユーザーにさらに高いパフォーマンスを提供する。
*  名前空間レベルでクラスター内でアクセスを制御するための複数の RBAC ポリシーをカスタマイズおよび管理する代わりに、クラスター・インスタンス・レベルでユーザー・アクセスを構成する。

**少数または単一のクラスター**: クラスター数が少ないほど、固定されたリソースのクラスターあたりのコストと運用労力を低減しやすくなります。 より多くのクラスターを作成する代わりに、使用するアプリとサービス・コンポーネントで使用できるコンピューティング・リソースのさまざまなマシン・タイプ用にワーカー・プールを追加できます。 アプリを開発する際は、そのアプリで使用されるリソースは同じゾーン内にあるか、そうでない場合はマルチゾーン内で緊密に接続されているため、待ち時間、帯域幅、または相関関係のある複数の障害に関して推測できます。 ただし、名前空間、リソース割り当て、およびラベルを使用してクラスターを編成することがさらに重要になります。

### どうすればクラスター内のリソースをセットアップできますか?
{: #env_resources}

<dl>
<dt>ワーカー・ノードの容量を考慮する</dt>
  <dd>ワーカー・ノードのパフォーマンスを最大化するには、以下のことを考慮してください。
  <ul><li><strong>コアの強度を維持する</strong>: 各マシンは特定数のコアを備えています。 アプリのワークロードに応じて、コアあたりのポッド数の上限 (10 など) を設定します。</li>
  <li><strong>ノードの過負荷を回避する</strong>: 同様に、1 つのノードに 100 を超えるポッドを格納できるからといって、そのようにすることは推奨されません。 アプリのワークロードに応じて、ノードあたりのポッド数の上限 (40 など) を設定します。</li>
  <li><strong>クラスターの帯域幅を使い切らない</strong>: スケーリングされる仮想マシン上のネットワーク帯域幅は約 1000 Mbps であることに留意してください。 1 つのクラスター内に何百ものワーカー・ノードが必要になる場合は、そのクラスターを複数のクラスターに分割して各クラスター内のノード数を減らすか、ベアメタル・ノードを注文してください。</li>
  <li><strong>サービスを整理する</strong>: デプロイ前に、ワークロードに必要なサービスの数を綿密に計画します。 ネットワーキングとポート転送の規則は Iptables に配置されます。 サービスの数がさらに多くなることが予想される場合は (5,000を超えるサービスなど)、クラスターを複数のクラスターに分割します。</li></ul></dd>
<dt>各種コンピューティング・リソースの組み合わせに対してさまざまなタイプのマシンをプロビジョンする</dt>
  <dd>誰もが選択することを好みます。 {{site.data.keyword.containerlong_notm}} では、集中型のワークロード向けのベアメタルや、迅速なスケーリングに対応できる仮想マシンを含む、[各種マシン・タイプを組み合わせて](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)デプロイできます。 ラベルや名前空間を使用して、マシンへのデプロイメントを編成します。 デプロイメントを作成するときには、適切な組み合わせのリソースを備えたマシンにのみ、アプリのポッドがデプロイされるように制限してください。 例えば、`md1c.28x512.4x4tb` のように大容量のローカル・ディスク・ストレージを備えたベアメタル・マシンにのみ、データベース・アプリケーションがデプロイされるように制限することをお勧めします。</dd>
<dt>クラスターを共有している複数のチームとプロジェクトがある場合は、複数の名前空間をセットアップする</dt>
  <dd><p>名前空間は、クラスター内のクラスターのようなものです。 これらは、[リソース割り当て ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/concepts/policy/resource-quotas/) と [デフォルト制限 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/tasks/administer-cluster/memory-default-namespace/) を使用してクラスターのリソースを配分するための手段となります。 新しい名前空間を作成するときには、アクセスを制御するために、必ず、適切な [RBAC ポリシー](/docs/containers?topic=containers-users#rbac)をセットアップしてください。 詳しくは、Kubernetes の資料内の [Share a cluster with namespaces ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/tasks/administer-cluster/namespaces/) を参照してください。</p>
  <p>クラスターが小規模で、ユーザーが数十人、かつリソースが類似している (同じソフトウェアのバージョン違いなど) 場合は、複数の名前空間は必要ない可能性があります。 代わりに、ラベルを使用することで対応できます。</p></dd>
<dt>リソース割り当てを設定して、クラスター内のユーザーがリソースの要求と制限を使用する必要があるようにします。</dt>
  <dd>サービスをクラスターにデプロイしてアプリを実行するために必要なリソースがすべてのチームに与えられるように、すべての名前空間に[リソース割り当て量](https://kubernetes.io/docs/concepts/policy/resource-quotas/) をセットアップする必要があります。 リソース割り当て量により、デプロイ可能な Kubernetes リソースの数、それらのリソースで消費できる CPU とメモリーの量など、名前空間のデプロイメント制約が決まります。 割り当て量を設定すると、ユーザーは、そのデプロイメントにリソース要求と制限を含める必要があります。</dd>
<dt>ラベルを使用して Kubernetes オブジェクトを編成する</dt>
  <dd><p>`pods` や `nodes` などの Kubernetes リソースを編成および選択するには、[Kubernetes ラベルを使用します ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/)。 デフォルトでは、{{site.data.keyword.containerlong_notm}} によっていくつかのラベル (`arch`、`os`、`region`、`zone`、`machine-type` など) が適用されます。</p>
  <p>ラベルのユース・ケースとしては、[ネットワーク・トラフィックをエッジ・ワーカー・ノードに制限](/docs/containers?topic=containers-edge) する場合、[アプリを GPU マシンにデプロイ](/docs/containers?topic=containers-app#gpu_app)する場合、特定のマシン・タイプや SDS 性能に適合するワーカー・ノード (ベアメタル・ワーカー・ノードなど) 上で [アプリのワークロードを実行するように制限 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/) する場合などが挙げられます。 任意のリソースに既に適用されているラベルを確認するには、<code>kubectl get</code> コマンドを <code>--show-labels</code> フラグ付きで使用します。 以下に例を示します。</p>
  <p><pre class="pre"><code>kubectl get node &lt;node_ID&gt; --show-labels</code></pre></p>
  ワーカー・ノードにラベルを適用するには、ラベルを使用して[ワーカー・プールを作成](/docs/containers?topic=containers-add_workers#add_pool)するか、または[既存のワーカー・プールを更新](/docs/containers?topic=containers-add_workers#worker_pool_labels)します。</dd>
</dl>




<br />


## リソースの可用性向上
{: #kube_ha}

完全にフェイルセーフなシステムはありませんが、{{site.data.keyword.containerlong_notm}} 内のアプリとサービスの高可用性を実現するための措置を講じることはできます。
{:shortdesc}

リソースの高可用性の実現について詳しくは、以下を参照してください。
* [潜在的な障害ポイントの低減](/docs/containers?topic=containers-ha#ha)。
* [複数ゾーン・クラスターの作成](/docs/containers?topic=containers-ha_clusters#ha_clusters)。
* 複数のゾーンにまたがってレプリカ・セットやポッドのアンチアフィニティーなどの機能を使用する[高可用性デプロイメントの計画](/docs/containers?topic=containers-app#highly_available_apps)。
* [クラウド・ベース・パブリック・レジストリー内のイメージに基づくコンテナーの実行](/docs/containers?topic=containers-images)。
* [データ・ストレージの計画](/docs/containers?topic=containers-storage_planning#persistent_storage_overview)。 特に複数ゾーン・クラスターについては、[{{site.data.keyword.cloudant_short_notm}}](/docs/services/Cloudant?topic=cloudant-getting-started#getting-started) や [{{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-about) などのクラウド・サービスの使用を検討してください。
* 複数ゾーン・クラスターについては、[ロード・バランサー・サービス](/docs/containers?topic=containers-loadbalancer#multi_zone_config)または Ingress [マルチゾーン・ロード・バランサー](/docs/containers?topic=containers-ingress#ingress)を有効にして、アプリをパブリックに公開してください。

<br />


## サービス・ディスカバリーのセットアップ
{: #service_discovery}

Kubernetes クラスター内の各ポッドには IP アドレスが割り当てられています。 ただし、アプリをクラスターにデプロイする際は、サービス・ディスカバリーやネットワーキングのためにポッドの IP アドレスを利用しないことをお勧めします。 ポッドは頻繁に動的に削除および置換されるからです。 代わりに、Kubernetes サービスを使用してください。このサービスはポッドのグループを表し、`cluster IP` と呼ばれる当サービスの仮想 IP アドレスを通じて安定したエントリー・ポイントを提供します。 詳しくは、[Services ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/concepts/services-networking/service/#discovering-services) に関する Kubernetes の資料を参照してください。
{:shortdesc}

### Kubernetes クラスターの DNS プロバイダーをカスタマイズできますか?
{: #services_dns}

サービスとポッドを作成すると、それらには DNS 名が割り当てられて、アプリのコンテナーで DNS サービスの IP を使用して DNS 名を解決できるようになります。 ポッドの DNS をカスタマイズして、ネーム・サーバー、検索、およびオブジェクト・リストのオプションを指定できます。 詳しくは、[クラスター DNS プロバイダーの構成](/docs/containers?topic=containers-cluster_dns#cluster_dns)を参照してください。
{: shortdesc}



### サービスが適切なデプロイメントに接続され、実行の準備が整っていることは、どうすれば確認できますか?
{: #services_connected}

ほとんどのサービスについては、そのサービスの `.yaml` ファイルにセレクターを追加することで、そのラベルによってアプリを実行するポッドにそのセレクターが適用されるようにします。 多くの場合は、アプリが最初に始動したときに、そのアプリによって要求を直ちに処理することは望ましくないでしょう。 デプロイメントに Readiness Probe を追加することで、準備ができていると見なされるポッドのみにトラフィックが送信されるようにします。 ラベルを使用するとともに Readiness Probe を設定するサービスを使用したデプロイメントの例については、この [NGINX YAML](https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/deploy-apps-clusters/nginx_preferredAntiAffinity.yaml) を確認してください。
{: shortdesc}

場合によっては、サービスでラベルを使用することが望ましくないことがあります。 例えば、外部データベースを使用している場合や、そのサービスでクラスター内の異なる名前空間内の別のサービスを参照することを希望する場合があります。 このような場合は、エンドポイント・オブジェクトを手動で追加して、このオブジェクトをそのサービスにリンクする必要があります。


### クラスター内で実行されているサービス間のネットワーク・トラフィックを制御するには、どうすればいいですか?
{: #services_network_traffic}

デフォルトでは、ポッドはクラスター内の他のポッドと通信できますが、ネットワーク・ポリシーに基づいて特定のポッドや名前空間へのトラフィックをブロックできます。 さらに、NodePort、ロード・バランサー、または Ingress サービスを使用してアプリを外部に公開する場合は、高度なネットワーク・ポリシーをセットアップしてトラフィックをブロックすることをお勧めします。 {{site.data.keyword.containerlong_notm}} では、Calico を使用して、Kubernetes と Calico の[ネットワーク・ポリシーを管理してトラフィックを制御](/docs/containers?topic=containers-network_policies#network_policies)できます。

ネットワーク・トラフィックを接続、管理、および保護する必要のある複数のプラットフォームにまたがって実行されるさまざまなマイクロサービスを使用している場合は、[管理対象 Istio アドオン](/docs/containers?topic=containers-istio)などのサービス・メッシュ・ツールを使用することを検討してください。

[エッジ・ノードをセットアップ](/docs/containers?topic=containers-edge#edge)して、ネットワーキングのワークロードを特定のワーカー・ノードに制限することで、クラスターのセキュリティーと分離を強化することもできます。



### どうすればサービスをインターネットで公開できますか?
{: #services_expose_apps}

NodePort、LoadBalancer、および Ingress という 3 タイプのサービスを外部ネットワーキング用に作成できます。 詳しくは、[ネットワーク・サービスの計画](/docs/containers?topic=containers-cs_network_planning#external)を参照してください。

クラスター内で必要な `Service` オブジェクトの数を計画する際は、Kubernetes では `iptables` を使用してネットワーキングとポート転送の規則が処理されることに留意してください。 多数のサービス (5000 など) をクラスター内で実行する場合は、パフォーマンスが低下する可能性があります。



## クラスターへのアプリ・ワークロードのデプロイ
{: #deployments}

Kubernetes では、ポッド、デプロイメント、ジョブなどの多くのタイプのオブジェクトを YAML 構成ファイルで宣言します。 これらのオブジェクトで記述される内容としては、実行されているコンテナー化アプリ、これらのアプリで使用されるリソース、アプリの再始動、更新、複製などの動作を管理しているポリシーなどが挙げられます。 詳しくは、[Configuration best practices ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/concepts/configuration/overview/) に関する Kubernetes の資料を参照してください。
{: shortdesc}

### アプリをコンテナーに配置する必要があると思っていました。 ポッドとは何ですか?
{: #deploy_pods}

[ポッド ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/concepts/workloads/pods/pod/) は、Kubernetes で管理できる最小のデプロイ可能単位です。 コンテナー (またはコンテナーのグループ) をポッドに格納して、そのポッドの構成ファイルを使用して、そのコンテナーを実行する方法および他のポッドとリソースを共有する方法をそのポッドに通知します。 ポッドに配置したすべてのコンテナーは共有コンテキスト内で実行されるため、同じ仮想マシンまたは物理マシンを共有することになります。
{: shortdesc}

**コンテナーに格納するもの**: 使用するアプリケーションのコンポーネントについて検討するときには、CPU やメモリーなどのリソース要件がそれらのコンポーネントの間で大きく異なるかどうかを考慮してください。 例えば、コンポーネントによっては、リソースを他の領域に転用するために一時的に稼働停止することが許容され、ベスト・エフォート方式で実行できる場合があります。 別のコンポーネントは、顧客に対応するものであるため、常時稼働させることが非常に重要である可能性があります。 これらは、別々のコンテナーに分けてください。 いつでも、これらを同じポッドにデプロイすることで、同期的に一緒に実行されるようにすることができます。

**ポッドに格納するもの**: 同じアプリの複数のコンテナーが常に同じポッドに格納される必要はありません。 ステートフルでありスケーリングが困難なコンポーネント (データベース・サービスなど) がある場合は、そのコンポーネントを異なるポッドに格納して、そのワークロードを処理するための十分なリソースを備えたワーカー・ノード上でそのポッドをスケジュールしてください。 コンテナーが正常に動作し、別々のワーカー・ノード上で実行される場合は、複数のポッドを使用してください。 同じマシンに配置して一緒にスケーリングする必要があるコンテナーは、まとめて同じポッドに格納します。

### 単にポッドを使用できるなら、なぜ、さまざまなタイプのオブジェクトが必要になるのですか?
{: #deploy_objects}

ポッドの YAML ファイルは簡単に作成できます。 次のように数行で記述できます。

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx
    ports:
    - containerPort: 80
```
{: codeblock}

ここからさらに設定を進めます。 ポッドが実行されているノードがダウンした場合は、そのポッドも一緒にダウンするため、そのポッドがスケジュール変更されることはありません。 代わりに、[デプロイメント ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) を使用して、ポッドのスケジュール変更、レプリカ・セット、およびローリング更新をサポートしてください。 基本的なデプロイメントは、ポッドと同じくらい簡単に作成できます。 ただし、`spec` 内で単独でコンテナーを定義する代わりに、デプロイメントの `spec` 内で `replicas` と `template` を指定します。 テンプレートには、次の例のようにコンテナー用の独自の `spec` が含まれています。

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 80
```
{: codeblock}

ポッドのアンチアフィニティーやリソース制限などの機能を、同じ YAML ファイルにさらに追加できます。

デプロイメントに追加できる各種の機能について詳しくは、[アプリ・デプロイメント YAML ファイルの作成](/docs/containers?topic=containers-app#app_yaml)を参照してください。
{: tip}

### どうすれば更新と管理がしやすいようにデプロイメントを編成できますか?
{: #deploy_organize}

デプロイメントに含めるものを把握したところで、これらすべての異なる YAML ファイルを管理する方法についての疑問が生じるかもしれません。 当然ながら、Kubernetes 環境内でデプロイメントによって作成されるオブジェクトについても同様でしょう。

以下では、デプロイメントの YAML ファイルを編成するためのいくつかのヒントを示しています。
*  Git などのバージョン管理システムを使用します。
*  相互に密接に関連する Kubernetes オブジェクトを同じ YAML ファイルにまとめます。 例えば、`deployment` を作成する場合は、`service` ファイルもその YAML ファイルに追加するとよいでしょう。 次の例のように、`---` によってオブジェクトを区切ってください。
    ```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
    ...
    ---
    apiVersion: v1
    kind: Service
    metadata:
    ...
    ```
    {: codeblock}
*  `kubectl apply -f` コマンドを使用して、単一ファイルだけでなくディレクトリー全体に適用できます。
*  Kubernetes リソースの YAML 構成の記述、カスタマイズ、および再利用に役立つよう使用可能な [`kustomize` プロジェクト](/docs/containers?topic=containers-app#kustomize)を試してみます。

YAML ファイル内では、デプロイメントを管理するためのメタデータとしてラベルやアノテーションを使用できます。

**ラベル**: [ラベル ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/) は、ポッドやデプロイメントなどの Kubernetes オブジェクトに添付できる `key:value` ペアとして表現されます。 ラベルには任意の値を使用でき、ラベル情報に基づいてオブジェクトを簡単に選択できます。 ラベルは、オブジェクトをグループ分けするための基盤となります。 以下にラベルの例を示します。
* `app: nginx
`
* `version: v1`
* `env: dev`

**アノテーション**: [アノテーション ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/concepts/overview/working-with-objects/annotations/) もラベルと同様に `key:value` ペアとして表現されます。 アノテーションは、各種のツールやライブラリーで活用できる、非識別情報に適しています。例えば、オブジェクトの取得元に関する追加情報、そのオブジェクトの使用方法、関連する追跡リポジトリーへのポインター、そのオブジェクトに関するポリシーなどを保持します。 アノテーションに基づいてオブジェクトを選択することはありません。

### アプリのデプロイメントの準備のために、他に何ができますか?
{: #deploy_prep}

多くのことがあります。 [コンテナー化アプリをクラスター内で実行するための準備](/docs/containers?topic=containers-app#plan_apps)を参照してください。 このトピックには、次のことに関する情報が含まれています。
*  Kubernetes で実行できるアプリの種類、およびステートフル・アプリとステートレス・アプリに関するヒント。
*  Kubernetes へのアプリのマイグレーション。
*  ワークロード要件に基づいたクラスターのサイズ設定。
*  追加のアプリ・リソースのセットアップ (IBM サービス、ストレージ、ロギング、モニタリングなど)。
*  デプロイメント内での変数の使用。
*  アプリへのアクセスの制御。

<br />


## アプリケーションのパッケージ化
{: #packaging}

複数のクラスター、パブリック環境とプライベート環境、または複数のクラウド・プロバイダーでアプリを実行する場合は、これらの環境を横断してデプロイメント戦略をどのように成功させるかが課題となります。 {{site.data.keyword.Bluemix_notm}} および他のオープン・ソース・ツールを使用すると、アプリケーションをパッケージ化してデプロイメントの自動化に役立てることができます。
{: shortdesc}

<dl>
<dt>インフラストラクチャーの自動化</dt>
  <dd>オープン・ソースの [Terraform](/docs/terraform?topic=terraform-getting-started#getting-started) ツールを使用して、Kubernetes クラスターを含めた {{site.data.keyword.Bluemix_notm}} インフラストラクチャーのプロビジョニングを自動化できます。 こちらのチュートリアルに従って、[デプロイメント環境を計画、作成、および更新](/docs/tutorials?topic=solution-tutorials-plan-create-update-deployments#plan-create-update-deployments)してください。 クラスターを作成した後に、[{{site.data.keyword.containerlong_notm}} クラスター自動スケーリング機能](/docs/containers?topic=containers-ca)をセットアップすることで、ワークロードのリソース要求に応じて、ワーカー・プールによってワーカー・ノードがスケールアップおよびスケールダウンされるように設定できます。</dd>
<dt>継続的な統合とデリバリー (CI/CD) パイプラインのセットアップ</dt>
  <dd>アプリの構成ファイルが Git などのソース制御管理システムで編成されている場合は、パイプラインを構築することで、コードをテストして、`test` や `prod` などの別々の環境にデプロイできます。 [Kubernetes への継続的なデプロイメントに関するこちらのチュートリアル](/docs/tutorials?topic=solution-tutorials-continuous-deployment-to-kubernetes#continuous-deployment-to-kubernetes)をご覧ください。</dd>
<dt>アプリ構成ファイルのパッケージ化</dt>
  <dd>Kubernetes パッケージ・マネージャーである [Helm ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://helm.sh/docs/) を使用すると、アプリが必要とするすべての Kubernetes リソースを Helm チャート内で指定できます。 次に、Helm を使用して YAML 構成ファイルを作成して、これらのファイルをクラスターにデプロイできます。 [{{site.data.keyword.Bluemix_notm}} 側で提供されている Helm チャートを統合 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/kubernetes/solutions/helm-charts) することで、Block Storage プラグインなどを使用してクラスターの機能を拡張することもできます。<p class="tip">YAML ファイルのテンプレートを簡単に作成するには、 Helm を使用するか、[`ytt` ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://get-ytt.io/) などの他のコミュニティー・ツールをお試しください。</p></dd>
</dl>

<br />


## アプリの最新状態の維持
{: #updating}

新バージョンのアプリに向けて準備するために、多大な労力が費やされます。 {{site.data.keyword.Bluemix_notm}} と Kubernetes の更新ツールを使用すると、保護されたクラスター環境でアプリが実行されることが保証されると同時に、アプリのさまざまなバージョンをロールアウトできます。
{: shortdesc}

### どうすればクラスターをサポート対象の状態に維持できますか?
{: #updating_kube}

[サポートされている Kubernetes バージョン](/docs/containers?topic=containers-cs_versions#cs_versions)がクラスターで常に実行されるようにします。 新しい Kubernetes マイナー・バージョンがリリースされると、すぐに旧バージョンは非推奨になり、その後にサポート対象外になります。 詳しくは、[Kubernetes マスターの更新](/docs/containers?topic=containers-update#master)および[ワーカー・ノード](/docs/containers?topic=containers-update#worker_node)を参照してください。

### アプリの更新戦略としては、どのようなものを使用できますか?
{: #updating_apps}

アプリを更新するには、以下のようなさまざまな戦略から選択できます。 最初はローリング・デプロイメントや即時切り替えから始めて、その後で複雑なカナリア・デプロイメントに進むことができます。

<dl>
<dt>ローリング・デプロイメント</dt>
  <dd>Kubernetes 固有の機能を使用して、`v2` デプロイメントを作成したり、以前の `v1` デプロイメントを段階的に置き換えたりできます。 この手法を使用する場合は、`v2` アプリ・バージョンのユーザーに対して破壊的変更が発生しないように、アプリが後方互換性を備えている必要があります。 詳しくは、[アプリを更新するためのローリング・デプロイメントの管理](/docs/containers?topic=containers-app#app_rolling)を参照してください。</dd>
<dt>即時切り替え</dt>
  <dd>即時切り替え (ブルー/グリーン・デプロイメントとも呼ばれます) では、同じアプリの 2 つのバージョンを同時に実行するために通常の 2 倍のコンピュート・リソースが必要になります。 この手法を使用すると、ほぼリアルタイムでユーザーを新しいバージョンに切り替えることができます。 必ずサービス・ラベル・セレクター (`version: green` や `version: blue` など) を使用して、要求が確実に適切なアプリ・バージョンに送信されるようにしてください。 新しい `version: green` デプロイメントを作成して、このデプロイメントの準備が完了するまで待ってから、`version: blue` デプロイメントを削除できます。 または、[ローリング更新](/docs/containers?topic=containers-app#app_rolling)を実行できますが、その場合は `maxUnavailable` パラメーターを `0%` に設定して、`maxSurge` パラメーターを `100%` に設定します。</dd>
<dt>カナリア・デプロイメントまたは A/B デプロイメント</dt>
  <dd>さらに複雑な更新戦略であるカナリア・デプロイメントでは、指定した比率 (5% など) のユーザーのみが新しいアプリ・バージョンを利用できるようにします。 新しいアプリ・バージョンのパフォーマンスに関するメトリックをロギング・ツールとモニタリング・ツールで収集し、A/B テストを実行してから、より多くのユーザーに更新をロールアウトします。 他のデプロイメント方式と同様に、アプリにラベル (`version: stable` や `version: canary` など) を割り当てることが非常に重要です。 カナリア・デプロイメントを管理するには、[管理対象 Istio アドオン・サービス・メッシュをインストール](/docs/containers?topic=containers-istio#istio)し、[クラスターに対して Sysdig モニタリングをセットアップ](/docs/services/Monitoring-with-Sysdig/tutorials?topic=Sysdig-kubernetes_cluster#kubernetes_cluster)してから、[こちらのブログ投稿 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://sysdig.com/blog/monitor-istio/) の説明に従って Istio サービス・メッシュを A/B テスト用に使用してください。 または、カナリア・デプロイメントの場合には Knative を使用します。</dd>
</dl>

<br />


## クラスター・パフォーマンスのモニタリング
{: #monitoring_health}

クラスターとアプリの効果的なロギングとモニタリングを通じて、現在の環境に対する理解を深めることで、リソースの使用状況を最適化して、起こり得る問題をトラブルシューティングできます。 クラスターに対するロギングとモニタリングのソリューションをセットアップするには、[ロギングとモニタリング](/docs/containers?topic=containers-health#health)を参照してください。
{: shortdesc}

ロギングとモニタリングをセットアップする際に、以下のことを考慮してください。

<dl>
<dt>ログとメトリックを収集してクラスターの正常性を確認する</dt>
  <dd>Kubernetes に含まれている Metrics Server は、基本的なクラスター・レベルのパフォーマンスの判定に役立ちます。 これらのメトリックを確認するには、[Kubernetes ダッシュボード](/docs/containers?topic=containers-app#cli_dashboard)を参照するか、端末で `kubectl top (pods | nodes)` コマンドを実行します。 これらのコマンドを自動化処理に含めることができます。<br><br>
  ログ解析ツールにログを転送して、後でログを解析できるようにします。 記録するログの冗長性とレベルを定義することで、必要以上の量のログが保管されることを防止します。 ログは短期間で大量のストレージを消費する可能性があり、その結果として、アプリのパフォーマンスが低下したり、ログの解析が難しくなったりする可能性があります。</dd>
<dt>アプリのパフォーマンスをテストする</dt>
  <dd>ロギングとモニタリングをセットアップした後に、パフォーマンス・テストを実行してください。 テスト環境で、さまざまな非理想的なシナリオを意図的に作成します。例えば、ゾーン内のすべてのワーカー・ノードを削除することでゾーン障害を再現します。 ログとメトリックを参照して、アプリがどのように復旧するかを確認します。</dd>
<dt>監査に向けて準備する</dt>
  <dd>アプリのログおよびクラスターのメトリックに加えて、アクティビティー追跡をセットアップして、誰がどのようなクラスター操作や Kubernetes 操作を実行したかに関する監査可能な記録を収集することをお勧めします。 詳しくは、[{{site.data.keyword.cloudaccesstrailshort}}](/docs/containers?topic=containers-at_events#at_events)を参照してください。</dd>
</dl>
