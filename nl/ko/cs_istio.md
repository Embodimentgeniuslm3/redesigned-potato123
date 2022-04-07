---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-11"

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



# 관리 Istio 추가 기능(베타) 사용
{: #istio}

Istio on {{site.data.keyword.containerlong}}는 Istio의 완벽한 설치, Istio 제어 플레인 컴포넌트의 자동 업데이트 및 라이프사이클 관리, 플랫폼 로깅 및 모니터링 도구와의 통합을 제공합니다.
{: shortdesc}

한 번의 클릭으로 Istio 핵심 컴포넌트, 추가 추적, 모니터링 및 시각화를 모두 확보할 수 있고 BookInfo 샘플 앱을 시작하고 실행할 수 있습니다. Istio on {{site.data.keyword.containerlong_notm}}는 관리 추가 기능으로 제공되므로 {{site.data.keyword.Bluemix_notm}}에서 사용자의 모든 Istio 컴포넌트를 최신 상태로 유지합니다. 

## Istio on {{site.data.keyword.containerlong_notm}} 이해
{: #istio_ov}

### Istio의 개념
{: #istio_ov_what_is}

[Istio ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/cloud/info/istio)는 {{site.data.keyword.containerlong_notm}}에서 클라우드 플랫폼(예: Kubernetes)의 마이크로서비스를 연결, 보호, 제어 및 관찰하기 위한 개방형 서비스 메시 플랫폼입니다.
{:shortdesc}

모놀리스(monolith) 애플리케이션을 분산된 마이크로서비스 아키텍처로 전환할 때는 서비스의 트래픽을 제어하고, 서비스의 다크 런치(Dark Launch) 및 Canary 롤아웃을 수행하고, 장애를 처리하고, 서비스 통신을 보호하고, 서비스를 관찰하고, 서비스 전체에서 일관된 액세스 정책을 적용하는 등의 작업을 어떻게 수행해야 하는가와 같은 새로운 어려움이 발생합니다. 이러한 어려움을 해결하기 위해 서비스 메시를 활용할 수 있습니다. 서비스 메시는 마이크로서비스 사이의 연결을 연결, 관찰, 보호 및 제어하기 위한 투명하고 언어 독립적인 네트워크를 제공합니다. Istio는 사용자가 네트워크 트래픽을 관리하고, 마이크로서비스 간에 로드 밸런싱을 수행하고, 액세스 정책을 적용하고, 서비스 ID를 확인하는 등의 작업을 수행할 수 있도록 서비스 메시에 대한 인사이트와 제어 기능을 제공합니다. 

예를 들어, 마이크로서비스 메시에서 Istio를 사용하면 다음과 같은 이점이 있습니다. 
- 클러스터에서 실행 중인 앱에 대한 가시성을 높일 수 있습니다. 
- 앱의 Canary 버전을 배치하고 이 앱에 전송되는 트래픽을 제어할 수 있습니다.
- 마이크로서비스 간에 전송되는 데이터의 자동 암호화를 사용할 수 있습니다.
- 속도 제한 및 속성 기반 화이트리스트와 블랙리스트 정책을 적용할 수 있습니다.

Istio 서비스 메시는 데이터 플레인과 제어 플레인으로 구성됩니다. 데이터 플레인은 마이크로서비스 간의 통신을 중개하는 각 앱 팟(Pod)에 있는 Envoy 프록시 사이드카로 구성됩니다. 제어 플레인은 클러스터에 Istio 구성을 적용하는 파일럿, 믹서 텔레메트리와 정책 및 Citadel로 구성됩니다. 이러한 각 컴포넌트에 대한 자세한 정보는 [`istio` 추가 기능 설명](#istio_components)을 참조하십시오.

### Istio on {{site.data.keyword.containerlong_notm}}(베타)는 무엇입니까?
{: #istio_ov_addon}

Istio on {{site.data.keyword.containerlong_notm}}는 Kubernetes 클러스터와 Istio를 직접 통합하는 관리 추가 기능으로 제공됩니다.
{: shortdesc}

관리 Istio 추가 기능은 베타로 분류되며 불안정하거나 자주 변경될 수 있습니다. 베타 기능은 일반적으로 사용 가능한 기능에서 제공하는 동일한 레벨의 성능 또는 호환성을 제공하지 않을 수 있으며 프로덕션 환경에서 사용할 수 없습니다.
{: note}

**내 클러스터에서 어떻게 보입니까?**</br>
Istio 추가 기능을 설치하면 Istio 제어 및 데이터 플레인은 클러스터가 이미 연결되어 있는 VLAN을 사용합니다. 구성 트래픽은 클러스터 내 사설 네트워크를 통해 이동되며 방화벽에서 추가 포트 또는 IP 주소를 열 필요가 없습니다. Istio 게이트웨이를 사용하여 Istio 관리 앱을 노출하는 경우 앱에 대한 외부 트래픽 요청은 공용 VLAN을 통해 플로우됩니다. 

**업데이트 프로세스는 어떻게 작동합니까?**</br>
관리 추가 기능으로 제공되는 Istio 버전은 {{site.data.keyword.Bluemix_notm}}에 의해 테스트되고 {{site.data.keyword.containerlong_notm}}에서의 사용을 승인받았습니다. Istio 컴포넌트를 {{site.data.keyword.containerlong_notm}}에서 지원되는 최신 Istio 버전으로 업데이트하기 위해 [관리 추가 기능 업데이트](/docs/containers?topic=containers-managed-addons#updating-managed-add-ons)의 단계를 따를 수 있습니다.  

Istio의 최신 버전을 사용하거나 Istio 설치를 사용자 정의해야 하는 경우에는 [{{site.data.keyword.Bluemix_notm}} 튜토리얼로 빠른 시작 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://istio.io/docs/setup/kubernetes/quick-start-ibm/)의 다음 단계를 수행하여 Istio의 오픈 소스 버전을 설치할 수 있습니다.
{: tip}

**제한사항이 있습니까?** </br>
클러스터에 [컨테이너 이미지 보안 적용기 허가 제어기](/docs/services/Registry?topic=registry-security_enforce#security_enforce)를 설치한 경우에는 클러스터에서 관리 Istio 추가 기능을 사용으로 설정할 수 없습니다. 

<br />


## 무엇을 설치할 수 있습니까?
{: #istio_components}

Istio on {{site.data.keyword.containerlong_notm}}는 클러스터에서 세 가지 관리 추가 기능으로 제공됩니다.
{: shortdesc}

<dl>
<dt>Istio(`istio`)</dt>
<dd>Prometheus를 포함하여 Istio의 핵심 컴포넌트를 설치합니다. 다음 제어 플레인 컴포넌트에 대한 자세한 정보는 [Istio 문서 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://istio.io/docs/concepts/what-is-istio/)를 참조하십시오.
  <ul><li>`Envoy`는 메시의 모든 서비스에 대한 인바운드 및 아웃 바운드 트래픽을 프록시합니다. Envoy는 앱 컨테이너와 동일한 팟의 사이드카 컨테이너로 배치됩니다.</li>
  <li>`Mixer`는 텔레메트리 콜렉션과 정책 제어를 제공합니다.<ul>
    <li>텔레메트리 팟(Pod)은 Prometheus 엔드포인트로 사용 가능하며 앱 팟(Pod)의 Envoy 프록시 사이드카와 서비스에서 모든 텔레메트리 데이터를 집계합니다.</li>
    <li>정책 팟(Pod)은 속도 제한 및 화이트리스트와 블랙리스트 정책 적용을 포함하여 액세스 제어를 강제 실행합니다.</li></ul>
  </li>
  <li>`Pilot`은 Envoy 사이드카에 대한 서비스 검색을 제공하고 사이드카에 대한 트래픽 관리 라우팅 규칙을 구성합니다.</li>
  <li>`Citadel`은 ID 및 인증 정보 관리를 사용하여 서비스 간의 인증과 일반 사용자 인증을 제공합니다.</li>
  <li>`Galley`는 다른 Istio 제어 플레인 컴포넌트에 대한 구성 변경을 유효성 검증합니다.</li>
</ul></dd>
<dt>Istio extras(`istio-extras`)</dt>
<dd>선택사항: Istio에 대한 추가 모니터링, 추적 및 시각화를 제공하려면 [Grafana ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://grafana.com/), [Jaeger ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.jaegertracing.io/) 및 [Kiali ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.kiali.io/)를 설치합니다.</dd>
<dt>BookInfo 샘플 앱(`istio-sample-bookinfo`)</dt>
<dd>선택사항: [Istio의 BookInfo 샘플 애플리케이션 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://istio.io/docs/examples/bookinfo/)을 배치합니다. 이 배치에는 기본 데모 설정 및 기본 대상 규칙이 포함되어 있어 Istio의 기능을 즉시 사용해 볼 수 있습니다.</dd>
</dl>

<br>
다음 명령을 실행하여 클러스터에서 사용 가능한 Istio 추가 기능을 항상 확인할 수 있습니다.
```
ibmcloud ks cluster-addons --cluster <cluster_name_or_ID>
```
{: pre}

<br />


## {{site.data.keyword.containerlong_notm}}에 Istio 설치
{: #istio_install}

기존 클러스터에 Istio 관리 추가 추가 기능을 설치하십시오.
{: shortdesc}

**시작하기 전에**</br>
* {{site.data.keyword.containerlong_notm}}에 대해 [**작성자** 또는 **관리자** {{site.data.keyword.Bluemix_notm}} IAM 서비스 역할](/docs/containers?topic=containers-users#platform)을 보유하고 있는지 확인하십시오. 
* [코어가 4개이고 메모리가 16GB 이상인 작업자 노드가 3개 이상 있는 표준 클러스터(`b3c.4x16`)를 작성하거나 그러한 기존 표준 클러스터를 사용](/docs/containers?topic=containers-clusters#clusters_ui)하십시오. 또한 해당 클러스터 및 작업자 노드는 지원되는 최소 Kubernetes 버전 이상을 실행해야 하며, 이는 `ibmcloud ks addon-versions --addon istio`를 실행하여 검토할 수 있습니다. 
* [CLI를 클러스터에 대상으로 지정](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)하십시오.
* 기존 클러스터를 사용하며 이전에 IBM Helm 차트를 사용하거나 다른 방법을 통해 클러스터에 Istio를 설치한 경우에는 [해당 Istio 설치를 제거](#istio_uninstall_other)하십시오. 

### CLI에 관리 Istio 추가 기능 설치
{: #istio_install_cli}

1. `istio` 추가 기능을 사용으로 설정하십시오.
  ```
  ibmcloud ks cluster-addon-enable istio --cluster <cluster_name_or_ID>
  ```
  {: pre}

2. 선택사항: `istio-extras` 추가 기능을 사용으로 설정하십시오.
  ```
  ibmcloud ks cluster-addon-enable istio-extras --cluster <cluster_name_or_ID>
  ```
  {: pre}

3. 선택사항: `istio-sample-bookinfo` 추가 기능을 사용으로 설정하십시오.
  ```
  ibmcloud ks cluster-addon-enable istio-sample-bookinfo --cluster <cluster_name_or_ID>
  ```
  {: pre}

4. 설치한 Istio 관리 대상 추가 기능이 이 클러스터에서 사용 가능한지 확인하십시오.
  ```
  ibmcloud ks cluster-addons --cluster <cluster_name_or_ID>
  ```
  {: pre}

  출력 예:
  ```
  Name                      Version
  istio                     1.1.5
  istio-extras              1.1.5
  istio-sample-bookinfo     1.1.5
  ```
  {: screen}

5. 클러스터에 있는 각 추가 기능의 개별 컴포넌트를 확인할 수도 있습니다.
  - `istio` 및 `istio-extras` 컴포넌트: Istio 서비스와 해당 팟(Pod)이 배치되어 있는지 확인하십시오.
    ```
   kubectl get svc -n istio-system
    ```
    {: pre}

    ```
    NAME                     TYPE           CLUSTER-IP       EXTERNAL-IP                                                                    AGE
    grafana                  ClusterIP      172.21.98.154    <none>          3000/TCP                                                       2m
    istio-citadel            ClusterIP      172.21.221.65    <none>          8060/TCP,9093/TCP                                              2m
    istio-egressgateway      ClusterIP      172.21.46.253    <none>          80/TCP,443/TCP                                                 2m
    istio-galley             ClusterIP      172.21.125.77    <none>          443/TCP,9093/TCP                                               2m
    istio-ingressgateway     LoadBalancer   172.21.230.230   169.46.56.125   80:31380/TCP,443:31390/TCP,31400:31400/TCP,5011:31323/TCP,
                                                                              8060:32483/TCP,853:32628/TCP,15030:31601/TCP,15031:31915/TCP   2m
    istio-pilot              ClusterIP      172.21.171.29    <none>          15010/TCP,15011/TCP,8080/TCP,9093/TCP                          2m
    istio-policy             ClusterIP      172.21.140.180   <none>          9091/TCP,15004/TCP,9093/TCP                                    2m
    istio-sidecar-injector   ClusterIP      172.21.248.36    <none>          443/TCP                                                        2m
    istio-telemetry          ClusterIP      172.21.204.173   <none>          9091/TCP,15004/TCP,9093/TCP,42422/TCP                          2m
    jaeger-agent             ClusterIP      None             <none>          5775/UDP,6831/UDP,6832/UDP                                     2m
    jaeger-collector         ClusterIP      172.21.65.195    <none>          14267/TCP,14268/TCP                                            2m
    jaeger-query             ClusterIP      172.21.171.199   <none>          16686/TCP                                                      2m
    kiali                    ClusterIP      172.21.13.35     <none>          20001/TCP                                                      2m
    prometheus               ClusterIP      172.21.105.229   <none>          9090/TCP                                                       2m
    tracing                  ClusterIP      172.21.125.177   <none>          80/TCP                                                         2m
    zipkin                   ClusterIP      172.21.1.77      <none>          9411/TCP                                                       2m
    ```
    {: screen}

    ```
   kubectl get pods -n istio-system
    ```
    {: pre}

    ```
    NAME                                      READY   STATUS    RESTARTS   AGE
    grafana-76dcdfc987-94ldq                  1/1     Running   0          2m
    istio-citadel-869c7f9498-wtldz            1/1     Running   0          2m
    istio-egressgateway-69bb5d4585-qxxbp      1/1     Running   0          2m
    istio-galley-75d7b5bdb9-c9d9n             1/1     Running   0          2m
    istio-ingressgateway-5c8764db74-gh8xg     1/1     Running   0          2m
    istio-pilot-55fd7d886f-vv6fb              2/2     Running   0          2m
    istio-policy-6bb6f6ddb9-s4c8t             2/2     Running   0          2m
    istio-sidecar-injector-7d9845dbb7-r8nq5   1/1     Running   0          2m
    istio-telemetry-7695b4c4d4-tlvn8          2/2     Running   0          2m
    istio-tracing-55bbf55878-z4rd2            1/1     Running   0          2m
    kiali-77566cc66c-kh6lm                    1/1     Running   0          2m
    prometheus-5d5cb44877-lwrqx               1/1     Running   0          2m
    ```
    {: screen}

  - `istio-sample-bookinfo`의 컴포넌트: BookInfo 마이크로서비스와 해당 팟(Pod)이 배치되었는지 확인하십시오.
    ```
    kubectl get svc -n default
    ```
    {: pre}

    ```
    NAME                      TYPE           CLUSTER-IP       EXTERNAL-IP    PORT(S)          AGE
    details                   ClusterIP      172.21.19.104    <none>         9080/TCP         2m
    productpage               ClusterIP      172.21.168.196   <none>         9080/TCP         2m
    ratings                   ClusterIP      172.21.11.131    <none>         9080/TCP         2m
    reviews                   ClusterIP      172.21.117.164   <none>         9080/TCP         2m
    ```
    {: screen}

    ```
    kubectl get pods -n default
    ```
    {: pre}

    ```
    NAME                                     READY     STATUS      RESTARTS   AGE
    details-v1-6865b9b99d-7v9h8              2/2       Running     0          2m
    productpage-v1-f8c8fb8-tbsz9             2/2       Running     0          2m
    ratings-v1-77f657f55d-png6j              2/2       Running     0          2m
    reviews-v1-6b7f6db5c5-fdmbq              2/2       Running     0          2m
    reviews-v2-7ff5966b99-zflkv              2/2       Running     0          2m
    reviews-v3-5df889bcff-nlmjp              2/2       Running     0          2m
    ```
    {: screen}

### UI에 관리 Istio 추가 기능 설치
{: #istio_install_ui}

1. [클러스터 대시보드 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/kubernetes/clusters)에서 클러스터의 이름을 클릭하십시오.

2. **추가 기능** 탭을 클릭하십시오.

3. Istio 카드에서 **설치**를 클릭하십시오.

4. **Istio** 선택란이 이미 선택되어 있습니다. 또한 Istio extras 및 BookInfo 샘플 앱을 설치하려면 **Istio Extras** 및 **Istio 샘플** 선택란을 선택하십시오.

5. **설치**를 클릭하십시오.

6. Istio 카드에 사용 가능한 추가 기능이 나열되어 있는지 확인하십시오.

그런 다음 [BookInfo 샘플 앱](#istio_bookinfo)을 확인하여 Istio의 기능을 사용해 볼 수 있습니다.

<br />


## BookInfo 샘플 앱 시범 사용
{: #istio_bookinfo}

BookInfo 추가 기능(`istio-sample-bookinfo`)은 [Istio의 BookInfo 샘플 애플리케이션 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://istio.io/docs/examples/bookinfo/)을 `default` 네임스페이스에 배치합니다. 이 배치에는 기본 데모 설정 및 기본 대상 규칙이 포함되어 있어 Istio의 기능을 즉시 사용해 볼 수 있습니다.
{: shortdesc}

네 개의 BookInfo 마이크로서비스에는 다음이 포함됩니다.
* `productpage`는 `details` 및 `reviews` 마이크로서비스를 호출하여 페이지를 채웁니다.
* `details`에는 Book 정보가 포함되어 있습니다.
* `reviews`에는 Book 검토가 포함되어 있으며 `ratings` 마이크로서비스를 호출합니다.
* `ratings`에는 Book 검토를 수반하는 Book 등급 정보가 포함되어 있습니다.

`reviews` 마이크로서비스에는 여러 가지 버전이 있습니다.
* `v1`은 `ratings` 마이크로서비스를 호출하지 않습니다.
* `v2`는 `ratings` 마이크로서비스를 호출하고 등급을 1-5개의 검은색 별로 표시합니다.
* `v3`은 `ratings` 마이크로서비스를 호출하고 등급을 1-5개의 빨간색 별로 표시합니다.

이러한 각 마이크로서비스에 대한 배치 YAML은 Envoy 사이드카 프록시가 배치되기 전에 마이크로서비스의 팟(Pod)에 컨테이너로 사전에 삽입되도록 수정되었습니다. 수동 사이드카 인젝션에 대한 자세한 정보는 [Istio 문서 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://istio.io/docs/setup/kubernetes/sidecar-injection/)를 참조하십시오. BookInfo 앱은 Istio 게이트웨이의 공용 IP Ingress 주소에도 이미 노출되어 있습니다. BookInfo 앱이 시작하는 데 도움을 줄 수는 있지만, 이 앱은 프로덕션용이 아닙니다. 

시작하기 전에 클러스터에 [`istio`, `istio-extras` 및 `istio-sample-bookinfo` 관리 추가 기능을 설치](#istio_install)하십시오.

1. 클러스터에 대한 공용 주소를 가져오십시오.
  1. ingress 호스트를 설정하십시오.
    ```
            export INGRESS_HOST=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.status.loadBalancer.ingress[0].ip}')
    ```
    {: pre}

  2. ingress 포트를 설정하십시오.
    ```
            export INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="http2")].port}')
    ```
    {: pre}

  3. ingress 호스트 및 포트를 사용하는 `GATEWAY_URL` 환경 변수를 작성하십시오.
     ```
           export GATEWAY_URL=$INGRESS_HOST:$INGRESS_PORT
     ```
     {: pre}

2. `GATEWAY_URL` 변수의 curl을 수행하여 BookInfo 앱이 실행 중인지 확인하십시오. `200` 응답은 BookInfo 앱이 Istio에서 적절하게 실행 중임을 의미합니다.
   ```
     curl -o /dev/null -s -w "%{http_code}\n" http://${GATEWAY_URL}/productpage
   ```
   {: pre}

3.  브라우저에서 BookInfo 웹 페이지를 보십시오.

    Mac OS 또는 Linux:
    ```
    open http://$GATEWAY_URL/productpage
    ```
    {: pre}

    Windows:
    ```
    start http://$GATEWAY_URL/productpage
    ```
    {: pre}

4. 페이지를 여러 번 새로 고쳐보십시오. 빨간색 별이 있거나, 검은색 별이 있거나, 별이 없는 등 다양한 버전의 검토 섹션이 표시됩니다. 

### 작동 방법 이해
{: #istio_bookinfo_understanding}

BookInfo 샘플은 Istio의 트래픽 관리 컴포넌트가 함께 작동하여 유입 트래픽을 앱에 라우팅하는 방법을 보여줍니다.
{: shortdesc}

<dl>
<dt>`Gateway`</dt>
<dd>`bookinfo-gateway` [게이트웨이 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://istio.io/docs/reference/config/networking/v1alpha3/gateway/)는 BookInfo에 대한 HTTP/TCP 트래픽의 Ingress 시작점 역할을 하는 로드 밸런서(`istio-system` 네임스페이스의 `istio-ingressgateway` 서비스)에 대해 설명합니다. Istio는 게이트웨이 구성 파일에 정의된 포트에서 Istio 관리 앱에 대한 수신 요청을 청취하도록 로드 밸런서를 구성합니다.
</br></br>BookInfo 게이트웨이에 대한 구성 파일을 보려면 다음 명령을 실행하십시오.
<pre class="pre"><code>kubectl get gateway bookinfo-gateway -o yaml</code></pre></dd>

<dt>`VirtualService`</dt>
<dd>`bookinfo` [`VirtualService` ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://istio.io/docs/reference/config/networking/v1alpha3/virtual-service/)는 마이크로서비스를 `destinations`으로 정의하여 서비스 메시 내에 요청이 라우팅되는 방법을 제어하는 규칙을 정의합니다. `bookinfo` 가상 서비스인 요청의 `/productpage` URI는 포트 `9080`에서 `productpage` 호스트로 라우팅됩니다. 이 방식으로 BookInfo 앱에 대한 모든 요청은 먼저 `productpage` 마이크로서비스로 라우팅된 후 BookInfo의 다른 마이크로서비스를 호출합니다.
</br></br>BookInfo에 적용되는 가상 서비스 규칙을 보려면 다음 명령을 실행하십시오.
<pre class="pre"><code>kubectl get virtualservice bookinfo -o yaml</code></pre></dd>

<dt>`DestinationRule`</dt>
<dd>가상 서비스 규칙에 따라 게이트웨이에서 요청을 라우팅하면 `details`, `productpage`, `ratings` 및 `reviews` [`DestinationRules` ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://istio.io/docs/reference/config/networking/v1alpha3/destination-rule/)에서 요청이 마이크로서비스에 도달할 때 요청에 적용되는 정책을 정의합니다. 예를 들어, BookInfo 제품 페이지를 새로 고치면 표시되는 변경사항은 `reviews` 마이크로서비스의 다른 버전, `v1`, `v2` 및 `v3`을 무작위로 호출하는 `productpage` 마이크로서비스의 결과입니다. `reviews` 대상 규칙은 마이크로서비스의 `subsets` 또는 이름 지정된 버전에 동일한 가중치를 부여하므로 버전이 무작위로 선택됩니다. 이 서브세트는 트래픽이 서비스의 특정 버전으로 라우팅될 때 가상 서비스 규칙에서 사용합니다.
</br></br>BookInfo에 적용되는 대상 규칙을 보려면 다음 명령을 실행하십시오.
<pre class="pre"><code>kubectl describe destinationrules</code></pre></dd>
</dl>

</br>

다음으로 [IBM 제공 Ingress 하위 도메인을 사용하여 BookInfo를 노출](#istio_expose_bookinfo)하거나 BookInfo 앱에 대한 서비스 메시를 [로그, 모니터, 추적 및 시각화](#istio_health)할 수 있습니다.

<br />


## Istio 로깅, 모니터링, 추적 및 시각화
{: #istio_health}

Istio on {{site.data.keyword.containerlong_notm}}에서 관리하는 앱을 로그하고, 모니터하고, 추적하고 시각화하려는 경우에는 `istio-extras` 추가 기능에 설치된 Grafana, Jaeger 및 Kiali 대시보드를 시작하거나, LogDNA 및 Sysdig를 서드파티 서비스로서 작업자 노드에 배치할 수 있습니다.
{: shortdesc}

### Grafana, Jaeger 및 Kiali 대시보드 시작
{: #istio_health_extras}

Istio extras 추가 기능(`istio-extras`)은 [Grafana ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://grafana.com/), [Jaeger ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.jaegertracing.io/) 및 [Kiali ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.kiali.io/)를 설치합니다. 이러한 서비스의 각각에 대한 대시보드를 시작하여 Istio에 대한 추가 모니터링, 추적 및 시각화를 제공합니다.
{: shortdesc}

시작하기 전에 클러스터에 [`istio` 및 `istio-extras` 관리 추가 기능을 설치](#istio_install)하십시오.

**Grafana**</br>
1. Grafana 대시보드에 대한 Kubernetes 포트 전달을 시작하십시오.
  ```
  kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=grafana -o jsonpath='{.items[0].metadata.name}') 3000:3000 &
  ```
  {: pre}

2. Istio Grafana 대시보드를 열려면 다음 URL: http://localhost:3000/dashboard/db/istio-mesh-dashboard로 이동하십시오. [BookInfo 추가 기능](#istio_bookinfo)을 설치한 경우 Istio 대시보드에는 사용자가 제품 페이지를 몇 번 새로 고칠 때 생성한 트래픽에 대한 메트릭이 표시됩니다. Istio Grafana 대시보드 사용에 대한 자세한 정보는 Istio 오픈 소스 문서에서 [Istio 대시보드 보기 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://istio.io/docs/tasks/telemetry/using-istio-dashboard/)를 참조하십시오.

**Jaeger**</br>
1. 기본적으로, 100개의 요청마다 1개의 추적 범위를 생성하며, 여기서 샘플링 비율은 1%입니다. 첫 번째 추적이 표시되기 전에 최소 100개의 요청을 전송해야 합니다. 100개의 요청을 [BookInfo 추가 기능](#istio_bookinfo)의 `productpage` 서비스로 전송하려면 다음 명령을 실행하십시오.
  ```
  for i in `seq 1 100`; do curl -s -o /dev/null http://$GATEWAY_URL/productpage; done
  ```
  {: pre}

2. Jaeger 대시보드에 대한 Kubernetes 포트 전달을 시작하십시오.
  ```
  kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=jaeger -o jsonpath='{.items[0].metadata.name}') 16686:16686 &
  ```
  {: pre}

3. Jaeger UI를 열려면 다음 URL: http://localhost:16686로 이동하십시오.

4. BookInfo 추가 기능을 설치한 경우, **서비스** 목록에서 `productpage`를 선택하고 **추적 찾기**를 클릭할 수 있습니다. 제품 페이지를 새로 고칠 때 생성한 트래픽에 대한 추적이 표시됩니다. Istio와 함께 Jaeger를 사용하는 방법에 대한 자세한 정보는 Istio 오픈 소스 문서에서 [BookInfo 샘플을 사용하여 추적 생성 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://istio.io/docs/tasks/telemetry/distributed-tracing/#generating-traces-using-the-bookinfo-sample)을 참조하십시오.

**Kiali**</br>
1. Kialir 대시보드에 대한 Kubernetes 포트 전달을 시작하십시오.
  ```
  kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=kiali -o jsonpath='{.items[0].metadata.name}') 20001:20001 &
  ```
  {: pre}

2. Kiali UI를 열려면 다음 URL: http://localhost:20001/kiali/console로 이동하십시오.

3. 사용자 이름과 비밀번호 문구 모두에 대해 `admin`을 입력하십시오. Kiali를 사용하여 Istio 관리 마이크로서비스를 시각화하는 방법에 대한 자세한 정보는 Istio 오픈 소스 문서에서 [서비스 그래프 생성 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://archive.istio.io/v1.0/docs/tasks/telemetry/kiali/#generating-a-service-graph)을 참조하십시오.

### {{site.data.keyword.la_full_notm}}로 로깅 설정
{: #istio_health_logdna}

로그를 {{site.data.keyword.loganalysislong}}에 전달할 작업자 노드에 LogDNA를 배치하여 각 팟(Pod)에 있는 Envoy 프록시 사이드카 컨테이너 앱 컨테이너와 앱 컨테이너에 대한 로그를 완벽하게 관리합니다.
{: shortdesc}

[{{site.data.keyword.la_full_notm}}](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-about)를 사용하려면 클러스터의 모든 작업자 노드에 로깅 에이전트를 배치합니다. 이 에이전트는 `kube-system`을 포함하여 모든 네임스페이스에서 팟(Pod)의 `/var/log` 디렉토리에 저장되는 `*.log` 확장자와 확장자 없는 파일을 사용하여 로그를 수집합니다. 이러한 로그에는 각 팟(pod)에 있는 앱 컨테이너와 Envoy 프록시 사이드카 컨테이너의 로그가 포함됩니다. 그런 다음 에이전트는 로그를 {{site.data.keyword.la_full_notm}} 서비스에 전달합니다.

시작하려면 [{{site.data.keyword.la_full_notm}}를 사용하여 Kubernetes 클러스터 로그 관리](/docs/services/Log-Analysis-with-LogDNA/tutorials?topic=LogDNA-kube#kube)의 단계를 수행하여 클러스터에 LogDNA를 설치하십시오.




### {{site.data.keyword.mon_full_notm}}로 모니터링 설정
{: #istio_health_sysdig}

메트릭을 {{site.data.keyword.monitoringlong}}에 전달할 수 있도록 Sysdig를 작업자 노드에 배치하여 Istio 관리 앱의 성능 및 상태에 대한 운영상의 가시성을 확보하십시오.
{: shortdesc}

Istio on {{site.data.keyword.containerlong_notm}}를 사용하면 관리 `istio` 추가 기능에서 Prometheus를 클러스터에 설치합니다. Prometheus가 팟(Pod)에 대한 모든 텔레메트리 데이터를 수집할 수 있도록 클러스터의 `istio-mixer-telemetry` 팟(Pod)에 Prometheus 엔드포인트로 어노테이션이 작성됩니다. SysDig 에이전트를 클러스터의 모든 작업자 노드에 배치하면 SysDig는 이러한 Prometheus 엔드포인트에서 데이터를 발견하고 스크랩하여 {{site.data.keyword.Bluemix_notm}} 모니터링 대시보드에 표시하도록 자동으로 활성화됩니다.

Prometheus 작업이 모두 완료되었으므로 Sysdig를 클러스터에 배치하기만 하면 됩니다.

1. [Kubernetes 클러스터에 배치된 앱의 메트릭 분석](/docs/services/Monitoring-with-Sysdig/tutorials?topic=Sysdig-kubernetes_cluster#kubernetes_cluster)의 단계를 수행하여 Sysdig를 설정하십시오.

2. [Sysdig UI를 실행 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](/docs/services/Monitoring-with-Sysdig/tutorials?topic=Sysdig-kubernetes_cluster#kubernetes_cluster_step3)하십시오.

3. **새 대시보드 추가**를 클릭하십시오.

4. `Istio`를 검색하고 Sysdig의 사전 정의된 Istio 대시보드 중 하나를 선택하십시오.

메트릭 및 대시보드를 참조하고, Istio 내부 컴포넌트를 모니터링하고, Istio A/B 배치 및 Canary 배치를 모니터링하는 데 대한 자세한 정보는 [How to monitor Istio, the Kubernetes service mesh ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://sysdig.com/blog/monitor-istio/)를 참조하십시오. "Monitoring Istio: reference metrics and dashboards"라는 섹션을 찾으십시오. 

<br />


## 앱에 대한 사이드카 인젝션 설정
{: #istio_sidecar}

Istio를 사용하여 사용자 고유의 앱을 관리할 준비가 되셨습니까? 앱을 배치하기 전에 먼저 Envoy 프록시 사이드카를 앱 팟(Pod)에 삽입할 방법을 결정해야 합니다.
{: shortdesc}

각 앱 팟(Pod)에는 마이크로서비스가 서비스 메시에 포함될 수 있도록 Envoy 프록시 사이드카가 실행되고 있어야 합니다. 사이드카가 자동 또는 수동으로 각 앱 팟(Pod)에 삽입되도록 할 수 있습니다. 사이드카 인젝션에 대한 자세한 정보는 [Istio 문서 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://istio.io/docs/setup/kubernetes/sidecar-injection/)를 참조하십시오.

### 자동 사이드카 인젝션 사용
{: #istio_sidecar_automatic}

자동 사이드카 인젝션이 사용 가능한 경우, 네임스페이스는 새 배치를 청취하고 앱 팟(Pod)이 Envoy 프록시 사이드카 컨테이너로 작성되도록 팟(Pod) 템플리트 스펙을 자동으로 수정합니다. 해당 네임스페이스에 Istio와 통합할 여러 앱을 배치할 계획이라면 네임스페이스에 대해 자동 사이드카 인젝션을 사용할 수 있습니다. Istio 관리 추가 기능에서는 기본적으로 모든 네임스페이스에 대해 자동 사이드카 삽입이 사용으로 설정되어 있지 않습니다. 

네임스페이스에 대해 자동 사이드카 인젝션을 사용하려면 다음을 수행하십시오.

1. Istio 관리 앱을 배치하려는 네임스페이스의 이름을 가져오십시오.
  ```
         kubectl get namespaces
  ```
  {: pre}

2. 네임스페이스의 레이블을 `istio-injection=enabled`로 지정하십시오.
  ```
  kubectl label namespace <namespace> istio-injection=enabled
  ```
  {: pre}

3. 레이블 지정된 네임스페이스에 앱을 배치하거나 이미 네임스페이스에 있는 앱을 다시 배치하십시오. 
  * 레이블 지정된 네임스페이스에 앱을 배치하려면 다음을 수행하십시오.
    ```
    kubectl apply <myapp>.yaml --namespace <namespace>
    ```
    {: pre}
  * 해당 네임스페이스에 이미 배치된 앱을 다시 배치하려면 삽입된 사이드카를 사용하여 다시 배치되도록 앱 팟(Pod)을 삭제하십시오.
    ```
    kubectl delete pod -l app=<myapp>
    ```
    {: pre}

5. 앱을 노출하는 서비스를 작성하지 않은 경우 Kubernetes 서비스를 작성하십시오. Istio 서비스 메시의 마이크로서비스로 포함되려면 앱이 Kubernetes 서비스에 의해 노출되어야 합니다. [팟(Pod)과 서비스에 대한 Istio 요구사항 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://istio.io/docs/setup/kubernetes/spec-requirements/)을 준수하는지 확인하십시오.

  1. 앱에 대한 서비스를 정의하십시오.
    ```
              apiVersion: v1
          kind: Service
          metadata:
            name: myappservice
          spec:
            selector:
              <selector_key>: <selector_value>
            ports:
       - protocol: TCP
             port: 8080
    ```
    {: codeblock}

    <table>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 서비스 YAML 파일 컴포넌트 이해</th>
    </thead>
    <tbody>
    <tr>
    <td><code>selector</code></td>
    <td>사용자의 앱이 실행되는 팟(Pod)을 대상으로 지정하기 위해 사용할 레이블 키(<em>&lt;selector_key&gt;</em>) 및 값(<em>&lt;selector_value&gt;</em>) 쌍을 입력하십시오.</td>
     </tr>
     <tr>
     <td><code>port</code></td>
     <td>서비스가 청취하는 포트입니다.</td>
     </tr>
     </tbody></table>

  2. 클러스터에 서비스를 작성하십시오. 서비스가 앱과 동일한 네임스페이스에 배치되었는지 확인하십시오.
    ```
    kubectl apply -f myappservice.yaml -n <namespace>
    ```
    {: pre}

앱 컨테이너와 함께 Istio 사이드카 컨테이너가 실행되고 있으므로 이제 앱 팟(Pod)이 Istio 서비스 메시에 통합되었습니다. 

### 수동으로 사이드카 삽입
{: #istio_sidecar_manual}

네임스페이스에서 자동 사이드카 인젝션을 사용하지 않으려면 수동으로 사이드카를 배치 YAML에 삽입할 수 있습니다. 사이드카를 자동으로 삽입하지 않을 다른 배치와 함께 네임스페이스에서 앱이 실행되고 있으면 사이드카를 수동으로 삽입하십시오.

배치를 수동으로 배치에 삽입하십시오.

1. `istioctl` 클라이언트를 다운로드하십시오.
  ```
  curl -L https://git.io/getLatestIstio | ISTIO_VERSION=1.1.5 sh -
  ```

2. Istio 패키지 디렉토리로 이동하십시오.
  ```
  cd istio-1.1.5
  ```
  {: pre}

3. Envoy 사이드카를 앱 배치 YAML에 삽입하십시오. 
  ```
  istioctl kube-inject -f <myapp>.yaml | kubectl apply -f -
  ```
  {: pre}

4. 앱을 배치하십시오.
  ```
  kubectl apply <myapp>.yaml
  ```
  {: pre}

5. 앱을 노출하는 서비스를 작성하지 않은 경우 Kubernetes 서비스를 작성하십시오. Istio 서비스 메시의 마이크로서비스로 포함되려면 앱이 Kubernetes 서비스에 의해 노출되어야 합니다. [팟(Pod)과 서비스에 대한 Istio 요구사항 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://istio.io/docs/setup/kubernetes/spec-requirements/)을 준수하는지 확인하십시오.

  1. 앱에 대한 서비스를 정의하십시오.
    ```
              apiVersion: v1
          kind: Service
          metadata:
            name: myappservice
          spec:
            selector:
              <selector_key>: <selector_value>
            ports:
       - protocol: TCP
             port: 8080
    ```
    {: codeblock}

    <table>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 서비스 YAML 파일 컴포넌트 이해</th>
    </thead>
    <tbody>
    <tr>
    <td><code>selector</code></td>
    <td>사용자의 앱이 실행되는 팟(Pod)을 대상으로 지정하기 위해 사용할 레이블 키(<em>&lt;selector_key&gt;</em>) 및 값(<em>&lt;selector_value&gt;</em>) 쌍을 입력하십시오.</td>
     </tr>
     <tr>
     <td><code>port</code></td>
     <td>서비스가 청취하는 포트입니다.</td>
     </tr>
     </tbody></table>

  2. 클러스터에 서비스를 작성하십시오. 서비스가 앱과 동일한 네임스페이스에 배치되었는지 확인하십시오.
    ```
    kubectl apply -f myappservice.yaml -n <namespace>
    ```
    {: pre}

앱 컨테이너와 함께 Istio 사이드카 컨테이너가 실행되고 있으므로 이제 앱 팟(Pod)이 Istio 서비스 메시에 통합되었습니다. 

<br />


## IBM 제공 호스트 이름을 사용하여 Istio 관리 앱 노출
{: #istio_expose}

[Envoy 프록시 사이드카 인젝션을 설정](#istio_sidecar)하고 앱을 Istio 서비스 메시에 배치하면 IBM 제공 호스트 이름을 사용하여 Istio 관리 앱을 공용 요청에 노출시킬 수 있습니다.
{: shortdesc}

Istio는 [게이트웨이 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://istio.io/docs/reference/config/networking/v1alpha3/gateway/) 및 [가상 서비스 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://istio.io/docs/reference/config/networking/v1alpha3/virtual-service/)를 사용하여 앱으로 라우팅되는 트래픽 방법을 제어합니다. 게이트웨이는 Istio 관리 앱의 시작점 역할을 하는 로드 밸런서 `istio-ingressgateway`를 구성합니다. DNS 항목 및 호스트 이름으로 `istio-ingressgateway` 로드 밸런서의 외부 IP 주소를 등록하여 Istio 관리 앱을 노출할 수 있습니다. 

먼저 [BookInfo를 노출하는 예제](#istio_expose_bookinfo)를 사용해 보거나 [공개적으로 사용자 고유의 앱을 노출](#istio_expose_link)할 수 있습니다.

### 예제: IBM 제공 호스트 이름을 사용하여 BookInfo 노출
{: #istio_expose_bookinfo}

클러스터에 BookInfo 추가 기능을 사용으로 설정하면 Istio 게이트웨이 `bookinfo-gateway`가 작성됩니다. 게이트웨이는 Istio 가상 서비스 및 대상 규칙을 사용하여 BookInfo 앱을 공용으로 노출하는 로드 밸런서 `istio-ingressgateway`를 구성합니다. 다음 단계에서는 BookInfo에 공용으로 액세스할 수 있는 `istio-ingressgateway` 로드 밸런서 IP 주소에 대한 호스트 이름을 작성합니다.
{: shortdesc}

시작하기 전에 클러스터에 [`istio-sample-bookinfo` 관리 추가 기능을 사용으로 설정](#istio_install)하십시오.

1. `istio-ingressgateway` 로드 밸런서의 **EXTERNAL-IP** 주소를 가져오십시오.
  ```
   kubectl get svc -n istio-system
  ```
  {: pre}

  다음 출력 예에서 **EXTERNAL-IP**는 `168.1.1.1`입니다.
  ```
  NAME                     TYPE           CLUSTER-IP       EXTERNAL-IP                                                                    AGE
  ...
  istio-ingressgateway     LoadBalancer   172.21.XXX.XXX   169.1.1.1       80:31380/TCP,443:31390/TCP,31400:31400/TCP,5011:31323/TCP,
                                                                            8060:32483/TCP,853:32628/TCP,15030:31601/TCP,15031:31915/TCP  22m
  ```
  {: screen}

2. DNS 호스트 이름을 작성하여 IP를 등록하십시오.
  ```
  ibmcloud ks nlb-dns-create --cluster <cluster_name_or_id> --ip <LB_IP>
  ```
  {: pre}

3. 호스트 이름이 작성되었는지 확인하십시오.
  ```
  ibmcloud ks nlb-dnss --cluster <cluster_name_or_id>
  ```
  {: pre}

  출력 예:
  ```
  Hostname                                                                                IP(s)              Health Monitor   SSL Cert Status           SSL Cert Secret Name
  mycluster-a1b2cdef345678g9hi012j3kl4567890-0001.us-south.containers.appdomain.cloud     ["168.1.1.1"]      None             created                   <certificate>
  ```
  {: screen}

4. 웹 브라우저에서 BookInfo 제품 페이지를 여십시오.
  ```
  https://<host_name>/productpage
  ```
  {: codeblock}

5. 페이지를 여러 번 새로 고쳐보십시오. `http://<host_name>/productpage`에 대한 요청이 ALB에 의해 수신되고 Istio 게이트웨이 로드 밸런서로 전달됩니다. Istio 게이트웨이가 마이크로서비스의 대상 라우팅 규칙 및 가상 서비스를 관리하므로 `reviews` 마이크로서비스의 다른 버전이 계속 무작위로 리턴됩니다.

게이트웨이, 가상 서비스 규칙 및 BookInfo 앱의 대상 규칙에 대한 자세한 정보는 [작동 방법 이해](#istio_bookinfo_understanding)를 참조하십시오. {{site.data.keyword.containerlong_notm}}에서 DNS 호스트 이름 등록에 대한 자세한 정보는 [NLB 호스트 이름 등록](/docs/containers?topic=containers-loadbalancer#loadbalancer_hostname)을 참조하십시오.

### IBM 제공 호스트 이름을 사용하여 공용으로 Istio 관리 앱 노출
{: #istio_expose_link}

Istio 게이트웨이, Istio 관리 서비스에 대한 트래픽 관리 규칙을 정의하는 가상 서비스 및 `istio-ingressgateway` 로드 밸런서의 외부 IP 주소에 대한 DNS 호스트 이름을 작성하여 Istio 관리 앱을 공용으로 노출합니다.
{: shortdesc}

**시작하기 전에:**
1. 클러스터에 [`istio` 관리 추가 기능을 설치](#istio_install)하십시오.
2. `istioctl` 클라이언트를 설치하십시오.
  1. `istioctl`을 다운로드하십시오.
    ```
   curl -L https://git.io/getLatestIstio | sh -
    ```
  2. Istio 패키지 디렉토리로 이동하십시오.
    ```
    cd istio-1.1.5
    ```
    {: pre}
3. [마이크로서비스에 사이드카 인젝션을 설치하고, 앱 마이크로서비스를 네임스페이스에 배치하고, 앱 마이크로서비스의 Kubernetes 서비스를 작성하여 이 서비스를 Istio 서비스 메시에 포함할 수 있습니다](#istio_sidecar).

</br>
**호스트 이름으로 Istio 관리 앱을 공용으로 노출하려면 다음을 수행하십시오.**

1. 게이트웨이를 작성하십시오. 이 샘플 게이트웨이는 `istio-ingressgateway` 로드 밸런서 서비스를 사용하여 HTTP용 포트 80을 노출합니다. `<namespace>`를 Istio 관리 마이크로서비스가 배치된 네임스페이스로 대체하십시오. 마이크로서비스가 `80`이 아닌 다른 포트에서 청취하는 경우 해당 포트를 추가하십시오. 게이트웨이 YAML 컴포넌트에 대한 자세한 정보는 [Istio 참조 문서 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://istio.io/docs/reference/config/networking/v1alpha3/gateway/)를 참조하십시오.
  ```
  apiVersion: networking.istio.io/v1alpha3
  kind: Gateway
  metadata:
    name: my-gateway
    namespace: <namespace>
  spec:
    selector:
      istio: ingressgateway
    servers:
    - port:
        name: http
        number: 80
        protocol: HTTP
      hosts:
      - '*'
  ```
  {: codeblock}

2. Istio 관리 마이크로서비스가 배치된 네임스페이스에 게이트웨이를 적용하십시오.
  ```
  kubectl apply -f my-gateway.yaml -n <namespace>
  ```
  {: pre}

3. `my-gateway` 게이트웨이를 사용하는 가상 서비스를 작성하고 앱 마이크로서비스에 대한 라우팅 규칙을 정의하십시오. 가상 서비스 YAML 컴포넌트에 대한 자세한 정보는 [Istio 참조 문서 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://istio.io/docs/reference/config/networking/v1alpha3/virtual-service/)를 참조하십시오.
  ```
  apiVersion: networking.istio.io/v1alpha3
  kind: VirtualService
  metadata:
    name: my-virtual-service
    namespace: <namespace>
  spec:
    gateways:
    - my-gateway
    hosts:
    - '*'
    http:
    - match:
      - uri:
          exact: /<service_path>
      route:
      - destination:
          host: <service_name>
          port:
            number: 80
  ```
  {: codeblock}

  <table>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> YAML 파일 컴포넌트 이해</th>
  </thead>
  <tbody>
  <tr>
  <td><code>namespace</code></td>
  <td><em>&lt;namespace&gt;</em>를 Istio 관리 마이크로서비스가 배치된 네임스페이스로 대체하십시오.</td>
  </tr>
  <tr>
  <td><code>gateways</code></td>
  <td>게이트웨이가 이러한 가상 서비스 라우팅 규칙을 <code>istio-ingressgateway</code> 로드 밸런서에 적용할 수 있도록 <code>my-gateway</code>가 지정됩니다. <td>
  </tr>
  <tr>
  <td><code>http.match.uri.exact</code></td>
  <td><em>&lt;service_path&gt;</em>를 시작점 마이크로서비스가 청취하는 경로로 대체하십시오. 예를 들어, BookInfo 앱에서 경로는 <code>/productpage</code>로 정의됩니다.</td>
  </tr>
  <tr>
  <td><code>http.route.destination.host</code></td>
  <td><em>&lt;service_name&gt;</em>을 시작점 마이크로서비스의 이름으로 대체하십시오. 예를 들면, BookInfo 앱에서는 <code>productpage</code>가 다른 앱 마이크로서비스를 호출하는 시작점 마이크로서비스로 제공됩니다.</td>
  </tr>
  <tr>
  <td><code>http.route.destination.port.number</code></td>
  <td>마이크로서비스가 다른 포트에서 청취하는 경우 <em>&lt;80&gt;</em>을 해당 포트로 대체하십시오.</td>
  </tr>
  </tbody></table>

4. Istio 관리 마이크로서비스가 배치된 네임스페이스에 가상 서비스 규칙을 적용하십시오.
  ```
  kubectl apply -f my-virtual-service.yaml -n <namespace>
  ```
  {: pre}

5. `istio-ingressgateway` 로드 밸런서의 **EXTERNAL-IP** 주소를 가져오십시오.
  ```
   kubectl get svc -n istio-system
  ```
  {: pre}

  다음 출력 예에서 **EXTERNAL-IP**는 `168.1.1.1`입니다.
  ```
  NAME                     TYPE           CLUSTER-IP       EXTERNAL-IP                                                                    AGE
  ...
  istio-ingressgateway     LoadBalancer   172.21.XXX.XXX   169.1.1.1       80:31380/TCP,443:31390/TCP,31400:31400/TCP,5011:31323/TCP,
                                                                            8060:32483/TCP,853:32628/TCP,15030:31601/TCP,15031:31915/TCP  22m
  ```
  {: screen}

6. DNS 호스트 이름을 작성하여 `istio-ingressgateway` 로드 밸런서 IP를 등록하십시오.
  ```
  ibmcloud ks nlb-dns-create --cluster <cluster_name_or_id> --ip <LB_IP>
  ```
  {: pre}

7. 호스트 이름이 작성되었는지 확인하십시오.
  ```
  ibmcloud ks nlb-dnss --cluster <cluster_name_or_id>
  ```
  {: pre}

  출력 예:
  ```
  Hostname                                                                                IP(s)              Health Monitor   SSL Cert Status           SSL Cert Secret Name
  mycluster-a1b2cdef345678g9hi012j3kl4567890-0001.us-south.containers.appdomain.cloud     ["168.1.1.1"]      None             created                   <certificate>
  ```
  {: screen}

7. 웹 브라우저에서 액세스할 앱 마이크로서비스의 URL을 입력하여 트래픽이 Istio 관리 마이크로서비스로 라우팅 중인지 확인하십시오.
  ```
  http://<host_name>/<service_path>
  ```
  {: codeblock}

검토 중에 `my-gateway`라고 하는 게이트웨이를 작성했습니다. 이 게이트웨이는 기존 `istio-ingressgateway` 로드 밸런서 서비스를 사용하여 앱을 노출합니다. `istio-ingressgateway` 로드 밸런서는 트래픽을 앱에 라우팅하도록 `my-virtual-service` 가상 서비스에서 정의한 규칙을 사용합니다. 마지막으로 `istio-ingressgateway` 로드 밸런서에 대한 호스트 이름을 작성했습니다. 호스트 이름에 대한 모든 사용자 요청은 Istio 라우팅 규칙에 따라 앱에 전달됩니다. 호스트 이름에 대한 사용자 정의 상태 검사 설정에 대한 정보를 포함하여 {{site.data.keyword.containerlong_notm}}에서 DNS 호스트 이름 등록에 대한 자세한 정보는 [NLB 호스트 이름 등록](/docs/containers?topic=containers-loadbalancer#loadbalancer_hostname)을 참조하십시오.

라우팅을 통한 보다 미세 조정된 제어를 찾고 계십니까? 선택사항: 트래픽을 서로 다른 버전의 마이크로서비스로 전송하는 규칙과 같이 로드 밸런서가 트래픽을 각 마이크로서비스에 라우팅한 후에 적용되는 규칙을 작성하려면 [`DestinationRules` ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://istio.io/docs/reference/config/networking/v1alpha3/destination-rule/)을 작성하여 적용할 수 있습니다.
{: tip}

<br />


## {{site.data.keyword.containerlong_notm}}에 Istio 업데이트
{: #istio_update}

관리 Istio 추가 기능으로 제공되는 Istio 버전은 {{site.data.keyword.Bluemix_notm}}에 의해 테스트되고 {{site.data.keyword.containerlong_notm}}에서의 사용을 승인받았습니다. Istio 컴포넌트를 {{site.data.keyword.containerlong_notm}}에서 지원되는 최신 Istio 버전으로 업데이트하려면 [관리 추가 기능 업데이트](/docs/containers?topic=containers-managed-addons#updating-managed-add-ons)를 참조하십시오.
{: shortdesc}

## {{site.data.keyword.containerlong_notm}}에서 Istio 설치 제거
{: #istio_uninstall}

Istio에 대한 작업을 완료한 경우, Istio 추가 기능을 설치 제거하여 클러스터에서 Istio 리소스를 정리할 수 있습니다.
{:shortdesc}

`istio` 추가 기능은 `istio-extras`, `istio-sample-bookinfo` 및 [`knative`](/docs/containers?topic=containers-serverless-apps-knative) 추가 기능에 대한 종속 항목입니다. `istio-extras` 추가 기능은 `istio-sample-bookinfo` 추가 기능에 대한 종속 항목입니다.
{: important}

**선택사항**: `istio-system` 네임스페이스에서 작성하거나 수정한 모든 리소스와 사용자 정의 리소스 정의(CRD)에서 자동으로 생성된 모든 Kubernetes 리소스가 제거됩니다. 이 리소스를 보관하려면 `istio` 추가 기능을 설치 제거하기 전에 저장하십시오.
1. 네임스페이스에서 작성하거나 수정한 리소스(예: 서비스 또는 앱에 대한 구성 파일)를 저장하십시오.
   명령 예:
   ```
   kubectl get pod <pod_name> -o yaml -n istio-system
   ```
   {: pre}

2. `istio-system`의 CRD에서 자동으로 생성된 Kubernetes 리소스를 로컬 머신의 YAML 파일에 저장하십시오.
   1. `istio-system`에서 CRD를 가져오십시오.
      ```
      kubectl get crd -n istio-system
      ```
      {: pre}

   2. 이 CRD에서 작성한 리소스를 저장하십시오.

### CLI에서 관리 Istio 추가 기능 설치 제거
{: #istio_uninstall_cli}

1. `istio-sample-bookinfo` 추가 기능을 사용 안함으로 설정하십시오.
  ```
  ibmcloud ks cluster-addon-disable istio-sample-bookinfo --cluster <cluster_name_or_ID>
  ```
  {: pre}

2. `istio-extras` 추가 기능을 사용 안함으로 설정하십시오.
  ```
  ibmcloud ks cluster-addon-disable istio-extras --cluster <cluster_name_or_ID>
  ```
  {: pre}

3. `istio` 추가 기능을 사용 안함으로 설정하십시오.
  ```
  ibmcloud ks cluster-addon-disable istio --cluster <cluster_name_or_ID> -f
  ```
  {: pre}

4. 이 클러스터에서 모든 관리 Istio 추가 기능이 사용 안함으로 설정되어 있는지 확인하십시오. 출력에 리턴된 Istio 추가 기능이 없습니다.
  ```
  ibmcloud ks cluster-addons --cluster <cluster_name_or_ID>
  ```
  {: pre}

### UI에서 관리 Istio 추가 기능 설치 제거
{: #istio_uninstall_ui}

1. [클러스터 대시보드 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/kubernetes/clusters)에서 클러스터의 이름을 클릭하십시오.

2. **추가 기능** 탭을 클릭하십시오.

3. Istio 카드에서 메뉴 아이콘을 클릭하십시오.

4. 개별 또는 모든 Istio 추가 기능을 설치 제거하십시오.
  - 개별 Istio 추가 기능:
    1. **관리**를 클릭하십시오.
    2. 사용 안함으로 설정할 추가 기능의 선택란을 선택 취소하십시오. 추가 기능을 지우면 해당 추가 기능이 종속 항목으로 필요한 다른 추가 기능이 자동으로 지워질 수 있습니다.
    3. **관리**를 클릭하십시오. Istio 추가 기능이 사용 안함으로 설정되고 해당 추가 기능에 해당하는 리소스가 이 클러스터에서 제거됩니다.
  - 모든 Istio 추가 기능:
    1. **설치 제거**를 클릭하십시오. 이 클러스터에서 모든 관리 Istio 추가 기능이 사용 안함으로 설정되며 이 클러스터의 모든 Istio 리소스가 제거됩니다.

5. Istio 카드에서 설치 제거한 추가 기능이 더 이상 나열되지 않는지 확인하십시오.

<br />


### 클러스터에서 기타 Istio 설치 설치 제거
{: #istio_uninstall_other}

이전에 IBM Helm 차트를 사용하거나 다른 방법을 통해 클러스터에 Istio를 설치한 경우에는 클러스터에서 관리 Istio 추가 기능을 사용으로 설정하기 전에 해당 Istio 설치를 제거하십시오. Istio가 이미 클러스터에 있는지 확인하려면 `kubectl get namespaces`을 실행하여 해당 출력에서 `istio-system` 네임스페이스를 찾으십시오.
{: shortdesc}

- {{site.data.keyword.Bluemix_notm}} Istio Helm 차트를 사용하여 Istio를 설치한 경우:
  1. Istio Helm 배치를 설치 제거하십시오.
    ```
    helm del istio --purge
    ```
    {: pre}

  2. Helm 2.9 이하를 사용한 경우, 추가 작업 리소스를 삭제하십시오.
    ```
      kubectl -n istio-system delete job --all
    ```
    {: pre}

- Istio를 수동으로 설치했거나 Istio 커뮤니티 Helm 차트를 사용한 경우, [Istio 설치 제거 문서 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://istio.io/docs/setup/kubernetes/quick-start/#uninstall-istio-core-components)을 참조하십시오.
* 이전에 클러스터에 BookInfo를 설치한 경우 해당 리소스를 정리하십시오.
  1. 디렉토리를 Istio 파일 위치로 변경하십시오.
    ```
    cd <filepath>/istio-1.1.5
    ```
    {: pre}

  2. 클러스터에서 모든 BookInfo 서비스, 팟(Pod) 및 배치를 삭제하십시오.
    ```
    samples/bookinfo/platform/kube/cleanup.sh
    ```
    {: pre}

<br />


## 다음에 수행할 작업
{: #istio_next}

* Istio를 추가로 탐색하려면 [Istio 문서 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://istio.io/)에서 더 많은 안내서를 찾을 수 있습니다.
* [코그너티브 클래스: IBM Cloud Kubernetes 서비스 및 Istio로 마이크로서비스 시작하기![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cognitiveclass.ai/courses/get-started-with-microservices-istio-and-ibm-cloud-container-service/)를 수행하십시오. **참고**: 이 과정의 Istio 설치 섹션을 건너뛸 수 있습니다.
* Istio 서비스 메시를 시각화하기 위해 [Vistio ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://itnext.io/vistio-visualize-your-istio-mesh-using-netflixs-vizceral-b075c402e18e)를 사용하는 이 블로그 게시물을 참조하십시오. 
