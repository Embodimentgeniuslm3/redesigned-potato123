---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-03"

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


# Kubernetes 전략 정의
{: #strategy}

{{site.data.keyword.containerlong}}를 사용하면 프로덕션에서 앱에 대한 컨테이너 워크로드를 빠르고 안전하게 배치할 수 있습니다. 클러스터 전략을 계획할 때 [Kubernetes![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/) 자동화된 배치, 스케일링 및 오케스트레이션 관리 도구를 최대한 활용하기 위해 설정을 최적화하려면 자세히 알아보십시오.
{:shortdesc}

## 워크로드를 {{site.data.keyword.Bluemix_notm}}로 이동
{: #cloud_workloads}

워크로드를 {{site.data.keyword.Bluemix_notm}}로 이동하면 총소유비용 감소, 보안 및 호환 환경에서 앱에 대한 고가용성 증가 및 사용자 요구에 대한 응답으로 스케일링 업 및 다운 등과 같은 이점이 있습니다. {{site.data.keyword.containerlong_notm}}는 컨테이너 기술을 오픈 소스 도구와 결합합니다. 벤더 잠금이 발생하지 않고 여러 클라우드 환경에서 마이그레이션할 수 있는 클라우드 원시 앱을 빌드할 수 있는 Kubernetes를 예로 들 수 있습니다.
{:shortdesc}

클라우드에 어떻게 연결할 수 있습니까? 필요한 옵션은 무엇입니까? 연결 후 워크로드를 관리할 수 있는 방법은 무엇입니까?

이 페이지를 사용하여 {{site.data.keyword.containerlong_notm}}의 Kubernetes 배치에 대한 일부 전략에 대해 알아보십시오. 또한 [Slack ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://ibm-container-service.slack.com)을 통해 언제든지 IBM 팀과 함께 작업해보십시오.

아직 Slack을 이용해보지 않으셨습니까? [초대를 요청하십시오!](https://bxcs-slack-invite.mybluemix.net/)
{: tip}

### {{site.data.keyword.Bluemix_notm}}로 이동할 수 있는 방법은 무엇입니까?
{: #move_to_cloud}

{{site.data.keyword.Bluemix_notm}}를 사용하면 Kubernetes 클러스터를 작성할 위치를 [오프프레미스, 온프레미스 또는 하이브리드 클라우드 환경](/docs/containers?topic=containers-cs_ov#differentiation) 중에서 유연하게 선택할 수 있습니다. 다음 표는 사용자가 일반적으로 다양한 클라우드 유형으로 이동하는 워크로드 유형에 대한 몇 가지 예를 제공합니다. 두 환경 모두에서 클러스터를 실행하는 하이브리드 접근법을 선택할 수도 있습니다.
{: shortdesc}

| 워크로드 | {{site.data.keyword.containershort_notm}} 오프프레미스 | 온프레미스 |
| --- | --- | --- |
| DevOps 인에이블먼트 도구 | <img src="images/confirm.svg" width="32" alt="사용 가능한 기능" style="width:32px;" /> | |
| 앱 개발 및 테스트 | <img src="images/confirm.svg" width="32" alt="사용 가능한 기능" style="width:32px;" /> | |
| 앱은 수요에 큰 변화가 있으며 빠르게 확장해야 함 | <img src="images/confirm.svg" width="32" alt="사용 가능한 기능" style="width:32px;" /> | |
| 비즈니스 앱(예: CRM, HCM, ERP 및 전자 상거래) | <img src="images/confirm.svg" width="32" alt="사용 가능한 기능" style="width:32px;" /> | |
| 협업 및 소셜 도구(예: 이메일) | <img src="images/confirm.svg" width="32" alt="사용 가능한 기능" style="width:32px;" /> | |
| Linux 및 x86 워크로드 | <img src="images/confirm.svg" width="32" alt="사용 가능한 기능" style="width:32px;" /> | |
| 베어메탈 및 GPU 컴퓨팅 리소스 | <img src="images/confirm.svg" width="32" alt="사용 가능한 기능" style="width:32px;" /> | <img src="images/confirm.svg" width="32" alt="사용 가능한 기능" style="width:32px;" /> |
| PCI 및 HIPAA 준수 워크로드 | <img src="images/confirm.svg" width="32" alt="사용 가능한 기능" style="width:32px;" /> | <img src="images/confirm.svg" width="32" alt="사용 가능한 기능" style="width:32px;" /> |
| 플랫폼 및 인프라 제한조건과 종속성이 포함된 레거시 앱 | | <img src="images/confirm.svg" width="32" alt="사용 가능한 기능" style="width:32px;" /> |
| 엄격한 디자인, 라이센싱 또는 강력한 규제가 적용된 독점 앱 | | <img src="images/confirm.svg" width="32" alt="사용 가능한 기능" style="width:32px;" /> |
| 퍼블릭 클라우드의 앱 스케일링 및 현장의 사설 데이터베이스로 데이터 동기화 | <img src="images/confirm.svg" width="32" alt="사용 가능한 기능" style="width:32px;" />  | <img src="images/confirm.svg" width="32" alt="사용 가능한 기능" style="width:32px;" /> |
{: caption="{{site.data.keyword.Bluemix_notm}} 구현은 워크로드를 지원함" caption-side="top"}

**{{site.data.keyword.containerlong_notm}}에서 워크로드를 오프프레미스로 실행할 준비가 되셨습니까?**</br>
좋습니다! 퍼블릭 클라우드 문서가 이미 제공되었습니다. 좀 더 전략적인 아이디어를 얻기 위해 읽기를 지속하거나 [지금 클러스터를 작성](/docs/containers?topic=containers-getting-started)하여 시작하십시오.

**온프레미스 클라우드에 관심이 있으십니까?**</br>
[{{site.data.keyword.Bluemix_notm}} Private 문서 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.1/kc_welcome_containers.html)를 탐색해보십시오. WebSphere Application Server and Liberty와 같은 IBM 기술에 이미 상당한 투자를 하고 있는 경우 다양한 도구를 사용하여 {{site.data.keyword.Bluemix_notm}} Private 현대화 전략을 최적화할 수 있습니다.

**온프레미스 클라우드와 오프프레미스 클라우드 모두에서 워크로드를 실행하려 하십니까?**</br>
{{site.data.keyword.Bluemix_notm}} Private 계정 설정을 시작하십시오. 그런 다음 [{{site.data.keyword.Bluemix_notm}} Private으로 {{site.data.keyword.containerlong_notm}} 사용](/docs/containers?topic=containers-hybrid_iks_icp)을 참조하여 {{site.data.keyword.Bluemix_notm}} Public에서 클러스터를 사용하여 {{site.data.keyword.Bluemix_notm}} Private 환경에 연결하십시오. {{site.data.keyword.Bluemix_notm}} Public 및 {{site.data.keyword.Bluemix_notm}} Private 등에서 다중 클라우드 Kubernetes 클러스터를 관리하려면 [IBM Multicloud Manager ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.1.0/mcm/getting_started/introduction.html)를 확인하십시오.

### {{site.data.keyword.containerlong_notm}}에서 실행할 수 있는 앱의 유형은 무엇입니까?
{: #app_types}

컨테이너화된 앱은 지원되는 운영 체제인 Ubuntu 16.64, 18.64에서 실행될 수 있어야 합니다. 또한 사용자는 앱의 상태 추적성(statefulness)을 고려하려고 합니다.
{: shortdesc}

<dl>
<dt>Stateless 앱</dt>
  <dd><p>Stateless 앱은 Kubernetes와 같은 클라우드 원시 환경에서 선호됩니다. 종속성을 선언하고, 코드와 별도로 구성을 저장하고, 앱에 결합하는 대신 연결된 리소스로서 데이터베이스와 같은 지원 서비스를 처리하므로 쉽게 마이그레이션하고 스케일링할 수 있습니다. 앱 팟(Pod)에는 지속적 데이터 스토리지 또는 고정 네트워크 IP 주소가 필요하지 않으며, 이에 따라 팟(Pod)은 워크로드 요구사항에 응답하여 종료되고, 다시 스케줄되고, 스케일링될 수 있습니다. 앱은 지속적 데이터에 대해 Database-as-a-Service를 사용하고 NodePort, 로드 밸런서 또는 Ingress 서비스를 사용하여 고정 IP 주소의 워크로드를 노출합니다.</p></dd>
<dt>Stateful 앱</dt>
  <dd><p>팟(Pod)에는 지속적 데이터 및 고정 네트워크 ID가 필요하므로 Stateful 앱은 설정하고, 관리하고, 스케일링하기가 Stateless 앱보다 더욱 복잡합니다. Stateful 앱은 주로 데이터베이스 또는 기타 분산된 데이터 집약적인 워크로드입니다. 여기서, 처리가 효율적일수록 데이터 자체에 더욱 가깝습니다.</p>
  <p>Stateful 앱을 배치하려면 지속적 스토리지를 설정하고 지속적 볼륨을 StatefulSet 오브젝트에서 제어하는 팟(Pod)에 마운트해야 합니다. [파일](/docs/containers?topic=containers-file_storage#file_statefulset), [블록](/docs/containers?topic=containers-block_storage#block_statefulset) 또는 [오브젝트](/docs/containers?topic=containers-object_storage#cos_statefulset) 스토리지를 Stateful 세트를 위한 지속적 스토리지에 추가하도록 선택할 수 있습니다. 또한 [Portworx](/docs/containers?topic=containers-portworx)를 베어메탈 작업자 노드 맨 위에 설치하고 Portworx를 고가용성 소프트웨어로 정의된 스토리지 솔루션으로 사용하여 Stateful 앱에 대한 지속적 스토리지를 관리할 수도 있습니다. Stateful에 대한 자세한 정보는 [Kubernetes 문서 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/)를 참조하십시오.</p></dd>
</dl>

### Stateless인 클라우드 원시 앱을 개발하기 위한 가이드라인은 무엇입니까?
{: #12factor}

[12가지 요소 앱 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://12factor.net/)을 확인하십시오. 12가지 요소 전체에서 앱을 개발하는 방법을 고려하기 위한 언어 불가지론 방법으로, 다음에 간략하게 설명되어 있습니다.
{: shortdesc}

1.  **코드베이스**: 사용자 배치에 맞게 버전 제어 시스템에서 단일 코드베이스를 사용합니다. 컨테이너 배치에 맞게 이미지를 가져오는 경우 `latest`를 사용하는 대신 테스트된 이미지 태그를 지정하십시오.
2.  **종속성**: 외부 종속성을 명시적으로 선언하고 격리합니다.
3.  **구성**: 코드가 아닌 환경 변수에서 배치별 특정 구성을 저장합니다.
4.  **지원 서비스**: 연결되거나 대체 가능한 리소스로서 데이터 저장소 또는 메시지 큐와 같은 지원 서비스를 처리합니다.
5.  **앱 단계**: 엄격하게 분리하여 `build`, `release`, `run`과 같은 명백한 단계로 빌드합니다.
6.  **프로세스**: 데이터를 저장하기 위해 아무 것도 공유하지 않고 [지속적 스토리지](/docs/containers?topic=containers-storage_planning)를 사용하는 하나 이상의 Stateless 프로세스로 실행합니다.
7.  **포트 바인딩**: 포트 바인딩은 내장되어 있으며 명확한 호스트 및 포트에서 서비스 엔드포인트를 제공합니다.
8.  **동시성**: 복제본 및 수평 스케일링과 같은 프로세스 인스턴스를 통해 앱을 관리하고 스케일링합니다. 사용자 배치에 맞게 리소스 요청 및 한계를 설정하십시오. Calico 네트워크 정책은 대역폭의 한계를 설정할 수 없습니다. 대신, [Istio](/docs/containers?topic=containers-istio)를 고려하십시오.
9.  **일회성**: 최소한으로 시작하고, 정상적으로 종료하고, 갑작스러운 프로세스 종료를 허용하여 앱을 일회성으로 설계합니다. 컨테이너, 팟(Pod) 및 작업자 노드도 일회성이 되도록 앱을 적절하게 계획해야 합니다.
10.  **개발/프로덕션 일치**: 개발 환경의 앱과 프로덕션 환경의 앱 간의 차이를 최소화하여 [지속적 통합](https://www.ibm.com/cloud/garage/content/code/practice_continuous_integration/) 및 [지속적 딜리버리](https://www.ibm.com/cloud/garage/content/deliver/practice_continuous_delivery/)를 설정하십시오. 
11.  **로그**: 로그를 이벤트 스트림으로 간주합니다. 외부 또는 호스트 환경은 로그 파일을 처리하고 라우팅합니다. **중요**: {{site.data.keyword.containerlong_notm}}에서 로그는 기본적으로 켜져 있지 않습니다. 사용으로 설정하려면 [로그 전달 구성](/docs/containers?topic=containers-health#configuring)을 참조하십시오.
12.  **관리자 프로세스**: 관리자 스크립트가 앱 자체와 동일한 환경에서 실행되도록 하기 위해 일회성 관리자 스크립트를 앱과 함께 유지하고 이를 [Kubernetes Job 오브젝트 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/workloads/controllers/jobs-run-to-completion/)로서 실행하십시오. Kubernetes 클러스터에서 실행할 대형 패키지에 대한 오케스트레이션의 경우 [Helm ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://helm.sh/)과 같은 패키지 관리자 사용을 고려하십시오.

### 앱이 이미 설치되어 있습니다. {{site.data.keyword.containerlong_notm}}으로 마이그레이션할 수 있는 방법은 무엇입니까?
{: #migrate_containerize}

앱을 컨테이너화하려면 다음과 같이 일반적인 단계를 수행할 수 있습니다.
{: shortdesc}

1.  최대한 종속성을 격리하고, 프로세스를 별도의 서비스로 분산하며, 앱의 상태 추적성(statefulness)을 줄이기 위한 안내서로 [12가지 요소 앱 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://12factor.net/)을 사용하십시오.
2.  사용할 적합한 기본 이미지를 찾으십시오. [Docker Hub ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://hub.docker.com/)에서 공개적으로 사용 가능한 이미지를 사용하고, [IBM 이미지를 공개](/docs/services/Registry?topic=registry-public_images#public_images)하고 또는 개인용 {{site.data.keyword.registryshort_notm}}에서 고유한 이미지를 빌드하고 관리할 수 있습니다.
3.  앱 실행에 필수인 항목만 Docker 이미지에 추가하십시오.
4.  로컬 스토리지에 의존하는 대신 앱의 데이터를 백업하려면 지속적 스토리지 또는 클라우드 Database-as-a-service 솔루션을 사용해보십시오.
5.  시간 경과에 따라 앱 프로세스를 마이크로서비스로 리팩터링하십시오.

자세한 정보는 다음 튜토리얼을 참조하십시오.
*  [Cloud Foundry에서 클러스터로 앱 마이그레이션](/docs/containers?topic=containers-cf_tutorial#cf_tutorial)
*  [VM 기반 앱을 Kubernetes로 이동](/docs/tutorials?topic=solution-tutorials-vm-to-containers-and-kubernetes)



워크로드를 이동할 때 Kubernetes 환경, 고가용성, 서비스 검색 및 배치와 같이 추가 고려사항에 대한 다음 주제로 계속 진행하십시오.

<br />


### 앱을 {{site.data.keyword.containerlong_notm}}로 이동하기 전에 갖추면 좋은 지식 및 기술에는 어떤 것이 있습니까?
{: #knowledge}

Kubernetes는 클러스터 관리자와 앱 개발자라는 두 가지 주요 인물에게 기능을 제공하도록 디자인되었습니다. 각 인물은 서로 다른 기술을 사용하여 클러스터에 앱을 배치하고 실행합니다.
{: shortdesc}

**클러스터 관리자의 주요 태스크 및 기술 지식은 무엇입니까?** </br>
클러스터 관리자는 클러스터의 {{site.data.keyword.Bluemix_notm}} 인프라를 설정하고, 운영하고, 보호하고, 관리해야 합니다. 일반적인 태스크에는 다음 항목이 포함됩니다. 
- 워크로드에 충분한 용량을 제공할 수 있도록 클러스터의 크기를 지정합니다. 
- 회사의 고가용성, 재해 복구 및 규제 준수 표준을 만족시킬 수 있도록 클러스터를 디자인합니다. 
- 사용자 권한을 설정하고 클러스터 내 조치를 제한하여 클러스터를 보호함으로써 컴퓨팅 리소스, 네트워크 및 데이터를 보호합니다. 
- 네트워크 보안, 분할 및 규제 준수를 보장하기 위해 인프라 컴포넌트 간의 네트워크 통신을 계획하고 관리합니다. 
- 데이터 상주 및 데이터 보호 요구사항을 만족시키기 위한 지속적 스토리지 옵션을 계획합니다. 

클러스터 관리자는 컴퓨팅, 네트워크, 스토리지, 보안 및 규제 준수를 포함한 광범위한 지식을 갖고 있어야 합니다. 일반적인 회사에서 이 지식은 시스템 엔지니어, 시스템 관리자, 네트워크 엔지니어, 네트워크 설계자, IT 관리자 또는 보안 및 규제 준수 전문가와 같은 여러 전문가에게 분산되어 있습니다. 클러스터를 운영하는 데 필요한 지식을 갖추기 위해 회사의 여러 사람에게 클러스터 관리자 역할을 지정하는 것을 고려하십시오. 

**앱 개발자의 주요 태스크 및 기술은 무엇입니까?** </br>
개발자는 Kubernetes 클러스터에서 컨테이너화된 클라우드 기반 앱을 디자인하고, 개발하고, 보호하고, 테스트하고, 모니터합니다. 이러한 앱을 작성하고 실행하려면 마이크로서비스, [12가지 요소 앱](#12factor) 가이드라인, [Docker 및 컨테이너화 원리](https://www.docker.com/), 그리고 사용 가능한 [Kubernetes 배치 옵션](/docs/containers?topic=containers-app#plan_apps)의 개념을 잘 알고 있어야 합니다. 서버리스 앱을 배치하려는 경우에는 [Knative](/docs/containers?topic=containers-cs_network_planning)에 대해 학습하십시오. 

Kubernetes 및 {{site.data.keyword.containerlong_notm}}에서는 [앱을 노출하고 개인용으로 유지](/docs/containers?topic=containers-cs_network_planning)하는 방법, 그리고 [지속적 스토리지 추가](/docs/containers?topic=containers-storage_planning), [다른 서비스 통합](/docs/containers?topic=containers-ibm-3rd-party-integrations), [워크로드 및 민감한 데이터 보호](/docs/containers?topic=containers-security#container)를 수행하는 방법에 대해 다양한 옵션을 제공합니다. 앱을 {{site.data.keyword.containerlong_notm}}의 클러스터로 이동하기 전에는 먼저 앱을 지원되는 Ubuntu 16.64, 18.64 운영 체제에서 컨테이너화된 앱으로 실행할 수 있는지, 그리고 Kubernetes 및 {{site.data.keyword.containerlong_notm}}에서 워크로드가 필요로 하는 기능을 제공하는지 확인하십시오. 

**클러스터 관리자와 개발자는 서로 소통합니까?** </br>
예. 클러스터 관리자는 클러스터에서 이 기능을 제공하는 데 필요한 워크로드 요구사항을 파악하고 개발자는 앱 개발 프로세스에서 고려해야 하는 사용 가능한 제한, 통합 및 보안 원리에 대해 알 수 있도록, 클러스터 관리자와 개발자는 빈번히 소통해야 합니다. 

## 워크로드를 지원하기 위해 Kubernetes 클러스터 크기 조정
{: #sizing}

워크로드를 지원하기 위해 클러스터에 필요한 작업자 노드의 수를 파악하는 것은 정확하지 않습니다. 다른 구성 및 조정을 테스트해야 할 수 있습니다. {{site.data.keyword.containerlong_notm}} 사용 시 워크로드 요구에 대한 응답으로 작업자 노드를 추가하고 제거할 수 있다는 이점이 있습니다.
{: shortdesc}

클러스터 크기 조정을 시작하려면 본인에게 다음 질문을 해보십시오.

### 내 앱에 몇 개의 리소스가 필요합니까?
{: #sizing_resources}

먼저, 기존 또는 프로젝트 워크로드 사용량부터 시작하십시오.

1.  워크로드의 평균 CPU 및 메모리 사용량을 계산하십시오. 예를 들어, Windows 시스템에서 태스크 관리자를 볼 수 있거나 Mac 또는 Linux에서 `top` 명령을 실행할 수 있습니다. 또한 메트릭 서비스를 사용하고 보고서를 실행하여 워크로드 사용량을 계산할 수도 있습니다.
2.  워크로드를 처리할 앱 복제본의 수를 결정할 수 있도록 워크로드가 제공해야 하는 요청 수를 예측하십시오. 예를 들어, 분당 1000개의 요청을 처리하도록 앱 인스턴스를 설계하고 워크로드가 분당 10000개의 요청을 제공해야 한다고 예측할 수 있습니다. 해당 경우, 12개의 앱 복제본(예측된 양을 처리하는 10개와 갑작스럽게 증가한 용량을 위한 추가 2개)을 작성하도록 결정할 수 있습니다.

### 내 앱을 제외하고 클러스터에서 리소스를 사용할 수 있는 앱은 무엇입니까?
{: #sizing_other}

이제 사용할 수 있는 몇 가지 다른 기능을 추가하십시오.



1.  앱이 작업자 노드에서 로컬 스토리지의 용량을 차지할 수 있는 대형 또는 다수의 이미지를 가져오는지 여부를 고려하십시오.
2.  클러스터로 [서비스를 통합](/docs/containers?topic=containers-supported_integrations#supported_integrations)할지 여부를 결정하십시오(예: [Helm](/docs/containers?topic=containers-helm#public_helm_install) 또는 [Prometheus ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/coreos/prometheus-operator/tree/master/contrib/kube-prometheus)). 이러한 통합 서비스 및 추가 기능은 클러스터 리소스를 이용하는 팟(Pod)을 회전합니다.

### 내 워크로드에 필요한 가용성의 유형은 무엇입니까?
{: #sizing_availability}

사용자는 최대한 많은 워크로드가 작동되기를 원한다는 사실을 기억하십시오!

1.  [고가용성 클러스터](/docs/containers?topic=containers-ha_clusters#ha_clusters)에 대한 전략을 계획하십시오(예: 단일 또는 다중 클러스터 중에서 결정).
2.  사용할 수 있는 앱의 수를 결정하는 데 도움이 되도록 [고가용성 배치](/docs/containers?topic=containers-app#highly_available_apps)를 검토하십시오.

### 내 워크로드를 처리해야 하는 작업자 노드의 수는 몇 개입니까?
{: #sizing_workers}

이제 워크로드가 어떻게 표시되는지 잘 알게 되었습니다. 예상 사용량을 사용 가능한 클러스터 구성에 맵핑하십시오.

1.  보유하고 있는 클러스터의 유형에 따라 최대 작업자 노드 용량을 예측하십시오. 갑작스러운 증가 또는 기타 임시 이벤트가 발생하는 경우 작업자 노드 용량 한계를 초과하지 않으려고 합니다.
    *  **단일 구역 클러스터**: 클러스터에 최소 세 개의 작업자 노드를 보유하도록 계획하십시오. 또한 사용자는 클러스터에 사용 가능한 CPU 및 메모리 용량 중에 한 개의 추가 노드분을 필요로 합니다.
    *  **다중 구역 클러스터**: 구역당 최소 두 개의 작업자 노드를 보유하도록 계획하십시오. 따라서 총 세 개의 구역에 여섯 개의 노드를 보유하게 됩니다. 또한 총 워크로드의 필요한 용량 중 최소 150%가 되도록 클러스터의 총 용량을 계획하십시오. 이에 따라 한 개의 구역이 작동 중지되면 워크로드를 유지보수하는 데 사용할 수 있는 리소스를 보유하게 됩니다.
2.  [사용 가능한 작업자 노드 특성](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes) 중 하나에 맞추어 앱 크기 및 작업자 노드 용량을 조정하십시오. 구역에서 사용 가능한 특성을 보려면 `ibmcloud ks machine-types <zone>`를 실행하십시오.
    *   **작업자 노드가 과부하되지 않아야 함**: 팟(Pod)이 CPU에 대해 경쟁하거나 비효율적으로 실행되지 않도록 하려면 필요한 작업자 노드의 수를 계획할 수 있도록 앱에 필요한 리소스를 알고 있어야 합니다. 예를 들어, 앱이 작업자 노드에서 사용 가능한 리소스보다 더 적은 리소스가 필요한 경우 하나의 작업자 노드에 배치하는 팟(Pod)의 수를 제한할 수 있습니다. 스케줄되어야 할 수 있는 기타 팟(Pod)의 공간을 남겨두기 위해 작업자 노드의 약 75% 용량을 유지하십시오. 앱이 작업자 노드에서 사용할 수 있는 리소스보다 더 많은 리소스를 필요로 하는 경우 이 요구사항을 충족시킬 수 있는 다른 작업자 노드 특성을 사용하십시오. 작업자 노드가 자주 `NotReady` 또는 제거 팟(Pod)의 상태를 다시 보고할 때 메모리 또는 기타 리소스가 부족하여 작업자 노드가 과부화됨을 알고 있습니다.
    *   **대형 대 소형 작업자 노드 특성**: 대형 노드는 소형 노드보다 비용 효율적일 수 있습니다. 특히 고성능 머신에서 처리될 때 효율성을 높이도록 설계된 워크로드가 이에 해당됩니다. 그러나 대형 작업 노드가 작동 중지된 경우 클러스터는 클러스터의 기타 작업자 노드에 대한 모든 워크로드 팟(Pod)을 단계적으로 다시 스케줄할 수 있도록 충분한 용량이 있는지 확인해야 합니다. 소형 작업자는 사용자가 좀 더 단계적으로 스케일링하도록 지원할 수 있습니다.
    *   **앱의 복제본**: 원하는 작업자 노드의 수를 판별하기 위해 실행할 앱의 복제본 수를 고려할 수도 있습니다. 예를 들어, 워크로드에 32개의 CPU 코어가 필요함을 알게 되고 앱의 16개의 복제본을 실행할 계획인 경우 각 복제본 팟(Pod)에는 2개의 CPU 코어가 필요합니다. 작업자 노드당 한 개의 앱 팟(Pod)만 실행할 경우 이 구성을 지원하도록 클러스터 유형에 대한 적절한 수의 작업자 노드를 주문할 수 있습니다.
3.  대표적인 대기 시간, 확장성, 데이터 세트 및 워크로드 요구사항과 함께 클러스터에 필요한 작업자 노드의 수를 계속해서 세분화하도록 성능 테스트를 실행하십시오.
4.  리소스 요청에 대한 응답으로 스케일링 업 및 다운해야 하는 워크로드의 경우 [수평 팟(Pod) 오토스케일러](/docs/containers?topic=containers-app#app_scaling) 및 [클러스터 작업자 풀 오토스케일러](/docs/containers?topic=containers-ca#ca)를 설정하십시오.

<br />


## Kubernetes 환경 구조화
{: #kube_env}

{{site.data.keyword.containerlong_notm}}가 하나의 IBM Cloud 인프라(SoftLayer) 포트폴리오에 연결됩니다. 계정 내에서 마스터 및 다양한 작업자 노드로 구성된 클러스터를 작성할 수 있습니다. IBM가 마스터를 관리하므로, 사용자는 동일한 특성의 개별 머신 또는 메모리 및 CPU 스펙을 함께 풀링하는 혼합된 작업자 노드를 작성할 수 있습니다. 클러스터 내에서 네임스페이스 및 레이블을 사용하여 리소스를 추가로 구성할 수 있습니다. 사용자의 팀과 워크로드가 필요한 리소스를 가져올 수 있도록 올바르게 혼합된 클러스터, 머신 유형 및 조직 전략을 선택하십시오.
{:shortdesc}

### 어떤 유형의 클러스터 및 머신 유형을 가져와야 합니까?
{: #env_flavors}

**클러스터 유형**: [단일 구역, 다중 구역 또는 다중 클러스터 설정](/docs/containers?topic=containers-ha_clusters#ha_clusters)을 사용할지 여부를 결정합니다. 다중 구역 클러스터가 [여섯 개의 모든 전 세계 {{site.data.keyword.Bluemix_notm}} 도시 지역](/docs/containers?topic=containers-regions-and-zones#zones)에서 사용 가능합니다. 또한 작업자 노드는 구역에 따라 달라지는 점에 유의하십시오.

**작업자 노드 유형**: 일반적으로 집약적인 워크로드는 베어메탈 실제 머신에서 실행되는 데 더욱 적합합니다. 반면에 비용 효율적인 테스트 및 개발 작업의 경우 공유 또는 전용 공유 하드웨어에서 가상 머신을 선택할 수 있습니다. 베어메탈 작업자 노드를 포함하여 클러스터에는 더 높은 처리량을 제공하는 10Gbps의 네트워크 속도와 하이퍼 스레드 코어가 있습니다. 가상 머신에는 1Gbps의 네트워크 속도와 하이퍼 스레드를 제공하지 않는 일반 코어가 함께 제공됩니다. [사용 가능한 머신 격리 및 특성을 확인](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)하십시오.

### 다중 클러스터를 사용합니까? 아니면 더 많은 작업자를 기존 클러스터에 추가만 합니까?
{: #env_multicluster}

작성하는 클러스터 수는 워크로드, 회사 정책 및 규제와 컴퓨팅 리소스로 작업할 항목에 따라 달라집니다. [컨테이너 격리 및 보안](/docs/containers?topic=containers-security#container)에서 이 의사결정에 대한 보안 정보를 검토할 수도 있습니다.

**다중 클러스터**: [글로벌 로드 밸런서](/docs/containers?topic=containers-ha_clusters#multiple_clusters)를 설정하고 복사한 후 동일한 구성 YAML 파일을 각각 적용하여 클러스터 간에 워크로드의 밸런스를 유지해야 합니다. 그러므로 다중 클러스터는 관리하기에 데 좀 더 복잡하지만 다음과 같은 중요 목표를 달성하는 데 도움이 될 수 있습니다.
*  워크로드를 격리하는 데 필요한 보안 정책을 준수합니다.
*  여러 버전의 Kubernetes 또는 기타 클러스터 소프트웨어(예: Calico)에서 앱을 실행하는 방법을 테스트합니다.
*  해당 지리적 영역에서 사용자의 성능을 향상시키기 위해  다른 지역에서 앱으로 클러스터를 작성합니다.
*  네임스페이스 레벨에서 클러스터 내의 액세스를 제어하기 위해 여러 RBAC 정책을 사용자 정의하고 관리하는 대신 클러스터 인스턴스 레벨의 사용자 액세스를 구성합니다.

**적은 수의 클러스터 또는 단일 클러스터**: 적은 수의 클러스터는 고정된 리소스에 대한 운영상의 노력과 클러스터당 비용을 줄이는 데 도움이 됩니다. 더 많은 클러스터를 작성하는 대신 사용할 앱 및 서비스 컴포넌트에 사용 가능한 다른 머신 유형의 컴퓨팅 리소스에 대해 작업자 풀을 추가할 수 있습니다. 앱 개발 시 앱을 사용하는 리소스는 동일한 구역에 있습니다. 동일한 구역에 없으면 다중 구역에 밀접하게 연결되어서 대기 시간, 대역폭 또는 상관된 실패를 가정할 수 있습니다. 그러나 네임스페이스, 리소스 할당량 및 레이블을 사용하여 클러스터를 구성하는 것이 더욱 중요합니다.

### 클러스터 내에서 내 리소스를 어떻게 설정할 수 있습니까?
{: #env_resources}

<dl>
<dt>작업자 노드 용량을 고려하십시오.</dt>
  <dd>작업자 노드의 성능을 최대한 활용하려면 다음을 고려하십시오.
  <ul><li><strong>코어 강도 유지</strong>: 각 머신에는 특정 수의 코어가 있습니다. 앱의 워크로드에 따라 코어당 팟(Pod)의 수에 대한 한계를 설정하십시오(예: 10).</li>
  <li><strong>노드 과부하 방지</strong>: 마찬가지로, 노드에 100가 넘는 팟(Pod)을 포함할 수 있다고 해서 사용자가 이를 원하는 것은 아닙니다. 앱의 워크로드에 따라 노드당 팟(Pod)의 수에 대한 한계를 설정하십시오(예: 40).</li>
  <li><strong>클러스터 대역폭을 누르지 않음</strong>: 스케일링 가상 머신의 네트워크 대역폭이 약 1000Mbps라는 점에 유의하십시오. 클러스터에 수백 개의 작업자 노드가 필요하면 더 적은 노드 또는 주문자 베어메탈 노드가 포함된 여러 클러스터로 분할하십시오.</li>
  <li><strong>서비스 정렬</strong>: 배치하기 전에 워크로드에 필요한 서비스의 수를 계획하십시오. 네트워킹 및 포트 전달 규칙은 Iptables에 배치됩니다. 5,000개의 서비스와 같이 많은 수의 서비스를 클러스터를 여러 클러스터로 분할하십시오.</li></ul></dd>
<dt>혼합된 컴퓨팅 리소스에 적합한 다양한 유형의 머신을 프로비저닝하십시오.</dt>
  <dd>모든 사람들은 선택하는 것을 좋아합니다. {{site.data.keyword.containerlong_notm}}에서는 배치할 수 있는 [혼합된 머신 유형](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)이 있으며, 집약적인 워크로드에 적합한 베어메탈부터 빠른 스케일링에 적합한 가상 머신까지 해당됩니다. 사용자의 머신에 배치를 구성하려면 레이블 또는 네임스페이스를 사용하십시오. 배치를 작성할 때 올바르게 리소스를 혼합하여 앱의 팟(Pod)만 머신에 배치되도록 배치를 제한하십시오. 예를 들어, 데이터베이스 애플리케이션을 `md1c.28x512.4x4tb`와 같이 상당한 양의 로컬 디스크 스토리지가 있는 베어메탈 머신으로 제한하려고 할 수 있습니다.</dd>
<dt>클러스터를 공유하는 여러 팀과 프로젝트가 있을 때 다중 네임스페이스를 설정하십시오.</dt>
  <dd><p>네임스페이스는 클러스터 내의 클러스터와 유사합니다. [리소스 할당량 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/policy/resource-quotas/) 및 [기본 한계 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/tasks/administer-cluster/memory-default-namespace/)를 사용하여 클러스터 리소스를 분할하는 방법입니다. 새 네임스페이스 작성 시 적절한 [RBAC 정책](/docs/containers?topic=containers-users#rbac)을 설정하여 액세스를 제어하십시오. 자세한 정보는 Kubernetes 문서에 있는 [네임스페이스와 클러스터 공유 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/tasks/administer-cluster/namespaces/)를 참조하십시오.</p>
  <p>사용자가 12명인 소형 클러스터와 유사한 리소스(예: 동일한 소프트웨어의 다른 버전)가 있는 경우 다중 네임스페이스가 필요하지 않을 수 있습니다. 대신 레이블을 사용할 수 있습니다.</p></dd>
<dt>클러스터의 사용자가 리소스 요청 및 한계를 사용하도록 리소스 할당량 설정</dt>
  <dd>모든 팀이 클러스터에서 서비스 배치와 앱 실행에 필요한 리소스를 보유하도록 보장하려면, 모든 네임스페이스에 대해 [리소스 할당량](https://kubernetes.io/docs/concepts/policy/resource-quotas/)을 설정해야 합니다. 리소스 할당량은 네임스페이스에 대한 배치 제한조건(예: 배치 가능한 Kubernetes 리소스의 수와 해당 리소스가 이용할 수 있는 CPU 및 메모리의 양)을 판별합니다. 할당량을 설정한 후에 사용자는 자체 배치의 리소스 요청 및 한계를 포함해야 합니다.</dd>
<dt>레이블이 있는 Kubernetes 오브젝트 구성</dt>
  <dd><p>`pods` 또는 `nodes`와 같은 Kubernetes 리소스를 구성하고 선택하려면 [Kubernetes 레이블 사용 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/)을 참조하십시오. 기본적으로 {{site.data.keyword.containerlong_notm}}는 `arch`, `os`, `region`, `zone` 및 `machine-type`을 포함한 몇 가지 레이블을 적용합니다.</p>
  <p>레이블에 대한 유스 케이스 예에는 베어메탈 작업자 노드와 같은 특정 머신 유형 또는 SDS 기능을 충족하는 작업자 노드에서 실행될 [에지 작업자 노드로 네트워크 트래픽 제한](/docs/containers?topic=containers-edge), [GPU 머신에 앱 배치](/docs/containers?topic=containers-app#gpu_app) 및 [앱 워크로드 제한![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/)이 포함됩니다. 리소스에 이미 적용된 레이블을 보려면 <code>--show-labels</code> 플래그를 포함한 <code>kubectl get</code> 명령을 사용하십시오. 예를 들어, 다음과 같습니다.</p>
  <p><pre class="pre"><code>kubectl get node &lt;node_ID&gt; --show-labels</code></pre></p>
  작업자 노드에 레이블을 적용하려면 레이블이 있는 [작업자 풀을 작성](/docs/containers?topic=containers-add_workers#add_pool)하거나 [기존 작업자 풀을 업데이트](/docs/containers?topic=containers-add_workers#worker_pool_labels)하십시오. </dd>
</dl>




<br />


## 리소스 고가용성 제공
{: #kube_ha}

시스템이 전체적으로 안전하지 않지만 {{site.data.keyword.containerlong_notm}}에서 앱 및 서비스의 고가용성을 늘리기 위한 단계를 수행할 수 있습니다.
{:shortdesc}

리소스 고가용성 제공에 대한 자세한 정보를 검토하십시오.
* [잠재적 실패 지점 감소](/docs/containers?topic=containers-ha#ha)
* [다중 클러스터 작성](/docs/containers?topic=containers-ha_clusters#ha_clusters)
* [고가용성 배치 계획](/docs/containers?topic=containers-app#highly_available_apps). 다중 구역에서 복제본 세트 및 팟(Pod) 반친화성과 같은 기능을 사용합니다.
* [클라우드 기반 공용 레지스트리의 이미지를 기반으로 하는 컨테이너 실행](/docs/containers?topic=containers-images)
* [데이터 스토리지 계획](/docs/containers?topic=containers-storage_planning#persistent_storage_overview). 특히 다중 구역 클러스터의 경우 [{{site.data.keyword.cloudant_short_notm}}](/docs/services/Cloudant?topic=cloudant-getting-started#getting-started) 또는 [{{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-about)와 같은 클라우드 서비스의 사용을 고려하십시오.
* 다중 구역 클러스터의 경우 [로드 밸런서 서비스](/docs/containers?topic=containers-loadbalancer#multi_zone_config) 또는 Ingress [다중 구역 로드 밸런서](/docs/containers?topic=containers-ingress#ingress)를 사용으로 설정하여 앱을 공개적으로 노출하십시오.

<br />


## 서비스 검색 설정
{: #service_discovery}

Kubernetes 클러스터의 각 팟(Pod)에는 IP 주소가 있습니다. 그러나 앱을 클러스터에 배치하는 경우 사용자는 서비스 검색 및 네트워킹에 대한 팟(Pod) IP 주소에 의존하려고 하지 않습니다. 팟(Pod)은 동적으로 자주 제거되고 대체됩니다. 대신, 팟(Pod)의 그룹을 표시하고 `cluster IP`라고 하는 서비스의 가상 IP 주소를 통해 고정 시작점을 제공하는 Kubernetes 서비스를 사용하십시오. 자세한 정보는 [서비스 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/services-networking/service/#discovering-services)에 대한 Kubernetes 문서를 참조하십시오.
{:shortdesc}

### Kubernetes 클러스터 DNS 제공자를 사용자 정의할 수 있습니까?
{: #services_dns}

서비스 및 팟(Pod)을 제공할 때 앱 컨테이너가 DNS 이름 분석을 위해 DNS 서비스 IP를 사용할 수 있도록 DNS 이름이 지정됩니다. 이름 서버, 검색 및 오브젝트 목록 옵션을 지정하도록 팟(Pod) DNS를 사용자 정의할 수 있습니다. 자세한 정보는 [클러스터 DNS 제공자 구성](/docs/containers?topic=containers-cluster_dns#cluster_dns)을 참조하십시오.
{: shortdesc}



### 내 서비스가 올바른 배치에 연결되고 이동할 준비가 되었는지 확인할 수 있는 방법은 무엇입니까?
{: #services_connected}

대부분의 서비스의 경우 해당 레이블로 앱을 실행하는 팟(Pod)에 적용되도록 선택기를 서비스 `.yaml` 파일에 추가하십시오. 앱을 처음 시작하는 다수의 경우 요청을 즉시 처리하지 않으려고 합니다. 트래픽이 준비 상태로 간주되는 팟(Pod)으로만 전송되도록 준비 상태 프로브를 배치에 추가하십시오. 레이블을 사용하고 준비 상태 프로브를 설정하는 서비스의 배치에 대한 예는 이 [NGINX YAML](https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/deploy-apps-clusters/nginx_preferredAntiAffinity.yaml)을 확인하십시오.
{: shortdesc}

서비스가 레이블을 사용하지 않기를 원하는 경우도 있습니다. 예를 들어, 외부 데이터베이스가 있거나 서비스를 클러스터 내 다른 네임스페이스의 또 다른 서비스로 지정하려고 할 수 있습니다. 해당 경우 엔드포인트 오브젝트를 수동으로 추가하고 이를 서비스에 연결해야 합니다.


### 내 클러스터에서 실행되는 서비스 중에서 네트워크 트래픽을 어떻게 제어합니까?
{: #services_network_traffic}

기본적으로 팟(Pod)은 클러스터의 다른 팟(Pod)과 통신할 수 있지만 특정 팟(Pod) 또는 네임스페이스에 대한 트래픽(네트워크 정책 사용)을 차단할 수 있습니다. 또한 NodePort, 로드 밸런서 또는 Ingress 서비스를 사용하여 외부적으로 앱을 노출하는 경우 트래픽을 차단하기 위해 고급 네트워크 정책을 설정하려고 할 수 있습니다. {{site.data.keyword.containerlong_notm}}에서 Kubernetes를 관리하기 위해 Calico를 사용하고 Calico [네트워크 정책을 사용하여 트래픽을 제어](/docs/containers?topic=containers-network_policies#network_policies)할 수 있습니다.

네트워크 트래픽을 연결하고, 관리하고, 보안 설정하는 데 필요한 플랫폼에서 실행되는 다양한 마이크로서비스가 있는 경우 [관리 Istio 추가 기능](/docs/containers?topic=containers-istio)과 같은 서비스 메시 도구 사용을 고려하십시오.

작업자 노드를 선택하도록 네트워킹 워크로드를 제한하여 클러스터의 보안 및 격리를 증가시키려면 [에지 노드를 설정](/docs/containers?topic=containers-edge#edge)할 수도 있습니다.



### 인터넷에 내 서비스를 노출할 수 있는 방법은 무엇입니까?
{: #services_expose_apps}

외부 네트워킹을 위한 세 가지 유형의 서비스 즉, NodePort, LoadBalancer 및 Ingress를 작성할 수 있습니다. 자세한 정보는 [네트워킹 서비스 계획](/docs/containers?topic=containers-cs_network_planning#external)을 참조하십시오.

클러스터에 필요한 `Service` 오브젝트의 수를 계획할 때 Kubernetes가 네트워킹 및 포트 전달 규칙을 처리하는 데 `iptables`를 사용하는 점에 유의하십시오. 클러스터에서 많은 수의 서비스(예: 5000개)를 실행하는 경우 성능에 영향을 줄 수 있습니다.



## 클러스터에 앱 워크로드 배치
{: #deployments}

팟(Pod), 배치 및 작업과 같은 Kubernetes를 사용하여 YAML 구성 파일로 여러 유형의 오브젝트를 선언합니다. 이 오브젝트에서는 실행 중인 컨테이너화된 앱, 사용하는 리소스 및 다시 시작, 업데이트, 복제 등을 위해 동작을 관리하는 정책과 같은 항목에 대해 설명합니다. 자세한 정보는 [우수 사례 구성 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/configuration/overview/)에 대한 Kubernetes 문서를 참조하십시오.
{: shortdesc}

### 내 앱을 컨테이너에 배치해야 한다고 생각했습니다. 팟(Pod)에 대한 이 항목은 무엇입니까?
{: #deploy_pods}

[팟(Pod) ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/workloads/pods/pod/)은 Kubernetes가 관리할 수 있는 배치 가능한 최소 단위입니다. 컨테이너(또는 컨테이너의 그룹)을 팟(Pod)에 배치하고 팟(Pod) 구성 파일을 사용하여 컨테이너를 실행하고 리소스를 다른 팟(Pod)과 공유하는 방법을 알려줍니다. 팟(Pod)에 배치되는 모든 컨테이너는 공유 컨텍스트에서 실행되며, 이는 동일한 가상 또는 실제 머신을 공유함을 의미합니다.
{: shortdesc}

**컨테이너에 배치하는 항목**: 애플리케이션의 컴포넌트에서와 같이, CPU 및 메모리와 같은 항목에 대한 크게 다른 리소스 요구사항이 있는지 여부를 고려하십시오. 일부 컴포넌트가 최대 성능으로 실행될 수 있으며, 리소스를 다른 영역으로 전환함에 따라 발생하는 잠깐 동안의 성능 저하가 허용됩니까? 다른 컴포넌트가 고객 지향적이므로 성능을 유지하는 것이 중요합니까? 별도의 컨테이너로 분할하십시오. 동기화 상태로 함께 실행되도록 항상 동일한 팟(Pod)에 배치할 수 있습니다.

**팟(Pod)에 배치하는 항목**: 앱이 항상 동일한 팟(Pod)에 있지 않아도 되는 컨테이너입니다. 실제로, 데이터베이스 서비스와 같이 Stateful이고 스케일링하는 데 어려움이 있는 컴포넌트가 있는 경우 워크로드를 처리하도록 추가 리소스가 포함된 작업자 노드에서 스케줄할 수 있는 다른 팟(Pod)에 배치하십시오. 컨테이너가 다른 작업자 노드에서 실행되면 올바르게 작동하는 경우에는 여러 팟(Pod)을 사용하십시오. 동일한 머신에 있어야 하고 함께 스케일링되어야 하는 경우 컨테이너를 동일한 팟(Pod)으로 그룹화하십시오.

### 팟(Pod)만 사용할 수 있는 경우 다양한 유형의 오브젝트가 필요한 이유는 무엇입니까?
{: #deploy_objects}

팟(Pod) YAML 파일을 작성하는 것은 쉽습니다. 다음과 같이 몇 개의 행으로만 팟(Pod) YAML 파일을 작성할 수 있습니다.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx
    ports:
    - containerPort: 80
```
{: codeblock}

그러나 사용자는 해당 지점에서의 중단을 원하지 않습니다. 팟(Pod)이 실행되는 노드가 작동 중지된 경우 팟(Pod)은 노드와 함께 작동 중지되고 다시 스케줄되지 않습니다. 대신, [배치 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)를 사용하여 팟(Pod) 다시 스케줄링, 복제본 세트 및 롤링 업데이트를 지원하십시오. 기본 배치는 팟(Pod)만큼 쉽게 작성할 수 있습니다. 그러나 자체적으로 `spec`에서 컨테이너를 정의하는 대신 배치 `spec`에서 `replicas` 및 `template`를 지정합니다. 템플리트에는 다음과 같이 배치 내의 클러스터에 적합한 고유 `spec`이 있습니다.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 80
```
{: codeblock}

동일한 모든 YAML 파일에서 팟(Pod) 반친화성 또는 리소스 한계와 같은 기능 추가를 계속할 수 있습니다.

배치에 추가할 수 있는 여러 기능에 대한 자세한 설명은 [앱 배치 YAML 파일 작성](/docs/containers?topic=containers-app#app_yaml)을 참조하십시오.
{: tip}

### 내 배치를 좀 더 쉽게 업데이트하고 관리할 수 있도록 구성하는 방법은 무엇입니까?
{: #deploy_organize}

이제 사용자는 배치에 포함될 항목에 대해 잘 알게 되었으며, 다양한 YAML 파일을 어떻게 관리할 것인지 궁금할 수 있습니다. Kubernetes 환경에서 작성하는 오브젝트 또한 해당됩니다!

배치 YAML 파일을 구성하기 위한 몇 가지 팁은 다음과 같습니다.
*  Git과 같은 버전 제어 시스템을 사용하십시오.
*  단일 YAML 파일 내에서 밀접하게 관련된 Kubernetes 오브젝트를 그룹화하십시오. 예를 들어, `deployment`를 작성하는 경우 `service` 파일을 YAML에 추가할 수도 있습니다. 다음과 같이 `---`를 사용하여 오브젝트를 구분하십시오.
    ```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
    ...
    ---
apiVersion: v1
    kind: Service
    metadata:
    ...
    ```
    {: codeblock}
*  단일 파일만이 아닌 `kubectl apply -f` 명령을 사용하여 전체 디렉토리를 적용할 수 있습니다.
*  Kubernetes 리소스 YAML 구성을 작성하고, 사용자 정의하고, 재사용하는 데 도움을 받기 위해 사용할 수 있는 [`kustomize` 프로젝트](/docs/containers?topic=containers-app#kustomize)를 사용해 보십시오. 

YAML 파일 내에서 배치를 관리하는 메타데이터로서 레이블 또는 어노테이션을 사용할 수 있습니다.

**레이블**: [레이블 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/)은 팟(Pod) 및 배치와 같은 Kubernetes 오브젝트에 연결될 수 있는 `key:value` 쌍입니다. 사용자가 원하는 대로 지정할 수 있으며 레이블 정보를 기반으로 하여 오브젝트를 선택하는 데 유용합니다. 레이블은 오브젝트를 그룹화하기 위한 기반을 제공합니다. 레이블에 대한 예는 다음과 같습니다.
* `app: nginx`
* `version: v1`
* `env: dev`

**어노테이션**: [어노테이션 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/overview/working-with-objects/annotations/)은 `key:value` 쌍이기도 하므로 레이블과 유사합니다. 오브젝트 생성 위치, 오브젝트 사용 방법, 연관된 추적 저장소에 대한 포인터 또는 오브젝트에 대한 정책과 관련된 추가 정보 보유와 같이 도구 또는 라이브러리로 활용될 수 있는 비식별 정보에 더욱 적합합니다. 어노테이션을 기반으로 한 오브젝트를 선택하지 않습니다.

### 배치를 위해 앱을 준비하려면 무엇을 더 수행할 수 있습니까?
{: #deploy_prep}

많은 것들이 있습니다! [클러스터에 실행되도록 컨테이너화된 앱 준비](/docs/containers?topic=containers-app#plan_apps)를 참조하십시오. 주제에는 다음 항목에 대한 정보가 포함되어 있습니다.
*  Kubernetes에서 실행할 수 있는 앱의 유형(Stateful 및 Stateless 앱에 대한 팁 포함)
*  Kubernetes에 앱 마이그레이션
*  워크로드 요구사항을 기반으로 한 클러스터 크기 지정
*  IBM 서비스, 스토리지, 로깅 및 모니터링과 같은 추가 앱 리소스 설정
*  배치에 변수 사용
*  앱에 대한 액세스 제어

<br />


## 애플리케이션 패키지
{: #packaging}

다중 클러스터, 공용 및 사설 환경 또는 클라우드 제공자에서도 앱을 실행할 경우 이 환경에서 배치 전략 작업을 수행할 수 있는 방법을 궁금해할 수 있습니다. {{site.data.keyword.Bluemix_notm}} 및 기타 오픈 소스 도구를 사용하여 애플리케이션 자동화를 지원하도록 애플리케이션을 패키지할 수 있습니다.
{: shortdesc}

<dl>
<dt>인프라 자동화</dt>
  <dd>오픈 소스 [Terraform](/docs/terraform?topic=terraform-getting-started#getting-started) 도구를 사용하여 Kubernetes 클러스터를 포함한 {{site.data.keyword.Bluemix_notm}} 인프라의 프로비저닝을 자동화할 수 있습니다. [배치 환경 계획, 작성 및 업데이트](/docs/tutorials?topic=solution-tutorials-plan-create-update-deployments#plan-create-update-deployments)를 수행하려면 이 튜토리얼을 따라 진행하십시오. 워크로드의 리소스 요청에 대한 응답으로 작업자 풀이 작업자 노드를 스케일링 업 및 다운하도록 클러스터 작성 후 [{{site.data.keyword.containerlong_notm}} 클러스터 오토스케일러](/docs/containers?topic=containers-ca)도 설정할 수 있습니다.</dd>
<dt>지속적 통합 및 지속적 딜리버리(CI/CD) 파이프라인 설정</dt>
  <dd>Git과 같은 소스 제어 관리 시스템에서 구성된 앱 구성 파일을 사용하여 여러 환경에 `test` 및 `prod`와 같은 코드를 테스트하고 배치하도록 파이프라인을 빌드할 수 있습니다. [Kubernetes에 대한 지속적인 배치에 대한 이 튜토리얼](/docs/tutorials?topic=solution-tutorials-continuous-deployment-to-kubernetes#continuous-deployment-to-kubernetes)을 확인하십시오.</dd>
<dt>앱 구성 파일 패키지</dt>
  <dd>[Helm ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://helm.sh/docs/) Kubernetes 패키지 관리자를 사용하여 Helm 차트에서 앱에 필요한 모든 Kubernetes 리소스를 지정할 수 있습니다. 그런 다음 클러스터에서 Helm을 사용하여 YAML 구성 파일을 작성하고 이 파일을 배치할 수 있습니다. [{{site.data.keyword.Bluemix_notm}} 제공 Helm 차트를 통합 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/kubernetes/solutions/helm-charts)하여 블록 스토리지 플러그인과 같은 클러스터의 기능을 확장할 수도 있습니다.<p class="tip">YAML 파일 템플리트를 작성하기 쉬운 방법만 찾고 있습니까? 일부 사용자는 Helm을 사용하여 이를 수행할 수 있습니다. 아니면 [`ytt` ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://get-ytt.io/)와 같은 기타 커뮤니티 도구를 사용해볼 수 있습니다.</p></dd>
</dl>

<br />


## 앱을 최신 상태로 유지
{: #updating}

사용자는 앱의 다음 버전을 준비하기 위한 많은 노력을 하고 있습니다. {{site.data.keyword.Bluemix_notm}} 및 Kubernetes 업데이트 도구를 사용하여 앱이 보안 설정된 클러스터 환경에서 실행 중이고 다른 버전의 앱을 롤아웃하는지 확인할 수 있습니다.
{: shortdesc}

### 내 클러스터를 지원되는 상태로 유지하는 방법은 무엇입니까?
{: #updating_kube}

클러스터가 항상 [지원되는 Kubernetes 버전](/docs/containers?topic=containers-cs_versions#cs_versions)을 실행하는지 확인하십시오. 새 Kubernetes 부 버전이 릴리스되면 이전 버전은 지원되지 않는 즉시 더 이상 사용되지 않습니다. 자세한 정보는 [Kubernetes 마스터 업데이트](/docs/containers?topic=containers-update#master) 및 [작업자 노드](/docs/containers?topic=containers-update#worker_node)를 참조하십시오.

### 사용할 수 있는 앱 업데이트 전략은 무엇입니까?
{: #updating_apps}

앱을 업데이트하려면 다음과 같은 다양한 전략 중에서 선택할 수 있습니다. 더욱 복잡한 Canary 배치를 진행하기 전에 롤링 배치 또는 즉각적인 전환을 시작할 수 있습니다.

<dl>
<dt>롤링 배치</dt>
  <dd>Kubernetes 원시 기능을 사용하여 `v2` 배치를 작성하고 점진적으로 이전 `v1` 배치를 대체할 수 있습니다. 이 접근 방식을 사용하려면 `v2` 앱 버전을 제공 받는 사용자가 중요한 변경사항을 경험하지 않도록 앱이 이전 버전과 호환되어야 합니다. 자세한 정보는 [앱 업데이트를 위한 롤링 배치 관리](/docs/containers?topic=containers-app#app_rolling)를 참조하십시오.</dd>
<dt>즉각적인 전환</dt>
  <dd>즉각적인 전환은 Blue-Green 배치라고도 하며, 두 개의 버전이 동시에 실행되도록 두 배 크기의 컴퓨팅 리소스가 필요합니다. 이 접근 방식을 사용하면 거의 실시간으로 사용자를 최신 버전으로 전환할 수 있습니다. 요청이 올바른 앱 버전으로 전송되도록 사용자가 서비스 레이블 선택기(예: `version: green` 및 `version: blue`)를 사용하는지 확인하십시오. 새 `version: green` 배치를 작성할 수 있습니다. 준비가 될 때까지 기다린 후 `version: blue` 배치를 삭제하십시오. 또는 [롤링 업데이트](/docs/containers?topic=containers-app#app_rolling)를 수행할 수 있으나 `maxUnavailable` 매개변수를 `0%`로 설정하고 `maxSurge` 매개변수를 `100%`로 설정하십시오.</dd>
<dt>Canary 또는 A/B 배치</dt>
  <dd>더욱 복잡한 업데이트 전략인 Canary 배치는 5%와 같은 사용자의 백분율을 선택하여 새 앱 버전으로 전송하는 경우에 수행됩니다. 새 앱 버전에서 수행되는 방식을 알아보기 위해 로깅 및 모니터링 도구에서 메트릭을 수집하고, A/B 테스트를 수행한 후 더 많은 사용자에게 업데이트를 롤아웃합니다. 모든 배치에서와 같이 앱(예: `version: stable` 및 `version: canary`)에 레이블을 지정하는 것이 중요합니다. Canary 배치를 관리하려면 [이 블로그 게시물 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://sysdig.com/blog/monitor-istio/)에서 설명된 대로 [관리 추가 기능 서비스 메시 설치](/docs/containers?topic=containers-istio#istio), [클러스터에 대한 Sysdig 모니터링 설정](/docs/services/Monitoring-with-Sysdig/tutorials?topic=Sysdig-kubernetes_cluster#kubernetes_cluster)을 수행한 후 A/B 테스트를 위해 Istio 서비스 메시를 사용할 수 있습니다. 또는, Canary 배치에 Knative를 사용하십시오. </dd>
</dl>

<br />


## 클러스터 성능 모니터링
{: #monitoring_health}

클러스터 및 앱의 효율적인 로깅과 모니터링을 사용하면 리소스 활용을 최적화하고 발생할 수 있는 문제점 해결 사안을 더 잘 이해할 수 있습니다. 클러스터에 대한 로깅 및 모니터링 솔루션을 설정하려면 [로깅 및 모니터링](/docs/containers?topic=containers-health#health)을 참조하십시오.
{: shortdesc}

로깅 및 모니터링을 설정할 때 다음 고려사항에 유의하십시오.

<dl>
<dt>클러스터 상태를 판별하도록 로그 및 메트릭 수집</dt>
  <dd>Kubernetes에는 기본 클러스터 레벨 성능을 판별하는 데 도움이 되는 메트릭이 포함됩니다. `kubectl top (pods | nodes)` 명령을 실행하여 [Kubernetes 대시보드](/docs/containers?topic=containers-app#cli_dashboard) 또는 터미널에서 이 메트릭을 검토할 수 있습니다. 자동화에서 이러한 명령을 포함할 수 있습니다.<br><br>
  로그를 나중에 분석할 수 있도록 로그를 로그 분석 도구에 전달하십시오. 필요한 로그보다 더 많은 로그를 저장하지 않으려면 로깅할 로그의 상세 출력 및 레벨을 정의하십시오. 로그는 앱 성능에 영향을 주고 로그 분석을 좀 더 어렵게 만들 수 있는 많은 스토리지를 빠르게 사용할 수 있습니다.</dd>
<dt>앱 성능 테스트</dt>
  <dd>로깅 및 모니터링 설정 후 성능 테스트를 수행하십시오. 테스트 환경에서 다양한 비이상적인 시나리오(예: 구역의 실패를 복제하도록 구역에서 모든 작업자 노드 삭제)를 의도적으로 작성하십시오. 로그 및 메트릭을 검토하여 앱이 복구되는 방식을 확인하십시오.</dd>
<dt>감사 준비</dt>
  <dd>앱 로그 및 클러스터 메트릭 외에도 사용자는 누가 어떠한 클러스터 및 Kubernetes 조치를 수행했는지에 대한 감사 가능한 레코드를 보유할 수 있도록 활동 추적을 설정하려고 합니다. 자세한 정보는 [{{site.data.keyword.cloudaccesstrailshort}}](/docs/containers?topic=containers-at_events#at_events)의 내용을 참조하십시오.</dd>
</dl>
