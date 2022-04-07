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



# 클러스터의 서브넷 구성
{: #subnets}

Kubernetes 클러스터에 서브넷을 추가하여 네트워크 로드 밸런서(NLB) 서비스에 사용 가능한 포터블 공인 또는 사설 IP 주소의 풀을 변경하십시오.
{:shortdesc}



## {{site.data.keyword.containerlong_notm}}의 네트워킹 개요
{: #basics}

{{site.data.keyword.containerlong_notm}} 클러스터의 네트워킹에 대한 기본 개념을 이해하십시오. {{site.data.keyword.containerlong_notm}}는 VLAN, 서브넷 및 IP 주소를 사용하여 클러스터 컴포넌트 네트워크 연결을 제공합니다.
{: shortdesc}

### VLAN
{: #basics_vlans}

클러스터를 작성하면 클러스터의 작업자 노드가 VLAN에 자동으로 연결됩니다. VLAN은 동일한 실제 회선에 연결된 것처럼 작업자 노드 및 팟(Pod)의 그룹을 구성하며, 작업자 및 팟(Pod) 간의 연결을 위한 채널을 제공합니다.
{: shortdesc}

<dl>
<dt>무료 클러스터의 VLAN</dt>
<dd>무료 클러스터의 경우에는 기본적으로 클러스터의 작업자 노드가 IBM 소유의 공용 VLAN 및 사설 VLAN에 연결됩니다. IBM에서 VLAN, 서브넷 및 IP 주소를 제어하므로 사용자는 다중 구역 클러스터를 작성하거나 클러스터에 서브넷을 추가할 수 없습니다. 오직 NodePort 서비스를 사용한 앱 노출만 수행할 수 있습니다.</dd>
<dt>표준 클러스터의 VLAN</dt>
<dd>표준 클러스터에서 구역의 클러스터를 처음으로 작성하는 경우, 해당 구역의 공용 VLAN 및 사설 VLAN은 IBM Cloud 인프라(SoftLayer) 계정에서 사용자를 위해 자동으로 프로비저닝됩니다. 해당 구역에서 작성하는 모든 후속 클러스터에 대해 해당 구역에서 사용할 VLAN 쌍을 지정해야 합니다. 다중 클러스터가 VLAN을 공유할 수 있으므로 사용자는 자신을 위해 작성된 동일한 공용 및 사설 VLAN을 재사용할 수 있습니다.</br>
</br>작업자 노드를 공용 VLAN 및 사설 VLAN 모두에 연결하거나 사설 VLAN에만 연결할 수 있습니다. 작업자 노드를 사설 VLAN에만 연결하려는 경우에는 기존 사설 VLAN의 ID를 사용하거나 [새 사설 VLAN을 작성](/docs/cli/reference/ibmcloud?topic=cloud-cli-manage-classic-vlans#sl_vlan_create)하여 클러스터 작성 중에 해당 ID를 사용할 수 있습니다.</dd></dl>

계정에 대해 각 구역에서 프로비저닝되는 VLAN을 보려면 `ibmcloud ks vlans --zone <zone>`을 실행하십시오. 하나의 클러스터가 프로비저닝된 VLAN을 보려면 `ibmcloud ks cluster-get --cluster <cluster_name_or_ID> --showResources`를 실행하고 **Subnet VLANs** 섹션을 찾으십시오.

IBM Cloud 인프라(SoftLayer)는 구역의 첫 번째 클러스터가 작성될 때 자동으로 프로비저닝되는 VLAN을 관리합니다. VLAN을 미사용 상태로 두면(예: VLAN에서 모든 작업자 노드를 제거하여) IBM Cloud 인프라(SoftLayer)가 VLAN을 재확보합니다. 향후에 새 VLAN이 필요하면 [contact {{site.data.keyword.Bluemix_notm}} 지원 팀에 문의](/docs/infrastructure/vlans?topic=vlans-ordering-premium-vlans#ordering-premium-vlans)하십시오.

**VLAN 결정사항을 나중에 변경할 수 있습니까?**</br>
클러스터에서 작업자 풀을 수정하여 VLAN 설정을 변경할 수 있습니다. 자세한 정보는 [작업자 노드 VLAN 연결 변경](/docs/containers?topic=containers-cs_network_cluster#change-vlans)을 참조하십시오.

### 서브넷 및 IP 주소
{: #basics_subnets}

작업자 노드 및 팟(Pod)에 추가하여 서브넷도 자동으로 VLAN에 프로비저닝됩니다. 서브넷은 클러스터 컴포넌트에 IP 주소를 지정하여 이에 대한 네트워크 연결을 제공합니다.
{: shortdesc}

다음의 서브넷이 기본 공용 및 사설 VLAN에서 자동으로 프로비저닝됩니다.

**공용 VLAN 서브넷**
* 기본 공용 서브넷은 클러스터 작성 중에 작업자 노드에 지정된 공인 IP 주소를 판별합니다. 동일한 VLAN의 다중 클러스터는 하나의 기본 공용 서브넷을 공유할 수 있습니다.
* 포터블 공용 서브넷은 하나의 클러스터에만 바인드되어 있으며 8개의 공인 IP 주소를 클러스터에 제공합니다. 3개의 IP 주소는 IBM Cloud 인프라(SoftLayer) 기능을 위해 예약됩니다. 1개의 IP는 기본 공용 Ingress ALB에 의해 사용되며, 4개의 IP는 공용 네트워크 로드 밸런서(NLB) 서비스 또는 추가 공용 ALB 작성에 사용될 수 있습니다. 포터블 공인 IP는 인터넷을 통해 NLB 또는 ALB에 액세스하기 위해 사용될 수 있는 영구적인 고정 IP 주소입니다. NLB 또는 ALB에 대해 5개 이상의 IP가 필요하면 [포터블 IP 주소 추가](/docs/containers?topic=containers-subnets#adding_ips)를 참조하십시오.

**사설 VLAN 서브넷**
* 기본 사설 서브넷은 클러스터 작성 중에 작업자 노드에 지정된 사설 IP 주소를 판별합니다. 동일한 VLAN의 다중 클러스터는 하나의 기본 사설 서브넷을 공유할 수 있습니다.
* 포터블 사설 서브넷은 하나의 클러스터에만 바인드되어 있으며 8개의 사설 IP 주소를 클러스터에 제공합니다. 3개의 IP 주소는 IBM Cloud 인프라(SoftLayer) 기능을 위해 예약됩니다. 1개의 IP는 기본 사설 Ingress ALB에 의해 사용되며, 4개의 IP는 사설 네트워크 로드 밸런서(NLB) 서비스 또는 추가 사설 ALB 작성에 사용될 수 있습니다. 포터블 사설 IP는 사설 네트워크를 통해 NLB 또는 ALB에 액세스하기 위해 사용될 수 있는 영구적인 고정 IP 주소입니다. 사설 NLB 또는 ALB에 대해 5개 이상의 IP가 필요하면 [포터블 IP 주소 추가](/docs/containers?topic=containers-subnets#adding_ips)를 참조하십시오.

계정에서 프로비저닝된 서브넷을 모두 보려면 `ibmcloud ks subnets`를 실행하십시오. 하나의 클러스터에 바인드된 포터블 공인 및 포터블 사설 서브넷을 보려면 `ibmcloud ks cluster-get --cluster <cluster_name_or_ID> --showResources`를 실행하고 **Subnet VLANs** 섹션을 찾으십시오.

{{site.data.keyword.containerlong_notm}}에서 VLAN에는 서브넷이 40개로 제한되어 있습니다. 이 한계에 도달하면 우선 [VLAN의 서브넷을 재사용하여 새 클러스터를 작성](/docs/containers?topic=containers-subnets#subnets_custom)할 수 있는지 여부를 확인하십시오. 새 VLAN이 필요하면 [{{site.data.keyword.Bluemix_notm}} 지원 팀에 문의](/docs/infrastructure/vlans?topic=vlans-ordering-premium-vlans#ordering-premium-vlans)하여 VLAN을 주문하십시오. 그런 다음, 이 새 VLAN을 사용하는 [클러스터를 작성](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_create)하십시오.
{: note}

**내 작업자 노드에 대한 IP 주소가 변경됩니까?**</br>
작업자 노드에는 클러스터가 사용하는 공용 또는 사설 VLAN의 IP 주소가 지정됩니다. 작업자 노드가 프로비저닝된 후에는 IP 주소가 변경되지 않습니다. 예를 들어, 작업자 노드 IP 주소가 `reload`, `reboot` 및 `update` 오퍼레이션에서 지속됩니다. 또한 작업자 노드의 사설 IP 주소가 대부분 `kubectl` 명령의 작업자 노드 ID에 사용됩니다. 작업자 풀이 사용하는 VLAN을 변경하는 경우 해당 풀에서 프로비저닝되는 새 작업자 노드가 IP 주소에 대해 새 VLAN을 사용합니다. 기존 작업자 노드 IP 주소가 변경되지 않지만 이전 VLAN을 사용하는 작업자 노드를 제거하도록 선택할 수 있습니다.

### 네트워크 세그먼트화
{: #basics_segmentation}

네트워크 세그먼트화는 네트워크를 다중 하위 네트워크로 분할하기 위한 접근 방식을 말합니다. 하나의 서브네트워크에서 실행되는 앱은 다른 서브네트워크의 앱을 보거나 이에 액세스할 수 없습니다. 네트워크 세그먼트화 옵션과 VLAN과 관련된 방법에 대한 자세한 정보는 [이 클러스터 보안 주제](/docs/containers?topic=containers-security#network_segmentation)를 참조하십시오.
{: shortdesc}

그러나 여러 상황에서 클러스터의 컴포넌트는 여러 개의 사설 VLAN을 통해 통신할 수 있어야 합니다. 예를 들어, 다중 구역 클러스터를 작성하려는 경우, 클러스터에 여러 개의 VLAN이 있거나 또는 동일한 VLAN에 여러 개의 서브넷이 있는 경우, 동일한 VLAN 또는 다른 VLAN의 다른 서브넷에 있는 작업자 노드는 자동으로 서로 통신할 수 없습니다. IBM Cloud 인프라(SoftLayer) 계정에 대해 VRF(Virtual Router Function) 또는 VLAN Spanning을 사용으로 설정해야 합니다.

**VRF(Virtual Router Functions)와 VLAN Spanning은 무엇입니까?**</br>

<dl>
<dt>[VRF(Virtual Router Function)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud)</dt>
<dd>VRF를 사용하면 인프라 계정의 모든 VLAN과 서브넷이 서로 통신할 수 있습니다. 또한 작업자와 마스터가 개인 서비스 엔드포인트를 통해 통신할 수 있도록 하려면 VRF가 필요합니다. VRF를 사용으로 설정하려면 [IBM Cloud 인프라(SoftLayer) 계정 담당자에게 문의](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#how-you-can-initiate-the-conversion)하십시오. VRF는 트래픽을 관리하도록 게이트웨이 디바이스를 구성하지 않는 한 모든 VLAN이 통신할 수 있으므로 계정에 대한 VLAN Spanning 옵션을 제거합니다.</dd>
<dt>[VLAN Spanning](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning)</dt>
<dd>VRF를 사용할 수 없거나 사용하지 않으려면 VLAN Spanning을 사용으로 설정하십시오. 이 조치를 수행하려면 **네트워크 > 네트워크 VLAN Spanning 관리** [인프라 권한](/docs/containers?topic=containers-users#infra_access)이 필요합니다. 또는 이를 사용으로 설정하도록 계정 소유자에게 요청할 수 있습니다. VLAN Spanning이 이미 사용으로 설정되었는지 확인하려면 `ibmcloud ks vlan-spanning-get --region <region>` [명령](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get)을 사용하십시오. VRF 대신 VLAN Spanning을 사용하도록 선택한 경우에는 개인 서비스 엔드포인트를 사용할 수 없습니다.</dd>
</dl>

**VRF 또는 VLAN Spanning은 네트워크 세그먼트화에 어떤 영향을 미칩니까?**</br>

VRF 또는 VLAN Spanning이 사용으로 설정되면 동일한 {{site.data.keyword.Bluemix_notm}} 계정에서 사설 VLAN에 연결된 시스템이 작업자와 통신할 수 있습니다. [Calico 사설 네트워크 정책](/docs/containers?topic=containers-network_policies#isolate_workers)을 적용하여 사설 네트워크의 기타 시스템으로부터 클러스터를 분리할 수 있습니다. 또한 {{site.data.keyword.containerlong_notm}}는 모든 [IBM Cloud 인프라(SoftLayer) 방화벽 오퍼링 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/cloud-computing/bluemix/network-security)과 호환 가능합니다. 사용자는 표준 클러스터에 대한 전용 네트워크 보안을 제공하고 네트워크 침입을 발견하여 이를 해결하기 위한 사용자 정의 네트워크 정책으로 방화벽(예: [Virtual Router Appliance](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-about-the-vra))을 설정할 수 있습니다.

<br />



## 기존 서브넷을 사용하여 클러스터 작성
{: #subnets_custom}

표준 클러스터를 작성하면 서브넷이 자동으로 작성됩니다. 그러나 자동으로 프로비저닝된 서브넷을 사용하는 대신에 IBM Cloud 인프라(SoftLayer) 계정에서 기존의 포터블 서브넷을 사용하거나 삭제된 클러스터의 서브넷을 재사용할 수 있습니다.
{:shortdesc}

클러스터 제거 및 작성 간에 안정된 정적 IP 주소를 유지하거나 더 큰 IP 주소 블록을 주문하려면 이 옵션을 사용하십시오. 대신 사용자의 자체 온프레미스 네트워크 서브넷을 사용하여 클러스터의 네트워크 로드 밸런서(NLB) 서비스에 대한 포터블 사설 IP 주소를 더 확보하려는 경우, [사설 VLAN에 사용자 관리 서브넷을 추가하여 포터블 사설 IP 추가](#subnet_user_managed)를 참조하십시오.

포터블 공인 IP 주소는 매월 비용이 청구됩니다. 클러스터가 프로비저닝된 후에 포터블 공인 IP 주소를 제거하는 경우에는 짧은 기간만 사용해도 여전히 월별 비용을 지불해야 합니다.
{: note}

시작하기 전에:
- [계정에 로그인하십시오. 해당되는 경우, 적절한 리소스 그룹을 대상으로 지정하십시오. 클러스터의 컨텍스트를 설정하십시오.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
- 더 이상 필요하지 않은 클러스터의 서브넷을 재사용하려면 필요하지 않은 클러스터를 삭제하십시오. 재사용하지 않으면 24시간 내에 서브넷이 삭제되므로, 새 클러스터를 즉시 작성하십시오.

   ```
   ibmcloud ks cluster-rm --cluster <cluster_name_or_ID>
   ```
   {: pre}

</br>IBM Cloud 인프라(SoftLayer) 포트폴리오의 기존 서브넷을 사용하려면 다음을 수행하십시오.

1. 사용할 서브넷의 ID와 서브넷이 위치한 VLAN의 ID를 가져오십시오.

    ```
    ibmcloud ks subnets
    ```
    {: pre}

    이 예제 출력에서 서브넷 ID는 `1602829`이며 VLAN ID는 `2234945`입니다.
    ```
        Getting subnet list...
    OK
    ID        Network             Gateway          VLAN ID   Type      Bound Cluster
    1550165   10.xxx.xx.xxx/26    10.xxx.xx.xxx    2234947   private
    1602829   169.xx.xxx.xxx/28   169.xx.xxx.xxx   2234945   public
    ```
    {: screen}

2. 식별된 VLAN ID를 사용하여 [CLI에서 클러스터를 작성](/docs/containers?topic=containers-clusters#clusters_cli_steps)하십시오. 새 포터블 공인 IP 서브넷 및 새 포터블 사설 IP 서브넷이 자동으로 작성되지 않도록 `--no-subnet` 플래그를 포함하십시오.

    ```
    ibmcloud ks cluster-create --zone dal10 --machine-type b3c.4x16 --no-subnet --public-vlan 2234945 --private-vlan 2234947 --workers 3 --name my_cluster
    ```
    {: pre}
    `--zone` 플래그에 대해 VLAN이 있는 구역을 기억할 수 없는 경우에는 `ibmcloud ks vlans --zone <zone>`을 실행하여 VLAN이 특정 구역에 있는지 여부를 확인할 수 있습니다.
    {: tip}

3.  클러스터가 작성되었는지 확인하십시오. 작업자 노드 머신이 주문되고 클러스터가 계정에서 설정 및 프로비저닝되는 데는 최대 15분 정도 걸릴 수 있습니다.

    ```
    ibmcloud ks clusters
    ```
    {: pre}

    클러스터가 완전히 프로비저닝되면 **상태(State)**가 `deployed`로 변경됩니다.

    ```
    Name         ID                                   State      Created          Workers    Zone      Version     Resource Group Name
    mycluster    aaf97a8843a29941b49a598f516da72101   deployed   20170201162433   3          dal10     1.13.6      Default
    ```
    {: screen}

4.  작업자 노드의 상태를 확인하십시오.

    ```
   ibmcloud ks workers --cluster <cluster_name_or_ID>
    ```
    {: pre}

    다음 단계를 계속하려면 우선 작업자 노드가 준비되어 있어야 합니다. **상태(State)**가 `normal`로 변경되며 **상태(Status)**는 `Ready`입니다.

    ```
    ID                                                  Public IP        Private IP     Machine Type   State      Status   Zone     Version
    prod-dal10-pa8dfcc5223804439c87489886dbbc9c07-w1    169.xx.xxx.xxx   10.xxx.xx.xxx  free           normal     Ready    dal10      1.13.6
    ```
    {: screen}

5.  서브넷 ID를 지정하여 클러스터에 서브넷을 추가하십시오. 클러스터가 서브넷을 사용할 수 있도록 하는 경우에는 사용자가 사용할 수 있는 모든 사용 가능한 포터블 공인 IP 주소가 포함된 Kubernetes configmap이 사용자를 위해 작성됩니다. 서브넷의 VLAN이 있는 구역에 Ingress ALB가 존재하지 않는 경우에는 하나의 포터블 공인 및 하나의 포터블 사설 IP 주소를 자동으로 사용하여 해당 구역에 대한 공용 및 사설 ALB를 작성합니다. 사용자는 서브넷의 기타 모든 포터블 공인 및 사설 IP 주소를 사용하여 앱에 대한 NLB 서비스를 작성할 수 있습니다.

  ```
  ibmcloud ks cluster-subnet-add --cluster <cluster_name_or_id> --subnet-id <subnet_ID>
  ```
  {: pre}

  명령 예:
  ```
  ibmcloud ks cluster-subnet-add --cluster mycluster --subnet-id 807861
  ```
  {: screen}

6. **중요**: 동일한 VLAN의 서로 다른 서브넷에 있는 작업자 간에 통신을 사용 가능하게 하려면 [동일한 VLAN의 서브넷 간에 라우팅 사용](#subnet-routing)을 설정해야 합니다.

<br />


## 기존 포터블 IP 주소 관리
{: #managing_ips}

기본적으로, 4개의 포터블 공인 및 4개의 포터블 사설 IP 주소는 [네트워크 로드 밸런서(NLB) 서비스 작성](/docs/containers?topic=containers-loadbalancer) 또는 [추가 Ingress 애플리케이션 로드 밸런서(ALB) 작성](/docs/containers?topic=containers-ingress#scale_albs)으로 단일 앱을 공용 또는 사설 네트워크에 노출시키는 데 사용될 수 있습니다. NLB 또는 ALB 서비스를 작성하려면 사용 가능한 올바른 유형의 포터블 IP 주소가 1개 이상 있어야 합니다. 사용 가능한 포터블 IP 주소를 보거나 사용된 포터블 IP 주소를 해제할 수 있습니다.
{: shortdesc}

### 사용 가능한 포터블 공인 IP 주소 보기
{: #review_ip}

사용되는 클러스터와 사용 가능한 클러스터 모두의 모든 포터블 IP 주소를 나열하려면 다음 명령을 실행할 수 있습니다.
{: shortdesc}

```
kubectl get cm ibm-cloud-provider-vlan-ip-config -n kube-system -o yaml
```
{: pre}

공용 NLB 또는 추가 공용 ALB를 작성하는 데 사용할 수 있는 포터블 공인 IP 주소만 나열하려면 다음 단계를 사용할 수 있습니다.

시작하기 전에:
-  `default` 네임스페이스에 대해 [**작성자** 또는 **관리자** {{site.data.keyword.Bluemix_notm}} IAM 서비스 역할](/docs/containers?topic=containers-users#platform)이 있는지 확인하십시오.
- [계정에 로그인하십시오. 해당되는 경우, 적절한 리소스 그룹을 대상으로 지정하십시오. 클러스터의 컨텍스트를 설정하십시오.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

사용 가능한 포터블 공인 IP 주로를 나열하려면 다음을 수행하십시오.

1.  `myservice.yaml`이라는 이름의 Kubernetes 서비스 구성 파일을 작성하고 더미 NLB IP 주소를 사용하여 `LoadBalancer` 유형의 서비스를 정의하십시오. 다음 예는 NLB IP 주소로 IP 주소 1.1.1.1을 사용합니다. `<zone>`을 사용 가능한 IP를 확인하려는 구역으로 대체하십시오.

    ```
    apiVersion: v1
    kind: Service
    metadata:
      labels:
        run: myservice
      name: myservice
      namespace: default
      annotations:
        service.kubernetes.io/ibm-load-balancer-cloud-provider-zone: "<zone>"
    spec:
      ports:
      - port: 80
        protocol: TCP
        targetPort: 80
      selector:
        run: myservice
      sessionAffinity: None
      type: LoadBalancer
      loadBalancerIP: 1.1.1.1
    ```
    {: codeblock}

2.  클러스터에 서비스를 작성하십시오.

    ```
    kubectl apply -f myservice.yaml
    ```
    {: pre}

3.  서비스를 검사하십시오.

    ```
    kubectl describe service myservice
    ```
    {: pre}

    Kubernetes 마스터가 Kubernetes configmap에서 지정된 NLB IP 주소를 찾을 수 없으므로 이 서비스의 작성에 실패합니다. 이 명령을 실행하면 오류 메시지 및 클러스터에 사용 가능한 공인 IP 주소의 목록을 볼 수 있습니다.

    ```
    Error on cloud load balancer a8bfa26552e8511e7bee4324285f6a4a for service default/myservice with UID 8bfa2655-2e85-11e7-bee4-324285f6a4af: Requested cloud provider IP 1.1.1.1 is not available. The following cloud provider IP addresses are available: <list_of_IP_addresses>
    ```
    {: screen}

<br />


### 사용된 IP 주소 해제
{: #free}

포터블 IP 주소를 사용 중인 네트워크 로드 밸런서(NLB) 서비스를 삭제하거나 Ingress 애플리케이션 로드 밸런서(ALB)를 사용 안함으로 설정하여 사용된 포터블 공인 IP 주소를 해제할 수 있습니다.
{:shortdesc}

시작하기 전에:
-  `default` 네임스페이스에 대해 [**작성자** 또는 **관리자** {{site.data.keyword.Bluemix_notm}} IAM 서비스 역할](/docs/containers?topic=containers-users#platform)이 있는지 확인하십시오.
- [계정에 로그인하십시오. 해당되는 경우, 적절한 리소스 그룹을 대상으로 지정하십시오. 클러스터의 컨텍스트를 설정하십시오.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

NLB를 삭제하거나 ALB를 사용 안함으로 설정하려면 다음을 수행하십시오.

1. 클러스터에서 사용 가능한 서비스를 나열하십시오.
    ```
    kubectl get services | grep LoadBalancer
    ```
    {: pre}

2. 공인 또는 사설 IP 주소를 사용하는 로드 밸런서를 제거하거나 ALB를 사용 안함으로 설정하십시오.
  * NLB를 삭제하려면 다음을 수행하십시오.
    ```
    kubectl delete service <service_name>
    ```
    {: pre}
  * ALB를 사용 안함으로 설정하려면 다음을 수행하십시오.
    ```
        ibmcloud ks alb-configure --albID <ALB_ID> --disable
    ```
    {: pre}

<br />


## 포터블 IP 주소 추가
{: #adding_ips}

기본적으로, 4개의 포터블 공인 및 4개의 포터블 사설 IP 주소는 [네트워크 로드 밸런서(NLB) 서비스 작성](/docs/containers?topic=containers-loadbalancer)으로 단일 앱을 공용 또는 사설 네트워크에 노출시키는 데 사용될 수 있습니다. 4개 이상의 공용 또는 4개 이상의 사설 NLB를 작성하기 위해 클러스터에 네트워크 서브넷을 추가하여 더 많은 포터블 IP 주소를 가져올 수 있습니다.
{: shortdesc}

클러스터에 서브넷을 사용 가능하게 하면 이 서브넷의 IP 주소가 클러스터 네트워킹 목적으로 사용됩니다. IP 주소 충돌을 피하려면 하나의 클러스터만 있는 서브넷을 사용해야 합니다. 동시에 {{site.data.keyword.containerlong_notm}}의 외부에서 다른 목적으로 또는 다중 클러스터에 대한 서브넷으로 사용하지 마십시오.
{: important}

### 추가로 서브넷을 주문하여 포터블 IP 추가
{: #request}

IBM Cloud 인프라(SoftLayer) 계정에서 새 서브넷을 작성하고 지정된 클러스터에서 이를 사용할 수 있도록 하여 NLB 서비스를 위한 포터블 IP를 더 가져올 수 있습니다.
{:shortdesc}

포터블 공인 IP 주소는 매월 비용이 청구됩니다. 서브넷이 프로비저닝된 후에 포터블 공인 IP 주소를 제거한 경우에는 짧은 시간 동안만 사용했어도 여전히 월별 비용을 지불해야 합니다.
{: note}

시작하기 전에:
-  클러스터에 대한 [**운영자** 또는 **관리자** {{site.data.keyword.Bluemix_notm}} IAM 플랫폼 역할](/docs/containers?topic=containers-users#platform)이 있는지 확인하십시오.
- [계정에 로그인하십시오. 해당되는 경우, 적절한 리소스 그룹을 대상으로 지정하십시오. 클러스터의 컨텍스트를 설정하십시오.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

서브넷을 주문하려면 다음을 수행하십시오.

1. 새 서브넷을 프로비저닝하십시오.

    ```
    ibmcloud ks cluster-subnet-create --cluster <cluster_name_or_id> --size <subnet_size> --vlan <VLAN_ID>
    ```
    {: pre}

    <table>
    <caption>이 명령의 컴포넌트 이해</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 이 명령의 컴포넌트 이해</th>
    </thead>
    <tbody>
    <tr>
    <td><code><em>&lt;cluster_name_or_id&gt;</em></code></td>
    <td><code>&lt;cluster_name_or_id&gt;</code>를 클러스터의 이름 또는 ID로 대체하십시오.</td>
    </tr>
    <tr>
    <td><code><em>&lt;subnet_size&gt;</em></code></td>
    <td><code>&lt;subnet_size&gt;</code>를 포터블 서브넷에서 작성할 IP 주소의 수로 대체하십시오. 허용되는 값은 8, 16, 32 또는 64입니다. <p class="note"> 서브넷에 대한 포터블 IP 주소를 추가하는 경우에는 세 개의 IP 주소를 사용하여 클러스터 내부 네트워킹을 설정합니다. Ingress 애플리케이션 로드 밸런서(ALB)에 대해 또는 네트워크 로드 밸런서(NLB) 서비스를 작성하는 데 세 개의 IP 주소를 사용할 수 없습니다. 예를 들어, 8개의 포터블 공인 IP 주소를 요청하는 경우 이 중에서 5개를 사용하여 앱을 공용으로 노출할 수 있습니다.</p> </td>
    </tr>
    <tr>
    <td><code><em>&lt;VLAN_ID&gt;</em></code></td>
    <td><code>&lt;VLAN_ID&gt;</code>를 포터블 공인 또는 사설 IP 주소를 할당할 공용 또는 사설 VLAN의 ID로 대체하십시오. 기존 작업자 노드가 연결되어 있는 공용 또는 사설 VLAN을 선택해야 합니다. 작업자 노드가 연결되는 공용 또는 사설 VLAN을 검토하려면 <code>ibmcloud ks cluster-get --cluster &lt;cluster&gt; --showResources</code>를 실행하고 출력에서 <strong>서브넷 VLAN</strong> 섹션을 찾으십시오. 서브넷은 VLAN이 있는 동일한 구역에서 프로비저닝됩니다.</td>
    </tr>
    </tbody></table>

2. 서브넷이 정상적으로 작성되어 클러스터에 추가되었는지 확인하십시오. 서브넷 CIDR은 **Subnet VLANs** 섹션에 나열되어 있습니다.

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

3. **중요**: 동일한 VLAN의 서로 다른 서브넷에 있는 작업자 간에 통신을 사용 가능하게 하려면 [동일한 VLAN의 서브넷 간에 라우팅 사용](#subnet-routing)을 설정해야 합니다.

<br />


### 사설 VLAN에 사용자 관리 서브넷을 추가하여 포터블 사설 IP 추가
{: #subnet_user_managed}

클러스터에 사용 가능한 온프레미스 네트워크에서 서브넷을 작성하여 네트워크 로드 밸런서(NLB) 서비스에 대한 추가 포터블 사설 IP를 가져올 수 있습니다.
{:shortdesc}

대신 IBM 클라우드 인프라(SoftLayer) 계정에서 기존 포터블 서브넷을 다시 사용하시겠습니까? [사용자 정의 또는 기존 IBM Cloud 인프라(SoftLayer) 서브넷을 사용하여 클러스터 작성](#subnets_custom)을 참조하십시오.
{: tip}

요구사항:
- 사용자가 관리하는 서브넷은 사설 VLAN에만 추가할 수 있습니다.
- 서브넷 접두부 길이 한계는 /24 - /30입니다. 예를 들면, `169.xx.xxx.xxx/24`는 253개의 사용 가능한 사설 IP 주소를 지정하지만 `169.xx.xxx.xxx/30`은 1개의 사용 가능한 사설 IP 주소를 지정합니다.
- 서브넷의 첫 번째 IP 주소는 서브넷에 대한 게이트웨이로 사용되어야 합니다.

시작하기 전에:
- 외부 서브넷에 들어오고 나가는 네트워크 트래픽의 라우팅을 구성하십시오.
- 온프레미스 데이터센터 게이트웨이 디바이스와 사설 네트워크 가상 라우터 어플라이언스 또는 클러스터에서 실행되는 ngSwan VPN 서비스 간의 VPN 연결이 있는지 확인하십시오. 자세한 정보는 [VPN 연결 설정](/docs/containers?topic=containers-vpn)을 참조하십시오.
-  클러스터에 대한 [**운영자** 또는 **관리자** {{site.data.keyword.Bluemix_notm}} IAM 플랫폼 역할](/docs/containers?topic=containers-users#platform)이 있는지 확인하십시오.
- [계정에 로그인하십시오. 해당되는 경우, 적절한 리소스 그룹을 대상으로 지정하십시오. 클러스터의 컨텍스트를 설정하십시오.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)


온프레미스 네트워크의 서브넷을 추가하려면 다음을 수행하십시오.

1. 클러스터의 사설 VLAN의 ID를 보십시오. **Subnet VLANs** 섹션을 찾으십시오. **User-managed** 필드에서 _false_인 VLAN ID를 식별하십시오.

    ```
    ibmcloud ks cluster-get --showResources <cluster_name>
    ```
    {: pre}

    ```
Subnet VLANs
  VLAN ID   Subnet CIDR         Public   User-managed
  2234947   10.xxx.xx.xxx/29    false    false
  2234945   169.xx.xxx.xxx/29  true    false
    ```
    {: screen}

2. 사설 VLAN에 외부 서브넷을 추가하십시오. 포터블 사설 IP 주소가 클러스터의 configmap에 추가됩니다.

    ```
    ibmcloud ks cluster-user-subnet-add --cluster <cluster_name> --subnet-cidr <subnet_CIDR> --private-vlan <private_VLAN>
    ```
    {: pre}

    예:

    ```
    ibmcloud ks cluster-user-subnet-add --cluster mycluster --subnet-cidr 10.xxx.xx.xxx/24 --private-vlan 2234947
    ```
    {: pre}

3. 사용자 제공 서브넷이 추가되었는지 확인하십시오. **User-managed** 필드가 _true_입니다.

    ```
    ibmcloud ks cluster-get --showResources <cluster_name>
    ```
    {: pre}

    이 예제 출력에서는 두 번째 서브넷이 `2234947` 사설 VLAN에 추가되었습니다.
    ```
    Subnet VLANs
    VLAN ID   Subnet CIDR       Public   User-managed
    2234947   10.xxx.xx.xxx/29  false    false
    2234945   169.xx.xxx.xxx/29 true     false
    2234947   10.xxx.xx.xxx/24  false    true
    ```
    {: screen}

4. [동일한 VLAN의 서브넷 간에 라우팅 사용](#subnet-routing)을 수행하십시오.

5. [사설 네트워크 로드 밸런서(NLB) 서비스](/docs/containers?topic=containers-loadbalancer)를 추가하거나 [사설 Ingress ALB](/docs/containers?topic=containers-ingress#private_ingress)를 사용하여 사설 네트워크를 통해 앱에 액세스하십시오. 사용자가 추가한 서브넷의 사설 IP 주소를 사용하려면 서브넷 CIDR의 IP 주소를 지정해야 합니다. 그렇지 않으면 IBM Cloud 인프라(SoftLayer) 서브넷 또는 사설 VLAN의 사용자 제공 서브넷에서 랜덤으로 IP 주소가 선택됩니다.

<br />


## 서브넷 라우팅 관리
{: #subnet-routing}

클러스터용 다중 VLAN, 동일한 VLAN의 다중 서브넷 또는 다중 구역 클러스터가 있는 경우에는 작업자 노드가 사설 네트워크에서 서로 간에 통신할 수 있도록 IBM Cloud 인프라(SoftLayer) 계정에 대해 [Virtual Router Function (VRF)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud)을 사용으로 설정해야 합니다. VRF를 사용으로 설정하려면 [IBM Cloud 인프라(SoftLayer) 계정 담당자에게 문의](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#how-you-can-initiate-the-conversion)하십시오. VRF를 사용할 수 없거나 사용하지 않으려면 [VLAN Spanning](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning)을 사용으로 설정하십시오. 이 조치를 수행하려면 **네트워크 > 네트워크 VLAN Spanning 관리** [인프라 권한](/docs/containers?topic=containers-users#infra_access)이 필요합니다. 또는 이를 사용으로 설정하도록 계정 소유자에게 요청할 수 있습니다. VLAN Spanning이 이미 사용으로 설정되었는지 확인하려면 `ibmcloud ks vlan-spanning-get --region <region>` [명령](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get)을 사용하십시오. 

VLAN Spanning이 역시 필요한 다음의 시나리오를 검토하십시오.

### 동일한 VLAN의 기본 서브넷 간에 라우팅 사용
{: #vlan-spanning}

클러스터를 작성하면 기본 공용 및 사설 서브넷이 공용 및 사설 VLAN에 프로비저닝됩니다. 기본 공용 서브넷은 `/28`로 끝나며 작업자 노드에 대한 14개의 공인 IP를 제공합니다. 기본 사설 서브넷은 `/26`으로 끝나며 최대 62개의 작업자 노드에 대한 사설 IP를 제공합니다.
{:shortdesc}

동일한 VLAN의 동일한 위치에 있는 대형 클러스터 또는 여러 개의 소형 클러스터를 사용하여 작업자 노드에 대한 초기 14개의 공인 및 62개의 사설 IP를 초과할 수 있습니다. 공용 또는 사설 서브넷이 작업자 노드의 한계에 도달하면 동일한 VLAN의 다른 기본 서브넷이 주문됩니다.

동일한 VLAN의 이러한 기본 서브넷에서 작업자가 통신할 수 있도록 보장하려면 VLAN Spanning을 켜야 합니다. 지시사항은 [VLAN Spanning 사용 또는 사용 안함](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning)을 참조하십시오.

VLAN Spanning이 이미 사용으로 설정되었는지 확인하려면 `ibmcloud ks vlan-spanning-get --region <region>` [명령](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get)을 사용하십시오.
{: tip}

### 게이트웨이 디바이스에 대한 서브넷 라우팅 관리
{: #vra-routing}

표준 클러스터를 작성하면 클러스터가 연결된 VLAN에서 포터블 공인 및 포터블 사설 서브넷이 주문됩니다. 이러한 서브넷은 Ingress 애플리케이션 로드 밸런서(ALB) 및 네트워크 로드 밸런서(NLB) 서비스에 대한 IP 주소를 제공합니다.
{: shortdesc}

그러나 [VRA(Virtual Router Appliance)](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-about-the-vra#about-the-vra) 등의 기존 라우터 어플라이언스가 있는 경우에는 클러스터가 연결된 해당 VLAN의 새로 추가된 포터블 서브넷이 라우터에 구성되어 있지 않습니다. NLB 또는 Ingress ALB를 사용하려면 [VLAN Spanning을 사용으로 설정](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning)하여 네트워크 디바이스가 동일한 VLAN의 서로 다른 서브넷 간에 라우팅할 수 있도록 보장해야 합니다.

VLAN Spanning이 이미 사용으로 설정되었는지 확인하려면 `ibmcloud ks vlan-spanning-get --region <region>` [명령](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get)을 사용하십시오.
{: tip}
