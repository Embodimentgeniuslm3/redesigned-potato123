---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: kubernetes, iks

subcollection: containers

---

{:new_window: target="blank"}
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



# 사용자 액세스 권한
{: #access_reference}

[클러스터 권한을 지정](/docs/containers?topic=containers-users)할 때는 사용자에게 어느 역할을 지정해야 할지 판단하기 어려울 수 있습니다. 다음 섹션의 표를 사용하여 {{site.data.keyword.containerlong}}에서 일반적인 태스크를 수행하는 데 필요한 최소한의 권한 레벨을 판별하십시오.
{: shortdesc}

2019년 1월 30일부터 {{site.data.keyword.containerlong_notm}}는 {{site.data.keyword.Bluemix_notm}} IAM([서비스 액세스 역할](#service))으로 사용자에게 권한을 부여하는 새로운 방법을 제공합니다. 이러한 서비스 역할은 클러스터 내의 리소스(예: Kubernetes 네임스페이스)에 대한 액세스 권한을 부여하는 데 사용됩니다. 자세한 정보는 블로그, [클러스터 액세스를 보다 세밀하게 제어하기 위한 IAM의 서비스 역할 및 네임스페이스 소개 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/blogs/bluemix/2019/02/introducing-service-roles-and-namespaces-in-iam-for-more-granular-control-of-cluster-access/)를 확인하십시오.
{: note}

## {{site.data.keyword.Bluemix_notm}} IAM 플랫폼 역할
{: #iam_platform}

{{site.data.keyword.containerlong_notm}}는 {{site.data.keyword.Bluemix_notm}} Identity and Access Management(IAM) 역할을 사용하도록 구성됩니다. {{site.data.keyword.Bluemix_notm}} IAM 플랫폼 역할은 사용자가 {{site.data.keyword.Bluemix_notm}} 리소스(예: 클러스터, 작업자 노드 및 Ingress 애플리케이션 애플리케이션 로드 밸런서(ALB))에 대해 수행할 수 있는 위치를 결정합니다. 또한 {{site.data.keyword.Bluemix_notm}} IAM 플랫폼 역할은 사용자의 기본 인프라 권한을 자동으로 설정합니다. 플랫폼 역할을 설정하려면 [{{site.data.keyword.Bluemix_notm}} IAM 플랫폼 권한 지정](/docs/containers?topic=containers-users#platform)을 참조하십시오.
{: shortdesc}

<p class="tip">서비스 역할과 동일한 시간에 {{site.data.keyword.Bluemix_notm}} IAM platform 역할을 지정하지 마십시오. 플랫폼 및 서비스 역할을 별도로 지정해야 합니다.</p>

다음 각 절에 나와 있는 표에는 클러스터 관리, 로깅 및 {{site.data.keyword.Bluemix_notm}} IAM 플랫폼 역할에 의해 부여되는 Ingress 권한이 표시됩니다. 해당 표는 CLI 명령 이름에 따라 알파벳순으로 구성되어 있습니다.

* [권한이 필요하지 않은 조치](#none-actions)
* [뷰어 조치](#view-actions)
* [편집자 조치](#editor-actions)
* [운영자 조치](#operator-actions)
* [관리자 조치](#admin-actions)

### 권한이 필요하지 않은 조치
{: #none-actions}

CLI 명령을 실행하거나 다음 표의 조치에 해당하는 API를 호출하는 계정의 모든 사용자는 지정된 권한이 없는 경우에도 결과를 볼 수 있습니다.
{: shortdesc}

<table>
<caption>{{site.data.keyword.containerlong_notm}}에서 권한이 필요하지 않은 CLI 명령 및 API 호출 개요</caption>
<thead>
<th id="none-actions-action">조치</th>
<th id="none-actions-cli">CLI 명령</th>
<th id="none-actions-api">API 호출</th>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.containerlong_notm}}에 관리 추가 기능에 대해 지원되는 버전 목록을 봅니다.</td>
<td><code>[ibmcloud ks addon-versions](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_addon_versions)</code></td>
<td><code>[GET /v1/addon](https://containers.cloud.ibm.com/global/swagger-global-api/#/util/GetAddons)</code></td>
</tr>
<tr>
<td>{{site.data.keyword.containerlong_notm}}에 대한 API 엔드포인트를 보거나 대상으로 지정합니다.</td>
<td><code>[ibmcloud ks api](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cli_api)</code></td>
<td>-</td>
</tr>
<tr>
<td>지원되는 명령 및 매개변수의 목록을 봅니다.</td>
<td><code>[ibmcloud ks help](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_help)</code></td>
<td>-</td>
</tr>
<tr>
<td>{{site.data.keyword.containerlong_notm}} 플러그인을 초기화하거나 Kubernetes 클러스터를 작성 또는 액세스할 지역을 지정합니다.</td>
<td><code>[ibmcloud ks init](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_init)</code></td>
<td>-</td>
</tr>
<tr>
<td>더 이상 사용되지 않음: {{site.data.keyword.containerlong_notm}}에서 지원되는 Kubernetes 버전의 목록을 봅니다. </td>
<td><code>[ibmcloud ks kube-versions](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_kube_versions)</code></td>
<td><code>[GET /v1/kube-versions](https://containers.cloud.ibm.com/global/swagger-global-api/#/util/GetKubeVersions)</code></td>
</tr>
<tr>
<td>작업자 노드에 대해 사용 가능한 머신 유형의 목록을 봅니다.</td>
<td><code>[ibmcloud ks machine-types](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_machine_types)</code></td>
<td><code>[GET /v1/datacenters/{datacenter}/machine-types](https://containers.cloud.ibm.com/global/swagger-global-api/#/util/GetDatacenterMachineTypes)</code></td>
</tr>
<tr>
<td>IBM ID 사용자에 대한 현재 메시지를 봅니다.</td>
<td><code>[ibmcloud ks messages](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_messages)</code></td>
<td><code>[GET /v1/messages](https://containers.cloud.ibm.com/global/swagger-global-api/#/util/GetMessages)</code></td>
</tr>
<tr>
<td>더 이상 사용되지 않음: 사용자가 현재 있는 {{site.data.keyword.containerlong_notm}} 지역을 찾습니다. </td>
<td><code>[ibmcloud ks region](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_region)</code></td>
<td>-</td>
</tr>
<tr>
<td>더 이상 사용되지 않음: {{site.data.keyword.containerlong_notm}}의 지역을 설정합니다. </td>
<td><code>[ibmcloud ks region-set](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_region-set)</code></td>
<td>-</td>
</tr>
<tr>
<td>더 이상 사용되지 않음: 사용 가능한 지역을 나열합니다. </td>
<td><code>[ibmcloud ks regions](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_regions)</code></td>
<td><code>[GET /v1/regions](https://containers.cloud.ibm.com/global/swagger-global-api/#/util/GetRegions)</code></td>
</tr>
<tr>
<td>{{site.data.keyword.containerlong_notm}}에서 지원되는 위치의 목록을 봅니다. </td>
<td><code>[ibmcloud ks supported-locations](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_supported-locations)</code></td>
<td><code>[GET /v1/locations](https://containers.cloud.ibm.com/global/swagger-global-api/#/util/ListLocations)</code></td>
</tr>
<tr>
<td>{{site.data.keyword.containerlong_notm}}에서 지원되는 버전의 목록을 봅니다. </td>
<td><code>[ibmcloud ks versions](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_versions)</code></td>
<td>-</td>
</tr>
<tr>
<td>사용자가 클러스터를 작성할 수 있는 사용 가능한 구역 목록을 봅니다.</td>
<td><code>[ibmcloud ks zones](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_datacenters)</code></td>
<td><code>[GET /v1/zones](https://containers.cloud.ibm.com/global/swagger-global-api/#/util/GetZones)</code></td>
</tr>
</tbody>
</table>

### 뷰어 조치
{: #view-actions}

**뷰어** 플랫폼 역할에는 [권한이 필요하지 않은 조치](#none-actions)와 다음 표에 표시된 권한이 포함됩니다. **뷰어** 역할로 감사자 또는 비용을 청구하는 사용자는 클러스터 세부사항을 볼 수 있지만 인프라를 수정할 수 없습니다.
{: shortdesc}

<table>
<caption>{{site.data.keyword.containerlong_notm}}에서 뷰어 플랫폼 역할이 필요한 CLI 명령 및 API 호출의 개요</caption>
<thead>
<th id="view-actions-mngt">조치</th>
<th id="view-actions-cli">CLI 명령</th>
<th id="view-actions-api">API 호출</th>
</thead>
<tbody>
<tr>
<td>Ingress ALB에 대한 정보를 봅니다.</td>
<td><code>[ibmcloud ks alb-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_get)</code></td>
<td><code>[GET /albs/{albId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/GetClusterALB)</code></td>
</tr>
<tr>
<td>지역에서 지원되는 ALB 유형을 봅니다.</td>
<td><code>[ibmcloud ks alb-types](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_types)</code></td>
<td><code>[GET /albtypes](https://containers.cloud.ibm.com/global/swagger-global-api/#/util/GetAvailableALBTypes)</code></td>
</tr>
<tr>
<td>클러스터의 모든 Ingress ALB를 나열합니다.</td>
<td><code>[ibmcloud ks albs](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_albs)</code></td>
<td><code>[GET /clusters/{idOrName}](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/GetClusterALBs)</code></td>
</tr>
<tr>
<td>리소스 그룹 및 지역에 대해 {{site.data.keyword.Bluemix_notm}} IAM API 키 소유자의 이름 및 이메일 주소를 봅니다.</td>
<td><code>[ibmcloud ks api-key-info](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_api_key_info)</code></td>
<td><code>[GET /v1/logging/{idOrName}/clusterkeyowner](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/GetClusterKeyOwner)</code></td>
</tr>
<tr>
<td>클러스터에 연결하는 데 필요한 Kubernetes 구성 데이터 및 인증서를 다운로드하고 `kubectl` 명령을 실행합니다.</td>
<td><code>[ibmcloud ks cluster-config](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_config)</code></td>
<td><code>[GET /v1/clusters/{idOrName}/config](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetClusterConfig)</code></td>
</tr>
<tr>
<td>클러스터에 대한 정보를 봅니다.</td>
<td><code>[ibmcloud ks cluster-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_get)</code></td>
<td><code>[GET /v1/clusters/{idOrName}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetCluster)</code></td>
</tr>
<tr>
<td>클러스터에 바인딩된 모든 네임스페이스의 모든 서비스를 나열합니다.</td>
<td><code>[ibmcloud ks cluster-services](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_services)</code></td>
<td><code>[GET /v1/clusters/{idOrName}/services](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/ListServicesForAllNamespaces)</code></td>
</tr>
<tr>
<td>모든 클러스터를 나열합니다.</td>
<td><code>[ibmcloud ks clusters](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_clusters)</code></td>
<td><code>[GET /v1/clusters](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetClusters)</code></td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} 계정이 다른 IBM Cloud 인프라(SoftLayer) 포트폴리오에 액세스하도록 설정된 인프라 인증 정보를 가져옵니다.</td>
<td><code>[ibmcloud ks credential-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credential_get)</code></td><td><code>[GET /v1/credentials](https://containers.cloud.ibm.com/global/swagger-global-api/#/accounts/GetUserCredentials)</code></td>
</tr>
<tr>
<td>대상으로 지정된 지역 및 리소스 그룹의 IBM Cloud 인프라(SoftLayer) 포트폴리오에 액세스할 수 있게 해 주는 인증 정보에 권장 또는 필수 인프라 권한이 누락되어 있는지 확인합니다. </td>
<td><code>[ibmcloud ks infra-permissions-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#infra_permissions_get)</code></td>
<td><code>[GET /v1/infra-permissions](https://containers.cloud.ibm.com/global/swagger-global-api/#/accounts/GetInfraPermissions)</code></td>
</tr>
<tr>
<td>Fluentd 추가 기능에 대한 자동 업데이트의 상태를 봅니다.</td>
<td><code>[ibmcloud ks logging-autoupdate-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_autoupdate_get)</code></td>
<td><code>[GET /v1/logging/{idOrName}/updatepolicy](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/GetUpdatePolicy)</code></td>
</tr>
<tr>
<td>대상으로 지정된 지역의 기본 로깅 엔드포인트를 봅니다. </td>
<td>-</td>
<td><code>[GET /v1/logging/{idOrName}/default](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/GetDefaultLoggingEndpoint)</code></td>
</tr>
<tr>
<td>클러스터의 모든 로그 전달 구성 또는 클러스터의 특정 로그 소스를 나열합니다.</td>
<td><code>[ibmcloud ks logging-config-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_logging_get)</code></td>
<td><code>[GET /v1/logging/{idOrName}/loggingconfig](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/FetchLoggingConfigs) 및 [GET /v1/logging/{idOrName}/loggingconfig/{logSource}](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/FetchLoggingConfigsForSource)</code></td>
</tr>
<tr>
<td>로그 필터링 구성에 대한 정보를 봅니다.</td>
<td><code>[ibmcloud ks logging-filter-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_filter_view)</code></td>
<td><code>[GET /v1/logging/{idOrName}/filterconfigs/{id}](https://containers.cloud.ibm.com/global/swagger-global-api/#/filter/FetchFilterConfig)</code></td>
</tr>
<tr>
<td>클러스터의 모든 로깅 필터 구성을 나열합니다.</td>
<td><code>[ibmcloud ks logging-filter-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_filter_view)</code></td>
<td><code>[GET /v1/logging/{idOrName}/filterconfigs](https://containers.cloud.ibm.com/global/swagger-global-api/#/filter/FetchFilterConfigs)</code></td>
</tr>
<tr>
<td>특정 네임스페이스에 바인드된 모든 서비스를 나열합니다. </td>
<td>-</td>
<td><code>[GET /v1/clusters/{idOrName}/services/{namespace}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/ListServicesInNamespace)</code></td>
</tr>
<tr>
<td>클러스터에 바인딩된 모든 사용자 관리 서브넷을 나열합니다.</td>
<td>-</td>
<td><code>[GET /v1/clusters/{idOrName}/usersubnets](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetClusterUserSubnet)</code></td>
</tr>
<tr>
<td>인프라 계정에서 사용 가능한 서브넷을 나열합니다.</td>
<td><code>[ibmcloud ks subnets](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_subnets)</code></td>
<td><code>[GET /v1/subnets](https://containers.cloud.ibm.com/global/swagger-global-api/#/properties/ListSubnets)</code></td>
</tr>
<tr>
<td>인프라 계정에 대한 VLAN Spanning 상태를 봅니다.</td>
<td><code>[ibmcloud ks vlan-spanning-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get)</code></td>
<td><code>[GET /v1/subnets/vlan-spanning](https://containers.cloud.ibm.com/global/swagger-global-api/#/accounts/GetVlanSpanning)</code></td>
</tr>
<tr>
<td>한 클러스터에 대해 설정된 경우: 구역 내에서 해당 클러스터가 연결된 VLAN을 나열합니다.</br>계정 내 모든 클러스터에 대해 설정된 경우: 구역 내의 모든 사용 가능한 VLAN을 나열합니다.</td>
<td><code>[ibmcloud ks vlans](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlans)</code></td>
<td><code>[GET /v1/datacenters/{datacenter}/vlans](https://containers.cloud.ibm.com/global/swagger-global-api/#/properties/GetDatacenterVLANs)</code></td>
</tr>
<tr>
<td>클러스터의 모든 웹훅을 나열합니다.</td>
<td>-</td>
<td><code>[GET /v1/clusters/{idOrName}/webhooks](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetClusterWebhooks)</code></td>
</tr>
<tr>
<td>작업자 노드에 대한 정보를 봅니다.</td>
<td><code>[ibmcloud ks worker-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_get)</code></td>
<td><code>[GET /v1/clusters/{idOrName}/workers/{workerId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetWorkers)</code></td>
</tr>
<tr>
<td>작업자 풀에 대한 정보를 봅니다.</td>
<td><code>[ibmcloud ks worker-pool-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_get)</code></td>
<td><code>[GET /v1/clusters/{idOrName}/workerpools/{poolidOrName}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetWorkerPool)</code></td>
</tr>
<tr>
<td>클러스터의 모든 작업자 풀을 나열합니다.</td>
<td><code>[ibmcloud ks worker-pools](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pools)</code></td>
<td><code>[GET /v1/clusters/{idOrName}/workerpools](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetWorkerPools)</code></td>
</tr>
<tr>
<td>클러스터의 모든 작업자 노드를 나열합니다.</td>
<td><code>[ibmcloud ks workers](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_workers)</code></td>
<td><code>[GET /v1/clusters/{idOrName}/workers](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetClusterWorkers)</code></td>
</tr>
</tbody>
</table>

### 편집자 조치
{: #editor-actions}

**편집자** 플랫폼 역할에는 **뷰어**에 의해 부여되는 권한과 다음과 같은 권한이 포함됩니다. **편집자** 역할로 개발자와 같은 사용자는 서비스를 바인드하고, Ingress 리소스 관련 작업을 수행하고, 앱에 대한 로그 전달을 설정할 수 있으나 인프라를 수정할 수 없습니다. **팁**: 이 역할은 앱 개발자에 대해 사용하고, <a href="#cloud-foundry">Cloud Foundry</a> **개발자** 역할을 지정하십시오.
{: shortdesc}

<table>
<caption>{{site.data.keyword.containerlong_notm}}에서 편집자 플랫폼 역할이 필요한 CLI 명령 및 API 호출의 개요</caption>
<thead>
<th id="editor-actions-mngt">조치</th>
<th id="editor-actions-cli">CLI 명령</th>
<th id="editor-actions-api">API 호출</th>
</thead>
<tbody>
<tr>
<td>Ingress ALB 추가 기능에 대한 자동 업데이트를 사용 안함으로 설정합니다.</td>
<td><code>[ibmcloud ks alb-autoupdate-disable](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_autoupdate_disable)</code></td>
<td><code>[PUT /clusters/{idOrName}/updatepolicy](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/ChangeUpdatePolicy)</code></td>
</tr>
<tr>
<td>Ingress ALB 추가 기능에 대한 자동 업데이트를 사용으로 설정합니다.</td>
<td><code>[ibmcloud ks alb-autoupdate-enable](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_autoupdate_enable)</code></td>
<td><code>[PUT /clusters/{idOrName}/updatepolicy](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/ChangeUpdatePolicy)</code></td>
</tr>
<tr>
<td>Ingress ALB 추가 기능에 대한 자동 업데이트가 사용으로 설정되어 있는지 확인합니다. </td>
<td><code>[ibmcloud ks alb-autoupdate-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_autoupdate_get)</code></td>
<td><code>[GET /clusters/{idOrName}/updatepolicy](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/GetUpdatePolicy)</code></td>
</tr>
<tr>
<td>Ingress ALB를 사용 또는 사용 안함으로 설정합니다.</td>
<td><code>[ibmcloud ks alb-configure](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_configure)</code></td>
<td><code>[POST /albs](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/EnableALB) 및 [DELETE /albs/{albId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/)</code></td>
</tr>
<tr>
<td>Ingress ALB를 작성합니다. </td>
<td><code>[ibmcloud ks alb-create](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_create)</code></td>
<td><code>[POST /clusters/{idOrName}/zone/{zoneId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/CreateALB)</code></td>
</tr>
<tr>
<td>Ingress ALB 추가 기능 업데이트를 ALB 팟(Pod)이 이전에 실행하던 빌드로 롤백합니다. </td>
<td><code>[ibmcloud ks alb-rollback](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_rollback)</code></td>
<td><code>[PUT /clusters/{idOrName}/updaterollback](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/RollbackUpdate)</code></td>
</tr>
<tr>
<td>Ingress ALB 추가 기능을 수동으로 업데이트하여 ALB 팟(Pod)의 일회성 업데이트를 강제 실행합니다.</td>
<td><code>[ibmcloud ks alb-update](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_update)</code></td>
<td><code>[PUT /clusters/{idOrName}/update](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/UpdateALBs)</code></td>
</tr>
<tr>
<td>API 서버 감사 웹훅을 작성합니다.</td>
<td><code>[ibmcloud ks apiserver-config-set](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_apiserver_config_set)</code></td>
<td><code>[PUT /v1/clusters/{idOrName}/apiserverconfigs/auditwebhook](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/apiserverconfigs/UpdateAuditWebhook)</code></td>
</tr>
<tr>
<td>API 서버 감사 웹훅을 삭제합니다.</td>
<td><code>[ibmcloud ks apiserver-config-unset](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_apiserver_config_unset)</code></td>
<td><code>[DELETE /v1/clusters/{idOrName}/apiserverconfigs/auditwebhook](https://containers.cloud.ibm.com/global/swagger-global-api/#/apiserverconfigs/DeleteAuditWebhook)</code></td>
</tr>
<tr>
<td>서비스를 클러스터에 바인드합니다. **참고**: 서비스 인스턴스가 있는 영역에 대한 Cloud Foundry 개발자 역할을 보유하고 있어야 합니다. </td>
<td><code>[ibmcloud ks cluster-service-bind](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_service_bind)</code></td>
<td><code>[POST /v1/clusters/{idOrName}/services](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/BindServiceToNamespace)</code></td>
</tr>
<tr>
<td>클러스터에서 서비스를 바인드 해제합니다. **참고**: 서비스 인스턴스가 있는 영역에 대한 Cloud Foundry 개발자 역할을 보유하고 있어야 합니다. </td>
<td><code>[ibmcloud ks cluster-service-unbind](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_service_unbind)</code></td>
<td><code>[DELETE /v1/clusters/{idOrName}/services/{namespace}/{serviceInstanceId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/UnbindServiceFromNamespace)</code></td>
</tr>
<tr>
<td><code>kube-audit</code>를 제외한 모든 로그 소스에 대한 로그 전달 구성을 작성합니다.</td>
<td><code>[ibmcloud ks logging-config-create](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_logging_create)</code></td>
<td><code>[POST /v1/logging/{idOrName}/loggingconfig/{logSource}](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/CreateLoggingConfig)</code></td>
</tr>
<tr>
<td>로그 전달 구성을 새로 고칩니다.</td>
<td><code>[ibmcloud ks logging-config-refresh](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_logging_refresh)</code></td>
<td><code>[PUT /v1/logging/{idOrName}/refresh](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/RefreshLoggingConfig)</code></td>
</tr>
<tr>
<td><code>kube-audit</code>를 제외한 모든 로그 소스에 대한 로그 전달 구성을 삭제합니다.</td>
<td><code>[ibmcloud ks logging-config-rm](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_logging_rm)</code></td>
<td><code>[DELETE /v1/logging/{idOrName}/loggingconfig/{logSource}/{id}](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/DeleteLoggingConfig)</code></td>
</tr>
<tr>
<td>클러스터에 대한 모든 로그 전달 구성을 삭제합니다.</td>
<td>-</td>
<td><code>[DELETE /v1/logging/{idOrName}/loggingconfig](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/DeleteLoggingConfigs)</code></td>
</tr>
<tr>
<td>로그 전달 구성을 업데이트하십시오.</td>
<td><code>[ibmcloud ks logging-config-update](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_logging_update)</code></td>
<td><code>[PUT /v1/logging/{idOrName}/loggingconfig/{logSource}/{id}](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/UpdateLoggingConfig)</code></td>
</tr>
<tr>
<td>로그 필터링 구성을 작성합니다.</td>
<td><code>[ibmcloud ks logging-filter-create](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_filter_create)</code></td>
<td><code>[POST /v1/logging/{idOrName}/filterconfigs](https://containers.cloud.ibm.com/global/swagger-global-api/#/filter/CreateFilterConfig)</code></td>
</tr>
<tr>
<td>로그 필터링 구성을 삭제합니다.</td>
<td><code>[ibmcloud ks logging-filter-rm](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_filter_delete)</code></td>
<td><code>[DELETE /v1/logging/{idOrName}/filterconfigs/{id}](https://containers.cloud.ibm.com/global/swagger-global-api/#/filter/DeleteFilterConfig)</code></td>
</tr>
<tr>
<td>Kubernetes 클러스터에 대한 모든 로깅 필터 구성을 삭제합니다.</td>
<td>-</td>
<td><code>[DELETE /v1/logging/{idOrName}/filterconfigs](https://containers.cloud.ibm.com/global/swagger-global-api/#/filter/DeleteFilterConfigs)</code></td>
</tr>
<tr>
<td>로그 필터링 구성을 업데이트합니다.</td>
<td><code>[ibmcloud ks logging-filter-update](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_filter_update)</code></td>
<td><code>[PUT /v1/logging/{idOrName}/filterconfigs/{id}](https://containers.cloud.ibm.com/global/swagger-global-api/#/filter/UpdateFilterConfig)</code></td>
</tr>
<tr>
<td>기존 NLB 호스트 이름에 하나의 NLB IP 주소를 추가합니다. </td>
<td><code>[ibmcloud ks nlb-dns-add](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-add)</code></td>
<td><code>[PUT /clusters/{idOrName}/add](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb45dns/UpdateDNSWithIP)</code></td>
</tr>
<tr>
<td>NLB IP 주소를 등록할 DNS 호스트 이름을 작성합니다. </td>
<td><code>[ibmcloud ks nlb-dns-create](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-create)</code></td>
<td><code>[POST /clusters/{idOrName}/register](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb45dns/RegisterDNSWithIP)</code></td>
</tr>
<tr>
<td>클러스터에 등록된 NLB 호스트 이름 및 IP 주소를 나열합니다.</td>
<td><code>[ibmcloud ks nlb-dnss](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-ls)</code></td>
<td><code>[GET /clusters/{idOrName}/list](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb45dns/ListNLBIPsForSubdomain)</code></td>
</tr>
<tr>
<td>호스트 이름에서 NLB IP 주소를 제거합니다.</td>
<td><code>[ibmcloud ks nlb-dns-rm](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-rm)</code></td>
<td><code>[DELETE /clusters/{idOrName}/host/{nlbHost}/ip/{nlbIP}/remove](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb45dns/UnregisterDNSWithIP)</code></td>
</tr>
<tr>
<td>클러스터의 기존 NLB 호스트 이름에 대한 상태 검사 모니터를 구성하고 선택적으로 사용으로 설정합니다.</td>
<td><code>[ibmcloud ks nlb-dns-monitor-configure](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-monitor-configure)</code></td>
<td><code>[POST /health/clusters/{idOrName}/config](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb45health45monitor/AddNlbDNSHealthMonitor)</code></td>
</tr>
<tr>
<td>기존 상태 검사 모니터에 대한 설정을 봅니다.</td>
<td><code>[ibmcloud ks nlb-dns-monitor-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-monitor-get)</code></td>
<td><code>[GET /health/clusters/{idOrName}/host/{nlbHost}/config](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb45health45monitor/GetNlbDNSHealthMonitor)</code></td>
</tr>
<tr>
<td>클러스터의 호스트 이름에 대한 기존 상태 검사 모니터를 사용 안함으로 설정합니다.</td>
<td><code>[ibmcloud ks nlb-dns-monitor-disable](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-monitor-disable)</code></td>
<td><code>[PUT /clusters/{idOrName}/health](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb45health45monitor/UpdateNlbDNSHealthMonitor)</code></td>
</tr>
<tr>
<td>구성한 상태 검사 모니터를 사용으로 설정합니다.</td>
<td><code>[ibmcloud ks nlb-dns-monitor-enable](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-monitor-enable)</code></td>
<td><code>[PUT /clusters/{idOrName}/health](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb45health45monitor/UpdateNlbDNSHealthMonitor)</code></td>
</tr>
<tr>
<td>클러스터의 각 NLB 호스트 이름에 대한 상태 검사 모니터 설정을 나열합니다.</td>
<td><code>[ibmcloud ks nlb-dns-monitor-ls](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-monitor-ls)</code></td>
<td><code>[GET /health/clusters/{idOrName}/list](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb45health45monitor/ListNlbDNSHealthMonitors)</code></td>
</tr>
<tr>
<td>클러스터의 NLB 호스트 이름으로 등록된 각 IP 주소의 상태 검사 상태를 나열합니다. </td>
<td><code>[ibmcloud ks nlb-dns-monitor-status](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-monitor-status)</code></td>
<td><code>[GET /health/clusters/{idOrName}/status](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb45health45monitor/ListNlbDNSHealthMonitorStatus)</code></td>
</tr>
<tr>
<td>클러스터에 웹훅을 작성합니다.</td>
<td><code>[ibmcloud ks webhook-create](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_webhook_create)</code></td>
<td><code>[POST /v1/clusters/{idOrName}/webhooks](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/AddClusterWebhooks)</code></td>
</tr>
</tbody>
</table>

### 운영자 조치
{: #operator-actions}

**운영자** 플랫폼 역할에는 **뷰어**에 의해 부여되는 권한과 다음 표에 표시된 권한이 포함됩니다. **운영자** 역할로 사이트 신뢰성 엔지니어, DevOps 엔지니어 또는 클러스터 관리자와 같은 사용자는 작업자 노드를 추가하고 작업자 노드 다시 로드와 같은 작업을 통해 인프라의 문제점을 해결할 수 있으나 클러스터 작성 또는 삭제, 인증 정보 변경 또는 클러스터 측 기능(예: 서비스 엔드포인트 또는 관리 추가 기능) 설정을 수행할 수 없습니다.
{: shortdesc}

<table>
<caption>{{site.data.keyword.containerlong_notm}}에서 운영자 플랫폼 역할이 필요한 CLI 명령 및 API 호출의 개요</caption>
<thead>
<th id="operator-mgmt">조치</th>
<th id="operator-cli">CLI 명령</th>
<th id="operator-api">API 호출</th>
</thead>
<tbody>
<tr>
<td>Kubernetes 마스터를 새로 고칩니다.</td>
<td><code>[ibmcloud ks apiserver-refresh](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_apiserver_refresh)(cluster-refresh)</code></td>
<td><code>[PUT /v1/clusters/{idOrName}/masters](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/HandleMasterAPIServer)</code></td>
</tr>
<tr>
<td>클러스터의 {{site.data.keyword.Bluemix_notm}} IAM 서비스 ID를 작성하고 {{site.data.keyword.registrylong_notm}}에서 **독자** 서비스 액세스 역할을 지정하는 서비스 ID에 대해 정책을 작성한 다음 서비스 ID의 API 키를 작성합니다.</td>
<td><code>[ibmcloud ks cluster-pull-secret-apply](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_pull_secret_apply)</code></td>
<td>-</td>
</tr>
<tr>
<td>클러스터에 서브넷을 추가합니다.</td>
<td><code>[ibmcloud ks cluster-subnet-add](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_subnet_add)</code></td>
<td><code>[PUT /v1/clusters/{idOrName}/subnets/{subnetId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/AddClusterSubnet)</code></td>
</tr>
<tr>
<td>서브넷을 작성하여 클러스터에 추가합니다. </td>
<td><code>[ibmcloud ks cluster-subnet-create](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_subnet_create)</code></td>
<td><code>[POST /v1/clusters/{idOrName}/vlans/{vlanId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/CreateClusterSubnet)</code></td>
</tr>
<tr>
<td>클러스터를 업데이트합니다.</td>
<td><code>[ibmcloud ks cluster-update](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_update)</code></td>
<td><code>[PUT /v1/clusters/{idOrName}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/UpdateCluster)</code></td>
</tr>
<tr>
<td>클러스터에 사용자 관리 서브넷을 추가합니다.</td>
<td><code>[ibmcloud ks cluster-user-subnet-add](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_user_subnet_add)</code></td>
<td><code>[POST /v1/clusters/{idOrName}/usersubnets](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/AddClusterUserSubnet)</code></td>
</tr>
<tr>
<td>클러스터에서 사용자 관리 서브넷을 제거합니다.</td>
<td><code>[ibmcloud ks cluster-user-subnet-rm](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_user_subnet_rm)</code></td>
<td><code>[DELETE /v1/clusters/{idOrName}/usersubnets/{subnetId}/vlans/{vlanId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/RemoveClusterUserSubnet)</code></td>
</tr>
<tr>
<td>작업자 노드를 추가합니다.</td>
<td><code>[ibmcloud ks worker-add(더 이상 사용되지 않음)](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_add)</code></td>
<td><code>[POST /v1/clusters/{idOrName}/workers](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/AddClusterWorkers)</code></td>
</tr>
<tr>
<td>작업자 풀을 작성하십시오.</td>
<td><code>[ibmcloud ks worker-pool-create](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_create)</code></td>
<td><code>[POST /v1/clusters/{idOrName}/workerpools](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/CreateWorkerPool)</code></td>
</tr>
<tr>
<td>작업자 풀을 리밸런싱합니다.</td>
<td><code>[ibmcloud ks worker-pool-rebalance](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_rebalance)</code></td>
<td><code>[PATCH /v1/clusters/{idOrName}/workerpools/{poolidOrName}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/PatchWorkerPool)</code></td>
</tr>
<tr>
<td>작업자 풀의 크기를 조정합니다.</td>
<td><code>[ibmcloud ks worker-pool-resize](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_resize)</code></td>
<td><code>[PATCH /v1/clusters/{idOrName}/workerpools/{poolidOrName}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/PatchWorkerPool)</code></td>
</tr>
<tr>
<td>작업자 풀을 삭제합니다.</td>
<td><code>[ibmcloud ks worker-pool-rm](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_rm)</code></td>
<td><code>[DELETE /v1/clusters/{idOrName}/workerpools/{poolidOrName}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/RemoveWorkerPool)</code></td>
</tr>
<tr>
<td>작업자 노드를 다시 부팅합니다.</td>
<td><code>[ibmcloud ks worker-reboot](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reboot)</code></td>
<td><code>[PUT /v1/clusters/{idOrName}/workers/{workerId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/UpdateClusterWorker)</code></td>
</tr>
<tr>
<td>작업자 노드를 다시 로드합니다.</td>
<td><code>[ibmcloud ks worker-reload](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)</code></td>
<td><code>[PUT /v1/clusters/{idOrName}/workers/{workerId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/UpdateClusterWorker)</code></td>
</tr>
<tr>
<td>작업자 노드를 제거합니다.</td>
<td><code>[ibmcloud ks worker-rm](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_rm)</code></td>
<td><code>[DELETE /v1/clusters/{idOrName}/workers/{workerId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/RemoveClusterWorker)</code></td>
</tr>
<tr>
<td>작업자 노드를 업데이트합니다.</td>
<td><code>[ibmcloud ks worker-update](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update)</code></td>
<td><code>[PUT /v1/clusters/{idOrName}/workers/{workerId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/UpdateClusterWorker)</code></td>
</tr>
<tr>
<td>작업자 풀에 구역을 추가합니다. </td>
<td><code>[ibmcloud ks zone-add](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_zone_add)</code></td>
<td><code>[POST /v1/clusters/{idOrName}/workerpools/{poolidOrName}/zones](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/AddWorkerPoolZone)</code></td>
</tr>
<tr>
<td>작업자 풀의 특정 구역에 대한 네트워크 구성을 업데이트합니다.</td>
<td><code>[ibmcloud ks zone-network-set](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_zone_network_set)</code></td>
<td><code>[PATCH /v1/clusters/{idOrName}/workerpools/{poolidOrName}/zones/{zoneid}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/AddWorkerPoolZoneNetwork)</code></td>
</tr>
<tr>
<td>작업자 풀에서 구역을 제거합니다.</td>
<td><code>[ibmcloud ks zone-rm](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_zone_rm)</code></td>
<td><code>[DELETE /v1/clusters/{idOrName}/workerpools/{poolidOrName}/zones/{zoneid}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/RemoveWorkerPoolZone)</code></td>
</tr>
</tbody>
</table>

### 관리자 조치
{: #admin-actions}

**관리자** 플랫폼 역할에는 **뷰어**, **편집자** 및 **운영자** 역할에 의해 부여되는 모든 권한과 다음과 같은 권한이 포함됩니다. **관리자** 역할로 클러스터 또는 계정 관리자와 같은 사용자가 클러스터 작성 또는 삭제 또는 클러스터 측 기능(예: 서비스 엔드포인트 또는 관리 추가 기능) 설정을 수행할 수 있습니다. 인프라 리소스(예: 작업자 노드 머신, VLAN 및 서브넷)와 같은 주문을 작성하려면 관리자에게 **수퍼유저** <a href="#infra">인프라 역할</a>이 필요하거나 해당 지역의 API 키를 적절한 권한으로 설정해야 합니다.
{: shortdesc}

<table>
<caption>{{site.data.keyword.containerlong_notm}}에서 관리자 플랫폼 역할이 필요한 CLI 명령 및 API 호출의 개요</caption>
<thead>
<th id="admin-mgmt">조치</th>
<th id="admin-cli">CLI 명령</th>
<th id="admin-api">API 호출</th>
</thead>
<tbody>
<tr>
<td>베타: {{site.data.keyword.cloudcerts_long_notm}} 인스턴스의 인증서를 ALB에 배치하거나 업데이트합니다. </td>
<td><code>[ibmcloud ks alb-cert-deploy](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_cert_deploy)</code></td>
<td><code>[POST /albsecrets](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/CreateALBSecret) 또는 [PUT /albsecrets](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/UpdateALBSecret)</code></td>
</tr>
<tr>
<td>베타: 클러스터의 ALB 시크릿에 대한 세부사항을 봅니다. </td>
<td><code>[ibmcloud ks alb-cert-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_cert_get)</code></td>
<td><code>[GET /clusters/{idOrName}/albsecrets](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/ViewClusterALBSecrets)</code></td>
</tr>
<tr>
<td>베타: 클러스터에서 ALB 시크릿을 제거합니다. </td>
<td><code>[ibmcloud ks alb-cert-rm](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_cert_rm)</code></td>
<td><code>[DELETE /clusters/{idOrName}/albsecrets](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/DeleteClusterALBSecrets)</code></td>
</tr>
<tr>
<td>클러스터의 모든 ALB 시크릿을 나열합니다.</td>
<td><code>[ibmcloud ks alb-certs](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_certs)</code></td>
<td>-</td>
</tr>
<tr>
<td>연결된 IBM Cloud 인프라(SoftLayer) 포트폴리오에 액세스하기 위한 {{site.data.keyword.Bluemix_notm}} 계정의 API 키를 설정합니다.</td>
<td><code>[ibmcloud ks api-key-reset](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_api_key_reset)</code></td>
<td><code>[POST /v1/keys](https://containers.cloud.ibm.com/global/swagger-global-api/#/accounts/ResetUserAPIKey)</code></td>
</tr>
<tr>
<td>클러스터에서 관리 추가 기능(예: Istio 또는 Knative)을 사용 안함으로 설정합니다.</td>
<td><code>[ibmcloud ks cluster-addon-disable](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_addon_disable)</code></td>
<td><code>[PATCH /v1/clusters/{idOrName}/addons](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/ManageClusterAddons)</code></td>
</tr>
<tr>
<td>클러스터에서 관리 추가 기능(예: Istio 또는 Knative)을 사용으로 설정합니다.</td>
<td><code>[ibmcloud ks cluster-addon-enable](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_addon_enable)</code></td>
<td><code>[PATCH /v1/clusters/{idOrName}/addons](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/ManageClusterAddons)</code></td>
</tr>
<tr>
<td>클러스터에서 사용으로 설정된 관리 추가 기능(예: Istio 또는 Knative)을 나열합니다. </td>
<td><code>[ibmcloud ks cluster-addons](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_addons)</code></td>
<td><code>[GET /v1/clusters/{idOrName}/addons](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetClusterAddons)</code></td>
</tr>
<tr>
<td>무료 또는 표준 클러스터를 작성합니다. **참고**: 또한 {{site.data.keyword.registrylong_notm}} 및 수퍼유저 인프라 역할에 대한 관리자 플랫폼 역할도 필요합니다.</td>
<td><code>[ibmcloud ks cluster-create](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_create)</code></td>
<td><code>[POST /v1/clusters](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/CreateCluster)</code></td>
</tr>
<tr>
<td>클러스터에 지정된 기능을 사용 안함으로 설정합니다(예: 클러스터 마스터의 공용 서비스 엔드포인트).</td>
<td><code>[ibmcloud ks cluster-feature-disable](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_disable)</code></td>
<td>-</td>
</tr>
<tr>
<td>클러스터에 지정된 기능을 사용으로 설정합니다(예: 클러스터 마스터의 개인 서비스 엔드포인트).</td>
<td><code>[ibmcloud ks cluster-feature-enable](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable)</code></td>
<td>-</td>
</tr>
<tr>
<td>클러스터를 삭제합니다.</td>
<td><code>[ibmcloud ks cluster-rm](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_rm)</code></td>
<td><code>[DELETE /v1/clusters/{idOrName}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/RemoveCluster)</code></td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} 계정이 다른 IBM Cloud 인프라(SoftLayer) 포트폴리오에 액세스하기 위한 인프라 인증 정보를 설정합니다.</td>
<td><code>[ibmcloud ks credential-set](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_set)</code></td>
<td><code>[POST /v1/credentials](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/accounts/StoreUserCredentials)</code></td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} 계정이 다른 IBM Cloud 인프라(SoftLayer) 포트폴리오에 액세스하기 위한 인프라 인증 정보를 제거합니다.</td>
<td><code>[ibmcloud ks credential-unset](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_unset)</code></td>
<td><code>[DELETE /v1/credentials](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/accounts/RemoveUserCredentials)</code></td>
</tr>
<tr>
<td>베타: {{site.data.keyword.keymanagementservicefull}}를 사용하여 Kubernetes 시크릿을 암호화합니다. </td>
<td><code>[ibmcloud ks key-protect-enable](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_messages)</code></td>
<td><code>[POST /v1/clusters/{idOrName}/kms](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/CreateKMSConfig)</code></td>
</tr>
<tr>
<td>Fluentd 클러스터 추가 기능에 대한 자동 업데이트를 사용 안함으로 설정합니다.</td>
<td><code>[ibmcloud ks logging-autoupdate-disable](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_autoupdate_disable)</code></td>
<td><code>[PUT /v1/logging/{idOrName}/updatepolicy](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/ChangeUpdatePolicy)</code></td>
</tr>
<tr>
<td>Fluentd 클러스터 추가 기능에 대한 자동 업데이트를 사용으로 설정합니다.</td>
<td><code>[ibmcloud ks logging-autoupdate-enable](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_autoupdate_enable)</code></td>
<td><code>[PUT /v1/logging/{idOrName}/updatepolicy](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/ChangeUpdatePolicy)</code></td>
</tr>
<tr>
<td>{{site.data.keyword.cos_full_notm}} 버킷의 API 서버 로그 스냅샷을 수집합니다.</td>
<td><code>[ibmcloud ks logging-collect](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_collect)</code></td>
<td>[POST /v1/log-collector/{idOrName}/masterlogs](https://containers.cloud.ibm.com/global/swagger-global-api/#/log45collector/CreateMasterLogCollection)</td>
</tr>
<tr>
<td>API 서버 로그 스냅샷 요청의 상태를 확인합니다.</td>
<td><code>[ibmcloud ks logging-collect-status](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_collect_status)</code></td>
<td>[GET /v1/log-collector/{idOrName}/masterlogs](https://containers.cloud.ibm.com/global/swagger-global-api/#/log45collector/GetMasterLogCollectionStatus)</td>
</tr>
<tr>
<td><code>kube-audit</code> 로그 소스에 대한 로그 전달 구성을 작성합니다.</td>
<td><code>[ibmcloud ks logging-config-create](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_logging_create)</code></td>
<td><code>[POST /v1/logging/{idOrName}/loggingconfig/{logSource}](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/CreateLoggingConfig)</code></td>
</tr>
<tr>
<td><code>kube-audit</code> 로그 소스에 대한 로그 전달 구성을 삭제합니다.</td>
<td><code>[ibmcloud ks logging-config-rm](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_logging_rm)</code></td>
<td><code>[DELETE /v1/logging/{idOrName}/loggingconfig/{logSource}/{id}](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/DeleteLoggingConfig)</code></td>
</tr>
</tbody>
</table>

<br />


## {{site.data.keyword.Bluemix_notm}} IAM 서비스 역할
{: #service}

{{site.data.keyword.Bluemix_notm}} IAM 서비스 액세스 역할이 지정된 모든 사용자에게는 특정 네임스페이스의 해당 Kubernetes 역할 기반 액세스 제어(RBAC) 역할도 자동으로 지정됩니다. 서비스 액세스 역할에 대해 자세히 알아보려면 [{{site.data.keyword.Bluemix_notm}} IAM 서비스 역할](/docs/containers?topic=containers-users#platform)을 참조하십시오. 서비스 역할과 동일한 시간에 {{site.data.keyword.Bluemix_notm}} IAM platform 역할을 지정하지 마십시오. 플랫폼 및 서비스 역할을 별도로 지정해야 합니다.
{: shortdesc}

각 서비스 역할이 RBAC를 통해 부여한 Kubernetes 조치를 찾으십니까? [RBAC 역할별 Kubernetes 리소스 권한](#rbac_ref)을 참조하십시오. RBAC 역할에 대해 자세히 알아보려면 [RBAC 권한 지정](/docs/containers?topic=containers-users#role-binding) 및 [클러스터 역할을 집계하여 기존 권한 확장](https://cloud.ibm.com/docs/containers?topic=containers-users#rbac_aggregate)을 참조하십시오.
{: tip}

다음 표는 각 서비스 역할 및 해당 RBAC 역할에 따라 부여되는 Kubernetes 리소스 권한을 보여줍니다. 

<table>
<caption>서비스 및 해당 RBAC 역할별 Kubernetes 리소스 권한</caption>
<thead>
    <th id="service-role">서비스 역할</th>
    <th id="rbac-role">해당 RBAC 역할, 바인딩 및 범위</th>
    <th id="kube-perm">Kubernetes 리소스 권한</th>
</thead>
<tbody>
  <tr>
    <td id="service-role-reader" headers="service-role">독자 역할</td>
    <td headers="service-role-reader rbac-role">하나의 네임스페이스로 범위 지정된 경우: 해당 네임스페이스에서 <strong><code>ibm-view</code></strong> 역할 바인딩에 의해 적용된 <strong><code>view</code></strong> 클러스터 역할</br><br>모든 네임스페이스로 범위 지정된 경우: 클러스터의 각 네임스페이스에서 <strong><code>ibm-view</code></strong> 역할 바인딩에 의해 적용된 <strong><code>view</code></strong> 클러스터 역할</td>
    <td headers="service-role-reader kube-perm"><ul>
      <li>네임스페이스의 리소스에 대한 읽기 액세스 권한</li>
      <li>역할과 역할 바인딩 또는 Kubernetes 시크릿에 대한 읽기 액세스 권한 없음</li>
      <li>Kubernetes 대시보드에 액세스하여 네임스페이스의 리소스 보기</li></ul>
    </td>
  </tr>
  <tr>
    <td id="service-role-writer" headers="service-role">작성자 역할</td>
    <td headers="service-role-writer rbac-role">하나의 네임스페이스로 범위 지정된 경우: 해당 네임스페이스에서 <strong><code>ibm-edit</code></strong> 역할 바인딩에 의해 적용된 <strong><code>edit</code></strong> 클러스터 역할</br><br>모든 네임스페이스로 범위 지정된 경우: 클러스터의 각 네임스페이스에서 <strong><code>ibm-edit</code></strong> 역할 바인딩에 의해 적용된 <strong><code>edit</code></strong> 클러스터 역할</td>
    <td headers="service-role-writer kube-perm"><ul><li>네임스페이스의 리소스에 대한 읽기/쓰기 액세스 권한</li>
    <li>역할과 역할 바인딩에 대한 읽기/쓰기 액세스 권한 없음</li>
    <li>Kubernetes 대시보드에 액세스하여 네임스페이스의 리소스 보기</li></ul>
    </td>
  </tr>
  <tr>
    <td id="service-role-manager" headers="service-role">관리자 역할</td>
    <td headers="service-role-manager rbac-role">하나의 네임스페이스로 범위 지정된 경우: 해당 네임스페이스에서 <strong><code>ibm-operate</code></strong> 역할 바인딩에 의해 적용된 <strong><code>admin</code></strong> 클러스터 역할</br><br>모든 네임스페이스로 범위 지정된 경우: <strong><code>ibm-admin</code></strong> 클러스터 역할 바인딩에 의해 적용된 <strong><code>cluster-admin</code></strong> 클러스터 역할</td> 모든 네임스페이스에 적용
    <td headers="service-role-manager kube-perm">하나의 네임스페이스로 범위 지정된 경우:
      <ul><li>네임스페이스의 모든 리소스에 대한 읽기/쓰기 액세스 권한이지만 리소스 할당량 또는 네임스페이스 자체에는 액세스할 수 없음</li>
      <li>네임스페이스에서 RBAC 역할 및 역할 바인딩 작성</li>
      <li>Kubernetes 대시보드에 액세스하여 네임스페이스의 모든 리소스 보기</li></ul>
    </br>모든 네임스페이스로 범위 지정된 경우:
        <ul><li>모든 네임스페이스의 모든 리소스에 대한 읽기/쓰기 액세스 권한</li>
        <li>모든 네임스페이스에서 네임스페이스 또는 클러스터 역할 및 클러스터 역할 바인딩에 RBAC 역할 및 역할 바인딩 작성</li>
        <li>Kubernetes 대시보드에 액세스</li>
        <li>앱을 공용으로 사용할 수 있도록 하는 Ingress 리소스 작성</li>
        <li><code>kubectl top pods</code>, <code>kubectl top nodes</code> 또는 <code>kubectl get nodes</code> 명령 등을 사용하여 클러스터 메트릭 검토</li></ul>
    </td>
  </tr>
</tbody>
</table>

<br />


## RBAC 역할별 Kubernetes 리소스 권한
{: #rbac_ref}

{{site.data.keyword.Bluemix_notm}} IAM 서비스 액세스 역할이 지정된 모든 사용자에게는 사전 정의된 해당 Kubernetes 역할 기반 액세스 제어(RBAC) 역할도 자동으로 지정됩니다. 자체 사용자 정의 Kubernetes RBAC 권한을 관리하려는 경우, [사용자, 그룹 또는 서비스 계정에 대한 사용자 정의 RBAC 권한 작성](/docs/containers?topic=containers-users#rbac)을 참조하십시오.
{: shortdesc}

네임스페이스의 리소스에서 특정 `kubectl` 명령을 실행할 수 있는 올바른 권한이 있는지 여부가 궁금하십니까? [`kubectl auth can-i` 명령 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#-em-can-i-em-)을 시도하십시오.
{: tip}

다음 표에는 개별 Kubernetes 리소스에 각 RBAC 역할에 의해 부여된 권한이 표시되어 있습니다. 권한은 해당 역할이 있는 사용자가 리소스에 대해 "get", "list", "describe", "create" 또는 "delete"와 같이 완료할 수 있는 동사로 표시됩니다.

<table>
 <caption>사전 정의된 RBAC 역할별로 부여된 Kubernetes 리소스 권한</caption>
 <thead>
  <th>Kubernetes 리소스</th>
  <th><code>view</code></th>
  <th><code>edit</code></th>
  <th><code>admin</code> 및 <code>cluster-admin</code></th>
 </thead>
<tbody>
<tr>
  <td><code>bindings</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
</tr><tr>
  <td><code>configmaps</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>cronjobs.batch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>daemonsets.apps </code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>daemonsets.extensions</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>deployments.apps</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>deployments.apps/rollback</code></td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>deployments.apps/scale</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>deployments.extensions</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>deployments.extensions/rollback</code></td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>deployments.extensions/scale</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>endpoints</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>events</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
</tr><tr>
  <td><code>horizontalpodautoscalers.autoscaling</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>ingresses.extensions</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>jobs.batch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>limitranges</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
</tr><tr>
  <td><code>localsubjectaccessreviews</code></td>
  <td>-</td>
  <td>-</td>
  <td><code>create</code></td>
</tr><tr>
  <td><code>namespaces</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></br>**cluster-admin 전용:** <code>create</code>, <code>delete</code></td>
</tr><tr>
  <td><code>namespaces/status</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
</tr><tr>
  <td><code>networkpolicies</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>networkpolicies.extensions</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>persistentvolumeclaims</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>poddisruptionbudgets.policy</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>pods</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>top</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>pods/attach</code></td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>pods/exec</code></td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>pods/log</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
</tr><tr>
  <td><code>pods/portforward</code></td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>pods/proxy</code></td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>pods/status</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
</tr><tr>
  <td><code>replicasets.apps</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>replicasets.apps/scale</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>replicasets.extensions</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>replicasets.extensions/scale</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>replicationcontrollers</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>replicationcontrollers/scale</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>replicationcontrollers/status</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
</tr><tr>
  <td><code>replicationcontrollers.extensions/scale</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>resourcequotas</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
</tr><tr>
  <td><code>resourcequotas/status</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
</tr><tr>
  <td><code>rolebindings</code></td>
  <td>-</td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>roles</code></td>
  <td>-</td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>시크릿</code></td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>serviceaccounts</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code>, <code>impersonate</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code>, <code>impersonate</code></td>
</tr><tr>
  <td><code>서비스</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>services/proxy</code></td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>statefulsets.apps</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>statefulsets.apps/scale</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr>
</tbody>
</table>

<br />


## Cloud Foundry 역할
{: #cloud-foundry}

Cloud Foundry 역할은 계정 내 조직 및 영역에 대한 액세스 권한을 부여합니다. {{site.data.keyword.Bluemix_notm}}에 있는 Cloud Foundry 기반 서비스의 목록을 보려면 `ibmcloud service list`를 실행하십시오. 보다 자세히 알아보려면 {{site.data.keyword.Bluemix_notm}} IAM 문서에서 사용 가능한 모든 [조직 및 영역 역할](/docs/iam?topic=iam-cfaccess) 또는 [Cloud Foundry 액세스 권한 관리](/docs/iam?topic=iam-mngcf) 단계를 참조하십시오.
{: shortdesc}

다음 표는 클러스터 조치 권한에 필요한 Cloud Foundry 역할을 보여줍니다. 

<table>
  <caption>Cloud Foundry 역할별 클러스터 관리 권한</caption>
  <thead>
    <th>Cloud Foundry 역할</th>
    <th>클러스터 관리 권한</th>
  </thead>
  <tbody>
  <tr>
    <td>영역 역할: 관리자</td>
    <td>{{site.data.keyword.Bluemix_notm}} 영역에 대한 사용자 액세스 관리</td>
  </tr>
  <tr>
    <td>영역 역할: 개발자</td>
    <td>
      <ul><li>{{site.data.keyword.Bluemix_notm}} 서비스 인스턴스 작성</li>
      <li>{{site.data.keyword.Bluemix_notm}} 서비스 인스턴스를 클러스터에 바인드</li>
      <li>영역 레벨에서 클러스터의 로그 전달 구성의 로그 보기</li></ul>
    </td>
  </tr>
  </tbody>
</table>

## 인프라 역할
{: #infra}

**수퍼유저** 인프라 액세스 역할이 있는 사용자는 해당 인프라 조치를 수행할 수 있도록 [지역 및 리소스 그룹에 대한 API 키를 설정](/docs/containers?topic=containers-users#api_key)(더 드물게는 [다른 계정 인증 정보를 수동으로 설정](/docs/containers?topic=containers-users#credentials))합니다. 그 후에는 계정에 있는 다른 사용자가 수행할 수 있는 인프라 조치가 {{site.data.keyword.Bluemix_notm}} IAM 플랫폼 역할을 통해 권한 부여됩니다. 사용자가 다른 사용자의 IBM Cloud 인프라(SoftLayer) 권한을 편집할 필요는 없습니다. API 키를 설정하는 사용자에게 **수퍼유저**를 지정할 수 없는 경우에만 다음 표를 사용하여 사용자의 IBM Cloud 인프라(SoftLayer) 권한을 사용자 정의하십시오. 권한 지정에 대한 지시사항은 [인프라 권한 사용자 정의](/docs/containers?topic=containers-users#infra_access)를 참조하십시오.
{: shortdesc}



다음 표는 일반적인 태스크 그룹을 완료하는 데 필요한 인프라 권한을 보여줍니다. 

<table>
<caption>{{site.data.keyword.containerlong_notm}}에 공통으로 필요한 인프라 권한</caption>
<thead>
  <th>{{site.data.keyword.containerlong_notm}}의 공통 태스크</th>
  <th>카테고리에서 필요한 인프라 권한</th>
</thead>
<tbody>
<tr>
<td>
  <strong>최소 권한</strong>: <ul>
  <li>클러스터 작성</li></ul></td>
<td>
<strong>계정</strong>: <ul>
<li>서버 추가</li></ul>
  <strong>디바이스</strong>:<ul>
  <li>베어메탈 작업자 노드의 경우: 하드웨어 세부사항 보기</li>
  <li>IPMI 원격 관리</li>
  <li>OS 다시 로드 및 복구 커널</li>
  <li>VM 작업자 노드의 경우: 가상 서버 세부사항 보기</li></ul></td>
</tr>
<tr>
<td>
<strong>클러스터 관리</strong>:<ul>
  <li>클러스터 작성, 업데이트 및 삭제</li>
  <li>작업자 노드 추가, 다시 로드 및 다시 부팅</li>
  <li>VLAN 보기</li>
  <li>서브넷 작성</li>
  <li>팟(Pod) 및 로드 밸런서 서비스 배치</li></ul>
  </td><td>
<strong>계정</strong>:<ul>
  <li>서버 추가</li>
  <li>서버 취소</li></ul>
<strong>디바이스</strong>:<ul>
  <li>베어메탈 작업자 노드의 경우: 하드웨어 세부사항 보기</li>
  <li>IPMI 원격 관리</li>
  <li>OS 다시 로드 및 복구 커널</li>
  <li>VM 작업자 노드의 경우: 가상 서버 세부사항 보기</li></ul>
<strong>네트워크</strong>:<ul>
  <li>공용 네트워크 포트로 컴퓨팅 추가</li></ul>
<p class="important">사용자에게 지원 케이스를 관리할 수 있는 기능 또한 지정해야 합니다. [인프라 권한 사용자 정의](/docs/containers?topic=containers-users#infra_access)의 8단계를 참조하십시오. </p>
</td>
</tr>
<tr>
<td>
  <strong>스토리지</strong>: <ul>
  <li>지속적 볼륨 클레임을 작성하여 지속적 볼륨 프로비저닝</li>
  <li>스토리지 인프라 리소스 작성 및 관리</li></ul></td>
<td>
<strong>계정</strong>:<ul>
  <li>스토리지(StorageLayer) 추가/업그레이드</li></ul>
<strong>서비스</strong>:<ul>
  <li>스토리지 관리</li></ul></td>
</tr>
<tr>
<td>
  <strong>사설 네트워크</strong>: <ul>
  <li>클러스터 내부 네트워킹을 위한 사설 VLAN 관리</li>
  <li>VPN 연결을 사설 네트워크에 설정</li></ul></td>
<td>
  <strong>네트워크</strong>:<ul>
  <li>네트워크 서브넷 라우트 관리</li></ul></td>
</tr>
<tr>
<td>
  <strong>사설 네트워킹</strong>:<ul>
  <li>공용 로드 밸런서 또는 Ingress 네트워킹을 설정하여 앱 노출</li></ul></td>
<td>
<strong>디바이스</strong>:<ul>
<li>포트 제어 관리</li>
  <li>호스트 이름/도메인 편집</li></ul>
<strong>네트워크</strong>:<ul>
  <li>IP 주소 추가</li>
  <li>네트워크 서브넷 라우트 관리</li>
  <li>공용 네트워크 포트로 컴퓨팅 추가</li></ul>
<strong>서비스</strong>:<ul>
  <li>DNS 관리</li>
  <li>인증서(SSL) 보기</li>
  <li>인증서(SSL) 관리</li></ul></td>
</tr>
</tbody>
</table>
