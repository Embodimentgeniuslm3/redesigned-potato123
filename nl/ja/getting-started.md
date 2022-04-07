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


# 入門チュートリアル
{: #getting-started}

Kubernetes クラスターで稼動する Docker コンテナーに可用性の高いアプリをデプロイすることによって、{{site.data.keyword.containerlong}} ですぐに作業を始めることができます。
{:shortdesc}

コンテナーは、環境間でアプリをシームレスに移動できるようにアプリとそのすべての従属物をパッケージ化するための標準的な手段です。 仮想マシンとは異なり、コンテナーはオペレーティング・システムをバンドルしません。 アプリのコード、ランタイム、システム・ツール、ライブラリー、設定値のみがコンテナー内にパッケージされます。 コンテナーは、仮想マシンより軽量で移植しやすく、効率的です。

## クラスターの概説
{: #clusters_gs}

コンテナーにアプリをデプロイするときには、 その前に、 まず Kubernetes クラスターを作成してください。 Kubernetes はコンテナー用のオーケストレーション・ツールです。 Kubernetes を使用すると、開発者はクラスターの高度な機能や柔軟性を活用して、可用性の高いアプリをすぐにデプロイできます。
{:shortdesc}

ところで、クラスターとは一体何でしょうか。 クラスターとは、アプリの高可用性を維持する一連のリソース、ワーカー・ノード、ネットワーク、ストレージ・デバイスのことです。 クラスターを作成したら、アプリをコンテナーにデプロイできます。

開始する前に、自分に合った [{{site.data.keyword.Bluemix_notm}} アカウント](https://cloud.ibm.com/registration)・タイプを取得します。
* **有料 (従量課金またはサブスクリプション)**: 無料のクラスターを作成できます。 IBM Cloud インフラストラクチャー (SoftLayer) リソースをプロビジョンして、標準クラスターで作成して使用することもできます。
* **ライト**: 無料クラスターも標準クラスターも作成できません。 有料アカウントに[アカウントをアップグレード](/docs/account?topic=account-accountfaqs#changeacct)してください。

フリー・クラスターを作成するには、以下のようにします。

1.  [{{site.data.keyword.Bluemix_notm}} **カタログ** ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/catalog?category=containers) で、**「{{site.data.keyword.containershort_notm}}」**を選択し、**「作成」**をクリックします。 クラスター構成ページが開きます。 デフォルトでは、**「フリー・クラスター (Free cluster)」**が選択されています。

2.  固有の名前をクラスターに付けます。

3.  **「クラスターの作成」**をクリックします。 ワーカー・ノードを 1 つ含むワーカー・プールが作成されます。 ワーカー・ノードがプロビジョンするまでに数分かかることがありますが、「**ワーカー・ノード**」タブで進行状況を確認できます。 状況が `Ready` になったら、クラスターを使い始めることができます。

<br>

おつかれさまでした。 これで、1 つ目の Kubernetes クラスターが作成されました。 フリー・クラスターの詳細は次のとおりです。

*   **マシン・タイプ**: フリー・クラスターには、ワーカー・プールにグループ化された仮想ワーカー・ノードが 1 つ含まれています。アプリでは CPU を 2 つ、メモリーを 4 GB、100 GB SAN ディスクを 1 台使用できます。 標準クラスターの作成時には、物理 (ベア・メタル) マシンと仮想マシンのどちらにするかや、さまざまなマシン・サイズを選択できます。
*   **管理マスター**: ワーカー･ノードは、{{site.data.keyword.IBM_notm}} が所有する可用性の高い専用のマスターで一元的にモニターされ、管理されます。Kubernetes マスターは、クラスター内のすべての Kubernetes リソースを制御および管理します。 作業担当者はこのマスターの管理のことは気にせずに、ワーカー・ノードとそのワーカー・ノードにデプロイされるアプリに集中できます。
*   **インフラストラクチャー・リソース**: VLAN や IP アドレスなどの、クラスターの実行に必要なリソースは、{{site.data.keyword.IBM_notm}} 所有の IBM Cloud インフラストラクチャー (SoftLayer) アカウントで管理されます。 標準クラスターの作成時は、独自の IBM Cloud インフラストラクチャー (SoftLayer) アカウントでこれらのリソースを管理します。 標準クラスターの作成時に、これらのリソースおよび[必要なアクセス権](/docs/containers?topic=containers-users#infra_access)について確認できます。
*   **その他のオプション**: フリー・クラスターは選択した地域内にデプロイされますが、ゾーンは選択できません。 ゾーン、ネットワーキング、永続ストレージを制御するには、標準クラスターを作成してください。 [フリー・クラスターと標準クラスターの利点について詳しくは、こちらを参照してください](/docs/containers?topic=containers-cs_ov#cluster_types)。

<br>

**次の作業**</br>
期限が切れる前に、フリー・クラスターでさまざまな機能を試してください。

* [最初の {{site.data.keyword.containerlong_notm}} チュートリアル](/docs/containers?topic=containers-cs_cluster_tutorial#cs_cluster_tutorial)で、Kubernetes クラスターの作成、CLI のインストールまたは Kubernetes Terminal の使用、専用レジストリーの作成、クラスター環境のセットアップ、クラスターへのサービスの追加について理解します。
* そのまま、[2 つ目の{{site.data.keyword.containerlong_notm}} チュートリアル](/docs/containers?topic=containers-cs_apps_tutorial#cs_apps_tutorial)も行い、クラスターにアプリをデプロイする方法について学習します。
* 可用性を高めるために、複数のノードを使用する[標準クラスターを作成します](/docs/containers?topic=containers-clusters#clusters_ui)。


