---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-05"

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


# 버전 변경 로그
{: #changelog}

{{site.data.keyword.containerlong}} Kubernetes 클러스터에 대해 사용 가능한 주 버전, 부 버전 및 패치 업데이트의 버전 변경 정보를 보십시오. 변경사항에는 Kubernetes 및 {{site.data.keyword.Bluemix_notm}} Provider 컴포넌트에 대한 업데이트가 포함됩니다.
{:shortdesc}

변경 로그에서 달리 언급되지 않는 한, {{site.data.keyword.containerlong_notm}} 제공자 버전은 베타에서 제공되는 Kubernetes API 및 기능을 사용으로 설정합니다. Kubernetes 알파 기능은 변경될 수 있으며 사용 안함으로 설정됩니다. 

부 버전 간의 준비 조치와 주 버전, 부 버전 및 패치 버전에 대한 자세한 정보는 [Kubernetes 버전](/docs/containers?topic=containers-cs_versions)을 참조하십시오.
{: tip}

이전 버전에서 변경된 사항에 대한 정보는 다음 변경 로그를 보십시오.
-  버전 1.14 [변경 로그](#114_changelog).
-  버전 1.13 [변경 로그](#113_changelog).
-  버전 1.12 [변경 로그](#112_changelog).
-  **더 이상 사용되지 않음**: 버전 1.11 [변경 로그](#111_changelog).
-  지원되지 않는 버전에 대한 변경 로그의 [아카이브](#changelog_archive).

일부는 _작업자 노드 수정팩_의 변경 로그이며, 이는 작업자 노드에만 적용됩니다. 작업자 노드가 확실히 규제를 준수하도록 하려면 [이러한 패치를 적용](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update)해야 합니다. 일부 빌드 수정팩은 작업자 노드에만 한정되므로 이러한 작업자 노드 수정팩은 마스터보다 더 상위 버전일 수 있습니다. 그 외의 변경 로그는 _마스터 수정팩_에 대한 것이며 클러스터 마스터에만 적용됩니다. 마스터 수정팩은 자동으로 적용되지 않을 수 있습니다. 이는 [수동으로 적용](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_update)하도록 선택할 수 있습니다. 패치 유형에 대한 자세한 정보는 [업데이트 유형](/docs/containers?topic=containers-cs_versions#update_types)을 참조하십시오.
{: note}

</br>

## 버전 1.14 변경 로그
{: #114_changelog}

### 2019년 6월 4일에 릴리스된 1.14.2_1521에 대한 변경 로그
{: #1142_1521}

다음 표는 패치 1.14.2_1521에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.14.1_1519 이후에 작성된 변경사항">
<caption>버전 1.14.1_1519 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>클러스터 DNS 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>클러스터 `create` 또는 `update` 오퍼레이션 이후 Kubernetes DNS 및 CoreDNS 팟(Pod)이 둘 다 실행 상태로 유지될 수 있는 버그가 해결되었습니다.</td>
</tr>
<tr>
<td>클러스터 마스터 HA 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>마스터 업데이트 중에 간헐적인 마스터 네트워크 연결 실패를 최소화할 수 있도록 구성이 업데이트되었습니다.</td>
</tr>
<tr>
<td>etcd</td>
<td>v3.3.11</td>
<td>v3.3.13</td>
<td>[etcd 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/coreos/etcd/releases/v3.3.13)를 참조하십시오.</td>
</tr>
<tr>
<td>GPU 디바이스 플러그인 및 설치 프로그램</td>
<td>55c1f66</td>
<td>32257d3</td>
<td>[CVE-2018-10844 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844), [CVE-2018-10845 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845), [CVE-2018-10846 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846), [CVE-2019-3829 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829), [CVE-2019-3836 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836), [CVE-2019-9893 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893), [CVE-2019-5435 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435) 및 [CVE-2019-5436 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436)의 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.14.1-71</td>
<td>v1.14.2-100</td>
<td>Kubernetes 1.14.2 릴리스를 지원하도록 업데이트되었습니다. </td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.14.1</td>
<td>v1.14.2</td>
<td>[Kubernetes 릴리스 정보![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/kubernetes/releases/tag/v1.14.2)를 참조하십시오.</td>
</tr>
<tr>
<td>Kubernetes 메트릭 서버</td>
<td>v0.3.1</td>
<td>v0.3.3</td>
<td>[Kubernetes 메트릭 서버 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes-incubator/metrics-server/releases/tag/v0.3.3)를 참조하십시오.</td>
</tr>
<tr>
<td>신뢰할 수 있는 컴퓨팅 에이전트</td>
<td>13c7ef0</td>
<td>e8c6d72</td>
<td>[CVE-2018-10844 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844), [CVE-2018-10845 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845), [CVE-2018-10846 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846), [CVE-2019-3829 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829), [CVE-2019-3836 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836), [CVE-2019-9893 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893), [CVE-2019-5435 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435) 및 [CVE-2019-5436 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436)의 이미지가 업데이트되었습니다.</td>
</tr>
</tbody>
</table>

### 2019년 5월 20일에 릴리스된 작업자 노드 수정팩 1.14.1_1519에 대한 변경 로그
{: #1141_1519}

다음 표는 패치 1.14.1_1519에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.14.1_1518 이후에 작성된 변경사항">
<caption>버전 1.14.1_1518 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 16.04 커널</td>
<td>4.4.0-145-generic</td>
<td>4.4.0-148-generic</td>
<td>[CVE-2018-12126 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html), [CVE-2018-12127 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html) 및 [CVE-2018-12130 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html)의 커널 업데이트로 작업자 노드 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>Ubuntu 18.04 커널</td>
<td>4.15.0-47-generic</td>
<td>4.15.0-50-generic</td>
<td>[CVE-2018-12126 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html), [CVE-2018-12127 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html) 및 [CVE-2018-12130 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html)의 커널 업데이트로 작업자 노드 이미지가 업데이트되었습니다.</td>
</tr>
</tbody>
</table>

### 2019년 5월 13일에 릴리스된 1.14.1_1518에 대한 변경 로그
{: #1141_1518}

다음 표는 패치 1.14.1_1518에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.14.1_1516 이후에 작성된 변경사항">
<caption>버전 1.14.1_1516 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>클러스터 마스터 HA 프록시</td>
<td>1.9.6-alpine</td>
<td>1.9.7-alpine</td>
<td>[HAProxy 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.haproxy.org/download/1.9/src/CHANGELOG)를 참조하십시오. 업데이트는 [CVE-2019-6706 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6706)을 해결합니다.</td>
</tr>
<tr>
<td>Kubernetes 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>Kubernetes API 서버 감사 정책 구성이 `/openapi/v2*` 읽기 전용 URL을 로깅하지 않도록 업데이트되었습니다. 또한 Kubernetes 제어기 관리자 구성에서는 서명된 `kubelet` 인증서의 유효성 검증 기간을 1년에서 3년으로 늘렸습니다. </td>
</tr>
<tr>
<td>OpenVPN 클라이언트 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>클러스터 DNS가 작동 중지될 때 팟(Pod)이 실패하지 않도록 `kube-system` 네임스페이스의 OpenVPN 클라이언트 `vpn-*` 팟(Pod)이 이제 `dnsPolicy`를 `Default`로 설정합니다.</td>
</tr>
<tr>
<td>신뢰할 수 있는 컴퓨팅 에이전트</td>
<td>e7182c7</td>
<td>13c7ef0</td>
<td>[CVE-2016-7076 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-7076) 및 [CVE-2017-1000368 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1000368)의 이미지가 업데이트되었습니다.</td>
</tr>
</tbody>
</table>

### 2019년 5월 7일에 릴리스된 1.14.1_1516에 대한 변경 로그
{: #1141_1516}

다음 표는 패치 1.14.1_1516에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.13.5_1519 이후에 작성된 변경사항">
<caption>버전 1.13.5_1519 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.4.4</td>
<td>v3.6.1</td>
<td>[Calico 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.projectcalico.org/v3.6/release-notes/)를 참조하십시오.</td>
</tr>
<tr>
<td>CoreDNS</td>
<td>1.2.6</td>
<td>1.3.1</td>
<td>[CoreDNS 릴리스 정보![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://coredns.io/2019/01/13/coredns-1.3.1-release/)를 참조하십시오. 업데이트에는 [메트릭 포트 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://coredns.io/plugins/metrics/) 클러스터 DNS 서비스의 추가가 포함됩니다.<br><br>CoreDNS는 이제 지원되는 유일한 클러스터 DNS 제공자입니다. 클러스터를 이전 버전 및 사용한 KubeDNS에서 Kubernetes 버전 1.14로 업데이트하는 경우 KubeDNS는 클러스터 업데이트 중에 자동으로 CoreDNS로 마이그레이션됩니다. 자세한 정보를 확인하거나 업데이트하기 전에 CoreDNS를 테스트하려면 [클러스터 DNS 제공자 구성](https://cloud.ibm.com/docs/containers?topic=containers-cluster_dns#cluster_dns)을 참조하십시오.</td>
</tr>
<tr>
<td>GPU 디바이스 플러그인 및 설치 프로그램</td>
<td>9ff3fda</td>
<td>ed0dafc</td>
<td>[CVE-2019-1543 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543)의 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.13.5-107</td>
<td>v1.14.1-71</td>
<td>Kubernetes 1.14.1 릴리스를 지원하도록 업데이트되었습니다. 또한 `calicoctl` 버전이 3.6.1로 업데이트되었습니다. 로드 밸런서 팟(Pod)에 사용 가능한 하나의 작업자 노드만 포함하여 버전 2.0 로드 밸런서에 대한 업데이트를 수정했습니다. 사설 로드 밸런서는 이제 [사설 에지 작업자 노드](/docs/containers?topic=containers-edge#edge)에서의 실행을 지원합니다.</td>
</tr>
<tr>
<td>IBM 팟(Pod) 보안 정책</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>[IBM 팟(Pod) 보안 정책](/docs/containers?topic=containers-psp#ibm_psp)은 Kubernetes [RunAsGroup ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/policy/pod-security-policy/#users-and-groups) 기능을 지원하도록 업데이트되었습니다.</td>
</tr>
<tr>
<td>`kubelet` 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>작업자 노드에서 단일 팟(Pod)이 모든 프로세스 ID를 이용하지 않도록 `--pod-max-pids` 옵션을 `14336`으로 설정합니다.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.13.5</td>
<td>v1.14.1</td>
<td>[Kubernetes 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/kubernetes/releases/tag/v1.14.1) 및 [Kubernetes 1.14 블로그 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/blog/2019/03/25/kubernetes-1-14-release-announcement/)를 참조하십시오.<br><br>Kubernetes 기반 역할 기반 액세스 제어(RBAC) 정책에서는 더 이상 [권한이 없는 사용자에 대한 검색 및 권한 검사 API ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#discovery-roles)에 대한 액세스 권한을 부여하지 않습니다. 이 변경사항은 새 버전 1.14 클러스터에만 적용됩니다. 이전 버전에서 클러스터를 업데이트하는 경우 권한이 없는 사용자는 계속해서 검색 및 권한 검사 API에 액세스할 수 있습니다. </td>
</tr>
<tr>
<td>Kubernetes 허가 제어기 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td><ul>
<li>클러스터의 Kubernetes API 서버에 대한 `--enable-admission-plugins` 옵션에 `NodeRestriction`이 추가되었으며 이 보안 개선사항을 지원하도록 관련 클러스터 리소스가 구성되었습니다. </li>
<li>API가 더 이상 지원되지 않으므로 클러스터의 Kubernetes API 서버에 대한 `--enable-admission-plugins` 옵션에서 `Initializers`를 제거하고 `--runtime-config` 옵션에서 `admissionregistration.k8s.io/v1alpha1=true`를 제거했습니다. 대신, [Kubernetes 승인 웹훅 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/reference/access-authn-authz/extensible-admission-controllers/)을 사용할 수 있습니다.</li></ul></td>
</tr>
<tr>
<td>Kubernetes DNS Autoscaler</td>
<td>1.3.0</td>
<td>1.4.0</td>
<td>[Kubernetes DNS Autoscaler 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes-incubator/cluster-proportional-autoscaler/releases/tag/1.4.0)를 참조하십시오.</td>
</tr>
<tr>
<td>Kubernetes 기능 게이트 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td><ul>
  <li>컨테이너 런타임 구성의 선택사항을 사용 안함으로 설정하도록 `RuntimeClass=false`를 추가했습니다. </li>
  <li>제거된 `ExperimentalCriticalPodAnnotation=true` because the `scheduler.alpha.kubernetes.io/critical-pod` 팟(Pod) 어노테이션은 더 이상 사용되지 않습니다. 대신, [Kubernetes 팟(Pod) 우선순위 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/docs/containers?topic=containers-pod_priority#pod_priority)를 사용할 수 있습니다.</li></ul></td>
</tr>
<tr>
<td>신뢰할 수 있는 컴퓨팅 에이전트</td>
<td>e132aa4</td>
<td>e7182c7</td>
<td>[CVE-2019-11068 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-11068)의 이미지가 업데이트되었습니다.</td>
</tr>
</tbody>
</table>

<br />


## 버전 1.13 변경 로그
{: #113_changelog}

### 2019년 6월 4일에 릴리스된 1.13.6_1524에 대한 변경 로그
{: #1136_1524}

다음 표는 패치 1.13.6_1524에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.13.6_1522 이후에 작성된 변경사항">
<caption>버전 1.13.6_1522 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>클러스터 DNS 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>클러스터 `create` 또는 `update` 오퍼레이션 이후 Kubernetes DNS 및 CoreDNS 팟(Pod)이 둘 다 실행 상태로 유지될 수 있는 버그가 해결되었습니다.</td>
</tr>
<tr>
<td>클러스터 마스터 HA 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>마스터 업데이트 중에 간헐적인 마스터 네트워크 연결 실패를 최소화할 수 있도록 구성이 업데이트되었습니다.</td>
</tr>
<tr>
<td>etcd</td>
<td>v3.3.11</td>
<td>v3.3.13</td>
<td>[etcd 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/coreos/etcd/releases/v3.3.13)를 참조하십시오.</td>
</tr>
<tr>
<td>GPU 디바이스 플러그인 및 설치 프로그램</td>
<td>55c1f66</td>
<td>32257d3</td>
<td>[CVE-2018-10844 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844), [CVE-2018-10845 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845), [CVE-2018-10846 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846), [CVE-2019-3829 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829), [CVE-2019-3836 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836), [CVE-2019-9893 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893), [CVE-2019-5435 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435) 및 [CVE-2019-5436 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436)의 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>Kubernetes 메트릭 서버</td>
<td>v0.3.1</td>
<td>v0.3.3</td>
<td>[Kubernetes 메트릭 서버 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes-incubator/metrics-server/releases/tag/v0.3.3)를 참조하십시오.</td>
</tr>
<tr>
<td>신뢰할 수 있는 컴퓨팅 에이전트</td>
<td>13c7ef0</td>
<td>e8c6d72</td>
<td>[CVE-2018-10844 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844), [CVE-2018-10845 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845), [CVE-2018-10846 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846), [CVE-2019-3829 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829), [CVE-2019-3836 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836), [CVE-2019-9893 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893), [CVE-2019-5435 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435) 및 [CVE-2019-5436 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436)의 이미지가 업데이트되었습니다.</td>
</tr>
</tbody>
</table>

### 2019년 5월 20일에 릴리스된 작업자 노드 수정팩 1.13.6_1522에 대한 변경 로그
{: #1136_1522}

다음 표는 패치 1.13.6_1522에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.13.6_1521 이후에 작성된 변경사항">
<caption>버전 1.13.6_1521 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 16.04 커널</td>
<td>4.4.0-145-generic</td>
<td>4.4.0-148-generic</td>
<td>[CVE-2018-12126 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html), [CVE-2018-12127 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html) 및 [CVE-2018-12130 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html)의 커널 업데이트로 작업자 노드 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>Ubuntu 18.04 커널</td>
<td>4.15.0-47-generic</td>
<td>4.15.0-50-generic</td>
<td>[CVE-2018-12126 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html), [CVE-2018-12127 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html) 및 [CVE-2018-12130 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html)의 커널 업데이트로 작업자 노드 이미지가 업데이트되었습니다.</td>
</tr>
</tbody>
</table>

### 2019년 5월 13일에 릴리스된 1.13.6_1521에 대한 변경 로그
{: #1136_1521}

다음 표는 패치 1.13.6_1521에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.13.5_1519 이후에 작성된 변경사항">
<caption>버전 1.13.5_1519 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>클러스터 마스터 HA 프록시</td>
<td>1.9.6-alpine</td>
<td>1.9.7-alpine</td>
<td>[HAProxy 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.haproxy.org/download/1.9/src/CHANGELOG)를 참조하십시오. 업데이트는 [CVE-2019-6706 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6706)을 해결합니다.</td>
</tr>
<tr>
<td>GPU 디바이스 플러그인 및 설치 프로그램</td>
<td>9ff3fda</td>
<td>55c1f66</td>
<td>[CVE-2019-1543 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543)의 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.13.5-107</td>
<td>v1.13.6-139</td>
<td>Kubernetes 1.13.6 릴리스를 지원하도록 업데이트되었습니다. 또한 로드 밸런서 팟(Pod)에 사용 가능한 하나의 작업자 노드만 있는 버전 2.0 로드 밸런서에 대한 업데이트도 수정했습니다. </td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.13.5</td>
<td>v1.13.6</td>
<td>[Kubernetes 릴리스 정보![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/kubernetes/releases/tag/v1.13.6)를 참조하십시오.</td>
</tr>
<tr>
<td>Kubernetes 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>Kubernetes API 서버 감사 정책 구성이 `/openapi/v2*` 읽기 전용 URL을 로깅하지 않도록 업데이트되었습니다. 또한 Kubernetes 제어기 관리자 구성에서는 서명된 `kubelet` 인증서의 유효성 검증 기간을 1년에서 3년으로 늘렸습니다. </td>
</tr>
<tr>
<td>OpenVPN 클라이언트 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>클러스터 DNS가 작동 중지될 때 팟(Pod)이 실패하지 않도록 `kube-system` 네임스페이스의 OpenVPN 클라이언트 `vpn-*` 팟(Pod)이 이제 `dnsPolicy`를 `Default`로 설정합니다.</td>
</tr>
<tr>
<td>신뢰할 수 있는 컴퓨팅 에이전트</td>
<td>e132aa4</td>
<td>13c7ef0</td>
<td>[CVE-2016-7076 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-7076), [CVE-2017-1000368 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1000368) 및 [CVE-2019-11068 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-11068)의 이미지가 업데이트되었습니다.</td>
</tr>
</tbody>
</table>

### 2019년 4월 29일에 릴리스된 작업자 노드 수정팩 1.13.5_1519에 대한 변경 로그
{: #1135_1519}

다음 표는 작업자 노드 수정팩 1.13.5_1519에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.13.5_1518 이후에 작성된 변경사항">
<caption>버전 1.13.5_1518 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 패키지</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>설치된 Ubuntu 패키지로 업데이트되었습니다.</td>
</tr>
<tr>
<td>containerd</td>
<td>1.2.5</td>
<td>1.2.6</td>
<td>[containerd 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/containerd/containerd/releases/tag/v1.2.6)를 참조하십시오.</td>
</tr>
</tbody>
</table>

### 2019년 4월 15일에 릴리스된 작업자 노드 수정팩 1.13.5_1518에 대한 변경 로그
{: #1135_1518}

다음 표는 작업자 노드 수정팩 1.13.5_1518에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.13.5_1517 이후에 작성된 변경사항">
<caption>버전 1.13.5_1517 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 패키지</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>[CVE-2019-3842 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-3842.html)에 대한 `systemd`를 포함하여 설치된 Ubuntu 패키지를 업데이트합니다.</td>
</tr>
</tbody>
</table>

### 2019년 4월 8일에 릴리스된 1.13.5_1517에 대한 변경 로그
{: #1135_1517}

다음 표는 패치 1.13.5_1517에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.13.4_1516 이후에 작성된 변경사항">
<caption>버전 1.13.4_1516 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.4.0</td>
<td>v3.4.4</td>
<td>[Calico 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.projectcalico.org/v3.4/releases/#v344)를 참조하십시오. 업데이트는 [CVE-2019-9946 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9946)을 해결합니다.</td>
</tr>
<tr>
<td>클러스터 마스터 HA 프록시</td>
<td>1.8.12-alpine</td>
<td>1.9.6-alpine</td>
<td>[HAProxy 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.haproxy.org/download/1.9/src/CHANGELOG)를 참조하십시오. 업데이트는 [CVE-2018-0732 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732), [CVE-2018-0734 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734), [CVE-2018-0737 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737), [CVE-2018-5407 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407), [CVE-2019-1543 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543) 및 [CVE-2019-1559 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559)를 해결합니다.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.13.4-86</td>
<td>v1.13.5-107</td>
<td>Kubernetes 1.13.5 및 Calico 3.4.4 릴리스를 지원하도록 업데이트되었습니다.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.13.4</td>
<td>v1.13.5</td>
<td>[Kubernetes 릴리스 정보![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/kubernetes/releases/tag/v1.13.5)를 참조하십시오.</td>
</tr>
<tr>
<td>신뢰할 수 있는 컴퓨팅 에이전트</td>
<td>a02f765</td>
<td>e132aa4</td>
<td>[CVE-2017-12447 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-12447)의 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>Ubuntu 16.04 커널</td>
<td>4.4.0-143-generic</td>
<td>4.4.0-145-generic</td>
<td>[CVE-2019-9213 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html)의 커널 업데이트로 작업자 노드 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>Ubuntu 18.04 커널</td>
<td>4.15.0-46-generic</td>
<td>4.15.0-47-generic</td>
<td>[CVE-2019-9213 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html)의 커널 업데이트로 작업자 노드 이미지가 업데이트되었습니다.</td>
</tr>
</tbody>
</table>

### 2019년 4월 1일에 릴리스된 작업자 노드 수정팩 1.13.4_1516에 대한 변경 로그
{: #1134_1516}

다음 표는 작업자 노드 수정팩 1.13.4_1516에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.13.4_1515 이후에 작성된 변경사항">
<caption>버전 1.13.4_1515 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>작업자 노드 리소스 활용</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>해당 컴포넌트의 리소스 소진을 방지하기 위해 kubelet 및 containerd에 대한 메모리 예약을 늘렸습니다. 자세한 정보는 [작업자 노드 리소스 예약](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node)을 참조하십시오.</td>
</tr>
</tbody>
</table>

### 2019년 3월 26일에 릴리스된 마스터 수정팩 1.13.4_1515에 대한 변경 로그
{: #1134_1515}

다음 표는 마스터 수정팩 1.13.4_1515에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.13.4_1513 이후에 작성된 변경사항">
<caption>버전 1.13.4_1513 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>클러스터 DNS 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>업데이트가 클러스터 DNS 제공자에서 CoreDNS로 전환되지 않도록 Kubernetes 버전 1.11에서 업데이트 프로세스를 수정했습니다. 업데이트 후 계속해서 [클러스터 DNS 제공자로 CoreDNS를 설정](/docs/containers?topic=containers-cluster_dns#set_coredns)할 수 있습니다.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 플러그인</td>
<td>345</td>
<td>346</td>
<td>[CVE-2019-9741 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741)의 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>키 관리 서비스 제공자</td>
<td>166</td>
<td>167</td>
<td>Kubernetes 시크릿을 관리하기 위해 중간 `context deadline exceeded` 및 `timeout` 오류를 수정합니다. 또한 기존 Kubernetes 시크릿이 암호되지 않은 상태로 유지될 수 있는 키 관리 서비스에 대한 업데이트를 수정합니다. 업데이트에는 [CVE-2019-9741 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741)에 대한 수정사항이 포함되어 있습니다.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} 제공자의 로드 밸런서 및 로드 밸런서 모니터</td>
<td>143</td>
<td>146</td>
<td>[CVE-2019-9741 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741)의 이미지가 업데이트되었습니다.</td>
</tr>
</tbody>
</table>

### 2019년 3월 20일에 릴리스된 1.13.4_1513에 대한 변경 로그
{: #1134_1513}

다음 표는 패치 1.13.4_1513에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.13.4_1510 이후에 작성된 변경사항">
<caption>버전 1.13.4_1510 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>클러스터 DNS 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>사용하지 않는 클러스터 DNS를 스케일링 다운해야 하는 경우 클러스터 마스터 조작(예: `refresh` 또는 `update`)이 실패할 수 있는 버그가 해결되었습니다.</td>
</tr>
<tr>
<td>클러스터 마스터 HA 프록시 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>클러스터 마스터에 대한 간헐적인 연결 실패를 보다 잘 처리하기 위한 구성이 업데이트되었습니다.</td>
</tr>
<tr>
<td>CoreDNS 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>1.12에서 클러스터 Kubernetes 버전을 업데이트한 후 [복수의 Corefile ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://coredns.io/2017/07/23/corefile-explained/)을 지원하기 위해 CoreDNS 구성이 업데이트되었습니다.</td>
</tr>
<tr>
<td>containerd</td>
<td>1.2.4</td>
<td>1.2.5</td>
<td>[containerd 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/containerd/containerd/releases/tag/v1.2.5)를 참조하십시오. 업데이트에는 [CVE-2019-5736 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5736)에 대한 향상된 수정사항이 포함되어 있습니다.</td>
</tr>
<tr>
<td>GPU 디바이스 플러그인 및 설치 프로그램</td>
<td>e32d51c</td>
<td>9ff3fda</td>
<td>[418.43 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.nvidia.com/object/unix.html)에 대한 GPU 드라이버가 업데이트되었습니다. 업데이트에는 [CVE-2019-9741 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9741.html)에 대한 수정사항이 포함되어 있습니다.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 플러그인</td>
<td>344</td>
<td>345</td>
<td>[개인 서비스 엔드포인트](/docs/containers?topic=containers-cs_network_cluster#set-up-private-se)에 대한 지원이 추가되었습니다.</td>
</tr>
<tr>
<td>Kernel</td>
<td>4.4.0-141</td>
<td>4.4.0-143</td>
<td>[CVE-2019-6133 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-6133.html)의 커널 업데이트로 작업자 노드 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>키 관리 서비스 제공자</td>
<td>136</td>
<td>166</td>
<td>[CVE-2018-16890 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16890), [CVE-2019-3822 ![ 외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3822) 및 [CVE-2019-3823 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3823)의 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>신뢰할 수 있는 컴퓨팅 에이전트</td>
<td>5f3d092</td>
<td>a02f765</td>
<td>[CVE-2018-10779 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10779), [CVE-2018-12900 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-12900), [CVE-2018-17000 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-17000), [CVE-2018-19210 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-19210), [CVE-2019-6128 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6128) 및 [CVE-2019-7663 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-7663)의 이미지가 업데이트되었습니다.</td>
</tr>
</tbody>
</table>

### 2019년 3월 4일에 릴리스된 1.13.4_1510에 대한 변경 로그
{: #1134_1510}

다음 표는 패치 1.13.4_1510에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.13.2_1509 이후에 작성된 변경사항">
<caption>버전 1.13.2_1509 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>클러스터 DNS 제공자</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>보다 많은 클러스터 서비스를 처리하기 위해서 Kubernetes DNS 및 CoreDNS 팟(Pod) 메모리 한계가 `170Mi`에서 `400Mi`로 증가되었습니다.</td>
</tr>
<tr>
<td>GPU 디바이스 플러그인 및 설치 프로그램</td>
<td>eb3a259</td>
<td>e32d51c</td>
<td>[CVE-2019-6454 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454)의 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.13.2-62</td>
<td>v1.13.4-86</td>
<td>Kubernetes 1.13.4 릴리스를 지원하도록 업데이트되었습니다. `externalTrafficPolicy`를 `local`로 설정하는 버전 1.0 로드 밸런서의 주기적인 연결 문제가 해결되었습니다. 최신 {{site.data.keyword.Bluemix_notm}} 문서 링크를 사용하기 위해 로드 밸런서 버전 1.0 및 2.0 이벤트가 업데이트되었습니다.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 플러그인</td>
<td>342</td>
<td>344</td>
<td>이미지에 대한 기본 운영 체제가 Fedora에서 Alpine으로 변경되었습니다. [CVE-2019-6486 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486)의 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>키 관리 서비스 제공자</td>
<td>122</td>
<td>136</td>
<td>클라이언트의 제한시간이 {{site.data.keyword.keymanagementservicefull_notm}}로 증가되었습니다. [CVE-2019-6486 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486)의 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.13.2</td>
<td>v1.13.4</td>
<td>[Kubernetes 릴리스 정보![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/kubernetes/releases/tag/v1.13.4)를 참조하십시오. 업데이트는 [CVE-2019-6486 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) 및 [CVE-2019-1002100 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1002100)을 해결합니다.</td>
</tr>
<tr>
<td>Kubernetes 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>`ExperimentalCriticalPodAnnotation=true`가 `--feature-gates` 옵션에 추가되었습니다. 이 설정은 팟(Pod)을 더 이상 사용되지 않는 `scheduler.alpha.kubernetes.io/critical-pod` 어노테이션에서 [Kubernetes 팟(Pod) 우선순위 지원](/docs/containers?topic=containers-pod_priority#pod_priority)으로 마이그레이션하는 데 도움이 됩니다.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} 제공자의 로드 밸런서 및 로드 밸런서 모니터</td>
<td>132</td>
<td>143</td>
<td>[CVE-2019-6486 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486)의 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>OpenVPN 클라이언트 및 서버</td>
<td>2.4.6-r3-IKS-13</td>
<td>2.4.6-r3-IKS-25</td>
<td>[CVE-2019-1559 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559)의 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>신뢰할 수 있는 컴퓨팅 에이전트</td>
<td>1ea5ad3</td>
<td>5f3d092</td>
<td>[CVE-2019-6454 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454)의 이미지가 업데이트되었습니다.</td>
</tr>
</tbody>
</table>

### 2019년 2월 27일에 릴리스된 작업자 노드 수정팩 1.13.2_1509에 대한 변경 로그
{: #1132_1509}

다음 표는 작업자 노드 수정팩 1.13.2_1509에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.13.2_1508 이후에 작성된 변경사항">
<caption>버전 1.13.2_1508 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-141</td>
<td>4.4.0-142</td>
<td>[CVE-2018-19407 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-142.168/changelog)의 커널 업데이트로 작업자 노드 이미지가 업데이트되었습니다.</td>
</tr>
</tbody>
</table>

### 2019년 2월 15일에 릴리스된 작업자 노드 수정팩 1.13.2_1508에 대한 변경 로그
{: #1132_1508}

다음 표는 작업자 노드 수정팩 1.13.2_1508에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.13.2_1507 이후에 작성된 변경사항">
<caption>버전 1.13.2_1507 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>클러스터 마스터 HA 프록시 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>팟(Pod) 구성 `spec.priorityClassName` 값을 `system-node-critical`로 변경하고 `spec.priority` 값을 `2000001000`으로 설정합니다.</td>
</tr>
<tr>
<td>containerd</td>
<td>1.2.2</td>
<td>1.2.4</td>
<td>[containerd 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/containerd/containerd/releases/tag/v1.2.4)를 참조하십시오. 업데이트는 [CVE-2019-5736 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5736)를 해결합니다.</td>
</tr>
<tr>
<td>Kubernetes `kubelet` 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>중요한 정적 팟(pod) 제거를 방지하기 위해 `ExperimentalCriticalPodAnnotation` 기능 게이트가 사용으로 설정되었습니다. 속도 제한 이벤트 작성을 방지하기 위해 `event-qps` 옵션을 `0`으로 설정합니다.</td>
</tr>
</tbody>
</table>

### 2019년 2월 5일에 릴리스된 1.13.2_1507에 대한 변경 로그
{: #1132_1507}

다음 표는 패치 1.13.2_1507에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.12.4_1535 이후에 작성된 변경사항">
<caption>버전 1.12.4_1535 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.3.1</td>
<td>v3.4.0</td>
<td>[Calico 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.projectcalico.org/v3.4/releases/#v340)를 참조하십시오.</td>
</tr>
<tr>
<td>클러스터 DNS 제공자</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>CoreDNS가 이제 새 클러스터의 기본 클러스터 DNS 제공자입니다. KubeDNS를 클러스터 DNS 제공자로 사용하는 기존 클러스터를 1.13으로 업데이트하면 KubeDNS가 계속 클러스터 DNS 제공자입니다. 그러나 [대신 CoreDNS를 사용](/docs/containers?topic=containers-cluster_dns#dns_set)하도록 선택할 수 있습니다.</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.5</td>
<td>1.2.2</td>
<td>[containerd 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/containerd/containerd/releases/tag/v1.2.2)를 참조하십시오.</td>
</tr>
<tr>
<td>CoreDNS</td>
<td>1.2.2</td>
<td>1.2.6</td>
<td>[CoreDNS 릴리스 정보![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/coredns/coredns/releases/tag/v1.2.6)를 참조하십시오. 또한 CoreDNS 구성이 [복수의 Corefile 지원 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")으로 ](https://coredns.io/2017/07/23/corefile-explained/)업데이트되었습니다.</td>
</tr>
<tr>
<td>etcd</td>
<td>v3.3.1</td>
<td>v3.3.11</td>
<td>[etcd 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/coreos/etcd/releases/v3.3.11)를 참조하십시오. 또한 etcd에 대해 지원되는 암호화 스위트는 이제 고강도의 암호화(128비트 이상)를 사용하는 서브세트로 제한됩니다.</td>
</tr>
<tr>
<td>GPU 디바이스 플러그인 및 설치 프로그램</td>
<td>13fdc0d</td>
<td>eb3a259</td>
<td>[CVE-2019-3462 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) 및 [CVE-2019-6486 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486)의 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.12.4-118</td>
<td>v1.13.2-62</td>
<td>Kubernetes 1.13.2 릴리스를 지원하도록 업데이트되었습니다. 또한 `calicoctl` 버전이 3.4.0으로 업데이트되었습니다. 작업자 노드 상태 변경에 대해 버전 2.0로드 밸런서에 대한 불필요한 구성 업데이트가 해결되었습니다.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 플러그인</td>
<td>338</td>
<td>342</td>
<td>파일 스토리지 플러그인이 다음과 같이 업데이트되었습니다.
<ul><li>[볼륨 토폴로지 인식 스케줄링](/docs/containers?topic=containers-file_storage#file-topology)을 사용한 동적 프로비저닝을 지원합니다.</li>
<li>스토리지가 이미 삭제된 경우, 지속적 볼륨 클레임(PVC) 삭제 오류가 무시됩니다.</li>
<li>실패 메시지 어노테이션을 실패한 PVC에 추가합니다.</li>
<li>스토리지 프로비저너 제어기의 리더 선택 및 재동기 기간 설정을 최적화하고 프로비저닝 제한시간을 30분에서 1시간으로 늘립니다.</li>
<li>프로비저닝을 시작하기 전에 사용자 권한을 확인합니다.</li>
<li>`CriticalAddonsOnly` 결함 허용을 `kube-system` 네임스페이스의 `ibm-file-plugin` 및 `ibm-storage-watcher` 배치에 추가합니다.</li></ul></td>
</tr>
<tr>
<td>키 관리 서비스 제공자</td>
<td>111</td>
<td>122</td>
<td>Kubernetes 시크릿을 {{site.data.keyword.keymanagementservicefull_notm}}에서 관리하는 경우 일시적인 장애를 방지하기 위해 재시도 로직이 추가되었습니다.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.4</td>
<td>v1.13.2</td>
<td>[Kubernetes 릴리스 정보![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/kubernetes/releases/tag/v1.13.2)를 참조하십시오.</td>
</tr>
<tr>
<td>Kubernetes 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>Kubernetes API 서버 감사 정책 구성이 `cluster-admin` 요청에 대한 메타데이터 로깅 및 워크로드 `create`, `update` 및 `patch` 요청의 요청 본문 로깅을 포함하도록 업데이트되었습니다.</td>
</tr>
<tr>
<td>Kubernetes DNS Autoscaler</td>
<td>1.2.0</td>
<td>1.3.0</td>
<td>[Kubernetes DNS Autoscaler 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes-incubator/cluster-proportional-autoscaler/releases/tag/1.3.0)를 참조하십시오.</td>
</tr>
<tr>
<td>OpenVPN 클라이언트</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>[CVE-2018-0734 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) 및 [CVE-2018-5407 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407)의 이미지가 업데이트되었습니다. `CriticalAddonsOnly` 결함 허용이 `kube-system` 네임스페이스의 `vpn` 배치에 추가되었습니다. 또한, 이제 configmap이 아닌 시크릿에서 팟(Pod) 구성을 가져옵니다.</td>
</tr>
<tr>
<td>OpenVPN 서버</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>[CVE-2018-0734 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) 및 [CVE-2018-5407 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407)의 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>systemd</td>
<td>230</td>
<td>229</td>
<td>[CVE-2018-16864 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16864)에 대한 보안 패치</td>
</tr>
</tbody>
</table>

<br />


## 버전 1.12 변경 로그
{: #112_changelog}

버전 1.12 변경 로그를 검토하십시오.
{: shortdesc}

### 2019년 6월 4일에 릴리스된 1.12.9_1555에 대한 변경 로그
{: #1129_1555}

다음 표는 패치 1.12.9_1555에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.12.8_1553 이후에 작성된 변경사항">
<caption>버전 1.12.8_1553 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>클러스터 DNS 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>클러스터 `create` 또는 `update` 오퍼레이션 이후 Kubernetes DNS 및 CoreDNS 팟(Pod)이 둘 다 실행 상태로 유지될 수 있는 버그가 해결되었습니다.</td>
</tr>
<tr>
<td>클러스터 마스터 HA 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>마스터 업데이트 중에 간헐적인 마스터 네트워크 연결 실패를 최소화할 수 있도록 구성이 업데이트되었습니다.</td>
</tr>
<tr>
<td>etcd</td>
<td>v3.3.11</td>
<td>v3.3.13</td>
<td>[etcd 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/coreos/etcd/releases/v3.3.13)를 참조하십시오.</td>
</tr>
<tr>
<td>GPU 디바이스 플러그인 및 설치 프로그램</td>
<td>55c1f66</td>
<td>32257d3</td>
<td>[CVE-2018-10844 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844), [CVE-2018-10845 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845), [CVE-2018-10846 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846), [CVE-2019-3829 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829), [CVE-2019-3836 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836), [CVE-2019-9893 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893), [CVE-2019-5435 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435) 및 [CVE-2019-5436 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436)의 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.12.8-210</td>
<td>v1.12.9-227</td>
<td>Kubernetes 1.12.9 릴리스를 지원하도록 업데이트되었습니다. </td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.8</td>
<td>v1.12.9</td>
<td>[Kubernetes 릴리스 정보![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.9)를 참조하십시오.</td>
</tr>
<tr>
<td>Kubernetes 메트릭 서버</td>
<td>v0.3.1</td>
<td>v0.3.3</td>
<td>[Kubernetes 메트릭 서버 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes-incubator/metrics-server/releases/tag/v0.3.3)를 참조하십시오.</td>
</tr>
<tr>
<td>신뢰할 수 있는 컴퓨팅 에이전트</td>
<td>13c7ef0</td>
<td>e8c6d72</td>
<td>[CVE-2018-10844 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844), [CVE-2018-10845 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845), [CVE-2018-10846 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846), [CVE-2019-3829 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829), [CVE-2019-3836 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836), [CVE-2019-9893 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893), [CVE-2019-5435 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435) 및 [CVE-2019-5436 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436)의 이미지가 업데이트되었습니다.</td>
</tr>
</tbody>
</table>

### 2019년 5월 20일에 릴리스된 작업자 노드 수정팩 1.12.8_1553에 대한 변경 로그
{: #1128_1533}

다음 표는 패치 1.12.8_1553에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.12.8_1553 이후에 작성된 변경사항">
<caption>버전 1.12.8_1553 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 16.04 커널</td>
<td>4.4.0-145-generic</td>
<td>4.4.0-148-generic</td>
<td>[CVE-2018-12126 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html), [CVE-2018-12127 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html) 및 [CVE-2018-12130 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html)의 커널 업데이트로 작업자 노드 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>Ubuntu 18.04 커널</td>
<td>4.15.0-47-generic</td>
<td>4.15.0-50-generic</td>
<td>[CVE-2018-12126 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html), [CVE-2018-12127 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html) 및 [CVE-2018-12130 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html)의 커널 업데이트로 작업자 노드 이미지가 업데이트되었습니다.</td>
</tr>
</tbody>
</table>

### 2019년 5월 13일에 릴리스된 1.12.8_1552에 대한 변경 로그
{: #1128_1552}

다음 표는 패치 1.12.8_1552에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.12.7_1550 이후에 작성된 변경사항">
<caption>버전 1.12.7_1550 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>클러스터 마스터 HA 프록시</td>
<td>1.9.6-alpine</td>
<td>1.9.7-alpine</td>
<td>[HAProxy 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.haproxy.org/download/1.9/src/CHANGELOG)를 참조하십시오. 업데이트는 [CVE-2019-6706 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6706)을 해결합니다.</td>
</tr>
<tr>
<td>GPU 디바이스 플러그인 및 설치 프로그램</td>
<td>9ff3fda</td>
<td>55c1f66</td>
<td>[CVE-2019-1543 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543)의 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.12.7-180</td>
<td>v1.12.8-210</td>
<td>Kubernetes 1.12.8 릴리스를 지원하도록 업데이트되었습니다. 또한 로드 밸런서 팟(Pod)에 사용 가능한 하나의 작업자 노드만 있는 버전 2.0 로드 밸런서에 대한 업데이트도 수정했습니다. </td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.7</td>
<td>v1.12.8</td>
<td>[Kubernetes 릴리스 정보![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.8)를 참조하십시오.</td>
</tr>
<tr>
<td>Kubernetes 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>Kubernetes API 서버 감사 정책 구성이 `/openapi/v2*` 읽기 전용 URL을 로깅하지 않도록 업데이트되었습니다. 또한 Kubernetes 제어기 관리자 구성에서는 서명된 `kubelet` 인증서의 유효성 검증 기간을 1년에서 3년으로 늘렸습니다. </td>
</tr>
<tr>
<td>OpenVPN 클라이언트 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>클러스터 DNS가 작동 중지될 때 팟(Pod)이 실패하지 않도록 `kube-system` 네임스페이스의 OpenVPN 클라이언트 `vpn-*` 팟(Pod)이 이제 `dnsPolicy`를 `Default`로 설정합니다.</td>
</tr>
<tr>
<td>신뢰할 수 있는 컴퓨팅 에이전트</td>
<td>e132aa4</td>
<td>13c7ef0</td>
<td>[CVE-2016-7076 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-7076), [CVE-2017-1000368 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1000368) 및 [CVE-2019-11068 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-11068)의 이미지가 업데이트되었습니다.</td>
</tr>
</tbody>
</table>

### 2019년 4월 29일에 릴리스된 작업자 노드 수정팩 1.12.7_1550에 대한 변경 로그
{: #1127_1550}

다음 표는 작업자 노드 수정팩 1.12.7_1550에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.12.7_1549 이후에 작성된 변경사항">
<caption>버전 1.12.7_1549 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 패키지</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>설치된 Ubuntu 패키지로 업데이트되었습니다.</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.6</td>
<td>1.1.7</td>
<td>[containerd 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/containerd/containerd/releases/tag/v1.1.7)를 참조하십시오.</td>
</tr>
</tbody>
</table>


### 2019년 4월 15일에 릴리스된 작업자 노드 수정팩 1.12.7_1549에 대한 변경 로그
{: #1127_1549}

다음 표는 작업자 노드 수정팩 1.12.7_1549에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.12.7_1548 이후에 작성된 변경사항">
<caption>버전 1.12.7_1548 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 패키지</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>[CVE-2019-3842 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-3842.html)에 대한 `systemd`를 포함하여 설치된 Ubuntu 패키지를 업데이트합니다.</td>
</tr>
</tbody>
</table>

### 2019년 4월 8일에 릴리스된 1.12.7_1548에 대한 변경 로그
{: #1127_1548}

다음 표는 패치 1.12.7_1548에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.12.6_1547 이후에 작성된 변경사항">
<caption>버전 1.12.6_1547 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.3.1</td>
<td>v3.3.6</td>
<td>[Calico 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.projectcalico.org/v3.3/releases/#v336)를 참조하십시오. 업데이트는 [CVE-2019-9946 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9946)을 해결합니다.</td>
</tr>
<tr>
<td>클러스터 마스터 HA 프록시</td>
<td>1.8.12-alpine</td>
<td>1.9.6-alpine</td>
<td>[HAProxy 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.haproxy.org/download/1.9/src/CHANGELOG)를 참조하십시오. 업데이트는 [CVE-2018-0732 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732), [CVE-2018-0734 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734), [CVE-2018-0737 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737), [CVE-2018-5407 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407), [CVE-2019-1543 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543) 및 [CVE-2019-1559 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559)를 해결합니다.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.12.6-157</td>
<td>v1.12.7-180</td>
<td>Kubernetes 1.12.7 및 Calico 3.3.6 릴리스를 지원하도록 업데이트되었습니다.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.6</td>
<td>v1.12.7</td>
<td>[Kubernetes 릴리스 정보![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.7)를 참조하십시오.</td>
</tr>
<tr>
<td>신뢰할 수 있는 컴퓨팅 에이전트</td>
<td>a02f765</td>
<td>e132aa4</td>
<td>[CVE-2017-12447 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-12447)의 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>Ubuntu 16.04 커널</td>
<td>4.4.0-143-generic</td>
<td>4.4.0-145-generic</td>
<td>[CVE-2019-9213 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html)의 커널 업데이트로 작업자 노드 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>Ubuntu 18.04 커널</td>
<td>4.15.0-46-generic</td>
<td>4.15.0-47-generic</td>
<td>[CVE-2019-9213 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html)의 커널 업데이트로 작업자 노드 이미지가 업데이트되었습니다.</td>
</tr>
</tbody>
</table>

### 2019년 4월 1일에 릴리스된 작업자 노드 수정팩 1.12.6_154에 대한 변경 로그
{: #1126_1547}

다음 표는 작업자 노드 수정팩 1.12.6_1547에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.12.6_1546 이후에 작성된 변경사항">
<caption>버전 1.12.6_1546 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>작업자 노드 리소스 활용</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>해당 컴포넌트의 리소스 소진을 방지하기 위해 kubelet 및 containerd에 대한 메모리 예약을 늘렸습니다. 자세한 정보는 [작업자 노드 리소스 예약](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node)을 참조하십시오.</td>
</tr>
</tbody>
</table>


### 2019년 3월 26일에 릴리스된 마스터 수정팩 1.12.6_1546에 대한 변경 로그
{: #1126_1546}

다음 표는 마스터 수정팩 1.12.6_1546에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.12.6_1544 이후에 작성된 변경사항">
<caption>버전 1.12.6_1544 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 플러그인</td>
<td>345</td>
<td>346</td>
<td>[CVE-2019-9741 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741)의 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>키 관리 서비스 제공자</td>
<td>166</td>
<td>167</td>
<td>Kubernetes 시크릿을 관리하기 위해 중간 `context deadline exceeded` 및 `timeout` 오류를 수정합니다. 또한 기존 Kubernetes 시크릿이 암호되지 않은 상태로 유지될 수 있는 키 관리 서비스에 대한 업데이트를 수정합니다. 업데이트에는 [CVE-2019-9741 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741)에 대한 수정사항이 포함되어 있습니다.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} 제공자의 로드 밸런서 및 로드 밸런서 모니터</td>
<td>143</td>
<td>146</td>
<td>[CVE-2019-9741 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741)의 이미지가 업데이트되었습니다.</td>
</tr>
</tbody>
</table>

### 2019년 3월 20일에 릴리스된 1.12.6_1544에 대한 변경 로그
{: #1126_1544}

다음 표는 패치 1.12.6_1544에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전  1.12.6_1541 이후에 작성된 변경사항">
<caption>버전 1.12.6_1541 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>클러스터 DNS 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>사용하지 않는 클러스터 DNS를 스케일링 다운해야 하는 경우 클러스터 마스터 조작(예: `refresh` 또는 `update`)이 실패할 수 있는 버그가 해결되었습니다.</td>
</tr>
<tr>
<td>클러스터 마스터 HA 프록시 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>클러스터 마스터에 대한 간헐적인 연결 실패를 보다 잘 처리하기 위한 구성이 업데이트되었습니다.</td>
</tr>
<tr>
<td>CoreDNS 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>[복수의 Corefile ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://coredns.io/2017/07/23/corefile-explained/)을 지원하도록 CoreDNS 구성이 업데이트되었습니다.</td>
</tr>
<tr>
<td>GPU 디바이스 플러그인 및 설치 프로그램</td>
<td>e32d51c</td>
<td>9ff3fda</td>
<td>[418.43 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.nvidia.com/object/unix.html)에 대한 GPU 드라이버가 업데이트되었습니다. 업데이트에는 [CVE-2019-9741 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9741.html)에 대한 수정사항이 포함되어 있습니다.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 플러그인</td>
<td>344</td>
<td>345</td>
<td>[개인 서비스 엔드포인트](/docs/containers?topic=containers-cs_network_cluster#set-up-private-se)에 대한 지원이 추가되었습니다.</td>
</tr>
<tr>
<td>Kernel</td>
<td>4.4.0-141</td>
<td>4.4.0-143</td>
<td>[CVE-2019-6133 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-6133.html)의 커널 업데이트로 작업자 노드 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>키 관리 서비스 제공자</td>
<td>136</td>
<td>166</td>
<td>[CVE-2018-16890 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16890), [CVE-2019-3822 ![ 외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3822) 및 [CVE-2019-3823 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3823)의 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>신뢰할 수 있는 컴퓨팅 에이전트</td>
<td>5f3d092</td>
<td>a02f765</td>
<td>[CVE-2018-10779 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10779), [CVE-2018-12900 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-12900), [CVE-2018-17000 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-17000), [CVE-2018-19210 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-19210), [CVE-2019-6128 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6128) 및 [CVE-2019-7663 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-7663)의 이미지가 업데이트되었습니다.</td>
</tr>
</tbody>
</table>

### 2019년 3월 4일에 릴리스된 1.12.6_1541에 대한 변경 로그
{: #1126_1541}

다음 표는 패치 1.12.6_1541에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.12.5_1540 이후에 작성된 변경사항">
<caption>버전 1.12.5_1540 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>클러스터 DNS 제공자</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>보다 많은 클러스터 서비스를 처리하기 위해서 Kubernetes DNS 및 CoreDNS 팟(Pod) 메모리 한계가 `170Mi`에서 `400Mi`로 증가되었습니다.</td>
</tr>
<tr>
<td>GPU 디바이스 플러그인 및 설치 프로그램</td>
<td>eb3a259</td>
<td>e32d51c</td>
<td>[CVE-2019-6454 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454)의 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.12.5-137</td>
<td>v1.12.6-157</td>
<td>Kubernetes 1.12.6 릴리스를 지원하도록 업데이트되었습니다. `externalTrafficPolicy`를 `local`로 설정하는 버전 1.0 로드 밸런서의 주기적인 연결 문제가 해결되었습니다. 최신 {{site.data.keyword.Bluemix_notm}} 문서 링크를 사용하기 위해 로드 밸런서 버전 1.0 및 2.0 이벤트가 업데이트되었습니다.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 플러그인</td>
<td>342</td>
<td>344</td>
<td>이미지에 대한 기본 운영 체제가 Fedora에서 Alpine으로 변경되었습니다. [CVE-2019-6486 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486)의 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>키 관리 서비스 제공자</td>
<td>122</td>
<td>136</td>
<td>클라이언트의 제한시간이 {{site.data.keyword.keymanagementservicefull_notm}}로 증가되었습니다. [CVE-2019-6486 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486)의 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.5</td>
<td>v1.12.6</td>
<td>[Kubernetes 릴리스 정보![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.6)를 참조하십시오. 업데이트는 [CVE-2019-6486 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) 및 [CVE-2019-1002100 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1002100)을 해결합니다.</td>
</tr>
<tr>
<td>Kubernetes 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>`ExperimentalCriticalPodAnnotation=true`가 `--feature-gates` 옵션에 추가되었습니다. 이 설정은 팟(Pod)을 더 이상 사용되지 않는 `scheduler.alpha.kubernetes.io/critical-pod` 어노테이션에서 [Kubernetes 팟(Pod) 우선순위 지원](/docs/containers?topic=containers-pod_priority#pod_priority)으로 마이그레이션하는 데 도움이 됩니다.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} 제공자의 로드 밸런서 및 로드 밸런서 모니터</td>
<td>132</td>
<td>143</td>
<td>[CVE-2019-6486 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486)의 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>OpenVPN 클라이언트 및 서버</td>
<td>2.4.6-r3-IKS-13</td>
<td>2.4.6-r3-IKS-25</td>
<td>[CVE-2019-1559 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559)의 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>신뢰할 수 있는 컴퓨팅 에이전트</td>
<td>1ea5ad3</td>
<td>5f3d092</td>
<td>[CVE-2019-6454 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454)의 이미지가 업데이트되었습니다.</td>
</tr>
</tbody>
</table>

### 2019년 2월 27일에 릴리스된 작업자 노드 수정팩 1.12.5_1540에 대한 변경 로그
{: #1125_1540}

다음 표는 작업자 노드 수정팩 1.12.5_1540에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.12.5_1538 이후에 작성된 변경사항">
<caption>버전 1.12.5_1538 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-141</td>
<td>4.4.0-142</td>
<td>[CVE-2018-19407 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-142.168/changelog)의 커널 업데이트로 작업자 노드 이미지가 업데이트되었습니다.</td>
</tr>
</tbody>
</table>

### 2019년 2월 15일에 릴리스된 작업자 노드 수정팩 1.12.5_1538에 대한 변경 로그
{: #1125_1538}

다음 표는 작업자 노드 수정팩 1.12.5_1538에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.12.5_1537 이후에 작성된 변경사항">
<caption>버전 1.12.5_1537 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>클러스터 마스터 HA 프록시 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>팟(Pod) 구성 `spec.priorityClassName` 값을 `system-node-critical`로 변경하고 `spec.priority` 값을 `2000001000`으로 설정합니다.</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.5</td>
<td>1.1.6</td>
<td>[containerd 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/containerd/containerd/releases/tag/v1.1.6)를 참조하십시오. 업데이트는 [CVE-2019-5736 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5736)를 해결합니다.</td>
</tr>
<tr>
<td>Kubernetes `kubelet` 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>중요한 정적 팟(pod) 제거를 방지하기 위해 `ExperimentalCriticalPodAnnotation` 기능 게이트가 사용으로 설정되었습니다.</td>
</tr>
</tbody>
</table>

### 2019년 2월 5일에 릴리스된 1.12.5_1537에 대한 변경 로그
{: #1125_1537}

다음 표는 패치 1.12.5_1537에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.12.4_1535 이후에 작성된 변경사항">
<caption>버전 1.12.4_1535 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>etcd</td>
<td>v3.3.1</td>
<td>v3.3.11</td>
<td>[etcd 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/coreos/etcd/releases/v3.3.11)를 참조하십시오. 또한 etcd에 대해 지원되는 암호화 스위트는 이제 고강도의 암호화(128비트 이상)를 사용하는 서브세트로 제한됩니다.</td>
</tr>
<tr>
<td>GPU 디바이스 플러그인 및 설치 프로그램</td>
<td>13fdc0d</td>
<td>eb3a259</td>
<td>[CVE-2019-3462 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) 및 [CVE-2019-6486 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486)의 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.12.4-118</td>
<td>v1.12.5-137</td>
<td>Kubernetes 1.12.5 릴리스를 지원하도록 업데이트되었습니다. 또한 `calicoctl` 버전이 3.3.1로 업데이트되었습니다. 작업자 노드 상태 변경에 대해 버전 2.0로드 밸런서에 대한 불필요한 구성 업데이트가 해결되었습니다.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 플러그인</td>
<td>338</td>
<td>342</td>
<td>파일 스토리지 플러그인이 다음과 같이 업데이트되었습니다.
<ul><li>[볼륨 토폴로지 인식 스케줄링](/docs/containers?topic=containers-file_storage#file-topology)을 사용한 동적 프로비저닝을 지원합니다.</li>
<li>스토리지가 이미 삭제된 경우, 지속적 볼륨 클레임(PVC) 삭제 오류가 무시됩니다.</li>
<li>실패 메시지 어노테이션을 실패한 PVC에 추가합니다.</li>
<li>스토리지 프로비저너 제어기의 리더 선택 및 재동기 기간 설정을 최적화하고 프로비저닝 제한시간을 30분에서 1시간으로 늘립니다.</li>
<li>프로비저닝을 시작하기 전에 사용자 권한을 확인합니다.</li></ul></td>
</tr>
<tr>
<td>키 관리 서비스 제공자</td>
<td>111</td>
<td>122</td>
<td>Kubernetes 시크릿을 {{site.data.keyword.keymanagementservicefull_notm}}에서 관리하는 경우 일시적인 장애를 방지하기 위해 재시도 로직이 추가되었습니다.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.4</td>
<td>v1.12.5</td>
<td>[Kubernetes 릴리스 정보![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.5)를 참조하십시오.</td>
</tr>
<tr>
<td>Kubernetes 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>Kubernetes API 서버 감사 정책 구성이 `cluster-admin` 요청에 대한 메타데이터 로깅 및 워크로드 `create`, `update` 및 `patch` 요청의 요청 본문 로깅을 포함하도록 업데이트되었습니다.</td>
</tr>
<tr>
<td>OpenVPN 클라이언트</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>[CVE-2018-0734 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) 및 [CVE-2018-5407 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407)의 이미지가 업데이트되었습니다. 또한, 이제 configmap이 아닌 시크릿에서 팟(Pod) 구성을 가져옵니다.</td>
</tr>
<tr>
<td>OpenVPN 서버</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>[CVE-2018-0734 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) 및 [CVE-2018-5407 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407)의 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>systemd</td>
<td>230</td>
<td>229</td>
<td>[CVE-2018-16864 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16864)에 대한 보안 패치</td>
</tr>
</tbody>
</table>

### 2019년 1월 28일에 릴리스된 작업자 노드 수정팩 1.12.4_1535에 대한 변경 로그
{: #1124_1535}

다음 표는 작업자 노드 수정팩 1.12.4_1535에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.12.4_1534 이후에 작성된 변경사항">
<caption>버전 1.12.4_1534 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 패키지</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>[CVE-2019-3462 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) 및 [USN-3863-1 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://usn.ubuntu.com/3863-1)에 대한 `apt`를 포함하여 설치된 Ubuntu 패키지를 업데이트합니다.</td>
</tr>
</tbody>
</table>


### 2019년 1월 21일에 릴리스된 1.12.4_1534에 대한 변경 로그
{: #1124_1534}

다음 표는 패치 1.12.3_1534에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.12.3_1533 이후에 작성된 변경사항">
<caption>버전 1.12.3_1533 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.12.3-91</td>
<td>v1.12.4-118</td>
<td>Kubernetes 1.12.4 릴리스를 지원하도록 업데이트되었습니다.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.3</td>
<td>v1.12.4</td>
<td>[Kubernetes 릴리스 정보![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.4)를 참조하십시오.</td>
</tr>
<tr>
<td>Kubernetes 추가 기능 resizer</td>
<td>1.8.1</td>
<td>1.8.4</td>
<td>[Kubernetes 추가 기능 resizer 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/autoscaler/releases/tag/addon-resizer-1.8.4)를 참조하십시오.</td>
</tr>
<tr>
<td>Kubernetes 대시보드</td>
<td>v1.8.3</td>
<td>v1.10.1</td>
<td>[Kubernetes 대시보드 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/dashboard/releases/tag/v1.10.1)를 참조하십시오. 업데이트는 [CVE-2018-18264 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-18264)를 해결합니다.</td>
</tr>
<tr>
<td>GPU 설치 프로그램</td>
<td>390.12</td>
<td>410.79</td>
<td>설치된 GPU 드라이버가 410.79로 업데이트되었습니다.</td>
</tr>
</tbody>
</table>

### 2019년 1월 7일에 릴리스된 작업자 노드 수정팩 1.12.3_1533에 대한 변경 로그
{: #1123_1533}

다음 표는 작업자 노드 수정팩 1.12.3_1533에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.12.3_1532 이후에 작성된 변경사항">
<caption>버전 1.12.3_1532 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-139</td>
<td>4.4.0-141</td>
<td>[CVE-2017-5753, CVE-2018-18690 ![외부 링크 아이콘n](../icons/launch-glyph.svg "외부 링크 아이콘")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-141.167/changelog)의 커널 업데이트로 작업자 노드 이미지가 업데이트되었습니다.</td>
</tr>
</tbody>
</table>

### 2018년 12월 17일에 릴리스된 작업자 노드 수정팩 1.12.3_1532에 대한 변경 로그
{: #1123_1532}

다음 표는 작업자 노드 수정팩 1.12.2_1532에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.12.3_1531 이후에 작성된 변경사항">
<caption>버전 1.12.3_1531 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 패키지</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>설치된 Ubuntu 패키지로 업데이트되었습니다.</td>
</tr>
</tbody>
</table>


### 2018년 12월 5일에 릴리스된 1.12.3_1531에 대한 변경 로그
{: #1123_1531}

다음 표는 패치 1.12.3_1531에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.12.2_1530 이후에 작성된 변경사항">
<caption>버전 1.12.2_1530 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.12.2-68</td>
<td>v1.12.3-91</td>
<td>Kubernetes 1.12.3 릴리스를 지원하도록 업데이트되었습니다.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.2</td>
<td>v1.12.3</td>
<td>[Kubernetes 릴리스 정보![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.3)를 참조하십시오. 업데이트는 [CVE-2018-1002105 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/kubernetes/issues/71411)를 해결합니다.</td>
</tr>
</tbody>
</table>

### 2018년 12월 4일에 릴리스된 작업자 노드 수정팩 1.12.2_1530에 대한 변경 로그
{: #1122_1530}

다음 표는 작업자 노드 수정팩 1.12.2_1530에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.12.2_1529 이후에 작성된 변경사항">
<caption>버전 1.12.2_1529 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>작업자 노드 리소스 활용</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>해당 컴포넌트의 리소스 소진을 방지하기 위해 kubelet 및 containerd에 대한 전용 cgroup이 추가되었습니다. 자세한 정보는 [작업자 노드 리소스 예약](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node)을 참조하십시오.</td>
</tr>
</tbody>
</table>



### 2018년 11월 27일에 릴리스된 1.12.2_1529에 대한 변경 로그
{: #1122_1529}

다음 표는 패치 1.12.2_1529에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.12.2_1528 이후에 작성된 변경사항">
<caption>버전 1.12.2_1528 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.2.1</td>
<td>v3.3.1</td>
<td>[Calico 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.projectcalico.org/v3.3/releases/#v331)를 참조하십시오. 업데이트는 [Tigera Technical Advisory TTA-2018-001 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.projectcalico.org/security-bulletins/)을 해결합니다.</td>
</tr>
<tr>
<td>클러스터 DNS 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>클러스터 작성 또는 업데이트 오퍼레이션 이후 Kubernetes DNS 및 CoreDNS 팟(Pod)이 둘 다 실행되는 버그가 해결되었습니다.</td>
</tr>
<tr>
<td>containerd</td>
<td>v1.2.0</td>
<td>v1.1.5</td>
<td>[containerd 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/containerd/containerd/releases/tag/v1.1.5)를 참조하십시오. [팟(Pod)의 종료를 중지 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/containerd/containerd/issues/2744)시킬 수 있는 교착 상태를 해결하기 위해 containerd가 업데이트되었습니다.</td>
</tr>
<tr>
<td>OpenVPN 클라이언트 및 서버</td>
<td>2.4.4-r1-6</td>
<td>2.4.6-r3-IKS-8</td>
<td>[CVE-2018-0732 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732) 및 [CVE-2018-0737 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737)의 이미지가 업데이트되었습니다.</td>
</tr>
</tbody>
</table>

### 2018년 11월 19일에 릴리스된 작업자 노드 수정팩 1.12.2_1528에 대한 변경 로그
{: #1122_1528}

다음 표는 작업자 노드 수정팩 1.12.2_1528에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.12.2_1527 이후에 작성된 변경사항">
<caption>버전 1.12.2_1527 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-137</td>
<td>4.4.0-139</td>
<td>[CVE-2018-7755 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-139.165/changelog)의 커널 업데이트로 작업자 노드 이미지가 업데이트되었습니다.</td>
</tr>
</tbody>
</table>


### 2018년 11월 7일에 릴리스된 1.12.2_1527에 대한 변경 로그
{: #1122_1527}

다음 표는 패치 1.12.2_1527에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.11.3_1533 이후에 작성된 변경사항">
<caption>버전 1.11.3_1533 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>`kube-system` 네임스페이스의 Calico `calico-*` 팟(Pod)이 이제 모든 컨테이너에 대해 CPU 및 메모리 리소스 요청을 설정합니다.</td>
</tr>
<tr>
<td>클러스터 DNS 제공자</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>Kubernetes DNS(KubeDNS)는 계속해서 기본 클러스터 DNS 제공자입니다. 그러나 이제 [클러스터 DNS 제공자를 CoreDNS로 변경](/docs/containers?topic=containers-cluster_dns#dns_set)하는 옵션이 있습니다.</td>
</tr>
<tr>
<td>클러스터 메트릭 제공자</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>Kubernetes 메트릭 서버는 Kubernetes Heapster(Kubernetes 버전 1.8부터 더 이상 사용되지 않음)를 클러스터 메트릭 제공자로 대체합니다. 조치 항목에 대해서는 [the `metrics-server` 준비 조치](/docs/containers?topic=containers-cs_versions#metrics-server)를 참조하십시오.</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.4</td>
<td>1.2.0</td>
<td>[containerd 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/containerd/containerd/releases/tag/v1.2.0)를 참조하십시오.</td>
</tr>
<tr>
<td>CoreDNS</td>
<td>해당사항 없음</td>
<td>1.2.2</td>
<td>[CoreDNS 릴리스 정보![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/coredns/coredns/releases/tag/v1.2.2)를 참조하십시오.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.3</td>
<td>v1.12.2</td>
<td>[Kubernetes 릴리스 정보![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.2)를 참조하십시오.</td>
</tr>
<tr>
<td>Kubernetes 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>3개의 새 IBM 팟(Pod) 보안 정책 및 이와 연관된 클러스터 역할이 추가되었습니다. 자세한 정보는 [IBM 클러스터 관리를 위한 기본 리소스 이해](/docs/containers?topic=containers-psp#ibm_psp)를 참조하십시오.</td>
</tr>
<tr>
<td>Kubernetes 대시보드</td>
<td>v1.8.3</td>
<td>v1.10.0</td>
<td>[Kubernetes 대시보드 릴리스 정보![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/dashboard/releases/tag/v1.10.0)를 참조하십시오.<br><br>
`kubectl proxy`를 통해 대시보드에 액세스할 경우 로그인 페이지의 **건너뛰기** 단추가 제거됩니다. 대신 [**토큰**을 사용하여 로그인](/docs/containers?topic=containers-app#cli_dashboard)하십시오. 또한 `kubectl -n kube-system scale deploy kubernetes-dashboard --replicas=3`을 실행하여 Kubernetes 대시보드 팟(Pod)의 수를 늘릴 수 있습니다.</td>
</tr>
<tr>
<td>Kubernetes DNS</td>
<td>1.14.10</td>
<td>1.14.13</td>
<td>[Kubernetes DNS 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/dns/releases/tag/1.14.13)를 참조하십시오.</td>
</tr>
<tr>
<td>Kubernetes 메트릭 서버</td>
<td>해당사항 없음</td>
<td>v0.3.1</td>
<td>[Kubernetes 메트릭 서버 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes-incubator/metrics-server/releases/tag/v0.3.1)를 참조하십시오.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.11.3-118</td>
<td>v1.12.2-68</td>
<td>Kubernetes 1.12 릴리스를 지원하도록 업데이트되었습니다. 추가 변경사항에는 다음이 포함됩니다.
<ul><li>로드 밸런서 팟(Pod)(`ibm-system` 네임스페이스의 `ibm-cloud-provider-ip-*`)이 이제 CPU 및 메모리 리소스 요청을 설정합니다.</li>
<li>로드 밸런서 서비스가 배치되는 VLAN을 지정하기 위해 `service.kubernetes.io/ibm-load-balancer-cloud-provider-vlan` 어노테이션이 추가되었습니다. 클러스터에서 사용 가능한 VLAN을 보려면 `ibmcloud ks vlans --zone <zone>`을 실행하십시오.</li>
<li>새 [로드 밸런서 2.0](/docs/containers?topic=containers-loadbalancer#planning_ipvs)이 베타로서 사용 가능합니다.</li></ul></td>
</tr>
<tr>
<td>OpenVPN 클라이언트 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>`kube-system` 네임스페이스의 OpenVPN 클라이언트 `vpn-* pod`이 이제 CPU 및 메모리 리소스 요청을 설정합니다.</td>
</tr>
</tbody>
</table>

## 더 이상 사용되지 않음: 버전 1.11 변경 로그
{: #111_changelog}

버전 1.11 변경 로그를 검토하십시오.
{: shortdesc}

Kubernetes 버전 1.11은 더 이상 사용되지 않으며 2019년 6월 27일(임시)부터 지원되지 않습니다. 각 Kubernetes 버전 업데이트의 [잠재적인 영향을 검토](/docs/containers?topic=containers-cs_versions#cs_versions)한 후에 버전 1.12 이상으로 즉시 [클러스터를 업데이트](/docs/containers?topic=containers-update#update)하십시오.
{: deprecated}

### 2019년 6월 4일에 릴리스된 1.11.10_1561에 대한 변경 로그
{: #11110_1561}

다음 표는 패치 1.11.10_1561에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.11.10_1559 이후에 작성된 변경사항">
<caption>버전 1.11.10_1559 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>클러스터 마스터 HA 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>마스터 업데이트 중에 간헐적인 마스터 네트워크 연결 실패를 최소화할 수 있도록 구성이 업데이트되었습니다.</td>
</tr>
<tr>
<td>etcd</td>
<td>v3.3.11</td>
<td>v3.3.13</td>
<td>[etcd 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/coreos/etcd/releases/v3.3.13)를 참조하십시오.</td>
</tr>
<tr>
<td>GPU 디바이스 플러그인 및 설치 프로그램</td>
<td>55c1f66</td>
<td>32257d3</td>
<td>[CVE-2018-10844 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844), [CVE-2018-10845 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845), [CVE-2018-10846 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846), [CVE-2019-3829 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829), [CVE-2019-3836 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836), [CVE-2019-9893 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893), [CVE-2019-5435 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435) 및 [CVE-2019-5436 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436)의 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>신뢰할 수 있는 컴퓨팅 에이전트</td>
<td>13c7ef0</td>
<td>e8c6d72</td>
<td>[CVE-2018-10844 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844), [CVE-2018-10845 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845), [CVE-2018-10846 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846), [CVE-2019-3829 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829), [CVE-2019-3836 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836), [CVE-2019-9893 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893), [CVE-2019-5435 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435) 및 [CVE-2019-5436 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436)의 이미지가 업데이트되었습니다.</td>
</tr>
</tbody>
</table>

### 2019년 5월 20일에 릴리스된 작업자 노드 수정팩 1.11.10_1559에 대한 변경 로그
{: #11110_1559}

다음 표는 패치 팩 1.11.10_1559에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.11.10_1558 이후에 작성된 변경사항">
<caption>버전 1.11.10_1558 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 16.04 커널</td>
<td>4.4.0-145-generic</td>
<td>4.4.0-148-generic</td>
<td>[CVE-2018-12126 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html), [CVE-2018-12127 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html) 및 [CVE-2018-12130 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html)의 커널 업데이트로 작업자 노드 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>Ubuntu 18.04 커널</td>
<td>4.15.0-47-generic</td>
<td>4.15.0-50-generic</td>
<td>[CVE-2018-12126 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html), [CVE-2018-12127 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html) 및 [CVE-2018-12130 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html)의 커널 업데이트로 작업자 노드 이미지가 업데이트되었습니다.</td>
</tr>
</tbody>
</table>

### 2019년 5월 13일에 릴리스된 1.11.10_1558에 대한 변경 로그
{: #11110_1558}

다음 표는 패치 1.11.10_1558에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.11.9_1556 이후에 작성된 변경사항">
<caption>버전 1.11.9_1556 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>클러스터 마스터 HA 프록시</td>
<td>1.9.6-alpine</td>
<td>1.9.7-alpine</td>
<td>[HAProxy 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.haproxy.org/download/1.9/src/CHANGELOG)를 참조하십시오. 업데이트는 [CVE-2019-6706 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6706)을 해결합니다.</td>
</tr>
<tr>
<td>GPU 디바이스 플러그인 및 설치 프로그램</td>
<td>9ff3fda</td>
<td>55c1f66</td>
<td>[CVE-2019-1543 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543)의 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.11.9-241</td>
<td>v1.11.10-270</td>
<td>Kubernetes 1.11.10 릴리스를 지원하도록 업데이트되었습니다. </td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.9</td>
<td>v1.11.10</td>
<td>[Kubernetes 릴리스 정보![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.10)를 참조하십시오.</td>
</tr>
<tr>
<td>Kubernetes 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>Kubernetes API 서버 감사 정책 구성이 `/openapi/v2*` 읽기 전용 URL을 로깅하지 않도록 업데이트되었습니다. 또한 Kubernetes 제어기 관리자 구성에서는 서명된 `kubelet` 인증서의 유효성 검증 기간을 1년에서 3년으로 늘렸습니다. </td>
</tr>
<tr>
<td>OpenVPN 클라이언트 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>클러스터 DNS가 작동 중지될 때 팟(Pod)이 실패하지 않도록 `kube-system` 네임스페이스의 OpenVPN 클라이언트 `vpn-*` 팟(Pod)이 이제 `dnsPolicy`를 `Default`로 설정합니다.</td>
</tr>
<tr>
<td>신뢰할 수 있는 컴퓨팅 에이전트</td>
<td>e132aa4</td>
<td>13c7ef0</td>
<td>[CVE-2016-7076 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-7076), [CVE-2017-1000368 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1000368) 및 [CVE-2019-11068 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-11068)의 이미지가 업데이트되었습니다.</td>
</tr>
</tbody>
</table>

### 2019년 4월 29일에 릴리스된 작업자 노드 수정팩 1.11.9_1556에 대한 변경 로그
{: #1119_1556}

다음 표는 작업자 노드 수정팩 1.11.9_1556에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.11.9_1555 이후에 작성된 변경사항">
<caption>버전 1.11.9_1555 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 패키지</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>설치된 Ubuntu 패키지로 업데이트되었습니다.</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.6</td>
<td>1.1.7</td>
<td>[containerd 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/containerd/containerd/releases/tag/v1.1.7)를 참조하십시오.</td>
</tr>
</tbody>
</table>


### 2019년 4월 15일에 릴리스된 작업자 노드 수정팩 1.11.9_1555에 대한 변경 로그
{: #1119_1555}

다음 표는 작업자 노드 수정팩 1.11.9_1555에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.11.9_1554 이후에 작성된 변경사항">
<caption>1.11.9_1554 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 패키지</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>[CVE-2019-3842 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-3842.html)에 대한 `systemd`를 포함하여 설치된 Ubuntu 패키지를 업데이트합니다.</td>
</tr>
</tbody>
</table>

### 2019년 4월 8일에 릴리스된 1.11.9_1554에 대한 변경 로그
{: #1119_1554}

다음 표는 패치 1.11.9_1554에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.11.8_1553 이후에 작성된 변경사항">
<caption>버전 1.11.8_1553 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.3.1</td>
<td>v3.3.6</td>
<td>[Calico 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.projectcalico.org/v3.3/releases/#v336)를 참조하십시오. 업데이트는 [CVE-2019-9946 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9946)을 해결합니다.</td>
</tr>
<tr>
<td>클러스터 마스터 HA 프록시</td>
<td>1.8.12-alpine</td>
<td>1.9.6-alpine</td>
<td>[HAProxy 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.haproxy.org/download/1.9/src/CHANGELOG)를 참조하십시오. 업데이트는 [CVE-2018-0732 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732), [CVE-2018-0734 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734), [CVE-2018-0737 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737), [CVE-2018-5407 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407), [CVE-2019-1543 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543) 및 [CVE-2019-1559 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559)를 해결합니다.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.11.8-219</td>
<td>v1.11.9-241</td>
<td>Kubernetes 1.11.9 및 Calico 3.3.6 릴리스를 지원하도록 업데이트되었습니다.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.8</td>
<td>v1.11.9</td>
<td>[Kubernetes 릴리스 정보![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.9)를 참조하십시오.</td>
</tr>
<tr>
<td>Kubernetes DNS</td>
<td>1.14.10</td>
<td>1.14.13</td>
<td>[Kubernetes DNS 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/dns/releases/tag/1.14.13)를 참조하십시오.</td>
</tr>
<tr>
<td>신뢰할 수 있는 컴퓨팅 에이전트</td>
<td>a02f765</td>
<td>e132aa4</td>
<td>[CVE-2017-12447 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-12447)의 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>Ubuntu 16.04 커널</td>
<td>4.4.0-143-generic</td>
<td>4.4.0-145-generic</td>
<td>[CVE-2019-9213 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html)의 커널 업데이트로 작업자 노드 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>Ubuntu 18.04 커널</td>
<td>4.15.0-46-generic</td>
<td>4.15.0-47-generic</td>
<td>[CVE-2019-9213 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html)의 커널 업데이트로 작업자 노드 이미지가 업데이트되었습니다.</td>
</tr>
</tbody>
</table>

### 2019년 4월 1일에 릴리스된 작업자 노드 수정팩 1.11.8_1553에 대한 변경 로그
{: #1118_1553}

다음 표는 작업자 노드 수정팩 1.11.8_1553에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.11.8_1552 이후에 작성된 변경사항">
<caption>버전 1.11.8_1552 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>작업자 노드 리소스 활용</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>해당 컴포넌트의 리소스 소진을 방지하기 위해 kubelet 및 containerd에 대한 메모리 예약을 늘렸습니다. 자세한 정보는 [작업자 노드 리소스 예약](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node)을 참조하십시오.</td>
</tr>
</tbody>
</table>

### 2019년 3월 26일에 릴리스된 마스터 수정팩 1.11.8_1552에 대한 변경 로그
{: #1118_1552}

다음 표는 마스터 수정팩 1.11.8_1552에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.11.8_1550 이후에 작성된 변경사항">
<caption>버전 1.11.8_1550 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 플러그인</td>
<td>345</td>
<td>346</td>
<td>[CVE-2019-9741 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741)의 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>키 관리 서비스 제공자</td>
<td>166</td>
<td>167</td>
<td>Kubernetes 시크릿을 관리하기 위해 중간 `context deadline exceeded` 및 `timeout` 오류를 수정합니다. 또한 기존 Kubernetes 시크릿이 암호되지 않은 상태로 유지될 수 있는 키 관리 서비스에 대한 업데이트를 수정합니다. 업데이트에는 [CVE-2019-9741 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741)에 대한 수정사항이 포함되어 있습니다.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} 제공자의 로드 밸런서 및 로드 밸런서 모니터</td>
<td>143</td>
<td>146</td>
<td>[CVE-2019-9741 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741)의 이미지가 업데이트되었습니다.</td>
</tr>
</tbody>
</table>

### 2019년 3월 20일에 릴리스된 1.11.8_1550에 대한 변경 로그
{: #1118_1550}

다음 표는 패치 1.11.8_1550에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.11.8_1547 이후에 작성된 변경사항">
<caption>버전 1.11.8_1547 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>클러스터 마스터 HA 프록시 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>클러스터 마스터에 대한 간헐적인 연결 실패를 보다 잘 처리하기 위한 구성이 업데이트되었습니다.</td>
</tr>
<tr>
<td>GPU 디바이스 플러그인 및 설치 프로그램</td>
<td>e32d51c</td>
<td>9ff3fda</td>
<td>[418.43 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.nvidia.com/object/unix.html)에 대한 GPU 드라이버가 업데이트되었습니다. 업데이트에는 [CVE-2019-9741 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9741.html)에 대한 수정사항이 포함되어 있습니다.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 플러그인</td>
<td>344</td>
<td>345</td>
<td>[개인 서비스 엔드포인트](/docs/containers?topic=containers-cs_network_cluster#set-up-private-se)에 대한 지원이 추가되었습니다.</td>
</tr>
<tr>
<td>Kernel</td>
<td>4.4.0-141</td>
<td>4.4.0-143</td>
<td>[CVE-2019-6133 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-6133.html)의 커널 업데이트로 작업자 노드 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>키 관리 서비스 제공자</td>
<td>136</td>
<td>166</td>
<td>[CVE-2018-16890 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16890), [CVE-2019-3822 ![ 외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3822) 및 [CVE-2019-3823 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3823)의 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>신뢰할 수 있는 컴퓨팅 에이전트</td>
<td>5f3d092</td>
<td>a02f765</td>
<td>[CVE-2018-10779 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10779), [CVE-2018-12900 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-12900), [CVE-2018-17000 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-17000), [CVE-2018-19210 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-19210), [CVE-2019-6128 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6128) 및 [CVE-2019-7663 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-7663)의 이미지가 업데이트되었습니다.</td>
</tr>
</tbody>
</table>

### 2019년 3월 4일에 릴리스된 1.11.8_1547에 대한 변경 로그
{: #1118_1547}

다음 표는 패치 1.11.8_1547에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.11.7_1546 이후에 작성된 변경사항">
<caption>버전 1.11.7_1546 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>GPU 디바이스 플러그인 및 설치 프로그램</td>
<td>eb3a259</td>
<td>e32d51c</td>
<td>[CVE-2019-6454 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454)의 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.11.7-198</td>
<td>v1.11.8-219</td>
<td>Kubernetes 1.11.8 릴리스를 지원하도록 업데이트되었습니다. `externalTrafficPolicy`를 `local`로 설정하는 로드 밸런서의 주기적인 연결 문제가 해결되었습니다. 최신 {{site.data.keyword.Bluemix_notm}} 문서 링크를 사용하기 위해 로드 밸런서 이벤트가 업데이트되었습니다.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 플러그인</td>
<td>342</td>
<td>344</td>
<td>이미지에 대한 기본 운영 체제가 Fedora에서 Alpine으로 변경되었습니다. [CVE-2019-6486 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486)의 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>키 관리 서비스 제공자</td>
<td>122</td>
<td>136</td>
<td>클라이언트의 제한시간이 {{site.data.keyword.keymanagementservicefull_notm}}로 증가되었습니다. [CVE-2019-6486 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486)의 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.7</td>
<td>v1.11.8</td>
<td>[Kubernetes 릴리스 정보![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.8)를 참조하십시오. 업데이트는 [CVE-2019-6486 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) 및 [CVE-2019-1002100 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1002100)을 해결합니다.</td>
</tr>
<tr>
<td>Kubernetes 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>`ExperimentalCriticalPodAnnotation=true`가 `--feature-gates` 옵션에 추가되었습니다. 이 설정은 팟(Pod)을 더 이상 사용되지 않는 `scheduler.alpha.kubernetes.io/critical-pod` 어노테이션에서 [Kubernetes 팟(Pod) 우선순위 지원](/docs/containers?topic=containers-pod_priority#pod_priority)으로 마이그레이션하는 데 도움이 됩니다.</td>
</tr>
<tr>
<td>Kubernetes DNS</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>보다 많은 클러스터 서비스를 처리하기 위해서 Kubernetes DNS 팟(Pod) 메모리 한계가 `170Mi`에서 `400Mi`로 증가되었습니다.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} 제공자의 로드 밸런서 및 로드 밸런서 모니터</td>
<td>132</td>
<td>143</td>
<td>[CVE-2019-6486 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486)의 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>OpenVPN 클라이언트 및 서버</td>
<td>2.4.6-r3-IKS-13</td>
<td>2.4.6-r3-IKS-25</td>
<td>[CVE-2019-1559 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559)의 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>신뢰할 수 있는 컴퓨팅 에이전트</td>
<td>1ea5ad3</td>
<td>5f3d092</td>
<td>[CVE-2019-6454 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454)의 이미지가 업데이트되었습니다.</td>
</tr>
</tbody>
</table>

### 2019년 2월 27일에 릴리스된 작업자 노드 수정팩 1.11.7_1546에 대한 변경 로그
{: #1117_1546}

다음 표는 작업자 노드 수정팩 1.11.7_1546에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.11.7_1546 이후에 작성된 변경사항">
<caption>버전 1.11.7_1546 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-141</td>
<td>4.4.0-142</td>
<td>[CVE-2018-19407 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-142.168/changelog)의 커널 업데이트로 작업자 노드 이미지가 업데이트되었습니다.</td>
</tr>
</tbody>
</table>

### 2019년 2월 15일에 릴리스된 작업자 노드 수정팩 1.11.7_1544에 대한 변경 로그
{: #1117_1544}

다음 표는 작업자 노드 수정팩 1.11.7_1544에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.11.7_1543 이후에 작성된 변경사항">
<caption>버전 1.11.7_1543 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>클러스터 마스터 HA 프록시 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>팟(Pod) 구성 `spec.priorityClassName` 값을 `system-node-critical`로 변경하고 `spec.priority` 값을 `2000001000`으로 설정합니다.</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.5</td>
<td>1.1.6</td>
<td>[containerd 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/containerd/containerd/releases/tag/v1.1.6)를 참조하십시오. 업데이트는 [CVE-2019-5736 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5736)를 해결합니다.</td>
</tr>
<tr>
<td>Kubernetes `kubelet` 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>중요한 정적 팟(pod) 제거를 방지하기 위해 `ExperimentalCriticalPodAnnotation` 기능 게이트가 사용으로 설정되었습니다.</td>
</tr>
</tbody>
</table>

### 2019년 2월 5일에 릴리스된 1.11.7_1543에 대한 변경 로그
{: #1117_1543}

다음 표는 패치 1.11.7_1543에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.11.6_1541 이후에 작성된 변경사항">
<caption>버전 1.11.6_1541 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>etcd</td>
<td>v3.3.1</td>
<td>v3.3.11</td>
<td>[etcd 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/coreos/etcd/releases/v3.3.11)를 참조하십시오. 또한 etcd에 대해 지원되는 암호화 스위트는 이제 고강도의 암호화(128비트 이상)를 사용하는 서브세트로 제한됩니다.</td>
</tr>
<tr>
<td>GPU 디바이스 플러그인 및 설치 프로그램</td>
<td>13fdc0d</td>
<td>eb3a259</td>
<td>[CVE-2019-3462 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) 및 [CVE-2019-6486 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486)의 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.11.6-180</td>
<td>v1.11.7-198</td>
<td>Kubernetes 1.11.7 릴리스를 지원하도록 업데이트되었습니다. 또한 `calicoctl` 버전이 3.3.1로 업데이트되었습니다. 작업자 노드 상태 변경에 대해 버전 2.0로드 밸런서에 대한 불필요한 구성 업데이트가 해결되었습니다.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 플러그인</td>
<td>338</td>
<td>342</td>
<td>파일 스토리지 플러그인이 다음과 같이 업데이트되었습니다.
<ul><li>[볼륨 토폴로지 인식 스케줄링](/docs/containers?topic=containers-file_storage#file-topology)을 사용한 동적 프로비저닝을 지원합니다.</li>
<li>스토리지가 이미 삭제된 경우, 지속적 볼륨 클레임(PVC) 삭제 오류가 무시됩니다.</li>
<li>실패 메시지 어노테이션을 실패한 PVC에 추가합니다.</li>
<li>스토리지 프로비저너 제어기의 리더 선택 및 재동기 기간 설정을 최적화하고 프로비저닝 제한시간을 30분에서 1시간으로 늘립니다.</li>
<li>프로비저닝을 시작하기 전에 사용자 권한을 확인합니다.</li></ul></td>
</tr>
<tr>
<td>키 관리 서비스 제공자</td>
<td>111</td>
<td>122</td>
<td>Kubernetes 시크릿을 {{site.data.keyword.keymanagementservicefull_notm}}에서 관리하는 경우 일시적인 장애를 방지하기 위해 재시도 로직이 추가되었습니다.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.6</td>
<td>v1.11.7</td>
<td>[Kubernetes 릴리스 정보![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.7)를 참조하십시오.</td>
</tr>
<tr>
<td>Kubernetes 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>Kubernetes API 서버 감사 정책 구성이 `cluster-admin` 요청에 대한 메타데이터 로깅 및 워크로드 `create`, `update` 및 `patch` 요청의 요청 본문 로깅을 포함하도록 업데이트되었습니다.</td>
</tr>
<tr>
<td>OpenVPN 클라이언트</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>[CVE-2018-0734 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) 및 [CVE-2018-5407 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407)의 이미지가 업데이트되었습니다. 또한, 이제 configmap이 아닌 시크릿에서 팟(Pod) 구성을 가져옵니다.</td>
</tr>
<tr>
<td>OpenVPN 서버</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>[CVE-2018-0734 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) 및 [CVE-2018-5407 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407)의 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>systemd</td>
<td>230</td>
<td>229</td>
<td>[CVE-2018-16864 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16864)에 대한 보안 패치</td>
</tr>
</tbody>
</table>

### 2019년 1월 28일에 릴리스된 작업자 노드 수정팩 1.11.6_1541에 대한 변경 로그
{: #1116_1541}

다음 표는 작업자 노드 수정팩 1.11.6_1541에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.11.6_1540 이후에 작성된 변경사항">
<caption>버전 1.11.6_1540 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 패키지</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>[CVE-2019-3462 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462)/[USN-3863-1 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://usn.ubuntu.com/3863-1)에 대한 `apt`를 포함하여 설치된 Ubuntu 패키지를 업데이트합니다.</td>
</tr>
</tbody>
</table>

### 2019년 1월 21일에 릴리스된 1.11.6_1540에 대한 변경 로그
{: #1116_1540}

다음 표는 패치 1.11.6_1540에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.11.5_1539 이후에 작성된 변경사항">
<caption>버전 1.11.5_1539 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.11.5-152</td>
<td>v1.11.6-180</td>
<td>Kubernetes 1.11.6 릴리스를 지원하도록 업데이트되었습니다.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.5</td>
<td>v1.11.6</td>
<td>[Kubernetes 릴리스 정보![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.6)를 참조하십시오.</td>
</tr>
<tr>
<td>Kubernetes 추가 기능 resizer</td>
<td>1.8.1</td>
<td>1.8.4</td>
<td>[Kubernetes 추가 기능 resizer 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/autoscaler/releases/tag/addon-resizer-1.8.4)를 참조하십시오.</td>
</tr>
<tr>
<td>Kubernetes 대시보드</td>
<td>v1.8.3</td>
<td>v1.10.1</td>
<td>[Kubernetes 대시보드 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/dashboard/releases/tag/v1.10.1)를 참조하십시오. 업데이트는 [CVE-2018-18264 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-18264)를 해결합니다.<br><br>`kubectl proxy`를 통해 대시보드에 액세스할 경우 로그인 페이지의 **건너뛰기** 단추가 제거됩니다. 대신 [**토큰**을 사용하여 로그인](/docs/containers?topic=containers-app#cli_dashboard)하십시오.</td>
</tr>
<tr>
<td>GPU 설치 프로그램</td>
<td>390.12</td>
<td>410.79</td>
<td>설치된 GPU 드라이버가 410.79로 업데이트되었습니다.</td>
</tr>
</tbody>
</table>

### 2019년 1월 7일에 릴리스된 작업자 노드 수정팩 1.11.5_1539에 대한 변경 로그
{: #1115_1539}

다음 표는 작업자 노드 수정팩 1.11.5_1539에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.11.5_1538 이후에 작성된 변경사항">
<caption>버전 1.11.5_1538 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-139</td>
<td>4.4.0-141</td>
<td>[CVE-2017-5753, CVE-2018-18690 ![외부 링크 아이콘n](../icons/launch-glyph.svg "외부 링크 아이콘")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-141.167/changelog)의 커널 업데이트로 작업자 노드 이미지가 업데이트되었습니다.</td>
</tr>
</tbody>
</table>

### 2018년 12월 17일에 릴리스된 작업자 노드 수정팩 1.11.5_1538에 대한 변경 로그
{: #1115_1538}

다음 표는 작업자 노드 수정팩 1.11.5_1538에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.11.5_1537 이후에 작성된 변경사항">
<caption>버전 1.11.5_1537 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 패키지</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>설치된 Ubuntu 패키지로 업데이트되었습니다.</td>
</tr>
</tbody>
</table>

### 2018년 12월 5일에 릴리스된 1.11.5_1537에 대한 변경 로그
{: #1115_1537}

다음 표는 패치 1.11.5_1537에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.11.4_1536 이후에 작성된 변경사항">
<caption>버전 1.11.4_1536 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.11.4-142</td>
<td>v1.11.5-152</td>
<td>Kubernetes 1.11.5 릴리스를 지원하도록 업데이트되었습니다.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.4</td>
<td>v1.11.5</td>
<td>[Kubernetes 릴리스 정보![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.5)를 참조하십시오. 업데이트는 [CVE-2018-1002105 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/kubernetes/issues/71411)를 해결합니다.</td>
</tr>
</tbody>
</table>

### 2018년 12월 4일에 릴리스된 작업자 노드 수정팩 1.11.4_1536에 대한 변경 로그
{: #1114_1536}

다음 표는 작업자 노드 수정팩 1.11.4_1536에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.11.4_1535 이후에 작성된 변경사항">
<caption>버전 1.11.4_1535 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>작업자 노드 리소스 활용</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>해당 컴포넌트의 리소스 소진을 방지하기 위해 kubelet 및 containerd에 대한 전용 cgroup이 추가되었습니다. 자세한 정보는 [작업자 노드 리소스 예약](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node)을 참조하십시오.</td>
</tr>
</tbody>
</table>

### 2018년 11월 27일에 릴리스된 1.11.4_1535에 대한 변경 로그
{: #1114_1535}

다음 표는 패치 1.11.4_1535에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.11.3_1534 이후에 작성된 변경사항">
<caption>버전 1.11.3_1534 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.2.1</td>
<td>v3.3.1</td>
<td>[Calico 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.projectcalico.org/v3.3/releases/#v331)를 참조하십시오. 업데이트는 [Tigera Technical Advisory TTA-2018-001 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.projectcalico.org/security-bulletins/)을 해결합니다.</td>
</tr>
<tr>
<td>containerd</td>
<td>v1.1.4</td>
<td>v1.1.5</td>
<td>[containerd 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/containerd/containerd/releases/tag/v1.1.5)를 참조하십시오. [팟(Pod)의 종료를 중지 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/containerd/containerd/issues/2744)시킬 수 있는 교착 상태를 해결하기 위해 containerd가 업데이트되었습니다.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.11.3-127</td>
<td>v1.11.4-142</td>
<td>Kubernetes 1.11.4 릴리스를 지원하도록 업데이트되었습니다.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.3</td>
<td>v1.11.4</td>
<td>[Kubernetes 릴리스 정보![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.4)를 참조하십시오.</td>
</tr>
<tr>
<td>OpenVPN 클라이언트 및 서버</td>
<td>2.4.4-r1-6</td>
<td>2.4.6-r3-IKS-8</td>
<td>[CVE-2018-0732 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732) 및 [CVE-2018-0737 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737)의 이미지가 업데이트되었습니다.</td>
</tr>
</tbody>
</table>

### 2018년 11월 19일에 릴리스된 작업자 노드 수정팩 1.11.3_1534에 대한 변경 로그
{: #1113_1534}

다음 표는 작업자 노드 수정팩 1.11.3_1534에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.11.3_1533 이후에 작성된 변경사항">
<caption>버전 1.11.3_1533 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-137</td>
<td>4.4.0-139</td>
<td>[CVE-2018-7755 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-139.165/changelog)의 커널 업데이트로 작업자 노드 이미지가 업데이트되었습니다.</td>
</tr>
</tbody>
</table>


### 2018년 11월 7일에 릴리스된 1.11.3_1533에 대한 변경 로그
{: #1113_1533}

다음 표는 패치 1.11.3_1533에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.11.3_1531 이후에 작성된 변경사항">
<caption>버전 1.11.3_1531 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>클러스터 마스터 HA 업데이트</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>`initializerconfigurations`, `mutatingwebhookconfigurations` 또는 `validatingwebhookconfigurations` 등의 승인 웹 훅을 사용하는 클러스터의 고가용성(HA) 마스터에 대한 업데이트가 해결되었습니다. [컨테이너 이미지 보안 적용](/docs/services/Registry?topic=registry-security_enforce#security_enforce) 등에 대해 Helm 차트에서 이러한 웹 훅을 사용할 수 있습니다.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.11.3-100</td>
<td>v1.11.3-127</td>
<td>로드 밸런서 서비스가 배치되는 VLAN을 지정하기 위해 `service.kubernetes.io/ibm-load-balancer-cloud-provider-vlan` 어노테이션이 추가되었습니다. 클러스터에서 사용 가능한 VLAN을 보려면 `ibmcloud ks vlans --zone <zone>`을 실행하십시오.</td>
</tr>
<tr>
<td>TPM 사용 커널</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>신뢰할 수 있는 컴퓨팅의 TPM 칩이 있는 베어메탈 작업자 노드는 신뢰가 사용으로 설정될 때까지 기본 Ubuntu 커널을 사용합니다. 기존 클러스터에서 [신뢰를 사용으로 설정](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable)하는 경우에는 TPM 칩이 있는 기존 베어메탈 작업자 노드를 [다시 로드](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)해야 합니다. 베어메탈 작업자 노드에 TPM 칩이 있는지 확인하려면 `ibmcloud ks machine-types --zone` [명령](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_machine_types)을 실행한 후에 **Trustable** 필드를 검토하십시오.</td>
</tr>
</tbody>
</table>

### 2018년 11월 1일에 릴리스된 마스터 수정팩 1.11.3_1531에 대한 변경 로그
{: #1113_1531_ha-master}

다음 표는 마스터 수정팩 1.11.3_1531에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.11.3_1527 이후에 작성된 변경사항">
<caption>버전 1.11.3_1527 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>클러스터 마스터</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>고가용성(HA)을 높이도록 클러스터 마스터 구성이 업데이트되었습니다. 클러스터에는 이제 각 마스터가 별도의 실제 호스트에 전개된 고가용성(HA) 구성으로 설정된 3개의 Kubernetes 마스터 복제본이 있습니다. 또한 클러스터가 다중 구역 가능 구역에 있을 경우 마스터가 전체 구역에 분산됩니다.<br>취해야 할 조치는 [고가용성 클러스터 마스터로 업데이트](/docs/containers?topic=containers-cs_versions#ha-masters)를 참조하십시오. 이 준비 조치는 다음과 같은 경우에 적용됩니다.<ul>
<li>방화벽 또는 사용자 정의 Calico 네트워크 정책이 있는 경우</li>
<li>작업자 노드에서 호스트 포트 `2040` 또는 `2041`을 사용 중인 경우</li>
<li>마스터에 대한 클러스터 내부 액세스를 위해 클러스터 마스터 IP 주소를 사용한 경우</li>
<li>Calico 정책을 작성하기 위해 Calico API 또는 CLI(`calicoctl`)를 호출하는 자동화가 있는 경우</li>
<li>마스터에 대한 팟(Pod) 유출 액세스를 제어하기 위해 Kubernetes 또는 Calico 네트워크 정책을 사용하는 경우</li></ul></td>
</tr>
<tr>
<td>클러스터 마스터 HA 프록시</td>
<td>해당사항 없음</td>
<td>1.8.12-alpine</td>
<td>각 작업자 노드 클라이언트가 사용 가능한 HA 마스터 복제본으로 요청을 라우팅할 수 있도록 모든 작업자 노드에서 클라이언트측 로드 밸런싱을 위해 `ibm-master-proxy-*` 팟(Pod)이 추가되었습니다.</td>
</tr>
<tr>
<td>etcd</td>
<td>v3.2.18</td>
<td>v3.3.1</td>
<td>[etcd 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/coreos/etcd/releases/v3.3.1)를 참조하십시오.</td>
</tr>
<tr>
<td>etcd에서 데이터 암호화</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>이전에는 etcd 데이터가 마스터의 NFS 파일 스토리지 인스턴스에 저장되어 저장 상태로 암호화되었습니다. 이제 etcd 데이터는 마스터의 로컬 디스크에 저장되고, {{site.data.keyword.cos_full_notm}}에 백업됩니다. 데이터는 {{site.data.keyword.cos_full_notm}}로 전송 중 및 저장 중에 암호화됩니다. 그러나 마스터의 로컬 디스크에 있는 etcd 데이터는 암호화되지 않습니다. 마스터의 로컬 etcd 데이터를 암호화하려는 경우 [클러스터에서 {{site.data.keyword.keymanagementservicelong_notm}}를 사용으로 설정](/docs/containers?topic=containers-encryption#keyprotect)하십시오.</td>
</tr>
</tbody>
</table>

### 2018년 10월 26일에 릴리스된 작업자 노드 수정팩 1.11.3_1531에 대한 변경 로그
{: #1113_1531}

다음 표는 작업자 노드 수정팩 1.11.3_1531에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.11.3_1525 이후에 작성된 변경사항">
<caption>버전 1.11.3_1525 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>OS 인터럽트 처리</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>인터럽트 요청(IRQ) 시스템 디먼이 보다 성능 기준에 맞는 인터럽트 핸들러로 대체되었습니다.</td>
</tr>
</tbody>
</table>

### 2018년 10월 15일에 릴리스된 마스터 수정팩 1.11.3_1527에 대한 변경 로그
{: #1113_1527}

다음 표는 마스터 수정팩 1.11.3_1527에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.11.3_1524 이후에 변경된 사항">
<caption>버전 1.11.3_1524 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>노드 실패를 더 잘 처리하기 위해 `calico-node` 컨테이너 준비 상태 프로브가 수정되었습니다.</td>
</tr>
<tr>
<td>클러스터 업데이트</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>마스터가 지원되지 않는 버전에서 업데이트되는 경우 발생하는 클러스터 추가 기능 업데이트 관련 문제점이 수정되었습니다.</td>
</tr>
</tbody>
</table>

### 2018년 10월 10일에 릴리스된 작업자 노드 수정팩 1.11.3_1525에 대한 변경 로그
{: #1113_1525}

다음 표는 작업자 노드 수정팩 1.11.3_1525에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.11.3_1524 이후에 변경된 사항">
<caption>버전 1.11.3_1524 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-133</td>
<td>4.4.0-137</td>
<td>[CVE-2018-14633, CVE-2018-17182 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-137.163/changelog)의 커널 업데이트로 작업자 노드 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>비활성 세션 제한시간</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>규제 준수를 위해 비활성 세션 제한시간이 5분으로 설정되었습니다.</td>
</tr>
</tbody>
</table>


### 2018년 10월 2일에 릴리스된 1.11.3_1524에 대한 변경 로그
{: #1113_1524}

다음 표는 패치 1.11.3_1524에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.11.3_1521 이후에 변경된 사항">
<caption>버전 1.11.3_1521 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>containerd</td>
<td>1.1.3</td>
<td>1.1.4</td>
<td>[containerd 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/containerd/containerd/releases/tag/v1.1.4)를 참조하십시오.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.11.3-91</td>
<td>v1.11.3-100</td>
<td>로드 밸런서 오류 메시지의 문서 링크가 업데이트되었습니다.</td>
</tr>
<tr>
<td>IBM 파일 스토리지 클래스</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>IBM 파일 스토리지 클래스에서 중복 `reclaimPolicy` 매개변수가 제거되었습니다.<br><br>
또한 이제 클러스터 마스터를 업데이트할 때 기본 IBM 파일 스토리지 클래스가 변경되지 않습니다. 기본 스토리지 클래스를 변경하려면 `kubectl patch storageclass <storageclass> -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'`를 실행하고 `<storageclass>`를 스토리지 클래스의 이름으로 대체하십시오.</td>
</tr>
</tbody>
</table>

### 2018년 9월 20일에 릴리스된 1.11.3_1521에 대한 변경 로그
{: #1113_1521}

다음 표는 패치 1.11.3_1521에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.11.2_1516 이후에 변경된 사항">
<caption>버전 1.11.2_1516 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.11.2-71</td>
<td>v1.11.3-91</td>
<td>Kubernetes 1.11.3 릴리스를 지원하도록 업데이트되었습니다.</td>
</tr>
<tr>
<td>IBM 파일 스토리지 클래스</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>작업자 노드에서 제공하는 기본값을 사용하기 위해 IBM 파일 스토리지 클래스의 `mountOptions`가 제거되었습니다.<br><br>
또한 이제 클러스터 마스터를 업데이트할 때 기본 IBM 파일 스토리지 클래스가 `ibmc-file-bronze`로 유지됩니다. 기본 스토리지 클래스를 변경하려면 `kubectl patch storageclass <storageclass> -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'`를 실행하고 `<storageclass>`를 스토리지 클래스의 이름으로 대체하십시오.</td>
</tr>
<tr>
<td>키 관리 서비스 제공자</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>{{site.data.keyword.keymanagementservicefull}}를 지원하기 위해 클러스터에서 Kubernetes 키 관리 서비스(KMS) 제공자를 사용하는 기능이 추가되었습니다. [클러스터에서 {{site.data.keyword.keymanagementserviceshort}}를 사용으로 설정](/docs/containers?topic=containers-encryption#keyprotect)하면 모든 Kubernetes secret이 암호화됩니다.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.2</td>
<td>v1.11.3</td>
<td>[Kubernetes 릴리스 정보![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.3)를 참조하십시오.</td>
</tr>
<tr>
<td>Kubernetes DNS Autoscaler</td>
<td>1.1.2-r2</td>
<td>1.2.0</td>
<td>[Kubernetes DNS Autoscaler 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes-incubator/cluster-proportional-autoscaler/releases/tag/1.2.0)를 참조하십시오.</td>
</tr>
<tr>
<td>로그 순환</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>90일 내에 다시 로드되거나 업데이트되지 않은 작업자 노드에 대해 `logrotate`가 실패하는 것을 방지하기 위해 `cronjobs` 대신 `systemd` 타이머를 사용하도록 전환되었습니다. **참고**: 모든 이전 부 릴리스 버전에서는 로그가 순환되지 않으므로 cron 작업이 실패한 후 기본 디스크가 채워집니다. cron 작업은 작업자 노드가 업데이트되거나 다시 로드되지 않고 90일 이상 활성 상태를 유지하면 실패합니다. 로그가 기본 디스크를 모두 채우면 작업자 노드가 실패 상태로 전환됩니다. 이 작업자 노드는 `ibmcloud ks worker-reload` [명령](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) 또는 `ibmcloud ks worker-update` [명령](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update)을 사용하여 수정할 수 있습니다.</td>
</tr>
<tr>
<td>루트 비밀번호 만료</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>규제 준수를 위해 작업자 노드의 루트 비밀번호는 90일이 지나면 만료됩니다. 자동화 도구가 작업자 노드에 루트로 로그인해야 하거나 루트로서 실행되는 cron 작업에 의존하는 경우에는 작업자 노드에 로그인한 후 `chage -M -1 root`를 실행하여 비밀번호 만료를 사용 안함으로 설정할 수 있습니다. **참고**: 루트로서 실행하는 것 또는 비밀번호 만료를 제거하는 것을 금지하는 보안 규제가 있는 경우에는 만료를 사용 안함으로 설정하지 마십시오. 대신 작업자 노드를 90일마다 [업데이트](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update)하거나 [다시 로드](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)할 수 있습니다.</td>
</tr>
<tr>
<td>작업자 노드 런타임 컴포넌트(`kubelet`, `kube-proxy`, `containerd`)</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>런타임 컴포넌트의 기본 디스크에 대한 종속성이 제거되었습니다. 이 개선사항은 기본 디스크가 가득 찬 경우 작업자 노드가 실패하지 않도록 합니다.</td>
</tr>
<tr>
<td>systemd</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>주기적으로 임시 마운트 장치를 정리하여 이들이 바인드 해제되지 않도록 합니다. 이 조치는 [Kubernetes 문제 57345 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/kubernetes/issues/57345)를 해결합니다.</td>
</tr>
</tbody>
</table>

### 2018년 9월 4일에 릴리스된 1.11.2_1516에 대한 변경 로그
{: #1112_1516}

다음 표는 패치 1.11.2_1516에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.11.2_1514 이후에 작성된 변경사항">
<caption>버전 1.11.2_1514 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.1.3</td>
<td>v3.2.1</td>
<td>[Calico 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.projectcalico.org/v3.2/releases/#v321)를 참조하십시오.</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.2</td>
<td>1.1.3</td>
<td>[`containerd` 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/containerd/containerd/releases/tag/v1.1.3)를 참조하십시오.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.11.2-60</td>
<td>v1.11.2-71</td>
<td>`externalTrafficPolicy`가 `local`로 설정된 로드 밸런서 서비스에 대한 업데이트 처리를 개선하도록 클라우드 제공자 구성이 변경되었습니다.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 플러그인 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>IBM 제공 파일 스토리지 클래스의 마운트 옵션에서 기본 NFS 버전이 제거되었습니다. 호스트의 운영 체제가 이제 IBM Cloud 인프라(SoftLayer) NFS 서버와 NFS 버전을 조정합니다. 특정 NFS 버전을 수동으로 설정하거나 호스트의 운영 체제에 의해 조정된 PV의 NFS 버전을 변경하려면 [기본 NFS 버전 변경](/docs/containers?topic=containers-file_storage#nfs_version_class)을 참조하십시오.</td>
</tr>
</tbody>
</table>

### 2018년 8월 23일에 릴리스된 작업자 노드 수정팩 1.11.2_1514에 대한 변경 로그
{: #1112_1514}

다음 표는 작업자 노드 수정팩 1.11.2_1514에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.11.2_1513 이후에 작성된 변경사항">
<caption>버전 1.11.2_1513 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>`systemd`</td>
<td>229</td>
<td>230</td>
<td>`cgroup` 유출을 정정하도록 `systemd`가 업데이트되었습니다.</td>
</tr>
<tr>
<td>Kernel</td>
<td>4.4.0-127</td>
<td>4.4.0-133</td>
<td>[CVE-2018-3620,CVE-2018-3646 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://usn.ubuntu.com/3741-1/)의 커널 업데이트로 작업자 노드 이미지가 업데이트되었습니다.</td>
</tr>
</tbody>
</table>

### 2018년 8월 14일에 릴리스된 1.11.2_1513에 대한 변경 로그
{: #1112_1513}

다음 표는 패치 1.11.2_1513에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.10.5_1518 이후에 작성된 변경사항">
<caption>버전 1.10.5_1518 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>containerd</td>
<td>해당사항 없음</td>
<td>1.1.2</td>
<td>`containerd`가 Kubernetes의 새 컨테이너 런타임으로서 Docker를 대체합니다. [`containerd` 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/containerd/containerd/releases/tag/v1.1.2)를 참조하십시오. 취해야 할 조치는 [컨테이너 런타임으로서 `containerd`로 업데이트](/docs/containers?topic=containers-cs_versions#containerd)를 참조하십시오.</td>
</tr>
<tr>
<td>Docker</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>`containerd`가 성능 향상을 위해 Kubernetes의 새 컨테이너 런타임으로서 Docker를 대체합니다. 취해야 할 조치는 [컨테이너 런타임으로서 `containerd`로 업데이트](/docs/containers?topic=containers-cs_versions#containerd)를 참조하십시오.</td>
</tr>
<tr>
<td>etcd</td>
<td>v3.2.14</td>
<td>v3.2.18</td>
<td>[etcd 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/coreos/etcd/releases/v3.2.18)를 참조하십시오.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.10.5-118</td>
<td>v1.11.2-60</td>
<td>Kubernetes 1.11 릴리스를 지원하도록 업데이트되었습니다. 또한 로드 밸런서 팟(Pod)이 이제 새 `ibm-app-cluster-critical` 팟(Pod) 우선순위 클래스를 사용합니다.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 플러그인</td>
<td>334</td>
<td>338</td>
<td>`incubator` 버전이 1.8로 업데이트되었습니다. 파일 스토리지가 사용자가 선택한 특정 구역으로 프로비저닝됩니다. 다중 구역 클러스터를 사용 중이며 [지역 및 구역 레이블을 추가](/docs/containers?topic=containers-kube_concepts#storage_multizone)해야 하는 경우가 아니면 기존의 (정적) PV 인스턴스 레이블을 업데이트할 수 없습니다.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.10.5</td>
<td>v1.11.2</td>
<td>[Kubernetes 릴리스 정보![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.2)를 참조하십시오.</td>
</tr>
<tr>
<td>Kubernetes 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>클러스터의 Kubernetes API 서버에 대한 OpenID Connect 구성이 {{site.data.keyword.Bluemix_notm}} IAM(Identity Access and Management) 액세스 관리를 지원하도록 업데이트되었습니다. 클러스터의 Kubernetes API 서버에 대한 `--enable-admission-plugins` 옵션에 `Priority`가 추가되었으며 팟(Pod) 우선순위를 지원하도록 클러스터가 구성되었습니다. 자세한 정보는 다음을 참조하십시오.
<ul><li>[{{site.data.keyword.Bluemix_notm}}IAM 액세스 그룹](/docs/containers?topic=containers-users#rbac)</li>
<li>[팟(Pod) 우선순위 구성](/docs/containers?topic=containers-pod_priority#pod_priority)</li></ul></td>
</tr>
<tr>
<td>Kubernetes Heapster</td>
<td>v1.5.2</td>
<td>v.1.5.4</td>
<td>`heapster-nanny` 컨테이너에 대한 리소스 한계가 늘어났습니다. [Kubernetes Heapster 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/heapster/releases/tag/v1.5.4)를 참조하십시오.</td>
</tr>
<tr>
<td>로깅 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>이제 컨테이너 로그 디렉토리가 이전의 `/var/lib/docker/containers/`가 아닌 `/var/log/pods/`입니다.</td>
</tr>
</tbody>
</table>

<br />


## 아카이브
{: #changelog_archive}

지원되지 않는 Kubernetes 버전:
*  [버전 1.10](#110_changelog)
*  [버전 1.9](#19_changelog)
*  [버전 1.8](#18_changelog)
*  [버전 1.7](#17_changelog)

### 버전 1.10 변경 로그(2019년 5월 16일 현재 지원되지 않음)
{: #110_changelog}

버전 1.10 변경 로그를 검토하십시오.
{: shortdesc}

*   [2019년 5월 13일에 릴리스된 작업자 노드 수정팩 1.10.13_1558에 대한 변경 로그](#11013_1558)
*   [2019년 4월 29일에 릴리스된 작업자 노드 수정팩 1.10.13_1557에 대한 변경 로그](#11013_1557)
*   [2019년 4월 15일에 릴리스된 작업자 노드 수정팩 1.10.13_1556 대한 변경 로그](#11013_1556)
*   [2019년 4월 8일에 릴리스된 1.10.13_1555에 대한 변경 로그](#11013_1555)
*   [2019년 4월 1일에 릴리스된 작업자 노드 수정팩 1.10.13_1554에 대한 변경 로그](#11013_1554)
*   [2019년 3월 26일에 릴리스된 마스터 수정팩 1.10.13_1553에 대한 변경 로그](#11118_1553)
*   [2019년 3월 20일에 릴리스된 1.10.13_1551에 대한 변경 로그](#11013_1551)
*   [2019년 3월 4일에 릴리스된 1.10.13_154에 대한 변경 로그](#11013_1548)
*   [2019년 2월 27일에 릴리스된 작업자 노드 수정팩 1.10.12_1546에 대한 변경 로그](#11012_1546)
*   [2019년 2월 15일에 릴리스된 작업자 노드 수정팩 1.10.12_1544에 대한 변경 로그](#11012_1544)
*   [2019년 2월 5일에 릴리스된 1.10.12_1543에 대한 변경 로그](#11012_1543)
*   [2019년 1월 28일에 릴리스된 작업자 노드 수정팩 1.10.12_1541에 대한 변경 로그](#11012_1541)
*   [2019년 1월 21일에 릴리스된 1.10.12_1540에 대한 변경 로그](#11012_1540)
*   [2019년 1월 7일에 릴리스된 작업자 노드 수정팩 1.10.11_1538에 대한 변경 로그](#11011_1538)
*   [2019년 12월 17일에 릴리스된 작업자 노드 수정팩 1.10.11_1537에 대한 변경 로그](#11011_1537)
*   [2018년 12월 4일에 릴리스된 1.10.11_1536에 대한 변경 로그](#11011_1536)
*   [2018년 11월 27일에 릴리스된 작업자 노드 수정팩 1.10.8_1532에 대한 변경 로그](#1108_1532)
*   [2018년 11월 19일에 릴리스된 작업자 노드 수정팩 1.10.8_1531에 대한 변경 로그](#1108_1531)
*   [2018년 11월 7일에 릴리스된 1.10.8_1530에 대한 변경 로그](#1108_1530_ha-master)
*   [2018년 10월 26일에 릴리스된 작업자 노드 수정팩 1.10.8_1528에 대한 변경 로그](#1108_1528)
*   [2018년 10월 10일에 릴리스된 작업자 노드 수정팩 1.10.8_1525에 대한 변경 로그](#1108_1525)
*   [2018년 10월 2일에 릴리스된 1.10.8_1524에 대한 변경 로그](#1108_1524)
*   [2018년 9월 20일에 릴리스된 작업자 노드 수정팩 1.10.7_1521에 대한 변경 로그](#1107_1521)
*   [2018년 9월 4일에 릴리스된 1.10.7_1520에 대한 변경 로그](#1107_1520)
*   [2018년 8월 23일에 릴리스된 작업자 노드 수정팩 1.10.5_1519에 대한 변경 로그](#1105_1519)
*   [2018년 8월 13일에 릴리스된 작업자 노드 수정팩 1.10.5_1518에 대한 변경 로그](#1105_1518)
*   [2018년 7월 27일에 릴리스된 1.10.5_1517에 대한 변경 로그](#1105_1517)
*   [2018년 7월 3일에 릴리스된 작업자 노드 수정팩 1.10.3_1514에 대한 변경 로그](#1103_1514)
*   [2018년 6월 21일에 릴리스된 작업자 노드 수정팩 1.10.3_1513에 대한 변경 로그](#1103_1513)
*   [2018년 6월 12일에 릴리스된 1.10.3_1512에 대한 변경 로그](#1103_1512)
*   [2018년 5월 18일에 릴리스된 작업자 노드 수정팩 1.10.1_1510에 대한 변경 로그](#1101_1510)
*   [2018년 5월 16일에 릴리스된 작업자 노드 수정팩 1.10.1_1509에 대한 변경 로그](#1101_1509)
*   [2018년 5월 1일에 릴리스된 1.10.1_1508에 대한 변경 로그](#1101_1508)

#### 2019년 5월 13일에 릴리스된 작업자 노드 수정팩 1.10.13_1558에 대한 변경 로그
{: #11013_1558}

다음 표는 작업자 노드 수정팩 1.10.13_1558에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.10.13_1557 이후에 작성된 변경사항">
<caption>버전 1.10.13_1557 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>클러스터 마스터 HA 프록시</td>
<td>1.9.6-alpine</td>
<td>1.9.7-alpine</td>
<td>[HAProxy 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.haproxy.org/download/1.9/src/CHANGELOG)를 참조하십시오. 업데이트는 [CVE-2019-6706 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6706)을 해결합니다.</td>
</tr>
</tbody>
</table>

#### 2019년 4월 29일에 릴리스된 작업자 노드 수정팩 1.10.13_1557에 대한 변경 로그
{: #11013_1557}

다음 표는 작업자 노드 수정팩 1.10.13_1557에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.10.13_1556 이후에 작성된 변경사항">
<caption>1.10.13_1556 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 패키지</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>설치된 Ubuntu 패키지로 업데이트되었습니다.</td>
</tr>
</tbody>
</table>


#### 2019년 4월 15일에 릴리스된 작업자 노드 수정팩 1.10.13_1556 대한 변경 로그
{: #11013_1556}

다음 표는 작업자 노드 수정팩 1.10.13_1556에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.10.13_1555 이후에 작성된 변경사항">
<caption>1.10.13_1555 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 패키지</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>[CVE-2019-3842 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-3842.html)에 대한 `systemd`를 포함하여 설치된 Ubuntu 패키지를 업데이트합니다.</td>
</tr>
</tbody>
</table>

#### 2019년 4월 8일에 릴리스된 1.10.13_1555에 대한 변경 로그
{: #11013_1555}

다음 표는 패치 1.10.13_155에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.10.13_1554 이후에 작성된 변경사항">
<caption>버전 1.10.13_1554 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>클러스터 마스터 HA 프록시</td>
<td>1.8.12-alpine</td>
<td>1.9.6-alpine</td>
<td>[HAProxy 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.haproxy.org/download/1.9/src/CHANGELOG)를 참조하십시오. 업데이트는 [CVE-2018-0732 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732), [CVE-2018-0734 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734), [CVE-2018-0737 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737), [CVE-2018-5407 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407), [CVE-2019-1543 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543) 및 [CVE-2019-1559 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559)를 해결합니다.</td>
</tr>
<tr>
<td>Kubernetes DNS</td>
<td>1.14.10</td>
<td>1.14.13</td>
<td>[Kubernetes DNS 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/dns/releases/tag/1.14.13)를 참조하십시오.</td>
</tr>
<tr>
<td>신뢰할 수 있는 컴퓨팅 에이전트</td>
<td>a02f765</td>
<td>e132aa4</td>
<td>[CVE-2017-12447 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-12447)의 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>Ubuntu 16.04 커널</td>
<td>4.4.0-143-generic</td>
<td>4.4.0-145-generic</td>
<td>[CVE-2019-9213 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html)의 커널 업데이트로 작업자 노드 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>Ubuntu 18.04 커널</td>
<td>4.15.0-46-generic</td>
<td>4.15.0-47-generic</td>
<td>[CVE-2019-9213 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html)의 커널 업데이트로 작업자 노드 이미지가 업데이트되었습니다.</td>
</tr>
</tbody>
</table>

#### 2019년 4월 1일에 릴리스된 작업자 노드 수정팩 1.10.13_1554에 대한 변경 로그
{: #11013_1554}

다음 표는 작업자 노드 수정팩 1.10.13_1554에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.10.13_1553 이후에 작성된 변경사항">
<caption>버전 1.10.13_1553 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>작업자 노드 리소스 활용</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>해당 컴포넌트의 리소스 소진을 방지하기 위해 kubelet 및 containerd에 대한 메모리 예약을 늘렸습니다. 자세한 정보는 [작업자 노드 리소스 예약](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node)을 참조하십시오.</td>
</tr>
</tbody>
</table>


#### 2019년 3월 26일에 릴리스된 마스터 수정팩 1.10.13_1553에 대한 변경 로그
{: #11118_1553}

다음 표는 마스터 수정팩 1.10.13_1553에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.10.13_1551 이후에 작성된 변경사항">
<caption>버전 1.10.13_1551 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 플러그인</td>
<td>345</td>
<td>346</td>
<td>[CVE-2019-9741 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741)의 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>키 관리 서비스 제공자</td>
<td>166</td>
<td>167</td>
<td>Kubernetes 시크릿을 관리하기 위해 중간 `context deadline exceeded` 및 `timeout` 오류를 수정합니다. 또한 기존 Kubernetes 시크릿이 암호되지 않은 상태로 유지될 수 있는 키 관리 서비스에 대한 업데이트를 수정합니다. 업데이트에는 [CVE-2019-9741 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741)에 대한 수정사항이 포함되어 있습니다.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} 제공자의 로드 밸런서 및 로드 밸런서 모니터</td>
<td>143</td>
<td>146</td>
<td>[CVE-2019-9741 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741)의 이미지가 업데이트되었습니다.</td>
</tr>
</tbody>
</table>

#### 2019년 3월 20일에 릴리스된 1.10.13_1551에 대한 변경 로그
{: #11013_1551}

다음 표는 패치 1.10.13_1551에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.10.13_1548 이후에 작성된 변경사항">
<caption>버전 1.10.13_154 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>클러스터 마스터 HA 프록시 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>클러스터 마스터에 대한 간헐적인 연결 실패를 보다 잘 처리하기 위한 구성이 업데이트되었습니다.</td>
</tr>
<tr>
<td>GPU 디바이스 플러그인 및 설치 프로그램</td>
<td>e32d51c</td>
<td>9ff3fda</td>
<td>[418.43 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.nvidia.com/object/unix.html)에 대한 GPU 드라이버가 업데이트되었습니다. 업데이트에는 [CVE-2019-9741 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9741.html)에 대한 수정사항이 포함되어 있습니다.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 플러그인</td>
<td>344</td>
<td>345</td>
<td>[개인 서비스 엔드포인트](/docs/containers?topic=containers-cs_network_cluster#set-up-private-se)에 대한 지원이 추가되었습니다.</td>
</tr>
<tr>
<td>Kernel</td>
<td>4.4.0-141</td>
<td>4.4.0-143</td>
<td>[CVE-2019-6133 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-6133.html)의 커널 업데이트로 작업자 노드 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>키 관리 서비스 제공자</td>
<td>136</td>
<td>166</td>
<td>[CVE-2018-16890 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16890), [CVE-2019-3822 ![ 외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3822) 및 [CVE-2019-3823 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3823)의 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>신뢰할 수 있는 컴퓨팅 에이전트</td>
<td>5f3d092</td>
<td>a02f765</td>
<td>[CVE-2018-10779 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10779), [CVE-2018-12900 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-12900), [CVE-2018-17000 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-17000), [CVE-2018-19210 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-19210), [CVE-2019-6128 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6128) 및 [CVE-2019-7663 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-7663)의 이미지가 업데이트되었습니다.</td>
</tr>
</tbody>
</table>

#### 2019년 3월 4일에 릴리스된 1.10.13_154에 대한 변경 로그
{: #11013_1548}

다음 표는 패치 1.10.13_1548에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.10.12_1546 이후에 작성된 변경사항">
<caption>버전 1.10.12_1546 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>GPU 디바이스 플러그인 및 설치 프로그램</td>
<td>eb3a259</td>
<td>e32d51c</td>
<td>[CVE-2019-6454 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454)의 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.10.12-252</td>
<td>v1.10.13-288</td>
<td>Kubernetes 1.10.13 릴리스를 지원하도록 업데이트되었습니다. `externalTrafficPolicy`를 `local`로 설정하는 로드 밸런서의 주기적인 연결 문제가 해결되었습니다. 최신 {{site.data.keyword.Bluemix_notm}} 문서 링크를 사용하기 위해 로드 밸런서 이벤트가 업데이트되었습니다.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 플러그인</td>
<td>342</td>
<td>344</td>
<td>이미지에 대한 기본 운영 체제가 Fedora에서 Alpine으로 변경되었습니다. [CVE-2019-6486 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486)의 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>키 관리 서비스 제공자</td>
<td>122</td>
<td>136</td>
<td>클라이언트의 제한시간이 {{site.data.keyword.keymanagementservicefull_notm}}로 증가되었습니다. [CVE-2019-6486 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486)의 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.10.12</td>
<td>v1.10.13</td>
<td>[Kubernetes 릴리스 정보![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.13)를 참조하십시오.</td>
</tr>
<tr>
<td>Kubernetes DNS</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>보다 많은 클러스터 서비스를 처리하기 위해서 Kubernetes DNS 팟(Pod) 메모리 한계가 `170Mi`에서 `400Mi`로 증가되었습니다.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} 제공자의 로드 밸런서 및 로드 밸런서 모니터</td>
<td>132</td>
<td>143</td>
<td>[CVE-2019-6486 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486)의 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>OpenVPN 클라이언트 및 서버</td>
<td>2.4.6-r3-IKS-13</td>
<td>2.4.6-r3-IKS-25</td>
<td>[CVE-2019-1559 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559)의 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>신뢰할 수 있는 컴퓨팅 에이전트</td>
<td>1ea5ad3</td>
<td>5f3d092</td>
<td>[CVE-2019-6454 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454)의 이미지가 업데이트되었습니다.</td>
</tr>
</tbody>
</table>

#### 2019년 2월 27일에 릴리스된 작업자 노드 수정팩 1.10.12_1546에 대한 변경 로그
{: #11012_1546}

다음 표는 작업자 노드 수정팩 1.10.12_1546에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.10.12_1544 이후에 작성된 변경사항">
<caption>버전 1.10.12_1544 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-141</td>
<td>4.4.0-142</td>
<td>[CVE-2018-19407 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-142.168/changelog)의 커널 업데이트로 작업자 노드 이미지가 업데이트되었습니다.</td>
</tr>
</tbody>
</table>

#### 2019년 2월 15일에 릴리스된 작업자 노드 수정팩 1.10.12_1544에 대한 변경 로그
{: #11012_1544}

다음 표는 작업자 노드 수정팩 1.10.12_1544에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.10.12_1543 이후에 작성된 변경사항">
<caption>버전 1.10.12_1543 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Docker</td>
<td>18.06.1-ce</td>
<td>18.06.2-ce</td>
<td>[Docker Community Edition 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/docker/docker-ce/releases/tag/v18.06.2-ce)를 참조하십시오. 업데이트는 [CVE-2019-5736 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5736)를 해결합니다.</td>
</tr>
<tr>
<td>Kubernetes `kubelet` 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>중요한 정적 팟(pod) 제거를 방지하기 위해 `ExperimentalCriticalPodAnnotation` 기능 게이트가 사용으로 설정되었습니다.</td>
</tr>
</tbody>
</table>

#### 2019년 2월 5일에 릴리스된 1.10.12_1543에 대한 변경 로그
{: #11012_1543}

다음 표는 패치 1.10.12_1543에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.10.12_1541 이후에 작성된 변경사항">
<caption>버전 1.10.12_1541 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>etcd</td>
<td>v3.3.1</td>
<td>v3.3.11</td>
<td>[etcd 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/coreos/etcd/releases/v3.3.11)를 참조하십시오. 또한 etcd에 대해 지원되는 암호화 스위트는 이제 고강도의 암호화(128비트 이상)를 사용하는 서브세트로 제한됩니다.</td>
</tr>
<tr>
<td>GPU 디바이스 플러그인 및 설치 프로그램</td>
<td>13fdc0d</td>
<td>eb3a259</td>
<td>[CVE-2019-3462 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) 및 [CVE-2019-6486 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486)의 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 플러그인</td>
<td>338</td>
<td>342</td>
<td>파일 스토리지 플러그인이 다음과 같이 업데이트되었습니다.
<ul><li>[볼륨 토폴로지 인식 스케줄링](/docs/containers?topic=containers-file_storage#file-topology)을 사용한 동적 프로비저닝을 지원합니다.</li>
<li>스토리지가 이미 삭제된 경우, 지속적 볼륨 클레임(PVC) 삭제 오류가 무시됩니다.</li>
<li>실패 메시지 어노테이션을 실패한 PVC에 추가합니다.</li>
<li>스토리지 프로비저너 제어기의 리더 선택 및 재동기 기간 설정을 최적화하고 프로비저닝 제한시간을 30분에서 1시간으로 늘립니다.</li>
<li>프로비저닝을 시작하기 전에 사용자 권한을 확인합니다.</li></ul></td>
</tr>
<tr>
<td>키 관리 서비스 제공자</td>
<td>111</td>
<td>122</td>
<td>Kubernetes 시크릿을 {{site.data.keyword.keymanagementservicefull_notm}}에서 관리하는 경우 일시적인 장애를 방지하기 위해 재시도 로직이 추가되었습니다.</td>
</tr>
<tr>
<td>Kubernetes 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>Kubernetes API 서버 감사 정책 구성이 `cluster-admin` 요청에 대한 메타데이터 로깅 및 워크로드 `create`, `update` 및 `patch` 요청의 요청 본문 로깅을 포함하도록 업데이트되었습니다.</td>
</tr>
<tr>
<td>OpenVPN 클라이언트</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>[CVE-2018-0734 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) 및 [CVE-2018-5407 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407)의 이미지가 업데이트되었습니다. 또한, 이제 configmap이 아닌 시크릿에서 팟(Pod) 구성을 가져옵니다.</td>
</tr>
<tr>
<td>OpenVPN 서버</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>[CVE-2018-0734 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) 및 [CVE-2018-5407 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407)의 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>systemd</td>
<td>230</td>
<td>229</td>
<td>[CVE-2018-16864 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16864)에 대한 보안 패치</td>
</tr>
</tbody>
</table>

#### 2019년 1월 28일에 릴리스된 작업자 노드 수정팩 1.10.12_1541에 대한 변경 로그
{: #11012_1541}

다음 표는 작업자 노드 수정팩 1.10.12_1541에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.10.12_1540 이후에 작성된 변경사항">
<caption>버전 1.10.12_1540 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 패키지</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>[CVE-2019-3462 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) 및 [USN-3863-1 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://usn.ubuntu.com/3863-1)에 대한 `apt`를 포함하여 설치된 Ubuntu 패키지를 업데이트합니다.</td>
</tr>
</tbody>
</table>

#### 2019년 1월 21일에 릴리스된 1.10.12_1540에 대한 변경 로그
{: #11012_1540}

다음 표는 패치 1.10.12_1540에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.10.11_1538 이후에 작성된 변경사항">
<caption>버전 1.10.11_1538 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.10.11-219</td>
<td>v1.10.12-252</td>
<td>Kubernetes 1.10.12 릴리스를 지원하도록 업데이트되었습니다.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.10.11</td>
<td>v1.10.12</td>
<td>[Kubernetes 릴리스 정보![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.12)를 참조하십시오.</td>
</tr>
<tr>
<td>Kubernetes 추가 기능 resizer</td>
<td>1.8.1</td>
<td>1.8.4</td>
<td>[Kubernetes 추가 기능 resizer 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/autoscaler/releases/tag/addon-resizer-1.8.4)를 참조하십시오.</td>
</tr>
<tr>
<td>Kubernetes 대시보드</td>
<td>v1.8.3</td>
<td>v1.10.1</td>
<td>[Kubernetes 대시보드 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/dashboard/releases/tag/v1.10.1)를 참조하십시오. 업데이트는 [CVE-2018-18264 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-18264)를 해결합니다.<br><br>`kubectl proxy`를 통해 대시보드에 액세스할 경우 로그인 페이지의 **건너뛰기** 단추가 제거됩니다. 대신 [**토큰**을 사용하여 로그인](/docs/containers?topic=containers-app#cli_dashboard)하십시오.</td>
</tr>
<tr>
<td>GPU 설치 프로그램</td>
<td>390.12</td>
<td>410.79</td>
<td>설치된 GPU 드라이버가 410.79로 업데이트되었습니다.</td>
</tr>
</tbody>
</table>

#### 2019년 1월 7일에 릴리스된 작업자 노드 수정팩 1.10.11_1538에 대한 변경 로그
{: #11011_1538}

다음 표는 작업자 노드 수정팩 1.10.11_1538에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.10.11_1537 이후에 작성된 변경사항">
<caption>버전 1.10.11_1537 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-139</td>
<td>4.4.0-141</td>
<td>[CVE-2017-5753, CVE-2018-18690 ![외부 링크 아이콘n](../icons/launch-glyph.svg "외부 링크 아이콘")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-141.167/changelog)의 커널 업데이트로 작업자 노드 이미지가 업데이트되었습니다.</td>
</tr>
</tbody>
</table>

#### 2018년 12월 17일에 릴리스된 작업자 노드 수정팩 1.10.11_1537에 대한 변경 로그
{: #11011_1537}

다음 표는 작업자 노드 수정팩 1.10.11_1537에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.10.11_1536 이후에 작성된 변경사항">
<caption>버전 1.10.11_1536 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 패키지</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>설치된 Ubuntu 패키지로 업데이트되었습니다.</td>
</tr>
</tbody>
</table>


#### 2018년 12월 4일에 릴리스된 1.10.11_1536에 대한 변경 로그
{: #11011_1536}

다음 표는 패치 1.10.11_1536에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.10.8_1532 이후에 작성된 변경사항">
<caption>버전 1.10.8_1532 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.2.1</td>
<td>v3.3.1</td>
<td>[Calico 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.projectcalico.org/v3.3/releases/#v331)를 참조하십시오. 업데이트는 [Tigera Technical Advisory TTA-2018-001 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.projectcalico.org/security-bulletins/)을 해결합니다.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.10.8-197</td>
<td>v1.10.11-219</td>
<td>Kubernetes 1.10.11 릴리스를 지원하도록 업데이트되었습니다.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.10.8</td>
<td>v1.10.11</td>
<td>[Kubernetes 릴리스 정보![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.11)를 참조하십시오. 업데이트는 [CVE-2018-1002105 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/kubernetes/issues/71411)를 해결합니다.</td>
</tr>
<tr>
<td>OpenVPN 클라이언트 및 서버</td>
<td>2.4.4-r1-6</td>
<td>2.4.6-r3-IKS-8</td>
<td>[CVE-2018-0732 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732) 및 [CVE-2018-0737 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737)의 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>작업자 노드 리소스 활용</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>해당 컴포넌트의 리소스 소진을 방지하기 위해 kubelet 및 docker에 대한 전용 cgroup이 추가되었습니다. 자세한 정보는 [작업자 노드 리소스 예약](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node)을 참조하십시오.</td>
</tr>
</tbody>
</table>

#### 2018년 11월 27일에 릴리스된 작업자 노드 수정팩 1.10.8_1532에 대한 변경 로그
{: #1108_1532}

다음 표는 작업자 노드 수정팩 1.10.8_1532에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.10.8_1531 이후에 작성된 변경사항">
<caption>버전 1.10.8_1531 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Docker</td>
<td>17.06.2</td>
<td>18.06.1</td>
<td>[Docker 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.docker.com/engine/release-notes/#18061-ce)를 참조하십시오.</td>
</tr>
</tbody>
</table>

#### 2018년 11월 19일에 릴리스된 작업자 노드 수정팩 1.10.8_1531에 대한 변경 로그
{: #1108_1531}

다음 표는 작업자 노드 수정팩 1.10.8_1531에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.10.8_1530 이후에 작성된 변경사항">
<caption>버전 1.10.8_1530 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-137</td>
<td>4.4.0-139</td>
<td>[CVE-2018-7755 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-139.165/changelog)의 커널 업데이트로 작업자 노드 이미지가 업데이트되었습니다.</td>
</tr>
</tbody>
</table>

#### 2018년 11월 7일에 릴리스된 1.10.8_1530에 대한 변경 로그
{: #1108_1530_ha-master}

다음 표는 패치 1.10.8_1530에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.10.8_1528 이후에 작성된 변경사항">
<caption>버전 1.10.8_1528 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>클러스터 마스터</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>고가용성(HA)을 높이도록 클러스터 마스터 구성이 업데이트되었습니다. 클러스터에는 이제 각 마스터가 별도의 실제 호스트에 전개된 고가용성(HA) 구성으로 설정된 3개의 Kubernetes 마스터 복제본이 있습니다. 또한 클러스터가 다중 구역 가능 구역에 있을 경우 마스터가 전체 구역에 분산됩니다.<br>취해야 할 조치는 [고가용성 클러스터 마스터로 업데이트](/docs/containers?topic=containers-cs_versions#ha-masters)를 참조하십시오. 이 준비 조치는 다음과 같은 경우에 적용됩니다.<ul>
<li>방화벽 또는 사용자 정의 Calico 네트워크 정책이 있는 경우</li>
<li>작업자 노드에서 호스트 포트 `2040` 또는 `2041`을 사용 중인 경우</li>
<li>마스터에 대한 클러스터 내부 액세스를 위해 클러스터 마스터 IP 주소를 사용한 경우</li>
<li>Calico 정책을 작성하기 위해 Calico API 또는 CLI(`calicoctl`)를 호출하는 자동화가 있는 경우</li>
<li>마스터에 대한 팟(Pod) 유출 액세스를 제어하기 위해 Kubernetes 또는 Calico 네트워크 정책을 사용하는 경우</li></ul></td>
</tr>
<tr>
<td>클러스터 마스터 HA 프록시</td>
<td>해당사항 없음</td>
<td>1.8.12-alpine</td>
<td>각 작업자 노드 클라이언트가 사용 가능한 HA 마스터 복제본으로 요청을 라우팅할 수 있도록 모든 작업자 노드에서 클라이언트측 로드 밸런싱을 위해 `ibm-master-proxy-*` 팟(Pod)이 추가되었습니다.</td>
</tr>
<tr>
<td>etcd</td>
<td>v3.2.18</td>
<td>v3.3.1</td>
<td>[etcd 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/coreos/etcd/releases/v3.3.1)를 참조하십시오.</td>
</tr>
<tr>
<td>etcd에서 데이터 암호화</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>이전에는 etcd 데이터가 마스터의 NFS 파일 스토리지 인스턴스에 저장되어 저장 상태로 암호화되었습니다. 이제 etcd 데이터는 마스터의 로컬 디스크에 저장되고, {{site.data.keyword.cos_full_notm}}에 백업됩니다. 데이터는 {{site.data.keyword.cos_full_notm}}로 전송 중 및 저장 중에 암호화됩니다. 그러나 마스터의 로컬 디스크에 있는 etcd 데이터는 암호화되지 않습니다. 마스터의 로컬 etcd 데이터를 암호화하려는 경우 [클러스터에서 {{site.data.keyword.keymanagementservicelong_notm}}를 사용으로 설정](/docs/containers?topic=containers-encryption#keyprotect)하십시오.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.10.8-172</td>
<td>v1.10.8-197</td>
<td>로드 밸런서 서비스가 배치되는 VLAN을 지정하기 위해 `service.kubernetes.io/ibm-load-balancer-cloud-provider-vlan` 어노테이션이 추가되었습니다. 클러스터에서 사용 가능한 VLAN을 보려면 `ibmcloud ks vlans --zone <zone>`을 실행하십시오.</td>
</tr>
<tr>
<td>TPM 사용 커널</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>신뢰할 수 있는 컴퓨팅의 TPM 칩이 있는 베어메탈 작업자 노드는 신뢰가 사용으로 설정될 때까지 기본 Ubuntu 커널을 사용합니다. 기존 클러스터에서 [신뢰를 사용으로 설정](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable)하는 경우에는 TPM 칩이 있는 기존 베어메탈 작업자 노드를 [다시 로드](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)해야 합니다. 베어메탈 작업자 노드에 TPM 칩이 있는지 확인하려면 `ibmcloud ks machine-types --zone` [명령](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_machine_types)을 실행한 후에 **Trustable** 필드를 검토하십시오.</td>
</tr>
</tbody>
</table>

#### 2018년 10월 26일에 릴리스된 작업자 노드 수정팩 1.10.8_1528에 대한 변경 로그
{: #1108_1528}

다음 표는 작업자 노드 수정팩 1.10.8_1528에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.10.8_1527 이후에 작성된 변경사항">
<caption>버전 1.10.8_1527 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>OS 인터럽트 처리</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>인터럽트 요청(IRQ) 시스템 디먼이 보다 성능 기준에 맞는 인터럽트 핸들러로 대체되었습니다.</td>
</tr>
</tbody>
</table>

#### 2018년 10월 15일에 릴리스된 마스터 수정팩 1.10.8_1527에 대한 변경 로그
{: #1108_1527}

다음 표는 마스터 수정팩 1.10.8_1527에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.10.8_1524 이후에 변경된 사항">
<caption>버전 1.10.8_1524 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>노드 실패를 더 잘 처리하기 위해 `calico-node` 컨테이너 준비 상태 프로브가 수정되었습니다.</td>
</tr>
<tr>
<td>클러스터 업데이트</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>마스터가 지원되지 않는 버전에서 업데이트되는 경우 발생하는 클러스터 추가 기능 업데이트 관련 문제점이 수정되었습니다.</td>
</tr>
</tbody>
</table>

#### 2018년 10월 10일에 릴리스된 작업자 노드 수정팩 1.10.8_1525에 대한 변경 로그
{: #1108_1525}

다음 표는 작업자 노드 수정팩 1.10.8_1525에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.10.8_1524 이후에 변경된 사항">
<caption>버전 1.10.8_1524 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-133</td>
<td>4.4.0-137</td>
<td>[CVE-2018-14633, CVE-2018-17182 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-137.163/changelog)의 커널 업데이트로 작업자 노드 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>비활성 세션 제한시간</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>규제 준수를 위해 비활성 세션 제한시간이 5분으로 설정되었습니다.</td>
</tr>
</tbody>
</table>


#### 2018년 10월 2일에 릴리스된 1.10.8_1524에 대한 변경 로그
{: #1108_1524}

다음 표는 패치 1.10.8_1524에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.10.7_1520 이후에 변경된 사항">
<caption>버전 1.10.7_1520 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>키 관리 서비스 제공자</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>{{site.data.keyword.keymanagementservicefull}}를 지원하기 위해 클러스터에서 Kubernetes 키 관리 서비스(KMS) 제공자를 사용하는 기능이 추가되었습니다. [클러스터에서 {{site.data.keyword.keymanagementserviceshort}}를 사용으로 설정](/docs/containers?topic=containers-encryption#keyprotect)하면 모든 Kubernetes secret이 암호화됩니다.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.10.7</td>
<td>v1.10.8</td>
<td>[Kubernetes 릴리스 정보![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.8)를 참조하십시오.</td>
</tr>
<tr>
<td>Kubernetes DNS Autoscaler</td>
<td>1.1.2-r2</td>
<td>1.2.0</td>
<td>[Kubernetes DNS Autoscaler 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes-incubator/cluster-proportional-autoscaler/releases/tag/1.2.0)를 참조하십시오.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.10.7-146</td>
<td>v1.10.8-172</td>
<td>Kubernetes 1.10.8 릴리스를 지원하도록 업데이트되었습니다. 로드 밸런서 오류 메시지의 문서 링크 또한 업데이트되었습니다.</td>
</tr>
<tr>
<td>IBM 파일 스토리지 클래스</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>작업자 노드에서 제공하는 기본값을 사용하기 위해 IBM 파일 스토리지 클래스의 `mountOptions`가 제거되었습니다. IBM 파일 스토리지 클래스에서 중복 `reclaimPolicy` 매개변수가 제거되었습니다.<br><br>
또한 이제 클러스터 마스터를 업데이트할 때 기본 IBM 파일 스토리지 클래스가 변경되지 않습니다. 기본 스토리지 클래스를 변경하려면 `kubectl patch storageclass <storageclass> -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'`를 실행하고 `<storageclass>`를 스토리지 클래스의 이름으로 대체하십시오.</td>
</tr>
</tbody>
</table>

#### 2018년 9월 20일에 릴리스된 작업자 노드 수정팩 1.10.7_1521에 대한 변경 로그
{: #1107_1521}

다음 표는 작업자 노드 수정팩 1.10.7_1521에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.10.7_1520 이후에 변경된 사항">
<caption>버전 1.10.7_1520 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>로그 순환</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>90일 내에 다시 로드되거나 업데이트되지 않은 작업자 노드에 대해 `logrotate`가 실패하는 것을 방지하기 위해 `cronjobs` 대신 `systemd` 타이머를 사용하도록 전환되었습니다. **참고**: 모든 이전 부 릴리스 버전에서는 로그가 순환되지 않으므로 cron 작업이 실패한 후 기본 디스크가 채워집니다. cron 작업은 작업자 노드가 업데이트되거나 다시 로드되지 않고 90일 이상 활성 상태를 유지하면 실패합니다. 로그가 기본 디스크를 모두 채우면 작업자 노드가 실패 상태로 전환됩니다. 이 작업자 노드는 `ibmcloud ks worker-reload` [명령](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) 또는 `ibmcloud ks worker-update` [명령](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update)을 사용하여 수정할 수 있습니다.</td>
</tr>
<tr>
<td>작업자 노드 런타임 컴포넌트(`kubelet`, `kube-proxy`, `docker`)</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>런타임 컴포넌트의 기본 디스크에 대한 종속성이 제거되었습니다. 이 개선사항은 기본 디스크가 가득 찬 경우 작업자 노드가 실패하지 않도록 합니다.</td>
</tr>
<tr>
<td>루트 비밀번호 만료</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>규제 준수를 위해 작업자 노드의 루트 비밀번호는 90일이 지나면 만료됩니다. 자동화 도구가 작업자 노드에 루트로 로그인해야 하거나 루트로서 실행되는 cron 작업에 의존하는 경우에는 작업자 노드에 로그인한 후 `chage -M -1 root`를 실행하여 비밀번호 만료를 사용 안함으로 설정할 수 있습니다. **참고**: 루트로서 실행하는 것 또는 비밀번호 만료를 제거하는 것을 금지하는 보안 규제가 있는 경우에는 만료를 사용 안함으로 설정하지 마십시오. 대신 작업자 노드를 90일마다 [업데이트](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update)하거나 [다시 로드](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)할 수 있습니다.</td>
</tr>
<tr>
<td>systemd</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>주기적으로 임시 마운트 장치를 정리하여 이들이 바인드 해제되지 않도록 합니다. 이 조치는 [Kubernetes 문제 57345 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/kubernetes/issues/57345)를 해결합니다.</td>
</tr>
<tr>
<td>Docker</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>`172.17.0.0/16` IP 범위가 사설 라우트에 대해 사용될 수 있도록 기본 Docker 브릿지가 사용 안함으로 설정되었습니다. 호스트에서 직접 `docker` 명령을 실행하거나 Docker 소켓을 마운트하는 팟(Pod)을 사용하여 작업자 노드에 Docker 컨테이너를 빌드하는 경우에는 다음 선택사항 중 하나를 선택하십시오.<ul><li>컨테이너를 빌드할 때 외부 네트워크 연결을 보장하려면 `docker build . --network host`를 실행하십시오.</li>
<li>컨테이너를 빌드할 때 사용할 네트워크를 명시적으로 작성하려면 `docker network create`를 실행한 후 이 네트워크를 사용하십시오.</li></ul>
**참고**: Docker 소켓에 대해 종속성이 있거나 Docker에 대한 직접적인 종속성이 있습니까? 클러스터가 Kubernetes 1.11 이상의 실행을 준비할 수 있도록 [컨테이너 런타임으로서 `docker`가 아닌 `containerd`에 대해 업데이트](/docs/containers?topic=containers-cs_versions#containerd)하십시오.</td>
</tr>
</tbody>
</table>

#### 2018년 9월 4일에 릴리스된 1.10.7_1520에 대한 변경 로그
{: #1107_1520}

다음 표는 패치 1.10.7_1520에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.10.5_1519 이후에 작성된 변경사항">
<caption>버전 1.10.5_1519 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.1.3</td>
<td>v3.2.1</td>
<td>Calico [릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.projectcalico.org/v3.2/releases/#v321)를 참조하십시오.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.10.5-118</td>
<td>v1.10.7-146</td>
<td>Kubernetes 1.10.7 릴리스를 지원하도록 업데이트되었습니다. 또한 `externalTrafficPolicy`가 `local`로 설정된 로드 밸런서 서비스에 대한 업데이트 처리를 개선하도록 클라우드 제공자 구성이 변경되었습니다.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 플러그인</td>
<td>334</td>
<td>338</td>
<td>incubator 버전이 1.8로 업데이트되었습니다. 파일 스토리지가 사용자가 선택한 특정 구역으로 프로비저닝됩니다. 다중 구역 클러스터를 사용 중이며 지역 및 구역 레이블을 추가해야 하는 경우가 아니면 기존의 (정적) PV 인스턴스 레이블을 업데이트할 수 없습니다.<br><br> IBM 제공 파일 스토리지 클래스의 마운트 옵션에서 기본 NFS 버전이 제거되었습니다. 호스트의 운영 체제가 이제 IBM Cloud 인프라(SoftLayer) NFS 서버와 NFS 버전을 조정합니다. 특정 NFS 버전을 수동으로 설정하거나 호스트의 운영 체제에 의해 조정된 PV의 NFS 버전을 변경하려면 [기본 NFS 버전 변경](/docs/containers?topic=containers-file_storage#nfs_version_class)을 참조하십시오.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.10.5</td>
<td>v1.10.7</td>
<td>Kubernetes [릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.7)를 참조하십시오.</td>
</tr>
<tr>
<td>Kubernetes Heapster 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>`heapster-nanny` 컨테이너에 대한 리소스 한계가 늘어났습니다.</td>
</tr>
</tbody>
</table>

#### 2018년 8월 23일에 릴리스된 작업자 노드 수정팩 1.10.5_1519에 대한 변경 로그
{: #1105_1519}

다음 표는 작업자 노드 수정팩 1.10.5_1519에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.10.5_1518 이후에 작성된 변경사항">
<caption>버전 1.10.5_1518 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>`systemd`</td>
<td>229</td>
<td>230</td>
<td>`cgroup` 유출을 정정하도록 `systemd`가 업데이트되었습니다.</td>
</tr>
<tr>
<td>Kernel</td>
<td>4.4.0-127</td>
<td>4.4.0-133</td>
<td>[CVE-2018-3620,CVE-2018-3646 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://usn.ubuntu.com/3741-1/)의 커널 업데이트로 작업자 노드 이미지가 업데이트되었습니다.</td>
</tr>
</tbody>
</table>


#### 2018년 8월 13일에 릴리스된 작업자 노드 수정팩 1.10.5_1518에 대한 변경 로그
{: #1105_1518}

다음 표는 작업자 노드 수정팩 1.10.5_1518에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.10.5_1517 이후에 작성된 변경사항">
<caption>버전 1.10.5_1517 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 패키지</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>설치된 Ubuntu 패키지로 업데이트되었습니다.</td>
</tr>
</tbody>
</table>

#### 2018년 7월 27일에 릴리스된 1.10.5_1517에 대한 변경 로그
{: #1105_1517}

다음 표는 패치 1.10.5_1517에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.10.3_1514 이후에 작성된 변경사항">
<caption>버전 1.10.3_1514 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.1.1</td>
<td>v3.1.3</td>
<td>Calico [릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.projectcalico.org/v3.1/releases/#v313)를 참조하십시오.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.10.3-85</td>
<td>v1.10.5-118</td>
<td>Kubernetes 1.10.5 릴리스를 지원하도록 업데이트되었습니다. 또한 LoadBalancer 서비스 `create failure` 이벤트에 이제 포터블 서브넷 오류가 포함되어 있습니다.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 플러그인</td>
<td>320</td>
<td>334</td>
<td>지속적 볼륨 작성에 대한 제한시간이 15분에서 30분으로 증가되었습니다. 기본 비용 청구 유형이 `hourly`로 변경되었습니다. 사전 정의된 스토리지 클래스에 마운트 옵션이 추가되었습니다. IBM Cloud 인프라(SoftLayer) 계정의 NFS 파일 스토리지 인스턴스에서 **참고** 필드가 JSON 형식으로 변경되었으며 PV가 배치된 Kubernetes 네임스페이스가 추가되었습니다. 다중 구역 클러스터를 지원하기 위해 지속적 볼륨에 구역 및 지역 레이블이 추가되었습니다.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.10.3</td>
<td>v1.10.5</td>
<td>Kubernetes [릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.5)를 참조하십시오.</td>
</tr>
<tr>
<td>Kernel</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>고성능 네트워킹 워크로드에 대한 작업자 노드 네트워크 설정이 다소 개선되었습니다.</td>
</tr>
<tr>
<td>OpenVPN 클라이언트</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>`kube-system` 네임스페이스에서 실행되는 OpenVPN 클라이언트 `vpn` 배치가 이제 Kubernetes `addon-manager`에 의해 관리됩니다.</td>
</tr>
</tbody>
</table>

#### 2018년 7월 3일에 릴리스된 작업자 노드 수정팩 1.10.3_1514에 대한 변경 로그
{: #1103_1514}

다음 표는 작업자 노드 수정팩 1.10.3_1514에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.10.3_1513 이후에 작성된 변경사항">
<caption>버전 1.10.3_1513 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>고성능 네트워킹 워크로드에 대해 최적화된 `sysctl`.</td>
</tr>
</tbody>
</table>


#### 2018년 6월 21일에 릴리스된 작업자 노드 수정팩 1.10.3_1513에 대한 변경 로그
{: #1103_1513}

다음 표는 작업자 노드 수정팩 1.10.3_1513에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.10.3_1512 이후에 작성된 변경사항">
<caption>버전 1.10.3_1512 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Docker</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>암호화되지 않은 머신 유형의 경우, 보조 디스크는 작업자 노드를 다시 로드하거나 업데이트할 때 새 파일 시스템을 가져오면 정리됩니다.</td>
</tr>
</tbody>
</table>

#### 2018년 6월 12일에 릴리스된 1.10.3_1512에 대한 변경 로그
{: #1103_1512}

다음 표는 패치 1.10.3_1512에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.10.1_1510 이후에 작성된 변경사항">
<caption>버전 1.10.1_1510 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.10.1</td>
<td>v1.10.3</td>
<td>Kubernetes [릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.3)를 참조하십시오.</td>
</tr>
<tr>
<td>Kubernetes 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>클러스터의 Kubernetes API 서버에 대한 `--enable-admission-plugins` 옵션에 `PodSecurityPolicy`가 추가되었으며 팟(Pod) 보안 정책을 지원하도록 클러스터가 구성되었습니다. 자세한 정보는 [팟(Pod) 보안 정책 구성](/docs/containers?topic=containers-psp)을 참조하십시오.</td>
</tr>
<tr>
<td>Kubelet 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>`kubelet` HTTPS 엔드포인트에 대한 인증을 위한 API 베어러 및 서비스 계정 토큰을 지원하기 위해 `--authentication-token-webhook` 옵션이 사용됩니다.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.10.1-52</td>
<td>v1.10.3-85</td>
<td>Kubernetes 1.10.3 릴리스를 지원하도록 업데이트되었습니다.</td>
</tr>
<tr>
<td>OpenVPN 클라이언트</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>`kube-system` 네임스페이스에서 실행되는 OpenVPN 클라이언트 `vpn` 배치에 `livenessProbe`가 추가되었습니다.</td>
</tr>
<tr>
<td>커널 업데이트</td>
<td>4.4.0-116</td>
<td>4.4.0-127</td>
<td>[CVE-2018-3639 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-3639)에 대한 커널 업데이트의 새 작업자 노드 이미지.</td>
</tr>
</tbody>
</table>



#### 2018년 5월 18일에 릴리스된 작업자 노드 수정팩 1.10.1_1510에 대한 변경 로그
{: #1101_1510}

다음 표는 작업자 노드 수정팩 1.10.1_1510에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.10.1_1509 이후에 작성된 변경사항">
<caption>버전 1.10.1_1509 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>블록 스토리지 플러그인을 사용한 경우 발생한 버그를 해결하기 위한 수정사항입니다.</td>
</tr>
</tbody>
</table>

#### 2018년 5월 16일에 릴리스된 작업자 노드 수정팩 1.10.1_1509에 대한 변경 로그
{: #1101_1509}

다음 표는 작업자 노드 수정팩 1.10.1_1509에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.10.1_1508 이후에 작성된 변경사항">
<caption>버전 1.10.1_1508 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>`kubelet` 루트 디렉토리에 저장하는 데이터가 이제 작업자 노드 머신의 더 큰 보조 디스크에 저장됩니다.</td>
</tr>
</tbody>
</table>

#### 2018년 5월 1일에 릴리스된 1.10.1_1508에 대한 변경 로그
{: #1101_1508}

다음 표는 패치 1.10.1_1508에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.9.7_1510 이후에 작성된 변경사항">
<caption>버전 1.9.7_1510 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v2.6.5</td>
<td>v3.1.1</td>
<td>Calico [릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.projectcalico.org/v3.1/releases/#v311)를 참조하십시오.</td>
</tr>
<tr>
<td>Kubernetes Heapster</td>
<td>v1.5.0</td>
<td>v1.5.2</td>
<td>Kubernetes Heapster [릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/heapster/releases/tag/v1.5.2)를 참조하십시오.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.9.7</td>
<td>v1.10.1</td>
<td>Kubernetes [릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.1)를 참조하십시오.</td>
</tr>
<tr>
<td>Kubernetes 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>클러스터의 Kubernetes API 서버에 대한 <code>--enable-admission-plugins</code> 옵션에 <code>StorageObjectInUseProtection</code>이 추가되었습니다.</td>
</tr>
<tr>
<td>Kubernetes DNS</td>
<td>1.14.8</td>
<td>1.14.10</td>
<td>Kubernetes DNS [릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/dns/releases/tag/1.14.10)를 참조하십시오.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.9.7-102</td>
<td>v1.10.1-52</td>
<td>Kubernetes 1.10 릴리스를 지원하도록 업데이트되었습니다.</td>
</tr>
<tr>
<td>GPU 지원</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>이제 스케줄링 및 실행을 위해 [그래픽 처리 장치(GPU) 컨테이너 워크로드](/docs/containers?topic=containers-app#gpu_app)에 대한 지원을 사용할 수 있습니다. 사용 가능한 GPU 머신 유형의 목록은 [작업자 노드에 대한 하드웨어](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)를 참조하십시오. 자세한 정보는 [GPU 스케줄 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/tasks/manage-gpus/scheduling-gpus/)에 대한 Kubernetes 문서를 참조하십시오.</td>
</tr>
</tbody>
</table>

<br />


### 버전 1.9 변경 로그(2018년 12월 27일 현재 지원되지 않음)
{: #19_changelog}

버전 1.9 변경 로그를 검토하십시오.
{: shortdesc}

*   [2019년 12월 17일에 릴리스된 작업자 노드 수정팩 1.9.11_1539에 대한 변경 로그](#1911_1539)
*   [2018년 12월 4일에 릴리스된 작업자 노드 수정팩 1.9.11_1538에 대한 변경 로그](#1911_1538)
*   [2018년 11월 27일에 릴리스된 작업자 노드 수정팩 1.9.11_1537에 대한 변경 로그](#1911_1537)
*   [2018년 11월 19일에 릴리스된 1.9.11_1536에 대한 변경 로그](#1911_1536)
*   [2018년 11월 7일에 릴리스된 작업자 노드 수정팩 1.9.10_1532에 대한 변경 로그](#1910_1532)
*   [2018년 10월 26일에 릴리스된 작업자 노드 수정팩 1.9.10_1531에 대한 변경 로그](#1910_1531)
*   [2018년 10월 15일에 릴리스된 마스터 수정팩 1.9.10_1530에 대한 변경 로그](#1910_1530)
*   [2018년 10월 10일에 릴리스된 작업자 노드 수정팩 1.9.10_1528에 대한 변경 로그](#1910_1528)
*   [2018년 10월 2일에 릴리스된 1.9.10_1527에 대한 변경 로그](#1910_1527)
*   [2018년 9월 20일에 릴리스된 작업자 노드 수정팩 1.9.10_1524에 대한 변경 로그](#1910_1524)
*   [2018년 9월 4일에 릴리스된 1.9.10_1523에 대한 변경 로그](#1910_1523)
*   [2018년 8월 23일에 릴리스된 작업자 노드 수정팩 1.9.9_1522에 대한 변경 로그](#199_1522)
*   [2018년 8월 13일에 릴리스된 작업자 노드 수정팩 1.9.9_1521에 대한 변경 로그](#199_1521)
*   [2018년 7월 27일에 릴리스된 1.9.9_1520에 대한 변경 로그](#199_1520)
*   [2018년 7월 3일에 릴리스된 작업자 노드 수정팩 1.9.8_1517에 대한 변경 로그](#198_1517)
*   [2018년 6월 21일에 릴리스된 작업자 노드 수정팩 1.9.8_1516에 대한 변경 로그](#198_1516)
*   [2018년 6월 19일에 릴리스된 1.9.8_1515에 대한 변경 로그](#198_1515)
*   [2018년 6월 11일에 릴리스된 작업자 노드 수정팩 1.9.7_1513에 대한 변경 로그](#197_1513)
*   [2018년 5월 18일에 릴리스된 작업자 노드 수정팩 1.9.7_1512에 대한 변경 로그](#197_1512)
*   [2018년 5월 16일에 릴리스된 작업자 노드 수정팩 1.9.7_1511에 대한 변경 로그](#197_1511)
*   [2018년 4월 30일에 릴리스된 1.9.7_1510에 대한 변경 로그](#197_1510)

#### 2018년 12월 17일에 릴리스된 작업자 노드 수정팩 1.9.11_1539에 대한 변경 로그
{: #1911_1539}

다음 표는 작업자 노드 수정팩 1.9.11_1539에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.9.11_1538 이후에 작성된 변경사항">
<caption>버전 1.9.11_1538 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 패키지</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>설치된 Ubuntu 패키지로 업데이트되었습니다.</td>
</tr>
</tbody>
</table>

#### 2018년 12월 4일에 릴리스된 작업자 노드 수정팩 1.9.11_1538에 대한 변경 로그
{: #1911_1538}

다음 표는 작업자 노드 수정팩 1.9.11_1538에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.9.11_1537 이후에 작성된 변경사항">
<caption>버전 1.9.11_1537 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>작업자 노드 리소스 활용</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>해당 컴포넌트의 리소스 소진을 방지하기 위해 kubelet 및 docker에 대한 전용 cgroup이 추가되었습니다. 자세한 정보는 [작업자 노드 리소스 예약](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node)을 참조하십시오.</td>
</tr>
</tbody>
</table>

#### 2018년 11월 27일에 릴리스된 작업자 노드 수정팩 1.9.11_1537에 대한 변경 로그
{: #1911_1537}

다음 표는 작업자 노드 수정팩 1.9.11_1537에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.9.11_1536 이후에 작성된 변경사항">
<caption>버전 1.9.11_1536 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Docker</td>
<td>17.06.2</td>
<td>18.06.1</td>
<td>[Docker 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.docker.com/engine/release-notes/#18061-ce)를 참조하십시오.</td>
</tr>
</tbody>
</table>

#### 2018년 11월 19일에 릴리스된 1.9.11_1536에 대한 변경 로그
{: #1911_1536}

다음 표는 패치 1.9.11_1536에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.9.10_1532 이후에 작성된 변경사항">
<caption>버전 1.9.10_1532 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v2.6.5</td>
<td>v2.6.12</td>
<td>[Calico 릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.projectcalico.org/v2.6/releases/#v2612)를 참조하십시오. 업데이트는 [Tigera Technical Advisory TTA-2018-001 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.projectcalico.org/security-bulletins/)을 해결합니다.</td>
</tr>
<tr>
<td>Kernel</td>
<td>4.4.0-137</td>
<td>4.4.0-139</td>
<td>[CVE-2018-7755 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-139.165/changelog)의 커널 업데이트로 작업자 노드 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.9.10</td>
<td>v1.9.11</td>
<td>[Kubernetes 릴리스 정보![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.11)를 참조하십시오.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}}</td>
<td>v1.9.10-219</td>
<td>v1.9.11-249</td>
<td>Kubernetes 1.9.11 릴리스를 지원하도록 업데이트되었습니다.</td>
</tr>
<tr>
<td>OpenVPN 클라이언트 및 서버</td>
<td>2.4.4-r2</td>
<td>2.4.6-r3-IKS-8</td>
<td>[CVE-2018-0732 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732) 및 [CVE-2018-0737 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737)의 이미지가 업데이트되었습니다.</td>
</tr>
</tbody>
</table>

#### 2018년 11월 7일에 릴리스된 작업자 노드 수정팩 1.9.10_1532에 대한 변경 로그
{: #1910_1532}

다음 표는 작업자 노드 수정팩 1.9.11_1532에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.9.10_1531 이후에 작성된 변경사항">
<caption>버전 1.9.10_1531 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>TPM 사용 커널</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>신뢰할 수 있는 컴퓨팅의 TPM 칩이 있는 베어메탈 작업자 노드는 신뢰가 사용으로 설정될 때까지 기본 Ubuntu 커널을 사용합니다. 기존 클러스터에서 [신뢰를 사용으로 설정](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable)하는 경우에는 TPM 칩이 있는 기존 베어메탈 작업자 노드를 [다시 로드](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)해야 합니다. 베어메탈 작업자 노드에 TPM 칩이 있는지 확인하려면 `ibmcloud ks machine-types --zone` [명령](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_machine_types)을 실행한 후에 **Trustable** 필드를 검토하십시오.</td>
</tr>
</tbody>
</table>

#### 2018년 10월 26일에 릴리스된 작업자 노드 수정팩 1.9.10_1531에 대한 변경 로그
{: #1910_1531}

다음 표는 작업자 노드 수정팩 1.9.10_1531에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.9.10_1530 이후에 작성된 변경사항">
<caption>버전 1.9.10_1530 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>OS 인터럽트 처리</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>인터럽트 요청(IRQ) 시스템 디먼이 보다 성능 기준에 맞는 인터럽트 핸들러로 대체되었습니다.</td>
</tr>
</tbody>
</table>

#### 2018년 10월 15일에 릴리스된 마스터 수정팩 1.9.10_1530에 대한 변경 로그
{: #1910_1530}

다음 표는 작업자 노드 수정팩 1.9.10_1530에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.9.10_1527 이후에 변경된 사항">
<caption>버전 1.9.10_1527 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>클러스터 업데이트</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>마스터가 지원되지 않는 버전에서 업데이트되는 경우 발생하는 클러스터 추가 기능 업데이트 관련 문제점이 수정되었습니다.</td>
</tr>
</tbody>
</table>

#### 2018년 10월 10일에 릴리스된 작업자 노드 수정팩 1.9.10_1528에 대한 변경 로그
{: #1910_1528}

다음 표는 작업자 노드 수정팩 1.9.10_1528에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.9.10_1527 이후에 변경된 사항">
<caption>버전 1.9.10_1527 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-133</td>
<td>4.4.0-137</td>
<td>[CVE-2018-14633, CVE-2018-17182 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-137.163/changelog)의 커널 업데이트로 작업자 노드 이미지가 업데이트되었습니다.</td>
</tr>
<tr>
<td>비활성 세션 제한시간</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>규제 준수를 위해 비활성 세션 제한시간이 5분으로 설정되었습니다.</td>
</tr>
</tbody>
</table>


#### 2018년 10월 2일에 릴리스된 1.9.10_1527에 대한 변경 로그
{: #1910_1527}

다음 표는 패치 1.9.10_1527에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.9.10_1523 이후에 변경된 사항">
<caption>버전 1.9.10_1523 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.9.10-192</td>
<td>v1.9.10-219</td>
<td>로드 밸런서 오류 메시지의 문서 링크가 업데이트되었습니다.</td>
</tr>
<tr>
<td>IBM 파일 스토리지 클래스</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>작업자 노드에서 제공하는 기본값을 사용하기 위해 IBM 파일 스토리지 클래스의 `mountOptions`가 제거되었습니다. IBM 파일 스토리지 클래스에서 중복 `reclaimPolicy` 매개변수가 제거되었습니다.<br><br>
또한 이제 클러스터 마스터를 업데이트할 때 기본 IBM 파일 스토리지 클래스가 변경되지 않습니다. 기본 스토리지 클래스를 변경하려면 `kubectl patch storageclass <storageclass> -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'`를 실행하고 `<storageclass>`를 스토리지 클래스의 이름으로 대체하십시오.</td>
</tr>
</tbody>
</table>

#### 2018년 9월 20일에 릴리스된 작업자 노드 수정팩 1.9.10_1524에 대한 변경 로그
{: #1910_1524}

다음 표는 작업자 노드 수정팩 1.9.10_1524에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.9.10_1523 이후에 변경된 사항">
<caption>버전 1.9.10_1523 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>로그 순환</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>90일 내에 다시 로드되거나 업데이트되지 않은 작업자 노드에 대해 `logrotate`가 실패하는 것을 방지하기 위해 `cronjobs` 대신 `systemd` 타이머를 사용하도록 전환되었습니다. **참고**: 모든 이전 부 릴리스 버전에서는 로그가 순환되지 않으므로 cron 작업이 실패한 후 기본 디스크가 채워집니다. cron 작업은 작업자 노드가 업데이트되거나 다시 로드되지 않고 90일 이상 활성 상태를 유지하면 실패합니다. 로그가 기본 디스크를 모두 채우면 작업자 노드가 실패 상태로 전환됩니다. 이 작업자 노드는 `ibmcloud ks worker-reload` [명령](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) 또는 `ibmcloud ks worker-update` [명령](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update)을 사용하여 수정할 수 있습니다.</td>
</tr>
<tr>
<td>작업자 노드 런타임 컴포넌트(`kubelet`, `kube-proxy`, `docker`)</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>런타임 컴포넌트의 기본 디스크에 대한 종속성이 제거되었습니다. 이 개선사항은 기본 디스크가 가득 찬 경우 작업자 노드가 실패하지 않도록 합니다.</td>
</tr>
<tr>
<td>루트 비밀번호 만료</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>규제 준수를 위해 작업자 노드의 루트 비밀번호는 90일이 지나면 만료됩니다. 자동화 도구가 작업자 노드에 루트로 로그인해야 하거나 루트로서 실행되는 cron 작업에 의존하는 경우에는 작업자 노드에 로그인한 후 `chage -M -1 root`를 실행하여 비밀번호 만료를 사용 안함으로 설정할 수 있습니다. **참고**: 루트로서 실행하는 것 또는 비밀번호 만료를 제거하는 것을 금지하는 보안 규제가 있는 경우에는 만료를 사용 안함으로 설정하지 마십시오. 대신 작업자 노드를 90일마다 [업데이트](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update)하거나 [다시 로드](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)할 수 있습니다.</td>
</tr>
<tr>
<td>systemd</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>주기적으로 임시 마운트 장치를 정리하여 이들이 바인드 해제되지 않도록 합니다. 이 조치는 [Kubernetes 문제 57345 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/kubernetes/issues/57345)를 해결합니다.</td>
</tr>
<tr>
<td>Docker</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>`172.17.0.0/16` IP 범위가 사설 라우트에 대해 사용될 수 있도록 기본 Docker 브릿지가 사용 안함으로 설정되었습니다. 호스트에서 직접 `docker` 명령을 실행하거나 Docker 소켓을 마운트하는 팟(Pod)을 사용하여 작업자 노드에 Docker 컨테이너를 빌드하는 경우에는 다음 선택사항 중 하나를 선택하십시오.<ul><li>컨테이너를 빌드할 때 외부 네트워크 연결을 보장하려면 `docker build . --network host`를 실행하십시오.</li>
<li>컨테이너를 빌드할 때 사용할 네트워크를 명시적으로 작성하려면 `docker network create`를 실행한 후 이 네트워크를 사용하십시오.</li></ul>
**참고**: Docker 소켓에 대해 종속성이 있거나 Docker에 대한 직접적인 종속성이 있습니까? 클러스터가 Kubernetes 1.11 이상의 실행을 준비할 수 있도록 [컨테이너 런타임으로서 `docker`가 아닌 `containerd`에 대해 업데이트](/docs/containers?topic=containers-cs_versions#containerd)하십시오.</td>
</tr>
</tbody>
</table>

#### 2018년 9월 4일에 릴리스된 1.9.10_1523에 대한 변경 로그
{: #1910_1523}

다음 표는 패치 1.9.10_1523에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.9.9_1522 이후에 작성된 변경사항">
<caption>버전 1.9.9_1522 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.9.9-167</td>
<td>v1.9.10-192</td>
<td>Kubernetes 1.9.10 릴리스를 지원하도록 업데이트되었습니다. 또한 `externalTrafficPolicy`가 `local`로 설정된 로드 밸런서 서비스에 대한 업데이트 처리를 개선하도록 클라우드 제공자 구성이 변경되었습니다.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 플러그인</td>
<td>334</td>
<td>338</td>
<td>incubator 버전이 1.8로 업데이트되었습니다. 파일 스토리지가 사용자가 선택한 특정 구역으로 프로비저닝됩니다. 다중 구역 클러스터를 사용 중이며 지역 및 구역 레이블을 추가해야 하는 경우가 아니면 기존의 (정적) PV 인스턴스 레이블을 업데이트할 수 없습니다.<br><br>IBM 제공 파일 스토리지 클래스의 마운트 옵션에서 기본 NFS 버전이 제거되었습니다. 호스트의 운영 체제가 이제 IBM Cloud 인프라(SoftLayer) NFS 서버와 NFS 버전을 조정합니다. 특정 NFS 버전을 수동으로 설정하거나 호스트의 운영 체제에 의해 조정된 PV의 NFS 버전을 변경하려면 [기본 NFS 버전 변경](/docs/containers?topic=containers-file_storage#nfs_version_class)을 참조하십시오.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.9.9</td>
<td>v1.9.10</td>
<td>Kubernetes [릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.10)를 참조하십시오.</td>
</tr>
<tr>
<td>Kubernetes Heapster 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>`heapster-nanny` 컨테이너에 대한 리소스 한계가 늘어났습니다.</td>
</tr>
</tbody>
</table>

#### 2018년 8월 23일에 릴리스된 작업자 노드 수정팩 1.9.9_1522에 대한 변경 로그
{: #199_1522}

다음 표는 작업자 노드 수정팩 1.9.9_1522에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.9.9_1521 이후에 작성된 변경사항">
<caption>버전 1.9.9_1521 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>`systemd`</td>
<td>229</td>
<td>230</td>
<td>`cgroup` 유출을 정정하도록 `systemd`가 업데이트되었습니다.</td>
</tr>
<tr>
<td>Kernel</td>
<td>4.4.0-127</td>
<td>4.4.0-133</td>
<td>[CVE-2018-3620,CVE-2018-3646 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://usn.ubuntu.com/3741-1/)의 커널 업데이트로 작업자 노드 이미지가 업데이트되었습니다.</td>
</tr>
</tbody>
</table>


#### 2018년 8월 13일에 릴리스된 작업자 노드 수정팩 1.9.9_1521에 대한 변경 로그
{: #199_1521}

다음 표는 작업자 노드 수정팩 1.9.9_1521에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.9.9_1520 이후에 작성된 변경사항">
<caption>버전 1.9.9_1520 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 패키지</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>설치된 Ubuntu 패키지로 업데이트되었습니다.</td>
</tr>
</tbody>
</table>

#### 2018년 7월 27일에 릴리스된 1.9.9_1520에 대한 변경 로그
{: #199_1520}

다음 표는 패치 1.9.9_1520에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.9.8_1517 이후에 작성된 변경사항">
<caption>버전 1.9.8_1517 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.9.8-141</td>
<td>v1.9.9-167</td>
<td>Kubernetes 1.9.9 릴리스를 지원하도록 업데이트되었습니다. 또한 LoadBalancer 서비스 `create failure` 이벤트에 이제 포터블 서브넷 오류가 포함되어 있습니다.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 플러그인</td>
<td>320</td>
<td>334</td>
<td>지속적 볼륨 작성에 대한 제한시간이 15분에서 30분으로 증가되었습니다. 기본 비용 청구 유형이 `hourly`로 변경되었습니다. 사전 정의된 스토리지 클래스에 마운트 옵션이 추가되었습니다. IBM Cloud 인프라(SoftLayer) 계정의 NFS 파일 스토리지 인스턴스에서 **참고** 필드가 JSON 형식으로 변경되었으며 PV가 배치된 Kubernetes 네임스페이스가 추가되었습니다. 다중 구역 클러스터를 지원하기 위해 지속적 볼륨에 구역 및 지역 레이블이 추가되었습니다.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.9.8</td>
<td>v1.9.9</td>
<td>Kubernetes [릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.9)를 참조하십시오.</td>
</tr>
<tr>
<td>Kernel</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>고성능 네트워킹 워크로드에 대한 작업자 노드 네트워크 설정이 다소 개선되었습니다.</td>
</tr>
<tr>
<td>OpenVPN 클라이언트</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>`kube-system` 네임스페이스에서 실행되는 OpenVPN 클라이언트 `vpn` 배치가 이제 Kubernetes `addon-manager`에 의해 관리됩니다.</td>
</tr>
</tbody>
</table>

#### 2018년 7월 3일에 릴리스된 작업자 노드 수정팩 1.9.8_1517에 대한 변경 로그
{: #198_1517}

다음 표는 작업자 노드 수정팩 1.9.8_1517에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.9.8_1516 이후에 작성된 변경사항">
<caption>버전 1.9.8_1516 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>고성능 네트워킹 워크로드에 대해 최적화된 `sysctl`.</td>
</tr>
</tbody>
</table>


#### 2018년 6월 21일에 릴리스된 작업자 노드 수정팩 1.9.8_1516에 대한 변경 로그
{: #198_1516}

다음 표는 작업자 노드 수정팩 1.9.8_1516에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.9.8_1515 이후에 작성된 변경사항">
<caption>버전 1.9.8_1515 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Docker</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>암호화되지 않은 머신 유형의 경우, 보조 디스크는 작업자 노드를 다시 로드하거나 업데이트할 때 새 파일 시스템을 가져오면 정리됩니다.</td>
</tr>
</tbody>
</table>

#### 2018년 6월 19일에 릴리스된 1.9.8_1515에 대한 변경 로그
{: #198_1515}

다음 표는 패치 1.9.8_1515에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.9.7_1513 이후에 작성된 변경사항">
<caption>버전 1.9.7_1513 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.9.7</td>
<td>v1.9.8</td>
<td>[Kubernetes 릴리스 정보![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.8)를 참조하십시오.</td>
</tr>
<tr>
<td>Kubernetes 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>클러스터의 Kubernetes API 서버에 대한 `--admission-control` 옵션에 `PodSecurityPolicy`가 추가되었으며 팟(Pod) 보안 정책을 지원하도록 클러스터가 구성되었습니다. 자세한 정보는 [팟(Pod) 보안 정책 구성](/docs/containers?topic=containers-psp)을 참조하십시오.</td>
</tr>
<tr>
<td>IBM Cloud 제공자</td>
<td>v1.9.7-102</td>
<td>v1.9.8-141</td>
<td>Kubernetes 1.9.8 릴리스를 지원하도록 업데이트되었습니다.</td>
</tr>
<tr>
<td>OpenVPN 클라이언트</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td><code>kube-system</code> 네임스페이스에서 실행되는 OpenVPN 클라이언트 <code>vpn</code> 배치에 <code>livenessProbe</code>가 추가되었습니다.</td>
</tr>
</tbody>
</table>


#### 2018년 6월 11일에 릴리스된 작업자 노드 수정팩 1.9.7_1513에 대한 변경 로그
{: #197_1513}

다음 표는 작업자 노드 수정팩 1.9.7_1513에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.9.7_1512 이후에 작성된 변경사항">
<caption>버전 1.9.7_1512 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>커널 업데이트</td>
<td>4.4.0-116</td>
<td>4.4.0-127</td>
<td>[CVE-2018-3639 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-3639)에 대한 커널 업데이트의 새 작업자 노드 이미지.</td>
</tr>
</tbody>
</table>

#### 2018년 5월 18일에 릴리스된 작업자 노드 수정팩 1.9.7_1512에 대한 변경 로그
{: #197_1512}

다음 표는 작업자 노드 수정팩 1.9.7_1512에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.9.7_1511 이후에 작성된 변경사항">
<caption>버전 1.9.7_1511 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>블록 스토리지 플러그인을 사용한 경우 발생한 버그를 해결하기 위한 수정사항입니다.</td>
</tr>
</tbody>
</table>

#### 2018년 5월 16일에 릴리스된 작업자 노드 수정팩 1.9.7_1511에 대한 변경 로그
{: #197_1511}

다음 표는 작업자 노드 수정팩 1.9.7_1511에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.9.7_1510 이후에 작성된 변경사항">
<caption>버전 1.9.7_1510 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>`kubelet` 루트 디렉토리에 저장하는 데이터가 이제 작업자 노드 머신의 더 큰 보조 디스크에 저장됩니다.</td>
</tr>
</tbody>
</table>

#### 2018년 4월 30일에 릴리스된 1.9.7_1510에 대한 변경 로그
{: #197_1510}

다음 표는 패치 1.9.7_1510에 포함된 변경사항을 보여줍니다.
{: shortdesc}

<table summary="버전 1.9.3_1506 이후에 작성된 변경사항">
<caption>버전 1.9.3_1506 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.9.3</td>
<td>v1.9.7	</td>
<td><p>[Kubernetes 릴리스 정보![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.7)를 참조하십시오. 이 릴리스에서는 [CVE-2017-1002101 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002101) 및 [CVE-2017-1002102 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002102) 취약성이 해결되었습니다.</p><p><strong>참고</strong>: 이제 `secret`, `configMap`, `downwardAPI` 및 투영된 볼륨은 읽기 전용으로 마운트됩니다. 이전에는 앱이 이러한 볼륨에 데이터를 쓸 수 있었지만 시스템이 데이터를 자동으로 되돌릴 수 있었습니다. 앱이 이전의 안전하지 않은 작동에 의존하는 경우에는 이를 적절히 수정하십시오.</p></td>
</tr>
<tr>
<td>Kubernetes 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>클러스터의 Kubernetes API 서버에 대한 `--runtime-config` 옵션에 <code>admissionregistration.k8s.io/v1alpha1=true</code>가 추가되었습니다.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.9.3-71</td>
<td>v1.9.7-102</td>
<td>`NodePort` 및 `LoadBalancer` 서비스는 이제 `service.spec.externalTrafficPolicy`를 `Local`로 설정하여 [클라이언스 소스 IP를 유지](/docs/containers?topic=containers-loadbalancer#node_affinity_tolerations)하는 것을 지원합니다.</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>이전 클러스터의 [에지 노드](/docs/containers?topic=containers-edge#edge) 결함 허용(toleration) 설정을 수정합니다.</td>
</tr>
</tbody>
</table>

<br />


### 버전 1.8 변경 로그(지원되지 않음)
{: #18_changelog}

버전 1.8 변경 로그를 검토하십시오.
{: shortdesc}

*   [2018년 9월 20일에 릴리스된 작업자 노드 수정팩 1.8.15_1521에 대한 변경 로그](#1815_1521)
*   [2018년 8월 23일에 릴리스된 작업자 노드 수정팩 1.8.15_1520에 대한 변경 로그](#1815_1520)
*   [2018년 8월 13일에 릴리스된 작업자 노드 수정팩 1.8.15_1519에 대한 변경 로그](#1815_1519)
*   [2018년 7월 27일에 릴리스된 1.8.15_1518에 대한 변경 로그](#1815_1518)
*   [2018년 7월 3일에 릴리스된 작업자 노드 수정팩 1.8.13_1516에 대한 변경 로그](#1813_1516)
*   [2018년 6월 21일에 릴리스된 작업자 노드 수정팩 1.8.13_1515에 대한 변경 로그](#1813_1515)
*   [2018년 6월 19일에 릴리스된 1.8.13_1514에 대한 변경 로그](#1813_1514)
*   [2018년 6월 11일에 릴리스된 작업자 노드 수정팩 1.8.11_1512에 대한 변경 로그](#1811_1512)
*   [2018년 5월 18일에 릴리스된 작업자 노드 수정팩 1.8.11_1511에 대한 변경 로그](#1811_1511)
*   [2018년 5월 16일에 릴리스된 작업자 노드 수정팩 1.8.11_1510에 대한 변경 로그](#1811_1510)
*   [2018년 4월 19일에 릴리스된 1.8.11_1509에 대한 변경 로그](#1811_1509)

#### 2018년 9월 20일에 릴리스된 작업자 노드 수정팩 1.8.15_1521에 대한 변경 로그
{: #1815_1521}

<table summary="버전 1.8.15_1520 이후에 변경된 사항">
<caption>버전 1.8.15_1520 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>로그 순환</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>90일 내에 다시 로드되거나 업데이트되지 않은 작업자 노드에 대해 `logrotate`가 실패하는 것을 방지하기 위해 `cronjobs` 대신 `systemd` 타이머를 사용하도록 전환되었습니다. **참고**: 모든 이전 부 릴리스 버전에서는 로그가 순환되지 않으므로 cron 작업이 실패한 후 기본 디스크가 채워집니다. cron 작업은 작업자 노드가 업데이트되거나 다시 로드되지 않고 90일 이상 활성 상태를 유지하면 실패합니다. 로그가 기본 디스크를 모두 채우면 작업자 노드가 실패 상태로 전환됩니다. 이 작업자 노드는 `ibmcloud ks worker-reload` [명령](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) 또는 `ibmcloud ks worker-update` [명령](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update)을 사용하여 수정할 수 있습니다.</td>
</tr>
<tr>
<td>작업자 노드 런타임 컴포넌트(`kubelet`, `kube-proxy`, `docker`)</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>런타임 컴포넌트의 기본 디스크에 대한 종속성이 제거되었습니다. 이 개선사항은 기본 디스크가 가득 찬 경우 작업자 노드가 실패하지 않도록 합니다.</td>
</tr>
<tr>
<td>루트 비밀번호 만료</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>규제 준수를 위해 작업자 노드의 루트 비밀번호는 90일이 지나면 만료됩니다. 자동화 도구가 작업자 노드에 루트로 로그인해야 하거나 루트로서 실행되는 cron 작업에 의존하는 경우에는 작업자 노드에 로그인한 후 `chage -M -1 root`를 실행하여 비밀번호 만료를 사용 안함으로 설정할 수 있습니다. **참고**: 루트로서 실행하는 것 또는 비밀번호 만료를 제거하는 것을 금지하는 보안 규제가 있는 경우에는 만료를 사용 안함으로 설정하지 마십시오. 대신 작업자 노드를 90일마다 [업데이트](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update)하거나 [다시 로드](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)할 수 있습니다.</td>
</tr>
<tr>
<td>systemd</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>주기적으로 임시 마운트 장치를 정리하여 이들이 바인드 해제되지 않도록 합니다. 이 조치는 [Kubernetes 문제 57345 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/kubernetes/issues/57345)를 해결합니다.</td>
</tr>
</tbody>
</table>

#### 2018년 8월 23일에 릴리스된 작업자 노드 수정팩 1.8.15_1520에 대한 변경 로그
{: #1815_1520}

<table summary="버전 1.8.15_1519 이후에 작성된 변경사항">
<caption>버전 1.8.15_1519 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>`systemd`</td>
<td>229</td>
<td>230</td>
<td>`cgroup` 유출을 정정하도록 `systemd`가 업데이트되었습니다.</td>
</tr>
<tr>
<td>Kernel</td>
<td>4.4.0-127</td>
<td>4.4.0-133</td>
<td>[CVE-2018-3620,CVE-2018-3646 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://usn.ubuntu.com/3741-1/)의 커널 업데이트로 작업자 노드 이미지가 업데이트되었습니다.</td>
</tr>
</tbody>
</table>

#### 2018년 8월 13일에 릴리스된 작업자 노드 수정팩 1.8.15_1519에 대한 변경 로그
{: #1815_1519}

<table summary="버전 1.8.15_1518 이후에 작성된 변경사항">
<caption>버전 1.8.15_1518 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 패키지</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>설치된 Ubuntu 패키지로 업데이트되었습니다.</td>
</tr>
</tbody>
</table>

#### 2018년 7월 27일에 릴리스된 1.8.15_1518에 대한 변경 로그
{: #1815_1518}

<table summary="버전 1.8.13_1516 이후에 작성된 변경사항">
<caption>버전 1.8.13_1516 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.8.13-176</td>
<td>v1.8.15-204</td>
<td>Kubernetes 1.8.15 릴리스를 지원하도록 업데이트되었습니다. 또한 LoadBalancer 서비스 `create failure` 이벤트에 이제 포터블 서브넷 오류가 포함되어 있습니다.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 플러그인</td>
<td>320</td>
<td>334</td>
<td>지속적 볼륨 작성에 대한 제한시간이 15분에서 30분으로 증가되었습니다. 기본 비용 청구 유형이 `hourly`로 변경되었습니다. 사전 정의된 스토리지 클래스에 마운트 옵션이 추가되었습니다. IBM Cloud 인프라(SoftLayer) 계정의 NFS 파일 스토리지 인스턴스에서 **참고** 필드가 JSON 형식으로 변경되었으며 PV가 배치된 Kubernetes 네임스페이스가 추가되었습니다. 다중 구역 클러스터를 지원하기 위해 지속적 볼륨에 구역 및 지역 레이블이 추가되었습니다.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.8.13</td>
<td>v1.8.15</td>
<td>Kubernetes [릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/kubernetes/releases/tag/v1.8.15)를 참조하십시오.</td>
</tr>
<tr>
<td>Kernel</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>고성능 네트워킹 워크로드에 대한 작업자 노드 네트워크 설정이 다소 개선되었습니다.</td>
</tr>
<tr>
<td>OpenVPN 클라이언트</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>`kube-system` 네임스페이스에서 실행되는 OpenVPN 클라이언트 `vpn` 배치가 이제 Kubernetes `addon-manager`에 의해 관리됩니다.</td>
</tr>
</tbody>
</table>

#### 2018년 7월 3일에 릴리스된 작업자 노드 수정팩 1.8.13_1516에 대한 변경 로그
{: #1813_1516}

<table summary="버전 1.8.13_1515 이후에 작성된 변경사항">
<caption>버전 1.8.13_1515 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>고성능 네트워킹 워크로드에 대해 최적화된 `sysctl`.</td>
</tr>
</tbody>
</table>


#### 2018년 6월 21일에 릴리스된 작업자 노드 수정팩 1.8.13_1515에 대한 변경 로그
{: #1813_1515}

<table summary="버전 1.8.13_1514 이후에 작성된 변경사항">
<caption>버전 1.8.13_1514 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Docker</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>암호화되지 않은 머신 유형의 경우, 보조 디스크는 작업자 노드를 다시 로드하거나 업데이트할 때 새 파일 시스템을 가져오면 정리됩니다.</td>
</tr>
</tbody>
</table>

#### 2018년 6월 19일에 릴리스된 1.8.13_1514에 대한 변경 로그
{: #1813_1514}

<table summary="버전 1.8.11_1512 이후에 작성된 변경사항">
<caption>버전 1.8.11_1512 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.8.11</td>
<td>v1.8.13</td>
<td>[Kubernetes 릴리스 정보![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/kubernetes/releases/tag/v1.8.13)를 참조하십시오.</td>
</tr>
<tr>
<td>Kubernetes 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>클러스터의 Kubernetes API 서버에 대한 `--admission-control` 옵션에 `PodSecurityPolicy`가 추가되었으며 팟(Pod) 보안 정책을 지원하도록 클러스터가 구성되었습니다. 자세한 정보는 [팟(Pod) 보안 정책 구성](/docs/containers?topic=containers-psp)을 참조하십시오.</td>
</tr>
<tr>
<td>IBM Cloud 제공자</td>
<td>v1.8.11-126</td>
<td>v1.8.13-176</td>
<td>Kubernetes 1.8.13 릴리스를 지원하도록 업데이트되었습니다.</td>
</tr>
<tr>
<td>OpenVPN 클라이언트</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td><code>kube-system</code> 네임스페이스에서 실행되는 OpenVPN 클라이언트 <code>vpn</code> 배치에 <code>livenessProbe</code>가 추가되었습니다.</td>
</tr>
</tbody>
</table>


#### 2018년 6월 11일에 릴리스된 작업자 노드 수정팩 1.8.11_1512에 대한 변경 로그
{: #1811_1512}

<table summary="버전 1.8.11_1511 이후에 작성된 변경사항">
<caption>버전 1.8.11_1511 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>커널 업데이트</td>
<td>4.4.0-116</td>
<td>4.4.0-127</td>
<td>[CVE-2018-3639 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-3639)에 대한 커널 업데이트의 새 작업자 노드 이미지.</td>
</tr>
</tbody>
</table>


#### 2018년 5월 18일에 릴리스된 작업자 노드 수정팩 1.8.11_1511에 대한 변경 로그
{: #1811_1511}

<table summary="버전 1.8.11_1510 이후에 작성된 변경사항">
<caption>버전 1.8.11_1510 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>블록 스토리지 플러그인을 사용한 경우 발생한 버그를 해결하기 위한 수정사항입니다.</td>
</tr>
</tbody>
</table>

#### 2018년 5월 16일에 릴리스된 작업자 노드 수정팩 1.8.11_1510에 대한 변경 로그
{: #1811_1510}

<table summary="버전 1.8.11_1509 이후에 작성된 변경사항">
<caption>버전 1.8.11_1509 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>`kubelet` 루트 디렉토리에 저장하는 데이터가 이제 작업자 노드 머신의 더 큰 보조 디스크에 저장됩니다.</td>
</tr>
</tbody>
</table>


#### 2018년 4월 19일에 릴리스된 1.8.11_1509에 대한 변경 로그
{: #1811_1509}

<table summary="버전 1.8.8_1507 이후에 작성된 변경사항">
<caption>버전 1.8.8_1507 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.8.8</td>
<td>v1.8.11	</td>
<td><p>[Kubernetes 릴리스 정보![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/kubernetes/releases/tag/v1.8.11)를 참조하십시오. 이 릴리스에서는 [CVE-2017-1002101 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002101) 및 [CVE-2017-1002102 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002102) 취약성이 해결되었습니다.</p><p>이제 `secret`, `configMap`, `downwardAPI` 및 투영된 볼륨은 읽기 전용으로 마운트됩니다. 이전에는 앱이 이러한 볼륨에 데이터를 쓸 수 있었지만 시스템이 데이터를 자동으로 되돌릴 수 있었습니다. 앱이 이전의 안전하지 않은 작동에 의존하는 경우에는 이를 적절히 수정하십시오.</p></td>
</tr>
<tr>
<td>컨테이너 이미지 일시정지</td>
<td>3.0</td>
<td>3.1</td>
<td>상속된 고아 좀비 프로세스를 제거합니다.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.8.8-86</td>
<td>v1.8.11-126</td>
<td>`NodePort` 및 `LoadBalancer` 서비스는 이제 `service.spec.externalTrafficPolicy`를 `Local`로 설정하여 [클라이언스 소스 IP를 유지](/docs/containers?topic=containers-loadbalancer#node_affinity_tolerations)하는 것을 지원합니다.</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>이전 클러스터의 [에지 노드](/docs/containers?topic=containers-edge#edge) 결함 허용(toleration) 설정을 수정합니다.</td>
</tr>
</tbody>
</table>

<br />


### 버전 1.7 변경 로그(지원되지 않음)
{: #17_changelog}

버전 1.7 변경 로그를 검토하십시오.
{: shortdesc}

*   [2018년 6월 11일에 릴리스된 작업자 노드 수정팩 1.7.16_1514에 대한 변경 로그](#1716_1514)
*   [2018년 5월 18일에 릴리스된 작업자 노드 수정팩 1.7.16_1513에 대한 변경 로그](#1716_1513)
*   [2018년 5월 16일에 릴리스된 작업자 노드 수정팩 1.7.16_1512에 대한 변경 로그](#1716_1512)
*   [2018년 4월 19일에 릴리스된 1.7.16_1511에 대한 변경 로그](#1716_1511)

#### 2018년 6월 11일에 릴리스된 작업자 노드 수정팩 1.7.16_1514에 대한 변경 로그
{: #1716_1514}

<table summary="버전 1.7.16_1513 이후에 작성된 변경사항">
<caption>버전 1.7.16_1513 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>커널 업데이트</td>
<td>4.4.0-116</td>
<td>4.4.0-127</td>
<td>[CVE-2018-3639 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-3639)에 대한 커널 업데이트의 새 작업자 노드 이미지.</td>
</tr>
</tbody>
</table>

#### 2018년 5월 18일에 릴리스된 작업자 노드 수정팩 1.7.16_1513에 대한 변경 로그
{: #1716_1513}

<table summary="버전 1.7.16_1512 이후에 작성된 변경사항">
<caption>버전 1.7.16_1512 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>블록 스토리지 플러그인을 사용한 경우 발생한 버그를 해결하기 위한 수정사항입니다.</td>
</tr>
</tbody>
</table>

#### 2018년 5월 16일에 릴리스된 작업자 노드 수정팩 1.7.16_1512에 대한 변경 로그
{: #1716_1512}

<table summary="버전 1.7.16_1511 이후에 작성된 변경사항">
<caption>버전 1.7.16_1511 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>`kubelet` 루트 디렉토리에 저장하는 데이터가 이제 작업자 노드 머신의 더 큰 보조 디스크에 저장됩니다.</td>
</tr>
</tbody>
</table>

#### 2018년 4월 19일에 릴리스된 1.7.16_1511에 대한 변경 로그
{: #1716_1511}

<table summary="버전 1.7.4_1509 이후에 작성된 변경사항">
<caption>버전 1.7.4_1509 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.7.4</td>
<td>v1.7.16	</td>
<td><p>[Kubernetes 릴리스 정보![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/kubernetes/releases/tag/v1.7.16)를 참조하십시오. 이 릴리스에서는 [CVE-2017-1002101 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002101) 및 [CVE-2017-1002102 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002102) 취약성이 해결되었습니다.</p><p>이제 `secret`, `configMap`, `downwardAPI` 및 투영된 볼륨은 읽기 전용으로 마운트됩니다. 이전에는 앱이 이러한 볼륨에 데이터를 쓸 수 있었지만 시스템이 데이터를 자동으로 되돌릴 수 있었습니다. 앱이 이전의 안전하지 않은 작동에 의존하는 경우에는 이를 적절히 수정하십시오.</p></td>
</tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.7.4-133</td>
<td>v1.7.16-17</td>
<td>`NodePort` 및 `LoadBalancer` 서비스는 이제 `service.spec.externalTrafficPolicy`를 `Local`로 설정하여 [클라이언스 소스 IP를 유지](/docs/containers?topic=containers-loadbalancer#node_affinity_tolerations)하는 것을 지원합니다.</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>이전 클러스터의 [에지 노드](/docs/containers?topic=containers-edge#edge) 결함 허용(toleration) 설정을 수정합니다.</td>
</tr>
</tbody>
</table>
