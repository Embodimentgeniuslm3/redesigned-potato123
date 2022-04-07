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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}



# ハイブリッド・クラウド
{: #hybrid_iks_icp}

{{site.data.keyword.Bluemix}} Private アカウントがある場合は、そのアカウントを使用して {{site.data.keyword.containerlong}} を含む上質な {{site.data.keyword.Bluemix_notm}} サービスをご利用いただけます。 詳しくは、[{{site.data.keyword.Bluemix_notm}} Private および IBM Public Cloud![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン") でのハイブリッド・エクスペリエンス](http://ibm.biz/hybridJune2018)に関するブログを参照してください。
{: shortdesc}

ここまでに、[{{site.data.keyword.Bluemix_notm}}オファリング](/docs/containers?topic=containers-cs_ov#differentiation)について理解して、[クラウド上でどのようなワークロードを実行するか](/docs/containers?topic=containers-strategy#cloud_workloads)に関する Kubernetes 戦略を策定しました。 これで、strongSwan VPN サービスまたは {{site.data.keyword.BluDirectLink}} を使用して、パブリック・クラウドとプライベート・クラウドを接続できるようになります。

* [strongSwan VPN サービス](#hybrid_vpn)は、業界標準の Internet Protocol Security (IPSec) プロトコル・スイートに基づくインターネットを介したセキュアなエンドツーエンドの通信チャネルを使用して、Kubernetes クラスターをオンプレミス・ネットワークと安全に接続します。
* [{{site.data.keyword.Bluemix_notm}} Direct Link](#hybrid_dl) を使用すると、パブリック・インターネット経由でルーティングせずに、リモート・ネットワーク環境と {{site.data.keyword.containerlong_notm}} の間に直接のプライベート接続を作成できます。

パブリック・クラウドとプライベート・クラウドを接続した後、[プライベート・パッケージをパブリック・コンテナーに再利用できます](#hybrid_ppa_importer)。

## strongSwan VPN でのパブリック・クラウドとプライベート・クラウドの接続
{: #hybrid_vpn}

パブリック Kubernetes クラスターと {{site.data.keyword.Bluemix}} Private インスタンスの間に VPN 接続を確立して、両方向通信を可能にします。
{: shortdesc}

1.  {{site.data.keyword.containerlong}} がある標準クラスターを {{site.data.keyword.Bluemix_notm}} Public に作成するか、または既存のクラスターを使用します。 クラスターを作成するには、以下のいずれかのオプションを選択します。
    - [コンソールまたは CLI から、標準クラスターを作成します](/docs/containers?topic=containers-clusters#clusters_ui)。
    - [クラウド自動化マネージャー (CAM) を使用して、事前定義テンプレート![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/support/knowledgecenter/SS2L37_2.1.0.3/cam_deploy_IKS.html) を使ってクラスターを作成します。 CAM を使用してクラスターをデプロイすると、Helm tiller が自動的にインストールされます。

2.  {{site.data.keyword.containerlong_notm}} クラスターで、[strongSwan IPSec VPN サービスをセットアップするための指示に従います](/docs/containers?topic=containers-vpn#vpn_configure)。

    *  [ステップ 2](/docs/containers?topic=containers-vpn#strongswan_2) では、以下のことに注意します。

       * {{site.data.keyword.containerlong_notm}} クラスターで設定した `local.id` は、後で {{site.data.keyword.Bluemix}} Private クラスターで `remote.id` として設定するものと一致している必要があります。
       * {{site.data.keyword.containerlong_notm}} クラスターで設定した `remote.id` は、後で {{site.data.keyword.Bluemix}} Private クラスターで `local.id` として設定するものと一致している必要があります。
       * {{site.data.keyword.containerlong_notm}} クラスターで設定した `preshared.secret` は、後で {{site.data.keyword.Bluemix}} Private クラスターで `preshared.secret` として設定するものと一致している必要があります。

    *  [ステップ 3](/docs/containers?topic=containers-vpn#strongswan_3) では、**インバウンド** VPN 接続用に strongSwan を構成します。

       ```
       ipsec.auto: add
       loadBalancerIP: <portable_public_IP>
       ```
       {: codeblock}

3.  `loadbalancerIP` として設定したポータブル・パブリック IP アドレスをメモします。

    ```
    kubectl get svc vpn-strongswan
    ```
    {: pre}

4.  [{{site.data.keyword.Bluemix_notm}} Private![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン") にクラスターを作成します](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/installing/installing.html)。

5.  {{site.data.keyword.Bluemix_notm}} Private クラスターに、strongSwan IPSec VPN サービスをデプロイします。

    1.  [strongSwan IPSec VPN 回避策 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン") を完了します](https://www.ibm.com/support/knowledgecenter/SS2L37_2.1.0.3/cam_strongswan.html)。

    2.  プライベート・クラスターに [strongSwan VPN Helm チャートをセットアップ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン") します。](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/app_center/create_release.html)

        *  構成パラメーターで、**Remote gateway** フィールドを、{{site.data.keyword.containerlong_notm}} クラスターの `loadbalancerIP` として設定したポータブル・パブリック IP アドレスの値に設定します。

           ```
           Operation at startup: start
           ...
           Remote gateway: <portable_public_IP>
           ...
           ```
           {: codeblock}

        *  プライベートの `local.id` とパブリックの `remote.id`、プライベートの `remote.id` とパブリックの `local.id`、およびプライベートとパブリックの `preshared.secret` の値が一致している必要があります。

        これで、{{site.data.keyword.Bluemix_notm}} Private クラスターから {{site.data.keyword.containerlong_notm}} クラスターへの接続を開始できます。

7.  クラスター間で [VPN 接続をテストします](/docs/containers?topic=containers-vpn#vpn_test)。

8.  接続するクラスターごとに上記の手順を繰り返します。

**次の作業**

*   [ライセンス交付を受けたソフトウェア・イメージをパブリック・クラスター内で実行します](#hybrid_ppa_importer)。
*   複数のクラウド Kubernetes クラスターを管理するには ({{site.data.keyword.Bluemix_notm}} Public と {{site.data.keyword.Bluemix_notm}} Private にまたがる場合など)、[IBM Multicloud Manager ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.1.0/mcm/getting_started/introduction.html) を確認してください。


## {{site.data.keyword.Bluemix_notm}} Direct Link を使用したパブリック・クラウドとプライベート・クラウドの接続
{: #hybrid_dl}

[{{site.data.keyword.BluDirectLink}}](/docs/infrastructure/direct-link?topic=direct-link-about-ibm-cloud-direct-link) を使用すると、パブリック・インターネット経由でルーティングせずに、リモート・ネットワーク環境と {{site.data.keyword.containerlong_notm}} の間に直接のプライベート接続を作成できます。
{: shortdesc}

パブリック・クラウドとオンプレミス {{site.data.keyword.Bluemix}} Private インスタンスを接続するには、以下の 4 つのオファリングのいずれかを使用できます。
* {{site.data.keyword.Bluemix_notm}} Direct Link Connect
* {{site.data.keyword.Bluemix_notm}} Direct Link Exchange
* {{site.data.keyword.Bluemix_notm}} Direct Link Dedicated
* {{site.data.keyword.Bluemix_notm}} Direct Link Dedicated Hosting

{{site.data.keyword.Bluemix_notm}} Direct Link オファリングを選択し、{{site.data.keyword.Bluemix_notm}} Direct Link 接続をセットアップするには、{{site.data.keyword.Bluemix_notm}} Direct Link の資料の [{{site.data.keyword.Bluemix_notm}}Direct Link での作業の開始](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link#how-do-i-know-which-type-of-ibm-cloud-direct-link-i-need-)を参照してください。

**次の作業**</br>
* [ライセンス交付を受けたソフトウェア・イメージをパブリック・クラスター内で実行します](#hybrid_ppa_importer)。
* 複数のクラウド Kubernetes クラスターを管理するには ({{site.data.keyword.Bluemix_notm}} Public と {{site.data.keyword.Bluemix_notm}} Private にまたがる場合など)、[IBM Multicloud Manager ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.1.0/mcm/getting_started/introduction.html) を確認してください。

<br />


## パブリック Kubernetes コンテナーでの {{site.data.keyword.Bluemix_notm}} Private イメージの実行
{: #hybrid_ppa_importer}

{{site.data.keyword.Bluemix_notm}} Public のクラスター内で、{{site.data.keyword.Bluemix_notm}} Private 用にパッケージされた IBM の選りすぐりのライセンス製品を実行することができます。  
{: shortdesc}

ライセンス・ソフトウェアは、[IBM パスポート・アドバンテージ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www-01.ibm.com/software/passportadvantage/index.html) で入手できます。 {{site.data.keyword.Bluemix_notm}} Public のクラスターでこのソフトウェアを使用するには、ソフトウェアをダウンロードし、イメージを解凍し、{{site.data.keyword.registryshort}} の名前空間にイメージをアップロードする必要があります。 ソフトウェアの使用を計画している環境には関係なく、まず必要な製品ライセンスを取得する必要があります。

次の表は、{{site.data.keyword.Bluemix_notm}} Public 内のクラスターで使用可能な {{site.data.keyword.Bluemix_notm}} Private 製品の概要です。

| 製品名 | バージョン | パーツ番号 |
| --- | --- | --- |
| IBM Db2 Direct Advanced Edition Server | 11.1 | CNU3TML |
| IBM Db2 Advanced Enterprise Server Edition Server | 11.1 | CNU3SML |
| IBM MQ Advanced | 9.1.0.0, 9.1.1,0, 9.1.2.0 | - |
| IBM WebSphere Application Server Liberty | 16.0.0.3 | Docker Hub イメージ |
{: caption="表。 {{site.data.keyword.Bluemix_notm}} Public で使用可能な {{site.data.keyword.Bluemix_notm}} Private 製品。" caption-side="top"}

開始前に、以下のことを行います。
- [{{site.data.keyword.registryshort}}CLI プラグイン (`ibmcloud cr`) をインストールします](/docs/services/Registry?topic=registry-registry_setup_cli_namespace#cli_namespace_registry_cli_install)。
- [{{site.data.keyword.registryshort}}](/docs/services/Registry?topic=registry-registry_setup_cli_namespace#registry_namespace_setup)に名前空間をセットアップするか、`ibmcloud cr namespaces` を実行して既存の名前空間を取得します。
- [`kubectl` CLI のターゲットを自分のクラスターに設定します](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)。
- [Helm CLI をインストールして、クラスターに tiller をセットアップします](/docs/containers?topic=containers-helm#public_helm_install)。

次のようにして、{{site.data.keyword.Bluemix_notm}} Public のクラスターに {{site.data.keyword.Bluemix_notm}} Private イメージをデプロイします。

1.  [{{site.data.keyword.registryshort}} の資料](/docs/services/Registry?topic=registry-ts_index#ts_ppa)の手順に従って、ライセンス・ソフトウェアを IBM パスポート・アドバンテージからダウンロードし、イメージを名前空間にプッシュし、クラスター内に Helm チャートをインストールします。

    **IBM WebSphere Application Server Liberty の場合**:

    1.  IBM パスポート・アドバンテージからイメージを取得する代わりに、[Docker Hub イメージ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://hub.docker.com/_/websphere-liberty/) を使用してください。 製品ライセンスを入手する手順については、
[Docker Hub から入手したイメージから実動イメージへのアップグレード![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/WASdev/ci.docker/tree/master/ga/production-upgrade) を参照してください。

    2.  [LibertyHelm チャートの指示![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/support/knowledgecenter/en/SSEQTP_liberty/com.ibm.websphere.wlp.doc/ae/rwlp_icp_helm.html) に従ってください。

2.  Helm チャートの **STATUS** が `DEPLOYED` になっていることを確認します。 それ以外の場合は、数分待ってから、再試行してください。
    ```
    helm status <helm_chart_name>
    ```
    {: pre}

3.  クラスターで製品を構成および使用する方法について詳しくは、製品固有の資料を参照してください。

    - [IBMDb2 Direct Advanced Edition Server ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/support/knowledgecenter/en/SSEPGG_11.1.0/com.ibm.db2.luw.licensing.doc/doc/c0070181.html)
    - [IBMMQ Advanced ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/support/knowledgecenter/en/SSFKSJ_9.0.0/com.ibm.mq.helphome.v90.doc/WelcomePagev9r0.html)
    - [IBMWebSphere Application Server Liberty ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/support/knowledgecenter/en/SSEQTP_liberty/as_ditamaps/was900_welcome_liberty.html)
