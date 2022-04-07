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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}


# クラスターのネットワーキングのトラブルシューティング
{: #cs_troubleshoot_network}

{{site.data.keyword.containerlong}} を使用する際は、ここに示すクラスターのネットワーキングのトラブルシューティング手法を検討してください。
{: shortdesc}

Ingress を介したアプリへの接続に問題が発生していますか? [Ingress のデバッグ](/docs/containers?topic=containers-cs_troubleshoot_debug_ingress)をお試しください。
{: tip}

トラブルシューティングの際に、[{{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool](/docs/containers?topic=containers-cs_troubleshoot#debug_utility) を使用して、テストを実行し、クラスターからネットワーキング、Ingress、および strongSwan の関連情報を収集することができます。
{: tip}

## ネットワーク・ロード・バランサー (NLB) サービス経由でアプリに接続できない
{: #cs_loadbalancer_fails}

{: tsSymptoms}
クラスター内に NLB サービスを作成して、アプリをパブリックに公開しました。 NLB のパブリック IP アドレスを使用してアプリに接続しようとしたところ、接続が失敗したか、タイムアウトになりました。

{: tsCauses}
次のいずれかの理由で、NLB サービスが正しく機能していない可能性があります。

-   クラスターが、フリー・クラスターであるか、またはワーカー・ノードが 1 つしかない標準クラスターです。
-   クラスターがまだ完全にデプロイされていません。
-   NLB サービスの構成スクリプトにエラーが含まれています。

{: tsResolve}
NLB サービスのトラブルシューティングを行うには、以下のようにします。

1.  標準クラスターをセットアップしたこと、クラスターが完全にデプロイされていること、また、NLB サービスの高可用性を確保するためにクラスターに 2 つ以上のワーカー・ノードがあることを確認します。

  ```
  ibmcloud ks workers --cluster <cluster_name_or_ID>
  ```
  {: pre}

    CLI 出力で、ワーカー・ノードの **Status** に **Ready** と表示され、**Machine Type** に **free** 以外のマシン・タイプが表示されていることを確認します。

2. バージョン 2.0 NLB の場合: [NLB 2.0 の前提条件](/docs/containers?topic=containers-loadbalancer#ipvs_provision)を満たしていることを確認します。

3. NLB サービスの構成ファイルが正しいことを確認します。
    * バージョン 2.0 NLB:
        ```
        apiVersion: v1
        kind: Service
        metadata:
          name: myservice
          annotations:
            service.kubernetes.io/ibm-load-balancer-cloud-provider-enable-features: "ipvs"
        spec:
          type: LoadBalancer
          selector:
            <selector_key>:<selector_value>
          ports:
           - protocol: TCP
           port: 8080
        externalTrafficPolicy: Local
        ```
        {: screen}

        1. サービスのタイプとして **LoadBalancer** を定義したことを確認します。
        2. `service.kubernetes.io/ibm-load-balancer-cloud-provider-enable-features: "ipvs"` アノテーションを指定したことを確認します。
        3. LoadBalancer サービスの `spec.selector` セクションの `<selector_key>` と `<selector_value>` が、デプロイメント YAML の `spec.template.metadata.labels` セクションで使用したキーと値のペアと同じであることを確認します。 ラベルが一致しない場合、LoadBalancer サービスの**「エンドポイント」**セクションに **<none>** と表示され、インターネットからアプリにアクセスできません。
        4. アプリで listen している **port** を使用していることを確認します。
        5. `externalTrafficPolicy` を `Local` に設定していることを確認します。

    * バージョン 1.0 NLB:
        ```
        apiVersion: v1
    kind: Service
    metadata:
      name: myservice
    spec:
      type: LoadBalancer
      selector:
        <selector_key>:<selector_value>
      ports:
           - protocol: TCP
         port: 8080
        ```
        {: screen}

        1. サービスのタイプとして **LoadBalancer** を定義したことを確認します。
        2. LoadBalancer サービスの `spec.selector` セクションの `<selector_key>` と `<selector_value>` が、デプロイメント YAML の `spec.template.metadata.labels` セクションで使用したキーと値のペアと同じであることを確認します。 ラベルが一致しない場合、LoadBalancer サービスの**「エンドポイント」**セクションに **<none>** と表示され、インターネットからアプリにアクセスできません。
        3. アプリで listen している **port** を使用していることを確認します。

3.  NLB サービスを確認し、**Events** セクションを参照して、エラーがないか探します。

    ```
    kubectl describe service <myservice>
    ```
    {: pre}

    次のようなエラー・メッセージを探します。

    <ul><li><pre class="screen"><code>Clusters with one node must use services of type NodePort</code></pre></br>NLB サービスを使用するには、2 つ以上のワーカー・ノードがある標準クラスターでなければなりません。</li>
    <li><pre class="screen"><code>No cloud provider IPs are available to fulfill the NLB service request. Add a portable subnet to the cluster and try again</code></pre></br>このエラー・メッセージは、NLB サービスに割り振れるポータブル・パブリック IP アドレスが残っていないことを示しています。 クラスター用にポータブル・パブリック IP アドレスを要求する方法については、<a href="/docs/containers?topic=containers-subnets#subnets">クラスターへのサブネットの追加</a>を参照してください。 クラスターにポータブル・パブリック IP アドレスを使用できるようになると、NLB サービスが自動的に作成されます。</li>
    <li><pre class="screen"><code>Requested cloud provider IP <cloud-provider-ip> is not available. The following cloud provider IPs are available: <available-cloud-provider-ips></code></pre></br>**`loadBalancerIP`** セクションを使用してロード・バランサー YAML のポータブル・パブリック IP アドレスを定義しましたが、そのポータブル・パブリック IP アドレスはポータブル・パブリック・サブネットに含まれていません。 構成スクリプトの **`loadBalancerIP`** セクションで、既存の IP アドレスを削除し、使用可能なポータブル・パブリック IP アドレスの 1 つを追加します。 スクリプトから **`loadBalancerIP`** セクションを削除して、使用可能なポータブル・パブリック IP アドレスが自動的に割り振られるようにすることもできます。</li>
    <li><pre class="screen"><code>No available nodes for NLB services</code></pre>ワーカー・ノードが不足しているため、NLB サービスをデプロイできません。 複数のワーカー・ノードを持つ標準クラスターをデプロイしましたが、ワーカー・ノードのプロビジョンが失敗した可能性があります。</li>
    <ol><li>使用可能なワーカー・ノードのリストを表示します。</br><pre class="pre"><code>kubectl get nodes</code></pre></li>
    <li>使用可能なワーカー・ノードが 2 つ以上ある場合は、ワーカー・ノードの詳細情報をリストします。</br><pre class="pre"><code>ibmcloud ks worker-get --cluster &lt;cluster_name_or_ID&gt; --worker &lt;worker_ID&gt;</code></pre></li>
    <li><code>kubectl get nodes</code> コマンドと <code>ibmcloud ks worker-get</code> コマンドから返されたワーカー・ノードのパブリック VLAN ID とプライベート VLAN ID が一致していることを確認します。</li></ol></li></ul>

4.  カスタム・ドメインを使用して NLB サービスに接続している場合は、カスタム・ドメインが NLB サービスのパブリック IP アドレスにマップされていることを確認します。
    1.  NLB サービスのパブリック IP アドレスを見つけます。
        ```
        kubectl describe service <service_name> | grep "LoadBalancer Ingress"
        ```
        {: pre}

    2.  カスタム・ドメインが、ポインター・レコード (PTR) で NLB サービスのポータブル・パブリック IP アドレスにマップされていることを確認します。

<br />


## Ingress 経由でアプリに接続できない
{: #cs_ingress_fails}

{: tsSymptoms}
クラスターでアプリ用の Ingress リソースを作成して、アプリをパブリックに公開しました。 Ingress アプリケーション・ロード・バランサー (ALB) のパブリック IP アドレスまたはサブドメインを使用してアプリに接続しようとしたところ、接続に失敗したか、タイムアウトになりました。

{: tsResolve}
まず、クラスターが完全にデプロイされており、ゾーンごとに 2 つ以上のワーカー・ノードが使用可能で、ALB の高可用性が確保されていることを確認します。
```
ibmcloud ks workers --cluster <cluster_name_or_ID>
```
{: pre}

CLI 出力で、ワーカー・ノードの **Status** に **Ready** と表示され、**Machine Type** に **free** 以外のマシン・タイプが表示されていることを確認します。

* 標準クラスターが完全にデプロイされており、ゾーンごとに 2 つ以上のワーカー・ノードがあるが、使用可能な **Ingress サブドメイン**がない場合は、[Ingress ALB のサブドメインを取得できない](/docs/containers?topic=containers-cs_troubleshoot_network#cs_subnet_limit)を参照してください。
* その他の問題については、[Ingress のデバッグ](/docs/containers?topic=containers-cs_troubleshoot_debug_ingress)のステップに従って、Ingress のセットアップをトラブルシューティングします。

<br />


## Ingress アプリケーション・ロード・バランサー (ALB) のシークレットの問題
{: #cs_albsecret_fails}

{: tsSymptoms}
`ibmcloud ks alb-cert-deploy` コマンドを使用して Ingress アプリケーション・ロード・バランサー (ALB) のシークレットをクラスターにデプロイした後で、{{site.data.keyword.cloudcerts_full_notm}} 内の証明書を表示したところ、`Description` フィールドがシークレット名で更新されていません。

ALB シークレットに関する情報をリストすると、状況は `*_failed` となっています。 例えば、`create_failed`、`update_failed`、`delete_failed` などです。

{: tsResolve}
以下に示す、ALB シークレットが失敗する理由と対応するトラブルシューティング手順を確認してください。

<table>
<caption>Ingress アプリケーション・ロード・バランサーのシークレットのトラブルシューティング</caption>
 <thead>
 <th>現象の理由</th>
 <th>修正方法</th>
 </thead>
 <tbody>
 <tr>
 <td>証明書データのダウンロードやアップデートに必要なアクセス役割を持っていない。</td>
 <td>アカウント管理者に問い合わせて、以下の {{site.data.keyword.Bluemix_notm}} IAM 役割を割り当てるように依頼してください。<ul><li>{{site.data.keyword.cloudcerts_full_notm}} インスタンスに対する**管理者**と**ライター**のサービス役割。 詳しくは、{{site.data.keyword.cloudcerts_short}} の <a href="/docs/services/certificate-manager?topic=certificate-manager-managing-service-access-roles#managing-service-access-roles">サービス・アクセスの管理</a>を参照してください。</li><li>クラスターに対する<a href="/docs/containers?topic=containers-users#platform">**管理者**のプラットフォーム役割</a>。</li></ul></td>
 </tr>
 <tr>
 <td>作成時、更新時、または削除時に提供された証明書 CRN が、クラスターと同じアカウントに属していない。</td>
 <td>提供した証明書 CRN が、クラスターと同じアカウントにデプロイされた、{{site.data.keyword.cloudcerts_short}} サービスのインスタンスにインポートされていることを確認します。</td>
 </tr>
 <tr>
 <td>作成時に提供された証明書 CRN が正しくない。</td>
 <td><ol><li>提供した証明書 CRN ストリングが正確であることを確認します。</li><li>証明書 CRN が正確であることがわかった場合は、<code>ibmcloud ks alb-cert-deploy --update --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code> を実行して、シークレットの更新を試行します。</li><li>このコマンドの結果が <code>update_failed</code> 状況になる場合は、<code>ibmcloud ks alb-cert-rm --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt;</code> を実行して、シークレットを削除します。</li><li><code>ibmcloud ks alb-cert-deploy --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code> を実行して、シークレットを再デプロイします。</li></ol></td>
 </tr>
 <tr>
 <td>更新時に提供された証明書 CRN が正しくない。</td>
 <td><ol><li>提供した証明書 CRN ストリングが正確であることを確認します。</li><li>証明書 CRN が正確であることを確認した場合は、<code>ibmcloud ks alb-cert-rm --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt;</code> を実行して、シークレットを削除します。</li><li><code>ibmcloud ks alb-cert-deploy --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code> を実行して、シークレットを再デプロイします。</li><li><code>ibmcloud ks alb-cert-deploy --update --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code> を実行して、シークレットの更新を試行します。</li></ol></td>
 </tr>
 <tr>
 <td>{{site.data.keyword.cloudcerts_long_notm}} サービスがダウンしている。</td>
 <td>{{site.data.keyword.cloudcerts_short}} サービスが稼働中であることを確認します。</td>
 </tr>
 <tr>
 <td>インポートされたシークレットの名前が、IBM 提供の Ingress シークレットと同じである。</td>
 <td>シークレットの名前を変更します。 IBM 提供の Ingress シークレットの名前は、`ibmcloud ks cluster-get --cluster <cluster_name_or_ID> | grep Ingress` を実行すると確認できます。</td>
 </tr>
 </tbody></table>

<br />


## Ingress ALB のサブドメインを取得できない、ALB がゾーンにデプロイされない、またはロード・バランサーをデプロイできない
{: #cs_subnet_limit}

{: tsSymptoms}
* Ingress サブドメインなし: `ibmcloud ks cluster-get --cluster <cluster>` を実行すると、クラスターが `normal` 状態であるのに、使用可能な **Ingress サブドメイン**がありません。
* ALB がゾーンにデプロイされない: 複数ゾーン・クラスターがある場合、`ibmcloud ks albs --cluster <cluster>` を実行しても、ALB はゾーンにデプロイされません。例えば、3 つのゾーンにワーカー・ノードがある場合、パブリック ALB が 3 番目のゾーンにデプロイされなかった次のような出力が表示される場合があります。
  ```
  ALB ID                                            Enabled    Status     Type      ALB IP           Zone    Build                          ALB VLAN ID
  private-cr96039a75fddb4ad1a09ced6699c88888-alb1   false      disabled   private   -                dal10   ingress:411/ingress-auth:315   2294021
  private-cr96039a75fddb4ad1a09ced6699c88888-alb2   false      disabled   private   -                dal12   ingress:411/ingress-auth:315   2234947
  private-cr96039a75fddb4ad1a09ced6699c88888-alb3   false      disabled   private   -                dal13   ingress:411/ingress-auth:315   2234943
  public-cr96039a75fddb4ad1a09ced6699c88888-alb1    true       enabled    public    169.xx.xxx.xxx   dal10   ingress:411/ingress-auth:315   2294019
  public-cr96039a75fddb4ad1a09ced6699c88888-alb2    true       enabled    public    169.xx.xxx.xxx   dal12   ingress:411/ingress-auth:315   2234945
  ```
  {: screen}
* ロード・バランサーをデプロイできない: `ibm-cloud-provider-vlan-ip-config` 構成マップを記述すると、以下の出力例のようなエラー・メッセージが表示される場合があります。
  ```
  kubectl get cm ibm-cloud-provider-vlan-ip-config
  ```
  {: pre}
  ```
  Warning  CreatingLoadBalancerFailed ... ErrorSubnetLimitReached: There are already the maximum number of subnets permitted in this VLAN.
  ```
  {: screen}

{: tsCauses}
標準クラスターでは、あるゾーンで初めてクラスターを作成したときに、そのゾーン内のパブリック VLAN とプライベート VLAN が IBM Cloud インフラストラクチャー (SoftLayer) アカウントで自動的にプロビジョンされます。 そのゾーンでは、指定したパブリック VLAN 上に 1 つのパブリック・ポータブル・サブネットが要求され、指定したプライベート VLAN 上に 1 つのプライベート・ポータブル・サブネットが要求されます。 {{site.data.keyword.containerlong_notm}} の場合、VLAN のサブネット数の上限は 40 個です。 ゾーン内のクラスターの VLAN が既にその上限に達している場合、**Ingress サブドメイン**のプロビジョンが失敗するか、そのゾーン用のパブリック Ingress ALB のプロビジョンが失敗するか、ポータブル・パブリック IP アドレスをネットワーク・ロード・バランサー (NLB) の作成に使用できない可能性があります。

VLAN のサブネット数を表示するには、以下のようにします。
1.  [IBM Cloud インフラストラクチャー (SoftLayer) コンソール](https://cloud.ibm.com/classic?)から、**「ネットワーク」**>**「IP 管理 (IP Management)」**>**「VLAN」**の順に選択します。
2.  クラスターの作成に使用した VLAN の **「VLAN の数 (VLAN Number)」**をクリックします。 **「サブネット」**セクションで、サブネットが 40 個以上存在するかどうかを確認します。

{: tsResolve}
新規 VLAN が必要な場合、[{{site.data.keyword.Bluemix_notm}} サポートに連絡して](/docs/infrastructure/vlans?topic=vlans-ordering-premium-vlans#ordering-premium-vlans)注文してください。 その後、その新規 VLAN を使用する[クラスターを作成します](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_create)。

使用可能な別の VLAN がある場合は、既存のクラスターで [VLAN スパンニングをセットアップ](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning)できます。 その後、使用可能なサブネットがある他の VLAN を使用する新規ワーカー・ノードをクラスターに追加できます。 VLAN スパンニングが既に有効になっているかどうかを確認するには、`ibmcloud ks vlan-spanning-get<region>` [コマンド](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get)を使用します。

VLAN に使用していないサブネットがある場合、クラスターに追加することで、VLAN のサブネットを再利用できます。
1. 使用したいサブネットが使用可能であることを確認します。
  <p class="note">使用しているインフラストラクチャー・アカウントが、複数の {{site.data.keyword.Bluemix_notm}} アカウントで共有されている場合があります。 この場合、**バインドされたクラスター**があるサブネットを表示するために `ibmcloud ks subnets` コマンドを実行しても、自分のクラスターの情報のみが表示されます。サブネットが使用可能であり、他のアカウントやチームで使用されていないことをインフラストラクチャー・アカウント所有者に確認してください。</p>

2. [`ibmcloud ks cluster-subnet-add` コマンド](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_subnet_add)を使用して、クラスターで既存のサブネットを使用できるようにします。

3. クラスターにサブネットが正常に作成されて追加されたことを確認します。 サブネットの CIDR は **Subnet VLANs** セクションにリストされます。
    ```
    ibmcloud ks cluster-get --showResources <cluster_name_or_ID>
    ```
    {: pre}

    次の例の出力では、2 番目のサブネットがパブリック VLAN の `2234945` に追加されています。
    ```
    Subnet VLANs
    VLAN ID   Subnet CIDR          Public   User-managed
    2234947   10.xxx.xx.xxx/29     false    false
    2234945   169.xx.xxx.xxx/29    true     false
    2234945   169.xx.xxx.xxx/29    true     false
    ```
    {: screen}

4. 追加したサブネットからのポータブル IP アドレスが、クラスター内の ALB またはロード・バランサーに使用されていることを確認します。新しく追加されたサブネットからのポータブル IP アドレスがサービスで使用されるまでに数分かかる場合があります。
  * Ingress サブドメインなし: `ibmcloud ks cluster-get --cluster <cluster>` を実行して、**Ingress サブドメイン**にデータが設定されていることを確認します。
  * ALB がゾーンにデプロイされない: `ibmcloud ks albs --cluster <cluster>` を実行して、欠落している ALB がデプロイされていることを確認します。
  * ロード・バランサーをデプロイできない: `kubectl get svc -n kube-system` を実行して、ロード・バランサーに **EXTERNAL-IP** があることを確認します。

<br />


## WebSocket を介した接続が 60 秒後に閉じる
{: #cs_ingress_websocket}

{: tsSymptoms}
Ingress サービスによって、WebSocket を使用するアプリが公開されます。 ただし、クライアントと WebSocket アプリとの間の接続は、60 秒間送信されるトラフィックがない場合は閉じられます。

{: tsCauses}
以下のいずれかの理由により、WebSocket アプリへの接続が非アクティブで 60 秒間経過した後に除去されることがあります。

* インターネット接続に、長時間の接続を許容しないプロキシーまたはファイアウォールがある。
* WebSocket アプリに対する ALB でのタイムアウトによって、接続が終了する。

{: tsResolve}
非アクティブで 60 秒間経過した後も接続が閉じられないようにするには、以下のようにします。

1. プロキシーまたはファイアウォールを介して WebSocket アプリに接続する場合は、プロキシーまたはファイアウォールが長時間の接続を自動的に終了するように構成されていないことを確認します。

2. 接続を活動状態で保つには、タイムアウトの値を増やすか、またはアプリでハートビートをセットアップします。
<dl><dt>タイムアウトの変更</dt>
<dd>ALB 構成の `proxy-read-timeout` の値を増やします。 例えば、`60s` から `300s` などのより大きい値にタイムアウトを変更するには、`ingress.bluemix.net/proxy-read-timeout: "serviceName=<service_name> timeout=300s"` の [アノテーション](/docs/containers?topic=containers-ingress_annotation#connection)を Ingress リソース・ファイルに追加します。 クラスター内のすべてのパブリック ALB のタイムアウトが変更されます。</dd>
<dt>ハートビートのセットアップ</dt>
<dd>ALB のデフォルトの読み取りタイムアウト値を変更しない場合は、WebSocket アプリでハートビートをセットアップします。 [WAMP ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://wamp-proto.org/) のようなフレームワークを使用してハートビート・プロトコルをセットアップすると、アプリのアップストリーム・サーバーは定期的に「ping」メッセージを送信し、クライアントは「pong」メッセージで応答します。 60 秒間のタイムアウトが適用される前に、「ping/pong」トラフィックによって接続が開いた状態に保たれるように、ハートビート間隔を 58 秒以下に設定します。</dd></dl>

<br />


## テイント適用ノードの使用時にソース IP 保持が失敗する
{: #cs_source_ip_fails}

{: tsSymptoms}
サービス構成ファイルの `externalTrafficPolicy` を `Local` に変更して、[バージョン 1.0 ロード・バランサー](/docs/containers?topic=containers-loadbalancer#node_affinity_tolerations)・サービスまたは [Ingress ALB](/docs/containers?topic=containers-ingress#preserve_source_ip) サービスのソース IP 保持を有効にしました。 しかし、トラフィックがアプリのバックエンド・サービスに到達しません。

{: tsCauses}
ロード・バランサー・サービスまたは Ingress ALB サービスのソース IP 保持を有効にすると、クライアント要求のソース IP アドレスは保持されます。 これらのサービスは、要求パケットの IP アドレスが変更されないようにするために、同じワーカー・ノード上の各アプリ・ポッドにのみトラフィックを転送します。 通常、ロード・バランサーまたは Ingress ALB のサービス・ポッドは、各アプリ・ポッドがデプロイされたワーカー・ノードと同じワーカー・ノードにデプロイされます。 しかし、次のように、サービス・ポッドとアプリ・ポッドが同じワーカー・ノードにスケジュールされない状況もあります。 ワーカー・ノードで [Kubernetes テイント ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/) を使用している場合、テイントの耐障害性がないポッドはすべて、テイント適用ワーカー・ノードでの実行が阻止されます。 ソース IP 保持が、次に示すように、使用しているテイントのタイプに基づいて動作していない可能性があります。

* **エッジ・ノードのテイント**: クラスター内の各パブリック VLAN 上の複数のワーカー・ノードに [`dedicated=edge` ラベルを追加](/docs/containers?topic=containers-edge#edge_nodes)して、Ingress ポッドとロード・バランサー・ポッドがそれらのワーカー・ノードにのみデプロイされるようにしました。 その後、[それらのエッジ・ノードにテイントも適用](/docs/containers?topic=containers-edge#edge_workloads)して、他のすべてのワークロードがエッジ・ノードで実行されないようにしました。 しかし、アプリ・デプロイメントにエッジ・ノードのアフィニティー・ルールと耐障害性を追加しませんでした。 アプリ・ポッドは、サービス・ポッドと同じテイント適用ノードにスケジュールすることはできず、トラフィックはアプリのバックエンド・サービスに到達しません。

* **カスタム・テイント**: カスタム・テイントをいくつかのノード上で使用して、そのテイントの耐障害性があるアプリ・ポッドだけをそれらのノードにデプロイできるようにしました。 アプリのデプロイメントと、ロード・バランサー・サービスまたは Ingress サービスにアフィニティー・ルールと耐障害性を追加したので、それらのポッドはそれらのノードにのみデプロイされます。 しかし、`ibm-system` 名前空間に自動的に作成される `ibm-cloud-provider-ip` `keepalived` ポッドがあるので、ロード・バランサー・ポッドとアプリ・ポッドは必ず同じワーカー・ノードにスケジュールされます。 これらの `keepalived` ポッドには、使用したカスタムのテイントに対する耐障害性がありません。 これらのポッドをアプリ・ポッドが実行されているノードと同じテイント適用ノードにスケジュールすることはできず、トラフィックはアプリのバックエンド・サービスに到達しません。

{: tsResolve}
以下のいずれかのオプションを選択して、この問題を解決します。

* **エッジ・ノードのテイント**: ロード・バランサー・ポッドとアプリ・ポッドをテイント適用エッジ・ノードに確実にデプロイするには、[アプリ・デプロイメントにエッジ・ノードのアフィニティー・ルールと耐障害性を追加します](/docs/containers?topic=containers-loadbalancer#lb_edge_nodes)。 ロード・バランサー・ポッドと Ingress ALB ポッドには、デフォルトでこれらのアフィニティー・ルールと耐障害性があります。

* **カスタム・テイント**: `keepalived` ポッドが耐障害性を保持していないカスタム・テイントを削除します。 代わりに、[ワーカー・ノードにエッジ・ノードのラベルを付けてから、それらのエッジ・ノードにテイントを適用する](/docs/containers?topic=containers-edge)ことができます。

上記オプションのいずれかを実行しても `keepalived` ポッドがまだスケジュールされない場合、`keepalived` ポッドに関する詳細情報を取得することができます。

1. `keepalived` ポッドを取得します。
    ```
    kubectl get pods -n ibm-system
    ```
    {: pre}

2. 出力で、**Status** が `Pending` になっている `ibm-cloud-provider-ip` ポッドを探します。 例:
    ```
    ibm-cloud-provider-ip-169-61-XX-XX-55967b5b8c-7zv9t     0/1       Pending   0          2m        <none>          <none>
    ibm-cloud-provider-ip-169-61-XX-XX-55967b5b8c-8ptvg     0/1       Pending   0          2m        <none>          <none>
    ```
    {:screen}

3. それぞれの `keepalived` ポッドに対して describe を実行して、**Events** セクションを探します。 リストされたエラー・メッセージまたは警告メッセージに対処します。
    ```
    kubectl describe pod ibm-cloud-provider-ip-169-61-XX-XX-55967b5b8c-7zv9t -n ibm-system
    ```
    {: pre}

<br />


## strongSwan Helm チャートとの VPN 接続を確立できない
{: #cs_vpn_fails}

{: tsSymptoms}
`kubectl exec  $STRONGSWAN_POD -- ipsec status` を実行して VPN 接続を確認すると、`ESTABLISHED` 状況が表示されない、または、VPN ポッドが `ERROR` 状態になっている、または、クラッシュと再始動が繰り返されます。

{: tsCauses}
Helm チャートの構成ファイルに誤った値があるか、値が欠落しているか、構文エラーがあります。

{: tsResolve}
strongSwan Helm チャートとの VPN 接続を確立しようとすると、初回は VPN の状況が `ESTABLISHED` にならない可能性があります。 いくつかのタイプの問題を確認し、それに応じて構成ファイルを変更する必要がある場合があります。 strongSwan VPN 接続をトラブルシューティングするには、以下のようにします。

1. strongSwan チャートの定義に含まれている 5 つの Helm テストを実行して、[strongSwan VPN 接続のテストと検証](/docs/containers?topic=containers-vpn#vpn_test)を行います。

2. Helm テストの実行後に VPN 接続を確立できない場合は、VPN ポッド・イメージ内にパッケージされている VPN デバッグ・ツールを実行できます。

    1. `STRONGSWAN_POD` 環境変数を設定します。

        ```
        export STRONGSWAN_POD=$(kubectl get pod -l app=strongswan,release=vpn -o jsonpath='{ .items[0].metadata.name }')
        ```
        {: pre}

    2. デバッグ・ツールを実行します。

        ```
        kubectl exec  $STRONGSWAN_POD -- vpnDebug
        ```
        {: pre}

        このツールは、一般的なネットワーキングの問題に関するさまざまなテストを実行するため、複数の情報ページを出力します。 先頭が `ERROR`、`WARNING`、`VERIFY`、または `CHECK` の出力行に、VPN 接続エラーが示されている可能性があります。

    <br />


## 新しい strongSwan Helm チャートのリリースをインストールできない
{: #cs_strongswan_release}

{: tsSymptoms}
strongSwan Helm チャートを変更し、`helm install -f config.yaml --name=vpn ibm/strongswan` を実行して、新しいリリースをインストールしようとします。 しかし、次のエラーが表示されます。
```
Error: release vpn failed: deployments.extensions "vpn-strongswan" already exists
```
{: screen}

{: tsCauses}
このエラーは、前のリリースの strongSwan チャートが完全にはアンインストールされなかったことを示しています。

{: tsResolve}

1. 前のチャートのリリースを削除します。
    ```
    helm delete --purge vpn
    ```
    {: pre}

2. 前のリリースのデプロイメントを削除します。 デプロイメントおよび関連付けられているポッドの削除には、最大で 1 分かかります。
    ```
    kubectl delete deploy vpn-strongswan
    ```
    {: pre}

3. デプロイメントが削除されたことを確認します。 デプロイメントの `vpn-strongswan` がリストに表示されません。
    ```
    kubectl get deployments
    ```
    {: pre}

4. 更新された strongSwan Helm チャートを、新しいリリース名で再インストールします。
    ```
    helm install -f config.yaml --name=vpn ibm/strongswan
    ```
    {: pre}

<br />


## ワーカー・ノードの追加または削除後に strongSwan VPN 接続が失敗する
{: #cs_vpn_fails_worker_add}

{: tsSymptoms}
以前は strongSwan IPSec VPN サービスを使用して、正常に機能する VPN 接続を確立できていました。 しかし、クラスターでワーカー・ノードを追加または削除した後に、以下の症状が 1 つ以上発生します。

* VPN 状況が `ESTABLISHED` にならない
* オンプレミス・ネットワークから新しいワーカー・ノードにアクセスできない
* 新しいワーカー・ノード上で実行されているポッドからリモート・ネットワークにアクセスできない

{: tsCauses}
ワーカー・ノードをワーカー・プールに追加した場合:

* 既存の `localSubnetNAT` 設定または `local.subnet` 設定によって VPN 接続を介して公開されていない、新しいプライベート・サブネットに、ワーカー・ノードがプロビジョンされました。
* ワーカー・ノードのテイントまたはラベルが既存の `tolerations` または `nodeSelector` 設定に含まれていないため、VPN 経路をワーカー・ノードに追加できません。
* 新しいワーカー・ノードで VPN ポッドが実行されていますが、そのワーカー・ノードのパブリック IP アドレスがオンプレミス・ファイアウォールを通過することを許可されていません。

ワーカー・ノードを削除した場合:

* 既存の `tolerations` 設定または `nodeSelector` 設定による特定のテイントまたはラベルに対する制限のため、削除されたワーカー・ノードが、VPN ポッドを実行する唯一のノードでした。

{: tsResolve}
以下のように、ワーカー・ノードの変更を反映するように Helm チャートの値を更新します。

1. 既存の Helm chart を削除します。

    ```
    helm delete --purge <release_name>
    ```
    {: pre}

2. strongSwan VPN サービスの構成ファイルを開きます。

    ```
    helm inspect values ibm/strongswan > config.yaml
    ```
    {: pre}

3. 以下の設定を確認し、必要に応じて、削除または追加したワーカー・ノードを反映するように設定を変更します。

    ワーカー・ノードを追加した場合:

    <table>
    <caption>ワーカー・ノードの設定</caption?
     <thead>
     <th>設定</th>
     <th>説明</th>
     </thead>
     <tbody>
     <tr>
     <td><code>localSubnetNAT</code></td>
     <td>追加したワーカーは、他のワーカー・ノードがある既存のサブネットとは違う新しいプライベート・サブネット上にデプロイされる可能性があります。 サブネット NAT を使用してクラスターのプライベート・ローカル IP アドレスを再マップしており、ワーカーが新しいサブネットに追加された場合は、この設定に新しいサブネット CIDR を追加してください。</td>
     </tr>
     <tr>
     <td><code>nodeSelector</code></td>
     <td>以前に VPN ポッドのデプロイメントを特定のラベルのワーカーに制限していた場合は、追加したワーカー・ノードにもそのラベルがあることを確認してください。</td>
     </tr>
     <tr>
     <td><code>tolerations</code></td>
     <td>追加したワーカー・ノードにテイントがある場合は、テイントがあるすべてのワーカーまたは特定のテイントがあるすべてのワーカーで VPN ポッドを実行できるように、この設定を変更してください。</td>
     </tr>
     <tr>
     <td><code>local.subnet</code></td>
     <td>追加したワーカーは、他のワーカーがある既存のサブネットとは異なる、新しいプライベート・サブネット上にデプロイされる可能性があります。 プライベート・ネットワークの NodePort または LoadBalancer サービスでアプリが公開されており、追加したワーカー上にそれらのアプリがある場合は、この設定に新しいサブネットの CIDR を追加してください。 **注**: `local.subnet` に値を追加する場合は、オンプレミス・サブネットの VPN 設定を確認して、それらの設定も更新する必要があるかどうか判断してください。</td>
     </tr>
     </tbody></table>

    ワーカー・ノードを削除した場合:

    <table>
    <caption>ワーカー・ノードの設定</caption>
     <thead>
     <th>設定</th>
     <th>説明</th>
     </thead>
     <tbody>
     <tr>
     <td><code>localSubnetNAT</code></td>
     <td>サブネット NAT を使用して特定のプライベート・ローカル IP アドレスを再マップしている場合は、その設定から古いワーカーの IP アドレスを削除してください。 サブネット NAT を使用してサブネット全体を再マップし、サブネット上にワーカーが残っていない場合は、そのサブネット CIDR をこの設定から削除してください。</td>
     </tr>
     <tr>
     <td><code>nodeSelector</code></td>
     <td>以前に VPN ポッドのデプロイメントを単一のワーカーに制限していて、そのワーカーが削除された場合は、他のワーカーで VPN ポッドを実行できるようにこの設定を変更してください。</td>
     </tr>
     <tr>
     <td><code>tolerations</code></td>
     <td>削除したワーカーにはテイントがなく、残りのワーカーにだけテイントがある場合は、テイントがあるワーカーまたは特定のテイントがあるワーカーで VPN ポッドを実行できるように、この設定を変更してください。
     </td>
     </tr>
     </tbody></table>

4. 更新した値を使用して新しい Helm チャートをインストールします。

    ```
    helm install -f config.yaml --name=<release_name> ibm/strongswan
    ```
    {: pre}

5. Chart のデプロイメント状況を確認します。 Chart の準備ができている場合は、出力の先頭付近の **STATUS** フィールドに `DEPLOYED` の値があります。

    ```
    helm status <release_name>
    ```
    {: pre}

6. 時により、VPN 構成ファイルに加えた変更と一致するように、オンプレミス設定とファイアウォール設定を変更する必要がある場合があります。

7. VPN を開始します。
    * クラスターから VPN 接続を開始する (`ipsec.auto` を `start` に設定した) 場合は、オンプレミス・ゲートウェイの VPN を開始してからクラスターの VPN を開始します。
    * オンプレミス・ゲートウェイから VPN 接続を開始する (`ipsec.auto` を `auto` に設定した) 場合は、クラスターの VPN を開始してからオンプレミス・ゲートウェイの VPN を開始します。

8. `STRONGSWAN_POD` 環境変数を設定します。

    ```
    export STRONGSWAN_POD=$(kubectl get pod -l app=strongswan,release=<release_name> -o jsonpath='{ .items[0].metadata.name }')
    ```
    {: pre}

9. VPN の状況を確認します。

    ```
    kubectl exec $STRONGSWAN_POD -- ipsec status
    ```
    {: pre}

    * VPN 接続の状況が `ESTABLISHED` の場合、VPN 接続は正常に行われました。 これ以上のアクションは不要です。

    * 接続の問題が解決されない場合は、[strongSwan Helm チャートとの VPN 接続を確立できない](#cs_vpn_fails)を参照して、VPN 接続のトラブルシューティングをさらに行います。

<br />



## Calico ネットワーク・ポリシーを取得できない
{: #cs_calico_fails}

{: tsSymptoms}
`calicoctl get policy` を実行してクラスター内の Calico ネットワーク・ポリシーを表示しようとすると、以下のいずれかの予期しない結果またはエラー・メッセージが表示されます。
- 空のリスト
- v3 ポリシーではなく、古い Calico v2 ポリシーのリスト
- `Failed to create Calico API client: syntax error in calicoctl.cfg: invalid config file: unknown APIVersion 'projectcalico.org/v3'`

`calicoctl get GlobalNetworkPolicy` を実行してクラスター内の Calico ネットワーク・ポリシーを表示しようとすると、以下のいずれかの予期しない結果またはエラー・メッセージが表示されます。
- 空のリスト
- `Failed to create Calico API client: syntax error in calicoctl.cfg: invalid config file: unknown APIVersion 'v1'`
- `Failed to create Calico API client: syntax error in calicoctl.cfg: invalid config file: unknown APIVersion 'projectcalico.org/v3'`
- `Failed to get resources: Resource type 'GlobalNetworkPolicy' is not supported`

{: tsCauses}
Calico ポリシーを使用するには、クラスターの Kubernetes バージョン、Calico CLI バージョン、Calico 構成ファイル構文、およびポリシーの表示コマンドの 4 つの要因すべてが正しい組み合わせになっている必要があります。 これらの要因のうち 1 つ以上が、正しいバージョンになっていません。

{: tsResolve}
v3.3 以降の Calico CLI、`calicoctl.cfg` v3 構成ファイル構文、および `calicoctl get GlobalNetworkPolicy` コマンドと `calicoctl get NetworkPolicy` コマンドを使用する必要があります。

すべての Calico 要因が正しい組み合わせになっていることを確認するには、以下のようにします。

1. [バージョン 3.3 以降の Calico CLI をインストールして構成します](/docs/containers?topic=containers-network_policies#cli_install)。
2. 作成してクラスターに適用するすべてのポリシーで、[Calico v3 構文 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.projectcalico.org/v3.3/reference/calicoctl/resources/networkpolicy) が使用されていることを確認します。 既存のポリシー `.yaml` または `.json` ファイルで Calico v2 構文が使用されている場合は、[`calicoctl convert` コマンド ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.projectcalico.org/v3.3/reference/calicoctl/commands/convert) を使用して、これらを Calico v3 構文に変換できます。
3. [ポリシーを表示](/docs/containers?topic=containers-network_policies#view_policies)するには、必ず `calicoctl get GlobalNetworkPolicy` (グローバル・ポリシーの場合) および `calicoctl get NetworkPolicy --namespace <policy_namespace>` (特定の名前空間にスコープ設定されたポリシーの場合) を使用します。

<br />


## 無効な VLAN ID が原因でワーカー・ノードを追加できない
{: #suspended}

{: tsSymptoms}
ご使用の {{site.data.keyword.Bluemix_notm}} アカウントが一時停止されたか、クラスター内のすべてのワーカー・ノードが削除されました。 アカウントが再アクティブ化された後、ワーカー・プールのサイズ変更または再バランス化を試みた場合、ワーカー・ノードを追加することはできません。 次のようなエラー・メッセージが表示されます。

```
SoftLayerAPIError(SoftLayer_Exception_Public): Could not obtain network VLAN with id #123456.
```
{: screen}

{: tsCauses}
アカウントが一時停止されると、アカウント内のワーカー・ノードは削除されます。 クラスターにワーカー・ノードがない場合、IBM Cloud インフラストラクチャー (SoftLayer) は、関連付けられたパブリック VLAN とプライベート VLAN を再利用します。 ただし、クラスターのワーカー・プールのメタデータには前の VLAN ID があり、プールの再バランス化またはサイズ変更時にこれらの使用不可の ID が使用されます。 各 VLAN は既にこのクラスターに関連付けられていないため、ノードを作成できません。

{: tsResolve}

[既存のワーカー・プールを削除](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_rm)してから、[新しいワーカー・プールを作成](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_create)できます。

また、新しい VLAN をオーダーし、これらを使用してプール内に新しいワーカー・ノードを作成することによって、既存のワーカー・プールを保持することもできます。

開始前に、以下のことを行います。 [アカウントにログインします。 該当する場合は、適切なリソース・グループをターゲットにします。 クラスターのコンテキストを設定します。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  新しい VLAN ID を必要とする対象のゾーンを取得するには、次のコマンドの出力の **Location** をメモします。 **注**: クラスターが複数ゾーンの場合、ゾーンごとに VLAN ID が必要です。

    ```
    ibmcloud ks clusters
    ```
    {: pre}

2.  [{{site.data.keyword.Bluemix_notm}} サポートに連絡する](/docs/infrastructure/vlans?topic=vlans-ordering-premium-vlans#ordering-premium-vlans)ことによって、クラスターが含まれているゾーンごとに新しいプライベート VLAN とパブリック VLAN を取得します。

3.  ゾーンごとの新しいプライベート VLAN ID と新しいパブリック VLAN ID をメモします。

4.  ワーカー・プールの名前をメモします。

    ```
    ibmcloud ks worker-pools --cluster <cluster_name_or_ID>
    ```
    {: pre}

5.  `zone-network-set` [コマンド](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_zone_network_set)を使用して、ワーカー・プールのネットワーク・メタデータを変更します。

    ```
    ibmcloud ks zone-network-set --zone <zone> --cluster <cluster_name_or_ID> -- worker-pools <worker-pool> --private-vlan <private_vlan_ID> --public-vlan <public_vlan_ID>
    ```
    {: pre}

6.  **複数ゾーン・クラスターのみ**: クラスター内のゾーンごとに、**ステップ 5** を繰り返します。

7.  ワーカー・プールの再バランス化またはサイズ変更を行って、新しい VLAN ID を使用するワーカー・ノードを追加します。 以下に例を示します。

    ```
    ibmcloud ks worker-pool-resize --cluster <cluster_name_or_ID> --worker-pool <worker_pool> --size-per-zone <number_of_workers_per_zone>
    ```
    {: pre}

8.  ワーカー・ノードが作成されたことを確認します。

    ```
    ibmcloud ks workers --cluster <cluster_name_or_ID> --worker-pool <worker_pool>
    ```
    {: pre}

<br />



## ヘルプとサポートの取得
{: #network_getting_help}

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

