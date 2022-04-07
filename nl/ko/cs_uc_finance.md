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


# {{site.data.keyword.cloud_notm}}의 금융 서비스 유스 케이스
{: #cs_uc_finance}

다음 유스 케이스는 {{site.data.keyword.containerlong_notm}}의
워크로드가 고가용성, 고성능 컴퓨팅, 빠른 배치를 위한 손쉬운 클러스터 스핀업,
{{site.data.keyword.ibmwatson}}의 AI를 어떻게 활용하는지 보여줍니다.
{: shortdesc}

## 모기지 회사가 비용을 절감하고 규제 준수를 가속화함
{: #uc_mortgage}

어떤 주택 모기지 회사의 위험 관리 VP가 하루에 7천만 개의 레코드를 처리하지만, 온프레미스 시스템이 느릴 뿐만 아니라 정확하지 않습니다. 하드웨어가 빠르게 시대에 뒤쳐지는 동시에 완전히 활용되지 않아 IT 비용이 급상승했습니다. 하드웨어가 정비되기를 기다리는 동안 규제 준수 작업이 느려졌습니다.
{: shortdesc}

{{site.data.keyword.Bluemix_notm}}를 사용해야 하는 이유: 위험 분석 능력을 향상시키기 위해, 회사는 {{site.data.keyword.containerlong_notm}} 및 IBM Cloud Analytic 서비스를 사용하여 비용을 줄이고, 전 세계에서의 가용성을 향상시키고, 최종적으로는 규제 준수를 가속화하고자 합니다. 여러 지역에서 {{site.data.keyword.containerlong_notm}}를 사용하면 회사의 분석 앱을 컨테이너화하여 전 세계에 배치함으로써 가용성을 향상시키는 동시에 각 지역의 규제에 대응할 수 있습니다. 이러한 배치는 이미 {{site.data.keyword.containerlong_notm}}의 일부가 되어 있는 친숙한 오픈 소스 도구를 통해 가속화됩니다.

{{site.data.keyword.containerlong_notm}} 및 주요 기술:
* [수평적 확장](/docs/containers?topic=containers-app#highly_available_apps)
* [고가용성을 위한 여러 지역](/docs/containers?topic=containers-regions-and-zones#regions-and-zones)
* [다양한 CPU, RAM, 스토리지 요구사항을 만족시키는 클러스터](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)
* [컨테이너 보안 및 격리](/docs/containers?topic=containers-security#security)
* [앱 간에 데이터를 지속시키고 동기화하는 데 필요한 {{site.data.keyword.cloudant}}](/docs/services/Cloudant?topic=cloudant-getting-started#getting-started)
* [SDK for Node.js](/docs/runtimes/nodejs?topic=Nodejs-nodejs_runtime#nodejs_runtime)

**솔루션**

회사는 분석 앱을 컨테이너화하고 이를 클라우드에 배치하는 것으로 작업을 시작합니다. 하드웨어로 인한 문제가 순식간에 사라집니다. 회사는 고성능 CPU, RAM, 스토리지 및 보안 요구사항에 부합하는 Kubernetes 클러스터를 쉽게 디자인할 수 있습니다. 분석 앱이 변경되는 경우에는 큰 하드웨어 투자 없이 컴퓨팅 인프라만 추가하거나 줄일 수 있습니다. {{site.data.keyword.containerlong_notm}} 수평적 확장을 통해, 앱은 레코드가 증가함에 따라 이에 비례하여 스케일링되며 따라서 규제 보고서가 더 빠르게 작성됩니다. {{site.data.keyword.containerlong_notm}}는 현재 컴퓨팅 리소스가 사용되는 모든 부문에서 강력한 보안 기능과 고성능을 제공하는 유연한 컴퓨팅 리소스를 전 세계에서 제공합니다.

이제 회사의 앱은 {{site.data.keyword.cloudant}}의 데이터 웨어하우스로부터 큰 볼륨의 데이터를 수신합니다. {{site.data.keyword.cloudant}}의 클라우드 기반 스토리지는 온프레미스 시스템에 갇혀 있었을 때보다 높은 가용성을 보장합니다. 가용성은 필수이므로 앱은 전 세계의 데이터 센터에 배치됩니다. 이는 재해 복구 및 대기 시간을 위해서이기도 합니다.

위험 분석 및 규제 준수 또한 가속화됩니다. 몬테카를로 계산과 같은 예측 및 위험 분석 기능은 이제 반복적인 Agile 배치를 통해 지속적으로 업데이트됩니다. 운영 비용 또한 절감될 수 있도록 컨테이너 오케스트레이션은 관리되는 Kubernetes에 의해 처리됩니다. 최종적으로, 모기지에 대한 위험 분석은 빠르게 변화하는 시장에 더 기민하게 반응합니다.

**컨텍스트: 주택 모기지에 대한 규제 준수 및 금융 모델링**

* 더 나은 금융 위험 관리 능력에 대한 필요성이 커지면서 규제 감독이 증가하였습니다. 동일한 필요성으로 인해 위험 평가 프로세스에서의 연관 검토, 그리고 더 빈번하고 자세하며 통합된 규제 보고 공개 또한 필요하게 되었습니다.
* 고성능 컴퓨팅 그리드는 금융 모델링에 있어서 중요한 인프라 컴포넌트입니다.

현재 회사의 문제는 스케일과 제공 소요 시간입니다.

회사의 현재 온프레미스 환경은 도입한지 7년 이상 지났으며 제한된 컴퓨팅, 스토리지 및 I/O 능력을 갖고 있습니다.
서버 일신에는 큰 비용이 필요하며 완료하는 데 오랜 시간이 소요됩니다.
소프트웨어 및 앱 업데이트가 비공식 프로세스를 따르며 반복 가능하지 않습니다.
실제 HPC 그리드가 프로그래밍하기 어렵습니다. API가 새로 참여하는 개발자가 이해하기에 너무 복잡하며 문서화되지 않은 지식을 필요로 합니다.
주요 앱 업그레이드를 완료하는 데 6 - 9개월이 소요됩니다.

**솔루션 모델: 필요한 경우 온프레미스 엔터프라이즈 자산에 안전하게 액세스할 수 있는, 퍼블릭 클라우드에서 실행되는 요청 시 컴퓨팅, 스토리지 및 I/O 서비스**

* 구조화된 문서 조회 및 구조화되지 않은 문서 조회를 지원하는 안전하며 확장 가능한 문서 스토리지
* 기존 엔터프라이즈 자산 및 앱이 마이그레이션되지 않을 일부 온프레미스 시스템과의 통합을 가능하게 하는 상태에서 이들을 "리프트 앤 시프트(lift and shift)" 방식으로 이동
* 솔루션 배치 소요 시간을 줄이고 보고 정확성에 영향을 주는 버그를 처리하기 위한 표준 DevOps 및 모니터링 프로세스 구현

**자세한 솔루션**

* {{site.data.keyword.containerlong_notm}}
* {{site.data.keyword.cos_full_notm}}
* {{site.data.keyword.sqlquery_notm}}(Spark)
* {{site.data.keyword.cloudant}}
* {{site.data.keyword.SecureGateway}}

{{site.data.keyword.containerlong_notm}}는 앱 및 서비스를 요청 시 작성, 스케일링 및 제거할 수 있도록 확장 가능한 컴퓨팅 리소스 및 연관 DevOps 대시보드를 제공합니다. 업계 표준 컨테이너를 사용하여, 처음에 앱을 주요 아키텍처 변경 없이 빠르게 {{site.data.keyword.containerlong_notm}}에서 다시 호스팅할 수 있습니다.

이 솔루션은 확장성에 대해 즉각적인 이점을 제공합니다. 모기지 회사는 Kubernetes의 풍부한 배치 및 런타임 오브젝트 세트를 사용하여 앱에 대한 업그레이드를 안정적으로 모니터링하고 관리합니다. 또한 정의된 규칙 및 자동화된 Kubernetes 오케스트레이터를 사용하여 앱을 복제하고 스케일링할 수도 있습니다.

{{site.data.keyword.SecureGateway}}는 {{site.data.keyword.containerlong_notm}}에서 실행되도록 다시 호스팅된 앱을 위해 온프레미스 데이터베이스 및 문서에 대한 안전한 파이프라인을 작성하는 데 사용됩니다.

{{site.data.keyword.cos_full_notm}}는 모든 원시 문서 및 데이터의 저장을 위한 스토리지입니다. 몬테카를로 시뮬레이션의 경우 워크플로우 파이프라인은 {{site.data.keyword.cos_full_notm}}에 저장된 구조화된 파일에서 시뮬레이션 데이터가 있는 위치에 배치됩니다. 시뮬레이션을 시작하는 트리거는 시뮬레이션 처리를 위해 파일의 데이터를 N개의 이벤트 버킷으로 분할하기 위해 {{site.data.keyword.containerlong_notm}}의 컴퓨팅 서비스를 스케일링합니다. {{site.data.keyword.containerlong_notm}}는 자동으로 N개의 연관된 서비스 실행으로 스케일링되며 중간 결과를 {{site.data.keyword.cos_full_notm}}에 기록합니다. 이러한 결과는 최종 결과를 생성하기 위해 다른 {{site.data.keyword.containerlong_notm}} 컴퓨팅 서비스 세트에 의해 처리됩니다.

{{site.data.keyword.cloudant}}는 키-값부터 복잡한 문서 지향 데이터 저장 및 조회에 이르기까지, 많은 데이터 중심 유스 케이스에 유용한 최신 NoSQL 데이터베이스입니다. 늘어나는 규제 및 관리 보고서 규칙 세트를 관리하기 위해, 모기지 회사는 {{site.data.keyword.cloudant}}를 사용하여 회사로 들어오는 원시 규제 데이터와 연관된 문서를 저장합니다. {{site.data.keyword.containerlong_notm}}의 컴퓨팅 프로세스는 다양한 보고 형식으로 데이터를 컴파일하고, 처리하고, 공개하기 위해 트리거됩니다. 보고서 간에 공통되는 중간 결과는 템플리트 중심 프로세스를 사용하여 필요한 보고서를 생성할 수 있도록 {{site.data.keyword.cloudant}} 문서에 저장됩니다.

**결과**

* 복잡한 금융 시뮬레이션이 이전에 기존 온프레미스 시스템으로 완료할 수 있었던 시간의 25% 내에 완료됩니다.
* 배치 소요 시간이 이전의 6 - 9개월에서 평균 1 - 3주일로 개선됩니다. 이는 {{site.data.keyword.containerlong_notm}}가 앱 컨테이너를 늘리고 새 버전으로 대체하는 데 있어서 세련되고 제어된 프로세스를 사용할 수 있도록 하기 때문입니다. 버그를 보고하면 정확성과 같은 문제를 빠르게 수정하고 해결할 수 있습니다.
* {{site.data.keyword.containerlong_notm}} 및 {{site.data.keyword.cloudant}}에서 제공하는 지속적이며 확장 가능한 스토리지 및 컴퓨팅 서비스 세트를 통해 규제 준수 보고 비용이 줄어듭니다.
* 시간이 경과함에 따라, 처음에 "리프트 앤 시프트(lift and shift)" 방식으로 클라우드로 이동된 원래 앱은 {{site.data.keyword.containerlong_notm}}에서 실행되는 협업 마이크로서비스로 다시 구성됩니다. 이 조치는 개발 및 배치 소요 시간을 더욱 가속화하며, 실험의 상대적 용이성으로 인해 더 많은 혁신이 이뤄질 수 있도록 합니다. 또한 회사에서는 시장 및 비즈니스 상태를 활용하기 위해 새 마이크로서비스 버전을 포함하는 혁신적인 앱을 릴리스합니다(소위 상황 적응형 앱 및 마이크로서비스).

## 지불 기술 회사가 개발자 생산성을 합리화하고 해당 파트너에게 4배 빠르게 AI 사용 도구를 배치함
{: #uc_payment_tech}

어떤 개발 책임자가 하드웨어 조달을 기다리는 동안 프로토타입 작성 속도를 저하시키는 기존 온프레미스 도구를 사용하는 개발자들을 거느리고 있습니다.
{: shortdesc}

{{site.data.keyword.Bluemix_notm}}를 사용해야 하는 이유: {{site.data.keyword.containerlong_notm}}는 오픈 소스 표준 기술을 사용하여 컴퓨팅 인프라의 스핀업을 제공합니다. 회사가 {{site.data.keyword.containerlong_notm}}를 사용하기 시작하면 개발자들은 이동 가능하며 쉽게 공유할 수 있는 컨테이너와 같은 DevOps 친화 도구에 액세스할 수 있습니다.

이렇게 되면 개발자들은 공개 도구 체인을 사용하여 개발 및 테스트 시스템으로 변경사항을 신속히 푸시하며 손쉽게 실험을 수행할 수 있게 됩니다. 한 번의 클릭으로 앱에 AI 클라우드 서비스를 추가하면 기존 소프트웨어 개발 도구가 새로운 모습으로 변신합니다.

주요 기술:
* [다양한 CPU, RAM, 스토리지 요구사항을 만족시키는 클러스터](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)
* [{{site.data.keyword.watson}} AI를 사용한 사기 방지](https://www.ibm.com/cloud/watson-studio)
* [{{site.data.keyword.contdelivery_full}}의 공개 도구 체인을 포함한 DevOps 기본 도구](https://www.ibm.com/cloud/garage/toolchains/)
* [SDK for Node.js](/docs/runtimes/nodejs?topic=Nodejs-nodejs_runtime#nodejs_runtime)
* [{{site.data.keyword.appid_short_notm}}를 사용하여 앱 코드를 변경하지 않는 사인온 기능](/docs/services/appid?topic=appid-getting-started)

**컨텍스트: 개발자 생산성 합리화 및 파트너에게 4배 빠르게 AI 도구 배치**

* 소비자 및 B2B 부문 둘 다에서 극적인 성장세를 보이고 있는 지불 업계에서는 사기 행위가 만연하고 있습니다. 그러나 지불 도구에 대한 업데이트는 느립니다.
* 새롭고 더 빠른 방법으로 사기 트랜잭션을 찾고 방지하려면 코그너티브 기능이 필요합니다.
* 파트너 수가 증가하고 이들과의 트랜잭션이 늘어남에 따라 도구 트래픽이 증가하지만, 리소스의 효율성을 최대화함으로써 인프라 예산은 줄여야 합니다.
* 회사가 시장의 요구에 부합하는 고품질 소프트웨어를 릴리스하는 데 실패하여 기술 관련 부채가 줄어들지 않고 증가하고 있습니다.
* 자금 지출 예산에는 여유가 없는 상태이며, IT 부서는 사내 시스템으로 테스트 및 스테이징 환경을 작성할 예산이나 인력이 없다고 판단하고 있습니다.
* 보안은 점점 주된 문제가 되어가고 있으며, 이 문제는 제공 부담에 가중되어 함께 지연 시간을 늘리고 있습니다.

**솔루션**

개발 책임자는 역동적인 지불 업계에서 많은 도전에 직면해 있습니다. 규제, 소비자 행동, 사기, 경쟁사 및 시장 인프라 모두 빠르게 변화하고 있습니다. 미래의 지불 업계에서 한 부분을 차지하려면 빠른 개발이 필수적입니다.

이 회사의 비즈니스 모델은 금융 기관 및 기타 조직이 강력한 보안을 갖춘 디지털 지불 경험을 제공하는 데 도움을 줄 수 있도록 이러한 비즈니스 파트너에게 지불 도구를 제공하는 것입니다.

이 회사는 개발자와 해당 비즈니스 파트너에게 도움을 주는 다음과 같은 솔루션을 필요로 합니다.
* 지불 도구의 프론트 엔드: 요금 시스템, 지불 추적(국외 지불 포함), 규제 준수, 생체인식, 송금 등
* 규제별 기능: 각 국가에는 고유한 규제가 있으므로 전체적인 도구 세트의 모습은 유사할 수 있으나 국가별 이점을 포함함
* 기능의 롤아웃 및 버그 수정을 가속화하는 개발자 친화적인 도구
* FDaaS(Fraud Detection as a Service)는 {{site.data.keyword.watson}} AI를 사용하여 증가하고 있는 빈번한 사기 행위에 한 발 앞서 대응함

**솔루션 모델**

백엔드 지불 시스템에 액세스할 수 있는, 퍼블릭 클라우드에서 실행되는 요청 시 컴퓨팅, DevOps 도구 및 AI. 제공 주기를 획기적으로 줄이기 위해 CI/CD 프로세스를 구현합니다.

기술적 솔루션:
* {{site.data.keyword.containerlong_notm}}
* {{site.data.keyword.contdelivery_full}}
* IBM Cloud Logging and Monitoring
* {{site.data.keyword.ibmwatson_notm}} for Financial Services
* {{site.data.keyword.appid_full_notm}}

회사는 지불 도구 VM을 컨테이너화하고 이를 클라우드에 배치하는 것으로 작업을 시작합니다. 하드웨어로 인한 문제가 순식간에 사라집니다. 회사는 CPU, RAM, 스토리지 및 보안 요구사항에 부합하는 Kubernetes 클러스터를 쉽게 디자인할 수 있습니다. 지불 도구를 변경해야 하는 경우에는 비싸고 느린 하드웨어를 구매할 필요 없이 컴퓨팅 인프라만 추가하거나 줄일 수 있습니다.

{{site.data.keyword.containerlong_notm}} 수평적 확장을 통해, 앱은 파트너가 증가함에 따라 이에 비례하여 스케일링되며 따라서 더 빠르게 성장합니다. {{site.data.keyword.containerlong_notm}}는 현재 컴퓨팅 리소스가 사용되는 모든 부문에서 강력한 보안 기능을 제공하는 유연한 컴퓨팅 리소스를 전 세계에서 제공합니다.

빠른 개발은 책임자가 가장 중요시하는 사항입니다. 최신 컨테이너를 사용하여, 개발자들은 별도의 클러스터에서 스케일 확장된 개발 및 테스트 시스템으로 변경사항을 푸시하며 원하는 언어로 손쉽게 실험을 수행할 수 있습니다. 이러한 푸시는 공개 도구 체인 및 {{site.data.keyword.contdelivery_full}}를 통해 자동화됩니다. 도구 업데이트는 더 이상 느리고 오류에 취약한 빌드 프로세스에서 정체되지 않습니다. 회사는 도구에 대한 증분 업데이트를 매일, 또는 그보다 더 빈번하게 제공할 수 있습니다.

도구에 대한 로깅 및 모니터링(특히 {{site.data.keyword.watson}} AI를 사용한 부분) 또한 시스템에 빠르게 통합됩니다. 개발자는 라이브 시스템의 문제를 해결할 수 있도록 복잡한 로깅 시스템을 빌드하는 데 시간을 낭비할 필요가 없습니다. 인건비 절감의 주된 요인은 개발자가 더 나은 지불 도구에 집중할 수 있도록 IBM이 Kubernetes를 관리하는 것입니다.

보안 우선: {{site.data.keyword.containerlong_notm}}에 베어메탈을 사용함으로써, 민감한 지불 도구는 이전과 마찬가지로 격리되는 동시에 퍼블릭 클라우드를 통한 유연함을 갖추게 됩니다. 베어메탈은 기반 하드웨어의 변조 여부를 확인할 수 있는, 신뢰할 수 있는 컴퓨팅을 제공합니다. 취약성 및 악성코드에 대한 스캔이 지속적으로 실행됩니다.

**1단계: 안전한 컴퓨팅 인프라로 리프트 앤 시프트(lift and shift)**
* 신뢰할 수 있는 컴퓨팅을 위해 베어메탈에서 실행되는 {{site.data.keyword.containerlong_notm}}로 매우 민감한 데이터를 관리하는 앱을 다시 호스팅할 수 있습니다. 신뢰할 수 있는 컴퓨팅은 기반 하드웨어의 변조 여부를 확인할 수 있습니다.
* 가상 머신 이미지를 퍼블릭 {{site.data.keyword.Bluemix_notm}}의 {{site.data.keyword.containerlong_notm}}에서 실행되는 컨테이너 이미지로 마이그레이션합니다.
* 해당 코어에서 Vulnerability Advisor는 알려진 악성코드에 대한 이미지, 정책, 컨테이너 및 패키징 취약성 스캔을 제공합니다.
* 사내 데이터 센터 / 온프레미스 자본 비용이 크게 줄어들며 워크로드 수요에 따라 스케일링되는 실용적 컴퓨팅 모델로 대체됩니다.
* 간단한 Ingress 어노테이션을 사용하여 정책 기반 인증을 서비스와 API에 일관되게 적용합니다. 선언적 보안으로 {{site.data.keyword.appid_short_notm}}를 사용하여 사용자 인증 및 토큰 유효성 검증을 보장할 수 있습니다.

**2단계: 기존 지불 시스템 백엔드에 대한 오퍼레이션 및 연결**
* IBM {{site.data.keyword.SecureGateway}}를 사용하여 온프레미스 도구 시스템과 안전한 연결을 유지합니다.
* Kubernetes를 통해 표준화된 DevOps 대시보드 및 방법을 제공합니다.
* 개발자는 개발 및 테스트 클러스터에 앱을 빌드하고 테스트한 후, {{site.data.keyword.contdelivery_full}} 도구 체인을 사용하여 전 세계의 {{site.data.keyword.containerlong_notm}} 클러스터에 앱을 배치합니다.
* {{site.data.keyword.containerlong_notm}}의 기본 제공 HA 도구는 자가 수리 및 로드 밸런싱을 포함, 각 지역 내 워크로드의 균형을 맞춥니다.

**3단계: 사기 분석 및 방지**
* IBM {{site.data.keyword.watson}} for Financial Services를 배치하여 사기를 발견하고 방지합니다.
* 앱은 도구 체인 및 Helm 배치 도구를 사용하여 전 세계의 {{site.data.keyword.containerlong_notm}} 클러스터에도 배치됩니다. 이렇게 하면 워크로드 및 데이터가 지역 규제에 부합하게 됩니다.

**결과**
* 개발 책임자는 가장 먼저 기존 모놀리식 VM을 클라우드에서 호스팅되는 컨테이너로 이동시킴으로써 자본 및 운영 비용을 절약합니다.
* IBM에서 인프라 관리를 책임짐으로써, 개발 팀은 매일 10번 업데이트를 제공할 여유가 생깁니다.
* 동시에, 제공자는 기존 기술 관련 부채를 관리하기 위해 시간이 제한된 단순 반복을 구현합니다.
* 회사는 처리하는 트랜잭션 수에 따라 운영을 기하급수적으로 스케일링할 수 있습니다.
* 동시에, {{site.data.keyword.watson}}을 사용한 새 사기 분석이 사기를 발견하고 방지하는 속도를 증가시켜 지역 평균보다 4배 많이 사기를 줄입니다.
