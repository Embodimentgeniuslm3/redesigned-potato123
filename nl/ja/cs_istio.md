---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-11"

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



# 管理対象 Istio アドオン (ベータ版) の使用
{: #istio}

{{site.data.keyword.containerlong}} 上の Istio は、Istio のシームレス・インストール、Istio コントロール・プレーン・コンポーネントの自動更新とライフサイクル管理、およびプラットフォームのロギングとモニタリングのツールとの統合を行います。
{: shortdesc}

ワンクリックですべての Istio コア・コンポーネントを取得し、追加のトレース、モニタリング、視覚化を行い、BookInfo サンプル・アプリを稼働状態にすることができます。 {{site.data.keyword.containerlong_notm}} 上の Istio は管理対象アドオンとして提供されるため、すべての Istio コンポーネントが {{site.data.keyword.Bluemix_notm}} によって自動的に最新に保たれます。

## {{site.data.keyword.containerlong_notm}} 上の Istio について
{: #istio_ov}

### Istio とは?
{: #istio_ov_what_is}

[Istio ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/cloud/info/istio) は、{{site.data.keyword.containerlong_notm}} における Kubernetes など、クラウド・プラットフォーム上のマイクロサービスを接続、保護、制御、監視するためのオープンなサービス・メッシュ・プラットフォームです。
{:shortdesc}

一体構造アプリケーションを分散マイクロサービス・アーキテクチャーにシフトする際には、一連の難題が新たに生じます。例えば、マイクロサービスのトラフィックの制御、サービスのダーク起動とカナリア配備の実施、障害の処理、サービス通信の保護、サービスの監視、サービスのフリート全体への一貫性のあるアクセス・ポリシーの実施をそれぞれどのように行うかなどです。 これらの問題に対処するために、サービス・メッシュを活用できます。 サービス・メッシュは、マイクロサービス間の接続と、その接続の監視、保護、制御のための、透過的で言語に依存しないネットワークを提供します。 Istio を使用すると、ネットワーク・トラフィックの管理、マイクロサービス間のロード・バランシング、アクセス・ポリシーの実施、サービス・アイデンティティーの検証などを行うことができるため、サービス・メッシュを洞察し、制御できます。

例えば、マイクロサービス・メッシュで Istio を使用すると、以下のことを行えるようになります。
- クラスター内で実行中のアプリに対する可視性を向上させる。
- カナリア版のアプリをデプロイし、それらのアプリに送信されるトラフィックを制御する。
- マイクロサービス間で転送されるデータの自動暗号化を使用可能にする。
- 速度制限と属性ベースのホワイトリスト・ポリシーとブラックリスト・ポリシーを実施する。

Istio サービス・メッシュは、データ・プレーンとコントロール・プレーンで構成されます。 データ・プレーンは各アプリ・ポッド内の Envoy プロキシー・サイドカーから成り、これらはマイクロサービス間の通信を仲介します。 コントロール・プレーンは Pilot、Mixer のテレメトリーとポリシー、Citadel から成り、これらは Istio 構成をクラスターに適用します。 これらのコンポーネントのそれぞれについて詳しくは、[`istio` アドオンについての説明](#istio_components)を参照してください。

### {{site.data.keyword.containerlong_notm}} 上の Istio (ベータ版) とは?
{: #istio_ov_addon}

{{site.data.keyword.containerlong_notm}} 上の Istio は、Istio を Kubernetes クラスターと直接統合する管理対象アドオンとして提供されます。
{: shortdesc}

管理対象 Istio アドオンはベータ版として分類されているため、不安定になったり頻繁に変更されたりする可能性があります。 また、ベータ機能は、一般出荷可能機能と同レベルのパフォーマンスや互換性を提供しない可能性もあり、実稼働環境での使用を意図したものではありません。
{: note}

**それはクラスター内でどのように見えますか?**</br>
Istio アドオンをインストールすると、Istio のコントロール・プレーンとデータ・プレーンは、クラスターが既に接続されている VLAN を使用します。 構成トラフィックはクラスター内のプライベート・ネットワーク上を流れるので、ファイアウォール内に追加のポートや IP アドレスを開く必要はありません。 Istio Gateway を使用して Istio 管理対象アプリを公開する場合は、アプリに対する外部トラフィック要求がパブリック VLAN 上を流れます。

**更新処理はどんな仕組みで行われますか?**</br>
管理対象アドオン内の Istio のバージョンが {{site.data.keyword.Bluemix_notm}} によって検査され、{{site.data.keyword.containerlong_notm}} での使用が受け入れられます。 ご使用の Istio コンポーネントを {{site.data.keyword.containerlong_notm}} でサポートされている最新バージョンの Istio に更新するには、[管理対象アドオンの更新](/docs/containers?topic=containers-managed-addons#updating-managed-add-ons)の手順に従ってください。  

最新バージョンの Istio を使用する必要がある場合、または Istio のインストールをカスタマイズする必要がある場合は、[{{site.data.keyword.Bluemix_notm}} を使用したクイック・スタートのチュートリアル ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://istio.io/docs/setup/kubernetes/quick-start-ibm/) の手順に従って、オープン・ソース版の Istio をインストールしてください。
{: tip}

**何か制限はありますか?** </br>
[Container Image Security Enforcer アドミッション・コントローラー](/docs/services/Registry?topic=registry-security_enforce#security_enforce)をクラスター内にインストールした場合は、クラスター内で管理対象 Istio アドオンを有効にすることはできません。

<br />


## 何をインストールできますか?
{: #istio_components}

{{site.data.keyword.containerlong_notm}} 上の Istio は、クラスター内の 3 つの管理対象アドオンとして提供されます。
{: shortdesc}

<dl>
<dt>Istio (`istio`)</dt>
<dd>Prometheus を含め、Istio のコア・コンポーネントをインストールします。 以下のコントロール・プレーン・コンポーネントのいずれについても詳しくは、[Istio の資料 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://istio.io/docs/concepts/what-is-istio/) を参照してください。
  <ul><li>`Envoy` は、メッシュ内のすべてのサービスのインバウンド・トラフィックとアウトバウンド・トラフィックのプロキシーになります。 Envoy は、アプリ・コンテナーと同じポッド内にサイドカー・コンテナーとしてデプロイされます。</li>
  <li>`Mixer` は、テレメトリー・コレクションとポリシー制御を行います。<ul>
    <li>テレメトリー・ポッドは、アプリ・ポッド内の Envoy プロキシーのサイドカーとサービスからすべてのテレメトリー・データを集約する Prometheus エンドポイントとともに使用可能になります。</li>
    <li>ポリシー・ポッドは、速度制限やホワイトリスト・ポリシーとブラックリスト・ポリシーの適用を含め、アクセス制御を実施します。</li></ul>
  </li>
  <li>`Pilot` は、Envoy サイドカーのためのサービス・ディスカバリーを行い、サイドカーのためのトラフィック管理ルーティング・ルールを構成します。</li>
  <li>`Citadel` は、ID と資格情報の管理を使用して、サービス間認証とエンド・ユーザー認証を行います。</li>
  <li>`Galley` は、他の Istio コントロール・プレーン・コンポーネントの構成変更を検証します。</li>
</ul></dd>
<dt>Istio 追加機能 (`istio-extras`)</dt>
<dd>オプション: [Grafana ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://grafana.com/)、[Jaeger ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.jaegertracing.io/)、[Kiali ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.kiali.io/) をインストールして、Istio で追加のモニタリング、トレース、視覚化を行えるようにします。</dd>
<dt>BookInfo サンプル・アプリ (`istio-sample-bookinfo`)</dt>
<dd>オプション: [Istio 用 BookInfo サンプル・アプリケーション ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://istio.io/docs/examples/bookinfo/) をデプロイします。 このデプロイメントには、Istio の機能を直ちに試すことができるように、基本デモがセットアップされ、デフォルトの宛先ルールが含まれています。</dd>
</dl>

<br>
次のコマンドを実行することにより、クラスター内でどの Istio アドオンが有効になっているかをいつでも確認できます。
```
ibmcloud ks cluster-addons --cluster <cluster_name_or_ID>
```
{: pre}

<br />


## {{site.data.keyword.containerlong_notm}} での Istio のインストール
{: #istio_install}

既存のクラスターに Istio 管理対象アドオンをインストールします。
{: shortdesc}

**始める前に**</br>
* {{site.data.keyword.containerlong_notm}} に対する[**ライター**または**管理者**の {{site.data.keyword.Bluemix_notm}} IAM サービス役割](/docs/containers?topic=containers-users#platform)があることを確認してください。
* [それぞれが 4 コアと 16 GB のメモリー (`b3c.4x16`) 以上を備えた、少なくとも 3 つのワーカー・ノードのある既存の標準クラスターを使用するか、このようなクラスターを作成します](/docs/containers?topic=containers-clusters#clusters_ui)。 さらに、クラスターおよびワーカー・ノードで、少なくともサポートされる最小バージョンの Kubernetes を実行する必要があります。これは、`ibmcloud ks addon-versions --addon istio` を実行して確認できます。
* [CLI のターゲットを自分のクラスターに設定します](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)。
* 既存のクラスターを使用している場合に、IBM Helm チャートまたは別の方法を使用して、そのクラスターに Istio を既にインストールしている場合は、[その Istio インストール済み環境をクリーンアップしてください](#istio_uninstall_other)。

### CLI での管理対象 Istio アドオンのインストール
{: #istio_install_cli}

1. `istio` アドオンを有効にします。
  ```
  ibmcloud ks cluster-addon-enable istio --cluster <cluster_name_or_ID>
  ```
  {: pre}

2. オプション: `istio-extras` アドオンを有効にします。
  ```
  ibmcloud ks cluster-addon-enable istio-extras --cluster <cluster_name_or_ID>
  ```
  {: pre}

3. オプション: `istio-sample-bookinfo` アドオンを有効にします。
  ```
  ibmcloud ks cluster-addon-enable istio-sample-bookinfo --cluster <cluster_name_or_ID>
  ```
  {: pre}

4. インストールした管理対象 Istio アドオンがこのクラスター内で有効になっていることを確認します。
  ```
  ibmcloud ks cluster-addons --cluster <cluster_name_or_ID>
  ```
  {: pre}

  出力例:
  ```
  Name                      Version
  istio                     1.1.5
  istio-extras              1.1.5
  istio-sample-bookinfo     1.1.5
  ```
  {: screen}

5. クラスター内の各アドオンの個別コンポーネントを確認することもできます。
  - `istio` と `istio-extras` のコンポーネント: Istio サービスとそれらに対応するポッドがデプロイされていることを確認します。
    ```
    kubectl get svc -n istio-system
    ```
    {: pre}

    ```
    NAME                     TYPE           CLUSTER-IP       EXTERNAL-IP                                                                    AGE
    grafana                  ClusterIP      172.21.98.154    <none>          3000/TCP                                                       2m
    istio-citadel            ClusterIP      172.21.221.65    <none>          8060/TCP,9093/TCP                                              2m
    istio-egressgateway      ClusterIP      172.21.46.253    <none>          80/TCP,443/TCP                                                 2m
    istio-galley             ClusterIP      172.21.125.77    <none>          443/TCP,9093/TCP                                               2m
    istio-ingressgateway     LoadBalancer   172.21.230.230   169.46.56.125   80:31380/TCP,443:31390/TCP,31400:31400/TCP,5011:31323/TCP,
                                                                              8060:32483/TCP,853:32628/TCP,15030:31601/TCP,15031:31915/TCP   2m
    istio-pilot              ClusterIP      172.21.171.29    <none>          15010/TCP,15011/TCP,8080/TCP,9093/TCP                          2m
    istio-policy             ClusterIP      172.21.140.180   <none>          9091/TCP,15004/TCP,9093/TCP                                    2m
    istio-sidecar-injector   ClusterIP      172.21.248.36    <none>          443/TCP                                                        2m
    istio-telemetry          ClusterIP      172.21.204.173   <none>          9091/TCP,15004/TCP,9093/TCP,42422/TCP                          2m
    jaeger-agent             ClusterIP      None             <none>          5775/UDP,6831/UDP,6832/UDP                                     2m
    jaeger-collector         ClusterIP      172.21.65.195    <none>          14267/TCP,14268/TCP                                            2m
    jaeger-query             ClusterIP      172.21.171.199   <none>          16686/TCP                                                      2m
    kiali                    ClusterIP      172.21.13.35     <none>          20001/TCP                                                      2m
    prometheus               ClusterIP      172.21.105.229   <none>          9090/TCP                                                       2m
    tracing                  ClusterIP      172.21.125.177   <none>          80/TCP                                                         2m
    zipkin                   ClusterIP      172.21.1.77      <none>          9411/TCP                                                       2m
    ```
    {: screen}

    ```
    kubectl get pods -n istio-system
    ```
    {: pre}

    ```
    NAME                                      READY   STATUS    RESTARTS   AGE
    grafana-76dcdfc987-94ldq                  1/1     Running   0          2m
    istio-citadel-869c7f9498-wtldz            1/1     Running   0          2m
    istio-egressgateway-69bb5d4585-qxxbp      1/1     Running   0          2m
    istio-galley-75d7b5bdb9-c9d9n             1/1     Running   0          2m
    istio-ingressgateway-5c8764db74-gh8xg     1/1     Running   0          2m
    istio-pilot-55fd7d886f-vv6fb              2/2     Running   0          2m
    istio-policy-6bb6f6ddb9-s4c8t             2/2     Running   0          2m
    istio-sidecar-injector-7d9845dbb7-r8nq5   1/1     Running   0          2m
    istio-telemetry-7695b4c4d4-tlvn8          2/2     Running   0          2m
    istio-tracing-55bbf55878-z4rd2            1/1     Running   0          2m
    kiali-77566cc66c-kh6lm                    1/1     Running   0          2m
    prometheus-5d5cb44877-lwrqx               1/1     Running   0          2m
    ```
    {: screen}

  - `istio-sample-bookinfo` のコンポーネント: BookInfo マイクロサービスとそれらに対応するポッドがデプロイされていることを確認します。
    ```
    kubectl get svc -n default
    ```
    {: pre}

    ```
    NAME                      TYPE           CLUSTER-IP       EXTERNAL-IP    PORT(S)          AGE
    details                   ClusterIP      172.21.19.104    <none>         9080/TCP         2m
    productpage               ClusterIP      172.21.168.196   <none>         9080/TCP         2m
    ratings                   ClusterIP      172.21.11.131    <none>         9080/TCP         2m
    reviews                   ClusterIP      172.21.117.164   <none>         9080/TCP         2m
    ```
    {: screen}

    ```
    kubectl get pods -n default
    ```
    {: pre}

    ```
    NAME                                     READY     STATUS      RESTARTS   AGE
    details-v1-6865b9b99d-7v9h8              2/2       Running     0          2m
    productpage-v1-f8c8fb8-tbsz9             2/2       Running     0          2m
    ratings-v1-77f657f55d-png6j              2/2       Running     0          2m
    reviews-v1-6b7f6db5c5-fdmbq              2/2       Running     0          2m
    reviews-v2-7ff5966b99-zflkv              2/2       Running     0          2m
    reviews-v3-5df889bcff-nlmjp              2/2       Running     0          2m
    ```
    {: screen}

### UI での管理対象 Istio アドオンのインストール
{: #istio_install_ui}

1. [クラスター・ダッシュボード ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/kubernetes/clusters) で、クラスターの名前をクリックします。

2. **「アドオン」**タブをクリックします。

3. Istio カード上で**「インストール」**をクリックします。

4. **「Istio」**チェック・ボックスが既に選択されています。 Istio 追加機能と BookInfo サンプル・アプリもインストールするには、**「Istio 追加機能 (Istio Extras)」**チェック・ボックスと**「Istio サンプル (Istio Sample)」**チェック・ボックスを選択します。

5. **「インストール」**をクリックします。

6. Istio カード上で、有効にしたアドオンがリストされていることを確認します。

次に、[BookInfo サンプル・アプリ](#istio_bookinfo)をチェックアウトすることにより、Istio の機能を試してみることができます。

<br />


## BookInfo サンプル・アプリを試してみる
{: #istio_bookinfo}

BookInfo アドオン (`istio-sample-bookinfo`) は、[Istio 用 BookInfo サンプル・アプリケーション ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://istio.io/docs/examples/bookinfo/) を `default` 名前空間にデプロイします。 このデプロイメントには、Istio の機能を直ちに試すことができるように、基本デモがセットアップされ、デフォルトの宛先ルールが含まれています。
{: shortdesc}

以下の 4 つの BookInfo マイクロサービスがあります。
* `productpage` は、`details` マイクロサービスと `reviews` マイクロサービスを呼び出して、ページに内容を取り込みます。
* `details` には本の情報が含まれています。
* `reviews` は本のレビューを含んでいて、`ratings` マイクロサービスを呼び出します。
* `ratings` には、本のレビューに伴う本のランキング情報が含まれています。

`reviews` マイクロサービスには、以下のような複数のバージョンがあります。
* `v1` は `ratings` マイクロサービスを呼び出しません。
* `v2` は `ratings` マイクロサービスを呼び出し、1 つから 5 つまでの黒色の星印で評価を表示します。
* `v3` は `ratings` マイクロサービスを呼び出し、1 つから 5 つまでの赤色の星印で評価を表示します。

これらのマイクロサービスそれぞれのデプロイメント YAML は、それらがデプロイされる前に Envoy サイドカー・プロキシーがマイクロサービスのポッドにコンテナーとして事前注入されるように、変更されています。 手動サイドカー注入について詳しくは、[Istio の資料 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://istio.io/docs/setup/kubernetes/sidecar-injection/) を参照してください。 BookInfo アプリは、Istio Gateway によってパブリック IP ingress アドレスでも既に公開されています。 BookInfo アプリは始めるのに役立ちますが、実動用ではありません。

始める前に、クラスターに [`istio`、`istio-extras`、`istio-sample-bookinfo` の各管理対象アドオンをインストールします](#istio_install)。

1. クラスターのパブリック・アドレスを取得します。
  1. ingress ホストを設定します。
    ```
    export INGRESS_HOST=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.status.loadBalancer.ingress[0].ip}')
    ```
    {: pre}

  2. ingress ポートを設定します。
    ```
    export INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="http2")].port}')
    ```
    {: pre}

  3. ingress のホストとポートを使用する `GATEWAY_URL` 環境変数を作成します。
     ```
     export GATEWAY_URL=$INGRESS_HOST:$INGRESS_PORT
     ```
     {: pre}

2. `GATEWAY_URL` 変数に対して curl を実行し、BookInfo アプリが稼働中であることを確認します。 応答 `200` は、Istio で BookInfo アプリが適切に稼働していることを意味します。
   ```
   curl -o /dev/null -s -w "%{http_code}\n" http://${GATEWAY_URL}/productpage
   ```
   {: pre}

3.  ブラウザーで BookInfo Web ページを表示します。

    Mac OS または Linux の場合:
    ```
    open http://$GATEWAY_URL/productpage
    ```
    {: pre}

    Windows:
    ```
    start http://$GATEWAY_URL/productpage
    ```
    {: pre}

4. ページの最新表示を複数回試行します。 さまざまなバージョンのレビュー・セクションが、赤い星形、黒い星形、星形なしの間でラウンドロビンします。

### 何が起きたかを理解する
{: #istio_bookinfo_understanding}

BookInfo サンプルは、Istio のトラフィック管理コンポーネントの 3 つがどのように連携して受信トラフィックをアプリにルーティングするかを示すデモ用サンプルです。
{: shortdesc}

<dl>
<dt>`Gateway`</dt>
<dd>`bookinfo-gateway` [Gateway ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://istio.io/docs/reference/config/networking/v1alpha3/gateway/) は、ロード・バランサーである `istio-system` 名前空間内の `istio-ingressgateway` サービスを表し、BookInfo の HTTP/TCP トラフィックの受信エントリー・ポイントとして機能します。 Istio は、ゲートウェイ構成ファイル内に定義されたポート上で Istio 管理対象アプリに対する着信要求を listen するようにロード・バランサーを構成します。
</br></br>BookInfo ゲートウェイの構成ファイルを調べるには、次のコマンドを実行します。
<pre class="pre"><code>kubectl get gateway bookinfo-gateway -o yaml</code></pre></dd>

<dt>`VirtualService`</dt>
<dd>`bookinfo` [`VirtualService` ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://istio.io/docs/reference/config/networking/v1alpha3/virtual-service/) は、マイクロサービスを `destinations` として定義することによって、サービス・メッシュ内で要求をどのようにルーティングするかを制御するルールを定義します。 `bookinfo` 仮想サービスでは、要求の `/productpage` URI はポート `9080` 上の `productpage` ホストにルーティングされます。 このように、BookInfo アプリに対する要求はすべて、まず `productpage` マイクロサービスにルーティングされ、そこで BookInfo の他のマイクロサービスが呼び出されます。
</br></br>BookInfo に適用される仮想サービス・ルールを調べるには、次のコマンドを実行します。
<pre class="pre"><code>kubectl get virtualservice bookinfo -o yaml</code></pre></dd>

<dt>`DestinationRule`</dt>
<dd>仮想サービス・ルールに従ってゲートウェイが要求をルーティングした後、`details`、`productpage`、`ratings`、および `reviews` の [`DestinationRules` ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://istio.io/docs/reference/config/networking/v1alpha3/destination-rule/) は、要求がマイクロサービスに到達したときにその要求に適用されるポリシーを定義します。 例えば、BookInfo 製品ページを最新表示すると表示が変わるのは、異なるバージョン (`v1`、`v2`、`v3`) の `reviews` マイクロサービスをランダムに呼び出す `productpage` マイクロサービスの結果です。 これらのバージョンはランダムに選択されます。これは、`reviews` 宛先ルールでマイクロサービスの `subsets` (名前付きバージョン) に均等な重みが与えられているためです。 これらのサブセットは、特定のバージョンのサービスにトラフィックがルーティングされるときに仮想サービス・ルールによって使用されます。
</br></br>BookInfo に適用される宛先ルールを調べるには、次のコマンドを実行します。
<pre class="pre"><code>kubectl describe destinationrules</code></pre></dd>
</dl>

</br>

次に、[IBM 提供の Ingress サブドメインを使用して BookInfo を公開する](#istio_expose_bookinfo)ことも、BookInfo アプリ用のサービス・メッシュの[ロギング、モニタリング、トレース、視覚化を行う](#istio_health)こともできます。

<br />


## Istio のロギング、モニタリング、トレース、視覚化
{: #istio_health}

{{site.data.keyword.containerlong_notm}} 上の Istio によって管理されるアプリのロギング、モニタリング、トレース、視覚化を行うには、`istio-extras` アドオン内にインストールされる Grafana、Jaeger、Kiali のダッシュボードを起動するか、LogDNA と Sysdig をサード・パーティー・サービスとしてワーカー・ノードにデプロイします。
{: shortdesc}

### Grafana、Jaeger、Kiali のダッシュボードの起動
{: #istio_health_extras}

Istio 追加機能アドオン (`istio-extras`) は、[Grafana ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://grafana.com/)、[Jaeger ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.jaegertracing.io/)、[Kiali ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.kiali.io/) をインストールします。 Istio で追加のモニタリング、トレース、視覚化を行うには、これらのサービスそれぞれのダッシュボードを起動します。
{: shortdesc}

始める前に、クラスターに [`istio` 管理対象アドオンと `istio-extras` 管理対象アドオンをインストールします](#istio_install)。

**Grafana**</br>
1. Grafana ダッシュボードのための Kubernetes ポート転送を開始します。
  ```
  kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=grafana -o jsonpath='{.items[0].metadata.name}') 3000:3000 &
  ```
  {: pre}

2. Istio Grafana ダッシュボードを開くには、URL http://localhost:3000/dashboard/db/istio-mesh-dashboard に移動します。 [BookInfo アドオン](#istio_bookinfo)がインストールされている場合は、製品ページを何回か最新表示したときに生成されたトラフィックのメトリックが Istio ダッシュボードに表示されます。 Istio Grafana ダッシュボードの使用について詳しくは、Istio オープン・ソース資料内の [Viewing the Istio Dashboard ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://istio.io/docs/tasks/telemetry/using-istio-dashboard/) を参照してください。

**Jaeger**</br>
1. デフォルトでは、Istio では 100 件の要求中 1 件という割合でトレース・スパンが生成され、これは 1% というサンプリング・レートに相当します。最初のトレースが表示されるためには、その前に少なくとも 100 件の要求を送信する必要があります。 100 件の要求を [BookInfo アドオン](#istio_bookinfo)の `productpage` サービスに送信するには、次のコマンドを実行します。
  ```
  for i in `seq 1 100`; do curl -s -o /dev/null http://$GATEWAY_URL/productpage; done
  ```
  {: pre}

2. Jaeger ダッシュボードのための Kubernetes ポート転送を開始します。
  ```
  kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=jaeger -o jsonpath='{.items[0].metadata.name}') 16686:16686 &
  ```
  {: pre}

3. Jaeger UI を開くには、URL http://localhost:16686 に移動します。

4. BookInfo アドオンがインストールされている場合は、**「サービス」**リストから `productpage` を選択し、**「トレースの検索 (Find Traces)」**をクリックします。 製品ページを何回か最新表示したときに生成されたトラフィックのトレースが表示されます。 Istio での Jaeger の使用について詳しくは、Istio オープン・ソース資料内の [Generating traces using the BookInfo sample ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://istio.io/docs/tasks/telemetry/distributed-tracing/#generating-traces-using-the-bookinfo-sample) を参照してください。

**Kiali**</br>
1. Kiali ダッシュボードのための Kubernetes ポート転送を開始します。
  ```
  kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=kiali -o jsonpath='{.items[0].metadata.name}') 20001:20001 &
  ```
  {: pre}

2. Kiali UI を開くには、URL http://localhost:20001/kiali/console に移動します。

3. ユーザー名とパスフレーズの両方に `admin` と入力します。 Kiali を使用して Istio 管理対象マイクロサービスを視覚化する方法について詳しくは、Istio オープン・ソース資料内の [Generating a service graph ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://archive.istio.io/v1.0/docs/tasks/telemetry/kiali/#generating-a-service-graph) を参照してください。

### {{site.data.keyword.la_full_notm}} を使用するロギングのセットアップ
{: #istio_health_logdna}

ワーカー・ノードに LogDNA をデプロイしてログを {{site.data.keyword.loganalysislong}} に転送することにより、各ポッド内のアプリ・コンテナーと Envoy プロキシー・サイドカー・コンテナーのログをシームレスに管理できます。
{: shortdesc}

[{{site.data.keyword.la_full_notm}}](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-about) を使用するには、クラスター内のどのワーカー・ノードにもロギング・エージェントをデプロイします。 このエージェントは、`kube-system` などのすべての名前空間から、ポッドの `/var/log` ディレクトリーに保管されている拡張子のないファイルと拡張子が `*.log` のログを収集します。 これらのログには、各ポッド内のアプリ・コンテナーと Envoy プロキシー・サイドカー・コンテナーからのログが含まれます。 そして、それらのログを {{site.data.keyword.la_full_notm}} サービスに転送します。

始めに、[{{site.data.keyword.la_full_notm}} による Kubernetes クラスター・ログの管理](/docs/services/Log-Analysis-with-LogDNA/tutorials?topic=LogDNA-kube#kube)の手順に従って、クラスター用の LogDNA をセットアップします。




### {{site.data.keyword.mon_full_notm}} を使用するモニタリングのセットアップ
{: #istio_health_sysdig}

Sysdig をワーカー・ノードにデプロイしてメトリックを {{site.data.keyword.monitoringlong}} に転送することによって、Istio 管理対象アプリのパフォーマンスと正常性を可視化して運用に役立てることができます。
{: shortdesc}

{{site.data.keyword.containerlong_notm}} 上の Istio では、管理対象 `istio` アドオンはクラスターに Prometheus をインストールします。 ポッドのすべてのテレメトリー・データを Prometheus が集約できるように、クラスター内の `istio-mixer-telemetry` ポッドに Prometheus エンドポイントのアノテーションが付けられます。 クラスター内のすべてのワーカー・ノードに Sysdig エージェントをデプロイした時点で既に Sysdig が自動的に有効になっているので、これらの Prometheus エンドポイントからデータが検出されて収集され、それらが {{site.data.keyword.Bluemix_notm}} モニタリング・ダッシュボードに表示されます。

Prometheus に関する作業はすべて行われているので、残っているのはクラスターに Sysdig をデプロイすることだけです。

1. [Kubernetes クラスターにデプロイされたアプリのメトリックの分析](/docs/services/Monitoring-with-Sysdig/tutorials?topic=Sysdig-kubernetes_cluster#kubernetes_cluster) の手順に従って、Sysdig をセットアップします。

2. [Sysdig UI を起動します ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/services/Monitoring-with-Sysdig/tutorials?topic=Sysdig-kubernetes_cluster#kubernetes_cluster_step3)。

3. **「新規ダッシュボードの追加 (Add new dashboard)」**をクリックします。

4. `Istio` を検索し、Sysdig で事前定義された Istio ダッシュボードのうちの 1 つを選択します。

メトリックとダッシュボードの参照、Istio 内部コンポーネントのモニター、および Istio A/B デプロイメントとカナリア・デプロイメントのモニターについて詳しくは、[How to monitor Istio, the Kubernetes service mesh ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://sysdig.com/blog/monitor-istio/) を確認してください。ブログ投稿の『Monitoring Istio: reference metrics and dashboards』というセクションを探してください。

<br />


## アプリのためのサイドカー注入のセットアップ
{: #istio_sidecar}

Istio を使用して独自のアプリを管理する準備ができましたか? アプリをデプロイする前に、まず、アプリ・ポッドに Envoy プロキシー・サイドカーを注入する方法を決める必要があります。
{: shortdesc}

マイクロサービスをサービス・メッシュに含めるためには、各アプリ・ポッドで Envoy プロキシー・サイドカーが実行されていなければなりません。 各アプリ・ポッドへのサイドカーの注入は、自動的に行われるようにすることも、手動で行うこともできます。 サイドカー注入について詳しくは、[Istio の資料 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://istio.io/docs/setup/kubernetes/sidecar-injection/) を参照してください。

### 自動サイドカー注入の有効化
{: #istio_sidecar_automatic}

自動サイドカー注入を有効にすると、名前空間によって新規デプロイメントの有無が listen されて、ポッド・テンプレート仕様が自動的に変更されて、Envoy プロキシー・サイドカー・コンテナーを使用してアプリ・ポッドが作成されるようになります。 Istio と統合する複数のアプリをある名前空間にデプロイする場合は、その名前空間に対して自動サイドカー注入を有効にします。 Istio 管理対象アドオンでは、どの名前空間に対しても自動サイドカー注入はデフォルトで有効にはなりません。

名前空間に対して自動サイドカー注入を有効にするには、以下のようにします。

1. Istio 管理対象アプリをデプロイする名前空間の名前を取得します。
  ```
  kubectl get namespaces
  ```
  {: pre}

2. 名前空間に `istio-injection=enabled` というラベルを設定します。
  ```
  kubectl label namespace <namespace> istio-injection=enabled
  ```
  {: pre}

3. ラベル付き名前空間にアプリをデプロイするか、その名前空間に既に存在するアプリを再デプロイします。
  * ラベル付き名前空間にアプリをデプロイするには、次のようにします。
    ```
    kubectl apply <myapp>.yaml --namespace <namespace>
    ```
    {: pre}
  * その名前空間に既にデプロイされているアプリを再デプロイするには、注入されるサイドカーとともに再デプロイされるようにアプリ・ポッドを削除します。
    ```
    kubectl delete pod -l app=<myapp>
    ```
    {: pre}

5. アプリを公開するためのサービスを作成していない場合は、Kubernetes サービスを作成します。 Istio サービス・メッシュ内にマイクロサービスとして含める Kubernetes サービスによってアプリが公開されなければなりません。 [ポッドとサービスのための Istio の要件 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://istio.io/docs/setup/kubernetes/spec-requirements/) に従うようにしてください。

  1. アプリ用のサービスを定義します。
    ```
    apiVersion: v1
          kind: Service
          metadata:
            name: myappservice
          spec:
            selector:
              <selector_key>: <selector_value>
            ports:
       - protocol: TCP
         port: 8080
    ```
    {: codeblock}

    <table>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> サービス YAML ファイルの構成要素について</th>
    </thead>
    <tbody>
    <tr>
    <td><code>selector</code></td>
    <td>アプリが実行されるポッドをターゲットにするために使用する、ラベル・キー (<em>&lt;selector_key&gt;</em>) と値 (<em>&lt;selector_value&gt;</em>) のペアを入力します。</td>
     </tr>
     <tr>
     <td><code>port</code></td>
     <td>サービスが listen するポート。</td>
     </tr>
     </tbody></table>

  2. クラスター内にサービスを作成します。 アプリと同じ名前空間にサービスがデプロイされるようにします。
    ```
    kubectl apply -f myappservice.yaml -n <namespace>
    ```
    {: pre}

これで、Istio サービス・メッシュにアプリ・ポッドが組み込まれました。アプリ・ポッドではアプリ・コンテナーとともに Istio サイドカー・コンテナーが実行されるからです。

### サイドカーの手動注入
{: #istio_sidecar_manual}

名前空間での自動サイドカー注入を有効にしないようにする場合は、手動でデプロイメント YAML にサイドカーを注入できます。 サイドカーが自動的に注入されないようにする他のデプロイメントとともにアプリが名前空間で実行される場合は、手動でサイドカーを注入します。

手動でデプロイメントにサイドカーを注入するには、以下のようにします。

1. `istioctl` クライアントをダウンロードします。
  ```
  curl -L https://git.io/getLatestIstio | ISTIO_VERSION=1.1.5 sh -
  ```

2. Istio パッケージ・ディレクトリーにナビゲートします。
  ```
  cd istio-1.1.5
  ```
  {: pre}

3. アプリ・デプロイメント YAML に Envoy サイドカーを注入します。
  ```
  istioctl kube-inject -f <myapp>.yaml | kubectl apply -f -
  ```
  {: pre}

4. アプリをデプロイします。
  ```
  kubectl apply <myapp>.yaml
  ```
  {: pre}

5. アプリを公開するためのサービスを作成していない場合は、Kubernetes サービスを作成します。 Istio サービス・メッシュ内にマイクロサービスとして含める Kubernetes サービスによってアプリが公開されなければなりません。 [ポッドとサービスのための Istio の要件 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://istio.io/docs/setup/kubernetes/spec-requirements/) に従うようにしてください。

  1. アプリ用のサービスを定義します。
    ```
    apiVersion: v1
          kind: Service
          metadata:
            name: myappservice
          spec:
            selector:
              <selector_key>: <selector_value>
            ports:
       - protocol: TCP
         port: 8080
    ```
    {: codeblock}

    <table>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> サービス YAML ファイルの構成要素について</th>
    </thead>
    <tbody>
    <tr>
    <td><code>selector</code></td>
    <td>アプリが実行されるポッドをターゲットにするために使用する、ラベル・キー (<em>&lt;selector_key&gt;</em>) と値 (<em>&lt;selector_value&gt;</em>) のペアを入力します。</td>
     </tr>
     <tr>
     <td><code>port</code></td>
     <td>サービスが listen するポート。</td>
     </tr>
     </tbody></table>

  2. クラスター内にサービスを作成します。 アプリと同じ名前空間にサービスがデプロイされるようにします。
    ```
    kubectl apply -f myappservice.yaml -n <namespace>
    ```
    {: pre}

これで、Istio サービス・メッシュにアプリ・ポッドが組み込まれました。アプリ・ポッドではアプリ・コンテナーとともに Istio サイドカー・コンテナーが実行されるからです。

<br />


## IBM 提供のホスト名を使用した Istio 管理対象アプリの公開
{: #istio_expose}

[Envoy プロキシー・サイドカー注入をセットアップ](#istio_sidecar)し、Istio サービス・メッシュにアプリをデプロイしたら、IBM 提供のホスト名を使用して、パブリック要求に対して Istio 管理対象アプリを公開できるようになります。
{: shortdesc}

Istio は、[Gateway ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://istio.io/docs/reference/config/networking/v1alpha3/gateway/) と [VirtualService ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://istio.io/docs/reference/config/networking/v1alpha3/virtual-service/) を使用して、トラフィックをアプリにどのようにルーティングするかを制御します。 ゲートウェイでは、Istio 管理対象アプリのエントリー・ポイントとして機能するロード・バランサー `istio-ingressgateway` が構成されます。 `istio-ingressgateway` ロード・バランサーの外部 IP アドレスを DNS エントリーおよび DNS ホスト名に登録することで、Istio 管理対象アプリを公開できます。

[BookInfo を公開する例](#istio_expose_bookinfo)をまず試すことも、[独自の Istio 管理対象アプリをパブリックに公開](#istio_expose_link)することもできます。

### 例: IBM 提供のホスト名を使用した BookInfo の公開
{: #istio_expose_bookinfo}

クラスター内で BookInfo アドオンを有効にすると、Istio ゲートウェイ `bookinfo-gateway` が自動的に作成されます。 このゲートウェイは、Istio の仮想サービス・ルールと宛先ルールを使用して、BookInfoアプリをパブリックに公開するロード・バランサー `istio-ingressgateway` を構成します。 以下の手順では、`istio-ingressgateway` ロード・バランサーの IP アドレスのホスト名を作成します。このホスト名を通じて BookInfo にパブリック・アクセスできます。
{: shortdesc}

開始前に、クラスター内で [`istio-sample-bookinfo` という管理対象アドオンを有効にします](#istio_install)。

1. `istio-ingressgateway` ロード・バランサーの **EXTERNAL-IP** アドレスを取得します。
  ```
  kubectl get svc -n istio-system
  ```
  {: pre}

  次の出力例では、**EXTERNAL-IP** は `168.1.1.1` です。
  ```
  NAME                     TYPE           CLUSTER-IP       EXTERNAL-IP                                                                    AGE
  ...
  istio-ingressgateway     LoadBalancer   172.21.XXX.XXX   169.1.1.1       80:31380/TCP,443:31390/TCP,31400:31400/TCP,5011:31323/TCP,
                                                                            8060:32483/TCP,853:32628/TCP,15030:31601/TCP,15031:31915/TCP  22m
  ```
  {: screen}

2. DNS ホスト名を作成することによって、この IP を登録します。
  ```
  ibmcloud ks nlb-dns-create --cluster <cluster_name_or_id> --ip <LB_IP>
  ```
  {: pre}

3. ホスト名が作成されたことを確認します。
  ```
  ibmcloud ks nlb-dnss --cluster <cluster_name_or_id>
  ```
  {: pre}

  出力例:
  ```
  Hostname                                                                                IP(s)              Health Monitor   SSL Cert Status           SSL Cert Secret Name
  mycluster-a1b2cdef345678g9hi012j3kl4567890-0001.us-south.containers.appdomain.cloud     ["168.1.1.1"]      None             created                   <certificate>
  ```
  {: screen}

4. Web ブラウザーで、BookInfo 製品ページを開きます。
  ```
  https://<host_name>/productpage
  ```
  {: codeblock}

5. ページの最新表示を複数回試行します。 `http://<host_name>/productpage` への要求は ALB で受信されて Istio ゲートウェイ・ロード・バランサーに転送されます。 Istio ゲートウェイはマイクロサービス用の仮想サービス・ルールと宛先ルーティング・ルールを管理するため、依然として複数の異なるバージョンの `reviews` マイクロサービスがランダムに返されます。

BookInfo アプリのゲートウェイ、仮想サービス・ルール、宛先ルールについて詳しくは、[何が起きたかを理解する](#istio_bookinfo_understanding)を参照してください。 {{site.data.keyword.containerlong_notm}} での DNS ホスト名の登録について詳しくは、[NLB ホスト名の登録](/docs/containers?topic=containers-loadbalancer#loadbalancer_hostname)を参照してください。

### IBM 提供のホスト名を使用した Istio 管理対象アプリのパブリック公開
{: #istio_expose_link}

Istio 管理対象アプリをパブリックに公開するには、Istio ゲートウェイを作成して、Istio 管理対象サービス用のトラフィック管理ルールを定義する仮想サービスを作成して、`istio-ingressgateway` ロード・バランサーの外部 IP アドレスの DNS ホスト名を作成します。
{: shortdesc}

**開始前に、以下のことを行います。**
1. クラスターに [`istio` 管理対象アドオンをインストール](#istio_install)します。
2. `istioctl` クライアントをインストールします。
  1. `istioctl` をダウンロードします。
    ```
    curl -L https://git.io/getLatestIstio | sh -
    ```
  2. Istio パッケージ・ディレクトリーにナビゲートします。
    ```
    cd istio-1.1.5
    ```
    {: pre}
3. [アプリ・マイクロサービスのためのサイドカー注入をセットアップし、アプリ・マイクロサービスを名前空間にデプロイします。さらに、アプリ・マイクロサービスを Istio サービス・メッシュに組み込めるように、アプリ・マイクロサービスのための Kubernetes サービスを作成します](#istio_sidecar)。

</br>
**ホスト名を使用して Istio 管理対象アプリをパブリックに公開するには、次のようにします。
**

1. ゲートウェイを作成します。 このサンプル・ゲートウェイは、`istio-ingressgateway` ロード・バランサー・サービスを使用して、HTTP 用のポート 80 を公開します。 `<namespace>` を、Istio 管理対象マイクロサービスがデプロイされている名前空間に置き換えてください。 `80` とは異なるポートでマイクロサービスが listen する場合は、そのポートを追加してください。 ゲートウェイ YAML の構成要素について詳しくは、[Istio のリファレンス資料 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://istio.io/docs/reference/config/networking/v1alpha3/gateway/) を参照してください。
  ```
  apiVersion: networking.istio.io/v1alpha3
  kind: Gateway
  metadata:
    name: my-gateway
    namespace: <namespace>
  spec:
    selector:
      istio: ingressgateway
    servers:
    - port:
        name: http
        number: 80
        protocol: HTTP
      hosts:
      - '*'
  ```
  {: codeblock}

2. Istio 管理対象マイクロサービスがデプロイされている名前空間にゲートウェイを適用します。
  ```
  kubectl apply -f my-gateway.yaml -n <namespace>
  ```
  {: pre}

3. `my-gateway` ゲートウェイを使用するとともにアプリ・マイクロサービスのためのルーティング・ルールを定義する仮想サービスを作成します。 仮想サービス YAML の構成要素について詳しくは、[Istio のリファレンス資料 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://istio.io/docs/reference/config/networking/v1alpha3/virtual-service/) を参照してください。
  ```
  apiVersion: networking.istio.io/v1alpha3
  kind: VirtualService
  metadata:
    name: my-virtual-service
    namespace: <namespace>
  spec:
    gateways:
    - my-gateway
    hosts:
    - '*'
    http:
    - match:
      - uri:
          exact: /<service_path>
      route:
      - destination:
          host: <service_name>
          port:
            number: 80
  ```
  {: codeblock}

  <table>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> YAML ファイルの構成要素について</th>
  </thead>
  <tbody>
  <tr>
  <td><code>namespace</code></td>
  <td><em>&lt;namespace&gt;</em> を、Istio 管理対象マイクロサービスがデプロイされている名前空間に置き換えてください。</td>
  </tr>
  <tr>
  <td><code>gateways</code></td>
  <td><code>my-gateway</code> が指定されているのは、このゲートウェイにより、これらの仮想サービス・ルーティング・ルールを <code>istio-ingressgateway</code> ロード・バランサーに適用できるようにするためです。<td>
  </tr>
  <tr>
  <td><code>http.match.uri.exact</code></td>
  <td><em>&lt;service_path&gt;</em> を、エントリー・ポイント・マイクロサービスが listen する際のパスに置き換えてください。 例えば、BookInfo アプリでは、パスは <code>/productpage</code> と定義されます。</td>
  </tr>
  <tr>
  <td><code>http.route.destination.host</code></td>
  <td><em>&lt;service_name&gt;</em> を、エントリー・ポイント・マイクロサービスの名前に置き換えてください。 例えば、BookInfo アプリでは、<code>productpage</code> は、他のアプリ・マイクロサービスを呼び出したエントリー・ポイント・マイクロサービスとして機能しました。</td>
  </tr>
  <tr>
  <td><code>http.route.destination.port.number</code></td>
  <td>異なるポートでマイクロサービスが listen する場合は、<em>&lt;80&gt;</em> をそのポートに置き換えてください。</td>
  </tr>
  </tbody></table>

4. Istio 管理対象マイクロサービスがデプロイされている名前空間に仮想サービス・ルールを適用します。
  ```
  kubectl apply -f my-virtual-service.yaml -n <namespace>
  ```
  {: pre}

5. `istio-ingressgateway` ロード・バランサーの **EXTERNAL-IP** アドレスを取得します。
  ```
  kubectl get svc -n istio-system
  ```
  {: pre}

  次の出力例では、**EXTERNAL-IP** は `168.1.1.1` です。
  ```
  NAME                     TYPE           CLUSTER-IP       EXTERNAL-IP                                                                    AGE
  ...
  istio-ingressgateway     LoadBalancer   172.21.XXX.XXX   169.1.1.1       80:31380/TCP,443:31390/TCP,31400:31400/TCP,5011:31323/TCP,
                                                                            8060:32483/TCP,853:32628/TCP,15030:31601/TCP,15031:31915/TCP  22m
  ```
  {: screen}

6. DNS ホスト名を作成することで、`istio-ingressgateway` ロード・バランサーの IP を登録します。
  ```
  ibmcloud ks nlb-dns-create --cluster <cluster_name_or_id> --ip <LB_IP>
  ```
  {: pre}

7. ホスト名が作成されたことを確認します。
  ```
  ibmcloud ks nlb-dnss --cluster <cluster_name_or_id>
  ```
  {: pre}

  出力例:
  ```
  Hostname                                                                                IP(s)              Health Monitor   SSL Cert Status           SSL Cert Secret Name
  mycluster-a1b2cdef345678g9hi012j3kl4567890-0001.us-south.containers.appdomain.cloud     ["168.1.1.1"]      None             created                   <certificate>
  ```
  {: screen}

7. Web ブラウザーで、アクセスするアプリ・マイクロサービスの URL を入力して、Istio 管理対象マイクロサービスにトラフィックがルーティングされていることを確認します。
  ```
  http://<host_name>/<service_path>
  ```
  {: codeblock}

ここまでの手順で、`my-gateway` という名前のゲートウェイを作成しました。 このゲートウェイは既存の `istio-ingressgateway` ロード・バランサー・サービスを使用してアプリを公開します。 `istio-ingressgateway` ロード・バランサーは、`my-virtual-service` 仮想サービスで定義されたルールを使用して、アプリへのトラフィックをルーティングします。 最後に、`istio-ingressgateway` ロード・バランサーのホスト名を作成しました。 このホスト名宛てのすべてのユーザー要求は、定義した Istio ルーティング・ルールに従って、アプリに転送されます。 {{site.data.keyword.containerlong_notm}} での DNS ホスト名の登録について、およびホスト名のカスタム・ヘルス・チェックのセットアップについては、[NLB ホスト名の登録](/docs/containers?topic=containers-loadbalancer#loadbalancer_hostname)を参照してください。

よりきめ細かくルーティングを制御することもできます。 ロード・バランサーによって各マイクロサービスにトラフィックがルーティングされた後に適用されるルールを作成するには (同一マイクロサービスの複数のバージョンにトラフィックを送信するためのルールなど)、[`DestinationRules` ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://istio.io/docs/reference/config/networking/v1alpha3/destination-rule/) を作成して適用します。
{: tip}

<br />


## {{site.data.keyword.containerlong_notm}} での Istio の更新
{: #istio_update}

管理対象 Istio アドオン内の Istio バージョンは、{{site.data.keyword.Bluemix_notm}} によってテストされており、{{site.data.keyword.containerlong_notm}} で使用することが承認されています。 Istio コンポーネントを {{site.data.keyword.containerlong_notm}} でサポートされている最新バージョンの Istio に更新するには、[管理対象アドオンの更新](/docs/containers?topic=containers-managed-addons#updating-managed-add-ons)を参照してください。
{: shortdesc}

## {{site.data.keyword.containerlong_notm}} での Istio のアンインストール
{: #istio_uninstall}

Istio に関する作業を終えたら、Istio アドオンをアンインストールすることによって、クラスター内の Istio リソースをクリーンアップできます。
{:shortdesc}

`istio` アドオンには、`istio-extras` アドオン、`istio-sample-bookinfo` アドオン、および [`knative`](/docs/containers?topic=containers-serverless-apps-knative) アドオンが依存しています。 `istio-extras` アドオンには `istio-sample-bookinfo` アドオンが依存しています。
{: important}

**オプション**: `istio-system` 名前空間内で作成または変更したすべてのリソースと、カスタム・リソース定義 (CRD) によって自動的に生成されたすべての Kubernetes リソースは削除されます。 これらのリソースを保持するには、これらを保存してから、`istio` アドオンをアンインストールしてください。
1. `istio-system` 名前空間内で作成または変更したすべてのリソース (サービスやアプリの構成ファイルなど) を保存します。
   コマンド例:
   ```
   kubectl get pod <pod_name> -o yaml -n istio-system
   ```
   {: pre}

2. `istio-system` 内の CRD によって自動的に生成された Kubernetes リソースを、ローカル・マシン上の YAML ファイルに保存します。
   1. `istio-system` 内の CRD を取得します。
      ```
      kubectl get crd -n istio-system
      ```
      {: pre}

   2. これらの CRD から作成されたすべてのリソースを保存します。

### CLI での管理対象 Istio アドオンのアンインストール
{: #istio_uninstall_cli}

1. `istio-sample-bookinfo` アドオンを無効にします。
  ```
  ibmcloud ks cluster-addon-disable istio-sample-bookinfo --cluster <cluster_name_or_ID>
  ```
  {: pre}

2. `istio-extras` アドオンを無効にします。
  ```
  ibmcloud ks cluster-addon-disable istio-extras --cluster <cluster_name_or_ID>
  ```
  {: pre}

3. `istio` アドオンを無効にします。
  ```
  ibmcloud ks cluster-addon-disable istio --cluster <cluster_name_or_ID> -f
  ```
  {: pre}

4. このクラスター内のすべての管理対象 Istio アドオンが無効になっていることを確認します。 どのような Istio アドオンも出力で返されません。
  ```
  ibmcloud ks cluster-addons --cluster <cluster_name_or_ID>
  ```
  {: pre}

### UI での管理対象 Istio アドオンのアンインストール
{: #istio_uninstall_ui}

1. [クラスター・ダッシュボード ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/kubernetes/clusters) で、クラスターの名前をクリックします。

2. **「アドオン」**タブをクリックします。

3. Istio カード上でメニュー・アイコンをクリックします。

4. 個々またはすべての Istio アドオンをアンインストールします。
  - 個々の Istio アドオン:
    1. **「管理」**をクリックします。
    2. 無効にするアドオンのチェック・ボックスをクリアします。 アドオンをクリアすると、そのアドオンを依存関係として必要とする他のアドオンも自動的にクリアされる場合があります。
    3. **「管理」**をクリックします。 Istio アドオンが無効になり、それらのアドオンのリソースがこのクラスターから削除されます。
  - すべての Istio アドオン:
    1. **「アンインストール」**をクリックします。 このクラスター内の管理対象 Istio アドオンがすべて無効になり、このクラスター内の Istio リソースがすべて削除されます。

5. Istio カード上で、アンインストールしたアドオンがリストされなくなったことを確認します。

<br />


### クラスター内にインストールされている他の Istio のアンインストール
{: #istio_uninstall_other}

IBM Helm チャートを使用して、または別の方法で、クラスター内に Istio が既にインストールされている場合は、クラスター内の管理対象 Istio アドオンを有効にする前に、その Istio インストール済み環境をクリーンアップしてください。 クラスター内に Istio が既に存在しているかどうかを確認するには、`kubectl get namespaces` を実行し、出力で `istio-system` 名前空間を探します。
{: shortdesc}

- {{site.data.keyword.Bluemix_notm}} Istio Helm チャートを使用して Istio をインストールした場合は、以下のようにします。
  1. Istio Helm デプロイメントをアンインストールします。
    ```
    helm del istio --purge
    ```
    {: pre}

  2. Helm 2.9 以前を使用した場合は、さらにジョブ・リソースを削除します。
    ```
    kubectl -n istio-system delete job --all
    ```
    {: pre}

- 手動で Istio をインストールした場合、または Istio コミュニティーの Helm チャートを使用した場合は、[Istio のアンインストールに関する資料 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://istio.io/docs/setup/kubernetes/quick-start/#uninstall-istio-core-components) を参照してください。
* 以前にクラスター内に BookInfo をインストールした場合は、それらのリソースをクリーンアップします。
  1. ディレクトリーを Istio ファイルの場所に切り替えます。
    ```
    cd <filepath>/istio-1.1.5
    ```
    {: pre}

  2. クラスター内の BookInfo サービス、ポッド、デプロイメントをすべて削除します。
    ```
    samples/bookinfo/platform/kube/cleanup.sh
    ```
    {: pre}

<br />


## 次の作業
{: #istio_next}

* Istio についてさらに詳しく知るには、[Istio の資料 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://istio.io/) でその他のガイドを参照できます。
* [Cognitive Class: Getting started with Microservices with Istio and IBM Cloud Kubernetes Service ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cognitiveclass.ai/courses/get-started-with-microservices-istio-and-ibm-cloud-container-service/) を受講します。 **注**: このコースの Istio のインストールのセクションはスキップできます。
* [Vistio ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://itnext.io/vistio-visualize-your-istio-mesh-using-netflixs-vizceral-b075c402e18e) の使用に関するこのブログ投稿を確認して、お客様の Istio サービス・メッシュを視覚化します。
