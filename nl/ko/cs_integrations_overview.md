---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

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
{:preview: .preview}f


# 지원되는 IBM Cloud 및 서드파티 통합
{: #supported_integrations}

{{site.data.keyword.containerlong}}의 표준 Kubernetes 클러스터에서 다양한 외부 서비스와 카탈로그 서비스를 사용할 수 있습니다.
{:shortdesc}

## 인기 있는 통합
{: #popular_services}

<table summary="이 표는 클러스터에 추가할 수 있으며 {{site.data.keyword.containerlong_notm}} 사용자에게 인기 있는 사용 가능한 서비스를 보여줍니다. 행은 왼쪽에서 오른쪽으로 읽어야 하며 1열에는 서비스의 이름, 2열에는 서비스에 대한 설명이 있습니다.">
<caption>인기 있는 서비스</caption>
<thead>
<tr>
<th>서비스</th>
<th>카테고리</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.cloudaccesstrailfull}}</td>
<td>클러스터 활동 로그</td>
<td>Grafana를 통해 로그를 분석하여 클러스터에서 수행된 관리 활동을 모니터합니다. 서비스에 대한 자세한 정보는 [활동 트래커](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started) 문서를 참조하십시오. 추적할 수 있는 이벤트의 유형에 대한 자세한 정보는 [활동 트래커 이벤트](/docs/containers?topic=containers-at_events)를 참조하십시오.</td>
</tr>
<tr>
<td>{{site.data.keyword.appid_full}}</td>
<td>인증</td>
<td>사용자가 로그인하도록 하여 [{{site.data.keyword.appid_short}}](/docs/services/appid?topic=appid-getting-started)를 통해 앱에 보안 레벨을 추가합니다. 앱에 웹 또는 API HTTP/HTTPS 요청을 인증하기 위해 [{{site.data.keyword.appid_short_notm}} 인증 Ingress 어노테이션](/docs/containers?topic=containers-ingress_annotation#appid-auth)을 사용하여 {{site.data.keyword.appid_short_notm}}를 Ingress 서비스와 통합할 수 있습니다.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix}} Block Storage</td>
<td>블록 스토리지</td>
<td>[{{site.data.keyword.Bluemix_notm}} Block Storage](/docs/infrastructure/BlockStorage?topic=BlockStorage-getting-started#getting-started)는 Kubernetes 지속적 볼륨(PV)을 사용하여 앱에 추가할 수 있는 지속적, 고성능 iSCSI 스토리지입니다. 블록 스토리지를 사용하여 단일 구역에 Stateful 앱을 배치하거나, 블록 스토리지를 단일 팟(Pod)에 대한 고성능 스토리지로 사용하십시오. 클러스터에서 블록 스토리지를 프로비저닝하는 방법에 대한 자세한 정보는 [{{site.data.keyword.Bluemix_notm}} Block Storage에 데이터 저장](/docs/containers?topic=containers-block_storage#block_storage)을 참조하십시오.,</td>
</tr>
<tr>
<td>{{site.data.keyword.cloudcerts_full}}</td>
<td>TLS 인증서</td>
<td><a href="/docs/services/certificate-manager?topic=certificate-manager-getting-started#getting-started" target="_blank">{{site.data.keyword.cloudcerts_long}} <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>를 사용하여 앱에 대한 SSL 인증서를 저장하고 관리할 수 있습니다. 자세한 정보는 <a href="https://www.ibm.com/blogs/bluemix/2018/01/use-ibm-cloud-certificate-manager-ibm-cloud-container-service-deploy-custom-domain-tls-certificates/" target="_blank">{{site.data.keyword.containerlong_notm}}에서 {{site.data.keyword.cloudcerts_long_notm}}를 사용하여 사용자 정의 도메인 TLS 인증서 배치 <img src="../icons/launch-glyph.svg" alt="외부 링크 문서"></a>를 참조하십시오. </td>
</tr>
<tr>
<td>{{site.data.keyword.registrylong}}</td>
<td>컨테이너 이미지</td>
<td>클러스터 사용자 간에 이미지를 안전하게 저장하고 공유할 수 있는 보안된 Docker 이미지 저장소를 설정하십시오. 자세한 정보는 <a href="/docs/services/Registry?topic=registry-getting-started" target="_blank">{{site.data.keyword.registrylong}} 문서 <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>를 참조하십시오.</td>
</tr>
<tr>
<td>{{site.data.keyword.contdelivery_full}}</td>
<td>빌드 자동화</td>
<td>도구 체인을 사용하여 Kubernetes 클러스터에 대한 컨테이너 배치 및 앱 빌드를 자동화합니다. 설정에 대한 자세한 정보는 블로그 <a href="https://developer.ibm.com/recipes/tutorials/deploy-kubernetes-pods-to-the-bluemix-container-service-using-devops-pipelines/" target="_blank">Deploy Kubernetes pods to the {{site.data.keyword.containerlong_notm}} using DevOps Pipelines <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>를 참조하십시오. </td>
</tr>
<tr>
<td>{{site.data.keyword.datashield_full}}(베타)</td>
<td>메모리 암호화</td>
<td><a href="/docs/services/data-shield?topic=data-shield-getting-started#getting-started" target="_blank">{{site.data.keyword.datashield_short}} <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>을 사용하여 데이터 메모리를 암호화할 수 있습니다. {{site.data.keyword.datashield_short}}는 Intel® Software Guard Extensions(SGX) 및 Fortanix® 기술과 통합되어 {{site.data.keyword.Bluemix_notm}} 컨테이너 워크로드 코드와 데이터가 사용 중에 보호됩니다. 앱 코드와 데이터는 앱의 중요한 부분을 보호하는 작업자 노드의 신뢰할 수 있는 메모리 영역인 CPU 강화 엔클레이브에서 실행되므로 코드와 데이터를 기밀로 유지하고 수정하지 않고 유지하는 데 도움이 됩니다.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix}} File Storage</td>
<td>파일 스토리지</td>
<td>[{{site.data.keyword.Bluemix_notm}} File Storage](/docs/infrastructure/FileStorage?topic=FileStorage-getting-started#getting-started)는 Kubernetes 지속적 볼륨을 사용하여 앱에 추가할 수 있는 지속적이고 빠르며 유연한 네트워크 연결 NFS 기반 파일 스토리지입니다. 워크로드의 요구사항을 충족하는 GB 크기와 IOPS를 사용하여 사전정의된 스토리지 계층 중에서 선택할 수 있습니다. 클러스터에서 파일 스토리지를 프로비저닝하는 방법에 대한 자세한 정보는 [{{site.data.keyword.Bluemix_notm}} File Storage에 데이터 저장](/docs/containers?topic=containers-file_storage#file_storage)을 참조하십시오.</td>
</tr>
<tr>
<td>{{site.data.keyword.keymanagementservicefull}}</td>
<td>데이터 암호화</td>
<td>{{site.data.keyword.keymanagementserviceshort}}를 사용으로 설정하여 클러스터에 있는 Kubernetes secret을 암호화하십시오. Kubernetes secret을 암호화하면 권한 없는 사용자가 민감한 클러스터 정보에 액세스하지 못하도록 할 수 있습니다.<br>설정하려면 <a href="/docs/containers?topic=containers-encryption#keyprotect">{{site.data.keyword.keymanagementserviceshort}}를 사용한 Kubernetes 시크릿 암호화</a>를 참조하십시오.<br>자세한 정보는 <a href="/docs/services/key-protect?topic=key-protect-getting-started-tutorial" target="_blank">{{site.data.keyword.keymanagementserviceshort}} 문서 <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>를 참조하십시오.</td>
</tr>
<tr>
<td>{{site.data.keyword.la_full}}</td>
<td>클러스터 및 앱 로그</td>
<td>팟(Pod) 컨테이너에서 로그를 관리하기 위해 LogDNA를 작업자 노드에 서드파티 서비스로 배치하여 로그 관리 기능을 클러스터에 추가합니다. 자세한 정보는 [LogDNA로 {{site.data.keyword.loganalysisfull_notm}}에서 Kubernetes 클러스터 로그 관리](/docs/services/Log-Analysis-with-LogDNA/tutorials?topic=LogDNA-kube#kube)를 참조하십시오.</td>
</tr>
<tr>
<td>{{site.data.keyword.mon_full}}</td>
<td>클러스터 및 앱 메트릭</td>
<td>메트릭을 {{site.data.keyword.monitoringlong}}에 전달하기 위해 Sysdig를 작업자 노드에 서드파티의 서비스로 배치하여 앱의 성능과 상태에 대한 작동 가시성을 얻을 수 있습니다. 자세한 정보는 [Kubernetes 클러스터에서 앱에 대한 메트릭 분석](/docs/services/Monitoring-with-Sysdig/tutorials?topic=Sysdig-kubernetes_cluster#kubernetes_cluster)을 참조하십시오. </td>
</tr>
<tr>
<td>{{site.data.keyword.cos_full}}</td>
<td>오브젝트 스토리지</td>
<td>{{site.data.keyword.cos_short}}에 저장된 데이터는 암호화되어 여러 지리적 위치에 분산되며, REST API를 사용하여 HTTP를 통해 액세스할 수 있습니다. [ibm-backup-restore 이미지](/docs/services/RegistryImages/ibm-backup-restore?topic=RegistryImages-ibmbackup_restore_starter) 를 사용하여 클러스터 내의 데이터에 대해 일회성 또는 스케줄된 백업을 작성하도록 서비스를 구성할 수 있습니다. 서비스에 대한 자세한 정보는 <a href="/docs/services/cloud-object-storage?topic=cloud-object-storage-about" target="_blank">{{site.data.keyword.cos_short}} 문서 <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>를 참조하십시오. </td>
</tr>
<tr>
<td>Istio on {{site.data.keyword.containerlong_notm}}</td>
<td>마이크로서비스 관리</td>
<td><a href="https://www.ibm.com/cloud/info/istio" target="_blank">Istio <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>는 클라우드 오케스트레이션 플랫폼에서 서비스 메시(service mesh)로도 알려진 마이크로서비스의 네트워크에 연결하고, 보안, 관리 및 모니터링하기 위한 방법을 개발자에게 제공하는 오픈 소스 서비스입니다. Istio on {{site.data.keyword.containerlong}}는 관리 추가 기능을 통해 클러스터에 Istio의 1단계 설치를 제공합니다. 한 번의 클릭으로 Istio 핵심 컴포넌트, 추가 추적, 모니터링 및 시각화를 모두 확보할 수 있고 BookInfo 샘플 앱을 시작하고 실행할 수 있습니다. 시작하려면 [관리 Istio 추가 기능(베타) 사용](/docs/containers?topic=containers-istio)을 참조하십시오.</td>
</tr>
<tr>
<td>Knative</td>
<td>서버리스 앱</td>
<td>[Knative ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/knative/docs)는 클러스터에 소스 중심으로 컨테이너화된 현대적 서버리스 앱을 작성할 수 있도록 Kubernetes의 기능을 확장하는 것을 목표로 IBM, Google, Pivotal, Red Hat, Cisco 등에서 개발한 오픈 소스 플랫폼입니다. 이 플랫폼은 프로그래밍 언어와 프레임워크 전반에서 일관된 접근 방식을 사용하여 개발자가 가장 중요한 것(소스 코드)에 집중할 수 있도록 Kubernetes에서 워크로드를 빌드, 배치 및 관리하는 데 따르는 운영 부담을 제거합니다. 자세한 정보는 [Knative를 사용한 서버리스 앱 배치](/docs/containers?topic=containers-serverless-apps-knative)를 참조하십시오. </td>
</tr>
<tr>
<td>Portworx</td>
<td>Stateful 앱을 위한 스토리지</td>
<td>[Portworx ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://portworx.com/products/introduction/)는 컨테이너화된 데이터베이스와 기타 stateful 앱에 대한 지속적 스토리지를 관리하거나 다중 구역 간 팟(Pod) 사이의 데이터를 공유하는 데 사용할 수 있는 고가용성 소프트웨어 정의 스토리지 솔루션입니다. Helm 차트에 Portworx를 설치하고 Kubernetes 지속적 볼륨을 사용하여 앱에 스토리지를 프로비저닝할 수 있습니다. 클러스터에 Portworx를 설정하는 방법에 대한 자세한 정보는 [Portworx를 사용하는 소프트웨어 정의 스토리지(SDS)에 데이터 저장](/docs/containers?topic=containers-portworx#portworx)을 참조하십시오.</td>
</tr>
<tr>
<td>Razee</td>
<td>배치 자동화</td>
<td>[Razee ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://razee.io/)는 클러스터, 환경 및 클라우드 제공자에 대한 Kubernetes 리소스 배치를 자동화 및 관리하며, 사용자가 롤아웃 프로세스를 모니터하고 배치 문제를 더 빨리 찾을 수 있도록 리소스의 배치 정보를 시각화하는 데 도움을 주는 오픈 소스 프로젝트입니다. Razee에 대한 자세한 정보 및 배치 프로세스를 자동화하기 위해 클러스터에 Razee를 설정하는 방법에 대한 자세한 정보는 [Razee 문서 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/razee-io/Razee)를 참조하십시오. </td>
</tr>
</tbody>
</table>

<br />


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
<td>컨테이너의 지속적 통합 및 지속적 딜리버리를 위해 <a href="https://codeship.com" target="_blank">Codeship <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>을 사용할 수 있습니다. 자세한 정보는 <a href="https://www.ibm.com/blogs/bluemix/2017/10/using-codeship-pro-deploy-workloads-ibm-container-service/" target="_blank">Codeship Pro를 사용하여 {{site.data.keyword.containerlong_notm}}에 워크로드 배치 <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>를 참조하십시오. </td>
</tr>
<tr>
<td>Grafeas</td>
<td>[Grafeas ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://grafeas.io)는 소프트웨어 공급 체인 프로세스 동안 메타데이터를 검색, 저장 및 교환하는 일반적인 방법을 제공하는 오픈 소스 CI/CD 서비스입니다. 예를 들어 Grafeas를 앱 빌드 프로세스에 통합하는 경우 Grafeas는 앱이 프로덕션에 배포될 때 정보에 입각한 결정을 내릴 수 있도록 빌드 요청의 개시자, 취약성 스캔 결과 및 품질 보증 사인오프에 대한 정보를 저장할 수 있습니다. 감사에서 이 메타데이터를 사용하거나 소프트웨어 공급 체인에 대한 규제 준수를 증명할 수 있습니다. </td>
</tr>
<tr>
<td>Helm</td>
<td> <a href="https://helm.sh" target="_blank">Helm <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>은 Kubernetes 패키지 관리자입니다. {{site.data.keyword.containerlong_notm}} 클러스터에서 실행되는 복잡한 Kubernetes 애플리케이션을 정의, 설치 및 업그레이드하기 위해 새 Helm 차트를 작성하거나 기존 Helm 차트를 사용할 수 있습니다. <p>자세한 정보는 [{{site.data.keyword.containerlong_notm}}에서 Helm 설정](/docs/containers?topic=containers-helm)을 참조하십시오. </p></td>
</tr>
<tr>
<td>{{site.data.keyword.contdelivery_full}}</td>
<td>도구 체인을 사용하여 Kubernetes 클러스터에 대한 컨테이너 배치 및 앱 빌드를 자동화합니다. 설정에 대한 자세한 정보는 블로그 <a href="https://developer.ibm.com/recipes/tutorials/deploy-kubernetes-pods-to-the-bluemix-container-service-using-devops-pipelines/" target="_blank">Deploy Kubernetes pods to the {{site.data.keyword.containerlong_notm}} using DevOps Pipelines <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>를 참조하십시오. </td>
</tr>
<tr>
<td>Istio on {{site.data.keyword.containerlong_notm}}</td>
<td><a href="https://www.ibm.com/cloud/info/istio" target="_blank">Istio <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>는 클라우드 오케스트레이션 플랫폼에서 서비스 메시(service mesh)로도 알려진 마이크로서비스의 네트워크에 연결하고, 보안, 관리 및 모니터링하기 위한 방법을 개발자에게 제공하는 오픈 소스 서비스입니다. Istio on {{site.data.keyword.containerlong}}는 관리 추가 기능을 통해 클러스터에 Istio의 1단계 설치를 제공합니다. 한 번의 클릭으로 Istio 핵심 컴포넌트, 추가 추적, 모니터링 및 시각화를 모두 확보할 수 있고 BookInfo 샘플 앱을 시작하고 실행할 수 있습니다. 시작하려면 [관리 Istio 추가 기능(베타) 사용](/docs/containers?topic=containers-istio)을 참조하십시오.</td>
</tr>
<tr>
<td>Jenkins X</td>
<td>Jenkins X는 빌드 프로세스를 자동화하는 데 사용할 수 있는 Kubernetes 고유 지속적 통합 및 지속적 딜리버리 플랫폼입니다. {{site.data.keyword.containerlong_notm}}에 Jenkins X를 설치하는 방법에 대한 자세한 정보는 [Jenkins X 오픈 소스 프로젝트 소개 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/blogs/bluemix/2018/08/installing-jenkins-x-on-ibm-cloud-kubernetes-service/)를 참조하십시오.</td>
</tr>
<tr>
<td>Knative</td>
<td>[Knative ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/knative/docs)는 클러스터에 소스 중심으로 컨테이너화된 현대적 서버리스 앱을 작성할 수 있도록 Kubernetes의 기능을 확장하는 것을 목표로 IBM, Google, Pivotal, Red Hat, Cisco 등에서 개발한 오픈 소스 플랫폼입니다. 이 플랫폼은 프로그래밍 언어와 프레임워크 전반에서 일관된 접근 방식을 사용하여 개발자가 가장 중요한 것(소스 코드)에 집중할 수 있도록 Kubernetes에서 워크로드를 빌드, 배치 및 관리하는 데 따르는 운영 부담을 제거합니다. 자세한 정보는 [Knative를 사용한 서버리스 앱 배치](/docs/containers?topic=containers-serverless-apps-knative)를 참조하십시오. </td>
</tr>
<tr>
<td>Razee</td>
<td>[Razee ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://razee.io/)는 클러스터, 환경 및 클라우드 제공자에 대한 Kubernetes 리소스 배치를 자동화 및 관리하며, 사용자가 롤아웃 프로세스를 모니터하고 배치 문제를 더 빨리 찾을 수 있도록 리소스의 배치 정보를 시각화하는 데 도움을 주는 오픈 소스 프로젝트입니다. Razee에 대한 자세한 정보 및 배치 프로세스를 자동화하기 위해 클러스터에 Razee를 설정하는 방법에 대한 자세한 정보는 [Razee 문서 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/razee-io/Razee)를 참조하십시오. </td>
</tr>
</tbody>
</table>

<br />


## 하이브리드 클라우드 서비스
{: #hybrid_cloud_services}

<table summary="이 표는 클러스터를 온프레미스 데이터 센터에 연결하는 데 사용할 수 있는 서비스를 보여줍니다. 행은 왼쪽에서 오른쪽으로 읽어야 하며 1열에는 서비스의 이름, 2열에는 서비스에 대한 설명이 있습니다.">
<caption>하이브리드 클라우드 서비스</caption>
<thead>
<tr>
<th>서비스</th>
<th>설명</th>
</tr>
</thead>
<tbody>
  <tr>
    <td>{{site.data.keyword.BluDirectLink}}</td>
    <td>[{{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-about-ibm-cloud-direct-link)는 사용자가 공용 인터넷을 통한 라우팅 없이 원격 네트워크 환경과 {{site.data.keyword.containerlong_notm}} 간에 개인용 직접 연결을 작성할 수 있게 해 줍니다. {{site.data.keyword.Bluemix_notm}} Direct Link 오퍼링은 하이브리드 워크로드, 교차 제공자 워크로드, 대규모 또는 빈번한 데이터 전송자 또는 개인용 워크로드를 구현해야 하는 경우 유용합니다. {{site.data.keyword.Bluemix_notm}} Direct Link 오퍼링을 선택하고 {{site.data.keyword.Bluemix_notm}} Direct Link 연결을 설정하려면 {{site.data.keyword.Bluemix_notm}} Direct Link 문서의 [{{site.data.keyword.Bluemix_notm}} Direct Link 시작하기](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link#how-do-i-know-which-type-of-ibm-cloud-direct-link-i-need-)를 참조하십시오. </td>
  </tr>
<tr>
  <td>strongSwan IPSec VPN 서비스</td>
  <td>Kubernetes 클러스터를 온프레미스 네트워크에 안전하게 연결하는 [strongSwan IPSec VPN 서비스 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.strongswan.org/about.html)를 설정하십시오. strongSwan IPSec VPN 서비스는 업계 표준 IPSec(Internet Protocol Security) 프로토콜 스위트를 기반으로 하는 인터넷 상의 엔드-투-엔드 보안 통신 채널을 제공합니다. 클러스터와 온프레미스 네트워크 간의 보안 연결을 설정하려면 클러스터 내의 팟(Pod)에 직접 [strongSwan IPSec VPN 서비스를 구성하고 배치](/docs/containers?topic=containers-vpn#vpn-setup)하십시오.</td>
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
<td>Grafana를 통해 로그를 분석하여 클러스터에서 수행된 관리 활동을 모니터합니다. 서비스에 대한 자세한 정보는 [활동 트래커](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started) 문서를 참조하십시오. 추적할 수 있는 이벤트의 유형에 대한 자세한 정보는 [활동 트래커 이벤트](/docs/containers?topic=containers-at_events)를 참조하십시오.</td>
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
<td>Prometheus는 Kubernetes에 맞게 디자인된 오픈 소스 모니터링, 로깅 및 경보 도구입니다. Prometheus는 Kubernetes 로깅 정보를 기반으로 클러스터, 작업자 노드 및 배치 상태에 대한 자세한 정보를 검색합니다. 클러스터에서 실행 중인 각 컨테이너에 대한 CPU, 메모리, I/O 및 네트워크 활동이 수집됩니다. 사용자 정의 조회 또는 경보에서 수집된 데이터를 사용하여 클러스터의 성능 및 워크로드를 모니터링할 수 있습니다.

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
<td>[Weave Scope ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.weave.works/oss/scope/)는 Kubernetes 클러스터 내의 리소스(서비스, 팟(Pod), 컨테이너, 프로세스, 노드 등 포함)에 대한 시각적 다이어그램을 제공합니다. Weave Scope는 CPU 및 메모리에 대한 대화식 메트릭을 제공하며 컨테이너로 tail 및 exec를 실행하기 위한 도구도 제공합니다.</li></ol>
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
  <tr>
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
  <td>[{{site.data.keyword.Bluemix_notm}} Block Storage](/docs/infrastructure/BlockStorage?topic=BlockStorage-getting-started#getting-started)는 Kubernetes 지속적 볼륨(PV)을 사용하여 앱에 추가할 수 있는 지속적, 고성능 iSCSI 스토리지입니다. 블록 스토리지를 사용하여 단일 구역에 Stateful 앱을 배치하거나, 블록 스토리지를 단일 팟(Pod)에 대한 고성능 스토리지로 사용하십시오. 클러스터에서 블록 스토리지를 프로비저닝하는 방법에 대한 자세한 정보는 [{{site.data.keyword.Bluemix_notm}} Block Storage에 데이터 저장](/docs/containers?topic=containers-block_storage#block_storage)을 참조하십시오.,</td>
  </tr>
<tr>
  <td>{{site.data.keyword.cos_full}}</td>
  <td>{{site.data.keyword.cos_short}}에 저장된 데이터는 암호화되어 여러 지리적 위치에 분산되며, REST API를 사용하여 HTTP를 통해 액세스할 수 있습니다. [ibm-backup-restore 이미지](/docs/services/RegistryImages/ibm-backup-restore?topic=RegistryImages-ibmbackup_restore_starter) 를 사용하여 클러스터 내의 데이터에 대해 일회성 또는 스케줄된 백업을 작성하도록 서비스를 구성할 수 있습니다. 서비스에 대한 자세한 정보는 <a href="/docs/services/cloud-object-storage?topic=cloud-object-storage-about" target="_blank">{{site.data.keyword.cos_short}} 문서 <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>를 참조하십시오. </td>
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
  <td>다양한 {{site.data.keyword.Bluemix_notm}} 데이터베이스 서비스(예: {{site.data.keyword.composeForMongoDB_full}} 또는 {{site.data.keyword.cloudantfull}}) 중에서 원하는 항목을 선택하여 클러스터에 확장 가능한 고가용성 데이터베이스 솔루션을 배치할 수 있습니다. 사용 가능한 클라우드 데이터베이스 목록은 [{{site.data.keyword.Bluemix_notm}} 카탈로그 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/catalog?category=databases)를 참조하십시오.  </td>
  </tr>
  </tbody>
  </table>
