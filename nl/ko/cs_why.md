---

copyright:
  years: 2014, 2019
lastupdated: "2019-05-31"

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


# {{site.data.keyword.containerlong_notm}}를 선택해야 하는 이유
{: #cs_ov}

{{site.data.keyword.containerlong}}는 Docker 컨테이너, Kubernetes 기술, 직관적인 사용자 경험, 기본 제공 보안 및 격리를 결합하여 강력한 도구를 제공함으로써 컴퓨팅 호스트의 클러스터에서 컨테이너화된 앱의 배치, 오퍼레이션, 스케일링 및 모니터링을 자동화합니다. 인증 정보는 다음을 참조하십시오. [Compliance on the {{site.data.keyword.Bluemix_notm}} ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/cloud/compliance).
{:shortdesc}


## 서비스 사용의 이점
{: #benefits}

클러스터는 기본 Kubernetes 및 {{site.data.keyword.IBM_notm}} 특정 기능을 제공하는 컴퓨팅 호스트에 배치됩니다.
{:shortdesc}

|이점|설명|
|-------|-----------|
|컴퓨팅, 네트워크 및 스토리지 인프라가 격리된 싱글 테넌트 Kubernetes 클러스터|<ul><li>조직의 요구사항을 충족하는 고유의 사용자 정의된 인프라를 작성합니다.</li><li>IBM Cloud 인프라(SoftLayer)에서 제공하는 리소스를 사용하여 데디케이티드 및 보안 Kubernetes 마스터, 작업자 노드, 가상 네트워크 및 스토리지를 프로비저닝합니다.</li><li>클러스터를 사용 가능한 상태로 유지하도록 {{site.data.keyword.IBM_notm}}에서 지속적으로 모니터링하고 업데이트하는 완전히 관리되는 Kubernetes 마스터.</li><li>신뢰할 수 있는 컴퓨팅을 사용하여 베어메탈 서버로 작업자 노드를 프로비저닝하는 옵션.</li><li>지속적 데이터를 저장하고, Kubernetes 팟(Pod) 간에 데이터를 공유하며, 통합 및 보안 볼륨 서비스에서 필요 시에 데이터를 복원합니다.</li><li>모든 기본 Kubernetes API에 대한 전체 지원의 이점.</li></ul>|
|고가용성을 높이기 위한 다중 구역 클러스터 | <ul><li>작업자 풀에서 동일한 머신 유형(CPU, 메모리, 가상 또는 실제)의 작업자 노드를 손쉽게 관리합니다.</li><li>선택된 다중 구역 간에 노드를 균등하게 전개하고 앱에 대해 반친화성 및 팟(Pod) 배치를 사용하여 구역 장애가 발생하지 않도록 경계합니다.</li><li>개별 클러스터에서 리소스를 복제하는 대신 다중 구역 클러스터를 사용하여 비용을 절감합니다.</li><li>클러스터의 각 구역에서 사용자를 위해 자동으로 설정되는 다중 구역 로드 밸런서(MZLB)를 사용한 앱 간의 자동 로드 밸런싱으로부터 이익을 얻습니다.</li></ul>|
| 고가용성 마스터 | <ul><li>클러스터 가동 중단 시간을 줄입니다(예: 클러스터 작성 시에 자동으로 프로비저닝된 고가용성 마스터의 마스터 업데이트 중에).</li><li>구역 장애로부터 클러스터를 보호하기 위해 [다중 구역 클러스터](/docs/containers?topic=containers-ha_clusters#multizone)의 구역 간에 마스터를 전개합니다.</li></ul> |
|Vulnerability Advisor에서 이미지 보안 준수|<ul><li>이미지가 저장되고 조직의 모든 사용자에 의해 공유되는 보안 Docker 개인용 이미지 레지스트리의 자체 저장소를 설정합니다.</li><li>개인용 {{site.data.keyword.Bluemix_notm}} 레지스트리에서 이미지를 자동 스캔하는 이점.</li><li>잠재적 취약점을 해결하기 위해 이미지에서 사용된 운영 체제에 특정한 권장사항을 검토합니다.</li></ul>|
|클러스터 상태의 지속적 모니터링|<ul><li>클러스터 대시보드를 사용하여 클러스터, 작업자 노드 및 컨테이너 배치의 상태를 빠르게 보고 관리합니다.</li><li>{{site.data.keyword.monitoringlong}}를 사용하여 세부 이용 메트릭을 찾아서 작업 로드를 충족하도록 클러스터를 신속하게 확장합니다.</li><li>{{site.data.keyword.loganalysislong}}를 사용하여 로깅 정보를 검토하고 자세한 클러스터 활동을 확인하십시오.</li></ul>|
|공용으로 앱을 안전하게 노출|<ul><li>인터넷에서 클러스터의 서비스에 액세스하기 위해 공인 IP 주소, {{site.data.keyword.IBM_notm}} 제공 라우트 또는 자체 사용자 정의 도메인 간에 선택합니다.</li></ul>|
|{{site.data.keyword.Bluemix_notm}} 서비스 통합|<ul><li>Watson API, Blockchain, 데이터 서비스 또는 Internet of Things와 같은 {{site.data.keyword.Bluemix_notm}} 서비스의 통합을 통해 앱에 부가 기능을 추가합니다.</li></ul>|
{: caption="{{site.data.keyword.containerlong_notm}}의 이점" caption-side="top"}

시작할 준비가 되셨습니까? [Kubernetes 클러스터 작성 튜토리얼](/docs/containers?topic=containers-cs_cluster_tutorial#cs_cluster_tutorial)을 이용해 보십시오.

<br />


## 오퍼링 및 오퍼링 조합의 비교
{: #differentiation}

{{site.data.keyword.containerlong_notm}}는 {{site.data.keyword.Bluemix_notm}} 퍼블릭, {{site.data.keyword.Bluemix_notm}} Private 또는 하이브리드 설정에서 실행할 수 있습니다.
{:shortdesc}


<table>
<caption>{{site.data.keyword.containershort_notm}} 설정 간의 차이점</caption>
<col width="22%">
<col width="78%">
 <thead>
 <th>{{site.data.keyword.containershort_notm}} 설정</th>
 <th>설명</th>
 </thead>
 <tbody>
 <tr>
 <td>{{site.data.keyword.Bluemix_notm}} 퍼블릭, 오프프레미스</td>
 <td>[공유 하드웨어, 전용 하드웨어 또는 베어메탈 머신](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)의 {{site.data.keyword.Bluemix_notm}} 퍼블릭을 사용하면 {{site.data.keyword.containerlong_notm}}를 사용하여 클러스터 내의 앱을 클라우드에서 호스팅할 수 있습니다. 다중 구역의 작업자 풀에서 클러스터를 작성하여 앱에 대한 고가용성을 높일 수도 있습니다. {{site.data.keyword.Bluemix_notm}} 퍼블릭의 {{site.data.keyword.containerlong_notm}}는 Docker 컨테이너, Kubernetes 기술, 직관적인 사용자 경험, 기본 제공 보안 및 격리를 결합하여 컴퓨팅 호스트의 클러스터에서 컨테이너화된 앱의 배치, 오퍼레이션, 스케일링 및 모니터링을 자동화하는 강력한 도구를 제공합니다.<br><br>자세한 정보는 [{{site.data.keyword.containerlong_notm}} 기술](/docs/containers?topic=containers-ibm-cloud-kubernetes-service-technology)을 참조하십시오.
 </td>
 </tr>
 <tr>
 <td>{{site.data.keyword.Bluemix_notm}} Private, 온프레미스</td>
 <td>{{site.data.keyword.Bluemix_notm}} Private은 사용자가 자신의 머신에 로컬로 설치할 수 있는 애플리케이션 플랫폼입니다. 컨테이너화된 온프레미스 앱을 방화벽으로 보호되는 사용자의 고유한 제어 환경에서 개발하고 관리해야 하는 경우에는 Kubernetes를 {{site.data.keyword.Bluemix_notm}} Private에서 사용하도록 선택할 수 있습니다. <br><br>자세한 정보는 [{{site.data.keyword.Bluemix_notm}} Private 제품 문서 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_1.2.0/kc_welcome_containers.html)를 참조하십시오.
 </td>
 </tr>
 <tr>
 <td>하이브리드 설정</td>
 <td>하이브리드는 {{site.data.keyword.Bluemix_notm}} 퍼블릭 온프레미스에서 실행되는 서비스와 {{site.data.keyword.Bluemix_notm}} Private의 앱과 같은 온프레미스에서 실행되는 기타 서비스를 결합하여 사용합니다. 하이브리드 설정의 예는 다음과 같습니다. <ul><li>{{site.data.keyword.Bluemix_notm}} 퍼블릭의 {{site.data.keyword.containerlong_notm}}에서 클러스터를 프로비저닝하지만, 해당 클러스터를 온프레미스 데이터베이스에 연결합니다.</li><li>{{site.data.keyword.Bluemix_notm}} Private의 {{site.data.keyword.containerlong_notm}}에서 클러스터를 프로비저닝하고 해당 클러스터에 앱을 배치합니다. 그러나 이 앱이 {{site.data.keyword.toneanalyzershort}}와 같은 {{site.data.keyword.ibmwatson}} 서비스를 {{site.data.keyword.Bluemix_notm}} 퍼블릭에서 사용할 수 있습니다.</li></ul><br>{{site.data.keyword.Bluemix_notm}} 퍼블릭 또는 전용에서 실행 중인 서비스와 온프레미스에서 실행 중인 서비스가 서로 통신할 수 있게 하려면 [VPN 연결을 설정](/docs/containers?topic=containers-vpn)해야 합니다. 자세한 정보는 [{{site.data.keyword.Bluemix_notm}} Private에서 {{site.data.keyword.containerlong_notm}} 사용](/docs/containers?topic=containers-hybrid_iks_icp)을 참조하십시오.
 </td>
 </tr>
 </tbody>
</table>

<br />


## 무료 및 표준 클러스터 비교
{: #cluster_types}

하나의 무료 클러스터와 임의의 수의 표준 클러스터를 작성할 수 있습니다. 무료 클러스터를 사용해 봄으로써 일부 Kubernetes 기능에 친숙해지거나 표준 클러스터를 작성함으로써 Kubernetes의 전체 기능을 사용하여 앱을 배치할 수 있습니다. 무료 클러스터는 30일 후에 자동으로 삭제됩니다.
{:shortdesc}

무료 클러스터를 보유 중이며 표준 클러스터로 업그레이드하려는 경우에는 [표준 클러스터](/docs/containers?topic=containers-clusters#clusters_ui)를 작성할 수 있습니다. 그리고 무료 클러스터에서 작성한 Kubernetes 리소스에 대한 YAML을 표준 클러스터에 배치하십시오.

|특성|무료 클러스터|표준 클러스터|
|---------------|-------------|-----------------|
|[클러스터 내부 네트워킹](/docs/containers?topic=containers-security#network)|<img src="images/confirm.svg" width="32" alt="사용 가능한 기능" style="width:32px;" />|<img src="images/confirm.svg" width="32" alt="사용 가능한 기능" style="width:32px;" />|
|[불안정한 IP 주소에 대한 NodePort 서비스의 공용 네트워크 앱 액세스](/docs/containers?topic=containers-nodeport)|<img src="images/confirm.svg" width="32" alt="사용 가능한 기능" style="width:32px;" />|<img src="images/confirm.svg" width="32" alt="사용 가능한 기능" style="width:32px;" />|
|[사용자 액세스 관리](/docs/containers?topic=containers-users#access_policies)|<img src="images/confirm.svg" width="32" alt="사용 가능한 기능" style="width:32px;" />|<img src="images/confirm.svg" width="32" alt="사용 가능한 기능" style="width:32px;" />|
|[클러스터 및 앱에서 {{site.data.keyword.Bluemix_notm}} 서비스 액세스](/docs/containers?topic=containers-service-binding#bind-services)|<img src="images/confirm.svg" width="32" alt="사용 가능한 기능" style="width:32px;" />|<img src="images/confirm.svg" width="32" alt="사용 가능한 기능" style="width:32px;" />|
|[비지속적 스토리지에 대한 작업자 노드의 디스크 공간](/docs/containers?topic=containers-storage_planning#non_persistent_overview)|<img src="images/confirm.svg" width="32" alt="사용 가능한 기능" style="width:32px;" />|<img src="images/confirm.svg" width="32" alt="사용 가능한 기능" style="width:32px;" />|
|[모든 {{site.data.keyword.containerlong_notm}} 지역에서 클러스터를 작성할 수 있는 기능](/docs/containers?topic=containers-regions-and-zones) | | <img src="images/confirm.svg" width="32" alt="사용 가능한 기능" style="width:32px;" /> |
|[앱 고가용성을 높이기 위한 다중 구역 클러스터](/docs/containers?topic=containers-ha_clusters#multizone) | |<img src="images/confirm.svg" width="32" alt="사용 가능한 기능" style="width:32px;" />|
| 고가용성을 위해 복제된 마스터 | | <img src="images/confirm.svg" width="32" alt="사용 가능한 기능" style="width:32px;" /> |
|[용량 증가를 위한 확장 가능한 작업자 노드 수](/docs/containers?topic=containers-app#app_scaling)| |<img src="images/confirm.svg" width="32" alt="사용 가능한 기능" style="width:32px;" />|
|[볼륨이 있는 지속적 NFS 파일 기반 스토리지](/docs/containers?topic=containers-file_storage#file_storage)| |<img src="images/confirm.svg" width="32" alt="사용 가능한 기능" style="width:32px;" />|
|[네트워크 로드 밸런서(NLB) 서비스에 의한 안정적인 IP 주소에 대한 공용 또는 사설 네트워크 앱 액세스](/docs/containers?topic=containers-loadbalancer)| |<img src="images/confirm.svg" width="32" alt="사용 가능한 기능" style="width:32px;" />|
|[Ingress 서비스에 의한 안정적인 IP 주소 및 사용자 정의할 수 있는 URL에 대한 공용 네트워크 앱 액세스](/docs/containers?topic=containers-ingress#planning)| |<img src="images/confirm.svg" width="32" alt="사용 가능한 기능" style="width:32px;" />|
|[포터블 공인 IP 주소](/docs/containers?topic=containers-subnets#review_ip)| |<img src="images/confirm.svg" width="32" alt="사용 가능한 기능" style="width:32px;" />|
|[로깅 및 모니터링](/docs/containers?topic=containers-health#logging)| |<img src="images/confirm.svg" width="32" alt="사용 가능한 기능" style="width:32px;" />|
|[실제(베어메탈) 서버에 작업자 노드를 프로비저닝하는 옵션](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes) | |<img src="images/confirm.svg" width="32" alt="사용 가능한 기능" style="width:32px;" />|
|[신뢰할 수 있는 컴퓨팅을 사용하여 베어메탈 작업자를 프로비저닝하는 옵션](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes) | |<img src="images/confirm.svg" width="32" alt="사용 가능한 기능" style="width:32px;" />|
{: caption="무료 및 표준 클러스터의 특성" caption-side="top"}
