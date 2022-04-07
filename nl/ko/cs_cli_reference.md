---

copyright:
  years: 2014, 2019
lastupdated: "2019-04-18"

keywords: kubernetes, iks, ibmcloud, ic, ks

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


# IBM Cloud Kubernetes Service CLI
{: #cs_cli_reference}

{{site.data.keyword.containerlong}}에서 Kubernetes 클러스터를 작성하고 관리하려면 다음 명령을 참조하십시오.
{:shortdesc}

CLI 플러그인을 설치하려면 [CLI 설치](/docs/containers?topic=containers-cs_cli_install#cs_cli_install_steps)를 참조하십시오.

터미널에서 `ibmcloud` CLI 및 플러그인에 대한 업데이트가 사용 가능한 시점을 사용자에게 알려줍니다. 사용 가능한 모든 명령과 플래그를 사용할 수 있도록 반드시 CLI를 최신 상태로 유지하십시오.

`ibmcloud cr` 명령을 찾고 계십니까? [{{site.data.keyword.registryshort_notm}} CLI 참조](/docs/services/Registry?topic=registry-registry_cli_reference#registry_cli_reference)를 확인하십시오. `kubectl` 명령을 찾고 계십니까? [Kubernetes 문서 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubectl.docs.kubernetes.io/)를 참조하십시오.
{:tip}


## 베타 {{site.data.keyword.containerlong_notm}} 플러그인 사용
{: #cs_beta}

다시 디자인된 버전의 {{site.data.keyword.containerlong_notm}} 플러그인을 베타로서 사용할 수 있습니다. 다시 디자인된 {{site.data.keyword.containerlong_notm}} 플러그인은 명령을 카테고리로 그룹화하고 하이픈으로 연결된 구조에서 명령을 일정 간격이 있는 구조로 변경합니다.
{: shortdesc}

다시 디자인된 {{site.data.keyword.containerlong_notm}} 플러그인을 사용하려면 `IKS_BETA_VERSION` 환경 변수를 사용하려는 베타 버전으로 설정하십시오.

```
export IKS_BETA_VERSION=<beta_version>
```
{: pre}

다시 디자인된 {{site.data.keyword.containerlong_notm}} 플러그인의 다음 베타 버전을 사용할 수 있습니다. 기본 작동은 `0.2`입니다.

<table>
<caption>다시 디자인된 {{site.data.keyword.containerlong_notm}} 플러그인의 베타 버전</caption>
  <thead>
    <th>베타 버전</th>
    <th>`ibmcloud ks help` 출력 구조</th>
    <th>명령 구조</th>
  </thead>
  <tbody>
    <tr>
      <td><code>0.2</code>(기본값)</td>
      <td>레거시: 명령은 하이픈으로 연결된 구조로 표시되며 알파벳순으로 나열됩니다.</td>
      <td>레거시 및 베타: 레거시 하이픈으로 연결된 구조(`ibmcloud ks alb-cert-get`) 또는 베타 간격이 있는 구조(`ibmcloud ks alb cert get`)의 명령을 실행할 수 있습니다.</td>
    </tr>
    <tr>
      <td><code>0.3</code></td>
      <td>베타: 명령은 간격이 있는 구조로 표시되며 카테고리로 나열됩니다.</td>
      <td>레거시 및 베타: 레거시 하이픈으로 연결된 구조(`ibmcloud ks alb-cert-get`) 또는 베타 간격이 있는 구조(`ibmcloud ks alb cert get`)의 명령을 실행할 수 있습니다.</td>
    </tr>
    <tr>
      <td><code>1.0</code></td>
      <td>베타: 명령은 간격이 있는 구조로 표시되며 카테고리로 나열됩니다.</td>
      <td>베타: 베타 간격이 있는 구조(`ibmcloud ks alb cert get`)의 명령만 실행할 수 있습니다.</td>
    </tr>
  </tbody>
</table>



## ibmcloud ks 명령
{: #cs_commands}

**팁:** {{site.data.keyword.containerlong_notm}} 플러그인의 버전을 보려면 다음 명령을 실행하십시오.

```
ibmcloud plugin list
```
{: pre}

<table summary="API 명령 표">
<caption>API 명령</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>API 명령</th>
 </thead>
 <tbody>
  <tr>
    <td>[ibmcloud ks api
](#cs_cli_api)</td>
    <td>[ibmcloud ks api-key-info](#cs_api_key_info)</td>
    <td>[ibmcloud ks api-key-reset](#cs_api_key_reset)</td>
    <td>[ibmcloud ks apiserver-config-get](#cs_apiserver_config_get)</td>
  </tr>
  <tr>
    <td>[ibmcloud ks apiserver-config-set](#cs_apiserver_config_set)</td>
    <td>[ibmcloud ks apiserver-config-unset](#cs_apiserver_config_unset)</td>
    <td>[ibmcloud ks apiserver-refresh](#cs_apiserver_refresh)</td>
    <td></td>
 </tr>
</tbody>
</table>

<br>

<table summary="CLI 플러그인 사용법 명령 표">
<caption>CLI 플러그인 사용법 명령</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>CLI 플러그인 사용법 명령</th>
 </thead>
 <tbody>
  <tr>
    <td>[ibmcloud ks help](#cs_help)</td>
    <td>[ibmcloud ks init](#cs_init)</td>
    <td>[ibmcloud ks messages](#cs_messages)</td>
    <td></td>
  </tr>
</tbody>
</table>

<br>

<table summary="클러스터 명령: 관리 표">
<caption>클러스터 명령: 관리 명령</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>클러스터 명령: 관리</th>
 </thead>
 <tbody>
  <tr>
    <td>[ibmcloud ks addon-versions](#cs_addon_versions)</td>
    <td>[ibmcloud ks cluster-addon-disable](#cs_cluster_addon_disable)</td>
    <td>[ibmcloud ks cluster-addon-enable](#cs_cluster_addon_enable)</td>
    <td>[ibmcloud ks cluster-addons](#cs_cluster_addons)</td>
  </tr>
  <tr>
  <td>[ibmcloud ks cluster-config](#cs_cluster_config)</td>
    <td>[ibmcloud ks cluster-create](#cs_cluster_create)</td>
    <td>[ibmcloud ks cluster-feature-disable](#cs_cluster_feature_disable)</td>
    <td>[ibmcloud ks cluster-feature-enable](#cs_cluster_feature_enable)</td>
  </tr>
  <tr>
  <td>[ibmcloud ks cluster-get](#cs_cluster_get)</td>
    <td>[ibmcloud ks cluster-pull-secret-apply](#cs_cluster_pull_secret_apply)</td>
    <td>[ibmcloud ks cluster-refresh](#cs_cluster_refresh)</td>
    <td>[ibmcloud ks cluster-rm](#cs_cluster_rm)</td>
  </tr>
  <tr>
  <td>[ibmcloud ks cluster-update](#cs_cluster_update)</td>
    <td>[ibmcloud ks clusters](#cs_clusters)</td>
    <td>[ibmcloud ks kube-versions](#cs_kube_versions)</td>
    <td> </td>
  </tr>
</tbody>
</table>

<br>

<table summary="클러스터 명령: 서비스 및 통합 표">
<caption>클러스터 명령: 서비스 및 통합 명령</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>클러스터 명령: 서비스 및 통합</th>
 </thead>
 <tbody>
  <tr>
    <td>[ibmcloud ks cluster-service-bind](#cs_cluster_service_bind)</td>
    <td>[ibmcloud ks cluster-service-unbind](#cs_cluster_service_unbind)</td>
    <td>[ibmcloud ks cluster-services](#cs_cluster_services)</td>
    <td>[ibmcloud ks va](#cs_va)</td>
  </tr>
    <td>[ibmcloud ks webhook-create](#cs_webhook_create)</td>
  <tr>
  </tr>
</tbody>
</table>

</br>

<table summary="클러스터 명령: 서브넷 표">
<caption>클러스터 명령: 서브넷 명령</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>클러스터 명령: 서브넷</th>
 </thead>
 <tbody>
  <tr>
    <td>[ibmcloud ks cluster-subnet-add](#cs_cluster_subnet_add)</td>
    <td>[ibmcloud ks cluster-subnet-create](#cs_cluster_subnet_create)</td>
    <td>[ibmcloud ks cluster-user-subnet-add](#cs_cluster_user_subnet_add)</td>
    <td>[ibmcloud ks cluster-user-subnet-rm](#cs_cluster_user_subnet_rm)</td>
  </tr>
  <tr>
    <td>[ibmcloud ks subnets](#cs_subnets)</td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
</tbody>
</table>

</br>

<table summary="인프라 명령 표">
<caption>클러스터 명령: 인프라 명령</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>인프라 명령</th>
 </thead>
 <tbody>
  <tr>
    <td>[ibmcloud ks credential-get](#cs_credential_get)</td>
    <td>[ibmcloud ks credential-set](#cs_credentials_set)</td>
    <td>[ibmcloud ks credential-unset](#cs_credentials_unset)</td>
    <td>[ibmcloud ks machine-types](#cs_machine_types)</td>
  </tr>
  <tr>
    <td>[ibmcloud ks vlans](#cs_vlans)</td>
    <td>[ibmcloud ks vlan-spanning-get](#cs_vlan_spanning_get)</td>
    <td> </td>
    <td> </td>
  </tr>
</tbody>
</table>

</br>

<table summary="Ingress 애플리케이션 로드 밸런서(ALB) 명령 표">
<caption>Ingress 애플리케이션 로드 밸런서(ALB) 명령</caption>
<col width = 25%>
<col width = 25%>
<col width = 25%>
  <thead>
    <tr>
      <th colspan=4>Ingress 애플리케이션 로드 밸런서(ALB) 명령</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>[ibmcloud ks alb-autoupdate-disable](#cs_alb_autoupdate_disable)</td>
      <td>[ibmcloud ks alb-autoupdate-enable](#cs_alb_autoupdate_enable)</td>
      <td>[ibmcloud ks alb-autoupdate-get](#cs_alb_autoupdate_get)</td>
      <td>[ibmcloud ks alb-cert-deploy](#cs_alb_cert_deploy)</td>
    </tr>
    <tr>
      <td>[ibmcloud ks alb-cert-get](#cs_alb_cert_get)</td>
      <td>[ibmcloud ks alb-cert-rm](#cs_alb_cert_rm)</td>
      <td>[ibmcloud ks alb-certs](#cs_alb_certs)</td>
      <td>[ibmcloud ks alb-configure](#cs_alb_configure)</td>
    </tr>
    <tr>
      <td>[ibmcloud ks alb-get](#cs_alb_get)</td>
      <td>[ibmcloud ks alb-rollback](#cs_alb_rollback)</td>
      <td>[ibmcloud ks alb-types](#cs_alb_types)</td>
      <td>[ibmcloud ks alb-update](#cs_alb_update)</td>
    </tr>
    <tr>
      <td>[ibmcloud ks albs](#cs_albs)</td>
      <td> </td>
      <td> </td>
      <td> </td>
    </tr>
  </tbody>
</table>

</br>

<table summary="로깅 명령 표">
<caption>로깅 명령</caption>
<col width = 25%>
<col width = 25%>
<col width = 25%>
  <thead>
    <tr>
      <th colspan=4>로깅 명령</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>[ibmcloud ks logging-autoupdate-enable](#cs_log_autoupdate_enable)</td>
      <td>[ibmcloud ks logging-autoupdate-disable](#cs_log_autoupdate_disable)</td>
      <td>[ibmcloud ks logging-autoupdate-get](#cs_log_autoupdate_get)</td>
      <td>[ibmcloud ks logging-config-create](#cs_logging_create)</td>
    </tr>
    <tr>
      <td>[ibmcloud ks logging-config-get](#cs_logging_get)</td>
      <td>[ibmcloud ks logging-config-refresh](#cs_logging_refresh)</td>
      <td>[ibmcloud ks logging-config-rm](#cs_logging_rm)</td>
      <td>[ibmcloud ks logging-config-update](#cs_logging_update)</td>
    </tr>
    <tr>
      <td>[ibmcloud ks logging-filter-create](#cs_log_filter_create)</td>
      <td>[ibmcloud ks logging-filter-update](#cs_log_filter_update)</td>
      <td>[ibmcloud ks logging-filter-get](#cs_log_filter_view)</td>
      <td>[ibmcloud ks logging-filter-rm](#cs_log_filter_delete)</td>
    </tr>
    <tr>
      <td>[ibmcloud ks logging-collect](#cs_log_collect)</td>
      <td>[ibmcloud ks logging-collect-status](#cs_log_collect_status)</td>
      <td> </td>
      <td> </td>
    </tr>
  </tbody>
</table>

</br>

<table summary="네트워크 로드 밸런서(NLB) 명령 테이블">
<caption>네트워크 로드 밸런서(NLB) 명령</caption>
<col width = 25%>
<col width = 25%>
<col width = 25%>
  <thead>
    <tr>
      <th colspan=4>네트워크 로드 밸런서(NLB) 명령</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>[ibmcloud ks nlb-dns-add](#cs_nlb-dns-add)</td>
      <td>[ibmcloud ks nlb-dns-create](#cs_nlb-dns-create)</td>
      <td>[ibmcloud ks nlb-dnss](#cs_nlb-dns-ls)</td>
      <td>[ibmcloud ks nlb-dns-rm](#cs_nlb-dns-rm)</td>
    </tr>
    <tr>
      <td>[ibmcloud ks nlb-dns-monitor-configure](#cs_nlb-dns-monitor-configure)</td>
      <td>[ibmcloud ks nlb-dns-monitor-get](#cs_nlb-dns-monitor-get)</td>
      <td>[ibmcloud ks nlb-dns-monitor-disable](#cs_nlb-dns-monitor-disable)</td>
      <td>[ibmcloud ks nlb-dns-monitor-enable](#cs_nlb-dns-monitor-enable)</td>
    </tr>
    <tr>
      <td>[ibmcloud ks nlb-dns-monitor-ls](#cs_nlb-dns-monitor-ls)</td>
      <td>[ibmcloud ks nlb-dns-monitor-status](#cs_nlb-dns-monitor-status)</td>
      <td> </td>
      <td> </td>
    </tr>
  </tbody>
</table>

</br>

<table summary="지역 명령 표">
<caption>지역 명령</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>지역 명령</th>
 </thead>
 <tbody>
  <tr>
    <td>[ibmcloud ks region](#cs_region)</td>
    <td>[ibmcloud ks region-set](#cs_region-set)</td>
    <td>[ibmcloud ks regions](#cs_regions)</td>
  <td>[ibmcloud ks zones](#cs_datacenters)</td>
  </tr>
</tbody>
</table>

</br>

<table summary="작업자 노드 명령 표">
<caption>작업자 노드 명령</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>작업자 노드 명령</th>
 </thead>
 <tbody>
    <tr>
      <td>더 이상 사용되지 않음: [ibmcloud ks worker-add](#cs_worker_add)</td>
      <td>[ibmcloud ks worker-get](#cs_worker_get)</td>
      <td>[ibmcloud ks worker-reboot](#cs_worker_reboot)</td>
      <td>[ibmcloud ks worker-reload](#cs_worker_reload)</td>
    </tr>
    <tr>
      <td>[ibmcloud ks worker-rm](#cs_worker_rm)</td>
      <td>[ibmcloud ks worker-update](#cs_worker_update)</td>
      <td>[ibmcloud ks workers](#cs_workers)</td>
      <td> </td>
    </tr>
  </tbody>
</table>

<table summary="작업자 풀 명령 테이블">
<caption>작업자 풀 명령</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>작업자 풀 명령</th>
 </thead>
 <tbody>
    <tr>
      <td>[ibmcloud ks worker-pool-create](#cs_worker_pool_create)</td>
      <td>[ibmcloud ks worker-pool-get](#cs_worker_pool_get)</td>
      <td>[ibmcloud ks worker-pool-rebalance](#cs_rebalance)</td>
      <td>[ibmcloud ks worker-pool-resize](#cs_worker_pool_resize)</td>
    </tr>
    <tr>
      <td>[ibmcloud ks worker-pool-rm](#cs_worker_pool_rm)</td>
      <td>[ibmcloud ks worker-pools](#cs_worker_pools)</td>
      <td>[ibmcloud ks zone-add](#cs_zone_add)</td>
      <td>[ibmcloud ks zone-network-set](#cs_zone_network_set)</td>
    </tr>
    <tr>
     <td>[ibmcloud ks zone-rm](#cs_zone_rm)</td>
     <td></td>
    </tr>
  </tbody>
</table>

## API 명령
{: #api_commands}

### ibmcloud ks api
{: #cs_cli_api}

{{site.data.keyword.containerlong_notm}}에 대한 API 엔드포인트를 대상으로 지정합니다. 엔드포인트를 지정하지 않으면 대상으로 지정된 현재 엔드포인트에 대한 정보를 볼 수 있습니다.
{: shortdesc}

지원되는 엔드포인트는 다음과 같습니다.
* 글로벌: `https://containers.cloud.ibm.com`
* 달라스(미국 남부, us-south): `https://us-south.containers.cloud.ibm.com`
* 프랑크푸르트(중앙 유럽, eu-de): `https://eu-central.containers.cloud.ibm.com`
* 런던(영국 남부, eu-gb): `https://uk-south.containers.cloud.ibm.com`
* 시드니(AP 남부, au-syd): `https://ap-south.containers.cloud.ibm.com`
* 도쿄(AP 중부, jp-tok): `https://ap-north.containers.cloud.ibm.com`
* 워싱턴, D.C.(미국 동부, us-east): `https://us-east.containers.cloud.ibm.com`

```
ibmcloud ks api --endpoint ENDPOINT [--insecure] [--skip-ssl-validation] [--api-version VALUE] [-s]
```
{: pre}

<strong>최소 필요 권한</strong>: 없음

<strong>명령 옵션</strong>:

   <dl>
   <dt><code>--endpoint <em>ENDPOINT</em></code></dt>
   <dd>{{site.data.keyword.containerlong_notm}} API 엔드포인트입니다. 이 엔드포인트는 {{site.data.keyword.Bluemix_notm}} 엔드포인트와 다릅니다. 이 값은 API 엔드포인트를 설정하는 데 필요합니다.
   </dd>

   <dt><code>--insecure</code></dt>
   <dd>비보안 HTTP 연결을 허용합니다. 이 플래그는 선택사항입니다.</dd>

   <dt><code>--skip-ssl-validation</code></dt>
   <dd>비보안 SSL 인증서를 허용합니다. 이 플래그는 선택사항입니다.</dd>

   <dt><code>--api-version VALUE</code></dt>
   <dd>사용할 서비스의 API 버전을 지정합니다.  이 값은 선택사항입니다.</dd>

   <dt><code>-s</code></dt>
   <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>

   </dl>

**예**: 대상으로 지정된 현재 API 엔드포인트에 대한 정보를 봅니다.
```
ibmcloud ks api
```
{: pre}

```
API Endpoint:          https://containers.cloud.ibm.com
API Version:           v1
Skip SSL Validation:   false
Region:                us-south
```
{: screen}


### ibmcloud ks api-key-info
{: #cs_api_key_info}

{{site.data.keyword.containerlong_notm}} 리소스 그룹 및 지역에 있는 {{site.data.keyword.Bluemix_notm}} IAM(Identity and Access Management) API 키의 소유자에 대한 이름과 이메일 주소를 봅니다.
{: shortdesc}

```
ibmcloud ks api-key-info --cluster CLUSTER [--json] [-s]
```
{: pre}

{{site.data.keyword.Bluemix_notm}} API 키는 {{site.data.keyword.containerlong_notm}} 관리자 액세스 정책이 필요한 첫 번째 조치가 수행될 때 리소스 그룹 및 지역에 대해 자동으로 설정됩니다. 예를 들어, 관리자 중 한 명이 `us-south` 지역의 `default` 리소스 그룹에 첫 번째 클러스터를 작성합니다. 이를 수행하면 이 사용자의 {{site.data.keyword.Bluemix_notm}} IAM API 키가 이 리소스 그룹 및 지역에 대해 계정에 저장됩니다. API 키는 새 작업자 노드 또는 VLAN과 같은 IBM Cloud 인프라(SoftLayer)에서 리소스를 정렬하는 데 사용됩니다. 리소스 그룹 내 각 지역에 대해서는 다른 API 키를 설정할 수 있습니다.

다른 사용자가 이 리소스 그룹 및 지역에서 새 클러스터 작성 또는 작업자 노드 다시 로드와 같이 IBM Cloud 인프라(SoftLayer) 포트폴리오와의 상호작용이 필요한 조치를 수행하는 경우, 저장된 API 키가 해당 조치를 수행하는 데 충분한 권한이 있는지 판별하는 데 사용됩니다. 클러스터의 인프라 관련 조치를 수행할 수 있는지 확인하려면 {{site.data.keyword.containerlong_notm}} 관리 사용자에게 **수퍼유저** 인프라 액세스 정책을 지정하십시오. 자세한 정보는 [사용자 액세스 관리](/docs/containers?topic=containers-users#infra_access)를 참조하십시오.

리소스 그룹 및 지역에 대해 저장된 API 키를 업데이트해야 한다고 판단하는 경우에는 [ibmcloud ks api-key-reset](#cs_api_key_reset) 명령을 사용하여 이를 수행할 수 있습니다. 이 명령은 {{site.data.keyword.containerlong_notm}} 관리자 액세스 정책이 필요하고 계정에서 이 명령을 실행하는 사용자의 API 키를 저장합니다.

**팁:** IBM Cloud 인프라(SoftLayer) 인증 정보가 [ibmcloud ks credential-set](#cs_credentials_set) 명령을 사용하여 수동으로 설정된 경우에는 이 명령에서 리턴된 API 키가 사용되지 않을 수 있습니다.

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **뷰어** 플랫폼 역할

<strong>명령 옵션</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

   <dt><code>--json</code></dt>
   <dd>명령 출력을 JSON 형식으로 인쇄합니다.  이 값은 선택사항입니다.</dd>

   <dt><code>-s</code></dt>
   <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>

   </dl>

**예제**:

  ```
  ibmcloud ks api-key-info --cluster my_cluster
  ```
  {: pre}


###         ibmcloud ks api-key-reset
{: #cs_api_key_reset}

{{site.data.keyword.containerlong_notm}} 지역의 현재 {{site.data.keyword.Bluemix_notm}} IAM API 키를 대체합니다.
{: shortdesc}

```
ibmcloud ks api-key-reset [-s]
```
{: pre}

이 명령은 {{site.data.keyword.containerlong_notm}} 관리자 액세스 정책이 필요하고 계정에서 이 명령을 실행하는 사용자의 API 키를 저장합니다. {{site.data.keyword.Bluemix_notm}} IAM API 키는 IBM Cloud 인프라(SoftLayer) 포트폴리오에서 인프라를 주문하는 데 필요합니다. 저장되면, API 키는 이 명령을 실행하는 사용자와 무관하게 인프라 권한이 필요한 지역의 모든 조치에 사용됩니다. {{site.data.keyword.Bluemix_notm}} IAM API 키의 작동 방법에 대한 자세한 정보는 [`ibmcloud ks api-key-info` 명령](#cs_api_key_info)을 참조하십시오.

이 명령을 시작하기 전에 이 명령을 실행하는 사용자가 필수 [{{site.data.keyword.containerlong_notm}} 및 IBM Cloud 인프라(SoftLayer) 권한](/docs/containers?topic=containers-users#users)을 보유하고 있는지 확인하십시오.
{: important}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **관리자** 플랫폼 역할

<strong>명령 옵션</strong>:

   <dl>
   <dt><code>-s</code></dt>
   <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>
   </dl>


**예제**:

  ```
  ibmcloud ks api-key-reset
  ```
  {: pre}



### ibmcloud ks apiserver-config-get
{: #cs_apiserver_config_get}

클러스터의 Kubernetes API 서버 구성 옵션에 대한 정보를 가져옵니다. 이 명령은 정보를 원하는 구성 옵션에 대한 다음 하위 명령 중 하나와 결합되어야 합니다.
{: shortdesc}

#### ibmcloud ks apiserver-config-get audit-webhook
{: #cs_apiserver_config_get_audit_webhook}

API 서버 감사 로그를 전송 중인 원격 로깅 서비스의 URL을 봅니다. URL은 API 서버 구성을 위한 웹훅 백엔드를 작성할 때 지정되었습니다.
{: shortdesc}

```
ibmcloud ks apiserver-config-get audit-webhook --cluster CLUSTER
```
{: pre}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **뷰어** 플랫폼 역할

<strong>명령 옵션</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>
   </dl>

**예제**:

  ```
  ibmcloud ks apiserver-config-get audit-webhook --cluster my_cluster
  ```
  {: pre}


### ibmcloud ks apiserver-config-set
{: #cs_apiserver_config_set}

클러스터의 Kubernetes API 서버 구성에 대한 한 옵션을 설정합니다. 이 명령은 설정할 구성 옵션에 대한 다음 하위 명령 중 하나와 결합되어야 합니다.
{: shortdesc}

#### ibmcloud ks apiserver-config-set audit-webhook
{: #cs_apiserver_set}

API 서버 구성을 위한 웹훅 백엔드를 설정합니다. 웹훅 백엔드는 API 서버 감사 로그를 원격 서버로 전달합니다. 웹훅 구성은 사용자가 이 명령의 플래그에 제공하는 정보를 기반으로 작성됩니다. 이 플래그에 정보를 제공하지 않은 경우에는 기본 웹훅 구성이 사용됩니다.
{: shortdesc}

```
ibmcloud ks apiserver-config-set audit-webhook --cluster CLUSTER [--remoteServer SERVER_URL_OR_IP] [--caCert CA_CERT_PATH] [--clientCert CLIENT_CERT_PATH] [--clientKey CLIENT_KEY_PATH]
```
{: pre}

웹훅을 설정하면 `ibmcloud ks apiserver-refresh` 명령을 실행하여 Kubernetes 마스터에 변경사항을 적용해야 합니다.
{: note}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **편집자** 플랫폼 역할

<strong>명령 옵션</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

   <dt><code>--remoteServer <em>SERVER_URL</em></code></dt>
   <dd>감사 로그를 전송할 원격 로깅 서비스의 URL 또는 IP 주소입니다. 비보안 서버 URL을 제공하는 경우에는 모든 인증서가 무시됩니다.  이 값은 선택사항입니다.</dd>

   <dt><code>--caCert <em>CA_CERT_PATH</em></code></dt>
   <dd>원격 로깅 서비스를 확인하는 데 사용되는 CA 인증서의 파일 경로입니다.  이 값은 선택사항입니다.</dd>

   <dt><code>--clientCert <em>CLIENT_CERT_PATH</em></code></dt>
   <dd>원격 로깅 서비스에 대해 인증하는 데 사용되는 클라이언트 인증서의 파일 경로입니다.  이 값은 선택사항입니다.</dd>

   <dt><code>--clientKey <em> CLIENT_KEY_PATH</em></code></dt>
   <dd>원격 로깅 서비스에 연결하는 데 사용되는 해당 클라이언트 키의 파일 경로입니다.  이 값은 선택사항입니다.</dd>
   </dl>

**예제**:

  ```
  ibmcloud ks apiserver-config-set audit-webhook --cluster my_cluster --remoteServer https://audit.example.com/audit --caCert /mnt/etc/kubernetes/apiserver-audit/ca.pem --clientCert /mnt/etc/kubernetes/apiserver-audit/cert.pem --clientKey /mnt/etc/kubernetes/apiserver-audit/key.pem
  ```
  {: pre}


### ibmcloud ks apiserver-config-unset
{: #cs_apiserver_config_unset}

클러스터의 Kubernetes API 서버 구성에 대한 옵션을 사용 안함으로 설정합니다. 이 명령은 설정 해제할 구성 옵션에 대한 다음 하위 명령 중 하나와 결합되어야 합니다.
{: shortdesc}

#### ibmcloud ks apiserver-config-unset audit-webhook
{: #cs_apiserver_unset}

클러스터의 API 서버를 위한 웹훅 백엔드 구성을 사용 안함으로 설정합니다. 웹훅 백엔드가 사용되지 않으면 원격 서버로의 API 서버 감사 로그 전달이 중지됩니다.
{: shortdesc}

```
ibmcloud ks apiserver-config-unset audit-webhook --cluster CLUSTER
```
{: pre}

웹훅을 사용 안함으로 설정하면 `ibmcloud ks apiserver-refresh` 명령을 실행하여 Kubernetes 마스터에 변경사항을 적용해야 합니다.
{: note}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **편집자** 플랫폼 역할

<strong>명령 옵션</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>
   </dl>

**예제**:

  ```
  ibmcloud ks apiserver-config-unset audit-webhook --cluster my_cluster
  ```
  {: pre}

###     ibmcloud ks apiserver-refresh
{: #cs_apiserver_refresh}

`ibmcloud ks apiserver-config-set`, `apiserver-config-unset`, `cluster-feature-enable` 또는 `cluster-feature-disable` 명령으로 요청된 Kubernetes 마스터에 대한 구성 변경사항을 적용합니다. 구성 변경에 다시 시작이 필요한 경우 영향을 받은 Kubernetes 마스터 컴포넌트가 다시 시작됩니다. 다시 시작하지 않고 구성 변경사항을 적용할 수 있는 경우에는 Kubernetes 마스터 컴포넌트가 재시작되지 않습니다. 작업자 노드, 앱 및 리소스는 수정되지 않고 계속 실행됩니다.
{: shortdesc}

```
ibmcloud ks apiserver-refresh --cluster CLUSTER [-s]
```
{: pre}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **운영자** 플랫폼 역할

<strong>명령 옵션</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

   <dt><code>-s</code></dt>
   <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>

   </dl>

**예제**:

  ```
  ibmcloud ks apiserver-refresh --cluster my_cluster
  ```
  {: pre}


<br />


## CLI 플러그인 사용법 명령
{: #cli_plug-in_commands}

### ibmcloud ks help
{: #cs_help}

지원되는 명령 및 매개변수의 목록을 봅니다.
{: shortdesc}

```
  ibmcloud ks help
```
{: pre}

<strong>최소 필요 권한</strong>: 없음

<strong>명령 옵션</strong>: 없음


###         ibmcloud ks init
{: #cs_init}

{{site.data.keyword.containerlong_notm}} 플러그인을 초기화하거나 Kubernetes 클러스터를 작성 또는 액세스할 지역을 지정합니다.
{: shortdesc}

지원되는 엔드포인트는 다음과 같습니다.
* 글로벌: `https://containers.cloud.ibm.com`
* 달라스(미국 남부, us-south): `https://us-south.containers.cloud.ibm.com`
* 프랑크푸르트(중앙 유럽, eu-de): `https://eu-central.containers.cloud.ibm.com`
* 런던(영국 남부, eu-gb): `https://uk-south.containers.cloud.ibm.com`
* 시드니(AP 남부, au-syd): `https://ap-south.containers.cloud.ibm.com`
* 도쿄(AP 중부, jp-tok): `https://ap-north.containers.cloud.ibm.com`
* 워싱턴, D.C.(미국 동부, us-east): `https://us-east.containers.cloud.ibm.com`

```
ibmcloud ks init [--host HOST] [--insecure] [-p] [-u] [-s]
```
{: pre}

<strong>최소 필요 권한</strong>: 없음

<strong>명령 옵션</strong>:

   <dl>
   <dt><code>--host <em>HOST</em></code></dt>
   <dd>사용할 {{site.data.keyword.containerlong_notm}} API 엔드포인트입니다. 이 값은 선택사항입니다. [사용 가능한 API 엔드포인트 값을 보십시오.](/docs/containers?topic=containers-regions-and-zones#container_regions)</dd>

   <dt><code>--insecure</code></dt>
   <dd>비보안 HTTP 연결을 허용합니다.</dd>

   <dt><code>-p</code></dt>
   <dd>IBM Cloud 비밀번호입니다.</dd>

   <dt><code>-u</code></dt>
   <dd>IBM Cloud 사용자 이름입니다.</dd>

   <dt><code>-s</code></dt>
   <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>

   </dl>

**예제**:
*  미국 남부 지역적 엔드포인트를 대상으로 지정하는 예:
  ```
  ibmcloud ks init --host https://us-south.containers.cloud.ibm.com
  ```
  {: pre}
*  글로벌 엔드포인트를 대상으로 지정하는 예:
  ```
  ibmcloud ks init --host https://containers.cloud.ibm.com
  ```
  {: pre}

### ibmcloud ks messages
{: #cs_messages}

IBM ID 사용자에 대한 현재 메시지를 봅니다.
{: shortdesc}

```
ibmcloud ks messages
```
{: pre}

<strong>최소 필요 권한</strong>: 없음

<br />


## 클러스터 명령: 관리
{: #cluster_mgmt_commands}

### ibmcloud ks addon-versions
{: #cs_addon_versions}

{{site.data.keyword.containerlong_notm}}에 관리 추가 기능에 대해 지원되는 버전 목록을 봅니다.
{: shortdesc}

```
ibmcloud ks addon-versions [--addon ADD-ON_NAME] [--json] [-s]
```
{: pre}

<strong>최소 필요 권한</strong>: 없음

**명령 옵션**:

  <dl>
  <dt><code>--addon <em>ADD-ON_NAME</em></code></dt>
  <dd>선택사항: 추가 기능 이름(예: <code>istio</code> 또는 <code>knative</code>)을 지정하여 버전을 필터링하십시오. </dd>

  <dt><code>--json</code></dt>
  <dd>명령 출력을 JSON 형식으로 인쇄합니다.  이 값은 선택사항입니다.</dd>

  <dt><code>-s</code></dt>
  <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>
  </dl>

**예제**:

  ```
  ibmcloud ks addon-versions --addon istio
  ```
  {: pre}

### ibmcloud ks cluster-addon-disable
{: #cs_cluster_addon_disable}

기존 클러스터에서 관리 추가 기능을 사용 안함으로 설정합니다. 이 명령은 사용 안함으로 설정할 관리 추가 기능에 대한 다음 하위 명령 중 하나와 결합되어야 합니다.
{: shortdesc}

#### ibmcloud ks cluster-addon-disable <ph class="ignoreSpelling">istio</ph>
{: #cs_cluster_addon_disable_istio}

관리 Istio 추가 기능을 사용 안함으로 설정합니다. Prometheus를 포함하여 클러스터에서 모든 lstio 핵심 컴포넌트를 제거합니다.
{: shortdesc}

```
ibmcloud ks cluster-addon-disable istio --cluster CLUSTER [-f]
```
{: pre}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **관리자** 플랫폼 역할

<strong>명령 옵션</strong>:
<dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>
   <dt><code>-f</code></dt>
   <dd>선택 사항: 이 추가 기능은 <code>istio-extras</code>, <code>istio-sample-bookinfo</code> 및 <code>knative</code> 관리 추가 기능에 대한 종속 항목입니다. 해당 추가 기능을 사용 안함으로 설정하려면 이 플래그를 포함하십시오.</dd>
</dl>

**예제**:

  ```
  ibmcloud ks cluster-addon-disable istio --cluster my_cluster
  ```
  {: pre}

#### ibmcloud ks cluster-addon-disable istio-extras
{: #cs_cluster_addon_disable_istio_extras}

관리 Istio extras 추가 기능을 사용 안함으로 설정합니다. 클러스터에서 Grafana, Jeager 및 Kiali를 제거합니다.
{: shortdesc}

```
ibmcloud ks cluster-addon-disable istio-extras --cluster CLUSTER [-f]
```
{: pre}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **관리자** 플랫폼 역할

<strong>명령 옵션</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

   <dt><code>-f</code></dt>
   <dd>선택 사항: 이 추가 기능은 <code>istio-sample-bookinfo</code> 관리 추가 기능에 대한 종속 항목입니다. 해당 추가 기능을 사용 안함으로 설정하려면 이 플래그를 포함하십시오.</dd>
</dl>

**예제**:

  ```
  ibmcloud ks cluster-addon-disable istio-extras --cluster my_cluster
  ```
  {: pre}

#### ibmcloud ks cluster-addon-disable istio-sample-bookinfo
{: #cs_cluster_addon_disable_istio_sample_bookinfo}

관리 Istio BookInfo 추가 기능을 사용 안함으로 설정합니다. 클러스터에서 모든 배치, 팟(Pod) 및 기타 BookInfo 앱 리소스를 제거합니다.
{: shortdesc}

```
ibmcloud ks cluster-addon-disable istio-sample-bookinfo --cluster CLUSTER
```
{: pre}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **관리자** 플랫폼 역할

<strong>명령 옵션</strong>:

<dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
  <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>
</dl>

**예제**:

  ```
  ibmcloud ks cluster-addon-disable istio-sample-bookinfo --cluster my_cluster
  ```
  {: pre}

#### ibmcloud ks cluster-addon-disable <ph class="ignoreSpelling">knative</ph>
{: #cs_cluster_addon_disable_knative}

관리 Knative 추가 기능을 사용 안함으로 설정하여 클러스터에서 Knative 서버리스 프레임워크를 제거합니다.
{: shortdesc}

```
ibmcloud ks cluster-addon-disable knative --cluster CLUSTER
```
{: pre}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **관리자** 플랫폼 역할

<strong>명령 옵션</strong>:
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>
</dl>

**예제**:
```
  ibmcloud ks cluster-addon-disable knative --cluster my_cluster
```
{: pre}

#### ibmcloud ks cluster-addon-disable kube-terminal
{: #cs_cluster_addon_disable_kube-terminal}

[Kubernetes 터미널](/docs/containers?topic=containers-cs_cli_install#cli_web) 추가 기능을 사용 안함으로 설정합니다. {{site.data.keyword.containerlong_notm}} 클러스터 콘솔에서 Kubernetes 터미널을 사용하려면 추가 기능을 먼저 사용으로 설정해야 합니다.
{: shortdesc}

```
ibmcloud ks cluster-addon-disable kube-terminal --cluster CLUSTER
```
{: pre}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **관리자** 플랫폼 역할

<strong>명령 옵션</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>
</dl>

**예제**:

```
ibmcloud ks cluster-addon-disable kube-terminal --cluster my_cluster
```
{: pre}

### ibmcloud ks cluster-addon-enable
{: #cs_cluster_addon_enable}

기존 클러스터에서 관리 추가 기능을 사용으로 설정합니다. 이 명령은 사용으로 설정할 관리 추가 기능에 대한 다음 하위 명령 중 하나와 결합되어야 합니다.
{: shortdesc}

#### ibmcloud ks cluster-addon-enable <ph class="ignoreSpelling">istio</ph>
{: #cs_cluster_addon_enable_istio}

관리 [Istio 추가 기능](/docs/containers?topic=containers-istio)을 사용으로 설정합니다. Prometheus를 포함하여 Istio의 핵심 컴포넌트를 설치합니다.
{: shortdesc}

```
ibmcloud ks cluster-addon-enable istio --cluster CLUSTER [--version VERSION]
```
{: pre}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **관리자** 플랫폼 역할

<strong>명령 옵션</strong>:
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

<dt><code>--version <em>VERSION</em></code></dt>
<dd>선택사항: 설치할 추가 기능의 버전을 지정하십시오. 버전이 지정되지 않은 경우, 기본 버전이 설치됩니다.</dd>
</dl>

**예제**:

```
  ibmcloud ks cluster-addon-enable istio --cluster my_cluster
```
{: pre}

#### ibmcloud ks cluster-addon-enable istio-extras
{: #cs_cluster_addon_enable_istio_extras}

관리 Istio extras 추가 기능을 사용으로 설정합니다. Grafana, Jeager 및 Kiali를 설치하여 Istio에 대한 추가 모니터링, 추적 및 시각화를 제공합니다.
{: shortdesc}

```
ibmcloud ks cluster-addon-enable istio-extras --cluster CLUSTER [--version VERSION] [-y]
```
{: pre}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **관리자** 플랫폼 역할

<strong>명령 옵션</strong>:
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

<dt><code>--version <em>VERSION</em></code></dt>
<dd>선택사항: 설치할 추가 기능의 버전을 지정하십시오. 버전이 지정되지 않은 경우, 기본 버전이 설치됩니다.</dd>

<dt><code>-y</code></dt>
<dd>선택사항: <code>istio</code> 추가 기능 종속성을 사용으로 설정합니다.</dd>
</dl>

**예제**:
```
  ibmcloud ks cluster-addon-enable istio-extras --cluster my_cluster
```
{: pre}

#### ibmcloud ks cluster-addon-enable istio-sample-bookinfo
{: #cs_cluster_addon_enable_istio_sample_bookinfo}

관리 Istio BookInfo 추가 기능을 사용으로 설정합니다. [Istio의 BookInfo 샘플 애플리케이션 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://istio.io/docs/examples/bookinfo/)을 <code>default</code> 네임스페이스에 배치합니다.
{: shortdesc}

```
ibmcloud ks cluster-addon-enable istio-sample-bookinfo --cluster CLUSTER [--version VERSION] [-y]
```
{: pre}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **관리자** 플랫폼 역할

<strong>명령 옵션</strong>:

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

<dt><code>--version <em>VERSION</em></code></dt>
<dd>선택사항: 설치할 추가 기능의 버전을 지정하십시오. 버전이 지정되지 않은 경우, 기본 버전이 설치됩니다.</dd>

<dt><code>-y</code></dt>
<dd>선택사항: <code>istio</code> 및 <code>istio-extras</code> 추가 기능 종속성을 사용으로 설정합니다.</dd>
</dl>

**예제**:
```
  ibmcloud ks cluster-addon-enable istio-sample-bookinfo --cluster my_cluster
```
{: pre}

#### ibmcloud ks cluster-addon-enable <ph class="ignoreSpelling">knative</ph>
{: #cs_cluster_addon_enable_knative}

관리[Knative 추가 기능](/docs/containers?topic=containers-knative_tutorial)을 사용으로 설정하여 Knative 서버리스 프레임워크를 설치합니다.
{: shortdesc}

```
ibmcloud ks cluster-addon-enable knative --cluster CLUSTER [--version VERSION] [-y]
```
{: pre}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **관리자** 플랫폼 역할

<strong>명령 옵션</strong>:

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

<dt><code>--version <em>VERSION</em></code></dt>
<dd>선택사항: 설치할 추가 기능의 버전을 지정하십시오. 버전이 지정되지 않은 경우, 기본 버전이 설치됩니다.</dd>

<dt><code>-y</code></dt>
<dd>선택사항: <code>istio</code> 추가 기능 종속성을 사용으로 설정합니다.</dd>
</dl>

**예제**:
```
  ibmcloud ks cluster-addon-enable knative --cluster my_cluster
```
{: pre}

#### ibmcloud ks cluster-addon-enable kube-terminal
{: #cs_cluster_addon_enable_kube-terminal}

[Kubernetes 터미널](/docs/containers?topic=containers-cs_cli_install#cli_web) 추가 기능을 사용으로 설정하여 {{site.data.keyword.containerlong_notm}} 클러스터 콘솔에서 Kubernetes 터미널을 사용합니다.
{: shortdesc}

```
ibmcloud ks cluster-addon-enable kube-terminal --cluster CLUSTER [--version VERSION]
```
{: pre}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **관리자** 플랫폼 역할

<strong>명령 옵션</strong>:

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

<dt><code>--version <em>VERSION</em></code></dt>
<dd>선택사항: 설치할 추가 기능의 버전을 지정하십시오. 버전이 지정되지 않은 경우, 기본 버전이 설치됩니다.</dd>
</dl>

**예제**:
```
ibmcloud ks cluster-addon-enable kube-terminal --cluster my_cluster
```
{: pre}

### ibmcloud ks cluster-addons
{: #cs_cluster_addons}

클러스터에서 사용으로 설정된 관리 추가 기능을 나열합니다.
{: shortdesc}

```
ibmcloud ks cluster-addons --cluster CLUSTER
```
{: pre}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **뷰어** 플랫폼 역할

<strong>명령 옵션</strong>:
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>
</dl>

**예제**:
```
  ibmcloud ks cluster-addons --cluster my_cluster
```
{: pre}

### ibmcloud ks cluster-config
{: #cs_cluster_config}

로그인한 후 클러스터에 연결하는 데 필요한 Kubernetes 구성 데이터 및 인증서를 다운로드하고 `kubectl` 명령을 실행합니다. 파일은 `user_home_directory/.bluemix/plugins/container-service/clusters/<cluster_name>`에 다운로드됩니다.
{: shortdesc}

```
ibmcloud ks cluster-config --cluster CLUSTER [--admin] [--export] [--network] [--skip-rbac] [-s] [--yaml]
```
{: pre}

**최소 필수 권한**:  {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **뷰어** 또는 **독자** {{site.data.keyword.Bluemix_notm}} IAM 서비스 역할. 또한 플랫폼 역할만 있거나 서비스 역할만 있는 경우 추가 제한조건이 적용됩니다.
* **플랫폼**: 플랫폼 역할만 있는 경우 이 명령을 수행할 수 있지만 클러스터에서 Kubernetes 조치를 수행하려면 [서비스 역할](/docs/containers?topic=containers-users#platform) 또는 [사용자 정의 RBAC 정책](/docs/containers?topic=containers-users#role-binding)이 필요합니다.
* **서비스**: 서비스 역할만 있는 경우 이 명령을 수행할 수 있습니다. 그러나 `ibmcloud ks clusters` 명령을 실행하거나 {{site.data.keyword.containerlong_notm}} 콘솔을 시작하여 클러스터를 볼 수 없으므로 클러스터 관리자가 클러스터 이름과 ID를 사용자에게 제공해야 합니다. 그런 다음 [CLI에서 Kubernetes 대시보드를 시작](/docs/containers?topic=containers-app#db_cli)하여 Kubernetes로 작업할 수 있습니다.

**명령 옵션**:

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

<dt><code>--admin</code></dt>
<dd>수퍼유저 역할을 위한 TLS 인증서와 권한 파일을 다운로드합니다. 재인증할 필요 없이 인증서를 사용하여 클러스터의 태스크를 자동화할 수 있습니다. 파일은 `<user_home_directory>/.bluemix/plugins/container-service/clusters/<cluster_name>-admin`에 다운로드됩니다.  이 값은 선택사항입니다.</dd>

<dt><code>--network</code></dt>
<dd>클러스터에서 <code>calicoctl</code> 명령을 실행하기 위해 필요한 Calico 구성 파일, TLS 인증서 및 권한 파일을 다운로드합니다. 이 값은 선택사항입니다. **참고**: 다운로드한 Kubernetes 구성 파일 및 인증서에 대한 내보내기 명령을 얻으려면 이 명령을 이 플래그 없이 실행해야 합니다.</dd>

<dt><code>--export</code></dt>
<dd>내보내기 명령 이외의 다른 메시지 없이 Kubernetes 구성 데이터와 인증서를 다운로드합니다. 메시지가 표시되지 않으므로 자동화된 스크립트를 작성할 때 이 플래그를 사용할 수 있습니다.  이 값은 선택사항입니다.</dd>

<dt><code>--skip-rbac</code></dt>
<dd>{{site.data.keyword.Bluemix_notm}} IAM 서비스 액세스 역할을 기반으로 한 사용자 Kubernetes RBAC 역할을 클러스터 구성에 추가하는 것을 건너뜁니다. [사용자 고유의 Kubernetes RBAC 역할을 관리](/docs/containers?topic=containers-users#rbac)하는 경우에만 이 옵션을 포함합니다. [{{site.data.keyword.Bluemix_notm}} IAM 서비스 액세스 역할](/docs/containers?topic=containers-access_reference#service)을 사용하여 모든 RBAC 사용자를 관리하는 경우 이 옵션을 포함하지 마십시오.</dd>

<dt><code>-s</code></dt>
<dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>

<dt><code>--yaml</code></dt>
<dd>명령 출력을 YAML 형식으로 인쇄합니다.  이 값은 선택사항입니다.</dd>
</dl>

**예제**:
```
ibmcloud ks cluster-config --cluster my_cluster
```
{: pre}


### ibmcloud ks cluster-create
{: #cs_cluster_create}

조직에 클러스터를 작성합니다. 무료 클러스터의 경우 클러스터 이름을 지정합니다. 그 외에는 모두 기본값으로 설정됩니다. 무료 클러스터는 30일 후에 자동으로 삭제됩니다. 한 번에 하나의 무료 클러스터가 제공됩니다. Kubernetes의 전체 기능을 활용하려면 표준 클러스터를 작성하십시오.
{: shortdesc}

```
ibmcloud ks cluster-create [--file FILE_LOCATION] [--hardware HARDWARE] --zone ZONE --machine-type MACHINE_TYPE --name NAME [--kube-version MAJOR.MINOR.PATCH] [--no-subnet] [--private-vlan PRIVATE_VLAN] [--public-vlan PUBLIC_VLAN] [--private-only] [--private-service-endpoint] [--public-service-endpoint] [--workers WORKER] [--disable-disk-encrypt] [--trusted] [-s]
```
{: pre}

<strong>최소 필요 권한</strong>:
* 계정 레벨에서 {{site.data.keyword.containerlong_notm}}에 대한 **관리자** 플랫폼 역할
* 계정 레벨에서 {{site.data.keyword.registrylong_notm}}에 대한 **관리자** 플랫폼 역할
* IBM Cloud 인프라(SoftLayer)에 대한 **수퍼유저** 역할.

<strong>명령 옵션</strong>

<dl>
<dt><code>--file <em>FILE_LOCATION</em></code></dt>
<dd>표준 클러스터를 작성하기 위한 YAML 파일의 경로입니다. 이 명령에 제공된 옵션을 사용하여 클러스터의 특징을 정의하지 않고 YAML 파일을 사용할 수 있습니다. 이 값은 표준 클러스터의 경우 선택사항이며 무료 클러스터에는 사용할 수 없습니다. <p class="note">YAML 파일의 매개변수와 동일한 옵션을 명령에서 제공하면 명령의 값이 YAML의 값에 우선합니다. 예를 들어, YAML 파일의 위치를 정의하고 명령에서 <code>--zone</code> 옵션을 사용하면 명령 옵션에 입력한 값이 YAML 파일의 값을 대체합니다.</p>

<pre class="codeblock">
<code>name: <em>&lt;cluster_name&gt;</em>
zone: <em>&lt;zone&gt;</em>
no-subnet: <em>&lt;no-subnet&gt;</em>
machine-type: <em>&lt;machine_type&gt;</em>
private-vlan: <em>&lt;private_VLAN&gt;</em>
public-vlan: <em>&lt;public_VLAN&gt;</em>
hardware: <em>&lt;shared_or_dedicated&gt;</em>
workerNum: <em>&lt;number_workers&gt;</em>
kube-version: <em>&lt;kube-version&gt;</em>
diskEncryption: <em>false</em>
trusted: <em>true</em>
</code></pre>
</dd>

<dt><code>--hardware <em>HARDWARE</em></code></dt>
<dd>작업자 노드에 대한 하드웨어 격리의 레벨입니다. 사용자 전용으로만 실제 리소스를 사용 가능하게 하려면 dedicated를 사용하고, 실제 리소스를 다른 IBM 고객과 공유하도록 허용하려면 shared를 사용하십시오. 기본값은 shared입니다. 이 값은 VM 표준 클러스터의 경우 선택사항이며 무료 클러스터에는 사용할 수 없습니다. 베어메탈 머신 유형의 경우에는 `dedicated`를 지정하십시오.</dd>

<dt><code>--zone <em>ZONE</em></code></dt>
<dd>이 값은 표준 클러스터에 필수입니다. 무료 클러스터는 <code>ibmcloud ks region-set</code> 명령을 사용하여 대상으로 설정하는 지역에서 작성할 수 있으나 구역을 지정할 수 없습니다. 

<p>[사용 가능한 구역](/docs/containers?topic=containers-regions-and-zones#zones)을 검토하십시오.</p>

<p class="note">자국 외에 있는 구역을 선택하는 경우에는 외국에서 데이터를 실제로 저장하기 전에 법적 인가를 받아야 할 수 있음을 유념하십시오.</p>
</dd>

<dt><code>--machine-type <em>MACHINE_TYPE</em></code></dt>
<dd>머신 유형을 선택합니다. 공유 또는 전용 하드웨어에서 가상 머신으로서 또는 베어메탈에서 실제 머신으로서 작업자 노드를 배치할 수 있습니다. 사용 가능한 실제 및 가상 머신 유형은 클러스터가 배치되는 구역에 따라 다양합니다. 자세한 정보는 `ibmcloud ks machine-types` [명령](/docs/containers?topic=containers-cs_cli_reference#cs_machine_types)에 대한 문서를 참조하십시오. 이 값은 표준 클러스터의 경우 필수이며 무료 클러스터에는 사용할 수 없습니다.</dd>

<dt><code>--name <em>NAME</em></code></dt>
<dd>클러스터의 이름입니다.  이 값은 필수입니다. 이름은 문자로 시작해야 하며 35자 이하의 문자, 숫자 및 하이픈(-)을 포함할 수 있습니다. 클러스터 이름과 클러스터가 배치된 지역이 Ingress 하위 도메인의 완전한 이름을 형성합니다. 특정 Ingress 하위 도메인이 지역 내에서 고유하도록 하기 위해 클러스터 이름을 자르고 Ingress 도메인 이름 내의 무작위 값을 추가할 수 있습니다.
</dd>

<dt><code>--kube-version <em>MAJOR.MINOR.PATCH</em></code></dt>
<dd>클러스터 마스터 노드를 위한 Kubernetes 버전입니다. 이 값은 선택사항입니다. 버전이 지정되지 않은 경우에는 클러스터가 지원되는 Kubernetes 버전의 기본값으로 작성됩니다. 사용 가능한 버전을 보려면 <code>ibmcloud ks kube-versions</code>를 실행하십시오.
</dd>

<dt><code>--no-subnet</code></dt>
<dd>기본적으로 공용 및 사설 포터블 서브넷이 클러스터와 연관된 VLAN에서 작성됩니다. 클러스터에서 서브넷을 작성하지 않도록 하려면 <code>--no-subnet</code> 플래그를 포함하십시오. 나중에 서브넷을 [작성](#cs_cluster_subnet_create)하거나 클러스터에 [추가](#cs_cluster_subnet_add)할 수 있습니다.</dd>

<dt><code>--private-vlan <em>PRIVATE_VLAN</em></code></dt>
<dd>

<ul>
<li>무료 클러스터에는 이 매개변수를 사용할 수 없습니다.</li>
<li>이 표준 클러스터가 이 구역에서 작성되는 첫 번째 표준 클러스터인 경우에는 이 플래그를 포함하지 마십시오. 클러스터가 작성되면 사설 VLAN이 작성됩니다.</li>
<li>이 구역에서 이전에 표준 클러스터를 작성했거나 IBM Cloud 인프라(SoftLayer)에서 이전에 사설 VLAN을 작성한 경우에는 해당 사설 VLAN을 지정해야 합니다. 사설 VLAN 라우터는 항상 <code>bcr</code>(벡엔드 라우터)로 시작하고 공용 VLAN 라우터는 항상 <code>fcr</code>(프론트 엔드 라우터)로 시작합니다. 클러스터를 작성하고 공인 및 사설 VLAN을 지정할 때는 이러한 접두부 뒤의 숫자 및 문자 조합이 일치해야 합니다.</li>
</ul>

<p>특정 구역에 대한 사설 VLAN을 이미 보유하고 있는지 알아내거나 기존 사설 VLAN의 이름을 찾으려면 <code>ibmcloud ks vlans <em>&lt;zone&gt;</em></code>을 실행하십시오.</p></dd>

<dt><code>--public-vlan <em>PUBLIC_VLAN</em></code></dt>
<dd>
<ul>
<li>무료 클러스터에는 이 매개변수를 사용할 수 없습니다.</li>
<li>이 표준 클러스터가 이 구역에서 작성되는 첫 번째 표준 클러스터인 경우에는 이 플래그를 사용하지 마십시오. 클러스터가 작성되면 공용 VLAN이 작성됩니다.</li>
<li>이 구역에서 이전에 표준 클러스터를 작성했거나 IBM Cloud 인프라(SoftLayer)에서 이전에 공용 VLAN을 작성한 경우에는 해당 공용 VLAN을 지정하십시오. 작업자 노드가 사설 VLAN에만 연결하려는 경우 이 옵션을 지정하지 마십시오. 사설 VLAN 라우터는 항상 <code>bcr</code>(벡엔드 라우터)로 시작하고 공용 VLAN 라우터는 항상 <code>fcr</code>(프론트 엔드 라우터)로 시작합니다. 클러스터를 작성하고 공인 및 사설 VLAN을 지정할 때는 이러한 접두부 뒤의 숫자 및 문자 조합이 일치해야 합니다.</li>
</ul>

<p>특정 구역에 대한 공용 VLAN을 이미 보유하고 있는지 알아내거나 기존 공용 VLAN의 이름을 찾으려면 <code>ibmcloud ks vlans <em>&lt;zone&gt;</em></code>을 실행하십시오.</p></dd>

<dt><code>--private-only </code></dt>
  <dd>공용 VLAN이 작성되지 않도록 하려면 이 옵션을 사용하십시오. `--private-vlan` 플래그를 지정하고 `--public-vlan` 플래그를 포함하지 않은 경우에만 필요합니다. <p class="note">작업자 노드가 사설 VLAN 전용으로 설정된 경우에는 개인 서비스 엔드포인트를 사용으로 설정하거나 게이트웨이 디바이스를 구성해야 합니다. 자세한 정보는 [사설 클러스터](/docs/containers?topic=containers-plan_clusters#private_clusters)를 참조하십시오.</p></dd>

<dt><code>--private-service-endpoint</code></dt>
  <dd>**[VRF 사용 계정](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started)의 Kubernetes 버전 1.11 이상을 실행하는 표준 클러스터**: Kubernetes 마스터와 작업자 노드가 사설 VLAN을 통해 통신하도록 [개인 서비스 엔드포인트](/docs/containers?topic=containers-cs_network_ov#cs_network_ov_master_private)를 사용으로 설정합니다. 또한 `--public-service-endpoint` 플래그를 사용하여 공용 서비스 엔드포인트를 사용하도록 선택하여 인터넷을 통해 클러스터에 액세스할 수 있습니다. 개인 서비스 엔드포인트만 사용으로 설정한 경우 사설 VLAN에 연결되어 Kubernetes 마스터와 통신해야 합니다. 개인 서비스 엔드포인트를 사용으로 설정한 후에는 나중에 사용 안함으로 설정할 수 없습니다.<br><br>클러스터를 작성한 후 `ibmcloud ks cluster-get <cluster_name_or_ID>`를 실행하여 엔드포인트를 가져올 수 있습니다.</dd>

<dt><code>--public-service-endpoint</code></dt>
  <dd>**Kubernetes 버전 1.11 이상을 실행하는 표준 클러스터**: 공용 네트워크를 통해 Kubernetes 마스터에 액세스할 수 있도록 [공용 서비스 엔드포인트](/docs/containers?topic=containers-cs_network_ov#cs_network_ov_master_public)를 사용으로 설정합니다(예: 터미널에서 `kubectl` 명령 실행). [VRF 사용 계정](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started)이 있고 `--private-service-endpoint` 플래그도 포함하고 있는 경우 [마스터와 작업자 노드 간 통신](/docs/containers?topic=containers-cs_network_ov#cs_network_ov_master_both)은 사설 네트워크에서 수행됩니다. 사설 전용 클러스터가 필요한 경우 나중에 공용 서비스 엔드포인트를 사용 안함으로 설정할 수 있습니다.<br><br>클러스터를 작성한 후 `ibmcloud ks cluster-get <cluster_name_or_ID>`를 실행하여 엔드포인트를 가져올 수 있습니다.</dd>

<dt><code>--workers WORKER</code></dt>
<dd>클러스터에 배치하려는 작업자 노드의 수입니다. 이 옵션을 지정하지 않으면 1개의 작업자 노드가 있는 클러스터가 작성됩니다. 이 값은 표준 클러스터의 경우 선택사항이며 무료 클러스터에는 사용할 수 없습니다.

<p class="important">모든 작업자 노드에는 클러스터가 작성된 이후 수동으로 변경될 수 없는 고유 작업자 노드 ID 및 도메인 이름이 지정됩니다. ID 또는 도메인 이름을 변경하면 Kubernetes 마스터가 클러스터를 관리할 수 없습니다.</p></dd>

<dt><code>--disable-disk-encrypt</code></dt>
<dd>작업자 노드는 기본적으로 AES 256비트 디스크 암호화 기능을 합니다. [자세히 보기](/docs/containers?topic=containers-security#encrypted_disk). 암호화를 사용 안함으로 설정하려면 이 옵션을 포함하십시오.</dd>

<dt><code>--trusted</code></dt>
<dd><p>**베어메탈 전용**: [신뢰할 수 있는 컴퓨팅](/docs/containers?topic=containers-security#trusted_compute)을 사용하여 베어메탈 작업자 노드의 변조 여부를 확인합니다. 클러스터 작성 중에 신뢰 사용을 설정하지 않고 나중에 이를 수행하려면 `ibmcloud ks feature-enable` [명령](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_feature_enable)을 사용할 수 있습니다. 신뢰를 사용하도록 설정한 후에는 나중에 사용하지 않도록 설정할 수 없습니다.</p>
<p>베어메탈 머신 유형의 신뢰 지원 여부를 확인하려면 `ibmcloud ks machine-types <zone>` [명령](#cs_machine_types)의 출력에서 **Trustable** 필드를 확인하십시오. 클러스터에서 신뢰가 사용되는지 확인하려면 `ibmcloud ks cluster-get` [명령](#cs_cluster_get)의 출력에서 **Trust ready** 필드를 보십시오. 베어메탈 작업자 노드에서 신뢰가 사용되는지 확인하려면 `ibmcloud ks worker-get` [명령](#cs_worker_get)의 출력에서 **Trust** 필드를 보십시오.</p></dd>

<dt><code>-s</code></dt>
<dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>
</dl>

**예제**:

**무료 클러스터 작성**: 클러스터 이름만 지정하십시오. 그 외에는 모두 기본값으로 설정됩니다. 무료 클러스터는 30일 후에 자동으로 삭제됩니다. 한 번에 하나의 무료 클러스터가 제공됩니다. Kubernetes의 전체 기능을 활용하려면 표준 클러스터를 작성하십시오.

```
  ibmcloud ks cluster-create --name my_cluster
```
{: pre}

**첫 번째 표준 클러스터 작성**: 구역에서 작성되는 첫 번째 표준 클러스터는 사설 VLAN도 작성합니다. 따라서 `--public-vlan` 플래그를 포함하지 마십시오.
{: #example_cluster_create}

```
ibmcloud ks cluster-create --zone dal10 --private-vlan my_private_VLAN_ID --machine-type b3c.4x16 --name my_cluster --hardware shared --workers 2
```
{: pre}

**후속 표준 클러스터 작성**: 이 구역에서 이미 표준 클러스터를 작성했거나 IBM Cloud 인프라(SoftLayer)에서 이전에 공용 VLAN을 작성한 경우에는 `--public-vlan` 플래그로 해당 공용 VLAN을 지정하십시오. 특정 구역에 대한 공용 VLAN을 이미 보유하고 있는지 알아내거나 기존 공용 VLAN의 이름을 찾으려면 `ibmcloud ks vlans <zone>`을 실행하십시오.

```
ibmcloud ks cluster-create --zone dal10 --public-vlan my_public_VLAN_ID --private-vlan my_private_VLAN_ID --machine-type b3c.4x16 --name my_cluster --hardware shared --workers 2
```
{: pre}

### ibmcloud ks cluster-feature-disable public-service-endpoint
{: #cs_cluster_feature_disable}

**Kubernetes 버전 1.11 이상을 실행하는 클러스터의 경우**: 클러스터의 공용 서비스 엔드포인트를 사용 안함으로 설정합니다.
{: shortdesc}

```
ibmcloud ks cluster-feature-disable public-service-endpoint --cluster CLUSTER [-s] [-f]
```
{: pre}

**중요**: 공용 엔드포인트를 사용 안함으로 설정하기 전에 먼저 다음 단계를 완료하여 개인 서비스 엔드포인트를 사용으로 설정해야 합니다.
1. `ibmcloud ks cluster-feature-enable private-service-endpoint --cluster <cluster_name>`를 실행하여 개인 서비스 엔드포인트를 사용으로 설정하십시오.
2. CLI의 프롬프트에 따라 Kubernetes 마스터 API 서버를 새로 고치십시오.
3. [클러스터의 모든 작업자 노드를 다시 로드하여 개인 엔드포인트 구성을 선택하십시오.](#cs_worker_reload)

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **관리자** 플랫폼 역할

<strong>명령 옵션</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

  <dt><code>-s</code></dt>
   <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>
</dl>

**예제**:

```
ibmcloud ks cluster-feature-disable public-service-endpoint --cluster my_cluster
```
{: pre}


### ibmcloud ks cluster-feature-enable
{: #cs_cluster_feature_enable}

기존 클러스터에서 기능을 사용으로 설정합니다. 이 명령은 사용으로 설정할 기능에 대한 다음 하위 명령 중 하나와 결합되어야 합니다.
{: shortdesc}

#### ibmcloud ks cluster-feature-enable private-service-endpoint
{: #cs_cluster_feature_enable_private_service_endpoint}

**Kubernetes 버전 1.11 이상을 실행하는 클러스터의 경우**: [개인 서비스 엔드포인트](/docs/containers?topic=containers-cs_network_ov#cs_network_ov_master_private)를 사용으로 설정하여 클러스터 마스터를 개인용으로 액세스 가능하도록 하십시오.
{: shortdesc}

```
ibmcloud ks cluster-feature-enable private-service-endpoint --cluster CLUSTER [-s]
```
{: pre}

이 명령을 실행하려면 다음을 수행하십시오.
1. IBM Cloud infrastructure (SoftLayer) 계정에서 [VRF](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud)를 사용으로 설정하십시오.
2. [{{site.data.keyword.Bluemix_notm}} 계정에서 서비스 엔드포인트를 사용하도록 하십시오](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started).
3. `ibmcloud ks cluster-feature-enable private-service-endpoint --cluster <cluster_name>`을 실행하십시오.
4. CLI의 프롬프트에 따라 Kubernetes 마스터 API 서버를 새로 고치십시오.
5. 클러스터에서 [모든 작업자 노드를 다시 로드](#cs_worker_reload)하여 개인 엔드포인트 구성을 선택하십시오.

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **관리자** 플랫폼 역할

<strong>명령 옵션</strong>:

   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

  <dt><code>-s</code></dt>
   <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>
</dl>

**예제**:

```
ibmcloud ks cluster-feature-enable private-service-endpoint --cluster my_cluster
```
{: pre}

#### ibmcloud ks cluster-feature-enable public-service-endpoint
{: #cs_cluster_feature_enable_public_service_endpoint}

**Kubernetes 버전 1.11 이상을 실행하는 클러스터의 경우**: [공용 서비스 엔드포인트](/docs/containers?topic=containers-cs_network_ov#cs_network_ov_master_public)를 사용으로 설정하여 클러스터 마스터를 공용으로 액세스 가능하도록 하십시오.
{: shortdesc}

```
ibmcloud ks cluster-feature-enable public-service-endpoint --cluster CLUSTER [-s]
```
{: pre}

이 명령을 실행한 후에는 CLI의 프롬프트에 따라 서비스 엔드포인트를 사용하도록 API 서버를 새로 고쳐야 합니다.

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **관리자** 플랫폼 역할

<strong>명령 옵션</strong>:

   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

  <dt><code>-s</code></dt>
   <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>
</dl>

**예제**:

```
ibmcloud ks cluster-feature-enable public-service-endpoint --cluster my_cluster
```
{: pre}

#### ibmcloud ks cluster-feature-enable trusted
{: #cs_cluster_feature_enable_trusted}

클러스터에 있는 모든 지원되는 베어메탈 작업자 노드에 대해 [신뢰할 수 있는 컴퓨팅](/docs/containers?topic=containers-security#trusted_compute)을 사용으로 설정합니다. 신뢰를 사용하도록 설정한 후에는 나중에 클러스터에 대해 이를 사용하지 않도록 설정할 수 없습니다.
{: shortdesc}

```
ibmcloud ks cluster-feature-enable trusted --cluster CLUSTER [-s] [-f]
```
{: pre}

베어메탈 머신 유형의 신뢰 지원 여부를 확인하려면 `ibmcloud ks machine-types <zone>` [명령](#cs_machine_types)의 출력에서 **Trustable** 필드를 확인하십시오. 클러스터에서 신뢰가 사용되는지 확인하려면 `ibmcloud ks cluster get` [명령](#cs_cluster_get)의 출력에서 **Trust ready** 필드를 보십시오. 베어메탈 작업자 노드에서 신뢰가 사용되는지 확인하려면 `ibmcloud ks worker get` [명령](#cs_worker_get)의 출력에서 **Trust** 필드를 보십시오.

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **관리자** 플랫폼 역할

<strong>명령 옵션</strong>:

   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

   <dt><code>-f</code></dt>
   <dd>사용자 프롬프트를 표시하지 않고 <code>--trusted</code> 옵션을 강제로 적용하려면 이 옵션을 사용하십시오.  이 값은 선택사항입니다.</dd>

  <dt><code>-s</code></dt>
   <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>
</dl>

**예제**:

```
ibmcloud ks cluster-feature-enable trusted --cluster my_cluster
```
{: pre}

### ibmcloud ks cluster-get
{: #cs_cluster_get}

조직의 클러스터에 대한 정보를 봅니다.
{: shortdesc}

```
ibmcloud ks cluster-get --cluster CLUSTER [--json] [--showResources] [-s]
```
{: pre}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **뷰어** 플랫폼 역할

<strong>명령 옵션</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

   <dt><code>--json</code></dt>
   <dd>명령 출력을 JSON 형식으로 인쇄합니다.  이 값은 선택사항입니다.</dd>

   <dt><code><em>--showResources</em></code></dt>
   <dd>추가 기능, VLAN, 서브넷 및 스토리지와 같은 추가 클러스터 리소스를 표시합니다.</dd>


  <dt><code>-s</code></dt>
  <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>
  </dl>

**예제**:

  ```
  ibmcloud ks cluster-get --cluster my_cluster --showResources
  ```
  {: pre}

**출력 예**:

```
Name:                           mycluster
ID:                             df253b6025d64944ab99ed63bb4567b6
State:                          normal
Created:                        2018-09-28T15:43:15+0000
Location:                       dal10
Master URL:                     https://c3.<region>.containers.cloud.ibm.com:30426
Public Service Endpoint URL:    https://c3.<region>.containers.cloud.ibm.com:30426
Private Service Endpoint URL:   https://c3-private.<region>.containers.cloud.ibm.com:31140
Master Location:                Dallas
Master Status:                  Ready (21 hours ago)
Ingress Subdomain:              mycluster.us-south.containers.appdomain.cloud
Ingress Secret:                 mycluster
Workers:                        6
Worker Zones:                   dal10, dal12
Version:                        1.11.3_1524
Owner:                          owner@email.com
Monitoring Dashboard:           ...
Resource Group ID:              a8a12accd63b437bbd6d58fb6a462ca7
Resource Group Name:            Default

Subnet VLANs
  VLAN ID   Subnet CIDR         Public   User-managed
  2234947   10.xxx.xx.xxx/29    false    false
  2234945   169.xx.xxx.xxx/29  true    false

```
{: screen}

### ibmcloud ks cluster-pull-secret-apply
{: #cs_cluster_pull_secret_apply}

클러스터의 {{site.data.keyword.Bluemix_notm}} IAM 서비스 ID를 작성하고 {{site.data.keyword.registrylong_notm}}에서 **독자** 서비스 액세스 역할을 지정하는 서비스 ID에 대해 정책을 작성한 다음 서비스 ID의 API 키를 작성합니다. 그런 다음 API 키는 `default` Kubernetes 네임스페이스에 있는 컨테이너의 {{site.data.keyword.registryshort_notm}} 네임스페이스에서 이미지를 가져올 수 있도록 Kubernetes `imagePullSecret`에 저장됩니다. 이 프로세스는 클러스터를 작성할 때 자동으로 발생합니다. 클러스터 작성 프로세스 중에 오류가 발생하거나 기존 클러스터가 있는 경우 이 명령을 사용하여 프로세스를 다시 적용할 수 있습니다.
{: shortdesc}

이 명령을 실행하면 IAM 인증 정보 및 이미지 풀 시크릿의 작성이 초기화되며 완료하는 데 약간의 시간이 소요될 수 있습니다. 이미지 풀 시크릿이 작성될 때까지 {{site.data.keyword.registrylong_notm}} `icr.io` 도메인에서 이미지를 가져오는 컨테이너를 배치할 수 없습니다. 이미지 풀 시크릿을 확인하려면 `kubectl get secrets | grep icr`을 실행하십시오.
{: important}

```
ibmcloud ks cluster-pull-secret-apply --cluster CLUSTER
```
{: pre}

이 API 키 메소드는 [토큰](/docs/services/Registry?topic=registry-registry_access#registry_tokens)을 자동으로 작성하고 토큰을 이미지 풀 시크릿에 저장하여 클러스터의 권한을 부여하는 이전 방법을 대체하여 {{site.data.keyword.registrylong_notm}}에 액세스합니다. 이제 IAM API 키를 사용하여 {{site.data.keyword.registrylong_notm}}에 액세스하여 네임스페이스나 특정 이미지에 대한 액세스를 제한하기 위해 서비스 ID에 대한 IAM 정책을 사용자 정의할 수 있습니다. 예를 들어, 특정 레지스트리 지역 또는 네임스페이스에서만 이미지를 가져오도록 클러스터의 이미지 풀 시크릿에서 서비스 ID 정책을 변경할 수 있습니다. IAM 정책을 사용자 정의하기 전에 [{{site.data.keyword.registrylong_notm}}에 대한 {{site.data.keyword.Bluemix_notm}} IAM 정책을 사용으로 설정해야 합니다](/docs/services/Registry?topic=registry-user#existing_users).

자세한 정보는 [{{site.data.keyword.registrylong_notm}}에서 이미지를 가져올 수 있는 권한을 클러스터에 부여하는 방법 이해](/docs/containers?topic=containers-images#cluster_registry_auth)를 참조하십시오.

IAM 정책을 기존 서비스 ID에 추가한 경우(예: 지역 레지스트리에 대한 액세스를 제한하기 위해), 이미지 풀 시크릿에 대한 서비스 ID, IAM 정책 및 API 키가 이 명령으로 재설정됩니다.
{: important}

<strong>최소 필요 권한</strong>:
*  {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **운영자 또는 관리자** 플랫폼 역할
*  {{site.data.keyword.registrylong_notm}}의 **관리자** 플랫폼 역할

<strong>명령 옵션</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

   </dl>

**예제**:

```
ibmcloud ks cluster-pull-secret-apply --cluster my_cluster
```
{: pre}

### ibmcloud ks cluster-refresh
{: #cs_cluster_refresh}

새 Kubernetes API 구성 변경사항을 적용하기 위해 클러스터 마스터 노드를 다시 시작합니다. 작업자 노드, 앱 및 리소스는 수정되지 않고 계속 실행됩니다.
{: shortdesc}

```
ibmcloud ks cluster-refresh --cluster CLUSTER [-f] [-s]
```
{: pre}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **운영자** 플랫폼 역할

<strong>명령 옵션</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

   <dt><code>-f</code></dt>
   <dd>사용자 프롬프트를 표시하지 않고 클러스터 제거를 강제 실행하려면 이 옵션을 사용하십시오.  이 값은 선택사항입니다.</dd>

   <dt><code>-s</code></dt>
   <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>

   </dl>

**예제**:

  ```
  ibmcloud ks cluster-refresh --cluster my_cluster
  ```
  {: pre}

### ibmcloud ks cluster-rm
{: #cs_cluster_rm}

조직에서 클러스터를 제거합니다.
{: shortdesc}

```
ibmcloud ks cluster-rm --cluster CLUSTER [--force-delete-storage] [-f] [-s]
```
{: pre}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **관리자** 플랫폼 역할

<strong>명령 옵션</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

   <dt><code>--force-delete-storage</code></dt>
   <dd>클러스터 및 이 클러스터가 사용하는 모든 지속적 스토리지를 삭제합니다. **주의**: 이 플래그를 포함하면 클러스터 또는 이와 연관된 스토리지 인스턴스에 저장된 데이터를 복구할 수 없습니다.  이 값은 선택사항입니다.</dd>

   <dt><code>-f</code></dt>
   <dd>사용자 프롬프트를 표시하지 않고 클러스터 제거를 강제 실행하려면 이 옵션을 사용하십시오.  이 값은 선택사항입니다.</dd>

   <dt><code>-s</code></dt>
   <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>

   </dl>

**예제**:

  ```
  ibmcloud ks cluster-rm --cluster my_cluster
  ```
  {: pre}


### ibmcloud ks cluster-update
{: #cs_cluster_update}

Kubernetes 마스터를 기본 API 버전으로 업데이트합니다. 업데이트 중에는 클러스터에 액세스하거나 클러스터를 변경할 수 없습니다. 사용자가 배치한 작업자 노드, 앱 및 리소스는 수정되지 않고 계속 실행됩니다.
{: shortdesc}

```
ibmcloud ks cluster-update --cluster CLUSTER [--kube-version MAJOR.MINOR.PATCH] [--force-update] [-f] [-s]
```
{: pre}

차후 배치를 위해 YAML 파일을 변경해야 할 수도 있습니다. 세부사항은 이 [릴리스 정보](/docs/containers?topic=containers-cs_versions)를 검토하십시오.

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **운영자** 플랫폼 역할

<strong>명령 옵션</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

   <dt><code>--kube-version <em>MAJOR.MINOR.PATCH</em></code></dt>
   <dd>클러스터의 Kubernetes 버전입니다. 버전을 지정하지 않으면 Kubernetes 마스터가 기본 API 버전으로 업데이트됩니다. 사용 가능한 버전을 보려면 [ibmcloud ks kube-versions](#cs_kube_versions)를 실행하십시오.  이 값은 선택사항입니다.</dd>

   <dt><code>--force-update</code></dt>
   <dd>변경 시 부 버전의 차이가 2보다 큰 경우에도 업데이트를 시도합니다.  이 값은 선택사항입니다.</dd>

   <dt><code>-f</code></dt>
   <dd>사용자 프롬프트를 표시하지 않고 명령을 강제 실행합니다.  이 값은 선택사항입니다.</dd>

   <dt><code>-s</code></dt>
   <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>
   </dl>

**예제**:

  ```
  ibmcloud ks cluster-update --cluster my_cluster
  ```
  {: pre}


###         ibmcloud ks clusters
{: #cs_clusters}

조직에서 클러스터의 목록을 봅니다.
{: shortdesc}

```
ibmcloud ks clusters [--json] [-s]
```
{: pre}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **뷰어** 플랫폼 역할

<strong>명령 옵션</strong>:

  <dl><dt><code>--json</code></dt>
  <dd>명령 출력을 JSON 형식으로 인쇄합니다.  이 값은 선택사항입니다.</dd>

  <dt><code>-s</code></dt>
  <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>
  </dl>

**예제**:

  ```
  ibmcloud ks clusters
  ```
  {: pre}


###   ibmcloud ks kube-versions
{: #cs_kube_versions}

{{site.data.keyword.containerlong_notm}}에서 지원되는 Kubernetes 버전 목록을 봅니다. [클러스터 마스터](#cs_cluster_update) 및 [작업자 노드](/docs/containers?topic=containers-cs_cli_reference#cs_worker_update)를 안정적인 최신 기능을 위한 기본 버전으로 업데이트합니다.
{: shortdesc}

```
ibmcloud ks kube-versions [--json] [-s]
```
{: pre}

<strong>최소 필요 권한</strong>: 없음

**명령 옵션**:

  <dl>
  <dt><code>--json</code></dt>
  <dd>명령 출력을 JSON 형식으로 인쇄합니다.  이 값은 선택사항입니다.</dd>

  <dt><code>-s</code></dt>
  <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>
  </dl>

**예제**:

  ```
  ibmcloud ks kube-versions
  ```
  {: pre}

<br />



## 클러스터 명령: 서비스 및 통합
{: #cluster_services_commands}


### ibmcloud ks cluster-service-bind
{: #cs_cluster_service_bind}

클러스터에 {{site.data.keyword.Bluemix_notm}} 서비스를 추가합니다. {{site.data.keyword.Bluemix_notm}} 카탈로그에서 사용 가능한 {{site.data.keyword.Bluemix_notm}} 서비스를 보려면 `ibmcloud service offerings`를 실행하십시오. **참고**: 서비스 키를 지원하는 {{site.data.keyword.Bluemix_notm}} 서비스만 추가할 수 있습니다.
{: shortdesc}

```
ibmcloud ks cluster-service-bind --cluster CLUSTER --namespace KUBERNETES_NAMESPACE --service SERVICE_INSTANCE_GUID [--key SERVICE_INSTANCE_KEY] [--role IAM_ROLE] [-s]
```
{: pre}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **편집자** 플랫폼 역할 및 **개발자** Cloud Foundry 역할

<strong>명령 옵션</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

   <dt><code>--key <em>SERVICE_INSTANCE_KEY</em></code></dt>
   <dd>서비스 키의 이름 또는 GUID입니다. 이 값은 선택사항입니다. 새 서비스 키를 작성하는 대신 기존 서비스 키를 사용하려면 이 값을 포함합니다.</dd>

   <dt><code>--namespace <em>KUBERNETES_NAMESPACE</em></code></dt>
   <dd>Kubernetes 네임스페이스의 이름입니다. 이 값은 필수입니다.</dd>

   <dt><code>--service <em>SERVICE_INSTANCE_GUID</em></code></dt>
   <dd>바인드하려는 {{site.data.keyword.Bluemix_notm}} 서비스 인스턴스의 ID입니다. ID를 찾으려면 <code>ibmcloud service show <service_instance_name> --guid</code>를 실행하십시오. 이 값은 필수입니다. </dd>

   <dt><code>--role <em>IAM_ROLE</em></code></dt>
   <dd>서비스 키에 필요한 {{site.data.keyword.Bluemix_notm}} IAM 역할입니다. 기본값은 IAM 서비스 역할 `Writer`입니다. 기존 서비스 키를 사용하거나 Cloud Foundry 서비스와 같이 IAM을 사용하도록 설정되지 않은 서비스에 대해서는 이 값을 포함하지 마십시오.<br><br>
   서비스에 대해 사용 가능한 역할을 나열하려면 `ibmcloud iam roles --service <service_name>`을 실행하십시오. 서비스 이름은 `ibmcloud catalog search`를 실행하여 얻을 수 있는 카탈로그의 서비스 이름입니다.</dd>

   <dt><code>-s</code></dt>
   <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>

   </dl>

**예제**:

  ```
  ibmcloud ks cluster-service-bind --cluster my_cluster --namespace my_namespace --service my_service_instance
  ```
  {: pre}


### ibmcloud ks cluster-service-unbind
{: #cs_cluster_service_unbind}

클러스터에서 {{site.data.keyword.Bluemix_notm}} 서비스를 제거합니다.
{: shortdesc}

```
ibmcloud ks cluster-service-unbind --cluster CLUSTER --namespace KUBERNETES_NAMESPACE --service SERVICE_INSTANCE_GUID [-s]
```
{: pre}

{{site.data.keyword.Bluemix_notm}} 서비스를 제거하면 서비스 인증 정보도 클러스터에서 제거됩니다. 팟(Pod)에서 서비스를 계속 사용 중인 경우, 서비스 인증 정보를 찾을 수 없기 때문에 실패합니다.
{: note}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **편집자** 플랫폼 역할 및 **개발자** Cloud Foundry 역할

<strong>명령 옵션</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

   <dt><code>--namespace <em>KUBERNETES_NAMESPACE</em></code></dt>
   <dd>Kubernetes 네임스페이스의 이름입니다. 이 값은 필수입니다.</dd>

   <dt><code>--service <em>SERVICE_INSTANCE_GUID</em></code></dt>
   <dd>제거하려는 {{site.data.keyword.Bluemix_notm}} 서비스 인스턴스의 ID입니다. 서비스 인스턴스의 ID를 찾으려면 `ibmcloud ks cluster-services <cluster_name_or_ID>`를 나열하십시오. -이 값은 필수입니다.</dd>

   <dt><code>-s</code></dt>
   <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>

   </dl>

**예제**:

  ```
  ibmcloud ks cluster-service-unbind --cluster my_cluster --namespace my_namespace --service 8567221
  ```
  {: pre}


### ibmcloud ks cluster-services
{: #cs_cluster_services}

클러스터에서 한 개 또는 모든 Kubernetes 네임스페이스에 바인딩된 서비스를 나열합니다. 옵션이 지정되지 않은 경우 기본 네임스페이스를 위한 서비스가 표시됩니다.
{: shortdesc}

```
ibmcloud ks cluster-services --cluster CLUSTER [--namespace KUBERNETES_NAMESPACE] [--all-namespaces] [--json] [-s]
```
{: pre}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **뷰어** 플랫폼 역할

<strong>명령 옵션</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

   <dt><code>--namespace <em>KUBERNETES_NAMESPACE</em></code>, <code>-n <em>KUBERNETES_NAMESPACE</em></code></dt>
   <dd>클러스터에서 특정 네임스페이스에 바인딩된 서비스를 포함합니다.  이 값은 선택사항입니다.</dd>

   <dt><code>--all-namespaces</code></dt>
   <dd>클러스터에서 모든 네임스페이스에 바인딩된 서비스를 포함합니다.  이 값은 선택사항입니다.</dd>

   <dt><code>--json</code></dt>
   <dd>명령 출력을 JSON 형식으로 인쇄합니다.  이 값은 선택사항입니다.</dd>

   <dt><code>-s</code></dt>
   <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>
   </dl>

**예제**:

  ```
  ibmcloud ks cluster-services --cluster my_cluster --namespace my_namespace
  ```
  {: pre}


### ibmcloud ks va
{: #cs_va}

[컨테이너 스캐너를 설치](/docs/services/va?topic=va-va_index#va_install_container_scanner)한 후에 클러스터의 컨테이너에 대한 상세 취약성 평가 보고서를 보십시오.
{: shortdesc}

```
ibmcloud ks va --container CONTAINER_ID [--extended] [--vulnerabilities] [--configuration-issues] [--json]
```
{: pre}

<strong>최소 필요 권한</strong>: {{site.data.keyword.registrylong_notm}}에 대한 **독자** 서비스 액세스 역할. **참고**: 리소스 그룹 레벨에서 {{site.data.keyword.registryshort_notm}}에 대한 정책을 지정하지 마십시오.

**명령 옵션**:

<dl>
<dt><code>--container CONTAINER_ID</code></dt>
<dd><p>컨테이너의 ID입니다. 이 값은 필수입니다.</p>
<p>컨테이너의 ID를 찾으려면 다음을 수행하십시오.<ol><li>[클러스터에 Kubernetes CLI를 대상으로 지정](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)하십시오.</li><li>`kubectl get pods`를 실행하여 팟(Pod)을 나열하십시오.</li><li>`kubectl describe pod <pod_name>` 명령의 출력에서 **컨테이너 ID** 필드를 찾으십시오. 예: `Container ID: containerd://1a11a1aa2b2b22223333c44444ccc555667d7dd777888e8ef99f1011121314g15`.</li><li>`ibmcloud ks va` 명령에 대해 컨테이너 ID를 사용하기 전에 ID에서 `containerd://` 접두부를 제거하십시오. 예: `1a11a1aa2b2b22223333c44444ccc555667d7dd777888e8ef99f1011121314g15`.</li></ol></p></dd>

<dt><code>--extended</code></dt>
<dd><p>명령 출력을 확장하여 취약한 패키지에 대한 추가 수정사항 정보를 표시합니다.  이 값은 선택사항입니다.</p>
<p>기본적으로 스캔 결과에는 ID, 정책 상태, 영향을 받는 패키지 및 이의 해결 방법이 표시됩니다. `--extended` 플래그가 있으면 이는 요약, 공급업체 보안 공지사항 및 공식 공지사항 링크 등의 정보를 추가합니다.</p></dd>

<dt><code>--vulnerabilities</code></dt>
<dd>패키지 취약성만 표시하도록 명령 출력을 제한합니다. 이 값은 선택사항입니다. `--configuration-issues` 플래그를 사용하는 경우에는 이 플래그를 사용할 수 없습니다.</dd>

<dt><code>--configuration-issues</code></dt>
<dd>구성 문제만 표시하도록 명령 출력을 제한합니다. 이 값은 선택사항입니다. `--vulnerabilities` 플래그를 사용하는 경우에는 이 플래그를 사용할 수 없습니다.</dd>

<dt><code>--json</code></dt>
<dd>명령 출력을 JSON 형식으로 인쇄합니다.  이 값은 선택사항입니다.</dd>
</dl>

**예제**:

```
ibmcloud ks va --container 1a11a1aa2b2b22223333c44444ccc555667d7dd777888e8ef99f1011121314g15 --extended --vulnerabilities --json
```
{: pre}

### ibmcloud ks key-protect-enable
{: #cs_key_protect}

[{{site.data.keyword.keymanagementservicefull}} ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](/docs/services/key-protect?topic=key-protect-getting-started-tutorial#getting-started-tutorial)를 클러스터의 [키 관리 서비스(KMS) 제공자 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/tasks/administer-cluster/kms-provider/)로 사용하여 Kubernetes secret을 암호화합니다. 기존 키 회전을 사용하여 클러스터에서 키를 회전하려면 새 루트 키 ID로 이 명령을 다시 실행하십시오.
{: shortdesc}

```
ibmcloud ks key-protect-enable --cluster CLUSTER_NAME_OR_ID --key-protect-url ENDPOINT --key-protect-instance INSTANCE_GUID --crk ROOT_KEY_ID
```
{: pre}

{{site.data.keyword.keymanagementserviceshort}} 인스턴스에서 루트 키를 삭제하지 마십시오. 새 키를 사용하기 위해 회전하는 경우에도 키를 삭제하지 마십시오. 루트 키를 삭제하는 경우, etcd 데이터 또는 클러스터의 시크릿에 있는 데이터에 액세스하거나 이로부터 데이터를 제거할 수 없게 됩니다.
{: important}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **관리자** 플랫폼 역할

**명령 옵션**:

<dl>
<dt><code>--container CLUSTER_NAME_OR_ID</code></dt>
<dd>클러스터의 이름 또는 ID입니다.</dd>

<dt><code>--key-protect-url ENDPOINT</code></dt>
<dd>클러스터 인스턴스의 {{site.data.keyword.keymanagementserviceshort}} 엔드포인트입니다. 이 엔드포인트를 얻으려면 [지역별 서비스 엔드포인트](/docs/services/key-protect?topic=key-protect-regions#service-endpoints)를 참조하십시오.</dd>

<dt><code>--key-protect-instance INSTANCE_GUID</code></dt>
<dd>{{site.data.keyword.keymanagementserviceshort}} 인스턴스 GUID입니다. 이 인스턴스 GUID를 얻으려면 <code>ibmcloud resource service-instance SERVICE_INSTANCE_NAME --id</code>를 실행하고 두 번째 값(전체 CRN이 아님)을 복사하십시오.</dd>

<dt><code>--crk ROOT_KEY_ID</code></dt>
<dd>{{site.data.keyword.keymanagementserviceshort}} 루트 키 ID입니다. CRK를 얻으려면 [키 보기](/docs/services/key-protect?topic=key-protect-view-keys#view-keys)를 참조하십시오.</dd>
</dl>

**예제**:

```
ibmcloud ks key-protect-enable --cluster mycluster --key-protect-url keyprotect.us-south.bluemix.net --key-protect-instance a11aa11a-bbb2-3333-d444-e5e555e5ee5 --crk 1a111a1a-bb22-3c3c-4d44-55e555e55e55
```
{: pre}


### ibmcloud ks webhook-create
{: #cs_webhook_create}

웹훅을 등록합니다.
{: shortdesc}

```
ibmcloud ks webhook-create --cluster CLUSTER --level LEVEL --type slack --url URL  [-s]
```
{: pre}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **편집자** 플랫폼 역할

<strong>명령 옵션</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

   <dt><code>--level <em>LEVEL</em></code></dt>
   <dd>알림 레벨(예: <code>Normal</code> 또는 <code>Warning</code>)입니다. 기본값은 <code>Warning</code>입니다.  이 값은 선택사항입니다.</dd>

   <dt><code>--type <em>slack</em></code></dt>
   <dd>웹훅 유형입니다. 현재 slack이 지원됩니다. 이 값은 필수입니다.</dd>

   <dt><code>--url <em>URL</em></code></dt>
   <dd>웹훅의 URL입니다. 이 값은 필수입니다.</dd>

   <dt><code>-s</code></dt>
   <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>
   </dl>

**예제**:

  ```
  ibmcloud ks webhook-create --cluster my_cluster --level Normal --type slack --url http://github.com/mywebhook
  ```
  {: pre}


<br />


## 클러스터 명령: 서브넷
{: #cluster_subnets_commands}

### ibmcloud ks cluster-subnet-add
{: #cs_cluster_subnet_add}

IBM Cloud 인프라(SoftLayer) 계정의 기존 포터블 공인 또는 사설 서브넷을 Kubernetes 클러스터에 추가하거나, 자동으로 프로비저닝된 서브넷을 사용하는 대신 삭제된 클러스터의 서브넷을 재사용할 수 있습니다.
{: shortdesc}

```
ibmcloud ks cluster-subnet-add --cluster CLUSTER --subnet-id SUBNET [-s]
```
{: pre}

<p class="important">포터블 공인 IP 주소는 매월 비용이 청구됩니다. 클러스터가 프로비저닝된 후에 포터블 공인 IP 주소를 제거하는 경우에는 짧은 기간만 사용해도 여전히 월별 비용을 지불해야 합니다.</br>
</br>클러스터에 서브넷을 사용 가능하게 하면 이 서브넷의 IP 주소가 클러스터 네트워킹 목적으로 사용됩니다. IP 주소 충돌을 피하려면 한 개의 클러스터만 있는 서브넷을 사용해야 합니다. 동시에 {{site.data.keyword.containerlong_notm}}의 외부에서 다른 목적으로 또는 다중 클러스터에 대한 서브넷으로 사용하지 마십시오.</br>
</br>동일한 VLAN의 서로 다른 서브넷에 있는 작업자 간에 통신을 사용 가능하게 하려면 [동일한 VLAN의 서브넷 간에 라우팅 사용](/docs/containers?topic=containers-subnets#subnet-routing)을 설정해야 합니다.</p>

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **운영자** 플랫폼 역할

<strong>명령 옵션</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

   <dt><code>--subnet-id <em>SUBNET</em></code></dt>
   <dd>서브넷의 ID입니다. 이 값은 필수입니다.</dd>

   <dt><code>-s</code></dt>
   <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>

   </dl>

**예제**:

  ```
  ibmcloud ks cluster-subnet-add --cluster my_cluster --subnet-id 1643389
  ```
  {: pre}


### ibmcloud ks cluster-subnet-create
{: #cs_cluster_subnet_create}

IBM Cloud 인프라(SoftLayer) 계정에서 서브넷을 작성하고 {{site.data.keyword.containerlong_notm}}의 지정된 클러스터에 사용 가능하도록 설정합니다.
{: shortdesc}

```
ibmcloud ks cluster-subnet-create --cluster CLUSTER --size SIZE --vlan VLAN_ID [-s]
```
{: pre}

<p class="important">클러스터에 서브넷을 사용 가능하게 하면 이 서브넷의 IP 주소가 클러스터 네트워킹 목적으로 사용됩니다. IP 주소 충돌을 피하려면 한 개의 클러스터만 있는 서브넷을 사용해야 합니다. 동시에 {{site.data.keyword.containerlong_notm}}의 외부에서 다른 목적으로 또는 다중 클러스터에 대한 서브넷으로 사용하지 마십시오.</br>
</br>클러스터용 다중 VLAN, 동일한 VLAN의 다중 서브넷 또는 다중 구역 클러스터가 있는 경우에는 작업자 노드가 사설 네트워크에서 서로 간에 통신할 수 있도록 IBM Cloud 인프라(SoftLayer) 계정에 대해 [Virtual Router Function (VRF)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud)을 사용으로 설정해야 합니다. VRF를 사용으로 설정하려면 [IBM Cloud 인프라(SoftLayer) 계정 담당자에게 문의하십시오](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#how-you-can-initiate-the-conversion). VRF를 사용할 수 없거나 사용하지 않으려면 [VLAN Spanning](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning)을 사용으로 설정하십시오. 이 조치를 수행하려면 **네트워크 > 네트워크 VLAN Spanning 관리** [인프라 권한](/docs/containers?topic=containers-users#infra_access)이 필요합니다. 또는 이를 사용으로 설정하도록 계정 소유자에게 요청할 수 있습니다. VLAN Spanning이 이미 사용으로 설정되었는지 확인하려면 `ibmcloud ks vlan-spanning-get` [명령](/docs/containers?topic=containers-cs_cli_reference#cs_vlan_spanning_get)을 사용하십시오.</p>

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **운영자** 플랫폼 역할

<strong>명령 옵션</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다. 클러스터를 나열하려면 `ibmcloud ks clusters` [명령](#cs_clusters)을 사용하십시오.</dd>

   <dt><code>--size <em>SIZE</em></code></dt>
   <dd>서브넷 IP 주소의 수입니다. 이 값은 필수입니다. 가능한 값은 8, 16, 32 또는 64입니다.</dd>

   <dd>서브넷을 작성할 VLAN입니다. 이 값은 필수입니다. 사용 가능한 VLAN을 나열하려면 `ibmcloud ks vlans <zone>` [명령](#cs_vlans)을 사용하십시오. 서브넷은 VLAN이 있는 동일한 구역에서 프로비저닝됩니다.</dd>

   <dt><code>-s</code></dt>
   <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>

   </dl>

**예제**:

  ```
  ibmcloud ks cluster-subnet-create --cluster my_cluster --size 8 --vlan 1764905
  ```
  {: pre}


### ibmcloud ks cluster-user-subnet-add
{: #cs_cluster_user_subnet_add}

고유한 사설 서브넷을 {{site.data.keyword.containerlong_notm}} 클러스터로 가져옵니다.
{: shortdesc}

```
ibmcloud ks cluster-user-subnet-add --cluster CLUSTER --subnet-cidr SUBNET_CIDR --private-vlan PRIVATE_VLAN
```
{: pre}

이 사설 서브넷은 IBM Cloud 인프라(SoftLayer)에서 제공되는 서브넷이 아닙니다. 따라서 서브넷에 대한 인바운드 및 아웃바운드 네트워크 트래픽 라우팅을 구성해야 합니다. IBM Cloud 인프라(SoftLayer) 서브넷을 추가하려면 `ibmcloud ks cluster-subnet-add` [명령](#cs_cluster_subnet_add)을 사용하십시오.

<p class="important">클러스터에 서브넷을 사용 가능하게 하면 이 서브넷의 IP 주소가 클러스터 네트워킹 목적으로 사용됩니다. IP 주소 충돌을 피하려면 한 개의 클러스터만 있는 서브넷을 사용해야 합니다. 동시에 {{site.data.keyword.containerlong_notm}}의 외부에서 다른 목적으로 또는 다중 클러스터에 대한 서브넷으로 사용하지 마십시오.</br>
</br>클러스터용 다중 VLAN, 동일한 VLAN의 다중 서브넷 또는 다중 구역 클러스터가 있는 경우에는 작업자 노드가 사설 네트워크에서 서로 간에 통신할 수 있도록 IBM Cloud 인프라(SoftLayer) 계정에 대해 [Virtual Router Function (VRF)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud)을 사용으로 설정해야 합니다. VRF를 사용으로 설정하려면 [IBM Cloud 인프라(SoftLayer) 계정 담당자에게 문의하십시오](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#how-you-can-initiate-the-conversion). VRF를 사용할 수 없거나 사용하지 않으려면 [VLAN Spanning](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning)을 사용으로 설정하십시오. 이 조치를 수행하려면 **네트워크 > 네트워크 VLAN Spanning 관리** [인프라 권한](/docs/containers?topic=containers-users#infra_access)이 필요합니다. 또는 이를 사용으로 설정하도록 계정 소유자에게 요청할 수 있습니다. VLAN Spanning이 이미 사용으로 설정되었는지 확인하려면 `ibmcloud ks vlan-spanning-get` [명령](/docs/containers?topic=containers-cs_cli_reference#cs_vlan_spanning_get)을 사용하십시오.</p>

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **운영자** 플랫폼 역할

<strong>명령 옵션</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

   <dt><code>--subnet-cidr <em>SUBNET_CIDR</em></code></dt>
   <dd>서브넷 CIDR(Classless InterDomain Routing)입니다. 이 값은 필수이며 IBM Cloud 인프라(SoftLayer)에서 사용되는 서브넷과 충돌하지 않아야 합니다.

   지원되는 접두부의 범위는 `/30`(1개의 IP 주소) - `/24`(253개의 IP 주소)입니다. 하나의 접두부 길이에 CIDR을 설정하고 나중에 이를 변경해야 하는 경우 먼저 새 CIDR을 추가한 후 [이전 CIDR을 제거](#cs_cluster_user_subnet_rm)하십시오.</dd>

   <dt><code>--private-vlan <em>PRIVATE_VLAN</em></code></dt>
   <dd>사설 VLAN의 ID입니다. 이 값은 필수입니다. 클러스터에 있는 하나 이상의 작업자 노드의 사설 VLAN ID와 일치해야 합니다.</dd>
   </dl>

**예제**:

  ```
  ibmcloud ks cluster-user-subnet-add --cluster my_cluster --subnet-cidr 169.xx.xxx.xxx/29 --private-vlan 1502175
  ```
  {: pre}


### ibmcloud ks cluster-user-subnet-rm
{: #cs_cluster_user_subnet_rm}

지정된 클러스터에서 고유한 사설 서브넷을 제거합니다. 고유한 사설 서브넷에서 IP 주소에 배치된 서비스는 서브넷이 제거된 후에도 활성 상태를 유지합니다.
{: shortdesc}

```
ibmcloud ks cluster-user-subnet-rm --cluster CLUSTER --subnet-cidr SUBNET_CIDR --private-vlan PRIVATE_VLAN
```
{: pre}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **운영자** 플랫폼 역할

<strong>명령 옵션</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

   <dt><code>--subnet-cidr <em>SUBNET_CIDR</em></code></dt>
   <dd>서브넷 CIDR(Classless InterDomain Routing)입니다. 이 값은 필수이며 `ibmcloud ks cluster-user-subnet-add` [명령](#cs_cluster_user_subnet_add)으로 설정된 CIDR과 일치해야 합니다.</dd>

   <dt><code>--private-vlan <em>PRIVATE_VLAN</em></code></dt>
   <dd>사설 VLAN의 ID입니다. 이 값은 필수이며 `ibmcloud ks cluster-user-subnet-add` [명령](#cs_cluster_user_subnet_add)으로 설정된 VLAN ID와 일치해야 합니다.</dd>
   </dl>

**예제**:

  ```
  ibmcloud ks cluster-user-subnet-rm --cluster my_cluster --subnet-cidr 169.xx.xxx.xxx/29 --private-vlan 1502175
  ```
  {: pre}


###     ibmcloud ks subnets
{: #cs_subnets}

IBM Cloud 인프라(SoftLayer) 계정에서 사용 가능한 서브넷의 목록을 봅니다.
{: shortdesc}

```
ibmcloud ks subnets [--json] [-s]
```
{: pre}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **뷰어** 플랫폼 역할

<strong>명령 옵션</strong>:

  <dl>
  <dt><code>--json</code></dt>
  <dd>명령 출력을 JSON 형식으로 인쇄합니다.  이 값은 선택사항입니다.</dd>

  <dt><code>-s</code></dt>
  <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>
  </dl>

**예제**:

  ```
  ibmcloud ks subnets
  ```
  {: pre}


<br />


## Ingress 애플리케이션 로드 밸런서(ALB) 명령
{: #alb_commands}

### ibmcloud ks alb-autoupdate-disable
{: #cs_alb_autoupdate_disable}

기본적으로, Ingress 애플리케이션 로드 밸런서(ALB) 추가 기능에 대한 자동 업데이트는 사용으로 설정되어 있습니다. ALB 팟(Pod)은 새 빌드 버전이 사용 가능할 때 자동으로 업데이트됩니다. 대신에 추가 기능을 수동으로 업데이트하려면 이 명령을 사용하여 자동 업데이트를 사용 안함으로 설정하십시오. 그리고 [`ibmcloud ks alb-update` 명령](#cs_alb_update)을 실행하여 ALB 팟(Pod)을 업데이트할 수 있습니다.
{: shortdesc}

```
ibmcloud ks alb-autoupdate-disable --cluster CLUSTER
```
{: pre}

클러스터의 Kubernetes 주 버전 또는 부 버전을 업데이트할 경우 IBM에서 Ingress 배치를 필요에 따라 자동으로 변경하지만, Ingress ALB 추가 기능의 빌드 버전은 변경하지 않습니다. 사용자는 최신 Kubernetes 버전 및 Ingress ALB 추가 기능 이미지의 호환성을 확인해야 합니다.

**최소 필요 권한**: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **편집자** 플랫폼 역할

**예제**:

```
ibmcloud ks alb-autoupdate-disable --cluster mycluster
```
{: pre}

### ibmcloud ks alb-autoupdate-enable
{: #cs_alb_autoupdate_enable}

Ingress ALB 추가 기능에 대한 자동 업데이트가 사용 안함으로 설정된 경우에는 자동 업데이트를 다시 사용으로 설정할 수 있습니다. 다음의 빌드 버전이 사용 가능할 때마다 ALB는 최신 빌드로 자동 업데이트됩니다.
{: shortdesc}

```
ibmcloud ks alb-autoupdate-enable --cluster CLUSTER
```
{: pre}

**최소 필요 권한**: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **편집자** 플랫폼 역할

**예제**:

```
ibmcloud ks alb-autoupdate-enable --cluster mycluster
```
{: pre}

### ibmcloud ks alb-autoupdate-get
{: #cs_alb_autoupdate_get}

Ingress ALB 추가 기능에 대한 자동 업데이트가 사용으로 설정되어 있는지와 ALB가 최신 빌드 버전으로 업데이트되었는지 확인하십시오.
{: shortdesc}

```
ibmcloud ks alb-autoupdate-get --cluster CLUSTER
```
{: pre}

**최소 필요 권한**: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **편집자** 플랫폼 역할

**예제**:

```
ibmcloud ks alb-autoupdate-get --cluster mycluster
```
{: pre}

### ibmcloud ks alb-cert-deploy
{: #cs_alb_cert_deploy}

{{site.data.keyword.cloudcerts_long_notm}} 인스턴스의 인증서를 클러스터의 ALB에 배치하거나 업데이트합니다. 같은 {{site.data.keyword.cloudcerts_long_notm}} 인스턴스에서 가져온 인증서만 업데이트할 수 있습니다.
{: shortdesc}

```
ibmcloud ks alb-cert-deploy [--update] --cluster CLUSTER --secret-name SECRET_NAME --cert-crn CERTIFICATE_CRN [--update] [-s]
```
{: pre}

이 명령으로 사용하여 인증서를 가져오면 인증서 시크릿이 `ibm-cert-store`라는 네임스페이스에 작성됩니다. 그런 다음 이 시크릿에 대한 참조는 네임스페이스의 모든 Ingress 리소스가 액세스할 수 있는 `default` 네임스페이스에 작성됩니다. ALB에서 요청을 처리하는 동안 이 참조를 따라 `ibm-cert-store` 네임스페이스에서 인증서 시크릿을 선택하여 사용합니다.

{{site.data.keyword.cloudcerts_short}}로 설정된 [속도 제한](https://cloud.ibm.com/apidocs/certificate-manager#rate-limiting) 내에서 유지하려면 연속적인 `alb-cert-deploy` 및 `alb-cert-deploy --update` 명령 사이에 45초 이상 기다리십시오.
{: note}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **관리자** 플랫폼 역할

<strong>명령 옵션</strong>

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

   <dt><code>--update</code></dt>
   <dd>클러스터의 ALB 시크릿에 대한 인증서를 업데이트합니다.  이 값은 선택사항입니다.</dd>

   <dt><code>--secret-name <em>SECRET_NAME</em></code></dt>
   <dd>ALB 시크릿이 클러스터에서 작성될 때 ALB 시크릿에 대한 이름을 지정합니다. 이 값은 필수입니다. IBM 제공 Ingress 시크릿과 동일한 이름으로 시크릿을 작성하지 않았는지 확인하십시오. <code>ibmcloud ks cluster-get --cluster <cluster_name_or_ID> | grep Ingress</code>를 실행하여 IBM 제공 Ingress 시크릿 이름을 가져올 수 있습니다.</dd>

   <dt><code>--cert-crn <em>CERTIFICATE_CRN</em></code></dt>
   <dd>인증서 CRN입니다. 이 값은 필수입니다.</dd>

   <dt><code>-s</code></dt>
   <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>
   </dl>

**예제**:

ALB 시크릿 배치 예:

   ```
   ibmcloud ks alb-cert-deploy --secret-name my_alb_secret --cluster my_cluster --cert-crn crn:v1:staging:public:cloudcerts:us-south:a/06580c923e40314421d3b6cb40c01c68:0db4351b-0ee1-479d-af37-56a4da9ef30f:certificate:4bc35b7e0badb304e60aef00947ae7ff
   ```
   {: pre}

기존 ALB 시크릿 업데이트 예:

 ```
 ibmcloud ks alb-cert-deploy --update --secret-name my_alb_secret --cluster my_cluster --cert-crn crn:v1:staging:public:cloudcerts:us-south:a/06580c923e40314421d3b6cb40c01c68:0db4351b-0ee1-479d-af37-56a4da9ef30f:certificate:7e21fde8ee84a96d29240327daee3eb2
 ```
 {: pre}


### ibmcloud ks alb-cert-get
{: #cs_alb_cert_get}

인증서를 {{site.data.keyword.cloudcerts_short}}에서 클러스터의 ALB로 가져온 경우 TLS 인증서 정보(예: TLS 인증서와 연관된 시크릿)를 봅니다.
{: shortdesc}

```
ibmcloud ks alb-cert-get --cluster CLUSTER [--secret-name SECRET_NAME] [--cert-crn CERTIFICATE_CRN] [--json] [-s]
```
{: pre}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **관리자** 플랫폼 역할

<strong>명령 옵션</strong>

  <dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
  <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

  <dt><code>--secret-name <em>SECRET_NAME</em></code></dt>
  <dd>ALB 시크릿의 이름입니다. 이 값은 클러스터의 특정 ALB 시크릿에 관한 정보를 가져오는 데 필요합니다.</dd>

  <dt><code>--cert-crn <em>CERTIFICATE_CRN</em></code></dt>
  <dd>인증서 CRN입니다. 이 값은 클러스터의 특정 인증서 CRN과 일치하는 모든 ALB 시크릿에 관한 정보를 가져오는 데 필요합니다.</dd>

  <dt><code>--json</code></dt>
  <dd>명령 출력을 JSON 형식으로 인쇄합니다.  이 값은 선택사항입니다.</dd>

  <dt><code>-s</code></dt>
  <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>
  </dl>

**예제**:

 ALB 시크릿에 관한 정보 가져오기 예:

 ```
 ibmcloud ks alb-cert-get --cluster my_cluster --secret-name my_alb_secret
 ```
 {: pre}

 지정된 인증서 CRN과 일치하는 모든 ALB 시크릿에 관한 정보 가져오기 예:

 ```
 ibmcloud ks alb-cert-get --cluster my_cluster --cert-crn  crn:v1:staging:public:cloudcerts:us-south:a/06580c923e40314421d3b6cb40c01c68:0db4351b-0ee1-479d-af37-56a4da9ef30f:certificate:4bc35b7e0badb304e60aef00947ae7ff
 ```
 {: pre}


### ibmcloud ks alb-cert-rm
{: #cs_alb_cert_rm}

인증서를 {{site.data.keyword.cloudcerts_short}}에서 클러스터의 ALB로 가져온 경우 클러스터에서 시크릿을 제거합니다.
{: shortdesc}

```
ibmcloud ks alb-cert-rm --cluster CLUSTER [--secret-name SECRET_NAME] [--cert-crn CERTIFICATE_CRN] [-s]
```
{: pre}

{{site.data.keyword.cloudcerts_short}}로 설정된 [속도 제한](https://cloud.ibm.com/apidocs/certificate-manager#rate-limiting) 내에서 유지하려면 연속적인 `alb-cert-rm` 명령 사이에 45초 이상 기다리십시오.
{: note}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **관리자** 플랫폼 역할

<strong>명령 옵션</strong>

  <dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
  <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

  <dt><code>--secret-name <em>SECRET_NAME</em></code></dt>
  <dd>ALB 시크릿의 이름입니다. 이 값은 클러스터의 특정 ALB 시크릿을 제거하는 데 필요합니다.</dd>

  <dt><code>--cert-crn <em>CERTIFICATE_CRN</em></code></dt>
  <dd>인증서 CRN입니다. 이 값은 클러스터의 특정 인증서 CRN과 일치하는 모든 ALB 시크릿을 제거하는 데 필요합니다.</dd>

  <dt><code>-s</code></dt>
  <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>

  </dl>

**예제**:

 ALB 시크릿 제거 예:

 ```
 ibmcloud ks alb-cert-rm --cluster my_cluster --secret-name my_alb_secret
 ```
 {: pre}

 지정된 인증서 CRN과 일치하는 모든 ALB 시크릿 제거 예:

 ```
 ibmcloud ks alb-cert-rm --cluster my_cluster --cert-crn crn:v1:staging:public:cloudcerts:us-south:a/06580c923e40314421d3b6cb40c01c68:0db4351b-0ee1-479d-af37-56a4da9ef30f:certificate:4bc35b7e0badb304e60aef00947ae7ff
 ```
 {: pre}


### ibmcloud ks alb-certs
{: #cs_alb_certs}

{{site.data.keyword.cloudcerts_long_notm}} 인스턴스에서 클러스터의 ALB로 가져온 인증서를 나열합니다.
{: shortdesc}

```
ibmcloud ks alb-certs --cluster CLUSTER [--json] [-s]
```
{: pre}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **관리자** 플랫폼 역할

<strong>명령 옵션</strong>

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>
   <dt><code>--json</code></dt>
   <dd>명령 출력을 JSON 형식으로 인쇄합니다.  이 값은 선택사항입니다.</dd>
   <dt><code>-s</code></dt>
   <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>
   </dl>

**예제**:

 ```
 ibmcloud ks alb-certs --cluster my_cluster
 ```
 {: pre}

### ibmcloud ks alb-configure
{: #cs_alb_configure}

표준 클러스터에서 ALB를 사용 또는 사용 안함으로 설정합니다. 기본적으로 공용 ALB는 사용 가능합니다.
{: shortdesc}

```
ibmcloud ks alb-configure --albID ALB_ID [--enable] [--user-ip USER_IP] [--disable] [--disable-deployment] [-s]
```
{: pre}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **편집자** 플랫폼 역할

**명령 옵션**:

   <dl>
   <dt><code><em>--albID </em>ALB_ID</code></dt>
   <dd>ALB의 ID입니다. 클러스터에서 ALB의 ID를 보려면 <code>ibmcloud ks albs <em>--cluster </em>CLUSTER</code>를 실행하십시오. 이 값은 필수입니다.</dd>

   <dt><code>--enable</code></dt>
   <dd>클러스터에서 ALB를 사용으로 설정하려면 이 플래그를 포함하십시오.</dd>

   <dt><code>--disable</code></dt>
   <dd>클러스터에서 ALB를 사용 안함으로 설정하려면 이 플래그를 포함하십시오.</dd>

   <dt><code>--disable-deployment</code></dt>
   <dd>IBM 제공 ALB 배치를 사용 안함으로 설정하려면 이 플래그를 포함시키십시오. 이 플래그는 Ingress 제어기를 노출하는 데 사용되는 IBM 제공 Ingress 하위 도메인 또는 로드 밸런서 서비스에 대한 DNS 등록을 제거하지 않습니다.</dd>

   <dt><code>--user-ip <em>USER_IP</em></code></dt>
   <dd><ul>
     <li>이 매개변수는 사설 ALB를 사용으로 설정하는 데만 사용할 수 있습니다.</li>
     <li>사설 ALB는 사용자 제공 사설 서브넷의 IP 주소로 배치됩니다. IP 주소가 제공되지 않으면 ALB는 클러스터를 작성할 때 자동으로 프로비저닝된 포터블 사설 서브넷의 사설 IP 주소로 배치됩니다.</li>
    </ul></dd>

   <dt><code>-s</code></dt>
   <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>

   </dl>

**예제**:

  ALB 사용 예:

  ```
  ibmcloud ks alb-configure --albID private-cr18a61a63a6a94b658596aa93a087aaa9-alb1 --enable
  ```
  {: pre}

  사용자 제공 IP 주소로 ALB를 사용으로 설정하는 예:

  ```
  ibmcloud ks alb-configure --albID private-cr18a61a63a6a94b658596aa93a087aaa9-alb1 --enable --user-ip user_ip
  ```
  {: pre}

  ALB 사용 안함 예:

  ```
  ibmcloud ks alb-configure --albID public-cr18a61a63a6a94b658596aa93a087aaa9-alb1 --disable
  ```
  {: pre}


### ibmcloud ks alb-get
{: #cs_alb_get}

ALB의 세부사항을 봅니다.
{: shortdesc}

```
ibmcloud ks alb-get --albID ALB_ID [--json] [-s]
```
{: pre}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **뷰어** 플랫폼 역할

<strong>명령 옵션</strong>:

   <dl>
   <dt><code><em>--albID </em>ALB_ID</code></dt>
   <dd>ALB의 ID입니다. 클러스터에서 ALB의 ID를 보려면 <code>ibmcloud ks albs --cluster <em>CLUSTER</em></code>를 실행하십시오. 이 값은 필수입니다.</dd>

   <dt><code>--json</code></dt>
   <dd>명령 출력을 JSON 형식으로 인쇄합니다.  이 값은 선택사항입니다.</dd>

  <dt><code>-s</code></dt>
  <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>

   </dl>

**예제**:

  ```
  ibmcloud ks alb-get --albID public-cr18a61a63a6a94b658596aa93a087aaa9-alb1
  ```
  {: pre}

### ibmcloud ks alb-rollback
{: #cs_alb_rollback}

ALB 팟(Pod)이 최근에 업데이트되었지만 ALB의 사용자 정의 구성이 최신 빌드의 영향을 받는 경우, ALB 팟(Pod)이 실행되던 이전 빌드로 업데이트를 롤백할 수 있습니다. 업데이트를 롤백하면 ALB 팟(Pod)의 자동 업데이트가 사용 안함으로 설정됩니다. 자동 업데이트를 다시 사용으로 설정하려면 [`alb-autoupdate-enable` 명령](#cs_alb_autoupdate_enable)을 사용하십시오.
{: shortdesc}

```
ibmcloud ks alb-rollback --cluster CLUSTER
```
{: pre}

**최소 필요 권한**: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **편집자** 플랫폼 역할

**예제**:

```
ibmcloud ks alb-rollback --cluster mycluster
```
{: pre}

###   ibmcloud ks alb-types
{: #cs_alb_types}

지역에서 지원되는 ALB 유형을 봅니다.
{: shortdesc}

```
ibmcloud ks alb-types [--json] [-s]
```
{: pre}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **뷰어** 플랫폼 역할

<strong>명령 옵션</strong>:

  <dl>
  <dt><code>--json</code></dt>
  <dd>명령 출력을 JSON 형식으로 인쇄합니다.  이 값은 선택사항입니다.</dd>

  <dt><code>-s</code></dt>
  <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>
  </dl>

**예제**:

  ```
  ibmcloud ks alb-types
  ```
  {: pre}

### ibmcloud ks alb-update
{: #cs_alb_update}

Ingress ALB 추가 기능의 자동 업데이트가 사용 안함으로 설정되어 있으며 추가 기능을 업데이트하고자 하는 경우에는 ALB 팟(Pod)의 일회성 업데이트를 강제 실행할 수 있습니다. 추가 기능의 수동 업데이트를 선택하는 경우에는 클러스터의 모든 ALB 팟(Pod)이 최신 빌드로 업데이트됩니다. 개별 ALB를 업데이트하거나 추가 기능을 업데이트할 빌드를 선택할 수는 없습니다. 자동 업데이트는 사용 안함으로 계속 설정됩니다.
{: shortdesc}

```
ibmcloud ks alb-update --cluster CLUSTER
```
{: pre}

클러스터의 Kubernetes 주 버전 또는 부 버전을 업데이트할 경우 IBM에서 Ingress 배치를 필요에 따라 자동으로 변경하지만, Ingress ALB 추가 기능의 빌드 버전은 변경하지 않습니다. 사용자는 최신 Kubernetes 버전 및 Ingress ALB 추가 기능 이미지의 호환성을 확인해야 합니다.

**최소 필요 권한**: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **편집자** 플랫폼 역할

**예제**:

```
ibmcloud ks alb-update --cluster <cluster_name_or_ID>
```
{: pre}

###     ibmcloud ks albs
{: #cs_albs}

클러스터에서 모든 ALB의 상태를 봅니다. ALB ID가 리턴되지 않으면 클러스터에 포터블 서브넷이 없습니다. 서브넷을 [작성](#cs_cluster_subnet_create)하거나 클러스터에 [추가](#cs_cluster_subnet_add)할 수 있습니다.
{: shortdesc}

```
ibmcloud ks albs --cluster CLUSTER [--json] [-s]
```
{: pre}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **뷰어** 플랫폼 역할

<strong>명령 옵션</strong>:

   <dl>
   <dt><code><em>--cluster </em>CLUSTER</code></dt>
   <dd>사용 가능한 ALB를 나열하는 클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

   <dt><code>--json</code></dt>
   <dd>명령 출력을 JSON 형식으로 인쇄합니다.  이 값은 선택사항입니다.</dd>

   <dt><code>-s</code></dt>
   <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>

   </dl>

**예제**:

  ```
  ibmcloud ks albs --cluster my_cluster
  ```
  {: pre}


<br />



## 인프라 명령
{: #infrastructure_commands}

###         ibmcloud ks credential-get
{: #cs_credential_get}

서로 다른 인증 정보를 사용하여 IBM Cloud 인프라 포트폴리오에 액세스하도록 IBM Cloud 계정을 설정한 경우에는 현재 대상으로 지정된 지역 및 리소스 그룹에 대한 인프라 사용자 이름을 가져오십시오.
{: shortdesc}

```
ibmcloud ks credential-get [-s] [--json]
```
{: pre}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **뷰어** 플랫폼 역할

<strong>명령 옵션</strong>:

 <dl>
 <dt><code>--json</code></dt>
 <dd>명령 출력을 JSON 형식으로 인쇄합니다.  이 값은 선택사항입니다.</dd>
 <dt><code>-s</code></dt>
 <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>
 </dl>

**예:**
```
        ibmcloud ks credential-get
```
{: pre}

**출력 예:**
```
        Infrastructure credentials for user name user@email.com set for resource group default.
```

### ibmcloud ks credential-set
{: #cs_credentials_set}

{{site.data.keyword.containerlong_notm}} 리소스 그룹 및 지역에 대한 IBM Cloud 인프라(SoftLayer) 계정 인증 정보를 설정합니다.
{: shortdesc}

```
ibmcloud ks credential-set --infrastructure-api-key API_KEY --infrastructure-username USERNAME [-s]
```
{: pre}

{{site.data.keyword.Bluemix_notm}} 종량과금제 계정이 있는 경우 기본적으로 IBM Cloud 인프라(SoftLayer) 포트폴리오에 대한 액세스 권한이 제공됩니다. 그러나 인프라를 정렬하기 위해 이미 보유하고 있는 다른 IBM Cloud 인프라(SoftLayer) 계정을 사용하려고 할 수 있습니다. 이 명령을 사용하여 인프라 계정을 {{site.data.keyword.Bluemix_notm}} 계정에 연결할 수 있습니다.

IBM Cloud 인프라(SoftLayer) 인증 정보가 지역 및 리소스 그룹에 대해 수동으로 설정된 경우, 이러한 인증 정보는 리소스 그룹 내 해당 지역의 모든 클러스터에 대한 인프라를 주문하는 데 사용됩니다. 이러한 인증 정보는 해당 리소스 그룹 및 지역에 대해 [{{site.data.keyword.Bluemix_notm}}IAM API 키](#cs_api_key_info)가 이미 존재하는 경우에도 인프라 권한을 판별하는 데 사용됩니다. 인증 정보를 저장하는 사용자에게 인프라를 정렬하기 위한 필수 권한이 없는 경우 클러스터 작성 또는 작업자 노드 다시 로드와 같은 인프라 관련 조치에 실패할 수 있습니다.

동일한 {{site.data.keyword.containerlong_notm}} 리소스 그룹 및 지역에 대해서는 여러 인증 정보를 설정할 수 없습니다.

이 명령을 사용하기 전에, 해당 인증 정보가 사용되는 사용자에게 필수 [{{site.data.keyword.containerlong_notm}} 및 IBM Cloud 인프라(SoftLayer) 권한](/docs/containers?topic=containers-users#users)이 있는지 확인하십시오.
{: important}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **관리자** 플랫폼 역할

<strong>명령 옵션</strong>:

   <dl>
   <dt><code>--infrastructure-username <em>USERNAME</em></code></dt>
   <dd>IBM Cloud 인프라(SoftLayer) 계정 API 사용자 이름입니다. 이 값은 필수입니다. 참고로, 인프라 API 사용자 이름은 IBM ID와 동일하지 않습니다. 인프라 API 사용자 이름을 보려면 다음을 수행하십시오.
   <ol>
   <li>[{{site.data.keyword.Bluemix_notm}} 콘솔![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com)에 로그인하십시오.
   <li>메뉴 표시줄에서 **관리 > 액세스(IAM)**를 선택하십시오.
   <li>**사용자** 탭을 선택한 후 사용자 이름을 클릭하십시오.
   <li>**API 키** 분할창에서 **클래식 인프라 API 키** 항목을 찾고 **조치 메뉴** ![조치 메뉴 아이콘](../icons/action-menu-icon.svg "조치 메뉴 아이콘") **> 세부사항**을 클릭하십시오.
   <li>API 사용자 이름을 복사하십시오.
   </ol></dd>


   <dt><code>--infrastructure-api-key <em>API_KEY</em></code></dt>
   <dd>IBM Cloud 인프라(SoftLayer) 계정 API 키입니다. 이 값은 필수입니다. 인프라 API 키를 보거나 생성하려면 다음을 수행하십시오.
  <li>[{{site.data.keyword.Bluemix_notm}} 콘솔![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com)에 로그인하십시오.
  <li>메뉴 표시줄에서 **관리 > 액세스(IAM)**를 선택하십시오.
  <li>**사용자** 탭을 선택한 후 사용자 이름을 클릭하십시오.
  <li>**API 키** 분할창에서 **클래식 인프라 API 키** 항목을 찾고 **조치 메뉴** ![조치 메뉴 아이콘](../icons/action-menu-icon.svg "조치 메뉴 아이콘") **> 세부사항**을 클릭하십시오. 클래식 인프라 API 키가 표시되지 않으면 **{{site.data.keyword.Bluemix_notm}} API 키 작성**을 클릭하여 이를 생성하십시오.
  <li>API 사용자 이름을 복사하십시오.
  </ol>
  </dd>
  <dt><code>-s</code></dt>
  <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>

  </dl>

**예제**:

  ```
  ibmcloud ks credential-set --infrastructure-api-key <api_key> --infrastructure-username dbmanager
  ```
  {: pre}


### ibmcloud ks credential-unset
{: #cs_credentials_unset}

{{site.data.keyword.containerlong_notm}} 지역에서 IBM Cloud 인프라(SoftLayer) 계정 인증 정보를 제거하십시오.
{: shortdesc}

인증 정보를 제거한 후에는 [{{site.data.keyword.Bluemix_notm}}IAM API 키](#cs_api_key_info)가 IBM Cloud 인프라(SoftLayer)에서 리소스를 정렬하는 데 사용됩니다.

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **관리자** 플랫폼 역할

```
ibmcloud ks credential-unset [-s]
```
{: pre}

<strong>명령 옵션</strong>:

  <dl>
  <dt><code>-s</code></dt>
  <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>
  </dl>


### ibmcloud ks machine-types
{: #cs_machine_types}

작업자 노드에 대해 사용 가능한 머신 유형의 목록을 봅니다. 머신 유형은 구역에 따라 다릅니다. 각각의 머신 유형에는 클러스터의 각 작업자 노드에 대한 가상 CPU, 메모리 및 디스크 공간의 양이 포함됩니다. 기본적으로, 모든 컨테이너 데이터가 저장된 보조 스토리지 디스크 디렉토리는 AES 256비트 LUKS 암호화를 통해 암호화됩니다. `disable-disk-encrypt` 옵션이 클러스터 작성 중에 포함된 경우에는 호스트의 컨테이너 런타임 데이터가 암호화되지 않습니다. [암호화에 대해 자세히 알아보십시오](/docs/containers?topic=containers-security#encrypted_disk).
{:shortdesc}

```
ibmcloud ks machine-types --zone ZONE [--json] [-s]
```
{: pre}

공유 또는 전용 하드웨어에서 가상 머신으로서 또는 베어메탈에서 실제 머신으로서 작업자 노드를 프로비저닝할 수 있습니다. [머신 유형 옵션에 대해 자세히 알아보십시오](/docs/containers?topic=containers-plan_clusters#shared_dedicated_node).

<strong>최소 필요 권한</strong>: 없음

<strong>명령 옵션</strong>:

   <dl>
   <dt><code>--zone <em>ZONE</em></code></dt>
   <dd>사용 가능한 머신 유형을 나열하려는 구역을 입력하십시오. 이 값은 필수입니다. [사용 가능한 구역](/docs/containers?topic=containers-regions-and-zones#zones)을 검토하십시오.</dd>

   <dt><code>--json</code></dt>
  <dd>명령 출력을 JSON 형식으로 인쇄합니다.  이 값은 선택사항입니다.</dd>

  <dt><code>-s</code></dt>
  <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>
  </dl>

**예제**:

  ```
  ibmcloud ks machine-types --zone dal10
  ```
  {: pre}



### ibmcloud ks <ph class="ignoreSpelling">vlans</ph>
{: #cs_vlans}

IBM Cloud 인프라(SoftLayer) 계정에서 구역에 대해 사용 가능한 공용 및 사설 VLAN을 나열합니다. 사용 가능한 VLAN을 나열하려면 유료 계정이 있어야 합니다.
{: shortdesc}

```
ibmcloud ks vlans --zone ZONE [--all] [--json] [-s]
```
{: pre}

<strong>최소 필요 권한</strong>:
* 구역에서 클러스터가 연결된 VLAN 보기: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **뷰어** 플랫폼 역할
* 구역에서 사용 가능한 모든 VLAN 나열: {{site.data.keyword.containerlong_notm}}의 지역에 대한 **뷰어** 플랫폼 역할

<strong>명령 옵션</strong>:

   <dl>
   <dt><code>--zone <em>ZONE</em></code></dt>
   <dd>사설 및 공용 VLAN을 나열하려는 구역을 입력하십시오. 이 값은 필수입니다. [사용 가능한 구역](/docs/containers?topic=containers-regions-and-zones#zones)을 검토하십시오.</dd>

   <dt><code>--all</code></dt>
   <dd>사용 가능한 모든 VLAN을 나열합니다. 기본적으로 VLAN은 유효한 VLAN만 표시되도록 필터링됩니다. 올바른 상태가 되려면 VLAN은 로컬 디스크 스토리지로 작업자를 호스팅할 수 있는 인프라와 연관되어야 합니다.</dd>

   <dt><code>--json</code></dt>
  <dd>명령 출력을 JSON 형식으로 인쇄합니다.  이 값은 선택사항입니다.</dd>

  <dt><code>-s</code></dt>
  <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>
   </dl>

**예제**:

  ```
  ibmcloud ks vlans --zone dal10
  ```
  {: pre}


###   ibmcloud ks vlan-spanning-get
{: #cs_vlan_spanning_get}

IBM Cloud 인프라(SoftLayer) 계정에 대한 VLAN Spanning 상태를 보십시오. VLAN Spanning은 지정된 VLAN과는 무관하게 사설 네트워크를 통해 계정의 모든 디바이스가 서로 간에 통신할 수 있도록 합니다.
{: shortdesc}

```
ibmcloud ks vlan-spanning-get [--json] [-s]
```
{: pre}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **뷰어** 플랫폼 역할

<strong>명령 옵션</strong>:

   <dl>
    <dt><code>--json</code></dt>
      <dd>명령 출력을 JSON 형식으로 인쇄합니다.  이 값은 선택사항입니다.</dd>

    <dt><code>-s</code></dt>
      <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>
   </dl>

**예제**:

  ```
  ibmcloud ks vlan-spanning-get
  ```
  {: pre}

<br />


## 로깅 명령
{: #logging_commands}

### ibmcloud ks logging-autoupdate-disable
{: #cs_log_autoupdate_disable}

특정 클러스터에서 Fluentd 팟(Pod)의 자동 업데이트를 사용 안함으로 설정합니다. 클러스터의 주 또는 부 Kubernetes 버전을 업데이트하는 경우, IBM은 Fluentd configmap에 대한 필수 변경사항을 자동으로 작성하지만 로깅 추가 기능에 대한 Fluentd의 빌드 버전은 변경하지 않습니다. 사용자는 최신 Kubernetes 버전 및 추가 기능 이미지의 호환성을 확인해야 합니다.
{: shortdesc}

```
ibmcloud ks logging-autoupdate-disable --cluster CLUSTER
```
{: pre}

<strong>명령 옵션</strong>:

<dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>Fluentd 추가 기능의 자동 업데이트를 사용 안함으로 설정할 클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>
</dl>

### ibmcloud ks logging-autoupdate-enable
{: #cs_log_autoupdate_enable}

특정 클러스터에서 Fluentd 팟(Pod)의 자동 업데이트를 사용으로 설정합니다. Fluentd 팟(Pod)은 새 빌드 버전이 사용 가능할 때 자동으로 업데이트됩니다.
{: shortdesc}

```
ibmcloud ks logging-autoupdate-enable --cluster CLUSTER
```
{: pre}

<strong>명령 옵션</strong>:

<dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>Fluentd 추가 기능의 자동 업데이트를 사용으로 설정할 클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>
</dl>

### ibmcloud ks logging-autoupdate-get
{: #cs_log_autoupdate_get}

Fluentd 팟(Pod)이 특정 클러스터에서 자동 업데이트로 설정되었는지 여부를 확인합니다.
{: shortdesc}

```
ibmcloud ks logging-autoupdate-get --cluster CLUSTER
```
{: pre}

<strong>명령 옵션</strong>:

<dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>Fluentd 추가 기능의 자동 업데이트가 사용으로 설정되었는지 여부를 확인할 클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>
</dl>

### ibmcloud ks logging-config-create
{: #cs_logging_create}

로깅 구성을 작성합니다. 이 명령을 사용하여 컨테이너, 애플리케이션, 작업자 노드, Kubernetes 클러스터 및 Ingress 애플리케이션 로드 밸런서에 대한 로그를 {{site.data.keyword.loganalysisshort_notm}} 또는 외부 syslog 서버로 전달할 수 있습니다.
{: shortdesc}

```
ibmcloud ks logging-config-create --cluster CLUSTER --logsource LOG_SOURCE --type LOG_TYPE [--namespace KUBERNETES_NAMESPACE] [--hostname LOG_SERVER_HOSTNAME_OR_IP] [--port LOG_SERVER_PORT] [--space CLUSTER_SPACE] [--org CLUSTER_ORG] [--app-containers CONTAINERS] [--app-paths PATHS_TO_LOGS] [--syslog-protocol PROTOCOL]  [--json] [--skip-validation] [--force-update][-s]
```
{: pre}

<strong>최소 필요 권한</strong>: `kube-audit`을 제외한 모든 로그 소스의 경우 클러스터에 대한 **편집자** 플랫폼 역할 또는 `kube-audit` 로그 소스의 경우 클러스터에 대한 **관리자** 플랫폼 역할

<strong>명령 옵션</strong>:

<dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>클러스터의 이름 또는 ID입니다.</dd>

  <dt><code>--logsource <em>LOG_SOURCE</em></code></dt>
    <dd>로그 전달을 사용할 로그 소스입니다. 이 인수는 구성을 적용할 로그 소스의 쉼표로 구분된 목록을 지원합니다. 허용되는 값은 <code>container</code>, <code>application</code>, <code>worker</code>, <code>kubernetes</code>, <code>storage</code>, <code>ingress</code> 및 <code>kube-audit</code>입니다. 로그 소스를 제공하지 않으면 구성이 <code>container</code> 및 <code>ingress</code>에 대해 작성됩니다.</dd>

  <dt><code>--type <em>LOG_TYPE</em></code></dt>
    <dd>로그를 전달할 위치입니다. 옵션은 <code>ibm</code>(로그를 {{site.data.keyword.loganalysisshort_notm}}로 전달) 및 <code>syslog</code>(로그를 외부 서버로 전달)입니다.</dd>

  <dt><code>--namespace <em>KUBERNETES_NAMESPACE</em></code></dt>
    <dd>로그를 전달할 Kubernetes 네임스페이스입니다. <code>ibm-system</code> 및 <code>kube-system</code> Kubernetes 네임스페이스의 경우 로그 전달이 지원되지 않습니다. 이 값은 container 로그 소스에 대해서만 유효하며 선택사항입니다. 네임스페이스를 지정하지 않으면 컨테이너의 모든 네임스페이스가 이 구성을 사용합니다.</dd>

  <dt><code>--hostname <em>LOG_SERVER_HOSTNAME</em></code></dt>
    <dd>로깅 유형이 <code>syslog</code>인 경우 로그 콜렉터 서버의 호스트 이름 또는 IP 주소입니다. 이 값은 <code>syslog</code>에 필수입니다. 로깅 유형이 <code>ibm</code>인 경우 {{site.data.keyword.loganalysislong_notm}} 유입 URL입니다. 사용 가능한 유입 URL 목록은 [여기](/docs/services/CloudLogAnalysis?topic=cloudloganalysis-log_ingestion#log_ingestion_urls)에서 찾을 수 있습니다. 유입 URL을 지정하지 않으면 클러스터가 작성된 지역의 엔드포인트가 사용됩니다.</dd>

  <dt><code>--port <em>LOG_SERVER_PORT</em></code></dt>
    <dd>로그 콜렉터 서버의 포트입니다. 이 값은 선택사항입니다. 포트를 지정하지 않으면 표준 포트 <code>514</code>가 <code>syslog</code>에 사용되고 표준 포트 <code>9091</code>이 <code>ibm</code>에 사용됩니다.</dd>

  <dt><code>--space <em>CLUSTER_SPACE</em></code></dt>
    <dd>선택사항: 로그를 전송할 Cloud Foundry 영역의 이름입니다. 이 값은 <code>ibm</code> 로그 유형에 대해서만 유효하며 선택사항입니다. 영역을 지정하지 않으면 로그는 계정 레벨로 전송됩니다. 영역을 지정하는 경우에는 조직도 지정해야 합니다.</dd>

  <dt><code>--org <em>CLUSTER_ORG</em></code></dt>
    <dd>선택사항: 영역이 위치한 Cloud Foundry 조직의 이름입니다. 이 값은 <code>ibm</code> 로그 유형에 대해서만 유효하며, 영역을 지정한 경우 필수입니다.</dd>

  <dt><code>--app-paths</code></dt>
    <dd>앱이 로그를 기록하는 컨테이너 상의 경로입니다. 소스 유형이 <code>application</code>인 로그를 전달하려면 경로를 제공해야 합니다. 두 개 이상의 경로를 지정하려면 쉼표로 구분된 목록을 사용하십시오. 이 값은 <code>application</code> 로그 소스의 경우 필수입니다. 예: <code>/var/log/myApp1/&ast;,/var/log/myApp2/&ast;</code></dd>

  <dt><code>--syslog-protocol</code></dt>
    <dd>로깅 유형이 <code>syslog</code>인 경우 사용되는 전송 계층 프로토콜입니다. 지원되는 값은 <code>tcp</code>, <code>tls</code> 및 기본값 <code>udp</code>입니다. <code>udp</code> 프로토콜을 사용하여 rsyslog 서버에 전달하는 경우 1KB를 초과하는 로그는 잘립니다.</dd>

  <dt><code>--app-containers</code></dt>
    <dd>앱에서 로그를 전달하기 위해 앱이 포함된 컨테이너의 이름을 지정할 수 있습니다. 쉼표로 구분된 목록을 사용하여 두 개 이상의 컨테이너를 지정할 수 있습니다. 컨테이너가 지정되지 않은 경우 로그는 사용자가 제공한 경로가 포함된 모든 컨테이너에서 전달됩니다. 이 옵션은 <code>application</code> 로그 소스에 대해서만 유효합니다.</dd>

  <dt><code>--json</code></dt>
    <dd>명령 출력을 JSON 형식으로 인쇄합니다.  이 값은 선택사항입니다.</dd>

  <dt><code>--skip-validation</code></dt>
    <dd>조직 및 영역 이름을 지정할 때 이러한 항목의 유효성 검증을 건너뜁니다. 유효성 검증을 건너뛰면 처리 시간이 줄어들지만 올바르지 않은 로깅 구성은 로그를 올바르게 전달하지 않습니다.  이 값은 선택사항입니다.</dd>

  <dt><code>--force-update</code></dt>
    <dd>최신 버전으로 Fluentd 팟(Pod)의 업데이트를 강제 실행합니다. 로깅 구성을 변경하려면 Fluentd가 최신 버전이어야 합니다.</dd>

    <dt><code>-s</code></dt>
    <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>
</dl>

**예제**:

기본 포트 9091에서 `container` 로그 소스로부터 전달하는 로그 유형 `ibm`의 예:

  ```
  ibmcloud ks logging-config-create my_cluster --logsource container --namespace my_namespace --hostname ingest.logging.ng.bluemix.net --type ibm
  ```
  {: pre}

기본 포트 514에서 `container` 로그 소스로부터 전달하는 로그 유형 `syslog`의 예:

  ```
  ibmcloud ks logging-config-create my_cluster --logsource container --namespace my_namespace  --hostname 169.xx.xxx.xxx --type syslog
  ```
  {: pre}

기본 포트가 아닌 다른 포트에서 `ingress` 소스로부터 로그를 전달하는 로그 유형 `syslog`의 예:

  ```
  ibmcloud ks logging-config-create --cluster my_cluster --logsource container --hostname 169.xx.xxx.xxx --port 5514 --type syslog
  ```
  {: pre}


### ibmcloud ks logging-config-get
{: #cs_logging_get}

클러스터에 대한 모든 로그 전달 구성을 보거나 로그 소스를 기반으로 로깅 구성을 필터링합니다.
{: shortdesc}

```
ibmcloud ks logging-config-get --cluster CLUSTER [--logsource LOG_SOURCE] [--json] [-s]
```
{: pre}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **뷰어** 플랫폼 역할

<strong>명령 옵션</strong>:

 <dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

  <dt><code>--logsource <em>LOG_SOURCE</em></code></dt>
    <dd>필터링하려는 로그 소스의 유형입니다. 클러스터에 있는 이 로그 소스의 로깅 구성만 리턴됩니다. 허용되는 값은 <code>container</code>, <code>storage</code>, <code>application</code>, <code>worker</code>, <code>kubernetes</code>, <code>ingress</code> 및 <code>kube-audit</code>입니다.  이 값은 선택사항입니다.</dd>

  <dt><code>--show-covering-filters</code></dt>
    <dd>더 이상 사용되지 않는 이전 필터를 렌더링하는 로깅 필터를 표시합니다.</dd>

  <dt><code>--json</code></dt>
    <dd>명령 출력을 JSON 형식으로 인쇄합니다.  이 값은 선택사항입니다.</dd>

  <dt><code>-s</code></dt>
    <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>
 </dl>

**예제**:

  ```
  ibmcloud ks logging-config-get --cluster my_cluster --logsource worker
  ```
  {: pre}


### ibmcloud ks logging-config-refresh
{: #cs_logging_refresh}

클러스터의 로깅 구성을 새로 고칩니다. 이렇게 하면 클러스터의 영역 레벨에 전달되는 모든 로깅 구성의 로깅 토큰이 새로 고쳐집니다.
{: shortdesc}

```
ibmcloud ks logging-config-refresh --cluster CLUSTER [--force-update] [-s]
```
{: pre}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **편집자** 플랫폼 역할

<strong>명령 옵션</strong>:

<dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

   <dt><code>--force-update</code></dt>
     <dd>최신 버전으로 Fluentd 팟(Pod)의 업데이트를 강제 실행합니다. 로깅 구성을 변경하려면 Fluentd가 최신 버전이어야 합니다.</dd>

   <dt><code>-s</code></dt>
     <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>
</dl>

**예제**:

  ```
  ibmcloud ks logging-config-refresh --cluster my_cluster
  ```
  {: pre}



### ibmcloud ks logging-config-rm
{: #cs_logging_rm}

클러스터에 대한 한 개의 로그 전달 구성을 삭제하거나 모든 로깅 구성을 삭제합니다. 이렇게 하면 원격 syslog 서버 또는 {{site.data.keyword.loganalysisshort_notm}}로의 로그 전달이 중지됩니다.
{: shortdesc}

```
ibmcloud ks logging-config-rm --cluster CLUSTER [--id LOG_CONFIG_ID] [--all] [--force-update] [-s]
```
{: pre}

<strong>최소 필요 권한</strong>: `kube-audit`을 제외한 모든 로그 소스의 경우 클러스터에 대한 **편집자** 플랫폼 역할 또는 `kube-audit` 로그 소스의 경우 클러스터에 대한 **관리자** 플랫폼 역할

<strong>명령 옵션</strong>:

<dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

  <dt><code>--id <em>LOG_CONFIG_ID</em></code></dt>
   <dd>로깅 구성 ID입니다(단일 로깅 구성을 제거하려는 경우).</dd>

  <dt><code>--all</code></dt>
   <dd>클러스터에서 모든 로깅 구성을 제거하는 플래그입니다.</dd>

  <dt><code>--force-update</code></dt>
    <dd>최신 버전으로 Fluentd 팟(Pod)의 업데이트를 강제 실행합니다. 로깅 구성을 변경하려면 Fluentd가 최신 버전이어야 합니다.</dd>

   <dt><code>-s</code></dt>
     <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>
</dl>

**예제**:

  ```
  ibmcloud ks logging-config-rm --cluster my_cluster --id f4bc77c0-ee7d-422d-aabf-a4e6b977264e
  ```
  {: pre}


### ibmcloud ks logging-config-update
{: #cs_logging_update}

로그 전달 구성의 세부사항을 업데이트합니다.
{: shortdesc}

```
ibmcloud ks logging-config-update --cluster CLUSTER --id LOG_CONFIG_ID --type LOG_TYPE  [--namespace NAMESPACE] [--hostname LOG_SERVER_HOSTNAME_OR_IP] [--port LOG_SERVER_PORT] [--space CLUSTER_SPACE] [--org CLUSTER_ORG] [--app-paths PATH] [--app-containers PATH] [--json] [--skipValidation] [--force-update] [-s]
```
{: pre}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **편집자** 플랫폼 역할

<strong>명령 옵션</strong>:

<dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

  <dt><code>--id <em>LOG_CONFIG_ID</em></code></dt>
   <dd>업데이트하려는 로깅 구성 ID입니다. 이 값은 필수입니다.</dd>

  <dt><code>--type <em>LOG_TYPE</em></code></dt>
   <dd>사용하려는 로그 전달 프로토콜입니다. 현재 <code>syslog</code> 및 <code>ibm</code>이 지원됩니다. 이 값은 필수입니다.</dd>

  <dt><code>--namespace <em>NAMESPACE</em></code>
    <dd>로그를 전달할 Kubernetes 네임스페이스입니다. <code>ibm-system</code> 및 <code>kube-system</code> Kubernetes 네임스페이스의 경우 로그 전달이 지원되지 않습니다. 이 값은 <code>container</code> 로그 소스에 대해서만 유효합니다. 네임스페이스를 지정하지 않으면 컨테이너의 모든 네임스페이스가 이 구성을 사용합니다.</dd>

  <dt><code>--hostname <em>LOG_SERVER_HOSTNAME</em></code></dt>
   <dd>로깅 유형이 <code>syslog</code>인 경우 로그 콜렉터 서버의 호스트 이름 또는 IP 주소입니다. 이 값은 <code>syslog</code>에 필수입니다. 로깅 유형이 <code>ibm</code>인 경우 {{site.data.keyword.loganalysislong_notm}} 유입 URL입니다. 사용 가능한 유입 URL 목록은 [여기](/docs/services/CloudLogAnalysis?topic=cloudloganalysis-log_ingestion#log_ingestion_urls)에서 찾을 수 있습니다. 유입 URL을 지정하지 않으면 클러스터가 작성된 지역의 엔드포인트가 사용됩니다.</dd>

   <dt><code>--port <em>LOG_SERVER_PORT</em></code></dt>
   <dd>로그 콜렉터 서버의 포트입니다. 로깅 유형이 <code>syslog</code>인 경우 이 값은 선택사항입니다. 포트를 지정하지 않으면 표준 포트 <code>514</code>가 <code>syslog</code>에 사용되고 <code>9091</code>이 <code>ibm</code>에 사용됩니다.</dd>

   <dt><code>--space <em>CLUSTER_SPACE</em></code></dt>
   <dd>선택사항: 로그를 전송할 대상 영역의 이름입니다. 이 값은 <code>ibm</code> 로그 유형에 대해서만 유효하며 선택사항입니다. 영역을 지정하지 않으면 로그는 계정 레벨로 전송됩니다. 영역을 지정하는 경우에는 조직도 지정해야 합니다.</dd>

   <dt><code>--org <em>CLUSTER_ORG</em></code></dt>
   <dd>선택사항: 영역이 위치한 Cloud Foundry 조직의 이름입니다. 이 값은 <code>ibm</code> 로그 유형에 대해서만 유효하며, 영역을 지정한 경우 필수입니다.</dd>

   <dt><code>--app-paths <em>PATH</em>,<em>PATH</em></code></dt>
     <dd>로그를 수집할 컨테이너의 절대 파일 경로입니다. '/var/log/*.log'와 같은 와일드카드를 사용할 수 있지만 '/var/log/**/test.log'와 같은 재귀적 글로브(glob)는 사용할 수 없습니다. 두 개 이상의 경로를 지정하려면 쉼표로 구분된 목록을 사용하십시오. 이 값은 로그 소스를 'application'으로 지정할 때 필요합니다. </dd>

   <dt><code>--app-containers <em>PATH</em>,<em>PATH</em></code></dt>
     <dd>앱이 로깅되는 컨테이너의 경로입니다. 소스 유형이 <code>application</code>인 로그를 전달하려면 경로를 제공해야 합니다. 두 개 이상의 경로를 지정하려면 쉼표로 구분된 목록을 사용하십시오. 예: <code>/var/log/myApp1/&ast;,/var/log/myApp2/&ast;</code></dd>

   <dt><code>--json</code></dt>
    <dd>명령 출력을 JSON 형식으로 인쇄합니다.  이 값은 선택사항입니다.</dd>

   <dt><code>--skipValidation</code></dt>
    <dd>조직 및 영역 이름을 지정할 때 이러한 항목의 유효성 검증을 건너뜁니다. 유효성 검증을 건너뛰면 처리 시간이 줄어들지만 올바르지 않은 로깅 구성은 로그를 올바르게 전달하지 않습니다.  이 값은 선택사항입니다.</dd>

  <dt><code>--force-update</code></dt>
    <dd>최신 버전으로 Fluentd 팟(Pod)의 업데이트를 강제 실행합니다. 로깅 구성을 변경하려면 Fluentd가 최신 버전이어야 합니다.</dd>

   <dt><code>-s</code></dt>
     <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>
  </dl>

**로그 유형 `ibm`**에 대한 예제:

  ```
  ibmcloud ks logging-config-update my_cluster --id f4bc77c0-ee7d-422d-aabf-a4e6b977264e --type ibm
  ```
  {: pre}

**로그 유형 `syslog`**에 대한 예제:

  ```
  ibmcloud ks logging-config-update --cluster my_cluster --id f4bc77c0-ee7d-422d-aabf-a4e6b977264e --hostname localhost --port 5514 --type syslog
  ```
  {: pre}



### ibmcloud ks logging-filter-create
{: #cs_log_filter_create}

로깅 필터를 작성합니다. 이 명령을 사용하여 로깅 구성에 의해 전달되는 로그를 필터링할 수 있습니다.
{: shortdesc}

```
ibmcloud ks logging-filter-create --cluster CLUSTER --type LOG_TYPE [--logging-configs CONFIGS] [--namespace KUBERNETES_NAMESPACE] [--container CONTAINER_NAME] [--level LOGGING_LEVEL] [--message MESSAGE] [--regex-message MESSAGE] [--force-update] [--json] [-s]
```
{: pre}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **편집자** 플랫폼 역할

<strong>명령 옵션</strong>:

<dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>로깅 필터를 작성할 클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

  <dt><code>--type <em>LOG_TYPE</em></code></dt>
    <dd>필터를 적용할 로그의 유형입니다. 현재는 <code>all</code>, <code>container</code> 및 <code>host</code>가 지원됩니다.</dd>

  <dt><code>--logging-configs <em>CONFIGS</em></code></dt>
    <dd>로깅 구성 ID의 쉼표로 구분된 목록입니다. 제공되지 않으면 필터에 전달된 모든 클러스터 로깅 구성에 필터가 적용됩니다. 명령과 함께 <code>--show-matching-configs</code> 플래그를 사용하여 필터와 일치하는 로그 구성을 볼 수 있습니다.  이 값은 선택사항입니다.</dd>

  <dt><code>--namespace <em>KUBERNETES_NAMESPACE</em></code></dt>
    <dd>로그를 필터링할 Kubernetes 네임스페이스입니다.  이 값은 선택사항입니다.</dd>

  <dt><code>--container <em>CONTAINER_NAME</em></code></dt>
    <dd>로그를 필터링할 컨테이너의 이름입니다. 이 플래그는 로그 유형 <code>container</code>를 사용하는 경우에만 적용됩니다.  이 값은 선택사항입니다.</dd>

  <dt><code>--level <em>LOGGING_LEVEL</em></code></dt>
    <dd>지정된 레벨 이하의 로그를 필터링합니다. 허용 가능한 값은 규범적 순서대로 <code>fatal</code>, <code>error</code>, <code>warn/warning</code>, <code>info</code>, <code>debug</code> 및 <code>trace</code>입니다. 이 값은 선택사항입니다. 예를 들어, <code>info</code> 레벨에서 로그를 필터링한 경우에는 <code>debug</code> 및 <code>trace</code> 또한 필터링됩니다. **참고**: 로그 메시지가 JSON 형식이며 level 필드를 포함하는 경우에만 이 플래그를 사용할 수 있습니다. 출력 예: <code>{"log": "hello", "level": "info"}</code></dd>

  <dt><code>--message <em>MESSAGE</em></code></dt>
    <dd>로그의 임의의 위치에 지정된 메시지가 포함되어 있는 로그를 필터링합니다. 이 값은 선택사항입니다. 예: 메시지 "Hello", "!" 및 "Hello, World!"는 로그 "Hello, World!"에 적용됩니다.</dd>

  <dt><code>--regex-message <em>MESSAGE</em></code></dt>
    <dd>로그의 임의의 위치에 정규식으로 작성된 지정된 메시지가 포함되어 있는 로그를 필터링합니다. 이 값은 선택사항입니다. 예: 패턴 "hello [0-9]"는 "hello 1", "hello 2" 및 "hello 9"에 적용됩니다.</dd>

  <dt><code>--force-update</code></dt>
    <dd>최신 버전으로 Fluentd 팟(Pod)의 업데이트를 강제 실행합니다. 로깅 구성을 변경하려면 Fluentd가 최신 버전이어야 합니다.</dd>

  <dt><code>--json</code></dt>
    <dd>명령 출력을 JSON 형식으로 인쇄합니다.  이 값은 선택사항입니다.</dd>

  <dt><code>-s</code></dt>
    <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>
</dl>

**예제**:

이 예는 기본 네임스페이스에 있는 `test-container`라는 이름의 컨테이너에서 전달되는, debug 레벨 이하이며 "GET request"를 포함하는 로그 메시지가 있는 모든 로그를 필터링합니다.

  ```
  ibmcloud ks logging-filter-create --cluster example-cluster --type container --namespace default --container test-container --level debug --message "GET request"
  ```
  {: pre}

이 예는 특정 클러스터에서 전달되는, `info` 레벨 이하의 모든 로그를 필터링합니다. 출력은 JSON으로 리턴됩니다.

  ```
  ibmcloud ks logging-filter-create --cluster example-cluster --type all --level info --json
  ```
  {: pre}



### ibmcloud ks logging-filter-get
{: #cs_log_filter_view}

로깅 필터 구성을 봅니다. 이 명령을 사용하여 작성한 로깅 필터를 볼 수 있습니다.
{: shortdesc}

```
ibmcloud ks logging-filter-get --cluster CLUSTER [--id FILTER_ID] [--show-matching-configs] [--show-covering-filters] [--json] [-s]
```
{: pre}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **뷰어** 플랫폼 역할

<strong>명령 옵션</strong>:

<dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>필터를 확인할 클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

  <dt><code>--id <em>FILTER_ID</em></code></dt>
    <dd>보려는 로그 필터의 ID입니다.</dd>

  <dt><code>--show-matching-configs</code></dt>
    <dd>보고 있는 구성과 일치하는 로깅 구성을 표시합니다.  이 값은 선택사항입니다.</dd>

  <dt><code>--show-covering-filters</code></dt>
    <dd>더 이상 사용되지 않는 이전 필터를 렌더링하는 로깅 필터를 표시합니다.  이 값은 선택사항입니다.</dd>

  <dt><code>--json</code></dt>
    <dd>명령 출력을 JSON 형식으로 인쇄합니다.  이 값은 선택사항입니다.</dd>

   <dt><code>-s</code></dt>
     <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>
</dl>

**예제**:

```
ibmcloud ks logging-filter-get mycluster --id 885732 --show-matching-configs
```
{: pre}

### ibmcloud ks logging-filter-rm
{: #cs_log_filter_delete}

로깅 필터를 삭제합니다. 이 명령을 사용하여 작성한 로깅 필터를 제거할 수 있습니다.
{: shortdesc}

```
ibmcloud ks logging-filter-rm --cluster CLUSTER [--id FILTER_ID] [--all] [--force-update] [-s]
```
{: pre}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **편집자** 플랫폼 역할

<strong>명령 옵션</strong>:

<dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>필터를 삭제할 클러스터의 이름 또는 ID입니다.</dd>

  <dt><code>--id <em>FILTER_ID</em></code></dt>
    <dd>삭제할 로그 필터의 ID입니다.</dd>

  <dt><code>--all</code></dt>
    <dd>모든 로그 전달 필터를 삭제합니다.  이 값은 선택사항입니다.</dd>

  <dt><code>--force-update</code></dt>
    <dd>최신 버전으로 Fluentd 팟(Pod)의 업데이트를 강제 실행합니다. 로깅 구성을 변경하려면 Fluentd가 최신 버전이어야 합니다.</dd>

  <dt><code>-s</code></dt>
    <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>
</dl>

**예제**:

```
ibmcloud ks logging-filter-rm mycluster --id 885732
```
{: pre}

### ibmcloud ks logging-filter-update
{: #cs_log_filter_update}

로깅 필터를 업데이트합니다. 이 명령을 사용하여 작성한 로깅 필터를 업데이트할 수 있습니다.
{: shortdesc}

```
ibmcloud ks logging-filter-update --cluster CLUSTER --id FILTER_ID --type LOG_TYPE [--logging-configs CONFIGS] [--namespace KUBERNETES_NAMESPACE] [--container CONTAINER_NAME] [--level LOGGING_LEVEL] [--message MESSAGE] [--regex-message MESSAGE] [--force-update] [--json] [-s]
```
{: pre}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **편집자** 플랫폼 역할

<strong>명령 옵션</strong>:

<dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>로깅 필터를 업데이트하려는 클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

 <dt><code>--id <em>FILTER_ID</em></code></dt>
    <dd>업데이트하려는 로그 필터의 ID입니다.</dd>

  <dt><code>--type <em>LOG_TYPE</em></code></dt>
    <dd>필터를 적용할 로그의 유형입니다. 현재는 <code>all</code>, <code>container</code> 및 <code>host</code>가 지원됩니다.</dd>

  <dt><code>--logging-configs <em>CONFIGS</em></code></dt>
    <dd>로깅 구성 ID의 쉼표로 구분된 목록입니다. 제공되지 않으면 필터에 전달된 모든 클러스터 로깅 구성에 필터가 적용됩니다. 명령과 함께 <code>--show-matching-configs</code> 플래그를 사용하여 필터와 일치하는 로그 구성을 볼 수 있습니다.  이 값은 선택사항입니다.</dd>

  <dt><code>--namespace <em>KUBERNETES_NAMESPACE</em></code></dt>
    <dd>로그를 필터링할 Kubernetes 네임스페이스입니다.  이 값은 선택사항입니다.</dd>

  <dt><code>--container <em>CONTAINER_NAME</em></code></dt>
    <dd>로그를 필터링할 컨테이너의 이름입니다. 이 플래그는 로그 유형 <code>container</code>를 사용하는 경우에만 적용됩니다.  이 값은 선택사항입니다.</dd>

  <dt><code>--level <em>LOGGING_LEVEL</em></code></dt>
    <dd>지정된 레벨 이하의 로그를 필터링합니다. 허용 가능한 값은 규범적 순서대로 <code>fatal</code>, <code>error</code>, <code>warn/warning</code>, <code>info</code>, <code>debug</code> 및 <code>trace</code>입니다. 이 값은 선택사항입니다. 예를 들어, <code>info</code> 레벨에서 로그를 필터링한 경우에는 <code>debug</code> 및 <code>trace</code> 또한 필터링됩니다. **참고**: 로그 메시지가 JSON 형식이며 level 필드를 포함하는 경우에만 이 플래그를 사용할 수 있습니다. 출력 예: <code>{"log": "hello", "level": "info"}</code></dd>

  <dt><code>--message <em>MESSAGE</em></code></dt>
    <dd>로그의 임의의 위치에 지정된 메시지가 포함되어 있는 로그를 필터링합니다. 이 값은 선택사항입니다. 예: 메시지 "Hello", "!" 및 "Hello, World!"는 로그 "Hello, World!"에 적용됩니다.</dd>

  <dt><code>--regex-message <em>MESSAGE</em></code></dt>
    <dd>로그의 임의의 위치에 정규식으로 작성된 지정된 메시지가 포함되어 있는 로그를 필터링합니다. 이 값은 선택사항입니다. 예: 패턴 "hello [0-9]"는 "hello 1", "hello 2" 및 "hello 9"에 적용됩니다.</dd>

  <dt><code>--force-update</code></dt>
    <dd>최신 버전으로 Fluentd 팟(Pod)의 업데이트를 강제 실행합니다. 로깅 구성을 변경하려면 Fluentd가 최신 버전이어야 합니다.</dd>

  <dt><code>--json</code></dt>
    <dd>명령 출력을 JSON 형식으로 인쇄합니다.  이 값은 선택사항입니다.</dd>

  <dt><code>-s</code></dt>
    <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>
</dl>

**예제**:

이 예는 기본 네임스페이스에 있는 `test-container`라는 이름의 컨테이너에서 전달되는, debug 레벨 이하이며 "GET request"를 포함하는 로그 메시지가 있는 모든 로그를 필터링합니다.

  ```
  ibmcloud ks logging-filter-update --cluster example-cluster --id 885274 --type container --namespace default --container test-container --level debug --message "GET request"
  ```
  {: pre}

이 예는 특정 클러스터에서 전달되는, `info` 레벨 이하의 모든 로그를 필터링합니다. 출력은 JSON으로 리턴됩니다.

  ```
  ibmcloud ks logging-filter-update --cluster example-cluster --id 274885 --type all --level info --json
  ```
  {: pre}

### ibmcloud ks logging-collect
{: #cs_log_collect}

특정 시점의 로그에 대한 스냅샷을 요청하고 로그를 {{site.data.keyword.cos_full_notm}} 버킷에 저장합니다.
{: shortdesc}

```
ibmcloud ks logging-collect --cluster CLUSTER --cos-bucket BUCKET_NAME --cos-endpoint ENDPOINT --hmac-key-id HMAC_KEY_ID --hmac-key HMAC_KEY --type LOG_TYPE [-s]
```
{: pre}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **관리자** 플랫폼 역할

<strong>명령 옵션</strong>:

<dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>스냅샷을 작성할 클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

 <dt><code>--cos-bucket <em>BUCKET_NAME</em></code></dt>
    <dd>로그를 저장할 {{site.data.keyword.cos_short}} 버킷의 이름입니다. 이 값은 필수입니다.</dd>

  <dt><code>--cos-endpoint <em>ENDPOINT</em></code></dt>
    <dd>로그를 저장하는 버킷의 {{site.data.keyword.cos_short}} 엔드포인트입니다. 이 값은 필수입니다.</dd>

  <dt><code>--hmac-key-id <em>HMAC_KEY_ID</em></code></dt>
    <dd>Object Storage 인스턴스에 대한 HMAC 인증 정보의 고유 ID입니다. 이 값은 필수입니다.</dd>

  <dt><code>--hmac-key <em>HMAC_KEY</em></code></dt>
    <dd>{{site.data.keyword.cos_short}} 인스턴스에 대한 HMAC 키입니다. 이 값은 필수입니다.</dd>

  <dt><code>--type <em>LOG_TYPE</em></code></dt>
    <dd>스냅샷을 작성할 로그의 유형입니다. 현재는 `master`가 유일한 선택사항이며 기본값입니다.</dd>

  <dt><code>-s</code></dt>
    <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>
</dl>

**예제**:

  ```
  ibmcloud ks logging-collect --cluster mycluster --cos-bucket mybucket --cos-endpoint s3-api.us-geo.objectstorage.softlayer.net --hmac-key-id e2e7f5c9fo0144563c418dlhi3545m86 --hmac-key c485b9b9fo4376722f692b63743e65e1705301ab051em96j
  ```
  {: pre}

**출력 예**:

  ```
  There is no specified log type. The default master will be used.
  Submitting log collection request for master logs for cluster mycluster...
  OK
  The log collection request was successfully submitted. To view the status of the request run ibmcloud ks logging-collect-status mycluster.
  ```
  {: screen}


### ibmcloud ks logging-collect-status
{: #cs_log_collect_status}

클러스터에 대한 로그 콜렉션 스냅샷 요청의 상태입니다.
{: shortdesc}

```
ibmcloud ks logging-collect-status --cluster CLUSTER [--json] [-s]
```
{: pre}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **관리자** 플랫폼 역할

<strong>명령 옵션</strong>:

<dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>스냅샷을 작성할 클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

  <dt><code>--json</code></dt>
    <dd>명령 출력을 JSON 형식으로 인쇄합니다.  이 값은 선택사항입니다.</dd>

  <dt><code>-s</code></dt>
    <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>
</dl>

**예제**:

  ```
  ibmcloud ks logging-collect-status --cluster mycluster
  ```
  {: pre}

**출력 예**:

  ```
  Getting the status of the last log collection request for cluster mycluster...
  OK
  State     Start Time             Error   Log URLs
  success   2018-09-18 16:49 PDT   - s3-api.us-geo.objectstorage.softlayer.net/mybucket/master-0-0862ae70a9ae6c19845ba3pc0a2a6o56-1297318756.tgz
  s3-api.us-geo.objectstorage.softlayer.net/mybucket/master-1-0862ae70a9ae6c19845ba3pc0a2a6o56-1297318756.tgz
  s3-api.us-geo.objectstorage.softlayer.net/mybucket/master-2-0862ae70a9ae6c19845ba3pc0a2a6o56-1297318756.tgz
  ```
  {: screen}

<br />


## 네트워크 로드 밸런서 명령(`nlb-dns`)
{: #nlb-dns}

네트워크 로드 밸런서(NLB) IP 주소에 대한 호스트 이름 및  NLB 호스트 이름에 대한 상태 검사 모니터를 작성하고 관리하기 위해 이 명령의 그룹을 사용합니다. 자세한 정보는 [로드 밸런서 호스트 이름 등록](/docs/containers?topic=containers-loadbalancer#loadbalancer_hostname)을 참조하십시오.
{: shortdesc}

### ibmcloud ks nlb-dns-add
{: #cs_nlb-dns-add}

네트워크 로드 밸런서(NLB) IP를 [`ibmcloud ks nlb-dns-create` 명령](#cs_nlb-dns-create)을 사용하여 작성한 기존 호스트 이름에 추가합니다.
{: shortdesc}

```
ibmcloud ks nlb-dns-add --cluster CLUSTER --ip IP --nlb-host HOST_NAME [--json] [-s]
```
{: pre}

예를 들어, 다중 구역 클러스터에서 앱을 노출할 각 구역에서 NLB를 작성할 수 있습니다. `ibmcloud ks nlb-dns-create` 명령을 실행하여 호스트 이름으로 하나의 구역에서 NLB IP를 등록합니다. 그러면 이제 다른 구역의 NLB IP를 이 기존 호스트 이름에 추가할 수 있습니다. 사용자가 앱 호스트 이름에 액세스하면 클라이언트는 무작위로 이러한 IP 중 하나에 액세스하고 요청이 해당 NLB로 전송됩니다. 추가할 각 IP 주소에 대해 다음 명령을 실행해야 합니다. 

**최소 필요 권한**: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **편집자** 플랫폼 역할

**명령 옵션**:

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

<dt><code>--ip <em>IP</em></code></dt>
<dd>호스트 이름에 추가할 NLB IP입니다. NLB IP를 보려면 <code>kubectl get svc</code>를 실행하십시오.</dd>

<dt><code>--nlb-host <em>HOST_NAME</em></code></dt>
<dd>IP를 추가할 호스트 이름입니다. 기존 호스트 이름을 보려면 <code>ibmcloud ks nlb-dnss</code>를 실행하십시오.</dd>

<dt><code>--json</code></dt>
<dd>명령 출력을 JSON 형식으로 인쇄합니다.  이 값은 선택사항입니다.</dd>

<dt><code>-s</code></dt>
<dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>
</dl>

**예제**:
```
ibmcloud ks nlb-dns-add --cluster mycluster --ip 1.1.1.1 --nlb-host mycluster-a1b2cdef345678g9hi012j3kl4567890-0001.us-south.containers.appdomain.cloud
```
{: pre}

### ibmcloud ks nlb-dns-create
{: #cs_nlb-dns-create}

네트워크 로드 밸런서(NLB) IP를 등록하기 위해 DNS 호스트 이름을 작성하여 앱을 공용으로 노출합니다.
{: shortdesc}

```
ibmcloud ks nlb-dns-create --cluster CLUSTER --ip IP [--json] [-s]
```
{: pre}

**최소 필요 권한**: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **편집자** 플랫폼 역할

**명령 옵션**:

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

<dt><code>--ip <em>IP</em></code></dt>
<dd>등록할 네트워크 로드 밸런서 IP 주소입니다. NLB IP를 보려면 <code>kubectl get svc</code>를 실행하십시오. 초기에는 하나의 IP 주소로만 호스트 이름을 작성할 수 있습니다. 하나의 앱을 노출하는 다중 구역 클러스터의 각 구역에 NLB가 있는 경우 [`ibmcloud ks nlb-dns-add` 명령](#cs_nlb-dns-add)을 실행하여 기타 NLB의 IP를 호스트 이름에 추가할 수 있습니다. </dd>

<dt><code>--json</code></dt>
<dd>명령 출력을 JSON 형식으로 인쇄합니다.  이 값은 선택사항입니다.</dd>

<dt><code>-s</code></dt>
<dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>
</dl>

**예제**:
```
ibmcloud ks nlb-dns-create --cluster mycluster --ip 1.1.1.1
```
{: pre}

### ibmcloud ks nlb-dnss
{: #cs_nlb-dns-ls}

클러스터에 등록된 네트워크 로드 밸런서 호스트 이름 및 IP 주소를 나열합니다.
{: shortdesc}

```
ibmcloud ks nlb-dnss --cluster CLUSTER [--json] [-s]
```
{: pre}

**최소 필요 권한**: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **편집자** 플랫폼 역할

**명령 옵션**:

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

<dt><code>--json</code></dt>
<dd>명령 출력을 JSON 형식으로 인쇄합니다.  이 값은 선택사항입니다.</dd>

<dt><code>-s</code></dt>
<dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>
</dl>

**예제**:
```
ibmcloud ks nlb-dnss --cluster mycluster
```
{: pre}

### ibmcloud ks nlb-dns-rm
{: #cs_nlb-dns-rm}

호스트 이름에서 네트워크 로드 밸런서 IP 주소를 제거합니다. 호스트 이름에서 모든 IP를 제거하는 경우 호스트 이름은 계속해서 존재하지만 IP가 이와 연관되지 않습니다. 제거할 각 IP 주소에 대해 이 명령을 실행해야 합니다.
{: shortdesc}

```
ibmcloud ks nlb-dns-rm --cluster CLUSTER --ip IP --nlb-host HOST_NAME [--json] [-s]
```
{: pre}

**최소 필요 권한**: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **편집자** 플랫폼 역할

**명령 옵션**:

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

<dt><code>--ip <em>IP</em></code></dt>
<dd>제거할 NLB IP입니다. NLB IP를 보려면 <code>kubectl get svc</code>를 실행하십시오.</dd>

<dt><code>--nlb-host <em>HOST_NAME</em></code></dt>
<dd>IP를 제거할 호스트 이름입니다. 기존 호스트 이름을 보려면 <code>ibmcloud ks nlb-dnss</code>를 실행하십시오.</dd>

<dt><code>--json</code></dt>
<dd>명령 출력을 JSON 형식으로 인쇄합니다.  이 값은 선택사항입니다.</dd>

<dt><code>-s</code></dt>
<dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>
</dl>

**예제**:
```
ibmcloud ks nlb-dns-rm --cluster mycluster --ip 1.1.1.1 --nlb-host mycluster-a1b2cdef345678g9hi012j3kl4567890-0001.us-south.containers.appdomain.cloud
```
{: pre}

### ibmcloud ks nlb-dns-monitor
{: #cs_nlb-dns-monitor}

클러스터의 네트워크 로드 밸런서 호스트 이름에 대한 상태 검사 모니터를 작성하고, 수정하고, 볼 수 있습니다. 이 명령은 다음 하위 명령 중 하나와 결합되어야 합니다.
{: shortdesc}

#### ibmcloud ks nlb-dns-monitor-configure
{: #cs_nlb-dns-monitor-configure}

클러스터의 기존 NLB 호스트 이름에 대한 상태 검사 모니터를 구성하고 선택적으로 사용으로 설정합니다. 호스트 이름에 대한 모니터를 사용으로 설정하면, 모니터 상태는 각 지역에서 NLB IP를 검사하고 이러한 상태 검사를 기반으로 DNS 검색 결과가 지속적으로 업데이트되도록 합니다.
{: shortdesc}

```
ibmcloud ks nlb-dns-monitor-configure --cluster CLUSTER --nlb-host HOST NAME [--enable] [--desc DESCRIPTION] [--type TYPE] [--method METHOD] [--path PATH] [--timeout TIMEOUT] [--retries RETRIES] [--interval INTERVAL] [--port PORT] [--header HEADER] [--expected-body BODY STRING] [--expected-codes HTTP CODES] [--follows-redirects TRUE] [--allows-insecure TRUE] [--json] [-s]
```
{: pre}

이 명령을 사용하여 새 상태 검사 모니터를 작성하고 사용으로 설정할 수 있거나 기존 상태 검사 모니터에 대한 설정을 업데이트할 수 있습니다. 새 모니터를 작성하려면 `--enable` 플래그 및 구성할 모든 설정에 대한 플래그를 포함하십시오. 기존 모니터를 업데이트하려면 변경할 설정에 대한 플래그만 포함하십시오. 

**최소 필요 권한**: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **편집자** 플랫폼 역할

**명령 옵션**:

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>호스트 이름이 등록된 클러스터의 이름 또는 ID입니다.</dd>

<dt><code>--nlb-host <em>HOST NAME</em></code></dt>
<dd>상태 검사 모니터를 구성할 호스트 이름입니다. 호스트 이름을 나열하려면 <code>ibmcloud ks nlb-dns list --cluster CLUSTER</code>를 실행하십시오.</dd>

<dt><code>--enable</code></dt>
<dd>이 플래그를 포함하여 호스트에 대한 새 상태 검사 모니터를 작성하고 사용으로 설정합니다. </dd>

<dt><code>--description <em>DESCRIPTION</em></code></dt>
<dd>상태 모니터에 대한 설명입니다.</dd>

<dt><code>--type <em>TYPE</em></code></dt>
<dd>상태 검사에 사용할 프로토콜: <code>HTTP</code>, <code>HTTPS</code> 또는 <code>TCP</code>. 기본값: <code>HTTP</code></dd>

<dt><code>--method <em>METHOD</em></code></dt>
<dd>상태 검사에 사용할 메소드입니다. <code>type</code> <code>HTTP</code> 및 <code>HTTPS</code>의 기본값: <code>GET</code>. <code>type</code> <code>TCP</code>의 기본값: <code>connection_established</code>.</dd>

<dt><code>--path <em>PATH</em></code></dt>
<dd><code>type</code>이 <code>HTTPS</code>인 경우: 상태 검사에 대한 엔드포인트 경로입니다. 기본값: <code>/</code></dd>

<dt><code>--timeout <em>TIMEOUT</em></code></dt>
<dd>IP가 비정상 상태로 간주되기 전의 시간(초)입니다. 기본값: <code>5</code></dd>

<dt><code>--retries <em>RETRIES</em></code></dt>
<dd>제한시간 초과 시 IP가 비정상 상태로 간주되기 전에 시도할 재시도 횟수입니다. 재시도가 즉시 시도됩니다. 기본값: <code>2</code></dd>

<dt><code>--interval <em>INTERVAL</em></code></dt>
<dd>각 상태 검사 간의 간격(초)입니다. 짧은 간격은 장애 복구 시간이 개선되지만 IP에 대한 로드가 증가될 수 있습니다. 기본값: <code>60</code></dd>

<dt><code>--port <em>PORT</em></code></dt>
<dd>상태 검사에 연결할 포트 번호입니다. <code>type</code>이 <code>TCP</code>인 경우 이 매개변수는 필수입니다. <code>type</code>이 <code>HTTP</code> 또는 <code>HTTPS</code>인 경우 80(HTTP) 또는 443(HTTPS) 이외의 포트를 사용하는 경우에만 포트를 정의하십시오. TCP의 기본값은 <code>0</code>이고 HTTP의 기본값은 <code>80</code>입니다. HTTPS의 기본값은 <code>443</code>입니다.</dd>

<dt><code>--header <em>HEADER</em></code></dt>
<dd><code>type</code>이 <code>HTTPS</code> 또는 <code>HTTPS</code>인 경우: 호스트 헤더와 같이 상태 검사에서 전송할 HTTP 요청 헤더입니다. 사용자 에이전트 헤더를 대체할 수 없습니다. </dd>

<dt><code>--expected-body <em>BODY STRING</em></code></dt>
<dd><code>type</code>이 <code>HTTP</code> 또는 <code>HTTPS</code>인 경우: 상태 검사가 응답 본문에서 찾는 하위 문자열입니다(대소문자 구분). 이 문자열을 찾을 수 없으면 IP가 비정상 상태로 간주됩니다. </dd>

<dt><code>--expected-codes <em>HTTP CODES</em></code></dt>
<dd><code>type</code>이 <code>HTTP</code> 또는 <code>HTTPS</code>인 경우: 상태 검사가 응답 본문에서 찾는 HTTP 코드입니다. HTTP 코드를 찾을 수 없으면 IP가 비정상 상태로 간주됩니다. 기본값: <code>2xx</code></dd>

<dt><code>--allows-insecure <em>TRUE</em></code></dt>
<dd><code>type</code>이 <code>HTTP</code> 또는 <code>HTTPS</code>인 경우: 인증서를 유효성 검증하지 않으려면 <code>true</code>를 설정하십시오. </dd>

<dt><code>--follows-redirects <em>TRUE</em></code></dt>
<dd><code>type</code>이 <code>HTTP</code> 또는 <code>HTTPS</code>인 경우: IP로 리턴되는 경로 재지정을 따르려면 <code>true</code>를 설정하십시오. </dd>

<dt><code>--json</code></dt>
<dd>명령 출력을 JSON 형식으로 인쇄합니다.  이 값은 선택사항입니다.</dd>

<dt><code>-s</code></dt>
<dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>
</dl>

**예제**:
```
ibmcloud ks nlb-dns-monitor-configure --cluster mycluster --nlb-host mycluster-a1b2cdef345678g9hi012j3kl4567890-0001.us-south.containers.appdomain.cloud --enable --desc "Login page monitor" --type HTTPS --method GET --path / --timeout 5 --retries 2 --interval 60  --expected-body "healthy" --expected-codes 2xx --follows-redirects true
```
{: pre}

#### ibmcloud ks nlb-dns-monitor-get
{: #cs_nlb-dns-monitor-get}

기존 상태 검사 모니터에 대한 설정을 봅니다.
{: shortdesc}

```
ibmcloud ks nlb-dns-monitor-get --cluster CLUSTER --nlb-host HOST_NAME [--json] [-s]
```
{: pre}

**최소 필요 권한**: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **편집자** 플랫폼 역할

**명령 옵션**:

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

<dt><code>--nlb-host <em>HOST_NAME</em></code></dt>
<dd>상태 검사를 모니터하는 호스트 이름입니다. 호스트 이름을 나열하려면 <code>ibmcloud ks nlb-dns list --cluster CLUSTER</code>를 실행하십시오.</dd>

<dt><code>--json</code></dt>
<dd>명령 출력을 JSON 형식으로 인쇄합니다.  이 값은 선택사항입니다.</dd>

<dt><code>-s</code></dt>
<dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>
</dl>

**예제**:
```
ibmcloud ks nlb-dns-monitor-get --cluster mycluster --nlb-host mycluster-a1b2cdef345678g9hi012j3kl4567890-0001.us-south.containers.appdomain.cloud
```
{: pre}

#### ibmcloud ks nlb-dns-monitor-disable
{: #cs_nlb-dns-monitor-disable}

클러스터의 호스트 이름에 대한 기존 상태 검사 모니터를 사용 안함으로 설정합니다.
{: shortdesc}

```
ibmcloud ks nlb-dns-monitor-disable --cluster CLUSTER --nlb-host HOST_NAME [--json] [-s]
```
{: pre}

**최소 필요 권한**: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **편집자** 플랫폼 역할

**명령 옵션**:

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

<dt><code>--nlb-host <em>HOST_NAME</em></code></dt>
<dd>상태 검사를 모니터하는 호스트 이름입니다. 호스트 이름을 나열하려면 <code>ibmcloud ks nlb-dns list --cluster CLUSTER</code>를 실행하십시오.</dd>

<dt><code>--json</code></dt>
<dd>명령 출력을 JSON 형식으로 인쇄합니다.  이 값은 선택사항입니다.</dd>

<dt><code>-s</code></dt>
<dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>
</dl>

**예제**:
```
ibmcloud ks nlb-dns-monitor-disable --cluster mycluster --nlb-host mycluster-a1b2cdef345678g9hi012j3kl4567890-0001.us-south.containers.appdomain.cloud
```
{: pre}

#### ibmcloud ks nlb-dns-monitor-enable
{: #cs_nlb-dns-monitor-enable}

구성한 상태 검사 모니터를 사용으로 설정합니다.
{: shortdesc}

```
ibmcloud ks nlb-dns-monitor-enable --cluster CLUSTER --nlb-host HOST_NAME [--json] [-s]
```
{: pre}

상태 검사 모니터를 처음 작성하는 경우 `ibmcloud ks nlb-dns-monitor-configure` 명령을 사용하여 상태 검사 모니터를 구성하고 사용으로 설정해야 합니다. 아직 사용으로 설정하지 않았으나 구성한 모니터만 사용으로 설정하거나 이전에 사용 안함으로 설정한 모니터를 다시 사용으로 설정하려면 `ibmcloud ks nlb-dns-monitor-enable` 명령을 사용하십시오. 

**최소 필요 권한**: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **편집자** 플랫폼 역할

**명령 옵션**:

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

<dt><code>--nlb-host <em>HOST_NAME</em></code></dt>
<dd>상태 검사를 모니터하는 호스트 이름입니다. 호스트 이름을 나열하려면 <code>ibmcloud ks nlb-dns list --cluster CLUSTER</code>를 실행하십시오.</dd>

<dt><code>--json</code></dt>
<dd>명령 출력을 JSON 형식으로 인쇄합니다.  이 값은 선택사항입니다.</dd>

<dt><code>-s</code></dt>
<dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>
</dl>

**예제**:
```
ibmcloud ks nlb-dns-monitor-enable --cluster mycluster --nlb-host mycluster-a1b2cdef345678g9hi012j3kl4567890-0001.us-south.containers.appdomain.cloud
```
{: pre}

#### ibmcloud ks nlb-dns-monitor-ls
{: #cs_nlb-dns-monitor-ls}

클러스터의 각 NLB 호스트 이름에 대한 상태 검사 모니터 설정을 나열합니다.
{: shortdesc}

```
ibmcloud ks nlb-dns-monitor-ls --cluster CLUSTER [--json] [-s]
```
{: pre}

**최소 필요 권한**: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **편집자** 플랫폼 역할

**명령 옵션**:

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

<dt><code>--json</code></dt>
<dd>명령 출력을 JSON 형식으로 인쇄합니다.  이 값은 선택사항입니다.</dd>

<dt><code>-s</code></dt>
<dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>
</dl>

**예제**:
```
ibmcloud ks nlb-dns-monitor-ls --cluster mycluster
```
{: pre}

#### ibmcloud ks nlb-dns-monitor-status
{: #cs_nlb-dns-monitor-status}

클러스터의 NLB 호스트 이름 뒤에 있는 IP에 대한 상태 검사 상태를 나열합니다.
{: shortdesc}

```
ibmcloud ks nlb-dns-monitor-status --cluster CLUSTER [--json] [-s]
```
{: pre}

**최소 필요 권한**: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **편집자** 플랫폼 역할

**명령 옵션**:

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

<dt><code>-- <em></em></code></dt>
<dd>하나의 호스트 이름에 대해서만 상태를 보려면 이 플래그를 포함하십시오. 호스트 이름을 나열하려면 <code>ibmcloud ks nlb-dns list --cluster CLUSTER</code>를 실행하십시오.</dd>

<dt><code>--json</code></dt>
<dd>명령 출력을 JSON 형식으로 인쇄합니다.  이 값은 선택사항입니다.</dd>

<dt><code>-s</code></dt>
<dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>
</dl>

**예제**:
```
ibmcloud ks nlb-dns-monitor-status --cluster mycluster
```
{: pre}

<br />


## 지역 명령
{: #region_commands}

이 명령의 그룹을 사용하여 사용 가능한 위치를 보고, 현재 대상으로 지정된 지역을 보고, 대상으로 지정된 지역을 설정합니다.
{: shortdesc}

### ibmcloud ks region
{: #cs_region}

현재 대상으로 지정된 {{site.data.keyword.containerlong_notm}} 지역을 찾으십시오. 사용자는 영역에 특정한 클러스터를 작성하고 관리합니다. `ibmcloud ks region-set` 명령을 사용하여 지역을 변경할 수 있습니다.
{: shortdesc}

```
ibmcloud ks region
```
{: pre}

<strong>최소 필요 권한</strong>: 없음

###         ibmcloud ks region-set
{: #cs_region-set}

{{site.data.keyword.containerlong_notm}}의 지역을 설정합니다. 사용자는 지역에 특정한 클러스터를 작성 및 관리하며, 고가용성을 위해 여러 지역에서 클러스터를 원할 수 있습니다.
{: shortdesc}

예를 들어, 미국 남부 지역에서 {{site.data.keyword.Bluemix_notm}}에 로그인하고 클러스터를 작성할 수 있습니다. 그 다음에는 `ibmcloud ks region-set eu-central`을 사용하여 EU 중앙 지역을 대상으로 지정하고 다른 클러스터를 작성할 수 있습니다. 최종적으로는 `ibmcloud ks region-set us-south`를 사용하여 미국 남부로 돌아가서 해당 지역의 클러스터를 관리할 수 있습니다.

<strong>최소 필요 권한</strong>: 없음

```
ibmcloud ks region-set [--region REGION]
```
{: pre}

**명령 옵션**:

<dl>
<dt><code>--region <em>REGION</em></code></dt>
<dd>대상으로 할 지역을 입력하십시오. 이 값은 선택사항입니다. 지역을 제공하지 않은 경우 출력의 목록에서 선택할 수 있습니다.

사용 가능한 지역의 목록을 보려면 [지역 및 구역](/docs/containers?topic=containers-regions-and-zones)을 검토하거나 `ibmcloud ks regions` [명령](#cs_regions)을 사용하십시오.</dd></dl>

**예제**:

```
ibmcloud ks region-set eu-central
```
{: pre}

```
ibmcloud ks region-set
```
{: pre}

**출력**:
```
Choose a region:
1. ap-north
2. ap-south
3. eu-central
4. uk-south
5. us-east
6. us-south
Enter a number> 3
OK
```
{: screen}

### ibmcloud ks regions
{: #cs_regions}

사용 가능한 지역을 나열합니다. `Region Name`은 {{site.data.keyword.containerlong_notm}} 이름이고 `Region Alias`는 해당 지역의 일반 {{site.data.keyword.Bluemix_notm}} 이름입니다.
{: shortdesc}

<strong>최소 필요 권한</strong>: 없음

**예제**:

```
ibmcloud ks regions
```
{: pre}

**출력**:
```
Region Name   Region Alias
ap-north      jp-tok
ap-south      au-syd
eu-central    eu-de
uk-south      eu-gb
us-east       us-east
us-south      us-south
```
{: screen}

###         ibmcloud ks zones
{: #cs_datacenters}

사용자가 클러스터를 작성하기 위해 사용할 수 있는 구역의 목록을 봅니다. 사용 가능한 구역은 사용자가 로그인한 지역에 따라 다릅니다. 지역을 전환하려면 `ibmcloud ks region-set`를 실행하십시오.
{: shortdesc}

```
ibmcloud ks zones [--region-only] [--json] [-s]
```
{: pre}

<strong>명령 옵션</strong>:

   <dl><dt><code>--region-only</code></dt>
   <dd>사용자가 로그인한 지역 내의 다중 구역만 나열합니다.  이 값은 선택사항입니다.</dd>

   <dt><code>--json</code></dt>
   <dd>명령 출력을 JSON 형식으로 인쇄합니다.  이 값은 선택사항입니다.</dd>

   <dt><code>-s</code></dt>
   <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>
   </dl>

**최소 권한**: 없음

<br />



## 작업자 노드 명령
{: worker_node_commands}


### 더 이상 사용되지 않음: ibmcloud ks worker-add
{: #cs_worker_add}

독립형 작업자 노드를 작업자 풀에 없는 표준 클러스터에 추가합니다.
{: deprecated}

```
ibmcloud ks worker-add --cluster CLUSTER [--file FILE_LOCATION] [--hardware HARDWARE] --machine-type MACHINE_TYPE --workers NUMBER --private-vlan PRIVATE_VLAN --public-vlan PUBLIC_VLAN [--disable-disk-encrypt] [-s]
```
{: pre}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **운영자** 플랫폼 역할

<strong>명령 옵션</strong>:

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

<dt><code>--file <em>FILE_LOCATION</em></code></dt>
<dd>클러스터에 작업자 노드를 추가하기 위한 YAML 파일의 경로입니다. 이 명령에 제공된 옵션을 사용하여 추가 작업자 노드를 정의하는 대신 YAML 파일을 사용할 수 있습니다.  이 값은 선택사항입니다.

<p class="note">YAML 파일의 매개변수와 동일한 옵션을 명령에서 제공하면 명령의 값이 YAML의 값에 우선합니다. 예를 들어, YAML 파일에 머신 유형을 정의하고 명령에서 --machine-type 옵션을 사용하십시오. 명령 옵션에 입력한 값이 YAML 파일의 값을 대체합니다.</p>

<pre class="codeblock">
<code>name: <em>&lt;cluster_name_or_ID&gt;</em>
zone: <em>&lt;zone&gt;</em>
machine-type: <em>&lt;machine_type&gt;</em>
private-vlan: <em>&lt;private_VLAN&gt;</em>
public-vlan: <em>&lt;public_VLAN&gt;</em>
hardware: <em>&lt;shared_or_dedicated&gt;</em>
workerNum: <em>&lt;number_workers&gt;</em>
diskEncryption: <em>false</em></code></pre>

<table>
<caption>YAML 파일 컴포넌트 이해</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> YAML 파일 컴포넌트 이해</th>
</thead>
<tbody>
<tr>
<td><code><em>name</em></code></td>
<td><code><em>&lt;cluster_name_or_ID&gt;</em></code>를 작업자 노드를 추가할 클러스터의 이름 또는 ID로 대체합니다.</td>
</tr>
<tr>
<td><code><em>zone</em></code></td>
<td><code><em>&lt;zone&gt;</em></code>을 작업자 노드를 배치할 구역으로 대체합니다. 사용 가능한 구역은 사용자가 로그인한 지역에 따라 다릅니다. 사용 가능한 구역을 나열하려면 <code>ibmcloud ks zones</code>를 실행하십시오.</td>
</tr>
<tr>
<td><code><em>machine-type</em></code></td>
<td><code><em>&lt;machine_type&gt;</em></code>을 작업자 노드를 배치하려는 머신 유형으로 대체합니다. 공유 또는 전용 하드웨어에서 가상 머신으로서 또는 베어메탈에서 실제 머신으로서 작업자 노드를 배치할 수 있습니다. 사용 가능한 실제 및 가상 머신 유형은 클러스터가 배치되는 구역에 따라 다양합니다. 자세한 정보는 `ibmcloud ks machine-types` [명령](/docs/containers?topic=containers-cs_cli_reference#cs_machine_types)을 참조하십시오.</td>
</tr>
<tr>
<td><code><em>private-vlan</em></code></td>
<td><code><em>&lt;private_VLAN&gt;</em></code>을 작업자 노드에 사용할 사설 VLAN의 ID로 대체합니다. 사용 가능한 VLAN을 나열하려면 <code>ibmcloud ks vlans <em>&lt;zone&gt;</em></code>을 실행하고 <code>bcr</code>(백엔드 라우터)로 시작하는 VLAN 라우터를 찾으십시오.</td>
</tr>
<tr>
<td><code>public-vlan</code></td>
<td><code>&lt;public_VLAN&gt;</code>을 작업자 노드에 사용할 공용 VLAN의 ID로 대체합니다. 사용 가능한 VLAN을 나열하려면 <code>ibmcloud ks vlans &lt;zone&gt;</code>을 실행하고 <code>fcr</code>(프론트 엔드 라우터)로 시작하는 VLAN 라우터를 찾으십시오. <br><strong>참고</strong>: 작업자 노드가 사설 VLAN 전용으로 설정된 경우에는 [개인 서비스 엔드포인트를 사용으로 설정](/docs/containers?topic=containers-cs_network_ov#cs_network_ov_master_private)하거나 [게이트웨이 디바이스를 구성](/docs/containers?topic=containers-cs_network_ov#cs_network_ov_master_gateway)하여 작업자 노드와 클러스터 마스터가 통신할 수 있도록 해야 합니다.</td>
</tr>
<tr>
<td><code>hardware</code></td>
<td>가상 머신 유형의 경우: 작업자 노드에 대한 하드웨어 격리의 레벨입니다. 사용자 전용으로만 실제 리소스를 사용 가능하게 하려면 dedicated를 사용하고, 실제 리소스를 다른 IBM 고객과 공유하도록 허용하려면 shared를 사용하십시오. 기본값은 shared입니다.</td>
</tr>
<tr>
<td><code>workerNum</code></td>
<td><code><em>&lt;number_workers&gt;</em></code>를 배치할 작업자 노드 수로 대체합니다.</td>
</tr>
<tr>
<td><code>diskEncryption: <em>false</em></code></td>
<td>작업자 노드는 기본적으로 AES 256비트 디스크 암호화 기능을 합니다. [자세히 보기](/docs/containers?topic=containers-security#encrypted_disk). 암호화를 사용 안함으로 설정하려면 이 옵션을 포함하고 값을 <code>false</code>로 설정하십시오.</td></tr>
</tbody></table></p></dd>

<dt><code>--hardware <em>HARDWARE</em></code></dt>
<dd>작업자 노드에 대한 하드웨어 격리의 레벨입니다. 사용자 전용으로만 실제 리소스를 사용 가능하게 하려면 dedicated를 사용하고, 실제 리소스를 다른 IBM 고객과 공유하도록 허용하려면 shared를 사용하십시오. 기본값은 shared입니다. 이 값은 선택사항입니다. 베어메탈 머신 유형의 경우에는 `dedicated`를 지정하십시오.</dd>

<dt><code>--machine-type <em>MACHINE_TYPE</em></code></dt>
<dd>머신 유형을 선택합니다. 공유 또는 전용 하드웨어에서 가상 머신으로서 또는 베어메탈에서 실제 머신으로서 작업자 노드를 배치할 수 있습니다. 사용 가능한 실제 및 가상 머신 유형은 클러스터가 배치되는 구역에 따라 다양합니다. 자세한 정보는 `ibmcloud ks machine-types` [명령](/docs/containers?topic=containers-cs_cli_reference#cs_machine_types)에 대한 문서를 참조하십시오. 이 값은 표준 클러스터의 경우 필수이며 무료 클러스터에는 사용할 수 없습니다.</dd>

<dt><code>--workers <em>NUMBER</em></code></dt>
<dd>클러스터에서 작성할 작업자 노드의 수를 표시하는 정수입니다. 기본값은 1입니다. 이 값은 선택사항입니다.</dd>

<dt><code>--private-vlan <em>PRIVATE_VLAN</em></code></dt>
<dd>클러스터가 작성될 때 지정된 사설 VLAN입니다. 이 값은 필수입니다. 사설 VLAN 라우터는 항상 <code>bcr</code>(벡엔드 라우터)로 시작하고 공용 VLAN 라우터는 항상 <code>fcr</code>(프론트 엔드 라우터)로 시작합니다. 클러스터를 작성하고 공인 및 사설 VLAN을 지정할 때는 이러한 접두부 뒤의 숫자 및 문자 조합이 일치해야 합니다.</dd>

<dt><code>--public-vlan <em>PUBLIC_VLAN</em></code></dt>
<dd>클러스터가 작성될 때 지정된 공용 VLAN입니다. 이 값은 선택사항입니다. 작업자 노드가 사설 VLAN에만 존재하도록 하려는 경우 공용 VLAN ID를 제공하지 마십시오. 사설 VLAN 라우터는 항상 <code>bcr</code>(벡엔드 라우터)로 시작하고 공용 VLAN 라우터는 항상 <code>fcr</code>(프론트 엔드 라우터)로 시작합니다. 클러스터를 작성하고 공인 및 사설 VLAN을 지정할 때는 이러한 접두부 뒤의 숫자 및 문자 조합이 일치해야 합니다.<p class="note">작업자 노드가 사설 VLAN 전용으로 설정된 경우에는 [개인 서비스 엔드포인트를 사용으로 설정](/docs/containers?topic=containers-cs_network_ov#cs_network_ov_master_private)하거나 [게이트웨이 디바이스를 구성](/docs/containers?topic=containers-cs_network_ov#cs_network_ov_master_gateway)하여 작업자 노드와 클러스터 마스터가 통신할 수 있도록 해야 합니다.</p></dd>

<dt><code>--disable-disk-encrypt</code></dt>
<dd>작업자 노드는 기본적으로 AES 256비트 디스크 암호화 기능을 합니다. [자세히 보기](/docs/containers?topic=containers-security#encrypted_disk). 암호화를 사용 안함으로 설정하려면 이 옵션을 포함하십시오.</dd>

<dt><code>-s</code></dt>
<dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>

</dl>

**예제**:

  ```
  ibmcloud ks worker-add --cluster my_cluster --workers 3 --public-vlan my_public_VLAN_ID --private-vlan my_private_VLAN_ID --machine-type b3c.4x16 --hardware shared
  ```
  {: pre}

### ibmcloud ks worker-get
{: #cs_worker_get}

작업자 노드의 세부사항을 봅니다.
{: shortdesc}

```
ibmcloud ks worker-get --cluster [CLUSTER_NAME_OR_ID] --worker WORKER_NODE_ID [--json] [-s]
```
{: pre}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **뷰어** 플랫폼 역할

<strong>명령 옵션</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER_NAME_OR_ID</em></code></dt>
   <dd>작업자 노드 클러스터의 이름 또는 ID입니다.  이 값은 선택사항입니다.</dd>

   <dt><code>--worker <em>WORKER_NODE_ID</em></code></dt>
   <dd>작업자 노드의 이름입니다. <code>ibmcloud ks workers <em>CLUSTER</em></code>를 실행하여 클러스터의 작업자 노드에 대한 ID를 볼 수 있습니다. 이 값은 필수입니다.</dd>

   <dt><code>--json</code></dt>
   <dd>명령 출력을 JSON 형식으로 인쇄합니다.  이 값은 선택사항입니다.</dd>

   <dt><code>-s</code></dt>
   <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>
   </dl>

**예제**:

  ```
  ibmcloud ks worker-get --cluster my_cluster --worker kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w1
  ```
  {: pre}

**출력 예**:

  ```
  ID:           kube-dal10-123456789-w1
  State:        normal
  Status:       Ready
  Trust:        disabled
  Private VLAN: 223xxxx
  Public VLAN:  223xxxx
  Private IP:   10.xxx.xx.xxx
  Public IP:    169.xx.xxx.xxx
  Hardware:     shared
  Zone:         dal10
  Version:      1.8.11_1509
  ```
  {: screen}

### ibmcloud ks worker-reboot
{: #cs_worker_reboot}

클러스터에서 작업자 노드를 다시 부팅합니다. 다시 부팅 중에 작업자 노드의 상태가 변경되지 않습니다. 예를 들어, IBM Cloud 인프라(SoftLayer)의 작업자 노드 상태가 `Powered Off`이며 사용자가 작업자 노드를 켜야 하는 경우에는 재부팅을 사용할 수 있습니다. 재부팅은 임시 디렉토리를 지우지만 전체 파일 시스템을 지우거나 디스크를 다시 포맷하지는 않습니다. 작업자 노드 공용 및 사설 IP 주소는 다시 부팅 오퍼레이션 후에도 동일하게 유지됩니다.
{: shortdesc}

```
ibmcloud ks worker-reboot [-f] [--hard] --cluster CLUSTER --worker WORKER [WORKER] [--skip-master-healthcheck] [-s]
```
{: pre}

**주의:** 작업자 노드를 다시 부팅하면 작업자 노드에서 데이터 손상이 발생할 수 있습니다. 이 명령은 다시 부팅하면 작업자 노드를 복구하는 데 도움이 된다고 알고 있는 경우에 주의하여 사용하십시오. 다른 모든 경우에는 대신 [작업자 노드를 다시 로드](#cs_worker_reload)하십시오.

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **운영자** 플랫폼 역할

작업자 노드를 다시 부팅하기 전에 다른 작업자 노드에서 팟(Pod)을 다시 스케줄하여 앱의 작동 중단 또는 작업자 노드의 데이터 손상을 방지할 수 있는지 확인하십시오.

1. 클러스터의 모든 작업자 노드를 나열하고 다시 부팅할 작업자 노드의 **이름**을 기록해 두십시오.
   ```
kubectl get nodes
   ```
   {: pre}
   이 명령에서 리턴되는 **이름**은 작업자 노드에 지정된 사설 IP 주소입니다. `ibmcloud ks workers <cluster_name_or_ID>` 명령을 실행하고 동일한 **사설 IP** 주소로 작업자 노드를 검색하면 작업자 노드에 대한 추가 정보를 찾을 수 있습니다.
2. 유출(cordoning)이라고 알려진 프로세스에서 작업자 노드를 스케줄 불가능으로 표시하십시오. 작업자 노드를 유출할 때 이후 팟(Pod) 스케줄링에서 사용할 수 없도록 합니다. 이전 단계에서 검색한 작업자 노드의 **이름**을 사용하십시오.
   ```
   kubectl cordon <worker_name>
   ```
   {: pre}

3. 팟(Pod) 스케줄링이 작업자 노드에 사용 불가능한지 확인하십시오.
   ```
kubectl get nodes
   ```
   {: pre}
상태가 **`SchedulingDisabled`**로 표시되는 경우 작업자 노드가 팟(Pod) 스케줄링에 사용 불가능합니다.
 4. 작업자 노드에서 팟(Pod)을 제거하고 클러스터에 남아 있는 작업자 노드로 다시 스케줄하도록 강제 실행하십시오.
    ```
    kubectl drain <worker_name>
    ```
    {: pre}
    이 프로세스에는 몇 분 정도 소요될 수 있습니다.
 5. 작업자 노드를 다시 부팅하십시오. `ibmcloud ks workers <cluster_name_or_ID>` 명령에서 리턴되는 작업자 ID를 사용하십시오.
    ```
    ibmcloud ks worker-reboot --cluster <cluster_name_or_ID> --workers <worker_name_or_ID>
    ```
    {: pre}
 6. 다시 부팅이 완료되었는지 확인하려면 작업자 노드를 팟(Pod) 스케줄링에서 사용할 수 있도록 하기 전에 약 5분 간 기다리십시오. 다시 부팅 중에 작업자 노드의 상태가 변경되지 않습니다. 일반적으로 작업자 노드의 다시 부팅은 몇 초 후에 완료됩니다.
 7. 작업자 노드를 팟(Pod) 스케줄링에서 사용할 수 있도록 하십시오. `kubectl get nodes` 명령에서 리턴되는 작업자 노드의 **이름**을 사용하십시오.
    ```
    kubectl uncordon <worker_name>
    ```
    {: pre}

    </br>

<strong>명령 옵션</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

   <dt><code>-f</code></dt>
   <dd>사용자 프롬프트를 표시하지 않고 작업자 노드의 재시작을 강제 실행하려면 이 옵션을 사용하십시오.  이 값은 선택사항입니다.</dd>

   <dt><code>--hard</code></dt>
   <dd>작업자 노드의 전원을 끊어서 작업자 노드의 하드 다시 시작을 강제 실행하려면 이 옵션을 사용하십시오. 작업자 노드가 반응하지 않거나 작업자 노드의 컨테이너 런타임이 반응하지 않으면 이 옵션을 사용하십시오.  이 값은 선택사항입니다.</dd>

   <dt><code>--workers <em>WORKER</em></code></dt>
   <dd>하나 이상의 작업자 노드의 이름 또는 ID입니다. 여러 작업자 노드를 나열하려면 공백을 사용하십시오. 이 값은 필수입니다.</dd>

   <dt><code>--skip-master-healthcheck</code></dt>
   <dd>작업자 노드를 다시 로드하거나 다시 부팅하기 전에 마스터의 상태 검사를 건너뜁니다.</dd>

   <dt><code>-s</code></dt>
   <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>
   </dl>

**예제**:

  ```
  ibmcloud ks worker-reboot --cluster my_cluster --worker kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w1 kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w2
  ```
  {: pre}



### ibmcloud ks worker-reload
{: #cs_worker_reload}

작업자 노드의 구성을 다시 로드합니다. 다시 로드는 작업자 노드에 성능 저하와 같은 문제점이 발생하거나 작업자 노드가 비정상적인 상태인 경우 유용할 수 있습니다. 다시 로드하는 중에 작업자 노드 머신은 최신 이미지로 업데이트되며 [작업자 노드의 외부에 저장](/docs/containers?topic=containers-storage_planning#persistent_storage_overview)되지 않은 경우 데이터가 삭제됨을 유념하십시오. 작업자 노드 IP 주소는 다시 로드 오퍼레이션 후에도 동일하게 유지됩니다.
{: shortdesc}

```
ibmcloud ks worker-reload [-f] --cluster CLUSTER --workers WORKER [WORKER] [--skip-master-healthcheck] [-s]
```
{: pre}

작업자 노드를 다시 로드하면 작업자 노드에 패치 버전 업데이트가 적용되지만 주 버전 또는 부 버전 업데이트는 적용되지 않습니다. 한 패치 버전에서 다음 패치 버전으로의 변경사항을 보려면 [버전 변경 로그](/docs/containers?topic=containers-changelog#changelog) 문서를 검토하십시오.
{: tip}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **운영자** 플랫폼 역할

작업자 노드를 다시 로드하기 전에 다른 작업자 노드에서 팟(Pod)을 다시 스케줄하여 앱의 작동 중단 또는 작업자 노드의 데이터 손상을 방지할 수 있는지 확인하십시오.

1. 클러스터의 모든 작업자 노드를 나열하고 다시 로드할 작업자 노드의 **이름**을 기록해 두십시오.
   ```
kubectl get nodes
   ```
   이 명령에서 리턴되는 **이름**은 작업자 노드에 지정된 사설 IP 주소입니다. `ibmcloud ks workers <cluster_name_or_ID>` 명령을 실행하고 동일한 **사설 IP** 주소로 작업자 노드를 검색하면 작업자 노드에 대한 추가 정보를 찾을 수 있습니다.
2. 유출(cordoning)이라고 알려진 프로세스에서 작업자 노드를 스케줄 불가능으로 표시하십시오. 작업자 노드를 유출할 때 이후 팟(Pod) 스케줄링에서 사용할 수 없도록 합니다. 이전 단계에서 검색한 작업자 노드의 **이름**을 사용하십시오.
   ```
   kubectl cordon <worker_name>
   ```
   {: pre}

3. 팟(Pod) 스케줄링이 작업자 노드에 사용 불가능한지 확인하십시오.
   ```
kubectl get nodes
   ```
   {: pre}
상태가 **`SchedulingDisabled`**로 표시되는 경우 작업자 노드가 팟(Pod) 스케줄링에 사용 불가능합니다.
 4. 작업자 노드에서 팟(Pod)을 제거하고 클러스터에 남아 있는 작업자 노드로 다시 스케줄하도록 강제 실행하십시오.
    ```
    kubectl drain <worker_name>
    ```
    {: pre}
    이 프로세스에는 몇 분 정도 소요될 수 있습니다.
 5. 작업자 노드를 다시 로드하십시오. `ibmcloud ks workers <cluster_name_or_ID>` 명령에서 리턴되는 작업자 ID를 사용하십시오.
    ```
    ibmcloud ks worker-reload --cluster <cluster_name_or_ID> --workers <worker_name_or_ID>
    ```
    {: pre}
 6. 다시 로드가 완료될 때까지 기다리십시오.
 7. 작업자 노드를 팟(Pod) 스케줄링에서 사용할 수 있도록 하십시오. `kubectl get nodes` 명령에서 리턴되는 작업자 노드의 **이름**을 사용하십시오.
    ```
    kubectl uncordon <worker_name>
    ```
</br>
<strong>명령 옵션</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

   <dt><code>-f</code></dt>
   <dd>사용자 프롬프트를 표시하지 않고 작업자 노드의 재로드를 강제 실행하려면 이 옵션을 사용하십시오.  이 값은 선택사항입니다.</dd>

   <dt><code>--worker <em>WORKER</em></code></dt>
   <dd>하나 이상의 작업자 노드의 이름 또는 ID입니다. 여러 작업자 노드를 나열하려면 공백을 사용하십시오. 이 값은 필수입니다.</dd>

   <dt><code>--skip-master-healthcheck</code></dt>
   <dd>작업자 노드를 다시 로드하거나 다시 부팅하기 전에 마스터의 상태 검사를 건너뜁니다.</dd>

   <dt><code>-s</code></dt>
   <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>
   </dl>

**예제**:

  ```
  ibmcloud ks worker-reload --cluster my_cluster --workers kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w1 kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w2
  ```
  {: pre}


### ibmcloud ks worker-rm
{: #cs_worker_rm}

클러스터에서 하나 이상의 작업자 노드를 제거합니다. 작업자 노드를 제거하면 클러스터가 불균형 상태가 됩니다. `ibmcloud ks worker-pool-rebalance` [명령](#cs_rebalance)을 실행하여 작업자 풀을 자동으로 리밸런싱할 수 있습니다.
{: shortdesc}

```
ibmcloud ks worker-rm [-f] --cluster CLUSTER --workers WORKER[,WORKER] [-s]
```
{: pre}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **운영자** 플랫폼 역할

작업자 노드를 제거하기 전에 다른 작업자 노드에서 팟(Pod)을 다시 스케줄하여 앱의 작동 중단 또는 작업자 노드의 데이터 손상을 방지할 수 있는지 확인하십시오.
{: tip}

1. 클러스터의 모든 작업자 노드를 나열하고 제거할 작업자 노드의 **이름**을 기록해 두십시오.
   ```
kubectl get nodes
   ```
   {: pre}
   이 명령에서 리턴되는 **이름**은 작업자 노드에 지정된 사설 IP 주소입니다. `ibmcloud ks workers <cluster_name_or_ID>` 명령을 실행하고 동일한 **사설 IP** 주소로 작업자 노드를 검색하면 작업자 노드에 대한 추가 정보를 찾을 수 있습니다.
2. 유출(cordoning)이라고 알려진 프로세스에서 작업자 노드를 스케줄 불가능으로 표시하십시오. 작업자 노드를 유출할 때 이후 팟(Pod) 스케줄링에서 사용할 수 없도록 합니다. 이전 단계에서 검색한 작업자 노드의 **이름**을 사용하십시오.
   ```
   kubectl cordon <worker_name>
   ```
   {: pre}

3. 팟(Pod) 스케줄링이 작업자 노드에 사용 불가능한지 확인하십시오.
   ```
kubectl get nodes
   ```
   {: pre}
상태가 **`SchedulingDisabled`**로 표시되는 경우 작업자 노드가 팟(Pod) 스케줄링에 사용 불가능합니다.
4. 작업자 노드에서 팟(Pod)을 제거하고 클러스터에 남아 있는 작업자 노드로 다시 스케줄하도록 강제 실행하십시오.
   ```
   kubectl drain <worker_name>
   ```
   {: pre}
   이 프로세스에는 몇 분 정도 소요될 수 있습니다.
5. 작업자 노드를 제거하십시오. `ibmcloud ks workers <cluster_name_or_ID>` 명령에서 리턴되는 작업자 ID를 사용하십시오.
   ```
   ibmcloud ks worker-rm --cluster <cluster_name_or_ID> --worker <worker_name_or_ID>
   ```
   {: pre}

6. 작업자 노드가 제거되었는지 확인하십시오.
   ```
   ibmcloud ks workers --cluster <cluster_name_or_ID>
   ```
   {: pre}
</br>
<strong>명령 옵션</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

   <dt><code>-f</code></dt>
   <dd>사용자 프롬프트를 표시하지 않고 작업자 노드의 제거를 강제 실행하려면 이 옵션을 사용하십시오.  이 값은 선택사항입니다.</dd>

   <dt><code>--workers <em>WORKER</em></code></dt>
   <dd>하나 이상의 작업자 노드의 이름 또는 ID입니다. 여러 작업자 노드를 나열하려면 공백을 사용하십시오. 이 값은 필수입니다.</dd>

   <dt><code>-s</code></dt>
   <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>
   </dl>

**예제**:

  ```
  ibmcloud ks worker-rm --cluster my_cluster --workers kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w1 kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w2
  ```
  {: pre}



### ibmcloud ks worker-update
{: #cs_worker_update}

작업자 노드를 업데이트하여 운영 체제에 최신 보안 업데이트 및 패치를 적용하고, Kubernetes 마스터의 버전과 일치하도록 Kubernetes 버전을 업데이트합니다. `ibmcloud ks cluster-update` [명령](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_update)으로 마스터 Kubernetes 버전을 업데이트할 수 있습니다. 작업자 노드 IP 주소는 업데이트 오퍼레이션 후에도 동일하게 유지됩니다.
{: shortdesc}

```
ibmcloud ks worker-update [-f] --cluster CLUSTER --workers WORKER[,WORKER] [--force-update] [-s]
```
{: pre}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **운영자** 플랫폼 역할

`ibmcloud ks worker-update`를 실행하면 앱과 서비스의 가동 중단이 발생할 수 있습니다. 업데이트 중에 모든 팟(Pod)은 기타 작업자 노드로 다시 스케줄되고 작업 노드의 이미지가 재작성되며 팟(Pod) 외부에 저장되지 않은 경우 데이터가 삭제됩니다. 가동 중단을 방지하려면 [선택한 작업자 노드가 업데이트되는 동안 워크로드를 처리하기에 충분한 작업자 노드가 있는지 확인](/docs/containers?topic=containers-update#worker_node)하십시오.
{: important}

업데이트하기 전에 배치를 위해 YAML 파일을 변경해야 할 수도 있습니다. 세부사항은 이 [릴리스 정보](/docs/containers?topic=containers-cs_versions)를 검토하십시오.

<strong>명령 옵션</strong>:

   <dl>

   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>사용 가능한 작업자 노드를 나열하는 클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

   <dt><code>-f</code></dt>
   <dd>사용자 프롬프트를 표시하지 않고 마스터 업데이트를 강제 실행하려면 이 옵션을 사용하십시오.  이 값은 선택사항입니다.</dd>

   <dt><code>--force-update</code></dt>
   <dd>변경 시 부 버전의 차이가 2보다 큰 경우에도 업데이트를 시도합니다.  이 값은 선택사항입니다.</dd>

   <dt><code>--workers <em>WORKER</em></code></dt>
   <dd>하나 이상의 작업자 노드의 ID입니다. 여러 작업자 노드를 나열하려면 공백을 사용하십시오. 이 값은 필수입니다.</dd>

   <dt><code>-s</code></dt>
   <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>

   </dl>

**예제**:

  ```
  ibmcloud ks worker-update --cluster my_cluster --worker kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w1 kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w2
  ```
  {: pre}


### ibmcloud ks workers
{: #cs_workers}

작업자 노드의 목록과 클러스터에서 각각의 상태(status)를 봅니다.
{: shortdesc}

```
ibmcloud ks workers --cluster CLUSTER [--worker-pool POOL] [--show-pools] [--show-deleted] [--json] [-s]
```
{: pre}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **뷰어** 플랫폼 역할

<strong>명령 옵션</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>사용 가능한 작업자 노드에 대한 클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

   <dt><code>--worker-pool <em>POOL</em></code></dt>
   <dd>작업자 풀에 속하는 작업자 노드만 봅니다. 사용 가능한 작업자 풀을 나열하려면 `ibmcloud ks worker-pools --cluster <cluster_name_or_ID>`를 실행하십시오.  이 값은 선택사항입니다.</dd>

   <dt><code>--show-pools</code></dt>
   <dd>각 작업자 노드가 속하는 작업자 풀을 나열합니다.  이 값은 선택사항입니다.</dd>

   <dt><code>--show-deleted</code></dt>
   <dd>삭제 이유를 포함, 클러스터에서 삭제된 작업자 노드를 봅니다.  이 값은 선택사항입니다.</dd>

   <dt><code>--json</code></dt>
   <dd>명령 출력을 JSON 형식으로 인쇄합니다.  이 값은 선택사항입니다.</dd>

  <dt><code>-s</code></dt>
  <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>
   </dl>

**예제**:

  ```
  ibmcloud ks workers --cluster my_cluster
  ```
  {: pre}

<br />



## 작업자 풀 명령
{: #worker-pool}

### ibmcloud ks worker-pool-create
{: #cs_worker_pool_create}

클러스터에서 작업자 풀을 작성할 수 있습니다. 작업자 풀을 추가할 때 이에는 기본적으로 구역이 지정되지 않습니다. 작업자에 대한 머신 유형 및 각 구역에서 원하는 작업자의 수는 사용자가 지정합니다. 작업자 풀에는 기본 Kubernetes 버전이 부여됩니다. 작업자 작성을 완료하려면 풀에 [하나 이상의 구역을 추가](#cs_zone_add)하십시오.
{: shortdesc}

```
ibmcloud ks worker-pool-create --name POOL_NAME --cluster CLUSTER --machine-type MACHINE_TYPE --size-per-zone WORKERS_PER_ZONE --hardware ISOLATION [--labels LABELS] [--disable-disk-encrypt] [-s] [--json]
```
{: pre}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **운영자** 플랫폼 역할

<strong>명령 옵션</strong>:
<dl>

  <dt><code>--name <em>POOL_NAME</em></code></dt>
    <dd>작업자 풀에 부여할 이름입니다.</dd>

  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

  <dt><code>--machine-type <em>MACHINE_TYPE</em></code></dt>
    <dd>머신 유형을 선택합니다. 공유 또는 전용 하드웨어에서 가상 머신으로서 또는 베어메탈에서 실제 머신으로서 작업자 노드를 배치할 수 있습니다. 사용 가능한 실제 및 가상 머신 유형은 클러스터가 배치되는 구역에 따라 다양합니다. 자세한 정보는 `ibmcloud ks machine-types` [명령](/docs/containers?topic=containers-cs_cli_reference#cs_machine_types)에 대한 문서를 참조하십시오. 이 값은 표준 클러스터의 경우 필수이며 무료 클러스터에는 사용할 수 없습니다.</dd>

  <dt><code>--size-per-zone <em>WORKERS_PER_ZONE</em></code></dt>
    <dd>각 구역에 작성할 작업자의 수입니다. 이 값은 필수이며 1 이상이어야 합니다.</dd>

  <dt><code>--hardware <em>ISOLATION</em></code></dt>
    <dd>작업자 노드에 대한 하드웨어 격리의 레벨입니다. 사용자 전용으로만 실제 리소스가 사용 가능하도록 하려면 `dedicated`를 사용하고, 실제 리소스가 다른 IBM 고객과 공유되도록 허용하려면 `shared`를 사용하십시오. 기본값은 `shared`입니다. 베어메탈 머신 유형의 경우에는 `dedicated`를 지정하십시오. 이 값은 필수입니다.</dd>

  <dt><code>--labels <em>LABELS</em></code></dt>
    <dd>풀에 있는 작업자에게 지정할 레이블입니다. 예: `<key1>=<val1>`,`<key2>=<val2>`</dd>

  <dt><code>--disable-disk-encrpyt</code></dt>
    <dd>디스크가 암호화되지 않도록 지정합니다. 기본값은 <code>false</code>입니다.</dd>

  <dt><code>--json</code></dt>
    <dd>명령 출력을 JSON 형식으로 인쇄합니다.  이 값은 선택사항입니다.</dd>

  <dt><code>-s</code></dt>
    <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>
</dl>

**예제**:

  ```
  ibmcloud ks worker-pool-create --name my_pool --cluster my_cluster --machine-type b3c.4x16 --size-per-zone 6
  ```
  {: pre}


### ibmcloud ks worker-pool-get
{: #cs_worker_pool_get}

작업자 풀의 세부사항을 봅니다.
{: shortdesc}

```
ibmcloud ks worker-pool-get --worker-pool WORKER_POOL --cluster CLUSTER [-s] [--json]
```
{: pre}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **뷰어** 플랫폼 역할

<strong>명령 옵션</strong>:

<dl>
  <dt><code>--worker-pool <em>WORKER_POOL</em></code></dt>
    <dd>세부사항을 보려는 작업자 노드 풀의 이름입니다. 사용 가능한 작업자 풀을 나열하려면 `ibmcloud ks worker-pools --cluster <cluster_name_or_ID>`를 실행하십시오. 이 값은 필수입니다.</dd>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>작업자 풀이 있는 클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

  <dt><code>--json</code></dt>
    <dd>명령 출력을 JSON 형식으로 인쇄합니다.  이 값은 선택사항입니다.</dd>

  <dt><code>-s</code></dt>
    <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>
</dl>

**예제**:

  ```
  ibmcloud ks worker-pool-get --worker-pool pool1 --cluster my_cluster
  ```
  {: pre}

**출력 예**:

  ```
  Name:               pool
  ID:                 a1a11b2222222bb3c33c3d4d44d555e5-f6f777g
  State:              active
  Hardware:           shared
  Zones:              dal10,dal12
  Workers per zone:   3
  Machine type:       b3c.4x16.encrypted
  Labels:             -
  Version:            1.12.7_1512
  ```
  {: screen}

### ibmcloud ks worker-pool-rebalance
{: #cs_rebalance}

작업자 노드를 삭제한 후에 작업자 풀을 리밸런싱할 수 있습니다. 이 명령을 실행하면 하나 이상의 새 작업자가 작업자 풀에 추가됩니다.
{: shortdesc}

```
ibmcloud ks worker-pool-rebalance --cluster CLUSTER --worker-pool WORKER_POOL [-s]
```
{: pre}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **운영자** 플랫폼 역할

<strong>명령 옵션</strong>:

<dl>
  <dt><code><em>--cluster CLUSTER</em></code></dt>
    <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>
  <dt><code><em>--worker-pool WORKER_POOL</em></code></dt>
    <dd>리밸런싱을 수행할 작업자 풀입니다. 이 값은 필수입니다.</dd>
  <dt><code>-s</code></dt>
    <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>
</dl>

**예제**:

  ```
  ibmcloud ks worker-pool-rebalance --cluster my_cluster --worker-pool my_pool
  ```
  {: pre}

### ibmcloud ks worker-pool-resize
{: #cs_worker_pool_resize}

작업자 풀의 크기를 조정하여 클러스터의 각 구역에 있는 작업자 노드의 수를 늘리거나 줄입니다. 작업자 풀에는 최소한 1개의 작업자 노드가 있어야 합니다.
{: shortdesc}

```
ibmcloud ks worker-pool-resize --worker-pool WORKER_POOL --cluster CLUSTER --size-per-zone WORKERS_PER_ZONE [-s]
```
{: pre}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **운영자** 플랫폼 역할

<strong>명령 옵션</strong>:

<dl>
  <dt><code>--worker-pool <em>WORKER_POOL</em></code></dt>
    <dd>업데이트하려는 작업자 노드 풀의 이름입니다. 이 값은 필수입니다.</dd>

  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>작업자 풀의 크기를 조정하려는 클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

  <dt><code>--size-per-zone <em>WORKERS_PER_ZONE</em></code></dt>
    <dd>각 구역에서 보유할 작업자의 수입니다. 이 값은 필수이며 1 이상이어야 합니다.</dd>

  <dt><code>-s</code></dt>
    <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>

</dl>

**예제**:

  ```
  ibmcloud ks worker-pool-resize --cluster my_cluster --worker-pool my_pool --size-per-zone 3
  ```
  {: pre}

### ibmcloud ks worker-pool-rm
{: #cs_worker_pool_rm}

클러스터에서 작업자 풀을 제거합니다. 풀에 있는 모든 작업자 노드가 삭제됩니다. 삭제 시 팟(Pod)이 다시 스케줄됩니다. 작동 중단을 방지하려면 워크로드를 실행하기에 충분한 작업자가 있는지 확인하십시오.
{: shortdesc}

```
ibmcloud ks worker-pool-rm --worker-pool WORKER_POOL --cluster CLUSTER [-f] [-s]
```
{: pre}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **운영자** 플랫폼 역할

<strong>명령 옵션</strong>:

<dl>
  <dt><code>--worker-pool <em>WORKER_POOL</em></code></dt>
    <dd>제거하려는 작업자 노드 풀의 이름입니다. 이 값은 필수입니다.</dd>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>작업자 풀을 제거하려는 클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>
  <dt><code>-f</code></dt>
    <dd>사용자 프롬프트를 표시하지 않고 명령을 강제 실행합니다.  이 값은 선택사항입니다.</dd>
  <dt><code>-s</code></dt>
    <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>
</dl>

**예제**:

  ```
  ibmcloud ks worker-pool-rm --cluster my_cluster --worker-pool pool1
  ```
  {: pre}

###         ibmcloud ks worker-pools
{: #cs_worker_pools}

클러스터에 있는 작업자 풀을 봅니다.
{: shortdesc}

```
ibmcloud ks worker-pools --cluster CLUSTER [--json] [-s]
```
{: pre}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **뷰어** 플랫폼 역할

<strong>명령 옵션</strong>:

<dl>
  <dt><code>--cluster <em>CLUSTER_NAME_OR_ID</em></code></dt>
    <dd>작업자 풀을 나열할 클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>
  <dt><code>--json</code></dt>
    <dd>명령 출력을 JSON 형식으로 인쇄합니다.  이 값은 선택사항입니다.</dd>
  <dt><code>-s</code></dt>
    <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>
</dl>

**예제**:

  ```
  ibmcloud ks worker-pools --cluster my_cluster
  ```
  {: pre}

### ibmcloud ks zone-add
{: #cs_zone_add}

**다중 구역 클러스터에만 해당**: 클러스터 또는 작업자 풀을 작성한 후에 구역을 추가할 수 있습니다. 구역을 추가하면 작업자 풀에 대해 지정한 구역당 작업자 수와 일치하도록 작업자 노드가 새 구역에 추가됩니다.
{: shortdesc}

```
ibmcloud ks zone-add --zone ZONE --cluster CLUSTER --worker-pools WORKER_POOL1[,WORKER_POOL2] --private-vlan PRIVATE_VLAN [--public-vlan PUBLIC_VLAN] [--private-only] [--json] [-s]
```
{: pre}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **운영자** 플랫폼 역할

<strong>명령 옵션</strong>:

<dl>
  <dt><code>--zone <em>ZONE</em></code></dt>
    <dd>추가하려는 구역입니다. 이는 클러스터의 지역 내에 있는 [다중 구역 가능 구역](/docs/containers?topic=containers-regions-and-zones#zones)이어야 합니다. 이 값은 필수입니다.</dd>

  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

  <dt><code>--worker-pool <em>WORKER_POOLS</em></code></dt>
    <dd>구역이 추가되는 작업자 풀의 쉼표로 구분된 목록입니다. 최소한 1개의 작업자 풀이 필요합니다.</dd>

  <dt><code>--private-vlan <em>PRIVATE_VLAN</em></code></dt>
    <dd><p>사설 VLAN의 ID입니다. 이 값은 조건부입니다.</p>
    <p>구역에 사설 VLAN이 있는 경우, 이 값은 클러스터에 있는 하나 이상의 작업자 노드의 사설 VLAN ID와 일치해야 합니다. 사용 가능한 VLAN을 보려면 <code>ibmcloud ks cluster-get --cluster &lt;cluster&gt; --showResources</code>를 실행하십시오. 지정된 VLAN에 임의의 새 작업자 노드가 추가되지만, 기존 작업자 노드에 대한 VLAN은 변경되지 않습니다.</p>
    <p>해당 구역에 사설 또는 공용 VLAN이 없는 경우에는 이 옵션을 지정하지 마십시오. 사설 및 공용 VLAN은 초기에 작업자 풀에 새 구역을 추가할 때 사용자를 위해 자동으로 작성됩니다.</p>
    <p>클러스터용 다중 VLAN, 동일한 VLAN의 다중 서브넷 또는 다중 구역 클러스터가 있는 경우에는 작업자 노드가 사설 네트워크에서 서로 간에 통신할 수 있도록 IBM Cloud 인프라(SoftLayer) 계정에 대해 [Virtual Router Function (VRF)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud)을 사용으로 설정해야 합니다. VRF를 사용으로 설정하려면 [IBM Cloud 인프라(SoftLayer) 계정 담당자에게 문의하십시오](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#how-you-can-initiate-the-conversion). VRF를 사용할 수 없거나 사용하지 않으려면 [VLAN Spanning](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning)을 사용으로 설정하십시오. 이 조치를 수행하려면 **네트워크 > 네트워크 VLAN Spanning 관리** [인프라 권한](/docs/containers?topic=containers-users#infra_access)이 필요합니다. 또는 이를 사용으로 설정하도록 계정 소유자에게 요청할 수 있습니다. VLAN Spanning이 이미 사용으로 설정되었는지 확인하려면 `ibmcloud ks vlan-spanning-get` [명령](/docs/containers?topic=containers-cs_cli_reference#cs_vlan_spanning_get)을 사용하십시오.</p></dd>

  <dt><code>--public-vlan <em>PUBLIC_VLAN</em></code></dt>
    <dd><p>공용 VLAN의 ID입니다. 클러스터를 작성한 후에 노드의 워크로드를 공용으로 노출하려면 이 값이 필요합니다. 이는 해당 구역의 클러스터에 있는 하나 이상의 작업자 노드의 공용 VLAN ID와 일치해야 합니다. 사용 가능한 VLAN을 보려면 <code>ibmcloud ks cluster-get --cluster &lt;cluster&gt; --showResources</code>를 실행하십시오. 지정된 VLAN에 임의의 새 작업자 노드가 추가되지만, 기존 작업자 노드에 대한 VLAN은 변경되지 않습니다.</p>
    <p>해당 구역에 사설 또는 공용 VLAN이 없는 경우에는 이 옵션을 지정하지 마십시오. 사설 및 공용 VLAN은 초기에 작업자 풀에 새 구역을 추가할 때 사용자를 위해 자동으로 작성됩니다.</p>
    <p>클러스터용 다중 VLAN, 동일한 VLAN의 다중 서브넷 또는 다중 구역 클러스터가 있는 경우에는 작업자 노드가 사설 네트워크에서 서로 간에 통신할 수 있도록 IBM Cloud 인프라(SoftLayer) 계정에 대해 [Virtual Router Function (VRF)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud)을 사용으로 설정해야 합니다. VRF를 사용으로 설정하려면 [IBM Cloud 인프라(SoftLayer) 계정 담당자에게 문의하십시오](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#how-you-can-initiate-the-conversion). VRF를 사용할 수 없거나 사용하지 않으려면 [VLAN Spanning](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning)을 사용으로 설정하십시오. 이 조치를 수행하려면 **네트워크 > 네트워크 VLAN Spanning 관리** [인프라 권한](/docs/containers?topic=containers-users#infra_access)이 필요합니다. 또는 이를 사용으로 설정하도록 계정 소유자에게 요청할 수 있습니다. VLAN Spanning이 이미 사용으로 설정되었는지 확인하려면 `ibmcloud ks vlan-spanning-get` [명령](/docs/containers?topic=containers-cs_cli_reference#cs_vlan_spanning_get)을 사용하십시오.</p></dd>

  <dt><code>--private-only </code></dt>
    <dd>공용 VLAN이 작성되지 않도록 하려면 이 옵션을 사용하십시오. `--private-vlan` 플래그를 지정하고 `--public-vlan` 플래그를 포함하지 않은 경우에만 필요합니다. <p class="note">작업자 노드가 사설 VLAN 전용으로 설정된 경우에는 개인 서비스 엔드포인트를 사용으로 설정하거나 게이트웨이 디바이스를 구성해야 합니다. 자세한 정보는 [사설 클러스터](/docs/containers?topic=containers-plan_clusters#private_clusters)를 참조하십시오.</p></dd>

  <dt><code>--json</code></dt>
    <dd>명령 출력을 JSON 형식으로 인쇄합니다.  이 값은 선택사항입니다.</dd>

  <dt><code>-s</code></dt>
    <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>
</dl>

**예제**:

  ```
  ibmcloud ks zone-add --zone dal10 --cluster my_cluster --worker-pools pool1,pool2,pool3 --private-vlan 2294021
  ```
  {: pre}

### ibmcloud ks zone-network-set
{: #cs_zone_network_set}

**다중 구역 클러스터에만 해당**: 구역에 대해 이전에 사용했던 것과 다른 공용 또는 사설 VLAN을 사용하도록 작업자 풀에 대한 네트워크 메타데이터를 설정하십시오. 풀에서 이미 작성된 작업자 노드는 계속해서 이전 공용 또는 사설 VLAN을 사용하지만, 풀의 새 작업자 노드는 새 네트워크 데이터를 사용합니다.
{: shortdesc}

```
ibmcloud ks zone-network-set --zone ZONE --cluster CLUSTER --worker-pools WORKER_POOL1[,WORKER_POOL2] --private-vlan PRIVATE_VLAN [--public-vlan PUBLIC_VLAN] [-f] [-s]
```
{: pre}

  <strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **운영자** 플랫폼 역할

  사설 VLAN 라우터는 항상 <code>bcr</code>(벡엔드 라우터)로 시작하고 공용 VLAN 라우터는 항상 <code>fcr</code>(프론트 엔드 라우터)로 시작합니다. 클러스터를 작성하고 공인 및 사설 VLAN을 지정할 때는 이러한 접두부 뒤의 숫자 및 문자 조합이 일치해야 합니다.
  <ol><li>클러스터에서 사용 가능한 VLAN을 확인하십시오. <pre class="pre"><code>ibmcloud ks cluster-get --cluster &lt;cluster_name_or_ID&gt; --showResources</code></pre><p>출력 예:</p>
  <pre class="screen"><code>Subnet VLANs
VLAN ID   Subnet CIDR         Public   User-managed
229xxxx   169.xx.xxx.xxx/29   true     false
229xxxx   10.xxx.xx.x/29      false    false</code></pre></li>
  <li>사용할 공용 및 사설 VLAN ID가 호환 가능한지 확인하십시오. 호환 가능하려면 <strong>라우터</strong>의 팟(Pod) ID가 동일해야 합니다.<pre class="pre"><code>ibmcloud ks vlans --zone &lt;zone&gt;</code></pre><p>출력 예:</p>
  <pre class="screen"><code>ID        Name   Number   Type      Router         Supports Virtual Workers
229xxxx          1234     private   bcr01a.dal12   true
229xxxx          5678     public    fcr01a.dal12   true</code></pre><p><strong>라우터</strong> 팟(Pod) ID가 일치함(`01a` 및 `01a`)을 유의하십시오. 하나의 팟(Pod) ID가 `01a`이고 다른 팟(Pod) ID가 `02a`인 경우에는 작업자 풀에 대해 이러한 공용 및 사설 VLAN ID를 설정할 수 없습니다.</p></li>
  <li>사용 가능한 VLAN이 없는 경우에는 <a href="/docs/infrastructure/vlans?topic=vlans-ordering-premium-vlans#ordering-premium-vlans">새 VLAN을 주문</a>할 수 있습니다.</li></ol>

  <strong>명령 옵션</strong>:

  <dl>
    <dt><code>--zone <em>ZONE</em></code></dt>
      <dd>추가하려는 구역입니다. 이는 클러스터의 지역 내에 있는 [다중 구역 가능 구역](/docs/containers?topic=containers-regions-and-zones#zones)이어야 합니다. 이 값은 필수입니다.</dd>

  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

  <dt><code>--worker-pool <em>WORKER_POOLS</em></code></dt>
    <dd>구역이 추가되는 작업자 풀의 쉼표로 구분된 목록입니다. 최소한 1개의 작업자 풀이 필요합니다.</dd>

  <dt><code>--private-vlan <em>PRIVATE_VLAN</em></code></dt>
    <dd>사설 VLAN의 ID입니다. 다른 작업자 노드에 사용한 것과 동일한 또는 상이한 사설 VLAN을 사용하고자 하는지 여부와 무관하게 이 값은 필수입니다. 지정된 VLAN에 임의의 새 작업자 노드가 추가되지만, 기존 작업자 노드에 대한 VLAN은 변경되지 않습니다.<p class="note">사설 및 공용 VLAN은 호환 가능해야 하며, **라우터** ID 접두부에서 이를 판별할 수 있습니다.</p></dd>

  <dt><code>--public-vlan <em>PUBLIC_VLAN</em></code></dt>
    <dd>공용 VLAN의 ID입니다. 이 값은 구역에 대한 공용 VLAN을 변경하려는 경우에만 필수입니다. 공용 VLAN을 변경하려면 항상 호환 가능한 사설 VLAN을 제공해야 합니다. 지정된 VLAN에 임의의 새 작업자 노드가 추가되지만, 기존 작업자 노드에 대한 VLAN은 변경되지 않습니다.<p class="note">사설 및 공용 VLAN은 호환 가능해야 하며, **라우터** ID 접두부에서 이를 판별할 수 있습니다.</p></dd>

  <dt><code>-f</code></dt>
    <dd>사용자 프롬프트를 표시하지 않고 명령을 강제 실행합니다.  이 값은 선택사항입니다.</dd>

  <dt><code>-s</code></dt>
    <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>
  </dl>

  **예제**:

  ```
  ibmcloud ks zone-network-set --zone dal10 --cluster my_cluster --worker-pools pool1,pool2,pool3 --private-vlan 2294021
  ```
  {: pre}

### ibmcloud ks zone-rm
{: #cs_zone_rm}

**다중 구역 클러스터에만 해당**: 클러스터의 모든 작업자 풀에서 구역을 제거하십시오. 이 구역에 대한 작업자 풀의 모든 작업자 노드가 삭제됩니다.
{: shortdesc}

```
ibmcloud ks zone-rm --zone ZONE --cluster CLUSTER [-f] [-s]
```
{: pre}

구역을 제거하기 전에, 작업자 노드에서 앱 또는 데이터 손상에 대한 작동 중지 시간의 방지를 돕기 위해 팟(Pod)의 재스케줄링이 가능하도록 클러스터의 기타 구역에 충분한 작업자 노드가 있는지 확인하십시오.
{: tip}

<strong>최소 필요 권한</strong>: {{site.data.keyword.containerlong_notm}}의 클러스터에 대한 **운영자** 플랫폼 역할

<strong>명령 옵션</strong>:

<dl>
  <dt><code>--zone <em>ZONE</em></code></dt>
    <dd>추가하려는 구역입니다. 이는 클러스터의 지역 내에 있는 [다중 구역 가능 구역](/docs/containers?topic=containers-regions-and-zones#zones)이어야 합니다. 이 값은 필수입니다.</dd>

  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

  <dt><code>-f</code></dt>
    <dd>사용자 프롬프트를 표시하지 않고 업데이트를 강제 실행합니다.  이 값은 선택사항입니다.</dd>

  <dt><code>-s</code></dt>
    <dd>오늘의 메시지 또는 업데이트 미리 알림을 표시하지 않습니다.  이 값은 선택사항입니다.</dd>
</dl>

**예제**:

  ```
  ibmcloud ks zone-rm --zone dal10 --cluster my_cluster
  ```
  {: pre}


