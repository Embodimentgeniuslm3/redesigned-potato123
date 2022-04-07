---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-31"

keywords: kubernetes, iks, audit

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



# {{site.data.keyword.cloudaccesstrailshort}} 이벤트
{: #at_events}

{{site.data.keyword.cloudaccesstrailshort}} 서비스를 사용하여 {{site.data.keyword.containerlong_notm}} 클러스터에서 사용자가 시작한 활동을 보고 관리하며 감사할 수 있습니다.
{: shortdesc}

{{site.data.keyword.containershort_notm}}는 두 가지 유형의 {{site.data.keyword.cloudaccesstrailshort}} 이벤트를 생성합니다.

* **클러스터 관리 이벤트**:
    * 이러한 이벤트는 자동으로 생성되며 {{site.data.keyword.cloudaccesstrailshort}}에 전달됩니다.
    * 이러한 이벤트는 {{site.data.keyword.cloudaccesstrailshort}} **계정 도메인**을 통해 볼 수 있습니다.

* **Kubernetes API 서버 감사 이벤트**:
    * 이러한 이벤트는 자동으로 생성되지만, 이러한 이벤트를 {{site.data.keyword.cloudaccesstrailshort}} 서비스로 전달하기 위해서는 사용자가 클러스터를 구성해야 합니다.
    * 사용자는 이벤트를 {{site.data.keyword.cloudaccesstrailshort}} **계정 도메인** 또는 **영역 도메인**으로 전송하도록 클러스터를 구성할 수 있습니다. 자세한 정보는 [감사 로그 전송](/docs/containers?topic=containers-health#api_forward)을 참조하십시오.

서비스가 작동하는 방식에 대한 자세한 정보는 [{{site.data.keyword.cloudaccesstrailshort}} 문서](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started)를 참조하십시오. 추적되는 Kubernetes 조치에 대한 자세한 정보는 [Kubernetes 문서 ![추적 외부 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/home/)를 참조하십시오.

{{site.data.keyword.containerlong_notm}}는 현재 {{site.data.keyword.at_full}}를 사용하도록 구성되어 있지 않습니다. 클러스터 관리 이벤트 및 Kubernetes API 감사 로그를 관리하려면 계속해서 {{site.data.keyword.cloudaccesstrailfull_notm}}을 Log Analysis와 함께 사용하십시오.
{: note}

## 이벤트에 대한 정보 찾기
{: #kube-find}

Kibana 대시보드에서 로그를 확인하여 클러스터의 활동을 모니터링할 수 있습니다.
{: shortdesc}

관리 활동을 모니터링하려면 다음을 수행하십시오.

1. {{site.data.keyword.Bluemix_notm}} 계정에 로그인하십시오.
2. 카탈로그에서 {{site.data.keyword.containerlong_notm}}의 인스턴스와 동일한 계정으로 {{site.data.keyword.cloudaccesstrailshort}} 서비스의 인스턴스를 프로비저닝하십시오.
3. {{site.data.keyword.cloudaccesstrailshort}} 대시보드의 **관리** 탭에서 계정 또는 영역 도메인을 선택하십시오.
  * **계정 로그**: 클러스터 관리 이벤트 및 Kubernetes API 서버 감사 이벤트는 이벤트가 생성된 {{site.data.keyword.Bluemix_notm}} 지역의 **계정 도메인**에 있습니다.
  * **영역 로그**: Kubernetes API 서버 감사 이벤트를 전달할 클러스터를 구성할 때 영역을 지정한 경우 이러한 이벤트는 {{site.data.keyword.cloudaccesstrailshort}} 서비스가 프로비저닝된 Cloud Foundry 영역과 연관된 **영역 도메인**에 있습니다.
4. **Kibana에서 보기**를 클릭하십시오.
5. 해당 로그를 보고자 하는 시간 범위를 설정하십시오. 기본값은 24시간입니다.
6. 검색 결과를 좁히기 위해 `ActivityTracker_Account_Search_in_24h`의 편집 아이콘을 클릭하고 **사용 가능한 필드** 컬럼에 필드를 추가할 수 있습니다.

다른 사용자가 계정 및 영역 이벤트를 볼 수 있도록 허용하려면 [계정 이벤트를 볼 수 있도록 권한 부여](/docs/services/cloud-activity-tracker/how-to?topic=cloud-activity-tracker-grant_permissions#grant_permissions)를 참조하십시오.
{: tip}

## 클러스터 관리 이벤트 추적
{: #cluster-events}

{{site.data.keyword.cloudaccesstrailshort}}에 전송된 클러스터 관리 이벤트의 다음 목록을 확인하십시오.
{: shortdesc}

<table>
<tr>
<th>조치</th>
<th>설명</th></tr><tr>
<td><code>containers-kubernetes.account-credentials.set</code></td>
<td>리소스 그룹에 대한 지역에 인프라 인증 정보가 설정되었습니다.</td></tr><tr>
<td><code>containers-kubernetes.account-credentials.unset</code></td>
<td>리소스 그룹에 대한 지역에 인프라 인증 정보가 설정 해제되었습니다.</td></tr><tr>
<td><code>containers-kubernetes.alb.create</code></td>
<td>Ingress ALB가 작성되었습니다.</td></tr><tr>
<td><code>containers-kubernetes.alb.delete</code></td>
<td>Ingress ALB가 삭제되었습니다.</td></tr><tr>
<td><code>containers-kubernetes.alb.get</code></td>
<td>Ingress ALB 정보가 열람되었습니다.</td></tr><tr>
<td><code>containers-kubernetes.apikey.reset</code></td>
<td>지역 및 리소스 그룹에 대해 API 키가 재설정되었습니다.</td></tr><tr>
<td><code>containers-kubernetes.cluster.create</code></td>
<td>클러스터가 작성되었습니다.</td></tr><tr>
<td><code>containers-kubernetes.cluster.delete</code></td>
<td>클러스터가 삭제되었습니다.</td></tr><tr>
<td><code>containers-kubernetes.cluster-feature.enable</code></td>
<td>베어메탈 작업자 노드를 위한 신뢰할 수 있는 컴퓨팅과 같은 기능이 클러스터에서 사용으로 설정되었습니다.</td></tr><tr>
<td><code>containers-kubernetes.cluster.get</code></td>
<td>클러스터 정보가 열람되었습니다.</td></tr><tr>
<td><code>containers-kubernetes.logging-config.create</code></td>
<td>로그 전달 구성이 작성되었습니다.</td></tr><tr>
<td><code>containers-kubernetes.logging-config.delete</code></td>
<td>로그 전달 구성이 삭제되었습니다.</td></tr><tr>
<td><code>containers-kubernetes.logging-config.get</code></td>
<td>로그 전달 구성에 대한 정보가 열람되었습니다.</td></tr><tr>
<td><code>containers-kubernetes.logging-config.update</code></td>
<td>로그 전달 구성이 업데이트되었습니다.</td></tr><tr>
<td><code>containers-kubernetes.logging-config.refresh</code></td>
<td>로그 전달 구성이 새로 고쳐졌습니다.</td></tr><tr>
<td><code>containers-kubernetes.logging-filter.create</code></td>
<td>로깅 필터가 작성되었습니다.</td></tr><tr>
<td><code>containers-kubernetes.logging-filter.delete</code></td>
<td>로깅 필터가 삭제되었습니다.</td></tr><tr>
<td><code>containers-kubernetes.logging-filter.get</code></td>
<td>로깅 필터에 대한 정보가 열람되었습니다.</td></tr><tr>
<td><code>containers-kubernetes.logging-filter.update</code></td>
<td>로깅 필터가 업데이트되었습니다.</td></tr><tr>
<td><code>containers-kubernetes.logging-autoupdate.changed</code></td>
<td>로깅 추가 기능 자동 업데이트 프로그램이 사용 또는 사용 안함으로 설정되었습니다.</td></tr><tr>
<td><code>containers-kubernetes.mzlb.create</code></td>
<td>다중 구역 로드 밸런서가 작성되었습니다.</td></tr><tr>
<td><code>containers-kubernetes.mzlb.delete</code></td>
<td>다중 구역 로드 밸런서가 삭제되었습니다.</td></tr><tr>
<td><code>containers-kubernetes.service.bind</code></td>
<td>서비스가 클러스터에 바인드되었습니다.</td></tr><tr>
<td><code>containers-kubernetes.service.unbind</code></td>
<td>서비스가 클러스터로부터 바인드 해제되었습니다.</td></tr><tr>
<td><code>containers-kubernetes.subnet.add</code></td>
<td>기존 IBM Cloud 인프라(SoftLayer) 서브넷이 클러스터에 추가되었습니다.</td></tr><tr>
<td><code>containers-kubernetes.subnet.create</code></td>
<td>서브넷이 작성되었습니다.</td></tr><tr>
<td><code>containers-kubernetes.usersubnet.add</code></td>
<td>사용자 관리 서브넷이 클러스터에 추가되었습니다.</td></tr><tr>
<td><code>containers-kubernetes.usersubnet.delete</code></td>
<td>사용자 관리 서브넷이 클러스터에서 삭제되었습니다.</td></tr><tr>
<td><code>containers-kubernetes.version.update</code></td>
<td>클러스터 마스터 노드의 Kubernetes 버전이 업데이트되었습니다.</td></tr><tr>
<td><code>containers-kubernetes.worker.create</code></td>
<td>작업자 노드가 작성되었습니다.</td></tr><tr>
<td><code>containers-kubernetes.worker.delete</code></td>
<td>작업자 노드가 삭제되었습니다.</td></tr><tr>
<td><code>containers-kubernetes.worker.get</code></td>
<td>작업자 노드에 대한 정보가 열람되었습니다.</td></tr><tr>
<td><code>containers-kubernetes.worker.reboot</code></td>
<td>작업자 노드가 다시 부팅되었습니다.</td></tr><tr>
<td><code>containers-kubernetes.worker.reload</code></td>
<td>작업자 노드가 다시 로드되었습니다.</td></tr><tr>
<td><code>containers-kubernetes.worker.update</code></td>
<td>작업자 노드가 업데이트되었습니다.</td></tr>
</table>

## Kubernetes 감사 이벤트 추적
{: #kube-events}

{{site.data.keyword.cloudaccesstrailshort}}에 전송되는 Kubernetes API 서버 감사 이벤트의 목록은 다음 표를 확인하십시오.
{: shortdesc}

시작하기 전에: 클러스터가 [Kubernetes API 감사 이벤트](/docs/containers?topic=containers-health#api_forward)를 전달하도록 구성되어 있는지 확인하십시오.

<table>
    <th>조치</th>
    <th>설명</th><tr>
    <td><code>bindings.create</code></td>
    <td>바인딩이 작성되었습니다.</td></tr><tr>
    <td><code>certificatesigningrequests.create</code></td>
    <td>인증서 서명 요청이 작성되었습니다.</td></tr><tr>
    <td><code>certificatesigningrequests.delete</code></td>
    <td>인증서 서명 요청이 삭제되었습니다.</td></tr><tr>
    <td><code>certificatesigningrequests.patch</code></td>
    <td>인증서 서명 요청이 패치되었습니다.</td></tr><tr>
    <td><code>certificatesigningrequests.update</code></td>
    <td>인증서 서명 요청이 업데이트되었습니다.</td></tr><tr>
    <td><code>clusterbindings.create</code></td>
    <td>클러스터 역할 바인딩이 작성되었습니다.</td></tr><tr>
    <td><code>clusterbindings.deleted</code></td>
    <td>클러스터 역할 바인딩이 삭제되었습니다.</td></tr><tr>
    <td><code>clusterbindings.patched</code></td>
    <td>클러스터 역할 바인딩이 패치되었습니다.</td></tr><tr>
    <td><code>clusterbindings.updated</code></td>
    <td>클러스터 역할 바인딩이 업데이트되었습니다.</td></tr><tr>
    <td><code>clusterroles.create</code></td>
    <td>클러스터 역할이 작성되었습니다.</td></tr><tr>
    <td><code>clusterroles.deleted</code></td>
    <td>클러스터 역할이 삭제되었습니다.</td></tr><tr>
    <td><code>clusterroles.patched</code></td>
    <td>클러스터 역할이 패치되었습니다.</td></tr><tr>
    <td><code>clusterroles.updated</code></td>
    <td>클러스터 역할이 업데이트되었습니다.</td></tr><tr>
    <td><code>configmaps.create</code></td>
    <td>구성 맵이 작성되었습니다.</td></tr><tr>
    <td><code>configmaps.delete</code></td>
    <td>구성 맵이 삭제되었습니다.</td></tr><tr>
    <td><code>configmaps.patch</code></td>
    <td>구성 맵이 패치되었습니다.</td></tr><tr>
    <td><code>configmaps.update</code></td>
    <td>구성 맵이 업데이트되었습니다.</td></tr><tr>
    <td><code>controllerrevisions.create</code></td>
    <td>제어기 개정이 작성되었습니다.</td></tr><tr>
    <td><code>controllerrevisions.delete</code></td>
    <td>제어기 개정이 삭제되었습니다.</td></tr><tr>
    <td><code>controllerrevisions.patch</code></td>
    <td>제어기 개정이 패치되었습니다.</td></tr><tr>
    <td><code>controllerrevisions.update</code></td>
    <td>제어기 개정이 업데이트되었습니다.</td></tr><tr>
    <td><code>daemonsets.create</code></td>
    <td>디먼 세트가 작성되었습니다.</td></tr><tr>
    <td><code>daemonsets.delete</code></td>
    <td>디먼 세트가 삭제되었습니다.</td></tr><tr>
    <td><code>daemonsets.patch</code></td>
    <td>디먼 세트가 패치되었습니다.</td></tr><tr>
    <td><code>daemonsets.update</code></td>
    <td>디먼 세트가 업데이트되었습니다.</td></tr><tr>
    <td><code>deployments.create</code></td>
    <td>배치가 작성되었습니다.</td></tr><tr>
    <td><code>deployments.delete</code></td>
    <td>배치가 삭제되었습니다.</td></tr><tr>
    <td><code>deployments.patch</code></td>
    <td>배치가 패치되었습니다.</td></tr><tr>
    <td><code>deployments.update</code></td>
    <td>배치가 업데이트되었습니다.</td></tr><tr>
    <td><code>events.create</code></td>
    <td>이벤트가 작성되었습니다.</td></tr><tr>
    <td><code>events.delete</code></td>
    <td>이벤트가 삭제되었습니다.</td></tr><tr>
    <td><code>events.patch</code></td>
    <td>이벤트가 패치되었습니다.</td></tr><tr>
    <td><code>events.update</code></td>
    <td>이벤트가 업데이트되었습니다.</td></tr><tr>
    <td><code>externaladmissionhookconfigurations.create</code></td>
    <td>Kubernetes v1.8에서 외부 허용 훅 구성이 작성되었습니다.</td></tr><tr>
    <td><code>externaladmissionhookconfigurations.delete</code></td>
    <td>Kubernetes v1.8에서 외부 허용 훅 구성이 삭제되었습니다.</td></tr><tr>
    <td><code>externaladmissionhookconfigurations.patch</code></td>
    <td>Kubernetes v1.8에서 외부 허용 훅 구성이 패치되었습니다.</td></tr><tr>
    <td><code>externaladmissionhookconfigurations.update</code></td>
    <td>Kubernetes v1.8에서 외부 허용 훅 구성이 업데이트되었습니다.</td></tr><tr>
    <td><code>horizontalpodautoscalers.create</code></td>
    <td>수평 팟(Pod) Auto-Scaling 정책이 작성되었습니다.</td></tr><tr>
    <td><code>horizontalpodautoscalers.delete</code></td>
    <td>수평 팟(Pod) Auto-Scaling 정책이 삭제되었습니다.</td></tr><tr>
    <td><code>horizontalpodautoscalers.patch</code></td>
    <td>수평 팟(Pod) Auto-Scaling 정책이 패치되었습니다.</td></tr><tr>
    <td><code>horizontalpodautoscalers.update</code></td>
    <td>수평 팟(Pod) Auto-Scaling 정책이 업데이트되었습니다.</td></tr><tr>
    <td><code>ingresses.create</code></td>
    <td>Ingress ALB가 작성되었습니다.</td></tr><tr>
    <td><code>ingresses.delete</code></td>
    <td>Ingress ALB가 삭제되었습니다.</td></tr><tr>
    <td><code>ingresses.patch</code></td>
    <td>Ingress ALB가 패치되었습니다.</td></tr><tr>
    <td><code>ingresses.update</code></td>
    <td>Ingress ALB가 업데이트되었습니다.</td></tr><tr>
    <td><code>jobs.create</code></td>
    <td>작업이 작성되었습니다.</td></tr><tr>
    <td><code>jobs.delete</code></td>
    <td>작업이 삭제되었습니다.</td></tr><tr>
    <td><code>jobs.patch</code></td>
    <td>작업이 패치되었습니다.</td></tr><tr>
    <td><code>jobs.update</code></td>
    <td>작업이 업데이트되었습니다.</td></tr><tr>
    <td><code>localsubjectaccessreviews.create</code></td>
    <td>로컬 주제 액세스 검토가 작성되었습니다.</td></tr><tr>
    <td><code>limitranges.create</code></td>
    <td>범위 한계가 작성되었습니다.</td></tr><tr>
    <td><code>limitranges.delete</code></td>
    <td>범위 한계가 삭제되었습니다.</td></tr><tr>
    <td><code>limitranges.patch</code></td>
    <td>범위 한계가 패치되었습니다.</td></tr><tr>
    <td><code>limitranges.update</code></td>
    <td>범위 한계가 업데이트되었습니다.</td></tr><tr>
    <td><code>mutatingwebhookconfigurations.create</code></td>
    <td>변형된 웹훅 구성이 작성되었습니다. </td></tr><tr>
    <td><code>mutatingwebhookconfigurations.delete</code></td>
    <td>변형된 웹훅 구성이 삭제되었습니다. </td></tr><tr>
    <td><code>mutatingwebhookconfigurations.patch</code></td>
    <td>변형된 웹훅 구성이 패치되었습니다. </td></tr><tr>
    <td><code>mutatingwebhookconfigurations.update</code></td>
    <td>변형된 웹훅 구성이 업데이트되었습니다. </td></tr><tr>
    <td><code>namespaces.create</code></td>
    <td>네임스페이스가 작성되었습니다.</td></tr><tr>
    <td><code>namespaces.delete</code></td>
    <td>네임스페이스가 삭제되었습니다.</td></tr><tr>
    <td><code>namespaces.patch</code></td>
    <td>네임스페이스가 패치되었습니다.</td></tr><tr>
    <td><code>namespaces.update</code></td>
    <td>네임스페이스가 업데이트되었습니다.</td></tr><tr>
    <td><code>networkpolicies.create</code></td>
    <td>네트워크 정책이 작성되었습니다.</td></tr><tr><tr>
    <td><code>networkpolicies.delete</code></td>
    <td>네트워크 정책이 삭제되었습니다.</td></tr><tr>
    <td><code>networkpolicies.patch</code></td>
    <td>네트워크 정책이 패치되었습니다.</td></tr><tr>
    <td><code>networkpolicies.update</code></td>
    <td>네트워크 정책이 업데이트되었습니다.</td></tr><tr>
    <td><code>nodes.create</code></td>
    <td>노드가 작성되었습니다.</td></tr><tr>
    <td><code>nodes.delete</code></td>
    <td>노드가 삭제되었습니다.</td></tr><tr>
    <td><code>nodes.patch</code></td>
    <td>노드가 패치되었습니다.</td></tr><tr>
    <td><code>nodes.update</code></td>
    <td>노드가 업데이트되었습니다.</td></tr><tr>
    <td><code>persistentvolumeclaims.create</code></td>
    <td>지속적 볼륨 클레임이 작성되었습니다.</td></tr><tr>
    <td><code>persistentvolumeclaims.delete</code></td>
    <td>지속적 볼륨 클레임이 삭제되었습니다.</td></tr><tr>
    <td><code>persistentvolumeclaims.patch</code></td>
    <td>지속적 볼륨 클레임이 패치되었습니다.</td></tr><tr>
    <td><code>persistentvolumeclaims.update</code></td>
    <td>지속적 볼륨 클레임이 업데이트되었습니다.</td></tr><tr>
    <td><code>persistentvolumes.create</code></td>
    <td>지속적 볼륨이 작성되었습니다.</td></tr><tr>
    <td><code>persistentvolumes.delete</code></td>
    <td>지속적 볼륨이 삭제되었습니다.</td></tr><tr>
    <td><code>persistentvolumes.patch</code></td>
    <td>지속적 볼륨이 패치되었습니다.</td></tr><tr>
    <td><code>persistentvolumes.update</code></td>
    <td>지속적 볼륨이 업데이트되었습니다.</td></tr><tr>
    <td><code>poddisruptionbudgets.create</code></td>
    <td>팟(Pod) 작동 중단 한계가 작성되었습니다.</td></tr><tr>
    <td><code>poddisruptionbudgets.delete</code></td>
    <td>팟(Pod) 작동 중단 한계가 삭제되었습니다.</td></tr><tr>
    <td><code>poddisruptionbudgets.patch</code></td>
    <td>팟(Pod) 작동 중단 한계가 패치되었습니다.</td></tr><tr>
    <td><code>poddisruptionbudgets.update</code></td>
    <td>팟(Pod) 작동 중단 한계가 업데이트되었습니다.</td></tr><tr>
    <td><code>podpresets.create</code></td>
    <td>팟(Pod) 사전 설정이 작성되었습니다.</td></tr><tr>
    <td><code>podpresets.deleted</code></td>
    <td>팟(Pod) 사전 설정이 삭제되었습니다.</td></tr><tr>
    <td><code>podpresets.patched</code></td>
    <td>팟(Pod) 사전 설정이 패치되었습니다.</td></tr><tr>
    <td><code>podpresets.updated</code></td>
    <td>팟(Pod) 사전 설정이 업데이트되었습니다.</td></tr><tr>
    <td><code>pods.create</code></td>
    <td>팟(Pod)이 작성되었습니다.</td></tr><tr>
    <td><code>pods.delete</code></td>
    <td>팟(Pod)이 삭제되었습니다.</td></tr><tr>
    <td><code>pods.patch</code></td>
    <td>팟(Pod)이 패치되었습니다.</td></tr><tr>
    <td><code>pods.update</code></td>
    <td>팟(Pod)이 업데이트되었습니다.</td></tr><tr>
    <td><code>podsecuritypolicies.create</code></td>
    <td>팟(Pod) 보안 정책이 작성되었습니다.</td></tr><tr>
    <td><code>podsecuritypolicies.delete</code></td>
    <td>팟(Pod) 보안 정책이 삭제되었습니다.</td></tr><tr>
    <td><code>podsecuritypolicies.patch</code></td>
    <td>팟(Pod) 보안 정책이 패치되었습니다.</td></tr><tr>
    <td><code>podsecuritypolicies.update</code></td>
    <td>팟(Pod) 보안 정책이 업데이트되었습니다.</td></tr><tr>
    <td><code>podtemplates.create</code></td>
    <td>팟(Pod) 템플리트가 작성되었습니다.</td></tr><tr>
    <td><code>podtemplates.delete</code></td>
    <td>팟(Pod) 템플리트가 삭제되었습니다.</td></tr><tr>
    <td><code>podtemplates.patch</code></td>
    <td>팟(Pod) 템플리트가 패치되었습니다.</td></tr><tr>
    <td><code>podtemplates.update</code></td>
    <td>팟(Pod) 템플리트가 업데이트되었습니다.</td></tr><tr>
    <td><code>replicasets.create</code></td>
    <td>복제본 세트가 작성되었습니다.</td></tr><tr>
    <td><code>replicasets.delete</code></td>
    <td>복제본 세트가 삭제되었습니다.</td></tr><tr>
    <td><code>replicasets.patch</code></td>
    <td>복제본 세트가 패치되었습니다.</td></tr><tr>
    <td><code>replicasets.update</code></td>
    <td>복제본 세트가 업데이트되었습니다.</td></tr><tr>
    <td><code>replicationcontrollers.create</code></td>
    <td>복제 제어기가 작성되었습니다.</td></tr><tr>
    <td><code>replicationcontrollers.delete</code></td>
    <td>복제 제어기가 삭제되었습니다.</td></tr><tr>
    <td><code>replicationcontrollers.patch</code></td>
    <td>복제 제어기가 패치되었습니다.</td></tr><tr>
    <td><code>replicationcontrollers.update</code></td>
    <td>복제 제어기가 업데이트되었습니다.</td></tr><tr>
    <td><code>resourcequotas.create</code></td>
    <td>리소스 할당량이 작성되었습니다.</td></tr><tr>
    <td><code>resourcequotas.delete</code></td>
    <td>리소스 할당량이 삭제되었습니다.</td></tr><tr>
    <td><code>resourcequotas.patch</code></td>
    <td>리소스 할당량이 패치되었습니다.</td></tr><tr>
    <td><code>resourcequotas.update</code></td>
    <td>리소스 할당량이 업데이트되었습니다.</td></tr><tr>
    <td><code>rolebindings.create</code></td>
    <td>역할 바인딩이 작성되었습니다.</td></tr><tr>
    <td><code>rolebindings.deleted</code></td>
    <td>역할 바인딩이 삭제되었습니다.</td></tr><tr>
    <td><code>rolebindings.patched</code></td>
    <td>역할 바인딩이 패치되었습니다.</td></tr><tr>
    <td><code>rolebindings.updated</code></td>
    <td>역할 바인딩이 업데이트되었습니다.</td></tr><tr>
    <td><code>roles.create</code></td>
    <td>역할이 작성되었습니다.</td></tr><tr>
    <td><code>roles.deleted</code></td>
    <td>역할이 삭제되었습니다.</td></tr><tr>
    <td><code>roles.patched</code></td>
    <td>역할이 패치되었습니다.</td></tr><tr>
    <td><code>roles.updated</code></td>
    <td>역할이 업데이트되었습니다.</td></tr><tr>
    <td><code>secrets.create</code></td>
    <td>시크릿이 작성되었습니다.</td></tr><tr>
    <td><code>secrets.deleted</code></td>
    <td>시크릿이 삭제되었습니다.</td></tr><tr>
    <td><code>secrets.get</code></td>
    <td>시크릿이 열람되었습니다.</td></tr><tr>
    <td><code>secrets.patch</code></td>
    <td>시크릿이 패치되었습니다.</td></tr><tr>
    <td><code>secrets.updated</code></td>
    <td>시크릿이 업데이트되었습니다.</td></tr><tr>
    <td><code>selfsubjectaccessreviews.create</code></td>
    <td>셀프 주제 액세스 검토가 작성되었습니다.</td></tr><tr>
    <td><code>selfsubjectrulesreviews.create</code></td>
    <td>셀프 주제 규칙 검토가 작성되었습니다.</td></tr><tr>
    <td><code>subjectaccessreviews.create</code></td>
    <td>주제 액세스 검토가 작성되었습니다.</td></tr><tr>
    <td><code>serviceaccounts.create</code></td>
    <td>서비스 계정이 작성되었습니다.</td></tr><tr>
    <td><code>serviceaccounts.deleted</code></td>
    <td>서비스 계정이 삭제되었습니다.</td></tr><tr>
    <td><code>serviceaccounts.patch</code></td>
    <td>서비스 계정이 패치되었습니다.</td></tr><tr>
    <td><code>serviceaccounts.updated</code></td>
    <td>서비스 계정이 업데이트되었습니다.</td></tr><tr>
    <td><code>services.create</code></td>
    <td>서비스가 작성되었습니다.</td></tr><tr>
    <td><code>services.deleted</code></td>
    <td>서비스가 삭제되었습니다.</td></tr><tr>
    <td><code>services.patch</code></td>
    <td>서비스가 패치되었습니다.</td></tr><tr>
    <td><code>services.updated</code></td>
    <td>서비스가 업데이트되었습니다.</td></tr><tr>
    <td><code>statefulsets.create</code></td>
    <td>stateful 세트가 작성되었습니다.</td></tr><tr>
    <td><code>statefulsets.delete</code></td>
    <td>stateful 세트가 삭제되었습니다.</td></tr><tr>
    <td><code>statefulsets.patch</code></td>
    <td>stateful 세트가 패치되었습니다.</td></tr><tr>
    <td><code>statefulsets.update</code></td>
    <td>stateful 세트가 업데이트되었습니다.</td></tr><tr>
    <td><code>tokenreviews.create</code></td>
    <td>토큰 검토가 작성되었습니다.</td></tr><tr>
    <td><code>validatingwebhookconfigurations.create</code></td>
    <td>웹훅 구성 유효성 검증이 작성되었습니다. </td></tr><tr>
    <td><code>validatingwebhookconfigurations.delete</code></td>
    <td>웹훅 구성 유효성 검증이 삭제되었습니다. </td></tr><tr>
    <td><code>validatingwebhookconfigurations.patch</code></td>
    <td>웹훅 구성 유효성 검증이 패치되었습니다. </td></tr><tr>
    <td><code>validatingwebhookconfigurations.update</code></td>
    <td>웹훅 구성 유효성 검증이 업데이트되었습니다. </td></tr>
</table>
