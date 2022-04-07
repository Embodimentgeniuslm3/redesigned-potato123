---

copyright:
  years: 2014, 2019
lastupdated: "2019-05-31"

keywords: kubernetes, iks, ingress

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



# 使用注释定制 Ingress
{: #ingress_annotation}

要向 Ingress 应用程序负载均衡器 (ALB) 添加功能，您可以将注释指定为 Ingress 资源中的元数据。
{: shortdesc}

在使用注释之前，请确保通过执行[使用 Ingress 应用程序负载均衡器 (ALB) 进行 HTTPS 负载均衡](/docs/containers?topic=containers-ingress)中的步骤来正确设置 Ingress 服务配置。使用基本配置设置 Ingress ALB 后，可以通过向 Ingress 资源文件添加注释来扩展其功能。
{: note}

<table>
<caption>一般注释</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>一般注释</th>
<th>名称</th>
<th>描述</th>
</thead>
<tbody>
<tr>
<td><a href="#custom-errors">定制错误操作</a></td>
<td><code>custom-errors 和 custom-error-actions</code></td>
<td>指示 ALB 可以对特定 HTTP 错误执行的定制操作。</td>
</tr>
<tr>
<td><a href="#location-snippets">位置片段</a></td>
<td><code>location-snippets</code></td>
<td>为服务添加定制位置块配置。</td>
</tr>
<tr>
<td><a href="#alb-id">专用 ALB 路由</a></td>
<td><code>ALB-ID</code></td>
<td>使用专用 ALB 将入局请求路由到应用程序。</td>
</tr>
<tr>
<td><a href="#server-snippets">服务器片段</a></td>
<td><code>server-snippets</code></td>
<td>添加定制服务器块配置。</td>
</tr>
</tbody></table>

<br>

<table>
<caption>连接注释</caption>
<col width="20%">
<col width="20%">
<col width="60%">
 <thead>
 <th>连接注释</th>
 <th>名称</th>
 <th>描述</th>
  </thead>
  <tbody>
  <tr>
  <td><a href="#proxy-connect-timeout">定制连接超时和读取超时</a></td>
  <td><code>proxy-connect-timeout，proxy-read-timeout</code></td>
  <td>设置 ALB 等待连接到后端应用程序以及从后端应用程序进行读取的时间，超过该时间后，后端应用程序将视为不可用。</td>
  </tr>
  <tr>
  <td><a href="#keepalive-requests">保持活动请求数</a></td>
  <td><code>keepalive-requests</code></td>
  <td>设置可通过一个保持活动连接处理的最大请求数。</td>
  </tr>
  <tr>
  <td><a href="#keepalive-timeout">保持活动超时</a></td>
  <td><code>keepalive-timeout</code></td>
  <td>设置保持活动连接在服务器上保持打开状态的最长时间。</td>
  </tr>
  <tr>
  <td><a href="#proxy-next-upstream-config">作为代理传递到下一个上游</a></td>
  <td><code>proxy-next-upstream-config</code></td>
  <td>设置 ALB 何时可以向下一个上游服务器传递请求。</td>
  </tr>
  <tr>
  <td><a href="#sticky-cookie-services">使用 cookie 确保会话亲缘关系</a></td>
  <td><code>sticky-cookie-services</code></td>
  <td>使用粘性 cookie 始终将入局网络流量路由到同一个上游服务器。</td>
  </tr>
  <tr>
  <td><a href="#upstream-fail-timeout">上游失败超时</a></td>
  <td><code>upstream-fail-timeout</code></td>
  <td>设置在服务器被视为不可用之前，ALB 可以尝试连接到服务器的时间量。</td>
  </tr>
  <tr>
  <td><a href="#upstream-keepalive">上游保持活动连接数</a></td>
  <td><code>upstream-keepalive</code></td>
  <td>设置上游服务器的最大空闲保持活动连接数。</td>
  </tr>
  <tr>
  <td><a href="#upstream-max-fails">上游最大失败次数</a></td>
  <td><code>upstream-max-fails</code></td>
  <td>设置在服务器被视为不可用之前，尝试与服务器通信失败的最大次数。</td>
  </tr>
  </tbody></table>

<br>

  <table>
  <caption>HTTPS 和 TLS/SSL 认证注释</caption>
  <col width="20%">
  <col width="20%">
  <col width="60%">
  <thead>
  <th>HTTPS 和 TLS/SSL 认证注释</th>
  <th>名称</th>
  <th>描述</th>
  </thead>
  <tbody>
  <tr>
  <td><a href="#custom-port">定制 HTTP 和 HTTPS 端口</a></td>
  <td><code>custom-port</code></td>
  <td>更改 HTTP（端口 80）和 HTTPS（端口 443）网络流量的缺省端口。</td>
  </tr>
  <tr>
  <td><a href="#redirect-to-https">HTTP 重定向到 HTTPS</a></td>
  <td><code>redirect-to-https</code></td>
  <td>将域上的不安全的 HTTP 请求重定向到 HTTPS。</td>
  </tr>
  <tr>
  <td><a href="#hsts">HTTP 严格传输安全性 (HSTS)</a></td>
  <td><code>hsts</code></td>
  <td>将浏览器设置为仅使用 HTTPS 访问域。</td>
  </tr>
  <tr>
  <td><a href="#mutual-auth">相互认证</a></td>
  <td><code>mutual-auth</code></td>
  <td>为 ALB 配置相互认证。</td>
  </tr>
  <tr>
  <td><a href="#ssl-services">SSL 服务支持</a></td>
  <td><code>ssl-services</code></td>
  <td>允许 SSL 服务支持加密流至需要 HTTPS 的上游应用程序的流量。</td>
  </tr>
  <tr>
  <td><a href="#tcp-ports">TCP 端口</a></td>
  <td><code>tcp-ports</code></td>
  <td>通过非标准 TCP 端口访问应用程序。</td>
  </tr>
  </tbody></table>

<br>

<table>
<caption>路径路由注释</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>路径路由注释</th>
<th>名称</th>
<th>描述</th>
</thead>
<tbody>
<tr>
<td><a href="#proxy-external-service">外部服务</a></td>
<td><code>proxy-external-service</code></td>
<td>添加外部服务（例如，{{site.data.keyword.Bluemix_notm}} 中托管的服务）的路径定义。</td>
</tr>
<tr>
<td><a href="#location-modifier">位置修饰符</a></td>
<td><code>location-modifier</code></td>
<td>修改 ALB 将请求 URI 与应用程序路径相匹配的方式。</td>
</tr>
<tr>
<td><a href="#rewrite-path">重写路径</a></td>
<td><code>rewrite-path</code></td>
<td>将入局网络流量路由到后端应用程序侦听的其他路径。</td>
</tr>
</tbody></table>

<br>

<table>
<caption>代理缓冲区注释</caption>
<col width="20%">
<col width="20%">
<col width="60%">
 <thead>
 <th>代理缓冲区注释</th>
 <th>名称</th>
 <th>描述</th>
 </thead>
 <tbody>
 <tr>
<td><a href="#large-client-header-buffers">大型客户机头缓冲区</a></td>
<td><code>large-client-header-buffers</code></td>
<td>设置读取大型客户机请求头的缓冲区的最大数目和大小。</td>
</tr>
 <tr>
 <td><a href="#proxy-buffering">客户机响应数据缓冲</a></td>
 <td><code>proxy-buffering</code></td>
 <td>禁止在向客户机发送响应时，在 ALB 上对客户机响应进行缓冲。</td>
 </tr>
 <tr>
 <td><a href="#proxy-buffers">代理缓冲区数</a></td>
 <td><code>proxy-buffers</code></td>
 <td>设置缓冲区的数目和大小，这些缓冲区用于读取来自通过代理传递的服务器的单个连接的响应。</td>
 </tr>
 <tr>
 <td><a href="#proxy-buffer-size">代理缓冲区大小</a></td>
 <td><code>proxy-buffer-size</code></td>
 <td>设置缓冲区的大小，该缓冲区用于读取从通过代理传递的服务器收到的响应的第一部分。</td>
 </tr>
 <tr>
 <td><a href="#proxy-busy-buffers-size">代理繁忙缓冲区大小</a></td>
 <td><code>proxy-busy-buffers-size</code></td>
 <td>设置可以处于繁忙状态的代理缓冲区的大小。</td>
 </tr>
 </tbody></table>

<br>

<table>
<caption>请求和响应注释</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>请求和响应注释</th>
<th>名称</th>
<th>描述</th>
</thead>
<tbody>
<tr>
<td><a href="#add-host-port">将服务器端口添加到主机头</a></td>
<td><code>add-host-port</code></td>
<td>将服务器端口添加到主机以路由请求。</td>
</tr>
<tr>
<td><a href="#client-max-body-size">客户机请求主体大小</a></td>
<td><code>client-max-body-size</code></td>
<td>设置客户机可以作为请求一部分发送的主体的最大大小。</td>
</tr>
<tr>
<td><a href="#proxy-add-headers">额外的客户机请求或响应头</a></td>
<td><code>proxy-add-headers，response-add-headers</code></td>
<td>向客户机请求添加头信息，然后将该请求转发到后端应用程序，或者向客户机响应添加头信息，然后将该响应发送到客户机。</td>
</tr>
<tr>
<td><a href="#response-remove-headers">除去客户机响应头</a></td>
<td><code>response-remove-headers</code></td>
<td>从客户机响应中除去头信息，然后将该响应转发到客户机。</td>
</tr>
</tbody></table>

<br>

<table>
<caption>服务限制注释</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>服务限制注释</th>
<th>名称</th>
<th>描述</th>
</thead>
<tbody>
<tr>
<td><a href="#global-rate-limit">全局速率限制</a></td>
<td><code>global-rate-limit</code></td>
<td>对于所有服务，按定义的键限制请求处理速率和连接数。</td>
</tr>
<tr>
<td><a href="#service-rate-limit">服务速率限制</a></td>
<td><code>service-rate-limit</code></td>
<td>对于特定服务，按定义的键限制请求处理速率和连接数。</td>
</tr>
</tbody></table>

<br>

<table>
<caption>用户认证注释</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>用户认证注释</th>
<th>名称</th>
<th>描述</th>
</thead>
<tbody>
<tr>
<td><a href="#appid-auth">{{site.data.keyword.appid_short}} 认证</a></td>
<td><code>appid-auth</code></td>
<td>使用 {{site.data.keyword.appid_full}} 向应用程序进行认证。</td>
</tr>
</tbody></table>

<br>

## 一般注释
{: #general}

### 定制错误操作（`custom-errors` 和 `custom-error-actions`）
{: #custom-errors}

指示 ALB 可以对特定 HTTP 错误执行的定制操作。
{: shortdesc}

**描述**</br>
要处理可能发生的特定 HTTP 错误，您可以设置供 ALB 执行的定制错误操作。

* `custom-errors` 注释用于定义服务名称、要处理的 HTTP 错误，以及 ALB 遇到服务的指定 HTTP 错误时所执行错误操作的名称。
* `custom-error-actions` 注释在 NGINX 代码片段中定义定制错误操作。

例如，在 `custom-errors` 注释中，可以通过返回名为 `/errorAction401` 的定制错误操作，将 ALB 设置为处理 `app1` 的 `401` HTTP 错误。然后，在 `custom-error-actions` 注释中，可以定义名为 `/errorAction401` 的代码片段，以便 ALB 将定制错误页面返回给客户机。</br>

此外，还可以使用 `custom-errors` 注释将客户机重定向到您管理的错误服务。必须在 Ingress 资源文件的 `paths` 部分中定义此错误服务的路径。

**样本 Ingress 资源 YAML**</br>

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/custom-errors: "serviceName=<app1> httpError=<401> errorActionName=</errorAction401>;serviceName=<app2> httpError=<403> errorActionName=</errorPath>"
    ingress.bluemix.net/custom-error-actions: |
         errorActionName=</errorAction401>
         #Example custom error snippet
         proxy_pass http://example.com/forbidden.html;
         <EOS>
  spec:
    tls:
    - hosts:
      - mydomain
      secretName: mysecret
    rules:
    - host: mydomain
      http:
        paths:
        - path: /path1
          backend:
            serviceName: app1
            servicePort: 80
        - path: /path2
          backend:
            serviceName: app2
            servicePort: 80
        - path: </errorPath>
          backend:
            serviceName: <error-svc>
            servicePort: 80
```
{: codeblock}

<table>
<caption>了解注释的组成部分</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>将 <code>&lt;<em>app1</em>&gt;</code> 替换为定制错误应用于的 Kubernetes 服务的名称。定制错误仅应用于使用此相同上游服务的特定路径。如果未设置服务名称，那么会将定制错误应用于所有服务路径。</td>
</tr>
<tr>
<td><code>httpError</code></td>
<td>将 <code>&lt;<em>401</em>&gt;</code> 替换为要使用定制错误操作进行处理的 HTTP 错误代码。</td>
</tr>
<tr>
<td><code>errorActionName</code></td>
<td>将 <code>&lt;<em>/errorAction401</em>&gt;</code> 替换为要执行的定制错误操作的名称或错误服务的路径。<ul>
<li>如果指定了定制错误操作的名称，那么必须在 <code>custom-error-actions</code> 注释中的代码片段中定义该错误操作。在样本 YAML 中，<code>app1</code> 使用 <code>/errorAction401</code>，这在 <code>custom-error-actions</code> 注释的片段中定义。</li>
<li>如果指定了错误服务的路径，那么必须在 <code>paths</code> 部分中指定错误路径和错误服务的名称。在样本 YAML 中，<code>app2</code> 使用 <code>/errorPath</code>，这在 <code>paths</code> 部分的末尾进行定义。</li></ul></td>
</tr>
<tr>
<td><code>ingress.bluemix.net/custom-error-actions</code></td>
<td>定义 ALB 对指定的服务和 HTTP 错误执行的定制错误操作。请使用 NGINX 代码片段，并且每个片段以 <code>&lt;EOS&gt;</code> 结束。在样本 YAML 中，<code>app1</code> 发生 <code>401</code> 错误时，ALB 会将定制错误页面 <code>http://example.com/forbidden.html</code> 传递到客户机。</td>
</tr>
</tbody></table>

<br />


### 位置片段 (`location-snippets`)
{: #location-snippets}

为服务添加定制位置块配置。
{:shortdesc}

**描述**</br>
服务器块是 NGINX 伪指令，用于定义 ALB 虚拟服务器的配置。位置块是在服务器块中定义的 NGINX 伪指令。位置块定义 Ingress 如何处理请求 URI，或者定义请求中在域名或 IP 地址和端口之后的部分。

服务器块收到请求时，位置块会将 URI 与路径相匹配，并将请求转发到部署了应用程序的 pod 的 IP 地址。通过使用 `location-snippets` 注释，可以修改位置块如何将请求转发到特定服务。

要改为修改整个服务器块，请参阅 [`server-snippets`](#server-snippets) 注释。

要查看 NGINX 配置文件中的服务器和位置块，请对其中一个 ALB pod 运行以下命令：`kubectl exec -ti <alb_pod> -n kube-system -c nginx-ingress -- cat ./etc/nginx/default-<ingress_resource_name>.conf`
{: tip}

**样本 Ingress 资源 YAML**</br>

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/location-snippets: |
      serviceName=<myservice1>
      # Example location snippet
      proxy_request_buffering off;
      rewrite_log on;
      proxy_set_header "x-additional-test-header" "location-snippet-header";
      <EOS>
      serviceName=<myservice2>
      proxy_set_header Authorization "";
      <EOS>
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080
```
{: codeblock}

<table>
<caption>了解注释的组成部分</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>将 <code>&lt;<em>myservice</em>&gt;</code> 替换针对应用程序创建的服务的名称。</td>
</tr>
<tr>
<td>位置片段</td>
<td>提供要用于指定服务的配置片段。用于 <code>myservice1</code> 服务的样本片段将位置块配置为关闭代理请求缓冲，开启日志重写，并在将请求转发到该服务时设置额外的头。用于 <code>myservice2</code> 服务的样本片段设置空的 <code>Authorization</code> 头。每个位置片段都必须以值 <code>&lt;EOS&gt;</code> 结尾。</td>
</tr>
</tbody></table>

<br />


### 专用 ALB 路由 (`ALB-ID`)
{: #alb-id}

使用专用 ALB 将入局请求路由到应用程序。
{:shortdesc}

**描述**</br>
选择专用 ALB 来路由入局请求，而不选择公共 ALB。

**样本 Ingress 资源 YAML**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/ALB-ID: "<private_ALB_ID_1>;<private_ALB_ID_2>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080
```
{: codeblock}

<table>
<caption>了解注释的组成部分</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
</thead>
<tbody>
<tr>
<td><code>&lt;private_ALB_ID&gt;</code></td>
<td>专用 ALB 的标识。要查找专用 ALB 标识，请运行 <code>ibmcloud ks albs --cluster &lt;my_cluster&gt;</code>。
<p>
如果您的多专区集群启用了多个专用 ALB，那么可以提供使用 <code>;</code> 分隔的 ALB 标识的列表。例如：<code>ingress.bluemix.net/ALB-ID: &lt;private_ALB_ID_1&gt;;&lt;private_ALB_ID_2&gt;;&lt;private_ALB_ID_3&gt;</code></p>
</td>
</tr>
</tbody></table>

<br />


### 服务器片段 (`server-snippets`)
{: #server-snippets}

添加定制服务器块配置。
{:shortdesc}

**描述**</br>
服务器块是 NGINX 伪指令，用于定义 ALB 虚拟服务器的配置。通过在 `server-snippets` 注释中提供定制配置片段，可以修改 ALB 在服务器级别处理请求的方式。

要查看 NGINX 配置文件中的服务器和位置块，请对其中一个 ALB pod 运行以下命令：`kubectl exec -ti <alb_pod> -n kube-system -c nginx-ingress -- cat ./etc/nginx/default-<ingress_resource_name>.conf`
{: tip}

**样本 Ingress 资源 YAML**</br>

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/server-snippets: |
      # Example snippet
      location = /health {
      return 200 'Healthy';
      add_header Content-Type text/plain;
      }
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080
```
{: codeblock}

<table>
<caption>了解注释的组成部分</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
</thead>
<tbody>
<tr>
<td>服务器片段</td>
<td>提供要使用的配置片段。此样本片段指定用于处理 <code>/health</code> 请求的位置块。位置块配置为返回运行正常的响应，并在转发请求时添加头。</td>
</tr>
</tbody>
</table>

可以使用 `server-snippets` 注释在服务器级别为所有服务响应添加头：
{: tip}

```
annotations:
  ingress.bluemix.net/server-snippets: |
    add_header <header1> <value1>;
```
{: codeblock}

<br />


## 连接注释
{: #connection}

通过使用连接注释，可以更改 ALB 连接到后端应用程序和上游服务器的方式，并设置应用程序或服务器被视为不可用之前的超时或最大保持活动连接数。
{: shortdesc}

### 定制连接超时和读取超时（`proxy-connect-timeout` 和 `proxy-read-timeout`）
{: #proxy-connect-timeout}

设置 ALB 等待连接到后端应用程序以及从后端应用程序进行读取的时间，超过该时间后，后端应用程序将视为不可用。
{:shortdesc}

**描述**</br>
将客户机请求发送到 Ingress ALB 时，ALB 会打开与后端应用程序的连接。缺省情况下，ALB 会等待 60 秒以接收来自后端应用程序的应答。如果后端应用程序在 60 秒内未应答，那么连接请求将异常中止，并且后端应用程序将视为不可用。

ALB 连接到后端应用程序后，ALB 会从后端应用程序读取响应数据。在此读操作期间，ALB 在两次读操作之间等待最长 60 秒以接收来自后端应用程序的数据。如果后端应用程序在 60 秒内未发送数据，那么与后端应用程序的连接请求将关闭，并且后端应用程序将视为不可用。

60 秒连接超时和读取超时是代理上的缺省超时，通常不应进行更改。

如果由于工作负载过高而导致应用程序的可用性不稳定或应用程序响应速度缓慢，那么您可能会希望增大连接超时或读取超时。请记住，增大超时会影响 ALB 的性能，因为与后端应用程序的连接必须保持打开状态，直至达到超时为止。

另一方面，您可以减小超时以提高 ALB 的性能。确保后端应用程序能够在指定的超时内处理请求，即使在更高的工作负载期间。

**样本 Ingress 资源 YAML**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/proxy-connect-timeout: "serviceName=<myservice> timeout=<connect_timeout>"
   ingress.bluemix.net/proxy-read-timeout: "serviceName=<myservice> timeout=<read_timeout>"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080
```
{: codeblock}

<table>
<caption>了解注释的组成部分</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
</thead>
<tbody>
<tr>
<td><code>&lt;connect_timeout&gt;</code></td>
<td>等待连接到后端应用程序的秒数或分钟数，例如 <code>65s</code> 或 <code>1m</code>。连接超时不能超过 75 秒。</td>
</tr>
<tr>
<td><code>&lt;read_timeout&gt;</code></td>
<td>读取后端应用程序之前等待的秒数或分钟数，例如 <code>65s</code> 或 <code>2m</code>。
 </tr>
</tbody></table>

<br />


### 保持活动请求数 (`keepalive-requests`)
{: #keepalive-requests}

**描述**</br>
设置可通过一个保持活动连接处理的最大请求数。

**样本 Ingress 资源 YAML**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/keepalive-requests: "serviceName=<myservice> requests=<max_requests>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: <myservice>
          servicePort: 8080
```
{: codeblock}

<table>
<caption>了解注释的组成部分</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>将 <code>&lt;<em>myservice</em>&gt;</code> 替换为您针对应用程序创建的 Kubernetes 服务的名称。此参数是可选的。除非指定了服务，否则配置将应用于 Ingress 子域中的所有服务。如果提供了此参数，那么将为给定的服务设置保持活动请求数。如果未提供此参数，那么将在 <code>nginx.conf</code> 的服务器级别为所有未配置保持活动请求数的服务设置保持活动请求数。</td>
</tr>
<tr>
<td><code>requests</code></td>
<td>将 <code>&lt;<em>max_requests</em>&gt;</code> 替换为可通过一个保持活动连接处理的最大请求数。</td>
</tr>
</tbody></table>

<br />


### 保持活动超时 (`keepalive-timeout`)
{: #keepalive-timeout}

**描述**</br>
设置保持活动连接在服务器上保持打开状态的最长时间。


**样本 Ingress 资源 YAML**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/keepalive-timeout: "serviceName=<myservice> timeout=<time>s"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080
```
{: codeblock}

<table>
<caption>了解注释的组成部分</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>将 <code>&lt;<em>myservice</em>&gt;</code> 替换为您针对应用程序创建的 Kubernetes 服务的名称。此参数是可选的。如果提供了此参数，那么将为给定的服务设置保持活动超时。如果未提供此参数，那么将在 <code>nginx.conf</code> 的服务器级别为所有未配置保持活动超时的服务设置保持活动超时。</td>
</tr>
<tr>
<td><code>timeout</code></td>
<td>将 <code>&lt;<em>time</em>&gt;</code> 替换为时间长度（以秒为单位）。示例：<code>timeout=20s</code>。值为 <code>0</code> 将禁用保持活动客户机连接。</td>
</tr>
</tbody></table>

<br />


### 作为代理传递到下一个上游 (`proxy-next-upstream-config`)
{: #proxy-next-upstream-config}

设置 ALB 何时可以向下一个上游服务器传递请求。
{:shortdesc}

**描述**</br>
Ingress ALB 充当客户机应用程序和您的应用程序之间的代理。某些应用程序设置需要多个上游服务器来处理来自 ALB 的入局客户机请求。有时，ALB 使用的代理服务器无法与应用程序使用的上游服务器建立连接。于是 ALB 可以尝试与下一个上游服务器建立连接，以改为将请求传递到这一个上游服务器。可以使用 `proxy-next-upstream-config` 注释来设置在哪些情况下 ALB 可以尝试将请求传递到下一个上游服务器，以及尝试的时间长度和次数。

使用 `proxy-next-upstream-config` 时，始终会配置超时，所以不要将 `timeout=true` 添加到此注释。
{: note}

**样本 Ingress 资源 YAML**</br>

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/proxy-next-upstream-config: "serviceName=<myservice1> retries=<tries> timeout=<time> error=true http_502=true; serviceName=<myservice2> http_403=true non_idempotent=true"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice1
          servicePort: 80
```
{: codeblock}

<table>
<caption>了解注释的组成部分</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>将 <code>&lt;<em>myservice</em>&gt;</code> 替换为您针对应用程序创建的 Kubernetes 服务的名称。</td>
</tr>
<tr>
<td><code>retries</code></td>
<td>将 <code>&lt;<em>tries</em>&gt;</code> 替换为 ALB 尝试将请求传递到下一个上游服务器的最大次数。此数字包括原始请求。要关闭此限制，请使用 <code>0</code>。如果不指定值，将使用缺省值 <code>0</code>。
</td>
</tr>
<tr>
<td><code>timeout</code></td>
<td>将 <code>&lt;<em>time</em>&gt;</code> 替换为 ALB 尝试将请求传递到下一个上游服务器的最长时间（以秒为单位）。例如，要设置 30 秒时间，请输入 <code>30s</code>。要关闭此限制，请使用 <code>0</code>。如果不指定值，将使用缺省值 <code>0</code>。
</td>
</tr>
<tr>
<td><code>error</code></td>
<td>如果设置为 <code>true</code>，那么在与第一个上游服务器建立连接，向其传递请求或读取响应头时发生错误的情况下，ALB 会将请求传递到下一个上游服务器。
</td>
</tr>
<tr>
<td><code>invalid_header</code></td>
<td>如果设置为 <code>true</code>，那么在第一个上游服务器返回空响应或无效响应时，ALB 会将请求传递到下一个上游服务器。
</td>
</tr>
<tr>
<td><code>http_502</code></td>
<td>如果设置为 <code>true</code>，那么在第一个上游服务器返回代码为 502 的响应时，ALB 会将请求传递到下一个上游服务器。可以指定以下 HTTP 响应代码：<code>500</code>、<code>502</code>、<code>503</code>、<code>504</code>、<code>403</code>、<code>404</code> 或 <code>429</code>。
</td>
</tr>
<tr>
<td><code>non_idempotent</code></td>
<td>如果设置为 <code>true</code>，那么 ALB 可以使用非幂等方法将请求传递到下一个上游服务器。缺省情况下，ALB 不会将这些请求传递到下一个上游服务器。
</td>
</tr>
<tr>
<td><code>off</code></td>
<td>要阻止 ALB 将请求传递到下一个上游服务器，请设置为 <code>true</code>。
</td>
</tr>
</tbody></table>

<br />


### 使用 cookie 确保会话亲缘关系 (`sticky-cookie-services`)
{: #sticky-cookie-services}

使用粘性 cookie 注释向 ALB 添加会话亲缘关系，并始终将入局网络流量路由到同一个上游服务器。
{:shortdesc}

**描述**</br>
为了实现高可用性，某些应用程序设置要求您部署多个上游服务器来处理入局客户机请求。客户机连接到后端应用程序后，可以使用会话亲缘关系，使得在会话期间或完成任务所需的时间内，客户机由同一个上游服务器处理。您可以将 ALB 配置为始终将入局网络流量路由到同一个上游服务器来确保会话亲缘关系。

连接到后端应用程序的每个客户机都会由 ALB 分配其中一个可用的上游服务器。ALB 会创建会话 cookie，该会话 cookie 存储在客户机的应用程序中，并且包含在 ALB 和客户机之间每个请求的头信息中。该 cookie 中的信息将确保在整个会话中的所有请求都由同一个上游服务器进行处理。

依赖粘性会话会增加复杂性并降低可用性。例如，您可能有一个 HTTP 服务器，用于维护初始连接的某个会话状态，以便 HTTP 服务只接受具有相同会话状态值的后续请求。但是，这会妨碍对 HTTP 服务进行轻松水平缩放。请考虑使用外部数据库（例如，Redis 或 Memcached）来存储 HTTP 请求会话值，以便可以在多个服务器之间维护该会话状态。
{: note}

当包含多个服务时，请使用分号 (;) 来分隔这些服务。

**样本 Ingress 资源 YAML**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/sticky-cookie-services: "serviceName=<myservice1> name=<cookie_name1> expires=<expiration_time1> path=<cookie_path1> hash=<hash_algorithm1>;serviceName=<myservice2> name=<cookie_name2> expires=<expiration_time2> path=<cookie_path2> hash=<hash_algorithm2>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /service1_path
        backend:
          serviceName: <myservice1>
          servicePort: 8080
      - path: /service2_path
        backend:
          serviceName: <myservice2>
          servicePort: 80
```
{: codeblock}

<table>
<caption>了解注释的组成部分</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>将 <code>&lt;<em>myservice</em>&gt;</code> 替换为您针对应用程序创建的 Kubernetes 服务的名称。</td>
</tr>
<tr>
<td><code>name</code></td>
<td>将 <code><em>&lt;cookie_name&gt;</em></code> 替换为在会话期间创建的粘性 cookie 的名称。</td>
</tr>
<tr>
<td><code>expires</code></td>
<td>将 <code>&lt;<em>expiration_time</em>&gt;</code> 替换为时间，以秒 (s)、分钟 (m) 或小时 (h) 为单位；超过该时间后，粘性 cookie 到期。此时间与用户活动无关。该 cookie 到期后，会被客户机 Web 浏览器删除，并且不会再发送到 ALB。例如，要将到期时间设置为 1 秒、1 分钟或 1 小时，请输入 <code>1s</code>、<code>1m</code> 或 <code>1h</code>。</td>
</tr>
<tr>
<td><code>path</code></td>
<td>将 <code><em>&lt;cookie_path&gt;</em></code> 替换为附加到 Ingress 子域的路径，该路径指示针对哪些域和子域将 cookie 发送到 ALB。例如，如果 Ingress 域为 <code>www.myingress.com</code>，并且您希望在每个客户机请求中都发送 cookie，那么必须设置 <code>path=/</code>。如果希望仅针对 <code>www.myingress.com/myapp</code> 及其所有子域发送 cookie，那么必须设置 <code>path=/myapp</code>。</td>
</tr>
<tr>
<td><code>hash</code></td>
<td>将 <code><em>&lt;hash_algorithm&gt;</em></code> 替换为用于保护 cookie 中信息的散列算法。仅支持 <code>sha1</code>。SHA1 基于 cookie 中的信息创建散列总和，并将此散列总和附加到 cookie。服务器可以解密 cookie 中的信息并验证数据完整性。</td>
</tr>
</tbody></table>

<br />


### 上游故障超时 (`upstream-fail-timeout`)
{: #upstream-fail-timeout}

设置 ALB 可以尝试连接到服务器的时间量。
{:shortdesc}

**描述**</br>
设置在服务器被视为不可用之前，ALB 可以尝试连接到服务器的时间量。如果要将服务器视为不可用，ALB 必须在设置的时间量内达到 [`upstream-max-fails`](#upstream-max-fails) 注释设置的最大失败连接尝试次数。此时间量还可确定服务器被视为不可用的持续时间。


**样本 Ingress 资源 YAML**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/upstream-fail-timeout: "serviceName=<myservice> fail-timeout=<fail_timeout>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080
```
{: codeblock}

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
</thead>
<tbody>
<tr>
<td><code>serviceName（可选）</code></td>
<td>将 <code>&lt;<em>myservice</em>&gt;</code> 替换为您针对应用程序创建的 Kubernetes 服务的名称。</td>
</tr>
<tr>
<td><code>fail-timeout</code></td>
<td>将 <code>&lt;<em>fail_timeout</em>&gt;</code> 替换为在服务器被视为不可用之前，ALB 可以尝试连接到服务器的时间量。缺省值为 <code>10s</code>。时间必须以秒为单位。</td>
</tr>
</tbody></table>

<br />


### 上游保持活动连接数 (`upstream-keepalive`)
{: #upstream-keepalive}

设置上游服务器的最大空闲保持活动连接数。
{:shortdesc}

**描述**</br>
设置给定服务的上游服务器的最大空闲保持活动连接数。缺省情况下，上游服务器具有 64 个空闲保持活动连接。

**样本 Ingress 资源 YAML**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/upstream-keepalive: "serviceName=<myservice> keepalive=<max_connections>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080
```
{: codeblock}

<table>
<caption>了解注释的组成部分</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>将 <code>&lt;<em>myservice</em>&gt;</code> 替换为您针对应用程序创建的 Kubernetes 服务的名称。</td>
</tr>
<tr>
<td><code>keepalive</code></td>
<td>将 <code>&lt;<em>max_connections</em>&gt;</code> 替换为上游服务器的最大空闲保持活动连接数。缺省值为 <code>64</code>。值为 <code>0</code> 将禁用给定服务的上游保持活动连接。</td>
</tr>
</tbody></table>

<br />


### 上游最大失败次数 (`upstream-max-fails`)
{: #upstream-max-fails}

设置尝试与服务器通信失败的最大次数。
{:shortdesc}

**描述**</br>
设置在服务器被视为不可用之前，ALB 可以尝试连接到服务器失败的最大次数。如果要将服务器视为不可用，ALB 必须在 [`upstream-fail-timeout`](#upstream-fail-timeout) 注释设置的持续时间内达到最大次数。此外，还可通过 `upstream-fail-timeout` 注释设置服务器被视为不可用的持续时间。

**样本 Ingress 资源 YAML**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/upstream-max-fails: "serviceName=<myservice> max-fails=<max_fails>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080
```
{: codeblock}

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
</thead>
<tbody>
<tr>
<td><code>serviceName（可选）</code></td>
<td>将 <code>&lt;<em>myservice</em>&gt;</code> 替换为您针对应用程序创建的 Kubernetes 服务的名称。</td>
</tr>
<tr>
<td><code>max-fails</code></td>
<td>将 <code>&lt;<em>max_fails</em>&gt;</code> 替换为 ALB 可以尝试与服务器通信失败的最大次数。缺省值为 <code>1</code>。<code>0</code> 值将禁用此注释。</td>
</tr>
</tbody></table>

<br />


## HTTPS 和 TLS/SSL 认证注释
{: #https-auth}

通过使用 HTTPS 和 TLS/SSL 认证注释，可以配置 ALB 用于 HTTPS 流量，更改缺省 HTTPS 端口，对发送到后端应用程序的流量启用 SSL 加密，或设置相互认证。
{: shortdesc}

### 定制 HTTP 和 HTTPS 端口 (`custom-port`)
{: #custom-port}

更改 HTTP（端口 80）和 HTTPS（端口 443）网络流量的缺省端口。
{:shortdesc}

**描述**</br>
缺省情况下，Ingress ALB 配置为在端口 80 上侦听入局 HTTP 网络流量，在端口 443 上侦听入局 HTTPS 网络流量。您可以更改缺省端口以向 ALB 域添加安全性，或仅启用 HTTPS 端口。

要在端口上启用相互认证，请[配置 ALB 以打开有效端口](/docs/containers?topic=containers-ingress#opening_ingress_ports)，然后在 [`mutual-auth` 注释](#mutual-auth)中指定该端口。不要使用 `custom-port` 注释来指定用于相互认证的端口。
{: note}

**样本 Ingress 资源 YAML**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/custom-port: "protocol=<protocol1> port=<port1>;protocol=<protocol2> port=<port2>"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080
```
{: codeblock}

<table>
<caption>了解注释的组成部分</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
 </thead>
 <tbody>
 <tr>
 <td><code>&lt;protocol&gt;</code></td>
 <td>输入 <code>http</code> 或 <code>https</code> 以更改入局 HTTP 或 HTTPS 网络流量的缺省端口。</td>
 </tr>
 <tr>
 <td><code>&lt;port&gt;</code></td>
 <td>输入要用于入局 HTTP 或 HTTPS 网络流量的端口号。<p class="note">指定定制端口用于 HTTP 或 HTTPS 时，缺省端口对于 HTTP 和 HTTPS 都不再有效。例如，要将 HTTPS 的缺省端口更改为 8443，而对 HTTP 使用缺省端口，那么必须为两者设置定制端口：<code>custom-port: "protocol=http port=80; protocol=https port=8443"</code>。</p></td>
 </tr>
</tbody></table>


**用法**</br>
1. 查看 ALB 的打开端口。

  ```
  kubectl get service -n kube-system
  ```
  {: pre}

  CLI 输出类似于以下内容：

  ```
  NAME                                             TYPE           CLUSTER-IP     EXTERNAL-IP    PORT(S)                      AGE
  public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx    169.xx.xxx.xxx 80:30416/TCP,443:32668/TCP   109d
  ```
  {: screen}

2. 打开 ALB 配置映射。

  ```
  kubectl edit configmap ibm-cloud-provider-ingress-cm -n kube-system
  ```
  {: pre}

3. 将非缺省 HTTP 和 HTTPS 端口添加到配置映射。将 `<port>` 替换为要打开的 HTTP 或 HTTPS 端口。
  <p class="note">缺省情况下，端口 80 和 443 已打开。如果要使 80 和 443 保持打开，那么必须在 `public-ports` 字段中包含这两个端口以及您指定的其他任何 TCP 端口。如果启用了专用 ALB，那么还必须在 `private-ports` 字段中指定要保持打开的任何端口。有关更多信息，请参阅[在 Ingress ALB 中打开端口](/docs/containers?topic=containers-ingress#opening_ingress_ports)。
</p>
  ```
  apiVersion: v1
  kind: ConfigMap
  data:
    public-ports: <port1>;<port2>
  metadata:
    creationTimestamp: 2017-08-22T19:06:51Z
    name: ibm-cloud-provider-ingress-cm
    namespace: kube-system
    resourceVersion: "1320"
    selfLink: /api/v1/namespaces/kube-system/configmaps/ibm-cloud-provider-ingress-cm
    uid: <uid>
  ```
  {: codeblock}

4. 验证是否已将 ALB 重新配置为使用非缺省端口。

  ```
  kubectl get service -n kube-system
  ```
  {: pre}

  CLI 输出类似于以下内容：

  ```
  NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP    PORT(S)                           AGE
  public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx    169.xx.xxx.xxx <port1>:30776/TCP,<port2>:30412/TCP   109d
  ```
  {: screen}

5. 配置 Ingress 以在将入局网络流量路由到服务时使用非缺省端口。在此参考内容中使用样本 YAML 文件中的注释。

6. 更新 ALB 配置。

  ```
  kubectl apply -f myingress.yaml
  ```
  {: pre}

7. 打开首选 Web 浏览器以访问您的应用程序。示例：`https://<ibmdomain>:<port>/<service_path>/`

<br />


### HTTP 重定向到 HTTPS (`redirect-to-https`)
{: #redirect-to-https}

将不安全的 HTTP 客户机请求转换为 HTTPS。
{:shortdesc}

**描述**</br>
将 Ingress ALB 设置为通过 IBM 提供的 TLS 证书或定制 TLS 证书来保护域。某些用户可能会尝试向 ALB 域发出不安全的 `http` 请求（例如 `http://www.myingress.com`，而不是使用 `https`）来访问应用程序。可以使用重定向注释始终将不安全的 HTTP 请求转换为 HTTPS。如果不使用此注释，缺省情况下，不安全的 HTTP 请求不会转换为 HTTPS 请求，并且可能会向公众公开未加密的保密信息。

 


缺省情况下，HTTP 请求到 HTTPS 的重定向是禁用的。

**样本 Ingress 资源 YAML**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/redirect-to-https: "True"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080
```
{: codeblock}

<br />


### HTTP 严格传输安全性 (`hsts`)
{: #hsts}

**描述**</br>
HSTS 指示浏览器仅使用 HTTPS 访问域。即使用户输入或访问普通 HTTP 链接，浏览器也会严格地将连接升级到 HTTPS。


**样本 Ingress 资源 YAML**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/hsts: enabled=true maxAge=<31536000> includeSubdomains=true
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /service1_path
        backend:
          serviceName: myservice1
          servicePort: 8443
      - path: /service2_path
        backend:
          serviceName: myservice2
          servicePort: 8444
          ```
{: codeblock}

<table>
<caption>了解注释的组成部分</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
</thead>
<tbody>
<tr>
<td><code>enabled</code></td>
<td>使用 <code>true</code> 以启用 HSTS。</td>
</tr>
<tr>
<td><code>maxAge</code></td>
<td>将 <code>&lt;<em>31536000</em>&gt;</code> 替换为整数，该整数表示浏览器将发送的请求直接高速缓存到 HTTPS 的秒数。缺省值为 <code>31536000</code>，等于 1 年。</td>
</tr>
<tr>
<td><code>includeSubdomains</code></td>
<td>使用 <code>true</code> 以指示浏览器 HSTS 策略也适用于当前域的所有子域。缺省值为 <code>true</code>。</td>
</tr>
</tbody></table>

<br />


### 相互认证 (`mutual-auth`)
{: #mutual-auth}

为 ALB 配置相互认证。
{:shortdesc}

**描述**</br>
为 Ingress ALB 配置下游流量的相互认证。外部客户机会认证服务器，而服务器也会使用证书来认证客户机。相互认证也称为基于证书的认证或双向认证。
 

对于客户机与 Ingress ALB 之间的 SSL 终止，请使用 `mutual-auth` 注释。对于 Ingress ALB 与后端应用程序之间的 SSL 终止，请使用 [`ssl-services` 注释](#ssl-services)。

相互认证注释用于验证客户机证书。要转发应用程序头中的客户机证书以处理授权，可以使用以下 [`proxy-add-headers` 注释](#proxy-add-headers)：`"ingress.bluemix.net/proxy-add-headers": "serviceName=router-set {\n X-Forwarded-Client-Cert $ssl_client_escaped_cert;\n}\n"`
{: tip}

**先决条件**</br>

* 您必须具有包含所需 `ca.crt` 的有效相互认证私钥。要创建相互认证私钥，请参阅此部分末尾的步骤。
* 要在 443 之外的端口上启用相互认证，请[配置 ALB 以打开有效端口](/docs/containers?topic=containers-ingress#opening_ingress_ports)，然后在此注释中指定该端口。不要使用 `custom-port` 注释来指定用于相互认证的端口。

**样本 Ingress 资源 YAML**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/mutual-auth: "secretName=<mysecret> port=<port> serviceName=<servicename1>,<servicename2>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080
```
{: codeblock}

<table>
<caption>了解注释的组成部分</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
</thead>
<tbody>
<tr>
<td><code>secretName</code></td>
<td>将 <code>&lt;<em>mysecret</em>&gt;</code> 替换为私钥资源的名称。</td>
</tr>
<tr>
<td><code>port</code></td>
<td>将 <code>&lt;<em>port</em>&gt;</code> 替换为 ALB 端口号。</td>
</tr>
<tr>
<td><code>serviceName</code></td>
<td>将 <code><em>&lt;serviceName&gt;</em></code> 替换为一个或多个 Ingress 资源的名称。此参数是可选的。</td>
</tr>
</tbody></table>

**要创建相互认证私钥，请执行以下操作：**

1. 通过证书提供者生成认证中心 (CA) 证书和密钥。如果您有自己的域，请为您的域购买正式的 TLS 证书。请确保每个证书的 [CN ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://support.dnsimple.com/articles/what-is-common-name/) 都是不同的。出于测试目的，可以使用 OpenSSL 创建自签名证书。有关更多信息，请参阅此[自签名 SSL 证书教程 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.akadia.com/services/ssh_test_certificate.html) 或[包含创建您自己的 CA 的相互认证教程 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://blog.codeship.com/how-to-set-up-mutual-tls-authentication/)。
    {: tip}
2. [将证书转换为 Base64 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.base64encode.org/)。
3. 使用证书创建私钥 YAML 文件。
     
   ```
     apiVersion: v1
     kind: Secret
     metadata:
       name: ssl-my-test
     type: Opaque
     data:
       ca.crt: <ca_certificate>
     ```
   {: codeblock}
4. 将证书创建为 Kubernetes 私钥。
     
   ```
     kubectl create -f ssl-my-test
     ```
   {: pre}

<br />


### SSL 服务支持 (`ssl-services`)
{: #ssl-services}

允许对上游应用程序进行 HTTPS 请求，并对流入上游应用程序的流量加密。
{:shortdesc}

**描述**</br>
Ingress 资源配置具有 TLS 部分时，Ingress ALB 可以处理向应用程序发出的通过 HTTPS 保护的 URL 请求。缺省情况下，ALB 会终止 TLS 终止，并在使用 HTTP 协议将流量转发到应用程序之前对请求解密。如果您具有需要 HTTPS 协议且需要对流量加密的应用程序，请使用 `ssl-services` 注释。通过使用 `ssl-services` 注释，ALB 将终止外部 TLS 连接，然后在 ALB 和应用程序 pod 之间创建新的 SSL 连接。在将流量发送到上游 pod 之前，会对流量重新加密。

如果后端应用程序可以处理 TLS，并且您希望添加额外的安全性，那么可以通过提供私钥中包含的证书来添加单向或相互认证。

对于 Ingress ALB 与后端应用程序之间的 SSL 终止，请使用 `ssl-services` 注释。对于客户机与 Ingress ALB 之间的 SSL 终止，请使用 [`mutual-auth` 注释](#mutual-auth)。
{: tip}

**样本 Ingress 资源 YAML**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: <myingressname>
  annotations:
    ingress.bluemix.net/ssl-services: ssl-service=<myservice1> ssl-secret=<service1-ssl-secret>;ssl-service=<myservice2> ssl-secret=<service2-ssl-secret>
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /service1_path
        backend:
          serviceName: myservice1
          servicePort: 8443
      - path: /service2_path
        backend:
          serviceName: myservice2
          servicePort: 8444
          ```
{: codeblock}

<table>
<caption>了解注释的组成部分</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
</thead>
<tbody>
<tr>
<td><code>ssl-service</code></td>
<td>将 <code>&lt;<em>myservice</em>&gt;</code> 替换为需要 HTTPS 的服务的名称。将加密从 ALB 到此应用程序服务的流量。</td>
</tr>
<tr>
<td><code>ssl-secret</code></td>
<td>如果后端应用程序可以处理 TLS，并且您希望添加额外的安全性，请将 <code>&lt;<em>service-ssl-secret</em>&gt;</code> 替换为该服务的单向或相互认证私钥。<ul><li>如果提供了单向认证私钥，那么该值必须包含来自上游服务器的 <code>trusted.crt</code>。要创建单向认证私钥，请参阅此部分末尾的步骤。</li><li>如果提供了相互认证私钥，那么值必须包含应用程序期望从客户机收到的必需的 <code>client.crt</code> 和 <code>client.key</code>。要创建相互认证私钥，请参阅此部分末尾的步骤。</li></ul><p class="important">如果未提供私钥，系统会允许不安全连接。如果要测试连接并且证书尚未就绪，或者如果证书已到期并且您希望允许不安全的连接，那么可选择省略私钥。</p></td>
</tr>
</tbody></table>


**要创建单向认证私钥，请执行以下操作：**

1. 从上游服务器获取认证中心 (CA) 密钥和证书并获取 SSL 客户机证书。IBM ALB 基于 NGINX，这需要根证书、中间证书和后端证书。有关更多信息，请参阅 [NGINX 文档 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://docs.nginx.com/nginx/admin-guide/security-controls/securing-http-traffic-upstream/)。
2. [将证书转换为 Base64 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.base64encode.org/)。
3. 使用证书创建私钥 YAML 文件。
     
   ```
     apiVersion: v1
     kind: Secret
     metadata:
       name: ssl-my-test
     type: Opaque
     data:
       trusted.crt: <ca_certificate>
     ```
   {: codeblock}

   如果还要对上游流量强制执行相互认证，那么除了在 data 部分中提供 `trusted.crt` 外，还可以提供 `client.crt` 和 `client.key`。
   {: tip}

4. 将证书创建为 Kubernetes 私钥。
     
   ```
     kubectl create -f ssl-my-test
     ```
   {: pre}

</br>
**要创建相互认证私钥，请执行以下操作：**

1. 通过证书提供者生成认证中心 (CA) 证书和密钥。如果您有自己的域，请为您的域购买正式的 TLS 证书。请确保每个证书的 [CN ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://support.dnsimple.com/articles/what-is-common-name/) 都是不同的。出于测试目的，可以使用 OpenSSL 创建自签名证书。有关更多信息，请参阅此[自签名 SSL 证书教程 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.akadia.com/services/ssh_test_certificate.html) 或[包含创建您自己的 CA 的相互认证教程 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://blog.codeship.com/how-to-set-up-mutual-tls-authentication/)。
    {: tip}
2. [将证书转换为 Base64 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.base64encode.org/)。
3. 使用证书创建私钥 YAML 文件。
     
   ```
     apiVersion: v1
     kind: Secret
     metadata:
       name: ssl-my-test
     type: Opaque
     data:
       ca.crt: <ca_certificate>
     ```
   {: codeblock}
4. 将证书创建为 Kubernetes 私钥。
     
   ```
     kubectl create -f ssl-my-test
     ```
   {: pre}

<br />


### TCP 端口 (`tcp-ports`)
{: #tcp-ports}

通过非标准 TCP 端口访问应用程序。
{:shortdesc}

**描述**</br>
将此注释用于在运行 TCP 流工作负载的应用程序。

<p class="note">ALB 以通过方式运行，并将流量转发到后端应用程序。在此情况下，不支持 SSL 终止。TLS 连接不会终止，也不会通过非接触方式传递。</p>

**样本 Ingress 资源 YAML**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/tcp-ports: "serviceName=<myservice> ingressPort=<ingress_port> servicePort=<service_port>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 80
```
{: codeblock}

<table>
<caption>了解注释的组成部分</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>将 <code>&lt;<em>myservice</em>&gt;</code> 替换为要通过非标准 TCP 端口访问的 Kubernetes 服务的名称。</td>
</tr>
<tr>
<td><code>ingressPort</code></td>
<td>将 <code><em>&lt;ingress_port&gt;</em></code> 替换为要用于访问应用程序的 TCP 端口。</td>
</tr>
<tr>
<td><code>servicePort</code></td>
<td>此参数是可选的。如果提供了此参数，那么在将流量发送到后端应用程序之前，会将端口替换为此值。否则，该端口与 Ingress 端口保持相同。如果不想设置此参数，那么可以将其从配置中除去。</td>
</tr>
</tbody></table>


**用法**</br>
1. 查看 ALB 的打开端口。

  ```
  kubectl get service -n kube-system
  ```
  {: pre}

  CLI 输出类似于以下内容：
  ```
  NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP      PORT(S)                      AGE
  public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx    169.xx.xxx.xxx   80:30416/TCP,443:32668/TCP   109d
  ```
  {: screen}

2. 打开 ALB 配置映射。

  ```
  kubectl edit configmap ibm-cloud-provider-ingress-cm -n kube-system
  ```
  {: pre}

3. 将 TCP 端口添加到配置映射。将 `<port>` 替换为要打开的 TCP 端口。缺省情况下，端口 80 和 443 已打开。如果要使 80 和 443 保持打开，那么必须在 `public-ports` 字段中包含这两个端口以及您指定的其他任何 TCP 端口。如果启用了专用 ALB，那么还必须在 `private-ports` 字段中指定要保持打开的任何端口。有关更多信息，请参阅[在 Ingress ALB 中打开端口](/docs/containers?topic=containers-ingress#opening_ingress_ports)。
  {: note}
  ```
  apiVersion: v1
  kind: ConfigMap
  data:
    public-ports: 80;443;<port1>;<port2>
  metadata:
    creationTimestamp: 2017-08-22T19:06:51Z
    name: ibm-cloud-provider-ingress-cm
    namespace: kube-system
    resourceVersion: "1320"
    selfLink: /api/v1/namespaces/kube-system/configmaps/ibm-cloud-provider-ingress-cm
    uid: <uid>
   ```
  {: codeblock}

4. 验证是否将 ALB 重新配置为使用 TCP 端口。

  ```
  kubectl get service -n kube-system
  ```
  {: pre}
CLI 输出类似于以下内容：
```
  NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP      PORT(S)                               AGE
  public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx    169.xx.xxx.xxx   <port1>:30776/TCP,<port2>:30412/TCP   109d
  ```
  {: screen}

5. 将 ALB 配置为通过非标准 TCP 端口访问应用程序。在此参考内容中使用样本 YAML 文件中的 `tcp-ports` 注释。

6. 创建 ALB 资源或更新现有 ALB 配置。

  ```
  kubectl apply -f myingress.yaml
  ```
  {: pre}

7. 通过 Curl 命令获取用于访问应用程序的 Ingress 子域。示例：`curl <domain>:<ingressPort>`

<br />


## 路径路由注释
{: #path-routing}

Ingress ALB 将流量路由到后端应用程序侦听的路径。通过路径路由注释，可以配置 ALB 如何将流量路由到应用程序。
{: shortdesc}

### 外部服务 (`proxy-external-service`)
{: #proxy-external-service}

添加外部服务（例如，{{site.data.keyword.Bluemix_notm}} 中托管的服务）的路径定义。
{:shortdesc}

**描述**</br>
添加外部服务的路径定义。仅当应用程序在外部服务（而不是后端服务）上运行时，才可使用此注释。使用此注释来创建外部服务路径时，仅支持 `client-max-body-size`、`proxy-read-timeout`、`proxy-connect-timeout` 和 `proxy-buffering` 注释一起使用。不支持其他任何注释与 `proxy-external-service` 一起使用。


不能为单个服务和路径指定多个主机。
{: note}

**样本 Ingress 资源 YAML**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/proxy-external-service: "path=<mypath> external-svc=https:<external_service> host=<mydomain>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 80
```
{: codeblock}

<table>
<caption>了解注释的组成部分</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
</thead>
<tbody>
<tr>
<td><code>path</code></td>
<td>将 <code>&lt;<em>mypath</em>&gt;</code> 替换为外部服务侦听的路径。</td>
</tr>
<tr>
<td><code>external-svc</code></td>
<td>将 <code>&lt;<em>external_service</em>&gt;</code> 替换为要调用的外部服务。例如，<code>https://&lt;myservice&gt;.&lt;region&gt;.mybluemix.net</code>。</td>
</tr>
<tr>
<td><code>host</code></td>
<td>将 <code>&lt;<em>mydomain</em>&gt;</code> 替换为外部服务的主机域。</td>
</tr>
</tbody></table>

<br />


### 位置修饰符 (`location-modifier`)
{: #location-modifier}

修改 ALB 将请求 URI 与应用程序路径相匹配的方式。
{:shortdesc}

**描述**</br>
缺省情况下，ALB 会将应用程序侦听的路径作为前缀进行处理。ALB 接收到对应用程序的请求时，ALB 会检查 Ingress 资源以查找与请求 URI 的开头相匹配的路径（作为前缀）。如果找到匹配项，那么会将请求转发到部署了应用程序的 pod 的 IP 地址。

`location-modifier` 注释通过修改位置块配置来更改 ALB 搜索匹配项的方式。位置块用于确定如何处理请求中的应用程序路径。

要处理正则表达式 (regex) 路径，此注释是必需的。
{: note}

**支持的修饰符**</br>
<table>
<caption>支持的修饰符</caption>
<col width="10%">
<col width="90%">
<thead>
<th>修饰符</th>
<th>描述</th>
</thead>
<tbody>
<tr>
<td><code>=</code></td>
<td>等号修饰符使 ALB 仅选择完全匹配项。找到完全匹配项时，搜索将停止，并选择匹配路径。<br>例如，如果应用程序侦听的是 <code>/tea</code>，那么 ALB 在匹配对应用程序的请求时，仅选择完全匹配的 <code>/tea</code> 路径。</td>
</tr>
<tr>
<td><code>~</code></td>
<td>波浪号修饰符使 ALB 在匹配期间将路径作为区分大小写的 regex 路径进行处理。<br>例如，如果应用程序侦听的是 <code>/coffee</code>，那么 ALB 在匹配对应用程序的请求时，可以选择 <code>/ab/coffee</code> 或 <code>/123/coffee</code> 路径，即便并未为应用程序显式设置这些路径也不例外。</td>
</tr>
<tr>
<td><code>~\*</code></td>
<td>波浪号后跟星号修饰符使 ALB 在匹配期间将路径作为不区分大小写的 regex 路径进行处理。<br>例如，如果应用程序侦听的是 <code>/coffee</code>，那么 ALB 在匹配对应用程序的请求时，可以选择 <code>/ab/Coffee</code> 或 <code>/123/COFFEE</code> 路径，即便并未为应用程序显式设置这些路径也不例外。</td>
</tr>
<tr>
<td><code>^~</code></td>
<td>重音符后跟波浪号修饰符使 ALB 选择最佳的非 regex 匹配项，而不选择 regex 路径。</td>
</tr>
</tbody>
</table>


**样本 Ingress 资源 YAML**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/location-modifier: "modifier='<location_modifier>' serviceName=<myservice1>;modifier='<location_modifier>' serviceName=<myservice2>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 80
```
{: codeblock}

<table>
<caption>了解注释的组成部分</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
</thead>
<tbody>
<tr>
<td><code>modifier</code></td>
<td>将 <code>&lt;<em>location_modifier</em>&gt;</code> 替换为要用于路径的位置修饰符。支持的修饰符为 <code>'='</code>、<code>'~'</code>、<code>'~\*'</code> 和 <code>'^~'</code>。必须用单引号将修饰符括起。</td>
</tr>
<tr>
<td><code>serviceName</code></td>
<td>将 <code>&lt;<em>myservice</em>&gt;</code> 替换为针对应用程序创建的 Kubernetes 服务的名称。</td>
</tr>
</tbody></table>

<br />


### 重写路径 (`rewrite-path`)
{: #rewrite-path}

将 ALB 域路径上的入局网络流量路由到后端应用程序侦听的其他路径。
{:shortdesc}

**描述**</br>
Ingress ALB 域将 `mykubecluster.us-south.containers.appdomain.cloud/beans` 上的入局网络流量路由到应用程序。应用程序侦听的是 `/coffee`，而不是 `/beans`。要将入局网络流量转发到应用程序，请将 rewrite 注释添加到 Ingress 资源配置文件。rewrite 注释可确保使用 `/coffee` 路径将 `/beans` 上的入局网络流量转发到应用程序。当包含多个服务时，请仅使用分号 (;) 来分隔这些服务，分号前后不留空格。

**样本 Ingress 资源 YAML**</br>

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/rewrite-path: "serviceName=<myservice1> rewrite=<target_path1>;serviceName=<myservice2> rewrite=<target_path2>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /beans
        backend:
          serviceName: myservice1
          servicePort: 80
```
{: codeblock}

<table>
<caption>了解注释的组成部分</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>将 <code>&lt;<em>myservice</em>&gt;</code> 替换为您针对应用程序创建的 Kubernetes 服务的名称。</td>
</tr>
<tr>
<td><code>rewrite</code></td>
<td>将 <code>&lt;<em>target_path</em>&gt;</code> 替换为应用程序侦听的路径。ALB 域上的入局网络流量会使用此路径转发到 Kubernetes 服务。大多数应用程序不会侦听特定路径，而是使用根路径和特定端口。在 <code>mykubecluster.us-south.containers.appdomain.cloud/beans</code> 的示例中，重写路径为 <code>/coffee</code>。<p class= "note">如果应用了此文件，但该 URL 显示 <code>404</code> 响应，说明后端应用程序可能正在侦听以 `/` 结尾的路径。请尝试向此 rewrite 字段添加尾部 `/`，然后重新应用此文件，并重试该 URL。</p></td>
</tr>
</tbody></table>

<br />


## 代理缓冲区注释
{: #proxy-buffer}

Ingress ALB 充当后端应用程序和客户机 Web 浏览器之间的代理。通过使用代理缓冲区注释，可以配置在发送或接收数据包时如何在 ALB 上对数据进行缓冲。  
{: shortdesc}

### 大型客户机头缓冲区 (`large-client-header-buffers`)
{: #large-client-header-buffers}

设置读取大型客户机请求头的缓冲区的最大数目和大小。
{:shortdesc}

**描述**</br>
读取大型客户机请求头的缓冲区仅按需进行分配：如果在请求结尾处理后连接已转换为保持活动状态，那么将释放这些缓冲区。缺省情况下，有 `4` 个缓冲区，缓冲区大小等于 `8K` 字节。如果请求行超过一个缓冲区的设定最大大小，那么会向客户机返回 `414 Request-URI Too Large` HTTP 错误。此外，如果请求头字段超过一个缓冲区的设定最大大小，那么会向客户机返回 `400 Bad Request` 错误。可以调整用于读取大型客户机请求头的缓冲区的最大数目和大小。

**样本 Ingress 资源 YAML**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/large-client-header-buffers: "number=<number> size=<size>"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080
```
{: codeblock}

<table>
<caption>了解注释的组成部分</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
 </thead>
 <tbody>
 <tr>
 <td><code>&lt;number&gt;</code></td>
 <td>应该为读取大型客户机请求头而分配的最大缓冲区数。例如，要将其设置为 4，请定义 <code>4</code>。</td>
 </tr>
 <tr>
 <td><code>&lt;size&gt;</code></td>
 <td>读取大型客户机请求头的缓冲区的最大大小。例如，要将其设置为 16 千字节，请定义 <code>16k</code>。
   对于千字节，大小必须以 <code>k</code> 结尾，对于兆字节，必须以 <code>m</code> 结尾。</td>
 </tr>
</tbody></table>

<br />


### 客户机响应数据缓冲 (`proxy-buffering`)
{: #proxy-buffering}

使用缓冲注释可禁止在将数据发送到客户机期间，在 ALB 上存储响应数据。
{:shortdesc}

**描述**</br>
Ingress ALB 充当后端应用程序和客户机 Web 浏览器之间的代理。当响应从后端应用程序发送到客户机时，缺省情况下会在 ALB 上对响应数据进行缓冲。ALB 作为代理传递客户机响应，并开始按客户机的速度将响应发送到客户机。ALB 接收到来自后端应用程序的所有数据后，会关闭与后端应用程序的连接。ALB 与客户机的连接会保持打开状态，直到客户机接收完所有数据为止。

如果在 ALB 上禁用响应数据缓冲，那么数据会立即从 ALB 发送到客户机。客户机必须能够按 ALB 的速度来处理入局数据。如果客户机速度太慢，上游连接会保持打开状态，直到客户机能够赶上。

缺省情况下，ALB 上启用了响应数据缓冲。

**样本 Ingress 资源 YAML**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/proxy-buffering: "enabled=<false> serviceName=<myservice1>"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080
```
{: codeblock}

<table>
<caption>了解注释的组成部分</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
</thead>
<tbody>
<tr>
<td><code>enabled</code></td>
<td>要禁用 ALB 上的响应数据缓冲，请设置为 <code>false</code>。</td>
</tr>
<tr>
<td><code>serviceName</code></td>
<td>将 <code><em>&lt;myservice1&gt;</em></code> 替换为您针对应用程序创建的 Kubernetes 服务的名称。使用分号 (;) 分隔多个服务。此字段是可选的。如果未指定服务名称，那么所有服务都将使用此注释。</td>
</tr>
</tbody></table>

<br />


### 代理缓冲区 (`proxy-buffers`)
{: #proxy-buffers}

为 ALB 配置代理缓冲区的数目和大小。
{:shortdesc}

**描述**</br>
设置缓冲区的数目和大小，这些缓冲区用于读取来自通过代理传递的服务器的单个连接的响应。除非指定了服务，否则配置将应用于 Ingress 子域中的所有服务。例如，如果指定如 `serviceName=SERVICE number=2 size=1k` 这样的配置，那么会对该服务应用 1k。如果指定如 `number=2 size=1k` 这样的配置，那么会对 Ingress 子域中的所有服务应用 1k。</br>
<p class="tip">如果收到错误消息 `upstream sent too big header while reading response header from upstream`，说明后端中的上游服务器发送的头大小大于缺省限制。请增大 `proxy-buffers` 和 [`proxy-buffer-size`](#proxy-buffer-size) 的大小。</p>

**样本 Ingress 资源 YAML**</br>

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: proxy-ingress
 annotations:
   ingress.bluemix.net/proxy-buffers: "serviceName=<myservice> number=<number_of_buffers> size=<size>"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080
```
{: codeblock}

<table>
<caption>了解注释的组成部分</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>将 <code>&lt;<em>myservice</em>&gt;</code> 替换为要应用 proxy-buffers 的服务的名称。</td>
</tr>
<tr>
<td><code>number</code></td>
<td>将 <code>&lt;<em>number_of_buffers</em>&gt;</code> 替换为数字，例如 <code>2</code>。</td>
</tr>
<tr>
<td><code>size</code></td>
<td>将 <code>&lt;<em>size</em>&gt;</code> 替换为每个缓冲区的大小（以千字节（k 或 K）为单位），例如 <code>1K</code>。</td>
</tr>
</tbody>
</table>

<br />


### 代理缓冲区大小 (`proxy-buffer-size`)
{: #proxy-buffer-size}

配置代理缓冲区的大小，该缓冲区用于读取响应的第一部分。
{:shortdesc}

**描述**</br>
设置缓冲区的大小，该缓冲区用于读取从通过代理传递的服务器收到的响应的第一部分。响应的这一部分通常包含小型响应头。除非指定了服务，否则配置将应用于 Ingress 子域中的所有服务。例如，如果指定如 `serviceName=SERVICE size=1k` 这样的配置，那么会对该服务应用 1k。如果指定如 `size=1k` 这样的配置，那么会对 Ingress 子域中的所有服务应用 1k。


如果收到错误消息 `upstream sent too big header while reading response header from upstream`，说明后端中的上游服务器发送的头大小大于缺省限制。请增大 `proxy-buffer-size` 和 [`proxy-buffers`](#proxy-buffers) 的大小。
{: tip}

**样本 Ingress 资源 YAML**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: proxy-ingress
 annotations:
   ingress.bluemix.net/proxy-buffer-size: "serviceName=<myservice> size=<size>"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080
```
{: codeblock}

<table>
<caption>了解注释的组成部分</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>将 <code>&lt;<em>myservice</em>&gt;</code> 替换为要应用 proxy-buffers-size 的服务的名称。</td>
</tr>
<tr>
<td><code>size</code></td>
<td>将 <code>&lt;<em>size</em>&gt;</code> 替换为每个缓冲区的大小（以千字节（k 或 K）为单位），例如 <code>1K</code>。要计算正确的大小，可以查看[此博客帖子 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.getpagespeed.com/server-setup/nginx/tuning-proxy_buffer_size-in-nginx)。</td>
</tr>
</tbody></table>

<br />


### 代理繁忙缓冲区大小 (`proxy-busy-buffers-size`)
{: #proxy-busy-buffers-size}

配置可以处于繁忙状态的代理缓冲区的大小。
{:shortdesc}

**描述**</br>
限制在尚未完全读取响应期间，向客户机发送响应的任何缓冲区的大小。在此期间，其余缓冲区可用于读取响应，并可以根据需要将响应的一部分缓冲到临时文件。除非指定了服务，否则配置将应用于 Ingress 子域中的所有服务。例如，如果指定如 `serviceName=SERVICE size=1k` 这样的配置，那么会对该服务应用 1k。如果指定如 `size=1k` 这样的配置，那么会对 Ingress 子域中的所有服务应用 1k。


**样本 Ingress 资源 YAML**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: proxy-ingress
 annotations:
   ingress.bluemix.net/proxy-busy-buffers-size: "serviceName=<myservice> size=<size>"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080
         ```
{: codeblock}

<table>
<caption>了解注释的组成部分</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>将 <code>&lt;<em>myservice</em>&gt;</code> 替换为要应用 proxy-busy-buffers-size 的服务的名称。</td>
</tr>
<tr>
<td><code>size</code></td>
<td>将 <code>&lt;<em>size</em>&gt;</code> 替换为每个缓冲区的大小（以千字节（k 或 K）为单位），例如 <code>1K</code>。</td>
</tr>
</tbody></table>

<br />


## 请求和响应注释
{: #request-response}

使用请求和响应注释可在客户机和服务器请求中添加或除去头信息，以及更改客户机可以发送的主体的大小。
{: shortdesc}

### 将服务器端口添加到主机头 (`add-host-port`)
{: #add-host-port}

向客户机请求添加服务器端口，然后将该请求转发到后端应用程序。
{: shortdesc}

**描述**</br>
将 `:server_port` 添加到客户机请求的主机头，然后将该请求转发到后端应用程序。

**样本 Ingress 资源 YAML**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/add-host-port: "enabled=<true> serviceName=<myservice>"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080
```
{: codeblock}

<table>
<caption>了解注释的组成部分</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
</thead>
<tbody>
<tr>
<td><code>enabled</code></td>
<td>要启用子域的 server_port 设置，请设置为 <code>true</code>。</td>
</tr>
<tr>
<td><code>serviceName</code></td>
<td>将 <code><em>&lt;myservice&gt;</em></code> 替换为您针对应用程序创建的 Kubernetes 服务的名称。使用分号 (;) 分隔多个服务。此字段是可选的。如果未指定服务名称，那么所有服务都将使用此注释。</td>
</tr>
</tbody></table>

<br />


### 额外的客户机请求或响应头（`proxy-add-headers` 和 `response-add-headers`）
{: #proxy-add-headers}

向客户机请求添加额外的头信息，然后将该请求发送到后端应用程序，或者向客户机响应添加额外的头信息，然后将该响应发送到客户机。
{:shortdesc}

**描述**</br>
Ingress ALB 充当客户机应用程序和后端应用程序之间的代理。发送到 ALB 的客户机请求经过处理（通过代理传递）后放入新的请求中，然后将该新请求发送到后端应用程序。与此类似，发送到 ALB 的后端应用程序响应经过处理（通过代理传递）后放入新的响应中，然后将该新响应发送到客户机。通过代理传递请求或响应会除去初始从客户机或后端应用程序发送的 HTTP 头信息，例如用户名。

如果后端应用程序需要 HTTP 头信息，那么可以使用 `proxy-add-headers` 注释向客户机请求添加头信息，然后由 ALB 将该请求转发到后端应用程序。如果客户机 Web 应用程序需要 HTTP 头信息，那么可以使用 `response-add-headers` 注释将头信息添加到响应，然后由 ALB 将响应转发到客户机 Web 应用程序。<br>

例如，在将请求转发到应用程序之前，可能需要将以下 X-Forward 头信息添加到请求中：
```
proxy_set_header Host $host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-Proto $scheme;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
```
{: screen}

要将 X-Forward 头信息添加到发送到应用程序的请求中，请按以下方式使用 `proxy-add-headers` 注释：

```
ingress.bluemix.net/proxy-add-headers: |
  serviceName=<myservice1> {
  Host $host;
  X-Real-IP $remote_addr;
  X-Forwarded-Proto $scheme;
  X-Forwarded-For $proxy_add_x_forwarded_for;
  }
```
{: codeblock}

</br>

`response-add-headers` 注释不支持用于所有服务的全局头。要在服务器级别添加用于所有服务响应的头，可以使用 [`server-snippets` 注释](#server-snippets)：
{: tip}
```
annotations:
  ingress.bluemix.net/server-snippets: |
    add_header <header1> <value1>;
```
{: pre}
</br>

**样本 Ingress 资源 YAML**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/proxy-add-headers: |
      serviceName=<myservice1> {
      <header1> <value1>;
      <header2> <value2>;
      }
      serviceName=<myservice2> {
      <header3> <value3>;
      }
    ingress.bluemix.net/response-add-headers: |
      serviceName=<myservice1> {
      <header1>:<value1>;
      <header2>:<value2>;
      }
      serviceName=<myservice2> {
      <header3>:<value3>;
      }
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /service1_path
        backend:
          serviceName: <myservice1>
          servicePort: 8080
      - path: /service2_path
        backend:
          serviceName: <myservice2>
          servicePort: 80
```
{: codeblock}

<table>
<caption>了解注释的组成部分</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
</thead>
<tbody>
<tr>
<td><code>service_name</code></td>
<td>将 <code>&lt;<em>myservice</em>&gt;</code> 替换为您针对应用程序创建的 Kubernetes 服务的名称。</td>
</tr>
<tr>
<td><code>&lt;header&gt;</code></td>
<td>要添加到客户机请求或客户机响应的头信息的键。</td>
</tr>
<tr>
<td><code>&lt;value&gt;</code></td>
<td>要添加到客户机请求或客户机响应的头信息的值。</td>
</tr>
</tbody></table>

<br />


### 除去客户机响应头 (`response-remove-headers`)
{: #response-remove-headers}

除去后端应用程序的客户机响应中包含的头信息，然后将该响应发送到客户机。
{:shortdesc}

**描述**</br>
Ingress ALB 充当后端应用程序和客户机 Web 浏览器之间的代理。发送到 ALB 的来自后端应用程序的客户机响应经过处理（通过代理传递）后放入新的响应中，然后将该新响应从 ALB 发送到客户机 Web 浏览器。尽管通过代理传递响应会除去初始从后端应用程序发送的 HTTP 头信息，但此过程可能不会除去所有特定于后端应用程序的头。从客户机响应中除去头信息，然后将该响应从 ALB 转发到客户机 Web 浏览器。

**样本 Ingress 资源 YAML**</br>

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/response-remove-headers: |
      serviceName=<myservice1> {
      "<header1>";
      "<header2>";
      }
      serviceName=<myservice2> {
      "<header3>";
      }
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /service1_path
        backend:
          serviceName: <myservice1>
          servicePort: 8080
      - path: /service2_path
        backend:
          serviceName: <myservice2>
          servicePort: 80
```
{: codeblock}

<table>
<caption>了解注释的组成部分</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
</thead>
<tbody>
<tr>
<td><code>service_name</code></td>
<td>将 <code>&lt;<em>myservice</em>&gt;</code> 替换为您针对应用程序创建的 Kubernetes 服务的名称。</td>
</tr>
<tr>
<td><code>&lt;header&gt;</code></td>
<td>要从客户机响应中除去的头的键。</td>
</tr>
</tbody></table>

<br />


### 客户机请求主体大小 (`client-max-body-size`)
{: #client-max-body-size}

设置客户机可以作为请求一部分发送的主体的最大大小。
{:shortdesc}

**描述**</br>
为了保持期望的性能，最大客户机请求主体大小设置为 1 兆字节。将主体大小超出此限制的客户机请求发送到 Ingress ALB，并且客户机不允许拆分数据时，ALB 会向客户机返回 413（请求实体太大）HTTP 响应。除非减小请求主体的大小，否则无法在客户机和 ALB 之间建立连接。客户机允许数据拆分为多个区块时，数据将分成 1 兆字节的包并发送到 ALB。

您可能希望增大最大主体大小，因为您预期客户机请求的主体大小大于 1 兆字节。例如，您希望客户机能够上传大型文件。增大最大请求主体大小可能会影响 ALB 的性能，因为与客户机的连接必须保持打开状态，直到接收完请求为止。


某些客户机 Web 浏览器无法正确显示 413 HTTP 响应消息。
{: note}

**样本 Ingress 资源 YAML**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/client-max-body-size: "serviceName=<myservice> size=<size>; size=<size>"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080
```
{: codeblock}

<table>
<caption>了解注释的组成部分</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>可选：要将客户机最大主体大小应用于特定服务，请将 <code>&lt;<em>myservice</em>&gt;</code> 替换为服务的名称。如果未指定服务名称，那么会将大小应用于所有服务。在示例 YAML 中，格式 <code>"serviceName=&lt;myservice&gt; size=&lt;size&gt;; size=&lt;size&gt;"</code> 会将第一个大小应用于 <code>myservice</code> 服务，将第二个大小应用于所有其他服务。</li>
</tr>
<td><code>&lt;size&gt;</code></td>
<td>客户机响应主体的最大大小。例如，要将最大大小设置为 200 兆字节，请定义 <code>200m</code>。可以将大小设置为 0 以禁止检查客户机请求主体大小。</td>
</tr>
</tbody></table>

<br />


## 服务限制注释
{: #service-limit}

通过使用服务限制注释，可以更改可来自单个 IP 地址的缺省请求处理速率和连接数。
{: shortdesc}

### 全局速率限制 (`global-rate-limit`)
{: #global-rate-limit}

对于所有服务，按定义的键限制请求处理速率和连接数。
{:shortdesc}

**描述**</br>
对于所有服务，限制来自所选后端中所有路径的单个 IP 地址的请求处理速率和每个定义的键的连接数。


**样本 Ingress 资源 YAML**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/global-rate-limit: "key=<key> rate=<rate> conn=<number-of-connections>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080
```
{: codeblock}

<table>
<caption>了解注释的组成部分</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
</thead>
<tbody>
<tr>
<td><code>key</code></td>
<td>支持的值为 `location`、`$http_` 头和 `$uri`。要基于专区或服务来设置入局请求的全局限制，请使用 `key=location`。要基于头来设置入局请求的全局限制，请使用 `X-USER-ID key=$http_x_user_id`。</td>
</tr>
<tr>
<td><code>rate</code></td>
<td>将 <code>&lt;<em>rate</em>&gt;</code> 替换为处理速率。输入以每秒速率 (r/s) 或每分钟速率 (r/m) 为单位的值。示例：<code>50r/m</code>。</td>
</tr>
<tr>
<td><code>conn</code></td>
<td>将 <code>&lt;<em>number-of-connections</em>&gt;</code> 替换为连接数。</td>
</tr>
</tbody></table>

<br />


### 服务速率限制 (`service-rate-limit`)
{: #service-rate-limit}

限制特定服务的请求处理速率和连接数。
{:shortdesc}

**描述**</br>
对于特定服务，限制来自所选后端中所有路径的单个 IP 地址的请求处理速率和每个定义的键的连接数。


**样本 Ingress 资源 YAML**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/service-rate-limit: "serviceName=<myservice> key=<key> rate=<rate> conn=<number_of_connections>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080
```
{: codeblock}

<table>
<caption>了解注释的组成部分</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>将 <code>&lt;<em>myservice</em>&gt;</code> 替换为要限制其处理速率的服务的名称。</li>
</tr>
<tr>
<td><code>key</code></td>
<td>支持的值为 `location`、`$http_` 头和 `$uri`。要基于专区或服务来设置入局请求的全局限制，请使用 `key=location`。要基于头来设置入局请求的全局限制，请使用 `X-USER-ID key=$http_x_user_id`。</td>
</tr>
<tr>
<td><code>rate</code></td>
<td>将 <code>&lt;<em>rate</em>&gt;</code> 替换为处理速率。要定义每秒速率，请使用 r/s：<code>10r/s</code>。要定义每分钟速率，请使用 r/m：<code>50r/m</code>。</td>
</tr>
<tr>
<td><code>conn</code></td>
<td>将 <code>&lt;<em>number-of-connections</em>&gt;</code> 替换为连接数。</td>
</tr>
</tbody></table>

<br />


## 用户认证注释
{: #user-authentication}

如果要使用 {{site.data.keyword.appid_full_notm}} 向应用程序进行认证，请使用用户认证注释。
{: shortdesc}

### {{site.data.keyword.appid_short_notm}} 认证 (`appid-auth`)
{: #appid-auth}

使用 {{site.data.keyword.appid_full_notm}} 向应用程序进行认证。
{:shortdesc}

**描述**</br>
使用 {{site.data.keyword.appid_short_notm}} 对 Web 或 API HTTP/HTTPS 请求进行认证。

如果将请求类型设置为 Web，那么将验证包含 {{site.data.keyword.appid_short_notm}} 访问令牌的 Web 请求。如果令牌验证失败，将拒绝 Web 请求。如果请求不包含访问令牌，那么会将请求重定向到 {{site.data.keyword.appid_short_notm}} 登录页面。要使 {{site.data.keyword.appid_short_notm}} Web 认证能够正常运作，必须在用户的浏览器中启用 cookie。

如果将请求类型设置为 API，那么将验证包含 {{site.data.keyword.appid_short_notm}} 访问令牌的 API 请求。如果请求不包含访问令牌，将向用户返回“401：未授权”错误消息。

出于安全原因，{{site.data.keyword.appid_short_notm}} 认证仅支持启用了 TLS/SSL 的后端。
{: note}

**样本 Ingress 资源 YAML**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/appid-auth: "bindSecret=<bind_secret> namespace=<namespace> requestType=<request_type> serviceName=<myservice> [idToken=false]"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080
```
{: codeblock}

<table>
<caption>了解注释的组成部分</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
</thead>
<tbody>
<tr>
<td><code>bindSecret</code></td>
<td>将 <em><code>&lt;bind_secret&gt;</code></em> 替换为存储 {{site.data.keyword.appid_short_notm}} 服务实例的绑定私钥的 Kubernetes 私钥。</td>
</tr>
<tr>
<td><code>namespace</code></td>
<td>将 <em><code>&lt;namespace&gt;</code></em> 替换为绑定私钥的名称空间。此字段缺省为 `default` 名称空间。</td>
</tr>
<tr>
<td><code>requestType</code></td>
<td>将 <code><em>&lt;request_type&gt;</em></code> 替换为要发送到 {{site.data.keyword.appid_short_notm}} 的请求的类型。接受的值为 `web` 或 `api`。缺省值为 `api`。</td>
</tr>
<tr>
<td><code>serviceName</code></td>
<td>将 <code><em>&lt;myservice&gt;</em></code> 替换为您针对应用程序创建的 Kubernetes 服务的名称。这是必填字段。如果未包含服务名称，那么将对所有服务启用注释。如果包含服务名称，那么仅对该服务启用注释。使用逗号 (,) 分隔多个服务。</td>
</tr>
<tr>
<td><code>idToken=false</code></td>
<td>可选：Liberty OIDC 客户机无法同时解析访问权和身份令牌。使用 Liberty 时，请将此值设置为 false，以便身份令牌不会发送到 Liberty 服务器。</td>
</tr>
</tbody></table>

**用法**</br>

由于应用程序使用 {{site.data.keyword.appid_short_notm}} 进行认证，因此必须供应 {{site.data.keyword.appid_short_notm}} 实例，使用有效的重定向 URI 来配置实例，并通过将实例绑定到集群来生成绑定私钥。

1. 选择现有或创建新的 {{site.data.keyword.appid_short_notm}} 实例。
  * 要使用现有实例，请确保服务实例名称不包含空格。要除去空格，请选择服务实例名称旁边的“更多选项”菜单，然后选择**重命名服务**。
  * 要供应[新 {{site.data.keyword.appid_short_notm}} 实例](https://cloud.ibm.com/catalog/services/app-id)，请执行以下操作：
      1. 将自动填充的**服务名称**替换为您自己的服务实例唯一名称。
            服务实例名称不能包含空格。
      2. 选择部署了您的集群的区域。
      3. 单击**创建**。

2. 添加应用程序的重定向 URL。重定向 URL 是应用程序的回调端点。要防止钓鱼攻击，应用程序标识将根据重定向 URL 的白名单来验证请求 URL。
  1. 在 {{site.data.keyword.appid_short_notm}} 管理控制台中，导航至**管理认证**。
  2. 在**身份提供者**选项卡中，确保已选择身份提供者。如果未选择身份提供者，那么不会对用户进行认证，但会向用户颁发访问令牌以供匿名访问应用程序。
  3. 在**认证设置**选项卡中，以 `http://<hostname>/<app_path>/appid_callback` 或 `https://<hostname>/<app_path>/appid_callback` 格式添加应用程序的重定向 URL。

    {{site.data.keyword.appid_full_notm}} 提供了注销功能：如果 {{site.data.keyword.appid_full_notm}} 路径中存在 `/logout`，那么将除去 cookie 并将用户发送回登录页面。要使用此功能，必须将 `/appid_logout` 附加到域，格式为 `https://<hostname>/<app_path>/appid_logout`，并在重定向 URL 列表中包含此 URL。
    {: note}

3. 将 {{site.data.keyword.appid_short_notm}} 服务实例与集群绑定。
    以下命令将为服务实例创建服务密钥，或者可以包含 `--key` 标志以使用现有服务密钥凭证。
  ```
    ibmcloud ks cluster-service-bind --cluster <cluster_name_or_ID> --namespace <namespace> --service <service_instance_name> [--key <service_instance_key>]
    ```
  {: pre}
服务成功添加到集群后，将创建集群私钥，用于保存服务实例的凭证。示例 CLI 输出：
    ```
    ibmcloud ks cluster-service-bind --cluster mycluster --namespace mynamespace --service appid1
    Binding service instance to namespace...
    OK
    Namespace:    mynamespace
    Secret name:  binding-<service_instance_name>
    ```
  {: screen}

4. 获取在集群名称空间中创建的私钥。
    
  ```
    kubectl get secrets --namespace=<namespace>
    ```
  {: pre}

5. 使用绑定私钥和集群名称空间，将 `appid-auth` 注释添加到 Ingress 资源。


