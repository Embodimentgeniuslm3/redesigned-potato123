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


# 클러스터 DNS 제공자 구성
{: #cluster_dns}

{{site.data.keyword.containerlong}} 클러스터에 있는 각 서비스에는 DNS(Domain Name System) 이름이 지정되는데, 이 이름은 클러스터 DNS 제공자가 DNS 요청을 분석하기 위해 등록하는 이름입니다. 클러스터의 Kubernetes 버전에 따라 Kubernetes DNS(KubeDNS) 또는 [CoreDNS ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://coredns.io/) 중에서 선택할 수 있습니다. 서비스 및 팟(Pod)의 DNS에 대한 자세한 정보는 [Kubernetes 문서![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/services-networking/dns-pod-service/)를 참조하십시오.
{: shortdesc}

**어떤 Kubernetes 버전이 어떤 클러스터 DNS 제공자를 지원합니까?**<br>

| Kubernetes 버전 | 새 클러스터에 대한 기본값 |설명 |
|---|---|---|
| 1.14 이상 |CoreDNS | 클러스터가 KubeDNS를 사용하며 이전 버전에서 버전 1.14 이상으로 업데이트된 경우에는 클러스터 업데이트 중에 클러스터 DNS 제공자가 KubeDNS에서 CoreDNS로 자동으로 마이그레이션됩니다. 클러스터 DNS 제공자를 다시 KubeDNS로 전환할 수는 없습니다. |
| 1.13 |CoreDNS | 이전 클러스터에서 1.13으로 업데이트된 클러스터는 업데이트 시점에 사용하던 DNS 제공자를 유지합니다. 다른 DNS 제공자를 사용하려면 [DNS 제공자를 전환](#dns_set)하십시오. |
| 1.12 |KubeDNS | 대신 CoreDNS를 사용하려면 [DNS 제공자를 전환](#set_coredns)하십시오. |
| 1.11 이하 |KubeDNS | DNS 제공자를 CoreDNS로 전환할 수 없습니다. |
{: caption="Kubernetes 버전별 기본 클러스터 DNS 제공자" caption-side="top"}

**KubeDNS 대신 CoreDNS를 사용하면 어떤 이점이 있습니까?**<br>
CoreDNS는 Kubernetes 버전 1.13 이상에서 기본 지원하는 클러스터 DNS 제공자이며 최근에 [Cloud Native Computing Foundation(CNCF)의 졸업 프로젝트 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.cncf.io/projects/)가 되었습니다. 졸업 프로젝트는 철저히 테스트되고 개선되어 프로덕션 레벨로 대규모 전환될 준비가 된 프로젝트입니다. 

이 [Kubernetes 공지사항 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/blog/2018/12/03/kubernetes-1-13-release-announcement/)에 언급된 바와 같이 CoreDNS는 Kubertenes와의, 하위 호환과 확장이 동시에 가능한 통합을 제공하는 권한 있는 범용 DNS 서버입니다. CoreDNS는 단일 실행 파일이며 단일 프로세스이므로 이전 DNS 제공자보다 문제를 일으킬 수 있는 종속 항목과 작동 부분이 더 적습니다. 또한 이 프로젝트는 Kubernetes 프로젝트와 동일한 `Go`(메모리를 보호하는 데 도움을 줌)로 작성되었습니다. 마지막으로, 사용자가 [CoreDNS 문서의 일반적인 설정 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://coredns.io/manual/toc/#setups)과 같은 사용자 정의 DNS 항목을 작성할 수 있으므로 CoreDNS는 KubeDNS보다 더 유연한 유스 케이스를 지원합니다. 

## 클러스터 DNS 제공자 Auto-Scaling
{: #dns_autoscale}

기본적으로 {{site.data.keyword.containerlong_notm}} 클러스터 DNS 제공자에는 클러스터의 작업자 노드와 코어의 수에 따라 DNS 팟(Pod)을 오토스케일링하는 배치가 포함되어 있습니다. DNS 자동 스케일링 기능 ConfigMap을 편집하여 DNS 오토스케일러 매개변수를 세부 조정할 수 있습니다. 예를 들어, 앱이 클러스터 DNS 제공자를 많이 사용하는 경우에는 앱을 지원하기 위해 최소 DNS 팟(Pod) 수를 늘려야 할 수 있습니다. 자세한 정보는 [Kubernetes 문서 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/tasks/administer-cluster/dns-horizontal-autoscaling/)를 참조하십시오.
{: shortdesc}

시작하기 전에: [계정에 로그인하십시오. 해당되는 경우, 적절한 리소스 그룹을 대상으로 지정하십시오. 클러스터의 컨텍스트를 설정하십시오.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  클러스터 DNS 제공자 배치가 사용 가능한지 확인하십시오. 클러스터에 설치된 KubeDNS, CoreDNS, 또는 둘 다에 대한 오토스케일러가 있을 수 있습니다. 두 DNS 오토스케일러가 모두 설치되어 있는 경우에는 CLI 출력의 **AVAILABLE** 열을 확인하여 사용 중인 오토스케일러를 찾으십시오. 사용 중인 배치가 사용 가능한 하나의 배치와 함께 나열됩니다.
    ```
    kubectl get deployment -n kube-system | grep dns-autoscaler
    ```
    {: pre}

    출력 예:
    ```
    NAME                   DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
    coredns-autoscaler     0         0         0            0           32d
    kube-dns-autoscaler    1         1         1            1           260d
    ```
    {: screen}
2.  DNS 오토스케일러 매개변수에 대한 configmap 이름을 가져오십시오.
    ```
    kubectl get configmap -n kube-system
    ```
    {: pre}    
3.  DNS 오토스케일러에 대한 기본 설정을 편집하십시오. `data.linear` 필드를 찾으십시오. 이 필드는 16개의 작업자 노드 또는 256개의 코어당 1개의 DNS 팟(Pod), 클러스터 크기에 관계없이 최소 2개의 DNS 팟(Pod)으로 기본 설정됩니다(`preventSinglePointFailure: true`). 자세한 정보는 [Kubernetes 문서 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/tasks/administer-cluster/dns-horizontal-autoscaling/#tuning-autoscaling-parameters)를 참조하십시오.
    ```
    kubectl edit configmap -n kube-system <dns-autoscaler>
    ```
    {: pre}

    출력 예:
    ```
    apiVersion: v1
    data:
      linear: '{"coresPerReplica":256,"nodesPerReplica":16,"preventSinglePointFailure":true}'
    kind: ConfigMap
    metadata:
    ...
    ```
    {: screen}

## 클러스터 DNS 제공자 사용자 정의
{: #dns_customize}

DNS configmap을 편집하여 {{site.data.keyword.containerlong_notm}} 클러스터 DNS 공급자를 사용자 정의할 수 있습니다. 예를 들어, 외부 호스트를 가리키는 서비스를 분석하기 위해 `stubdomains` 및 업스트림 이름 서버를 구성할 수 있습니다. 또한 CoreDNS를 사용하는 경우, 여러 개의 [Corefile ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://coredns.io/2017/07/23/corefile-explained/)을 CoreDNS configmap 내에 구성할 수 있습니다. 자세한 정보는 [Kubernetes 문서 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/tasks/administer-cluster/dns-custom-nameservers/)를 참조하십시오.
{: shortdesc}

시작하기 전에: [계정에 로그인하십시오. 해당되는 경우, 적절한 리소스 그룹을 대상으로 지정하십시오. 클러스터의 컨텍스트를 설정하십시오.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  클러스터 DNS 제공자 배치가 사용 가능한지 확인하십시오. 클러스터에 설치된 KubeDNS, CoreDNS, 또는 둘 다에 대한 DNS 클러스터 제공자가 있을 수 있습니다. 두 DNS 제공자가 모두 설치되어 있는 경우에는 CLI 출력의 **AVAILABLE** 열을 확인하여 사용 중인 DNS 제공자를 찾으십시오. 사용 중인 배치가 사용 가능한 하나의 배치와 함께 나열됩니다.
    ```
    kubectl get deployment -n kube-system -l k8s-app=kube-dns
    ```
    {: pre}

    출력 예:
    ```
    NAME                   DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
    coredns                0         0         0            0           89d
    kube-dns-amd64         2         2         2            2           89d
    ```
    {: screen}
2.  CoreDNS 또는 KubeDNS configmap에 대한 기본 설정을 편집하십시오.

    *   **CoreDNS의 경우**: ConfigMap의 `data` 섹션에 있는 Corefile을 사용하여 `stubdomains` 및 업스트림 이름 서버를 사용자 정의하십시오. 자세한 정보는 [Kubernetes 문서 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/tasks/administer-cluster/dns-custom-nameservers/#coredns)를 참조하십시오.
        ```
        kubectl edit configmap -n kube-system coredns
        ```
        {: pre}

        **CoreDNS 출력 예**:
          ```
          apiVersion: v1
          kind: ConfigMap
          metadata:
            name: coredns
            namespace: kube-system
          data:
            Corefile: |
              abc.com:53 {
                  errors
                  cache 30
                  proxy . 1.2.3.4
              }
              .:53 {
                  errors
                  health
                  kubernetes cluster.local in-addr.arpa ip6.arpa {
                     pods insecure
                     upstream 172.16.0.1
                     fallthrough in-addr.arpa ip6.arpa
                  }
                  prometheus :9153
                  proxy . /etc/resolv.conf
                  cache 30
                  loop
                  reload
                  loadbalance
              }
          ```
          {: screen}

          구성할 사용자 정의 사항이 많으십니까? Kubernetes 버전 1.12.6_1543 이상에서는 복수의 Corefiles을 CoreDNS configmap에 추가할 수 있습니다. 자세한 정보는 [Corefile 가져오기 문서 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://coredns.io/plugins/import/)를 참조하십시오.
          {: tip}

    *   **KubeDNS의 경우**: ConfigMap의 `data` 섹션에서 `stubdomains` 및 업스트림 이름 서버를 구성하십시오. 자세한 정보는 [Kubernetes 문서 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/tasks/administer-cluster/dns-custom-nameservers/#kube-dns)를 참조하십시오.
        ```
        kubectl edit configmap -n kube-system kube-dns
        ```
        {: pre}

        **KubeDNS 출력 예**:
        ```
        apiVersion: v1
        kind: ConfigMap
        metadata:
          name: kube-dns
          namespace: kube-system
        data:
          stubDomains: |
          {"abc.com": ["1.2.3.4"]}
        ```
        {: screen}

## 클러스터 DNS 제공자를 CoreDNS 또는 KubeDNS로 설정
{: #dns_set}

Kubernetes 버전 1.12 또는 1.13을 실행하는 {{site.data.keyword.containerlong_notm}} 클러스터가 있는 경우에는 Kubernetes DNS(KubeDNS) 또는 CoreDNS를 클러스터 DNS 제공자로 사용하도록 선택할 수 있습니다.
{: shortdesc}

그 외 Kubernetes 버전을 실행하는 클러스터는 클러스터 DNS 제공자를 설정할 수 없습니다. 버전 1.11 이하는 KubeDNS만 지원하며 버전 1.14 이상은 CoreDNS만 지원합니다.
{: note}

**시작하기 전에**:
1.  [계정에 로그인하십시오. 해당되는 경우, 적절한 리소스 그룹을 대상으로 지정하십시오. 클러스터의 컨텍스트를 설정하십시오.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
2.  현재 클러스터 DNS 제공자를 판별하십시오. 다음 예에서 현재 클러스터 DNS 제공자는 KubeDNS입니다.
    ```
    kubectl cluster-info
    ```
    {: pre}

    출력 예:
    ```
    ...
    KubeDNS is running at https://c2.us-south.containers.cloud.ibm.com:20190/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
    ...
    ```
    {: screen}
3.  클러스터에서 사용하는 DNS 제공자에 따라 다음 단계를 수행하여 DNS 제공자를 전환하십시오.
    *  [CoreDNS 사용으로 전환](#set_coredns)하십시오.
    *  [KubeDNS 사용으로 전환](#set_kubedns)하십시오.

### CoreDNS를 클러스터 DNS 제공자로 설정
{: #set_coredns}

KubeDNS 대신 CoreDNS를 클러스터 DNS 제공자로 설정하십시오.
{: shortdesc}

1.  KubeDNS 제공자 configmap 또는 KubeDNS 오토스케일러 configmap을 사용자 정의한 경우, 모든 사용자 정의 항목을 CoreDNS configmap으로 전송하십시오.
    *   `kube-system` 네임스페이스의 `kube-dns` configmap에 대해서는 모든 [DNS 사용자 정의 항목 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/tasks/administer-cluster/dns-custom-nameservers/)을 `kube-system` 네임스페이스의 `coredns` configmap으로 전송하십시오. 구문은 `kube-dns` 및 `coredns` ConfigMap에서 서로 다릅니다. 예제는 [Kubernetes 문서 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/tasks/administer-cluster/dns-custom-nameservers/#coredns-configuration-equivalent-to-kube-dns)를 참조하십시오.
    *   `kube-system` 네임스페이스의 `kube-dns-autoscaler` configmap에 대해서는 모든 [DNS 오토스케일러 사용자 정의 항목 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/tasks/administer-cluster/dns-horizontal-autoscaling/)을 `kube-system` 네임스페이스의 `coredns-autoscaler` configmap으로 전송하십시오. 사용자 정의 구문은 둘 모두에서 동일합니다. 
2.  KubeDNS Autoscaler 배치를 스케일링 다운하십시오.
    ```
    kubectl scale deployment -n kube-system --replicas=0 kube-dns-autoscaler
    ```
    {: pre}
3.  팟(Pod)을 확인하고 팟(Pod)이 삭제될 때까지 기다리십시오.
    ```
        kubectl get pods -n kube-system -l k8s-app=kube-dns-autoscaler
    ```
    {: pre}
4.  KubeDNS 배치를 스케일링 다운하십시오.
    ```
    kubectl scale deployment -n kube-system --replicas=0 kube-dns-amd64
    ```
    {: pre}
5.  CoreDNS Autoscaler 배치를 스케일링 업하십시오.
    ```
        kubectl scale deployment -n kube-system --replicas=1 coredns-autoscaler
    ```
    {: pre}
6.  CoreDNS의 클러스터 DNS 서비스에 레이블을 지정하고 어노테이션을 작성하십시오.
    ```
    kubectl label service --overwrite -n kube-system kube-dns kubernetes.io/name=CoreDNS
    ```
    {: pre}
    ```
    kubectl annotate service --overwrite -n kube-system kube-dns prometheus.io/port=9153
    ```
    {: pre}
    ```
    kubectl annotate service --overwrite -n kube-system kube-dns prometheus.io/scrape=true
    ```
    {: pre}
7.  **선택사항**: CoreDNS 팟(Pod)에서 메트릭을 수집하기 위해 Prometheus를 사용하려면 전환 중인 `kube-dns` 서비스에 메트릭 포트를 추가해야 합니다.
    ```
    kubectl -n kube-system patch svc kube-dns --patch '{"spec": {"ports": [{"name":"metrics","port":9153,"protocol":"TCP"}]}}' --type strategic
    ```
    {: pre}



### KubeDNS를 클러스터 DNS 제공자로 설정
{: #set_kubedns}

CoreDNS 대신 KubeDNS를 클러스터 DNS 제공자로 설정하십시오.
{: shortdesc}

1.  CoreDNS 제공자 configmap 또는 CoreDNS 오토스케일러 configmap을 사용자 정의한 경우, 모든 사용자 정의 항목을 KubeDNS configmap으로 전송하십시오.
    *   `kube-system` 네임스페이스의 `coredns` configmap의 경우 모든 [DNS 사용자 정의 항목 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/tasks/administer-cluster/dns-custom-nameservers/)을 `kube-system` 네임스페이스의 `kube-dns` configmap으로 전송하십시오. 구문은 `kube-dns` 및 `coredns` ConfigMap에서 서로 다릅니다. 예제는 [Kubernetes 문서 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/tasks/administer-cluster/dns-custom-nameservers/#coredns-configuration-equivalent-to-kube-dns)를 참조하십시오.
    *   `kube-system` 네임스페이스의 `coredns-autoscaler` configmap의 경우 모든 [DNS 오토스케일러 사용자 정의 항목 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/tasks/administer-cluster/dns-horizontal-autoscaling/)을 `kube-system` 네임스페이스의 `kube-dns-autoscaler` configmap으로 전송하십시오. 사용자 정의 구문은 둘 모두에서 동일합니다. 
2.  CoreDNS Autoscaler 배치를 스케일링 다운하십시오.
    ```
    kubectl scale deployment -n kube-system --replicas=0 coredns-autoscaler
    ```
    {: pre}
3.  팟(Pod)을 확인하고 팟(Pod)이 삭제될 때까지 기다리십시오.
    ```
    kubectl get pods -n kube-system -l k8s-app=coredns-autoscaler
    ```
    {: pre}
4.  CoreDNS 배치를 스케일링 다운하십시오.
    ```
    kubectl scale deployment -n kube-system --replicas=0 coredns
    ```
    {: pre}
5.  KubeDNS Autoscaler 배치를 스케일링 업하십시오.
    ```
    kubectl scale deployment -n kube-system --replicas=1 kube-dns-autoscaler
    ```
    {: pre}
6.  CoKubeDNS의 클러스터 DNS 서비스에 레이블을 지정하고 어노테이션을 작성하십시오.
    ```
    kubectl label service --overwrite -n kube-system kube-dns kubernetes.io/name=KubeDNS
    ```
    {: pre}
    ```
    kubectl annotate service --overwrite -n kube-system kube-dns prometheus.io/port-
    ```
    {: pre}
    ```
    kubectl annotate service --overwrite -n kube-system kube-dns prometheus.io/scrape-
    ```
    {: pre}
7.  **선택사항**: CoreDNS 팟(Pod)에서 메트릭을 수집하기 위해 Prometheus를 사용한 경우 `kube-dns` 서비스에는 메트릭 포트가 있습니다. 그러나 KubeDNS는 서비스에서 포트를 제거할 수 있도록 이 메트릭 포트를 포함할 필요가 없습니다.
    ```
    kubectl -n kube-system patch svc kube-dns --patch '{"spec": {"ports": [{"name":"dns","port":53,"protocol":"UDP"},{"name":"dns-tcp","port":53,"protocol":"TCP"}]}}' --type merge
    ```
    {: pre}
