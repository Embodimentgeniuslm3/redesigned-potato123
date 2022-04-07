---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: kubernetes, iks, node scaling

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



# クラスターのスケーリング
{: #ca}

{{site.data.keyword.containerlong_notm}} の `ibm-iks-cluster-autoscaler` プラグインを使用すると、スケジュールされたワークロードのサイズ要件に応じて、クラスター内のワーカー・プールを自動的にスケーリングして、ワーカー・プール内のワーカー・ノード数を増減できます。 `ibm-iks-cluster-autoscaler` プラグインは、[Kubernetes クラスター自動スケーリング機能のプロジェクト ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/autoscaler/tree/master/cluster-autoscaler) をベースにしています。
{: shortdesc}

代わりにポッドの自動スケーリングを実行したい場合は、 [アプリのスケーリング](/docs/containers?topic=containers-app#app_scaling)を参照してください。
{: tip}

クラスター自動スケーリング機能は、パブリック・ネットワーク接続がセットアップされている標準クラスターで使用できます。 ファイアウォールの内側にあるプライベート・クラスターや、プライベート・サービス・エンドポイントしか有効にされていないクラスターなどのように、パブリック・ネットワークにアクセスできないクラスターでは、クラスター自動スケーリング機能は使用できません。
{: important}

## スケールアップ/スケールダウンについて
{: #ca_about}

クラスター自動スケーリング機能は、クラスターを定期的にスキャンし、ユーザーが構成したワークロード・リソース要求やカスタム設定 (スキャン間隔など) に応じて、管理対象のワーカー・プール内のワーカー・ノードの数を調整します。 クラスター自動スケーリング機能は、毎分、以下の状況が発生していないか検査します。
{: shortdesc}

*   **スケールアップすべき保留ポッド**: ワーカー・ノードにポッドをスケジュールできるだけの十分なコンピュート・リソースが存在しない場合、そのポッドは保留ポッドと見なされます。 クラスター自動スケーリング機能は保留ポッドを検出すると、ワークロード・リソース要求を満たすために、ゾーン間で均等にワーカー・ノード数を増やします。
*   **スケールダウンすべき低使用率のワーカー・ノード**: デフォルトでは、ワーカー・ノードが、要求された合計コンピュート・リソースの 50% 未満の状態で 10 分以上実行されていて、ワークロードを他のワーカー・ノードにスケジュール変更できる場合、このワーカー・ノードを、低使用率のワーカー・ノードと見なします。 クラスター自動スケーリング機能は、低使用率のワーカー・ノードを検出すると、必要なコンピュート・リソースだけを使用するように、ワーカー・ノードを 1 つずつ減らしていきます。 必要に応じて、デフォルトのスケールダウン使用率しきい値 (50% が 10 分間) を[カスタマイズ](/docs/containers?topic=containers-ca#ca_chart_values)できます。

スキャンとスケールアップ/スケールダウンは一定の間隔で継続して行われ、ワーカー・ノード数によっては、完了までに長い時間がかかることがあります (30 分など)。

クラスター自動スケーリング機能は、実際のワーカー・ノードの使用状況ではなく、デプロイメントに対してユーザーが定義した [リソース要求 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/) を考慮して、ワーカー・ノード数を調整します。 ポッドとデプロイメントから適切な量のリソースが要求されない場合は、ユーザーがそれらの構成ファイルを調整する必要があります。 クラスター自動スケーリング機能でそれらを自動的に調整することはできません。 また、ワーカー・ノードは、クラスターの基本機能、デフォルトとカスタムの[アドオン](/docs/containers?topic=containers-update#addons)、および[リソース予約](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node)のためにコンピュート・リソースの一部を使用することにも注意してください。
{: note}

<br>
**スケールアップ/スケールダウンはどのように行われるのですか?**<br>
一般的に、クラスター自動スケーリング機能は、クラスターのワークロードを実行するために必要なワーカー・ノード数を計算します。 クラスターのスケールアップ/スケールダウンは、以下のようなさまざまな要因によって決定されます。
*   ユーザーが設定した 1 ゾーンあたりの最小および最大ワーカー・ノード・サイズ。
*   保留ポッドのリソース要求、ワークロードにユーザーが関連付けた特定のメタデータ (アンチアフィニティー、特定のマシン・タイプにのみポッドを配置するためのラベル、[PodDisruptionBudget ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/concepts/workloads/pods/disruptions/) など)。
*   クラスター自動スケーリング機能が管理するワーカー・プール。[マルチゾーン・クラスター](/docs/containers?topic=containers-ha_clusters#multizone)の複数のゾーンにまたがっている場合があります。
*   設定されている[カスタム Helm チャート値](#ca_chart_values) (ワーカー・ノードでローカル・ストレージが使用されている場合は削除をスキップするなど)。

詳しくは、Kubernetes クラスターの自動スケーリング機能 FAQ の [How does scale-up work? ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#how-does-scale-up-work) および [How does scale-down work? ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#how-does-scale-down-work) を参照してください。

<br>

**スケールアップ/スケールダウンの仕組みを変更できますか?**<br>
設定をカスタマイズしたり、スケールアップ/スケールダウンの仕組みに影響を及ぼす他の Kubernetes リソースを使用したりできます。
*   **スケールアップ**: `scanInterval`、`expander`、`skipNodes`、`maxNodeProvisionTime` などの[クラスター自動スケーリング機能の Helm チャート値をカスタマイズします](#ca_chart_values)。 ワーカー・プールのリソースが不足する前にワーカー・ノードをスケールアップできるように、[ワーカー・ノードのオーバープロビジョン](#ca_scaleup)の方法を検討します。 また、[Kubernetes のポッド中断の割り当て量とポッド優先度のカットオフを設定して](#scalable-practices-apps)、スケールアップの仕組みに影響を与えることができます。
*   **スケールダウン**: `scaleDownUnneededTime`、`scaleDownDelayAfterAdd`、`scaleDownDelayAfterDelete`、`scaleDownUtilizationThreshold` などの[クラスター自動スケーリング機能の Helm チャート値をカスタマイズします](#ca_chart_values)。

<br>
**ゾーンあたりの最小サイズを設定することで、クラスターをそのサイズに即時にスケールアップできますか?**<br>
いいえ。`minSize` を設定しても、自動的にスケールアップはトリガーされません。`minSize` は、クラスター自動スケーリング機能によって、ゾーンあたりのワーカー・ノードが一定数を下回るようにスケーリングされることを防止するためのしきい値です。クラスターで、ゾーンあたりの数がまだこれに達していない場合は、より多くのリソースを必要とするワークロード・リソース要求が発生するまで、クラスター自動スケーリング機能によってスケールアップされることはありません。例えば、3 つのゾーンあたり 1 つのワーカー・ノードが含まれた 1 つのワーカー・プールがある場合に (合計 3 つのワーカー・ノード)、`minSize` をゾーンあたり `4` に設定した場合は、クラスター自動スケーリング機能によって、ゾーンあたりの追加の 3 つのワーカー・ノード (合計 12 個のワーカー・ノード) が即時にプロビジョンされることはありません。代わりに、リソース要求によってスケールアップがトリガーされます。15 個のワーカー・ノードのリソースを要求するワークロードを作成した場合、クラスター自動スケーリング機能はこの要求を満たすようにワーカー・プールをスケールアップします。ここでの `minSize` は、ゾーンあたり 4 つのワーカー・ノードを要求するワークロードを手動で削除した場合でも、クラスター自動スケーリング機能がこのゾーンあたりの数を下回るようにスケールダウンしないことを指定します。

<br>
**この動作と、クラスター自動スケーリング機能で管理されないワーカー・プールの違いは何ですか?**<br>
[ワーカー・プールを作成](/docs/containers?topic=containers-add_workers#add_pool)するときには、1 ゾーンあたりのワーカー・ノード数を指定します。 ワーカー・プールのワーカー・ノード数は、ユーザーがワーカー・プールの[サイズを変更する](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_resize)か、または[再バランス化](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_rebalance)するまで変わりません。 ワーカー・プールがワーカー・ノードを自動的に追加/削除することはありません。 スケジュールできる数より多くのポッドがある場合は、ユーザーがワーカー・プールのサイズを変更するまで、余分なポッドは保留状態のままです。

ワーカー・プールに対してクラスター自動スケーリング機能を有効にすると、ポッドの仕様の設定とリソース要求に応じてワーカー・ノード数が増減されます。 手動でワーカー・プールをサイズ変更したり再バランス化したりする必要はありません。

<br>
**クラスター自動スケーリング機能のスケールアップ/スケールダウンの例を見ることはできますか?**<br>
クラスターのスケールアップ/スケールダウンの例として、次の図を参考にしてください。

_図: クラスターの自動スケールアップ/スケールダウン。_
![クラスターの自動スケールアップ/スケールダウンの GIF](images/cluster-autoscaler-x3.gif){: gif}

1.  このクラスターは、2 つのゾーンにまたがる 2 つのワーカー・プール内に 4 つのワーカー・ノードを持っています。 どちらのプールもゾーンごとに 1 つのワーカー・ノードが存在しますが、**ワーカー・プール A** のマシン・タイプは `u2c.2x4`、**ワーカー・プール B** のマシン・タイプは `b2c.4x16` です。 コンピュート・リソースの合計は約 10 コアです (**ワーカー・プール A** は 2 コア x 2 ワーカー・ノード、**ワーカー・プール B** は 4 コア x 2 ワーカー・ノード)。 クラスターが現在実行しているワークロードは、これらの 10 個のコアのうち 6 個を要求しています。 各ワーカー・ノード上で、クラスター、ワーカー・ノード、アドオン (クラスター自動スケーリング機能など) を実行するために必要な[予約済みリソース](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node)により、他にもコンピューティング・リソースが消費されます。
2.  両方のワーカー・プールを管理するために、クラスター自動スケーリング機能には、1 ゾーンあたりの最小サイズと最大サイズが次のように構成されています。
    *  **ワーカー・プール A**: `minSize=1`、`maxSize=5`。
    *  **ワーカー・プール B**: `minSize=1`、`maxSize=2`。
3.  アプリのポッド・レプリカを追加で 14 個必要とするデプロイメントをスケジュールに入れます。このアプリはレプリカごとに 1 コアの CPU を要求します。 1 個のポッド・レプリカは現在のリソースでデプロイできますが、残りの 13 個は保留状態になります。
4.  クラスター自動スケーリング機能は、次の制約の範囲でワーカー・ノード数を増やして、追加の 13 個のポッド・レプリカ・リソース要求をサポートします。
    *  **ワーカー・プール A**: 7 台のワーカー・ノードをゾーン間で可能な限り均等にラウンドロビン方式で追加します。 それらのワーカー・ノードにより、クラスターのコンピュート容量が約 14 コア分 (2 コア x 7 ワーカー・ノード) 増えます。
    *  **ワーカー・プール B**: 2 台のワーカー・ノードをゾーン間で均等に追加します。これにより、1 ゾーンあたりの `maxSize` である 2 ワーカー・ノードに達します。 それらのワーカー・ノードにより、クラスター容量が約 8 コア分 (4 コア x 2 ワーカー・ノード) 増えます。
5.  1 コアを要求するポッド 20 個が、以下のようにワーカー・ノード間に分散されます。 ワーカー・ノードには、リソースの予約、およびデフォルトのクラスター機能をカバーするために実行されるポッドがあるので、ワークロード用のポッドが、ワーカー・ノードの使用可能なコンピュート・リソースをすべて使用することはできません。例えば、`b2c.4x16` ワーカー・ノードには 4 つのコアがありますが、それぞれに最低 1 つのコアを要求するポッドを 3 つしかスケジュールできません。
    <table summary="スケーリングされたクラスター内のワークロードの分散を説明する表。">
    <caption>スケーリングされたクラスター内でのワークロードの分散</caption>
    <thead>
    <tr>
      <th>ワーカー・プール</th>
      <th>ゾーン</th>
      <th>タイプ</th>
      <th>ワーカー・ノード数</th>
      <th>ポッド数</th>
    </tr>
    </thead>
    <tbody>
    <tr>
      <td>ア</td>
      <td>dal10</td>
      <td>u2c.2x4</td>
      <td>4 つのノード</td>
      <td>3 つのポッド</td>
    </tr>
    <tr>
      <td>ア</td>
      <td>dal12</td>
      <td>u2c.2x4</td>
      <td>5 つのノード</td>
      <td>5 つのポッド</td>
    </tr>
    <tr>
      <td>B</td>
      <td>dal10</td>
      <td>b2c.4x16</td>
      <td>2 つのノード</td>
      <td>6 つのポッド</td>
    </tr>
    <tr>
      <td>B</td>
      <td>dal12</td>
      <td>b2c.4x16</td>
      <td>2 つのノード</td>
      <td>6 つのポッド</td>
    </tr>
    </tbody>
    </table>
6.  この追加のワークロードが不要になったので、デプロイメントを削除します。 少し経つと、クラスター自動スケーリング機能が、クラスターの一部のコンピュート・リソースが不要になったことを検出し、ワーカー・ノードを 1 つずつ減らしていきます。
7.  ワーカー・プールがスケールダウンされます。 クラスター自動スケーリング機能は、保留ポッドのリソース要求や低使用率のワーカー・ノードがないか定期的にスキャンして、ワーカー・プールをスケールアップ/スケールダウンします。

## スケーラブル・デプロイメントの実践の遵守
{: #scalable-practices}

ワーカー・ノード戦略とワークロード・デプロイメント戦略に従うことによって、クラスター自動スケーリング機能を最大限に活用します。 詳しくは、[Kubernetes クラスターの自動スケーリング機能 FAQ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md) を参照してください。
{: shortdesc}

少数のテスト・ワークロードを使用して[クラスター自動スケーリング機能を試し](#ca_helm)、[スケールアップ/スケールダウンの仕組み](#ca_about)、構成する[カスタム値](#ca_chart_values)、および必要になりそうなその他の方法 (ワーカー・ノードの[オーバープロビジョン](#ca_scaleup)や[アプリの制限](#ca_limit_pool)など) についての感覚をつかみます。 その後、テスト環境をクリーンアップして、クラスター自動スケーリング機能のフレッシュ・インストールでこれらのカスタム値や追加設定を組み込む計画を立てます。

### 複数のワーカー・プールをすぐに自動スケーリングできますか?
{: #scalable-practices-multiple}
はい。Helm チャートをインストールした後、[構成マップで](#ca_cm)クラスター内のどのワーカー・プールを自動スケーリングするかを選択できます。 クラスターごとに実行できる `ibm-iks-cluster-autoscaler` Helm チャートは 1 つだけです。
{: shortdesc}

### どうすればクラスター自動スケーリング機能が、アプリで必要なリソースに対応して確実に実行されますか?
{: #scalable-practices-resrequests}

クラスター自動スケーリング機能は、ワークロードの[リソース要求 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/) に応じてクラスターをスケーリングします。 つまり、クラスター自動スケーリング機能は、ワークロードの実行に必要なワーカー・ノード数を計算するためにリソース要求を使用するので、すべてのデプロイメントに[リソース要求 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/) を指定してください。 自動スケーリング機能は、ワークロード構成によって要求されるコンピュートの使用量に基づいており、マシン・コストなどの他の要因を考慮していないことに注意してください。
{: shortdesc}

### ワーカー・プールをゼロ (0) ノードまでスケールダウンできますか?
{: #scalable-practices-zero}

いいえ。クラスター自動スケーリング機能の `minSize` を `0` に設定することはできません。また、クラスターの各ゾーンですべてのパブリック・アプリケーション・ロード・バランサー (ALB) を[無効](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_configure)にしている場合を除き、ALB ポッドを分散させて高可用性を確保するために、1 ゾーンあたりの `minSize` を `2` ワーカー・ノードに変更する必要があります。
{: shortdesc}

### 自動スケーリングのためにデプロイメントを最適化できますか?
{: #scalable-practices-apps}

はい。デプロイメントにいくつかの Kubernetes 機能を追加して、クラスター自動スケーリング機能によってスケーリングのためにリソース要求がどのように考慮されるかを調整できます。
{: shortdesc}
*   [PodDisruptionBudget ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/concepts/workloads/pods/disruptions/) を使用して、ポッドの突然の再スケジュールや削除を回避します。
*   ポッドの優先度を使用している場合は、[優先度のカットオフを編集 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#how-does-cluster-autoscaler-work-with-pod-priority-and-preemption) して、スケールアップをトリガーする優先度のタイプを変更できます。 デフォルトでは、優先度のカットオフはゼロ (`0`) です。

### 自動スケーリングされたワーカー・プールでテイントと容認を使用できますか?
{: #scalable-practices-taints}

テイントはワーカー・プール・レベルでは適用できないので、予期しない結果を避けるために[ワーカー・ノードにテイントを適用](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/)しないでください。 例えば、テイントを適用したワーカー・ノードでは許容されないワークロードをデプロイすると、そのワーカー・ノードはスケールアップ対象とは見なされないので、クラスターに十分な容量があっても追加のワーカー・ノードが注文されることがあります。 一方、テイントを適用したワーカー・ノードでも、使用されているリソースがしきい値 (デフォルトでは 50%) 未満であるものは、低使用率ワーカー・ノードとして検出され、スケールダウンの対象と見なされます。
{: shortdesc}

### 自動スケーリングされたワーカー・プールがアンバランスになるのはなぜですか?
{: #scalable-practices-unbalanced}

スケールアップ時に、クラスター自動スケーリング機能はゾーン間でノード数のバランスを取ります。許容されるワーカー・ノード数の差はプラス・マイナス 1 (+/- 1) です。 保留中のワークロードが要求している容量が少ないために、各ゾーンのバランスが取れていない可能性があります。 そのような場合に手動でワーカー・プールのバランスを取るには、アンバランスなワーカー・プールを削除するように[クラスター自動スケーリング機能の構成マップを更新](#ca_cm)します。 次に、`ibmcloud ks worker-pool-rebalance` [コマンド](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_rebalance)を実行して、そのワーカー・プールをクラスター自動スケーリング機能の構成マップに戻します。
{: shortdesc}


### ワーカー・プールのサイズ変更も再バランス化もできないのはなぜですか?
{: #scalable-practices-resize}

クラスター自動スケーリング機能を有効にしたワーカー・プールは、[サイズ変更](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_resize)も[再バランス化](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_rebalance)もできません。 [構成マップを編集](#ca_cm)して、ワーカー・プールの最小/最大サイズを変更するか、そのワーカー・プールのクラスター自動スケーリングを無効にする必要があります。 `ibmcloud ks worker-rm` [コマンド](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_rm)を使用してワーカー・プールから個々のワーカー・ノードを削除しないでください。この操作を行うと、ワーカー・プールがアンバランスになる可能性があります。
{: shortdesc}

さらに、ワーカー・プールを無効にせずに `ibm-iks-cluster-autoscaler` Helm チャートをアンインストールした場合は、ワーカー・プールのサイズを手動で変更することはできません。 `ibm-iks-cluster-autoscaler` Helm チャートを再インストールし、[構成マップを編集](#ca_cm)してワーカー・プールを無効にしてから、やり直してください。

<br />


## クラスター自動スケーリング機能の Helm チャートをクラスターにデプロイする
{: #ca_helm}

クラスター内のワーカー・プールを自動でスケーリングするには、Helm チャートで {{site.data.keyword.containerlong_notm}} クラスター自動スケーリング機能プラグインをインストールします。
{: shortdesc}

**始める前に**:

1.  [必要な CLI とプラグインをインストール](/docs/cli?topic=cloud-cli-getting-started)します。
    *  {{site.data.keyword.Bluemix_notm}} CLI (`ibmcloud`)
    *  {{site.data.keyword.containerlong_notm}} プラグイン (`ibmcloud ks`)
    *  {{site.data.keyword.registrylong_notm}} プラグイン (`ibmcloud cr`)
    *  Kubernetes (`kubectl`)
    *  Helm (`helm`)
2.  **Kubernetes バージョン1.12 以降**を実行する[標準クラスターを作成](/docs/containers?topic=containers-clusters#clusters_ui)します。
3.   [アカウントにログインします。 該当する場合は、適切なリソース・グループをターゲットにします。 クラスターのコンテキストを設定します。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
4.  {{site.data.keyword.Bluemix_notm}} の ID/アクセス管理の資格情報がクラスターに保管されていることを確認します。 クラスター自動スケーリング機能はこのシークレットを使用して資格情報を認証します。シークレットが欠落している場合は、[資格情報をリセットすることでシークレットを作成します](/docs/containers?topic=containers-cs_troubleshoot_storage#missing_permissions)。
    ```
    kubectl get secrets -n kube-system | grep storage-secret-store
    ```
    {: pre}
5.  クラスター自動スケーリング機能は、`ibm-cloud.kubernetes.io/worker-pool-id` ラベルを持つワーカー・プールのみをスケーリングできます。
    1.  ワーカー・プールに必要なラベルがあるかどうかを確認します。
        ```
        ibmcloud ks worker-pool-get --cluster <cluster_name_or_ID> --worker-pool <worker_pool_name_or_ID> | grep Labels
        ```
        {: pre}

        ラベル付きのワーカー・プールの出力例:
        ```
        Labels:             ibm-cloud.kubernetes.io/worker-pool-id=a1aa111111b22b22cc3c3cc444444d44-4d555e5
        ```
        {: screen}
    2.  ワーカー・プールに必要なラベルがない場合は、[新しいワーカー・プールを追加](/docs/containers?topic=containers-add_workers#add_pool)し、クラスター自動スケーリング機能で使用します。


<br>
**クラスターに `ibm-iks-cluster-autoscaler` プラグインをインストールするには、以下のようにします。**

1.  [この手順に従い](/docs/containers?topic=containers-helm#public_helm_install)、ローカル・マシンに **Helm バージョン 2.11 以降**のクライアントをインストールし、サービス・アカウントを使用して Helm サーバー (tiller) をクラスターにインストールします。
2.  tiller がサービス・アカウントでインストールされていることを確認します。

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
3.  クラスター自動スケーリング機能の Helm チャートがある Helm リポジトリーを追加および更新します。
    ```
    helm repo add iks-charts https://icr.io/helm/iks-charts
    ```
    {: pre}
    ```
    helm repo update
    ```
    {: pre}
4.  クラスター自動スケーリング機能の Helm チャートをクラスターの `kube-system` 名前空間にインストールします。

    インストール中に、さらに[クラスター自動スケーリング機能の設定をカスタマイズ](#ca_chart_values)できます (ワーカー・ノードをスケールアップまたはスケールダウンするまでの待機時間など)。
    {: tip}

    ```
    helm install iks-charts/ibm-iks-cluster-autoscaler --namespace kube-system --name ibm-iks-cluster-autoscaler
    ```
    {: pre}

    出力例:
    ```
    NAME:   ibm-iks-cluster-autoscaler
    LAST DEPLOYED: Thu Nov 29 13:43:46 2018
    NAMESPACE: kube-system
    STATUS: DEPLOYED

    RESOURCES:
    ==> v1/ClusterRole
    NAME                       AGE
    ibm-iks-cluster-autoscaler  1s

    ==> v1/Pod(related)

    NAME                                        READY  STATUS             RESTARTS  AGE
    ibm-iks-cluster-autoscaler-67c8f87b96-qbb6c  0/1    ContainerCreating  0         1s

    ==> v1/ConfigMap

    NAME              AGE
    iks-ca-configmap  1s

    ==> v1/ClusterRoleBinding
    ibm-iks-cluster-autoscaler  1s

    ==> v1/Role
    ibm-iks-cluster-autoscaler  1s

    ==> v1/RoleBinding
    ibm-iks-cluster-autoscaler  1s

    ==> v1/Service
    ibm-iks-cluster-autoscaler  1s

    ==> v1beta1/Deployment
    ibm-iks-cluster-autoscaler  1s

    ==> v1/ServiceAccount
    ibm-iks-cluster-autoscaler  1s

    NOTES:
    Thank you for installing: ibm-iks-cluster-autoscaler. Your release is named: ibm-iks-cluster-autoscaler

    クラスター自動スケーリング機能の使用方法の詳細については、チャートの README.md ファイルを参照してください。
    ```
    {: screen}

5.  インストールが成功したことを確認します。

    1.  クラスター自動スケーリング機能ポッドが**「Running」**の状態であることを確認します。
        ```
        kubectl get pods --namespace=kube-system | grep ibm-iks-cluster-autoscaler
        ```
        {: pre}
        出力例:
        ```
        ibm-iks-cluster-autoscaler-8497bfc968-dbn7w   1/1       Running   0          9m
        ```
        {: screen}
    2.  クラスター自動スケーリング機能サービスが作成されていることを確認します。
        ```
        kubectl get service --namespace=kube-system | grep ibm-iks-cluster-autoscaler
        ```
        {: pre}
        出力例:
        ```
        ibm-iks-cluster-autoscaler   ClusterIP   172.21.xxx.xx    <none>        8085/TCP        9m
        ```
        {: screen}

6.  クラスター自動スケーリング機能をプロビジョンするすべてのクラスターに対して、上記の手順を繰り返します。

7.  ワーカー・プールのスケーリングを開始するには、[クラスター自動スケーリング機能の構成の更新](#ca_cm)を参照してください。

<br />


## スケーリングを有効にするためにクラスター自動スケーリング機能の構成マップを更新する
{: #ca_cm}

設定した最小値と最大値に基づいて、ワーカー・プール内のワーカー・ノードを自動的にスケーリングできるように、クラスター自動スケーリング機能の構成マップを更新します。
{: shortdesc}

構成マップを編集してワーカー・プールを有効にすると、クラスター自動スケーリング機能はワークロード要求に応じてクラスターをスケーリングします。そのため、ワーカー・プールの[サイズ変更](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_resize)や[再バランス化](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_rebalance)はできません。 スキャンとスケールアップ/スケールダウンは一定の間隔で継続して行われ、ワーカー・ノード数によっては、完了までに長い時間がかかることがあります (30 分など)。 後から[クラスター自動スケーリング機能を削除](#ca_rm)するには、まず構成マップで各ワーカー・プールを無効にする必要があります。
{: note}

**始める前に**:
*  [`ibm-iks-cluster-autoscaler` プラグイン](#ca_helm)をインストールします。
*  [アカウントにログインします。 該当する場合は、適切なリソース・グループをターゲットにします。 クラスターのコンテキストを設定します。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

**クラスター自動スケーリング機能の構成マップと値を更新するには**:

1.  クラスター自動スケーリング機能の構成マップ YAML ファイルを編集します。
    ```
    kubectl edit cm iks-ca-configmap -n kube-system -o yaml
    ```
    {: pre}
    出力例:
    ```
    apiVersion: v1
    data:
      workerPoolsConfig.json: |
        [
         {"name": "default","minSize": 1,"maxSize": 2,"enabled":false}
        ]
    kind: ConfigMap
    metadata:
      creationTimestamp: 2018-11-29T18:43:46Z
      name: iks-ca-configmap
      namespace: kube-system
      resourceVersion: "2227854"
      selfLink: /api/v1/namespaces/kube-system/configmaps/iks-ca-configmap
      uid: b45d047b-f406-11e8-b7f0-82ddffc6e65e
    ```
    {: screen}
2.  パラメーターを使用して構成マップを編集し、クラスター自動スケーリング機能でクラスター・ワーカー・プールをどのようにスケーリングするかを定義します。 **注:** 標準クラスターの各ゾーンですべてのパブリック・アプリケーション・ロード・バランサー (ALB) を[無効](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_configure)にしている場合を除き、ALB ポッドを分散させて高可用性を確保するために、1 ゾーンあたりの `minSize` を `2` に変更する必要があります。

    <table>
    <caption>クラスター自動スケーリング機能の構成マップ・パラメーター</caption>
    <thead>
    <th id="parameter-with-default">パラメーターとデフォルト値</th>
    <th id="parameter-with-description">説明</th>
    </thead>
    <tbody>
    <tr>
    <td id="parameter-name" headers="parameter-with-default">`"name": "default"`</td>
    <td headers="parameter-name parameter-with-description">`"default"` を、スケーリングするワーカー・プールの名前または ID に置き換えます。 ワーカー・プールをリストするには、`ibmcloud ks worker-pools --cluster <cluster_name_or_ID>` を実行します。<br><br>
    複数のワーカー・プールを管理するには、次のように JSON 行をコンマ区切りの行としてコピーします。 <pre class="codeblock">[
     {"name": "default","minSize": 1,"maxSize": 2,"enabled":false},
     {"name": "Pool2","minSize": 2,"maxSize": 5,"enabled":true}
    ]</pre><br><br>
    **注**: クラスター自動スケーリング機能は、`ibm-cloud.kubernetes.io/worker-pool-id` ラベルを持つワーカー・プールのみをスケーリングできます。 ワーカー・プールに必要なラベルがあるかどうかを確認するには、`ibmcloud ks worker-pool-get --cluster <cluster_name_or_ID> --worker-pool <worker_pool_name_or_ID> | grep Labels` を実行します。 ワーカー・プールに必要なラベルがない場合は、[新しいワーカー・プールを追加](/docs/containers?topic=containers-add_workers#add_pool)し、クラスター自動スケーリング機能で使用します。</td>
    </tr>
    <tr>
    <td id="parameter-minsize" headers="parameter-with-default">`"minSize": 1`</td>
    <td headers="parameter-minsize parameter-with-description">ゾーンあたりの最小ワーカー・ノード数を指定します。クラスター自動スケーリング機能はこの最小数を下限として、ワーカー・プールをスケールダウンできます。ALB ポッドを分散させて高可用性を確保するためには、値を `2` 以上にする必要があります。 標準クラスターの各ゾーンですべてのパブリック ALB を[無効](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_configure)にした場合は、この値を `1` に設定できます。
<p class="note">`minSize` を設定しても、自動的にスケールアップはトリガーされません。`minSize` は、クラスター自動スケーリング機能によって、ゾーンあたりのワーカー・ノードが一定数を下回るようにスケーリングされることを防止するためのしきい値です。クラスターで、ゾーンあたりの数がまだこれに達していない場合は、より多くのリソースを必要とするワークロード・リソース要求が発生するまで、クラスター自動スケーリング機能によってスケールアップされることはありません。例えば、3 つのゾーンあたり 1 つのワーカー・ノードが含まれた 1 つのワーカー・プールがある場合に (合計 3 つのワーカー・ノード)、`minSize` をゾーンあたり `4` に設定した場合は、クラスター自動スケーリング機能によって、ゾーンあたりの追加の 3 つのワーカー・ノード (合計 12 個のワーカー・ノード) が即時にプロビジョンされることはありません。代わりに、リソース要求によってスケールアップがトリガーされます。15 個のワーカー・ノードのリソースを要求するワークロードを作成した場合、クラスター自動スケーリング機能はこの要求を満たすようにワーカー・プールをスケールアップします。ここでの `minSize` は、ゾーンあたり 4 つのワーカー・ノードを要求するワークロードを手動で削除した場合でも、クラスター自動スケーリング機能がこのゾーンあたりの数を下回るようにスケールダウンしないことを指定します。詳しくは、[Kubernetes の資料 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#when-does-cluster-autoscaler-change-the-size-of-a-cluster) を参照してください。</p></td>
    </tr>
    <tr>
    <td id="parameter-maxsize" headers="parameter-with-default">`"maxSize": 2`</td>
    <td headers="parameter-maxsize parameter-with-description">ゾーンあたりの最大ワーカー・ノード数を指定します。クラスター自動スケーリング機能はこの最大数を上限として、ワーカー・プールをスケールアップできます。値は、`minSize` に設定した値以上でなければなりません。</td>
    </tr>
    <tr>
    <td id="parameter-enabled" headers="parameter-with-default">`"enabled": false`</td>
    <td headers="parameter-enabled parameter-with-description">値を `true` に設定すると、クラスター自動スケーリング機能でそのワーカー・プールのスケーリングを管理できます。 値を `false` に設定すると、クラスター自動スケーリング機能でワーカー・プールをスケーリングできなくなります。<br><br>
    後から[クラスター自動スケーリング機能を削除](#ca_rm)するには、まず構成マップで各ワーカー・プールを無効にする必要があります。</td>
    </tr>
    </tbody>
    </table>
3.  構成ファイルを保存します。
4.  クラスター自動スケーリング機能のポッドを表示します。
    ```
    kubectl get pods -n kube-system
    ```
    {: pre}
5.  クラスター自動スケーリング機能ポッドの**`「Events」`**セクションで **`ConfigUpdated`** イベントを探し、構成マップが正常に更新されたことを確認します。 構成マップのイベント・メッセージが、`minSize:maxSize:PoolName:<SUCCESS|FAILED>:error message` という形式で記録されています。

    ```
    kubectl describe pod -n kube-system <cluster_autoscaler_pod>
    ```
    {: pre}

    出力例:
    ```
		Name:               ibm-iks-cluster-autoscaler-857c4d9d54-gwvc6
		Namespace:          kube-system
		...
		Events:
    Type    Reason                 Age   From                     Message
		----     ------         ----  ----                                        -------

		Normal  ConfigUpdated  3m    ibm-iks-cluster-autoscaler-857c4d9d54-gwvc6  {"1:3:default":"SUCCESS:"}
    ```
    {: screen}

## クラスター自動スケーリング機能の Helm チャート構成値をカスタマイズする
{: #ca_chart_values}

ワーカー・ノード数を増減するまでの待機時間など、クラスター自動スケーリング機能の設定をカスタマイズします。
{: shortdesc}

**始める前に**:
*  [アカウントにログインします。 該当する場合は、適切なリソース・グループをターゲットにします。 クラスターのコンテキストを設定します。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
*  [`ibm-iks-cluster-autoscaler` プラグイン](#ca_helm)をインストールします。

**クラスター自動スケーリング機能の値を更新するには**:

1.  クラスター自動スケーリング機能の Helm チャートの構成値を確認します。 クラスター自動スケーリング機能にはデフォルト設定があります。 ただし、クラスターのワークロードを変更する頻度に応じて、スケールダウンやスキャンの間隔など、いくつかの値を変更したほうが良い場合があります。
    ```
    helm get values ibm-iks-cluster-autoscaler -a
    ```
    {: pre}

    出力例:
    ```
    expander: least-waste
    image:
      pullPolicy: Always
      repository: icr.io/iks-charts/ibm-iks-cluster-autoscaler
      tag: dev1
    maxNodeProvisionTime: 120m
    resources:
      limits:
        cpu: 300m
        memory: 300Mi
      requests:
        cpu: 100m
        memory: 100Mi
    scaleDownDelayAfterAdd: 10m
    scaleDownDelayAfterDelete: 10m
    scaleDownUtilizationThreshold: 0.5
    scaleDownUnneededTime: 10m
    scanInterval: 1m
    skipNodes:
      withLocalStorage: true
      withSystemPods: true
    ```
    {: codeblock}

    <table>
    <caption>クラスター自動スケーリング機能の構成値</caption>
    <thead>
    <th>パラメーター</th>
    <th>説明</th>
    <th>デフォルト値</th>
    </thead>
    <tbody>
    <tr>
    <td>`api_route` パラメーター</td>
    <td>クラスターが存在する地域の [{{site.data.keyword.containerlong_notm}} API エンドポイント](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cli_api)を設定します。</td>
    <td>デフォルトはありません。クラスターが存在するターゲット地域を使用します。</td>
    </tr>
    <tr>
    <td>`expander` パラメーター</td>
    <td>ワーカー・プールが複数ある場合に、クラスター自動スケーリング機能でスケーリングするワーカー・プールを決定する方法を指定します。 使用可能な値を以下に示します。
    <ul><li>`random`: `most-pods` と `least-waste` の間でランダムに選択します。</li>
    <li>`most-pods`: スケールアップ時に最も多くのポッドをスケジュールできるワーカー・プールを選択します。 `nodeSelector` を使用してポッドを特定のワーカー・ノードにデプロイしている場合は、このメソッドを使用します。</li>
    <li>`least-waste`: スケールアップ後に不使用の CPU が最も少ないワーカー・プールを選択します。2 つのワーカー・プールがスケールアップ後に同じ量の CPU リソースを使用する場合は、未使用メモリーが最も少ないワーカー・プールが選択されます。</li></ul></td>
    <td>random</td>
    </tr>
    <tr>
    <td>`image.repository` パラメーター</td>
    <td>使用するクラスター自動スケーリング機能の Docker イメージを指定します。</td>
    <td>`icr.io/iks-charts/ibm-iks-cluster-autoscaler`</td>
    </tr>
    <tr>
    <td>`image.pullPolicy` パラメーター</td>
    <td>Docker イメージをプルするタイミングを指定します。 使用可能な値を以下に示します。
    <ul><li>`Always`: ポッドが開始されるたびにイメージをプルします。</li>
    <li>`IfNotPresent`: イメージがまだローカルに存在しない場合にのみ、イメージをプルします。</li>
    <li>`Never`: イメージはローカルに存在すると想定し、イメージをプルすることはありません。</li></ul></td>
    <td>Always</td>
    </tr>
    <tr>
    <td>`maxNodeProvisionTime` パラメーター</td>
    <td>ワーカー・ノードがプロビジョニングの開始に費やせる最大時間を分単位で設定します。これを過ぎると、クラスター自動スケーリング機能によってスケールアップ要求は取り消されます。</td>
    <td>`120m`</td>
    </tr>
    <tr>
    <td>`resources.limits.cpu` パラメーター</td>
    <td>`ibm-iks-cluster-autoscaler` ポッドが消費できるワーカー・ノードの CPU の最大量を設定します。</td>
    <td>`300m`</td>
    </tr>
    <tr>
    <td>`resources.limits.memory` パラメーター</td>
    <td>`ibm-iks-cluster-autoscaler` ポッドが消費できるワーカー・ノードのメモリーの最大量を設定します。</td>
    <td>`300Mi`</td>
    </tr>
    <tr>
    <td>`resources.requests.cpu` パラメーター</td>
    <td>`ibm-iks-cluster-autoscaler` ポッド開始時のワーカー・ノードの CPU の最小量を設定します。</td>
    <td>`100m`</td>
    </tr>
    <tr>
    <td>`resources.requests.memory` パラメーター</td>
    <td>`ibm-iks-cluster-autoscaler` ポッド開始時のワーカー・ノードのメモリーの最小量を設定します。</td>
    <td>`100Mi`</td>
    </tr>
    <tr>
    <td>`scaleDownUnneededTime` パラメーター</td>
    <td>ワーカー・ノードが不要になってからスケールダウンが開始されるまでの時間を分単位で設定します。</td>
    <td>`10m`</td>
    </tr>
    <tr>
    <td>`scaleDownDelayAfterAdd` パラメーター、`scaleDownDelayAfterDelete` パラメーター</td>
    <td>スケールアップ (`add`) またはスケールダウン (`delete`) 後に、クラスター自動スケーリング機能が次のスケーリング操作を開始するまで待機する時間を分単位で設定します。</td>
    <td>`10m`</td>
    </tr>
    <tr>
    <td>`scaleDownUtilizationThreshold` パラメーター</td>
    <td>ワーカー・ノードの使用率しきい値を設定します。 ワーカー・ノードの使用率がこのしきい値を下回ると、スケールダウン対象のワーカー・ノードと見なされます。 ワーカー・ノードの使用率は、ワーカー・ノードで実行されているすべてのポッドから要求される CPU リソースとメモリー・リソースの合計を、ワーカー・ノードのリソース容量で除算して算出します。</td>
    <td>`0.5`</td>
    </tr>
    <tr>
    <td>`scanInterval` パラメーター</td>
    <td>スケールアップまたはスケールダウンをトリガーするワークロードの使用状況がないか、クラスター自動スケーリング機能がスキャンする間隔を分単位で設定します。</td>
    <td>`1m`</td>
    </tr>
    <tr>
    <td>`skipNodes.withLocalStorage` パラメーター</td>
    <td>`true` に設定すると、ローカル・ストレージにデータを保存しているポッドを持つワーカー・ノードはスケールダウンされません。</td>
    <td>`true`</td>
    </tr>
    <tr>
    <td>`skipNodes.withSystemPods` パラメーター</td>
    <td>`true` に設定すると、`kube-system` ポッドを持つワーカー・ノードはスケールダウンされません。 `kube-system` ポッドがスケールダウンされると予期しない結果が生じる可能性があるため、この値を `false` に設定しないでください。</td>
    <td>`true`</td>
    </tr>
    </tbody>
    </table>
2.  クラスター自動スケーリング機能の構成値を変更するには、新しい値を指定して Helm チャートを更新します。 `--recreate-pods` フラグを含めて、カスタム設定変更を適用するために既存のクラスター自動スケーリング機能ポッドが再作成されるようにします。
    ```
    helm upgrade --set scanInterval=2m ibm-iks-cluster-autoscaler iks-charts/ibm-iks-cluster-autoscaler -i --recreate-pods
    ```
    {: pre}

    チャートをデフォルト値にリセットするには、以下のようにします。
    ```
    helm upgrade --reset-values ibm-iks-cluster-autoscaler iks-charts/ibm-iks-cluster-autoscaler --recreate-pods
    ```
    {: pre}
3.  変更を確認するために、Helm チャートの値をもう一度表示します。
    ```
    helm get values ibm-iks-cluster-autoscaler -a
    ```
    {: pre}


## 自動スケーリングされる特定のワーカー・プールでのみ実行するようにアプリを制限する
{: #ca_limit_pool}

ポッドのデプロイメントを、クラスター自動スケーリング機能によって管理される特定のワーカー・プールのみに制限するには、ラベルと `nodeSelector` または `nodeAffinity` を使用します。`nodeAffinity` を使用すると、ポッドをワーカー・ノードに照合するためにスケジューリング動作がどのように機能するのかをより細かく制御できます。ワーカー・ノードへのポッドの割り当てについて詳しくは、[Kubernetes の資料 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/) を参照してください。
{: shortdesc}

**始める前に**:
*  [`ibm-iks-cluster-autoscaler` プラグイン](#ca_helm)をインストールします。
*  [アカウントにログインします。 該当する場合は、適切なリソース・グループをターゲットにします。 クラスターのコンテキストを設定します。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

**特定の自動スケーリングされるワーカー・プールで実行するようにポッドを制限するには**:

1.  使用するラベルを指定してワーカー・プールを作成します。 例えば、`app: nginx` などのラベルを使用します。
    ```
    ibmcloud ks worker-pool-create --name <name> --cluster <cluster_name_or_ID> --machine-type <machine_type> --size-per-zone <number_of_worker_nodes> --labels <key>=<value>
    ```
    {: pre}
2.  [ワーカー・プールをクラスター自動スケーリング機能の構成に追加](#ca_cm)します。
3.  ポッド仕様テンプレートで、`nodeSelector` または `nodeAffinity` をワーカー・プールで使用したラベルと一致させます。

    `nodeSelector` の例:
    ```
    ...
    spec:
          containers:
      - name: nginx
        image: nginx
        imagePullPolicy: IfNotPresent
      nodeSelector:
        app: nginx
    ...
    ```
    {: codeblock}

    `nodeAffinity` の例:
    ```
    spec:
          containers:
      - name: nginx
        image: nginx
        imagePullPolicy: IfNotPresent
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: app
                  operator: In
                  values:
                - nginx
    ```
    {: codeblock}
4.  ポッドをデプロイします。 ラベルが一致しているため、ポッドはラベルが付いたワーカー・プール内のワーカー・ノードにスケジュールされます。
    ```
    kubectl apply -f pod.yaml
    ```
    {: pre}

<br />


## ワーカー・プールのリソースが不足する前にワーカー・ノードを増やす
{: #ca_scaleup}

[クラスター自動スケーリング機能について](#ca_about)のトピックおよび [Kubernetes クラスターの自動スケーリング機能 FAQ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md) で説明したように、クラスター自動スケーリング機能は、ワークロードの要求されたリソースと、ワーカー・プールの使用可能なリソースを比較した結果に応じて、ワーカー・プールをスケールアップします。 しかし、ワーカー・プールのリソースが不足する前に、クラスター自動スケーリング機能でワーカー・ノードをスケールアップしたい場合があります。 そうすれば、ワーカー・プールは既にリソース要求を満たすようにスケールアップされているので、ワーカー・ノードがプロビジョンされるまでワークロードが待つ必要はありません。
{: shortdesc}

クラスター自動スケーリング機能は、ワーカー・プールの早期スケーリング (オーバープロビジョン) をサポートしません。 ただし、他の Kubernetes リソースをクラスター自動スケーリング機能と連携して早期スケーリングを実行するように構成することができます。

<dl>
  <dt><strong>一時停止ポッド</strong></dt>
  <dd>特定のリソース要求を持つポッドで[一時停止コンテナー ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://stackoverflow.com/questions/48651269/what-are-the-pause-containers) をデプロイするデプロイメントを作成し、そのデプロイメントに低いポッド優先度を割り当てることができます。 それらのリソースが高優先度のワークロードで必要になると、その一時停止ポッドは先行停止 (preempt) され、保留ポッドになります。 このイベントにより、クラスター自動スケーリング機能のスケールアップがトリガーされます。<br><br>一時停止ポッドのデプロイメントのセットアップについて詳しくは、[Kubernetes の FAQ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#how-can-i-configure-overprovisioning-with-cluster-autoscaler) を参照してください。[このサンプルのオーバープロビジョン構成ファイル ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/IBM-Cloud/kube-samples/blob/master/ibm-ks-cluster-autoscaler/overprovisioning-autoscaler.yaml) を使用して、優先度クラス、サービス・アカウント、およびデプロイメントを作成できます。<p class="note">この方法を使用する場合は、[ポッドの優先度](/docs/containers?topic=containers-pod_priority#pod_priority)がどのように機能するか、およびデプロイメントに合わせたポッドの優先度の設定方法について必ず理解してください。 例えば、一時停止ポッドに、優先度の高いポッドに使用できるリソースが十分にない場合、そのポッドは先行停止されません。 優先度の高いワークロードが保留中のままになり、クラスター自動スケーリング機能のスケールアップがトリガーされます。 ただし、この場合は、実行するワークロードはリソース不足のためにスケジュールできないので、スケールアップ・アクションは早期になりません。</p></dd>

  <dt><strong>水平ポッド自動スケーリング (HPA)</strong></dt>
  <dd>水平ポッドの自動スケーリングはポッドの平均 CPU 使用率に基づいて行われるので、設定した CPU 使用限度には、ワーカー・プールがリソースを使い果たす前に達します。 さらにポッドが要求されると、クラスター自動スケーリング機能のワーカー・プールのスケールアップがトリガーされます。<br><br>HPA のセットアップについて詳しくは、[Kubernetes の資料 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale-walkthrough/) を参照してください。</dd>
</dl>

<br />


## クラスター自動スケーリング機能の Helm チャートを更新する
{: #ca_helm_up}

クラスター自動スケーリング機能の既存の Helm チャートを最新バージョンに更新できます。 Helm チャートの現在のバージョンを確認するには、`helm ls | grep cluster-autoscaler` を実行します。
{: shortdesc}

Helm チャートをバージョン 1.0.2 以前から最新バージョンに更新する場合は、 [この手順](#ca_helm_up_102)に従ってください。
{: note}

開始前に、以下のことを行います。 [アカウントにログインします。 該当する場合は、適切なリソース・グループをターゲットにします。 クラスターのコンテキストを設定します。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  Helm リポジトリーを更新して、このリポジトリーにあるすべての Helm チャートの最新バージョンを取得します。
    ```
    helm repo update
    ```
    {: pre}

2.  オプション: 最新の Helm チャートをローカル・マシンにダウンロードします。 そして、パッケージを解凍し、`release.md` ファイルを参照して最新リリース情報を確認します。
    ```
    helm fetch iks-charts/ibm-iks-cluster-autoscaler
    ```
    {: pre}

3.  クラスターにインストールしたクラスター自動スケーリング機能 Helm チャートの名前を調べます。
    ```
    helm ls | grep cluster-autoscaler
    ```
    {: pre}

    出力例:
    ```
    myhelmchart 	1       	Mon Jan  7 14:47:44 2019	DEPLOYED	ibm-ks-cluster-autoscaler-1.0.0  	kube-system
    ```
    {: screen}

4.  クラスター自動スケーリング機能の Helm チャートを最新バージョンに更新します。
    ```
    helm upgrade --force --recreate-pods <helm_chart_name>  iks-charts/ibm-iks-cluster-autoscaler
    ```
    {: pre}

5.  スケールリングするワーカー・プールに対して、[クラスター自動スケーリング機能の構成マップ](#ca_cm) の `workerPoolsConfig.json` セクションが `"enabled": true` に設定されていることを確認します。
    ```
    kubectl describe cm iks-ca-configmap -n kube-system
    ```
    {: pre}

    出力例:
    ```
    Name:         iks-ca-configmap
    Namespace:    kube-system
    Labels:       <none>
    Annotations:  kubectl.kubernetes.io/last-applied-configuration={"apiVersion":"v1","data":{"workerPoolsConfig.json":"[\n {\"name\": \"docs\",\"minSize\": 1,\"maxSize\": 3,\"enabled\":true}\n]\n"},"kind":"ConfigMap",...

    Data
    ====
    workerPoolsConfig.json:
    ----
    [
     {"name": "docs","minSize": 1,"maxSize": 3,"enabled":true}
    ]

    Events:  <none>
    ```
    {: screen}

### Helm チャートをバージョン 1.0.2 以前から最新バージョンに更新する
{: #ca_helm_up_102}

クラスター自動スケーリング機能の Helm チャートを最新バージョンにするには、以前にインストールされたクラスター自動スケーリング機能の Helm チャートのバージョンを完全に削除する必要があります。 バージョン 1.0.2 以前の Helm チャートがインストールされている場合は、クラスター自動スケーリング機能の最新の Helm チャートをインストールする前に、まずそのバージョンをアンインストールしてください。
{: shortdesc}

開始前に、以下のことを行います。 [アカウントにログインします。 該当する場合は、適切なリソース・グループをターゲットにします。 クラスターのコンテキストを設定します。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  クラスター自動スケーリング機能の構成マップを取得します。
    ```
    kubectl get cm iks-ca-configmap -n kube-system -o yaml > iks-ca-configmap.yaml
    ```
    {: pre}
2.  構成マップからすべてのワーカー・プールを削除するため、`"enabled"` の値を `false` に設定します。
    ```
    kubectl edit cm iks-ca-configmap -n kube-system
    ```
    {: pre}
3.  Helm チャートにカスタム設定を適用していた場合は、カスタム設定を書き留めます。
    ```
    helm get values ibm-ks-cluster-autoscaler -a
    ```
    {: pre}
4.  現在の Helm チャートをアンインストールします。
    ```
    helm delete --purge ibm-ks-cluster-autoscaler
    ```
    {: pre}
5.  Helm チャート・リポジトリーを更新して、クラスター自動スケーリング機能 Helm チャートの最新バージョンを取得します。
    ```
    helm repo update
    ```
    {: pre}
6.  クラスター自動スケーリング機能の最新の Helm チャートをインストールします。 前に `--set` フラグで使用していたカスタム設定 (`scanInterval=2m` など) を適用します。
    ```
    helm install  iks-charts/ibm-iks-cluster-autoscaler --namespace kube-system --name ibm-iks-cluster-autoscaler [--set <custom_settings>]
    ```
    {: pre}
7.  以前に取得したクラスター自動スケーリング機能の構成マップを適用して、ワーカー・プールの自動スケーリングを有効にします。
    ```
    kubectl apply -f iks-ca-configmap.yaml
    ```
    {: pre}
8.  クラスター自動スケーリング機能のポッドを表示します。
    ```
    kubectl get pods -n kube-system
    ```
    {: pre}
9.  クラスター自動スケーリング機能ポッドの**`「Events」`**セクションで **`ConfigUpdated`** イベントを探し、構成マップが正常に更新されたことを確認します。 構成マップのイベント・メッセージが、`minSize:maxSize:PoolName:<SUCCESS|FAILED>:error message` という形式で記録されています。
    ```
    kubectl describe pod -n kube-system <cluster_autoscaler_pod>
    ```
    {: pre}

    出力例:
    ```
		Name:               ibm-iks-cluster-autoscaler-857c4d9d54-gwvc6
		Namespace:          kube-system
		...
		Events:
    Type    Reason                 Age   From                     Message
		----     ------         ----  ----                                        -------

		Normal  ConfigUpdated  3m    ibm-iks-cluster-autoscaler-857c4d9d54-gwvc6  {"1:3:default":"SUCCESS:"}
    ```
    {: screen}

<br />


## クラスター自動スケーリング機能を削除する
{: #ca_rm}

ワーカー・プールを自動的にスケーリングしないようにするには、クラスター自動スケーリング機能の Helm チャートをアンインストールします。 削除した後は、手動でワーカー・プールの[サイズを変更](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_resize)したり、[再バランス化](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_rebalance)したりする必要があります。
{: shortdesc}

開始前に、以下のことを行います。 [アカウントにログインします。 該当する場合は、適切なリソース・グループをターゲットにします。 クラスターのコンテキストを設定します。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  [クラスター自動スケーリング機能の構成マップ](#ca_cm)で、`"enabled"` 値を `false` に設定してワーカー・プールを削除します。
    ```
    kubectl edit cm iks-ca-configmap -n kube-system
    ```
    {: pre}
    出力例:
    ```
    apiVersion: v1
    data:
      workerPoolsConfig.json: |
        [
         {"name": "default","minSize": 1,"maxSize": 2,"enabled":false},
         {"name":"mypool","minSize":1,"maxSize":5,"enabled":false}
        ]
    kind: ConfigMap
    ...
    ```
2.  既存の Helm チャートをリストし、クラスター自動スケーリング機能の名前をメモします。
    ```
    helm ls
    ```
    {: pre}
3.  クラスターから既存の Helm チャートを削除します。
    ```
    helm delete --purge <ibm-iks-cluster-autoscaler_name>
    ```
    {: pre}
