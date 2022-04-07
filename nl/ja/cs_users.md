---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: kubernetes, iks

subcollection: containers

---

{:new_window: target="blank"}
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



# クラスター・アクセス権限の割り当て
{: #users}

クラスター管理者は、{{site.data.keyword.containerlong}} クラスターのアクセス・ポリシーを定義して、さまざまなユーザーに対してさまざまなレベルのアクセス権限を作成することができます。 例えば、特定のユーザーにはクラスター・インフラストラクチャー・リソースを処理する権限を与え、他のユーザーにはコンテナーをデプロイする権限だけを与えることができます。
{: shortdesc}

2019 年 1 月 30 日以降、{{site.data.keyword.containerlong_notm}} では、{{site.data.keyword.Bluemix_notm}} IAM の[サービス・アクセス役割](/docs/containers?topic=containers-access_reference#service)を使用する新しい方法でユーザーを許可できるようになりました。 これらのサービス役割は、Kubernetes 名前空間など、クラスター内のリソースにアクセス権限を付与する場合に使用します。 詳しくは、ブログ [Introducing service roles and namespaces in IAM for more granular control of cluster access ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/blogs/bluemix/2019/02/introducing-service-roles-and-namespaces-in-iam-for-more-granular-control-of-cluster-access/) を参照してください。
{: note}

## アクセス・ポリシーと役割について
{: #access_policies}

アクセス・ポリシーによって、{{site.data.keyword.Bluemix_notm}} アカウントのユーザーが {{site.data.keyword.Bluemix_notm}} プラットフォーム全体のリソースに対して持つアクセス・レベルが決定されます。 ポリシーによって、単一のサービス、またはまとめてリソース・グループに編成される一連のサービスとリソースへのアクセス権限の有効範囲を定義する 1 つ以上の役割がユーザーに割り当てられます。 {{site.data.keyword.Bluemix_notm}} のサービスごとに、アクセス・ポリシーの独自のセットが必要な場合があります。
{: shortdesc}

ユーザー・アクセスを管理する計画を策定する際には、以下の一般的な手順を検討してください。
1.  [ユーザーに対する適切なアクセス・ポリシーと役割の選出](#access_roles)
2.  [{{site.data.keyword.Bluemix_notm}} IAM での個別ユーザーまたはユーザー・グループへのアクセス役割の割り当て](#iam_individuals_groups)
3.  [クラスター・インスタンス、Kubernetes 名前空間、またはリソース・グループへのユーザー・アクセスの有効範囲設定](#resource_groups)

アカウントでの役割、ユーザー、およびリソースを管理する方法を理解した後、[クラスターへのアクセス権限のセットアップ](#access-checklist)で、アクセス権限の構成方法のチェックリストを確認します。

### ユーザーに対する適切なアクセス・ポリシーと役割の選出
{: #access_roles}

{{site.data.keyword.containerlong_notm}} を使用するすべてのユーザー用にアクセス・ポリシーを定義する必要があります。 アクセス・ポリシーの有効範囲はユーザーの定義済み役割に基づいており、これにより、ユーザーが実行できるアクションが決定されます。 一部のポリシーは事前定義されていますが、他はカスタマイズ可能です。 ユーザーが要求の実行に {{site.data.keyword.containerlong_notm}} コンソールを使用しようと CLI を使用しようと、また、IBM Cloud インフラストラクチャー (SoftLayer) で操作を実行しようと、同じポリシーが適用されます。
{: shortdesc}

以下の図は、さまざまなタイプの許可と役割、各役割で実行できる操作の種類、役割どうしの関係を示しています。

<img src="images/user_access.png" alt="{{site.data.keyword.containerlong_notm}} では、IAM プラットフォーム役割、IAM サービス役割、Cloud Foundry 役割、およびインフラストラクチャー役割を割り当てることができます。" style="border-style: none"/>

各役割の具体的な {{site.data.keyword.containerlong_notm}} の許可を参照するには、リファレンス・トピックの[ユーザー・アクセス許可](/docs/containers?topic=containers-access_reference)を確認してください。
{: tip}

<dl>
<dt><a href="#platform">{{site.data.keyword.Bluemix_notm}} IAM プラットフォーム役割とサービス役割</a></dt>
<dd>{{site.data.keyword.containerlong_notm}} では、{{site.data.keyword.Bluemix_notm}} の ID/アクセス管理 (IAM) のプラットフォーム役割とサービス役割を使用して、ユーザーにクラスターへのアクセスを許可します。
<ul><li>**プラットフォーム**: プラットフォーム役割は、{{site.data.keyword.containerlong_notm}} API、コンソール、および CLI (`ibmcloud ks`) を使用してクラスター・インフラストラクチャーに対してユーザーが実行できる操作を決定するものです。 プラットフォーム役割では、Kubernetes API へのアクセスは許可されません。 これらの役割のポリシーは、リソース・グループ、地域、またはクラスター・インスタンスごとに設定できます。 プラットフォーム役割は、クラスターに対するインフラストラクチャー操作の実行を許可しますが、IBM Cloud インフラストラクチャー (SoftLayer) のリソースに対するアクセスは許可しません。 IBM Cloud インフラストラクチャー (SoftLayer) リソースへのアクセス権限は、[地域に対して設定される API キー](#api_key)によって決まります。 プラットフォーム役割によって許可される操作には、例えば、クラスターの作成/削除、クラスターへのサービスのバインディング、ネットワーキング・リソースやストレージ・リソースの管理、ワーカー・ノードの追加などがあります。<br><br>プラットフォーム役割しか割り当てられていないユーザーは、クラスター内の Kubernetes リソースを操作できません。 しかし、その状態でも `ibmcloud ks cluster-config` [コマンド](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_config)は実行できます。 そのようなユーザーに、[カスタム RBAC ポリシー](/docs/containers?topic=containers-users#role-binding)を使用して、特定の Kubernetes 操作の実行を許可することができます。 例えば、Kubernetes のアクセス制御にカスタム RBAC ポリシーを現在使用していて、今後もサービス役割ではなくカスタム RBAC を使用し続ける予定の組織では、このような方法を取ると良いでしょう。</li>
<li>**サービス**: サービス役割は、対応する Kubernetes RBAC ポリシーを付与するものです。Kubernetes RBAC ポリシーは、クラスター内でユーザーに与えられます。 つまり、サービス役割は、Kubernetes API、ダッシュボード、および CLI (`kubectl`) へのアクセスを許可します。 サービス役割のポリシーは、リソース・グループ、地域、またはクラスター・インスタンスごとに設定できます。 さらに、サービス役割の対象を、すべてのクラスター、個々のクラスター、または地域全体のクラスターの Kubernetes 名前空間にすることもできます。 サービス役割の対象を名前空間にする場合は、同時にそのポリシーをリソース・グループに適用したり、プラットフォーム役割を割り当てたりすることはできません。 サービス役割によって許可される操作には、例えば、アプリ・デプロイメントの作成、名前空間の追加、構成マップのセットアップがあります。<br><br>サービス役割しか割り当てられていないユーザーは、{{site.data.keyword.containerlong_notm}} リソースを表示することも、操作することもできません。 クラスターにアクセスしてクラスターの Kubernetes リソースを使用するユーザーには、`ibmcloud ks cluster-config` [コマンド](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_config)を実行して [CLI から Kubernetes ダッシュボードを起動](/docs/containers?topic=containers-app#db_cli)できるように、クラスター名と ID を伝える必要があります。 さらにそのユーザーが {{site.data.keyword.containerlong_notm}} クラスター・コンソールにもアクセスして、CLI からクラスターやその他のインフラストラクチャー・リソースを一覧表示できるようにするには、ユーザーに**ビューアー**のプラットフォーム役割を与えてください。</li></ul></dd>
<dt><a href="#role-binding">RBAC</a></dt>
<dd>Kubernetes では、役割ベース・アクセス制御 (RBAC) は、クラスター内部のリソースを保護する 1 つの方法です。 RBAC 役割によって、リソースでユーザーが実行できる Kubernetes アクションが決まります。 サービス役割が割り当てられているすべてのユーザーには、対応する RBAC クラスター役割が自動的に割り当てられます。 この RBAC クラスター役割は、ポリシーの対象を名前空間にしたかどうかによって、特定の名前空間またはすべての名前空間に適用されます。</br></br>
RBAC 役割によって許可されるアクションの例として、ポッドなどのオブジェクトの作成やポッド・ログの読み取りがあります。</dd>
<dt><a href="#api_key">インフラストラクチャー</a></dt>
<dd>インフラストラクチャー役割では、IBM Cloud インフラストラクチャー (SoftLayer) リソースへのアクセスが可能になります。 インフラストラクチャー役割**スーパーユーザー**を使用してユーザーをセットアップし、このユーザーのインフラストラクチャー資格情報を API キーに保管します。 次に、クラスターを作成する各地域に API キーを設定します。 API キーをセットアップした後は、地域内のすべてのユーザー間で API キーが共有されるため、{{site.data.keyword.containerlong_notm}} へのアクセス権限が付与された他のユーザーにはインフラストラクチャー役割は必要ありません。 代わりに、{{site.data.keyword.Bluemix_notm}} IAM プラットフォーム役割によって、ユーザーが実行を許可されるインフラストラクチャー・アクションが決まります。 完全な<strong>スーパーユーザー</strong>のインフラストラクチャーを使用して API キーをセットアップしない場合や、特定のデバイス・アクセス権限をユーザーに付与する必要がある場合は、[インフラストラクチャー許可をカスタマイズ](#infra_access)できます。 </br></br>
インフラストラクチャー役割によって許可されるアクションの例として、クラスター・ワーカー・ノード・マシンの詳細表示や、ネットワーキング・リソースおよびストレージ・リソースの編集などがあります。</dd>
<dt>Cloud Foundry</dt>
<dd>すべてのサービスが {{site.data.keyword.Bluemix_notm}} IAM で管理されているわけではありません。 このようなサービスの 1 つを使用している場合は、引き続き Cloud Foundry ユーザー役割を使用してサービスへのアクセス権限を管理できます。 Cloud Foundry の役割は、このアカウントに属する組織およびスペースにアクセス権限を付与します。 {{site.data.keyword.Bluemix_notm}} の Cloud Foundry ベースのサービスのリストを表示するには、<code>ibmcloud service list</code> を実行します。</br></br>
Cloud Foundry 役割によって許可されるアクションの例として、新規 Cloud Foundry サービス・インスタンスの作成や、クラスターへの Cloud Foundry サービス・インスタンスのバインディングなどがあります。 詳しくは、{{site.data.keyword.Bluemix_notm}} IAM 資料内の、使用可能な[組織の役割とスペースの役割](/docs/iam?topic=iam-cfaccess)、または [Cloud Foundry のアクセス権限の管理](/docs/iam?topic=iam-mngcf)の手順を参照してください。</dd>
</dl>

### {{site.data.keyword.Bluemix_notm}} IAM での個別ユーザーまたはユーザー・グループへのアクセス役割の割り当て
{: #iam_individuals_groups}

{{site.data.keyword.Bluemix_notm}} IAM ポリシーを設定する際に、個別ユーザーまたはユーザー・グループに役割を割り当てることができます。
{: shortdesc}

<dl>
<dt>個別ユーザー</dt>
<dd>チームの他のユーザーとは異なる許可を必要とする特定のユーザーがいる場合があります。 各人がそれぞれのタスクを完了するために必要な許可を持つように、個人レベルで許可をカスタマイズできます。 ユーザーごとに複数の {{site.data.keyword.Bluemix_notm}} IAM 役割を割り当てることができます。</dd>
<dt>アクセス・グループ内の複数のユーザー</dt>
<dd>ユーザーのグループを作成して、そのグループに許可を割り当てることができます。 例えば、すべてのチーム・リーダーをグループ化して、そのグループに管理者権限を割り当てることができます。 次に、すべての開発者をグループ化して、そのグループに書き込み権限のみを割り当てることができます。 アクセス・グループごとに複数の {{site.data.keyword.Bluemix_notm}} IAM 役割を割り当てることができます。 グループに許可を割り当てると、そのグループに追加される、またはそのグループから削除されるすべてのユーザーは影響を受けます。 グループにユーザーを追加した場合、ユーザーにはアクセス権限が追加されます。 ユーザーを削除した場合、アクセス権限は取り消されます。</dd>
</dl>

{{site.data.keyword.Bluemix_notm}} IAM 役割をサービス・アカウントに割り当てることはできません。 代わりに、直接 [RBAC 役割をサービス・アカウントに割り当てる](#rbac)ことができます。
{: tip}

また、ユーザーに付与するアクセス権限が、1 つのリソース・グループの 1 つのクラスター、1 つのリソース・グループのすべてのクラスター、またはアカウントのすべてのリソース・グループのすべてのクラスターのいずれに対するものであるかを指定する必要があります。

### クラスター・インスタンス、名前空間、またはリソース・グループに対するユーザー・アクセスの設定
{: #resource_groups}

{{site.data.keyword.Bluemix_notm}} IAM では、ユーザーのアクセス役割をリソース・インスタンス、Kubernetes 名前空間、またはリソース・グループに割り当てることができます。
{: shortdesc}

{{site.data.keyword.Bluemix_notm}} アカウントを作成すると、デフォルトのリソース・グループが自動的に作成されます。 リソースの作成時にリソース・グループを指定しない場合、リソース・インスタンス (クラスター) はデフォルトのリソース・グループに属します。 {{site.data.keyword.Bluemix_notm}} IAM では、Kubernetes 名前空間のリソース・タイプは、リソース・インスタンス (クラスター) です。 アカウントにリソース・グループを追加する場合は、[アカウントをセットアップするためのベスト・プラクティス](/docs/account?topic=account-account_setup)および[リソース・グループのセットアップ](/docs/resources?topic=resources-bp_resourcegroups#setuprgs)を参照してください。

<dl>
<dt>リソース・インスタンス</dt>
  <dd><p>アカウントの各 {{site.data.keyword.Bluemix_notm}} サービスは、インスタンスを持つリソースです。 インスタンスはサービスによって異なります。 例えば、{{site.data.keyword.containerlong_notm}} では、インスタンスはクラスターですが、{{site.data.keyword.cloudcerts_long_notm}} では、インスタンスは証明書です。 デフォルトでは、リソースもアカウントのデフォルトのリソース・グループに属します。 以下のシナリオの場合、リソース・インスタンスに対するアクセス役割をユーザーに割り当てることができます。
  <ul><li>アカウントのすべての {{site.data.keyword.Bluemix_notm}} IAM サービス ({{site.data.keyword.containerlong_notm}} のすべてのクラスターおよび {{site.data.keyword.registrylong_notm}} のすべてのイメージなど)。</li>
  <li>サービス内のすべてのインスタンス ({{site.data.keyword.containerlong_notm}} のすべてのクラスターなど)。</li>
  <li>サービスの地域内のすべてのインスタンス ({{site.data.keyword.containerlong_notm}} の**米国南部**地域のすべてのクラスターなど)。</li>
  <li>個別インスタンス (1 つのクラスターなど)。</li></ul></dd>
<dt>Kubernetes 名前空間</dt>
  <dd><p>{{site.data.keyword.Bluemix_notm}} IAM のクラスター・リソース・インスタンスの一部として、クラスター内の Kubernetes 名前空間に対するサービス・アクセス役割をユーザーに割り当てることができます。</p>
  <p>名前空間に対するアクセス権限を割り当てると、許可したすべてのクラスターにおいて、その名前空間の現在および将来のすべてのインスタンスにポリシーが適用されます。 例えば、`dev` グループのユーザーが、アジア太平洋北部地域のすべてのクラスターの `test` 名前空間に Kubernetes リソースをデプロイできるようにしたいとします。 `dev` アクセス・グループに、`default` リソース・グループ内のアジア太平洋北部地域のすべてのクラスター内の Kubernetes 名前空間 test に対する**ライター**のサービス・アクセス役割を割り当てれば、その `dev` グループは、 `default` リソース・グループ内のアジア太平洋北部地域のすべてのクラスターの現在および将来の `test` 名前空間にアクセスできるようになります。</p>
  <p class="important">サービス役割の対象を名前空間にする場合は、同時にそのポリシーをリソース・グループに適用したり、プラットフォーム役割を割り当てたりすることはできません。</p></dd>
<dt>リソース・グループ</dt>
  <dd><p>アカウント・リソースはカスタマイズ可能なグループに編成することができるため、個別ユーザーまたはユーザー・グループに複数のリソースへのアクセス権限を一度に素早く割り当てることができます。 リソース・グループは、オペレーターや管理者がリソースの現在の使用量を表示したり、問題をトラブルシューティングしたり、チームを管理したりするために、リソースをフィルターに掛ける場合に役立ちます。</p>
  <p class="important">クラスターは 1 つのリソース・グループにしか作成できず、作成後にリソース・グループを変更することはできません。 誤ったリソース・グループにクラスターを作成した場合、クラスターを削除してから、正しいリソース・グループにクラスターを再作成する必要があります。 また、`ibmcloud ks cluster-service-bind` [コマンド](/docs/containers-cli-plugin?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_service_bind)を使用して [{{site.data.keyword.Bluemix_notm}} サービスと統合](/docs/containers?topic=containers-service-binding#bind-services)する必要がある場合は、このサービスがクラスターと同じリソース・グループにある必要があります。 {{site.data.keyword.registrylong_notm}} のようにリソース・グループを使用しないサービスや、{{site.data.keyword.la_full_notm}} のようにサービス・バインディングを必要としないサービスは、異なるリソース・グループにクラスターがあっても機能します。</p>
  <p>[メトリックに {{site.data.keyword.monitoringlong_notm}}](/docs/containers?topic=containers-health#view_metrics) を使用する計画の場合は、メトリックでの名前の競合を回避するために、アカウント内のリソース・グループおよび地域全体で固有の名前をクラスターに付けることを検討してください。 クラスターの名前を変更することはできません。</p>
  <p>以下のシナリオの場合、リソース・グループに対するアクセス役割をユーザーに割り当てることができます。 リソース・インスタンスとは異なり、リソース・グループ内の個別インスタンスへのアクセス権限を付与することはできないので注意してください。</p>
  <ul><li>リソース・グループのすべての {{site.data.keyword.Bluemix_notm}} IAM サービス ({{site.data.keyword.containerlong_notm}} のすべてのクラスターおよび {{site.data.keyword.registrylong_notm}} のすべてのイメージなど)。</li>
  <li>リソース・グループのサービス内のすべてのインスタンス ({{site.data.keyword.containerlong_notm}} のすべてのクラスターなど)。</li>
  <li>リソース・グループのサービスの地域内のすべてのインスタンス ({{site.data.keyword.containerlong_notm}} の**米国南部**地域のすべてのクラスターなど)。</li></ul></dd>
</dl>

<br />


## クラスターへのアクセス権限のセットアップ
{: #access-checklist}

[アカウントで役割、ユーザー、およびリソースを管理する方法](#access_policies)を理解した後、以下のチェックリストを使用して、クラスターのユーザー・アクセス権限を構成します。
{: shortdesc}

<p class="tip">{{site.data.keyword.Bluemix_notm}} IAM プラットフォーム役割は、サービス役割と同時に割り当てないでください。 プラットフォーム役割とサービス役割は別々に割り当てる必要があります。</p>

1. クラスターを作成するすべての地域とリソース・グループに対して、[API キーを設定します](#api_key)。
2. アカウントにユーザーを招待して、{{site.data.keyword.containerlong_notm}} に対する [{{site.data.keyword.Bluemix_notm}} IAM 役割をユーザーに割り当てます](#platform)。
3. サービスをクラスターにバインドしたり、クラスター・ロギング構成から転送されるログを表示したりすることをユーザーに許可するため、サービスがデプロイされたりログが収集されたりする組織およびスペースに対する [Cloud Foundry 役割をユーザーに付与します](/docs/iam?topic=iam-mngcf)。
4. Kubernetes 名前空間を使用してクラスター内でリソースを分離している場合は、[ユーザーに名前空間の {{site.data.keyword.Bluemix_notm}} IAM サービス役割を割り当てる](#platform)ことで名前空間に対するアクセス権限を付与します。
5. CI/CD パイプラインにあるような自動化ツールの場合は、サービス・アカウントをセットアップし、[サービス・アカウントに Kubernetes RBAC 許可を割り当てます](#rbac)。
6. ポッド・レベルでクラスター・リソースへのアクセス権限を制御するその他の拡張構成については、[ポッド・セキュリティーの構成](/docs/containers?topic=containers-psp)を参照してください。

</br>

アカウントおよびリソースのセットアップについて詳しくは、[ユーザー、チーム、アプリケーションを編成するためのベスト・プラクティス](/docs/tutorials?topic=solution-tutorials-users-teams-applications)に関するこのチュートリアルをお試しください。
{: tip}

<br />


## API キーのセットアップによるインフラストラクチャー・ポートフォリオへのアクセスの有効化
{: #api_key}

クラスターを正常にプロビジョンして使用するには、クラスターのあるリソース・グループおよび地域ごとの IBM Cloud インフラストラクチャー (SoftLayer) ポートフォリオにアクセスできるように {{site.data.keyword.Bluemix_notm}} アカウントが正しくセットアップされていることを確認する必要があります。
{: shortdesc}

**ほとんどの場合**: {{site.data.keyword.Bluemix_notm}} 従量課金 (PAYG) アカウントには既に IBM Cloud インフラストラクチャー (SoftLayer) ポートフォリオへのアクセス権限があります。 ポートフォリオにアクセスするように {{site.data.keyword.containerlong_notm}} をセットアップするには、**アカウント所有者**が、地域とリソース・グループの API キーを設定する必要があります。

1. アカウント所有者として端末にログインします。
    ```
    ibmcloud login [--sso]
    ```
    {: pre}

2. API キーを設定するリソース・グループをターゲットに指定します。 リソース・グループをターゲットに指定しないと、API キーはデフォルトのリソース・グループに対して設定されます。 使用可能なリソース・グループをリストするには、`ibmcloud resource groups` を実行します。
    ```
    ibmcloud target -g <resource_group_name>
    ```
    {:pre}

3. 地域およびリソース・グループの API キーを設定します。
    ```
    ibmcloud ks api-key-reset --region <region>
    ```
    {: pre}    

4. API キーが設定されていることを確認してください。
    ```
    ibmcloud ks api-key-info --cluster <cluster_name_or_ID>
    ```
    {: pre}

5. クラスターを作成する地域とリソース・グループごとに繰り返します。

**代替オプションと詳細情報**: IBM Cloud インフラストラクチャー (SoftLayer) ポートフォリオにアクセスする別の方法については、以下のセクションを確認してください。
* アカウントに IBM Cloud インフラストラクチャー (SoftLayer) ポートフォリオへのアクセス権限が既にあるかどうかが不明の場合は、[IBM Cloud インフラストラクチャー (SoftLayer) ポートフォリオへのアクセスについて](#understand_infra)を参照してください。
* アカウント所有者が API キーを設定しない場合は、[API キーを設定するユーザーが正しい許可を得ていることを確認してください](#owner_permissions)。
* デフォルト・アカウントを使用した API キーの設定について詳しくは、[デフォルトの {{site.data.keyword.Bluemix_notm}} 従量課金アカウントでのインフラストラクチャー・ポートフォリオへのアクセス](#default_account)を参照してください。
* デフォルトの従量課金アカウントがない場合や、別の IBM Cloud インフラストラクチャー (SoftLayer) アカウントを使用する必要がある場合は、[別の IBM Cloud インフラストラクチャー (SoftLayer) アカウントへのアクセス](#credentials)を参照してください。

### IBM Cloud インフラストラクチャー (SoftLayer) ポートフォリオへのアクセスについて
{: #understand_infra}

アカウントに IBM Cloud インフラストラクチャー (SoftLayer) ポートフォリオへのアクセス権限があるかどうかを判別し、{{site.data.keyword.containerlong_notm}} で API キーを使用してポートフォリオにアクセスする方法について学習します。
{: shortdesc}

**アカウントに IBM Cloud インフラストラクチャー (SoftLayer) ポートフォリオへのアクセス権限が既にありますか?**</br>

IBM Cloud インフラストラクチャー (SoftLayer) ポートフォリオにアクセスするために、{{site.data.keyword.Bluemix_notm}} 従量課金アカウントを使用します。 別のタイプのアカウントがある場合は、以下の表でオプションを確認してください。

<table summary="この表は、アカウント・タイプ別の標準クラスターの作成オプションを示しています。各行は、左から右に読む必要があります。1 番目の列にアカウントの説明があり、2 番目の列に標準クラスターを作成するためのオプションがあります。">
<caption>アカウント・タイプ別の標準クラスター作成の選択肢</caption>
  <thead>
  <th>アカウントの説明</th>
  <th>標準クラスターを作成するためのオプション</th>
  </thead>
  <tbody>
    <tr>
      <td>**ライト・アカウント**ではクラスターをプロビジョンできません。</td>
      <td>[ライト・アカウントを {{site.data.keyword.Bluemix_notm}} 従量課金アカウントにアップグレードします](/docs/account?topic=account-accounts#paygo)。</td>
    </tr>
    <tr>
      <td>**従量制課金**アカウントには、インフラストラクチャー・ポートフォリオへのアクセス権限が付属しています。</td>
      <td>標準クラスターを作成できます。 API キーを使用して、クラスターのインフラストラクチャー許可をセットアップします。</td>
    </tr>
    <tr>
      <td>**サブスクリプション・アカウント**には、IBM Cloud インフラストラクチャー (SoftLayer) ポートフォリオへのアクセスがセットアップされていません。</td>
      <td><p><strong>選択肢 1:</strong> [新しい従量制課金アカウントを作成します](/docs/account?topic=account-accounts#paygo)。このアカウントには、IBM Cloud インフラストラクチャー (SoftLayer) ポートフォリオへのアクセスがセットアップされます。 この選択肢を取る場合は、2 つの異なる {{site.data.keyword.Bluemix_notm}} アカウントを所有し、2 つの異なる課金が行われることになります。</p><p>サブスクリプション・アカウントを引き続き使用する場合は、新しい従量制課金アカウントを使用して、IBM Cloud インフラストラクチャー (SoftLayer) で API キーを生成します。 次に、手動でサブスクリプション・アカウント用に IBM Cloud インフラストラクチャー (SoftLayer) API キーを設定する必要があります。 IBM Cloud インフラストラクチャー (SoftLayer) リソースは新しい従量制課金アカウントを介して課金されることに注意してください。</p><p><strong>選択肢 2:</strong> 既存の IBM Cloud インフラストラクチャー (SoftLayer) アカウントを使用する場合は、{{site.data.keyword.Bluemix_notm}} アカウントに手動で IBM Cloud インフラストラクチャー (SoftLayer) 資格情報を設定できます。</p><p class="note">IBM Cloud インフラストラクチャー (SoftLayer) アカウントに手動でリンクする場合、資格情報は {{site.data.keyword.Bluemix_notm}} アカウントでの IBM Cloud インフラストラクチャー (SoftLayer) 固有のすべてのアクションに使用されます。 ユーザーがクラスターを作成して操作できるように、設定した API キーに[十分なインフラストラクチャー許可](/docs/containers?topic=containers-users#infra_access)があることを確認する必要があります。</p></td>
    </tr>
    <tr>
      <td>**IBM Cloud インフラストラクチャー (SoftLayer) アカウント**があり、{{site.data.keyword.Bluemix_notm}} アカウントはない</td>
      <td><p>[{{site.data.keyword.Bluemix_notm}} 従量課金アカウントを作成します](/docs/account?topic=account-accounts#paygo)。 2 つの別個の IBM Cloud インフラストラクチャー (SoftLayer) アカウントを所有し、課金されることになります。</p><p>デフォルトでは、お客様の新規の {{site.data.keyword.Bluemix_notm}} アカウントでは新規のインフラストラクチャー・アカウントを使用します。 古いインフラストラクチャー・アカウントを引き続き使用するには、手動で資格情報を設定します。</p></td>
    </tr>
  </tbody>
  </table>

**インフラストラクチャー・ポートフォリオがセットアップされた後、{{site.data.keyword.containerlong_notm}} はポートフォリオにどのようにアクセスしますか?**</br>

{{site.data.keyword.containerlong_notm}} は API キーを使用して IBM Cloud インフラストラクチャー (SoftLayer) ポートフォリオにアクセスします。 API キーには、IBM Cloud インフラストラクチャー (SoftLayer) アカウントへのアクセス権限を持つユーザーの資格情報が保管されています。 API キーは、リソース・グループ内の地域ごとに設定され、その地域のユーザーによって共有されます。
 
すべてのユーザーが IBM Cloud インフラストラクチャー (SoftLayer) ポートフォリオにアクセスできるようにするには、API キーに格納された資格情報の所有者ユーザーに、[{{site.data.keyword.Bluemix_notm}} アカウントの {{site.data.keyword.containerlong_notm}} と {{site.data.keyword.registryshort_notm}} に対する**スーパーユーザー**のインフラストラクチャー役割と**管理者**のプラットフォーム役割](#owner_permissions)がある必要があります。次に、そのユーザーが地域およびリソース・グループでの最初の管理アクションを実行する必要があります。 ユーザーのインフラストラクチャー資格情報がその地域およびリソース・グループの API キーに保管されます。

アカウント内の他のユーザーは、インフラストラクチャーにアクセスするための API キーを共有します。 ユーザーが {{site.data.keyword.Bluemix_notm}} アカウントにログインすると、 API キーに基づいて CLI セッション用の {{site.data.keyword.Bluemix_notm}} IAM トークンが生成され、クラスターでインフラストラクチャー関連のコマンドを実行できるようになります。

CLI セッション用の {{site.data.keyword.Bluemix_notm}} IAM トークンを表示するには、`ibmcloud iam oauth-tokens` を実行します。 {{site.data.keyword.Bluemix_notm}} IAM トークンを使用して、[{{site.data.keyword.containerlong_notm}} API を直接呼び出す](/docs/containers?topic=containers-cs_cli_install#cs_api) こともできます。
{: tip}

ユーザーが **{{site.data.keyword.Bluemix_notm}} IAM トークンを使用してポートフォリオにアクセスできる場合に、ユーザーが実行できるコマンドを制限するにはどうすればよいですか?**

アカウント内のユーザーのためにポートフォリオへのアクセスをセットアップしたら、該当する[プラットフォーム役割](#platform)を割り当てることで、ユーザーが実行できるインフラストラクチャー・アクションを制御できます。 ユーザーに {{site.data.keyword.Bluemix_notm}} IAM 役割を割り当てると、ユーザーがクラスターに対して実行できるコマンドが制限されます。 例えば、API キーの所有者は**スーパーユーザー**のインフラストラクチャー役割を持つので、クラスター内でインフラストラクチャー関連のコマンドをすべて実行できます。 しかし、ユーザーに割り当てられた {{site.data.keyword.Bluemix_notm}} IAM 役割によっては、それらのインフラストラクチャー関連コマンドの一部しか実行できない場合もあります。

例えば、新しい地域にクラスターを作成する場合、インフラストラクチャー役割**スーパーユーザー**を持つユーザー (アカウント所有者など) を使用して最初のクラスターを作成するようにしてください。 その後、その地域で個別ユーザーまたは {{site.data.keyword.Bluemix_notm}} IAM アクセス・グループのユーザーに対してプラットフォーム管理ポリシーを設定することで、これらのユーザーをその地域に招待できます。 **ビューアー**のプラットフォーム役割を持つユーザーは、ワーカー・ノードの追加を許可されません。 したがって、API キーに正しいインフラストラクチャー許可があっても、`worker-add` アクションは失敗します。 ユーザーのプラットフォーム役割を**オペレーター**に変更すると、ユーザーはワーカー・ノードの追加を許可されます。 ユーザーが許可され、API キーが正しく設定されるため、`worker-add` アクションは成功します。 ユーザーの IBM Cloud インフラストラクチャー (SoftLayer) の許可を編集する必要はありません。

アカウント内のユーザーが実行するアクションを監査するために、[{{site.data.keyword.cloudaccesstrailshort}}](/docs/containers?topic=containers-at_events) ですべてのクラスター関連イベントを表示することができます。
{: tip}

**API キー所有者または資格情報所有者にインフラストラクチャー役割スーパーユーザーを割り当てないとどうなりますか?**</br>

コンプライアンス、セキュリティー、または課金の理由から、API キーを設定しているユーザーまたは `ibmcloud ks credential-set` コマンドを使用して資格情報が設定されているユーザーにインフラストラクチャー役割**スーパーユーザー**を付与しない場合があります。 ただし、このユーザーに**スーパーユーザー**役割がない場合、インフラストラクチャー関連のアクション、例えば、クラスターの作成やワーカー・ノードの再ロードは失敗する可能性があります。 {{site.data.keyword.Bluemix_notm}} IAM プラットフォーム役割を使用してユーザーのインフラストラクチャー・アクセスを制御する代わりに、ユーザーに[特定の IBM Cloud インフラストラクチャー (SoftLayer) 許可を設定](#infra_access)する必要があります。

**地域とリソース・グループに関する API キーを設定したユーザーが退職した場合はどうなりますか?**

ユーザーが退職した場合は、{{site.data.keyword.Bluemix_notm}} アカウント所有者がそのユーザーの許可を削除できます。 ただし、ユーザーの特定のアクセス許可を削除したり、アカウントからユーザーを完全に削除したりする前に、別のユーザーのインフラストラクチャー資格情報を指定して API キーをリセットする必要があります。 そうしないと、アカウント内の他のユーザーが IBM Cloud インフラストラクチャー (SoftLayer) ポータルにアクセスできなくなり、インフラストラクチャー関連のコマンドが失敗する可能性があります。 詳しくは、[ユーザー許可の削除](#removing) を参照してください。

**API キーが漏えいした場合にクラスターをロックダウンするにはどうすればよいですか?**

クラスター内の地域とリソース・グループに設定した API キーが漏えいした場合は、その API キーを認証に使用した呼び出しを実行不能にするために、その [API キーを削除](/docs/iam?topic=iam-userapikey#delete_user_key)してください。 Kubernetes API サーバーへのアクセスを保護する方法について詳しくは、[Kubernetes API サーバーと etcd](/docs/containers?topic=containers-security#apiserver) のセキュリティーに関するトピックを参照してください。

**クラスターに API キーをセットアップするにはどうしたらよいですか?**</br>

IBM Cloud インフラストラクチャー (SoftLayer) ポートフォリオへのアクセスに使用するアカウントのタイプによって異なります。
* [デフォルトの {{site.data.keyword.Bluemix_notm}} 従量課金アカウント](#default_account)
* [デフォルトの {{site.data.keyword.Bluemix_notm}} 従量課金アカウントにリンクされていない別の IBM Cloud インフラストラクチャー (SoftLayer) アカウント](#credentials)

### API キーまたはインフラストラクチャー資格情報の所有者に正しい許可があることの確認
{: #owner_permissions}

クラスター内でインフラストラクチャー関連のすべてのアクションを正常に完了できるようにするには、API キーに設定する資格情報を持つユーザーに適切な許可がある必要があります。
{: shortdesc}

1. [{{site.data.keyword.Bluemix_notm}} コンソール ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/) にログインします。

2. アカウント関連のすべてのアクションが正常に実行されるようにするため、ユーザーに正しい {{site.data.keyword.Bluemix_notm}} IAM プラットフォーム役割があることを確認します。
    1. メニュー・バーで**「管理」>「アクセス (IAM)」**を選択し、**「ユーザー」**ページをクリックします。
    2. API キーを設定するユーザーまたは API キーに設定する資格情報を持つユーザーの名前をクリックし、**「アクセス・ポリシー」**タブをクリックします。
    3. ユーザーに、すべての地域のすべての {{site.data.keyword.containerlong_notm}} クラスターに対する**管理者**プラットフォーム役割がない場合は、[ユーザーにそのプラットフォーム役割を割り当てます](#platform)。
    4. ユーザーに、少なくとも API キーが設定されるリソース・グループに対する**ビューアー**・プラットフォーム役割がない場合は、[ユーザーにそのリソース・グループ役割を割り当てます](#platform)。
    5. クラスターを作成するには、{{site.data.keyword.registrylong_notm}} に対するアカウント・レベルの**管理者**のプラットフォーム役割もユーザーに必要です。 {{site.data.keyword.registryshort_notm}} のポリシーを、リソース・グループ・レベルに制限しないでください。

3. クラスター内でインフラストラクチャー関連のすべてのアクションが正常に実行されるようにするため、ユーザーに正しいインフラストラクチャー・アクセス・ポリシーがあることを確認します。
    1.  メニュー・バーから、**「管理」 > 「アクセス (IAM)」**を選択します。
    2.  **「ユーザー」**タブを選択し、ユーザーをクリックします。
    3. **「API キー」**ペインで、ユーザーが**「クラシック・インフラストラクチャー API キー」**を保持していることを確認するか、または**「IBM Cloud API キーの作成」**をクリックします。 詳しくは、[クラシック・インフラストラクチャー API キーの管理](/docs/iam?topic=iam-classic_keys#classic_keys)を参照してください。
    4. **「クラシック・インフラストラクチャー」**タブをクリックし、**「許可」**タブをクリックします。
    5. ユーザーが各カテゴリーにチェック・マークを付けていない場合は、**「許可セット」**ドロップダウン・リストを使用して、**「スーパーユーザー」**役割を割り当てることができます。 または、各カテゴリーを展開表示して、必要な[インフラストラクチャー許可](/docs/containers?topic=containers-access_reference#infra)をユーザーに付与することもできます。

### デフォルトの {{site.data.keyword.Bluemix_notm}} 従量課金アカウントでのインフラストラクチャー・ポートフォリオへのアクセス
{: #default_account}

{{site.data.keyword.Bluemix_notm}} 従量課金アカウントがある場合は、デフォルトで、リンクされた IBM Cloud インフラストラクチャー (SoftLayer) ポートフォリオへのアクセス権限があります。 この IBM Cloud インフラストラクチャー (SoftLayer) ポートフォリオから新規ワーカー・ノードや VLAN などのインフラストラクチャー・リソースを注文する際には、この API キーが使用されます。
{: shortdec}

現在の API キー所有者を調べるには、[`ibmcloud ks api-key-info --cluster <cluster>`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_api_key_info) を実行します。 地域に対して保管されている API キーを更新する必要があることがわかった場合は、[`ibmcloud ks api-key-reset --region <region>`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_api_key_reset) コマンドを実行して更新できます。このコマンドには {{site.data.keyword.containerlong_notm}} 管理アクセス・ポリシーが必要です。このコマンドを実行すると、実行したユーザーの API キーがアカウントに保管されます。

キーをリセットする場合、アプリへの影響を確認してください。 キーは、複数の異なる場所で使用されるので、不必要に変更されると破壊的変更が発生する可能性があります。
{: note}

**始める前に**:
- アカウント所有者が API キーを設定しない場合は、[API キーを設定するユーザーが正しい許可を得ていることを確認してください](#owner_permissions)。
- [アカウントにログインします。 該当する場合は、適切なリソース・グループをターゲットにします。 クラスターのコンテキストを設定します。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

IBM Cloud インフラストラクチャー (SoftLayer) ポートフォリオにアクセスするために API キーを設定するには、以下のようにします。

1.  クラスターが所在する地域およびリソース・グループの API キーを設定します。
    1.  使用するインフラストラクチャー許可を持つユーザーとして端末にログインします。
    2.  API キーを設定するリソース・グループをターゲットに指定します。 リソース・グループをターゲットに指定しないと、API キーはデフォルトのリソース・グループに対して設定されます。
        ```
        ibmcloud target -g <resource_group_name>
        ```
        {:pre}
    4.  ユーザーの地域の API キーを設定します。
        ```
        ibmcloud ks api-key-reset --region <region>
        ```
        {: pre}    
    5.  API キーが設定されていることを確認してください。
        ```
        ibmcloud ks api-key-info --cluster <cluster_name_or_ID>
        ```
        {: pre}

2. [クラスターを作成します](/docs/containers?topic=containers-clusters)。 クラスターの作成には、地域およびリソース・グループに対して設定した API キー資格情報が使用されます。

### 別の IBM Cloud インフラストラクチャー (SoftLayer) アカウントへのアクセス
{: #credentials}

地域内のクラスターのインフラストラクチャーを注文するために、デフォルトのリンクされた IBM Cloud インフラストラクチャー (SoftLayer) アカウントを使用する代わりに、既に所有している別の IBM Cloud インフラストラクチャー (SoftLayer) アカウントを使用する場合があります。 [`ibmcloud ks credential-set`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_set) コマンドを使用すると、そのようなインフラストラクチャー・アカウントを {{site.data.keyword.Bluemix_notm}} アカウントにリンクできます。 地域に対して保管されているデフォルトの従量課金アカウントの資格情報の代わりに IBM Cloud インフラストラクチャー (SoftLayer) の資格情報が使用されます。
{: shortdesc}

`ibmcloud ks credential-set` コマンドによって設定された IBM Cloud インフラストラクチャー (SoftLayer) の資格情報は、セッション終了後も永続します。 手動で設定した IBM Cloud インフラストラクチャー (SoftLayer) の資格情報を [`ibmcloud ks credential-unset --region <region>`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_unset) コマンドを使用して削除した場合、デフォルトの従量課金アカウントの資格情報が使用されます。ただし、インフラストラクチャー・アカウントの資格情報をこのように変更することによって、[孤立クラスター](/docs/containers?topic=containers-cs_troubleshoot_clusters#orphaned)が発生する可能性があります。
{: important}

**始める前に**:
- アカウント所有者の資格情報を使用していない場合は、[API キーに設定する資格情報を持つユーザーに正しい許可があることを確認します](#owner_permissions)。
- [アカウントにログインします。 該当する場合は、適切なリソース・グループをターゲットにします。 クラスターのコンテキストを設定します。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

IBM Cloud インフラストラクチャー (SoftLayer) ポートフォリオにアクセスするためにインフラストラクチャー・アカウントの資格情報を設定するには、以下のようにします。

1. IBM Cloud インフラストラクチャー (SoftLayer) ポートフォリオにアクセスするために使用するインフラストラクチャー・アカウントを取得します。 ご使用の[現在のアカウント・タイプ](#understand_infra)に依存する異なるオプションがあります。

2.  正しいアカウントのユーザーでインフラストラクチャー API 資格情報を設定します。

    1.  ユーザーのインフラストラクチャー API 資格情報を取得します。 資格情報は IBMid とは異なるので注意してください。

        1.  [{{site.data.keyword.Bluemix_notm}} ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/) コンソールで、**「管理」**>**「アクセス (IAM)」**>**「ユーザー」**テーブルを選択し、ユーザー名をクリックします。

        2.  **「API キー」**セクションで、クラシック・インフラストラクチャー API キーを見つけるか作成します。   

    2.  使用するインフラストラクチャー API 資格情報を設定します。
        ```
        ibmcloud ks credential-set --infrastructure-username <infrastructure_API_username> --infrastructure-api-key <infrastructure_API_authentication_key> --region <region>
        ```
        {: pre}

    3. 正しい資格情報が設定されていることを確認します。
        ```
        ibmcloud ks credential-get --region <region>
        ```
        出力例:
        ```
        Infrastructure credentials for user name user@email.com set for resource group default.
        ```
        {: screen}

3. [クラスターを作成します](/docs/containers?topic=containers-clusters)。 クラスターの作成には、地域およびリソース・グループに対して設定したインフラストラクチャー資格情報が使用されます。

4. 設定したインフラストラクチャー・アカウントの資格情報がクラスターで使用されていることを確認します。
  1. [{{site.data.keyword.containerlong_notm}} コンソール ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/kubernetes/clusters) を開き、クラスターを選択します。
  2. 「概要」タブで、**「インフラストラクチャー・ユーザー (Infrastructure User)」**フィールドを探します。
  3. このフィールドが表示されている場合は、この地域の従量課金 (PAYG) アカウントに付属しているデフォルトのインフラストラクチャー資格情報を使用していません。 代わりに、地域は、設定した別のインフラストラクチャー・アカウントの資格情報を使用するように設定されています。

<br />


## {{site.data.keyword.Bluemix_notm}} IAM によるユーザーへのクラスター・アクセス権限の付与
{: #platform}

ユーザーが {{site.data.keyword.containerlong_notm}} のクラスターを操作できるように、[{{site.data.keyword.Bluemix_notm}} コンソール](#add_users)または [CLI](#add_users_cli) で {{site.data.keyword.Bluemix_notm}} IAM プラットフォーム管理ポリシーおよびサービス・アクセス・ポリシーを設定します。 始める前に、ポリシーの内容、ポリシーを誰に割り当てることができるか、どのようなリソースにポリシーを付与できるかを検討するため、[アクセス・ポリシーと役割について](#access_policies)を確認してください。
{: shortdesc}

{{site.data.keyword.Bluemix_notm}} IAM 役割をサービス・アカウントに割り当てることはできません。 代わりに、直接 [RBAC 役割をサービス・アカウントに割り当てる](#rbac)ことができます。
{: tip}

### コンソールを使用した {{site.data.keyword.Bluemix_notm}} IAM 役割の割り当て
{: #add_users}

{{site.data.keyword.Bluemix_notm}} コンソールを使用して {{site.data.keyword.Bluemix_notm}} IAM プラットフォーム管理役割およびサービス・アクセス役割を割り当てることによって、クラスターへのアクセス権限をユーザーに付与します。
{: shortdesc}

<p class="tip">プラットフォーム役割をサービス役割と同時に割り当てないでください。 プラットフォーム役割とサービス役割は別々に割り当てる必要があります。</p>

始める前に、作業している {{site.data.keyword.Bluemix_notm}} アカウントの**管理者**プラットフォーム役割が割り当てられていることを確認します。

1. [{{site.data.keyword.Bluemix_notm}} コンソール ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/) にログインします。 メニュー・バーから、**「管理」 > 「アクセス (IAM)」**を選択します。

2. 個別にユーザーを選択するか、ユーザーのアクセス・グループを作成します。
    * **個別ユーザーに役割を割り当てるには、以下のようにします。**
      1. 左側のナビゲーションで、**「ユーザー」**ページをクリックし、許可を設定するユーザーの名前をクリックします。 ユーザーが表示されていない場合は、**「ユーザーの招待」**をクリックして、アカウントにユーザーを追加します。
      2. **「アクセス・ポリシー」**タブをクリックし、**「アクセス権限の割り当て」**をクリックします。
    * **アクセス・グループ内の複数のユーザーに役割を割り当てるには、以下のようにします。**
      1. 左側のナビゲーションで、**「アクセス・グループ」**ページをクリックします。
      2. **「作成」**をクリックして、グループに**「名前」**と**「説明」**を指定します。 **「作成」**をクリックします。
      3. **「ユーザーの追加」**をクリックして、ユーザーをアクセス・グループに追加します。 アカウントにアクセスできるユーザーのリストが表示されます。
      4. グループに追加するユーザーの横のボックスにチェック・マークを付けます。 ダイアログ・ボックスが表示されます。
      5. **「グループに追加」**をクリックします。
      6. **「アクセス・ポリシー」**タブをクリックします。
      7. **「アクセス権限の割り当て」**をクリックします。

3. リソース・グループまたはリソース・インスタンスのいずれかにポリシーを割り当てます。 ポリシーの対象を名前空間にする場合は、ポリシーを (リソース・グループにではなく) リソース・インスタンスに割り当てる必要があります。
  * **リソース・グループの場合**:
    1. **「リソース・グループ内のアクセス権限の割り当て」**をクリックします。
    2. リソース・グループ名を選択します。
    3. **「リソース・グループへのアクセス権限の割り当て」**ドロップダウン・リストで、ユーザーに付与する (グループ内のリソースではなく) リソース・グループ自体への許可のレベルを選択します。 例えば、ユーザーに複数のリソース・グループ内の利用可能なクラスターを表示させたい場合には、各リソース・グループの**ビューアー**役割をユーザーに付与します。
    4. **「サービス」**リストで、**「{{site.data.keyword.containershort_notm}}」**を選択します。
    5. **「地域」**リストから 1 つまたはすべての地域を選択します。
    6. ポリシーの役割を選択します。
       * **プラットフォーム・アクセス役割**: ユーザーがクラスター、ワーカー・ノード、ワーカー・プール、Ingress アプリケーションのロード・バランサー、ストレージなどのインフラストラクチャー・リソースを管理できるように、{{site.data.keyword.containerlong_notm}} へのアクセスを許可します。 役割別にサポートされる操作をリストしたものが、[プラットフォーム役割の参照ページ](/docs/containers?topic=containers-access_reference#iam_platform)にあります。
       * **サービス・アクセス役割**: ユーザーがポッド、デプロイメント、サービス、名前空間などの Kubernetes リソースを管理できるように、Kubernetes に対するアクセス (クラスター内からのアクセス) を許可します。 役割別にサポートされる操作をリストしたものが、[サービス役割の参照ページ](/docs/containers?topic=containers-access_reference#service)にあります。<p class="note">サービス・アクセス役割をリソース・グループ・レベルで割り当てる場合は、その役割の対象を名前空間にすることはできません。 代わりに、リソース・インスタンスに対するアクセス権限を割り当ててください。 また、プラットフォーム役割をサービス役割と同時に割り当てないでください。</p>
    7. **「割り当て」**をクリックします。
    8. **オプション**: ユーザーにサービス役割しか割り当てなかった場合は、ユーザーが `ibmcloud ks cluster-config` [コマンド](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_config)を実行して [CLI から Kubernetes ダッシュボードを起動](/docs/containers?topic=containers-app#db_cli)できるように、あるいは Kubernetes API と対話できるように、クラスター名と ID を伝えてください。 そのユーザーが {{site.data.keyword.containerlong_notm}} クラスター・コンソールにもアクセスして、CLI からクラスターやその他のインフラストラクチャー・リソースを一覧表示できるようにするには、この手順を繰り返してユーザーに**ビューアー**のプラットフォーム役割を与えてください。
  * **リソース・グループ内のリソース・インスタンスまたはリソース・グループをまたぐリソース・インスタンスの場合**:
    1. **「リソースへのアクセス権限の割り当て」**をクリックします。
    2. **「サービス」**リストで、**「{{site.data.keyword.containershort_notm}}」**を選択します。
    3. **「地域」**リストから 1 つまたはすべての地域を選択します。
    4. **「クラスター」**リストから 1 つまたはすべてのクラスター・インスタンスを選択します。
    5. **「名前空間」**フィールドに、_サービス・アクセス_・ポリシーの対象にする Kubernetes 名前空間の名前を入力します。 _プラットフォーム・アクセス_・ポリシーの対象を名前空間にすることはできないので注意してください。 このポリシーは、ある地域内のすべてのクラスターなど、これまでに選択したすべてのクラスター内の名前空間へのアクセスを付与します。 すべての名前空間へのアクセス権限を付与する場合は、名前空間フィールドを空白のままにしてください。
    6. ポリシーの役割を選択します。
       *  **プラットフォーム・アクセス役割**: ユーザーがクラスター、ワーカー・ノード、ワーカー・プール、Ingress アプリケーションのロード・バランサー、ストレージなどのインフラストラクチャー・リソースを管理できるように、{{site.data.keyword.containerlong_notm}} へのアクセスを許可します。 役割別にサポートされる操作をリストしたものが、[プラットフォーム役割の参照ページ](/docs/containers?topic=containers-access_reference#iam_platform)にあります。
          * 1 つのクラスターのみに対する**管理者**プラットフォーム役割をユーザーに割り当てる場合、リソース・グループ内のその地域のすべてのクラスターに対する**ビューアー**・プラットフォーム役割もユーザーに割り当てる必要があります。
          * ポリシーの対象を名前空間にした場合に、同時にプラットフォームの役割も割り当てることはできません。 ユーザーにプラットフォーム役割も割り当てる場合は、この手順を繰り返し (ただし、名前空間フィールドは空白にします)、プラットフォーム役割のみを割り当ててください (サービス・アクセス役割は再度割り当てないでください)。
       * **サービス・アクセス役割**: ユーザーがポッド、デプロイメント、サービス、名前空間などの Kubernetes リソースを管理できるように、Kubernetes に対するアクセス (クラスター内からのアクセス) を許可します。 役割別にサポートされる操作をリストしたものが、[サービス役割の参照ページ](/docs/containers?topic=containers-access_reference#service)にあります。
    7. **「割り当て」**をクリックします。
    8. **オプション**: ユーザーにサービス役割しか割り当てなかった場合は、ユーザーが `ibmcloud ks cluster-config` [コマンド](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_config)を実行して [CLI から Kubernetes ダッシュボードを起動](/docs/containers?topic=containers-app#db_cli)できるように、あるいは Kubernetes API と対話できるように、クラスター名と ID を伝えてください。 そのユーザーが {{site.data.keyword.containerlong_notm}} クラスター・コンソールにもアクセスして、CLI からクラスターやその他のインフラストラクチャー・リソースを一覧表示できるようにするには、この手順を繰り返してユーザーに**ビューアー**のプラットフォーム役割を与えてください。

4.  オプション: デフォルト以外のリソース・グループにあるクラスターでユーザーが作業できるようにする場合、これらのユーザーには、そのクラスターのあるリソース・グループへの追加のアクセス権限が必要になります。 そのようなユーザーには、リソース・グループに対する**ビューアー**以上のプラットフォーム役割を割り当てることができます (まだ割り当てていない場合)。
    1.  **「リソース・グループ内のアクセス権限の割り当て」**をクリックします。
    2.  リソース・グループ名を選択します。
    3.  **「リソース・グループへのアクセス権限の割り当て」**リストから、**ビューアー**役割を選択します。 この役割によって、ユーザーはリソース・グループ自体へのアクセスは許可されますが、グループ内のリソースへのアクセスが許可されるわけではありません。
    4.  **「割り当て」**をクリックします。

5.  ユーザーを追加するために、RBAC 許可をクラスターに同期する必要があります。 アクセス権限を付与されたユーザーが、同期を開始するために [Kubernetes ダッシュボード](/docs/containers?topic=containers-app#db_gui)を起動する必要があります。 RBAC 許可はキャッシュに入れられているので、同期はすぐには行われない可能性があります。

### CLI を使用した {{site.data.keyword.Bluemix_notm}} IAM 役割の割り当て
{: #add_users_cli}

CLI を使用して {{site.data.keyword.Bluemix_notm}} IAM プラットフォーム管理役割およびサービス・アクセス役割を割り当てることによって、クラスターへのアクセス権限をユーザーに付与します。
{: shortdesc}

**始める前に**:

- 作業している {{site.data.keyword.Bluemix_notm}} アカウントの `cluster-admin` {{site.data.keyword.Bluemix_notm}} IAM プラットフォーム役割が割り当てられていることを確認します。
- ユーザーがアカウントに追加されていることを確認します。 ユーザーが追加されていない場合、`ibmcloud account user-invite <user@email.com>` を実行してユーザーをアカウントに招待します。
- [アカウントにログインします。 該当する場合は、適切なリソース・グループをターゲットにします。 クラスターのコンテキストを設定します。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
- [プラットフォームとサービス](/docs/containers?topic=containers-users#access_policies)のいずれのアクセス役割を割り当てるかを決定します。 CLI ステップは、割り当てるアクセス役割によって異なります。
  * [CLI からのプラットフォーム役割の割り当て](#add_users_cli_platform)
  * [CLI からのサービス役割の割り当て](#add_users_cli_service)

**CLI から {{site.data.keyword.Bluemix_notm}} IAM _プラットフォーム_ 役割を割り当てるには、以下のようにします。**
{: #add_users_cli_platform}

1.  {{site.data.keyword.containerlong_notm}} の許可を設定するための {{site.data.keyword.Bluemix_notm}} IAM アクセス・ポリシーを作成します (**`--service-name containers-kubernetes`**)。 何に対するアクセス権限を割り当てるのかに応じて、アクセス・ポリシーの対象を決定します。

    <table summary="この表は、CLI フラグを使用してポリシーの対象にできるアクセス領域について説明しています。行は左から右に読み、1 列目が対象、2 列目が CLI フラグ、3 列目が説明です。">
    <caption>アクセス・ポリシーの対象を指定するためのオプション</caption>
      <thead>
      <th>対象</th>
      <th>CLI フラグ</th>
      <th>説明</th>
      </thead>
      <tbody>
        <tr>
        <td>ユーザー</td>
        <td>N/A</td>
        <td>ポリシーを個人またはユーザーのグループに割り当てることができます。 この位置引数はコマンドの直後に置きます。
        <ul><li>**個別のユーザー**: ユーザーの E メール・アドレスを入力します。</li>
        <li>**アクセス・グループ**: ユーザーのアクセス・グループの名前を入力します。 アクセス・グループは、`ibmcloud iam access-group-create` コマンドで作成できます。 使用可能なアクセス・グループをリストするには、`ibmcloud iam access-groups` を実行します。 ユーザーをアクセス・グループに追加するには、`ibmcloud iam access-group-user-add <access_group_name> <user_email>` を実行します。</li></ul></td>
        </tr>
        <tr>
        <td>リソース・グループ</td>
        <td>`--resource-group-name`</td>
        <td>リソース・グループのポリシーを付与できます。 リソース・グループまたは特定のクラスター ID を指定しない場合、ポリシーはすべてのリソース・グループのすべてのクラスターに適用されます。 使用可能なリソース・グループをリストするには、`ibmcloud resource groups` を実行します。</td>
        </tr>
        <tr>
        <td>クラスター</td>
        <td>`--service-instance`</td>
        <td>ポリシーを単一のクラスターだけに限定できます。 クラスター ID をリストするには、`ibmcloud ks clusters` を実行します。 **注**: 1 つのクラスターのみに対する**管理者**プラットフォーム役割をユーザーに割り当てる場合、リソース・グループ内の地域のすべてのクラスターに対する**ビューアー**・プラットフォーム役割もユーザーに割り当てる必要があります。</td>
        </tr>
        <tr>
        <td>地域</td>
        <td>`--region`</td>
        <td>特定の地域内のクラスターに適用するようにポリシーの対象を指定できます。 地域または特定のクラスター ID を指定しない場合、ポリシーはすべての地域のすべてのクラスターに適用されます。 使用可能な地域をリストするには、`ibmcloud ks regions` を実行し、**Region Alias** 列の値を使用します。</td>
        </tr>
        <tr>
        <td>役割</td>
        <td>`--role`</td>
        <td>割り当てる[プラットフォーム役割](/docs/containers?topic=containers-access_reference#iam_platform)を選択します。 指定可能な値は、`Administrator`、`Operator`、`Editor`、または `Viewer` です。</td>
        </tr>
      </tbody>
      </table>

    **コマンド例**:

    *  個人ユーザーに、デフォルト・リソース・グループの米国東部地域の 1 つのクラスターに対する**ビューアー**のプラットフォーム役割を割り当てます。
       ```
       ibmcloud iam user-policy-create user@email.com --resource-group-name default --service-name containers-kubernetes --region us-east --service-instance clusterID-1111aa2b2bb22bb3333c3c4444dd4ee5 --roles Viewer
       ```
       {: pre}

    *  個人ユーザーに、`HR` リソース・グループ内のすべてのクラスターへの**管理者**のプラットフォーム・アクセス権限を割り当てます。
       ```
       ibmcloud iam user-policy-create user@email.com --resource-group-name HR --service-name containers-kubernetes [--region <region>] --roles Administrator
       ```
       {: pre}

    *  `auditors` グループのユーザーに、すべてのリソース・グループ内のすべてのクラスターに対する**ビューアー**のプラットフォーム役割を割り当てます。
       ```
       ibmcloud iam access-group-policy-create auditors --service-name containers-kubernetes --roles Viewer
       ```
       {: pre}

2. デフォルト以外のリソース・グループにあるクラスターでユーザーが作業できるようにする場合、これらのユーザーには、そのクラスターのあるリソース・グループへの追加のアクセス権限が必要になります。 これらのユーザーには、少なくともリソース・グループに対する**ビューアー**役割を割り当てることができます。 `ibmcloud resource group <resource_group_name> --id` を実行することによって、リソース・グループ ID を見つけることができます。
    *   個人ユーザーの場合:
        ```
        ibmcloud iam user-policy-create <user@email.com> --resource-type resource-group --resource <resource_group_ID> --roles Viewer
        ```
        {: pre}
    *   アクセス・グループの場合:
        ```
        ibmcloud iam access-group-policy-create <access_group> --resource-type resource-group --resource <resource_group_ID> --roles Viewer
        ```
        {: pre}

3.  ユーザーまたはアクセス・グループにプラットフォーム役割が割り当てられていることを確認します。
    *   個人ユーザーの場合:
        ```
        ibmcloud iam user-policies <user@email.com>
        ```
        {: pre}
    *   アクセス・グループの場合:
        ```
        ibmcloud iam access-group-policies <access_group>
        ```
        {: pre}

<br>
<br>

**CLI から {{site.data.keyword.Bluemix_notm}} IAM _サービス_ 役割を割り当てるには、以下のようにします。**
{: #add_users_cli_service}

1.  サービス役割を割り当てる個人ユーザーまたはアクセス・グループのユーザー情報を取得します。

    1.  **アカウント ID** を取得します。
        ```
        ibmcloud account show
        ```
        {: pre}
    2.  個人ユーザーの場合、ユーザーの **userID** と **ibmUniqueId** を取得します。
        ```
        ibmcloud account users --account-id <account_ID> --output JSON
        ```
        {: pre}
    3.  アクセス・グループの場合、**名前**と**ID** を取得します。
        ```
        ibmcloud iam access-groups
        ```
        {: pre}

2.  サービス・アクセス役割の有効範囲をクラスター内の Kubernetes 名前空間に設定する `policy.json` ファイルを作成します。

    ```
    {
        "subjects": [
            {
                "attributes": [
                    {
                        "name": "(iam_id|access_group_id)",
                        "value": "<user_or_group_ID>"
                    }
                ]
            }
        ],
        "roles": [
            {
                "role_id": "crn:v1:bluemix:public:iam::::serviceRole:<(Manager|Writer|Reader)>"
            }
        ],
        "resources": [
            {
                "attributes": [
                    {
                        "name": "accountId",
                        "value": "<account_ID>"
                    },
                    {
                        "name": "serviceName",
                        "value": "containers-kubernetes"
                    },
                    {
                        "name": "serviceInstance",
                        "value": "<cluster_ID1,cluster_ID2>"
                    },
                    {
                        "name": "namespace",
                        "value": "<namespace_name>"
                    }
                ]
            }
        ]
    }
    ```
    {: codeblock}

    <table summary="この表は JSON ファイルの入力するフィールドを説明します。行は左から右に読みます。1 列目は有効範囲、2 列目は CLI フラグ、3 列目は説明です。">
    <caption>JSON ファイルの構成要素について</caption>
      <thead>
      <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/>JSON ファイルの構成要素について</th>
      </thead>
      <tbody>
        <tr>
        <td>`subjects.attributes`</td>
        <td>前に取得した個人ユーザーまたはアクセス・グループの {{site.data.keyword.Bluemix_notm}} IAM 詳細を入力します。
        <ul><li>個人ユーザーの場合、`name` フィールドの `iam_id` を設定します。 `value` フィールドに前に取得した **ibmUniqueId** を入力します。</li>
        <li>アクセス・グループの場合、`name` フィールドの `access_group_id` を設定します。 `value` フィールドに前に取得した **ID** を入力します。</li></ul></td>
        </tr>
        <tr>
        <td>`roles.role_id`</td>
        <td>割り当てる [IAM サービス・アクセス役割](/docs/containers?topic=containers-access_reference#service)を選択します。 使用可能な値を以下に示します。
        <ul><li>`crn:v1:bluemix:public:iam::::serviceRole:Manager`</li>
        <li>`crn:v1:bluemix:public:iam::::serviceRole:Writer`</li>
        <li>`crn:v1:bluemix:public:iam::::serviceRole:Reader`</li></ul></td>
        </tr>
        <tr>
        <td>`resources.attributes`</td>
        <td>アカウント、クラスター、および名前空間に対するポリシーの有効範囲を構成します。 `"name"` フィールドは例で指定されているとおりにし、以下のように特定の `"value"` フィールドに入力します。
        <ul><li>**`"accountId"`**: 前に取得した {{site.data.keyword.Bluemix_notm}} アカウント ID を入力します</li>
        <li>**`"serviceName"`**: 指定されたサービス名をそのまま使用します (`containers-kubernetes`)。</li>
        <li>**`"serviceInstance"`**: クラスター ID を入力します。 クラスターが複数ある場合はコンマで区切ります。 クラスター ID を取得するには、`ibmcloud ks clusters` を実行します。</li>
        <li>**`"namespace"`**: クラスター内の Kubernetes 名前空間を入力します。 クラスター内の名前空間をリストするには、`kubectl get namespaces` を実行します。 <p class="note">クラスター内のすべての名前空間にアクセス・ポリシーを割り当てるには、`{"name": "namespace", "value": "<namespace_name"}` エントリー全体を削除します。</p></li></td>
        </tr>
      </tbody>
      </table>

3.  個人ユーザーまたはアクセス・グループに {{site.data.keyword.Bluemix_notm}} IAM ポリシーを適用します。
    *   個人ユーザーの場合:
        ```
        ibmcloud iam user-policy-create <user@email.com> --file <filepath>/policy.json
        ```
        {: pre}
    *   アクセス・グループの場合:
        ```
        ibmcloud iam access-group-policy-create <access_group> --file <filepath>/policy.json
        ```
        {: pre}

4.  ユーザーにサービス役割しか割り当てなかった場合は、ユーザーが `ibmcloud ks cluster-config` [コマンド](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_config)を実行して [CLI から Kubernetes ダッシュボードを起動](/docs/containers?topic=containers-app#db_cli)できるように、あるいは Kubernetes API と対話できるように、クラスター名と ID を伝えてください。 さらにそのユーザーが {{site.data.keyword.containerlong_notm}} クラスター・コンソールにもアクセスして、CLI からクラスターやその他のインフラストラクチャー・リソースを一覧表示できるようにするには、[ユーザーに**ビューアー**のプラットフォーム役割を与えてください](#add_users_cli_platform)。

5.  変更を反映させるために、アクセス権限を与えられたユーザーがクラスター構成をリフレッシュする必要があります。 複数のユーザーを同時に追加した場合でも、ユーザーは、個別にクラスター構成をリフレッシュするまで役割バインディングに追加されません。 また、より高い許可を持っている場合にもユーザーは役割バインディングに追加されません。 例えば、ユーザーがクラスター役割を割り当てられており、クラスター役割バインディング内にいる場合は、個別の名前空間の役割バインディングには追加されません。
    ```
    ibmcloud ks cluster-config --cluster <cluster_name_or_id>
    ```
    {: pre}

6.  **オプション**: 対応する [RBAC 役割バインディングまたはクラスター役割バインディング](#role-binding)にユーザーが追加されていることを確認します。 役割バインディングとクラスター役割バインディングを確認するには、クラスター管理者 (すべての名前空間の**管理者**のサービス役割) でなければなりません。
    役割バインディングまたはクラスター役割バインディングで役割を確認します。
    *   リーダー:
        ```
        kubectl get rolebinding ibm-view -o yaml -n <namespace>
        ```
        {: pre}
    *   ライター:
        ```
        kubectl get rolebinding ibm-edit -o yaml -n <namespace>
        ```
        {: pre}
    *   管理者 (1 つの名前空間が対象):
        ```
        kubectl get rolebinding ibm-operate -o yaml -n <namespace>
        ```
        {: pre}
    *   管理者 (すべての名前空間):
        ```
        kubectl get clusterrolebinding ibm-admin -o yaml
        ```
        {: pre}

    **出力例**: ユーザー `user@email.com` とアクセス・グループ `team1` に**リーダー**・サービス役割を割り当て、`kubectl get rolebinding ibm-view -o yaml -n default` を実行した場合は、以下の出力例が示されます。

    ```
    apiVersion: rbac.authorization.k8s.io/v1
    kind: RoleBinding
    metadata:
      creationTimestamp: 2018-05-23T14:34:24Z
      name: ibm-view
      namespace: default
      resourceVersion: "8192510"
      selfLink: /apis/rbac.authorization.k8s.io/v1/namespaces/default/rolebindings/ibm-view
      uid: 63f62887-5e96-11e8-8a75-b229c11ba64a
    roleRef:
      apiGroup: rbac.authorization.k8s.io
      kind: ClusterRole
      name: view
    subjects:
    - apiGroup: rbac.authorization.k8s.io
      kind: User
      name: IAM#user@email.com
    - apiGroup: rbac.authorization.k8s.io
      kind: group
      name: team1
    ```
    {: screen}

<br />


## RBAC 許可の割り当て
{: #role-binding}

RBAC 役割を使用して、クラスター内の Kubernetes リソースを操作するためにユーザーが実行できるアクションを定義します。
{: shortdesc}

**RBAC 役割および RBAC クラスター役割とは何ですか?**</br>
RBAC 役割および RBAC クラスター役割によって、ユーザーがクラスター内の Kubernetes リソースと対話できる方式についての一連の許可が定義されます。 役割の有効範囲は、特定の名前空間内のリソースに設定されます (デプロイメントなど)。 クラスター役割の有効範囲は、クラスター全体のリソース (ワーカー・ノードなど)、または各名前空間内で見つかる名前空間に有効範囲設定されたリソース (ポッドなど) に設定されます。

**RBAC 役割バインディングおよび RBAC クラスター役割バインディングとは何ですか?**</br>
役割バインディングによって、RBAC 役割または RBAC クラスター役割が特定の名前空間に適用されます。 役割バインディングを使用して役割を適用すると、ユーザー・アクセス権限を、特定の名前空間内の特定のリソースに付与することになります。 役割バインディングを使用してクラスター役割を適用すると、ユーザー・アクセス権限を、各名前空間内で見つかる名前空間に有効範囲設定されたリソース (ポッドなど) に付与することになりますが、特定の名前空間内でのみ有効です。

クラスター役割バインディングによって、RBAC クラスター役割がクラスター内のすべての名前空間に適用されます。 クラスター役割バインディングを使用してクラスター役割を適用すると、ユーザー・アクセス権限を、クラスター全体のリソース (ワーカー・ノードなど)、またはすべての名前空間内の名前空間に有効範囲設定されたリソース (ポッドなど) に付与することになります。

**これらの役割のクラスター内での機能は?**</br>
ユーザーがクラスター内から Kubernetes リソースを操作できるようにするには、[{{site.data.keyword.Bluemix_notm}}IAM サービス役割](#platform)を介してユーザーに 1 つ以上の名前空間に対するアクセス権限を割り当てる必要があります。 サービス役割が割り当てられているすべてのユーザーには、対応する RBAC クラスター役割が自動的に割り当てられます。 これらの RBAC クラスター役割は事前定義されており、これらによってユーザーはクラスター内の Kubernetes リソースと対話することが許可されます。 また、特定の名前空間にクラスター役割を適用するために役割バインディングが作成されるか、すべての名前空間にクラスター役割を適用するためにクラスター役割バインディングが作成されます。

RBAC 役割ごとに許可されているアクションについて詳しくは、リファレンス・トピックの [{{site.data.keyword.Bluemix_notm}} IAM サービス役割](/docs/containers?topic=containers-access_reference#service)を確認してください。 各 RBAC 役割によって付与される、個々の Kubernetes リソースに対する権限を確認するには、[RBAC 役割別の Kubernetes リソースの許可](/docs/containers?topic=containers-access_reference#rbac_ref)を確認してください。
{: tip}

**カスタム役割またはカスタム・クラスター役割を作成できますか?**
クラスター役割 `view`、`edit`、`admin` および `cluster-admin` は、対応する {{site.data.keyword.Bluemix_notm}} IAM サービス役割をユーザーに割り当てたときに自動的に作成される事前定義役割です。 他の Kubernetes 許可を付与するには、[カスタム RBAC 許可を作成できます](#rbac)。 カスタム RBAC 役割は追加型です。既にサービス・アクセス役割に割り当てられた可能性がある RBAC 役割を変更したりオーバーライドしたりはしません。 カスタム RBAC 許可を作成するには、IAM の**管理者**のサービス・アクセス役割が必要です。これにより、Kubernetes の `cluster-admin` の RBAC 役割が付与されます。 ただし、自分で独自のカスタム Kubernetes RBAC 役割を管理している場合、他のユーザーには IAM サービス・アクセス役割は必要ありません。

独自のカスタム RBAC ポリシーを作成していますか? クラスター内の既存の IBM 役割バインディングを編集したり、新規の役割バインディングに同じ名前を付けたりしないでください。 IBM 提供の RBAC 役割バインディングは、変更しても定期的に上書きされます。 代わりに、独自の役割バインディングを作成してください。
{: tip}

**設定した {{site.data.keyword.Bluemix_notm}} IAM 許可に結合していないクラスター役割バインディングおよび役割バインディングはいつ使用する必要がありますか?**
クラスター内でポッドを作成したりアップデートしたりできる権限を与える場合があります。 [ポッド・セキュリティー・ポリシー](/docs/containers?topic=containers-psp#psp)を使用して、クラスターに付属している既存のクラスター役割バインディングを使用することも、独自のものを作成することもできます。

クラスターにアドオンを組み込む場合もあります。 例えば、[クラスターに Helm をセットアップする](/docs/containers?topic=containers-helm#public_helm_install)場合、Tiller のサービス・アカウントを `kube-system` 名前空間に作成し、`tiller-deploy` ポッドに対する Kubernetes RBAC クラスター役割バインディングを作成する必要があります。

### ユーザー、グループ、またはサービス・アカウントに対するカスタム RBAC 許可の作成
{: #rbac}

クラスター役割 `view`、`edit`、`admin` および `cluster-admin` は、対応する {{site.data.keyword.Bluemix_notm}} IAM サービス役割を割り当てると、自動的に作成されます。 これらの事前定義許可で実行されるものよりも粒度の高いクラスター・アクセス・ポリシーが必要ですか? 問題ありません。 カスタム RBAC 役割およびカスタム RBAC クラスター役割を作成できます。
{: shortdesc}

カスタム RBAC 役割およびカスタム RBAC クラスター役割を個別のユーザー、ユーザー・グループ (Kubernetes v1.11 以降を実行するクラスター内)、またはサービス・アカウントに割り当てることができます。 グループに対してバインディングが作成されると、そのグループに追加またはそのグループから削除されるすべてのユーザーに影響を与えます。 ユーザーをグループに追加すると、ユーザーは、付与された個別のアクセス権限に加えて、グループのアクセス権限を得ます。 ユーザーを削除した場合、アクセス権限は取り消されます。 サービス・アカウントをアクセス・グループに追加することはできないことに注意してください。

継続的デリバリー・ツールチェーンなど、ポッドで実行されるプロセスにアクセス権限を割り当てる場合、[Kubernetes `ServiceAccounts` ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/reference/access-authn-authz/service-accounts-admin/) を使用できます。 Travis と Jenkins のサービス・アカウントをセットアップしてカスタム RBAC 役割をサービス・アカウントに割り当てる方法を説明するチュートリアルに従うには、ブログ投稿 [Kubernetes `ServiceAccounts` for use in automated systems ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://medium.com/@jakekitchener/kubernetes-serviceaccounts-for-use-in-automated-systems-515297974982) を参照してください。

破壊的変更を防止するため、事前定義の `view`、`edit`、`admin`、および `cluster-admin` の各クラスター役割は変更しないでください。 カスタム RBAC 役割は追加型です。既に {{site.data.keyword.Bluemix_notm}} IAM サービス・アクセス役割に割り当てられた可能性がある RBAC 役割を変更したりオーバーライドしたりはしません。
{: important}

**役割またはクラスター役割を作成しますか? 役割バインディングまたはクラスター役割バインディングをそれに適用しますか?**

* **名前空間アクセス**: 特定の名前空間内のリソースへのアクセスをユーザー、アクセス・グループ、またはサービス・アカウントに許可するには、以下の組み合わせのいずれかを選択します。
  * 役割を作成して、役割バインディングをそれに適用します。 このオプションは、1 つの名前空間にのみ存在する固有リソース (アプリのデプロイメントなど) へのアクセスを制御する場合に役立ちます。
  * クラスター役割を作成して、役割バインディングをそれに適用します。 このオプションは、1 つの名前空間内の一般リソース (ポッドなど) へのアクセスを制御する場合に役立ちます。
* **クラスター全体のアクセス**: クラスター全体のリソースまたはすべての名前空間内のリソースへのアクセスをユーザーまたはアクセス・グループに許可するには、クラスター役割を作成して、クラスター役割バインディングをそれに適用します。 このオプションは、有効範囲が名前空間に設定されていないリソース (ワーカー・ノードなど)、またはクラスターのすべての名前空間内のリソース (各名前空間のポッドなど) へのアクセスを制御する場合に役立ちます。

**始める前に**:

- クラスターを [Kubernetes CLI](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure) のターゲットとして設定します。
- すべての名前空間に対する[**管理者**の {{site.data.keyword.Bluemix_notm}} IAM サービス役割](/docs/containers?topic=containers-users#platform)があることを確認してください。
- 個別のユーザーまたはアクセス・グループ内のユーザーにアクセス権限を割り当てるには、ユーザーまたはグループに {{site.data.keyword.containerlong_notm}} のサービス・レベルで少なくとも 1 つの [{{site.data.keyword.Bluemix_notm}}IAM プラットフォーム役割](#platform)が割り当てられていることを確認します。

**カスタム RBAC 許可を作成するには、以下のようにします。**

1. 割り当てるアクセス権限を含む役割またはクラスター役割を作成します。

    1. 役割またはクラスター役割を定義するための `.yaml` ファイルを作成します。

        ```
        kind: Role
        apiVersion: rbac.authorization.k8s.io/v1
        metadata:
          namespace: default
          name: my_role
        rules:
        - apiGroups: [""]
          resources: ["pods"]
          verbs: ["get", "watch", "list"]
        - apiGroups: ["apps", "extensions"]
          resources: ["daemonsets", "deployments"]
          verbs: ["get", "list", "watch", "create", "update", "patch", "delete"]
        ```
        {: codeblock}

        <table>
        <caption>YAML の構成要素について</caption>
          <thead>
            <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> YAML の構成要素について</th>
          </thead>
          <tbody>
            <tr>
              <td><code>kind</code></td>
              <td>特定の名前空間内のリソースへのアクセス権限を付与する場合は `Role` を使用します。 クラスター全体のリソース (ワーカー・ノードなど)、または名前空間に有効範囲が設定されたリソース (すべての名前空間のポッドなど) へのアクセス権限を付与する場合は `ClusterRole` を使用します。</td>
            </tr>
            <tr>
              <td><code>apiVersion</code></td>
              <td><ul><li>Kubernetes 1.8 以降を実行するクラスターの場合は、`rbac.authorization.k8s.io/v1` を使用します。 </li><li>それより前のバージョンの場合は、`apiVersion: rbac.authorization.k8s.io/v1beta1` を使用します。</li></ul></td>
            </tr>
            <tr>
              <td><code>metadata.namespace</code></td>
              <td>kind が `Role` のみの場合: アクセス権限を付与する Kubernetes 名前空間を指定します。</td>
            </tr>
            <tr>
              <td><code>metadata.name</code></td>
              <td>役割またはクラスター役割の名前を指定します。</td>
            </tr>
            <tr>
              <td><code>rules.apiGroups</code></td>
              <td>ユーザーに対話を許可する Kubernetes [API グループ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/reference/using-api/api-overview/#api-groups) (`"apps"`、`"batch"`、`"extensions"` など) を指定します。 REST パス `api/v1` にあるコア API グループに対するアクセス権限については、`[""]` のようにグループをブランクにします。</td>
            </tr>
            <tr>
              <td><code>rules.resources</code></td>
              <td>アクセス権限を付与する Kubernetes [リソース・タイプ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/reference/kubectl/cheatsheet/) (`"daemonsets"`、`"deployments"`、`"events"`、`"ingresses"` など) を指定します。 `"nodes"` を指定する場合、kind は `ClusterRole` でなければなりません。</td>
            </tr>
            <tr>
              <td><code>rules.verbs</code></td>
              <td>ユーザーに実行を許可する [アクション ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubectl.docs.kubernetes.io/) のタイプ (`"get"`、`"list"`、`"describe"`、`"create"`、`"delete"` など) を指定します。</td>
            </tr>
          </tbody>
        </table>

    2. クラスター内に役割またはクラスター役割を作成します。

        ```
        kubectl apply -f my_role.yaml
        ```
        {: pre}

    3. 役割またはクラスター役割が作成されたことを確認します。
      * 役割:
          ```
          kubectl get roles -n <namespace>
          ```
          {: pre}

      * クラスター役割:
          ```
          kubectl get clusterroles
          ```
          {: pre}

2. ユーザーを役割またはクラスター役割にバインドします。

    1. ユーザーを役割またはクラスター役割にバインドするための `.yaml` ファイルを作成します。 各件名 (subject) の name に使用している固有の URL に注目してください。

        ```
        kind: RoleBinding
        apiVersion: rbac.authorization.k8s.io/v1
        metadata:
          name: my_role_binding
          namespace: default
        subjects:
        - kind: User
          name: IAM#user1@example.com
          apiGroup: rbac.authorization.k8s.io
        - kind: Group
          name: team1
          apiGroup: rbac.authorization.k8s.io
        - kind: ServiceAccount
          name: <service_account_name>
          namespace: <kubernetes_namespace>
        roleRef:
          kind: Role
          name: my_role
          apiGroup: rbac.authorization.k8s.io
        ```
        {: codeblock}

        <table>
        <caption>YAML の構成要素について</caption>
          <thead>
            <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> YAML の構成要素について</th>
          </thead>
          <tbody>
            <tr>
              <td><code>kind</code></td>
              <td><ul><li>名前空間特定の `Role` または `ClusterRole` の場合 `RoleBinding` を指定します。</li><li>クラスター全体の `ClusterRole` の場合 `ClusterRoleBinding` を指定します。</li></ul></td>
            </tr>
            <tr>
              <td><code>apiVersion</code></td>
              <td><ul><li>Kubernetes 1.8 以降を実行するクラスターの場合は、`rbac.authorization.k8s.io/v1` を使用します。 </li><li>それより前のバージョンの場合は、`apiVersion: rbac.authorization.k8s.io/v1beta1` を使用します。</li></ul></td>
            </tr>
            <tr>
              <td><code>metadata.namespace</code></td>
              <td><ul><li>kind が `RoleBinding` の場合: アクセス権限を付与する Kubernetes 名前空間を指定します。</li><li>kind が `ClusterRoleBinding` の場合: `namespace` フィールドは使用しません。</li></ul></td>
            </tr>
            <tr>
              <td><code>metadata.name</code></td>
              <td>役割バインディングまたはクラスター役割バインディングの名前を指定します。</td>
            </tr>
            <tr>
              <td><code>subjects.kind</code></td>
              <td>次のいずれかの種類を指定します。
              <ul><li>`User`: RBAC 役割またはクラスター役割をアカウント内の個々のユーザーにバインドします。</li>
              <li>`Group`: Kubernetes 1.11 以降を実行するクラスターの場合、RBAC 役割またはクラスター役割をアカウント内の [{{site.data.keyword.Bluemix_notm}}IAM アクセス・グループ](/docs/iam?topic=iam-groups#groups)にバインドします。</li>
              <li>`ServiceAccount`: RBAC 役割またはクラスター役割をクラスター内の名前空間のサービス・アカウントにバインドします。</li></ul></td>
            </tr>
            <tr>
              <td><code>subjects.name</code></td>
              <td><ul><li>`User` の場合: <code>IAM#user@email.com</code> のように、`IAM#` に個別ユーザーの E メール・アドレスを追加します。</li>
              <li>`Group` の場合: Kubernetes 1.11 以降を実行するクラスターの場合、アカウント内の [{{site.data.keyword.Bluemix_notm}}IAM アクセス・グループ](/docs/iam?topic=iam-groups#groups)の名前を指定します。</li>
              <li>`ServiceAccount` の場合: サービス・アカウント名を指定します。</li></ul></td>
            </tr>
            <tr>
              <td><code>subjects.apiGroup</code></td>
              <td><ul><li>`User` または `Group` の場合: `rbac.authorization.k8s.io` を使用します。</li>
              <li>`ServiceAccount` の場合: このフィールドを含めないでください。</li></ul></td>
            </tr>
            <tr>
              <td><code>subjects.namespace</code></td>
              <td>`ServiceAccount` の場合のみ: サービス・アカウントがデプロイされる Kubernetes 名前空間の名前を指定します。</td>
            </tr>
            <tr>
              <td><code>roleRef.kind</code></td>
              <td>役割用の `.yaml` ファイルの `kind` と同じ値 (`Role` または `ClusterRole`) を入力します。</td>
            </tr>
            <tr>
              <td><code>roleRef.name</code></td>
              <td>役割用の `.yaml` ファイルの名前を入力します。</td>
            </tr>
            <tr>
              <td><code>roleRef.apiGroup</code></td>
              <td>`rbac.authorization.k8s.io` を使用します。</td>
            </tr>
          </tbody>
        </table>

    2. クラスター内に役割バインディングまたはクラスター役割バインディングのリソースを作成します。

        ```
        kubectl apply -f my_role_binding.yaml
        ```
        {: pre}

    3.  バインディングが作成されたことを確認します。

        ```
        kubectl get rolebinding -n <namespace>
        ```
        {: pre}

3. オプション: 他の名前空間で同じレベルのユーザー・アクセスを実行するには、他の名前空間にこれらの役割またはクラスター役割に対する役割バインディングをコピーします。
    1. 役割バインディングを、ある名前空間から別の名前空間にコピーします。
        ```
        kubectl get rolebinding <role_binding_name> -o yaml | sed 's/<namespace_1>/<namespace_2>/g' | kubectl -n <namespace_2> create -f -
        ```
        {: pre}

        例えば、`custom-role` 役割バインディングを `default` 名前空間から `testns` 名前空間にコピーするには、以下のようにします。
        ```
        kubectl get rolebinding custom-role -o yaml | sed 's/default/testns/g' | kubectl -n testns create -f -
        ```
        {: pre}

    2. 役割バインディングがコピーされたことを確認します。 {{site.data.keyword.Bluemix_notm}} IAM アクセス・グループを役割バインディングに追加した場合は、アクセス・グループ ID としてではなく、そのグループの各ユーザーが個別に追加されます。
        ```
        kubectl get rolebinding -n <namespace_2>
        ```
        {: pre}

カスタム Kubernetes RBAC 役割またはクラスター役割を作成してバインドしたので、ユーザーをフォローアップします。 役割を介して実行許可を持っているアクション (ポッドを削除するなど) をテストするようにユーザーに依頼してください。

### クラスター役割の集約による既存の許可の拡張 
{: #rbac_aggregate}

クラスター役割を他のクラスター役割と集約または結合することにより、ユーザーの既存の許可を拡張できます。ユーザーに {{site.data.keyword.Bluemix_notm}} サービス役割を割り当てると、そのユーザーは[対応する Kubernetes RBAC クラスター役割](/docs/containers?topic=containers-access_reference#service)に追加されます。ただし、特定のユーザーが追加の操作を実行できるようにすることもできます。
{: shortdesc}

例えば、名前空間に有効範囲が設定された `admin` クラスター役割を持つユーザーは、`kubectl top pods` コマンドを使用して、名前空間内のすべてのポッドのポッド・メトリックを表示することはできません。`admin` クラスター役割のユーザーに `top pods` コマンドの実行が許可されるように、クラスター役割を集約できます。詳しくは、[Kubernetes の資料 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン") を参照してください](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#aggregated-clusterroles)。

**デフォルトのクラスター役割の許可を拡張できる一般的な操作は何ですか?**<br>
[デフォルトの RBAC クラスター役割ごとに許可される操作](/docs/containers?topic=containers-access_reference#rbac_ref)を確認して、ユーザーが実行できる操作を十分に理解してから、許可された操作と実行できるようにする操作を比較します。

同じクラスター役割のユーザーに、同じタイプの操作で以下のようなエラーが発生した場合、クラスター役割を拡張してこの操作を含めることができます。

```
Error from server (Forbidden): pods.metrics.k8s.io is forbidden: User "IAM#myname@example.com" cannot list resource "pods" in API group "metrics.k8s.io" in the namespace "mynamespace"
```
{: screen}

**クラスター役割を集約するには**、以下のようにします。

開始前に、以下のことを行います。 [アカウントにログインします。 該当する場合は、適切なリソース・グループをターゲットにします。 クラスターのコンテキストを設定します。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  クラスター役割の YAML ファイルを作成します。`labels` セクションで、許可を集約する既存のクラスター役割を指定します。以下の例では、事前定義された `admin` クラスター役割を拡張して、ユーザーに `kubectl top pods` の実行を許可しています。その他の例については、[Kubernetes の資料 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン") を参照してください](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#aggregated-clusterroles)。
    ```
    apiVersion: rbac.authorization.k8s.io/v1
    kind: ClusterRole
    metadata:
      name: view-pod-metrics
      labels:
        rbac.authorization.k8s.io/aggregate-to-admin: "true"
    rules:
    - apiGroups:
      - "metrics.k8s.io"
      resources:
      - pods
      verbs:
      - list
    ```
    {: codeblock}
    
    <table>
    <caption>YAML の構成要素について</caption>
      <thead>
        <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> YAML の構成要素について</th>
      </thead>
      <tbody>
        <tr>
          <td><code>metadata.name</code></td>
          <td>クラスター役割の名前を入力します。`view`、`edit`、`admin`、および `cluster-admin` の事前定義されたクラスター役割名は使用**しないでください**。</td>
        </tr>
        <tr>
          <td><code>metadata.labels</code></td>
          <td>集約するクラスター役割に一致するラベルを `rbac.authorization.k8s.io/aggregate-to-<cluster_role>: "true"` の形式で追加します。事前定義されたクラスター役割のラベルは、以下のとおりです。<ul>
          <li>IAM の**管理者**のサービス役割 (1 つの名前空間が対象): `rbac.authorization.k8s.io/aggregate-to-admin: "true"`</li>
          <li>IAM の**ライター**のサービス役割: `rbac.authorization.k8s.io/aggregate-to-edit: "true"`</li>
          <li>IAM の**リーダー**のサービス役割: `rbac.authorization.k8s.io/aggregate-to-view: "true"`</li></ul></td>
        </tr>
        <tr>
          <td><code>rules.apiGroups</code></td>
          <td>ユーザーに対話を許可する Kubernetes [API グループ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/reference/using-api/api-overview/#api-groups) (`"apps"`、`"batch"`、`"extensions"` など) を指定します。 REST パス `api/v1` にあるコア API グループに対するアクセス権限については、`[""]` のようにグループをブランクにします。</td>
        </tr>
        <tr>
          <td><code>rules.resources</code></td>
          <td>アクセス権限を付与する Kubernetes [リソース・タイプ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/reference/kubectl/cheatsheet/) (`"daemonsets"`、`"deployments"`、`"events"`、`"ingresses"` など) を指定します。</td>
        </tr>
        <tr>
          <td><code>rules.verbs</code></td>
          <td>ユーザーに実行を許可する [アクション ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubectl.docs.kubernetes.io/) のタイプ (`"get"`、`"list"`、`"describe"`、`"create"`、`"delete"` など) を指定します。</td>
        </tr>
      </tbody>
    </table>
2.  クラスター内にクラスター役割を作成します。`admin` クラスター役割に対する役割バインディングを持つユーザーは、`view-pod-metrics` クラスター役割の追加の許可を持つようになりました。
    ```
    kubectl apply -f <cluster_role_file.yaml>
    ```
    {: pre}
3.  `admin` クラスター役割を持つユーザーをフォローアップします。[クラスター構成をリフレッシュ](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)して、`kubectl top pods` などのアクションをテストするように要求してください。


<br />


## インフラストラクチャー許可のカスタマイズ
{: #infra_access}

API キーを設定している管理者またはインフラストラクチャー資格情報が設定されている管理者にインフラストラクチャー役割**スーパーユーザー**を割り当てると、API キーまたは資格情報がアカウント内の他のユーザーによってインフラストラクチャー・アクションを実行するために共有されます。 適切な [{{site.data.keyword.Bluemix_notm}}IAM プラットフォーム役割](#platform)を割り当てることによって、ユーザーが実行できるインフラストラクチャー・アクションを制御できます。 ユーザーの IBM Cloud インフラストラクチャー (SoftLayer) の許可を編集する必要はありません。
{: shortdesc}

コンプライアンス、セキュリティー、または課金の理由から、API キーを設定しているユーザーまたは `ibmcloud ks credential-set` コマンドを使用して資格情報が設定されているユーザーにインフラストラクチャー役割**スーパーユーザー**を付与しない場合があります。 ただし、このユーザーに**スーパーユーザー**役割がない場合、インフラストラクチャー関連のアクション、例えば、クラスターの作成やワーカー・ノードの再ロードは失敗する可能性があります。 {{site.data.keyword.Bluemix_notm}} IAM プラットフォーム役割を使用してユーザーのインフラストラクチャー・アクセスを制御する代わりに、ユーザーに特定の IBM Cloud インフラストラクチャー (SoftLayer) 許可を設定する必要があります。

例えば、アカウントが VRF 対応でない場合、IBM Cloud インフラストラクチャー (SoftLayer) アカウント所有者は VLAN スパンニングをオンにする必要があります。アカウント所有者は、ユーザーが VLAN スパンニングを有効にできるように、**「ネットワーク」>「ネットワーク VLAN スパンニングの管理 (Manage Network VLAN Spanning)」**許可をユーザーに割り当てることもできます。 詳しくは、[VLAN spanning for cross-VLAN communication](/docs/containers?topic=containers-subnets#basics_segmentation) を参照してください。

<p class="tip">`ibmcloud ks credential-set` コマンドを使用して既にインフラストラクチャー資格情報を設定していますか?[`ibmcloud ks infra-permissions-get --region <region>` コマンド](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#infra_permissions_get)を実行して、資格情報に、推奨または必須のインフラストラクチャー許可が欠落しているかどうかを確認できます。出力で、推奨または必須の許可がない場合は、このセクションのステップを使用して必要なアクセス権限を割り当てることができます。</p>

始める前に、アカウント所有者であること、または**スーパーユーザー**およびすべてのデバイス・アクセス権限を所有していることを確認してください。 所有していないアクセス権限をユーザーに付与することはできません。

1. [{{site.data.keyword.Bluemix_notm}} コンソール ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com) にログインします。 メニュー・バーから、**「管理」 > 「アクセス (IAM)」**を選択します。

2. **「ユーザー」**ページをクリックし、許可を設定するユーザーの名前をクリックします。

3. **「クラシック・インフラストラクチャー」**タブをクリックし、**「許可」**タブをクリックします。

4. ユーザーのアクセスをカスタマイズします。 ユーザーが必要とする許可は、ユーザーが使用する必要があるインフラストラクチャー・リソースによって異なります。 アクセス権限を割り当てる 2 つのオプションがあります。
    * **「許可セット」**ドロップダウン・リストを使用して、以下の事前定義役割のいずれかを割り当てます。 役割を選択した後、**「設定」**をクリックします。
        * **「表示のみ」**は、インフラストラクチャー詳細を表示する許可のみをユーザーに付与します。
        * **「基本ユーザー」**は、一部の (すべてではない) インフラストラクチャー許可をユーザーに付与します。
        * **「スーパーユーザー」**は、すべてのインフラストラクチャー許可をユーザーに付与します。
    * 各カテゴリーで個別に許可を選択します。 {{site.data.keyword.containerlong_notm}} の一般的なタスクの実行に必要な許可を確認するには、[ユーザー・アクセス許可](/docs/containers?topic=containers-access_reference#infra)を参照してください。

5.  **「保存」**をクリックします。

6.  **「デバイス」**タブで、アクセス権限を付与するデバイスを選択します。

    * **「タイプの選択」**グループで、すべてのベアメタル・サーバー、専用サーバー、および仮想サーバーへのアクセスを付与できるので、ユーザーはすべての[ワーカー・ノードのマシン・タイプ](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)を操作できます。
    * **「将来のアクセスの有効化」**グループで、将来のすべてのベアメタル・サーバー、専用サーバー、および仮想サーバーへのアクセスをユーザーに付与できます。
    * デバイスの表で、適切なデバイスが選択されていることを確認します。

7. 変更を保存するには、**「設定」**をクリックします。

8. **重要**: ユーザーがクラスターとワーカー・ノードを管理できるように許可を割り当てる場合は、サポート・ケースを処理するためのユーザー・アクセス権限を割り当てる必要があります。
  1. **「アクセス・ポリシー」**タブをクリックし、**「アクセス権限の割り当て」**をクリックします。
  2. **「アカウント管理サービスへのアクセス権限の割り当て」**カードをクリックします。
  3. **「サポート・センター」**を選択します。
  4. ユーザーがサポート・ケースを表示、追加、および編集できるようにするには、**「管理者」**を選択します。
  5. **「割り当て」**をクリックします。

アクセス権をダウングレードしていますか? このアクションは完了するまで数分かかる場合があります。
{: tip}

<br />


## ユーザー許可の削除
{: #removing}

特定のアクセス許可がユーザーに不要になった場合や、ユーザーが退職した場合は、{{site.data.keyword.Bluemix_notm}} アカウント所有者がそのユーザーの許可を削除できます。
{: shortdesc}

ユーザーの特定のアクセス許可を削除したり、アカウントからユーザーを完全に削除したりする前に、API キーの設定や `ibmcloud ks credential-set` コマンドでそのユーザーのインフラストラクチャー資格情報が使用されていないことを確認してください。 そうしないと、アカウント内の他のユーザーが IBM Cloud インフラストラクチャー (SoftLayer) ポータルにアクセスできなくなり、インフラストラクチャー関連のコマンドが失敗する可能性があります。
{: important}

1. CLI コンテキストのターゲットを、クラスターが存在する地域とリソース・グループに設定します。
    ```
    ibmcloud target -g <resource_group_name> -r <region>
    ```
    {: pre}

2. その地域とリソース・グループに設定されている API キーまたはインフラストラクチャー資格情報の所有者を確認します。
    * [API キーを使用して IBM Cloud インフラストラクチャー (SoftLayer) ポートフォリオにアクセスする](#default_account)場合は、以下のようにします。
        ```
        ibmcloud ks api-key-info --cluster <cluster_name_or_id>
        ```
        {: pre}
    * [インフラストラクチャー資格情報を設定して IBM Cloud インフラストラクチャー (SoftLayer) ポートフォリオにアクセスする](#credentials)場合は、以下のようにします。
        ```
        ibmcloud ks credential-get --region <region>
        ```
        {: pre}

3. 該当するユーザーのユーザー名が返された場合は、別のユーザーの資格情報を使用して API キーまたはインフラストラクチャー資格情報を設定します。

  アカウント所有者で API キーを設定しない場合、または、アカウント所有者のインフラストラクチャー資格情報を設定しない場合は、[API キーを設定するユーザー、または、設定する資格情報の所有者ユーザーに正しい許可があることを確認](#owner_permissions)してください。
  {: note}

    * API キーをリセットするには、以下のようにします。
        ```
        ibmcloud ks api-key-reset --region <region>
        ```
        {: pre}
    * インフラストラクチャー資格情報をリセットするには、以下のようにします。
        ```
        ibmcloud ks credential-set --infrastructure-username <infrastructure_API_username> --infrastructure-api-key <infrastructure_API_authentication_key> --region <region>
        ```
        {: pre}

4. クラスターが存在するリソース・グループと地域の組み合わせごとに、これらの手順を繰り返します。

### アカウントからのユーザーの削除
{: #remove_user}

アカウント内のユーザーが退職する場合は、そのユーザーの許可を慎重に削除して、クラスターやその他のリソースが孤立しないようにする必要があります。 その後、{{site.data.keyword.Bluemix_notm}} アカウントからユーザーを削除できます。
{: shortdesc}

開始前に、以下のことを行います。
- [API キーの設定や `ibmcloud ks credential-set` コマンドで該当ユーザーのインフラストラクチャー資格情報が使用されていないことを確認します](#removing)。
- {{site.data.keyword.Bluemix_notm}} アカウント内に、ユーザーがプロビジョンした可能性がある他のサービス・インスタンスがある場合は、それらのサービスの資料を参照し、アカウントからユーザーを削除する前に実行すべき手順がないか確認します。

ユーザーが退職する前に、{{site.data.keyword.Bluemix_notm}} アカウント所有者が以下の手順を実行して、{{site.data.keyword.containerlong_notm}} に破壊的な変化が生じないようにする必要があります。

1. ユーザーが作成したクラスターを調べます。
    1.  [{{site.data.keyword.containerlong_notm}} コンソール ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/kubernetes/clusters) にログインします。
    2.  テーブルから、該当のクラスターを選択します。
    3.  **「概要」**タブで、**「所有者」**フィールドを探します。

2. ユーザーが作成したクラスターごとに、以下の手順に従います。
    1. ユーザーがクラスターをプロビジョンするためにどのインフラストラクチャー・アカウントが使用したかを確認します。
        1.  **「ワーカー・ノード」**タブでワーカー・ノードを選択し、その **ID** を書き留めます。
        2.  メニュー ![メニュー・アイコン](../icons/icon_hamburger.svg "メニュー・アイコン") を開き、**「クラシック・インフラストラクチャー」**をクリックします。
        3.  インフラストラクチャーのナビゲーション・ペインから**「デバイス」>「デバイス・リスト」**をクリックします。
        4.  先ほど書き留めたワーカー・ノードの ID を検索します。
        5.  このワーカー・ノード ID が見つからない場合、そのワーカー・ノードはこのインフラストラクチャー・アカウントにプロビジョンされていません。 別のインフラストラクチャー・アカウントに切り替えて、もう一度やり直します。
    2. ユーザーがクラスターのプロビジョンに使用したインフラストラクチャー・アカウントが、ユーザーの退職後にどうなるかを調べます。
        * ユーザーがインフラストラクチャー・アカウントを所有していない場合は、他のユーザーはこのインフラストラクチャー・アカウントにアクセスできます。ユーザーが退職した後も変わりません。 アカウント内のこれらのクラスターを引き続き操作できます。 1 人以上の他のユーザーに、クラスターに対する[**管理者**のプラットフォーム役割](#platform)があることを確認します。
        * ユーザーがインフラストラクチャー・アカウントを所有している場合は、ユーザーが退職するとインフラストラクチャー・アカウントが削除されます。 それらのクラスターは操作できなくなります。 クラスターが孤立しないようにするには、ユーザー自身が退職する前にクラスターを削除する必要があります。 ユーザーが退職したのにクラスターが削除されていない場合は、`ibmcloud ks credential-set` コマンドを使用して、インフラストラクチャー資格情報を、クラスター・ワーカー・ノードがプロビジョンされたアカウントに変更し、クラスターを削除します。 詳しくは、[孤立クラスターのインフラストラクチャーを変更または削除できない](/docs/containers?topic=containers-cs_troubleshoot_clusters#orphaned)を参照してください。

3. {{site.data.keyword.Bluemix_notm}} アカウントからユーザーを削除します。
    1. メニュー・バーから、**「管理」 > 「アクセス (IAM)」**を選択します。 次に**「ユーザー」**ページをクリックします。
    2. ユーザーのユーザー名をクリックします。
    3. ユーザーのテーブル・エントリーで、**「アクション」メニュー** ![「アクション」メニュー・アイコン](../icons/action-menu-icon.svg "「アクション」メニュー・アイコン") **>「ユーザーの削除」**をクリックします。 ユーザーを削除すると、そのユーザーに割り当てられていた {{site.data.keyword.Bluemix_notm}} IAM プラットフォーム役割、Cloud Foundry 役割、IBM Cloud インフラストラクチャー (SoftLayer) 役割が自動的に削除されます。

4.  {{site.data.keyword.Bluemix_notm}} IAM プラットフォーム許可が削除されると、関連付けられている事前定義 RBAC 役割からもユーザーの許可が自動的に削除されます。 しかし、カスタムの RBAC 役割またはクラスター役割を作成していた場合は、[それらの RBAC 役割バインディングまたはクラスター役割バインディングからユーザーを削除](#remove_custom_rbac)してください。<p class="note">{{site.data.keyword.Bluemix_notm}} IAM 許可の削除プロセスは非同期で行われるため、完了するまでしばらく時間がかかる可能性があります。</p>


### 特定の許可の削除
{: #remove_permissions}

ユーザーの特定の許可を削除する必要がある場合は、そのユーザーに割り当てられているアクセス・ポリシーを個々に削除できます。
{: shortdesc}

始めに、[API キーの設定や `ibmcloud ks credential-set` コマンドで該当ユーザーのインフラストラクチャー資格情報が使用されていないことを確認してください](#removing)。 その後に以下を削除できます。
* [アクセス・グループ内のユーザー](#remove_access_group)
* [ユーザーの {{site.data.keyword.Bluemix_notm}} IAM プラットフォーム許可や関連付けられている RBAC 許可](#remove_iam_rbac)
* [ユーザーのカスタム RBAC 許可](#remove_custom_rbac)
* [ユーザーの Cloud Foundry 許可](#remove_cloud_foundry)
* [ユーザーのインフラストラクチャー許可](#remove_infra)

#### アクセス・グループからユーザーを削除します
{: #remove_access_group}

1. [{{site.data.keyword.Bluemix_notm}} コンソール ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/) にログインします。メニュー・バーから、**「管理」>「アクセス (IAM)」**を選択します。
2. **「ユーザー」**ページをクリックし、許可を削除するユーザーの名前をクリックします。
3. **「アクセス・グループ」**タブをクリックします。
4. アクセス・グループのテーブル・エントリーで、**「アクション」メニュー **![「アクション」メニュー・アイコン](../icons/action-menu-icon.svg "「アクション」メニュー・アイコン") **>「ユーザーの削除」**をクリックします。 ユーザーが削除されると、アクセス・グループに割り当てられていたすべての役割がそのユーザーから削除されます。

#### {{site.data.keyword.Bluemix_notm}} IAM プラットフォーム許可や関連付けられている事前定義 RBAC 許可の削除
{: #remove_iam_rbac}

1. [{{site.data.keyword.Bluemix_notm}} コンソール ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/) にログインします。 メニュー・バーから、**「管理」 > 「アクセス (IAM)」**を選択します。
2. **「ユーザー」**ページをクリックし、許可を削除するユーザーの名前をクリックします。
3. ユーザーのテーブル・エントリーで、**「アクション」メニュー**![「アクション」メニュー・アイコン](../icons/action-menu-icon.svg "「アクション」メニュー・アイコン")**>「ユーザーの削除」**をクリックします。
5. {{site.data.keyword.Bluemix_notm}} IAM プラットフォーム許可が削除されると、関連付けられている事前定義 RBAC 役割からもユーザーの許可が自動的に削除されます。 これらの変更を適用して RBAC 役割を更新するには、`ibmcloud ks cluster-config` を実行します。 しかし、[カスタムの RBAC 役割またはクラスター役割](#rbac)を作成していた場合は、それらの RBAC 役割バインディングまたはクラスター役割バインディングに関する `.yaml` ファイルからユーザーを削除しなければなりません。 以下のカスタム RBAC 許可を削除する手順を参照してください。

#### カスタム RBAC 許可の削除
{: #remove_custom_rbac}

不要になったカスタム RBAC 許可は削除できます。
{: shortdesc}

1. 作成した役割バインディングまたはクラスター役割バインディングに関する `.yaml` ファイルを開きます。
2. `subjects` セクションで、該当ユーザーのセクションを削除します。
3. ファイルを保存します。
4. クラスターの役割バインディングまたはクラスター役割バインディングのリソースの変更を適用します。
    ```
    kubectl apply -f my_role_binding.yaml
    ```
    {: pre}

#### Cloud Foundry 許可の削除
{: #remove_cloud_foundry}

ユーザーの Cloud Foundry 許可をすべて削除するには、そのユーザーの組織の役割を削除します。 ユーザーの能力 (クラスター内でサービスをバインドする能力など) のみを削除しようとしている場合は、そのユーザーのスペース役割のみを削除します。
{: shortdesc}

1. [{{site.data.keyword.Bluemix_notm}} コンソール](https://cloud.ibm.com/) にログインします。 メニュー・バーから、**「管理」 > 「アクセス (IAM)」**を選択します。
2. **「ユーザー」**ページをクリックし、許可を削除するユーザーの名前をクリックします。
3. **「Cloud Foundry アクセス権限」**タブをクリックします。
    * ユーザーのスペース役割を削除するには、以下のようにします。
        1. スペースが含まれている組織のテーブル・エントリーを展開します。
        2. スペース役割のテーブル・エントリーで、アクション・メニューをクリックし、**「スペースの役割の編集」**を選択します。
        3. 「閉じる」ボタンをクリックして、役割を削除します。
        4. すべてのスペース役割を削除するには、ドロップダウン・リストで**「スペースの役割なし」**を選択します。
        5. **「役割の保存」**をクリックします。
    * ユーザーの組織の役割を削除するには、以下のようにします。
        1. 組織の役割のテーブル・エントリーで、アクション・メニューをクリックし、**「組織の役割の編集」**を選択します。
        3. 「閉じる」ボタンをクリックして、役割を削除します。
        4. すべての組織の役割を削除するには、ドロップダウン・リストで**「組織の役割なし」**を選択します。
        5. **「役割の保存」**をクリックします。

#### IBM Cloud インフラストラクチャー (SoftLayer) 許可の削除
{: #remove_infra}

{{site.data.keyword.Bluemix_notm}} コンソールを使用して、ユーザーの IBM Cloud インフラストラクチャー (SoftLayer) 許可を削除できます。
{: shortdesc}

1. [{{site.data.keyword.Bluemix_notm}} コンソール ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/) にログインします。 メニュー・バーから、**「管理」 > 「アクセス (IAM)」**を選択します。
2. **「ユーザー」**ページをクリックし、許可を削除するユーザーの名前をクリックします。
4. **「クラシック・インフラストラクチャー」**タブをクリックし、**「許可」、「デバイス」、または「VPN サブネット」**タブをクリックします。
5. 各タブで、特定の許可を選択解除します。
6. 変更を保存するには、**「設定」**および**「保存」**をクリックします。 数分後に許可がダウングレードされます。
