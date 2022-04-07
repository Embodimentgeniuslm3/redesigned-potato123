---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-03"

keywords: kubernetes, iks

subcollection: containers

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}rwo
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:preview: .preview}



# IBM Cloud の IBM ブロック・ストレージへのデータの保管
{: #block_storage}

{{site.data.keyword.Bluemix_notm}} のブロック・ストレージは、Kubernetes 永続ボリューム (PV) を使用してアプリに追加できる高性能な永続 iSCSI ストレージです。 ワークロードの要件を満たす GB サイズと IOPS を考慮して、事前定義されたストレージ層の中から選択できます。 {{site.data.keyword.Bluemix_notm}} のブロック・ストレージが適切なストレージ・オプションかどうかを確認するには、[ストレージ・ソリューションの選択](/docs/containers?topic=containers-storage_planning#choose_storage_solution)を参照してください。 価格情報については、[課金](/docs/infrastructure/BlockStorage?topic=BlockStorage-About#billing)を参照してください。
{: shortdesc}

{{site.data.keyword.Bluemix_notm}} のブロック・ストレージは、標準クラスター専用です。 ご使用のクラスターがパブリック・ネットワークにアクセスできない場合は (ファイアウォール保護下のプライベート・クラスターや、プライベート・サービス・エンドポイントのみが有効化されているクラスターなど)、プライベート・ネットワークを介してブロック・ストレージ・インスタンスに接続するために、{{site.data.keyword.Bluemix_notm}} Block Storage プラグイン・バージョン 1.3.0 以降をインストールしていることを確認してください。 ブロック・ストレージ・インスタンスは、単一のゾーンに固有のものです。 マルチゾーン・クラスターを使用している場合は、[マルチゾーン永続ストレージ・オプション](/docs/containers?topic=containers-storage_planning#persistent_storage_overview)を検討してください。
{: important}

## クラスターでの {{site.data.keyword.Bluemix_notm}} Block Storage プラグインのインストール
{: #install_block}

ブロック・ストレージ用に事前定義されたストレージ・クラスをセットアップするには、{{site.data.keyword.Bluemix_notm}} Block Storage プラグインと Helm チャートをインストールします。 これらのストレージ・クラスを使用すると、アプリ用にブロック・ストレージをプロビジョンするための PVC を作成できます。
{: shortdesc}

開始前に、以下のことを行います。 [アカウントにログインします。 該当する場合は、適切なリソース・グループをターゲットにします。 クラスターのコンテキストを設定します。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1. ワーカー・ノードで、ご使用のマイナー・バージョンに対する最新パッチが適用されていることを確認します。
   1. ワーカー・ノードの現在のパッチ・バージョンをリストします。
      ```
      ibmcloud ks workers --cluster <cluster_name_or_ID>
      ```
      {: pre}

      出力例:
      ```
      OK
      ID                                                  Public IP        Private IP     Machine Type           State    Status   Zone    Version
      kube-dal10-crb1a23b456789ac1b20b2nc1e12b345ab-w26   169.xx.xxx.xxx    10.xxx.xx.xxx   b3c.4x16.encrypted     normal   Ready    dal10   1.13.6_1523*
      ```
      {: screen}

      ワーカー・ノードで、最新のパッチ・バージョンが適用されていない場合は、CLI 出力の **Version** 列にアスタリスク (`*`) が表示されます。

   2. [version changelog](/docs/containers?topic=containers-changelog#changelog) を参照して、最新のパッチ・バージョンに含まれる変更内容を確認してください。

   3. ワーカー・ノードを再ロードして、最新のパッチ・バージョンを適用します。 [ibmcloud ks worker-reload コマンド](/docs/containers?topic=containers-cs_cli_reference#cs_worker_reload)の説明に従って、ワーカー・ノード上で実行されているポッドを安全な方法でスケジュール変更した後に、ワーカー・ノードを再ロードしてください。 再ロード中に、ワーカー・ノード・マシンが最新のイメージで更新されるので、[ワーカー・ノードの外部に保管](/docs/containers?topic=containers-storage_planning#persistent_storage_overview)していないデータは削除されることに注意してください。

2.  [こちらの手順に従って](/docs/containers?topic=containers-helm#public_helm_install)、Helm クライアントをローカル・マシンにインストールして、サービス・アカウントを使用して Helm サーバー (Tiller) をクラスター内にインストールします。

    Helm サーバー Tiller をインストールするには、パブリック Google Container Registry へのパブリック・ネットワーク接続が必要です。 ご使用のクラスターがパブリック・ネットワークにアクセスできない場合は (ファイアウォール保護下のプライベート・クラスターや、プライベート・サービス・エンドポイントのみが有効化されているクラスターなど)、[Tiller イメージをローカル・マシンにプルして、そのイメージを {{site.data.keyword.registryshort_notm}} 内の名前空間にプッシュする](/docs/containers?topic=containers-helm#private_local_tiller)のか、[Tiller を使用せずに Helm チャートをインストールする](/docs/containers?topic=containers-helm#private_install_without_tiller)のかを選択できます。
    {: note}

3.  Tiller がサービス・アカウントでインストールされていることを確認します。

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

4. {{site.data.keyword.Bluemix_notm}} Block Storage プラグインを使用するクラスターに {{site.data.keyword.Bluemix_notm}} Helm チャート・リポジトリーを追加します。
   ```
   helm repo add iks-charts https://icr.io/helm/iks-charts
   ```
   {: pre}

5. Helm リポジトリーを更新して、このリポジトリーにあるすべての Helm チャートの最新バージョンを取得します。
   ```
   helm repo update
   ```
   {: pre}

6. {{site.data.keyword.Bluemix_notm}} Block Storage プラグインをインストールします。 このプラグインをインストールすると、事前定義されたブロック・ストレージ・クラスがクラスターに追加されます。
   ```
   helm install iks-charts/ibmcloud-block-storage-plugin
   ```
   {: pre}

   出力例:
   ```
   NAME:   bald-olm
   LAST DEPLOYED: Wed Apr 18 10:02:55 2018
   NAMESPACE: default
   STATUS: DEPLOYED

   RESOURCES:
   ==> v1beta1/DaemonSet
   NAME                           DESIRED  CURRENT  READY  UP-TO-DATE  AVAILABLE  NODE SELECTOR  AGE
   ibmcloud-block-storage-driver  0        0        0      0           0          <none>         0s

   ==> v1beta1/Deployment
   NAME                           DESIRED  CURRENT  UP-TO-DATE  AVAILABLE  AGE
   ibmcloud-block-storage-plugin  1        0        0           0          0s

   ==> v1/StorageClass
   NAME                      PROVISIONER        AGE
   ibmc-block-bronze         ibm.io/ibmc-block  0s
   ibmc-block-custom         ibm.io/ibmc-block  0s
   ibmc-block-gold           ibm.io/ibmc-block  0s
   ibmc-block-retain-bronze  ibm.io/ibmc-block  0s
   ibmc-block-retain-custom  ibm.io/ibmc-block  0s
   ibmc-block-retain-gold    ibm.io/ibmc-block  0s
   ibmc-block-retain-silver  ibm.io/ibmc-block  0s
   ibmc-block-silver         ibm.io/ibmc-block  0s

   ==> v1/ServiceAccount
   NAME                           SECRETS  AGE
   ibmcloud-block-storage-plugin  1        0s

   ==> v1beta1/ClusterRole
   NAME                           AGE
   ibmcloud-block-storage-plugin  0s

   ==> v1beta1/ClusterRoleBinding
   NAME                           AGE
   ibmcloud-block-storage-plugin  0s

   NOTES:
   Thank you for installing: ibmcloud-block-storage-plugin.   Your release is named: bald-olm
   ```
   {: screen}

7. 正常にインストールされたことを確認します。
   ```
   kubectl get pod -n kube-system | grep block
   ```
   {: pre}

   出力例:
   ```
   ibmcloud-block-storage-driver-kh4mt                              1/1       Running   0          27d       10.118.98.19   10.118.98.19
   ibmcloud-block-storage-plugin-58c5f9dc86-pbl4t                   1/1       Running   0          14d       172.21.0.204   10.118.98.19
   ```
   {: screen}

   1 つの `ibmcloud-block-storage-plugin` ポッドと 1 つ以上の `ibmcloud-block-storage-driver` ポッドが表示されたら、インストールは成功しています。 `ibmcloud-block-storage-driver` ポッドの数は、クラスター内のワーカー・ノードの数と等しくなります。 すべてのポッドが **Running** 状態である必要があります。

8. ブロック・ストレージのストレージ・クラスがクラスターに追加されたことを確認します。
   ```
   kubectl get storageclasses | grep block
   ```
   {: pre}

   出力例:
   ```
   ibmc-block-bronze            ibm.io/ibmc-block
   ibmc-block-custom            ibm.io/ibmc-block
   ibmc-block-gold              ibm.io/ibmc-block
   ibmc-block-retain-bronze     ibm.io/ibmc-block
   ibmc-block-retain-custom     ibm.io/ibmc-block
   ibmc-block-retain-gold       ibm.io/ibmc-block
   ibmc-block-retain-silver     ibm.io/ibmc-block
   ibmc-block-silver            ibm.io/ibmc-block
   ```
   {: screen}

9. ブロック・ストレージをプロビジョンするすべてのクラスターに対して、上記の手順を繰り返します。

これで、アプリ用にブロック・ストレージをプロビジョンするための [PVC の作成](#add_block)に進むことができます。


### {{site.data.keyword.Bluemix_notm}} Block Storage プラグインの更新
既存の {{site.data.keyword.Bluemix_notm}} Block Storage プラグインを最新バージョンにアップグレードできます。
{: shortdesc}

開始前に、以下のことを行います。 [アカウントにログインします。 該当する場合は、適切なリソース・グループをターゲットにします。 クラスターのコンテキストを設定します。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1. Helm リポジトリーを更新して、このリポジトリーにあるすべての Helm チャートの最新バージョンを取得します。
   ```
   helm repo update
   ```
   {: pre}

2. オプション: 最新の Helm チャートをローカル・マシンにダウンロードします。 そして、パッケージを解凍し、`release.md` ファイルを参照して最新リリース情報を確認します。
   ```
   helm fetch iks-charts/ibmcloud-block-storage-plugin
   ```
   {: pre}

3. クラスターにインストールしたブロック・ストレージ Helm チャートの名前を調べます。
   ```
   helm ls | grep ibmcloud-block-storage-plugin
   ```
   {: pre}

   出力例:
   ```
   myhelmchart 	1       	Mon Sep 18 15:31:40 2017	DEPLOYED	ibmcloud-block-storage-plugin-0.1.0	default
   ```
   {: screen}

4. {{site.data.keyword.Bluemix_notm}} Block Storage プラグインを最新バージョンにアップグレードします。
   ```
   helm upgrade --force --recreate-pods <helm_chart_name>  iks-charts/ibmcloud-block-storage-plugin
   ```
   {: pre}

5. オプション: プラグインを更新すると、`デフォルト`のストレージ・クラスの設定が解除されます。 デフォルトのストレージ・クラスを特定のストレージ・クラスに設定するには、次のコマンドを実行してください。
   ```
   kubectl patch storageclass <storageclass> -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'
   ```
   {: pre}


### {{site.data.keyword.Bluemix_notm}} Block Storage プラグインの削除
クラスターで {{site.data.keyword.Bluemix_notm}} Block Storage をプロビジョンして使用する必要がない場合は、Helm チャートをアンインストールできます。
{: shortdesc}

このプラグインを削除しても、既存の PVC、PV、データは削除されません。 このプラグインを削除すると、関連するすべてのポッドとデーモン・セットがクラスターから削除されます。 このプラグインを削除した後は、新しいブロック・ストレージをクラスターにプロビジョンしたり、既存のブロック・ストレージの PVC と PV を使用したりできません。
{: important}

開始前に、以下のことを行います。
- [アカウントにログインします。 該当する場合は、適切なリソース・グループをターゲットにします。 クラスターのコンテキストを設定します。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
- ブロック・ストレージを使用する PVC と PV がクラスターにないことを確認してください。

プラグインを削除するには、以下のようにします。

1. クラスターにインストールしたブロック・ストレージ Helm チャートの名前を調べます。
   ```
   helm ls | grep ibmcloud-block-storage-plugin
   ```
   {: pre}

   出力例:
   ```
   myhelmchart 	1       	Mon Sep 18 15:31:40 2017	DEPLOYED	ibmcloud-block-storage-plugin-0.1.0	default
   ```
   {: screen}

2. {{site.data.keyword.Bluemix_notm}} Block Storage プラグインを削除します。
   ```
   helm delete <helm_chart_name>
   ```
   {: pre}

3. ブロック・ストレージ・ポッドが削除されたことを確認します。
   ```
   kubectl get pod -n kube-system | grep ibmcloud-block-storage-plugin
   ```
   {: pre}
   CLI 出力にポッドが表示されていなければ、ポッドは正常に削除されています。

4. ブロック・ストレージのストレージ・クラスが削除されたことを確認します。
   ```
   kubectl get storageclasses | grep block
   ```
   {: pre}
   CLI 出力にストレージ・クラスが表示されていなければ、ストレージ・クラスは正常に削除されています。

<br />



## ブロック・ストレージ構成の決定
{: #block_predefined_storageclass}

{{site.data.keyword.containerlong}} には、ブロック・ストレージ用の事前定義のストレージ・クラスが用意されているので、これを使用して、特定の構成のブロック・ストレージをプロビジョンできます。
{: shortdesc}

ストレージ・クラスごとに、プロビジョンされるブロック・ストレージのタイプ (使用可能なサイズ、IOPS、ファイル・システム、保存ポリシーなど) が指定されています。  

データを保管できる十分な容量が得られるように、ストレージ構成は慎重に選択してください。 ストレージ・クラスを使用して特定のタイプのストレージをプロビジョンした後に、ストレージ・デバイスのサイズ、タイプ、IOPS、保存ポリシーを変更することはできません。 さらに容量が必要になった場合や、別の構成のストレージが必要になった場合は、[新規ストレージ・インスタンスを作成し、前のストレージ・インスタンスから新規ストレージ・インスタンスにデータをコピー](/docs/containers?topic=containers-kube_concepts#update_storageclass)する必要があります。
{: important}

1. {{site.data.keyword.containerlong}} で使用可能なストレージ・クラスをリストします。
    ```
    kubectl get storageclasses | grep block
    ```
    {: pre}

    出力例:
    ```
    $ kubectl get storageclasses
    NAME                         TYPE
    ibmc-block-custom            ibm.io/ibmc-block
    ibmc-block-bronze            ibm.io/ibmc-block
    ibmc-block-gold              ibm.io/ibmc-block
    ibmc-block-silver            ibm.io/ibmc-block
    ibmc-block-retain-bronze     ibm.io/ibmc-block
    ibmc-block-retain-silver     ibm.io/ibmc-block
    ibmc-block-retain-gold       ibm.io/ibmc-block
    ibmc-block-retain-custom     ibm.io/ibmc-block
    ```
    {: screen}

2. ストレージ・クラスの構成を確認します。
   ```
   kubectl describe storageclass <storageclass_name>
   ```
   {: pre}

   各ストレージ・クラスについて詳しくは、[ストレージ・クラス・リファレンス](#block_storageclass_reference)を参照してください。 ニーズに合うものがない場合は、カスタマイズした独自のストレージ・クラスを作成することを検討してください。 まずは、[ストレージ・クラスのカスタマイズ例](#block_custom_storageclass)を確認してください。
   {: tip}

3. プロビジョンするブロック・ストレージのタイプを選択します。
   - **ブロンズ、シルバー、ゴールドのストレージ・クラス:** これらのストレージ・クラスは[エンデュランス・ストレージ](/docs/infrastructure/BlockStorage?topic=BlockStorage-About#provendurance)をプロビジョンします。 エンデュランス・ストレージの場合、事前定義された IOPS ティアでストレージのサイズ (ギガバイト単位) を選択できます。
   - **カスタム・ストレージ・クラス:** このストレージ・クラスは[パフォーマンス・ストレージ](/docs/infrastructure/BlockStorage?topic=BlockStorage-About#provperformance)をプロビジョンします。 パフォーマンス・ストレージの場合は、より柔軟にストレージのサイズと IOPS を選択できます。

4. ブロック・ストレージのサイズと IOPS を選択します。 IOPS のサイズと数値によって、合計 IOPS (1 秒あたりの入出力操作数) が決まります。これは、ストレージの速度を示す指標となります。 ストレージの合計 IOPS が多いほど、読み取り/書き込み操作の処理が高速になります。
   - **ブロンズ、シルバー、ゴールドのストレージ・クラス:** これらのストレージ・クラスは、1 ギガバイトあたりの IOPS 数が固定されていて、SSD ハード・ディスクにプロビジョンされます。 合計の IOPS 数は、選択したストレージのサイズによって決まります。 指定可能なサイズの範囲内で、任意の整数のギガバイト (20 Gi、256 Gi、11854 Gi など) を選択できます。 合計 IOPS 数を求めるには、選択したサイズと IOPS を乗算します。 例えば、4 IOPS/GB のシルバー・ストレージ・クラスで、1000Gi のブロック・ストレージ・サイズを選択すると、ストレージの合計 IOPS は 4000 になります。  
     <table>
         <caption>ストレージ・クラスのサイズの範囲と IOPS/GB を示す表</caption>
         <thead>
         <th>ストレージ・クラス</th>
         <th>IOPS/GB</th>
         <th>サイズの範囲 (ギガバイト単位)</th>
         </thead>
         <tbody>
         <tr>
         <td>ブロンズ</td>
         <td>2 IOPS/GB</td>
         <td>20 から 12000 Gi</td>
         </tr>
         <tr>
         <td>シルバー</td>
         <td>4 IOPS/GB</td>
         <td>20 から 12000 Gi</td>
         </tr>
         <tr>
         <td>ゴールド</td>
         <td>10 IOPS/GB</td>
         <td>20 から 4000 Gi</td>
         </tr>
         </tbody></table>
   - **カスタム・ストレージ・クラス:** このストレージ・クラスを選択した場合は、より柔軟に、希望するサイズと IOPS を選択できます。 サイズについては、指定可能なサイズの範囲内で、任意の整数のギガバイトを選択できます。 選択したサイズに応じて、選択可能な IOPS の範囲が決まります。 指定された範囲にある 100 の倍数で IOPS を選択できます。 選択する IOPS は静的であり、ストレージのサイズに合わせてスケーリングされません。 例えば、40Gi と 100 IOPS を選択した場合、合計 IOPS は 100 のままです。 </br></br>ギガバイトに対する IOPS の比率によって、プロビジョンされるハード・ディスクのタイプも決まります。 例えば、500Gi を 100 IOPS で選択した場合、ギガバイトに対する IOPS の比率は 0.2 となります。 0.3 以下の比率のストレージは、SATA ハード・ディスクにプロビジョンされます。 0.3 より大きい比率のストレージは、SSD ハード・ディスクにプロビジョンされます。
     <table>
         <caption>カスタム・ストレージ・クラスのサイズ範囲と IOPS を示す表</caption>
         <thead>
         <th>サイズの範囲 (ギガバイト単位)</th>
         <th>IOPS の範囲 (100 の倍数)</th>
         </thead>
         <tbody>
         <tr>
         <td>20 から 39 Gi</td>
         <td>100 から 1000 IOPS</td>
         </tr>
         <tr>
         <td>40 から 79 Gi</td>
         <td>100 から 2000 IOPS</td>
         </tr>
         <tr>
         <td>80 から 99 Gi</td>
         <td>100 から 4000 IOPS</td>
         </tr>
         <tr>
         <td>100 から 499 Gi</td>
         <td>100 から 6000 IOPS</td>
         </tr>
         <tr>
         <td>500 から 999 Gi</td>
         <td>100 から 10000 IOPS</td>
         </tr>
         <tr>
         <td>1000 から 1999 Gi</td>
         <td>100 から 20000 IOPS</td>
         </tr>
         <tr>
         <td>2000 から 2999 Gi</td>
         <td>200 から 40000 IOPS</td>
         </tr>
         <tr>
         <td>3000 から 3999 Gi</td>
         <td>200 から 48000 IOPS</td>
         </tr>
         <tr>
         <td>4000 から 7999 Gi</td>
         <td>300 から 48000 IOPS</td>
         </tr>
         <tr>
         <td>8000 から 9999 Gi</td>
         <td>500 から 48000 IOPS</td>
         </tr>
         <tr>
         <td>10000 から 12000 Gi</td>
         <td>1000 から 48000 IOPS</td>
         </tr>
         </tbody></table>

5. クラスターまたは永続ボリューム請求 (PVC) が削除された後もデータを保持するかどうかを選択します。
   - データを保持する場合、`retain` ストレージ・クラスを選択します。 PVC を削除すると、PVC のみが削除されます。 PV と、IBM Cloud インフラストラクチャー (SoftLayer) アカウント内の物理ストレージ・デバイスと、データは残ります。 ストレージを回収し、クラスターで再使用するには、PV を削除し、[既存のブロック・ストレージを使用する](#existing_block)手順に従う必要があります。
   - PVC を削除するときに PV、データ、物理ブロック・ストレージ・デバイスが削除されるようにするには、`retain` なしのストレージ・クラスを選択します。

6. 時間単位と月単位のどちらの課金方法にするかを選択します。 詳しくは、[料金設定 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/cloud/block-storage/pricing) を参照してください。 デフォルトでは、すべてのブロック・ストレージ・デバイスが時間単位の課金タイプでプロビジョンされます。

<br />



## アプリへのブロック・ストレージの追加
{: #add_block}

ブロック・ストレージをクラスターに[動的にプロビジョン](/docs/containers?topic=containers-kube_concepts#dynamic_provisioning)するために、永続ボリューム請求 (PVC) を作成します。 動的プロビジョニングでは、対応する永続ボリューム (PV) が自動的に作成され、IBM Cloud インフラストラクチャー (SoftLayer) アカウントで実際のストレージ・デバイスが注文されます。
{:shortdesc}

ブロック・ストレージは、`ReadWriteOnce` アクセス・モードです。 これは、一度にクラスター内の 1 つのワーカー・ノードの 1 つのポッドにのみマウントできます。
{: note}

開始前に、以下のことを行います。
- ファイアウォールがある場合は、クラスターのあるゾーンの IBM Cloud インフラストラクチャー (SoftLayer) の IP 範囲に[発信アクセスを許可](/docs/containers?topic=containers-firewall#pvc)し、PVC を作成できるようにします。
- [{{site.data.keyword.Bluemix_notm}} Block Storage プラグイン](#install_block)をインストールします。
- [事前定義されたストレージ・クラスを選択する](#block_predefined_storageclass)か、または[カスタマイズしたストレージ・クラス](#block_custom_storageclass)を作成します。

ステートフル・セットにブロック・ストレージをデプロイしようとお考えですか? 詳しくは、[ステートフル・セットでのブロック・ストレージの使用](#block_statefulset)を参照してください。
{: tip}

ブロック・ストレージを追加するには、以下のようにします。

1.  永続ボリューム請求 (PVC) を定義した構成ファイルを作成し、`.yaml` ファイルとして構成を保存します。

    -  **ブロンズ、シルバー、ゴールドのストレージ・クラスの例**:
       以下の `.yaml` ファイルは、名前が `mypvc`、ストレージ・クラスが `"ibmc-block-silver"`、課金が `"hourly"`、ギガバイト・サイズが `24Gi` の請求を作成します。

       ```
       apiVersion: v1
       kind: PersistentVolumeClaim
       metadata:
         name: mypvc
         labels:
           billingType: "hourly"
           region: us-south
           zone: dal13
       spec:
         accessModes:
           - ReadWriteOnce
         resources:
           requests:
             storage: 24Gi
	     storageClassName: ibmc-block-silver
       ```
       {: codeblock}

    -  **カスタム・ストレージ・クラスの使用例**:
       以下の `.yaml` ファイルで作成する請求は、名前が `mypvc`、ストレージ・クラスが `ibmc-block-retain-custom`、課金タイプが `"hourly"`、ギガバイト・サイズが `45Gi`、IOPS が `"300"` です。

       ```
       apiVersion: v1
       kind: PersistentVolumeClaim
       metadata:
         name: mypvc
         labels:
           billingType: "hourly"
           region: us-south
           zone: dal13
       spec:
         accessModes:
           - ReadWriteOnce
         resources:
           requests:
             storage: 45Gi
             iops: "300"
	     storageClassName: ibmc-block-retain-custom
       ```
       {: codeblock}

       <table>
       <caption>YAML ファイルの構成要素について</caption>
       <thead>
       <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> YAML ファイルの構成要素について</th>
       </thead>
       <tbody>
       <tr>
       <td><code>metadata.name</code></td>
       <td>PVC の名前を入力します。</td>
       </tr>
       <tr>
         <td><code>metadata.labels.billingType</code></td>
         <td>ストレージの課金額を計算する頻度として「monthly」または「hourly」を指定します。 デフォルトは「hourly」です。</td>
       </tr>
       <tr>
       <td><code>metadata.labels.region</code></td>
       <td>ブロック・ストレージをプロビジョンする地域を指定します。 地域を指定する場合は、ゾーンも指定する必要があります。 地域を指定しなかった場合、または指定した地域が見つからなかった場合、ストレージはクラスターと同じ地域に作成されます。 <p class="note">このオプションは、IBM Cloud Block Storage プラグインのバージョン 1.0.1 以上でのみサポートされます。 複数ゾーン・クラスターを使用している場合、これより古いバージョンのプラグインでは、ボリューム要求をすべてのゾーン間で均等に分散させるために、ストレージは、ラウンドロビン・ベースで選択されたゾーンにプロビジョンされます。 ストレージのゾーンを指定するには、まず[カスタマイズしたストレージ・クラス](#block_multizone_yaml)を作成できます。 それから、カスタマイズしたストレージ・クラスを使用して PVC を作成します。</p></td>
       </tr>
       <tr>
       <td><code>metadata.labels.zone</code></td>
	<td>ブロック・ストレージをプロビジョンするゾーンを指定します。 ゾーンを指定する場合は、地域も指定する必要があります。 ゾーンを指定しなかった場合、または指定したゾーンが複数ゾーン・クラスターで見つからなかった場合、ゾーンはラウンドロビン・ベースで選択されます。 <p class="note">このオプションは、IBM Cloud Block Storage プラグインのバージョン 1.0.1 以上でのみサポートされます。 複数ゾーン・クラスターを使用している場合、これより古いバージョンのプラグインでは、ボリューム要求をすべてのゾーン間で均等に分散させるために、ストレージは、ラウンドロビン・ベースで選択されたゾーンにプロビジョンされます。 ストレージのゾーンを指定するには、まず[カスタマイズしたストレージ・クラス](#block_multizone_yaml)を作成できます。 それから、カスタマイズしたストレージ・クラスを使用して PVC を作成します。</p></td>
	</tr>
        <tr>
        <td><code>spec.resources.requests.storage</code></td>
        <td>ブロック・ストレージのサイズをギガバイト (Gi) 単位で入力します。 ストレージがプロビジョンされた後は、ブロック・ストレージのサイズを変更できません。 保管するデータの量に一致するサイズを指定してください。 </td>
        </tr>
        <tr>
        <td><code>spec.resources.requests.iops</code></td>
        <td>このオプションは、カスタム・ストレージ・クラス (`ibmc-block-custom / ibmc-block-retain-custom`) でのみ使用できます。 許容範囲内で 100 の倍数を選択して、ストレージの合計 IOPS を指定します。 リストされているもの以外の IOPS を選択した場合、その IOPS は切り上げられます。</td>
        </tr>
	<tr>
	<td><code>spec.storageClassName</code></td>
	<td>ブロック・ストレージをプロビジョンするために使用するストレージ・クラスの名前。 [IBM 提供のストレージ・クラス](#block_storageclass_reference)の 1 つを使用するのか、[独自のストレージ・クラスを作成](#block_custom_storageclass)するのかを選択できます。 </br> ストレージ・クラスを指定しなかった場合は、デフォルト・ストレージ・クラス <code>ibmc-file-bronze</code> を使用して PV が作成されます。<p>**ヒント:** デフォルトのストレージ・クラスを変更する場合は、<code>kubectl patch storageclass &lt;storageclass&gt; -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'</code> を実行して、<code>&lt;storageclass&gt;</code> をストレージ・クラスの名前に置き換えます。</p></td>
	</tr>
        </tbody></table>

    カスタマイズされたストレージ・クラスを使用する場合は、対応するストレージ・クラス名、有効な IOPS、およびサイズを指定して PVC を作成します。   
    {: tip}

2.  PVC を作成します。

    ```
    kubectl apply -f mypvc.yaml
    ```
    {: pre}

3.  PVC が作成され、PV にバインドされたことを確認します。 この処理には数分かかる場合があります。

    ```
    kubectl describe pvc mypvc
    ```
    {: pre}

    出力例:

    ```
    Name:		mypvc
    Namespace:	default
    StorageClass:	""
    Status:		Bound
    Volume:		pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2
    Labels:		<none>
    Capacity:	20Gi
    Access Modes:	RWO
    Events:
      FirstSeen	LastSeen	Count	From								SubObjectPath	Type		Reason			Message
      ---------	--------	-----	----								-------------	--------	------			-------
      3m 3m 1 {ibm.io/ibmc-block 31898035-3011-11e7-a6a4-7a08779efd33 } Normal Provisioning External provisioner is provisioning volume  for claim "default/my-persistent-volume-claim"
       3m 1m	 10 {persistentvolume-controller } Normal ExternalProvisioning cannot find provisioner "ibm.io/ibmc-block", expecting that  a volume for the claim is provisioned either manually or via external software
       1m 1m 1 {ibm.io/ibmc-block 31898035-3011-11e7-a6a4-7a08779efd33 } Normal ProvisioningSucceeded	Successfully provisioned volume  pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2

    ```
    {: screen}

4.  {: #block_app_volume_mount}PV をデプロイメントにマウントするには、構成 `.yaml` ファイルを作成し、PV をバインドする PVC を指定します。

    ```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: <deployment_name>
      labels:
        app: <deployment_label>
    spec:
      selector:
        matchLabels:
          app: <app_name>
      template:
        metadata:
          labels:
            app: <app_name>
        spec:
          containers:
          - image: <image_name>
            name: <container_name>
            volumeMounts:
            - name: <volume_name>
              mountPath: /<file_path>
          volumes:
          - name: <volume_name>
            persistentVolumeClaim:
              claimName: <pvc_name>
    ```
    {: codeblock}

    <table>
    <caption>YAML ファイルの構成要素について</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> YAML ファイルの構成要素について</th>
    </thead>
    <tbody>
        <tr>
    <td><code>metadata.labels.app</code></td>
    <td>デプロイメントのラベル。</td>
      </tr>
      <tr>
        <td><code>spec.selector.matchLabels.app</code> <br/> <code>spec.template.metadata.labels.app</code></td>
        <td>アプリのラベル。</td>
      </tr>
    <tr>
    <td><code>template.metadata.labels.app</code></td>
    <td>デプロイメントのラベル。</td>
      </tr>
    <tr>
    <td><code>spec.containers.image</code></td>
    <td>使用するイメージの名前。 {{site.data.keyword.registryshort_notm}} アカウント内の使用可能なイメージをリストするには、`ibmcloud cr image-list` を実行します。</td>
    </tr>
    <tr>
    <td><code>spec.containers.name</code></td>
    <td>クラスターにデプロイするコンテナーの名前。</td>
    </tr>
    <tr>
    <td><code>spec.containers.volumeMounts.mountPath</code></td>
    <td>コンテナー内でボリュームがマウントされるディレクトリーの絶対パス。 マウント・パスに書き込まれるデータは、物理ブロック・ストレージ・インスタンスのルート・ディレクトリーに保管されます。 複数のアプリ間で 1 つのボリュームを共有する場合は、アプリごとに[ボリューム・サブパス ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/concepts/storage/volumes/#using-subpath) を指定できます。 </td>
    </tr>
    <tr>
    <td><code>spec.containers.volumeMounts.name</code></td>
    <td>ポッドにマウントするボリュームの名前。</td>
    </tr>
    <tr>
    <td><code>volumes.name</code></td>
    <td>ポッドにマウントするボリュームの名前。 通常、この名前は <code>volumeMounts/name</code> と同じです。</td>
    </tr>
    <tr>
    <td><code>volumes.persistentVolumeClaim.claimName</code></td>
    <td>使用する PV をバインドする PVC の名前。 </td>
    </tr>
    </tbody></table>

5.  デプロイメントを作成します。
     ```
     kubectl apply -f <local_yaml_path>
     ```
     {: pre}

6.  PV が正常にマウントされたことを確認します。

     ```
     kubectl describe deployment <deployment_name>
     ```
     {: pre}

     マウント・ポイントは **Volume Mounts** フィールドにあり、ボリュームは **Volumes** フィールドにあります。

     ```
      Volume Mounts:
           /var/run/secrets/kubernetes.io/serviceaccount from default-token-tqp61 (ro)
           /volumemount from myvol (rw)
     ...
     Volumes:
       myvol:
         Type:	PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
         ClaimName:	mypvc
         ReadOnly:	false
     ```
     {: screen}

<br />




## クラスターでの既存のブロック・ストレージの使用
{: #existing_block}

クラスターで使用したい既存の物理ストレージ・デバイスがある場合は、PV と PVC を手動で作成して、そのストレージを[静的にプロビジョン](/docs/containers?topic=containers-kube_concepts#static_provisioning)することができます。
{: shortdesc}

アプリへの既存のストレージのマウントを開始するには、その前に、PV に関する必要な情報をすべて取得する必要があります。  

### ステップ 1: 既存のブロック・ストレージの情報を取得する
{: #existing-block-1}

1.  IBM Cloud インフラストラクチャー (SoftLayer) アカウントの API キーを取得または生成します。
    1. [IBM Cloud インフラストラクチャー (SoftLayer) ポータル ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/classic?) にログインします。
    2. **「アカウント」**、**「ユーザー」**、**「ユーザー・リスト」**の順に選択します。
    3. 自分のユーザー ID を見つけます。
    4. **「API キー」**列で、**「生成」**をクリックして API キーを生成するか、または**「表示」**をクリックして既存の API キーを表示します。
2.  IBM Cloud インフラストラクチャー (SoftLayer) アカウントの API ユーザー名を取得します。
    1. **「ユーザー・リスト」**メニューで、自分のユーザー ID を選択します。
    2. **「API アクセス情報」**セクションで、自分の**「API ユーザー名」**を見つけます。
3.  IBM Cloud インフラストラクチャー CLI プラグインにログインします。
    ```
    ibmcloud sl init
    ```
    {: pre}

4.  IBM Cloud インフラストラクチャー (SoftLayer) アカウントのユーザー名と API キーを使用して認証を受けることを選択します。
5.  前の手順で取得したユーザー名と API キーを入力します。
6.  使用可能なブロック・ストレージ・デバイスをリストします。
    ```
    ibmcloud sl block volume-list
    ```
    {: pre}

    出力例:
    ```
    id         username            datacenter   storage_type              capacity_gb   bytes_used   ip_addr         lunId   active_transactions
    38642141   IBM02SEL1543159-1   dal10        endurance_block_storage   20            -            169.xx.xxx.xxx   170     0
    ```
    {: screen}

7.  クラスターにマウントするブロック・ストレージ・デバイスの `id`、`ip_addr`、`capacity_gb`、`datacenter`、`lunId` をメモします。 **注:** 既存のストレージをクラスターにマウントするには、そのストレージと同じゾーンにワーカー・ノードが存在する必要があります。 ワーカー・ノードのゾーンを確認するには、`ibmcloud ks workers --cluster <cluster_name_or_ID>` を実行します。

### ステップ 2: 永続ボリューム (PV) および対応する永続ボリューム請求 (PVC) を作成する
{: #existing-block-2}

1.  オプション: `retain` ストレージ・クラスを指定してプロビジョンしたストレージの場合、PVC を削除しても、PV と物理ストレージ・デバイスは削除されません。 そのストレージをクラスターで再使用するには、まず PV を削除する必要があります。
    1. 既存の PV をリストします。
       ```
       kubectl get pv
       ```
       {: pre}

       対象の永続ストレージに属する PV を探します。 PV は `released` 状態になっています。

    2. PV を削除します。
       ```
       kubectl delete pv <pv_name>
       ```
       {: pre}

    3. PV が削除されたことを確認します。
       ```
       kubectl get pv
       ```
       {: pre}

2.  PV の構成ファイルを作成します。 先ほど取得したブロック・ストレージの `id`、`ip_addr`、`capacity_gb`、`datacenter`、`lunIdID` を含めます。

    ```
    apiVersion: v1
    kind: PersistentVolume
    metadata:
      name: mypv
      labels:
         failure-domain.beta.kubernetes.io/region: <region>
         failure-domain.beta.kubernetes.io/zone: <zone>
    spec:
      capacity:
        storage: "<storage_size>"
      accessModes:
        - ReadWriteOnce
      flexVolume:
        driver: "ibm/ibmc-block"
        fsType: "<fs_type>"
        options:
          "Lun": "<lun_ID>"
          "TargetPortal": "<IP_address>"
          "VolumeID": "<volume_ID>"
          "volumeName": "<volume_name>"
      ```
      {: codeblock}

    <table>
    <caption>YAML ファイルの構成要素について</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> YAML ファイルの構成要素について</th>
    </thead>
    <tbody>
    <tr>
    <td><code>metadata.name</code></td>
    <td>作成する PV の名前を入力します。</td>
    </tr>
    <tr>
    <td><code>metadata.labels</code></td>
    <td>先ほど取得した地域とゾーンを入力します。 クラスターでストレージをマウントするには、永続ストレージと同じ地域およびゾーンに少なくとも 1 つのワーカー・ノードが存在していなければなりません。 ストレージの PV が既に存在する場合は、PV に[ゾーンおよび地域のラベルを追加](/docs/containers?topic=containers-kube_concepts#storage_multizone)します。
    </tr>
    <tr>
    <td><code>spec.flexVolume.fsType</code></td>
    <td>既存のブロック・ストレージ用に構成されているファイル・システム・タイプを入力します。 <code>ext4</code> または <code>xfs</code> のいずれかを選択します。 このオプションを指定しない場合、PV はデフォルトで <code>ext4</code> に設定されます。 誤った `fsType` が定義された場合、PV の作成は成功しますが、PV のポッドへのマウントは失敗します。 </td></tr>	    
    <tr>
    <td><code>spec.capacity.storage</code></td>
    <td>前の手順で取得した既存のブロック・ストレージのストレージ・サイズ (<code>capacity-gb</code>) を入力します。 このストレージ・サイズはギガバイト単位 (例えば、20Gi (20 GB) や 1000Gi (1 TB) など) で入力する必要があります。</td>
    </tr>
    <tr>
    <td><code>flexVolume.options.Lun</code></td>
    <td>先ほど <code>lunId</code> として取得した、ブロック・ストレージの lun ID を入力します。</td>
    </tr>
    <tr>
    <td><code>flexVolume.options.TargetPortal</code></td>
    <td>先ほど <code>ip_addr</code> として取得したブロック・ストレージの IP アドレスを入力します。 </td>
    </tr>
    <tr>
	    <td><code>flexVolume.options.VolumeId</code></td>
	    <td>先ほど <code>id</code> として取得したブロック・ストレージの ID を入力します。</td>
	    </tr>
	    <tr>
		    <td><code>flexVolume.options.volumeName</code></td>
		    <td>ボリュームの名前を入力します。</td>
	    </tr>
    </tbody></table>

3.  クラスターに PV を作成します。
    ```
    kubectl apply -f mypv.yaml
    ```
    {: pre}

4. PV が作成されたことを確認します。
    ```
    kubectl get pv
    ```
    {: pre}

5. PVC を作成するために、別の構成ファイルを作成します。 PVC が、前の手順で作成した PV と一致するようにするには、`storage` および `accessMode` に同じ値を選択する必要があります。 `storage-class` フィールドは空である必要があります。 これらのいずれかのフィールドが PV と一致しない場合、代わりに新しい PV が自動的に作成されます。

     ```
     kind: PersistentVolumeClaim
     apiVersion: v1
     metadata:
      name: mypvc
     spec:
      accessModes:
        - ReadWriteOnce
      resources:
        requests:
          storage: "<storage_size>"
      storageClassName:
     ```
     {: codeblock}

6.  PVC を作成します。
     ```
     kubectl apply -f mypvc.yaml
     ```
     {: pre}

7.  PVC が作成され、先ほど作成した PV にバインドされたことを確認します。 この処理には数分かかる場合があります。
     ```
     kubectl describe pvc mypvc
     ```
     {: pre}

     出力例:

     ```
     Name: mypvc
     Namespace: default
     StorageClass:	""
     Status: Bound
     Volume: pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2
     Labels: <none>
     Capacity: 20Gi
     Access Modes: RWO
     Events:
       FirstSeen LastSeen Count From        SubObjectPath Type Reason Message
       --------- -------- ----- ----        ------------- -------- ------ -------
       3m 3m 1 {ibm.io/ibmc-block 31898035-3011-11e7-a6a4-7a08779efd33 } Normal Provisioning External provisioner is provisioning volume  for claim "default/my-persistent-volume-claim"
       3m 1m	 10 {persistentvolume-controller } Normal ExternalProvisioning cannot find provisioner "ibm.io/ibmc-block", expecting that  a volume for the claim is provisioned either manually or via external software
       1m 1m 1 {ibm.io/ibmc-block 31898035-3011-11e7-a6a4-7a08779efd33 } Normal ProvisioningSucceeded	Successfully provisioned volume  pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2
     ```
     {: screen}

PV が正常に作成され、PVC にバインドされました。 これで、クラスター・ユーザーがデプロイメントに[PVC をマウント](#block_app_volume_mount)して、PV への読み書きを開始できるようになりました。

<br />



## ステートフル・セットでのブロック・ストレージの使用
{: #block_statefulset}

データベースなどのステートフルなアプリがある場合は、そのアプリのデータを保管するために、ブロック・ストレージを使用するステートフル・セットを作成することができます。 別の方法として、{{site.data.keyword.Bluemix_notm}} Database as a Service を使用し、クラウドにデータを保管することもできます。
{: shortdesc}

**ブロック・ストレージをステートフル・セットに追加する場合、どんな点に留意する必要がありますか?** </br>
ストレージをステートフル・セットに追加するには、ステートフル・セット YAML の `volumeClaimTemplates` セクションでストレージ構成を指定します。 `volumeClaimTemplates` は、PVC の基礎となるもので、プロビジョンするブロック・ストレージのストレージ・クラスとサイズ (または IOPS) を含めることができます。 ただし、`volumeClaimTemplates` にラベルを含める場合、Kubernetes による PVC の作成時にそれらのラベルは組み込まれません。 その代わりに、自分で直接、それらのラベルをステートフル・セットに追加する必要があります。

2 つのステートフル・セットを同時にデプロイすることはできません。 1 つのステートフル・セットが完全にデプロイされる前に別のステートフル・セットを作成しようとすると、ステートフル・セットのデプロイメントが予期しない結果となる可能性があります。
{: important}

**ステートフル・セットをどのように特定のゾーンに作成できますか?** </br>
複数ゾーン・クラスターでは、ステートフル・セット YAML の `spec.selector.matchLabels` および `spec.template.metadata.labels` セクションで、ステートフル・セットを作成するゾーンと地域を指定することができます。 あるいは、それらのラベルを[カスタマイズ・ストレージ・クラス](/docs/containers?topic=containers-kube_concepts#customized_storageclass)に追加し、このストレージ・クラスをステートフル・セットの `volumeClaimTemplates` セクションで使用することもできます。

**ステートフル・ポッドに対する PV のバインディングをそのポッドの準備が完了するまで延期できますか?**<br>
はい。[`volumeBindingMode: WaitForFirstConsumer` ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/concepts/storage/storage-classes/#volume-binding-mode) フィールドが含まれた PVC 用の[カスタム・ストレージ・クラスを作成](#topology_yaml)できます。

**ステートフル・セットにブロック・ストレージを追加するときに選択できるどんなオプションがありますか?** </br>
ステートフル・セットを作成するときに PVC を自動的に作成する場合は、[動的プロビジョニング](#block_dynamic_statefulset)を使用します。 また、ステートフル・セットで使用するために [PVC を事前プロビジョンするか、既存の PVC を使用する](#block_static_statefulset)こともできます。  

### 動的プロビジョニング: ステートフル・セット作成時の PVC の作成
{: #block_dynamic_statefulset}

このオプションは、ステートフル・セットを作成するときに PVC を自動的に作成する場合に使用します。
{: shortdesc}

開始前に、以下のことを行います。 [アカウントにログインします。 該当する場合は、適切なリソース・グループをターゲットにします。 クラスターのコンテキストを設定します。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1. クラスターにある既存のすべてのステートフル・セットが完全にデプロイ済みであることを確認します。 デプロイ中のステートフル・セットがある場合は、ステートフル・セットの作成を開始できません。 予期しない結果が生じるのを避けるため、クラスター内のすべてのステートフル・セットが完全にデプロイされるまで待つ必要があります。
   1. クラスター内の既存のステートフル・セットをリストします。
      ```
      kubectl get statefulset --all-namespaces
      ```
      {: pre}

      出力例:
      ```
      NAME              DESIRED   CURRENT   AGE
      mystatefulset     3         3         6s
      ```
      {: screen}

   2. ステートフル・セットのデプロイメントが完了していることを確かめるため、各ステートフル・セットの **Pods Status** を確認します。  
      ```
      kubectl describe statefulset <statefulset_name>
      ```
      {: pre}

      出力例:
      ```
      Name:               nginx
      Namespace:          default
      CreationTimestamp:  Fri, 05 Oct 2018 13:22:41 -0400
      Selector:           app=nginx,billingType=hourly,region=us-south,zone=dal10
      Labels:             app=nginx
                          billingType=hourly
                          region=us-south
                          zone=dal10
      Annotations:        kubectl.kubernetes.io/last-applied-configuration={"apiVersion":"apps/v1","kind":"StatefulSet","metadata":{"annotations":{},"name":"nginx","namespace":"default"},"spec":{"podManagementPolicy":"Par...
      Replicas:           3 desired | 3 total
      Pods Status:        0 Running / 3 Waiting / 0 Succeeded / 0 Failed
      Pod Template:
        Labels:  app=nginx
                 billingType=hourly
                 region=us-south
                 zone=dal10
      ...
      ```
      {: screen}

      CLI 出力の **Replicas** セクションに示されたレプリカの数が、**Pods Status** セクションの **Running** ポッドの数と等しい場合は、ステートフル・セットが完全にデプロイ済みです。 ステートフル・セットがまだ完全にデプロイされていない場合は、デプロイメントが完了するまで待ってから、次に進んでください。

2. ステートフル・セットと、そのステートフル・セットを公開するために使用するサービスに関する、構成ファイルを作成します。

   - **ゾーンを指定するステートフル・セットの例:**

     下記の例は、3 つのレプリカを伴うステートフル・セットとして NGINX をデプロイする方法を示しています。 レプリカごとに、`ibmc-block-retain-bronze` ストレージ・クラスで定義された仕様に基づいて、20 ギガバイトのブロック・ストレージ・デバイスがプロビジョンされます。 すべてのストレージ・デバイスは `dal10` ゾーンでプロビジョンされます。 ブロック・ストレージは他のゾーンからアクセスできないため、ステートフル・セットのすべてのレプリカも、`dal10` に配置されたワーカー・ノードにデプロイされます。

     ```
     apiVersion: v1
    kind: Service
    metadata:
     name: nginx
     labels:
       app: nginx
    spec:
     ports:
      - port: 80
        name: web
      clusterIP: None
      selector:
        app: nginx
     ---
     apiVersion: apps/v1
    kind: StatefulSet
    metadata:
     name: nginx
    spec:
     serviceName: "nginx"
     replicas: 3
     podManagementPolicy: Parallel
     selector:
       matchLabels:
         app: nginx
         billingType: "hourly"
         region: "us-south"
         zone: "dal10"
     template:
       metadata:
         labels:
           app: nginx
           billingType: "hourly"
           region: "us-south"
           zone: "dal10"
       spec:
         containers:
          - name: nginx
            image: k8s.gcr.io/nginx-slim:0.8
            ports:
            - containerPort: 80
              name: web
            volumeMounts:
            - name: myvol
             mountPath: /usr/share/nginx/html
     volumeClaimTemplates:
      - metadata:
         name: myvol
       spec:
         accessModes:
          - ReadWriteOnce
          resources:
            requests:
              storage: 20Gi
              iops: "300" #required only for performance storage
	      storageClassName: ibmc-block-retain-bronze
     ```
     {: codeblock}

   - **ブロック・ストレージの作成が遅延される、アンチアフィニティー・ルールを使用するステートフル・セットの例:**

     下記の例は、3 つのレプリカを伴うステートフル・セットとして NGINX をデプロイする方法を示しています。 このステートフル・セットでは、ブロック・ストレージの作成場所である地域とゾーンは指定されません。 代わりに、このステートフル・セットではアンチアフィニティー・ルールを使用して、ポッドが複数のワーカー・ノードとゾーンにまたがって分散されるようにします。 `topologykey: failure-domain.beta.kubernetes.io/zone` を定義することで、Kubernetes スケジューラーは、`app: nginx` ラベルを持つポッドと同じゾーン内のワーカー・ノード上でどのポッドもスケジュールできなくなります。 各ステートフル・セット・ポッドについて、`volumeClaimTemplates` セクション内の定義に従って 2 つの PVC が作成されますが、ブロック・ストレージ・インスタンスの作成は、そのストレージを使用するステートフル・セット・ポッドがスケジュールされるまで遅延されます。 このセットアップは、[トポロジー対応ボリューム・スケジューリング](https://kubernetes.io/blog/2018/10/11/topology-aware-volume-provisioning-in-kubernetes/)と呼ばれます。

     ```
     apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
    name: ibmc-block-bronze-delayed
  parameters:
    billingType: hourly
    classVersion: "2"
    fsType: ext4
    iopsPerGB: "2"
    sizeRange: '[20-12000]Gi'
    type: Endurance
  provisioner: ibm.io/ibmc-block
  reclaimPolicy: Delete
  volumeBindingMode: WaitForFirstConsumer
     ---
     apiVersion: v1
    kind: Service
    metadata:
      name: nginx
      labels:
        app: nginx
    spec:
      ports:
       - port: 80
       name: web
     clusterIP: None
     selector:
       app: nginx
     ---
     apiVersion: apps/v1
    kind: StatefulSet
    metadata:
      name: web
    spec:
      serviceName: "nginx"
      replicas: 3
      podManagementPolicy: "Parallel"
      selector:
        matchLabels:
          app: nginx
      template:
        metadata:
          labels:
            app: nginx
        spec:
          affinity:
            podAntiAffinity:
              preferredDuringSchedulingIgnoredDuringExecution:
               - weight: 100
            podAffinityTerm:
              labelSelector:
                matchExpressions:
                     - key: app
                  operator: In
                  values:
                       - nginx
                  topologyKey: failure-domain.beta.kubernetes.io/zone
          containers:
           - name: nginx
           image: k8s.gcr.io/nginx-slim:0.8
           ports:
             - containerPort: 80
             name: web
           volumeMounts:
             - name: www
              mountPath: /usr/share/nginx/html
             - name: wwwww
              mountPath: /tmp1
      volumeClaimTemplates:
       - metadata:
          name: myvol1
        spec:
          accessModes:
           - ReadWriteOnce # access mode
           resources:
             requests:
               storage: 20Gi
	       storageClassName: ibmc-block-bronze-delayed
       - metadata:
          name: myvol2
        spec:
          accessModes:
           - ReadWriteOnce # access mode
           resources:
             requests:
               storage: 20Gi
	       storageClassName: ibmc-block-bronze-delayed
     ```
     {: codeblock}

     <table>
     <caption>ステートフル・セット YAML ファイルの構成要素について</caption>
     <thead>
     <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> ステートフル・セット YAML ファイルの構成要素について</th>
     </thead>
     <tbody>
     <tr>
     <td style="text-align:left"><code>metadata.name</code></td>
     <td style="text-align:left">ステートフル・セットに対する名前を入力します。 入力した名前が使用されて、PVC の名前が次の形式で作成されます: <code>&lt;volume_name&gt;-&lt;statefulset_name&gt;-&lt;replica_number&gt;</code>。 </td>
     </tr>
     <tr>
     <td style="text-align:left"><code>spec.serviceName</code></td>
     <td style="text-align:left">ステートフル・セットを公開するために使用するサービスの名前を入力します。 </td>
     </tr>
     <tr>
     <td style="text-align:left"><code>spec.replicas</code></td>
     <td style="text-align:left">ステートフル・セットのレプリカの数を入力します。 </td>
     </tr>
     <tr>
     <td style="text-align:left"><code>spec.podManagementPolicy</code></td>
     <td style="text-align:left">ステートフル・セットで使用するポッド管理ポリシーを入力します。 次のいずれかのオプションを選択します。 <ul><li><strong>`OrderedReady`: </strong>このオプションを指定すると、ステートフル・セット・レプリカが 1 つずつデプロイされます。 例えば、3 つのレプリカを指定した場合、Kubernetes は 1 番目のレプリカの PVC を作成し、PVC がバインドされるまで待機し、ステートフル・セット・レプリカをデプロイし、PVC をレプリカにマウントします。 このデプロイメントが完了したら、2 番目のレプリカがデプロイされます。 このオプションについて詳しくは、[`OrderedReady` Pod Management ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/#orderedready-pod-management) を参照してください。 </li><li><strong>Parallel: </strong>このオプションを指定すると、ステートフル・セット・レプリカのすべてのデプロイメントが同時に開始します。 アプリでレプリカの並行デプロイメントをサポートしている場合は、このオプションを使用して、PVC とステートフル・セット・レプリカをデプロイする時間を節約してください。 </li></ul></td>
     </tr>
     <tr>
     <td style="text-align:left"><code>spec.selector.matchLabels</code></td>
     <td style="text-align:left">ステートフル・セットと PVC に含めるすべてのラベルを入力します。 ステートフル・セットの <code>volumeClaimTemplates</code> に含めたラベルは、Kubernetes によって認識されません。 含めることができるサンプル・ラベルには次のものがあります。 <ul><li><code><strong>region</strong></code> および <code><strong>zone</strong></code>: ステートフル・セット・レプリカと PVC のすべてを、1 つの特定のゾーンに作成する場合は、これらのラベルを両方とも追加します。 また、使用するストレージ・クラスでゾーン (zone) と地域 (region) を指定することもできます。 ゾーンと地域を指定せず、複数ゾーン・クラスターを使用している場合、ボリューム要求をすべてのゾーン間で均等に分散させるために、ストレージは、ラウンドロビン・ベースで選択されたゾーンにプロビジョンされます。</li><li><code><strong>billingType</strong></code>: PVC で使用する課金タイプを入力します。 <code>hourly</code> または <code>monthly</code> のいずれかを選択します。 このラベルを指定しない場合、すべての PVC が時間単位 (hourly) の課金タイプで作成されます。 </li></ul></td>
     </tr>
     <tr>
     <td style="text-align:left"><code>spec.template.metadata.labels</code></td>
     <td style="text-align:left"><code>spec.selector.matchLabels</code> セクションに追加したラベルと同じラベルを入力します。 </td>
     </tr>
     <tr>
     <td style="text-align:left"><code>spec.template.spec.affinity</code></td>
     <td style="text-align:left">アンチアフィニティー・ルールを指定して、ステートフル・セット・ポッドが複数のワーカー・ノードとゾーンにまたがって分散されるようにします。 この例では、`app: nginx` ラベルを持つポッドが実行されるワーカー・ノード上でステートフル・セット・ポッドがスケジュールされることが望ましくないアンチアフィニティー・ルールを示しています。 `topologykey: failure-domain.beta.kubernetes.io/zone` によって、このアンチアフィニティー・ルールがさらに制限されて、このポッドが、`app: nginx` ラベルを持つポッドと同じゾーン内のワーカー・ノード上でスケジュールされることが防止されます。 このアンチアフィニティー・ルールを使用すると、複数のワーカー・ノードとゾーンにまたがってアンチアフィニティーを実現できます。 </td>
     </tr>
     <tr>
     <td style="text-align:left"><code>spec.volumeClaimTemplates.metadata.name</code></td>
     <td style="text-align:left">ボリュームの名前を入力します。 <code>spec.containers.volumeMount.name</code> セクションで定義した名前と同じ名前を使用します。 ここに入力した名前が使用されて、PVC の名前が次の形式で作成されます: <code>&lt;volume_name&gt;-&lt;statefulset_name&gt;-&lt;replica_number&gt;</code>。 </td>
     </tr>
     <tr>
     <td style="text-align:left"><code>spec.volumeClaimTemplates.spec.resources.</code></br><code>requests.storage</code></td>
     <td style="text-align:left">ブロック・ストレージのサイズをギガバイト (Gi) 単位で入力します。</td>
     </tr>
     <tr>
     <td style="text-align:left"><code>spec.volumeClaimTemplates.spec.resources.</code></br><code>requests.iops</code></td>
     <td style="text-align:left">[パフォーマンス・ストレージ](#block_predefined_storageclass)をプロビジョンする場合は、IOPS 数を入力します。 エンデュランス・ストレージ・クラスを使用することにして IOPS 数を指定した場合は、IOPS 数が無視されます。 その代わりに、そのストレージ・クラスで指定されている IOPS が使用されます。  </td>
     </tr>
     <tr>
     <td style="text-align:left"><code>spec.volumeClaimTemplates.</code></br><code>spec.storageClassName</code></td>
     <td style="text-align:left">使用するストレージ・クラスを入力します。 既存のストレージ・クラスをリストするには、<code>kubectl get storageclasses | grep block</code> を実行します。 ストレージ・クラスを指定しなかった場合は、クラスターで設定されているデフォルト・ストレージ・クラスを使用して PVC が作成されます。 ブロック・ストレージを使用してステートフル・セットがプロビジョンされるようにするために、デフォルト・ストレージ・クラスで <code>ibm.io/ibmc-block</code> プロビジョナーが使用されていることを確認してください。</td>
     </tr>
     </tbody></table>

4. ステートフル・セットを作成します。
   ```
   kubectl apply -f statefulset.yaml
   ```
   {: pre}

5. ステートフル・セットがデプロイされるまで待ちます。
   ```
   kubectl describe statefulset <statefulset_name>
   ```
   {: pre}

   PVC の現在の状況を確認するには、`kubectl get pvc` を実行します。 PVC の名前は `<volume_name>-<statefulset_name>-<replica_number>` というフォーマットになります。
   {: tip}

### 静的プロビジョニング: ステートフル・セットと組み合わせた既存の PVC の使用
{: #block_static_statefulset}

ステートフル・セットを作成する前に PVC を事前プロビジョンするか、既存の PVC をステートフル・セットで使用することができます。
{: shortdesc}

[ステートフル・セットの作成時に PVC を動的にプロビジョンする](#block_dynamic_statefulset)場合は、ステートフル・セット YAML ファイルで使用した値に基づいて、PVC の名前が割り当てられます。 ステートフル・セットによって既存の PVC が使用されるようにするには、PVC の名前が、動的プロビジョニングを使用する場合に自動的に作成される名前と一致していなければなりません。

開始前に、以下のことを行います。 [アカウントにログインします。 該当する場合は、適切なリソース・グループをターゲットにします。 クラスターのコンテキストを設定します。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1. ステートフル・セットを作成する前に、そのステートフル・セット用の PVC を事前プロビジョンする場合は、[アプリへのブロック・ストレージの追加](#add_block)のステップ 1 から 3 に従って、各ステートフル・セット・レプリカ用の PVC を作成してください。 作成する PVC の名前は、必ず次のフォーマットに従ったものにしてください: `<volume_name>-<statefulset_name>-<replica_number>`。
   - **`<volume_name>`**: ステートフル・セットの `spec.volumeClaimTemplates.metadata.name` セクションで指定する名前を使用します (例: `nginxvol`)。
   - **`<statefulset_name>`**: ステートフル・セットの `metadata.name` セクションで指定する名前を使用します (例: `nginx_statefulset`)。
   - **`<replica_number>`**: 0 から始まる、レプリカの番号を入力します。

   例えば、3 つのステートフル・セット・レプリカを作成する必要がある場合は、次の名前を使用して 3 つの PVC を作成します: `nginxvol-nginx_statefulset-0`、`nginxvol-nginx_statefulset-1`、`nginxvol-nginx_statefulset-2`。  

   既存のストレージ・デバイス用の PVC と PV を作成することを検討している場合は、 [静的プロビジョニング](#existing_block)を使用して PVC と PV を作成してください。

2. [動的プロビジョニング: ステートフル・セット作成時の PVC の作成](#block_dynamic_statefulset)のステップに従って、ステートフル・セットを作成します。 PVC の名前は、`<volume_name>-<statefulset_name>-<replica_number>` という形式に従います。 PVC 名に含まれている次の値を必ず使用してステートフル・セットを指定してください。
   - **`spec.volumeClaimTemplates.metadata.name`**: PVC 名の `<volume_name>` を入力します。
   - **`metadata.name`**: PVC 名の `<statefulset_name>` を入力します。
   - **`spec.replicas`**: 作成するステートフル・セット・レプリカの数を入力します。 レプリカの数は、先ほど作成した PVC の数と等しくなければなりません。

   ご使用の複数の PVC が別々のゾーンにある場合は、ステートフル・セットに地域やゾーンのラベルを含めないでください。
   {: note}

3. PVC がステートフル・セット・レプリカ・ポッドで使用されることを確認します。
   1. クラスター内のポッドをリストします。 ステートフル・セットに属するポッドを確認します。
      ```
      kubectl get pods
      ```
      {: pre}

   2. 既存の PVC がステートフル・セット・レプリカにマウントされていることを確認します。 CLI 出力の **`Volumes`** セクションで **`ClaimName`** を確認してください。
      ```
      kubectl describe pod <pod_name>
      ```
      {: pre}

      出力例:
      ```
      Name:           nginx-0
      Namespace:      default
      Node:           10.xxx.xx.xxx/10.xxx.xx.xxx
      Start Time:     Fri, 05 Oct 2018 13:24:59 -0400
      ...
      Volumes:
        myvol:
          Type:       PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
          ClaimName:  myvol-nginx-0
     ...
      ```
      {: screen}

<br />


## 既存のストレージ・デバイスのサイズと IOPS の変更
{: #block_change_storage_configuration}

ストレージ容量またはパフォーマンスを向上させるために既存のボリュームを変更することができます。
{: shortdesc}

課金方法について不明な点がある場合や {{site.data.keyword.Bluemix_notm}} コンソールを使用してストレージを変更する手順を調べたい場合は、[ブロック・ストレージ容量の拡張](/docs/infrastructure/BlockStorage?topic=BlockStorage-expandingcapacity#expandingcapacity)および [IOPS の調整](/docs/infrastructure/BlockStorage?topic=BlockStorage-adjustingIOPS)を参照してください。 コンソールから行った更新は、永続ボリューム (PV) には反映されません。 この情報を PV に追加するには、`kubectl patch pv <pv_name>` を実行し、PV の **Labels** セクションと **Annotation** セクションで、サイズと IOPS を手動で更新します。
{: tip}

1. クラスターの PVC をリストし、**VOLUME** 列に表示される関連 PV の名前をメモします。
   ```
   kubectl get pvc
   ```
   {: pre}

   出力例:
   ```
   NAME             STATUS    VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS        AGE
   myvol            Bound     pvc-01ac123a-123b-12c3-abcd-0a1234cb12d3   20Gi       RWO            ibmc-block-bronze    147d
   ```
   {: screen}

2. ブロック・ストレージの IOPS とサイズを変更する場合は、まず PV の `metadata.labels.IOPS` セクションの IOPS を編集します。 IOPS 値を小さくしたり、大きくしたりすることができます。 必ず、ご使用のストレージ・タイプでサポートされている IOPS を入力するようにしてください。 例えば、4 IOPS のエンデュランス・ブロック・ストレージがある場合は、IOPS を 2 または 10 に変更できます。 サポート対象の IOPS 値について詳しくは、[ブロック・ストレージ構成の決定](/docs/containers?topic=containers-block_storage#block_predefined_storageclass)を参照してください。
   ```
   kubectl edit pv <pv_name>
   ```
   {: pre}

   CLI から IOPS を変更するには、ブロック・ストレージのサイズを変更する必要もあります。 IOPS のみを変更し、サイズを変更しない場合は、[コンソールから IOPS 変更を要求する](/docs/infrastructure/BlockStorage?topic=BlockStorage-adjustingIOPS)必要があります。
   {: note}

3. PVC を編集し、PVC の `spec.resources.requests.storage` セクションに新しいサイズを追加します。 より大きなサイズに変更する場合は、ストレージ・クラスによって設定された最大容量までのみです。 既存のストレージのサイズを小さくすることはできません。 ストレージ・クラスに使用可能なサイズを確認するには、[ブロック・ストレージ構成の決定](/docs/containers?topic=containers-block_storage#block_predefined_storageclass)を参照してください。
   ```
   kubectl edit pvc <pvc_name>
   ```
   {: pre}

4. ボリューム拡張が要求されていることを確認してください。 CLI 出力の **Conditions** セクションに `FileSystemResizePending` メッセージが表示された場合、ボリューム拡張は正常に要求されています。 
   ```
   kubectl describe pvc <pvc_name>
   ```
   {: pre}

   出力例:
   ```
   ...
   Conditions:
   Type                      Status  LastProbeTime                     LastTransitionTime                Reason  Message
   ----                      ------  -----------------                 ------------------                ------  -------
   FileSystemResizePending   True    Mon, 01 Jan 0001 00:00:00 +0000   Thu, 25 Apr 2019 15:52:49 -0400           Waiting for user to (re-)start a pod to finish file system resize of volume on node.
   ```
   {: screen}

5. PVC をマウントするすべてのポッドをリストします。 PVC がポッドによってマウントされている場合、ボリューム拡張は自動的に処理されます。 PVC がポッドによってマウントされていない場合は、ボリューム拡張を処理できるように、PVC をポッドにマウントする必要があります。 
   ```
   kubectl get pods --all-namespaces -o=jsonpath='{range .items[*]}{"\n"}{.metadata.name}{":\t"}{range .spec.volumes[*]}{.persistentVolumeClaim.claimName}{" "}{end}{end}' | grep "<pvc_name>"
   ```
   {: pre}

   マウントされたポッドは、`<pod_name>: <pvc_name>` の形式で返されます。

6. PVC がポッドによってマウントされていない場合は、[ポッドまたはデプロイメントを作成し、PVC をマウントします](/docs/containers?topic=containers-block_storage#add_block)。 PVC がポッドによってマウントされている場合は、次のステップに進みます。 

7. ボリューム拡張の状況をモニターします。 CLI 出力に `"message":"Success"` メッセージが表示されたら、ボリューム拡張は完了です。
   ```
   kubectl get pv <pv_name> -o go-template=$'{{index .metadata.annotations "ibm.io/volume-expansion-status"}}\n'
   ```
   {: pre}

   出力例:
   ```
   {"size":50,"iops":500,"orderid":38832711,"start":"2019-04-30T17:00:37Z","end":"2019-04-30T17:05:27Z","status":"complete","message":"Success"}
   ```
   {: screen}

8. CLI 出力の **Labels** セクションで、サイズと IOPS が変更されていることを確認します。
   ```
   kubectl describe pv <pv_name>
   ```
   {: pre}

   出力例:
   ```
   ...
   Labels:       CapacityGb=50
                 Datacenter=dal10
                 IOPS=500
   ```
   {: screen}


## データのバックアップとリストア
{: #block_backup_restore}

ブロック・ストレージは、クラスター内のワーカー・ノードと同じロケーションにプロビジョンされます。 サーバーがダウンした場合の可用性を確保するために、IBM は、クラスター化したサーバーでストレージをホストしています。 ただし、ブロック・ストレージは自動的にはバックアップされないため、ロケーション全体で障害が発生した場合はアクセス不能になる可能性があります。 データの損失や損傷を防止するために、定期バックアップをセットアップすると、必要時にバックアップを使用してデータをリストアできます。
{: shortdesc}

ブロック・ストレージのバックアップとリストアのための以下のオプションを検討してください。

<dl>
  <dt>定期的なスナップショットをセットアップする</dt>
  <dd><p>[ブロック・ストレージの定期的なスナップショットをセットアップ](/docs/infrastructure/BlockStorage?topic=BlockStorage-snapshots#snapshots)できます。スナップショットとは、特定の時点のインスタンスの状態をキャプチャーした読み取り専用のイメージです。 スナップショットを保管するには、ブロック・ストレージでスナップショット・スペースを要求する必要があります。 スナップショットは、同じゾーン内の既存のストレージ・インスタンスに保管されます。 ユーザーが誤って重要なデータをボリュームから削除した場合に、スナップショットからデータをリストアできます。</br></br> <strong>ボリュームのスナップショットを作成するには、以下のようにします。</strong><ol><li>[アカウントにログインします。 該当する場合は、適切なリソース・グループをターゲットにします。 クラスターのコンテキストを設定します。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)</li><li>`ibmcloud sl` CLI にログインします。 <pre class="pre"><code>    ibmcloud sl init
    </code></pre></li><li>クラスター内の既存の PV をリストします。 <pre class="pre"><code>kubectl get pv</code></pre></li><li>スナップショット・スペースを作成する PV の詳細を取得し、ボリューム ID、サイズ、および IOPS をメモします。 <pre class="pre"><code>kubectl describe pv &lt;pv_name&gt;</code></pre> サイズと IOPS は CLI 出力の <strong>Labels</strong> セクションに表示されます。 ボリューム ID は、CLI 出力の <code>ibm.io/network-storage-id</code> アノテーションで確認します。 </li><li>前のステップで取得したパラメーターを使用して、既存のボリュームのスナップショット・サイズを作成します。 <pre class="pre"><code>ibmcloud sl block snapshot-order &lt;volume_ID&gt; --size &lt;size&gt; --tier &lt;iops&gt;</code></pre></li><li>スナップショット・サイズが作成されるまで待ちます。 <pre class="pre"><code>ibmcloud sl block volume-detail &lt;volume_ID&gt;</code></pre>CLI 出力の <strong>Snapshot Size (GB)</strong> が 0 から注文したサイズに変更されていれば、スナップショット・サイズは正常にプロビジョンされています。 </li><li>ボリュームのスナップショットを作成し、作成されたスナップショットの ID をメモします。 <pre class="pre"><code>ibmcloud sl block snapshot-create &lt;volume_ID&gt;</code></pre></li><li>スナップショットが正常に作成されたことを確認します。 <pre class="pre"><code>ibmcloud sl block snapshot-list &lt;volume_ID&gt;</code></pre></li></ol></br><strong>スナップショットから既存のボリュームにデータをリストアするには、以下のようにします。</strong><pre class="pre"><code>ibmcloud sl block snapshot-restore &lt;volume_ID&gt; &lt;snapshot_ID&gt;</code></pre></p></dd>
  <dt>スナップショットを別のゾーンにレプリケーションする</dt>
 <dd><p>ゾーンの障害からデータを保護するために、別のゾーンにセットアップしたブロック・ストレージのインスタンスに[スナップショットをレプリケーション](/docs/infrastructure/BlockStorage?topic=BlockStorage-replication#replication)することができます。 データは、1 次ストレージからバックアップ・ストレージにのみレプリケーションできます。 レプリケーションされたブロック・ストレージのインスタンスを、クラスターにマウントすることはできません。 1 次ストレージに障害が発生した場合には、レプリケーションされたバックアップ・ストレージを 1 次ストレージに手動で設定できます。 すると、そのファイル共有をクラスターにマウントできます。 1 次ストレージがリストアされたら、バックアップ・ストレージからデータをリストアできます。</p></dd>
 <dt>ストレージを複製する</dt>
 <dd><p>元のストレージ・インスタンスと同じゾーンに、[ブロック・ストレージ・インスタンスを複製](/docs/infrastructure/BlockStorage?topic=BlockStorage-duplicatevolume#duplicatevolume)できます。 複製インスタンスのデータは、それを作成した時点の元のストレージ・インスタンスと同じです。 レプリカとは異なり、複製インスタンスは、元のインスタンスから独立したストレージ・インスタンスとして使用します。 複製するには、まず、ボリュームのスナップショットをセットアップします。</p></dd>
  <dt>{{site.data.keyword.cos_full}} にデータをバックアップする</dt>
  <dd><p>[**ibm-backup-restore image**](/docs/services/RegistryImages/ibm-backup-restore?topic=RegistryImages-ibmbackup_restore_starter#ibmbackup_restore_starter) を使用して、クラスター内にバックアップとリストアのポッドをスピンアップできます。 このポッドには、クラスター内の任意の永続ボリューム請求 (PVC) のために 1 回限りのバックアップまたは定期バックアップを実行するスクリプトが含まれています。 データは、ゾーンにセットアップした {{site.data.keyword.cos_full}} インスタンスに保管されます。</p><p class="note">ブロック・ストレージは、RWO アクセス・モードでマウントされます。 このアクセスでは、一度に 1 つのポッドしかブロック・ストレージにマウントできません。 データをバックアップするには、ストレージからアプリ・ポッドをアンマウントし、バックアップ・ポッドにマウントしてデータをバックアップしてから、アプリ・ポッドに再マウントする必要があります。 </p>
データを可用性をさらに高め、アプリをゾーン障害から保護するには、2 つ目の {{site.data.keyword.cos_short}} インスタンスをセットアップして、ゾーン間でデータを複製します。 {{site.data.keyword.cos_short}} インスタンスからデータをリストアする必要がある場合は、イメージに付属するリストア・スクリプトを使用します。</dd>
<dt>ポッドおよびコンテナーとの間でデータをコピーする</dt>
<dd><p>`kubectl cp` [コマンド ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/reference/kubectl/overview/#cp) を使用して、クラスター内のポッドまたは特定のコンテナーとの間でファイルとディレクトリーをコピーできます。</p>
<p>開始前に、以下のことを行います。 [アカウントにログインします。 該当する場合は、適切なリソース・グループをターゲットにします。 クラスターのコンテキストを設定します。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure) <code>-c</code> を使用してコンテナーを指定しない場合、コマンドはポッド内で最初に使用可能なコンテナーを使用します。</p>
<p>このコマンドは、以下のようにさまざまな方法で使用できます。</p>
<ul>
<li>ローカル・マシンからクラスター内のポッドにデータをコピーする: <pre class="pre"><code>kubectl cp <var>&lt;local_filepath&gt;/&lt;filename&gt;</var> <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;</var></code></pre></li>
<li>クラスター内のポッドからローカル・マシンにデータをコピーする: <pre class="pre"><code>kubectl cp <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;/&lt;filename&gt;</var> <var>&lt;local_filepath&gt;/&lt;filename&gt;</var></code></pre></li>
<li>ローカル・マシンからクラスター内のポッドで実行される特定のコンテナーにデータをコピーする: <pre class="pre"><code>kubectl cp <var>&lt;local_filepath&gt;/&lt;filename&gt;</var> <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;</var> -c <var>&lt;container&gt;</var></code></pre></li>
</ul></dd>
  </dl>

<br />


## ストレージ・クラス・リファレンス
{: #block_storageclass_reference}

### ブロンズ
{: #block_bronze}

<table>
<caption>ブロック・ストレージ・クラス: ブロンズ</caption>
<thead>
<th>特性</th>
<th>設定</th>
</thead>
<tbody>
<tr>
<td>名前</td>
<td><code>ibmc-block-bronze</code></br><code>ibmc-block-retain-bronze</code></td>
</tr>
<tr>
<td>タイプ</td>
<td>[エンデュランス・ストレージ](/docs/infrastructure/BlockStorage?topic=BlockStorage-About#provendurance)</td>
</tr>
<tr>
<td>ファイル・システム</td>
<td>ext4</td>
</tr>
<tr>
<td>IOPS/GB</td>
<td>2</td>
</tr>
<tr>
<td>サイズの範囲 (ギガバイト単位)</td>
<td>20 から 12000 Gi</td>
</tr>
<tr>
<td>ハード・ディスク</td>
<td>SSD</td>
</tr>
<tr>
<td>課金</td>
<td>デフォルトの課金タイプは、{{site.data.keyword.Bluemix_notm}} Block Storage プラグインのバージョンに応じて異なります。 <ul><li> バージョン 1.0.1 以上: 時間単位</li><li>バージョン 1.0.0 以下: 月単位</li></ul></td>
</tr>
<tr>
<td>料金設定</td>
<td>[料金情報 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/cloud/block-storage/pricing)</td>
</tr>
</tbody>
</table>


### シルバー
{: #block_silver}

<table>
<caption>ブロック・ストレージ・クラス: シルバー</caption>
<thead>
<th>特性</th>
<th>設定</th>
</thead>
<tbody>
<tr>
<td>名前</td>
<td><code>ibmc-block-silver</code></br><code>ibmc-block-retain-silver</code></td>
</tr>
<tr>
<td>タイプ</td>
<td>[エンデュランス・ストレージ](/docs/infrastructure/BlockStorage?topic=BlockStorage-About#provendurance)</td>
</tr>
<tr>
<td>ファイル・システム</td>
<td>ext4</td>
</tr>
<tr>
<td>IOPS/GB</td>
<td>4</td>
</tr>
<tr>
<td>サイズの範囲 (ギガバイト単位)</td>
<td>20 から 12000 Gi</td>
</tr>
<tr>
<td>ハード・ディスク</td>
<td>SSD</td>
</tr>
<tr>
<td>課金</td>
<td>デフォルトの課金タイプは、{{site.data.keyword.Bluemix_notm}} Block Storage プラグインのバージョンに応じて異なります。 <ul><li> バージョン 1.0.1 以上: 時間単位</li><li>バージョン 1.0.0 以下: 月単位</li></ul></td>
</tr>
<tr>
<td>料金設定</td>
<td>[料金情報 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/cloud/block-storage/pricing)</td>
</tr>
</tbody>
</table>

### ゴールド
{: #block_gold}

<table>
<caption>ブロック・ストレージ・クラス: ゴールド</caption>
<thead>
<th>特性</th>
<th>設定</th>
</thead>
<tbody>
<tr>
<td>名前</td>
<td><code>ibmc-block-gold</code></br><code>ibmc-block-retain-gold</code></td>
</tr>
<tr>
<td>タイプ</td>
<td>[エンデュランス・ストレージ](/docs/infrastructure/BlockStorage?topic=BlockStorage-About#provendurance)</td>
</tr>
<tr>
<td>ファイル・システム</td>
<td>ext4</td>
</tr>
<tr>
<td>IOPS/GB</td>
<td>10</td>
</tr>
<tr>
<td>サイズの範囲 (ギガバイト単位)</td>
<td>20 から 4000 Gi</td>
</tr>
<tr>
<td>ハード・ディスク</td>
<td>SSD</td>
</tr>
<tr>
<td>課金</td>
<td>デフォルトの課金タイプは、{{site.data.keyword.Bluemix_notm}} Block Storage プラグインのバージョンに応じて異なります。 <ul><li> バージョン 1.0.1 以上: 時間単位</li><li>バージョン 1.0.0 以下: 月単位</li></ul></td>
</tr>
<tr>
<td>料金設定</td>
<td>[料金情報 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/cloud/block-storage/pricing)</td>
</tr>
</tbody>
</table>

### カスタム
{: #block_custom}

<table>
<caption>ブロック・ストレージ・クラス: カスタム</caption>
<thead>
<th>特性</th>
<th>設定</th>
</thead>
<tbody>
<tr>
<td>名前</td>
<td><code>ibmc-block-custom</code></br><code>ibmc-block-retain-custom</code></td>
</tr>
<tr>
<td>タイプ</td>
<td>[パフォーマンス](/docs/infrastructure/BlockStorage?topic=BlockStorage-About#provperformance)</td>
</tr>
<tr>
<td>ファイル・システム</td>
<td>ext4</td>
</tr>
<tr>
<td>IOPS とサイズ</td>
<td><strong>サイズの範囲 (ギガバイト単位) / IOPS の範囲 (100 の倍数)</strong><ul><li>20 から 39 Gi / 100 から 1000 IOPS</li><li>40 から 79 Gi / 100 から 2000 IOPS</li><li>80 から 99 Gi / 100 から 4000 IOPS</li><li>100 から 499 Gi / 100 から 6000 IOPS</li><li>500 から 999 Gi / 100 から 10000 IOPS</li><li>1000 から 1999 Gi / 100 から 20000 IOPS</li><li>2000 から 2999 Gi / 200 から 40000 IOPS</li><li>3000 から 3999 Gi / 200 から 48000 IOPS</li><li>4000 から 7999 Gi / 300 から 48000 IOPS</li><li>8000 から 9999 Gi / 500 から 48000 IOPS</li><li>10000 から 12000 Gi / 1000 から 48000 IOPS</li></ul></td>
</tr>
<tr>
<td>ハード・ディスク</td>
<td>ギガバイトに対する IOPS の比率によって、プロビジョンされるハード・ディスクのタイプが決まります。 ギガバイトに対する IOPS の比率を求めるには、IOPS をストレージ・サイズで除算します。 </br></br>例: </br>100 IOPS で 500Gi のストレージを選択しました。 比率は 0.2 です (100 IOPS/500Gi)。 </br></br><strong>比率に応じた大まかなハード・ディスクのタイプ</strong><ul><li>0.3 以下: SATA</li><li>0.3 より大: SSD</li></ul></td>
</tr>
<tr>
<td>課金</td>
<td>デフォルトの課金タイプは、{{site.data.keyword.Bluemix_notm}} Block Storage プラグインのバージョンに応じて異なります。 <ul><li> バージョン 1.0.1 以上: 時間単位</li><li>バージョン 1.0.0 以下: 月単位</li></ul></td>
</tr>
<tr>
<td>料金設定</td>
<td>[料金情報 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/cloud/block-storage/pricing)</td>
</tr>
</tbody>
</table>

<br />


## ストレージ・クラスのカスタマイズ例
{: #block_custom_storageclass}

カスタマイズされたストレージ・クラスを作成し、PVC でそのストレージ・クラスを使用することができます。
{: shortdesc}

{{site.data.keyword.containerlong_notm}} では、特定の層と構成のブロック・ストレージをプロビジョンするための、[事前定義ストレージ・クラス](#block_storageclass_reference)が用意されています。 状況に応じて、それらの事前定義ストレージ・クラスではカバーされない、異なる構成のストレージをプロビジョンすることができます。 このトピックの例では、カスタマイズ・ストレージ・クラスのサンプルを示します。

カスタマイズ・ストレージ・クラスを作成するには、[ストレージ・クラスのカスタマイズ](/docs/containers?topic=containers-kube_concepts#customized_storageclass)を参照してください。 それから、[カスタマイズ・ストレージ・クラスを PVC で使用](#add_block)します。

### トポロジー対応ストレージの作成
{: #topology_yaml}

マルチゾーン・クラスターでブロック・ストレージを使用するには、ブロック・ストレージ・インスタンスと同じゾーン内でポッドがスケジュールされる必要があります。その結果として、ボリュームの読み取りと書き込みが可能になります。 Kubernetes でトポロジー対応ボリューム・スケジューリングが導入される前は、ストレージの動的プロビジョニングによって、PVC の作成時にブロック・ストレージ・インスタンスが自動的に作成されていました。 その当時は、ポッドを作成したときに、Kubernetes スケジューラーはそのポッドをブロック・ストレージ・インスタンスと同じデータ・センターにデプロイしようとしていました。
{: shortdesc}

ポッドの制約事項を知らずにブロック・ストレージ・インスタンスを作成すると、望ましくない結果が生じる可能性があります。 例えば、ご使用のストレージと同じワーカー・ノードに対してポッドをスケジュールできない場合があります。その理由は、そのワーカー・ノードのリソースが不十分であるか、そのワーカー・ノードにテイントが適用されているためそのワーカー・ノードではそのポッドのスケジューリングが許可されないからです。 トポロジー対応ボリューム・スケジューリングを使用すると、該当ストレージを使用する最初のポッドが作成されるまで、ブロック・ストレージ・インスタンスが遅延されます。

トポロジー対応ボリューム・スケジューリングは、Kubernetes バージョン 1.12 以降を実行しているクラスターのみでサポートされています。 この機能を使用するには、{{site.data.keyword.Bluemix_notm}} Block Storage プラグイン・バージョン 1.2.0 以降をインストールしている必要があります。
{: note}

次の例では、当該ストレージを使用する最初のポッドがスケジュール可能になるまで、ブロック・ストレージ・インスタンスの作成を遅延させるストレージ・クラスを作成する方法を示しています。 作成を遅延させるには、`volumeBindingMode: WaitForFirstConsumer` オプションを含める必要があります。 このオプションを含めない場合は、`volumeBindingMode` は自動的に `Immediate` に設定されて、PVC の作成時にブロック・ストレージ・インスタンスが作成されます。

- **エンデュランス・ブロック・ストレージの例:**
  ```
  apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
    name: ibmc-block-bronze-delayed
  parameters:
    billingType: hourly
    classVersion: "2"
    fsType: ext4
    iopsPerGB: "2"
    sizeRange: '[20-12000]Gi'
    type: Endurance
  provisioner: ibm.io/ibmc-block
  reclaimPolicy: Delete
  volumeBindingMode: WaitForFirstConsumer
  ```
  {: codeblock}

- **パフォーマンス・ブロック・ストレージの例:**
  ```
  apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
   name: ibmc-block-performance-storageclass
   labels:
     kubernetes.io/cluster-service: "true"
  provisioner: ibm.io/ibmc-block
  parameters:
   billingType: "hourly"
   classVersion: "2"
   sizeIOPSRange: |-
     "[20-39]Gi:[100-1000]"
     "[40-79]Gi:[100-2000]"
     "[80-99]Gi:[100-4000]"
     "[100-499]Gi:[100-6000]"
     "[500-999]Gi:[100-10000]"
     "[1000-1999]Gi:[100-20000]"
     "[2000-2999]Gi:[200-40000]"
     "[3000-3999]Gi:[200-48000]"
     "[4000-7999]Gi:[300-48000]"
     "[8000-9999]Gi:[500-48000]"
     "[10000-12000]Gi:[1000-48000]"
   type: "Performance"
  reclaimPolicy: Delete
  volumeBindingMode: WaitForFirstConsumer
  ```
  {: codeblock}

### ゾーンと地域の指定
{: #block_multizone_yaml}

特定のゾーン内でブロック・ストレージを作成する場合は、カスタマイズされたストレージ・クラス内でそのゾーンと地域を指定できます。
{: shortdesc}

{{site.data.keyword.Bluemix_notm}} Block Storage プラグインのバージョン 1.0.0 を使用する場合、または特定のゾーンで[ブロック・ストレージを静的にプロビジョンする](#existing_block)場合には、カスタマイズ・ストレージ・クラスを使用します。 その他の場合は、[PVC で直接ゾーンを指定してください](#add_block)。
{: note}

次の `.yaml` ファイルでカスタマイズしているストレージ・クラスは、非 retain の `ibm-block-silver` ストレージ・クラスに基づいています。`type` は `"Endurance"`、`iopsPerGB` は `4`、`sizeRange` は `"[20-12000]Gi"`、`reclaimPolicy` の設定は `"Delete"` です。 ゾーンは `dal12` と指定しています。 別のストレージ・クラスをベースとして使用するには、[ストレージ・クラス・リファレンス](#block_storageclass_reference)を参照してください。

ご使用のクラスターおよびワーカー・ノードと同じ地域およびゾーン内にストレージ・クラスを作成します。 クラスターの地域を取得するには、`ibmcloud ks cluster-get --cluster <cluster_name_or_ID>` を実行して、**マスター URL** 内の地域プレフィックスを確認します (例: `https://c2.eu-de.containers.cloud.ibm.com:11111` 内の `eu-de`)。 ワーカー・ノードのゾーンを取得するには、`ibmcloud ks workers --cluster <cluster_name_or_ID>` を実行します。

- **エンデュランス・ブロック・ストレージの例:**
  ```
  apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
    name: ibmc-block-silver-mycustom-storageclass
    labels:
      kubernetes.io/cluster-service: "true"
  provisioner: ibm.io/ibmc-block
  parameters:
    zone: "dal12"
    region: "us-south"
    type: "Endurance"
    iopsPerGB: "4"
    sizeRange: "[20-12000]Gi"
  reclaimPolicy: "Delete"
  ```
  {: codeblock}

- **パフォーマンス・ブロック・ストレージの例:**
  ```
  apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
    name: ibmc-block-performance-storageclass
    labels:
      kubernetes.io/cluster-service: "true"
  provisioner: ibm.io/ibmc-block
  parameters:
    zone: "dal12"
    region: "us-south"
    type: "Performance"
    sizeIOPSRange: |-
      "[20-39]Gi:[100-1000]"
      "[40-79]Gi:[100-2000]"
      "[80-99]Gi:[100-4000]"
      "[100-499]Gi:[100-6000]"
      "[500-999]Gi:[100-10000]"
      "[1000-1999]Gi:[100-20000]"
      "[2000-2999]Gi:[200-40000]"
      "[3000-3999]Gi:[200-48000]"
      "[4000-7999]Gi:[300-48000]"
      "[8000-9999]Gi:[500-48000]"
      "[10000-12000]Gi:[1000-48000]"
  reclaimPolicy: "Delete"
  ```
  {: codeblock}

### `XFS` ファイル・システムを使用してブロック・ストレージをマウントする
{: #xfs}

次の例では、`XFS` ファイル・システムを使用するブロック・ストレージをプロビジョンするストレージ・クラスを作成します。
{: shortdesc}

- **エンデュランス・ブロック・ストレージの例:**
  ```
  apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
    name: ibmc-block-custom-xfs
    labels:
      addonmanager.kubernetes.io/mode: Reconcile
  provisioner: ibm.io/ibmc-block
  parameters:
    type: "Endurance"
    iopsPerGB: "4"
    sizeRange: "[20-12000]Gi"
    fsType: "xfs"
  reclaimPolicy: "Delete"
  ```

- **パフォーマンス・ブロック・ストレージの例:**
  ```
  apiVersion: storage.k8s.io/v1
   kind: StorageClass
   metadata:
     name: ibmc-block-custom-xfs
     labels:
       addonmanager.kubernetes.io/mode: Reconcile
   provisioner: ibm.io/ibmc-block
   parameters:
     type: "Performance"
     sizeIOPSRange: |-
       [20-39]Gi:[100-1000]
       [40-79]Gi:[100-2000]
       [80-99]Gi:[100-4000]
       [100-499]Gi:[100-6000]
       [500-999]Gi:[100-10000]
       [1000-1999]Gi:[100-20000]
       [2000-2999]Gi:[200-40000]
       [3000-3999]Gi:[200-48000]
       [4000-7999]Gi:[300-48000]
       [8000-9999]Gi:[500-48000]
       [10000-12000]Gi:[1000-48000]
    fsType: "xfs"
     reclaimPolicy: "Delete"
     classVersion: "2"
  ```
  {: codeblock}

<br />

