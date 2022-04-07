---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

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


# Knative를 사용한 서버리스 앱 배치
{: #serverless-apps-knative}

{{site.data.keyword.containerlong_notm}}에서 Kubernetes 클러스터에 Knative를 설치하고 사용하는 방법을 알아보십시오.
{: shortdesc}

**Knative는 무엇이며 이를 사용해야 하는 이유는 무엇입니까?**</br>
[Knative](https://github.com/knative/docs)는 IBM, Google, Pivotal, Red Hat, Cisco 등에서 개발한 오픈 소스 플랫폼입니다. 이 플랫폼의 목표는 Kubernetes 클러스터에 소스 중심으로 컨테이너화된 현대적 서버리스 앱을 작성할 수 있도록 Kubernetes의 기능을 확장하는 것입니다. 플랫폼은 클라우드에서 실행하려는 앱의 유형(12가지 요소 앱, 컨테이너 또는 함수)을 결정해야 하는 오늘날의 개발자 요구사항을 해결하기 위해 디자인되었습니다. 각 유형의 앱에는 이러한 앱에 맞게 조정된 오픈 소스 또는 사유 솔루션(12가지 요소 앱용 Cloud Foundry, 컨테이너용 Kubernetes, 함수용 OpenWhisk 및 기타)이 필요합니다. 과거에는 개발자가 따라야 할 접근 방법을 결정해야 했으므로 다양한 유형의 앱을 결합해야 하는 경우 경직되고 복잡했습니다.  

Knative는 프로그래밍 언어와 프레임워크 전반에서 일관된 접근 방식을 사용하여 개발자가 가장 중요한 것(소스 코드)에 집중할 수 있도록 Kubernetes에서 워크로드를 빌드, 배치 및 관리하는 데 따르는 운영 부담을 제거합니다. Cloud Foundry, Kaniko, Dockerfile, Bazel 등 이미 익숙한 입증된 빌드 팩을 사용할 수 있습니다. Knative는 Istio와 통합하여 서버리스 및 컨테이너화된 워크로드가 인터넷에 쉽게 노출되고 모니터링되며 제어되고 전송 중에 데이터가 암호화되도록 지원합니다.

**Knative는 어떻게 작동합니까?**</br>
Knative는 Kuberbetes 클러스터에서 서버리스 앱을 빌드, 배치 및 관리하는 데 도움을 주는 세 개의 주요 컴포넌트(또는 _기본요소_)와 함께 제공됩니다. 

- **Build:** `Build` 기본요소는 소스 코드에서 컨테이너 이미지로 앱을 빌드하는 일련의 단계 작성을 지원합니다. 앱 코드와 이미지를 호스팅할 컨테이너 레지스트리를 찾을 소스 저장소를 지정하는 간단한 빌드 템플리트를 사용한다고 가정해 보십시오. 사용자는 하나의 명령만으로 컨테이너에서 해당 이미지를 사용할 수 있도록 이 빌드 템플리트를 사용하고, 소스 코드를 가져오고, 이미지를 작성하고, 이미지를 컨테이너 레지스트리에 푸시하도록 Knative에 지시할 수 있습니다. 
- **Serving:** `Serving` 기본요소는 서버리스 앱을 Knative 서비스로 배치하고 이를 자동으로 스케일링하는 데 도움이 됩니다(스케일링 다운하여 제로 인스턴스도 가능). Knative는 컨테이너화된 서버리스 워크로드를 노출하기 위해 Istio를 사용합니다. 관리 Knative 추가 기능을 설치하면 관리 Istio 추가 기능도 자동으로 설치됩니다. 사용자는 Istio의 트래픽 관리 및 지능형 라우팅 기능을 사용하여 서비스의 특정 버전으로 라우팅되는 트래픽을 제어할 수 있으며, 이는 개발자가 새 앱 버전을 테스트하고 롤아웃하거나 A-B 테스트를 수행하기 쉽게 해 줍니다. 
- **Eventing:** `Eventing` 기본요소를 사용하면 다른 서비스에서 구독할 수 있는 트리거 또는 이벤트 스트림을 작성할 수 있습니다. 예를 들어, 코드가 GitHub 마스터 저장소에 푸시될 때마다 앱의 새로운 빌드를 시작하고자 할 수 있습니다. 또는 온도가 빙점 이하로 떨어지는 경우에만 서버리스 앱을 실행하려고 합니다. 예를 들면, `Eventing` 기본요소는 CI/CD 파이프라인에 통합되어 특정 이벤트가 발생하는 경우에 앱의 빌드 및 배치를 자동화할 수 있습니다. 

**Managed Knative on {{site.data.keyword.containerlong_notm}}(시범) 추가 기능은 무엇입니까?** </br>
Managed Knative on {{site.data.keyword.containerlong_notm}}는 Kubernetes 클러스터와 Knative 및 Istio를 직접 통합하는 [관리 추가 기능](/docs/containers?topic=containers-managed-addons#managed-addons)입니다. 이 추가 기능의 Knative 및 Istio 버전은 IBM에 의해 테스트되었으며 {{site.data.keyword.containerlong_notm}}에서 사용할 수 있도록 지원됩니다. 관리 추가 기능에 대한 자세한 정보는 [관리 추가 기능을 사용하여 서비스 추가](/docs/containers?topic=containers-managed-addons#managed-addons)를 참조하십시오.

**제한사항이 있습니까?** </br>
클러스터에 [컨테이너 이미지 보안 적용기 허가 제어기](/docs/services/Registry?topic=registry-security_enforce#security_enforce)가 설치되어 있으면 클러스터에서 관리 Knative 추가 기능을 사용할 수 없습니다.

## 클러스터에 Knative 설정
{: #knative-setup}

Knative는 Istio 맨 위에 빌드되어 서버리스 및 컨테이너화된 워크로드가 클러스터 내부 및 인터넷에 노출될 수 있도록 보장합니다. 또한 Istio를 사용하면 사용자 서비스 간의 네트워크 트래픽을 모니터링하고 제어할 수 있으며 전송 중에 데이터가 암호화되도록 할 수 있습니다. 관리 Knative 추가 기능을 설치하면 관리 Istio 추가 기능도 자동으로 설치됩니다.
{: shortdesc}

시작하기 전에:
-  [IBM Cloud CLI, {{site.data.keyword.containerlong_notm}} 플러그인 및 Kubernetes CLI를 설치](/docs/containers?topic=containers-cs_cli_install#cs_cli_install_steps)하십시오. 클러스터의 Kubernetes 버전과 일치하는 `kubectl` CLI 버전을 설치해야 합니다.
-  [코어가 4개이고 메모리가 16GB 이상인 작업자 노드가 3개 이상 있는 표준 클러스터(`b3c.4x16`)를 작성](/docs/containers?topic=containers-clusters#clusters_ui)하십시오. 또한 해당 클러스터 및 작업자 노드는 지원되는 최소 Kubernetes 버전 이상을 실행해야 하며, 이는 `ibmcloud ks addon-versions --addon knative`를 실행하여 검토할 수 있습니다. 
-  {{site.data.keyword.containerlong_notm}}에 대해 [**작성자** 또는 **관리자** {{site.data.keyword.Bluemix_notm}} IAM 서비스 역할](/docs/containers?topic=containers-users#platform)을 보유하고 있는지 확인하십시오. 
-  [CLI를 클러스터에 대상으로 지정](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)하십시오.
</br>

클러스터에 Knative를 설치하려면 다음을 수행하십시오. 

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

2. Istio가 설치되었는지 확인하십시오. 아홉 개의 Istio 서비스용 팟(Pod)과 Prometheus용 팟(Pod)이 모두 `Running` 상태여야 합니다. 
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

4. Knative `Serving` 컴포넌트의 모든 팟(Pod)이 `Running` 상태인지 확인하십시오.
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

## Knative 서비스를 사용한 서버리스 앱 배치
{: #knative-deploy-app}

클러스터에 Knative를 설정한 후에는 서버리스 앱을 Knative 서비스로서 배치할 수 있습니다.
{: shortdesc}

**Knative 서비스의 개념** </br>
Knativ로 앱을 배치하려면 Knative `Service` 리소스를 지정해야 합니다. Knative 서비스는 Knative `Serving` 기본요소에 의해 관리되며 워크로드의 전체 라이프사이클을 관리합니다. 서비스를 작성하면 Knative `Serving` 기본요소가 자동으로 사용자 서버리스 앱의 버전을 작성하고 이 버전을 서비스의 개정 히스토리에 추가합니다. 사용자의 서버리스 앱에는 `<knative_service_name>.<namespace>.<ingress_subdomain>` 형식으로 Ingress 하위 도메인의 공용 URL이 지정되며, 사용자는 이를 사용하여 인터넷에서 해당 앱에 액세스할 수 있습니다. 클러스터 내에서 앱에 액세스하는 데 사용할 수 있는 개인용 호스트 이름 또한 앱에 `<knative_service_name>.<namespace>.cluster.local` 형식으로 지정됩니다. 

**Knative 서비스를 작성하면 어떤 관련 작업이 수행됩니까?**</br>
Knative 서비스를 작성하면 앱이 Kubernetes 팟(Pod)으로서 클러스터에 자동으로 배치되며 Kubernetes 서비스를 사용하여 노출됩니다. 공용 호스트 이름을 지정하기 위해, Knative는 IBM 제공 Ingress 하위 도메인 및 TLS 인증서를 사용합니다. 수신 네트워크 트래픽은 기본 IBM 제공 Ingress 라우팅 규칙에 따라 라우팅됩니다. 

**내 앱의 새 버전을 롤아웃하는 방법은 무엇입니까?**</br>
Knative 서비스를 업데이트하면 서버리스 앱의 새 버전이 작성됩니다. 이 버전에는 이전 버전과 동일한 공용 및 개인용 호스트 이름이 지정됩니다. 기본적으로 모든 수신 네트워크 트래픽은 사용자 앱의 최신 버전으로 라우팅됩니다. 그러나 A-B 테스트를 수행할 수 있도록 특정 앱 버전으로 라우팅되는 수신 네트워크 트래픽의 백분율을 지정할 수도 있습니다. 사용자는 한 번에 두 개의 앱 버전(앱의 현재 버전과 롤오버할 새 버전) 간에 수신 네트워크 트래픽을 분할할 수 있습니다.   

**나의 고유 사용자 정의 도메인 및 TLS 인증서를 사용할 수 있습니까?** </br>
서버리스 앱에 호스트 이름을 지정할 때 사용자 정의 도메인 이름 및 TLS 인증서를 사용하도록 Istio Ingress 게이트웨이의 ConfigMap과 Ingress 라우팅 규칙을 변경할 수 있습니다. 자세한 정보는 [사용자 정의 도메인 이름 및 인증서 설정](#knative-custom-domain-tls)을 참조하십시오. 

서버리스 앱을 Knative 서비스로 배치하려면 다음을 수행하십시오. 

1. Knative를 사용하여 첫 번째 서버리스 [`Hello World`](https://hub.docker.com/r/ibmcom/kn-helloworld) 앱을 위한 YAML 파일을 Go로 작성하십시오. 샘플 앱에 요청을 보내면 해당 앱은 환경 변수 `TARGET`을 읽고 `"Hello ${TARGET}!"`을 인쇄합니다. 환경 변수가 비어 있으면 `"Hello World!"`가 리턴됩니다.
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
    <td>선택사항: Knative 서비스로 앱을 배치할 Kubernetes 네임스페이스입니다. 기본적으로 모든 서비스는 <code>default</code> Kubernetes 네임스페이스에 배치됩니다. </td>
    </tr>
    <tr>
    <td><code>spec.container.image</code></td>
    <td>이미지가 저장된 컨테이너 레지스트리에 대한 URL입니다. 이 예제에서는 Docker Hub의 <code>ibmcom</code> 네임스페이스에 저장되는 Knative Hello World 앱을 배치합니다. </td>
    </tr>
    <tr>
    <td><code>spec.container.env</code></td>
    <td>선택사항: Knative 서비스가 사용하도록 할 환경 변수의 목록입니다. 이 예제에서, 환경 변수 <code>TARGET</code> 값은 샘플 앱에서 읽고 앱에 <code>"Hello ${TARGET}!"</code> 형식으로 요청을 보내면 리턴됩니다. 값이 제공되지 않으면 샘플 앱은 <code>"Hello World!"</code>를 리턴합니다.  </td>
    </tr>
    </tbody>
    </table>

2. 클러스터에 Knative 서비스를 작성하십시오.
   ```
   kubectl apply -f service.yaml
   ```
   {: pre}

   출력 예:
   ```
   service.serving.knative.dev "kn-helloworld" created
   ```
   {: screen}

3. Knative 서비스가 작성되었는지 확인하십시오. CLI 출력에서 사용자의 서버리스 앱에 지정된 공용 **DOMAIN**을 볼 수 있습니다. **LATESTCREATED** 및 **LATESTREADY** 열은 마지막으로 작성된 앱의 버전과 현재 배치된 앱의 버전을 `<knative_service_name>-<version>` 형식으로 보여줍니다. 앱에 지정되는 버전은 무작위 문자열 값입니다. 이 예에서 서버리스 앱의 버전은 `rjmwt`입니다. 서비스를 업데이트하면 앱의 새 버전이 작성되고 이 버전에 대한 새 무작위 문자열이 지정됩니다.   
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

4. 앱에 지정된 공용 URL에 요청을 전송하여 `Hello World` 앱을 사용해 보십시오. 
   ```
   curl -v <public_app_url>
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

5. Knative 서비스용으로 작성된 팟(Pod)의 수를 나열하십시오. 이 주제의 예에서는 두 개의 컨테이너로 구성된 하나의 팟(Pod)이 배치되었습니다. 하나의 컨테이너가 `Hello World` 앱을 실행하며 다른 컨테이너는 Istio 및 Knative 모니터링과 로깅 도구를 실행하는 사이드카입니다.
   ```
   kubectl get pods
   ```
   {: pre}

   출력 예:
   ```
   NAME                                             READY     STATUS    RESTARTS   AGE
   kn-helloworld-rjmwt-deployment-55db6bf4c5-2vftm  2/2      Running   0          16s
   ```
   {: screen}

6. Knative가 팟(Pod)을 스케일링 다운을 수행하도록 몇 분간 기다리십시오. Knative는 수신 워크로드를 처리하는 데 필요한 시간동안 작동되어야 하는 팟(Pod)의 수를 평가합니다. 네트워크 트래픽이 수신되지 않으면 Knative가 자동으로 팟(Pod)을 스케일링 다운합니다. 이 예제에 표시된 바와 같이 제로 팟(Pod)이 될 수도 있습니다.

   Knative가 팟(Pod)을 스케일링 업하는 방법을 확인하시겠습니까? 예를 들어, [단순 클라우드 기반 로드 테스터](https://loader.io/)와 같은 도구를 사용하여 앱의 워크로드를 늘리십시오.
   {: tip}
   ```
   kubectl get pods
   ```
   {: pre}

   `kn-helloworld` 팟(Pod)이 표시되지 않으면 Knative가 앱을 제로 팟(Pod)으로 스케일링 다운한 것입니다.

7. Knative 서비스 샘플을 업데이트하고 `TARGET` 환경 변수에 다른 값을 입력하십시오.

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

8. 서비스에 변경을 적용하십시오. 구성을 변경하면 Knative가 자동으로 앱의 새 버전을 작성합니다. 
   ```
   kubectl apply -f service.yaml
   ```
   {: pre}

9. 앱의 새 버전이 배치되었는지 확인하십시오. CLI 출력의 **LATESTCREATED** 열에서 앱의 새 버전을 볼 수 있습니다. **LATESTREADY** 열에 동일한 앱 버전이 표시되면 앱의 설정이 완료되어 지정된 공용 URL에서 수신 네트워크 트래픽을 수신할 준비가 된 것입니다. 
   ```
   kubectl get ksvc/kn-helloworld
   ```
   {: pre}

   출력 예:
   ```
   NAME            DOMAIN                                                                LATESTCREATED         LATESTREADY           READY   REASON
   kn-helloworld   kn-helloworld.default.mycluster.us-south.containers.appdomain.cloud   kn-helloworld-ghyei   kn-helloworld-ghyei   True
   ```
   {: screen}

9. 변경사항이 적용되었는지 확인하려면 앱에 대한 새 요청을 수행하십시오.
   ```
   curl -v <service_domain>
   ```

   출력 예:
   ```
   ...
   Hello Mr. Smith!
   ```
   {: screen}

10. Knative가 증가된 네트워크 트래픽을 고려하여 팟(Pod)을 다시 스케일링 업했는지 확인하십시오.
    ```
    kubectl get pods
    ```

    출력 예:
    ```
    NAME                                              READY     STATUS    RESTARTS   AGE
    kn-helloworld-ghyei-deployment-55db6bf4c5-2vftm   2/2       Running   0          16s
    ```
    {: screen}

11. 선택사항: Knative 서비스를 정리하십시오.
    ```
    kubectl delete -f service.yaml
    ```
    {: pre}


## 사용자 정의 도메인 이름 및 인증서 설정
{: #knative-custom-domain-tls}

TLS 구성에 사용한 고유 사용자 정의 도메인의 호스트 이름을 지정하도록 Knative를 구성할 수 있습니다.
{: shortdesc}

기본적으로 모든 앱에는 `<knative_service_name>.<namespace>.<ingress_subdomain>` 형식으로 Ingress 하위 도메인의 공용 URL이 지정되며, 사용자는 이를 사용하여 인터넷에서 해당 앱에 액세스할 수 있습니다. 클러스터 내에서 앱에 액세스하는 데 사용할 수 있는 개인용 호스트 이름 또한 앱에 `<knative_service_name>.<namespace>.cluster.local` 형식으로 지정됩니다. 자신이 소유한 사용자 정의 도메인의 호스트 이름을 지정하려는 경우에는 사용자 정의 도메인을 사용하도록 Knative ConfigMap을 변경할 수 있습니다. 

1. 사용자 정의 도메인을 작성하십시오. 사용자 정의 도메인을 등록하려면 DNS(Domain Name Service) 제공자 또는 [IBM Cloud DNS](/docs/infrastructure/dns?topic=dns-getting-started)를 통해 작업하십시오. 
2. 수신 네트워크 트래픽을 IBM 제공 Ingress 게이트웨이로 라우팅하도록 사용자의 도메인을 구성하십시오. 다음 옵션 중에서 선택하십시오.
   - 표준 이름 레코드(CNAME)로서 IBM 제공 도메인을 지정하여 사용자 정의 도메인의 별명을 정의하십시오. IBM 제공 Ingress 도메인을 찾으려면 `ibmcloud ks cluster-get --cluster <cluster_name>`을 실행하고 **Ingress subdomain** 필드를 찾으십시오. IBM에서 IBM 하위 도메인의 자동 상태 검사를 제공하고 실패한 IP를 DNS 응답에서 제거하므로, CNAME을 사용하도록 권장합니다.
   - IP 주소를 레코드로 추가하여 사용자 정의 도메인을 Ingress 게이트웨이의 포터블 공인 IP 주소에 맵핑하십시오. Ingress 게이트웨이의 공인 IP 주소를 찾으려면 `nslookup <ingress_subdomain>`을 실행하십시오. 
3. 사용자 정의 도메인에 대한 공식 와일드카드 TLS 인증서를 구입하십시오. 여러 TLS 인증서를 구입하려는 경우에는 각 인증서의 [CN ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://support.dnsimple.com/articles/what-is-common-name/)이 서로 다른지 확인하십시오. 
4. 인증서 및 키에 대한 Kubernetes 시크릿을 작성하십시오. 
   1. 인증서 및 키를 Base-64로 인코딩하고 Base-64로 인코딩된 값을 새 파일에 저장하십시오.
      ```
      openssl base64 -in tls.key -out tls.key.base64
      ```
      {: pre}

   2. 인증서 및 키의 Base-64로 인코딩된 값을 보십시오.
      ```
      cat tls.key.base64
      ```
      {: pre}

   3. 인증서 및 키를 사용하여 시크릿 YAML 파일을 작성하십시오.
      ```
      apiVersion: v1
      kind: Secret
      metadata:
        name: mydomain-ssl
      type: Opaque
      data:
        tls.crt: <client_certificate>
        tls.key: <client_key>
      ```
      {: codeblock}

   4. 클러스터에 인증서를 작성하십시오.
      ```
      kubectl create -f secret.yaml
      ```
      {: pre}

5. 클러스터의 `istio-system` 네임스페이스에 있는 `iks-knative-ingress` Ingress 리소스를 열어 편집을 시작하십시오. 
   ```
   kubectl edit ingress iks-knative-ingress -n istio-system
   ```
   {: pre}

6. Ingress에 대한 기본 라우팅 규칙을 변경하십시오. 
   - 사용자 정의 도메인 및 하위 도메인의 모든 수신 네트워크 트래픽이 `istio-ingressgateway`로 라우팅되도록 `spec.rules.host` 섹션에 사용자 정의 와일드카드 도메인을 추가하십시오. 
   - `spec.tls.hosts` 섹션에서, 이전에 작성한 TLS 시크릿을 사용하도록 사용자 정의 와일드카드 도메인의 모든 호스트를 구성하십시오. 

   Ingress 예:
   ```
   apiVersion: extensions/v1beta1
   kind: Ingress
   metadata:
     name: iks-knative-ingress
     namespace: istio-system
   spec:
     rules:
     - host: '*.mydomain'
       http:
         paths:
         - backend:
             serviceName: istio-ingressgateway
             servicePort: 80
           path: /
     tls:
     - hosts:
       - '*.mydomain'
       secretName: mydomain-ssl
   ```
   {: codeblock}

   `spec.rules.host` 및 `spec.tls.hosts` 섹션은 목록이며 여러 사용자 정의 도메인 및 TLS 인증서를 포함할 수 있습니다.
   {: tip}

7. 새 Knative 서비스에 호스트 이름을 지정하는 데 사용자 정의 도메인을 사용하도록 Knative `config-domain` ConfigMap을 수정하십시오. 
   1. `config-domain` ConfigMap을 열어 편집을 시작하십시오.
      ```
      kubectl edit configmap config-domain -n knative-serving
      ```
      {: pre}

   2. ConfigMap의 `data` 섹션에 사용자 정의 도메인을 지정하고 클러스터에 대해 설정된 기본 도메인을 제거하십시오. 
      - **사용자 정의 도메인의 호스트 이름을 모든 Knative 서비스에 지정하는 예**:
        ```
        apiVersion: v1
        kind: ConfigMap
        data:
          <custom_domain>: ""
        metadata:
          name: config-domain
          namespace: knative-serving
        ```
        {: codeblock}

        `""`를 사용자 정의 도메인에 추가하면 작성하는 모든 Knative 서비스에 사용자 정의 도메인의 호스트 이름이 지정됩니다.   

      - **사용자 정의 도메인의 호스트 이름을 일부 Knative 서비스에 지정하는 예**:
        ```
        apiVersion: v1
        kind: ConfigMap
        data:
          <custom_domain>: |
            selector:
              app: sample
          mycluster.us-south.containers.appdomain.cloud: ""
        metadata:
          name: config-domain
          namespace: knative-serving
        ```
        {: codeblock}

        사용자 정의 도메인의 호스트 이름을 일부 Knative 서비스에만 지정하려면 ConfigMap에 `data.selector` 레이블 키 및 값을 추가하십시오. 이 예에서는 `app: sample`로 레이블 지정된 모든 서비스에 사용자 정의 도메인의 호스트 이름이 지정됩니다. `app: sample` 레이블이 없는 모든 기타 앱에 지정할 도메인 이름이 있는지도 확인하십시오. 이 예에서는 기본 IBM 제공 도메인인 `mycluster.us-south.containers.appdomain.cloud`가 사용됩니다. 
    3. 변경사항을 저장하십시오.

Ingress 라우팅 규칙과 Knative ConfigMap이 모두 설정되었으면 사용자 정의 도메인 및 TLS 인증서를 사용하여 Knative 서비스를 작성할 수 있습니다. 

## 다른 Knative 서비스에서의 특정 Knative 서비스 액세스
{: #knative-access-service}

Knative 서비스에 지정된 URL에 대한 REST API 호출을 사용하여 다른 Knative 서비스에서 사용자의 Knative 서비스에 액세스할 수 있습니다.
{: shortdesc}

1. 클러스터에 있는 모든 Knative 서비스를 나열하십시오. 
   ```
   kubectl get ksvc --all-namespaces
   ```
   {: pre}

2. 사용자의 Knative 서비스에 지정된 **DOMAIN**을 검색하십시오. 
   ```
   kubect get ksvc/<knative_service_name>
   ```
   {: pre}

   출력 예:
   ```
   NAME        DOMAIN                                                            LATESTCREATED     LATESTREADY       READY   REASON
   myservice   myservice.default.mycluster.us-south.containers.appdomain.cloud   myservice-rjmwt   myservice-rjmwt   True
   ```
   {: screen}

3. 이 도메인 이름을 사용하여 사용자의 Knative 서비스에 액세스하기 위한 REST API 호출을 구현하십시오. 이 REST API 호출은 Knative 서비스를 작성한 앱의 일부여야 합니다. 액세스하려는 Knative 서비스에 `<service_name>.<namespace>.svc.cluster.local` 형식의 로컬 URL이 지정되어 있는 경우 Knative는 REST API 요청을 클러스터 내부 네트워크로 유지합니다. 

   Go를 사용한 코드 스니펫 예:
   ```go
   resp, err := http.Get("<knative_service_domain_name>")
   if err != nil {
       fmt.Fprintf(os.Stderr, "Error: %s\n", err)
   } else if resp.Body != nil {
       body, _ := ioutil.ReadAll(resp.Body)
       fmt.Printf("Response: %s\n", string(body))
   }
   ```
   {: codeblock}

## 공통 Knative 서비스 설정
{: #knative-service-settings}

서버리스 앱을 배치할 때 유용한 공통 Knative 서비스 설정을 검토하십시오.
{: shortdesc}

- [최대 및 최소 팟(Pod) 수 설정](#knative-min-max-pods)
- [팟(Pod)당 최대 요청 수 지정](#max-request-per-pod)
- [개인용 전용 서버리스 앱 작성](#knative-private-only)
- [Knative 서비스가 컨테이너 이미지를 다시 가져오도록 강제](#knative-repull-image)

### 최대 및 최소 팟(Pod) 수 설정
{: #knative-min-max-pods}

어노테이션을 사용하여 앱에 대해 실행할 최소 및 최대 팟(Pod) 수를 지정할 수 있습니다. 예를 들어, Knative가 0개의 인스턴스로 스케일링 다운되지 않도록 하려면 최소 팟(Pod) 수를 1로 설정하십시오.
{: shortdesc}

```
apiVersion: serving.knative.dev/v1alpha1
kind: Service
metadata:
  name: ...
spec:
  runLatest:
    configuration:
      revisionTemplate:
        metadata:
          annotations:
            autoscaling.knative.dev/minScale: "1"
            autoscaling.knative.dev/maxScale: "100"
        spec:
          container:
            image:
...
```
{: codeblock}

<table>
<caption>YAML 파일 컴포넌트 이해</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> YAML 파일 컴포넌트 이해</th>
</thead>
<tbody>
<tr>
<td><code>autoscaling.knative.dev/minScale</code></td>
<td>클러스터에서 실행할 최소 팟(Pod) 수를 입력하십시오. Knative는 앱에서 수신하는 네트워크 트래픽이 없는 경우에도 설정된 수보다 적게 앱을 스케일링 다운할 수 없습니다. 기본 팟(Pod) 수는 0입니다. </td>
</tr>
<tr>
<td><code>autoscaling.knative.dev/maxScale</code></td>
<td>클러스터에서 실행할 최대 팟(Pod) 수를 입력하십시오. Knative는 현재 앱 인스턴스가 처리할 수 있는 것보다 많은 요청이 있는 경우에도 설정된 수보다 많게 앱을 스케일링 업할 수 없습니다. </td>
</tr>
</tbody>
</table>

### 팟(Pod)당 최대 요청 수 지정
{: #max-request-per-pod}

Knative가 앱 인스턴스의 스케일링 업을 고려하기 전에 앱 인스턴스가 수신하여 처리할 수 있는 최대 요청 수를 지정할 수 있습니다. 예를 들어, 최대 요청 수를 1로 설정하면 앱 인스턴스는 한 번에 하나의 요청을 수신할 수 있습니다. 첫 번째 요청이 완전히 처리되기 전에 두 번째 요청이 도착하는 경우 Knative는 다른 인스턴스를 스케일링 업합니다. 

```
apiVersion: serving.knative.dev/v1alpha1
kind: Service
metadata:
  name: myservice
spec:
  runLatest:
    configuration:
      revisionTemplate:
        spec:
          container:
            image: myimage
          containerConcurrency: 1
....
```
{: codeblock}

<table>
<caption>YAML 파일 컴포넌트 이해</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> YAML 파일 컴포넌트 이해</th>
</thead>
<tbody>
<tr>
<td><code>containerConcurrency</code></td>
<td>Knative가 앱 인스턴스의 스케일링 업을 고려하기 전에 앱 인스턴스가 한 번에 수신할 수 있는 최대 요청 수를 입력하십시오. </td>
</tr>
</tbody>
</table>

### 개인용 전용 서버리스 앱 작성
{: #knative-private-only}

기본적으로 모든 Knative 서비스에는 Istio Ingress 하위 도메인의 공용 라우트와 `<service_name>.<namespace>.cluster.local` 형식의 개인용 라우트가 지정됩니다. 사용자는 공용 라우트를 사용하여 공용 네트워크에서 앱에 액세스할 수 있습니다. 서비스를 개인용으로 유지하려는 경우에는 Knative 서비스에 `serving.knative.dev/visibility` 레이블을 추가할 수 있습니다. 이 레이블은 사용자의 서비스에 개인용 호스트 이름만 지정하도록 Knative에 지시합니다.
{: shortdesc}

```
apiVersion: serving.knative.dev/v1alpha1
kind: Service
metadata:
  name: myservice
  labels:
    serving.knative.dev/visibility: cluster-local
spec:
  runLatest:
    configuration:
      revisionTemplate:
        spec:
          container:
            image:
...
```
{: codeblock}

<table>
<caption>YAML 파일 컴포넌트 이해</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> YAML 파일 컴포넌트 이해</th>
</thead>
<tbody>
<tr>
<td><code>serving.knative.dev/visibility</code></td>
  <td><code>serving.knative.dev/visibility: cluster-local</code> 레이블을 추가하면 서비스에 <code>&lt;service_name&gt;.&lt;namespace&gt;.cluster.local</code> 형식의 개인용 라우트만 지정됩니다. 클러스터 내에서는 개인용 호스트 이름을 사용하여 서비스에 액세스할 수 있지만, 공용 네트워크에서 서비스에 액세스할 수는 없습니다. </td>
</tr>
</tbody>
</table>

### Knative 서비스가 컨테이너 이미지를 다시 가져오도록 강제
{: #knative-repull-image}

Knative의 현재 구현은 Knative `Serving` 컴포넌트가 컨테이너 이미지를 다시 가져오도록 하는 일반적인 방법을 제공하지 않습니다. 레지스트리에서 이미지를 다시 가져오려면 다음 옵션 중 하나를 선택하십시오. 

- **Knative 서비스 `revisionTemplate` 수정**: Knative 서비스의 `revisionTemplate`는 Knative 서비스의 개정을 작성하는 데 사용됩니다. `repullFlag`  어노테이션을 추가하는 등과 같이 이 개정 템플리트를 수정하는 경우 Knative는 앱의 새 개정을 작성해야 합니다. 개정 작성 과정의 일부로서 Knative는 컨테이너 이미지 업데이트를 확인해야 합니다. `imagePullPolicy: Always`를 설정하는 경우 Knative는 클러스터의 이미지 캐시를 사용할 수 없으며, 대신 이미지를 컨테이너 레지스트리에서 가져와야 합니다. 
   ```
   apiVersion: serving.knative.dev/v1alpha1
   kind: Service
   metadata:
     name: <service_name>
   spec:
     runLatest:
       configuration:
         revisionTemplate:
           metadata:
             annotations:
               repullFlag: 123
           spec:
             container:
               image: <image_name>
               imagePullPolicy: Always
    ```
    {: codeblock}

    컨테이너 레지스트리에서 최신 이미지 버전을 가져오는 서비스의 새 개정을 작성하려 할 때마다 `repullFlag` 값을 변경해야 합니다. 동일한 두 Knative 서비스 구성으로 인해 Knative가 이전 이미지 버전을 사용하지 않도록 각 개정에 대해 고유 값을 사용하십시오.   
    {: note}

- **태그를 사용하여 고유 컨테이너 이미지 작성**: 작성하는 각 컨테이너 이미지에 대해 고유 태그를 사용하고 Knative 서비스 `container.image` 구성에서 이를 참조할 수 있습니다. 다음 예에서는 `v1`이 이미지 태그로 사용되었습니다. Knative가 컨테이너 레지스트리에서 새 이미지를 가져오도록 강제하려면 이미지 태그를 변경해야 합니다. 예를 들면, `v2`를 새 이미지 태그로 사용하십시오. 
  ```
  apiVersion: serving.knative.dev/v1alpha1
  kind: Service
  metadata:
    name: <service_name>
  spec:
    runLatest:
      configuration:
          spec:
            container:
              image: myapp:v1
              imagePullPolicy: Always
    ```
    {: codeblock}


## 관련 링크  
{: #knative-related-links}

- 이 [Knative 워크샵 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/IBM/knative101/tree/master/workshop)을 사용하여 첫 번째 `Node.js` fibonacci 앱을 클러스터에 배치하십시오.
  - Knative `Build` 기본요소의 사용 방법을 탐색하고 GitHub의 Dockerfile에서 이미지를 빌드하여 해당 이미지를 자동으로 {{site.data.keyword.registrylong_notm}}의 네임스페이스에 푸시하십시오.  
  - IBM 제공 Ingress 하위 도메인으로부터 Knative에서 제공하는 Istio Ingress 게이트웨이로 네트워크 트래픽을 라우팅하도록 설정하는 방법에 대해 알아보십시오.
  - 앱의 새 버전을 롤아웃하고 Istio를 사용하여 각 앱 버전으로 라우팅되는 트래픽의 양을 제어하십시오.
- [Knative `Eventing` ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/knative/docs/tree/master/eventing/samples) 샘플을 탐색하십시오.
- Knative를 자세히 알아보려면 [Knative 문서 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/knative/docs)를 참조하십시오.
