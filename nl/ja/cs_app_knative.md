---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

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


# Knative を使用したサーバーレス・アプリのデプロイ
{: #serverless-apps-knative}

{{site.data.keyword.containerlong_notm}} の Kubernetes クラスターに Knative をインストールして使用する方法について説明します。
{: shortdesc}

**Knative とは何ですか? なぜ使用するのですか?**</br>
[Knative](https://github.com/knative/docs) は IBM、Google、Pivotal、Red Hat、Cisco などによって開発されたオープン・ソースのプラットフォームです。Kubernetes の機能を拡張して、最新でソース中心のコンテナー化されたサーバーレス・アプリを Kubernetes クラスター上に作成できるようにすることを目的としています。このプラットフォームは、12-Factor App、コンテナー、または関数といった、クラウドで実行するアプリのタイプを決定しなければならない今日の開発者のニーズに対処するように設計されています。 各タイプのアプリには、それらのアプリのために調整されたオープン・ソース・ソリューションまたは独自のソリューション、つまり、12-Factor App には Cloud Foundry、コンテナーには Kubernetes、関数には OpenWhisk などが必要になります。 以前は、開発者は従うべきアプローチを決定する必要があり、これによって、異なるタイプのアプリを結合しなければならない場合に、柔軟性がなく、複雑度が増すことになりました。  

Knative では、プログラミング言語やフレームワークにわたって一貫性のあるアプローチが使用され、Kubernetes でのワークロードの作成、デプロイ、および管理の作業負担が減るので、開発者は、最も重要なこと、つまりソース・コードに集中できます。 既に慣れ親しんだ実績のあるビルド・パック (Cloud Foundry、Kaniko、Dockerfile、Bazel など) を使用できます。 Istio と統合されることによって、Knative では、サーバーレスでコンテナー化されたワークロードを容易にインターネットに公開し、モニターして制御することが可能になり、データは転送中に確実に暗号化されます。

**Knative はどのように機能しますか?**</br>
Knative は、3 つのキー・コンポーネント (_プリミティブ_ と呼ばれます) から構成され、Kubernetes クラスターでのサーバーレス・アプリの作成、デプロイ、および管理を支援します。

- **Build:** `Build` プリミティブは、ソース・コードからコンテナー・イメージまでのアプリを作成する一連の手順をサポートします。 アプリ・コードを検索するソース・リポジトリーと、イメージをホストするコンテナー・レジストリーを指定する単純なビルド・テンプレートを使用するとします。 単一コマンドを使用するだけで、このビルド・テンプレートを取得し、ソース・コードをプルし、イメージを作成し、コンテナー・レジストリーにイメージをプッシュするように Knative に指示することができ、コンテナー内のイメージを使用できるようになります。
- **Serving:** `Serving` プリミティブは、Knative サービスとしてサーバーレス・アプリをデプロイして、それらを自動的にスケーリングすることを支援し、インスタンスをゼロにすることもできます。 サーバーレスでコンテナー化されたワークロードを公開するために、Knative では Istio が使用されます。マネージド Knative アドオンをインストールすると、マネージド Istio アドオンも自動的にインストールされます。 Istio のトラフィック管理機能とインテリジェント・ルーティング機能を使用することによって、サービスの特定バージョンにルーティングされるようにトラフィックを制御できるので、開発者がアプリの新規バージョンをテストしてロールアウトすることや A-B テストを実行することが容易になります。
- **Eventing:** `Eventing` プリミティブを使用すると、他のサービスがサブスクライブできるトリガーまたはイベント・ストリームを作成できます。 例えば、GitHub マスター・リポジトリーにコードがプッシュされるごとに、アプリの新規ビルドを開始するなどです。 または、温度が氷点を下回った場合にのみサーバーレス・アプリを実行するなどです。 例えば、特定のイベントが発生した場合にアプリの作成とデプロイメントを自動化する CI/CD パイプラインに `Eventing` プリミティブを統合できます。

**Managed Knative on {{site.data.keyword.containerlong_notm}} (試験的) アドオンとは何ですか?** </br>
{{site.data.keyword.containerlong_notm}} のマネージド Knative は、Kubernetes クラスターに Knative と Istio を直接統合する[マネージド・アドオン](/docs/containers?topic=containers-managed-addons#managed-addons)です。 アドオン内の Knative と Istio のバージョンは、IBM によってテストされ、{{site.data.keyword.containerlong_notm}} での使用をサポートされています。 アドオンの管理について詳しくは、[管理対象アドオンを使用したサービスの追加](/docs/containers?topic=containers-managed-addons#managed-addons)を参照してください。

**何か制限はありますか?** </br>
クラスターに [container image security enforcer アドミッション・コントローラー](/docs/services/Registry?topic=registry-security_enforce#security_enforce)をインストールしている場合、マネージド Knative アドオンをクラスターで有効にすることはできません。

## クラスターでの Knative のセットアップ
{: #knative-setup}

Knative は、Istio の上に構築され、サーバーレスでコンテナー化されたワークロードをクラスター内およびインターネット上で公開できるようにします。 また、Istio を使用すると、サービス間のネットワーク・トラフィックをモニターして制御することができ、データは転送中に確実に暗号化されます。 マネージド Knative アドオンをインストールすると、マネージド Istio アドオンも自動的にインストールされます。
{: shortdesc}

開始前に、以下のことを行います。
-  [IBM Cloud CLI、{{site.data.keyword.containerlong_notm}} プラグイン、および Kubernetes CLI をインストールします](/docs/containers?topic=containers-cs_cli_install#cs_cli_install_steps)。 必ず、ご使用のクラスターの Kubernetes バージョンに一致する `kubectl` CLI バージョンをインストールしてください。
-  [それぞれが 4 コアと 16 GB のメモリー (`b3c.4x16`) 以上を備えた、少なくとも 3 つのワーカー・ノードのある標準クラスターを作成します](/docs/containers?topic=containers-clusters#clusters_ui)。また、クラスターとワーカー・ノードは、少なくともサポートされる最低バージョンの Kubernetes で実行される必要があり、これについては、`ibmcloud ks addon-versions --addon knative` を実行して確認できます。
-  {{site.data.keyword.containerlong_notm}} に対する[**ライター**または**管理者**の {{site.data.keyword.Bluemix_notm}} IAM サービス役割](/docs/containers?topic=containers-users#platform)があることを確認してください。
-  [CLI のターゲットを自分のクラスターに設定します](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)。
</br>

クラスターに Knative をインストールするには、以下のようにします。

1. クラスターでマネージド Knative アドオンを有効にします。 クラスターで Knative を有効にすると、Istio コンポーネントおよびすべての Knative コンポーネントがクラスターにインストールされます。
   ```
   ibmcloud ks cluster-addon-enable knative --cluster <cluster_name_or_ID> -y
   ```
   {: pre}

   出力例:
   ```
   Enabling add-on knative for cluster knative...
   OK
   ```
   {: screen}

   すべての Knative コンポーネントのインストールが完了するまで、数分かかることがあります。

2. Istio が正常にインストールされていることを確認します。 9 つの Istio サービス用のすべてのポッドと Prometheus 用のポッドが `Running` 状況になっている必要があります。
   ```
   kubectl get pods --namespace istio-system
   ```
   {: pre}

   出力例:
   ```
   NAME                                       READY     STATUS      RESTARTS   AGE
    istio-citadel-748d656b-pj9bw               1/1       Running     0          2m
    istio-egressgateway-6c65d7c98d-l54kg       1/1       Running     0          2m
    istio-galley-65cfbc6fd7-bpnqx              1/1       Running     0          2m
    istio-ingressgateway-f8dd85989-6w6nj       1/1       Running     0          2m
    istio-pilot-5fd885964b-l4df6               2/2       Running     0          2m
    istio-policy-56f4f4cbbd-2z2bk              2/2       Running     0          2m
    istio-sidecar-injector-646655c8cd-rwvsx    1/1       Running     0          2m
    istio-statsd-prom-bridge-7fdbbf769-8k42l   1/1       Running     0          2m
    istio-telemetry-8687d9d745-mwjbf           2/2       Running     0          2m
    prometheus-55c7c698d6-f4drj                1/1       Running     0          2m
   ```
   {: screen}

3. オプション: `default` 名前空間のすべてのアプリに Istio を使用する場合は、この名前空間 `istio-injection=enabled` ラベルを追加します。 アプリを Istio サービス・メッシュに含めるためには、各サーバーレス・アプリ・ポッドで Envoy プロキシー・サイドカーが実行される必要があります。 このラベルによって、ポッドが Envoy プロキシー・サイドカー・コンテナーとともに作成されるように Istio が新しいアプリ・デプロイメントのポッド・テンプレート仕様を自動的に変更できます。
   ```
   kubectl label namespace default istio-injection=enabled
   ```
   {: pre}

4. Knative `Serving` コンポーネントのすべてのポッドが `Running` 状態であることを確認します。
   ```
   kubectl get pods --namespace knative-serving
   ```
   {: pre}

   出力例:
   ```
   NAME                          READY     STATUS    RESTARTS   AGE
      activator-598b4b7787-ps7mj    2/2       Running   0          21m
      autoscaler-5cf5cfb4dc-mcc2l   2/2       Running   0          21m
      controller-7fc84c6584-qlzk2   1/1       Running   0          21m
      webhook-7797ffb6bf-wg46v      1/1       Running   0          21m
   ```
   {: screen}

## Knative サービスを使用したサーバーレス・アプリのデプロイ
{: #knative-deploy-app}

クラスターで Knative をセットアップした後、サーバーレス・アプリを Knative サービスとしてデプロイできます。
{: shortdesc}

**Knative サービスとは何ですか?** </br>
Knative を使用してアプリをデプロイするには、Knative `サービス`・リソースを指定する必要があります。 Knative サービスは、Knative `Serving` プリミティブによって管理され、ワークロードのライフサイクル全体の管理を行います。 サービスを作成すると、Knative `Serving` プリミティブによって自動的にサーバーレス・アプリのバージョンが作成され、このバージョンがサービスのリビジョン・ヒストリーに追加されます。サーバーレス・アプリには `<knative_service_name>.<namespace>.<ingress_subdomain>` 形式で Ingress サブドメインのパブリック URL が割り当てられ、これを使用してインターネットからアプリにアクセスできます。また、アプリには `<knative_service_name>.<namespace>.cluster.local` 形式でプライベート・ホスト名が割り当てられ、これを使用してクラスター内からアプリにアクセスできます。

**Knative サービスの作成時にバックグラウンドでは何が起こりますか?**</br>
Knative サービスを作成すると、アプリは自動的に Kubernetes ポッドとしてクラスターにデプロイされ、Kubernetes サービスを使用して公開されます。パブリック・ホスト名を割り当てるために、Knative は IBM 提供の Ingress サブドメインと TLS 証明書を使用します。着信ネットワーク・トラフィックは、IBM 提供のデフォルトの Ingress ルーティング・ルールに基づいてルーティングされます。

**アプリの新規バージョンをロールアウトするにはどうすればよいですか?**</br>
Knative サービスを更新すると、サーバーレス・アプリの新規バージョンが作成されます。このバージョンには、前のバージョンと同じパブリック・ホスト名およびプライベート・ホスト名が割り当てられます。デフォルトでは、すべての着信ネットワーク・トラフィックは、アプリの最新バージョンにルーティングされます。ただし、A-B テストを実行できるように、特定のアプリ・バージョンにルーティングする着信ネットワーク・トラフィックのパーセンテージを指定することもできます。着信ネットワーク・トラフィックを、アプリの現行バージョンとロールオーバーする新規バージョンの 2 つのアプリ・バージョン間で分割できます。  

**独自のカスタム・ドメインと TLS 証明書を使用できますか?** </br>
サーバーレス・アプリにホスト名を割り当てるときに、カスタム・ドメイン・ネームおよび TLS 証明書を使用するように、Istio Ingress ゲートウェイおよび Ingress ルーティング・ルールの構成マップを変更できます。詳しくは、[カスタム・ドメイン・ネームおよび証明書のセットアップ](#knative-custom-domain-tls)を参照してください。

サーバーレス・アプリを Knative サービスとしてデプロイするには、以下のようにします。

1. Knative を使用する Go 言語で記述された最初のサーバーレス [`Hello World`](https://hub.docker.com/r/ibmcom/kn-helloworld) アプリの YAML ファイルを作成します。サンプル・アプリに要求を送信すると、アプリでは、環境変数 `TARGET` が読み取られ、`"Hello ${TARGET}!"` が出力されます。 この環境変数が空の場合は、`"Hello World!"` が返されます。
   ```
   apiVersion: serving.knative.dev/v1alpha1
   kind: Service
   metadata:
     name: kn-helloworld
     namespace: default
   spec:
     runLatest:
       configuration:
         revisionTemplate:
           spec:
             container:
               image: docker.io/ibmcom/kn-helloworld
               env:
               - name: TARGET
                 value: "Go Sample v1"
    ```
    {: codeblock}

    <table>
    <caption>YAML ファイルの構成要素について</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> YAML ファイルの構成要素について</th>
    </thead>
    <tbody>
    <tr>
    <td><code>metadata.name</code></td>
    <td>Knative サービスの名前。</td>
    </tr>
    <tr>
    <td><code>metadata.namespace</td>
    <td>オプション: Knative サービスとしてアプリをデプロイする Kubernetes 名前空間。 デフォルトでは、すべてのサービスが <code>default</code> Kubernetes 名前空間にデプロイされます。</td>
    </tr>
    <tr>
    <td><code>spec.container.image</code></td>
    <td>イメージが保管されているコンテナー・レジストリーの URL。 この例では、Docker Hub の <code>ibmcom</code> 名前空間に保管されている Knative Hello World アプリをデプロイします。 </td>
    </tr>
    <tr>
    <td><code>spec.container.env</code></td>
    <td>オプション: Knative サービスで使用する環境変数のリスト。 この例では、環境変数 <code>TARGET</code> の値が、サンプル・アプリによって読み取られ、アプリに要求を送信したときに <code>"Hello ${TARGET}!"</code> 形式で返されます。 値がない場合は、サンプル・アプリは <code>"Hello World!"</code> を返します。  </td>
    </tr>
    </tbody>
    </table>

2. クラスターに Knative サービスを作成します。
   ```
   kubectl apply -f service.yaml
   ```
   {: pre}

   出力例:
   ```
   service.serving.knative.dev "kn-helloworld" created
   ```
   {: screen}

3. Knative サービスが作成されていることを確認します。CLI 出力で、サーバーレス・アプリに割り当てられているパブリック **DOMAIN** を確認できます。**LATESTCREATED** 列と **LATESTREADY** 列には、最後に作成されたアプリのバージョンと現在デプロイされているアプリのバージョンが `<knative_service_name>-<version>` 形式で表示されます。アプリに割り当てられるバージョンは、ランダムなストリング値です。この例では、サーバーレス・アプリのバージョンは `rjmwt` です。サービスを更新すると、アプリの新規バージョンが作成され、そのバージョンに新規ランダム・ストリングが割り当てられます。  
   ```
   kubectl get ksvc/kn-helloworld
   ```
   {: pre}

   出力例:
   ```
   NAME            DOMAIN                                                                LATESTCREATED         LATESTREADY           READY   REASON
      kn-helloworld   kn-helloworld.default.mycluster.us-south.containers.appdomain.cloud   kn-helloworld-rjmwt   kn-helloworld-rjmwt   True
   ```
   {: screen}

4. アプリに割り当てられているパブリック URL に要求を送信して、`Hello World` アプリを試行します。
   ```
   curl -v <public_app_url>
   ```
   {: pre}

   出力例:
   ```
   * Rebuilt URL to: kn-helloworld.default.mycluster.us-south.containers.appdomain.cloud/
   *   Trying 169.46.XX.XX...
   * TCP_NODELAY set
   * Connected to kn-helloworld.default.mycluster.us-south.containers.appdomain.cloud (169.46.XX.XX) port 80 (#0)
   > GET / HTTP/1.1
   > Host: kn-helloworld.default.mycluster.us-south.containers.appdomain.cloud
   > User-Agent: curl/7.54.0
   > Accept: */*
   >
   < HTTP/1.1 200 OK
      < Date: Thu, 21 Mar 2019 01:12:48 GMT
      < Content-Type: text/plain; charset=utf-8
      < Content-Length: 20
      < Connection: keep-alive
      < x-envoy-upstream-service-time: 17
      <
      Hello Go Sample v1!
   * Connection #0 to host kn-helloworld.default.mycluster.us-south.containers.appdomain.cloud left intact
   ```
   {: screen}

5. Knative サービス用に作成されたポッドの数をリストします。このトピックの例では、2 つのコンテナーで構成される 1 つのポッドがデプロイされます。一方のコンテナーは、`Hello World` アプリを実行し、もう一方のコンテナーは、Istio および Knative のモニタリング・ツールとロギング・ツールを実行するサイドカーです。
   ```
   kubectl get pods
   ```
   {: pre}

   出力例:
   ```
   NAME                                             READY     STATUS    RESTARTS   AGE
   kn-helloworld-rjmwt-deployment-55db6bf4c5-2vftm  2/2      Running   0          16s
   ```
   {: screen}

6. Knative でポッドがスケール・ダウンされるように、数分待ちます。 Knative は、着信ワークロードを処理するために同時に稼働している必要のあるポッドの数を評価します。 ネットワーク・トラフィックが受信されない場合、Knative は自動的にポッドをスケール・ダウンして、この例に示されているように、ポッドをゼロにすることもできます。

   Knative でポッドがどのようにスケール・アップされるかご覧になりますか? アプリのワークロードを増やすことを試行してください。それには、例えば、[Simple cloud-based load tester](https://loader.io/) などのツールを使用します。
   {: tip}
   ```
   kubectl get pods
   ```
   {: pre}

   `kn-helloworld` ポッドが表示されない場合、Knative によってアプリがスケール・ダウンされてポッドがゼロになっています。

7. Knative サービス・サンプルを更新して、`TARGET` 環境変数に別の値を入力します。

   サービス YAML 例:
   ```
   apiVersion: serving.knative.dev/v1alpha1
   kind: Service
   metadata:
     name: kn-helloworld
     namespace: default
   spec:
     runLatest:
       configuration:
         revisionTemplate:
           spec:
             container:
               image: docker.io/ibmcom/kn-helloworld
               env:
               - name: TARGET
                 value: "Mr. Smith"
   ```
   {: codeblock}

8. 変更をサービスに適用します。 構成を変更すると、Knative は自動的にアプリの新規バージョンを作成します。
   ```
   kubectl apply -f service.yaml
   ```
   {: pre}

9. アプリの新規バージョンがデプロイされていることを確認します。CLI 出力の **LATESTCREATED** 列で、アプリの新規バージョンを確認できます。**LATESTREADY** 列に同じアプリ・バージョンが表示されている場合、アプリはすべてセットアップされ、割り当てられたパブリック URL で着信ネットワーク・トラフィックを受信する準備ができています。
   ```
   kubectl get ksvc/kn-helloworld
   ```
   {: pre}

   出力例:
   ```
   NAME            DOMAIN                                                                LATESTCREATED         LATESTREADY           READY   REASON
   kn-helloworld   kn-helloworld.default.mycluster.us-south.containers.appdomain.cloud   kn-helloworld-ghyei   kn-helloworld-ghyei   True
   ```
   {: screen}

9. アプリに新規要求を送信して、変更が適用されていることを確認します。
   ```
   curl -v <service_domain>
   ```

   出力例:
   ```
   ...
   Hello Mr. Smith!
   ```
   {: screen}

10. 増えたネットワーク・トラフィックのために Knative によって再度ポッドがスケール・アップされたことを確認します。
    ```
    kubectl get pods
    ```

    出力例:
    ```
    NAME                                              READY     STATUS    RESTARTS   AGE
    kn-helloworld-ghyei-deployment-55db6bf4c5-2vftm   2/2       Running   0          16s
    ```
    {: screen}

11. オプション: Knative サービスをクリーンアップします。
    ```
    kubectl delete -f service.yaml
    ```
    {: pre}


## カスタム・ドメイン・ネームおよび証明書のセットアップ
{: #knative-custom-domain-tls}

TLS で構成した独自のカスタム・ドメインのホスト名が割り当てられるように Knative を構成できます。
{: shortdesc}

デフォルトでは、すべてのアプリには `<knative_service_name>.<namespace>.<ingress_subdomain>` 形式で Ingress サブドメインのパブリック・サブドメインが割り当てられ、これを使用してインターネットからアプリにアクセスできます。また、アプリには `<knative_service_name>.<namespace>.cluster.local` 形式でプライベート・ホスト名が割り当てられ、これを使用してクラスター内からアプリにアクセスできます。所有するカスタム・ドメインのホスト名を割り当てる場合は、代わりにカスタム・ドメインを使用するように Knative 構成マップを変更できます。

1. カスタム・ドメインを作成します。 カスタム・ドメインを登録するには、Domain Name Service (DNS) プロバイダーまたは [IBM Cloud DNS](/docs/infrastructure/dns?topic=dns-getting-started) を使用します。
2. 着信ネットワーク・トラフィックを IBM 提供の Ingress ゲートウェイにルーティングするようにドメインを構成します。以下の選択肢があります。
   - IBM 提供ドメインを正規名レコード (CNAME) として指定することで、カスタム・ドメインの別名を定義します。 IBM 提供の Ingress ドメインを確認するには、`ibmcloud ks cluster-get --cluster <cluster_name>` を実行し、**「Ingress サブドメイン (Ingress subdomain)」**フィールドを見つけます。 IBM では IBM サブドメインに対する自動ヘルス・チェックを提供しており、障害のある IP がすべて DNS 応答から削除されるため、CNAME の使用をお勧めします。
   - カスタム・ドメインを Ingress ゲートウェイのポータブル・パブリック IP アドレスにマップします。これは、IP アドレスをレコードとして追加して行います。Ingress ゲートウェイのパブリック IP アドレスを確認するには、`nslookup <ingress_subdomain>` を実行します。
3. カスタム・ドメイン用の正式なワイルドカード TLS 証明書を購入します。複数の TLS 証明書を購入する場合は、証明書ごとに異なる [CN ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://support.dnsimple.com/articles/what-is-common-name/) を使用してください。
4. 証明書と鍵の Kubernetes シークレットを作成します。
   1. 証明書と鍵を base-64 にエンコードし、base-64 エンコード値を新しいファイルに保存します。
      ```
      openssl base64 -in tls.key -out tls.key.base64
      ```
      {: pre}

   2. 証明書と鍵の base-64 エンコード値を表示します。
      ```
      cat tls.key.base64
      ```
      {: pre}

   3. 証明書と鍵を使用して、シークレット YAML ファイルを作成します。
      ```
      apiVersion: v1
      kind: Secret
      metadata:
        name: mydomain-ssl
      type: Opaque
      data:
        tls.crt: <client_certificate>
        tls.key: <client_key>
      ```
      {: codeblock}

   4. クラスターに証明書を作成します。
      ```
      kubectl create -f secret.yaml
      ```
      {: pre}

5. クラスターの `istio-system` 名前空間の `iks-knative-ingress` Ingress リソースを開き、編集を開始します。
   ```
   kubectl edit ingress iks-knative-ingress -n istio-system
   ```
   {: pre}

6. Ingress のデフォルト・ルーティング・ルールを変更します。
   - カスタム・ドメインおよびサブドメインからのすべての着信ネットワーク・トラフィックが `istio-ingressgateway` にルーティングされるように、`spec.rules.host` セクションにカスタム・ワイルドカード・ドメインを追加します。
   - `spec.tls.hosts` セクションで、前に作成した TLS シークレットを使用するように、カスタム・ワイルドカード・ドメインのすべてのホストを構成します。

   Ingress の例:
   ```
   apiVersion: extensions/v1beta1
   kind: Ingress
   metadata:
     name: iks-knative-ingress
     namespace: istio-system
   spec:
     rules:
     - host: '*.mydomain'
       http:
         paths:
         - backend:
             serviceName: istio-ingressgateway
             servicePort: 80
           path: /
     tls:
     - hosts:
       - '*.mydomain'
       secretName: mydomain-ssl
   ```
   {: codeblock}

   `spec.rules.host` セクションと `spec.tls.hosts` セクションはリストであり、複数のカスタム・ドメインと TLS 証明書を含めることができます。
   {: tip}

7. カスタム・ドメインを使用するように Knative `config-domain` 構成マップを変更して、新規 Knative サービスにホスト名を割り当てます。
   1. `config-domain` 構成マップを開き、編集を開始します。
      ```
      kubectl edit configmap config-domain -n knative-serving
      ```
      {: pre}

   2. 構成マップの `data` セクションでカスタム・ドメインを指定し、クラスターに設定されているデフォルト・ドメインを削除します。
      - **すべての Knative サービスにカスタム・ドメインのホスト名を割り当てる例**:
        ```
        apiVersion: v1
        kind: ConfigMap
        data:
          <custom_domain>: ""
        metadata:
          name: config-domain
          namespace: knative-serving
        ```
        {: codeblock}

        カスタム・ドメインに `""` を追加することにより、作成するすべての Knative サービスにカスタム・ドメインのホスト名が割り当てられます。  

      - **選択した Knative サービスにカスタム・ドメインのホスト名を割り当てる例**:
        ```
        apiVersion: v1
        kind: ConfigMap
        data:
          <custom_domain>: |
            selector:
              app: sample
          mycluster.us-south.containers.appdomain.cloud: ""
        metadata:
          name: config-domain
          namespace: knative-serving
        ```
        {: codeblock}

        選択した Knative サービスにのみカスタム・ドメインのホスト名を割り当てるには、構成マップに `data.selector` ラベルのキーと値を追加します。この例では、ラベル `app: sample` が付いているすべてのサービスに、カスタム・ドメインのホスト名が割り当てられます。ラベル `app: sample` が付いていない他のすべてのアプリに割り当てるドメイン名も必ず指定してください。この例では、IBM 提供のデフォルト・ドメイン `mycluster.us-south.containers.appdomain.cloud` が使用されます。
    3. 変更を保存します。

Ingress ルーティング・ルールと Knative 構成マップをすべてセットアップしたら、カスタム・ドメインと TLS 証明書を使用して Knative サービスを作成できます。

## 別の Knative サービスからの Knative サービスへのアクセス
{: #knative-access-service}

Knative サービスに割り当てられた URL への REST API 呼び出しを使用して、別の Knative サービスから Knative サービスにアクセスできます。
{: shortdesc}

1. クラスター内のすべての Knative サービスをリストします。
   ```
   kubectl get ksvc --all-namespaces
   ```
   {: pre}

2. Knative サービスに割り当てられた **DOMAIN** を取得します。
   ```
   kubect get ksvc/<knative_service_name>
   ```
   {: pre}

   出力例:
   ```
   NAME        DOMAIN                                                            LATESTCREATED     LATESTREADY       READY   REASON
   myservice   myservice.default.mycluster.us-south.containers.appdomain.cloud   myservice-rjmwt   myservice-rjmwt   True
   ```
   {: screen}

3. ドメイン・ネームを使用して REST API 呼び出しを実装し、Knative サービスにアクセスします。この REST API 呼び出しは、Knative サービスの作成対象であるアプリの一部にする必要があります。アクセスする Knative サービスに `<service_name>.<namespace>.svc.cluster.local` 形式のローカル URL が割り当てられている場合、Knative は REST API 要求をクラスター内部ネットワーク内で保持します。

   Go 言語のコード・スニペット例:
   ```go
   resp, err := http.Get("<knative_service_domain_name>")
   if err != nil {
       fmt.Fprintf(os.Stderr, "Error: %s\n", err)
   } else if resp.Body != nil {
       body, _ := ioutil.ReadAll(resp.Body)
       fmt.Printf("Response: %s\n", string(body))
   }
   ```
   {: codeblock}

## 一般的な Knative サービスの設定
{: #knative-service-settings}

サーバーレス・アプリの開発時に役立ちそうな一般的な Knative サービスの設定について説明します。
{: shortdesc}

- [ポッドの最小数と最大数の設定](#knative-min-max-pods)
- [ポッドごとの要求最大数の指定](#max-request-per-pod)
- [プライベート専用サーバーレス・アプリの作成](#knative-private-only)
- [Knative サービスでのコンテナー・イメージの再プルの強制](#knative-repull-image)

### ポッドの最小数と最大数の設定
{: #knative-min-max-pods}

アノテーションを使用して、アプリに対して実行するポッドの最小数と最大数を指定できます。例えば、Knative でアプリがスケールダウンされてインスタンスがゼロになることを避ける場合、ポッドの最小数を 1 に設定します。
{: shortdesc}

```
apiVersion: serving.knative.dev/v1alpha1
kind: Service
metadata:
  name: ...
spec:
  runLatest:
    configuration:
      revisionTemplate:
        metadata:
          annotations:
            autoscaling.knative.dev/minScale: "1"
            autoscaling.knative.dev/maxScale: "100"
        spec:
          container:
            image:
...
```
{: codeblock}

<table>
<caption>YAML ファイルの構成要素について</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> YAML ファイルの構成要素について</th>
</thead>
<tbody>
<tr>
<td><code>autoscaling.knative.dev/minScale</code></td>
<td>クラスター内で実行するポッドの最小数を入力します。アプリでネットワーク・トラフィックが受信されない場合でも、Knative は、設定された数より低くまでアプリをスケールダウンすることはできません。デフォルトのポッド数はゼロです。 </td>
</tr>
<tr>
<td><code>autoscaling.knative.dev/maxScale</code></td>
<td>クラスター内で実行するポッドの最大数を入力します。現行のアプリ・インスタンスで処理できるよりも多くの要求がある場合でも、Knative は、設定された数より高くまでアプリをスケールアップすることはできません。</td>
</tr>
</tbody>
</table>

### ポッドごとの要求最大数の指定
{: #max-request-per-pod}

Knative でアプリ・インスタンスのスケールアップが考慮されるまでにアプリ・インスタンスが受信して処理できる要求の最大数を指定できます。例えば、要求最大数を 1 に設定すると、アプリ・インスタンスは一度に 1 つの要求を受信できます。最初の要求が完全に処理される前に 2 番目の要求が到着すると、Knative は別のインスタンスをスケールアップします。

```
apiVersion: serving.knative.dev/v1alpha1
kind: Service
metadata:
  name: myservice
spec:
  runLatest:
    configuration:
      revisionTemplate:
        spec:
          container:
            image: myimage
          containerConcurrency: 1
....
```
{: codeblock}

<table>
<caption>YAML ファイルの構成要素について</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> YAML ファイルの構成要素について</th>
</thead>
<tbody>
<tr>
<td><code>containerConcurrency</code></td>
<td>Knative でアプリ・インスタンスのスケールアップが考慮されるまでにアプリ・インスタンスが一度に受信できる要求の最大数を入力します。 </td>
</tr>
</tbody>
</table>

### プライベート専用サーバーレス・アプリの作成
{: #knative-private-only}

デフォルトで、すべての Knative サービスには、Istio Ingress サブドメインからのパブリック経路と、`<service_name>.<namespace>.cluster.local` 形式のプライベート経路が割り当てられます。パブリック経路を使用して、パブリック・ネットワークからアプリにアクセスできます。サービスをプライベートのままにする場合は、Knative サービスに `serving.knative.dev/visibility` ラベルを追加できます。このラベルを指定すると、Knative によってサービスにプライベート・ホスト名のみが割り当てられます。
{: shortdesc}

```
apiVersion: serving.knative.dev/v1alpha1
kind: Service
metadata:
  name: myservice
  labels:
    serving.knative.dev/visibility: cluster-local
spec:
  runLatest:
    configuration:
      revisionTemplate:
        spec:
          container:
            image:
...
```
{: codeblock}

<table>
<caption>YAML ファイルの構成要素について</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> YAML ファイルの構成要素について</th>
</thead>
<tbody>
<tr>
<td><code>serving.knative.dev/visibility</code></td>
  <td><code>serving.knative.dev/visibility: cluster-local</code> ラベルを追加すると、サービスには <code>&lt;service_name&gt;.&lt;namespace&gt;.cluster.local</code> 形式のプライベート経路のみが割り当てられます。プライベート・ホスト名を使用してクラスター内からサービスにアクセスできますが、パブリック・ネットワークからサービスにアクセスすることはできません。 </td>
</tr>
</tbody>
</table>

### Knative サービスでのコンテナー・イメージの再プルの強制
{: #knative-repull-image}

現在の Knative の実装では、Knative `Serving` コンポーネントでコンテナー・イメージを強制的に再プルする標準方法は提供されていません。レジストリーからイメージを再プルするには、以下のオプションのいずれかを選択します。

- **Knative サービス `revisionTemplate` の変更**: Knative サービスの `revisionTemplate` は、Knative サービスのリビジョンを作成するために使用されます。このリビジョン・テンプレートを変更 (例えば、`repullFlag` アノテーションを追加) すると、Knative はアプリの新規リビジョンを作成する必要があります。リビジョン作成の一部として、Knative はコンテナー・イメージの更新を確認する必要があります。`imagePullPolicy: Always` を設定すると、Knative は、クラスター内のイメージ・キャッシュを使用できず、代わりに、コンテナー・レジストリーからイメージをプルする必要があります。
   ```
   apiVersion: serving.knative.dev/v1alpha1
   kind: Service
   metadata:
     name: <service_name>
   spec:
     runLatest:
       configuration:
         revisionTemplate:
           metadata:
             annotations:
               repullFlag: 123
           spec:
             container:
               image: <image_name>
               imagePullPolicy: Always
    ```
    {: codeblock}

    コンテナー・レジストリーから最新のイメージ・バージョンをプルするサービスの新規リビジョンを作成するごとに、`repullFlag` 値を変更する必要があります。リビジョンごとに必ず固有値を使用して、2 つの同一の Knative サービス構成のために Knative で古いバージョンのイメージが使用されることがないようにしてください。  
    {: note}

- **タグを使用した固有コンテナー・イメージの作成**: 作成するコンテナー・イメージごとに固有タグを使用し、Knative サービスの `container.image` 構成でこのイメージを参照できます。以下の例では、`v1` がイメージ・タグとして使用されます。Knative で強制的にコンテナー・レジストリーから新規イメージをプルするには、イメージ・タグを変更する必要があります。例えば、新規イメージ・タグとして `v2` を使用します。
  ```
  apiVersion: serving.knative.dev/v1alpha1
  kind: Service
  metadata:
    name: <service_name>
  spec:
    runLatest:
      configuration:
          spec:
            container:
              image: myapp:v1
              imagePullPolicy: Always
    ```
    {: codeblock}


## 関連リンク  
{: #knative-related-links}

- この [Knative workshop ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/IBM/knative101/tree/master/workshop) を試行して、最初の `Node.js` フィボナッチ・アプリをクラスターにデプロイします。
  - GitHub の Dockerfile からイメージを作成して、そのイメージを自動的に {{site.data.keyword.registrylong_notm}} の名前空間にプッシュするため、Knative `Build` プリミティブの使用法を探索します。  
  - IBM 提供の Ingress サブドメインから、Knative により提供される Istio Ingress ゲートウェイへの、ネットワーク・トラフィックのルーティングをセットアップする方法を学習します。
  - アプリの新規バージョンをロールアウトし、Istio を使用して、各アプリ・バージョンにルーティングされるトラフィック量を制御します。
- [Knative `Eventing` ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/knative/docs/tree/master/eventing/samples) サンプルを探索します。
- Knative について詳しくは、[Knative の資料 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/knative/docs) を参照してください。
