---

copyright:
  years: 2014, 2019
lastupdated: "2019-05-31"

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


# {{site.data.keyword.containerlong_notm}} を使用する理由
{: #cs_ov}

{{site.data.keyword.containerlong}} では、Docker コンテナー、Kubernetes テクノロジー、直観的なユーザー・エクスペリエンス、標準装備のセキュリティーと分離機能を結合させることにより、コンピュート・ホストのクラスター内でコンテナー化アプリのデプロイメント、操作、スケーリング、モニタリングを自動化する強力なツールが提供されます。 認定情報については、[Compliance on the {{site.data.keyword.Bluemix_notm}} ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/cloud/compliance) を参照してください。
{:shortdesc}


## サービスを使用する利点
{: #benefits}

クラスターは、ネイティブの Kubernetes と {{site.data.keyword.IBM_notm}} に固有の機能を提供するコンピュート・ホストにデプロイされます。
{:shortdesc}

|利点|説明|
|-------|-----------|
|コンピュート、ネットワーク、およびストレージそれぞれのインフラストラクチャーを分離する、単一テナントの Kubernetes クラスター|<ul><li>組織の要件に適合する、カスタマイズされた独自のインフラストラクチャーを作成できる。</li><li>IBM Cloud インフラストラクチャー (SoftLayer) によって提供されるリソースを使用して、機密保護機能のある専用の Kubernetes マスター、ワーカー・ノード、仮想ネットワーク、そしてストレージをプロビジョンできる。</li><li>クラスターを使用可能にしておくために {{site.data.keyword.IBM_notm}} によって継続的にモニターされて更新される、完全に管理された Kubernetes マスターを利用できる。</li><li>トラステッド・コンピューティングを使用するベア・メタル・サーバーとしてワーカー・ノードをプロビジョンできるオプションがある。</li><li>統合されたセキュアなボリューム・サービスによって、永続データを保管し、Kubernetes ポッド間でデータを共有し、必要に応じてデータを復元することができる。</li><li>すべてのネイティブ Kubernetes API をフルサポートすることによる利点。</li></ul>|
| 高可用性を高めるマルチゾーン・クラスター | <ul><li>ワーカー・プールを使用して、同じマシン・タイプ (CPU、メモリー、仮想または物理) のワーカー・ノードを容易に管理する。</li><li>選択したマルチゾーンにノードを均等に分散させ、アプリのアンチアフィニティー・ポッド・デプロイメントを使用して、ゾーン障害から保護する。</li><li>別のクラスターにリソースを重複させるのではなく、マルチゾーン・クラスターを使用して、コストを削減する。</li><li>クラスターの各ゾーンに自動的にセットアップされるマルチゾーン・ロード・バランサー (MZLB) を使用するアプリの自動ロード・バランシングによる利点。</li></ul>|
| 高可用性のマスター | <ul><li>クラスターの作成時に自動的にプロビジョンされる高可用性マスターによって、クラスターのダウン時間 (マスターの更新時など) が削減される。</li><li>ゾーン障害からクラスターを保護するために、[複数ゾーン・クラスター](/docs/containers?topic=containers-ha_clusters#multizone)で複数のゾーンにマスターを分散させる。</li></ul> |
|Vulnerability Advisor によるイメージ・セキュリティー・コンプライアンス|<ul><li>保護された Docker プライベート・イメージ・レジストリーに独自のリポジトリーをセットアップでき、ここにイメージが格納され、組織内のすべてのユーザーによって共有される。</li><li>プライベート {{site.data.keyword.Bluemix_notm}} レジストリー内のイメージを自動スキャンすることによる利点。</li><li>イメージ内で使用されるオペレーティング・システムに固有の推奨を確認して、潜在的な脆弱性を修正できる。</li></ul>|
|クラスターの正常性に関する継続的なモニター|<ul><li>クラスター・ダッシュボードを使用して、クラスター、ワーカー・ノード、およびコンテナー・デプロイメントの正常性を素早く参照して管理できる。</li><li>{{site.data.keyword.monitoringlong}}を使用して詳細な使用量メトリックを確認し、ワークロードに合わせてクラスターを素早く拡張することができる。</li><li>{{site.data.keyword.loganalysislong}}を使用してロギング情報を参照して、クラスター・アクティビティーの詳細を確認できる。</li></ul>|
|アプリにパブリック・アクセスできるよう安全に公開する|<ul><li>インターネットからクラスター内のサービスにアクセスする方法を、パブリック IP アドレス、{{site.data.keyword.IBM_notm}} 提供の経路、独自のカスタム・ドメインの中から選択できる。</li></ul>|
|{{site.data.keyword.Bluemix_notm}} サービスの統合|<ul><li>Watson API、Blockchain、データ・サービス、モノのインターネットなどの {{site.data.keyword.Bluemix_notm}} サービスを統合してアプリに付加的な機能を追加する。</li></ul>|
{: caption="{{site.data.keyword.containerlong_notm}} の利点" caption-side="top"}

さあ始めましょう。 [Kubernetes クラスターを作成するチュートリアル](/docs/containers?topic=containers-cs_cluster_tutorial#cs_cluster_tutorial)を参照してください。

<br />


## オファーとそれらの組み合わせの比較
{: #differentiation}

{{site.data.keyword.containerlong_notm}} は、{{site.data.keyword.Bluemix_notm}} Public、{{site.data.keyword.Bluemix_notm}} Private、またはハイブリッド構成で実行できます。
{:shortdesc}


<table>
<caption>{{site.data.keyword.containershort_notm}} の各構成の相違点</caption>
<col width="22%">
<col width="78%">
 <thead>
 <th>{{site.data.keyword.containershort_notm}} 構成</th>
 <th>説明</th>
 </thead>
 <tbody>
 <tr>
 <td>{{site.data.keyword.Bluemix_notm}} Public、オフプレミス</td>
 <td>[共有ハードウェアまたは専用ハードウェア、あるいはベア・メタル・マシン上の](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes) {{site.data.keyword.Bluemix_notm}} Public では、{{site.data.keyword.containerlong_notm}} を使用してクラウド上のクラスターでアプリをホストできます。 また、複数のゾーンのワーカー・プールを使用するクラスターを作成して、アプリの高可用性を向上させることもできます。 {{site.data.keyword.Bluemix_notm}} Public の {{site.data.keyword.containerlong_notm}} では、Docker コンテナー、Kubernetes テクノロジー、直観的なユーザー・エクスペリエンス、標準装備のセキュリティーと分離機能を結合させることにより、コンピュート・ホストのクラスター内でコンテナー化アプリのデプロイメント、操作、スケーリング、モニタリングを自動化する強力なツールが提供されます。<br><br>詳しくは、[{{site.data.keyword.containerlong_notm}} テクノロジー](/docs/containers?topic=containers-ibm-cloud-kubernetes-service-technology)を参照してください。
 </td>
 </tr>
 <tr>
 <td>{{site.data.keyword.Bluemix_notm}} Private、オンプレミス</td>
 <td>{{site.data.keyword.Bluemix_notm}} Private は、ユーザー自身のマシン上にローカルにインストールできるアプリケーション・プラットフォームです。 ファイアウォールの内側にあるユーザー独自の制御された環境で、オンプレミスのコンテナー化アプリを開発して管理する必要がある場合は、{{site.data.keyword.Bluemix_notm}} Private で Kubernetes を使用することを選択できます。 <br><br>詳しくは、[{{site.data.keyword.Bluemix_notm}} Private の製品資料![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_1.2.0/kc_welcome_containers.html)を参照してください。
 </td>
 </tr>
 <tr>
 <td>ハイブリッド構成</td>
 <td>ハイブリッドとは、{{site.data.keyword.Bluemix_notm}} Public オフプレミスで実行されるサービスと、{{site.data.keyword.Bluemix_notm}} Private 内のアプリなどのオンプレミスで実行される他のサービスを組み合わせて使用することです。ハイブリッド構成の例を以下に示します。 <ul><li>{{site.data.keyword.Bluemix_notm}} Public で {{site.data.keyword.containerlong_notm}} を使用してクラスターをプロビジョンしながら、そのクラスターを社内データベースに接続する。</li><li>{{site.data.keyword.Bluemix_notm}} Private で {{site.data.keyword.containerlong_notm}} を使用してクラスターをプロビジョンしながら、アプリをそのクラスターにデプロイする。 ただし、そのアプリは {{site.data.keyword.Bluemix_notm}} Public の {{site.data.keyword.ibmwatson}} サービス ({{site.data.keyword.toneanalyzershort}} など) を使用する可能性がある。</li></ul><br>{{site.data.keyword.Bluemix_notm}} Public または Dedicated で実行しているサービスとオンプレミスで実行しているサービスの間の通信を有効にするには、[VPN 接続をセットアップする](/docs/containers?topic=containers-vpn)必要があります。 詳しくは、[{{site.data.keyword.Bluemix_notm}} Privateでの {{site.data.keyword.containerlong_notm}} の使用 ](/docs/containers?topic=containers-hybrid_iks_icp) を参照してください。
 </td>
 </tr>
 </tbody>
</table>

<br />


## フリー・クラスターと標準クラスターの比較
{: #cluster_types}

1 つのフリー・クラスターまたは任意の数の標準クラスターを作成できます。 フリー・クラスターを試して、いくつかの Kubernetes 機能を習得することもできますし、標準クラスターを作成して、Kubernetes の機能全体を使用してアプリをデプロイすることもできます。 フリー・クラスターは 30 日後に自動的に削除されます。
{:shortdesc}

フリー・クラスターを使用してから、標準クラスターにアップグレードする場合は、[標準クラスターを作成できます](/docs/containers?topic=containers-clusters#clusters_ui)。 その後、フリー・クラスターで作成した Kubernetes リソースの任意の YAML を標準クラスターにデプロイします。

|特性|フリー・クラスター|標準クラスター|
|---------------|-------------|-----------------|
|[クラスター内ネットワーキング](/docs/containers?topic=containers-security#network)|<img src="images/confirm.svg" width="32" alt="機能は使用可能" style="width:32px;" />|<img src="images/confirm.svg" width="32" alt="機能は使用可能" style="width:32px;" />|
|[NodePort サービスによる安定 IP アドレス以外へのパブリック・ネットワーク・アプリ・アクセス](/docs/containers?topic=containers-nodeport)|<img src="images/confirm.svg" width="32" alt="機能は使用可能" style="width:32px;" />|<img src="images/confirm.svg" width="32" alt="機能は使用可能" style="width:32px;" />|
|[ユーザー・アクセス管理](/docs/containers?topic=containers-users#access_policies)|<img src="images/confirm.svg" width="32" alt="機能は使用可能" style="width:32px;" />|<img src="images/confirm.svg" width="32" alt="機能は使用可能" style="width:32px;" />|
|[クラスターとアプリからの {{site.data.keyword.Bluemix_notm}} サービス・アクセス](/docs/containers?topic=containers-service-binding#bind-services)|<img src="images/confirm.svg" width="32" alt="機能は使用可能" style="width:32px;" />|<img src="images/confirm.svg" width="32" alt="機能は使用可能" style="width:32px;" />|
|[非永続ストレージ用のワーカー・ノードのディスク・スペース](/docs/containers?topic=containers-storage_planning#non_persistent_overview)|<img src="images/confirm.svg" width="32" alt="機能は使用可能" style="width:32px;" />|<img src="images/confirm.svg" width="32" alt="機能は使用可能" style="width:32px;" />|
| [すべての {{site.data.keyword.containerlong_notm}} 地域でクラスターを作成する機能](/docs/containers?topic=containers-regions-and-zones) | | <img src="images/confirm.svg" width="32" alt="機能は使用可能" style="width:32px;" /> |
|[アプリの高可用性を高めるマルチゾーン・クラスター](/docs/containers?topic=containers-ha_clusters#multizone) | |<img src="images/confirm.svg" width="32" alt="機能は使用可能" style="width:32px;" />|
| マスターの複製による可用性の向上 | | <img src="images/confirm.svg" width="32" alt="機能は使用可能" style="width:32px;" /> |
|[能力の向上のためにワーカー・ノード数を引き上げ可能](/docs/containers?topic=containers-app#app_scaling)| |<img src="images/confirm.svg" width="32" alt="機能は使用可能" style="width:32px;" />|
|[NFS ファイル・ベースの永続ストレージのボリューム](/docs/containers?topic=containers-file_storage#file_storage)| |<img src="images/confirm.svg" width="32" alt="機能は使用可能" style="width:32px;" />|
|[ネットワーク・ロード・バランサー (NLB) サービスによる安定 IP アドレスへのパブリック・ネットワークまたはプライベート・ネットワークのアプリ・アクセス](/docs/containers?topic=containers-loadbalancer)| |<img src="images/confirm.svg" width="32" alt="機能は使用可能" style="width:32px;" />|
|[Ingress サービスによる安定 IP アドレスとカスタマイズ可能 URL へのパブリック・ネットワークのアプリ・アクセス](/docs/containers?topic=containers-ingress#planning)| |<img src="images/confirm.svg" width="32" alt="機能は使用可能" style="width:32px;" />|
|[ポータブル・パブリック IP アドレス](/docs/containers?topic=containers-subnets#review_ip)| |<img src="images/confirm.svg" width="32" alt="機能は使用可能" style="width:32px;" />|
|[ロギングとモニタリング](/docs/containers?topic=containers-health#logging)| |<img src="images/confirm.svg" width="32" alt="機能は使用可能" style="width:32px;" />|
|[物理 (ベア・メタル) サーバー上にワーカー・ノードをプロビジョンするためのオプション](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes) | |<img src="images/confirm.svg" width="32" alt="機能は使用可能" style="width:32px;" />|
|[トラステッド・コンピューティングを使用するベア・メタル・ワーカーをプロビジョンするためのオプション](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes) | |<img src="images/confirm.svg" width="32" alt="機能は使用可能" style="width:32px;" />|
{: caption="フリー・クラスターと標準クラスターの特性" caption-side="top"}
