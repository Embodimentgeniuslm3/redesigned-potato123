---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

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

# 管理対象アドオンを使用したサービスの追加
{: #managed-addons}

管理対象アドオンを使用して、クラスターにオープン・ソース・テクノロジーを素早く追加できます。
{: shortdesc}

**管理対象アドオンとは何ですか?** </br>
管理対象 {{site.data.keyword.containerlong_notm}} アドオンを使用すると、Istio や Knative などのオープン・ソース機能を使用してクラスターを簡単に強化できます。 クラスターに追加するオープン・ソース・ツールのバージョンは、IBM によってテストされて、{{site.data.keyword.containerlong_notm}} で使用することが承認されています。

**管理対象アドオンの課金とサポートはどのような仕組みになっていますか?** </br>
管理対象アドオンは、{{site.data.keyword.Bluemix_notm}} のサポート組織に完全に統合されています。 管理対象アドオンの使用に関する質問や問題がある場合は、いずれかの {{site.data.keyword.containerlong_notm}} サポート・チャネルを使用できます。 詳しくは、[ヘルプとサポートの取得](/docs/containers?topic=containers-cs_troubleshoot_clusters#clusters_getting_help)を参照してください。

クラスターに追加するツールについて費用が生じる場合は、これらの費用は自動的に統合されて、{{site.data.keyword.containerlong_notm}} に関する課金の一部としてリストされます。 課金サイクルは、クラスター内でそのアドオンを有効にしたタイミングに応じて {{site.data.keyword.Bluemix_notm}} 側で決定されます。

**どのような制限事項を考慮する必要がありますか?** </br>
[Container Image Security Enforcer アドミッション・コントローラー](/docs/services/Registry?topic=registry-security_enforce#security_enforce)をクラスター内にインストールした場合は、そのクラスター内で管理対象アドオンを有効にすることはできません。

## 管理対象アドオンの追加
{: #adding-managed-add-ons}

クラスター内で管理対象アドオンを有効にするには、[`ibmcloud ks cluster-addon-enable <addon_name> --cluster <cluster_name_or_ID>` コマンドを使用します](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_addon_enable)。 管理対象アドオンを有効にすると、当該ツールのサポート対象バージョンが、すべての Kubernetes リソースを含めてクラスターに自動的にインストールされます。 管理対象アドオンをインストールするためにクラスター側で満たしている必要のある前提条件については、各管理対象アドオンの資料を参照してください。

各アドオンの前提条件について詳しくは、以下を参照してください。

- [Istio](/docs/containers?topic=containers-istio#istio)
- [Knative](/docs/containers?topic=containers-serverless-apps-knative)

## 管理対象アドオンの更新
{: #updating-managed-add-ons}

各管理対象アドオンのバージョンは、{{site.data.keyword.Bluemix_notm}} によってテストされており、{{site.data.keyword.containerlong_notm}} で使用することが承認されています。 アドオンのコンポーネントを {{site.data.keyword.containerlong_notm}} でサポートされている最新バージョンに更新するには、以下の手順を使用します。
{: shortdesc}

1. 対象のアドオンが最新バージョンかどうかを確認します。 `* (<version> latest)` が付いているアドオンはすべて更新できます。
   ```
   ibmcloud ks cluster-addons --cluster <mycluster>
   ```
   {: pre}

   出力例:
   ```
   OK
   Name      Version
   istio     1.1.5
   knative   0.5.2
   ```
   {: screen}

2. そのアドオンによって生成された名前空間内で作成または変更したすべてのリソース (サービスやアプリの構成ファイルなど) を保存します。 例えば、Istio アドオンでは `istio-system` が使用され、Knative アドオンでは `knative-serving`、`knative-monitoring`、`knative-eventing`、および `knative-build` が使用されます。
   コマンド例:
   ```
   kubectl get pod <pod_name> -o yaml -n istio-system
   ```
   {: pre}

3. 管理対象アドオンの名前空間内で自動的に生成された Kubernetes リソースをローカル・マシン上の YAML ファイルに保存します。 これらのリソースは、カスタム・リソース定義 (CRD) を使用して生成されます。
   1. アドオンで使用されている名前空間から、そのアドオンの CRD を取得します。 例えば、Istio アドオンの場合は、`istio-system` 名前空間から CRD を取得します。
      ```
      kubectl get crd -n istio-system
      ```
      {: pre}

   2. これらの CRD から作成されたすべてのリソースを保存します。

4. Knative の場合のオプション: 以下のいずれかのリソースを変更した場合は、YAML ファイルを取得して、ローカル・マシンに保存してください。 これらのリソースのいずれかを変更した場合に、インストール済みのデフォルトを代わりに使用する場合は、そのリソースを削除してかまいません。数分後に、そのリソースはインストール済みデフォルト値を使用して再作成されます。
  <table summary="Knative リソースの表">
  <caption>Knative リソース</caption>
  <thead><tr><th>リソース名</th><th>リソース・タイプ</th><th>名前空間</th></tr></thead>
  <tbody>
  <tr><td><code>config-autoscaler</code></td><td>ConfigMap</td><td><code>knative-serving</code></td></tr>
  <tr><td><code>config-controller</code></td><td>ConfigMap</td><td><code>knative-serving</code></td></tr>
  <tr><td><code>config-domain</code></td><td>ConfigMap</td><td><code>knative-serving</code></td></tr>
  <tr><td><code>config-gc</code></td><td>ConfigMap</td><td><code>knative-serving</code></td></tr>
  <tr><td><code>config-istio</code></td><td>ConfigMap</td><td><code>knative-serving</code></td></tr>
  <tr><td><code>config-logging</code></td><td>ConfigMap</td><td><code>knative-serving</code></td></tr>
  <tr><td><code>config-network</code></td><td>ConfigMap</td><td><code>knative-serving</code></td></tr>
  <tr><td><code>config-observability</code></td><td>ConfigMap</td><td><code>knative-serving</code></td></tr>
  <tr><td><code>kaniko</code></td><td>BuildTemplate</td><td><code>default</code></td></tr>
  <tr><td><code>iks-knative-ingress</code></td><td>Ingress</td><td><code>istio-system</code></td></tr>
  </tbody></table>

  コマンド例:
  ```
  kubectl get cm config-autoscaler -n knative-serving -o yaml > config-autoscaler.yaml
  ```
  {: pre}

5. `istio-sample-bookinfo` アドオンと `istio-extras` アドオンを有効にした場合は、これらを無効にします。
   1. `istio-sample-bookinfo` アドオンを無効にします。
      ```
      ibmcloud ks cluster-addon-disable istio-sample-bookinfo --cluster <cluster_name_or_ID>
      ```
      {: pre}

   2. `istio-extras` アドオンを無効にします。
      ```
      ibmcloud ks cluster-addon-disable istio-extras --cluster <cluster_name_or_ID>
      ```
      {: pre}

6. アドオンを無効にします。
   ```
   ibmcloud ks cluster-addon-disable <add-on_name> --cluster <cluster_name_or_ID> -f
   ```
   {: pre}

7. 次のステップに進む前に、対象アドオンの名前空間内の対象アドオンのリソースが削除されていること、または対象アドオンの名前空間自体が削除されていることを確認します。
   * 例えば、`istio-extras` アドオンを更新する場合は、`grafana`、`kiali` および `jaeger-*` というサービスが `istio-system` 名前空間から削除されていることを確認します。
     ```
     kubectl get svc -n istio-system
     ```
     {: pre}
   * 例えば、`knative` アドオンを更新する場合は、`knative-serving`、`knative-monitoring`、`knative-eventing`、`knative-build`、および `istio-system` という名前空間が削除されていることを確認します。 これらの名前空間は、削除されるまでの数分間、**状況**が `Terminating` になることがあります。
     ```
     kubectl get namespaces -w
     ```
     {: pre}

8. 更新後のアドオン・バージョンを選択します。
   ```
   ibmcloud ks addon-versions
   ```
   {: pre}

9. アドオンを再び有効にします。 `--version` フラグを使用して、インストールするバージョンを指定します。 バージョンが指定されない場合は、デフォルトのバージョンがインストールされます。
   ```
   ibmcloud ks cluster-addon-enable <add-on_name> --cluster <cluster_name_or_ID> --version <version>
   ```
   {: pre}

10. ステップ 2 で保存した CRD リソースを適用します。
    ```
    kubectl apply -f <file_name> -n <namespace>
    ```
    {: pre}

11. Knative の場合のオプション: ステップ 3 でいずれかのリソースを保存した場合は、それらを再び適用します。
    コマンド例:
    ```
    kubectl apply -f config-autoscaler.yaml -n knative-serving
    ```
    {: pre}

12. Istio の場合のオプション: `istio-extras` アドオンと `istio-sample-bookinfo` アドオンを再び有効にします。 `istio` アドオンと同じバージョンをこれらのアドオンに使用します。
    1. `istio-extras` アドオンを有効にします。
       ```
       ibmcloud ks cluster-addon-enable istio-extras --cluster <cluster_name_or_ID> --version <version>
       ```
       {: pre}

    2. `istio-sample-bookinfo` アドオンを有効にします。
       ```
       ibmcloud ks cluster-addon-enable istio-sample-bookinfo --cluster <cluster_name_or_ID> --version <version>
       ```
       {: pre}

13. Istio の場合のオプション: ゲートウェイ構成ファイルで TLS セクションを使用する場合は、Envoy がシークレットにアクセスできるように、ゲートウェイを削除して再作成する必要があります。
  ```
  kubectl delete gateway mygateway
  kubectl create -f mygateway.yaml
  ```
  {: pre}
