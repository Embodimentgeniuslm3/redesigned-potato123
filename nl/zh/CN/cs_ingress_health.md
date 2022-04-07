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


# 日志记录和监视 Ingress
{: #ingress_health}

定制日志记录和设置监视可帮助您对问题进行故障诊断，并提高 Ingress 配置的性能。
{: shortdesc}

## 查看 Ingress 日志
{: #ingress_logs}

如果要对 Ingress 进行故障诊断或监视 Ingress 活动，您可以查看 Ingress 日志。
{: shortdesc}

针对 Ingress ALB 自动收集日志。要查看 ALB 日志，请在两个选项之间进行选择。
* 在集群中[为 Ingress 服务创建日志记录配置](/docs/containers?topic=containers-health#configuring)。
* 通过 CLI 检查日志。**注**：您必须至少具有对 `kube-system` 名称空间的 [{{site.data.keyword.Bluemix_notm}} IAM **读取者**服务角色](/docs/containers?topic=containers-users#platform)。
    1. 获取 ALB 的 pod 的标识。
        ```
        kubectl get pods -n kube-system | grep alb
        ```
        {: pre}

    2. 打开该 ALB pod 的日志。
        ```
        kubectl logs <ALB_pod_ID> nginx-ingress -n kube-system
        ```
        {: pre}

</br>缺省 Ingress 日志内容采用 JSON 格式，并显示描述客户机与应用程序之间的连接会话的常用字段。包含缺省字段的示例日志如下所示：

```
{"time_date": "2018-08-21T17:33:19+00:00", "client": "108.162.248.42", "host": "albhealth.multizone.us-south.containers.appdomain.cloud", "scheme": "http", "request_method": "GET", "request_uri": "/", "request_id": "c2bcce90cf2a3853694da9f52f5b72e6", "status": 200, "upstream_addr": "192.168.1.1:80", "upstream_status": 200, "request_time": 0.005, "upstream_response_time": 0.005, "upstream_connect_time": 0.000, "upstream_header_time": 0.005}
```
{: screen}

<table>
<caption>了解采用缺省 Ingress 日志格式的字段</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解采用缺省 Ingress 日志格式的字段</th>
</thead>
<tbody>
<tr>
<td><code>"time_date": "$time_iso8601"</code></td>
<td>写入日志的本地时间，采用 ISO 8601 标准格式。</td>
</tr>
<tr>
<td><code>"client": "$remote_addr"</code></td>
<td>客户机向应用程序发送的请求包的 IP 地址。此 IP 可根据以下情况进行更改：<ul><li>对应用程序的客户机请求发送到集群时，该请求会路由到用于公开 ALB 的 LoadBalancer 服务的 pod。如果在 LoadBalancer 服务 pod 所在的工作程序节点上不存在应用程序 pod，那么负载均衡器会将该请求转发到其他工作程序节点上的应用程序 pod。请求包的源 IP 地址将更改为运行应用程序 pod 的工作程序节点的公共 IP 地址。</li><li>如果[启用源 IP 保留](/docs/containers?topic=containers-ingress#preserve_source_ip)，那么将改为记录针对应用程序的客户机请求的原始 IP 地址。</li></ul></td>
</tr>
<tr>
<td><code>"host": "$http_host"</code></td>
<td>可据以访问应用程序的主机或子域。在 Ingress 资源文件中针对 ALB 配置此子域。</td>
</tr>
<tr>
<td><code>"scheme": "$scheme"</code></td>
<td>请求的类型：<code>HTTP</code> 或 <code>HTTPS</code>。</td>
</tr>
<tr>
<td><code>"request_method": "$request_method"</code></td>
<td>针对后端应用程序的请求调用的方法，例如，<code>GET</code> 或 <code>POST</code>。</td>
</tr>
<tr>
<td><code>"request_uri": "$uri"</code></td>
<td>应用程序路径的原始请求 URI。ALB 会将应用程序侦听的路径作为前缀进行处理。在 ALB 接收到从客户机到应用程序的请求时，ALB 检查与请求 URI 中的路径相匹配的路径（作为前缀）的 Ingress 资源。</td>
</tr>
<tr>
<td><code>"request_id": "$request_id"</code></td>
<td>从 16 位随机字节生成的唯一请求标识。</td>
</tr>
<tr>
<td><code>"status": $status</code></td>
<td>连接会话的状态码。<ul>
<li><code>200</code>：会话已成功完成</li>
<li><code>400</code>：无法解析客户机数据</li>
<li><code>403</code>：禁止访问；例如，特定客户机 IP 地址的访问受限时</li>
<li><code>500</code>：内部服务器错误</li>
<li><code>502</code>：无效网关；例如，如果无法选择或访问上游服务器</li>
<li><code>503</code>：服务不可用；例如，连接数限制访问时</li>
</ul></td>
</tr>
<tr>
<td><code>"upstream_addr": "$upstream_addr"</code></td>
<td>上游服务器的 UNIX 域套接字的 IP 地址和端口或路径。如果在请求处理期间联系了多台服务器，那么用逗号分隔其地址：<code>"192.168.1.1:80, 192.168.1.2:80, unix:/tmp/sock"</code>。如果在内部将请求从一个服务器组重定向到另一个组，那么将用冒号分隔来自不同组的服务器地址：<code>"192.168.1.1:80, 192.168.1.2:80, unix:/tmp/sock : 192.168.10.1:80, 192.168.10.2:80"</code>。如果 ALB 无法选择服务器，那么将改为记录服务器组的名称。</td>
</tr>
<tr>
<td><code>"upstream_status": $upstream_status</code></td>
<td>从后端应用程序的上游服务器获取的响应的状态码，例如，标准 HTTP 响应代码。多个响应的状态码用逗号和冒号分隔，例如，<code>$upstream_addr</code> 变量中的地址。如果 ALB 无法选择服务器，那么将记录 502（无效网关）状态码。</td>
</tr>
<tr>
<td><code>"request_time": $request_time</code></td>
<td>请求处理时间，以秒为单位，精确到毫秒。此时间在 ALB 读取客户机请求的前几个字节时开始，在 ALB 将响应的最后几个字节发送到客户机时停止。在请求处理时间停止后立即写入日志。</td>
</tr>
<tr>
<td><code>"upstream_response_time": $upstream_response_time</code></td>
<td>ALB 接收来自后端应用程序的上游服务器的响应所用时间，以秒为单位，精确到毫秒。多个响应的时间用逗号和冒号分隔，例如，<code>$upstream_addr</code> 变量中的地址。</td>
</tr>
<tr>
<td><code>"upstream_connect_time": $upstream_connect_time</code></td>
<td>ALB 与后端应用程序的上游服务器建立连接所用时间，以秒为单位，精确到毫秒。如果在 Ingress 资源配置中启用 TLS/SSL，那么此时间包含在握手上花费的时间。多个连接的时间用逗号和冒号分隔，例如，<code>$upstream_addr</code> 变量中的地址。</td>
</tr>
<tr>
<td><code>"upstream_header_time": $upstream_header_time</code></td>
<td>ALB 接收来自后端应用程序的上游服务器的响应头所用时间，以秒为单位，精确到毫秒。多个连接的时间用逗号和冒号分隔，例如，<code>$upstream_addr</code> 变量中的地址。</td>
</tr>
</tbody></table>

## 定制 Ingress 日志内容和格式
{: #ingress_log_format}

可以定制为 Ingress ALB 收集的日志的内容和格式。
{:shortdesc}

缺省情况下，Ingress 日志设置为 JSON 格式并显示公共日志字段。但是，您还可以通过选择转发哪些日志组成部分，以及这些组成部分在日志输出中如何排列，从而创建定制日志格式。

开始之前，请确保您具有对 `kube-system` 名称空间的 [{{site.data.keyword.Bluemix_notm}} IAM **写入者**或**管理者**服务角色](/docs/containers?topic=containers-users#platform)。

1. 编辑配置文件中的 `ibm-cloud-provider-ingress-cm` 配置映射资源。

    ```
    kubectl edit cm ibm-cloud-provider-ingress-cm -n kube-system
    ```
    {: pre}

2. 添加 <code>data</code> 部分。添加 `log-format` 字段和（可选）`log-format-escape-json` 字段。

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
    <caption>YAML 文件的组成部分</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解 log-format 配置</th>
    </thead>
    <tbody>
    <tr>
    <td><code>log-format</code></td>
    <td>将 <code>&lt;key&gt;</code> 替换为日志组成部分的名称，将 <code>&lt;log_variable&gt;</code> 替换为要在日志条目中收集的日志组成部分的变量。可以包含您希望日志条目包含的文本和标点符号，例如用于将字符串值括起的引号和用于分隔各日志组成部分的逗号。例如，设置组成部分（如 <code>request: "$request"</code>）的格式会在日志条目中生成以下内容：<code>request: "GET /HTTP/1.1"</code>。有关可以使用的所有变量的列表，请参阅 <a href="http://nginx.org/en/docs/varindex.html">NGINX 变量索引</a>。<br><br>要记录其他头，例如 <em>x-custom-ID</em>，请将以下键/值对添加到定制日志内容：<br><pre class="codeblock"><code>customID: $http_x_custom_id</code></pre> <br>连字符 (<code>-</code>) 会转换为下划线 (<code>_</code>)，并且必须将 <code>$http_</code> 附加到定制头名称的开头。</td>
    </tr>
    <tr>
    <td><code>log-format-escape-json</code></td>
    <td>可选：缺省情况下，生成的日志为文本格式。要生成 JSON 格式的日志，请添加 <code>log-format-escape-json</code> 字段并使用值 <code>true</code>。</td>
    </tr>
    </tbody></table>

    例如，日志格式可能包含以下变量：
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

    符合此格式的日志条目类似于以下示例：
    ```
    remote_address: 127.0.0.1, remote_user: "dbmanager", time_date: [30/Mar/2018:18:52:17 +0000], request: "GET / HTTP/1.1", status: 401, http_referer: "-", http_user_agent: "Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:47.0) Gecko/20100101 Firefox/47.0", request_id: a02b2dea9cf06344a25611c1d7ad72db
    ```
    {: screen}

    要创建基于 ALB 日志缺省格式的定制日志格式，请根据需要修改以下部分，并将其添加到配置映射中：
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

4. 保存配置文件。

5. 验证是否已应用配置映射更改。

   ```
   kubectl get cm ibm-cloud-provider-ingress-cm -n kube-system -o yaml
   ```
   {: pre}

4. 要查看 Ingress ALB 日志，请在以下两个选项之间进行选择。
    * 在集群中[为 Ingress 服务创建日志记录配置](/docs/containers?topic=containers-health#logging)。
    * 通过 CLI 检查日志。
        1. 获取 ALB 的 pod 的标识。
            ```
            kubectl get pods -n kube-system | grep alb
            ```
            {: pre}

        2. 打开该 ALB pod 的日志。验证日志是否遵循更新的格式。
            ```
            kubectl logs <ALB_pod_ID> nginx-ingress -n kube-system
            ```
            {: pre}

<br />


## 监视 Ingress ALB
{: #ingress_monitoring}

通过将度量值导出器和 Prometheus 代理程序部署到集群中来监视 ALB。
{: shortdesc}

ALB 度量值导出器使用 NGINX 伪指令 `vhost_traffic_status_zone` 从每个 Ingress ALB pod 上的 `/status/format/json` 端点收集度量数据。度量值导出器会自动将 JSON 文件中的每个数据字段的格式重新设置为 Prometheus 可读的度量值。然后，Prometheus 代理程序会选取导出器生成的度量值，并使度量值在 Prometheus 仪表板上显示。

### 安装度量值导出器 Helm chart
{: #metrics-exporter}

安装度量值导出器 Helm chart 来监视集群中的 ALB。
{: shortdesc}

ALB 度量值导出器 pod 必须部署到部署了 ALB 的工作程序节点。如果 ALB 在边缘工作程序节点上运行，并且这些边缘节点有污点，会阻止其他工作负载部署，那么无法安排度量值导出器 pod。必须通过运行 `kubectl taint node <node_name> dedicated:NoSchedule- dedicated:NoExecute-` 来除去污点。
{: note}

1.  **重要信息**：[遵循指示信息](/docs/containers?topic=containers-helm#public_helm_install)在本地计算机上安装 Helm 客户机，使用服务帐户安装 Helm 服务器 (Tiller)，然后添加 {{site.data.keyword.Bluemix_notm}} Helm 存储库。

2. 将 `ibmcloud-alb-metrics-exporter` Helm chart 安装到集群。此 Helm chart 会部署 ALB 度量值导出器，并在 `kube-system` 名称空间中创建 `alb-metrics-service-account` 服务帐户。请将 <alb-ID> 替换为要收集其度量值的 ALB 的标识。要查看集群中 ALB 的标识，请运行 <code>ibmcloud ks albs --cluster &lt;cluster_name&gt;</code>。必须为要监视的每个 ALB 部署一个 chart。
  {: note}
  ```
  helm install iks-charts/ibmcloud-alb-metrics-exporter --name ibmcloud-alb-metrics-exporter --set metricsNameSpace=kube-system --set albId=<alb-ID>
  ```
  {: pre}

3. 检查 chart 部署状态。当 chart 就绪时，输出顶部附近的 **STATUS** 字段的值为 `DEPLOYED`。
  ```
    helm status ibmcloud-alb-metrics-exporter
    ```
  {: pre}

4. 验证 `ibmcloud-alb-metrics-exporter` pod 是否在运行。
  ```
    kubectl get pods -n kube-system -o wide
    ```
  {:pre}

  输出示例：
  ```
  NAME                                             READY     STATUS      RESTARTS   AGE       IP               NODE
  ...
  alb-metrics-exporter-868fddf777-d49l5            1/1       Running     0          19s       172.30.xxx.xxx   10.xxx.xx.xxx
  alb-metrics-exporter-868fddf777-pf7x5            1/1       Running     0          19s       172.30.xxx.xxx   10.xxx.xx.xxx
  ```
  {:screen}

5. 可选：[安装 Prometheus 代理程序](#prometheus-agent)以选取导出器生成的度量值，并使度量值在 Prometheus 仪表板上显示。

### 安装 Prometheus 代理程序 Helm chart
{: #prometheus-agent}

安装[度量值导出器](#metrics-exporter)后，可以安装 Prometheus 代理程序 Helm chart 以选取导出器生成的度量值，并使度量值在 Prometheus 仪表板上显示。
{: shortdesc}

1. 下载度量值导出器 Helm chart 的 TAR 文件：https://icr.io/helm/iks-charts/charts/ibmcloud-alb-metrics-exporter-1.0.7.tgz

2. 导航至 Prometheus 子文件夹。
  ```
  cd ibmcloud-alb-metrics-exporter-1.0.7.tar/ibmcloud-alb-metrics-exporter/subcharts/prometheus
  ```
  {: pre}

3. 将 Prometheus Helm chart 安装到集群。请将 <ingress_subdomain> 替换为集群的 Ingress 子域。Prometheus 仪表板的 URL 是缺省 Prometheus 子域 `prom-dash` 和 Ingress 子域的组合，例如 `prom-dash.mycluster-12345.us-south.containers.appdomain.cloud`。要查找集群的 Ingress 子域，请运行 <code>ibmcloud ks cluster-get --cluster &lt;cluster_name&gt;</code>。
  ```
  helm install --name prometheus . --set nameSpace=kube-system --set hostName=prom-dash.<ingress_subdomain>
  ```
  {: pre}

4. 检查 chart 部署状态。当 chart 就绪时，输出顶部附近的 **STATUS** 字段的值为 `DEPLOYED`。
    ```
    helm status prometheus
    ```
    {: pre}

5. 验证 `prometheus` pod 是否在运行。
    ```
    kubectl get pods -n kube-system -o wide
    ```
    {:pre}

    输出示例：
    ```
    NAME                                             READY     STATUS      RESTARTS   AGE       IP               NODE
    alb-metrics-exporter-868fddf777-d49l5            1/1       Running     0          19s       172.30.xxx.xxx   10.xxx.xx.xxx
    alb-metrics-exporter-868fddf777-pf7x5            1/1       Running     0          19s       172.30.xxx.xxx   10.xxx.xx.xxx
    prometheus-9fbcc8bc7-2wvbk                       1/1       Running     0          1m        172.30.xxx.xxx   10.xxx.xx.xxx
    ```
    {:screen}

6. 在浏览器中，输入 Prometheus 仪表板的 URL。此主机名的格式为 `prom-dash.mycluster-12345.us-south.containers.appdomain.cloud`。这将打开 ALB 的 Prometheus 仪表板。

7. 查看有关仪表板中列出的 [ALB](#alb_metrics)、[服务器](#server_metrics)和[上游](#upstream_metrics)度量值的更多信息。

### ALB 度量值
{: #alb_metrics}

`alb-metrics-exporter` 会自动将 JSON 文件中的每个数据字段的格式重新设置为 Prometheus 可读的度量值。ALB 度量值将收集有关 ALB 正在处理的连接和响应的数据。
{: shortdesc}

ALB 度量值的格式为 `kube_system_<ALB-ID>_<METRIC-NAME><VALUE>`。例如，如果 ALB 收到 23 个具有 2xx 级别状态码的响应，那么度量值的格式会设置为 `kube_system_public_crf02710f54fcc40889c301bfd6d5b77fe_alb1_totalHandledRequest {.. metric="2xx"} 23`，其中 `metric` 是 Prometheus 标签。

下表列出了支持的 ALB 度量值名称（带有度量值标签），格式为 `<ALB_metric_name>_<metric_label>`
<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 支持的 ALB 度量值</th>
</thead>
<tbody>
<tr>
<td><code>connections_reading</code></td>
<td>正在读取的客户机连接总数。</td>
</tr>
<tr>
<td><code>connections_accepted</code></td>
<td>接受的客户机连接总数。</td>
</tr>
<tr>
<td><code>connections_active</code></td>
<td>活动客户机连接总数。</td>
</tr>
<tr>
<td><code>connections_handled</code></td>
<td>处理的客户机连接总数。</td>
</tr>
<tr>
<td><code>connections_requests</code></td>
<td>请求的客户机连接总数。</td>
</tr>
<tr>
<td><code>connections_waiting</code></td>
<td>正在等待的客户机连接总数。</td>
</tr>
<tr>
<td><code>connections_writing</code></td>
<td>正在写入的客户机连接总数。</td>
</tr>
<tr>
<td><code>totalHandledRequest_1xx</code></td>
<td>具有状态码 1xx 的响应数。</td>
</tr>
<tr>
<td><code>totalHandledRequest_2xx</code></td>
<td>具有状态码 2xx 的响应数。</td>
</tr>
<tr>
<td><code>totalHandledRequest_3xx</code></td>
<td>具有状态码 3xx 的响应数。</td>
</tr>
<tr>
<td><code>totalHandledRequest_4xx</code></td>
<td>具有状态码 4xx 的响应数。</td>
</tr>
<tr>
<td><code>totalHandledRequest_5xx</code></td>
<td>具有状态码 5xx 的响应数。</td>
</tr>
<tr>
<td><code>totalHandledRequest_total</code></td>
<td>从客户机收到的客户机请求总数。</td>
  </tr></tbody>
</table>

### 服务器度量值
{: #server_metrics}

`alb-metrics-exporter` 会自动将 JSON 文件中的每个数据字段的格式重新设置为 Prometheus 可读的度量值。服务器度量值将收集 Ingress 资源中定义的子域上的数据；例如，`dev.demostg1.stg.us.south.containers.appdomain.cloud`。
{: shortdesc}

服务器度量值的格式为 `kube_system_server_<ALB-ID>_<SUB-TYPE>_<SERVER-NAME>_<METRIC-NAME> <VALUE>`。

`<SERVER-NAME>_<METRIC-NAME>` 的格式设置为标签。例如，`albId="dev_demostg1_us-south_containers_appdomain_cloud",metric="out"`

例如，如果服务器向客户机发送了总计 22319 字节，那么度量值的格式设置为：
```
kube_system_server_public_cra6a6eb9e897e41c4a5e58f957b417aec_alb1_bytes{albId="dev_demostg1_us-south_containers_appdomain_cloud",app="alb-metrics-exporter",instance="172.30.140.68:9913",job="kubernetes-pods",kubernetes_namespace="kube-system",kubernetes_pod_name="alb-metrics-exporter-7d495d785c-8wfw4",metric="out",pod_template_hash="3805183417"} 22319
```
{: screen}

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解服务器度量值格式</th>
</thead>
<tbody>
<tr>
<td><code>&lt;ALB-ID&gt;</code></td>
<td>ALB 标识。在以上示例中，ALB 标识为 <code>public\_cra6a6eb9e897e41c4a5e58f957b417aec\_alb1</code>。</td>
</tr>
<tr>
<td><code>&lt;SUB-TYPE&gt;</code></td>
<td>度量值的子类型。每个子类型对应于一个或多个度量值名称。<ul>
<li><code>bytes</code> 和 <code>processing\_time</code> 对应于度量值 <code>in</code> 和 <code>out</code>。</li>
<li><code>cache</code> 对应于度量值 <code>bypass</code>、<code>expired</code>、<code>hit</code>、<code>miss</code>、<code>revalidated</code>、<code>scare</code>、<code>stale</code> 和 <code>updating</code>。</li>
<li><code>requests</code> 对应于度量值 <code>requestMsec</code>。<code>1xx</code>、<code>2xx</code>、<code>3xx</code>、<code>4xx</code>、<code>5xx</code> 和 <code>total</code>。</li></ul>
在以上示例中，子类型为 <code>bytes</code>。</td>
</tr>
<tr>
<td><code>&lt;SERVER-NAME&gt;</code></td>
<td>在 Ingress 资源中定义的服务器的名称。为了保持与 Prometheus 的兼容性，句点 (<code>.</code>) 会替换为下划线 <code>(\_)</code>。在以上示例中，服务器名称为 <code>dev_demostg1_stg_us_south_containers_appdomain_cloud</code>。</td>
</tr>
<tr>
<td><code>&lt;METRIC_NAME&gt;</code></td>
<td>所收集度量值类型的名称。有关度量值名称的列表，请参阅下表“支持的服务器度量值”。在以上示例中，度量值名称为 <code>out</code>。</td>
</tr>
<tr>
<td><code>&lt;VALUE&gt;</code></td>
<td>所收集度量值的值。在以上示例中，值为 <code>22319</code>。</td>
</tr>
</tbody></table>

下表列出了支持的服务器度量值名称。

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 支持的服务器度量值</th>
</thead>
<tbody>
<tr>
<td><code>in</code></td>
<td>从客户机收到的字节总数。</td>
</tr>
<tr>
<td><code>out</code></td>
<td>发送到客户机的字节总数。</td>
</tr>
<tr>
<td><code>bypass</code></td>
<td>从源服务器访存因不满足阈值而无法处于高速缓存中的可高速缓存项的次数（例如，请求数）。</td>
</tr>
<tr>
<td><code>expired</code></td>
<td>在高速缓存中找到项，但由于已到期而未选择该项的次数。</td>
</tr>
<tr>
<td><code>hit</code></td>
<td>从高速缓存中选择有效项的次数。</td>
</tr>
<tr>
<td><code>miss</code></td>
<td>在高速缓存中找不到有效的高速缓存项，但服务器从源服务器访存到该项的次数。</td>
</tr>
<tr>
<td><code>revalidated</code></td>
<td>重新验证高速缓存中到期项的次数。</td>
</tr>
<tr>
<td><code>scarce</code></td>
<td>高速缓存除去很少使用或低优先级的项以释放稀有内存的次数。</td>
</tr>
<tr>
<td><code>stale</code></td>
<td>在高速缓存中找到已到期项，但由于其他请求导致服务器从源服务器访存该项，因此已从高速缓存中选择该项的次数。</td>
</tr>
<tr>
<td><code>更新</code></td>
<td>更新旧内容的次数。</td>
</tr>
<tr>
<td><code>requestMsec</code></td>
<td>平均请求处理时间，以毫秒为单位。</td>
</tr>
<tr>
<td><code>1xx</code></td>
<td>具有状态码 1xx 的响应数。</td>
</tr>
<tr>
<td><code>2xx</code></td>
<td>具有状态码 2xx 的响应数。</td>
</tr>
<tr>
<td><code>3xx</code></td>
<td>具有状态码 3xx 的响应数。</td>
</tr>
<tr>
<td><code>4xx</code></td>
<td>具有状态码 4xx 的响应数。</td>
</tr>
<tr>
<td><code>5xx</code></td>
<td>具有状态码 5xx 的响应数。</td>
</tr>
<tr>
<td><code>total</code></td>
<td>具有状态码的响应总数。</td>
  </tr></tbody>
</table>

### 上游度量值
{: #upstream_metrics}

`alb-metrics-exporter` 会自动将 JSON 文件中的每个数据字段的格式重新设置为 Prometheus 可读的度量值。上游度量值将收集有关 Ingress 资源中定义的后端服务的数据。
{: shortdesc}

上游度量值的格式以两种方式进行设置。
* [类型 1](#type_one) 包含上游服务名称。
* [类型 2](#type_two) 包含上游服务名称和特定上游 pod IP 地址。

#### 类型 1 上游度量值
{: #type_one}

上游类型 1 度量值的格式为 `kube_system_upstream_<ALB-ID>_<SUB-TYPE>_<UPSTREAM-NAME>_<METRIC-NAME> <VALUE>`。
{: shortdesc}

`<UPSTREAM-NAME>_<METRIC-NAME>` 的格式设置为标签。例如，`albId="default-cafe-ingress-dev_demostg1_us-south_containers_appdomain_cloud-coffee-svc",metric="in"`

例如，如果上游服务从 ALB 收到了总计 1227 字节，那么度量值的格式设置为：
```
kube_system_upstream_public_cra6a6eb9e897e41c4a5e58f957b417aec_alb1_bytes{albId="default-cafe-ingress-dev_demostg1_us-south_containers_appdomain_cloud-coffee-svc",app="alb-metrics-exporter",instance="172.30.140.68:9913",job="kubernetes-pods",kubernetes_namespace="kube-system",kubernetes_pod_name="alb-metrics-exporter-7d495d785c-8wfw4",metric="in",pod_template_hash="3805183417"} 1227
```
{: screen}

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解上游类型 1 度量值格式</th>
</thead>
<tbody>
<tr>
<td><code>&lt;ALB-ID&gt;</code></td>
<td>ALB 标识。在以上示例中，ALB 标识为 <code>public\_crf02710f54fcc40889c301bfd6d5b77fe\_alb1</code>。</td>
</tr>
<tr>
<td><code>&lt;SUB-TYPE&gt;</code></td>
<td>度量值的子类型。支持的值为 <code>bytes</code>、<code>processing\_time</code> 和 <code>requests</code>。在以上示例中，子类型为 <code>bytes</code>。</td>
</tr>
<tr>
<td><code>&lt;UPSTREAM-NAME&gt;</code></td>
<td>在 Ingress 资源中定义的上游服务的名称。为了保持与 Prometheus 的兼容性，句点 (<code>.</code>) 会替换为下划线 <code>(\_)</code>。在以上示例中，上游名称为 <code>default-cafe-ingress-dev_demostg1_us-south_containers_appdomain_cloud-coffee-svc</code>。</td>
</tr>
<tr>
<td><code>&lt;METRIC_NAME&gt;</code></td>
<td>所收集度量值类型的名称。有关度量值名称的列表，请参阅下表“支持的上游类型 1 度量值”。在以上示例中，度量值名称为 <code>in</code>。</td>
</tr>
<tr>
<td><code>&lt;VALUE&gt;</code></td>
<td>所收集度量值的值。在以上示例中，值为 <code>1227</code>。</td>
</tr>
</tbody></table>

下表列出了支持的上游类型 1 度量值名称。

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 支持的上游类型 1 度量值</th>
</thead>
<tbody>
<tr>
<td><code>in</code></td>
<td>从 ALB 服务器收到的字节总数。</td>
</tr>
<tr>
<td><code>out</code></td>
<td>发送到 ALB 服务器的字节总数。</td>
</tr>
<tr>
<td><code>1xx</code></td>
<td>具有状态码 1xx 的响应数。</td>
</tr>
<tr>
<td><code>2xx</code></td>
<td>具有状态码 2xx 的响应数。</td>
</tr>
<tr>
<td><code>3xx</code></td>
<td>具有状态码 3xx 的响应数。</td>
</tr>
<tr>
<td><code>4xx</code></td>
<td>具有状态码 4xx 的响应数。</td>
</tr>
<tr>
<td><code>5xx</code></td>
<td>具有状态码 5xx 的响应数。</td>
</tr>
<tr>
<td><code>total</code></td>
<td>具有状态码的响应总数。</td>
  </tr></tbody>
</table>

#### 类型 2 上游度量值
{: #type_two}

上游类型 2 度量值的格式为 `kube_system_upstream_<ALB-ID>_<METRIC-NAME>_<UPSTREAM-NAME>_<POD-IP> <VALUE>`。
{: shortdesc}

`<UPSTREAM-NAME>_<POD-IP>` 的格式设置为标签。例如，`albId="default-cafe-ingress-dev_dev_demostg1_us-south_containers_appdomain_cloud-tea-svc",backend="172_30_75_6_80"`

例如，如果上游服务的平均请求处理时间（包括上游）为 40 毫秒，那么度量值的格式设置为：
```
kube_system_upstream_public_cra6a6eb9e897e41c4a5e58f957b417aec_alb1_requestMsec{albId="default-cafe-ingress-dev_dev_demostg1_us-south_containers_appdomain_cloud-tea-svc",app="alb-metrics-exporter",backend="172_30_75_6_80",instance="172.30.75.3:9913",job="kubernetes-pods",kubernetes_namespace="kube-system",kubernetes_pod_name="alb-metrics-exporter-7d495d785c-swkls",pod_template_hash="3805183417"} 40
```

{: screen}

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解上游类型 2 度量值格式</th>
</thead>
<tbody>
<tr>
<td><code>&lt;ALB-ID&gt;</code></td>
<td>ALB 标识。在以上示例中，ALB 标识为 <code>public\_cra6a6eb9e897e41c4a5e58f957b417aec\_alb1</code>。</td>
</tr>
<td><code>&lt;UPSTREAM-NAME&gt;</code></td>
<td>在 Ingress 资源中定义的上游服务的名称。为了保持与 Prometheus 的兼容性，句点 (<code>.</code>) 会替换为下划线 (<code>\_</code>)。在以上示例中，上游名称为 <code>demostg1\_stg\_us\_south\_containers\_appdomain\_cloud\_tea\_svc</code>。</td>
</tr>
<tr>
<td><code>&lt;POD\_IP&gt;</code></td>
<td>特定上游服务 pod 的 IP 地址和端口。为了保持与 Prometheus 的兼容性，句点 (<code>.</code>) 和冒号 (<code>:</code>) 会替换为下划线 <code>(_)</code>。在以上示例中，上游 pod IP 为 <code>172_30_75_6_80</code>。</td>
</tr>
<tr>
<td><code>&lt;METRIC_NAME&gt;</code></td>
<td>所收集度量值类型的名称。有关度量值名称的列表，请参阅下表“支持的上游类型 2 度量值”。在以上示例中，度量值名称为 <code>requestMsec</code>。</td>
</tr>
<tr>
<td><code>&lt;VALUE&gt;</code></td>
<td>所收集度量值的值。在以上示例中，值为 <code>40</code>。</td>
</tr>
</tbody></table>

下表列出了支持的上游类型 2 度量值名称。

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 支持的上游类型 2 度量值</th>
</thead>
<tbody>
<tr>
<td><code>requestMsec</code></td>
<td>平均请求处理时间（包括上游），以毫秒为单位。</td>
</tr>
<tr>
<td><code>responseMsec</code></td>
<td>仅上游响应平均处理时间，以毫秒为单位。</td>
  </tr></tbody>
</table>

<br />


## 增大用于 Ingress 度量值收集的共享内存专区大小
{: #vts_zone_size}

定义了共享内存专区，以便工作程序进程可以共享高速缓存、会话持久性和速率限制等信息。设置了共享内存专区（称为虚拟主机流量状态专区），以供 Ingress 收集 ALB 的度量值数据。
{:shortdesc}

在 `ibm-cloud-provider-ingress-cm` Ingress 配置映射中，`vts-status-zone-size` 字段设置用于度量值数据收集的共享内存专区的大小。缺省情况下，`vts-status-zone-size` 设置为 `10m`。如果大型环境需要更多内存用于度量值收集，那么可以通过执行以下步骤来覆盖缺省值，而改为使用更大的值。

开始之前，请确保您具有对 `kube-system` 名称空间的 [{{site.data.keyword.Bluemix_notm}} IAM **写入者**或**管理者**服务角色](/docs/containers?topic=containers-users#platform)。

1. 编辑配置文件中的 `ibm-cloud-provider-ingress-cm` 配置映射资源。

    ```
    kubectl edit cm ibm-cloud-provider-ingress-cm -n kube-system
    ```
    {: pre}

2. 将 `vts-status-zone-size` 的值从 `10m` 更改为更大的值。

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

3. 保存配置文件。

4. 验证是否已应用配置映射更改。

   ```
   kubectl get cm ibm-cloud-provider-ingress-cm -n kube-system -o yaml
   ```
   {: pre}
