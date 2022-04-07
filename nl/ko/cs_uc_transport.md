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


# {{site.data.keyword.cloud_notm}}의 수송 유스 케이스
{: #cs_uc_transport}

다음 유스 케이스는 {{site.data.keyword.containerlong_notm}}의 워크로드가
빠른 앱 업데이트 및 전 세계 다중 지역 배치를 위해 도구 체인을 어떻게 활용하는지 보여줍니다. 동시에, 이러한 워크로드는 기존 백엔드 시스템에 연결하고, 개인화를 위해 Watson AI를 사용하고, {{site.data.keyword.messagehub_full}}를 사용하여 IOT 데이터에 액세스할 수 있습니다.

{: shortdesc}

## 운송 회사가 비즈니스 파트너 에코시스템을 위해 전 세계 시스템의 가용성을 향상시킴
{: #uc_shipping}

IT 책임자에게 파트너가 상호작용하는 전 세계 규모의 선박 항로 지정 및 스케줄 관리 시스템이 있습니다. 파트너는 IoT 디바이스 데이터에 액세스하는 이러한 시스템으로부터 분 단위의 정보를 필요로 합니다. 그러나 충분한 고가용성을 제공하면서 이러한 시스템을 전 세계 규모로 스케일링할 수는 없었습니다.
{: shortdesc}

{{site.data.keyword.cloud_notm}}를 사용해야 하는 이유: {{site.data.keyword.containerlong_notm}}는 늘어나는 수요에 부합하기 위해 99.999%의 가용성을 제공하면서 컨테이너화된 앱을 스케일링합니다. 개발자들이 개발 및 테스트 시스템으로 변경사항을 신속히 푸시하며 손쉽게 실험을 수행할 수 있게 되면 매일 40번 앱 배치가 수행됩니다. IoT 플랫폼은 IoT 데이터에 쉽게 액세스할 수 있도록 해 줍니다.

주요 기술:    
* [비즈니스 파트너 에코시스템을 위한 다중 지역](/docs/containers?topic=containers-regions-and-zones#regions-and-zones)
* [수평적 확장](/docs/containers?topic=containers-app#highly_available_apps)
* [{{site.data.keyword.contdelivery_full}}의 공개 도구 체인](https://www.ibm.com/cloud/garage/toolchains/)
* [혁신을 위한 클라우드 서비스](https://www.ibm.com/cloud/products/#analytics)
* [앱에 이벤트 데이터를 피드하기 위한 {{site.data.keyword.messagehub_full}}](/docs/services/EventStreams?topic=eventstreams-about#about)

**운송 회사가 비즈니스 파트너 에코시스템을 위해 전 세계 시스템의 가용성을 향상시킴**

* 운송 물류의 지역별 차이로 인해 여러 국가에서 늘어나는 파트너의 수에 대처하기 어려워졌습니다. 예를 들면, 회사가 국경 간에 일관된 기록을 유지해야 하는 고유 규제 및 운송 물류가 있습니다.
* JIT(Just-In-Time) 데이터는 운송 활동의 지연 시간을 줄이기 위해 전 세계의 시스템이 고가용성을 갖춰야 함을 의미합니다. 선박 터미널의 시간표는 엄격하게 제어되며 변경할 수 없는 경우도 있습니다. 웹 사용이 늘어나고 있으므로 불안정성은 좋지 않은 사용자 경험을 발생시킬 수 있습니다.
* 개발자는 지속적으로 앱을 발전시켜야 하지만 기존 도구는 이들의 업데이트 및 기능 배치 능력을 저하시켰습니다.  

**솔루션**

운송 회사는 운송 시간표, 인벤토리 및 통과 서류를 서로 밀접하게 관리해야 합니다. 이렇게 해야 화물의 위치, 운송 화물, 고객으로의 전달 일정을 정확하게 공유할 수 있습니다. 회사는 물품(가전제품, 의류 또는 농산물 등)이 도착하는 시간을 정확히 파악할 수 있도록 하여, 운송 고객이 이 정보를 자신의 고객에게 전달할 수 있도록 하려 합니다.

이 솔루션은 다음 기본 컴포넌트로 구성됩니다.
1. 각 운송 컨테이너의 IoT 디바이스로부터의 데이터 스트리밍: 화물 목록 및 위치
2. 액세스 제어를 포함, 해당 항만 및 운송 파트너와 디지털로 공유되는 통관 서류
3. 운송 고객이 자신의 소매 및 B2B 앱에서 화물 데이터를 재사용하는 데 필요한 운송 고객용 API를 포함, 운송되는 물품에 대한 도착 정보를 집계하고 전달하는 운송 고객용 앱

운송 회사가 글로벌 파트너와 함께 일하기 위해서는 항로 설정 및 스케줄 관리 시스템이 각 지역의 언어, 규제 및 고유 항만 물류에 맞도록 수정되어야 합니다. {{site.data.keyword.containerlong_notm}}는 회사의 앱이 각 국가에서 파트너의 요구사항을 반영할 수 있도록 북아메리카, 유럽, 아시아 및 오스트레일리아를 포함한 여러 지역에서 전 세계 규모로 서비스를 제공합니다.

IoT 디바이스는 데이터를 스트리밍하며 {{site.data.keyword.messagehub_full}}는 이를 지역 항만 앱, 그리고 연관된 통관 및 컨테이너 화물 목록 데이터 저장소로 분배합니다. {{site.data.keyword.messagehub_full}}는 IoT 이벤트의 도착 지점입니다. 이는 Watson IoT Platform에서 {{site.data.keyword.messagehub_full}}에 제공하는 관리 대상 연결을 기반으로 이벤트를 분배합니다.

이벤트는 {{site.data.keyword.messagehub_full}}에 집계된 후 항만 운송 앱에서 즉시 사용하거나 미래에 사용할 수 있도록 지속됩니다. 가장 낮은 대기 시간을 필요로 하는 앱은 이벤트 스트림({{site.data.keyword.messagehub_full}})으로부터 직접 실시간으로 이용합니다. 이벤트를 나중에 사용하는 기타 앱(분석 도구 등)은 {{site.data.keyword.cos_full}}를 사용하는 이벤트 저장소로부터 일괄처리 모드로 이용하도록 선택할 수 있습니다.

운송 데이터가 회사의 고객과 공유되므로, 개발자는 고객이 API를 사용하여 자신의 앱에 운송 데이터를 표시할 수 있도록 합니다. 이러한 앱의 예로는 모바일 추적 앱 또는 웹 전자상거래 솔루션이 있습니다. 또한 개발자는 지역 항만 앱을 빌드 및 유지보수하고, 통관 기록 및 운송 화물 목록을 수집 및 전달하기 바쁩니다. 간단히 말하면, 개발자는 인프라를 관리하기보다 코딩에 집중해야 합니다. 따라서 개발자들은 IBM의 간략화된 인프라 관리 기능을 이용할 수 있는 {{site.data.keyword.containerlong_notm}}를 선택합니다.
* Kubernetes 마스터, IaaS 및 운영 컴포넌트(Ingress 및 스토리지 등) 관리
* 작업자 노드의 상태 및 복구 모니터링
* 개발자가 워크로드 및 데이터가 있어야 하는 지역에서 직접 인프라를 확보할 필요가 없도록 글로벌 컴퓨팅 인프라 제공

전 세계에서의 가용성을 달성하기 위해 개발, 테스트 및 프로덕션 시스템이 세계 곳곳에 있는 여러 데이터 센터에 배치됩니다. 회사에서는 고가용성을 위해 다중 지역 클러스터 외에, 서로 다른 지역의 다중 클러스터 조합을 사용합니다. 회사는 비즈니스 요구사항을 만족시키기 위해 다음과 같이 항만 앱을 손쉽게 배치할 수 있습니다.
* 유럽 규제를 준수하기 위해 프랑크푸르트 클러스터에 배치
* 지역 가용성 및 장애 복구를 보장하기 위해 미국 클러스터에 배치

회사는 앱의 유럽 버전의 가용성을 보장하고 효율적으로 워크로드의 균형을 맞출 수 있도록 프랑크푸르트 내의 여러 지역 클러스터에 워크로드를 분산시킵니다. 각 지역이 항만 앱에 고유 데이터를 업로드하므로 앱의 클러스터는 대기 시간이 낮은 지역에서 호스팅됩니다.

개발자의 경우 대부분의 지속적 통합 및 지속적 딜리버리(CI/CD) 프로세스는 {{site.data.keyword.contdelivery_full}}를 통해 자동화할 수 있습니다. 회사는 컨테이너 이미지를 준비하고, 취약성을 확인하고, 이를 Kubernetes 클러스터에 배치하는 데 사용되는 워크플로우 도구 체인을 정의할 수 있습니다.

**솔루션 모델**

필요한 경우 전 세계의 화물 데이터에 액세스할 수 있는, 퍼블릭 클라우드에서 실행되는 요청 시 컴퓨팅, 스토리지 및 이벤트 관리

기술적 솔루션:
* {{site.data.keyword.containerlong_notm}}
* {{site.data.keyword.messagehub_full}}
* {{site.data.keyword.cos_full}}
* {{site.data.keyword.contdelivery_full}}

**1단계: 마이크로서비스를 사용한 앱 컨테이너화**

* 앱의 기능 영역 및 해당 종속 항목을 기반으로, 앱을 {{site.data.keyword.containerlong_notm}} 내 협업 마이크로서비스의 세트로 통합합니다.
* 앱을 {{site.data.keyword.containerlong_notm}}에 있는 컨테이너에 배치합니다.
* Kubernetes를 통해 표준화된 DevOps 대시보드를 제공합니다.
* 비정기적으로 실행되는 일괄처리 및 기타 인벤토리 워크로드를 위해 컴퓨팅 인프라의 요청 시 스케일링을 사용으로 설정합니다.
* {{site.data.keyword.messagehub_full}}를 사용하여 IoT 디바이스로부터의 스트리밍 데이터를 관리합니다.

**2단계: 전 세계에서의 가용성 보장**
* {{site.data.keyword.containerlong_notm}}의 기본 제공 HA 도구는 자가 수리 및 로드 밸런싱을 포함, 각 지역 내 워크로드의 균형을 맞춥니다.
* 로드 밸런싱, 방화벽 및 DNS는 IBM Cloud Internet Services가 처리합니다.
* 워크로드와 데이터가 지역 요구사항을 만족시킬 수 있도록, 앱은 도구 체인 및 Helm 배치 도구를 사용하여 전 세계의 클러스터에도 배치됩니다.

**3단계: 데이터 공유**
* {{site.data.keyword.cos_full}}와 {{site.data.keyword.messagehub_full}}는 실시간 및 히스토리 데이터 스토리지를 제공합니다.
* API는 운송 회사의 고객이 자신의 앱과 데이터를 공유할 수 있도록 합니다.

**4단계: 지속적 제공**
* {{site.data.keyword.contdelivery_full}}는 개발자가 사용자 정의할 수 있으며 공유 가능한 템플리트를 IBM, 서드파티 및 오픈 소스에서 제공하는 도구와 함께 사용하여 통합 도구 체인을 신속하게 제공할 수 있도록 도와줍니다. 개발자는 분석으로 품질을 제어하면서 빌드와 테스트를 자동화합니다.
* 개발자는 개발 및 테스트 클러스터에 앱을 빌드하고 테스트한 후, IBM CI/CD 도구 체인을 사용하여 전 세계의 클러스터에 앱을 배치합니다.
* {{site.data.keyword.containerlong_notm}}는 앱의 손쉬운 롤아웃 및 롤백 기능을 제공합니다. Istio의 지능형 라우팅 및 로드 밸런싱 기능을 통해, 맞춤 작성된 앱이 지역 요구사항을 만족시키기 위해 배치됩니다.

**결과**

* {{site.data.keyword.containerlong_notm}} 및 IBM CI/CD 도구를 사용하여, 앱의 지역 버전이 데이터가 수집되는 실제 디바이스와 가까운 곳에서 호스팅됩니다.
* 마이크로서비스가 패치, 버그 수정 및 새 기능의 제공 소요 시간을 크게 줄여줍니다. 초기 개발은 빠르게 진행되며, 업데이트는 빈번합니다.
* 운송 고객은 화물의 위치, 전달 일정, 그리고 승인된 항만 기록까지도 실시간으로 액세스할 수 있습니다.
* 여러 운송 터미널의 운송 파트너가 화물 목록 및 화물 세부사항을 미리 파악함으로써 현장 물류가 지연되지 않고 개선됩니다.
* 이 케이스와는 별개로, Blockchain을 사용하여 국제 공급망을 개선하기 위해 [Maersk와 IBM이 합작투자를 실시하기로 합의하였습니다](https://www.ibm.com/press/us/en/pressrelease/53602.wss).

## 항공사가 3주일 안에 혁신적인 인적 자원(HR) 혜택 사이트를 제공함
{: #uc_airline}

HR 책임자(CHRO)가 혁신적인 챗봇을 사용하는 새 HR 혜택 사이트를 제공하려 하지만 현재 개발 도구 및 플랫폼으로는 앱이 실용화되기까지 오랜 시간이 소요됩니다. 이 상황은 하드웨어 조달을 위한 긴 대기 시간을 포함합니다.
{: shortdesc}

{{site.data.keyword.cloud_notm}}를 사용해야 하는 이유: {{site.data.keyword.containerlong_notm}}는 컴퓨팅 인프라의 쉬운 스핀업을 제공합니다. 이렇게 되면 개발자들은 공개 도구 체인을 사용하여 개발 및 테스트 시스템으로 변경사항을 신속히 푸시하며 손쉽게 실험을 수행할 수 있게 됩니다. 기존 소프트웨어 개발 도구는 IBM Watson Assistant가 추가되면 새로운 활력을 얻습니다. 3주일 안에 새 혜택 사이트가 작성됩니다.

주요 기술:    
* [다양한 CPU, RAM, 스토리지 요구사항을 만족시키는 클러스터](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)
* [Watson에서 제공하는 챗봇 서비스](https://developer.ibm.com/code/patterns/create-cognitive-banking-chatbot/)
* [{{site.data.keyword.contdelivery_full}}의 공개 도구 체인을 포함한 DevOps 기본 도구](https://www.ibm.com/cloud/garage/toolchains/)
* [SDK for Node.js](/docs/runtimes/nodejs?topic=Nodejs-nodejs_runtime#nodejs_runtime)

**컨텍스트: 3주일 내에 혁신적인 HR 혜택 사이트를 신속하게 빌드하고 배치**
* 직원 증가 및 HR 정책 변경으로 인해 연간 등록에 완전히 새로운 사이트가 필요합니다.
* 챗봇과 같은 대화식 기능은 기존 직원에게 새 HR 정책을 전달하는 데 도움을 줄 것입니다.
* 직원 수의 증가로 인해 사이트 트래픽이 증가하고 있지만 인프라 예산은 변함이 없습니다.
* HR 팀은 새 사이트 기능을 빠르게 롤아웃하고 혜택의 최신 변경사항을 빈번하게 게시하는 등과 같이 더 빠르게 움직여 달라는 압력에 직면해 있습니다.
* 등록 기간은 2주일이며, 따라서 새 앱을 위한 가동 중단 시간은 용납되지 않습니다.

**솔루션**

항공사는 사람을 우선하는 열린 문화를 설계하고자 합니다. HR 책임자는 재능 있는 직원에게 보상하여 유지하는 것이 항공사의 수익성에 영향을 준다는 점을 잘 알고 있습니다. 따라서 연간 혜택의 공개는 직원 중심의 문화를 배양하는 데 있어서 중요한 요소입니다.

회사는 개발자와 해당 사용자에게 도움을 주는 다음과 같은 솔루션을 필요로 합니다.
* 기존 혜택의 프론트 엔드: 보험, 교육 기회 제공, 복지 등
* 지역별 기능: 각 국가에는 고유한 HR 정책이 있으므로 전체적인 사이트의 모습은 유사할 수 있으나 지역별 혜택을 포함함
* 기능의 롤아웃 및 버그 수정을 가속화하는 개발자 친화적인 도구
* 혜택에 대한 확실한 정보를 제공하고, 사용자 요청 및 질문을 효율적으로 해결하기 위한 챗봇

기술적 솔루션:
* {{site.data.keyword.containerlong_notm}}
* IBM Watson Assistant 및 Tone Analyzer
* {{site.data.keyword.contdelivery_full}}
* IBM Logging and Monitoring
* {{site.data.keyword.SecureGatewayfull}}
* {{site.data.keyword.appid_full_notm}}

빠른 개발은 HR 책임자가 가장 중요시하는 사항입니다. 팀은 앱을 컨테이너화하고 이를 클라우드에 배치하는 것으로 작업을 시작합니다. 최신 컨테이너를 사용하여, 개발자들은 별도의 클러스터에서 스케일 확장된 개발 및 테스트 시스템으로 변경사항을 푸시하며 Node.js SDK로 손쉽게 실험을 수행할 수 있습니다. 이러한 푸시는 공개 도구 체인 및 {{site.data.keyword.contdelivery_full}}를 통해 자동화됩니다. HR 사이트 업데이트는 더 이상 느리고 오류에 취약한 빌드 프로세스에서 정체되지 않습니다. 회사는 사이트에 대한 증분 업데이트를 매일, 또는 그보다 더 빈번하게 제공할 수 있습니다.  HR 사이트에 대한 로깅 및 모니터링(특히 사이트가 백엔드 혜택 시스템에서 개인화된 데이터를 가져오는 방법에 대해) 또한 신속히 통합됩니다. 개발자는 라이브 시스템의 문제를 해결할 수 있도록 복잡한 로깅 시스템을 빌드하는 데 시간을 낭비할 필요가 없습니다. 개발자는 클라우드 보안의 전문가가 되는 데 시간을 소비할 필요가 없으며 {{site.data.keyword.appid_full_notm}}를 사용하여 쉽게 정책 기반 인증을 적용할 수 있습니다.

회사는 {{site.data.keyword.containerlong_notm}}를 사용하여, 환경을 사내 데이터 센터에 있는 지나치게 높은 스펙의 하드웨어로부터 IT 운영, 유지보수 및 에너지 비용을 줄여주는, 사용자 정의할 수 있는 컴퓨팅 인프라로 옮깁니다. 회사는 HR 사이트를 호스팅하기 위해 CPU, RAM 및 스토리지 요구사항에 부합하는 Kubernetes 클러스터를 쉽게 디자인할 수 있습니다. 인건비 절감의 또 다른 요인은 개발자가 직원에게 더 나은 혜택 등록 경험을 제공하는 데 집중할 수 있도록 IBM이 Kubernetes를 관리하는 것입니다.

{{site.data.keyword.containerlong_notm}}는 앱 및 서비스를 요청 시 작성, 스케일링 및 제거할 수 있도록 확장 가능한 컴퓨팅 리소스 및 연관 DevOps 대시보드를 제공합니다. 업계 표준 컨테이너 기술을 사용하면 앱을 신속히 개발하여 여러 개발, 테스트 및 프로덕션 환경 간에 공유할 수 있습니다. 이 구성은 확장성에 대해 즉각적인 이점을 제공합니다. HR 팀은 Kubernetes의 풍부한 배치 및 런타임 오브젝트 세트를 사용하여 앱에 대한 업그레이드를 안정적으로 모니터링하고 관리할 수 있습니다. 또한 정의된 규칙 및 자동화된 Kubernetes 오케스트레이터를 사용하여 앱을 복제하고 스케일링할 수도 있습니다.

**1단계: 컨테이너, 마이크로서비스 및 Garage Method**
* 앱이 {{site.data.keyword.containerlong_notm}} 내에서 실행되는 협업 마이크로서비스의 세트로 빌드됩니다. 이 아키텍처는 품질 문제점이 가장 큰 앱의 기능 영역을 나타냅니다.
* IBM Vulnerability Advisor로 지속적으로 스캔되는 {{site.data.keyword.containerlong_notm}}의 컨테이너에 앱을 배치합니다.
* Kubernetes를 통해 표준화된 DevOps 대시보드를 제공합니다.
* IBM Garage Method의 반복적인 Agile 개발 방법을 채택하여 작동 중단 시간이 없는 새 기능, 패치 및 수정사항 릴리스를 가능하게 합니다.

**2단계: 기존 혜택 백엔드에 대한 연결**
* {{site.data.keyword.SecureGatewayfull}}는 혜택 시스템을 호스팅하는 온프레미스 시스템으로의 안전한 경로를 작성하는 데 사용됩니다.  
* 온프레미스 데이터와 {{site.data.keyword.containerlong_notm}}의 조합은 회사가 민감한 데이터에 액세스하는 동시에 규제를 준수할 수 있도록 해 줍니다.
* 챗봇 대화는 다시 HR 정책에 반영되어, 성적이 좋지 않은 햬택에 대한 맞춤형 개선을 포함, 혜택 사이트가 가장 인기있는 혜택과 인기없는 혜택을 반영할 수 있도록 합니다.

**3단계: 챗봇과 개인화**
* IBM Watson Assistant는 사용자에게 올바른 혜택 정보를 제공할 수 있는 챗봇을 신속하게 작성하는 데 필요한 도구를 제공합니다.
* Watson Tone Analyzer는 챗봇 대화에서 고객의 만족도를 높이며, 필요한 경우 사람이 개입할 수 있도록 합니다.

**4단계: 전 세계에서의 지속적 제공**
* {{site.data.keyword.contdelivery_full}}는 개발자가 사용자 정의할 수 있으며 공유 가능한 템플리트를 IBM, 서드파티 및 오픈 소스에서 제공하는 도구와 함께 사용하여 통합 도구 체인을 신속하게 제공할 수 있도록 도와줍니다. 개발자는 분석으로 품질을 제어하면서 빌드와 테스트를 자동화합니다.
* 개발자는 개발 및 테스트 클러스터에 앱을 빌드하고 테스트한 후, IBM CI/CD 도구 체인을 사용하여 전 세계의 프로덕션 클러스터에 앱을 배치합니다.
* {{site.data.keyword.containerlong_notm}}는 앱의 손쉬운 롤아웃 및 롤백 기능을 제공합니다. Istio의 지능형 라우팅 및 로드 밸런싱 기능을 통해, 맞춤 작성된 앱이 지역 요구사항을 만족시키기 위해 배치됩니다.
* {{site.data.keyword.containerlong_notm}}의 기본 제공 HA 도구는 자가 수리 및 로드 밸런싱을 포함, 각 지역 내 워크로드의 균형을 맞춥니다.

**결과**
* 챗봇과 같은 도구를 통해, HR 팀은 직원들에게 거창한 말 뿐만 아니라 혁신 또한 회사 문화의 일부임을 증명합니다.
* 개인화를 동반한 사이트의 신뢰성은 현재 항공사 직원들의 변화하는 기대에 부응합니다.
* 개발자들이 매일 10회 이상 변경사항을 푸시함으로써 HR 사이트에 대한 최신 업데이트(직원과 챗봇 간의 대화로 인한 것 포함)가 빠르게 공개됩니다.
* IBM에서 인프라 관리를 책임짐으로써, 개발 팀은 3주일 안에 사이트를 제공할 여유가 생깁니다.
