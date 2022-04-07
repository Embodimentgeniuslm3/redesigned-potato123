---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-07"

keywords: kubernetes, iks, clusters, worker nodes, worker pools, delete

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
{:gif: data-image-type='gif'}


# クラスターの作成
{: #clusters}

{{site.data.keyword.containerlong}} で Kubernetes クラスターを作成します。
{: shortdesc}

基礎的な内容から始めたい場合は、 [Kubernetes クラスターを作成するチュートリアル](/docs/containers?topic=containers-cs_cluster_tutorial#cs_cluster_tutorial)を参照してください。 どのクラスターのセットアップを選べばよいかわからない場合は、[クラスター・ネットワークのセットアップの計画](/docs/containers?topic=containers-plan_clusters)を参照してください。
{: tip}

前にクラスターを作成したことがあり、簡単なコマンド例を探しているだけであれば、 以下の例をお試しください。
*  **フリー・クラスター**:
   ```
   ibmcloud ks cluster-create --name my_cluster
   ```
   {: pre}
*  **標準クラスター、共有仮想マシン**:
   ```
   ibmcloud ks cluster-create --name my_cluster --zone dal10 --machine-type b3c.4x16 --hardware shared --workers 3 --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID>
   ```
   {: pre}
*  **標準クラスター、ベアメタル**:
   ```
   ibmcloud ks cluster-create --name my_cluster --zone dal10 --machine-type mb2c.4x32 --hardware dedicated --workers 3 --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID>
   ```
   {: pre}
*  **標準クラスター、VRF 対応アカウントでパブリックおよびプライベートのサービス・エンドポイントを使用できる仮想マシン**:
   ```
   ibmcloud ks cluster-create --name my_cluster --zone dal10 --machine-type b3c.4x16 --hardware shared --workers 3 --public-service-endpoint --private-service-endpoint --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID>
   ```
   {: pre}
*  **プライベート VLAN およびプライベート・サービス・エンドポイントのみを使用する標準クラスター**:
   ```
   ibmcloud ks cluster-create --name my_cluster --zone dal10 --machine-type b3c.4x16 --hardware shared --workers 3 --private-service-endpoint --private-vlan <private_VLAN_ID> --private-only
   ```
   {: pre}

<br />


## アカウント・レベルでのクラスター作成の準備
{: #cluster_prepare}

{{site.data.keyword.containerlong_notm}} の {{site.data.keyword.Bluemix_notm}} アカウントを準備します。アカウント管理者がこれを作成した後、この準備は、クラスターを作成するたびに変更する必要がない可能性があります。しかし、クラスターを作成するたびに、現行のアカウント・レベルの状態が必要にかなっているか検証することができます。
{: shortdesc}

1. [有料アカウント ({{site.data.keyword.Bluemix_notm}} 従量課金またはサブスクリプション) を作成するか、有料アカウントにアップグレードします](https://cloud.ibm.com/registration/)。

2. クラスターを作成しようとしている地域内で[{{site.data.keyword.containerlong_notm}} API キーをセットアップ](/docs/containers?topic=containers-users#api_key)します。 以下のような、クラスターを作成するための該当する許可を API キーに割り当てます。
  * IBM Cloud インフラストラクチャー (SoftLayer) に対する**スーパーユーザー**の役割。
  * {{site.data.keyword.containerlong_notm}} に対するアカウント・レベルの**管理者**のプラットフォーム管理役割。
  * {{site.data.keyword.registrylong_notm}} に対するアカウント・レベルの**管理者**のプラットフォーム管理役割。 2018 年 10 月 4 日より前に作成されたアカウントの場合は、[{{site.data.keyword.registryshort_notm}} に対して {{site.data.keyword.Bluemix_notm}} IAM ポリシーを有効にする](/docs/services/Registry?topic=registry-user#existing_users)必要があります。 IAM ポリシーを使用すると、レジストリー名前空間などのリソースへのアクセスを制御できます。

  アカウント所有者には、 必要な許可が既にあります。 クラスターを作成すると、ご使用の資格情報を使用してその地域とリソース・グループの API キーが設定されます。
  {: tip}

3. {{site.data.keyword.containerlong_notm}} に対する**管理者**のプラットフォーム管理役割があることを確認します。 クラスターで専用レジストリーからイメージをプルできるようにするには、{{site.data.keyword.registrylong_notm}} に対する**管理者**のプラットフォーム役割も必要です。
  1. [{{site.data.keyword.Bluemix_notm}} コンソール ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/) のメニュー・バーで、**「管理」> 「アクセス (IAM)」**をクリックします。
  2. **「ユーザー」**ページをクリックし、テーブルから自分を選択します。
  3. **「アクセス・ポリシー」**タブから、**「役割」**が**「管理者」**になっていることを確認します。 アカウントに属するすべてのリソースか、少なくとも {{site.data.keyword.containershort_notm}} に対して、**管理者**になることができます。 **注**: アカウント全体ではなく 1 つのリソース・グループまたは地域のみの {{site.data.keyword.containershort_notm}} に対する**管理者**役割がある場合、アカウントの VLAN を参照するには、少なくともアカウント・レベルで**ビューアー**役割がなければなりません。
  <p class="tip">アカウント管理者によって、**管理者**のプラットフォーム役割がサービス役割として同時に割り当てられていないことを確認します。 プラットフォーム役割とサービス役割は別々に割り当てる必要があります。</p>

4. ご使用のアカウントで複数のリソース・グループが使用されている場合は、[リソース・グループを管理](/docs/containers?topic=containers-users#resource_groups)するためのアカウントの戦略を考えます。
  * {{site.data.keyword.Bluemix_notm}} にログインする際にターゲットとして設定するリソース・グループ内にクラスターを作成します。 リソース・グループをターゲットとして設定しない場合は、デフォルトのリソース・グループが自動的にターゲットとして設定されます。
  * デフォルトとは異なるリソース・グループにクラスターを作成する場合は、そのリソース・グループに対する**ビューアー**以上の役割が必要です。 そのリソース・グループに対する役割はないものの、リソース・グループ内のサービスに対する**管理者**ではある場合、デフォルトのリソース・グループ内にクラスターが作成されます。
  * クラスターのリソース・グループを変更することはできません。 また、`ibmcloud ks cluster-service-bind` [コマンド](/docs/containers-cli-plugin?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_service_bind)を使用して [{{site.data.keyword.Bluemix_notm}} サービスと統合](/docs/containers?topic=containers-service-binding#bind-services)する必要がある場合は、このサービスがクラスターと同じリソース・グループにある必要があります。 {{site.data.keyword.registrylong_notm}} のようにリソース・グループを使用しないサービスや、{{site.data.keyword.la_full_notm}} のようにサービス・バインディングを必要としないサービスは、異なるリソース・グループにクラスターがあっても機能します。
  * [メトリック用に {{site.data.keyword.monitoringlong_notm}}](/docs/containers?topic=containers-health#view_metrics) を使用する計画の場合は、メトリックの命名に関する競合を避けるために、アカウントに属するすべてのリソース・グループと地域において固有の名前をクラスターに付けるよう計画します。
  * `default` リソース・グループにフリー・クラスターが作成されます。

5. **標準クラスター**: クラスターがワークロードおよび環境のニーズを満たすように、クラスターの[ネットワーク・セットアップ](/docs/containers?topic=containers-plan_clusters)を計画します。次に、IBM Cloud インフラストラクチャー (SoftLayer) ネットワーキングをセットアップして、マスター間およびユーザー間の通信を可能にします。
  * プライベート・サービス・エンドポイントのみ、またはパブリック・サービス・エンドポイントとプライベート・サービス・エンドポイント (インターネット向けワークロードの実行またはオンプレミス・データ・センターの拡張) を使用するには、以下のようにします。
    1. ご使用の IBM Cloud インフラストラクチャー (SoftLayer) アカウントで [VRF](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) を有効にします。
    2. [{{site.data.keyword.Bluemix_notm}} アカウントでサービス・エンドポイントを使用できるようにします](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started)。
    <p class="note">許可されたクラスター・ユーザーは、{{site.data.keyword.Bluemix_notm}} プライベート・ネットワークの中で作業している場合、または [VPN 接続](/docs/infrastructure/iaas-vpn?topic=VPN-gettingstarted-with-virtual-private-networking)や [{{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link) 経由でプライベート・ネットワークに接続している場合に、プライベート・サービス・エンドポイントを介して、Kubernetes マスターにアクセスできます。ただし、プライベート・サービス・エンドポイントを介した Kubernetes マスターとの通信は、<code>166.X.X.X</code> IP アドレス範囲を経由する必要があり、これは VPN 接続から、および {{site.data.keyword.Bluemix_notm}} Direct Link を介してルーティング可能ではありません。プライベート・ネットワーク・ロード・バランサー (NLB) を使用して、クラスター・ユーザーのマスターのプライベート・サービス・エンドポイントを公開できます。プライベート NLB は、ユーザーが VPN または {{site.data.keyword.Bluemix_notm}} Direct Link 接続を使用してアクセスできる内部 <code>10.X.X.X</code> IP アドレス範囲として、マスターのプライベート・サービス・エンドポイントを公開します。プライベート・サービス・エンドポイントのみを有効にする場合は、Kubernetes ダッシュボードを使用するか、パブリック・サービス・エンドポイントを一時的に有効にして、プライベート NLB を作成できます。詳しくは、[プライベート・サービス・エンドポイントを介したクラスターへのアクセス](/docs/containers?topic=containers-clusters#access_on_prem)を参照してください。</p>

  * パブリック・サービス・エンドポイントのみ (インターネット向けワークロードの実行) を使用するには、以下のようにします。
    1. IBM Cloud インフラストラクチャー (SoftLayer) アカウントに対して [VLAN スパンニング](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning)を有効にして、ワーカー・ノードがプライベート・ネットワーク上で相互に通信できるようにします。 この操作を実行するには、**「ネットワーク」>「ネットワーク VLAN スパンニングの管理」**で設定する[インフラストラクチャー権限](/docs/containers?topic=containers-users#infra_access)が必要です。ない場合は、アカウント所有者に対応を依頼してください。 VLAN スパンニングが既に有効になっているかどうかを確認するには、`ibmcloud ks vlan-spanning-get<region>` [コマンド](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get)を使用します。
  * ゲートウェイ・デバイスを使用する (オンプレミス・データ・センターを拡張する) には、以下のようにします。
    1. IBM Cloud インフラストラクチャー (SoftLayer) アカウントに対して [VLAN スパンニング](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning)を有効にして、ワーカー・ノードがプライベート・ネットワーク上で相互に通信できるようにします。 この操作を実行するには、**「ネットワーク」>「ネットワーク VLAN スパンニングの管理」**で設定する[インフラストラクチャー権限](/docs/containers?topic=containers-users#infra_access)が必要です。ない場合は、アカウント所有者に対応を依頼してください。 VLAN スパンニングが既に有効になっているかどうかを確認するには、`ibmcloud ks vlan-spanning-get<region>` [コマンド](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get)を使用します。
    2. ゲートウェイ・デバイスを構成します。例えば、ファイアウォールとして機能し、必要なトラフィックを許可し、不要なトラフィックをブロックする [Virtual Router Appliance](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-about-the-vra) または [Fortigate Security Appliance](/docs/services/vmwaresolutions/services?topic=vmware-solutions-fsa_considerations) をセットアップできます。
    3. 各地域の[必要なプライベート IP アドレスとポートを開き](/docs/containers?topic=containers-firewall#firewall_outbound)、マスター・ノードとワーカー・ノードが通信できるようにし、使用する予定の {{site.data.keyword.Bluemix_notm}} サービスを探します。

<br />


## クラスター・レベルでのクラスター作成の準備
{: #prepare_cluster_level}

クラスターを作成するためのアカウントをセットアップしたら、クラスターのセットアップを準備します。この準備は、クラスターを作成するたびに、そのクラスターに影響があります。
{: shortdesc}

1. [フリー・クラスターまたは標準クラスター](/docs/containers?topic=containers-cs_ov#cluster_types)のどちらにするかを決定します。 フリー・クラスターを 1 つ作成して機能を 30 日間試すことも、ハードウェア分離を選択して完全にカスタマイズ可能な標準クラスターを作成することもできます。 クラスター・パフォーマンスの利点を活かし、制御するには、標準クラスターを作成します。

2. 標準クラスターの場合は、クラスターのセットアップを計画します。
  * [単一ゾーン](/docs/containers?topic=containers-ha_clusters#single_zone)・クラスターと[複数ゾーン](/docs/containers?topic=containers-ha_clusters#multizone)・クラスターのどちらを作成するかを決定します。 複数ゾーン・クラスターは特定のロケーションでのみ提供されていることに注意してください。
  * クラスターのワーカー・ノードにとって必要な[ハードウェアと分離](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)のタイプを選択します。仮想マシンとベアメタル・マシンのどちらにするか決定することも含まれます。

3. 標準クラスターの場合は、{{site.data.keyword.Bluemix_notm}} コンソールで[コストを見積もる](/docs/billing-usage?topic=billing-usage-cost#cost)ことができます。 見積もりツールに含まれない可能性がある料金について詳しくは、[価格設定および請求](/docs/containers?topic=containers-faqs#charges)を参照してください。

4. オンプレミス・データ・センターを拡張するクラスターなど、ファイアウォールの背後にある環境でクラスターを作成する場合は、使用する予定の {{site.data.keyword.Bluemix_notm}} サービスの[パブリック IP およびプライベート IP へのアウトバウンド・ネットワーク・トラフィックを許可します](/docs/containers?topic=containers-firewall#firewall_outbound)。

<br />


## フリー・クラスターの作成
{: #clusters_free}

{{site.data.keyword.containerlong_notm}} の作業に慣れるために、フリー・クラスターを 1 つ使用することができます。 フリー・クラスターで、用語について学習し、チュートリアルを行い、やるべきことを理解した後に、実動レベルの標準クラスターの使用に踏み出すことができます。 ご安心ください。有料アカウントの場合でもフリー・クラスターを利用できます。
{: shortdesc}

フリー・クラスターには、2vCPU と 4GB のメモリーでセットアップされたワーカー・ノードが 1 つあり、存続期間は 30 日間です。その後、クラスターは期限切れになり、クラスターとそのデータは削除されます。 削除されたデータは、{{site.data.keyword.Bluemix_notm}} でバックアップされず、リストアできません。 重要なデータは必ずバックアップしてください。
{: note}

### コンソールでのフリー・クラスターの作成
{: #clusters_ui_free}

1. [カタログ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/catalog?category=containers) で、クラスターを作成する **{{site.data.keyword.containershort_notm}}** を選択します。
2. **「無料」**クラスター・プランを選択します。
3. クラスターをデプロイするジオグラフィーを選択します。
4. ジオグラフィーで大都市ロケーションを選択します。クラスターはこの大都市内のゾーンに作成されます。
5. 名前をクラスターに付けます。 名前は先頭が文字でなければならず、文字、数字、およびハイフン (-) を使用できます。35 文字以内でなければなりません。 地域をまたいで固有の名前を使用します。
6. **「クラスターの作成」**をクリックします。 デフォルトでは、ワーカー・ノードを 1 つ含むワーカー・プールが作成されます。 **「ワーカー・ノード」**タブでワーカー・ノードのデプロイメントの進行状況を確認できます。 デプロイが完了すると、クラスターが**「概要」**タブに準備されていることが分かります。 クラスターの準備ができている場合でも、レジストリーのイメージ・プル・シークレットなどの他のサービスによって使用される、クラスターの一部がまだ処理中である可能性があることに注意してください。

  どのワーカー・ノードにも固有のワーカー・ノード ID とドメイン名が割り当てられます。それらをクラスター作成後に手動で変更してはいけません。 ID またはドメイン名を変更すると、Kubernetes マスターがクラスターを管理できなくなります。
  {: important}
7. クラスターが作成されたら、[CLI セッションを構成してクラスターの処理を開始](#access_cluster)できます。

### CLI でのフリー・クラスターの作成
{: #clusters_cli_free}

開始する前に、{{site.data.keyword.Bluemix_notm}} CLI および [{{site.data.keyword.containerlong_notm}} プラグイン](/docs/containers?topic=containers-cs_cli_install#cs_cli_install)をインストールしてください。

1. {{site.data.keyword.Bluemix_notm}} CLI にログインします。
  1. ログインし、プロンプトが出されたら {{site.data.keyword.Bluemix_notm}} 資格情報を入力します。
     ```
     ibmcloud login
     ```
     {: pre}

     フェデレーテッド ID がある場合は、`ibmcloud login --sso` を使用して、{{site.data.keyword.Bluemix_notm}} CLI にログインします。 ユーザー名を入力し、CLI 出力に示された URL を使用して、ワンタイム・パスコードを取得してください。 `--sso` なしではログインに失敗し、`--sso` オプションを指定すると成功する場合、フェデレーテッド ID があることがわかります。
     {: tip}

  2. 複数の {{site.data.keyword.Bluemix_notm}} アカウントがある場合は、Kubernetes クラスターを作成するアカウントを選択します。

  3. 特定の地域に空きクラスターを作成するには、その地域をターゲットにする必要があります。フリー・クラスターは、`ap-south`、`eu-central`、`uk-south`、または `us-south` で作成できます。クラスターは、この地域内のゾーンに作成されます。
     ```
     ibmcloud ks region-set
     ```
     {: pre}

2. クラスターを作成します。
  ```
  ibmcloud ks cluster-create --name my_cluster
  ```
  {: pre}

3. クラスターの作成が要求されたことを確認します。 ワーカー・ノード・マシンの注文には、数分かかることがあります。
    ```
    ibmcloud ks clusters
    ```
    {: pre}

    クラスターのプロビジョニングが完了すると、クラスターの状況が **deployed** に変わります。
    ```
    Name         ID                                   State      Created          Workers   Zone       Version     Resource Group Name
    my_cluster   paf97e8843e29941b49c598f516de72101   deployed   20170201162433   1         mil01      1.13.6      Default
    ```
    {: screen}

4. ワーカー・ノードの状況を確認します。
    ```
    ibmcloud ks workers --cluster <cluster_name_or_ID>
    ```
    {: pre}

    ワーカー・ノードの準備が完了すると、状態が **normal** に変わり、状況が **Ready** になります。 ノードの状況が**「Ready」**になったら、クラスターにアクセスできます。 クラスターの準備ができている場合でも、Ingress シークレットやレジストリーのイメージ・プル・シークレットなどの他のサービスによって使用される、クラスターの一部がまだ処理中である可能性があることに注意してください。
    ```
    ID                                                 Public IP       Private IP      Machine Type   State    Status   Zone        Version     Resource Group Name
    kube-mil01-paf97e8843e29941b49c598f516de72101-w1   169.xx.xxx.xxx  10.xxx.xx.xxx   free           normal   Ready    mil01       1.13.6      Default
    ```
    {: screen}

    どのワーカー・ノードにも固有のワーカー・ノード ID とドメイン名が割り当てられます。それらをクラスター作成後に手動で変更してはいけません。 ID またはドメイン名を変更すると、Kubernetes マスターがクラスターを管理できなくなります。
    {: important}

5. クラスターが作成されたら、[CLI セッションを構成してクラスターの処理を開始](#access_cluster)できます。

<br />


## 標準クラスターの作成
{: #clusters_standard}

{{site.data.keyword.Bluemix_notm}} CLI または {{site.data.keyword.Bluemix_notm}} コンソールを使用して、高可用性環境用の複数のワーカー・ノードなどのハードウェア分離および機能へのアクセスを選択して、完全にカスタマイズ可能な標準クラスターを作成します。
{: shortdesc}

### コンソールでの標準クラスターの作成
{: #clusters_ui}

1. [カタログ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/catalog?category=containers) で、クラスターを作成する **{{site.data.keyword.containershort_notm}}** を選択します。

2. クラスターを作成するリソース・グループを選択します。
  **注:**
    * 1 つのリソース・グループのみにクラスターを作成できます。また、クラスターの作成後にそのリソース・グループを変更することはできません。
    * デフォルト以外のリソース・グループにクラスターを作成するには、リソース・グループに対する[**ビューアー**以上の役割](/docs/containers?topic=containers-users#platform)が必要です。

3. クラスターをデプロイするジオグラフィーを選択します。

4. 固有の名前をクラスターに付けます。 名前は先頭が文字でなければならず、文字、数字、およびハイフン (-) を使用できます。35 文字以内でなければなりません。 地域をまたいで固有の名前を使用します。Ingress サブドメインの完全修飾ドメイン・ネームは、クラスター名と、クラスターがデプロイされる地域で形成されます。 Ingress サブドメインを地域内で固有にするために、クラスター名が切り捨てられ、Ingress ドメイン・ネームにランダムな値が付加されることがあります。
 **注**: 作成時に割り当てられる固有の ID またはドメイン名を変更すると、Kubernetes マスターがクラスターを管理できなくなります。

5. **「単一ゾーン (Single zone)」**または**「複数ゾーン (Multizone)」**の可用性を選択します。 複数ゾーン・クラスターでは、マスター・ノードは複数ゾーン対応のゾーンにデプロイされ、クラスターのリソースは複数のゾーンに分散されます。

6. 大都市およびゾーンの詳細情報を入力します。
  * 複数ゾーン・クラスター:
    1. 大都市ロケーションを選択します。パフォーマンスを最大化するために、物理的に最も近い大都市ロケーションを選択してください。ジオグラフィーによって選択肢が限定される可能性があります。
    2. クラスターをホストする特定のゾーンを選択します。 少なくとも 1 つのゾーンを選択する必要がありますが、ご希望の数だけ選択できます。 複数のゾーンを選択すると、その選択したゾーン間でワーカー・ノードが分散され、可用性が高くなります。 ゾーンを 1 つだけ選択した場合は、クラスターの作成後に[ゾーンを追加](/docs/containers?topic=containers-add_workers#add_zone)できます。
  * 単一ゾーン・クラスター: クラスターをホストするゾーンを 1 つ選択します。パフォーマンスを最大化するために、物理的に最も近いゾーンを選択してください。ジオグラフィーによって選択肢が限定される可能性があります。

7. ゾーンごとに VLAN を選択します。
  * インターネット向けワークロードを実行できるクラスターを作成するには、以下のようにします。
    1. ゾーンごとにパブリック VLAN とプライベート VLAN を IBM Cloud インフラストラクチャー (SoftLayer) アカウントから選択します。ワーカー・ノードは、プライベート VLAN を使用して相互に通信し、パブリック VLAN またはプライベート VLAN を使用して Kubernetes マスターと通信できます。そのゾーン内にパブリック VLAN またはプライベート VLAN がない場合は、パブリック VLAN とプライベート VLAN が自動的に作成されます。複数のクラスターで同じ VLAN を使用できます。
  * プライベート・ネットワーク上でのみオンプレミス・データ・センターを拡張するクラスターや、ゲートウェイ・デバイスを介して限定パブリック・アクセスを追加するオプション付きでオンプレミス・データ・センターを拡張するクラスターを作成するには、以下のようにします。
    1. ゾーンごとにプライベート VLAN を IBM Cloud インフラストラクチャー (SoftLayer) アカウントから選択します。ワーカー・ノードはプライベート VLAN を使用して相互に通信します。 ゾーン内にプライベート VLAN がない場合は、プライベート VLAN が自動的に作成されます。複数のクラスターで同じ VLAN を使用できます。
    2. パブリック VLAN として**「なし」**を選択します。

8. **「マスター・サービス・エンドポイント (Master service endpoint)」**で、Kubernetes マスターとワーカー・ノードの通信方法を選択します。
  * インターネット向けワークロードを実行できるクラスターを作成するには、以下のようにします。
    * {{site.data.keyword.Bluemix_notm}} アカウントで VRF およびサービス・エンドポイントが有効になっている場合は、**「プライベート・エンドポイントとパブリック・エンドポイントの両方 (Both private & public endpoints)」**を選択します。
    * VRF の有効化が不可能または不要な場合は、**「パブリック・エンドポイントのみ (Public endpoint only)」**を選択します。
  * オンプレミス・データ・センターのみを拡張するクラスター、またはオンプレミス・データ・センターを拡張し、エッジ・ワーカー・ノードとの限定パブリック・アクセスを提供するクラスターを作成するには、**「プライベート・エンドポイントとパブリック・エンドポイントの両方 (Both private & public endpoints)」**または**「プライベート・エンドポイントのみ (Private endpoint only)」**を選択します。{{site.data.keyword.Bluemix_notm}} アカウントで VRF およびサービス・エンドポイントが有効になっていることを確認します。プライベート・サービス・エンドポイントのみを有効にする場合は、[プライベート・ネットワーク・ロード・バランサーを介してマスター・エンドポイントを公開](#access_on_prem)して、ユーザーが VPN 接続または {{site.data.keyword.BluDirectLink}} 接続を介してマスターにアクセスできるようにする必要があります。
  * オンプレミス・データ・センターを拡張し、ゲートウェイ・デバイスによる限定パブリック・アクセスを提供するクラスターを作成するには、**「パブリック・エンドポイントのみ (Public endpoint only)」**を選択します。

9. デフォルトのワーカー・プールを構成します。 ワーカー・プールとは、同じ構成を共有するワーカー・ノードのグループのことです。 後で、別のワーカー・プールをクラスターに追加することができます。
  1. クラスター・マスター・ノードおよびワーカー・ノードの Kubernetes API サーバーのバージョンを選択します。
  2. マシン・タイプを選択して、ワーカー・フレーバーをフィルターに掛けます。仮想マシンは時間単位で請求され、ベア・メタル・マシンは月単位で請求されます。
    - **ベアメタル**: 月単位で課金されるベアメタル・サーバーは、お客様が注文した後に IBM Cloud インフラストラクチャー (SoftLayer) によって手動でプロビジョンされます。完了するまでに 1 営業日以上かかることがあります。 多くのリソースとホスト制御を必要とする高性能アプリケーションには、ベア・メタルが最適です。 ワーカー・ノードが改ざんされていないことを検証するために、[トラステッド・コンピューティング](/docs/containers?topic=containers-security#trusted_compute)を有効にすることも選択できます。 トラステッド・コンピューティングは、選ばれたベア・メタル・マシン・タイプでのみ使用できます。 例えば、`mgXc` GPU フレーバーではトラステッド・コンピューティングはサポートされません。 クラスターの作成時にトラストを有効にしなかった場合に、後で有効にするには、`ibmcloud ks feature-enable` [コマンド](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable)を使用します。 トラストを有効にした後に無効にすることはできません。
    ベア・メタル・マシンは、必ず確認してからプロビジョンしてください。 月単位で課金されるので、誤って注文した後にすぐに解約しても、1 カ月分の料金が課金されます。
    {:tip}
    - **仮想 - 共有**: ハイパーバイザーや物理ハードウェアなどのインフラストラクチャー・リソースはお客様と IBM の他の利用者の間で共有されます。ただし、各ワーカー・ノードにはお客様だけがアクセスできます。 ほとんどの場合はこの安価なオプションで十分ですが、企業ポリシーに照らしてパフォーマンスとインフラストラクチャーの要件を確認する必要があります。
    - **仮想 - 専用**: ワーカー・ノードはお客様のアカウント専用のインフラストラクチャーでホストされます。 物理リソースは完全に分離されます。
  3. フレーバーを選択します。各ワーカー・ノードにセットアップされ、コンテナーで使用できるようになる仮想 CPU、メモリー、ディスク・スペースの量は、フレーバーによって定義されます。使用可能なベア・メタル・マシンと仮想マシンのタイプは、クラスターをデプロイするゾーンによって異なります。 クラスターを作成した後、ワーカーまたはプールをクラスターに追加して別のマシン・タイプを追加できます。
  4. クラスターに必要なワーカー・ノードの数を指定します。 入力した数のワーカーが、選択した数のゾーンのそれぞれに複製されます。 つまり、2 つのゾーンがある場合に、3 つのワーカー・ノードを選択すると、6 つのノードがプロビジョンされ、各ゾーンに 3 つのノードが存在することになります。

10. **「クラスターの作成」**をクリックします。 指定した数のワーカーを含むワーカー・プールが作成されます。 **「ワーカー・ノード」**タブでワーカー・ノードのデプロイメントの進行状況を確認できます。 デプロイが完了すると、クラスターが**「概要」**タブに準備されていることが分かります。 クラスターの準備ができている場合でも、Ingress シークレットやレジストリーのイメージ・プル・シークレットなどの他のサービスによって使用される、クラスターの一部がまだ処理中である可能性があることに注意してください。

  どのワーカー・ノードにも固有のワーカー・ノード ID とドメイン名が割り当てられます。それらをクラスター作成後に手動で変更してはいけません。 ID またはドメイン名を変更すると、Kubernetes マスターがクラスターを管理できなくなります。
  {: important}

11. クラスターが作成されたら、[CLI セッションを構成してクラスターの処理を開始](#access_cluster)できます。

### CLI での標準クラスターの作成
{: #clusters_cli_steps}

開始する前に、{{site.data.keyword.Bluemix_notm}} CLI および [{{site.data.keyword.containerlong_notm}} プラグイン](/docs/containers?topic=containers-cs_cli_install#cs_cli_install)をインストールしてください。

1. {{site.data.keyword.Bluemix_notm}} CLI にログインします。
  1. ログインし、プロンプトが出されたら {{site.data.keyword.Bluemix_notm}} 資格情報を入力します。
     ```
     ibmcloud login
     ```
     {: pre}

     フェデレーテッド ID がある場合は、`ibmcloud login --sso` を使用して、{{site.data.keyword.Bluemix_notm}} CLI にログインします。 ユーザー名を入力し、CLI 出力に示された URL を使用して、ワンタイム・パスコードを取得してください。 `--sso` なしではログインに失敗し、`--sso` オプションを指定すると成功する場合、フェデレーテッド ID があることがわかります。
     {: tip}

  2. 複数の {{site.data.keyword.Bluemix_notm}} アカウントがある場合は、Kubernetes クラスターを作成するアカウントを選択します。

  3. デフォルト以外のリソース・グループにクラスターを作成するには、そのリソース・グループをターゲットとして設定します。 **注:**
      * 1 つのリソース・グループのみにクラスターを作成できます。また、クラスターの作成後にそのリソース・グループを変更することはできません。
      * リソース・グループに対する[**ビューアー**以上の役割](/docs/containers?topic=containers-users#platform)が必要です。

      ```
      ibmcloud target -g <resource_group_name>
      ```
      {: pre}

2. 使用可能なゾーンを確認します。以下のコマンドの出力では、ゾーンの**「ロケーション・タイプ (Location Type)」**は `dc` です。複数のゾーンにクラスターを広げるには、[複数ゾーン対応ゾーン](/docs/containers?topic=containers-regions-and-zones#zones)内でクラスターを作成する必要があります。 複数ゾーン対応ゾーンでは、**「複数ゾーンの大都市 (Multizone Metro)」**列にメトロ値があります。
    ```
    ibmcloud ks supported-locations
    ```
    {: pre}
    国外のゾーンを選択する場合は、その国にデータを物理的に保管する前に法的な許可を得なければならないことがあります。
    {: note}

3. そのゾーンで使用可能なワーカー・ノードのフレーバーを確認します。ワーカー・フレーバーの指定によって、各ワーカー・ノードで使用可能な仮想コンピュート・ホストまたは物理コンピュート・ホストが決まります。
  - **仮想**: 仮想マシンは時間単位で課金され、共有ハードウェアまたは専用ハードウェア上にプロビジョンされます。
  - **物理**: 月単位で課金されるベアメタル・サーバーは、お客様が注文した後に IBM Cloud インフラストラクチャー (SoftLayer) によって手動でプロビジョンされます。完了するまでに 1 営業日以上かかることがあります。 多くのリソースとホスト制御を必要とする高性能アプリケーションには、ベア・メタルが最適です。
  - **トラステッド・コンピューティングを使用する物理マシン**: ベア・メタル・ワーカー・ノードが改ざんされていないことを検証するために、[トラステッド・コンピューティング](/docs/containers?topic=containers-security#trusted_compute)を有効にすることもできます。 トラステッド・コンピューティングは、選ばれたベア・メタル・マシン・タイプでのみ使用できます。 例えば、`mgXc` GPU フレーバーではトラステッド・コンピューティングはサポートされません。 クラスターの作成時にトラストを有効にしなかった場合に、後で有効にするには、`ibmcloud ks feature-enable` [コマンド](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable)を使用します。 トラストを有効にした後に無効にすることはできません。
  - **マシン・タイプ**: デプロイするマシン・タイプを決定するには、[使用可能なワーカー・ノードのハードウェア](/docs/containers?topic=containers-planning_worker_nodes#shared_dedicated_node)のコア、メモリー、ストレージの組み合わせについて検討してください。 クラスターを作成した後、[ワーカー・プールを追加](/docs/containers?topic=containers-add_workers#add_pool)して、別の物理または仮想マシン・タイプを追加できます。

     ベア・メタル・マシンは、必ず確認してからプロビジョンしてください。 月単位で課金されるので、誤って注文した後にすぐに解約しても、1 カ月分の料金が課金されます。
     {:tip}

  ```
  ibmcloud ks machine-types --zone <zone>
  ```
  {: pre}

4. このアカウントの IBM Cloud インフラストラクチャー (SoftLayer) にゾーンの VLAN が既に存在しているかどうかを確認します。
  ```
  ibmcloud ks vlans --zone <zone>
  ```
  {: pre}

  出力例:
  ```
  ID        Name   Number   Type      Router
        1519999   vlan   1355     private   bcr02a.dal10
        1519898   vlan   1357     private   bcr02a.dal10
        1518787   vlan   1252     public    fcr02a.dal10
        1518888   vlan   1254     public    fcr02a.dal10
  ```
  {: screen}
  * インターネット向けワークロードを実行できるクラスターを作成するには、パブリック VLAN とプライベート VLAN が存在するかどうかを確認します。パブリック VLAN およびプライベート VLAN が既に存在する場合、対応するルーターをメモに取ります。 必ず、プライベート VLAN ルーターの先頭は <code>bcr</code> (バックエンド・ルーター)、パブリック VLAN ルーターの先頭は <code>fcr</code> (フロントエンド・ルーター) になります。 クラスターを作成し、パブリック VLAN とプライベート VLAN を指定するときには、それらの接頭部の後の番号と文字の組み合わせが一致する必要があります。 このサンプル出力では、すべてのルーターに
`02a.dal10` が含まれているため、これらのプライベート VLAN とパブリック VLAN はどの組み合わせでも使用できます。
  * プライベート・ネットワーク上でのみオンプレミス・データ・センターを拡張するクラスターや、後でエッジ・ワーカー・ノードを介して限定パブリック・アクセスを追加するオプション付きでオンプレミス・データ・センターを拡張するクラスター、または、オンプレミス・データ・センターを拡張し、ゲートウェイ・デバイスを介して限定パブリック・アクセスを提供するクラスターを作成するには、プライベート VLAN が存在するかどうかを確認します。プライベート VLAN がある場合は、ID をメモします。

5. `cluster-create` コマンドを実行します。 デフォルトでは、ワーカー・ノード・ディスクは AES 256 ビット暗号化され、クラスターは使用時間に応じて請求されます。
  * インターネット向けワークロードを実行できるクラスターを作成するには、以下のようにします。
    ```
    ibmcloud ks cluster-create --zone <zone> --machine-type <machine_type> --hardware <shared_or_dedicated> --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID> --workers <number> --name <cluster_name> --kube-version <major.minor.patch> [--private-service-endpoint] [--public-service-endpoint] [--disable-disk-encrypt] [--trusted]
    ```
    {: pre}
  * 後でエッジ・ワーカー・ノードを介して限定パブリック・アクセスを追加するオプション付きで、プライベート・ネットワーク上でオンプレミス・データ・センターを拡張するクラスターを作成するには、以下のようにします。
    ```
    ibmcloud ks cluster-create --zone <zone> --machine-type <machine_type> --hardware <shared_or_dedicated> --private-vlan <private_VLAN_ID> --private-only --workers <number> --name <cluster_name> --kube-version <major.minor.patch> --private-service-endpoint [--public-service-endpoint] [--disable-disk-encrypt] [--trusted]
    ```
    {: pre}
  * オンプレミス・データ・センターを拡張し、ゲートウェイ・デバイスを介して限定パブリック・アクセスを提供するクラスターを作成するには、以下のようにします。
    ```
    ibmcloud ks cluster-create --zone <zone> --machine-type <machine_type> --hardware <shared_or_dedicated> --private-vlan <private_VLAN_ID> --private-only --workers <number> --name <cluster_name> --kube-version <major.minor.patch> --public-service-endpoint [--disable-disk-encrypt] [--trusted]
    ```
    {: pre}

    <table>
    <caption>cluster-create コンポーネント</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> このコマンドの構成要素について</th>
    </thead>
    <tbody>
    <tr>
    <td><code>cluster-create</code></td>
    <td>{{site.data.keyword.Bluemix_notm}} 組織内にクラスターを作成するためのコマンド。</td>
    </tr>
    <tr>
    <td><code>--zone <em>&lt;zone&gt;</em></code></td>
    <td>ステップ 4 で選択したクラスターを作成する {{site.data.keyword.Bluemix_notm}} ゾーン ID を指定します。</td>
    </tr>
    <tr>
    <td><code>--machine-type <em>&lt;machine_type&gt;</em></code></td>
    <td>ステップ 5 で選択したマシン・タイプを指定します。</td>
    </tr>
    <tr>
    <td><code>--hardware <em>&lt;shared_or_dedicated&gt;</em></code></td>
    <td>ワーカー・ノードのハードウェア分離のレベルを使用して指定します。使用可能な物理リソースを自分専用にする場合は dedicated を使用し、IBM の他のお客様と物理リソースを共有することを許可する場合は shared を使用します。 デフォルトは shared です。 この値は、VM 標準クラスターの場合にはオプションです。ベアメタル・マシン・タイプの場合、`dedicated` を指定します。</td>
    </tr>
    <tr>
    <td><code>--public-vlan <em>&lt;public_vlan_id&gt;</em></code></td>
    <td>IBM Cloud インフラストラクチャー (SoftLayer) アカウントでそのゾーン用にパブリック VLAN を既にセットアップしている場合には、ステップ 4 で見つけたパブリック VLAN の ID を入力します。<p>必ず、プライベート VLAN ルーターの先頭は <code>bcr</code> (バックエンド・ルーター)、パブリック VLAN ルーターの先頭は <code>fcr</code> (フロントエンド・ルーター) になります。 クラスターを作成し、パブリック VLAN とプライベート VLAN を指定するときには、それらの接頭部の後の番号と文字の組み合わせが一致する必要があります。</p></td>
    </tr>
    <tr>
    <td><code>--private-vlan <em>&lt;private_vlan_id&gt;</em></code></td>
    <td>IBM Cloud インフラストラクチャー (SoftLayer) アカウントでそのゾーン用にプライベート VLAN を既にセットアップしている場合には、ステップ 4 で見つけたプライベート VLAN の ID を入力します。アカウントにプライベート VLAN がない場合は、このオプションを指定しないでください。{{site.data.keyword.containerlong_notm}} が自動的にプライベート VLAN を作成します。<p>必ず、プライベート VLAN ルーターの先頭は <code>bcr</code> (バックエンド・ルーター)、パブリック VLAN ルーターの先頭は <code>fcr</code> (フロントエンド・ルーター) になります。 クラスターを作成し、パブリック VLAN とプライベート VLAN を指定するときには、それらの接頭部の後の番号と文字の組み合わせが一致する必要があります。</p></td>
    </tr>
    <tr>
    <td><code>--private-only </code></td>
    <td>プライベート VLAN のみを使用してクラスターを作成します。このオプションを含める場合は、<code>--public-vlan</code> オプションを含めないでください。</td>
    </tr>
    <tr>
    <td><code>--name <em>&lt;name&gt;</em></code></td>
    <td>クラスターの名前を指定します。名前は先頭が文字でなければならず、文字、数字、およびハイフン (-) を使用できます。35 文字以内でなければなりません。 地域をまたいで固有の名前を使用します。Ingress サブドメインの完全修飾ドメイン・ネームは、クラスター名と、クラスターがデプロイされる地域で形成されます。 Ingress サブドメインを地域内で固有にするために、クラスター名が切り捨てられ、Ingress ドメイン・ネームにランダムな値が付加されることがあります。
</td>
    </tr>
    <tr>
    <td><code>--workers <em>&lt;number&gt;</em></code></td>
    <td>クラスターに含めるワーカー・ノードの数を指定します。<code>--workers</code> オプションが指定されていない場合は、ワーカー・ノードが 1 つ作成されます。</td>
    </tr>
    <tr>
    <td><code>--kube-version <em>&lt;major.minor.patch&gt;</em></code></td>
    <td>クラスター・マスター・ノードの Kubernetes のバージョン。 この値はオプションです。 バージョンを指定しなかった場合、クラスターは、サポートされるデフォルトの Kubernetes バージョンを使用して作成されます。 使用可能なバージョンを確認するには、<code>ibmcloud ks versions</code> を実行します。
</td>
    </tr>
    <tr>
    <td><code>--private-service-endpoint</code></td>
    <td>**[VRF 対応アカウント](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started)**: Kubernetes マスターとワーカー・ノードがプライベート VLAN を介して通信できるように、プライベート・サービス・エンドポイントを有効にします。さらに、`--public-service-endpoint` フラグを使用してパブリック・サービス・エンドポイントを有効にすることで、インターネットを介してクラスターにアクセスすることもできます。 プライベート・サービス・エンドポイントのみを有効にする場合は、Kubernetes マスターと通信するためにプライベート VLAN に接続している必要があります。 プライベート・サービス・エンドポイントを有効にしたら、後で無効にすることはできません。<br><br>クラスターを作成した後、`ibmcloud ks cluster-get --cluster <cluster_name_or_ID>` を実行してエンドポイントを取得できます。</td>
    </tr>
    <tr>
    <td><code>--public-service-endpoint</code></td>
    <td>パブリック・ネットワークを介して Kubernetes マスターにアクセスできるようにパブリック・サービス・エンドポイントを有効にします。例えば、端末から `kubectl` コマンドを実行し、Kubernetes マスターとワーカー・ノードがパブリック VLAN を介して通信できるようにします。後で、プライベート専用クラスターにする場合は、パブリック・サービス・エンドポイントを無効にすることができます。<br><br>クラスターを作成した後、`ibmcloud ks cluster-get --cluster <cluster_name_or_ID>` を実行してエンドポイントを取得できます。</td>
    </tr>
    <tr>
    <td><code>--disable-disk-encrypt</code></td>
    <td>ワーカー・ノードには、デフォルトで AES 256 ビット・[ディスク暗号化](/docs/containers?topic=containers-security#encrypted_disk)の機能があります。 暗号化を無効にする場合は、このオプションを組み込みます。</td>
    </tr>
    <tr>
    <td><code>--trusted</code></td>
    <td>**ベアメタル・クラスター**: [トラステッド・コンピューティング](/docs/containers?topic=containers-security#trusted_compute)を有効にして、ベアメタル・ワーカー・ノードが改ざんされていないことを検証します。トラステッド・コンピューティングは、選ばれたベア・メタル・マシン・タイプでのみ使用できます。 例えば、`mgXc` GPU フレーバーではトラステッド・コンピューティングはサポートされません。 クラスターの作成時にトラストを有効にしなかった場合に、後で有効にするには、`ibmcloud ks feature-enable` [コマンド](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable)を使用します。 トラストを有効にした後に無効にすることはできません。</td>
    </tr>
    </tbody></table>

6. クラスターの作成が要求されたことを確認します。 仮想マシンの場合、ワーカー・ノード・マシンの注文と、アカウントへのクラスターのセットアップとプロビジョンには、数分かかります。 ベア・メタル物理マシンは、IBM Cloud インフラストラクチャー (SoftLayer) との人同士のやりとりによりプロビジョンされるので、完了するのに 1 営業日以上かかることがあります。
    ```
    ibmcloud ks clusters
    ```
    {: pre}

    クラスターのプロビジョニングが完了すると、クラスターの状況が **deployed** に変わります。
    ```
    Name         ID                                   State      Created          Workers   Zone       Version     Resource Group Name
    my_cluster   paf97e8843e29941b49c598f516de72101   deployed   20170201162433   1         mil01      1.13.6      Default
    ```
    {: screen}

7. ワーカー・ノードの状況を確認します。
    ```
    ibmcloud ks workers --cluster <cluster_name_or_ID>
    ```
    {: pre}

    ワーカー・ノードの準備が完了すると、状態が **normal** に変わり、状況が **Ready** になります。 ノードの状況が**「Ready」**になったら、クラスターにアクセスできます。 クラスターの準備ができている場合でも、Ingress シークレットやレジストリーのイメージ・プル・シークレットなどの他のサービスによって使用される、クラスターの一部がまだ処理中である可能性があることに注意してください。 プライベート VLAN のみでクラスターを作成した場合は、**パブリック IP** アドレスがワーカー・ノードに割り当てられないことに注意してください。

    ```
    ID                                                 Public IP       Private IP      Machine Type   State    Status   Zone        Version     Resource Group Name
    kube-mil01-paf97e8843e29941b49c598f516de72101-w1   169.xx.xxx.xxx  10.xxx.xx.xxx   standard       normal   Ready    mil01       1.13.6      Default
    ```
    {: screen}

    どのワーカー・ノードにも固有のワーカー・ノード ID とドメイン名が割り当てられます。それらをクラスター作成後に手動で変更してはいけません。 ID またはドメイン名を変更すると、Kubernetes マスターがクラスターを管理できなくなります。
    {: important}

8. クラスターが作成されたら、[CLI セッションを構成してクラスターの処理を開始](#access_cluster)できます。

<br />


## クラスターへのアクセス
{: #access_cluster}

クラスターが作成されたら、CLI セッションを構成してクラスターの処理を開始できます。
{: shortdesc}

### パブリック・サービス・エンドポイントを介したクラスターへのアクセス
{: #access_internet}

クラスターを処理するには、作成したクラスターを CLI セッションのコンテキストとして設定して、`kubectl` コマンドを実行します。
{: shortdesc}

1. ネットワークが会社のファイアウォールによって保護されている場合は、{{site.data.keyword.Bluemix_notm}} および {{site.data.keyword.containerlong_notm}} API エンドポイントとポートへのアクセスを許可します。
  1. [ファイアウォール内の `ibmcloud` API および `ibmcloud ks` API のパブリック・エンドポイントへのアクセスを許可します](/docs/containers?topic=containers-firewall#firewall_bx)。
  2. [許可されたクラスター・ユーザーに対して、`kubectl` コマンドの実行を許可し](/docs/containers?topic=containers-firewall#firewall_kubectl)、パブリック専用、プライベート専用、またはパブリックとプライベートのサービス・エンドポイントを介してマスターにアクセスできるようにします。
  3. [許可されたクラスター・ユーザーに対して、`calicotl` コマンドの実行を許可し](/docs/containers?topic=containers-firewall#firewall_calicoctl)、クラスター内の Calico ネットワーク・ポリシーを管理できるようにします。

2. 作成したクラスターを、このセッションのコンテキストとして設定します。 次の構成手順は、クラスターの操作時に毎回行ってください。

  {{site.data.keyword.Bluemix_notm}} コンソールを代わりに使用する場合は、[Kubernetes Terminal](/docs/containers?topic=containers-cs_cli_install#cli_web) の Web ブラウザーから直接 CLI コマンドを実行できます。
  {: tip}
  1. 環境変数を設定して Kubernetes 構成ファイルをダウンロードするためのコマンドを取得します。
      ```
      ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
      ```
      {: pre}
      構成ファイルのダウンロードが完了すると、そのローカルの Kubernetes 構成ファイルのパスを環境変数として設定するために使用できるコマンドが表示されます。

      OS X の場合の例:
      ```
      export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/mycluster/kube-config-prod-dal10-mycluster.yml
      ```
      {: screen}
  2. `KUBECONFIG` 環境変数を設定するためのコマンドとしてターミナルに表示されたものを、コピーして貼り付けます。
  3. `KUBECONFIG` 環境変数が適切に設定されたことを確認します。
      OS X の場合の例:
      ```
      echo $KUBECONFIG
      ```
      {: pre}

      出力:
      ```
      /Users/<user_name>/.bluemix/plugins/container-service/clusters/mycluster/kube-config-prod-dal10-mycluster.yml
      ```
      {: screen}

3. デフォルトのポート `8001` で Kubernetes ダッシュボードを起動します。
  1. デフォルトのポート番号でプロキシーを設定します。
      ```
      kubectl proxy
      ```
      {: pre}

      ```
      Starting to serve on 127.0.0.1:8001
      ```
      {: screen}

  2.  Web ブラウザーで以下の URL を開いて、Kubernetes ダッシュボードを表示します。
      ```
      http://localhost:8001/ui
      ```
      {: codeblock}

### プライベート・サービス・エンドポイントを介したクラスターへのアクセス
{: #access_on_prem}

許可されたクラスター・ユーザーは、{{site.data.keyword.Bluemix_notm}} プライベート・ネットワークの中で作業している場合、または [VPN 接続](/docs/infrastructure/iaas-vpn?topic=VPN-gettingstarted-with-virtual-private-networking)や [{{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link) 経由でプライベート・ネットワークに接続している場合に、プライベート・サービス・エンドポイントを介して、Kubernetes マスターにアクセスできます。 ただし、プライベート・サービス・エンドポイントを介した Kubernetes マスターとの通信は、<code>166.X.X.X</code> IP アドレス範囲を経由する必要があり、これは VPN 接続から、および {{site.data.keyword.Bluemix_notm}} Direct Link を介してルーティング可能ではありません。プライベート・ネットワーク・ロード・バランサー (NLB) を使用して、クラスター・ユーザーのマスターのプライベート・サービス・エンドポイントを公開できます。プライベート NLB は、ユーザーが VPN または {{site.data.keyword.Bluemix_notm}} Direct Link 接続を使用してアクセスできる内部 <code>10.X.X.X</code> IP アドレス範囲として、マスターのプライベート・サービス・エンドポイントを公開します。プライベート・サービス・エンドポイントのみを有効にする場合は、Kubernetes ダッシュボードを使用するか、パブリック・サービス・エンドポイントを一時的に有効にして、プライベート NLB を作成できます。
{: shortdesc}

1. ネットワークが会社のファイアウォールによって保護されている場合は、{{site.data.keyword.Bluemix_notm}} および {{site.data.keyword.containerlong_notm}} API エンドポイントとポートへのアクセスを許可します。
  1. [ファイアウォール内の `ibmcloud` API および `ibmcloud ks` API のパブリック・エンドポイントへのアクセスを許可します](/docs/containers?topic=containers-firewall#firewall_bx)。
  2. [許可されたクラスター・ユーザーに対して、`kubectl` コマンドの実行を許可します](/docs/containers?topic=containers-firewall#firewall_kubectl)。プライベート NLB を使用してマスターのプライベート・サービス・エンドポイントをクラスターに公開するまで、ステップ 6 でクラスターへの接続をテストすることはできません。

2. クラスターのプライベート・サービス・エンドポイント URL およびポートを取得します。
  ```
  ibmcloud ks cluster-get --cluster <cluster_name_or_ID>
  ```
  {: pre}

  この出力例では、**プライベート・サービス・エンドポイント URL** は `https://c1.private.us-east.containers.cloud.ibm.com:25073` です。
  ```
  Name:                           setest
  ID:                             b8dcc56743394fd19c9f3db7b990e5e3
  State:                          normal
  Created:                        2019-04-25T16:03:34+0000
  Location:                       wdc04
  Master URL:                     https://c1.private.us-east.containers.cloud.ibm.com:25073
  Public Service Endpoint URL:    -
  Private Service Endpoint URL:   https://c1.private.us-east.containers.cloud.ibm.com:25073
  Master Location:                Washington D.C.
  ...
  ```
  {: screen}

3. `kube-api-via-nlb.yaml` という名前の YAML ファイルを作成します。この YAML はプライベート `LoadBalancer` サービスを作成し、NLB を介してプライベート・サービス・エンドポイントを公開します。`<private_service_endpoint_port>` を前のステップで取得したポートで置き換えます。
  ```
  apiVersion: v1
  kind: Service
  metadata:
    name: kube-api-via-nlb
    annotations:
      service.kubernetes.io/ibm-load-balancer-cloud-provider-ip-type: private
    namespace: default
  spec:
    type: LoadBalancer
    ports:
    - protocol: TCP
      port: <private_service_endpoint_port>
      targetPort: <private_service_endpoint_port>
  ---
  kind: Endpoints
  apiVersion: v1
  metadata:
    name: kube-api-via-nlb
  subsets:
    - addresses:
        - ip: 172.20.0.1
      ports:
        - port: 2040
  ```
  {: codeblock}

4. プライベート NLB を作成するには、クラスター・マスターに接続する必要があります。VPN または {{site.data.keyword.Bluemix_notm}} Direct Link からプライベート・サービス・エンドポイントを介して接続できないため、クラスター・マスターに接続し、パブリック・サービス・エンドポイントまたは Kubernetes ダッシュボードを使用して NLB を作成する必要があります。
  * プライベート・サービス・エンドポイントのみを有効にした場合は、Kubernetes ダッシュボードを使用して NLB を作成できます。ダッシュボードは、すべての要求をマスターのプライベート・サービス・エンドポイントに自動的に経路指定します。
    1.  [{{site.data.keyword.Bluemix_notm}} コンソール](https://cloud.ibm.com/) にログインします。
    2.  メニュー・バーから、使用するアカウントを選択します。
    3.  メニュー ![メニュー・アイコン](../icons/icon_hamburger.svg "メニュー・アイコン") から、**「Kubernetes」**をクリックします。
    4.  **「クラスター」**ページで、アクセスするクラスターをクリックします。
    5.  クラスターの詳細ページで、**「Kubernetes ダッシュボード (Kubernetes Dashboard)」**をクリックします。
    6.  **「+ 作成」**をクリックします。
    7.  **「ファイルから作成 (Create from file)」**を選択し、`kube-api-via-nlb.yaml` ファイルをアップロードして、**「アップロード」**をクリックします。
    8.  **「概要」**ページで、`kube-api-via-nlb` サービスが作成されていることを確認します。**「外部エンドポイント (External endpoints)」**列で、`10.x.x.x` アドレスをメモします。この IP アドレスは、YAML ファイルで指定したポートで Kubernetes マスターのプライベート・サービス・エンドポイントを公開します。

  * パブリック・サービス・エンドポイントも有効にした場合は、既にマスターにアクセスできます。
    1. NLB およびエンドポイントを作成します。
      ```
      kubectl apply -f kube-api-via-nlb.yaml
      ```
      {: pre}
    2. `kube-api-via-nlb` NLB が作成されていることを確認します。出力の `10.x.x.x` **EXTERNAL-IP** アドレスをメモします。この IP アドレスは、YAML ファイルで指定したポートで Kubernetes マスターのプライベート・サービス・エンドポイントを公開します。
      ```
      kubectl get svc -o wide
      ```
      {: pre}

      この出力例では、Kubernetes マスターのプライベート・サービス・エンドポイントの IP アドレスは `10.186.92.42` です。
      ```
      NAME                     TYPE           CLUSTER-IP       EXTERNAL-IP      PORT(S)          AGE   SELECTOR
      kube-api-via-nlb         LoadBalancer   172.21.150.118   10.186.92.42     443:32235/TCP    10m   <none>
      ...
      ```
      {: screen}

  <p class="note">[strongSwan VPN サービス](/docs/containers?topic=containers-vpn#vpn-setup)を使用してマスターに接続する場合は、代わりに次のステップで `172.21.x.x` **クラスター IP** を使用します。strongSwan VPN ポッドはクラスター内で実行されるため、内部クラスター IP サービスの IP アドレスを使用して NLB にアクセスできます。strongSwan Helm チャートの `config.yaml` ファイルで、`local.subnet` 設定に Kubernetes サービス・サブネット CIDR `172.21.0.0/16` がリストされていることを確認します。</p>

5. 自分自身またはユーザーが `kubectl` コマンドを実行するクライアント・マシンで、NLB IP アドレスとプライベート・サービス・エンドポイント URL を `/etc/hosts` ファイルに追加します。IP アドレスおよび URL にポートを含めないでください。また、URL に `https://` を含めないでください。
  * OSX ユーザーおよび Linux ユーザーの場合:
    ```
    sudo nano /etc/hosts
    ```
    {: pre}
  * Windows ユーザーの場合:
    ```
    notepad C:\Windows\System32\drivers\etc\hosts
    ```
    {: pre}

    ローカル・マシンの権限によっては、ホスト・ファイルを編集するためにノートパッドを管理者として実行することが必要になる場合があります。
    {: tip}

  追加するテキストの例:
  ```
  10.186.92.42      c1.private.us-east.containers.cloud.ibm.com
  ```
  {: codeblock}

6. VPN または {{site.data.keyword.Bluemix_notm}} Direct Link 接続を介してプライベート・ネットワークに接続していることを確認します。

7. Kubernetes CLI サーバーのバージョンを調べて、プライベート・サービス・エンドポイントを介して、ご使用のクラスターで `kubectl` コマンドが正常に実行することを確認します。
  ```
  kubectl version  --short
  ```
  {: pre}

  出力例:
  ```
  Client Version: v1.13.6
    Server Version: v1.13.6
  ```
  {: screen}

<br />


## 次のステップ
{: #next_steps}

クラスターが稼働状態になったら、以下の作業について検討できます。
- 複数ゾーン対応ゾーンにクラスターを作成した場合は、[ゾーンをクラスターに追加して、ワーカー・ノードを分散させます](/docs/containers?topic=containers-add_workers)。
- [クラスターにアプリをデプロイします。](/docs/containers?topic=containers-app#app_cli)
- [独自のプライベート・レジストリーを {{site.data.keyword.Bluemix_notm}} でセットアップし、Docker イメージを保管して他のユーザーと共有します。](/docs/services/Registry?topic=registry-getting-started)
- ワークロード・リソース要求に基づいてワーカー・プールにワーカー・ノードを自動的に追加または削除するように[クラスター自動スケーリング機能をセットアップ](/docs/containers?topic=containers-ca#ca)します。
- [ポッドのセキュリティー・ポリシー](/docs/containers?topic=containers-psp)を使用して、クラスター内にポッドを作成できる人を制御します。
- [Istio](/docs/containers?topic=containers-istio) 管理対象アドオンおよび [Knative](/docs/containers?topic=containers-serverless-apps-knative) 管理対象アドオンを有効にして、クラスター機能を拡張します。

その後、クラスターのセットアップに対する以下のネットワーク構成ステップを確認できます。

### クラスター内のインターネット向けアプリ・ワークロードの実行
{: #next_steps_internet}

* ネットワーク・ワークロードを[エッジ・ワーカー・ノード](/docs/containers?topic=containers-edge)に分離する。
* [パブリック・ネットワーク・サービス](/docs/containers?topic=containers-cs_network_planning#public_access)でアプリを公開します。
* ホワイトリスト・ポリシーやブラックリスト・ポリシーなどの [Calico pre-DNAT ポリシー](/docs/containers?topic=containers-network_policies#block_ingress)を作成して、アプリを公開するネットワーク・サービスへのパブリック・トラフィックを制御します。
* [strongSwan IPSec VPN サービス](/docs/containers?topic=containers-vpn)をセットアップして、{{site.data.keyword.Bluemix_notm}} アカウント外のプライベート・ネットワーク内のサービスにクラスターを接続します。

### オンプレミス・データ・センターをクラスターに拡張し、エッジ・ノードおよび Calico ネットワーク・ポリシーを使用した限定パブリック・アクセスを許可する
{: #next_steps_calico}

* [{{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link) または [strongSwan IPSec VPN サービス](/docs/containers?topic=containers-vpn#vpn-setup)をセットアップして、{{site.data.keyword.Bluemix_notm}} アカウント外のプライベート・ネットワーク内のサービスにクラスターを接続します。{{site.data.keyword.Bluemix_notm}} Direct Link によって、プライベート・ネットワークを介したクラスターおよびオンプレミス・ネットワークでのアプリとサービスの通信が可能になり、strongSwan によって、パブリック・ネットワークでの暗号化された VPN トンネルを介した通信が可能になります。
* パブリック VLAN およびプライベート VLAN に接続されたワーカー・ノードの[エッジ・ワーカー・プール](/docs/containers?topic=containers-edge)を作成して、パブリック・ネットワーキング・ワークロードを分離します。
* [プライベート・ネットワーク・サービス](/docs/containers?topic=containers-cs_network_planning#private_access)でアプリを公開します。
* [Calico ホスト・ネットワーク・ポリシーを作成](/docs/containers?topic=containers-network_policies#isolate_workers)して、ポッドへのパブリック・アクセスをブロックし、プライベート・ネットワーク上のクラスターを分離し、他の {{site.data.keyword.Bluemix_notm}} サービスへのアクセスを許可します。

### オンプレミス・データ・センターをクラスターに拡張し、ゲートウェイ・デバイスを使用した限定パブリック・アクセスを許可する
{: #next_steps_gateway}

* プライベート・ネットワークのゲートウェイ・ファイアウォールも構成する場合、[ワーカー・ノード間の通信を許可し、クラスターがプライベート・ネットワークを介してインフラストラクチャー・リソースにアクセスできるようにする](/docs/containers?topic=containers-firewall#firewall_private)必要があります。
* ワーク・ノードおよびアプリを {{site.data.keyword.Bluemix_notm}} アカウント外のプライベート・ネットワークに安全に接続するには、ゲートウェイ・デバイスに IPSec VPN エンドポイントをセットアップします。次に、クラスターで [strongSwan IPSec VPN サービス構成](/docs/containers?topic=containers-vpn#vpn-setup)して、ゲートウェイで VPN エンドポイントを使用するか、[VRA を使用して直接 VPN 接続をセットアップ](/docs/containers?topic=containers-vpn#vyatta)します。
* [プライベート・ネットワーク・サービス](/docs/containers?topic=containers-cs_network_planning#private_access)でアプリを公開します。
* これらのネットワーク・サービスへのインバウンド・トラフィックを許可するために、ゲートウェイ・デバイス・ファイアウォールで、[必要なポートおよび IP アドレスを開きます](/docs/containers?topic=containers-firewall#firewall_inbound)。

### オンプレミス・データ・センターをプライベート・ネットワーク上のクラスターのみに拡張する
{: #next_steps_extend}

* プライベート・ネットワーク上のファイアウォールがある場合、ワーカー・ノード間の通信を許可し、[クラスターがプライベート・ネットワークを介してインフラストラクチャー・リソースにアクセスできるようにします](/docs/containers?topic=containers-firewall#firewall_private)。
* [{{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link) をセットアップして、{{site.data.keyword.Bluemix_notm}} アカウント外のプライベート・ネットワーク内のサービスにクラスターを接続します。
* [プライベート・ネットワーク・サービス](/docs/containers?topic=containers-cs_network_planning#private_access)を使用して、プライベート・ネットワーク上でアプリを公開します。
