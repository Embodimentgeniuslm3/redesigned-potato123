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
{:download: .download}
{:preview: .preview}
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}


# ロギングとモニタリングのトラブルシューティング
{: #cs_troubleshoot_health}

{{site.data.keyword.containerlong}} を使用する際は、ここに示すロギングとモニタリングの問題のトラブルシューティング手法を検討してください。
{: shortdesc}

より一般的な問題が起きている場合は、[クラスターのデバッグ](/docs/containers?topic=containers-cs_troubleshoot)を試してください。
{: tip}

## ログが表示されない
{: #cs_no_logs}

{: tsSymptoms}
Kibana ダッシュボードにアクセスしてもログが表示されません。

{: tsResolve}
クラスター・ログが表示されない理由と、対応するトラブルシューティング手順について以下に説明します。

<table>
<caption>表示されないログのトラブルシューティング</caption>
  <col width="40%">
  <col width="60%">
  <thead>
    <tr>
      <th>現象の理由</th>
      <th>修正方法</th>
    </tr>
 </thead>
 <tbody>
  <tr>
    <td>ロギング構成がセットアップされていない。</td>
    <td>ログが送信されるようにするには、ロギング構成を作成する必要があります。 そのためには、<a href="/docs/containers?topic=containers-health#logging">クラスター・ロギングの構成</a>を参照してください。</td>
  </tr>
  <tr>
    <td>クラスターが <code>Normal</code> 状態ではない。</td>
    <td>クラスターの状態を確認する方法については、<a href="/docs/containers?topic=containers-cs_troubleshoot#debug_clusters">クラスターのデバッグ</a>を参照してください。</td>
  </tr>
  <tr>
    <td>ログ・ストレージの割り当て量に達している。</td>
    <td>ログ・ストレージの限度を増やす方法については、<a href="/docs/services/CloudLogAnalysis/troubleshooting?topic=cloudloganalysis-error_msgs">{{site.data.keyword.loganalysislong_notm}} の資料</a>を参照してください。</td>
  </tr>
  <tr>
    <td>クラスター作成の際にスペースを指定した場合、アカウント所有者に、そのスペースに対する管理者、開発者、または監査員の権限がない。</td>
      <td>アカウント所有者のアクセス許可を変更するには、以下のようにします。
      <ol><li>クラスターのアカウント所有者を見つけるために、<code>ibmcloud ks api-key-info --cluster &lt;cluster_name_or_ID&gt;</code> を実行します。</li>
      <li>そのアカウント所有者にスペースに対する管理者、開発者、または監査員の {{site.data.keyword.containerlong_notm}} アクセス許可を付与する方法については、<a href="/docs/containers?topic=containers-users">クラスター・アクセス権限の管理</a>を参照してください。</li>
      <li>許可が変更された後にロギング・トークンをリフレッシュするには、<code>ibmcloud ks logging-config-refresh --cluster &lt;cluster_name_or_ID&gt;</code> を実行します。</li></ol></td>
    </tr>
    <tr>
      <td>アプリのロギング構成でアプリのパスにシンボリック・リンクが使用されている。</td>
      <td><p>ログが送信されるようにするには、ロギング構成で絶対パスを使用する必要があります。そうしないと、ログを読み取れません。 パスがワーカー・ノードにマウントされている場合は、シンボリック・リンクが作成されている可能性があります。</p> <p>例: 指定されたパスが <code>/usr/local/<b>spark</b>/work/app-0546/0/stderr</code> であるのに、ログが <code>/usr/local/<b>spark-1.0-hadoop-1.2</b>/work/app-0546/0/stderr</code> に送信されている場合、ログは読み取れません。</p></td>
    </tr>
  </tbody>
</table>

トラブルシューティング中に行った変更をテストするために、複数のログ・イベントを生成するサンプルのポッド *Noisy* をクラスター内のワーカー・ノードにデプロイすることができます。

開始前に、以下のことを行います。 [アカウントにログインします。 該当する場合は、適切なリソース・グループをターゲットにします。 クラスターのコンテキストを設定します。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1. `deploy-noisy.yaml` 構成ファイルを作成します。
    ```
    apiVersion: v1
      kind: Pod
      metadata:
        name: noisy
      spec:
        containers:
      - name: noisy
          image: ubuntu:16.04
          command: ["/bin/sh"]
          args: ["-c", "while true; do sleep 10; echo 'Hello world!'; done"]
          imagePullPolicy: "Always"
      ```
      {: codeblock}

2. クラスターのコンテキストで構成ファイルを実行します。
    ```
    kubectl apply -f noisy.yaml
    ```
    {:pre}

3. 数分後に、Kibana ダッシュボードにログが表示されます。 Kibana ダッシュボードにアクセスするには、以下のいずれかの URL にアクセスし、クラスターを作成した {{site.data.keyword.Bluemix_notm}} アカウントを選択します。 クラスター作成の際にスペースを指定した場合は、代わりにそのスペースに移動します。
    - 米国南部および米国東部: `https://logging.ng.bluemix.net`
    - 英国南部: `https://logging.eu-gb.bluemix.net`
    - EU 中央: `https://logging.eu-fra.bluemix.net`
    - 南アジア太平洋地域: `https://logging.au-syd.bluemix.net`

<br />


## Kubernetes ダッシュボードに使用状況グラフが表示されない
{: #cs_dashboard_graphs}

{: tsSymptoms}
Kubernetes ダッシュボードにアクセスするとき、使用状況グラフが表示されません。

{: tsCauses}
クラスターの更新やワーカー・ノードのリブートの後に、`kube-dashboard` ポッドが更新されないことがあります。

{: tsResolve}
`kube-dashboard` ポッドを削除して、強制的に再始動してください。 ポッドは RBAC ポリシーを使用して再作成され、`heapster` にアクセスして使用状況情報を取得します。

  ```
  kubectl delete pod -n kube-system $(kubectl get pod -n kube-system --selector=k8s-app=kubernetes-dashboard -o jsonpath='{.items..metadata.name}')
  ```
  {: pre}

<br />


## ログ割り当て量が少なすぎる
{: #quota}

{: tsSymptoms}
クラスターのロギング構成をセットアップして、ログを {{site.data.keyword.loganalysisfull}} に転送します。 ログを表示すると、以下のようなエラー・メッセージが表示されます。

```
You have reached the daily quota that is allocated to the Bluemix space {Space GUID} for the IBM® Cloud Log Analysis instance {Instance GUID}. Your current daily allotment is XXX for Log Search storage, which is retained for a period of 3 days, during which it can be searched for in Kibana. This does not affect your log retention policy in Log Collection storage. To upgrade your plan so that you can store more data in Log Search storage per day, upgrade the Log Analysis service plan for this space. For more information about service plans and how to upgrade your plan, see Plans.
```
{: screen}

{: tsResolve}
以下に示すログ割り当て量に到達する理由、および対応するトラブルシューティング手順を確認してください。

<table>
<caption>ログ・ストレージの問題のトラブルシューティング</caption>
  <col width="40%">
  <col width="60%">
  <thead>
    <tr>
      <th>現象の理由</th>
      <th>修正方法</th>
    </tr>
 </thead>
 <tbody>
  <tr>
    <td>1 つ以上のポッドで多数のログが生成される。</td>
    <td>特定のポッドからのログの転送を防止することによって、ログ・ストレージ・スペースを解放できます。 これらのポッドに対して[ロギング・フィルター](/docs/containers?topic=containers-health#filter-logs)を作成します。</td>
  </tr>
  <tr>
    <td>ライト・プランでのログ・ストレージの毎日の割り当ての 500 MB を超過した。</td>
    <td>まず、ログ・ドメインの[検索割り当て量および日次使用量を計算](/docs/services/CloudLogAnalysis/how-to?topic=cloudloganalysis-quota)します。 次に、[{{site.data.keyword.loganalysisshort_notm}} サービス・プランをアップグレード](/docs/services/CloudLogAnalysis/how-to?topic=cloudloganalysis-change_plan#change_plan)することによって、ログ・ストレージ割り当て量を増やすことができます。</td>
  </tr>
  <tr>
    <td>現行の有料プランのログ・ストレージ割り当て量を超過した。</td>
    <td>まず、ログ・ドメインの[検索割り当て量および日次使用量を計算](/docs/services/CloudLogAnalysis/how-to?topic=cloudloganalysis-quota)します。 次に、[{{site.data.keyword.loganalysisshort_notm}} サービス・プランをアップグレード](/docs/services/CloudLogAnalysis/how-to?topic=cloudloganalysis-change_plan#change_plan)することによって、ログ・ストレージ割り当て量を増やすことができます。</td>
  </tr>
  </tbody>
</table>

<br />


## ログの行が長すぎる
{: #long_lines}

{: tsSymptoms}
クラスターのロギング構成をセットアップして、ログを {{site.data.keyword.loganalysisfull_notm}} に転送します。 ログを表示すると、長いログ・メッセージが表示されます。また、Kibana では、ログ・メッセージの最後の 600 から 700 文字のみが表示される可能性があります。

{: tsCauses}
長いログ・メッセージは、Fluentd によって収集される前に、長さのために切り捨てられる場合があるため、ログは、{{site.data.keyword.loganalysisshort_notm}} に転送される前に、Fluentd によって正しく解析されない場合があります。

{: tsResolve}
行の長さを制限するため、独自のロガーを構成して、各ログの `stack_trace` の最大長を設定できます。 例えば、ロガーに Log4j を使用している場合、[`EnhancedPatternLayout` ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](http://logging.apache.org/log4j/1.2/apidocs/org/apache/log4j/EnhancedPatternLayout.html) を使用して、`stack_trace` を 15 KB に制限できます。

## ヘルプとサポートの取得
{: #health_getting_help}

まだクラスターに問題がありますか?
{: shortdesc}

-  `ibmcloud` CLI およびプラグインの更新が使用可能になると、端末に通知が表示されます。 使用可能なすべてのコマンドおよびフラグを使用できるように、CLI を最新の状態に保つようにしてください。
-   {{site.data.keyword.Bluemix_notm}} が使用可能かどうかを確認するために、[{{site.data.keyword.Bluemix_notm}} 状況ページ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン") を確認します](https://cloud.ibm.com/status?selected=status)。
-   [{{site.data.keyword.containerlong_notm}} Slack ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://ibm-container-service.slack.com) に質問を投稿します。
    {{site.data.keyword.Bluemix_notm}} アカウントに IBM ID を使用していない場合は、この Slack への[招待を要求](https://bxcs-slack-invite.mybluemix.net/)してください。
    {: tip}
-   フォーラムを確認して、同じ問題が他のユーザーで起こっているかどうかを調べます。 フォーラムを使用して質問するときは、{{site.data.keyword.Bluemix_notm}} 開発チームの目に止まるように、質問にタグを付けてください。
    -   {{site.data.keyword.containerlong_notm}} を使用したクラスターまたはアプリの開発やデプロイに関する技術的な質問がある場合は、[Stack Overflow![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://stackoverflow.com/questions/tagged/ibm-cloud+containers) に質問を投稿し、`ibm-cloud`、`kubernetes`、`containers` のタグを付けてください。
    -   サービスや概説の説明について質問がある場合は、[IBM Developer Answers Answers ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix) フォーラムを使用してください。 `ibm-cloud` と `containers` のタグを含めてください。
    フォーラムの使用について詳しくは、[ヘルプの取得](/docs/get-support?topic=get-support-getting-customer-support#using-avatar)を参照してください。
-   ケースを開いて、IBM サポートに連絡してください。 IBM サポート・ケースを開く方法や、サポート・レベルとケースの重大度については、[サポートへのお問い合わせ](/docs/get-support?topic=get-support-getting-customer-support)を参照してください。
問題を報告する際に、クラスター ID も報告してください。 クラスター ID を取得するには、`ibmcloud ks clusters` を実行します。 また、[{{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool](/docs/containers?topic=containers-cs_troubleshoot#debug_utility) を使用して、クラスターから関連情報を収集してエクスポートし、IBM サポートと情報を共有することができます。
{: tip}

