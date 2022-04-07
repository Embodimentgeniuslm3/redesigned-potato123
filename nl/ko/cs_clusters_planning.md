---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: kubernetes, iks, multi az, multi-az, szr, mzr

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


# 클러스터 네트워크 설정 계획
{: #plan_clusters}

자신의 워크로드 및 환경 요구사항을 만족시키는 {{site.data.keyword.containerlong}} 클러스터의 네트워크 설정을 디자인하십시오.
{: shortdesc}

{{site.data.keyword.containerlong_notm}} 클러스터에서, 사용자의 컨테이너화된 앱은 작업자 노드라는 컴퓨팅 호스트에서 호스팅됩니다. 작업자 노드는 Kubernetes 마스터가 관리합니다. 작업자 노드와 Kubernetes 마스터, 기타 서비스, 인터넷 또는 기타 사설 네트워크 간의 통신은 IBM Cloud 인프라(SoftLayer) 네트워크를 어떻게 설정하느냐에 따라 달라집니다. 

클러스터를 처음으로 작성해 보십니까? 먼저 이 [튜토리얼](/docs/containers?topic=containers-cs_cluster_tutorial)을 수행하고, 프로덕션용으로 사용한 가능 클러스터를 계획할 준비가 되면 여기로 돌아오십시오.
{: tip}

클러스터 네트워크 설정을 계획하려면 먼저 [클러스터 네트워크의 기본 사항을 이해](#plan_basics)해야 합니다. 그 후에는 [인터넷 연결 앱 워크로드 실행](#internet-facing), [제한된 공용 액세스를 허용하는 온프레미스 데이터 센터 확장](#limited-public), [사설 네트워크 전용 온프레미스 데이터 센터 확장](#private_clusters)을 포함하는 환경 기반 시나리오에 적합한, 세 가지 가능한 클러스터 네트워크 설정을 검토할 수 있습니다. 

## 클러스터 네트워크 기본 사항 이해
{: #plan_basics}

클러스터를 작성할 때는 특정 클러스터 컴포넌트가 서로 통신할 수 있도록, 그리고 클러스터 외부의 네트워크 또는 서비스와도 통신할 수 있도록 네트워킹 설정을 선택해야 합니다.
{: shortdesc}

* [작업자 간 통신](#worker-worker): 모든 작업자 노드는 사설 네트워크에서 서로 통신할 수 있어야 합니다. 많은 경우에는 VLAN 및 구역이 다른 여러 작업자가 서로 연결할 수 있도록 여러 사설 VLAN 간에 통신이 허용되어야 합니다. 
* [작업자 대 마스터 및 사용자 대 마스터 통신](#workeruser-master): 작업자 노드와 권한 부여된 클러스터 사용자는 TLS를 사용하는 공용 네트워크를 통해, 또는 개인 서비스 엔드포인트를 통해 Kubernetes 마스터와 안전하게 통신할 수 있습니다. 
* [작업자와 기타 {{site.data.keyword.Bluemix_notm}} 서비스 또는 온프레미스 네트워크 간의 통신](#worker-services-onprem): 작업자 노드가 {{site.data.keyword.registrylong}} 등의 기타 {{site.data.keyword.Bluemix_notm}} 서비스, 그리고 온프레미스 네트워크와 안전하게 통신할 수 있도록 하십시오. 
* [작업자 노드에서 실행되는 앱에 대한 외부 통신](#external-workers): 클러스터에 공용 및 개인용 요청을 전달하거나, 클러스터에서 공용 엔드포인트로 요청이 전달될 수 있도록 하십시오. 

### 작업자 간 통신
{: #worker-worker}

클러스터를 작성하면 클러스터의 작업자 노드가 사설 VLAN에 자동으로 연결되며 선택적으로 공용 VLAN에 연결됩니다. VLAN은 동일한 실제 회선에 연결된 것처럼 작업자 노드 및 팟(Pod)의 그룹을 구성하며, 작업자 간의 연결을 위한 채널을 제공합니다.
{: shortdesc}

**작업자 노드를 위한 VLAN 연결**</br>
모든 작업자 노드는 각 작업자 노드가 다른 작업자 노드에 정보를 전송하고 수신할 수 있도록 사설 VLAN에 연결되어야 합니다. 공용 VLAN에도 연결된 작업자 노드를 포함하는 클러스터를 작성하는 경우 작업자 노드는 자동으로 공용 VALN을 통해 Kubernetes 마스터와 통신할 수 있으며, 개인 서비스 엔드포인트를 사용으로 설정하는 경우에는 사설 VLAN을 통해서도 통신할 수 있습니다. 공용 VLAN은 클러스터의 앱을 인터넷에 노출할 수 있도록 공용 인터넷 연결도 제공합니다. 그러나 공용 인터페이스로부터 앱을 보호해야 하는 경우에는 Calico 네트워크 정책을 사용하거나 외부 네트워크 워크로드를 에지 작업자 노드로 격리하는 등과 같이 클러스터를 보호하는 데 사용할 수 있는 몇 가지 옵션이 있습니다. 
* 무료 클러스터: 무료 클러스터의 경우에는 기본적으로 클러스터의 작업자 노드가 IBM 소유의 공용 VLAN 및 사설 VLAN에 연결됩니다. IBM에서 VLAN, 서브넷 및 IP 주소를 제어하므로 사용자는 다중 구역 클러스터를 작성하거나 클러스터에 서브넷을 추가할 수 없습니다. 오직 NodePort 서비스를 사용한 앱 노출만 수행할 수 있습니다.</dd>
* 표준 클러스터: 표준 클러스터에서 구역의 클러스터를 처음으로 작성하는 경우, 해당 구역의 공용 VLAN 및 사설 VLAN은 IBM Cloud 인프라(SoftLayer) 계정에서 사용자를 위해 자동으로 프로비저닝됩니다. 해당 작업자 노드가 사설 VLAN에만 연결되도록 지정하는 경우에는 해당 구역의 사설 VLAN만 자동으로 프로비저닝됩니다. 사용자는 해당 구역에서 작성하는 모든 후속 클러스터에 대해 사용할 VLAN 쌍을 지정할 수 있습니다. 다중 클러스터가 VLAN을 공유할 수 있으므로 사용자는 자신을 위해 작성된 동일한 공용 및 사설 VLAN을 재사용할 수 있습니다.

VLAN, 서브넷 및 IP 주소에 대한 자세한 정보는 [{{site.data.keyword.containerlong_notm}}의 네트워킹 개요](/docs/containers?topic=containers-subnets#basics)를 참조하십시오. 

**여러 서브넷 및 VLAN 간 작업자 노드 통신**</br>
몇 가지 상황에서는 클러스터의 컴포넌트가 여러 사설 VLAN과 통신할 수 있도록 허용되어야 합니다. 예를 들어, 다중 구역 클러스터를 작성하려는 경우, 클러스터에 여러 개의 VLAN이 있거나 또는 동일한 VLAN에 여러 개의 서브넷이 있는 경우, 동일한 VLAN 또는 다른 VLAN의 다른 서브넷에 있는 작업자 노드는 자동으로 서로 통신할 수 없습니다. 사용자는 IBM Cloud 인프라(SoftLayer) 계정에 대해 VRF(Virtual Routing and Forwarding) 또는 VLAN Spanning을 사용으로 설정해야 합니다. 

* [VRF(Virtual Routing and Forwarding)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud): VRF는 인프라 계정의 모든 사설 VLAN과 서브넷이 서로 통신할 수 있도록 합니다. 또한 작업자 및 마스터가 개인 서비스 엔드포인트를 통해 통신하고, 개인 서비스 엔드포인트를 지원하는 다른 {{site.data.keyword.Bluemix_notm}} 인스턴스와도 통신할 수 있도록 하려면 VRF가 필요합니다. VRF를 사용으로 설정하려면 `ibmcloud account update --service-endpoint-enable true`를 실행하십시오. 이 명령 출력은 계정에서 VRF 및 서비스 엔드포인트를 사용할 수 있도록 설정하는 지원 케이스를 열 것인지 묻는 프롬프트를 표시합니다. VRF는 모든 VLAN이 통신할 수 있으므로 이를 사용하면 계정에 대한 VLAN Spanning 옵션을 제거합니다. </br></br>
VRF가 사용으로 설정되면 동일한 {{site.data.keyword.Bluemix_notm}} 계정에서 사설 VLAN에 연결된 모든 시스템이 클러스터 작업자 노드와 통신할 수 있습니다. [Calico 사설 네트워크 정책](/docs/containers?topic=containers-network_policies#isolate_workers)을 적용하여 사설 네트워크의 기타 시스템으로부터 클러스터를 분리할 수 있습니다.</dd>
* [VLAN Spanning](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning): 사설 네트워크에서 마스터에 액세스할 필요가 없는 경우 또는 게이트웨이 디바이스를 사용하여 공용 VLAN을 통해 마스터에 액세스하는 경우와 같이, VRF를 사용으로 설정할 수 없거나 이를 사용하지 않으려는 경우에는 VLAN Spanning을 사용으로 설정하십시오. 예를 들어 기존 게이트웨이 디바이스가 있으며 클러스터를 추가하는 경우, 클러스터에 대해 주문된 새 포터블 서브넷은 게이트웨이 디바이스에 구성되어 있지 않지만 VLAN Spanning은 이러한 서브넷 간의 라우팅을 가능하게 합니다. VLAN Spanning을 사용으로 설정하려면 **네트워크 > 네트워크 VLAN Spanning 관리** [인프라 권한](/docs/containers?topic=containers-users#infra_access)이 필요하며, 계정 소유자에게 이를 사용으로 설정하도록 요청할 수도 있습니다. VLAN Spanning이 이미 사용으로 설정되었는지 확인하려면 `ibmcloud ks vlan-spanning-get` [명령](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get)을 사용하십시오. VRF 대신 VLAN Spanning을 사용하도록 선택하는 경우에는 개인 서비스 엔드포인트를 사용으로 설정할 수 없습니다. 

</br>

### 작업자 대 마스터 및 사용자 대 마스터 통신
{: #workeruser-master}

작업자 노드가 Kubernetes 마스터와의 연결을 설정할 수 있도록 통신 채널을 설정해야 합니다. 공용 서비스 엔드포인트 또는 개인 서비스 엔드포인트만, 또는 공용 및 개인 서비스 엔드포인트를 함께 사용으로 설정하여 작업자 노드와 Kubernetes 마스터가 통신하도록 할 수 있습니다.
{: shortdesc}

공용 및 개인 서비스 엔드포인트를 통한 통신을 보호하기 위해 {{site.data.keyword.containerlong_notm}}는 클러스터가 작성될 때 Kubernetes 마스터와 작업자 노드 간의 OpenVPN 연결을 자동으로 설정합니다. 작업자는 TLS 인증서를 통해 마스터와 안전하게 통신하고 마스터는 OpenVPN 연결을 통해 작업자와 통신합니다. 

**공용 서비스 엔드포인트만**</br>
계정에서 VRF를 사용하지 않으려 하거나 사용할 수 없는 경우, 작업자 노드는 공용 서비스 엔드포인트를 통해 공용 VLAN을 거쳐 Kubernetes 마스터와 자동으로 연결할 수 있습니다. 
* 작업자 노드와 마스터 간의 통신은 공용 서비스 엔드포인트를 통해 공용 네트워크를 거쳐 안전하게 설정됩니다. 
* 마스터는 권한 부여된 클러스터 사용자만 공용 서비스 엔드포인트를 통해 공개적으로 액세스할 수 있습니다. 클러스터 사용자는 인터넷을 통해 Kubernetes 마스터에 안전하게 액세스하여 `kubectl` 명령(예)을 실행할 수 있습니다.

**공용 및 개인 서비스 엔드포인트**</br>
클러스터 사용자가 마스터에 공개적으로, 또는 개인적으로 액세스할 수 있도록 하려면 공용 및 개인 서비스 엔드포인트를 사용으로 설정할 수 있습니다. 사용자의 {{site.data.keyword.Bluemix_notm}} 계정에서 VRF를 사용해야 하며 서비스 엔드포인트를 사용할 수 있도록 계정을 설정해야 합니다. VRF 및 서비스 엔드포인트를 사용으로 설정하려면 `ibmcloud account update --service-endpoint-enable true`를 실행하십시오. 
* 작업자 노드가 공용 및 사설 VLAN에 연결된 경우, 작업자 노드와 마스터 간의 통신은 개인 서비스를 엔드포인트를 통한 사설 네트워크를 통해 설정되는 동시에 공용 서비스 엔드포인트를 통한 공용 네트워크를 통해서도 설정됩니다. 공용 엔드포인트를 통한 작업자와 마스터 간의 트래픽 절반과 개인 엔드포인트를 통핸 절반을 라우팅하여 마스터와 작업자 간의 통신은 공용 또는 사설 네트워크의 잠재적인 가동 중단으로부터 보호됩니다. 작업자 노드가 사설 VLAN에만 연결된 경우에는 작업자 노드와 마스터 간의 통신이 개인 서비스 엔드포인트를 통한 사설 네트워크를 통해서만 설정됩니다. 
* 마스터는 권한 부여된 클러스터 사용자가 공용 서비스 엔드포인트를 통해 공개적으로 액세스할 수 있습니다. 권한 부여된 클러스터 사용자가 {{site.data.keyword.Bluemix_notm}} 사설 네트워크에 있거나, VPN 연결 또는 {{site.data.keyword.Bluemix_notm}} Direct Link를 통해 사설 네트워크에 연결되어 있는 경우에는 개인 서비스 엔드포인트를 통해 개인적으로 마스터에 액세스할 수 있습니다. 사용자가 VPN 또는 {{site.data.keyword.Bluemix_notm}} Direct Link 연결을 통해 마스터에 액세스할 수 있도록 [사설 로드 밸런서를 통해 마스터 엔드포인트를 노출](/docs/containers?topic=containers-clusters#access_on_prem)해야 한다는 점을 유의하십시오. 

**개인 서비스 엔드포인트만**</br>
마스터에 개인적으로만 액세스할 수 있도록 하려는 경우에는 개인 서비스 엔드포인트를 사용으로 설정할 수 있습니다. 사용자의 {{site.data.keyword.Bluemix_notm}} 계정에서 VRF를 사용해야 하며 서비스 엔드포인트를 사용할 수 있도록 계정을 설정해야 합니다. VRF 및 서비스 엔드포인트를 사용으로 설정하려면 `ibmcloud account update --service-endpoint-enable true`를 실행하십시오. 개인 서비스 엔드포인트만 사용하면 대역폭에 대해 비용이 청구되지 않는다는 점을 유의하십시오. 
* 작업자 노드와 마스터 간의 통신은 개인 서비스 엔드포인트를 통해 사설 네트워크를 거쳐 설정됩니다. 
* 마스터는 권한 부여된 클러스터 사용자가 {{site.data.keyword.Bluemix_notm}} 사설 네트워크에 있거나, VPN 연결 또는 Direct Link를 통해 사설 네트워크에 연결되어 있는 경우 개인적으로 액세스할 수 있습니다. 사용자가 VPN 또는 Direct Link 연결을 통해 마스터에 액세스할 수 있도록 [사설 로드 밸런서를 통해 마스터 엔드포인트를 노출](/docs/containers?topic=containers-clusters#access_on_prem)해야 한다는 점을 유의하십시오. 

</br>

### 작업자와 기타 {{site.data.keyword.Bluemix_notm}} 서비스 또는 온프레미스 네트워크 간의 통신
{: #worker-services-onprem}

작업자 노드가 {{site.data.keyword.registrylong}} 등의 기타 {{site.data.keyword.Bluemix_notm}} 서비스, 그리고 온프레미스 네트워크와 안전하게 통신할 수 있도록 하십시오.
{: shortdesc}

**사설 또는 공용 네트워크를 통한 기타 {{site.data.keyword.Bluemix_notm}} 서비스와의 통신**</br>
작업자 노드는 IBM Cloud 인프라(SoftLayer) 사설 네트워크를 통해 개인 서비스 엔드포인트를 지원하는 기타 {{site.data.keyword.Bluemix_notm}} 서비스({{site.data.keyword.registrylong}} 등)와 자동으로 안전하게 통신할 수 있습니다. {{site.data.keyword.Bluemix_notm}} 서비스가 개인 서비스 엔드포인트를 지원하지 않는 경우 작업자 노드는 공용 네트워크를 통해 서비스와 안전하게 통신할 수 있도록 공용 VLAN에 연결되어야 합니다. 

클러스터 내 공용 네트워크를 잠그기 위해 Calico 네트워크 정책을 사용하는 경우에는 Calico 정책에서 사용하려는 서비스의 공인 및 사설 IP 주소에 대한 액세스를 허용해야 할 수 있습니다. 게이트웨이 디바이스(Virtual Router Appliance(Vyatta) 등)를 사용하는 경우에는 게이트웨이 디바이스 방화벽에서 [사용하려는 서비스의 사설 IP 주소에 대한 액세스를 허용](/docs/containers?topic=containers-firewall#firewall_outbound)해야 합니다.
{: note}

**온프레미스 데이터 센터에 있는 리소스와의 사설 네트워크를 통한 통신을 위한 {{site.data.keyword.BluDirectLink}}**</br>
클러스터를 온프레미스 데이터 센터({{site.data.keyword.icpfull_notm}} 등)에 연결하려는 경우에는 [{{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link)를 설정할 수 있습니다. {{site.data.keyword.Bluemix_notm}} Direct Link를 사용하면 공용 인터넷을 통한 라우팅 없이 원격 네트워크 환경과 {{site.data.keyword.containerlong_notm}} 간에 개인용 직접 연결을 작성할 수 있습니다. 

**온프레미스 데이터 센터에 있는 리소스와의 공용 네트워크를 통한 연결을 위한 strongSwan IPSec VPN 연결**
* 공용 및 사설 VLAN에 연결된 작업자 노드: 클러스터에서 직접 [strongSwan IPSec VPN 서비스 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.strongswan.org/about.html)를 설정하십시오. strongSwan IPSec VPN 서비스는 업계 표준 IPSec(Internet Protocol Security) 프로토콜 스위트를 기반으로 하는 인터넷 상의 엔드-투-엔드 보안 통신 채널을 제공합니다. 클러스터와 온프레미스 네트워크 간의 보안 연결을 설정하려면 클러스터 내의 팟(Pod)에 직접 [strongSwan IPSec VPN 서비스를 구성하고 배치](/docs/containers?topic=containers-vpn#vpn-setup)하십시오.
* 사설 VLAN에만 연결된 작업자 노드: 게이트웨이 디바이스(Virtual Router Appliance(Vyatta) 등)에서 IPSec VPN 엔드포인트를 설정하십시오. 그 후 게이트웨이의 VPN 엔드포인트를 사용하도록 클러스터에서 [strongSwan IPSec VPN 서비스를 구성](/docs/containers?topic=containers-vpn#vpn-setup)하십시오. strongSwan을 사용하지 않으려면 [VPN 연결을 VRA와 직접 설정](/docs/containers?topic=containers-vpn#vyatta)할 수 있습니다.

</br>

### 작업자 노드에서 실행되는 앱에 대한 외부 통신
{: #external-workers}

클러스터 외부로부터 작업자 노드에서 실행되는 앱으로의 공용 또는 개인용 트래픽 요청을 허용하십시오.
{: shortdesc}

**클러스터 앱에 대한 개인용 트래픽**</br>
사용자가 클러스터에 앱을 배치할 때, 클러스터와 동일한 사설 네트워크에 있는 사용자 및 서비스에서만 앱에 액세스할 수 있게 설정하고자 할 수 있습니다. 사설 로드 밸런싱은 앱을 일반 사용자에게 노출하지 않고 클러스터 외부의 요청에서 앱을 사용할 수 있도록 하는 데 적합합니다. 나중에 공용 네트워크 서비스를 사용하여 앱을 공용으로 노출하기 전에 액세스, 요청 라우팅 및 기타 구성을 테스트하기 위해 사설 로드 밸런싱도 사용할 수 있습니다. 클러스터 외부에서 앱으로의 개인용 트래픽 요청을 허용하려는 경우에는 사설 NodePort, NLB 및 Ingress ALB와 같은 사설 Kubernetes 네트워킹 서비스를 작성할 수 있습니다. 그 후에는 Calico Pre-DNAT 정책을 사용하여 사설 네트워킹 서비스의 NodePort에 대한 트래픽을 차단할 수 있습니다. 자세한 정보는 [사설 외부 로드 밸런싱 계획](/docs/containers?topic=containers-cs_network_planning#private_access)을 참조하십시오. 

**클러스터 앱에 대한 공용 트래픽**</br>
외부의 공용 인터넷에서 앱에 액세스할 수 있도록 하려는 경우에는 공용 NodePort, 네트워크 로드 밸런서(NLB) 및 Ingress 애플리케이션 로드 밸런서(ALB)를 작성할 수 있습니다. 공용 네트워킹 서비스는 앱에 공인 IP 주소 및 공용 URL(선택사항)을 제공하여 이 공용 네트워크 인터페이스에 연결합니다. 앱이 공용으로 노출되면 공용 서비스 IP 주소 또는 사용자 앱에 맞게 설정한 URL을 갖고 있는 사용자는 요청을 앱에 전송할 수 있습니다. 그 후에는 특정 소스 IP 주소 또는 CIDR에서의 트래픽을 화이트리스트 지정하고 모든 기타 트래픽을 차단하는 등과 같이 Calico Pre-DNAT 정책을 사용하여 공용 네트워킹 서비스에 대한 트래픽을 제어할 수 있습니다. 자세한 정보는 [공용 외부 로드 밸런싱 계획](/docs/containers?topic=containers-cs_network_planning#private_access)을 참조하십시오. 

추가적인 보안을 위해 네트워킹 워크로드를 에지 작업자 노드로 격리하십시오. 에지 작업자 노드는 공용 VLAN에 연결된 작업자 노드 중 외부에서 액세스되는 노드의 수를 줄이고 네트워킹 워크로드를 격리함으로써 클러스터의 보안을 향상시킬 수 있습니다. [작업자 노드를 에지 노드로 레이블 지정](/docs/containers?topic=containers-edge#edge_nodes)하면 NLB 및 ALB 팟(Pod)이 이렇게 지정된 작업자 노드에만 배치됩니다. 에지 노드에서 다른 워크로드가 실행되는 것 또한 방지하기 위해 [에지 노드를 오염](/docs/containers?topic=containers-edge#edge_workloads)시킬 수 있습니다. Kubernetes 버전 1.14 이상에서는 에지 노드에 사설 NLB 및 ALB를 모두 배치할 수 있습니다.
예를 들어, 작업자 노드가 사설 VLAN에만 연결되었으나 클러스터에 있는 앱에 대한 공용 액세스를 허용해야 하는 경우에는 에지 노드가 공용 및 사설 VLAN에 연결된 에지 작업자 풀을 작성할 수 있습니다. 사용자는 이러한 작업자만 공용 연결을 처리하도록 이러한 에지 노드에 공용 NLB 및 ALB를 배치할 수 있습니다. 

작업자 노드가 사설 VLAN에만 연결되었으며 게이트웨이 디바이스를 사용하여 작업자 노드와 클러스터 마스터 간에 연결을 제공하는 경우에는 디바이스를 공용 또는 개인용 방화벽으로 구성할 수도 있습니다. 클러스터 외부에서 앱으로의 공용 또는 개인용 트래픽 요청을 허용하려는 경우에는 사설 NodePort, NLB 및 Ingress ALB를 작성할 수 있습니다. 그 후에는 이러한 서비스에 대한, 공용 또는 사설 네트워크를 통한 인바운드 트래픽을 허용하기 위해 게이트웨이 디바이스 방화벽에서 [필수 포트 및 IP 주소를 열어야](/docs/containers?topic=containers-firewall#firewall_inbound) 합니다.
{: note}

<br />


## 시나리오: 클러스터에서 인터넷 연결 앱 워크로드 실행
{: #internet-facing}

이 시나리오에서는 일반 사용자가 앱에 액세스할 수 있도록 인터넷에서의 요청이 액세스 가능한 클러스터에서 워크로드를 실행하고자 합니다. 여기서는 클러스터에서 공용 액세스를 격리하고 클러스터에 허용되는 공용 요청을 제어합니다. 또한 작업자가 클러스터에 연결할 {{site.data.keyword.Bluemix_notm}} 서비스에 대한 액세스 권한을 자동으로 획득합니다.
{: shortdesc}

<p>
<figure>
 <img src="images/cs_clusters_planning_internet.png" alt="인터넷 연결 워크로드를 실행하는 클러스터의 아키텍처 이미지"/>
 <figcaption>인터넷 연결 워크로드를 실행하는 클러스터의 아키텍처</figcaption>
</figure>
</p>

이 설정을 수행하기 위해, 사용자는 작업자 노드를 공용 및 사설 VLAN에 연결하여 클러스터를 작성합니다. 

공용 및 사설 VLAN를 모두 사용하여 클러스터를 작성하면 나중에 해당 클러스터에서 공용 VLAN을 제거할 수 없습니다. 클러스터에서 모든 공용 VLAN을 제거하면 여러 클러스터 컴포넌트의 작동이 중지됩니다. 이 경우에는 사설 VLAN에만 연결된 새 작업자 풀을 작성하십시오.
{: note}

작업자 대 마스터 및 사용자 대 마스터 통신을 공용 및 사설 네트워크를 통해 허용할지, 또는 공용 네트워크를 통해서만 허용할지 선택할 수 있습니다. 
* 공용 및 개인 서비스 엔드포인트: 계정이 VRF 및 서비스 엔드포인트를 사용하도록 설정되어야 합니다. 작업자 노드와 마스터 간의 통신은 사설 서비스 엔드포인트를 통한 사설 네트워크와 공용 서비스 엔드포인트를 통한 공용 네트워크 모두를 통해 설정됩니다. 마스터는 권한 부여된 클러스터 사용자가 공용 서비스 엔드포인트를 통해 공개적으로 액세스할 수 있습니다. 
* 공용 서비스 엔드포인트: 계정에서 VRF를 사용하지 않으려 하거나 사용할 수 없는 경우, 작업자 노드 및 권한 부여된 클러스터 사용자는 공용 서비스 엔드포인트를 통해 공용 네트워크를 거쳐 Kubernetes 마스터와 자동으로 연결할 수 있습니다. 

작업자 노드는 개인 서비스 엔드포인트를 지원하는 기타 {{site.data.keyword.Bluemix_notm}} 서비스와 IBM Cloud 인프라(SoftLayer) 사설 네트워크를 통해 자동으로 안전하게 통신할 수 있습니다. {{site.data.keyword.Bluemix_notm}} 서비스가 개인 서비스 엔드포인트를 지원하지 않는 경우 작업자 노드는 공용 네트워크를 통해 서비스와 안전하게 통신할 수 있습니다. 공용 네트워크 또는 사설 네트워크 격리를 위해 Calico 네트워크 정책을 사용하여 작업자 노드의 공용 또는 사설 인터페이스를 잠글 수 있습니다. 사용자는 이러한 Calico 격리 정책에서 사용하려는 서비스의 공인 및 사설 IP 주소에 대한 액세스를 허용해야 할 수 있습니다. 

클러스터에 있는 앱을 인터넷에 노출하려는 경우에는 공용 네트워크 로드 밸런서(NLB) 또는 Ingress 애플리케이션 로드 밸런서(ALB) 서비스를 작성할 수 있습니다. 에지 노드로 레이블 지정된 작업자 노드의 풀을 작성하여 클러스터의 보안을 향상시킬 수 있습니다. 외부 트래픽 워크로드가 클러스터에 있는 소수의 작업자로 격리되도록 공용 네트워크 서비스를 위한 팟(Pod)이 에지 노드에 배치됩니다. 화이트리스트 및 블랙리스트 정책과 같은 Calico Pre-DNAT 정책을 작성하여 앱을 노출하는 네트워크 서비스에 대한 공용 트래픽을 추가로 제어할 수 있습니다. 

작업자 노드가 {{site.data.keyword.Bluemix_notm}} 계정 외부의 사설 네트워크에 있는 서비스에 액세스해야 하는 경우에는 클러스터에 strongSwan IPSec VPN 서비스를 구성하여 배치하거나 {{site.data.keyword.Bluemix_notm}} {{site.data.keyword.Bluemix_notm}} Direct Link 서비스를 이용하여 이러한 네트워크에 연결할 수 있습니다. 

이 시나리오의 클러스터를 시작할 준비가 되셨습니까? [고가용성](/docs/containers?topic=containers-ha_clusters) 및 [작업자 노드](/docs/containers?topic=containers-planning_worker_nodes) 설정을 계획한 후 [클러스터 작성](/docs/containers?topic=containers-clusters#cluster_prepare)을 참조하십시오. 

<br />


## 시나리오: 온프레미스 데이터 센터를 사설 네트워크의 클러스터로 확장하고 제한된 공용 액세스 추가
{: #limited-public}

이 시나리오에서는 온프레미스 데이터 센터에 있는 서비스, 데이터베이스 및 기타 리소스에서 액세스할 수 있는 클러스터에서 워크로드를 실행하고자 합니다. 그러나 클러스터에 제한된 공용 액세스를 제공해야 할 수도 있으며, 이 경우에는 클러스터에서 모든 공용 액세스를 제어 및 격리하려 합니다. 예를 들면, 작업자가 개인 서비스 엔드포인트를 지원하지 않는 {{site.data.keyword.Bluemix_notm}} 서비스에 공용 네트워크를 통해 액세스해야 할 수 있습니다. 또는 클러스터에서 실행되는 앱에 대해 제한된 공용 액세스를 제공해야 할 수도 있습니다.
{: shortdesc}

이 클러스터 설정을 수행하기 위해, [에지 노드 및 Calico 네트워크 정책을 사용](#calico-pc)하거나 [게이트웨이 디바이스를 사용](#vyatta-gateway)하여 방화벽을 작성할 수 있습니다. 

### 에지 노드 및 Calico 네트워크 정책 사용
{: #calico-pc}

에지 노드를 공용 게이트웨이로, Calico 네트워크 정책을 공용 방화벽으로 사용하여 클러스터에 대해 제한된 공용 연결을 허용하십시오.
{: shortdesc}

<p>
<figure>
 <img src="images/cs_clusters_planning_calico.png" alt="안전한 공용 액세스를 위해 에지 노드 및 Calico 네트워크 정책을 사용하는 클러스터의 아키텍처 이미지"/>
 <figcaption>안전한 공용 액세스를 위해 에지 노드 및 Calico 네트워크 정책을 사용하는 클러스터의 아키텍처</figcaption>
</figure>
</p>

이 설정에서는 작업자 노드를 사설 VLAN에만 연결하여 클러스터를 작성합니다. 사용자의 계정이 VRF 및 개인 서비스 엔드포인트를 사용하도록 설정되어야 합니다. 

Kubernetes 마스터는 권한 부여된 클러스터 사용자가 {{site.data.keyword.Bluemix_notm}} 사설 네트워크에 있거나, [VPN 연결](/docs/infrastructure/iaas-vpn?topic=VPN-gettingstarted-with-virtual-private-networking) 또는 [{{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link)를 통해 사설 네트워크에 연결되어 있는 경우 개인 서비스 엔드포인트를 통해 액세스할 수 있습니다. 그러나 개인 서비스 엔드포인트를 통한 Kubernetes 마스터와의 연결은 VPN 연결 또는 {{site.data.keyword.Bluemix_notm}} Direct Link를 통해 라우팅할 수 없는 <code>166.X.X.X</code> IP 주소 범위를 통해 설정되어야 합니다. 사용자는 클러스터 사용자를 위해 사설 네트워크 로드 밸런서(NLB)를 사용하여 개인 서비스 엔드포인트를 노출할 수 있습니다. 사설 NLB는 사용자가 VPN 또는 {{site.data.keyword.Bluemix_notm}} Direct Link 연결로 액세스할 수 있는 내부 <code>10.X.X.X</code> IP 주소로 마스터의 개인 서비스 엔드포인트를 노출합니다. 개인 서비스 엔드포인트만 사용으로 설정하는 경우에는 Kubernetes 대시보드를 사용하거나 임시로 공용 서비스 엔드포인트를 사용으로 설정하여 사설 NLB를 작성할 수 있습니다. 

그 다음에는 공용 및 사설 VLAN에 연결되었으며 에지 노드로 레이블 지정된 작업자 노드의 풀을 작성할 수 있습니다. 에지 노드는 소수의 작업자 노드만 외부에서 액세스할 수 있도록 하고 네트워킹 워크로드를 이러한 작업자로 격리함으로써 클러스터의 보안을 향상시킬 수 있습니다. 

작업자 노드는 개인 서비스 엔드포인트를 지원하는 기타 {{site.data.keyword.Bluemix_notm}} 서비스와 IBM Cloud 인프라(SoftLayer) 사설 네트워크를 통해 자동으로 안전하게 통신할 수 있습니다. {{site.data.keyword.Bluemix_notm}} 서비스가 개인 서비스 엔드포인트를 지원하지 않는 경우 공용 VLAN에 연결된 에지 노드는 공용 네트워크를 통해 서비스와 안전하게 통신할 수 있습니다. 공용 네트워크 또는 사설 네트워크 격리를 위해 Calico 네트워크 정책을 사용하여 작업자 노드의 공용 또는 사설 인터페이스를 잠글 수 있습니다. 사용자는 이러한 Calico 격리 정책에서 사용하려는 서비스의 공인 및 사설 IP 주소에 대한 액세스를 허용해야 할 수 있습니다. 

클러스터에 있는 앱에 대한 개인용 액세스를 제공하려는 경우에는 사설 네트워크 로드 밸런서(NLB) 또는 Ingress 애플리케이션 로드 밸런서(ALB)를 작성하여 앱을 사설 네트워크에만 노출할 수 있습니다. 작업자 노드의 공용 NodePort를 차단하는 정책과 같은 Calico Pre-DNAT 정책을 작성하여 앱을 노출하는 네트워크 서비스에 대한 모든 공용 트래픽을 차단할 수 있습니다. 클러스터에 있는 앱에 대해 제한된 공용 액세스를 제공해야 하는 경우에는 공용 NLB 또는 ALB를 작성하여 앱을 노출할 수 있습니다. 그 후에는 NLB 및 ALB가 앱 팟(Pod)으로 공용 트래픽을 경로 지정할 수 있도록 앱을 이러한 에지 노드에 배치해야 합니다. 화이트리스트 및 블랙리스트 정책과 같은 Calico Pre-DNAT 정책을 작성하여 앱을 노출하는 네트워크 서비스에 대한 공용 트래픽을 추가로 제어할 수 있습니다. 외부 트래픽 워크로드가 클러스터에 있는 소수의 작업자에게로 제한되도록 공용 및 사설 네트워크 서비스를 위한 팟(Pod)이 에지 노드에 배치됩니다.   

{{site.data.keyword.Bluemix_notm}} 외부의 서비스 및 기타 온프레미스 네트워크에 안전하게 액세스하기 위해 클러스터에 strongSwan IPSec VPN 서비스를 구성하고 배치할 수 있습니다. strongSwan 로드 밸런서 팟(Pod)은 에지 풀의 작업자에 배치되며, 해당 팟(Pod)은 공용 네트워크를 통해 암호화된 VPN 터널을 거쳐 온프레미스 네트워크에 대한 안전한 연결을 설정합니다. 또는  {{site.data.keyword.Bluemix_notm}} Direct Link 서비스를 사용하여 클러스터를 사설 네트워크로만 온프레미스 데이터 센터에 연결할 수 있습니다. 

이 시나리오의 클러스터를 시작할 준비가 되셨습니까? [고가용성](/docs/containers?topic=containers-ha_clusters) 및 [작업자 노드](/docs/containers?topic=containers-planning_worker_nodes) 설정을 계획한 후 [클러스터 작성](/docs/containers?topic=containers-clusters#cluster_prepare)을 참조하십시오. 

</br>

### 게이트웨이 디바이스 사용
{: #vyatta-gateway}

Virtual Router Appliance(Vyatta)를 공용 게이트웨이 및 방화벽으로 구성하여 클러스터에 대한 제한된 공용 연결을 허용하십시오.
{: shortdesc}

<p>
<figure>
 <img src="images/cs_clusters_planning_gateway.png" alt="안전한 공용 액세스를 위해 게이트웨이 디바이스를 사용하는 클러스터의 아키텍처 이미지"/>
 <figcaption>안전한 공용 액세스를 위해 게이트웨이 디바이스를 사용하는 클러스터의 아키텍처</figcaption>
</figure>
</p>

사설 VLAN에서만 작업자 노드를 설정했으며 계정에서 VRF를 사용으로 설정할 수 없거나 사용하지 않으려는 경우에는 공용 네트워크를 통해 작업자 노드와 마스터 간에 네트워크 연결을 제공하기 위해 게이트웨이 디바이스를 구성해야 합니다. 예를 들면, [Virtual Router Appliance](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-about-the-vra) 또는 [Fortigate Security Appliance](/docs/services/vmwaresolutions/services?topic=vmware-solutions-fsa_considerations)를 설정하도록 선택할 수 있습니다.

사용자는 클러스터에 전용 네트워크 보안을 제공하고, 네트워크 침입을 발견 및 해결하기 위해 사용자 정의 네트워크 정책을 사용하는 게이트웨이 디바이스를 설정할 수 있습니다. 공용 네트워크에 대해 방화벽을 설정할 때는 마스터와 작업자 노드가 통신할 수 있도록 각 지역에 대해 필수 포트와 사설 IP 주소를 열어야 합니다. 이 방화벽을 사설 네트워크에 대해서도 구성하는 경우에는 작업자 노드가 서로 통신하고 클러스터가 사설 네트워크를 통해 인프라 리소스에 액세스할 수 있도록 필수 포트 및 사설 IP 주소도 열어야 합니다. 또한 서브넷이 동일한 VLAN 및 다른 VLAN으로 라우팅할 수 있도록 계정에 대해 VLAN Spanning을 사용으로 설정해야 합니다. 

작업자 노드 및 앱을 온프레미스 네트워크 또는 {{site.data.keyword.Bluemix_notm}} 외부의 서비스에 안전하게 연결하려면 게이트웨이 디바이스에서 IPSec VPN 엔드포인트를 설정하고 클러스터에서 VPN 엔드포인트를 사용하도록 strongSwan IPSec VPN 서비스를 설정하십시오. strongSwan을 사용하지 않으려는 경우에는 VRA와의 직접 VPN 연결을 설정할 수 있습니다. 

작업자 노드는 게이트웨이 디바이스를 통해 기타 {{site.data.keyword.Bluemix_notm}} 서비스, 그리고 {{site.data.keyword.Bluemix_notm}} 외부의 공용 서비스와 안전하게 통신할 수 있습니다. 사용자는 사용할 서비스의 공인 또는 사설 IP 주소에 대해서만 액세스를 허용하도록 방화벽을 구성할 수 있습니다. 

클러스터에 있는 앱에 대한 개인용 액세스를 제공하려는 경우에는 사설 네트워크 로드 밸런서(NLB) 또는 Ingress 애플리케이션 로드 밸런서(ALB)를 작성하여 앱을 사설 네트워크에만 노출할 수 있습니다. 클러스터에 있는 앱에 대해 제한된 공용 액세스를 제공해야 하는 경우에는 공용 NLB 또는 ALB를 작성하여 앱을 노출할 수 있습니다. 모든 트래픽이 게이트웨이 디바이스 방화벽을 통과하므로, 앱을 노출하는 네트워크 서비스에 대한 인바운드 트래픽을 허용하기 위해 방화벽에 이러한 서비스의 포트 및 IP 주소를 열어 해당 서비스에 대한 공용 및 개인용 트래픽을 제어할 수 있습니다. 

이 시나리오의 클러스터를 시작할 준비가 되셨습니까? [고가용성](/docs/containers?topic=containers-ha_clusters) 및 [작업자 노드](/docs/containers?topic=containers-planning_worker_nodes) 설정을 계획한 후 [클러스터 작성](/docs/containers?topic=containers-clusters#cluster_prepare)을 참조하십시오. 

<br />


## 시나리오: 온프레미스 데이터 센터를 사설 네트워크의 클러스터로 확장
{: #private_clusters}

이 시나리오에서는 {{site.data.keyword.containerlong_notm}} 클러스터의 워크로드를 실행하고자 합니다. 그러나 이러한 워크로드를 온프레미스 데이터 센터({{site.data.keyword.icpfull_notm}} 등)에 있는 서비스, 데이터베이스 또는 기타 리소스에서만 액세스할 수 있도록 하려 합니다. 클러스터 워크로드가 사설 네트워크를 통한 통신을 지원하는 몇몇 다른 {{site.data.keyword.Bluemix_notm}} 서비스({{site.data.keyword.cos_full_notm}} 등)에 액세스해야 할 수도 있습니다.
{: shortdesc}

<p>
<figure>
 <img src="images/cs_clusters_planning_extend.png" alt="사설 네트워크의 온프레미스 데이터 센터에 연결하는 클러스터의 아키텍처 이미지"/>
 <figcaption>사설 네트워크의 온프레미스 데이터 센터에 연결하는 클러스터의 아키텍처</figcaption>
</figure>
</p>

이 설정을 수행하기 위해, 사용자는 작업자 노드를 사설 VLAN에만 연결하여 클러스터를 작성합니다. 개인 서비스 엔드포인트를 통해서만 사설 네트워크를 통해 클러스터 마스터와 작업자 노드 간 연결을 제공하려면 계정이 VRF 및 서비스 엔드포인트를 사용하도록 설정되어야 합니다. VRF가 사용으로 설정되면 사설 네트워크의 모든 리소스에서 클러스터를 볼 수 있으므로, Calico 사설 네트워크 정책을 적용하여 사설 네트워크의 다른 시스템으로부터 클러스터를 격리시킬 수 있습니다. 

Kubernetes 마스터는 권한 부여된 클러스터 사용자가 {{site.data.keyword.Bluemix_notm}} 사설 네트워크에 있거나, [VPN 연결](/docs/infrastructure/iaas-vpn?topic=VPN-gettingstarted-with-virtual-private-networking) 또는 [{{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link)를 통해 사설 네트워크에 연결되어 있는 경우 개인 서비스 엔드포인트를 통해 액세스할 수 있습니다. 그러나 개인 서비스 엔드포인트를 통한 Kubernetes 마스터와의 연결은 VPN 연결 또는 {{site.data.keyword.Bluemix_notm}} Direct Link를 통해 라우팅할 수 없는 <code>166.X.X.X</code> IP 주소 범위를 통해 설정되어야 합니다. 사용자는 클러스터 사용자를 위해 사설 네트워크 로드 밸런서(NLB)를 사용하여 개인 서비스 엔드포인트를 노출할 수 있습니다. 사설 NLB는 사용자가 VPN 또는 {{site.data.keyword.Bluemix_notm}} Direct Link 연결로 액세스할 수 있는 내부 <code>10.X.X.X</code> IP 주소로 마스터의 개인 서비스 엔드포인트를 노출합니다. 개인 서비스 엔드포인트만 사용으로 설정하는 경우에는 Kubernetes 대시보드를 사용하거나 임시로 공용 서비스 엔드포인트를 사용으로 설정하여 사설 NLB를 작성할 수 있습니다. 

작업자 노드는 IBM Cloud 인프라(SoftLayer) 사설 네트워크를 통해 개인 서비스 엔드포인트를 지원하는 기타 {{site.data.keyword.Bluemix_notm}} 서비스({{site.data.keyword.registrylong}} 등)와 자동으로 안전하게 통신할 수 있습니다. 예를 들어, {{site.data.keyword.cloudant_short_notm}}의 모든 표준 플랜 인스턴스에 대한 전용 하드웨어 환경은 개인 서비스 엔드포인트를 지원합니다. {{site.data.keyword.Bluemix_notm}} 서비스가 개인 서비스 엔드포인트를 지원하지 않는 경우에는 클러스터가 해당 서비스에 액세스할 수 없습니다. 

클러스터에 있는 앱에 대한 개인용 액세스를 제공하려는 경우에는 사설 네트워크 로드 밸런서(NLB) 또는 Ingress 애플리케이션 로드 밸런서(ALB)를 작성할 수 있습니다. 이러한 Kubernetes 네트워크 서비스는 NLB IP가 있는 서브넷에 연결된 온프레미스 시스템이 앱에 액세스할 수 있도록 앱을 사설 네트워크에만 노출합니다. 

이 시나리오의 클러스터를 시작할 준비가 되셨습니까? [고가용성](/docs/containers?topic=containers-ha_clusters) 및 [작업자 노드](/docs/containers?topic=containers-planning_worker_nodes) 설정을 계획한 후 [클러스터 작성](/docs/containers?topic=containers-clusters#cluster_prepare)을 참조하십시오. 
