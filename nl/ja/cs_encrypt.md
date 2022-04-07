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



# クラスターでの機密情報の保護
{: #encryption}

データ保全性を確保し、無許可ユーザーにデータをさらさないために、クラスターの機密情報を保護してください。
{: shortdesc}

クラスター内には、さまざまなレベルの機密データが作成されます。それぞれに適切な保護が必要になります。
- **クラスター・レベル:** クラスター構成データが、Kubernetes マスターの etcd コンポーネントに保管されます。 etcd のデータは Kubernetes マスターのローカル・ディスクに保管され、{{site.data.keyword.cos_full_notm}} にバックアップされます。 {{site.data.keyword.cos_full_notm}} に転送中のデータも保存されたデータも暗号化されています。 クラスターの [{{site.data.keyword.keymanagementservicelong_notm}} 暗号化を有効にする](/docs/containers?topic=containers-encryption#encryption)と、Kubernetes マスターのローカル・ディスク上の etcd データの暗号化を有効にできます。 以前のバージョンの Kubernetes を実行するクラスターでは、etcd データは IBM 管理の暗号化ディスクに保管され、毎日バックアップされます。
- **アプリ・レベル:** アプリをデプロイする際に、資格情報や鍵などの機密情報を YAML 構成ファイルや構成マップ、スクリプトに保管しないでください。 代わりに、[Kubernetes シークレット ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/concepts/configuration/secret/) を使用します。 [Kubernetes シークレット内でデータを暗号化して](#keyprotect)、許可されていないユーザーがクラスターの機密情報にアクセスできないようにすることもできます。

クラスターの保護について詳しくは、[{{site.data.keyword.containerlong_notm}} のセキュリティー](/docs/containers?topic=containers-security#security)を参照してください。

<img src="images/cs_encrypt_ov.png" width="700" alt="クラスター暗号化の概要" style="width:700px; border-style: none"/>

_図: クラスター内のデータ暗号化の概要_

1.  **etcd**: etcd は、オブジェクト構成 `.yaml` ファイルやシークレットなどの Kubernetes リソースのデータを保管するマスターのコンポーネントです。 etcd のデータは Kubernetes マスターのローカル・ディスクに保管され、{{site.data.keyword.cos_full_notm}} にバックアップされます。 {{site.data.keyword.cos_full_notm}} に転送中のデータも保存されたデータも暗号化されています。 クラスターの [{{site.data.keyword.keymanagementservicelong_notm}} 暗号化を有効にする](#keyprotect)と、Kubernetes マスターのローカル・ディスク上の etcd データの暗号化を有効にできます。 以前のバージョンの Kubernetes を実行するクラスター内では、etcd データは IBM 管理の暗号化ディスクに保管され、毎日バックアップされます。 etcd データがポッドに送信されるときには、データの保護と保全性を確保するために、データが TLS で暗号化されます。
2.  **ワーカー・ノードの 2 次ディスク**: ワーカー・ノードの 2 次ディスクは、コンテナー・ファイル・システムとローカルにプルしたイメージが保管される場所です。 このディスクは AES 256 ビット LUKS 暗号鍵で暗号化されます。この暗号鍵はワーカー・ノードに固有であり、IBM 管理の etcd にシークレットとして保管されます。 ワーカー・ノードを再ロードまたは更新すると、LUKS 鍵はローテーションされます。
3.  **ストレージ**: [ファイル、ブロック、またはオブジェクトの永続ストレージをセットアップ](/docs/containers?topic=containers-storage_planning#persistent_storage_overview)してデータを保管することができます。 IBM Cloud インフラストラクチャー (SoftLayer) ストレージ・インスタンスは、暗号化されたディスクにデータを保存するので、保存中のデータは暗号化されています。 さらに、オブジェクト・ストレージを選択した場合は、転送中のデータも暗号化されます。
4.  **{{site.data.keyword.Bluemix_notm}} サービス**: クラスターと {{site.data.keyword.registryshort_notm}} や {{site.data.keyword.watson}} などの [{{site.data.keyword.Bluemix_notm}} サービスを統合](/docs/containers?topic=containers-service-binding#bind-services)できます。 サービス資格情報は、etcd に保存されるシークレットに保管されます。このシークレットにアプリがアクセスするには、シークレットをボリュームとしてマウントするか、[デプロイメント](/docs/containers?topic=containers-app#secret)でシークレットを環境変数として指定します。
5.  **{{site.data.keyword.keymanagementserviceshort}}**: クラスターで [{{site.data.keyword.keymanagementserviceshort}} を有効にする](#keyprotect)と、ラップされたデータ暗号鍵 (DEK) が etcd に保管されます。 DEK は、サービス資格情報や LUKS 鍵など、クラスター内のシークレットを暗号化します。 ルート・キーは {{site.data.keyword.keymanagementserviceshort}} インスタンス内にあるので、暗号化されたシークレットへのアクセスを制御できます。 {{site.data.keyword.keymanagementserviceshort}} キーは、情報の盗難を防止する FIPS 140-2 レベル 2 認定のクラウド・ベースのハードウェア・セキュリティー・モジュールによって保護されています。 {{site.data.keyword.keymanagementserviceshort}} の暗号化の仕組みについて詳しくは、[エンベロープ暗号化](/docs/services/key-protect/concepts?topic=key-protect-envelope-encryption#envelope-encryption)を参照してください。

## シークレットを使用するケースについて
{: #secrets}

Kubernetes シークレットは、機密情報 (ユーザー名、パスワード、鍵など) を安全に保管するための手段です。 機密情報を暗号化する必要がある場合は、[{{site.data.keyword.keymanagementserviceshort}} を有効化して](#keyprotect)シークレットを暗号化します。 シークレットに保管できるものについて詳しくは、[Kubernetes の資料 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/concepts/configuration/secret/) を参照してください。
{:shortdesc}

シークレットを必要とする以下のタスクを確認してください。

### クラスターへのサービスの追加
{: #secrets_service}

サービスをクラスターにバインドする場合は、シークレットを作成してサービス資格情報を保管する必要はありません。 シークレットは自動的に作成されます。 詳しくは、[{{site.data.keyword.Bluemix_notm}} サービスをクラスターに追加する](/docs/containers?topic=containers-service-binding#bind-services)を参照してください。
{: shortdesc}

### アプリへのトラフィックを TLS シークレットで暗号化する
{: #secrets_tls}

ALB は、HTTP ネットワーク・トラフィックをクラスター内のアプリに振り分けてロード・バランシングを行います。 着信 HTTPS 接続のロード・バランシングも行う場合は ALB でその機能を構成できます。つまり、ネットワーク・トラフィックを復号し、復号した要求をクラスター内で公開されているアプリに転送するように構成します。 詳しくは、[Ingress の構成の資料](/docs/containers?topic=containers-ingress#public_inside_3)を参照してください。
{: shortdesc}

また、HTTPS プロトコルを必要とするアプリがあり、トラフィックの暗号化を維持する必要がある場合は、片方向認証または相互認証用シークレットと `ssl-services` アノテーションを併用できます。 詳しくは、[Ingress のアノテーションの資料](/docs/containers?topic=containers-ingress_annotation#ssl-services)を参照してください。

### Kubernetes イメージ・プル・シークレットに保存されている資格情報を使用したレジストリーへのアクセス
{: #imagepullsecret}

クラスターを作成すると、{{site.data.keyword.registrylong}} 資格情報用のシークレットが `default` Kubernetes 名前空間内に自動的に作成されます。 しかし、以下の状況でコンテナーをデプロイする場合は、[クラスター用に独自のイメージ・プル・シークレットを作成](/docs/containers?topic=containers-images#other)する必要があります。
* {{site.data.keyword.registryshort_notm}} レジストリー内のイメージから `default` 以外の Kubernetes 名前空間へ。
* 別の {{site.data.keyword.Bluemix_notm}} 地域または {{site.data.keyword.Bluemix_notm}} アカウントに保管されている、{{site.data.keyword.registryshort_notm}} レジストリー内のイメージから。
* 外部のプライベート・レジストリーに保管されているイメージから。

<br />


## {{site.data.keyword.keymanagementserviceshort}} (ベータ版) を使用した Kubernetes マスターのローカル・ディスクとシークレットの暗号化
{: #keyprotect}

クラスター内で Kubernetes [鍵管理サービス (KMS) プロバイダー ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/tasks/administer-cluster/kms-provider/) として [{{site.data.keyword.keymanagementservicefull}} ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/services/key-protect?topic=key-protect-getting-started-tutorial) を使用することによって、Kubernetes マスター内の etcd コンポーネントと Kubernetes シークレットを保護できます。 KMS プロバイダーは、Kubernetes バージョン 1.11 のアルファ版の機能であり、{{site.data.keyword.keymanagementserviceshort}} 統合が {{site.data.keyword.containerlong_notm}} のベータ版になりました。
{: shortdesc}

デフォルトでは、クラスター構成と Kubernetes シークレットは、IBM 管理の Kubernetes マスターの etcd コンポーネントに保管されます。 ワーカー・ノードには 2 次ディスクもあり、これは etcd にシークレットとして保管される IBM 管理の LUKS 鍵で暗号化されます。 etcd のデータは Kubernetes マスターのローカル・ディスクに保管され、{{site.data.keyword.cos_full_notm}} にバックアップされます。 {{site.data.keyword.cos_full_notm}} に転送中のデータも保存されたデータも暗号化されています。 ただし、Kubernetes マスターのローカル・ディスク上の etcd コンポーネント内のデータが自動的に暗号化されるのは、クラスターの {{site.data.keyword.keymanagementserviceshort}} 暗号化を有効にした後です。 以前のバージョンの Kubernetes を実行するクラスターでは、etcd データは IBM 管理の暗号化ディスクに保管され、毎日バックアップされます。

クラスターで {{site.data.keyword.keymanagementserviceshort}} を有効にすると、お客様固有のルート・キーを使用して、LUKS シークレットなどの etcd 内のデータが暗号化されます。 このルート・キーを使用してシークレットを暗号化すると、機密データの制御を強化できます。 お客様固有の暗号化を使用して、etcd データと Kubernetes シークレットにセキュリティー層を追加し、クラスターの機密情報にアクセスできる人をさらに細かく制御することができます。 etcd またはシークレットに対するアクセス権限を不可逆的に削除する必要が生じた場合は、ルート・キーを削除します。

{{site.data.keyword.keymanagementserviceshort}} インスタンス内のルート・キーは削除しないでください。 新しい鍵を使用するために交替させる場合でも、鍵は削除しないでください。 ルート・キーを削除すると、クラスターに含まれる etcd 内のデータやシークレットのデータにアクセスすることも削除することもできなくなります。
{: important}

開始前に、以下のことを行います。
* [アカウントにログインします。 該当する場合は、適切なリソース・グループをターゲットにします。 クラスターのコンテキストを設定します。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
* `ibmcloud ks cluster-get --cluster <cluster_name_or_ID>` を実行して **Version** フィールドを確認し、クラスターで Kubernetes バージョン 1.11.3_1521 以降が実行されていることを確認します。
* クラスターに対する[**管理者**の {{site.data.keyword.Bluemix_notm}} IAM プラットフォーム役割](/docs/containers?topic=containers-users#platform)があることを確認してください。
* クラスターが置かれている地域を対象に設定された API キーが、Key Protect の使用を許可されていることを確認します。 その地域において資格情報が保管されている API キー所有者を調べるには、`ibmcloud ks api-key-info --cluster <cluster_name_or_ID>` を実行します。

{{site.data.keyword.keymanagementserviceshort}} を有効にしたり、クラスター内のシークレットを暗号化するインスタンスまたはルート・キーを更新したりするには、以下のようにします。

1.  [{{site.data.keyword.keymanagementserviceshort}} のインスタンスを作成します](/docs/services/key-protect?topic=key-protect-provision#provision)。

2.  サービス・インスタンス ID を取得します。

    ```
    ibmcloud resource service-instance <kp_instance_name> | grep GUID
    ```
    {: pre}

3.  [ルート・キーを作成します](/docs/services/key-protect?topic=key-protect-create-root-keys#create-root-keys)。 デフォルトでは、ルート・キーは有効期限なしで作成されます。

    内部セキュリティー・ポリシーに準拠するように有効期限を設定する必要がありますか? [API を使用してルート・キーを作成し](/docs/services/key-protect?topic=key-protect-create-root-keys#create-root-key-api)、`expirationDate` パラメーターを組み込みます。 **重要**: ルート・キーの有効期限が切れる前に、これらの手順を繰り返して、新しいルート・キーを使用するようにクラスターを更新する必要があります。 更新しないと、シークレットを復号できません。
    {: tip}

4.  [ルート・キーの **ID**](/docs/services/key-protect?topic=key-protect-view-keys#view-keys-gui) をメモします。

5.  インスタンスの [{{site.data.keyword.keymanagementserviceshort}} エンドポイント](/docs/services/key-protect?topic=key-protect-regions#service-endpoints)を取得します。

6.  {{site.data.keyword.keymanagementserviceshort}} を有効にするクラスターの名前を取得します。

    ```
    ibmcloud ks clusters
    ```
    {: pre}

7.  クラスター内で {{site.data.keyword.keymanagementserviceshort}} を有効にします。 以前に取得した情報をフラグに記入します。 有効化プロセスが完了するまで、しばらく時間がかかる可能性があります。

    ```
    ibmcloud ks key-protect-enable --cluster <cluster_name_or_ID> --key-protect-url <kp_endpoint> --key-protect-instance <kp_instance_ID> --crk <kp_root_key_ID>
    ```
    {: pre}

8.  有効化中は、Kubernetes マスターにアクセスしてデプロイメントの YAML 構成を更新したりすることはできません。 以下のコマンドの出力で、**Master Status** が **Ready** になっていることを確認してください。
    ```
    ibmcloud ks cluster-get --cluster <cluster_name_or_ID>
    ```
    {: pre}

    有効化が進行中の場合の出力例:
    ```
    Name:                   <cluster_name>   
    ID:                     <cluster_ID>   
    ...
    Master Status:          Key Protect feature enablement in progress.  
    ```
    {: screen}

    マスターが使用可能である場合の出力例:
    ```
    Name:                   <cluster_name>   
    ID:                     <cluster_ID>   
    ...
    Master Status:          Ready (1 min ago)   
    ```
    {: screen}

    クラスターで {{site.data.keyword.keymanagementserviceshort}} を有効にすると、クラスターに含まれる `etcd` 内のデータ、既存のシークレット、新規作成されるシークレットは、{{site.data.keyword.keymanagementserviceshort}} ルート・キーを使用して自動的に暗号化されます。

9.  オプション: 鍵を交替させるには、新規ルート・キー ID を使用してこれらの手順を繰り返します。 新規ルート・キーは、前のルート・キーと共にクラスター構成に追加されるため、既存の暗号化データは引き続き保護されます。

{{site.data.keyword.keymanagementserviceshort}} インスタンス内のルート・キーは削除しないでください。 新しい鍵を使用するために交替させる場合でも、鍵は削除しないでください。 ルート・キーを削除すると、クラスターに含まれる etcd 内のデータやシークレットのデータにアクセスすることも削除することもできなくなります。
{: important}


## IBM Cloud Data Shield (ベータ版) を使用したデータの暗号化
{: #datashield}

{{site.data.keyword.datashield_short}} は、インテル® Software Guard Extensions (SGX) および Fortanix® テクノロジーと統合されているため、使用中の {{site.data.keyword.Bluemix_notm}} コンテナーのワークロードのコードとデータを保護できます。 アプリのコードとデータは、CPU で保護されたエンクレーブで実行されます。エンクレーブは、ワーカー・ノード上の信頼できるメモリー領域であり、ここでアプリの重要な側面を保護することで、コードとデータの機密を保ち、改ざんを防止できます。
{: shortdesc}

データの保護については、暗号化が最も一般的で効果的な制御方法の 1 つです。 しかし、データの暗号化はライフサイクルのすべての段階で行う必要があります。 ライフサイクルの中で、データは「保存中のデータ」、「転送中のデータ」、および「使用中のデータ」という 3 段階を踏みます。 「保存中のデータ」および「転送中のデータ」は、一般的には、保存されているときと転送されているときのデータを保護することを言います。 その保護をさらに一歩進めると、「使用中のデータ」を暗号化することになります。

社内の方針、政府規制、業界のコンプライアンス要件のためにデータの機密性を確保する必要がある場合は、このソリューションを使用すればクラウドへの移行の役に立つはずです。 ソリューションの使用事例には、金融機関や医療機関、オンプレミスのクラウド・ソリューションを必要とする政策を行っている国などがあります。

まずは、マシン・タイプ mb2c.4x32 を使用して SGX 対応ベアメタル・ワーカー・クラスターをプロビジョンし、[{{site.data.keyword.datashield_short}} の資料](/docs/services/data-shield?topic=data-shield-getting-started#getting-started)をお読みください。
