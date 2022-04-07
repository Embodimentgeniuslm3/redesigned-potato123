---

copyright:
  years: 2014, 2019
lastupdated: "2019-04-11"

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


# チュートリアル: マネージド Knative を使用した Kubernetes クラスターでのサーバーレス・アプリの実行
{: #knative_tutorial}

このチュートリアルでは、{{site.data.keyword.containerlong_notm}} の Kubernetes クラスターに Knative をインストールする方法を確認できます。
{: shortdesc}

**Knative とは何ですか? なぜ使用する必要があるのですか?**</br>
[Knative](https://github.com/knative/docs) は、Kubernetes の機能を拡張することを目標として、IBM、Google、Pivotal、Red Hat、Cisco などによって開発されたオープン・ソースのプラットフォームであり、最新のソース中心のコンテナー化されたサーバーレス・アプリを Kubernetes クラスター上に作成することを支援します。 このプラットフォームは、12-Factor App、コンテナー、または関数といった、クラウドで実行するアプリのタイプを決定しなければならない今日の開発者のニーズに対処するように設計されています。 各タイプのアプリには、それらのアプリのために調整されたオープン・ソース・ソリューションまたは独自のソリューション、つまり、12-Factor App には Cloud Foundry、コンテナーには Kubernetes、関数には OpenWhisk などが必要になります。 以前は、開発者は従うべきアプローチを決定する必要があり、これによって、異なるタイプのアプリを結合しなければならない場合に、柔軟性がなく、複雑度が増すことになりました。  

Knative では、プログラミング言語やフレームワークにわたって一貫性のあるアプローチが使用され、Kubernetes でのワークロードの作成、デプロイ、および管理の作業負担が減るので、開発者は、最も重要なこと、つまりソース・コードに集中できます。 既に慣れ親しんだ実績のあるビルド・パック (Cloud Foundry、Kaniko、Dockerfile、Bazel など) を使用できます。 Istio と統合されることによって、Knative では、サーバーレスでコンテナー化されたワークロードを容易にインターネットに公開し、モニターして制御することが可能になり、データは転送中に確実に暗号化されます。

**Knative はどのように機能しますか?**</br>
Knative は、3 つのキー・コンポーネント (_プリミティブ_ と呼ばれます) から構成され、Kubernetes クラスターでのサーバーレス・アプリの作成、デプロイ、および管理を支援します。

- **Build:** `Build` プリミティブは、ソース・コードからコンテナー・イメージまでのアプリを作成する一連の手順をサポートします。 アプリ・コードを検索するソース・リポジトリーと、イメージをホストするコンテナー・レジストリーを指定する単純なビルド・テンプレートを使用するとします。 単一コマンドを使用するだけで、このビルド・テンプレートを取得し、ソース・コードをプルし、イメージを作成し、コンテナー・レジストリーにイメージをプッシュするように Knative に指示することができ、コンテナー内のイメージを使用できるようになります。
- **Serving:** `Serving` プリミティブは、Knative サービスとしてサーバーレス・アプリをデプロイして、それらを自動的にスケーリングすることを支援し、インスタンスをゼロにすることもできます。 Istio のトラフィック管理機能とインテリジェント・ルーティング機能を使用することによって、サービスの特定バージョンにルーティングされるようにトラフィックを制御でき、それにより、開発者がアプリの新規バージョンをテストしてロールアウトすることや A-B テストを実行することが容易になります。
- **Eventing:** `Eventing` プリミティブを使用すると、他のサービスがサブスクライブできるトリガーまたはイベント・ストリームを作成できます。 例えば、GitHub マスター・リポジトリーにコードがプッシュされるごとに、アプリの新規ビルドを開始するなどです。 または、温度が氷点を下回った場合にのみサーバーレス・アプリを実行するなどです。 `Eventing` プリミティブは、特定のイベントが発生した場合にアプリの作成とデプロイメントを自動化する CI/CD パイプラインに統合できます。

**Managed Knative on {{site.data.keyword.containerlong_notm}} (試験的) アドオンとは何ですか?** </br>
{{site.data.keyword.containerlong_notm}} のマネージド Knative は、Kubernetes クラスターに Knative と Istio を直接統合するマネージド・アドオンです。 アドオン内の Knative と Istio のバージョンは、IBM によってテストされ、{{site.data.keyword.containerlong_notm}} での使用をサポートされています。 アドオンの管理について詳しくは、[管理対象アドオンを使用したサービスの追加](/docs/containers?topic=containers-managed-addons#managed-addons)を参照してください。

**何か制限はありますか?** </br>
クラスターに [container image security enforcer アドミッション・コントローラー](/docs/services/Registry?topic=registry-security_enforce#security_enforce)をインストールしている場合、マネージド Knative アドオンをクラスターで有効にすることはできません。

よろしいですね? 以下のチュートリアルに従って、{{site.data.keyword.containerlong_notm}} で Knative を開始してください。

## 達成目標
{: #knative_objectives}

- Knative および Knative プリミティブについての基礎を学習します。  
- マネージド Knative およびマネージド Istio のアドオンをクラスターにインストールします。
- Knative `Serving` プリミティブを使用して、Knative を使用する最初のサーバーレス・アプリをデプロイして、そのアプリをインターネットで公開します。
- Knative のスケーリング機能およびリビジョン機能を探索します。

## 所要時間
{: #knative_time}

30 分

## 対象読者
{: #knative_audience}

このチュートリアルは、Kubernetes クラスターにサーバーレス・アプリをデプロイするために Knative の使用法を学習することに関心がある開発者、およびクラスターに Knative をセットアップする方法を学習するクラスター管理者のために設計されています。

## 前提条件
{: #knative_prerequisites}

-  [IBM Cloud CLI、{{site.data.keyword.containerlong_notm}} プラグイン、および Kubernetes CLI をインストールします](/docs/containers?topic=containers-cs_cli_install#cs_cli_install_steps)。 必ず、ご使用のクラスターの Kubernetes バージョンに一致する `kubectl` CLI バージョンをインストールしてください。
-  [それぞれが 4 コアと 16 GB のメモリー (`b3c.4x16`) 以上を備えた、少なくとも 3 つのワーカー・ノードのあるクラスターを作成します](/docs/containers?topic=containers-clusters#clusters_cli)。 すべてのワーカー・ノードは、Kubernetes バージョン 1.12 以上を実行している必要があります。
-  {{site.data.keyword.containerlong_notm}} に対する[**ライター**または**管理者**の {{site.data.keyword.Bluemix_notm}} IAM サービス役割](/docs/containers?topic=containers-users#platform)があることを確認してください。
-  [CLI のターゲットを自分のクラスターに設定します](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)。

## レッスン 1: マネージド Knative アドオンをセットアップする
{: #knative_setup}

Knative は、Istio の上に構築され、サーバーレスでコンテナー化されたワークロードをクラスター内およびインターネット上で公開できるようにします。 また、Istio を使用すると、サービス間のネットワーク・トラフィックをモニターして制御することができ、データは転送中に確実に暗号化されます。 マネージド Knative アドオンをインストールすると、マネージド Istio アドオンも自動的にインストールされます。
{: shortdesc}

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

4. すべての Knative コンポーネントが正常にインストールされていることを確認します。
   1. Knative `Serving` コンポーネントのすべてのポッドが `Running` 状態であることを確認します。  
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

   2. Knative `Build` コンポーネントのすべてのポッドが `Running` 状態であることを確認します。  
      ```
      kubectl get pods --namespace knative-build
      ```
      {: pre}

      出力例:
      ```
      NAME                                READY     STATUS    RESTARTS   AGE
      build-controller-79cb969d89-kdn2b   1/1       Running   0          21m
      build-webhook-58d685fc58-twwc4      1/1       Running   0          21m
      ```
      {: screen}

   3. Knative `Eventing` コンポーネントのすべてのポッドが `Running` 状態であることを確認します。
      ```
      kubectl get pods --namespace knative-eventing
      ```
      {: pre}

      出力例:

      ```
      NAME                                            READY     STATUS    RESTARTS   AGE
      eventing-controller-847d8cf969-kxjtm            1/1       Running   0          22m
      in-memory-channel-controller-59dd7cfb5b-846mn   1/1       Running   0          22m
      in-memory-channel-dispatcher-67f7497fc-dgbrb    2/2       Running   1          22m
      webhook-7cfff8d86d-vjnqq                        1/1       Running   0          22m
      ```
      {: screen}

   4. Knative `Sources` コンポーネントのすべてのポッドが `Running` 状態であることを確認します。
      ```
      kubectl get pods --namespace knative-sources
      ```
      {: pre}

      出力例:
      ```
      NAME                   READY     STATUS    RESTARTS   AGE
      controller-manager-0   1/1       Running   0          22m
      ```
      {: screen}

   5. Knative `Monitoring` コンポーネントのすべてのポッドが `Running` 状態であることを確認します。
      ```
      kubectl get pods --namespace knative-monitoring
      ```
      {: pre}

      出力例:
      ```
      NAME                                  READY     STATUS                 RESTARTS   AGE
      elasticsearch-logging-0               1/1       Running                0          22m
      elasticsearch-logging-1               1/1       Running                0          21m
      grafana-79cf95cc7-mw42v               1/1       Running                0          21m
      kibana-logging-7f7b9698bc-m7v6r       1/1       Running                0          22m
      kube-state-metrics-768dfff9c5-fmkkr   4/4       Running                0          21m
      node-exporter-dzlp9                   2/2       Running                0          22m
      node-exporter-hp6gv                   2/2       Running                0          22m
      node-exporter-hr6vs                   2/2       Running                0          22m
      prometheus-system-0                   1/1       Running                0          21m
      prometheus-system-1                   1/1       Running                0          21m
      ```
      {: screen}

Knative および Istio がすべてセットアップされたので、最初のサーバーレス・アプリをクラスターにデプロイできます。

## レッスン 2: サーバーレス・アプリをクラスターにデプロイする
{: #deploy_app}

このレッスンでは、最初のサーバーレス [`Hello World`](https://hub.docker.com/r/ibmcom/kn-helloworld) アプリをデプロイしてみます。 サンプル・アプリに要求を送信すると、アプリでは、環境変数 `TARGET` が読み取られ、`"Hello ${TARGET}!"` が出力されます。 この環境変数が空の場合は、`"Hello World!"` が返されます。
{: shortdesc}

1. Knative で、最初のサーバーレス `Hello World` アプリの YAML ファイルを作成します。 Knative を使用してアプリをデプロイするには、Knative サービス・リソースを指定する必要があります。 サービスは、Knative `Serving` プリミティブによって管理され、ワークロードのライフサイクル全体の管理を行います。 サービスによって、各デプロイメントに Knative リビジョン、経路、および構成が確実に提供されます。 サービスを更新すると、アプリの新規バージョンが作成され、サービスのリビジョン・ヒストリーに追加されます。 Knative 経路によって、アプリの各リビジョンがネットワーク・エンドポイントにマップされ、特定のリビジョンにルーティングされるネットワーク・トラフィック量を制御できるようになります。 Knative 構成によって、特定のリビジョンの設定が保持されるので、いつでも、古いリビジョンにロールバックしたり、リビジョン間で切り替えたりすることができます。 Knative `Serving` リソースについて詳しくは、[Knative の資料](https://github.com/knative/docs/tree/master/serving)を参照してください。
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
    <td>Knative サービスとしてアプリをデプロイする Kubernetes 名前空間。 </td>
    </tr>
    <tr>
    <td><code>spec.container.image</code></td>
    <td>イメージが保管されているコンテナー・レジストリーの URL。 この例では、Docker Hub の <code>ibmcom</code> 名前空間に保管されている Knative Hello World アプリをデプロイします。 </td>
    </tr>
    <tr>
    <td><code>spec.container.env</code></td>
    <td>Knative サービスで使用する環境変数のリスト。 この例では、環境変数 <code>TARGET</code> の値が、サンプル・アプリによって読み取られ、アプリに要求を送信したときに <code>"Hello ${TARGET}!"</code> 形式で返されます。 値がない場合は、サンプル・アプリは <code>"Hello World!"</code> を返します。  </td>
    </tr>
    </tbody>
    </table>

2. クラスターに Knative サービスを作成します。 サービスを作成すると、Knative `Serving` プリミティブによって、アプリ用に、変更不可能のリビジョン、Knative 経路、Ingress ルーティング・ルール、Kubernetes サービス、Kubernetes ポッドおよびロード・バランサーが作成されます。 アプリは、`<knative_service_name>.<namespace>.<ingress_subdomain>` 形式で Ingress サブドメインからのサブドメインを割り当てられ、これはインターネットからのアプリへのアクセスに使用できます。
   ```
   kubectl apply -f service.yaml
   ```
   {: pre}

   出力例:
   ```
   service.serving.knative.dev "kn-helloworld" created
   ```
   {: screen}

3. ポッドが作成されていることを確認します。 ポッドは 2 つのコンテナーで構成されます。 一方のコンテナーは、`Hello World` アプリを実行し、もう一方のコンテナーは、Istio および Knative のモニタリング・ツールとロギング・ツールを実行するサイドカーです。 ポッドには、`00001` リビジョン番号が割り当てられます。
   ```
   kubectl get pods
   ```
   {: pre}

   出力例:
   ```
   NAME                                              READY     STATUS    RESTARTS   AGE
   kn-helloworld-00001-deployment-55db6bf4c5-2vftm   2/2       Running   0          16s
   ```
   {: screen}

4. `Hello World` アプリを試行します。
   1. Knative サービスに割り当てられたデフォルト・ドメインを取得します。 Knative サービスの名前を変更した場合、または別の名前空間にアプリをデプロイした場合は、照会内のこれらの値を更新してください。
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

   2. 前のステップで取得したサブドメインを使用して、アプリに要求を送信します。
      ```
      curl -v <service_domain>
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

5. Knative でポッドがスケール・ダウンされるように、数分待ちます。 Knative は、着信ワークロードを処理するために同時に稼働している必要のあるポッドの数を評価します。 ネットワーク・トラフィックが受信されない場合、Knative は自動的にポッドをスケール・ダウンして、この例に示されているように、ポッドをゼロにすることもできます。

   Knative でポッドがどのようにスケール・アップされるかご覧になりますか? アプリのワークロードを増やすことを試行してください。それには、例えば、[Simple cloud-based load tester](https://loader.io/) などのツールを使用します。
   {: tip}
   ```
   kubectl get pods
   ```
   {: pre}

   `kn-helloworld` ポッドが表示されない場合、Knative によってアプリがスケール・ダウンされてポッドがゼロになっています。

6. Knative サービス・サンプルを更新して、`TARGET` 環境変数に別の値を入力します。

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

7. 変更をサービスに適用します。 構成を変更した場合、Knative は自動的に、新規リビジョンを作成し、新規経路を割り当て、デフォルトで Istio に着信ネットワーク・トラフィックを最新のリビジョンにルーティングするように指示します。
   ```
   kubectl apply -f service.yaml
   ```
   {: pre}

8. アプリに新規要求を送信して、変更が適用されていることを確認します。
   ```
   curl -v <service_domain>
   ```

   出力例:
   ```
   ...
   Hello Mr. Smith!
   ```
   {: screen}

9. 増えたネットワーク・トラフィックのために Knative によって再度ポッドがスケール・アップされたことを確認します。 ポッドには、`00002` リビジョン番号が割り当てられます。 リビジョン番号を使用すると、例えば、2 つのリビジョン間で着信トラフィックを分割するように Istio に指示する場合などに、特定のバージョンのアプリを参照できます。
   ```
   kubectl get pods
   ```

   出力例:
   ```
   NAME                                              READY     STATUS    RESTARTS   AGE
   kn-helloworld-00002-deployment-55db6bf4c5-2vftm   3/3       Running   0          16s
   ```
   {: screen}

10. オプション: Knative サービスをクリーンアップします。
    ```
    kubectl delete -f service.yaml
    ```
    {: pre}

お疲れさまでした。 最初の Knative アプリをクラスターに正常にデプロイして、Knative `Serving` プリミティブのリビジョン機能とスケーリング機能を探索できました。


## 次の作業   
{: #whats-next}

- この [Knative workshop ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/IBM/knative101/tree/master/workshop) を試行して、最初の `Node.js` フィボナッチ・アプリをクラスターにデプロイします。
  - GitHub の Dockerfile からイメージを作成して、そのイメージを自動的に {{site.data.keyword.registrylong_notm}} の名前空間にプッシュするため、Knative `Build` プリミティブの使用法を探索します。  
  - IBM 提供の Ingress サブドメインから、Knative により提供される Istio Ingress ゲートウェイへの、ネットワーク・トラフィックのルーティングをセットアップする方法を学習します。
  - アプリの新規バージョンをロールアウトし、Istio を使用して、各アプリ・バージョンにルーティングされるトラフィック量を制御します。
- [Knative `Eventing` ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/knative/docs/tree/master/eventing/samples) サンプルを探索します。
- Knative について詳しくは、[Knative の資料 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/knative/docs) を参照してください。
