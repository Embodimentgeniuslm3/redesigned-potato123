---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-11"

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


# ネットワーク・トラフィックをエッジ・ワーカー・ノードに制限する
{: #edge}

エッジ・ワーカー・ノードを使用すると、外部からアクセスされるワーカー・ノードの数を減らし、{{site.data.keyword.containerlong}} のネットワーキングのワークロードを分離することができるので、Kubernetes クラスターのセキュリティーが改善されます。
{:shortdesc}

これらのワーカー・ノードがネットワーキング専用としてマーク付けされると、他のワークロードはワーカー・ノードの CPU やメモリーを消費してネットワーキングに干渉することがなくなります。

複数ゾーン・クラスターがあり、エッジ・ワーカー・ノードへのネットワーク・トラフィックを制限する場合は、ロード・バランサーまたは Ingress ポッドの高可用性のために各ゾーンで少なくとも 2 つのエッジ・ワーカー・ノードを使用可能にする必要があります。 クラスター内のすべてのゾーンにかかるエッジ・ノード・ワーカー・プールを作成し、ゾーンごとに少なくとも 2 つのワーカー・ノードがあるようにします。
{: tip}

## ワーカー・ノードにエッジ・ノードとしてラベル付けする
{: #edge_nodes}

クラスター内のそれぞれのパブリック VLAN またはプライベート VLAN 上の複数のワーカー・ノードに `dedicated=edge` ラベルを追加することで、ネットワーク・ロード・バランサー (NLB) と Ingress アプリケーション・ロード・バランサー (ALB) がそれらのワーカー・ノードのみにデプロイされるようにします。
{:shortdesc}

Kubernetes 1.14 以降では、パブリックとプライベートの両方の NLB と ALB をエッジ・ワーカー・ノードにデプロイできます。Kubernetes 1.13 以前では、パブリックとプライベートの ALB とパブリック NLB をエッジ・ノードにデプロイできますが、プライベート NLB はクラスター内の非エッジ・ワーカー・ノードのみにデプロイする必要があります。
{: note}

開始前に、以下のことを行います。

* 以下の [{{site.data.keyword.Bluemix_notm}} IAM 役割](/docs/containers?topic=containers-users#platform)があることを確認します。
  * クラスターに対するプラットフォームの役割
  * すべての名前空間に対する**ライター**または**管理者**のサービス役割
* [アカウントにログインします。 該当する場合は、適切なリソース・グループをターゲットにします。 クラスターのコンテキストを設定します。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

</br>ワーカー・ノードにエッジ・ノードとしてラベル付けするには、以下のようにします。

1. クラスター内のすべてのゾーンにかかる[新しいワーカー・プールを作成](/docs/containers?topic=containers-add_workers#add_pool)し、ゾーンごとに少なく少なくとも 2 つのワーカーがあるようにします。 `ibmcloud ks worker-pool-create` コマンドで、`--labels dedicated=edge` フラグを指定してプール内のすべてのワーカー・ノードにラベル付けします。このプール内のすべてのワーカー・ノード (後で追加するワーカー・ノードを含む) は、エッジ・ノードとしてラベル付けされます。
  <p class="tip">既存のワーカー・プールを使用する場合、そのプールはクラスター内のすべてのゾーンにまたがっている必要があり、ゾーンあたり少なくとも 2 つのワーカーを含んでいる必要があります。このワーカー・プールに `dedicated=edge` というラベルを付けるには、[PATCH ワーカー・プール API](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/PatchWorkerPool) を使用します。要求の本体で、次の JSON を渡します。このワーカー・プールに `dedicated=edge` というマークが付けられた後は、すべての既存および後続のワーカー・ノードにはこのラベルが付けられて、Ingress とロード・バランサーはエッジ・ワーカー・ノードにデプロイされます。
<pre class="screen">
      {
        "labels": {"dedicated":"edge"},
        "state": "labels"
      }</pre></p>

2. 上記のワーカー・プールとワーカー・ノードに `dedicated=edge` というラベルが付けられていることを確認します。
  * ワーカー・プールを確認するには、以下のようにします。
    ```
    ibmcloud ks worker-pool-get --cluster <cluster_name_or_ID> --worker-pool <worker_pool_name_or_ID>
    ```
    {: pre}

  * 個々のワーカー・ノードを確認するには、以下のコマンドの出力の **Labels** フィールドを確認します。
    ```
    kubectl describe node <worker_node_private_IP>
    ```
    {: pre}

3. クラスター内のすべての既存の NLB と ALB を取得します。
  ```
  kubectl get services --all-namespaces | grep LoadBalancer
  ```
  {: pre}

  この出力内で、各ロード・バランサー・サービスの **Namespace** と **Name** をメモします。例えば、以下の出力には 4 つのロード・バランサー・サービスがあります。その内訳は、`default` 名前空間内の 1 つのパブリック NLB と、`kube-system` 名前空間内の 1 つのプライベート ALB および 2 つのパブリック ALB です。
  ```
  NAMESPACE     NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP     PORT(S)                                     AGE
  default       webserver-lb                                     LoadBalancer   172.21.190.18    169.46.17.2     80:30597/TCP                                10m
  kube-system   private-crdf253b6025d64944ab99ed63bb4567b6-alb1  LoadBalancer   172.21.158.78    10.185.94.150   80:31015/TCP,443:31401/TCP,9443:32352/TCP   25d
  kube-system   public-crdf253b6025d64944ab99ed63bb4567b6-alb1   LoadBalancer   172.21.84.248    169.48.228.78   80:30286/TCP,443:31363/TCP                  1h
  kube-system   public-crdf253b6025d64944ab99ed63bb4567b6-alb2   LoadBalancer   172.21.229.73    169.46.17.6     80:31104/TCP,443:31138/TCP                  57m
  ```
  {: screen}

4. 前のステップで得られた出力を使用して、それぞれの NLB と ALB に対して次のコマンドを実行します。このコマンドは、NLB または ALB をエッジ・ワーカー・ノードに再デプロイします。

  クラスターで Kubernetes 1.14 以降を実行している場合は、パブリックとプライベートの両方の NLB と ALB をエッジ・ワーカー・ノードにデプロイできます。Kubernetes 1.13 以前では、パブリックとプライベートの ALB とパブリック NLB のみをエッジ・ノードにデプロイできるため、プライベート NLB サービスを再デプロイしないでください。
  {: note}

  ```
  kubectl get service -n <namespace> <service_name> -o yaml | kubectl apply -f -
  ```
  {: pre}

  出力例:
  ```
  service "webserver-lb" configured
  ```
  {: screen}

5. ネットワーキング・ワークロードがエッジ・ノードに制限されていることを確認するには、NLB ポッドと ALB ポッドがエッジ・ノードにスケジュールされ、非エッジ・ノードにはスケジュールされていないことを確認します。

  * NLB ポッド:
    1. NLB ポッドがエッジ・ノードにデプロイされていることを確認します。ステップ 3 の出力にリストされているロード・バランサー・サービスの外部 IP アドレスを検索します。ピリオド (`.`) をハイフン (`-`) に置き換えます。`169.46.17.2` という外部 IP アドレスを持つ `webserver-lb` という NLB の例:
      ```
      kubectl describe nodes -l dedicated=edge | grep "169-46-17-2"
      ```
      {: pre}

      出力例:
      ```
      ibm-system                 ibm-cloud-provider-ip-169-46-17-2-76fcb4965d-wz6dg                 5m (0%)       0 (0%)      10Mi (0%)        0 (0%)
      ibm-system                 ibm-cloud-provider-ip-169-46-17-2-76fcb4965d-2z64r                 5m (0%)       0 (0%)      10Mi (0%)        0 (0%)
      ```
      {: screen}
    2. どの NLB ポッドも非エッジ・ノードにデプロイされていないことを確認します。`169.46.17.2` という外部 IP アドレスを持つ `webserver-lb` という NLB の例:
      ```
      kubectl describe nodes -l dedicated!=edge | grep "169-46-17-2"
      ```
      {: pre}
      * NLB ポッドがエッジ・ノードに正しくデプロイされている場合は、NLB ポッドは返されません。NLB は、エッジ・ワーカー・ノードのみに正常にスケジュール変更されています。
      * NLB ポッドが返された場合は、次のステップに進みます。

  * ALB ポッド:
    1. すべての ALB ポッドがエッジ・ノードにデプロイされていることを確認します。`alb` というキーワードを検索します。それぞれのパブリック ALB とプライベート ALB には 2 つのポッドがあります。例:
      ```
      kubectl describe nodes -l dedicated=edge | grep alb
      ```
      {: pre}

      1 つのデフォルト・プライベート ALB と 2 つのデフォルト・パブリック ALB が有効になっている 2 つのゾーンを持つクラスターの出力例:
      ```
      kube-system                private-crdf253b6025d64944ab99ed63bb4567b6-alb1-d5dd478db-27pv4    0 (0%)        0 (0%)      0 (0%)           0 (0%)
      kube-system                private-crdf253b6025d64944ab99ed63bb4567b6-alb1-d5dd478db-7p9q6    0 (0%)        0 (0%)      0 (0%)           0 (0%)
      kube-system                public-crdf253b6025d64944ab99ed63bb4567b6-alb1-5ff8cdff89-s77z6    0 (0%)        0 (0%)      0 (0%)           0 (0%)
      kube-system                public-crdf253b6025d64944ab99ed63bb4567b6-alb1-5ff8cdff89-kvs9f    0 (0%)        0 (0%)      0 (0%)           0 (0%)
      kube-system                public-crdf253b6025d64944ab99ed63bb4567b6-alb2-57df7c7b5b-tp6xw    0 (0%)        0 (0%)      0 (0%)           0 (0%)
      kube-system                public-crdf253b6025d64944ab99ed63bb4567b6-alb2-57df7c7b5b-z5p2v    0 (0%)        0 (0%)      0 (0%)           0 (0%)
      ```
      {: screen}

    2. どの ALB ポッドも非エッジ・ノードにデプロイされていないことを確認します。例:
      ```
      kubectl describe nodes -l dedicated!=edge | grep alb
      ```
      {: pre}
      * ALB ポッドがエッジ・ノードに正しくデプロイされている場合は、ALB ポッドは返されません。ALB は、エッジ・ワーカー・ノードのみに正常にスケジュール変更されています。
      * ALB ポッドが返された場合は、次のステップに進みます。

6. NLB ポッドまたは ALB ポッドがまだ非エッジ・ノードにデプロイされている場合は、それらのポッドを削除してエッジ・ノードに再デプロイすることができます。**重要**: 一度に 1 つのポッドのみを削除して、そのポッドがエッジ・ノードにスケジュール変更されていることを確認してから、他のポッドを削除してください。
  1. ポッドを削除します。`webserver-lb` という NLB ポッドの 1 つがエッジ・ノードにスケジュールされなかった場合の例:
    ```
    kubectl delete pod ibm-cloud-provider-ip-169-46-17-2-76fcb4965d-wz6dg
    ```
    {: pre}

  2. このポッドがエッジ・ワーカー・ノードにスケジュール変更されていることを確認します。スケジュール変更は自動的に行われますが、数分かかることがあります。`169.46.17.2` という外部 IP アドレスを持つ `webserver-lb` という NLB の例:
      ```
    kubectl describe nodes -l dedicated=edge | grep "169-46-17-2"
    ```
    {: pre}

    出力例:
    ```
    ibm-system                 ibm-cloud-provider-ip-169-46-17-2-76fcb4965d-wz6dg                 5m (0%)       0 (0%)      10Mi (0%)        0 (0%)
      ibm-system                 ibm-cloud-provider-ip-169-46-17-2-76fcb4965d-2z64r                 5m (0%)       0 (0%)      10Mi (0%)        0 (0%)
      ```
    {: screen}

</br>上記では、ワーカー・プール内のワーカー・ノードに `dedicated=edge` というラベルを付けて、すべての既存の ALB と NLB をエッジ・ノードに再デプロイしました。これ以降にクラスターに追加されるすべての ALB と NLB も、エッジ・ワーカー・プール内のエッジ・ノードにデプロイされます。次に、他の[ワークロードがエッジ・ワーカー・ノード上で実行されないようにして](#edge_workloads)、[ワーカー・ノード上の NodePort へのインバウンド・トラフィックをブロックします](/docs/containers?topic=containers-network_policies#block_ingress)。

<br />


## ワークロードがエッジ・ワーカー・ノード上で実行されないようにする
{: #edge_workloads}

エッジ・ワーカー・ノードの利点の 1 つは、それらがネットワーク・サービスだけを実行するように指定できることです。
{:shortdesc}

`dedicated=edge` 耐障害性を使用すると、すべてのネットワーク・ロード・バランサー (NLB) と Ingress アプリケーション・ロード・バランサー (ALB) のサービスが、このラベルが付けられたワーカー・ノードのみにデプロイされます。ただし、他のワークロードがエッジ・ワーカー・ノード上で実行されてワーカー・ノードのリソースを消費することがないようにするため、[Kubernetes テイント ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/) を使用する必要があります。

開始前に、以下のことを行います。
- すべての名前空間に対する[**管理者**の {{site.data.keyword.Bluemix_notm}} IAM サービス役割](/docs/containers?topic=containers-users#platform)があることを確認します。
- [アカウントにログインします。 該当する場合は、適切なリソース・グループをターゲットにします。 クラスターのコンテキストを設定します。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

</br>他のワークロードがエッジ・ワーカー・ノード上で実行されないようにするには、以下のようにします。

1. `dedicated=edge` ラベルが付けられたすべてのワーカー・ノードにテイントを適用します。このテイントは、それらのワーカー・ノード上でポッドが実行されることを防止して、`dedicated=edge` ラベルが付けられていないポッドをそれらのワーカー・ノードから削除するものです。削除されたポッドは、容量のある他のワーカー・ノードに再デプロイされます。
  ```
  kubectl taint node -l dedicated=edge dedicated=edge:NoSchedule dedicated=edge:NoExecute
  ```
  {: pre}
  これで、`dedicated=edge` 耐障害性のあるポッドだけがエッジ・ワーカー・ノードにデプロイされます。

2. エッジ・ノードにテイントが適用されていることを確認します。
  ```
  kubectl describe nodes -l dedicated=edge | grep "Taint|Hostname"
  ```
  {: pre}

  出力例:
  ```
  Taints:             dedicated=edge:NoExecute
    Hostname:    10.176.48.83
  Taints:             dedicated=edge:NoExecute
    Hostname:    10.184.58.7
  ```
  {: screen}

3. [NLB 1.0 サービスのソース IP 保持を有効にする](/docs/containers?topic=containers-loadbalancer#node_affinity_tolerations)場合は、[アプリ・ポッドにエッジ・ノード・アフィニティーを追加](/docs/containers?topic=containers-loadbalancer#lb_edge_nodes)することで、アプリ・ポッドが確実にエッジ・ワーカー・ノードにスケジュールされるようにしてください。着信要求を受信するには、アプリ・ポッドをエッジ・ノードにスケジュールする必要があります。

4. テイントを削除するには、以下のコマンドを実行します。
    ```
    kubectl taint node <node_name> dedicated:NoSchedule- dedicated:NoExecute-
    ```
    {: pre}
