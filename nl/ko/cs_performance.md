---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-05"

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


# 성능 튜닝
{: #kernel}

특정 성능 최적화 요구사항이 있는 경우, {{site.data.keyword.containerlong}}의 일부 클러스터 컴포넌트에 대한 기본 설정을 변경할 수 있습니다.
{: shortdesc}

기본 설정을 변경하도록 선택한 경우 사용자가 직접 위험을 감수해야 합니다. 사용자 환경의 변경된 설정으로 인해 발생하는 잠재적 중단에 대해, 그리고 변경된 설정에 대한 테스트의 실행에 대해 사용자가 직접 책임을 져야 합니다.
{: important}

## 작업자 노드 성능 최적화
{: #worker}

특정 성능 최적화 요구사항이 있는 경우, 작업자 노드의 Linux 커널 `sysctl` 매개변수에 대한 기본 설정을 변경할 수 있습니다.
{: shortdesc}

작업자 노드는 최적화된 커널 성능으로 자동 프로비저닝되지만, 사용자 정의 [Kubernetes `DaemonSet` ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) 오브젝트를 클러스터에 적용하여 기본 설정을 변경할 수 있습니다. 디먼 세트는 모든 기존 작업자 노드의 설정을 변경하며 클러스터에서 프로비저닝되는 새 작업자 노드에 해당 설정을 적용합니다. 어떤 팟(Pod)도 영향을 받지 않습니다.

권한 부여된 샘플 `initContainer`를 실행하려면 모든 네임스페이스에 대해 [**관리자** {{site.data.keyword.Bluemix_notm}} IAM 서비스 역할](/docs/containers?topic=containers-users#platform)을 보유하고 있어야 합니다. 배치를 위한 컨테이너가 초기화되면 권한이 삭제됩니다.
{: note}

1. 이름이 `worker-node-kernel-settings.yaml`인 파일에 다음 디먼 세트를 저장하십시오. `spec.template.spec.initContainers` 섹션에서 튜닝하고자 하는 `sysctl` 매개변수의 필드와 값을 추가하십시오. 이 디먼 세트 예는 `net.core.somaxconn` 설정을 통해 환경에서 허용되는 기본 최대 연결 수를 변경하고, `net.ipv4.ip_local_port_range` 설정을 통해 임시 포트 범위를 변경합니다.
    ```
    apiVersion: apps/v1
    kind: DaemonSet
    metadata:
      name: kernel-optimization
      namespace: kube-system
      labels:
        tier: management
        app: kernel-optimization
    spec:
      selector:
        matchLabels:
          name: kernel-optimization
      template:
        metadata:
          labels:
            name: kernel-optimization
        spec:
          hostNetwork: true
          hostPID: true
          hostIPC: true
          initContainers:
            - command:
                - sh
                - -c
                - sysctl -w net.ipv4.ip_local_port_range="1025 65535"; sysctl -w net.core.somaxconn=32768;
              image: alpine:3.6
              imagePullPolicy: IfNotPresent
              name: sysctl
              resources: {}
              securityContext:
                privileged: true
                capabilities:
                  add:
                    - NET_ADMIN
              volumeMounts:
                - name: modifysys
                  mountPath: /sys
          containers:
            - resources:
                requests:
                  cpu: 0.01
              image: alpine:3.6
              name: sleepforever
              command: ["/bin/sh", "-c"]
              args:
                - >
                  while true; do
                    sleep 100000;
                  done
          volumes:
            - name: modifysys
              hostPath:
                path: /sys
    ```
    {: codeblock}

2. 작업자 노드에 디먼 세트를 적용하십시오. 변경사항은 즉시 적용됩니다.
    ```
    kubectl apply -f worker-node-kernel-settings.yaml
    ```
    {: pre}

<br />

{{site.data.keyword.containerlong_notm}}에서 설정한 기본값으로 작업자 노드의 `sysctl` 매개변수를 되돌리려면 다음을 수행하십시오.

1. 디먼 세트를 삭제하십시오. 사용자 정의 설정을 적용한 `initContainers`가 제거됩니다.
    ```
    kubectl delete ds kernel-optimization
    ```
    {: pre}

2. [클러스터의 모든 작업자 노드를 재부팅](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reboot)하십시오. 작업자 노드가 기본값이 적용되어 다시 온라인 상태가 됩니다.

<br />


## 팟(Pod) 성능 최적화
{: #pod}

특정 성능 워크로드 요구가 있는 경우 팟(Pod) 네트워크 네임스페이스의 Linux 커널 `sysctl` 매개변수에 대한 기본 설정을 변경할 수 있습니다.
{: shortdesc}

앱 팟(Pod)의 커널 설정을 최적화하기 위해 배치마다 [`initContainer ` ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/) 패치를 `pod/ds/rs/deployment` YAML에 삽입할 수 있습니다. `initContainer`는 해당 성능의 최적화를 원하는 팟(Pod) 네트워크 네임스페이스에 있는 각 앱 배치에 추가됩니다.

시작하기 전에, 권한 부여된 샘플 `initContainer`를 실행하려면 모든 네임스페이스에 대해 [**관리자** {{site.data.keyword.Bluemix_notm}} IAM 서비스 역할](/docs/containers?topic=containers-users#platform)을 보유하고 있는지 확인하십시오. 배치를 위한 컨테이너가 초기화되면 권한이 삭제됩니다.

1. 이름이 `pod-patch.yaml`인 파일에 다음의 `initContainer` 패치를 저장하고 튜닝할 `sysctl` 매개변수의 필드와 값을 추가하십시오. 이 `initContainer` 예는 `net.core.somaxconn` 설정을 통해 환경에서 허용되는 기본 최대 연결 수를 변경하고, `net.ipv4.ip_local_port_range` 설정을 통해 임시 포트 범위를 변경합니다.
    ```
    spec:
      template:
        spec:
          initContainers:
          - command:
            - sh
            - -c
            - sysctl -e -w net.core.somaxconn=32768;  sysctl -e -w net.ipv4.ip_local_port_range="1025 65535";
            image: alpine:3.6
            imagePullPolicy: IfNotPresent
            name: sysctl
            resources: {}
            securityContext:
              privileged: true
    ```
    {: codeblock}

2. 각 배치를 패치하십시오.
    ```
    kubectl patch deployment <deployment_name> --patch pod-patch.yaml
    ```
    {: pre}

3. 커널 설정에서 `net.core.somaxconn` 값을 변경한 경우에는 대부분의 앱이 업데이트된 값을 자동으로 사용할 수 있습니다. 그러나 일부 앱에서는 커널 값과 일치하도록 앱 코드의 대응되는 값을 수동으로 변경해야 할 수도 있습니다. 예를 들어, NGINX 앱이 실행 중인 팟(Pod)의 성능을 튜닝 중인 경우에는 일치시킬 NGINX 앱 코드의 `backlog` 필드 값을 변경해야 합니다. 자세한 정보는 이 [NGINX 블로그 게시물 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.nginx.com/blog/tuning-nginx/)을 참조하십시오.

<br />


## 클러스터 메트릭 제공자 리소스 조정
{: #metrics}

클러스터 메트릭 제공자(Kubernetes 1.12 이상의 경우 `metrics-server`, 이전 버전의 경우 `heapster`) 구성은 작업자 노드당 팟(Pod)이 30개 이하인 클러스터에 맞게 최적화되었습니다. 클러스터에서 작업자 노드당 팟(Pod) 수가 이를 초과할 경우, 팟(Pod)에 대한 메트릭 제공자 `metrics-server` 또는 `heapster` 기본 컨테이너가 `OOMKilled`와 같은 오류 메시지를 리턴하면서 자주 다시 시작될 수 있습니다.

메트릭 제공자 팟(Pod)에는 또한 `nanny` 컨테이너도 있는데, 이 컨테이너는 클러스터의 작업자 노드 수에 대한 응답으로 `metrics-server` 또는 `heapster` 기본 컨테이너의 리소스 요청 및 한계를 스케일링합니다. 메트릭 제공자의 configmap을 편집하여 기본 리소스를 변경할 수 있습니다.

시작하기 전에: [계정에 로그인하십시오. 해당되는 경우, 적절한 리소스 그룹을 대상으로 지정하십시오. 클러스터의 컨텍스트를 설정하십시오.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  클러스터 메트릭 제공자 configmap YAML을 여십시오.
    *  `metrics-server`의 경우:
       ```
       kubectl get configmap metrics-server-config -n kube-system -o yaml
       ```
       {: pre}
    *  `heapster`의 경우:
       ```
       kubectl get configmap heapster-config -n kube-system -o yaml
       ```
       {: pre}
    출력 예:
    ```
    apiVersion: v1
    data:
      NannyConfiguration: |-
        apiVersion: nannyconfig/v1alpha1
        kind: NannyConfiguration
    kind: ConfigMap
    metadata:
      annotations:
        armada-service: cruiser-kube-addons
        version: --
      creationTimestamp: 2018-10-09T20:15:32Z
      labels:
        addonmanager.kubernetes.io/mode: EnsureExists
        kubernetes.io/cluster-service: "true"
      name: heapster-config
      namespace: kube-system
      resourceVersion: "526"
      selfLink: /api/v1/namespaces/kube-system/configmaps/heapster-config
      uid: 11a1aaaa-bb22-33c3-4444-5e55e555e555
    ```
    {: screen}

2.  `data.NannyConfiguration` 섹션의 configmap에 `memoryPerNode` 필드를 추가하십시오. `metrics-server` 및 `heapster`에 대한 기본값은 둘 다 `4Mi`로 설정됩니다.
    ```
    apiVersion: v1
    data:
      NannyConfiguration: |-
        apiVersion: nannyconfig/v1alpha1
        kind: NannyConfiguration
        memoryPerNode: 5Mi
    kind: ConfigMap
    ...
    ```
    {: codeblock}

3.  변경사항을 적용하십시오.
    ```
    kubectl apply -f heapster-config.yaml
    ```
    {: pre}

4.  `OOMKilled` 오류 메시지로 인해 컨테이너가 계속 다시 시작되는지 메트릭 제공자 팟(Pod)을 모니터링하십시오. 그럴 경우 이러한 단계를 반복하여 팟(Pod)이 안정될 때까지 `memoryPerNode` 크기를 늘리십시오.

더 많은 설정을 튜닝하고 싶으십니까? 자세한 내용은 [Kubernetes 추가 기능 resizer 구성 문서 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/autoscaler/tree/master/addon-resizer#addon-resizer-configuration)를 확인하십시오.
{: tip}
