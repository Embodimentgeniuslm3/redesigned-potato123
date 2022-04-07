---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-11"

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
{:note: .note}


# 버전 정보 및 업데이트 조치
{: #cs_versions}

## Kubernetes 버전 유형
{: #version_types}

{{site.data.keyword.containerlong}}는 동시에 여러 Kubernetes 버전을 지원합니다. 최신 버전이 릴리스될 때 최대 두 개의 이전 버전이(n-2)이 지원됩니다. 최신 버전보다 세 개 이상 이전인 버전(n-3)은 먼저 더 이상 사용되지 않게 되고 그런 다음 지원되지 않게 됩니다.
{:shortdesc}

**지원되는 Kubernetes 버전**:
*   최신: 1.14.2 
*   기본값: 1.13.6
*   기타: 1.12.9

**더 이상 사용되지 않거나 지원되지 않는 Kubernetes 버전**:
*   더 이상 사용되지 않음: 1.11
*   지원되지 않음: 1.5, 1.7, 1.8, 1.9, 1.10 

</br>

**더 이상 사용되지 않는 버전**: 더 이상 사용되지 않는 Kubernetes에서 클러스터가 실행되는 경우, 버전 지원이 중단되기 전에 지원되는 Kubernetes 버전을 검토하고 이로 업데이트할 수 있도록 최소 30일이 제공됩니다. 폐기 기간 중에도 클러스터는 작동하지만, 보안 취약성을 수정하기 위해 지원되는 릴리스로 업데이트해야 할 수 있습니다. 예를 들어, 작업자 노드를 추가하고 다시 로드할 수 있으나 지원되지 않는 날짜가 30일 미만인 경우 지원되지 않는 버전을 사용하는 새 클러스터를 작성할 수는 없습니다. 

**지원되지 않는 버전**: 클러스터가 지원되지 않는 Kubernetes 버전을 실행하는 경우에는 다음의 잠재적인 업데이트 영향을 검토한 후에 즉시 [클러스터를 업데이트](/docs/containers?topic=containers-update#update)하여 중요한 보안 업데이트 및 지원을 계속 받으십시오. 지원되지 않는 클러스터는 기존 작업자 노드를 추가하거나 다시 로드할 수 없습니다. `ibmcloud ks clusters` 명령의 출력에 있는 **상태** 필드 또는 [{{site.data.keyword.containerlong_notm}} 콘솔 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/kubernetes/clusters)을 검토하여 클러스터가 **지원되지 않는**지 확인할 수 있습니다.

클러스터가 지원되는 버전 이전의 3개 이상의 부 버전일 때까지 기다리면 클러스터를 업데이트할 수 없습니다. 대신 [새 클러스터 작성](/docs/containers?topic=containers-clusters#clusters)하고, 새 클러스터에 [앱을 배치](/docs/containers?topic=containers-app#app)하고 지원되지 않는 클러스터를 [삭제](/docs/containers?topic=containers-remove)하십시오.<br><br>이 문제를 방지하려면 더 이상 사용되지 않는 클러스터를 현재 버전에 앞선 3개 미만의 지원되는 버전(예: 1.11에서 1.12)으로 업데이트한 다음 최신 버전은 1.1.4로 업데이트하십시오. 작업자 노드가 마스터 뒤의 세 개의 이상의 버전을 실행하는 경우 작업자 노드를 마스터와 동일한 버전으로 업데이트할 때까지 `MatchNodeSelector`, `CrashLoopBackOff` 또는 `ContainerCreating`과 같은 상태를 입력하여 팟(Pod)의 실패가 표시될 수 있습니다. 더 이상 사용되지 않는 버전에서 지원되는 버전으로 업데이트한 후에는 클러스터가 정상 오퍼레이션을 재개하고 지원을 계속 받을 수 있습니다.
{: important}

</br>

클러스터의 서버 버전을 확인하려면 다음 명령을 실행하십시오.
```
kubectl version  --short | grep -i server
```
{: pre}

출력 예:
```
Server Version: v1.13.6+IKS
```
{: screen}


## 업데이트 유형
{: #update_types}

Kubernetes 클러스터에 대한 업데이트에는 세 가지 유형(주 버전, 부 버전 및 패치)가 있습니다.
{:shortdesc}

|업데이트 유형|버전 레이블의 예|업데이트 수행자|영향
|-----|-----|-----|-----|
|주 버전|1.x.x|사용자|스크립트 또는 배치를 포함한 클러스터의 오퍼레이션 변경.|
|부 버전|x.9.x|사용자|스크립트 또는 배치를 포함한 클러스터의 오퍼레이션 변경.|
|패치|x.x.4_1510|IBM 및 사용자|Kubernetes 패치 및 기타 {{site.data.keyword.Bluemix_notm}} Provider 컴포넌트 업데이트(보안 및 운영 체제 패치 등)입니다. 마스터는 IBM이 자동으로 업데이트하지만 작업자 노드에는 사용자가 패치를 적용해야 합니다. 패치에 대한 자세한 정보는 다음 섹션을 참조하십시오.|
{: caption="Kubernetes 업데이트의 영향" caption-side="top"}

업데이트가 사용 가능한 경우에는 `ibmcloud ks workers --cluster <cluster>` 또는 `ibmcloud ks worker-get --cluster <cluster> --worker <worker>` 명령 등을 사용하여 작업자 노드에 대한 정보를 볼 때 알림을 받습니다.
-  **주 버전 및 부 버전 업데이트(1.x)**: 먼저 [마스터 노드를 업데이트](/docs/containers?topic=containers-update#master)한 후 [작업자 노드를 업데이트](/docs/containers?topic=containers-update#worker_node)하십시오. 작업자 노드는 마스터보다 높은 Kubernetes 주 또는 부 버전을 실행할 수 없습니다.
   - Kubernetes 마스터를 3개 이상의 부 버전 이후로 업데이트할 수 없습니다. 예를 들어, 현재 마스터가 버전 1.11인데 1.14로 업데이트하려면 먼저 1.12로 업데이트해야 합니다. 
   - 클러스터의 최소 `major.minor` CLI 버전과 일치하는 `kubectl` CLI 버전을 사용하는 경우 예상치 못한 결과가 발생할 수 있습니다. Kubernetes 클러스터 및 [CLI 버전](/docs/containers?topic=containers-cs_cli_install#kubectl)을 최신 상태로 유지해야 합니다.
-  **패치 업데이트(x.x.4_1510)**: 패치 간 변경사항은 [버전 변경 로그](/docs/containers?topic=containers-changelog)에 기록되어 있습니다. 마스터 패치는 자동으로 적용되지만 사용자가 작업자 노드 패치 업데이트를 시작합니다. 작업자 노드도 마스터보다 높은 패치 버전을 실행할 수 있습니다. 업데이트가 사용 가능한 경우에는 `ibmcloud ks clusters`, `cluster-get`, `workers` 또는 `worker-get` 등의 명령으로 {{site.data.keyword.Bluemix_notm}} 콘솔 또는 CLI에서 마스터 및 작업자 노드에 대한 정보를 볼 때 알림을 받습니다.
   - **작업자 노드 패치**: 업데이트가 사용 가능한지 매월 확인하고, `ibmcloud ks worker-update` [명령](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update) 또는 `ibmcloud ks worker-reload` [명령](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)을 사용하여 이러한 보안 및 운영 체제 패치를 적용하십시오. 업데이트 또는 다시 로드 중 작업자 노드 머신이 다시 이미징되며, [작업자 노드 외부에 저장](/docs/containers?topic=containers-storage_planning#persistent_storage_overview)되지 않은 경우 데이터가 삭제됩니다.
   - **마스터 패치**: 마스터 패치는 며칠에 걸쳐 자동으로 적용되므로, 마스터에 적용되기 전에는 마스터 패치 버전이 사용 가능으로 표시되지 않을 수 있습니다. 업데이트 자동화는 비정상 상태이거나 현재 오퍼레이션이 진행 중인 클러스터 또한 건너뜁니다. 마스터가 한 부 버전에서 다른 부 버전으로 업데이트되는 경우에만 필요한 패치와 같은 특정 마스터 수정팩에 대해서는 IBM에서 자동 업데이트를 사용 안함으로 설정할 수 있습니다(변경 로그에 기록되어 있음). 이러한 경우에는 업데이트 자동화가 적용되기를 기다릴 필요 없이 직접 `ibmcloud ks cluster-update` [명령](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_update)을 사용할 수 있습니다.

</br>

{: #prep-up}
이 정보에는 클러스터를 이전 버전에서 새 버전으로 업데이트할 때 배치된 앱에 영향을 미칠 수 있는 업데이트가 요약되어 있습니다.
-  버전 1.14 [준비 조치](#cs_v114)
-  버전 1.13 [준비 조치](#cs_v113)
-  버전 1.12 [준비 조치](#cs_v112)
-  **더 이상 사용되지 않음**: 버전 1.11 [준비 조치](#cs_v111)
-  지원되지 않는 버전의 [아카이브](#k8s_version_archive)

<br/>

전체 변경사항 목록은 다음 정보를 검토하십시오.
* [Kubernetes 변경 로그 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG.md).
* [IBM 버전 변경 로그](/docs/containers?topic=containers-changelog).

</br>

## 릴리스 히스토리
{: #release-history}

다음 표에는 {{site.data.keyword.containerlong_notm}} 버전 릴리스 히스토리가 기록되어 있습니다. 특정 릴리스가 지원되지 않을 수 있는 일반적인 시간 범위를 예상하는 등 플랜 용도로 이 정보를 사용할 수 있습니다. Kubernetes 커뮤니티에서 버전 업데이트를 릴리스하면 IBM 팀은 {{site.data.keyword.containerlong_notm}} 환경에 대해 릴리스를 강화하고 테스트하는 프로세스를 시작합니다. 가용성 및 지원되지 않는 릴리스 날짜는 이러한 테스트의 결과, 커뮤니티 업데이트, 보안 패치 및 버전 간의 기술 변경사항에 따라 다릅니다. `n-2` 버전 지원 정책에 따라 클러스터 마스터 및 작업자 노드 버전을 최신 상태로 유지하도록 계획하십시오.
{: shortdesc}

일반적으로 {{site.data.keyword.containerlong_notm}}는 처음 Kubernetes 버전 1.5에서 사용할 수 있었습니다. 계획된 릴리스 또는 지원되지 않는 날짜는 변경될 수 있습니다. 버전 업데이트 준비 단계로 이동하려면 버전 번호를 클릭하십시오.

단검으로 표시된 날짜(`†`)는 잠정적이며 변경될 수 있습니다.
{: important}

<table summary="다음 표는 {{site.data.keyword.containerlong_notm}}에 대한 릴리스 히스토리를 보여줍니다.">
<caption>{{site.data.keyword.containerlong_notm}}에 대한 릴리스 히스토리</caption>
<col width="20%" align="center">
<col width="20%">
<col width="30%">
<col width="30%">
<thead>
<tr>
<th>지원 여부</th>
<th>버전</th>
<th>{{site.data.keyword.containerlong_notm}}<br>릴리스 날짜</th>
<th>{{site.data.keyword.containerlong_notm}}<br>지원되지 않는 날짜</th>
</tr>
</thead>
<tbody>
<tr>
  <td><img src="images/checkmark-filled.png" align="left" width="32" style="width:32px;" alt="이 버전은 지원됩니다."/></td>
  <td>[1.14](#cs_v114)</td>
  <td>2019년 5월 7일</td>
  <td>2020년 3월 `†`</td>
</tr>
<tr>
  <td><img src="images/checkmark-filled.png" align="left" width="32" style="width:32px;" alt="이 버전은 지원됩니다."/></td>
  <td>[1.13](#cs_v113)</td>
  <td>2019년 2월 5일</td>
  <td>2019년 12월 `†`</td>
</tr>
<tr>
  <td><img src="images/checkmark-filled.png" align="left" width="32" style="width:32px;" alt="이 버전은 지원됩니다."/></td>
  <td>[1.12](#cs_v112)</td>
  <td>2018년 11월 7일</td>
  <td>2019년 9월 `†`</td>
</tr>
<tr>
  <td><img src="images/warning-filled.png" align="left" width="32" style="width:32px;" alt="이 버전은 더 이상 사용되지 않습니다."/></td>
  <td>[1.11](#cs_v111)</td>
  <td>2018년 8월 14일</td>
  <td>2019년 6월 27일`†`</td>
</tr>
<tr>
  <td><img src="images/close-filled.png" align="left" width="32" style="width:32px;" alt="이 버전은 지원되지 않습니다."/></td>
  <td>[1.10](#cs_v110)</td>
  <td>2018년 5월 1일</td>
  <td>2019년 5월 16일</td>
</tr>
<tr>
  <td><img src="images/close-filled.png" align="left" width="32" style="width:32px;" alt="이 버전은 지원되지 않습니다."/></td>
  <td>[1.9](#cs_v19)</td>
  <td>2018년 2월 8일</td>
  <td>2018년 12월 27일</td>
</tr>
<tr>
  <td><img src="images/close-filled.png" align="left" width="32" style="width:32px;" alt="이 버전은 지원되지 않습니다."/></td>
  <td>[1.8](#cs_v18)</td>
  <td>2008년 11월 8일</td>
  <td>2018년 9월 22일</td>
</tr>
<tr>
  <td><img src="images/close-filled.png" align="left" width="32" style="width:32px;" alt="이 버전은 지원되지 않습니다."/></td>
  <td>[1.7](#cs_v17)</td>
  <td>2017년 9월 19일</td>
  <td>2018년 6월 21일</td>
</tr>
<tr>
  <td><img src="images/close-filled.png" align="left" width="32" style="width:32px;" alt="이 버전은 지원되지 않습니다."/></td>
  <td>1.6</td>
  <td>해당사항 없음</td>
  <td>해당사항 없음</td>
</tr>
<tr>
  <td><img src="images/close-filled.png" align="left" width="32" style="width:32px;" alt="이 버전은 지원되지 않습니다."/></td>
  <td>[1.5](#cs_v1-5)</td>
  <td>2017년 5월 23일</td>
  <td>2018년 4월 4일</td>
</tr>
</tbody>
</table>

<br />


## 버전 114
{: #cs_v114}

<p><img src="images/certified_kubernetes_1x14.png" style="padding-right: 10px;" align="left" alt="이 배지는 {{site.data.keyword.containerlong_notm}}에 대한 Kubernetes 버전 1.14 인증을 표시합니다."/> {{site.data.keyword.containerlong_notm}}는 CNCF Kubernetes Software Conformance Certification 프로그램에서 버전 1.14에 대해 인증된 Certified Kubernetes 제품입니다. _Kubernetes®는 미국 또는 기타 국가에서 사용되는 Linux Foundation의 등록상표이며, Linux Foundation의 라이센스에 따라 사용됩니다. _</p>

이전 Kubernetes 버전에서 1.14로 업데이트할 때 변경해야 할 사항을 검토합니다.
{: shortdesc}

Kubernetes 1.14는 사용자가 탐색할 수 있는 새 기능을 도입했습니다. Kubernetes 리소스 YAML 구성을 작성, 사용자 정의 및 재사용하는 데 사용할 수 있는 새 [`kustomize` 제품 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes-sigs/kustomize)을 사용해 보십시오. 또는 새 [`kubectl` CLI 문서 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubectl.docs.kubernetes.io/)를 살펴보십시오.
{: tip}

### 마스터 이전 업데이트
{: #114_before}

다음 표는 Kubernetes 마스터를 업데이트하기 전에 수행해야 하는 조치를 보여줍니다.
{: shortdesc}

<table summary="버전 1.14에 대한 Kubernetes 업데이트">
<caption>마스터를 Kubernetes 1.14로 업데이트한 전에 작성할 변경사항</caption>
<thead>
<tr>
<th>유형</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>CRI 팟(Pod) 로그 디렉토리 구성 변경</td>
<td>컨테이너 런타임 인터페이스(CRI)는 `/var/log/pods/<UID>`에서 `/var/log/pods/<NAMESPACE_NAME_UID>`로 팟(Pod) 로그 디렉토리 구조를 변경했습니다. 앱이 작업자 노드에서 직접 팟(Pod) 로그에 액세스하도록 Kubernetes 및 CRI를 우회하는 경우 업데이트하여 두 디렉토리 구조를 처리하십시오. 예를 들어, `kubectl logs`를 실행하여 Kubernetes를 통해 팟(Pod) 로그에 액세스해도 이 변경에 영향을 주지 않습니다. </td>
</tr>
<tr>
<td>시스템 상태 검사가 더 이상 경로 재지정을 수행하지 않음</td>
<td>`HTTPGetAction`을 사용하는 활동 상태 및 준비 상태 프로브는 더 이상 원래의 프로브 요청과 다른 호스트 이름에 대한 경로 재지정을 수행하지 않습니다. 대신, 로컬이 아닌 경로 재지정은 `Success` 응답을 리턴하며 `ProbeWarning`의 이유가 있는 이벤트가 경로 재지정이 무시되었음을 표시하도록 생성됩니다. 이전에 다른 호스트 이름 엔드포인트에 대한 상태 검사를 실행하도록 경로 재지정에 의존한 경우 `kubelet` 외부에 상태 검사 로직을 수행해야 합니다. 예를 들어, 프로브 요청 대신 외부 엔드포인트를 프록시할 수 있습니다. </td>
</tr>
<tr>
<td>지원되지 않음: KubeDNS 클러스터 DNS 제공자</td>
<td>CoreDNS는 이제 Kubernetes 버전 1.14 이상을 실행하는 클러스터를 위해 지원되는 유일한 클러스터 DNS 제공자입니다. KubeDNS를 클러스터 DNS 제공자로 사용하는 기존 클러스터를 버전 1.14로 업데이트하면 KubeDNS는 업데이트 중에 CoreDNS로 자동 마이그레이션됩니다. 그러므로 클러스터를 업데이트하기 전에 [CoreDNS를 클러스터 DNS 제공자로 설정](/docs/containers?topic=containers-cluster_dns#set_coredns)한 후 테스트할 것을 고려해보십시오. <br><br>CoreDNS는 도메인 이름을 Kubernetes 서비스 `ExternalName` 필드로 입력하도록 [클러스터 DNS 스펙 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/dns/blob/master/docs/specification.md#25---records-for-external-name-services)을 지원합니다. 이전 클러스터 DNS 제공자인 KubeDNS는 클러스터 DNS 스펙을 따르지 않으므로 `ExternalName`의 IP 주소를 허용합니다. Kubernetes 서비스가 DNS 대신 IP 주소를 사용하고 있는 경우 기능을 계속 사용하려면 `ExternalName`을 DNS에 업데이트해야 합니다.</td>
</tr>
<tr>
<td>지원되지 않음: Kubernetes `Initializers` 알파 기능</td>
<td>Kubernetes `Initializers` 알파 기능인 `admissionregistration.k8s.io/v1alpha1` API 버전, `Initializers` 허가 제어 플러그인 및 `metadata.initializers` API 필드 사용이 제거됩니다. `Initializers`를 사용하는 경우 [Kubernetes admission webhooks ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/reference/access-authn-authz/extensible-admission-controllers/)를 사용하고 클러스터를 업데이트하기 전에 기존 `InitializerConfiguration` API 오브젝트를 삭제하도록 전환하십시오. </td>
</tr>
<tr>
<td>지원되지 않음: 노드 알파 오염</td>
<td>오염 `node.alpha.kubernetes.io/notReady` 및`node.alpha.kubernetes.io/unreachable`의 사용은 더 이상 지원되지 않습니다. 이 오염에 의존하는 경우 `node.kubernetes.io/not-ready` 및 `node.kubernetes.io/unreachable` 오염을 대신 사용하도록 앱을 업데이트하십시오.</td>
</tr>
<tr>
<td>지원되지 않음: Kubernetes API Swagger 문서</td>
<td>`swagger/*`, `/swagger.json` 및 `/swagger-2.0.0.pb-v1` 스키마 API 문서는 이제 `/openapi/v2` 스키마 API 문서를 위해 제거되었습니다. OpenAPI 문서가 Kubernetes 버전 1.10에서 사용 가능할 때 Swagger 문서는 더 이상 사용되지 않는 상태였습니다. 또한 Kubernetes API 서버는 이제 집계된 API 서버의 `/openapi/v2` 엔드포인트에서 OpenAPI 스키마만 집계합니다. `/swagger.json`에서 집계하기 위한 폴백이 제거됩니다. Kubernetes API 확장기능을 제공하는 앱을 설치한 경우 앱이 `/openapi/v2` 스키마 API 문서를 지원하는지 확인하십시오. </td>
</tr>
<tr>
<td>지원되지 않으며 더 이상 사용되지 않음: 메트릭 선택</td>
<td>[제거되었으며 더 이상 사용되지 않는 Kubernetes 메트릭 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG-1.14.md#removed-and-deprecated-metrics)을 검토하십시오. 더 이상 사용되지 않는 메트릭을 사용하는 경우 사용 가능한 대체 메트릭으로 변경하십시오. </td>
</tr>
</tbody>
</table>

### 마스터 이후 업데이트
{: #114_after}

다음 표는 Kubernetes 마스터를 업데이트한 후에 수행해야 하는 조치를 보여줍니다.
{: shortdesc}

<table summary="버전 1.14에 대한 Kubernetes 업데이트">
<caption>마스터를 Kubernetes 1.14로 업데이트한 후에 작성할 변경사항</caption>
<thead>
<tr>
<th>유형</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>지원되지 않음: `kubectl --show-all`</td>
<td>`--show-all` 및 약식 `-a` 플래그가 더 이상 사용되지 않습니다. 스크립트가 이 플래그에 의존하는 경우 스크립트를 업데이트하십시오.</td>
</tr>
<tr>
<td>인증되지 않은 사용자를 위한 Kubernetes 기본 RBAC 정책</td>
<td>Kubernetes 기반 역할 기반 액세스 제어(RBAC) 정책에서는 더 이상 [권한이 없는 사용자에 대한 검색 및 권한 검사 API ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#discovery-roles)에 대한 액세스 권한을 부여하지 않습니다. 이 변경사항은 새 버전 1.14 클러스터에만 적용됩니다. 이전 버전에서 클러스터를 업데이트하는 경우 권한이 없는 사용자는 계속해서 검색 및 권한 검사 API에 액세스할 수 있습니다. 인증되지 않은 사용자를 위해 보안이 강화된 기본값으로 업데이트할 경우 `system:basic-user` 및 `system:discovery` 클러스터 역할 바인딩에서 `system:unauthenticated` 그룹을 제거하십시오. </td>
</tr>
<tr>
<td>더 이상 사용되지 않음: `pod_name` 및 `container_name` 레이블을 사용하는 Prometheus 정책</td>
<td>대신 `pod` 또는 `container` 레이블을 사용하도록 `pod_name` 또는 `container_name` 레이블과 일치하는 Prometheus 조회를 업데이트하십시오. 더 이상 사용되지 않는 레이블을 사용할 수 있는 조회 예제에는 kubelet 프로브 메트릭이 포함됩니다. 더 이상 사용되지 않는 `pod_name` 및 `container_name` 레이블은 다음 Kubernetes 릴리스에서 지원되지 않습니다. </td>
</tr>
</tbody>
</table>

<br />


## 버전 1.13
{: #cs_v113}

<p><img src="images/certified_kubernetes_1x13.png" style="padding-right: 10px;" align="left" alt="이 배지는 {{site.data.keyword.containerlong_notm}}에 대한 Kubernetes 버전 1.13 인증을 표시합니다."/> {{site.data.keyword.containerlong_notm}}는 CNCF Kubernetes Software Conformance Certification 프로그램에서 버전 1.13에 대해 인증된 Kubernetes 제품입니다. _Kubernetes®는 미국 또는 기타 국가에서 사용되는 Linux Foundation의 등록상표이며, Linux Foundation의 라이센스에 따라 사용됩니다. _</p>

이전 Kubernetes 버전에서 1.13로 업데이트할 때 변경해야 할 사항을 검토하십시오.
{: shortdesc}

### 마스터 이전 업데이트
{: #113_before}

다음 표는 Kubernetes 마스터를 업데이트하기 전에 수행해야 하는 조치를 보여줍니다.
{: shortdesc}

<table summary="버전 1.13에 대한 Kubernetes 업데이트">
<caption>마스터를 Kubernetes 1.13으로 업데이트하기 전에 작성할 변경사항</caption>
<thead>
<tr>
<th>유형</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>해당사항 없음</td>
<td></td>
</tr>
</tbody>
</table>

### 마스터 이후 업데이트
{: #113_after}

다음 표는 Kubernetes 마스터를 업데이트한 후에 수행해야 하는 조치를 보여줍니다.
{: shortdesc}

<table summary="버전 1.13에 대한 Kubernetes 업데이트">
<caption>마스터를 Kubernetes 1.13으로 업데이트한 후 작성할 변경사항</caption>
<thead>
<tr>
<th>유형</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>새 기본 클러스터 DNS 제공자로 사용 가능한 CoreDNS</td>
<td>CoreDNS가 이제 Kubernetes 1.13 이상에서 새 클러스터의 기본 클러스터 DNS 제공자입니다. KubeDNS를 클러스터 DNS 제공자로 사용하는 기존 클러스터를 1.13으로 업데이트하면 KubeDNS가 계속 클러스터 DNS 제공자입니다. 그러나 [대신 CoreDNS를 사용](/docs/containers?topic=containers-cluster_dns#dns_set)하도록 선택할 수 있습니다.
<br><br>CoreDNS는 도메인 이름을 Kubernetes 서비스 `ExternalName` 필드로 입력하도록 [클러스터 DNS 스펙 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/dns/blob/master/docs/specification.md#25---records-for-external-name-services)을 지원합니다. 이전 클러스터 DNS 제공자인 KubeDNS는 클러스터 DNS 스펙을 따르지 않으므로 `ExternalName`의 IP 주소를 허용합니다. Kubernetes 서비스가 DNS 대신 IP 주소를 사용하고 있는 경우 기능을 계속 사용하려면 `ExternalName`을 DNS에 업데이트해야 합니다.</td>
</tr>
<tr>
<td>`Deployment` 및 `StatefulSet`에 대한 `kubectl` 출력</td>
<td>`Deployment` 및 `StatefulSet`에 대한 `kubectl` 출력에는 이제 `Ready` 열이 포함되며 가독성이 향상되었습니다. 스크립트가 이전 동작에 의존하는 경우 스크립트를 업데이트하십시오.</td>
</tr>
<tr>
<td>`PriorityClass`에 대한 `kubectl` 출력</td>
<td>`PriorityClass`에 대한 `kubectl` 출력에는 이제 `Value` 열이 포함됩니다. 스크립트가 이전 동작에 의존하는 경우 스크립트를 업데이트하십시오.</td>
</tr>
<tr>
<td>`kubectl get componentstatuses`</td>
<td>`kubectl get componentstatuses` 명령은 이제 `localhost`와 비보안(HTTP) 포트가 비활성화되어 Kubernetes 마스터 컴포넌트를 더 이상 Kubernetes API 서버에서 액세스할 수 없기 때문에 일부 Kubernetes 마스터 컴포넌트의 상태를 제대로 보고하지 않습니다. Kubernetes 버전 1.10에서 고가용성(HA) 마스터를 도입한 후에는 각 Kubernetes 마스터가 복수의 `apiserver`, `controller-manager`, `scheduler` 및 `etcd` 인스턴스로 설정됩니다. 대신 [{{site.data.keyword.Bluemix_notm}} 콘솔 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/kubernetes/landing)을 확인하거나 `ibmcloud ks cluster-get` [명령](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_get)을 사용하여 클러스터 상태를 검토하십시오.</td>
</tr>
<tr>
<tr>
<td>지원되지 않음: `kubectl run-container`</td>
<td>`kubectl run-container` 명령이 제거되었습니다. 대신 `kubectl run` 명령을 사용하십시오.</td>
</tr>
<tr>
<td>`kubectl rollout undo`</td>
<td>존재하지 않는 개정에 대해 `kubectl rollout undo`를 실행하면 오류가 리턴됩니다. 스크립트가 이전 동작에 의존하는 경우 스크립트를 업데이트하십시오.</td>
</tr>
<tr>
<td>더 이상 사용되지 않음: `scheduler.alpha.kubernetes.io/critical-pod` 어노테이션</td>
<td>`scheduler.alpha.kubernetes.io/critical-pod` 어노테이션은 이제 더 이상 사용되지 않습니다. 대신 이 어노테이션에 영향을 받는 팟(Pod)을 변경하여 [팟(Pod) 우선순위](/docs/containers?topic=containers-pod_priority#pod_priority)를 사용하십시오.</td>
</tr>
</tbody>
</table>

### 작업자 노드 이후 업데이트
{: #113_after_workers}

다음 표는 작업자 노드를 업데이트한 후에 수행해야 하는 조치를 보여줍니다.
{: shortdesc}

<table summary="버전 1.13에 대한 Kubernetes 업데이트">
<caption>작업자 노드를 Kubernetes 1.13으로 업데이트한 후 작성할 변경사항</caption>
<thead>
<tr>
<th>유형</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>containerd `cri` 스트림 서버</td>
<td>containerd 버전 1.2에서는 `cri` 플러그인 스트림 서버가 랜덤 포트, `http://localhost:0`에서 제공됩니다. 이 변경사항으로 인해 `kubelet` 스트리밍 프록시를 지원하며 컨테이너 `exec` 및 `logs` 오퍼레이션에 필요한 추가 보안 스트리밍 인터페이스를 제공합니다. 이전에는 `cri` 스트림 서버가 포트 10010을 사용하여 작업자 노드의 사설 네트워크 인터페이스에서 청취했습니다. 앱이 컨테이너 `cri` 플러그인을 사용하고 이전 동작에 종속적인 경우 앱을 업데이트하십시오.</td>
</tr>
</tbody>
</table>

<br />


## 버전 1.12
{: #cs_v112}

<p><img src="images/certified_kubernetes_1x12.png" style="padding-right: 10px;" align="left" alt="이 배지는 IBM Cloud 컨테이너 서비스에 대한 Kubernetes 버전 1.9 인증을 나타냅니다."/> {{site.data.keyword.containerlong_notm}}는 CNCF Kubernetes Software Conformance Certification 프로그램에서 버전 1.12에 대해 인증된 Kubernetes 제품입니다. _Kubernetes®는 미국 또는 기타 국가에서 사용되는 Linux Foundation의 등록상표이며, Linux Foundation의 라이센스에 따라 사용됩니다. _</p>

이전 Kubernetes 버전에서 1.12로 업데이트할 때 변경해야 할 사항을 검토하십시오.
{: shortdesc}

### 마스터 이전 업데이트
{: #112_before}

다음 표는 Kubernetes 마스터를 업데이트하기 전에 수행해야 하는 조치를 보여줍니다.
{: shortdesc}

<table summary="버전 1.12에 대한 Kubernetes 업데이트">
<caption>마스터를 Kubernetes 1.12로 업데이트하기 전에 변경할 사항</caption>
<thead>
<tr>
<th>유형</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes 메트릭 서버</td>
<td>클러스터에 Kubernetes `metric-server`가 배치되어 있는 경우 클러스터를 Kubernetes 1.12로 업데이트하기 전에 `metric-server`를 제거해야 합니다. 이를 제거하면 업데이트 중 배치된 `metric-server`와의 충돌을 방지할 수 있습니다.</td>
</tr>
<tr>
<td>`kube-system` `default` 서비스 계정에 대한 역할 바인딩</td>
<td>`kube-system` `default` 서비스 계정은 더 이상 Kubernetes API에 **cluster-admin**으로 액세스할 수 없습니다. 클러스터의 프로세스에 액세스해야 하는 기능 또는 추가 기능(예: [Helm](/docs/containers?topic=containers-helm#public_helm_install))을 배치하는 경우 [서비스 계정![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/reference/access-authn-authz/service-accounts-admin/)을 설정하십시오. 적절한 권한을 가진 개별 서비스 계정을 작성하고 설정하는 데 시간이 필요한 경우 다음과 같은 역할 바인딩을 사용하여 **cluster-admin** 역할을 일시적으로 부여할 수 있습니다. `kubectl create clusterrolebinding kube-system:default --clusterrole=cluster-admin --serviceaccount=kube-system:default`</td>
</tr>
</tbody>
</table>

### 마스터 이후 업데이트
{: #112_after}

다음 표는 Kubernetes 마스터를 업데이트한 후에 수행해야 하는 조치를 보여줍니다.
{: shortdesc}

<table summary="버전 1.12에 대한 Kubernetes 업데이트">
<caption>마스터를 Kubernetes 1.12로 업데이트한 후에 변경할 사항</caption>
<thead>
<tr>
<th>유형</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes용 API</td>
<td>Kubernetes API는 다음과 같이 더 이상 사용되지 않는 API를 대체합니다.
<ul><li><strong>apps/v1</strong>: `apps/v1` Kubernetes API는 `apps/v1beta1` 및 `apps/v1alpha` API를 대체합니다. `apps/v1` API도 `daemonset`, `deployment`, `replicaset` 및 `statefulset` 리소스용 `extensions/v1beta1` API를 대체합니다. Kubernetes 프로젝트는 이전 API에 대한 지원을 `apiserver` 및 `kubectl` 클라이언트에서 단계적으로 제거하고 있습니다.</li>
<li><strong>networking.k8s.io/v1</strong>: `networking.k8s.io/v1` API는 NetworkPolicy 리소스용 `extensions/v1beta1` API를 대체합니다.</li>
<li><strong>policy/v1beta1</strong>: `policy/v1beta1` API는 `podsecuritypolicy` 리소스용 `extensions/v1beta1` API를 대체합니다.</li></ul>
<br><br>더 이상 사용되지 않는 API가 지원되지 않기 전에 모든 YAML `apiVersion` 필드를 업데이트하여 적절한 Kubernetes API를 사용하십시오. 또한 다음과 같은 `apps/v1` 관련 변경사항을 보려면 [Kubernetes 문서 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)를 검토하십시오.
<ul><li>배치를 작성한 후에는 `.spec.selector` 필드를 변경할 수 없습니다.</li>
<li>`.spec.rollbackTo` 필드는 더 이상 사용되지 않습니다. `kubectl rollout undo` 명령을 대신 사용하십시오.</li></ul></td>
</tr>
<tr>
<td>클러스터 DNS 제공자로 사용 가능한 CoreDNS</td>
<td>Kubernetes 프로젝트는 현재 Kubernetes DNS(KubeDNS) 대신 CoreDNS를 지원하도록 전환되는 중입니다. 버전 1.12에서는 기본 클러스터 DNS가 KubeDNS로 남아 있지만, [CoreDNS를 사용하도록 선택](/docs/containers?topic=containers-cluster_dns#dns_set) 할 수 있습니다.</td>
</tr>
<tr>
<td>`kubectl apply --force`</td>
<td>이제 업데이트할 수 없는 리소스(예: YAML 파일의 변경할 수 없는 필드)에 대해 적용 조치를 강제 실행(`kubectl apply --force`)하면 리소스가 다시 작성됩니다. 스크립트가 이전 동작에 의존하는 경우 스크립트를 업데이트하십시오.</td>
</tr>
<tr>
<td>`kubectl get componentstatuses`</td>
<td>`kubectl get componentstatuses` 명령은 이제 `localhost`와 비보안(HTTP) 포트가 비활성화되어 Kubernetes 마스터 컴포넌트를 더 이상 Kubernetes API 서버에서 액세스할 수 없기 때문에 일부 Kubernetes 마스터 컴포넌트의 상태를 제대로 보고하지 않습니다. Kubernetes 버전 1.10에서 고가용성(HA) 마스터를 도입한 후에는 각 Kubernetes 마스터가 복수의 `apiserver`, `controller-manager`, `scheduler` 및 `etcd` 인스턴스로 설정됩니다. 대신 [{{site.data.keyword.Bluemix_notm}} 콘솔 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/kubernetes/landing)을 확인하거나 `ibmcloud ks cluster-get` [명령](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_get)을 사용하여 클러스터 상태를 검토하십시오.</td>
</tr>
<tr>
<td>`kubectl logs --interactive`</td>
<td>`kubectl logs`에 대해 `--interactive` 플래그가 더 이상 지원되지 않습니다. 이 플래그를 사용하는 자동화가 있을 경우 업데이트하십시오.</td>
</tr>
<tr>
<td>`kubectl patch`</td>
<td>`patch` 명령으로 변경되지 않을 경우(중복 패치) 이 명령이 더 이상 `1` 리턴 코드로 종료되지 않습니다. 스크립트가 이전 동작에 의존하는 경우 스크립트를 업데이트하십시오.</td>
</tr>
<tr>
<td>`kubectl version -c`</td>
<td>`-c` 약식 플래그가 더 이상 지원되지 않습니다. 전체 `--client` 플래그를 대신 사용하십시오. 이 플래그를 사용하는 자동화가 있을 경우 업데이트하십시오.</td>
</tr>
<tr>
<td>`kubectl wait`</td>
<td>일치하는 선택기를 찾을 수 없는 경우 이 명령은 이제 오류 메시지를 출력하고, `1` 리턴 코드로 종료됩니다. 스크립트가 이전 동작에 의존하는 경우 스크립트를 업데이트하십시오.</td>
</tr>
<tr>
<td>kubelet cAdvisor 포트</td>
<td>`--cadvisor-port`를 사용하여 Kubelet에서 사용하는 [Container Advisor(cAdvisor) ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/google/cadvisor) 웹 UI가 Kubernetes 1.12에서 제거되었습니다. 그래도 cAdvisor를 사용해야 하는 경우 [cAdvisor를 디먼 세트로 배치![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/google/cadvisor/tree/master/deploy/kubernetes)하십시오.<br><br>디먼 세트에서 cAdvisor가 `http://node-ip:4194`를 통해 연결될 수 있도록 다음과 같이 ports 섹션을 지정하십시오. 이전 버전의 kubelet에서는 cAdvisor에 포트 4194를 사용하기 때문에 작업자 노드가 1.12로 업데이트될 때까지 cAdvisor 팟(Pod)이 실패합니다.
<pre class="screen"><code>ports:
          - name: http
            containerPort: 8080
            hostPort: 4194
            protocol: TCP</code></pre></td>
</tr>
<tr>
<td>Kubernetes 대시보드</td>
<td>`kubectl proxy`를 통해 대시보드에 액세스할 경우 로그인 페이지의 **건너뛰기** 단추가 제거됩니다. 대신 [**토큰**을 사용하여 로그인](/docs/containers?topic=containers-app#cli_dashboard)하십시오.</td>
</tr>
<tr>
<td id="metrics-server">Kubernetes 메트릭 서버</td>
<td>Kubernetes 메트릭 서버는 Kubernetes Heapster(Kubernetes 버전 1.8부터 더 이상 사용되지 않음)를 클러스터 메트릭 제공자로 대체합니다. 클러스터에서 작업자 노드당 30개 이상의 팟(Pod)을 실행할 경우 [성능을 위해 `metrics-server` 구성을 조정](/docs/containers?topic=containers-kernel#metrics)하십시오.
<p>Kubernetes 대시보드는 `metrics-server`에서 작동하지 않습니다. 대시보드에 메트릭을 표시하려면 다음 옵션 중에서 선택하십시오.</p>
<ul><li>클러스터 모니터링 대시보드를 사용하여 [메트릭을 분석하도록 Grafana를 설정](/docs/services/cloud-monitoring/tutorials?topic=cloud-monitoring-container_service_metrics#container_service_metrics)합니다.</li>
<li>클러스터에 [Heapster ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/heapster)를 배치합니다.
<ol><li>`heapster-rbac` [YAML ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/kubernetes/blob/release-1.12/cluster/addons/cluster-monitoring/heapster-rbac.yaml), `heapster-service` [YAML ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/kubernetes/blob/release-1.12/cluster/addons/cluster-monitoring/standalone/heapster-service.yaml) 및 `heapster-controller` [YAML ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/kubernetes/blob/release-1.12/cluster/addons/cluster-monitoring/standalone/heapster-controller.yaml) 파일을 복사하십시오.</li>
<li>다음 문자열을 대체하여 `heapster-controller` YAML을 편집하십시오.
<ul><li>`{{ nanny_memory }}`를 `90Mi`로 대체하십시오.</li>
<li>`{{ base_metrics_cpu }}`를 `80m`으로 대체하십시오.</li>
<li>`{{ metrics_cpu_per_node }}`를 `0.5m`으로 대체하십시오.</li>
<li>`{{ base_metrics_memory }}`를 `140Mi`로 대체하십시오.</li>
<li>`{{ metrics_memory_per_node }}`를 `4Mi`로 대체하십시오.</li>
<li>`{{ heapster_min_cluster_size }}`를 `16`으로 대체하십시오.</li></ul></li>
<li>`kubectl apply -f` 명령을 실행하여 `heapster-rbac`, `heapster-service` 및 `heapster-controller` YAML 파일을 클러스터에 적용하십시오.</li></ol></li></ul></td>
</tr>
<tr>
<td>`rbac.authorization.k8s.io/v1` Kubernetes API</td>
<td>`rbac.authorization.k8s.io/v1` Kubernetes API(Kubernetes 1.8부터 지원됨)는 `rbac.authorization.k8s.io/v1alpha1` 및 `rbac.authorization.k8s.io/v1beta1` API로 대체됩니다. 지원되지 않는 `v1alpha` API에서는 더 이상 역할 또는 역할 바인딩과 같은 RBAC 오브젝트를 작성할 수 있습니다. 기존 RBAC 오브젝트는 `v1` API로 변환됩니다.</td>
</tr>
</tbody>
</table>

<br />


## 더 이상 사용되지 않음: 버전 1.11
{: #cs_v111}

<p><img src="images/certified_kubernetes_1x11.png" style="padding-right: 10px;" align="left" alt="이 배지는 IBM Cloud 컨테이너 서비스에 대한 Kubernetes 버전 1.11 인증을 표시합니다."/> {{site.data.keyword.containerlong_notm}}는 CNCF Kubernetes Software Conformance Certification 프로그램에서 버전 1.11에 대해 인증된 Kubernetes 제품입니다. _Kubernetes®는 미국 또는 기타 국가에서 사용되는 Linux Foundation의 등록상표이며, Linux Foundation의 라이센스에 따라 사용됩니다. _</p>

이전 Kubernetes 버전에서 1.11로 업데이트할 때 변경해야 할 사항을 검토하십시오.
{: shortdesc}

Kubernetes 버전 1.11은 더 이상 사용되지 않으며 2019년 6월 27일(임시)부터 지원되지 않습니다. 각 Kubernetes 버전 업데이트의 [잠재적인 영향을 검토](/docs/containers?topic=containers-cs_versions#cs_versions)한 후에 버전 1.12 이상으로 즉시 [클러스터를 업데이트](/docs/containers?topic=containers-update#update)하십시오.
{: deprecated}

Kubernetes 버전 1.9 또는 이전 버전에서 버전 1.11로 클러스터를 업데이트하려면 먼저 [Calico v3으로 업데이트 준비](#111_calicov3)에 나열된 단계를 따라야 합니다.
{: important}

### 마스터 이전 업데이트
{: #111_before}

다음 표는 Kubernetes 마스터를 업데이트하기 전에 수행해야 하는 조치를 보여줍니다.
{: shortdesc}

<table summary="버전 1.11에 대한 Kubernetes 업데이트">
<caption>마스터를 Kubernetes 1.11로 업데이트하기 전에 작성할 변경사항</caption>
<thead>
<tr>
<th>유형</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>클러스터 마스터 고가용성(HA) 구성</td>
<td>고가용성(HA)을 높이도록 클러스터 마스터 구성이 업데이트되었습니다. 클러스터에는 이제 각 마스터가 별도의 실제 호스트에 배치되도록 설정된 Kubernetes 마스터 복제본이 있습니다. 또한 클러스터가 다중 구역 가능 구역에 있을 경우 마스터가 전체 구역에 분산됩니다.<br><br>취해야 할 조치는 [고가용성 클러스터 마스터로 업데이트](#ha-masters)를 참조하십시오. 이 준비 조치는 다음과 같은 경우에 적용됩니다.<ul>
<li>방화벽 또는 사용자 정의 Calico 네트워크 정책이 있는 경우</li>
<li>작업자 노드에서 호스트 포트 `2040` 또는 `2041`을 사용 중인 경우</li>
<li>마스터에 대한 클러스터 내부 액세스를 위해 클러스터 마스터 IP 주소를 사용한 경우</li>
<li>Calico 정책을 작성하기 위해 Calico API 또는 CLI(`calicoctl`)를 호출하는 자동화가 있는 경우</li>
<li>마스터에 대한 팟(Pod) 유출 액세스를 제어하기 위해 Kubernetes 또는 Calico 네트워크 정책을 사용하는 경우</li></ul></td>
</tr>
<tr>
<td>`containerd` 새 Kubernetes 컨테이너 런타임</td>
<td><p class="important">`containerd`가 Kubernetes의 새 컨테이너 런타임으로서 Docker를 대체합니다. 취해야 할 조치는 [컨테이너 런타임으로서 `containerd`로 업데이트](#containerd)를 참조하십시오.</p></td>
</tr>
<tr>
<td>etcd에서 데이터 암호화</td>
<td>이전에는 etcd 데이터가 마스터의 NFS 파일 스토리지 인스턴스에 저장되어 저장 상태로 암호화되었습니다. 이제 etcd 데이터는 마스터의 로컬 디스크에 저장되고, {{site.data.keyword.cos_full_notm}}에 백업됩니다. 데이터는 {{site.data.keyword.cos_full_notm}}로 전송 중 및 저장 중에 암호화됩니다. 그러나 마스터의 로컬 디스크에 있는 etcd 데이터는 암호화되지 않습니다. 마스터의 로컬 etcd 데이터를 암호화하려는 경우 [클러스터에서 {{site.data.keyword.keymanagementservicelong_notm}}를 사용으로 설정](/docs/containers?topic=containers-encryption#keyprotect)하십시오.</td>
</tr>
<tr>
<td>Kubernetes 컨테이너 볼륨 마운트 전파</td>
<td>컨테이너 `VolumeMount`에 대한 [`mountPropagation` 필드 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/storage/volumes/#mount-propagation)의 기본값이 `HostToContainer`에서 `None`으로 변경되었습니다. 이 변경사항으로 인해 Kubernetes 버전 1.9 이하에 존재하던 작동이 복원됩니다. 팟(Pod) 스펙이 기본값인 `HostToContainer`에 의존하는 경우에는 이를 업데이트하십시오.</td>
</tr>
<tr>
<td>Kubernetes API 서버 JSON 디시리얼라이저</td>
<td>Kubernetes API 서버 JSON 디시리얼라이저가 이제 대소문자를 구분합니다. 이 변경사항으로 인해 Kubernetes 버전 1.7 이하에 존재하던 작동이 복원됩니다. JSON 리소스 정의에서 올바르지 않은 대소문자를 사용하는 경우에는 이를 업데이트하십시오. <br><br>직접적인 Kubernetes API 서버 요청만 영향을 받습니다. `kubectl` CLI가 Kubernetes 버전 1.7 이하에서 대소문자 구분 키를 계속해서 적용하므로, `kubectl`에서 리소스를 엄격하게 관리하는 경우에는 영향을 받지 않습니다.</td>
</tr>
</tbody>
</table>

### 마스터 이후 업데이트
{: #111_after}

다음 표는 Kubernetes 마스터를 업데이트한 후에 수행해야 하는 조치를 보여줍니다.
{: shortdesc}

<table summary="버전 1.11에 대한 Kubernetes 업데이트">
<caption>마스터를 Kubernetes 1.11로 업데이트한 후에 작성할 변경사항</caption>
<thead>
<tr>
<th>유형</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>클러스터 로깅 구성</td>
<td>`logging-autoupdate`를 사용하지 않아도 `fluentd` 클러스터 추가 기능이 버전 1.11에서 자동으로 업데이트됩니다.<br><br>
컨테이너 로그 디렉토리가 `/var/lib/docker/`에서 `/var/log/pods/`로 변경되었습니다. 이전 디렉토리를 모니터링하는 자체 로깅 솔루션을 사용하는 경우에는 이에 맞게 업데이트하십시오.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} IAM(Identity and Access Management) 지원</td>
<td>Kubernetes 버전 1.11  이상을 실행하는 클러스터는 IAM [액세스 그룹](/docs/iam?topic=iam-groups#groups) 및 [서비스 ID](/docs/iam?topic=iam-serviceids#serviceids)를 지원합니다. 이제 이 기능을 사용하여 [클러스터에 액세스할 권한을 부여](/docs/containers?topic=containers-users#users)할 수 있습니다.</td>
</tr>
<tr>
<td>Kubernetes 구성 새로 고치기</td>
<td>클러스터의 Kubernetes API 서버에 대한 OpenID Connect 구성이 {{site.data.keyword.Bluemix_notm}} Identity and Access Management(IAM) 액세스 그룹을 지원하도록 업데이트되었습니다. 따라서 클러스터의 Kubernetes v1.11 업데이트 이후에는 `ibmcloud ks cluster-config --cluster <cluster_name_or_ID>`를 실행하여 Kubernetes 구성을 새로 고쳐야 합니다. 이 명령을 사용하면 해당 구성이 `default` 네임스페이스의 역할 바인딩에 적용됩니다.<br><br>구성을 새로 고치지 않으면 다음 오류 메시지와 함께 클러스터 조치가 실패합니다. `You must be logged in to the server (Unauthorized).`</td>
</tr>
<tr>
<td>Kubernetes 대시보드</td>
<td>`kubectl proxy`를 통해 대시보드에 액세스할 경우 로그인 페이지의 **건너뛰기** 단추가 제거됩니다. 대신 [**토큰**을 사용하여 로그인](/docs/containers?topic=containers-app#cli_dashboard)하십시오.</td>
</tr>
<tr>
<td>`kubectl` CLI</td>
<td>Kubernetes 버전 1.11용 `kubectl` CLI에서는 `apps/v1` API가 필요합니다. 따라서 v1.11 `kubectl` CLI는 Kubernetes 버전 1.8 이하를 실행하는 클러스터의 경우 작동하지 않습니다. 클러스터의 Kubernetes API 서버 버전과 일치하는 `kubectl` CLI의 버전을 사용하십시오.</td>
</tr>
<tr>
<td>`kubectl auth can-i`</td>
<td>이제 사용자에게 권한이 부여되지 않으면 `kubectl auth can-i` 명령이 `exit code 1`로 실패합니다. 스크립트가 이전 작동에 의존하는 경우에는 이를 업데이트하십시오.</td>
</tr>
<tr>
<td>`kubectl delete`</td>
<td>이제 레이블 등의 선택 기준을 사용하여 리소스를 삭제할 때 `kubectl delete` 명령이 기본적으로 `not found` 오류를 무시합니다. 스크립트가 이전 동작에 의존하는 경우 스크립트를 업데이트하십시오.</td>
</tr>
<tr>
<td>Kubernetes `sysctls` 기능</td>
<td>`security.alpha.kubernetes.io/sysctls` 어노테이션이 이제 무시됩니다. 대신 Kubernetes에서는 `sysctls`를 지정하고 제어하기 위해 `PodSecurityPolicy` 및 `Pod` 오브젝트에 필드를 추가했습니다. 자세한 정보는 [Kubernetes에서 sysctls 사용 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/tasks/administer-cluster/sysctl-cluster/)을 참조하십시오. <br><br>클러스터 마스터 및 작업자를 업데이트한 후에는 새 `sysctls` 필드를 사용할 수 있도록 `PodSecurityPolicy` 및 `Pod` 오브젝트를 업데이트하십시오.</td>
</tr>
</tbody>
</table>

### Kubernetes 1.11에서 고가용성 클러스터 마스터로 업데이트
{: #ha-masters}

Kubernetes 버전 1.10.8_1530, 1.11.3_1531 이상을 실행하는 클러스터의 경우 고가용성(HA)을 높이도록 클러스터 마스터 구성이 업데이트되었습니다. 클러스터에는 이제 각 마스터가 별도의 실제 호스트에 배치되도록 설정된 Kubernetes 마스터 복제본이 있습니다. 또한 클러스터가 다중 구역 가능 구역에 있을 경우 마스터가 전체 구역에 분산됩니다.
{: shortdesc}

콘솔에서 클러스터의 마스터 URL을 확인하거나 `ibmcloud ks cluster-get --cluster <cluster_name_or_ID` 명령을 실행하여 클러스터에 HA 마스터 구성이 있는지 확인할 수 있습니다. 마스터 URL에 ` https://c2.us-south.containers.cloud.ibm.com:xxxxx`와 같은 호스트 이름이 있고 ` https://169.xx.xx.xx:xxxxx`와 같은 IP 주소가 없는 경우 클러스터에는 HA 마스터 구성이 있습니다. 자동 마스터 패치 업데이트 또는 수동으로 업데이트를 적용하여 HA 마스터 구성을 가져올 수 있습니다. 어느 경우든 다음 항목을 검토하여 클러스터 네트워크가 구성을 최대한 활용하도록 설정되었는지 확인해야 합니다.

* 방화벽 또는 사용자 정의 Calico 네트워크 정책이 있는 경우
* 작업자 노드에서 호스트 포트 `2040` 또는 `2041`을 사용 중인 경우
* 마스터에 대한 클러스터 내부 액세스를 위해 클러스터 마스터 IP 주소를 사용한 경우
* Calico 정책을 작성하기 위해 Calico API 또는 CLI(`calicoctl`)를 호출하는 자동화가 있는 경우
* 마스터에 대한 팟(Pod) 유출 액세스를 제어하기 위해 Kubernetes 또는 Calico 네트워크 정책을 사용하는 경우

<br>
**HA 마스터에 대해 방화벽 또는 사용자 정의 Calico 네트워크 정책 업데이트**:</br>
{: #ha-firewall}
방화벽 또는 사용자 정의 Calico 호스트 네트워크 정책을 사용하여 작업자 노드에서의 유출을 제어하는 경우 클러스터가 있는 영역 내의 모든 구역에 대해 포트 및 IP 주소로의 발신 트래픽을 허용하십시오. [클러스터가 인프라 리소스 및 기타 서비스에 액세스하도록 허용](/docs/containers?topic=containers-firewall#firewall_outbound)을 참조하십시오.

<br>
**작업자 노드에서 호스트 포트 `2040` 및 `2041` 예약**:</br>
{: #ha-ports}
HA 구성에서 클러스터 마스터에 대한 액세스를 허용하려면 호스트 포트 `2040` 및 `2041`을 모든 작업자 노드에서 사용 가능한 상태로 유지해야 합니다.
* `hostPort`가 `2040` 또는 `2041`로 설정된 팟(Pod)은 다른 포트를 사용하도록 업데이트하십시오.
* `hostNetwork`가 `true`이고 `2040` 또는 `2041` 포트에서 청취하는 팟(Pod)은 다른 포트를 사용하도록 업데이트하십시오.

팟(Pod)이 현재 `2040` 또는 `2041` 포트를 사용하고 있는지 확인하려면 사용 중인 클러스터를 대상으로 설정하고 다음 명령을 실행하십시오.

```
kubectl get pods --all-namespaces -o yaml | grep -B 3 "hostPort: 204[0,1]"
```
{: pre}

HA 마스터 구성이 이미 있는 경우 다음 예에서와 같이 `kube-system` 네임스페이스에서 `ibm-master-proxy-*`에 대한 결과를 볼 수 있습니다. 기타 팟(Pod)이 리턴되면 포트를 업데이트하십시오.

```
name: ibm-master-proxy-static
ports:
- containerPort: 2040
  hostPort: 2040
  name: apiserver
  protocol: TCP
- containerPort: 2041
  hostPort: 2041
...
```
{: screen}


<br>
**마스터에 대한 클러스터 내부 액세스를 위해 `kubernetes` 서비스 클러스터 IP 또는 도메인 사용**:</br>
{: #ha-incluster}
클러스터 내의 HA 구성에서 클러스터 마스터에 액세스하려면 다음 중 하나를 사용하십시오.
* `kubernetes` 서비스 클러스터 IP 주소 - 기본값은 `https://172.21.0.1`입니다.
* `kubernetes` 서비스 도메인 이름 - 기본값은 `https://kubernetes.default.svc.cluster.local`입니다.

이전에 클러스터 마스터 IP 주소를 사용한 경우 이 방식이 계속 작동합니다. 그러나 가용성 향상을 위해 `kubernetes` 서비스 클러스터 IP 주소 또는 도메인 이름을 사용하도록 업데이트하십시오.

<br>
**HA 구성의 마스터에 대한 클러스터 외부 액세스를 위해 Calico 구성**:</br>
{: #ha-outofcluster}
`kube-system` 네임스페이스의 `calico-config` configmap에 저장된 데이터는 HA 마스터 구성을 지원하도록 변경되었습니다. 특히, `etcd_endpoints` 값은 이제 클러스터 내부 액세스만 지원합니다. 이 값을 사용하여 클러스터 외부에서 액세스하도록 Calico CLI를 구성하는 것은 더 이상 작동하지 않습니다.

대신 `kube-system` 네임스페이스의 `cluster-info` configmap에 저장된 데이터를 사용하십시오. 특히, 클러스터 외부에서 HA 구성의 마스터에 액세스하려면 `etcd_host` 및 `etcd_port` 값을 사용하여 [Calico CLI](/docs/containers?topic=containers-network_policies#cli_install)에 대한 엔드포인트를 구성하십시오.

<br>
**Kubernetes 또는 Calico 네트워크 정책 업데이트**:</br>
{: #ha-networkpolicies}
[Kubernetes 또는 Calico 네트워크 정책](/docs/containers?topic=containers-network_policies#network_policies)을 사용하여 클러스터 마스터에 대한 유출 액세스를 제어하며 현재 다음을 사용 중인 경우 추가 조치를 수행해야 합니다.
*  Kubernetes 서비스 클러스터 IP - `kubectl get service kubernetes -o yaml | grep clusterIP`를 실행하여 얻을 수 있습니다.
*  kubernetes 서비스 도메인 이름 - 기본값은 `https://kubernetes.default.svc.cluster.local`입니다.
*  클러스터 마스터 IP - `kubectl cluster-info | grep Kubernetes`를 실행하여 얻을 수 있습니다.

다음 단계는 Kubernetes 네트워크 정책을 업데이트하는 방법에 대해 설명합니다. Calico 네트워크 정책을 업데이트하려면 정책 구문을 약간 변경하고 `calicoctl`를 통해 이 단계를 반복하여 정책에서 영향을 검색하십시오.
{: note}

시작하기 전에: [계정에 로그인하십시오. 해당되는 경우, 적절한 리소스 그룹을 대상으로 지정하십시오. 클러스터의 컨텍스트를 설정하십시오.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  클러스터 마스터 IP 주소를 가져오십시오.
    ```
    kubectl cluster-info | grep Kubernetes
    ```
    {: pre}

2.  Kubernetes 네트워크 정책에서 영향을 검색하십시오. YAML이 리턴되지 않는다면 클러스터가 영향을 받지 않는 것이므로 추가로 변경할 필요가 없습니다.
    ```
    kubectl get networkpolicies --all-namespaces -o yaml | grep <cluster-master-ip>
    ```
    {: pre}

3.  YAML을 검토하십시오. 예를 들어 클러스터가 다음과 같은 Kubernetes 네트워크 정책을 사용하여 `default` 네임스페이스의 팟(Pod)이 `kubernetes` 서비스 클러스터 IP 또는 클러스터 마스터 IP를 통해 클러스터 마스터에 액세스하도록 허용하는 경우 정책을 업데이트해야 합니다.
    ```
    apiVersion: networking.k8s.io/v1
    kind: NetworkPolicy
    metadata:
      name: all-master-egress
      namespace: default
    spec:
      egress:
      # Allow access to cluster master using kubernetes service cluster IP address
      # or domain name or cluster master IP address.
      - ports:
        - protocol: TCP
        to:
        - ipBlock:
            cidr: 161.202.126.210/32
      # Allow access to Kubernetes DNS in order to resolve the kubernetes service
      # domain name.
      - ports:
        - protocol: TCP
          port: 53
        - protocol: UDP
          port: 53
      podSelector: {}
      policyTypes:
      - Egress
    ```
    {: screen}

4.  클러스터 내부 마스터 프록시 IP 주소 `172.20.0.1`로의 유출을 허용하도록 Kubernetes 네트워크 정책을 수정하십시오. 지금은 클러스터 마스터 IP 주소를 유지하십시오. 예를 들어 위의 네트워크 정책 예는 다음과 같이 변경됩니다.

    이전에 단일 Kubernetes 마스터에 대해 단일 IP 주소와 포트만 열도록 유출 정책을 설정한 경우 이제 클러스터 내부 마스터 프록시 IP 주소 범위 172.20.0.1/32와 포트 2040을 사용하십시오.
    {: tip}

    ```
    apiVersion: networking.k8s.io/v1
    kind: NetworkPolicy
    metadata:
      name: all-master-egress
      namespace: default
    spec:
      egress:
      # Allow access to cluster master using kubernetes service cluster IP address
      # or domain name.
      - ports:
        - protocol: TCP
        to:
        - ipBlock:
            cidr: 172.20.0.1/32
        - ipBlock:
            cidr: 161.202.126.210/32
      # Allow access to Kubernetes DNS in order to resolve the kubernetes service domain name.
      - ports:
        - protocol: TCP
          port: 53
        - protocol: UDP
          port: 53
      podSelector: {}
      policyTypes:
      - Egress
    ```
    {: screen}

5.  수정된 네트워크 정책을 클러스터에 적용하십시오.
    ```
    kubectl apply -f all-master-egress.yaml
    ```
    {: pre}

6.  이 단계를 포함하여 [준비 조치](#ha-masters)를 모두 완료한 후에는 HA 마스터 수정팩에 대해 [클러스터 마스터를 업데이트](/docs/containers?topic=containers-update#master)하십시오.

7.  업데이트가 완료되면 클러스터 마스터 IP 주소를 네트워크 정책에서 제거하십시오. 예를 들어 위의 네트워크 정책에서 다음 행을 제거한 후 정책을 다시 적용하십시오.

    ```
    - ipBlock:
        cidr: 161.202.126.210/32
    ```
    {: screen}

    ```
    kubectl apply -f all-master-egress.yaml
    ```
    {: pre}

### 컨테이너 런타임으로서 `containerd`로 업데이트
{: #containerd}

Kubernetes 버전 1.11 이상을 실행하는 클러스터의 경우, `containerd`는 성능 개선을 위해 Kubernetes의 새 컨테이너 런타임으로서 Docker를 대체합니다. 팟(Pod)이 Kubernetes 컨테이너 런타임으로서 Docker에 의존하는 경우, 사용자는 컨테이너 런타임으로서 `containerd`를 처리하도록 이를 업데이트해야 합니다. 자세한 정보는 [Kubernetes containerd 공지사항 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/blog/2018/05/24/kubernetes-containerd-integration-goes-ga/)을 참조하십시오.
{: shortdesc}

**내 앱이 `containerd`가 아닌 `docker`에 의존하는지 어떻게 알 수 있습니까? **<br>
컨테이너 런타임으로서 Docker에 의존할 수 있는 경우의 예:
*  권한 부여된 컨테이너를 사용하여 직접 Docker 엔진 또는 API에 액세스하는 경우에는 런타임으로서 `containerd`를 지원하도록 팟(Pod)을 업데이트하십시오. 예를 들어 Docker 소켓을 직접 호출하여 컨테이너를 실행하거나 다른 Docker 조작을 수행하십시오. Docker 소켓은 `/var/run/docker.sock`에서 `/run/containerd/containerd.sock`으로 변경되었습니다. `containerd` 소켓에 사용되는 프로토콜은 Docker에 사용된 프로토콜과 약간 다릅니다. 앱을 `containerd` 소켓으로 업데이트하도록 시도하십시오. Docker 소켓을 계속 사용하려는 경우 [Docker-inside-Docker(DinD) ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://hub.docker.com/_/docker/) 사용을 검토하십시오.
*  클러스터에 설치되는 일부 서드파티 추가 기능(예: 로깅 및 모니터링 도구)은 Docker 엔진에 의존할 수 있습니다. 제공자에게 문의하여 해당 도구가 containerd와 호환 가능한지 확인하십시오. 가능한 유스 케이스는 다음과 같습니다.
   - 로깅 도구는 로그에 액세스하도록 컨테이너 `stderr/stdout` 디렉토리 `/var/log/pods/<pod_uuid>/<container_name>/*.log`를 사용할 수 있습니다. Docker에서 이 디렉토리는 `/var/data/cripersistentstorage/containers/<container_uuid>/<container_uuid>-json.log`에 대한 symlink입니다. 반면 `containerd`에서는 symlink 없이 직접 디렉토리에 액세스할 수 있습니다.
   - 모니터링 도구는 Docker 소켓에 직접 액세스합니다. Docker 소켓은 `/var/run/docker.sock`에서 `/run/containerd/containerd.sock`으로 변경되었습니다.

<br>

**런타임에 대한 의존도 외에도 다른 준비 조치를 취해야 합니까?**<br>

**Manifest 도구**: Docker 버전 18.06 이전에 시범 `docker manifest` [도구 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.docker.com/edge/engine/reference/commandline/manifest/)로 빌드된 다중 플랫폼 이미지가 있는 경우에는 `containerd`를 사용하여 DockerHub에서 이미지를 가져올 수 없습니다.

팟(Pod) 이벤트를 확인할 때 다음과 같은 오류가 나타날 수 있습니다.
```
failed size validation
```
{: screen}

`containerd`와 함께 Manifest 도구를 사용하여 빌드된 이미지를 사용하려면 다음 옵션에서 선택하십시오.

*  [manifest 도구 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/estesp/manifest-tool)로 이미지를 다시 빌드합니다.
*  Docker 버전 18.06 이상으로 업데이트한 후에 `docker-manifest` 도구로 이미지를 다시 빌드합니다.

<br>

**영향을 받지 않는 것은 무엇입니까? 내 컨테이너를 배치하는 방법을 변경해야 합니까?**<br>
일반적으로 컨테이너 배치 프로세스는 변경되지 않습니다. 여전히 Dockerfile을 사용하여 Docker 이미지를 정의하고 앱에 대해 Docker 컨테이너를 빌드할 수 있습니다. `docker` 명령을 사용하여 이미지를 빌드하고 이를 레지스트리에 푸시하는 경우, 계속해서 `docker`를 사용하거나 `ibmcloud cr` 명령을 대신 사용할 수 있습니다.

### Calico v3으로 업데이트 준비
{: #111_calicov3}

Kubernetes 버전 1.9 또는 이전 버전에서 버전 1.11로 클러스터를 업데이트하는 경우 마스터를 업데이트하기 전에 Calico v3 업데이트를 준비하십시오. 마스터를 Kubernetes v1.11로 업그레이드하는 동안에는 새 팟(Pod)과 새 Kubernetes 또는 Calico 네트워크 정책이 스케줄되지 않습니다. 업데이트로 인해 새로 스케줄되지 못하는 시간은 경우에 따라 다릅니다. 소규모 클러스터에서는 몇 분이 소요되며 10개의 노드마다 몇 분이 더 추가될 수 있습니다. 기존 네트워크 정책 및 팟(Pod)은 계속 실행됩니다.
{: shortdesc}

Kubernetes 버전 1.10 또는 이전 버전에서 버전 1.11로 클러스터를 업데이트하는 경우 이러한 단계를 1.10으로 업데이트할 때 완료했으므로 건너뛰십시오.
{: note}

이 작업을 시작하기 전에는 클러스터 마스터 및 모든 작업자 노드가 Kubernetes 버전 1.8 또는 1.9를 실행 중이어야 하며 하나 이상의 작업자 노드를 보유해야 합니다.

1.  Calico 팟(Pod)이 정상 상태인지 확인하십시오.
    ```
    kubectl get pods -n kube-system -l k8s-app=calico-node -o wide
    ```
    {: pre}

2.  팟(Pod)이 **실행 중** 상태가 아니면 해당 팟(Pod)을 삭제하고 계속하기 전에 **실행 중** 상태가 될 때까지 기다리십시오. 팟(Pod)이 **실행 중** 상태로 돌아가지 않을 경우 다음을 수행하십시오.
    1.  작업자 노드의 **State** 및 **Status**를 확인하십시오.
        ```
   ibmcloud ks workers --cluster <cluster_name_or_ID>
        ```
        {: pre}
    2.  작업자 노드가 **정상** 상태가 아닌 경우 [작업자 노드 디버깅](/docs/containers?topic=containers-cs_troubleshoot#debug_worker_nodes) 단계를 수행하십시오. 예를 들어 **위험** 또는 **알 수 없음** 상태는 주로 [작업자 노드 다시 로드](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)를 수행하면 해결됩니다.

3.  Calico 정책 또는 Calico 리소스를 자동 생성하는 경우 [Calico v3 구문 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.projectcalico.org/v3.1/reference/calicoctl/resources/)을 사용하여 이러한 리소스를 생성하도록 자동화 도구를 업데이트하십시오.

4.  VPN 연결에 [strongSwan](/docs/containers?topic=containers-vpn#vpn-setup)을 사용하는 경우 strongSwan 2.0.0 Helm 차트는 Calico v3 또는 Kubernetes 1.11에서 작동하지 않습니다. Calico 2.6 및 Kubernetes 1.7, 1.8, 1.9와 역호환 가능한 2.1.0 Helm 차트로 [strongSwan을 업데이트](/docs/containers?topic=containers-vpn#vpn_upgrade)하십시오.

5.  [클러스터 마스터를 Kubernetes v1.11로 업데이트](/docs/containers?topic=containers-update#master)하십시오.

<br />


## 아카이브
{: #k8s_version_archive}

{{site.data.keyword.containerlong_notm}}에서 지원되지 않는 Kubernetes 버전의 개요를 찾으십시오.
{: shortdesc}

### 버전 1.10(지원되지 않음)
{: #cs_v110}

2019년 5월 16일 현재, [Kubernetes 버전 1.10](/docs/containers?topic=containers-changelog#changelog_archive)을 실행하는 {{site.data.keyword.containerlong_notm}} 클러스터는 지원되지 않습니다. 버전 1.10 클러스터는 다음 최신 버전으로 업데이트되지 않는 한 보안 업데이트나 지원을 받을 수 없습니다.
{: shortdesc}

Kubernetes 1.11이 더 이상 사용되지 않으므로 각 Kubernetes 버전 업데이트의 [잠재적인 영향을 검토](/docs/containers?topic=containers-cs_versions#cs_versions)한 후에 [Kubernetes 1.12](#cs_v112)로 [클러스터를 업데이트](/docs/containers?topic=containers-update#update)하십시오.

### 버전 1.9(지원되지 않음)
{: #cs_v19}

2018년 12월 27일부터, [Kubernetes 버전 1.9](/docs/containers?topic=containers-changelog#changelog_archive)를 실행하는 {{site.data.keyword.containerlong_notm}} 클러스터는 지원되지 않습니다. 버전 1.9 클러스터는 다음 최신 버전으로 업데이트되지 않는 한 보안 업데이트나 지원을 받을 수 없습니다.
{: shortdesc}

각 Kubernetes 버전 업데이트 [잠재적인 영향을 검토](/docs/containers?topic=containers-cs_versions#cs_versions)한 후 먼저 [더 이상 사용되지 않는 Kubernetes 1.11](#cs_v111)를 업데이트하고 바로 [Kubernetes 1.12](#cs_v112)로 [클러스터를 업데이트](/docs/containers?topic=containers-update#update)하십시오. 

### 버전 1.8(지원되지 않음)
{: #cs_v18}

2018년 9월 22일부터, [Kubernetes 버전 1.8](/docs/containers?topic=containers-changelog#changelog_archive)을 실행하는 {{site.data.keyword.containerlong_notm}} 클러스터는 지원되지 않습니다. 버전 1.8 클러스터는 보안 업데이트 또는 지원을 받을 수 없습니다.
{: shortdesc}

{{site.data.keyword.containerlong_notm}}에서 앱을 계속 실행하려면 [새 클러스터를 작성](/docs/containers?topic=containers-clusters#clusters)하고 새 클러스터에 [앱을 배치](/docs/containers?topic=containers-app#app)하십시오.

### 버전 1.7(지원되지 않음)
{: #cs_v17}

2018년 6월 21일 현재, [Kubernetes 버전 1.7](/docs/containers?topic=containers-changelog#changelog_archive)을 실행하는 {{site.data.keyword.containerlong_notm}} 클러스터는 지원되지 않습니다. 버전 1.7 클러스터는 보안 업데이트 또는 지원을 받을 수 없습니다.
{: shortdesc}

{{site.data.keyword.containerlong_notm}}에서 앱을 계속 실행하려면 [새 클러스터를 작성](/docs/containers?topic=containers-clusters#clusters)하고 새 클러스터에 [앱을 배치](/docs/containers?topic=containers-app#app)하십시오.

### 버전 1.5(지원되지 않음)
{: #cs_v1-5}

2018년 4월 4일부터, [Kubernetes 버전 1.5](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG-1.5.md)를 실행하는 {{site.data.keyword.containerlong_notm}} 클러스터는 지원되지 않습니다. 버전 1.5 클러스터는 보안 업데이트 또는 지원을 받을 수 없습니다.
{: shortdesc}

{{site.data.keyword.containerlong_notm}}에서 앱을 계속 실행하려면 [새 클러스터를 작성](/docs/containers?topic=containers-clusters#clusters)하고 새 클러스터에 [앱을 배치](/docs/containers?topic=containers-app#app)하십시오.
