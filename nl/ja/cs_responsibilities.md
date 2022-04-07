---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

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


# {{site.data.keyword.containerlong_notm}} を使用する際の責任
{: #responsibilities_iks}

{{site.data.keyword.containerlong}} を使用する際のクラスターの管理責任とご利用条件について説明します。
{:shortdesc}

## クラスター管理の責任
{: #responsibilities}

IBM は、{{site.data.keyword.Bluemix_notm}} DevOps、AI、データ、およびセキュリティー・サービスとともに、アプリをデプロイするためのエンタープライズ・クラウド・プラットフォームを提供します。 クラウドでのアプリおよびサービスのセットアップ、統合、および操作方法を選択します。
{:shortdesc}

<table summary="この表は、IBM とお客様の責任を示しています。行は左から右に読みます。1 列目は各責任を表すアイコン、2 列目は説明です。">
<caption>IBM とお客様の責任</caption>
  <thead>
  <th colspan=2>タイプ別の責任</th>
  </thead>
  <tbody>
    <tr>
    <td align="center"><img src="images/icon_clouddownload.svg" alt="下向き矢印の付いたクラウドのアイコン"/><br>クラウド・インフラストラクチャー</td>
    <td>
    **IBM の責任**:
    <ul><li>各クラスターの保護された IBM 所有のインフラストラクチャー・アカウントに完全に管理された可用性の高い専用マスターをデプロイします。</li>
    <li>ご使用の IBM Cloud インフラストラクチャー (SoftLayer) アカウントでワーカー・ノードをプロビジョンします。</li>
    <li>VLAN やロード・バランサーなどのクラスター管理コンポーネントをセットアップします。</li>
    <li>ワーカー・ノードの追加と削除、デフォルトのサブネットの作成、永続ボリューム請求に対応するためのストレージ・ボリュームのプロビジョニングなど、インフラストラクチャーの追加要求を満たします。</li>
    <li>注文したインフラストラクチャー・リソースを統合して、ご使用のクラスター・アーキテクチャーで自動的に機能し、デプロイしたアプリおよびワークロードで使用できるようにします。</li></ul>
    <br><br>
    **お客様の責任**:
    <ul><li>提供された API、CLI、またはコンソール・ツールを使用して、[コンピュート](/docs/containers?topic=containers-clusters#clusters)および[ストレージ](/docs/containers?topic=containers-storage_planning#storage_planning)容量を調整し、ワークロードのニーズを満たすように[ネットワーク構成](/docs/containers?topic=containers-cs_network_cluster#cs_network_cluster)を調整します。</li></ul><br><br>
    </td>
     </tr>
     <tr>
     <td align="center"><img src="images/icon_tools.svg" alt="レンチ・アイコン"/><br>管理対象クラスター</td>
     <td>
     **IBM の責任**:
     <ul><li>{{site.data.keyword.containerlong_notm}} [API ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://containers.cloud.ibm.com/global/swagger-global-api/)、[CLI プラグイン](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli)、[コンソール ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/kubernetes/clusters) など、クラスター管理を自動化するためのツールを提供します。</li>
     <li>Kubernetes マスターに OS、バージョン、およびセキュリティーのパッチ更新を自動的に適用します。 メジャー更新とマイナー更新を適用できるようにします。</li>
     <li>Ingress アプリケーション・ロード・バランサーやファイル・ストレージ・プラグインなど、クラスター内の運用 {{site.data.keyword.containerlong_notm}} および Kubernetes コンポーネントを更新およびリカバリーします。</li>
     <li>Kubernetes ワークロード構成ファイルなどの etcd でデータをバックアップおよびリカバリーします</li>
     <li>クラスターの作成時にマスター・ノードとワーカー・ノードの間の OpenVPN 接続をセットアップします。</li>
     <li>さまざまなインターフェースでマスター・ノードとワーカー・ノードの正常性をモニターおよび報告します。</li>
     <li>ワーカー・ノードに OS、バージョン、およびセキュリティーのメジャー、マイナー、およびパッチ更新を提供します。</li>
     <li>ワーカー・ノードを更新およびリカバリーするための自動化要求を満たします。 オプションの[ワーカー・ノードの自動リカバリー](/docs/containers?topic=containers-health#autorecovery)を提供します。</li>
     <li>[クラスター自動スケーリング機能](/docs/containers?topic=containers-ca#ca)などのツールを提供して、クラスター・インフラストラクチャーを拡張します。</li>
     </ul>
     <br><br>
     **お客様の責任**:
     <ul>
     <li>API、CLI、またはコンソール・ツールを使用して、提供されたメジャーまたはマイナー Kubernetes マスター更新とワーカー・ノードのメジャー、マイナー、およびパッチ更新を[適用](/docs/containers?topic=containers-update#update)します。</li>
     <li>API、CLI、またはコンソール・ツールを使用して、インフラストラクチャー・リソースを[リカバリー](/docs/containers?topic=containers-cs_troubleshoot#cs_troubleshoot)したり、オプションの[ワーカー・ノードの自動リカバリー](/docs/containers?topic=containers-health#autorecovery)をセットアップおよび構成したりします。</li></ul>
     <br><br></td>
      </tr>
    <tr>
      <td align="center"><img src="images/icon_locked.svg" alt="ロック・アイコン"/><br>セキュリティーが充実した環境</td>
      <td>
      **IBM の責任**:
      <ul>
      <li>PCI DSS など、[さまざまな業界のコンプライアンス規格](/docs/containers?topic=containers-faqs#standards)に対応した制御を維持します。</li>
      <li>クラスターのマスターをモニター、分離、およびリカバリーします。</li>
      <li>Kubernetes マスターの API サーバー、etcd、スケジューラー、およびコントローラー・マネージャー・コンポーネントの可用性の高いレプリカを提供して、マスターの停止を防ぎます。</li>
      <li>マスターにセキュリティーのパッチ更新を自動的に適用し、ワーカー・ノードにセキュリティーのパッチ更新を提供します。</li>
      <li>ワーカー・ノードの暗号化されたディスクなど、特定のセキュリティー設定を有効にします</li>
      <li>ユーザーのホストへの SSH を許可しないなど、ワーカー・ノードの特定の非セキュアなアクションを無効にします。</li>
      <li>マスターとワーカー・ノードの間の通信を TLS で暗号化します。</li>
      <li>ワーカー・ノードのオペレーティング・システムに CIS 準拠の Linux イメージを提供します。</li>
      <li>マスターおよびワーカー・ノードのイメージを継続的にモニターして、脆弱性およびセキュリティー・コンプライアンス上の問題を検出します。</li>
      <li>ワーカー・ノードを 2 つのローカル SSD、AES 256 ビット暗号化データ・パーティションでプロビジョンします。</li>
      <li>パブリックおよびプライベートのサービス・エンドポイントなど、クラスターのネットワークの接続に関するオプションを提供します。</li>
      <li>専用仮想マシン、ベアメタル、トラステッド・コンピューティングを含むベアメタルなど、コンピュートの分離のためのオプションを提供します。</li>
      <li>Kubernetes 役割ベース・アクセス制御 (RBAC) を {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM) と統合します。</li>
      </ul>
      <br><br>
      **お客様の責任**:
      <ul>
      <li>API、CLI、またはコンソール・ツールを使用して、提供された[セキュリティーのパッチ更新](/docs/containers?topic=containers-changelog#changelog)をワーカー・ノードに適用します。</li>
      <li>[クラスター・ネットワーク](/docs/containers?topic=containers-plan_clusters)のセットアップ方法を選択し、追加の[セキュリティー設定](/docs/containers?topic=containers-security#security)を構成して、ワークロードのセキュリティーとコンプライアンスのニーズを満たします。 適用可能であれば、[ファイアウォール](/docs/containers?topic=containers-firewall#firewall)を構成します。</li></ul>
      <br><br></td>
      </tr>

      <tr>
        <td align="center"><img src="images/icon_code.svg" alt="コードの括弧アイコン"/><br>アプリのオーケストレーション</td>
        <td>
        **IBM の責任**:
        <ul>
        <li>Kubernetes API にアクセスできるように、インストールされた Kubernetes コンポーネントでクラスターをプロビジョンします。</li>
        <li>[Istio](/docs/containers?topic=containers-istio#istio) や [Knative](/docs/containers?topic=containers-serverless-apps-knative) など、多くの管理対象アドオンを提供して、アプリの機能を拡張します。 IBM によって管理対象アドオンのインストールおよび更新が提供されるため、保守が簡素化されます。</li>
        <li>{{site.data.keyword.la_short}}、{{site.data.keyword.mon_short}}、Portworx など、選択したサード・パーティーのパートナーシップ・テクノロジーとのクラスター統合を提供します。</li>
        <li>他の {{site.data.keyword.Bluemix_notm}} サービスへのサービス・バインディングを可能にする自動化を提供します。</li>
        <li>`default` Kubernetes 名前空間でのデプロイメントが {{site.data.keyword.registrylong_notm}} からイメージをプルできるように、イメージ・プル・シークレットを使用してクラスターを作成します。</li>
        <li>ご使用のアプリで使用する永続ボリュームをサポートするストレージ・クラスおよびプラグインを提供します。</li>
        <li>アプリを外部から公開する場合に使用するために予約されているサブネット IP アドレスを使用してクラスターを作成します。</li>
        <li>サービスを外部から公開するためのネイティブ Kubernetes パブリックおよびプライベート・ロード・バランサーおよび Ingress ルートをサポートします。</li>
        </ul>
        <br><br>
        **お客様の責任**:
        <ul>
        <li>提供されたツールおよび機能を使用して、[構成とデプロイ](/docs/containers?topic=containers-app#app)、[許可のセットアップ](/docs/containers?topic=containers-users#users)、[他のサービスとの統合](/docs/containers?topic=containers-supported_integrations#supported_integrations)、[外部からの処理](/docs/containers?topic=containers-cs_network_planning#cs_network_planning)、[正常性のモニター](/docs/containers?topic=containers-health#health)、[データの保存、バックアップ、およびリストア](/docs/containers?topic=containers-storage_planning#storage_planning)を行い、それ以外の場合は[可用性が高く](/docs/containers?topic=containers-ha#ha)、回復力の高いワークロードを管理します。</li>
        </ul>
        </td>
        </tr>
  </tbody>
  </table>

<br />


## {{site.data.keyword.containerlong_notm}} の不正使用
{: #terms}

お客様は {{site.data.keyword.containerlong_notm}} を不正使用してはいけません。
{:shortdesc}

不正使用には、以下が含まれます。

*   不法行為
*   マルウェアの配布や実行
*   {{site.data.keyword.containerlong_notm}} に損害を与えたり、他のお客様による {{site.data.keyword.containerlong_notm}} の使用に干渉したりすること
*   他のサービスやシステムに損害を与えたり、他のお客様による使用に干渉したりすること
*   サービスまたはシステムに対する無許可アクセス
*   サービスまたはシステムに対する無許可の変更
*   他のお客様の権利を侵害すること

すべての使用条件については、[クラウド・サービスのご利用条件](/docs/overview/terms-of-use?topic=overview-terms#terms)を参照してください。
