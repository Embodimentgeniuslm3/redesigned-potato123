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


#  {{site.data.keyword.containerlong_notm}} 사용 시 사용자의 책임
{: #responsibilities_iks}

{{site.data.keyword.containerlong}} 사용 시 지켜야 하는 클러스터 관리 책임 및 이용 약관에 대해 알아보십시오.
{:shortdesc}

## 클러스터 관리 책임
{: #responsibilities}

IBM은 {{site.data.keyword.Bluemix_notm}} DevOps, AI, 데이터 및 보안 서비스와 함께 앱을 배치할 수 있도록 엔터프라이즈 클라우드 플랫폼을 제공합니다. 클라우드에서 앱 및 서비스를 설정하고, 통합하고, 운영하는 방식을 선택할 수 있습니다.
{:shortdesc}

<table summary="이 표에서는 IBM과 사용자의 책임에 대해 설명합니다. 행은 왼쪽에서 오른쪽으로 읽어야 하며, 아이콘이 1열의 각 책임, 2열의 설명을 나타냅니다.">
<caption>IBM과 사용자의 책임</caption>
  <thead>
  <th colspan=2>유형별 책임</th>
  </thead>
  <tbody>
    <tr>
    <td align="center"><img src="images/icon_clouddownload.svg" alt="아래쪽 화살표가 포함된 클라우드의 아이콘"/><br>클라우드 인프라</td>
    <td>
    **IBM 책임**:
    <ul><li>각 클러스터마다 안전한 IBM 소유 인프라 계정에 완전히 관리된 고가용성 전용 마스터를 배치하십시오.</li>
    <li>IBM Cloud 인프라(SoftLayer) 계정에서 작업자 노드를 프로비저닝하십시오.</li>
    <li>클러스터 관리 컴포넌트(예: VLAN 및 로드 밸런서)를 설정하십시오.</li>
    <li>추가 인프라에 대한 요청(예: 지속적 볼륨 클레임에 대한 응답으로 작업자 노드 추가 및 제거, 기본 서브넷 작성 및 스토리지 볼륨 프로비저닝)을 수행하십시오.</li>
    <li>자동으로 클러스터 아키텍처에서 작동하도록 주문된 인프라 리소스를 통합하고 배치된 앱 및 워크로드에서 사용 가능하도록 설정하십시오.</li></ul>
    <br><br>
    **사용자 책임**:
    <ul><li>제공된 API, CLI 또는 콘솔 도구를 사용하여 [컴퓨팅](/docs/containers?topic=containers-clusters#clusters) 및 [스토리지](/docs/containers?topic=containers-storage_planning#storage_planning) 용량을 조정하고 [네트워킹 구성](/docs/containers?topic=containers-cs_network_cluster#cs_network_cluster)을 조정하여 워크로드의 요구사항을 충족하십시오.</li></ul><br><br>
    </td>
     </tr>
     <tr>
     <td align="center"><img src="images/icon_tools.svg" alt="렌치 아이콘"/><br>관리 클러스터</td>
     <td>
     **IBM 책임**:
     <ul><li>도구 모음을 제공하여 클러스터 관리(예: {{site.data.keyword.containerlong_notm}} [API ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://containers.cloud.ibm.com/global/swagger-global-api/), [CLI 플러그인](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli) 및 [콘솔 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/kubernetes/clusters))를 자동화하십시오.</li>
     <li>Kubernetes 마스터 패치 OS, 버전 및 보안 업데이트를 자동으로 적용하십시오. 적용할 수 있는 주 및 부 업데이트를 사용 가능하도록 설정하십시오.</li>
     <li>클러스터 내의 운영 {{site.data.keyword.containerlong_notm}} 및 Kubernetes 컴포넌트를 업데이트하고 복구하십시오(예: Ingress 애플리케이션 로드 밸런서 및 파일 스토리지 플러그인),</li>
     <li>etcd에 Kubernetes 워크로드 구성 파일과 같은 데이터를 백업하고 복구하십시오.</li>
     <li>클러스터 작성 시 마스터와 작업자 노드 간의 OpenVPN 연결을 설정하십시오.</li>
     <li>여러 인터페이스에서 마스터 및 작업자 노드의 상태를 모니터하고 보고하십시오.</li>
     <li>작업자 노드 주, 부 및 패치 OS, 버전 및 보안 업데이트를 제공하십시오.</li>
     <li>작업자 노드를 업데이트하고 복구하기 위한 자동화 요청을 수행하십시오. 선택적인 [작업자 노드 자동 복구](/docs/containers?topic=containers-health#autorecovery)를 제공하십시오.</li>
     <li>[클러스터 오토스케일러](/docs/containers?topic=containers-ca#ca)와 같은 도구를 제공하여 클러스터 인프라를 확장하십시오.</li>
     </ul>
     <br><br>
     **사용자 책임**:
     <ul>
     <li>API, CLI 또는 콘솔 도구를 사용하여 제공된 주 및 부 Kubernetes 업데이트 및 주 부 및 패치 작업자 노드 업데이트를 [적용](/docs/containers?topic=containers-update#update)하십시오.</li>
     <li>API, CLI 또는 콘솔 도구를 사용하여 인프라 리소스를 [복구](/docs/containers?topic=containers-cs_troubleshoot#cs_troubleshoot)하거나 선택적인 [작업자 노드 자동 복구](/docs/containers?topic=containers-health#autorecovery)를 설정하고 구성하십시오.</li></ul>
     <br><br></td>
      </tr>
    <tr>
      <td align="center"><img src="images/icon_locked.svg" alt="자물쇠의 아이콘"/><br>보안이 강화된 환경</td>
      <td>
      **IBM 책임**:
      <ul>
      <li>[다양한 산업 규제 준수](/docs/containers?topic=containers-faqs#standards)에 적합한 제어를 유지보수하십시오.</li>
      <li>클러스터 마스터를 모니터하고, 분리하고, 복구하십시오.</li>
      <li>마스터 가동 중단으로부터 보호하도록 Kubernetes 마스터 API 서버, etcd, 스케줄러 및 제어 관리자 컴포넌트의 고가용성 복제본을 제공하십시오.</li>
      <li>자동으로 마스터 보안 패치 업데이트를 적용하고 작업자 노드 보안 패치 업데이트를 제공하십시오.</li>
      <li>특정 보안 설정을 사용으로 설정하십시오(예: 작업자 노드의 암호화된 디스크).</li>
      <li>작업자 노드에 대해 안전하지 않은 특정 조치를 사용 안함으로 설정하십시오(예: 사용자가 호스트에 SSH를 설정하도록 허용하지 않음).</li>
      <li>TLS를 통해 마스터와 작업자 노드 간의 통신을 암호화하십시오.</li>
      <li>작업자 노드 운영 체제에 대한 CIS 준수 Linux 이미지를 제공하십시오.</li>
      <li>취약성 및 보안 규제 준수 문제를 발견하도록 마스터 및 작업자 노드 이미지를 계속해서 모니터하십시오.</li>
      <li>작업자 노드를 두 개의 로컬 SSD, AES 256비트 암호화된 데이터 파티션으로 프로비저닝하십시오.</li>
      <li>클러스터 네트워크 연결을 위한 옵션을 제공하십시오(예: 공용 및 사설 서비스 엔드포인트).</li>
      <li>컴퓨팅 격리를 위한 옵션을 제공하십시오(예: 전용 가상 머신, 베어메탈, 신뢰할 수 있는 컴퓨팅이 포함된 베어메탈).</li>
      <li>Kubernetes 역할 기반 액세스 제어(RBAC)와 {{site.data.keyword.Bluemix_notm}} Identity and Access Management(IAM)를 통합하십시오.</li>
      </ul>
      <br><br>
      **사용자 책임**:
      <ul>
      <li>API, CLI 또는 콘솔 도구를 사용하여 제공된 [보안 패치 업데이트](/docs/containers?topic=containers-changelog#changelog)를 작업자 노드에 적용하십시오.</li>
      <li>워크로드의 보안 및 규제 준수 요구사항을 충족시키도록 [클러스터 작업](/docs/containers?topic=containers-plan_clusters)을 설정하고 추가 [보안 설정](/docs/containers?topic=containers-security#security)을 구성하는 방법을 선택하십시오. 해당되는 경우 [방화벽](/docs/containers?topic=containers-firewall#firewall)을 구성하십시오.</li></ul>
      <br><br></td>
      </tr>

      <tr>
        <td align="center"><img src="images/icon_code.svg" alt="코드 대괄호의 아이콘"/><br>앱 오케스트레이션</td>
        <td>
        **IBM 책임**:
        <ul>
        <li>Kubernetes API에 액세스할 수 있도록 Kubernetes 컴포넌트를 설치하여 클러스터를 프로비저닝하십시오.</li>
        <li>많은 관리 추가 기능을 제공하여 앱의 기능을 제공하십시오(예: [Istio](/docs/containers?topic=containers-istio#istio) 및 [Knative](/docs/containers?topic=containers-serverless-apps-knative)). IBM이 관리 추가 기능에 적합한 설치 및 업데이트를 제공하므로 유지보수가 단순화됩니다.</li>
        <li>선택한 서드파티 파트너십 기술(예: {{site.data.keyword.la_short}}, {{site.data.keyword.mon_short}} 및 Portworx)과의 클러스터 통합을 제공하십시오.</li>
        <li>기타 {{site.data.keyword.Bluemix_notm}} 서비스에 대한 서비스 바인딩을 사용으로 설정하도록 자동화를 제공하십시오.</li>
        <li>`default` Kubernetes 네임스페이스의 배치가 {{site.data.keyword.registrylong_notm}}에서 이미지를 가져올 수 있도록 이미지 풀 시크릿으로 클러스터를 작성하십시오.</li>
        <li>앱에 사용할 지속적 볼륨을 지원하도록 스토리지 클래스 및 플러그인을 제공하십시오.</li>
        <li>앱을 외부로 노출하기 위해 사용하도록 예약된 서브넷 IP 주소를 사용하여 클러스터를 작성하십시오.</li>
        <li>서비스를 외부로 노출하기 위해 원시 Kubernetes 공용 및 사설 로드 밸런서 및 Igress 라우트를 지원하십시오.</li>
        </ul>
        <br><br>
        **사용자 책임**:
        <ul>
        <li>제공된 도구 및 기능을 사용하여 [구성 및 배치](/docs/containers?topic=containers-app#app), [권한 설정](/docs/containers?topic=containers-users#users), [기타 서비스와의 통합](/docs/containers?topic=containers-supported_integrations#supported_integrations), [외부로 제공](/docs/containers?topic=containers-cs_network_planning#cs_network_planning), [상태 모니터](/docs/containers?topic=containers-health#health), [데이터 저장, 백업 및 복원](/docs/containers?topic=containers-storage_planning#storage_planning)을 위해 제공된 도구 및 기능을 사용하십시오. 그렇지 않으면 [고가용성](/docs/containers?topic=containers-ha#ha) 및 복원성 워크로드를 관리하십시오.</li>
        </ul>
        </td>
        </tr>
  </tbody>
  </table>

<br />


## {{site.data.keyword.containerlong_notm}}의 오용
{: #terms}

클라이언트는 {{site.data.keyword.containerlong_notm}}를 오용할 수 없습니다.
{:shortdesc}

오용에는 다음이 포함됩니다.

*   불법적 활동
*   맬웨어의 배포 또는 실행
*   {{site.data.keyword.containerlong_notm}}에 손상을 주거나 사용자의 {{site.data.keyword.containerlong_notm}} 사용을 방해함
*   기타 서비스 또는 시스템에 손상을 주거나 사용자의 사용을 방해함
*   서비스 또는 시스템에 대한 비인가된 액세스
*   서비스 또는 시스템의 비인가된 수정
*   기타 사용자의 권한 위반

전체 이용 약관은 [클라우드 서비스 이용 약관](/docs/overview/terms-of-use?topic=overview-terms#terms)을 참조하십시오.
