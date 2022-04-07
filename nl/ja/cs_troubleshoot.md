---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-06"

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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}



# クラスターのデバッグ
{: #cs_troubleshoot}

{{site.data.keyword.containerlong}} を使用する際は、ここに示す一般的なクラスターのトラブルシューティング手法とデバッグ手法を検討してください。 [{{site.data.keyword.Bluemix_notm}} システムの状況 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://developer.ibm.com/bluemix/support/#status) を確認することもできます。
{: shortdesc}

以下の一般的な手順を実行して、クラスターが最新の状態であることを確認できます。
- [ワーカー・ノードを更新](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update)するためのセキュリティー・パッチやオペレーティング・システム・パッチが使用可能になっていないか、毎月確認してください。
- クラスターを {{site.data.keyword.containerlong_notm}} 用の [Kubernetes の最新のデフォルト・バージョン](/docs/containers?topic=containers-cs_versions)に[更新](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_update)します<p class="important">[`kubectl` CLI](/docs/containers?topic=containers-cs_cli_install#kubectl) クライアントが、クラスター・サーバーと同じ Kubernetes バージョンであることを確認します。Kubernetes は、サーバーのバージョンから 2 つ以上離れたバージョン (n +/- 2) の `kubectl` クライアントのバージョンは[サポートしません ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/setup/version-skew-policy/)。</p>

## {{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool を使用したテストの実行
{: #debug_utility}

トラブルシューティングの際に、{{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool を使用して、テストを実行し、クラスターから関連情報を収集することができます。 このデバッグ・ツールを使用するには、[`ibmcloud-iks-debug` Helm チャート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/kubernetes/helm/iks-charts/ibmcloud-iks-debug) をインストールします。
{: shortdesc}


1. [クラスターで Helm をセットアップし、Tiller のサービス・アカウントを作成し、Helm インスタンスに `ibm` リポジトリーを追加します](/docs/containers?topic=containers-helm)。

2. Helm チャートをクラスターにインストールします。
  ```
  helm install ibm/ibmcloud-iks-debug --name debug-tool
  ```
  {: pre}


3. デバッグ・ツール・インターフェースを表示するためにプロキシー・サーバーを始動します。
  ```
  kubectl proxy --port 8080
  ```
  {: pre}

4. Web ブラウザーで、デバッグ・ツール・インターフェースの URL (http://localhost:8080/api/v1/namespaces/default/services/debug-tool-ibmcloud-iks-debug:8822/proxy/page) を開きます。

5. 実行する個別のテストまたはテストのグループを選択します。 潜在的な警告、エラー、または問題を検査するテストもあれば、トラブルシューティング中に参照できる情報を収集するだけのテストもあります。 各テストの機能について詳しくは、テストの名前の隣にある情報アイコンをクリックしてください。

6. **「実行 (Run)」**をクリックします。

7. 各テストの結果を確認します。
  * テストが失敗する場合、問題の解決方法について詳しくは、左側の列内のテスト名の隣にある情報アイコンをクリックしてください。
  * 情報 (完全な YAML など) を収集するテストの結果も使用でき、以下のセクションでクラスターをデバッグする際に役立てることができます。

## クラスターのデバッグ
{: #debug_clusters}

クラスターをデバッグするためのオプションを確認し、障害の根本原因を探します。

1.  クラスターをリストし、クラスターの `State` を見つけます。

  ```
  ibmcloud ks clusters
  ```
  {: pre}

2.  クラスターの `State` を確認します。 クラスターが **Critical**、**Delete failed**、または **Warning** 状態の場合、あるいは **Pending** 状態が長時間続いている場合は、[ワーカー・ノードのデバッグ](#debug_worker_nodes)を開始してください。

    現在のクラスターの状態を確認するには、`ibmcloud ks clusters` コマンドを実行して **State** フィールドを見つけます。 
{: shortdesc}

<table summary="表の行はすべて左から右に読みます。1 列目はクラスターの状態、2 列目は説明です。">
<caption>クラスターの状態</caption>
   <thead>
   <th>クラスターの状態</th>
   <th>説明</th>
   </thead>
   <tbody>
<tr>
   <td>`Aborted`</td>
   <td>Kubernetes マスターがデプロイされる前にユーザーからクラスターの削除が要求されました。 クラスターの削除が完了すると、クラスターはダッシュボードから除去されます。 クラスターが長時間この状態になっている場合は、[{{site.data.keyword.Bluemix_notm}} サポート・ケース](/docs/containers?topic=containers-cs_troubleshoot#ts_getting_help)を開いてください。</td>
   </tr>
 <tr>
     <td>`Critical`</td>
     <td>Kubernetes マスターにアクセスできないか、クラスター内のワーカー・ノードがすべてダウンしています。 </td>
    </tr>
   <tr>
     <td>`Delete failed`</td>
     <td>Kubernetes マスターまたは 1 つ以上のワーカー・ノードを削除できません。  </td>
   </tr>
   <tr>
     <td>`Deleted`</td>
     <td>クラスターは削除されましたが、まだダッシュボードからは除去されていません。 クラスターが長時間この状態になっている場合は、[{{site.data.keyword.Bluemix_notm}} サポート・ケース](/docs/containers?topic=containers-cs_troubleshoot#ts_getting_help)を開いてください。 </td>
   </tr>
   <tr>
   <td>`Deleting`</td>
   <td>クラスターの削除とクラスター・インフラストラクチャーの破棄を実行中です。 クラスターにアクセスできません。  </td>
   </tr>
   <tr>
     <td>`Deploy failed`</td>
     <td>Kubernetes マスターのデプロイメントを完了できませんでした。 この状態はお客様には解決できません。 [{{site.data.keyword.Bluemix_notm}} サポート・ケース](/docs/containers?topic=containers-cs_troubleshoot#ts_getting_help)を開いて、IBM Cloud サポートに連絡してください。</td>
   </tr>
     <tr>
       <td>`Deploying`</td>
       <td>Kubernetes マスターがまだ完全にデプロイされていません。 クラスターにアクセスできません。 クラスターが完全にデプロイされるまで待ってからクラスターの正常性を確認してください。</td>
      </tr>
      <tr>
       <td>`Normal`</td>
       <td>クラスター内のすべてのワーカー・ノードが稼働中です。 クラスターにアクセスし、アプリをクラスターにデプロイできます。 この状態は正常と見なされるので、アクションは必要ありません。<p class="note">ワーカー・ノードは正常であっても、[ネットワーキング](/docs/containers?topic=containers-cs_troubleshoot_network)や[ストレージ](/docs/containers?topic=containers-cs_troubleshoot_storage)などの他のインフラストラクチャー・リソースには注意が必要な可能性もあります。 クラスターの作成直後は、Ingress シークレットやレジストリー・イメージ・プル・シークレットなど、他のサービスによって使用されるクラスターの一部が、まだ処理中である場合があります。</p></td>
    </tr>
      <tr>
       <td>`Pending`</td>
       <td>Kubernetes マスターはデプロイされています。 ワーカー・ノードはプロビジョン中であるため、まだクラスターでは使用できません。 クラスターにはアクセスできますが、アプリをクラスターにデプロイすることはできません。  </td>
     </tr>
   <tr>
     <td>`Requested`</td>
     <td>クラスターを作成し、Kubernetes マスターとワーカー・ノードのインフラストラクチャーを注文するための要求が送信されました。 クラスターのデプロイメントが開始されると、クラスターの状態は「<code>Deploying</code>」に変わります。 クラスターが長時間「<code>Requested</code>」状態になっている場合は、[{{site.data.keyword.Bluemix_notm}} サポート・ケース](/docs/containers?topic=containers-cs_troubleshoot#ts_getting_help)を開いてください。 </td>
   </tr>
   <tr>
     <td>`Updating`</td>
     <td>Kubernetes マスターで実行される Kubernetes API サーバーが、新しい Kubernetes API バージョンに更新されています。 更新中、クラスターにアクセスすることも変更することもできません。 ユーザーがデプロイしたワーカー・ノード、アプリ、リソースは変更されず、引き続き実行されます。 更新が完了するまで待ってから、クラスターの正常性を確認してください。 </td>
   </tr>
   <tr>
    <td>`Unsupported`</td>
    <td>クラスターで実行される [Kubernetes バージョン](/docs/containers?topic=containers-cs_versions#cs_versions)はサポートされなくなりました。クラスターの正常性は、アクティブにモニターされなくなり、報告もされなくなりました。また、ワーカー・ノードを追加したり再ロードしたりすることはできません。重要なセキュリティー更新およびサポートを引き続き受けるには、クラスターを更新する必要があります。[バージョン更新の準備アクション](/docs/containers?topic=containers-cs_versions#prep-up)を確認してから、サポートされている Kubernetes バージョンに[クラスターを更新](/docs/containers?topic=containers-update#update)します。<br><br><p class="note">最も古いサポート対象バージョンより 3 バージョン以上古いバージョンのクラスターを更新することはできません。この状況を回避するには、現行バージョンより 1 バージョンまたは 2 バージョンだけ新しい Kubernetes バージョンに (1.12 から 1.14 へなど) クラスターを更新します。さらに、クラスターでバージョン 1.5、1.7、または 1.8 が実行されている場合、そのバージョンは古すぎるため更新できません。代わりに、[クラスターを作成](/docs/containers?topic=containers-clusters#clusters)して、そのクラスターに[アプリをデプロイ](/docs/containers?topic=containers-app#app)する必要があります。</p></td>
   </tr>
    <tr>
       <td>`Warning`</td>
       <td>クラスター内の 1 つ以上のワーカー・ノードが使用不可です。ただし、他のワーカー・ノードが使用可能であるため、ワークロードを引き継ぐことができます。 </td>
    </tr>
   </tbody>
 </table>


[Kubernetes マスター](/docs/containers?topic=containers-ibm-cloud-kubernetes-service-technology#architecture)は、クラスターを稼働状態に保つための主要なコンポーネントです。 マスターは、クラスターの真実の単一点 (Single Point of Truth) として機能する etcd データベースに、クラスター・リソースとその構成を保管します。 Kubernetes API サーバーは、ワーカー・ノードからマスターへのすべてのクラスター管理要求、またはクラスター・リソースと対話する場合のメインエントリー・ポイントです。<br><br>マスターに障害が発生した場合、ワークロードは引き続きワーカー・ノードで実行されますが、`kubectl` コマンドを使用してクラスター・リソースを操作したり、マスターの Kubernetes API サーバーがバックアップされるまでクラスターの正常性を表示したりすることはできません。 マスターの障害時にポッドがダウンすると、ワーカー・ノードが再び Kubernetes API サーバーに到達できるまで、ポッドをスケジュール変更することはできません。<br><br>マスターの障害時にも、`ibmcloud ks` コマンドを {{site.data.keyword.containerlong_notm}} API に対して実行して、ワーカー・ノードや VLAN などのインフラストラクチャー・リソースを操作することができます。 クラスターに対してワーカー・ノードを追加または削除して現在のクラスター構成を変更する場合、マスターがバックアップされるまで変更は行われません。

マスターの障害時はワーカー・ノードを再始動またはリブートしないでください。 このアクションにより、ワーカー・ノードからポッドが削除されます。 Kubernetes API サーバーが使用不可のため、ポッドをクラスター内の他のワーカー・ノードにスケジュール変更することはできません。
{: important}


<br />


## ワーカー・ノードのデバッグ
{: #debug_worker_nodes}

ワーカー・ノードをデバッグするためのオプションを確認し、障害の根本原因を探します。

<ol><li>クラスターが **Critical**、**Delete failed**、または **Warning** 状態の場合、あるいは **Pending** 状態が長時間続いている場合は、ワーカー・ノードの状態を確認してください。<p class="pre">ibmcloud ks workers --cluster <cluster_name_or_id></p></li>
<li>CLI 出力内ですべてのワーカー・ノードの **State** フィールドと **Status** フィールドを確認します。<p>現在のワーカー・ノードの状態を確認するには、`ibmcloud ks workers --cluster <cluster_name_or_ID` コマンドを実行して **State** と **Status** の各フィールドを見つけます。
{: shortdesc}

<table summary="表の行はすべて左から右に読みます。1 列目はクラスターの状態、2 列目は説明です。">
<caption>ワーカー・ノードの状態</caption>
  <thead>
  <th>ワーカー・ノードの状態</th>
  <th>説明</th>
  </thead>
  <tbody>
<tr>
    <td>`Critical`</td>
    <td>ワーカー・ノードは、次のようなさまざまな理由で Critical 状態になることがあります。 <ul><li>閉鎖と排出を行わずに、ワーカー・ノードのリブートを開始した。。 ワーカー・ノードをリブートすると、<code>containerd</code>、<code>kubelet</code>、<code>kube-proxy</code>、および <code>calico</code> でデータ破損が発生する可能性があります。 </li>
    <li>ワーカー・ノードにデプロイしたポッドが、[メモリー ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/tasks/configure-pod-container/assign-memory-resource/) と [CPU ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/tasks/configure-pod-container/assign-cpu-resource/) のリソース制限を使用していない。 リソース制限を使用しないと、ポッドが、使用可能なリソースをすべて使い果たして、このワーカー・ノード上の他のポッドを実行するためのリソースがなくなる可能性があります。 この過剰なワークロードにより、ワーカー・ノードに障害が発生します。 </li>
    <li>数百、数千ものコンテナーを長時間実行した後、<code>containerd</code>、<code>kubelet</code>、または <code>calico</code> がリカバリー不能な状態になった。 </li>
    <li>ワーカー・ノード用にセットアップした Virtual Router Appliance が停止したために、ワーカー・ノードと Kubernetes マスターの間の通信が切断された。 </li><li> {{site.data.keyword.containerlong_notm}} または IBM Cloud インフラストラクチャー (SoftLayer) の現在のネットワーキングの問題によって、ワーカー・ノードと Kubernetes マスターが通信できなくなっている。</li>
    <li>ワーカー・ノードが容量を使い尽くした。 ワーカー・ノードの <strong>Status</strong> に <strong>Out of disk</strong> または <strong>Out of memory</strong> と表示されていないか確認します。 ワーカー・ノードが容量を使い尽くしている場合は、ワーカー・ノードのワークロードを減らすか、ワークロードの負荷を分散できるようにクラスターにワーカー・ノードを追加してください。</li>
    <li>デバイスが [{{site.data.keyword.Bluemix_notm}} コンソールのリソース・リスト ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/resources) で電源オフにされた。 リソース・リストを開き、**「デバイス」**リストでワーカー・ノード ID を見つけます。 アクション・メニューで、**「パワーオン」**をクリックします。</li></ul>
    多くの場合、ワーカー・ノードを[再ロードする](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)と問題を解決できます。 ワーカー・ノードを再ロードすると、最新の[パッチ・バージョン](/docs/containers?topic=containers-cs_versions#version_types)がワーカー・ノードに適用されます。 メジャー・バージョンとマイナー・バージョンは変更されません。 必ず、ワーカー・ノードを再ロードする前に、ワーカー・ノードを閉鎖して排出してください。これにより、既存のポッドが正常終了し、残りのワーカー・ノードに再スケジュールされます。 </br></br> ワーカー・ノードを再ロードしても問題が解決しない場合は、次の手順に進み、ワーカー・ノードのトラブルシューティングを続けてください。 </br></br><strong>ヒント:</strong> [ワーカー・ノードのヘルス・チェックを構成し、Autorecovery を有効にする](/docs/containers?topic=containers-health#autorecovery)ことができます。 Autorecovery は、構成された検査に基づいて正常でないワーカー・ノードを検出すると、ワーカー・ノードの OS の再ロードのような修正アクションをトリガーします。 Autorecovery の仕組みについて詳しくは、[Autorecovery のブログ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/blogs/bluemix/2017/12/autorecovery-utilizes-consistent-hashing-high-availability/) を参照してください。
    </td>
   </tr>
   <tr>
   <td>`Deployed`</td>
   <td>更新はワーカー・ノードに正常にデプロイされました。 更新がデプロイされた後、{{site.data.keyword.containerlong_notm}} によって、ワーカー・ノードでヘルス・チェックが開始されます。 ヘルス・チェックが正常に完了したら、ワーカー・ノードは <code>Normal</code> 状態になります。 <code>Deployed</code> 状態のワーカー・ノードは、通常、ワークロードを受信する準備ができています。これをチェックするには、<code>kubectl get nodes</code> を実行して状態に <code>Normal</code> が表示されることを確認します。 </td>
   </tr>
    <tr>
      <td>`Deploying`</td>
      <td>ワーカー・ノードの Kubernetes バージョンを更新する際には、更新をインストールするためにワーカー・ノードが再デプロイされます。 ワーカー・ノードを再ロードまたはリブートすると、ワーカー・ノードが再デプロイされて、最新のパッチ・バージョンが自動的にインストールされます。 ワーカー・ノードが長時間この状態になっている場合は、次のステップに進み、デプロイメント中に問題が発生したかどうかを調べてください。 </td>
   </tr>
      <tr>
      <td>`Normal`</td>
      <td>ワーカー・ノードは完全にプロビジョンされ、クラスターで使用できる状態です。 この状態は正常と見なされるので、ユーザーのアクションは必要ありません。 **注**: ワーカー・ノードは正常であっても、[ネットワーキング](/docs/containers?topic=containers-cs_troubleshoot_network)や[ストレージ](/docs/containers?topic=containers-cs_troubleshoot_storage)などの他のインフラストラクチャー・リソースには注意が必要な可能性もあります。</td>
   </tr>
 <tr>
      <td>`Provisioning`</td>
      <td>ワーカー・ノードはプロビジョン中であるため、まだクラスターでは使用できません。 CLI 出力の <strong>Status</strong> 列で、プロビジョニングのプロセスをモニターできます。 ワーカー・ノードが長時間この状態になっている場合は、次のステップに進み、プロビジョニング中に問題が発生したかどうかを調べてください。</td>
    </tr>
    <tr>
      <td>`Provision_failed`</td>
      <td>ワーカー・ノードをプロビジョンできませんでした。 次のステップに進んで、障害に関する詳細を調べてください。</td>
    </tr>
 <tr>
      <td>`Reloading`</td>
      <td>ワーカー・ノードは再ロード中であるため、クラスターでは使用できません。 CLI 出力の <strong>Status</strong> 列で、再ロードのプロセスをモニターできます。 ワーカー・ノードが長時間この状態になっている場合は、次のステップに進み、再ロード中に問題が発生したかどうかを調べてください。</td>
     </tr>
     <tr>
      <td>`Reloading_failed`</td>
      <td>ワーカー・ノードを再ロードできませんでした。 次のステップに進んで、障害に関する詳細を調べてください。</td>
    </tr>
    <tr>
      <td>`Reload_pending `</td>
      <td>ワーカー・ノードの Kubernetes バージョンを再ロードまたは更新する要求が送信されました。 ワーカー・ノードが再ロードされると、状態は「<code>Reloading</code>」に変わります。 </td>
    </tr>
    <tr>
     <td>`Unknown`</td>
     <td>次のいずれかの理由で、Kubernetes マスターにアクセスできません。<ul><li>Kubernetes マスターの更新を要求しました。 更新中は、ワーカー・ノードの状態を取得できません。 Kubernetes マスターが正常に更新された後でもワーカー・ノードが長期間この状態のままである場合は、ワーカー・ノードの[再ロード](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)を試行してください。</li><li>ワーカー・ノードを保護している別のファイアウォールが存在するか、最近ファイアウォールの設定を変更した可能性があります。 {{site.data.keyword.containerlong_notm}} では、ワーカー・ノードと Kubernetes マスター間で通信を行うには、特定の IP アドレスとポートが開いている必要があります。 詳しくは、[ファイアウォールがあるためにワーカー・ノードが接続しない](/docs/containers?topic=containers-cs_troubleshoot_clusters#cs_firewall)を参照してください。</li><li>Kubernetes マスターがダウンしています。 [{{site.data.keyword.Bluemix_notm}} サポート・ケース](/docs/containers?topic=containers-cs_troubleshoot#ts_getting_help)を開いて、{{site.data.keyword.Bluemix_notm}} サポートに連絡してください。</li></ul></td>
</tr>
   <tr>
      <td>`Warning`</td>
      <td>ワーカー・ノードが、メモリーまたはディスク・スペースの限度に達しています。 ワーカー・ノードのワークロードを減らすか、またはワークロードを分散できるようにクラスターにワーカー・ノードを追加してください。</td>
</tr>
  </tbody>
</table>
</p></li>
<li>ワーカー・ノードの詳細情報をリストします。 詳細情報にエラー・メッセージが含まれている場合は、[ワーカー・ノードに関する一般的なエラー・メッセージ](#common_worker_nodes_issues)のリストを参照して、問題の解決方法を確認してください。<p class="pre">ibmcloud ks worker-get --cluster <cluster_name_or_id> --worker <worker_node_id></li>
</ol>

<br />


## ワーカー・ノードに関する一般的な問題
{: #common_worker_nodes_issues}

一般的なエラー・メッセージを確認し、解決方法を調べます。

  <table>
  <caption>一般的なエラー・メッセージ</caption>
    <thead>
    <th>エラー・メッセージ</th>
    <th>説明と解決
    </thead>
    <tbody>
      <tr>
        <td>{{site.data.keyword.Bluemix_notm}} Infrastructure Exception: Your account is currently prohibited from ordering 'Computing Instances'.</td>
        <td>ご使用の IBM Cloud インフラストラクチャー (SoftLayer) アカウントは、コンピュート・リソースの注文を制限されている可能性があります。 [{{site.data.keyword.Bluemix_notm}} サポート・ケース](#ts_getting_help)を開いて、{{site.data.keyword.Bluemix_notm}} サポートに連絡してください。</td>
      </tr>
      <tr>
      <td>{{site.data.keyword.Bluemix_notm}} infrastructure exception: Could not place order.<br><br>
      {{site.data.keyword.Bluemix_notm}} Infrastructure Exception: Could not place order. There are insufficient resources behind router 'router_name' to fulfill the request for the following guests: 'worker_id'.</td>
      <td>選択したゾーンに、ワーカー・ノードをプロビジョンするための十分なインフラストラクチャー容量がない可能性があります。 または、IBM Cloud インフラストラクチャー (SoftLayer) アカウントの制限を超えた可能性があります。 解決するには、以下のいずれかのオプションを試してください。
      <ul><li>ゾーン内のインフラストラクチャー・リソースの可用性は、頻繁に変動します。 数分待ってから、再試行してください。</li>
      <li>単一ゾーン・クラスターの場合は、別のゾーンにクラスターを作成します。 複数ゾーン・クラスターの場合は、クラスターにゾーンを追加します。</li>
      <li>IBM Cloud インフラストラクチャー (SoftLayer) アカウントで、ワーカー・ノードに対してパブリック VLAN とプライベート VLAN の異なるペアを指定します。 ワーカー・プール内にあるワーカー・ノードの場合は、<code>ibmcloud ks zone-network-set</code> [コマンド](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_zone_network_set)を使用できます。</li>
      <li>IBM Cloud インフラストラクチャー (SoftLayer) アカウント・マネージャーに連絡して、グローバルな割り当て量などのアカウント制限を超えないことを確認してください。</li>
      <li>[IBM Cloud インフラストラクチャー (SoftLayer) サポート・ケース](#ts_getting_help)を開きます。</li></ul></td>
      </tr>
      <tr>
        <td>{{site.data.keyword.Bluemix_notm}} Infrastructure Exception: Could not obtain network VLAN with ID: <code>&lt;vlan id&gt;</code>.</td>
        <td>次のいずれかの理由で、選択した VLAN ID が見つからなかったため、ワーカー・ノードをプロビジョンできませんでした。<ul><li>VLAN ID ではなく VLAN 番号を指定した可能性があります。 VLAN 番号の長さは 3 桁または 4 桁ですが、VLAN ID の長さは 7 桁です。 VLAN ID を取得するには、<code>ibmcloud ks vlans --zone &lt;zone&gt;</code> を実行してください。<li>ご使用の IBM Cloud インフラストラクチャー (SoftLayer) アカウントに VLAN ID が関連付けられていない可能性があります。 アカウントの使用可能な VLAN ID をリストするには、<code>ibmcloud ks vlans --zone &lt;zone&gt;</code> を実行します。 IBM Cloud インフラストラクチャー (SoftLayer) アカウントを変更するには、[`ibmcloud ks credential-set`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_set) を参照してください。 </ul></td>
      </tr>
      <tr>
        <td>SoftLayer_Exception_Order_InvalidLocation: The location provided for this order is invalid. (HTTP 500)</td>
        <td>ご使用の IBM Cloud インフラストラクチャー (SoftLayer) は、選択したデータ・センター内のコンピュート・リソースを注文するようにセットアップされていません。 [{{site.data.keyword.Bluemix_notm}} サポート](#ts_getting_help)に問い合わせて、アカウントが正しくセットアップされているか確認してください。</td>
       </tr>
       <tr>
        <td>{{site.data.keyword.Bluemix_notm}} Infrastructure Exception: The user does not have the necessary {{site.data.keyword.Bluemix_notm}} Infrastructure permissions to add servers
        </br></br>
        {{site.data.keyword.Bluemix_notm}} Infrastructure Exception: 'Item' must be ordered with permission.
        </br></br>
        The {{site.data.keyword.Bluemix_notm}} infrastructure credentials could not be validated.</td>
        <td>ご使用の IBM Cloud インフラストラクチャー (SoftLayer) ポートフォリオでアクションを実行するために必要なアクセス権限がない可能性があるか、または正しくないインフラストラクチャー資格情報を使用しています。 [API キーのセットアップによるインフラストラクチャー・ポートフォリオへのアクセスの有効化](/docs/containers?topic=containers-users#api_key)を参照してください。</td>
      </tr>
      <tr>
       <td>Worker unable to talk to {{site.data.keyword.containerlong_notm}} servers. Please verify your firewall setup is allowing traffic from this worker.
       <td><ul><li>ファイアウォールがある場合は、[該当するポートと IP アドレスへの発信トラフィックを許可するようにファイアウォール設定を構成します](/docs/containers?topic=containers-firewall#firewall_outbound)。</li>
       <li>`ibmcloud ks workers &lt;mycluster&gt;` を実行して、クラスターにパブリック IP が含まれていないかどうかを確認します。 パブリック IP がリストされない場合、クラスターにはプライベート VLAN だけがあります。
       <ul><li>クラスターにプライベート VLAN のみが含まれるようにするには、[VLAN 接続](/docs/containers?topic=containers-plan_clusters#private_clusters)と[ファイアウォール](/docs/containers?topic=containers-firewall#firewall_outbound)をセットアップします。</li>
       <li>[VRF](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) および[サービス・エンドポイント](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started)のアカウントを有効にする前に、プライベート・サービス・エンドポイントのみでクラスターを作成した場合、ワーカーはマスターに接続できません。[パブリック・サービス・エンドポイントのセットアップ](/docs/containers?topic=containers-cs_network_cluster#set-up-public-se)を試行して、サポート・ケースが処理されてアカウントが更新されるまで、クラスターを使用できるようにします。アカウントの更新後もプライベート・サービス・エンドポイントのみのクラスターが必要な場合は、パブリック・サービス・エンドポイントを無効にすることができます。</li>
       <li>パブリック IP があるクラスターにするには、パブリック VLAN とプライベート VLAN の両方を指定して[新しいワーカー・ノードを追加](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_add)します。</li></ul></li></ul></td>
     </tr>
      <tr>
  <td>Cannot create IMS portal token, as no IMS account is linked to the selected BSS account</br></br>Provided user not found or active</br></br>SoftLayer_Exception_User_Customer_InvalidUserStatus: User account is currently cancel_pending.</br></br>Waiting for machine to be visible to the user</td>
  <td>IBM Cloud インフラストラクチャー (SoftLayer) ポートフォリオへのアクセスに使用される API キーの所有者には、このアクションを実行するために必要な権限がありません。あるいは、その所有者が削除の保留中である可能性があります。</br></br><strong>ユーザーは</strong>、以下の手順に従ってください。
  <ol><li>複数のアカウントを利用できる場合は、{{site.data.keyword.containerlong_notm}} を操作したいアカウントにログインしていることを確認します。 </li>
  <li><code>ibmcloud ks api-key-info --cluster &lt;cluster_name_or_ID&gt;</code> を実行して、IBM Cloud インフラストラクチャー (SoftLayer) ポートフォリオへのアクセスに使用される現在の API キーの所有者を表示します。</li>
  <li><code>ibmcloud account list</code> を実行して、現在使用している {{site.data.keyword.Bluemix_notm}} アカウントの所有者を表示します。 </li>
  <li>{{site.data.keyword.Bluemix_notm}} アカウントの所有者に連絡して、API キーの所有者に IBM Cloud インフラストラクチャー (SoftLayer) での十分な権限がないか、その所有者が削除の保留中である可能性があることを報告します。 </li></ol>
  </br><strong>アカウント所有者は</strong>、以下の手順に従ってください。
  <ol><li>先ほど失敗したアクションを実行するために [IBM Cloud インフラストラクチャー (SoftLayer) に必要な権限](/docs/containers?topic=containers-users#infra_access)を確認します。 </li>
  <li>API キーの所有者の権限を修正するか、[<code>ibmcloud ks api-key-reset --region <region></code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_api_key_reset) コマンドを使用して新しい API キーを作成します。</li>
  <li>自分または別のアカウント管理者が、IBM Cloud インフラストラクチャー (SoftLayer) の資格情報を手動でアカウントに設定した場合は、[<code>ibmcloud ks credential-unset --region <region></code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_unset) を実行してアカウントから資格情報を削除します。</li></ol></td>
  </tr>
    </tbody>
  </table>

<br />


## マスターの正常性の確認
{: #debug_master}

{{site.data.keyword.containerlong_notm}} には可用性の高いレプリカを備えた IBM 管理のマスターが含まれ、このマスターには、セキュリティー・パッチ更新を自動的に適用する機能と、インシデントの発生時に自動的にリカバリーする機能があります。このクラスター・マスターの正常性、状況、および状態を確認するには、`ibmcloud ks cluster-get --cluster <cluster_name_or_ID>` を実行します。
{: shortdesc} 

**Master Health**<br>
**Master Health** にはマスター・コンポーネントの状態が反映され、対処が必要な場合に通知します。health (正常性) は次のいずれかになります。
*   `error`: マスターは作動不可です。IBM は自動的に通知を受け、この問題を解決するためのアクションを実行します。マスターが `normal` になるまで、正常性のモニタリングを続行できます。
*   `normal`: マスターは作動可能で、正常です。アクションは不要です。
*   `unavailable`: マスターがアクセス不能になっている可能性があります。その場合は、ワーカー・プールのサイズ変更などのいくつかのアクションを一時的に実行できません。IBM は自動的に通知を受け、この問題を解決するためのアクションを実行します。マスターが `normal` になるまで、正常性のモニタリングを続行できます。 
*   `unsupported`: マスターでは、サポートされないバージョンの Kubernetes が実行されています。マスターの正常性を `normal` に戻すには、[クラスターを更新](/docs/containers?topic=containers-update)する必要があります。

**Master Status (状況) と Master State (状態)**<br>
**Master Status** には、master state からどの操作が進行中であるかについて、詳細が示されます。この status (状況) には、マスターが同じ state (状態) で保たれた期間を示すタイム・スタンプが含まれます (例: `Ready (1 month ago)`)。**Master State** には、マスター上で実行できる操作 (デプロイ、更新、削除など) のライフサイクルが反映されます。次の表では、各状態について説明しています。

<table summary="表の行はすべて左から右に読みます。1 列目は master state、2 列目は説明です。">
<caption>Master state</caption>
   <thead>
   <th>Master state</th>
   <th>説明</th>
   </thead>
   <tbody>
<tr>
   <td>`deployed`</td>
   <td>マスターは正常にデプロイされています。状況をチェックして、マスターが `Ready` であることを確認するか、更新を利用可能かどうかを確認します。</td>
   </tr>
 <tr>
     <td>`deploying`</td>
     <td>マスターは現在デプロイ中です。クラスターに対するワーカー・ノードの追加などの操作は、状態が `deployed` になった後に行ってください。</td>
    </tr>
   <tr>
     <td>`deploy_failed`</td>
     <td>マスターをデプロイできませんでした。IBM サポートが通知を受けて、問題の解決に取り組みます。**Master Status** フィールドで詳細情報を確認するか、状態が `deployed` になるまで待ちます。</td>
   </tr>
   <tr>
   <td>`deleting`</td>
   <td>クラスターを削除したため、マスターは現在削除中です。削除を元に戻すことはできません。クラスターを削除すると、クラスターは完全に除去されるため、マスターの状態を確認できなくなります。</td>
   </tr>
     <tr>
       <td>`delete_failed`</td>
       <td>マスターを削除できませんでした。IBM サポートが通知を受けて、問題の解決に取り組みます。クラスターの再削除を試行しても、この問題を解決することはできません。代わりに、**Master Status** フィールドで詳細情報を確認するか、クラスターの削除を待ちます。</td>
      </tr>
      <tr>
       <td>`updating`</td>
       <td>マスターの Kubernetes バージョンを更新中です。この更新は、自動的に適用されるパッチ更新であるか、クラスターを更新することで手動で適用したマイナー・バージョンまたはメジャー・バージョンである可能性があります。この更新中に、高可用性マスターは要求の処理を続行でき、アプリのワークロードとワーカー・ノードは引き続き実行されます。マスターの更新が完了したら、[ワーカー・ノードを更新](/docs/containers?topic=containers-update#worker_node)できます。<br><br>
       更新が失敗すると、マスターは `deployed` 状態に戻り、前のバージョンの実行を続行します。IBM サポートが通知を受けて、問題の解決に取り組みます。**Master Status** フィールドで、更新が失敗したかどうかを確認できます。</td>
    </tr>
   </tbody>
 </table>


<br />



## アプリ・デプロイメントのデバッグ
{: #debug_apps}

アプリ・デプロイメントをデバッグするためのオプションを確認し、障害の根本原因を探します。

始める前に、アプリがデプロイされている名前空間に対する[**ライター**または**管理者**の {{site.data.keyword.Bluemix_notm}} IAM サービス役割](/docs/containers?topic=containers-users#platform)があることを確認してください。

1. `describe` コマンドを実行して、サービス・リソースまたはデプロイメント・リソース内の異常を見つけます。

 例:
 <pre class="pre"><code>kubectl describe service &lt;service_name&gt; </code></pre>

2. [コンテナーが `ContainerCreating` 状態で停滞しているかどうかを確認します](/docs/containers?topic=containers-cs_troubleshoot_storage#stuck_creating_state)。

3. クラスターが `Critical` 状態かどうかを確認します。 クラスターが `Critical` 状態の場合、ファイアウォール・ルールを調べて、マスターがワーカー・ノードと通信できることを検査します。

4. サービスが正しいポートで listen していることを確認します。
   1. ポッドの名前を取得します。
     <pre class="pre"><code>kubectl get pods</code></pre>
   2. コンテナーにログインします。
     <pre class="pre"><code>kubectl exec -it &lt;pod_name&gt; -- /bin/bash</code></pre>
   3. コンテナー内からアプリに対して curl を実行します。 ポートにアクセスできない場合、サービスは正しいポートで listen していないか、またはアプリに問題が生じている可能性があります。 正しいポートを指定してサービスの構成ファイルを更新して再デプロイするか、またはアプリに存在する可能性がある問題を調査します。
     <pre class="pre"><code>curl localhost: &lt;port&gt;</code></pre>

5. サービスがポッドに正しくリンクされていることを確認します。
   1. ポッドの名前を取得します。
     <pre class="pre"><code>kubectl get pods</code></pre>
   2. コンテナーにログインします。
     <pre class="pre"><code>kubectl exec -it &lt;pod_name&gt; -- /bin/bash</code></pre>
   3. サービスのクラスター IP アドレスとポートに対して curl を実行します。 IP アドレスとポートにアクセスできない場合、サービスのエンドポイントを調べます。 エンドポイントがリストされない場合は、サービスのセレクターがポッドと一致していません。 エンドポイントがリストされている場合は、サービスの宛先ポート・フィールドを調べて、宛先ポートがポッドに使用されているポートと同じであることを確認します。
     <pre class="pre"><code>curl &lt;cluster_IP&gt;:&lt;port&gt;</code></pre>

6. Ingress サービスの場合、クラスター内からサービスにアクセスできることを検証します。
   1. ポッドの名前を取得します。
     <pre class="pre"><code>kubectl get pods</code></pre>
   2. コンテナーにログインします。
     <pre class="pre"><code>kubectl exec -it &lt;pod_name&gt; -- /bin/bash</code></pre>
   2. Ingress サービス用に指定された URL に対して curl を実行します。 URL にアクセスできない場合、クラスターと外部エンドポイントの間にファイアウォールの問題が生じていないかを調べます。 
     <pre class="pre"><code>curl &lt;host_name&gt;.&lt;domain&gt;</code></pre>

<br />



## ヘルプとサポートの取得
{: #ts_getting_help}

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

