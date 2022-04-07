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



# 어노테이션으로 Ingress의 사용자 정의
{: #ingress_annotation}

Ingress 애플리케이션 로드 밸런서(ALB)에 기능을 추가하려면 Ingress 리소스에서 어노테이션을 메타데이터로 지정할 수 있습니다.
{: shortdesc}

어노테이션을 사용하기 전에 [Ingress 애플리케이션 로드 밸런서(ALB)로 HTTPS 로드 밸런싱](/docs/containers?topic=containers-ingress)의 단계를 따라 Ingress 서비스 구성을 적절하게 설정했는지 확인하십시오. 일단 기본 구성으로 Ingress ALB를 설정한 경우에는 Ingress 리소스 파일에 어노테이션을 추가하여 해당 기능을 확장할 수 있습니다.
{: note}

<table>
<caption>일반 어노테이션</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>일반 어노테이션</th>
<th>이름</th>
<th>설명</th>
</thead>
<tbody>
<tr>
<td><a href="#custom-errors">사용자 정의 오류 조치</a></td>
<td><code>custom-errors, custom-error-actions</code></td>
<td>ALB에서 특정 HTTP 오류에 대해 수행할 수 있는 사용자 정의 조치를 표시합니다.</td>
</tr>
<tr>
<td><a href="#location-snippets">위치 스니펫</a></td>
<td><code>location-snippets</code></td>
<td>서비스에 대한 사용자 정의 위치 블록 구성을 추가합니다.</td>
</tr>
<tr>
<td><a href="#alb-id">사설 ALB 라우팅</a></td>
<td><code>ALB-ID</code></td>
<td>사설 ALB를 사용하여 수신 요청을 앱으로 라우팅합니다.</td>
</tr>
<tr>
<td><a href="#server-snippets">서버 스니펫</a></td>
<td><code>server-snippets</code></td>
<td>사용자 정의 서버 블록 구성을 추가합니다.</td>
</tr>
</tbody></table>

<br>

<table>
<caption>연결 어노테이션</caption>
<col width="20%">
<col width="20%">
<col width="60%">
 <thead>
 <th>연결 어노테이션</th>
 <th>이름</th>
 <th>설명</th>
  </thead>
  <tbody>
  <tr>
  <td><a href="#proxy-connect-timeout">사용자 정의 연결 제한시간 및 읽기 제한시간</a></td>
  <td><code>proxy-connect-timeout, proxy-read-timeout</code></td>
  <td>백엔드 앱이 사용할 수 없는 것으로 간주되기 전에 백엔드 앱에 연결하고 백엔드 앱에서 읽기 위해 ALB가 대기하는 시간을 설정합니다.</td>
  </tr>
  <tr>
  <td><a href="#keepalive-requests">Keepalive 요청</a></td>
  <td><code>keepalive-requests</code></td>
  <td>하나의 Keepalive 연결을 통해 서비스할 수 있는 최대 요청 수를 설정합니다.</td>
  </tr>
  <tr>
  <td><a href="#keepalive-timeout">Keepalive 제한시간</a></td>
  <td><code>keepalive-timeout</code></td>
  <td>서버에서 Keepalive 연결이 열려 있는 최대 시간을 설정합니다.</td>
  </tr>
  <tr>
  <td><a href="#proxy-next-upstream-config">다음 업스트림 프록시</a></td>
  <td><code>proxy-next-upstream-config</code></td>
  <td>ALB가 다음 업스트림 서버로 요청을 전달할 수 있는 시점을 설정합니다.</td>
  </tr>
  <tr>
  <td><a href="#sticky-cookie-services">쿠키를 사용하는 세션 선호도</a></td>
  <td><code>sticky-cookie-services</code></td>
  <td>스티키 쿠키(sticky cookie)를 사용하여 동일한 업스트림 서버에 수신 네트워크 트래픽을 항상 라우팅합니다.</td>
  </tr>
  <tr>
  <td><a href="#upstream-fail-timeout">업스트림 실패 제한시간</a></td>
  <td><code>upstream-fail-timeout</code></td>
  <td>서버가 사용 불가능하다고 간주되기 전에 ALB가 서버에 연결을 시도할 수 있는 기간을 설정합니다.</td>
  </tr>
  <tr>
  <td><a href="#upstream-keepalive">업스트림 Keepalive</a></td>
  <td><code>upstream-keepalive</code></td>
  <td>업스트림 서버에 대한 최대 유휴 Keepalive 연결 수를 설정합니다.</td>
  </tr>
  <tr>
  <td><a href="#upstream-max-fails">업스트림 최대 실패 횟수</a></td>
  <td><code>upstream-max-fails</code></td>
  <td>서버가 사용 불가능하다고 간주되기 전에 서버와의 통신 시도에 실패한 최대 횟수를 설정합니다.</td>
  </tr>
  </tbody></table>

<br>

  <table>
  <caption>HTTPS 및 TLS/SSL 인증 어노테이션</caption>
  <col width="20%">
  <col width="20%">
  <col width="60%">
  <thead>
  <th>HTTPS 및 TLS/SSL 인증 어노테이션</th>
  <th>이름</th>
  <th>설명</th>
  </thead>
  <tbody>
  <tr>
  <td><a href="#custom-port">사용자 정의 HTTP 및 HTTPS 포트</a></td>
  <td><code>custom-port</code></td>
  <td>HTTP(포트 80) 및 HTTPS(포트 443) 네트워크 트래픽에 대한 기본 포트를 변경합니다.</td>
  </tr>
  <tr>
  <td><a href="#redirect-to-https">HTTP의 경로를 HTTPS로 재지정</a></td>
  <td><code>redirect-to-https</code></td>
  <td>도메인에서 비보안 HTTP 요청의 경로를 HTTPS로 재지정합니다.</td>
  </tr>
  <tr>
  <td><a href="#hsts">HSTS(HTTP Strict Transport Security)</a></td>
  <td><code>hsts</code></td>
  <td>HTTPS를 통해서만 도메인에 액세스하도록 브라우저를 설정합니다.</td>
  </tr>
  <tr>
  <td><a href="#mutual-auth">상호 인증</a></td>
  <td><code>mutual-auth</code></td>
  <td>ALB에 대한 상호 인증을 구성합니다.</td>
  </tr>
  <tr>
  <td><a href="#ssl-services">SSL 서비스 지원</a></td>
  <td><code>ssl-services</code></td>
  <td>SSL 서비스 지원이 HTTPS가 필요한 업스트림 앱에 트래픽을 암호화하도록 허용합니다.</td>
  </tr>
  <tr>
  <td><a href="#tcp-ports">TCP 포트</a></td>
  <td><code>tcp-ports</code></td>
  <td>비표준 TCP 포트를 통해 앱에 액세스합니다.</td>
  </tr>
  </tbody></table>

<br>

<table>
<caption>경로 라우팅 어노테이션</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>경로 라우팅 어노테이션</th>
<th>이름</th>
<th>설명</th>
</thead>
<tbody>
<tr>
<td><a href="#proxy-external-service">외부 서비스</a></td>
<td><code>proxy-external-service</code></td>
<td>외부 서비스(예: {{site.data.keyword.Bluemix_notm}}에서 호스팅되는 서비스)에 경로 정의를 추가합니다.</td>
</tr>
<tr>
<td><a href="#location-modifier">위치 수정자</a></td>
<td><code>location-modifier</code></td>
<td>ALB가 앱 경로에 대해 요청 URI를 일치시키는 방법을 수정합니다.</td>
</tr>
<tr>
<td><a href="#rewrite-path">경로 재작성</a></td>
<td><code>rewrite-path</code></td>
<td>백엔드 앱이 청취하는 다른 경로로 수신 네트워크 트래픽을 라우팅합니다.</td>
</tr>
</tbody></table>

<br>

<table>
<caption>프록시 버퍼 어노테이션</caption>
<col width="20%">
<col width="20%">
<col width="60%">
 <thead>
 <th>프록시 버퍼 어노테이션</th>
 <th>이름</th>
 <th>설명</th>
 </thead>
 <tbody>
 <tr>
<td><a href="#large-client-header-buffers">대형 클라이언트 헤더 버퍼</a></td>
<td><code>large-client-header-buffers</code></td>
<td>대형 클라이언트 요청 헤더를 읽는 최대 버퍼의 수 및 크기를 설정합니다.</td>
</tr>
 <tr>
 <td><a href="#proxy-buffering">클라이언트 응답 데이터 버퍼링</a></td>
 <td><code>proxy-buffering</code></td>
 <td>클라이언트로 응답을 보내는 동안 ALB에서 클라이언트 응답의 버퍼링을 사용하지 않도록 설정합니다.</td>
 </tr>
 <tr>
 <td><a href="#proxy-buffers">프록시 버퍼</a></td>
 <td><code>proxy-buffers</code></td>
 <td>프록시된 서버에서 단일 연결에 대한 응답을 읽는 데 사용되는 버퍼의 수와 크기를 설정합니다.</td>
 </tr>
 <tr>
 <td><a href="#proxy-buffer-size">프록시 버퍼 크기</a></td>
 <td><code>proxy-buffer-size</code></td>
 <td>프록시된 서버에서 수신되는 응답의 첫 번째 파트를 읽는 데 사용되는 버퍼의 크기를 설정합니다.</td>
 </tr>
 <tr>
 <td><a href="#proxy-busy-buffers-size">프록시의 사용 중인 버퍼 크기</a></td>
 <td><code>proxy-busy-buffers-size</code></td>
 <td>사용 중인 프록시 버퍼 크기를 설정합니다.</td>
 </tr>
 </tbody></table>

<br>

<table>
<caption>요청 및 응답 어노테이션</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>요청 및 응답 어노테이션</th>
<th>이름</th>
<th>설명</th>
</thead>
<tbody>
<tr>
<td><a href="#add-host-port">호스트 헤더에 서버 포트 추가</a></td>
<td><code>add-host-port</code></td>
<td>요청을 라우팅하기 위해 호스트에 서버 포트를 추가합니다.</td>
</tr>
<tr>
<td><a href="#client-max-body-size">클라이언트 요청 본문 크기</a></td>
<td><code>client-max-body-size</code></td>
<td>클라이언트가 요청의 일부로 전송할 수 있는 최대 본문 크기를 설정합니다.</td>
</tr>
<tr>
<td><a href="#proxy-add-headers">추가 클라이언트 요청 또는 응답 헤더</a></td>
<td><code>proxy-add-headers, response-add-headers</code></td>
<td>백엔드 앱에 요청을 전달하기 전에 클라이언트 요청에 헤더 정보를 추가하거나 클라이언트에 응답을 전송하기 전에 클라이언트 응답에 헤더 정보를 추가합니다.</td>
</tr>
<tr>
<td><a href="#response-remove-headers">클라이언트 응답 헤더 제거</a></td>
<td><code>response-remove-headers</code></td>
<td>클라이언트에 응답을 전달하기 전에 클라이언트 응답에서 헤더 정보를 제거합니다.</td>
</tr>
</tbody></table>

<br>

<table>
<caption>서비스 제한 어노테이션</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>서비스 제한 어노테이션</th>
<th>이름</th>
<th>설명</th>
</thead>
<tbody>
<tr>
<td><a href="#global-rate-limit">글로벌 속도 제한</a></td>
<td><code>global-rate-limit</code></td>
<td>모든 서비스의 정의된 키별 연결 수와 요청 처리 속도를 제한합니다.</td>
</tr>
<tr>
<td><a href="#service-rate-limit">서비스 속도 제한</a></td>
<td><code>service-rate-limit</code></td>
<td>특정 서비스의 정의된 키별 연결 수와 요청 처리 속도를 제한합니다.</td>
</tr>
</tbody></table>

<br>

<table>
<caption>사용자 인증 어노테이션</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>사용자 인증 어노테이션</th>
<th>이름</th>
<th>설명</th>
</thead>
<tbody>
<tr>
<td><a href="#appid-auth">{{site.data.keyword.appid_short}} 인증</a></td>
<td><code>appid-auth</code></td>
<td>{{site.data.keyword.appid_full}}를 사용하여 앱으로 인증합니다.</td>
</tr>
</tbody></table>

<br>

## 일반 어노테이션
{: #general}

### 사용자 정의 오류 조치(`custom-errors`, `custom-error-actions`)
{: #custom-errors}

ALB에서 특정 HTTP 오류에 대해 수행할 수 있는 사용자 정의 조치를 표시합니다.
{: shortdesc}

**설명**</br>
발생할 수 있는 특정 HTTP 오류를 처리하기 위해 수행할 ALB에 대한 사용자 정의 오류 조치를 설정할 수 있습니다.

* `custom-errors` 어노테이션은 서비스 이름, 처리할 HTTP 오류, 서비스에 대해 지정된 HTTP 오류가 발생했을 때 ALB에서 수행하는 오류 조치 이름을 정의합니다.
* `custom-error-actions` 어노테이션은 NGINX 코드 스니펫에 사용자 정의 오류 조치를 정의합니다.

예를 들어, `custom-errors` 어노테이션에서 `H/errorAction401`라고 하는 사용자 정의 오류 조치를 리턴하여 `app1`의 `401` 조치를 처리하도록 ALB를 설정할 수 있습니다. 그런 다음 `custom-error-actions` 어노테이션에서 ALB에서 클라이언트로 사용자 정의 오류 페이지를 리턴하도록 `/errorAction401`이라고 하는 코드 스니펫을 정의할 수 있습니다.</br>

또한 `custom-errors` 어노테이션을 사용하여 클라이언트를 관리할 오류 서비스로 경로 재지정할 수도 있습니다. Ingress 리소스 파일의 `paths` 섹션에서 오류 서비스의 경로를 정의해야 합니다.

**샘플 Ingress 리소스 YAML**</br>

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
<caption>어노테이션 컴포넌트 이해</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td><code>&lt;<em>app1</em>&gt;</code>을 사용자 정의 오류가 적용된 Kubernetes 서비스의 이름으로 대체합니다. 사용자 정의 오류는 이 동일한 업스트림 서비스를 사용하는 특정 경로에만 적용됩니다. 서비스 이름을 지정하지 않으면 사용자 정의 오류가 모든 서비스 경로에 적용됩니다.</td>
</tr>
<tr>
<td><code>httpError</code></td>
<td><code>&lt;<em>401</em>&gt;</code>을 사용자 오류 조치로 처리할 HTTP 오류 코드로 대체합니다.</td>
</tr>
<tr>
<td><code>errorActionName</code></td>
<td><code>&lt;<em>/errorAction401</em>&gt;</code>을 처리할 사용자 정의 오류 조치 이름 또는 오류 서비스의 경로로 대체합니다.<ul>
<li>사용자 정의 오류 조치의 이름을 지정하는 경우, <code>custom-error-actions</code> 어노테이션의 코드 스니펫에 해당 오류 조치를 정의해야 합니다. 샘플 YAML에서, <code>app1</code>은 <code>custom-error-actions</code> 어노테이션의 스니펫에 정의된 <code>/errorAction401</code>입니다.</li>
<li>오류 서비스의 경로를 지정하는 경우 <code>paths</code> 섹션에서 해당 오류 경로와 오류 서비스의 이름을 지정해야 합니다. 샘플 YAML에서, <code>app2</code>는 <code>paths</code> 섹션의 끝에서 정의된 <code>/errorPath</code>입니다.</li></ul></td>
</tr>
<tr>
<td><code>ingress.bluemix.net/custom-error-actions</code></td>
<td>ALB에서 사용자가 지정한 서비스 및 HTTP 오류에 대해 수행할 사용자 정의 오류 조치를 정의합니다. NGINX 코드 스니펫을 사용하여 <code>&lt;EOS&gt;</code>로 각 스니펫을 종료합니다. 샘플 YAML에서, ALB는 <code>app1</code>에 대해 <code>401</code> 오류가 발생하면 사용자 정의 오류 페이지(<code>http://example.com/forbidden.html</code>)를 클라이언트에 전달합니다.</td>
</tr>
</tbody></table>

<br />


### 위치 스니펫(`location-snippets`)
{: #location-snippets}

서비스에 대한 사용자 정의 위치 블록 구성을 추가합니다.
{:shortdesc}

**설명**</br>
서버 블록은 ALB 가상 서버에 대한 구성을 정의하는 NGINX 지시문입니다. 위치 블록은 서버 블록 내에 정의된 NGINX 지시문입니다. 위치 블록은 도메인 이름이나 IP 주소 및 포트 이후에 나오는 요청의 일부 또는 요청 URI를 Ingress가 처리하는 방법을 정의합니다.

서버 블록에서 요청을 수신하면 위치 블록은 URI를 경로와 일치시키며 요청은 앱이 배치된 팟(Pod)의 IP 주소로 전달됩니다. `location-snippets` 어노테이션을 사용하면 위치 블록이 특정 서비스로 요청을 전달하는 방법을 수정할 수 있습니다.

대신에 서버 블록을 전체적으로 수정하려면 [`server-snippets`](#server-snippets) 어노테이션을 참조하십시오.

NGINX 구성 파일에서 서버 및 위치 블록을 보려면 ALB 팟(Pod) 중 하나에 대해 다음 명령을 실행하십시오. `kubectl exec -ti <alb_pod> -n kube-system -c nginx-ingress -- cat ./etc/nginx/default-<ingress_resource_name>.conf`
{: tip}

**샘플 Ingress 리소스 YAML**</br>

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
<caption>어노테이션 컴포넌트 이해</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td><code>&lt;<em>myservice</em>&gt;</code>를 앱에 대해 작성된 서비스의 이름으로 대체합니다.</td>
</tr>
<tr>
<td>위치 스니펫</td>
<td>지정된 서비스에 사용할 구성 스니펫을 제공하십시오. <code>myservice1</code> 서비스의 샘플 스니펫은 프록시 요청 버퍼링을 끄고 로그 재작성을 켜며 서비스에 요청 전달 시 추가 헤더를 설정하도록 위치 블록을 구성합니다. <code>myservice2</code> 서비스의 샘플 스니펫은 비어 있는 <code>Authorization</code> 헤더를 설정합니다. 모든 위치 스니펫은 <code>&lt;EOS&gt;</code> 값으로 끝나야 합니다.</td>
</tr>
</tbody></table>

<br />


### 사설 ALB 라우팅(`ALB-ID`)
{: #alb-id}

사설 ALB를 사용하여 수신 요청을 앱으로 라우팅합니다.
{:shortdesc}

**설명**</br>
공용 ALB 대신 수신 요청을 라우팅할 사설 ALB를 선택합니다.

**샘플 Ingress 리소스 YAML**</br>
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
<caption>어노테이션 컴포넌트 이해</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
</thead>
<tbody>
<tr>
<td><code>&lt;private_ALB_ID&gt;</code></td>
<td>사설 ALB의 ID입니다. 사설 ALB ID를 찾으려면 <code>ibmcloud ks albs --cluster &lt;my_cluster&gt;</code>를 실행하십시오.<p>
둘 이상의 사설 ALB가 사용되는 다중 구역 클러스터가 있는 경우에는 <code>;</code>로 분리된 ALB ID의 목록을 제공할 수 있습니다. 예: <code>ingress.bluemix.net/ALB-ID: &lt;private_ALB_ID_1&gt;;&lt;private_ALB_ID_2&gt;;&lt;private_ALB_ID_3&gt;</code></p>
</td>
</tr>
</tbody></table>

<br />


### 서버 스니펫(`server-snippets`)
{: #server-snippets}

사용자 정의 서버 블록 구성을 추가합니다.
{:shortdesc}

**설명**</br>
서버 블록은 ALB 가상 서버에 대한 구성을 정의하는 NGINX 지시문입니다. `server-snippets` 어노테이션에서 사용자 정의 구성 스니펫을 제공하면 ALB가 서버 레벨에서 요청을 처리하는 방법을 수정할 수 있습니다.

NGINX 구성 파일에서 서버 및 위치 블록을 보려면 ALB 팟(Pod) 중 하나에 대해 다음 명령을 실행하십시오. `kubectl exec -ti <alb_pod> -n kube-system -c nginx-ingress -- cat ./etc/nginx/default-<ingress_resource_name>.conf`
{: tip}

**샘플 Ingress 리소스 YAML**</br>

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
<caption>어노테이션 컴포넌트 이해</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
</thead>
<tbody>
<tr>
<td>서버 스니펫</td>
<td>사용할 구성 스니펫을 제공하십시오. 이 샘플 스니펫은 <code>/health</code> 요청을 처리하기 위한 위치 블록을 지정합니다. 위치 블록은 정상적인 응답을 리턴하고 요청 전달 시에 헤더를 추가하도록 구성되어 있습니다.</td>
</tr>
</tbody>
</table>

`server-snippets` 어노테이션을 사용하여 서버 레벨에서 모든 서비스 응답에 대해 헤더를 추가할 수 있습니다.
{: tip}

```
annotations:
  ingress.bluemix.net/server-snippets: |
    add_header <header1> <value1>;
```
{: codeblock}

<br />


## 연결 어노테이션
{: #connection}

연결 어노테이션을 사용하면 ALB가 백엔드 앱 및 업스트림 서버에 연결되는 방법을 변경할 수 있으며 앱 또는 서버가 사용 불가능하다고 간주되기 전에 제한시간 또는 Keepalive 연결의 최대 수를 설정할 수 있습니다.
{: shortdesc}

### 사용자 정의 연결 제한시간 및 읽기 제한시간(`proxy-connect-timeout`, `proxy-read-timeout`)
{: #proxy-connect-timeout}

백엔드 앱이 사용할 수 없는 것으로 간주되기 전에 백엔드 앱에 연결하고 백엔드 앱에서 읽기 위해 ALB가 대기하는 시간을 설정합니다.
{:shortdesc}

**설명**</br>
클라이언트 요청이 Ingress ALB로 전송될 때 ALB가 백엔드 앱에 대한 연결을 엽니다. 기본적으로 ALB는 백엔드 앱에서 응답을 수신하기 위해 60초간 대기합니다. 백엔드 앱에서 60초 내에 응답하지 않으면 연결 요청이 중단되며 백엔드 앱이 사용 불가능한 것으로 간주됩니다.

ALB가 백엔드 앱에 연결되고 나면 ALB에서 백엔드 앱의 응답 데이터를 읽습니다. 이 읽기 조작 중에는 ALB가 백엔드 앱에서 데이터를 받기 위한 두 읽기 작업 사이에 최대 60초간 대기합니다. 백엔드 앱에서 60초 내에 데이터를 보내지 않으면 백엔드 앱에 대한 연결이 닫히며 앱을 사용할 수 없는 것으로 간주합니다.

프록시에서는 60초 연결 제한시간 및 읽기 제한시간이 기본 제한시간이며 변경하지 않아야 합니다.

앱의 가용성이 안정되지 않거나 높은 워크로드 때문에 앱이 느리게 응답하는 경우 연결 제한시간 또는 읽기 제한시간을 늘릴 수 있습니다. 제한시간에 도달할 때까지 백엔드 앱에 대한 연결이 열린 상태로 있어야 하므로 제한시간을 늘리면 ALB의 성능에 영향을 미칠 수 있습니다.

반면에 ALB의 성능을 높이기 위해 제한시간을 줄일 수도 있습니다. 워크로드가 높은 경우에도 백엔드 앱에서 지정된 제한시간 내에 요청을 처리할 수 있는지 확인하십시오.

**샘플 Ingress 리소스 YAML**</br>
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
<caption>어노테이션 컴포넌트 이해</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
</thead>
<tbody>
<tr>
<td><code>&lt;connect_timeout&gt;</code></td>
<td>백엔드 앱에 연결하기 위해 대기하는 시간(초 또는 분)입니다(예: <code>65s</code> 또는 <code>1m</code>). 연결 제한시간은 75초를 초과할 수 없습니다.</td>
</tr>
<tr>
<td><code>&lt;read_timeout&gt;</code></td>
<td>백엔드 앱을 읽기 전에 대기하는 시간(초 또는 분)입니다(예: <code>65s</code> 또는 <code>2m</code>).
</tr>
</tbody></table>

<br />


### Keepalive 요청(`keepalive-requests`)
{: #keepalive-requests}

**설명**</br>
하나의 Keepalive 연결을 통해 제공할 수 있는 최대 요청 수를 설정합니다.

**샘플 Ingress 리소스 YAML**</br>
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
<caption>어노테이션 컴포넌트 이해</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td><code>&lt;<em>myservice</em>&gt;</code>를 앱에 대해 작성된 Kubernetes 서비스의 이름으로 대체합니다. 이 매개변수는 선택사항입니다. 서비스가 지정되지 않으면 구성은 Ingress 하위 도메인의 모든 서비스에 적용됩니다. 매개변수가 제공되면 Keepalive 요청이 지정된 서비스에 설정됩니다. 매개변수가 제공되지 않으면 Keepalive 요청이 구성되지 않은 모든 서비스에 대해 <code>nginx.conf</code>의 서버 레벨에서 Keepalive 요청이 설정됩니다.</td>
</tr>
<tr>
<td><code>requests</code></td>
<td><code>&lt;<em>max_requests</em>&gt;</code>를 Keepalive 연결을 통해 제공할 수 있는 최대 요청 수로 대체합니다.</td>
</tr>
</tbody></table>

<br />


### Keepalive 제한시간(`keepalive-timeout`)
{: #keepalive-timeout}

**설명**</br>
서버에서 Keepalive 연결이 열려 있는 최대 시간을 설정합니다.

**샘플 Ingress 리소스 YAML**</br>
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
<caption>어노테이션 컴포넌트 이해</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td><code>&lt;<em>myservice</em>&gt;</code>를 앱에 대해 작성된 Kubernetes 서비스의 이름으로 대체합니다. 이 매개변수는 선택사항입니다. 매개변수가 제공되면 Keepalive 제한시간이 지정된 서비스에 설정됩니다. 매개변수가 제공되지 않으면 Keepalive 제한시간이 구성되지 않은 모든 서비스에 대해 <code>nginx.conf</code>의 서버 레벨에서 Keepalive 제한시간이 설정됩니다.</td>
</tr>
<tr>
<td><code>timeout</code></td>
<td><code>&lt;<em>time</em>&gt;</code>을 시간(초)으로 대체합니다. 예: <code>timeout=20s</code>. <code>0</code> 값을 사용하면 Keepalive 클라이언트 연결을 사용할 수 없습니다.</td>
</tr>
</tbody></table>

<br />


### 다음 업스트림 프록시(`proxy-next-upstream-config`)
{: #proxy-next-upstream-config}

ALB가 다음 업스트림 서버로 요청을 전달할 수 있는 시점을 설정합니다.
{:shortdesc}

**설명**</br>
Ingress ALB가 클라이언트 앱과 사용자의 앱 간의 프록시 역할을 수행합니다. 일부 앱 설정은 ALB로부터 수신되는 클라이언트 요청을 처리하는 여러 업스트림 서버를 필요로 합니다. ALB가 사용하는 프록시 서버가 앱이 사용하는 업스트림 서버와의 연결을 설정하지 못하는 경우가 있습니다. 이 경우 ALB는 다음 업스트림 서버와 연결을 설정하여 여기에 대신 요청을 전달하려 할 수 있습니다. 사용자는 `proxy-next-upstream-config` 어노테이션을 사용하여 ALB가 다음 업스트림 서버로 요청을 전달할 수 있는 경우, 시간 및 횟수를 설정할 수 있습니다.

`proxy-next-upstream-config` 사용 시에 제한시간이 항상 구성되므로 이 어노테이션에 `timeout=true`를 추가하지 마십시오.
{: note}

**샘플 Ingress 리소스 YAML**</br>

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
<caption>어노테이션 컴포넌트 이해</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td><code>&lt;<em>myservice</em>&gt;</code>를 앱에 대해 작성된 Kubernetes 서비스의 이름으로 대체합니다.</td>
</tr>
<tr>
<td><code>retries</code></td>
<td><code>&lt;<em>tries</em>&gt;</code>를 ALB가 다음 업스트림 서버로 요청을 전달하려 시도하는 최대 횟수로 대체하십시오. 이 숫자는 원래 요청을 포함합니다. 이 한계를 해제하려면 <code>0</code>을 사용하십시오. 값을 지정하지 않으면 기본값 <code>0</code>이 사용됩니다.
</td>
</tr>
<tr>
<td><code>timeout</code></td>
<td><code>&lt;<em>time</em>&gt;</code>을 ALB가 다음 업스트림 서버로 요청을 전달하려 시도하는 최대 시간(초)으로 대체하십시오. 예를 들어, 30초를 설정하려면 <code>30s</code>를 입력하십시오. 이 한계를 해제하려면 <code>0</code>을 사용하십시오. 값을 지정하지 않으면 기본값 <code>0</code>이 사용됩니다.
</td>
</tr>
<tr>
<td><code>error</code></td>
<td><code>true</code>로 설정되면 첫 번째 업스트림 서버와 연결을 설정하거나, 여기에 요청을 전달하거나, 응답 헤더를 읽는 중에 오류가 발생하는 경우 ALB가 요청을 다음 업스트림 서버로 전달합니다.
</td>
</tr>
<tr>
<td><code>invalid_header</code></td>
<td><code>true</code>로 설정되면 첫 번째 업스트림 서버가 비어 있거나 올바르지 않은 응답을 리턴하는 경우 ALB가 요청을 다음 업스트림 서버로 전달합니다.
</td>
</tr>
<tr>
<td><code>http_502</code></td>
<td><code>true</code>로 설정되면 첫 번째 업스트림 서버가 코드 502를 포함하는 응답을 리턴하는 경우 ALB가 요청을 다음 업스트림 서버로 전달합니다. HTTP 응답 코드 <code>500</code>, <code>502</code>, <code>503</code>, <code>504</code>, <code>403</code>, <code>404</code>, <code>429</code>를 지정할 수 있습니다.
</td>
</tr>
<tr>
<td><code>non_idempotent</code></td>
<td><code>true</code>로 설정되면 ALB가 멱등이 아닌 메소드를 사용한 요청을 다음 업스트림 서버로 전달할 수 있습니다. 기본적으로 ALB는 이러한 요청을 다음 업스트림 서버로 전달하지 않습니다.
</td>
</tr>
<tr>
<td><code>off</code></td>
<td>ALB가 다음 업스트림 서버로 요청을 전달하지 않도록 하려면 <code>true</code>로 설정하십시오.
</td>
</tr>
</tbody></table>

<br />


### 쿠키에 대한 세션 선호도(`sticky-cookie-services`)
{: #sticky-cookie-services}

ALB에 세션 선호도를 추가하고 수신 네트워크 트래픽을 항상 동일한 업스트림 서버로 라우팅하려면 스티키 쿠키 어노테이션을 사용하십시오.
{:shortdesc}

**설명**</br>
고가용성을 위해 앱 설정에 따라 수신 클라이언트 요청을 처리하는 여러 업스트림 서버를 배치해야 합니다. 클라이언트가 백엔드 앱에 연결할 때 세션 지속 기간 동안 또는 태스크를 완료할 때까지 소요되는 시간 동안 동일한 업스트림 서버에서 클라이언트에 서비스를 제공하도록 세션 선호도를 사용할 수 있습니다. 수신 네트워크 트래픽이 항상 동일한 업스트림 서버로 라우팅되게 하여 세션 선호도를 보장하도록 ALB를 구성할 수 있습니다.

백엔드 앱에 연결하는 모든 클라이언트는 ALB를 통해 사용 가능한 업스트림 서버 중 하나에 지정됩니다. ALB는 클라이언트 앱에 저장되는 세션 쿠키를 작성하며, 이 쿠키는 애플리케이션 로드 밸런서와 클라이언트 간 모든 요청의 헤더 정보에 포함됩니다. 쿠키의 정보를 사용하면 세션 전체에서 동일한 업스트림 서버가 모든 요청을 처리할 수 있습니다.

스티키 세션에 의존하면 복잡도를 가중시키고 가용성을 감소시킬 수 있습니다. 예를 들어, HTTP 서비스에서 동일한 세션 상태 값의 후속 요청만 수락하도록 초기 연결에 대한 일부 세션 상태를 유지보수하는 HTTP 서버가 있을 수 있습니다. 하지만, 이런 경우 HTTP 서비스의 수평적 확장이 쉽지 않습니다. 여러 대의 서버에서 세션 상태를 유지보수할 수 있도록 HTTP 요청 세션 값을 저장하려면 Redis 또는 Memcached와 같은 외부 데이터베이스의 사용을 고려하십시오.
{: note}

여러 서비스가 포함된 경우에는 세미콜론(;)을 사용하여 구분하십시오.

**샘플 Ingress 리소스 YAML**</br>
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
<caption>어노테이션 컴포넌트 이해</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td><code>&lt;<em>myservice</em>&gt;</code>를 앱에 대해 작성된 Kubernetes 서비스의 이름으로 대체합니다.</td>
</tr>
<tr>
<td><code>name</code></td>
<td><code>&lt;<em>cookie_name</em>&gt;</code>을 세션 중 작성된 스티키 쿠키의 이름으로 대체합니다.</td>
</tr>
<tr>
<td><code>expires</code></td>
<td>스티키 쿠키가 만료되기 전에 <code>&lt;<em>expiration_time</em>&gt;</code>을 초, 분 또는 시간 단위의 시간으로 대체합니다. 이 시간은 사용자 활동과 무관합니다. 쿠키가 만료되고 나면 클라이언트 웹 브라우저에서 쿠키가 삭제되어 더 이상 ALB로 전송되지 않습니다. 예를 들어, 만기 시간을 1초, 1분 또는 1시간으로 설정하려면 <code>1s</code>, <code>1m</code> 또는 <code>1h</code>를 입력하십시오.</td>
</tr>
<tr>
<td><code>path</code></td>
<td><code>&lt;<em>cookie_path</em>&gt;</code>를 Ingress 하위 도메인에 추가되는 경로로 대체합니다. 이 경로는 ALB로 쿠키가 전송되는 도메인과 하위 도메인을 표시합니다. 예를 들어, Ingress 도메인이 <code>www.myingress.com</code>이고 모든 클라이언트 요청에 해당 쿠키를 전송하려는 경우 <code>path=/</code>를 설정해야 합니다. <code>www.myingress.com/myapp</code> 및 해당 하위 도메인 모두에 대해서만 쿠키를 전송하려는 경우 <code>path=/myapp</code>으로 설정해야 합니다.</td>
</tr>
<tr>
<td><code>hash</code></td>
<td><code>&lt;<em>hash_algorithm</em>&gt;</code>를 쿠키의 정보를 보호하는 해시 알고리즘으로 대체합니다. <code>sha1</code>만 지원됩니다. SHA1은 쿠키의 정보를 기반으로 해시 합계를 작성하고 이 해시 합계를 쿠키에 추가합니다. 서버에서 쿠키의 정보를 복호화하고 데이터 무결성을 확인할 수 있습니다.</td>
</tr>
</tbody></table>

<br />


### 업스트림 실패 제한시간(`upstream-fail-timeout`)
{: #upstream-fail-timeout}

ALB가 서버에 연결을 시도할 수 있는 기간을 설정합니다.
{:shortdesc}

**설명**</br>
서버가 사용 불가능하다고 간주되기 전에 ALB가 서버에 연결을 시도할 수 있는 기간을 설정합니다. 서버가 사용 불가능하다고 간주될 수 있으려면 설정된 기간 내에 ALB가 [`upstream-max-fails`](#upstream-max-fails) 어노테이션에 의해 설정된 최대 연결 시도 실패 횟수에 도달해야 합니다. 이 기간은 서버가 사용 불가능하다고 간주되는 기간도 판별합니다.

**샘플 Ingress 리소스 YAML**</br>
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
<th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
</thead>
<tbody>
<tr>
<td><code>serviceName(선택사항)</code></td>
<td><code>&lt;<em>myservice</em>&gt;</code>를 앱에 대해 작성된 Kubernetes 서비스의 이름으로 대체합니다.</td>
</tr>
<tr>
<td><code>fail-timeout</code></td>
<td><code>&lt;<em>fail_timeout</em>&gt;</code>을 서버가 사용 불가능하다고 간주되기 전에 ALB가 서버에 연결을 시도할 수 있는 기간으로 대체합니다. 기본값은 <code>10s</code>입니다. 시간은 초 단위여야 합니다.</td>
</tr>
</tbody></table>

<br />


### 업스트림 keepalive(`upstream-keepalive`)
{: #upstream-keepalive}

업스트림 서버에 대한 최대 유휴 Keepalive 연결 수를 설정합니다.
{:shortdesc}

**설명**</br>
지정된 서비스의 업스트림 서버에 대한 유휴 Keepalive 연결의 최대 수를 설정합니다. 업스트림 서버에는 기본적으로 64개의 유휴 Keepalive 연결이 있습니다.

**샘플 Ingress 리소스 YAML**</br>
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
<caption>어노테이션 컴포넌트 이해</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td><code>&lt;<em>myservice</em>&gt;</code>를 앱에 대해 작성된 Kubernetes 서비스의 이름으로 대체합니다.</td>
</tr>
<tr>
<td><code>keepalive</code></td>
<td><code>&lt;<em>max_connections</em>&gt;</code>를 업스트림 서버에 대한 유휴 Keepalive 연결의 최대 수로 대체합니다. 기본값은 <code>64</code>입니다. <code>0</code> 값을 사용하면 지정된 서비스의 업스트림 Keepalive 연결을 사용할 수 없습니다.</td>
</tr>
</tbody></table>

<br />


### 업스트림 최대 실패 횟수(`upstream-max-fails`)
{: #upstream-max-fails}

서버와의 통신 시도에 실패한 최대 횟수를 설정합니다.
{:shortdesc}

**설명**</br>
서버가 사용 불가능하다고 간주되기 전에 ALB가 서버와의 연결에 실패할 수 있는 최대 횟수를 설정합니다. 서버가 사용 불가능하다고 간주될 수 있으려면 [`upstream-fail-timeout`](#upstream-fail-timeout) 어노테이션에 의해 설정된 기간 내에 ALB가 최대 횟수에 도달해야 합니다. 서버가 사용 불가능하다고 간주되는 기간은 `upstream-fail-timeout` 어노테이션에 의해서도 설정됩니다.

**샘플 Ingress 리소스 YAML**</br>
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
<th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
</thead>
<tbody>
<tr>
<td><code>serviceName(선택사항)</code></td>
<td><code>&lt;<em>myservice</em>&gt;</code>를 앱에 대해 작성된 Kubernetes 서비스의 이름으로 대체합니다.</td>
</tr>
<tr>
<td><code>max-fails</code></td>
<td><code>&lt;<em>max_fails</em>&gt;</code>를 ALB가 서버와 통신을 시도하는 데 실패할 수 있는 최대 횟수로 대체합니다. 기본값은 <code>1</code>입니다. <code>0</code> 값을 지정하면 어노테이션이 사용되지 않습니다.</td>
</tr>
</tbody></table>

<br />


## HTTPS 및 TLS/SSL 인증 어노테이션
{: #https-auth}

HTTPS 및 TLS/SSL 인증 어노테이션을 사용하면 HTTPS 트래픽의 ALB를 구성하고 기본 HTTPS 포트를 변경하며 백엔드 앱에 전송된 트래픽의 SSL 인증을 사용으로 설정하거나 상호 인증을 설정할 수 있습니다.
{: shortdesc}

### 사용자 정의 HTTP 및 HTTPS 포트(`custom-port`)
{: #custom-port}

HTTP(포트 80) 및 HTTPS(포트 443) 네트워크 트래픽에 대한 기본 포트를 변경합니다.
{:shortdesc}

**설명**</br>
기본적으로 Ingress ALB는 수신 HTTP 네트워크 트래픽을 포트 80에서 청취하고 수신 HTTPS 네트워크 트래픽은 포트 443에서 청취하도록 구성됩니다. ALB 도메인에 보안을 추가하거나 HTTPS 포트만 사용하도록 기본 포트를 변경할 수 있습니다.

포트에서 상호 인증을 사용으로 설정하려면 [유효한 포트를 열도록 ALB를 구성](/docs/containers?topic=containers-ingress#opening_ingress_ports)한 후에 [`mutual-auth` 어노테이션](#mutual-auth)에서 해당 포트를 지정하십시오. `custom-port` 어노테이션을 사용하여 상호 인증을 위한 포트를 지정하지 마십시오.
{: note}

**샘플 Ingress 리소스 YAML**</br>
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
<caption>어노테이션 컴포넌트 이해</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
 </thead>
 <tbody>
 <tr>
 <td><code>&lt;protocol&gt;</code></td>
 <td>수신 HTTP 또는 HTTPS 네트워크 트래픽의 기본 포트를 변경하려면 <code>http</code> 또는 <code>https</code>를 입력하십시오.</td>
 </tr>
 <tr>
 <td><code>&lt;port&gt;</code></td>
 <td>수신 HTTP 또는 HTTPS 네트워크 트래픽에 사용할 포트 번호를 입력하십시오.  <p class="note">사용자 정의 포트가 HTTP 또는 HTTPS에 대해 지정된 경우에는 기본 포트가 HTTP 및 HTTPS 둘 다에 대해 더 이상 유효하지 않습니다. 예를 들어, HTTPS에 대한 기본 포트를 8443으로 변경하지만 HTTP에는 기본 포트를 사용하려면 <code>custom-port: "protocol=http port=80; protocol=https port=8443"</code>과 같이 둘 다에 대한 사용자 정의 포트를 설정해야 합니다.</p></td>
 </tr>
</tbody></table>


**사용법**</br>
1. ALB를 위한 열린 포트를 검토하십시오.
  ```
  kubectl get service -n kube-system
  ```
  {: pre}

  CLI 출력이 다음과 유사하게 나타납니다.
  ```
  NAME                                             TYPE           CLUSTER-IP     EXTERNAL-IP    PORT(S)                      AGE
  public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx    169.xx.xxx.xxx 80:30416/TCP,443:32668/TCP   109d
  ```
  {: screen}

2. ALB 구성 맵을 여십시오.
  ```
  kubectl edit configmap ibm-cloud-provider-ingress-cm -n kube-system
  ```
  {: pre}

3. 기본이 아닌 HTTP 및 HTTPS 포트를 구성 맵에 추가하십시오. 열려는 HTTP 또는 HTTPS 포트로 `<port>`를 대체하십시오.
  <p class="note">기본적으로 포트 80 및 443이 열립니다. 80 및 443을 열린 상태로 유지하려면 `public-ports` 필드에 지정하는 다른 TCP 포트 이외에 이러한 포트를 포함해야 합니다. 사설 ALB를 사용으로 설정한 경우 `private-ports` 필드에도 열린 상태로 유지할 포트를 지정해야 합니다. 자세한 정보는 [Ingress ALB에서 포트 열기](/docs/containers?topic=containers-ingress#opening_ingress_ports)를 참조하십시오.</p>
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

4. ALB가 기본이 아닌 포트로 다시 구성되었는지 확인하십시오.
  ```
  kubectl get service -n kube-system
  ```
  {: pre}

  CLI 출력이 다음과 유사하게 나타납니다.
  ```
  NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP    PORT(S)                           AGE
  public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx    169.xx.xxx.xxx <port1>:30776/TCP,<port2>:30412/TCP   109d
  ```
  {: screen}

5. 수신 네트워크 트래픽을 서버로 라우팅할 때 기본이 아닌 포트를 사용하도록 Ingress를 구성하십시오. 이 참조서에 있는 샘플 YAML 파일의 어노테이션을 사용하십시오.

6. ALB 구성을 업데이트하십시오.
  ```
  kubectl apply -f myingress.yaml
  ```
  {: pre}

7. 선호하는 웹 브라우저를 열어 앱에 액세스하십시오. 예: `https://<ibmdomain>:<port>/<service_path>/`

<br />


### HTTP의 경로를 HTTPS로 재지정(`redirect-to-https`)
{: #redirect-to-https}

비보안 HTTP 클라이언트 요청을 HTTPS로 변환합니다.
{:shortdesc}

**설명**</br>
IBM 제공 TLS 인증서 또는 사용자 정의 TLS 인증서로 도메인을 보호하도록 Ingress ALB를 설정합니다. 일부 사용자는 `https`를 사용하지 않고 ALB 도메인에 대한 비보안 `http` 요청을 사용하여 앱에 액세스하려고 할 수 있습니다(예: `http://www.myingress.com`). 경로 재지정 어노테이션을 사용하여 항상 비보안 HTTP 요청을 HTTPS로 변환할 수 있습니다. 이 어노테이션을 사용하지 않으면 기본적으로 비보안 HTTP 요청이 HTTPS 요청으로 변환되지 않으며 암호화되지 않은 기밀 정보를 공용으로 노출할 수 있습니다.


HTTP 요청의 경로를 HTTPS로 재지정하는 기능은 기본적으로 사용되지 않습니다.

**샘플 Ingress 리소스 YAML**</br>
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


### HTTP Strict Transport Security(`hsts`)
{: #hsts}

**설명**</br>
HSTS는 HTTPS를 사용해서만 도메인에 액세스하도록 브라우저에 지시합니다. 사용자가 일반 HTTP 링크를 입력하거나 따르는 경우에도 브라우저는 연결을 HTTPS로 엄격하게 업그레이드합니다.

**샘플 Ingress 리소스 YAML**</br>
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
<caption>어노테이션 컴포넌트 이해</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
</thead>
<tbody>
<tr>
<td><code>enabled</code></td>
<td><code>true</code>를 사용하면 HSTS를 사용으로 설정합니다.</td>
</tr>
<tr>
<td><code>maxAge</code></td>
<td><code>&lt;<em>31536000</em>&gt;</code>을 브라우저가 바로 HTTP에 전송 요청을 캐시하는 기간(초)을 나타내는 정수로 대체합니다. 기본값은 <code>31536000</code>로, 1년과 같습니다.</td>
</tr>
<tr>
<td><code>includeSubdomains</code></td>
<td><code>true</code>를 사용하면 HSTS 정책이 현재 도메인의 모든 하위 도메인에도 적용됨을 브라우저에게 알려줍니다. 기본값은 <code>true</code>입니다. </td>
</tr>
</tbody></table>

<br />


### 상호 인증(`mutual-auth`)
{: #mutual-auth}

ALB에 대한 상호 인증을 구성합니다.
{:shortdesc}

**설명**</br>
Ingress ALB에 대한 다운스트림 트래픽의 상호 인증을 구성합니다. 외부 클라이언트가 서버를 인증하고 또한 서버는 인증서를 사용하여 클라이언트를 인증합니다. 상호 인증은 인증서 기반 인증 또는 양방향(two-way) 인증이라고도 합니다.

클라이언트와 Ingress ALB 간의 SSL 종료에 대해서는 `mutual-auth` 어노테이션을 사용하십시오. Ingress ALB와 백엔드 앱 간의 SSL 종료에 대해서는 [`ssl-services` 어노테이션](#ssl-services)을 사용하십시오.

상호 인증 어노테이션은 클라이언트 인증서를 유효성 검증합니다. 애플리케이션에서 인증을 처리하도록 클라이언트 인증서를 헤더에 전달하려면 다음 [`proxy-add-headers` annotation](#proxy-add-headers)을 사용하십시오. `"ingress.bluemix.net/proxy-add-headers": "serviceName=router-set {\n X-Forwarded-Client-Cert $ssl_client_escaped_cert;\n}\n"`.
{: tip}

**전제조건**</br>

* 필수 `ca.crt`가 포함된 올바른 상호 인증 시크릿이 있어야 합니다. 상호 인증 시크릿을 작성하려는 경우에는 이 섹션의 끝에 있는 단계를 참조하십시오.
* 443 이외의 포트에서 상호 인증을 사용하려면 [유효한 포트를 열도록 ALB를 구성](/docs/containers?topic=containers-ingress#opening_ingress_ports)한 후에 이 어노테이션에서 해당 포트를 지정하십시오. `custom-port` 어노테이션을 사용하여 상호 인증을 위한 포트를 지정하지 마십시오.

**샘플 Ingress 리소스 YAML**</br>
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
<caption>어노테이션 컴포넌트 이해</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
</thead>
<tbody>
<tr>
<td><code>secretName</code></td>
<td><code>&lt;<em>mysecret</em>&gt;</code>을 시크릿 리소스의 이름으로 대체합니다.</td>
</tr>
<tr>
<td><code>port</code></td>
<td><code>&lt;<em>port</em>&gt;</code>를 ALB 포트 번호로 대체하십시오.</td>
</tr>
<tr>
<td><code>serviceName</code></td>
<td><code>&lt;<em>servicename</em>&gt;</code>을 하나 이상의 Ingress 리소스의 이름으로 대체하십시오. 이 매개변수는 선택사항입니다.</td>
</tr>
</tbody></table>

**상호 인증 시크릿을 작성하려면 다음을 수행하십시오. **

1. 인증서 제공자의 인증 기관(CA) 인증서 및 키를 생성하십시오. 고유 도메인이 있는 경우 도메인의 공식적 TLS 인증서를 구매하십시오. [CN ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://support.dnsimple.com/articles/what-is-common-name/)이 각 인증서에 대해 서로 다른지 확인하십시오.
    테스트 용도로 OpenSSL을 사용하여 자체 서명 인증서를 작성할 수 있습니다. 자세한 정보는 이 [자체 서명 SSL 인증서 튜토리얼![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.akadia.com/services/ssh_test_certificate.html) 또는 이 [사용자 고유의 CA 작성이 포함된 상호 인증 튜토리얼![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://blog.codeship.com/how-to-set-up-mutual-tls-authentication/)을 참조하십시오.
    {: tip}
2. [인증서를 base-64로 변환 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.base64encode.org/)하십시오.
3. 인증서를 사용하여 시크릿 YAML 파일을 작성하십시오.
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
4. 인증서를 Kubernetes 시크릿으로 작성하십시오.
   ```
   kubectl create -f ssl-my-test
   ```
   {: pre}

<br />


### SSL 서비스 지원(`ssl-services`)
{: #ssl-services}

HTTPS 요청을 허용하고 업스트림 앱에 대한 트래픽을 암호화합니다.
{:shortdesc}

**설명**</br>
Ingress 리소스 구성에 TLS 섹션이 있는 경우, Ingress ALB는 앱에 대한 HTTPS 보안 URL 요청을 처리할 수 있습니다. 기본적으로, HTTP 프로토콜을 사용하여 트래픽을 앱에 전달하기 전에 ALB는 TLS 종료를 끝내고 요청을 복호화합니다. HTTPS 프로토콜이 필요하며 트래픽의 암호화가 필요한 앱이 있는 경우에는 `ssl-services` 어노테이션을 사용하십시오. `ssl-services` 어노테이션을 사용하여 ALB는 외부 TLS 연결을 종료한 후에 ALB 및 앱 팟(Pod) 간의 새 SSL 연결을 작성합니다. 트래픽은 업스트림 팟(Pod)에 전송되기 전에 다시 암호화됩니다.

백엔드 앱이 TLS를 처리할 수 있으며 사용자가 보안을 더 추가하려는 경우, 사용자는 시크릿에 포함된 인증서를 제공하여 단방향 또는 상호 인증을 추가할 수 있습니다.

Ingress ALB와 백엔드 앱 간의 SSL 종료에 대해서는 `ssl-services` 어노테이션을 사용하십시오. 클라이언트와 Ingress ALB 간의 SSL 종료에 대해서는 [`mutual-auth` 어노테이션](#mutual-auth)을 사용하십시오.
{: tip}

**샘플 Ingress 리소스 YAML**</br>
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
<caption>어노테이션 컴포넌트 이해</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
</thead>
<tbody>
<tr>
<td><code>ssl-service</code></td>
<td><code>&lt;<em>myservice</em>&gt;</code>를 HTTPS를 필요로 하는 서비스의 이름으로 대체하십시오. ALB에서 이 앱의 서비스로의 트래픽이 암호화됩니다.</td>
</tr>
<tr>
<td><code>ssl-secret</code></td>
<td>백엔드 앱이 TLS를 처리할 수 있으며 사용자가 보안을 더 추가하려는 경우에는 <code>&lt;<em>service-ssl-secret</em>&gt;</code>을 서비스의 단방향 또는 상호 인증 시크릿으로 대체하십시오.<ul><li>단방향 인증 시크릿을 제공하는 경우, 값에는 업스트림 서버의 <code>trusted.crt</code>가 포함되어야 합니다. 단방향 시크릿을 작성하려는 경우에는 이 섹션의 끝에 있는 단계를 참조하십시오.</li><li>상호 인증 시크릿을 제공하는 경우, 값에는 앱이 클라이언트로부터 기대하는 필수 <code>client.crt</code> 및 <code>client.key</code>가 포함되어야 합니다. 상호 인증 시크릿을 작성하려는 경우에는 이 섹션의 끝에 있는 단계를 참조하십시오.</li></ul><p class="important">시크릿을 제공하지 않으면 비보안 연결이 허용됩니다. 연결을 테스트하고자 하며 인증서가 준비되어 있지 않은 경우 또는 인증서가 만료되었으며 비보안 연결을 허용하려는 경우에는 시크릿을 생략하도록 선택할 수 있습니다.</p></td>
</tr>
</tbody></table>


**단방향 인증 시크릿을 작성하려면 다음을 수행하십시오. **

1. 업스트림 서버 및 SSL 클라이언트 인증서에서 인증 기관(CA) 키 및 인증서를 가져오십시오. IBM ALB는 루트 인증서, 중간 인증서 및 백엔드 인증서가 필요한 NGINX를 기반으로 합니다. 자세한 정보는 [NGINX 문서![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.nginx.com/nginx/admin-guide/security-controls/securing-http-traffic-upstream/)를 참조하십시오.
2. [인증서를 base-64로 변환 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.base64encode.org/)하십시오.
3. 인증서를 사용하여 시크릿 YAML 파일을 작성하십시오.
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

   업스트림 트래픽에 대한 상호 인증도 적용하려면 데이터 섹션에서 `trusted.crt`에 추가하여 `client.crt` 및 `client.key`를 제공할 수 있습니다.
   {: tip}

4. 인증서를 Kubernetes 시크릿으로 작성하십시오.
   ```
   kubectl create -f ssl-my-test
   ```
   {: pre}

</br>
**상호 인증 시크릿을 작성하려면 다음을 수행하십시오. **

1. 인증서 제공자의 인증 기관(CA) 인증서 및 키를 생성하십시오. 고유 도메인이 있는 경우 도메인의 공식적 TLS 인증서를 구매하십시오. [CN ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://support.dnsimple.com/articles/what-is-common-name/)이 각 인증서에 대해 서로 다른지 확인하십시오.
    테스트 용도로 OpenSSL을 사용하여 자체 서명 인증서를 작성할 수 있습니다. 자세한 정보는 이 [자체 서명 SSL 인증서 튜토리얼![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.akadia.com/services/ssh_test_certificate.html) 또는 이 [사용자 고유의 CA 작성이 포함된 상호 인증 튜토리얼![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://blog.codeship.com/how-to-set-up-mutual-tls-authentication/)을 참조하십시오.
    {: tip}
2. [인증서를 base-64로 변환 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.base64encode.org/)하십시오.
3. 인증서를 사용하여 시크릿 YAML 파일을 작성하십시오.
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
4. 인증서를 Kubernetes 시크릿으로 작성하십시오.
   ```
   kubectl create -f ssl-my-test
   ```
   {: pre}

<br />


### TCP 포트(`tcp-ports`)
{: #tcp-ports}

비표준 TCP 포트를 통해 앱에 액세스합니다.
{:shortdesc}

**설명**</br>
TCP 스트림 워크로드를 실행 중인 앱에 이 어노테이션을 사용합니다.

<p class="note">ALB는 패스 스루(pass-through) 모드에서 작동하며 백엔드 앱으로 트래픽을 전달합니다. 이 경우에 SSL 종료는 지원되지 않습니다. TLS 연결은 종료되지 않으며 영향 없이 패스 스루(pass through)됩니다.</p>

**샘플 Ingress 리소스 YAML**</br>
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
<caption>어노테이션 컴포넌트 이해</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td><code>&lt;<em>myservice</em>&gt;</code>를 비표준 TCP 포트를 통해 액세스할 Kubernetes 서비스의 이름으로 대체합니다.</td>
</tr>
<tr>
<td><code>ingressPort</code></td>
<td><code>&lt;<em>ingress_port</em>&gt;</code>를 앱에 액세스하려는 TCP 포트로 대체합니다.</td>
</tr>
<tr>
<td><code>servicePort</code></td>
<td>이 매개변수는 선택사항입니다. 제공되는 경우, 트래픽이 백엔드 앱으로 전송되기 전에 포트가 이 값으로 대체됩니다. 그렇지 않으면 포트가 Ingress 포트와 동일하게 유지됩니다. 이 매개변수를 설정하지 않으려면 구성에서 이를 제거할 수 있습니다. </td>
</tr>
</tbody></table>


**사용법**</br>
1. ALB를 위한 열린 포트를 검토하십시오.
  ```
  kubectl get service -n kube-system
  ```
  {: pre}

  CLI 출력이 다음과 유사하게 나타납니다.
  ```
  NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP      PORT(S)                      AGE
  public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx    169.xx.xxx.xxx   80:30416/TCP,443:32668/TCP   109d
  ```
  {: screen}

2. ALB 구성 맵을 여십시오.
  ```
  kubectl edit configmap ibm-cloud-provider-ingress-cm -n kube-system
  ```
  {: pre}

3. TCP 포트를 구성 맵에 추가하십시오. 열려는 TCP 포트로 `<port>`를 대체하십시오.
  기본적으로 포트 80 및 443이 열립니다. 80 및 443을 열린 상태로 유지하려면 `public-ports` 필드에 지정하는 다른 TCP 포트 이외에 이러한 포트를 포함해야 합니다. 사설 ALB를 사용으로 설정한 경우 `private-ports` 필드에도 열린 상태로 유지할 포트를 지정해야 합니다. 자세한 정보는 [Ingress ALB에서 포트 열기](/docs/containers?topic=containers-ingress#opening_ingress_ports)를 참조하십시오.
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

4. ALB가 TCP 포트로 다시 구성되었는지 확인하십시오.
  ```
  kubectl get service -n kube-system
  ```
  {: pre}
  CLI 출력이 다음과 유사하게 나타납니다.
  ```
  NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP      PORT(S)                               AGE
  public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx    169.xx.xxx.xxx   <port1>:30776/TCP,<port2>:30412/TCP   109d
  ```
  {: screen}

5. 비표준 TCP 포트를 통해 앱에 액세스하도록 ALB를 구성하십시오. 이 참조서에 있는 샘플 YAML 파일의 `tcp-ports` 어노테이션을 사용하십시오.

6. ALB 리소스를 작성하거나 기존 ALB 구성을 업데이트하십시오.
  ```
  kubectl apply -f myingress.yaml
  ```
  {: pre}

7. Ingress 하위 도메인에 대해 Curl을 실행하여 앱에 액세스하십시오. 예: `curl <domain>:<ingressPort>`

<br />


## 경로 라우팅 어노테이션
{: #path-routing}

Ingress ALB는 백엔드 애플리케이션이 청취하는 경로로 트래픽을 라우팅합니다. 경로 라우팅 어노테이션을 사용하여 ALB가 앱으로 트래픽을 라우팅하는 방법을 구성할 수 있습니다.
{: shortdesc}

### 외부 서비스(`proxy-external-service`)
{: #proxy-external-service}

외부 서비스(예: {{site.data.keyword.Bluemix_notm}}에서 호스팅되는 서비스)에 경로 정의를 추가합니다.
{:shortdesc}

**설명**</br>
외부 서비스에 경로 정의를 추가합니다. 앱이 백엔드 서비스 대신 외부 서비스에서 작동할 때만 이 어노테이션을 사용하십시오. 이 어노테이션을 사용하여 외부 서비스 라우트를 작성하는 경우, `client-max-body-size`, `proxy-read-timeout`, `proxy-connect-timeout`, `proxy-buffering` 어노테이션만 함께 지원됩니다. 기타 어노테이션은 `proxy-external-service`와 함께 지원되지 않습니다.

단일 서비스와 경로에 대해 다중 호스트를 지정할 수 없습니다.
{: note}

**샘플 Ingress 리소스 YAML**</br>
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
<caption>어노테이션 컴포넌트 이해</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
</thead>
<tbody>
<tr>
<td><code>path</code></td>
<td><code>&lt;<em>mypath</em>&gt;</code>를 외부 서비스에서 청취하는 경로로 대체합니다.</td>
</tr>
<tr>
<td><code>external-svc</code></td>
<td><code>&lt;<em>external_service</em>&gt;</code>를 호출하려는 외부 서비스로 대체합니다. 예: <code>https://&lt;myservice&gt;.&lt;region&gt;.mybluemix.net</code>.</td>
</tr>
<tr>
<td><code>host</code></td>
<td><code>&lt;<em>mydomain</em>&gt;</code>을 외부 서비스의 호스트 도메인으로 대체합니다.</td>
</tr>
</tbody></table>

<br />


### 위치 수정자(`location-modifier`)
{: #location-modifier}

ALB가 앱 경로에 대해 요청 URI를 일치시키는 방법을 수정합니다.
{:shortdesc}

**설명**</br>
기본적으로, ALB는 앱이 청취하는 경로를 접두부로 처리합니다. ALB가 앱에 대한 요청을 수신하는 경우 ALB는 요청 URI의 시작과 일치하는 경로(접두부로)의 Ingress 리소스를 처리합니다. 일치 항목이 있으면 요청은 앱이 배치된 팟(Pod)의 IP 주소로 전달됩니다.

`location-modifier` 어노테이션은 ALB가 위치 블록 구성을 수정하여 일치 항목을 검색하는 방법을 변경합니다. 위치 블록은 앱 경로에 대한 요청을 처리하는 방법을 결정합니다.

정규식(regex) 경로를 처리하려면 이 어노테이션이 필요합니다.
{: note}

**지원되는 수정자**</br>
<table>
<caption>지원되는 수정자</caption>
<col width="10%">
<col width="90%">
<thead>
<th>수정자</th>
<th>설명</th>
</thead>
<tbody>
<tr>
<td><code>=</code></td>
<td>등호 수정자는 ALB가 정확히 일치하는 항목만 선택하도록 합니다. 정확하게 일치하는 항목을 찾으면 검색을 중지하고 일치하는 경로가 선택됩니다.<br>예를 들어, 앱이 <code>/tea</code>에서 청취하는 경우 ALB가 앱에 대한 요청을 일치시킬 때 정확한 <code>/tea</code> 경로만 선택합니다.</td>
</tr>
<tr>
<td><code>~</code></td>
<td>물결 기호 수정자는 ALB가 비교 중에 경로를 대소문자를 구분하는 정규식 경로로 처리하도록 합니다.<br>예를 들어, 앱이 <code>/coffee</code>에서 청취하면 경로가 앱에 대해 명시적으로 설정되지 않은 경우에도 ALB가 앱에 대한 요청을 일치시킬 때 <code>/ab/coffee</code> 또는 <code>/123/coffee</code> 경로를 선택할 수 있습니다.</td>
</tr>
<tr>
<td><code>~\*</code></td>
<td>물결 기호와 그 뒤의 별표는 ALB가 비교 중에 경로를 대소문자를 구분하지 않는 정규식 경로로 처리하도록 합니다.<br>예를 들어, 앱이 <code>/coffee</code>에서 청취하면 경로가 앱에 대해 명시적으로 설정되지 않은 경우에도 ALB가 앱에 대한 요청을 일치시킬 때 <code>/ab/Coffee</code> 또는 <code>/123/COFFEE</code> 경로를 선택할 수 있습니다.</td>
</tr>
<tr>
<td><code>^~</code></td>
<td>캐럿과 그 뒤의 물결 기호는 ALB가 정규식 경로 대신 정규식이 아닌 최적 일치 항목을 선택하도록 합니다.</td>
</tr>
</tbody>
</table>


**샘플 Ingress 리소스 YAML**</br>
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
<caption>어노테이션 컴포넌트 이해</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
</thead>
<tbody>
<tr>
<td><code>modifier</code></td>
<td><code>&lt;<em>location_modifier</em>&gt;</code>를 경로에 사용할 위치 수정자로 대체합니다. 지원되는 수정자는 <code>'='</code>, <code>'~'</code>, <code>'~\*'</code> 및 <code>'^~'</code>입니다. 작은따옴표로 수정자를 묶어야 합니다.</td>
</tr>
<tr>
<td><code>serviceName</code></td>
<td><code>&lt;<em>myservice</em>&gt;</code>를 앱에 대해 작성된 Kubernetes 서비스의 이름으로 대체합니다.</td>
</tr>
</tbody></table>

<br />


### 경로 재작성(`rewrite-path`)
{: #rewrite-path}

ALB 도메인 경로의 수신 네트워크 트래픽을 백엔드 앱이 청취하는 다른 경로로 라우팅합니다.
{:shortdesc}

**설명**</br>
Ingress ALB 도메인은 `mykubecluster.us-south.containers.appdomain.cloud/beans`의 수신 네트워크 트래픽을 사용자의 앱으로 라우팅합니다. 사용자 앱은 `/beans` 대신 `/coffee`를 청취합니다. 수신 네트워크 트래픽을 사용자 앱에 전달하려면 재작성 어노테이션을 Ingress 리소스 구성 파일에 추가하십시오. 재작성 어노테이션에서는 `/coffee` 경로를 사용하여 `/beans`의 수신 네트워크 트래픽을 사용자 앱으로 전달할 수 있습니다. 여러 서비스를 포함하는 경우에는 세미콜론(;) 앞뒤에 공백 없이, 세미콜론만 사용하여 이를 구분하십시오. 

**샘플 Ingress 리소스 YAML**</br>

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
<caption>어노테이션 컴포넌트 이해</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td><code>&lt;<em>myservice</em>&gt;</code>를 앱에 대해 작성된 Kubernetes 서비스의 이름으로 대체합니다.</td>
</tr>
<tr>
<td><code>rewrite</code></td>
<td><code>&lt;<em>target_path</em>&gt;</code>를 앱에서 청취하는 경로로 대체합니다. ALB 도메인의 수신 네트워크 트래픽은 이 경로를 사용하여 Kubernetes 서비스로 전달됩니다. 대부분의 앱은 특정 경로에서 청취하지 않지만 루트 경로와 특정 포트를 사용합니다. <code>mykubecluster.us-south.containers.appdomain.cloud/beans</code>의 예에서 재작성 경로는 <code>/coffee</code>입니다. <p class= "note">이 파일을 적용하고 URL에 <code>404</code> 응답이 표시되면 백엔드 앱은 `/`로 끝나는 경로에서 청취할 수도 있습니다. 이 재작성 필드에 후행 `/`를 추가한 다음 파일을 다시 적용하고 URL을 다시 시도합니다.</p></td>
</tr>
</tbody></table>

<br />


## 프록시 버퍼 어노테이션
{: #proxy-buffer}

Ingress ALB는 백엔드 앱과 클라이언트 웹 브라우저 사이의 프록시 역할을 합니다. 프록시 버퍼 어노테이션을 사용하면 데이터 패킷을 전송하거나 수신할 때 ALB에서 데이터가 버퍼링되는 방법을 구성할 수 있습니다.  
{: shortdesc}

### 대형 클라이언트 헤더 버퍼(`large-client-header-buffers`)
{: #large-client-header-buffers}

대형 클라이언트 요청 헤더를 읽는 최대 버퍼의 수 및 크기를 설정합니다.
{:shortdesc}

**설명**</br>
대형 클라이언트 요청 헤더를 읽는 버퍼는 요청 시에만 할당됩니다. 요청 처리가 끝난 후 연결이 keepalive 상태로 전이되는 경우 이 버퍼가 해제됩니다. 기본적으로, `4`개의 버퍼가 있으며 버퍼 크기는 `8K`바이트와 같습니다. 요청 행이 한 개 버퍼의 설정 최대 크기를 초과하는 경우 `414 Request-URI Too Large` HTTP 오류가 클라이언트에 리턴됩니다. 또한 요청 헤더 필드가 한 개 버퍼의 설정 최대 크기를 초과하는 경우 `400 Bad Request Large` 오류가 클라이언트에 리턴됩니다. 대형 클라이언트 요청 헤더를 읽는 데 사용된 버퍼의 최대 수 및 크기를 조정할 수 있습니다.

**샘플 Ingress 리소스 YAML**</br>
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
<caption>어노테이션 컴포넌트 이해</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
 </thead>
 <tbody>
 <tr>
 <td><code>&lt;number&gt;</code></td>
 <td>대형 클라이언트 요청 헤더를 읽는 데 할당되어야 하는 최대 버퍼의 수입니다. 예를 들어, 4로 설정하려면 <code>4</code>를 정의하십시오.</td>
 </tr>
 <tr>
 <td><code>&lt;size&gt;</code></td>
 <td>대형 클라이언트 요청 헤더를 읽는 최대 버퍼의 크기입니다. 예를 들어, 16KB로 설정하려면 <code>16k</code>를 정의하십시오. 크기는 KB의 경우 <code>k</code>로 끝나고 MB의 경우 <code>m</code>으로 끝나야 합니다.</td>
 </tr>
</tbody></table>

<br />


### 클라이언트 응답 데이터 버퍼링(`proxy-buffering`)
{: #proxy-buffering}

데이터를 클라이언트로 전송하는 동안 ALB에서 응답 데이터의 스토리지를 사용하지 않도록 설정하려면 버퍼 어노테이션을 사용하십시오.
{:shortdesc}

**설명**</br>
Ingress ALB는 백엔드 앱과 클라이언트 웹 브라우저 사이의 프록시 역할을 합니다. 백엔드 앱에서 클라이언트로 응답을 전송하면 기본적으로 응답 데이터가 ALB에서 버퍼링됩니다. ALB는 클라이언트 응답을 프록시하고 클라이언트의 속도에 맞춰 클라이언트로 응답을 전송하기 시작합니다. ALB가 백엔드 앱으로부터 모든 데이터를 수신하면 백엔드 앱에 대한 연결이 종료됩니다. 클라이언트가 모든 데이터를 수신할 때까지 ALB에서 클라이언트로의 연결은 열린 상태로 남아 있습니다.

ALB에서 응답 데이터 버퍼링을 사용하지 않으면 ALB에서 클라이언트로 데이터가 즉시 전송됩니다. 클라이언트는 ALB의 속도에 따라 수신되는 데이터를 처리할 수 있어야 합니다. 클라이언트가 너무 느린 경우에는 클라이언트가 따라잡을 수 있을 때까지 업스트림 연결이 열린 상태로 유지됩니다. 

ALB에서 응답 데이터 버퍼링은 기본적으로 사용으로 설정됩니다.

**샘플 Ingress 리소스 YAML**</br>
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
<caption>어노테이션 컴포넌트 이해</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
</thead>
<tbody>
<tr>
<td><code>enabled</code></td>
<td>ALB에서 응답 데이터 버퍼링을 사용하지 않도록 설정하려면 <code>false</code>로 설정하십시오.</td>
</tr>
<tr>
<td><code>serviceName</code></td>
<td><code><em>&lt;myservice1&gt;</em></code>을 앱을 대해 작성한 Kubernetes 서비스의 이름으로 대체하십시오. 여러 서비스는 세미콜론(;)으로 구분하십시오. 이 필드는 선택사항입니다. 서비스 이름을 지정하지 않으면 모든 서비스에서 이 어노테이션을 사용합니다.</td>
</tr>
</tbody></table>

<br />


### 프록시 버퍼(`proxy-buffers`)
{: #proxy-buffers}

ALB를 위한 프록시 버퍼 수와 크기를 구성하십시오.
{:shortdesc}

**설명**</br>
프록시된 서버에서 단일 연결에 대한 응답을 읽는 데 사용되는 버퍼의 수와 크기를 설정합니다. 서비스가 지정되지 않으면 구성은 Ingress 하위 도메인의 모든 서비스에 적용됩니다. 예를 들어, `serviceName=SERVICE number=2 size=1k`와 같은 구성이 지정되면 1k가 서비스에 적용됩니다. `number=2 size=1k`와 같은 구성이 지정되면 1k가 Ingress 하위 도메인의 모든 서비스에 적용됩니다.</br>
<p class="tip">오류 메시지 `upstream sent too big header while reading response header from upstream`을 수신하는 경우에는 백엔드의 업스트림 서버가 기본 한계보다 큰 헤더 크기를 전송한 것입니다. `proxy-buffers` 및 [`proxy-buffer-size`](#proxy-buffer-size) 둘 다의 크기를 늘리십시오.</p>

**샘플 Ingress 리소스 YAML**</br>

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
<caption>어노테이션 컴포넌트 이해</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td><code>&lt;<em>myservice</em>&gt;</code>를 프록시-버퍼를 적용할 서비스 이름으로 대체합니다.</td>
</tr>
<tr>
<td><code>number</code></td>
<td><code>&lt;<em>number_of_buffers</em>&gt;</code>를 숫자(예: <code>2</code>)로 대체합니다.</td>
</tr>
<tr>
<td><code>size</code></td>
<td><code>&lt;<em>size</em>&gt;</code>를 각 버퍼의 크기(킬로바이트, k 또는 K)로 대체합니다(예: <code>1K</code>).</td>
</tr>
</tbody>
</table>

<br />


### 프록시 버퍼 크기(`proxy-buffer-size`)
{: #proxy-buffer-size}

응답의 첫 번째 파트를 읽는 프록시 버퍼의 크기를 구성합니다.
{:shortdesc}

**설명**</br>
프록시된 서버에서 수신되는 응답의 첫 번째 파트를 읽는 데 사용되는 버퍼의 크기를 설정합니다. 응답의 이 파트는 대개 작은 응답 헤더를 포함합니다. 서비스가 지정되지 않으면 구성은 Ingress 하위 도메인의 모든 서비스에 적용됩니다. 예를 들어, `serviceName=SERVICE size=1k`와 같은 구성이 지정되면 1k가 서비스에 적용됩니다. `size=1k`와 같은 구성이 지정되면 1k가 Ingress 하위 도메인의 모든 서비스에 적용됩니다.

오류 메시지 `upstream sent too big header while reading response header from upstream`을 수신하는 경우에는 백엔드의 업스트림 서버가 기본 한계보다 큰 헤더 크기를 전송한 것입니다. `proxy-buffer-size` 및 [`proxy-buffers`](#proxy-buffers) 둘 다의 크기를 늘리십시오.
{: tip}

**샘플 Ingress 리소스 YAML**</br>
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
<caption>어노테이션 컴포넌트 이해</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td><code>&lt;<em>myservice</em>&gt;</code>를 프록시-버퍼-크기를 적용할 서비스 이름으로 대체합니다.</td>
</tr>
<tr>
<td><code>size</code></td>
<td><code>&lt;<em>size</em>&gt;</code>를 각 버퍼의 크기(킬로바이트, k 또는 K)로 대체합니다(예: <code>1K</code>). 적절한 크기를 계산하기 위해 [이 블로그 게시물 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.getpagespeed.com/server-setup/nginx/tuning-proxy_buffer_size-in-nginx)을 확인할 수 있습니다. </td>
</tr>
</tbody></table>

<br />


### 프록시의 사용 중인 버퍼 크기(`proxy-busy-buffers-size`)
{: #proxy-busy-buffers-size}

사용 중일 수 있는 프록시 버퍼 크기를 구성합니다.
{:shortdesc}

**설명**</br>
응답을 완전히 읽기 전에 클라이언트에 응답을 보내는 모든 버퍼의 크기를 제한합니다. 한편 나머지 버퍼는 응답을 읽을 수 있으며 필요한 경우 응답의 일부를 임시 파일로 버퍼링할 수 있습니다. 서비스가 지정되지 않으면 구성은 Ingress 하위 도메인의 모든 서비스에 적용됩니다. 예를 들어, `serviceName=SERVICE size=1k`와 같은 구성이 지정되면 1k가 서비스에 적용됩니다. `size=1k`와 같은 구성이 지정되면 1k가 Ingress 하위 도메인의 모든 서비스에 적용됩니다.

**샘플 Ingress 리소스 YAML**</br>
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
<caption>어노테이션 컴포넌트 이해</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td><code>&lt;<em>myservice</em>&gt;</code>를 프록시-사용-버퍼-크기를 적용할 서비스 이름으로 대체합니다.</td>
</tr>
<tr>
<td><code>size</code></td>
<td><code>&lt;<em>size</em>&gt;</code>를 각 버퍼의 크기(킬로바이트, k 또는 K)로 대체합니다(예: <code>1K</code>).</td>
</tr>
</tbody></table>

<br />


## 요청 및 응답 어노테이션
{: #request-response}

요청 및 응답 어노테이션을 사용하면 클라이언트 및 서버 요청의 헤더 정보를 추가 또는 제거하고 클라이언트가 전송할 수 있는 본문의 크기를 변경할 수 있습니다.
{: shortdesc}

### 호스트 헤더에 서버 포트 추가(`add-host-port`)
{: #add-host-port}

요청이 백엔드에 전달되기 전에 서버 포트를 클라이언트 요청에 추가하십시오.
{: shortdesc}

**설명**</br>
요청을 백엔드 앱에 전달하기 전에 `:server_port`를 클라이언트 요청의 호스트 헤더에 추가하십시오.

**샘플 Ingress 리소스 YAML**</br>
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
<caption>어노테이션 컴포넌트 이해</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
</thead>
<tbody>
<tr>
<td><code>enabled</code></td>
<td>하위 도메인에 대해 server_port의 설정을 사용하려면 <code>true</code>로 설정하십시오.</td>
</tr>
<tr>
<td><code>serviceName</code></td>
<td><code><em>&lt;myservice&gt;</em></code>를 앱에 대해 작성된 Kubernetes 서비스의 이름으로 대체합니다. 여러 서비스는 세미콜론(;)으로 구분하십시오. 이 필드는 선택사항입니다. 서비스 이름을 지정하지 않으면 모든 서비스에서 이 어노테이션을 사용합니다.</td>
</tr>
</tbody></table>

<br />


### 추가 클라이언트 요청 또는 응답 헤더(`proxy-add-headers`, `response-add-headers`)
{: #proxy-add-headers}

백엔드 앱에 요청을 전송하기 전에 클라이언트 요청에 헤더 정보를 추가하거나 클라이언트에 응답을 전송하기 전에 클라이언트 응답에 헤더 정보를 추가합니다.
{:shortdesc}

**설명**</br>
Ingress ALB는 클라이언트 앱과 백엔드 앱 사이의 프록시 역할을 합니다. ALB로 전송된 클라이언트 요청은 처리(프록시)되어 이후에 백엔드 앱으로 전송되는 새 요청에 넣어집니다. 이와 유사하게, ALB로 전송된 백엔드 앱 응답이 처리(프록시)되며 다시 클라이언트로 전송되는 새 요청에 놓입니다. 요청 또는 응답을 프록시하면 클라이언트 또는 백엔드 앱에서 처음에 전송된 HTTP 헤더 정보(예: 사용자 이름)가 제거됩니다.

백엔드 앱에서 HTTP 헤더 정보가 필요하면 ALB가 백엔드 앱으로 요청을 전달하기 전에 `proxy-add-headers` 어노테이션을 사용하여 클라이언트 요청에 헤더 정보를 추가할 수 있습니다. 클라이언트 웹 앱에서 HTTP 헤더 정보가 필요하면 ALB가 클라이언트 웹 앱으로 응답을 전달하기 전에 `response-add-headers` 어노테이션을 사용하여 응답에 헤더 정보를 추가할 수 있습니다.<br>

예를 들어, 앱에 요청을 전송하기 전에 다음 X-Forward 헤더 정보를 요청에 추가해야 할 수 있습니다.
```
proxy_set_header Host $host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-Proto $scheme;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
```
{: screen}

X-Forward 헤더 정보를 앱에 전송된 요청에 추가하려면 다음과 같은 방법으로 `proxy-add-headers` 어노테이션을 사용하십시오.
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

`response-add-headers` 어노테이션은 모든 서비스에 대해 글로벌 헤더를 지원하지 않습니다. 서비스 레벨에서 모든 서비스 응답에 대해 헤더를 추가하려면 다음 [`server-snippets` 어노테이션](#server-snippets)을 사용할 수 있습니다.
{: tip}
```
annotations:
  ingress.bluemix.net/server-snippets: |
    add_header <header1> <value1>;
```
{: pre}
</br>

**샘플 Ingress 리소스 YAML**</br>
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
<caption>어노테이션 컴포넌트 이해</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
</thead>
<tbody>
<tr>
<td><code>service_name</code></td>
<td><code>&lt;<em>myservice</em>&gt;</code>를 앱에 대해 작성된 Kubernetes 서비스의 이름으로 대체합니다.</td>
</tr>
<tr>
<td><code>&lt;header&gt;</code></td>
<td>클라이언트 요청 또는 클라이언트 응답에 추가할 헤더 정보의 키입니다.</td>
</tr>
<tr>
<td><code>&lt;value&gt;</code></td>
<td>클라이언트 요청 또는 클라이언트 응답에 추가할 헤더 정보의 값입니다.</td>
</tr>
</tbody></table>

<br />


### 클라이언트 응답 헤더 제거(`response-remove-headers`)
{: #response-remove-headers}

응답을 클라이언트에 전송하기 전에 백엔드 앱에서 클라이언트 응답에 포함되는 헤더 정보를 제거합니다.
{:shortdesc}

**설명**</br>
Ingress ALB는 백엔드 앱과 클라이언트 웹 브라우저 사이의 프록시 역할을 합니다. ALB로 전송된 백엔드 앱의 클라이언트 응답은 처리(프록시)되어 새 응답에 넣어진 후 새 응답이 ALB에서 클라이언트 웹 브라우저로 전송됩니다. 응답을 프록시 처리하면 백엔드 앱에서 처음에 전송된 http 헤더 정보가 제거되지만 이 프로세스에서는 모든 백엔드 앱 고유 헤더를 제거하지 않을 수 있습니다. 응답이 ALB에서 클라이언트 웹 브라우저로 전달되기 전에 클라이언트 응답에서 헤더 정보를 제거하십시오.

**샘플 Ingress 리소스 YAML**</br>

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
<caption>어노테이션 컴포넌트 이해</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
</thead>
<tbody>
<tr>
<td><code>service_name</code></td>
<td><code>&lt;<em>myservice</em>&gt;</code>를 앱에 대해 작성된 Kubernetes 서비스의 이름으로 대체합니다.</td>
</tr>
<tr>
<td><code>&lt;header&gt;</code></td>
<td>클라이언트 응답에서 제거할 헤더의 키입니다.</td>
</tr>
</tbody></table>

<br />


### 클라이언트 요청 본문 크기(`client-max-body-size`)
{: #client-max-body-size}

클라이언트가 요청의 일부로 전송할 수 있는 최대 본문 크기를 설정합니다.
{:shortdesc}

**설명**</br>
예상 성능을 유지보수하려면 최대 클라이언트 요청 본문 크기를 1MB로 설정하십시오. 본문 크기가 한계를 초과하는 클라이언트 요청을 Ingress ALB로 전송한 경우, 클라이언트가 데이터를 분할하지 못하게 하면 ALB는 413(요청 엔티티가 너무 큼) HTTP 응답을 클라이언트로 리턴합니다. 요청 본문 크기를 줄일 때까지 클라이언트와 ALB 사이에 연결이 불가능합니다. 클라이언트에서 데이터를 여러 청크로 분할하도록 허용하는 경우, 데이터는 1MB의 패키지로 나뉘어 ALB로 전송됩니다.

본문 크기가 1MB보다 큰 클라이언트 요청을 예상하므로 최대 본문 크기를 늘릴 수 있습니다. 예를 들어, 클라이언트에서 큰 파일을 업로드할 수 있게 합니다. 요청을 받을 때까지 클라이언트에 대한 연결이 열린 상태를 유지해야 하므로 최대 요청 본문 크기를 늘리면 ALB의 성능에 영향을 미칠 수 있습니다.

일부 클라이언트 웹 브라우저에서는 413 HTTP 응답 메시지를 제대로 표시할 수 없습니다.
{: note}

**샘플 Ingress 리소스 YAML**</br>
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
<caption>어노테이션 컴포넌트 이해</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>선택사항: 특정 서비스에 클라이언트 최대 본문 크기를 적용하려면 <code>&lt;<em>myservice</em>&gt;</code>를 서비스의 이름으로 대체하십시오. 서비스 이름을 지정하지 않으면 해당 크기가 모든 서비스에 적용됩니다. YAML 예에서, <code>"serviceName=&lt;myservice&gt; size=&lt;size&gt;; size=&lt;size&gt;"</code> 형식은 <code>myservice</code> 서비스에 첫 번째 크기를 적용하고 모든 기타 서비스에 두 번째 크기를 적용합니다. </li>
</tr>
<td><code>&lt;size&gt;</code></td>
<td>클라이언트 응답 본문의 최대 크기입니다. 예를 들어, 최대 크기를 200MB로 설정하려면 <code>200m</code>을 정의하십시오. 클라이언트 요청 본문 크기의 검사를 사용 안함으로 설정하려면 크기를 0으로 설정할 수 있습니다.</td>
</tr>
</tbody></table>

<br />


## 서비스 제한 어노테이션
{: #service-limit}

서비스 제한 어노테이션을 사용하면 기본 요청 처리 속도 및 단일 IP 주소에서 수신 가능한 연결의 수를 변경할 수 있습니다.
{: shortdesc}

### 글로벌 속도 제한(`global-rate-limit`)
{: #global-rate-limit}

모든 서비스의 정의된 키별 연결 수와 요청 처리 속도를 제한합니다.
{:shortdesc}

**설명**</br>
모든 서비스의 경우, 선택된 백엔드의 모든 경로에 대한 단일 IP 주소에서 수신되는 정의된 키마다 연결 수와 요청 처리 속도를 제한합니다.

**샘플 Ingress 리소스 YAML**</br>
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
<caption>어노테이션 컴포넌트 이해</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
</thead>
<tbody>
<tr>
<td><code>key</code></td>
<td>지원되는 값은 `location`, `$http_` 헤더 및 `$uri`입니다. 구역 또는 서비스를 기반으로 수신 요청에 대한 글로벌 한계를 설정하려면 `key=location`을 사용하십시오. 헤더를 기반으로 수신 요청의 글로벌 한계를 설정하려면 `X-USER-ID key=$http_x_user_id`를 사용하십시오.</td>
</tr>
<tr>
<td><code>rate</code></td>
<td><code>&lt;<em>rate</em>&gt;</code>를 처리 속도로 대체합니다. 값을 초당 비율(r/s) 또는 분당 비율(r/m)로 입력하십시오. 예: <code>50r/m</code>.</td>
</tr>
<tr>
<td><code>conn</code></td>
<td><code>&lt;<em>number-of-connections</em>&gt;</code>을 연결 수로 대체합니다.</td>
</tr>
</tbody></table>

<br />


### 서비스 속도 제한(`service-rate-limit`)
{: #service-rate-limit}

특정 서비스의 연결 수와 요청 처리 속도를 제한합니다.
{:shortdesc}

**설명**</br>
특정 서비스의 경우, 선택된 백엔드의 모든 경로에 대한 단일 IP 주소에서 수신되는 정의된 키마다 연결 수와 요청 처리 속도를 제한합니다.

**샘플 Ingress 리소스 YAML**</br>
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
<caption>어노테이션 컴포넌트 이해</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td><code>&lt;<em>myservice</em>&gt;</code>를 처리 속도를 제한하려는 서비스 이름으로 대체합니다.</li>
</tr>
<tr>
<td><code>key</code></td>
<td>지원되는 값은 `location`, `$http_` 헤더 및 `$uri`입니다. 구역 또는 서비스를 기반으로 수신 요청에 대한 글로벌 한계를 설정하려면 `key=location`을 사용하십시오. 헤더를 기반으로 수신 요청의 글로벌 한계를 설정하려면 `X-USER-ID key=$http_x_user_id`를 사용하십시오.</td>
</tr>
<tr>
<td><code>rate</code></td>
<td><code>&lt;<em>rate</em>&gt;</code>를 처리 속도로 대체합니다. 초당 비율을 정의하려면 r/s를 사용합니다(<code>10r/s</code>). 분당 비율을 정의할 때는 r/m을 사용합니다(<code>50r/m</code>).</td>
</tr>
<tr>
<td><code>conn</code></td>
<td><code>&lt;<em>number-of-connections</em>&gt;</code>을 연결 수로 대체합니다.</td>
</tr>
</tbody></table>

<br />


## 사용자 인증 어노테이션
{: #user-authentication}

{{site.data.keyword.appid_full_notm}}를 사용하여 앱으로 인증하려면 인증 어노테이션을 사용합니다.
{: shortdesc}

### {{site.data.keyword.appid_short_notm}} 인증(`appid-auth`)
{: #appid-auth}

{{site.data.keyword.appid_full_notm}}를 사용하여 앱으로 인증합니다.
{:shortdesc}

**설명**</br>
{{site.data.keyword.appid_short_notm}}를 사용하여 웹 또는 API HTTP/HTTPS 요청을 인증합니다.

요청 유형을 web으로 설정하면 {{site.data.keyword.appid_short_notm}} 액세스 토큰이 포함된 웹 요청이 유효성 검증됩니다. 토큰 유효성 검증에 실패하는 경우 웹 요청이 거부됩니다. 요청에 액세스 토큰이 포함되지 않으면 요청이 {{site.data.keyword.appid_short_notm}} 로그인 페이지로 경로 재지정됩니다. {{site.data.keyword.appid_short_notm}} 웹 인증이 작동하려면 사용자의 브라우저에서 쿠키가 사용으로 설정되어야 합니다.

요청 유형을 api로 설정하면 {{site.data.keyword.appid_short_notm}} 액세스 토큰이 포함된 API 요청이 유효성 검증됩니다. 요청에 액세스 토큰이 포함되지 않으면 401: Unauthorized 오류 메시지가 사용자에게 리턴됩니다.

보안상의 이유로 {{site.data.keyword.appid_short_notm}} 인증은 TLS/SSL이 사용 설정된 백엔드만 지원합니다.
{: note}

**샘플 Ingress 리소스 YAML**</br>
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
<caption>어노테이션 컴포넌트 이해</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
</thead>
<tbody>
<tr>
<td><code>bindSecret</code></td>
<td><em><code>&lt;bind_secret&gt;</code></em>을 {{site.data.keyword.appid_short_notm}} 서비스 인스턴스에 대한 바인드 시크릿을 저장하는 Kubernetes 시크릿으로 대체합니다.</td>
</tr>
<tr>
<td><code>namespace</code></td>
<td><em><code>&lt;namespace&gt;</code></em>를 바인드 시크릿으로 대체합니다. 이 필드의 기본값은 `default` 네임스페이스입니다.</td>
</tr>
<tr>
<td><code>requestType</code></td>
<td><code><em>&lt;request_type&gt;</em></code>을 {{site.data.keyword.appid_short_notm}}에 전송할 요청의 유형으로 대체합니다. 허용되는 값은 `web` 또는 `api`입니다. 기본값은 `api`입니다.</td>
</tr>
<tr>
<td><code>serviceName</code></td>
<td><code><em>&lt;myservice&gt;</em></code>를 앱에 대해 작성된 Kubernetes 서비스의 이름으로 대체합니다. 이는 필수 필드입니다. 서비스 이름이 포함되지 않으면 어노테이션은 모든 서비스에 대해 사용으로 설정됩니다.  서비스 이름이 포함되면 어노테이션은 해당 서비스에 대해서만 사용으로 설정됩니다. 여러 개의 서비스는 쉼표(,)로 구분하십시오.</td>
</tr>
<tr>
<td><code>idToken=false</code></td>
<td>선택사항: Liberty OIDC 클라이언트는 액세스 및 ID 토큰을 모두 동시에 구문 분석할 수 없습니다. Liberty에 대해 작업하는 경우 ID 토큰이 Liberty 서버로 전송되지 않도록 이 값을 false로 설정하십시오.</td>
</tr>
</tbody></table>

**사용법**</br>

앱이 인증에 {{site.data.keyword.appid_short_notm}}를 사용하므로, 사용자는 {{site.data.keyword.appid_short_notm}} 인스턴스를 프로비저닝하고 유효한 경로 재지정 URI로 인스턴스를 구성한 후에 해당 인스턴스를 클러스터에 바인딩하여 바인드 시크릿을 생성해야 합니다.

1. 새 {{site.data.keyword.appid_short_notm}} 인스턴스를 작성하거나 기존 인스턴스를 선택하십시오.
  * 기존 인스턴스를 사용하려면 서비스 인스턴스 이름에 공백이 포함되지 않음을 확인하십시오. 공백을 제거하려면 서비스 인스턴스 이름 옆의 추가 옵션 메뉴를 선택하고 **서비스 이름 바꾸기**를 선택하십시오.
  * [새 {{site.data.keyword.appid_short_notm}} 인스턴스](https://cloud.ibm.com/catalog/services/app-id)를 프로비저닝하려면 다음을 수행하십시오.
      1. 자동으로 채워진 **서비스 이름**을 서비스 인스턴스의 고유 이름으로 대체하십시오. 서비스 인스턴스 이름에는 공백이 포함될 수 없습니다.
      2. 클러스터가 배치된 동일 지역을 선택하십시오.
      3. **작성**을 클릭하십시오.

2. 앱에 대한 경로 재지정 URL을 추가하십시오. 경로 재지정 URL은 앱의 콜백 엔드포인트입니다. 피싱 공격 방지를 위해 앱 ID는 경로 재지정 URL의 화이트리스트에 대해 요청 URL의 유효성을 검증합니다.
  1. {{site.data.keyword.appid_short_notm}} 관리 콘솔에서 **인증 관리**로 이동하십시오. 
  2. **ID 제공자** 탭에서 ID 제공자가 선택되었는지 확인하십시오. ID 제공자가 선택되지 않으면 사용자가 인증되지 않지만, 앱에 대한 익명 액세스를 위해 액세스 토큰이 사용자에게 발행됩니다.
  3. **인증 설정** 탭에서 `http://<hostname>/<app_path>/appid_callback` 또는 `https://<hostname>/<app_path>/appid_callback` 형식으로 앱에 대한 경로 재지정 URL을 추가하십시오. 

    {{site.data.keyword.appid_full_notm}}는 로그아웃 기능을 제공합니다. `/logout`이 {{site.data.keyword.appid_full_notm}} 경로에 있으면 쿠키가 제거되고 사용자를 로그인 페이지로 다시 보냅니다. 이 기능을 사용하려면 `/appid_logout`을 도메인에 `https://<hostname>/<app_path>/appid_logout` 형식으로 추가하고 이 URL을 경로 재지정 URL 목록에 포함해야 합니다.
    {: note}

3. {{site.data.keyword.appid_short_notm}} 서비스 인스턴스를 클러스터에 바인드하십시오. 이 명령은 서비스 인스턴스의 서비스 키를 작성합니다. 또는 `--key` 플래그를 포함하여 기존 서비스 키 인증 정보를 사용할 수 있습니다.
  ```
  ibmcloud ks cluster-service-bind --cluster <cluster_name_or_ID> --namespace <namespace> --service <service_instance_name> [--key <service_instance_key>]
  ```
  {: pre}
  서비스가 클러스터에 정상적으로 추가되면 서비스 인스턴스의 인증 정보를 보유하는 클러스터 시크릿이 작성됩니다. CLI 출력 예:
  ```
  ibmcloud ks cluster-service-bind --cluster mycluster --namespace mynamespace --service appid1
  Binding service instance to namespace...
  OK
  Namespace:    mynamespace
  Secret name:  binding-<service_instance_name>
  ```
  {: screen}

4. 클러스터 네임스페이스에서 작성된 시크릿을 가져오십시오.
  ```
  kubectl get secrets --namespace=<namespace>
  ```
  {: pre}

5. 바인드 시크릿 및 클러스터 네임스페이스를 사용하여 `appid-auth` 어노테이션을 Ingress 리소스에 추가하십시오.


