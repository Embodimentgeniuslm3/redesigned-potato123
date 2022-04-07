---

copyright:
  years: 2014, 2019
lastupdated: "2019-05-31"

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


# IBM Cloud Kubernetes Service 파트너
{: #service-partners}

IBM은 {{site.data.keyword.containerlong_notm}}를 Kubernetes 워크로드의 마이그레이션, 운영 및 관리를 돕는 최상의 Kubernetes 서비스로 만들기 위해 노력하고 있습니다. 클라우드에서 프로덕션 워크로드를 실행하는 데 필요한 모든 기능을 제공하기 위해, {{site.data.keyword.containerlong_notm}}는 다른 서드파티 서비스 제공자와 파트너 관계를 맺고 고객의 클러스터에 최상의 로깅, 모니터링 및 스토리지 도구를 제공하고 있습니다.
{: shortdesc}

각 파트너와 이들이 제공하는 솔루션의 이점을 살펴보십시오. 클러스터에서 사용할 수 있는 다른 사유 {{site.data.keyword.Bluemix_notm}} 및 서드파티 오픈 소스 서비스를 찾아보려면 [{{site.data.keyword.Bluemix_notm}}와 서드파티의 통합 이해](/docs/containers?topic=containers-ibm-3rd-party-integrations)를 참조하십시오. 

## LogDNA
{: #logdna-partner}

{{site.data.keyword.la_full_notm}}는 [LogDNA ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://logdna.com/)를 클러스터 및 앱에 지능형 로깅 기능을 추가하기 위해 사용할 수 있는 서드파티 서비스로서 제공합니다. 

### 이점
{: #logdna-benefits}

LogDNA를 사용하여 얻을 수 있는 주요 이점의 목록을 보려면 다음 표를 검토하십시오.
{: shortdesc}

|이점|설명|
|-------------|------------------------------|
|중앙 집중식 로그 관리 및 분석|클러스터를 로그 소스로 구성하면 LogDNA가 작업자 노드, 팟(Pod), 앱 및 네트워크에 대한 로깅 정보를 자동으로 수집하기 시작합니다. LogDNA는 로그에 대해 구문 분석, 색인화, 태그 지정 및 집계를 자동으로 수행하며, 사용자가 손쉽게 클러스터 리소스에 대한 정보를 얻을 수 있도록 LogDNA 대시보드에 로그를 시각화합니다. 사용자는 기본 제공 그래프 도구를 사용하여 가장 일반적인 오류 코드 또는 로그 항목을 시각화할 수 있습니다. |
|Google과 유사한 검색 구문을 통한 검색 용이성|LogDNA는 표준 검색어, `AND` 및 `OR` 연산을 지원하는, Google과 유사한 검색 구문을 사용하며 로그를 더 쉽게 찾을 수 있게 검색어를 제외하거나 결합할 수 있도록 합니다. 로그에 대한 스마트 색인화를 사용하면 언제든지 특정 로그 항목으로 즉시 이동할 수 있습니다. |
|전송 중 및 저장 중 암호화|LogDNA는 전송 중 및 저장 중에 로그를 보호하기 위해 로그를 자동으로 암호화합니다. |
|사용자 정의 경보 및 로그 보기|사용자는 대시보드를 사용하여 검색 기준과 일치하는 로그를 찾고, 이러한 로그를 보기에 저장하고, 이 보기를 다른 사용자와 공유하여 여러 팀 구성원이 참여하는 디버깅 작업을 간소화할 수 있습니다. 이 보기를 사용하여 PagerDuty, Slack 또는 이메일과 같은 다운스트림 시스템에 전송할 수 있는 경보를 작성할 수도 있습니다. |
|즉시 사용 가능한 사용자 정의 대시보드|원하는 방식으로 로그를 시각화하기 위해 다양한 기존 대시보드 중에 하나를 선택하거나 고유한 대시보드를 작성할 수 있습니다. |

### {{site.data.keyword.containerlong_notm}}와의 통합
{: #logdna-integration}

LogDNA는 클러스터와 함께 사용할 수 있는 {{site.data.keyword.Bluemix_notm}} 플랫폼 서비스인 {{site.data.keyword.la_full_notm}}에서 제공합니다. {{site.data.keyword.la_full_notm}}는 IBM과의 파트너 관계에 따라 LogDNA에서 운영합니다.
{: shortdesc}

클러스터에서 LogDNA를 사용하려면 {{site.data.keyword.Bluemix_notm}} 계정에서 {{site.data.keyword.la_full_notm}}의 인스턴스를 프로비저닝하고 Kubernetes 클러스터를 로그 소스로 구성해야 합니다. 클러스터가 구성되고 나면 로그가 자동으로 수집되어 {{site.data.keyword.la_full_notm}} 서비스 인스턴스에 전달됩니다. 사용자는 {{site.data.keyword.la_full_notm}} 대시보드를 사용하여 로그에 액세스할 수 있습니다.    

자세한 정보는 [{{site.data.keyword.la_full_notm}}를 사용하여 Kubernetes 클러스터 로그 관리](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-kube)를 참조하십시오. 

### 청구 및 지원
{: #logdna-billing-support}

{{site.data.keyword.la_full_notm}}는 {{site.data.keyword.Bluemix_notm}} 지원 시스템에 완전히 통합되어 있습니다. {{site.data.keyword.la_full_notm}}를 사용하는 중에 문제가 발생하는 경우에는 [{{site.data.keyword.containerlong_notm}} Slack ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://ibm-container-service.slack.com/)의 `logdna-on-iks` 채널에 질문을 게시하거나, [{{site.data.keyword.Bluemix_notm}} 지원 케이스](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support)를 여십시오. IBM ID를 사용하여 Slack에 로그인하십시오. {{site.data.keyword.Bluemix_notm}} 계정에 IBM ID를 사용하지 않는 경우에는 [이 Slack에 대한 초대를 요청 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://bxcs-slack-invite.mybluemix.net/)하십시오. 

## Sysdig
{: #sydig-partner}

{{site.data.keyword.mon_full_notm}}는 [Sysdig Monitor ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://sysdig.com/products/monitor/)를 컴퓨팅 호스트, 앱, 컨테이너 및 네트워크의 성능 및 상태에 대한 정보를 얻는 데 사용할 수 있는 서드파티 클라우드 기반 컨테이너 분석 시스템으로서 제공합니다.
{: shortdesc}

### 이점
{: #sydig-benefits}

Sysdig를 사용하여 얻을 수 있는 주요 이점의 목록을 보려면 다음 표를 검토하십시오.
{: shortdesc}

|이점|설명|
|-------------|------------------------------|
|클라우드 기반 및 Prometheus 사용자 정의 메트릭에 대한 자동 액세스|사전 정의된 다양한 클라우드 기반 및 Prometheus 사용자 정의 메트릭 중에서 원하는 항목을 선택하여 컴퓨팅 호스트, 앱, 컨테이너 및 네트워크의 성능 및 상태에 대한 정보를 얻으십시오. |
|고급 필터를 사용한 문제점 해결|Sysdig Monitor는 작업자 노드의 연결 상태와 Kubernetes 서비스 간 통신 상태를 보여주는 네트워크 토폴로지를 작성합니다. 사용자는 작업자 노드에서 컨테이너 및 단일 시스템 호출로 이동하면서 각 리소스에 대해 중요한 메트릭을 그룹화하고 볼 수 있습니다. 예를 들면, 이러한 메트릭을 사용하여 가장 요청을 많이 수신하는 서비스, 또는 조회 및 응답 시간이 느린 서비스를 찾을 수 있습니다. 사용자는 이러한 데이터를 Kubernetes 이벤트, 사용자 정의 CI/CD 이벤트 또는 코드 커미트와 조합할 수 있습니다.
|이상 항목 자동 발견 및 사용자 정의 경보|특정 리소스가 나머지 리소스와 다르게 작동하는 경우 Sysdig가 이를 알릴 수 있도록, 클러스터 또는 그룹 리소스의 이상 항목을 발견하기 위해 알림을 받을 경우에 대한 규칙 및 임계값을 정의하십시오. 이러한 경보를 ServiceNow, PagerDuty, Slack, VictorOps 또는 이메일과 같은 다운스트림 도구에 전송할 수 있습니다. |
|즉시 사용 가능한 사용자 정의 대시보드|원하는 방식으로 마이크로서비스에 대한 메트릭을 시각화하기 위해 다양한 기존 대시보드 중에 하나를 선택하거나 고유한 대시보드를 작성할 수 있습니다. |
{: caption="Sysdig Monitor 사용의 이점" caption-side="top"}

### {{site.data.keyword.containerlong_notm}}와의 통합
{: #sysdig-integration}

Sysdig Monitor는 클러스터와 함께 사용할 수 있는 {{site.data.keyword.Bluemix_notm}} 플랫폼 서비스인 {{site.data.keyword.mon_full_notm}}에서 제공합니다. {{site.data.keyword.mon_full_notm}}는 IBM과의 파트너 관계에 따라 Sysdig에서 운영합니다.
{: shortdesc}

클러스터에서 Sysdig Monitor를 사용하려면 {{site.data.keyword.Bluemix_notm}} 계정에서 {{site.data.keyword.mon_full_notm}}의 인스턴스를 프로비저닝하고 Kubernetes 클러스터를 메트릭 소스로 구성해야 합니다. 클러스터가 구성되고 나면 메트릭이 자동으로 수집되어 {{site.data.keyword.mon_full_notm}} 서비스 인스턴스에 전달됩니다. 사용자는 {{site.data.keyword.mon_full_notm}} 대시보드를 사용하여 메트릭에 액세스할 수 있습니다.    

자세한 정보는 [Kubernetes 클러스터에서 앱에 대한 메트릭 분석](/docs/services/Monitoring-with-Sysdig?topic=Sysdig-kubernetes_cluster)을 참조하십시오. 

### 청구 및 지원
{: #sysdig-billing-support}

Sysdig Monitor는 {{site.data.keyword.mon_full_notm}}에서 제공하므로, 해당 사용량은 플랫폼 서비스에 대한 {{site.data.keyword.Bluemix_notm}} 청구에 포함됩니다. 가격 정보는 [{{site.data.keyword.Bluemix_notm}} 카탈로그 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/observe/monitoring/create)에 있는 사용 가능한 플랜을 검토하십시오. 

{{site.data.keyword.mon_full_notm}}는 {{site.data.keyword.Bluemix_notm}} 지원 시스템에 완전히 통합되어 있습니다. {{site.data.keyword.mon_full_notm}}를 사용하는 중에 문제가 발생하는 경우에는 [{{site.data.keyword.containerlong_notm}} Slack ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://ibm-container-service.slack.com/)의 `sysdig-monitoring` 채널에 질문을 게시하거나, [{{site.data.keyword.Bluemix_notm}} 지원 케이스](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support)를 여십시오. IBM ID를 사용하여 Slack에 로그인하십시오. {{site.data.keyword.Bluemix_notm}} 계정에 IBM ID를 사용하지 않는 경우에는 [이 Slack에 대한 초대를 요청 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://bxcs-slack-invite.mybluemix.net/)하십시오. 

## Portworx
{: #portworx-parter}

[Portworx ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://portworx.com/products/introduction/)는 컨테이너화된 데이터베이스 및 기타 Stateful 앱의 로컬 지속적 스토리지를 관리하거나, 여러 구역의 팟(Pod) 간에 데이터를 공유하는 데 사용할 수 있는 고가용성 소프트웨어 정의 스토리지 솔루션입니다.
{: shortdesc}

**소프트웨어 정의 스토리지(SDS)는 무엇입니까?** </br>
SDS 솔루션은 클러스터의 작업자 노드에 연결된 다양한 유형, 크기 또는 서로 다른 벤터의 스토리지 디바이스를 추출합니다. 하드 디스크에서 사용 가능한 스토리지가 있는 작업자 노드가 스토리지 클러스터에 노드로 추가됩니다. 이 클러스터에서 실제 스토리지가 가상화되며 사용자에게 가상 스토리지 풀로 표시됩니다. 스토리지 클러스터는 SDS 소프트웨어에 의해 관리됩니다. 데이터가 스토리지 클러스터에 저장되어야 하는 경우, SDS 소프트웨어는 고가용성을 위해 데이터를 저장할 위치를 결정합니다. 가상 스토리지는 실제 기본 스토리지 아키텍처에 관계없이 활용할 수 있는 기능과 서비스의 공통 세트를 제공합니다.

### 이점
{: #portworx-benefits}

Portworx를 사용하여 얻을 수 있는 주요 이점의 목록을 보려면 다음 표를 검토하십시오.
{: shortdesc}

|이점|설명|
|-------------|------------------------------|
|Stateful 앱에 대한 클라우드 기반 스토리지 및 데이터 관리|Portworx는 작업자 노드에 연결된, 서로 크기 또는 유형이 다를 수 있는 사용 가능한 로컬 스토리지를 모아, 컨테이너화된 데이터베이스 또는 클러스터에서 실행할 기타 Stateful 앱에 대한 통합된 지속적 스토리지 계층을 작성합니다. 사용자는 Kubernetes 지속적 볼륨 클레임(PVC)을 사용하여 데이터를 저장하기 위한 로컬 지속적 스토리지를 앱에 추가할 수 있습니다. |
|볼륨 복제를 사용한 고가용성 데이터|Portworx는 사용자가 언제나 데이터에 액세스할 수 있도록, 그리고 작업자 노드에 장애가 발생하거나 이를 다시 부팅하는 경우 Stateful 앱이 다른 작업자 노드에 다시 스케줄될 수 있도록 클러스터의 작업자 노드 및 구역 전체에 볼륨의 데이터를 복제합니다. |
|`hyper-converged` 실행 지원|Portworx는 컴퓨팅 리소스와 스토리지가 항상 동일한 작업자 노드에 배치되도록 하기 위해 [`hyper-converged` ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.portworx.com/portworx-install-with-kubernetes/storage-operations/hyperconvergence/)를 실행하도록 구성될 수 있습니다. 앱을 다시 스케줄해야 하는 경우, Portworx는 Stateful 앱에 로컬 디스크 액세스 속도와 고성능을 보장하기 위해 앱을 볼륨 복제본 중 하나가 있는 작업자 노드로 이동합니다. |
|{{site.data.keyword.keymanagementservicelong_notm}}를 사용한 데이터 암호화|사용자는 FIPS 140-2 레벨 2로 인증된 클라우드 기반 하드웨어 보안 모듈(HSM)로 보호되는 [{{site.data.keyword.keymanagementservicelong_notm}} 암호화 키를 설정](/docs/containers?topic=containers-portworx#encrypt_volumes)하여 볼륨에 있는 데이터를 보호할 수 있습니다. 하나의 암호화 키를 사용하여 클러스터의 모든 볼륨을 암호화하거나 각 볼륨마다 하나의 암호화 키를 사용할 수 있습니다. Portworx는 이 키를 사용하여 저장 데이터를 암호화하며 데이터를 다른 작업자 노드로 전송할 때 전환되는 동안 데이터를 암호화합니다.|
|기본 제공되는 스냅샷 작성 및 클라우드 백업 기능|[Portworx 스냅샷 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.portworx.com/portworx-install-with-kubernetes/storage-operations/create-snapshots/)을 작성하여 볼륨의 현재 상태와 데이터를 저장할 수 있습니다. 스냅샷은 로컬 Portworx 클러스터 또는 클라우드에 저장할 수 있습니다. |
|Lighthouse를 사용한 통합 모니터링|[Lighthouse ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.portworx.com/reference/lighthouse/)는 Portworx 클러스터 및 볼륨 스냅샷을 관리하고 모니터링하는 데 도움이 되는 직관적인 그래픽 도구입니다. Lighthouse를 사용하여 사용 가능한 스토리지 노드 수, 볼륨 및 사용 가능한 용량을 포함하여 Portworx 클러스터의 상태를 확인하고 Prometheus, Grafana 또는 Kibana에서 데이터를 분석할 수 있습니다.|
{: caption="Portworx 사용의 이점" caption-side="top"}

### {{site.data.keyword.containerlong_notm}}와의 통합
{: #portworx-integration}

{{site.data.keyword.containerlong_notm}}에서는 데이터를 저장하는 데 사용할 수 있는 하나 이상의 원시, 포맷되지 않은, 마운트 해제된 로컬 디스크로 제공되는 SDS 사용에 최적화된 작업자 노드 특성을 제공합니다. Portworx는 10Gbps 네트워크 속도로 제공되는 [SDS 작업자 노드 시스템](/docs/containers?topic=containers-planning_worker_nodes#sds)을 사용할 때 최고의 성능을 제공합니다. 그러나 비 SDS 작업자 노드 특성에 Portworx를 설치할 수 있지만 앱에 필요한 성능 이점은 얻지 못할 수도 있습니다.
{: shortdesc}

Portworx는 [Helm 차트](/docs/containers?topic=containers-portworx#install_portworx)를 사용하여 설치됩니다. Helm 차트를 설치하면 Portworx가 클러스터에서 사용 가능한 로컬 지속적 스토리지를 자동으로 분석하고 해당 스토리지를 Portworx 스토리지 계층에 추가합니다. Portworx 스토리지 계층에서 앱으로 스토리지를 추가하려면 [Kubernetes 지속적 볼륨 클레임](/docs/containers?topic=containers-portworx#add_portworx_storage)을 사용해야 합니다. 

클러스터에 Portworx를 설치하려면 Portworx 라이센스가 있어야 합니다. 처음 사용해 보는 경우에는 `px-enterprise` 에디션을 평가판으로 사용할 수 있습니다. 평가판은 30일 동안 테스트할 수 있는 전체 Portworx 기능을 제공합니다. 평가판이 만료된 후 Portworx 클러스터를 계속 사용하려면 [Portworx 라이센스를 구입 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](/docs/containers?topic=containers-portworx#portworx_license)해야 합니다.


Portworx를 설치하여 {{site.data.keyword.containerlong_notm}}와 함께 사용하는 방법에 대한 자세한 정보는 [Portworx를 사용하는 소프트웨어 정의 스토리지(SDS)에 데이터 저장](/docs/containers?topic=containers-portworx)을 참조하십시오. 

### 청구 및 지원
{: #portworx-billing-support}

로컬 디스크와 함께 제공되는 SDS 작업자 노드 머신, 그리고 Portworx를 위해 사용하는 가상 머신은 월별 {{site.data.keyword.containerlong_notm}} 청구에 포함됩니다. 가격 정보는 [{{site.data.keyword.Bluemix_notm}} 카탈로그 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/kubernetes/catalog/cluster)를 참조하십시오. Portworx 라이센스는 별도로 발생하는 비용이며 월별 청구에 포함되지 않습니다.
{: shortdesc}

Portworx를 사용하여 문제가 발생하거나 특정 유스 케이스의 Portworx 구성에 대해 대화하려면 [{{site.data.keyword.containerlong_notm}} Slack ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://ibm-container-service.slack.com/)의 `portworx-on-iks` 채널에 질문을 게시하십시오. IBM ID를 사용하여 Slack에 로그인하십시오. {{site.data.keyword.Bluemix_notm}} 계정에 IBM ID를 사용하지 않는 경우에는 [이 Slack에 대한 초대를 요청 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://bxcs-slack-invite.mybluemix.net/)하십시오. 
