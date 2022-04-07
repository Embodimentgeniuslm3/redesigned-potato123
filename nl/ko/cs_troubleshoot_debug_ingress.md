---

copyright:
  years: 2014, 2019
lastupdated: "2019-05-31"

keywords: kubernetes, iks, nginx, ingress controller

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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}


# Ingress 디버깅
{: #cs_troubleshoot_debug_ingress}

{{site.data.keyword.containerlong}}를 사용할 때는 일반적인 Ingress 문제점 해결 및 디버깅과 관련된 이러한 기술을 고려하십시오.
{: shortdesc}

클러스터에 앱의 Ingress 리소스를 작성하여 공용으로 앱을 노출했습니다. 그러나 ALB의 공인 IP 주소나 하위 도메인을 통해 앱에 연결을 시도하면 연결에 실패하거나 제한시간이 초과됩니다. 다음 절의 단계는 Ingress 설정을 디버깅하는 데 도움이 될 수 있습니다.

하나의 호스트를 하나의 Ingress 리소스에서만 정의했는지 확인하십시오. 여러 Ingress 리소스에 하나의 호스트를 정의하면 ALB가 올바르게 트래픽을 전달하지 않으면서 오류가 발생할 수 있습니다.
{: tip}

시작하기 전에 다음의 [{{site.data.keyword.Bluemix_notm}} IAM 액세스 정책](/docs/containers?topic=containers-users#platform)이 있는지 확인하십시오.
  - 클러스터에 대한 **편집자** 또는 **관리자** 플랫폼 역할
  - **작성자** 또는 **관리자** 서비스 역할

## 1단계: {{site.data.keyword.containerlong_notm}} 진단 및 디버그 도구로 Ingress 테스트 실행

문제점을 해결하는 중에 {{site.data.keyword.containerlong_notm}} 진단 및 디버그 도구를 사용하여 Ingress 테스트를 실행하여 관련 Ingress 정보를 클러스터에서 수집할 수 있습니다. 디버그 도구를 사용하려면 다음과 같이 [`ibmcloud-iks-debug` Helm 차트 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/kubernetes/helm/iks-charts/ibmcloud-iks-debug)를 설치하십시오.
{: shortdesc}


1. [클러스터에 Helm을 설정하고 Tiller의 서비스 계정을 작성한 후 Helm 인스턴스에 `ibm` 저장소를 추가](/docs/containers?topic=containers-helm)하십시오.

2. 클러스터에 Helm 차트를 설치하십시오.
  ```
  helm install iks-charts/ibmcloud-iks-debug --name debug-tool
  ```
  {: pre}


3. 프록시 서버를 시작하여 디버그 도구 인터페이스를 표시하십시오.
  ```
  kubectl proxy --port 8080
  ```
  {: pre}

4. 웹 브라우저에서 다음 디버그 도구 인터페이스 URL을 여십시오. http://localhost:8080/api/v1/namespaces/default/services/debug-tool-ibmcloud-iks-debug:8822/proxy/page

5. 테스트의 **ingress** 그룹을 선택하십시오. 일부 테스트에서는 잠재적 경고, 오류 또는 문제를 확인하고 일부 테스트에서는 문제점을 해결하는 동안 참조할 수 있는 정보만 수집합니다. 각 테스트의 기능에 대한 자세한 정보는 테스트 이름 옆의 정보 아이콘을 클릭하십시오.

6. **실행**을 클릭하십시오.

7. 각 테스트의 결과를 확인하십시오.
  * 테스트가 실패하는 경우, 문제 해결 방법에 대한 정보는 왼쪽 열의 테스트 이름 옆에 있는 정보 아이콘을 클릭하십시오.
  * 또한 다음 섹션에서 Ingress 서비스를 디버그하는 동안애만 정보를 수집하는 테스트 결과를 사용할 수 있습니다.

## 2단계: Ingress 배치 및 ALB 팟(Pod) 로그에서 오류 메시지 확인
{: #errors}

Ingress 리소스 배치 이벤트와 ALB 팟(Pod) 로그에서 오류 메시지를 확인하여 시작하십시오. 이러한 오류 메시지는 장애에 대한 근본 원인을 찾고 다음 절에서 Ingress 설정을 추가로 디버깅하는 데 도움이 될 수 있습니다.
{: shortdesc}

1. Ingress 리소스 배치를 확인하고 경고 또는 오류 메시지를 찾으십시오.
    ```
  kubectl describe ingress <myingress>
    ```
    {: pre}

    출력의 **Events** 섹션에서 Ingress 리소스 또는 사용된 특정 어노테이션의 올바르지 않은 값에 대한 경고 메시지가 표시될 수 있습니다. [Ingress 리소스 구성 문서](/docs/containers?topic=containers-ingress#public_inside_4) 또는 [어노테이션 문서](/docs/containers?topic=containers-ingress_annotation)를 확인하십시오.

    ```
        Name:             myingress
    Namespace:        default
    Address:          169.xx.xxx.xxx,169.xx.xxx.xxx
    Default backend:  default-http-backend:80 (<none>)
    Rules:
      Host                                             Path  Backends
      ----                                             ----  --------
      mycluster.us-south.containers.appdomain.cloud
                                                       /tea      myservice1:80 (<none>)
                                                       /coffee   myservice2:80 (<none>)
    Annotations:
      custom-port:        protocol=http port=7490; protocol=https port=4431
      location-modifier:  modifier='~' serviceName=myservice1;modifier='^~' serviceName=myservice2
    Events:
      Type     Reason             Age   From                                                            Message
      ----     ------             ----  ----                                                            -------
      Normal   Success            1m    public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Successfully applied ingress resource.
      Warning  TLSSecretNotFound  1m    public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Failed to apply ingress resource.
      Normal   Success            59s   public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Successfully applied ingress resource.
      Warning  AnnotationError    40s   public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Failed to apply ingress.bluemix.net/custom-port annotation. Error annotation format error : One of the mandatory fields not valid/missing for annotation ingress.bluemix.net/custom-port
      Normal   Success            40s   public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Successfully applied ingress resource.
      Warning  AnnotationError    2s    public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Failed to apply ingress.bluemix.net/custom-port annotation. Invalid port 7490. Annotation cannot use ports 7481 - 7490
      Normal   Success            2s    public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Successfully applied ingress resource.
    ```
    {: screen}
{: #check_pods}
2. ALB 팟(Pod)의 상태를 확인하십시오.
    1. 클러스터에서 실행 중인 ALB 팟(Pod)을 가져오십시오.
        ```
        kubectl get pods -n kube-system | grep alb
        ```
        {: pre}

    2. **STATUS** 열을 확인하여 모든 팟(Pod)이 실행 중인지 확인하십시오.

    3. 팟(Pod)이 `Running` 상태가 아니면 ALB를 사용 중지한 후에 이를 다시 사용할 수 있습니다. 다음 명령에서 `<ALB_ID>`를 팟(Pod)의 ALB의 ID로 대체하십시오. 예를 들어, 실행 중이 아닌 팟(Pod)의 이름이 `public-crb2f60e9735254ac8b20b9c1e38b649a5-alb1-5d6d86fbbc-kxj6z`이면 ALB ID는 `public-crb2f60e9735254ac8b20b9c1e38b649a5-alb1`입니다.
        ```
        ibmcloud ks alb-configure --albID <ALB_ID> --disable
        ```
        {: pre}

        ```
        ibmcloud ks alb-configure --albID <ALB_ID> --enable
        ```
        {: pre}

3. ALB에 대한 로그를 확인하십시오.
    1.  클러스터에서 실행 중인 ALB 팟(Pod)의 ID를 가져오십시오.
        ```
        kubectl get pods -n kube-system | grep alb
        ```
        {: pre}

    3. 각 ALB 팟(Pod)의 `nginx-ingress` 컨테이너에 대한 로그를 가져오십시오.
        ```
      kubectl logs <ingress_pod_ID> nginx-ingress -n kube-system
        ```
        {: pre}

    4. ALB 로그에서 오류 메시지를 찾으십시오.

## 3단계: ALB 하위 도메인 및 공인 IP 주소에 대한 Ping 실행
{: #ping}

Ingress 하위 도메인과 ALB의 공인 IP 주소에 대한 가용성을 확인하십시오.
{: shortdesc}

1. 공용 ALB가 청취 중인 IP 주소를 가져오십시오.
    ```
    ibmcloud ks albs --cluster <cluster_name_or_ID>
    ```
    {: pre}

    `dal10` 및 `dal13`에서 작업자 노드의 다중 구역 클러스터에 대한 예제 출력:

    ```
    ALB ID                                            Enabled   Status     Type      ALB IP          Zone    Build                          ALB VLAN ID
    private-cr24a9f2caf6554648836337d240064935-alb1   false     disabled   private   -               dal13   ingress:411/ingress-auth:315   2294021
    private-cr24a9f2caf6554648836337d240064935-alb2   false     disabled   private   -               dal10   ingress:411/ingress-auth:315   2234947
    public-cr24a9f2caf6554648836337d240064935-alb1    true      enabled    public    169.62.196.238  dal13   ingress:411/ingress-auth:315   2294019
    public-cr24a9f2caf6554648836337d240064935-alb2    true      enabled    public    169.46.52.222   dal10   ingress:411/ingress-auth:315   2234945
    ```
    {: screen}

    * 공용 ALB에 IP 주소가 없는 경우에는 [Ingress ALB가 구역에 배치되지 않음](/docs/containers?topic=containers-cs_troubleshoot_network#cs_subnet_limit)을 참조하십시오.

2. ALB IP의 상태를 확인하십시오.

    * 단일 구역 클러스터 및 다중 구역 클러스터의 경우: 각 공용 ALB의 IP 주소에 대해 ping을 실행하여 각 ALB가 패킷을 성공적으로 수신할 수 있는지 확인하십시오. 사설 ALB를 사용 중이면 사설 네트워크에서만 해당 IP 주소에 대해 ping을 실행할 수 있습니다.
        ```
      ping <ALB_IP>
        ```
        {: pre}

        * CLI가 제한시간 초과를 리턴하고 작업자 노드를 보호하는 사용자 정의 방화벽이 있는 경우, [방화벽](/docs/containers?topic=containers-cs_troubleshoot_clusters#cs_firewall)에서 ICMP가 허용되는지 확인하십시오.
        * Ping 실행을 차단하는 방화벽이 없으며 Ping 실행이 여전히 제한시간을 초과하는 경우에는 [ALB 팟(Pod)의 상태를 확인](#check_pods)하십시오.

    * 다중 구역 클러스터에만 해당: MZLB 상태 검사를 사용하여 ALB IP의 상태를 판별할 수 있습니다. MZLB에 대한 자세한 정보는 [다중 구역 로드 밸런서(MZLB)](/docs/containers?topic=containers-ingress#planning)를 참조하십시오. MZLB 상태 검사는 `<cluster_name>.<region_or_zone>.containers.appdomain.cloud` 형식의 새 Ingress 하위 도메인이 있는 클러스터에만 사용할 수 있습니다. 클러스터가 `<cluster_name>.<region>.containers.mybluemix.net`의 이전 형식을 계속해서 사용하는 경우 [단일 구역 클러스터를 다중 구역으로 변환](/docs/containers?topic=containers-add_workers#add_zone)하십시오. 클러스터에 새 형식의 하위 도메인이 지정되지만, 이전 하위 도메인 형식이 계속 사용될 수도 있습니다. 또는 새 하위 도메인 형식이 자동으로 지정된 새 클러스터를 주문할 수 있습니다.

    다음의 HTTP cURL 명령에서는 ALB IP에 대해 `healthy` 또는 `unhealthy` 상태를 리턴하도록 {{site.data.keyword.containerlong_notm}}에 의해 구성된 `albhealth` 호스트를 사용합니다.
        ```
            curl -X GET http://169.62.196.238/ -H "Host: albhealth.mycluster-12345.us-south.containers.appdomain.cloud"
        ```
        {: pre}

        출력 예:
        ```
        healthy
        ```
        {: screen}
            하나 이상의 IP가 `unhealthy`를 리턴하면 [ALB 팟(Pod)의 상태를 확인](#check_pods)하십시오.

3. IBM 제공 Ingress 하위 도메인을 가져오십시오.
    ```
    ibmcloud ks cluster-get --cluster <cluster_name_or_ID> | grep Ingress
    ```
    {: pre}

    출력 예:
    ```
    Ingress Subdomain:      mycluster-12345.us-south.containers.appdomain.cloud
    Ingress Secret:         <tls_secret>
    ```
    {: screen}

4. 이 절의 2단계에서 가져온 각각의 공용 ALB에 대한 IP가 클러스터의 IBM 제공 Ingress 하위 도메인에 등록되었는지 확인하십시오. 예를 들어, 다중 구역 클러스터에서 작업자 노드가 있는 각 구역의 공용 ALB IP는 동일한 호스트 이름으로 등록되어야 합니다.

    ```
    kubectl get ingress -o wide
    ```
    {: pre}

    출력 예:
    ```
    NAME                HOSTS                                                    ADDRESS                        PORTS     AGE
    myingressresource   mycluster-12345.us-south.containers.appdomain.cloud      169.46.52.222,169.62.196.238   80        1h
    ```
    {: screen}

## 4단계: 도메인 맵핑 및 Ingress 리소스 구성 확인
{: #ts_ingress_config}

1. 사용자 정의 도메인을 사용하는 경우에는 DNS 제공자를 사용하여 사용자 정의 도메인을 IBM 제공 하위 도메인 또는 ALB의 공인 IP 주소로 맵핑했는지 확인하십시오. 참고로, IBM에서 IBM 하위 도메인의 자동 상태 검사를 제공하고 실패한 IP를 DNS 응답에서 제거하므로 CNAME을 사용하도록 권장합니다.
    * IBM 제공 하위 도메인: 사용자 정의 도메인이 표준 이름 레코드(CNAME)에 있는 클러스터의 IBM 제공 하위 도메인에 맵핑되었는지 확인하십시오.
        ```
        host www.my-domain.com
        ```
        {: pre}

        출력 예:
        ```
        www.my-domain.com is an alias for mycluster-12345.us-south.containers.appdomain.cloud
        mycluster-12345.us-south.containers.appdomain.cloud has address 169.46.52.222
        mycluster-12345.us-south.containers.appdomain.cloud has address 169.62.196.238
        ```
        {: screen}

    * 공인 IP 주소: 사용자 정의 도메인이 A 레코드에 있는 ALB의 포터블 공인 IP 주소에 맵핑되었는지 확인하십시오. IP는 [이전 절](#ping)의 1단계에서 가져온 공용 ALB IP와 일치해야 합니다.
        ```
        host www.my-domain.com
        ```
        {: pre}

        출력 예:
        ```
        www.my-domain.com has address 169.46.52.222
        www.my-domain.com has address 169.62.196.238
        ```
        {: screen}

2. 클러스터의 Ingress 리소스 구성 파일을 확인하십시오.
    ```
    kubectl get ingress -o yaml
    ```
    {: pre}

    1. 하나의 호스트를 하나의 Ingress 리소스에서만 정의했는지 확인하십시오. 여러 Ingress 리소스에 하나의 호스트를 정의하면 ALB가 올바르게 트래픽을 전달하지 않으면서 오류가 발생할 수 있습니다.

    2. 하위 도메인 및 TLS 인증서가 올바른지 확인하십시오. IBM 제공 Ingress 하위 도메인 및 TLS 인증서를 찾으려면 `ibmcloud ks cluster-get --cluster <cluster_name_or_ID>`를 실행하십시오.

    3.  Ingress의 **path** 섹션에 구성된 동일한 경로에서 앱이 청취하는지 확인하십시오. 루트 경로에서 청취하도록 앱이 설정된 경우에는 `/`를 경로로 사용하십시오. 이 경로에 대한 수신 트래픽을 앱이 청취하는 다른 경로로 라우팅해야 하는 경우에는 [rewrite paths](/docs/containers?topic=containers-ingress_annotation#rewrite-path) 어노테이션을 사용하십시오.

    4. 필요하면 리소스 구성 YAML을 편집하십시오. 편집기를 닫으면 변경사항이 저장되고 자동으로 적용됩니다.
        ```
        kubectl edit ingress <myingressresource>
        ```
        {: pre}

## 디버깅을 위해 DNS에서 ALB 제거
{: #one_alb}

특정 ALB IP를 통해 앱에 액세스할 수 없는 경우에는 해당 DNS 등록을 사용 중지하여 프로덕션에서 ALB를 임시로 제거할 수 있습니다. 그리고 ALB의 IP 주소를 사용하여 해당 ALB에서 디버깅 테스트를 실행할 수 있습니다.

예를 들어, 2개의 구역에 다중 구역 클러스터가 있으며 2개의 공용 ALB의 IP 주소가 `169.46.52.222` 및 `169.62.196.238`이라고 가정합니다. 상태 검사가 두 번째 구역의 ALB에 대해 정상 상태(healthy)를 리턴하지만, 이를 통해 앱에 직접 접속할 수 없습니다. 사용자는 디버깅을 위해 프로덕션에서 해당 ALB의 IP 주소 `169.62.196.238`을 제거하기로 결정합니다. 첫 번째 구역의 ALB IP `169.46.52.222`는 사용자의 도메인에 등록되어 있으며, 사용자가 두 번째 구역의 ALB를 디버깅하는 동안 트래픽을 계속 라우팅합니다.

1. 접속 불가능한 IP 주소가 있는 ALB의 이름을 가져오십시오.
    ```
    ibmcloud ks albs --cluster <cluster_name> | grep <ALB_IP>
    ```
    {: pre}

    예를 들어, 접속 불가능한 IP `169.62.196.238`은 ALB `public-cr24a9f2caf6554648836337d240064935-alb1`에 속합니다.
    ```
    ALB ID                                            Enabled   Status     Type      ALB IP           Zone    Build                          ALB VLAN ID
    public-cr24a9f2caf6554648836337d240064935-alb1    false     disabled   private   169.62.196.238   dal13   ingress:411/ingress-auth:315   2294021
    ```
    {: screen}

2. 이전 단계의 ALB 이름을 사용하여 ALB 팟(Pod)의 이름을 가져오십시오. 다음 명령에서는 이전 단계의 예제 ALB 이름을 사용합니다.
    ```
    kubectl get pods -n kube-system | grep public-cr24a9f2caf6554648836337d240064935-alb1
    ```
    {: pre}

    출력 예:
    ```
    public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq   2/2       Running   0          24m
    public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-trqxc   2/2       Running   0          24m
    ```
    {: screen}

3. 모든 ALB 팟(Pod)에 대해 실행되는 상태 검사를 사용 중지하십시오. 이전 단계에서 가져온 각각의 ALB 팟(Pod)에 대해 이러한 단계를 반복하십시오. 이러한 단계의 예제 명령과 출력에서는 첫 번째 팟(Pod) `public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq`를 사용합니다.
    1. ALB 팟(Pod)에 로그인하고 NGINX 구성 파일에서 `server_name` 행을 확인하십시오.
        ```
        kubectl exec -ti public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq -n kube-system -c nginx-ingress -- grep server_name /etc/nginx/conf.d/kube-system-alb-health.conf
        ```
        {: pre}

        ALB 팟(Pod)이 올바른 상태 검사 호스트 이름, `albhealth.<domain>`으로 구성되어 있음을 확인하는 예제:
        ```
        server_name albhealth.mycluster-12345.us-south.containers.appdomain.cloud;
        ```
        {: screen}

    2. 상태 검사를 사용 중지하여 IP를 제거하려면 `server_name` 앞에 `#`을 삽입하십시오. `albhealth.mycluster-12345.us-south.containers.appdomain.cloud` 가상 호스트가 ALB에 대해 사용되지 않는 경우에는 자동화된 상태 검사가 DNS 응답에서 IP를 자동으로 제거합니다.
        ```
        kubectl exec -ti public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq -n kube-system -c nginx-ingress -- sed -i -e 's*server_name*#server_name*g' /etc/nginx/conf.d/kube-system-alb-health.conf
        ```
        {: pre}

    3. 변경사항이 적용되었는지 확인하십시오.
        ```
        kubectl exec -ti public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq -n kube-system -c nginx-ingress -- grep server_name /etc/nginx/conf.d/kube-system-alb-health.conf
        ```
        {: pre}

        출력 예:
        ```
        #server_name albhealth.mycluster-12345.us-south.containers.appdomain.cloud
        ```
        {: screen}

    4. DNS 등록에서 IP를 제거하려면 NGINX 구성을 다시 로드하십시오.
        ```
        kubectl exec -ti public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq -n kube-system -c nginx-ingress -- nginx -s reload
        ```
        {: pre}

    5. 각 ALB 팟(Pod)에 대해 이러한 단계를 반복하십시오.

4. 이제 ALB IP의 상태를 검사하기 위해 `albhealth` 호스트에 대해 cURL 실행을 시도하면 검사에 실패합니다.
    ```
    curl -X GET http://169.62.196.238/ -H "Host: albhealth.mycluster-12345.us-south.containers.appdomain.cloud"
    ```
    {: pre}

    출력:
    ```
    <html>
        <head>
            <title>404 Not Found</title>
        </head>
        <body bgcolor="white"><center>
            <h1>404 Not Found</h1>
        </body>
    </html>
    ```
    {: screen}

5. Cloudflare 서버를 확인하여 ALB IP 주소가 사용자 도메인의 DNS 등록에서 제거되었는지 확인하십시오. 참고로, DNS 등록을 업데이트하려면 수 분이 소요될 수 있습니다.
    ```
    host mycluster-12345.us-south.containers.appdomain.cloud ada.ns.cloudflare.com
    ```
    {: pre}

    Example output that confirms that only the healthy ALB IP, `169.46.52.222`, remains in the DNS registration and that the unhealthy ALB IP, `169.62.196.238`, has been removed:
    ```
    mycluster-12345.us-south.containers.appdomain.cloud has address 169.46.52.222
    ```
    {: screen}

6. 이제 ALB IP가 프로덕션에서 제거되었으므로 이를 통해 앱에 대해 디버깅 테스트를 실행할 수 있습니다. 이 IP를 통해 앱에 대한 통신을 테스트하기 위해, 예제 값을 사용자 자신의 값으로 대체하여 다음 cURL 명령을 실행할 수 있습니다.
    ```
    curl -X GET --resolve mycluster-12345.us-south.containers.appdomain.cloud:443:169.62.196.238 https://mycluster-12345.us-south.containers.appdomain.cloud/
    ```
    {: pre}

    * 모든 항목이 올바르게 구성되어 있으면 앱으로부터 예상된 응답을 받습니다.
    * 응답에서 오류를 받는 경우, 이 특정 ALB에만 적용되는 구성이나 앱에 오류가 있을 수 있습니다. 앱 코드, [Ingress 리소스 구성 파일](/docs/containers?topic=containers-ingress#public_inside_4) 또는 이 ALB에만 적용된 기타 구성을 확인하십시오.

7. 디버깅이 완료되면 ALB 팟(Pod)에서 상태 검사를 복원하십시오. 각 ALB 팟(Pod)에 대해 이러한 단계를 반복하십시오.
  1. ALB 팟(Pod)에 로그인하고 `server_name`에서 `#`을 제거하십시오.
    ```
    kubectl exec -ti <pod_name> -n kube-system -c nginx-ingress -- sed -i -e 's*#server_name*server_name*g' /etc/nginx/conf.d/kube-system-alb-health.conf
    ```
    {: pre}

  2. 상태 검사 복원이 적용되도록 NGINX 구성을 다시 로드하십시오.
    ```
    kubectl exec -ti <pod_name> -n kube-system -c nginx-ingress -- nginx -s reload
    ```
    {: pre}

9. 이제 `albhealth` 호스트에 대해 cURL을 실행하여 ALB IP의 상태를 검사하면 검사 결과로서 `healthy`가 리턴됩니다.
    ```
    curl -X GET http://169.62.196.238/ -H "Host: albhealth.mycluster-12345.us-south.containers.appdomain.cloud"
    ```
    {: pre}

10. Cloudflare 서버를 확인하여 ALB IP 주소가 사용자 도메인의 DNS 등록에서 복원되었는지 확인하십시오. 참고로, DNS 등록을 업데이트하려면 수 분이 소요될 수 있습니다.
    ```
    host mycluster-12345.us-south.containers.appdomain.cloud ada.ns.cloudflare.com
    ```
    {: pre}

    출력 예:
    ```
    mycluster-12345.us-south.containers.appdomain.cloud has address 169.46.52.222
    mycluster-12345.us-south.containers.appdomain.cloud has address 169.62.196.238
    ```
    {: screen}

<br />


## 도움 및 지원 받기
{: #ingress_getting_help}

클러스터에 여전히 문제점이 있습니까?
{: shortdesc}

-  터미널에서 `ibmcloud` CLI 및 플러그인에 대한 업데이트가 사용 가능한 시점을 사용자에게 알려줍니다. 사용 가능한 모든 명령과 플래그를 사용할 수 있도록 반드시 CLI를 최신 상태로 유지하십시오.
-   {{site.data.keyword.Bluemix_notm}}가 사용 가능한지 확인하려면 [{{site.data.keyword.Bluemix_notm}} 상태 페이지 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")를 확인](https://cloud.ibm.com/status?selected=status)하십시오.
-   [{{site.data.keyword.containerlong_notm}} Slack ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://ibm-container-service.slack.com)에 질문을 게시하십시오.
    {{site.data.keyword.Bluemix_notm}} 계정에 대해 IBM ID를 사용 중이 아닌 경우에는 이 Slack에 대한 [초대를 요청](https://bxcs-slack-invite.mybluemix.net/)하십시오.
    {: tip}
-   포럼을 검토하여 다른 사용자에게도 동일한 문제가 발생하는지 여부를 확인하십시오. 포럼을 사용하여 질문을 할 때는 {{site.data.keyword.Bluemix_notm}} 개발 팀이 볼 수 있도록 질문에 태그를 지정하십시오.
    -   {{site.data.keyword.containerlong_notm}}로 클러스터 또는 앱을 개발하거나 배치하는 데 대한 기술적 질문이 있으면 [Stack Overflow![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://stackoverflow.com/questions/tagged/ibm-cloud+containers)에 질문을 게시하고 질문에 `ibm-cloud`, `kubernetes` 및 `containers` 태그를 지정하십시오.
    -   서비스 및 시작하기 지시사항에 대한 질문이 있으면 [IBM Developer Answers ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix) 포럼을 사용하십시오. `ibm-cloud` 및 `containers` 태그를 포함하십시오.
    포럼 사용에 대한 세부사항은 [도움 받기](/docs/get-support?topic=get-support-getting-customer-support#using-avatar)를 참조하십시오.
-   케이스를 열어 IBM 지원 센터에 문의하십시오. IBM 지원 케이스 열기 또는 지원 레벨 및 케이스 심각도에 대해 알아보려면 [지원 문의](/docs/get-support?topic=get-support-getting-customer-support)를 참조하십시오.
문제를 보고할 때 클러스터 ID를 포함하십시오. 클러스터 ID를 가져오려면 `ibmcloud ks clusters`를 실행하십시오. 또한 [{{site.data.keyword.containerlong_notm}} 진단 및 디버그 도구](/docs/containers?topic=containers-cs_troubleshoot#debug_utility)를 사용하여 IBM 지원 센터와 공유할 관련 정보를 클러스터에서 수집하고 내보낼 수도 있습니다.
{: tip}

