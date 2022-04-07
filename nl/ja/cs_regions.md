---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-06"

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

# ロケーション
{: #regions-and-zones}

{{site.data.keyword.containerlong}} クラスターは、世界規模でデプロイできます。 Kubernetes クラスターを作成すると、そのリソースは、クラスターをデプロイしたロケーションに残ります。 グローバル API エンドポイントを介して {{site.data.keyword.containerlong_notm}} にアクセスし、クラスターを操作できます。
{:shortdesc}

![{{site.data.keyword.containerlong_notm}} ロケーション](images/locations.png)

_{{site.data.keyword.containerlong_notm}} locations_

[地域に固有のエンドポイント](#bluemix_regions)を介してアクセスされた地域に編成するために使用される {{site.data.keyword.Bluemix_notm}} リソース。代わりに[グローバル・エンドポイント](#endpoint)を使用してください。
{: deprecated}

## {{site.data.keyword.containerlong_notm}} locations
{: #locations}

{{site.data.keyword.Bluemix_notm}} リソースは、地理的ロケーションの階層に編成されます。 {{site.data.keyword.containerlong_notm}} は、世界規模の 6 つすべての複数ゾーン対応地域を含む、これらのロケーションのサブセットで使用できます。 フリー・クラスターは、選択したロケーションでのみ使用できます。 その他の {{site.data.keyword.Bluemix_notm}} サービスはグローバルで使用できる場合もあれば、特定のロケーション内で使用できる場合もあります。
{: shortdesc}

### 使用可能なロケーション
{: #available-locations}

使用可能な {{site.data.keyword.containerlong_notm}} ロケーションをリストするには、`ibmcloud ks supported-locations` コマンドを使用します。
{: shortdesc}

次の図は、{{site.data.keyword.containerlong_notm}} ロケーションの編成方法を説明するための例として使用されています。

![{{site.data.keyword.containerlong_notm}} ロケーションの組織](images/cs_regions_hierarchy.png)

<table summary="この表は、{{site.data.keyword.containerlong_notm}} ロケーションの組織を示しています。行は左から右に読み、1 列目はロケーション・タイプ、2 列目はタイプの例、3 列目は説明です。">
<caption>{{site.data.keyword.containerlong_notm}} ロケーションの組織。</caption>
  <thead>
  <th>タイプ</th>
  <th>例</th>
  <th>説明</th>
  </thead>
  <tbody>
    <tr>
      <td>地理</td>
      <td>北アメリカ (`na`)</td>
      <td>地理的大陸に基づく組織のグループ化。</td>
    </tr>
    <tr>
      <td>国</td>
      <td>カナダ (`ca`)</td>
      <td>地理内でのロケーションの国。</td>
    </tr>
    <tr>
      <td>大都市</td>
      <td>メキシコ・シティー (`mex-cty`)、ダラス (`dal`)</td>
      <td>1 つ以上のデータ・センター (ゾーン) が配置されている都市の名前。 大都市は複数ゾーンに対応可能であり、複数ゾーン対応のデータ・センターが存在する場合も (ダラスなど)、単一ゾーンのデータ・センターのみが存在する場合も (メキシコ・シティーなど) あります。 複数ゾーン対応の大都市でクラスターを作成する場合、高可用性を実現するために、Kubernetes マスターおよびワーカー・ノードをゾーン間に分散できます。</td>
    </tr>
    <tr>
      <td>データ・センター (ゾーン)</td>
      <td>ダラス 12 (`dal12`)</td>
      <td>クラウドのサービスとアプリケーションをホストする、コンピュート、ネットワーク、ストレージのインフラストラクチャー、および関連冷却機器と電源機器の物理的なロケーション。 クラスターは、高可用性を実現するために、複数ゾーン・アーキテクチャーでデータ・センターまたはゾーン間に分散できます。 単一障害点を共有しないようにゾーンは互いに独立しています。</td>
    </tr>
  </tbody>
  </table>

### {{site.data.keyword.containerlong_notm}} での単一ゾーンおよび複数ゾーンのロケーション
{: #zones}

以下の表に、{{site.data.keyword.containerlong_notm}} で使用可能な単一ゾーンと複数ゾーンのロケーションを示します。 特定の大都市では、クラスターを単一ゾーンまたは複数ゾーンのクラスターとしてプロビジョンできることに注意してください。 また、フリー・クラスターは、1 つのワーカー・ノードを含む単一ゾーン・クラスターとしてのみ、選択した地域でのみ使用できます。
{: shortdesc}

* **複数ゾーン**: 複数ゾーンの大都市ロケーションにクラスターを作成した場合は、可用性の高い Kubernetes マスターのレプリカが、ゾーン間に自動的に分散されます。 1 つのゾーンの障害からアプリを保護するために、ワーカー・ノードを複数のゾーンに分散させるという選択肢があります。
* **単一ゾーン**: 単一データ・センターのロケーションにクラスターを作成する場合は、複数のワーカー・ノードを作成できますが、それらをゾーン間に分散することはできません。 可用性の高いマスターには、別々のホストに 3 つのレプリカを持ちますが、複数のゾーンには分散されません。

ゾーンが複数ゾーン対応かどうかを素早く判別するには、`ibmcloud ks supported-locations` を実行し、`Multizone Metro` 列の値を探します。
{: tip}


[地域に固有のエンドポイント](#bluemix_regions)を介してアクセスされた地域に編成するために使用される {{site.data.keyword.Bluemix_notm}} リソース。 以下の表に、情報提供を目的として以前の地域を示します。 今後は、[グローバル・エンドポイント](#endpoint)を使用して、地域のないアーキテクチャーに移行できます。
{: deprecated}

**複数ゾーンの大都市ロケーション**

<table summary="この表は、{{site.data.keyword.containerlong_notm}} で使用可能な複数ゾーンの大都市ロケーションを示しています。行は左から右に読み、1 列目はロケーションが存在する地理、2 列目はロケーションの国、3 列目はロケーションの大都市、4 列目はデータ・センター、および 5 列目はロケーションが以前に編成されていた非推奨の地域です。">
<caption>{{site.data.keyword.containerlong_notm}} で使用可能な複数ゾーンの大都市ロケーション。</caption>
  <thead>
  <th>地理</th>
  <th>国</th>
  <th>大都市</th>
  <th>データ・センター</th>
  <th>非推奨の地域</th>
  </thead>
  <tbody>
    <tr>
      <td>アジア太平洋</td>
      <td>オーストラリア</td>
      <td>シドニー</td>
      <td>syd01、syd04、syd05</td>
      <td>南アジア太平洋地域 (`ap-south`、`au-syd`)</td>
    </tr>
    <tr>
      <td>アジア太平洋</td>
      <td>日本</td>
      <td>東京</td>
      <td>tok02、tok04、tok05</td>
      <td>北アジア太平洋地域 (`ap-north`、`jp-tok`)</td>
    </tr>
    <tr>
      <td>ヨーロッパ</td>
      <td>ドイツ</td>
      <td>フランクフルト</td>
      <td>fra02、fra04、fra05</td>
      <td>中欧 (`eu-central`、`eu-de`)</td>
    </tr>
    <tr>
      <td>ヨーロッパ</td>
      <td>英国</td>
      <td>ロンドン</td>
      <td>lon04、lon05`*`、lon06</td>
      <td>英国南部 (`uk-south`、`eu-gb`)</td>
    </tr>
    <tr>
      <td>北アメリカ</td>
      <td>米国</td>
      <td>ダラス</td>
      <td>dal10、dal12、dal13</td>
      <td>米国南部 (`us-south`)</td>
    </tr>
    <tr>
      <td>北アメリカ</td>
      <td>米国</td>
      <td>ワシントン D.C.</td>
      <td>wdc04、wdc06、wdc07</td>
      <td>米国東部 (`us-east`)</td>
    </tr>
  </tbody>
  </table>

**単一ゾーンのデータ・センター・ロケーション**

<table summary="この表は、{{site.data.keyword.containerlong_notm}} で使用可能な単一ゾーンのデータ・センター・ロケーションを示しています。行は左から右に読み、1 列目はロケーションが存在する地理、2 列目はロケーションの国、3 列目はロケーションの大都市、4 列目はデータ・センター、および 5 列目はロケーションが以前に編成されていた非推奨の地域です。">
<caption>{{site.data.keyword.containerlong_notm}} で使用可能な単一ゾーンのロケーション。</caption>
  <thead>
  <th>地理</th>
  <th>国</th>
  <th>大都市</th>
  <th>データ・センター</th>
  <th>非推奨の地域</th>
  </thead>
  <tbody>
    <tr>
      <td>アジア太平洋</td>
      <td>オーストラリア</td>
      <td>メルボルン</td>
      <td>mel01</td>
      <td>南アジア太平洋地域 (`ap-south`、`au-syd`)</td>
    </tr>
    <tr>
      <td>アジア太平洋</td>
      <td>オーストラリア</td>
      <td>シドニー</td>
      <td>syd01、syd04、syd05</td>
      <td>南アジア太平洋地域 (`ap-south`、`au-syd`)</td>
    </tr>
    <tr>
      <td>アジア太平洋</td>
      <td>中国</td>
      <td>香港<br>中華人民共和国の特別行政区</td>
      <td>hkg02</td>
      <td>北アジア太平洋地域 (`ap-north`、`jp-tok`)</td>
    </tr>
    <tr>
      <td>アジア太平洋</td>
      <td>インド</td>
      <td>チェンナイ</td>
      <td>che01</td>
      <td>北アジア太平洋地域 (`ap-north`、`jp-tok`)</td>
    </tr>
    <tr>
      <td>アジア太平洋</td>
      <td>日本</td>
      <td>東京</td>
      <td>tok02、tok04、tok05</td>
      <td>北アジア太平洋地域 (`ap-north`、`jp-tok`)</td>
    </tr>
    <tr>
      <td>アジア太平洋</td>
      <td>韓国</td>
      <td>ソウル</td>
      <td>seo01</td>
      <td>北アジア太平洋地域 (`ap-north`、`jp-tok`)</td>
    </tr>
    <tr>
      <td>アジア太平洋</td>
      <td>シンガポール</td>
      <td>シンガポール</td>
      <td>sng01</td>
      <td>北アジア太平洋地域 (`ap-north`、`jp-tok`)</td>
    </tr>
    <tr>
      <td>ヨーロッパ</td>
      <td>フランス</td>
      <td>パリ</td>
      <td>par01</td>
      <td>中欧 (`eu-central`、`eu-de`)</td>
    </tr>
    <tr>
      <td>ヨーロッパ</td>
      <td>ドイツ</td>
      <td>フランクフルト</td>
      <td>fra02、fra04、fra05</td>
      <td>中欧 (`eu-central`、`eu-de`)</td>
    </tr>
    <tr>
      <td>ヨーロッパ</td>
      <td>イタリア</td>
      <td>ミラノ</td>
      <td>mil01</td>
      <td>中欧 (`eu-central`、`eu-de`)</td>
    </tr>
    <tr>
      <td>ヨーロッパ</td>
      <td>オランダ</td>
      <td>アムステルダム</td>
      <td>ams03</td>
      <td>中欧 (`eu-central`、`eu-de`)</td>
    </tr>
    <tr>
      <td>ヨーロッパ</td>
      <td>ノルウェー</td>
      <td>オスロ</td>
      <td>osl</td>
      <td>中欧 (`eu-central`、`eu-de`)</td>
    </tr>
    <tr>
      <td>ヨーロッパ</td>
      <td>英国</td>
      <td>ロンドン</td>
      <td>lon02`*`、lon04、lon05`*`、lon06</td>
      <td>英国南部 (`uk-south`、`eu-gb`)</td>
    </tr>
    <tr>
      <td>北アメリカ</td>
      <td>カナダ</td>
      <td>モントリオール</td>
      <td>mon01</td>
      <td>米国東部 (`us-east`)</td>
    </tr>
    <tr>
      <td>北アメリカ</td>
      <td>カナダ</td>
      <td>トロント</td>
      <td>tor01</td>
      <td>米国東部 (`us-east`)</td>
    </tr>
    <tr>
      <td>北アメリカ</td>
      <td>メキシコ</td>
      <td>メキシコ・シティー</td>
      <td>mex01</td>
      <td>米国南部 (`us-south`)</td>
    </tr>
    <tr>
      <td>北アメリカ</td>
      <td>米国</td>
      <td>ダラス</td>
      <td>dal10、dal12、dal13</td>
      <td>米国南部 (`us-south`)</td>
    </tr>
    <tr>
      <td>北アメリカ</td>
      <td>米国</td>
      <td>サンノゼ</td>
      <td>sjc03、sjc04</td>
      <td>米国南部 (`us-south`)</td>
    </tr>
    <tr>
      <td>北アメリカ</td>
      <td>米国</td>
      <td>ワシントン D.C.</td>
      <td>wdc04、wdc06、wdc07</td>
      <td>米国東部 (`us-east`)</td>
    </tr>
    <tr>
      <td>南米</td>
      <td>ブラジル</td>
      <td>サンパウロ</td>
      <td>sao01</td>
      <td>米国南部 (`us-south`)</td>
    </tr>
  </tbody>
  </table>

`*` lon02 は lon05 に置き換えられました。 新規クラスターでは lon05 を使用する必要があります。lon05 のみで、ゾーン間での可用性の高いマスター・スプレッドがサポートされます。
{: note}

### 単一ゾーン・クラスター
{: #regions_single_zone}

単一ゾーンのクラスターでは、クラスターのリソースは、クラスターがデプロイされたゾーンにとどまります。 次の図は、カナダのトロント `tor01` ロケーションの例を使用して、単一ゾーン・クラスターのコンポーネントの関係に焦点を当てています。
{: shortdesc}

<img src="/images/location-cluster-resources.png" width="650" alt="クラスター・リソースが存在する場所について" style="width:650px; border-style: none"/>

_単一ゾーンのクラスターのリソースが存在する場所について_

1.  クラスターのリソース (マスター・ノードやワーカー・ノードなど) は、クラスターをデプロイした同じデータ・センターにあります。 ローカル・コンテナーのオーケストレーション・アクション (`kubectl` コマンドなど) を実行すると、同じゾーン内のマスター・ノードとワーカー・ノードの間で情報が交換されます。

2.  他のクラスター・リソース (ストレージ、ネットワーキング、コンピュート、ポッドで実行されているアプリなど) をセットアップした場合、リソースとそのデータは、クラスターをデプロイしたゾーンに残ります。

3.  クラスターの管理操作 (`ibmcloud ks` コマンドの使用など) を開始すると、クラスターに関する基本情報 (名前、ID、ユーザー、コマンドなど) が、グローバル・エンドポイントを経由した地域のエンドポイントを介してルーティングされます。 地域エンドポイントは、最も近い複数ゾーンの大都市地域にあります。 この例では、大都市地域はワシントン D.C. です。

### 複数ゾーン・クラスター
{: #regions_multizone}

複数ゾーン・クラスターでは、クラスターのリソースは、高可用性を実現するために複数ゾーン間に分散されます。
{: shortdesc}

1.  ワーカー・ノードは、クラスターの可用性を向上するために、大都市ロケーション内の複数ゾーン間に分散されます。 Kubernetes マスター・レプリカもゾーン間に分散されます。 ローカル・コンテナーのオーケストレーション操作 (`kubectl` コマンドなど) を開始すると、グローバル・エンドポイントを介してマスター・ノードとワーカー・ノードの間で情報が交換されます。

2.  ストレージ、ネットワーキング、コンピュート、ポッドで実行されるアプリなど、他のクラスター・リソースは、さまざまな方法で複数ゾーン・クラスターにデプロイできます。 詳しくは、以下のトピックを参照してください。
    *   複数ゾーン・クラスターでの[ファイル・ストレージ](/docs/containers?topic=containers-file_storage#add_file)と[ブロック・ストレージ](/docs/containers?topic=containers-block_storage#add_block)のセットアップ、または [複数ゾーン永続ストレージ・ソリューションの選択](/docs/containers?topic=containers-storage_planning#persistent_storage_overview)
    *   [複数ゾーン・クラスターでネットワーク・ロード・バランサー (NLB) サービスを使用した、アプリへのパブリック・アクセスまたはプライベート・アクセスの有効化](/docs/containers?topic=containers-loadbalancer#multi_zone_config)
    *   [Ingress を使用したネットワーク・トラフィックの管理](/docs/containers?topic=containers-ingress#planning)
    *   [アプリの可用性の向上](/docs/containers?topic=containers-app#increase_availability)

3.  クラスターの管理操作 ([`ibmcloud ks` コマンド](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli)の使用など) を開始すると、クラスターに関する基本情報 (名前、ID、ユーザー、コマンドなど) がグローバル・エンドポイントを介してルーティングされます。

### フリー・クラスター
{: #regions_free}

フリー・クラスターは、特定のロケーションに制限されます。
{: shortdesc}

**CLI でのフリー・クラスターの作成**: フリー・クラスターを作成する前に、`ibmcloud ks region-set` を実行して、その地域をターゲットにする必要があります。 クラスターは、ターゲットにする地域内の大都市 ( `ap-south` の大都市シドニー、`eu-central` の大都市フランクフルト、`uk-south` の大都市ロンドン、または `us-south` の大都市ダラス) に作成されます。 大都市内のゾーンを指定することはできないことに注意してください。

**{{site.data.keyword.Bluemix_notm}} コンソールでのフリー・クラスターの作成**: コンソールを使用する場合、地理および地理内の大都市ロケーションを選択できます。 北アメリカの大都市ダラス、ヨーロッパの大都市ロンドン、またはアジア太平洋の大都市シドニーを選択できます。 クラスターは、選択した大都市内のゾーンに作成されます。

<br />


## グローバル・エンドポイントへのアクセス
{: #endpoint}

{{site.data.keyword.Bluemix_notm}} ロケーション (以前は地域と呼ばれていました) を使用して、{{site.data.keyword.Bluemix_notm}} サービス間のリソースを編成できます。 例えば、同じロケーションの {{site.data.keyword.registryshort_notm}} に保管されるプライベート Docker イメージを使用して、Kubernetes クラスターを作成することができます。 これらのリソースにアクセスするには、グローバル・エンドポイントを使用して、ロケーションでフィルタリングできます。
{:shortdesc}

### {{site.data.keyword.Bluemix_notm}} へのログイン
{: #login-ic}

{{site.data.keyword.Bluemix_notm}} (`ibmcloud`) コマンド・ラインにログインすると、地域を選択するよう求めるプロンプトが出されます。 ただし、この地域は、{{site.data.keyword.containerlong_notm}} プラグイン (`ibmcloud ks`) エンドポイントには影響しません。この地域では、引き続きグローバル・エンドポイントが使用されます。 クラスターがデフォルトのリソース・グループに含まれていない場合でも、クラスターが含まれているリソース・グループをターゲットにする必要があることに注意してください。
{: shortdesc}

{{site.data.keyword.Bluemix_notm}} グローバル API エンドポイントにログインして、クラスターが含まれているリソース・グループをターゲットにするには、次を実行します。
```
ibmcloud login -a https://cloud.ibm.com -g <nondefault_resource_group_name>
```
{: pre}

### {{site.data.keyword.containerlong_notm}} へのログイン
{: #login-iks}

{{site.data.keyword.Bluemix_notm}} にログインすると、{{site.data.keyword.containershort_notm}} にアクセスできます。 開始に役立つように、{{site.data.keyword.containerlong_notm}} の CLI および API を使用するための以下のリソースを確認してください。
{: shortdesc}

**{{site.data.keyword.containerlong_notm}} CLI**:
* [`ibmcloud ks` プラグインを使用するように、CLI をセットアップします](/docs/containers?topic=containers-cs_cli_install#cs_cli_install)。
* [特定のクラスターに接続するように CLI を構成して、`kubectl` コマンドを実行します](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)。

デフォルトでは、グローバル {{site.data.keyword.containerlong_notm}} エンドポイント (`https://containers.cloud.ibm.com`) にログインしています。

{{site.data.keyword.containerlong_notm}} CLI で新しいグローバル機能を使用する場合は、既存の地域ベース機能からの以下の変更点を考慮してください。

* リソースのリスト表示:
  * `ibmcloud ks clusters`、`ibmcloud ks subnets`、`ibmcloud ks zones` などのコマンドを使用してリソースをリストすると、すべてのロケーション内のリソースが返されます。特定のロケーションでリソースをフィルタリングするには、`--locations` フラグを含めます。例えば、大都市 `dal` のクラスターをフィルタリングする場合、その大都市内の複数ゾーン・クラスターおよびその大都市内のデータ・センター (ゾーン) の単一ゾーン・クラスターが返されます。`dal10` データ・センター (ゾーン) のクラスターをフィルタリングすると、そのゾーンにワーカー・ノードがある複数ゾーン・クラスターとそのゾーンにある単一ゾーン・クラスターが返されます。1 つのロケーションまたはロケーションのコンマ区切りのリストを渡すことができます。
    ロケーションでフィルタリングする例:
    ```
    ibmcloud ks clusters --locations dal
    ```
    {: pre}
  * その他のコマンドでは、すべてのロケーション内のリソースが返されません。 `credential-set/unset/get`、`api-key-reset`、および `vlan-spanning-get` の各コマンドを実行するには、`--region` に地域を指定する必要があります。

* リソースの操作:
  * グローバル・エンドポイントを使用する場合、`ibmcloud ks region-set` を実行して地域を設定しており、作業対象のリソースが別の地域に存在する場合であっても、任意のロケーションでアクセス許可を持つリソースを操作できます。
  * 異なる地域に同じ名前のクラスターがある場合は、コマンドの実行時にクラスター ID を使用するか、`ibmcloud ks region-set` コマンドで地域を設定し、コマンドの実行時にクラスター名を使用できます。

* 既存の機能:
  * ある地域のリソースのみをリストして処理する必要がある場合は、`ibmcloud ks init` [コマンド](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_init)を使用して、グローバル・エンドポイントではなく地域エンドポイントをターゲットにすることができます。
    米国南部地域エンドポイントをターゲットにする例:
    ```
    ibmcloud ks init --host https://us-south.containers.cloud.ibm.com
    ```
    {: pre}
  * グローバル機能を使用するには、再度 `ibmcloud ks init` コマンドを使用して、グローバル・エンドポイントをターゲットにします。 グローバル・エンドポイントを再度ターゲットにする例:
    ```
    ibmcloud ks init --host https://containers.cloud.ibm.com
    ```
    {: pre}

</br></br>
**{{site.data.keyword.containerlong_notm}} API**:
* [API を使用して作業を開始します](/docs/containers?topic=containers-cs_cli_install#cs_api)。
* [API コマンドに関する資料を表示します](https://containers.cloud.ibm.com/global/swagger-global-api/)。
* [`swagger.json` API](https://containers.cloud.ibm.com/global/swagger-global-api/swagger.json) を使用して、自動化で使用する API のクライアントを生成します。

グローバル {{site.data.keyword.containerlong_notm}} API と対話するには、コマンド・タイプを入力し、エンドポイントに `global/v1/command` を追加します。

`GET /clusters` グローバル API の例:
```
GET https://containers.cloud.ibm.com/global/v1/clusters
```
{: codeblock}

</br>

API 呼び出しで地域を指定する必要がある場合は、パスから `/global` パラメーターを削除して、`X-Region` ヘッダーに地域名を渡します。 使用可能な地域をリストするには、`ibmcloud ks regions` を実行します。

<br />



## 非推奨: 以前の {{site.data.keyword.Bluemix_notm}} 地域およびゾーン構造
{: #bluemix_regions}

以前は、{{site.data.keyword.Bluemix_notm}} リソースは地域に編成されていました。地域は、ゾーンを編成するための概念的なツールであり、さまざまな国のゾーン (データ・センター) を含めることができます。以下の表は、以前の {{site.data.keyword.Bluemix_notm}} 地域、{{site.data.keyword.containerlong_notm}} 地域、および {{site.data.keyword.containerlong_notm}} ゾーンをマップしています。複数ゾーン対応のゾーンは、太字で示されています。
{: shortdesc}

地域固有のエンドポイントは推奨されません。代わりに[グローバル・エンドポイント](#endpoint)を使用してください。
地域エンドポイントを使用する必要がある場合は、[{{site.data.keyword.containerlong_notm}} プラグインの `IKS_BETA_VERSION` 環境変数を `0.2` に設定](/docs/containers-cli-plugin?topic=containers-cli-plugin-kubernetes-service-cli#cs_beta)します。
{: deprecated}

| {{site.data.keyword.containerlong_notm}} 地域 | 対応する {{site.data.keyword.Bluemix_notm}} 地域 | 地域内で使用可能なゾーン |
| --- | --- | --- |
| 北アジア太平洋地域 (標準クラスターのみ) | 東京 | che01、hkg02、seo01、sng01、**tok02、tok04、tok05** |
| 南アジア太平洋地域 | シドニー | mel01、**syd01、syd04、syd05** |
| 中欧 | フランクフルト | ams03、**fra02、fra04、fra05**、mil01、osl01、par01 |
| 英国南部 | ロンドン | lon02、**lon04、lon05、lon06** |
| 米国東部 (標準クラスターのみ) | ワシントン DC | mon01、tor01、**wdc04、wdc06、wdc07** |
| 米国南部 | ダラス | **dal10、dal12、dal13**、mex01、sjc03、sjc04、sao01 |
{: caption="対応する {{site.data.keyword.containershort_notm}} 地域および {{site.data.keyword.Bluemix_notm}} 地域 (ゾーンを含む)。 複数ゾーン対応のゾーンは、太字で示されています。" caption-side="top"}

{{site.data.keyword.containerlong_notm}} 地域を使用して、ログインしている {{site.data.keyword.Bluemix_notm}} 地域以外の地域で、Kubernetes クラスターの作成とアクセスを行うことができます。 {{site.data.keyword.containerlong_notm}} 地域のエンドポイントとは、{{site.data.keyword.Bluemix_notm}} 全体ではなく、{{site.data.keyword.containerlong_notm}} のみを指します。

以下の理由で、別の {{site.data.keyword.containerlong_notm}} 地域にログインしたい場合があります。
  * ある地域で作成した {{site.data.keyword.Bluemix_notm}} サービスまたはプライベート Docker イメージを、別の地域の {{site.data.keyword.containerlong_notm}} で使用したい。
  * ログインしているデフォルトの {{site.data.keyword.Bluemix_notm}} 地域とは別の地域のクラスターにアクセスしたい。

素早く地域を切り替えるには、`ibmcloud ks region-set` [コマンド](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_region-set)を使用します。
