---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-06"

keywords: kubernetes, iks, ImagePullBackOff, registry, image, failed to pull image,

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


# クラスターとワーカー・ノードのトラブルシューティング
{: #cs_troubleshoot_clusters}

{{site.data.keyword.containerlong}} を使用する際は、ここに示すクラスターとワーカー・ノードのトラブルシューティング手法を検討してください。
{: shortdesc}

<p class="tip">より一般的な問題が起きている場合は、[クラスターのデバッグ](/docs/containers?topic=containers-cs_troubleshoot)を試してください。<br>また、トラブルシューティングの際に、[{{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool](/docs/containers?topic=containers-cs_troubleshoot#debug_utility) を使用して、テストを実行し、クラスターから関連情報を収集することもできます。</p>

## 許可エラーのためにクラスターを作成できないまたはワーカー・ノードを管理できない
{: #cs_credentials}

{: tsSymptoms}
以下のいずれかのコマンドを実行して、新規または既存のクラスターのワーカー・ノードを管理しようとしています。
* ワーカーのプロビジョン: `ibmcloud ks cluster-create`、`ibmcloud ks worker-pool-rebalance`、または `ibmcloud ks worker-pool-resize`
* ワーカーの再ロード: `ibmcloud ks worker-reload` または `ibmcloud ks worker-update`
* ワーカーのリブート: `ibmcloud ks worker-reboot`
* ワーカーの削除: `ibmcloud ks cluster-rm`、`ibmcloud ks worker-rm`、`ibmcloud ks worker-pool-rebalance`、または `ibmcloud ks worker-pool-resize`

しかし、以下のいずれかのようなエラー・メッセージを受け取ります。

```
We were unable to connect to your IBM Cloud infrastructure (SoftLayer) account.
Creating a standard cluster requires that you have either a Pay-As-You-Go account
that is linked to an IBM Cloud infrastructure (SoftLayer) account term or that you have used the {{site.data.keyword.containerlong_notm}} CLI to set your {{site.data.keyword.Bluemix_notm}} Infrastructure API keys.
```
{: screen}

```
{{site.data.keyword.Bluemix_notm}} Infrastructure Exception: 'Item' must be ordered with permission.
```
{: screen}

```
Worker not found. Review {{site.data.keyword.Bluemix_notm}} infrastructure permissions.
```
{: screen}

```
{{site.data.keyword.Bluemix_notm}} Infrastructure Exception: The user does not have the necessary {{site.data.keyword.Bluemix_notm}} Infrastructure permissions to add servers
```
{: screen}

```
IAM token exchange request failed: Cannot create IMS portal token, as no IMS account is linked to the selected BSS account
```
{: screen}

```
The cluster could not be configured with the registry. Make sure that you have the Administrator role for {{site.data.keyword.registrylong_notm}}.
```
{: screen}

{: tsCauses}
地域およびリソース・グループに対して設定されたインフラストラクチャー資格情報に、適切な[インフラストラクチャー許可](/docs/containers?topic=containers-access_reference#infra)がありません。通常、ユーザーのインフラストラクチャー許可は、地域およびリソース・グループの [API キー](/docs/containers?topic=containers-users#api_key)として保管されます。まれに、[別の {{site.data.keyword.Bluemix_notm}} アカウント・タイプ](/docs/containers?topic=containers-users#understand_infra)を使用する場合は、[手動でインフラストラクチャー資格情報を設定](/docs/containers?topic=containers-users#credentials)している可能性があります。インフラストラクチャー・リソースのプロビジョンに別の IBM Cloud インフラストラクチャー (SoftLayer) アカウントを使用する場合は、アカウントに[孤立クラスター](#orphaned)が作成されることもあります。

{: tsResolve}
アカウント所有者は、インフラストラクチャー・アカウントの資格情報を正しくセットアップする必要があります。 資格情報は、使用しているインフラストラクチャー・アカウントのタイプによって異なります。

開始前に、以下のことを行います。 [アカウントにログインします。 該当する場合は、適切なリソース・グループをターゲットにします。 クラスターのコンテキストを設定します。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  地域およびリソース・グループのインフラストラクチャー許可に使用されるユーザー資格情報を特定します。
    1.  クラスターの地域およびリソース・グループの API キーを確認します。
        ```
        ibmcloud ks api-key-info --cluster <cluster_name_or_ID>
        ```
        {: pre}

        出力例:
        ```
        Getting information about the API key owner for cluster <cluster_name>...
        OK
        Name                Email   
        <user_name>         <name@email.com>
        ```
        {: screen}
    2.  地域およびリソース・グループのインフラストラクチャー・アカウントが、別の IBM Cloud インフラストラクチャー (SoftLayer) アカウントを使用するように手動で設定されているかどうかを確認します。
        ```
        ibmcloud ks credential-get --region <us-south>
        ```
        {: pre}

        **資格情報が別のアカウントを使用するように設定されている場合の出力例**。この場合、前のステップで取得した API キーに別のユーザーの資格情報が保管されていても、ターゲットにした地域とリソース・グループには、ユーザーのインフラストラクチャー資格情報が使用されます。
        ```
        OK
        Infrastructure credentials for user name <1234567_name@email.com> set for resource group <resource_group_name>.
        ```
        {: screen}

        **資格情報が別のアカウントを使用するように設定されていない場合の出力例**。この場合、前のステップで取得した API キーの所有者は、地域およびリソース・グループに使用されるインフラストラクチャー資格情報を持っています。
        ```
        FAILED
        No credentials set for resource group <resource_group_name>.: The user credentials could not be found. (E0051)
        ```
        {: screen}
2.  ユーザーのインフラストラクチャー許可を検証します。
    1.  地域およびリソース・グループの推奨および必須のインフラストラクチャー許可をリストします。
        ```
        ibmcloud ks infra-permissions-get --region <region>
        ```
        {: pre}
    2.  [API キーまたは手動で設定されたアカウントのインフラストラクチャー資格情報の所有者が正しい許可を持っている](/docs/containers?topic=containers-users#owner_permissions)ことを確認します。
    3.  必要に応じて、地域およびリソース・グループの [API キー](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_api_key_reset)または[手動で設定された](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_set)インフラストラクチャー資格情報の所有者を変更できます。
3.  変更された許可によって、許可ユーザーがクラスターのインフラストラクチャー操作を実行できることをテストします。
    1.  例えば、ワーカー・ノードの削除を試行できます。
        ```
        ibmcloud ks worker-rm --cluster <cluster_name_or_ID> --worker <worker_node_ID>
        ```
        {: pre}
    2.  ワーカー・ノードが削除されたかどうかを確認します。
        ```
        ibmcloud ks worker-get --cluster <cluster_name_or_ID> --worker <worker_node_ID>
        ```
        {: pre}

        ワーカー・ノードが正常に削除された場合の出力例。ワーカー・ノードが削除されたため、`worker-get` 操作は失敗します。インフラストラクチャー許可が正しくセットアップされています。
        ```
        FAILED
        The specified worker node could not be found. (E0011)
        ```
        {: screen}

    3.  ワーカー・ノードが削除されていない場合は、[**State** フィールドと **Status** フィールド](/docs/containers?topic=containers-cs_troubleshoot#debug_worker_nodes)、および[ワーカー・ノードに関する一般的な問題](/docs/containers?topic=containers-cs_troubleshoot#common_worker_nodes_issues)を確認して、デバッグを続行します。
    4.  資格情報を手動で設定しており、引き続きインフラストラクチャー・アカウントにクラスターのワーカー・ノードを表示できない場合は、[クラスターが孤立](#orphaned)していないかどうかを確認できます。

<br />


## ファイアウォールがあるために CLI コマンドを実行できない
{: #ts_firewall_clis}

{: tsSymptoms}
CLI からコマンド `ibmcloud`、`kubectl`、または `calicoctl` を実行すると、失敗します。

{: tsCauses}
ローカル・システムからプロキシーまたはファイアウォール経由での公共のエンドポイントへのアクセスが企業ネットワーク・ポリシーによって禁止されている可能性があります。

{: tsResolve}
[CLI コマンドでの TCP アクセスを許可します](/docs/containers?topic=containers-firewall#firewall_bx)。 このタスクには、クラスターに対する [**管理者** {{site.data.keyword.Bluemix_notm}} IAM プラットフォーム役割](/docs/containers?topic=containers-users#platform) が必要です。


## クラスターのリソースにアクセスできない
{: #cs_firewall}

{: tsSymptoms}
クラスターのワーカー・ノードがプライベート・ネットワークで通信できない場合、さまざまな症状が現れることがあります。

- `kubectl exec`、`attach`、`logs`、`proxy`、または `port-forward` を実行した場合のサンプル・エラー・メッセージ:
  ```
  Error from server: error dialing backend: dial tcp XXX.XXX.XXX:10250: getsockopt: connection timed out
  ```
  {: screen}

- `kubectl proxy` は正常に実行されたが Kubernetes ダッシュボードを使用できない場合のサンプル・エラー・メッセージ:
  ```
  timeout on 172.xxx.xxx.xxx
  ```
  {: screen}

- `kubectl proxy` が失敗したか、サービスへの接続が失敗した場合のサンプル・エラー・メッセージ:
  ```
  Connection refused
  ```
  {: screen}

  ```
  Connection timed out
  ```
  {: screen}

  ```
  Unable to connect to the server: net/http: TLS handshake timeout
  ```
  {: screen}


{: tsCauses}
クラスターのリソースにアクセスするには、ワーカー・ノードがプライベート・ネットワークで通信可能でなければなりません。 Vyatta などの別のファイアウォールをセットアップしたか、IBM Cloud インフラストラクチャー (SoftLayer) アカウントの既存のファイアウォール設定をカスタマイズした可能性があります。 {{site.data.keyword.containerlong_notm}} では、ワーカー・ノードと Kubernetes マスター間で通信を行うには、特定の IP アドレスとポートが開いている必要があります。 ワーカー・ノードが複数のゾーンに分散されている場合、VLAN スパンニングを有効にして、プライベート・ネットワーク通信を許可する必要があります。 ワーカー・ノードが再ロード・ループにはまっている場合にも、ワーカー・ノード間の通信が不能になることがあります。

{: tsResolve}
1. クラスター内のワーカー・ノードをリストして、ワーカー・ノードが `Reloading` 状態で停滞していないことを確認します。
   ```
   ibmcloud ks workers --cluster <cluster_name_or_id>
   ```
   {: pre}

2. 複数ゾーン・クラスターを使用しており、アカウントで VRF が有効になっていない場合は、アカウントに対して [VLAN スパンニングが有効になっている](/docs/containers?topic=containers-subnets#subnet-routing)ことを確認します。
3. Vyatta またはカスタム・ファイアウォールを設定している場合は、[必要なポートが開いており](/docs/containers?topic=containers-firewall#firewall_outbound)、クラスターがインフラストラクチャーのリソースとサービスへのアクセスを許可されていることを確認します。

<br />



## クラスターの表示や操作ができない
{: #cs_cluster_access}

{: tsSymptoms}
* クラスターが見つかりません。 `ibmcloud ks clusters` を実行しても、クラスターが出力にリストされません。
* クラスターを操作できません。 `ibmcloud ks cluster-config` やその他のクラスター固有のコマンドを実行しても、クラスターが見つかりません。


{: tsCauses}
{{site.data.keyword.Bluemix_notm}} では各リソースが特定のリソース・グループに属している必要があります。 例えば、クラスター `mycluster` は、`default` リソース・グループに存在している可能性があります。 アカウント所有者によって {{site.data.keyword.Bluemix_notm}} IAM プラットフォーム役割を割り当てられてリソースへのアクセス権限を付与されるとき、そのアクセス権限は特定のリソースを対象にしたものである場合と、リソース・グループを対象にしたものである場合があります。 特定のリソースを対象にしたアクセス権限を付与された場合、それが属するリソース・グループへのアクセス権限はありません。 その場合、アクセス権限を持つクラスターを操作するために、そのリソース・グループをターゲットにする必要はありません。 そのクラスターが属するグループとは異なるリソース・グループをターゲットとした場合、そのクラスターに対するアクションは失敗する可能性があります。 反対に、特定のリソース・グループへのアクセス権限の一部として 1 つのリソースへのアクセス権限が付与されている場合、そのグループのクラスターを操作するために、リソース・グループをターゲットとする必要があります。 クラスターが属するリソース・グループをターゲットとして CLI セッションを実行しないと、そのクラスターに対するアクションは失敗する可能性があります。

クラスターが見つからない、またはクラスターの操作ができない場合、次のいずれかの問題が発生している可能性があります。
* クラスターに対するアクセス権限と、そのクラスターが属するリソース・グループに対するアクセス権限はあるが、そのクラスターが属するリソース・グループをターゲットとして CLI セッションが実行されていない。
* クラスターに対するアクセス権限はあるが、それは、そのクラスターが属するリソース・グループの一部として付与されたアクセス権限ではない。 CLI セッションはそのリソース・グループまたは別のリソース・グループをターゲットとしている。
* クラスターに対するアクセス権限がない。

{: tsResolve}
自分のユーザー・アクセス許可を確認するには、以下のようにします。

1. 自分のすべてのユーザー許可をリストします。
    ```
    ibmcloud iam user-policies <your_user_name>
    ```
    {: pre}

2. クラスターに対するアクセス権限と、そのクラスターが属するリソース・グループに対するアクセス権限があるかどうかを確認します。
    1. **「リソース・グループ名」**の値がクラスターのリソース・グループになっていて、**「メモ」**の値が`「ポリシーはリソース・グループに適用されます」`になっているポリシーを検索します。 このポリシーがある場合、そのリソース・グループに対するアクセス権限があります。 例えば、次のポリシーは、ユーザーが `test-rg` リソース・グループに対するアクセス権限を持っていることを示しています。
        ```
        Policy ID:   3ec2c069-fc64-4916-af9e-e6f318e2a16c
        Roles:       Viewer
        Resources:
                     Resource Group ID     50c9b81c983e438b8e42b2e8eca04065
                     Resource Group Name   test-rg
                     Memo                  Policy applies to the resource group
        ```
        {: screen}
    2. **「リソース・グループ名」**の値がクラスターのリソース・グループになっていて、**「サービス名」**の値が `containers-kubernetes` になっているか値がなく、**「メモ」**の値が`「ポリシーはリソース・グループ内のリソースに適用されます」`になっているポリシーを検索します。 このポリシーがある場合、そのリソース・グループ内のクラスターまたはすべてのリソースに対するアクセス権限があります。 例えば、次のポリシーは、ユーザーが `test-rg` リソース・グループ内のクラスターに対するアクセス権限を持っていることを示しています。
        ```
        Policy ID:   e0ad889d-56ba-416c-89ae-a03f3cd8eeea
        Roles:       Administrator
        Resources:
                     Resource Group ID     a8a12accd63b437bbd6d58fb6a462ca7
                     Resource Group Name   test-rg
                     Service Name          containers-kubernetes
                     Service Instance
                     Region
                     Resource Type
                     Resource
                     Memo                  Policy applies to the resource(s) within the resource group
        ```
        {: screen}
    3. これらのポリシーを両方とも持っている場合は、ステップ 4 の最初の黒丸にスキップします。 ステップ 2a のポリシーはないが、ステップ 2b のポリシーはある場合は、ステップ 4 の 2 番目の黒丸にスキップします。 どちらのポリシーもない場合は、そのままステップ 3 に進んでください。

3. クラスターに対するアクセス権限はあるが、そのクラスターが属するリソース・グループに対するアクセス権限の一部ではない、という状態になっているかどうかを確認します。
    1. **「ポリシー ID」**フィールドと**「役割」**フィールドの横に値がないポリシーを見つけます。 このポリシーがある場合、アカウント全体に対するアクセス権限の一部としてこのクラスターに対するアクセス権限があります。 例えば、次のポリシーは、ユーザーがアカウント内のすべてのリソースに対するアクセス権限を持っていることを示しています。
        ```
        Policy ID:   8898bdfd-d520-49a7-85f8-c0d382c4934e
        Roles:       Administrator, Manager
        Resources:
                     Service Name
                     Service Instance
                     Region
                     Resource Type
                     Resource
        ```
        {: screen}
    2. **「サービス名」**の値が `containers-kubernetes` になっていて、**「サービス・インスタンス」**の値がクラスターの ID になっているポリシーを見つけます。 クラスター ID は、`ibmcloud ks cluster-get --cluster <cluster_name>` を実行して見つけることができます。 例えば、次のポリシーは、ユーザーが特定のクラスターに対するアクセス権限を持っていることを示しています。
        ```
        Policy ID:   140555ce-93ac-4fb2-b15d-6ad726795d90
        Roles:       Administrator
        Resources:
                     Service Name       containers-kubernetes
                     Service Instance   df253b6025d64944ab99ed63bb4567b6
                     Region
                     Resource Type
                     Resource
        ```
        {: screen}
    3. これらのいずれかのポリシーがある場合、ステップ 4 の 2 番目の黒丸にスキップします。これらのいずれのポリシーもない場合は、ステップ 4 の 3 番目の黒丸にスキップします。

4. アクセス・ポリシーの状態に応じて、次のオプションのいずれかを選択します。
    * クラスターに対するアクセス権限と、そのクラスターが属するリソース・グループに対するアクセス権限がある場合:
      1. リソース・グループをターゲットとして設定します。 **注意**: このリソース・グループへのターゲット設定を解除しない限り、他のリソース・グループのクラスターは操作できません。
          ```
          ibmcloud target -g <resource_group>
          ```
          {: pre}

      2. クラスターをターゲットとして設定します。
          ```
          ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
          ```
          {: pre}

    * クラスターに対するアクセス権限はあるが、そのクラスターが属するリソース・グループに対するアクセス権限はない場合:
      1. リソース・グループをターゲットとして設定しないでください。 既にリソース・グループをターゲットとして設定していた場合は、ターゲット設定を解除してください。
        ```
        ibmcloud target --unset-resource-group
        ```
        {: pre}

      2. クラスターをターゲットとして設定します。
        ```
        ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
        ```
        {: pre}

    * クラスターに対するアクセス権限がない場合:
        1. アカウント所有者に連絡して、そのクラスターに対する [{{site.data.keyword.Bluemix_notm}}IAM プラットフォーム役割](/docs/containers?topic=containers-users#platform)を自分に割り当ててもらいます。
        2. リソース・グループをターゲットとして設定しないでください。 既にリソース・グループをターゲットとして設定していた場合は、ターゲット設定を解除してください。
          ```
          ibmcloud target --unset-resource-group
          ```
          {: pre}
        3. クラスターをターゲットとして設定します。
          ```
          ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
          ```
          {: pre}

<br />


## SSH によるワーカー・ノードへのアクセスが失敗する
{: #cs_ssh_worker}

{: tsSymptoms}
SSH 接続を使用してワーカー・ノードにアクセスすることはできません。

{: tsCauses}
パスワードによる SSH は、ワーカー・ノードでは使用できません。

{: tsResolve}
すべてのノードで実行する必要があるアクションについては、Kubernetes [`DaemonSet` ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) を使用するか、または実行する必要がある一回限りのアクション用のジョブを使用します。

<br />


## ベアメタルのインスタンス ID がワーカー・レコードと不整合
{: #bm_machine_id}

{: tsSymptoms}
ベアメタルのワーカー・ノードで `ibmcloud ks worker` コマンドを使用すると、次のようなメッセージが表示されます。

```
Instance ID inconsistent with worker records
```
{: screen}

{: tsCauses}
マシンでハードウェアの問題が発生した場合、マシン ID が {{site.data.keyword.containerlong_notm}} のワーカー・レコードと不整合になる可能性があります。 IBM Cloud インフラストラクチャー (SoftLayer) がこの問題を解決すると、サービスが識別していないシステム内で、あるコンポーネントが変化する可能性があります。

{: tsResolve}
{{site.data.keyword.containerlong_notm}} がそのマシンを再識別するには、[ベアメタルのワーカー・ノードを再ロードします](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)。 **注**: 再ロードによって、マシンの[パッチ・バージョン](/docs/containers?topic=containers-changelog)も更新されます。

[ベアメタル・ワーカー・ノードは削除](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_rm)することもできます。 **注**: ベア・メタル・インスタンスは月単位で請求されます。

<br />


## 孤立クラスターのインフラストラクチャーを変更または削除できない
{: #orphaned}

{: tsSymptoms}
クラスターで次のようなインフラストラクチャー関連のコマンドを実行できません。
* ワーカー・ノードの追加または削除
* ワーカー・ノードの再ロードまたはリブート
* ワーカー・プールのサイズ変更
* クラスターの更新

IBM Cloud インフラストラクチャー (SoftLayer) アカウントのクラスター・ワーカー・ノードを表示できません。 ただし、アカウントの他のクラスターは更新、管理できます。

さらに、[適切なインフラストラクチャー資格情報](#cs_credentials)を持っていることは確認済みです。

{: tsCauses}
クラスターが、{{site.data.keyword.containerlong_notm}} アカウントとのリンクがなくなった IBM Cloud インフラストラクチャー (SoftLayer) アカウントにプロビジョンされている可能性があります。 そのクラスターは孤立しています。 リソースが別のアカウントにあるため、リソースを変更するためのインフラストラクチャー資格情報がありません。

クラスターがどのように孤立する可能性があるのかを理解するために、次のシナリオを検討してください。
1.  あなたは {{site.data.keyword.Bluemix_notm}} 従量課金 (PAYG) アカウントを持っています。
2.  あなたは `Cluster1` という名前のクラスターを作成します。 ワーカー・ノードと他のインフラストラクチャー・リソースは従量課金アカウントに付属するインフラストラクチャー・アカウントにプロビジョンされます。
3.  後で、チームが既存のまたは共有の IBM Cloud インフラストラクチャー (SoftLayer) アカウントを使用していることがわかります。 そこであなたは、`ibmcloud ks credential-set` コマンドを使用して IBM Cloud インフラストラクチャー (SoftLayer) 資格情報を変更し、チーム・アカウントを使用するようにします。
4.  あなたは `Cluster2` という名前の別のクラスターを作成します。 ワーカー・ノードと他のインフラストラクチャー・リソースが、チームのインフラストラクチャー・アカウントにプロビジョンされます。
5.  あなたは `Cluster1` でワーカー・ノードの更新、ワーカー・ノードの再ロードが必要なことに気付きますが、ここでは単純にワーカー・ノードを削除することによってクリーンアップしようと考えます。 しかし、`Cluster1` は別のインフラストラクチャー・アカウントにプロビジョンされているため、そのインフラストラクチャー・リソースを変更することはできません。 `Cluster1` は孤立しています。
6.  あなたは次のセクションの解決手順を実行しますが、インフラストラクチャー資格情報をチーム・アカウントに戻す設定はしていません。 `Cluster1` は削除できますが、今度は `Cluster2` が孤立しています。
7.  あなたはインフラストラクチャー資格情報を、`Cluster2` を作成したチーム・アカウントに戻します。 これで、もう孤立クラスターはなくなりました。

<br>

{: tsResolve}
1.  クラスターが現在存在している地域がどのインフラストラクチャー・アカウントを使用してクラスターをプロビジョンしているかを確認します。
    1.  [{{site.data.keyword.containerlong_notm}} クラスター・コンソール![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/kubernetes/clusters) にログインします。
    2.  テーブルから、該当のクラスターを選択します。
    3.  **「概要」**タブで、**「インフラストラクチャー・ユーザー (Infrastructure User)」**フィールドを確認します。 このフィールドは、{{site.data.keyword.containerlong_notm}} アカウントがデフォルトとは異なるインフラストラクチャー・アカウントを使用しているかどうかを判別するのに役立ちます。
        * **「インフラストラクチャー・ユーザー (Infrastructure User)」**フィールドが表示されない場合、インフラストラクチャー・アカウントとプラットフォーム・アカウントに同じ資格情報を使用する、リンクされた従量課金 (PAYG) アカウントがあります。 変更できないクラスターは、別のインフラストラクチャー・アカウントでプロビジョンされている可能性があります。
        * **「インフラストラクチャー・ユーザー (Infrastructure User)」**フィールドが表示されている場合、従量課金 (PAYG) アカウントに付属するものとは異なるインフラストラクチャー・アカウントを使用しています。 これらの異なる資格情報は、地域内のすべてのクラスターに適用されます。 変更できないクラスターは、従量課金 (PAYG) アカウントまたは別のインフラストラクチャー・アカウントでプロビジョンされている可能性があります。
2.  クラスターをプロビジョンするためにどのインフラストラクチャー・アカウントが使用されたかを確認します。
    1.  **「ワーカー・ノード」**タブでワーカー・ノードを選択し、その **ID** を書き留めます。
    2.  メニュー ![メニュー・アイコン](../icons/icon_hamburger.svg "メニュー・アイコン") を開き、**「クラシック・インフラストラクチャー」**をクリックします。
    3.  インフラストラクチャーのナビゲーション・ペインから**「デバイス」>「デバイス・リスト」**をクリックします。
    4.  先ほど書き留めたワーカー・ノードの ID を検索します。
    5.  このワーカー・ノード ID が見つからない場合、そのワーカー・ノードはこのインフラストラクチャー・アカウントにプロビジョンされていません。 別のインフラストラクチャー・アカウントに切り替えて、もう一度やり直します。
3.  `ibmcloud ks credential-set` [コマンド](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_set)を使用して、インフラストラクチャー資格情報を、クラスター・ワーカー・ノードがプロビジョンされたアカウント (前のステップで見つけたアカウント) に変更します。
    インフラストラクチャー資格情報にもうアクセスできず、それを取得できない場合は、{{site.data.keyword.Bluemix_notm}} サポート・ケースを開いて孤立クラスターを除去する必要があります。
    {: note}
4.  [クラスターを削除します](/docs/containers?topic=containers-remove)。
5.  必要に応じて、インフラストラクチャー資格情報を前のアカウントにリセットします。 切り替え後のアカウントとは異なるインフラストラクチャー・アカウントを使用してクラスターを作成した場合、それらのクラスターは孤立化する可能性があります。
    * 資格情報を別のインフラストラクチャー・アカウントに設定するには、`ibmcloud ks credential-set` [コマンド](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_set)を使用します。
    * {{site.data.keyword.Bluemix_notm}} 従量課金 (PAYG) アカウントに付属するデフォルトの資格情報を使用するには、`ibmcloud ks credential-unset --region <region>` [コマンド](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_unset)を使用します。

<br />


## `kubectl` コマンドがタイムアウトする
{: #exec_logs_fail}

{: tsSymptoms}
`kubectl exec`、`kubectl attach`、`kubectl proxy`、`kubectl port-forward`、`kubectl logs` などのコマンドを実行すると、以下のメッセージが表示されます。

  ```
  <workerIP>:10250: getsockopt: connection timed out
  ```
  {: screen}

{: tsCauses}
マスター・ノードとワーカー・ノードの間の OpenVPN 接続が正しく機能していません。

{: tsResolve}
1. 1 つのクラスターに複数の VLAN がある場合、同じ VLAN 上に複数のサブネットがある場合、または複数ゾーン・クラスターがある場合は、IBM Cloud インフラストラクチャー (SoftLayer) アカウントに対して[仮想ルーター機能 (VRF)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) を有効にして、ワーカー・ノードがプライベート・ネットワーク上で相互に通信できるようにする必要があります。 VRF を有効にするには、[IBM Cloud インフラストラクチャー (SoftLayer) のアカウント担当者に連絡してください](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#how-you-can-initiate-the-conversion)。 VRF の有効化が不可能または不要な場合は、[VLAN スパンニング](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning)を有効にしてください。 この操作を実行するには、**「ネットワーク」>「ネットワーク VLAN スパンニングの管理」**で設定する[インフラストラクチャー権限](/docs/containers?topic=containers-users#infra_access)が必要です。ない場合は、アカウント所有者に対応を依頼してください。 VLAN スパンニングが既に有効になっているかどうかを確認するには、`ibmcloud ks vlan-spanning-get<region>` [コマンド](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get)を使用します。
2. OpenVPN クライアント・ポッドを再始動します。
  ```
  kubectl delete pod -n kube-system -l app=vpn
  ```
  {: pre}
3. それでも同じエラー・メッセージが表示される場合は、VPN ポッドがあるワーカー・ノードが正常ではない可能性があります。 VPN ポッドを再始動し、異なるワーカー・ノードにスケジュールを変更するには、VPN ポッドがある[ワーカー・ノードを閉鎖、排出してリブートします](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reboot)。

<br />


## サービスをクラスターにバインドすると同名エラーが発生する
{: #cs_duplicate_services}

{: tsSymptoms}
`ibmcloud ks cluster-service-bind --cluster <cluster_name> --namespace <namespace> --service <service_instance_name>` を実行すると、以下のメッセージが表示されます。

```
Multiple services with the same name were found.
Run 'ibmcloud service list' to view available Bluemix service instances...
```
{: screen}

{: tsCauses}
異なる地域にある複数のサービス・インスタンスの名前が等しい可能性があります。

{: tsResolve}
`ibmcloud ks cluster-service-bind` コマンドで、サービス・インスタンス名ではなくサービス GUID を使用してください。

1. [バインドするサービス・インスタンスが含まれる {{site.data.keyword.Bluemix_notm}} 地域にログインします。](/docs/containers?topic=containers-regions-and-zones#bluemix_regions)

2. サービス・インスタンスの GUID を取得します。
  ```
  ibmcloud service show <service_instance_name> --guid
  ```
  {: pre}

  出力:
  ```
  Invoking 'cf service <service_instance_name> --guid'...
  <service_instance_GUID>
  ```
  {: screen}
3. サービスをクラスターに再びバインドします。
  ```
  ibmcloud ks cluster-service-bind --cluster <cluster_name> --namespace <namespace> --service <service_instance_GUID>
  ```
  {: pre}

<br />


## サービスをクラスターにバインドすると、サービスが見つからないというエラーが発生する
{: #cs_not_found_services}

{: tsSymptoms}
`ibmcloud ks cluster-service-bind --cluster <cluster_name> --namespace <namespace> --service <service_instance_name>` を実行すると、以下のメッセージが表示されます。

```
Binding service to a namespace...
FAILED

The specified IBM Cloud service could not be found. If you just created the service, wait a little and then try to bind it again. To view available IBM Cloud service instances, run 'ibmcloud service list'. (E0023)
```
{: screen}

{: tsCauses}
サービスをクラスターにバインドするには、サービス・インスタンスがプロビジョンされているスペースに対する Cloud Foundry 開発者ユーザー役割が必要です。 また、{{site.data.keyword.containerlong}} に対するエディターの {{site.data.keyword.Bluemix_notm}} IAM プラットフォーム・アクセス権限も必要です。 サービス・インスタンスにアクセスするには、サービス・インスタンスがプロビジョンされているスペースにログインする必要があります。

{: tsResolve}

**ユーザーは、以下の手順を実行します。**

1. {{site.data.keyword.Bluemix_notm}} にログインします。
   ```
   ibmcloud login
   ```
   {: pre}

2. サービス・インスタンスがプロビジョンされている組織とスペースをターゲットにします。
   ```
   ibmcloud target -o <org> -s <space>
   ```
   {: pre}

3. サービス・インスタンスをリストして、正しいスペースにいることを確認します。
   ```
   ibmcloud service list
   ```
   {: pre}

4. サービスのバインドを再試行してください。 同じエラーが発生した場合は、アカウント管理者に連絡して、サービスをバインドするための十分な許可があることを確認してください (以下のアカウント管理者の手順を参照してください)。

**アカウント管理者は、以下の手順を実行します。**

1. この問題が発生しているユーザーに、[{{site.data.keyword.containerlong}} の Editor 許可](/docs/iam?topic=iam-iammanidaccser#edit_existing)があることを確認します。

2. この問題が発生したユーザーに、サービスがプロビジョンされている[スペースに対する Cloud Foundry 開発者役割](/docs/iam?topic=iam-mngcf#update_cf_access)があることを確認します。

3. 正しい許可が存在する場合は、別の許可を割り当ててから、必要な許可を再度割り当ててみてください。

4. 数分待ってから、ユーザーにサービスのバインドを再試行してもらってください。

5. これで問題が解決しない場合は、{{site.data.keyword.Bluemix_notm}} IAM 許可が同期していないため、自分で問題を解決することはできません。 サポート・ケースを開いて、[IBM サポートにお問い合わせください](/docs/get-support?topic=get-support-getting-customer-support)。 必ず、クラスター ID、ユーザー ID、およびサービス・インスタンス ID をご提供ください。
   1. クラスター ID を取得します。
      ```
      ibmcloud ks clusters
      ```
      {: pre}

   2. サービス・インスタンス ID を取得します。
      ```
      ibmcloud service show <service_name> --guid
      ```
      {: pre}


<br />


## サービスをクラスターにバインドすると、サービス・キーがサポートされないというエラーが発生する
{: #cs_service_keys}

{: tsSymptoms}
`ibmcloud ks cluster-service-bind --cluster <cluster_name> --namespace <namespace> --service <service_instance_name>` を実行すると、以下のメッセージが表示されます。

```
This service doesn't support creation of keys
```
{: screen}

{: tsCauses}
{{site.data.keyword.Bluemix_notm}} の一部のサービス ({{site.data.keyword.keymanagementservicelong}} など) では、サービス資格情報 (サービス・キーとも呼ばれます) の作成はサポートされません。 サービス・キーをサポートしない場合、サービスはクラスターにバインドできません。 サービス・キーの作成をサポートするサービスのリストを確認するには、[{{site.data.keyword.Bluemix_notm}} サービスを使用するための外部アプリの使用可能化](/docs/resources?topic=resources-externalapp#externalapp)を参照してください。

{: tsResolve}
サービス・キーをサポートしないサービスを統合するには、アプリから直接サービスにアクセスするために使用できる API がサービスで提供されているかどうかを確認します。 例えば、{{site.data.keyword.keymanagementservicelong}} を使用する場合は、[API リファレンス ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/apidocs/kms?language=curl) を参照してください。

<br />


## ワーカー・ノードが更新または再ロードされた後で、重複するノードとポッドが表示される
{: #cs_duplicate_nodes}

{: tsSymptoms}
`kubectl get nodes` を実行すると、状況が **`NotReady`** の重複したワーカー・ノードが表示されます。 **`NotReady`** のワーカー・ノードにはパブリック IP アドレスがありますが、**`Ready`** のワーカー・ノードにはプライベート IP アドレスがあります。

{: tsCauses}
以前のクラスターでは、クラスターのパブリック IP アドレスごとにワーカー・ノードがリストされていました。 現在は、ワーカー・ノードはクラスターのプライベート IP アドレスごとにリストされています。 ノードを再ロードまたは更新すると、IP アドレスは変更されますがパブリック IP アドレスへの参照は残ります。

{: tsResolve}
これらの重複によってサービスが中断されることはありませんが、以前のワーカー・ノード参照は API サーバーから削除できます。

  ```
  kubectl delete node <node_name1> <node_name2>
  ```
  {: pre}

<br />


## 新しいワーカー・ノード上のポッドへのアクセスがタイムアウトで失敗する
{: #cs_nodes_duplicate_ip}

{: tsSymptoms}
クラスターのワーカー・ノードを削除した後にワーカー・ノードを追加しました。 ポッドまたは Kubernetes サービスをデプロイすると、新しく作成したワーカー・ノードにリソースがアクセスできずに、接続がタイムアウトになります。

{: tsCauses}
クラスターからワーカー・ノードを削除した後でワーカー・ノードを追加すると、削除されたワーカー・ノードのプライベート IP アドレスが新しいワーカー・ノードに割り当てられる場合があります。 Calico はこのプライベート IP アドレスをタグとして使用して、削除されたノードにアクセスし続けます。

{: tsResolve}
正しいノードを指すように、プライベート IP アドレスの参照を手動で更新します。

1.  2 つのワーカー・ノードの**プライベート IP** アドレスが同じであることを確認します。 削除されたワーカーの**プライベート IP** と **ID** をメモします。

  ```
  ibmcloud ks workers --cluster <cluster_name_or_id>
  ```
  {: pre}

  ```
  ID                                                 Public IP       Private IP       Machine Type   State     Status   Zone   Version
  kube-dal10-cr9b7371a7fcbe46d08e04f046d5e6d8b4-w1   169.xx.xxx.xxx  10.xxx.xx.xxx    b3c.4x16       normal    Ready    dal10      1.13.6
  kube-dal10-cr9b7371a7fcbe46d08e04f046d5e6d8b4-w2   169.xx.xxx.xxx  10.xxx.xx.xxx    b3c.4x16       deleted    -       dal10      1.13.6
  ```
  {: screen}

2.  [Calico CLI](/docs/containers?topic=containers-network_policies#adding_network_policies) をインストールします。
3.  Calico で使用可能なワーカー・ノードをリストします。 <path_to_file> は、Calico 構成ファイルのローカル・パスに置き換えてください。

  ```
  calicoctl get nodes --config=filepath/calicoctl.cfg
  ```
  {: pre}

  ```
  NAME
  kube-dal10-cr9b7371a7faaa46d08e04f046d5e6d8b4-w1
  kube-dal10-cr9b7371a7faaa46d08e04f046d5e6d8b4-w2
  ```
  {: screen}

4.  Calico で重複しているワーカー・ノードを削除します。 NODE_ID はワーカー・ノード ID に置き換えてください。

  ```
  calicoctl delete node NODE_ID --config=<path_to_file>/calicoctl.cfg
  ```
  {: pre}

5.  削除されていないワーカー・ノードをリブートします。

  ```
  ibmcloud ks worker-reboot --cluster <cluster_name_or_id> --worker <worker_id>
  ```
  {: pre}


削除されたノードが Calico にリストされなくなります。

<br />




## ポッド・セキュリティー・ポリシーが原因でポッドをデプロイできない
{: #cs_psp}

{: tsSymptoms}
ポッドの作成後、または `kubectl get events` を実行してポッドのデプロイメントを確認した後に、次のようなエラー・メッセージが表示されます。

```
unable to validate against any pod security policy
```
{: screen}

{: tsCauses}
[`PodSecurityPolicy` アドミッション・コントローラー](/docs/containers?topic=containers-psp)は、ポッドを作成しようとしたユーザー・アカウントまたはサービス・アカウント (デプロイメントまたは Helm tiller など) の許可を検査します。 ポッドのセキュリティー・ポリシーがユーザー・アカウントまたはサービス・アカウントをサポートしていない場合、`PodSecurityPolicy` アドミッション・コントローラーはポッドが作成されないようにします。

[{{site.data.keyword.IBM_notm}} クラスター管理](/docs/containers?topic=containers-psp#ibm_psp)用のポッドのセキュリティー・ポリシー・リソースのいずれかを削除した場合、同様の問題が発生する場合があります。

{: tsResolve}
ユーザー・アカウントまたはサービス・アカウントがポッドのセキュリティー・ポリシーによって許可されていることを確認してください。 [既存のポリシーを変更する](/docs/containers?topic=containers-psp#customize_psp)ことが必要になる場合があります。

{{site.data.keyword.IBM_notm}} クラスター管理リソースを削除した場合は、Kubernetes マスターをリフレッシュして復元します。

1.  [アカウントにログインします。 該当する場合は、適切なリソース・グループをターゲットにします。 クラスターのコンテキストを設定します。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
2.  Kubernetes マスターを最新表示して復元します。

    ```
    ibmcloud ks apiserver-refresh
    ```
    {: pre}


<br />




## クラスターが保留状態のままである
{: #cs_cluster_pending}

{: tsSymptoms}
デプロイしたクラスターが、保留状態のままで開始されません。

{: tsCauses}
クラスターを作成したばかりの場合は、まだワーカー・ノードが構成中の可能性があります。 既にしばらく待機している場合は、VLAN が無効である可能性があります。

{: tsResolve}

以下のいずれかの解決策を試してください。
  - `ibmcloud ks clusters` を実行して、クラスターの状況を確認します。 その後、`ibmcloud ks workers --cluster <cluster_name>` を実行して、ワーカー・ノードがデプロイされていることを確認します。
  - VLAN が有効かどうかを確認します。 VLAN が有効であるためには、ローカル・ディスク・ストレージを持つワーカーをホストできるインフラストラクチャーに VLAN が関連付けられている必要があります。 `ibmcloud ks vlans --zone <zone>` を実行して [VLAN をリスト](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlans)できます。VLAN がリストに表示されない場合、その VLAN は有効ではありません。 別の VLAN を選択してください。

<br />


## クラスター作成エラーにより、レジストリーからイメージをプルできない
{: #ts_image_pull_create}

{: tsSymptoms}
クラスターを作成した場合に、以下のようなエラー・メッセージが表示されました。


```
Your cluster cannot pull images from the IBM Cloud Container Registry 'icr.io' domains because an IAM access policy could not be created. Make sure that you have the IAM Administrator platform role to IBM Cloud Container Registry. Then, create an image pull secret with IAM credentials to the registry by running 'ibmcloud ks cluster-pull-secret-apply'.
```
{: screen}

{: tsCauses}
クラスターの作成中に、クラスターに対してサービス ID が作成され、そのサービス ID に、{{site.data.keyword.registrylong_notm}} に対する**リーダー**のサービス・アクセス・ポリシーが割り当てられます。次に、クラスターによる {{site.data.keyword.registrylong_notm}} からのイメージのプルが許可されるように、このサービス ID の API キーが生成されて[イメージ・プル・シークレット](/docs/containers?topic=containers-images#cluster_registry_auth)に保管されます。

クラスターの作成時に**リーダー**のサービス・アクセス・ポリシーをサービス ID に正常に割り当てるには、{{site.data.keyword.registrylong_notm}} に対する**管理者**のプラットフォーム・アクセス・ポリシーが必要です。

{: tsResolve}

手順:
1.  アカウント所有者がユーザーに {{site.data.keyword.registrylong_notm}} に対する**管理者**の役割を付与していることを確認します。
    ```
    ibmcloud iam user-policy-create <your_user_email> --service-name container-registry --roles Administrator
    ```
    {: pre}
2.  [`ibmcloud ks cluster-pull-secret-apply` コマンドを使用して](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_pull_secret_apply)、適切なレジストリー資格情報を持つイメージ・プル・シークレットを再作成します。

<br />


## `ImagePullBackOff` または許可エラーにより、レジストリーからイメージをプルできない
{: #ts_image_pull}

{: tsSymptoms}

{{site.data.keyword.registrylong_notm}} からイメージをプルするワークロードをデプロイすると、ポッドが **`ImagePullBackOff`** 状況で失敗します。

```
kubectl get pods
```
{: pre}

```
NAME         READY     STATUS             RESTARTS   AGE
<pod_name>   0/1       ImagePullBackOff   0          2m
```
{: screen}

ポッドに describe を実行すると、以下のような認証エラーが表示されます。

```
kubectl describe pod <pod_name>
```
{: pre}

```
Failed to pull image "<region>.icr.io/<namespace>/<image>:<tag>" ... unauthorized: authentication required
Failed to pull image "<region>.icr.io/<namespace>/<image>:<tag>" ... 401 Unauthorized
```
{: screen}

```
Failed to pull image "registry.ng.bluemix.net/<namespace>/<image>:<tag>" ... unauthorized: authentication required
Failed to pull image "registry.ng.bluemix.net/<namespace>/<image>:<tag>" ... 401 Unauthorized
```
{: screen}

{: tsCauses}
クラスターでは、[イメージ・プル・シークレット](/docs/containers?topic=containers-images#cluster_registry_auth)に保管されている API キーまたはトークンを使用して、クラスターが {{site.data.keyword.registrylong_notm}} からイメージをプルすることが許可されます。 デフォルトで、新規クラスターでは API キーを使用するイメージ・プル・シークレットが作成されるので、クラスターは、`default` Kubernetes 名前空間にデプロイされるコンテナーのイメージをどの地域の `icr.io` レジストリーからもプルできます。 クラスターで、トークンを使用するイメージ・プル・シークレットが使用されている場合は、{{site.data.keyword.registrylong_notm}} へのデフォルト・アクセスは、非推奨の `<region>.registry.bluemix.net` ドメインを使用する特定の地域レジストリーのみに制限されます。

{: tsResolve}

1.  デプロイメント YAML ファイルでイメージの名前とタグが正しく使用されていることを確認します。
    ```
    ibmcloud cr images
    ```
    {: pre}
2.  失敗するポッドのポッド構成ファイルを取得して、`imagePullSecrets` セクションを探します。
    ```
    kubectl get pod <pod_name> -o yaml
    ```
    {: pre}

    出力例:
    ```
    ...
    imagePullSecrets:
    - name: bluemix-default-secret
    - name: bluemix-default-secret-regional
    - name: bluemix-default-secret-international
    - name: default-us-icr-io
    - name: default-uk-icr-io
    - name: default-de-icr-io
    - name: default-au-icr-io
    - name: default-jp-icr-io
    - name: default-icr-io
    ...
    ```
    {: screen}
3.  イメージ・プル・シークレットが 1 つもリストされない場合は、名前空間にイメージ・プル・シークレットをセットアップします。
    1.  [`default` Kubernetes 名前空間から、ワークロードをデプロイする名前空間に、イメージ・プル・シークレットをコピーします](/docs/containers?topic=containers-images#copy_imagePullSecret)。
    2.  [この Kubernetes 名前空間のサービス・アカウントにイメージ・プル・シークレットを追加して](/docs/containers?topic=containers-images#store_imagePullSecret)、名前空間内のすべてのポッドがイメージ・プル・シークレット資格情報を使用できるようにします。
4.  イメージ・プル・シークレットがリストされる場合は、コンテナー・レジストリーのアクセスのために使用している資格情報のタイプを判別します。
    *   **非推奨**: シークレットの名前に `bluemix` が入っている場合は、非推奨の `registry.<region>.bluemix.net` ドメイン・ネームで認証するレジストリー・トークンを使用しています。 [トークンを使用するイメージ・プル・シークレットのトラブルシューティング](#ts_image_pull_token)に進みます。
    *   シークレットの名前に `icr` が入っている場合は、`icr.io` ドメイン・ネームで認証する API キーを使用しています。 [API キーを使用するイメージ・プル・シークレットのトラブルシューティング](#ts_image_pull_apikey)に進みます。
    *   両方のタイプのシークレットがある場合は、両方の認証方式を使用しています。 今後は、コンテナー・イメージのデプロイメント YAML で `icr.io` ドメイン・ネームを使用します。 [API キーを使用するイメージ・プル・シークレットのトラブルシューティング](#ts_image_pull_apikey)に進みます。

<br>
<br>

**API キーを使用するイメージ・プル・シークレットのトラブルシューティング**</br>
{: #ts_image_pull_apikey}

ポッド構成で、API キーを使用するイメージ・プル・シークレットが使用されている場合、API キー資格情報が正しくセットアップされていることを確認します。
{: shortdesc}

以下の手順では、API キーにサービス ID の資格情報が保管されていることを想定しています。 個別ユーザーの API キーを使用するイメージ・プル・シークレットをセットアップしている場合、ユーザーの {{site.data.keyword.Bluemix_notm}} IAM 許可および資格情報を確認する必要があります。
{: note}

1.  **説明**を確認して、イメージ・プル・シークレット用に API キーで使用されるサービス ID を見つけます。 クラスターで作成されるサービス ID は、説明が `ID for <cluster_name>` となっており、`default` Kubernetes 名前空間で使用されます。 別の Kubernetes 名前空間にアクセスする、または {{site.data.keyword.Bluemix_notm}} IAM 許可を変更するなどのために、別のサービス ID を作成した場合は、説明はカスタマイズされています。
    ```
    ibmcloud iam service-ids
    ```
    {: pre}

    出力例:
    ```
    UUID                Name               Created At              Last Updated            Description                                                                                                                                                                                         Locked     
    ServiceId-aa11...   <service_ID_name>  2019-02-01T19:01+0000   2019-02-01T19:01+0000   ID for <cluster_name>                                                                                                                                         false   
    ServiceId-bb22...   <service_ID_name>  2019-02-01T19:01+0000   2019-02-01T19:01+0000   Service ID for IBM Cloud Container Registry in Kubernetes cluster <cluster_name> namespace <kube_namespace>                                                                                                                                         false    
    ```
    {: screen}
2.  サービス ID に少なくとも {{site.data.keyword.Bluemix_notm}} IAM **リーダー**の[サービス・アクセス役割ポリシーが {{site.data.keyword.registryshort_notm}}](/docs/services/Registry?topic=registry-user#create) に対して割り当てられていることを確認します。 サービス ID に**リーダー**のサービス役割が割り当てられていない場合は、[IAM ポリシーを編集します](/docs/iam?topic=iam-serviceidpolicy#access_edit)。 ポリシーが正しい場合は、次のステップに進み、資格情報が有効かどうかを確認してください。
    ```
    ibmcloud iam service-policies <service_ID_name>
    ```
    {: pre}

    出力例:
    ```              
    Policy ID:   a111a111-b22b-333c-d4dd-e555555555e5   
    Roles:       Reader   
    Resources:                            
                  Service Name       container-registry      
                  サービス・インスタンス         
                  地域                  
                  Resource Type      namespace      
                  Resource           <registry_namespace>  
    ```
    {: screen}  
3.  イメージ・プル・シークレットの資格情報が有効かどうかを確認します。
    1.  イメージ・プル・シークレットの構成を取得します。 ポッドが `default` 名前空間にない場合は、`-n` フラグを含めます。
        ```
        kubectl get secret <image_pull_secret_name> -o yaml [-n <namespace>]
        ```
        {: pre}
    2.  出力で、`.dockercfg` フィールドの base64 エンコード値をコピーします。
        ```
        apiVersion: v1
        kind: Secret
        data:
          .dockercfg: eyJyZWdp...==
        ...
        ```
        {: screen}
    3.  base64 ストリングをデコードします。 例えば、OS X の場合、以下のコマンドを実行できます。
        ```
        echo -n "<base64_string>" | base64 --decode
        ```
        {: pre}

        出力例:
        ```
        {"auths":{"<region>.icr.io":{"username":"iamapikey","password":"<password_string>","email":"<name@abc.com>","auth":"<auth_string>"}}}
        ```
        {: screen}
    4.  イメージ・プル・シークレットの地域レジストリー・ドメイン・ネームと、コンテナー・イメージで指定されているドメイン・ネームを比較します。 デフォルトで、新規クラスターでは `default` Kubernetes 名前空間で実行されるコンテナーの地域レジストリー・ドメイン・ネームごとにイメージ・プル・シークレットが作成されます。 ただし、デフォルト設定を変更した場合や、別の Kubernetes 名前空間を使用している場合は、地域レジストリーのイメージ・プル・シークレットがない可能性があります。 地域レジストリー・ドメイン・ネームの[イメージ・プル・シークレットをコピーします](/docs/containers?topic=containers-images#copy_imagePullSecret)。
    5.  イメージ・プル・シークレットの `username` と `password` を使用して、ローカル・マシンからレジストリーにログインします。 ログインできない場合は、サービス ID を修正する必要があるかもしれません。
        ```
        docker login -u iamapikey -p <password_string> <region>.icr.io
        ```
        {: pre}
        1.  `default` Kubernetes 名前空間で実行されるコンテナーのクラスター・サービス ID、{{site.data.keyword.Bluemix_notm}} IAM ポリシー、API キー、およびイメージ・プル・シークレットを再作成します。
            ```
            ibmcloud ks cluster-pull-secret-apply --cluster <cluster_name_or_ID>
            ```
            {: pre}
        2.  `default` Kubernetes 名前空間のデプロイメントを再作成します。 それでも許可エラー・メッセージが表示される場合は、新規イメージ・プル・シークレットでステップ 1 から 5 を繰り返します。 それでもログインできない場合は、[Slack で IBM チームに連絡を取るか、{{site.data.keyword.Bluemix_notm}} サポート・ケースを開いてください](#clusters_getting_help)。
    6.  ログインに成功した場合は、イメージをローカルにプルします。 コマンドが `access denied` エラーで失敗する場合は、レジストリー・アカウントがクラスターとは別の {{site.data.keyword.Bluemix_notm}} アカウントにあります。 [他のアカウントにあるイメージにアクセスするためのイメージ・プル・シークレットを作成します](/docs/containers?topic=containers-images#other_registry_accounts)。 ローカル・マシンにイメージをプルできる場合は、API キーに適切な許可がありますが、クラスター内の API セットアップが適切ではありません。 この問題はお客様には解決できません。 [Slack で IBM チームに連絡を取るか、{{site.data.keyword.Bluemix_notm}} サポート・ケースを開いてください](#clusters_getting_help)。
        ```
        docker pull <region>icr.io/<namespace>/<image>:<tag>
        ```
        {: pre}

<br>
<br>

**非推奨: トークンを使用するイメージ・プル・シークレットのトラブルシューティング**</br>
{: #ts_image_pull_token}

ポッド構成で、トークンを使用するイメージ・プル・シークレットが使用されている場合、トークン資格情報が有効であることを確認します。
{: shortdesc}

トークンを使用して {{site.data.keyword.registrylong_notm}} へのアクセス権限をクラスターに与えるこの方式は、`registry.bluemix.net` ドメイン名ではサポートされていますが、非推奨になっています。 代わりに、[API キー方式を使用して](/docs/containers?topic=containers-images#cluster_registry_auth)、クラスターの新規 `icr.io` レジストリー・ドメイン・ネームへのアクセスを許可してください。
{: deprecated}

1.  イメージ・プル・シークレットの構成を取得します。 ポッドが `default` 名前空間にない場合は、`-n` フラグを含めます。
    ```
    kubectl get secret <image_pull_secret_name> -o yaml [-n <namespace>]
    ```
    {: pre}
2.  出力で、`.dockercfg` フィールドの base64 エンコード値をコピーします。
    ```
    apiVersion: v1
        kind: Secret
        data:
      .dockercfg: eyJyZWdp...==
    ...
    ```
    {: screen}
3.  base64 ストリングをデコードします。 例えば、OS X の場合、以下のコマンドを実行できます。
    ```
    echo -n "<base64_string>" | base64 --decode
    ```
    {: pre}

    出力例:
    ```
    {"auths":{"registry.<region>.bluemix.net":{"username":"token","password":"<password_string>","email":"<name@abc.com>","auth":"<auth_string>"}}}
    ```
    {: screen}
4.  レジストリー・ドメイン・ネームと、コンテナー・イメージで指定されているドメイン・ネームを比較します。 例えば、イメージ・プル・シークレットで `registry.ng.bluemix.net` ドメインへのアクセスが許可されているが、`registry.eu-de.bluemix.net` に保管されているイメージが指定されている場合、`registry.eu-de.bluemix.net` の[イメージ・プル・シークレットで使用するトークンを作成](/docs/containers?topic=containers-images#token_other_regions_accounts)する必要があります。
5.  イメージ・プル・シークレットの `username` と `password` を使用して、ローカル・マシンからレジストリーにログインします。 ログインできない場合は、解決できない問題がトークンで発生しています。 [Slack で IBM チームに連絡を取るか、{{site.data.keyword.Bluemix_notm}} サポート・ケースを開いてください](#clusters_getting_help)。
    ```
    docker login -u token -p <password_string> registry.<region>.bluemix.net
    ```
    {: pre}
6.  ログインに成功した場合は、イメージをローカルにプルします。 コマンドが `access denied` エラーで失敗する場合は、レジストリー・アカウントがクラスターとは別の {{site.data.keyword.Bluemix_notm}} アカウントにあります。 [他のアカウントにあるイメージにアクセスするためのイメージ・プル・シークレットを作成します](/docs/containers?topic=containers-images#token_other_regions_accounts)。 コマンドが成功した場合は、[Slack で IBM チームに連絡を取るか、{{site.data.keyword.Bluemix_notm}} サポート・ケースを開いてください](#clusters_getting_help)。
    ```
    docker pull registry.<region>.bluemix.net/<namespace>/<image>:<tag>
    ```
    {: pre}

<br />


## ポッドが保留状態のままである
{: #cs_pods_pending}

{: tsSymptoms}
`kubectl get pods` を実行すると、ポッドの状態が **Pending** になる場合があります。

{: tsCauses}
Kubernetes クラスターを作成したばかりの場合は、まだワーカー・ノードが構成中の可能性があります。

このクラスターが以前から存在するものである場合:
*  ポッドをデプロイするための十分な容量がクラスター内にない可能性があります。
*  ポッドがリソースの要求または制限を超えた可能性があります。

{: tsResolve}
このタスクには、クラスターに対する {{site.data.keyword.Bluemix_notm}} IAM [**管理者**プラットフォーム役割](/docs/containers?topic=containers-users#platform)と、すべての名前空間に対する[**管理者**サービス役割](/docs/containers?topic=containers-users#platform)が必要です。

Kubernetes クラスターを作成したばかりの場合は、以下のコマンドを実行して、ワーカー・ノードが初期化するまで待ちます。

```
kubectl get nodes
```
{: pre}

クラスターが以前から存在するものである場合は、クラスターの容量を確認します。

1.  デフォルトのポート番号でプロキシーを設定します。

  ```
  kubectl proxy
  ```
   {: pre}

2.  Kubernetes ダッシュボードを開きます。

  ```
  http://localhost:8001/ui
  ```
  {: pre}

3.  ポッドをデプロイするための十分な容量がクラスター内にあるか確認します。

4.  クラスターに十分な容量がない場合は、ワーカー・プールをサイズ変更してノードをさらに追加します。

    1.  ワーカー・プールの現在のサイズとマシン・タイプを確認して、サイズ変更するワーカー・プールを決定します。

        ```
        ibmcloud ks worker-pools
        ```
        {: pre}

    2.  ワーカー・プールをサイズ変更して、そのプールが及ぶ範囲のゾーンごとにノードをさらに追加します。

        ```
        ibmcloud ks worker-pool-resize --worker-pool <worker_pool> --cluster <cluster_name_or_ID> --size-per-zone <workers_per_zone>
        ```
        {: pre}

5.  オプション: ポッドのリソース要求を確認します。

    1.  `resources.requests` 値がワーカー・ノードの容量を超えていないことを確認します。 例えば、ポッド要求が `cpu: 4000m` (つまり 4 コア) で、ワーカー・ノード・サイズが 2 コアのみの場合、ポッドをデプロイすることはできません。

        ```
        kubectl get pod <pod_name> -o yaml
        ```
        {: pre}

    2.  要求が使用可能な容量を超える場合は、要求を満たすワーカー・ノードを使用して[新規ワーカー・プールを追加](/docs/containers?topic=containers-add_workers#add_pool)します。

6.  ワーカー・ノードが完全にデプロイされたのにまだポッドが **pending** 状態のままである場合は、[Kubernetes の資料![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/tasks/debug-application-cluster/debug-pod-replication-controller/#my-pod-stays-pending) を参照して、ポッドの pending 状態のトラブルシューティングを行ってください。

<br />


## コンテナーが開始しない
{: #containers_do_not_start}

{: tsSymptoms}
ポッドはクラスターに正常にデプロイされましたが、コンテナーが開始しません。

{: tsCauses}
レジストリーの割り当て量に到達すると、コンテナーが開始しないことがあります。

{: tsResolve}
[{{site.data.keyword.registryshort_notm}} 内のストレージを解放してください。](/docs/services/Registry?topic=registry-registry_quota#registry_quota_freeup)

<br />


## ポッドの再始動が繰り返し失敗するまたはポッドが予期せず除去される
{: #pods_fail}

{: tsSymptoms}
ポッドは正常ですが、予期せずに除去されたり、再始動ループから抜け出せなくなったりします。

{: tsCauses}
コンテナーがリソース制限を超えているか、または、ポッドがより優先度の高いポッドに置き換えられている可能性があります。

{: tsResolve}
コンテナーがリソース制限のために強制終了させられているかどうかを確認するには、以下のようにします。
<ol><li>ポッドの名前を取得します。 ラベルを使用していた場合は、それを使用して結果をフィルタリングできます。<pre class="pre"><code>kubectl get pods --selector='app=wasliberty'</code></pre></li>
<li>ポッドに対して describe を実行して、**Restart Count** を検索します。<pre class="pre"><code>kubectl describe pod</code></pre></li>
<li>ポッドが短時間で何回も再始動していた場合は、その状況をフェッチします。 <pre class="pre"><code>kubectl get pod -o go-template={{range.status.containerStatuses}}{{"Container Name: "}}{{.name}}{{"\r\nLastState: "}}{{.lastState}}{{end}}</code></pre></li>
<li>理由を確認します。 例えば、`OOM Killed` とあれば「メモリー不足」を意味し、コンテナーがリソース制限によってクラッシュしていることがわかります。</li>
<li>リソースを十分に使用できるように、クラスターに容量を追加します。</li></ol>

<br>

ポッドがより優先度の高いポッドに置き換えられているかどうかを確認するには、以下のようにします。
1.  ポッドの名前を取得します。

    ```
    kubectl get pods
    ```
    {: pre}

2.  ポッドの YAML を記述します。

    ```
    kubectl get pod <pod_name> -o yaml
    ```
    {: pre}

3.  `priorityClassName` フィールドを調べます。

    1.  `priorityClassName` フィールドに値がない場合、対象ポッドの優先度クラスは `globalDefault` です。 クラスター管理者が `globalDefault` 優先度クラスを設定しなかった場合、デフォルトはゼロ (0)、つまり最も低い優先度です。 より高い優先度クラスを持つポッドによって、対象ポッドが置き換わる (または削除される) ことがあります。

    2.  `priorityClassName` フィールドに値がある場合は、優先度クラスを取得します。

        ```
        kubectl get priorityclass <priority_class_name> -o yaml
        ```
        {: pre}

    3.  `value` フィールドを書き留め、ポッドの優先度を調べます。

4.  クラスター内の既存の優先度クラスをリストします。

    ```
    kubectl get priorityclasses
    ```
    {: pre}

5.  それぞれの優先度クラスについて YAML ファイルを取得し、`value` フィールドを書き留めます。

    ```
    kubectl get priorityclass <priority_class_name> -o yaml
    ```
    {: pre}

6.  対象ポッドの優先度クラスの値を他の優先度クラスの値と比較し、優先度が他のクラスより高いか低いかを調べます。

7.  クラスター内の他のポッドについてもステップ 1 から 3 を繰り返し、使用されている優先度クラスを確認します。 これらの他のポッドの優先度クラスが対象ポッドより高い場合、対象ポッドおよび優先度が高い各ポッドに十分なリソースが用意されるまで、対象ポッドはプロビジョンされません。

8.  クラスター管理者に連絡してクラスターにより多くの容量を追加してもらい、適切な優先度クラスが割り当てられていることを確認します。

<br />


## 更新した構成値で Helm チャートをインストールできない
{: #cs_helm_install}

{: tsSymptoms}
`helm install -f config.yaml --namespace=kube-system --name=<release_name> ibm/<chart_name>` を実行して、更新した Helm チャートをインストールしようとすると、エラー・メッセージ `Error: failed to download "ibm/<chart_name>"` を受け取ります。

{: tsCauses}
Helm インスタンス内の {{site.data.keyword.Bluemix_notm}} リポジトリーの URL が正しくない可能性があります。

{: tsResolve}
Helm チャートをトラブルシューティングするには、以下のようにします。

1. Helm インスタンス内に現在存在するリポジトリーをリストします。

    ```
    helm repo list
    ```
    {: pre}

2. 出力で、{{site.data.keyword.Bluemix_notm}} リポジトリー `ibm` の URL が `https://icr.io/helm/iks-charts` であることを確認します。

    ```
    NAME    URL
    stable  https://kubernetes-charts.storage.googleapis.com
    local   http://127.0.0.1:8888/charts
    ibm     https://icr.io/helm/iks-charts
    ```
    {: screen}

    * URL が正しくない場合は、以下のようにします。

        1. {{site.data.keyword.Bluemix_notm}} リポジトリーを削除します。

            ```
            helm repo remove ibm
            ```
            {: pre}

        2. {{site.data.keyword.Bluemix_notm}} リポジトリーを再度追加します。

            ```
            helm repo add iks-charts https://icr.io/helm/iks-charts
            ```
            {: pre}

    * URL が正しい場合は、リポジトリーから最新の更新を取得します。

        ```
        helm repo update
        ```
        {: pre}

3. 取得した更新を使用して、Helm チャートをインストールします。

    ```
    helm install -f config.yaml --namespace=kube-system --name=<release_name> ibm/<chart_name>
    ```
    {: pre}

<br />


## クラスターで Helm tiller をインストールできないまたはパブリック・イメージからコンテナーをデプロイできない
{: #cs_tiller_install}

{: tsSymptoms}

Helm tiller のインストールを試行した場合、または DockerHub などのパブリック・レジストリーからイメージをデプロイした場合に、以下のようなエラーでインストールが失敗します。

```
Failed to pull image "gcr.io/kubernetes-helm/tiller:v2.12.0": rpc error: code = Unknown desc = failed to resolve image "gcr.io/kubernetes-helm/tiller:v2.12.0": no available registry endpoint:
```
{: screen}

{: tsCauses}
カスタム・ファイアウォールをセットアップしたり、カスタム Calico ポリシーを指定したり、またはイメージが保管されているコンテナー・レジストリーへのパブリック・ネットワーク接続をブロックするプライベート・サービス・エンドポイントを使用することによってプライベート専用クラスターを作成したりしている可能性があります。

{: tsResolve}
- カスタム・ファイアウォールを使用しているか、カスタム Calico ポリシーを設定している場合は、ワーカー・ノードと、イメージが保管されているコンテナー・レジストリーとの間のアウトバウンドおよびインバウンドのネットワーク・トラフィックを許可します。 イメージが {{site.data.keyword.registryshort_notm}} に保管されている場合は、[クラスターからインフラストラクチャー・リソースや他のサービスへのアクセスの許可](/docs/containers?topic=containers-firewall#firewall_outbound)で必要なポートを確認してください。
- プライベート・サービス・エンドポイントのみを有効にしてプライベート・クラスターを作成した場合は、そのクラスターの[パブリック・サービス・エンドポイントを有効にする](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_disable)ことができます。 パブリック接続を開かずにプライベート・クラスターに Helm チャートをインストールする場合、[Tiller を使用しても](/docs/containers?topic=containers-helm#private_local_tiller)、[Tiller を使用しなくても](/docs/containers?topic=containers-helm#private_install_without_tiller)、Helm をインストールできます。

<br />


## ヘルプとサポートの取得
{: #clusters_getting_help}

まだクラスターに問題がありますか?
{: shortdesc}

-  `ibmcloud` CLI およびプラグインの更新が使用可能になると、端末に通知が表示されます。 使用可能なすべてのコマンドおよびフラグを使用できるように、CLI を最新の状態に保つようにしてください。
-   {{site.data.keyword.Bluemix_notm}} が使用可能かどうかを確認するために、[{{site.data.keyword.Bluemix_notm}} 状況ページ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン") を確認します](https://cloud.ibm.com/status?selected=status)。
-   [{{site.data.keyword.containerlong_notm}} Slack ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://ibm-container-service.slack.com) に質問を投稿します。
    {{site.data.keyword.Bluemix_notm}} アカウントに IBM ID を使用していない場合は、この Slack への[招待を要求](https://bxcs-slack-invite.mybluemix.net/)してください。
    {: tip}
-   フォーラムを確認して、同じ問題が他のユーザーで起こっているかどうかを調べます。 フォーラムを使用して質問するときは、{{site.data.keyword.Bluemix_notm}} 開発チームの目に止まるように、質問にタグを付けてください。
    -   {{site.data.keyword.containerlong_notm}} を使用したクラスターまたはアプリの開発やデプロイに関する技術的な質問がある場合は、[Stack Overflow![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://stackoverflow.com/questions/tagged/ibm-cloud+containers) に質問を投稿し、`ibm-cloud`、`kubernetes`、`containers` のタグを付けてください。
    -   サービスや概説の説明について質問がある場合は、[IBM Developer Answers Answers ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix) フォーラムを使用してください。 `ibm-cloud` と `containers` のタグを含めてください。
    フォーラムの使用について詳しくは、[ヘルプの取得](/docs/get-support?topic=get-support-getting-customer-support#using-avatar)を参照してください。
-   ケースを開いて、IBM サポートに連絡してください。 IBM サポート・ケースを開く方法や、サポート・レベルとケースの重大度については、[サポートへのお問い合わせ](/docs/get-support?topic=get-support-getting-customer-support)を参照してください。
問題を報告する際に、クラスター ID も報告してください。 クラスター ID を取得するには、`ibmcloud ks clusters` を実行します。 また、[{{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool](/docs/containers?topic=containers-cs_troubleshoot#debug_utility) を使用して、クラスターから関連情報を収集してエクスポートし、IBM サポートと情報を共有することができます。
{: tip}

