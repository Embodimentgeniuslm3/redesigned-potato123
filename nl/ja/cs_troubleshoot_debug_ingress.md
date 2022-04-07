---

copyright:
  years: 2014, 2019
lastupdated: "2019-05-31"

keywords: kubernetes, iks, nginx, ingress controller

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


# Ingress のデバッグ
{: #cs_troubleshoot_debug_ingress}

{{site.data.keyword.containerlong}} を使用する際は、ここに示す一般的な Ingress のトラブルシューティングとデバッグの手法を検討してください。
{: shortdesc}

クラスターでアプリ用の Ingress リソースを作成して、アプリをパブリックに公開しました。 ただし、ALB のパブリック IP アドレスまたはサブドメインを介してアプリに接続しようとすると、接続が失敗するか、タイムアウトになります。 以下のセクションのステップは、Ingress のセットアップのデバッグに役立ちます。

1 つのホストは、必ず 1 つの Ingress リソースだけに定義するようにしてください。 1 つのホストが複数の Ingress リソースに定義された場合、ALB はトラフィックを正しく転送しないことがあり、その場合エラーが発生する場合があります。
{: tip}

始めに、以下の [{{site.data.keyword.Bluemix_notm}} IAM アクセス・ポリシー](/docs/containers?topic=containers-users#platform)があることを確認してください。
  - クラスターに対する**エディター**または**管理者**のプラットフォーム役割
  - **ライター**または**管理者**のサービス役割

## ステップ 1: {{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool を使用した Ingress テストを実行する

トラブルシューティングの際に、{{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool を使用して、Ingress テストを実行し、クラスターから Ingress の関連情報を収集することができます。 このデバッグ・ツールを使用するには、[`ibmcloud-iks-debug` Helm チャート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/kubernetes/helm/iks-charts/ibmcloud-iks-debug) をインストールします。
{: shortdesc}


1. [クラスターで Helm をセットアップし、Tiller のサービス・アカウントを作成し、Helm インスタンスに `ibm` リポジトリーを追加します](/docs/containers?topic=containers-helm)。

2. Helm チャートをクラスターにインストールします。
  ```
  helm install iks-charts/ibmcloud-iks-debug --name debug-tool
  ```
  {: pre}


3. デバッグ・ツール・インターフェースを表示するためにプロキシー・サーバーを始動します。
  ```
  kubectl proxy --port 8080
  ```
  {: pre}

4. Web ブラウザーで、デバッグ・ツール・インターフェースの URL (http://localhost:8080/api/v1/namespaces/default/services/debug-tool-ibmcloud-iks-debug:8822/proxy/page) を開きます。

5. テストの**「ingress」**グループを選択します。 潜在的な警告、エラー、または問題を検査するテストもあれば、トラブルシューティング中に参照できる情報を収集するだけのテストもあります。 各テストの機能について詳しくは、テストの名前の隣にある情報アイコンをクリックしてください。

6. **「実行 (Run)」**をクリックします。

7. 各テストの結果を確認します。
  * テストが失敗する場合、問題の解決方法について詳しくは、左側の列内のテスト名の隣にある情報アイコンをクリックしてください。
  * 以下のセクションで Ingress サービスをデバッグする際に、情報を収集するだけのテストの結果も使用できます。

## ステップ 2: Ingress デプロイメントと ALB ポッドのログでエラー・メッセージを確認する
{: #errors}

まず、Ingress リソースのデプロイメント・イベントと ALB ポッドのログでエラー・メッセージを確認します。 これらのエラー・メッセージは、障害の根本原因を探して、次のセクションで Ingress のセットアップをさらにデバッグするのに役立ちます。
{: shortdesc}

1. Ingress リソースのデプロイメントを確認して、警告またはエラー・メッセージがないか探します。
    ```
    kubectl describe ingress <myingress>
    ```
    {: pre}

    出力の **Events** セクションに、Ingress リソースや使用した特定のアノテーション内の無効な値に関する警告メッセージが表示される場合があります。 [Ingress リソース構成の資料](/docs/containers?topic=containers-ingress#public_inside_4)または[アノテーションの資料](/docs/containers?topic=containers-ingress_annotation)を参照してください。

    ```
    Name:             myingress
    Namespace:        default
    Address:          169.xx.xxx.xxx,169.xx.xxx.xxx
    Default backend:  default-http-backend:80 (<none>)
    Rules:
      Host                                             Path  Backends
      ----                                             ----  --------
      mycluster.us-south.containers.appdomain.cloud
                                                       /tea      myservice1:80 (<none>)
                                                       /coffee   myservice2:80 (<none>)
    Annotations:
      custom-port:        protocol=http port=7490; protocol=https port=4431
      location-modifier:  modifier='~' serviceName=myservice1;modifier='^~' serviceName=myservice2
    Events:
      Type     Reason             Age   From                                                            Message
      ----     ------             ----  ----                                                            -------
      Normal   Success            1m    public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Successfully applied ingress resource.
      Warning  TLSSecretNotFound  1m    public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Failed to apply ingress resource.
      Normal   Success            59s   public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Successfully applied ingress resource.
      Warning  AnnotationError    40s   public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Failed to apply ingress.bluemix.net/custom-port annotation. Error annotation format error : One of the mandatory fields not valid/missing for annotation ingress.bluemix.net/custom-port
      Normal   Success            40s   public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Successfully applied ingress resource.
      Warning  AnnotationError    2s    public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Failed to apply ingress.bluemix.net/custom-port annotation. Invalid port 7490. Annotation cannot use ports 7481 - 7490
      Normal   Success            2s    public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Successfully applied ingress resource.
    ```
    {: screen}
{: #check_pods}
2. ALB ポッドの状況を確認します。
    1. クラスター内で稼働している ALB ポッドを取得します。
        ```
        kubectl get pods -n kube-system | grep alb
        ```
        {: pre}

    2. **STATUS** 列を確認して、すべてのポッドが実行されていることを確認します。

    3. ポッドが `Running` でない場合は、ALB を無効にして再度有効にすることができます。 以下のコマンドで、`<ALB_ID>` をポッドの ALB の ID に置き換えます。 例えば、稼働していないポッドの名前が `public-crb2f60e9735254ac8b20b9c1e38b649a5-alb1-5d6d86fbbc-kxj6z` の場合、ALB ID は `public-crb2f60e9735254ac8b20b9c1e38b649a5-alb1` です。
        ```
        ibmcloud ks alb-configure --albID <ALB_ID> --disable
        ```
        {: pre}

        ```
        ibmcloud ks alb-configure --albID <ALB_ID> --enable
        ```
        {: pre}

3. ALB のログを確認します。
    1.  クラスター内で稼働している ALB ポッドの ID を取得します。
        ```
        kubectl get pods -n kube-system | grep alb
        ```
        {: pre}

    3. 各 ALB ポッドの `nginx-ingress` コンテナーのログを取得します。
        ```
        kubectl logs <ingress_pod_ID> nginx-ingress -n kube-system
        ```
        {: pre}

    4. ALB ログでエラー・メッセージを確認します。

## ステップ 3: ALB サブドメインとパブリック IP アドレスに ping する
{: #ping}

Ingress サブドメインと ALB のパブリック IP アドレスの可用性を確認します。
{: shortdesc}

1. パブリック ALB が listen している IP アドレスを取得します。
    ```
    ibmcloud ks albs --cluster <cluster_name_or_ID>
    ```
    {: pre}

    `dal10` および `dal13` のワーカー・ノードを持つ複数ゾーン・クラスターの出力例:

    ```
    ALB ID                                            Enabled   Status     Type      ALB IP          Zone    Build                          ALB VLAN ID
    private-cr24a9f2caf6554648836337d240064935-alb1   false     disabled   private   -               dal13   ingress:411/ingress-auth:315   2294021
    private-cr24a9f2caf6554648836337d240064935-alb2   false     disabled   private   -               dal10   ingress:411/ingress-auth:315   2234947
    public-cr24a9f2caf6554648836337d240064935-alb1    true      enabled    public    169.62.196.238  dal13   ingress:411/ingress-auth:315   2294019
    public-cr24a9f2caf6554648836337d240064935-alb2    true      enabled    public    169.46.52.222   dal10   ingress:411/ingress-auth:315   2234945
    ```
    {: screen}

    * パブリック ALB に IP アドレスが指定されていない場合は、[Ingress ALB がゾーンにデプロイされない](/docs/containers?topic=containers-cs_troubleshoot_network#cs_subnet_limit)を参照してください。

2. ALB IP の正常性を確認します。

    * 単一ゾーン・クラスターと複数ゾーン・クラスターの場合: 各 ALB が正常にパケットを受信できるように、各パブリック ALB の IP アドレスを ping します。 プライベート ALB を使用している場合は、プライベート・ネットワークからのみ IP アドレスを ping できます。
        ```
        ping <ALB_IP>
        ```
        {: pre}

        * CLI がタイムアウトを返し、ワーカー・ノードを保護するカスタム・ファイアウォールがある場合は、[ファイアウォール](/docs/containers?topic=containers-cs_troubleshoot_clusters#cs_firewall)で ICMP を許可していることを確認します。
        * ping をブロックしているファイアウォールがなく、引き続き ping がタイムアウトになる場合は、[ALB ポッドの状況を確認](#check_pods)します。

    * 複数ゾーン・クラスターのみの場合: MZLB ヘルス・チェックを使用して、ALB IP の状況を確認できます。 MZLB について詳しくは、[複数ゾーン・ロード・バランサー (MZLB)](/docs/containers?topic=containers-ingress#planning) を参照してください。 MZLB ヘルス・チェックは、`<cluster_name>.<region_or_zone>.containers.appdomain.cloud` の形式の新しい Ingress サブドメインを持つクラスターのみに使用できます。 クラスターでまだ `<cluster_name>.<region>.containers.mybluemix.net` の古い形式を使用している場合は、[単一ゾーン・クラスターを複数ゾーンに変換](/docs/containers?topic=containers-add_workers#add_zone)します。 クラスターに新しい形式のサブドメインが割り当てられますが、古いサブドメイン形式も引き続き使用できます。 別の方法として、新しいサブドメイン形式が自動的に割り当てられた新しいクラスターを注文することもできます。

    以下の HTTP cURL コマンドは、`albhealth` ホストを使用します。このホストは、{{site.data.keyword.containerlong_notm}} によって構成され、ALB IP の `healthy` または `unhealthy` の状況を返します。
        ```
        curl -X GET http://169.62.196.238/ -H "Host: albhealth.mycluster-12345.us-south.containers.appdomain.cloud"
        ```
        {: pre}

        出力例:
        ```
        健全な
        ```
        {: screen}
        If one or more of the IPs returns `unhealthy`, [check the status of your ALB pods](#check_pods).

3. IBM 提供の Ingress サブドメインを取得します。
    ```
    ibmcloud ks cluster-get --cluster <cluster_name_or_ID> | grep Ingress
    ```
    {: pre}

    出力例:
    ```
    Ingress Subdomain:      mycluster-12345.us-south.containers.appdomain.cloud
    Ingress Secret:         <tls_secret>
    ```
    {: screen}

4. このセクションのステップ 2 で取得した各パブリック ALB の IP が、クラスターの IBM 提供の Ingress サブドメインに登録されていることを確認します。 例えば、複数ゾーン・クラスターでは、ワーカー・ノードがある各ゾーンのパブリック ALB IP は、同じホスト名で登録される必要があります。

    ```
    kubectl get ingress -o wide
    ```
    {: pre}

    出力例:
    ```
    NAME                HOSTS                                                    ADDRESS                        PORTS     AGE
    myingressresource   mycluster-12345.us-south.containers.appdomain.cloud      169.46.52.222,169.62.196.238   80        1h
    ```
    {: screen}

## ステップ 4: ドメイン・マッピングと Ingress リソース構成を確認する
{: #ts_ingress_config}

1. カスタム・ドメインを使用する場合は、DNS プロバイダーを使用してカスタム・ドメインを IBM 提供のサブドメインまたは ALB のパブリック IP アドレスにマップしていることを確認します。 IBM では IBM サブドメインに対する自動ヘルス・チェックを提供しており、障害のある IP がすべて DNS 応答から削除されるため、CNAME の使用がお勧めされることに注意してください。
    * IBM 提供のサブドメイン: 正規名レコード (CNAME) 内のクラスターの IBM 提供のサブドメインにカスタム・ドメインがマップされていることを確認します。
        ```
        host www.my-domain.com
        ```
        {: pre}

        出力例:
        ```
        www.my-domain.com is an alias for mycluster-12345.us-south.containers.appdomain.cloud
        mycluster-12345.us-south.containers.appdomain.cloud has address 169.46.52.222
        mycluster-12345.us-south.containers.appdomain.cloud has address 169.62.196.238
        ```
        {: screen}

    * パブリック IP アドレス: A レコードの ALB のポータブル・パブリック IP アドレスにカスタム・ドメインがマップされていることを確認します。 IP は、[前のセクション](#ping)のステップ 1 で取得したパブリック ALB IP と一致する必要があります。
        ```
        host www.my-domain.com
        ```
        {: pre}

        出力例:
        ```
        www.my-domain.com has address 169.46.52.222
        www.my-domain.com has address 169.62.196.238
        ```
        {: screen}

2. クラスターの Ingress リソース構成ファイルを確認します。
    ```
    kubectl get ingress -o yaml
    ```
    {: pre}

    1. 1 つのホストは、必ず 1 つの Ingress リソースだけに定義するようにしてください。 1 つのホストが複数の Ingress リソースに定義された場合、ALB はトラフィックを正しく転送しないことがあり、その場合エラーが発生する場合があります。

    2. サブドメインと TLS 証明書が正しいことを確認します。 IBM 提供の Ingress サブドメインと TLS 証明書を見つけるには、`ibmcloud ks cluster-get --cluster <cluster_name_or_ID>` を実行します。

    3.  アプリが、Ingress の **path** セクションで構成されているパスを使用して listen していることを確認します。 アプリがルート・パスで listen するようにセットアップされている場合は、`/` をパスとして使用します。 このパスへの着信トラフィックを、アプリが listen している別のパスにルーティングする必要がある場合は、[再書き込みパス](/docs/containers?topic=containers-ingress_annotation#rewrite-path)・アノテーションを使用します。

    4. 必要に応じてリソース構成 YAML を編集します。 エディターを閉じると、変更内容が保存され、自動的に適用されます。
        ```
        kubectl edit ingress <myingressresource>
        ```
        {: pre}

## デバッグのための DNS からの ALB の削除
{: #one_alb}

特定の ALB IP を介してアプリにアクセスできない場合は、その DNS 登録を無効にすることによって、一時的に ALB を実動から削除することができます。 その後、ALB の IP アドレスを使用して、その ALB に対してデバッグ・テストを実行できます。

例えば、2 つのゾーンに複数ゾーン・クラスターがあり、2 つのパブリック ALB に IP アドレス `169.46.52.222` と `169.62.196.238` が指定されているとします。 2 番目のゾーンの ALB についてはヘルス・チェックで正常と返されますが、アプリがそこから直接到達することはできません。 デバッグのために実動から ALB の IP アドレス `169.62.196.238` を削除します。 1 番目のゾーンの ALB IP `169.46.52.222` がドメインに登録され、2 番目のゾーンの ALB のデバッグ中にトラフィックのルーティングを続行します。

1. 到達不能な IP アドレスを持つ ALB の名前を取得します。
    ```
    ibmcloud ks albs --cluster <cluster_name> | grep <ALB_IP>
    ```
    {: pre}

    例えば、到達不能な IP `169.62.196.238` は、ALB `public-cr24a9f2caf6554648836337d240064935-alb1` に属しています。
    ```
    ALB ID                                            Enabled   Status     Type      ALB IP           Zone    Build                          ALB VLAN ID
    public-cr24a9f2caf6554648836337d240064935-alb1    false     disabled   private   169.62.196.238   dal13   ingress:411/ingress-auth:315   2294021
    ```
    {: screen}

2. 前のステップの ALB 名を使用して、ALB ポッドの名前を取得します。 以下のコマンドでは、前のステップの ALB 名の例を使用しています。
    ```
    kubectl get pods -n kube-system | grep public-cr24a9f2caf6554648836337d240064935-alb1
    ```
    {: pre}

    出力例:
    ```
    public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq   2/2       Running   0          24m
    public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-trqxc   2/2       Running   0          24m
    ```
    {: screen}

3. すべての ALB ポッドに対して実行されるヘルス・チェックを無効にします。 前のステップで取得した ALB ポッドごとに、これらのステップを繰り返します。 これらのステップのコマンドと出力の例では、1 番目のポッド `public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq` を使用しています。
    1. ALB ポッドにログインして、NGINX 構成ファイルで `server_name` 行を確認します。
        ```
        kubectl exec -ti public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq -n kube-system -c nginx-ingress -- grep server_name /etc/nginx/conf.d/kube-system-alb-health.conf
        ```
        {: pre}

        ALB ポッドが正しいヘルス・チェックのホスト名 `albhealth.<domain>` で構成されたことを確認できる出力例:
        ```
        server_name albhealth.mycluster-12345.us-south.containers.appdomain.cloud;
        ```
        {: screen}

    2. ヘルス・チェックを無効にして IP を削除するには、`server_name` の前に `#` を挿入します。 `albhealth.mycluster-12345.us-south.containers.appdomain.cloud` 仮想ホストが ALB に対して無効になっている場合、自動化ヘルス・チェックによって自動的に DNS 応答から IP が削除されます。
        ```
        kubectl exec -ti public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq -n kube-system -c nginx-ingress -- sed -i -e 's*server_name*#server_name*g' /etc/nginx/conf.d/kube-system-alb-health.conf
        ```
        {: pre}

    3. 変更が適用されたことを確認します。
        ```
        kubectl exec -ti public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq -n kube-system -c nginx-ingress -- grep server_name /etc/nginx/conf.d/kube-system-alb-health.conf
        ```
        {: pre}

        出力例:
        ```
        #server_name albhealth.mycluster-12345.us-south.containers.appdomain.cloud
        ```
        {: screen}

    4. DNS 登録から IP を削除するには、NGINX 構成を再ロードします。
        ```
        kubectl exec -ti public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq -n kube-system -c nginx-ingress -- nginx -s reload
        ```
        {: pre}

    5. ALB ポッドごとに、これらのステップを繰り返します。

4. ここで、`albhealth` ホストに cURL を実行して ALB IP をヘルス・チェックしようとすると、チェックが失敗します。
    ```
    curl -X GET http://169.62.196.238/ -H "Host: albhealth.mycluster-12345.us-south.containers.appdomain.cloud"
    ```
    {: pre}

    出力:
    ```
    <html>
        <head>
            <title>404 Not Found</title>
        </head>
        <body bgcolor="white"><center>
            <h1>404 Not Found</h1>
        </body>
    </html>
    ```
    {: screen}

5. Cloudflare サーバーを調べて、ご使用のドメインの DNS 登録から ALB IP アドレスが削除されたことを確認します。 DNS 登録は更新に数分かかることがあります。
    ```
    host mycluster-12345.us-south.containers.appdomain.cloud ada.ns.cloudflare.com
    ```
    {: pre}

    正常な ALB IP (`169.46.52.222`) のみが DNS 登録に残っており、正常でない ALB IP (`169.62.196.238`) が削除されたことを確認できる出力例:
    ```
    mycluster-12345.us-south.containers.appdomain.cloud has address 169.46.52.222
    ```
    {: screen}

6. これで、ALB IP が実動から削除されたので、これを使用してアプリに対してデバッグ・テストを実行できます。 この IP を使用してアプリとの通信をテストするには、次の cURL コマンドを、例の値を独自の値に置き換えて実行します。
    ```
    curl -X GET --resolve mycluster-12345.us-south.containers.appdomain.cloud:443:169.62.196.238 https://mycluster-12345.us-south.containers.appdomain.cloud/
    ```
    {: pre}

    * すべてが正しく構成されていれば、アプリから予期される応答が返されます。
    * 応答にエラーがある場合、アプリにエラーがあるか、この特定の ALB にのみ適用される構成にエラーがある可能性があります。 アプリのコード、[Ingress リソース構成ファイル](/docs/containers?topic=containers-ingress#public_inside_4)、またはこの ALB のみに適用したその他の構成を確認します。

7. デバッグが完了したら、ALB ポッドに対するヘルス・チェックをリストアします。 ALB ポッドごとに、これらのステップを繰り返します。
  1. ALB ポッドにログインし、`server_name` から `#` を削除します。
    ```
    kubectl exec -ti <pod_name> -n kube-system -c nginx-ingress -- sed -i -e 's*#server_name*server_name*g' /etc/nginx/conf.d/kube-system-alb-health.conf
    ```
    {: pre}

  2. ヘルス・チェックのリストアが適用されるように、NGINX 構成を再ロードします。
    ```
    kubectl exec -ti <pod_name> -n kube-system -c nginx-ingress -- nginx -s reload
    ```
    {: pre}

9. ここで、`albhealth` ホストに cURL を実行して ALB IP をヘルス・チェックすると、チェックで `healthy` が返されます。
    ```
    curl -X GET http://169.62.196.238/ -H "Host: albhealth.mycluster-12345.us-south.containers.appdomain.cloud"
    ```
    {: pre}

10. Cloudflare サーバーを調べて、ご使用のドメインの DNS 登録で ALB IP アドレスがリストアされたことを確認します。 DNS 登録は更新に数分かかることがあります。
    ```
    host mycluster-12345.us-south.containers.appdomain.cloud ada.ns.cloudflare.com
    ```
    {: pre}

    出力例:
    ```
    mycluster-12345.us-south.containers.appdomain.cloud has address 169.46.52.222
    mycluster-12345.us-south.containers.appdomain.cloud has address 169.62.196.238
    ```
    {: screen}

<br />


## ヘルプとサポートの取得
{: #ingress_getting_help}

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

