---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-31"

keywords: kubernetes, iks, audit

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



# {{site.data.keyword.cloudaccesstrailshort}} イベント
{: #at_events}

{{site.data.keyword.cloudaccesstrailshort}} サービスを使用して、{{site.data.keyword.containerlong_notm}} クラスター内でユーザーによって開始されたアクティビティーを表示、管理、および監査することができます。
{: shortdesc}

{{site.data.keyword.containershort_notm}} は、以下の 2 つのタイプの {{site.data.keyword.cloudaccesstrailshort}} イベントを生成します。

* **クラスター管理イベント**:
    * これらのイベントは自動的に生成され、{{site.data.keyword.cloudaccesstrailshort}} に転送されます。
    * これらのイベントは {{site.data.keyword.cloudaccesstrailshort}} **アカウント・ドメイン**を通して表示できます。

* **Kubernetes API サーバー監査イベント**:
    * これらのイベントは自動的に生成されますが、これらのイベントを {{site.data.keyword.cloudaccesstrailshort}} サービスに転送するようにクラスターを構成する必要があります。
    * イベントを {{site.data.keyword.cloudaccesstrailshort}} **アカウント・ドメイン**に送信するように、または、**スペース・ドメイン**に送信するように、クラスターを構成できます。 詳しくは、[監査ログの送信](/docs/containers?topic=containers-health#api_forward)を参照してください。

サービスの仕組みについて詳しくは、[{{site.data.keyword.cloudaccesstrailshort}} の資料](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started)を参照してください。 追跡される Kubernetes アクションについて詳しくは、[Kubernetes の資料 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/home/) を参照してください。

{{site.data.keyword.containerlong_notm}} は現在 {{site.data.keyword.at_full}} を使用するように構成されていません。クラスター管理イベントと Kubernetes API 監査ログを管理するには、引き続き {{site.data.keyword.cloudaccesstrailfull_notm}} を LogAnalysis と組み合わせて使用します。
{: note}

## イベントの情報の検出
{: #kube-find}

Kibana ダッシュボードでログを参照して、クラスター内のアクティビティーをモニターできます。
{: shortdesc}

管理アクティビティーをモニターするには、次の手順を実行します。

1. {{site.data.keyword.Bluemix_notm}} アカウントにログインします。
2. カタログから、{{site.data.keyword.containerlong_notm}} のインスタンスと同じアカウントで、{{site.data.keyword.cloudaccesstrailshort}} サービスのインスタンスをプロビジョンします。
3. {{site.data.keyword.cloudaccesstrailshort}} ダッシュボードの**「管理」**タブで、アカウント・ドメインまたはスペース・ドメインを選択します。
  * **アカウント・ログ**: クラスター管理イベントと Kubernetes API サーバー監査イベントは、イベントが生成される {{site.data.keyword.Bluemix_notm}} 地域の**アカウント・ドメイン**で参照できます。
  * **スペース・ログ**: Kubernetes API サーバー監査イベントを転送するようにクラスターを構成するときにスペースを指定した場合、これらのイベントは、{{site.data.keyword.cloudaccesstrailshort}} サービスがプロビジョンされている Cloud Foundry スペースに関連付けられた**スペース・ドメイン**で参照できます。
4. **「Kibana で表示」**をクリックします。
5. ログを表示する時間フレームを設定します。 デフォルトは 24 時間です。
6. 検索を絞り込むには、`ActivityTracker_Account_Search_in_24h` の編集アイコンをクリックして、**「使用可能なフィールド」**列にフィールドを追加します。

他のユーザーがアカウントとスペースのイベントを表示できるようにするには、[Granting permissions to see account events](/docs/services/cloud-activity-tracker/how-to?topic=cloud-activity-tracker-grant_permissions#grant_permissions) を参照してください。
{: tip}

## クラスター管理イベントの追跡
{: #cluster-events}

{{site.data.keyword.cloudaccesstrailshort}} に送信されるクラスター管理イベントを示す以下のリストを確認してください。
{: shortdesc}

<table>
<tr>
<th>アクション</th>
<th>説明</th></tr><tr>
<td><code>containers-kubernetes.account-credentials.set</code></td>
<td>リソース・グループの地域のインフラストラクチャー資格情報が設定されます。</td></tr><tr>
<td><code>containers-kubernetes.account-credentials.unset</code></td>
<td>リソース・グループの地域のインフラストラクチャー資格情報が設定解除されます。</td></tr><tr>
<td><code>containers-kubernetes.alb.create</code></td>
<td>Ingress ALB が作成されます。</td></tr><tr>
<td><code>containers-kubernetes.alb.delete</code></td>
<td>Ingress ALB が削除されます。</td></tr><tr>
<td><code>containers-kubernetes.alb.get</code></td>
<td>Ingress ALB 情報が表示されます。</td></tr><tr>
<td><code>containers-kubernetes.apikey.reset</code></td>
<td>地域およびリソース・グループの API キーが再設定されます。</td></tr><tr>
<td><code>containers-kubernetes.cluster.create</code></td>
<td>クラスターが作成されます。</td></tr><tr>
<td><code>containers-kubernetes.cluster.delete</code></td>
<td>クラスターが削除されます。</td></tr><tr>
<td><code>containers-kubernetes.cluster-feature.enable</code></td>
<td>ベアメタル・ワーカー・ノードのトラステッド・コンピューティングなどの機能がクラスターで有効になります。</td></tr><tr>
<td><code>containers-kubernetes.cluster.get</code></td>
<td>クラスター情報が表示されます。</td></tr><tr>
<td><code>containers-kubernetes.logging-config.create</code></td>
<td>ログ転送構成が作成されます。</td></tr><tr>
<td><code>containers-kubernetes.logging-config.delete</code></td>
<td>ログ転送構成が削除されます。</td></tr><tr>
<td><code>containers-kubernetes.logging-config.get</code></td>
<td>ログ転送構成の情報が表示されます。</td></tr><tr>
<td><code>containers-kubernetes.logging-config.update</code></td>
<td>ログ転送構成が更新されます。</td></tr><tr>
<td><code>containers-kubernetes.logging-config.refresh</code></td>
<td>ログ転送構成がリフレッシュされます。</td></tr><tr>
<td><code>containers-kubernetes.logging-filter.create</code></td>
<td>ロギング・フィルターが作成されます。</td></tr><tr>
<td><code>containers-kubernetes.logging-filter.delete</code></td>
<td>ロギング・フィルターが削除されます。</td></tr><tr>
<td><code>containers-kubernetes.logging-filter.get</code></td>
<td>ロギング・フィルターの情報が表示されます。</td></tr><tr>
<td><code>containers-kubernetes.logging-filter.update</code></td>
<td>ロギング・フィルターが更新されます。</td></tr><tr>
<td><code>containers-kubernetes.logging-autoupdate.changed</code></td>
<td>ロギング・アドオン自動アップデーターが有効または無効になります。</td></tr><tr>
<td><code>containers-kubernetes.mzlb.create</code></td>
<td>複数ゾーン・ロード・バランサーが作成されます。</td></tr><tr>
<td><code>containers-kubernetes.mzlb.delete</code></td>
<td>複数ゾーン・ロード・バランサーが削除されます。</td></tr><tr>
<td><code>containers-kubernetes.service.bind</code></td>
<td>サービスがクラスターにバインドされます。</td></tr><tr>
<td><code>containers-kubernetes.service.unbind</code></td>
<td>サービスがクラスターからアンバインドされます。</td></tr><tr>
<td><code>containers-kubernetes.subnet.add</code></td>
<td>既存の IBM Cloud インフラストラクチャー (SoftLayer) サブネットがクラスターに追加されます。</td></tr><tr>
<td><code>containers-kubernetes.subnet.create</code></td>
<td>サブネットが作成されます。</td></tr><tr>
<td><code>containers-kubernetes.usersubnet.add</code></td>
<td>ユーザー管理サブネットがクラスターに追加されます。</td></tr><tr>
<td><code>containers-kubernetes.usersubnet.delete</code></td>
<td>ユーザー管理サブネットがクラスターから削除されます。</td></tr><tr>
<td><code>containers-kubernetes.version.update</code></td>
<td>クラスター・マスター・ノードの Kubernetes のバージョンが更新されます。</td></tr><tr>
<td><code>containers-kubernetes.worker.create</code></td>
<td>ワーカー・ノードが作成されます。</td></tr><tr>
<td><code>containers-kubernetes.worker.delete</code></td>
<td>ワーカー・ノードが削除されます。</td></tr><tr>
<td><code>containers-kubernetes.worker.get</code></td>
<td>ワーカー・ノードの情報が表示されます。</td></tr><tr>
<td><code>containers-kubernetes.worker.reboot</code></td>
<td>ワーカー・ノードがリブートされます。</td></tr><tr>
<td><code>containers-kubernetes.worker.reload</code></td>
<td>ワーカー・ノードが再ロードされます。</td></tr><tr>
<td><code>containers-kubernetes.worker.update</code></td>
<td>ワーカー・ノードが更新されます。</td></tr>
</table>

## Kubernetes 監査イベントの追跡
{: #kube-events}

{{site.data.keyword.cloudaccesstrailshort}} に送信される Kubernetes API サーバー監査イベントのリストについては、以下の表を確認してください。
{: shortdesc}

始める前に: [Kubernetes API 監査イベント](/docs/containers?topic=containers-health#api_forward)を転送するようにクラスターを構成しておいてください。

<table>
    <th>アクション</th>
    <th>説明</th><tr>
    <td><code>bindings.create</code></td>
    <td>バインディングが作成されます。</td></tr><tr>
    <td><code>certificatesigningrequests.create</code></td>
    <td>証明書署名要求が作成されます。</td></tr><tr>
    <td><code>certificatesigningrequests.delete</code></td>
    <td>証明書署名要求が削除されます。</td></tr><tr>
    <td><code>certificatesigningrequests.patch</code></td>
    <td>証明書署名要求にパッチが適用されます。</td></tr><tr>
    <td><code>certificatesigningrequests.update</code></td>
    <td>証明書署名要求が更新されます。</td></tr><tr>
    <td><code>clusterbindings.create</code></td>
    <td>クラスター役割バインディングが作成されます。</td></tr><tr>
    <td><code>clusterbindings.deleted</code></td>
    <td>クラスター役割バインディングが削除されます。</td></tr><tr>
    <td><code>clusterbindings.patched</code></td>
    <td>クラスター役割バインディングにパッチが適用されます。</td></tr><tr>
    <td><code>clusterbindings.updated</code></td>
    <td>クラスター役割バインディングが更新されます。</td></tr><tr>
    <td><code>clusterroles.create</code></td>
    <td>クラスター役割が作成されます。</td></tr><tr>
    <td><code>clusterroles.deleted</code></td>
    <td>クラスター役割が削除されます。</td></tr><tr>
    <td><code>clusterroles.patched</code></td>
    <td>クラスター役割にパッチが適用されます。</td></tr><tr>
    <td><code>clusterroles.updated</code></td>
    <td>クラスター役割が更新されます。</td></tr><tr>
    <td><code>configmaps.create</code></td>
    <td>構成マップが作成されます。</td></tr><tr>
    <td><code>configmaps.delete</code></td>
    <td>構成マップは削除されます。</td></tr><tr>
    <td><code>configmaps.patch</code></td>
    <td>構成マップにパッチが適用されます。</td></tr><tr>
    <td><code>configmaps.update</code></td>
    <td>構成マップは更新されます。</td></tr><tr>
    <td><code>controllerrevisions.create</code></td>
    <td>コントローラー・リビジョンが作成されます。</td></tr><tr>
    <td><code>controllerrevisions.delete</code></td>
    <td>コントローラー・リビジョンが削除されます。</td></tr><tr>
    <td><code>controllerrevisions.patch</code></td>
    <td>コントローラー・リビジョンにパッチが適用されます。</td></tr><tr>
    <td><code>controllerrevisions.update</code></td>
    <td>コントローラー・リビジョンが更新されます。</td></tr><tr>
    <td><code>daemonsets.create</code></td>
    <td>デーモン・セットが作成されます。</td></tr><tr>
    <td><code>daemonsets.delete</code></td>
    <td>デーモン・セットが削除されます。</td></tr><tr>
    <td><code>daemonsets.patch</code></td>
    <td>デーモン・セットにパッチが適用されます。</td></tr><tr>
    <td><code>daemonsets.update</code></td>
    <td>デーモン・セットが更新されます。</td></tr><tr>
    <td><code>deployments.create</code></td>
    <td>デプロイメントが作成されます。</td></tr><tr>
    <td><code>deployments.delete</code></td>
    <td>デプロイメントが削除されます。</td></tr><tr>
    <td><code>deployments.patch</code></td>
    <td>デプロイメントにパッチが適用されます。</td></tr><tr>
    <td><code>deployments.update</code></td>
    <td>デプロイメントに更新されます。</td></tr><tr>
    <td><code>events.create</code></td>
    <td>イベントが作成されます。</td></tr><tr>
    <td><code>events.delete</code></td>
    <td>イベントが削除されます。</td></tr><tr>
    <td><code>events.patch</code></td>
    <td>イベントにパッチが適用されます。</td></tr><tr>
    <td><code>events.update</code></td>
    <td>イベントが更新されます。</td></tr><tr>
    <td><code>externaladmissionhookconfigurations.create</code></td>
    <td>Kubernetes v1.8 で、外部アドミッション・フック構成が作成されます。</td></tr><tr>
    <td><code>externaladmissionhookconfigurations.delete</code></td>
    <td>Kubernetes v1.8 で、外部アドミッション・フック構成が削除されます。</td></tr><tr>
    <td><code>externaladmissionhookconfigurations.patch</code></td>
    <td>Kubernetes v1.8 で、外部アドミッション・フック構成にパッチが適用されます。</td></tr><tr>
    <td><code>externaladmissionhookconfigurations.update</code></td>
    <td>Kubernetes v1.8 で、外部アドミッション・フック構成が更新されます。</td></tr><tr>
    <td><code>horizontalpodautoscalers.create</code></td>
    <td>水平ポッド自動スケーリング・ポリシーが作成されます。</td></tr><tr>
    <td><code>horizontalpodautoscalers.delete</code></td>
    <td>水平ポッド自動スケーリング・ポリシーが削除されます。</td></tr><tr>
    <td><code>horizontalpodautoscalers.patch</code></td>
    <td>水平ポッド自動スケーリング・ポリシーにパッチが適用されます。</td></tr><tr>
    <td><code>horizontalpodautoscalers.update</code></td>
    <td>水平ポッド自動スケーリング・ポリシーが更新されます。</td></tr><tr>
    <td><code>ingresses.create</code></td>
    <td>Ingress ALB が作成されます。</td></tr><tr>
    <td><code>ingresses.delete</code></td>
    <td>Ingress ALB が削除されます。</td></tr><tr>
    <td><code>ingresses.patch</code></td>
    <td>Ingress ALB にパッチが適用されます。</td></tr><tr>
    <td><code>ingresses.update</code></td>
    <td>Ingress ALB が更新されます。</td></tr><tr>
    <td><code>jobs.create</code></td>
    <td>ジョブが作成されます。</td></tr><tr>
    <td><code>jobs.delete</code></td>
    <td>ジョブが削除されます。</td></tr><tr>
    <td><code>jobs.patch</code></td>
    <td>ジョブにパッチが適用されます。</td></tr><tr>
    <td><code>jobs.update</code></td>
    <td>ジョブが更新されます。</td></tr><tr>
    <td><code>localsubjectaccessreviews.create</code></td>
    <td>ローカル件名アクセス・レビューが作成されます。</td></tr><tr>
    <td><code>limitranges.create</code></td>
    <td>範囲制限が作成されます。</td></tr><tr>
    <td><code>limitranges.delete</code></td>
    <td>範囲制限が削除されます。</td></tr><tr>
    <td><code>limitranges.patch</code></td>
    <td>範囲制限にパッチが適用されます。</td></tr><tr>
    <td><code>limitranges.update</code></td>
    <td>範囲制限が更新されます。</td></tr><tr>
    <td><code>mutatingwebhookconfigurations.create</code></td>
    <td>変更された Webhook 構成が作成されます。</td></tr><tr>
    <td><code>mutatingwebhookconfigurations.delete</code></td>
    <td>変更された Webhook 構成が削除されます。</td></tr><tr>
    <td><code>mutatingwebhookconfigurations.patch</code></td>
    <td>変更された Webhook 構成にパッチが適用されます。</td></tr><tr>
    <td><code>mutatingwebhookconfigurations.update</code></td>
    <td>変更された Webhook 構成が更新されます。</td></tr><tr>
    <td><code>namespaces.create</code></td>
    <td>名前空間が作成されます。</td></tr><tr>
    <td><code>namespaces.delete</code></td>
    <td>名前空間が削除されます。</td></tr><tr>
    <td><code>namespaces.patch</code></td>
    <td>名前空間にパッチが適用されます。</td></tr><tr>
    <td><code>namespaces.update</code></td>
    <td>名前空間が更新されます。</td></tr><tr>
    <td><code>networkpolicies.create</code></td>
    <td>ネットワーク・ポリシーが作成されます。</td></tr><tr><tr>
    <td><code>networkpolicies.delete</code></td>
    <td>ネットワーク・ポリシーが削除されます。</td></tr><tr>
    <td><code>networkpolicies.patch</code></td>
    <td>ネットワーク・ポリシーにパッチが適用されます。</td></tr><tr>
    <td><code>networkpolicies.update</code></td>
    <td>ネットワーク・ポリシーが更新されます。</td></tr><tr>
    <td><code>nodes.create</code></td>
    <td>ノードが作成されます。</td></tr><tr>
    <td><code>nodes.delete</code></td>
    <td>ノードが削除されます。</td></tr><tr>
    <td><code>nodes.patch</code></td>
    <td>ノードにパッチが適用されます。</td></tr><tr>
    <td><code>nodes.update</code></td>
    <td>ノードが更新されます。</td></tr><tr>
    <td><code>persistentvolumeclaims.create</code></td>
    <td>永続ボリューム請求が作成されます。</td></tr><tr>
    <td><code>persistentvolumeclaims.delete</code></td>
    <td>永続ボリューム請求が削除されます。</td></tr><tr>
    <td><code>persistentvolumeclaims.patch</code></td>
    <td>永続ボリューム請求にパッチが適用されます。</td></tr><tr>
    <td><code>persistentvolumeclaims.update</code></td>
    <td>永続ボリューム請求が更新されます。</td></tr><tr>
    <td><code>persistentvolumes.create</code></td>
    <td>永続ボリュームが作成されます。</td></tr><tr>
    <td><code>persistentvolumes.delete</code></td>
    <td>永続ボリュームが削除されます。</td></tr><tr>
    <td><code>persistentvolumes.patch</code></td>
    <td>永続ボリュームにパッチが適用されます。</td></tr><tr>
    <td><code>persistentvolumes.update</code></td>
    <td>永続ボリュームが更新されます。</td></tr><tr>
    <td><code>poddisruptionbudgets.create</code></td>
    <td>ポッド中断予算が作成されます。</td></tr><tr>
    <td><code>poddisruptionbudgets.delete</code></td>
    <td>ポッド中断予算が削除されます。</td></tr><tr>
    <td><code>poddisruptionbudgets.patch</code></td>
    <td>ポッド中断予算にパッチが適用されます。</td></tr><tr>
    <td><code>poddisruptionbudgets.update</code></td>
    <td>ポッド中断予算が更新されます。</td></tr><tr>
    <td><code>podpresets.create</code></td>
    <td>ポッドの事前設定が作成されます。</td></tr><tr>
    <td><code>podpresets.deleted</code></td>
    <td>ポッドの事前設定が削除されます。</td></tr><tr>
    <td><code>podpresets.patched</code></td>
    <td>ポッドの事前設定にパッチが適用されます。</td></tr><tr>
    <td><code>podpresets.updated</code></td>
    <td>ポッドの事前設定が更新されます。</td></tr><tr>
    <td><code>pods.create</code></td>
    <td>ポッドが作成されます。</td></tr><tr>
    <td><code>pods.delete</code></td>
    <td>ポッドが削除されます。</td></tr><tr>
    <td><code>pods.patch</code></td>
    <td>ポッドにパッチが適用されます。</td></tr><tr>
    <td><code>pods.update</code></td>
    <td>ポッドが更新されます。</td></tr><tr>
    <td><code>podsecuritypolicies.create</code></td>
    <td>ポッド・セキュリティー・ポリシーが作成されます。</td></tr><tr>
    <td><code>podsecuritypolicies.delete</code></td>
    <td>ポッド・セキュリティー・ポリシーが削除されます。</td></tr><tr>
    <td><code>podsecuritypolicies.patch</code></td>
    <td>ポッド・セキュリティー・ポリシーにパッチが適用されます。</td></tr><tr>
    <td><code>podsecuritypolicies.update</code></td>
    <td>ポッド・セキュリティー・ポリシーが更新されます。</td></tr><tr>
    <td><code>podtemplates.create</code></td>
    <td>ポッド・テンプレートが作成されます。</td></tr><tr>
    <td><code>podtemplates.delete</code></td>
    <td>ポッド・テンプレートが削除されます。</td></tr><tr>
    <td><code>podtemplates.patch</code></td>
    <td>ポッド・テンプレートにパッチが適用されます。</td></tr><tr>
    <td><code>podtemplates.update</code></td>
    <td>ポッド・テンプレートが更新されます。</td></tr><tr>
    <td><code>replicasets.create</code></td>
    <td>レプリカ・セットが作成されます。</td></tr><tr>
    <td><code>replicasets.delete</code></td>
    <td>レプリカ・セットが削除されます。</td></tr><tr>
    <td><code>replicasets.patch</code></td>
    <td>レプリカ・セットにパッチが適用されます。</td></tr><tr>
    <td><code>replicasets.update</code></td>
    <td>レプリカ・セットが更新されます。</td></tr><tr>
    <td><code>replicationcontrollers.create</code></td>
    <td>レプリケーション・コントローラーが作成されます。</td></tr><tr>
    <td><code>replicationcontrollers.delete</code></td>
    <td>レプリケーション・コントローラーが削除されます。</td></tr><tr>
    <td><code>replicationcontrollers.patch</code></td>
    <td>レプリケーション・コントローラーにパッチが適用されます。</td></tr><tr>
    <td><code>replicationcontrollers.update</code></td>
    <td>レプリケーション・コントローラーが更新されます。</td></tr><tr>
    <td><code>resourcequotas.create</code></td>
    <td>リソース割り当て量が作成されます。</td></tr><tr>
    <td><code>resourcequotas.delete</code></td>
    <td>リソース割り当て量が削除されます。</td></tr><tr>
    <td><code>resourcequotas.patch</code></td>
    <td>リソース割り当て量にパッチが適用されます。</td></tr><tr>
    <td><code>resourcequotas.update</code></td>
    <td>リソース割り当て量が更新されます。</td></tr><tr>
    <td><code>rolebindings.create</code></td>
    <td>役割バインディングが作成されます。</td></tr><tr>
    <td><code>rolebindings.deleted</code></td>
    <td>役割バインディングが削除されます。</td></tr><tr>
    <td><code>rolebindings.patched</code></td>
    <td>役割バインディングにパッチが適用されます。</td></tr><tr>
    <td><code>rolebindings.updated</code></td>
    <td>役割バインディングが更新されます。</td></tr><tr>
    <td><code>roles.create</code></td>
    <td>役割が作成されます。</td></tr><tr>
    <td><code>roles.deleted</code></td>
    <td>役割が削除されます。</td></tr><tr>
    <td><code>roles.patched</code></td>
    <td>役割にパッチが適用されます。</td></tr><tr>
    <td><code>roles.updated</code></td>
    <td>役割が更新されます。</td></tr><tr>
    <td><code>secrets.create</code></td>
    <td>シークレットが作成されます。</td></tr><tr>
    <td><code>secrets.deleted</code></td>
    <td>シークレットが削除されます。</td></tr><tr>
    <td><code>secrets.get</code></td>
    <td>シークレットが表示されます。</td></tr><tr>
    <td><code>secrets.patch</code></td>
    <td>シークレットにパッチが適用されます。</td></tr><tr>
    <td><code>secrets.updated</code></td>
    <td>シークレットが更新されます。</td></tr><tr>
    <td><code>selfsubjectaccessreviews.create</code></td>
    <td>セルフ件名アクセス・レビューが作成されます。</td></tr><tr>
    <td><code>selfsubjectrulesreviews.create</code></td>
    <td>セルフ件名ルール・レビューが作成されます。</td></tr><tr>
    <td><code>subjectaccessreviews.create</code></td>
    <td>件名アクセス・レビューが作成されます。</td></tr><tr>
    <td><code>serviceaccounts.create</code></td>
    <td>サービス・アカウントが作成されます。</td></tr><tr>
    <td><code>serviceaccounts.deleted</code></td>
    <td>サービス・アカウントが削除されます。</td></tr><tr>
    <td><code>serviceaccounts.patch</code></td>
    <td>サービス・アカウントにパッチが適用されます。</td></tr><tr>
    <td><code>serviceaccounts.updated</code></td>
    <td>サービス・アカウントが更新されます。</td></tr><tr>
    <td><code>services.create</code></td>
    <td>サービスが作成されます。</td></tr><tr>
    <td><code>services.deleted</code></td>
    <td>サービスが削除されます。</td></tr><tr>
    <td><code>services.patch</code></td>
    <td>サービスにパッチが適用されます。</td></tr><tr>
    <td><code>services.updated</code></td>
    <td>サービスが更新されます。</td></tr><tr>
    <td><code>statefulsets.create</code></td>
    <td>ステートフル・セットが作成されます。</td></tr><tr>
    <td><code>statefulsets.delete</code></td>
    <td>ステートフル・セットが削除されます。</td></tr><tr>
    <td><code>statefulsets.patch</code></td>
    <td>ステートフル・セットにパッチが適用されます。</td></tr><tr>
    <td><code>statefulsets.update</code></td>
    <td>ステートフル・セットが更新されます。</td></tr><tr>
    <td><code>tokenreviews.create</code></td>
    <td>トークン・レビューが作成されます。</td></tr><tr>
    <td><code>validatingwebhookconfigurations.create</code></td>
    <td>Webhook 構成の検証が作成されます。</td></tr><tr>
    <td><code>validatingwebhookconfigurations.delete</code></td>
    <td>Webhook 構成の検証が削除されます。</td></tr><tr>
    <td><code>validatingwebhookconfigurations.patch</code></td>
    <td>Webhook 構成の検証にパッチが適用されます。</td></tr><tr>
    <td><code>validatingwebhookconfigurations.update</code></td>
    <td>Webhook 構成の検証が更新されます。</td></tr>
</table>
