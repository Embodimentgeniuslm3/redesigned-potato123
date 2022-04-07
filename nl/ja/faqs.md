---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-05"

keywords: kubernetes, iks, compliance, security standards

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
{:download: .download}
{:preview: .preview}
{:faq: data-hd-content-type='faq'}


# FAQ
{: #faqs}

## Kubernetes とは何ですか?
{: #kubernetes}
{: faq}

Kubernetes は、コンテナー化されたワークロードやサービスを複数のホストにわたって管理するためのオープン・ソース・プラットフォームです。手作業による介入をまったく、あるいはほとんど必要とせずに、コンテナー化アプリのデプロイ、自動化、モニター、スケーリングを行える管理ツールを備えています。 マイクロサービスを構成するすべてのコンテナーは、管理と検出を簡単にするための論理的なユニットであるポッドにグループ分けされます。 これらのポッドを実行するコンピュート・ホストは、ポータブルで拡張性に優れ、障害発生時の自己修復機能を備えた Kubernetes クラスターで管理されます。
{: shortdesc}

Kubernetes について詳しくは、[Kubernetes の資料 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/home/?path=users&persona=app-developer&level=foundational) を参照してください。

## IBM Cloud Kubernetes Service はどのように機能しますか?
{: #kubernetes_service}
{: faq}

{{site.data.keyword.containerlong_notm}} を利用すると、{{site.data.keyword.Bluemix_notm}} で独自の Kubernetes クラスターを作成してコンテナー化アプリをデプロイおよび管理できます。 コンテナー化アプリは、ワーカー・ノードと呼ばれる IBM Cloud インフラストラクチャー (SoftLayer) のコンピュート・ホスト上でホストされます。 コンピュート・ホストは、共有リソースまたは専用リソースを備えた[仮想マシン](/docs/containers?topic=containers-planning_worker_nodes#vm)としてプロビジョンすることも、GPU およびソフトウェア定義ストレージ (SDS) を使用するために最適化できる[ベアメタル・マシン](/docs/containers?topic=containers-planning_worker_nodes#bm)としてプロビジョンすることもできます。ワーカー・ノードは、IBM が構成、モニター、および管理する、高可用性の Kubernetes マスターによって制御されます。 クラスター・インフラストラクチャーのリソースの操作には {{site.data.keyword.containerlong_notm}} の API または CLI を、デプロイメントとサービスの管理には Kubernetes の API または CLI を使用できます。

クラスター・リソースのセットアップ方法について詳しくは、[サービス・アーキテクチャー](/docs/containers?topic=containers-ibm-cloud-kubernetes-service-technology#architecture)を参照してください。 機能と利点を示したリストを確認するには、[{{site.data.keyword.containerlong_notm}} を使用する理由](/docs/containers?topic=containers-cs_ov#cs_ov)を参照してください。

## IBM Cloud Kubernetes Service を使用するとよいのはなぜですか?
{: #faq_benefits}
{: faq}

{{site.data.keyword.containerlong_notm}} は、IBM Watson®、AI、IoT、DevOps、セキュリティー、データ分析に関連するクラウド・サービスにバインド可能なアプリを迅速にデリバリーできるように、強力なツール、直感的なユーザー・エクスペリエンス、組み込みのセキュリティーを提供するマネージド Kubernetes オファリングです。 {{site.data.keyword.containerlong_notm}} は、認定された Kubernetes プロバイダーとして、インテリジェントなスケジューリング、自己修復、水平スケーリング、サービス・ディスカバリーとロード・バランシング、自動によるロールアウトとロールバック、シークレットと構成の管理を可能にします。 また、このサービスは、シンプルなクラスター管理、コンテナー・セキュリティーおよび分離ポリシーに関連する拡張機能、独自のクラスターを設計する機能、一貫性のあるデプロイメントを行うための統合運用ツールを備えています。

機能と利点の概要については、[{{site.data.keyword.containerlong_notm}} を使用する理由](/docs/containers?topic=containers-cs_ov#cs_ov)を参照してください。

## このサービスにはマネージドの Kubernetes マスターおよびワーカー・ノードは付属していますか?
{: #managed_master_worker}
{: faq}

{{site.data.keyword.containerlong_notm}} に含まれる Kubernetes クラスターはすべて、IBM が所有する {{site.data.keyword.Bluemix_notm}} インフラストラクチャー・アカウントで、IBM が管理する専用 Kubernetes マスターによって制御されます。 Kubernetes マスター (すべてのマスター・コンポーネント、コンピュート・リソース、ネットワーキング・リソース、ストレージ・リソースを含む) は、IBM サイト信頼性エンジニア (SRE) によって継続的にモニターされます。 SRE は、最新のセキュリティー規格を適用し、悪意のあるアクティビティーを検出して対処し、{{site.data.keyword.containerlong_notm}} の信頼性と可用性を確保するための作業を行います。 クラスターのプロビジョン時に自動的にインストールされるアドオン (ロギング用の Fluentd など) は、IBM によって自動的に更新されます。 ただし、一部のアドオンの自動更新を無効にして、マスターおよびワーカー・ノードとは別に手動で更新することもできます。 詳しくは、[クラスター・アドオンの更新](/docs/containers?topic=containers-update#addons)を参照してください。

Kubernetes は、定期的に[メジャー、マイナー、またはパッチの更新](/docs/containers?topic=containers-cs_versions#version_types)をリリースしています。 これらの更新は、Kubernetes API サーバーのバージョンや Kubernetes マスター内の他のコンポーネントに影響を与える可能性があります。 パッチ・バージョンの更新は IBM によって自動的に実行されますが、マスターのメジャー・バージョンとマイナー・バージョンの更新はお客様が行う必要があります。 詳しくは、[Kubernetes マスターの更新](/docs/containers?topic=containers-update#master)を参照してください。

標準クラスターのワーカー・ノードは、{{site.data.keyword.Bluemix_notm}} インフラストラクチャー・アカウントにプロビジョンされます。 ワーカー・ノードはユーザー・アカウントに専用のものです。このためユーザーは、ワーカー・ノードの OS および {{site.data.keyword.containerlong_notm}} コンポーネントに最新のセキュリティー更新とパッチが適用されるように、ワーカー・ノードに対するタイムリーな更新を要求する責任があります。 セキュリティー更新およびパッチは、IBM サイト信頼性エンジニア (SRE) から提供されます。SRE は、脆弱性およびセキュリティー・コンプライアンス上の問題を検出するために、お客様のワーカー・ノードにインストールされている Linux イメージを継続的にモニターしています。 詳しくは、[ワーカー・ノードの更新](/docs/containers?topic=containers-update#worker_node)を参照してください。

## Kubernetes マスターおよびワーカー・ノードは高可用性ですか?
{: #faq_ha}
{: faq}

{{site.data.keyword.containerlong_notm}} のアーキテクチャーとインフラストラクチャーは、信頼性を確保し、処理待ち時間を短く、サービスの実行可能時間を最大にするように設計されています。 デフォルトでは、{{site.data.keyword.containerlong_notm}} のすべてのクラスターに、複数の Kubernetes マスター・インスタンスがセットアップされます。これにより、1 つ以上の Kubernetes マスター・インスタンスが使用不可になっても、クラスター・リソースの可用性と利用可能性を確保できます。

クラスターの可用性をさらに高め、アプリのダウン時間を回避するために、地域の複数のゾーンに複数のワーカー・ノードを置いてワークロードを分散させることができます。 [複数ゾーン・クラスター](/docs/containers?topic=containers-ha_clusters#multizone)と呼ばれるこのセットアップでは、1 つのワーカー・ノードまたは 1 つのゾーン全体が使用不可になっても、引き続きアプリにアクセスできます。

地域全体の障害から保護するには、[複数のクラスターを作成し、それらを複数の {{site.data.keyword.containerlong_notm}} 地域に分散](/docs/containers?topic=containers-ha_clusters#multiple_clusters)させます。それらのクラスターに対するネットワーク・ロード・バランサー (NLB) をセットアップすることで、クラスターの地域間ロード・バランシングと地域間ネットワーキングを実現できます。

障害発生時にもデータを使用できるようにするには、必ず[永続ストレージ](/docs/containers?topic=containers-storage_planning#storage_planning)にデータを保管してください。

クラスターを高可用性にする方法について詳しくは、[{{site.data.keyword.containerlong_notm}} の高可用性](/docs/containers?topic=containers-ha#ha)を参照してください。

## どのような方法でクラスターを保護できますか?
{: #secure_cluster}
{: faq}

{{site.data.keyword.containerlong_notm}} の組み込みセキュリティー機能を使用して、クラスター内のコンポーネント、データ、アプリ・デプロイメントを保護し、セキュリティー・コンプライアンスとデータ保全性を確保できます。 これらの機能を使用して、Kubernetes API サーバー、etcd データ・ストア、ワーカー・ノード、ネットワーク、ストレージ、イメージ、デプロイメントを悪意のある攻撃から保護できます。 また、ロギングおよびモニタリングのための組み込みのツールを利用して、悪意のある攻撃や不審な使用パターンを検出することもできます。

クラスターのコンポーネント、および各コンポーネントを保護する方法について詳しくは、[{{site.data.keyword.containerlong_notm}} のセキュリティー](/docs/containers?topic=containers-security#security)を参照してください。

## クラスター・ユーザーには、どのようなアクセス・ポリシーを付与しますか?
{: #faq_access}
{: faq}

{{site.data.keyword.containerlong_notm}} では、{{site.data.keyword.iamshort}} (IAM) を使用して、IAM プラットフォーム役割によってクラスター・リソースへのアクセス権限を付与するか、IAM サービス役割によって Kubernetes 役割ベース・アクセス制御 (RBAC) ポリシーを割り当てます。アクセス・ポリシーのタイプについて詳しくは、[ユーザーに対する適切なアクセス・ポリシーと役割の選出](/docs/containers?topic=containers-users#access_roles)を参照してください。
{: shortdesc}

ユーザーに割り当てるアクセス・ポリシーは、ユーザーに実行できるようにする操作によって異なります。どの役割がどのタイプのアクションを許可するかについて詳しくは、[ユーザー・アクセスのリファレンス・ページ](/docs/containers?topic=containers-access_reference)または以下の表のリンクを参照してください。ポリシーを割り当てる手順については、[{{site.data.keyword.Bluemix_notm}} IAM によるユーザーへのクラスター・アクセス権限の付与](/docs/containers?topic=containers-users#platform)を参照してください。

| ユース・ケース | 役割および有効範囲の例 |
| --- | --- |
| アプリ監査員 | [クラスター、地域、またはリソース・グループに対するビューアー・プラットフォーム役割](/docs/containers?topic=containers-access_reference#view-actions)、[クラスター、地域、またはリソース・グループに対するリーダー・サービス役割](/docs/containers?topic=containers-access_reference#service)。|
| アプリ開発者 | [クラスターに対するエディター・プラットフォーム役割](/docs/containers?topic=containers-access_reference#editor-actions)、[1 つの名前空間に有効範囲が設定されたライター・サービス役割](/docs/containers?topic=containers-access_reference#service)、[Cloud Foundry 開発者スペース役割](/docs/containers?topic=containers-access_reference#cloud-foundry)。|
| 課金 | [クラスター、地域、またはリソース・グループに対するビューアー・プラットフォーム役割](/docs/containers?topic=containers-access_reference#view-actions)。|
| クラスターの作成 | スーパーユーザー・インフラストラクチャー資格情報に対するアカウント・レベルの許可、{{site.data.keyword.containerlong_notm}} に対する管理者プラットフォーム役割、および {{site.data.keyword.registrylong_notm}} に対する管理者プラットフォーム役割。詳しくは、[クラスター作成の準備](/docs/containers?topic=containers-clusters#cluster_prepare)を参照してください。|
| クラスター管理者| [クラスターに対する管理者プラットフォーム役割](/docs/containers?topic=containers-access_reference#admin-actions)、[1 つの名前空間に有効範囲が設定されていない (クラスター全体に対する) マネージャー・サービス役割](/docs/containers?topic=containers-access_reference#service)。|
| DevOps オペレーター | [クラスターに対するオペレーター・プラットフォーム役割](/docs/containers?topic=containers-access_reference#operator-actions)、[1 つの名前空間に有効範囲が設定されていない (クラスター全体に対する) ライター・サービス役割](/docs/containers?topic=containers-access_reference#service)、[Cloud Foundry 開発者スペース役割](/docs/containers?topic=containers-access_reference#cloud-foundry)。|
| オペレーターまたはサイト信頼性エンジニア | [クラスター、地域、またはリソース・グループに対する管理者プラットフォーム役割](/docs/containers?topic=containers-access_reference#admin-actions)、[クラスターまたは地域に対するリーダー・サービス役割](/docs/containers?topic=containers-access_reference#service)、または `kubectl top nodes,pods` コマンドを使用できる[すべてのクラスター名前空間に対するマネージャー・サービス役割](/docs/containers?topic=containers-access_reference#service)。|
{: caption="さまざまなユース・ケースを満たすために割り当てることができる役割のタイプ" caption-side="top"}

## クラスターに影響を与えるセキュリティー情報のリストはどこにありますか?
{: #faq_security_bulletins}
{: faq}

Kubernetes で脆弱性が検出されると、Kubernetes は CVE をセキュリティー情報にリリースして、ユーザーに通知し、脆弱性に対処するためにユーザーが取る必要のある処置について説明します。 {{site.data.keyword.containerlong_notm}} ユーザーや {{site.data.keyword.Bluemix_notm}} プラットフォームに影響のある Kubernetes のセキュリティー情報は、[{{site.data.keyword.Bluemix_notm}} のセキュリティー情報](https://cloud.ibm.com/status?component=containers-kubernetes&selected=security)で公開されています。

一部の CVE では、{{site.data.keyword.containerlong_notm}} 内の定期的な[クラスター更新プロセス](/docs/containers?topic=containers-update#update)の一部としてインストールできる、Kubernetes バージョンの最新パッチ更新が必要になります。 悪意のある攻撃からクラスターを保護するには、適時にセキュリティー・パッチを適用するようにしてください。 セキュリティー・パッチの内容に関する情報については、[バージョンの変更ログ](/docs/containers?topic=containers-changelog#changelog)を参照してください。

## このサービスはベア・メタルと GPU をサポートしていますか?
{: #bare_metal_gpu}
{: faq}

はい。ワーカー・ノードを単一テナントの物理ベア・メタル・サーバーとしてプロビジョンできます。 ベア・メタル・サーバーは、データ、AI、GPU などのワークロードで高性能を発揮します。 また、すべてのハードウェア・リソースがお客様のワークロード専用になるので、「ノイジー・ネイバー」に関する問題がありません。

使用可能なベア・メタル・フレーバー、およびベア・メタルと仮想マシンの違いについて詳しくは、[物理マシン (ベア・メタル)](/docs/containers?topic=containers-planning_worker_nodes#bm) を参照してください。

## このサービスは、どの Kubernetes バージョンをサポートしていますか?
{: #supported_kube_versions}
{: faq}

{{site.data.keyword.containerlong_notm}} は、複数のバージョンの Kubernetes を同時にサポートします。 最新バージョン (n) がリリースされると、2 つ前のバージョン (n-2) までサポートされます。 最新バージョンから 2 つより前のバージョン (n-3) は、まず非推奨になり、その後サポートされなくなります。 現在、以下のバージョンがサポートされています。

*   最新: 1.14.2
*   デフォルト: 1.13.6
*   その他: 1.12.9

サポートされるバージョンと、バージョンの移行のために実行する必要がある更新操作について詳しくは、[バージョン情報および更新操作](/docs/containers?topic=containers-cs_versions#cs_versions)を参照してください。

## このサービスはどの地域で利用できますか?
{: #supported_regions}
{: faq}

{{site.data.keyword.containerlong_notm}} は世界中で利用できます。 標準クラスターは、サポートされるすべての {{site.data.keyword.containerlong_notm}} 地域で作成できます。 フリー・クラスターは、選択地域でのみ使用可能です。

サポートされている地域について詳しくは、[ロケーション](/docs/containers?topic=containers-regions-and-zones#regions-and-zones)を参照してください。

## サービスはどのような標準に準拠していますか?
{: #standards}
{: faq}

{{site.data.keyword.containerlong_notm}} は、以下の標準に対応した制御を実装しています。
- EU - 米国間のプライバシー・シールドおよびスイス - 米国間のプライバシー・シールド・フレームワーク
- 医療保険の相互運用性と説明責任に関する法令 (HIPAA)
- 受託会社の内部統制基準 (SOC 1、SOC 2 Type 1)
- 国際保証業務基準 3402 (ISAE 3402)、受託業務に係る内部統制の保証報告書
- 国際標準化機構 (ISO 27001、ISO 27017、ISO 27018)
- クレジット・カード業界のデータ・セキュリティー基準 (PCI DSS)

## IBM Cloud と他のサービスをクラスターで使用できますか?
{: #faq_integrations}
{: faq}

{{site.data.keyword.Bluemix_notm}} のプラットフォームおよびインフラストラクチャー・サービスとサード・パーティー・ベンダーのサービスを {{site.data.keyword.containerlong_notm}} クラスターに追加して、自動化を実現したり、セキュリティーを強化したり、クラスターのモニター機能とロギング機能を拡張したりできます。

サポートされるサービスのリストについては、[サービスの統合](/docs/containers?topic=containers-supported_integrations#supported_integrations)を参照してください。

## IBM Cloud Public のクラスターを、オンプレミス・データ・センターで実行されるアプリと接続できますか?
{: #hybrid}
{: faq}

{{site.data.keyword.Bluemix_notm}} Public のサービスをオンプレミス・データ・センターに接続して、独自のハイブリッド・クラウド・セットアップを作成できます。 オンプレミス・データ・センターで実行されるアプリと {{site.data.keyword.Bluemix_notm}} Public および Private を利用する方法の例は、以下のとおりです。
- {{site.data.keyword.Bluemix_notm}} Public で {{site.data.keyword.containerlong_notm}} を使用してクラスターを作成するが、そのクラスターをオンプレミス・データベースに接続する必要がある。
- 独自のデータ・センターの {{site.data.keyword.Bluemix_notm}} Private で Kubernetes クラスターを作成し、そのクラスターにアプリをデプロイする。 ただし、そのアプリは {{site.data.keyword.Bluemix_notm}} Public の {{site.data.keyword.ibmwatson_notm}} サービス (Tone Analyzer など) を使用する可能性がある。

{{site.data.keyword.Bluemix_notm}} Public で実行されるサービスと、オンプレミスで実行されるサービスの間で通信できるようにするには、[VPN 接続をセットアップする](/docs/containers?topic=containers-vpn#vpn)必要があります。 {{site.data.keyword.Bluemix_notm}} Public または Dedicated 環境と {{site.data.keyword.Bluemix_notm}} Private 環境を接続するには、[{{site.data.keyword.Bluemix_notm}} Private での {{site.data.keyword.containerlong_notm}} の使用](/docs/containers?topic=containers-hybrid_iks_icp#hybrid_iks_icp)を参照してください。

サポートされる {{site.data.keyword.containerlong_notm}} オファリングの概要については、[オファーとそれらの組み合わせの比較](/docs/containers?topic=containers-cs_ov#differentiation)を参照してください。

## 独自のデータ・センターに IBM Cloud Kubernetes Service をデプロイできますか?
{: #private}
{: faq}

アプリを {{site.data.keyword.Bluemix_notm}} Public に移行したくはないが、{{site.data.keyword.containerlong_notm}} の機能は利用したいという場合は、{{site.data.keyword.Bluemix_notm}} Private をインストールすることができます。 {{site.data.keyword.Bluemix_notm}} Private は、お客様のマシンにローカルにインストールできるアプリケーション・プラットフォームです。これを利用すると、ファイアウォールの内側にある自分で管理できる環境で、オンプレミスのコンテナー化アプリを開発して管理することができます。

詳しくは、[{{site.data.keyword.Bluemix_notm}} Private の製品資料![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_1.2.0/kc_welcome_containers.html)を参照してください。

## IBM Cloud Kubernetes Service を使用した場合は何が課金されますか?
{: #charges}
{: faq}

{{site.data.keyword.containerlong_notm}} クラスターを使用して、Watson AI や Compose Database-as-a-Service などのプラットフォーム・サービスで、IBM Cloud インフラストラクチャー (SoftLayer) のコンピュート・リソース、ネットワーキング・リソース、およびストレージ・リソースを使用できます。 リソースごとに、[定額制、従量制、段階制、予約制](/docs/billing-usage?topic=billing-usage-charges#charges)の料金が個々に課金される場合があります。
* [ワーカー・ノード](#nodes)
* [アウトバウンド・ネットワーキング](#bandwidth)
* [サブネット IP アドレス](#subnet_ips)
* [ストレージ](#persistent_storage)
* [{{site.data.keyword.Bluemix_notm}} サービス](#services)
* [Red Hat OpenShift on IBM Cloud](#rhos_charges)

<dl>
<dt id="nodes">ワーカー・ノード</dt>
  <dd><p>クラスターには、仮想マシンと物理 (ベアメタル) マシンという 2 つの主なワーカー・ノード・タイプがあります。 マシン・タイプの可用性および価格設定は、クラスターのデプロイ先のゾーンによって異なります。</p>
  <p><strong>仮想マシン</strong>の特徴は、ベアメタルと比較して柔軟性が高く、プロビジョニング時間が短く、自動スケーラビリティー機能が多い点で、これらをベアメタルよりも高いコスト効率の価格で提供します。 ただし、VM は、ベアメタルの仕様と比較して、ネットワーキング Gbps、RAM およびメモリーのしきい値、ストレージ・オプションなど、パフォーマンス上のトレードオフがあります。 VM のコストに影響を与える以下のような要因に留意してください。</p>
  <ul><li><strong>共有と専用</strong>: VM の基礎となるハードウェアを共有する場合、専用ハードウェアよりコストは低くなりますが、物理リソースはご使用の VM 専用ではありません。</li>
  <li><strong>時間単位の課金のみ</strong>: 時間単位の場合、より柔軟に VM の迅速な注文やキャンセルを行うことができます。
  <li><strong>月ごとの段階制の時間</strong>: 時間単位の課金は段階制です。 課金月内の時間階層に対して VM は注文されたままになるほど、時間単位の課金率が低下します。 時間の階層には、0 - 150 時間、151 - 290 時間、291 - 540 時間、および 541 時間以上があります。</li></ul>
  <p><strong>物理マシン (ベアメタル)</strong> は、データ、AI、GPU などのワークロードで高性能を発揮します。 また、すべてのハードウェア・リソースは独自のワークロード専用なので、「ノイジー・ネイバー」はありません。 ベアメタルのコストに影響を与える以下のような要因に留意してください。</p>
  <ul><li><strong>月単位の課金のみ</strong>: すべてのベアメタルは月単位で課金されます。</li>
  <li><strong>注文処理に時間がかかる</strong>: お客様がベアメタル・サーバーを注文またはキャンセルした後に、IBM Cloud インフラストラクチャー (SoftLayer) アカウントの処理が手動で実行されます。 そのため、完了するまでに 1 営業日以上かかる場合があります。</li></ul>
  <p>マシンの仕様について詳しくは、[ワーカー・ノードに使用可能なハードウェア](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)を参照してください。</p></dd>

<dt id="bandwidth">パブリック帯域幅</dt>
  <dd><p>帯域幅とは、世界中のデータ・センター内の {{site.data.keyword.Bluemix_notm}} リソース間で送受信される、インバウンドおよびアウトバウンドのネットワーク・トラフィックのパブリック・データ転送を指します。 パブリック帯域幅は、GB 単位で課金されます。 現行帯域幅のサマリーを確認するには、[{{site.data.keyword.Bluemix_notm}} コンソール](https://cloud.ibm.com/)にログインし、メニュー ![メニュー・アイコン](../icons/icon_hamburger.svg "メニュー・アイコン") から**「クラシック・インフラストラクチャー」**を選択し、**「ネットワーク」>「帯域幅」>「サマリー」**ページを選択します。
  <p>パブリック帯域幅の課金に影響を与える以下のような要因を確認してください。</p>
  <ul><li><strong>場所</strong>: ワーカー・ノードと同様に、課金はリソースのデプロイ先のゾーンに応じて異なります。</li>
  <li><strong>組み込み帯域幅または従量課金</strong>: ワーカー・ノード・マシンは、1 カ月当たりのアウトバウンド・ネットワーキングの特定の割り振りが付属している場合があります (例えば、VM の 250 GB、ベアメタルの 500 GB)。 または、割り振りは、GB 使用量に基づく従量課金の場合があります。</li>
  <li><strong>段階制パッケージ</strong>: 組み込み帯域幅を超過した後、場所によって異なる段階制使用量方式に従って課金されます。 段階制割り振りを超過すると、標準データ転送料金も課金される場合があります。</li></ul>
  <p>詳しくは、[Bandwidth packages ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/cloud/bandwidth) を参照してください。</p></dd>

<dt id="subnet_ips">サブネット IP アドレス</dt>
  <dd><p>標準クラスターを作成すると、8 つのパブリック IP アドレスを持つポータブル・パブリック・サブネットが注文され、月単位でアカウントに課金されます。</p><p>インフラストラクチャー・アカウントに使用可能なサブネットが既にある場合は、これらのサブネットを代わりに使用できます。 クラスターを `--no-subnets` [フラグ](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_create)を使用して作成し、[それらのサブネットを再使用します](/docs/containers?topic=containers-subnets#subnets_custom)。</p>
  </dd>

<dt id="persistent_storage">ストレージ</dt>
  <dd>ストレージをプロビジョンする場合、ユース・ケースに適切なストレージ・タイプとストレージ・クラスを選択できます。 課金は、ストレージ・タイプ、場所、およびストレージ・インスタンスの仕様に応じて異なります。 ファイル・ストレージやブロック・ストレージなどの一部のストレージ・ソリューションでは、時間単位と月単位のプランから選択することができます。 適切なストレージ・ソリューションを選択するには、[可用性の高い永続ストレージの計画](/docs/containers?topic=containers-storage_planning#storage_planning)を参照してください。 詳しくは、以下を参照してください。
  <ul><li>[NFS ファイル・ストレージの価格設定 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/cloud/file-storage/pricing)</li>
  <li>[ブロック・ストレージの価格設定 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/cloud/block-storage/pricing)</li>
  <li>[オブジェクト・ストレージ・プラン ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api)</li></ul></dd>

<dt id="services">{{site.data.keyword.Bluemix_notm}}services</dt>
  <dd>クラスターと統合する各サービスには、それぞれ独自の価格設定モデルがあります。 各製品資料を確認し、{{site.data.keyword.Bluemix_notm}} コンソールを使用して、[コストを見積もってください](/docs/billing-usage?topic=billing-usage-cost#cost)。</dd>

<dt id="rhos_charges">Red Hat OpenShift on IBM Cloud</dt>
  <dd>
  <p class="preview">[Red Hat OpenShift on IBM Cloud](/docs/containers?topic=containers-openshift_tutorial) は、OpenShift クラスターをテストするためのベータ版として使用できます。</p>[Red Hat OpenShift on IBM Cloud クラスター](/docs/containers?topic=containers-openshift_tutorial)を作成すると、ワーカー・ノードが Red Hat Enterprise Linux オペレーティング・システムとともにインストールされるため、[ワーカー・ノード・マシン](#nodes)の価格が高くなります。OpenShift ライセンスも必要です。これにより、時間単位の VM コストまたは月単位のベアメタル・コストに加えて、月単位のコストが発生します。OpenShift ライセンスは、ワーカー・ノード・フレーバーの 2 つのコアごとに必要です。月末より前にワーカー・ノードを削除した場合、使用するワーカー・プール内の他のワーカー・ノードで月次ライセンスを使用できます。OpenShift クラスターについて詳しくは、[Red Hat OpenShift on IBM Cloud クラスターの作成](/docs/containers?topic=containers-openshift_tutorial) を参照してください。</dd>

</dl>
<br><br>

月単位のリソースは、月初めをベースにして前月の使用量について課金されます。 月の半ばに月単位のリソースを注文した場合、その月については日割り計算した金額が課金されます。 ただし、月の半ばにリソースをキャンセルした場合でも、月単位のリソースの 1 カ月分全額が課金されます。
{: note}

## 使用するプラットフォームおよびインフラストラクチャーのリソースの請求は 1 つにまとめられますか?
{: #bill}
{: faq}

有料の {{site.data.keyword.Bluemix_notm}} アカウントをご利用の場合、プラットフォームおよびインフラストラクチャーのリソースの請求は 1 つにまとめられます。
{{site.data.keyword.Bluemix_notm}} アカウントと IBM Cloud インフラストラクチャー (SoftLayer) アカウントをリンクしている場合、{{site.data.keyword.Bluemix_notm}} プラットフォームおよびインフラストラクチャーのリソースに対する[統合請求](/docs/customer-portal?topic=customer-portal-unifybillaccounts#unifybillaccounts)を受け取ることになります。

## コストの見積もりは可能ですか?
{: #cost_estimate}
{: faq}

はい。[コストの見積もり](/docs/billing-usage?topic=billing-usage-cost#cost)を参照してください。 使用時間に応じた段階制の価格設定など、見積もりに反映されない料金があることに注意してください。 詳しくは、[{{site.data.keyword.containerlong_notm}} 使用時の課金内容は?](#charges)を参照してください。

## 現在の使用量を確認できますか?
{: #usage}
{: faq}

{{site.data.keyword.Bluemix_notm}} プラットフォームおよびインフラストラクチャーのリソースの現在の使用量と、月額合計の見積もりを確認できます。 詳しくは、[使用量の表示](/docs/billing-usage?topic=billing-usage-viewingusage#viewingusage)を参照してください。 請求を編成するために、ご使用のリソースを[リソース・グループ](/docs/resources?topic=resources-bp_resourcegroups#bp_resourcegroups)にグループ化することができます。
