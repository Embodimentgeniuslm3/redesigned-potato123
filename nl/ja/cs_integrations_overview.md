---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: kubernetes, iks, helm

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
{:preview: .preview}フィ


# サポートされる IBM Cloud とサード・パーティーの統合
{: #supported_integrations}

{{site.data.keyword.containerlong}} の標準の Kubernetes クラスターでは、さまざまな外部サービスとカタログ・サービスを使用できます。
{:shortdesc}

## 一般的な統合
{: #popular_services}

<table summary="この表は、クラスターに追加できる、{{site.data.keyword.containerlong_notm}} ユーザーの間でよく利用される使用可能なサービスを示しています。行は左から右に読み、1 列目はサービスの名前、2 列目はサービスの説明です。">
<caption>一般的なサービス</caption>
<thead>
<tr>
<th>サービス</th>
<th>カテゴリー</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.cloudaccesstrailfull}}</td>
<td>クラスター・アクティビティー・ログ</td>
<td>Grafana を使用してログを分析し、クラスター内で実行された管理アクティビティーをモニターします。 このサービスについて詳しくは、[Activity Tracker](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started) の資料を参照してください。 追跡できるイベントのタイプについて詳しくは、[Activity Tracker イベント](/docs/containers?topic=containers-at_events)を参照してください。</td>
</tr>
<tr>
<td>{{site.data.keyword.appid_full}}</td>
<td>認証</td>
<td>ユーザーに対してサインインを要求することにより、[{{site.data.keyword.appid_short}}](/docs/services/appid?topic=appid-getting-started) によって、アプリのセキュリティー・レベルを強化します。 アプリに対する Web または API の HTTP /HTTPS 要求を認証するために、[{{site.data.keyword.appid_short_notm}} 認証 Ingress アノテーション](/docs/containers?topic=containers-ingress_annotation#appid-auth)を使用して、{{site.data.keyword.appid_short_notm}} を Ingress サービスと統合できます。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix}} Block Storage</td>
<td>ブロック・ストレージ</td>
<td>[{{site.data.keyword.Bluemix_notm}} Block Storage](/docs/infrastructure/BlockStorage?topic=BlockStorage-getting-started#getting-started) は、Kubernetes 永続ボリューム (PV) を使用してアプリに追加できる高性能永続 iSCSI ストレージです。 　ブロック・ストレージは、単一ゾーンにステートフル・アプリをデプロイする場合に使用したり、単一ポッド用の高性能ストレージとして使用したりします。 クラスターにブロック・ストレージをプロビジョンする方法について詳しくは、[{{site.data.keyword.Bluemix_notm}} Block Storage にデータを格納する](/docs/containers?topic=containers-block_storage#block_storage) を参照してください。</td>
</tr>
<tr>
<td>{{site.data.keyword.cloudcerts_full}}</td>
<td>TLS 証明書</td>
<td><a href="/docs/services/certificate-manager?topic=certificate-manager-getting-started#getting-started" target="_blank">{{site.data.keyword.cloudcerts_long}} <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を使用して、アプリの SSL 証明書を保管および管理できます。 詳しくは、<a href="https://www.ibm.com/blogs/bluemix/2018/01/use-ibm-cloud-certificate-manager-ibm-cloud-container-service-deploy-custom-domain-tls-certificates/" target="_blank">Use {{site.data.keyword.cloudcerts_long_notm}} with {{site.data.keyword.containerlong_notm}} to deploy custom domain TLS Certificates <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を参照してください。 </td>
</tr>
<tr>
<td>{{site.data.keyword.registrylong}}</td>
<td>コンテナー・イメージ</td>
<td>保護された独自の Docker イメージ・リポジトリーをセットアップします。そこでイメージを安全に保管し、クラスター・ユーザー間で共有することができます。 詳しくは、<a href="/docs/services/Registry?topic=registry-getting-started" target="_blank">{{site.data.keyword.registrylong}} の資料 <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を参照してください。</td>
</tr>
<tr>
<td>{{site.data.keyword.contdelivery_full}}</td>
<td>ビルド自動化</td>
<td>アプリのビルドと Kubernetes クラスターへのコンテナーのデプロイメントを、ツールチェーンを使用して自動化します。 セットアップについて詳しくは、<a href="https://developer.ibm.com/recipes/tutorials/deploy-kubernetes-pods-to-the-bluemix-container-service-using-devops-pipelines/" target="_blank">Deploy Kubernetes pods to the {{site.data.keyword.containerlong_notm}} using DevOps Pipelines <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> というブログを参照してください。 </td>
</tr>
<tr>
<td>{{site.data.keyword.datashield_full}} (ベータ版)</td>
<td>メモリー暗号化</td>
<td><a href="/docs/services/data-shield?topic=data-shield-getting-started#getting-started" target="_blank">{{site.data.keyword.datashield_short}} <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を使用して、データ・メモリーを暗号化できます。 {{site.data.keyword.datashield_short}} は、インテル® Software Guard Extensions (SGX) および Fortanix® テクノロジーと統合されているため、使用中の {{site.data.keyword.Bluemix_notm}} コンテナーのワークロードのコードとデータを保護できます。 アプリのコードとデータは、CPU で保護されたエンクレーブで実行されます。エンクレーブは、ワーカー・ノード上の信頼できるメモリー領域であり、ここでアプリの重要な側面を保護することで、コードとデータの機密を保ち、改ざんを防止できます。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix}} File Storage</td>
<td>ファイル・ストレージ</td>
<td>[{{site.data.keyword.Bluemix_notm}} File Storage](/docs/infrastructure/FileStorage?topic=FileStorage-getting-started#getting-started) は、NFS ベースの高速で柔軟なネットワーク接続型永続ファイル・ストレージであり、Kubernetes 永続ボリュームを使用してアプリに追加することができます。 ワークロードの要件を満たす GB サイズと IOPS を考慮して、事前定義されたストレージ層の中から選択できます。 クラスターにファイル・ストレージをプロビジョンする方法について詳しくは、[{{site.data.keyword.Bluemix_notm}} File Storage にデータを格納する](/docs/containers?topic=containers-file_storage#file_storage) を参照してください。</td>
</tr>
<tr>
<td>{{site.data.keyword.keymanagementservicefull}}</td>
<td>データ暗号化</td>
<td>{{site.data.keyword.keymanagementserviceshort}} を有効にして、クラスター内にある Kubernetes シークレットを暗号化します。 Kubernetes シークレットを暗号化すると、許可されていないユーザーがクラスターの機密情報にアクセスできなくなります。<br>セットアップするには、<a href="/docs/containers?topic=containers-encryption#keyprotect">{{site.data.keyword.keymanagementserviceshort}} を使用した Kubernetes シークレットの暗号化</a>を参照してください。<br>詳しくは、<a href="/docs/services/key-protect?topic=key-protect-getting-started-tutorial" target="_blank">{{site.data.keyword.keymanagementserviceshort}} の資料 <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を参照してください。</td>
</tr>
<tr>
<td>{{site.data.keyword.la_full}}</td>
<td>クラスターおよびアプリのログ</td>
<td>LogDNA をサード・パーティー・サービスとしてワーカー・ノードにデプロイし、ポッド・コンテナーのログを管理することで、ログ管理機能をクラスターに追加できます。 詳しくは、[{{site.data.keyword.loganalysisfull_notm}} with LogDNA による Kubernetes クラスター・ログの管理](/docs/services/Log-Analysis-with-LogDNA/tutorials?topic=LogDNA-kube#kube) を参照してください。</td>
</tr>
<tr>
<td>{{site.data.keyword.mon_full}}</td>
<td>クラスターおよびアプリのメトリック</td>
<td>Sysdig をサード・パーティー・サービスとしてワーカー・ノードにデプロイし、メトリックを {{site.data.keyword.monitoringlong}} に転送することで、アプリのパフォーマンスと正常性を可視化して運用することができます。 詳しくは、[Kubernetes クラスターにデプロイされたアプリのメトリックの分析方法](/docs/services/Monitoring-with-Sysdig/tutorials?topic=Sysdig-kubernetes_cluster#kubernetes_cluster)を参照してください。 </td>
</tr>
<tr>
<td>{{site.data.keyword.cos_full}}</td>
<td>オブジェクト・ストレージ</td>
<td>{{site.data.keyword.cos_short}} で保管するデータは、暗号化され、複数の地理的ロケーションに分散され、REST API を使用して HTTP によってアクセスされます。 [ibm-backup-restore image](/docs/services/RegistryImages/ibm-backup-restore?topic=RegistryImages-ibmbackup_restore_starter) を使用して、クラスターのデータの一回限りのバックアップ、またはスケジュールしたバックアップを実行するようにサービスを構成できます。 サービスについて詳しくは、<a href="/docs/services/cloud-object-storage?topic=cloud-object-storage-about" target="_blank">{{site.data.keyword.cos_short}}の資料 <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を参照してください。</td>
</tr>
<tr>
<td>{{site.data.keyword.containerlong_notm}} 上での Istio</td>
<td>マイクロサービス管理</td>
<td><a href="https://www.ibm.com/cloud/info/istio" target="_blank">Istio <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> は 、クラウド・オーケストレーション・プラットフォームでマイクロサービス・ネットワーク (別名、サービス・メッシュ) の接続、保護、管理、モニターを行うための方法を開発者に提供するオープン・ソース・サービスです。 {{site.data.keyword.containerlong}} 上の Istio は、管理対象アドオンを介したクラスターへの Istio のワンステップ・インストールを提供します。 ワンクリックですべての Istio コア・コンポーネントを取得し、追加のトレース、モニタリング、視覚化を行い、BookInfo サンプル・アプリを稼働状態にすることができます。 開始するには、[管理対象 Istio アドオン (ベータ版) の使用](/docs/containers?topic=containers-istio) を参照してください。</td>
</tr>
<tr>
<td>Knative</td>
<td>サーバーレス・アプリ</td>
<td>[Knative ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/knative/docs) は IBM、Google、Pivotal、Red Hat、Cisco などによって開発されたオープン・ソースのプラットフォームであり、Kubernetes の機能を拡張して Kubernetes クラスター上にソース中心の最新のコンテナー化サーバーレス・アプリを作成できるようにすることを目的としています。 このプラットフォームは、Kubernetes におけるビルドの操作負担およびワークロードのデプロイと管理の抽象化に関して、さまざまなプログラミング言語とフレームワークに対して一貫性のあるアプローチを取っているので、開発者は最も重要なもの、つまりソース・コードに集中することができます。 詳しくは、[Knative を使用したサーバーレス・アプリのデプロイ](/docs/containers?topic=containers-serverless-apps-knative)を参照してください。</td>
</tr>
<tr>
<td>Portworx</td>
<td>ステートフル・アプリのストレージ</td>
<td>[Portworx ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://portworx.com/products/introduction/) は、可用性の高いソフトウェア定義のストレージ・ソリューションです。これを使用して、コンテナー化データベースなどのステートフル・アプリ用の永続ストレージを管理したり、複数のゾーンにわたってポッド間でデータを共有したりすることができます。 Helm チャートを使用して Portworx をインストールし、Kubernetes 永続ボリュームを使用してアプリ用ストレージをプロビジョンできます。 クラスターに Portworx をセットアップする方法について詳しくは、[Portworx を使ってソフトウェア定義のストレージ (SDS) にデータを格納する](/docs/containers?topic=containers-portworx#portworx) を参照してください。</td>
</tr>
<tr>
<td>Razee</td>
<td>デプロイメント自動化</td>
<td>[Razee ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://razee.io/) は、クラスター、環境、およびクラウド・プロバイダー間で Kubernetes リソースのデプロイメントを自動化して管理するオープン・ソース・プロジェクトであり、ロールアウト・プロセスをモニターしてより素早くデプロイメントに関する問題を検出できるよう、リソースのデプロイメント情報を視覚化するのに役立ちます。Razee およびクラスターで Razee をセットアップしてデプロイメント・プロセスを自動化する方法については、[Razee の資料 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/razee-io/Razee) を参照してください。</td>
</tr>
</tbody>
</table>

<br />


## DevOps サービス
{: #devops_services}

<table summary="この表は、クラスターに追加してさらに DevOps 機能を追加するために使用できるサービスを示しています。行は左から右に読み、1 列目はサービスの名前、2 列目はサービスの説明です。">
<caption>DevOps サービス</caption>
<thead>
<tr>
<th>サービス</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.cfee_full_notm}}</td>
<td>Kubernetes クラスターの上に独自の Cloud Foundry プラットフォームをデプロイして管理できます。これにより、クラウド・ネイティブ・アプリを開発、パッケージ、デプロイ、管理し、{{site.data.keyword.Bluemix_notm}} エコシステムを活用して追加のサービスをアプリにバインドできるようになります。 {{site.data.keyword.cfee_full_notm}} インスタンスを作成するときは、ワーカー・ノードのマシン・タイプと VLAN を選択して Kubernetes クラスターを構成する必要があります。 こうすることで、クラスターに {{site.data.keyword.containerlong_notm}} がプロビジョンされ、{{site.data.keyword.cfee_full_notm}} が自動的にクラスターにデプロイされます。 {{site.data.keyword.cfee_full_notm}} のセットアップ方法について詳しくは、[入門チュートリアル](/docs/cloud-foundry?topic=cloud-foundry-getting-started#getting-started)を参照してください。 </td>
</tr>
<tr>
<td>Codeship</td>
<td>コンテナーの継続的な統合とデリバリーのために <a href="https://codeship.com" target="_blank">Codeship <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を使用できます。 詳しくは、<a href="https://www.ibm.com/blogs/bluemix/2017/10/using-codeship-pro-deploy-workloads-ibm-container-service/" target="_blank">Using Codeship Pro To Deploy Workloads to {{site.data.keyword.containerlong_notm}} <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を参照してください。 </td>
</tr>
<tr>
<td>Grafeas</td>
<td>[Grafeas ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://grafeas.io) は、ソフトウェア・サプライ・チェーンの過程でメタデータを取得、保管、交換する方法のための一般的な手段を提供するオープン・ソースの CI/CD サービスです。 例えば、Grafeas をアプリ・ビルド・プロセスに組み込むと、ビルド要求のイニシエーター、脆弱点スキャン結果、品質保証サインオフに関する情報を Grafeas が保管できるので、アプリを実動にデプロイできるかどうかを情報に基づいて決定できるようになります。 このメタデータを監査で使用したり、このメタデータを使用してソフトウェア・サプライ・チェーンのコンプライアンスを実証したりすることができます。 </td>
</tr>
<tr>
<td>Helm</td>
<td> <a href="https://helm.sh" target="_blank">Helm <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> は Kubernetes パッケージ・マネージャーです。 新しい Helm チャートを作成するか、既存の Helm チャートを使用して、{{site.data.keyword.containerlong_notm}} クラスターで実行される複雑な Kubernetes アプリケーションの定義、インストール、アップグレードを行うことができます。 <p>詳しくは、[{{site.data.keyword.containerlong_notm}} での Helm のセットアップ](/docs/containers?topic=containers-helm)を参照してください。</p></td>
</tr>
<tr>
<td>{{site.data.keyword.contdelivery_full}}</td>
<td>アプリのビルドと Kubernetes クラスターへのコンテナーのデプロイメントを、ツールチェーンを使用して自動化します。 セットアップについて詳しくは、<a href="https://developer.ibm.com/recipes/tutorials/deploy-kubernetes-pods-to-the-bluemix-container-service-using-devops-pipelines/" target="_blank">Deploy Kubernetes pods to the {{site.data.keyword.containerlong_notm}} using DevOps Pipelines <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> というブログを参照してください。</td>
</tr>
<tr>
<td>{{site.data.keyword.containerlong_notm}} 上での Istio</td>
<td><a href="https://www.ibm.com/cloud/info/istio" target="_blank">Istio <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> は 、クラウド・オーケストレーション・プラットフォームでマイクロサービス・ネットワーク (別名、サービス・メッシュ) の接続、保護、管理、モニターを行うための方法を開発者に提供するオープン・ソース・サービスです。 {{site.data.keyword.containerlong}} 上の Istio は、管理対象アドオンを介したクラスターへの Istio のワンステップ・インストールを提供します。 ワンクリックですべての Istio コア・コンポーネントを取得し、追加のトレース、モニタリング、視覚化を行い、BookInfo サンプル・アプリを稼働状態にすることができます。 開始するには、[管理対象 Istio アドオン (ベータ版) の使用](/docs/containers?topic=containers-istio) を参照してください。</td>
</tr>
<tr>
<td>Jenkins X</td>
<td>Jenkins X は Kubernetes ネイティブの継続的統合と継続的デリバリーのプラットフォームであり、これを使用してビルド・プロセスを自動化できます。 {{site.data.keyword.containerlong_notm}} へのインストール方法について詳しくは、[Introducing the Jenkins X open source project ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/blogs/bluemix/2018/08/installing-jenkins-x-on-ibm-cloud-kubernetes-service/) を参照してください。</td>
</tr>
<tr>
<td>Knative</td>
<td>[Knative ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/knative/docs) は IBM、Google、Pivotal、Red Hat、Cisco などによって開発されたオープン・ソースのプラットフォームであり、Kubernetes の機能を拡張して Kubernetes クラスター上にソース中心の最新のコンテナー化サーバーレス・アプリを作成できるようにすることを目的としています。 このプラットフォームは、Kubernetes におけるビルドの操作負担およびワークロードのデプロイと管理の抽象化に関して、さまざまなプログラミング言語とフレームワークに対して一貫性のあるアプローチを取っているので、開発者は最も重要なもの、つまりソース・コードに集中することができます。 詳しくは、[Knative を使用したサーバーレス・アプリのデプロイ](/docs/containers?topic=containers-serverless-apps-knative)を参照してください。</td>
</tr>
<tr>
<td>Razee</td>
<td>[Razee ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://razee.io/) は、クラスター、環境、およびクラウド・プロバイダー間で Kubernetes リソースのデプロイメントを自動化して管理するオープン・ソース・プロジェクトであり、ロールアウト・プロセスをモニターしてより素早くデプロイメントに関する問題を検出できるよう、リソースのデプロイメント情報を視覚化するのに役立ちます。Razee およびクラスターで Razee をセットアップしてデプロイメント・プロセスを自動化する方法については、[Razee の資料 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/razee-io/Razee) を参照してください。</td>
</tr>
</tbody>
</table>

<br />


## ハイブリッド・クラウド・サービス
{: #hybrid_cloud_services}

<table summary="この表は、クラスターをオンプレミス・データ・センターに接続する際に使用できるサービスを示しています。行は左から右に読み、1 列目はサービスの名前、2 列目はサービスの説明です。">
<caption>ハイブリッド・クラウド・サービス</caption>
<thead>
<tr>
<th>サービス</th>
<th>説明</th>
</tr>
</thead>
<tbody>
  <tr>
    <td>{{site.data.keyword.BluDirectLink}}</td>
    <td>[{{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-about-ibm-cloud-direct-link) を使用すると、パブリック・インターネット経由でルーティングせずに、リモート・ネットワーク環境と {{site.data.keyword.containerlong_notm}} の間にプライベート接続を直接作成できます。{{site.data.keyword.Bluemix_notm}} Direct Link オファリングは、ハイブリッド・ワークロード、プロバイダー間ワークロード、大規模なデータ転送や頻繁なデータ転送、またはプライベート・ワークロードを実装する必要がある場合に役立ちます。{{site.data.keyword.Bluemix_notm}} Direct Link オファリングを選択し、{{site.data.keyword.Bluemix_notm}} Direct Link 接続をセットアップするには、{{site.data.keyword.Bluemix_notm}} Direct Link の資料の [{{site.data.keyword.Bluemix_notm}}Direct Link での作業の開始](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link#how-do-i-know-which-type-of-ibm-cloud-direct-link-i-need-)を参照してください。</td>
  </tr>
<tr>
  <td>strongSwan IPSec VPN サービス</td>
  <td>Kubernetes クラスターをオンプレミス・ネットワークと安全に接続する [strongSwan IPSec VPN サービス ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.strongswan.org/about.html) をセットアップします。 strongSwan IPSec VPN サービスは、業界標準の Internet Protocol Security (IPSec) プロトコル・スイートに基づき、インターネット上にセキュアなエンドツーエンドの通信チャネルを確立します。 クラスターとオンプレミス・ネットワークの間にセキュアな接続をセットアップするためには、クラスター内のポッドに直接、[strongSwan IPSec VPN サービスを構成してデプロイします](/docs/containers?topic=containers-vpn#vpn-setup)。</td>
  </tr>
  </tbody>
  </table>

<br />


## サービスのロギングとモニタリング
{: #health_services}
<table summary="この表は、クラスターに追加してさらにロギング機能とモニタリング機能を追加するために使用できるサービスを示しています。行は左から右に読み、1 列目はサービスの名前、2 列目はサービスの説明です。">
<caption>サービスのロギングとモニタリング</caption>
<thead>
<tr>
<th>サービス</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>CoScale</td>
<td><a href="https://www.newrelic.com/coscale" target="_blank">CoScale <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を使用して、ワーカー・ノード、コンテナー、レプリカ・セット、レプリケーション・コントローラー、サービスをモニターします。 詳しくは、<a href="https://www.ibm.com/blogs/bluemix/2017/06/monitoring-ibm-bluemix-container-service-coscale/" target="_blank">Monitoring {{site.data.keyword.containerlong_notm}} with CoScale <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を参照してください。 </td>
</tr>
<tr>
<td>Datadog</td>
<td><a href="https://www.datadoghq.com/" target="_blank">Datadog <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を使用して、クラスターをモニターし、インフラストラクチャーとアプリケーションのパフォーマンス・メトリックを表示します。 詳しくは、<a href="https://www.ibm.com/blogs/bluemix/2017/07/monitoring-ibm-bluemix-container-service-datadog/" target="_blank">Monitoring {{site.data.keyword.containerlong_notm}} with Datadog <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を参照してください。 </td>
</tr>
<tr>
<td>{{site.data.keyword.cloudaccesstrailfull}}</td>
<td>Grafana を使用してログを分析し、クラスター内で実行された管理アクティビティーをモニターします。 このサービスについて詳しくは、[Activity Tracker](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started) の資料を参照してください。 追跡できるイベントのタイプについて詳しくは、[Activity Tracker イベント](/docs/containers?topic=containers-at_events)を参照してください。</td>
</tr>
<tr>
<td>{{site.data.keyword.la_full_notm}}</td>
<td>LogDNA をサード・パーティー・サービスとしてワーカー・ノードにデプロイし、ポッド・コンテナーのログを管理することで、ログ管理機能をクラスターに追加できます。 詳しくは、[{{site.data.keyword.loganalysisfull_notm}} with LogDNA による Kubernetes クラスター・ログの管理](/docs/services/Log-Analysis-with-LogDNA/tutorials?topic=LogDNA-kube#kube) を参照してください。</td>
</tr>
<tr>
<td>{{site.data.keyword.mon_full_notm}}</td>
<td>Sysdig をサード・パーティー・サービスとしてワーカー・ノードにデプロイし、メトリックを {{site.data.keyword.monitoringlong}} に転送することで、アプリのパフォーマンスと正常性を可視化して運用することができます。 詳しくは、[Kubernetes クラスターにデプロイされたアプリのメトリックの分析方法](/docs/services/Monitoring-with-Sysdig/tutorials?topic=Sysdig-kubernetes_cluster#kubernetes_cluster)を参照してください。 </td>
</tr>
<tr>
<td>Instana</td>
<td> <a href="https://www.instana.com/" target="_blank">Instana <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> は、アプリを自動的に検出してマップする GUI を使用して、インフラストラクチャーとアプリのパフォーマンス・モニターを提供します。 Instana はアプリに対するすべての要求をキャプチャーするので、その情報を使用してトラブルシューティングと根本原因分析を行い、問題の再発を防げるようになります。 詳しくは、<a href="https://www.instana.com/blog/precise-visibility-applications-ibm-bluemix-container-service/" target="_blank">{{site.data.keyword.containerlong_notm}} での Instana のデプロイ <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> に関するブログ投稿を参照してください。</td>
</tr>
<tr>
<td>Prometheus</td>
<td>Prometheus は、Kubernetes のために設計されたモニタリング、ロギング、アラートのためのオープン・ソースのツールです。 Prometheus は、Kubernetes のロギング情報に基づいて、クラスター、ワーカー・ノード、デプロイメントの正常性に関する詳細情報を取得します。 クラスターで実行されているコンテナーごとに、CPU、メモリー、I/O、ネットワーク・アクティビティーの情報が収集されます。 収集されたデータをカスタム・クエリーやアラートで使用して、クラスター内のパフォーマンスとワークロードをモニターできます。

<p>Prometheus を使用する場合は、<a href="https://github.com/coreos/prometheus-operator/tree/master/contrib/kube-prometheus" target="_blank">CoreOS の説明 <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> に従ってください。</p>
</td>
</tr>
<tr>
<td>Sematext</td>
<td><a href="https://sematext.com/" target="_blank">Sematext <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を使用して、コンテナー化アプリケーションのメトリックとログを表示します。 詳しくは、<a href="https://www.ibm.com/blogs/bluemix/2017/09/monitoring-logging-ibm-bluemix-container-service-sematext/" target="_blank">Monitoring and logging for containers with Sematext <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を参照してください。 </td>
</tr>
<tr>
<td>Splunk</td>
<td>Splunk Connect for Kubernetes を使用することにより、Splunk で Kubernetes のロギング・データ、オブジェクト・データ、メトリック・データをインポートおよび検索できます。 Splunk Connect for Kubernetes は、Splunk でサポートされる Fluentd のデプロイメントを Kubernetes クラスターにデプロイする Helm チャート、ログとメタデータを送信するための Splunk 組み込み Fluentd HTTP Event Collector (HEC) プラグイン、クラスター・メトリックを収集するするメトリック・デプロイメントのコレクションです。 詳しくは、<a href="https://www.ibm.com/blogs/bluemix/2019/02/solving-business-problems-with-splunk-on-ibm-cloud-kubernetes-service/" target="_blank">Solving Business Problems with Splunk on {{site.data.keyword.containerlong_notm}} <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を参照してください。</td>
</tr>
<tr>
<td>Weave Scope</td>
<td>[Weave Scope ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.weave.works/oss/scope/) は、Kubernetes クラスター内のリソース (サービス、ポッド、コンテナー、プロセス、ノードなど) のビジュアル図を表示します。 Weave Scope は CPU とメモリーのインタラクティブ・メトリックを示し、コンテナーの中で追尾したり実行したりするツールも備えています。</li></ol>
</td>
</tr>
</tbody>
</table>

<br />



## セキュリティー・サービス
{: #security_services}

{{site.data.keyword.Bluemix_notm}} セキュリティー・サービスをクラスターと統合する方法の包括的な説明が必要ですか? [Apply end-to-end security to a cloud application チュートリアル](/docs/tutorials?topic=solution-tutorials-cloud-e2e-security)を確認してください。
{: shortdesc}

<table summary="この表は、クラスターに追加してさらにセキュリティー機能を追加するために使用できるサービスを示しています。行は左から右に読み、1 列目はサービスの名前、2 列目はサービスの説明です。">
<caption>セキュリティー・サービス</caption>
<thead>
<tr>
<th>サービス</th>
<th>説明</th>
</tr>
</thead>
<tbody>
  <tr>
    <td>{{site.data.keyword.appid_full}}</td>
    <td>ユーザーに対してサインインを要求することにより、[{{site.data.keyword.appid_short}}](/docs/services/appid?topic=appid-getting-started) によって、アプリのセキュリティー・レベルを強化します。 アプリに対する Web または API の HTTP /HTTPS 要求を認証するために、[{{site.data.keyword.appid_short_notm}} 認証 Ingress アノテーション](/docs/containers?topic=containers-ingress_annotation#appid-auth)を使用して、{{site.data.keyword.appid_short_notm}} を Ingress サービスと統合できます。</td>
  </tr>
<tr>
<td>Aqua Security</td>
  <td><a href="/docs/services/va?topic=va-va_index" target="_blank">Vulnerability Advisor</a> を補完するために、<a href="https://www.aquasec.com/" target="_blank">Aqua Security <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を使用して、アプリで実行できる動作を減らすことで、コンテナー・デプロイメントのセキュリティーを強化できます。 詳しくは、<a href="https://blog.aquasec.com/securing-container-deployments-on-bluemix-with-aqua-security" target="_blank">Securing container deployments on {{site.data.keyword.Bluemix_notm}} with Aqua Security <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を参照してください。 </td>
</tr>
<tr>
<td>{{site.data.keyword.cloudcerts_full}}</td>
<td><a href="/docs/services/certificate-manager?topic=certificate-manager-getting-started#getting-started" target="_blank">{{site.data.keyword.cloudcerts_long}} <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を使用して、アプリの SSL 証明書を保管および管理できます。 詳しくは、<a href="https://www.ibm.com/blogs/bluemix/2018/01/use-ibm-cloud-certificate-manager-ibm-cloud-container-service-deploy-custom-domain-tls-certificates/" target="_blank">Use {{site.data.keyword.cloudcerts_long_notm}} with {{site.data.keyword.containerlong_notm}} to deploy custom domain TLS Certificates <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を参照してください。 </td>
</tr>
<tr>
  <td>{{site.data.keyword.datashield_full}} (ベータ版)</td>
  <td><a href="/docs/services/data-shield?topic=data-shield-getting-started#getting-started" target="_blank">{{site.data.keyword.datashield_short}} <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を使用して、データ・メモリーを暗号化できます。 {{site.data.keyword.datashield_short}} は、インテル® Software Guard Extensions (SGX) および Fortanix® テクノロジーと統合されているため、使用中の {{site.data.keyword.Bluemix_notm}} コンテナーのワークロードのコードとデータを保護できます。 アプリのコードとデータは、CPU で保護されたエンクレーブで実行されます。エンクレーブは、ワーカー・ノード上の信頼できるメモリー領域であり、ここでアプリの重要な側面を保護することで、コードとデータの機密を保ち、改ざんを防止できます。</td>
</tr>
<tr>
  <td>{{site.data.keyword.registrylong}}</td>
  <td>保護された独自の Docker イメージ・リポジトリーをセットアップします。そこでイメージを安全に保管し、クラスター・ユーザー間で共有することができます。 詳しくは、<a href="/docs/services/Registry?topic=registry-getting-started" target="_blank">{{site.data.keyword.registrylong}} の資料 <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を参照してください。</td>
</tr>
<tr>
  <td>{{site.data.keyword.keymanagementservicefull}}</td>
  <td>{{site.data.keyword.keymanagementserviceshort}} を有効にして、クラスター内にある Kubernetes シークレットを暗号化します。 Kubernetes シークレットを暗号化すると、許可されていないユーザーがクラスターの機密情報にアクセスできなくなります。<br>セットアップするには、<a href="/docs/containers?topic=containers-encryption#keyprotect">{{site.data.keyword.keymanagementserviceshort}} を使用した Kubernetes シークレットの暗号化</a>を参照してください。<br>詳しくは、<a href="/docs/services/key-protect?topic=key-protect-getting-started-tutorial" target="_blank">{{site.data.keyword.keymanagementserviceshort}} の資料 <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を参照してください。</td>
</tr>
<tr>
<td>NeuVector</td>
<td><a href="https://neuvector.com/" target="_blank">NeuVector <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を使用して、コンテナーをクラウド・ネイティブ・ファイアウォールによって保護します。 詳しくは、<a href="https://www.ibm.com/us-en/marketplace/neuvector-container-security" target="_blank">NeuVector Container Security <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を参照してください。 </td>
</tr>
<tr>
<td>Twistlock</td>
<td><a href="/docs/services/va?topic=va-va_index" target="_blank">Vulnerability Advisor</a> を補完するために、<a href="https://www.twistlock.com/" target="_blank">Twistlock <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を使用して、ファイアウォール、脅威防御、インシデント対応を管理できます。 詳しくは、<a href="https://www.ibm.com/blogs/bluemix/2017/07/twistlock-ibm-bluemix-container-service/" target="_blank">Twistlock on {{site.data.keyword.containerlong_notm}} <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を参照してください。 </td>
</tr>
</tbody>
</table>

<br />



## ストレージ・サービス
{: #storage_services}
<table summary="この表は、クラスターに追加して永続ストレージ機能を追加するために使用できるサービスを示しています。行は左から右に読み、1 列目はサービスの名前、2 列目はサービスの説明です。">
<caption>ストレージ・サービス</caption>
<thead>
<tr>
<th>サービス</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>Heptio Velero</td>
  <td><a href="https://github.com/heptio/velero" target="_blank">Heptio Velero <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を使用して、クラスター・リソースと永続ボリュームのバックアップとリストアを実行できます。 詳しくは、Heptio Velero の <a href="https://github.com/heptio/velero/blob/release-0.9/docs/use-cases.md" target="_blank">Use cases for disaster recovery and cluster migration <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を参照してください。</td>
</tr>
<tr>
  <td>{{site.data.keyword.Bluemix_notm}} Block Storage</td>
  <td>[{{site.data.keyword.Bluemix_notm}} Block Storage](/docs/infrastructure/BlockStorage?topic=BlockStorage-getting-started#getting-started) は、Kubernetes 永続ボリューム (PV) を使用してアプリに追加できる高性能永続 iSCSI ストレージです。 　ブロック・ストレージは、単一ゾーンにステートフル・アプリをデプロイする場合に使用したり、単一ポッド用の高性能ストレージとして使用したりします。 クラスターにブロック・ストレージをプロビジョンする方法について詳しくは、[{{site.data.keyword.Bluemix_notm}} Block Storage にデータを格納する](/docs/containers?topic=containers-block_storage#block_storage) を参照してください。</td>
  </tr>
<tr>
  <td>{{site.data.keyword.cos_full}}</td>
  <td>{{site.data.keyword.cos_short}} で保管するデータは、暗号化され、複数の地理的ロケーションに分散され、REST API を使用して HTTP によってアクセスされます。 [ibm-backup-restore image](/docs/services/RegistryImages/ibm-backup-restore?topic=RegistryImages-ibmbackup_restore_starter) を使用して、クラスターのデータの一回限りのバックアップ、またはスケジュールしたバックアップを実行するようにサービスを構成できます。 サービスについて詳しくは、<a href="/docs/services/cloud-object-storage?topic=cloud-object-storage-about" target="_blank">{{site.data.keyword.cos_short}}の資料 <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を参照してください。</td>
</tr>
  <tr>
  <td>{{site.data.keyword.Bluemix_notm}} File Storage</td>
  <td>[{{site.data.keyword.Bluemix_notm}} File Storage](/docs/infrastructure/FileStorage?topic=FileStorage-getting-started#getting-started) は、NFS ベースの高速で柔軟なネットワーク接続型永続ファイル・ストレージであり、Kubernetes 永続ボリュームを使用してアプリに追加することができます。 ワークロードの要件を満たす GB サイズと IOPS を考慮して、事前定義されたストレージ層の中から選択できます。 クラスターにファイル・ストレージをプロビジョンする方法について詳しくは、[{{site.data.keyword.Bluemix_notm}} File Storage にデータを格納する](/docs/containers?topic=containers-file_storage#file_storage) を参照してください。</td>
  </tr>
  <tr>
    <td>Portworx</td>
    <td>[Portworx ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://portworx.com/products/introduction/) は、可用性の高いソフトウェア定義のストレージ・ソリューションです。これを使用して、コンテナー化データベースなどのステートフル・アプリ用の永続ストレージを管理したり、複数のゾーンにわたってポッド間でデータを共有したりすることができます。 Helm チャートを使用して Portworx をインストールし、Kubernetes 永続ボリュームを使用してアプリ用ストレージをプロビジョンできます。 クラスターに Portworx をセットアップする方法について詳しくは、[Portworx を使ってソフトウェア定義のストレージ (SDS) にデータを格納する](/docs/containers?topic=containers-portworx#portworx) を参照してください。</td>
  </tr>
</tbody>
</table>

<br />


## データベース・サービス
{: #database_services}

<table summary="この表は、クラスターに追加してデータベース機能を追加するために使用できるサービスを示しています。行は左から右に読み、1 列目はサービスの名前、2 列目はサービスの説明です。">
<caption>データベース・サービス</caption>
<thead>
<tr>
<th>サービス</th>
<th>説明</th>
</tr>
</thead>
<tbody>
  <tr>
    <td>{{site.data.keyword.blockchainfull_notm}} Platform 2.0 ベータ版</td>
    <td>独自の {{site.data.keyword.blockchainfull_notm}} Platform を {{site.data.keyword.containerlong_notm}} にデプロイして管理できます。 {{site.data.keyword.blockchainfull_notm}} Platform 2.0 では、{{site.data.keyword.blockchainfull_notm}} ネットワークをホストしたり、他の {{site.data.keyword.blockchainfull_notm}} 2.0 ネットワークに参加できる組織を作成したりすることができます。 {{site.data.keyword.containerlong_notm}} に {{site.data.keyword.blockchainfull_notm}} をセットアップする方法について詳しくは、[{{site.data.keyword.blockchainfull_notm}} Platform 2.0 無料ベータ版について](/docs/services/blockchain?topic=blockchain-ibp-console-overview#ibp-console-overview)を参照してください。</td>
  </tr>
<tr>
  <td>クラウド・データベース</td>
  <td>{{site.data.keyword.composeForMongoDB_full}} や {{site.data.keyword.cloudantfull}} などのさまざまな {{site.data.keyword.Bluemix_notm}} データベース・サービスの中から選択して、可用性の高いスケーラブルなデータベース・ソリューションをクラスターにデプロイできます。 使用可能なクラウド・データベースのリストについては、[{{site.data.keyword.Bluemix_notm}} カタログ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/catalog?category=databases) を参照してください。  </td>
  </tr>
  </tbody>
  </table>
