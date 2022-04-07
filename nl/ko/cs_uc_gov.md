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


# {{site.data.keyword.Bluemix_notm}}의 정부 유스 케이스
{: #cs_uc_gov}

다음 유스 케이스는 {{site.data.keyword.containerlong_notm}}의 워크로드가 퍼블릭 클라우드를 활용하면 어떤 이점이 있는지 보여줍니다. 이러한 워크로드는 신뢰할 수 있는 컴퓨팅을 통해 격리되고, 데이터 주권을 위해 전 세계 지역에 있으며, 순수 새 코드 대신 Watson 기계 학습을 사용하고, 온프레미스 데이터베이스에 연결합니다.
{: shortdesc}

## 지역 정부가 공공-민간 데이터를 결합하는 지역 공동체 개발자와의 협업 과정 및 속도를 개선함
{: #uc_data_mashup}

열린 정부 데이터 프로그램 책임자가 공공 데이터를 지역 공동체 및 민간 부문과 공유해야 하지만, 이 데이터가 온프레미스 모놀리식 시스템에 갇혀 있습니다.
{: shortdesc}

{{site.data.keyword.Bluemix_notm}}를 사용해야 하는 이유: {{site.data.keyword.containerlong_notm}}를 통해, 책임자는 변화를 주도하는, 결합된 공공-민간 데이터의 가치를 제공합니다. 마찬가지로, 이 서비스는 기존 모놀리식 온프레미스 앱을 다시 작성하여 마이크로서비스를 노출하는 데 필요한 퍼블릭 클라우드를 제공합니다. 또한 퍼블릭 클라우드는 정부 및 공공 파트너십에서 외부 클라우드 서비스 및 협업 친화적인 오픈 소스 도구를 사용할 수 있게 해 줍니다.

주요 기술:    
* [다양한 CPU, RAM, 스토리지 요구사항을 만족시키는 클러스터](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)
* [{{site.data.keyword.contdelivery_full}}의 공개 도구 체인을 포함한 DevOps 기본 도구](https://www.ibm.com/cloud/garage/toolchains/)
* [{{site.data.keyword.cos_full_notm}}를 사용하여 공공 데이터에 대한 액세스 제공](/docs/services/cloud-object-storage?topic=cloud-object-storage-about)
* [IBM Cloud 분석 서비스 즉시 사용](https://www.ibm.com/cloud/analytics)

**컨텍스트: 지역 정부가 공공-민간 데이터를 결합하는 지역 공동체 개발자와의 협업 과정 및 속도를 개선함**
* 이 지역 정부 기관의 목표는 "열린 정부" 모델이지만, 현재 온프레미스 시스템으로는 이를 달성할 수 없습니다.
* 이 기관은 혁신을 지원하고 민간 부문, 시민 및 공공 기관과의 공동 개발을 장려하려 합니다.
* 정부와 민간 조직의 서로 다른 개발자 그룹에는 API 및 데이터를 쉽게 공유할 수 있는 통합된 오픈 소스 플랫폼이 없습니다.
* 정부 데이터가 외부에서 액세스하기 어려운 온프레미스 시스템에 갇혀 있습니다.

**솔루션**

열린 정부로의 전환은 성능, 복원력, 비즈니스 연속성 및 보안을 제공하는 기반에서 이뤄져야 합니다. 혁신 및 공동 개발이 진행됨에 따라, 기관 및 시민들은 소프트웨어, 서비스 및 인프라 회사가 "보호하고 봉사하는" 역할을 수행할 것을 기대합니다.

관료주의를 타파하고 정부와 유권자의 관계를 변화시키기 위해, 이 기관은 공동 개발을 위한 플랫폼을 구축하는 데 있어서 개방형 표준을 따릅니다.

* 열린 데이터 - 시민, 정부 기관 및 기업에서 자유롭게 액세스하고, 공유하고, 개선할 수 있는 데이터 스토리지
* 얼린 API - 모든 지역 공동체 참여자가 API를 기여하고 재사용하는 개발 플랫폼
* 열린 혁신 - 개발자가 혁신적인 부분을 수동으로 코딩하는 대신 추가할 수 있도록 해 주는 클라우드 서비스 세트

먼저, 정부는 {{site.data.keyword.cos_full_notm}}를 사용하여 공공 데이터를 클라우드에 저장합니다. 이 스토리지는 자유롭게 사용하거나 재사용할 수 있고, 누구나 공유할 수 있으며, 기여 및 공유 등에 대해서만 대상이 됩니다. 민감한 데이터는 클라우드에 푸시되기 전에 제거될 수 있습니다. 그 외에, 지역 공동체에서 개선된 기존 자유 사용 데이터에 대한 POC를 선보일 수 있는 새 데이터 스토리지를 클라우드가 제한하는 액세스 제어를 설정할 수 있습니다.

공공-민간 파트너십을 위한 정부의 다음 단계는 {{site.data.keyword.apiconnect_long}}에서 호스팅되는 API 체계를 확립하는 것입니다. 여기서 지역 공동체 및 기업 개발자는 API 양식으로 손쉽게 액세스할 수 있도록 데이터를 변경합니다. 이들의 목표는 상호 운용을 가능하게 하고 앱 통합을 가속화하기 위해 공용으로 사용 가능한 REST API를 작성하는 것입니다. 이들은 IBM {{site.data.keyword.SecureGateway}}를 사용하여 민간 온프레미스 데이터 소스에 연결합니다.

마지막으로, 이러한 공유 API를 기반으로 하는 앱이 클러스터 스핀업이 용이한 {{site.data.keyword.containerlong_notm}}에서 호스팅됩니다. 이렇게 되면 지역 공동체, 민간 부문 및 정부의 개발자가 앱을 쉽게 공동 개발할 수 있게 됩니다. 간단히 말하면, 개발자는 인프라를 관리하기보다 코딩에 집중해야 합니다. 따라서 개발자들은 IBM의 간략화된 인프라 관리 기능을 이용할 수 있는 {{site.data.keyword.containerlong_notm}}를 선택합니다.
* Kubernetes 마스터, IaaS 및 운영 컴포넌트(Ingress 및 스토리지 등) 관리
* 작업자 노드의 상태 및 복구 모니터링
* 워크로드 및 데이터가 있어야 하는 세계 어느 지역에서든 개발자가 직접 인프라를 확보할 필요가 없도록 글로벌 컴퓨팅 인프라 제공

컴퓨팅 워크로드를 {{site.data.keyword.Bluemix_notm}}로 이동하는 것만으로는 충분하지 않습니다. 정부는 프로세스 및 방법 또한 변경해야 합니다. 제공업체는 IBM Garage Method의 방법을 채택하여 지속적 통합 및 지속적 딜리버리(CI/CD)와 같은 최신 DevOps 방법을 지원하는 반복적인 Agile 전달 프로세스를 구현할 수 있습니다.

대부분의 CI/CD 프로세스 자체는 클라우드의 {{site.data.keyword.contdelivery_full}}를 통해 자동화됩니다. 제공업체는 컨테이너 이미지를 준비하고, 취약성을 확인하고, 이를 Kubernetes 클러스터에 배치하는 데 사용되는 워크플로우 도구 체인을 정의할 수 있습니다.

**솔루션 모델**

온프레미스 데이터 소스와의 사이에 안전한 액세스가 가능한, 퍼블릭 클라우드에서 실행되는 요청 시 컴퓨팅, 스토리지 및 API 도구

기술적 솔루션:
* {{site.data.keyword.containerlong_notm}}
* {{site.data.keyword.cos_full_notm}} 및 {{site.data.keyword.cloudant}}
* {{site.data.keyword.apiconnect_long}}
* IBM {{site.data.keyword.SecureGateway}}
* {{site.data.keyword.contdelivery_full}}

**1단계: 클라우드에 데이터 저장**
* {{site.data.keyword.cos_full_notm}}는 퍼블릭 클라우드 내의 모든 사용자가 액세스할 수 있는 히스토리 데이터 스토리지를 제공합니다.
* {{site.data.keyword.cloudant}}를 개발자 제공 키와 함께 사용하여 데이터를 클라우드에 캐시합니다.
* IBM {{site.data.keyword.SecureGateway}}를 사용하여 기존 온프레미스 데이터베이스와 안전한 연결을 유지합니다.

**2단계: API를 사용하여 데이터에 대한 액세스 제공**
* API 체계 플랫폼에 {{site.data.keyword.apiconnect_long}}를 사용합니다. API는 공공 및 민간 부문에서 자신의 앱으로 데이터를 결합할 수 있게 해 줍니다.
* API에 의해 작동하는, 공공-민간 앱을 위한 클러스터를 작성합니다.
* 앱의 기능 영역 및 해당 종속 항목을 기반으로, 앱을 {{site.data.keyword.containerlong_notm}} 내에서 실행되는 협업 마이크로서비스의 세트로 구성합니다.
* 앱을 {{site.data.keyword.containerlong_notm}}에서 실행되는 컨테이너에 배치합니다. {{site.data.keyword.containerlong_notm}}의 기본 제공 HA 도구는 자가 수리 및 로드 밸런싱을 포함, 워크로드의 균형을 맞춥니다.
* 모든 유형의 개발자에게 친숙한 오픈 소스 도구인 Kubernetes를 통해 표준화된 DevOps 대시보드를 제공합니다.

**3단계: IBM Garage 및 클라우드 서비스를 사용한 혁신**
* IBM Garage Method의 반복적인 Agile 개발 방법을 채택하여 작동 중단 시간이 없는 기능, 패치 및 수정사항 릴리스를 가능하게 합니다.
* {{site.data.keyword.contdelivery_full}}는 공공 또는 민간 부문의 개발자가 사용자 정의할 수 있는, 공유 가능한 템플리트를 사용하여 통합된 도구 체인을 빠르게 제공하는 데 도움을 줍니다.
* 개발자는 개발 및 테스트 클러스터에 앱을 빌드하고 테스트한 후 {{site.data.keyword.contdelivery_full}} 도구 체인을 사용하여 프로덕션 클러스터에 앱을 배치합니다.
* 개발자는 {{site.data.keyword.Bluemix_notm}} 카탈로그에서 사용 가능한 Watson AI, 기계 학습 및 딥러닝 도구를 사용함으로써 전문 분야의 문제점에 집중할 수 있습니다. 서비스 바인딩을 통해, 사용자 정의 고유 ML 코드 대신 ML 로직이 앱에 포함됩니다.

**결과**
* 일반적으로는 느린 공공-민간 파트너십이 이제 몇 개월이 아니라 몇 주일 내로 앱을 신속하게 개발합니다. 이러한 개발 파트너십은 이제 기능 및 버그 수정사항을 1주일에 최대 10번 제공합니다.
* 모든 참여자가 Kubernetes와 같이 널리 얄려진 오픈 소스 도구를 사용하면 개발이 가속화됩니다. 긴 학습 곡선은 더 이상 걸림돌이 아닙니다.
* 시민들과 민간 부문에 활동, 정보 및 계획이 투명하게 제공됩니다. 이를 통해 시민들은 정부 프로세스, 서비스 및 지원과 통합됩니다.
* 공공-민간 파트너십이 지카 바이러스 추적, 스마트 배전, 범죄 통계 분석, 대학의 "뉴칼라" 교육과 같은 매우 어려운 문제들을 해결합니다.

## 대형 공공 항만에서 공공 조직과 민간 조직을 연결하는 항만 데이터 및 선박 화물 목록의 교환을 보호함
{: #uc_port}

민간 운송 회사와 정부 운영 항만의 IT 책임자들이 항만 정보를 연결하고, 가시성을 제공하고, 안전하게 교환하려 합니다. 그러나 공공 항만 정보와 민간 운송 화물 목록을 연결하는 통합된 시스템이 없습니다.
{: shortdesc}

{{site.data.keyword.Bluemix_notm}}를 사용해야 하는 이유: {{site.data.keyword.containerlong_notm}}는 정부 및 공공 파트너십에서 외부 클라우드 서비스 및 협업 친화적인 오픈 소스 도구를 사용할 수 있게 해 줍니다. 컨테이너는 항만과 운송 회사 모두 공유된 정보가 안전한 플랫폼에 호스팅되었다고 인정할 수 있는 공유 가능한 플랫폼을 제공합니다. 또한 이 플랫폼은 소형 개발-테스트 시스템에서 프로덕션 규모의 시스템에 이르기까지 자유롭게 스케일링됩니다. 공개 도구 체인은 빌드, 테스트 및 배치를 자동화함으로써 개발을 더욱 가속화합니다.

주요 기술:    
* [다양한 CPU, RAM, 스토리지 요구사항을 만족시키는 클러스터](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)
* [컨테이너 보안 및 격리](/docs/containers?topic=containers-security#security)
* [{{site.data.keyword.contdelivery_full}}의 공개 도구 체인을 포함한 DevOps 기본 도구](https://www.ibm.com/cloud/garage/toolchains/)
* [SDK for Node.js](/docs/runtimes/nodejs?topic=Nodejs-nodejs_runtime#nodejs_runtime)

**컨텍스트: 항만에서 공공 조직과 민간 조직을 연결하는 항만 데이터 및 선박 화물 목록의 교환을 보호함**

* 정부와 민간 조직의 서로 다른 개발자 그룹에는 협업을 수행할 수 있는 통합된 플랫폼이 없으며, 이는 업데이트 및 기능의 배치 속도를 저하시킵니다.
* 개발자는 전 세계 각 조직에 퍼져 있으며 이는 오픈 소스 및 PaaS가 최선의 선택사항임을 의미합니다.
* 보안은 주된 문제이며 이는 소프트웨어의 기능 및 업데이트에 영향을 주는 협업 부담을 가중시킵니다(특히 앱이 프로덕션 상태가 된 후).
* JIT(Just-In-Time) 데이터는 운송 활동의 지연 시간을 줄이기 위해 전 세계의 시스템이 고가용성을 갖춰야 함을 의미합니다. 선박 터미널의 시간표는 엄격하게 제어되며 변경할 수 없는 경우도 있습니다. 웹 사용이 늘어나고 있으므로 불안정성은 좋지 않은 사용자 경험을 발생시킬 수 있습니다.

**솔루션**

항만과 운송 회사가 화물 및 선박에 대한 허가를 위해 규제 준수 관련 정보를 여러 기관에 몇 번씩 제출할 필요 없이 한 번만 전자 제출하면 되도록 하는 통합된 거래 시스템을 공동 개발합니다. 화물 목록 및 통관 앱은 특정 화물의 내용물을 빠르게 공유하고 모든 서류가 전자 전송되어 항만 기관에서 처리되도록 할 수 있습니다.

따라서 두 기관은 거래 시스템 솔루션을 개발하기 위한 파트너십을 체결합니다.
* 신고 - 운송 화물 목록을 제출받아 일반적인 통관 서류를 디지털로 처리하고, 조사 및 집행을 위해 정책을 위반한 항목에 플래그를 지정하는 앱
* 관세 - 관세를 계산하고, 운송자에게 비용을 전자 통보하고, 디지털로 지불을 받는 앱
* 규제 - 계속해서 변경되는 수입, 수출 및 관세 처리에 영향을 주는 정책 및 규제를 앞의 두 앱에 전달하는, 유연하며 구성 가능한 앱

개발자들은 {{site.data.keyword.containerlong_notm}}를 사용하여 컨테이너에 자신의 앱을 배치하는 것으로 작업을 시작합니다. 이들은 전 세계의 개발자들이 함께 앱 개선사항을 빠르게 배치할 수 있게 해 주는 공유 개발 환경을 위해 클러스터를 작성합니다. 컨테이너는 각 개발 팀에서 원하는 언어를 사용할 수 있게 해 줍니다.

보안 우선: IT 책임자들은 클러스터를 호스팅하기 위한 베어메탈로 신뢰할 수 있는 컴퓨팅을 선택합니다. {{site.data.keyword.containerlong_notm}}에 베어메탈을 사용함으로써, 민감한 통관 워크로드는 이전과 마찬가지로 격리되는 동시에 퍼블릭 클라우드를 통한 유연함을 갖추게 됩니다. 베어메탈은 기반 하드웨어의 변조 여부를 확인할 수 있는, 신뢰할 수 있는 컴퓨팅을 제공합니다.

운송 회사는 다른 항만과도 협력하고자 하므로, 앱 보안은 필수입니다. 선박 화물 목록 및 통관 정보는 극비 정보입니다. Vulnerability Advisor는 안전한 코어에서 다음 스캔을 제공합니다.
* 이미지 취약성 스캔
* ISO 27k를 기반으로 하는 정책 스캔
* 실시간 컨테이너 스캔
* 알려진 악성코드에 대한 패키지 스캔

동시에, {{site.data.keyword.iamlong}}는 리소스에 대해 누가, 어떤 액세스 권한 레벨을 갖는지 제어하는 데 도움을 줍니다.

개발자는 기존 도구를 사용하여 전문 분야의 문제점에 집중합니다. 이들은 고유 로깅 및 모니터링 코드를 작성하는 대신 {{site.data.keyword.Bluemix_notm}} 서비스를 클러스터에 바인드하여 이를 앱에 포함합니다. IBM이 Kubernetes와 인프라 업그레이드, 보안 등을 관리하므로 개발자는 또한 인프라 관리 태스크로부터도 자유로워집니다.

**솔루션 모델**

필요한 경우 전 세계 어디서든 운송 데이터에 안전하게 액세스할 수 있는, 퍼블릭 클라우드에서 실행되는 요청 시 컴퓨팅, 스토리지 및 Node.js 스타터 킷. 클러스터의 컴퓨팅 인프라는 변조에 대해 안전하며 베어메탈에 격리되어 있습니다.  

기술적 솔루션:
* 신뢰할 수 있는 컴퓨팅을 사용하는 {{site.data.keyword.containerlong_notm}}
* {{site.data.keyword.openwhisk}}
* {{site.data.keyword.cloudant}}
* IBM {{site.data.keyword.SecureGateway}}

**1단계: 마이크로서비스를 사용한 앱 컨테이너화**
* IBM의 Node.js 스타터 킷을 사용하여 개발을 즉시 시작힙니다.
* 앱의 기능 영역 및 해당 종속 항목을 기반으로, 앱을 {{site.data.keyword.containerlong_notm}} 내에서 실행되는 협업 마이크로서비스의 세트로 구성합니다.
* {{site.data.keyword.containerlong_notm}}에서 실행되는 컨테이너에 화물 목록 및 화물 앱을 배치합니다.
* Kubernetes를 통해 표준화된 DevOps 대시보드를 제공합니다.
* IBM {{site.data.keyword.SecureGateway}}를 사용하여 기존 온프레미스 데이터베이스와 안전한 연결을 유지합니다.

**2단계: 전 세계에서의 가용성 보장**
* 개발자는 개발 및 테스트 클러스터에 앱을 배치한 후 {{site.data.keyword.contdelivery_full}} 도구 체인 및 Helm을 사용하여 전 세계의 클러스터에 각 국가별 앱을 배치합니다.
* 이렇게 하면 워크로드 및 데이터가 지역 규제에 부합하게 됩니다.
* {{site.data.keyword.containerlong_notm}}의 기본 제공 HA 도구는 자가 수리 및 로드 밸런싱을 포함, 각 지역 내 워크로드의 균형을 맞춥니다.

**3단계: 데이터 공유**
* {{site.data.keyword.cloudant}}는 키-값부터 복잡한 문서 지향 데이터 저장 및 조회에 이르기까지, 다양한 데이터 중심 유스 케이스에 적합한 최신 NoSQL 데이터베이스입니다.
* 여러 앱 전체에서 사용자의 세션 데이터를 캐시함으로써 지역 데이터베이스에 대한 조회를 최소화하기 위해 {{site.data.keyword.cloudant}}가 사용됩니다.
* 이 구성은 {{site.data.keyword.containershort}}에 있는 앱 전체에서 프론트 엔드 앱 사용성 및 성능을 향상시킵니다.
* {{site.data.keyword.containerlong_notm}}의 작업자 앱은 온프레미스 데이터를 분석하고 결과를 {{site.data.keyword.cloudant}}에 저장하는 반면, {{site.data.keyword.openwhisk}}는 변경사항에 반응하며 수신 데이터 피드에서 민감한 데이터를 자동으로 제거합니다.
* 마찬가지로, 모든 다운스트림 이용자가 새 데이터에 액세스할 수 있도록, 한 지역의 화물에 대한 알림은 데이터 업로드를 통해 트리거될 수 있습니다.

**결과**
* IBM 스타터 킷, {{site.data.keyword.containerlong_notm}} 및 {{site.data.keyword.contdelivery_full}} 도구를 사용하여, 각 조직 및 정부 전역에서 전 세계의 개발자들이 공조합니다. 이들은 친숙하며 상호 운용 가능한 도구를 사용하여 통관 앱을 함께 개발합니다.
* 마이크로서비스가 패치, 버그 수정 및 새 기능의 제공 소요 시간을 크게 줄여줍니다. 초기 개발은 빠르게 진행되며, 업데이트는 1주일에 10회에 이를 정도로 빈번합니다.
* 운송 고객 및 정부 관계자가 지역 규제를 준수하면서 화물 목록 데이터에 액세스하고 통관 데이터를 공유할 수 있습니다.
* 운송 회사는 공급 체인에서의 개선된 물류 관리(비용 절감 및 더 빠른 허가 시간)로부터 이익을 얻습니다.
* 처리량의 99%는 디지털 신고이며, 수입의 90%는 사람이 개입하지 않고 처리됩니다.
