---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-07"

keywords: kubernetes, iks, clusters, worker nodes, worker pools, delete

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
{:gif: data-image-type='gif'}


# 클러스터 작성
{: #clusters}

{{site.data.keyword.containerlong}}에 Kubernetes 클러스터를 작성하십시오.
{: shortdesc}

아직도 시작 단계입니까? [Kubernetes 클러스터 작성 튜토리얼](/docs/containers?topic=containers-cs_cluster_tutorial#cs_cluster_tutorial)을 이용해 보십시오. 어떤 클러스터 설정을 선택해야 할지 모르시겠습니까? [클러스터 네트워크 설정 계획](/docs/containers?topic=containers-plan_clusters)을 참조하십시오.
{: tip}

이전에 클러스터를 작성했고 빠른 예제 명령을 찾을 수 있으십니까? 이 예제를 사용해 보십시오.
*  **무료 클러스터**:
   ```
   ibmcloud ks cluster-create --name my_cluster
   ```
   {: pre}
*  **표준 클러스터, 공유 가상 머신**:
   ```
   ibmcloud ks cluster-create --name my_cluster --zone dal10 --machine-type b3c.4x16 --hardware shared --workers 3 --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID>
   ```
   {: pre}
*  **표준 클러스터, 베어메탈**:
   ```
   ibmcloud ks cluster-create --name my_cluster --zone dal10 --machine-type mb2c.4x32 --hardware dedicated --workers 3 --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID>
   ```
   {: pre}
*  **표준 클러스터, VRF 사용 계정의 공용 및 개인 서비스 엔드포인트가 포함된 가상 머신**:
   ```
   ibmcloud ks cluster-create --name my_cluster --zone dal10 --machine-type b3c.4x16 --hardware shared --workers 3 --public-service-endpoint --private-service-endpoint --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID>
   ```
   {: pre}
*  **사설 VLAN 및 개인 서비스 엔드포인트만 사용하는 표준 클러스터**:
   ```
   ibmcloud ks cluster-create --name my_cluster --zone dal10 --machine-type b3c.4x16 --hardware shared --workers 3 --private-service-endpoint --private-vlan <private_VLAN_ID> --private-only
   ```
   {: pre}

<br />


## 계정 레벨에서의 클러스터 작성 준비
{: #cluster_prepare}

{{site.data.keyword.containerlong_notm}}를 위해 {{site.data.keyword.Bluemix_notm}} 계정을 준비하십시오. 이 준비 항목들은 계정 관리자가 한 번 수행하고 나면 클러스터를 작성할 때마다 변경하지 않아도 되는 항목입니다. 그러나 클러스터를 작성할 때마다 현재 계정 레벨 상태가 원하는 상태인지 확인하는 것은 여전히 중요합니다.
{: shortdesc}

1. [계정을 작성하거나 청구 가능 계정({{site.data.keyword.Bluemix_notm}} 종량과금제 또는 구독)으로 업그레이드](https://cloud.ibm.com/registration/)하십시오.

2. 클러스터를 작성할 지역에 [{{site.data.keyword.containerlong_notm}} API 키를 설정](/docs/containers?topic=containers-users#api_key)하십시오. 이 API 키에 클러스터를 작성할 수 있는 권한을 지정하십시오.
  * IBM Cloud 인프라(SoftLayer)에 대한 **수퍼유저** 역할.
  * 계정 레벨에서 {{site.data.keyword.containerlong_notm}}에 대한 **관리자** 플랫폼 관리 역할.
  * 계정 레벨에서 {{site.data.keyword.registrylong_notm}}에 대한 **관리자** 플랫폼 관리 역할. 사용자 계정이 2018년 10월 4일 이전의 계정인 경우, [{{site.data.keyword.registryshort_notm}}에 대한 {{site.data.keyword.Bluemix_notm}} IAM 정책을 사용으로 설정](/docs/services/Registry?topic=registry-user#existing_users)하십시오. IAM 정책을 사용하면 레지스트리 네임스페이스와 같은 리소스에 대한 액세스를 제어할 수 있습니다.

  계정 소유자이십니까? 이 경우에는 이미 필요한 권한을 갖고 있습니다. 클러스터를 작성하면 지역 및 리소스 그룹에 대한 API 키가 사용자의 인증 정보를 사용하여 설정됩니다.
  {: tip}

3. {{site.data.keyword.containerlong_notm}}에 대한 **Administrator** 플랫폼 역할이 있는지 확인하십시오. 클러스터가 사설 레지스트리에서 이미지를 가져올 수 있도록 하려면 {{site.data.keyword.registrylong_notm}}에 대한 **관리자** 플랫폼 역할도 필요합니다.
  1. [{{site.data.keyword.Bluemix_notm}} 콘솔 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/) 메뉴 표시줄에서 **관리 > 액세스(IAM)**를 클릭하십시오.
  2. **사용자** 페이지를 클릭한 후, 테이블에서 자신을 선택하십시오.
  3. **액세스 정책** 탭에서 자신의 **역할**이 **Administrator**인지 확인하십시오. 사용자는 계정 내 모든 리소스에 대한 **Administrator**이거나, 적어도 {{site.data.keyword.containershort_notm}}에 대해 이 역할일 수 있습니다. **참고**: 사용자가 전체 계정이 아니라 하나의 리소스 그룹 또는 지역에서만 {{site.data.keyword.containershort_notm}}에 대한 **Administrator** 역할을 갖고 있는 경우에는 계정의 VLAN을 보려면 계정 레벨에서 **Viewer** 이상의 역할을 갖고 있어야 합니다.
  <p class="tip">계정 관리자가 서비스 역할과 동시에 **관리자** 플랫폼 역할을 지정하지 않는지 확인하십시오. 플랫폼 및 서비스 역할을 별도로 지정해야 합니다.</p>

4. 계정이 여러 리소스 그룹을 사용하는 경우에는 계정의 [리소스 그룹 관리](/docs/containers?topic=containers-users#resource_groups) 전략을 파악하십시오.
  * 클러스터는 {{site.data.keyword.Bluemix_notm}}에 로그인할 때 대상으로 지정하는 리소스 그룹에 작성됩니다. 특정 리소스 그룹을 대상으로 지정하지 않으면 기본 리소스 그룹이 자동으로 대상으로 지정됩니다.
  * 기본이 아닌 리소스 그룹에 클러스터를 작성하려는 경우에는 해당 리소스 그룹에 대해 **Viewer** 이상의 역할을 갖고 있어야 합니다. 사용자에게 해당 리소스 그룹에 대한 역할이 전혀 없지만 사용자가 여전히 이 리소스 그룹 내에 있는 서비스의 **Administrator**인 경우에는 클러스터가 기본 리소스 그룹에 작성됩니다.
  * 클러스터의 리소스 그룹은 변경할 수 없습니다. 또한 `ibmcloud ks cluster-service-bind` [명령](/docs/containers-cli-plugin?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_service_bind)을 사용하여 [{{site.data.keyword.Bluemix_notm}} 서비스와 통합](/docs/containers?topic=containers-service-binding#bind-services)해야 하는 경우 해당 서비스는 클러스터와 동일한 리소스 그룹에 있어야 합니다. 리소스 그룹(예: {{site.data.keyword.registrylong_notm}})을 사용하지 않거나 서비스 바인딩(예: {{site.data.keyword.la_full_notm}})이 필요하지 않은 서비스는 클러스터가 다른 리소스 그룹에 있는 경우에도 작동합니다.
  * [메트릭에 대해 {{site.data.keyword.monitoringlong_notm}}](/docs/containers?topic=containers-health#view_metrics)을 사용하려는 경우에는 메트릭 이름 충돌을 방지하기 위해 계정 내 모든 리소스 그룹 및 지역 전체에서 고유한 이름을 클러스터에 지정하도록 계획하십시오.
  * 무료 클러스터는 `default` 리소스 그룹에 작성됩니다. 

5. **표준 클러스터**: 클러스터가 자신의 워크로드 및 환경 요구사항을 만족시킬 수 있도록 클러스터 [네트워크 설정](/docs/containers?topic=containers-plan_clusters)을 계획하십시오. 그 후 작업자 대 마스터 및 사용자 대 마스터 통신을 허용하도록 IBM Cloud 인프라(SoftLayer) 네트워킹을 설정하십시오. 
  * 개인 서비스 엔드포인트만 사용하거나 공용 및 개인 서비스 엔드포인트를 사용하려는 경우(인터넷 연결 워크로드를 실행하거나 온프레미스 데이터 센터를 확장)에는 다음을 수행하십시오. 
    1. IBM Cloud infrastructure (SoftLayer) 계정에서 [VRF](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud)를 사용으로 설정하십시오.
    2. [{{site.data.keyword.Bluemix_notm}} 계정에서 서비스 엔드포인트를 사용하도록 설정](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started)하십시오.
    <p class="note">Kubernetes 마스터는 권한 부여된 클러스터 사용자가 {{site.data.keyword.Bluemix_notm}} 사설 네트워크에 있거나, [VPN 연결](/docs/infrastructure/iaas-vpn?topic=VPN-gettingstarted-with-virtual-private-networking) 또는 [{{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link)를 통해 사설 네트워크에 연결되어 있는 경우 개인 서비스 엔드포인트를 통해 액세스할 수 있습니다. 그러나 개인 서비스 엔드포인트를 통한 Kubernetes 마스터와의 연결은 VPN 연결 또는 {{site.data.keyword.Bluemix_notm}} Direct Link를 통해 라우팅할 수 없는 <code>166.X.X.X</code> IP 주소 범위를 통해 설정되어야 합니다. 사용자는 클러스터 사용자를 위해 사설 네트워크 로드 밸런서(NLB)를 사용하여 개인 서비스 엔드포인트를 노출할 수 있습니다. 사설 NLB는 사용자가 VPN 또는 {{site.data.keyword.Bluemix_notm}} Direct Link 연결로 액세스할 수 있는 내부 <code>10.X.X.X</code> IP 주소로 마스터의 개인 서비스 엔드포인트를 노출합니다. 개인 서비스 엔드포인트만 사용으로 설정하는 경우에는 Kubernetes 대시보드를 사용하거나 임시로 공용 서비스 엔드포인트를 사용으로 설정하여 사설 NLB를 작성할 수 있습니다. 자세한 정보는 [개인 서비스 엔드포인트를 통한 클러스터 액세스](/docs/containers?topic=containers-clusters#access_on_prem)를 참조하십시오. </p>

  * 공용 서비스 엔드포인트만 사용하려는 경우(인터넷 연결 워크로드 실행)에는 다음을 수행하십시오. 
    1. 작업자 노드가 사설 네트워크에서 서로 통신할 수 있도록 IBM Cloud 인프라(SoftLayer) 계정에 대해 [VLAN Spanning](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning)을 사용으로 설정하십시오. 이 조치를 수행하려면 **네트워크 > 네트워크 VLAN Spanning 관리** [인프라 권한](/docs/containers?topic=containers-users#infra_access)이 필요합니다. 또는 이를 사용으로 설정하도록 계정 소유자에게 요청할 수 있습니다. VLAN Spanning이 이미 사용으로 설정되었는지 확인하려면 `ibmcloud ks vlan-spanning-get --region <region>` [명령](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get)을 사용하십시오. 
  * 게이트웨이 디바이스를 사용하려는 경우(온프레미스 데이터 센터를 확장)에는 다음을 수행하십시오. 
    1. 작업자 노드가 사설 네트워크에서 서로 통신할 수 있도록 IBM Cloud 인프라(SoftLayer) 계정에 대해 [VLAN Spanning](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning)을 사용으로 설정하십시오. 이 조치를 수행하려면 **네트워크 > 네트워크 VLAN Spanning 관리** [인프라 권한](/docs/containers?topic=containers-users#infra_access)이 필요합니다. 또는 이를 사용으로 설정하도록 계정 소유자에게 요청할 수 있습니다. VLAN Spanning이 이미 사용으로 설정되었는지 확인하려면 `ibmcloud ks vlan-spanning-get --region <region>` [명령](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get)을 사용하십시오. 
    2. 게이트웨이 디바이스를 구성하십시오. 예를 들면, 필요한 트래픽을 허용하고 원치 않는 트래픽은 차단하는 방화벽 역할을 수행하는 [Virtual Router Appliance](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-about-the-vra) 또는 [Fortigate Security Appliance](/docs/services/vmwaresolutions/services?topic=vmware-solutions-fsa_considerations)를 설정하도록 선택할 수 있습니다. 
    3. 마스터와 작업자 노드가 통신할 수 있도록 각 지역에 대해, 그리고 사용할 {{site.data.keyword.Bluemix_notm}} 서비스에 대해 [필수 사설 IP 주소 및 포트를 여십시오](/docs/containers?topic=containers-firewall#firewall_outbound). 

<br />


## 클러스터 레벨에서의 클러스터 작성 준비
{: #prepare_cluster_level}

클러스터 작성을 위한 계정을 설정한 후에는 클러스터 설정을 준비하십시오. 이 준비 항목들은 클러스터를 작성할 때마다 클러스터에 영향을 주는 항목입니다.
{: shortdesc}

1. [무료 또는 표준 클러스터](/docs/containers?topic=containers-cs_ov#cluster_types) 간에 결정하십시오. 1개의 무료 클러스터를 작성하여 30일 동안 일부 기능을 사용해 보거나, 선택한 하드웨어 격리로 완벽히 사용자 정의가 가능한 표준 클러스터를 작성할 수 있습니다. 더 많은 혜택을 받아 보고 클러스터 성능을 제어하고자 하려면 표준 클러스터를 작성하십시오.

2. 표준 클러스터의 경우에는  클러스터 설정을 계획하십시오. 
  * [단일 구역](/docs/containers?topic=containers-ha_clusters#single_zone) 또는 [다중 구역](/docs/containers?topic=containers-ha_clusters#multizone) 클러스터 중 무엇을 작성할지 결정하십시오. 다중 구역 클러스터는 선택된 위치에서만 사용 가능하다는 점을 유의하십시오.
  * 가상 머신 또는 베어메탈 머신 중에서 선택하는 것을 포함, 클러스터의 작업자 노드에 대해 사용할 [하드웨어 및 격리](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes) 유형을 선택하십시오.

3. 표준 클러스터의 경우에는 {{site.data.keyword.Bluemix_notm}} 콘솔에서 [비용을 예측](/docs/billing-usage?topic=billing-usage-cost#cost)할 수 있습니다. 예측기에 포함되지 않을 수 있는 비용에 대한 자세한 정보는 [가격 및 비용 청구](/docs/containers?topic=containers-faqs#charges)를 참조하십시오.

4. 온프레미스 데이터 센터를 확장하는 클러스터와 같이 방화벽으로 보호되는 환경에 클러스터를 작성하는 경우 사용하려는 {{site.data.keyword.Bluemix_notm}} 서비스에 대해 [공인 및 사설 IP에 대한 아웃바운드 네트워크 트래픽을 허용](/docs/containers?topic=containers-firewall#firewall_outbound)하십시오. 

<br />


## 무료 클러스터 작성
{: #clusters_free}

1개의 무료 클러스터를 사용하여 {{site.data.keyword.containerlong_notm}}의 작동 방법에 친숙해질 수 있습니다. 무료 클러스터를 사용하면 전문 용어를 배우고 튜토리얼을 마치며 프로덕션 레벨 표준 클러스터로 도약하기 전에 방향감을 터득할 수 있습니다. 걱정 마십시오. 무료 클러스터는 청구 가능 계정이 있어도 받을 수 있습니다.
{: shortdesc}

무료 클러스터는 2vCPU 및 4GB 메모리로 설정된 하나의 작업자 노드를 포함하며 유효 기간은 30일입니다. 이 기간이 지나면 클러스터가 만료되며 클러스터와 해당 데이터가 삭제됩니다. 삭제된 데이터는 {{site.data.keyword.Bluemix_notm}}에 의해 백업되지 않으며 이를 복원할 수 없습니다. 중요한 데이터는 반드시 백업을 받으십시오.
{: note}

### 콘솔에서 무료 클러스터 작성
{: #clusters_ui_free}

1. [카탈로그 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/catalog?category=containers)에서 클러스터를 작성할 **{{site.data.keyword.containershort_notm}}**를 선택하십시오.
2. **무료** 클러스터 플랜을 선택하십시오.
3. 클러스터를 배치할 지역을 선택하십시오.
4. 지역에서 메트로 위치를 선택하십시오. 클러스터가 이 메트로 내의 구역에 작성됩니다. 
5. 클러스터에 이름을 지정하십시오. 이름은 문자로 시작해야 하며 35자 이하의 문자, 숫자 및 하이픈(-)을 포함할 수 있습니다. 지역 전체에서 고유한 이름을 사용하십시오. 
6. **클러스터 작성**을 클릭하십시오. 기본적으로는 1개의 작업자 노드가 있는 작업자 풀이 작성됩니다. **작업자 노드** 탭에서 작업자 노드 배치의 진행상태를 볼 수 있습니다. 배치가 완료되면 **개요** 탭에서 클러스터가 준비되었는지 확인할 수 있습니다. 클러스터가 준비된 상태인 경우에도 레지스트리 이미지 풀 시크릿과 같이 다른 서비스에서 사용하는 클러스터의 일부는 여전히 처리 중 상태일 수 있다는 점을 유의하십시오. 

  모든 작업자 노드에는 클러스터가 작성된 이후 수동으로 변경될 수 없는 고유 작업자 노드 ID 및 도메인 이름이 지정됩니다. ID 또는 도메인 이름을 변경하면 Kubernetes 마스터가 클러스터를 관리할 수 없습니다.
  {: important}
7. 클러스터가 작성되고 나면 [CLI 세션을 구성하여 클러스터에 대한 작업을 시작](#access_cluster)할 수 있습니다. 

### CLI에서 무료 클러스터 작성
{: #clusters_cli_free}

시작하기 전에, {{site.data.keyword.Bluemix_notm}} CLI 및 [{{site.data.keyword.containerlong_notm}} 플러그인](/docs/containers?topic=containers-cs_cli_install#cs_cli_install)을 설치하십시오.

1. {{site.data.keyword.Bluemix_notm}} CLI에 로그인하십시오.
  1. 로그인을 수행하고 프롬프트가 표시되면 {{site.data.keyword.Bluemix_notm}} 인증 정보를 입력하십시오.
     ```
     ibmcloud login
     ```
     {: pre}

     연합 ID가 있는 경우에는 `ibmcloud login --sso`를 사용하여 {{site.data.keyword.Bluemix_notm}} CLI에 로그인하십시오. 사용자 이름을 입력하고 CLI 출력에서 제공된 URL을 사용하여 일회성 패스코드를 검색하십시오. `--sso` 옵션을 사용하지 않으면 로그인에 실패하고 `--sso` 옵션을 사용하면 성공하는 경우에는 연합 ID를 보유하고 있다는 것입니다.
     {: tip}

  2. 여러 {{site.data.keyword.Bluemix_notm}} 계정이 있는 경우에는 Kubernetes 클러스터를 작성할 계정을 선택하십시오.

  3. 특정 지역에 무료 클러스터를 작성하려면 해당 지역을 대상으로 지정해야 합니다. 무료 클러스터는 `ap-south`, `eu-central`, `uk-south` 또는 `us-south`에 작성할 수 있습니다. 이 클러스터는 해당 지역 내의 구역에 작성됩니다.
     ```
     ibmcloud ks region-set
     ```
     {: pre}

2. 클러스터를 작성하십시오.
  ```
  ibmcloud ks cluster-create --name my_cluster
  ```
  {: pre}

3. 클러스터 작성이 요청되었는지 확인하십시오. 작업자 노드 머신이 주문되는 데는 몇 분 정도 걸릴 수 있습니다.
    ```
    ibmcloud ks clusters
    ```
    {: pre}

    클러스터의 프로비저닝이 완료되면 클러스터의 상태가 **배치됨(deployed)**으로 변경됩니다.
    ```
    Name         ID                                   State      Created          Workers   Zone       Version     Resource Group Name
    my_cluster   paf97e8843e29941b49c598f516de72101   deployed   20170201162433   1         mil01      1.13.6      Default
    ```
    {: screen}

4. 작업자 노드의 상태를 확인하십시오.
    ```
    ibmcloud ks workers --cluster <cluster_name_or_ID>
    ```
    {: pre}

    작업자 노드가 준비되면 상태(state)가 **normal**로 변경되며, 상태(status)는 **Ready**입니다. 노드 상태(status)가 **준비(Ready)**인 경우에는 클러스터에 액세스할 수 있습니다. 클러스터가 준비된 상태인 경우에도 Ingress 시크릿 또는 레지스트리 이미지 풀 시크릿과 같이 다른 서비스에서 사용하는 클러스터의 일부는 여전히 처리 중 상태일 수 있다는 점을 유의하십시오.
    ```
    ID                                                 Public IP       Private IP      Machine Type   State    Status   Zone        Version     Resource Group Name
    kube-mil01-paf97e8843e29941b49c598f516de72101-w1   169.xx.xxx.xxx  10.xxx.xx.xxx   free           normal   Ready    mil01       1.13.6      Default
    ```
    {: screen}

    모든 작업자 노드에는 클러스터가 작성된 이후 수동으로 변경될 수 없는 고유 작업자 노드 ID 및 도메인 이름이 지정됩니다. ID 또는 도메인 이름을 변경하면 Kubernetes 마스터가 클러스터를 관리할 수 없습니다.
    {: important}

5. 클러스터가 작성되고 나면 [CLI 세션을 구성하여 클러스터에 대한 작업을 시작](#access_cluster)할 수 있습니다. 

<br />


## 표준 클러스터 작성
{: #clusters_standard}

사용자가 원하는 하드웨어 격리를 사용하는, 완전 사용자 정의 가능한 표준 클러스터를 작성하고 고가용성 환경을 위한 다중 작업자 노드와 같은 기능을 이용하려면 {{site.data.keyword.Bluemix_notm}} CLI 또는 {{site.data.keyword.Bluemix_notm}} 콘솔을 사용하십시오.
{: shortdesc}

### 콘솔에서 표준 클러스터 작성
{: #clusters_ui}

1. [카탈로그 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/catalog?category=containers)에서 클러스터를 작성할 **{{site.data.keyword.containershort_notm}}**를 선택하십시오.

2. 클러스터를 작성할 리소스 그룹을 선택하십시오.
  **참고**:
    * 특정 클러스터는 하나의 리소스 그룹에서만 작성될 수 있으며, 클러스터가 작성되고 나면 해당 리소스 그룹을 변경할 수 없습니다.
    * 기본 외의 리소스 그룹에 클러스터를 작성하려면 해당 리소스 그룹에 대해 [**Viewer** 역할](/docs/containers?topic=containers-users#platform) 이상의 역할을 갖고 있어야 합니다.

3. 클러스터를 배치할 지역을 선택하십시오.

4. 클러스터에 고유 이름을 지정하십시오. 이름은 문자로 시작해야 하며 35자 이하의 문자, 숫자 및 하이픈(-)을 포함할 수 있습니다. 지역 전체에서 고유한 이름을 사용하십시오. 클러스터 이름과 클러스터가 배치된 지역이 Ingress 하위 도메인의 완전한 이름을 형성합니다. 특정 Ingress 하위 도메인이 지역 내에서 고유하도록 하기 위해 클러스터 이름을 자르고 Ingress 도메인 이름 내의 무작위 값을 추가할 수 있습니다.
 **참고**: 작성 중에 지정된 고유 ID 또는 도메인 이름을 변경하면 Kubernetes 마스터가 더 이상 클러스터를 관리할 수 없습니다.

5. **단일 구역** 또는 **다중 구역** 가용성을 선택하십시오. 다중 구역 클러스터에서 마스터 노드는 다중 구역 가능 구역에 배치되며 클러스터의 리소스는 다중 구역 간에 전개됩니다.

6. 메트로 및 구역 세부사항을 입력하십시오. 
  * 다중 구역 클러스터:
    1. 메트로 위치를 선택하십시오. 최상의 성능을 얻으려면 실제로 사용자와 가장 가까운 메트로 위치를 선택하십시오. 선택사항은 지역에 따라 제한될 수 있습니다. 
    2. 클러스터가 호스팅될 특정 구역을 선택하십시오. 최소한 1개의 구역을 선택해야 하지만 원하는 수만큼 선택할 수 있습니다. 둘 이상의 구역을 선택하면 사용자가 선택한 구역 간에 작업자 노드가 전개되므로 보다 높은 고가용성이 제공됩니다. 1개의 구역만 선택하는 경우에는 작성 이후에 [클러스터에 구역을 추가](/docs/containers?topic=containers-add_workers#add_zone)할 수 있습니다.
  * 단일 구역 클러스터: 클러스터를 호스팅할 구역을 선택하십시오. 최상의 성능을 얻으려면 실제로 사용자와 가장 가까운 구역을 선택하십시오. 선택사항은 지역에 따라 제한될 수 있습니다. 

7. 각 구역에 대해 VLAN을 선택하십시오. 
  * 인터넷 연결 워크로드를 실행할 수 있는 클러스터를 작성하려면 다음을 수행하십시오. 
    1. 각 구역에 대해 IBM Cloud 인프라(SoftLayer) 계정에서 공용 VLAN 및 사설 VLAN을 선택하십시오. 작업자 노드는 사설 VLAN을 사용하여 서로 통신할 수 있으며, 공용 또는 사설 VLAN을 사용하여 Kubernetes 마스터와 통신할 수 있습니다. 사용자에게 이 구역의 공용 또는 사설 VLAN이 없는 경우에는 공용 및 사설 VLAN이 사용자를 위해 자동으로 작성됩니다. 다중 클러스터에 대해 동일한 VLAN을 사용할 수 있습니다.
  * 사설 네트워크 전용인 온프레미스 데이터 센터를 확장하는 클러스터, 온프레미스 데이터 센터를 확장하며 나중에 제한된 공용 액세스를 추가할 수 있는 클러스터, 또는 온프레미스 데이터 센터를 확장하고 게이트웨이 디바이스를 통해 제한된 공용 액세스를 제공하는 클러스터를 작성하려면 다음을 수행하십시오. 
    1. 각 구역에 대해 IBM Cloud 인프라(SoftLayer) 계정에서 사설 VLAN을 선택하십시오. 작업자 노드는 사설 VLAN을 사용하여 서로 간에 통신합니다. 자신에게 이 구역의 사설 VLAN이 없는 경우에는 사설 VLAN이 사용자를 위해 자동으로 작성됩니다. 다중 클러스터에 대해 동일한 VLAN을 사용할 수 있습니다.
    2. 공용 VLAN에 대해서는 **없음**을 선택하십시오. 

8. **마스터 서비스 엔드포인트**에서는 Kubernetes 마스터와 작업자 노드가 통신하는 방법을 선택하십시오. 
  * 인터넷 연결 워크로드를 실행할 수 있는 클러스터를 작성하려면 다음을 수행하십시오. 
    * {{site.data.keyword.Bluemix_notm}} 계정에서 VRF 및 서비스 엔드포인트가 사용으로 설정된 경우에는 **개인 및 공용 엔드포인트 모두**를 선택하십시오. 
    * VRF를 사용할 수 없거나 사용하지 않으려는 경우에는 **공용 엔드포인트만**을 선택하십시오. 
  * 온프레미스 데이터 센터만 확장하는 클러스터, 또는 온프레미스 데이터 센터를 확장하고 에지 작업자 노드를 사용하여 제한된 공용 액세스를 제공하는 클러스터를 작성하려면 **개인 및 공용 엔드포인트 모두** 또는 **개인 엔드포인트만**을 선택하십시오. {{site.data.keyword.Bluemix_notm}} 계정에서 VRF 및 서비스 엔드포인트를 사용으로 설정했는지 확인하십시오. 개인 서비스 엔드포인트만 사용으로 설정하는 경우에는 사용자가 VPN 또는 {{site.data.keyword.BluDirectLink}} 연결을 통해 마스터에 액세스할 수 있도록 [사설 네트워크 로드 밸런서를 통해 마스터 엔드포인트를 노출](#access_on_prem)해야 한다는 점을 유의하십시오. 
  * 온프레미스 데이터 센터를 확장하며 게이트웨이 디바이스를 사용하여 제한된 공용 액세스를 제공하는 클러스터를 작성하려면 **공용 엔드포인트만**을 선택하십시오. 

9. 기본 작업자 풀을 구성하십시오. 작업자 풀은 동일한 구성을 공유하는 작업자 노드의 그룹입니다. 나중에 언제든지 클러스터에 작업자 풀을 더 추가할 수 있습니다.
  1. 클러스터 마스터 노드와 작업자 노드의 Kubernetes API 서버 버전을 선택하십시오. 
  2. 머신 유형을 선택하여 작업자 특성을 필터링하십시오. 가상은 시간별로 비용이 청구되고 베어메탈은 월별로 비용이 청구됩니다.
    - **베어메탈**: 월별로 비용이 청구되는 베어메탈 서버는 주문 후 IBM Cloud 인프라(SoftLayer)에 의해 수동으로 프로비저닝되며 완료하는 데 영업일 기준으로 이틀 이상 걸릴 수 있습니다. 베어메탈은 추가 리소스 및 호스트 제어가 필요한 고성능 애플리케이션에 가장 적합합니다. [신뢰할 수 있는 컴퓨팅](/docs/containers?topic=containers-security#trusted_compute)을 사용으로 설정하여 작업자 노드의 변조 여부를 확인하도록 선택할 수도 있습니다. 신뢰할 수 있는 컴퓨팅은 선택된 베어메탈 머신 유형에만 사용할 수 있습니다. 예를 들어, `mgXc` GPU 특성(flavor)은 신뢰할 수 있는 컴퓨팅을 지원하지 않습니다. 클러스터 작성 중에 신뢰 사용을 설정하지 않고 나중에 이를 수행하려면 `ibmcloud ks feature-enable` [명령](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable)을 사용할 수 있습니다. 신뢰를 사용하도록 설정한 후에는 나중에 사용하지 않도록 설정할 수 없습니다.
    베어메탈 머신을 프로비저닝하려는지 확인하십시오. 월별로 비용이 청구되므로 실수로 주문한 후 즉시 취소하는 경우 한 달의 요금이 청구됩니다.
    {:tip}
    - **가상 - 공유**: 하이퍼바이저 및 실제 하드웨어와 같은 인프라 리소스가 사용자와 다른 IBM 고객 간에 공유되지만, 각 작업자 노드는 사용자만 액세스할 수 있습니다. 이 옵션은 비용이 적게 들고 대부분의 경우에 충분하지만 회사 정책에 따라 성능 및 인프라 요구사항을 확인해야 할 수 있습니다.
    - **가상 - 데디케이티드**: 작업자 노드가 사용자 계정 전용의 인프라에서 호스팅됩니다. 실제 리소스가 완전히 격리됩니다.
  3. 특성(flavor)을 선택하십시오. 특성(flavor)은 각 작업자 노드에서 설정되며 컨테이너에서 사용 가능한 가상 CPU, 메모리 및 디스크 공간의 양을 정의합니다. 사용 가능한 베어메탈 및 가상 머신 유형은 클러스터가 배치되는 구역에 따라 다양합니다. 클러스터를 작성한 후에는 작업자 또는 풀을 클러스터에 추가하여 서로 다른 머신 유형을 추가할 수 있습니다.
  4. 클러스터에서 필요한 작업자 노드의 수를 지정하십시오. 사용자가 입력하는 작업자의 수는 선택한 구역의 수에 따라 복제됩니다. 이는 2개의 구역을 보유 중이며 3개의 작업자 노드를 선택하는 경우에 6개의 노드가 프로비저닝되고 각 구역에서 3개의 노드가 라이브 상태임을 의미합니다.

10. **클러스터 작성**을 클릭하십시오. 작업자 풀은 사용자가 지정한 작업자 수로 작성됩니다. **작업자 노드** 탭에서 작업자 노드 배치의 진행상태를 볼 수 있습니다. 배치가 완료되면 **개요** 탭에서 클러스터가 준비되었는지 확인할 수 있습니다. 클러스터가 준비된 상태인 경우에도 Ingress 시크릿 또는 레지스트리 이미지 풀 시크릿과 같이 다른 서비스에서 사용하는 클러스터의 일부는 여전히 처리 중 상태일 수 있다는 점을 유의하십시오. 

  모든 작업자 노드에는 클러스터가 작성된 이후 수동으로 변경될 수 없는 고유 작업자 노드 ID 및 도메인 이름이 지정됩니다. ID 또는 도메인 이름을 변경하면 Kubernetes 마스터가 클러스터를 관리할 수 없습니다.
  {: important}

11. 클러스터가 작성되고 나면 [CLI 세션을 구성하여 클러스터에 대한 작업을 시작](#access_cluster)할 수 있습니다. 

### CLI에서 표준 클러스터 작성
{: #clusters_cli_steps}

시작하기 전에, {{site.data.keyword.Bluemix_notm}} CLI 및 [{{site.data.keyword.containerlong_notm}} 플러그인](/docs/containers?topic=containers-cs_cli_install#cs_cli_install)을 설치하십시오.

1. {{site.data.keyword.Bluemix_notm}} CLI에 로그인하십시오.
  1. 로그인을 수행하고 프롬프트가 표시되면 {{site.data.keyword.Bluemix_notm}} 인증 정보를 입력하십시오.
     ```
     ibmcloud login
     ```
     {: pre}

     연합 ID가 있는 경우에는 `ibmcloud login --sso`를 사용하여 {{site.data.keyword.Bluemix_notm}} CLI에 로그인하십시오. 사용자 이름을 입력하고 CLI 출력에서 제공된 URL을 사용하여 일회성 패스코드를 검색하십시오. `--sso` 옵션을 사용하지 않으면 로그인에 실패하고 `--sso` 옵션을 사용하면 성공하는 경우에는 연합 ID를 보유하고 있다는 것입니다.
     {: tip}

  2. 여러 {{site.data.keyword.Bluemix_notm}} 계정이 있는 경우에는 Kubernetes 클러스터를 작성할 계정을 선택하십시오.

  3. 기본 외의 리소스 그룹에 클러스터를 작성하려면 해당 리소스 그룹을 대상으로 지정하십시오. **참고**:
      * 특정 클러스터는 하나의 리소스 그룹에서만 작성될 수 있으며, 클러스터가 작성되고 나면 해당 리소스 그룹을 변경할 수 없습니다.
      * 해당 리소스 그룹에 대해 [**Viewer** 역할](/docs/containers?topic=containers-users#platform) 이상의 역할을 갖고 있어야 합니다.

      ```
      ibmcloud target -g <resource_group_name>
      ```
      {: pre}

2. 사용 가능한 구역을 검토하십시오. 다음 명령의 출력에서 구역의 **위치 유형**은 `dc`입니다. 구역 간에 클러스터를 전개하려면 [다중 구역 가능 구역](/docs/containers?topic=containers-regions-and-zones#zones)에서 클러스터를 작성해야 합니다. 다중 구역 가능 구역의 메트로 값은 **Multizone Metro** 열에 있습니다.
    ```
    ibmcloud ks supported-locations
    ```
    {: pre}
    자국 외에 있는 구역을 선택하는 경우에는 외국에서 데이터를 실제로 저장하기 전에 법적 인가를 받아야 할 수 있음을 유념하십시오.
    {: note}

3. 해당 구역에서 사용 가능한 작업자 노드 특성을 검토하십시오. 작업자 특성은 각 작업자 노드가 사용할 수 있는 가상 또는 실제 컴퓨팅 호스트를 지정합니다. 
  - **가상**: 시간별로 비용이 청구되는 가상 머신은 공유 또는 전용 하드웨어에 프로비저닝됩니다.
  - **실제**: 월별로 비용이 청구되는 베어메탈 서버는 주문 후 IBM Cloud 인프라(SoftLayer)에 의해 수동으로 프로비저닝되며 완료하는 데 영업일 기준으로 이틀 이상 걸릴 수 있습니다. 베어메탈은 추가 리소스 및 호스트 제어가 필요한 고성능 애플리케이션에 가장 적합합니다.
  - **신뢰할 수 있는 컴퓨팅의 실제 머신**: [신뢰할 수 있는 컴퓨팅](/docs/containers?topic=containers-security#trusted_compute)이 베어메탈 작업자 노드의 변조 여부를 확인할 수 있도록 선택할 수도 있습니다. 신뢰할 수 있는 컴퓨팅은 선택된 베어메탈 머신 유형에만 사용할 수 있습니다. 예를 들어, `mgXc` GPU 특성(flavor)은 신뢰할 수 있는 컴퓨팅을 지원하지 않습니다. 클러스터 작성 중에 신뢰 사용을 설정하지 않고 나중에 이를 수행하려면 `ibmcloud ks feature-enable` [명령](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable)을 사용할 수 있습니다. 신뢰를 사용하도록 설정한 후에는 나중에 사용하지 않도록 설정할 수 없습니다.
  - **머신 유형**: 배치할 머신 유형을 결정하려면 [사용 가능한 작업자 노드 하드웨어](/docs/containers?topic=containers-planning_worker_nodes#shared_dedicated_node)의 코어, 메모리 및 스토리지 조합을 검토하십시오. 클러스터를 작성한 후에는 [작업자 풀을 추가](/docs/containers?topic=containers-add_workers#add_pool)하여 서로 다른 실제 또는 가상 머신 유형을 추가할 수 있습니다.

     베어메탈 머신을 프로비저닝하려는지 확인하십시오. 월별로 비용이 청구되므로 실수로 주문한 후 즉시 취소하는 경우 한 달의 요금이 청구됩니다.
     {:tip}

  ```
  ibmcloud ks machine-types --zone <zone>
  ```
  {: pre}

4. 이 계정의 IBM Cloud 인프라(SoftLayer)에 해당 구역에 대한 VLAN이 있는지 확인하십시오. 
  ```
  ibmcloud ks vlans --zone <zone>
  ```
  {: pre}

  출력 예:
  ```
  ID        Name   Number   Type      Router
  1519999   vlan   1355     private   bcr02a.dal10
  1519898   vlan   1357     private   bcr02a.dal10
  1518787   vlan   1252     public    fcr02a.dal10
  1518888   vlan   1254     public    fcr02a.dal10
  ```
  {: screen}
  * 인터넷 연결 워크로드를 실행할 수 있는 클러스터를 작성하려면 공용 및 사설 VLAN이 있는지 확인하십시오. 공용 및 사설 VLAN이 이미 존재하는 경우 일치하는 라우터를 기록해 두십시오. 사설 VLAN 라우터는 항상 <code>bcr</code>(벡엔드 라우터)로 시작하고 공용 VLAN 라우터는 항상 <code>fcr</code>(프론트 엔드 라우터)로 시작합니다. 클러스터를 작성하고 공인 및 사설 VLAN을 지정할 때는 이러한 접두부 뒤의 숫자 및 문자 조합이 일치해야 합니다. 출력 예에서는 라우터가 모두 `02a.dal10`을 포함하고 있기 때문에 사설 VLAN이 공용 VLAN과 함께 사용될 수 있습니다.
  * 사설 네트워크 전용인 온프레미스 데이터 센터를 확장하는 클러스터, 온프레미스 데이터 센터를 확장하며 나중에 에지 작업자 노드를 통해 제한된 공용 액세스를 추가할 수 있는 클러스터, 또는 온프레미스 데이터 센터를 확장하고 게이트웨이 디바이스를 통해 제한된 공용 액세스를 제공하는 클러스터를 작성하려면 사설 VLAN이 있는지 확인하십시오. 사설 VLAN이 있는 경우에는 해당 ID를 기록하십시오. 

5. `cluster-create` 명령을 실행하십시오. 기본적으로 작업자 노드 디스크는 AES 256비트 암호화되며 클러스터는 사용 시간을 기준으로 비용이 청구됩니다. 
  * 인터넷 연결 워크로드를 실행할 수 있는 클러스터를 작성하려면 다음 명령을 실행하십시오.
    ```
    ibmcloud ks cluster-create --zone <zone> --machine-type <machine_type> --hardware <shared_or_dedicated> --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID> --workers <number> --name <cluster_name> --kube-version <major.minor.patch> [--private-service-endpoint] [--public-service-endpoint] [--disable-disk-encrypt] [--trusted]
    ```
    {: pre}
  * 사설 네트워크의 온프레미스 데이터 센터를 확장하며 나중에 에지 작업자 노드를 통해 제한된 공용 액세스를 추가할 수 있는 클러스터를 작성하려면 다음 명령을 실행하십시오.
    ```
    ibmcloud ks cluster-create --zone <zone> --machine-type <machine_type> --hardware <shared_or_dedicated> --private-vlan <private_VLAN_ID> --private-only --workers <number> --name <cluster_name> --kube-version <major.minor.patch> --private-service-endpoint [--public-service-endpoint] [--disable-disk-encrypt] [--trusted]
    ```
    {: pre}
  * 온프레미스 데이터 센터를 확장하며 게이트웨이 디바이스를 통해 제한된 공용 액세스를 제공하는 클러스터를 작성하려면 다음 명령을 실행하십시오.
    ```
    ibmcloud ks cluster-create --zone <zone> --machine-type <machine_type> --hardware <shared_or_dedicated> --private-vlan <private_VLAN_ID> --private-only --workers <number> --name <cluster_name> --kube-version <major.minor.patch> --public-service-endpoint [--disable-disk-encrypt] [--trusted]
    ```
    {: pre}

    <table>
    <caption>cluster-create 컴포넌트</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 이 명령의 컴포넌트 이해</th>
    </thead>
    <tbody>
    <tr>
    <td><code>cluster-create</code></td>
    <td>{{site.data.keyword.Bluemix_notm}} 조직에 클러스터를 작성하는 명령입니다.</td>
    </tr>
    <tr>
    <td><code>--zone <em>&lt;zone&gt;</em></code></td>
    <td>4단계에서 선택한 클러스터를 작성할 {{site.data.keyword.Bluemix_notm}} 구역 ID를 지정하십시오. </td>
    </tr>
    <tr>
    <td><code>--machine-type <em>&lt;machine_type&gt;</em></code></td>
    <td>5단계에서 선택한 머신 유형을 지정하십시오. </td>
    </tr>
    <tr>
    <td><code>--hardware <em>&lt;shared_or_dedicated&gt;</em></code></td>
    <td>작업자 노드에 대한 하드웨어 격리의 레벨을 지정하십시오. 사용자 전용으로만 실제 리소스를 사용 가능하게 하려면 dedicated를 사용하고, 실제 리소스를 다른 IBM 고객과 공유하도록 허용하려면 shared를 사용하십시오. 기본값은 shared입니다. VM 표준 클러스터의 경우 이 값은 선택사항입니다. 베어메탈 머신 유형의 경우에는 `dedicated`를 지정하십시오.</td>
    </tr>
    <tr>
    <td><code>--public-vlan <em>&lt;public_vlan_id&gt;</em></code></td>
    <td>IBM Cloud 인프라(SoftLayer) 계정에 해당 구역에 대한 공용 VLAN이 이미 설정되어 있는 경우에는 4단계에서 찾은 공용 VLAN의 ID를 입력하십시오. <p>사설 VLAN 라우터는 항상 <code>bcr</code>(벡엔드 라우터)로 시작하고 공용 VLAN 라우터는 항상 <code>fcr</code>(프론트 엔드 라우터)로 시작합니다. 클러스터를 작성하고 공인 및 사설 VLAN을 지정할 때는 이러한 접두부 뒤의 숫자 및 문자 조합이 일치해야 합니다.</p></td>
    </tr>
    <tr>
    <td><code>--private-vlan <em>&lt;private_vlan_id&gt;</em></code></td>
    <td>IBM Cloud 인프라(SoftLayer) 계정에 해당 구역에 대한 사설 VLAN이 이미 설정되어 있는 경우에는 4단계에서 찾은 사설 VLAN의 ID를 입력하십시오. 계정에 사설 VLAN이 없는 경우에는 이 옵션을 지정하지 마십시오. {{site.data.keyword.containerlong_notm}}에서 사용자를 위해 자동으로 사설 VLAN을 작성합니다.<p>사설 VLAN 라우터는 항상 <code>bcr</code>(벡엔드 라우터)로 시작하고 공용 VLAN 라우터는 항상 <code>fcr</code>(프론트 엔드 라우터)로 시작합니다. 클러스터를 작성하고 공인 및 사설 VLAN을 지정할 때는 이러한 접두부 뒤의 숫자 및 문자 조합이 일치해야 합니다.</p></td>
    </tr>
    <tr>
    <td><code>--private-only </code></td>
    <td>사설 VLAN 전용으로 클러스터를 작성합니다. 이 옵션을 포함하는 경우에는 <code>--public-vlan</code> 옵션을 포함하지 마십시오. </td>
    </tr>
    <tr>
    <td><code>--name <em>&lt;name&gt;</em></code></td>
    <td>클러스터의 이름을 지정하십시오. 이름은 문자로 시작해야 하며 35자 이하의 문자, 숫자 및 하이픈(-)을 포함할 수 있습니다. 지역 전체에서 고유한 이름을 사용하십시오. 클러스터 이름과 클러스터가 배치된 지역이 Ingress 하위 도메인의 완전한 이름을 형성합니다. 특정 Ingress 하위 도메인이 지역 내에서 고유하도록 하기 위해 클러스터 이름을 자르고 Ingress 도메인 이름 내의 무작위 값을 추가할 수 있습니다.
</td>
    </tr>
    <tr>
    <td><code>--workers <em>&lt;number&gt;</em></code></td>
    <td>클러스터에 포함시킬 작업자 노드의 수를 지정하십시오. <code>--workers</code> 옵션이 지정되지 않은 경우 1개의 작업자 노드가 작성됩니다.</td>
    </tr>
    <tr>
    <td><code>--kube-version <em>&lt;major.minor.patch&gt;</em></code></td>
    <td>클러스터 마스터 노드를 위한 Kubernetes 버전입니다. 이 값은 선택사항입니다. 버전이 지정되지 않은 경우에는 클러스터가 지원되는 Kubernetes 버전의 기본값으로 작성됩니다. 사용 가능한 버전을 보려면 <code>ibmcloud ks versions</code>를 실행하십시오.
</td>
    </tr>
    <tr>
    <td><code>--private-service-endpoint</code></td>
    <td>**[VRF 사용 계정](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started)의 경우**: Kubernetes 마스터와 작업자 노드가 사설 VLAN을 통해 서로 통신할 수 있도록 개인 서비스 엔드포인트를 사용으로 설정하십시오. 또한 `--public-service-endpoint` 플래그를 사용하여 공용 서비스 엔드포인트를 사용하도록 선택하여 인터넷을 통해 클러스터에 액세스할 수 있습니다. 개인 서비스 엔드포인트만 사용으로 설정한 경우 사설 VLAN에 연결되어 Kubernetes 마스터와 통신해야 합니다. 개인 서비스 엔드포인트를 사용으로 설정한 후에는 나중에 사용 안함으로 설정할 수 없습니다.<br><br>클러스터를 작성한 후에는 `ibmcloud ks cluster-get --cluster <cluster_name_or_ID>`를 실행하여 엔드포인트를 가져올 수 있습니다. </td>
    </tr>
    <tr>
    <td><code>--public-service-endpoint</code></td>
    <td>터미널에서 `kubectl` 명령을 실행하는 등과 같이 Kubernetes 마스터에 공용 네트워크를 통해 액세스하고, Kubernetes 마스터와 작업자 노드가 공용 VLAN을 통해 통신할 수 있도록 공용 서비스 엔드포인트를 사용으로 설정하십시오. 사설 전용 클러스터가 필요한 경우 나중에 공용 서비스 엔드포인트를 사용 안함으로 설정할 수 있습니다.<br><br>클러스터를 작성한 후에는 `ibmcloud ks cluster-get --cluster <cluster_name_or_ID>`를 실행하여 엔드포인트를 가져올 수 있습니다. </td>
    </tr>
    <tr>
    <td><code>--disable-disk-encrypt</code></td>
    <td>작업자 노드는 기본적으로 AES 256비트 [디스크 암호화](/docs/containers?topic=containers-security#encrypted_disk) 기능을 합니다. 암호화를 사용 안함으로 설정하려면 이 옵션을 포함하십시오.</td>
    </tr>
    <tr>
    <td><code>--trusted</code></td>
    <td>**베어메탈 클러스터**: [신뢰할 수 있는 컴퓨팅](/docs/containers?topic=containers-security#trusted_compute)을 사용으로 설정하여 베어메탈 작업자 노드의 변조 여부를 확인합니다. 신뢰할 수 있는 컴퓨팅은 선택된 베어메탈 머신 유형에만 사용할 수 있습니다. 예를 들어, `mgXc` GPU 특성(flavor)은 신뢰할 수 있는 컴퓨팅을 지원하지 않습니다. 클러스터 작성 중에 신뢰 사용을 설정하지 않고 나중에 이를 수행하려면 `ibmcloud ks feature-enable` [명령](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable)을 사용할 수 있습니다. 신뢰를 사용하도록 설정한 후에는 나중에 사용하지 않도록 설정할 수 없습니다.</td>
    </tr>
    </tbody></table>

6. 클러스터 작성이 요청되었는지 확인하십시오. 가상 머신의 경우 작업자 노드 머신을 정렬하며 클러스터를 설정하고 계정에 프로비저닝하는 데 몇 분 정도 걸릴 수 있습니다. 베어메탈 실제 머신은 IBM Cloud 인프라(SoftLayer)의 수동 상호작용으로 프로비저닝되며 완료하는 데 영업일 기준으로 이틀 이상 걸릴 수 있습니다.
    ```
    ibmcloud ks clusters
    ```
    {: pre}

    클러스터의 프로비저닝이 완료되면 클러스터의 상태가 **배치됨(deployed)**으로 변경됩니다.
    ```
    Name         ID                                   State      Created          Workers   Zone       Version     Resource Group Name
    my_cluster   paf97e8843e29941b49c598f516de72101   deployed   20170201162433   1         mil01      1.13.6      Default
    ```
    {: screen}

7. 작업자 노드의 상태를 확인하십시오.
    ```
    ibmcloud ks workers --cluster <cluster_name_or_ID>
    ```
    {: pre}

    작업자 노드가 준비되면 상태(state)가 **정상(normal)**으로 변경되며, 상태(status)는 **준비(Ready)**입니다. 노드 상태(status)가 **준비(Ready)**인 경우에는 클러스터에 액세스할 수 있습니다. 클러스터가 준비된 상태인 경우에도 Ingress 시크릿 또는 레지스트리 이미지 풀 시크릿과 같이 다른 서비스에서 사용하는 클러스터의 일부는 여전히 처리 중 상태일 수 있다는 점을 유의하십시오. 사설 VLAN 전용으로 클러스터를 작성한 경우에는 작업자 노드에 **공인 IP** 주소가 지정되지 않는다는 점을 유의하십시오. 

    ```
    ID                                                 Public IP       Private IP      Machine Type   State    Status   Zone        Version     Resource Group Name
    kube-mil01-paf97e8843e29941b49c598f516de72101-w1   169.xx.xxx.xxx  10.xxx.xx.xxx   standard       normal   Ready    mil01       1.13.6      Default
    ```
    {: screen}

    모든 작업자 노드에는 클러스터가 작성된 이후 수동으로 변경될 수 없는 고유 작업자 노드 ID 및 도메인 이름이 지정됩니다. ID 또는 도메인 이름을 변경하면 Kubernetes 마스터가 클러스터를 관리할 수 없습니다.
    {: important}

8. 클러스터가 작성되고 나면 [CLI 세션을 구성하여 클러스터에 대한 작업을 시작](#access_cluster)할 수 있습니다. 

<br />


## 클러스터에 액세스
{: #access_cluster}

클러스터가 작성되고 나면 CLI 세션을 구성하여 클러스터에 대한 작업을 시작할 수 있습니다.
{: shortdesc}

### 공용 서비스 엔드포인트를 통해 클러스터에 액세스
{: #access_internet}

클러스터에 대해 작업하려면 작성한 클러스터를 `kubectl` 명령을 실행하기 위한 CLI 세션의 컨텍스트로 설정하십시오.
{: shortdesc}

1. 네트워크가 회사 방화벽으로 보호되는 경우에는 {{site.data.keyword.Bluemix_notm}} 및 {{site.data.keyword.containerlong_notm}} API 엔드포인트와 포트에 대한 액세스를 허용하십시오. 
  1. [방화벽에서 `ibmcloud` API 및 `ibmcloud ks` API의 공용 엔드포인트에 대한 액세스를 허용](/docs/containers?topic=containers-firewall#firewall_bx)하십시오. 
  2. 공용 서비스 엔드포인트를 통해서만, 또는 개인 서비스 엔드포인트를 통해서만, 또는 공용 및 개인 서비스 엔드포인트를 통해 마스터에 액세스할 수 있도록 [권한 부여된 클러스터 사용자가 `kubectl` 명령을 실행할 수 있게 허용](/docs/containers?topic=containers-firewall#firewall_kubectl)하십시오. 
  3. 클러스터의 Calico 네트워크 정책을 관리할 수 있도록 [권한 부여된 클러스터 사용자가 `calicotl` 명령을 실행할 수 있게 허용](/docs/containers?topic=containers-firewall#firewall_calicoctl)하십시오. 

2. 작성한 클러스터를 이 세션에 대한 컨텍스트로 설정하십시오. 클러스터 관련 작업을 수행할 때마다 다음 구성 단계를 완료하십시오.

  {{site.data.keyword.Bluemix_notm}} 콘솔을 대신 사용하려는 경우에는 [Kubernetes 터미널](/docs/containers?topic=containers-cs_cli_install#cli_web)의 웹 브라우저에서 직접 CLI 명령을 실행할 수 있습니다.
  {: tip}
  1. 환경 변수를 설정하기 위한 명령을 가져오고 Kubernetes 구성 파일을 다운로드하십시오.
      ```
      ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
      ```
      {: pre}
      구성 파일 다운로드가 완료되면 환경 변수로서 경로를 로컬 Kubernetes 구성 파일로 설정하는 데 사용할 수 있는 명령이 표시됩니다.

      OS X에 대한 예:
      ```
      export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/mycluster/kube-config-prod-dal10-mycluster.yml
      ```
      {: screen}
  2. `KUBECONFIG` 환경 변수를 설정하려면 터미널에 표시되는 명령을 복사하여 붙여넣으십시오.
  3. `KUBECONFIG` 환경 변수가 올바르게 설정되었는지 확인하십시오.
      OS X에 대한 예:
      ```
      echo $KUBECONFIG
      ```
      {: pre}

      출력:
      ```
      /Users/<user_name>/.bluemix/plugins/container-service/clusters/mycluster/kube-config-prod-dal10-mycluster.yml
      ```
      {: screen}

3. 기본 포트 `8001`로 Kubernetes 대시보드를 실행하십시오.
  1. 기본 포트 번호로 프록시를 설정하십시오.
      ```
      kubectl proxy
      ```
      {: pre}

      ```
      Starting to serve on 127.0.0.1:8001
      ```
      {: screen}

  2.  웹 브라우저에서 다음 URL을 열어서 Kubernetes 대시보드를 보십시오.
      ```
      http://localhost:8001/ui
      ```
      {: codeblock}

### 개인 서비스 엔드포인트를 통해 클러스터에 액세스
{: #access_on_prem}

Kubernetes 마스터는 권한 부여된 클러스터 사용자가 {{site.data.keyword.Bluemix_notm}} 사설 네트워크에 있거나, [VPN 연결](/docs/infrastructure/iaas-vpn?topic=VPN-gettingstarted-with-virtual-private-networking) 또는 [{{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link)를 통해 사설 네트워크에 연결되어 있는 경우 개인 서비스 엔드포인트를 통해 액세스할 수 있습니다. 그러나 개인 서비스 엔드포인트를 통한 Kubernetes 마스터와의 연결은 VPN 연결 또는 {{site.data.keyword.Bluemix_notm}} Direct Link를 통해 라우팅할 수 없는 <code>166.X.X.X</code> IP 주소 범위를 통해 설정되어야 합니다. 사용자는 클러스터 사용자를 위해 사설 네트워크 로드 밸런서(NLB)를 사용하여 개인 서비스 엔드포인트를 노출할 수 있습니다. 사설 NLB는 사용자가 VPN 또는 {{site.data.keyword.Bluemix_notm}} Direct Link 연결로 액세스할 수 있는 내부 <code>10.X.X.X</code> IP 주소로 마스터의 개인 서비스 엔드포인트를 노출합니다. 개인 서비스 엔드포인트만 사용으로 설정하는 경우에는 Kubernetes 대시보드를 사용하거나 임시로 공용 서비스 엔드포인트를 사용으로 설정하여 사설 NLB를 작성할 수 있습니다.
{: shortdesc}

1. 네트워크가 회사 방화벽으로 보호되는 경우에는 {{site.data.keyword.Bluemix_notm}} 및 {{site.data.keyword.containerlong_notm}} API 엔드포인트와 포트에 대한 액세스를 허용하십시오. 
  1. [방화벽에서 `ibmcloud` API 및 `ibmcloud ks` API의 공용 엔드포인트에 대한 액세스를 허용](/docs/containers?topic=containers-firewall#firewall_bx)하십시오. 
  2. [권한 부여된 클러스터 사용자가 `kubectl` 명령을 실행할 수 있게 허용](/docs/containers?topic=containers-firewall#firewall_kubectl)하십시오. 사설 NLB를 사용하여 클러스터에 마스터의 개인 서비스 엔드포인트를 노출하기 전까지는 6단계에서 클러스터에 대한 연결을 테스트할 수 없다는 점을 유의하십시오. 

2. 클러스터의 개인 서비스 엔드포인트 URL 및 포트를 가져오십시오. 
  ```
  ibmcloud ks cluster-get --cluster <cluster_name_or_ID>
  ```
  {: pre}

  이 출력 예에서 **개인 서비스 엔드포인트 URL**은 `https://c1.private.us-east.containers.cloud.ibm.com:25073`입니다. 
  ```
  Name:                           setest
  ID:                             b8dcc56743394fd19c9f3db7b990e5e3
  State:                          normal
  Created:                        2019-04-25T16:03:34+0000
  Location:                       wdc04
  Master URL:                     https://c1.private.us-east.containers.cloud.ibm.com:25073
  Public Service Endpoint URL:    -
  Private Service Endpoint URL:   https://c1.private.us-east.containers.cloud.ibm.com:25073
  Master Location:                Washington D.C.
  ...
  ```
  {: screen}

3. `kube-api-via-nlb.yaml`이라는 이름의 YAML 파일을 작성하십시오. 이 YAML은 사설 `LoadBalancer` 서비스를 작성하고 이 NLB를 통해 개인 서비스 엔드포인트를 노출합니다. `<private_service_endpoint_port>`를 이전 단계에서 찾은 포트로 대체하십시오. 
  ```
  apiVersion: v1
  kind: Service
  metadata:
    name: kube-api-via-nlb
    annotations:
      service.kubernetes.io/ibm-load-balancer-cloud-provider-ip-type: private
    namespace: default
  spec:
    type: LoadBalancer
    ports:
    - protocol: TCP
      port: <private_service_endpoint_port>
      targetPort: <private_service_endpoint_port>
  ---
  kind: Endpoints
  apiVersion: v1
  metadata:
    name: kube-api-via-nlb
  subsets:
    - addresses:
        - ip: 172.20.0.1
      ports:
        - port: 2040
  ```
  {: codeblock}

4. 사설 NLB를 작성하려면 클러스터 마스터에 연결되어 있어야 합니다. 아직 VPN 또는 {{site.data.keyword.Bluemix_notm}} Direct Link에서 사설 서비스 엔드포인트를 통해 연결할 수 없으므로, 공용 서비스 엔드포인트 또는 Kubernetes 대시보드를 사용하여 클러스터 마스터에 연결하고 NLB를 작성해야 합니다. 
  * 개인 서비스 엔드포인트만 사용으로 설정한 경우에는 Kubernetes 대시보드를 사용하여 NLB를 작성할 수 있습니다. 이 대시보드는 자동으로 모든 요청을 마스터의 개인 서비스 엔드포인트로 라우팅합니다. 
    1.  [{{site.data.keyword.Bluemix_notm}} 콘솔](https://cloud.ibm.com/)에 로그인하십시오.
    2.  메뉴 표시줄에서 사용할 계정을 선택하십시오.
    3.  메뉴 ![메뉴 아이콘](../icons/icon_hamburger.svg "메뉴 아이콘")에서 **Kubernetes**를 클릭하십시오.
    4.  **클러스터** 페이지에서 액세스하려는 클러스터를 클릭하십시오.
    5.  클러스터 세부사항 페이지에서 **Kubernetes 대시보드**를 클릭하십시오. 
    6.  **+ 작성**을 클릭하십시오. 
    7.  **파일로부터 작성**을 선택하고 `kube-api-via-nlb.yaml` 파일을 업로드한 후 **업로드**를 클릭하십시오. 
    8.  **개요** 페이지에서 `kube-api-via-nlb` 서비스가 작성되었는지 확인하십시오. **외부 엔드포인트** 열에서 `10.x.x.x` 주소를 기록해 두십시오. 이 IP 주소는 Kubernetes 마스터의 개인 서비스 엔드포인트를 YAML 파일에 지정된 포트에서 노출합니다. 

  * 공용 서비스 엔드포인트도 사용으로 설정한 경우에는 이미 마스터에 액세스할 수 있는 상태입니다. 
    1. NLB 및 엔드포인트를 작성하십시오.
      ```
      kubectl apply -f kube-api-via-nlb.yaml
      ```
      {: pre}
    2. `kube-api-via-nlb` NLB가 작성되었는지 확인하십시오. 출력에서 `10.x.x.x` **EXTERNAL-IP** 주소를 기록해 두십시오. 이 IP 주소는 Kubernetes 마스터의 개인 서비스 엔드포인트를 YAML 파일에 지정된 포트에서 노출합니다.
      ```
      kubectl get svc -o wide
      ```
      {: pre}

      이 출력 예에서 Kubernetes 마스터의 개인 서비스 엔드포인트 IP 주소는 `10.186.92.42`입니다.
      ```
      NAME                     TYPE           CLUSTER-IP       EXTERNAL-IP      PORT(S)          AGE   SELECTOR
      kube-api-via-nlb         LoadBalancer   172.21.150.118   10.186.92.42     443:32235/TCP    10m   <none>
      ...
      ```
      {: screen}

  <p class="note">[strongSwan VPN 서비스](/docs/containers?topic=containers-vpn#vpn-setup)를 사용하여 마스터에 연결하려는 경우에는 다음 단계에서 사용하기 위해 `172.21.x.x` **CLUSTER-IP**를 대신 기록해 두십시오. strongSwan VPN 팟(Pod)은 클러스터 내부에서 실행되므로 내부 클러스터 IP 서비스의 IP 주소를 사용하여 NLB에 액세스할 수 있습니다. strongSwan Helm 차트에 대한 `config.yaml` 파일에서 Kubernetes 서비스 서브넷 CIDR인 `172.21.0.0/16`이 `local.subnet` 설정에 나열되어 있는지 확인하십시오. </p>

5. `kubectl` 명령을 실행할 클라이언트 시스템에서 `/etc/hosts` 파일에 NLB IP 주소와 개인 서비스 엔드포인트 URL을 추가하십시오. IP 주소 및 URL에 포트를 포함하지 말고, URL에 `https://`를 포함하지 마십시오. 
  * OSX 및 Linux 사용자의 경우 다음 명령을 실행하십시오.
    ```
    sudo nano /etc/hosts
    ```
    {: pre}
  * Windows 사용자의 경우 다음 명령을 실행하십시오.
    ```
    notepad C:\Windows\System32\drivers\etc\hosts
    ```
    {: pre}

    로컬 시스템 권한에 따라, 호스트 파일을 편집하기 위해 메모장을 관리자 권한으로 실행해야 할 수도 있습니다.
    {: tip}

  추가할 텍스트 예:
  ```
  10.186.92.42      c1.private.us-east.containers.cloud.ibm.com
  ```
  {: codeblock}

6. VPN 또는 {{site.data.keyword.Bluemix_notm}} Direct Link 연결을 통해 사설 네트워크에 연결했는지 확인하십시오. 

7. Kubernetes CLI 서버 버전을 확인하여 `kubectl` 명령이 개인 서비스 엔드포인트를 통해 클러스터에 대해 올바르게 실행되는지 확인하십시오. 
  ```
  kubectl version --short
  ```
  {: pre}

  출력 예:
  ```
  Client Version: v1.13.6
  Server Version: v1.13.6
  ```
  {: screen}

<br />


## 다음 단계
{: #next_steps}

클러스터가 시작되어 실행 중인 경우에는 다음 태스크를 수행할 수 있습니다.
- 다중 구역 가능 구역에 클러스터를 작성한 경우에는 [클러스터에 구역을 추가하여 작업자 노드를 분산](/docs/containers?topic=containers-add_workers)하십시오. 
- [클러스터에 앱을 배치합니다. ](/docs/containers?topic=containers-app#app_cli)
- [Docker 이미지를 저장하고 다른 사용자들과 공유하도록 {{site.data.keyword.Bluemix_notm}}에서 개인용 레지스트리를 설정합니다.](/docs/services/Registry?topic=registry-getting-started)
- 워크로드 리소스 요청을 기반으로 작업자 풀에 작업자 노드를 자동으로 추가하거나 제거하려면 [클러스터 오토스케일러를 설정](/docs/containers?topic=containers-ca#ca)합니다.
- [팟(Pod) 보안 정책](/docs/containers?topic=containers-psp)으로 클러스터에서 팟을 작성할 수 있는 사용자를 제어합니다.
- 클러스터 기능을 확장하기 위해 [Istio](/docs/containers?topic=containers-istio) 및 [Knative](/docs/containers?topic=containers-serverless-apps-knative) 관리 추가 기능을 사용으로 설정하십시오. 

그 후에는 클러스터 설정을 위한 다음 네트워크 구성 단계를 확인할 수 있습니다. 

### 클러스터에서 인터넷 연결 앱 워크로드 실행
{: #next_steps_internet}

* [에지 작업자 노드](/docs/containers?topic=containers-edge)에 대한 네트워킹 워크로드를 분리하십시오.
* [공용 네트워킹 서비스](/docs/containers?topic=containers-cs_network_planning#public_access)를 사용하여 앱을 노출하십시오. 
* 화이트리스트 및 블랙리스트 정책과 같은 [Calico Pre-DNAT 정책](/docs/containers?topic=containers-network_policies#block_ingress)을 작성하여 앱을 노출하는 네트워크 서비스에 대한 공용 트래픽을 제어하십시오. 
* [strongSwan IPSec VPN 서비스](/docs/containers?topic=containers-vpn)를 설정하여 클러스터를 {{site.data.keyword.Bluemix_notm}} 계정 외부의 사설 네트워크에 있는 서비스와 연결하십시오. 

### 온프레미스 데이터 센터를 클러스터로 확장하고 에지 노드 및 Calico 네트워크 정책을 사용하여 제한된 공용 액세스를 허용
{: #next_steps_calico}

* [{{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link) 또는 [strongSwan IPSec VPN 서비스](/docs/containers?topic=containers-vpn#vpn-setup)를 설정하여 클러스터를 {{site.data.keyword.Bluemix_notm}} 외부의 사설 네트워크에 있는 서비스와 연결하십시오. {{site.data.keyword.Bluemix_notm}} Direct Link는 클러스터에 있는 앱 및 서비스가 사설 네트워크를 통해 온프레미스 네트워크와 통신할 수 있도록 하며, strongSwan은 공용 네트워크를 통해 암호화된 VPN 터널을 거쳐 통신할 수 있도록 합니다. 
* 공용 및 사설 VLAN에 연결된 작업자 노드의 [에지 작업자 풀](/docs/containers?topic=containers-edge)을 작성하여 공용 네트워킹 워크로드를 격리하십시오. 
* [사설 네트워킹 서비스](/docs/containers?topic=containers-cs_network_planning#private_access)를 사용하여 앱을 노출하십시오. 
* [Calico 호스트 네트워크 정책을 작성](/docs/containers?topic=containers-network_policies#isolate_workers)하여 팟(Pod)에 대한 공용 액세스를 차단하고, 클러스터를 사설 네트워크로 격리하고, 다른 {{site.data.keyword.Bluemix_notm}} 서비스에 대한 액세스를 허용하십시오. 

### 온프레미스 데이터 센터를 클러스터로 확장하고 게이트웨이 디바이스를 사용하여 제한된 공용 액세스를 허용
{: #next_steps_gateway}

* 사설 네트워크에 대한 게이트웨이 방화벽도 구성하는 경우에는 [작업자 노드 간 통신을 허용하고 클러스터가 사설 네트워크를 통해 인프라 리소스에 액세스할 수 있도록 허용](/docs/containers?topic=containers-firewall#firewall_private)해야 합니다. 
* 작업자 노드 및 앱을 {{site.data.keyword.Bluemix_notm}} 계정 외부의 사설 네트워크에 안전하게 연결하려면 게이트웨이 디바이스에 IPSec VPN 엔드포인트를 설정하십시오. 그 후 게이트웨이의 VPN 엔드포인트를 사용하도록 클러스터에서 [strongSwan IPSec VPN 서비스를 구성](/docs/containers?topic=containers-vpn#vpn-setup)하거나 [VRA와의 직접 VPN 연결을 설정](/docs/containers?topic=containers-vpn#vyatta)하십시오. 
* [사설 네트워킹 서비스](/docs/containers?topic=containers-cs_network_planning#private_access)를 사용하여 앱을 노출하십시오. 
* 게이트웨이 디바이스에서 [필수 포트 및 IP 주소를 개방](/docs/containers?topic=containers-firewall#firewall_inbound)하여 네트워킹 서비스에 대한 인바운드 트래픽을 허용하십시오. 

### 온프레미스 데이터 센터를 사설 네트워크 전용의 클러스터로 확장
{: #next_steps_extend}

* 사설 네트워크에 방화벽이 있는 경우에는 [작업자 노드 간 통신을 허용하고 클러스터가 사설 네트워크를 통해 인프라 리소스에 액세스할 수 있도록 허용](/docs/containers?topic=containers-firewall#firewall_private)하십시오. 
* [{{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link)를 설정하여 클러스터를 {{site.data.keyword.Bluemix_notm}} 계정 외부의 사설 네트워크에 있는 서비스와 연결하십시오. 
* [사설 네트워킹 서비스](/docs/containers?topic=containers-cs_network_planning#private_access)를 사용하여 사설 네트워크의 앱을 노출하십시오. 
