---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-07"

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



# ファイアウォールで必要なポートと IP アドレスを開く
{: #firewall}

{{site.data.keyword.containerlong}} のファイアウォールで特定のポートと IP アドレスを開く必要がある以下の状況について説明します。
{:shortdesc}

* [プロキシーまたはファイアウォール経由の公共のインターネットのエンドポイントへのアクセスが企業ネットワーク・ポリシーによって禁止されているときに、ローカル・システムから `ibmcloud` コマンドおよび`ibmcloud ks` コマンドを実行します](#firewall_bx)。
* [プロキシーまたはファイアウォール経由の公共のインターネットのエンドポイントへのアクセスが企業ネットワーク・ポリシーによって禁止されているときに、ローカル・システムから `kubectl` コマンドを実行します](#firewall_kubectl)。
* [プロキシーまたはファイアウォール経由の公共のインターネットのエンドポイントへのアクセスが企業ネットワーク・ポリシーによって禁止されているときに、ローカル・システムから `calicoctl` コマンドを実行します](#firewall_calicoctl)。
* [ワーカー・ノードに対してファイアウォールがセットアップされているとき、またはファイアウォール設定が IBM Cloud インフラストラクチャー (SoftLayer) アカウント内でカスタマイズされたときに、Kubernetes マスターとワーカー・ノード間の通信を可能にします](#firewall_outbound)。
* [クラスターがプライベート・ネットワーク上のファイアウォールを介してリソースにアクセスすることを許可します](#firewall_private)。
* [Calico ネットワーク・ポリシーがワーカー・ノードの発信をブロックする場合にクラスターがリソースにアクセスすることを許可します](#firewall_calico_egress)。
* [クラスター外から NodePort サービス、ロード・バランサー・サービス、または Ingress へアクセス](#firewall_inbound)します。
* [{{site.data.keyword.Bluemix_notm}} の内外やオンプレミスで実行されていてファイアウォールで保護されているサービスにクラスターがアクセスすることを許可します](#whitelist_workers)。

<br />


## ファイアウォール保護下での `ibmcloud` および `ibmcloud ks` コマンドの実行
{: #firewall_bx}

ローカル・システムからプロキシーまたはファイアウォール経由での公共のエンドポイントへのアクセスが企業ネットワーク・ポリシーによって禁止されている場合、`ibmcloud` コマンドおよび `ibmcloud ks` コマンドを実行するには、{{site.data.keyword.Bluemix_notm}} および {{site.data.keyword.containerlong_notm}} における TCP アクセスを許可する必要があります。
{:shortdesc}

1. ファイアウォールで、`cloud.ibm.com` のポート 443 へのアクセスを許可します。
2. この API エンドポイントから {{site.data.keyword.Bluemix_notm}} にログインして、接続を確認します。
  ```
  ibmcloud login -a https://cloud.ibm.com/
  ```
  {: pre}
3. ファイアウォールで、`containers.cloud.ibm.com` のポート 443 へのアクセスを許可します。
4. 接続を確認します。 アクセスが正しく構成されている場合は、出力に船が表示されます。
   ```
   curl https://containers.cloud.ibm.com/v1/
   ```
   {: pre}

   出力例:
   ```
                                     )___(
                              _______/__/_
                     ___     /===========|   ___
    ____       __   [\\\]___/____________|__[///]   __
    \   \_____[\\]__/___________________________\__[//]___
     \                                                    |
      \                                                  /
   ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ```
   {: screen}

<br />


## ファイアウォール保護下での `kubectl` コマンドの実行
{: #firewall_kubectl}

ローカル・システムからプロキシーまたはファイアウォール経由での公共のエンドポイントへのアクセスが企業ネットワーク・ポリシーによって禁止されている場合、`kubectl` コマンドを実行するには、クラスターにおける TCP アクセスを許可する必要があります。
{:shortdesc}

クラスターの作成時に、サービス・エンドポイント URL のポートが、20000 から 32767 までの範囲からランダムに割り当てられます。 作成される可能性のあるすべてのクラスターを対象に 20000 から 32767 までの範囲のポートを開くか、または特定の既存のクラスターを対象にアクセスを許可するかを選択できます。

始めに、[`ibmcloud ks` コマンドを実行](#firewall_bx)するためのアクセスを許可します。

特定のクラスターを対象にアクセスを許可するには、以下のようにします。

1. {{site.data.keyword.Bluemix_notm}} CLI にログインします。 プロンプトが出されたら、{{site.data.keyword.Bluemix_notm}} 資格情報を入力します。 統合されたアカウントがある場合は、`--sso` オプションを含めます。

   ```
   ibmcloud login [--sso]
   ```
   {: pre}

2. `default` 以外のリソース・グループ内にクラスターがある場合は、そのリソース・グループをターゲットとして設定します。 各クラスターが属するリソース・グループを表示するには、`ibmcloud ks clusters` を実行します。 **注**: リソース・グループに対する[**ビューアー**以上の役割](/docs/containers?topic=containers-users#platform)が必要です。
   ```
   ibmcloud target -g <resource_group_name>
   ```
   {: pre}

4. クラスターの名前を取得します。

   ```
   ibmcloud ks clusters
   ```
   {: pre}

5. クラスターのサービス・エンドポイント URL を取得します。
 * **パブリックのサービス・エンドポイント URL** だけが設定されている場合は、その URL を取得します。 権限を持つクラスター・ユーザーは、パブリック・ネットワークでこのエンドポイントから Kubernetes マスターにアクセスできます。
 * **プライベートのサービス・エンドポイント URL** だけが設定されている場合は、その URL を取得します。 権限を持つクラスター・ユーザーは、プライベート・ネットワークでこのエンドポイントから Kubernetes マスターにアクセスできます。
 * **パブリック・サービス・エンドポイント URL** と**プライベート・サービス・エンドポイント URL** の両方が設定されている場合は、両方の URL を取得します。 権限を持つクラスター・ユーザーは、パブリック・ネットワークのパブリック・エンドポイントからでもプライベート・ネットワークのプライベート・エンドポイントからでも Kubernetes マスターにアクセスできます。

  ```
  ibmcloud ks cluster-get --cluster <cluster_name_or_ID>
  ```
  {: pre}

  出力例:
  ```
  ...
  Public Service Endpoint URL:    https://c3.<region>.containers.cloud.ibm.com:30426
  Private Service Endpoint URL:   https://c3-private.<region>.containers.cloud.ibm.com:31140
  ...
  ```
  {: screen}

6. 前の手順で取得したサービス・エンドポイント URL とポートへのアクセスを許可します。 ファイアウォールが IP ベースの場合は、サービス・エンドポイント URL へのアクセスを許可するときに開く IP アドレスを[この表](#master_ips)で確認できます。

7. 接続を確認します。
  * パブリック・サービス・エンドポイントが有効な場合:
    ```
    curl --insecure <public_service_endpoint_URL>/version
    ```
    {: pre}

    コマンド例:
    ```
    curl --insecure https://c3.<region>.containers.cloud.ibm.com:31142/version
    ```
    {: pre}

    出力例:
    ```
    {
     "major": "1",
     "minor": "7+",
     "gitVersion": "v1.7.4-2+eb9172c211dc41",
     "gitCommit": "eb9172c211dc4108341c0fd5340ee5200f0ec534",
     "gitTreeState": "clean",
     "buildDate": "2017-11-16T08:13:08Z",
     "goVersion": "go1.8.3",
     "compiler": "gc",
     "platform": "linux/amd64"
   }
    ```
    {: screen}
  * プライベート・サービス・エンドポイントが有効な場合にマスターへの接続を確認するには、{{site.data.keyword.Bluemix_notm}} プライベート・ネットワークの中で作業しているか、VPN 接続経由でプライベート・ネットワークに接続している必要があります。[プライベート・ロード・バランサーを介してマスター・エンドポイントを公開](/docs/containers?topic=containers-clusters#access_on_prem)することで、ユーザーが VPN または {{site.data.keyword.BluDirectLink}} の接続を介してマスターにアクセスできるようにする必要があります。
```
    curl --insecure <private_service_endpoint_URL>/version
    ```
    {: pre}

    コマンド例:
    ```
    curl --insecure https://c3-private.<region>.containers.cloud.ibm.com:31142/version
    ```
    {: pre}

    出力例:
    ```
    {
     "major": "1",
     "minor": "7+",
     "gitVersion": "v1.7.4-2+eb9172c211dc41",
     "gitCommit": "eb9172c211dc4108341c0fd5340ee5200f0ec534",
     "gitTreeState": "clean",
     "buildDate": "2017-11-16T08:13:08Z",
     "goVersion": "go1.8.3",
     "compiler": "gc",
     "platform": "linux/amd64"
   }
    ```
    {: screen}

8. オプション: 公開する必要のあるクラスターごとに、上記のステップを繰り返します。

<br />


## ファイアウォール保護下での `calicoctl` コマンドの実行
{: #firewall_calicoctl}

ローカル・システムからプロキシーまたはファイアウォール経由での公共のエンドポイントへのアクセスが企業ネットワーク・ポリシーによって禁止されている場合、`calicoctl` コマンドを実行するには、Calico コマンドにおける TCP アクセスを許可する必要があります。
{:shortdesc}

始めに、[`ibmcloud` コマンド](#firewall_bx)と [`kubectl` コマンド](#firewall_kubectl)を実行するためのアクセスを許可します。

1. [`kubectl` コマンド](#firewall_kubectl)の許可に使用したマスター URL から IP アドレスを取得します。

2. etcd のポートを取得します。

  ```
  kubectl get cm -n kube-system cluster-info -o yaml | grep etcd_host
  ```
  {: pre}

3. マスター URL の IP アドレスと etcd ポート経由の Calico ポリシーにおけるアクセスを許可します。

<br />


## クラスターからパブリック・ファイアウォールを介したインフラストラクチャー・リソースや他のサービスへのアクセスの許可
{: #firewall_outbound}

{{site.data.keyword.containerlong_notm}} 地域、{{site.data.keyword.registrylong_notm}}、{{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM)、{{site.data.keyword.monitoringlong_notm}}、{{site.data.keyword.loganalysislong_notm}}、IBM Cloud インフラストラクチャー (SoftLayer) プライベート IP、永続ボリューム請求の発信など、インフラストラクチャーのリソースとサービスにパブリック・ファイアウォールの内側のクラスターからアクセスできるようにします。
{:shortdesc}

クラスターのセットアップに応じて、パブリックとプライベートの IP アドレスのいずれかまたは両方を使用してサービスにアクセスします。 パブリック・ネットワークとプライベート・ネットワークのどちらについても、ファイアウォールの内側のパブリック VLAN とプライベート VLAN の両方にワーカー・ノードが存在するクラスターの場合は、パブリックとプライベートの両方の IP アドレスの接続を開く必要があります。 ファイアウォールの内側のプライベート VLAN だけにワーカー・ノードが存在するクラスターの場合は、プライベート IP アドレスの接続だけを開きます。
{: note}

1.  クラスター内のすべてのワーカー・ノードのパブリック IP アドレスをメモします。

    ```
    ibmcloud ks workers --cluster <cluster_name_or_ID>
    ```
    {: pre}

2.  ソースの <em>&lt;each_worker_node_publicIP&gt;</em> から、宛先の TCP/UDP ポート (20000 から 32767 までの範囲とポート 443) への発信ネットワーク・トラフィックと、以下の IP アドレスとネットワーク・グループへの発信ネットワーク・トラフィックを許可します。 これらの IP アドレスにより、ワーカー・ノードはクラスター・マスターと通信できます。ローカル・マシンから公共のインターネットのエンドポイントへのアクセスが企業ファイアウォールによって禁止されている場合は、このステップをローカル・マシンに対しても実行して、クラスター・マスターにアクセスできるようにしてください。

    ブートストラッピング・プロセスの際にロードのバランスを取るため、地域内のすべてのゾーンのために、ポート 443 への発信トラフィックを許可する必要があります。 例えば、クラスターが米国南部にある場合、各ワーカー・ノードのパブリック IP からすべてのゾーンの IP アドレスのポート 443 へのトラフィックを許可する必要があります。
    {: important}

    {: #master_ips}
    <table summary="表の 1 行目は両方の列にまたがっています。残りの行は左から右に読みます。1 列目はサーバー・ゾーン、2 列目は対応する IP アドレスです。">
      <caption>発信トラフィック用に開く IP アドレス</caption>
          <thead>
          <th>地域</th>
          <th>ゾーン</th>
          <th>パブリック IP アドレス</th>
          <th>プライベート IP アドレス</th>
          </thead>
        <tbody>
          <tr>
            <td>北アジア太平洋地域</td>
            <td>che01<br>hkg02<br>seo01<br>sng01<br><br>tok02、tok04、tok05</td>
            <td><code>169.38.70.10</code><br><code>169.56.132.234</code><br><code>169.56.69.242</code><br><code>161.202.186.226</code><br><br><code>161.202.126.210、128.168.71.117、165.192.69.69</code></td>
            <td><code>166.9.40.7</code><br><code>166.9.42.7</code><br><code>166.9.44.5</code><br><code>166.9.40.8</code><br><br><code>166.9.40.6、166.9.42.6、166.9.44.4</code></td>
           </tr>
          <tr>
             <td>南アジア太平洋地域</td>
             <td>mel01<br><br>syd01、syd04、syd05</td>
             <td><code>168.1.97.67</code><br><br><code>168.1.8.195、130.198.66.26、168.1.12.98、130.198.64.19</code></td>
             <td><code>166.9.54.10</code><br><br><code>166.9.52.14、166.9.52.15、166.9.54.11、166.9.54.13、166.9.54.12</code></td>
          </tr>
          <tr>
             <td>中欧</td>
             <td>ams03<br>mil01<br>osl01<br>par01<br><br>fra02、fra04、fra05</td>
             <td><code>169.50.169.110, 169.50.154.194</code><br><code>159.122.190.98, 159.122.141.69</code><br><code>169.51.73.50</code><br><code>159.8.86.149、159.8.98.170</code><br><br><code>169.50.56.174、161.156.65.42、149.81.78.114</code></td>
             <td><code>166.9.28.17、166.9.30.11</code><br><code>166.9.28.20、166.9.30.12</code><br><code>166.9.32.8</code><br><code>166.9.28.19、166.9.28.22</code><br><br><code>	166.9.28.23、166.9.30.13、166.9.32.9</code></td>
            </tr>
          <tr>
            <td>英国南部</td>
            <td>lon02、lon04、lon05、lon06</td>
            <td><code>159.122.242.78、158.175.111.42、158.176.94.26、159.122.224.242、158.175.65.170、158.176.95.146</code></td>
            <td><code>166.9.34.5、166.9.34.6、166.9.36.10、166.9.36.11、166.9.36.12、166.9.36.13、166.9.38.6、166.9.38.7</code></td>
          </tr>
          <tr>
            <td>米国東部</td>
             <td>mon01<br>tor01<br><br>wdc04、wdc06、wdc07</td>
             <td><code>169.54.126.219</code><br><code>169.53.167.50</code><br><br><code>169.63.88.186、169.60.73.142、169.61.109.34、169.63.88.178、169.60.101.42、169.61.83.62</code></td>
             <td><code>166.9.20.11</code><br><code>166.9.22.8</code><br><br><code>166.9.20.12、166.9.20.13、166.9.22.9、166.9.22.10、166.9.24.4、166.9.24.5</code></td>
          </tr>
          <tr>
            <td>米国南部</td>
            <td>hou02<br>mex01<br>sao01<br>sjc03<br>sjc04<br><br>dal10、dal12、dal13</td>
            <td><code>184.173.44.62</code><br><code>169.57.100.18</code><br><code>169.57.151.10</code><br><code>169.45.67.210</code><br><code>169.62.82.197</code><br><br><code>169.46.7.238、169.48.230.146、169.61.29.194、169.46.110.218、169.47.70.10、169.62.166.98、169.48.143.218、169.61.177.2、169.60.128.2</code></td>
            <td><code>166.9.15.74</code><br><code>166.9.15.76</code><br><code>166.9.12.143</code><br><code>166.9.12.144</code><br><code>166.9.15.75</code><br><br><code>166.9.12.140、166.9.12.141、166.9.12.142、166.9.15.69、166.9.15.70、166.9.15.72、166.9.15.71、166.9.15.73、166.9.16.183、166.9.16.184、166.9.16.185</code></td>
          </tr>
          </tbody>
        </table>

3.  {: #firewall_registry}ワーカー・ノードが {{site.data.keyword.registrylong_notm}} と通信できるようにするには、それらのワーカー・ノードから [{{site.data.keyword.registrylong_notm}} 地域](/docs/services/Registry?topic=registry-registry_overview#registry_regions)への発信ネットワーク・トラフィックを許可します。
  - `TCP port 443, port 4443 FROM <each_worker_node_publicIP> TO <registry_subnet>`
  -  <em>&lt;registry_subnet&gt;</em> は、トラフィックを許可するレジストリー・サブネットに置き換えてください。 グローバル・レジストリーには IBM 提供のパブリック・イメージが保管され、地域レジストリーにはユーザー独自のプライベートまたはパブリック・イメージが保管されます。 ポート 4443 は、公証人などの機能 ([イメージの署名の確認](/docs/services/Registry?topic=registry-registry_trustedcontent#registry_trustedcontent)など) のために必須です。 <table summary="表の 1 行目は両方の列にまたがっています。残りの行は左から右に読みます。1 列目はサーバー・ゾーン、2 列目は対応する IP アドレスです。">
  <caption>レジストリー・トラフィック用に開く IP アドレス</caption>
    <thead>
      <th>{{site.data.keyword.containerlong_notm}} 地域</th>
      <th>レジストリー・アドレス</th>
      <th>レジストリーのパブリック・サブネット</th>
      <th>レジストリーのプライベート IP アドレス</th>
    </thead>
    <tbody>
      <tr>
        <td>{{site.data.keyword.containerlong_notm}} 地域にまたがる<br>グローバル・レジストリー</td>
        <td><code>icr.io</code><br><br>
        非推奨: <code>registry.bluemix.net</code></td>
        <td><code>169.60.72.144/28</code></br><code>169.61.76.176/28</code></br><code>169.62.37.240/29</code></br><code>169.60.98.80/29</code></br><code>169.63.104.232/29</code></td>
        <td><code>166.9.20.4</code></br><code>166.9.22.3</code></br><code>166.9.24.2</code></td>
      </tr>
      <tr>
        <td>北アジア太平洋地域</td>
        <td><code>jp.icr.io</code><br><br>
        非推奨: <code>registry.au-syd.bluemix.net</code></td>
        <td><code>161.202.146.86/29</code></br><code>128.168.71.70/29</code></br><code>165.192.71.222/29</code></td>
        <td><code>166.9.40.3</code></br><code>166.9.42.3</code></br><code>166.9.44.3</code></td>
      </tr>
      <tr>
        <td>南アジア太平洋地域</td>
        <td><code>au.icr.io</code><br><br>
        非推奨: <code>registry.au-syd.bluemix.net</code></td>
        <td><code>168.1.45.160/27</code></br><code>168.1.139.32/27</code></br><code>168.1.1.240/29</code></br><code>130.198.88.128/29</code></br><code>135.90.66.48/29</code></td>
        <td><code>166.9.52.2</code></br><code>166.9.54.2</code></br><code>166.9.56.3</code></td>
      </tr>
      <tr>
        <td>中欧</td>
        <td><code>de.icr.io</code><br><br>
        非推奨: <code>registry.eu-de.bluemix.net</code></td>
        <td><code>169.50.56.144/28</code></br><code>159.8.73.80/28</code></br><code>169.50.58.104/29</code></br><code>161.156.93.16/29</code></br><code>149.81.79.152/29</code></td>
        <td><code>166.9.28.12</code></br><code>166.9.30.9</code></br><code>166.9.32.5</code></td>
       </tr>
       <tr>
        <td>英国南部</td>
        <td><code>uk.icr.io</code><br><br>
        非推奨: <code>registry.eu-gb.bluemix.net</code></td>
        <td><code>159.8.188.160/27</code></br><code>169.50.153.64/27</code></br><code>158.175.97.184/29</code></br><code>158.176.105.64/29</code></br><code>141.125.71.136/29</code></td>
        <td><code>166.9.36.9</code></br><code>166.9.38.5</code></br><code>166.9.34.4</code></td>
       </tr>
       <tr>
        <td>米国東部、米国南部</td>
        <td><code>us.icr.io</code><br><br>
        非推奨: <code>registry.ng.bluemix.net</code></td>
        <td><code>169.55.39.112/28</code></br><code>169.46.9.0/27</code></br><code>169.55.211.0/27</code></br><code>169.61.234.224/29</code></br><code>169.61.135.160/29</code></br><code>169.61.46.80/29</code></td>
        <td><code>166.9.12.114</code></br><code>166.9.15.50</code></br><code>166.9.16.173</code></td>
       </tr>
      </tbody>
    </table>

4. オプション: ワーカー・ノードから {{site.data.keyword.monitoringlong_notm}}、{{site.data.keyword.loganalysislong_notm}}、Sysdig、および LogDNA サービスへの発信ネットワーク・トラフィックを許可します。
    *   **{{site.data.keyword.monitoringlong_notm}}**:
        <pre class="screen">TCP port 443, port 9095 FROM &lt;each_worker_node_public_IP&gt; TO &lt;monitoring_subnet&gt;</pre>
        <em>&lt;monitoring_subnet&gt;</em> は、トラフィックの宛先として許可するモニタリング地域のサブネットに置き換えてください。
        <p><table summary="表の 1 行目は両方の列にまたがっています。残りの行は左から右に読みます。1 列目はサーバー・ゾーン、2 列目は対応する IP アドレスです。">
  <caption>モニター・トラフィック用に開く IP アドレス</caption>
        <thead>
        <th>{{site.data.keyword.containerlong_notm}} 地域</th>
        <th>モニタリング・アドレス</th>
        <th>モニタリング・サブネット</th>
        </thead>
      <tbody>
        <tr>
         <td>中欧</td>
         <td><code>metrics.eu-de.bluemix.net</code></td>
         <td><code>158.177.65.80/30</code></td>
        </tr>
        <tr>
         <td>英国南部</td>
         <td><code>metrics.eu-gb.bluemix.net</code></td>
         <td><code>169.50.196.136/29</code></td>
        </tr>
        <tr>
          <td>米国東部、米国南部、北アジア太平洋地域、南アジア太平洋地域</td>
          <td><code>metrics.ng.bluemix.net</code></td>
          <td><code>169.47.204.128/29</code></td>
         </tr>
         
        </tbody>
      </table>
</p>
    *   **{{site.data.keyword.mon_full_notm}}**:
        <pre class="screen">TCP port 443, port 6443 FROM &lt;each_worker_node_public_IP&gt; TO &lt;sysdig_public_IP&gt;</pre>
        `<sysdig_public_IP>` は [Sysdig IP アドレス](/docs/services/Monitoring-with-Sysdig?topic=Sysdig-network#network)に置き換えてください。
    *   **{{site.data.keyword.loganalysislong_notm}}**:
        <pre class="screen">TCP port 443, port 9091 FROM &lt;each_worker_node_public_IP&gt; TO &lt;logging_public_IP&gt;</pre>
        <em>&lt;logging_public_IP&gt;</em> は、トラフィックを許可するロギング地域のすべてのアドレスに置き換えます。
        <p><table summary="表の 1 行目は両方の列にまたがっています。残りの行は左から右に読みます。1 列目はサーバー・ゾーン、2 列目は対応する IP アドレスです。">
<caption>ロギング・トラフィック用に開く IP アドレス</caption>
        <thead>
        <th>{{site.data.keyword.containerlong_notm}} 地域</th>
        <th>ロギング・アドレス</th>
        <th>ロギング IP アドレス</th>
        </thead>
        <tbody>
          <tr>
            <td>米国東部、米国南部</td>
            <td><code>ingest.logging.ng.bluemix.net</code></td>
            <td><code>169.48.79.236</code><br><code>169.46.186.113</code></td>
          </tr>
          <tr>
           <td>英国南部</td>
           <td><code>ingest.logging.eu-gb.bluemix.net</code></td>
           <td><code>169.50.115.113</code></td>
          </tr>
          <tr>
           <td>中欧</td>
           <td><code>ingest-eu-fra.logging.bluemix.net</code></td>
           <td><code>158.177.88.43</code><br><code>159.122.87.107</code></td>
          </tr>
          <tr>
           <td>南アジア太平洋地域、北アジア太平洋地域</td>
           <td><code>ingest-au-syd.logging.bluemix.net</code></td>
           <td><code>130.198.76.125</code><br><code>168.1.209.20</code></td>
          </tr>
         </tbody>
       </table>
</p>
    *   **{{site.data.keyword.la_full_notm}}**:
        <pre class="screen">TCP port 443, port 80 FROM &lt;each_worker_node_public_IP&gt; TO &lt;logDNA_public_IP&gt;</pre>
        `<logDNA_public_IP>` は [LogDNA IP アドレス](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-network#network)に置き換えます。

5. ロード・バランサー・サービスを使用している場合は、VRRP プロトコルを使用するすべてのトラフィックが、パブリック・インターフェースおよびプライベート・インターフェースでのワーカー・ノード間で許可されることを確認します。 {{site.data.keyword.containerlong_notm}} では、パブリック・ロード・バランサーおよびプライベート・ロード・バランサーの IP アドレスを管理するために VRRP プロトコルが使用されます。

6. {: #pvc}プライベート・クラスターで永続ボリュームの請求を作成するには、そのクラスターが以下のバージョンの Kubernetes または {{site.data.keyword.Bluemix_notm}} ストレージ・プラグインを使用してセットアップされたものであることを確認します。 これらのバージョンは、クラスターから永続ストレージ・インスタンスへのプライベート・ネットワークによる通信が可能です。
    <table>
    <caption>プライベート・クラスターに必要な Kubernetes または {{site.data.keyword.Bluemix_notm}} ストレージ・プラグインのバージョンの概要</caption>
    <thead>
      <th>ストレージのタイプ</th>
      <th>必要なバージョン</th>
   </thead>
   <tbody>
     <tr>
       <td>ファイル・ストレージ</td>
       <td>Kubernetes バージョン <code>1.13.4_1512</code>、<code>1.12.6_1544</code>、<code>1.11.8_1550</code>、<code>1.10.13_1551</code>、またはこれ以降</td>
     </tr>
     <tr>
       <td>ブロック・ストレージ</td>
       <td>{{site.data.keyword.Bluemix_notm}} ブロック・ストレージ・プラグイン・バージョン 1.3.0 以降</td>
     </tr>
     <tr>
       <td>オブジェクト・ストレージ</td>
       <td><ul><li>{{site.data.keyword.cos_full_notm}} プラグイン・バージョン 1.0.3 以降</li><li>HMAC 認証がセットアップされた {{site.data.keyword.cos_full_notm}} サービス</li></ul></td>
     </tr>
   </tbody>
   </table>

   プライベート・ネットワークによるネットワーク通信ができないバージョンの Kubernetes または {{site.data.keyword.Bluemix_notm}} ストレージ・プラグインを使用しなければならない場合や、HMAC 認証機能を持たない {{site.data.keyword.cos_full_notm}} を使用したい場合は、ファイアウォールで IBM Cloud インフラストラクチャー (SoftLayer) と {{site.data.keyword.Bluemix_notm}} ID/アクセス管理への発信アクセスを許可してください。
   - TCP ポート 443 のすべての発信ネットワーク・トラフィックを許可します。
   - [**フロントエンド (パブリック) ネットワーク**](/docs/infrastructure/hardware-firewall-dedicated?topic=hardware-firewall-dedicated-ibm-cloud-ip-ranges#frontend-public-network)と[**バックエンド (プライベート) ネットワーク**](/docs/infrastructure/hardware-firewall-dedicated?topic=hardware-firewall-dedicated-ibm-cloud-ip-ranges#backend-private-network)の両方について、クラスターがあるゾーンの IBM Cloud インフラストラクチャー (SoftLayer) IP 範囲へのアクセスを許可します。 クラスターのゾーンを確認するには、`ibmcloud ks clusters` を実行します。


<br />


## クラスターからのプライベート・ファイアウォールを介したリソースへのアクセスの許可
{: #firewall_private}

プライベート・ネットワーク上のファイアウォールがある場合、ワーカー・ノード間の通信を許可し、クラスターがプライベート・ネットワークを介してインフラストラクチャー・リソースにアクセスできるようにします。
{:shortdesc}

1. ワーカー・ノード間のすべてのトラフィックを許可します。
    1. パブリック・インターフェースとプライベート・インターフェース上のワーカー・ノード間の TCP、UDP、VRRP、IPEncap トラフィックをすべて許可します。 {{site.data.keyword.containerlong_notm}} では、プライベート・ロード・バランサーの IP アドレスを管理するために VRRP プロトコルが使用され、サブネットをまたぐポッド間トラフィックを許可するために IPEncap プロトコルが使用されます。
    2. Calico ポリシーを使用している場合や、複数ゾーン・クラスターの各ゾーンにファイアウォールがある場合は、ファイアウォールによってワーカー・ノード間の通信がブロックされることがあります。 ワーカーのポート、ワーカーのプライベート IP アドレス、または Calico ワーカー・ノード・ラベルを使用して、クラスター内のすべてのワーカー・ノードを相互に開く必要があります。

2. IBM Cloud インフラストラクチャー (SoftLayer) のプライベート IP の範囲を許可して、クラスター内にワーカー・ノードを作成できるようにします。
    1. IBM Cloud インフラストラクチャー (SoftLayer) のプライベート IP のために適切な範囲を許可します。 [Backend (private) Network](/docs/infrastructure/hardware-firewall-dedicated?topic=hardware-firewall-dedicated-ibm-cloud-ip-ranges#backend-private-network) を参照してください。
    2. 使用しているすべての[ゾーン](/docs/containers?topic=containers-regions-and-zones#zones)のために IBM Cloud インフラストラクチャー (SoftLayer) のプライベート IP の範囲を許可します。 `dal01`、`dal10`、`wdc04` の各ゾーンの IP を追加する必要があるとともに、ご使用のクラスターがヨーロッパ地域にある場合は、`ams01` ゾーンの IP も追加する必要があります。[Service Network (on backend/private network)](/docs/infrastructure/hardware-firewall-dedicated?topic=hardware-firewall-dedicated-ibm-cloud-ip-ranges#service-network-on-backend-private-network-) を参照してください。

3. 以下のポートを開きます。
    - ワーカー・ノードの更新および再ロードを許可するために、ワーカーからポート 80 および 443 へのアウトバウンド TCP および UDP 接続を許可します。
    - ボリュームとしてファイル・ストレージのマウントを許可するために、ポート 2049 へのアウトバウンド TCP および UDP を許可します。
    - ブロック・ストレージに対する通信用に、ポート 3260 へのアウトバウンド TCP と UDP を許可します。
    - Kubernetes ダッシュボードおよび `kubectl logs` や `kubectl exec` などのコマンドのために、ポート 10250 へのインバウンド TCP および UDP 接続を許可します。
    - DNS アクセスのために、TCP および UDP のポート 53 へのインバウンドおよびアウトバウンドの接続を許可します。

4. パブリック・ネットワークにもファイアウォールがある場合、またはプライベート VLAN 専用クラスターがあり、ファイアウォールとしてゲートウェイ・デバイスを使用している場合、[クラスターからインフラストラクチャー・リソースや他のサービスへのアクセスの許可](#firewall_outbound)で指定した IP およびポートも許可する必要があります。

<br />


## クラスターからの Calico 発信ポリシーを介したリソースへのアクセスの許可
{: #firewall_calico_egress}

すべてのパブリック・ワーカーの発信を制限するために [Calico ネットワーク・ポリシー](/docs/containers?topic=containers-network_policies)を使用してファイアウォールとして機能するようにしている場合、ワーカーがマスター API サーバーと etcd のローカル・プロキシーにアクセスすることを許可する必要があります。
{: shortdesc}

1. [アカウントにログインします。 該当する場合は、適切なリソース・グループをターゲットにします。 クラスターのコンテキストを設定します。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure) `ibmcloud ks cluster-config` コマンドに `--admin` オプションと `--network` オプションを指定します。 `--admin` は、インフラストラクチャー・ポートフォリオにアクセスし、ワーカー・ノードで Calico コマンドを実行するためのキーをダウンロードします。 `--network` は、すべての Calico コマンドを実行するための Calico 構成ファイルをダウンロードします。
  ```
  ibmcloud ks cluster-config --cluster <cluster_name_or_ID> --admin --network
  ```
  {: pre}

2. クラスターから API サーバー・ローカル・プロキシーの 172.20.0.1:2040 および 172.21.0.1:443 と etcd ローカル・プロキシーの 172.20.0.1:2041 へのパブリック・トラフィックを許可する Calico ネットワーク・ポリシーを作成します。
  ```
  apiVersion: projectcalico.org/v3
  kind: GlobalNetworkPolicy
  metadata:
    name: allow-master-local
  spec:
    egress:
    - action: Allow
      destination:
        ports:
        - 2040:2041
        nets:
        - 172.20.0.1/32
        protocol: UDP
    - action: Allow
      destination:
        ports:
        - 2040:2041
        nets:
        - 172.20.0.1/32
        protocol: TCP
    - action: Allow
      destination:
        ports:
        - 443
        nets:
        - 172.21.0.1/32
        protocol: UDP
    - action: Allow
      destination:
        ports:
        - 443
        nets:
        - 172.21.0.1/32
        protocol: TCP
    order: 1500
    selector: ibm.role == 'worker_public'
    types:
    - Egress
  ```
  {: codeblock}

3. ポリシーをクラスターに適用します。
    - Linux および OS X:

      ```
      calicoctl apply -f allow-master-local.yaml
      ```
      {: pre}

    - Windows:

      ```
      calicoctl apply -f filepath/allow-master-local.yaml --config=filepath/calicoctl.cfg
      ```
      {: pre}

## クラスター外から NodePort、ロード・バランサー、Ingress の各サービスへのアクセス
{: #firewall_inbound}

NodePort、ロード・バランサー、Ingress の各サービスへの着信アクセスを許可できます。
{:shortdesc}

<dl>
  <dt>NodePort サービス</dt>
  <dd>トラフィックを許可するすべてのワーカー・ノードのパブリック IP アドレスへのサービスのデプロイ時に構成したポートを開きます。 ポートを見つけるには、`kubectl get svc` を実行します。 ポートの範囲は 20000 から 32000 までです。<dd>
  <dt>ロード・バランサー・サービス</dt>
  <dd>ロード・バランサー・サービスのパブリック IP アドレスへのサービスのデプロイ時に構成したポートを開きます。</dd>
  <dt>Ingress</dt>
  <dd>Ingress アプリケーション・ロード・バランサーの IP アドレスへのポート 80 (HTTP の場合) またはポート 443 (HTTPS の場合) を開きます。</dd>
</dl>

<br />


## 他のサービスのファイアウォールやオンプレミスのファイアウォールのホワイトリストへのクラスターの登録
{: #whitelist_workers}

{{site.data.keyword.Bluemix_notm}} の内外やオンプレミスで実行されているサービスがファイアウォールで保護されている場合は、そうしたサービスにアクセスするには、そのファイアウォールにワーカー・ノードの IP アドレスを追加して、クラスターへのアウトバウンド・ネットワーク・トラフィックを許可します。 例えば、ファイアウォールで保護されている {{site.data.keyword.Bluemix_notm}} データベースからデータを読み取る必要がある場合は、オンプレミスのファイアウォールのホワイトリストにワーカー・ノードのサブネットを登録して、クラスターからのネットワーク・トラフィックを許可します。
{:shortdesc}

1.  [アカウントにログインします。 該当する場合は、適切なリソース・グループをターゲットにします。 クラスターのコンテキストを設定します。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

2. ワーカー・ノードのサブネットやワーカー・ノードの IP アドレスを取得します。
  * **ワーカー・ノードのサブネット**: クラスター内のワーカー・ノード数が頻繁に変わる場合に ([クラスターの自動スケーリング機能](/docs/containers?topic=containers-ca#ca)を有効にする場合など)、新規ワーカー・ノードの追加時にその都度ファイアウォールを更新するというのは望ましくありません。 その場合は、クラスターで使用している VLAN サブネットをホワイトリストに登録できます。 ただし、VLAN サブネットは、他のクラスターのワーカー・ノードと共有されている可能性があることを忘れないでください。
    <p class="note">{{site.data.keyword.containerlong_notm}} がクラスターのためにプロビジョンする**プライマリー・パブリック・サブネット**には 14 個の使用可能な IP アドレスが含まれおり、このサブネットは、同じ VLAN にある他のクラスターと共有される可能性があります。 ワーカー・ノード数が 14 を超える場合は、他のサブネットを注文することになるので、ホワイトリストに登録する必要のあるサブネットが変わる可能性が出てきます。 変更の頻度を下げるには、CPU とメモリー・リソースの高いワーカー・ノード・フレーバーのワーカー・プールを作成してください。そうすれば、ワーカー・ノードをそれほど頻繁に追加する必要がなくなります。</p>
    1. クラスター内のワーカー・ノードをリストします。
      ```
      ibmcloud ks workers --cluster <cluster_name_or_ID>
      ```
      {: pre}

    2. 前のステップの出力から、クラスター内のワーカー・ノードの**パブリック IP** の固有ネットワーク ID (最初の 3 つのオクテット) をすべてメモしてください。        プライベート専用クラスターをホワイトリストに登録する場合は、代わりに**プライベート IP** をメモしてください。以下の出力では、固有ネットワーク ID は `169.xx.178` と `169.xx.210` です。
        ```
        ID                                                 Public IP        Private IP     Machine Type        State    Status   Zone    Version   
        kube-dal10-crb2f60e9735254ac8b20b9c1e38b649a5-w31   169.xx.178.101   10.xxx.xx.xxx   b3c.4x16.encrypted   normal   Ready    dal10   1.13.6   
        kube-dal10-crb2f60e9735254ac8b20b9c1e38b649a5-w34   169.xx.178.102   10.xxx.xx.xxx   b3c.4x16.encrypted   normal   Ready    dal10   1.13.6  
        kube-dal12-crb2f60e9735254ac8b20b9c1e38b649a5-w32   169.xx.210.101   10.xxx.xx.xxx   b3c.4x16.encrypted   normal   Ready    dal12   1.13.6   
        kube-dal12-crb2f60e9735254ac8b20b9c1e38b649a5-w33   169.xx.210.102   10.xxx.xx.xxx   b3c.4x16.encrypted   normal   Ready    dal12   1.13.6  
        ```
        {: screen}
    3.  それぞれの固有ネットワーク ID の VLAN サブネットをリストします。
        ```
        ibmcloud sl subnet list | grep -e <networkID1> -e <networkID2>
        ```
        {: pre}

        出力例:
        ```
        ID        identifier       type                 network_space   datacenter   vlan_id   IPs   hardware   virtual_servers
        1234567   169.xx.210.xxx   ADDITIONAL_PRIMARY   PUBLIC          dal12        1122334   16    0          5   
        7654321   169.xx.178.xxx   ADDITIONAL_PRIMARY   PUBLIC          dal10        4332211   16    0          6    
        ```
        {: screen}
    4.  サブネット・アドレスを取得します。 出力中の **IP** の数を確認します。 次に、IP の数と等しくなるのは `2` の `n` 乗かを計算します。 例えば、IP の数が `16` であれば、`2` の `4` (`n`) 乗が `16` に等しくなります。 `32` ビットから `n` の値を減算すれば、サブネットの CIDR が求まります。 例えば、`n` が `4` の場合、CIDR は `28` になります (式 `32 - 4 = 28`)。 **identifier** のマスクと CIDR の値を組み合わせれば、完全なサブネット・アドレスになります。 前の出力では、サブネット・アドレスは次になります。
        *   `169.xx.210.xxx/28`
        *   `169.xx.178.xxx/28`
  * **個々のワーカー・ノードの IP アドレス**: 1 つのアプリだけを実行する少数のワーカー・ノードしかなく、スケーリングの必要がない場合や、1 つのワーカー・ノードだけをホワイトリストに登録する場合は、クラスター内のすべてのワーカー・ノードを表示して、**パブリック IP** アドレスをメモします。 ワーカー・ノードをプライベート・ネットワークだけに接続し、プライベート・サービス・エンドポイントを使用して {{site.data.keyword.Bluemix_notm}} サービスに接続する場合は、**プライベート IP** アドレスをメモします。 それらのワーカー・ノードだけをホワイトリストに登録します。 クラスターのワーカー・ノードを削除/追加する場合は、その都度ファイアウォールを更新する必要があります。
    ```
    ibmcloud ks workers --cluster <cluster_name_or_ID>
    ```
    {: pre}
4.  アウトバウンド・トラフィックについてはサービスのファイアウォールに、インバウンド・トラフィックについてはオンプレミスのファイアウォールに、サブネット CIDR または IP アドレスを追加します。
5.  ホワイトリストに登録するクラスターごとにこの手順を繰り返します。
