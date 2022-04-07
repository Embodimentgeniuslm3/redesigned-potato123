---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

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

# 릴리스 정보
{: #iks-release}

릴리스 정보를 사용하여 매월 그룹화되는 {{site.data.keyword.containerlong}} 문서의 최신 변경사항에 대해 알아볼 수 있습니다.
{:shortdesc}

## 2019년 6월
{: #jun19}

<table summary="이 표는 인기 있는 주제를 보여줍니다. 행은 왼쪽에서 오른쪽 방향으로 읽어야 하며 1열에는 날짜, 2열에는 기능 제목, 3열에는 설명이 있습니다.">
<caption>2019년 6월의 {{site.data.keyword.containerlong_notm}} 문서 업데이트</caption>
<thead>
<th>날짜</th>
<th>설명</th>
</thead>
<tbody>
<tr>
  <td>2019년 6월 7일</td>
  <td><ul>
  <li><strong>개인 서비스 엔드포인트를 통해 Kubernetes 마스터에 액세스</strong>: 사설 로드 밸런서를 통해 개인 서비스 엔드포인트를 노출하는 [단계](/docs/containers?topic=containers-clusters#access_on_prem)를 추가했습니다. 이 단계를 완료한 후 권한이 부여된 클러스터 사용자는 VPN 또는 {{site.data.keyword.Bluemix_notm}} Direct Link 연결로부터 Kubernetes에 액세스할 수 있습니다. </li>
  <li><strong>{{site.data.keyword.BluDirectLink}}</strong>: 공용 인터넷을 라우팅하지 않고 원격 네트워크 환경 및 {{site.data.keyword.containerlong_notm}} 간의 직접적인 사설 연결을 작성하기 위한 수단으로 {{site.data.keyword.Bluemix_notm}} Direct Link를 [VPN 연결](/docs/containers?topic=containers-vpn) 및 [하이브리드 클라우드](/docs/containers?topic=containers-hybrid_iks_icp) 페이지에 추가했습니다. </li>
  <li><strong>Ingress ALB 변경 로그</strong>: [빌드 330에 대한 ALB `ingress-auth` 이미지](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog)를 업데이트했습니다.</li>
  <li><strong>OpenShift 베타</strong>: {{site.data.keyword.la_full_notm}} 및 {{site.data.keyword.mon_full_notm}} 추가 기능에 대한 권한 있는 보안 컨텍스트 제한조건에 맞게 앱 배치를 수정하는 방법에 대한 [학습을 추가](/docs/containers?topic=containers-openshift_tutorial#openshift_logdna_sysdig)했습니다. </li>
  </ul></td>
</tr>
<tr>
  <td>2019년 6월 6일</td>
  <td><ul>
  <li><strong>Fluentd 변경 로그</strong>: [Fluentd 버전 변경 로그](/docs/containers?topic=containers-cluster-add-ons-changelog#fluentd_changelog)를 추가했습니다.</li>
  <li><strong>Ingress ALB 변경 로그</strong>: [빌드 470에 대한 ALB `nginx-ingress` 이미지](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog)를 업데이트했습니다.</li>
  </ul></td>
</tr>
<tr>
  <td>2019년 5월 6일</td>
  <td><ul>
  <li><strong>CLI 참조</strong>: {{site.data.keyword.containerlong_notm}} CLI 플러그인의 [버전 0.3.34 릴리스](/docs/containers?topic=containers-cs_cli_changelog)에 대한 여러 변경사항을 반영하도록 [CLI 참조 페이지](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli)를 업데이트했습니다. </li>
  <li><strong>새로운 Red Hat OpenShift on IBM Cloud 클러스터(베타)</strong>: Red Hat OpenShift on IBM Cloud 베타를 통해 OpenShift 컨테이너 오케스트레이션 플랫폼 소프트웨어가 설치된 상태에서 제공되는 작업자 노드가 포함된 {{site.data.keyword.containerlong_notm}} 클러스터를 작성할 수 있습니다. 앱 배치를 위해 Red Hat Enterprise Linux에 실행되는 [OpenShift 도구 및 카탈로그 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.openshift.com/container-platform/3.11/welcome/index.html)와 함께 클러스터 인프라 환경에 적합한 모든 관리 {{site.data.keyword.containerlong_notm}}의 이점을 누리십시오. 시작하려면 [튜토리얼: Red Hat OpenShift on IBM Cloud 클러스터(베타) 작성](/docs/containers?topic=containers-openshift_tutorial)을 참조하십시오.</li>
  </ul></td>
</tr>
<tr>
  <td>2019년 6월 4일</td>
  <td><ul>
  <li><strong>버전 변경 로그</strong>: [1.14.2_1521](/docs/containers?topic=containers-changelog#1142_1521), [1.13.6_1524](/docs/containers?topic=containers-changelog#1136_1524), [1.12.9_1555](/docs/containers?topic=containers-changelog#1129_1555) 및 [1.11.10_1561](/docs/containers?topic=containers-changelog#11110_1561) 패치 릴리스에 대한 변경 로그를 업데이트했습니다. </li></ul>
  </td>
</tr>
<tr>
  <td>2019년 6월 3일</td>
  <td><ul>
  <li><strong>자체 Ingress 제어기 가져오기</strong>: 기본 커뮤니티 제어기를 반영하고 다중 구역 클러스터에서 제어기 IP 주소에 대한 상태 검사를 필요로 하는 [단계](/docs/containers?topic=containers-ingress#user_managed)를 업데이트했습니다. </li>
  <li><strong>{{site.data.keyword.cos_full_notm}}</strong>: Helm 서버인 Tiller를 사용하거나 사용하지 않고 {{site.data.keyword.cos_full_notm}} 플러그인을 설치하는 [단계](/docs/containers?topic=containers-object_storage#install_cos)를 업데이트했습니다. </li>
  <li><strong>Ingress ALB 변경 로그</strong>: [빌드 467에 대한 ALB `nginx-ingress` 이미지](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog)를 업데이트했습니다.</li>
  <li><strong>Kustomize</strong>: [Kustomize를 사용하여 여러 환경에서 Kubernetes 구성 파일 재사용](/docs/containers?topic=containers-app#kustomize)의 예를 추가했습니다.</li>
  <li><strong>Razee</strong>: 클러스터의 배치 정보를 시각화하고 Kubernetes 리소스의 배치를 자동화하기 위해 [Razee ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/razee-io/Razee)를 지원되는 통합에 추가했습니다. </li></ul>
  </td>
</tr>
</tbody></table>

## 2019년 5월
{: #may19}

<table summary="이 표는 인기 있는 주제를 보여줍니다. 행은 왼쪽에서 오른쪽 방향으로 읽어야 하며 1열에는 날짜, 2열에는 기능 제목, 3열에는 설명이 있습니다.">
<caption>2019년 5월의 {{site.data.keyword.containerlong_notm}} 문서 업데이트</caption>
<thead>
<th>날짜</th>
<th>설명</th>
</thead>
<tbody>
<tr>
  <td>2019년 5월 30일</td>
  <td><ul>
  <li><strong>CLI 참조</strong>: {{site.data.keyword.containerlong_notm}} CLI 플러그인의 [버전 0.3.33 릴리스](/docs/containers?topic=containers-cs_cli_changelog)에 대한 여러 변경사항을 반영하도록 [CLI 참조 페이지](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli)를 업데이트했습니다. </li>
  <li><strong>스토리지 문제점 해결</strong>: <ul>
  <li>[PVC 보류 상태](/docs/containers?topic=containers-cs_troubleshoot_storage#file_pvc_pending)에 대한 파일 및 블록 스토리지 문제점 해결 주제를 추가했습니다.</li>
  <li>[앱이 PVC에 액세스하거나 쓸 수 없는](/docs/containers?topic=containers-cs_troubleshoot_storage#block_app_failures) 경우의 블록 스토리지 문제점 해결 주제를 추가했습니다. </li>
  </ul></li>
  </ul></td>
</tr>
<tr>
  <td>2019년 5월 28일</td>
  <td><ul>
  <li><strong>클러스터 추가 기능 변경 로그</strong>: [빌드 462에 대한 ALB `nginx-ingress` 이미지](/docs/containers?topic=containers-cluster-add-ons-changelog)를 업데이트했습니다.</li>
  <li><strong>레지스트리 문제점 해결</strong>: 클러스터 작성 중에 발생하는 오류로 인해 {{site.data.keyword.registryfull}}에서 이미지를 가져올 수 없는 경우의 [문제점 해결 주제](/docs/containers?topic=containers-cs_troubleshoot_clusters#ts_image_pull_create)를 추가했습니다. </li>
  <li><strong>스토리지 문제점 해결</strong>: <ul>
  <li>[지속적 스토리지 장애 디버깅](/docs/containers?topic=containers-cs_troubleshoot_storage#debug_storage)에 대한 주제를 추가했습니다.</li>
  <li>[누락된 권한으로 인한 PVC 작성 실패](/docs/containers?topic=containers-cs_troubleshoot_storage#missing_permissions)에 대한 문제점 해결 주제를 추가했습니다.</li>
  </ul></li>
  </ul></td>
</tr>
<tr>
  <td>2019년 5월 23일</td>
  <td><ul>
  <li><strong>CLI 참조</strong>: {{site.data.keyword.containerlong_notm}} CLI 플러그인의 [버전 0.3.28 릴리스](/docs/containers?topic=containers-cs_cli_changelog)에 대한 여러 변경사항을 반영하도록 [CLI 참조 페이지](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli)를 업데이트했습니다. </li>
  <li><strong>C클러스터 추가 기능 변경 로그</strong>: [빌드 457에 대한 ALB `nginx-ingress` 이미지](/docs/containers?topic=containers-cluster-add-ons-changelog)를 업데이트했습니다.</li>
  <li><strong>클러스터 및 작업자 상태</strong>: 클러스터 및 작업자 노드 상태에 대한 참조 테이블을 추가하기 위해 [로깅 및 모니터링 페이지](/docs/containers?topic=containers-health#states)를 업데이트했습니다. </li>
  <li><strong>클러스터 계획 및 작성</strong>: 이제 다음 페이지에서 클러스터 계획, 작성 및 제거와 네트워크 계획에 대한 정보를 찾을 수 있습니다.
    <ul><li>[클러스터 네트워크 설정 계획](/docs/containers?topic=containers-plan_clusters)</li>
    <li>[고가용성을 위한 클러스터 계획](/docs/containers?topic=containers-ha_clusters)</li>
    <li>[작업자 노드 설정 계획](/docs/containers?topic=containers-planning_worker_nodes)</li>
    <li>[클러스터 작성](/docs/containers?topic=containers-clusters)</li>
    <li>[클러스터에 작업자 노드 및 구역 추가](/docs/containers?topic=containers-add_workers)</li>
    <li>[클러스터 제거](/docs/containers?topic=containers-remove)</li>
    <li>[서비스 엔드포인트 또는 VLAN 연결 변경](/docs/containers?topic=containers-cs_network_cluster)</li></ul>
  </li>
  <li><strong>클러스터 버전 업데이트</strong>: 마스터 버전이 지원되는 가장 오래된 버전보다 세 버전 이상 이전 버전이면 업그레이드할 수 없음을 나타내도록 [지원되지 않는 버전 정책](/docs/containers?topic=containers-cs_versions)을 업데이트했습니다. 클러스터를 나열할 때 **상태** 필드를 검토하여 클러스터가 **지원되지 않는지** 여부를 확인할 수 있습니다. </li>
  <li><strong>Istio</strong>: Istio가 사설 VLA에만 연결되는 클러스터에서 작동하지 않는 제한사항을 제거하도록 [Istio 페이지](/docs/containers?topic=containers-istio)를 업데이트했습니다. Istio 관리 추가 기능의 업데이트가 완료된 후 TLS 섹션을 사용하는 Istio 게이트웨이를 다시 작성하기 위해 단계를 [관리 추가 기능 업데이트 주제](/docs/containers?topic=containers-managed-addons#updating-managed-add-ons)에 추가했습니다. </li>
  <li><strong>인기 있는 주제</strong>: 인기 있는 주제를 {{site.data.keyword.containershort_notm}}에 특정한 새 기능 및 업데이트에 대한 해당 릴리스 정보 페이지로 대체했습니다. {{site.data.keyword.Bluemix_notm}} 제품에 대한 최신 정보는 [공지사항](https://www.ibm.com/cloud/blog/announcements)을 참조하십시오.</li>
  </ul></td>
</tr>
<tr>
  <td>2019년 5월 20일</td>
  <td><ul>
  <li><strong>버전 변경 로그</strong>: [작업자 노드 수정팩 변경 로그](/docs/containers?topic=containers-changelog)를 추가했습니다.</li>
  </ul></td>
</tr>
<tr>
  <td>2019년 5월 16일</td>
  <td><ul>
  <li><strong>CLI 참조</strong>: `logging-collect` 명령에 대한 COS 엔드포인트를 추가하고 `apiserver-refresh`로 Kubernetes 마스터 컴포넌트가 다시 시작되는 점을 명확히 하도록 [CLI 참조 페이지](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli)를 업데이트했습니다. </li>
  <li><strong>지원되지 않음: Kubernetes 버전 1.10</strong>: [Kubernetes 버전 1.10](/docs/containers?topic=containers-cs_versions#cs_v114)은 이제 지원되지 않습니다. </li>
  </ul></td>
</tr>
<tr>
  <td>2019년 5월 15일</td>
  <td><ul>
  <li><strong>기본 Kubernetes 버전</strong>: 현재 기본 Kubernetes 버전은 1.13.6입니다.</li>
  <li><strong>서비스 제한</strong>: [서비스 제한사항 주제](/docs/containers?topic=containers-ibm-cloud-kubernetes-service-technology#tech_limits)를 추가했습니다.</li>
  </ul></td>
</tr>
<tr>
  <td>2019년 5월 13일</td>
  <td><ul>
  <li><strong>버전 변경 로그</strong>: 새 패치 업데이트가 [1.14.1_1518](/docs/containers?topic=containers-changelog#1141_1518), [1.13.6_1521](/docs/containers?topic=containers-changelog#1136_1521), [1.12.8_1552](/docs/containers?topic=containers-changelog#1128_1552), [1.11.10_1558](/docs/containers?topic=containers-changelog#11110_1558) 및 [1.10.13_1558](/docs/containers?topic=containers-changelog#11013_1558)에 제공될 수 있음을 추가했습니다. </li>
  <li><strong>작업자 노드 특성</strong>: [클라우드 상태 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/status?component=containers-kubernetes&selected=status)당 코어가 48개 이상인 [가상 머신 작업자 노드 특성](/docs/containers?topic=containers-planning_worker_nodes#vm)을 제거했습니다. 코어가 48개 이상인 [베어메탈 작업자 노드](/docs/containers?topic=containers-plan_clusters#bm)를 계속해서 프로비저닝할 수 있습니다. </li></ul></td>
</tr>
<tr>
  <td>2019년 5월 8일</td>
  <td><ul>
  <li><strong>API</strong>: 링크를 [글로벌 API Swagger 문서 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://containers.cloud.ibm.com/global/swagger-global-api/#/)에 추가했습니다.</li>
  <li><strong>Cloud Object Storage</strong>: {{site.data.keyword.containerlong_notm}} 클러스터에서 [Cloud Object Storage용 문제점 해결 안내서를 추가](/docs/containers?topic=containers-cs_troubleshoot_storage#cos_pvc_pending)했습니다. </li>
  <li><strong>Kubernetes 전략</strong>: [내 앱을 {{site.data.keyword.containerlong_notm}}로 이동하기 전에 갖고 있으면 유용한 지식 및 기술은 무엇입니까?](/docs/containers?topic=containers-strategy#knowledge)에 대한 주제를 추가했습니다.</li>
  <li><strong>Kubernetes 버전 1.14</strong>: [Kubernetes 1.14 릴리스](/docs/containers?topic=containers-cs_versions#cs_v114)가 인증되었음을 추가했습니다. </li>
  <li><strong>참조 주제</strong>: [사용자 액세스](/docs/containers?topic=containers-access_reference) 및 [CLI](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli) 참조 페이지에 있는 다양한 서비스 바인딩, 즉 `logging` 및 `nlb` 오퍼레이션에 대한 정보를 업데이트했습니다. </li></ul></td>
</tr>
<tr>
  <td>2019년 5월 7일</td>
  <td><ul>
  <li><strong>클러스터 DNS 제공자</strong>: Kubernetes 1.14 이상을 실행하는 클러스터가 CoreDNS만 지원함에 따라 [CoreDNS의 이점을 설명](/docs/containers?topic=containers-cluster_dns)했습니다. </li>
  <li><strong>에지 노드</strong>: [에지 노드](/docs/containers?topic=containers-edge)에 대한 사설 로드 밸런서 지원을 추가했습니다.</li>
  <li><strong>무료 클러스터</strong>: [무료 클러스터](/docs/containers?topic=containers-regions-and-zones#regions_free)가 지원되는 위치를 명시했습니다. </li>
  <li><strong>새로운 통합</strong>: [{{site.data.keyword.Bluemix_notm}} 서비스 및 서드파티 통합](/docs/containers?topic=containers-ibm-3rd-party-integrations), [인기 있는 통합](/docs/containers?topic=containers-supported_integrations) 및 [파트너십](/docs/containers?topic=containers-service-partners)에 대한 정보를 추가하고 재구성했습니다. </li>
  <li><strong>새로운 Kubernetes 버전 1.14</strong>: [Kubernetes 1.14](/docs/containers?topic=containers-cs_versions#cs_v114)에 대한 클러스터를 작성하거나 업데이트합니다.</li>
  <li><strong>더 이상 사용되지 않는 Kubernetes 버전 1.11</strong>: [Kubernetes 1.11](/docs/containers?topic=containers-cs_versions#cs_v111)에서 실행하는 모든 클러스터가 지원되지 않는 상태가 되기 전에 [업데이트](/docs/containers?topic=containers-update)합니다. </li>
  <li><strong>권한</strong>: [내 클러스터 사용자에게 제공하는 액세스 정책은 무엇입니까?](/docs/containers?topic=containers-faqs#faq_access)라는 FAQ를 추가했습니다.</li>
  <li><strong>작업자 풀</strong>: [기존 작업자 풀에 레이블을 적용](/docs/containers?topic=containers-add_workers#worker_pool_labels)하는 방법에 대한 지시사항을 추가했습니다.</li>
  <li><strong>참조 주제</strong>: Kubernetes 1.14와 같은 새 기능을 지원하기 위해 [변경 로그](/docs/containers?topic=containers-changelog#changelog) 참조 페이지가 업데이트됩니다.</li></ul></td>
</tr>
<tr>
  <td>2019년 5월 1일</td>
  <td><strong>인프라 액세스 지정</strong>: [지원 케이스를 열기 위해 IAM 권한을 지정하는 단계](/docs/containers?topic=containers-users#infra_access)를 수정했습니다.</td>
</tr>
</tbody></table>


