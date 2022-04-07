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


# 記載及監視 Ingress
{: #ingress_health}

自訂記載以及設定監視，以協助對問題進行疑難排解，以及改善 Ingress 配置的效能。
{: shortdesc}

## 檢視 Ingress 日誌
{: #ingress_logs}

如果您要對 Ingress 進行疑難排解，或監視 Ingress 活動，可以檢閱 Ingress 日誌。
{: shortdesc}

系統會自動收集 Ingress ALB 的日誌。若要檢視 ALB 日誌，有兩個選項可供您選擇。
* 在叢集裡[建立 Ingress 服務的記載配置](/docs/containers?topic=containers-health#configuring)。
* 從 CLI 檢查日誌。**附註**：您必須至少具有 `kube-system` 名稱空間的 [**Reader** {{site.data.keyword.Bluemix_notm}} IAM 服務角色](/docs/containers?topic=containers-users#platform)。
    1. 取得 ALB Pod 的 ID。
        ```
        kubectl get pods -n kube-system | grep alb
        ```
        {: pre}

    2. 開啟該 ALB Pod 的日誌。
        ```
        kubectl logs <ALB_pod_ID> nginx-ingress -n kube-system
        ```
        {: pre}

</br>預設 Ingress 日誌內容採用 JSON 格式化，並顯示一般欄位來說明用戶端與應用程式之間的連線階段作業。具有預設欄位的範例日誌如下所示：

```
{"time_date": "2018-08-21T17:33:19+00:00", "client": "108.162.248.42", "host": "albhealth.multizone.us-south.containers.appdomain.cloud", "scheme": "http", "request_method": "GET", "request_uri": "/", "request_id": "c2bcce90cf2a3853694da9f52f5b72e6", "status": 200, "upstream_addr": "192.168.1.1:80", "upstream_status": 200, "request_time": 0.005, "upstream_response_time": 0.005, "upstream_connect_time": 0.000, "upstream_header_time": 0.005}
```
{: screen}

<table>
<caption>瞭解預設 Ingress 日誌格式的欄位</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="構想圖示"/>瞭解預設 Ingress 日誌格式的欄位</th>
</thead>
<tbody>
<tr>
<td><code>"time_date": "$time_iso8601"</code></td>
<td>寫入日誌時使用 ISO 8601 標準格式的當地時間。</td>
</tr>
<tr>
<td><code>"client": "$remote_addr"</code></td>
<td>用戶端傳至您應用程式的要求套件的 IP 位址。此 IP 可能因下列狀況而變更：<ul><li>將應用程式的用戶端要求傳送至叢集時，會將該要求遞送至可公開 ALB 之負載平衡器服務的 Pod。如果沒有應用程式 Pod 存在於與負載平衡器服務 Pod 相同的工作者節點上，則負載平衡器會將要求轉遞至不同工作者節點上的應用程式 Pod。要求套件的來源 IP 位址會變更為應用程式 Pod 執行所在之工作者節點的公用 IP 位址。</li><li>如果[已啟用來源 IP 保留](/docs/containers?topic=containers-ingress#preserve_source_ip)，則會改為記錄您應用程式之用戶端要求的原始 IP 位址。</li></ul></td>
</tr>
<tr>
<td><code>"host": "$http_host"</code></td>
<td>可透過其存取您的應用程式的主機或子網域。此子網域是配置在 ALB 的 Ingress 資源檔中。</td>
</tr>
<tr>
<td><code>"scheme": "$scheme"</code></td>
<td>要求的類型：<code>HTTP</code> 或 <code>HTTPS</code>。</td>
</tr>
<tr>
<td><code>"request_method": "$request_method"</code></td>
<td>對後端應用程式的要求呼叫方法，例如 <code>GET</code> 或 <code>POST</code>。</td>
</tr>
<tr>
<td><code>"request_uri": "$uri"</code></td>
<td>應用程式路徑的原始要求 URI。ALB 會以字首來處理應用程式接聽的路徑。當 ALB 接收用戶端對應用程式的要求時，ALB 會檢查 Ingress 資源中的路徑（作為字首）是否有符合此要求 URI 中的路徑。</td>
</tr>
<tr>
<td><code>"request_id": "$request_id"</code></td>
<td>從 16 個隨機位元組產生的唯一要求 ID。</td>
</tr>
<tr>
<td><code>"status": $status</code></td>
<td>連線階段作業的狀態碼。<ul>
<li><code>200</code>：階段作業順利完成</li>
<li><code>400</code>：無法剖析用戶端資料</li>
<li><code>403</code>：禁止存取；例如，當特定用戶端 IP 位址的存取受到限制時</li>
<li><code>500</code>：內部伺服器錯誤</li>
<li><code>502</code>：不當的閘道；例如，如果無法選取或呼叫到上游伺服器</li>
<li><code>503</code>：服務無法使用；例如，存取受到連線數目的限制</li>
</ul></td>
</tr>
<tr>
<td><code>"upstream_addr": "$upstream_addr"</code></td>
<td>上游伺服器之 UNIX 網域 Socket 的 IP 位址及埠或路徑。如果在要求處理期間聯絡了數個伺服器，其位址會以逗點區隔：<code>"192.168.1.1:80, 192.168.1.2:80, unix:/tmp/sock"</code>。如果該要求在內部就從一個伺服器群組重新導向至另一個伺服器群組，則不同群組中的伺服器位址會以冒號區隔：<code>"192.168.1.1:80, 192.168.1.2:80, unix:/tmp/sock : 192.168.10.1:80, 192.168.10.2:80"</code>。如果 ALB 無法選取伺服器，則會改為記載伺服器群組的名稱。</td>
</tr>
<tr>
<td><code>"upstream_status": $upstream_status</code></td>
<td>從上游伺服器取得對後端應用程式的回應狀態碼，例如標準 HTTP 回應碼。數個回應的狀態碼以逗點和冒號區隔，例如 <code>$upstream_addr</code> 變數中的位址。如果 ALB 無法選取伺服器，則會記載 502（不當的閘道）狀態碼。</td>
</tr>
<tr>
<td><code>"request_time": $request_time</code></td>
<td>要求處理時間（以秒為測量單位），並以毫秒解析。此時間會在 ALB 讀取用戶端要求的第一個位元組時開始，並在 ALB 將回應的最後位元組傳送給用戶端時停止。在要求處理時間停止之後，會立即寫入日誌。</td>
</tr>
<tr>
<td><code>"upstream_response_time": $upstream_response_time</code></td>
<td>ALB 從上游伺服器接收對後端應用程式的回應所花費的時間（以秒為測量單位），並以毫秒解析。數個回應的時間以逗點和冒號區隔，例如 <code>$upstream_addr</code> 變數中的位址。</td>
</tr>
<tr>
<td><code>"upstream_connect_time": $upstream_connect_time</code></td>
<td>ALB 為了後端應用程式而與上游伺服器建立連線所花費的時間（以秒為測量單位），並以毫秒解析。如果 Ingress 資源配置中已啟用 TLS/SSL，則此時間包括花費在信號交換的時間。數個連線的時間以逗點和冒號區隔，例如 <code>$upstream_addr</code> 變數中的位址。</td>
</tr>
<tr>
<td><code>"upstream_header_time": $upstream_header_time</code></td>
<td>ALB 從上游伺服器接收對後端應用程式的回應標頭所花費的時間（以秒為測量單位），並以毫秒解析。數個連線的時間以逗點和冒號區隔，例如 <code>$upstream_addr</code> 變數中的位址。</td>
</tr>
</tbody></table>

## 自訂 Ingress 日誌內容及格式
{: #ingress_log_format}

您可以自訂針對 Ingress ALB 所收集日誌的內容及格式。
{:shortdesc}

依預設，Ingress 日誌會格式化為 JSON，並顯示一般的日誌欄位。不過，您也可以選擇要轉遞的日誌元件，以及元件在日誌輸出中的排列方式，來建立自訂日誌格式。

開始之前，請確定您具有 `kube-system` 名稱空間的[**撰寫者**或**管理員** {{site.data.keyword.Bluemix_notm}}IAM 服務角色](/docs/containers?topic=containers-users#platform)。

1. 編輯 `ibm-cloud-provider-ingress-cm` configmap 資源的配置檔。

    ```
    kubectl edit cm ibm-cloud-provider-ingress-cm -n kube-system
    ```
    {: pre}

2. 新增 <code>data</code> 區段。新增 `log-format` 欄位，並選擇性地新增 `log-format-escape-json` 欄位。

    ```
    apiVersion: v1
    data:
      log-format: '{<key1>: <log_variable1>, <key2>: <log_variable2>, <key3>: <log_variable3>}'
      log-format-escape-json: "true"
    kind: ConfigMap
    metadata:
      name: ibm-cloud-provider-ingress-cm
      namespace: kube-system
    ```
    {: codeblock}

    <table>
    <caption>YAML 檔案元件</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解 log-format 配置</th>
    </thead>
    <tbody>
    <tr>
    <td><code>log-format</code></td>
    <td>將 <code>&lt;key&gt;</code> 取代為日誌元件的名稱，並將 <code>&lt;log_variable&gt;</code> 取代為您要在日誌項目中收集的日誌元件的變數。您可以包括想要日誌項目包含的文字和標點符號，例如，用來括住字串值的引號以及用來區隔日誌元件的逗點。例如，將元件格式化為 <code>request: "$request"</code> 會在日誌項目中產生下列內容：<code>request: "GET / HTTP/1.1"</code>。如需您可以使用的所有變數清單，請參閱 <a href="http://nginx.org/en/docs/varindex.html">NGINX 變數索引</a>。<br><br>若要記載 <em>x-custom-ID</em> 這類額外的標頭，請將下列鍵值組新增至自訂日誌內容：<br><pre class="codeblock"><code>customID: $http_x_custom_id</code></pre> <br>連字號 (<code>-</code>) 會轉換為底線 (<code>_</code>)，而且必須將 <code>$http_</code> 附加到自訂標頭名稱前面。</td>
    </tr>
    <tr>
    <td><code>log-format-escape-json</code></td>
    <td>選用項目：依預設，會以文字格式產生日誌。若要產生 JSON 格式的日誌，請新增 <code>log-format-escape-json</code> 欄位，並使用值 <code>true</code>。</td>
    </tr>
    </tbody></table>

    例如，您的日誌格式可能包含下列變數：
    ```
    apiVersion: v1
    data:
      log-format: '{remote_address: $remote_addr, remote_user: "$remote_user",
                    time_date: [$time_local], request: "$request",
                    status: $status, http_referer: "$http_referer",
                    http_user_agent: "$http_user_agent",
                    request_id: $request_id}'
    kind: ConfigMap
    metadata:
      name: ibm-cloud-provider-ingress-cm
      namespace: kube-system
    ```
    {: screen}

    根據此格式的日誌項目類似下列範例：
    ```
    remote_address: 127.0.0.1, remote_user: "dbmanager", time_date: [30/Mar/2018:18:52:17 +0000], request: "GET / HTTP/1.1", status: 401, http_referer: "-", http_user_agent: "Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:47.0) Gecko/20100101 Firefox/47.0", request_id: a02b2dea9cf06344a25611c1d7ad72db
    ```
    {: screen}

    若要建立根據預設 ALB 日誌格式的自訂日誌格式，請視需要修改下列區段，並將它新增至您的 configmap：
    ```
    apiVersion: v1
    data:
      log-format: '{"time_date": "$time_iso8601", "client": "$remote_addr",
                    "host": "$http_host", "scheme": "$scheme",
                    "request_method": "$request_method", "request_uri": "$uri",
                    "request_id": "$request_id", "status": $status,
                    "upstream_addr": "$upstream_addr", "upstream_status":
                    $upstream_status, "request_time": $request_time,
                    "upstream_response_time": $upstream_response_time,
                    "upstream_connect_time": $upstream_connect_time,
                    "upstream_header_time": $upstream_header_time}'
      log-format-escape-json: "true"
    kind: ConfigMap
    metadata:
      name: ibm-cloud-provider-ingress-cm
      namespace: kube-system
    ```
    {: codeblock}

4. 儲存配置檔。

5. 驗證已套用 configmap 變更。

   ```
   kubectl get cm ibm-cloud-provider-ingress-cm -n kube-system -o yaml
   ```
   {: pre}

4. 若要檢視 Ingress ALB 日誌，有兩個選項可供您選擇。
    * 在叢集裡[建立 Ingress 服務的記載配置](/docs/containers?topic=containers-health#logging)。
    * 從 CLI 檢查日誌。
        1. 取得 ALB Pod 的 ID。
            ```
            kubectl get pods -n kube-system | grep alb
            ```
            {: pre}

        2. 開啟該 ALB Pod 的日誌。驗證日誌遵循已更新格式。
            ```
            kubectl logs <ALB_pod_ID> nginx-ingress -n kube-system
            ```
            {: pre}

<br />


## 監視 Ingress ALB
{: #ingress_monitoring}

將度量值匯出程式和 Prometheus 代理程式部署至叢集，以監視 ALB。
{: shortdesc}

ALB 度量值匯出程式使用 NGINX 指引 `vhost_traffic_status_zone`，來收集每個 Ingress ALB Pod 上 `/status/format/json` 端點中的度量值資料。度量值匯出程式會自動將 JSON 檔案中的每個資料欄位都重新格式化為 Prometheus 可讀取的度量值。然後，Prometheus 代理程式會反映匯出程式所產生的度量值，並在 Prometheus 儀表板上顯示度量值。

### 安裝度量值匯出程式 Helm 圖表
{: #metrics-exporter}

安裝度量值匯出程式 Helm 圖表，以監視叢集裡的 ALB。
{: shortdesc}

ALB 度量值匯出程式 Pod 必須部署至 ALB 部署所在的相同工作者節點。如果 ALB 是在邊緣工作者節點上執行，並且這些邊緣節點有污點而導致無法進行其他工作負載部署，則無法排定度量值匯出程式 Pod。您必須執行 `kubectl taint node <node_name> dedicated:NoSchedule- dedicated:NoExecute-` 來移除污點。
{: note}

1.  **重要事項**：[遵循指示](/docs/containers?topic=containers-helm#public_helm_install)，在您的本端機器上安裝 Helm 用戶端、使用服務帳戶安裝 Helm 伺服器 (Tiller)，以及新增 {{site.data.keyword.Bluemix_notm}} Helm 儲存庫。

2. 將 `ibmcloud-alb-metrics-exporter` Helm 圖表安裝至叢集。此 Helm 圖表會部署 ALB 度量值匯出程式，並在 `kube-system` 名稱空間中建立 `alb-metrics-service-account` 服務帳戶。將 <alb-ID> 取代為您要收集其度量值之 ALB 的 ID。若要檢視叢集裡 ALB 的 ID，請執行 <code>ibmcloud ks albs --cluster &lt;cluster_name&gt;</code>。您必須針對要監視的每個 ALB 部署圖表。
  {: note}
  ```
  helm install iks-charts/ibmcloud-alb-metrics-exporter --name ibmcloud-alb-metrics-exporter --set metricsNameSpace=kube-system --set albId=<alb-ID>
  ```
  {: pre}

3. 檢查圖表部署狀態。圖表就緒時，輸出頂端附近的 **STATUS** 欄位會具有 `DEPLOYED` 值。
  ```
    helm status ibmcloud-alb-metrics-exporter
    ```
  {: pre}

4. 驗證 `ibmcloud-alb-metrics-exporter` Pod 在執行中。
  ```
    kubectl get pods -n kube-system -o wide
    ```
  {:pre}

  輸出範例：
  ```
  NAME                                             READY     STATUS      RESTARTS   AGE       IP               NODE
  ...
  alb-metrics-exporter-868fddf777-d49l5            1/1       Running     0          19s       172.30.xxx.xxx   10.xxx.xx.xxx
  alb-metrics-exporter-868fddf777-pf7x5            1/1       Running     0          19s       172.30.xxx.xxx   10.xxx.xx.xxx
  ```
  {:screen}

5. 選用項目：[安裝 Prometheus 代理程式](#prometheus-agent)以反映匯出程式所產生的度量值，並在 Prometheus 儀表板上顯示度量值。

### 安裝 Prometheus 代理程式 Helm 圖表
{: #prometheus-agent}

在安裝[度量值匯出程式](#metrics-exporter)之後，您可以安裝 Prometheus 代理程式 Helm 圖表以反映匯出程式所產生的度量值，並在 Prometheus 儀表板上顯示度量值。
{: shortdesc}

1. 從 https://icr.io/helm/iks-charts/charts/ibmcloud-alb-metrics-exporter-1.0.7.tgz 下載度量值匯出程式 Helm 圖表的 TAR 檔案。

2. 導覽至 Prometheus 子資料夾。
  ```
  cd ibmcloud-alb-metrics-exporter-1.0.7.tar/ibmcloud-alb-metrics-exporter/subcharts/prometheus
  ```
  {: pre}

3. 將 Prometheus Helm 圖表安裝至叢集。將 <ingress_subdomain> 取代為叢集的 Ingress 子網域。Prometheus 儀表板的 URL 是預設 Prometheus 子網域 `prom-dash` 與 Ingress 子網域的組合，例如 `prom-dash.mycluster-12345.us-south.containers.appdomain.cloud`。若要尋找叢集的 Ingress 子網域，請執行 <code>ibmcloud ks cluster-get --cluster &lt;cluster_name&gt;</code>。
  ```
  helm install --name prometheus . --set nameSpace=kube-system --set hostName=prom-dash.<ingress_subdomain>
  ```
  {: pre}

4. 檢查圖表部署狀態。圖表就緒時，輸出頂端附近的 **STATUS** 欄位會具有 `DEPLOYED` 值。
    ```
    helm status prometheus
    ```
    {: pre}

5. 驗證 `prometheus` Pod 在執行中。
    ```
    kubectl get pods -n kube-system -o wide
    ```
    {:pre}

    輸出範例：
    ```
    NAME                                             READY     STATUS      RESTARTS   AGE       IP               NODE
    alb-metrics-exporter-868fddf777-d49l5            1/1       Running     0          19s       172.30.xxx.xxx   10.xxx.xx.xxx
    alb-metrics-exporter-868fddf777-pf7x5            1/1       Running     0          19s       172.30.xxx.xxx   10.xxx.xx.xxx
    prometheus-9fbcc8bc7-2wvbk                       1/1       Running     0          1m        172.30.xxx.xxx   10.xxx.xx.xxx
    ```
    {:screen}

6. 在瀏覽器中，輸入 Prometheus 儀表板的 URL。此主機名稱的格式為 `prom-dash.mycluster-12345.us-south.containers.appdomain.cloud`。即會開啟 ALB 的 Prometheus 儀表板。

7. 檢閱儀表板中所列 [ALB](#alb_metrics)、[伺服器](#server_metrics)及[上游](#upstream_metrics)度量值的相關資訊。

### ALB 度量值
{: #alb_metrics}

`alb-metrics-exporter` 會自動將 JSON 檔案中的每個資料欄位都重新格式化為 Prometheus 可讀取的度量值。ALB 度量值會收集 ALB 所處理之連線和回應的資料。
{: shortdesc}

ALB 度量值的格式為 `kube_system_<ALB-ID>_<METRIC-NAME> <VALUE>`。例如，如果 ALB 收到 23 個具有 2xx 層次狀態碼的回應，則會將度量值格式化為 `kube_system_public_crf02710f54fcc40889c301bfd6d5b77fe_alb1_totalHandledRequest {.. metric="2xx"} 23`，其中 `metric` 是 Prometheus 標籤。

下表列出度量值標籤格式為 `<ALB_metric_name>_<metric_label>` 的受支援 ALB 度量值名稱
<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="構想圖示"/> 受支援的 ALB 度量值</th>
</thead>
<tbody>
<tr>
<td><code>connections_reading</code></td>
<td>讀取中用戶端連線總數。</td>
</tr>
<tr>
<td><code>connections_accepted</code></td>
<td>已接受用戶端連線總數。</td>
</tr>
<tr>
<td><code>connections_active</code></td>
<td>作用中用戶端連線總數。</td>
</tr>
<tr>
<td><code>connections_handled</code></td>
<td>已處理用戶端連線總數。</td>
</tr>
<tr>
<td><code>connections_requests</code></td>
<td>已要求用戶端連線總數。</td>
</tr>
<tr>
<td><code>connections_waiting</code></td>
<td>等待中用戶端連線總數。</td>
</tr>
<tr>
<td><code>connections_writing</code></td>
<td>寫入中用戶端連線總數。</td>
</tr>
<tr>
<td><code>totalHandledRequest_1xx</code></td>
<td>狀態碼為 1xx 的回應數目。</td>
</tr>
<tr>
<td><code>totalHandledRequest_2xx</code></td>
<td>狀態碼為 2xx 的回應數目。</td>
</tr>
<tr>
<td><code>totalHandledRequest_3xx</code></td>
<td>狀態碼為 3xx 的回應數目。</td>
</tr>
<tr>
<td><code>totalHandledRequest_4xx</code></td>
<td>狀態碼為 4xx 的回應數目。</td>
</tr>
<tr>
<td><code>totalHandledRequest_5xx</code></td>
<td>狀態碼為 5xx 的回應數目。</td>
</tr>
<tr>
<td><code>totalHandledRequest_total</code></td>
<td>接收自用戶端的用戶端要求總數。</td>
  </tr></tbody>
</table>

### 伺服器度量值
{: #server_metrics}

`alb-metrics-exporter` 會自動將 JSON 檔案中的每個資料欄位都重新格式化為 Prometheus 可讀取的度量值。伺服器度量值收集 Ingress 資源中所定義子網域的資料；例如，`dev.demostg1.stg.us.south.containers.appdomain.cloud`。
{: shortdesc}

伺服器度量值的格式為 `kube_system_server_<ALB-ID>_<SUB-TYPE>_<SERVER-NAME>_<METRIC-NAME> <VALUE>`。

`<SERVER-NAME>_<METRIC-NAME>` 已格式化為標籤。例如，`albId="dev_demostg1_us-south_containers_appdomain_cloud",metric="out"`

例如，如果伺服器已將共 22319 個位元組傳送給用戶端，則會將度量值格式化為：
```
kube_system_server_public_cra6a6eb9e897e41c4a5e58f957b417aec_alb1_bytes{albId="dev_demostg1_us-south_containers_appdomain_cloud",app="alb-metrics-exporter",instance="172.30.140.68:9913",job="kubernetes-pods",kubernetes_namespace="kube-system",kubernetes_pod_name="alb-metrics-exporter-7d495d785c-8wfw4",metric="out",pod_template_hash="3805183417"} 22319
```
{: screen}

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解伺服器度量值格式</th>
</thead>
<tbody>
<tr>
<td><code>&lt;ALB-ID&gt;</code></td>
<td>ALB ID。在上述範例中，ALB ID 為 <code>public\_cra6a6eb9e897e41c4a5e58f957b417aec\_alb1</code>。</td>
</tr>
<tr>
<td><code>&lt;SUB-TYPE&gt;</code></td>
<td>度量值的子類型。每個子類型都對應至一個以上的度量值名稱。
<ul>
<li><code>bytes</code> 和 <code>processing\_time</code> 對應至度量值 <code>in</code> 和 <code>out</code>。</li>
<li><code>cache</code> 對應至度量值 <code>bypass</code>、<code>expired</code>、<code>hit</code>、<code>miss</code>、<code>revalidated</code>、<code>scare</code>、<code>stale</code> 及 <code>updating</code>。</li>
<li><code>requests</code> 對應至度量值 <code>requestMsec</code>、<code>1xx</code>、<code>2xx</code>、<code>3xx</code>、<code>4xx</code>、<code>5xx</code> 及 <code>total</code>。</li></ul>
在上述範例中，子類型為 <code>bytes</code>。</td>
</tr>
<tr>
<td><code>&lt;SERVER-NAME&gt;</code></td>
<td>Ingress 資源中所定義的伺服器名稱。為了維持與 Prometheus 的相容性，將句點 (<code>.</code>) 取代為底線 <code>(\_)</code>。在上述範例中，伺服器名稱為 <code>dev_demostg1_stg_us_south_containers_appdomain_cloud</code>。</td>
</tr>
<tr>
<td><code>&lt;METRIC_NAME&gt;</code></td>
<td>所收集度量值類型的名稱。如需度量值名稱的清單，請參閱下表：「受支援的伺服器度量值」。在上述範例中，度量值名稱為 <code>out</code>。</td>
</tr>
<tr>
<td><code>&lt;VALUE&gt;</code></td>
<td>所收集度量值的值。在上述範例中，值為 <code>22319</code>。</td>
</tr>
</tbody></table>

下表列出受支援的伺服器度量值名稱。

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="構想圖示"/> 受支援的伺服器度量值</th>
</thead>
<tbody>
<tr>
<td><code>in</code></td>
<td>接收自用戶端的位元組總數。</td>
</tr>
<tr>
<td><code>out</code></td>
<td>傳送至用戶端的位元組總數。</td>
</tr>
<tr>
<td><code>bypass</code></td>
<td>因不符合快取中臨界值而從原點伺服器提取可快取項目的次數（例如，要求數目）。</td>
</tr>
<tr>
<td><code>expired</code></td>
<td>在快取中找到項目、但因過期而未選取的次數。</td>
</tr>
<tr>
<td><code>hit</code></td>
<td>從快取中選取有效項目的次數。</td>
</tr>
<tr>
<td><code>miss</code></td>
<td>在快取中找不到有效快取項目、且伺服器已從原點伺服器提取該項目的次數。</td>
</tr>
<tr>
<td><code>revalidated</code></td>
<td>已重新驗證快取中過期項目的次數。</td>
</tr>
<tr>
<td><code>scarce</code></td>
<td>快取已移除不常使用或低優先順序項目來釋放稀少記憶體的次數。</td>
</tr>
<tr>
<td><code>stale</code></td>
<td>在快取中找到過期項目，但因另一個要求導致伺服器從原點伺服器提取項目，而從快取中選取了該項目的次數。</td>
</tr>
<tr>
<td><code>updating</code></td>
<td>已更新過時內容的次數。</td>
</tr>
<tr>
<td><code>requestMsec</code></td>
<td>要求處理時間的平均值（以毫秒為單位）。</td>
</tr>
<tr>
<td><code>1xx</code></td>
<td>狀態碼為 1xx 的回應數目。</td>
</tr>
<tr>
<td><code>2xx</code></td>
<td>狀態碼為 2xx 的回應數目。</td>
</tr>
<tr>
<td><code>3xx</code></td>
<td>狀態碼為 3xx 的回應數目。</td>
</tr>
<tr>
<td><code>4xx</code></td>
<td>狀態碼為 4xx 的回應數目。</td>
</tr>
<tr>
<td><code>5xx</code></td>
<td>狀態碼為 5xx 的回應數目。</td>
</tr>
<tr>
<td><code>total</code></td>
<td>具有狀態碼的回應總數。</td>
  </tr></tbody>
</table>

### 上游度量值
{: #upstream_metrics}

`alb-metrics-exporter` 會自動將 JSON 檔案中的每個資料欄位都重新格式化為 Prometheus 可讀取的度量值。上游度量值收集 Ingress 資源中所定義後端服務的資料。
{: shortdesc}

以兩種方式來格式化上游度量值。
* [類型 1](#type_one) 包括上游服務名稱。
* [類型 2](#type_two) 包括上游服務名稱和特定上游 Pod IP 位址。

#### 類型 1 上游度量值
{: #type_one}

上游類型 1 度量值的格式為 `kube_system_upstream_<ALB-ID>_<SUB-TYPE>_<UPSTREAM-NAME>_<METRIC-NAME> <VALUE>`。
{: shortdesc}

`<UPSTREAM-NAME>_<METRIC-NAME>` 已格式化為標籤。例如，`albId="default-cafe-ingress-dev_demostg1_us-south_containers_appdomain_cloud-coffee-svc",metric="in"`

例如，如果上游服務從 ALB 收到共 1227 個位元組，則會將度量值格式化為：
```
kube_system_upstream_public_cra6a6eb9e897e41c4a5e58f957b417aec_alb1_bytes{albId="default-cafe-ingress-dev_demostg1_us-south_containers_appdomain_cloud-coffee-svc",app="alb-metrics-exporter",instance="172.30.140.68:9913",job="kubernetes-pods",kubernetes_namespace="kube-system",kubernetes_pod_name="alb-metrics-exporter-7d495d785c-8wfw4",metric="in",pod_template_hash="3805183417"} 1227
```
{: screen}

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解上游類型 1 度量值格式</th>
</thead>
<tbody>
<tr>
<td><code>&lt;ALB-ID&gt;</code></td>
<td>ALB ID。在上述範例中，ALB ID 為 <code>public\_crf02710f54fcc40889c301bfd6d5b77fe\_alb1</code>。</td>
</tr>
<tr>
<td><code>&lt;SUB-TYPE&gt;</code></td>
<td>度量值的子類型。受支援的值為 <code>bytes</code>、<code>processing\_time</code> 及 <code>requests</code>。在上述範例中，子類型為 <code>bytes</code>。</td>
</tr>
<tr>
<td><code>&lt;UPSTREAM-NAME&gt;</code></td>
<td>Ingress 資源中所定義的上游服務名稱。為了維持與 Prometheus 的相容性，將句點 (<code>.</code>) 取代為底線 <code>(\_)</code>。在上述範例中，上游名稱為 <code>default-cafe-ingress-dev_demostg1_us-south_containers_appdomain_cloud-coffee-svc</code>。</td>
</tr>
<tr>
<td><code>&lt;METRIC_NAME&gt;</code></td>
<td>所收集度量值類型的名稱。如需度量值名稱的清單，請參閱下表：「受支援的上游類型 1 度量值」。在上述範例中，度量值名稱為 <code>in</code>。</td>
</tr>
<tr>
<td><code>&lt;VALUE&gt;</code></td>
<td>所收集度量值的值。在上述範例中，值為 <code>1227</code>。</td>
</tr>
</tbody></table>

下表列出受支援的上游類型 1 度量值名稱。

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="構想圖示"/> 受支援的上游類型 1 度量值</th>
</thead>
<tbody>
<tr>
<td><code>in</code></td>
<td>接收自 ALB 伺服器的位元組總數。</td>
</tr>
<tr>
<td><code>out</code></td>
<td>傳送至 ALB 伺服器的位元組總數。</td>
</tr>
<tr>
<td><code>1xx</code></td>
<td>狀態碼為 1xx 的回應數目。</td>
</tr>
<tr>
<td><code>2xx</code></td>
<td>狀態碼為 2xx 的回應數目。</td>
</tr>
<tr>
<td><code>3xx</code></td>
<td>狀態碼為 3xx 的回應數目。</td>
</tr>
<tr>
<td><code>4xx</code></td>
<td>狀態碼為 4xx 的回應數目。</td>
</tr>
<tr>
<td><code>5xx</code></td>
<td>狀態碼為 5xx 的回應數目。</td>
</tr>
<tr>
<td><code>total</code></td>
<td>具有狀態碼的回應總數。</td>
  </tr></tbody>
</table>

#### 類型 2 上游度量值
{: #type_two}

上游類型 2 度量值的格式為 `kube_system_upstream_<ALB-ID>_<METRIC-NAME>_<UPSTREAM-NAME>_<POD-IP> <VALUE>`。
{: shortdesc}

`<UPSTREAM-NAME>_<POD-IP>` 已格式化為標籤。例如，`albId="default-cafe-ingress-dev_dev_demostg1_us-south_containers_appdomain_cloud-tea-svc",backend="172_30_75_6_80"`

例如，如果上游服務的平均要求處理時間（包括上游）為 40 毫秒，則會將度量值格式化為：
```
kube_system_upstream_public_cra6a6eb9e897e41c4a5e58f957b417aec_alb1_requestMsec{albId="default-cafe-ingress-dev_dev_demostg1_us-south_containers_appdomain_cloud-tea-svc",app="alb-metrics-exporter",backend="172_30_75_6_80",instance="172.30.75.3:9913",job="kubernetes-pods",kubernetes_namespace="kube-system",kubernetes_pod_name="alb-metrics-exporter-7d495d785c-swkls",pod_template_hash="3805183417"} 40
```

{: screen}

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解上游類型 2 度量值格式</th>
</thead>
<tbody>
<tr>
<td><code>&lt;ALB-ID&gt;</code></td>
<td>ALB ID。在上述範例中，ALB ID 為 <code>public\_cra6a6eb9e897e41c4a5e58f957b417aec\_alb1</code>。</td>
</tr>
<td><code>&lt;UPSTREAM-NAME&gt;</code></td>
<td>Ingress 資源中所定義的上游服務名稱。為了維持與 Prometheus 的相容性，將句點 (<code>.</code>) 取代為底線 (<code>\_</code>)。在上述範例中，上游名稱為 <code>demostg1\_stg\_us\_south\_containers\_appdomain\_cloud\_tea\_svc</code>。</td>
</tr>
<tr>
<td><code>&lt;POD\_IP&gt;</code></td>
<td>特定上游服務 Pod 的 IP 位址和埠。為了維持與 Prometheus 的相容性，將句點 (<code>.</code>) 和冒號 (<code>:</code>) 取代為底線 <code>(_)</code>。在上述範例中，上游 Pod IP 為 <code>172_30_75_6_80</code>。</td>
</tr>
<tr>
<td><code>&lt;METRIC_NAME&gt;</code></td>
<td>所收集度量值類型的名稱。如需度量值名稱的清單，請參閱下表：「受支援的上游類型 2 度量值」。在上述範例中，度量值名稱為 <code>requestMsec</code>。</td>
</tr>
<tr>
<td><code>&lt;VALUE&gt;</code></td>
<td>所收集度量值的值。在上述範例中，值為 <code>40</code>。</td>
</tr>
</tbody></table>

下表列出受支援的上游類型 2 度量值名稱。

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="構想圖示"/> 受支援的上游類型 2 度量值</th>
</thead>
<tbody>
<tr>
<td><code>requestMsec</code></td>
<td>要求處理時間的平均值（包括上游）（以毫秒為單位）。</td>
</tr>
<tr>
<td><code>responseMsec</code></td>
<td>僅限上游回應處理時間的平均值（以毫秒為單位）。</td>
  </tr></tbody>
</table>

<br />


## 增加 Ingress 度量值集合的共用記憶體區域大小
{: #vts_zone_size}

已定義共用記憶體區域，讓工作者處理程序可以共用快取、階段作業持續性及速率限制這類資訊。已設定共用記憶體區域（稱為虛擬主機資料流量狀態區域），讓 Ingress 可以收集 ALB 的度量值資料。
{:shortdesc}

在 `ibm-cloud-provider-ingress-cm` Ingress configmap 中，`vts-status-zone-size` 欄位會設定度量值資料集合的共用記憶體區域大小。依預設，`vts-status-zone-size` 設為 `10m`。如果您的大型環境需要更多記憶體來收集度量值，則可以遵循下列步驟來置換預設值，而非使用較大的值。

開始之前，請確定您具有 `kube-system` 名稱空間的[**撰寫者**或**管理員** {{site.data.keyword.Bluemix_notm}}IAM 服務角色](/docs/containers?topic=containers-users#platform)。

1. 編輯 `ibm-cloud-provider-ingress-cm` configmap 資源的配置檔。

    ```
    kubectl edit cm ibm-cloud-provider-ingress-cm -n kube-system
    ```
    {: pre}

2. 將 `vts-status-zone-size` 的值從 `10m` 變更為較大的值。

   ```
   apiVersion: v1
   data:
     vts-status-zone-size: "10m"
   kind: ConfigMap
   metadata:
     name: ibm-cloud-provider-ingress-cm
     namespace: kube-system
   ```
   {: codeblock}

3. 儲存配置檔。

4. 驗證已套用 configmap 變更。

   ```
   kubectl get cm ibm-cloud-provider-ingress-cm -n kube-system -o yaml
   ```
   {: pre}
