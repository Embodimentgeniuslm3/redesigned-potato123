---

copyright:
  years: 2014, 2019
lastupdated: "2019-04-09"

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


# サービスの統合
{: #integrations}

{{site.data.keyword.containerlong}} の標準の Kubernetes クラスターでは、さまざまな外部サービスとカタログ・サービスを使用できます。
{:shortdesc}


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
<td> <a href="https://helm.sh" target="_blank">Helm <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> は Kubernetes パッケージ・マネージャーです。 新しい Helm チャートを作成するか、既存の Helm チャートを使用して、{{site.data.keyword.containerlong_notm}} クラスターで実行される複雑な Kubernetes アプリケーションの定義、インストール、アップグレードを行うことができます。 <p>詳しくは、[{{site.data.keyword.containerlong_notm}} での Helm のセットアップ](/docs/containers?topic=containers-integrations#helm)を参照してください。</p></td>
</tr>
<tr>
<td>{{site.data.keyword.contdelivery_full}}</td>
<td>アプリのビルドと Kubernetes クラスターへのコンテナーのデプロイメントを、ツールチェーンを使用して自動化します。 セットアップ情報については、<a href="https://developer.ibm.com/recipes/tutorials/deploy-kubernetes-pods-to-the-bluemix-container-service-using-devops-pipelines/" target="_blank">Deploy Kubernetes pods to the {{site.data.keyword.containerlong_notm}} using DevOps Pipelines <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> というブログを参照してください。 </td>
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
<td>[Knative ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/knative/docs) は IBM、Google、Pivotal、Red Hat、Cisco などによって開発されたオープン・ソースのプラットフォームであり、Kubernetes の機能を拡張して Kubernetes クラスター上にソース中心の最新のコンテナー化サーバーレス・アプリを作成できるようにすることを目的としています。 このプラットフォームは、Kubernetes におけるビルドの操作負担およびワークロードのデプロイと管理の抽象化に関して、さまざまなプログラミング言語とフレームワークに対して一貫性のあるアプローチを取っているので、開発者は最も重要なもの、つまりソース・コードに集中することができます。 詳しくは、[チュートリアル: 管理対象の Knative を使用して、Kubernetes クラスターでサーバーレス・アプリを実行する](/docs/containers?topic=containers-knative_tutorial#knative_tutorial)を参照してください。 </td>
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
<td>Grafana を使用してログを分析し、クラスター内で実行された管理アクティビティーをモニターします。 このサービスについて詳しくは、[Activity Tracker](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started-with-cla) の資料を参照してください。 追跡できるイベントのタイプについて詳しくは、[Activity Tracker イベント](/docs/containers?topic=containers-at_events)を参照してください。</td>
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
<td>Prometheus は、特に Kubernetes のために設計された、モニタリング、ロギング、アラートのためのオープン・ソース・ツールです。 Prometheus は、Kubernetes のロギング情報に基づいて、クラスター、ワーカー・ノード、デプロイメントの正常性に関する詳細情報を取得します。 クラスターで実行されているコンテナーごとに、CPU、メモリー、I/O、ネットワーク・アクティビティーの情報が収集されます。 収集されたデータをカスタム・クエリーやアラートで使用して、クラスター内のパフォーマンスとワークロードをモニターできます。

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
<td>Weave Scope は、Kubernetes クラスター内のリソース (サービス、ポッド、コンテナー、プロセス、ノードなど) のビジュアル図を表示します。 Weave Scope は CPU とメモリーのインタラクティブ・メトリックを示し、コンテナーの中で追尾したり実行したりするツールも備えています。<p>詳しくは、[Weave Scope と {{site.data.keyword.containerlong_notm}} での Kubernetes クラスター・リソースの視覚化](/docs/containers?topic=containers-integrations#weavescope)を参照してください。</p></li></ol>
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
  <tr id="appid">
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
  <td>[{{site.data.keyword.Bluemix_notm}} Block Storage](/docs/infrastructure/BlockStorage?topic=BlockStorage-getting-started#getting-started) は、Kubernetes 永続ボリューム (PV) を使用してアプリに追加できる高性能永続 iSCSI ストレージです。 ブロック・ストレージは、単一ゾーンにステートフル・アプリをデプロイする場合に使用したり、単一ポッド用の高性能ストレージとして使用したりします。 クラスターにブロック・ストレージをプロビジョンする方法について詳しくは、[{{site.data.keyword.Bluemix_notm}} Block Storage にデータを格納する](/docs/containers?topic=containers-block_storage#block_storage) を参照してください。</td>
  </tr>
<tr>
  <td>{{site.data.keyword.cos_full}}</td>
  <td>{{site.data.keyword.cos_short}} で保管するデータは、暗号化され、複数の地理的ロケーションに分散され、REST API を使用して HTTP によってアクセスされます。 [ibm-backup-restore image](/docs/services/RegistryImages/ibm-backup-restore?topic=RegistryImages-ibmbackup_restore_starter) を使用して、クラスターのデータの一回限りのバックアップ、またはスケジュールしたバックアップを実行するようにサービスを構成できます。 このサービスに関する一般情報については、<a href="/docs/services/cloud-object-storage?topic=cloud-object-storage-about-ibm-cloud-object-storage" target="_blank">{{site.data.keyword.cos_short}} の資料 <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を参照してください。</td>
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


## {{site.data.keyword.Bluemix_notm}} サービスをクラスターに追加する
{: #adding_cluster}

{{site.data.keyword.Bluemix_notm}} サービスを追加して、Watson AI、データ、セキュリティー、モノのインターネット (IoT) などの領域の追加機能によって Kubernetes クラスターを拡張します。
{:shortdesc}

サービス・キーをサポートするサービスのみをバインドできます。 サービス・キーをサポートするサービスのリストを確認するには、[{{site.data.keyword.Bluemix_notm}} サービスを使用するための外部アプリの使用可能化](/docs/resources?topic=resources-externalapp#externalapp)を参照してください。
{: note}

開始前に、以下のことを行います。
- 以下の役割があることを確認してください。
    - クラスターに対する[**エディター**または**管理者**の {{site.data.keyword.Bluemix_notm}} IAM サービス役割](/docs/containers?topic=containers-users#platform)。
    - サービスをバインドする名前空間に対する[**ライター**または**管理者**の {{site.data.keyword.Bluemix_notm}} IAM サービス役割](/docs/containers?topic=containers-users#platform)。
    - 使用するスペースに対する[**開発者**の Cloud Foundry 役割](/docs/iam?topic=iam-mngcf#mngcf)。
- [アカウントにログインします。 該当する地域とリソース・グループ (該当する場合) をターゲットとして設定します。 クラスターのコンテキストを設定します](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)。

クラスターに {{site.data.keyword.Bluemix_notm}} サービスを追加するには、以下の手順を実行します。

1. [{{site.data.keyword.Bluemix_notm}} サービスのインスタンスを作成します](/docs/resources?topic=resources-externalapp#externalapp)。
    * 一部の {{site.data.keyword.Bluemix_notm}} サービスは、選択された地域でのみ使用可能です。 サービスがクラスターと同じ地域で使用可能な場合にのみ、サービスをクラスターにバインドできます。 さらに、ワシントン DC ゾーンでサービス・インスタンスを作成する場合は、CLI を使用する必要があります。
    * クラスターと同じリソース・グループにサービス・インスタンスを作成する必要があります。 リソースは 1 つのリソース・グループにしか作成できず、その後にリソース・グループを変更することはできません。

2. 作成したサービスのタイプを確認し、サービス・インスタンスの**名前**をメモします。
   - **Cloud Foundry サービス:**
     ```
     ibmcloud service list
     ```
     {: pre}

     出力例:
     ```
     name                         service           plan    bound apps   last operation
     <cf_service_instance_name>   <service_name>    spark                create succeeded
     ```
     {: screen}

  - **{{site.data.keyword.Bluemix_notm}}IAM 対応サービス:**
     ```
     ibmcloud resource service-instances
     ```
     {: pre}

     出力例:
     ```
     Name                          Location   State    Type               Tags
     <iam_service_instance_name>   <region>   active   service_instance
     ```
     {: screen}

   ダッシュボードに異なるサービス・タイプが**「Cloud Foundry サービス」**および**「サービス」**として表示される場合もあります。

3. サービスを追加するために使用するクラスターの名前空間を識別します。 次のいずれかのオプションを選択します。
     ```
     kubectl get namespaces
     ```
     {: pre}

4.  `ibmcloud ks cluster-service-bind` [コマンド](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_service_bind)を使用して、クラスターにサービスを追加します。 {{site.data.keyword.Bluemix_notm}} IAM 対応サービスの場合は、前に作成した Cloud Foundry 別名を使用します。 IAM 対応サービスの場合は、デフォルトの**ライター**のサービス・アクセス役割を使用することも、`--role` フラグを使用してサービス・アクセス役割を指定することもできます。 このコマンドによって、サービス・インスタンスのサービス・キーが作成されます。または、`--key` フラグを含めて、既存のサービス・キー資格情報を使用することができます。 `--key` フラグを使用する場合は、`--role` フラグを組み込まないでください。
    ```
    ibmcloud ks cluster-service-bind --cluster <cluster_name_or_ID> --namespace <namespace> --service <service_instance_name> [--key <service_instance_key>] [--role <IAM_service_role>]
    ```
    {: pre}

    サービスがクラスターに正常に追加されると、サービス・インスタンスの資格情報を保持するクラスター・シークレットが作成されます。 シークレットは、データを保護するために etcd では自動的に暗号化されます。

    出力例:
    ```
    ibmcloud ks cluster-service-bind --cluster mycluster --namespace mynamespace --service cleardb
    Binding service instance to namespace...
    OK
    Namespace:	     mynamespace
    Secret name:     binding-<service_instance_name>
    ```
    {: screen}

5.  Kubernetes シークレットでサービス資格情報を確認します。
    1. シークレットの詳細を取得し、**binding** 値をメモします。 **binding** 値は base64 エンコードであり、サービス・インスタンスの資格情報を JSON 形式で保持します。
       ```
       kubectl get secrets binding-<service_instance_name> --namespace=<namespace> -o yaml
       ```
       {: pre}

       出力例:
       ```
       apiVersion: v1
       data:
         binding: <binding>
       kind: Secret
       metadata:
         annotations:
           service-instance-id: 1111aaaa-a1aa-1aa1-1a11-111aa111aa11
           service-key-id: 2b22bb2b-222b-2bb2-2b22-b22222bb2222
         creationTimestamp: 2018-08-07T20:47:14Z
         name: binding-<service_instance_name>
         namespace: <namespace>
         resourceVersion: "6145900"
         selfLink: /api/v1/namespaces/default/secrets/binding-mycloudant
         uid: 33333c33-3c33-33c3-cc33-cc33333333c
       type: Opaque
       ```
       {: screen}

    2. binding 値をデコードします
       ```
       echo "<binding>" | base64 -D
       ```
       {: pre}

       出力例:
       ```
       {"apikey":"KL34Ys893284NGJEPFjgrioJ12NElpow","host":"98765aab-9ce1-7tr3-ba87-bfbab6e6d9d6-bluemix.cloudant.com","iam_apikey_description":"Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:cloudantnosqldb:us-south:a/1234g56789cfe8e6388dd2ec098:98746cw-43d7-49ce-947a-d8fe3eebb381::","iam_apikey_name":"auto-generated-apikey-1234abcde-987f-3t64-9d96-d13775ec5663","iam_role_crn":"crn:v1:bluemix:public:iam::::serviceRole:Writer","iam_serviceid_crn":"crn:v1:bluemix:public:iam-identity::a/1234567890brasge5htn2ec098::serviceid:ServiceId-12345vgh-6c4c-ytr12-af6b-467d30d6ef44","password":"jfiavhui12484fnivhuo472nvei23913c3ff","port":443,"url":"https://25c73aac-9ce1-4c24-ba98-bfbab6e6d9d6-bluemix:ugvioev823inreuiegn43donvri29989wiu9t22@25c73aac-9ce1-4c24-ba98-abdrjio123562lnsb-bluemix.cloudant.com","username":"123b45da-9ce1-4c24-ab12-rinwnwub1294-bluemix"}
       ```
       {: screen}

    3. オプション: 前のステップでデコードしたサービス資格情報を、{{site.data.keyword.Bluemix_notm}} ダッシュボードのサービス・インスタンスで検索したサービス資格情報と比較します。

6. これでサービスがクラスターにバインドされたため、[Kubernetes シークレットのサービス資格情報にアクセスする](#adding_app)ために、アプリを構成する必要があります。


## アプリからのサービス資格情報へのアクセス
{: #adding_app}

アプリから {{site.data.keyword.Bluemix_notm}} サービス・インスタンスにアクセスするには、Kubernetes シークレットに保管されているサービス資格情報をアプリで使用可能にする必要があります。
{: shortdesc}

サービス・インスタンスの資格情報は base64 エンコードであり、JSON 形式でシークレットの内部に保管されます。 シークレットにあるデータにアクセスするには、次のオプションの中から選択します。
- [シークレットをボリュームとしてポッドにマウントする](#mount_secret)
- [環境変数でシークレットを参照する](#reference_secret)
<br>
シークレットをさらに保護したいですか? クラスター管理者に連絡してクラスター内の [{{site.data.keyword.keymanagementservicefull}} を有効にして](/docs/containers?topic=containers-encryption#keyprotect)もらい、{{site.data.keyword.Bluemix_notm}} サービス・インスタンスの資格情報を保管するシークレットなどの、新しいシークレットと既存のシークレットを暗号化します。
{: tip}

始める前に
-  `kube-system` 名前空間に対する[**ライター**または**管理者**の {{site.data.keyword.Bluemix_notm}} IAM サービス役割](/docs/containers?topic=containers-users#platform)があることを確認してください。
- [アカウントにログインします。 該当する地域とリソース・グループ (該当する場合) をターゲットとして設定します。 クラスターのコンテキストを設定します](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)。
- [{{site.data.keyword.Bluemix_notm}} サービスをクラスターに追加します](#adding_cluster)。

### シークレットをボリュームとしてポッドにマウントする
{: #mount_secret}

シークレットをボリュームとしてポッドにマウントすると、`binding` という名前のファイルがボリューム・マウント・ディレクトリーに保管されます。 JSON 形式の `binding` ファイルには、{{site.data.keyword.Bluemix_notm}} サービスにアクセスするために必要なすべての情報と資格情報が格納されます。
{: shortdesc}

1.  クラスター内の使用可能なシークレットをリストし、シークレットの **name** をメモします。 **Opaque** タイプのシークレットを探します。 複数のシークレットが存在する場合は、クラスター管理者に連絡して、サービスに対応する正しいシークレットを確認してください。

    ```
    kubectl get secrets
    ```
    {: pre}

    出力例:

    ```
    NAME                              TYPE            DATA      AGE
    binding-<service_instance_name>   Opaque          1         3m

    ```
    {: screen}

2.  Kubernetes デプロイメントの YAML ファイルを作成し、シークレットをボリュームとしてポッドにマウントします。
    ```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      labels:
        app: secret-test
      name: secret-test
      namespace: <my_namespace>
    spec:
      selector:
        matchLabels:
          app: secret-test
      replicas: 1
      template:
        metadata:
          labels:
            app: secret-test
        spec:
          containers:
          - image: registry.bluemix.net/ibmliberty:latest
            name: secret-test
            volumeMounts:
            - mountPath: <mount_path>
              name: <volume_name>
          volumes:
          - name: <volume_name>
            secret:
              defaultMode: 420
              secretName: binding-<service_instance_name>
    ```
    {: codeblock}

    <table>
    <caption>YAML ファイルの構成要素について</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> YAML ファイルの構成要素について</th>
    </thead>
    <tbody>
    <tr>
    <td><code>volumeMounts.mountPath</code></td>
    <td>コンテナー内でボリュームがマウントされるディレクトリーの絶対パス。</td>
    </tr>
    <tr>
    <td><code>volumeMounts.name</code></br><code>volumes.name</code></td>
    <td>ポッドにマウントするボリュームの名前。</td>
    </tr>
    <tr>
    <td><code>secret.defaultMode</code></td>
    <td>シークレットに対する読み取りおよび書き込みアクセス権。 読み取り専用アクセス権を設定するには、`420` を使用します。 </td>
    </tr>
    <tr>
    <td><code>secret.secretName</code></td>
    <td>前のステップでメモしたシークレットの名前。</td>
    </tr></tbody></table>

3.  ポッドを作成して、シークレットをボリュームとしてマウントします。
    ```
    kubectl apply -f secret-test.yaml
    ```
    {: pre}

4.  ポッドが作成されたことを確認します。
    ```
    kubectl get pods
    ```
    {: pre}

    CLI 出力例:

    ```
    NAME                           READY     STATUS    RESTARTS   AGE
    secret-test-1111454598-gfx32   1/1       Running   0          1m
    ```
    {: screen}

5.  サービス資格情報にアクセスします。
    1. ポッドにログインします。
       ```
       kubectl exec <pod_name> -it bash
       ```
       {: pre}

    2. 先ほど定義したボリューム・マウント・パスにナビゲートし、ボリューム・マウント・パスのファイルをリストします。
       ```
       cd <volume_mountpath> && ls
       ```
       {: pre}

       出力例:
       ```
       バインド
       ```
       {: screen}

       `binding` ファイルには、Kubernetes シークレットに保管したサービス資格情報が含まれています。

    4. サービス資格情報を表示します。 資格情報は、JSON 形式でキー値ペアとして保管されています。
       ```
       cat binding
       ```
       {: pre}

       出力例:
       ```
       {"apikey":"KL34Ys893284NGJEPFjgrioJ12NElpow","host":"98765aab-9ce1-7tr3-ba87-bfbab6e6d9d6-bluemix.cloudant.com","iam_apikey_description":"Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:cloudantnosqldb:us-south:a/1234g56789cfe8e6388dd2ec098:98746cw-43d7-49ce-947a-d8fe3eebb381::","iam_apikey_name":"auto-generated-apikey-1234abcde-987f-3t64-9d96-d13775ec5663","iam_role_crn":"crn:v1:bluemix:public:iam::::serviceRole:Writer","iam_serviceid_crn":"crn:v1:bluemix:public:iam-identity::a/1234567890brasge5htn2ec098::serviceid:ServiceId-12345vgh-6c4c-ytr12-af6b-467d30d6ef44","password":"jfiavhui12484fnivhuo472nvei23913c3ff","port":443,"url":"https://25c73aac-9ce1-4c24-ba98-bfbab6e6d9d6-bluemix:ugvioev823inreuiegn43donvri29989wiu9t22@25c73aac-9ce1-4c24-ba98-abdrjio123562lnsb-bluemix.cloudant.com","username":"123b45da-9ce1-4c24-ab12-rinwnwub1294-bluemix"}
       ```
       {: screen}

    5. JSON コンテンツを解析してサービスにアクセスするために必要な情報を取得するようにアプリを構成します。


### 環境変数でシークレットを参照する
{: #reference_secret}

Kubernetes シークレットのサービス資格情報およびその他のキー値ペアを、環境変数としてデプロイメントに追加できます。
{: shortdesc}

1. クラスター内の使用可能なシークレットをリストし、シークレットの **name** をメモします。 **Opaque** タイプのシークレットを探します。 複数のシークレットが存在する場合は、クラスター管理者に連絡して、サービスに対応する正しいシークレットを確認してください。

    ```
    kubectl get secrets
    ```
    {: pre}

    出力例:

    ```
    NAME                                    TYPE                                  DATA      AGE
    binding-<service_instance_name>         Opaque                                1         3m
    ```
    {: screen}

2. シークレットの詳細を入手して、ポッドで環境変数として参照できる潜在的なキー値ペアを検索します。 サービス資格情報は、シークレットの `binding` キーに保管されています。
   ```
   kubectl get secrets binding-<service_instance_name> --namespace=<namespace> -o yaml
   ```
   {: pre}

   出力例:
   ```
   apiVersion: v1
   data:
     binding: <binding>
   kind: Secret
   metadata:
     annotations:
       service-instance-id: 7123acde-c3ef-4ba2-8c52-439ac007fa70
       service-key-id: 9h30dh8a-023f-4cf4-9d96-d12345ec7890
     creationTimestamp: 2018-08-07T20:47:14Z
     name: binding-<service_instance_name>
     namespace: <namespace>
     resourceVersion: "6145900"
     selfLink: /api/v1/namespaces/default/secrets/binding-mycloudant
     uid: 12345a31-9a83-11e8-ba83-cd49014748f
   type: Opaque
   ```
   {: screen}

3. Kubernetes デプロイメントの YAML ファイルを作成し、`binding` キーを参照する環境変数を指定します。
   ```
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     labels:
       app: secret-test
     name: secret-test
     namespace: <my_namespace>
   spec:
     selector:
       matchLabels:
         app: secret-test
     template:
       metadata:
         labels:
           app: secret-test
       spec:
         containers:
         - image: registry.bluemix.net/ibmliberty:latest
           name: secret-test
           env:
           - name: BINDING
             valueFrom:
               secretKeyRef:
                 name: binding-<service_instance_name>
                 key: binding
     ```
     {: codeblock}

     <table>
     <caption>YAML ファイルの構成要素について</caption>
     <thead>
     <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> YAML ファイルの構成要素について</th>
     </thead>
     <tbody>
     <tr>
     <td><code>containers.env.name</code></td>
     <td>環境変数の名前。</td>
     </tr>
     <tr>
     <td><code>env.valueFrom.secretKeyRef.name</code></td>
     <td>前のステップでメモしたシークレットの名前。</td>
     </tr>
     <tr>
     <td><code>env.valueFrom.secretKeyRef.key</code></td>
     <td>シークレットの一部であり、環境変数で参照するキー。 サービス資格情報を参照するには、<strong>binding</strong> キーを使用する必要があります。  </td>
     </tr>
     </tbody></table>

4. シークレットの `binding` キーを環境変数として参照するポッドを作成します。
   ```
   kubectl apply -f secret-test.yaml
   ```
   {: pre}

5. ポッドが作成されたことを確認します。
   ```
   kubectl get pods
   ```
   {: pre}

   CLI 出力例:
   ```
   NAME                           READY     STATUS    RESTARTS   AGE
    secret-test-1111454598-gfx32   1/1       Running   0          1m
   ```
   {: screen}

6. 環境変数が正しく設定されていることを確認します。
   1. ポッドにログインします。
      ```
      kubectl exec <pod_name> -it bash
      ```
      {: pre}

   2. すべての環境変数をポッド内にリストします。
      ```
      env
      ```
      {: pre}

      出力例:
      ```
      BINDING={"apikey":"KL34Ys893284NGJEPFjgrioJ12NElpow","host":"98765aab-9ce1-7tr3-ba87-bfbab6e6d9d6-bluemix.cloudant.com","iam_apikey_description":"Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:cloudantnosqldb:us-south:a/1234g56789cfe8e6388dd2ec098:98746cw-43d7-49ce-947a-d8fe3eebb381::","iam_apikey_name":"auto-generated-apikey-1234abcde-987f-3t64-9d96-d13775ec5663","iam_role_crn":"crn:v1:bluemix:public:iam::::serviceRole:Writer","iam_serviceid_crn":"crn:v1:bluemix:public:iam-identity::a/1234567890brasge5htn2ec098::serviceid:ServiceId-12345vgh-6c4c-ytr12-af6b-467d30d6ef44","password":"jfiavhui12484fnivhuo472nvei23913c3ff","port":443,"url":"https://25c73aac-9ce1-4c24-ba98-bfbab6e6d9d6-bluemix:ugvioev823inreuiegn43donvri29989wiu9t22@25c73aac-9ce1-4c24-ba98-abdrjio123562lnsb-bluemix.cloudant.com","username":"123b45da-9ce1-4c24-ab12-rinwnwub1294-bluemix"}
      ```
      {: screen}

7. 環境変数を読み取り、JSON コンテンツを解析してサービスにアクセスするために必要な情報を取得するようにアプリを構成します。

   Python のコード例:
   ```
   if os.environ.get('BINDING'):
        credentials = json.loads(os.environ.get('BINDING'))
   ```
   {: codeblock}

## {{site.data.keyword.containerlong_notm}} での Helm のセットアップ
{: #helm}

[Helm ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://helm.sh) は Kubernetes パッケージ・マネージャーです。 Helm チャートを作成するか、既存の Helm チャートを使用して、{{site.data.keyword.containerlong_notm}} クラスターで実行される複雑な Kubernetes アプリケーションの定義、インストール、アップグレードを行うことができます。
{:shortdesc}

Helm チャートをデプロイするためには、ローカル・マシンに Helm CLI をインストールし、クラスターに Helm サーバー Tiller をインストールする必要があります。 Tiller のイメージはパブリック Google Container Registry に保管されます。 Tiller のインストール時にイメージにアクセスするためには、パブリック Google Container Registry へのパブリック・ネットワーク接続がクラスターで許可されていなければなりません。 パブリック・サービス・エンドポイントが有効になっているクラスターは、自動的にイメージにアクセスできます。 カスタム・ファイアウォールで保護されているプライベート・クラスターや、プライベート・サービス・エンドポイントのみを有効にしているクラスターは、Tiller イメージへのアクセスを許可しません。 代わりに、[イメージをローカル・マシンにプルしてそれを {{site.data.keyword.registryshort_notm}} 内の名前空間にプッシュする](#private_local_tiller)か、[Tiller を使用せずに Helm チャートをインストールする](#private_install_without_tiller)ことができます。
{: note}

### パブリック・アクセスが可能なクラスターでの Helm のセットアップ
{: #public_helm_install}

クラスターでパブリック・サービス・エンドポイントが有効になっている場合は、Google Container Registry 内のパブリック・イメージを使用して Tiller をインストールできます。
{: shortdesc}

開始前に、以下のことを行います。 [アカウントにログインします。 該当する地域とリソース・グループ (該当する場合) をターゲットとして設定します。 クラスターのコンテキストを設定します](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)。

1. <a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">Helm CLI <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> をインストールします。

2. **重要**: クラスターのセキュリティーを維持するため、[{{site.data.keyword.Bluemix_notm}} `kube-samples` リポジトリー](https://github.com/IBM-Cloud/kube-samples/blob/master/rbac/serviceaccount-tiller.yaml)から以下の `.yaml` ファイルを適用することによって、Tiller のサービス・アカウントを `kube-system` 名前空間に作成し、`tiller-deploy` ポッドに対する Kubernetes RBAC クラスター役割バインディングを作成します。 **注**: `kube-system` 名前空間のサービス・アカウントとクラスター役割バインディングを使用して Tiller をインストールするには、[`cluster-admin` 役割](/docs/containers?topic=containers-users#access_policies)が必要です。
    ```
    kubectl apply -f https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/rbac/serviceaccount-tiller.yaml
    ```
    {: pre}

3. 作成したサービス・アカウントを使用して、Helm を初期化し、Tiller をインストールします。

    ```
    helm init --service-account tiller
    ```
    {: pre}

4.  インストールが成功したことを確認します。
    1.  Tiller サービス・アカウントが作成されたことを確認します。
        ```
        kubectl get serviceaccount -n kube-system tiller
        ```
        {: pre}

        出力例:

        ```
        NAME                                 SECRETS   AGE
    tiller                               1         2m
        ```
        {: screen}

    2.  クラスター内の `tiller-deploy` ポッドの**「状況」**が`「実行中」`になっていることを確認します。
        ```
        kubectl get pods -n kube-system -l app=helm
        ```
        {: pre}

        出力例:

        ```
        NAME                            READY     STATUS    RESTARTS   AGE
    tiller-deploy-352283156-nzbcm   1/1       Running   0          2m
        ```
        {: screen}

5. {{site.data.keyword.Bluemix_notm}} Helm リポジトリーを Helm インスタンスに追加します。
   ```
   helm repo add ibm  https://registry.bluemix.net/helm/ibm
   ```
   {: pre}

   ```
   helm repo add ibm-charts https://registry.bluemix.net/helm/ibm-charts
   ```
   {: pre}

6. すべての Helm チャートの最新バージョンを取得するようにリポジトリーを更新します。
   ```
   helm repo update
   ```
   {: pre}

7. {{site.data.keyword.Bluemix_notm}} リポジトリーで現在使用可能な Helm チャートをリストします。
   ```
   helm search ibm
   ```
   {: pre}

   ```
   helm search ibm-charts
   ```
   {: pre}

8. インストールする Helm チャートを特定し、Helm チャートの `README` に記載されている手順に従って Helm チャートをクラスターにインストールします。


### プライベート・クラスター: {{site.data.keyword.registryshort_notm}} 内のプライベート・レジストリーに Tiller イメージをプッシュする
{: #private_local_tiller}

Tiller イメージをローカル・マシンにプルしてそれを {{site.data.keyword.registryshort_notm}} 内の名前空間にプッシュし、{{site.data.keyword.registryshort_notm}} 内のイメージを使用して Helm で Tiller をインストールすることができます。
{: shortdesc}

開始前に、以下のことを行います。
- ローカル・マシンに Docker をインストールします。 [{{site.data.keyword.Bluemix_notm}} CLI](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli) がインストールされていれば、Docker は既にインストールされています。
- [{{site.data.keyword.registryshort_notm}} CLI プラグインをインストールして、名前空間をセットアップします](/docs/services/Registry?topic=registry-getting-started#gs_registry_cli_install)。

{{site.data.keyword.registryshort_notm}} を使用して Tiller をインストールするには、以下のようにします。

1. ローカル・マシンに <a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">Helm CLI <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> をインストールします。
2. セットアップした {{site.data.keyword.Bluemix_notm}} インフラストラクチャー VPN トンネルを使用して、プライベート・クラスターに接続します。
3. **重要**: クラスターのセキュリティーを維持するため、[{{site.data.keyword.Bluemix_notm}} `kube-samples` リポジトリー](https://github.com/IBM-Cloud/kube-samples/blob/master/rbac/serviceaccount-tiller.yaml)から以下の `.yaml` ファイルを適用することによって、Tiller のサービス・アカウントを `kube-system` 名前空間に作成し、`tiller-deploy` ポッドに対する Kubernetes RBAC クラスター役割バインディングを作成します。 **注**: `kube-system` 名前空間のサービス・アカウントとクラスター役割バインディングを使用して Tiller をインストールするには、[`cluster-admin` 役割](/docs/containers?topic=containers-users#access_policies)が必要です。
    1. [Kubernetes サービス・アカウントとクラスター役割バインディングの YAML ファイル ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン") を取得します](https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/rbac/serviceaccount-tiller.yaml)。

    2. クラスター内に Kubernetes リソースを作成します。
       ```
       kubectl apply -f service-account.yaml
       ```
       {: pre}

       ```
       kubectl apply -f cluster-role-binding.yaml
       ```
       {: pre}

4. クラスターにインストールする[バージョンの Tiller を見つけます ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://console.cloud.google.com/gcr/images/kubernetes-helm/GLOBAL/tiller?gcrImageListsize=30)。 特定のバージョンを必要としない場合は、最新のものを使用してください。

5. ローカル・マシンに Tiller イメージをプルします。
   ```
   docker pull gcr.io/kubernetes-helm/tiller:v2.11.0
   ```
   {: pre}

   出力例:
   ```
   docker pull gcr.io/kubernetes-helm/tiller:v2.13.0
   v2.13.0: Pulling from kubernetes-helm/tiller
   48ecbb6b270e: Pull complete
   d3fa0712c71b: Pull complete
   bf13a43b92e9: Pull complete
   b3f98be98675: Pull complete
   Digest: sha256:c4bf03bb67b3ae07e38e834f29dc7fd43f472f67cad3c078279ff1bbbb463aa6
   Status: Downloaded newer image for gcr.io/kubernetes-helm/tiller:v2.13.0
   ```
   {: screen}

6. [Tiller イメージを {{site.data.keyword.registryshort_notm}} 内の名前空間にプッシュします](/docs/services/Registry?topic=registry-getting-started#gs_registry_images_pushing)。

7. [{{site.data.keyword.registryshort_notm}}にアクセスするためのイメージ・プル・シークレットを、デフォルトの名前空間から `kube-system` 名前空間にコピーします](/docs/containers?topic=containers-images#copy_imagePullSecret)。

8. {{site.data.keyword.registryshort_notm}} 内の名前空間に保管したイメージを使用して、プライベート・クラスターに Tiller をインストールします。
   ```
   helm init --tiller-image <region>.icr.io/<mynamespace>/<myimage>:<tag> --service-account tiller
   ```
   {: pre}

9. {{site.data.keyword.Bluemix_notm}} Helm リポジトリーを Helm インスタンスに追加します。
   ```
   helm repo add ibm  https://registry.bluemix.net/helm/ibm
   ```
   {: pre}

   ```
   helm repo add ibm-charts https://registry.bluemix.net/helm/ibm-charts
   ```
   {: pre}

10. すべての Helm チャートの最新バージョンを取得するようにリポジトリーを更新します。
    ```
    helm repo update
    ```
    {: pre}

11. {{site.data.keyword.Bluemix_notm}} リポジトリーで現在使用可能な Helm チャートをリストします。
    ```
    helm search ibm
    ```
    {: pre}

    ```
    helm search ibm-charts
    ```
    {: pre}

12. インストールする Helm チャートを特定し、Helm チャートの `README` に記載されている手順に従って Helm チャートをクラスターにインストールします。

### プライベート・クラスター: Tiller を使用せずに Helm チャートをインストールする
{: #private_install_without_tiller}

プライベート・クラスターに Tiller をインストールしない場合は、Helm チャートの YAML ファイルを手動で作成し、`kubectl` コマンドを使用してそれらの YAML ファイルを適用できます。
{: shortdesc}

この例のステップは、{{site.data.keyword.Bluemix_notm}} Helm チャート・リポジトリーからプライベート・クラスターに Helm チャートをインストールする方法を示しています。 {{site.data.keyword.Bluemix_notm}} Helm チャート・リポジトリーのいずれにも保管されていない Helm チャートをインストールする場合は、このトピックの手順に従って Helm チャート用の YAML ファイルを作成する必要があります。 さらに、パブリック・コンテナー・リポジトリーから Helm チャート・イメージをダウンロードし、それを {{site.data.keyword.registryshort_notm}} 内の名前空間にプッシュして、{{site.data.keyword.registryshort_notm}} 内のイメージを使用するように `values.yaml` ファイルを更新する必要があります。
{: note}

1. ローカル・マシンに <a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">Helm CLI <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> をインストールします。
2. セットアップした {{site.data.keyword.Bluemix_notm}} インフラストラクチャー VPN トンネルを使用して、プライベート・クラスターに接続します。
3. {{site.data.keyword.Bluemix_notm}} Helm リポジトリーを Helm インスタンスに追加します。
   ```
   helm repo add ibm  https://registry.bluemix.net/helm/ibm
   ```
   {: pre}

   ```
   helm repo add ibm-charts https://registry.bluemix.net/helm/ibm-charts
   ```
   {: pre}

4. すべての Helm チャートの最新バージョンを取得するようにリポジトリーを更新します。
   ```
   helm repo update
   ```
   {: pre}

5. {{site.data.keyword.Bluemix_notm}} リポジトリーで現在使用可能な Helm チャートをリストします。
   ```
   helm search ibm
   ```
   {: pre}

   ```
   helm search ibm-charts
   ```
   {: pre}

6. インストールする Helm チャートを特定し、その Helm チャートをローカル・マシンにダウンロードして、Helm チャートのファイルをアンパックします。 次の例は、cluster autoscaler バージョン 1.0.3 用の Helm チャートをダウンロードして、`cluster-autoscaler` ディレクトリーにファイルをアンパックする方法を示しています。
   ```
   helm fetch ibm/ibm-iks-cluster-autoscaler --untar --untardir ./cluster-autoscaler --version 1.0.3
   ```
   {: pre}

7. Helm チャート・ファイルをアンパックしたディレクトリーにナビゲートします。
   ```
   cd cluster-autoscaler
   ```
   {: pre}

8. Helm チャート内のファイルを使用して生成する YAML ファイル用の `output` ディレクトリーを作成します。
   ```
   mkdir output
   ```
   {: pre}

9. `values.yaml` ファイルを開き、Helm チャートのインストール手順で必要とされる変更を行います。
   ```
   nano ibm-iks-cluster-autoscaler/values.yaml
   ```
   {: pre}

10. ローカルにインストールされている Helm を使用して、Helm チャート用のすべての Kubernetes YAML ファイルを作成します。 既に作成した `output` ディレクトリーに YAML ファイルが保管されます。
    ```
    helm template --values ./ibm-iks-cluster-autoscaler/values.yaml --output-dir ./output ./ibm-iks-cluster-autoscaler
    ```
    {: pre}

    出力例:
    ```
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-configmap.yaml
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-service-account-roles.yaml
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-service.yaml
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-deployment.yaml
    ```
    {: screen}

11. すべての YAML ファイルをプライベート・クラスターにデプロイします。
    ```
    kubectl apply --recursive --filename ./output
    ```
   {: pre}

12. オプション: `output` ディレクトリーからすべての YAML ファイルを削除します。
    ```
    kubectl delete --recursive --filename ./output
    ```
    {: pre}


### Helm の関連リンク
{: #helm_links}

* strongSwan Helm チャートを使用するには、[strongSwan IPSec VPN サービスの Helm Chart を使用した VPN 接続のセットアップ](/docs/containers?topic=containers-vpn#vpn-setup)を参照してください。
* コンソールの [Helm チャートのカタログ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/kubernetes/solutions/helm-charts) で、{{site.data.keyword.Bluemix_notm}} で使用可能な Helm チャートを確認します。
* Helm チャートのセットアップと管理に使用する Helm コマンドについて詳しくは、<a href="https://docs.helm.sh/helm/" target="_blank">Helm の資料 <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を参照してください。
* Kubernetes Helm チャートを使用して開発速度を上げる方法について詳しくは、[こちら ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://developer.ibm.com/recipes/tutorials/increase-deployment-velocity-with-kubernetes-helm-charts/) を参照してください。

## Kubernetes クラスター・リソースの視覚化
{: #weavescope}

Weave Scope は、Kubernetes クラスター内のリソース (サービス、ポッド、コンテナーなど) のビジュアル図を表示します。 Weave Scope には、CPU とメモリーのインタラクティブ・メトリックと、コンテナーの中で追跡したり実行したりできるツールが備わっています。
{:shortdesc}

開始前に、以下のことを行います。

-   クラスター情報を公共のインターネットに公開しないようにしてください。 安全に Weave Scope をデプロイして Web ブラウザーからローカル・アクセスするには、以下の手順を実行します。
-   標準クラスターがまだない場合は、[標準クラスターを作成します](/docs/containers?topic=containers-clusters#clusters_ui)。 Weave Scope は、アプリでは特に、CPU の負荷が大きくなります。 フリー・クラスターではなく、比較的大規模な標準クラスターで Weave Scope を実行してください。
-  すべての名前空間に対する[**管理者**の {{site.data.keyword.Bluemix_notm}} IAM サービス役割](/docs/containers?topic=containers-users#platform)があることを確認してください。
-   [アカウントにログインします。 該当する地域とリソース・グループ (該当する場合) をターゲットとして設定します。 クラスターのコンテキストを設定します](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)。


Weave Scope をクラスターで使用するには、以下のようにします。
2.  提供された RBAC 許可構成ファイルをクラスターにデプロイします。

    読み取り/書き込みアクセス権を有効にするには、以下のようにします。

    ```
    kubectl apply -f "https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/weave-scope/weave-scope-rbac.yaml"
    ```
    {: pre}

    読み取り専用アクセス権を有効にするには、以下のようにします。

    ```
    kubectl apply --namespace weave -f "https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/weave-scope/weave-scope-rbac-readonly.yaml"
    ```
    {: pre}

    出力:

    ```
    clusterrole "weave-scope-mgr" created
    clusterrolebinding "weave-scope-mgr-role-binding" created
    ```
    {: screen}

3.  Weave Scope サービスをデプロイします。このサービスは、クラスター IP アドレスでのプライベート・アクセスが可能です。

    <pre class="pre">
    <code>kubectl apply --namespace weave -f "https://cloud.weave.works/k8s/scope.yaml?k8s-version=$(kubectl version | base64 | tr -d '&bsol;n')"</code>
    </pre>

    出力:

    ```
    serviceaccount "weave-scope" created
    deployment "weave-scope-app" created
    service "weave-scope-app" created
    daemonset "weave-scope-agent" created
    ```
    {: screen}

4.  ポート転送コマンドを実行して、コンピューター上でサービスを開きます。 次回 Weave Scope にアクセスする時は、上記の構成ステップを再度実行する必要はなく、次のポート転送コマンドを実行することができます。

    ```
    kubectl port-forward -n weave "$(kubectl get -n weave pod --selector=weave-scope-component=app -o jsonpath='{.items..metadata.name}')" 4040
    ```
    {: pre}

    出力:

    ```
    Forwarding from 127.0.0.1:4040 -> 4040
    Forwarding from [::1]: :1]:4040 -> 4040
    Handling connection for 4040
    ```
    {: screen}

5.  ブラウザーで `http://localhost:4040` を開きます。 デフォルトのコンポーネントをデプロイしないと、次の図が表示されます。 クラスター内の Kubernetes リソースのトポロジー・ダイアグラムまたはテーブルを表示するように選択できます。

     <img src="images/weave_scope.png" alt="Weave Scope のトポロジーの例" style="width:357px;" />


[Weave Scope の機能についての詳細 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.weave.works/docs/scope/latest/features/)。

<br />

