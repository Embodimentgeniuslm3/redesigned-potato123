---

copyright:
  years: 2014, 2019
lastupdated: "2019-05-31"

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


# Ingress のロギングとモニタリング
{: #ingress_health}

ロギングをカスタマイズし、モニタリングをセットアップすると、問題のトラブルシューティングや、Ingress 構成のパフォーマンスを向上させるのに役立ちます。
{: shortdesc}

## Ingress ログの表示
{: #ingress_logs}

Ingress をトラブルシューティングしたり Ingress アクティビティーをモニターしたりするには、Ingress のログを確認します。
{: shortdesc}

Ingress ALB のログは自動的に収集されます。 ALB ログを表示するには、次の 2 つの方法から選択します。
* クラスターで [Ingress サービスのロギング構成を作成します](/docs/containers?topic=containers-health#configuring)。
* CLI からログを確認します。 **注**: 少なくとも、`kube-system` 名前空間に対する [**リーダー**の {{site.data.keyword.Bluemix_notm}} IAM サービス役割](/docs/containers?topic=containers-users#platform)が必要です。
    1. ALB のポッドの ID を取得します。
        ```
        kubectl get pods -n kube-system | grep alb
        ```
        {: pre}

    2. その ALB ポッドのログを開きます。
        ```
        kubectl logs <ALB_pod_ID> nginx-ingress -n kube-system
        ```
        {: pre}

</br>デフォルトの Ingress ログの内容は JSON 形式で、クライアントとアプリの間の接続セッションを記述する共通フィールドが表示されます。 デフォルト・フィールドのサンプル・ログは、以下のようになります。

```
{"time_date": "2018-08-21T17:33:19+00:00", "client": "108.162.248.42", "host": "albhealth.multizone.us-south.containers.appdomain.cloud", "scheme": "http", "request_method": "GET", "request_uri": "/", "request_id": "c2bcce90cf2a3853694da9f52f5b72e6", "status": 200, "upstream_addr": "192.168.1.1:80", "upstream_status": 200, "request_time": 0.005, "upstream_response_time": 0.005, "upstream_connect_time": 0.000, "upstream_header_time": 0.005}
```
{: screen}

<table>
<caption>デフォルトの Ingress ログ・フォーマットのフィールドについて</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> デフォルトの Ingress ログ・フォーマットのフィールドについて</th>
</thead>
<tbody>
<tr>
<td><code>"time_date": "$time_iso8601"</code></td>
<td>ログが書き込まれた現地時間 (ISO 8601 標準形式)。</td>
</tr>
<tr>
<td><code>"client": "$remote_addr"</code></td>
<td>クライアントがアプリに送信した要求パッケージの IP アドレス。 この IP は、以下の状況に基づいて変更されることがあります。<ul><li>アプリへのクライアント要求がクラスターに送信されると、その要求は、ALB を公開しているロード・バランサー・サービス・ポッドに転送されます。 ロード・バランサー・サービス・ポッドと同じワーカー・ノードにアプリ・ポッドが存在しない場合、ロード・バランサーは異なるワーカー・ノード上のアプリ・ポッドに要求を転送します。 要求パッケージのソース IP アドレスが、アプリ・ポッドが実行されているワーカー・ノードのパブリック IP アドレスに変更されます。</li><li>[ソース IP の保持が有効な場合](/docs/containers?topic=containers-ingress#preserve_source_ip)、アプリへのクライアント要求の元の IP アドレスが代わりに記録されます。</li></ul></td>
</tr>
<tr>
<td><code>"host": "$http_host"</code></td>
<td>アプリへのアクセスが可能なホストまたはサブドメイン。 このサブドメインは、ALB の Ingress リソース・ファイルで構成されています。</td>
</tr>
<tr>
<td><code>"scheme": "$scheme"</code></td>
<td>要求の種類 (<code>HTTP</code> または <code>HTTPS</code>)。</td>
</tr>
<tr>
<td><code>"request_method": "$request_method"</code></td>
<td><code>GET</code> や <code>POST</code> など、バックエンド・アプリへの要求呼び出しのメソッド。</td>
</tr>
<tr>
<td><code>"request_uri": "$uri"</code></td>
<td>アプリ・パスへの元の要求 URI。 ALB はアプリが listen するパスを接頭部として処理します。 ALB がクライアントからアプリへの要求を受け取ると、ALB は、要求 URI のパスに一致するパス (接頭部として指定) があるか Ingress リソースを検査します。</td>
</tr>
<tr>
<td><code>"request_id": "$request_id"</code></td>
<td>16 ランダム・バイトから生成された一意の要求 ID。</td>
</tr>
<tr>
<td><code>"status": $status</code></td>
<td>接続セッションの状況コード。<ul>
<li><code>200</code>: セッションが正常に完了しました</li>
<li><code>400</code>: クライアント・データを解析できません</li>
<li><code>403</code>: アクセスが禁止されています (特定のクライアント IP アドレスについてはアクセスが制限されている場合など)</li>
<li><code>500</code>: 内部サーバー・エラー</li>
<li><code>502</code>: 不正なゲートウェイ (アップストリーム・サーバーを選択できないか、または到達できない場合など)</li>
<li><code>503</code>: サービスは使用不可です (接続数によってアクセスが制限されている場合など)</li>
</ul></td>
</tr>
<tr>
<td><code>"upstream_addr": "$upstream_addr"</code></td>
<td>アップストリーム・サーバーの IP アドレスおよびポート、または UNIX ドメイン・ソケットへのパス。 要求の処理時に複数のサーバーが接続される場合は、アドレスがコンマで区切られます (<code>"192.168.1.1:80, 192.168.1.2:80, unix:/tmp/sock"</code>)。 要求が内部的にサーバー・グループ間でリダイレクトされる場合、さまざまなグループからのサーバー・アドレスがコロンで区切られます (<code>"192.168.1.1:80, 192.168.1.2:80, unix:/tmp/sock : 192.168.10.1:80, 192.168.10.2:80"</code>)。 ALB がサーバーを選択できない場合は、代わりにサーバー・グループの名前がログに記録されます。</td>
</tr>
<tr>
<td><code>"upstream_status": $upstream_status</code></td>
<td>バックエンド・アプリのアップストリーム・サーバーから取得された応答の状況コード (標準 HTTP 応答コードなど)。 いくつかの応答の状況コードは、<code>$upstream_addr</code> 変数のアドレスのようにコンマとコロンで区切られます。 ALB がサーバーを選択できない場合は、502 (不正なゲートウェイ) 状況コードがログに記録されます。</td>
</tr>
<tr>
<td><code>"request_time": $request_time</code></td>
<td>要求処理時間 (ミリ秒分解能の秒単位で測定)。 この時間は、ALB がクライアント要求の最初のバイトを読み取ったときに開始され、ALB が応答の最後のバイトをクライアントに送信するときに停止されます。 ログは、要求処理時間が停止した直後に書き込まれます。</td>
</tr>
<tr>
<td><code>"upstream_response_time": $upstream_response_time</code></td>
<td>ALB がバックエンド・アプリのアップストリーム・サーバーから応答を受信するのにかかる時間 (ミリ秒分解能の秒単位で測定)。 いくつかの応答の時間は、<code>$upstream_addr</code> 変数のアドレスのようにコンマとコロンで区切られます。</td>
</tr>
<tr>
<td><code>"upstream_connect_time": $upstream_connect_time</code></td>
<td>ALB がバックエンド・アプリのアップストリーム・サーバーとの接続を確立するのにかかる時間 (ミリ秒分解能の秒単位で測定)。 Ingress リソース構成で TLS/SSL が有効になっている場合、この時間にはハンドシェークに費やされた時間が含まれます。 いくつかの接続の時間は、<code>$upstream_addr</code> 変数のアドレスのようにコンマとコロンで区切られます。</td>
</tr>
<tr>
<td><code>"upstream_header_time": $upstream_header_time</code></td>
<td>ALB がバックエンド・アプリのアップストリーム・サーバーから応答ヘッダーを受信するのにかかる時間 (ミリ秒分解能の秒単位で測定)。 いくつかの接続の時間は、<code>$upstream_addr</code> 変数のアドレスのようにコンマとコロンで区切られます。</td>
</tr>
</tbody></table>

## Ingress ログの内容とフォーマットをカスタマイズする
{: #ingress_log_format}

Ingress ALB に関して収集されるログの内容とフォーマットをカスタマイズすることができます。
{:shortdesc}

デフォルトでは、Ingress ログは JSON 形式にフォーマットされ、一般的なログ・フィールドが示されます。 ただし、ログのどの構成要素を転送するか、各構成要素をログ出力中にどう配置するかを選択して、カスタム・ログ形式を作成することも可能です。

始める前に、`kube-system` 名前空間に対する[**ライター**または**管理者**の {{site.data.keyword.Bluemix_notm}} IAM サービス役割](/docs/containers?topic=containers-users#platform)があることを確認してください。

1. `ibm-cloud-provider-ingress-cm` 構成マップ・リソースに対応する構成ファイルを編集します。

    ```
    kubectl edit cm ibm-cloud-provider-ingress-cm -n kube-system
    ```
    {: pre}

2. <code>data</code> セクションを追加し、オプションで `log-format` フィールドを追加します。`log-format-escape-json` フィールドを追加することもできます。

    ```
    apiVersion: v1
    data:
      log-format: '{<key1>: <log_variable1>, <key2>: <log_variable2>, <key3>: <log_variable3>}'
      log-format-escape-json: "true"
    kind: ConfigMap
    metadata:
      name: ibm-cloud-provider-ingress-cm
      namespace: kube-system
    ```
    {: codeblock}

    <table>
    <caption>YAML ファイルの構成要素</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> log-format の構成について</th>
    </thead>
    <tbody>
    <tr>
    <td><code>log-format</code></td>
    <td><code>&lt;key&gt;</code> は、ログの構成要素の名前に置き換えます。<code>&lt;log_variable&gt;</code> は、ログ項目で収集するログの構成要素の変数に置き換えます。 ログ項目に含めるテキストや句読点も指定できます。ストリング値を囲む引用符やログの構成要素を区切るコンマなどです。 例えば、構成要素の形式を <code>request: "$request"</code> にすると、「<code>request: "GET / HTTP/1.1"</code>」というログ項目が生成されます。 使用できるすべての変数のリストについては、<a href="http://nginx.org/en/docs/varindex.html">NGINX 変数の索引</a>を参照してください。<br><br>追加のヘッダー (<em>x-custom-ID</em> など) をログに記録するには、カスタム・ログ・コンテンツに以下のキー/値のペアを追加します。 <br><pre class="codeblock"><code>customID: $http_x_custom_id</code></pre> <br>ハイフン (<code>-</code>) は下線 (<code>_</code>) に変換されます。カスタム・ヘッダー名の先頭に <code>$http_</code> を付ける必要があります。</td>
    </tr>
    <tr>
    <td><code>log-format-escape-json</code></td>
    <td>オプション: デフォルトではログはテキスト形式で生成されます。 JSON 形式でログを生成するには、<code>log-format-escape-json</code> フィールドを追加して、<code>true</code> の値を使用します。</td>
    </tr>
    </tbody></table>

    例えば、以下の変数を含むログ形式を使用するとします。
    ```
    apiVersion: v1
    data:
      log-format: '{remote_address: $remote_addr, remote_user: "$remote_user",
                    time_date: [$time_local], request: "$request",
                    status: $status, http_referer: "$http_referer",
                    http_user_agent: "$http_user_agent",
                    request_id: $request_id}'
    kind: ConfigMap
    metadata:
      name: ibm-cloud-provider-ingress-cm
      namespace: kube-system
    ```
    {: screen}

    この形式に基づくログ項目は以下の例のようになります。
    ```
    remote_address: 127.0.0.1, remote_user: "dbmanager", time_date: [30/Mar/2018:18:52:17 +0000], request: "GET / HTTP/1.1", status: 401, http_referer: "-", http_user_agent: "Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:47.0) Gecko/20100101 Firefox/47.0", request_id: a02b2dea9cf06344a25611c1d7ad72db
    ```
    {: screen}

    ALB ログのデフォルト形式に基づいてカスタム・ログ形式を作成するには、以下のセクションを適宜変更して構成マップに追加してください。
    ```
    apiVersion: v1
    data:
      log-format: '{"time_date": "$time_iso8601", "client": "$remote_addr",
                    "host": "$http_host", "scheme": "$scheme",
                    "request_method": "$request_method", "request_uri": "$uri",
                    "request_id": "$request_id", "status": $status,
                    "upstream_addr": "$upstream_addr", "upstream_status":
                    $upstream_status, "request_time": $request_time,
                    "upstream_response_time": $upstream_response_time,
                    "upstream_connect_time": $upstream_connect_time,
                    "upstream_header_time": $upstream_header_time}'
      log-format-escape-json: "true"
    kind: ConfigMap
    metadata:
      name: ibm-cloud-provider-ingress-cm
      namespace: kube-system
    ```
    {: codeblock}

4. 構成ファイルを保存します。

5. 構成マップの変更が適用されたことを確認します。

   ```
   kubectl get cm ibm-cloud-provider-ingress-cm -n kube-system -o yaml
   ```
   {: pre}

4. Ingress ALB ログを表示するには、次の 2 つの方法から選択します。
    * クラスターで [Ingress サービスのロギング構成を作成します](/docs/containers?topic=containers-health#logging)。
    * CLI からログを確認します。
        1. ALB のポッドの ID を取得します。
            ```
            kubectl get pods -n kube-system | grep alb
            ```
            {: pre}

        2. その ALB ポッドのログを開きます。 ログが更新されたフォーマットに従っていることを確認します。
            ```
            kubectl logs <ALB_pod_ID> nginx-ingress -n kube-system
            ```
            {: pre}

<br />


## Ingress ALB のモニタリング
{: #ingress_monitoring}

メトリック・エクスポーターと Prometheus エージェントをクラスターにデプロイすることによって、ALB をモニターできます。
{: shortdesc}

ALB メトリック・エクスポーターは、NGINX ディレクティブ `vhost_traffic_status_zone` を使用して、各 Ingress ALB ポッドの `/status/format/json` エンドポイントからメトリック・データを収集します。 このメトリック・エクスポーターは、JSON ファイルの各データ・フィールドの形式を、Prometheus で読み取り可能なメトリックに自動的に変換します。 その後、エクスポーターによって生成されたメトリックを Prometheus エージェントが取り込んで、Prometheus ダッシュボードに表示します。

### メトリック・エクスポーターの Helm チャートのインストール
{: #metrics-exporter}

メトリック・エクスポーターの Helm チャートをインストールして、クラスター内の ALB をモニターします。
{: shortdesc}

ALB メトリック・エクスポーターのポッドは、ALB がデプロイされた同じワーカー・ノードにデプロイされる必要があります。 エッジ・ワーカー・ノードで ALB を実行していて、そのエッジ・ノードに他のワークロードのデプロイを回避するテイントが適用されていると、メトリック・エクスポーターのポッドをスケジュールできません。 `kubectl taint node <node_name> dedicated:NoSchedule- dedicated:NoExecute-` を実行して、そのテイントを解除する必要があります。
{: note}

1.  **重要**: [こちらの指示に従って](/docs/containers?topic=containers-helm#public_helm_install)、Helm クライアントをローカル・マシンにインストールして、サービス・アカウントを使用して Helm サーバー (tiller) をインストールして、{{site.data.keyword.Bluemix_notm}} Helm リポジトリーを追加します。

2. Helm チャート `ibmcloud-alb-metrics-exporter` をクラスターにインストールします。 この Helm チャートによって、ALB メトリック・エクスポーターがデプロイされて、`alb-metrics-service-account` サービス・アカウントが `kube-system` 名前空間内に作成されます。 <alb-ID> は、メトリックを収集する ALB の ID に置き換えてください。 クラスター内の ALB の ID を表示するには、<code>ibmcloud ks albs --cluster &lt;cluster_name&gt;</code> を実行します。
  モニターする ALB ごとに 1 つのチャートをデプロイする必要があります。
  {: note}
  ```
  helm install iks-charts/ibmcloud-alb-metrics-exporter --name ibmcloud-alb-metrics-exporter --set metricsNameSpace=kube-system --set albId=<alb-ID>
  ```
  {: pre}

3. Chart のデプロイメント状況を確認します。 Chart の準備ができている場合は、出力の先頭付近の **STATUS** フィールドに `DEPLOYED` の値があります。
  ```
  helm status ibmcloud-alb-metrics-exporter
  ```
  {: pre}

4. `ibmcloud-alb-metrics-exporter` ポッドが実行されていることを確認します。
  ```
  kubectl get pods -n kube-system -o wide
  ```
  {:pre}

  出力例:
  ```
  NAME                                             READY     STATUS      RESTARTS   AGE       IP               NODE
  ...
  alb-metrics-exporter-868fddf777-d49l5            1/1       Running     0          19s       172.30.xxx.xxx   10.xxx.xx.xxx
  alb-metrics-exporter-868fddf777-pf7x5            1/1       Running     0          19s       172.30.xxx.xxx   10.xxx.xx.xxx
  ```
  {:screen}

5. オプション: [Prometheus エージェントをインストール](#prometheus-agent)することで、エクスポーターによって作成されたメトリックを取り込んで、これらのメトリックを Prometheus ダッシュボードに表示します。

### Prometheus エージェントの Helm チャートのインストール
{: #prometheus-agent}

[メトリック・エクスポーター](#metrics-exporter)をインストールした後に、Prometheus エージェントの Helm チャートをインストールすることで、エクスポーターによって作成されたメトリックを取り込んで、これらのメトリックを Prometheus ダッシュボードに表示できます。
{: shortdesc}

1. メトリック・エクスポーターの Helm チャートの TAR ファイルを https://icr.io/helm/iks-charts/charts/ibmcloud-alb-metrics-exporter-1.0.7.tgz からダウンロードします。

2. Prometheus サブフォルダーにナビゲートします。
  ```
  cd ibmcloud-alb-metrics-exporter-1.0.7.tar/ibmcloud-alb-metrics-exporter/subcharts/prometheus
  ```
  {: pre}

3. Prometheus の Helm チャートをクラスターにインストールします。 <ingress_subdomain> は、クラスターの Ingress サブドメインに置き換えてください。 Prometheus ダッシュボードの URL は、デフォルトの Prometheus サブドメイン `prom-dash` と Ingress サブドメインを組み合わせたものです (例: `prom-dash.mycluster-12345.us-south.containers.appdomain.cloud`)。 クラスターの Ingress サブドメインを確認するには、<code>ibmcloud ks cluster-get --cluster &lt;cluster_name&gt;</code> を実行します。
  ```
  helm install --name prometheus . --set nameSpace=kube-system --set hostName=prom-dash.<ingress_subdomain>
  ```
  {: pre}

4. Chart のデプロイメント状況を確認します。 Chart の準備ができている場合は、出力の先頭付近の **STATUS** フィールドに `DEPLOYED` の値があります。
    ```
    helm status prometheus
    ```
    {: pre}

5. `prometheus` ポッドが実行されていることを確認します。
    ```
    kubectl get pods -n kube-system -o wide
    ```
    {:pre}

    出力例:
    ```
    NAME                                             READY     STATUS      RESTARTS   AGE       IP               NODE
    alb-metrics-exporter-868fddf777-d49l5            1/1       Running     0          19s       172.30.xxx.xxx   10.xxx.xx.xxx
    alb-metrics-exporter-868fddf777-pf7x5            1/1       Running     0          19s       172.30.xxx.xxx   10.xxx.xx.xxx
    prometheus-9fbcc8bc7-2wvbk                       1/1       Running     0          1m        172.30.xxx.xxx   10.xxx.xx.xxx
    ```
    {:screen}

6. ブラウザーに Prometheus ダッシュボードの URL を入力します。 このホスト名は `prom-dash.mycluster-12345.us-south.containers.appdomain.cloud` という形式です。 Prometheus ダッシュボードに対象の ALB が表示されます。

7. ダッシュボードに表示される [ALB](#alb_metrics)、[サーバー](#server_metrics)、[アップストリーム](#upstream_metrics)のメトリックの詳細を確認します。

### ALB メトリック
{: #alb_metrics}

`alb-metrics-exporter` は、JSON ファイルの各データ・フィールドの形式を、Prometheus で読み取り可能なメトリックに自動的に変換します。 ALB メトリックは、ALB が処理している接続や応答のデータを収集したものです。
{: shortdesc}

ALB メトリックの形式は `kube_system_<ALB-ID>_<METRIC-NAME> <VALUE>` です。 例えば、ALB が 2xx レベルの状況コードの応答を 23 個受信すると、メトリックの形式は `kube_system_public_crf02710f54fcc40889c301bfd6d5b77fe_alb1_totalHandledRequest {.. metric="2xx"} 23` のようになります (`metric` は Prometheus のラベルです)。

以下の表に、サポートされている ALB メトリック名とメトリック・ラベルを、`<ALB_metric_name>_<metric_label>` という形式で示します。
<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> サポートされている ALB メトリック</th>
</thead>
<tbody>
<tr>
<td><code>connections_reading</code></td>
<td>読み取り中のクライアント接続の総数。</td>
</tr>
<tr>
<td><code>connections_accepted</code></td>
<td>受け入れられたクライアント接続の総数。</td>
</tr>
<tr>
<td><code>connections_active</code></td>
<td>アクティブなクライアント接続の総数。</td>
</tr>
<tr>
<td><code>connections_handled</code></td>
<td>処理されたクライアント接続の総数。</td>
</tr>
<tr>
<td><code>connections_requests</code></td>
<td>要求されたクライアント接続の総数。</td>
</tr>
<tr>
<td><code>connections_waiting</code></td>
<td>待機中のクライアント接続の総数。</td>
</tr>
<tr>
<td><code>connections_writing</code></td>
<td>書き込み中のクライアント接続の総数。</td>
</tr>
<tr>
<td><code>totalHandledRequest_1xx</code></td>
<td>状況コード 1xx の応答の数。</td>
</tr>
<tr>
<td><code>totalHandledRequest_2xx</code></td>
<td>状況コード 2xx の応答の数。</td>
</tr>
<tr>
<td><code>totalHandledRequest_3xx</code></td>
<td>状況コード 3xx の応答の数。</td>
</tr>
<tr>
<td><code>totalHandledRequest_4xx</code></td>
<td>状況コード 4xx の応答の数。</td>
</tr>
<tr>
<td><code>totalHandledRequest_5xx</code></td>
<td>状況コード 5xx の応答の数。</td>
</tr>
<tr>
<td><code>totalHandledRequest_total</code></td>
<td>クライアントから受信されたクライアント要求の総数。</td>
  </tr></tbody>
</table>

### サーバー・メトリック
{: #server_metrics}

`alb-metrics-exporter` は、JSON ファイルの各データ・フィールドの形式を、Prometheus で読み取り可能なメトリックに自動的に変換します。 サーバー・メトリックは、Ingress リソースに定義されているサブドメイン (`dev.demostg1.stg.us.south.containers.appdomain.cloud` など) のデータを収集したものです。
{: shortdesc}

サーバー・メトリックの形式は `kube_system_server_<ALB-ID>_<SUB-TYPE>_<SERVER-NAME>_<METRIC-NAME> <VALUE>` です。

`<SERVER-NAME>_<METRIC-NAME>` はラベルとして形式設定されます。 例えば、`albId="dev_demostg1_us-south_containers_appdomain_cloud",metric="out"` のようになります。

例えば、サーバーからクライアントに合計 22319 バイトが送信された場合は、メトリックが以下のように形式設定されます。
```
kube_system_server_public_cra6a6eb9e897e41c4a5e58f957b417aec_alb1_bytes{albId="dev_demostg1_us-south_containers_appdomain_cloud",app="alb-metrics-exporter",instance="172.30.140.68:9913",job="kubernetes-pods",kubernetes_namespace="kube-system",kubernetes_pod_name="alb-metrics-exporter-7d495d785c-8wfw4",metric="out",pod_template_hash="3805183417"} 22319
```
{: screen}

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> サーバー・メトリックの形式について</th>
</thead>
<tbody>
<tr>
<td><code>&lt;ALB-ID&gt;</code></td>
<td>ALB ID。 上の例では、ALB ID は <code>public\_cra6a6eb9e897e41c4a5e58f957b417aec\_alb1</code> です。</td>
</tr>
<tr>
<td><code>&lt;SUB-TYPE&gt;</code></td>
<td>メトリックのサブタイプ。 各サブタイプは 1 つ以上のメトリック名に対応します。
<ul>
<li><code>bytes</code> と <code>processing\_time</code> は、メトリック <code>in</code> と <code>out</code> に対応します。</li>
<li><code>cache</code> は、メトリック <code>bypass</code>、<code>expired</code>、<code>hit</code>、<code>miss</code>、<code>revalidated</code>、<code>scare</code>、<code>stale</code>、<code>updating</code> に対応します。</li>
<li><code>requests</code> は、メトリック <code>requestMsec</code>、<code>1xx</code>、<code>2xx</code>、<code>3xx</code>、<code>4xx</code>、<code>5xx</code>、<code>total</code> に対応します。</li></ul>
上の例では、サブタイプは <code>bytes</code> です。</td>
</tr>
<tr>
<td><code>&lt;SERVER-NAME&gt;</code></td>
<td>Ingress リソースに定義されているサーバーの名前。 Prometheus との互換性を維持するために、ピリオド (<code>.</code>) を下線 <code>(\_)</code> に置き換えます。 上の例では、サーバー名は <code>dev_demostg1_stg_us_south_containers_appdomain_cloud</code> です。</td>
</tr>
<tr>
<td><code>&lt;METRIC_NAME&gt;</code></td>
<td>収集されたメトリック・タイプの名前。 メトリック名のリストについては、下記の「サポートされているサーバー・メトリック」の表を参照してください。 上の例では、メトリック名は <code>out</code> です。</td>
</tr>
<tr>
<td><code>&lt;VALUE&gt;</code></td>
<td>収集するメトリックの値。 上の例では、値は <code>22319</code> です。</td>
</tr>
</tbody></table>

以下の表に、サポートされているサーバー・メトリック名を示します。

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> サポートされているサーバー・メトリック</th>
</thead>
<tbody>
<tr>
<td><code>in</code></td>
<td>クライアントから受信されたバイトの総数。</td>
</tr>
<tr>
<td><code>out</code></td>
<td>クライアントに送信されたバイトの総数。</td>
</tr>
<tr>
<td><code>bypass</code></td>
<td>キャッシュ可能項目がキャッシュに入るためのしきい値に達しなかったために起点サーバーからフェッチされた回数 (要求数など)。</td>
</tr>
<tr>
<td><code>expired</code></td>
<td>キャッシュ内で項目が検出されたのに有効期限が切れていたために選択されなかった回数。</td>
</tr>
<tr>
<td><code>hit</code></td>
<td>キャッシュから有効な項目が選択された回数。</td>
</tr>
<tr>
<td><code>miss</code></td>
<td>キャッシュ内で有効な項目が検出されずに、サーバーが起点サーバーからその項目をフェッチした回数。</td>
</tr>
<tr>
<td><code>revalidated</code></td>
<td>キャッシュ内で有効期限の切れた項目が再び有効になった回数。</td>
</tr>
<tr>
<td><code>scarce</code></td>
<td>乏しいメモリーを解放するために使用頻度の低い項目や優先度の低い項目がキャッシュから削除された回数。</td>
</tr>
<tr>
<td><code>stale</code></td>
<td>有効期限の切れた項目がキャッシュ内で検出されたのに、別の要求によってサーバーが起点サーバーからその項目をフェッチしたために、その項目がキャッシュで選択された回数。</td>
</tr>
<tr>
<td><code>updating</code></td>
<td>古くなったコンテンツが更新された回数。</td>
</tr>
<tr>
<td><code>requestMsec</code></td>
<td>要求処理時間の平均 (ミリ秒)。</td>
</tr>
<tr>
<td><code>1xx</code></td>
<td>状況コード 1xx の応答の数。</td>
</tr>
<tr>
<td><code>2xx</code></td>
<td>状況コード 2xx の応答の数。</td>
</tr>
<tr>
<td><code>3xx</code></td>
<td>状況コード 3xx の応答の数。</td>
</tr>
<tr>
<td><code>4xx</code></td>
<td>状況コード 4xx の応答の数。</td>
</tr>
<tr>
<td><code>5xx</code></td>
<td>状況コード 5xx の応答の数。</td>
</tr>
<tr>
<td><code>total</code></td>
<td>状況コードの付いた応答の総数。</td>
  </tr></tbody>
</table>

### アップストリーム・メトリック
{: #upstream_metrics}

`alb-metrics-exporter` は、JSON ファイルの各データ・フィールドの形式を、Prometheus で読み取り可能なメトリックに自動的に変換します。 アップストリーム・メトリックは、Ingress リソースに定義されているバックエンド・サービスのデータを収集したものです。
{: shortdesc}

アップストリーム・メトリックには 2 とおりの形式があります。
* [タイプ 1](#type_one) では、アップストリーム・サービス名が含まれます。
* [タイプ 2](#type_two) では、アップストリーム・サービス名と特定のアップストリーム・ポッドの IP アドレスが含まれます。

#### タイプ 1 のアップストリーム・メトリック
{: #type_one}

タイプ 1 のアップストリーム・メトリックの形式は、`kube_system_upstream_<ALB-ID>_<SUB-TYPE>_<UPSTREAM-NAME>_<METRIC-NAME> <VALUE>` です。
{: shortdesc}

`<UPSTREAM-NAME>_<METRIC-NAME>` はラベルとして形式設定されます。 例えば、`albId="default-cafe-ingress-dev_demostg1_us-south_containers_appdomain_cloud-coffee-svc",metric="in"` のようになります。

例えば、アップストリーム・サービスが ALB から合計 1227 バイトを受信した場合は、メトリックが以下のように形式設定されます。
```
kube_system_upstream_public_cra6a6eb9e897e41c4a5e58f957b417aec_alb1_bytes{albId="default-cafe-ingress-dev_demostg1_us-south_containers_appdomain_cloud-coffee-svc",app="alb-metrics-exporter",instance="172.30.140.68:9913",job="kubernetes-pods",kubernetes_namespace="kube-system",kubernetes_pod_name="alb-metrics-exporter-7d495d785c-8wfw4",metric="in",pod_template_hash="3805183417"} 1227
```
{: screen}

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> タイプ 1 のアップストリーム・メトリックの形式について</th>
</thead>
<tbody>
<tr>
<td><code>&lt;ALB-ID&gt;</code></td>
<td>ALB ID。 上の例では、ALB ID は <code>public\_crf02710f54fcc40889c301bfd6d5b77fe\_alb1</code> です。</td>
</tr>
<tr>
<td><code>&lt;SUB-TYPE&gt;</code></td>
<td>メトリックのサブタイプ。 サポートされている値は、<code>bytes</code>、<code>processing\_time</code>、<code>requests</code> です。 上の例では、サブタイプは <code>bytes</code> です。</td>
</tr>
<tr>
<td><code>&lt;UPSTREAM-NAME&gt;</code></td>
<td>Ingress リソースに定義されているアップストリーム・サービスの名前。 Prometheus との互換性を維持するために、ピリオド (<code>.</code>) を下線 <code>(\_)</code> に置き換えます。 上の例では、アップストリーム名は <code>default-cafe-ingress-dev_demostg1_us-south_containers_appdomain_cloud-coffee-svc</code> です。</td>
</tr>
<tr>
<td><code>&lt;METRIC_NAME&gt;</code></td>
<td>収集されたメトリック・タイプの名前。 メトリック名のリストについては、下記の「サポートされているタイプ 1 のアップストリーム・メトリック」の表を参照してください。 上の例では、メトリック名は <code>in</code> です。</td>
</tr>
<tr>
<td><code>&lt;VALUE&gt;</code></td>
<td>収集するメトリックの値。 上の例では、値は <code>1227</code> です。</td>
</tr>
</tbody></table>

以下の表に、サポートされているタイプ 1 のアップストリーム・メトリック名を示します。

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> サポートされているタイプ 1 のアップストリーム・メトリック</th>
</thead>
<tbody>
<tr>
<td><code>in</code></td>
<td>ALB サーバーから受信されたバイトの総数。</td>
</tr>
<tr>
<td><code>out</code></td>
<td>ALB サーバーに送信されたバイトの総数。</td>
</tr>
<tr>
<td><code>1xx</code></td>
<td>状況コード 1xx の応答の数。</td>
</tr>
<tr>
<td><code>2xx</code></td>
<td>状況コード 2xx の応答の数。</td>
</tr>
<tr>
<td><code>3xx</code></td>
<td>状況コード 3xx の応答の数。</td>
</tr>
<tr>
<td><code>4xx</code></td>
<td>状況コード 4xx の応答の数。</td>
</tr>
<tr>
<td><code>5xx</code></td>
<td>状況コード 5xx の応答の数。</td>
</tr>
<tr>
<td><code>total</code></td>
<td>状況コードの付いた応答の総数。</td>
  </tr></tbody>
</table>

#### タイプ 2 のアップストリーム・メトリック
{: #type_two}

タイプ 2 のアップストリーム・メトリックの形式は `kube_system_upstream_<ALB-ID>_<METRIC-NAME>_<UPSTREAM-NAME>_<POD-IP> <VALUE>` です。
{: shortdesc}

`<UPSTREAM-NAME>_<POD-IP>` はラベルとして形式設定されます。 例えば、`albId="default-cafe-ingress-dev_dev_demostg1_us-south_containers_appdomain_cloud-tea-svc",backend="172_30_75_6_80"` のようになります。

例えば、アップストリーム・サービスの要求処理時間 (アップストリームを含む) の平均が 40 ミリ秒の場合は、メトリックが以下のように形式設定されます。
```
kube_system_upstream_public_cra6a6eb9e897e41c4a5e58f957b417aec_alb1_requestMsec{albId="default-cafe-ingress-dev_dev_demostg1_us-south_containers_appdomain_cloud-tea-svc",app="alb-metrics-exporter",backend="172_30_75_6_80",instance="172.30.75.3:9913",job="kubernetes-pods",kubernetes_namespace="kube-system",kubernetes_pod_name="alb-metrics-exporter-7d495d785c-swkls",pod_template_hash="3805183417"} 40
```

{: screen}

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> タイプ 2 のアップストリーム・メトリックの形式について</th>
</thead>
<tbody>
<tr>
<td><code>&lt;ALB-ID&gt;</code></td>
<td>ALB ID。 上の例では、ALB ID は <code>public\_cra6a6eb9e897e41c4a5e58f957b417aec\_alb1</code> です。</td>
</tr>
<td><code>&lt;UPSTREAM-NAME&gt;</code></td>
<td>Ingress リソースに定義されているアップストリーム・サービスの名前。 Prometheus との互換性を維持するために、ピリオド (<code>.</code>) を下線 (<code>\_</code>) に置き換えます。 上の例では、アップストリーム名は <code>demostg1\_stg\_us\_south\_containers\_appdomain\_cloud\_tea\_svc</code> です。</td>
</tr>
<tr>
<td><code>&lt;POD\_IP&gt;</code></td>
<td>特定のアップストリーム・サービス・ポッドの IP アドレスとポート。 Prometheus との互換性を維持するために、ピリオド (<code>.</code>) とコロン (<code>:</code>) を下線 <code>(_)</code> に置き換えます。 上の例では、アップストリーム・ポッド IP は <code>172_30_75_6_80</code> です。</td>
</tr>
<tr>
<td><code>&lt;METRIC_NAME&gt;</code></td>
<td>収集されたメトリック・タイプの名前。 メトリック名のリストについては、下記の「サポートされているタイプ 2 のアップストリーム・メトリック」の表を参照してください。 上の例では、メトリック名は <code>requestMsec</code> です。</td>
</tr>
<tr>
<td><code>&lt;VALUE&gt;</code></td>
<td>収集するメトリックの値。 上の例では、値は <code>40</code> です。</td>
</tr>
</tbody></table>

以下の表に、サポートされているタイプ 2 のアップストリーム・メトリック名を示します。

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> サポートされているタイプ 2 のアップストリーム・メトリック</th>
</thead>
<tbody>
<tr>
<td><code>requestMsec</code></td>
<td>要求処理時間 (アップストリームを含む) の平均 (ミリ秒)。</td>
</tr>
<tr>
<td><code>responseMsec</code></td>
<td>アップストリームだけの応答処理時間の平均 (ミリ秒)。</td>
  </tr></tbody>
</table>

<br />


## Ingress メトリックの収集に関する共有メモリー・ゾーン・サイズの引き上げ
{: #vts_zone_size}

共有メモリー・ゾーンは、ワーカー・プロセスがキャッシュ、セッション・パーシスタンス、速度制限などの情報を共有できるように定義されています。 仮想ホスト・トラフィック状況ゾーンとも呼ばれる共有メモリー・ゾーンは、Ingress が ALB のメトリック・データを収集できるようにセットアップされています。
{:shortdesc}

`ibm-cloud-provider-ingress-cm` Ingress 構成マップの `vts-status-zone-size` フィールドで、メトリック・データ収集の共有メモリー・ゾーンのサイズを設定します。 デフォルトでは、`vts-status-zone-size` は `10m` に設定されます。 大きな環境で、メトリックを収集するためにさらに多くのメモリーが必要な場合は、以下の手順に従って、デフォルトをオーバーライドして大きい値を使用することができます。

始める前に、`kube-system` 名前空間に対する[**ライター**または**管理者**の {{site.data.keyword.Bluemix_notm}} IAM サービス役割](/docs/containers?topic=containers-users#platform)があることを確認してください。

1. `ibm-cloud-provider-ingress-cm` 構成マップ・リソースに対応する構成ファイルを編集します。

    ```
    kubectl edit cm ibm-cloud-provider-ingress-cm -n kube-system
    ```
    {: pre}

2. `vts-status-zone-size` の値を `10m` より大きい値に変更します。

   ```
   apiVersion: v1
   data:
     vts-status-zone-size: "10m"
   kind: ConfigMap
   metadata:
     name: ibm-cloud-provider-ingress-cm
     namespace: kube-system
   ```
   {: codeblock}

3. 構成ファイルを保存します。

4. 構成マップの変更が適用されたことを確認します。

   ```
   kubectl get cm ibm-cloud-provider-ingress-cm -n kube-system -o yaml
   ```
   {: pre}
