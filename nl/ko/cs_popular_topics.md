---

copyright:
  years: 2014, 2019
lastupdated: "2019-04-15"

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



# {{site.data.keyword.containerlong_notm}}에 대한 인기 있는 주제
{: #cs_popular_topics}

{{site.data.keyword.containerlong}}에서 무슨 일이 일어나고 있는지 지속적으로 확인하십시오. 탐색할 새로운 기능, 사용해 볼 트릭 또는 다른 개발자가 현재 유용하다고 생각하는 인기 있는 주제에 대해 알아보십시오.
{:shortdesc}



## 2019년 4월의 인기 있는 주제
{: #apr19}

<table summary="이 표는 인기 있는 주제를 보여줍니다. 행은 왼쪽에서 오른쪽 방향으로 읽어야 하며 1열에는 날짜, 2열에는 기능 제목, 3열에는 설명이 있습니다.">
<caption>2019년 4월의 컨테이너 및 Kubernetes 클러스터에 대한 인기 있는 주제</caption>
<thead>
<th>날짜</th>
<th>제목</th>
<th>설명</th>
</thead>
<tbody>
<tr>
<td>2019년 4월 15일</td>
<td>[네트워크 로드 밸런서(NLB) 호스트 이름 등록](/docs/containers?topic=containers-loadbalancer#loadbalancer_hostname)</td>
<td>앱을 인터넷에 노출하도록 공용 네트워크 로드 밸런서(NLB)를 설정한 후 호스트 이름을 작성하여 NLB IP에 대한 DNS 항목을 작성할 수 있습니다. {{site.data.keyword.Bluemix_notm}}가 사용자에게 적합한 호스트 이름에 대한 와일드카드 SSL 인증서의 생성 및 유지보수를 관리합니다. TCP/HTTP(S) 모니터를 설정하여 각 호스트 이름 뒤에 있는 NLB IP 주소의 상태를 검사할 수도 있습니다.
</td>
</tr>
<tr>
<td>2019년 4월 8일</td>
<td>[웹 브라우저의 Kubernetes 터미널(베타)](/docs/containers?topic=containers-cs_cli_install#cli_web)</td>
<td>{{site.data.keyword.Bluemix_notm}} 콘솔에서 클러스터 대시보드를 사용하여 클러스터를 관리하지만 추가 고급 구성 변경사항을 빠르게 수행할 경우 Kubernetes 터미널의 웹 브라우저에서 직접 CLI 명령을 실행할 수 있습니다. 클러스터의 세부사항 페이지에서 **터미널** 단추를 클릭하여 Kubernetes 터미널을 실행하십시오. Kubernetes 터미널이 베타 추가 기능으로 릴리스되며 프로덕션 클러스터에서 사용할 수 없습니다. </td>
</tr>
<tr>
<td>2019년 4월 4일</td>
<td>[현재 시드니에서의 고가용성 마스터](/docs/containers?topic=containers-regions-and-zones#zones)</td>
<td>시드니를 포함하여 다중 구역 메트로 위치에서 [클러스터를 작성](/docs/containers?topic=containers-clusters#clusters_ui)하면 고가용성을 위해 구역에 Kubernetes 마스터 복제본이 분산됩니다. </td>
</tr>
</tbody></table>

## 2019년 3월의 인기 있는 주제
{: #mar19}

<table summary="이 표는 인기 있는 주제를 보여줍니다. 행은 왼쪽에서 오른쪽 방향으로 읽어야 하며 1열에는 날짜, 2열에는 기능 제목, 3열에는 설명이 있습니다.">
<caption>2019년 3월의 컨테이너 및 Kubernetes 클러스터에 대한 인기 있는 주제</caption>
<thead>
<th>날짜</th>
<th>제목</th>
<th>설명</th>
</thead>
<tbody>
<tr>
<td>2019년 3월 21일</td>
<td>Kubernetes 클러스터 마스터용 개인 서비스 엔드포인트 소개</td>
<td>기본적으로 {{site.data.keyword.containerlong_notm}}는 공용 및 사설 VLAN에 대한 액세스 권한으로 클러스터를 설정합니다. 이전에는 [사설 VLAN 전용 클러스터](/docs/containers?topic=containers-plan_clusters#private_clusters)를 사용하려면, 클러스터의 작업자 노드를 마스터와 연결하는 게이트웨이 어플라이언스를 설정해야 했습니다. 이제 개인 서비스 엔드포인트를 사용할 수 있습니다. 개인 서비스 엔드포인트를 사용으로 설정하면 게이트웨이 어플라이언스 디바이스를 사용할 필요없이 작업자 노드와 마스터 간의 모든 트래픽이 사설 네트워크을 사용합니다. 이러한 강화된 보안 외에도, 사설 네트워크의 인바운드 및 아웃바운드 트래픽은 [무제한이며 비용이 청구되지 않습니다 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/cloud/bandwidth). 인터넷을 통해 Kubernetes 마스터에 안전하게 액세스하기 위해 공용 서비스 엔드포인트를 계속 유지할 수 있습니다(예를 들어, 사설 네트워크에 있지 않고 `kubectl` 명령을 실행할 수 있음).<br><br>
개인 서비스 엔드포인트를 사용하려면 IBM Cloud 인프라(SoftLayer) 계정에 [VRF](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) 및 [서비스 엔드포인트](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started)를 사용으로 설정해야 합니다. 클러스터는 Kubernetes 버전 1.11 이상을 실행해야 합니다. 클러스터가 이전 Kubernetes 버전을 실행하는 경우, [1.11 이상으로 업그레이드](/docs/containers?topic=containers-update#update)하십시오. 자세한 정보는 다음 링크를 확인하십시오.<ul>
<li>[서비스 엔드포인트를 사용한 마스터와 작업자 간 통신 이해](/docs/containers?topic=containers-cs_network_ov#cs_network_ov_master)</li>
<li>[개인 서비스 엔드포인트 설정](/docs/containers?topic=containers-cs_network_cluster#set-up-private-se)</li>
<li>[공용에서 개인 서비스 엔드포인트로 전환](/docs/containers?topic=containers-cs_network_cluster#migrate-to-private-se)</li>
<li>사설 네트워크에 방화벽이 있는 경우, [{{site.data.keyword.containerlong_notm}}, {{site.data.keyword.registrylong_notm}} 및 기타 {{site.data.keyword.Bluemix_notm}} 서비스용 사설 IP 주소 추가](/docs/containers?topic=containers-firewall#firewall_outbound)</li>
</ul>
<p class="important">개인 전용 서비스 엔드포인트 클러스터로 전환하는 경우에는 클러스터가 사용 중인 다른 {{site.data.keyword.Bluemix_notm}} 서비스와 계속 통신할 수 있는지 확인하십시오. [Portworx 소프트웨어 정의 스토리지(SDS)](/docs/containers?topic=containers-portworx#portworx) 및 [클러스터 오토스케일러](/docs/containers?topic=containers-ca#ca)는 개인 전용 서비스 엔드포인트를 지원하지 않습니다. 대신 공용 및 개인 서비스 엔드포인트를 사용하는 클러스터를 사용하십시오. 클러스터에서 Kubernetes 버전 1.13.4_1513, 1.12.6_1544, 1.11.8_1550, 1.10.13_1551 이상을 실행하는 경우 [NFS 기반 파일 스토리지](/docs/containers?topic=containers-file_storage#file_storage)가 지원됩니다. </p>
</td>
</tr>
<tr>
<td>2019년 3월 7일</td>
<td>[클러스터 오토스케일러가 베타에서 GA로 변경됨](/docs/containers?topic=containers-ca#ca)</td>
<td>이제 클러스터 오토스케일러가 일반적으로 사용 가능합니다. Helm 플러그인을 설치하고 클러스터의 작업자 풀을 자동으로 스케일링하여 스케줄된 워크로드의 크기 결정 요구사항에 따라 작업자 노드 수를 늘리거나 줄이십시오.<br><br>
클러스터 오토스케일러에 대한 도움말 또는 피드백이 필요하십니까? 외부 사용자인 경우 [공용 Slack에 등록 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://bxcs-slack-invite.mybluemix.net/)하고 [#cluster-autoscaler ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://ibm-container-service.slack.com/messages/CF6APMLBB) 채널에 게시하십시오. IBM 직원인 경우 [내부 Slack ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://ibm-argonauts.slack.com/messages/C90D3KZUL) 채널에 게시하십시오.</td>
</tr>
</tbody></table>




## 2019년 2월의 인기 있는 주제
{: #feb19}

<table summary="이 표는 인기 있는 주제를 보여줍니다. 행은 왼쪽에서 오른쪽 방향으로 읽어야 하며 1열에는 날짜, 2열에는 기능 제목, 3열에는 설명이 있습니다.">
<caption>2019년 2월의 컨테이너 및 Kubernetes 클러스터에 대한 인기 있는 주제</caption>
<thead>
<th>날짜</th>
<th>제목</th>
<th>설명</th>
</thead>
<tbody>
<tr>
<td>2월 25일</td>
<td>{{site.data.keyword.registrylong_notm}}에서 이미지 가져오기를 보다 세부적으로 제어함</td>
<td>워크로드를 {{site.data.keyword.containerlong_notm}} 클러스터에 배치하는 경우 이제 컨테이너는 [{{site.data.keyword.registrylong_notm}}의 새 `icr.io` 도메인 이름](/docs/services/Registry?topic=registry-registry_overview#registry_regions)에서 이미지를 가져올 수 있습니다. 또한, {{site.data.keyword.Bluemix_notm}} IAM의 정밀한 액세스 정책을 사용하여 이미지에 대한 액세스를 제어할 수 있습니다. 자세한 정보는 [이미지를 가져올 수 있는 권한을 클러스터에 부여하는 방법 이해](/docs/containers?topic=containers-images#cluster_registry_auth)를 참조하십시오.</td>
</tr>
<tr>
<td>2월 21일</td>
<td>[멕시코의 사용 가능한 구역](/docs/containers?topic=containers-regions-and-zones#regions-and-zones)</td>
<td>미국 남부 지역 클러스터에 대한 새 구역으로서 멕시코(`mex01`)를 소개합니다. 방화벽이 있는 경우에는 이 구역 및 클러스터가 있는 지역 내의 다른 구역에 대해 반드시 [방화벽 포트를 여십시오](/docs/containers?topic=containers-firewall#firewall_outbound).</td>
</tr>
<tr><td>2019년 2월 6일</td>
<td>[Istio on {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-istio)</td>
<td>Istio on {{site.data.keyword.containerlong_notm}}는 Istio의 완벽한 설치, Istio 제어 플레인 컴포넌트의 자동 업데이트 및 라이프사이클 관리, 플랫폼 로깅 및 모니터링 도구와의 통합을 제공합니다. 한 번의 클릭으로 Istio 핵심 컴포넌트, 추가 추적, 모니터링 및 시각화를 모두 확보할 수 있고 BookInfo 샘플 앱을 시작하고 실행할 수 있습니다. Istio on {{site.data.keyword.containerlong_notm}}는 관리 추가 기능으로 제공되므로 {{site.data.keyword.Bluemix_notm}}는 모든 Istio 컴포넌트를 최신 상태로 유지합니다.</td>
</tr>
<tr>
<td>2019년 2월 6일</td>
<td>[Knative on {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-knative_tutorial)</td>
<td>Knative는 Kubernetes 클러스터 맨 위에 최신의, 소스 중심으로 컨테이너화된 서버리스 앱을 작성할 수 있도록 Kubernetes의 기능을 확장하는 오픈 소스 플랫폼입니다. Managed Knative on {{site.data.keyword.containerlong_notm}}는 Kubernetes 클러스터와 Knative 및 Istio를 직접 통합하는 관리 추가 기능입니다. 추가 기능의 Knative 및 Istio 버전은 IBM에서 테스트되며 {{site.data.keyword.containerlong_notm}}에서 사용할 수 있도록 지원됩니다.</td>
</tr>
</tbody></table>


## 2019년 1월의 인기 있는 주제
{: #jan19}

<table summary="이 표는 인기 있는 주제를 보여줍니다. 행은 왼쪽에서 오른쪽 방향으로 읽어야 하며 1열에는 날짜, 2열에는 기능 제목, 3열에는 설명이 있습니다.">
<caption>2019년 1월의 컨테이너 및 Kubernetes 클러스터에 대한 인기 있는 주제</caption>
<thead>
<th>날짜</th>
<th>제목</th>
<th>설명</th>
</thead>
<tbody>
<tr>
<td>1월 30일</td>
<td>{{site.data.keyword.Bluemix_notm}} IAM 서비스 액세스 역할 및 Kubernetes 네임스페이스</td>
<td>{{site.data.keyword.containerlong_notm}}는 이제 {{site.data.keyword.Bluemix_notm}} IAM [서비스 액세스 역할](/docs/containers?topic=containers-access_reference#service)을 지원합니다. 이러한 서비스 액세스 역할은 [Kubernetes RBAC](/docs/containers?topic=containers-users#role-binding)에 맞춰 팟(Pod) 또는 배치와 같은 Kubernetes 리소스를 관리하기 위해 클러스터에서 `kubectl` 조치를 수행하도록 사용자에게 권한을 부여합니다. 또한 {{site.data.keyword.Bluemix_notm}} IAM 서비스 액세스 역할을 사용하여 클러스터의 [특정 Kubernetes 네임스페이스에 대한 사용자 액세스를 제한](/docs/containers?topic=containers-users#platform)할 수 있습니다. [플랫폼 액세스 역할](/docs/containers?topic=containers-access_reference#iam_platform)은 이제 작업자 노드와 같은 클러스터 인프라를 관리하기 위해 `ibmcloud ks` 조치를 수행하도록 사용자에게 권한을 부여하는 데 사용됩니다.<br><br>{{site.data.keyword.Bluemix_notm}} IAM 서비스 액세스 역할은 이전에 IAM 플랫폼 액세스 역할이 있었던 사용자의 권한을 기반으로 기존 {{site.data.keyword.containerlong_notm}} 계정과 클러스터에 자동으로 추가됩니다. 앞으로는 IAM을 사용하여 액세스 그룹을 작성하고 액세스 그룹에 사용자를 추가하고 그룹 플랫폼 또는 서비스 액세스 역할을 지정할 수 있습니다. 자세한 정보는 블로그, [클러스터 액세스를 보다 세밀하게 제어하기 위한 IAM의 서비스 역할 및 네임스페이스 소개 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/blogs/bluemix/2019/02/introducing-service-roles-and-namespaces-in-iam-for-more-granular-control-of-cluster-access/)를 확인하십시오.</td>
</tr>
<tr>
<td>1월 8일</td>
<td>[클러스터 오토스케일러 미리보기 베타](/docs/containers?topic=containers-ca#ca)</td>
<td>클러스터의 작업자 풀을 자동으로 스케일링하여 스케줄된 워크로드의 크기 결정 요구사항에 따라 작업자 풀의 작업자 노드 수를 늘리거나 줄입니다. 이 베타를 테스트하려면 화이트리스트에 대한 액세스를 요청해야 합니다.</td>
</tr>
<tr>
<td>1월 8일</td>
<td>[{{site.data.keyword.containerlong_notm}} 진단 및 디버그 도구](/docs/containers?topic=containers-cs_troubleshoot#debug_utility)</td>
<td>문제를 해결하는 데 필요한 모든 YAML 파일 및 기타 정보를 얻는 것이 어려운 경우가 있으십니까? 클러스터, 배치 및 네트워크 정보를 수집하는 데 도움이되는 그래픽 사용자 인터페이스를 위한 {{site.data.keyword.containerlong_notm}} 진단 및 디버그 도구를 사용해 보십시오.</td>
</tr>
</tbody></table>

## 2018년 12월의 인기 있는 주제
{: #dec18}

<table summary="이 표는 인기 있는 주제를 보여줍니다. 행은 왼쪽에서 오른쪽 방향으로 읽어야 하며 1열에는 날짜, 2열에는 기능 제목, 3열에는 설명이 있습니다.">
<caption>2018년 12월의 컨테이너 및 Kubernetes 클러스터에 대한 인기 있는 주제</caption>
<thead>
<th>날짜</th>
<th>제목</th>
<th>설명</th>
</thead>
<tbody>
<tr>
<td>12월 11일</td>
<td>Portworx를 사용한 SDS 지원</td>
<td>컨테이너화된 데이터베이스와 기타 stateful 앱에 대한 지속적 스토리지를 관리하거나 다중 구역 간 팟(Pod) 사이의 데이터를 Portworx를 사용하여 공유합니다. [Portworx ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://portworx.com/products/introduction/)는 작업자 노드의 로컬 블록 스토리지를 관리하여 앱에 대한 통합된 지속적 스토리지 계층을 작성하는 고가용성 소프트웨어 정의 스토리지 솔루션(SDS)입니다. Portworx는 다중 작업자 노드에서 각 컨테이너 레벨 볼륨의 볼륨 복제를 사용하여 구역 간 데이터 지속성 및 데이터 접근성을 보장합니다. 자세한 정보는 [Portworx를 사용하는 소프트웨어 정의 스토리지(SDS)에 데이터 저장](/docs/containers?topic=containers-portworx#portworx)을 참조하십시오.    </td>
</tr>
<tr>
<td>12월 6일</td>
<td>{{site.data.keyword.mon_full_notm}}</td>
<td>메트릭을 {{site.data.keyword.monitoringlong}}에 전달하기 위해 Sysdig를 작업자 노드에 서드파티의 서비스로 배치하여 앱의 성능과 상태에 대한 작동 가시성을 얻을 수 있습니다. 자세한 정보는 [Kubernetes 클러스터에서 앱에 대한 메트릭 분석](/docs/services/Monitoring-with-Sysdig/tutorials?topic=Sysdig-kubernetes_cluster#kubernetes_cluster)을 참조하십시오. **참고**:Kubernetes 버전 1.11 이상을 실행하는 클러스터에서 {{site.data.keyword.mon_full_notm}}을 사용할 경우 Sysdig가 현재 `containerd`를 지원하지 않으므로 일부 컨테이너만 수집됩니다.</td>
</tr>
<tr>
<td>12월 4일</td>
<td>[작업자 노드 리소스 예약](/docs/containers?topic=containers-plan_clusters#resource_limit_node)</td>
<td>클러스터 용량 초과에 대해 걱정할 정도로 많은 앱을 배치하고 있으십니까? 작업자 노드 리소스 예약 및 Kubernetes 제거 기능은 클러스터가 계속 실행될 수 있는 충분한 리소스를 확보하도록 클러스터의 컴퓨팅 용량을 보호합니다. 작업자 노드를 몇 개만 더 추가하면 팟(Pod)이 이에 대한 스케줄링을 자동으로 다시 시작합니다. 작업자 노드 리소스 예약은 최신 [버전 패치 변경](/docs/containers?topic=containers-changelog#changelog)으로 업데이트됩니다.</td>
</tr>
</tbody></table>

## 2018년 11월의 인기 있는 주제
{: #nov18}

<table summary="이 표는 인기 있는 주제를 보여줍니다. 행은 왼쪽에서 오른쪽 방향으로 읽어야 하며 1열에는 날짜, 2열에는 기능 제목, 3열에는 설명이 있습니다.">
<caption>2018년 11월의 컨테이너 및 Kubernetes 클러스터에 대한 인기 있는 주제</caption>
<thead>
<th>날짜</th>
<th>제목</th>
<th>설명</th>
</thead>
<tbody>
<tr>
<td>11월 29일</td>
<td>[센네이에서 사용 가능한 구역](/docs/containers?topic=containers-regions-and-zones)</td>
<td>AP 북부 지역의 클러스터에 대한 새 구역으로서 인도의 센네이를 소개합니다. 방화벽이 있는 경우에는 이 구역 및 클러스터가 있는 지역 내의 다른 구역에 대해 반드시 [방화벽 포트를 여십시오](/docs/containers?topic=containers-firewall#firewall).</td>
</tr>
<tr>
<td>11월 27일</td>
<td>{{site.data.keyword.la_full_notm}}</td>
<td>팟(Pod) 컨테이너에서 로그를 관리하기 위해 LogDNA를 작업자 노드에 서드파티 서비스로 배치하여 로그 관리 기능을 클러스터에 추가합니다. 자세한 정보는 [LogDNA로 {{site.data.keyword.loganalysisfull_notm}}에서 Kubernetes 클러스터 로그 관리](/docs/services/Log-Analysis-with-LogDNA/tutorials?topic=LogDNA-kube#kube)를 참조하십시오.</td>
</tr>
<tr>
<td>11월 7일</td>
<td>네트워크 로드 밸런서 2.0(베타)</td>
<td>클러스터 앱을 안전하게 공용으로 노출하기 위해 이제 [NLB 1.0 또는 2.0](/docs/containers?topic=containers-loadbalancer#planning_ipvs) 간에 선택할 수 있습니다.</td>
</tr>
<tr>
<td>11월 7일</td>
<td>Kubernetes 버전 1.12 사용 가능</td>
<td>이제 [Kubernetes 버전 1.12](/docs/containers?topic=containers-cs_versions#cs_v112)를 실행하는 클러스터를 업데이트하거나 작성할 수 있습니다! 1.12 클러스터는 기본적으로 고가용성 Kubernetes 마스터와 함께 제공됩니다.</td>
</tr>
<tr>
<td>11월 7일</td>
<td> 고가용성 마스터</td>
<td>고가용성 마스터를 Kubernetes 버전 1.10을 실행하는 클러스터에 사용할 수 있습니다! 취해야 하는 [준비 단계](/docs/containers?topic=containers-cs_versions#110_ha-masters)를 포함하여 1.11 클러스터의 이전 항목에서 설명한 모든 이점이 1.10 클러스터에 적용됩니다.</td>
</tr>
<tr>
<td>11월 1일</td>
<td>Kubernetes 버전 1.11을 실행하는 클러스터의 고가용성 마스터</td>
<td>단일 구역에서, 마스터는 고가용성이며 가동 중단(예: 클러스터 업데이트 중에)에 대해 보호할 수 있도록 Kubernetes API 서버, etcd, 스케줄러 및 제어기 관리자에 대해 별도 실제 호스트의 복제본을 포함합니다. 클러스터가 다중 구역 가능 구역에 있는 경우, 고가용성 마스터도 역시 구역 장애에 대해 보호하도록 도움을 주기 위해 구역 간에 전개됩니다.<br>취해야 할 조치는 [고가용성 클러스터 마스터로 업데이트](/docs/containers?topic=containers-cs_versions#ha-masters)를 참조하십시오. 이 준비 조치는 다음과 같은 경우에 적용됩니다.<ul>
<li>방화벽 또는 사용자 정의 Calico 네트워크 정책이 있는 경우</li>
<li>작업자 노드에서 호스트 포트 `2040` 또는 `2041`을 사용 중인 경우</li>
<li>마스터에 대한 클러스터 내부 액세스를 위해 클러스터 마스터 IP 주소를 사용한 경우</li>
<li>Calico 정책을 작성하기 위해 Calico API 또는 CLI(`calicoctl`)를 호출하는 자동화가 있는 경우</li>
<li>마스터에 대한 팟(Pod) 유출 액세스를 제어하기 위해 Kubernetes 또는 Calico 네트워크 정책을 사용하는 경우</li></ul></td>
</tr>
</tbody></table>

## 2018년 10월의 인기 있는 주제
{: #oct18}

<table summary="이 표는 인기 있는 주제를 보여줍니다. 행은 왼쪽에서 오른쪽 방향으로 읽어야 하며 1열에는 날짜, 2열에는 기능 제목, 3열에는 설명이 있습니다.">
<caption>2018년 10월의 컨테이너 및 Kubernetes 클러스터에 대한 인기 있는 주제</caption>
<thead>
<th>날짜</th>
<th>제목</th>
<th>설명</th>
</thead>
<tbody>
<tr>
<td>10월 25일</td>
<td>[밀라노의 사용 가능한 구역](/docs/containers?topic=containers-regions-and-zones)</td>
<td>EU 중부 지역의 새 유료 클러스터 구역으로서 이탈리아의 밀라노를 소개합니다. 이전에는 밀라노를 무료 클러스터에 대해서만 사용할 수 있었습니다. 방화벽이 있는 경우에는 이 구역 및 클러스터가 있는 지역 내의 다른 구역에 대해 반드시 [방화벽 포트를 여십시오](/docs/containers?topic=containers-firewall#firewall).</td>
</tr>
<tr>
<td>10월 22일</td>
<td>[새 런던 다중 구역 위치 `lon05`](/docs/containers?topic=containers-regions-and-zones#zones)</td>
<td>런던 다중 메트로 위치가 `lon02`에서 `lon02`보다 더 많은 인프라 리소스가 있는 구역인 새 `lon05` 구역으로 대체되었습니다. 새 다중 구역 클러스터는 `lon05`를 사용하여 작성하십시오. `lon02`의 기존 클러스터는 지원되지만 새 다중 구역 클러스터는 `lon05`를 대신 사용해야 합니다.</td>
</tr>
<tr>
<td>10월 5일</td>
<td>{{site.data.keyword.keymanagementservicefull}}(베타)와의 통합</td>
<td>[{{site.data.keyword.keymanagementserviceshort}}(베타)를 사용으로 설정](/docs/containers?topic=containers-encryption#keyprotect)하여 클러스터의 Kubernetes secret을 암호화할 수 있습니다.</td>
</tr>
<tr>
<td>10월 4일</td>
<td>[{{site.data.keyword.registrylong}}가 {{site.data.keyword.Bluemix_notm}} Identity and Access Management(IAM)와 통합됨](/docs/services/Registry?topic=registry-iam#iam)</td>
<td>{{site.data.keyword.Bluemix_notm}} IAM을 사용하여 이미지 풀, 푸시 및 빌드와 같은 레지스트리 리소스에 대한 액세스를 제어할 수 있습니다. 클러스터를 작성할 때는 클러스터가 레지스트리에 대해 작업할 수 있도록 레지스트리 토큰 또한 작성합니다. 따라서 클러스터를 작성하려면 계정 레벨 레지스트리 **관리자** 플랫폼 관리 역할이 필요합니다. 레지스트리 계정에 대해 {{site.data.keyword.Bluemix_notm}} IAM을 사용으로 설정하려면 [기존 사용자에 대한 정책 적용의 사용 설정](/docs/services/Registry?topic=registry-user#existing_users)을 참조하십시오.</td>
</tr>
<tr>
<td>10월 1일</td>
<td>[리소스 그룹](/docs/containers?topic=containers-users#resource_groups)</td>
<td>액세스 권한을 지정하고 사용량을 측정하는 데 도움을 주기 위해, 리소스 그룹을 사용하여 {{site.data.keyword.Bluemix_notm}} 리소스를 파이프라인, 부서 또는 기타 그룹으로 구분할 수 있습니다. 이제 {{site.data.keyword.containerlong_notm}}는 `default` 그룹 또는 사용자가 작성하는 다른 리소스 그룹에 클러스터를 작성하는 것을 지원합니다.</td>
</tr>
</tbody></table>

## 2018년 9월의 인기 있는 주제
{: #sept18}

<table summary="이 표는 인기 있는 주제를 보여줍니다. 행은 왼쪽에서 오른쪽 방향으로 읽어야 하며 1열에는 날짜, 2열에는 기능 제목, 3열에는 설명이 있습니다.">
<caption>2018년 9월의 컨테이너 및 Kubernetes 클러스터에 대한 인기 있는 주제</caption>
<thead>
<th>날짜</th>
<th>제목</th>
<th>설명</th>
</thead>
<tbody>
<tr>
<td>9월 25일</td>
<td>[새 구역 사용 가능](/docs/containers?topic=containers-regions-and-zones)</td>
<td>이제 앱을 배치할 위치를 더 다양하게 선택할 수 있습니다.
<ul><li>미국 남부의 새로운 두 구역으로서 산호세 `sjc03` 및 `sjc04`를 소개합니다. 방화벽이 있는 경우에는 이 구역 및 클러스터가 있는 지역 내의 다른 구역에 대해 반드시 [방화벽 포트를 여십시오](/docs/containers?topic=containers-firewall#firewall).</li>
<li>새로운 두 `tok04` 및 `tok05` 구역을 통해, 이제 도쿄 및 아시아/태평양 북부 지역에 [다중 구역 클러스터를 작성](/docs/containers?topic=containers-plan_clusters#multizone)할 수 있습니다.</li></ul></td>
</tr>
<tr>
<td>9월 5일</td>
<td>[오슬로의 사용 가능한 구역](/docs/containers?topic=containers-regions-and-zones)</td>
<td>EU 중부 지역의 새 구역으로서 노르웨이의 오슬로를 소개합니다. 방화벽이 있는 경우에는 이 구역 및 클러스터가 있는 지역 내의 다른 구역에 대해 반드시 [방화벽 포트를 여십시오](/docs/containers?topic=containers-firewall#firewall).</td>
</tr>
</tbody></table>

## 2018년 8월의 인기 있는 주제
{: #aug18}

<table summary="이 표는 인기 있는 주제를 보여줍니다. 행은 왼쪽에서 오른쪽 방향으로 읽어야 하며 1열에는 날짜, 2열에는 기능 제목, 3열에는 설명이 있습니다.">
<caption>2018년 8월의 컨테이너 및 Kubernetes 클러스터에 대한 인기 있는 주제</caption>
<thead>
<th>날짜</th>
<th>제목</th>
<th>설명</th>
</thead>
<tbody>
<tr>
<td>8월 31일</td>
<td>{{site.data.keyword.cos_full_notm}}가 이제 {{site.data.keyword.containerlong}}와 통합됨</td>
<td>Kubernetes 고유의 지속적 볼륨 클레임(PVC)을 사용하여 클러스터에 {{site.data.keyword.cos_full_notm}}를 프로비저닝합니다. {{site.data.keyword.cos_full_notm}}는 읽기 집약적 워크로드에, 그리고 다중 구역 클러스터의 여러 구역 간에 데이터를 저장하려는 경우에 최적으로 사용됩니다. 클러스터에서 [{{site.data.keyword.cos_full_notm}} 서비스 인스턴스를 작성](/docs/containers?topic=containers-object_storage#create_cos_service)하고 [{{site.data.keyword.cos_full_notm}} 플러그인을 설치](/docs/containers?topic=containers-object_storage#install_cos)하여 시작하십시오. </br></br>자신에게 적합한 스토리지 솔루션을 찾을 수 없습니까? 데이터를 분석하고 해당 데이터에 맞는 적합한 스토리지 솔루션을 선택하려면 [여기](/docs/containers?topic=containers-storage_planning#choose_storage_solution)에서 시작하십시오. </td>
</tr>
<tr>
<td>8월 14일</td>
<td>팟(Pod) 우선순위 지정을 위해 Kubernetes 버전 1.11로 클러스터 업데이트</td>
<td>[Kubernetes 버전 1.11](/docs/containers?topic=containers-cs_versions#cs_v111)로 클러스터를 업데이트하면 새 기능(예: [팟(Pod) 우선순위 지정](/docs/containers?topic=containers-pod_priority#pod_priority) 또는 `containerd`의 사용으로 증가된 컨테이너 런타임 성능)을 활용할 수 있습니다.</td>
</tr>
</tbody></table>

## 2018년 7월의 인기 있는 주제
{: #july18}

<table summary="이 표는 인기 있는 주제를 보여줍니다. 행은 왼쪽에서 오른쪽 방향으로 읽어야 하며 1열에는 날짜, 2열에는 기능 제목, 3열에는 설명이 있습니다.">
<caption>2018년 7월의 컨테이너 및 Kubernetes 클러스터에 대한 인기 있는 주제</caption>
<thead>
<th>날짜</th>
<th>제목</th>
<th>설명</th>
</thead>
<tbody>
<tr>
<td>7월 30일</td>
<td>[자체 Ingress 제어기 가져오기](/docs/containers?topic=containers-ingress#user_managed)</td>
<td>클러스터의 Ingress 제어기에 대해 매우 특정한 보안 또는 기타 사용자 정의 요구사항이 있습니까? 그렇다면 기본값 대신 자체 Ingress 제어기를 실행하고자 할 수 있습니다.</td>
</tr>
<tr>
<td>7월 10일</td>
<td>다중 구역 클러스터 도입</td>
<td>클러스터 가용성의 개선을 원하십니까? 이제 선택된 메트로 영역의 다중 구역 간에 클러스터를 전개할 수 있습니다. 자세한 정보는 [{{site.data.keyword.containerlong_notm}}에서 외부 링크 아이콘중 구역 클러스터 작성](/docs/containers?topic=containers-plan_clusters#multizone)을 참조하십시오.</td>
</tr>
</tbody></table>

## 2018년 6월의 인기 있는 주제
{: #june18}

<table summary="이 표는 인기 있는 주제를 보여줍니외부 링크 아이콘. 행은 왼쪽에서 오른쪽 방향으로 읽어야 하며 1열에는 날짜, 2열에는 기능 제목, 3열에는 설명이 있습니외부 링크 아이콘.">
<caption>2018년 6월의 컨테이너 및 Kubernetes 클러스터에 대한 인기 있는 주제</caption>
<thead>
<th>날짜</th>
<th>제목</th>
<th>설명</th>
</thead>
<tbody>
<tr>
<td>6월 13일</td>
<td>`bx` CLI 명령어가 `ic` CLI로 변경되었음</td>
<td>{{site.data.keyword.Bluemix_notm}} CLI의 최신 버전을 외부 링크 아이콘운로드하는 경우, 이제 `bx` 대신 `ic` 접두부를 사용하여 명령을 실행합니외부 링크 아이콘. 예를 들어, `ibmcloud ks clusters`를 실행하여 클러스터를 나열합니외부 링크 아이콘.</td>
</tr>
<tr>
<td>6월 12일</td>
<td>[팟(Pod) 보안 정책](/docs/containers?topic=containers-psp)</td>
<td>Kubernetes 1.10.3 이상을 실행하는 클러스터의 경우에는 {{site.data.keyword.containerlong_notm}}에서 팟(Pod)을 작성하고 업데이트할 수 있는 사용자에게 권한 부여하도록 팟(Pod) 보안 정책을 구성할 수 있습니외부 링크 아이콘.</td>
</tr>
<tr>
<td>6월 6일</td>
<td>[IBM 제공 Ingress 와일드카드 하위 도메인에 대한 TLS 지원](/docs/containers?topic=containers-ingress#wildcard_tls)</td>
<td>2018년 6월 6일에 또는 그 이후에 작성된 클러스터의 경우, IBM 제공 Ingress 하위 도메인 TLS 인증서는 이제 와일드카드 인증서이며 등록된 와일드카드 하위 도메인에 사용될 수 있습니외부 링크 아이콘. 2018년 6월 6일 이전에 작성된 클러스터의 경우, TLS 인증서는 현재 TLS 인증서가 갱신될 때 와일드카드 인증서로 업데이트됩니외부 링크 아이콘.</td>
</tr>
</tbody></table>

## 2018년 5월의 인기 있는 주제
{: #may18}


<table summary="이 표는 인기 있는 주제를 보여줍니외부 링크 아이콘. 행은 왼쪽에서 오른쪽 방향으로 읽어야 하며 1열에는 날짜, 2열에는 기능 제목, 3열에는 설명이 있습니외부 링크 아이콘.">
<caption>2018년 5월의 컨테이너 및 Kubernetes 클러스터에 대한 인기 있는 주제</caption>
<thead>
<th>날짜</th>
<th>제목</th>
<th>설명</th>
</thead>
<tbody>
<tr>
<td>5월 24일</td>
<td>[새 Ingress 하위 도메인 형식](/docs/containers?topic=containers-ingress)</td>
<td>5월 24일 이후에 작성된 클러스터에는 새 형식(<code>&lt;cluster_name&gt;.&lt;region&gt;.containers.appdomain.cloud</code>)의 Ingress 하위 도메인이 지정됩니외부 링크 아이콘. Ingress를 사용하여 앱을 노출할 때 새 하위 도메인을 사용하여 인터넷에서 앱에 액세스할 수 있습니외부 링크 아이콘.</td>
</tr>
<tr>
<td>5월 14일</td>
<td>[업데이트: 전세계에서 GPU 베어메탈에 워크로드 배치](/docs/containers?topic=containers-app#gpu_app)</td>
<td>클러스터에 [베어메탈 그래픽 처리 장치(GPU) 머신 유형](/docs/containers?topic=containers-plan_clusters#shared_dedicated_node)이 있는 경우 수학적으로 집약적인 앱을 스케줄할 수 있습니외부 링크 아이콘. 성능 향상을 위해 GPU 작업자 노드가 CPU 및 GPU 모두에서 앱의 워크로드를 처리할 수 있습니외부 링크 아이콘.</td>
</tr>
<tr>
<td>5월 3일</td>
<td>[컨테이너 이미지 보안 적용(베타)](/docs/services/Registry?topic=registry-security_enforce#security_enforce)</td>
<td>앱 컨테이너에서 가져올 이미지를 파악하기 위해 팀에 약간의 추가 도움이 필요합니까? 컨테이너 이미지를 배치하기 전에 확인하려면 컨테이너 이미지 보안 적용 베타를 사용해 보십시오. Kubernetes 1.9 이상을 실행하는 클러스터에 사용할 수 있습니외부 링크 아이콘.</td>
</tr>
<tr>
<td>5월 1일</td>
<td>[콘솔에서 Kubernetes 대시보드 배치](/docs/containers?topic=containers-app#cli_dashboard)</td>
<td>한 번의 클릭으로 Kubernetes 대시보드에 액세스하고 싶었던 적이 있습니까? {{site.data.keyword.Bluemix_notm}} 콘솔에서 **Kubernetes 대시보드** 단추를 확인하십시오.</td>
</tr>
</tbody></table>




## 2018년 4월의 인기 있는 주제
{: #apr18}

<table summary="이 표는 인기 있는 주제를 보여줍니외부 링크 아이콘. 행은 왼쪽에서 오른쪽 방향으로 읽어야 하며 1열에는 날짜, 2열에는 기능 제목, 3열에는 설명이 있습니외부 링크 아이콘.">
<caption>2018년 4월의 컨테이너 및 Kubernetes 클러스터에 대한 인기 있는 주제</caption>
<thead>
<th>날짜</th>
<th>제목</th>
<th>설명</th>
</thead>
<tbody>
<tr>
<td>4월 17일</td>
<td>{{site.data.keyword.Bluemix_notm}} Block Storage</td>
<td>지속적 데이터를 블록 스토리지에 저장하려면 {{site.data.keyword.Bluemix_notm}} Block Storage [플러그인](/docs/containers?topic=containers-block_storage#install_block)을 설치하십시오. 그런 외부 링크 아이콘음 클러스터에 대한 블록 스토리지를 [새로 작성](/docs/containers?topic=containers-block_storage#add_block)하거나 [기존 블록 스토리지를 사용](/docs/containers?topic=containers-block_storage#existing_block)할 수 있습니외부 링크 아이콘.</td>
</tr>
<tr>
<td>4월 13일</td>
<td>[Cloud Foundry 앱을 클러스터로 마이그레이션하는 데 대한 새 튜토리얼](/docs/containers?topic=containers-cf_tutorial#cf_tutorial)</td>
<td>Cloud Foundry 앱을 보유하고 계십니까? 이러한 앱과 동일한 코드를 Kubernetes 클러스터에서 실행되는 컨테이너에 배치하는 방법을 알아보십시오.</td>
</tr>
<tr>
<td>4월 5일</td>
<td>[로그 필터링](/docs/containers?topic=containers-health#filter-logs)</td>
<td>특정 로그가 전달되지 않도록 필터링하십시오. 특정 네임스페이스, 컨테이너 이름, 로그 레벨 및 메시지 문자열을 사용하여 로그를 필터링할 수 있습니외부 링크 아이콘.</td>
</tr>
</tbody></table>

## 2018년 3월의 인기 있는 주제
{: #mar18}

<table summary="이 표는 인기 있는 주제를 보여줍니외부 링크 아이콘. 행은 왼쪽에서 오른쪽 방향으로 읽어야 하며 1열에는 날짜, 2열에는 기능 제목, 3열에는 설명이 있습니외부 링크 아이콘.">
<caption>2018년 3월의 컨테이너 및 Kubernetes 클러스터에 대한 인기 있는 주제</caption>
<thead>
<th>날짜</th>
<th>제목</th>
<th>설명</th>
</thead>
<tbody>
<tr>
<td>3월 16일</td>
<td>[신뢰할 수 있는 컴퓨팅을 사용하여 베어메탈 클러스터 프로비저닝](/docs/containers?topic=containers-plan_clusters#shared_dedicated_node)</td>
<td>[Kubernetes 버전 1.9](/docs/containers?topic=containers-cs_versions#cs_v19) 이상을 실행하는 베어메탈 클러스터를 작성하고 신뢰할 수 있는 컴퓨팅을 사용하여 작업자 노드의 변조 여부를 확인합니외부 링크 아이콘.</td>
</tr>
<tr>
<td>3월 14일</td>
<td>[{{site.data.keyword.appid_full}}로 보안 로그인](/docs/containers?topic=containers-supported_integrations#appid)</td>
<td>사용자가 로그인하도록 하여 {{site.data.keyword.containerlong_notm}}에서 실행 중인 앱을 개선합니외부 링크 아이콘.</td>
</tr>
<tr>
<td>3월 13일</td>
<td>[상파울루의 사용 가능한 구역](/docs/containers?topic=containers-regions-and-zones)</td>
<td>미국 남부 지역의 새 구역으로서 브라질의 상파울루를 소개합니외부 링크 아이콘. 방화벽이 있는 경우에는 이 구역 및 클러스터가 있는 지역 내의 외부 링크 아이콘른 구역에 대해 반드시 [방화벽 포트를 여십시오](/docs/containers?topic=containers-firewall#firewall).</td>
</tr>
</tbody></table>

## 2018년 2월의 인기 있는 주제
{: #feb18}

<table summary="이 표는 인기 있는 주제를 보여줍니외부 링크 아이콘. 행은 왼쪽에서 오른쪽 방향으로 읽어야 하며 1열에는 날짜, 2열에는 기능 제목, 3열에는 설명이 있습니외부 링크 아이콘.">
<caption>2018년 2월의 컨테이너 및 Kubernetes 클러스터에 대한 인기 있는 주제</caption>
<thead>
<th>날짜</th>
<th>제목</th>
<th>설명</th>
</thead>
<tbody>
<tr>
<td>2월 27일</td>
<td>작업자 노드용 하드웨어 가상 머신(HVM) 이미지</td>
<td>HVM 이미지를 사용하여 워크로드의 I/O 성능을 향상시킵니외부 링크 아이콘. `ibmcloud ks worker-reload` [명령](/docs/containers?topic=containers-cs_cli_reference#cs_worker_reload) 또는 `ibmcloud ks worker-update` [명령](/docs/containers?topic=containers-cs_cli_reference#cs_worker_update)을 사용하여 기존의 각 작업자 노드에서 활성화합니외부 링크 아이콘.</td>
</tr>
<tr>
<td>2월 26일</td>
<td>[KubeDNS Auto-Scaling](https://kubernetes.io/docs/tasks/administer-cluster/dns-horizontal-autoscaling/)</td>
<td>이제 KubeDNS는 증가함에 따라 클러스터로 스케일링합니외부 링크 아이콘. `kubectl -n kube-system edit cm kube-dns-autoscaler` 명령을 사용하여 스케일링 할당을 조정할 수 있습니외부 링크 아이콘.</td>
</tr>
<tr>
<td>2월 23일</td>
<td>[로깅](/docs/containers?topic=containers-health#view_logs) 및 [메트릭](/docs/containers?topic=containers-health#view_metrics)에 대한 웹 콘솔 보기</td>
<td>향상된 웹 UI를 사용하여 클러스터 및 해당 컴포넌트의 로그 및 메트릭 데이터를 쉽게 볼 수 있습니외부 링크 아이콘. 액세스하려면 클러스터 세부사항 페이지를 참조하십시오.</td>
</tr>
<tr>
<td>2월 20일</td>
<td>암호화된 이미지 및 [서명되고 신뢰할 수 있는 컨텐츠](/docs/services/Registry?topic=registry-registry_trustedcontent#registry_trustedcontent)</td>
<td>{{site.data.keyword.registryshort_notm}}에서 이미지에 서명하고 암호화하여 저장소 네임스페이스에 저장할 때 무결성을 보장할 수 있습니외부 링크 아이콘. 신뢰할 수 있는 컨텐츠만 사용하여 컨테이너 인스턴스를 실행하십시오.</td>
</tr>
<tr>
<td>2월 19일</td>
<td>[strongSwan IPSec VPN 설정](/docs/containers?topic=containers-vpn#vpn-setup)</td>
<td>strongSwan IPSec VPN Helm 차트를 신속하게 배치하여 가상 라우터 어플라이언스 없이 {{site.data.keyword.containerlong_notm}} 클러스터를 안전하게 온프레미스 데이터센터에 연결합니외부 링크 아이콘.</td>
</tr>
<tr>
<td>2월 14일</td>
<td>[서울의 사용 가능한 구역](/docs/containers?topic=containers-regions-and-zones)</td>
<td>올림픽에 맞추어 AP 북부 지역의 서울에 Kubernetes 클러스터를 배치합니외부 링크 아이콘. 방화벽이 있는 경우에는 이 구역 및 클러스터가 있는 지역 내의 외부 링크 아이콘른 구역에 대해 반드시 [방화벽 포트를 여십시오](/docs/containers?topic=containers-firewall#firewall).</td>
</tr>
<tr>
<td>2월 8일</td>
<td>[Kubernetes 1.9 업데이트](/docs/containers?topic=containers-cs_versions#cs_v19)</td>
<td>Kubernetes 1.9로 업데이트하기 전에 클러스터에 작성할 변경사항을 검토하십시오.</td>
</tr>
</tbody></table>

## 2018년 1월의 인기 있는 주제
{: #jan18}

<table summary="이 표는 인기 있는 주제를 보여줍니외부 링크 아이콘. 행은 왼쪽에서 오른쪽 방향으로 읽어야 하며 1열에는 날짜, 2열에는 기능 제목, 3열에는 설명이 있습니외부 링크 아이콘.">
<caption>2018년 1월의 컨테이너 및 Kubernetes 클러스터에 대한 인기 있는 주제</caption>
<thead>
<th>날짜</th>
<th>제목</th>
<th>설명</th>
</thead>
<tbody>
<td>1월 25일</td>
<td>[글로벌 레지스트리 사용 가능](/docs/services/Registry?topic=registry-registry_overview#registry_regions)</td>
<td>{{site.data.keyword.registryshort_notm}}를 통해 글로벌 `registry.bluemix.net`을 사용하여 IBM에서 제공한 공용 이미지를 가져올 수 있습니외부 링크 아이콘.</td>
</tr>
<tr>
<td>1월 23일</td>
<td>[싱가포르 및 캐나외부 링크 아이콘 몬트리올의 사용 가능한 구역](/docs/containers?topic=containers-regions-and-zones)</td>
<td>싱가포르 및 몬트리올은 {{site.data.keyword.containerlong_notm}} AP 북부 및 미국 동부 지역에서 사용 가능한 구역입니외부 링크 아이콘. 방화벽이 있는 경우에는 이러한 구역 및 클러스터가 있는 지역 내의 기타 구역에 대해 반드시 [방화벽 포트를 여십시오](/docs/containers?topic=containers-firewall#firewall).</td>
</tr>
<tr>
<td>1월 8일</td>
<td>[향상된 특성(flavor) 사용 가능](/docs/containers?topic=containers-cs_cli_reference#cs_machine_types)</td>
<td>시리즈 2 가상 머신 유형에는 로컬 SSD 스토리지 및 디스크 암호화가 포함됩니외부 링크 아이콘. 향상된 성능 및 안전성을 위해 이러한 특성으로 [워크로드를 이동](/docs/containers?topic=containers-update#machine_type)하십시오.</td>
</tr>
</tbody></table>

## Slack에서 비슷한 생각을 가진 개발자들과 대화하기
{: #slack}

[{{site.data.keyword.containerlong_notm}} Slack ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://ibm-container-service.slack.com)에서 외부 링크 아이콘른 사용자가 이야기하는 내용을 보고 직접 질문할 수 있습니외부 링크 아이콘.
{:shortdesc}

{{site.data.keyword.Bluemix_notm}} 계정에 대해 IBM ID를 사용 중이 아닌 경우에는 이 Slack에 대한 [초대를 요청](https://bxcs-slack-invite.mybluemix.net/)하십시오.
{: tip}
