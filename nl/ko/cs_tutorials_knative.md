---

copyright:
  years: 2014, 2019
lastupdated: "2019-04-11"

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


# 튜토리얼: 관리 Knative를 사용하여 Kubernetes 클러스터의 서버리스 앱 실행
{: #knative_tutorial}

이 튜토리얼을 이용하면 {{site.data.keyword.containerlong_notm}}에서 Kubernetes 클러스터에 Knative를 설치하는 방법을 학습할 수 있습니다.
{: shortdesc}

**Knative는 무엇이며 이를 사용해야 하는 이유는 무엇입니까?**</br>
[Knative](https://github.com/knative/docs)는 Kubernetes 클러스터 맨 위에 최신의, 소스 중심으로 컨테이너화된 서버리스 앱을 작성할 수 있도록 Kubernetes의 기능을 확장한다는 목표로 IBM, Google, Pivotal, Red Hat, Cisco 등에서 개발한 오픈 소스 플랫폼입니다. 플랫폼은 클라우드에서 실행하려는 앱의 유형(예: 12 팩터 앱, 컨테이너 또는 기능)을 결정해야 하는 오늘날의 개발자의 요구사항을 해결하기 위해 디자인되었습니다 (12개의 요인 애플리케이션, 컨테이너 또는 기능). 각 유형의 앱에는 다음의 앱에 맞게 사용자 조정된 오픈 소스 또는 전용 솔루션이 필요합니다. 12 팩터 앱용 Cloud Foundry, 컨테이너용 Kubernetes, OpenWhisk 및 기능을 위한 기타 등. 과거에는 개발자가 따라야 할 접근 방법을 결정해야 했으므로 다양한 유형의 앱을 결합해야 하는 경우 경직되고 복잡했습니다.  

Knative는 프로그래밍 언어와 프레임워크 전반에서 일관된 접근 방식을 사용하여 개발자가 가장 중요한 것(소스 코드)에 집중할 수 있도록 Kubernetes에 워크로드를 빌드, 배치 및 관리하는 데 따르는 운영 부담을 추상화합니다. Cloud Foundry, Kaniko, Dockerfile, Bazel 등 이미 익숙한 입증된 빌드 팩을 사용할 수 있습니다. Knative는 Istio와 통합하여 서버리스 및 컨테이너화된 워크로드가 인터넷에 쉽게 노출되고 모니터링되며 제어되고 전송 중에 데이터가 암호화되도록 지원합니다.

**Knative는 어떻게 작동합니까?**</br>
Knative는 Kuberbetes 클러스터에서 서버리스 앱을 빌드, 배치 및 관리하는 데 도움이 되는 3개의 주요 컴포넌트 또는 _기본요소_로 제공됩니다.

- **Build:** `Build` 기본요소는 소스 코드에서 컨테이너 이미지로 앱을 빌드하는 일련의 단계 작성을 지원합니다. 앱 코드와 이미지를 호스팅할 컨테이너 레지스트리를 찾도록 소스 저장소를 지정하는 간단한 빌드 템플리트를 사용하는 것으로 가정해 보십시오. 하나의 명령만으로 컨테이너에서 해당 이미지를 사용할 수 있도록 Knative에서 이 빌드 템플리트를 사용하고 소스 코드를 가져와서 이미지를 작성하고 이미지를 컨테이너 레지스트리에 푸시하도록 지시할 수 있습니다.
- **Serving:** `Serving` 기본요소는 서버리스 앱을 Knative 서비스로 배치하고 이를 자동으로 스케일링하는 데 도움이 됩니다(스케일링 다운하여 제로 인스턴스도 가능). Istio의 트래픽 관리와 지능형 라우팅 기능을 사용하여 개발자가 새 앱 버전을 테스트하고 롤아웃하거나 A-B 테스트를 수행할 수 있도록 하는 특정 버전의 서비스로 라우팅되는 트래픽을 제어할 수 있습니다.
- **Eventing:** `Eventing` 기본요소를 사용하면 다른 서비스에서 구독할 수 있는 트리거 또는 이벤트 스트림을 작성할 수 있습니다. 예를 들어, 코드가 GitHub 마스터 저장소에 푸시될 때마다 앱의 새로운 빌드를 시작하고자 할 수 있습니다. 또는 온도가 빙점 이하로 떨어지는 경우에만 서버리스 앱을 실행하려고 합니다. `Eventing` 기본요소는 CI/CD 파이프라인에 통합되어 특정 이벤트가 발생하는 경우에 앱의 빌드 및 배치를 자동화할 수 있습니다.

**Managed Knative on {{site.data.keyword.containerlong_notm}}(시범) 추가 기능은 무엇입니까?** </br>
Managed Knative on {{site.data.keyword.containerlong_notm}}는 Kubernetes 클러스터와 Knative 및 Istio를 직접 통합하는 관리 추가 기능입니다. 추가 기능의 Knative 및 Istio 버전은 IBM에서 테스트되며 {{site.data.keyword.containerlong_notm}}에서 사용할 수 있도록 지원됩니다. 관리 추가 기능에 대한 자세한 정보는 [관리 추가 기능을 사용하여 서비스 추가](/docs/containers?topic=containers-managed-addons#managed-addons)를 참조하십시오.

**제한사항이 있습니까?** </br>
클러스터에 [컨테이너 이미지 보안 적용기 허가 제어기](/docs/services/Registry?topic=registry-security_enforce#security_enforce)가 설치되어 있으면 클러스터에서 관리 Knative 추가 기능을 사용할 수 없습니다.

그럴듯해 보이십니까? 이 튜토리얼을 따라 {{site.data.keyword.containerlong_notm}}의 Knative를 시작하십시오.

## 목표
{: #knative_objectives}

- Knative및 Knative 기본요소에 기본 사항을 학습합니다.  
- 클러스터에 관리 Knative 및 관리 Istio 추가 기능을 설치합니다.
- 첫 번째 서버리스 앱을 Knative을 사용하여 배치하고 Knative `Serving` 기본요소를 사용하여 인터넷에 앱을 노출합니다.
- Knative 스케일링 및 변경내용을 탐색합니다.

## 소요 시간
{: #knative_time}

30분

## 대상
{: #knative_audience}

이 튜토리얼은 Knative를 사용하여 Kubernetes 클러스터에 서버리스 앱을 배치하는 방법을 배우는 데 관심이 있는 개발자와 클러스터에서 Knative를 설정하는 방법을 배우고자 하는 클러스터 관리자를 대상으로 디자인되었습니다.

## 전제조건
{: #knative_prerequisites}

-  [IBM Cloud CLI, {{site.data.keyword.containerlong_notm}} 플러그인 및 Kubernetes CLI를 설치](/docs/containers?topic=containers-cs_cli_install#cs_cli_install_steps)하십시오. 클러스터의 Kubernetes 버전과 일치하는 `kubectl` CLI 버전을 설치해야 합니다.
-  [코어가 4개이고 메모리가 16GB(`b3c.4x16`) 이상인 작업자 노드가 3개 이상 있는 클러스터를 작성하십시오](/docs/containers?topic=containers-clusters#clusters_cli). 모든 작업자 노드는 Kubernetes 버전 1.12 이상을 실행해야 합니다.
-  {{site.data.keyword.containerlong_notm}}에 대해 [**작성자** 또는 **관리자** {{site.data.keyword.Bluemix_notm}} IAM 서비스 역할](/docs/containers?topic=containers-users#platform)이 있는지 확인하십시오.
-  [CLI를 클러스터에 대상으로 지정](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)하십시오.

## 학습 1: 관리 Knative 추가 기능 설정
{: #knative_setup}

Knative는 Istio 맨 위에 빌드되어 서버리스 및 컨테이너화된 워크로드가 클러스터 내부 및 인터넷에 노출될 수 있도록 보장합니다. 또한 Istio를 사용하면 사용자 서비스 간의 네트워크 트래픽을 모니터링하고 제어할 수 있으며 전송 중에 데이터가 암호화되도록 할 수 있습니다. 관리 Knative 추가 기능을 설치하면 관리 Istio 추가 기능도 자동으로 설치됩니다.
{: shortdesc}

1. 클러스터에서 관리 Knative 추가 기능을 사용으로 설정하십시오. 클러스터에서 Knative를 사용으로 설정하면 Istio 및 모든 Knative 컴포넌트가 클러스터에 설치됩니다.
   ```
   ibmcloud ks cluster-addon-enable knative --cluster <cluster_name_or_ID> -y
   ```
   {: pre}

   출력 예:
   ```
   Enabling add-on knative for cluster knative...
   OK
   ```
   {: screen}

   모든 Knative 컴포넌트 설치를 완료하는 데 몇 분 정도 소요될 수 있습니다.

2. Istio가 설치되었는지 확인하십시오. 9개의 Istio 서비스용 팟(Pod)과 Prometheus용 팟(Pod)은 모두 `Running` 상태여야 합니다.
   ```
   kubectl get pods --namespace istio-system
   ```
   {: pre}

   출력 예:
   ```
    NAME                                       READY     STATUS      RESTARTS   AGE
    istio-citadel-748d656b-pj9bw               1/1       Running     0          2m
    istio-egressgateway-6c65d7c98d-l54kg       1/1       Running     0          2m
    istio-galley-65cfbc6fd7-bpnqx              1/1       Running     0          2m
    istio-ingressgateway-f8dd85989-6w6nj       1/1       Running     0          2m
    istio-pilot-5fd885964b-l4df6               2/2       Running     0          2m
    istio-policy-56f4f4cbbd-2z2bk              2/2       Running     0          2m
    istio-sidecar-injector-646655c8cd-rwvsx    1/1       Running     0          2m
    istio-statsd-prom-bridge-7fdbbf769-8k42l   1/1       Running     0          2m
    istio-telemetry-8687d9d745-mwjbf           2/2       Running     0          2m
    prometheus-55c7c698d6-f4drj                1/1       Running     0          2m
   ```
   {: screen}

3. 선택사항: `default` 네임스페이스어 모든 앱에 적합한 Istio를 사용할 경우 `istio-injection=enabled` 레이블을 네임스페이스에 추가하십시오. 앱이 Istio 서비스 메시에 포함될 수 있도록 각 서버리스 앱 팟(Pod)이 Envoy 프록시 사이드카로 실행되어야 합니다. 이 레이블은 앱 팟(Pod)이 Envoy 프록시 사이드카 컨테이너로 작성되도록 Istio에서 팟(Pod) 템플리트 스펙을 자동으로 수정하도록 허용합니다. 
  ```
    kubectl label namespace default istio-injection=enabled
  ```
  {: pre}

4. 모든 Knative 컴포넌트가 설치되었는지 확인하십시오.
   1. Knative `Serving` 컴포넌트의 모든 팟(Pod)이 `Running` 상태인지 확인하십시오.  
      ```
      kubectl get pods --namespace knative-serving
      ```
      {: pre}

      출력 예:
      ```
      NAME                          READY     STATUS    RESTARTS   AGE
      activator-598b4b7787-ps7mj    2/2       Running   0          21m
      autoscaler-5cf5cfb4dc-mcc2l   2/2       Running   0          21m
      controller-7fc84c6584-qlzk2   1/1       Running   0          21m
      webhook-7797ffb6bf-wg46v      1/1       Running   0          21m
      ```
      {: screen}

   2. Knative `Build` 컴포넌트의 모든 팟(Pod)이 `Running` 상태인지 확인하십시오.  
      ```
      kubectl get pods --namespace knative-build
      ```
      {: pre}

      출력 예:
      ```
      NAME                                READY     STATUS    RESTARTS   AGE
      build-controller-79cb969d89-kdn2b   1/1       Running   0          21m
      build-webhook-58d685fc58-twwc4      1/1       Running   0          21m
      ```
      {: screen}

   3. Knative `Eventing` 컴포넌트의 모든 팟(Pod)이 `Running` 상태인지 확인하십시오.
      ```
      kubectl get pods --namespace knative-eventing
      ```
      {: pre}

      출력 예:

      ```
      NAME                                            READY     STATUS    RESTARTS   AGE
      eventing-controller-847d8cf969-kxjtm            1/1       Running   0          22m
      in-memory-channel-controller-59dd7cfb5b-846mn   1/1       Running   0          22m
      in-memory-channel-dispatcher-67f7497fc-dgbrb    2/2       Running   1          22m
      webhook-7cfff8d86d-vjnqq                        1/1       Running   0          22m
      ```
      {: screen}

   4. Knative `Sources` 컴포넌트의 모든 팟(Pod)이 `Running` 상태인지 확인하십시오.
      ```
      kubectl get pods --namespace knative-sources
      ```
      {: pre}

      출력 예:
      ```
      NAME                   READY     STATUS    RESTARTS   AGE
      controller-manager-0   1/1       Running   0          22m
      ```
      {: screen}

   5. Knative `Monitoring` 컴포넌트의 모든 팟(Pod)이 `Running` 상태인지 확인하십시오.
      ```
      kubectl get pods --namespace knative-monitoring
      ```
      {: pre}

      출력 예:
      ```
      NAME                                  READY     STATUS                 RESTARTS   AGE
      elasticsearch-logging-0               1/1       Running                0          22m
      elasticsearch-logging-1               1/1       Running                0          21m
      grafana-79cf95cc7-mw42v               1/1       Running                0          21m
      kibana-logging-7f7b9698bc-m7v6r       1/1       Running                0          22m
      kube-state-metrics-768dfff9c5-fmkkr   4/4       Running                0          21m
      node-exporter-dzlp9                   2/2       Running                0          22m
      node-exporter-hp6gv                   2/2       Running                0          22m
      node-exporter-hr6vs                   2/2       Running                0          22m
      prometheus-system-0                   1/1       Running                0          21m
      prometheus-system-1                   1/1       Running                0          21m
      ```
      {: screen}

좋습니다! Knative와 Istio를 모두 설정하면 이제 첫 번째 서버리스 앱을 클러스터에 배치할 수 있습니다.

## 학습 2: 클러스터에 서버리스 앱 배치
{: #deploy_app}

이 학습에서는 첫 번째 서버리스 [`Hello World`](https://hub.docker.com/r/ibmcom/kn-helloworld) 앱을 Go에 배치합니다. 샘플 앱에 요청을 보내면 해당 앱은 환경 변수 `TARGET`을 읽고 `"Hello ${TARGET}!"`을 인쇄합니다. 환경 변수가 비어 있으면 `"Hello World!"`가 리턴됩니다.
{: shortdesc}

1. 첫 번째 서버리스 `Hello World` 앱을 위한 YAML 파일을 Knative에 작성하십시오. Knativ로 앱을 배치하려면 Knative 서비스 리소스를 지정해야 합니다. 서비스는 Knative `Serving` 기본요소에 의해 관리되며 워크로드의 전체 라이프사이클을 관리할 책임이 있습니다. 서비스는 각 배치에 Knative 변경내용, 라우트 및 구성이 있는지 확인합니다. 서비스를 업데이트하면 새 버전의 앱이 작성되고 서비스의 개정 히스토리에 추가됩니다. Knative 라우트는 각 개정의 앱이 네트워크 엔드포인트에 맵핑되어 특정 개정으로 라우팅되는 네트워크 트래픽 양을 제어할 수 있도록 합니다. Knative 구성은 특정 개정의 설정을 유지하므로 언제든지 이전 개정으로 롤백하거나 개정 간에 전환할 수 있습니다. Knative `Serving` 리소스에 대한 자세한 정보는 [Knative 문서](https://github.com/knative/docs/tree/master/serving)를 참조하십시오.
   ```
   apiVersion: serving.knative.dev/v1alpha1
   kind: Service
   metadata:
     name: kn-helloworld
     namespace: default
   spec:
     runLatest:
       configuration:
         revisionTemplate:
           spec:
             container:
               image: docker.io/ibmcom/kn-helloworld
               env:
               - name: TARGET
                 value: "Go Sample v1"
    ```
    {: codeblock}

    <table>
    <caption>YAML 파일 컴포넌트 이해</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> YAML 파일 컴포넌트 이해</th>
    </thead>
    <tbody>
    <tr>
    <td><code>metadata.name</code></td>
    <td>Knative 서비스의 이름입니다.</td>
    </tr>
    <tr>
    <td><code>metadata.namespace</td>
    <td>Knative 서비스로 앱을 배치하려는 Kubernetes 네임스페이스입니다. </td>
    </tr>
    <tr>
    <td><code>spec.container.image</code></td>
    <td>이미지가 저장된 컨테이너 레지스트리에 대한 URL입니다. 이 예제에서는 Docker Hub의 <code>ibmcom</code> 네임스페이스에 저장되는 Knative Hello World 앱을 배치합니다. </td>
    </tr>
    <tr>
    <td><code>spec.container.env</code></td>
    <td>Knative 서비스에서 사용할 환경 변수 목록입니다. 이 예제에서, 환경 변수 <code>TARGET</code> 값은 샘플 앱에서 읽고 앱에 <code>"Hello ${TARGET}!"</code> 형식으로 요청을 보내면 리턴됩니다. 값이 제공되지 않으면 샘플 앱은 <code>"Hello World!"</code>를 리턴합니다.  </td>
    </tr>
    </tbody>
    </table>

2. 클러스터에 Knative 서비스를 작성하십시오. 서비스를 작성하면 Knative `Serving` 기본요소는 변경할 수 없는 개정, Knative 라우트, Ingress 라우팅 규칙, Kubernetes 서비스, Kubernetes 팟(Pod) 및 앱에 대한 로드 밸런서를 작성합니다. 앱에는 인터넷에서 앱을 액세스하는 데 사용할 수 있는 `<knative_service_name>.<namespace>.<ingress_subdomain>` 형식으로 Ingress 하위 도메인에서 하위 도메인이 지정됩니다.
   ```
   kubectl apply -f service.yaml
   ```
   {: pre}

   출력 예:
   ```
   service.serving.knative.dev "kn-helloworld" created
   ```
   {: screen}

3. 포드가 작성되었는지 확인하십시오. 팟(Pod)은 두 개의 컨테이너로 구성되어 있습니다. 하나의 컨테이너가 `Hello World` 앱을 실행하며 다른 컨테이너는 Istio 및 Knative 모니터링과 로깅 도구를 실행하는 사이드카입니다. 팟(Pod)에는 `00001` 개정 번호가 지정됩니다.
   ```
   kubectl get pods
   ```
   {: pre}

   출력 예:
   ```
   NAME                                              READY     STATUS    RESTARTS   AGE
   kn-helloworld-00001-deployment-55db6bf4c5-2vftm   2/2       Running   0          16s
   ```
   {: screen}

4. `Hello World` 앱을 사용해 보십시오.
   1. Knative 서비스에 지정된 기본 도메인을 가져오십시오. Knative 서비스의 이름을 변경하거나 앱을 다른 네임스페이스에 배치한 경우, 조회에 해당 값을 업데이트하십시오.
      ```
      kubectl get ksvc/kn-helloworld
      ```
      {: pre}

      출력 예:
      ```
      NAME            DOMAIN                                                                LATESTCREATED         LATESTREADY           READY   REASON
      kn-helloworld   kn-helloworld.default.mycluster.us-south.containers.appdomain.cloud   kn-helloworld-rjmwt   kn-helloworld-rjmwt   True
      ```
      {: screen}

   2. 이전 단계에서 검색한 하위 도메인을 사용하여 앱에 대한 요청을 수행하십시오.
      ```
      curl -v <service_domain>
      ```
      {: pre}

      출력 예:
      ```
      * Rebuilt URL to: kn-helloworld.default.mycluster.us-south.containers.appdomain.cloud/
      *   Trying 169.46.XX.XX...
      * TCP_NODELAY set
      * Connected to kn-helloworld.default.mycluster.us-south.containers.appdomain.cloud (169.46.XX.XX) port 80 (#0)
      > GET / HTTP/1.1
      > Host: kn-helloworld.default.mycluster.us-south.containers.appdomain.cloud
      > User-Agent: curl/7.54.0
      > Accept: */*
      >
      < HTTP/1.1 200 OK
      < Date: Thu, 21 Mar 2019 01:12:48 GMT
      < Content-Type: text/plain; charset=utf-8
      < Content-Length: 20
      < Connection: keep-alive
      < x-envoy-upstream-service-time: 17
      <
      Hello Go Sample v1!
      * Connection #0 to host kn-helloworld.default.mycluster.us-south.containers.appdomain.cloud left intact
      ```
      {: screen}

5. Knative가 팟(Pod)을 스케일링 다운을 수행하도록 몇 분간 기다리십시오. Knative는 수신 워크로드를 처리하는 데 필요한 시간동안 작동되어야 하는 팟(Pod)의 수를 평가합니다. 네트워크 트래픽이 수신되지 않으면 Knative가 자동으로 팟(Pod)을 스케일링 다운합니다. 이 예제에 표시된 바와 같이 제로 팟(Pod)이 될 수도 있습니다.

   Knative가 팟(Pod)을 스케일링 업하는 방법을 확인하시겠습니까? 예를 들어, [단순 클라우드 기반 로드 테스터](https://loader.io/)와 같은 도구를 사용하여 앱의 워크로드를 늘리십시오.
   {: tip}
   ```
   kubectl get pods
   ```
   {: pre}

   `kn-helloworld` 팟(Pod)이 표시되지 않으면 Knative가 앱을 제로 팟(Pod)으로 스케일링 다운한 것입니다.

6. Knative 서비스 샘플을 업데이트하고 `TARGET` 환경 변수에 다른 값을 입력하십시오.

   예제 서비스 YAML:
   ```
   apiVersion: serving.knative.dev/v1alpha1
   kind: Service
   metadata:
     name: kn-helloworld
     namespace: default
   spec:
     runLatest:
       configuration:
         revisionTemplate:
           spec:
             container:
               image: docker.io/ibmcom/kn-helloworld
               env:
               - name: TARGET
                 value: "Mr. Smith"
    ```
    {: codeblock}

7. 서비스에 변경을 적용하십시오. 구성을 변경하면 Knative는 자동으로 새 개정을 작성하고 새 라우트를 지정하며 기본적으로 Istio가 수신 네트워크 트래픽을 최신 개정으로 라우팅하도록 지시합니다.
   ```
   kubectl apply -f service.yaml
   ```
   {: pre}

8. 변경사항이 적용되었는지 확인하려면 앱에 대한 새 요청을 수행하십시오.
   ```
   curl -v <service_domain>
   ```

   출력 예:
   ```
   ...
   Hello Mr. Smith!
   ```
   {: screen}

9. Knative가 증가된 네트워크 트래픽을 고려하여 팟(Pod)을 다시 스케일링 업했는지 확인하십시오. 팟(Pod)에는 `00002` 개정 번호가 지정됩니다. 개정 번호를 사용하여 특정 버전의 앱을 참조할 수 있습니다. 예를 들어, Istio가 두 개정 간의 수신 트래픽을 분할하도록 지시할 수 있습니다.
   ```
   kubectl get pods
   ```

   출력 예:
   ```
   NAME                                              READY     STATUS    RESTARTS   AGE
   kn-helloworld-00002-deployment-55db6bf4c5-2vftm   3/3       Running   0          16s
   ```
   {: screen}

10. 선택사항: Knative 서비스를 정리하십시오.
    ```
    kubectl delete -f service.yaml
    ```
    {: pre}

훌륭합니다! 첫 번째 Knative 애플리케이션을 클러스터에 정상적으로 배치하고 Knative `Serving` 기본요소의 개정 및 스케일링 기능을 살펴보았습니다.


## 다음 단계   
{: #whats-next}

- 이 [Knative 워크샵 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/IBM/knative101/tree/master/workshop)을 사용하여 첫 번째 `Node.js` fibonacci 앱을 클러스터에 배치하십시오.
  - Knative `Build` 기본요소의 사용 방법을 탐색하고 GitHub의 Dockerfile에서 이미지를 빌드하여 해당 이미지를 자동으로 {{site.data.keyword.registrylong_notm}}의 네임스페이스에 푸시하십시오.  
  - IBM 제공 Ingress 하위 도메인으로부터 Knative에서 제공하는 Istio Ingress 게이트웨이로 네트워크 트래픽을 라우팅하도록 설정하는 방법에 대해 알아보십시오.
  - 앱의 새 버전을 롤아웃하고 Istio를 사용하여 각 앱 버전으로 라우팅되는 트래픽의 양을 제어하십시오.
- [Knative `Eventing` ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/knative/docs/tree/master/eventing/samples) 샘플을 탐색하십시오.
- Knative를 자세히 알아보려면 [Knative 문서 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/knative/docs)를 참조하십시오.
