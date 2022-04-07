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


# Ingress 로깅 및 모니터링
{: #ingress_health}

문제를 해결하고 Ingress 구성의 성능을 개선하는 데 도움이 되도록 로깅을 사용자 정의하고 모니터링을 설정합니다.
{: shortdesc}

## Ingress 로그 보기
{: #ingress_logs}

Ingress의 문제점을 해결하거나 Ingress 활동을 모니터링하려는 경우 Ingress 로그를 검토할 수 있습니다.
{: shortdesc}

로그는 Ingress ALB에 대해 자동으로 수집됩니다. ALB 로그를 보려면 2개의 옵션 중에서 선택하십시오.
* 클러스터에서 [Ingress 서비스에 대한 로깅 구성을 작성](/docs/containers?topic=containers-health#configuring)하십시오.
* CLI에서 로그를 확인하십시오. **참고**: `kube-system` 네임스페이스에 대해 [**독자** {{site.data.keyword.Bluemix_notm}} IAM 서비스 역할](/docs/containers?topic=containers-users#platform) 이상의 역할이 있어야 합니다.
    1. ALB에 대한 팟(Pod)의 ID를 가져오십시오.
        ```
        kubectl get pods -n kube-system | grep alb
        ```
        {: pre}

    2. 해당 ALB 팟(Pod)의 로그를 여십시오.
        ```
        kubectl logs <ALB_pod_ID> nginx-ingress -n kube-system
        ```
        {: pre}

</br>기본 Ingress 로그 컨텐츠는 JSON으로 형식화되어 있으며, 클라이언트와 앱 간의 연결 세션을 설명하는 공통 필드를 표시합니다. 기본 필드의 예제 로그는 다음과 같습니다.

```
{"time_date": "2018-08-21T17:33:19+00:00", "client": "108.162.248.42", "host": "albhealth.multizone.us-south.containers.appdomain.cloud", "scheme": "http", "request_method": "GET", "request_uri": "/", "request_id": "c2bcce90cf2a3853694da9f52f5b72e6", "status": 200, "upstream_addr": "192.168.1.1:80", "upstream_status": 200, "request_time": 0.005, "upstream_response_time": 0.005, "upstream_connect_time": 0.000, "upstream_header_time": 0.005}
```
{: screen}

<table>
<caption>기본 Ingress 로그 형식의 필드 이해</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 기본 Ingress 로그 형식의 필드 이해</th>
</thead>
<tbody>
<tr>
<td><code>"time_date": "$time_iso8601"</code></td>
<td>로그가 작성될 때 ISO 8601 표준 형식의 로컬 시간입니다.</td>
</tr>
<tr>
<td><code>"client": "$remote_addr"</code></td>
<td>클라이언트가 사용자 앱에 보낸 요청 패키지의 IP 주소입니다. 이 IP는 다음 상황에 따라 변경될 수 있습니다.<ul><li>앱에 대한 클라이언트 요청이 클러스터에 전송되는 경우, 요청은 ALB를 노출시키는 로드 밸런서 서비스에 대해 팟(Pod)으로 라우팅됩니다. 앱 팟(Pod)이 로드 밸런서 서비스 팟(Pod)과 동일한 작업자 노드에 없는 경우 로드 밸런서는 다른 작업자 노드의 앱 팟(Pod)으로 요청을 전달합니다. 요청 패키지의 소스 IP 주소는 앱 팟(Pod)이 실행 중인 작업자 노드의 공인 IP 주소로 변경됩니다.</li><li>[소스 IP 유지가 사용](/docs/containers?topic=containers-ingress#preserve_source_ip)되는 경우에는 앱에 대한 클라이언트 요청의 원래 IP 주소가 대신 기록됩니다.</li></ul></td>
</tr>
<tr>
<td><code>"host": "$http_host"</code></td>
<td>앱이 액세스할 수 있는 호스트 또는 하위 도메인입니다. 이 하위 도메인은 ALB의 Ingress 리소스 파일에 구성되어 있습니다.</td>
</tr>
<tr>
<td><code>"scheme": "$scheme"</code></td>
<td>요청 유형: <code>HTTP</code> 또는 <code>HTTPS</code>.</td>
</tr>
<tr>
<td><code>"request_method": "$request_method"</code></td>
<td>백엔드 앱에 대한 요청 호출의 메소드입니다(예: <code>GET</code> 또는 <code>POST</code>).</td>
</tr>
<tr>
<td><code>"request_uri": "$uri"</code></td>
<td>앱 경로에 대한 원래 요청 URI입니다. ALB는 앱이 청취하는 경로를 접두부로 처리합니다. ALB가 앱에 대한 클라이언트의 요청을 수신하는 경우, ALB는 요청 URI의 경로과 일치하는 경로(접두부로서)의 Ingress 리소스를 확인합니다.</td>
</tr>
<tr>
<td><code>"request_id": "$request_id"</code></td>
<td>16 랜덤 바이트에서 생성된 고유 요청 ID입니다.</td>
</tr>
<tr>
<td><code>"status": $status</code></td>
<td>연결 세션에 대한 상태 코드입니다.<ul>
<li><code>200</code>: 세션이 성공적으로 완료됨</li>
<li><code>400</code>: 클라이언트 데이터를 구문 분석할 수 없음</li>
<li><code>403</code>: 액세스가 금지됨(예: 특정 클라이언트 IP 주소에 대해 액세스가 제한된 경우)</li>
<li><code>500</code>: 내부 서버 오류</li>
<li><code>502</code>: 잘못된 게이트웨이(예: 업스트림 서버를 선택하거나 이에 접근할 수 없는 경우)</li>
<li><code>503</code>: 사용 불가능한 서비스(예: 연결의 수로 인해 액세스가 제한되는 경우)</li>
</ul></td>
</tr>
<tr>
<td><code>"upstream_addr": "$upstream_addr"</code></td>
<td>업스트림 서버의 UNIX-도메인 소켓에 대한 IP 주소 및 포트 또는 경로입니다. 요청 처리 중에 여러 서버에 접속하는 경우, 해당 주소는 쉼표로 구분됩니다(예: <code>"192.168.1.1:80, 192.168.1.2:80, unix:/tmp/sock"</code>). 요청이 하나의 서버 그룹에서 다른 서버 그룹으로 내부적으로 경로 재지정되는 경우, 서로 다른 그룹의 서버 주소는 콜론으로 구분됩니다(예: <code>"192.168.1.1:80, 192.168.1.2:80, unix:/tmp/sock : 192.168.10.1:80, 192.168.10.2:80"</code>). ALB가 서버를 선택할 수 없는 경우에는 서버 그룹의 이름이 대신 로깅됩니다.</td>
</tr>
<tr>
<td><code>"upstream_status": $upstream_status</code></td>
<td>백엔드 앱의 업스트림 서버에서 가져온 응답의 상태 코드입니다(예: 표준 HTTP 응답 코드). 여러 응답의 상태 코드는 <code>$upstream_addr</code> 변수의 주소와 같이 쉼표와 콜론으로 구분됩니다. ALB가 서버를 선택할 수 없는 경우에는 502(잘못된 게이트웨이) 상태 코드가 로깅됩니다.</td>
</tr>
<tr>
<td><code>"request_time": $request_time</code></td>
<td>밀리초 해상도의 초 단위로 측정되는 요청 처리 시간입니다. 이 시간은 ALB가 클라이언트 요청의 첫 번째 바이트를 읽을 때 시작되며, ALB가 응답의 마지막 바이트를 클라이언트에 전송할 때 중지됩니다. 로그는 요청 처리 시간이 중지된 직후에 쓰여집니다.</td>
</tr>
<tr>
<td><code>"upstream_response_time": $upstream_response_time</code></td>
<td>ALB가 백엔드 앱에 대한 업스트림 서버의 응답을 수신하는 데 걸리는 시간이며, 밀리초 해상도의 시간(초)으로 측정됩니다. 여러 응답의 시간은 <code>$upstream_addr</code> 변수의 주소와 같이 쉼표와 콜론으로 구분됩니다.</td>
</tr>
<tr>
<td><code>"upstream_connect_time": $upstream_connect_time</code></td>
<td>ALB가 백엔드 앱에 대한 업스트림 서버와의 연결을 설정하는 데 걸리는 시간이며, 밀리초 해상도의 시간(초)으로 측정됩니다. Ingress 리소스 구성에서 TLS/SSL을 사용하는 경우, 이 시간에는 핸드쉐이크에서 사용된 시간이 포함됩니다. 여러 연결의 시간은 <code>$upstream_addr</code> 변수의 주소와 같이 쉼표와 콜론으로 구분됩니다.</td>
</tr>
<tr>
<td><code>"upstream_header_time": $upstream_header_time</code></td>
<td>ALB가 백엔드 앱에 대한 업스트림 서버의 응답 헤더를 수신하는 데 걸리는 시간이며, 밀리초 해상도의 시간(초)으로 측정됩니다. 여러 연결의 시간은 <code>$upstream_addr</code> 변수의 주소와 같이 쉼표와 콜론으로 구분됩니다.</td>
</tr>
</tbody></table>

## Ingress 로그 컨텐츠 및 형식 사용자 정의
{: #ingress_log_format}

Ingress ALB에 대해 수집되는 로그의 컨텐츠 및 형식을 사용자 정의할 수 있습니다.
{:shortdesc}

기본적으로 Ingress 로그는 JSON으로 형식화되며 일반적인 로그 필드를 표시합니다. 그러나 전달되는 로그 컴포넌트 및 컴포넌트가 로그 출력에 배열되는 방법을 선택하여 사용자 정의 로그 형식을 작성할 수도 있습니다.

시작하기 전에 `kube-system` 네임스페이스에 대해 [**작성자** 또는 **관리자** {{site.data.keyword.Bluemix_notm}} IAM 서비스 역할](/docs/containers?topic=containers-users#platform)이 있는지 확인하십시오.

1. `ibm-cloud-provider-ingress-cm` configmap 리소스에 대한 구성 파일을 편집하십시오.

    ```
    kubectl edit cm ibm-cloud-provider-ingress-cm -n kube-system
    ```
    {: pre}

2. <code>data</code> 섹션을 추가하십시오. `log-format` 필드를 추가하고 선택적으로 `log-format-escape-json` 필드를 추가하십시오.

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
    <caption>YAML 파일 컴포넌트</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> log-format 구성 이해</th>
    </thead>
    <tbody>
    <tr>
    <td><code>log-format</code></td>
    <td><code>&lt;key&gt;</code>를 로그 컴포넌트의 이름으로 대체하고 <code>&lt;log_variable&gt;</code>을 로그 항목으로 수집할 로그 컴포넌트의 변수로 대체합니다. 로그 항목에 포함할 텍스트 및 구두점을 포함할 수 있습니다(예: 문자열 값 앞뒤에 따옴표 및 로그 컴포넌트를 구분하는 쉼표). 예를 들어, <code>request: "$request"</code>와 같은 컴포넌트를 형식화하면 로그 항목에 다음이 생성됩니다. <code>request: "GET / HTTP/1.1"</code>. 사용할 수 있는 모든 변수 목록은 <a href="http://nginx.org/en/docs/varindex.html">NGINX 변수 색인</a>을 참조하십시오.<br><br>추가 헤더(예: <em>x-custom-ID</em>)를 로깅하려면 다음 키-값 쌍을 사용자 정의 로그 컨텐츠에 추가하십시오. <br><pre class="codeblock"><code>customID: $http_x_custom_id</code></pre> <br>하이픈(<code>-</code>)은 밑줄(<code>_</code>)로 변환되고 <code>$http_</code>는 사용자 정의 헤더 이름 앞에 추가되어야 합니다.</td>
    </tr>
    <tr>
    <td><code>log-format-escape-json</code></td>
    <td>선택사항: 기본적으로, 로그는 텍스트 형식으로 생성됩니다. JSON 형식으로 로그를 생성하려면 <code>log-format-escape-json</code> 필드를 추가하고 <code>true</code> 값을 사용하십시오.</td>
    </tr>
    </tbody></table>

    예를 들어, 로그 형식은 다음 변수를 포함할 수 있습니다.
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

    이 형식에 따른 로그 항목은 다음 예제와 유사합니다.
    ```
    remote_address: 127.0.0.1, remote_user: "dbmanager", time_date: [30/Mar/2018:18:52:17 +0000], request: "GET / HTTP/1.1", status: 401, http_referer: "-", http_user_agent: "Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:47.0) Gecko/20100101 Firefox/47.0", request_id: a02b2dea9cf06344a25611c1d7ad72db
    ```
    {: screen}

    ALB 로그의 기본 형식을 기반으로 하는 사용자 정의 로그 형식을 작성하려면 필요에 따라 다음 섹션을 수정한 후 이를 configmap에 추가합니다.
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

4. 구성 파일을 저장하십시오.

5. configmap 변경사항이 적용되었는지 확인하십시오.

   ```
   kubectl get cm ibm-cloud-provider-ingress-cm -n kube-system -o yaml
   ```
   {: pre}

4. Ingress ALB 로그를 보려면 2개의 옵션 중에서 선택하십시오.
    * 클러스터에서 [Ingress 서비스에 대한 로깅 구성을 작성](/docs/containers?topic=containers-health#logging)하십시오.
    * CLI에서 로그를 확인하십시오.
        1. ALB에 대한 팟(Pod)의 ID를 가져오십시오.
            ```
        kubectl get pods -n kube-system | grep alb
            ```
            {: pre}

        2. 해당 ALB 팟(Pod)의 로그를 여십시오. 로그가 업데이트된 형식을 따르는지 확인하십시오.
            ```
        kubectl logs <ALB_pod_ID> nginx-ingress -n kube-system
            ```
            {: pre}

<br />


## Ingress ALB 모니터링
{: #ingress_monitoring}

클러스터에 메트릭 내보내기 프로그램 및 Prometheus 에이전트를 배치하여 ALB를 모니터링합니다.
{: shortdesc}

ALB 메트릭 내보내기 프로그램은 NGINX 지시문(`vhost_traffic_status_zone`)을 사용하여 각 Ingress ALB 팟(Pod)의 `/status/format/json` 엔드포인트에서 메트릭 데이터를 수집합니다. 메트릭 내보내기 프로그램에서 자동으로 JSON 파일의 각 데이터 필드를 Prometheus에서 읽을 수 있는 메트릭으로 다시 형식화합니다. 그런 다음 Prometheus 에이전트는 내보내기 프로그램에서 생성한 메트릭을 선택하여 Prometheus 대시보드에 메트릭을 표시할 수 있도록 합니다.

### 메트릭 내보내기 프로그램 Helm 차트 설치
{: #metrics-exporter}

메트릭 내보내기 프로그램 Helm 차트를 설치하여 클러스터의 ALB를 모니터합니다.
{: shortdesc}

ALB 메트릭 내보내기 프로그램 팟(Pod)은 ALB가 배치된 동일한 작업자 노드에 배치해야 합니다. ALB가 에지 작업자 노드에서 실행되고 해당 에지 노드가 다른 워크로드 배치를 방지하도록 오염된 경우, 메트릭 내보내기 프로그램 팟(Pod)을 스케줄할 수 없습니다. `kubectl taint node <node_name> dedicated:NoSchedule- dedicated:NoExecute-`를 실행하여 오염을 제거해야 합니다.
{: note}

1.  **중요**: [지시사항에 따라](/docs/containers?topic=containers-helm#public_helm_install) 로컬 시스템에 Helm 클라이언트를 설치하고 서비스 계정이 있는 Helm 서버(tiller)를 설치한 후 {{site.data.keyword.Bluemix_notm}} Helm 저장소를 추가하십시오.

2. 클러스터에 `ibmcloud-alb-metrics-exporter` Helm 차트를 설치하십시오. 이 Helm 차트는 ALB 메트릭 내보내기 프로그램을 배치하고 `kube-system` 네임스페이스에 `alb-metrics-service-account` 서비스 계정을 작성합니다. <alb-ID>를 메트릭을 수집할 ALB로 대체하십시오. 클러스터의 ALB에 대한 ID를 보려면 <code>ibmcloud ks albs --cluster &lt;cluster_name&gt;</code>을 실행하십시오.
  모니터링하려는 각 ALB를 배치해야 합니다.
  {: note}
  ```
  helm install iks-charts/ibmcloud-alb-metrics-exporter --name ibmcloud-alb-metrics-exporter --set metricsNameSpace=kube-system --set albId=<alb-ID>
  ```
  {: pre}

3. 차트 배치 상태를 확인하십시오. 차트가 준비 상태인 경우, 출력의 맨 위 근처에 있는 **STATUS** 필드의 값은 `DEPLOYED`입니다.
  ```
    helm status ibmcloud-alb-metrics-exporter
  ```
  {: pre}

4. `ibmcloud-alb-metrics-exporter` 팟(Pod)이 실행 중인지 확인하십시오.
  ```
    kubectl get pods -n kube-system -o wide
  ```
  {:pre}

  출력 예:
  ```
  NAME                                             READY     STATUS      RESTARTS   AGE       IP               NODE
  ...
  alb-metrics-exporter-868fddf777-d49l5            1/1       Running     0          19s       172.30.xxx.xxx   10.xxx.xx.xxx
  alb-metrics-exporter-868fddf777-pf7x5            1/1       Running     0          19s       172.30.xxx.xxx   10.xxx.xx.xxx
  ```
  {:screen}

5. 선택사항: [Prometheus 에이전트를 설치](#prometheus-agent)하여 내보내기 프로그램에서 생성한 메트릭을 선택하고 Prometheus 대시보드에 메트릭을 표시합니다.

### Prometheus 에이전트 Helm 차트 설치
{: #prometheus-agent}

[메트릭 내보내기 프로그램](#metrics-exporter)을 선택한 후, Prometheus 에이전트 Helm 차트를 설치하여 내보내기 프로그램에서 생성한 메트릭을 선택하고 Prometheus 대시보드에 메트릭을 표시할 수 있도록 할 수 있습니다.
{: shortdesc}

1. https://icr.io/helm/iks-charts/charts/ibmcloud-alb-metrics-exporter-1.0.7.tgz에서 메트릭 내보내기 프로그램 Helm 차트에 대한 TAR 파일을 다운로드하십시오.

2. Prometheus 서브폴더로 이동하십시오.
  ```
  cd ibmcloud-alb-metrics-exporter-1.0.7.tar/ibmcloud-alb-metrics-exporter/subcharts/prometheus
  ```
  {: pre}

3. 클러스터에 Prometheus Helm 차트를 설치하십시오. <ingress_subdomain>을 클러스터의 Ingress 하위 도메인으로 대체하십시오. Prometheus 대시보드의 URL은 기본 Prometheus 하위 도메인 `prom-dash`와 Ingress 하위 도메인의 조합입니다(예 `prom-dash.mycluster-12345.us-south.containers.appdomain.cloud`). 클러스터에 대한 Ingress 하위 도메인을 찾으려면 <code>ibmcloud ks cluster-get --cluster &lt;cluster_name&gt;</code>을 실행하십시오.
  ```
  helm install --name prometheus . --set nameSpace=kube-system --set hostName=prom-dash.<ingress_subdomain>
  ```
  {: pre}

4. 차트 배치 상태를 확인하십시오. 차트가 준비 상태인 경우, 출력의 맨 위 근처에 있는 **STATUS** 필드의 값은 `DEPLOYED`입니다.
    ```
    helm status prometheus
    ```
    {: pre}

5. `prometheus` 팟(Pod)이 실행 중인지 확인하십시오.
    ```
    kubectl get pods -n kube-system -o wide
    ```
    {:pre}

    출력 예:
    ```
    NAME                                             READY     STATUS      RESTARTS   AGE       IP               NODE
    alb-metrics-exporter-868fddf777-d49l5            1/1       Running     0          19s       172.30.xxx.xxx   10.xxx.xx.xxx
    alb-metrics-exporter-868fddf777-pf7x5            1/1       Running     0          19s       172.30.xxx.xxx   10.xxx.xx.xxx
    prometheus-9fbcc8bc7-2wvbk                       1/1       Running     0          1m        172.30.xxx.xxx   10.xxx.xx.xxx
    ```
    {:screen}

6. 브라우저에서 Prometheus 대시보드에 대한 URL을 입력하십시오. 이 호스트 이름의 형식은 `prom-dash.mycluster-12345.us-south.containers.appdomain.cloud`입니다. ALB에 대한 Prometheus 대시보드가 열립니다.

7. 대시보드에 나열된 [ALB](#alb_metrics), [서버](#server_metrics) 및 [업스트림](#upstream_metrics) 메트릭에 대한 추가 정보를 검토하십시오.

### ALB 메트릭
{: #alb_metrics}

`alb-metrics-exporter`는 자동으로 JSON 파일의 각 데이터 필드를 Prometheus에서 읽을 수 있는 메트릭으로 다시 형식화합니다. ALB 메트릭은 연결에 대한 데이터를 수집하고 ALB에서 처리하는 응답을 수집합니다.
{: shortdesc}

ALB 메트릭은 `kube_system_<ALB-ID>_<METRIC-NAME> <VALUE>` 형식입니다. 예를 들어, ALB가 2xx 레벨 상태 코드로 23개의 응답을 수신하면 메트릭은 `kube_system_public_crf02710f54fcc40889c301bfd6d5b77fe_alb1_totalHandledRequest {.. metric="2xx"} 23`으로 형식화됩니다. 여기서, `metric`은 prometheus 레벨입니다.

다음 표에는 지원되는 ALB 메트릭 이름과 다음 형식의 메트릭 레이블이 나열됩니다. `<ALB_metric_name>_<metric_label>`
<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 지원되는 ALB 메트릭</th>
</thead>
<tbody>
<tr>
<td><code>connections_reading</code></td>
<td>클라이언트 연결을 읽는 총 횟수입니다.</td>
</tr>
<tr>
<td><code>connections_accepted</code></td>
<td>허용된 총 클라이언트 연결 수입니다.</td>
</tr>
<tr>
<td><code>connections_active</code></td>
<td>총 활성 클라이언트 연결 수입니다.</td>
</tr>
<tr>
<td><code>connections_handled</code></td>
<td>처리된 총 클라이언트 연결 수입니다.</td>
</tr>
<tr>
<td><code>connections_requests</code></td>
<td>요청된 총 클라이언트 연결 수입니다.</td>
</tr>
<tr>
<td><code>connections_waiting</code></td>
<td>대기하는 총 클라이언트 연결 수입니다.</td>
</tr>
<tr>
<td><code>connections_writing</code></td>
<td>총 쓰기 클라이언트 연결 수입니다.</td>
</tr>
<tr>
<td><code>totalHandledRequest_1xx</code></td>
<td>상태 코드가 1xx인 응답 수입니다.</td>
</tr>
<tr>
<td><code>totalHandledRequest_2xx</code></td>
<td>상태 코드가 2xx인 응답 수입니다.</td>
</tr>
<tr>
<td><code>totalHandledRequest_3xx</code></td>
<td>상태 코드가 3xx인 응답 수입니다.</td>
</tr>
<tr>
<td><code>totalHandledRequest_4xx</code></td>
<td>상태 코드가 4xx인 응답 수입니다.</td>
</tr>
<tr>
<td><code>totalHandledRequest_5xx</code></td>
<td>상태 코드가 5xx인 응답 수입니다.</td>
</tr>
<tr>
<td><code>totalHandledRequest_total</code></td>
<td>클라이언트에서 수신한 클라이언트 요청의 총 수입니다.</td>
  </tr></tbody>
</table>

### 서버 메트릭
{: #server_metrics}

`alb-metrics-exporter`는 자동으로 JSON 파일의 각 데이터 필드를 Prometheus에서 읽을 수 있는 메트릭으로 다시 형식화합니다. 서버 메트릭은 Ingress 리소스에 정의된 하위 도메인의 데이터를 수집합니다. 예를 들면 `dev.demostg1.stg.us.south.containers.appdomain.cloud`입니다.
{: shortdesc}

서버 메트릭은 `kube_system_server_<ALB-ID>_<SUB-TYPE>_<SERVER-NAME>_<METRIC-NAME> <VALUE>` 형식입니다.

`<SERVER-NAME>_<METRIC-NAME>`은 레이블로 형식화됩니다. 예를 들면, `albId="dev_demostg1_us-south_containers_appdomain_cloud",metric="out"`입니다.

예를 들어, 서버가 총 22319바이트를 클라이언트에 보낸 경우 메트릭은 다음과 같이 형식화됩니다.
```
kube_system_server_public_cra6a6eb9e897e41c4a5e58f957b417aec_alb1_bytes{albId="dev_demostg1_us-south_containers_appdomain_cloud",app="alb-metrics-exporter",instance="172.30.140.68:9913",job="kubernetes-pods",kubernetes_namespace="kube-system",kubernetes_pod_name="alb-metrics-exporter-7d495d785c-8wfw4",metric="out",pod_template_hash="3805183417"} 22319
```
{: screen}

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 서버 메트릭 형식 이해</th>
</thead>
<tbody>
<tr>
<td><code>&lt;ALB-ID&gt;</code></td>
<td>ALB ID입니다. 위의 예제에서 ALB ID는 <code>public\_cra6a6eb9e897e41c4a5e58f957b417aec\_alb1</code>입니다.</td>
</tr>
<tr>
<td><code>&lt;SUB-TYPE&gt;</code></td>
<td>메트릭의 하위 유형입니다. 각 하위 유형은 하나 이상의 메트릭 이름에 해당합니다.
<ul>
<li><code>bytes</code>와 <code>processing\_time</code>은 메트릭 <code>in</code>과 <code>out</code>에 해당합니다.</li>
<li><code>cache</code>는 메트릭 <code>bypass</code>, <code>expired</code>, <code>hit</code>, <code>miss</code>, <code>revalidated</code>, <code>scare</code>, <code>stale</code> 및 <code>updating</code>에 해당합니다.</li>
<li><code>requests</code>는 메트릭 <code>requestMsec</code>, <code>1xx</code>, <code>2xx</code>, <code>3xx</code>, <code>4xx</code>, <code>5xx</code> 및 <code>total</code>에 해당합니다.</li></ul>
위의 예제에서 하위 유형은 <code>bytes</code>입니다.</td>
</tr>
<tr>
<td><code>&lt;SERVER-NAME&gt;</code></td>
<td>Ingress 리소스에 정의된 서버의 이름입니다. Prometheus와의 호환성을 유지보수하기 위해 마침표(<code>.</code>)는 밑줄<code>(\_)</code>로 대체됩니다. 위의 예제에서 서버 이름은 <code>dev_demostg1_stg_us_south_containers_appdomain_cloud</code>입니다.</td>
</tr>
<tr>
<td><code>&lt;METRIC_NAME&gt;</code></td>
<td>수집된 메트릭 유형의 이름입니다. 메트릭 이름 목록은 다음 표 "지원되는 서버 메트릭"을 참조하십시오. 위의 예제에서 메트릭 이름은 <code>out</code>입니다.</td>
</tr>
<tr>
<td><code>&lt;VALUE&gt;</code></td>
<td>수집된 메트릭의 값입니다. 위의 예제에서 값은 <code>22319</code>입니다.</td>
</tr>
</tbody></table>

다음 표에는 지원되는 서버 메트릭 이름이 나열되어 있습니다.

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 지원되는 서버 메트릭</th>
</thead>
<tbody>
<tr>
<td><code>in</code></td>
<td>클라이언트에서 수신된 총 바이트 수입니다.</td>
</tr>
<tr>
<td><code>out</code></td>
<td>클라이언트로 전송된 총 바이트 수입니다.</td>
</tr>
<tr>
<td><code>bypass</code></td>
<td>캐시에 있는 임계값(예: 요청 수)을 충족하지 않아 캐시 가능 항목이 원래 서버에서 페치된 횟수입니다.</td>
</tr>
<tr>
<td><code>expired</code></td>
<td>항목이 캐시에서 발견되었지만 만료되어 선택되지 않은 횟수입니다.</td>
</tr>
<tr>
<td><code>hit</code></td>
<td>캐시에서 유효한 항목이 선택된 횟수입니다.</td>
</tr>
<tr>
<td><code>miss</code></td>
<td>유효한 캐시 항목이 캐시에서 발견되지 않고 서버가 원래 서버에서 항목을 페치한 횟수입니다.</td>
</tr>
<tr>
<td><code>revalidated</code></td>
<td>캐시에서 만료된 항목의 유효성을 다시 검증한 횟수입니다.</td>
</tr>
<tr>
<td><code>scarce</code></td>
<td>부족한 메모리를 확보하기 위해 캐시에서 거의 사용되지 않거나 우선순위가 낮은 항목을 제거한 횟수입니다.</td>
</tr>
<tr>
<td><code>stale</code></td>
<td>캐시에서 만료된 항목이 발견된 횟수이지만 다른 요청으로 인해 서버가 원래 서버에서 항목을 페치하여 항목이 캐시에서 선택되었습니다.</td>
</tr>
<tr>
<td><code>업데이트</code></td>
<td>시간이 경과된(stale) 컨텐츠가 업데이트된 횟수입니다.</td>
</tr>
<tr>
<td><code>requestMsec</code></td>
<td>요청 처리 시간의 평균(밀리초)입니다.</td>
</tr>
<tr>
<td><code>1xx</code></td>
<td>상태 코드가 1xx인 응답 수입니다.</td>
</tr>
<tr>
<td><code>2xx</code></td>
<td>상태 코드가 2xx인 응답 수입니다.</td>
</tr>
<tr>
<td><code>3xx</code></td>
<td>상태 코드가 3xx인 응답 수입니다.</td>
</tr>
<tr>
<td><code>4xx</code></td>
<td>상태 코드가 4xx인 응답 수입니다.</td>
</tr>
<tr>
<td><code>5xx</code></td>
<td>상태 코드가 5xx인 응답 수입니다.</td>
</tr>
<tr>
<td><code>total</code></td>
<td>상태 코드가 있는 총 응답 수입니다.</td>
  </tr></tbody>
</table>

### 업스트림 메트릭
{: #upstream_metrics}

`alb-metrics-exporter`는 자동으로 JSON 파일의 각 데이터 필드를 Prometheus에서 읽을 수 있는 메트릭으로 다시 형식화합니다. 업스트림 메트릭은 Ingress 리소스에 정의된 백엔드 서비스의 데이터를 수집합니다.
{: shortdesc}

업스트림 메트릭은 두 가지 방식으로 형식화됩니다.
* [유형 1](#type_one)에는 업스트림 서비스 이름이 포함됩니다.
* [유형 2](#type_two)에는 업스트림 서비스 이름 및 특정 업스트림 팟(Pod) IP 주소가 포함됩니다.

#### 유형 1 업스트림 메트릭
{: #type_one}

업스트림 유형 1 메트릭은 `kube_system_upstream_<ALB-ID>_<SUB-TYPE>_<UPSTREAM-NAME>_<METRIC-NAME> <VALUE>` 형식입니다.
{: shortdesc}

`<UPSTREAM-NAME>_<METRIC-NAME>`은 레이블로 형식화됩니다. 예를 들면, `albId="default-cafe-ingress-dev_demostg1_us-south_containers_appdomain_cloud-coffee-svc",metric="in"`입니다.

예를 들어, 업스트림 서비스가 ALB에서 총 1227바이트를 수신한 경우 메트릭은 다음과 같이 형식화됩니다.
```
kube_system_upstream_public_cra6a6eb9e897e41c4a5e58f957b417aec_alb1_bytes{albId="default-cafe-ingress-dev_demostg1_us-south_containers_appdomain_cloud-coffee-svc",app="alb-metrics-exporter",instance="172.30.140.68:9913",job="kubernetes-pods",kubernetes_namespace="kube-system",kubernetes_pod_name="alb-metrics-exporter-7d495d785c-8wfw4",metric="in",pod_template_hash="3805183417"} 1227
```
{: screen}

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 업스트림 유형 1 메트릭 형식 이해</th>
</thead>
<tbody>
<tr>
<td><code>&lt;ALB-ID&gt;</code></td>
<td>ALB ID입니다. 위의 예제에서 ALB ID는 <code>public\_crf02710f54fcc40889c301bfd6d5b77fe\_alb1</code>입니다.</td>
</tr>
<tr>
<td><code>&lt;SUB-TYPE&gt;</code></td>
<td>메트릭의 하위 유형입니다. 지원되는 값은 <code>bytes</code>, <code>processing\_time</code> 및 <code>requests</code>입니다. 위의 예제에서 하위 유형은 <code>bytes</code>입니다.</td>
</tr>
<tr>
<td><code>&lt;UPSTREAM-NAME&gt;</code></td>
<td>Ingress 리소스에 정의된 업스트림 서비스의 이름입니다. Prometheus와의 호환성을 유지보수하기 위해 마침표(<code>.</code>)는 밑줄<code>(\_)</code>로 대체됩니다. 위의 예제에서 업스트림 이름은 <code>default-cafe-ingress-dev_demostg1_us-south_containers_appdomain_cloud-coffee-svc</code>입니다.</td>
</tr>
<tr>
<td><code>&lt;METRIC_NAME&gt;</code></td>
<td>수집된 메트릭 유형의 이름입니다. 메트릭 이름 목록은 다음 표 "지원되는 업스트림 유형 2 메트릭"을 참조하십시오. 위의 예제에서 메트릭 이름은 <code>in</code>입니다.</td>
</tr>
<tr>
<td><code>&lt;VALUE&gt;</code></td>
<td>수집된 메트릭의 값입니다. 위의 예제에서 값은 <code>1227</code>입니다.</td>
</tr>
</tbody></table>

다음 표에는 지원되는 업스트림 유형 1 메트릭 이름이 나열되어 있습니다.

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 지원되는 업스트림 유형 1 메트릭</th>
</thead>
<tbody>
<tr>
<td><code>in</code></td>
<td>ALB 서버에서 수신된 총 바이트 수입니다.</td>
</tr>
<tr>
<td><code>out</code></td>
<td>ALB 서버로 전송된 총 바이트 수입니다.</td>
</tr>
<tr>
<td><code>1xx</code></td>
<td>상태 코드가 1xx인 응답 수입니다.</td>
</tr>
<tr>
<td><code>2xx</code></td>
<td>상태 코드가 2xx인 응답 수입니다.</td>
</tr>
<tr>
<td><code>3xx</code></td>
<td>상태 코드가 3xx인 응답 수입니다.</td>
</tr>
<tr>
<td><code>4xx</code></td>
<td>상태 코드가 4xx인 응답 수입니다.</td>
</tr>
<tr>
<td><code>5xx</code></td>
<td>상태 코드가 5xx인 응답 수입니다.</td>
</tr>
<tr>
<td><code>total</code></td>
<td>상태 코드가 있는 총 응답 수입니다.</td>
  </tr></tbody>
</table>

#### 유형 2 업스트림 메트릭
{: #type_two}

업스트림 유형 2 메트릭은 `kube_system_upstream_<ALB-ID>_<METRIC-NAME>_<UPSTREAM-NAME>_<POD-IP> <VALUE>` 형식입니다.
{: shortdesc}

`<UPSTREAM-NAME>_<POD-IP>`은 레이블로 형식화됩니다. 예: `albId="default-cafe-ingress-dev_dev_demostg1_us-south_containers_appdomain_cloud-tea-svc",backend="172_30_75_6_80"`

예를 들어, 업스트림 서비스의 평균 요청 처리 시간(업스트림 포함)이 40밀리초인 경우 메트릭은 다음과 같이 형식화됩니다.
```
kube_system_upstream_public_cra6a6eb9e897e41c4a5e58f957b417aec_alb1_requestMsec{albId="default-cafe-ingress-dev_dev_demostg1_us-south_containers_appdomain_cloud-tea-svc",app="alb-metrics-exporter",backend="172_30_75_6_80",instance="172.30.75.3:9913",job="kubernetes-pods",kubernetes_namespace="kube-system",kubernetes_pod_name="alb-metrics-exporter-7d495d785c-swkls",pod_template_hash="3805183417"} 40
```

{: screen}

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 업스트림 유형 2 메트릭 형식 이해</th>
</thead>
<tbody>
<tr>
<td><code>&lt;ALB-ID&gt;</code></td>
<td>ALB ID입니다. 위의 예제에서 ALB ID는 <code>public\_cra6a6eb9e897e41c4a5e58f957b417aec\_alb1</code>입니다.</td>
</tr>
<td><code>&lt;UPSTREAM-NAME&gt;</code></td>
<td>Ingress 리소스에 정의된 업스트림 서비스의 이름입니다. Prometheus와의 호환성을 유지보수하기 위해 마침표(<code>.</code>)는 밑줄(<code>\_</code>)로 대체됩니다. 위의 예제에서 업스트림 이름은 <code>demostg1\_stg\_us\_south\_containers\_appdomain\_cloud\_tea\_svc</code>입니다.</td>
</tr>
<tr>
<td><code>&lt;POD\_IP&gt;</code></td>
<td>특정 업스트림 서비스 팟(Pod)의 IP 주소 및 포트입니다. Prometheus와의 호환성을 유지보수하기 위해 마침표(<code>.</code>)와 콜론(<code>:</code>)은 밑줄<code>(_)</code>로 대체됩니다. 위의 예제에서 업스트림 팟(Pod) IP는 <code>172_30_75_6_80</code>입니다.</td>
</tr>
<tr>
<td><code>&lt;METRIC_NAME&gt;</code></td>
<td>수집된 메트릭 유형의 이름입니다. 메트릭 이름 목록은 다음 표 "지원되는 업스트림 유형 2 메트릭"을 참조하십시오. 위의 예제에서 메트릭 이름은 <code>requestMsec</code>입니다.</td>
</tr>
<tr>
<td><code>&lt;VALUE&gt;</code></td>
<td>수집된 메트릭의 값입니다. 위의 예제에서 값은 <code>40</code>입니다.</td>
</tr>
</tbody></table>

다음 표에는 지원되는 업스트림 유형 2 메트릭 이름이 나열되어 있습니다.

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 지원되는 업스트림 유형 2 메트릭</th>
</thead>
<tbody>
<tr>
<td><code>requestMsec</code></td>
<td>요청 처리 시간(업스트림 포함)의 평균(밀리초)입니다.</td>
</tr>
<tr>
<td><code>responseMsec</code></td>
<td>업스트림 응답 처리 시간의 평균(밀리초)입니다.</td>
  </tr></tbody>
</table>

<br />


## Ingress 메트릭 콜렉션에 대한 공유 메모리 구역 크기 늘리기
{: #vts_zone_size}

작업자 프로세스가 캐시, 세션 지속성 및 속도 한계 등의 정보를 공유할 수 있도록 공유 메모리 구역이 정의됩니다. 가상 호스트 트래픽 상태 구역이라고 하는 공유 메모리 구역은 Ingress가 ALB에 대한 메트릭 데이터를 수집할 수 있도록 설정되어 있습니다.
{:shortdesc}

`ibm-cloud-provider-ingress-cm` Ingress configmap에서 `vts-status-zone-size` 필드는 메트릭 데이터 콜렉션에 대한 공유 메모리 구역의 크기를 설정합니다. 기본적으로 `vts-status-zone-size`는 `10m`로 설정되어 있습니다. 메트릭 콜렉션을 위해 추가 메모리가 필요한 대형 환경을 보유한 경우에는 다음 단계에 따라 보다 큰 값을 대신 사용하도록 기본값을 대체할 수 있습니다.

시작하기 전에 `kube-system` 네임스페이스에 대해 [**작성자** 또는 **관리자** {{site.data.keyword.Bluemix_notm}} IAM 서비스 역할](/docs/containers?topic=containers-users#platform)이 있는지 확인하십시오.

1. `ibm-cloud-provider-ingress-cm` configmap 리소스에 대한 구성 파일을 편집하십시오.

    ```
    kubectl edit cm ibm-cloud-provider-ingress-cm -n kube-system
    ```
    {: pre}

2. `vts-status-zone-size`의 값을 `10m`에서 보다 큰 값으로 변경하십시오.

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

3. 구성 파일을 저장하십시오.

4. configmap 변경사항이 적용되었는지 확인하십시오.

   ```
   kubectl get cm ibm-cloud-provider-ingress-cm -n kube-system -o yaml
   ```
   {: pre}
