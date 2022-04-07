---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-11"

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


# 네트워크 트래픽을 에지 작업자 노드로 제한
{: #edge}

{{site.data.keyword.containerlong}}에서 에지 작업자 노드는 외부에서 적은 수의 작업자 노드에 액세스할 수 있게 하고 네트워크 워크로드를 격리함으로써 Kubernetes 클러스터의 보안을 향상시킬 수 있습니다.
{:shortdesc}

이러한 작업자 노드가 네트워킹 전용으로 표시되는 경우 다른 워크로드는 작업자 노드의 CPU 또는 메모리를 이용할 수 없고 네트워킹을 방해할 수도 없습니다.

다중 구역 클러스터가 있으며 네트워크 트래픽을 에지 작업자 노드로 제한하려는 경우에는 로드 밸런서 또는 Ingress 팟(Pod)의 고가용성을 위해 각 구역에서 두 개 이상의 에지 작업자 노드가 사용으로 설정되어야 합니다. 클러스터의 모든 구역을 포함하는 에지 노드 작업자 풀을 작성하고 구역당 두 개 이상의 작업자 노드를 두십시오.
{: tip}

## 에지 노드로 작업자 노드에 레이블 지정
{: #edge_nodes}

`dedicated=edge` 레이블을 클러스터의 각 공용 또는 사설 VLAN에 있는 둘 이상의 작업자 노드에 추가하여 네트워크 로드 밸런서(NLB) 및 Ingress 애플리케이션 로드 밸런서(ALB)가 해당 작업자 노드에만 배치되도록 하십시오.
{:shortdesc}

Kubernetes 1.14 이상에서는 에지 작업자 노드에 공용 및 사설 NLB와 ALB를 모두 배치할 수 있습니다. Kubernetes 1.13 이하에서는 공용 및 사설 ALB와 공용 NLB는 에지 노드에 배치할 수 있지만, 사설 NLB는 클러스터에서 에지 노드가 아닌 노드에만 배치해야 합니다.
{: note}

시작하기 전에:

* 다음 [{{site.data.keyword.Bluemix_notm}} IAM 역할](/docs/containers?topic=containers-users#platform)을 보유하고 있는지 확인하십시오. 
  * 클러스터에 대한 임의의 플랫폼 역할
  * 모든 네임스페이스에 대한 **작성자** 또는 **관리자** 서비스 역할
* [계정에 로그인하십시오. 해당되는 경우, 적절한 리소스 그룹을 대상으로 지정하십시오. 클러스터의 컨텍스트를 설정하십시오.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

</br>작업자 노드를 에지 노드로 레이블 지정하려면 다음을 수행하십시오. 

1. 클러스터의 모든 구역을 포함하며 구역당 둘 이상의 작업자가 있는 [새 작업자 풀](/docs/containers?topic=containers-add_workers#add_pool)을 작성하십시오. `ibmcloud ks worker-pool-create` 명령에 `--labels dedicated=edge` 플래그를 포함시켜 풀에 있는 모든 작업자 노드에 레이블을 지정하십시오. 나중에 추가되는 작업자 노드를 포함, 이 풀에 있는 모든 작업자 노드는 에지 노드로 레이블 지정됩니다. 
  <p class="tip">기존 작업자 풀을 사용하려는 경우에는 해당 풀이 클러스터의 모든 구역을 포함해야 하며 구역당 둘 이상의 작업자 노드가 있어야 합니다. [PATCH 작업자 풀 API](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/PatchWorkerPool)를 사용하여 작업자 풀을 `dedicated=edge`로 레이블 지정할 수 있습니다. 요청의 본문에서 다음 JSON을 전달하십시오. 작업자 풀이 `dedicated=edge`로 표시되고 나면 모든 기존 및 후속 작업자 노드에 이 레이블이 지정되며 Ingress 및 로드 밸런서가 에지 작업자 노드에 배치됩니다.
      <pre class="screen">
      {
        "labels": {"dedicated":"edge"},
        "state": "labels"
      }</pre></p>

2. 작업자 풀 및 작업자 노드에 `dedicated=edge` 레이블이 있는지 확인하십시오. 
  * 작업자 풀을 확인하려면 다음 명령을 실행하십시오.
    ```
    ibmcloud ks worker-pool-get --cluster <cluster_name_or_ID> --worker-pool <worker_pool_name_or_ID>
    ```
    {: pre}

  * 개별 작업자 노드를 확인하려면 다음 명령의 출력에서 **Labels** 필드를 검토하십시오.
    ```
    kubectl describe node <worker_node_private_IP>
    ```
    {: pre}

3. 클러스터에 있는 모든 기존 NLB 및 ALB를 검색하십시오. 
  ```
  kubectl get services --all-namespaces | grep LoadBalancer
  ```
  {: pre}

  출력에서 각 로드 밸런서 서비스의 **NAMESPACE** 및 **NAME**을 기록해 두십시오. 예를 들어, 다음 출력에는 네 개의 로드 밸런서 서비스가 있습니다(`default` 네임스페이스에 하나의 공용 NLB, `kube-system` 네임스페이스에 하나의 사설 및 두 개의 공용 ALB). 
  ```
  NAMESPACE     NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP     PORT(S)                                     AGE
  default       webserver-lb                                     LoadBalancer   172.21.190.18    169.46.17.2     80:30597/TCP                                10m
  kube-system   private-crdf253b6025d64944ab99ed63bb4567b6-alb1  LoadBalancer   172.21.158.78    10.185.94.150   80:31015/TCP,443:31401/TCP,9443:32352/TCP   25d
  kube-system   public-crdf253b6025d64944ab99ed63bb4567b6-alb1   LoadBalancer   172.21.84.248    169.48.228.78   80:30286/TCP,443:31363/TCP                  1h
  kube-system   public-crdf253b6025d64944ab99ed63bb4567b6-alb2   LoadBalancer   172.21.229.73    169.46.17.6     80:31104/TCP,443:31138/TCP                  57m
  ```
  {: screen}

4. 이전 단계의 출력을 사용하여, 각 NLB 및 ALB에 대해 다음 명령을 실행하십시오. 이 명령은 NLB 또는 ALB를 에지 작업자 노드에 다시 배치합니다. 

  클러스터가 Kubernetes 1.14 이상을 실행하는 경우에는 공용 및 사설 NLB와 ALB를 모두 에지 작업자 노드에 배치할 수 있습니다. Kubernetes 1.13 이하에서는 공용 및 사설 ALB와 공용 NLB만 에지 노드에 배치할 수 있으므로 사설 NLB를 다시 배치하지 마십시오.
  {: note}

  ```
  kubectl get service -n <namespace> <service_name> -o yaml | kubectl apply -f -
  ```
  {: pre}

  출력 예:
  ```
  service "webserver-lb" configured
  ```
  {: screen}

5. 네트워킹 워크로드가 에지 노드로 제한되는지 확인하려면 NLB 및 ALB 팟(Pod)이 에지 노드에 스케줄되며 에지 노드가 아닌 노드에는 스케줄되지 않음을 확인하십시오. 

  * NLB 팟(Pod)의 경우:
    1. NLB 팟(Pod)이 에지 노드에 배치되었는지 확인하십시오. 3단계의 출력에 나열된 로드 밸런서 서비스의 외부 IP 주소를 검색하십시오. 마침표(`.`)를 하이픈(`-`)으로 대체하십시오. 외부 IP 주소가 `169.46.17.2`인 `webserver-lb` NLB에 대한 예는 다음과 같습니다.
      ```
      kubectl describe nodes -l dedicated=edge | grep "169-46-17-2"
      ```
      {: pre}

      출력 예:
      ```
      ibm-system                 ibm-cloud-provider-ip-169-46-17-2-76fcb4965d-wz6dg                 5m (0%)       0 (0%)      10Mi (0%)        0 (0%)
      ibm-system                 ibm-cloud-provider-ip-169-46-17-2-76fcb4965d-2z64r                 5m (0%)       0 (0%)      10Mi (0%)        0 (0%)
      ```
      {: screen}
    2. 에지 노드가 아닌 노드에 배치된 NLB 팟(Pod)이 없는 것을 확인하십시오. 외부 IP 주소가 `169.46.17.2`인 `webserver-lb` NLB에 대한 예는 다음과 같습니다.
      ```
      kubectl describe nodes -l dedicated!=edge | grep "169-46-17-2"
      ```
      {: pre}
      * NLB 팟(Pod)이 올바르게 에지 노드에 배치된 경우에는 NLB 팟(Pod)이 리턴되지 않습니다. 이 경우에는 NLB가 성공적으로 에지 작업자 노드에만 다시 스케줄된 것입니다. 
      * NLB 팟(Pod)이 리턴된 경우에는 다음 단계로 진행하십시오. 

  * ALB 팟(Pod)의 경우:
    1. 모든 ALB 팟(Pod)이 에지 노드에 배치되었는지 확인하십시오. 키워드 `alb`를 검색하십시오. 각 공용 및 사설 ALB에는 두 개의 팟(Pod)이 있습니다. 예:
      ```
      kubectl describe nodes -l dedicated=edge | grep alb
      ```
      {: pre}

      하나의 기본 사설 ALB와 두 개의 기본 공용 ALB가 사용으로 설정된 두 구역이 있는 클러스터에 대한 출력 예:
      ```
      kube-system                private-crdf253b6025d64944ab99ed63bb4567b6-alb1-d5dd478db-27pv4    0 (0%)        0 (0%)      0 (0%)           0 (0%)
      kube-system                private-crdf253b6025d64944ab99ed63bb4567b6-alb1-d5dd478db-7p9q6    0 (0%)        0 (0%)      0 (0%)           0 (0%)
      kube-system                public-crdf253b6025d64944ab99ed63bb4567b6-alb1-5ff8cdff89-s77z6    0 (0%)        0 (0%)      0 (0%)           0 (0%)
      kube-system                public-crdf253b6025d64944ab99ed63bb4567b6-alb1-5ff8cdff89-kvs9f    0 (0%)        0 (0%)      0 (0%)           0 (0%)
      kube-system                public-crdf253b6025d64944ab99ed63bb4567b6-alb2-57df7c7b5b-tp6xw    0 (0%)        0 (0%)      0 (0%)           0 (0%)
      kube-system                public-crdf253b6025d64944ab99ed63bb4567b6-alb2-57df7c7b5b-z5p2v    0 (0%)        0 (0%)      0 (0%)           0 (0%)
      ```
      {: screen}

    2. 에지 노드가 아닌 노드에 배치된 ALB 팟(Pod)이 없는 것을 확인하십시오. 예:
      ```
      kubectl describe nodes -l dedicated!=edge | grep alb
      ```
      {: pre}
      * ALB 팟(Pod)이 올바르게 에지 노드에 배치된 경우에는 ALB 팟(Pod)이 리턴되지 않습니다. 이 경우에는 ALB가 성공적으로 에지 작업자 노드에만 다시 스케줄된 것입니다. 
      * ALB 팟(Pod)이 리턴된 경우에는 다음 단계로 진행하십시오. 

6. NLB 또는 ALB 팟(Pod)이 여전히 에지 노드가 아닌 노드에 배치되는 경우에는 에지 노드에 다시 배치되도록 해당 팟(Pod)을 삭제할 수 있습니다. **중요**: 팟(Pod)은 한 번에 하나만 삭제하고, 다른 팟(Pod)을 삭제하기 전에 해당 팟(Pod)이 에지 노드에 다시 스케줄되었는지 확인하십시오. 
  1. 팟(Pod)을 삭제하십시오. `webserver-lb` NLB 팟(Pod) 중 하나가 에지 노드에 스케줄되지 않은 경우에 대한 예는 다음과 같습니다.
    ```
    kubectl delete pod ibm-cloud-provider-ip-169-46-17-2-76fcb4965d-wz6dg
    ```
    {: pre}

  2. 해당 팟이 에지 작업자 노드에 다시 스케줄되었는지 확인하십시오. 재스케줄링은 자동이지만 몇 분 정도 소요될 수 있습니다. 외부 IP 주소가 `169.46.17.2`인 `webserver-lb` NLB에 대한 예는 다음과 같습니다.
    ```
    kubectl describe nodes -l dedicated=edge | grep "169-46-17-2"
    ```
    {: pre}

    출력 예:
    ```
    ibm-system                 ibm-cloud-provider-ip-169-46-17-2-76fcb4965d-wz6dg                 5m (0%)       0 (0%)      10Mi (0%)        0 (0%)
    ibm-system                 ibm-cloud-provider-ip-169-46-17-2-76fcb4965d-2z64r                 5m (0%)       0 (0%)      10Mi (0%)        0 (0%)
    ```
    {: screen}

</br>작업자 풀의 작업자 노드를 `dedicated=edge`로 레이블 지정했으며 모든 기존 ALB 및 NLB를 에지 노드에 다시 배치하였습니다. 클러스터에 추가되는 모든 후속 ALB 및 NLB 또한 에지 작업자 풀의 에지 노드에 배치됩니다. 다음으로 다른 [워크로드가 에지 작업자 노드에서 실행](#edge_workloads)되지 않도록 하고 [작업자 노드에서 NodePort에 대한 인바운드 트래픽을 차단](/docs/containers?topic=containers-network_policies#block_ingress)하십시오.

<br />


## 워크로드가 에지 작업자 노드에서 실행되지 않도록 금지
{: #edge_workloads}

에지 작업자 노드의 이점은 네트워킹 서비스만 실행하도록 이러한 작업자 노드를 지정할 수 있다는 것입니다.
{:shortdesc}

`dedicated=edge` 허용을 사용하는 것은 모든 네트워크 로드 밸런서(NLB) 및 Ingress 애플리케이션 로드 밸런서(ALB) 서비스가 레이블 지정된 작업자 노드에만 배치됨을 의미합니다. 하지만 다른 워크로드가 에지 작업자 노드에서 실행되지 않고 작업자 노드 리소스를 이용하지 못하도록 하려면 [Kubernetes 오염(taint) ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/)을 사용해야 합니다.

시작하기 전에:
- [모든 네임스페이스에 대해 **관리자** {{site.data.keyword.Bluemix_notm}} IAM 서비스 역할](/docs/containers?topic=containers-users#platform)을 보유하고 있는지 확인하십시오. 
- [계정에 로그인하십시오. 해당되는 경우, 적절한 리소스 그룹을 대상으로 지정하십시오. 클러스터의 컨텍스트를 설정하십시오.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

</br>다른 워크로드가 에지 작업자 노드에서 실행되지 않도록 하려면 다음을 수행하십시오. 

1. `dedicated=edge` 레이블이 있는 모든 작업자 노드에, 작업자 노드에서 `dedicated=edge` 레이블이 없는 팟(Pod)이 실행되지 않도록 하며 이러한 팟(Pod)을 제거하는 오염을 적용하십시오. 제거된 팟(Pod)은 용량이 있는 다른 작업자 노드에 다시 배치됩니다. 
  ```
  kubectl taint node -l dedicated=edge dedicated=edge:NoSchedule dedicated=edge:NoExecute
  ```
  {: pre}
이제 `dedicated=edge` 결함 허용을 사용하는 팟(Pod)만 에지 작업자 노드에 배치됩니다.

2. 에지 노드가 오염되었는지 확인하십시오. 
  ```
  kubectl describe nodes -l dedicated=edge | grep "Taint|Hostname"
  ```
  {: pre}

  출력 예:
  ```
  Taints:             dedicated=edge:NoExecute
    Hostname:    10.176.48.83
  Taints:             dedicated=edge:NoExecute
    Hostname:    10.184.58.7
  ```
  {: screen}

3. [NLB 1.0 서비스에 대해 소스 IP 보존을 사용으로 설정](/docs/containers?topic=containers-loadbalancer#node_affinity_tolerations)하도록 선택한 경우에는 [에지 노드 친화성을 앱 팟(Pod)에 추가](/docs/containers?topic=containers-loadbalancer#lb_edge_nodes)하여 앱 팟(Pod)이 에지 작업자 노드에 스케줄되도록 하십시오. 수신 요청을 받도록 앱 팟(Pod)이 에지 노드에 스케줄되어야 합니다.

4. 오염을 제거하려면 다음 명령을 실행하십시오.
    ```
    kubectl taint node <node_name> dedicated:NoSchedule- dedicated:NoExecute-
    ```
    {: pre}
