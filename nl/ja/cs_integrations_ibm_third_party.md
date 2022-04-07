---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-07"

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
{:preview: .preview}


# IBM Cloud サービスおよびサード・パーティー統合
{: #ibm-3rd-party-integrations}

{{site.data.keyword.Bluemix_notm}} のプラットフォームとインフラストラクチャー・サービス、およびその他のサード・パーティー統合を使用して、さらに機能をクラスターに追加できます。
{: shortdesc}

## IBM Cloud サービス
{: #ibm-cloud-services}

{{site.data.keyword.Bluemix_notm}} のプラットフォームとインフラストラクチャー・サービスが {{site.data.keyword.containerlong_notm}} とどのように統合されているか、およびクラスターでのこれらの使用方法については、以下の情報を参照してください。
{: shortdesc}

### IBM Cloud プラットフォーム・サービス
{: #platform-services}

サービス・キーをサポートするすべての {{site.data.keyword.Bluemix_notm}} プラットフォーム・サービスは、{{site.data.keyword.containerlong_notm}} [サービス・バインディング](/docs/containers?topic=containers-service-binding)を使用して統合できます。
{: shortdesc}

サービス・バインディングは、{{site.data.keyword.Bluemix_notm}} サービスのサービス資格情報を作成して、これらの資格情報をクラスター内の Kubernetes シークレットに保管するための迅速な方法です。 この Kubernetes シークレットは、データを保護するために、etcd 内で自動的に暗号化されます。 アプリは、シークレット内の資格情報を使用して {{site.data.keyword.Bluemix_notm}} サービス・インスタンスにアクセスできます。

サービス・キーをサポートしていないサービスでは、通常、アプリで直接使用できる API が提供されます。

一般的な {{site.data.keyword.Bluemix_notm}} サービスの概要については、[一般的な統合](/docs/containers?topic=containers-supported_integrations#popular_services)を参照してください。

### IBM Cloud インフラストラクチャー・サービス
{: #infrastructure-services}

{{site.data.keyword.containerlong_notm}} では、{{site.data.keyword.Bluemix_notm}} インフラストラクチャーに基づく Kubernetes クラスターを作成できるため、仮想サーバー、ベアメタル・サーバー、VLAN などの一部のインフラストラクチャー・サービスは、{{site.data.keyword.containerlong_notm}} に完全に統合されます。{{site.data.keyword.containerlong_notm}} の API、CLI、またはコンソールを使用して、これらのサービス・インスタンスを作成および操作します。
{: shortdesc}

{{site.data.keyword.Bluemix_notm}} File Storage、{{site.data.keyword.Bluemix_notm}} Block Storage、{{site.data.keyword.cos_full}} などのサポートされる永続ストレージ・ソリューションは、Kubernetes flex ドライバーとして統合され、[Helm チャート](/docs/containers?topic=containers-helm)を使用してセットアップできます。Helm チャートを使用すると、Kubernetes ストレージ・クラス、ストレージ・プロバイダー、およびストレージ・ドライバーがクラスター内に自動的にセットアップされます。ストレージ・クラスを使用すると、永続ボリューム請求 (PVC) を使用して永続ストレージをプロビジョンできます。

クラスター・ネットワークを保護したり、オンプレミス・データ・センターに接続したりするために、以下のいずれかのオプションを構成できます。
- [strongSwan IPSec VPN サービス](/docs/containers?topic=containers-vpn#vpn-setup)
- [{{site.data.keyword.BluDirectLink}}](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link)
- [Virtual Router Appliance (VRA)](/docs/containers?topic=containers-vpn#vyatta)
- [Fortigate Security Appliance (FSA)](/docs/services/vmwaresolutions/services?topic=vmware-solutions-fsa_considerations)

## Kubernetes コミュニティーとオープン・ソースの統合
{: #kube-community-tools}

{{site.data.keyword.containerlong_notm}} で作成する標準クラスターを所有しているため、サード・パーティー・ソリューションをインストールしてさらに機能をクラスターに追加することを選択できます。
{: shortdesc}

Knative、Istio、LogDNA、Sysdig、Portworx などの一部のオープン・ソース・テクノロジーは、IBM によってテストされ、管理対象アドオン、Helm チャート、または IBM とのパートナーシップによりサービス・プロバイダーによって運用される {{site.data.keyword.Bluemix_notm}} サービスとして提供されます。これらのオープン・ソース・ツールは、{{site.data.keyword.Bluemix_notm}} の課金とサポートのシステムに完全に統合されています。

クラスター内にその他のオープン・ソース・ツールをインストールできますが、これらのツールは、{{site.data.keyword.containerlong_notm}} で機能するように管理、サポート、または検証されない場合があります。

### パートナーシップで運用される統合
{: #open-source-partners}

{{site.data.keyword.containerlong_notm}} パートナーおよび提供される各ソリューションの利点について詳しくは、[{{site.data.keyword.containerlong_notm}} パートナー](/docs/containers?topic=containers-service-partners)を参照してください。

### 管理対象アドオン
{: #cluster-add-ons}
{{site.data.keyword.containerlong_notm}} では、[Knative](/docs/containers?topic=containers-serverless-apps-knative) や [Istio](/docs/containers?topic=containers-istio) などの一般的なオープン・ソース統合が、[管理対象アドオン](/docs/containers?topic=containers-managed-addons)を使用して統合されます。管理対象アドオンは、IBM によってテストされ、{{site.data.keyword.containerlong_notm}} での使用が承認されたオープン・ソース・ツールを、クラスターに簡単にインストールする方法です。

管理対象アドオンは、{{site.data.keyword.Bluemix_notm}} のサポート組織に完全に統合されています。 管理対象アドオンの使用に関する質問や問題がある場合は、いずれかの {{site.data.keyword.containerlong_notm}} サポート・チャネルを使用できます。詳しくは、[ヘルプとサポートの取得](/docs/containers?topic=containers-cs_troubleshoot_clusters#clusters_getting_help)を参照してください。

クラスターに追加するツールについてコストが生じる場合、これらのコストは自動的に統合されて、月単位の {{site.data.keyword.Bluemix_notm}} に関する課金の一部としてリストされます。課金サイクルは、クラスター内でそのアドオンを有効にしたタイミングに応じて {{site.data.keyword.Bluemix_notm}} 側で決定されます。

### 他のサード・パーティー統合
{: #kube-community-helm}

Kubernetes と統合する任意のサード・パーティーのオープン・ソース・ツールをインストールできます。例えば、Kubernetes コミュニティーでは、特定の Helm チャート `stable` または `incubator` が指定されます。これらのチャートまたはツールは、{{site.data.keyword.containerlong_notm}} で機能することが確認されていないことに注意してください。ツールにライセンスが必要な場合は、ツールを使用する前にライセンスを購入する必要があります。Kubernetes コミュニティーから入手できる Helm チャートの概要については、[Helm チャート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/kubernetes/solutions/helm-charts) カタログの `kubernetes` リポジトリーおよび `kubernetes-incubator` リポジトリーを参照してください。
{: shortdesc}

サード・パーティーのオープン・ソース統合を使用して生じるすべてのコストは、月単位の {{site.data.keyword.Bluemix_notm}} 課金には含まれません。

Kubernetes コミュニティーからサード・パーティーのオープン・ソース統合または Helm チャートをインストールすると、デフォルトのクラスター構成が変更され、クラスターがサポートされない状態になる可能性があります。これらのツールのいずれかを使用して問題が発生した場合は、Kubernetes コミュニティーまたはサービス・プロバイダーに直接お問い合わせください。
{: important}
