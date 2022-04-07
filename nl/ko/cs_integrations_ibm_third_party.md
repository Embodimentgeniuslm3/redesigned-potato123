---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-07"

keywords: kubernetes, iks, helm

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


# IBM Cloud 서비스와 서드파티의 통합
{: #ibm-3rd-party-integrations}

{{site.data.keyword.Bluemix_notm}} 플랫폼 및 인프라 서비스와 기타 서드파티의 통합을 사용하여 클러스터에 기능을 추가할 수 있습니다.
{: shortdesc}

## IBM Cloud 서비스
{: #ibm-cloud-services}

다음 정보를 검토하여 {{site.data.keyword.Bluemix_notm}} 플랫폼 및 인프라 서비스가 어떻게 {{site.data.keyword.containerlong_notm}}와 통합되며 이를 클러스터에서 어떻게 사용할 수 있는지 살펴보십시오.
{: shortdesc}

### IBM Cloud 플랫폼 서비스
{: #platform-services}

서비스 키를 지원하는 모든 {{site.data.keyword.Bluemix_notm}} 플랫폼 서비스는 {{site.data.keyword.containerlong_notm}} [서비스 바인딩](/docs/containers?topic=containers-service-binding)을 사용하여 통합할 수 있습니다.
{: shortdesc}

서비스 바인딩은 {{site.data.keyword.Bluemix_notm}} 서비스에 대한 서비스 인증 정보를 작성하고 클러스터의 Kubernetes 시크릿에 이 인증 정보를 저장할 수 있는 빠른 방법입니다. Kubernetes 시크릿은 데이터 보호를 위해 etcd에서 자동으로 암호화됩니다. 사용자의 앱은 시크릿의 인증 정보를 사용하여 {{site.data.keyword.Bluemix_notm}} 서비스 인스턴스에 액세스할 수 있습니다. 

서비스 키를 지원하지 않는 서비스는 일반적으로 앱에서 직접 사용할 수 있는 API를 제공합니다. 

인기 있는 {{site.data.keyword.Bluemix_notm}} 서비스에 대한 개요를 보려면 [인기 있는 통합](/docs/containers?topic=containers-supported_integrations#popular_services)을 참조하십시오. 

### IBM Cloud 인프라 서비스
{: #infrastructure-services}

{{site.data.keyword.containerlong_notm}}에서는 사용자가 {{site.data.keyword.Bluemix_notm}} 인프라를 기반으로 하는 Kubernetes 클러스터를 작성할 수 있도록 허용하므로 Virtual Server, Bare Metal Server 또는 VLAN과 같은 일부 인프라 서비스는 {{site.data.keyword.containerlong_notm}}에 완전히 통합됩니다. 이러한 서비스 인스턴스의 작성 및 관련 작업은 {{site.data.keyword.containerlong_notm}} API, CLI 또는 콘솔을 사용하여 수행합니다.
{: shortdesc}

지원되는 지속적 스토리지 솔루션({{site.data.keyword.Bluemix_notm}} File Storage, {{site.data.keyword.Bluemix_notm}} Block Storage, {{site.data.keyword.cos_full}} 등)은 Kubernetes flex 드라이버로서 통합되며 [Helm 차트](/docs/containers?topic=containers-helm)를 사용하여 설정할 수 있습니다. Helm 차트는 클러스터에 Kubernetes 스토리지 클래스, 스토리지 제공자 및 스토리지 드라이버를 자동으로 설정합니다. 사용자는 스토리지 클래스를 사용해 지속적 볼륨 클레임(PVC)을 사용하여 지속적 스토리지를 프로비저닝할 수 있습니다. 

클러스터 네트워크를 보호하거나 온프레미스 데이터 센터에 연결하려는 경우에는 다음 옵션 중 하나를 구성할 수 있습니다. 
- [strongSwan IPSec VPN 서비스](/docs/containers?topic=containers-vpn#vpn-setup)
- [{{site.data.keyword.BluDirectLink}}](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link)
- [Virtual Router Appliance(VRA)](/docs/containers?topic=containers-vpn#vyatta)
- [Fortigate Security Appliance(FSA)](/docs/services/vmwaresolutions/services?topic=vmware-solutions-fsa_considerations)

## Kubernetes 커뮤니티 및 오픈 소스 통합
{: #kube-community-tools}

{{site.data.keyword.containerlong_notm}}에서 작성한 표준 클러스터가 있는 경우에는 클러스터에 기능을 추가하기 위해 서드파티 솔루션을 설치하도록 선택할 수 있습니다.
{: shortdesc}

일부 오픈 소스 기술(Knative, Istio, LogDNA, Sysdig, Portworx 등)은 IBM에 의해 테스트되었으며 추가 기능, Helm 차트, 또는 IBM과 파트너 관계인 서비스 제공자가 운영하는 {{site.data.keyword.Bluemix_notm}} 서비스로 제공됩니다. 이러한 오픈 소스 도구는 {{site.data.keyword.Bluemix_notm}} 청구 및 지원 시스템에 완전히 통합됩니다. 

그 외의 오픈 소스 도구를 클러스터에 설치할 수도 있지만, 이러한 도구는 관리 또는 지원되지 않거나 {{site.data.keyword.containerlong_notm}}에서 작동하지 않을 수 있습니다. 

### 파트너 관계로 운영되는 통합
{: #open-source-partners}

{{site.data.keyword.containerlong_notm}} 파트너와 이들이 제공하는 각 솔루션의 이점은 [{{site.data.keyword.containerlong_notm}} 파트너](/docs/containers?topic=containers-service-partners)를 참조하십시오. 

### 관리 추가 기능
{: #cluster-add-ons}
{{site.data.keyword.containerlong_notm}}에서는 [관리 추가 기능](/docs/containers?topic=containers-managed-addons)을 사용하여 인기 있는 오픈 소스 통합([Knative](/docs/containers?topic=containers-serverless-apps-knative) 또는 [Istio](/docs/containers?topic=containers-istio) 등)을 통합합니다. 관리 추가 기능은 IBM에 의해 테스트되어 {{site.data.keyword.containerlong_notm}}에서 사용할 수 있도록 승인된 오픈 소스 도구를 클러스터에 설치하는 손쉬운 방법입니다. 

관리 추가 기능은 {{site.data.keyword.Bluemix_notm}} 지원 조직으로 완전히 통합됩니다. 관리 추가 기능 사용에 대한 질문이나 문제가 있으면 {{site.data.keyword.containerlong_notm}} 지원 채널 중 하나를 사용할 수 있습니다. 자세한 정보는 [도움 및 지원 받기](/docs/containers?topic=containers-cs_troubleshoot_clusters#clusters_getting_help)를 참조하십시오.

클러스터에 추가하는 도구에 대해 비용이 발생하는 경우 해당 비용은 자동으로 통합되어 월별 {{site.data.keyword.Bluemix_notm}} 청구의 일부로서 나열됩니다. 비용 청구 주기는 {{site.data.keyword.Bluemix_notm}}에서 결정하며 클러스터에서 추가 기능을 사용으로 설정한 시기에 따라 달라집니다.

### 기타 서드파티 통합
{: #kube-community-helm}

사용자는 Kubernetes와 통합되는 모든 서드파티 오픈 소스 도구를 설치할 수 있습니다. 예를 들어, Kubernetes 커뮤니티는 특정 Helm 차트를 `stable` 또는 `incubator`로 지정합니다. 이러한 차트 또는 도구는 {{site.data.keyword.containerlong_notm}}에서 작동하는지 확인되지 않았다는 점을 유의하십시오. 도구가 라이센스를 필요로 하는 경우에는 해당 도구를 사용하기 전에 라이센스를 구입해야 합니다. Kubernetes 커뮤니티의 사용 가능한 Helm 차트에 대한 개요는 [Helm 차트 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/kubernetes/solutions/helm-charts) 카탈로그에 있는 `kubernetes` 및 `kubernetes-incubator` 저장소를 참조하십시오.
{: shortdesc}

서드파티 오픈 소스 통합을 사용하여 발생하는 비용은 월별 {{site.data.keyword.Bluemix_notm}} 청구에 포함되지 않습니다. 

서드파티 오픈 소스 통합 또는 Kubernetes 커뮤니티의 Helm 차트를 설치하면 기본 클러스터 구성이 변경되어 클러스터가 지원되지 않음 상태가 될 수 있습니다. 이러한 도구를 사용하는 중에 문제가 발생하는 경우에는 Kubernetes 커뮤니티에 문의하거나 서비스 제공자에게 직접 문의하십시오.
{: important}
