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


# CLI の変更ログ
{: #cs_cli_changelog}

`ibmcloud` CLI およびプラグインの更新が使用可能になると、端末に通知が表示されます。 使用可能なすべてのコマンドおよびフラグを使用できるように、CLI を最新の状態に保つようにしてください。
{:shortdesc}

{{site.data.keyword.containerlong}} CLI プラグインをインストールするには、[CLI のインストール](/docs/containers?topic=containers-cs_cli_install#cs_cli_install_steps)を参照してください。

{{site.data.keyword.containerlong_notm}} CLI プラグインの各バージョンにおける変更の要約については、以下の表を参照してください。

<table summary="{{site.data.keyword.containerlong_notm}} CLI プラグインのバージョン変更の概要">
<caption>{{site.data.keyword.containerlong_notm}} CLI プラグインの変更ログ</caption>
<thead>
<tr>
<th>バージョン</th>
<th>リリース日付</th>
<th>変更内容</th>
</tr>
</thead>
<tbody>
<tr>
<td>0.3.34</td>
<td>2019 年 5 月 31 日</td>
<td>IBM Cloud クラスターで Red Hat OpenShift を作成するためのサポートが追加されました。<ul>
<li>`cluster-create` コマンドの `--kube-version` フラグで OpenShift バージョンのサポートが追加されました。例えば、標準の OpenShift クラスターを作成するには、`cluster-create` コマンドで `--kube-version 3.11_openshift` と入力します。</li>
<li>サポートされるすべての Kubernetes バージョンと OpenShift バージョンをリストする `versions` コマンドが追加されました。</li>
<li>`kube-versions` コマンドが非推奨になりました。</li>
</ul></td>
</tr>
<tr>
<td>0.3.33</td>
<td>2019 年 5 月 30 日</td>
<td><ul>
<li>Kubernetes 環境変数を Windows PowerShell 形式で取得するため、`cluster-config` コマンドに <code>--powershell</code> フラグが追加されました。</li>
<li>`region-get` コマンド、`region-set` コマンド、および `regions` コマンドが非推奨になりました。詳しくは、[グローバル・エンドポイント機能](/docs/containers?topic=containers-regions-and-zones#endpoint)を参照してください。</li>
</ul></td>
</tr>
<tr>
<td>0.3.28</td>
<td>2019 年 5 月 23 日</td>
<td><ul><li>Ingress ALB を作成するための [<code>ibmcloud ks alb-create</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_create) コマンドが追加されました。詳しくは、[ALB のスケーリング](/docs/containers?topic=containers-ingress#scale_albs)を参照してください。</li>
<li>ターゲットになっているリソース・グループと地域の [IBM Cloud インフラストラクチャー (SoftLayer) ポートフォリオにアクセス](/docs/containers?topic=containers-users#api_key)できるようにする資格情報に、推奨または必須のインフラストラクチャー許可の欠落がないかどうかを確認するための [<code>ibmcloud ks infra-permissions-get</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#infra_permissions_get) コマンドが追加されました。</li>
<li>ワーカー・プール・メタデータのパブリック VLAN を設定解除し、そのワーカー・プール・ゾーン内の後続のワーカー・ノードがプライベート VLAN にのみ接続されるようにするため、`zone-network-set` コマンドに <code>--private-only</code> フラグが追加されました。</li>
<li>`worker-update` コマンドから <code>--force-update</code> フラグが削除されました。</li>
<li>`albs` コマンドと `alb-get` コマンドの出力に **VLAN ID** 列が追加されました。</li>
<li>複数ゾーン対応のゾーンを指定するため、`supported-locations` コマンドの出力に **Multizone Metro** 列が追加されました。</li>
<li>`cluster-get` コマンドの出力に **Master State** フィールドと **Master Health** フィールドが追加されました。詳しくは、[マスターの状態](/docs/containers?topic=containers-health#states_master)を参照してください。</li>
<li>ヘルプ・テキストの翻訳を更新しました。</li>
</ul></td>
</tr>
<tr>
<td>0.3.8</td>
<td>2019 年 4 月 30 日</td>
<td>[グローバル・エンドポイント機能](/docs/containers?topic=containers-regions-and-zones#endpoint)のサポートがバージョン `0.3` で追加されました。デフォルトで、すべてのロケーションのすべての {{site.data.keyword.containerlong_notm}} リソースを表示および管理できるようになりました。リソースを操作するために地域をターゲットにする必要はありません。</li>
<ul><li>{{site.data.keyword.containerlong_notm}} でサポートされるすべてのロケーションをリストするための [<code>ibmcloud ks supported-locations</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_supported-locations) コマンドが追加されました。</li>
<li>リソースを 1 つ以上のロケーションによってフィルターに掛けるため、`clusters` コマンドと `zones` コマンドに <code>--locations</code> フラグが追加されました。</li>
<li>`credential-set/unset/get` コマンド、`api-key-reset` コマンド、および `vlan-spanning-get` コマンドに <code>--region</code> フラグが追加されました。これらのコマンドを実行するには、`--region` フラグで地域を指定する必要があります。</li></ul></td>
</tr>
<tr>
<td>0.2.102</td>
<td>2019 年 4 月 15 日</td>
<td>ネットワーク・ロード・バランサー (NLB) IP アドレスのホスト名を登録および管理する [`ibmcloud ks nlb-dns` コマンド・グループ](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#nlb-dns)と NLB ホスト名のヘルス・チェック・モニターを作成および変更する [`ibmcloud ks nlb-dns-monitor` コマンド・グループ](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-monitor)が追加されました。 詳しくは、[DNS ホスト名への NLB IP の登録](/docs/containers?topic=containers-loadbalancer#loadbalancer_hostname_dns)を参照してください。
</td>
</tr>
<tr>
<td>0.2.99</td>
<td>2019 年 4 月 9 日</td>
<td><ul>
<li>ヘルプ・テキストを更新しました。</li>
<li>Go バージョンを 1.12.2 に更新しました。</li>
</ul></td>
</tr>
<tr>
<td>0.2.95</td>
<td>2019 年 4 月 3 日</td>
<td><ul>
<li>管理対象クラスター・アドオンのバージョン管理サポートが追加されました。</li>
<ul><li>[<code>ibmcloud ks addon-versions</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_addon_versions) コマンドを追加します。</li>
<li>[ibmcloud ks cluster-addon-enable](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_addon_enable) コマンドに <code>--version</code> フラグを追加します。</li></ul>
<li>ヘルプ・テキストの翻訳を更新しました。</li>
<li>ヘルプ・テキスト内の資料への短縮リンクを更新しました。</li>
<li>JSON エラー・メッセージが正しくない形式で出力されるバグを修正しました。</li>
<li>一部のコマンドでサイレント・フラグ (`-s`) を使用するとエラーが出力されないバグを修正しました。</li>
</ul></td>
</tr>
<tr>
<td>0.2.80</td>
<td>2019 年 3 月 19 日</td>
<td><ul>
<li>[VRF 対応アカウント](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started)で Kubernetes バージョン 1.11 以降を実行する標準クラスターで、[サービス・エンドポイントを使用したマスターからワーカーへの通信](/docs/containers?topic=containers-plan_clusters#workeruser-master)を有効にするためのサポートが追加されました。<ul>
<li>`--private-service-endpoint` フラグと `--public-service-endpoint` フラグを [<code>ibmcloud ks cluster-create</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_create) コマンドに追加。</li>
<li>**Public Service Endpoint URL** フィールドと **Private Service Endpoint URL** フィールドを <code>ibmcloud ks cluster-get</code> の出力に追加。</li>
<li>[<code>ibmcloud ks cluster-feature-enable private-service-endpoint</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable_private_service_endpoint) コマンドを追加します。</li>
<li>[<code>ibmcloud ks cluster-feature-enable public-service-endpoint</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable_public_service_endpoint) コマンドを追加します。</li>
<li>[<code>ibmcloud ks cluster-feature-disable public-service-endpoint</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_disable) コマンドを追加します。</li>
</ul></li>
<li>資料および翻訳を更新。</li>
<li>Go バージョンを 1.11.6 に更新しました。</li>
<li>偶発的なネットワーキングの問題 (macOS ユーザーの場合) を解決しました。</li>
</ul></td>
</tr>
<tr>
<td>0.2.75</td>
<td>2019 年 3 月 14 日</td>
<td><ul><li>エラー出力で生の HTML を非表示にしました。</li>
<li>ヘルプ・テキスト内のタイプミスを修正しました。</li>
<li>ヘルプ・テキストの翻訳を修正しました。</li>
</ul></td>
</tr>
<tr>
<td>0.2.61</td>
<td>2019 年 2 月 26 日</td>
<td><ul>
<li>`cluster-pull-secret-apply` コマンドが追加されました。このコマンドは、クラスターの IAM サービス ID、ポリシー、API キー、イメージ・プル・シークレットを作成して、`default` Kubernetes 名前空間で実行されるコンテナーが IBM Cloud Container Registry からイメージをプルできるようにします。 新規クラスターの場合、デフォルトで、IAM 資格情報を使用するイメージ・プル・シークレットが作成されます。 既存のクラスターを更新する際、またはクラスターで作成時にイメージ・プル・シークレット・エラーが発生した場合に、このコマンドを使用してください。 詳しくは、[資料](https://test.cloud.ibm.com/docs/containers?topic=containers-images#cluster_registry_auth)を参照してください。</li>
<li>`ibmcloud ks init` の失敗によってヘルプ出力が表示されるバグを修正しました。</li>
</ul></td>
</tr>
<tr>
<td>0.2.53</td>
<td>2019 年 2 月 19 日</td>
<td><ul><li>`ibmcloud ks api-key-reset`、`ibmcloud ks credential-get/set`、および `ibmcloud ks vlan-spanning-get` で地域が無視されるバグを修正しました。</li>
<li>`ibmcloud ks worker-update` のパフォーマンスが向上しました。</li>
<li>`ibmcloud ks cluster-addon-enable` プロンプトでアドオンのバージョンが追加されました。</li>
</ul></td>
</tr>
<tr>
<td>0.2.44</td>
<td>2019 年 2 月 8 日</td>
<td><ul>
<li>`--skip-rbac` オプションが `ibmcloud ks cluster-config` コマンドに追加されました。このオプションにより、{{site.data.keyword.Bluemix_notm}}IAM サービス・アクセス役割に基づくユーザー Kubernetes RBAC 役割をクラスター構成に追加する処理が省略されます。 [独自の Kubernetes RBAC 役割を管理](/docs/containers?topic=containers-users#rbac)する場合にのみ、このオプションを組み込んでください。 [{{site.data.keyword.Bluemix_notm}} IAM サービス・アクセス役割](/docs/containers?topic=containers-access_reference#service)を使用してすべての RBAC ユーザーを管理する場合は、このオプションを組み込まないでください。</li>
<li>Go バージョンを 1.11.5 に更新しました。</li>
</ul></td>
</tr>
<tr>
<td>0.2.40</td>
<td>2019 年 2 月 6 日</td>
<td><ul>
<li>[<code>ibmcloud ks cluster-addons</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_addons)、[<code>ibmcloud ks cluster-addon-enable</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_addon_enable)、および [<code>ibmcloud ks cluster-addon-disable</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_addon_disable) の各コマンドが追加されました。これらのコマンドは、管理対象クラスター・アドオン ({{site.data.keyword.containerlong_notm}} 用の [Istio](/docs/containers?topic=containers-istio) 管理対象アドオンや [Knative](/docs/containers?topic=containers-serverless-apps-knative) 管理対象アドオンなど) を操作するためのものです。</li>
<li><code>ibmcloud ks vlans</code> コマンドの {{site.data.keyword.Bluemix_dedicated_notm}} ユーザー向けのヘルプ・テキストが改善されました。</li></ul></td>
</tr>
<tr>
<td>0.2.30</td>
<td>2019 年 1 月 31 日</td>
<td>`ibmcloud ks cluster-config` のデフォルトのタイムアウト値を `500s` に引き上げました。</td>
</tr>
<tr>
<td>0.2.19</td>
<td>2019 年 1 月 16 日</td>
<td><ul><li>再設計されたベータ版の {{site.data.keyword.containerlong_notm}} プラグイン CLI を有効にする、`IKS_BETA_VERSION` 環境変数が追加されました。 再設計されたこのバージョンを試用するには、[ベータ版のコマンド構造の使用](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_beta)を参照してください。</li>
<li>`ibmcloud ks subnets` のデフォルトのタイムアウト値を `60s` に引き上げました。</li>
<li>軽微なバグと翻訳を修正しました。</li></ul></td>
</tr>
<tr>
<td>0.1.668</td>
<td>2018 年 12 月 18 日</td>
<td><ul><li>デフォルトの API エンドポイントを <code>https://containers.bluemix.net</code> から <code>https://containers.cloud.ibm.com</code> に変更しました。</li>
<li>コマンド・ヘルプとエラー・メッセージに翻訳が適切に表示されるように、バグを修正しました。</li>
<li>コマンド・ヘルプの表示が速くなりました。</li></ul></td>
</tr>
<tr>
<td>0.1.654</td>
<td>2018 年 12 月 5 日</td>
<td>資料および翻訳を更新。</td>
</tr>
<tr>
<td>0.1.638</td>
<td>2018 年 11 月 15 日</td>
<td>
<ul><li>`apiserver-refresh` コマンドに [<code>ibmcloud ks cluster-refresh</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_apiserver_refresh) 別名が追加されました。</li>
<li><code>ibmcloud ks cluster-get</code> および <code>ibmcloud ks clusters</code> の出力にリソース・グループ名を追加。</li></ul>
</td>
</tr>
<tr>
<td>0.1.635</td>
<td>2018 年 11 月 6 日</td>
<td>Ingress ALB クラスター・アドオンの自動更新を管理するためのコマンドが追加されました。<ul>
<li>[<code>ibmcloud ks alb-autoupdate-disable</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_autoupdate_disable)</li>
<li>[<code>ibmcloud ks alb-autoupdate-enable</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_autoupdate_enable)</li>
<li>[<code>ibmcloud ks alb-autoupdate-get</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_autoupdate_get)</li>
<li>[<code>ibmcloud ks alb-rollback</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_rollback)</li>
<li>[<code>ibmcloud ks alb-update</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_update)</li>
</ul></td>
</tr>
<tr>
<td>0.1.621</td>
<td>2018 年 10 月 30 日</td>
<td><ul>
<li>[<code>ibmcloud ks credential-get</code> コマンドを追加します](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credential_get)。</li>
<li>すべてのクラスター・ロギング・コマンドに <code>storage</code> ログ・ソースのサポートを追加。 詳しくは、<a href="/docs/containers?topic=containers-health#logging">クラスターおよびアプリのログ転送について</a>を参照してください。</li>
<li>[<code>ibmcloud ks cluster-config</code> コマンド](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_config)に `--network` フラグを追加。このフラグは、すべての Calico コマンドを実行するための Calico 構成ファイルをダウンロードします。</li>
<li>軽微な不具合の修正およびリファクタリング</li></ul>
</td>
</tr>
<tr>
<td>0.1.593</td>
<td>2018 年 10 月 10 日</td>
<td><ul><li><code>ibmcloud ks cluster-get</code> の出力にリソース・グループ ID を追加。</li>
<li>クラスター内の鍵管理サービス (KMS) プロバイダーとして [{{site.data.keyword.keymanagementserviceshort}} が有効になる](/docs/containers?topic=containers-encryption#keyprotect)と、<code>ibmcloud ks cluster-get</code> の出力に「KMS enabled」フィールドを追加。</li></ul></td>
</tr>
<tr>
<td>0.1.591</td>
<td>2018 年 10 月 2 日</td>
<td>[リソース・グループ](/docs/containers?topic=containers-clusters#cluster_prepare)のサポートを追加。</td>
</tr>
<tr>
<td>0.1.590</td>
<td>2018 年 10 月 1 日</td>
<td><ul>
<li>クラスター内の API サーバー・ログを収集するための [<code>ibmcloud ks logging-collect</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_collect) コマンドと [<code>ibmcloud ks logging-collect-status</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_collect_status) コマンドを追加。</li>
<li>{{site.data.keyword.keymanagementserviceshort}} をクラスター内の鍵管理サービス (KMS) プロバイダーとして有効にするための [<code>ibmcloud ks key-protect-enable</code> コマンド](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_key_protect)を追加。</li>
<li>リブートまたは再ロード開始前のマスター・ヘルス・チェックをスキップするための <code>--skip-master-health</code> フラグを [ibmcloud ks worker-reboot](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reboot) および [ibmcloud ks worker-reload](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reboot) コマンドに追加。</li>
<li><code>ibmcloud ks cluster-get</code> の出力の中の <code>Owner Email</code> を <code>Owner</code> に名前変更。</li></ul></td>
</tr>
</tbody>
</table>
