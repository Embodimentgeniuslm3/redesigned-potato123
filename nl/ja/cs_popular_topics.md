---

copyright:
  years: 2014, 2019
lastupdated: "2019-04-15"

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



# よく閲覧された {{site.data.keyword.containerlong_notm}} のトピック
{: #cs_popular_topics}

{{site.data.keyword.containerlong}} の最新情報を確認しましょう。 知っておくべき新機能、試してみたい便利な方法、他の開発者が役に立つと感じている、よく閲覧されたトピックをご覧ください。
{:shortdesc}



## 2019 年 4 月によく閲覧されたトピック
{: #apr19}

<table summary="この表には、よく閲覧されたトピックを示しています。行は左から右に読みます。1 列目は日付、2 列目は機能のタイトル、3 列目は説明です。">
<caption>2019 年 4 月によく閲覧されたコンテナーと Kubernetes クラスターについてのトピック</caption>
<thead>
<th>日</th>
<th>タイトル</th>
<th>説明</th>
</thead>
<tbody>
<tr>
<td>2019 年 4 月 15 日</td>
<td>[ネットワーク・ロード・バランサー (NLB) ホスト名の登録](/docs/containers?topic=containers-loadbalancer#loadbalancer_hostname)</td>
<td>インターネットにアプリを公開するパブリック・ネットワーク・ロード・バランサー (NLB) をセットアップした後に、ホスト名を作成して、NLB IP の DNS エントリーを作成できます。 {{site.data.keyword.Bluemix_notm}} によって、ホスト名のワイルドカード SSL 証明書の生成と保守が行われます。 また、TCP/HTTP(S) モニターをセットアップして、各ホスト名の背後にある NLB IP アドレスのヘルス・チェックを行うこともできます。</td>
</tr>
<tr>
<td>2019 年 4 月 8 日</td>
<td>[Web ブラウザーでの Kubernetes Terminal (ベータ版)](/docs/containers?topic=containers-cs_cli_install#cli_web)</td>
<td>{{site.data.keyword.Bluemix_notm}} コンソールでクラスター・ダッシュボードを使用してクラスターを管理する場合、より高度な構成変更を迅速に行うために、Kubernetes Terminal の Web ブラウザーから直接 CLI コマンドを実行できます。クラスターの詳細ページで**「Terminal」**ボタンをクリックして、Kubernetes Terminal を起動します。 Kubernetes Terminal はベータ版のアドオンとしてリリースされており、実動クラスターでの使用は想定されていません。</td>
</tr>
<tr>
<td>2019 年 4 月 4 日</td>
<td>[高可用性マスターがシドニーに](/docs/containers?topic=containers-regions-and-zones#zones)</td>
<td>シドニーを含む、複数ゾーンの大都市ロケーションに[クラスターを作成した](/docs/containers?topic=containers-clusters#clusters_ui)場合は、Kubernetes 高可用性を実現するために、マスターのレプリカがゾーン間に分散されます。</td>
</tr>
</tbody></table>

## 2019 年 3 月によく閲覧されたトピック
{: #mar19}

<table summary="この表には、よく閲覧されたトピックを示しています。行は左から右に読みます。1 列目は日付、2 列目は機能のタイトル、3 列目は説明です。">
<caption>2019 年 3 月によく閲覧されたコンテナーと Kubernetes クラスターについてのトピック</caption>
<thead>
<th>日</th>
<th>タイトル</th>
<th>説明</th>
</thead>
<tbody>
<tr>
<td>2019 年 3 月 21 日</td>
<td>Kubernetes クラスター・マスター用のプライベート・サービス・エンドポイントの導入</td>
<td>デフォルトでは、{{site.data.keyword.containerlong_notm}} は、パブリック VLAN およびプライベート VLAN にアクセスできるクラスターをセットアップします。 以前は、[プライベート VLAN 専用クラスター](/docs/containers?topic=containers-plan_clusters#private_clusters)を使用したい場合には、クラスターのワーカー・ノードとマスターを接続するためにゲートウェイ・アプライアンスをセットアップする必要がありました。 今では、プライベート・サービス・エンドポイントを使用できます。 プライベート・サービス・エンドポイントが有効になっていれば、ワーカー・ノードとマスターの間のすべてのトラフィックがプライベート・ネットワーク経由で行われるので、ゲートウェイ・アプライアンス・デバイスは必要ありません。 このようにセキュリティーが向上するだけでなく、プライベート・ネットワークのインバウンドとアウトバウンドのトラフィックは[無制限かつ無料です ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/cloud/bandwidth)。 パブリック・サービス・エンドポイントは、インターネット経由で Kubernetes マスターに安全にアクセスできるように残しておくことができます。そうすれば、例えば、プライベート・ネットワークの中にいなくても `kubectl` コマンドを実行したりできます。<br><br>
プライベート・サービス・エンドポイントを使用するには、IBM Cloud インフラストラクチャー (SoftLayer) アカウントの [VRF](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) および[サービス・エンドポイント](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started)を有効にする必要があります。 クラスターが Kubernetes バージョン 1.11 以降を実行している必要があります。 これよりも前のバージョンの Kubernetes がクラスターで実行されている場合は、[1.11 以上のバージョンに更新](/docs/containers?topic=containers-update#update)してください。 詳しくは、以下のリンクを参照してください。<ul>
<li>[サービス・エンドポイントを使用したマスター対ワーカーの通信について](/docs/containers?topic=containers-cs_network_ov#cs_network_ov_master)</li>
<li>[プライベート・サービス・エンドポイントのセットアップ](/docs/containers?topic=containers-cs_network_cluster#set-up-private-se)</li>
<li>[パブリック・サービス・エンドポイントからプライベート・サービス・エンドポイントへの切り替え](/docs/containers?topic=containers-cs_network_cluster#migrate-to-private-se)</li>
<li>プライベート・ネットワークにファイアウォールを設けている場合は、[{{site.data.keyword.containerlong_notm}}、{{site.data.keyword.registrylong_notm}}、およびその他の {{site.data.keyword.Bluemix_notm}} サービスのプライベート IP アドレスを追加](/docs/containers?topic=containers-firewall#firewall_outbound)します</li>
</ul>
<p class="important">プライベート・サービス・エンドポイントのみを使用するクラスターに切り替える場合は、使用している他の {{site.data.keyword.Bluemix_notm}} サービスとまだ通信できることを確認してください。 [Portworx ソフトウェア定義ストレージ (SDS)](/docs/containers?topic=containers-portworx#portworx) および[クラスターの自動スケーリング機能](/docs/containers?topic=containers-ca#ca)は、プライベート・サービス・エンドポイントのみの環境をサポートしていません。 代わりに、パブリックとプライベートの両方のサービス・エンドポイントを持つクラスターを使用してください。 クラスターで Kubernetes のバージョン 1.13.4_1513、1.12.6_1544、1.11.8_1550、1.10.13_1551、またはそれ以降が実行されている場合は、[NFS ベースのファイル・ストレージ](/docs/containers?topic=containers-file_storage#file_storage)がサポートされます。</p>
</td>
</tr>
<tr>
<td>2019 年 3 月 7 日</td>
<td>[ベータ版のクラスター自動スケーリング機能の一般提供が開始](/docs/containers?topic=containers-ca#ca)</td>
<td>クラスターの自動スケーリング機能の一般提供が開始されました。 Helm プラグインをインストールすると、スケジュールされているワークロードのサイズ要件に応じて、クラスター内のワーカー・プールが自動的にスケーリングされ、ワーカー・ノード数が増減します。<br><br>
クラスターの自動スケーリング機能についての支援が必要な場合やフィードバックを提供したい場合は、次のようにしてください。 外部ユーザーの場合は、[公開されている Slack ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://bxcs-slack-invite.mybluemix.net/) に登録し、[#cluster-autoscaler ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://ibm-container-service.slack.com/messages/CF6APMLBB) チャネルに投稿してください。 IBM 登録ユーザーの場合は、[内部 Slack ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://ibm-argonauts.slack.com/messages/C90D3KZUL) チャネルに投稿してください。</td>
</tr>
</tbody></table>




## 2019 年 2 月によく閲覧されたトピック
{: #feb19}

<table summary="この表には、よく閲覧されたトピックを示しています。行は左から右に読みます。1 列目は日付、2 列目は機能のタイトル、3 列目は説明です。">
<caption>2019 年 2 月によく閲覧されたコンテナーと Kubernetes クラスターについてのトピック</caption>
<thead>
<th>日</th>
<th>タイトル</th>
<th>説明</th>
</thead>
<tbody>
<tr>
<td>2 月 25 日</td>
<td>{{site.data.keyword.registrylong_notm}} からイメージをプルする処理がより細かく制御可能に</td>
<td>{{site.data.keyword.containerlong_notm}} クラスターにワークロードをデプロイするときに、コンテナーが [{{site.data.keyword.registrylong_notm}} の新しい `icr.io` ドメイン名](/docs/services/Registry?topic=registry-registry_overview#registry_regions)からイメージをプルできるようになりました。 さらに、{{site.data.keyword.Bluemix_notm}} IAM のきめの細かいアクセス・ポリシーを、イメージへのアクセスの制御に使用できます。 詳しくは、[イメージをプルする権限がクラスターに与えられる仕組みについて](/docs/containers?topic=containers-images#cluster_registry_auth)を参照してください。</td>
</tr>
<tr>
<td>2 月 21 日</td>
<td>[メキシコでゾーンの提供開始](/docs/containers?topic=containers-regions-and-zones#regions-and-zones)</td>
<td>クラスターの新しいゾーンとして米国南部地域にメキシコ (`mex01`) が追加されました。 ファイアウォールを使用している場合は、このゾーンと、ご使用のクラスターがある地域内の他のゾーンに対し、必ず[ファイアウォール・ポートを開いてください](/docs/containers?topic=containers-firewall#firewall_outbound)。</td>
</tr>
<tr><td>2019 年 2 月 6 日</td>
<td>[{{site.data.keyword.containerlong_notm}} 上での Istio](/docs/containers?topic=containers-istio)</td>
<td>{{site.data.keyword.containerlong_notm}} の Istio は、Istio のシームレスなインストール、Istio コントロール・プレーン・コンポーネントの自動更新とライフサイクル管理、プラットフォームのロギング・ツールやモニタリング・ツールとの統合が可能です。 ワンクリックですべての Istio コア・コンポーネントを取得し、追加のトレース、モニタリング、視覚化を行い、BookInfo サンプル・アプリを稼働状態にすることができます。 {{site.data.keyword.containerlong_notm}} 上の Istio は管理対象アドオンとして提供されるので、すべての Istio コンポーネントが {{site.data.keyword.Bluemix_notm}} によって自動的に最新に保たれます。</td>
</tr>
<tr>
<td>2019 年 2 月 6 日</td>
<td>[{{site.data.keyword.containerlong_notm}} の Knative](/docs/containers?topic=containers-knative_tutorial)</td>
<td>Knative は、Kubernetes の機能を拡張するオープン・ソースのプラットフォームであり、ソース中心のモダンでサーバーレスなコンテナー化アプリを Kubernetes クラスター上に作成するのを支援します。 {{site.data.keyword.containerlong_notm}} のマネージド Knative は、Kubernetes クラスターに Knative と Istio を直接統合するマネージド・アドオンです。 アドオン内の Knative と Istio のバージョンは、IBM によってテストされ、{{site.data.keyword.containerlong_notm}} での使用をサポートされています。</td>
</tr>
</tbody></table>


## 2019 年 1 月によく閲覧されたトピック
{: #jan19}

<table summary="この表には、よく閲覧されたトピックを示しています。行は左から右に読みます。1 列目は日付、2 列目は機能のタイトル、3 列目は説明です。">
<caption>2019 年 1 月によく閲覧されたコンテナーと Kubernetes クラスターについてのトピック</caption>
<thead>
<th>日</th>
<th>タイトル</th>
<th>説明</th>
</thead>
<tbody>
<tr>
<td>1 月 30 日</td>
<td>{{site.data.keyword.Bluemix_notm}} IAM サービス・アクセス役割と Kubernetes 名前空間</td>
<td>{{site.data.keyword.containerlong_notm}} で {{site.data.keyword.Bluemix_notm}} IAM の[サービス・アクセス役割](/docs/containers?topic=containers-access_reference#service)がサポートされるようになりました。 これらのサービス・アクセス役割が [Kubernetes の RBAC](/docs/containers?topic=containers-users#role-binding) と連携して、クラスター内で `kubectl` 操作を実行することをユーザーに許可します。これにより、ポッドやデプロイメントなどの Kubernetes リソースを管理できます。 さらに、{{site.data.keyword.Bluemix_notm}} IAM サービス・アクセス役割を使用して、クラスター内の[特定の Kubernetes 名前空間にユーザーのアクセスを制限する](/docs/containers?topic=containers-users#platform)ことができます。 [プラットフォーム・アクセス役割](/docs/containers?topic=containers-access_reference#iam_platform)を使用して、`ibmcloud ks` 操作の実行をユーザーに許可できるようになりました。これにより、ワーカー・ノードなどのクラスター・インフラストラクチャーを管理できます。<br><br>{{site.data.keyword.Bluemix_notm}} IAM サービス・アクセス役割は、これまでユーザーが IAM プラットフォーム・アクセス役割によって得ていた権限に基づいて、既存の {{site.data.keyword.containerlong_notm}} アカウントおよびクラスターに自動的に追加されます。 将来は、IAM を使用して、アクセス・グループの作成、アクセス・グループへのユーザーの追加、プラットフォーム・アクセス役割やサービス・アクセス役割のグループへの割り当てを行えるようになります。 詳しくは、ブログ [Introducing service roles and namespaces in IAM for more granular control of cluster access ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/blogs/bluemix/2019/02/introducing-service-roles-and-namespaces-in-iam-for-more-granular-control-of-cluster-access/) を参照してください。</td>
</tr>
<tr>
<td>1 月 8 日</td>
<td>[クラスターの自動スケーリング機能のベータ版プレビュー](/docs/containers?topic=containers-ca#ca)</td>
<td>スケジュールされているワークロードのサイズ要件に応じて、クラスター内のワーカー・プールが自動的にスケーリングされ、ワーカー・プール内のワーカー・ノード数が増減します。 このベータ版を試すには、ホワイトリストへのアクセスを要求する必要があります。</td>
</tr>
<tr>
<td>1 月 8 日</td>
<td>[{{site.data.keyword.containerlong_notm}} 診断およびデバッグ・ツール](/docs/containers?topic=containers-cs_troubleshoot#debug_utility)</td>
<td>問題をトラブルシューティングするためにすべての YAML ファイルとその他の必要な情報を取得する作業が大変だと思うことはありませんか? クラスター、デプロイメント、ネットワークの情報を収集できるグラフィカル・ユーザー・インターフェースの {{site.data.keyword.containerlong_notm}} 診断およびデバッグ・ツールをお試しください。</td>
</tr>
</tbody></table>

## 2018 年 12 月によく閲覧されたトピック
{: #dec18}

<table summary="この表には、よく閲覧されたトピックを示しています。行は左から右に読みます。1 列目は日付、2 列目は機能のタイトル、3 列目は説明です。">
<caption>2018 年 12 月によく閲覧されたコンテナーと Kubernetes クラスターについてのトピック</caption>
<thead>
<th>日</th>
<th>タイトル</th>
<th>説明</th>
</thead>
<tbody>
<tr>
<td>12 月 11 日</td>
<td>Portworx による SDS のサポート</td>
<td>Portworx を使用すると、コンテナー化されたデータベースやその他のステートフル・アプリのための永続ストレージを管理したり、複数のゾーンのポッド間でデータを共有したりできます。 [Portworx ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://portworx.com/products/introduction/) は、複数のワーカー・ノードのローカル・ブロック・ストレージを管理して 1 つの統合永続ストレージ層をアプリのために作成する、可用性の高いソフトウェア定義ストレージ (SDS) ソリューションです。 複数のワーカー・ノードの間でコンテナー・レベルの各ボリュームを複製することで、Portworx はデータの永続性とデータ・アクセシビリティーをゾーンをまたいで確保します。 詳しくは、[Portworx によるソフトウェア定義ストレージ (SDS) へのデータ保管](/docs/containers?topic=containers-portworx#portworx)を参照してください。    </td>
</tr>
<tr>
<td>12 月 6 日</td>
<td>{{site.data.keyword.mon_full_notm}}</td>
<td>Sysdig をサード・パーティー・サービスとしてワーカー・ノードにデプロイし、メトリックを {{site.data.keyword.monitoringlong}} に転送することで、アプリのパフォーマンスと正常性を可視化して運用することができます。 詳しくは、[Kubernetes クラスターにデプロイされたアプリのメトリックの分析方法](/docs/services/Monitoring-with-Sysdig/tutorials?topic=Sysdig-kubernetes_cluster#kubernetes_cluster)を参照してください。 **注**: Kubernetes バージョン 1.11 以降を実行するクラスターで {{site.data.keyword.mon_full_notm}} を使用する場合、Sysdig は現在 `containerd` をサポートしていないため、すべてのコンテナー・メトリックが収集されるわけではありません。</td>
</tr>
<tr>
<td>12 月 4 日</td>
<td>[ワーカー・ノードのリソース予約](/docs/containers?topic=containers-plan_clusters#resource_limit_node)</td>
<td>デプロイするアプリが多すぎるためにクラスターの容量を超過しないか心配していませんか? ワーカー・ノードのリソース予約と Kubernetes 強制除去機能を利用すれば、稼働を維持するためのリソースをクラスターが十分に確保できるようにクラスターのコンピューティング容量を保護することができます。 少数のワーカー・ノードを追加するだけで、ポッドがそれらで実行されるように自動的に再スケジュールされます。 ワーカー・ノードのリソース予約は、最新 [バージョンのパッチ変更](/docs/containers?topic=containers-changelog#changelog)で更新されています。</td>
</tr>
</tbody></table>

## 2018 年 11 月によく閲覧されたトピック
{: #nov18}

<table summary="この表には、よく閲覧されたトピックを示しています。行は左から右に読みます。1 列目は日付、2 列目は機能のタイトル、3 列目は説明です。">
<caption>2018 年 11 月によく閲覧されたコンテナーと Kubernetes クラスターについてのトピック</caption>
<thead>
<th>日</th>
<th>タイトル</th>
<th>説明</th>
</thead>
<tbody>
<tr>
<td>11 月 29 日</td>
<td>[チェンナイのゾーンの提供開始](/docs/containers?topic=containers-regions-and-zones)</td>
<td>北アジア太平洋地域のクラスターの新しいゾーンとして、インドのチェンナイが追加されました。 ファイアウォールを使用している場合は、このゾーンと、ご使用のクラスターがある地域内の他のゾーンに対し、必ず[ファイアウォール・ポートを開いてください](/docs/containers?topic=containers-firewall#firewall)。</td>
</tr>
<tr>
<td>11 月 27 日</td>
<td>{{site.data.keyword.la_full_notm}}</td>
<td>LogDNA をサード・パーティー・サービスとしてワーカー・ノードにデプロイし、ポッド・コンテナーのログを管理することで、ログ管理機能をクラスターに追加できます。 詳しくは、[{{site.data.keyword.loganalysisfull_notm}} with LogDNA による Kubernetes クラスター・ログの管理](/docs/services/Log-Analysis-with-LogDNA/tutorials?topic=LogDNA-kube#kube) を参照してください。</td>
</tr>
<tr>
<td>11 月 7 日</td>
<td>ネットワーク・ロード・バランサー 2.0 (ベータ版)</td>
<td>クラスター・アプリをパブリックに安全に公開するために、[NLB 1.0 と 2.0](/docs/containers?topic=containers-loadbalancer#planning_ipvs) の間で選択できるようになりました。</td>
</tr>
<tr>
<td>11 月 7 日</td>
<td>Kubernetes バージョン 1.12 の提供開始</td>
<td>[Kubernetes バージョン  1.12](/docs/containers?topic=containers-cs_versions#cs_v112) を実行するクラスターに更新またはこれを作成できるようになりました。 1.12 クラスターの Kubernetes マスターは、デフォルトで高可用性です。</td>
</tr>
<tr>
<td>11 月 7 日</td>
<td>高可用性のマスター</td>
<td>Kubernetes バージョン 1.10 を実行するクラスターで高可用性マスターを使用できます。 1.11 クラスターに関する過去の情報で説明されているすべてのメリットが 1.10 クラスターに適用されます。また、実行する必要がある[準備手順](/docs/containers?topic=containers-cs_versions#110_ha-masters)も適用されます。</td>
</tr>
<tr>
<td>11 月 1 日</td>
<td>Kubernetes バージョン 1.11 を実行するクラスターの高可用性マスター</td>
<td>単一ゾーンのマスターは高可用性です。クラスター更新時などの停止を防ぐために、Kubernetes API サーバー、etcd、スケジューラー、およびコントローラー・マネージャー用に別々の物理ホスト上にレプリカが配置されます。 クラスターが複数ゾーン対応ゾーンにある場合は、高可用性マスターも複数ゾーンに分散されるので、ゾーン障害から保護されます。<br>実行する必要があるアクションについては、[高可用性クラスター・マスターへの更新](/docs/containers?topic=containers-cs_versions#ha-masters)を参照してください。 これらの準備アクションは、以下の場合に適用されます。<ul>
<li>ファイアウォールまたはカスタム Calico ネットワーク・ポリシーがある場合。</li>
<li>ワーカー・ノードでホスト・ポート `2040` または `2041` を使用している場合。</li>
<li>マスターへのクラスター内アクセス用にクラスターのマスター IP アドレスを使用していた場合。</li>
<li>Calico ポリシーの作成などのために、Calico API または CLI (`calicoctl`) を呼び出す自動化機能がある場合。</li>
<li>Kubernetes または Calico ネットワーク・ポリシーを使用して、ポッドからマスターへの発信アクセスを制御している場合。</li></ul></td>
</tr>
</tbody></table>

## 2018 年 10 月によく閲覧されたトピック
{: #oct18}

<table summary="この表には、よく閲覧されたトピックを示しています。行は左から右に読みます。1 列目は日付、2 列目は機能のタイトル、3 列目は説明です。">
<caption>2018 年 10 月によく閲覧されたコンテナーと Kubernetes クラスターについてのトピック</caption>
<thead>
<th>日</th>
<th>タイトル</th>
<th>説明</th>
</thead>
<tbody>
<tr>
<td>10 月 25 日</td>
<td>[ミラノでゾーンが使用可能に](/docs/containers?topic=containers-regions-and-zones)</td>
<td>中欧地域の有料クラスターの新しいゾーンとして、イタリアのミラノが加わりました。 これまでミラノは、無料クラスターでのみ使用可能でした。 ファイアウォールを使用している場合は、このゾーンと、ご使用のクラスターがある地域内の他のゾーンに対し、必ず[ファイアウォール・ポートを開いてください](/docs/containers?topic=containers-firewall#firewall)。</td>
</tr>
<tr>
<td>10 月 22 日</td>
<td>[新しいロンドン複数ゾーン・ロケーション `lon05`](/docs/containers?topic=containers-regions-and-zones#zones)</td>
<td>複数ゾーンの大都市ロケーションであるロンドンは、`lon02` ゾーンから、新しい `lon05` ゾーンに置き換えられました。これは、`lon02` より多くのインフラストラクチャー・リソースを備えています。新しい複数ゾーン・クラスターを作成するときは、`lon05` を使用してください。 `lon02` を使用した既存のクラスターもサポートされますが、新しい複数ゾーン・クラスターでは代わりに `lon05` を使用する必要があります。</td>
</tr>
<tr>
<td>10 月 5 日</td>
<td>{{site.data.keyword.keymanagementservicefull}} (ベータ版) との統合</td>
<td>[{{site.data.keyword.keymanagementserviceshort}} (ベータ) を有効にして](/docs/containers?topic=containers-encryption#keyprotect)、クラスター内にある Kubernetes シークレットを暗号化できます。</td>
</tr>
<tr>
<td>10 月 4 日</td>
<td>[{{site.data.keyword.registrylong}} が {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM)](/docs/services/Registry?topic=registry-iam#iam) と統合される</td>
<td>{{site.data.keyword.Bluemix_notm}} IAM を使用して、レジストリー・リソースへのアクセス (イメージのプル、プッシュ、作成など) を制御できます。 クラスターを作成するときに、レジストリー・トークンも作成することによって、クラスターがレジストリーと連携できるようにします。 したがって、クラスターを作成するには、アカウント・レベルのレジストリーの**管理者**のプラットフォーム管理役割が必要です。 ご使用のレジストリー・アカウントで {{site.data.keyword.Bluemix_notm}} IAM を有効にするには、[Enabling policy enforcement for existing users](/docs/services/Registry?topic=registry-user#existing_users) を参照してください。</td>
</tr>
<tr>
<td>10 月 1 日</td>
<td>[リソース・グループ](/docs/containers?topic=containers-users#resource_groups)</td>
<td>リソース・グループを使用すると、{{site.data.keyword.Bluemix_notm}} リソースをパイプライン、部門、その他のグループに分離して、アクセス権限の割り当てや使用量の課金に役立てることができます。 {{site.data.keyword.containerlong_notm}} は、`default` グループや、自分で作成した他のリソース・グループでのクラスターの作成をサポートするようになりました。</td>
</tr>
</tbody></table>

## 2018 年 9 月によく閲覧されたトピック
{: #sept18}

<table summary="この表には、よく閲覧されたトピックを示しています。行は左から右に読みます。1 列目は日付、2 列目は機能のタイトル、3 列目は説明です。">
<caption>2018 年 9 月によく閲覧されたコンテナーと Kubernetes クラスターについてのトピック</caption>
<thead>
<th>日</th>
<th>タイトル</th>
<th>説明</th>
</thead>
<tbody>
<tr>
<td>9 月 25 日</td>
<td>[新しいゾーンが使用可能に](/docs/containers?topic=containers-regions-and-zones)</td>
<td>アプリのデプロイ先として選択できるオプションが増えました。
<ul><li>サンノゼが、米国南部地域の 2 つの新しいゾーン `sjc03` および `sjc04` として加わりました。 ファイアウォールを使用している場合は、このゾーンと、ご使用のクラスターがある地域内の他のロケーションに対し、必ず[ファイアウォール・ポートを開いてください](/docs/containers?topic=containers-firewall#firewall)。</li>
<li>北アジア太平洋地域の東京で、2 つの新しい `tok04` および `tok05` ゾーンを使用して、[複数ゾーン・クラスターを作成](/docs/containers?topic=containers-plan_clusters#multizone)できるようになりました。</li></ul></td>
</tr>
<tr>
<td>9 月 5 日</td>
<td>[オスロでゾーンが使用可能に](/docs/containers?topic=containers-regions-and-zones)</td>
<td>中欧地域の新しいゾーンとして、ノルウェーのオスロが加わりました。 ファイアウォールを使用している場合は、このゾーンと、ご使用のクラスターがある地域内の他のロケーションに対し、必ず[ファイアウォール・ポートを開いてください](/docs/containers?topic=containers-firewall#firewall)。</td>
</tr>
</tbody></table>

## 2018 年 8 月によく閲覧されたトピック
{: #aug18}

<table summary="この表には、よく閲覧されたトピックを示しています。行は左から右に読みます。1 列目は日付、2 列目は機能のタイトル、3 列目は説明です。">
<caption>2018 年 8 月によく閲覧されたコンテナーと Kubernetes クラスターについてのトピック</caption>
<thead>
<th>日</th>
<th>タイトル</th>
<th>説明</th>
</thead>
<tbody>
<tr>
<td>8 月 31 日</td>
<td>{{site.data.keyword.cos_full_notm}} が {{site.data.keyword.containerlong}} と統合される</td>
<td>Kubernetes ネイティブの永続ボリューム請求 (PVC) を使用して、{{site.data.keyword.cos_full_notm}} をクラスターにプロビジョンします。 {{site.data.keyword.cos_full_notm}} は読み取り集中型ワークロードに最適です。また、複数ゾーン・クラスター内の複数のゾーンにデータを保管する場合に使用します。 まず、[{{site.data.keyword.cos_full_notm}} サービス・インスタンスを作成](/docs/containers?topic=containers-object_storage#create_cos_service)し、クラスターに [{{site.data.keyword.cos_full_notm}} プラグインをインストール](/docs/containers?topic=containers-object_storage#install_cos)します。 </br></br>どのストレージ・ソリューションが正しいかわからない場合: [ここ](/docs/containers?topic=containers-storage_planning#choose_storage_solution)から開始してデータを分析し、データに適したストレージ・ソリューションを選択します。 </td>
</tr>
<tr>
<td>8 月 14 日</td>
<td>ポッドの優先度を割り当てるためにクラスターを Kubernetes バージョン 1.11 に更新する</td>
<td>クラスターを [Kubernetes バージョン 1.11](/docs/containers?topic=containers-cs_versions#cs_v111) に更新した後、`containerd` を使用したコンテナー・ランタイムのパフォーマンスの向上や[ポッドへの優先度の割り当て](/docs/containers?topic=containers-pod_priority#pod_priority)など、新しい機能を利用できます。</td>
</tr>
</tbody></table>

## 2018 年 7 月によく閲覧されたトピック
{: #july18}

<table summary="この表には、よく閲覧されたトピックを示しています。行は左から右に読みます。1 列目は日付、2 列目は機能のタイトル、3 列目は説明です。">
<caption>2018 年 7 月によく閲覧されたコンテナーと Kubernetes クラスターについてのトピック</caption>
<thead>
<th>日</th>
<th>タイトル</th>
<th>説明</th>
</thead>
<tbody>
<tr>
<td>7 月 30 日</td>
<td>[自分の Ingress コントローラーを持ち込む](/docs/containers?topic=containers-ingress#user_managed)</td>
<td>クラスターの Ingress コントローラーに関して非常に特殊なセキュリティー要件やその他のカスタム要件を持っていますか? そうであれば、デフォルトの代わりに、持ち込みの Ingress コントローラーを実行することができます。</td>
</tr>
<tr>
<td>7 月 10 日</td>
<td>複数ゾーン・クラスターの概要</td>
<td>クラスターの可用性を向上させる必要がありますか? 一部の大都市圏では、複数のゾーンにまたがるクラスターを作成できるようになりました。 詳しくは、[{{site.data.keyword.containerlong_notm}} でのマルチゾーン・クラスターの作成](/docs/containers?topic=containers-plan_clusters#multizone)を参照してください。</td>
</tr>
</tbody></table>

## 2018 年 6 月によく閲覧されたトピック
{: #june18}

<table summary="この表には、よく閲覧されたトピックを示しています。行は左から右に読みます。1 列目は日付、2 列目は機能のタイトル、3 列目は説明です。">
<caption>2018 年 6 月によく閲覧されたコンテナーと Kubernetes クラスターについてのトピック</caption>
<thead>
<th>日</th>
<th>タイトル</th>
<th>説明</th>
</thead>
<tbody>
<tr>
<td>6 月 13 日</td>
<td>`bx` CLI コマンド名が `ic` CLI に変更されています。</td>
<td>最新バージョンの {{site.data.keyword.Bluemix_notm}} CLI をダウンロードした後は、`bx` ではなく `ic` 接頭部を使用してコマンドを実行するようになります。 例えば、`ibmcloud ks clusters` を実行するとクラスターがリスト表示されます。</td>
</tr>
<tr>
<td>6 月 12 日</td>
<td>[ポッド・セキュリティー・ポリシー](/docs/containers?topic=containers-psp)</td>
<td>Kubernetes 1.10.3 以降を実行するクラスターでは、ポッド・セキュリティー・ポリシーを構成して、{{site.data.keyword.containerlong_notm}} のポッドの作成および更新をユーザーに許可することができます。</td>
</tr>
<tr>
<td>6 月 6 日</td>
<td>[IBM 提供の Ingress ワイルドカード・サブドメインに対する TLS サポート](/docs/containers?topic=containers-ingress#wildcard_tls)</td>
<td>2018 年 6 月 6 日以降に作成されたクラスターの場合、IBM 提供の Ingress サブドメインの TLS 証明書はワイルドカード証明書ですので、登録されているワイルドカード・サブドメインに使用できます。 2018 年 6 月 6 日より前に作成されたクラスターの場合、現在の TLS 証明書が更新されるときに、ワイルドカード証明書に更新されます。</td>
</tr>
</tbody></table>

## 2018 年 5 月によく閲覧されたトピック
{: #may18}


<table summary="この表には、よく閲覧されたトピックを示しています。行は左から右に読みます。1 列目は日付、2 列目は機能のタイトル、3 列目は説明です。">
<caption>2018 年 5 月によく閲覧されたコンテナーと Kubernetes クラスターについてのトピック</caption>
<thead>
<th>日</th>
<th>タイトル</th>
<th>説明</th>
</thead>
<tbody>
<tr>
<td>5 月 24 日</td>
<td>[新しい Ingress サブドメイン形式](/docs/containers?topic=containers-ingress)</td>
<td>5 月 24 日より後に作成されたクラスターには、新しい形式の <code>&lt;cluster_name&gt;.&lt;region&gt;.containers.appdomain.cloud</code> で Ingress サブドメインが割り当てられます。 Ingress を使用してアプリを公開する場合、新しいサブドメインを使用してインターネットからアプリにアクセスできます。</td>
</tr>
<tr>
<td>5 月 14 日</td>
<td>[更新: 世界中の GPU ベアメタルへのワークロードのデプロイ](/docs/containers?topic=containers-app#gpu_app)</td>
<td>クラスター内に[ベアメタルのグラフィックス・プロセッシング・ユニット (GPU) マシン・タイプ](/docs/containers?topic=containers-plan_clusters#shared_dedicated_node)がある場合、大量の数学的処理が行われるアプリをスケジュールできます。 GPU ワーカー・ノードは、CPU と GPU の両方にわたってアプリのワークロードを処理することで、パフォーマンスを向上させることができます。</td>
</tr>
<tr>
<td>5 月 3 日</td>
<td>[Container Image Security Enforcement (ベータ)](/docs/services/Registry?topic=registry-security_enforce#security_enforce)</td>
<td>チームでは、アプリ・コンテナーでプルするイメージを決める際の支援が必要ではありませんか。 コンテナー・イメージを検証してからデプロイできる、Container Image Security Enforcement ベータをお試しください。 Kubernetes 1.9 以降を実行するクラスターで使用可能です。</td>
</tr>
<tr>
<td>5 月 1 日</td>
<td>[コンソールからの Kubernetes ダッシュボードのデプロイ](/docs/containers?topic=containers-app#cli_dashboard)</td>
<td>ワンクリックで Kubernetes ダッシュボードにアクセスしたいと思ったことはありませんか。 {{site.data.keyword.Bluemix_notm}} コンソールの**「Kubernetes ダッシュボード (Kubernetes Dashboard)」**ボタンをお試しください。</td>
</tr>
</tbody></table>




## 2018 年 4 月によく閲覧されたトピック
{: #apr18}

<table summary="この表には、よく閲覧されたトピックを示しています。行は左から右に読みます。1 列目は日付、2 列目は機能のタイトル、3 列目は説明です。">
<caption>2018 年 4 月によく閲覧されたコンテナーと Kubernetes クラスターについてのトピック</caption>
<thead>
<th>日</th>
<th>タイトル</th>
<th>説明</th>
</thead>
<tbody>
<tr>
<td>4 月 17 日</td>
<td>{{site.data.keyword.Bluemix_notm}} Block Storage</td>
<td>永続データをブロック・ストレージに保存するための {{site.data.keyword.Bluemix_notm}} Block Storage [プラグイン](/docs/containers?topic=containers-block_storage#install_block)をインストールします。 そして、クラスターのために[ブロック・ストレージを新規作成](/docs/containers?topic=containers-block_storage#add_block)するか、または[既存のブロック・ストレージを使用](/docs/containers?topic=containers-block_storage#existing_block)することができます。</td>
</tr>
<tr>
<td>4 月 13 日</td>
<td>[Cloud Foundry アプリをクラスターにマイグレーションするための新しいチュートリアル](/docs/containers?topic=containers-cf_tutorial#cf_tutorial)</td>
<td>Cloud Foundry アプリをお持ちですか? そのアプリの同じコードを、Kubernetes クラスターで実行されるコンテナーにデプロイする方法を示します。</td>
</tr>
<tr>
<td>4 月 5 日</td>
<td>[ログのフィルタリング](/docs/containers?topic=containers-health#filter-logs)</td>
<td>特定のログを転送対象から除外することができます。 特定の名前空間、コンテナー名、ログ・レベル、メッセージ・ストリングを基準にしてログを除外できます。</td>
</tr>
</tbody></table>

## 2018 年 3 月によく閲覧されたトピック
{: #mar18}

<table summary="この表には、よく閲覧されたトピックを示しています。行は左から右に読みます。1 列目は日付、2 列目は機能のタイトル、3 列目は説明です。">
<caption>2018 年 3 月によく閲覧されたコンテナーと Kubernetes クラスターについてのトピック</caption>
<thead>
<th>日</th>
<th>タイトル</th>
<th>説明</th>
</thead>
<tbody>
<tr>
<td>3 月 16 日</td>
<td>[トラステッド・コンピューティングを使用するベア・メタル・クラスターのプロビジョン](/docs/containers?topic=containers-plan_clusters#shared_dedicated_node)</td>
<td>[Kubernetes バージョン 1.9](/docs/containers?topic=containers-cs_versions#cs_v19) 以降を実行するベア・メタル・クラスターを作成し、トラステッド・コンピューティングを有効にしてワーカー・ノードが改ざんされていないことを検証できます。</td>
</tr>
<tr>
<td>3 月 14 日</td>
<td>[{{site.data.keyword.appid_full}} による安全なサインイン](/docs/containers?topic=containers-supported_integrations#appid)</td>
<td>ユーザーに対してサインインを要求することにより、{{site.data.keyword.containerlong_notm}} で実行されるアプリのセキュリティーを強化します。</td>
</tr>
<tr>
<td>3 月 13 日</td>
<td>[サンパウロで使用可能なゾーン](/docs/containers?topic=containers-regions-and-zones)</td>
<td>米国南部地域の新しいゾーンとしてブラジルのサンパウロが追加されました。 ファイアウォールを使用している場合は、このゾーンと、ご使用のクラスターがある地域内の他のロケーションに対し、必ず[ファイアウォール・ポートを開いてください](/docs/containers?topic=containers-firewall#firewall)。</td>
</tr>
</tbody></table>

## 2018 年 2 月によく閲覧されたトピック
{: #feb18}

<table summary="この表には、よく閲覧されたトピックを示しています。行は左から右に読みます。1 列目は日付、2 列目は機能のタイトル、3 列目は説明です。">
<caption>2018 年 2 月によく閲覧されたコンテナーと Kubernetes クラスターについてのトピック</caption>
<thead>
<th>日</th>
<th>タイトル</th>
<th>説明</th>
</thead>
<tbody>
<tr>
<td>2 月 27 日</td>
<td>ワーカー・ノード用のハードウェア仮想マシン (HVM) のイメージ</td>
<td>HVM イメージを使用して、ワークロードの入出力パフォーマンスを向上させます。 `ibmcloud ks worker-reload` [コマンド](/docs/containers?topic=containers-cs_cli_reference#cs_worker_reload)または `ibmcloud ks worker-update` [コマンド](/docs/containers?topic=containers-cs_cli_reference#cs_worker_update)を使用して、既存の各ワーカー・ノード上でアクティブ化します。</td>
</tr>
<tr>
<td>2 月 26 日</td>
<td>[KubeDNS 自動スケーリング](https://kubernetes.io/docs/tasks/administer-cluster/dns-horizontal-autoscaling/)</td>
<td>KubeDNS は、クラスターの拡大に合わせてスケーリングされるようになりました。 スケーリングの比率は、コマンド `kubectl -n kube-system edit cm kube-dns-autoscaler` を使用して調整できます。</td>
</tr>
<tr>
<td>2 月 23 日</td>
<td>Web コンソールでの[ロギング](/docs/containers?topic=containers-health#view_logs)および[メトリック](/docs/containers?topic=containers-health#view_metrics)の表示</td>
<td>改善された Web UI を使用して、クラスターとそのコンポーネントのログとメトリック・データを簡単に表示できます。 アクセス方法については、クラスターの詳細ページを参照してください。</td>
</tr>
<tr>
<td>2 月 20 日</td>
<td>暗号化されたイメージと[署名された信頼できるコンテンツ](/docs/services/Registry?topic=registry-registry_trustedcontent#registry_trustedcontent)</td>
<td>{{site.data.keyword.registryshort_notm}} では、イメージをレジストリー名前空間に保管する際に、イメージに署名して暗号化することで、イメージの保全性を確保できます。 コンテナー・インスタンスは、信頼できるコンテンツのみを使用して実行してください。</td>
</tr>
<tr>
<td>2 月 19 日</td>
<td>[strongSwan IPSec VPN のセットアップ](/docs/containers?topic=containers-vpn#vpn-setup)</td>
<td>strongSwan IPSec VPN Helm チャートを素早くデプロイして、Virtual Router Appliance なしで {{site.data.keyword.containerlong_notm}} クラスターをオンプレミス・データ・センターに安全に接続します。</td>
</tr>
<tr>
<td>2 月 14 日</td>
<td>[ソウルで使用可能なゾーン](/docs/containers?topic=containers-regions-and-zones)</td>
<td>オリンピックの時期に合わせて、Kubernetes クラスターを北アジア太平洋地域のソウルにデプロイします。 ファイアウォールを使用している場合は、このゾーンと、ご使用のクラスターがある地域内の他のロケーションに対し、必ず[ファイアウォール・ポートを開いてください](/docs/containers?topic=containers-firewall#firewall)。</td>
</tr>
<tr>
<td>2 月 8 日</td>
<td>[Kubernetes 1.9 の更新](/docs/containers?topic=containers-cs_versions#cs_v19)</td>
<td>Kubernetes 1.9 に更新する前に、クラスターに対して加える変更を確認してください。</td>
</tr>
</tbody></table>

## 2018 年 1 月によく閲覧されたトピック
{: #jan18}

<table summary="この表には、よく閲覧されたトピックを示しています。行は左から右に読みます。1 列目は日付、2 列目は機能のタイトル、3 列目は説明です。">
<caption>2018 年 1 月によく閲覧されたコンテナーと Kubernetes クラスターについてのトピック</caption>
<thead>
<th>日</th>
<th>タイトル</th>
<th>説明</th>
</thead>
<tbody>
<td>1 月 25 日</td>
<td>[グローバル・レジストリーの提供](/docs/services/Registry?topic=registry-registry_overview#registry_regions)</td>
<td>{{site.data.keyword.registryshort_notm}} では、グローバル `registry.bluemix.net` を使用して、IBM 提供のパブリック・イメージをプルできます。</td>
</tr>
<tr>
<td>1 月 23 日</td>
<td>[シンガポールおよびカナダのモントリオールで使用可能なゾーン](/docs/containers?topic=containers-regions-and-zones)</td>
<td>シンガポールおよびモントリオールは、{{site.data.keyword.containerlong_notm}} の北アジア太平洋地域と米国東部地域で使用可能なゾーンです。 ファイアウォールを使用している場合は、これらのゾーンと、ご使用のクラスターがある地域内の他のロケーションに対し、必ず[ファイアウォール・ポートを開いてください](/docs/containers?topic=containers-firewall#firewall)。</td>
</tr>
<tr>
<td>1 月 8 日</td>
<td>[拡張されたフレーバーが使用可能](/docs/containers?topic=containers-cs_cli_reference#cs_machine_types)</td>
<td>Series 2 仮想マシン・タイプには、ローカル SSD ストレージとディスク暗号化が含まれます。 パフォーマンスと安定性を向上させるために、これらのフレーバーに[ワークロードを移動](/docs/containers?topic=containers-update#machine_type)してください。</td>
</tr>
</tbody></table>

## 同じ関心を持つ開発者との Slack でのチャット
{: #slack}

[{{site.data.keyword.containerlong_notm}} Slack ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://ibm-container-service.slack.com) で、他の開発者の間で話題となっている内容を確認したり、質問をしたりすることができます。
{:shortdesc}

{{site.data.keyword.Bluemix_notm}} アカウントに IBM ID を使用していない場合は、この Slack への[招待を要求](https://bxcs-slack-invite.mybluemix.net/)してください。
{: tip}
