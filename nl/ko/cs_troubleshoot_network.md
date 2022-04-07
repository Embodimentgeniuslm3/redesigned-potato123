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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}


# 클러스터 네트워킹 문제점 해결
{: #cs_troubleshoot_network}

{{site.data.keyword.containerlong}}를 사용할 때 클러스터 네트워킹 관련 문제점을 해결하려면 이러한 기술을 고려하십시오.
{: shortdesc}

Ingress를 통해 앱에 연결하는 데 문제가 있습니까? [Ingress 디버깅](/docs/containers?topic=containers-cs_troubleshoot_debug_ingress)을 시도하십시오.
{: tip}

문제점을 해결하는 중에 [{{site.data.keyword.containerlong_notm}} 진단 및 디버그 도구](/docs/containers?topic=containers-cs_troubleshoot#debug_utility)를 사용하여 테스트를 수행하고 클러스터에서 관련 네트워킹, Ingress 및 strongSwan 정보를 수집할 수 있습니다.
{: tip}

## 네트워크 로드 밸런서(NLB) 서비스를 통해 앱에 연결하는 데 실패
{: #cs_loadbalancer_fails}

{: tsSymptoms}
클러스터에서 NLB 서비스를 작성하여 공용으로 앱을 노출했습니다. NLB의 공인 IP 주소를 사용하여 앱에 연결하려고 할 때 연결에 실패했거나 제한시간이 초과되었습니다.

{: tsCauses}
다음과 이유 중 하나로 인해 로드 NLB가 제대로 작동하지 않을 수 있습니다.

-   클러스터는 작업자 노드가 하나뿐인 표준 클러스터 또는 무료 클러스터입니다.
-   클러스터가 아직 완전히 배치되지 않았습니다.
-   NLB 서비스의 구성 스크립트에 오류가 포함되어 있습니다.

{: tsResolve}
NLB 서비스의 문제점을 해결하려면 다음을 수행하십시오.

1.  NLB 서비스의 고가용성을 보장할 수 있도록 2개 이상의 작업자 노드가 있으며 완전히 배치된 표준 클러스터를 설정했는지 확인하십시오.

  ```
   ibmcloud ks workers --cluster <cluster_name_or_ID>
  ```
  {: pre}

    CLI 출력에서 작업자 노드의 **상태**에 **준비**가 표시되고 **머신 유형**에 **무료** 이외의 머신 유형이 표시되는지 확인하십시오.

2. 버전 2.0 NLB의 경우: [NLB 2.0 전제조건](/docs/containers?topic=containers-loadbalancer#ipvs_provision)을 완료했는지 확인하십시오.

3. NLB 서비스의 구성 파일이 정확한지 확인하십시오.
    * 버전 2.0 NLB:
        ```
        apiVersion: v1
        kind: Service
        metadata:
          name: myservice
          annotations:
            service.kubernetes.io/ibm-load-balancer-cloud-provider-enable-features: "ipvs"
        spec:
          type: LoadBalancer
          selector:
            <selector_key>:<selector_value>
          ports:
           - protocol: TCP
             port: 8080
          externalTrafficPolicy: Local
        ```
        {: screen}

        1. **LoadBalancer**를 서비스 유형으로 정의했는지 확인하십시오.
        2. `service.kubernetes.io/ibm-load-balancer-cloud-provider-enable-features: "ipvs"` 어노테이션을 포함했는지 확인하십시오.
        3. LoadBalancer 서비스의 `spec.selector` 섹션에서 `<selector_key>` 및 `<selector_value>`가 배치 YAML의 `spec.template.metadata.labels` 섹션에서 사용한 키/값 쌍과 동일한지 확인하십시오. 레이블이 일치하지 않으면 LoadBalancer 서비스의 **Endpoints** 섹션에 **`<none>`**이 표시되며 인터넷을 통해 앱에 액세스할 수 없습니다.
        4. 앱에서 청취하는 **port**를 사용했는지 확인하십시오.
        5. `externalTrafficPolicy`를 `Local`로 설정했는지 확인하십시오.

    * 버전 1.0 NLB:
        ```
            apiVersion: v1
    kind: Service
    metadata:
      name: myservice
    spec:
      type: LoadBalancer
      selector:
        <selector_key>:<selector_value>
      ports:
           - protocol: TCP
             port: 8080
        ```
        {: screen}

        1. **LoadBalancer**를 서비스 유형으로 정의했는지 확인하십시오.
        2. LoadBalancer 서비스의 `spec.selector` 섹션에서 `<selector_key>` 및 `<selector_value>`가 배치 YAML의 `spec.template.metadata.labels` 섹션에서 사용한 키/값 쌍과 동일한지 확인하십시오. 레이블이 일치하지 않으면 LoadBalancer 서비스의 **Endpoints** 섹션에 **`<none>`**이 표시되며 인터넷을 통해 앱에 액세스할 수 없습니다.
        3. 앱에서 청취하는 **port**를 사용했는지 확인하십시오.

3.  선택 서비스를 확인하고 **Events** 섹션을 검토하여 잠재적 오류를 찾으십시오.

    ```
    kubectl describe service <myservice>
    ```
    {: pre}

    다음 오류 메시지를 찾으십시오.

    <ul><li><pre class="screen"><code>Clusters with one node must use services of type NodePort</code></pre></br>NLB 서비스를 사용하려면 두 개 이상의 작업자 노드가 있는 표준 클러스터가 있어야 합니다.</li>
    <li><pre class="screen"><code>No cloud provider IPs are available to fulfill the NLB service request. Add a portable subnet to the cluster and try again</code></pre></br>이 오류 메시지는 NLB 서비스에 할당할 포터블 공인 IP 주소가 남아 있지 않음을 나타냅니다. 클러스터의 포터블 공인 IP 주소를 요청하는 방법에 대한 정보는 <a href="/docs/containers?topic=containers-subnets#subnets">클러스터에 서브넷 추가</a>를 참조하십시오. 포터블 공인 IP 주소를 클러스터에 사용할 수 있게 되면 NLB 서비스가 자동으로 작성됩니다.</li>
    <li><pre class="screen"><code>Requested cloud provider IP <cloud-provider-ip> is not available. The following cloud provider IPs are available: <available-cloud-provider-ips></code></pre></br>**`loadBalancerIP`** 섹션을 사용하여 로드 밸런서 YAML의 포터블 공인 IP 주소를 정의했지만, 이 포터블 공인 IP 주소는 포터블 공용 서브넷에서 사용할 수 없습니다. 구성 스크립트의 **`loadBalancerIP`** 섹션에서 기존 IP 주소를 제거하고 사용 가능한 포터블 공인 IP 주소 중 하나를 추가하십시오. 사용 가능한 포터블 공인 IP 주소를 자동으로 할당할 수 있도록 스크립트에서 **`loadBalancerIP`** 섹션을 제거할 수도 있습니다.</li>
    <li><pre class="screen"><code>No available nodes for NLB services</code></pre>NLB 서비스를 배치하는 데 충분한 작업자 노드가 없습니다. 그 이유 중 하나는 작업자 노드가 두 개 이상인 표준 클러스터를 배치했지만 작업자 노드의 프로비저닝에 실패했기 때문일 수 있습니다.</li>
    <ol><li>사용 가능한 작업자 노드를 나열하십시오.</br><pre class="pre"><code>kubectl get nodes</code></pre></li>
    <li>두 개 이상의 사용 가능한 작업자 노드를 발견하면 작업자 노드 세부사항을 나열하십시오.</br><pre class="pre"><code>ibmcloud ks worker-get --cluster &lt;cluster_name_or_ID&gt; --worker &lt;worker_ID&gt;</code></pre></li>
    <li><code>kubectl get nodes</code> 및 <code>ibmcloud ks worker-get</code> 명령으로 리턴된 작업자 노드의 공인 및 사설 VLAN ID가 일치하는지 확인하십시오.</li></ol></li></ul>

4.  사용자 정의 도메인을 사용하여 로드 밸런서 서비스에 연결하는 경우 사용자 정의 도메인이 NLB 서비스의 공인 IP 주소에 맵핑되었는지 확인하십시오.
    1.  NLB 서비스의 공인 IP 주소를 찾으십시오.
        ```
        kubectl describe service <service_name> | grep "LoadBalancer Ingress"
        ```
        {: pre}

    2.  사용자 정의 도메인이 포인터 레코드(PTR)에 있는 NLB 서비스의 포터블 공인 IP 주소에 맵핑되었는지 확인하십시오.

<br />


## Ingress를 통해 앱에 연결하는 데 실패
{: #cs_ingress_fails}

{: tsSymptoms}
클러스터에 앱의 Ingress 리소스를 작성하여 공용으로 앱을 노출했습니다. Ingress 애플리케이션 로드 밸런서(ALB)의 하위 도메인 또는 공인 IP 주소를 사용하여 앱에 연결하려고 할 때 연결에 실패했거나 제한시간이 초과되었습니다.

{: tsResolve}
ALB의 고가용성을 보장할 수 있도록 우선 클러스터가 완전히 배치되어 있으며 구역마다 2개 이상의 작업자 노드가 사용 가능한지 확인하십시오.
```
   ibmcloud ks workers --cluster <cluster_name_or_ID>
```
{: pre}

CLI 출력에서 작업자 노드의 **상태**에 **준비**가 표시되고 **머신 유형**에 **무료** 이외의 머신 유형이 표시되는지 확인하십시오.

* 표준 클러스터가 완전히 배치되어 있으며 구역마다 2개 이상의 작업자 노드가 있지만 **Ingress 하위 도메인**을 사용할 수 없는 경우에는 [Ingress ALB의 하위 도메인을 가져올 수 없음](/docs/containers?topic=containers-cs_troubleshoot_network#cs_subnet_limit)을 참조하십시오.
* 기타 문제의 경우에는 [Ingress 디버깅](/docs/containers?topic=containers-cs_troubleshoot_debug_ingress)의 단계에 따라 Ingress 설정의 문제점을 해결하십시오.

<br />


## Ingress 애플리케이션 로드 밸런서(ALB) 시크릿 문제
{: #cs_albsecret_fails}

{: tsSymptoms}
`ibmcloud ks alb-cert-deploy` 명령을 실행하여 Ingress 애플리케이션 로드 밸런서(ALB) 시크릿을 클러스터에 배치한 후 {{site.data.keyword.cloudcerts_full_notm}}에서 인증서를 볼 때 `Description` 필드가 시크릿 이름으로 업데이트되지 않습니다.

ALB 시크릿에 대한 정보를 나열할 때 상태가 `*_failed`로 표시됩니다. 예: `create_failed`, `update_failed`, `delete_failed`.

{: tsResolve}
ALB 시크릿이 실패할 수 있는 다음과 같은 이유와 해당 문제점 해결 단계를 검토하십시오.

<table>
<caption>Ingress 애플리케이션 로드 밸런서 시크릿 문제점 해결</caption>
 <thead>
 <th>발생 원인</th>
 <th>해결 방법</th>
 </thead>
 <tbody>
 <tr>
 <td>인증서 데이터 다운로드와 업데이트에 필요한 액세스 역할이 없습니다.</td>
 <td>계정 관리자에게 요청하여 다음의 {{site.data.keyword.Bluemix_notm}} IAM 역할을 지정받으십시오.<ul><li>{{site.data.keyword.cloudcerts_full_notm}} 인스턴스에 대한 **관리자** 및 **작성자** 서비스 역할. 자세한 정보는 {{site.data.keyword.cloudcerts_short}}에 대한 <a href="/docs/services/certificate-manager?topic=certificate-manager-managing-service-access-roles#managing-service-access-roles">서비스 액세스 관리</a>를 참조하십시오.</li><li>클러스터에 대한 <a href="/docs/containers?topic=containers-users#platform">**관리자** 플랫폼 역할</a>.</li></ul></td>
 </tr>
 <tr>
 <td>작성, 업데이트 또는 제거 시 제공한 인증서 CRN이 클러스터와 동일한 계정에 속하지 않습니다.</td>
 <td>제공한 인증서 CRN을 클러스터와 동일한 계정에 배치된 {{site.data.keyword.cloudcerts_short}} 서비스 인스턴스로 가져왔는지 확인하십시오.</td>
 </tr>
 <tr>
 <td>작성 시 제공한 인증서 CRN이 잘못되었습니다.</td>
 <td><ol><li>제공한 인증서 CRN이 정확한지 확인하십시오.</li><li>인증서 CRN이 정확하다고 판단되면 <code>ibmcloud ks alb-cert-deploy --update --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code>을 실행하여 시크릿을 업데이트해 보십시오.</li><li>이 명령을 실행한 결과로 <code>update_failed</code> 상태가 되면 <code>ibmcloud ks alb-cert-rm --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt;</code>을 실행하여 시크릿을 제거하십시오.</li><li><code>ibmcloud ks alb-cert-deploy --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code>을 실행하여 시크릿을 다시 배치하십시오.</li></ol></td>
 </tr>
 <tr>
 <td>업데이트 시 제공한 인증서 CRN이 잘못되었습니다.</td>
 <td><ol><li>제공한 인증서 CRN이 정확한지 확인하십시오.</li><li>인증서 CRN이 정확하다고 판단되면 <code>ibmcloud ks alb-cert-rm --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt;</code>을 실행하여 시크릿을 제거하십시오.</li><li><code>ibmcloud ks alb-cert-deploy --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code>을 실행하여 시크릿을 다시 배치하십시오.</li><li><code>ibmcloud ks alb-cert-deploy --update --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code>을 실행하여 시크릿을 업데이트해 보십시오.</li></ol></td>
 </tr>
 <tr>
 <td>{{site.data.keyword.cloudcerts_long_notm}} 서비스에서 가동 중단이 발생합니다.</td>
 <td>{{site.data.keyword.cloudcerts_short}} 서비스가 시작되어 실행 중인지 확인하십시오.</td>
 </tr>
 <tr>
 <td>가져온 시크릿은 IBM 에서 제공하는 Ingress 시크릿과 동일한 이름입니다.</td>
 <td>시크릿의 이름을 바꾸십시오. `ibmcloud ks cluster-get --cluster <cluster_name_or_ID> | grep Ingress`를 실행하여 IBM 제공 Ingress 시크릿 이름을 확인할 수 있습니다.</td>
 </tr>
 </tbody></table>

<br />


## Ingress ALB에 대한 하위 도메인을 가져올 수 없으며, ALB는 구역에 배치되지 않거나 로드 밸런서를 배치할 수 없음
{: #cs_subnet_limit}

{: tsSymptoms}
* Ingress 하위 도메인 없음: `ibmcloud ks cluster-get --cluster <cluster>`를 실행하면 클러스터는 `normal` 상태이지만 사용 가능한 **Ingress 하위 도메인**이 없습니다.
* ALB는 구역에 배치되지 않음: 다중 구역 클러스터가 있으며 `ibmcloud ks albs --cluster <cluster>`를 실행하는 경우, ALB가 구역에 배치되지 않습니다. 예를 들어, 3개의 구역에 작업자 노드가 있으면 공용 ALB가 세 번째 구역에 배치되지 않은 다음과 유사한 출력을 볼 수 있습니다.
  ```
  ALB ID                                            Enabled    Status     Type      ALB IP           Zone    Build                          ALB VLAN ID
  private-cr96039a75fddb4ad1a09ced6699c88888-alb1   false      disabled   private   -                dal10   ingress:411/ingress-auth:315   2294021
  private-cr96039a75fddb4ad1a09ced6699c88888-alb2   false      disabled   private   -                dal12   ingress:411/ingress-auth:315   2234947
  private-cr96039a75fddb4ad1a09ced6699c88888-alb3   false      disabled   private   -                dal13   ingress:411/ingress-auth:315   2234943
  public-cr96039a75fddb4ad1a09ced6699c88888-alb1    true       enabled    public    169.xx.xxx.xxx   dal10   ingress:411/ingress-auth:315   2294019
  public-cr96039a75fddb4ad1a09ced6699c88888-alb2    true       enabled    public    169.xx.xxx.xxx   dal12   ingress:411/ingress-auth:315   2234945
  ```
  {: screen}
* 로드 밸런서를 배치할 수 없음: `ibm-cloud-provider-vlan-ip-config` configmap을 설명할 때 다음 출력 예와 유사한 오류 메시지가 표시될 수 있습니다. 
  ```
  kubectl get cm ibm-cloud-provider-vlan-ip-config
  ```
  {: pre}
  ```
  Warning  CreatingLoadBalancerFailed ... ErrorSubnetLimitReached: There are already the maximum number of subnets permitted in this VLAN.
  ```
  {: screen}

{: tsCauses}
표준 클러스터에서 구역의 클러스터를 처음으로 작성하는 경우, 해당 구역의 공용 VLAN 및 사설 VLAN은 IBM Cloud 인프라(SoftLayer) 계정에서 사용자를 위해 자동으로 프로비저닝됩니다. 해당 구역에서는 사용자가 지정하는 공용 VLAN에서 1개의 공용 포터블 서브넷이 요청되며 사용자가 지정하는 사설 VLAN에서 1개의 사설 포터블 서브넷이 요청됩니다. {{site.data.keyword.containerlong_notm}}의 경우 VLAN에는 서브넷이 40개로 제한되어 있습니다. 구역에서 클러스터의 VLAN이 이미 해당 한계에 도달한 경우에는 **Ingress 하위 도메인**이 프로비저닝에 실패하거나 해당 구역에 대한 공용 Ingress ALB가 프로비저닝에 실패하거나 네트워크 로드 밸런서(NLB)를 작성하는 데 사용할 수 있는 포터블 공인 IP 주소를 보유하고 있지 않을 수 있습니다. 

VLAN의 서브넷 수를 보려면 다음을 수행하십시오.
1.  [IBM Cloud 인프라(SoftLayer) 콘솔](https://cloud.ibm.com/classic?)에서 **네트워크** > **IP 관리** > **VLAN**을 선택하십시오.
2.  클러스터를 작성하는 데 사용한 VLAN의 **VLAN 번호**를 클릭하십시오. **서브넷** 섹션을 검토하여 40개 이상의 서브넷이 있는지 확인하십시오.

{: tsResolve}
새 VLAN이 필요하면 [{{site.data.keyword.Bluemix_notm}} 지원 팀에 문의](/docs/infrastructure/vlans?topic=vlans-ordering-premium-vlans#ordering-premium-vlans)하여 VLAN을 주문하십시오. 그런 다음, 이 새 VLAN을 사용하는 [클러스터를 작성](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_create)하십시오.

사용 가능한 다른 VLAN이 있는 경우에는 기존 클러스터에 [VLAN Spanning을 설정](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning)할 수 있습니다. 그 후에는 사용 가능한 서브넷이 있는 다른 VLAN을 사용하는 클러스터에 새 작업자 노드를 추가할 수 있습니다. VLAN Spanning이 이미 사용으로 설정되었는지 확인하려면 `ibmcloud ks vlan-spanning-get --region <region>` [명령](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get)을 사용하십시오. 

VLAN의 모든 서브넷을 사용 중인 경우가 아니면 서브넷을 클러스터에 추가하여 VLAN에서 서브넷을 재사용할 수 있습니다.
1. 사용할 서브넷이 사용 가능한지 확인하십시오.
  <p class="note">사용 중인 인프라 계정이 여러 {{site.data.keyword.Bluemix_notm}} 계정 간에 공유될 수 있습니다. 이 경우에는 `ibmcloud ks subnets` 명령을 실행하여 **바인딩된 클러스터**의 서브넷을 확인해도 사용자가 자체 클러스터에 대한 정보만 볼 수 있습니다. 인프라 계정 소유자에게 확인하여 해당 서브넷이 사용 가능하며 다른 계정 또는 팀에 의해 사용 중이 아닌지 확인하십시오.</p>

2. [`ibmcloud ks cluster-subnet-add` 명령](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_subnet_add) 기존 서브넷을 클러스터에서 사용할 수 있도록 하십시오.

3. 서브넷이 정상적으로 작성되어 클러스터에 추가되었는지 확인하십시오. 서브넷 CIDR은 **Subnet VLANs** 섹션에 나열되어 있습니다.
    ```
    ibmcloud ks cluster-get --showResources <cluster_name_or_ID>
    ```
    {: pre}

    이 예제 출력에서는 두 번째 서브넷이 `2234945` 공용 VLAN에 추가되었습니다.
    ```
    Subnet VLANs
    VLAN ID   Subnet CIDR          Public   User-managed
    2234947   10.xxx.xx.xxx/29     false    false
    2234945   169.xx.xxx.xxx/29    true     false
    2234945   169.xx.xxx.xxx/29    true     false
    ```
    {: screen}

4. 추가한 서브넷의 포터블 IP 주소가 클러스터의 ALB 또는 로드 밸런서에 사용되는지 확인하십시오. 서비스가 새로 추가된 서브넷에서 포터블 IP 주소를 사용하는 데 몇 분이 걸릴 수 있습니다. 
  * Ingress 하위 도메인 없음: `ibmcloud ks cluster-get --cluster <cluster>`를 실행하여 **Ingress 하위 도메인**이 채워져 있는지 확인하십시오.
  * ALB가 구역에 배치되지 않음: `ibmcloud ks albs --cluster <cluster>`를 실행하여 누락된 ALB가 배치되었는지 확인하십시오.
  * 로드 밸런서를 배치할 수 없음: `kubectl get svc -n kube-system`을 실행하여 로드 밸런서에 **EXTERNAL-IP**가 있는지 확인하십시오.

<br />


## WebSocket을 통한 연결이 60초 후에 닫힘
{: #cs_ingress_websocket}

{: tsSymptoms}
Ingress 서비스는 WebSocket을 사용하는 앱을 노출합니다. 그러나 60초 동안 서로 간에 트래픽이 전송되지 않으면 클라이언트와 WebSocket 앱 간의 연결이 닫힙니다.

{: tsCauses}
다음 이유 중 하나로 인해 60초 넘게 비활성 상태이면 WebSocket 앱에 대한 연결이 삭제될 수 있습니다.

* 인터넷 연결에 장기 연결을 허용하지 않는 프록시 또는 방화벽이 있습니다.
* WebSocket 앱에 대한 ALB의 제한시간 초과로 연결이 종료됩니다.

{: tsResolve}
60초 넘게 비활성 상태여도 연결이 닫히지 않도록 하려면 다음을 수행하십시오.

1. 프록시 또는 방화벽을 통해 WebSocket 앱에 연결하는 경우에는 프록시 또는 방화벽이 장기 연결을 자동 종료하도록 구성되지 않았는지 확인하십시오.

2. 연결이 활성 상태를 유지할 수 있도록 제한시간 값을 늘리거나 앱의 하트 비트를 설정할 수 있습니다.
<dl><dt>제한시간 변경</dt>
<dd>ALB 구성에서 `proxy-read-timeout`의 값을 늘리십시오. 예를 들어, `60s`의 제한시간을 보다 큰 값(예: `300s`)으로 변경하고 다음의 [어노테이션](/docs/containers?topic=containers-ingress_annotation#connection)을 Ingress 리소스 파일에 추가하십시오. `ingress.bluemix.net/proxy-read-timeout: "serviceName=<service_name> timeout=300s"`. 클러스터의 모든 공용 ALB에 대해 제한시간이 변경됩니다.</dd>
<dt>하트비트 설정</dt>
<dd>ALB의 기본 읽기 제한시간 값을 변경하지 않으려면 WebSocket 앱의 하트비트를 설정하십시오. [WAMP ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://wamp-proto.org/) 등의 프레임워크를 사용하여 하트비트 프로토콜을 설정하는 경우, 앱의 업스트림 서버는 주기적으로 "ping" 메시지를 일정 시간 간격마다 전송하고 클라이언트는 "pong" 메시지로 응답합니다. 60초 제한시간이 적용되기 전에 "ping/pong" 트래픽이 열린 연결 상태를 유지할 수 있도록 하트비트 간격을 58초 이하로 설정하십시오.</dd></dl>

<br />


## 오염된 노드를 사용하는 경우 소스 IP 보존이 실패함
{: #cs_source_ip_fails}

{: tsSymptoms}
서비스의 구성 파일에서 `externalTrafficPolicy`를 `Local`로 변경하여 [버전 1.0 로드 밸런서](/docs/containers?topic=containers-loadbalancer#node_affinity_tolerations) 또는 [Ingress ALB](/docs/containers?topic=containers-ingress#preserve_source_ip) 서비스에 대해 소스 IP 주소 보존을 사용으로 설정했습니다. 그러나 앱의 백엔드 서비스에 트래픽이 도달하지 않습니다.

{: tsCauses}
로드 밸런서 또는 Ingress ALB 서비스에 대해 소스 IP 주소 보존을 사용으로 설정하면 클라이언트 요청의 소스 IP 주소가 보존됩니다. 해당 서비스는 요청 패킷의 IP 주소가 변경되지 않았음을 보장하기 위해 동일한 작업자 노드에 있는 앱 팟(Pod)에만 트래픽을 전달합니다. 일반적으로, 로드 밸런서 또는 Ingress ALB 서비스 팟(Pod)은 앱 팟(Pod)이 배치된 작업자 노드와 동일한 노드에 배치됩니다. 그러나 서비스 팟(Pod)과 앱 팟(Pod)이 동일한 작업자 노드에 스케줄되지 않는 상황 또한 있습니다. 작업자 노드에 [Kubernetes 오염 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/)을 사용하면 오염 허용이 설정되지 않은 팟(Pod)은 오염된 작업자 노드에서 실행되지 않게 됩니다. 소스 IP 주소 보존은 사용하는 오염 유형에 따라 작동하지 않을 수 있습니다.

* **에지 노드 오염**: 사용자가 클러스터의 각 공용 VLAN에 있는 둘 이상의 작업자 노드에 [`dedicated=edge` 레이블을 추가](/docs/containers?topic=containers-edge#edge_nodes)하여 이러한 작업자 노드에만 Ingress 및 로드 밸런서 팟(Pod)이 배치되도록 하였습니다. 그 후에는 [이러한 에지 노드 또한 오염](/docs/containers?topic=containers-edge#edge_workloads)시켜 다른 워크로드가 이러한 에지 노드에서 실행되지 않도록 하였습니다. 그러나 앱 배치에 에지 노드 친화성 규칙 및 오염 허용을 추가하지는 않았습니다. 앱 팟(Pod)이 서비스 팟(Pod)과 동일한 오염된 노드에 스케줄될 수 없으므로 앱의 백엔드 서비스에 트래픽이 도달하지 않습니다.

* **사용자 정의 오염**: 사용자가 여러 노드에 사용자 정의 오염을 사용하여 해당 오염 허용이 있는 팟(Pod)만 이러한 노드에 배치될 수 있도록 하였습니다. 그 후 앱과 로드 밸런서 또는 Ingress 서비스의 배치에 친화성 규칙 및 오염 허용을 추가하여 이들의 팟(Pod)이 이러한 노드에만 배치되도록 하였습니다. 그러나 `ibm-system` 네임스페이스에서 자동으로 작성된 `ibm-cloud-provider-ip` `keepalived` 팟(Pod)은 로드 밸런서 팟(Pod) 및 앱 팟(Pod)이 항상 동일한 작업자 노드로 스케줄되도록 보장합니다. 이러한 `keepalived` 팟(Pod)에는 사용된 사용자 정의 오염에 대한 오염 허용이 없습니다. 이들은 앱 팟(Pod)이 실행 중인 동일한 오염된 노드에 스케줄될 수 없으므로 앱의 백엔드 서비스에 트래픽이 도달하지 않습니다.

{: tsResolve}
다음 선택사항 중 하나를 선택하여 문제를 해결하십시오.

* **에지 노드 오염**: 로드 밸런서 및 앱 팟(Pod)이 오염된 에지 노드에 배치되도록 하려면 [앱 배치에 에지 노드 친화성 규칙 및 오염 허용을 추가](/docs/containers?topic=containers-loadbalancer#lb_edge_nodes)하십시오. 로드 밸런서 및 Ingress ALB 팟(Pod)에는 기본적으로 이러한 친화성 규칙 및 오염 허용이 있습니다.

* **사용자 정의 오염**: `keepalived` 팟(Pod)에 오염 허용이 없는 사용자 정의 오염을 제거하십시오. 대신 [작업자 노드를 에지 노드로 레이블 지정한 후 이러한 에지 노드를 오염](/docs/containers?topic=containers-edge)시킬 수 있습니다.

위 선택사항 중 하나를 완료했으나 `keepalived` 팟(Pod)이 여전히 스케줄되지 않는 경우에는 다음을 수행하여 `keepalived` 팟(Pod)에 대한 더 자세한 정보를 얻을 수 있습니다.

1. `keepalived` 팟(Pod)을 가져오십시오.
    ```
    kubectl get pods -n ibm-system
    ```
    {: pre}

2. 출력에서 **Status**가 `Pending`인 `ibm-cloud-provider-ip` 팟(Pod)을 찾으십시오. 예:
    ```
    ibm-cloud-provider-ip-169-61-XX-XX-55967b5b8c-7zv9t     0/1       Pending   0          2m        <none>          <none>
    ibm-cloud-provider-ip-169-61-XX-XX-55967b5b8c-8ptvg     0/1       Pending   0          2m        <none>          <none>
    ```
    {:screen}

3. 각 `keepalived` 팟(Pod)에 대한 설명을 출력하고 **Events** 섹션을 찾으십시오. 나열된 모든 오류 또는 경고 메시지를 처리하십시오.
    ```
    kubectl describe pod ibm-cloud-provider-ip-169-61-XX-XX-55967b5b8c-7zv9t -n ibm-system
    ```
    {: pre}

<br />


## strongSwan Helm 차트를 사용하여 VPN 연결을 설정할 수 없음
{: #cs_vpn_fails}

{: tsSymptoms}
`kubectl exec  $STRONGSWAN_POD -- ipsec status`를 실행하여 VPN 연결을 확인하는 경우, `ESTABLISHED`의 상태가 보이지 않거나 VPN 팟(Pod)이 `ERROR` 상태에 있거나 계속해서 중단된 후 다시 시작됩니다.

{: tsCauses}
Helm 차트 구성 파일에 올바르지 않은 값, 누락된 값 또는 구문 오류가 있습니다.

{: tsResolve}
strongSwan Helm 차트를 사용하여 VPN 연결을 설정하려는 경우 처음에 VPN 상태는 `ESTABLISHED`가 아닐 수 있습니다. 여러 유형의 문제를 확인하고 구성 파일을 적절하게 변경해야 할 수 있습니다. strongSwan VPN 연결의 문제점을 해결하려면 다음을 수행하십시오.

1. strongSwan 차트 정의에 포함된 5개의 Helm 테스트를 실행하여 [strongSwan VPN 연결을 테스트하고 확인](/docs/containers?topic=containers-vpn#vpn_test)하십시오.

2. Helm 테스트를 실행한 후에 VPN 연결을 설정할 수 없는 경우에는 VPN 팟(Pod) 이미지의 내부에 패키징된 VPN 디버깅 도구를 실행할 수 있습니다.

    1. `STRONGSWAN_POD` 환경 변수를 설정하십시오.

        ```
    export STRONGSWAN_POD=$(kubectl get pod -l app=strongswan,release=vpn -o jsonpath='{ .items[0].metadata.name }')
        ```
        {: pre}

    2. 디버깅 도구를 실행하십시오.

        ```
        kubectl exec  $STRONGSWAN_POD -- vpnDebug
        ```
        {: pre}

        도구는 공통 네트워킹 문제에 대한 여러 테스트를 실행할 때 정보의 여러 페이지를 출력합니다. `ERROR`, `WARNING`, `VERIFY` 또는 `CHECK`로 시작하는 출력 행에는 VPN 연결에 발생할 수 있는 오류가 표시됩니다.

    <br />


## 새 strongSwan Helm 차트 릴리스를 설치할 수 없음
{: #cs_strongswan_release}

{: tsSymptoms}
사용자는 strongSwan Helm 차트를 수정하고 `helm install -f config.yaml --name=vpn ibm/strongswan`을 실행하여 새 릴리스의 설치를 시도합니다. 그러나 다음 오류가 표시됩니다.
```
Error: release vpn failed: deployments.extensions "vpn-strongswan" already exists
```
{: screen}

{: tsCauses}
이 오류는 strongSwan 차트의 이전 릴리스가 완전히 설치 제거되지 않았음을 표시합니다.

{: tsResolve}

1. 이전 차트 릴리스를 삭제하십시오.
    ```
    helm delete --purge vpn
    ```
    {: pre}

2. 이전 릴리스에 대한 배치를 삭제하십시오. 배치 및 연관된 팟(Pod)을 삭제하려면 최대 1분이 소요됩니다.
    ```
    kubectl delete deploy vpn-strongswan
    ```
    {: pre}

3. 배치가 삭제되었는지 확인하십시오. `vpn-strongswan` 배치가 목록에 나타나지 않습니다.
    ```
         kubectl get deployments
    ```
    {: pre}

4. 새 릴리스 이름으로 업데이트된 strongSwan Helm 차트를 다시 설치하십시오.
    ```
    helm install -f config.yaml --name=vpn ibm/strongswan
    ```
    {: pre}

<br />


## 작업자 노드를 추가 또는 삭제한 후에 strongSwan VPN 연결이 실패함
{: #cs_vpn_fails_worker_add}

{: tsSymptoms}
이전에 strongSwan IPSec VPN 서비스를 사용하여 작동하는 VPN 연결을 설정했습니다. 그러나 클러스터의 작업자 노드를 추가하거나 삭제한 후 다음 증상 중 하나가 발생합니다.

* VPN 상태가 `ESTABLISHED`가 아님
* 온프레미스 네트워크에서 새 작업자 노드에 액세스할 수 없음
* 새 작업자 노드에서 실행 중인 팟(Pod)에서 원격 네트워크에 액세스할 수 없음

{: tsCauses}
작업자 풀에 작업자 노드를 추가한 경우:

* 작업자 노드가 기존 `localSubnetNAT` 또는 `local.subnet` 설정으로 VPN 연결에 노출되지 않은 새 사설 서브넷에 프로비저닝됨
* 작업자에게 기존 `tolerations` 또는 `nodeSelector` 설정에 포함되지 않은 오염(taint)이나 레이블이 있기 때문에 VPN 라우트를 작업자 노드에 추가할 수 없음
* VPN 팟(Pod)이 새 작업자 노드에서 실행 중이지만 해당 작업자 노드의 공인 IP 주소는 온프레미스 방화벽을 통해 허용되지 않음

작업자 노드를 삭제한 경우:

* 작업자 노드는 기존 `tolerations` 또는 `nodeSelector` 설정의 특정 오염 또는 레이블에 대한 제한사항으로 인해 VPN 팟(Pod)이 실행되는 유일한 노드임

{: tsResolve}
Helm 차트 값을 업데이트하여 작업자 노드 변경사항을 반영하려면 다음을 수행하십시오.

1. 기존 Helm 차트를 삭제하십시오.

    ```
    helm delete --purge <release_name>
    ```
    {: pre}

2. strongSwan VPN 서비스의 구성 파일을 여십시오.

    ```
    helm inspect values ibm/strongswan > config.yaml
    ```
    {: pre}

3. 필요한 경우 다음 설정을 확인하고 삭제되거나 추가된 작업자 노드를 반영하도록 설정을 변경하십시오.

    작업자 노드를 추가한 경우:

    <table>
    <caption>작업자 노드 설정</caption?
     <thead>
     <th>설정</th>
     <th>설명</th>
     </thead>
     <tbody>
     <tr>
     <td><code>localSubnetNAT</code></td>
     <td>다른 작업자 노드가 있는 기존의 기타 서브넷과 다른 새 사설 서브넷에 추가된 작업자를 배치할 수 있습니다. 서브넷 NAT를 사용하여 클러스터의 사설 로컬 IP 주소를 다시 맵핑하는 중이고 작업자가 새 서브넷에 추가된 경우 새 서브넷 CIDR을 이 설정에 추가하십시오.</td>
     </tr>
     <tr>
     <td><code>nodeSelector</code></td>
     <td>이전에 VPN 팟(Pod) 배치를 특정 레이블이 있는 작업자로 제한한 경우 추가된 작업자 노드에도 해당 레이블이 있는지 확인하십시오.</td>
     </tr>
     <tr>
     <td><code>tolerations</code></td>
     <td>추가된 작업자 노드가 오염된 경우 VPN 팟(Pod)이 임의의 오염 또는 특정 오염이 있는 오염된 모든 작업자 노드에서 실행될 수 있도록 이 설정을 변경하십시오.</td>
     </tr>
     <tr>
     <td><code>local.subnet</code></td>
     <td>다른 작업자가 있는 기존 서브넷과 다른 새 사설 서브넷에 추가된 작업자를 배치할 수 있습니다. 사설 네트워크의 NodePort 또는 LoadBalancer 서비스에서 앱을 노출하고 앱이 추가된 작업자에 있는 경우 새 서브넷 CIDR을 이 설정에 추가하십시오. **참고**: 값을 `local.subnet`에 추가하는 경우, 값도 업데이트되어야 하는지 확인하려면 온프레미스 서브넷에 대한 VPN 설정을 검사하십시오.</td>
     </tr>
     </tbody></table>

    작업자 노드를 삭제한 경우:

    <table>
    <caption>작업자 노드 설정</caption>
     <thead>
     <th>설정</th>
     <th>설명</th>
     </thead>
     <tbody>
     <tr>
     <td><code>localSubnetNAT</code></td>
     <td>서브넷 NAT를 사용하여 특정 사설 로컬 IP 주소를 다시 맵핑하는 경우 이 설정에서 이전 작업자의 IP 주소를 제거하십시오. 서브넷 NAT를 사용하여 전체 서브넷을 다시 맵핑하는 중이고 서브넷에 남아 있는 작업자가 없는 경우 이 설정에서 해당 서브넷 CIDR을 제거하십시오.</td>
     </tr>
     <tr>
     <td><code>nodeSelector</code></td>
     <td>이전에 VPN 팟(Pod) 배치를 단일 작업자로 제한했으며 해당 작업자가 삭제된 경우 VPN 팟(Pod)이 다른 작업자에서 실행될 수 있도록 이 설정을 변경하십시오.</td>
     </tr>
     <tr>
     <td><code>tolerations</code></td>
     <td>삭제한 작업자가 오염되지 않았지만 남아 있는 작업자 노드만 오염된 경우 VPN 팟(Pod)이 임의의 오염 또는 특정 오염이 있는 작업자에서 실행될 수 있도록 이 설정을 변경하십시오.
     </td>
     </tr>
     </tbody></table>

4. 업데이트된 값을 사용하여 새 Helm 차트를 설치하십시오.

    ```
    helm install -f config.yaml --name=<release_name> ibm/strongswan
    ```
    {: pre}

5. 차트 배치 상태를 확인하십시오. 차트가 준비 상태인 경우, 출력의 맨 위 근처에 있는 **STATUS** 필드의 값은 `DEPLOYED`입니다.

    ```
    helm status <release_name>
    ```
    {: pre}

6. 일부 경우, VPN 구성 파일에 작성한 변경사항과 일치하도록 온프레미스 설정 및 방화벽 설정을 변경해야 할 수 있습니다.

7. VPN을 시작하십시오.
    * VPN 연결이 클러스터로 시작된 경우(`ipsec.auto`가 `start`로 설정됨) 온프레미스 게이트웨이에서 VPN을 시작한 후 클러스터에서 VPN을 시작하십시오.
    * VPN 연결이 온프레미스 게이트웨이로 시작된 경우(`ipsec.auto`가 `auto`로 설정됨) 클러스터에서 VPN을 시작한 후 온프레미스 게이트웨이에서 VPN을 시작하십시오.

8. `STRONGSWAN_POD` 환경 변수를 설정하십시오.

    ```
    export STRONGSWAN_POD=$(kubectl get pod -l app=strongswan,release=<release_name> -o jsonpath='{ .items[0].metadata.name }')
    ```
    {: pre}

9. VPN의 상태를 확인하십시오.

    ```
    kubectl exec $STRONGSWAN_POD -- ipsec status
    ```
    {: pre}

    * VPN 연결이 `ESTABLISHED` 상태인 경우 VPN 연결이 성공했습니다. 추가 조치는 필요하지 않습니다.

    * 계속해서 연결 문제가 발생하는 경우 VPN 연결에 대한 문제점을 좀 더 해결하려면 [strongSwan Helm 차트를 사용하여 VPN 연결을 설정할 수 없음](#cs_vpn_fails)을 참조하십시오.

<br />



## Calico 네트워크 정책을 검색할 수 없음
{: #cs_calico_fails}

{: tsSymptoms}
`calicoctl get policy`를 실행하여 클러스터의 Calico 네트워크 정책을 보려고 할 때 다음과 같은 예상치 못한 결과 또는 오류 메시지 중 하나가 발생합니다.
- 비어 있는 목록
- v3 정책을 대신하는 이전 Calico v2 정책의 목록
- `Failed to create Calico API client: syntax error in calicoctl.cfg: invalid config file: unknown APIVersion 'projectcalico.org/v3'`

`calicoctl get GlobalNetworkPolicy`를 실행하여 클러스터의 Calico 네트워크 정책을 보려고 할 때 다음과 같은 예상치 못한 결과 또는 오류 메시지 중 하나가 발생합니다.
- 비어 있는 목록
- `Failed to create Calico API client: syntax error in calicoctl.cfg: invalid config file: unknown APIVersion 'v1'`
- `Failed to create Calico API client: syntax error in calicoctl.cfg: invalid config file: unknown APIVersion 'projectcalico.org/v3'`
- `Failed to get resources: Resource type 'GlobalNetworkPolicy' is not supported`

{: tsCauses}
Calico 정책을 사용하려면 클러스터 Kubernetes 버전, Calico CLI 버전, Calico 구성 파일 구문 및 정책 보기 명령 등 4개의 요인이 모두 맞아야 합니다. 이러한 요인 중 하나 이상의 버전이 올바르지 않습니다.

{: tsResolve}
v3.3 이상 Calico CLI, `calicoctl.cfg` v3 구성 파일 구문과 `calicoctl get GlobalNetworkPolicy` 및 `calicoctl get NetworkPolicy` 명령을 사용해야 합니다.

모든 Calico 요인이 맞는지 확인하려면 다음을 수행하십시오.

1. [버전 3.3 이상 Calico CLI를 설치하고 구성](/docs/containers?topic=containers-network_policies#cli_install)하십시오.
2. 작성하고 클러스터에 적용할 정책이 [Calico v3 구문![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.projectcalico.org/v3.3/reference/calicoctl/resources/networkpolicy)을 사용하는지 확인하십시오. Calico v2 구문으로 된 기존 정책 `.yaml` 또는 `.json` 파일이 있는 경우 [`calicoctl convert` 명령 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.projectcalico.org/v3.3/reference/calicoctl/commands/convert)을 사용하여 Calico v3 구문으로 변환할 수 있습니다.
3. [정책을 보기](/docs/containers?topic=containers-network_policies#view_policies) 위해 글로벌 정책의 경우 `calicoctl get GlobalNetworkPolicy`를 사용하고 특정 네임스페이스로 범위가 지정된 정책의 경우 `calicoctl get NetworkPolicy --namespace <policy_namespace>`를 사용 중인지 확인하십시오.

<br />


## 올바르지 않은 VLAN ID로 인해 작업자 노드를 추가할 수 없음
{: #suspended}

{: tsSymptoms}
{{site.data.keyword.Bluemix_notm}} 계정이 일시중단되었거나 클러스터의 모든 작업자 노드가 삭제되었습니다. 계정이 다시 활성화된 후에는 작업자 풀의 크기를 조정하거나 리밸런싱할 때 작업자 노드를 추가할 수 없습니다. 다음과 유사한 오류 메시지가 표시될 수 있습니다.

```
SoftLayerAPIError(SoftLayer_Exception_Public): Could not obtain network VLAN with id #123456.
```
{: screen}

{: tsCauses}
계정이 일시중단되면 계정 내의 작업자 노드가 삭제됩니다. 클러스터에 작업자 노드가 없는 경우, IBM Cloud 인프라(SoftLayer)는 연관된 공용 및 사설 VLAN을 재확보합니다. 그러나 클러스터 작업자 풀에는 여전히 메타데이터에 이전 VLAN ID가 있으며 풀을 리밸런싱하거나 크기를 조정할 때 이러한 사용 불가능한 ID를 사용합니다. VLAN이 더 이상 클러스터와 연관되지 않으므로 노드의 작성에 실패합니다.

{: tsResolve}

사용자는 [기존 작업자 풀을 삭제](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_rm)한 후에 [새 작업자 풀을 작성](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_create)할 수 있습니다.

또는 새 VLAN을 주문하고 이를 사용하여 풀에서 새 작업자 노드를 작성하여 기존 작업자 풀을 유지할 수 있습니다.

시작하기 전에: [계정에 로그인하십시오. 해당되는 경우, 적절한 리소스 그룹을 대상으로 지정하십시오. 클러스터의 컨텍스트를 설정하십시오.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  새 VLAN ID가 필요한 구역을 가져오려면 다음 명령 출력의 **위치**를 기록해 두십시오. **참고**: 클러스터가 다중 구역인 경우에는 각 구역마다 VLAN ID가 필요합니다.

    ```
    ibmcloud ks clusters
    ```
    {: pre}

2.  [{{site.data.keyword.Bluemix_notm}} 지원 팀에 문의](/docs/infrastructure/vlans?topic=vlans-ordering-premium-vlans#ordering-premium-vlans)하여 클러스터가 있는 각 구역에 대한 새 사설 및 공용 VLAN을 가져오십시오.

3.  각 구역에 대한 새 사설 및 공용 VLAN ID를 기록해 두십시오.

4.  작업자 풀의 이름을 기록해 두십시오.

    ```
    ibmcloud ks worker-pools --cluster <cluster_name_or_ID>
    ```
    {: pre}

5.  `zone-network-set` [명령](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_zone_network_set)을 사용하여 작업자 풀 네트워크 메타데이터를 변경하십시오.

    ```
    ibmcloud ks zone-network-set --zone <zone> --cluster <cluster_name_or_ID> -- worker-pools <worker-pool> --private-vlan <private_vlan_ID> --public-vlan <public_vlan_ID>
    ```
    {: pre}

6.  **다중 구역 클러스터만 해당**: 클러스터의 각 구역에 대해 **5단계**를 반복하십시오.

7.  새 VLAN ID를 사용하는 작업자 노드를 추가할 수 있도록 작업자 풀을 리밸런싱하거나 크기를 조정하십시오. 예를 들어, 다음과 같습니다.

    ```
    ibmcloud ks worker-pool-resize --cluster <cluster_name_or_ID> --worker-pool <worker_pool> --size-per-zone <number_of_workers_per_zone>
    ```
    {: pre}

8.  작업자 노드가 작성되었는지 확인하십시오.

    ```
    ibmcloud ks workers --cluster <cluster_name_or_ID> --worker-pool <worker_pool>
    ```
    {: pre}

<br />



## 도움 및 지원 받기
{: #network_getting_help}

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

