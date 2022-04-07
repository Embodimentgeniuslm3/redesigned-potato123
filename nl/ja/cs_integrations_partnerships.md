---

copyright:
  years: 2014, 2019
lastupdated: "2019-05-31"

keywords: kubernetes, iks, helm

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


# IBM Cloud Kubernetes Service パートナー
{: #service-partners}

IBM は、{{site.data.keyword.containerlong_notm}} を Kubernetes ワークロードのマイグレーション、操作、および管理に役立つ最良の Kubernetes サービスにすることを目的としています。 クラウドで実動ワークロードを実行するのに必要なすべての機能を提供するために、{{site.data.keyword.containerlong_notm}} パートナーは、その他のサード・パーティー・サービス・プロバイダーと連携し、最高のロギング、モニタリング、およびストレージの各ツールでクラスターを拡張します。
{: shortdesc}

パートナーおよび提供される各ソリューションの利点を確認します。 クラスターで使用可能な他の独自開発の {{site.data.keyword.Bluemix_notm}} およびサード・パーティーのオープン・ソース・サービスを見つけるには、[{{site.data.keyword.Bluemix_notm}} とサード・パーティーの統合について](/docs/containers?topic=containers-ibm-3rd-party-integrations)を参照してください。

## LogDNA
{: #logdna-partner}

{{site.data.keyword.la_full_notm}} は、[LogDNA ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://logdna.com/) をインテリジェント・ロギング機能をクラスターおよびアプリに追加するために使用可能なサード・パーティー・サービスとして提供しています。

### 利点
{: #logdna-benefits}

LogDNA を使用して得られる主な利点のリストについては、以下の表を参照してください。
{: shortdesc}

|利点|説明|
|-------------|------------------------------|
|集中ログ管理およびログ分析|クラスターをログ・ソースとして構成すると、LogDNA では、ワーカー・ノード、ポッド、アプリ、およびネットワーク用のロギング情報の収集が自動的に開始されます。 ログは、LogDNA によって自動的に解析、索引付け、タグ付け、および集約され、LogDNA ダッシュボードで視覚化されるため、クラスター・リソースを簡単に確認できます。 組み込みのグラフ化ツールを使用して、最も一般的なエラー・コードまたはログ・エントリーを視覚化できます。 |
|Google のような検索構文を使用した簡単な検索機能|LogDNA では、標準の用語 (`AND` 演算と `OR` 演算) をサポートする Google のような検索構文を使用することで、検索語を除外したり組み合わせたりすることができ、より簡単にログを検索するのに役立ちます。 ログのスマートな索引付けを使用すると、特定のログ・エントリーにいつでもジャンプできます。 |
|転送中および保存時の暗号化|LogDNA では、ログを自動的に暗号化して、転送中および保存時にログを保護します。 |
|カスタム・アラートおよびログ・ビュー|ダッシュボードを使用して、検索基準に一致するログを検索し、これらのログをビューに保存して、このビューを他のユーザーと共有し、チーム・メンバー間のデバッグを簡素化できます。 このビューを使用すると、PagerDuty、Slack、E メールなどのダウンストリーム・システムに送信できるアラートを作成することもできます。   |
|すぐに使用可能なカスタム・ダッシュボード|さまざまな既存のダッシュボードから選択することも、独自のダッシュボードを作成して、必要な方法でログを視覚化することもできます。 |

### {{site.data.keyword.containerlong_notm}} との統合
{: #logdna-integration}

LogDNA は、{{site.data.keyword.la_full_notm}} (クラスターで使用可能な {{site.data.keyword.Bluemix_notm}} プラットフォーム・サービス) によって提供されます。 {{site.data.keyword.la_full_notm}} は、IBM とのパートナーシップにより LogDNA によって運用されます。
{: shortdesc}

クラスターで LogDNA を使用するには、{{site.data.keyword.Bluemix_notm}} アカウントに {{site.data.keyword.la_full_notm}} のインスタンスをプロビジョンし、ログ・ソースとして Kubernetes クラスターを構成する必要があります。 クラスターが構成されると、ログが自動的に収集され、{{site.data.keyword.la_full_notm}} サービス・インスタンスに転送されます。 {{site.data.keyword.la_full_notm}} ダッシュボードを使用して、ログにアクセスできます。   

詳しくは、[{{site.data.keyword.la_full_notm}} による Kubernetes クラスター・ログの管理](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-kube)を参照してください。

### 課金とサポート
{: #logdna-billing-support}

{{site.data.keyword.la_full_notm}} は、{{site.data.keyword.Bluemix_notm}} サポート・システムに完全に統合されています。 {{site.data.keyword.la_full_notm}} の使用に関する問題が発生した場合は、[{{site.data.keyword.containerlong_notm}} Slack ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://ibm-container-service.slack.com/) で `logdna-on-iks` チャネルに質問を投稿するか、または [{{site.data.keyword.Bluemix_notm}} サポート・ケース](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support)を開きます。 IBMid を使用して Slack にログインします。 自身の {{site.data.keyword.Bluemix_notm}} アカウントで IBMid を使用していない場合は、[この Slack への招待を要請してください ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://bxcs-slack-invite.mybluemix.net/)。

## Sysdig
{: #sydig-partner}

{{site.data.keyword.mon_full_notm}} では、[Sysdig Monitor ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://sysdig.com/products/monitor/) が、サード・パーティーのクラウド・ネイティブ・コンテナー分析システムとして提供されています。このモニターを使用すると、コンピュート・ホスト、アプリ、コンテナー、およびネットワークのパフォーマンスと正常性を把握できます。
{: shortdesc}

### 利点
{: #sydig-benefits}

Sysdig を使用して得られる主な利点のリストについては、以下の表を参照してください。
{: shortdesc}

|利点|説明|
|-------------|------------------------------|
|クラウド・ネイティブ・メトリックおよび Prometheus カスタム・メトリックへの自動アクセス|事前定義されたさまざまなクラウド・ネイティブ・メトリックおよび Prometheus カスタム・メトリックから選択して、コンピュート・ホスト、アプリ、コンテナー、およびネットワークのパフォーマンスと正常性を把握します。|
|拡張フィルターによるトラブルシューティング|Sysdig Monitor により、ワーカー・ノードがどのように接続されているか、および Kubernetes サービスが相互にどのように通信するかを示すネットワーク・トポロジーが作成されます。 ワーカー・ノードからコンテナーおよび単一のシステム呼び出しにナビゲートし、各リソースのこれまでの重要なメトリックをグループ化して表示することができます。 例えば、これらのメトリックを使用して、ほとんどの要求を受信するサービス、または照会と応答時間が遅いサービスを検出します。 このデータを、Kubernetes イベント、カスタム CI/CD イベント、またはコード・コミットと組み合わせることができます。
|自動異常検出およびカスタム・アラート|クラスターまたはグループのリソース内で異常を検出したという通知を受け取る必要があるタイミングのルールとしきい値を定義して、いずれかのリソースの動作が残りのリソースと異なる場合に Sysdig から通知を受け取るようにします。 これらのアラートは、ServiceNow、PagerDuty、Slack、VictorOps、または E メールなどのダウンストリーム・ツールに送信できます。|
|すぐに使用可能なカスタム・ダッシュボード|さまざまな既存のダッシュボードから選択することも、独自のダッシュボードを作成して、必要な方法でマイクロサービスのメトリックを視覚化することもできます。 |
{: caption="Sysdig Monitor を使用する利点" caption-side="top"}

### {{site.data.keyword.containerlong_notm}} との統合
{: #sysdig-integration}

Sysdig Monitor は、{{site.data.keyword.mon_full_notm}} (クラスターで使用可能な {{site.data.keyword.Bluemix_notm}} プラットフォーム・サービス) によって提供されます。 {{site.data.keyword.mon_full_notm}} は、IBM とのパートナーシップにより Sysdig によって運用されます。
{: shortdesc}

クラスターで Sysdig Monitor を使用するには、{{site.data.keyword.Bluemix_notm}} アカウントに {{site.data.keyword.mon_full_notm}} のインスタンスをプロビジョンし、メトリック・ソースとして Kubernetes クラスターを構成する必要があります。 クラスターが構成されると、メトリックが自動的に収集され、{{site.data.keyword.mon_full_notm}} サービス・インスタンスに転送されます。 {{site.data.keyword.mon_full_notm}} ダッシュボードを使用して、メトリックにアクセスできます。   

詳しくは、[Kubernetes クラスターにデプロイされたアプリのメトリックの分析方法](/docs/services/Monitoring-with-Sysdig?topic=Sysdig-kubernetes_cluster)を参照してください。

### 課金とサポート
{: #sysdig-billing-support}

Sysdig Monitor は {{site.data.keyword.mon_full_notm}} によって提供されるため、その使用は、プラットフォーム・サービスの {{site.data.keyword.Bluemix_notm}} の課金に含まれます。 価格情報については、[{{site.data.keyword.Bluemix_notm}} カタログ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/observe/monitoring/create) の使用可能なプランを参照してください。

{{site.data.keyword.mon_full_notm}} は、{{site.data.keyword.Bluemix_notm}} サポート・システムに完全に統合されています。 {{site.data.keyword.mon_full_notm}} の使用に関する問題が発生した場合は、[{{site.data.keyword.containerlong_notm}} Slack ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://ibm-container-service.slack.com/) で `sysdig-monitoring` チャネルに質問を投稿するか、または [{{site.data.keyword.Bluemix_notm}} サポート・ケース](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support)を開きます。 IBMid を使用して Slack にログインします。 自身の {{site.data.keyword.Bluemix_notm}} アカウントで IBMid を使用していない場合は、[この Slack への招待を要請してください ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://bxcs-slack-invite.mybluemix.net/)。

## Portworx
{: #portworx-parter}

[Portworx ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://portworx.com/products/introduction/) は、可用性の高いソフトウェア定義のストレージ・ソリューションです。これを使用して、コンテナー化データベースなどのステートフル・アプリ用のローカル永続ストレージを管理したり、複数のゾーンにわたってポッド間でデータを共有したりすることができます。
{: shortdesc}

**ソフトウェア定義ストレージ (SDS) とは何ですか?** </br>
SDS ソリューションは、クラスター内のワーカー・ノードに接続されたさまざまなタイプ、サイズ、またはベンダーのストレージ・デバイスを抽象化します。 ハード・ディスク上に使用可能なストレージを持つワーカー・ノードは、ノードとしてストレージ・クラスターに追加されます。 このクラスター内では、物理ストレージは仮想化されて、仮想ストレージ・プールとしてユーザーに表示されます。 このストレージ・クラスターは SDS ソフトウェアによって管理されます。 このストレージ・クラスターにデータを保管する必要がある場合は、SDS ソフトウェアによって最も高い可用性を確保できるデータ保管場所が決定されます。 仮想ストレージには、実際の基盤ストレージ・アーキテクチャーについて意識することなく利用できる一連の共通の機能とサービスが付属しています。

### 利点
{: #portworx-benefits}

Portworx を使用して得られる主な利点のリストについては、以下の表を参照してください。
{: shortdesc}

|利点|説明|
|-------------|------------------------------|
|ステートフル・アプリのクラウド・ネイティブ・ストレージおよびデータ管理|Portworx は、ワーカー・ノードに接続されたさまざまなサイズやタイプの使用可能なローカル・ストレージを集約して、クラスター内で実行するコンテナー化されたデータベースなどのステートフル・アプリ用の統一された永続ストレージ層を作成します。 Kubernetes 永続ボリューム請求 (PVC) を使用することで、データを保管するためのローカル永続ストレージをアプリに追加できます。|
|ボリューム複製による可用性の高いデータ|Portworx により、クラスター内のワーカー・ノードとゾーン間でボリューム内のデータが自動的に複製されるため、データにいつでもアクセスすることができ、ワーカー・ノードで障害またはリブートが発生した場合にはステートフル・アプリを別のワーカー・ノードにスケジュール変更できます。 |
|`hyper-converged` の実行のサポート|Portworx は、[`hyper-converged` ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.portworx.com/portworx-install-with-kubernetes/storage-operations/hyperconvergence/) を実行して、コンピュート・リソースとストレージが常に同じワーカー・ノード上に配置されるように構成できます。 アプリのスケジュールを変更する必要がある場合は、Portworx は、いずれかのボリューム・レプリカが配置されているワーカー・ノードにそのアプリを移動することで、ステートフル・アプリでローカル・ディスク並のアクセス速度とハイパフォーマンスを実現します。 |
|{{site.data.keyword.keymanagementservicelong_notm}} を使用したデータの暗号化|ボリューム内のデータを保護するために、FIPS 140-2 レベル 2 認証済みのクラウド・ベースのハードウェア・セキュリティー・モジュール (HSM) によって保護された [{{site.data.keyword.keymanagementservicelong_notm}}暗号鍵をセットアップ](/docs/containers?topic=containers-portworx#encrypt_volumes)できます。 クラスター内のすべてのボリュームを同じ暗号鍵を使用して暗号化するのか、ボリュームごとに異なる暗号鍵を使用するのかを選択できます。 Portworx はこの鍵を使用して、保存中のデータを暗号化するとともに、異なるワーカー・ノードに転送中のデータを暗号化します。|
|組み込みのスナップショットおよびクラウド・バックアップ|[Portworx スナップショット ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.portworx.com/portworx-install-with-kubernetes/storage-operations/create-snapshots/) を作成することで、ボリュームおよびそのデータの現在の状態を保存できます。 スナップショットは、ローカル Portworx クラスターまたはクラウドに保管できます。|
|Lighthouse と統合されたモニタリング|[Lighthouse ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.portworx.com/reference/lighthouse/) は、Portworx クラスターとボリューム・スナップショットを管理およびモニターするのに役立つ直感的なグラフィカル・ツールです。 Lighthouse を使用すると、Portworx クラスターの正常性 (使用可能なストレージ・ノードの数、ボリューム、使用可能な容量など) を表示できるとともに、Prometheus、Grafana、または Kibana でデータを分析できます。|
{: caption="Portworx を使用する利点" caption-side="top"}

### {{site.data.keyword.containerlong_notm}} との統合
{: #portworx-integration}

{{site.data.keyword.containerlong_notm}} で提供されるワーカー・ノード・フレーバーには、SDS の使用向けに最適化されているものと、データ保管用に使用できる 1 台以上の未フォーマットおよび未マウントのロー・ローカル・ディスクを備えているものがあります。 Portworx が最高のパフォーマンスを発揮するのは、10 Gbps のネットワーク速度に対応した [SDS ワーカー・ノード・マシン](/docs/containers?topic=containers-planning_worker_nodes#sds)を使用している場合です。 ただし、Portworx を非 SDS ワーカー・ノード・フレーバーにインストールできますが、ご使用のアプリで要求されるパフォーマンス上の利点は得られない可能性があります。
{: shortdesc}

Portworx は、[Helm チャート](/docs/containers?topic=containers-portworx#install_portworx)を使用してインストールされます。 Helm チャートをインストールすると、Portworx により、クラスター内で使用可能なローカル永続ストレージが自動的に分析され、ストレージが Portworx ストレージ層に追加されます。 Portworx ストレージ層からアプリにストレージを追加するには、[Kubernetes 永続ボリューム請求](/docs/containers?topic=containers-portworx#add_portworx_storage)を使用する必要があります。

Portworx をクラスターにインストールするには、Portworx ライセンスが必要です。 初めて使用する場合は、`px-enterprise` エディションを評価版として使用できます。 この評価版では、Portworx の全機能を 30 日間にわたって試用できます。 評価版の有効期限が切れた後も Portworx クラスターの使用を継続するには、[Portworx ライセンスを購入 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/containers?topic=containers-portworx#portworx_license) する必要があります。

Portworx を {{site.data.keyword.containerlong_notm}} でインストールおよび使用する方法について詳しくは、[Portworx を使用したソフトウェア定義ストレージ (SDS) へのデータの保管](/docs/containers?topic=containers-portworx)を参照してください。

### 課金とサポート
{: #portworx-billing-support}

ローカル・ディスクに付属する SDS ワーカー・ノード・マシン、および Portworx 用に使用する仮想マシンは、月単位の {{site.data.keyword.containerlong_notm}} 課金に含まれます。価格情報については、[{{site.data.keyword.Bluemix_notm}} カタログ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/kubernetes/catalog/cluster) を参照してください。 Portworx ライセンスは個別のコストであり、月単位の課金には含まれません。
{: shortdesc}

Portworx の使用に関する問題が発生した場合や、特定のユース・ケースでの Portworx 構成についてチャットを希望する場合は、[{{site.data.keyword.containerlong_notm}} Slack ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://ibm-container-service.slack.com/) で `portworx-on-iks` チャネルに質問を投稿してください。 IBMid を使用して Slack にログインします。 自身の {{site.data.keyword.Bluemix_notm}} アカウントで IBMid を使用していない場合は、[この Slack への招待を要請してください ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://bxcs-slack-invite.mybluemix.net/)。
