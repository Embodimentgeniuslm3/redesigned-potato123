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

# リリース・ノート
{: #iks-release}

リリース・ノートを使用して、月別にグループ化された {{site.data.keyword.containerlong}} の資料に対する最新の変更をご確認ください。
{:shortdesc}

## 2019 年 6 月
{: #jun19}

<table summary="この表は、リリース・ノートを示しています。行は左から右に読みます。1 列目は日付、2 列目は機能のタイトル、3 列目は説明です。">
<caption>{{site.data.keyword.containerlong_notm}} の資料の更新 (2019 年 6 月)</caption>
<thead>
<th>日</th>
<th>説明</th>
</thead>
<tbody>
<tr>
  <td>2019 年 6 月 7 日</td>
  <td><ul>
  <li><strong>プライベート・サービス・エンドポイントを介した Kubernetes マスターへのアクセス</strong>: プライベート・ロード・バランサーを介してプライベート・サービス・エンドポイントを公開するための[ステップ](/docs/containers?topic=containers-clusters#access_on_prem)が追加されました。これらのステップを完了すると、権限があるクラスター・ユーザーは、VPN または {{site.data.keyword.Bluemix_notm}} Direct Link 接続から Kubernetes マスターにアクセスできます。</li>
  <li><strong>{{site.data.keyword.BluDirectLink}}</strong>: パブリック・インターネット経由でルーティングせずに、リモート・ネットワーク環境と {{site.data.keyword.containerlong_notm}} の間の直接のプライベート接続を作成する手段として、{{site.data.keyword.Bluemix_notm}} Direct Link が [VPN 接続](/docs/containers?topic=containers-vpn)と[ハイブリッド・クラウド](/docs/containers?topic=containers-hybrid_iks_icp)のページに追加されました。</li>
  <li><strong>Ingress ALB の変更ログ</strong>: [ALB `ingress-auth` イメージがビルド 330 に](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog)更新されました。</li>
  <li><strong>OpenShift ベータ</strong>: {{site.data.keyword.la_full_notm}} および {{site.data.keyword.mon_full_notm}} アドオンの特権セキュリティー・コンテキスト制約に対処するためのアプリ・デプロイメントの変更方法に関する[レッスンが追加されました](/docs/containers?topic=containers-openshift_tutorial#openshift_logdna_sysdig)。</li>
  </ul></td>
</tr>
<tr>
  <td>2019 年 6 月 6 日</td>
  <td><ul>
  <li><strong>Fluentd の変更ログ</strong>: [Fluentd バージョンの変更ログ](/docs/containers?topic=containers-cluster-add-ons-changelog#fluentd_changelog)が追加されました。</li>
  <li><strong>Ingress ALB の変更ログ</strong>: [ALB `nginx-ingress` イメージがビルド 470 に](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog)更新されました。</li>
  </ul></td>
</tr>
<tr>
  <td>2019 年 6 月 5 日</td>
  <td><ul>
  <li><strong>CLI リファレンス</strong>: {{site.data.keyword.containerlong_notm}} CLI プラグインの[バージョン 0.3.34 のリリース](/docs/containers?topic=containers-cs_cli_changelog)に対する複数の変更を反映するように [CLI リファレンス・ページ](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli)が更新されました。</li>
  <li><strong>新着情報! Red Hat OpenShift on IBM Cloud クラスター (ベータ)</strong>: Red Hat OpenShift on IBM Cloud ベータでは、OpenShift コンテナー・オーケストレーション・プラットフォーム・ソフトウェアとともにインストールされるワーカー・ノードを持つ {{site.data.keyword.containerlong_notm}} クラスターを作成できます。クラスター・インフラストラクチャー環境用の管理対象 {{site.data.keyword.containerlong_notm}} のすべての利点は、アプリのデプロイメントのために Red Hat Enterprise Linux 上で実行される [OpenShift ツールおよびカタログ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.openshift.com/container-platform/3.11/welcome/index.html) とともに入手できます。開始するには、[チュートリアル: Red Hat OpenShift on IBM Cloud クラスター (ベータ版) の作成](/docs/containers?topic=containers-openshift_tutorial)を参照してください。</li>
  </ul></td>
</tr>
<tr>
  <td>2019 年 6 月 4 日</td>
  <td><ul>
  <li><strong>バージョンの変更ログ</strong>: パッチ・リリース [1.14.2_1521](/docs/containers?topic=containers-changelog#1142_1521)、[1.13.6_1524](/docs/containers?topic=containers-changelog#1136_1524)、[1.12.9_1555](/docs/containers?topic=containers-changelog#1129_1555)、および [1.11.10_1561](/docs/containers?topic=containers-changelog#11110_1561) の変更ログが更新されました。</li></ul>
  </td>
</tr>
<tr>
  <td>2019 年 6 月 3 日</td>
  <td><ul>
  <li><strong>独自の Ingress コントローラーの持ち込み</strong>: デフォルトのコミュニティー・コントローラーに対する変更を反映し、複数ゾーン・クラスター内のコントローラー IP アドレスのヘルス・チェックを必要とするように[ステップ](/docs/containers?topic=containers-ingress#user_managed)が更新されました。</li>
  <li><strong>{{site.data.keyword.cos_full_notm}}</strong>: Helm サーバー Tiller の有無にかかわらず {{site.data.keyword.cos_full_notm}} プラグインをインストールするように[ステップ](/docs/containers?topic=containers-object_storage#install_cos)が更新されました。</li>
  <li><strong>Ingress ALB の変更ログ</strong>: [ALB `nginx-ingress` イメージがビルド 467 に](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog)更新されました。</li>
  <li><strong>Kustomize</strong>: [Kustomize がある複数の環境で Kubernetes 構成ファイルを再利用する](/docs/containers?topic=containers-app#kustomize)例が追加されました。</li>
  <li><strong>Razee</strong>: クラスター内のデプロイメント情報を視覚化し、Kubernetes リソースのデプロイメントを自動化するために、[Razee ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/razee-io/Razee) がサポートされる統合に追加されました。</li></ul>
  </td>
</tr>
</tbody></table>

## 2019 年 5 月
{: #may19}

<table summary="この表は、リリース・ノートを示しています。行は左から右に読みます。1 列目は日付、2 列目は機能のタイトル、3 列目は説明です。">
<caption>{{site.data.keyword.containerlong_notm}} の資料の更新 (2019 年 5 月)</caption>
<thead>
<th>日</th>
<th>説明</th>
</thead>
<tbody>
<tr>
  <td>2019 年 5 月 30 日</td>
  <td><ul>
  <li><strong>CLI リファレンス</strong>: {{site.data.keyword.containerlong_notm}} CLI プラグインの[バージョン 0.3.33 のリリース](/docs/containers?topic=containers-cs_cli_changelog)に対する複数の変更を反映するように [CLI リファレンス・ページ](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli)が更新されました。</li>
  <li><strong>ストレージのトラブルシューティング</strong>: <ul>
  <li>[PVC 保留状態](/docs/containers?topic=containers-cs_troubleshoot_storage#file_pvc_pending)に関するファイルおよびブロック・ストレージのトラブルシューティングのトピックが追加されました。</li>
  <li>[アプリが PVC にアクセスできない、または PVC に書き込むことができない](/docs/containers?topic=containers-cs_troubleshoot_storage#block_app_failures)場合のブロック・ストレージのトラブルシューティング・トピックが追加されました。</li>
  </ul></li>
  </ul></td>
</tr>
<tr>
  <td>2019 年 5 月 28 日</td>
  <td><ul>
  <li><strong>クラスター・アドオンの変更ログ</strong>: [ALB `nginx-ingress` イメージがビルド 462 に](/docs/containers?topic=containers-cluster-add-ons-changelog)更新されました。</li>
  <li><strong>レジストリーのトラブルシューティング</strong>: クラスター作成時にエラーが発生したためにクラスターが {{site.data.keyword.registryfull}} からイメージをプルできない場合の[トラブルシューティング・トピック](/docs/containers?topic=containers-cs_troubleshoot_clusters#ts_image_pull_create)が追加されました。
  </li>
  <li><strong>ストレージのトラブルシューティング</strong>: <ul>
  <li>[永続ストレージ障害のデバッグ](/docs/containers?topic=containers-cs_troubleshoot_storage#debug_storage)に関するトピックが追加されました。</li>
  <li>[許可の欠落による PVC 作成の失敗](/docs/containers?topic=containers-cs_troubleshoot_storage#missing_permissions)に関するトラブルシューティング・トピックが追加されました。</li>
  </ul></li>
  </ul></td>
</tr>
<tr>
  <td>2019 年 5 月 23 日</td>
  <td><ul>
  <li><strong>CLI リファレンス</strong>: {{site.data.keyword.containerlong_notm}} CLI プラグインの[バージョン 0.3.28 のリリース](/docs/containers?topic=containers-cs_cli_changelog)に対する複数の変更を反映するように [CLI リファレンス・ページ](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli)が更新されました。</li>
  <li><strong>クラスター・アドオンの変更ログ</strong>: [ALB `nginx-ingress` イメージがビルド 457 に](/docs/containers?topic=containers-cluster-add-ons-changelog)更新されました。</li>
  <li><strong>クラスターおよびワーカーの状態</strong>: [ロギングおよびモニタリングのページ](/docs/containers?topic=containers-health#states)が更新され、クラスターおよびワーカー・ノードの状態に関する参照表が追加されました。</li>
  <li><strong>クラスターの計画および作成</strong>: 以下のページで、クラスターの計画、作成、および削除とネットワークの計画に関する情報を見つけることができるようになりました。
    <ul><li>[クラスター・ネットワークのセットアップの計画](/docs/containers?topic=containers-plan_clusters)</li>
    <li>[高可用性のためのクラスターの計画](/docs/containers?topic=containers-ha_clusters)</li>
    <li>[ワーカー・ノードのセットアップの計画](/docs/containers?topic=containers-planning_worker_nodes)</li>
    <li>[クラスターの作成](/docs/containers?topic=containers-clusters)</li>
    <li>[クラスターへのワーカー・ノードとゾーンの追加](/docs/containers?topic=containers-add_workers)</li>
    <li>[クラスターの削除](/docs/containers?topic=containers-remove)</li>
    <li>[サービス・エンドポイントまたは VLAN 接続の変更](/docs/containers?topic=containers-cs_network_cluster)</li></ul>
  </li>
  <li><strong>クラスター・バージョンの更新</strong>: マスター・バージョンがサポートされる最も古いバージョンより 3 つ以上前のバージョンである場合はクラスターを更新できないことを示すように[サポートされないバージョンのポリシー](/docs/containers?topic=containers-cs_versions)が更新されました。クラスターが**サポートされていない**かどうかを確認するには、クラスターをリストするときに**「状態」**フィールドを確認します。</li>
  <li><strong>Istio</strong>: [Istio ページ](/docs/containers?topic=containers-istio)が更新され、プライベート VLAN のみに接続されたクラスターでは Istio が機能しないという制限が削除されました。Istio 管理対象アドオンの更新が完了した後に TLS セクションを使用する Istio ゲートウェイを再作成するためのステップが、[管理対象アドオンの更新に関するトピック](/docs/containers?topic=containers-managed-addons#updating-managed-add-ons)に追加されました。</li>
  <li><strong>よく閲覧されたトピック</strong>: よく閲覧されたトピックは、{{site.data.keyword.containershort_notm}} に固有の新機能と更新に関するこのリリース・ノート・ページに置き換えられました。{{site.data.keyword.Bluemix_notm}} 製品の最新情報については、[発表](https://www.ibm.com/cloud/blog/announcements)を確認してください。</li>
  </ul></td>
</tr>
<tr>
  <td>2019 年 5 月 20 日</td>
  <td><ul>
  <li><strong>バージョンの変更ログ</strong>: [ワーカー・ノードのフィックスパックの変更ログ](/docs/containers?topic=containers-changelog)が追加されました。</li>
  </ul></td>
</tr>
<tr>
  <td>2019 年 5 月 16 日</td>
  <td><ul>
  <li><strong>CLI reference</strong>: [CLI リファレンス・ページ](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli)が更新されて、`logging-collect` コマンドの COS エンドポイントが追加され、`apiserver-refresh` によって Kubernetes マスター・コンポーネントが再始動されることが明記されました。</li>
  <li><strong>非サポート: Kubernetes バージョン 1.10</strong>: [Kubernetes バージョン 1.10](/docs/containers?topic=containers-cs_versions#cs_v114) はサポートされなくなりました。</li>
  </ul></td>
</tr>
<tr>
  <td>2019 年 5 月 15 日</td>
  <td><ul>
  <li><strong>デフォルトの Kubernetes バージョン</strong>: デフォルトの Kubernetes バージョンは 1.13.6 になりました。</li>
  <li><strong>サービス制限</strong>: [サービス制限のトピック](/docs/containers?topic=containers-ibm-cloud-kubernetes-service-technology#tech_limits)が追加されました。</li>
  </ul></td>
</tr>
<tr>
  <td>2019 年 5 月 13 日</td>
  <td><ul>
  <li><strong>バージョンの変更ログ</strong>: [1.14.1_1518](/docs/containers?topic=containers-changelog#1141_1518)、[1.13.6_1521](/docs/containers?topic=containers-changelog#1136_1521)、[1.12.8_1552](/docs/containers?topic=containers-changelog#1128_1552)、[1.11.10_1558](/docs/containers?topic=containers-changelog#11110_1558)、および [1.10.13_1558](/docs/containers?topic=containers-changelog#11013_1558) で新しいパッチ更新が使用可能になったことが追加されました。</li>
  <li><strong>ワーカー・ノード・フレーバー</strong>: [クラウド状況 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/status?component=containers-kubernetes&selected=status) ごとに 48 以上のコアであるすべての[仮想マシン・ワーカー・ノード・フレーバー](/docs/containers?topic=containers-planning_worker_nodes#vm)が削除されました。引き続き、48 以上のコアを持つ[ベアメタル・ワーカー・ノード](/docs/containers?topic=containers-plan_clusters#bm)をプロビジョンできます。</li></ul></td>
</tr>
<tr>
  <td>2019 年 5 月 8 日</td>
  <td><ul>
  <li><strong>API</strong>: [グローバル API swagger の資料 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://containers.cloud.ibm.com/global/swagger-global-api/#/) へのリンクが追加されました。</li>
  <li><strong>Cloud Object Storage</strong>: {{site.data.keyword.containerlong_notm}} クラスター内の [Cloud Object Storage に関するトラブルシューティング・ガイドが追加されました](/docs/containers?topic=containers-cs_troubleshoot_storage#cos_pvc_pending)。</li>
  <li><strong>Kubernetes 戦略</strong>: [アプリを {{site.data.keyword.containerlong_notm}} に移動する前に持っていると好ましい知識および技術スキル](/docs/containers?topic=containers-strategy#knowledge)に関するトピックが追加されました。</li>
  <li><strong>Kubernetes バージョン 1.14</strong>: [Kubernetes 1.14 リリース](/docs/containers?topic=containers-cs_versions#cs_v114)が認定されたことが追加されました。</li>
  <li><strong>リファレンス・トピック</strong>: [ユーザー・アクセス](/docs/containers?topic=containers-access_reference)と [CLI](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli) のリファレンス・ページで、さまざまなサービス・バインディング、`logging`、および `nlb` 操作に関する情報が更新されました。</li></ul></td>
</tr>
<tr>
  <td>2019 年 5 月 7 日</td>
  <td><ul>
  <li><strong>クラスター DNS プロバイダー</strong>: Kubernetes 1.14 以降を実行するクラスターでは CoreDNS のみサポートされるようになったため、[CoreDNS の利点が説明されています](/docs/containers?topic=containers-cluster_dns)。</li>
  <li><strong>エッジ・ノード</strong>: [エッジ・ノード](/docs/containers?topic=containers-edge)に対するプライベート・ロード・バランサー・サポートが追加されました。</li>
  <li><strong>フリー・クラスター</strong>: [フリー・クラスター](/docs/containers?topic=containers-regions-and-zones#regions_free)がサポートされる場所が明記されました。</li>
  <li><strong>新着情報! 統合</strong>: [{{site.data.keyword.Bluemix_notm}} サービスとサード・パーティーの統合](/docs/containers?topic=containers-ibm-3rd-party-integrations)、[一般的な統合](/docs/containers?topic=containers-supported_integrations)、および[パートナーシップ](/docs/containers?topic=containers-service-partners)に関する情報が追加され、再編成されました。</li>
  <li><strong>新着情報! Kubernetes version 1.14</strong>: クラスターを作成するか、[Kubernetes 1.14](/docs/containers?topic=containers-cs_versions#cs_v114) に更新します。</li>
  <li><strong>非推奨 Kubernetes バージョン 1.11</strong>: [Kubernetes 1.11](/docs/containers?topic=containers-cs_versions#cs_v111) をサポートされなくなる前に実行していたクラスターを[更新](/docs/containers?topic=containers-update)します。</li>
  <li><strong>許可</strong>: FAQ、[クラスター・ユーザーには、どのようなアクセス・ポリシーを付与しますか?](/docs/containers?topic=containers-faqs#faq_access) が追加されました。</li>
  <li><strong>ワーカー・プール</strong>: [既存のワーカー・プールにラベルを適用](/docs/containers?topic=containers-add_workers#worker_pool_labels)する方法に関する説明が追加されました。</li>
  <li><strong>リファレンス・トピック</strong>: Kubernetes 1.14 などの新機能をサポートするために、[変更ログ](/docs/containers?topic=containers-changelog#changelog)の参照ページが更新されました。</li></ul></td>
</tr>
<tr>
  <td>2019 年 5 月 1 日</td>
  <td><strong>インフラストラクチャー・アクセスの割り当て</strong>: [サポート・ケースを開くための IAM 許可を割り当てるステップ](/docs/containers?topic=containers-users#infra_access)が修正されました。</td>
</tr>
</tbody></table>


