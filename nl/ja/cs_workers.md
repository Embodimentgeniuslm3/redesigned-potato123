---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-06"

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



# クラスターへのワーカー・ノードとゾーンの追加
{: #add_workers}

アプリの可用性を高めるために、クラスター内の既存の 1 つまたは複数のゾーンにワーカー・ノードを追加できます。 ゾーンの障害からアプリケーションを保護するには、ゾーンをクラスターに追加します。
{:shortdesc}

クラスターを作成すると、ワーカー・ノードがワーカー・プールにプロビジョンされます。 クラスターの作成後に、さらにワーカー・ノードをプールに追加するには、プールをサイズ変更するか、別のワーカー・プールを追加します。 デフォルトでは、ワーカー・プールは 1 つのゾーン内に存在します。 1 つのゾーン内のみにワーカー・プールがあるクラスターを、単一ゾーン・クラスターと呼びます。 クラスターに別のゾーンを追加すると、ワーカー・プールはそれらのゾーンにまたがって存在します。 複数のゾーン間にワーカー・プールが分散しているクラスターを、複数ゾーン・クラスターと呼びます。

複数ゾーン・クラスターの場合は、そのワーカー・ノードのリソースをバランスが取れた状態に維持してください。 すべてのワーカー・プールが同じゾーン間に分散されていることを確認し、ワーカーの追加/削除を行う場合は、個々のノードを追加するのではなく、プールのサイズを変更してください。
{: tip}

始める前に、[**オペレーター**または**管理者**の {{site.data.keyword.Bluemix_notm}} IAM プラットフォーム役割](/docs/containers?topic=containers-users#platform)があることを確認してください。 次に、以下のいずれかのセクションを選択してください。
  * [クラスター内の既存のワーカー・プールのサイズを変更してワーカー・ノードを追加する](#resize_pool)
  * [ワーカー・プールをクラスターに追加してワーカー・ノードを追加する](#add_pool)
  * [ゾーンをクラスターに追加し、複数のゾーン間でワーカー・プール内のワーカー・ノードを複製する](#add_zone)
  * [非推奨: スタンドアロン・ワーカー・ノードをクラスターに追加する](#standalone)

ワーカー・プールをセットアップしたら、ワークロード・リソース要求に基づいてワーカー・プールにワーカー・ノードを自動的に追加または削除するように[クラスター自動スケーリング機能をセットアップ](/docs/containers?topic=containers-ca#ca)します。
{:tip}

## 既存のワーカー・プールのサイズを変更してワーカー・ノードを追加する
{: #resize_pool}

ワーカー・プールが 1 つのゾーンにあるか複数のゾーン間に分散しているかにかかわらず、既存のワーカー・プールのサイズを変更して、クラスター内のワーカー・ノードの数を追加したり減らしたりできます。
{: shortdesc}

例えば、 1 つのワーカー・プールにゾーンあたり 3 つのワーカー・ノードがあるクラスターについて検討してみましょう。
* クラスターが単一ゾーンで `dal10` にある場合は、ワーカー・プールは `dal10` 内に 3 つのワーカー・ノードを持っています。 クラスターには合計で 3 つのワーカー・ノードがあります。
* クラスターが複数ゾーンで `dal10` と `dal12` にある場合、ワーカー・プールは `dal10` 内に 3 つのワーカー・ノードと `dal12` 内に 3 つのワーカー・ノードを持っています。 クラスターには合計で 6 つのワーカー・ノードがあります。

ベア・メタル・ワーカー・プールの場合は、月単位で課金されることに留意してください。 サイズを変更して増減すると、その月のコストに影響します。
{: tip}

ワーカー・プールのサイズを変更するには、そのワーカー・プールが各ゾーン内にデプロイするワーカー・ノードの数を変更します。

1. サイズ変更するワーカー・プールの名前を取得します。
    ```
    ibmcloud ks worker-pools --cluster <cluster_name_or_ID>
    ```
    {: pre}

2. 各ゾーン内にデプロイするワーカー・ノードの数を指定して、ワーカー・プールのサイズを変更します。 最小値は 1 です。
    ```
    ibmcloud ks worker-pool-resize --cluster <cluster_name_or_ID> --worker-pool <pool_name>  --size-per-zone <number_of_workers_per_zone>
    ```
    {: pre}

3. ワーカー・プールがサイズ変更されたことを確認します。
    ```
    ibmcloud ks workers --cluster <cluster_name_or_ID> --worker-pool <pool_name>
    ```
    {: pre}

    `dal10` と `dal12` の 2 つのゾーン内にあり、ゾーンあたり 2 つのワーカー・ノードにサイズ変更されるワーカー・プールの出力例:
    ```
    ID                                                 Public IP        Private IP      Machine Type      State    Status  Zone    Version
    kube-dal10-crb20b637238ea471f8d4a8b881aae4962-w7   169.xx.xxx.xxx   10.xxx.xx.xxx   b3c.4x16          normal   Ready   dal10   1.8.6_1504
    kube-dal10-crb20b637238ea471f8d4a8b881aae4962-w8   169.xx.xxx.xxx   10.xxx.xx.xxx   b3c.4x16          normal   Ready   dal10   1.8.6_1504
    kube-dal12-crb20b637238ea471f8d4a8b881aae4962-w9   169.xx.xxx.xxx   10.xxx.xx.xxx   b3c.4x16          normal   Ready   dal12   1.8.6_1504
    kube-dal12-crb20b637238ea471f8d4a8b881aae4962-w10  169.xx.xxx.xxx   10.xxx.xx.xxx   b3c.4x16          normal   Ready   dal12   1.8.6_1504
    ```
    {: screen}

## 新しいワーカー・プールを作成してワーカー・ノードを追加する
{: #add_pool}

新しいワーカー・プールを作成して、クラスターにワーカー・ノードを追加できます。
{:shortdesc}

1. クラスターの**ワーカー・ゾーン**を取得し、ワーカー・プール内のワーカー・ノードをデプロイするゾーンを選択します。 単一ゾーン・クラスターの場合は、**Worker Zones** フィールドに表示されるゾーンを使用する必要があります。 複数ゾーン・クラスターの場合は、クラスターの既存の**ワーカー・ゾーン**を選択することも、クラスターが存在する地域の[複数ゾーンの大都市ロケーション](/docs/containers?topic=containers-regions-and-zones#zones)のゾーンを追加することもできます。 `ibmcloud ks zones` を実行して、使用可能なゾーンをリストできます。
   ```
   ibmcloud ks cluster-get --cluster <cluster_name_or_ID>
   ```
   {: pre}

   出力例:
   ```
   ...
   Worker Zones: dal10, dal12, dal13
   ```
   {: screen}

2. ゾーンごとに、使用可能なプライベート VLAN とパブリック VLAN をリストします。 使用するプライベート VLAN とパブリック VLAN をメモしておきます。 プライベート VLAN やパブリック VLAN がない場合は、ゾーンをワーカー・プールに追加したときに VLAN が自動的に作成されます。
   ```
   ibmcloud ks vlans --zone <zone>
   ```
   {: pre}

3.  ゾーンごとに、[ワーカー・ノードで選択可能なマシン・タイプ](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)を確認します。

    ```
    ibmcloud ks machine-types <zone>
    ```
    {: pre}

4. ワーカー・プールを作成します。 `--labels` オプションを指定して、プール内のワーカー・ノードにラベル `key=value` を自動的に付けます。ベアメタル・ワーカー・プールをプロビジョンする場合は、`--hardware dedicated` を指定します。
   ```
   ibmcloud ks worker-pool-create --name <pool_name> --cluster <cluster_name_or_ID> --machine-type <machine_type> --size-per-zone <number_of_workers_per_zone> --hardware <dedicated_or_shared> --labels <key=value>
   ```
   {: pre}

5. ワーカー・プールが作成されたことを確認します。
   ```
   ibmcloud ks worker-pools --cluster <cluster_name_or_ID>
   ```
   {: pre}

6. デフォルトでは、ワーカー・プールを追加すると、ゾーンのないプールが作成されます。 ワーカー・ノードをゾーン内にデプロイするには、以前に取得したゾーンをワーカー・プールに追加する必要があります。 ワーカー・ノードを複数のゾーン間に分散させる場合は、ゾーンごとにこのコマンドを繰り返します。
   ```
   ibmcloud ks zone-add --zone <zone> --cluster <cluster_name_or_ID> --worker-pools <pool_name> --private-vlan <private_VLAN_ID> --public-vlan <public_VLAN_ID>
   ```
   {: pre}

7. 追加したゾーンにワーカー・ノードがプロビジョンされたことを確認します。 状況が **provision_pending** から **normal** に変わったら、ワーカー・ノードの準備ができています。
   ```
   ibmcloud ks workers --cluster <cluster_name_or_ID> --worker-pool <pool_name>
   ```
   {: pre}

   出力例:
   ```
   ID                                                 Public IP        Private IP      Machine Type      State    Status  Zone    Version
   kube-dal10-crb20b637238ea471f8d4a8b881aae4962-w7   169.xx.xxx.xxx   10.xxx.xx.xxx   b3c.4x16          provision_pending   Ready   dal10   1.8.6_1504
   kube-dal10-crb20b637238ea471f8d4a8b881aae4962-w8   169.xx.xxx.xxx   10.xxx.xx.xxx   b3c.4x16          provision_pending   Ready   dal10   1.8.6_1504
   ```
   {: screen}

## ゾーンをワーカー・プールに追加してワーカー・ノードを追加する
{: #add_zone}

ゾーンを既存のワーカー・プールに追加して、1 つの地域内の複数のゾーンにクラスターを広げることができます。
{:shortdesc}

ゾーンをワーカー・プールに追加すると、ワーカー・プールで定義されているワーカー・ノードがその新しいゾーンにプロビジョンされ、今後のワークロード・スケジュールの対象に含められます。 {{site.data.keyword.containerlong_notm}} により、地域を表す `failure-domain.beta.kubernetes.io/region` ラベルと、ゾーンを表す `failure-domain.beta.kubernetes.io/zone` ラベルが各ワーク・ノードに自動的に追加されます。 Kubernetes スケジューラーは、これらのラベルを使用して、同じ地域内のゾーン間にポッドを分散させます。

クラスター内に複数のワーカー・プールがある場合は、すべてのプールにゾーンを追加して、ワーカー・ノードがクラスター全体に均等に分散されるようにします。

開始前に、以下のことを行います。
*  ワーカー・プールにゾーンを追加するには、そのワーカー・プールが[複数ゾーン対応ゾーン](/docs/containers?topic=containers-regions-and-zones#zones)内になければなりません。 ワーカー・プールが複数ゾーン対応ゾーン内にない場合は、[新しいワーカー・プールを作成](#add_pool)することを検討してください。
*  1 つのクラスターに複数の VLAN がある場合、同じ VLAN 上に複数のサブネットがある場合、または複数ゾーン・クラスターがある場合は、IBM Cloud インフラストラクチャー (SoftLayer) アカウントに対して[仮想ルーター機能 (VRF)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) を有効にして、ワーカー・ノードがプライベート・ネットワーク上で相互に通信できるようにする必要があります。 VRF を有効にするには、[IBM Cloud インフラストラクチャー (SoftLayer) のアカウント担当者に連絡してください](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#how-you-can-initiate-the-conversion)。 VRF の有効化が不可能または不要な場合は、[VLAN スパンニング](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning)を有効にしてください。 この操作を実行するには、**「ネットワーク」>「ネットワーク VLAN スパンニングの管理」**で設定する[インフラストラクチャー権限](/docs/containers?topic=containers-users#infra_access)が必要です。ない場合は、アカウント所有者に対応を依頼してください。 VLAN スパンニングが既に有効になっているかどうかを確認するには、`ibmcloud ks vlan-spanning-get<region>` [コマンド](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get)を使用します。

ゾーンをワーカー・ノードと一緒にワーカー・プールに追加するには、次のようにします。

1. 使用可能なゾーンをリストし、ワーカー・プールに追加するゾーンを選びます。 複数ゾーン対応ゾーンを選ばなければなりません。
   ```
   ibmcloud ks zones
   ```
   {: pre}

2. そのゾーン内の使用可能な VLAN をリストします。 プライベート VLAN やパブリック VLAN がない場合は、ゾーンをワーカー・プールに追加したときに VLAN が自動的に作成されます。
   ```
   ibmcloud ks vlans --zone <zone>
   ```
   {: pre}

3. クラスター内のワーカー・プールをリストし、それらの名前をメモします。
   ```
   ibmcloud ks worker-pools --cluster <cluster_name_or_ID>
   ```
   {: pre}

4. ゾーンをワーカー・プールに追加します。 ワーカー・プールが複数ある場合は、すべてのゾーンでクラスターのバランスが取れるように、すべてのワーカー・プールにゾーンを追加してください。 `<pool1_id_or_name,pool2_id_or_name,...>` を、すべてのワーカー・プールの名前をコンマ区切りで指定したリストに置き換えます。

    複数のワーカー・プールにゾーンを追加するには、その前にプライベート VLAN とパブリック VLAN がなければなりません。 そのゾーン内にプライベート VLAN もパブリック VLAN もない場合は、まず、いずれかのワーカー・プールにゾーンを追加して、プライベート VLAN とパブリック VLAN が作成されるようにしてください。 その後、作成されたプライベート VLAN とパブリック VLAN を指定して、他のワーカー・プールにゾーンを追加できます。
    {: note}

   ワーカー・プールごとに別の VLAN を使用する場合は、このコマンドを VLAN とその対応するワーカー・プールごとに繰り返します。 指定した VLAN に新しいワーカー・ノードが追加されますが、既存のワーカー・ノードの VLAN は変更されません。
   {: tip}
   ```
   ibmcloud ks zone-add --zone <zone> --cluster <cluster_name_or_ID> --worker-pools <pool1_name,pool2_name,...> --private-vlan <private_VLAN_ID> --public-vlan <public_VLAN_ID>
   ```
   {: pre}

5. ゾーンがクラスターに追加されたことを確認します。 出力の **Worker zones** フィールド内で、追加したゾーンを探します。 追加したゾーンに新しいワーカー・ノードがプロビジョンされたため、**Workers** フィールド内のワーカーの合計数が増えていることに注意してください。
  ```
  ibmcloud ks cluster-get --cluster <cluster_name_or_ID>
  ```
  {: pre}

  出力例:
  ```
  Name:                           mycluster
ID:                             df253b6025d64944ab99ed63bb4567b6
State:                          normal
Created:                        2018-09-28T15:43:15+0000
Location:                       dal10
Master URL:                     https://c3.<region>.containers.cloud.ibm.com:30426
Public Service Endpoint URL:    https://c3.<region>.containers.cloud.ibm.com:30426
Private Service Endpoint URL:   https://c3-private.<region>.containers.cloud.ibm.com:31140
Master Location:                Dallas
Master Status:                  Ready (21 hours ago)
Ingress Subdomain:              mycluster.us-south.containers.appdomain.cloud
Ingress Secret:                 mycluster
Workers:                        6
Worker Zones:                   dal10, dal12
Version:                        1.11.3_1524
Owner:                          owner@email.com
Resource Group ID:              a8a12accd63b437bbd6d58fb6a462ca7
Resource Group Name:            Default
  ```
  {: screen}

## 非推奨: スタンドアロン・ワーカー・ノードを追加する
{: #standalone}

ワーカー・プールが導入される前に作成されたクラスターがある場合は、非推奨のコマンドを使用してスタンドアロン・ワーカー・ノードを追加できます。
{: deprecated}

ワーカー・プールが導入された後にクラスターが作成された場合は、スタンドアロン・ワーカー・ノードを追加できません。 代わりに、[ワーカー・プールを作成する](#add_pool)か、[既存のワーカー・プールのサイズを変更する](#resize_pool)か、または[ゾーンをワーカー・プールに追加](#add_zone)して、ワーカー・ノードをクラスターに追加できます。
{: note}

1. 使用可能なゾーンをリストし、ワーカー・ノードを追加するゾーンを選びます。
   ```
   ibmcloud ks zones
   ```
   {: pre}

2. そのゾーン内の使用可能な VLAN をリストし、それらの ID をメモします。
   ```
   ibmcloud ks vlans --zone <zone>
   ```
   {: pre}

3. そのゾーン内で使用可能なマシン・タイプをリストします。
   ```
   ibmcloud ks machine-types --zone <zone>
   ```
   {: pre}

4. スタンドアロン・ワーカー・ノードをクラスターに追加します。 ベアメタル・マシン・タイプの場合、`dedicated` を指定します。
   ```
   ibmcloud ks worker-add --cluster <cluster_name_or_ID> --workers <number_of_worker_nodes> --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID> --machine-type <machine_type> --hardware <shared_or_dedicated>
   ```
   {: pre}

5. ワーカー・ノードが作成されたことを確認します。
   ```
   ibmcloud ks workers --cluster <cluster_name_or_ID>
   ```
   {: pre}

<br />


## 既存のワーカー・プールへのラベルの追加
{: #worker_pool_labels}

ワーカー・プールのラベルは、[ワーカー・プールの作成](#add_pool)時に割り当てるか、後で以下のステップに従って割り当てることができます。ワーカー・プールにラベルが付けられると、既存のワーカー・ノードと後続のワーカー・ノードのすべてにこのラベルが割り当てられます。ラベルを使用して、特定のワークロードをワーカー・プール内のワーカー・ノード ([ロード・バランサー・ネットワーク・トラフィック用のエッジ・ノード](/docs/containers?topic=containers-edge)など) にのみデプロイできます。
{: shortdesc}

開始前に、以下のことを行います。 [アカウントにログインします。 該当する場合は、適切なリソース・グループをターゲットにします。 クラスターのコンテキストを設定します。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  クラスター内のワーカー・プールをリストします。
      ```
    ibmcloud ks worker-pools --cluster <cluster_name_or_ID>
    ```
    {: pre}
2.  `key=value` ラベルを使用してワーカー・プールにラベルを付けるには、[PATCH ワーカー・プール API ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/PatchWorkerPool) を使用します。以下の JSON の例のように要求の本文をフォーマットします。
    ```
    {
      "labels": {"key":"value"},
      "state": "labels"
    }
    ```
    {: codeblock}
3.  ワーカー・プールとワーカー・ノードに、割り当てた `key=value` ラベルがあることを確認します。
    *   ワーカー・プールを確認するには、以下のようにします。
        ```
        ibmcloud ks worker-pool-get --cluster <cluster_name_or_ID> --worker-pool <worker_pool_name_or_ID>
        ```
        {: pre}
    *   ワーカー・ノードを確認するには、以下のようにします。
        1.  ワーカー・プール内のワーカー・ノードをリストし、**プライベート IP** をメモします。
            ```
            ibmcloud ks workers --cluster <cluster_name_or_ID> --worker-pool <worker_pool_name_or_ID>
            ```
            {: pre}
        2.  出力の **Labels** フィールドを確認します。
            ```
            kubectl describe node <worker_node_private_IP>
            ```
            {: pre}

ワーカー・プールにラベルを付けたら、[アプリ・デプロイメントでラベル](/docs/containers?topic=containers-app#label)を使用してワークロードをこれらのワーカー・ノードでのみ実行するか、[テイント ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/) を使用してデプロイメントがこれらのワーカー・ノードで実行されないようにすることができます。

<br />


## ワーカー・ノードの自動リカバリー
{: #planning_autorecovery}

`containerd`、`kubelet`、`kube-proxy`、および `calico` などの重要なコンポーネントは、Kubernetes ワーカー・ノードを正常に保つために適切に機能している必要があります。 時間の経過とともに、これらのコンポーネントが中断し、ワーカー・ノードが機能しない状態になることがあります。 機能しない状態のワーカー・ノードがあると、クラスターの合計処理能力が減少し、アプリにダウン時間が生じる可能性があります。
{:shortdesc}

[ワーカー・ノードのヘルス・チェックを構成し、Autorecovery を有効にする](/docs/containers?topic=containers-health#autorecovery)ことができます。 Autorecovery は、構成された検査に基づいて正常でないワーカー・ノードを検出すると、ワーカー・ノードの OS の再ロードのような修正アクションをトリガーします。 Autorecovery の仕組みについて詳しくは、[Autorecovery のブログ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/blogs/bluemix/2017/12/autorecovery-utilizes-consistent-hashing-high-availability/) を参照してください。
