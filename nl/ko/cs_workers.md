---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-06"

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



# 클러스터에 작업자 노드 및 구역 추가
{: #add_workers}

앱의 가용성을 높이기 위해 클러스터의 한 기존 구역 또는 다수의 기존 구역에 작업자 노드를 추가할 수 있습니다. 구역 장애로부터 앱을 보호하는 데 도움이 되도록 클러스터에 구역을 추가할 수 있습니다.
{:shortdesc}

클러스터를 작성하면 작업자 노드가 작업자 풀에서 프로비저닝됩니다. 클러스터 작성 후에는 해당 크기를 조정하거나 작업자 풀을 더 추가하여 풀에 작업자 노드를 더 추가할 수 있습니다. 기본적으로 작업자 풀은 하나의 구역에 존재합니다. 하나의 구역에만 작업자 풀이 있는 클러스터를 단일 구역 클러스터(single zone cluster)라고 합니다. 클러스터에 구역을 더 추가하면 작업자 풀이 구역 간에 존재합니다. 둘 이상의 구역 간에 전개된 작업자 풀이 있는 클러스터를 다중 구역 클러스터(multizone cluster)라고 합니다.

다중 구역 클러스터가 있으면 해당 작업자 노드 리소스의 밸런스를 유지하십시오. 모든 작업자 풀이 동일한 구역 간에 전개되어 있는지 확인하고 개별 노드를 추가하는 대신 풀의 크기를 조정하여 작업자를 추가하거나 제거하십시오.
{: tip}

시작하기 전에, [**운영자** 또는 **관리자** {{site.data.keyword.Bluemix_notm}} IAM 플랫폼 역할](/docs/containers?topic=containers-users#platform)이 있는지 확인하십시오. 그리고 다음 섹션 중 하나를 선택하십시오.
  * [클러스터에서 기존 작업자 풀의 크기를 조정하여 작업자 노드 추가](#resize_pool)
  * [클러스터에 작업자 풀을 추가하여 작업자 노드 추가](#add_pool)
  * [클러스터에 구역을 추가하고 다중 구역 간의 작업자 풀에서 작업자 노드 복제](#add_zone)
  * [더 이상 사용되지 않음: 클러스터에 독립형 작업자 노드 추가](#standalone)

작업자 풀을 설정한 후, 워크로드 리소스 요청을 기반으로 작업자 풀에 작업자 노드를 자동으로 추가하거나 제거하려면 [클러스터 오토스케일러를 설정](/docs/containers?topic=containers-ca#ca)할 수 있습니다.
{:tip}

## 기존 작업자 풀의 크기를 조정하여 작업자 노드 추가
{: #resize_pool}

작업자 풀이 하나의 구역에 있는지 또는 다중 구역 간에 전개되었는지 여부와 무관하게, 기존 작업자 풀의 크기를 조정하여 클러스터에 있는 작업자 노드의 수를 늘리거나 줄일 수 있습니다.
{: shortdesc}

예를 들어, 구역별로 3개의 작업자 노드가 있는 하나의 작업자 풀이 있는 클러스터를 고려하십시오.
* 클러스터가 단일 구역이고 `dal10`에 존재하는 경우, 작업자 풀에는 `dal10`에 3개의 작업자 노드가 있습니다. 클러스터에는 총 3개의 작업자 노드가 있습니다.
* 클러스터가 다중 구역이고 `dal10` 및 `dal12`에 존재하는 경우, 작업자 풀에는 `dal10`에 3개의 작업자 노드가 있으며 `dal12`에 3개의 작업자 노드가 있습니다. 클러스터에는 총 6개의 작업자 노드가 있습니다.

베어메탈 작업자 풀의 경우에는 비용이 매월 청구됨을 유념하십시오. 크기를 늘리거나 줄이는 경우, 이는 해당 월의 비용에 영향을 줍니다.
{: tip}

작업자 풀의 크기를 조정하려면 각 구역에서 작업자 풀에 배치된 작업자 노드의 수를 변경하십시오.

1. 크기 조정을 원하는 작업자 풀의 이름을 가져오십시오.
    ```
    ibmcloud ks worker-pools --cluster <cluster_name_or_ID>
    ```
    {: pre}

2. 각 구역에서 배치할 작업자 노드의 수를 지정하여 작업자 풀의 크기를 조정하십시오. 최소값은 1입니다.
    ```
    ibmcloud ks worker-pool-resize --cluster <cluster_name_or_ID> --worker-pool <pool_name>  --size-per-zone <number_of_workers_per_zone>
    ```
    {: pre}

3. 작업자 풀의 크기가 조정되었는지 확인하십시오.
    ```
    ibmcloud ks workers --cluster <cluster_name_or_ID> --worker-pool <pool_name>
    ```
    {: pre}

    Example output for a worker pool that is in two zones, `dal10` and `dal12`, and is resized to two worker nodes per zone:
    ```
    ID                                                 Public IP        Private IP      Machine Type      State    Status  Zone    Version
    kube-dal10-crb20b637238ea471f8d4a8b881aae4962-w7   169.xx.xxx.xxx   10.xxx.xx.xxx   b3c.4x16          normal   Ready   dal10   1.8.6_1504
    kube-dal10-crb20b637238ea471f8d4a8b881aae4962-w8   169.xx.xxx.xxx   10.xxx.xx.xxx   b3c.4x16          normal   Ready   dal10   1.8.6_1504
    kube-dal12-crb20b637238ea471f8d4a8b881aae4962-w9   169.xx.xxx.xxx   10.xxx.xx.xxx   b3c.4x16          normal   Ready   dal12   1.8.6_1504
    kube-dal12-crb20b637238ea471f8d4a8b881aae4962-w10  169.xx.xxx.xxx   10.xxx.xx.xxx   b3c.4x16          normal   Ready   dal12   1.8.6_1504
    ```
    {: screen}

## 새 작업자 풀을 작성하여 작업자 노드 추가
{: #add_pool}

새 작업자 풀을 작성하여 클러스터에 작업자 노드를 추가할 수 있습니다.
{:shortdesc}

1. 클러스터의 **작업자 구역**을 검색하고 작업자 풀에 작업자 노드를 배치할 구역을 선택하십시오. 단일 구역 클러스터를 보유하는 경우에는 **작업자 구역** 필드에 나타난 구역을 사용해야 합니다. 다중 구역 클러스터의 경우에는 클러스터의 기존 **작업자 구역**을 선택하거나 클러스터가 있는 지역의 [다중 구역 메트로 위치](/docs/containers?topic=containers-regions-and-zones#zones) 중 하나를 선택할 수 있습니다. `ibmcloud ks zones`를 실행하여 사용 가능한 구역을 나열할 수 있습니다.
   ```
   ibmcloud ks cluster-get --cluster <cluster_name_or_ID>
   ```
   {: pre}

   출력 예:
   ```
   ...
   Worker Zones: dal10, dal12, dal13
   ```
   {: screen}

2. 각 구역에 대해 사용 가능한 사설 및 공용 VLAN을 나열하십시오. 사용할 사설 및 공용 VLAN을 기록해 두십시오. 사설 또는 공용 VLAN이 없으면 구역을 작업자 풀에 추가할 때 사용자를 위해 VLAN이 자동으로 작성됩니다.
   ```
   ibmcloud ks vlans --zone <zone>
   ```
   {: pre}

3.  각 구역의 [작업자 노드에 사용 가능한 머신 유형](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)을 검토하십시오.

    ```
        ibmcloud ks machine-types <zone>
    ```
    {: pre}

4. 작업자 풀을 작성하십시오. `key=value` 레이블로 풀에 위치한 작업자 노드에 자동으로 레이블을 지정하도록 `--labels` 옵션을 포함하십시오. 베어메탈 작업자 풀을 프로비저닝하는 경우에는 `--hardware dedicated`를 지정하십시오.
   ```
   ibmcloud ks worker-pool-create --name <pool_name> --cluster <cluster_name_or_ID> --machine-type <machine_type> --size-per-zone <number_of_workers_per_zone> --hardware <dedicated_or_shared> --labels <key=value>
   ```
   {: pre}

5. 작업자 풀이 작성되었는지 확인하십시오.
   ```
   ibmcloud ks worker-pools --cluster <cluster_name_or_ID>
   ```
   {: pre}

6. 기본적으로, 작업자 풀을 추가하면 구역이 없는 풀이 작성됩니다. 구역에 작업자 노드를 배치하려면 이전에 검색한 구역을 작업자 풀에 추가해야 합니다. 다중 구역 전체에 작업자 노드를 분산시키려는 경우에는 각 구역에 대해 이 명령을 반복하십시오.
   ```
   ibmcloud ks zone-add --zone <zone> --cluster <cluster_name_or_ID> --worker-pools <pool_name> --private-vlan <private_VLAN_ID> --public-vlan <public_VLAN_ID>
   ```
   {: pre}

7. 사용자가 추가한 구역에서 작업자 노드의 프로비저닝을 확인하십시오. 상태가 **provision_pending**에서 **normal**로 변경되면 작업자 노드가 준비된 것입니다.
   ```
   ibmcloud ks workers --cluster <cluster_name_or_ID> --worker-pool <pool_name>
   ```
   {: pre}

   출력 예:
   ```
   ID                                                 Public IP        Private IP      Machine Type      State    Status  Zone    Version
   kube-dal10-crb20b637238ea471f8d4a8b881aae4962-w7   169.xx.xxx.xxx   10.xxx.xx.xxx   b3c.4x16          provision_pending   Ready   dal10   1.8.6_1504
   kube-dal10-crb20b637238ea471f8d4a8b881aae4962-w8   169.xx.xxx.xxx   10.xxx.xx.xxx   b3c.4x16          provision_pending   Ready   dal10   1.8.6_1504
   ```
   {: screen}

## 작업자 풀에 구역을 추가하여 작업자 노드 추가
{: #add_zone}

기존 작업자 풀에 구역을 추가하여 한 지역 내의 다중 구역 간에 클러스터를 전개할 수 있습니다.
{:shortdesc}

작업자 풀에 구역을 추가하면 작업자 풀에서 정의된 작업자 노드가 새 구역에서 프로비저닝되며 향후 워크로드 스케줄을 위해 고려됩니다. {{site.data.keyword.containerlong_notm}}는 지역에 대한 `failure-domain.beta.kubernetes.io/region` 레이블과 구역에 대한 `failure-domain.beta.kubernetes.io/zone` 레이블을 각 작업자 노드에 자동으로 추가합니다. Kubernetes 스케줄러는 이러한 레이블을 사용하여 동일한 지역 내의 구역 간에 팟(Pod)을 전개합니다.

클러스터에 다중 작업자 풀이 있는 경우에는 작업자 노드가 클러스터에서 균등하게 전개되도록 이들 모두에 구역을 추가하십시오.

시작하기 전에:
*  작업자 풀에 구역을 추가하려면 작업자 풀이 [다중 구역 가능 구역](/docs/containers?topic=containers-regions-and-zones#zones)에 있어야 합니다. 작업자 풀이 다중 구역 가능 구역에 없으면 [새 작업자 풀 작성](#add_pool)을 고려하십시오.
*  클러스터용 다중 VLAN, 동일한 VLAN의 다중 서브넷 또는 다중 구역 클러스터가 있는 경우에는 작업자 노드가 사설 네트워크에서 서로 간에 통신할 수 있도록 IBM Cloud 인프라(SoftLayer) 계정에 대해 [Virtual Router Function (VRF)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud)을 사용으로 설정해야 합니다. VRF를 사용으로 설정하려면 [IBM Cloud 인프라(SoftLayer) 계정 담당자에게 문의](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#how-you-can-initiate-the-conversion)하십시오. VRF를 사용할 수 없거나 사용하지 않으려면 [VLAN Spanning](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning)을 사용으로 설정하십시오. 이 조치를 수행하려면 **네트워크 > 네트워크 VLAN Spanning 관리** [인프라 권한](/docs/containers?topic=containers-users#infra_access)이 필요합니다. 또는 이를 사용으로 설정하도록 계정 소유자에게 요청할 수 있습니다. VLAN Spanning이 이미 사용으로 설정되었는지 확인하려면 `ibmcloud ks vlan-spanning-get --region <region>` [명령](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get)을 사용하십시오. 

작업자 노드가 있는 구역을 작업자 풀에 추가하려면 다음을 수행하십시오.

1. 사용 가능한 구역을 나열하고 작업자 풀에 추가할 구역을 선택하십시오. 선택하는 구역은 다중 구역 가능 구역이어야 합니다.
   ```
   ibmcloud ks zones
   ```
   {: pre}

2. 해당 구역의 사용 가능한 VLAN을 나열하십시오. 사설 또는 공용 VLAN이 없으면 구역을 작업자 풀에 추가할 때 사용자를 위해 VLAN이 자동으로 작성됩니다.
   ```
   ibmcloud ks vlans --zone <zone>
   ```
   {: pre}

3. 클러스터의 작업자 풀을 나열하고 해당 이름을 기록해 두십시오.
   ```
   ibmcloud ks worker-pools --cluster <cluster_name_or_ID>
   ```
   {: pre}

4. 작업자 풀에 구역을 추가하십시오. 다중 작업자 풀이 있는 경우에는 모든 구역에서 클러스터의 밸런스가 유지되도록 모든 작업자 풀에 구역을 추가하십시오. `<pool1_id_or_name,pool2_id_or_name,...>`을 모든 작업자 풀 이름의 쉼표로 구분된 목록으로 대체하십시오.

    다중 작업자 풀에 구역을 추가하려면 우선 사설 및 공용 VLAN이 있어야 합니다. 해당 구역에 공용 및 사설 VLAN이 없는 경우에는 공용 및 사설 VLAN이 사용자를 위해 작성되도록 우선 하나의 작업자 풀에 구역을 추가하십시오. 그러면 사용자를 위해 작성된 사설 및 공용 VLAN을 지정하여 다른 작업자 풀에 구역을 추가할 수 있습니다.
    {: note}

   서로 다른 작업자 풀에 대해 서로 다른 VLAN을 사용하려면 각 VLAN 및 이의 대응되는 작업자 풀에 대해 이 명령을 반복하십시오. 지정된 VLAN에 임의의 새 작업자 노드가 추가되지만, 기존 작업자 노드에 대한 VLAN은 변경되지 않습니다.
   {: tip}
   ```
   ibmcloud ks zone-add --zone <zone> --cluster <cluster_name_or_ID> --worker-pools <pool1_name,pool2_name,...> --private-vlan <private_VLAN_ID> --public-vlan <public_VLAN_ID>
   ```
   {: pre}

5. 구역이 클러스터에 추가되었는지 확인하십시오. 출력의 **Worker zones** 필드에서 추가된 구역을 찾으십시오. 추가된 구역에서 새 작업자 노드가 프로비저닝되었으므로 **Workers** 필드에서 총 작업자 수가 증가되었음을 유의하십시오.
  ```
   ibmcloud ks cluster-get --cluster <cluster_name_or_ID>
  ```
  {: pre}

  출력 예:
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
  Resource Group ID:              a8a12accd63b437bbd6d58fb6a462ca7
  Resource Group Name:            Default
  ```
  {: screen}

## 더 이상 사용되지 않음: 독립형 작업자 노드 추가
{: #standalone}

작업자 풀이 도입되기 전에 작성된 클러스터가 있는 경우에는 더 이상 사용되지 않는 명령을 사용하여 독립형 작업자 노드를 추가할 수 있습니다.
{: deprecated}

작업자 풀이 도입된 후에 작성된 클러스터가 있는 경우에는 독립형 작업자 노드를 추가할 수 없습니다. 그 대신에 [작업자 풀을 작성](#add_pool)하고 [기존 작업자 풀의 크기를 조정](#resize_pool)하거나 [작업자 풀에 구역을 추가](#add_zone)하여 클러스터에 작업자 노드를 추가할 수 있습니다.
{: note}

1. 사용 가능한 구역을 나열하고 작업자 노드를 추가할 구역을 선택하십시오.
   ```
   ibmcloud ks zones
   ```
   {: pre}

2. 해당 구역의 사용 가능한 VLAN을 나열하고 해당 ID를 기록하십시오.
   ```
   ibmcloud ks vlans --zone <zone>
   ```
   {: pre}

3. 해당 구역에서 사용 가능한 머신 유형을 나열하십시오.
   ```
   ibmcloud ks machine-types --zone <zone>
   ```
   {: pre}

4. 클러스터에 독립형 작업자 노드를 추가하십시오. 베어메탈 머신 유형의 경우에는 `dedicated`를 지정하십시오.
   ```
   ibmcloud ks worker-add --cluster <cluster_name_or_ID> --workers <number_of_worker_nodes> --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID> --machine-type <machine_type> --hardware <shared_or_dedicated>
   ```
   {: pre}

5. 작업자 노드가 작성되었는지 확인하십시오.
   ```
   ibmcloud ks workers --cluster <cluster_name_or_ID>
   ```
   {: pre}

<br />


## 기존 작업자 풀에 레이블 추가
{: #worker_pool_labels}

[작업자 풀을 작성](#add_pool)하거나 나중에 이 단계를 수행하여 작업자 풀에 레이블을 지정할 수 있습니다. 작업자 풀에 레이블이 지정된 후 모든 기존 및 후속 작업자 노드가 이 레이블을 가져옵니다. 특정 워크로드만 작업자 풀의 작업자 노드에 배치하도록 레이블을 사용할 수 있습니다(예: [로드 밸런서 네트워크 트래픽을 위한 에지 노드](/docs/containers?topic=containers-edge)).
{: shortdesc}

시작하기 전에: [계정에 로그인하십시오. 해당되는 경우, 적절한 리소스 그룹을 대상으로 지정하십시오. 클러스터의 컨텍스트를 설정하십시오.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  클러스터의 작업자 풀을 나열합니다.
    ```
    ibmcloud ks worker-pools --cluster <cluster_name_or_ID>
    ```
    {: pre}
2.  `key=value` 레이블로 작업자 풀에 레이블을 지정하려면 [PATCH 작업자 풀 API ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/PatchWorkerPool)를 사용하십시오. 다음 JSON 예와 같이 요청의 본문을 형식화하십시오.
    ```
    {
      "labels": {"key":"value"},
      "state": "labels"
    }
    ```
    {: codeblock}
3.  작업자 풀 및 작업자 노드에 지정한 `key=value` 레이블이 있는지 확인하십시오.
    *   작업자 풀을 확인하려면 다음을 수행하십시오.
        ```
        ibmcloud ks worker-pool-get --cluster <cluster_name_or_ID> --worker-pool <worker_pool_name_or_ID>
        ```
        {: pre}
    *   작업자 노드를 확인하려면 다음을 수행하십시오. 
        1.  작업자 풀의 작업자 노드를 나열하고 **개인용 IP**를 기록해 두십시오.
            ```
        ibmcloud ks workers --cluster <cluster_name_or_ID> --worker-pool <worker_pool_name_or_ID>
            ```
            {: pre}
        2.  출력의 **레이블** 필드를 검토하십시오.
            ```
    kubectl describe node <worker_node_private_IP>
            ```
            {: pre}

작업자 풀에 레이블을 지정한 후, 워크로드가 작업자 노드에서만 실행되도록 [앱 배치의 레이블](/docs/containers?topic=containers-app#label)을 사용하거나 이 작업자 노드에 배치가 실행되지 않도록 [오염 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/)을 사용할 수 있습니다.

<br />


## 작업자 노드에 대한 자동 복구
{: #planning_autorecovery}

정상적인 Kubernetes 작업자 노드를 보유하려면 중요 컴포넌트(예: `containerd`, `kubelet`, `kube-proxy` 및 `calico`)가 작동해야 합니다. 시간 경과에 따라 이러한 컴포넌트는 중단될 수 있고 작업자 노드가 작동되지 않을 수 있습니다. 작동되지 않는 작업자 노드에서는 클러스터의 총 용량이 줄어들고 앱의 작동이 중단될 수 있습니다.
{:shortdesc}

[작업자 노드에 대한 상태 검사를 구성하고 자동 복구를 사용으로 설정](/docs/containers?topic=containers-health#autorecovery)할 수 있습니다. 자동 복구는 구성된 검사에 따라 비정상적인 작업자 노드를 발견하면 작업자 노드에서 OS 다시 로드와 같은 정정 조치를 트리거합니다. 자동 복구 작동 방식에 대한 자세한 정보는 [자동 복구 블로그 게시물![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/blogs/bluemix/2017/12/autorecovery-utilizes-consistent-hashing-high-availability/)을 참조하십시오.
