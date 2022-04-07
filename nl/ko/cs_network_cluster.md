---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-06"

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


# 서비스 엔드포인트 또는 VLAN 연결 변경
{: #cs_network_cluster}

[클러스터를 작성](/docs/containers?topic=containers-clusters)할 때 처음 네트워크를 설정한 후에는 Kubernetes 마스터가 액세스할 수 있는 서비스 엔드포인트를 변경하거나 작업자 노드의 VLAN 연결을 변경할 수 있습니다.
{: shortdesc}

## 개인 서비스 엔드포인트 설정
{: #set-up-private-se}

Kubernetes 버전 1.11 이상을 실행하는 클러스터의 경우, 클러스터의 개인 서비스 엔드포인트를 사용 안함으로 설정합니다.
{: shortdesc}

개인 서비스 엔드포인트는 Kubernetes 마스터를 개인용으로 액세스할 수 있게 합니다. 작업자 노드와 권한이 부여된 클러스터 사용자는 사설 네트워크를 통해 Kubernetes 마스터와 통신할 수 있습니다. 개인 서비스 엔드포인트를 사용으로 설정할 수 있는지 판별하려면 [작업자 대 마스터 및 사용자 대 마스터 통신](/docs/containers?topic=containers-plan_clusters#internet-facing)을 참조하십시오. 개인 서비스 엔드포인트를 사용으로 설정한 후에는 사용 안함으로 설정할 수 없습니다.

계정에서 [VRF](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) 및 [서비스 엔드포인트](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started)를 사용으로 설정하기 전에 개인 서비스 엔드포인트만 사용하여 클러스터를 작성하셨습니까? 지원 케이스가 처리되어 계정이 업데이트되기 전까지 클러스터를 사용할 수 있도록 [공용 서비스 엔드포인트를 설정](#set-up-public-se)해 보십시오.
{: tip}

1. IBM Cloud infrastructure (SoftLayer) 계정에서 [VRF](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud)를 사용으로 설정하십시오.
2. [{{site.data.keyword.Bluemix_notm}} 계정에서 서비스 엔드포인트를 사용하도록 설정](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started)하십시오.
3. 개인 서비스 엔드포인트를 사용으로 설정하십시오.
   ```
  ibmcloud ks cluster-feature-enable private-service-endpoint --cluster <cluster_name_or_ID>
   ```
   {: pre}
4. 개인 서비스 엔드포인트를 사용하려면 Kubernetes 마스터 API 서버를 새로 고치십시오. CLI의 프롬프트에 따라 수동으로 다음 명령을 실행할 수 있습니다.
   ```
        ibmcloud ks apiserver-refresh --cluster <cluster_name_or_ID>
   ```
   {: pre}

5. [configmap을 작성](/docs/containers?topic=containers-update#worker-up-configmap)하여 클러스터에서 한 번에 사용할 수 없는 최대 작업자 노드 수를 제어하십시오. 작업자 노드를 업데이트할 때 configmap을 사용하면 앱이 사용 가능한 작업자 노드에 순서대로 다시 스케줄되므로 앱의 작동중단시간을 방지할 수 있습니다.
6. 클러스터에서 모든 작업자 노드를 업데이트하여 개인 서비스 엔드포인트 구성을 선택하십시오.

   <p class="important">업데이트 명령을 실행하면 작업자 노드가 다시 로드되어 서비스 엔드포인트 구성을 선택합니다. 작업자 업데이트를 사용할 수 없으면 [작업자 노드를 수동으로 다시 로드](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli)해야 합니다. 다시 로드하는 경우, 한 번에 사용할 수 없는 최대 작업자 노드 수를 제어하려면 순서를 유출, 드레인 및 관리해야 합니다.</p>
   ```
ibmcloud ks worker-update --cluster <cluster_name_or_ID> --workers <worker1,worker2>
   ```
   {: pre}

8. 클러스터가 방화벽으로 보호되는 환경에 있는 경우:
  * [권한이 부여된 클러스터 사용자가 개인 서비스 엔드포인트를 통해 마스터에 액세스할 수 있도록 `kubectl` 명령을 실행하도록 하십시오.](/docs/containers?topic=containers-firewall#firewall_kubectl)
  * 사용하려는 인프라 리소스와 {{site.data.keyword.Bluemix_notm}} 서비스에 대해 [사설 IP에 대한 아웃바운드 네트워크 트래픽을 허용](/docs/containers?topic=containers-firewall#firewall_outbound)하십시오.

9. 선택사항: 개인 서비스 엔드포인트만 사용하려면 공용 서비스 엔드포인트를 사용 안함으로 설정하십시오.
   ```
  ibmcloud ks cluster-feature-disable public-service-endpoint --cluster <cluster_name_or_ID>
   ```
   {: pre}

<br />


## 공용 서비스 엔드포인트 설정
{: #set-up-public-se}

클러스터에 대한 공용 서비스 엔드포인트를 사용 또는 사용 안함으로 설정합니다.
{: shortdesc}

공용 서비스 엔드포인트는 Kubernetes 마스터에 공용으로 액세스할 수 있게 합니다. 작업자 노드와 권한이 부여된 클러스터 사용자는 공용 네트워크를 통해 Kubernetes 마스터와 안전하게 통신할 수 있습니다. 자세한 정보는 [작업자 대 마스터 및 사용자 대 마스터 통신](/docs/containers?topic=containers-plan_clusters#internet-facing)을 참조하십시오. 

**사용 설정 단계**</br>
이전에 공용 엔드포인트를 사용 안함으로 설정한 경우에는 이를 다시 사용으로 설정할 수 있습니다. 
1. 공용 서비스 엔드포인트를 사용으로 설정하십시오.
   ```
  ibmcloud ks cluster-feature-enable public-service-endpoint --cluster <cluster_name_or_ID>
   ```
   {: pre}
2. 공용 서비스 엔드포인트를 사용하려면 Kubernetes 마스터 API 서버를 새로 고치십시오. CLI의 프롬프트에 따라 수동으로 다음 명령을 실행할 수 있습니다.
   ```
        ibmcloud ks apiserver-refresh --cluster <cluster_name_or_ID>
   ```
   {: pre}

   </br>

**사용 안함으로 설정하기 위한 단계**</br>
공용 서비스 엔드포인트를 사용 안함으로 설정하려면 먼저 작업자 노드가 Kubernetes 마스터와 통신할 수 있도록 개인 서비스 엔드포인트를 사용으로 설정해야 합니다.
1. 개인 서비스 엔드포인트를 사용으로 설정하십시오.
   ```
  ibmcloud ks cluster-feature-enable private-service-endpoint --cluster <cluster_name_or_ID>
   ```
   {: pre}
2. CLI 프롬프트를 따르거나 다음 명령을 수동으로 실행하여 개인 서비스 엔드포인트를 사용하도록 Kubernetes 마스터 API 서버를 새로 고치십시오.
   ```
        ibmcloud ks apiserver-refresh --cluster <cluster_name_or_ID>
   ```
   {: pre}
3. [configmap을 작성](/docs/containers?topic=containers-update#worker-up-configmap)하여 클러스터에서 한 번에 사용할 수 없는 최대 작업자 노드 수를 제어하십시오. 작업자 노드를 업데이트할 때 configmap을 사용하면 앱이 사용 가능한 작업자 노드에 순서대로 다시 스케줄되므로 앱의 작동중단시간을 방지할 수 있습니다.

4. 클러스터에서 모든 작업자 노드를 업데이트하여 개인 서비스 엔드포인트 구성을 선택하십시오.

   <p class="important">업데이트 명령을 실행하면 작업자 노드가 다시 로드되어 서비스 엔드포인트 구성을 선택합니다. 작업자 업데이트를 사용할 수 없으면 [작업자 노드를 수동으로 다시 로드](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli)해야 합니다. 다시 로드하는 경우, 한 번에 사용할 수 없는 최대 작업자 노드 수를 제어하려면 순서를 유출, 드레인 및 관리해야 합니다.</p>
   ```
ibmcloud ks worker-update --cluster <cluster_name_or_ID> --workers <worker1,worker2>
   ```
  {: pre}
5. 공용 서비스 엔드포인트를 사용 안함으로 설정하십시오.
   ```
  ibmcloud ks cluster-feature-disable public-service-endpoint --cluster <cluster_name_or_ID>
   ```
   {: pre}

## 공용 서비스 엔드포인트에서 개인 서비스 엔드포인트로 전환
{: #migrate-to-private-se}

Kubernetes 버전 1.11 이상을 실행하는 클러스터에서 작업자 노드는 개인 서비스 엔드포인트를 사용으로 설정하여 공용 네트워크 대신 사설 네트워크를 통해 마스터와 통신할 수 있도록 합니다.
{: shortdesc}

공용 및 사설 VLAN에 연결된 모든 클러스터는 기본적으로 공용 서비스 엔드포인트를 사용합니다. 작업자 노드와 권한이 부여된 클러스터 사용자는 공용 네트워크를 통해 Kubernetes 마스터와 안전하게 통신할 수 있습니다. 공용 네트워크 대신 사설 네트워크를 통해 Kubernetes 마스터와 통신할 작업자 노드를 사용하기 위해 개인 서비스 엔드포인트를 사용으로 설정할 수 있습니다. 그런 다음, 선택적으로 공용 서비스 엔드포인트를 사용 안함으로 설정할 수 있습니다.
* 개인 서비스 엔드포인트를 사용으로 설정하고 공용 서비스 엔드포인트를 유지하는 경우 작업자는 언제나 사설 네트워크를 통해 마스터와 통신하지만 사용자는 공용 또는 사설 네트워크를 통해 마스터와 통신할 수 있습니다.
* 개인 서비스 엔드포인트를 사용으로 설정하고 공용 서비스 엔드포인트를 사용 안함으로 설정한 경우 작업자와 사용자는 사설 네트워크를 통해 마스터와 통신해야 합니다.

개인 서비스 엔드포인트를 사용으로 설정한 후에는 사용 안함으로 설정할 수 없습니다.

1. IBM Cloud infrastructure (SoftLayer) 계정에서 [VRF](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud)를 사용으로 설정하십시오.
2. [{{site.data.keyword.Bluemix_notm}} 계정에서 서비스 엔드포인트를 사용하도록 설정](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started)하십시오.
3. 개인 서비스 엔드포인트를 사용으로 설정하십시오.
   ```
  ibmcloud ks cluster-feature-enable private-service-endpoint --cluster <cluster_name_or_ID>
   ```
   {: pre}
4. CLI 프롬프트를 따르거나 다음 명령을 수동으로 실행하여 개인 서비스 엔드포인트를 사용하도록 Kubernetes 마스터 API 서버를 새로 고치십시오.
   ```
        ibmcloud ks apiserver-refresh --cluster <cluster_name_or_ID>
   ```
   {: pre}
5. [configmap을 작성](/docs/containers?topic=containers-update#worker-up-configmap)하여 클러스터에서 한 번에 사용할 수 없는 최대 작업자 노드 수를 제어하십시오. 작업자 노드를 업데이트할 때 configmap을 사용하면 앱이 사용 가능한 작업자 노드에 순서대로 다시 스케줄되므로 앱의 작동중단시간을 방지할 수 있습니다.

6.  클러스터에서 모든 작업자 노드를 업데이트하여 개인 서비스 엔드포인트 구성을 선택하십시오.

    <p class="important">업데이트 명령을 실행하면 작업자 노드가 다시 로드되어 서비스 엔드포인트 구성을 선택합니다. 작업자 업데이트를 사용할 수 없으면 [작업자 노드를 수동으로 다시 로드](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli)해야 합니다. 다시 로드하는 경우, 한 번에 사용할 수 없는 최대 작업자 노드 수를 제어하려면 순서를 유출, 드레인 및 관리해야 합니다.</p>
    ```
    ibmcloud ks worker-update --cluster <cluster_name_or_ID> --workers <worker1,worker2>
    ```
    {: pre}

7. 선택사항: 공용 서비스 엔드포인트를 사용 안함으로 설정하십시오.
   ```
  ibmcloud ks cluster-feature-disable public-service-endpoint --cluster <cluster_name_or_ID>
   ```
   {: pre}

<br />


## 작업자 노드 VLAN 연결 변경
{: #change-vlans}

클러스터를 작성할 때 작업자 노드를 사설 및 공용 VLAN에 연결할지 또는 사설 VLAN에만 연결할지 선택합니다. 작업자 노드는 풀에서 향후의 작업자 노드를 프로비저닝하는 데 사용할 VLAN을 포함하는 네트워킹 메타데이터를 저장하는 작업자 풀의 파트입니다. 다음과 같은 경우에 클러스터의 VLAN 연결 설정을 나중에 변경할 수 있습니다.
{: shortdesc}

* 구역의 작업자 풀 VLAN이 용량이 부족하여 클러스터 작업자 노드가 사용할 새 VLAN을 프로비저닝해야 합니다.
* 공용 및 사설 VLAN 둘 다에 있는 작업자 노드가 포함된 클러스터가 있지만 [사설 전용 클러스터](/docs/containers?topic=containers-plan_clusters#private_clusters)로 변경하려고 합니다.
* 사설 전용 클러스터가 있지만 공용 VLAN에 있는 [에지 노드](/docs/containers?topic=containers-edge#edge)의 작업자 풀과 같은 일부 작업자 노드에서 인터넷에 앱을 노출시키려고 합니다.

대신 마스터-작업자 통신을 위해 서비스 엔드포인트를 변경하시겠습니까? [공용](#set-up-public-se) 및 [개인](#set-up-private-se) 서비스 엔드포인트를 설정하기 위한 주제를 확인하십시오.
{: tip}

시작하기 전에:
* [계정에 로그인하십시오. 해당되는 경우, 적절한 리소스 그룹을 대상으로 지정하십시오. 클러스터의 컨텍스트를 설정하십시오.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
* 작업자 노드가 독립형(작업자 풀의 파트가 아님)인 경우, [작업자 노드를 작업자 풀에 업데이트](/docs/containers?topic=containers-update#standalone_to_workerpool)하십시오.

작업자 풀이 작업자 노드를 프로비저닝하는 데 사용하는 VLAN을 변경하려면 다음을 수행하십시오.

1. 클러스터에 있는 작업자 풀의 이름을 나열하십시오.
  ```
   ibmcloud ks worker-pools --cluster <cluster_name_or_ID>
  ```
  {: pre}

2. 작업자 풀 중 하나에 대한 구역을 판별하십시오. 출력에서 **구역** 필드를 찾으십시오.
  ```
  ibmcloud ks worker-pool-get --cluster <cluster_name_or_ID> --worker-pool <pool_name>
  ```
  {: pre}

3. 이전 단계에서 찾은 각 구역에 대해 서로 호환되는 사용 가능한 공용 및 사설 VLAN을 가져오십시오.

  1. 출력에서 **유형** 아래에 나열된 사용 가능한 공용 및 사설 VLAN을 확인하십시오.
     ```
        ibmcloud ks vlans --zone <zone>
     ```
     {: pre}

  2. 구역의 공용 및 사설 VLAN이 호환 가능한지 확인하십시오. 호환 가능하려면 **라우터**의 팟(Pod) ID가 동일해야 합니다. 이 예제 출력에서 **Router** 팟(Pod) ID가 일치합니다(`01a` 및 `01a`). 하나의 팟(Pod) ID가 `01a`이고 다른 팟(Pod) ID가 `02a`인 경우에는 작업자 풀에 대해 이러한 공용 및 사설 VLAN ID를 설정할 수 없습니다.
     ```
    ID        Name   Number   Type      Router         Supports Virtual Workers
    229xxxx          1234     private   bcr01a.dal12   true
    229xxxx          5678     public    fcr01a.dal12   true
     ```
     {: screen}

  3. 구역에 대한 새 공용 또는 사설 VLAN을 주문해야 하는 경우 [{{site.data.keyword.Bluemix_notm}} 콘솔](/docs/infrastructure/vlans?topic=vlans-ordering-premium-vlans#ordering-premium-vlans)에서 주문하거나 다음 명령을 사용할 수 있습니다. 이전 단계에서와 마찬가지로 **라우터** 팟(Pod) ID와 일치하며 VLAN이 호환 가능해야 합니다. 한 쌍의 새 공용 및 사설 VLAN을 작성하는 경우, 서로 호환 가능해야 합니다.
     ```
    ibmcloud sl vlan create -t [public|private] -d <zone> -r <compatible_router>
     ```
     {: pre}

  4. 호환 가능한 VLAN의 ID를 기록해 두십시오.

4. 각 구역에 대한 새 VLAN 네트워크 메타데이터를 사용하여 작업자 풀을 설정하십시오. 새 작업자 풀을 작성하거나 기존 작업자 풀을 수정할 수 있습니다.

  * **새 작업자 풀 작성**: [새 작업자 풀을 작성하여 작업자 노드 추가](/docs/containers?topic=containers-add_workers#add_pool)를 참조하십시오.

  * **기존 작업자 풀 수정**: 작업자 풀의 네트워크 메타데이터를 설정하여 각 구역에 대한 VLAN을 사용하십시오. 풀에서 이미 작성된 작업자 노드는 계속해서 이전 VLAN을 사용하지만, 풀의 새 작업자 노드는 설정한 새 VLAN 메타데이터를 사용합니다.

    * 사설 전용에서 사설 및 공용 모두로 변경하는 경우와 같이 공용 및 사설 VLAN 모두를 추가하는 예제는 다음과 같습니다.
      ```
      ibmcloud ks zone-network-set --zone <zone> --cluster <cluster_name_or_ID> --worker-pools <pool_name> --private-vlan <private_vlan_id> --public-vlan <public_vlan_id>
      ```
      {: pre}

    * [서비스 엔드포인트를 사용하는 VRF가 사용으로 설정된 계정](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started)이 있을 때 공용 및 사설 VLAN에서 사설 전용으로 변경하는 경우와 같이 사설 VLAN만 추가하는 예제는 다음과 같습니다.
      ```
      ibmcloud ks zone-network-set --zone <zone> --cluster <cluster_name_or_ID> --worker-pools <pool_name> --private-vlan <private_vlan_id> --public-vlan <public_vlan_id>
      ```
      {: pre}

5. 풀의 크기를 다시 조정하여 작업자 풀에 작업자 노드를 추가하십시오.
   ```
    ibmcloud ks worker-pool-resize --cluster <cluster_name_or_ID> --worker-pool <pool_name>  --size-per-zone <number_of_workers_per_zone>
   ```
   {: pre}

   이전 네트워크 메타데이터를 사용하는 작업자 노드를 제거하려면 구역 당 작업자 수를 변경하여 구역 당 작업자 수를 이전의 두 배로 늘리십시오. 이 단계의 후반부에서 이전 작업자 노드를 유출, 드레인 및 제거할 수 있습니다.
  {: tip}

6. 출력에서 새 작업자 노드가 적절한 **공인 IP** 및 **사설 IP**로 작성되었는지 확인하십시오. 예를 들어, 작업자 풀을 공용 및 사설 VLAN에서 사설 전용으로 변경하면 새 작업자 노드에는 사설 IP만 있습니다. 작업자 풀을 사설 전용에서 공용 및 사설 VLAN 모두로 변경하면 새 작업자 노드에는 공용 및 사설 IP 모두 있습니다.
   ```
   ibmcloud ks workers --cluster <cluster_name_or_ID> --worker-pool <pool_name>
   ```
   {: pre}

7. 선택사항: 작업자 풀에서 이전 네트워크 메타데이터를 사용하는 작업자 노드를 제거하십시오.
  1. 이전 단계의 출력에서 작업자 풀에서 제거할 작업자 노드의 **ID** 및 **사설 IP**를 기록해 두십시오.
  2. 유출(cordoning)이라고 알려진 프로세스에서 작업자 노드를 스케줄 불가능으로 표시하십시오. 작업자 노드를 유출할 때 이후 팟(Pod) 스케줄링에서 사용할 수 없도록 합니다.
     ```
    kubectl cordon <worker_private_ip>
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
    kubectl drain <worker_private_ip>
     ```
     {: pre}
이 프로세스에는 몇 분 정도 소요될 수 있습니다.
  5. 작업자 노드를 제거하십시오. 이전에 검색한 작업자 ID를 사용하십시오.
     ```
   ibmcloud ks worker-rm --cluster <cluster_name_or_ID> --worker <worker_name_or_ID>
     ```
     {: pre}
  6. 작업자 노드가 제거되었는지 확인하십시오.
     ```
        ibmcloud ks workers --cluster <cluster_name_or_ID> --worker-pool <pool_name>
     ```
     {: pre}

8. 선택사항: 클러스터의 각 작업자 풀에 대해 2-7단계를 반복할 수 있습니다. 이러한 단계를 완료하면 클러스터의 모든 작업자 노드가 새 VLAN으로 설정됩니다.

9. 클러스터의 기본 ALB는 해당 IP 주소가 이전 VLAN의 서브넷에 속하므로 여전히 이전 VLAN에 바인드되어 있습니다. ALB를 VLAN 간에 이동할 수는 없으므로, 사용자는 대신 [새 VLAN에 ALB를 작성하고 이전 VLAN에서 ALB를 사용 안함으로 설정](/docs/containers?topic=containers-ingress#migrate-alb-vlan)할 수 있습니다. 
