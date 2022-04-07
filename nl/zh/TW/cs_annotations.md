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



# 使用註釋來自訂 Ingress
{: #ingress_annotation}

若要將功能新增至 Ingress 應用程式負載平衡器 (ALB)，您可以將註釋指定為 Ingress 資源中的 meta 資料。
{: shortdesc}

在使用註釋之前，請遵循[使用 Ingress 應用程式負載平衡器 (ALB) 的 HTTPS 負載平衡](/docs/containers?topic=containers-ingress)中的步驟，確定已適當地設定 Ingress 服務配置。使用基本配置設定 Ingress ALB 之後，接著可以將註釋新增至 Ingress 資源檔，來擴充其功能。
{: note}

<table>
<caption>一般註釋</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>一般註釋</th>
<th>名稱</th>
<th>說明</th>
</thead>
<tbody>
<tr>
<td><a href="#custom-errors">自訂錯誤動作</a></td>
<td><code>custom-errors, custom-error-actions</code></td>
<td>指出 ALB 可對特定 HTTP 錯誤採取的自訂動作。</td>
</tr>
<tr>
<td><a href="#location-snippets">位置 Snippet</a></td>
<td><code>location-snippets</code></td>
<td>新增服務的自訂位置區塊配置。</td>
</tr>
<tr>
<td><a href="#alb-id">專用 ALB 遞送</a></td>
<td><code>ALB-ID</code></td>
<td>使用專用 ALB，將送入要求遞送至應用程式。</td>
</tr>
<tr>
<td><a href="#server-snippets">伺服器 Snippet</a></td>
<td><code>server-snippets</code></td>
<td>新增自訂伺服器區塊配置。</td>
</tr>
</tbody></table>

<br>

<table>
<caption>連線註釋</caption>
<col width="20%">
<col width="20%">
<col width="60%">
 <thead>
 <th>連線註釋</th>
 <th>名稱</th>
 <th>說明</th>
  </thead>
  <tbody>
  <tr>
  <td><a href="#proxy-connect-timeout">自訂連接逾時和讀取逾時</a></td>
  <td><code>proxy-connect-timeout、proxy-read-timeout</code></td>
  <td>設定 ALB 等待連接及讀取後端應用程式的時間，在此時間之後，後端應用程式將被視為無法使用。</td>
  </tr>
  <tr>
  <td><a href="#keepalive-requests">保留作用中要求</a></td>
  <td><code>keepalive-requests</code></td>
  <td>設定可以透過一個保留作用中連線服務的要求數目上限。</td>
  </tr>
  <tr>
  <td><a href="#keepalive-timeout">保留作用中逾時</a></td>
  <td><code>keepalive-timeout</code></td>
  <td>設定保留作用中連線在伺服器上保持開啟的時間上限。</td>
  </tr>
  <tr>
  <td><a href="#proxy-next-upstream-config">Proxy 下一個上游</a></td>
  <td><code>proxy-next-upstream-config</code></td>
  <td>設定 ALB 可以將要求傳遞給下一個上游伺服器的時間。</td>
  </tr>
  <tr>
  <td><a href="#sticky-cookie-services">Cookie 的階段作業親緣性</a></td>
  <td><code>sticky-cookie-services</code></td>
  <td>使用 Sticky Cookie 一律將送入的網路資料流量遞送至相同的上游伺服器。</td>
  </tr>
  <tr>
  <td><a href="#upstream-fail-timeout">上游失敗逾時</a></td>
  <td><code>upstream-fail-timeout</code></td>
  <td>設定時間量，在此期間，ALB 可以先嘗試連接至伺服器，然後才將伺服器視為無法使用。</td>
  </tr>
  <tr>
  <td><a href="#upstream-keepalive">上游保留作用中</a></td>
  <td><code>upstream-keepalive</code></td>
  <td>設定上游伺服器之閒置保留作用中連線的數目上限。</td>
  </tr>
  <tr>
  <td><a href="#upstream-max-fails">上游失敗次數上限</a></td>
  <td><code>upstream-max-fails</code></td>
  <td>設定與伺服器通訊的不成功嘗試次數上限，在此次數之後，會將伺服器視為無法使用。</td>
  </tr>
  </tbody></table>

<br>

  <table>
  <caption>HTTPS 及 TLS/SSL 鑑別註釋</caption>
  <col width="20%">
  <col width="20%">
  <col width="60%">
  <thead>
  <th>HTTPS 及 TLS/SSL 鑑別註釋</th>
  <th>名稱</th>
  <th>說明</th>
  </thead>
  <tbody>
  <tr>
  <td><a href="#custom-port">自訂 HTTP 及 HTTPS 埠</a></td>
  <td><code>custom-port</code></td>
  <td>變更 HTTP 及 HTTPS 網路資料流量的預設埠，HTTP 為埠 80，HTTPS 為埠 443。
</td>
  </tr>
  <tr>
  <td><a href="#redirect-to-https">HTTP 重新導向至 HTTPS</a></td>
  <td><code>redirect-to-https</code></td>
  <td>將您網域上不安全的 HTTP 要求重新導向 HTTPS。</td>
  </tr>
  <tr>
  <td><a href="#hsts">HTTP 強制安全傳輸技術 (HSTS)</a></td>
  <td><code>hsts</code></td>
  <td>將瀏覽器設為僅使用 HTTPS 來存取網域。</td>
  </tr>
  <tr>
  <td><a href="#mutual-auth">交互鑑別</a></td>
  <td><code>mutual-auth</code></td>
  <td>配置 ALB 的交互鑑別。</td>
  </tr>
  <tr>
  <td><a href="#ssl-services">SSL 服務支援</a></td>
  <td><code>ssl-services</code></td>
  <td>容許 SSL 服務支援將要送至需要 HTTPS 之上游應用程式的資料流量加密。</td>
  </tr>
  <tr>
  <td><a href="#tcp-ports">TCP 埠</a></td>
  <td><code>tcp-ports</code></td>
  <td>透過非標準 TCP 埠存取應用程式。</td>
  </tr>
  </tbody></table>

<br>

<table>
<caption>路徑遞送註釋</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>路徑遞送註釋</th>
<th>名稱</th>
<th>說明</th>
</thead>
<tbody>
<tr>
<td><a href="#proxy-external-service">外部服務</a></td>
<td><code>proxy-external-service</code></td>
<td>將路徑定義新增至外部服務，例如 {{site.data.keyword.Bluemix_notm}} 中管理的服務。</td>
</tr>
<tr>
<td><a href="#location-modifier">位置修飾元</a></td>
<td><code>location-modifier</code></td>
<td>修改 ALB 比對要求 URI 與應用程式路徑的方式。</td>
</tr>
<tr>
<td><a href="#rewrite-path">重寫路徑</a></td>
<td><code>rewrite-path</code></td>
<td>將送入的網路資料流量遞送至後端應用程式接聽的不同路徑。</td>
</tr>
</tbody></table>

<br>

<table>
<caption>Proxy 緩衝區註釋</caption>
<col width="20%">
<col width="20%">
<col width="60%">
 <thead>
 <th>Proxy 緩衝區註釋</th>
 <th>名稱</th>
 <th>說明</th>
 </thead>
 <tbody>
 <tr>
<td><a href="#large-client-header-buffers">大型用戶端標頭緩衝區</a></td>
<td><code>large-client-header-buffers</code></td>
<td>設定讀取大型用戶端要求標頭的緩衝區數目上限及大小。</td>
</tr>
 <tr>
 <td><a href="#proxy-buffering">用戶端回應資料緩衝</a></td>
 <td><code>proxy-buffering</code></td>
 <td>將回應傳送至用戶端時，停用 ALB 上的用戶端回應緩衝處理。</td>
 </tr>
 <tr>
 <td><a href="#proxy-buffers">Proxy 緩衝區</a></td>
 <td><code>proxy-buffers</code></td>
 <td>針對來自被 Proxy 處理之伺服器的單一連線，設定讀取回應的緩衝區數目及大小。</td>
 </tr>
 <tr>
 <td><a href="#proxy-buffer-size">Proxy 緩衝區大小</a></td>
 <td><code>proxy-buffer-size</code></td>
 <td>設定緩衝區大小，以讀取從被 Proxy 處理之伺服器所收到回應的第一部分。</td>
 </tr>
 <tr>
 <td><a href="#proxy-busy-buffers-size">Proxy 工作中緩衝區大小</a></td>
 <td><code>proxy-busy-buffers-size</code></td>
 <td>設定可以在忙碌中的 Proxy 緩衝區大小。</td>
 </tr>
 </tbody></table>

<br>

<table>
<caption>要求及回應註釋</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>要求及回應註釋</th>
<th>名稱</th>
<th>說明</th>
</thead>
<tbody>
<tr>
<td><a href="#add-host-port">將伺服器埠新增至主機標頭</a></td>
<td><code>add-host-port</code></td>
<td>將伺服器埠新增至主機，以遞送要求。</td>
</tr>
<tr>
<td><a href="#client-max-body-size">用戶端要求內文大小</a></td>
<td><code>client-max-body-size</code></td>
<td>設定用戶端在要求中所能傳送的內文大小上限。</td>
</tr>
<tr>
<td><a href="#proxy-add-headers">額外的用戶端要求或回應標頭</a></td>
<td><code>proxy-add-headers、response-add-headers</code></td>
<td>將標頭資訊新增至用戶端要求，然後再將要求轉遞至後端應用程式，或是新增至用戶端回應，然後再將回應傳送至用戶端。</td>
</tr>
<tr>
<td><a href="#response-remove-headers">移除用戶端回應標頭</a></td>
<td><code>response-remove-headers</code></td>
<td>從用戶端回應移除標頭資訊，然後才將回應轉遞至用戶端。</td>
</tr>
</tbody></table>

<br>

<table>
<caption>服務限制註釋</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>服務限制註釋</th>
<th>名稱</th>
<th>說明</th>
</thead>
<tbody>
<tr>
<td><a href="#global-rate-limit">廣域速率限制</a></td>
<td><code>global-rate-limit</code></td>
<td>針對所有服務的已定義金鑰，限制要求處理速率及連線數。</td>
</tr>
<tr>
<td><a href="#service-rate-limit">服務速率限制</a></td>
<td><code>service-rate-limit</code></td>
<td>針對特定服務的已定義金鑰，限制要求處理速率及連線數。</td>
</tr>
</tbody></table>

<br>

<table>
<caption>使用者鑑別註釋</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>使用者鑑別註釋</th>
<th>名稱</th>
<th>說明</th>
</thead>
<tbody>
<tr>
<td><a href="#appid-auth">{{site.data.keyword.appid_short}} 鑑別</a></td>
<td><code>appid-auth</code></td>
<td>使用 {{site.data.keyword.appid_full}} 向您的應用程式進行鑑別。</td>
</tr>
</tbody></table>

<br>

## 一般註釋
{: #general}

### 自訂錯誤動作（`custom-errors`、`custom-error-actions`）
{: #custom-errors}

指出 ALB 可對特定 HTTP 錯誤採取的自訂動作。
{: shortdesc}

**說明**</br>
若要處理可能發生的特定 HTTP 錯誤，您可以設定 ALB 要採取的自訂錯誤動作。

* `custom-errors` 註釋會定義服務名稱、要處理的 HTTP 錯誤，以及當 ALB 遇到針對該服務所指定的 HTTP 錯誤時要採取的錯誤動作名稱。
* `custom-error-actions` 註釋會在 NGINX 程式碼 Snippet 中定義自訂錯誤動作。

例如，在 `custom-errors` 註釋中，您可以藉由傳回一個稱為 `/errorAction401` 的自訂錯誤動作，而設定 ALB 以處理 `app1` 的 `401` HTTP 錯誤。然後，在 `custom-error-actions` 註釋中，您可以定義一個稱為 `/errorAction401` 的程式碼 Snippet，讓 ALB 將自訂錯誤頁面傳回給用戶端。</br>

您也可以使用 `custom-errors` 註釋，將用戶端重新導向至您所管理的錯誤服務。您必須在 Ingress 資源檔的 `paths` 區段中，定義此錯誤服務的路徑。

**Ingress 資源 YAML 範例**</br>

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
<caption>瞭解註釋元件</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>將 <code>&lt;<em>app1</em>&gt;</code> 取代為自訂錯誤適用的 Kubernetes 服務名稱。自訂錯誤只適用於使用此相同上游服務的特定路徑。如果未設定服務名稱，則自訂錯誤適用於所有服務路徑。</td>
</tr>
<tr>
<td><code>httpError</code></td>
<td>將 <code>&lt;<em>401</em>&gt;</code> 取代為您要以自訂錯誤動作來處理的 HTTP 錯誤碼。</td>
</tr>
<tr>
<td><code>errorActionName</code></td>
<td>將 <code>&lt;<em>/errorAction401</em>&gt;</code> 取代為要採取之自訂錯誤動作的名稱，或是錯誤服務的路徑。<ul>
<li>如果您指定自訂錯誤動作的名稱，則必須在 <code>custom-error-actions</code> 註釋的程式碼 Snippet 中定義該錯誤動作。在 YAML 範例中，<code>app1</code> 會使用 <code>/errorAction401</code>，它定義在 <code>custom-error-actions</code> 註釋的 Snippet 中。</li>
<li>如果您指定錯誤服務的路徑，則必須在 <code>paths</code> 區段中指定錯誤服務的錯誤路徑和名稱。在 YAML 範例中，<code>app2</code> 會使用 <code>/errorPath</code>，它定義在 <code>paths</code> 區段的結尾。</li></ul></td>
</tr>
<tr>
<td><code>ingress.bluemix.net/custom-error-actions</code></td>
<td>定義針對您指定的服務及 HTTP 錯誤，ALB 所要採取的自訂錯誤動作。請使用 NGINX 程式碼 Snippet，並以 <code>&lt;EOS&gt;</code> 作為每一個 Snippet 的結尾。在 YAML 範例中，當 <code>app1</code> 發生 <code>401</code> 錯誤時，ALB 會將自訂錯誤頁面 <code>http://example.com/forbidden.html</code> 傳遞給用戶端。</td>
</tr>
</tbody></table>

<br />


### 位置 Snippet (`location-snippets`)
{: #location-snippets}

新增服務的自訂位置區塊配置。
{:shortdesc}

**說明**</br>
伺服器區塊是一種 NGINX 指引，定義了 ALB 虛擬伺服器的配置。位置區塊是伺服器區塊內定義的 NGINX 指引。位置區塊定義 Ingress 如何處理要求 URI，或在網域名稱或 IP 位址及埠後面的要求部分。

伺服器區塊收到要求時，位置區塊會比對 URI 與路徑，而且要求會轉遞至應用程式部署所在 Pod 的 IP 位址。使用 `location-snippets` 註釋，即可修改位置區塊如何將要求轉遞至特定服務。

若要改為修改整個伺服器區塊，請參閱 [`server-snippets`](#server-snippets) 註釋。

若要檢視 NGINX 配置檔中的伺服器及位置區塊，請針對您的其中一個 ALB Pod 執行下列指令：`kubectl exec -ti <alb_pod> -n kube-system -c nginx-ingress -- cat ./etc/nginx/default-<ingress_resource_name>.conf`
{: tip}

**Ingress 資源 YAML 範例**</br>

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
<caption>瞭解註釋元件</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>將 <code>&lt;<em>myservice</em>&gt;</code> 取代為您為應用程式所建立之服務的名稱。</td>
</tr>
<tr>
<td>位置 Snippet</td>
<td>提供要用於所指定服務的配置 Snippet。<code>myservice1</code> 服務的 Snippet 範例會配置位置區塊，以便在將要求轉遞至服務時，關閉 Proxy 要求緩衝、開啟日誌重寫，以及設定其他標頭。<code>myservice2</code> 服務的 Snippet 範例會設定空的 <code>Authorization</code> 標頭。每一個位置 Snippet 的結尾都必須為 <code>&lt;EOS&gt;</code> 值。</td>
</tr>
</tbody></table>

<br />


### 專用 ALB 遞送 (`ALB-ID`)
{: #alb-id}

使用專用 ALB，將送入要求遞送至應用程式。
{:shortdesc}

**說明**</br>
選擇專用 ALB 來遞送送入要求，而非公用 ALB。

**Ingress 資源 YAML 範例**</br>
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
<caption>瞭解註釋元件</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
</thead>
<tbody>
<tr>
<td><code>&lt;private_ALB_ID&gt;</code></td>
<td>專用 ALB 的 ID。若要尋找專用 ALB ID，請執行 <code>ibmcloud ks albs --cluster &lt;my_cluster&gt;</code>。<p>
如果您的多區域叢集已啟用多個專用 ALB，則可以提供以 <code>;</code> 區隔的 ALB ID 清單。例如：<code>ingress.bluemix.net/ALB-ID: &lt;private_ALB_ID_1&gt;;&lt;private_ALB_ID_2&gt;;&lt;private_ALB_ID_3&gt;</code></p>
</td>
</tr>
</tbody></table>

<br />


### 伺服器 Snippet (`server-snippets`)
{: #server-snippets}

新增自訂伺服器區塊配置。
{:shortdesc}

**說明**</br>
伺服器區塊是一種 NGINX 指引，定義了 ALB 虛擬伺服器的配置。透過在 `server-snippets` 註釋中提供自訂配置 Snippet，您可以修改 ALB 在伺服器層次處理要求的方式。

若要檢視 NGINX 配置檔中的伺服器及位置區塊，請針對您的其中一個 ALB Pod 執行下列指令：`kubectl exec -ti <alb_pod> -n kube-system -c nginx-ingress -- cat ./etc/nginx/default-<ingress_resource_name>.conf`
{: tip}

**Ingress 資源 YAML 範例**</br>

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
<caption>瞭解註釋元件</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
</thead>
<tbody>
<tr>
<td>伺服器 Snippet</td>
<td>提供要使用的配置 Snippet。此 Snippet 範例指定位置區塊來處理 <code>/health</code> 要求。位置區塊配置成在轉遞要求時傳回性能良好的回應，並且新增標頭。</td>
</tr>
</tbody>
</table>

您可以利用 `server-snippets` 註釋，在伺服器層次新增所有服務回應的標頭：
{: tip}

```
annotations:
  ingress.bluemix.net/server-snippets: |
    add_header <header1> <value1>;
```
{: codeblock}

<br />


## 連線註釋
{: #connection}

使用連線註釋，您可以在應用程式或伺服器被視為無法使用之前，變更 ALB 連接至後端應用程式與上游伺服器的方式，以及設定逾時或保留作用中連線的數目上限。
{: shortdesc}

### 自訂連接逾時和讀取逾時（`proxy-connect-timeout`、`proxy-read-timeout`）
{: #proxy-connect-timeout}

設定 ALB 等待連接及讀取後端應用程式的時間，在此時間之後，後端應用程式將被視為無法使用。
{:shortdesc}

**說明**</br>
當用戶端要求傳送至 Ingress ALB 時，ALB 會開啟與後端應用程式的連線。依預設，ALB 會等待 60 秒，以接收來自後端應用程式的回覆。如果後端應用程式未在 60 秒內回覆，則會中斷連線要求，且後端應用程式將被視為無法使用。

在 ALB 連接至後端應用程式之後，ALB 會讀取來自後端應用程式的回應資料。在此讀取作業期間，ALB 在兩次讀取作業之間最多等待 60 秒，以接收來自後端應用程式的資料。如果後端應用程式未在 60 秒內傳送資料，則會關閉與後端應用程式的連線，且應用程式將被視為無法使用。


60 秒的連接逾時及讀取逾時是 Proxy 上的預設逾時，通常不應該加以變更。


如果應用程式的可用性不穩定，或是您的應用程式因為高工作負載而回應太慢，您或許會想增加連接逾時或讀取逾時。請記住，增加逾時值會影響 ALB 的效能，因為與後端應用程式的連線必須維持開啟，直到達到逾時值為止。


另一方面，您可以減少逾時值，以提高 ALB 的效能。請確保您的後端應用程式能夠在指定的逾時內處理要求，即使是在較高工作負載的時段。

**Ingress 資源 YAML 範例**</br>
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
<caption>瞭解註釋元件</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
</thead>
<tbody>
<tr>
<td><code>&lt;connect_timeout&gt;</code></td>
<td>要等待連接至後端應用程式的秒數或分鐘數，例如 <code>65s</code> 或 <code>1m</code>。連接逾時不可超過 75 秒。</td>
</tr>
<tr>
<td><code>&lt;read_timeout&gt;</code></td>
<td>要在讀取後端應用程式之前等待的秒數或分鐘數，例如 <code>65s</code> 或 <code>2m</code>。
 </tr>
</tbody></table>

<br />


### 保留作用中要求 (`keepalive-requests`)
{: #keepalive-requests}

**說明**</br>
設定可以透過一個保留作用中連線提供的要求數目上限。

**Ingress 資源 YAML 範例**</br>
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
<caption>瞭解註釋元件</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>將 <code>&lt;<em>myservice</em>&gt;</code> 取代為您為應用程式所建立之 Kubernetes 服務的名稱。這是選用參數。除非已指定服務，否則此配置適用於 Ingress 子網域中的所有服務。如果提供參數，則會針對給定的服務設定保留作用中要求。如果未提供參數，則會針對未配置保留作用中要求的所有服務，在 <code>nginx.conf</code> 的伺服器層次設定保留作用中要求。</td>
</tr>
<tr>
<td><code>requests</code></td>
<td>將 <code>&lt;<em>max_requests</em>&gt;</code> 取代為可以透過一個保留作用中連線提供的要求數目上限。</td>
</tr>
</tbody></table>

<br />


### 保留作用中逾時 (`keepalive-timeout`)
{: #keepalive-timeout}

**說明**</br>
設定保留作用中連線在伺服器上保持開啟的最長時間。

**Ingress 資源 YAML 範例**</br>
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
<caption>瞭解註釋元件</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>將 <code>&lt;<em>myservice</em>&gt;</code> 取代為您為應用程式所建立之 Kubernetes 服務的名稱。這是選用參數。如果提供參數，則會針對給定的服務設定保留作用中逾時。如果未提供參數，則會針對未配置保留作用中逾時的所有服務，在 <code>nginx.conf</code> 的伺服器層次設定保留作用中逾時。</td>
</tr>
<tr>
<td><code>timeout</code></td>
<td>將 <code>&lt;<em>time</em>&gt;</code> 取代為時間量（以秒為單位）。範例：<code>timeout=20s</code>。<code>0</code> 值會停用保留作用中的用戶端連線。</td>
</tr>
</tbody></table>

<br />


### Proxy 下一個上游 (`proxy-next-upstream-config`)
{: #proxy-next-upstream-config}

設定 ALB 可以將要求傳遞給下一個上游伺服器的時間。
{:shortdesc}

**說明**</br>
Ingress ALB 用來作為用戶端應用程式與您的應用程式之間的 Proxy。部分應用程式設定需要多個上游伺服器，從 ALB 處理送入的用戶端要求。有時，ALB 所使用的 Proxy 伺服器無法與應用程式所使用的上游伺服器建立連線。然後，ALB 可以嘗試與下一個上游伺服器建立連線，以改為將要求傳遞給它。您可以使用 `proxy-next-upstream-config` 註釋，來設定 ALB 可在哪些情況、多久時間及多少次嘗試將要求傳遞給下一個上游伺服器。

當您使用 `proxy-next-upstream-config` 時，一律會配置逾時，因此不要將 `timeout=true` 新增至此註釋。
{: note}

**Ingress 資源 YAML 範例**</br>

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
<caption>瞭解註釋元件</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>將 <code>&lt;<em>myservice</em>&gt;</code> 取代為您為應用程式所建立之 Kubernetes 服務的名稱。</td>
</tr>
<tr>
<td><code>retries</code></td>
<td>將 <code>&lt;<em>tries</em>&gt;</code> 取代為 ALB 嘗試將要求傳遞給下一個上游伺服器的次數上限。此數字包含原始要求。若要關閉此限制，請使用 <code>0</code>。如果未指定值，則會使用預設值 <code>0</code>。</td>
</tr>
<tr>
<td><code>timeout</code></td>
<td>將 <code>&lt;<em>time</em>&gt;</code> 取代為 ALB 嘗試將要求傳遞給下一個上游伺服器的時間量上限（以秒為單位）。例如，若要設定 30 秒的時間，請輸入 <code>30s</code>。若要關閉此限制，請使用 <code>0</code>。如果未指定值，則會使用預設值 <code>0</code>。</td>
</tr>
<tr>
<td><code>error</code></td>
<td>如果設為 <code>true</code>，則在與第一個上游伺服器建立連線、將要求傳遞至其中，或讀取回應標頭時，若發生錯誤，ALB 會將要求傳遞給下一個上游伺服器。
</td>
</tr>
<tr>
<td><code>invalid_header</code></td>
<td>如果設為 <code>true</code>，則當第一個上游伺服器傳回空的或無效的回應時，ALB 會將要求傳遞給下一個上游伺服器。</td>
</tr>
<tr>
<td><code>http_502</code></td>
<td>如果設為 <code>true</code>，則當第一個上游伺服器傳回代碼為 502 的回應時，ALB 會將要求傳遞給下一個上游伺服器。您可以指定下列 HTTP 回應碼：<code>500</code>、<code>502</code>、<code>503</code>、<code>504</code>、<code>403</code>、<code>404</code>、<code>429</code>。</td>
</tr>
<tr>
<td><code>non_idempotent</code></td>
<td>如果設為 <code>true</code>，則 ALB 可以將具有非冪等方法的要求傳遞給下一個上游伺服器。依預設，ALB 不會將這些要求傳遞給下一個上游伺服器。</td>
</tr>
<tr>
<td><code>off</code></td>
<td>若要防止 ALB 將要求傳遞給下一個上游伺服器，請設為 <code>true</code>。
</td>
</tr>
</tbody></table>

<br />


### 使用 Cookie 的階段作業親緣性 (`sticky-cookie-services`)
{: #sticky-cookie-services}

使用 Sticky Cookie 註釋，可為 ALB 增加階段作業親緣性，並一律將送入的網路資料流量遞送至相同的上游伺服器。
{:shortdesc}

**說明**</br>
對於高可用性，部分應用程式設定需要您部署多個上游伺服器來處理送入的用戶端要求。當用戶端連接至您的後端應用程式時，您可以使用階段作業親緣性，以在階段作業時間，或是在完成作業所需的時間內，由相同的上游伺服器服務某一用戶端。您可以配置 ALB，一律將送入的網路資料流量遞送至相同的上游伺服器，以確保階段作業親緣性。

每一個連接至後端應用程式的用戶端，都會由 ALB 指派給其中一個可用的上游伺服器。ALB 會建立一個儲存在用戶端應用程式中的階段作業 Cookie，該 Cookie 內含在 ALB 與用戶端之間每個要求的標頭資訊中。Cookie 中的資訊可確保所有要求在整個階段作業中，都由相同的上游伺服器處理。


仰賴組合階段作業會增加複雜性並降低您的可用性。例如，您可能有一個 HTTP 伺服器維護起始連線的某個階段作業狀態，使 HTTP 服務只接受階段作業狀態值相同的後續要求。不過，這樣會導致 HTTP 服務無法輕鬆進行水平調整。請考量使用外部資料庫（如 Redis 或 Memcached）以儲存 HTTP 要求階段作業值，讓您可以在多部伺服器之間維護階段作業狀態。
{: note}

當您包含多個服務時，請使用分號 (;) 來區隔它們。

**Ingress 資源 YAML 範例**</br>
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
<caption>瞭解註釋元件</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>將 <code>&lt;<em>myservice</em>&gt;</code> 取代為您為應用程式所建立之 Kubernetes 服務的名稱。</td>
</tr>
<tr>
<td><code>name</code></td>
<td>將 <code>&lt;<em>cookie_name</em>&gt;</code> 取代為在階段作業期間建立的 Sticky Cookie 名稱。</td>
</tr>
<tr>
<td><code>expires</code></td>
<td>將 <code>&lt;<em>expiration_time</em>&gt;</code> 取代為 Sticky Cookie 到期之前的時間，以秒 (s)、分鐘 (m) 或小時 (h) 為單位。此時間與使用者活動無關。Cookie 過期之後，用戶端 Web 瀏覽器就會刪除 Cookie，且不會再傳送至 ALB。例如，若要設定 1 秒、1 分鐘或 1 小時的有效期限，請輸入 <code>1s</code>、<code>1m</code> 或 <code>1h</code>。</td>
</tr>
<tr>
<td><code>path</code></td>
<td>將 <code>&lt;<em>cookie_path</em>&gt;</code> 取代為附加至 Ingress 子網域的路徑，且該路徑指出針對哪些網域和子網域而將 Cookie 傳送至 ALB。例如，如果您的 Ingress 網域是 <code>www.myingress.com</code> 且想要在每個用戶端要求中傳送 Cookie，您必須設定 <code>path=/</code>。如果您只想針對 <code>www.myingress.com/myapp</code> 及其所有子網域傳送 Cookie，則必須設定 <code>path=/myapp</code>。</td>
</tr>
<tr>
<td><code>hash</code></td>
<td>將 <code>&lt;<em>hash_algorithm</em>&gt;</code> 取代為保護 Cookie 中資訊的雜湊演算法。僅支援 <code>sha1</code>。SHA1 會根據 Cookie 中的資訊建立雜湊總和，並將此雜湊總和附加到 Cookie。伺服器可以將 Cookie 中的資訊解密，然後驗證資料完整性。</td>
</tr>
</tbody></table>

<br />


### 上游失敗逾時 (`upstream-fail-timeout`)
{: #upstream-fail-timeout}

設定時間量，在此期間，ALB 可以嘗試連接至伺服器。
{:shortdesc}

**說明**</br>
設定時間量，在此期間，ALB 可以先嘗試連接至伺服器，再將伺服器視為無法使用。若要將伺服器視為無法使用，ALB 必須在所設定時間量內達到 [`upstream-max-fails`](#upstream-max-fails) 註釋所設定的連線失敗嘗試次數上限。此時間量也決定了將伺服器視為無法使用的時間。


**Ingress 資源 YAML 範例**</br>
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
<th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
</thead>
<tbody>
<tr>
<td><code>serviceName（選用）</code></td>
<td>將 <code>&lt;<em>myservice</em>&gt;</code> 取代為您為應用程式所建立之 Kubernetes 服務的名稱。</td>
</tr>
<tr>
<td><code>fail-timeout</code></td>
<td>將 <code>&lt;<em>fail_timeout</em>&gt;</code> 取代為 ALB 可以先嘗試連接至伺服器、再將伺服器視為無法使用的時間量。預設值為 <code>10s</code>。時間必須以秒為單位。</td>
</tr>
</tbody></table>

<br />


### 上游保留作用中 (`upstream-keepalive`)
{: #upstream-keepalive}

設定上游伺服器之閒置保留作用中連線的數目上限。
{:shortdesc}

**說明**</br>
設定所給定服務之上游伺服器的閒置保留作用中連線數目上限。依預設，上游伺服器具有 64 個閒置保留作用中連線。


**Ingress 資源 YAML 範例**</br>
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
<caption>瞭解註釋元件</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>將 <code>&lt;<em>myservice</em>&gt;</code> 取代為您為應用程式所建立之 Kubernetes 服務的名稱。</td>
</tr>
<tr>
<td><code>keepalive</code></td>
<td>將 <code>&lt;<em>max_connections</em>&gt;</code> 取代為上游伺服器之閒置保留作用中連線的數目上限。預設值為 <code>64</code>。<code>0</code> 值會停用給定服務的上游保留作用中連線。</td>
</tr>
</tbody></table>

<br />


### 上游失敗次數上限 (`upstream-max-fails`)
{: #upstream-max-fails}

設定與伺服器通訊的不成功嘗試次數上限。
{:shortdesc}

**說明**</br>
設定 ALB 無法連接至伺服器的次數上限，在此次數之後，會將伺服器視為無法使用。若要將伺服器視為無法使用，ALB 必須在此期間內符合 [`upstream-fail-timeout`](#upstream-fail-timeout) 註釋所設定的次數上限。`upstream-fail-timeout` 註釋也會設定伺服器視為無法使用的期間。

**Ingress 資源 YAML 範例**</br>
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
<th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
</thead>
<tbody>
<tr>
<td><code>serviceName（選用）</code></td>
<td>將 <code>&lt;<em>myservice</em>&gt;</code> 取代為您為應用程式所建立之 Kubernetes 服務的名稱。</td>
</tr>
<tr>
<td><code>max-fails</code></td>
<td>將 <code>&lt;<em>max_fails</em>&gt;</code> 取代為 ALB 可進行以與伺服器通訊的不成功嘗試次數上限。預設值為 <code>1</code>。<code>0</code> 值會停用註釋。</td>
</tr>
</tbody></table>

<br />


## HTTPS 及 TLS/SSL 鑑別註釋
{: #https-auth}

使用及 TLS/SSL 鑑別註釋，您可以為 HTTPS 資料流量配置 ALB、變更預設 HTTPS 埠、針對傳送至後端應用程式的資料流量啟用 SSL 加密，或設定交互鑑別。
{: shortdesc}

### 自訂 HTTP 及 HTTPS 埠 (`custom-port`)
{: #custom-port}

變更 HTTP 及 HTTPS 網路資料流量的預設埠，HTTP 為埠 80，HTTPS 為埠 443。
{:shortdesc}

**說明**</br>
依預設，Ingress ALB 是配置為在埠 80 接聽送入的 HTTP 網路資料流量，並在埠 443 接聽送入的 HTTPS 網路資料流量。您可以變更預設埠來增加 ALB 網域的安全，或只啟用 HTTPS 埠。


若要在埠上啟用交互鑑別，請[配置 ALB 以開啟有效的埠](/docs/containers?topic=containers-ingress#opening_ingress_ports)，然後在 [`mutual-auth` 註釋中指定該埠](#mutual-auth)。請不要使用 `custom-port` 註釋來指定用於交互鑑別的埠。
{: note}

**Ingress 資源 YAML 範例**</br>
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
<caption>瞭解註釋元件</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
 </thead>
 <tbody>
 <tr>
 <td><code>&lt;protocol&gt;</code></td>
 <td>輸入 <code>http</code> 或 <code>https</code> 來變更送入之 HTTP 或 HTTPS 網路資料流量的預設埠。</td>
 </tr>
 <tr>
 <td><code>&lt;port&gt;</code></td>
 <td>輸入要用於送入之 HTTP 或 HTTPS 網路資料流量的埠號。<p class="note">指定 HTTP 或 HTTPS 的自訂埠時，預設埠對於 HTTP 和 HTTPS 便不再有效。例如，若要將 HTTPS 的預設埠變更為 8443，但 HTTP 使用預設埠，您必須兩者都設定自訂埠：<code>custom-port: "protocol=http port=80; protocol=https port=8443"</code>。</p></td>
 </tr>
</tbody></table>


**用法**</br>
1. 檢閱 ALB 的開啟埠。

  ```
  kubectl get service -n kube-system
  ```
  {: pre}

  CLI 輸出會與下列內容類似：

  ```
  NAME                                             TYPE           CLUSTER-IP     EXTERNAL-IP    PORT(S)                      AGE
  public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx    169.xx.xxx.xxx 80:30416/TCP,443:32668/TCP   109d
  ```
  {: screen}

2. 開啟 ALB 配置對映。
  ```
  kubectl edit configmap ibm-cloud-provider-ingress-cm -n kube-system
  ```
  {: pre}

3. 將非預設的 HTTP 及 HTTPS 埠新增至配置對映。將 `<port>` 取代為您要開啟的 HTTP 或 HTTPS 埠。
  <p class="note">依預設，會開啟埠 80 及 443。如果您要將 80 及 443 保留為開啟狀態，則除了您在 `public-ports` 欄位中指定的任何其他 TCP 埠之外，還必須包括它們。如果已啟用專用 ALB，則也必須在 `private-ports` 欄位中指定您要保留開啟狀態的任何埠。如需相關資訊，請參閱[在 Ingress ALB 中開啟埠](/docs/containers?topic=containers-ingress#opening_ingress_ports)。
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

4. 驗證已使用非預設埠重新配置 ALB。

  ```
  kubectl get service -n kube-system
  ```
  {: pre}

  CLI 輸出會與下列內容類似：

  ```
  NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP    PORT(S)                           AGE
  public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx    169.xx.xxx.xxx <port1>:30776/TCP,<port2>:30412/TCP   109d
  ```
  {: screen}

5. 配置您的 Ingress，以在將送入的網路資料流量遞送至服務時使用非預設埠。請使用本參考資料中 YAML 範例檔案的註釋。

6. 更新 ALB 配置。

  ```
        kubectl apply -f myingress.yaml
        ```
  {: pre}

7. 開啟偏好的 Web 瀏覽器以存取您的應用程式。範例：`https://<ibmdomain>:<port>/<service_path>/`

<br />


### HTTP 重新導向至 HTTPS (`redirect-to-https`)
{: #redirect-to-https}

將不安全的 HTTP 用戶端要求轉換成 HTTPS。
 {:shortdesc}

**說明**</br>
請設定 Ingress ALB，以 IBM 提供的 TLS 憑證或您的自訂 TLS 憑證來保護網域安全。部分使用者可能會嘗試對 ALB 網域使用不安全的 `http` 要求（例如 `http://www.myingress.com`）來存取您的應用程式，而非使用 `https`。您可以使用重新導向註釋，一律將不安全的 HTTP 用戶端要求重新導向至 HTTPS。如果您未使用此註釋，依預設，不安全的 HTTP 要求不會轉換成 HTTPS 要求，且可能會將未加密的機密資訊公開給大眾使用。


依預設，會停用將 HTTP 要求重新導向至 HTTPS。

**Ingress 資源 YAML 範例**</br>
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


### HTTP 嚴格傳輸安全 (`hsts`)
{: #hsts}

**說明**</br>
HSTS 指示瀏覽器僅使用 HTTPS 來存取網域。即使使用者輸入或遵循一般 HTTP 鏈結，瀏覽器也會強制升級至 HTTPS 的連線。


**Ingress 資源 YAML 範例**</br>
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
<caption>瞭解註釋元件</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
</thead>
<tbody>
<tr>
<td><code>enabled</code></td>
<td>使用 <code>true</code> 來啟用 HSTS。</td>
</tr>
<tr>
<td><code>maxAge</code></td>
<td>將 <code>&lt;<em>31536000</em>&gt;</code> 取代為整數，代表瀏覽器將傳送中要求直接快取至 HTTPS 的秒數。預設值為 <code>31536000</code>，相當於 1 年。</td>
</tr>
<tr>
<td><code>includeSubdomains</code></td>
<td>使用 <code>true</code> 來告知瀏覽器，HSTS 原則也適用於現行網域的所有子網域。預設值為 <code>true</code>。</td>
</tr>
</tbody></table>

<br />


### 交互鑑別 (`mutual-auth`)
{: #mutual-auth}

配置 ALB 的交互鑑別。
{:shortdesc}

**說明**</br>
針對 Ingress ALB 配置下游資料流量的交互鑑別。外部用戶端會鑑別伺服器，而伺服器也會使用憑證來鑑別用戶端。交互鑑別也稱為憑證型鑑別或雙向鑑別。
 

將 `mutual-auth` 註釋用於用戶端與 Ingress ALB 之間的 SSL 終止。將 [`ssl-services` 註釋](#ssl-services)用於 Ingress ALB 與後端應用程式之間的 SSL 終止。


交互鑑別註釋會驗證用戶端憑證。若要轉遞標頭中的用戶端憑證，使應用程式能夠處理授權，您可以使用下列 [`proxy-add-headers` 註釋](#proxy-add-headers)：`"ingress.bluemix.net/proxy-add-headers": "serviceName=router-set {\n X-Forwarded-Client-Cert $ssl_client_escaped_cert;\n}\n"`
{: tip}

**必要條件**</br>

* 您必須具有包含必要 `ca.crt` 的有效交互鑑別密碼。若要建立交互鑑別密碼，請參閱本節結尾的步驟。
* 若要在 443 以外的埠上啟用交互鑑別，請[配置 ALB 以開啟有效的埠](/docs/containers?topic=containers-ingress#opening_ingress_ports)，然後在此註釋中指定該埠。請不要使用 `custom-port` 註釋來指定用於交互鑑別的埠。

**Ingress 資源 YAML 範例**</br>
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
<caption>瞭解註釋元件</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
</thead>
<tbody>
<tr>
<td><code>secretName</code></td>
<td>將 <code>&lt;<em>mysecret</em>&gt;</code> 取代為密碼資源的名稱。</td>
</tr>
<tr>
<td><code>port</code></td>
<td>將 <code>&lt;<em>port</em>&gt;</code> 取代為 ALB 埠號。</td>
</tr>
<tr>
<td><code>serviceName</code></td>
<td>將 <code>&lt;<em>servicename</em>&gt;</code> 取代為一個以上的 Ingress 資源的名稱。這是選用參數。</td>
</tr>
</tbody></table>

**若要建立交互鑑別密碼，請執行下列動作：**

1. 從憑證提供者產生憑證管理中心 (CA) 憑證及金鑰。如果您有自己的網域，請為您的網域購買正式的 TLS 憑證。請確定每一個憑證的 [CN ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://support.dnsimple.com/articles/what-is-common-name/) 都不同。基於測試用途，您可以使用 OpenSSL 來建立自簽憑證。如需相關資訊，請參閱這個[自簽 SSL 憑證指導教學 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.akadia.com/services/ssh_test_certificate.html)或[交互鑑別指導教學，其中包括建立您自己的 CA ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://blog.codeship.com/how-to-set-up-mutual-tls-authentication/)。
    {: tip}
2. [將憑證轉換為 base-64 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.base64encode.org/)。
3. 使用憑證，以建立密碼 YAML 檔案。
     
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
4. 建立憑證作為 Kubernetes 密碼。
   ```
     kubectl create -f ssl-my-test
     ```
   {: pre}

<br />


### SSL 服務支援 (`ssl-services`)
{: #ssl-services}

容許 HTTPS 要求並且將要送至上游應用程式的資料流量加密。
{:shortdesc}

**說明**</br>
當您的 Ingress 資源配置具有 TLS 區段時，Ingress ALB 可以處理應用程式的 HTTPS 保護 URL 要求。依預設，ALB 會終止 TLS，並先將要求解密，再使用 HTTP 通訊協定，將資料流量轉遞至應用程式。如果您的應用程式需要 HTTPS 通訊協定，並且需要將資料流量加密，請使用 `ssl-services` 註釋。使用 `ssl-services` 註釋，ALB 會終止外部 TLS 連線，然後在 ALB 與應用程式 Pod 之間建立新的 SSL 連線。資料流量會在傳送至上游 Pod 之前重新加密。

如果後端應用程式可以處理 TLS，而且您想要額外增加安全，則可以藉由提供密碼中包含的憑證，來新增單向或交互鑑別。

將 `ssl-services` 註釋用於 Ingress ALB 與後端應用程式之間的 SSL 終止。將 [`mutual-auth` 註釋](#mutual-auth)用於用戶端與 Ingress ALB 之間的 SSL 終止。
{: tip}

**Ingress 資源 YAML 範例**</br>
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
<caption>瞭解註釋元件</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
</thead>
<tbody>
<tr>
<td><code>ssl-service</code></td>
<td>將 <code>&lt;<em>myservice</em>&gt;</code> 取代為需要 HTTPS 的服務名稱。系統會將從 ALB 到此應用程式之服務的資料流量加密。</td>
</tr>
<tr>
<td><code>ssl-secret</code></td>
<td>如果後端應用程式可以處理 TLS，而且您想要額外增加安全，請將 <code>&lt;<em>service-ssl-secret</em>&gt;</code> 取代為服務的單向或交互鑑別密碼。<ul><li>如果您提供單向鑑別密碼，則此值必須包含來自上游伺服器的 <code>trusted.crt</code>。若要建立單向密碼，請參閱本節結尾的步驟。</li><li>如果您提供交互鑑別密碼，則此值必須包含您應用程式預期從用戶端收到的必要 <code>client.crt</code> 及 <code>client.key</code>。若要建立交互鑑別密碼，請參閱本節結尾的步驟。</li></ul><p class="important">如果您未提供密碼，則允許不安全的連線。如果要測試連線且未備妥憑證，或者您的憑證已過期且您想要允許不安全的連線，您可以選擇省略密碼。</p></td>
</tr>
</tbody></table>


**若要建立單向鑑別密碼，請執行下列動作：**

1. 從上游伺服器取得憑證管理中心 (CA) 金鑰及憑證，以及 SSL 用戶端憑證。IBM ALM 是根據 NGINX，它需要主要憑證、中繼憑證及後端憑證。如需相關資訊，請參閱 [NGINX 文件 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.nginx.com/nginx/admin-guide/security-controls/securing-http-traffic-upstream/)。
2. [將憑證轉換為 base-64 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.base64encode.org/)。
3. 使用憑證，以建立密碼 YAML 檔案。
     
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

   如果您也要為上游資料流量施行交互鑑別，則除了在 data 區段中提供 `trusted.crt` 之外，您還可以提供 `client.crt` 和 `client.key`。
   {: tip}

4. 建立憑證作為 Kubernetes 密碼。
   ```
   kubectl create -f ssl-my-test
   ```
   {: pre}

</br>
**若要建立交互鑑別密碼，請執行下列動作：**

1. 從憑證提供者產生憑證管理中心 (CA) 憑證及金鑰。如果您有自己的網域，請為您的網域購買正式的 TLS 憑證。請確定每一個憑證的 [CN ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://support.dnsimple.com/articles/what-is-common-name/) 都不同。基於測試用途，您可以使用 OpenSSL 來建立自簽憑證。如需相關資訊，請參閱這個[自簽 SSL 憑證指導教學 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.akadia.com/services/ssh_test_certificate.html)或[交互鑑別指導教學，其中包括建立您自己的 CA ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://blog.codeship.com/how-to-set-up-mutual-tls-authentication/)。
    {: tip}
2. [將憑證轉換為 base-64 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.base64encode.org/)。
3. 使用憑證，以建立密碼 YAML 檔案。
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
4. 建立憑證作為 Kubernetes 密碼。
   ```
   kubectl create -f ssl-my-test
   ```
   {: pre}

<br />


### TCP 埠 (`tcp-ports`)
{: #tcp-ports}

透過非標準 TCP 埠存取應用程式。
{:shortdesc}

**說明**</br>
對正在執行 TCP 串流工作負載的應用程式使用此註釋。

<p class="note">ALB 係以透通模式運作，並將資料流量轉遞至後端應用程式。在此情況下，不支援 SSL 終止。TLS 連線不會終止，並會通過而不受影響。</p>

**Ingress 資源 YAML 範例**</br>
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
<caption>瞭解註釋元件</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>將 <code>&lt;<em>myservice</em>&gt;</code> 取代為您為要透過非標準 TCP 埠存取之 Kubernetes 服務的名稱。</td>
</tr>
<tr>
<td><code>ingressPort</code></td>
<td>將 <code>&lt;<em>ingress_port</em>&gt;</code> 取代為您要在其上存取應用程式的 TCP 埠。</td>
</tr>
<tr>
<td><code>servicePort</code></td>
<td>這是選用參數。若已提供，埠會替換為此值，然後再將資料流量傳送至後端應用程式。否則，埠會保持與 Ingress 埠相同。如果您不想要設定此參數，則可以從配置移除它。</td>
</tr>
</tbody></table>


**用法**</br>
1. 檢閱 ALB 的開啟埠。

  ```
  kubectl get service -n kube-system
  ```
  {: pre}

  CLI 輸出會與下列內容類似：

  ```
  NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP      PORT(S)                      AGE
  public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx    169.xx.xxx.xxx   80:30416/TCP,443:32668/TCP   109d
  ```
  {: screen}

2. 開啟 ALB 配置對映。

  ```
  kubectl edit configmap ibm-cloud-provider-ingress-cm -n kube-system
  ```
  {: pre}

3. 將 TCP 埠新增至配置對映。將 `<port>` 取代為您要開啟的 TCP 埠。依預設，會開啟埠 80 及 443。如果您要將 80 及 443 保留為開啟狀態，則除了您在 `public-ports` 欄位中指定的任何其他 TCP 埠之外，還必須包括它們。如果已啟用專用 ALB，則也必須在 `private-ports` 欄位中指定您要保留開啟狀態的任何埠。如需相關資訊，請參閱[在 Ingress ALB 中開啟埠](/docs/containers?topic=containers-ingress#opening_ingress_ports)。
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

4. 驗證已使用 TCP 埠重新配置 ALB。

  ```
  kubectl get service -n kube-system
  ```
  {: pre}
  CLI 輸出會與下列內容類似：
  ```
  NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP      PORT(S)                               AGE
  public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx    169.xx.xxx.xxx   <port1>:30776/TCP,<port2>:30412/TCP   109d
  ```
  {: screen}

5. 配置 ALB 透過非標準 TCP 埠來存取您的應用程式。請使用本參考資料中 YAML 範例檔案的 `tcp-ports` 註釋。

6. 建立您的 ALB 資源，或更新現有的 ALB 配置。
  ```
  kubectl apply -f myingress.yaml
  ```
  {: pre}

7. Curl Ingress 子網域，以存取應用程式。範例：`curl <domain>:<ingressPort>`

<br />


## 路徑遞送註釋
{: #path-routing}

Ingress ALB 會將資料流量遞送至後端應用程式所接聽的路徑。使用路徑遞送註釋，您可以配置 ALB 如何將資料流量遞送至應用程式。
{: shortdesc}

### 外部服務 (`proxy-external-service`)
{: #proxy-external-service}

將路徑定義新增至外部服務，例如 {{site.data.keyword.Bluemix_notm}} 中管理的服務。
{:shortdesc}

**說明**</br>
將路徑定義新增至外部服務。只有在您的應用程式在外部服務上作業，而非在後端服務上作業時才使用此註釋。當您使用此註釋建立外部服務路徑時，只會一起支援 `client-max-body-size`、`proxy-read-timeout`、`proxy-connect-timeout` 及 `proxy-buffering` 註釋。任何其他註釋都不支援與 `proxy-external-service` 一起使用。


您無法針對單一服務及路徑指定多個主機。
{: note}

**Ingress 資源 YAML 範例**</br>
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
<caption>瞭解註釋元件</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
</thead>
<tbody>
<tr>
<td><code>path</code></td>
<td>將 <code>&lt;<em>mypath</em>&gt;</code> 取代為外部服務接聽的路徑。</td>
</tr>
<tr>
<td><code>external-svc</code></td>
<td>將 <code>&lt;<em>external_service</em>&gt;</code> 取代為要呼叫的外部服務。例如 <code>https://&lt;myservice&gt;.&lt;region&gt;.mybluemix.net</code>。</td>
</tr>
<tr>
<td><code>host</code></td>
<td>將 <code>&lt;<em>mydomain</em>&gt;</code> 取代為外部服務的主機網域。</td>
</tr>
</tbody></table>

<br />


### 位置修飾元 (`location-modifier`)
{: #location-modifier}

修改 ALB 比對要求 URI 與應用程式路徑的方式。
{:shortdesc}

**說明**</br>
依預設，ALB 會處理應用程式所接聽的路徑（作為字首）。當 ALB 收到對應用程式的要求時，ALB 會檢查 Ingress 資源以尋找符合此要求 URI 開頭的路徑（作為字首）。如果找到相符項，要求會轉遞至應用程式部署所在 Pod 的 IP 位址。

`location-modifier` 註釋可藉由修改位置區塊配置來變更 ALB 搜尋相符項的方式。位置區塊決定如何處理對應用程式路徑的要求。

若要處理正規表示式 (regex) 路徑，則需要此註釋。
{: note}

**支援的修飾元**</br>
<table>
<caption>支援的修飾元</caption>
<col width="10%">
<col width="90%">
<thead>
<th>修飾元</th>
<th>說明</th>
</thead>
<tbody>
<tr>
<td><code>=</code></td>
<td>等號修飾元會導致 ALB 只選取完全相符項。找到完全相符項時，搜尋即會停止，並選取相符的路徑。<br>例如，如果您的應用程式在 <code>/tea</code> 上接聽，則在將要求與您的應用程式進行比對時，ALB 只會選取確切的 <code>/tea</code> 路徑。</td>
</tr>
<tr>
<td><code>~</code></td>
<td>波狀符號修飾元讓 ALB 在比對期間將路徑當作區分大小寫的 regex 路徑來處理。<br>例如，如果您的應用程式在 <code>/coffee</code> 上接聽，則在將要求與您的應用程式進行比對時，即使未針對您的應用程式明確地指定路徑，ALB 也可以選取 <code>/ab/coffee</code> 或 <code>/123/coffee</code> 路徑。</td>
</tr>
<tr>
<td><code>~\*</code></td>
<td>波狀符號後面接一個星號修飾元，讓 ALB 在比對期間將路徑當作不區分大小寫的 regex 路徑來處理。<br>例如，如果您的應用程式在 <code>/coffee</code> 上接聽，則在將要求與您的應用程式進行比對時，即使未針對您的應用程式明確地指定路徑，ALB 也可以選取 <code>/ab/Coffee</code> 或 <code>/123/COFFEE</code> 路徑。</td>
</tr>
<tr>
<td><code>^~</code></td>
<td>^ 符號後面接一個波狀符號修飾元，讓 ALB 選取最佳的非 regex 比對，而非 regex 路徑。</td>
</tr>
</tbody>
</table>


**Ingress 資源 YAML 範例**</br>
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
<caption>瞭解註釋元件</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
</thead>
<tbody>
<tr>
<td><code>modifier</code></td>
<td>將 <code>&lt;<em>location_modifier</em>&gt;</code> 取代為您要用於路徑的位置修飾元。支援的修飾元為 <code>'='</code>、<code>'~'</code>、<code>'~\*'</code> 及 <code>'^~'</code>。您必須以單引號括住這些修飾元。</td>
</tr>
<tr>
<td><code>serviceName</code></td>
<td>將 <code>&lt;<em>myservice</em>&gt;</code> 取代為您為應用程式所建立之 Kubernetes 服務的名稱。</td>
</tr>
</tbody></table>

<br />


### 重寫路徑 (`rewrite-path`)
{: #rewrite-path}

將 ALB 網域路徑上的送入網路資料流量，遞送至與後端應用程式所接聽路徑不同的路徑。
{:shortdesc}

**說明**</br>
Ingress ALB 網域會將 `mykubecluster.us-south.containers.appdomain.cloud/beans` 上的送入網路資料流量遞送至您的應用程式。您的應用程式會接聽 `/coffee`，而非 `/beans`。若要將送入的網路資料流量轉遞至您的應用程式，請將重寫註釋新增至您的 Ingress 資源配置檔。重寫註釋可確保 `/beans` 上的送入網路資料流量會使用 `/coffee` 路徑轉遞至您的應用程式。當包含多個服務時，請僅使用分號 (;) 來分隔這些服務，分號前後不留空格。

**Ingress 資源 YAML 範例**</br>

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
<caption>瞭解註釋元件</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>將 <code>&lt;<em>myservice</em>&gt;</code> 取代為您為應用程式所建立之 Kubernetes 服務的名稱。</td>
</tr>
<tr>
<td><code>rewrite</code></td>
<td>將 <code>&lt;<em>target_path</em>&gt;</code> 取代為應用程式所接聽的路徑。ALB 網域上的送入網路資料流量會使用此路徑轉遞至 Kubernetes 服務。大部分的應用程式不會接聽特定路徑，而是使用根路徑及特定埠。在 <code>mykubecluster.us-south.containers.appdomain.cloud/beans</code> 的範例中，重寫路徑是 <code>/coffee</code>。<p class= "note">如果您套用這個檔案，且 URL 顯示 <code>404</code> 回應，則您的後端應用程式可能是在接聽結尾是 `/` 的路徑。請嘗試為這個重寫欄位新增尾端的 `/`、重新套用檔案，然後重試 URL。</p></td>
</tr>
</tbody></table>

<br />


## Proxy 緩衝區註釋
{: #proxy-buffer}

Ingress ALB 會充當後端應用程式與用戶端 Web 瀏覽器之間的 Proxy。使用 Proxy 緩衝區註釋，您可以配置在傳送或接收資料封包時，如何在 ALB 上緩衝資料。  
{: shortdesc}

### 大型用戶端標頭緩衝區 (`large-client-header-buffers`)
{: #large-client-header-buffers}

設定讀取大型用戶端要求標頭的緩衝區數目上限及大小。
{:shortdesc}

**說明**</br>
讀取大型用戶端要求標頭的緩衝區僅依需求配置：如果在結束要求處理之後連線轉移至保留作用中狀況，則會釋放這些緩衝區。依預設，有 `4` 個緩衝區，且緩衝區大小等於 `8K` 位元組。如果要求行超出設定的大小上限，即一個緩衝區，則會對用戶端傳回 `414 Request-URI Too Large` HTTP 錯誤。此外，如果要求標頭欄位超出設定的大小上限，即一個緩衝區，則會對用戶端傳回 `400 Bad Request` 錯誤。您可以調整用於讀取大型用戶端要求標頭的緩衝區數目上限及大小。

**Ingress 資源 YAML 範例**</br>
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
<caption>瞭解註釋元件</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
 </thead>
 <tbody>
 <tr>
 <td><code>&lt;number&gt;</code></td>
 <td>應該配置用於讀取大型用戶端要求標頭的緩衝區數目上限。例如，若要將它設為 4，請定義 <code>4</code>。</td>
 </tr>
 <tr>
 <td><code>&lt;size&gt;</code></td>
 <td>讀取大型用戶端要求標頭的緩衝區大小上限。例如，若要將它設為 16 KB，請定義 <code>16k</code>。
   大小結尾必須為 <code>k</code> 代表 KB，或 <code>m</code> 代表 MB。</td>
 </tr>
</tbody></table>

<br />


### 用戶端回應資料緩衝 (`proxy-buffering`)
{: #proxy-buffering}

使用緩衝區註釋，可以在資料傳送至用戶端時，停用在 ALB 上儲存回應資料的功能。
{:shortdesc}

**說明**</br>
Ingress ALB 用來作為後端應用程式與用戶端 Web 瀏覽器之間的 Proxy。當回應從後端應用程式傳送至用戶端時，依預設，回應資料會在 ALB 上進行緩衝處理。ALB 會代理用戶端回應，並開始依照用戶端的速度將回應傳送至用戶端。ALB 收到來自後端應用程式的所有資料之後，會關閉與後端應用程式的連線。從 ALB 到用戶端的連線會維持開啟，直到用戶端收到所有資料為止。

如果停用在 ALB 上緩衝回應資料的功能，資料會立即從 ALB 傳送至用戶端。用戶端必須能夠依照 ALB 的速度處理送入的資料。如果用戶端速度太慢，上游連線會保持開啟狀態，直到用戶端能夠趕上。

依預設，會啟用 ALB 上的回應資料緩衝功能。

**Ingress 資源 YAML 範例**</br>
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
<caption>瞭解註釋元件</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
</thead>
<tbody>
<tr>
<td><code>enabled</code></td>
<td>若要在 ALB 上停用回應資料緩衝，請設為 <code>false</code>。</td>
</tr>
<tr>
<td><code>serviceName</code></td>
<td>將 <code><em>&lt;myservice1&gt;</em></code> 取代為您為應用程式所建立之 Kubernetes 服務的名稱。以分號 (;) 區隔多項服務。這是選用欄位。如果您未指定服務名稱，則所有服務都會使用此註釋。</td>
</tr>
</tbody></table>

<br />


### Proxy 緩衝區 (`proxy-buffers`)
{: #proxy-buffers}

配置 ALB 的 Proxy 緩衝區數目及大小。
{:shortdesc}

**說明**</br>
針對來自 Proxy 伺服器的單一連線，設定讀取回應的緩衝區大小及數目。除非已指定服務，否則此配置適用於 Ingress 子網域中的所有服務。例如，如果指定 `serviceName=SERVICE number=2 size=1k` 之類的配置，則會將 1k 套用至服務。如果指定 `number=2 size=1k` 之類的配置，則會將 1k 套用至 Ingress 子網域中的所有服務。</br>
<p class="tip">如果您得到錯誤訊息 `upstream sent too big header while reading response header from upstream`，表示後端的上游伺服器所傳送的標頭大小超過預設限制。請同時增加 `proxy-buffers` 和 [`proxy-buffer-size`](#proxy-buffer-size) 的大小。</p>

**Ingress 資源 YAML 範例**</br>

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
<caption>瞭解註釋元件</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>將 <code>&lt;<em>myservice</em>&gt;</code> 取代為要套用 proxy-buffers 的服務名稱。</td>
</tr>
<tr>
<td><code>number</code></td>
<td>將 <code>&lt;<em>number_of_buffers</em>&gt;</code> 取代為數字，例如 <code>2</code>。</td>
</tr>
<tr>
<td><code>size</code></td>
<td>將 <code>&lt;<em>size</em>&gt;</code> 取代為每一個緩衝區的大小，並以千位元組（k 或 K）為單位，例如 <code>1K</code>。</td>
</tr>
</tbody>
</table>

<br />


### Proxy 緩衝區大小 (`proxy-buffer-size`)
{: #proxy-buffer-size}

配置從回應的第一部分讀取的 Proxy 緩衝區大小。
{:shortdesc}

**說明**</br>
設定緩衝區大小，以讀取從 Proxy 伺服器收到之回應的第一部分。此部分的回應通常包含小型回應標頭。除非已指定服務，否則此配置適用於 Ingress 子網域中的所有服務。例如，如果指定 `serviceName=SERVICE size=1k` 之類的配置，則會將 1k 套用至服務。如果指定 `size=1k` 之類的配置，則會將 1k 套用至 Ingress 子網域中的所有服務。


如果您得到錯誤訊息 `upstream sent too big header while reading response header from upstream`，表示後端的上游伺服器所傳送的標頭大小超過預設限制。請同時增加 `proxy-buffer-size` 和 [`proxy-buffers`](#proxy-buffers) 的大小。
{: tip}

**Ingress 資源 YAML 範例**</br>
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
<caption>瞭解註釋元件</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>將 <code>&lt;<em>myservice</em>&gt;</code> 取代為要套用 proxy-buffer-size 的服務名稱。</td>
</tr>
<tr>
<td><code>size</code></td>
<td>將 <code>&lt;<em>size</em>&gt;</code> 取代為每一個緩衝區的大小，並以千位元組（k 或 K）為單位，例如 <code>1K</code>。若要計算適當的大小，您可以查看[此部落格文章 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.getpagespeed.com/server-setup/nginx/tuning-proxy_buffer_size-in-nginx)。</td>
</tr>
</tbody></table>

<br />


### Proxy 工作中緩衝區大小 (`proxy-busy-buffers-size`)
{: #proxy-busy-buffers-size}

配置可以在忙碌中的 Proxy 緩衝區大小。
{:shortdesc}

**說明**</br>
限制在尚未完全讀取回應時，正在將回應傳送至用戶端的任何緩衝區大小。同時，其餘的緩衝區可讀取回應，並在必要時，將回應的一部分緩衝至暫存檔。除非已指定服務，否則此配置適用於 Ingress 子網域中的所有服務。例如，如果指定 `serviceName=SERVICE size=1k` 之類的配置，則會將 1k 套用至服務。如果指定 `size=1k` 之類的配置，則會將 1k 套用至 Ingress 子網域中的所有服務。


**Ingress 資源 YAML 範例**</br>
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
<caption>瞭解註釋元件</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>將 <code>&lt;<em>myservice</em>&gt;</code> 取代為要套用 proxy-busy-buffers-size 的服務名稱。</td>
</tr>
<tr>
<td><code>size</code></td>
<td>將 <code>&lt;<em>size</em>&gt;</code> 取代為每一個緩衝區的大小，並以千位元組（k 或 K）為單位，例如 <code>1K</code>。</td>
</tr>
</tbody></table>

<br />


## 要求及回應註釋
{: #request-response}

使用要求和回應註釋，以新增或移除用戶端和伺服器要求中的標頭資訊，以及變更用戶端可以傳送的內文大小。
{: shortdesc}

### 將伺服器埠新增至主機標頭 (`add-host-port`)
{: #add-host-port}

將伺服器埠新增至用戶端要求，然後再將此要求轉遞到後端應用程式。
{: shortdesc}

**說明**</br>
先將 `:server_port` 新增至用戶端要求的主機標頭，再將要求轉遞至後端應用程式。

**Ingress 資源 YAML 範例**</br>
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
<caption>瞭解註釋元件</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
</thead>
<tbody>
<tr>
<td><code>enabled</code></td>
<td>若要啟用子網域的 server_port 設定，請設為 <code>true</code>。</td>
</tr>
<tr>
<td><code>serviceName</code></td>
<td>將 <code><em>&lt;myservice&gt;</em></code> 取代為您為應用程式所建立之 Kubernetes 服務的名稱。以分號 (;) 區隔多項服務。這是選用欄位。如果您未指定服務名稱，則所有服務都會使用此註釋。</td>
</tr>
</tbody></table>

<br />


### 其他用戶端要求或回應標頭（`proxy-add-headers`、`response-add-headers`）
{: #proxy-add-headers}

將額外的標頭資訊新增至用戶端要求，然後再將要求傳送至後端應用程式，或新增至用戶端回應，然後再將回應傳送至用戶端。
{:shortdesc}

**說明**</br>
Ingress ALB 用來作為用戶端應用程式與後端應用程式之間的 Proxy。傳送至 ALB 的用戶端要求會經過處理（代理），然後放入新的要求，再傳送到您的後端應用程式。同樣地，傳送至 ALB 的後端應用程式回應會經過處理（代理），然後放入新的要求，再傳送到用戶端。代理 (Proxy) 要求或回應會移除一開始從用戶端或後端應用程式傳送的 HTTP 標頭資訊，例如使用者名稱。

如果您的後端應用程式需要 HTTP 標頭資訊，在 ALB 將用戶端要求轉遞至後端應用程式之前，您可以使用 `proxy-add-headers` 註釋將標頭資訊新增至該用戶端要求。如果用戶端 Web 應用程式需要 HTTP 標頭資訊，在 ALB 將回應轉遞至用戶端 Web 應用程式之前，您可以使用 `response-add-headers` 註釋將標頭資訊新增至該回應。<br>

例如，您可能需要在要求轉遞至應用程式之前，將下列 X-Forward 標頭資訊新增至該要求：
```
proxy_set_header Host $host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-Proto $scheme;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
```
{: screen}

若要將 X-Forward 標頭資訊新增至已傳送至應用程式的要求，請以下列方式使用 `proxy-add-headers` 註釋：
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

`response-add-headers` 註釋不支援所有服務的廣域標頭。若要在伺服器層次新增所有服務回應的標頭，您可以使用 [`server-snippets` 註釋](#server-snippets)：
{: tip}
```
annotations:
  ingress.bluemix.net/server-snippets: |
    add_header <header1> <value1>;
```
{: pre}
</br>

**Ingress 資源 YAML 範例**</br>
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
<caption>瞭解註釋元件</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
</thead>
<tbody>
<tr>
<td><code>service_name</code></td>
<td>將 <code>&lt;<em>myservice</em>&gt;</code> 取代為您為應用程式所建立之 Kubernetes 服務的名稱。</td>
</tr>
<tr>
<td><code>&lt;header&gt;</code></td>
<td>要新增至用戶端要求或用戶端回應的標頭資訊索引鍵。</td>
</tr>
<tr>
<td><code>&lt;value&gt;</code></td>
<td>要新增至用戶端要求或用戶端回應的標頭資訊值。</td>
</tr>
</tbody></table>

<br />


### 用戶端回應標頭移除 (`response-remove-headers`)
{: #response-remove-headers}

移除來自後端應用程式之用戶端回應中內含的標頭資訊，再將回應傳送至用戶端。
{:shortdesc}

**說明**</br>
Ingress ALB 用來作為後端應用程式與用戶端 Web 瀏覽器之間的 Proxy。從後端應用程式傳送到 ALB 的用戶端回應會經過處理（代理），然後放入新的回應，再從 ALB 傳送到用戶端 Web 瀏覽器。雖然代理回應會移除一開始從後端應用程式傳送的 HTTP 標頭資訊，但這個程序不一定會移除所有後端應用程式特有的標頭。從用戶端回應移除標頭資訊，再將回應從 ALB 轉遞到用戶端 Web 瀏覽器。

**Ingress 資源 YAML 範例**</br>

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
<caption>瞭解註釋元件</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
</thead>
<tbody>
<tr>
<td><code>service_name</code></td>
<td>將 <code>&lt;<em>myservice</em>&gt;</code> 取代為您為應用程式所建立之 Kubernetes 服務的名稱。</td>
</tr>
<tr>
<td><code>&lt;header&gt;</code></td>
<td>要從用戶端回應移除的標頭索引鍵。</td>
</tr>
</tbody></table>

<br />


### 用戶端要求內文大小 (`client-max-body-size`)
{: #client-max-body-size}

設定用戶端在要求中所能傳送的內文大小上限。
{:shortdesc}

**說明**</br>
為了維護預期的效能，用戶端要求內文大小上限設為 1 MB。當內文大小超過限制的用戶端要求傳送至 Ingress ALB，且用戶端不容許分割資料時，ALB 會對該用戶端傳回 413（要求實體太大）的 HTTP 回應。在要求內文的大小減少之前，用戶端與 ALB 之間無法進行連線。當用戶端容許資料分割成多個片段時，資料會分成多個 1 MB 的套件，然後傳送至 ALB。

您可能會想要增加內文大小上限，因為您預期用戶端要求的內文大小會大於 1 MB。例如，您要讓用戶端能上傳大型檔案。增加要求內文大小上限可能會影響 ALB 的效能，因為與用戶端的連線必須保持開啟，直到收到要求為止。


部分用戶端 Web 瀏覽器無法適當地顯示 413 HTTP 回應訊息。
{: note}

**Ingress 資源 YAML 範例**</br>
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
<caption>瞭解註釋元件</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>選用：若要將用戶端內文大小上限套用至特定服務，請將 <code>&lt;<em>myservice</em>&gt;</code> 取代為服務的名稱。如果未指定服務名稱，則會將大小套用至所有服務。在範例 YAML 中，格式 <code>"serviceName=&lt;myservice&gt; size=&lt;size&gt;; size=&lt;size&gt;"</code> 會將第一個大小套用至 <code>myservice</code> 服務，而第二個大小則會套用至所有其他服務。</li>
</tr>
<td><code>&lt;size&gt;</code></td>
<td>用戶端回應內文的大小上限。例如，若要將大小上限設為 200 MB，請定義 <code>200m</code>。您可以將大小設為 0，以停用用戶端要求內文大小的檢查。</td>
</tr>
</tbody></table>

<br />


## 服務限制註釋
{: #service-limit}

使用服務限制註釋，您可以變更預設要求處理率，以及來自單一 IP 位址的連線數目。
{: shortdesc}

### 廣域比率限制 (`global-rate-limit`)
{: #global-rate-limit}

針對所有服務的已定義金鑰，限制要求處理速率及連線數。
{:shortdesc}

**說明**</br>
針對所有服務，限制要求處理速率及每個已定義金鑰來自所選取後端的所有路徑之單一 IP 位址的連線數目。

**Ingress 資源 YAML 範例**</br>
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
<caption>瞭解註釋元件</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
</thead>
<tbody>
<tr>
<td><code>key</code></td>
<td>支援的值為 `location`、`$http_` 標頭及 `$uri`。若要根據區域或服務來設定送入要求的廣域限制，請使用 `key=location`。若要根據標頭來設定送入要求的廣域限制，請使用 `X-USER-ID key=$http_x_user_id`。</td>
</tr>
<tr>
<td><code>rate</code></td>
<td>將 <code>&lt;<em>rate</em>&gt;</code> 取代為處理速率。輸入值作為每秒速率 (r/s) 或每分鐘速率 (r/m)。範例：<code>50r/m</code>。</td>
</tr>
<tr>
<td><code>conn</code></td>
<td>將 <code>&lt;<em>number-of-connections</em>&gt;</code> 取代為連線數目。</td>
</tr>
</tbody></table>

<br />


### 服務比率限制 (`service-rate-limit`)
{: #service-rate-limit}

針對特定服務，限制要求處理速率及連線數。
{:shortdesc}

**說明**</br>
針對特定服務，限制要求處理速率及每個已定義金鑰來自所選取後端的所有路徑之單一 IP 位址的連線數目。

**Ingress 資源 YAML 範例**</br>
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
<caption>瞭解註釋元件</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>將 <code>&lt;<em>myservice</em>&gt;</code> 取代為您要建立處理速率的服務名稱。</li>
</tr>
<tr>
<td><code>key</code></td>
<td>支援的值為 `location`、`$http_` 標頭及 `$uri`。若要根據區域或服務來設定送入要求的廣域限制，請使用 `key=location`。若要根據標頭來設定送入要求的廣域限制，請使用 `X-USER-ID key=$http_x_user_id`。</td>
</tr>
<tr>
<td><code>rate</code></td>
<td>將 <code>&lt;<em>rate</em>&gt;</code> 取代為處理速率。若要定義每秒速率，請使用 r/s：<code>10r/s</code>。若要定義每分鐘速率，請使用 r/m：<code>50r/m</code>。</td>
</tr>
<tr>
<td><code>conn</code></td>
<td>將 <code>&lt;<em>number-of-connections</em>&gt;</code> 取代為連線數目。</td>
</tr>
</tbody></table>

<br />


## 使用者鑑別註釋
{: #user-authentication}

如果您想要使用 {{site.data.keyword.appid_full_notm}} 對應用程式進行鑑別，請利用使用者鑑別註釋。
{: shortdesc}

### {{site.data.keyword.appid_short_notm}} 鑑別 (`appid-auth`)
{: #appid-auth}

使用 {{site.data.keyword.appid_full_notm}} 對您的應用程式進行鑑別。
{:shortdesc}

**說明**</br>
使用 {{site.data.keyword.appid_short_notm}} 鑑別 Web 或 API HTTP/HTTPS 要求。

如果您將要求類型設為 web，則會驗證包含 {{site.data.keyword.appid_short_notm}} 存取記號的 Web 要求。如果記號驗證失敗，則會拒絕 Web 要求。如果要求不包含存取記號，則會將該要求重新導向至 {{site.data.keyword.appid_short_notm}} 登入頁面。若要讓 {{site.data.keyword.appid_short_notm}} Web 鑑別能夠運作，必須在使用者的瀏覽器中啟用 Cookie。

如果您將要求類型設為 api，則會驗證包含 {{site.data.keyword.appid_short_notm}} 存取記號的 API 要求。如果要求不包含存取記號，則會向使用者傳回 "401: Unauthorized" 錯誤訊息。

基於安全考量，{{site.data.keyword.appid_short_notm}} 鑑別僅支援已啟用 TLS/SSL 的後端。
{: note}

**Ingress 資源 YAML 範例**</br>
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
<caption>瞭解註釋元件</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
</thead>
<tbody>
<tr>
<td><code>bindSecret</code></td>
<td>將 <em><code>&lt;bind_secret&gt;</code></em> 取代為儲存 {{site.data.keyword.appid_short_notm}} 服務實例之連結密碼的 Kubernetes 密碼。</td>
</tr>
<tr>
<td><code>namespace</code></td>
<td>將 <em><code>&lt;namespace&gt;</code></em> 取代為連結密碼的名稱空間。這個欄位預設為 `default` 名稱空間。</td>
</tr>
<tr>
<td><code>requestType</code></td>
<td>將 <code><em>&lt;request_type&gt;</em></code> 取代為您要傳送至 {{site.data.keyword.appid_short_notm}} 的要求類型。接受的值包含 `web` 或 `api`。預設值為 `api`。</td>
</tr>
<tr>
<td><code>serviceName</code></td>
<td>將 <code><em>&lt;myservice&gt;</em></code> 取代為您為應用程式所建立之 Kubernetes 服務的名稱。這是必要欄位。如果不包括服務名稱，則會啟用所有服務的註釋。如果包括服務名稱，則只會啟用該服務的註釋。以逗點 (,) 區隔多項服務。</td>
</tr>
<tr>
<td><code>idToken=false</code></td>
<td>選用項目：Liberty OIDC 用戶端無法同時剖析存取記號和身分記號。使用 Liberty 時，請將此值設為 false，如此就不會將身分記號傳送至 Liberty 伺服器。</td>
</tr>
</tbody></table>

**用法**</br>

因為應用程式使用 {{site.data.keyword.appid_short_notm}} 進行鑑別，所以您必須佈建 {{site.data.keyword.appid_short_notm}} 實例，以有效的重新導向 URI 配置該實例，並藉由將實例連結至叢集來產生連結密碼。

1. 選擇現有實例，或建立新的 {{site.data.keyword.appid_short_notm}} 實例。
  * 若要使用現有實例，請確定服務實例名稱不包含空格。若要移除空格，請選取服務實例名稱旁邊的其他選項功能表，然後選取**重新命名服務**。
  * 若要佈建[新的 {{site.data.keyword.appid_short_notm}} 實例](https://cloud.ibm.com/catalog/services/app-id)，請執行下列動作：
      1. 將自動填入的**服務名稱**取代為您自己的唯一服務實例名稱。
            服務實例名稱不得包含空格。
      2. 選擇叢集部署所在的相同地區。
      3. 按一下**建立**。

2. 新增應用程式的重新導向 URL。重新導向 URL 是應用程式的回呼端點。為了防止網路釣魚攻擊，App ID 會根據重新導向 URL 白名單來驗證要求 URL。
  1. 在 {{site.data.keyword.appid_short_notm}} 管理主控台中，導覽至**管理鑑別**。
  2. 在**身分提供者**標籤中，確保已選取身分提供者。如果未選取「身分提供者」，則不會鑑別使用者，但會向其發出存取記號，以匿名存取應用程式。
  3. 在**鑑別設定**標籤中，以 `http://<hostname>/<app_path>/appid_callback` 或 `https://<hostname>/<app_path>/appid_callback` 格式新增應用程式的重新導向 URL。

    {{site.data.keyword.appid_full_notm}} 提供 logout 函數：如果 {{site.data.keyword.appid_full_notm}} 路徑中有 `/logout`，則會移除 Cookie，並將使用者送回到登入頁面。若要使用此函數，您必須將 `/appid_logout` 附加至您的網域，格式為 `https://<hostname>/<app_path>/appid_logout`，並在重新導向 URL 清單中包含這個 URL。
    {: note}

3. 將 {{site.data.keyword.appid_short_notm}} 服務實例連結至叢集。
    這個指令會建立服務實例的服務金鑰，或者，您可以包含 `--key` 旗標來使用現有的服務金鑰認證。
  ```
  ibmcloud ks cluster-service-bind --cluster <cluster_name_or_ID> --namespace <namespace> --service <service_instance_name> [--key <service_instance_key>]
  ```
  {: pre}
    將服務順利新增至叢集之後，即會建立叢集密碼，以保留服務實例認證。CLI 輸出範例：
    ```
    ibmcloud ks cluster-service-bind --cluster mycluster --namespace mynamespace --service appid1
    Binding service instance to namespace...
    OK
    Namespace:    mynamespace
    Secret name:  binding-<service_instance_name>
    ```
  {: screen}

4. 取得已在叢集名稱空間中建立的密碼。
  ```
  kubectl get secrets --namespace=<namespace>
  ```
  {: pre}

5. 使用連結密碼及叢集名稱空間，將 `appid-auth` 註釋新增至 Ingress 資源。


