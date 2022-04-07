---

copyright:
  years: 2014, 2019
lastupdated: "2019-04-09"

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


# 서비스 통합
{: #integrations}

{{site.data.keyword.containerlong}}의 표준 Kubernetes 클러스터에서 다양한 외부 서비스와 카탈로그 서비스를 사용할 수 있습니다.
{:shortdesc}


## DevOps 서비스
{: #devops_services}
<table summary="이 표에는 클러스터에 추가하여 추가 DevOps 기능을 추가할 수 있는 사용 가능한 서비스가 표시됩니다. 행은 왼쪽에서 오른쪽으로 읽어야 하며, 1열에는 서비스 이름, 2열에는 서비스의 설명이 있습니다.">
<caption>DevOps 서비스</caption>
<thead>
<tr>
<th>서비스</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.cfee_full_notm}}</td>
<td>클라우드 고유 앱을 개발, 패키지, 배치 및 관리하고 {{site.data.keyword.Bluemix_notm}} 에코시스템을 활용하여 앱에 추가 서비스를 바인딩할 수 있도록 Kubernetes 클러스터 맨 위에 자체 Cloud Foundry 플랫폼을 배치하고 관리합니다. {{site.data.keyword.cfee_full_notm}} 인스턴스를 작성할 때 작업자 노드에 대한 시스템 유형과 VLAN을 선택하여 Kubernetes 클러스터를 구성해야 합니다. 그런 다음 {{site.data.keyword.containerlong_notm}} 및 {{site.data.keyword.cfee_full_notm}}와 함께 클러스터가 사용자의 클러스터에 자동으로 배치됩니다. {{site.data.keyword.cfee_full_notm}} 설정 방법에 대한 자세한 정보는 [시작하기 튜토리얼](/docs/cloud-foundry?topic=cloud-foundry-getting-started#getting-started)을 참조하십시오. </td>
</tr>
<tr>
<td>Codeship</td>
<td>컨테이너의 Continuous Integration 및 Continuous Delivery를 위해 <a href="https://codeship.com" target="_blank">Codeship <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>을 사용할 수 있습니다. 자세한 정보는 <a href="https://www.ibm.com/blogs/bluemix/2017/10/using-codeship-pro-deploy-workloads-ibm-container-service/" target="_blank">Codeship Pro를 사용하여 {{site.data.keyword.containerlong_notm}}에 워크로드 배치 <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>를 참조하십시오. </td>
</tr>
<tr>
<td>Grafeas</td>
<td>[Grafeas ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://grafeas.io)는 소프트웨어 공급 체인 프로세스 동안 메타데이터를 검색, 저장 및 교환하는 일반적인 방법을 제공하는 오픈 소스 CI/CD 서비스입니다. 예를 들어 Grafeas를 앱 빌드 프로세스에 통합하는 경우 Grafeas는 앱이 프로덕션에 배포될 때 정보에 입각한 결정을 내릴 수 있도록 빌드 요청의 개시자, 취약성 스캔 결과 및 품질 보증 사인오프에 대한 정보를 저장할 수 있습니다. 감사에서 이 메타데이터를 사용하거나 소프트웨어 공급 체인에 대한 규제 준수를 증명할 수 있습니다. </td>
</tr>
<tr>
<td>Helm</td>
<td> <a href="https://helm.sh" target="_blank">Helm <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>은 Kubernetes 패키지 관리자입니다. {{site.data.keyword.containerlong_notm}} 클러스터에서 실행되는 복잡한 Kubernetes 애플리케이션을 정의, 설치 및 업그레이드하기 위해 새 Helm 차트를 작성하거나 기존 Helm 차트를 사용할 수 있습니다. <p>자세한 정보는 [{{site.data.keyword.containerlong_notm}}에서 Helm 설정](/docs/containers?topic=containers-integrations#helm)을 참조하십시오.</p></td>
</tr>
<tr>
<td>{{site.data.keyword.contdelivery_full}}</td>
<td>도구 체인을 사용하여 Kubernetes 클러스터에 대한 컨테이너 배치 및 앱 빌드를 자동화합니다. 설정 정보는 <a href="https://developer.ibm.com/recipes/tutorials/deploy-kubernetes-pods-to-the-bluemix-container-service-using-devops-pipelines/" target="_blank">DevOps 파이프라인을 사용하여 {{site.data.keyword.containerlong_notm}}에 Kubernetes 팟(Pod) 배치 <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a> 블로그를 참조하십시오. </td>
</tr>
<tr>
<td>Istio on {{site.data.keyword.containerlong_notm}}</td>
<td><a href="https://www.ibm.com/cloud/info/istio" target="_blank">Istio <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>는 클라우드 오케스트레이션 플랫폼에서 서비스 메시(service mesh)로도 알려진 마이크로서비스의 네트워크에 연결하고, 보안, 관리 및 모니터링하기 위한 방법을 개발자에게 제공하는 오픈 소스 서비스입니다. Istio on {{site.data.keyword.containerlong}}는 관리 추가 기능을 통해 클러스터에 Istio의 1단계 설치를 제공합니다. 한 번의 클릭으로 Istio 핵심 컴포넌트, 추가 추적, 모니터링 및 시각화를 모두 확보할 수 있고 BookInfo 샘플 앱을 시작하고 실행할 수 있습니다. 시작하려면 [관리 Istio 추가 기능 사용(베타)](/docs/containers?topic=containers-istio)을 참조하십시오.</td>
</tr>
<tr>
<td>Jenkins X</td>
<td>Jenkins X는 빌드 프로세스를 자동화하는 데 사용할 수 있는 Kubernetes 고유 Continuous Integration 및 Continuous Delivery 플랫폼입니다. {{site.data.keyword.containerlong_notm}}에 Jenkins X를 설치하는 방법에 대한 자세한 정보는 [Jenkins X 오픈 소스 프로젝트 소개 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/blogs/bluemix/2018/08/installing-jenkins-x-on-ibm-cloud-kubernetes-service/)를 참조하십시오.</td>
</tr>
<tr>
<td>Knative</td>
<td>[Knative ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/knative/docs)는 Kubernetes 클러스터 맨 위에 최신의, 소스 중심으로 컨테이너화된 서버리스 앱을 작성할 수 있도록 Kubernetes의 기능을 확장한다는 목표로 IBM, Google, Pivotal, Red Hat, Cisco 등에서 개발한 오픈 소스 플랫폼입니다. 이 플랫폼은 프로그래밍 언어와 프레임워크 전반에서 일관된 접근 방식을 사용하여 개발자가 가장 중요한 것(소스 코드)에 집중할 수 있도록 Kubernetes에 워크로드를 빌드, 배치 및 관리하는 데 따르는 운영 부담을 추상화합니다. 자세한 정보는 [튜토리얼: 관리 Knative를 사용하여 Kubernetes 클러스터의 서버리스 앱 실행](/docs/containers?topic=containers-knative_tutorial#knative_tutorial)을 참조하십시오. </td>
</tr>
</tbody>
</table>

<br />



## 서비스 로깅 및 모니터링
{: #health_services}
<table summary="이 표에는 클러스터에 추가하여 추가 로깅 및 모니터링 기능을 추가할 수 있는 사용 가능한 서비스가 표시됩니다. 행은 왼쪽에서 오른쪽으로 읽어야 하며, 1열에는 서비스 이름, 2열에는 서비스의 설명이 있습니다.">
<caption>서비스 로깅 및 모니터링</caption>
<thead>
<tr>
<th>서비스</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>CoScale</td>
<td><a href="https://www.newrelic.com/coscale" target="_blank">CoScale <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>을 사용하여 작업자 노드, 컨테이너, 복제본 세트 및 서비스를 모니터링합니다. 자세한 정보는 <a href="https://www.ibm.com/blogs/bluemix/2017/06/monitoring-ibm-bluemix-container-service-coscale/" target="_blank">CoScale을 사용하여 {{site.data.keyword.containerlong_notm}} 모니터링 <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>을 참조하십시오. </td>
</tr>
<tr>
<td>Datadog</td>
<td><a href="https://www.datadoghq.com/" target="_blank">Datadog <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>를 사용하여 클러스터를 모니터링하고 인프라 및 애플리케이션 성능 메트릭을 봅니다. 자세한 정보는 <a href="https://www.ibm.com/blogs/bluemix/2017/07/monitoring-ibm-bluemix-container-service-datadog/" target="_blank">Datadog를 사용하여 {{site.data.keyword.containerlong_notm}} 모니터링 <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>을 참조하십시오. </td>
</tr>
<tr>
<td>{{site.data.keyword.cloudaccesstrailfull}}</td>
<td>Grafana를 통해 로그를 분석하여 클러스터에서 작성된 관리 활동을 모니터링합니다. 서비스에 대한 자세한 정보는 [활동 트래커](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started-with-cla) 문서를 참조하십시오. 추적할 수 있는 이벤트의 유형에 대한 자세한 정보는 [활동 트래커 이벤트](/docs/containers?topic=containers-at_events)를 참조하십시오.</td>
</tr>
<tr>
<td>{{site.data.keyword.la_full_notm}}</td>
<td>팟(Pod) 컨테이너에서 로그를 관리하기 위해 LogDNA를 작업자 노드에 서드파티 서비스로 배치하여 로그 관리 기능을 클러스터에 추가합니다. 자세한 정보는 [LogDNA로 {{site.data.keyword.loganalysisfull_notm}}에서 Kubernetes 클러스터 로그 관리](/docs/services/Log-Analysis-with-LogDNA/tutorials?topic=LogDNA-kube#kube)를 참조하십시오.</td>
</tr>
<tr>
<td>{{site.data.keyword.mon_full_notm}}</td>
<td>메트릭을 {{site.data.keyword.monitoringlong}}에 전달하기 위해 Sysdig를 작업자 노드에 서드파티의 서비스로 배치하여 앱의 성능과 상태에 대한 작동 가시성을 얻을 수 있습니다. 자세한 정보는 [Kubernetes 클러스터에서 앱에 대한 메트릭 분석](/docs/services/Monitoring-with-Sysdig/tutorials?topic=Sysdig-kubernetes_cluster#kubernetes_cluster)을 참조하십시오. </td>
</tr>
<tr>
<td>Instana</td>
<td> <a href="https://www.instana.com/" target="_blank">Instana <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>는 인프라 및 앱 성능 모니터링에 자동으로 앱을 발견하고 맵핑하는 GUI를 제공합니다. Instana는 앱에 대한 모든 요청을 캡처하며 문제점을 해결하고 근본 원인 분석을 수행하여 문제점이 다시 발생하지 않도록 방지하는 데 사용할 수 있습니다. 자세히 알아보려면 <a href="https://www.instana.com/blog/precise-visibility-applications-ibm-bluemix-container-service/" target="_blank">{{site.data.keyword.containerlong_notm}}에 Instana 배치 <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>에 대한 블로그 게시물을 확인하십시오.</td>
</tr>
<tr>
<td>Prometheus</td>
<td>Prometheus는 특별히 Kubernetes용으로 디자인된 오픈 소스 모니터링, 로깅 및 경보 도구입니다. Prometheus는 Kubernetes 로깅 정보를 기반으로 클러스터, 작업자 노드 및 배치 상태에 대한 자세한 정보를 검색합니다. 클러스터에서 실행 중인 각 컨테이너에 대한 CPU, 메모리, I/O 및 네트워크 활동이 수집됩니다. 사용자 정의 조회 또는 경보에서 수집된 데이터를 사용하여 클러스터의 성능 및 워크로드를 모니터링할 수 있습니다.

<p>Prometheus를 사용하려면 <a href="https://github.com/coreos/prometheus-operator/tree/master/contrib/kube-prometheus" target="_blank">CoreOS 지시사항 <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>을 따르십시오.</p>
</td>
</tr>
<tr>
<td>Sematext</td>
<td><a href="https://sematext.com/" target="_blank">Sematext <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>를 사용하여 컨테이너화된 애플리케이션에 대한 메트릭 및 로그를 봅니다. 자세한 정보는 <a href="https://www.ibm.com/blogs/bluemix/2017/09/monitoring-logging-ibm-bluemix-container-service-sematext/" target="_blank">Sematext를 사용하여 컨테이너에 대해 모니터링 및 로깅 <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>을 참조하십시오. </td>
</tr>
<tr>
<td>Splunk</td>
<td>Splunk Connect for Kubernetes를 사용하여 Kubernetes 로깅, 오브젝트 및 메트릭 데이터를 가져오고 검색합니다. Splunk Connect for Kubernetes는 Splunk에서 지원하는 Fluentd 배치를 Kubernetes 클러스터에 배치하는 Helm 차트, 로그 및 메타데이터를 전송하는 Splunk-built Fluentd HTTP Event Collector(HEC) 플러그인, 클러스터 메트릭을 캡처하는 메트릭 배치 콜렉션입니다. 자세한 정보는 <a href="https://www.ibm.com/blogs/bluemix/2019/02/solving-business-problems-with-splunk-on-ibm-cloud-kubernetes-service/" target="_blank">Splunk를 사용하여 비즈니스 문제 해결 {{site.data.keyword.containerlong_notm}} <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>을 참조하십시오.</td>
</tr>
<tr>
<td>Weave Scope</td>
<td>Weave Scope는 서비스, 팟(Pod), 컨테이너, 프로세스, 노드 등을 포함하여 Kubernetes 클러스터 내의 리소스에 대한 시각적 다이어그램을 제공합니다. Weave Scope는 CPU 및 메모리에 대한 대화식 메트릭을 제공하며 컨테이너로 tail 및 exec를 실행하기 위한 도구도 제공합니다.<p>자세한 정보는 [Weave Scope 및 {{site.data.keyword.containerlong_notm}}를 사용하여 Kubernetes 클러스터 리소스 시각화](/docs/containers?topic=containers-integrations#weavescope)를 참조하십시오.</p></li></ol>
</td>
</tr>
</tbody>
</table>

<br />



## 보안 서비스
{: #security_services}

{{site.data.keyword.Bluemix_notm}} 보안 서비스를 클러스터와 통합하는 방법을 포괄적으로 보려 하십니까? [클라우드 애플리케이션에 엔드-투-엔드 보안 적용 튜토리얼](/docs/tutorials?topic=solution-tutorials-cloud-e2e-security)을 참조하십시오.
{: shortdesc}

<table summary="이 표에는 클러스터에 추가하여 추가 보안 기능을 추가할 수 있는 사용 가능한 서비스가 표시됩니다. 행은 왼쪽에서 오른쪽으로 읽어야 하며, 1열에는 서비스 이름, 2열에는 서비스의 설명이 있습니다.">
<caption>보안 서비스</caption>
<thead>
<tr>
<th>서비스</th>
<th>설명</th>
</tr>
</thead>
<tbody>
  <tr id="appid">
    <td>{{site.data.keyword.appid_full}}</td>
    <td>사용자가 로그인하도록 하여 [{{site.data.keyword.appid_short}}](/docs/services/appid?topic=appid-getting-started)를 통해 앱에 보안 레벨을 추가합니다. 앱에 웹 또는 API HTTP/HTTPS 요청을 인증하기 위해 [{{site.data.keyword.appid_short_notm}} 인증 Ingress 어노테이션](/docs/containers?topic=containers-ingress_annotation#appid-auth)을 사용하여 {{site.data.keyword.appid_short_notm}}를 Ingress 서비스와 통합할 수 있습니다.</td>
  </tr>
<tr>
<td>Aqua Security</td>
  <td><a href="/docs/services/va?topic=va-va_index" target="_blank">Vulnerability Advisor</a>를 보완하여 <a href="https://www.aquasec.com/" target="_blank">Aqua Security <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>를 사용하면 앱에서 수행하도록 허용되는 항목을 줄여서 컨테이너 배치의 보안을 향상시킬 수 있습니다. 자세한 정보는 <a href="https://blog.aquasec.com/securing-container-deployments-on-bluemix-with-aqua-security" target="_blank">Aqua Security를 사용하여 {{site.data.keyword.Bluemix_notm}}에서 컨테이너 배치 보호 <img src="../icons/launch-glyph.svg" alt="외부링크 아이콘"></a>를 참조하십시오. </td>
</tr>
<tr>
<td>{{site.data.keyword.cloudcerts_full}}</td>
<td><a href="/docs/services/certificate-manager?topic=certificate-manager-getting-started#getting-started" target="_blank">{{site.data.keyword.cloudcerts_long}} <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>를 사용하여 앱에 대한 SSL 인증서를 저장하고 관리할 수 있습니다. 자세한 정보는 <a href="https://www.ibm.com/blogs/bluemix/2018/01/use-ibm-cloud-certificate-manager-ibm-cloud-container-service-deploy-custom-domain-tls-certificates/" target="_blank">{{site.data.keyword.containerlong_notm}}에서 {{site.data.keyword.cloudcerts_long_notm}}를 사용하여 사용자 정의 도메인 TLS 인증서 배치 <img src="../icons/launch-glyph.svg" alt="외부 링크 문서"></a>를 참조하십시오. </td>
</tr>
<tr>
  <td>{{site.data.keyword.datashield_full}}(베타)</td>
  <td><a href="/docs/services/data-shield?topic=data-shield-getting-started#getting-started" target="_blank">{{site.data.keyword.datashield_short}} <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>을 사용하여 데이터 메모리를 암호화할 수 있습니다. {{site.data.keyword.datashield_short}}는 Intel® Software Guard Extensions(SGX) 및 Fortanix® 기술과 통합되어 {{site.data.keyword.Bluemix_notm}} 컨테이너 워크로드 코드와 데이터가 사용 중에 보호됩니다. 앱 코드와 데이터는 앱의 중요한 부분을 보호하는 작업자 노드의 신뢰할 수 있는 메모리 영역인 CPU 강화 엔클레이브에서 실행되므로 코드와 데이터를 기밀로 유지하고 수정하지 않고 유지하는 데 도움이 됩니다.</td>
</tr>
<tr>
  <td>{{site.data.keyword.registrylong}}</td>
  <td>클러스터 사용자 간에 이미지를 안전하게 저장하고 공유할 수 있는 보안된 Docker 이미지 저장소를 설정하십시오. 자세한 정보는 <a href="/docs/services/Registry?topic=registry-getting-started" target="_blank">{{site.data.keyword.registrylong}} 문서 <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>를 참조하십시오.</td>
</tr>
<tr>
  <td>{{site.data.keyword.keymanagementservicefull}}</td>
  <td>{{site.data.keyword.keymanagementserviceshort}}를 사용으로 설정하여 클러스터에 있는 Kubernetes secret을 암호화하십시오. Kubernetes secret을 암호화하면 권한 없는 사용자가 민감한 클러스터 정보에 액세스하지 못하도록 할 수 있습니다.<br>설정하려면 <a href="/docs/containers?topic=containers-encryption#keyprotect">{{site.data.keyword.keymanagementserviceshort}}를 사용한 Kubernetes 시크릿 암호화</a>를 참조하십시오.<br>자세한 정보는 <a href="/docs/services/key-protect?topic=key-protect-getting-started-tutorial" target="_blank">{{site.data.keyword.keymanagementserviceshort}} 문서 <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>를 참조하십시오.</td>
</tr>
<tr>
<td>NeuVector</td>
<td><a href="https://neuvector.com/" target="_blank">NeuVector <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>를 사용하여 클라우드 고유 방화벽으로 컨테이너를 보호합니다. 자세한 정보는 <a href="https://www.ibm.com/us-en/marketplace/neuvector-container-security" target="_blank">NeuVector Container Security <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>를 참조하십시오. </td>
</tr>
<tr>
<td>Twistlock</td>
<td><a href="/docs/services/va?topic=va-va_index" target="_blank">Vulnerability Advisor</a>를 보완하여 <a href="https://www.twistlock.com/" target="_blank">Twistlock <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>을 사용하여 방화벽, 위협 방지 및 인시던트 응답을 관리할 수 있습니다. 자세한 정보는 <a href="https://www.ibm.com/blogs/bluemix/2017/07/twistlock-ibm-bluemix-container-service/" target="_blank">{{site.data.keyword.containerlong_notm}}의 Twistlock <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>을 참조하십시오. </td>
</tr>
</tbody>
</table>

<br />



## 스토리지 서비스
{: #storage_services}
<table summary="이 표에는 클러스터에 추가하여 지속적 스토리지 기능을 추가할 수 있는 사용 가능한 서비스가 표시됩니다. 행은 왼쪽에서 오른쪽으로 읽어야 하며, 1열에는 서비스 이름, 2열에는 서비스의 설명이 있습니다.">
<caption>스토리지 서비스</caption>
<thead>
<tr>
<th>서비스</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
  <td>Heptio Velero</td>
  <td><a href="https://github.com/heptio/velero" target="_blank">Heptio Velero <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>를 사용하여 클러스터 리소스 및 지속적 볼륨을 백업하고 복원할 수 있습니다. 자세한 정보는 Heptio Velero의 <a href="https://github.com/heptio/velero/blob/release-0.9/docs/use-cases.md" target="_blank">재해 복구 및 클러스터 마이그레이션을 위한 유스 케이스 <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>를 참조하십시오.</td>
</tr>
<tr>
  <td>{{site.data.keyword.Bluemix_notm}} Block Storage</td>
  <td>[{{site.data.keyword.Bluemix_notm}} Block Storage](/docs/infrastructure/BlockStorage?topic=BlockStorage-getting-started#getting-started)는 Kubernetes 지속적 볼륨(PV)을 사용하여 앱에 추가할 수 있는 지속적, 고성능 iSCSI 스토리지입니다. 블록 스토리지를 사용하여 단일 구역에 Stateful 앱을 배치하거나 단일 팟(Pod)에 대한 고성능 스토리지로 사용합니다. 클러스터에서 블록 스토리지를 프로비저닝하는 방법에 대한 자세한 정보는 [{{site.data.keyword.Bluemix_notm}} Block Storage에 데이터 저장](/docs/containers?topic=containers-block_storage#block_storage)을 참조하십시오.,</td>
  </tr>
<tr>
  <td>{{site.data.keyword.cos_full}}</td>
  <td>{{site.data.keyword.cos_short}}에 저장된 데이터는 암호화되어 여러 지리적 위치에 분산되며, REST API를 사용하여 HTTP를 통해 액세스할 수 있습니다. [ibm-backup-restore 이미지](/docs/services/RegistryImages/ibm-backup-restore?topic=RegistryImages-ibmbackup_restore_starter) 를 사용하여 클러스터 내의 데이터에 대해 일회성 또는 스케줄된 백업을 작성하도록 서비스를 구성할 수 있습니다. 이 서비스에 대한 일반 정보는 <a href="/docs/services/cloud-object-storage?topic=cloud-object-storage-about-ibm-cloud-object-storage" target="_blank">{{site.data.keyword.cos_short}} 문서 <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>를 참조하십시오.</td>
</tr>
  <tr>
  <td>{{site.data.keyword.Bluemix_notm}} File Storage</td>
  <td>[{{site.data.keyword.Bluemix_notm}} File Storage](/docs/infrastructure/FileStorage?topic=FileStorage-getting-started#getting-started)는 Kubernetes 지속적 볼륨을 사용하여 앱에 추가할 수 있는 지속적이고 빠르며 유연한 네트워크 연결 NFS 기반 파일 스토리지입니다. 워크로드의 요구사항을 충족하는 GB 크기와 IOPS를 사용하여 사전정의된 스토리지 계층 중에서 선택할 수 있습니다. 클러스터에서 파일 스토리지를 프로비저닝하는 방법에 대한 자세한 정보는 [{{site.data.keyword.Bluemix_notm}} File Storage에 데이터 저장](/docs/containers?topic=containers-file_storage#file_storage)을 참조하십시오.</td>
  </tr>
  <tr>
    <td>Portworx</td>
    <td>[Portworx ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://portworx.com/products/introduction/)는 컨테이너화된 데이터베이스와 기타 stateful 앱에 대한 지속적 스토리지를 관리하거나 다중 구역 간 팟(Pod) 사이의 데이터를 공유하는 데 사용할 수 있는 고가용성 소프트웨어 정의 스토리지 솔루션입니다. Helm 차트에 Portworx를 설치하고 Kubernetes 지속적 볼륨을 사용하여 앱에 스토리지를 프로비저닝할 수 있습니다. 클러스터에 Portworx를 설정하는 방법에 대한 자세한 정보는 [Portworx를 사용하는 소프트웨어 정의 스토리지(SDS)에 데이터 저장](/docs/containers?topic=containers-portworx#portworx)을 참조하십시오.</td>
  </tr>
</tbody>
</table>

<br />


## 데이터베이스 서비스
{: #database_services}

<table summary="이 표에는 클러스터에 추가하여 데이터베이스 기능을 추가할 수 있는 사용 가능한 서비스가 표시됩니다. 행은 왼쪽에서 오른쪽으로 읽어야 하며, 1열에는 서비스 이름, 2열에는 서비스의 설명이 있습니다.">
<caption>데이터베이스 서비스</caption>
<thead>
<tr>
<th>서비스</th>
<th>설명</th>
</tr>
</thead>
<tbody>
  <tr>
    <td>{{site.data.keyword.blockchainfull_notm}} Platform 2.0 베타</td>
    <td>{{site.data.keyword.containerlong_notm}}에 사용자 고유의 {{site.data.keyword.blockchainfull_notm}} Platform을 배치하고 관리합니다. {{site.data.keyword.blockchainfull_notm}} Platform 2.0을 사용하면 {{site.data.keyword.blockchainfull_notm}} 네트워크를 호스팅하거나 다른 {{site.data.keyword.blockchainfull_notm}} 2.0 네트워크에 참여할 수 있는 조직을 작성할 수 있습니다. {{site.data.keyword.containerlong_notm}}에서 {{site.data.keyword.blockchainfull_notm}}을 설정하는 방법에 대한 자세한 정보는 [{{site.data.keyword.blockchainfull_notm}} Platform 무료 2.0 베타 개요](/docs/services/blockchain?topic=blockchain-ibp-console-overview#ibp-console-overview)를 참조하십시오.</td>
  </tr>
<tr>
  <td>클라우드 데이터베이스</td>
  <td>다양한 {{site.data.keyword.Bluemix_notm}} 데이터베이스 서비스(예: {{site.data.keyword.composeForMongoDB_full}} 또는 {{site.data.keyword.cloudantfull}}) 중에서 선택하여 클러스터에 고가용성 및 확장 가능한 데이터베이스 솔루션을 배치할 수 있습니다. 사용 가능한 클라우드 데이터베이스 목록은 [{{site.data.keyword.Bluemix_notm}} 카탈로그 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/catalog?category=databases)를 참조하십시오.  </td>
  </tr>
  </tbody>
  </table>


## 클러스터에 {{site.data.keyword.Bluemix_notm}} 서비스 추가
{: #adding_cluster}

Watson AI, 데이터, 보안 및 IoT(Internet of Things) 등의 분야에서 추가 기능으로 Kubernetes 클러스터를 개선하려면 {{site.data.keyword.Bluemix_notm}} 서비스를 추가하십시오.
{:shortdesc}

서비스 키를 지원하는 서비스만 바인딩할 수 있습니다. 서비스 키를 지원하는 서비스가 있는 목록을 찾으려면 [외부 앱이 {{site.data.keyword.Bluemix_notm}} 서비스를 사용하도록 설정](/docs/resources?topic=resources-externalapp#externalapp)을 참조하십시오.
{: note}

시작하기 전에:
- 다음 역할을 보유하고 있는지 확인하십시오.
    - 클러스터에 대한 [**편집자** 또는 **관리자** {{site.data.keyword.Bluemix_notm}} IAM 서비스 역할](/docs/containers?topic=containers-users#platform)
    - 서비스를 바인드할 네임스페이스에 대한 [**작성자** 또는 **관리자** {{site.data.keyword.Bluemix_notm}} IAM 서비스 역할](/docs/containers?topic=containers-users#platform)
    - 사용하려는 영역에 대한 [**개발자** Cloud Foundry 역할](/docs/iam?topic=iam-mngcf#mngcf)
- [계정에 로그인하십시오. 적절한 지역을 대상으로 지정하고, 해당되는 경우에는 리소스 그룹도 지정하십시오. 클러스터의 컨텍스트를 설정하십시오](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).

클러스터에 {{site.data.keyword.Bluemix_notm}} 서비스를 추가하려면 다음을 수행하십시오.

1. [{{site.data.keyword.Bluemix_notm}} 서비스의 인스턴스를 작성](/docs/resources?topic=resources-externalapp#externalapp)하십시오.
    * 몇몇 {{site.data.keyword.Bluemix_notm}} 서비스는 일부 지역에서만 사용 가능합니다. 서비스가 클러스터와 동일한 지역에서 사용 가능한 경우에만 클러스터에 서비스를 바인드할 수 있습니다. 또한 워싱턴 DC 구역에서 서비스 인스턴스를 작성하려면 CLI를 사용해야 합니다.
    * 서비스 인스턴스는 클러스터와 동일한 리소스 그룹에 작성해야 합니다. 특정 리소스는 하나의 리소스 그룹에서만 작성될 수 있으며 이후에는 이를 변경할 수 없습니다.

2. 작성한 서비스 유형을 확인하고 서비스 인스턴스 **이름**을 기록해 두십시오.
   - **Cloud Foundry 서비스:**
     ```
    ibmcloud service list
     ```
     {: pre}

     출력 예:
     ```
     name                         service           plan    bound apps   last operation
     <cf_service_instance_name>   <service_name>    spark                create succeeded
     ```
     {: screen}

  - **{{site.data.keyword.Bluemix_notm}}IAM 사용 서비스:**
     ```
    ibmcloud resource service-instances
     ```
     {: pre}

     출력 예:
     ```
     Name                          Location   State    Type               Tags
     <iam_service_instance_name>   <region>   active   service_instance
     ```
     {: screen}

   **Cloud Foundry 서비스** 및 **서비스**로서 대시보드에서 다른 서비스 유형을 볼 수도 있습니다.

3. 서비스를 추가하는 데 사용할 클러스터 네임스페이스를 식별하십시오. 다음 옵션 중에 선택하십시오.
     ```
         kubectl get namespaces
     ```
     {: pre}

4.  `ibmcloud ks cluster-service-bind` [명령](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_service_bind)을 사용하여 클러스터에 서비스를 추가하십시오. {{site.data.keyword.Bluemix_notm}} IAM 사용 서비스의 경우, 앞에서 작성한 Cloud Foundry 별명을 사용해야 합니다. IAM 사용 서비스의 경우, 기본 **작성자** 서비스 액세스 역할을 사용하거나 `--role` 플래그로 서비스 액세스 역할을 지정할 수 있습니다. 이 명령은 서비스 인스턴스의 서비스 키를 작성합니다. 또는 `--key` 플래그를 포함하여 기존 서비스 키 인증 정보를 사용할 수 있습니다. `--key` 플래그를 사용하지 않는 경우, `--role` 플래그를 포함하지 마십시오.
    ```
    ibmcloud ks cluster-service-bind --cluster <cluster_name_or_ID> --namespace <namespace> --service <service_instance_name> [--key <service_instance_key>] [--role <IAM_service_role>]
    ```
    {: pre}

    서비스가 클러스터에 정상적으로 추가되면 서비스 인스턴스의 인증 정보를 보유하는 클러스터 시크릿이 작성됩니다. 시크릿은 데이터 보호를 위해 etcd에서 자동으로 암호화됩니다.

    출력 예:
    ```
    ibmcloud ks cluster-service-bind --cluster mycluster --namespace mynamespace --service cleardb
    Binding service instance to namespace...
    OK
    Namespace:	     mynamespace
    Secret name:     binding-<service_instance_name>
    ```
    {: screen}

5.  Kubernetes 시크릿의 서비스 인증 정보를 확인하십시오.
    1. 시크릿의 세부사항을 가져오고 **바인딩** 값을 기록해 두십시오. **바인딩** 값은 base64 인코딩되어 있으며, JSON 형식으로 서비스 인스턴스의 인증 정보를 보관합니다.
       ```
       kubectl get secrets binding-<service_instance_name> --namespace=<namespace> -o yaml
       ```
       {: pre}

       출력 예:
       ```
       apiVersion: v1
       data:
         binding: <binding>
       kind: Secret
       metadata:
         annotations:
           service-instance-id: 1111aaaa-a1aa-1aa1-1a11-111aa111aa11
           service-key-id: 2b22bb2b-222b-2bb2-2b22-b22222bb2222
         creationTimestamp: 2018-08-07T20:47:14Z
         name: binding-<service_instance_name>
         namespace: <namespace>
         resourceVersion: "6145900"
         selfLink: /api/v1/namespaces/default/secrets/binding-mycloudant
         uid: 33333c33-3c33-33c3-cc33-cc33333333c
       type: Opaque
       ```
       {: screen}

    2. 바인딩 값을 디코딩하십시오.
       ```
       echo "<binding>" | base64 -D
       ```
       {: pre}

       출력 예:
       ```
       {"apikey":"KL34Ys893284NGJEPFjgrioJ12NElpow","host":"98765aab-9ce1-7tr3-ba87-bfbab6e6d9d6-bluemix.cloudant.com","iam_apikey_description":"Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:cloudantnosqldb:us-south:a/1234g56789cfe8e6388dd2ec098:98746cw-43d7-49ce-947a-d8fe3eebb381::","iam_apikey_name":"auto-generated-apikey-1234abcde-987f-3t64-9d96-d13775ec5663","iam_role_crn":"crn:v1:bluemix:public:iam::::serviceRole:Writer","iam_serviceid_crn":"crn:v1:bluemix:public:iam-identity::a/1234567890brasge5htn2ec098::serviceid:ServiceId-12345vgh-6c4c-ytr12-af6b-467d30d6ef44","password":"jfiavhui12484fnivhuo472nvei23913c3ff","port":443,"url":"https://25c73aac-9ce1-4c24-ba98-bfbab6e6d9d6-bluemix:ugvioev823inreuiegn43donvri29989wiu9t22@25c73aac-9ce1-4c24-ba98-abdrjio123562lnsb-bluemix.cloudant.com","username":"123b45da-9ce1-4c24-ab12-rinwnwub1294-bluemix"}
       ```
       {: screen}

    3. 선택사항: 이전 단계에서 디코딩한 서비스 인증 정보를 {{site.data.keyword.Bluemix_notm}} 대시보드에서 서비스 인스턴스에 대해 찾은 서비스 인증 정보와 비교하십시오.

6. 이제 서비스가 클러스터에 바인드되었으므로 [Kubernetes 시크릿의 서비스 인증 정보에 액세스](#adding_app)하도록 앱을 구성해야 합니다.


## 앱에서 서비스 인증 정보에 액세스
{: #adding_app}

앱에서 {{site.data.keyword.Bluemix_notm}} 서비스 인스턴스에 액세스하려면 Kubernetes 시크릿에 저장된 서비스 인증 정보를 앱에서 사용할 수 있도록 해야 합니다.
{: shortdesc}

서비스 인스턴스의 인증 정보는 base64 인코딩되어 있으며, JSON 형식으로 시크릿 내에 저장됩니다. 시크릿의 데이터에 액세스하려면 다음 옵션 중에서 선택하십시오.
- [볼륨으로서 시크릿을 팟(Pod)에 마운트](#mount_secret)
- [환경 변수의 시크릿 참조](#reference_secret)
<br>
secret을 더 안전하게 보호하려 하십니까? 클러스터에서 {{site.data.keyword.Bluemix_notm}} 서비스 인스턴스의 인증 정보를 저장하는 secret과 같은 기존 및 신규 secret을 암호화하기 위해 클러스터 관리자에게 [{{site.data.keyword.keymanagementservicefull}}를 사용으로 설정](/docs/containers?topic=containers-encryption#keyprotect)하도록 요청하십시오.
{: tip}

시작하기 전에:
-  `kube-system` 네임스페이스에 대해 [**작성자** 또는 **관리자** {{site.data.keyword.Bluemix_notm}} IAM 서비스 역할](/docs/containers?topic=containers-users#platform)이 있는지 확인하십시오.
- [계정에 로그인하십시오. 적절한 지역을 대상으로 지정하고, 해당되는 경우에는 리소스 그룹도 지정하십시오. 클러스터의 컨텍스트를 설정하십시오](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).
- [{{site.data.keyword.Bluemix_notm}} 서비스를 클러스터에 추가](#adding_cluster)하십시오.

### 볼륨으로서 시크릿을 팟(Pod)에 마운트
{: #mount_secret}

볼륨으로서 시크릿을 팟(Pod)에 마운트하면 이름이 `binding`인 파일이 볼륨 마운트 디렉토리에 저장됩니다. JSON 형식의 `binding` 파일에는 {{site.data.keyword.Bluemix_notm}} 서비스에 액세스하는 데 필요한 모든 정보와 인증 정보가 포함되어 있습니다.
{: shortdesc}

1.  클러스터의 사용 가능한 시크릿을 나열하고 시크릿의 **이름**을 기록해 두십시오. **오파크** 유형의 시크릿을 찾으십시오. 다수의 시크릿이 존재하면 클러스터 관리자에게 문의하여 올바른 서비스 시크릿을 식별하십시오.

    ```
    kubectl get secrets
    ```
    {: pre}

    출력 예:

    ```
    NAME                              TYPE            DATA      AGE
    binding-<service_instance_name>   Opaque          1         3m

    ```
    {: screen}

2.  Kubernetes 배치를 위한 YAML 파일을 작성하고 볼륨으로서 시크릿을 팟(Pod)에 마운트하십시오.
    ```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      labels:
        app: secret-test
      name: secret-test
      namespace: <my_namespace>
    spec:
      selector:
        matchLabels:
          app: secret-test
      replicas: 1
      template:
        metadata:
          labels:
            app: secret-test
        spec:
          containers:
          - image: registry.bluemix.net/ibmliberty:latest
            name: secret-test
            volumeMounts:
            - mountPath: <mount_path>
              name: <volume_name>
          volumes:
          - name: <volume_name>
            secret:
              defaultMode: 420
              secretName: binding-<service_instance_name>
    ```
    {: codeblock}

    <table>
    <caption>YAML 파일 컴포넌트 이해</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> YAML 파일 컴포넌트 이해</th>
    </thead>
    <tbody>
    <tr>
    <td><code>volumeMounts.mountPath</code></td>
    <td>컨테이너 내에서 볼륨이 마운트되는 디렉토리의 절대 경로입니다.</td>
    </tr>
    <tr>
    <td><code>volumeMounts.name</code></br><code>volumes.name</code></td>
    <td>팟(Pod)에 마운트할 볼륨의 이름입니다.</td>
    </tr>
    <tr>
    <td><code>secret.defaultMode</code></td>
    <td>시크릿에 대한 읽기 및 쓰기 권한입니다. 읽기 전용 권한을 설정하려면 `420`을 사용하십시오. </td>
    </tr>
    <tr>
    <td><code>secret.secretName</code></td>
    <td>이전 단계에서 기록해 둔 시크릿의 이름입니다.</td>
    </tr></tbody></table>

3.  팟(Pod)을 작성하고 볼륨으로서 시크릿을 마운트하십시오.
    ```
    kubectl apply -f secret-test.yaml
    ```
    {: pre}

4.  팟(Pod)이 작성되었는지 확인하십시오.
    ```
   kubectl get pods
    ```
    {: pre}

    CLI 출력 예:

    ```
    NAME                           READY     STATUS    RESTARTS   AGE
    secret-test-1111454598-gfx32   1/1       Running   0          1m
    ```
    {: screen}

5.  서비스 인증 정보에 액세스하십시오.
    1. 팟(Pod)에 로그인하십시오.
       ```
       kubectl exec <pod_name> -it bash
       ```
       {: pre}

    2. 이전에 정의한 볼륨 마운트 경로로 이동하고 볼륨 마운트 경로의 파일을 나열하십시오.
       ```
       cd <volume_mountpath> && ls
       ```
       {: pre}

       출력 예:
       ```
       binding
       ```
       {: screen}

       `binding` 파일에는 Kubernetes 시크릿에 저장된 서비스 인증 정보가 포함되어 있습니다.

    4. 서비스 인증 정보를 보십시오. 인증 정보는 JSON 형식의 키 값 쌍으로 저장됩니다.
       ```
       cat binding
       ```
       {: pre}

       출력 예:
       ```
       {"apikey":"KL34Ys893284NGJEPFjgrioJ12NElpow","host":"98765aab-9ce1-7tr3-ba87-bfbab6e6d9d6-bluemix.cloudant.com","iam_apikey_description":"Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:cloudantnosqldb:us-south:a/1234g56789cfe8e6388dd2ec098:98746cw-43d7-49ce-947a-d8fe3eebb381::","iam_apikey_name":"auto-generated-apikey-1234abcde-987f-3t64-9d96-d13775ec5663","iam_role_crn":"crn:v1:bluemix:public:iam::::serviceRole:Writer","iam_serviceid_crn":"crn:v1:bluemix:public:iam-identity::a/1234567890brasge5htn2ec098::serviceid:ServiceId-12345vgh-6c4c-ytr12-af6b-467d30d6ef44","password":"jfiavhui12484fnivhuo472nvei23913c3ff","port":443,"url":"https://25c73aac-9ce1-4c24-ba98-bfbab6e6d9d6-bluemix:ugvioev823inreuiegn43donvri29989wiu9t22@25c73aac-9ce1-4c24-ba98-abdrjio123562lnsb-bluemix.cloudant.com","username":"123b45da-9ce1-4c24-ab12-rinwnwub1294-bluemix"}
       ```
       {: screen}

    5. JSON 컨텐츠를 구문 분석하고 서비스에 액세스하는 데 필요한 정보를 검색하도록 앱을 구성하십시오.


### 환경 변수의 시크릿 참조
{: #reference_secret}

서비스 인증 정보와 Kubernetes 시크릿의 기타 키 값 쌍을 환경 변수로서 배치에 추가할 수 있습니다.
{: shortdesc}

1. 클러스터의 사용 가능한 시크릿을 나열하고 시크릿의 **이름**을 기록해 두십시오. **오파크** 유형의 시크릿을 찾으십시오. 다수의 시크릿이 존재하면 클러스터 관리자에게 문의하여 올바른 서비스 시크릿을 식별하십시오.

    ```
    kubectl get secrets
    ```
    {: pre}

    출력 예:

    ```
    NAME                              TYPE            DATA      AGE
    binding-<service_instance_name>   Opaque          1         3m
    ```
    {: screen}

2. 시크릿의 세부사항을 가져와서 팟(Pod)에서 환경 변수로서 참조할 수 있는 잠재적 키 값 쌍을 찾으십시오. 서비스 인증 정보는 시크릿의 `binding` 키에 저장됩니다.
   ```
   kubectl get secrets binding-<service_instance_name> --namespace=<namespace> -o yaml
   ```
   {: pre}

   출력 예:
   ```
   apiVersion: v1
   data:
     binding: <binding>
   kind: Secret
   metadata:
     annotations:
       service-instance-id: 7123acde-c3ef-4ba2-8c52-439ac007fa70
       service-key-id: 9h30dh8a-023f-4cf4-9d96-d12345ec7890
     creationTimestamp: 2018-08-07T20:47:14Z
     name: binding-<service_instance_name>
     namespace: <namespace>
     resourceVersion: "6145900"
     selfLink: /api/v1/namespaces/default/secrets/binding-mycloudant
     uid: 12345a31-9a83-11e8-ba83-cd49014748f
   type: Opaque
   ```
   {: screen}

3. Kubernetes 배치를 위한 YAML 파일을 작성하고 `binding` 키를 참조하는 환경 변수를 지정하십시오.
   ```
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     labels:
       app: secret-test
     name: secret-test
     namespace: <my_namespace>
   spec:
     selector:
       matchLabels:
         app: secret-test
     template:
       metadata:
         labels:
           app: secret-test
       spec:
         containers:
         - image: registry.bluemix.net/ibmliberty:latest
           name: secret-test
           env:
           - name: BINDING
             valueFrom:
               secretKeyRef:
                 name: binding-<service_instance_name>
                 key: binding
     ```
     {: codeblock}

     <table>
     <caption>YAML 파일 컴포넌트 이해</caption>
     <thead>
     <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> YAML 파일 컴포넌트 이해</th>
     </thead>
     <tbody>
     <tr>
     <td><code>containers.env.name</code></td>
     <td>환경 변수의 이름입니다.</td>
     </tr>
     <tr>
     <td><code>env.valueFrom.secretKeyRef.name</code></td>
     <td>이전 단계에서 기록해 둔 시크릿의 이름입니다.</td>
     </tr>
     <tr>
     <td><code>env.valueFrom.secretKeyRef.key</code></td>
     <td>시크릿의 일부이며 사용자가 환경 변수에서 참조하고자 하는 키입니다. 서비스 인증 정보를 참조하려면 <strong>바인딩</strong> 키를 사용해야 합니다.  </td>
     </tr>
     </tbody></table>

4. 환경 변수로서 시크릿의 `binding` 키를 참조하는 팟(Pod)을 작성하십시오.
   ```
    kubectl apply -f secret-test.yaml
   ```
   {: pre}

5. 팟(Pod)이 작성되었는지 확인하십시오.
   ```
   kubectl get pods
   ```
   {: pre}

   CLI 출력 예:
   ```
    NAME                           READY     STATUS    RESTARTS   AGE
    secret-test-1111454598-gfx32   1/1       Running   0          1m
   ```
   {: screen}

6. 환경 변수가 올바르게 설정되었는지 확인하십시오.
   1. 팟(Pod)에 로그인하십시오.
      ```
      kubectl exec <pod_name> -it bash
      ```
      {: pre}

   2. 팟(Pod)의 모든 환경 변수를 나열하십시오.
      ```
      env
      ```
      {: pre}

      출력 예:
      ```
      BINDING={"apikey":"KL34Ys893284NGJEPFjgrioJ12NElpow","host":"98765aab-9ce1-7tr3-ba87-bfbab6e6d9d6-bluemix.cloudant.com","iam_apikey_description":"Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:cloudantnosqldb:us-south:a/1234g56789cfe8e6388dd2ec098:98746cw-43d7-49ce-947a-d8fe3eebb381::","iam_apikey_name":"auto-generated-apikey-1234abcde-987f-3t64-9d96-d13775ec5663","iam_role_crn":"crn:v1:bluemix:public:iam::::serviceRole:Writer","iam_serviceid_crn":"crn:v1:bluemix:public:iam-identity::a/1234567890brasge5htn2ec098::serviceid:ServiceId-12345vgh-6c4c-ytr12-af6b-467d30d6ef44","password":"jfiavhui12484fnivhuo472nvei23913c3ff","port":443,"url":"https://25c73aac-9ce1-4c24-ba98-bfbab6e6d9d6-bluemix:ugvioev823inreuiegn43donvri29989wiu9t22@25c73aac-9ce1-4c24-ba98-abdrjio123562lnsb-bluemix.cloudant.com","username":"123b45da-9ce1-4c24-ab12-rinwnwub1294-bluemix"}
      ```
      {: screen}

7. 환경 변수를 읽고 JSON 컨텐츠를 구문 분석하여 서비스에 액세스하는 데 필요한 정보를 검색하도록 앱을 구성하십시오.

   Python의 예제 코드:
   ```
   if os.environ.get('BINDING'):
        credentials = json.loads(os.environ.get('BINDING'))
   ```
   {: codeblock}

## {{site.data.keyword.containerlong_notm}}에서 Helm 설정
{: #helm}

[Helm ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://helm.sh)은 Kubernetes 패키지 관리자입니다. {{site.data.keyword.containerlong_notm}} 클러스터에서 실행되는 복잡한 Kubernetes 애플리케이션을 정의, 설치 및 업그레이드하기 위해 Helm 차트를 작성하거나 기존 Helm 차트를 사용할 수 있습니다.
{:shortdesc}

Helm 차트를 배치하려면 로컬 시스템에 Helm CLI를 설치하고 클러스터에 Helm 서버(Tiller)를 설치해야 합니다. Tiller 이미지는 공용 Google Container Registry에 저장됩니다. Tiller 설치 중에 이미지에 액세스하려면 클러스터에서 공용 Google Container Registry에 대한 공용 네트워크 연결을 허용해야 합니다. 공용 서비스 엔드포인트가 사용으로 설정된 클러스터는 자동으로 이미지에 액세스할 수 있습니다. 사용자 정의 방화벽으로 보호되는 사설 클러스터 또는 개인 서비스 엔드포인트만 사용하도록 설정된 클러스터는 Tiller 이미지에 대한 액세스를 허용하지 않습니다. 대신 [로컬 시스템으로 이미지를 가져오고 {{site.data.keyword.registryshort_notm}}의 네임스페이스에 이미지를 푸시](#private_local_tiller)하거나 [Tiller를 사용하지 않고 Helm 차트를 설치](#private_install_without_tiller)하십시오.
{: note}

### 공용 액세스 권한이 있는 클러스터에 Helm 설정
{: #public_helm_install}

클러스터에서 공용 서비스 엔드포인트를 사용으로 설정한 경우에는 Google Container Registry의 공용 이미지를 사용하여 Tiller를 설치할 수 있습니다.
{: shortdesc}

시작하기 전에: [계정에 로그인하십시오. 적절한 지역을 대상으로 지정하고, 해당되는 경우에는 리소스 그룹도 지정하십시오. 클러스터의 컨텍스트를 설정하십시오](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).

1. <a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">Helm CLI <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>를 설치하십시오.

2. **중요**: 클러스터 보안을 유지하기 위해, [{{site.data.keyword.Bluemix_notm}} `kube-samples` 저장소](https://github.com/IBM-Cloud/kube-samples/blob/master/rbac/serviceaccount-tiller.yaml)로부터 다음 `.yaml` 파일을 적용하여 `kube-system` 네임스페이스에 Tiller에 대한 서비스 계정을 작성하고 `tiller-deploy` 팟(Pod)에 대한 Kubernetes RBAC 클러스터 역할 바인딩을 작성하십시오. **참고**: `kube-system` 네임스페이스에서 서비스 계정 및 클러스터 역할 바인딩으로 Tiller를 설치하려면 [`cluster-admin` 역할](/docs/containers?topic=containers-users#access_policies)이 있어야 합니다.
    ```
    kubectl apply -f https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/rbac/serviceaccount-tiller.yaml
    ```
    {: pre}

3. Helm을 초기화하고 작성한 서비스 계정을 사용하여 Tiller를 설치하십시오.

    ```
    helm init --service-account tiller
    ```
    {: pre}

4.  설치가 완료되었는지 확인하십시오.
    1.  Tiller 서비스 계정이 작성되었는지 확인하십시오.
        ```
        kubectl get serviceaccount -n kube-system tiller
        ```
        {: pre}

        출력 예:

        ```
        NAME                                 SECRETS   AGE
        tiller                               1         2m
        ```
        {: screen}

    2.  `tiller-deploy` 팟(Pod)이 클러스터에서 `Running` **상태**인지 확인하십시오.
        ```
        kubectl get pods -n kube-system -l app=helm
        ```
        {: pre}

        출력 예:

        ```
    NAME                            READY     STATUS    RESTARTS   AGE
    tiller-deploy-352283156-nzbcm   1/1       Running   0          2m
        ```
        {: screen}

5. {{site.data.keyword.Bluemix_notm}} Helm 저장소를 Helm 인스턴스에 추가하십시오.
   ```
   helm repo add ibm https://registry.bluemix.net/helm/ibm
   ```
   {: pre}

   ```
    helm repo add ibm-charts https://registry.bluemix.net/helm/ibm-charts
   ```
   {: pre}

6. 모든 Helm 차트의 최신 버전을 검색하려면 저장소를 업데이트하십시오.
   ```
   helm repo update
   ```
   {: pre}

7. 현재 {{site.data.keyword.Bluemix_notm}} 저장소에서 사용 가능한 Helm 차트를 나열하십시오.
   ```
    helm search ibm
   ```
   {: pre}

   ```
    helm search ibm-charts
   ```
   {: pre}

8. 설치할 Helm 차트를 식별하고 Helm 차트 `README`의 지시사항에 따라 클러스터에 Helm 차트를 설치하십시오.


### 사설 클러스터: {{site.data.keyword.registryshort_notm}}의 개인용 레지스트리에 Tiller 이미지 푸시
{: #private_local_tiller}

Tiller 이미지를 로컬 시스템으로 가져오고 이를 {{site.data.keyword.registryshort_notm}}의 네임스페이스에 푸시하고 {{site.data.keyword.registryshort_notm}}의 이미지를 사용하여 Helm이 Tiller를 설치하도록 할 수 있습니다.
{: shortdesc}

시작하기 전에:
- 로컬 시스템에 Docker를 설치하십시오. [{{site.data.keyword.Bluemix_notm}} CLI](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli)를 설치한 경우 Docker는 이미 설치되어 있습니다.
- [{{site.data.keyword.registryshort_notm}} CLI 플러그인을 설치하고 네임스페이스를 설정](/docs/services/Registry?topic=registry-getting-started#gs_registry_cli_install)하십시오.

{{site.data.keyword.registryshort_notm}}을 사용하여 Tiller를 설치하려면 다음을 수행하십시오.

1. 로컬 시스템에 <a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">Helm CLI <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>를 설치하십시오.
2. 설정한 {{site.data.keyword.Bluemix_notm}} 인프라 VPN 터널을 사용하여 사설 클러스터에 연결하십시오.
3. **중요**: 클러스터 보안을 유지하기 위해, [{{site.data.keyword.Bluemix_notm}} `kube-samples` 저장소](https://github.com/IBM-Cloud/kube-samples/blob/master/rbac/serviceaccount-tiller.yaml)로부터 다음 `.yaml` 파일을 적용하여 `kube-system` 네임스페이스에 Tiller에 대한 서비스 계정을 작성하고 `tiller-deploy` 팟(Pod)에 대한 Kubernetes RBAC 클러스터 역할 바인딩을 작성하십시오. **참고**: `kube-system` 네임스페이스에서 서비스 계정 및 클러스터 역할 바인딩으로 Tiller를 설치하려면 [`cluster-admin` 역할](/docs/containers?topic=containers-users#access_policies)이 있어야 합니다.
    1. [Kubernetes 서비스 계정 및 클러스터 역할 바인딩 YAML 파일을 가져오십시오 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/rbac/serviceaccount-tiller.yaml).

    2. 클러스터에 Kubernetes 리소스를 작성하십시오.
       ```
       kubectl apply -f service-account.yaml
       ```
       {: pre}

       ```
       kubectl apply -f cluster-role-binding.yaml
       ```
       {: pre}

4. 클러스터에 설치할 [Tiller 버전을 찾으십시오 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://console.cloud.google.com/gcr/images/kubernetes-helm/GLOBAL/tiller?gcrImageListsize=30)]. 특정 버전이 필요하지 않은 경우 최신 버전을 사용하십시오.

5. Tiller 이미지를 로컬 시스템으로 가져오십시오.
   ```
   docker pull gcr.io/kubernetes-helm/tiller:v2.11.0
   ```
   {: pre}

   출력 예:
   ```
   docker pull gcr.io/kubernetes-helm/tiller:v2.13.0
   v2.13.0: Pulling from kubernetes-helm/tiller
   48ecbb6b270e: Pull complete
   d3fa0712c71b: Pull complete
   bf13a43b92e9: Pull complete
   b3f98be98675: Pull complete
   Digest: sha256:c4bf03bb67b3ae07e38e834f29dc7fd43f472f67cad3c078279ff1bbbb463aa6
   Status: Downloaded newer image for gcr.io/kubernetes-helm/tiller:v2.13.0
   ```
   {: screen}

6. [{{site.data.keyword.registryshort_notm}}에서 네임스페이스에 Tiller 이미지를 푸시](/docs/services/Registry?topic=registry-getting-started#gs_registry_images_pushing)하십시오.

7. [이미지 풀 시크릿을 복사하여 기본 네임스페이스에서 `kube-system` 네임스페이스로 {{site.data.keyword.registryshort_notm}}에 액세스하십시오](/docs/containers?topic=containers-images#copy_imagePullSecret).

8. {{site.data.keyword.registryshort_notm}}의 네임스페이스에 저장한 이미지를 사용하여 사설 클러스터에 Tiller를 설치하십시오.
   ```
   helm init --tiller-image <region>.icr.io/<mynamespace>/<myimage>:<tag> --service-account tiller
   ```
   {: pre}

9. {{site.data.keyword.Bluemix_notm}} Helm 저장소를 Helm 인스턴스에 추가하십시오.
   ```
   helm repo add ibm https://registry.bluemix.net/helm/ibm
   ```
   {: pre}

   ```
    helm repo add ibm-charts https://registry.bluemix.net/helm/ibm-charts
   ```
   {: pre}

10. 모든 Helm 차트의 최신 버전을 검색하려면 저장소를 업데이트하십시오.
    ```
   helm repo update
    ```
    {: pre}

11. 현재 {{site.data.keyword.Bluemix_notm}} 저장소에서 사용 가능한 Helm 차트를 나열하십시오.
    ```
    helm search ibm
    ```
    {: pre}

    ```
    helm search ibm-charts
    ```
    {: pre}

12. 설치할 Helm 차트를 식별하고 Helm 차트 `README`의 지시사항에 따라 클러스터에 Helm 차트를 설치하십시오.

### 사설 클러스터: Tiller를 사용하지 않고 Helm 차트 설치
{: #private_install_without_tiller}

사설 클러스터에 Tiller를 설치하지 않으려면, Helm 차트 YAML 파일을 수동으로 작성하고 `kubectl` 명령을 사용하여 적용할 수 있습니다.
{: shortdesc}

이 예제의 단계는 사설 클러스터의 {{site.data.keyword.Bluemix_notm}} Helm 차트 저장소에서 Helm 차트를 설치하는 방법을 보여줍니다. {{site.data.keyword.Bluemix_notm}} Helm 차트 저장소 중 하나에 저장되지 않은 Helm 차트를 설치하려면 이 주제의 지시사항에 따라 Helm 차트에 필요한 YAML 파일을 작성해야 합니다. 또한 공용 컨테이너 레지스트리에서 Helm 차트 이미지를 다운로드하고 이를 {{site.data.keyword.registryshort_notm}}의 네임스페이스에 푸시하고 `values.yaml` 파일을 업데이트하여 {{site.data.keyword.registryshort_notm}}의 이미지를 사용해야 합니다.
{: note}

1. 로컬 시스템에 <a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">Helm CLI <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>를 설치하십시오.
2. 설정한 {{site.data.keyword.Bluemix_notm}} 인프라 VPN 터널을 사용하여 사설 클러스터에 연결하십시오.
3. {{site.data.keyword.Bluemix_notm}} Helm 저장소를 Helm 인스턴스에 추가하십시오.
   ```
   helm repo add ibm https://registry.bluemix.net/helm/ibm
   ```
   {: pre}

   ```
    helm repo add ibm-charts https://registry.bluemix.net/helm/ibm-charts
   ```
   {: pre}

4. 모든 Helm 차트의 최신 버전을 검색하려면 저장소를 업데이트하십시오.
   ```
   helm repo update
   ```
   {: pre}

5. 현재 {{site.data.keyword.Bluemix_notm}} 저장소에서 사용 가능한 Helm 차트를 나열하십시오.
   ```
    helm search ibm
   ```
   {: pre}

   ```
    helm search ibm-charts
   ```
   {: pre}

6. 설치하려는 Helm 차트를 식별하고, 로컬 시스템에 Helm 차트를 다운로드하고, Helm 차트의 파일을 압축 해체하십시오. 다음 예제는 클러스터 오토스케일러 버전 1.0.3의 Helm 차트를 다운로드하고 `cluster-autoscaler` 디렉토리에 해당 파일을 압축 해제하는 방법을 보여줍니다.
   ```
   helm fetch ibm/ibm-iks-cluster-autoscaler --untar --untardir ./cluster-autoscaler --version 1.0.3
   ```
   {: pre}

7. Helm 차트 파일을 압축 해제한 디렉토리로 이동하십시오.
   ```
   cd cluster-autoscaler
   ```
   {: pre}

8. Helm 차트의 파일을 사용하여 생성하는 YAML 파일에 대한 `output` 디렉토리를 작성하십시오.
   ```
   mkdir output
   ```
   {: pre}

9. `values.yaml` 파일을 열고 Helm 차트 설치 지시사항에 필요한 사항을 변경하십시오.
   ```
   nano ibm-iks-cluster-autoscaler/values.yaml
   ```
   {: pre}

10. 로컬 Helm 설치를 사용하여 Helm 차트에 대한 모든 Kubernetes YAML 파일을 작성하십시오. YAML 파일은 이전에 작성한 `output` 디렉토리에 저장됩니다.
    ```
    helm template --values ./ibm-iks-cluster-autoscaler/values.yaml --output-dir ./output ./ibm-iks-cluster-autoscaler
    ```
    {: pre}

    출력 예:
    ```
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-configmap.yaml
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-service-account-roles.yaml
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-service.yaml
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-deployment.yaml
    ```
    {: screen}

11. 사설 클러스터에 모든 YAML 파일을 배치하십시오.
    ```
    kubectl apply --recursive --filename ./output
    ```
   {: pre}

12. 선택사항: `output` 디렉토리에서 모든 YAML 파일을 제거하십시오.
    ```
    kubectl delete --recursive --filename ./output
    ```
    {: pre}


### 관련된 Helm 링크
{: #helm_links}

* strongSwan Helm 차트를 사용하려면 [strongSwan IPSec VPN 서비스 Helm 차트와 VPN 연결 설정](/docs/containers?topic=containers-vpn#vpn-setup)을 참조하십시오.
* {{site.data.keyword.Bluemix_notm}}에서 사용할 수 있는 사용 가능한 Helm 차트는 콘솔의 [Helm 차트 카탈로그 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/kubernetes/solutions/helm-charts)에서 확인하십시오.
* <a href="https://docs.helm.sh/helm/" target="_blank">Helm 문서 <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>에서 Helm 차트를 설정하고 관리하는 데 사용되는 Helm 명령에 대해 자세히 알아보십시오.
* [Kubernetes Helm 차트를 사용하여 배치 속도를 향상 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://developer.ibm.com/recipes/tutorials/increase-deployment-velocity-with-kubernetes-helm-charts/)시키는 방법에 대해 자세히 알아보십시오.

## Kubernetes 클러스터 리소스 시각화
{: #weavescope}

Weave Scope는 서비스, 팟(Pod), 컨테이너, 프로세스 등을 포함하여 Kubernetes 클러스터 내의 리소스에 대한 시각적 다이어그램을 제공합니다. Weave Scope는 CPU 및 메모리 및 도구에 대한 대화식 메트릭을 제공하여 컨테이너로 tail 및 exec를 실행합니다.
{:shortdesc}

시작하기 전에:

-   공용 인터넷에 클러스터 정보를 노출하지 않도록 유념하십시오. 이러한 단계를 완료하여 안전하게 Weave Scope를 배치하고 웹 브라우저에서 로컬로 이에 액세스하십시오.
-   클러스터가 아직 없으면 [표준 클러스터를 작성](/docs/containers?topic=containers-clusters#clusters_ui)하십시오. Weave Scope는 CPU를 많이 사용할 수 있습니다(특히, 앱의 경우). 무료 클러스터가 아닌 더 큰 표준 클러스터에서 Weave Scope를 실행하십시오.
-  모든 네임스페이스에 대해 [**관리자** {{site.data.keyword.Bluemix_notm}} IAM 서비스 역할](/docs/containers?topic=containers-users#platform)이 있는지 확인하십시오.
-   [계정에 로그인하십시오. 적절한 지역을 대상으로 지정하고, 해당되는 경우에는 리소스 그룹도 지정하십시오. 클러스터의 컨텍스트를 설정하십시오](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).


클러스터에서 Weave Scope를 사용하려면 다음을 수행하십시오.
2.  클러스터에서 제공된 RBAC 권한 구성 파일 중 하나를 배치하십시오.

    읽기/쓰기 권한을 사용하려면 다음을 실행하십시오.

    ```
    kubectl apply -f "https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/weave-scope/weave-scope-rbac.yaml"
    ```
    {: pre}

    읽기 전용 권한을 사용하려면 다음을 실행하십시오.

    ```
    kubectl apply --namespace weave -f "https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/weave-scope/weave-scope-rbac-readonly.yaml"
    ```
    {: pre}

    출력:

    ```
    clusterrole "weave-scope-mgr" created
    clusterrolebinding "weave-scope-mgr-role-binding" created
    ```
    {: screen}

3.  클러스터 IP 주소로 개별적 액세스가 가능한 Weave Scope 서비스를 배치하십시오.

    <pre class="pre">
    <code>kubectl apply --namespace weave -f "https://cloud.weave.works/k8s/scope.yaml?k8s-version=$(kubectl version | base64 | tr -d '&bsol;n')"</code>
    </pre>

    출력:

    ```
    serviceaccount "weave-scope" created
    deployment "weave-scope-app" created
    service "weave-scope-app" created
    daemonset "weave-scope-agent" created
    ```
    {: screen}

4.  포트 전달 명령을 실행하여 컴퓨터에서 서비스를 여십시오. 다음에 Weave Scope에 액세스할 때 이전 구성 단계를 다시 완료하지 않고도 이 포트 전달 명령을 실행할 수 있습니다.

    ```
    kubectl port-forward -n weave "$(kubectl get -n weave pod --selector=weave-scope-component=app -o jsonpath='{.items..metadata.name}')" 4040
    ```
    {: pre}

    출력:

    ```
    Forwarding from 127.0.0.1:4040 -> 4040
    Forwarding from [::1]: :1]:4040 -> 4040
    Handling connection for 4040
    ```
    {: screen}

5.  `http://localhost:4040` 주소로 브라우저를 여십시오. 기본 컴포넌트가 배치되지 않은 경우 다음 다이어그램이 표시됩니다. 클러스터에서 Kubernetes 리소스의 토폴로지 다이어그램이나 테이블을 보도록 선택할 수 있습니다.

     <img src="images/weave_scope.png" alt="Weave Scope의 토폴로지 예" style="width:357px;" />


[Weave Scope 기능에 대해 자세히 알아보십시오 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.weave.works/docs/scope/latest/features/).

<br />

