---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-10"

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


# VPN 연결 설정
{: #vpn}

VPN 연결을 사용하면 {{site.data.keyword.containerlong}}에서 Kubernetes 클러스터의 앱을 온프레미스 네트워크에 안전하게 연결할 수 있습니다. 클러스터 내에서 실행 중인 앱에 클러스터 외부의 앱을 연결할 수도 있습니다.
{:shortdesc}

작업자 노드 및 앱을 온프레미스 데이터센터에 연결하려면 다음 옵션 중 하나를 구성할 수 있습니다.

- **strongSwan IPSec VPN 서비스**: Kubernetes 클러스터를 온프레미스 네트워크와 안전하게 연결하는 [strongSwan IPSec VPN 서비스 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.strongswan.org/about.html)를 설정할 수 있습니다. strongSwan IPSec VPN 서비스는 업계 표준 IPSec(Internet Protocol Security) 프로토콜 스위트를 기반으로 하는 인터넷 상의 엔드-투-엔드 보안 통신 채널을 제공합니다. 클러스터와 온프레미스 네트워크 간의 보안 연결을 설정하려면 클러스터 내의 팟(Pod)에 직접 [strongSwan IPSec VPN 서비스를 구성하고 배치](#vpn-setup)하십시오.

- **{{site.data.keyword.BluDirectLink}}**: 공용 인터넷을 라우팅하지 않고 [{{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-about-ibm-cloud-direct-link)를 통해 원격 네트워크 환경과 {{site.data.keyword.containerlong_notm}} 간의 직접적인 사설 연결을 작성할 수 있습니다. {{site.data.keyword.Bluemix_notm}} Direct Link 오퍼링은 하이브리드 워크로드, 교차 제공자 워크로드, 대규모 또는 빈번한 데이터 전송자 또는 개인용 워크로드를 구현해야 하는 경우 유용합니다. {{site.data.keyword.Bluemix_notm}} Direct Link 오퍼링을 선택하고 {{site.data.keyword.Bluemix_notm}} Direct Link 연결을 설정하려면 {{site.data.keyword.Bluemix_notm}} Direct Link 문서의 [IBM Cloud {{site.data.keyword.Bluemix_notm}} Direct Link 시작하기](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link#how-do-i-know-which-type-of-ibm-cloud-direct-link-i-need-)를 참조하십시오.

- **가상 라우터 어플라이언스(VRA) 또는 Fortigate 보안 어플라이언스(FSA)**: [VRA(Vyatta)](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-about-the-vra) 또는 [FSA](/docs/services/vmwaresolutions/services?topic=vmware-solutions-fsa_considerations)를 설정하여 IPSec VPN 엔드포인트를 구성하도록 선택할 수 있습니다. 이 옵션은 대규모 클러스터가 있으며 단일 VPN 에서 다중 클러스터에 액세스하려는 경우 또는 라우트 기반 VPN이 필요한 경우에 유용합니다. VRA를 구성하려면 [VRA를 사용하여 VPN 연결 설정](#vyatta)을 참조하십시오.

## strongSwan IPSec VPN 서비스 Helm 차트 사용
{: #vpn-setup}

Helm 차트를 사용하여 Kubernetes 팟(Pod) 내부에 strongSwan IPSec VPN 서비스를 구성 및 배치하십시오.
{:shortdesc}

strongSwan은 클러스터와 통합되어 있으므로 외부 게이트웨이 디바이스가 필요하지 않습니다. VPN 연결이 설정되면 클러스터 내의 모든 작업자 노드에서 라우트가 자동으로 구성됩니다. 이러한 라우트는 작업자 노드와 원격 시스템 간에 VPN 터널을 통한 양방향 연결을 설정할 수 있게 해 줍니다. 예를 들면, 다음 다이어그램은 {{site.data.keyword.containerlong_notm}}의 앱이 strongSwan VPN 연결을 통해 온프레미스 서버와 통신할 수 있는 방법을 보여줍니다.

<img src="images/cs_vpn_strongswan.png" width="700" alt="네트워크 로드 밸런서(NLB)를 사용하여 {{site.data.keyword.containerlong_notm}}에 앱 노출" style="width:700px; border-style: none"/>

1. 클러스터 내의 앱 `myapp`이 Ingress 또는 LoadBalancer 서비스로부터 요청을 수신하여 온프레미스 네트워크의 데이터와 안전하게 연결해야 합니다.

2. 온프레미스 데이터센터에 대한 요청이 IPSec strongSwan VPN 팟(Pod)에 전달됩니다. 대상 IP 주소는 IPSec strongSwan VPN 팟(Pod)에 전송할 네트워크 패킷을 판별하는 데 사용됩니다.

3. 요청이 암호화되어 VPN 터널을 통해 온프레미스 데이터센터에 전송됩니다.

4. 수신 요청이 온프레미스 방화벽을 통과해 VPN 터널 엔드포인트(라우터)에 전달되어 여기서 복호화됩니다.

5. VPN 터널 엔드포인트(라우터)는 2단계에서 지정된 대상 IP 주소에 따라 요청을 온프레미스 서버 또는 메인프레임에 전달합니다. 필요한 데이터는 동일한 프로세스를 거쳐 VPN 연결을 통해 `myapp`에 다시 전송됩니다.

## strongSwan VPN 서비스 고려사항
{: #strongswan_limitations}

strongSwan Helm 차트를 사용하기 전에 다음 고려사항 및 제한사항을 검토하십시오.
{: shortdesc}

* strongSwan Helm 차트에서는 NAT 순회가 원격 VPN 엔드포인트에 의해 사용되어야 함을 요구합니다. NAT 순회에서는 500의 기본 IPSec UDP 포트 외에도 UDP 포트 4500이 필요합니다. 두 UDP 포트 모두는 구성된 방화벽을 통해 허용되어야 합니다.
* strongSwan Helm 차트는 라우트 기반 IPSec VPN을 지원하지 않습니다.
* strongSwan Helm 차트는 사전 공유된 키를 사용하는 IPSec VPN은 지원하지만 인증서가 필요한 IPSec VPN은 지원하지 않습니다.
* strongSwan Helm 차트는 다중 클러스터 및 기타 IaaS 리소스의 단일 VPN 연결 공유를 허용하지 않습니다.
* strongSwan Helm 차트는 클러스터 내의 Kubernetes 팟(Pod)으로서 실행됩니다. VPN 성능은 Kubernetes 및 클러스터에 실행 중인 기타 팟(Pod)의 메모리와 네트워크 사용에 따라 영향을 받습니다. 성능이 중요한 환경을 보유 중이면 전용 하드웨어의 클러스터 외부에서 실행되는 VPN 솔루션의 사용을 고려하십시오.
* strongSwan Helm 차트는 IPSec 터널 엔드포인트로서 단일 VPN 팟(Pod)을 실행합니다. 팟(Pod)이 실패하면 클러스터가 팟(Pod)을 다시 시작합니다. 그러나 새 팟(Pod)이 시작되고 VPN 연결이 다시 설정되는 동안에는 짧은 작동 중지 시간이 발생할 수 있습니다. 보다 빠른 오류 복구 또는 보다 정교한 고가용성 솔루션이 필요한 경우에는 전용 하드웨어의 클러스터 외부에서 실행되는 VPN 솔루션의 사용을 고려하십시오.
* strongSwan Helm 차트는 VPN 연결을 통해 전달되는 네트워크 트래픽의 메트릭 또는 모니터링을 제공하지 않습니다. 지원되는 모니터링 도구의 목록은 [로깅 및 모니터링 서비스](/docs/containers?topic=containers-supported_integrations#health_services)를 참조하십시오.

클러스터 사용자는 strongSwan VPN 서비스를 사용하여 개인 서비스 엔드포인트를 통해 Kubernetes 마스터에 연결할 수 있습니다. 그러나 개인 서비스 엔드포인트를 통한 Kubernetes 마스터와의 통신은 VPN 연결에서 라우트되지 않는 <code>166.X.X.X</code> IP 주소 범위를 통해 수행되어야 합니다. [사설 네트워크 로드 밸런서(NLB) 사용](/docs/containers?topic=containers-clusters#access_on_prem)을 통해 클러스터 사용자에게 적합한 마스터의 개인 서비스 엔드포인트를 노출할 수 있습니다. 사설 NLB는 strongSwan VPN 팟(Pod)이 액세스할 수 있는 내부 `172.21.x.x` 클러스터 IP 주소로 마스터의 개인 서비스 엔드포인트를 노출합니다. 개인 서비스 엔드포인트만 사용으로 설정하는 경우에는 Kubernetes 대시보드를 사용하거나 임시로 공용 서비스 엔드포인트를 사용으로 설정하여 사설 NLB를 작성할 수 있습니다.
{: tip}

<br />


## 다중 구역 클러스터에서 strongSwan VPN 구성
{: #vpn_multizone}

다중 구역 클러스터는 다중 구역의 작업자 노드에서 앱 인스턴스를 사용할 수 있도록 하여 가동 중단 시 앱에 대한 고가용성을 제공합니다. 그러나, 다중 구역 클러스터에서 strongSwan VPN 서비스를 구성하는 것은 단일 구역 클러스터에서 strongSwan을 구성하는 것보다 훨씬 복잡합니다.
{: shortdesc}

다중 구역 클러스터에서 strongSwan을 구성하기 전에 먼저 strongSwan Helm 차트를 단일 구역 클러스터에 배치하십시오. 단일 구역 클러스터와 온프레미스 네트워크 간의 VPN 연결을 먼저 설정하면 다중 구역 strongSwan 구성에 중요한 원격 네트워크 방화벽 설정을 보다 다음과 같이 쉽게 판별할 수 있습니다.
* 일부 원격 VPN 엔드포인트는 `ipsec.conf` 파일에서 `leftid` 또는 `rightid`와 같은 설정을 사용합니다. 이러한 설정이 있으면 VPN IPSec 터널의 IP 주소로 `leftid`를 설정해야 하는지 여부를 확인하십시오.
*	연결이 원격 네트워크에서 클러스터에 인바운드인 경우, 한 구역에서 로드 밸런서가 실패하는 경우에 원격 VPN 엔드포인트가 VPN 연결을 다른 IP 주소로 다시 설정할 수 있는지 확인하십시오.

다중 구역 클러스터에서 strongSwan을 시작하려면 다음 옵션 중 하나를 선택하십시오.
* 아웃바운드 VPN 연결을 사용할 수 있는 경우, 하나의 strongSwan VPN 배치만 구성하도록 선택할 수 있습니다. [다중 구역 클러스터에서 하나의 아웃바운드 VPN 연결 구성](#multizone_one_outbound)을 참조하십시오.
* 인바운드 VPN 연결이 필요한 경우, 사용할 수 있는 구성 설정은 가동 중단이 감지될 때 다른 공용 로드 밸런서 IP에 대한 VPN 연결을 재설정하도록 원격 VPN 엔드포인트를 구성할 수 있는지 여부에 따라 다릅니다.
  * 원격 VPN 엔드포인트가 다른 IP로 VPN 연결을 자동으로 재설정할 수 있는 경우, 하나의 strongSwan VPN 배치만 구성하도록 선택할 수 있습니다. [다중 구역 클러스터에 하나의 인바운드 VPN 연결 구성](#multizone_one_inbound)을 참조하십시오.
  * 원격 VPN 엔드포인트가 다른 IP로 VPN 연결을 자동으로 재설정할 수 없는 경우 각 구역에 별도의 인바운드 strongSwan VPN 서비스를 배치해야 합니다. [다중 구역 클러스터의 각 구역에서 VPN 연결 구성](#multizone_multiple)을 참조하십시오.

다중 구역 클러스터에 대한 아웃바운드 또는 인바운드 VPN 연결을 위해 하나의 strongSwan VPN 배치만 필요하도록 환경을 설정하십시오. 각 구역에 별도의 strongSwan VPN을 설정해야 하는 경우, 이 추가된 복잡도와 증가된 리소스 사용량을 관리할 수 있는 방법을 계획해야 합니다.
{: note}

### 다중 구역 클러스터에서 단일 아웃바운드 VPN 연결 구성
{: #multizone_one_outbound}

다중 구역 클러스터에서 strongSwan VPN 서비스를 구성하는 가장 간단한 솔루션은 클러스터의 모든 가용성 구역에서 서로 다른 작업자 노드 간에 이동하는 단일 아웃바운드 VPN 연결을 사용하는 것입니다.
{: shortdesc}

VPN 연결이 다중 구역 클러스터에서 아웃바운드인 경우 하나의 strongSwan 배치만 필요합니다. 작업자 노드가 제거되거나 작동중지시간이 발생하면 `kubelet`가 VPN 팟(Pod)를 새 작업자 노드로 다시 스케줄합니다. 가용성 구역에서 가동 중단이 발생하는 경우, `kubelet`는 VPN 팟(Pod)을 다른 구역의 새 작업자 노드로 다시 스케줄합니다.

1. [하나의 strongSwan VPN Helm 차트를 구성](/docs/containers?topic=containers-vpn#vpn_configure)하십시오. 해당 절의 단계를 수행하는 경우 다음 설정을 지정해야 합니다.
    - `ipsec.auto`: `start`로 변경하십시오. 연결은 클러스터에서 아웃바운드입니다.
    - `loadBalancerIP`: IP 주소를 지정하지 마십시오. 이 설정을 공백으로 두십시오.
    - `zoneLoadBalancer`: 작업자 노드가 있는 각 구역의 공용 로드 밸런서 IP 주소를 지정합니다. [사용 가능한 공인 IP 주소를 볼 수 있는지 확인](/docs/containers?topic=containers-subnets#review_ip)하거나 [사용된 IP 주소를 해제](/docs/containers?topic=containers-subnets#free)할 수 있습니다. strongSwan VPN이 임의의 구역에 있는 작업자 노드로 스케줄될 수 있으므로 이 IP 목록은 로드 밸런서 IP가 VPN 팟(Pod)이 스케줄된 구역에서 사용될 수 있도록 보장합니다.
    - `connectUsingLoadBalancerIP`: `true`로 설정하십시오. strongSwan VPN이 작업자 노드에 스케줄되면 strongSwan 서비스는 동일한 구역에 있는 로드 밸런서 IP 주소를 선택하고 이 IP를 사용하여 아웃바운드 연결을 설정합니다.
    - `local.id`: 원격 VPN 엔드포인에서 지원하는 고정 값을 지정합니다. 원격 VPN 엔드포인트에서 `local.id` 옵션(`ipsec.conf`의 `leftid` 값)을 VPN IPSec 터널의 공인 IP 주소로 설정해야 하는 경우 `local.id`를 `%loadBalancerIP`로 설정하십시오. 이 값은 `ipsec.conf`의 `leftid` 값을 연결에 사용되는 로드 밸런서 IP 주소로 자동 구성합니다.

2. 원격 네트워크 방화벽에서는 `zoneLoadBalancer` 설정에 나열된 공인 IP 주소로부터의 수신 IPSec VPN 연결을 허용합니다.

3. 원격 VPN 엔드포인트를 구성하여 `zoneLoadBalancer` 설정에 나열된 가능한 로드 밸런서 IP 각각으로부터의 수신 VPN 연결을 허용합니다.

### 다중 구역 클러스터에서 단일 인바운드 VPN 연결 구성
{: #multizone_one_inbound}

수신 VPN 연결이 필요하고 장애가 발견 시 원격 VPN 엔드포인트에서 다른 IP로 VPN 연결을 자동으로 재설정할 수 있는 경우, 클러스터의 모든 가용성 구역에서 서로 다른 작업자 노드 간에 이동하는 단일 인바운드 VPN 연결을 사용할 수 있습니다.
{: shortdesc}

원격 VPN 엔드포인트는 모든 구역의 모든 strongSwan 로드 밸런서에 VPN 연결을 설정할 수 있습니다. 수신 요청은 VPN 팟(Pod)이 있는 구역에 관계없이 VPN 팟(Pod)으로 전송됩니다. VPN 팟(Pod)의 응답은 원래 로드 밸런서를 통해 원격 VPN 엔드포인트로 다시 전송됩니다. 이 옵션은 `kubelet`에서 작업자 노드가 제거되거나 작동중지시간이 발생하면 VPN 팟(Pod)을 새 작업자 노드로 다시 스케줄하므로 고가용성을 보장합니다. 또한 가용성 구역에서 가동 중단이 발생하는 경우, 원격 VPN 엔드포인트는 VPN 팟(Pod)에 계속 연결할 수 있도록 다른 구역의 로드 밸런서 IP 주소로 VPN 연결을 재설정할 수 있습니다.

1. [하나의 strongSwan VPN Helm 차트를 구성](/docs/containers?topic=containers-vpn#vpn_configure)하십시오. 해당 절의 단계를 수행하는 경우 다음 설정을 지정해야 합니다.
    - `ipsec.auto`: `add`로 변경하십시오. 연결은 클러스터에 인바운드입니다.
    - `loadBalancerIP`: IP 주소를 지정하지 마십시오. 이 설정을 공백으로 두십시오.
    - `zoneLoadBalancer`: 작업자 노드가 있는 각 구역의 공용 로드 밸런서 IP 주소를 지정합니다. [사용 가능한 공인 IP 주소를 볼 수 있는지 확인](/docs/containers?topic=containers-subnets#review_ip)하거나 [사용된 IP 주소를 해제](/docs/containers?topic=containers-subnets#free)할 수 있습니다.
    - `local.id`: 원격 VPN 엔드포인트에서 `local.id` 옵션(`ipsec.conf`의 `leftid` 값)을 VPN IPSec 터널의 공인 IP 주소로 설정해야 하는 경우 `local.id`를 `%loadBalancerIP`. 로 설정하십시오. 이 값은 `ipsec.conf`의 `leftid` 값을 연결에 사용되는 로드 밸런서 IP 주소로 자동 구성합니다.

2. 원격 네트워크 방화벽에서는 `zoneLoadBalancer` 설정에 나열된 공인 IP 주소로의 발신 IPSec VPN 연결을 허용합니다.

### 다중 구역 클러스터의 각 구역에서 인바운드 VPN 연결 구성
{: #multizone_multiple}

수신 VPN 연결이 필요하고 원격 VPN 엔드포인트에서 다른 IP로 VPN 연결을 자동으로 재설정할 수 없는 경우 각 구역에 별도의 strongSwan VPN 서비스를 배치해야 합니다.
{: shortdesc}

각 구역에 있는 로드 밸런서와 별도의 VPN 연결을 설정하려면 원격 VPN 엔드포인트를 업데이트해야 합니다. 또한 각 VPN 연결이 고유하도록 원격 VPN 엔드포인트에서 구역 특정 설정을 구성해야 합니다. 이러한 여러 개의 수신 VPN 연결이 항상 활성 상태로 유지되는지 확인하십시오.

각 Helm 차트를 배치하면 각 strongSwan VPN 배치가 올바른 구역에서 Kubernetes 로드 밸런서 서비스로 시작됩니다. 해당 공용 IP에 대한 수신 요청은 또한 동일한 구역에 지정된 VPN 팟(Pod)에 전달됩니다. 구역에 가동 중단이 발생하는 경우 다른 구역에 설정된 VPN 연결은 영향을 받지 않습니다.

1. 각 구역에 대해 [trongSwan VPN Helm 차트를 구성](/docs/containers?topic=containers-vpn#vpn_configure)하십시오. 해당 절의 단계를 수행하는 경우 다음 설정을 지정해야 합니다.
    - `loadBalancerIP`: 이 strongSwan 서비스를 배치하는 구역에서 사용할 수 있는 공용 로드 밸런서 IP 주소를 지정합니다. [사용 가능한 공인 IP 주소를 볼 수 있는지 확인](/docs/containers?topic=containers-subnets#review_ip)하거나 [사용된 IP 주소를 해제](/docs/containers?topic=containers-subnets#free)할 수 있습니다.
    - `zoneSelector`: VPN 팟(Pod)이 스케줄될 구역을 지정합니다.
    - 추가 설정(예: `zoneSpecificRoutes`, `remoteSubnetNAT`, `localSubnetNAT` 또는 `enableSingleSourceIP`)이 VPN을 통해 액세스할 수 있어야 하는 리소스에 따라 필요할 수 있습니다. 세부사항은 다음 단계를 참조하십시오.

2. 각 VPN 연결이 고유하도록 VPN 터널의 양쪽에 구역 특정 설정을 구성하십시오. VPN을 통해 액세스할 수 있어야 하는 리소스에 따라 연결을 구별할 수 있는 다음 두 가지 옵션이 있습니다.
    * 클러스터의 팟(Pod)이 원격 온프레미스 네트워크의 서비스에 액세스해야 하는 경우:
      - `zoneSpecificRoutes`: `true`로 설정합니다. 이 설정은 VPN 연결을 클러스터의 단일 구역으로 제한합니다. 특정 구역의 팟(Pod)은 해당 특정 구역에 설정된 VPN 연결만 사용합니다. 이 솔루션은 다중 구역 클러스터에서 다중 VPN을 지원하기 위해 필요한 strongSwan 팟(Pod)의 수를 줄이고 VPN 트래픽이 현재 구역에 있는 작업자 노드로만 이동하므로 VPN 성능을 개선하고, 각 구역에 대한 VPN 접속이 VPN 연결, 중단된 팟(Pod) 또는 다른 구역의 구역 가동 중단에 의해 영향을 받지 않도록 보장합니다. `remoteSubnetNAT`를 구성할 필요가 없습니다. `zoneSpecificRoutes` 설정을 사용하는 복수의 VPN에는 라우팅이 구역 단위로 설정되므로 동일한 `remote.subnet`이 있을 수 있습니다.
      - `enableSingleSourceIP`: `true`로 설정하고 `local.subnet`를 단일 /32 IP 주소로 설정합니다. 이 설정 조합은 단일 /32 IP 주소 뒤에 있는 모든 클러스터 사설 IP 주소를 숨깁니다. 이 고유한 /32 IP 주소를 사용하면 원격 온프레미스 환경의 네트워크가 올바른 VPN 연결을 통해 요청을 시작한 클러스터의 올바른 팟(Pod)에 응답을 다시 전송할 수 있습니다. `local.subnet` 옵션에 구성되는 단일 /32 IP 주소는 각 strongSwan VPN 구성에서 고유해야 합니다.
    * 원격 온프레미스 네트워크의 애플리케이션이 클러스터의 서비스에 액세스해야 하는 경우:    
      - `localSubnetNAT`: 온프레미스 원격 네트워크에 있는 애플리케이션에서 특정 VPN 연결을 선택하여 해당 클러스터에 트래픽을 전송하고 수신할 수 있는지 확인합니다. 각 strongSwan Helm 구성에서 `localSubnetNAT`를 사용하여 원격 온프레미스 애플리케이션에서 액세스할 수 있는 클러스터 리소스를 고유하게 식별합니다. 다수의 VPN이 원격 온프레미스 네트워크에서 클러스터로 설정되므로 클러스터의 서비스에 액세스할 때 사용할 VPN을 선택할 수 있도록 온프레미스 네트워크에 있는 애플리케이션에 로직을 추가해야 합니다. 클러스터의 서비스는 각 strongSwan VPN 구성의 `localSubnetNAT`에 구성한 값에 따라 여러 개의 다른 서브넷을 통해 액세스할 수 있습니다.
      - `remoteSubnetNAT`: 클러스터에서 팟(Pod)이 동일한 VPN 연결을 사용하여 원격 네트워크에 트래픽을 리턴하도록 보장합니다. 각 strongSwan 배치 파일에서 원격 온프레미스 서브넷을 `remoteSubnetNAT` 설정을 사용하는 고유한 서브넷에 맵핑하십시오. 클러스터의 팟(Pod)이 VPN 특정 `remoteSubnetNAT`에서 수신한 트래픽은 동일한 VPN 특정 `remoteSubnetNAT`에 전송된 후 동일한 VPN 연결을 통해 다시 전송됩니다.

3. 각 구역에서 로드 밸런서 IP에 별도의 VPN 연결을 설정하도록 원격 VPN 엔드포인트 소프트웨어를 구성하십시오.

<br />


## strongSwan Helm 차트 구성
{: #vpn_configure}

strongSwan Helm 차트를 설치하기 전에 strongSwan 구성을 결정해야 합니다.
{: shortdesc}

시작하기 전에:
* 온프레미스 데이터센터에 IPSec VPN 게이트웨이를 설치하십시오.
* `default` 네임스페이스에 대해 [**작성자** 또는 **관리자** {{site.data.keyword.Bluemix_notm}} IAM 서비스 역할](/docs/containers?topic=containers-users#platform)이 있는지 확인하십시오.
* [계정에 로그인하십시오. 해당되는 경우, 적절한 리소스 그룹을 대상으로 지정하십시오. 클러스터의 컨텍스트를 설정하십시오.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
  * **참고**: 모든 strongSwan 구성은 표준 클러스터에서 허용됩니다. 무료 클러스터를 사용하는 경우 [3단계](#strongswan_3)에서 아웃바운드 VPN 연결만 선택할 수 있습니다. 인바운드 VPN 연결에는 클러스터에 로드 밸런서가 필요하며 로드 밸런서는 무료 클러스터에서 사용할 수 없습니다.

### 1단계: strongSwan Helm 차트 가져오기
{: #strongswan_1}

Helm을 설치하고 strongSwan Helm 차트를 가져와서 가능한 구성을 확인하십시오.
{: shortdesc}

1.  [지시사항에 따라](/docs/containers?topic=containers-helm#public_helm_install) 로컬 시스템에 Helm 클라이언트를 설치하고 서비스 계정이 있는 Helm 서버(tiller)를 설치한 후 {{site.data.keyword.Bluemix_notm}} Helm 저장소를 추가하십시오. Helm 버전 2.8 이상이 필요합니다.

2.  Tiller가 서비스 계정으로 설치되어 있는지 확인하십시오.

    ```
    kubectl get serviceaccount -n kube-system | grep tiller
    ```
    {: pre}

    출력 예:

    ```
    NAME                                 SECRETS   AGE
    tiller                               1         2m
    ```
    {: screen}

3. strongSwan Helm 차트를 위한 기본 구성 설정을 로컬 YAML 파일에 저장하십시오.

    ```
    helm inspect values iks-charts/strongswan > config.yaml
    ```
    {: pre}

4. `config.yaml` 파일을 여십시오.

### 2단계: 기본 IPSec 설정 구성
{: #strongswan_2}

VPN 연결의 설정을 제어하려면 다음의 기본 IPSec 설정을 수정하십시오.
{: shortdesc}

각 설정에 대한 자세한 정보를 보려면 Helm 차트에 대한 `config.yaml` 파일 내에서 제공된 문서를 읽으십시오.
{: tip}

1. 온프레미스 VPN 터널 엔드포인트가 연결 초기화를 위한 프로토콜로 `ikev2`를 지원하지 않는 경우 `ipsec.keyexchange`의 값을 `ikev1`로 변경하십시오.
2. `ipsec.esp`를 온프레미스 VPN 터널 엔드포인트가 연결에 사용하는 ESP 암호화 및 인증 알고리즘의 목록으로 설정하십시오.
    * `ipsec.keyexchange`가 `ikev1`로 설정된 경우에는 이 설정을 지정해야 합니다.
    * `ipsec.keyexchange`가 `ikev2`로 설정된 경우 이 설정은 선택사항입니다.
    * 이 설정을 공백으로 두면 기본 strongSwan 알고리즘 `aes128-sha1,3des-sha1`이 연결에 사용됩니다.
3. `ipsec.ike`를 온프레미스 VPN 터널 엔드포인트가 연결에 사용하는 IKE/ISAKMP SA 암호화 및 인증 알고리즘의 목록으로 설정하십시오. 알고리즘은 `encryption-integrity[-prf]-dhgroup` 형식으로 특정해야 합니다.
    * `ipsec.keyexchange`가 `ikev1`로 설정된 경우에는 이 설정을 지정해야 합니다.
    * `ipsec.keyexchange`가 `ikev2`로 설정된 경우 이 설정은 선택사항입니다.
    * 이 설정을 공백으로 두면 기본 strongSwan 알고리즘 `aes128-sha1-modp2048,3des-sha1-modp1536`이 연결에 사용됩니다.
4. `local.id`의 값을 VPN 터널 엔드포인트가 사용하는 로컬 Kubernetes 클러스터 측을 식별하는 데 사용할 문자열로 변경하십시오. 기본값은 `ibm-cloud`입니다. 일부 VPN 구현에서는 로컬 엔드포인트에 대해 공인 IP 주소를 사용하도록 요구합니다.
5. `remote.id`의 값을 VPN 터널 엔드포인트가 사용하는 원격 온프레미스 측을 식별하는 데 사용할 문자열로 변경하십시오. 기본값은 `on-prem`입니다. 일부 VPN 구현에서는 원격 엔드포인트에 대해 공인 IP 주소를 사용하도록 요구합니다.
6. `preshared.secret`의 값을 온프레미스 VPN 터널 엔드포인트 게이트웨이가 연결에 사용하는 사전 공유된 시크릿으로 변경하십시오. 이 값은 `ipsec.secrets`에 저장됩니다.
7. 선택사항: Helm 연결 유효성 검증 테스트의 일부로서 ping을 실행할 수 있도록 `remote.privateIPtoPing`을 원격 서브넷의 사설 IP 주소로 설정하십시오.

### 3단계: 인바운드 및 아웃바운드 VPN 연결 선택
{: #strongswan_3}

strongSwan VPN 연결을 구성하는 경우, 사용자는 VPN 연결이 클러스터에 인바운드인지 또는 클러스터에서 아웃바운드인지 여부를 선택합니다.
{: shortdesc}

<dl>
<dt>인바운드</dt>
<dd>원격 네트워크의 온프레미스 VPN 엔드포인트는 VPN 연결을 시작하고 클러스터는 연결을 청취합니다.</dd>
<dt>아웃바운드</dt>
<dd>클러스터는 VPN 연결을 시작하고 원격 네트워크의 온프레미스 VPN 엔드포인트는 연결을 청취합니다.</dd>
</dl>

무료 클러스터를 사용하는 경우 아웃바운드 VPN 연결만 선택할 수 있습니다. 인바운드 VPN 연결에는 클러스터에 로드 밸런서가 필요하며 로드 밸런서는 무료 클러스터에서 사용할 수 없습니다.

인바운드 VPN 연결을 설정하려면 다음 설정을 수정하십시오.
1. `ipsec.auto`가 `add`로 설정되었는지 확인하십시오.
2. 선택사항: `loadBalancerIP`를 strongSwan VPN 서비스에 대한 포터블 공인 IP 주소로 설정하십시오. 온프레미스 방화벽을 통과할 수 있는 IP 주소를 지정해야 하는 경우와 같이 고정 IP 주소가 필요한 경우에는 IP 주소를 지정하는 것이 유용합니다. 클러스터에는 하나 이상의 사용 가능한 공인 로드 밸런서 IP 주소가 있어야 합니다. [사용 가능한 공인 IP 주소를 볼 수 있는지 확인](/docs/containers?topic=containers-subnets#review_ip)하거나 [사용된 IP 주소를 해제](/docs/containers?topic=containers-subnets#free)할 수 있습니다.
    * 이 설정을 공백으로 두면 사용 가능한 포터블 공인 IP 주소 중 하나가 사용됩니다.
    * 사용자는 온프레미스 VPN 엔드포인트에서 클러스터 VPN 엔드포인트에 지정된 공인 IP 주소 또는 자신이 선택하는 공인 IP 주소도 구성해야 합니다.

아웃바운드 VPN 연결을 설정하려면 다음 설정을 수정하십시오.
1. `ipsec.auto`를 `start`로 변경하십시오.
2. `remote.gateway`를 원격 네트워크의 온프레미스 VPN 게이트웨이에 대한 공인 IP 주소로 설정하십시오.
3. 클러스터 VPN 엔드포인트의 IP 주소에 대해 다음 옵션 중 하나를 선택하십시오.
    * **클러스터의 사설 게이트웨이의 공인 IP 주소**: 작업자 노드가 사설 VLAN에만 연결되어 있는 경우, 아웃바운드 VPN 요청은 인터넷에 도달하기 위해 사설 게이트웨이를 통해 라우팅됩니다. 사설 게이트웨이의 공인 IP 주소는 VPN 연결에 사용됩니다.
    * **strongSwan 팟(Pod)이 실행 중인 작업자 노드의 공인 IP 주소**: strongSwan 팟(Pod)이 실행 중인 작업자 노드가 공용 VLAN에 연결되어 있으면 작업자 노드의 공인 IP 주소가 VPN 연결에 사용됩니다.
        <br>
        * strongSwan 팟(Pod)이 삭제되었으며 클러스터의 다른 작업자 노드로 다시 스케줄된 경우에는 VPN의 공인 IP 주소가 변경됩니다. 원격 네트워크의 온프레미스 VPN 엔드포인트는 VPN 연결이 클러스터 작업자 노드의 공인 IP 주소에서 설정되도록 허용해야 합니다.
        * 원격 VPN 엔드포인트가 여러 공인 IP 주소의 VPN 연결을 처리할 수 없는 경우에는 strongSwan VPN 팟(Pod)이 배치되는 노드를 제한하십시오. `nodeSelector`를 작업자 노드 레이블 또는 특정 작업자 노드의 IP 주소로 설정하십시오. 예를 들어, `kubernetes.io/hostname: 10.232.xx.xx` 값은 VPN 팟(Pod)이 해당 작업자 노드에만 배치되도록 허용합니다. 값 `strongswan: vpn`은 VPN 팟(Pod)을 해당 레이블과 함께 모든 작업자 노드에서 실행하도록 제한합니다. 임의의 작업자 노드 레이블을 사용할 수 있습니다. 서로 다른 작업자 노드가 서로 다른 Helm 차트 배치에서 사용될 수 있도록 허용하려면 `strongswan: <release_name>`을 사용하십시오. 고가용성을 위해서는 최소한 2개의 작업자 노드를 선택하십시오.
    * **strongSwan 서비스의 공인 IP 주소**: strongSwan VPN 서비스의 공인 IP 주소를 사용하여 연결을 설정하려면 `connectUsingLoadBalancerIP`를 `true`로 설정하십시오. strongSwan 서비스 IP 주소는 `loadBalancerIP` 설정에서 지정할 수 있는 포터블 공인 IP 주소이거나 서비스에 자동으로 지정되는 사용 가능한 포터블 공인 IP 주소입니다.
        <br>
        * `loadBalancerIP` 설정을 사용한 IP 주소 선정을 선택하는 경우, 클러스터에는 최소한 1개의 사용 가능한 공용 로드 밸런서 IP 주소가 있어야 합니다. [사용 가능한 공인 IP 주소를 볼 수 있는지 확인](/docs/containers?topic=containers-subnets#review_ip)하거나 [사용된 IP 주소를 해제](/docs/containers?topic=containers-subnets#free)할 수 있습니다.
        * 모든 클러스터 작업자 노드는 동일한 공용 VLAN에 있어야 합니다. 그렇지 않은 경우에는 `nodeSelector` 설정을 사용하여 VPN 팟(Pod)이 `loadBalancerIP`와 동일한 사설 VLAN의 작업자 노드에 배치되도록 해야 합니다.
        * `connectUsingLoadBalancerIP`가 `true`로 설정되었으며 `ipsec.keyexchange`가 `ikev1`로 설정된 경우에는 `enableServiceSourceIP`를 `true`로 설정해야 합니다.

### 단계 4: VPN 연결을 통한 클러스터 리소스 액세스
{: #strongswan_4}

VPN 연결을 통해 원격 네트워크가 액세스할 수 있어야 하는 클러스터 리소스를 판별하십시오.
{: shortdesc}

1. 하나 이상의 클러스터 서브넷의 CIDR을 `local.subnet` 설정에 추가하십시오. 사용자는 온프레미스 VPN 엔드포인트에서 로컬 서브넷 CIDR을 구성해야 합니다. 이 목록은 다음 서브넷을 포함할 수 있습니다.  
    * Kubernetes 팟(Pod) 서브넷 CIDR: `172.30.0.0/16`. 양방향 통신은 `remote.subnet` 설정에서 나열된 원격 네트워크 서브넷의 임의의 호스트와 모든 클러스터 팟(Pod) 간에 사용됩니다. 보안상의 이유 때문에 `remote.subnet` 호스트가 클러스터 팟(Pod)에 액세스하지 못하도록 차단하는 경우에는 Kubernetes 팟(Pod) 서브넷을 `local.subnet` 설정에 추가하지 마십시오.
    * Kubernetes 서비스 서브넷 CIDR: `172.21.0.0/16`. 서비스 IP 주소는 단일 IP 뒤의 여러 작업자 노드에 배치된 여러 앱 팟(Pod)을 노출하는 방법을 제공합니다.
    * 사설 Ingress ALB 또는 사설 네트워크의 NodePort 서비스에서 앱을 노출한 경우에는 작업자 노드의 사설 서브넷 CIDR을 추가하십시오. `ibmcloud ks worker <cluster_name>`을 실행하여 작업자의 사설 IP 주소의 처음 세 옥텟을 검색하십시오. 예를 들어, 주소가 `10.176.48.xx`인 경우에는 `10.176.48`을 기록하십시오. 그리고 `ibmcloud sl subnet list | grep <xxx.yyy.zzz>` 명령을 실행하여 작업자 사설 서브넷 CIDR을 가져오되, 명령에서 `<xxx.yyy.zz>`를 이전에 검색한 옥텟으로 대체하십시오. **참고**: 작업자 노드가 새 사설 서브넷에서 추가된 경우에는 새 사설 서브넷 CIDR을 온프레미스 VPN 엔드포인트 및 `local.subnet` 설정에 추가해야 합니다. 그리고 VPN 연결을 다시 시작해야 합니다.
    * 사설 네트워크에서 LoadBalancer 서비스가 노출한 앱이 있는 경우에는 클러스터의 사설 사용자 관리 서브넷 CIDR을 추가하십시오. 이러한 값을 찾으려면 `ibmcloud ks cluster-get --cluster <cluster_name> --showResources`를 실행하십시오. **VLANs** 섹션에서 **Public** 값이 `false`인 CIDR을 찾으십시오. **참고**: `ipsec.keyexchange`가 `ikev1`로 설정된 경우에는 하나의 서브넷만 지정할 수 있습니다. 그러나 사용자는 `localSubnetNAT` 설정을 사용하여 다중 클러스터 서브넷을 단일 서브네트로 결합할 수 있습니다.

2. 선택사항: `localSubnetNAT` 설정을 사용하여 클러스터 서브넷을 다시 맵핑하십시오. 서브넷에 대한 NAT(Network Address Translation)는 클러스터 네트워크 및 온프레미스 원격 네트워크 간의 서브넷 충돌에 대한 임시 해결책을 제공합니다. NAT를 사용하여 클러스터의 사설 로컬 IP 서브넷, 팟(Pod) 서브넷(172.30.0.0/16) 또는 팟(Pod) 서비스 서브넷(172.21.0.0/16)을 다른 사설 서브넷에 다시 맵핑할 수 있습니다. VPN 터널에서 원래의 서브넷 대신 다시 맵핑된 IP 서브넷을 확인합니다. VPN을 통해 패킷을 전송하기 전과 VPN 터널에서 패킷이 도달된 후에 다시 맵핑이 발생합니다. VPN을 통해 동시에 다시 맵핑되고 다시 맵핑되지 않은 서브넷을 모두 노출할 수 있습니다. NAT를 사용하려면 전체 서브넷 또는 개별 IP 주소를 추가할 수 있습니다.
    * `10.171.42.0/24=10.10.10.0/24` 형식의 전체 서브넷을 추가하는 경우, 리맵핑(remapping)은 1 대 1입니다. 내부 네트워크 서브넷의 모든 IP 주소가 외부 네트워크 서브넷으로 맵핑되며, 반대의 경우도 마찬가지입니다.
    * `10.171.42.17/32=10.10.10.2/32,10.171.42.29/32=10.10.10.3/32` 형식의 개별 IP 주소를 추가하는 경우에는 해당되는 IP 주소만 지정된 외부 IP 주소로 맵핑됩니다.

3. 버전 2.2.0 이상 strongSwan Helm 차트에 대한 선택사항: `enableSingleSourceIP`를 `true`로 설정하여 모든 클러스터 IP 주소를 단일 IP 주소 뒤에 숨기십시오. 원격 네트워크에서 다시 클러스터로 연결이 허용되지 않으므로, 이 옵션은 VPN 연결에 대한 가장 안전한 구성 중 하나를 제공합니다.
    <br>
    * 이 설정에서는 클러스터에서 또는 원격 네트워크에서 VPN 연결이 설정되었는지 여부에 관계없이 VPN 연결 상의 모든 데이터 플로우가 아웃바운드여야 함을 요구합니다.
    * `local.subnet`은 오직 하나의 /32 서브넷으로 설정되어야 합니다.

4. 버전 2.2.0 이상 strongSwan Helm 차트에 대한 선택사항: `localNonClusterSubnet` 설정을 사용하여 원격 네트워크의 수신 요청을 클러스터 외부에 존재하는 서비스로 라우팅하는 strongSwan 서비스를 사용하십시오.
    <br>
    * 비-클러스터 서비스는 동일한 사설 네트워크에 또는 작업자 노드가 접근할 수 있는 사설 네트워크에 존재해야 합니다.
    * 비-클러스터 작업자 노드는 VPN 연결을 통해 원격 네트워크에 대한 트래픽을 시작할 수 없지만, 비-클러스터 노드는 원격 네트워크의 수신 요청의 대상이 될 수 있습니다.
    * 사용자는 `local.subnet` 설정에서 비-클러스터 서브넷의 CIDR을 나열해야 합니다.

### 5단계: VPN 연결을 통한 원격 네트워크 리소스 액세스
{: #strongswan_5}

VPN 연결을 통해 클러스터가 액세스할 수 있어야 하는 원격 네트워크 리소스를 판별하십시오.
{: shortdesc}

1. 하나 이상의 온프레미스 사설 서브넷의 CIDR을 `remote.subnet` 설정에 추가하십시오. **참고**: `ipsec.keyexchange`가 `ikev1`로 설정된 경우에는 하나의 서브넷만 지정할 수 있습니다.
2. 버전 2.2.0 이상 strongSwan Helm 차트의 선택사항: `remoteSubnetNAT` 설정을 사용하여 원격 네트워크 서브넷을 재맵핑하십시오. 서브넷에 대한 NAT(Network Address Translation)는 클러스터 네트워크 및 온프레미스 원격 네트워크 간의 서브넷 충돌에 대한 임시 해결책을 제공합니다. NAT를 사용하여 원격 네트워크의 IP 서브넷을 다른 사설 서브넷에 재맵핑할 수 있습니다. 재맵핑은 VPN 터널을 통해 패킷이 전송되기 전에 발생합니다. 클러스터에 있는 팟(Pod)은 원래 서브넷 대신 재맵핑된 IP 서브넷을 확인합니다. 팟(Pod)이 VPN 터널을 통해 데이터를 다시 전송하기 전에 재맵핑된 IP 서브넷이 다시 원격 네트워크에서 사용하고 있는 실제 서브넷으로 전환됩니다. VPN을 통해 동시에 다시 맵핑되고 다시 맵핑되지 않은 서브넷을 모두 노출할 수 있습니다.

### 6단계(선택사항): Slack 웹훅 통합으로 모니터링 사용
{: #strongswan_6}

strongSwan VPN의 상태를 모니터링하려면 VPN 연결 메시지를 Slack 채널에 자동으로 게시하도록 웹훅을 설정하십시오.
{: shortdesc}

1. Slack 작업공간에 로그인하십시오.

2. [수신 WebHooks 앱 페이지 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://slack.com/apps/A0F7XDUAZ-incoming-webhooks)로 이동하십시오.

3. **설치 요청**을 클릭하십시오. 이 앱이 Slack 설정에 나열되지 않을 경우 Slack 작업공간 소유자에게 문의하십시오.

4. 설치 요청이 승인되면 **구성 추가**를 클릭하십시오.

5. Slack 채널을 선택하거나 VPN 메시지를 보낼 새 채널을 작성하십시오.

6. 생성되는 웹훅 URL을 복사하십시오. URL 형식은 다음과 유사하게 표시됩니다.
  ```
  https://hooks.slack.com/services/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
  ```
  {: screen}

7. Slack 웹훅이 설치되었는지 확인하려면 다음 명령을 실행하여 테스트 메시지를 웹훅 URL로 보내십시오.
    ```
    curl -X POST -H 'Content-type: application/json' -d '{"text":"VPN test message"}' <webhook_URL>
    ```
    {: pre}

8. 선택한 Slack 채널로 이동하여 테스트 메시지가 성공적인지 확인하십시오.

9. Helm 차트의 `config.yaml` 파일에서 VPN 연결을 모니터링하도록 웹훅을 구성하십시오.
    1. `monitoring.enable`을 `true`로 변경하십시오.
    2. VPN 연결을 통해 `monitoring.privateIPs` 또는 `monitoring.httpEndpoints`에 연결 가능한지 확인하려는 원격 서브넷에서 사설 IP 주소 또는 HTTP 엔드포인트를 추가하십시오. 예를 들어 `remote.privateIPtoPing` 설정의 IP를 `monitoring.privateIPs`에 추가할 수 있습니다.
    3. 웹훅 URL을 `monitoring.slackWebhook`에 추가하십시오.
    4. 필요에 따라 다른 선택적 `monitoring` 설정을 변경하십시오.

### 7단계: Helm 차트 배치
{: #strongswan_7}

앞에서 선택한 구성을 사용하여 strongSwan Helm 차트를 클러스터에 배치하십시오.
{: shortdesc}

1. 보다 고급 설정을 구성해야 하는 경우에는 Helm 차트의 각 설정에 대해 제공된 문서를 따르십시오.

3. 업데이트된 `config.yaml` 파일을 저장하십시오.

4. 업데이트된 `config.yaml` 파일로 Helm 차트를 클러스터에 설치하십시오.

    단일 클러스터에 VPN 배치가 여러 개 있는 경우 `vpn`보다 더 구체적인 릴리스 이름을 선택하면 이름 지정 충돌을 방지하고 배치를 구별할 수 있습니다. 릴리스 이름이 잘리지 않으려면 릴리스 이름을 35자 미만으로 제한하십시오.
    {: tip}

    ```
    helm install -f config.yaml --name=vpn iks-charts/strongswan
    ```
    {: pre}

5. 차트 배치 상태를 확인하십시오. 차트가 준비 상태인 경우, 출력의 맨 위 근처에 있는 **STATUS** 필드의 값은 `DEPLOYED`입니다.

    ```
    helm status vpn
    ```
    {: pre}

6. 차트가 배치된 후 `config.yaml` 파일에서 업데이트된 설정이 사용되었는지 확인하십시오.

    ```
    helm get values vpn
    ```
    {: pre}

## strongSwan VPN 연결 테스트 및 확인
{: #vpn_test}

Helm 차트를 배치한 후 VPN 연결을 테스트하십시오.
{:shortdesc}

1. 온프레미스 게이트웨이에서 VPN이 활성이 아닌 경우 VPN을 시작하십시오.

2. `STRONGSWAN_POD` 환경 변수를 설정하십시오.

    ```
    export STRONGSWAN_POD=$(kubectl get pod -l app=strongswan,release=vpn -o jsonpath='{ .items[0].metadata.name }')
    ```
    {: pre}

3. VPN의 상태를 확인하십시오. `ESTABLISHED` 상태는 VPN 연결이 성공임을 의미합니다.

    ```
    kubectl exec $STRONGSWAN_POD -- ipsec status
    ```
    {: pre}

    출력 예:

    ```
    Security Associations (1 up, 0 connecting):
    k8s-conn[1]: ESTABLISHED 17 minutes ago, 172.30.xxx.xxx[ibm-cloud]...192.xxx.xxx.xxx[on-premises]
    k8s-conn{2}: INSTALLED, TUNNEL, reqid 12, ESP in UDP SPIs: c78cb6b1_i c5d0d1c3_o
    k8s-conn{2}: 172.21.0.0/16 172.30.0.0/16 === 10.91.152.xxx/26
    ```
    {: screen}

    * strongSwan Helm 차트를 사용하여 VPN 연결을 설정하려는 경우 처음에 VPN 상태는 `ESTABLISHED`가 아닐 수 있습니다. 연결이 성공하기 전에 온프레미스 VPN 엔드포인트 설정을 확인하고 구성 파일을 여러 번 변경해야 할 수도 있습니다.
        1. `helm delete --purge <release_name>`을 실행하십시오.
        2. 구성 파일에서 올바르지 않은 값을 수정하십시오.
        3. `helm install -f config.yaml --name=<release_name> ibm/strongswan`을 실행하십시오.
      다음 단계에서 추가 검사를 실행할 수도 있습니다.

    * VPN 팟(Pod)이 `ERROR` 상태이거나 계속 충돌하고 다시 시작되는 경우, 차트의 configmap에 있는 `ipsec.conf` 설정의 매개변수 유효성 검증 때문일 수 있습니다.
        1. `kubectl logs $STRONGSWAN_POD`을 실행하여 strongSwan 팟(Pod) 로그에서 유효성 검증 오류가 있는지 확인하십시오.
        2. 유효성 검증 오류가 있는 경우 `helm delete --purge <release_name>`을 실행하십시오.
        3. 구성 파일에서 올바르지 않은 값을 수정하십시오.
        4. `helm install -f config.yaml --name=<release_name> ibm/strongswan`을 실행하십시오.

4. strongSwan 차트 정의에 포함된 5회의 Helm 테스트를 실행하여 VPN 연결을 추가로 테스트할 수 있습니다.

    ```
    helm test vpn
    ```
    {: pre}

    * 모든 테스트에 통과하면 strongSwan VPN 연결이 설정됩니다.
    * 테스트에 실패한 경우 다음 단계로 진행하십시오.

5. 테스트 팟(Pod)의 로그를 확인하여 실패한 테스트의 출력을 보십시오.

    ```
    kubectl logs <test_program>
    ```
    {: pre}

    일부 테스트에는 VPN 구성에서 선택적 설정인 요구사항이 포함됩니다. 일부 테스트가 실패하는 경우 실패는 선택적 설정의 지정 여부에 따라 허용될 수 있습니다. 각 테스트에 대한 자세한 정보와 실패 이유는 다음 표를 참조하십시오.
    {: note}

    {: #vpn_tests_table}
    <table>
    <caption>Helm VPN 연결 테스트 이해</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> Helm VPN 연결 테스트 이해</th>
    </thead>
    <tbody>
    <tr>
    <td><code>vpn-strongswan-check-config</code></td>
    <td><code>config.yaml</code> 파일에서 생성된 <code>ipsec.conf</code> 파일의 구문을 유효성 검증하십시오. 이 테스트는 <code>config.yaml</code> 파일의 올바르지 않은 값으로 인해 실패할 수 있습니다.</td>
    </tr>
    <tr>
    <td><code>vpn-strongswan-check-state</code></td>
    <td>VPN 연결이 <code>ESTABLISHED</code> 상태인지 확인합니다. 다음과 같은 이유로 인해 이 테스트가 실패할 수 있습니다.<ul><li><code>config.yaml</code> 파일의 값 및 온프레미스 VPN 엔드포인트 설정 간에 차이점이 있습니다.</li><li>클러스터가 "청취" 모드에 있는 경우(<code>ipsec.auto</code>가 <code>add</code>로 설정됨) 연결이 온프레미스 측에 설정되지 않습니다.</li></ul></td>
    </tr>
    <tr>
    <td><code>vpn-strongswan-ping-remote-gw</code></td>
    <td><code>config.yaml</code> 파일에 구성한 <code>remote.gateway</code> 공인 IP 주소에 대해 ping을 실행합니다. VPN 연결이 <code>ESTABLISHED</code> 상태일 경우 이 테스트의 결과를 무시해도 됩니다. VPN 연결이 <code>ESTABLISHED</code> 상태가 아닐 경우 다음과 같은 이유로 이 테스트가 실패할 수 있습니다.<ul><li>온프레미스 VPN 게이트웨이 IP 주소를 지정하지 않았습니다. <code>ipsec.auto</code>가 <code>start</code>로 설정된 경우 <code>remote.gateway</code> IP 주소가 필수입니다.</li><li>ICMP(ping) 패킷이 방화벽에 의해 차단됩니다.</li></ul></td>
    </tr>
    <tr>
    <td><code>vpn-strongswan-ping-remote-ip-1</code></td>
    <td>클러스터의 VPN 팟(Pod)에서 온프레미스 VPN 게이트웨이의 <code>remote.privateIPtoPing</code> 사설 IP 주소에 대해 ping을 실행합니다. 다음과 같은 이유로 인해 이 테스트가 실패할 수 있습니다.<ul><li><code>remote.privateIPtoPing</code> IP 주소를 지정하지 않았습니다. 의도적으로 IP 주소를 지정하지 않은 경우 이 실패는 허용됩니다.</li><li><code>local.subnet</code> 목록에 있는 클러스터 팟(Pod) 서브넷 CIDR인 <code>172.30.0.0/16</code>을 지정하지 않았습니다.</li></ul></td>
    </tr>
    <tr>
    <td><code>vpn-strongswan-ping-remote-ip-2</code></td>
    <td>클러스터의 작업자 노드에서 온프레미스 VPN 게이트웨이의 <code>remote.privateIPtoPing</code> 사설 IP 주소에 대해 ping을 실행합니다. 다음과 같은 이유로 인해 이 테스트가 실패할 수 있습니다.<ul><li><code>remote.privateIPtoPing</code> IP 주소를 지정하지 않았습니다. 의도적으로 IP 주소를 지정하지 않은 경우 이 실패는 허용됩니다.</li><li><code>local.subnet</code> 목록에 있는 클러스터 작업자 노드 사설 서브넷 CIDR을 지정하지 않았습니다.</li></ul></td>
    </tr>
    </tbody></table>

6. 현재 Helm 차트를 삭제하십시오.

    ```
    helm delete --purge vpn
    ```
    {: pre}

7. `config.yaml` 파일을 열고 올바르지 않은 값을 수정하십시오.

8. 업데이트된 `config.yaml` 파일을 저장하십시오.

9. 업데이트된 `config.yaml` 파일로 Helm 차트를 클러스터에 설치하십시오. 업데이트된 특성은 차트의 configmap에 저장됩니다.

    ```
    helm install -f config.yaml --name=<release_name> ibm/strongswan
    ```
    {: pre}

10. 차트 배치 상태를 확인하십시오. 차트가 준비 상태인 경우, 출력의 맨 위 근처에 있는 **STATUS** 필드의 값은 `DEPLOYED`입니다.

    ```
    helm status vpn
    ```
    {: pre}

11. 차트가 배치된 후 `config.yaml` 파일에서 업데이트된 설정이 사용되었는지 확인하십시오.

    ```
    helm get values vpn
    ```
    {: pre}

12. 현재 테스트 팟(Pod)을 정리하십시오.

    ```
    kubectl get pods -a -l app=strongswan-test
    ```
    {: pre}

    ```
    kubectl delete pods -l app=strongswan-test
    ```
    {: pre}

13. 테스트를 다시 실행하십시오.

    ```
    helm test vpn
    ```
    {: pre}

<br />


## 네임스페이스 또는 작업자 노드로 strongSwan VPN 트래픽 제한
{: #limit}

싱글 테넌트 클러스터가 있거나, 클러스터 리소스가 테넌트 간에 공유되는 멀티 테넌트 클러스터가 있는 경우 [strongSwan 배치 각각에 대한 VPN 트래픽을 특정 네임스페이스의 팟(Pod)으로 제한](#limit_namespace)할 수 있습니다. 클러스터 리소스가 테넌트에만 사용되는 멀티 테넌트 클러스터가 있는 경우 [strongSwan 배치 각각에 대한 VPN 트래픽을 각 테넌트에 전용인 작업자 노드로 제한](#limit_worker)할 수 있습니다.
{: shortdesc}

### 네임스페이스로 strongSwan VPN 트래픽 제한
{: #limit_namespace}

싱글 테넌트 클러스터 또는 멀티 테넌트 클러스터가 있는 경우 VPN 트래픽을 특정 네임스페이스에만 있는 팟(Pod)으로 제한할 수 있습니다.
{: shortdesc}

예를 들어, 특정 네임스페이스 `my-secure-namespace`에만 있는 팟(Pod)이 VPN을 통해 데이터를 전송 및 수신하려 한다고 가정하십시오. `kube-system`, `ibm-system` 또는 `default`와 같은 다른 네임스페이스에 있는 팟(Pod)은 온프레미스 네트워크에 액세스하지 못하도록 하려고 합니다. VPN 트래픽을 `my-secure-namespace`로만 제한하려면 Calico 글로벌 네트워크 정책을 작성하십시오.

이 솔루션을 사용하기 전에 다음 고려사항 및 제한사항을 검토하십시오.
* strongSwan Helm 차트를 지정된 네임스페이스에 배치할 필요가 없습니다. strongSwan VPN 팟(Pod)과 라우트 디먼 세트는 `kube-system` 또는 다른 네임스페이스에 배치할 수 있습니다. strongSwan VPN이 지정된 네임스페이스에 배치되지 않을 경우 `vpn-strongswan-ping-remote-ip-1` Helm 테스트가 실패합니다. 이 실패는 예상된 결과이므로 허용 가능합니다. 테스트 시 팟(Pod)에서 온프레미스 VPN 게이트웨이의 `remote.privateIPtoPing` 사설 IP 주소에 대해 ping이 실행됩니다. 이때 이 팟(Pod)은 원격 서브넷에 직접 액세스할 수 있는 네임스페이스에 있지 않습니다. 그러나 해당 VPN 팟(Pod)은 여전히 원격 서브넷으로 라우팅되지 않는 네임스페이스의 팟(Pod)으로 트래픽을 전달할 수 있고, 트래픽 플로우도 올바를 수 있습니다. VPN 상태는 계속 `ESTABLISHED`이고 지정된 네임스페이스에 있는 팟(Pod)은 VPN을 통해 연결될 수 있습니다.

* 다음 단계에 설명된 Calico 글로벌 네트워크 정책으로는 호스트 네트워킹을 사용하는 Kubernetes 팟(Pod)이 VPN을 통해 데이터를 전송 및 수신하는 것을 방지할 수 없습니다. 팟(Pod)이 호스트 네트워킹을 사용하도록 구성된 경우 팟(Pod)에서 실행되는 앱은 팟(Pod)이 있는 작업자 노드의 네트워크 인터페이스에서 청취할 수 있습니다. 이러한 호스트 네트워킹 팟(Pod)은 어떤 네임스페이스에나 있을 수 있습니다. 호스트 네트워킹을 사용하는 팟(Pod)을 판별하려면 `kubectl get pods --all-namespaces -o wide`를 실행하고 `172.30.0.0/16` 팟(Pod) IP 주소가 없는 팟(Pod)을 찾으십시오. 호스트 네트워킹 팟(Pod)이 VPN을 통해 데이터를 전송 및 수신하지 못하도록 하려면 `values.yaml` 배치 파일에서 `local.subnet: 172.30.0.0/16` 및 `enablePodSNAT: false` 옵션을 설정하십시오. 이러한 구성 설정을 사용하면 모든 Kubernetes 팟(Pod)이 VPN 연결을 통해 다른 원격 네트워크에 노출됩니다. 그러나 지정된 보안 네임스페이스에 있는 팟(Pod)만 VPN을 통해 연결될 수 있습니다.

시작하기 전에:
* [strongSwan Helm 차트를 배치](#vpn_configure)하고 [VPN 연결이 올바르게 작동하는지 확인](#vpn_test)하십시오.
* [Calico CLI를 설치하고 구성](/docs/containers?topic=containers-network_policies#cli_install)하십시오.

VPN 트래픽을 특정 네임스페이스로 제한하려면 다음을 수행하십시오.

1. `allow-non-vpn-outbound.yaml`이라는 Calico 글로벌 네트워크 정책을 작성하십시오. 이 정책을 사용하면 모든 네임스페이스가 모든 대상(strongSwan VPN이 액세스하는 원격 서브넷 제외)으로 아웃바운드 트래픽을 전송할 수 있습니다. `<remote.subnet>`을 `values.yaml` 구성 파일에 지정한 `remote.subnet`으로 대체하십시오. 원격 서브넷을 여러 개 지정하려면 [Calico 문서 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.projectcalico.org/v3.3/reference/calicoctl/resources/globalnetworkpolicy)를 참조하십시오.
    ```yaml
    apiVersion: projectcalico.org/v3
    kind: GlobalNetworkPolicy
    metadata:
      name: allow-non-vpn-outbound
    spec:
      selector: has(projectcalico.org/namespace)
      egress:
      - action: Allow
        destination:
          notNets:
          - <remote.subnet>
      order: 900
      types:
      - Egress
    ```
    {: codeblock}

2. 정책을 적용하십시오.

    ```
    calicoctl apply -f allow-non-vpn-outbound.yaml --config=filepath/calicoctl.cfg
    ```
    {: pre}

3. `allow-vpn-from-namespace.yaml`이라는 다른 Calico 글로벌 네트워크 정책을 작성하십시오. 이 정책을 사용하면 지정된 네임스페이스만 strongSwan VPN이 액세스하는 원격 서브넷으로 아웃바운드 트래픽을 전송할 수 있습니다. `<namespace>`는 VPN에 액세스할 수 있는 네임스페이스로 대체하고, `<remote.subnet>`은 Helm `values.yaml` 구성 파일에 지정한 `remote.subnet`으로 대체하십시오. 네임스페이스 또는 원격 서브넷을 여러 개 지정하려면 [Calico 문서 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.projectcalico.org/v3.3/reference/calicoctl/resources/globalnetworkpolicy)를 참조하십시오.
    ```yaml
    apiVersion: projectcalico.org/v3
    kind: GlobalNetworkPolicy
    metadata:
      name: allow-vpn-from-namespace
    spec:
      selector: projectcalico.org/namespace == "<namespace>"
      egress:
      - action: Allow
        destination:
          nets:
          - <remote.subnet>
      order: 900
      types:
      - Egress
    ```
    {: codeblock}

4. 정책을 적용하십시오.

    ```
    calicoctl apply -f allow-vpn-from-namespace.yaml --config=filepath/calicoctl.cfg
    ```
    {: pre}

5. 글로벌 네트워크 정책이 클러스터에 작성되었는지 확인하십시오.
    ```
calicoctl get GlobalNetworkPolicy -o wide --config=filepath/calicoctl.cfg
    ```
    {: pre}

### 작업자 노드로 strongSwan VPN 트래픽 제한
{: #limit_worker}

멀티 테넌트 클러스터에 strongSwan VPN 배치가 여러 개 있을 경우 배치 각각에 대한 VPN 트래픽을 각 테넌트에 전용인 특정 작업자 노드로 제한할 수 있습니다.
{: shortdesc}

strongSwan Helm 차트를 배치하면 strongSwan VPN 배치가 작성됩니다. strongSwan VPN 팟(Pod)은 오염되지 않은 작업자 노드에 배치됩니다. 또한 Kubernetes 디먼 세트가 작성됩니다. 이 디먼 세트는 클러스터의 오염되지 않은 모든 작업자 노드에서 원격 서브넷 각각에 대해 라우트를 자동으로 구성합니다. strongSwan VPN 팟(Pod)은 작업자 노드의 라우트를 사용하여 온프레미스 네트워크의 원격 서브넷으로 요청을 전달합니다.

`value.yaml` 파일의 `tolerations` 설정에 오염을 지정하지 않는 한 오염된 노드에 대해서는 라우트가 구성되지 않습니다. 작업자 노드를 오염시키면 해당 작업자에 대해 VPN 라우트가 구성되는 것을 방지할 수 있습니다. 그런 다음 오염된 작업자에서 허용하지 않을 VPN 배치에 대해서만 `tolerations` 설정에서 오염을 지정할 수 있습니다. 이와 같이, 한 테넌트의 Helm 차트 배치에 대한 strongSwan VPN 팟(Pod)만 해당 테넌트의 작업자 노드의 라우트를 사용하여 VPN 연결을 통해 원격 서브넷에 트래픽을 전달합니다.

이 솔루션을 사용하기 전에 다음 고려사항 및 제한사항을 검토하십시오.
* 기본적으로 Kubernetes는 오염되지 않은 사용 가능한 작업자 노드에 앱 팟(Pod)을 배치합니다. 이 솔루션이 올바르게 작동되도록 하려면 각 테넌트는 먼저 올바른 테넌트를 위해 오염된 작업자에만 앱 팟(Pod)을 배치했는지 확인해야 합니다. 또한 오염된 작업자 노드 각각에는 앱 팟(Pod)을 노드에 배치할 수 있도록 하는 결함 허용이 있어야 합니다. 오염 및 결함 허용에 대한 자세한 정보는 [Kubernetes 문서 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/)를 참조하십시오.
* 테넌트는 오염되지 않은 공유 노드에 앱 팟(Pod)을 배치할 수 있으므로 클러스터 리소스가 최적으로 사용되지 않을 수 있습니다.

strongSwan VPN 트래픽을 작업자 노드로 제한하는 다음 단계는 이 예제 시나리오를 사용합니다. 작업자 노드가 6개 있는 멀티 테넌트 {{site.data.keyword.containerlong_notm}} 클러스터가 있다고 가정하십시오. 클러스터는 테넌트 A와 테넌트 B를 지원합니다. 작업자 노드를 다음과 같은 방법으로 오염시키십시오.
* 테넌트 A 팟(Pod)만 작업자에서 스케줄링되도록 두 개의 작업자 노드를 오염시킵니다.
* 테넌트 B 팟(Pod)만 작업자에서 스케줄링되도록 두 개의 작업자 노드를 오염시킵니다.
* strongSwan VPN 팟(Pod)과 로드 밸런서 IP가 실행되려면 최소 두 개의 작업자 노드가 필요하므로 두 개의 작업자 노드는 오염되지 않습니다.

VPN 트래픽을 각 테넌트의 오염된 노드로 제한하려면 다음을 수행하십시오.

1. 이 예에서 테넌트 A에 전용인 작업자로만 VPN 트래픽을 제한하려면 테넌트 A strongSwan Helm 차트에 대해 `values.yaml` 파일에 다음 `toleration`을 지정하십시오.
    ```
    tolerations:
     - key: dedicated
       operator: "Equal"
       value: "tenantA"
       effect: "NoSchedule"
    ```
    {: codeblock}
    이 결함 허용을 사용하면 `dedicated="tenantA"` 오염이 있는 두 개의 작업자 노드와 오염되지 않은 두 개의 작업자 노드에서 라우트 디먼 세트가 실행될 수 있습니다. 이 배치에 대한 strongSwan VPN 팟(Pod)은 오염되지 않은 두 개의 작업자 노드에서 실행됩니다.

2. 이 예에서 테넌트 B에 전용인 작업자로만 VPN 트래픽을 제한하려면 테넌트 B strongSwan Helm 차트에 대해 `values.yaml` 파일에 다음 `toleration`을 지정하십시오.
    ```
    tolerations:
     - key: dedicated
       operator: "Equal"
       value: "tenantB"
       effect: "NoSchedule"
    ```
    {: codeblock}
    이 결함 허용을 사용하면 `dedicated="tenantB"` 오염이 있는 두 개의 작업자 노드와 오염되지 않은 두 개의 작업자 노드에서 라우트 디먼 세트가 실행될 수 있습니다. 이 배치에 대한 strongSwan VPN 팟(Pod)은 오염되지 않은 두 개의 작업자 노드에서도 실행됩니다.

<br />


## strongSwan Helm 차트 업그레이드
{: #vpn_upgrade}

strongSwan Helm 차트를 업그레이드하여 최신 상태인지 확인하십시오.
{:shortdesc}

strongSwan Helm 차트를 최신 버전으로 업그레이드하려면 다음을 수행하십시오.

  ```
  helm upgrade -f config.yaml <release_name> ibm/strongswan
  ```
  {: pre}

## strongSwan IPSec VPN 서비스 사용 안함
{: vpn_disable}

Helm 차트를 삭제하여 VPN 연결을 사용 안함으로 설정할 수 있습니다.
{:shortdesc}

  ```
  helm delete --purge <release_name>
  ```
  {: pre}

<br />


## 가상 라우터 어플라이언스 사용
{: #vyatta}

[가상 라우터 어플라이언스(VRA)](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-about-the-vra)는 x86 베어메탈 서버에 대한 최신 Vyatta 5600 운영 체제를 제공합니다. VRA를 VPN 게이트웨이로 사용하여 온프레미스 네트워크에 안전하게 연결할 수 있습니다.
{:shortdesc}

클러스터 VLAN에 들어오거나 나가는 모든 공용 및 사설 네트워크 트래픽은 VRA를 통해 라우팅됩니다. VRA를 VPN 엔드포인트로 사용하여 IBM Cloud 인프라(SoftLayer)의 서버와 온프레미스 리소스 간에 암호화된 IPSec 터널을 작성할 수 있습니다. 예를 들어, 다음 다이어그램은 {{site.data.keyword.containerlong_notm}}에 있는 개인 전용 작업자 노드의 앱이 VRA VPN 연결을 통해 온프레미스 서버와 통신할 수 있는 방법을 보여줍니다.

<img src="images/cs_vpn_vyatta.png" width="725" alt="로드 밸런서를 사용하여 {{site.data.keyword.containerlong_notm}}에서 앱 노출" style="width:725px; border-style: none"/>

1. 클러스터 내의 앱 `myapp2`가 Ingress 또는 LoadBalancer 서비스로부터 요청을 수신하여 온프레미스 네트워크의 데이터와 안전하게 연결해야 합니다.

2. `myapp2`는 사설 VLAN 전용의 작업자 노드에 있으므로 VRA는 작업자 노드와 온프레미스 네트워크 간의 보안 연결 역할을 수행합니다. VRA는 대상 IP 주소를 사용하여 온프레미스 네트워크에 전송할 네트워크 패킷을 판별합니다.

3. 요청이 암호화되어 VPN 터널을 통해 온프레미스 데이터센터에 전송됩니다.

4. 수신 요청이 온프레미스 방화벽을 통과해 VPN 터널 엔드포인트(라우터)에 전달되어 여기서 복호화됩니다.

5. VPN 터널 엔드포인트(라우터)는 2단계에서 지정된 대상 IP 주소에 따라 요청을 온프레미스 서버 또는 메인프레임에 전달합니다. 필요한 데이터는 동일한 프로세스를 거쳐 VPN 연결을 통해 `myapp2`에 다시 전송됩니다.

가상 라우터 어플라이언스를 설정하려면 다음을 수행하시시오.

1. [VRA를 주문](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-getting-started)하십시오.

2. [VRA에서 사설 VLAN을 구성](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-managing-your-vlans)하십시오.

3. VRA를 사용하여 VPN 연결을 사용으로 설정하려면 [VRA에서 VRRP를 구성](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-working-with-high-availability-and-vrrp#high-availability-vpn-with-vrrp)하십시오.

기존 라우터 어플라이언스가 있으며 클러스터를 추가하는 경우, 클러스터에 대해 주문된 새 포터블 서브넷은 라우터 어플라이언스에서 구성되지 않습니다. 네트워크 서비스를 사용하려면 [VLAN Spanning을 사용으로 설정](/docs/containers?topic=containers-subnets#subnet-routing)하여 동일한 VLAN의 서브넷 간에 라우팅을 사용하도록 설정해야 합니다. VLAN Spanning이 이미 사용으로 설정되었는지 확인하려면 `ibmcloud ks vlan-spanning-get --region <region>` [명령](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get)을 사용하십시오.
{: important}
