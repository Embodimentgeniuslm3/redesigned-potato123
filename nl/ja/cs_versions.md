---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-11"

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
{:note: .note}


# バージョン情報および更新操作
{: #cs_versions}

## Kubernetes バージョンのタイプ
{: #version_types}

{{site.data.keyword.containerlong}} は、複数のバージョンの Kubernetes を同時にサポートします。 最新バージョン (n) がリリースされると、2 つ前のバージョン (n-2) までサポートされます。 最新バージョンから 2 つより前のバージョン (n-3) は、まず非推奨になり、その後サポートされなくなります。
{:shortdesc}

**サポートされる Kubernetes バージョン**:
*   最新: 1.14.2 
*   デフォルト: 1.13.6
*   その他: 1.12.9

**非推奨および非サポートの Kubernetes バージョン**:
*   非推奨: 1.11
*   非サポート: 1.5、1.7、1.8、1.9、1.10 

</br>

**非推奨バージョン**: 非推奨 Kubernetes バージョンでクラスターを実行している場合は、そのバージョンが非サポート・バージョンになる前に、サポート対象バージョンの Kubernetes を確認して更新できるように最低 30 日の期間が設けられます。 非推奨の期間中もクラスターはまだ機能しますが、セキュリティーの脆弱性を修正するために、サポートされるリリースへの更新が必要になる場合があります。 例えば、ワーカー・ノードを追加および再ロードできますが、サポート終了日が 30 日以内のときは、非推奨バージョンを使用する新規クラスターは作成できません。

**非サポート・バージョン**: サポートされない Kubernetes バージョンがクラスターで実行されている場合は、後述する更新の潜在的な影響を確認し、即時に[クラスターを更新](/docs/containers?topic=containers-update#update)して、重要なセキュリティー更新とサポートを継続して受けられるようにしてください。 非サポートのクラスターは、既存のワーカー・ノードを追加したり再ロードしたりすることはできません。 ご使用のクラスターが**サポート対象外**かどうかは、`ibmcloud ks clusters` コマンドの出力または [{{site.data.keyword.containerlong_notm}} コンソール ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/kubernetes/clusters) で**「状態」**フィールドを調べることで確認できます。

クラスターが、最も古いサポートされるバージョンの 3 つ以上マイナーのバージョンになった場合は、クラスターを更新できません。代わりに、[新規クラスターを作成](/docs/containers?topic=containers-clusters#clusters)し、新規クラスターに[アプリをデプロイ](/docs/containers?topic=containers-app#app)し、サポートされないクラスターを[削除](/docs/containers?topic=containers-remove)します。<br><br>この問題を回避するには、非推奨のクラスターを現在のバージョンより 2 つ以内のサポート対象バージョン (1.11 から 1.12 など) に更新してから、最新バージョンの 1.14 に更新します。ワーカー・ノードで、マスターよりも 3 つ以上前のバージョンが実行される場合、ワーカー・ノードをマスターと同じバージョンに更新するまで、ポッドが、`MatchNodeSelector`、`CrashLoopBackOff`、`ContainerCreating` などの状態になり、失敗することがあります。非推奨のバージョンからサポートされるバージョンに更新した後は、クラスターで、通常の操作を再開し、引き続きサポートを受けることができます。
{: important}

</br>

クラスターのサーバー・バージョンを確認するには、以下のコマンドを実行します。
```
kubectl version  --short | grep -i server
```
{: pre}

出力例:
```
Server Version: v1.13.6+IKS
```
{: screen}


## 更新タイプ
{: #update_types}

Kubernetes クラスターには、メジャー、マイナー、およびパッチという 3 つのタイプの更新があります。
{:shortdesc}

|更新タイプ|バージョン・ラベルの例|更新の実行者|影響
|-----|-----|-----|-----|
|メジャー|1.x.x|お客様|スクリプトやデプロイメントを含むクラスターの操作変更。|
|マイナー|x.9.x|お客様|スクリプトやデプロイメントを含むクラスターの操作変更。|
|パッチ|x.x.4_1510|IBM とお客様|Kubernetes のパッチ、および、セキュリティー・パッチやオペレーティング・システム・パッチなどの他の {{site.data.keyword.Bluemix_notm}} Provider コンポーネントの更新。 マスターは IBM が自動的に更新しますが、ワーカー・ノードへのパッチはお客様が適用する必要があります。 パッチについて詳しくは、以下のセクションを参照してください。|
{: caption="Kubernetes 更新の影響" caption-side="top"}

更新が利用可能になると、`ibmcloud ks workers --cluster <cluster>` や `ibmcloud ks worker-get --cluster <cluster> --worker <worker>` コマンドなどを使用してワーカー・ノードに関する情報を表示したときに通知されます。
-  **メジャー更新とマイナー更新 (1.x)**: まず[マスター・ノードを更新して](/docs/containers?topic=containers-update#master)、それから[ワーカー・ノードを更新します](/docs/containers?topic=containers-update#worker_node)。 ワーカー・ノードは、マスターよりも大きい Kubernetes メジャー・バージョンまたはマイナー・バージョンを実行できません。
   - Kubernetes マスターを更新できるのは 2 つ先のマイナー・バージョンまでです。例えば、現在のマスターがバージョン 1.11 であり 1.14 に更新する計画の場合、まず 1.12 に更新する必要があります。
   - 少なくともクラスターの `major.minor` バージョンに一致する `kubectl` CLI バージョンを使用しないと、予期しない結果になる可能性があります。 Kubernetes クラスターと [CLI のバージョン](/docs/containers?topic=containers-cs_cli_install#kubectl)を最新の状態に保つようにしてください。
-  **パッチ更新 (x.x.4_1510)**: 各パッチの変更内容については、[バージョンの変更ログ](/docs/containers?topic=containers-changelog)に記載しています。 マスターのパッチは自動的に適用されますが、ワーカー・ノードのパッチはユーザーが更新を開始します。 ワーカー・ノードはマスターよりも大きいパッチ・バージョンを実行することもできます。 更新が利用可能になると、マスター・ノードやワーカー・ノードに関する情報を {{site.data.keyword.Bluemix_notm}} コンソールまたは CLI で `ibmcloud ks clusters`、`cluster-get`、`workers`、`worker-get` などのコマンドを使用して表示したときに通知されます。
   - **ワーカー・ノードのパッチ**: 更新が使用可能かどうかを毎月確認し、`ibmcloud ks worker-update` [コマンド](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update)または `ibmcloud ks worker-reload` [コマンド](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)を使用して、これらのセキュリティー・パッチおよびオペレーティング・システム・パッチを適用してください。 更新中や再ロード中にワーカー・ノード・マシンのイメージが再作成されるので、[ワーカー・ノードの外部に保管](/docs/containers?topic=containers-storage_planning#persistent_storage_overview)していないデータは削除されます。
   - **マスターのパッチ**: マスターのパッチは、数日にわたって自動的に適用されるため、マスターに適用される前に、マスターのパッチ・バージョンが使用可能であると表示される場合があります。 また、更新の自動化では、正常な状態でないクラスターや現在進行中の操作があるクラスターはスキップされます。 必要に応じて、IBM は特定のマスターのフィックスパック (変更ログに記載されているように、マスターが 1 つ前のマイナー・バージョンから更新される場合にのみ必要なパッチなど) に対して、自動更新を無効にする場合があります。 これらのいずれかの場合、自動更新が適用されるのを待たずに、`ibmcloud ks cluster-update` [コマンド](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_update)を使用して手動で確実に更新することができます。

</br>

{: #prep-up}
この情報は、クラスターを前のバージョンから新しいバージョンに更新した場合に、デプロイされているアプリに影響を与える可能性がある更新についてまとめたものです。
-  バージョン 1.14 の[準備アクション](#cs_v114)。
-  バージョン 1.13 の[準備アクション](#cs_v113)。
-  バージョン 1.12 の[準備アクション](#cs_v112)。
-  **非推奨**: バージョン 1.11 [準備アクション](#cs_v111)。
-  非サポートのバージョンの[アーカイブ](#k8s_version_archive)。

<br/>

変更内容の完全なリストは、以下の情報を参照してください。
* [Kubernetes の変更ログ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG.md)。
* [IBM バージョンの変更ログ](/docs/containers?topic=containers-changelog)。

</br>

## リリース履歴
{: #release-history}

以下の表は、{{site.data.keyword.containerlong_notm}} バージョンのリリース履歴の記録です。 特定のリリースが非サポート・リリースになる大まかな時間フレームを推測するなど、計画の際にはこの情報を参考にしてください。 Kubernetes コミュニティーがバージョン更新をリリースすると、IBM チームは {{site.data.keyword.containerlong_notm}} 環境用にそのリリースを強化してテストするプロセスを開始します。 リリース日およびサポート終了日は、これらのテストの結果、コミュニティーの更新、セキュリティー・パッチ、およびバージョン間のテクノロジーの変更に依存します。 `n-2` のバージョン・サポート・ポリシーに従い、クラスター・マスターとワーカー・ノードのバージョンを最新に保つように計画してください。
{: shortdesc}

{{site.data.keyword.containerlong_notm}} の一般提供が初めて開始されたときには、Kubernetes バージョン 1.5 が使用されていました。 リリース予定日またはサポート終了予定日は変更されることがあります。 バージョン更新の準備手順に進むには、バージョン番号をクリックしてください。

剣標 (`†`) の付いた日付は暫定的な日付であり、変更されることがあります。
{: important}

<table summary="この表は、{{site.data.keyword.containerlong_notm}} のリリース履歴を示しています。">
<caption>{{site.data.keyword.containerlong_notm}} のリリース履歴。</caption>
<col width="20%" align="center">
<col width="20%">
<col width="30%">
<col width="30%">
<thead>
<tr>
<th>サポートされているかどうか</th>
<th>バージョン</th>
<th>{{site.data.keyword.containerlong_notm}}<br>リリース日付</th>
<th>{{site.data.keyword.containerlong_notm}}<br>サポート終了日</th>
</tr>
</thead>
<tbody>
<tr>
  <td><img src="images/checkmark-filled.png" align="left" width="32" style="width:32px;" alt="このバージョンはサポートされています。"/></td>
  <td>[1.14](#cs_v114)</td>
  <td>2019 年 5 月 7 日</td>
  <td>2020 年 3 月`†`</td>
</tr>
<tr>
  <td><img src="images/checkmark-filled.png" align="left" width="32" style="width:32px;" alt="このバージョンはサポートされています。"/></td>
  <td>[1.13](#cs_v113)</td>
  <td>2019 年 2 月 5 日</td>
  <td>2019 年 12 月`†`</td>
</tr>
<tr>
  <td><img src="images/checkmark-filled.png" align="left" width="32" style="width:32px;" alt="このバージョンはサポートされています。"/></td>
  <td>[1.12](#cs_v112)</td>
  <td>2018 年 11 月 7 日</td>
  <td>2019 年 9 月`†`</td>
</tr>
<tr>
  <td><img src="images/warning-filled.png" align="left" width="32" style="width:32px;" alt="このバージョンは推奨されていません。"/></td>
  <td>[1.11](#cs_v111)</td>
  <td>2018 年 8 月 14 日</td>
  <td>2019 年 6 月 27 日`†`</td>
</tr>
<tr>
  <td><img src="images/close-filled.png" align="left" width="32" style="width:32px;" alt="このバージョンはサポートされていません。"/></td>
  <td>[1.10](#cs_v110)</td>
  <td>2018 年 5 月 1 日</td>
  <td>2019 年 5 月 16 日</td>
</tr>
<tr>
  <td><img src="images/close-filled.png" align="left" width="32" style="width:32px;" alt="このバージョンはサポートされていません。"/></td>
  <td>[1.9](#cs_v19)</td>
  <td>2018 年 2 月 8 日</td>
  <td>2018 年 12 月 27 日</td>
</tr>
<tr>
  <td><img src="images/close-filled.png" align="left" width="32" style="width:32px;" alt="このバージョンはサポートされていません。"/></td>
  <td>[1.8](#cs_v18)</td>
  <td>2017 年 11 月 8 日</td>
  <td>2018 年 9 月 22 日</td>
</tr>
<tr>
  <td><img src="images/close-filled.png" align="left" width="32" style="width:32px;" alt="このバージョンはサポートされていません。"/></td>
  <td>[1.7](#cs_v17)</td>
  <td>2017 年 9 月 19 日</td>
  <td>2018 年 6 月 21 日</td>
</tr>
<tr>
  <td><img src="images/close-filled.png" align="left" width="32" style="width:32px;" alt="このバージョンはサポートされていません。"/></td>
  <td>1.6</td>
  <td>N/A</td>
  <td>N/A</td>
</tr>
<tr>
  <td><img src="images/close-filled.png" align="left" width="32" style="width:32px;" alt="このバージョンはサポートされていません。"/></td>
  <td>[1.5](#cs_v1-5)</td>
  <td>2017 年 5 月 23 日</td>
  <td>2018 年 4 月 4 日</td>
</tr>
</tbody>
</table>

<br />


## バージョン 1.14
{: #cs_v114}

<p><img src="images/certified_kubernetes_1x14.png" style="padding-right: 10px;" align="left" alt="このバッジは、{{site.data.keyword.containerlong_notm}} が Kubernetes バージョン 1.14 の認定を受けたことを示しています。"/> {{site.data.keyword.containerlong_notm}} は、CNCF Kubernetes Software Conformance Certification プログラムのもとで認定を受けたバージョン 1.14 の Kubernetes 製品です。__</p>

Kubernetes を前のバージョンから 1.14 に更新する場合に必要な可能性がある変更作業について説明します。
{: shortdesc}

Kubernetes 1.14 では、探索のための新機能が導入されています。Kubernetes リソースの YAML 構成の書き込み、カスタマイズ、および再利用に使用できる新しい [`kustomize` プロジェクト ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes-sigs/kustomize) を試してください。または、新しい [`kubectl` CLI の資料 ![ 外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubectl.docs.kubernetes.io/) を参照してください。
{: tip}

### マスターの前に行う更新
{: #114_before}

以下の表に、Kubernetes マスターを更新する前に実行する必要があるアクションを示します。
{: shortdesc}

<table summary="バージョン 1.14 の Kubernetes の更新">
<caption>マスターを Kubernetes 1.14 に更新する前に行う変更</caption>
<thead>
<tr>
<th>タイプ</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>CRI ポッドのログ・ディレクトリー構造の変更</td>
<td>コンテナー・ランタイム・インターフェース (CRI) によって、ポッドのログ・ディレクトリー構造が `/var/log/pods/<UID>` から `/var/log/pods/<NAMESPACE_NAME_UID>` に変更されました。アプリが Kubernetes と CRI をバイパスし、ワーカー・ノード上のポッドのログに直接アクセスする場合は、両方のディレクトリー構造を処理するように更新します。`kubectl logs` を実行するなど、Kubernetes を介してポッドのログにアクセスする場合は、この変更の影響を受けません。</td>
</tr>
<tr>
<td>ヘルス・チェックがリダイレクトに従わなくなる</td>
<td>`HTTPGetAction` を使用するヘルス・チェックの Liveness Probe および Readiness Probe が、元のプローブ要求とは異なるホスト名へのリダイレクトには従わなくなります。代わりに、これらの非ローカルのリダイレクトによって、`Success` 応答を返し、リダイレクトが無視されたことを示す理由 `ProbeWarning` を含むイベントが生成されます。これまでリダイレクトに依存して、別のホスト名のエンドポイントに対してヘルス・チェックを実行していた場合は、`kubelet` の外部でヘルス・チェック・ロジックを実行する必要があります。例えば、プローブ要求をリダイレクトする代わりに、外部エンドポイントをプロキシーすることができます。</td>
</tr>
<tr>
<td>非サポート: KubeDNS クラスター DNS プロバイダー</td>
<td>CoreDNS は、Kubernetes バージョン 1.14 以降を実行するクラスターで現在サポートされる唯一のクラスター DNS プロバイダーです。KubeDNS をクラスター DNS プロバイダーとして使用している既存のクラスターをバージョン 1.14 に更新する場合、KubeDNS は更新中に自動的に CoreDNS にマイグレーションされます。したがって、クラスターを更新する前に、[クラスター DNS プロバイダーとして CoreDNS をセットアップ](/docs/containers?topic=containers-cluster_dns#set_coredns)し、テストすることを検討してください。<br><br>Kubernetes サービスの `ExternalName` フィールドにドメイン・ネームを入力できるように、CoreDNS は [クラスターの DNS 仕様 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/dns/blob/master/docs/specification.md#25---records-for-external-name-services) をサポートしています。 以前のクラスター DNS プロバイダーである KubeDNS は、クラスターの DNS 仕様に準拠していないので、`ExternalName` には IP アドレスが使用されます。 Kubernetes のサービスで DNS ではなく IP アドレスを使用している場合は、引き続き機能させるために `ExternalName` を DNS に更新する必要があります。</td>
</tr>
<tr>
<td>非サポート: Kubernetes の `Initializers` のアルファ版の機能</td>
<td>Kubernetes の `Initializers` のアルファ版の機能、`admissionregistration.k8s.io/v1alpha1` API バージョン、`Initializers` アドミッション・コントローラー・プラグイン、および `metadata.initializers` API フィールドの使用は、削除されます。`Initializers` を使用する場合は、[Kubernetes アドミッション Webhook ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/reference/access-authn-authz/extensible-admission-controllers/) を使用するように切り替えて、クラスターを更新する前に既存の `InitializerConfiguration` API オブジェクトをすべて削除します。</td>
</tr>
<tr>
<td>非サポート: ノードのアルファ版のテイント</td>
<td>`node.alpha.kubernetes.io/notReady` と `node.alpha.kubernetes.io/unreachable` のテイントの使用は、サポートされなくなります。これらのテイントに依存する場合は、代わりに `node.kubernetes.io/not-ready` および `node.kubernetes.io/unreachable` のテイントを使用するようにアプリを更新します。</td>
</tr>
<tr>
<td>非サポート: Kubernetes API Swagger の資料</td>
<td>`swagger/*`、`/swagger.json`、および `/swagger-2.0.0.pb-v1` のスキーマ API 資料は、`/openapi/v2` のスキーマ API 資料があるため、削除されました。Kubernetes バージョン 1.10 で OpenAPI の資料が使用可能になったため、Swagger の資料は非推奨になりました。また、Kubernetes API サーバーが、集約された API サーバーの `/openapi/v2` エンドポイントからのみ OpenAPI スキーマを集約するようになりました。`/swagger.json` から集約するフォールバックは削除されます。Kubernetes API 拡張を提供するアプリをインストールした場合は、アプリが `/openapi/v2` のスキーマ API 資料をサポートするようにしてください。</td>
</tr>
<tr>
<td>非サポートおよび非推奨: メトリックの選択</td>
<td>[削除された Kubernetes メトリックおよび非推奨になった Kubernetes メトリック ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG-1.14.md#removed-and-deprecated-metrics) を参照してください。これらの非推奨のメトリックのいずれかを使用する場合は、使用可能な後継のメトリックに変更してください。</td>
</tr>
</tbody>
</table>

### マスターの更新後に行う操作
{: #114_after}

以下の表に、Kubernetes マスターを更新した後に実行する必要があるアクションを示します。
{: shortdesc}

<table summary="バージョン 1.14 の Kubernetes の更新">
<caption>マスターを Kubernetes 1.14 に更新した後に行う変更</caption>
<thead>
<tr>
<th>タイプ</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>非サポート: `kubectl --show-all`</td>
<td>`--show-all` とその省略表現の `-a` のフラグは、サポートされなくなりました。これらのフラグに依存したスクリプトがある場合は更新してください。</td>
</tr>
<tr>
<td>非認証ユーザーの Kubernetes のデフォルトの RBAC ポリシー</td>
<td>Kubernetes のデフォルトの役割ベースのアクセス制御 (RBAC) ポリシーで、[非認証ユーザーに対するディスカバリー API および許可確認 API ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#discovery-roles) へのアクセスが認可されなくなりました。この変更は、新規バージョン 1.14 のクラスターにのみ適用されます。以前のバージョンからクラスターを更新した場合、非認証ユーザーはディスカバリー API および許可確認 API に引き続きアクセスできます。非認証ユーザーのデフォルトがよりセキュアになるように更新する場合は、`system:basic-user` および `system:discovery` のクラスター役割バインディングから `system:unauthenticated` グループを削除します。</td>
</tr>
<tr>
<td>非推奨: `pod_name` および `container_name` のラベルを使用する Prometheus 照会</td>
<td>`pod_name` または `container_name` のラベルに一致する Prometheus 照会を、代わりに `pod` または `container` のラベルを使用するように更新します。これらの非推奨ラベルを使用する照会の例としては、kubelet プローブ・メトリックがあります。非推奨の `pod_name` と `container_name` のラベルは、次の Kubernetes リリースではサポートされなくなります。</td>
</tr>
</tbody>
</table>

<br />


## バージョン 1.13
{: #cs_v113}

<p><img src="images/certified_kubernetes_1x13.png" style="padding-right: 10px;" align="left" alt="このバッジは、{{site.data.keyword.containerlong_notm}} が Kubernetes バージョン 1.13 の認定を受けたことを示しています。"/> {{site.data.keyword.containerlong_notm}} は、CNCF Kubernetes Software Conformance Certification プログラムのもとで認定を受けたバージョン 1.13 の Kubernetes 製品です。__</p>

Kubernetes を前のバージョンから 1.13 に更新する場合に必要な可能性がある変更作業について説明します。
{: shortdesc}

### マスターの前に行う更新
{: #113_before}

以下の表に、Kubernetes マスターを更新する前に実行する必要があるアクションを示します。
{: shortdesc}

<table summary="バージョン 1.13 の Kubernetes の更新">
<caption>マスターを Kubernetes 1.13 に更新する前に行う変更</caption>
<thead>
<tr>
<th>タイプ</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>N/A</td>
<td></td>
</tr>
</tbody>
</table>

### マスターの更新後に行う操作
{: #113_after}

以下の表に、Kubernetes マスターを更新した後に実行する必要があるアクションを示します。
{: shortdesc}

<table summary="バージョン 1.13 の Kubernetes の更新">
<caption>マスターを Kubernetes 1.13 に更新した後に行う変更</caption>
<thead>
<tr>
<th>タイプ</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>クラスターの新しいデフォルトの DNS プロバイダーとして CoreDNS が使用可能に</td>
<td>Kubernetes 1.13 以降の新しいクラスターでは、CoreDNS がクラスターのデフォルトの DNS プロバイダーになりました。 KubeDNS をクラスター DNS プロバイダーとして使用している既存のクラスターを 1.13 に更新した場合は、クラスター DNS プロバイダーは KubeDNS のままです。 しかし、[CoreDNS を代わりに使用する](/docs/containers?topic=containers-cluster_dns#dns_set)こともできます。
<br><br>Kubernetes サービスの `ExternalName` フィールドにドメイン・ネームを入力できるように、CoreDNS は [クラスターの DNS 仕様 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/dns/blob/master/docs/specification.md#25---records-for-external-name-services) をサポートしています。 以前のクラスター DNS プロバイダーである KubeDNS は、クラスターの DNS 仕様に準拠していないので、`ExternalName` には IP アドレスが使用されます。 Kubernetes のサービスで DNS ではなく IP アドレスを使用している場合は、引き続き機能させるために `ExternalName` を DNS に更新する必要があります。</td>
</tr>
<tr>
<td>`Deployment` と `StatefulSet` の `kubectl` 出力</td>
<td>`Deployment` と `StatefulSet` の `kubectl` 出力に`「Ready」`列が含まれるようになり、よりわかりやすくなりました。 以前の動作に依存したスクリプトがある場合は更新してください。</td>
</tr>
<tr>
<td>`PriorityClass` の `kubectl` 出力</td>
<td>`PriorityClass` の `kubectl` 出力に`「Value」`列が含まれるようになりました。 以前の動作に依存したスクリプトがある場合は更新してください。</td>
</tr>
<tr>
<td>`kubectl get componentstatuses`</td>
<td>`kubectl get componentstatuses` コマンドが、一部の Kubernetes マスター・コンポーネントの正常性を正しく報告しなくなりました。`localhost` のポートおよび非セキュアな (HTTP) ポートが無効になったために、これらのコンポーネントに Kubernetes API サーバーからアクセスできなくなったからです。 Kubernetes バージョン 1.10 で高可用性 (HA) マスターが導入された後、各 Kubernetes マスターには、複数の `apiserver`、`controller-manager`、`scheduler`、および `etcd` インスタンスがセットアップされます。 代わりに、[{{site.data.keyword.Bluemix_notm}} コンソール ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/kubernetes/landing) を確認するか、`ibmcloud ks cluster-get` [コマンド](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_get)を使用してクラスターの正常性を確認できます。</td>
</tr>
<tr>
<tr>
<td>非サポート: `kubectl run-container`</td>
<td>`kubectl run-container` コマンドが廃止されました。 代わりに、`kubectl run` コマンドを使用します。</td>
</tr>
<tr>
<td>`kubectl rollout undo`</td>
<td>存在しないリビジョンに対して `kubectl rollout undo` を実行すると、エラーが返されます。 以前の動作に依存したスクリプトがある場合は更新してください。</td>
</tr>
<tr>
<td>非推奨: `scheduler.alpha.kubernetes.io/critical-pod` アノテーション</td>
<td>`scheduler.alpha.kubernetes.io/critical-pod` アノテーションが非推奨になりました。 このアノテーションに依存しているポッドは、代わりに[ポッドの優先度](/docs/containers?topic=containers-pod_priority#pod_priority)を使用するように変更してください。</td>
</tr>
</tbody>
</table>

### ワーカー・ノードの更新後に行う操作
{: #113_after_workers}

以下の表に、ワーカー・ノードを更新した後に実行する必要がある操作を示します。
{: shortdesc}

<table summary="バージョン 1.13 の Kubernetes の更新">
<caption>ワーカー・ノードを Kubernetes 1.13 に更新した後に行う変更</caption>
<thead>
<tr>
<th>タイプ</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>containerd `cri` ストリーム・サーバー</td>
<td>containerd バージョン 1.2 では、`cri` プラグイン・ストリーム・サーバーがランダム・ポート `http://localhost:0` で動作するようになりました。 この変更により `kubelet` ストリーミング・プロキシーがサポートされ、コンテナーの `exec` 操作と `logs` の操作で、より安全なストリーミング・インターフェースを利用できるようになりました。 以前は、`cri` ストリーム・サーバーはポート 10010 を使用してワーカー・ノードのプライベート・ネットワーク・インターフェースを listen していました。 コンテナー `cri` プラグインを使用するアプリがあり、以前の動作に依存している場合は、そのアプリを更新してください。</td>
</tr>
</tbody>
</table>

<br />


## バージョン 1.12
{: #cs_v112}

<p><img src="images/certified_kubernetes_1x12.png" style="padding-right: 10px;" align="left" alt="このバッジは、IBM Cloud Container Service が Kubernetes バージョン 1.12 の認定を受けたことを示しています。"/> {{site.data.keyword.containerlong_notm}} は、CNCF Kubernetes Software Conformance Certification プログラムのもとで認定を受けたバージョン 1.12 の Kubernetes 製品です。 __</p>

Kubernetes を前のバージョンから 1.12 に更新する場合に必要な可能性がある変更作業について説明します。
{: shortdesc}

### マスターの前に行う更新
{: #112_before}

以下の表に、Kubernetes マスターを更新する前に実行する必要があるアクションを示します。
{: shortdesc}

<table summary="バージョン 1.12 の Kubernetes の更新">
<caption>マスターを Kubernetes 1.12 に更新する前に行う変更</caption>
<thead>
<tr>
<th>タイプ</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes Metrics Server</td>
<td>クラスター内に Kubernetes `metric-server` を現在デプロイしている場合は、クラスターを Kubernetes 1.12 に更新する前に `metric-server` を削除する必要があります。 削除しておくことで、更新中にデプロイされる `metric-server` との競合を避けることができます。</td>
</tr>
<tr>
<td>`kube-system` `default` サービス・アカウントの役割バインディング</td>
<td>`kube-system` `default` サービス・アカウントに、Kubernetes API への **cluster-admin** アクセス権限はなくなりました。 [Helm](/docs/containers?topic=containers-helm#public_helm_install) などの、クラスター内のプロセスにアクセスする必要があるフィーチャーまたはアドオンをデプロイする場合は、[サービス・アカウント![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/reference/access-authn-authz/service-accounts-admin/) をセットアップしてください。 該当する許可を持つサービス・アカウントを個別に作成してセットアップすると時間がかかるという場合は、クラスター役割バインディング `kubectl create clusterrolebinding kube-system:default --clusterrole=cluster-admin --serviceaccount=kube-system:default` を使用して **cluster-admin** 役割を一時的に付与することができます。</td>
</tr>
</tbody>
</table>

### マスターの更新後に行う操作
{: #112_after}

以下の表に、Kubernetes マスターを更新した後に実行する必要があるアクションを示します。
{: shortdesc}

<table summary="バージョン 1.12 の Kubernetes の更新">
<caption>マスターを Kubernetes 1.12 に更新した後に行う変更</caption>
<thead>
<tr>
<th>タイプ</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes の API</td>
<td>Kubernetes API では、非推奨の API が次のように置き換えられました。
<ul><li><strong>apps/v1</strong>: `apps/v1` Kubernetes API によって、`apps/v1beta1` API と `apps/v1alpha` API は置き換えられました。 また、`apps/v1` API によって、リソース `daemonset`、`deployment`、`replicaset`、および `statefulset` の `extensions/v1beta1` API も置き換えられました。 Kubernetes プロジェクトでは、以前の Kubernetes `apiserver` と `kubectl` クライアントの API のサポートを非推奨にして段階的に終了しています。</li>
<li><strong>networking.k8s.io/v1</strong>: `networking.k8s.io/v1` API によって、NetworkPolicy リソースの `extensions/v1beta1` API は置き換えられました。</li>
<li><strong>policy/v1beta1</strong>: `policy/v1beta1` API によって、`podsecuritypolicy` リソースの `extensions/v1beta1` API は置き換えられました。</li></ul>
<br><br>非推奨 API が非サポート API になる前に、適切な Kubernetes API を使用するようにすべての YAML `apiVersion` フィールドを更新してください。 また、`apps/v1` に関連する以下のような変更について、[Kubernetes 資料![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) を確認してください。
<ul><li>デプロイメントの作成後、`.spec.selector` フィールドは変更不可能になります。</li>
<li>`.spec.rollbackTo` フィールドは非推奨です。 代わりに、`kubectl rollout undo` コマンドを使用します。</li></ul></td>
</tr>
<tr>
<td>クラスターの DNS プロバイダーとして CoreDNS が使用可能に</td>
<td>Kubernetes プロジェクトは、現在の Kubernetes DNS (KubeDNS) の代わりに CoreDNS をサポートするように移行中です。 バージョン 1.12 では、デフォルトのクラスター DNS は KubeDNS のままですが、[CoreDNS を使用することを選択](/docs/containers?topic=containers-cluster_dns#dns_set)できます。</td>
</tr>
<tr>
<td>`kubectl apply --force`</td>
<td>現時点では、YAML ファイル内の変更不可能フィールドなど、更新できないリソースに対して適用アクション (`kubectl apply --force`) を強制すると、代わりにそのリソースが再作成されます。 以前の動作に依存したスクリプトがある場合は更新してください。</td>
</tr>
<tr>
<td>`kubectl get componentstatuses`</td>
<td>`kubectl get componentstatuses` コマンドが、一部の Kubernetes マスター・コンポーネントの正常性を正しく報告しなくなりました。`localhost` のポートおよび非セキュアな (HTTP) ポートが無効になったために、これらのコンポーネントに Kubernetes API サーバーからアクセスできなくなったからです。 Kubernetes バージョン 1.10 で高可用性 (HA) マスターが導入された後、各 Kubernetes マスターには、複数の `apiserver`、`controller-manager`、`scheduler`、および `etcd` インスタンスがセットアップされます。 代わりに、[{{site.data.keyword.Bluemix_notm}} コンソール ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/kubernetes/landing) を確認するか、`ibmcloud ks cluster-get` [コマンド](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_get)を使用してクラスターの正常性を確認できます。</td>
</tr>
<tr>
<td>`kubectl logs --interactive`</td>
<td>`kubectl logs` では、`--interactive` フラグがサポートされなくなりました。 このフラグを使用している自動化を更新してください。</td>
</tr>
<tr>
<td>`kubectl patch`</td>
<td>`patch` コマンドの結果が変更なしであった場合に (冗長パッチ)、このコマンドは戻りコード `1` で終了しなくなりました。 以前の動作に依存したスクリプトがある場合は更新してください。</td>
</tr>
<tr>
<td>`kubectl version -c`</td>
<td>`-c` 省略表現フラグはサポートされなくなりました。 代わりに、完全な `--client` フラグを使用してください。 このフラグを使用している自動化を更新してください。</td>
</tr>
<tr>
<td>`kubectl wait`</td>
<td>一致するセレクターが見つからない場合、このコマンドはエラー・メッセージを出力し、戻りコード `1` で終了するようになりました。 以前の動作に依存したスクリプトがある場合は更新してください。</td>
</tr>
<tr>
<td>kubelet cAdvisor port</td>
<td>kubelet で `-cadvisor-port` を開始して使用されていた [Container Advisor (cAdvisor) ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/google/cadvisor) Web UI が、Kubernetes 1.12 から削除されました。 引き続き cAdvisor を実行する必要がある場合は、[cAdvisor をデーモン・セットとしてデプロイ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/google/cadvisor/tree/master/deploy/kubernetes) してください。<br><br>`http://node-ip:4194` から cAdvisor に到達できるように、デーモン・セット内に以下のように ports セクションを指定します。 旧バージョンの kubelet では cAdvisor 用にホスト・ポート 4194 が使用されているので、ワーカー・ノードが 1.12 に更新されるまでは cAdvisor ポッドが失敗します。
<pre class="screen"><code>ports:
          - name: http
            containerPort: 8080
            hostPort: 4194
            protocol: TCP</code></pre></td>
</tr>
<tr>
<td>Kubernetes ダッシュボード</td>
<td>`kubectl proxy` でダッシュボードにアクセスすると、ログイン・ページ上の**「スキップ (SKIP)」**ボタンが削除されます。 代わりに、[**「トークン (Token)」**を使用してログインしてください](/docs/containers?topic=containers-app#cli_dashboard)。</td>
</tr>
<tr>
<td id="metrics-server">Kubernetes Metrics Server</td>
<td>クラスター・メトリック・プロバイダーとして、Kubernetes Heapster (Kubernetes バージョン 1.8 以降では非推奨) が Kubernetes Metrics Server に置き換えられました。 クラスターで実行するポッド数がワーカー・ノード 1 つあたり 30 を超える場合は、[`metrics-server` 構成でパフォーマンスを調整](/docs/containers?topic=containers-kernel#metrics)してください。
<p>Kubernetes ダッシュボードでは、`metrics-server` を操作できません。 ダッシュボードにメトリックを表示する場合は、以下の選択肢があります。</p>
<ul><li>Cluster Monitoring Dashboard を使用して、[メトリックを分析するように Grafana をセットアップします](/docs/services/cloud-monitoring/tutorials?topic=cloud-monitoring-container_service_metrics#container_service_metrics)。</li>
<li>[Heapster ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/heapster) をクラスターにデプロイします。
<ol><li>`heapster-rbac` [YAML ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/kubernetes/blob/release-1.12/cluster/addons/cluster-monitoring/heapster-rbac.yaml)、`heapster-service` [YAML ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/kubernetes/blob/release-1.12/cluster/addons/cluster-monitoring/standalone/heapster-service.yaml)、`heapster-controller` [YAML ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/kubernetes/blob/release-1.12/cluster/addons/cluster-monitoring/standalone/heapster-controller.yaml) ファイルをコピーします。</li>
<li>以下のストリングを置き換えて、`heapster-controller` YAML を編集します。
<ul><li>`{{ nanny_memory }}` を `90Mi` に置き換えます</li>
<li>`{{ base_metrics_cpu }}` を `80m` に置き換えます</li>
<li>`{{ metrics_cpu_per_node }}` を `0.5m` に置き換えます</li>
<li>`{{ base_metrics_memory }}` を `140Mi` に置き換えます</li>
<li>`{{ metrics_memory_per_node }}` を `4Mi` に置き換えます</li>
<li>`{{ heapster_min_cluster_size }}` を `16` に置き換えます</li></ul></li>
<li>`kubectl apply -f` コマンドを実行して、`heapster-rbac`、`heapster-service`、`heapster-controller` YAML ファイルをクラスターに適用します。</li></ol></li></ul></td>
</tr>
<tr>
<td>`rbac.authorization.k8s.io/v1` Kubernetes API</td>
<td>`rbac.authorization.k8s.io/v1` Kubernetes API (Kubernetes 1.8 以降でサポートされます) は、`rbac.authorization.k8s.io/v1alpha1` API や `rbac.authorization.k8s.io/v1beta1` API を置き換えるものです。 サポートされていない `v1alpha` API を使用して役割や役割バインディングなどの RBAC オブジェクトを作成することはできなくなりました。 既存の RBAC オブジェクトは `v1` API に変換されます。</td>
</tr>
</tbody>
</table>

<br />


## 非推奨: バージョン 1.11
{: #cs_v111}

<p><img src="images/certified_kubernetes_1x11.png" style="padding-right: 10px;" align="left" alt="このバッジは、IBM Cloud コンテナー・サービスが Kubernetes バージョン 1.11 の認定を受けたことを示しています。"/> {{site.data.keyword.containerlong_notm}} は、CNCF Kubernetes Software Conformance Certification プログラムのもとで認定を受けたバージョン 1.11 の Kubernetes 製品です。 __</p>

Kubernetes を前のバージョンから 1.11 に更新する場合に必要な可能性がある変更作業について説明します。
{: shortdesc}

Kubernetes バージョン 1.11 は非推奨バージョンであり、2019 年 6 月 27 日 (暫定) に非サポート・バージョンになります。各 Kubernetes バージョンの更新が[与える可能性のある影響を確認](/docs/containers?topic=containers-cs_versions#cs_versions)したうえで、少なくとも 1.12 にただちに[クラスターを更新](/docs/containers?topic=containers-update#update)してください。
{: deprecated}

Kubernetes バージョン 1.9 以前からバージョン 1.11 にクラスターを正常に更新するには、その前に、[Calico v3 への更新の準備](#111_calicov3)に記載の手順を実行しておく必要があります。
{: important}

### マスターの前に行う更新
{: #111_before}

以下の表に、Kubernetes マスターを更新する前に実行する必要があるアクションを示します。
{: shortdesc}

<table summary="バージョン 1.11 の Kubernetes の更新">
<caption>マスターを Kubernetes 1.11 に更新する前に行う変更</caption>
<thead>
<tr>
<th>タイプ</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>クラスター・マスターの高可用性 (HA) 構成</td>
<td>クラスター・マスターの構成が更新され、高可用性 (HA) が向上しました。 現在のクラスターは、別々の物理ホスト上にデプロイされた Kubernetes マスター・レプリカ 3 台で構成されるようになりました。 さらに、クラスターが複数ゾーン対応ゾーンにある場合は、それらのマスターがゾーン間に分散されます。<br><br>実行する必要があるアクションについては、[高可用性クラスター・マスターへの更新](#ha-masters)を参照してください。 これらの準備アクションは、以下の場合に適用されます。<ul>
<li>ファイアウォールまたはカスタム Calico ネットワーク・ポリシーがある場合。</li>
<li>ワーカー・ノードでホスト・ポート `2040` または `2041` を使用している場合。</li>
<li>マスターへのクラスター内アクセス用にクラスターのマスター IP アドレスを使用していた場合。</li>
<li>Calico ポリシーの作成などのために、Calico API または CLI (`calicoctl`) を呼び出す自動化機能がある場合。</li>
<li>Kubernetes または Calico ネットワーク・ポリシーを使用して、ポッドからマスターへの発信アクセスを制御している場合。</li></ul></td>
</tr>
<tr>
<td>`containerd` の新しい Kubernetes コンテナー・ランタイム</td>
<td><p class="important">`containerd` は、Kubernetes の新しいコンテナー・ランタイムとして Docker を置き換えるものです。 実行する必要があるアクションについては、[コンテナー・ランタイムとしての `containerd` への更新](#containerd)を参照してください。</p></td>
</tr>
<tr>
<td>etcd のデータの暗号化</td>
<td>以前は、etcd データは、保存時に暗号化が行われたマスターの NFS ファイル・ストレージ・インスタンスに保管されていました。 現在は、etcd データはマスターのローカル・ディスクに保管され、{{site.data.keyword.cos_full_notm}} にバックアップされます。 {{site.data.keyword.cos_full_notm}} に転送中のデータも保存されたデータも暗号化されています。 しかし、マスターのローカル・ディスク上の etcd データは暗号化されません。 マスターのローカル etcd データを暗号化する場合は、[クラスター内で {{site.data.keyword.keymanagementservicelong_notm}} を有効にします](/docs/containers?topic=containers-encryption#keyprotect)。</td>
</tr>
<tr>
<td>Kubernetes コンテナー・ボリューム・マウント伝搬</td>
<td>コンテナー `VolumeMount` の [`mountPropagation` フィールド ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/concepts/storage/volumes/#mount-propagation) のデフォルト値は、`HostToContainer` から `None` に変更されました。 この変更により、Kubernetes バージョン 1.9 以前に存在していた動作が復元されます。 ポッドの仕様がデフォルトの `HostToContainer` に依存している場合は、それらを更新します。</td>
</tr>
<tr>
<td>Kubernetes API サーバー JSON デシリアライザー</td>
<td>Kubernetes API サーバー JSON デシリアライザーで、大/小文字が区別されるようになりました。 この変更により、Kubernetes バージョン 1.7 以前に存在していた動作が復元されます。 JSON リソース定義で正しくない大/小文字が使用されている場合は、それらを更新します。 <br><br>影響を受けるのは、直接の Kubernetes API サーバー要求のみです。 `kubectl` CLI では、Kubernetes バージョン 1.7 以降で大/小文字を区別するキーが引き続き強制されるため、`kubectl` でリソースを厳密に管理する場合は、影響を受けません。</td>
</tr>
</tbody>
</table>

### マスターの更新後に行う操作
{: #111_after}

以下の表に、Kubernetes マスターを更新した後に実行する必要があるアクションを示します。
{: shortdesc}

<table summary="バージョン 1.11 の Kubernetes の更新">
<caption>マスターを Kubernetes 1.11 に更新した後に行う変更</caption>
<thead>
<tr>
<th>タイプ</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>クラスター・ロギング構成</td>
<td>`fluentd` クラスター・アドオンは、`logging-autoupdate` が無効になっている場合でも、バージョン 1.11 で自動的に更新されます。<br><br>
コンテナー・ログ・ディレクトリーが `/var/lib/docker/` から `/var/log/pods/` に変更されました。 以前のディレクトリーをモニターする独自のロギング・ソリューションを使用する場合は、それに応じて更新します。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM) のサポート</td>
<td>Kubernetes バージョン 1.11 以降を実行するクラスターでは、IAM [アクセス・グループ](/docs/iam?topic=iam-groups#groups)および[サービス ID](/docs/iam?topic=iam-serviceids#serviceids) がサポートされます。 これらの機能を使用して、[クラスターに対するアクセスを許可](/docs/containers?topic=containers-users#users)できるようになりました。</td>
</tr>
<tr>
<td>Kubernetes 構成のリフレッシュ</td>
<td>クラスターの Kubernetes API サーバーの OpenID Connect 構成が、{{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM) アクセス・グループをサポートするように更新されました。 そのため、`ibmcloud ks cluster-config --cluster <cluster_name_or_ID>` を実行して、マスター Kubernetes v1.11 の更新の後にクラスターの Kubernetes 構成をリフレッシュする必要があります。 このコマンドを使用することで、構成が `default` 名前空間の役割バインディングに適用されます。<br><br>構成をリフレッシュしないと、クラスター・アクションは失敗し、`You must be logged in to the server (Unauthorized).` というエラー・メッセージが表示されます。</td>
</tr>
<tr>
<td>Kubernetes ダッシュボード</td>
<td>`kubectl proxy` でダッシュボードにアクセスすると、ログイン・ページ上の**「スキップ (SKIP)」**ボタンが削除されます。 代わりに、[**「トークン (Token)」**を使用してログインしてください](/docs/containers?topic=containers-app#cli_dashboard)。</td>
</tr>
<tr>
<td>`kubectl` CLI</td>
<td>Kubernetes バージョン 1.11 の `kubectl` CLI には、`apps/v1` API が必要です。 そのため、v1.11 の `kubectl` CLI は、Kubernetes バージョン 1.8 以前を実行するクラスターでは機能しません。 クラスターの Kubernetes API サーバーのバージョンに一致している `kubectl` CLI のバージョンを使用してください。</td>
</tr>
<tr>
<td>`kubectl auth can-i`</td>
<td>ユーザーが許可されていない場合、`kubectl auth can-i` コマンドが `exit code 1` で失敗するようになりました。以前の動作に依存したスクリプトがある場合は更新してください。</td>
</tr>
<tr>
<td>`kubectl delete`</td>
<td>ラベルなどの選択基準を使用してリソースを削除すると、デフォルトでは `kubectl delete` コマンドで `not found` エラーが無視されるようになりました。 以前の動作に依存したスクリプトがある場合は更新してください。</td>
</tr>
<tr>
<td>Kubernetes `sysctls` 機能</td>
<td>`security.alpha.kubernetes.io/sysctls` アノテーションが無視されるようになりました。 代わりに、Kubernetes では、`sysctls` を指定および制御できるように `PodSecurityPolicy` オブジェクトと `Pod` オブジェクトにフィールドが追加されました。 詳しくは、[Using sysctls in Kubernetes ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/tasks/administer-cluster/sysctl-cluster/) を参照してください。 <br><br>クラスターのマスターおよびワーカーを更新したら、新しい `sysctls` フィールドを使用するように `PodSecurityPolicy` オブジェクトと `Pod` オブジェクトを更新します。</td>
</tr>
</tbody>
</table>

### Kubernetes 1.11 の高可用性クラスター・マスターへの更新
{: #ha-masters}

Kubernetes バージョン 1.10.8_1530、1.11.3_1531、またはそれ以降を実行するクラスターは、高可用性 (HA) を向上させるためにクラスター・マスターの構成が更新されています。現在のクラスターは、別々の物理ホスト上にデプロイされた Kubernetes マスター・レプリカ 3 台で構成されるようになりました。 さらに、クラスターが複数ゾーン対応ゾーンにある場合は、それらのマスターがゾーン間に分散されます。
{: shortdesc}

クラスターが HA マスター構成になっているどうかを調べるには、コンソールでクラスターのマスター URL を確認するか、`ibmcloud ks cluster-get --cluster <cluster_name_or_ID` を実行します。マスター URL が、`https://169.xx.xx.xx:xxxxx` のような IP アドレスを含むものではなく、`https://c2.us-south.containers.cloud.ibm.com:xxxxx` のようなホスト名を含むものであれば、HA マスター構成のクラスターです。 マスター・パッチの自動更新または手動の更新適用によって、HA マスター構成になることもあります。 どちらの場合も、次の項目を確認し、構成を最大限に活用するようにクラスター・ネットワークをセットアップする必要があります。

* ファイアウォールまたはカスタム Calico ネットワーク・ポリシーがある場合。
* ワーカー・ノードでホスト・ポート `2040` または `2041` を使用している場合。
* マスターへのクラスター内アクセス用にクラスターのマスター IP アドレスを使用していた場合。
* Calico ポリシーの作成などのために、Calico API または CLI (`calicoctl`) を呼び出す自動化機能がある場合。
* Kubernetes または Calico ネットワーク・ポリシーを使用して、ポッドからマスターへの発信アクセスを制御している場合。

<br>
**ファイアウォールまたはカスタム Calico ホスト・ネットワーク・ポリシーを HA マスターのために更新する**</br>
{: #ha-firewall}
ファイアウォールまたはカスタム Calico ホスト・ネットワーク・ポリシーを使用してワーカー・ノードからの発信を制御している場合は、クラスターがある地域内のすべてのゾーンで該当するポートと IP アドレスへの発信トラフィックを許可してください。 [クラスターからインフラストラクチャー・リソースや他のサービスへのアクセスの許可](/docs/containers?topic=containers-firewall#firewall_outbound)を参照してください。

<br>
**ワーカー・ノードのホスト・ポート `2040` と `2041` を予約する**</br>
{: #ha-ports}
HA 構成のクラスター・マスターにアクセスできるようにするには、すべてのワーカー・ノードのホスト・ポート `2040` と `2041` を使用可能な状態にしておく必要があります。
* `hostPort` が `2040` または `2041` に設定されているポッドを、別のポートを使用するように更新します。
* `hostNetwork` が `true` に設定され、`2040` または `2041` ポート上で listen しているポッドを、別のポートを使用するように更新します。

ポッドが現在ポート `2040` または `2041` を使用しているかどうかを確認するには、クラスターをターゲットに設定して、以下のコマンドを実行します。

```
kubectl get pods --all-namespaces -o yaml | grep -B 3 "hostPort: 204[0,1]"
```
{: pre}

HA マスター構成が既にある場合は、以下の例のように、`kube-system` 名前空間の `ibm-master-proxy-*` の結果が表示されます。 他のポッドが返された場合は、そのポートを更新します。

```
name: ibm-master-proxy-static
ports:
- containerPort: 2040
  hostPort: 2040
  name: apiserver
  protocol: TCP
- containerPort: 2041
  hostPort: 2041
...
```
{: screen}


<br>
**マスターへのクラスター内アクセス用に `kubernetes` サービスのクラスター IP またはドメインを使用する**</br>
{: #ha-incluster}
クラスター内から HA 構成のクラスター・マスターにアクセスするには、以下のいずれかを使用します。
* `kubernetes` サービスのクラスター IP アドレス (デフォルトでは `https://172.21.0.1`)
* `kubernetes` サービスのドメイン・ネーム (デフォルトでは `https://kubernetes.default.svc.cluster.local`)

これまでクラスター・マスター IP アドレスを使用していた場合は、その方式を引き続き使用できます。 しかし、可用性を向上させるには、`kubernetes` サービスのクラスター IP アドレスまたはドメイン・ネームを使用するように更新してください。

<br>
**HA 構成のマスターへのクラスター外アクセス用に Calico を構成する**</br>
{: #ha-outofcluster}
`kube-system` 名前空間の `calico-config` 構成マップに保管されるデータが、HA マスター構成をサポートするために変更されています。 具体的には、`etcd_endpoints` 値が、クラスター内アクセスのみをサポートするようになりました。 クラスター外からのアクセスを Calico CLI に構成するためにこの値を使用しても、機能しなくなりました。

代わりに、`kube-system` 名前空間の `cluster-info` 構成マップに保管されるデータを使用してください。 具体的には、`etcd_host` 値と `etcd_port` 値を使用して、クラスターの外部から HA 構成のマスターにアクセスするためのエンドポイントを [Calico CLI](/docs/containers?topic=containers-network_policies#cli_install) に構成してください。

<br>
**Kubernetes または Calico ネットワーク・ポリシーを更新する**</br>
{: #ha-networkpolicies}
[Kubernetes または Calico ネットワーク・ポリシー](/docs/containers?topic=containers-network_policies#network_policies)を使用してポッドからクラスター・マスターへの発信アクセスを制御しており、現在以下のものを使用している場合は、追加のアクションを実行する必要があります。
*  Kubernetes サービスのクラスター IP (`kubectl get service kubernetes -o yaml | grep clusterIP` を実行すると表示できます)。
*  Kubernetes サービスのドメイン・ネーム (デフォルトでは `https://kubernetes.default.svc.cluster.local`)。
*  クラスター・マスター IP (`kubectl cluster-info | grep Kubernetes` を実行すると表示できます)。

以下の手順では、Kubernetes ネットワーク・ポリシーを更新する方法について説明します。 Calico ネットワーク・ポリシーを更新するには、ポリシー構文を少し変更してこれらの手順を繰り返し、`calicoctl` を使用してポリシーに影響がないか検索します。
{: note}

開始前に、以下のことを行います。 [アカウントにログインします。 該当する場合は、適切なリソース・グループをターゲットにします。 クラスターのコンテキストを設定します。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  クラスター・マスター IP アドレスを取得します。
    ```
    kubectl cluster-info | grep Kubernetes
    ```
    {: pre}

2.  Kubernetes ネットワーク・ポリシーに影響がないか検索します。 YAML が返されなかった場合は、クラスターに影響はないので、追加の変更を行う必要はありません。
    ```
    kubectl get networkpolicies --all-namespaces -o yaml | grep <cluster-master-ip>
    ```
    {: pre}

3.  YAML を確認します。 例えば、`default` 名前空間のポッドが `kubernetes` サービスのクラスター IP またはクラスター・マスター IP を介してクラスター・マスターにアクセスできるように、クラスターで以下の Kubernetes ネットワーク・ポリシーを使用している場合は、このポリシーを更新する必要があります。
    ```
    apiVersion: networking.k8s.io/v1
    kind: NetworkPolicy
    metadata:
      name: all-master-egress
      namespace: default
    spec:
      egress:
      # Allow access to cluster master using kubernetes service cluster IP address
      # or domain name or cluster master IP address.
      - ports:
        - protocol: TCP
        to:
        - ipBlock:
            cidr: 161.202.126.210/32
      # Allow access to Kubernetes DNS in order to resolve the kubernetes service
      # domain name.
      - ports:
        - protocol: TCP
          port: 53
        - protocol: UDP
          port: 53
      podSelector: {}
      policyTypes:
      - Egress
    ```
    {: screen}

4.  クラスター内マスター・プロキシー IP アドレス `172.20.0.1` への発信を許可するように、Kubernetes ネットワーク・ポリシーを修正します。 ここでは、クラスター・マスター IP アドレスを保持します。 例えば、前述のネットワーク・ポリシーの例を以下のように変更します。

    これまで単一の Kubernetes マスター用に単一の IP アドレスとポートのみを開くように発信ポリシーをセットアップしていた場合は、172.20.0.1/32 のクラスター内マスター・プロキシー IP アドレスの範囲とポート 2040 を使用するようにします。
    {: tip}

    ```
    apiVersion: networking.k8s.io/v1
    kind: NetworkPolicy
    metadata:
      name: all-master-egress
      namespace: default
    spec:
      egress:
      # Allow access to cluster master using kubernetes service cluster IP address
      # or domain name.
      - ports:
        - protocol: TCP
        to:
        - ipBlock:
            cidr: 172.20.0.1/32
        - ipBlock:
            cidr: 161.202.126.210/32
      # Allow access to Kubernetes DNS in order to resolve the kubernetes service domain name.
      - ports:
        - protocol: TCP
          port: 53
        - protocol: UDP
          port: 53
      podSelector: {}
      policyTypes:
      - Egress
    ```
    {: screen}

5.  修正したネットワーク・ポリシーをクラスターに適用します。
    ```
    kubectl apply -f all-master-egress.yaml
    ```
    {: pre}

6.  すべての[準備アクション](#ha-masters) (この手順を含む) が完了したら、HA マスター・フィックスパックに[クラスター・マスターを更新](/docs/containers?topic=containers-update#master)します。

7.  更新が完了したら、ネットワーク・ポリシーからクラスター・マスター IP アドレスを削除します。 例えば、前述のネットワーク・ポリシーでは、以下の行を削除してからポリシーを再適用します。

    ```
    - ipBlock:
        cidr: 161.202.126.210/32
    ```
    {: screen}

    ```
    kubectl apply -f all-master-egress.yaml
    ```
    {: pre}

### コンテナー・ランタイムとしての `containerd` への更新
{: #containerd}

Kubernetes バージョン 1.11 以降を実行するクラスターの場合、`containerd` は、パフォーマンスを向上させるために、Kubernetes の新しいコンテナー・ランタイムとして Docker を置き換えるものです。 ご使用のポッドが Kubernetes コンテナー・ランタイムとしての Docker に依存している場合は、これらを更新して、`containerd` をコンテナー・ランタイムとして処理する必要があります。 詳しくは、[Kubernetes の containerd についての発表 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/blog/2018/05/24/kubernetes-containerd-integration-goes-ga/) を参照してください。
{: shortdesc}

**アプリが `containerd` ではなく `docker` に依存しているかどうかを確認するには、どうすればよいですか?**<br>
コンテナー・ランタイムとしての Docker に依存している可能性のある場合の例:
*  特権コンテナーを使用して Docker エンジンまたは API に直接アクセスする場合は、ランタイムとしての `containerd` をサポートするようにポッドを更新します。 例えば、Docker ソケットを直接呼び出して、コンテナーを起動したり、その他の Docker 操作を実行したりできます。 Docker ソケットは `/var/run/docker.sock` から `/run/containerd/containerd.sock` に変更されました。 `containerd` ソケットで使用されるプロトコルは、Docker のものとは若干異なります。 アプリを `containerd` ソケットに更新することを試みてください。 引き続き Docker ソケットを使用する場合は、[Docker-inside-Docker (DinD) ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://hub.docker.com/_/docker/) の使用を検討してください。
*  クラスター内にインストールするロギング・ツールやモニター・ツールなどの一部のサード・パーティー・アドオンは、Docker エンジンに依存している場合があります。 ツールが containerd と互換性があることをプロバイダーに確認してください。以下のユース・ケースが考えられます。
   - ロギング・ツールがコンテナー `stderr/stdout` ディレクトリー `/var/log/pods/<pod_uuid>/<container_name>/*.log` を使用してログにアクセスする場合があります。 Docker ではこのディレクトリーは `/var/data/cripersistentstorage/containers/<container_uuid>/<container_uuid>-json.log` へのシンボリック・リンクですが、`containerd` ではシンボリック・リンクではなく直接このディレクトリーにアクセスします。
   - モニター・ツールは Docker ソケットに直接アクセスします。 Docker ソケットは `/var/run/docker.sock` から `/run/containerd/containerd.sock` に変更されました。

<br>

**ランタイムに依存している場合以外に、準備アクションを実行する必要がありますか?**<br>

**マニフェスト・ツール**: Docker バージョン 18.06 より前の試験的な `docker manifest` [ツール ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.docker.com/edge/engine/reference/commandline/manifest/) を使用してビルドされたマルチプラットフォーム・イメージがある場合は、`containerd` を使用して DockerHub からイメージをプルすることはできません。

ポッドのイベントを確認すると、次のようなエラーが表示されることがあります。
```
failed size validation
```
{: screen}

マニフェスト・ツールを使用してビルドされたイメージとともに `containerd` を使用するには、以下のオプションのいずれかを選択します。

*  [マニフェスト・ツール ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/estesp/manifest-tool) を使用してイメージを再ビルドします。
*  Docker バージョン 18.06 以降に更新した後で `docker-manifest` ツールを使用してイメージを再ビルドします。

<br>

**影響を受けないものは何ですか? コンテナーのデプロイ方法を変更する必要がありますか?**<br>
一般に、コンテナー・デプロイメント・プロセスは変更されません。 引き続き Dockerfile を使用して Docker イメージを定義し、アプリの Docker コンテナーをビルドできます。 `docker` コマンドを使用して、イメージをビルドしてレジストリーにプッシュする場合は、引き続き `docker` を使用することも、代わりに `ibmcloud cr` コマンドを使用することもできます。

### Calico v3 への更新の準備
{: #111_calicov3}

Kubernetes バージョン 1.9 以前からバージョン 1.11 にクラスターを更新する場合、マスターを更新する前に、Calico v3 の更新の準備をします。 Kubernetes v1.11 へのマスター・アップグレード中は、新規ポッドおよび新規 Kubernetes または Calico ネットワーク・ポリシーはスケジュールされません。 更新によって新しいスケジュールが妨げられる時間はさまざまです。 小規模なクラスターでは数分かかり、ノードが 10 個増えるごとにさらに数分追加されます。 既存のネットワーク・ポリシーとポッドは引き続き実行されます。
{: shortdesc}

Kubernetes バージョン 1.10 からバージョン 1.11 にクラスターを更新する場合は、1.10 への更新時にこれらの手順は完了しているので、これらの手順をスキップします。
{: note}

始める前に、クラスター・マスターとすべてのワーカー・ノードで Kubernetes バージョン 1.8 または 1.9 が実行されている必要があり、少なくとも 1 つのワーカー・ノードが必要です。

1.  Calico ポッドが正常であることを確認します。
    ```
    kubectl get pods -n kube-system -l k8s-app=calico-node -o wide
    ```
    {: pre}

2.  **Running** 状態になっていないポッドがある場合は、そのポッドを削除し、**Running** 状態になるまで待ってから続行します。 ポッドが **Running** 状態に戻らない場合は、以下のようにします。
    1.  ワーカー・ノードの**状態**と**状況**を確認します。
        ```
        ibmcloud ks workers --cluster <cluster_name_or_ID>
        ```
        {: pre}
    2.  ワーカー・ノードの状態が **Normal** でない場合は、[ワーカー・ノードのデバッグ](/docs/containers?topic=containers-cs_troubleshoot#debug_worker_nodes)の手順に従います。 例えば、**Critical** または **Unknown** 状態は、多くの場合、[ワーカー・ノードを再ロード](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)すると解決されます。

3.  Calico ポリシーまたはその他の Calico リソースを自動生成する場合は、これらのリソースを生成するための自動化ツールを [Calico v3 構文 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.projectcalico.org/v3.1/reference/calicoctl/resources/) で更新します。

4.  VPN 接続に [strongSwan](/docs/containers?topic=containers-vpn#vpn-setup) を使用している場合、strongSwan 2.0.0 Helm チャートは Calico v3 または Kubernetes 1.11 では機能しません。 Calico 2.6、および Kubernetes 1.7、1.8、および 1.9 との後方互換性のある 2.1.0 Helm チャートに [strongSwan を更新](/docs/containers?topic=containers-vpn#vpn_upgrade)してください。

5.  [クラスター・マスターを Kubernetes 1.11 に更新します](/docs/containers?topic=containers-update#master)。

<br />


## アーカイブ
{: #k8s_version_archive}

{{site.data.keyword.containerlong_notm}} でサポートされていない Kubernetes バージョンの概要を示します。
{: shortdesc}

### バージョン 1.10 (サポート対象外)
{: #cs_v110}

2019 年 5 月 16 日現在、[Kubernetes バージョン 1.10](/docs/containers?topic=containers-changelog#changelog_archive) を実行する {{site.data.keyword.containerlong_notm}} クラスターはサポートされていません。バージョン 1.10 クラスターは、次に最新のバージョンに更新しない限り、セキュリティー更新もサポートも受けられません。
{: shortdesc}

Kubernetes 1.11 は非推奨であるため、各 Kubernetes バージョンの更新が[与える可能性のある影響を確認](/docs/containers?topic=containers-cs_versions#cs_versions)したうえで、[Kubernetes 1.12](#cs_v112) に[クラスターを更新](/docs/containers?topic=containers-update#update)してください。

### バージョン 1.9 (サポート対象外)
{: #cs_v19}

2018 年 12 月 27 日現在、[Kubernetes バージョン 1.9](/docs/containers?topic=containers-changelog#changelog_archive) を実行する {{site.data.keyword.containerlong_notm}} クラスターはサポートされていません。 バージョン 1.9 クラスターは、次に最新のバージョンに更新しない限り、セキュリティー更新もサポートも受けられません。
{: shortdesc}

各 Kubernetes バージョンの更新が[与える可能性のある影響を確認](/docs/containers?topic=containers-cs_versions#cs_versions)したうえで、まず[非推奨の Kubernetes 1.11](#cs_v111) に更新してから、ただちに [Kubernetes 1.12](#cs_v112) に[クラスターを更新](/docs/containers?topic=containers-update#update)してください。

### バージョン 1.8 (サポート対象外)
{: #cs_v18}

2018 年 9 月 22 日現在、[Kubernetes バージョン 1.8](/docs/containers?topic=containers-changelog#changelog_archive) を実行する {{site.data.keyword.containerlong_notm}} クラスターはサポートされていません。 バージョン 1.8 クラスターは、セキュリティー更新もサポートも受けられません。
{: shortdesc}

{{site.data.keyword.containerlong_notm}} でアプリの実行を継続するには、[新しいクラスターを作成して](/docs/containers?topic=containers-clusters#clusters)、その新しいクラスターに[アプリをデプロイ](/docs/containers?topic=containers-app#app)してください。

### バージョン 1.7 (サポート対象外)
{: #cs_v17}

2018 年 6 月 21 日現在、[Kubernetes バージョン 1.7](/docs/containers?topic=containers-changelog#changelog_archive) を実行する {{site.data.keyword.containerlong_notm}} クラスターはサポートされていません。 バージョン 1.7 クラスターは、セキュリティー更新もサポートも受けられません。
{: shortdesc}

{{site.data.keyword.containerlong_notm}} でアプリの実行を継続するには、[新しいクラスターを作成して](/docs/containers?topic=containers-clusters#clusters)、その新しいクラスターに[アプリをデプロイ](/docs/containers?topic=containers-app#app)してください。

### バージョン 1.5 (サポート対象外)
{: #cs_v1-5}

2018 年 4 月 4 日現在、[Kubernetes バージョン 1.5](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG-1.5.md) を実行する {{site.data.keyword.containerlong_notm}} クラスターはサポートされていません。 バージョン 1.5 クラスターは、セキュリティー更新もサポートも受けられません。
{: shortdesc}

{{site.data.keyword.containerlong_notm}} でアプリの実行を継続するには、[新しいクラスターを作成して](/docs/containers?topic=containers-clusters#clusters)、その新しいクラスターに[アプリをデプロイ](/docs/containers?topic=containers-app#app)してください。
