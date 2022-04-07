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

# 使用受管理附加程式新增服務
{: #managed-addons}

使用受管理附加程式，快速將開放程式碼技術新增至叢集。
{: shortdesc}

**何謂受管理附加程式？** </br>
受管理 {{site.data.keyword.containerlong_notm}} 附加程式是一種使用 Istio 或 Knative 這類開放程式碼功能來加強叢集的簡單方式。您新增至叢集的開放程式碼工具版本經由 IBM 測試並核准用於 {{site.data.keyword.containerlong_notm}} 中。

**計費及支援如何作用於受管理附加程式？** </br>
受管理附加程式已完全整合至 {{site.data.keyword.Bluemix_notm}} 支援組織。如果您有使用受管理附加程式的問題，可以使用其中一個 {{site.data.keyword.containerlong_notm}} 支援頻道。如需相關資訊，請參閱[取得協助及支援](/docs/containers?topic=containers-cs_troubleshoot_clusters#clusters_getting_help)。

如果您新增至叢集的工具產生成本需求，則這些成本會自動整合並列在您的 {{site.data.keyword.containerlong_notm}} 計費中。計費週期由 {{site.data.keyword.Bluemix_notm}} 決定，視您在叢集裡啟用附加程式的時間而定。

**需要考量的限制為何？** </br>
如果您已在叢集裡安裝[容器映像檔安全強制執行程式許可控制器](/docs/services/Registry?topic=registry-security_enforce#security_enforce)，則無法在叢集裡啟用受管理附加程式。

## 新增受管理附加程式
{: #adding-managed-add-ons}

若要在叢集裡啟用受管理附加程式，請使用 [`ibmcloud ks cluster-addon-enable <addon_name> --cluster <cluster_name_or_ID>` 指令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_addon_enable)。當您啟用受管理附加程式時，會自動在叢集裡安裝工具的支援版本，包括所有 Kubernetes 資源。請參閱每個受管理附加程式的文件，以尋找您的叢集必須符合才能安裝受管理附加程式的必要條件。

如需每個附加程式必要條件的相關資訊，請參閱：

- [Istio](/docs/containers?topic=containers-istio#istio)
- [Knative](/docs/containers?topic=containers-serverless-apps-knative)

## 更新受管理附加程式
{: #updating-managed-add-ons}

每個受管理附加程式的版本都經由 {{site.data.keyword.Bluemix_notm}} 測試並核准用於 {{site.data.keyword.containerlong_notm}} 中。若要將附加程式的元件更新為 {{site.data.keyword.containerlong_notm}} 所支援的最新版本，請使用下列步驟。
{: shortdesc}

1. 檢查附加程式是否為最新版本。可以更新以 `* (<version> latest)` 表示的任何附加程式。
   ```
   ibmcloud ks cluster-addons --cluster <mycluster>
   ```
   {: pre}

   輸出範例：
   ```
   OK
   Name      Version
   istio     1.1.5
   knative   0.5.2
   ```
   {: screen}

2. 儲存您在附加程式所產生的名稱空間中建立或修改的任何資源（例如任何服務或應用程式的配置檔）。例如，Istio 附加程式使用 `istio-system`，而 Knative 附加程式使用 `knative-serving`、`knative-monitoring`、`knative-eventing` 及 `knative-build`。
   指令範例：
   ```
   kubectl get pod <pod_name> -o yaml -n istio-system
   ```
   {: pre}

3. 將受管理附加程式名稱空間中自動產生的 Kubernetes 資源儲存至本端機器上的 YAML 檔案。這些資源是使用自訂資源定義 (CRD) 所產生。
   1. 從附加程式使用的名稱空間中取得附加程式的 CRD。例如，若為 Istio 附加程式，請從 `istio-system` 名稱空間中取得 CRD。
      ```
      kubectl get crd -n istio-system
      ```
      {: pre}

   2. 儲存從這些 CRD 建立的所有資源。

4. Knative 的選用項目：如果您已修改下列任何資源，請取得 YAML 檔案，並將其儲存至本端機器。如果修改了其中任何資源，但希望改為使用安裝的預設，則可以刪除該資源。在幾分鐘之後，會使用已安裝的預設值來重建資源。
  <table summary="Knative 資源表格">
  <caption>Knative 資源</caption>
  <thead><tr><th>資源名稱</th><th>資源類型</th><th>名稱空間</th></tr></thead>
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

  指令範例：
  ```
  kubectl get cm config-autoscaler -n knative-serving -o yaml > config-autoscaler.yaml
  ```
  {: pre}

5. 如果您已啟用 `istio-sample-bookinfo` 及 `istio-extras` 附加程式，請予以停用。
   1. 停用 `istio-sample-bookinfo` 附加程式。
      ```
      ibmcloud ks cluster-addon-disable istio-sample-bookinfo --cluster <cluster_name_or_ID>
      ```
      {: pre}

   2. 停用 `istio-extras` 附加程式。
      ```
      ibmcloud ks cluster-addon-disable istio-extras --cluster <cluster_name_or_ID>
      ```
      {: pre}

6. 停用附加程式。
   ```
   ibmcloud ks cluster-addon-disable <add-on_name> --cluster <cluster_name_or_ID> -f
   ```
   {: pre}

7. 在繼續執行下一步之前，請驗證是已移除附加程式名稱空間內附加程式中的資源，還是已移除附加程式名稱空間本身。
   * 例如，如果您更新 `istio-extras` 附加程式，則可能會驗證已從 `istio-system` 名稱空間移除 `grafana`、`kiali` 及 `jaeger-*` 服務。
     ```
     kubectl get svc -n istio-system
     ```
     {: pre}
   * 例如，如果您更新 `knative` 附加程式，則可能會驗證已刪除 `knative-serving`、`knative-monitoring`、`knative-eventing`、`knative-build` 及 `istio-system` 名稱空間。名稱空間在被刪除前的幾分鐘內，其 **STATUS** 可能為 `Terminating`。
     ```
     kubectl get namespaces -w
     ```
     {: pre}

8. 選擇您要更新為的附加程式版本。
   ```
   ibmcloud ks addon-versions
   ```
   {: pre}

9. 重新啟用附加程式。使用 `--version` 旗標來指定您要安裝的版本。如果未指定任何版本，則會安裝預設版本。
   ```
   ibmcloud ks cluster-addon-enable <add-on_name> --cluster <cluster_name_or_ID> --version <version>
   ```
   {: pre}

10. 套用您在步驟 2 中儲存的 CRD 資源。
    ```
    kubectl apply -f <file_name> -n <namespace>
    ```
    {: pre}

11. 對於 Knative 為選用：如果在步驟 3 中已儲存任何資源，請重新應用這些資源。
    指令範例：
    ```
    kubectl apply -f config-autoscaler.yaml -n knative-serving
    ```
    {: pre}

12. Istio 的選用項目：重新啟用 `istio-extras` 及 `istio-sample-bookinfo` 附加程式。對於這些附加程式，使用與 `istio` 附加程式相同的版本。
    1. 啟用 `istio-extras` 附加程式。
       ```
       ibmcloud ks cluster-addon-enable istio-extras --cluster <cluster_name_or_ID> --version <version>
       ```
       {: pre}

    2. 啟用 `istio-sample-bookinfo` 附加程式。
       ```
       ibmcloud ks cluster-addon-enable istio-sample-bookinfo --cluster <cluster_name_or_ID> --version <version>
       ```
       {: pre}

13. 對於 Istio 為選用：如果在閘道配置檔中使用 TLS 區段，則必須刪除並重建閘道，以便 Envoy 可以存取這些密碼。
  ```
  kubectl delete gateway mygateway
  kubectl create -f mygateway.yaml
  ```
  {: pre}
