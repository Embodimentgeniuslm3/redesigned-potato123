---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-06"

keywords: kubernetes, iks, ImagePullBackOff, registry, image, failed to pull image,

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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}


# 클러스터 및 작업자 노드 문제점 해결
{: #cs_troubleshoot_clusters}

{{site.data.keyword.containerlong}}를 사용할 때 클러스터 및 작업자 노드 관련 문제점을 해결하려면 이러한 기술을 고려하십시오.
{: shortdesc}

<p class="tip">더 일반적인 문제점이 있는 경우에는 [클러스터 디버깅](/docs/containers?topic=containers-cs_troubleshoot)을 시도해 보십시오.<br>또한 문제점을 해결하는 중에 [{{site.data.keyword.containerlong_notm}} 진단 및 디버그 도구](/docs/containers?topic=containers-cs_troubleshoot#debug_utility)를 사용하여 테스트를 수행하고 클러스터에서 관련 정보를 수집할 수 있습니다.</p>

## 권한 오류로 인해 클러스터를 작성하거나 작업자 노드를 관리할 수 없음
{: #cs_credentials}

{: tsSymptoms}
다음 명령 중 하나를 실행하여 새 클러스터 또는 기존 클러스터에 대한 작업자 노드를 관리하려고 합니다.
* 작업자 프로비저닝: `ibmcloud ks cluster-create`, `ibmcloud ks worker-pool-rebalance` 또는 `ibmcloud ks worker-pool-resize`
* 작업자 다시 로드: `ibmcloud ks worker-reload` 또는 `ibmcloud ks worker-update`
* 작업자 다시 부팅: `ibmcloud ks worker-reboot`
* 작업자 삭제: `ibmcloud ks cluster-rm`, `ibmcloud ks worker-rm`, `ibmcloud ks worker-pool-rebalance` 또는 `ibmcloud ks worker-pool-resize`

그러나 다음 중 하나와 유사한 오류 메시지가 수신됩니다.

```
We were unable to connect to your IBM Cloud infrastructure (SoftLayer) account.
Creating a standard cluster requires that you have either a Pay-As-You-Go account
that is linked to an IBM Cloud infrastructure (SoftLayer) account term or that you have used the {{site.data.keyword.containerlong_notm}} CLI to set your {{site.data.keyword.Bluemix_notm}} Infrastructure API keys.
```
{: screen}

```
{{site.data.keyword.Bluemix_notm}} Infrastructure Exception:
'Item' must be ordered with permission.
```
{: screen}

```
Worker not found. Review {{site.data.keyword.Bluemix_notm}} infrastructure permissions.
```
{: screen}

```
{{site.data.keyword.Bluemix_notm}} Infrastructure Exception:
The user does not have the necessary {{site.data.keyword.Bluemix_notm}}
Infrastructure permissions to add servers
```
{: screen}

```
IAM token exchange request failed: Cannot create IMS portal token, as no IMS account is linked to the selected BSS account
```
{: screen}

```
The cluster could not be configured with the registry. Make sure that you have the Administrator role for {{site.data.keyword.registrylong_notm}}.
```
{: screen}

{: tsCauses}
지역 및 리소스 그룹에 대해 설정된 인프라 인증 정보에는 적절한 [인프라 권한](/docs/containers?topic=containers-access_reference#infra)이 없습니다. 사용자의 인프라 권한은 일반적으로 지역 및 리소스 그룹에 대한 [API 키](/docs/containers?topic=containers-users#api_key)로 저장됩니다. 보다 드물게, [다른 {{site.data.keyword.Bluemix_notm}} 계정 유형](/docs/containers?topic=containers-users#understand_infra)을 사용하는 경우 [수동으로 인프라 인증 정보를 설정](/docs/containers?topic=containers-users#credentials)했을 수 있습니다. 인프라 리소스를 프로비저닝하는 데 다른 IBM Cloud 인프라(SoftLayer) 계정을 사용하는 경우에는 계정에 [고아 클러스터](#orphaned)가 있을 수 있습니다.

{: tsResolve}
계정 소유자는 인프라 계정 인증 정보를 올바르게 설정해야 합니다. 인증 정보는 사용 중인 인프라 계정의 유형에 따라 다릅니다.

시작하기 전에, [계정에 로그인하십시오. 해당되는 경우, 적절한 리소스 그룹을 대상으로 지정하십시오. 클러스터의 컨텍스트를 설정하십시오.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  지역 및 리소스 그룹의 인프라 권한에 사용되는 사용자 인증 정보를 식별하십시오.
    1.  클러스터의 지역 및 리소스 그룹에 대한 API 키를 확인하십시오.
        ```
        ibmcloud ks api-key-info --cluster <cluster_name_or_ID>
        ```
        {: pre}

        출력 예:
        ```
        Getting information about the API key owner for cluster <cluster_name>...
        OK
        Name                Email   
        <user_name>         <name@email.com>
        ```
        {: screen}
    2.  지역 및 리소스 그룹에 대한 인프라 계정이 다른 IBM Cloud 인프라(SoftLayer) 계정을 사용하도록 수동으로 설정되었는지 확인하십시오.
        ```
        ibmcloud ks credential-get --region <us-south>
        ```
        {: pre}

        **인증 정보가 다른 계정을 사용하도록 설정된 경우의 출력 예**입니다. 이 경우, 다른 사용자의 인증 정보가 이전 단계에서 검색한 API 키에 저장되더라도 사용자의 인프라 인증 정보가 대상으로 지정한 지역 및 리소스 그룹에 사용됩니다.
        ```
        OK
        Infrastructure credentials for user name <1234567_name@email.com> set for resource group <resource_group_name>.
        ```
        {: screen}

        **인증 정보가 다른 계정을 사용하도록 설정되지 않은 경우의 출력 예**입니다. 이 경우, 이전 단계에서 검색한 API 키 소유자는 지역 및 리소스 그룹에 사용된 인프라 인증 정보를 보유하고 있습니다.
        ```
        FAILED
        No credentials set for resource group <resource_group_name>.: The user credentials could not be found. (E0051)
        ```
        {: screen}
2.  사용자가 보유하고 있는 인프라 권한을 유효성 검증하십시오.
    1.  지역 및 리소스 그룹에 대해 제안되고 필요한 인프라 권한을 나열하십시오.
        ```
        ibmcloud ks infra-permissions-get --region <region>
        ```
        {: pre}
    2.  [API 키에 대한 인프라 인증 정보 소유자 또는 수동으로 설정된 계정에 올바른 권한이 있는지](/docs/containers?topic=containers-users#owner_permissions) 확인하십시오.
    3.  필요한 경우 지역 및 리소스 그룹에 대한 [API 키](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_api_key_reset) 또는 [수동으로 설정된](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_set) 인프라 인증 정보 소유자를 변경할 수 있습니다. 
3.  변경된 권한으로 권한이 부여된 사용자가 클러스터에 대한 인프라 오퍼레이션을 수행할 수 있는지 테스트하십시오. 
    1.  예를 들어, 작업자 노드를 삭제하려고 할 수 있습니다.
        ```
        ibmcloud ks worker-rm --cluster <cluster_name_or_ID> --worker <worker_node_ID>
        ```
        {: pre}
    2.  작업자 노드가 제거되었는지 확인하십시오.
        ```
        ibmcloud ks worker-get --cluster <cluster_name_or_ID> --worker <worker_node_ID>
        ```
        {: pre}

        작업자 노드가 제거되었는지 여부에 대한 출력 예입니다. 작업자 노드가 삭제되었으므로 `worker-get` 오퍼레이션이 실패합니다. 인프라 권한이 올바르게 설정되어 있습니다.
        ```
        FAILED
        The specified worker node could not be found. (E0011)
        ```
        {: screen}

    3.  작업자 노드가 제거되지 않은 경우 디버깅을 계속하려면 [**상태(State)** 및 **상태(Status)** 필드](/docs/containers?topic=containers-cs_troubleshoot#debug_worker_nodes)와 [작업자 노드에 대한 일반 문제](/docs/containers?topic=containers-cs_troubleshoot#common_worker_nodes_issues)를 검토하십시오. 
    4.  수동으로 인증 정보를 설정하고 계속해서 인프라 계정에 클러스터의 작업자 노드가 표시되지 않는 경우에는 [클러스터가 고아 상태가 되었는지](#orphaned) 확인해야 합니다.

<br />


## 방화벽으로 인해 CLI 명령을 실행할 수 없음
{: #ts_firewall_clis}

{: tsSymptoms}
CLI에서 `ibmcloud`, `kubectl` 또는 `calicoctl` 명령을 실행하면 실패합니다.

{: tsCauses}
로컬 시스템에서 프록시 또는 방화벽을 통해 공용 엔드포인트에 액세스하지 못하도록 방지하는 회사 네트워크 정책이 있을 수 있습니다.

{: tsResolve}
[CLI 명령에 대한 TCP 액세스가 작동하도록 허용](/docs/containers?topic=containers-firewall#firewall_bx)하십시오. 이 태스크에서는 클러스터에 대한 [**관리자** {{site.data.keyword.Bluemix_notm}} IAM 플랫폼 역할](/docs/containers?topic=containers-users#platform)이 필요합니다.


## 내 클러스터의 리소스에 액세스할 수 없음
{: #cs_firewall}

{: tsSymptoms}
클러스터의 작업자 노드가 사설 네트워크에서 통신할 수 없는 경우 다양한 증상이 나타날 수 있습니다.

- `kubectl exec`, `attach`, `logs`, `proxy` 또는 `port-forward`를 실행하는 경우 샘플 오류 메시지:
  ```
  Error from server: error dialing backend: dial tcp XXX.XXX.XXX:10250: getsockopt: connection timed out
  ```
  {: screen}

- `kubectl proxy`가 성공하지만 Kubernetes 대시보드를 사용할 수 없는 경우의 샘플 오류 메시지:
  ```
  timeout on 172.xxx.xxx.xxx
  ```
  {: screen}

- `kubectl proxy`가 실패하거나 서비스에 대한 연결이 실패한 경우의 샘플 오류 메시지:
  ```
  Connection refused
  ```
  {: screen}

  ```
  Connection timed out
  ```
  {: screen}

  ```
  Unable to connect to the server: net/http: TLS handshake timeout
  ```
  {: screen}


{: tsCauses}
클러스터의 리소스에 액세스하려면 작업자 노드가 사설 네트워크에서 통신할 수 있어야 합니다. IBM Cloud 인프라(SoftLayer) 계정에 Vyatta 또는 다른 방화벽 설정이 있거나 기존 방화벽 설정을 사용자 정의했을 수 있습니다. {{site.data.keyword.containerlong_notm}}에서는 작업자 노드와 Kubernetes 마스터 간의 양방향 통신을 허용하기 위해 특정 IP 주소와 포트를 열도록 요구합니다. 작업자 노드가 여러 개의 구역에 분산되어 있는 경우, VLAN Spanning을 사용하여 사설 네트워크 통신을 허용해야 합니다. 작업자 노드가 다시 로드 루프에서 고착 상태에 빠진 경우에는 작업자 노드 사이의 통신도 불가능할 수 있습니다.

{: tsResolve}
1. 클러스터의 작업자 노드를 나열하고 작업자 노드가 오랫동안 `Reloading` 상태가 아닌지 확인하십시오.
   ```
   ibmcloud ks workers --cluster <cluster_name_or_id>
   ```
   {: pre}

2. 다중 구역 클러스터가 있고 사용자의 계정이 VRF에 대해 사용으로 설정되지 않은 경우 사용자 계정에 대해 [VLAN spanning을 사용으로 설정](/docs/containers?topic=containers-subnets#subnet-routing)했는지 확인하십시오.
3. Vyatta 또는 사용자 정의 방화벽 설정이 있는 경우 클러스터가 인프라 리소스 및 서비스에 액세스할 수 있도록 [필수 포트를 열었는지](/docs/containers?topic=containers-firewall#firewall_outbound) 확인하십시오.

<br />



## 클러스터를 볼 수 없거나 이에 대해 작업할 수 없음
{: #cs_cluster_access}

{: tsSymptoms}
* 클러스터를 찾을 수 없습니다. `ibmcloud ks clusters`를 실행했을 때 해당 클러스터가 출력에 나열되지 않습니다.
* 클러스터에 대해 작업을 수행할 수 없습니다. `ibmcloud ks cluster-config` 또는 클러스터 고유 명령을 실행했을 때 해당 클러스터를 찾을 수 없습니다.


{: tsCauses}
{{site.data.keyword.Bluemix_notm}}에서 각 리소스는 리소스 그룹에 속해야 합니다. 예를 들면, 클러스터 `mycluster`는 `default` 리소스 그룹에 속해 있을 수 있습니다. 계정 소유자가 {{site.data.keyword.Bluemix_notm}} IAM 플랫폼 역할을 지정하여 사용자에게 리소스에 대한 액세스 권한을 부여하는 경우, 이 액세스 권한은 특정 리소스에 대한 또는 리소스 그룹에 대한 권한일 수 있습니다. 특정 리소스에 대한 액세스 권한이 부여된 경우에는 리소스 그룹에 대한 액세스 권한이 없습니다. 이 경우에는 액세스 권한이 있는 클러스터에 대해 작업하기 위해 리소스 그룹을 대상으로 지정할 필요가 없습니다. 해당 클러스터가 속한 그룹이 아닌 다른 리소스 그룹을 대상으로 지정하면 해당 클러스터에 대한 조치가 실패할 수 있습니다. 반대로, 리소스 그룹에 대한 액세스 권한의 일부로서 리소스에 대한 액세스 권한을 획득한 경우에는 해당 그룹의 클러스터에 대해 작업하려면 해당 리소스 그룹을 대상으로 지정해야 합니다. CLI 세션의 대상을 해당 클러스터가 속한 리소스 그룹으로 지정하지 않으면 해당 클러스터에 대한 조치가 실패할 수 있습니다.

클러스터를 찾을 수 없거나 이에 대해 작업할 수 없는 경우에는 다음 문제 중 하나가 있는 것입니다.
* 클러스터 및 해당 클러스터가 속한 리소스 그룹에 대한 액세스 권한이 있으나 CLI 세션이 해당 클러스터가 속한 리소스 그룹을 대상으로 지정하고 있지 않습니다.
* 클러스터에 대해서는 액세스 권한이 있으나 해당 클러스터가 속한 리소스 그룹에 대해서는 액세스 권한이 없습니다. CLI 세션이 이 리소스 그룹이나 다른 리소스 그룹을 대상으로 지정하고 있습니다.
* 클러스터에 대한 액세스 권한이 없습니다.

{: tsResolve}
사용자 액세스 권한을 확인하려면 다음을 수행하십시오.

1. 자신의 모든 사용자 권한을 나열하십시오.
    ```
    ibmcloud iam user-policies <your_user_name>
    ```
    {: pre}

2. 클러스터 및 해당 클러스터가 속한 리소스 그룹에 대한 액세스 권한이 있는지 확인하십시오.
    1. **Resource Group Name** 값이 클러스터의 리소스 그룹이고 **Memo** 값이 `Policy applies to the resource group`인 정책을 찾으십시오. 이 정책이 있는 경우에는 리소스 그룹에 대한 액세스 권한이 있는 것입니다. 예를 들면, 다음 정책은 사용자에게 `test-rg` 리소스 그룹에 대한 액세스 권한이 있음을 나타냅니다.
        ```
        Policy ID:   3ec2c069-fc64-4916-af9e-e6f318e2a16c
        Roles:       Viewer
        Resources:
                     Resource Group ID     50c9b81c983e438b8e42b2e8eca04065
                     Resource Group Name   test-rg
                     Memo                  Policy applies to the resource group
        ```
        {: screen}
    2. **Resource Group Name** 값이 클러스터의 리소스 그룹이고, **Service Name** 값이 `containers-kubernetes`이거나 값이 없으며, 그리고 **Memo** 값이 `Policy applies to the resource(s) within the resource group`인 정책을 찾으십시오. 이 정책이 있는 경우에는 해당 리소스 그룹 내의 클러스터 또는 모든 리소스에 대한 액세스 권한이 있는 것입니다. 예를 들면, 다음 정책은 사용자에게 `test-rg` 리소스 그룹의 클러스터에 대한 액세스 권한이 있음을 나타냅니다.
        ```
        Policy ID:   e0ad889d-56ba-416c-89ae-a03f3cd8eeea
        Roles:       Administrator
        Resources:
                     Resource Group ID     a8a12accd63b437bbd6d58fb6a462ca7
                     Resource Group Name   test-rg
                     Service Name          containers-kubernetes
                     Service Instance
                     Region
                     Resource Type
                     Resource
                     Memo                  Policy applies to the resource(s) within the resource group
        ```
        {: screen}
    3. 이러한 정책이 둘 다 있는 경우에는 4단계의 첫 번째 글머리 기호로 건너뛰십시오. 2a단계의 정책은 없으나 2b단계의 정책은 있는 경우에는 4단계의 두 번째 글머리 기호로 건너뛰십시오. 두 정책이 모두 없는 경우에는 3단계로 진행하십시오.

3. 클러스터에 대한 액세스 권한이 있는지, 그리고 이 권한이 해당 클러스터가 속한 리소스 그룹에 대한 액세스 권한의 일부가 아닌지 확인하십시오.
    1. **Policy ID** 및 **Roles** 필드를 제외하고 값이 없는 정책을 찾으십시오. 이 정책이 있는 경우에는 전체 계정에 대한 액세스 권한의 일부로서 해당 클러스터에 대한 액세스 권한이 있는 것입니다. 예를 들면, 다음 정책은 사용자에게 계정 내의 모든 리소스에 대한 액세스 권한이 있음을 나타냅니다.
        ```
        Policy ID:   8898bdfd-d520-49a7-85f8-c0d382c4934e
        Roles:       Administrator, Manager
        Resources:
                     Service Name
                     Service Instance
                     Region
                     Resource Type
                     Resource
        ```
        {: screen}
    2. **Service Name** 값이 `containers-kubernetes`이며 **Service Instance** 값이 클러스터의 ID인 정책을 찾으십시오. `ibmcloud ks cluster-get --cluster <cluster_name>`을 실행하여 클러스터 ID를 찾을 수 있습니다. 예를 들면, 다음 정책은 사용자에게 특정 클러스터에 대한 액세스 권한이 있음을 나타냅니다.
        ```
        Policy ID:   140555ce-93ac-4fb2-b15d-6ad726795d90
        Roles:       Administrator
        Resources:
                     Service Name       containers-kubernetes
                     Service Instance   df253b6025d64944ab99ed63bb4567b6
                     Region
                     Resource Type
                     Resource
        ```
        {: screen}
    3. 두 정책 중 하나가 있는 경우에는 4단계의 두 번째 글머리 기호로 건너뛰십시오. 두 정책이 모두 없는 경우에는 4단계의 세 번째 글머리 기호로 건너뛰십시오.

4. 액세스 정책에 따라 다음 선택사항 중 하나를 선택하십시오.
    * 클러스터 및 해당 클러스터가 속한 리소스 그룹에 대한 액세스 권한이 있는 경우:
      1. 리소스 그룹을 대상으로 지정하십시오. **참고**: 이 리소스 그룹을 대상에서 해제하기 전까지는 다른 리소스 그룹의 클러스터에 대해 작업할 수 없습니다.
          ```
          ibmcloud target -g <resource_group>
          ```
          {: pre}

      2. 클러스터를 대상으로 지정하십시오.
          ```
          ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
          ```
          {: pre}

    * 클러스터에 대해서는 액세스 권한이 있으나 해당 클러스터가 속한 리소스 그룹에 대해서는 액세스 권한이 없는 경우:
      1. 리소스 그룹을 대상으로 지정하지 마십시오. 이미 리소스 그룹을 대상으로 지정한 경우에는 이를 해제하십시오.
        ```
        ibmcloud target --unset-resource-group
        ```
        {: pre}

      2. 클러스터를 대상으로 지정하십시오.
        ```
        ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
        ```
        {: pre}

    * 클러스터에 대한 액세스 권한이 없는 경우:
        1. 계정 소유자에게 해당 클러스터에 대한 [{{site.data.keyword.Bluemix_notm}}IAM 플랫폼 역할](/docs/containers?topic=containers-users#platform)을 지정해 달라고 요청하십시오.
        2. 리소스 그룹을 대상으로 지정하지 마십시오. 이미 리소스 그룹을 대상으로 지정한 경우에는 이를 해제하십시오.
          ```
          ibmcloud target --unset-resource-group
          ```
          {: pre}
        3. 클러스터를 대상으로 지정하십시오.
          ```
          ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
          ```
          {: pre}

<br />


## SSH로 작업자 노드에 액세스하는 데 실패
{: #cs_ssh_worker}

{: tsSymptoms}
SSH 연결을 사용하여 작업자 노드에 액세스할 수 없습니다.

{: tsCauses}
비밀번호에 의한 SSH를 작업자 노드에서 사용할 수 없습니다.

{: tsResolve}
모든 노드에서 실행해야 하는 조치에 대해서는 Kubernetes [`DaemonSet` ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/)을 사용하고, 실행해야 하는 일회성 조치에 대해서는 작업을 사용하십시오.

<br />


## 베어메탈 인스턴스 ID가 작업자 레코드와 일치하지 않음
{: #bm_machine_id}

{: tsSymptoms}
베어메탈 작업자 노드에서 `ibmcloud ks worker` 명령을 사용할 때 다음과 유사한 메시지가 표시됩니다.

```
Instance ID inconsistent with worker records
```
{: screen}

{: tsCauses}
머신에서 하드웨어 문제가 발생하면 머신 ID가 {{site.data.keyword.containerlong_notm}} 작업자 레코드와 일치하지 않게 될 수 있습니다. IBM Cloud 인프라(SoftLayer)에서 이 문제를 해결하는 경우, 서비스가 식별하지 않는 시스템 내에서 컴포넌트가 변경될 수 있습니다.

{: tsResolve}
{{site.data.keyword.containerlong_notm}}가 머신을 다시 식별할 수 있도록 [베어메탈 작업자 노드를 다시 로드](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)하십시오. **참고**: 다시 로드하면 머신의 [패치 버전](/docs/containers?topic=containers-changelog)도 업데이트됩니다.

[베어메탈 작업자 노드를 삭제](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_rm)할 수도 있습니다. **참고**: 베어메탈 인스턴스는 월별로 비용이 청구됩니다.

<br />


## 고아 상태의 클러스터에 있는 인프라를 수정하거나 삭제할 수 없음
{: #orphaned}

{: tsSymptoms}
클러스터에서 다음과 같은 인프라 관련 명령을 수행할 수 없습니다.
* 작업자 노드 추가 또는 제거
* 작업자 노드 다시 로드 또는 다시 부팅
* 작업자 풀 크기 조정
* 클러스터 업데이트

자신의 IBM Cloud 인프라(SoftLayer) 계정에 있는 클러스터 작업자 노드를 볼 수 없습니다. 그러나 계정 내의 다른 클러스터는 업데이트하고 관리할 수 있습니다.

또한 자신에게 [적절한 인프라 인증 정보](#cs_credentials)가 있음을 확인했습니다.

{: tsCauses}
클러스터가 사용자의 {{site.data.keyword.containerlong_notm}} 계정에 더 이상 연결되어 있지 않은 IBM Cloud 인프라(SoftLayer) 계정에 프로비저닝되는 경우가 있습니다. 이 경우 해당 클러스터는 고아 상태가 됩니다. 리소스가 다른 계정에 속해 있으므로 사용자에게 이러한 리소스를 수정하는 데 필요한 인프라 인증 정보가 없습니다.

클러스터가 고아 상태가 될 수 있는 경우에 대해 이해하기 위해서는 다음 시나리오를 고려하십시오.
1.  사용자에게 {{site.data.keyword.Bluemix_notm}} 종량과금제 계정이 있습니다.
2.  사용자가 `Cluster1`이라는 클러스터를 작성합니다. 작업자 노드 및 기타 인프라 리소스가 종량과금제 계정과 함께 제공된 인프라 계정에 프로비저닝됩니다.
3.  사용자는 나중에 사용자의 팀이 레거시 또는 공유 IBM Cloud 인프라(SoftLayer) 계정을 사용함을 발견합니다. 사용자는 `ibmcloud ks credential-set` 명령을 사용하여 팀 계정 사용을 위한 IBM Cloud 인프라(SoftLayer) 인증 정보를 변경합니다.
4.  사용자가 `Cluster2`라는 다른 클러스터를 작성합니다. 작업자 노드 및 기타 인프라 리소스가 팀 인프라 계정에 프로비저닝됩니다.
5.  사용자가 `Cluster1`에 작업자 노드 업데이트 또는 작업자 노드 다시 로드를 수행하려 하거나, 그냥 이를 삭제하여 정리하고자 합니다. 그러나 `Cluster1`은 다른 인프라 계정에 프로비저닝되었으므로 사용자는 해당 인프라 리소스를 수정할 수 없습니다. `Cluster1`이 고아 상태가 된 것입니다.
6.  사용자가 다음 섹션의 해결 단계를 따르지만 인프라 인증 정보를 다시 팀 계정으로 설정하지 않습니다. 사용자는 `Cluster1`를 삭제할 수는 있지만 이제 `Cluster2`가 고아 상태가 됩니다.
7.  사용자가 `Cluster2`를 작성한 팀 계정으로 인프라 인증 정보를 다시 변경합니다. 이제는 고아 클러스터가 없습니다.

<br>

{: tsResolve}
1.  클러스터가 현재 있는 영역이 클러스터를 프로비저닝하는 데 어느 인프라 계정을 사용하는지 확인하십시오.
    1.  [{{site.data.keyword.containerlong_notm}} 클러스터 콘솔 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/kubernetes/clusters)에 로그인하십시오.
    2.  테이블에서 클러스터를 선택하십시오.
    3.  **개요** 탭에서 **인프라 사용자** 필드를 확인하십시오. 이 필드는 {{site.data.keyword.containerlong_notm}} 계정이 기본 외의 인프라 계정을 사용하고 있는지 판별하는 데 도움을 줍니다.
        * **인프라 사용자** 필드가 나타나지 않으면 사용자가 인프라 및 플랫폼 계정에 대해 동일한 인증 정보를 사용하는 연결된 종량과금제 계정을 보유 중입니다. 수정할 수 없는 클러스터는 다른 인프라 계정에 프로비저닝되었을 수 있습니다.
        * **인프라 사용자** 필드가 표시된 경우에는 사용자가 종량과금제 계정과 함께 제공된 것과 다른 인프라 계정을 사용하는 것입니다. 이러한 서로 다른 인증 정보는 지역 내의 모든 클러스터에 적용됩니다. 수정할 수 없는 클러스터는 사용자의 종량과금제 계정 또는 다른 인프라 계정에 프로비저닝되었을 수 있습니다.
2.  클러스터를 프로비저닝하는 데 어느 인프라 계정이 사용되었는지 확인하십시오.
    1.  **작업자 노드** 탭에서 작업자 노드를 선택하고 해당 **ID**를 기록하십시오.
    2.  메뉴 ![메뉴 아이콘](../icons/icon_hamburger.svg "메뉴 아이콘")를 열고 **클래식 인프라**를 클릭하십시오.
    3.  인프라 탐색 분할창에서 **디바이스 > 디바이스 목록**을 클릭하십시오.
    4.  이전에 기록한 작업자 노드 ID를 검색하십시오.
    5.  해당 작업자 노드 ID를 찾을 수 없는 경우에는 이 작업자 노드가 이 인프라 계정에 프로비저닝되지 않은 것입니다. 다른 인프라 계정으로 전환하고 다시 시도하십시오.
3.  `ibmcloud ks credential-set` [명령](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_set)을 사용하여 이전 단계에서 찾은, 클러스터 작업자 노드가 프로비저닝된 계정으로 인프라 인증 정보를 변경하십시오.
    인프라 인증 정보에 대한 액세스 권한을 더 이상 보유하지 않으며 이를 가져올 수 없는 경우에는 {{site.data.keyword.Bluemix_notm}} 지원 케이스를 열어서 고아 클러스터를 제거해야 합니다.
    {: note}
4.  [클러스터를 삭제](/docs/containers?topic=containers-remove)하십시오.
5.  원하는 경우에는 인프라 인증 정보를 이전 계정으로 재설정하십시오. 전환한 계정과 다른 인프라 계정으로 클러스터를 작성한 경우에는 이러한 클러스터가 고아 상태가 될 수 있다는 점을 유의하십시오.
    * 인증 정보를 다른 인프라 계정으로 설정하려면 `ibmcloud ks credential-set` [명령](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_set)을 사용하십시오.
    * {{site.data.keyword.Bluemix_notm}} 종량과금제 계정에서 제공하는 기본 인증 정보를 사용하려면 `ibmcloud ks credential-unset --region <region>` [명령](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_unset)을 사용하십시오.

<br />


## `kubectl` 명령 제한시간 초과
{: #exec_logs_fail}

{: tsSymptoms}
`kubectl exec`, `kubectl attach`, `kubectl proxy`, `kubectl port-forward` 또는 `kubectl logs` 등의 명령을 실행하면 다음 메시지가 표시됩니다.

  ```
  <workerIP>:10250: getsockopt: connection timed out
  ```
  {: screen}

{: tsCauses}
마스터 노드와 작업자 노드 간의 OpenVPN 연결이 제대로 작동하지 않습니다.

{: tsResolve}
1. 클러스터용 다중 VLAN, 동일한 VLAN의 다중 서브넷 또는 다중 구역 클러스터가 있는 경우에는 작업자 노드가 사설 네트워크에서 서로 간에 통신할 수 있도록 IBM Cloud 인프라(SoftLayer) 계정에 대해 [Virtual Router Function (VRF)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud)을 사용으로 설정해야 합니다. VRF를 사용으로 설정하려면 [IBM Cloud 인프라(SoftLayer) 계정 담당자에게 문의](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#how-you-can-initiate-the-conversion)하십시오. VRF를 사용할 수 없거나 사용하지 않으려면 [VLAN Spanning](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning)을 사용으로 설정하십시오. 이 조치를 수행하려면 **네트워크 > 네트워크 VLAN Spanning 관리** [인프라 권한](/docs/containers?topic=containers-users#infra_access)이 필요합니다. 또는 이를 사용으로 설정하도록 계정 소유자에게 요청할 수 있습니다. VLAN Spanning이 이미 사용으로 설정되었는지 확인하려면 `ibmcloud ks vlan-spanning-get --region <region>` [명령](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get)을 사용하십시오. 
2. OpenVPN 클라이언트 팟(Pod)을 다시 시작하십시오.
  ```
  kubectl delete pod -n kube-system -l app=vpn
  ```
  {: pre}
3. 동일한 오류 메시지가 계속 표시되는 경우 VPN 팟(Pod)이 있는 작업자 노드가 비정상일 수 있습니다. VPN 팟(Pod)을 다시 시작하고 다른 작업자 노드로 다시 스케줄하려면 VPN 팟(Pod)이 있는 [작업자 노드를 유출, 드레인 및 다시 부팅](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reboot)하십시오.

<br />


## 서비스를 클러스터에 바인딩하면 동일한 이름 오류가 발생함
{: #cs_duplicate_services}

{: tsSymptoms}
`ibmcloud ks cluster-service-bind --cluster <cluster_name> --namespace <namespace> --service <service_instance_name>`을 실행하면 다음 메시지가 표시됩니다.

```
Multiple services with the same name were found.
Run 'ibmcloud service list' to view available Bluemix service instances...
```
{: screen}

{: tsCauses}
서로 다른 지역에 동일한 이름의 서비스 인스턴스가 여러 개 있습니다.

{: tsResolve}
`ibmcloud ks cluster-service-bind` 명령에서 서비스 인스턴스 이름 대신 서비스 GUID를 사용하십시오.

1. [바인딩할 서비스 인스턴스가 포함된 {{site.data.keyword.Bluemix_notm}} 지역에 로그인하십시오.](/docs/containers?topic=containers-regions-and-zones#bluemix_regions)

2. 서비스 인스턴스의 GUID를 가져오십시오.
  ```
  ibmcloud service show <service_instance_name> --guid
  ```
  {: pre}

  출력:
  ```
  Invoking 'cf service <service_instance_name> --guid'...
  <service_instance_GUID>
  ```
  {: screen}
3. 서비스를 클러스터에 다시 바인딩하십시오.
  ```
  ibmcloud ks cluster-service-bind --cluster <cluster_name> --namespace <namespace> --service <service_instance_GUID>
  ```
  {: pre}

<br />


## 서비스를 클러스터에 바인딩하면 서비스를 찾을 수 없음 오류가 발생함
{: #cs_not_found_services}

{: tsSymptoms}
`ibmcloud ks cluster-service-bind --cluster <cluster_name> --namespace <namespace> --service <service_instance_name>`을 실행하면 다음 메시지가 표시됩니다.

```
Binding service to a namespace...
FAILED

The specified IBM Cloud service could not be found. If you just created the service, wait a little and then try to bind it again. To view available IBM Cloud service instances, run 'ibmcloud service list'. (E0023)
```
{: screen}

{: tsCauses}
서비스를 클러스터에 바인드하려면 서비스 인스턴스가 프로비저닝된 영역에 대한 Cloud Foundry 개발자 사용자 역할이 있어야 합니다. 또한 {{site.data.keyword.containerlong}}에 대한 {{site.data.keyword.Bluemix_notm}} IAM 편집자 플랫폼 액세스 권한이 있어야 합니다. 서비스 인스턴스에 액세스하려면 서비스 인스턴스가 프로비저닝된 영역에 로그인되어 있어야 합니다.

{: tsResolve}

**사용자로서 다음을 수행하십시오.**

1. {{site.data.keyword.Bluemix_notm}}에 로그인하십시오.
   ```
   ibmcloud login
   ```
   {: pre}

2. 서비스 인스턴스가 프로비저닝된 조직 및 영역을 대상으로 지정하십시오.
   ```
   ibmcloud target -o <org> -s <space>
   ```
   {: pre}

3. 서비스 인스턴스를 나열하여 올바른 영역에 있는지 확인하십시오.
   ```
   ibmcloud service list
   ```
   {: pre}

4. 서비스를 다시 바인딩하십시오. 일부 오류가 발생하는 경우 계정 관리자에게 문의하여 서비스를 바인딩하기에 충분한 권한이 있는지 확인하십시오(다음 계정 관리자 단계 참조).

**계정 관리자로서 다음을 수행하십시오.**

1. 이 문제점이 발생한 사용자에게 [{{site.data.keyword.containerlong}}에 대한 편집자 권한](/docs/iam?topic=iam-iammanidaccser#edit_existing)이 있는지 확인하십시오.

2. 이 문제점이 발생한 사용자에게 서비스가 프로비저닝되는 [영역에 대한 Cloud Foundry 개발자 역할](/docs/iam?topic=iam-mngcf#update_cf_access)이 있는지 확인하십시오.

3. 올바른 권한이 있는 경우 다른 권한을 지정한 후 필요한 권한을 다시 지정해 보십시오.

4. 몇 분 동안 기다린 후 사용자가 서비스 바인드를 다시 시도하도록 하십시오.

5. 그래도 문제점이 해결되지 않는 경우에는 {{site.data.keyword.Bluemix_notm}} IAM 권한이 비동기화되며 사용자가 직접 문제를 해결할 수 없습니다. 지원 케이스를 열어서 [IBM 지원 센터에 문의](/docs/get-support?topic=get-support-getting-customer-support)하십시오. 클러스터 ID, 사용자 ID 및 서비스 인스턴스 ID를 제공해야 합니다.
   1. 클러스터 ID를 검색하십시오.
      ```
      ibmcloud ks clusters
      ```
      {: pre}

   2. 서비스 인스턴스 ID를 검색하십시오.
      ```
      ibmcloud service show <service_name> --guid
      ```
      {: pre}


<br />


## 서비스를 클러스터에 바인딩하면 서비스의 서비스 키 미지원 오류가 발생함
{: #cs_service_keys}

{: tsSymptoms}
`ibmcloud ks cluster-service-bind --cluster <cluster_name> --namespace <namespace> --service <service_instance_name>`을 실행하면 다음 메시지가 표시됩니다.

```
This service doesn't support creation of keys
```
{: screen}

{: tsCauses}
{{site.data.keyword.Bluemix_notm}}의 일부 서비스(예: {{site.data.keyword.keymanagementservicelong}})는 서비스 키라고도 하는 서비스 인증 정보의 작성을 지원하지 않습니다. 서비스 키의 지원이 없으면 서비스를 클러스터에 바인드할 수 없습니다. 서비스 키의 작성을 지원하는 서비스의 목록을 찾으려면 [외부 앱이 {{site.data.keyword.Bluemix_notm}} 서비스를 사용할 수 있도록 허용](/docs/resources?topic=resources-externalapp#externalapp)을 참조하십시오.

{: tsResolve}
서비스 키를 지원하지 않는 서비스를 통합하려면 앱에서 직접 서비스에 액세스하는 데 사용할 수 있는 API를 서비스가 제공하는지 확인하십시오. 예를 들어, {{site.data.keyword.keymanagementservicelong}}를 사용하려면 [API 참조 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/apidocs/kms?language=curl)를 참조하십시오.

<br />


## 작업자 노드가 업데이트되거나 다시 로드된 후 중복 노드 및 팟(Pod)이 표시됨
{: #cs_duplicate_nodes}

{: tsSymptoms}
`kubectl get nodes`를 실행할 때 **`NotReady`** 상태의 중복된 작업자 노드가 표시됩니다. **`NotReady`** 상태의 작업자 노드에는 공인 IP 주소가 있지만 **`Ready`** 상태의 작업자 노드에는 사설 IP 주소가 있습니다.

{: tsCauses}
이전 클러스터는 클러스터의 공인 IP 주소별로 작업자 노드를 나열했습니다. 이제 작업자 노드가 클러스터의 사설 IP 주소별로 나열됩니다. 노드를 다시 로드하거나 업데이트할 때 IP 주소가 변경되지만 공인 IP 주소에 대한 참조는 그대로 유지됩니다.

{: tsResolve}
이러한 중복 항목으로 인해 서비스가 중단되지는 않지만 API 서버에서 이전 작업자 노드 참조를 제거할 수 있습니다.

  ```
  kubectl delete node <node_name1> <node_name2>
  ```
  {: pre}

<br />


## 제한시간 초과로 인해 새 작업자 노드에서 팟(Pod)에 액세스하는 데 실패
{: #cs_nodes_duplicate_ip}

{: tsSymptoms}
클러스터에서 작업자 노드를 삭제한 다음 작업자 노드를 추가했습니다. 팟(Pod) 또는 Kubernetes 서비스를 배치한 경우 리소스에서 새로 작성한 작업자 노드에 액세스할 수 없으며 연결 제한시간이 초과됩니다.

{: tsCauses}
클러스터에서 작업자 노드를 삭제한 다음 작업자 노드를 추가하는 경우 삭제된 작업자 노드의 사설 IP 주소에 새 작업자 노드를 지정할 수 있습니다. Calico에서는 이 사설 IP 주소를 태그로 사용하고 계속하여 삭제된 노드에 연결하려고 합니다.

{: tsResolve}
정확한 노드를 가리키도록 사설 IP 주소의 참조를 수동으로 업데이트하십시오.

1.  **사설 IP** 주소가 동일한 작업자 노드가 두 개 있는지 확인하십시오. 삭제된 작업자의 **사설 IP** 및 **ID**를 참고하십시오.

  ```
  ibmcloud ks workers --cluster <cluster_name_or_id>
  ```
  {: pre}

  ```
  ID                                                 Public IP       Private IP       Machine Type   State     Status   Zone   Version
  kube-dal10-cr9b7371a7fcbe46d08e04f046d5e6d8b4-w1   169.xx.xxx.xxx  10.xxx.xx.xxx    b3c.4x16       normal    Ready    dal10      1.13.6
  kube-dal10-cr9b7371a7fcbe46d08e04f046d5e6d8b4-w2   169.xx.xxx.xxx  10.xxx.xx.xxx    b3c.4x16       deleted    -       dal10      1.13.6
  ```
  {: screen}

2.  [Calico CLI](/docs/containers?topic=containers-network_policies#adding_network_policies)를 설치하십시오.
3.  Calico에서 사용 가능한 작업자 노드를 나열하십시오. <path_to_file>을 Calico 구성 파일의 로컬 경로로 대체하십시오.

  ```
  calicoctl get nodes --config=filepath/calicoctl.cfg
  ```
  {: pre}

  ```
  NAME
  kube-dal10-cr9b7371a7faaa46d08e04f046d5e6d8b4-w1
  kube-dal10-cr9b7371a7faaa46d08e04f046d5e6d8b4-w2
  ```
  {: screen}

4.  Calico에서 중복 작업자 노드를 삭제하십시오. NODE_ID를 작업자 노드 ID로 대체하십시오.

  ```
  calicoctl delete node NODE_ID --config=<path_to_file>/calicoctl.cfg
  ```
  {: pre}

5.  삭제되지 않은 작업자 노드를 다시 부팅하십시오.

  ```
  ibmcloud ks worker-reboot --cluster <cluster_name_or_id> --worker <worker_id>
  ```
  {: pre}


삭제된 노드는 더 이상 Calico에 나열되지 않습니다.

<br />




## 팟(Pod) 보안 정책으로 인한 팟(Pod) 배치 실패
{: #cs_psp}

{: tsSymptoms}
팟(Pod)을 작성하거나 `kubectl get events`를 실행하여 팟(Pod) 배치를 확인한 후에 다음과 유사한 오류 메시지가 표시됩니다.

```
unable to validate against any pod security policy
```
{: screen}

{: tsCauses}
[`PodSecurityPolicy` 허가 제어기](/docs/containers?topic=containers-psp)는 팟(Pod) 작성을 시도한 배치 또는 Helm Tiller 등의 사용자 또는 서비스 계정에 대한 권한을 검사합니다. 팟(Pod) 보안 정책이 사용자 또는 서비스 계정을 지원하지 않는 경우, `PodSecurityPolicy` 허가 제어기는 팟(Pod)이 작성되지 않도록 합니다.

[{{site.data.keyword.IBM_notm}} 클러스터 관리](/docs/containers?topic=containers-psp#ibm_psp)를 위한 팟(Pod) 보안 정책 리소스 중 하나를 삭제한 경우, 이와 유사한 문제가 발생할 수 있습니다.

{: tsResolve}
사용자 또는 서비스 계정이 팟(Pod) 보안 정책에 의해 권한이 부여되었는지 확인하십시오. [기존 정책을 수정](/docs/containers?topic=containers-psp#customize_psp)해야 할 수 있습니다.

{{site.data.keyword.IBM_notm}} 클러스터 관리 리소스를 삭제한 경우에는 Kubernetes 마스터를 새로 고쳐서 이를 복원하십시오.

1.  [계정에 로그인하십시오. 해당되는 경우, 적절한 리소스 그룹을 대상으로 지정하십시오. 클러스터의 컨텍스트를 설정하십시오.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
2.  Kubernetes 마스터를 새로 고쳐서 이를 복원하십시오.

    ```
    ibmcloud ks apiserver-refresh
    ```
    {: pre}


<br />




## 클러스터가 보류 상태를 유지함
{: #cs_cluster_pending}

{: tsSymptoms}
클러스터를 배치하는 경우 보류 상태를 유지하고 시작하지 않습니다.

{: tsCauses}
클러스터를 방금 작성한 경우, 작업자 노드가 여전히 구성 중일 수 있습니다. 이미 일정한 기간 동안 대기한 경우 올바르지 않은 VLAN이 있을 수 있습니다.

{: tsResolve}

다음 솔루션 중 하나를 수행할 수 있습니다.
  - `ibmcloud ks clusters`를 실행하여 클러스터의 상태를 확인하십시오. 그런 다음, `ibmcloud ks workers --cluster <cluster_name>`을 실행하여 작업자 노드가 배치되었는지 확인하십시오.
  - VLAN이 올바른지 확인하십시오. 올바른 상태가 되려면 VLAN은 로컬 디스크 스토리지로 작업자를 호스팅할 수 있는 인프라와 연관되어야 합니다. `ibmcloud ks vlans --zone <zone>`을 실행하여 [VLAN을 나열](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlans)할 수 있으며, 해당 VLAN이 목록에 표시되지 않으면 이는 유효하지 않은 것입니다. 다른 VALN을 선택하십시오.

<br />


## 클러스터 작성 오류로 레지스트리에서 이미지를 가져올 수 없음
{: #ts_image_pull_create}

{: tsSymptoms}
클러스터를 작성할 때 다음과 유사한 오류 메시지가 표시됩니다. 


```
Your cluster cannot pull images from the IBM Cloud Container Registry 'icr.io' domains because an IAM access policy could not be created. Make sure that you have the IAM Administrator platform role to IBM Cloud Container Registry. Then, create an image pull secret with IAM credentials to the registry by running 'ibmcloud ks cluster-pull-secret-apply'.
```
{: screen}

{: tsCauses}
클러스터 작성 중에 서비스 ID는 클러스터를 위해 작성되고 {{site.data.keyword.registrylong_notm}}에 대한 **독자** 서비스 액세스 정책이 지정됩니다. 그런 다음 이 서비스 ID에 대한 API 키를 생성하고 [이미지 풀 시크릿](/docs/containers?topic=containers-images#cluster_registry_auth)에 저장하여 {{site.data.keyword.registrylong_notm}}에서 이미지를 가져올 수 있는 권한을 부여합니다. 

클러스터 작성 중에 **Reader** 서비스 액세스 정책을 서비스 ID에 지정하려면 {{site.data.keyword.registrylong_notm}}에 대한 **관리자** 플랫폼 액세스 정책이 있어야 합니다. 

{: tsResolve}

단계:
1.  계정 소유자가 사용자에게 {{site.data.keyword.registrylong_notm}}에 대한 **관리자** 역할을 부여했는지 확인하십시오.
    ```
        ibmcloud iam user-policy-create <your_user_email> --service-name container-registry --roles Administrator
    ```
    {: pre}
2.  [`ibmcloud ks cluster-pull-secret-apply` 명령](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_pull_secret_apply)을 사용하여 적절한 레지스트리 인증 정보로 이미지 풀 시크릿을 다시 작성하십시오. 

<br />


## `ImagePullBackOff` 또는 권한 부여 오류로 레지스트리에서 이미지를 가져오는 데 실패함
{: #ts_image_pull}

{: tsSymptoms}

{{site.data.keyword.registrylong_notm}}에서 이미지를 가져오는 워크로드를 배치할 때 팟(Pod)에 오류가 발생하며 **`ImagePullBackOff`** 상태가 됩니다.

```
kubectl get pods
```
{: pre}

```
NAME         READY     STATUS             RESTARTS   AGE
<pod_name>   0/1       ImagePullBackOff   0          2m
```
{: screen}

팟(Pod)을 설명할 때 다음과 유사한 인증 오류가 발생합니다.

```
kubectl describe pod <pod_name>
```
{: pre}

```
Failed to pull image "<region>.icr.io/<namespace>/<image>:<tag>" ... unauthorized: authentication required
Failed to pull image "<region>.icr.io/<namespace>/<image>:<tag>" ... 401 Unauthorized
```
{: screen}

```
Failed to pull image "registry.ng.bluemix.net/<namespace>/<image>:<tag>" ... unauthorized: authentication required
Failed to pull image "registry.ng.bluemix.net/<namespace>/<image>:<tag>" ... 401 Unauthorized
```
{: screen}

{: tsCauses}
클러스터는 API 키 또는 [이미지 풀 시크릿](/docs/containers?topic=containers-images#cluster_registry_auth)에 저장된 토큰을 사용하여 클러스터가 {{site.data.keyword.registrylong_notm}}에서 이미지를 가져올 수 있는 권한을 부여합니다. 기본적으로 새 클러스터에는 API 키를 사용하여 클러스터가 `default` Kubernetes 네임스페이스에 배치된 컨테이너의 모든 지역 `icr.io` 레지스트리에서 이미지를 가져올 수 있도록 하는 이미지 풀 시크릿이 있습니다. 클러스터에 토큰을 사용하는 이미지 풀 시크릿이 있는 경우, {{site.data.keyword.registrylong_notm}}에 대한 기본 액세스 권한은 더 이상 사용되지 않는 `<region>.registry.bluemix.net` 도메인을 사용하는 특정 지역 레지스트리로만 더 제한됩니다.

{: tsResolve}

1.  배치 YAML 파일에서 이미지의 올바른 이름 및 태그를 사용하는지 확인하십시오.
    ```
    ibmcloud cr images
    ```
    {: pre}
2.  장애가 발생한 팟(Pod)의 팟(Pod) 구성 파일을 가져오고 `imagePullSecrets` 섹션을 찾으십시오.
    ```
    kubectl get pod <pod_name> -o yaml
    ```
    {: pre}

    출력 예:
    ```
    ...
    imagePullSecrets:
    - name: bluemix-default-secret
    - name: bluemix-default-secret-regional
    - name: bluemix-default-secret-international
    - name: default-us-icr-io
    - name: default-uk-icr-io
    - name: default-de-icr-io
    - name: default-au-icr-io
    - name: default-jp-icr-io
    - name: default-icr-io
    ...
    ```
    {: screen}
3.  이미지 풀 시크릿이 나열되어 있지 않은 경우, 네임스페이스에 이미지 풀 시크릿을 설정하십시오.
    1.  [이미지 풀 시크릿을 `default` Kubernetes 네임스페이스에서 워크로드를 배치할 네임스페이스로 복사](/docs/containers?topic=containers-images#copy_imagePullSecret)하십시오.
    2.  네임스페이스의 모든 팟(Pod)이 이미지 풀 시크릿 인증 정보를 사용할 수 있도록 [이미지 풀 시크릿을 이 Kubernetes 네임스페이스의 서비스 계정에 추가](/docs/containers?topic=containers-images#store_imagePullSecret)하십시오.
4.  이미지 풀 시크릿이 나열되면 컨테이너 레지스트리에 액세스하는 데 사용하는 인증 정보 유형을 판별하십시오.
    *   **더이상 사용되지 않음**: 시크릿의 이름에 `bluemix`가 있는 경우, 레지스트리 토큰을 사용하여 더 이상 사용되지 않는 `registry.<region>.bluemix.net` 도메인 이름으로 인증합니다. [토큰을 사용하는 이미지 풀 시크릿 문제점 해결](#ts_image_pull_token)으로 계속하십시오.
    *   시크릿의 이름에 `icr`이 있는 경우 API 키를 사용하여 `icr.io` 도메인 이름으로 인증합니다. [API 키를 사용하는 이미지 풀 시크릿 문제점 해결](#ts_image_pull_apikey)으로 계속하십시오.
    *   두 유형의 시크릿이 모두 있으면, 두 인증 방법 모두 사용합니다. 앞으로는 컨테이너 이미지용 배치 YAML에 `icr.io` 도메인 이름을 사용하십시오. [API 키를 사용하는 이미지 풀 시크릿 문제점 해결](#ts_image_pull_apikey)으로 계속하십시오.

<br>
<br>

**API 키를 사용하는 이미지 풀 시크릿 문제점 해결**</br>
{: #ts_image_pull_apikey}

팟(Pod) 구성에 API 키를 사용하는 이미지 풀 시크릿이 있는 경우, API 키 인증 정보가 올바르게 설정되어 있는지 확인합니다.
{: shortdesc}

다음 단계에서는 API 키가 서비스 ID의 인증 정보를 저장한다고 가정합니다. 개별 사용자의 API 키를 사용하기 위해 이미지 풀 시크릿을 설정한 경우에는 해당 사용자의 {{site.data.keyword.Bluemix_notm}} IAM 권한과 인증 정보를 확인해야 합니다.
{: note}

1.  **설명**을 검토하여 API 키가 이미지 풀 시크릿에 사용하는 서비스 ID를 찾으십시오. 클러스터로 작성된 서비스 ID는 `ID for <cluster_name>`을 표시하며 `default` Kubernetes 네임스페이스에서 사용됩니다. 다른 Kubernetes 네임스페이스에 액세스하거나 {{site.data.keyword.Bluemix_notm}} IAM 권한을 수정하는 등 다른 서비스 ID를 작성한 경우 설명을 사용자 정의했습니다.
    ```
    ibmcloud iam service-ids
    ```
    {: pre}

    출력 예:
    ```
    UUID                Name               Created At              Last Updated            Description                                                                                                                                                                                         Locked     
    ServiceId-aa11...   <service_ID_name>  2019-02-01T19:01+0000   2019-02-01T19:01+0000   ID for <cluster_name>                                                                                                                                         false   
    ServiceId-bb22...   <service_ID_name>  2019-02-01T19:01+0000   2019-02-01T19:01+0000   Service ID for IBM Cloud Container Registry in Kubernetes cluster <cluster_name> namespace <kube_namespace>                                                                                                                                         false    
    ```
    {: screen}
2.  서비스 ID가 {{site.data.keyword.Bluemix_notm}} IAM **독자** [{{site.data.keyword.registryshort_notm}}의 서비스 액세스 역할 정책](/docs/services/Registry?topic=registry-user#create) 이상의 역할로 지정되어 있는지 확인하십시오. 서비스 ID에 **독자** 서비스 역할이 없으면 [IAM 정책을 편집](/docs/iam?topic=iam-serviceidpolicy#access_edit)하십시오. 정책이 올바른 경우 다음 단계를 계속하여 인증 정보가 올바른지 확인하십시오.
    ```
    ibmcloud iam service-policies <service_ID_name>
    ```
    {: pre}

    출력 예:
    ```              
    Policy ID:   a111a111-b22b-333c-d4dd-e555555555e5   
    Roles:       Reader   
    Resources:                            
                  Service Name       container-registry      
                  Service Instance         
                  Region                  
                  Resource Type      namespace      
                  Resource           <registry_namespace>  
    ```
    {: screen}  
3.  이미지 풀 보안 인증 정보가 올바른지 확인하십시오.
    1.  이미지 풀 시크릿 구성을 가져오십시오. 팟(Pod)이 `default` 네임스페이스에 없으면 `-n` 플래그를 포함하십시오.
        ```
        kubectl get secret <image_pull_secret_name> -o yaml [-n <namespace>]
        ```
        {: pre}
    2.  출력에서 `.dockercfg` 필드의 base64 인코딩된 값을 복사하십시오.
        ```
        apiVersion: v1
        kind: Secret
        data:
          .dockercfg: eyJyZWdp...==
        ...
        ```
        {: screen}
    3.  base64 문자열을 디코딩하십시오. 예를 들어, OS X에서 다음 명령을 실행할 수 있습니다.
        ```
        echo -n "<base64_string>" | base64 --decode
        ```
        {: pre}

        출력 예:
        ```
        {"auths":{"<region>.icr.io":{"username":"iamapikey","password":"<password_string>","email":"<name@abc.com>","auth":"<auth_string>"}}}
        ```
        {: screen}
    4.  이미지 풀 시크릿 지역 레지스트리 도메인 이름을 컨테이너 이미지에 지정한 도메인 이름과 비교하십시오. 기본적으로 새 클러스터에는 `default` Kubernetes 네임스페이스에서 실행되는 컨테이너의 각 지역 레지스트리 도메인 이름에 대한 이미지 풀 시크릿이 포함되어 있습니다. 그러나 기본 설정을 수정했거나 다른 Kubernetes 네임스페이스를 사용하는 경우에는 지역 레지스트리에 대힌 이미지 풀 시크릿이 없을 수 있습니다. 지역 레지스트리 도메인 이름에 필요한 [이미지 풀 시크릿을 복사](/docs/containers?topic=containers-images#copy_imagePullSecret)하십시오.
    5.  이미지 풀 시크릿의 `username` 및 `password`를 사용하여 로컬 시스템에서 해당 레지스트리에 로그인하십시오. 로그인할 수 없는 경우 서비스 ID를 수정해야 합니다.
        ```
        docker login -u iamapikey -p <password_string> <region>.icr.io
        ```
        {: pre}
        1.  클러스터 서비스 ID, {{site.data.keyword.Bluemix_notm}} IAM 정책, API 키 및 `default` Kubernetes 네임스페이스에서 실행되는 이미지 풀 시크릿을 다시 작성하십시오.
            ```
            ibmcloud ks cluster-pull-secret-apply --cluster <cluster_name_or_ID>
            ```
            {: pre}
        2.  `default` Kubernetes 네임스페이스에서 배치를 다시 작성하십시오. 권한 오류 메시지가 계속 표시되면 새 이미지 풀 시크릿을 사용하여 1-5단계를 반복하십시오. 여전히 로그인할 수 없으면 [Slack에서 IBM 팀에 문의하거나 {{site.data.keyword.Bluemix_notm}} 지원 케이스를 여십시오](#clusters_getting_help).
    6.  로그인에 성공하면 로컬로 이미지를 가져오십시오. 명령이 실패하고 `access denied` 오류 메시지가 표시되는 경우 레지스트리 계정은 클러스터가 있는 계정과 다른 {{site.data.keyword.Bluemix_notm}} 계정에 있습니다. [다른 계정의 이미지에 액세스하는 이미지 풀 시크릿을 작성](/docs/containers?topic=containers-images#other_registry_accounts)하십시오. 이미지를 로컬 머신에 가져올 수 있는 경우 API 키에 올바른 권한이 제공되지만 클러스터의 API 설정은 올바르지 않습니다. 이 문제를 해결할 수 없습니다. [Slack에서 IBM 팀에 문의하거나 {{site.data.keyword.Bluemix_notm}} 지원 케이스를 여십시오](#clusters_getting_help).
        ```
        docker pull <region>icr.io/<namespace>/<image>:<tag>
        ```
        {: pre}

<br>
<br>

**더 이상 사용되지 않음: 토큰을 사용하는 이미지 풀 시크릿 문제점 해결**</br>
{: #ts_image_pull_token}

팟(Pod) 구성에 토큰을 사용하는 이미지 풀 시크릿이 있는 경우, 토큰 인증 정보가 올바른지 확인합니다.
{: shortdesc}

{{site.data.keyword.registrylong_notm}}에 대한 클러스터 액세스 권한을 부여하기 위해 토큰을 사용하는 이 방법은 `registry.bluemix.net` 도메인 이름에 대해서는 지원되지만 더 이상 사용되지 않습니다. 대신 [API 키 메소드를 사용](/docs/containers?topic=containers-images#cluster_registry_auth)하여 새 `icr.io` 레지스트리 도메인 이름에 대한 클러스터 액세스 권한을 부여하십시오.
{: deprecated}

1.  이미지 풀 시크릿 구성을 가져오십시오. 팟(Pod)이 `default` 네임스페이스에 없으면 `-n` 플래그를 포함하십시오.
    ```
    kubectl get secret <image_pull_secret_name> -o yaml [-n <namespace>]
    ```
    {: pre}
2.  출력에서 `.dockercfg` 필드의 base64 인코딩된 값을 복사하십시오.
    ```
    apiVersion: v1
    kind: Secret
    data:
      .dockercfg: eyJyZWdp...==
    ...
    ```
    {: screen}
3.  base64 문자열을 디코딩하십시오. 예를 들어, OS X에서 다음 명령을 실행할 수 있습니다.
    ```
    echo -n "<base64_string>" | base64 --decode
    ```
    {: pre}

    출력 예:
    ```
    {"auths":{"registry.<region>.bluemix.net":{"username":"token","password":"<password_string>","email":"<name@abc.com>","auth":"<auth_string>"}}}
    ```
    {: screen}
4.  레지스트리 도메인 이름을 컨테이너 이미지에 지정한 도메인 이름과 비교하십시오. 예를 들어, 이미지 풀 시크릿이 `registry.ng.bluemix.net` 도메인에 대한 액세스 권한을 부여하지만 `registry.eu-de.bluemix.net`에 저장된 이미지를 지정한 경우 `registry.eu-de.bluemix.net`에 대해 [이미지 풀 시크릿에서 사용할 토큰을 작성](/docs/containers?topic=containers-images#token_other_regions_accounts)해야 합니다.
5.  이미지 풀 시크릿의 `username` 및 `password`를 사용하여 로컬 시스템에서 해당 레지스트리에 로그인하십시오. 로그인할 수 없는 경우 토큰에 해결할 수 없는 문제가 있습니다. [Slack에서 IBM 팀에 문의하거나 {{site.data.keyword.Bluemix_notm}} 지원 케이스를 여십시오](#clusters_getting_help).
    ```
    docker login -u token -p <password_string> registry.<region>.bluemix.net
    ```
    {: pre}
6.  로그인에 성공하면 로컬로 이미지를 가져오십시오. 명령이 실패하고 `access denied` 오류 메시지가 표시되는 경우 레지스트리 계정은 클러스터가 있는 계정과 다른 {{site.data.keyword.Bluemix_notm}} 계정에 있습니다. [다른 계정의 이미지에 액세스하는 이미지 풀 시크릿을 작성](/docs/containers?topic=containers-images#token_other_regions_accounts)하십시오. 명령이 성공하면 [Slack에서 IBM 팀에 문의하거나 {{site.data.keyword.Bluemix_notm}} 지원 케이스를 여십시오](#clusters_getting_help).
    ```
    docker pull registry.<region>.bluemix.net/<namespace>/<image>:<tag>
    ```
    {: pre}

<br />


## 팟(Pod)이 보류 상태를 유지함
{: #cs_pods_pending}

{: tsSymptoms}
`kubectl get pods`를 실행할 때 **보류** 상태를 유지하는 팟(Pod)을 볼 수 있습니다.

{: tsCauses}
Kubernetes 클러스터를 방금 작성한 경우, 작업자 노드가 여전히 구성 중일 수 있습니다.

이 클러스터가 기존 클러스터인 경우:
*  클러스터에 팟(Pod)을 배치하기에 충분한 용량이 없을 수 있습니다.
*  팟(Pod)이 리소스 요청 또는 한계를 초과했을 수 있습니다.

{: tsResolve}
이 태스크에서는 클러스터에 {{site.data.keyword.Bluemix_notm}} IAM [**관리자** 플랫폼 역할](/docs/containers?topic=containers-users#platform)과 모든 네임스페이스에 대한 [**관리자** 서비스 역할](/docs/containers?topic=containers-users#platform)이 필요합니다.

방금 Kubernetes 클러스터를 작성한 경우에는 다음 명령을 실행하고 작업자 노드가 초기화될 때까지 대기하십시오.

```
kubectl get nodes
```
{: pre}

이 클러스터가 기존 클러스터인 경우, 클러스터 용량을 확인하십시오.

1.  기본 포트 번호로 프록시를 설정하십시오.

  ```
  kubectl proxy
  ```
   {: pre}

2.  Kubernetes 대시보드를 여십시오.

  ```
  http://localhost:8001/ui
  ```
  {: pre}

3.  팟(Pod)을 배치하기에 충분한 용량이 클러스터에 있는지 확인하십시오.

4.  클러스터에 충분한 용량이 없으면 작업자 풀의 크기를 조정하여 노드를 더 추가하십시오.

    1.  작업자 풀의 현재 크기와 머신 유형을 검토하여 크기를 조정할 대상을 결정하십시오.

        ```
        ibmcloud ks worker-pools
        ```
        {: pre}

    2.  작업자 풀의 크기를 조정하여 풀의 범위에 속한 각 구역에 노드를 더 추가하십시오.

        ```
        ibmcloud ks worker-pool-resize --worker-pool <worker_pool> --cluster <cluster_name_or_ID> --size-per-zone <workers_per_zone>
        ```
        {: pre}

5.  선택사항: 팟(Pod) 리소스 요청을 확인하십시오.

    1.  `resources.requests` 값이 작업자 노드의 용량보다 크지 않은지 확인하십시오. 예를 들어, 팟(Pod)에서 `cpu: 4000m` 또는 4 코어를 요청하지만 작업자 노드 크기가 단지 2 코어이면 팟(Pod)을 배치할 수 없습니다.

        ```
        kubectl get pod <pod_name> -o yaml
        ```
        {: pre}

    2.  요청이 사용 가능한 용량을 초과하면 요청을 이행할 수 있는 작업자 노드의 [새 작업자 풀을 추가](/docs/containers?topic=containers-add_workers#add_pool)하십시오.

6.  작업자 노드를 완전히 배치한 후에도 팟(Pod)이 계속 **보류** 상태를 유지하면 [Kubernetes 문서![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/tasks/debug-application-cluster/debug-pod-replication-controller/#my-pod-stays-pending)를 검토하여 보류 상태인 팟(Pod)의 추가 문제점을 해결할 수 있습니다.

<br />


## 컨테이너가 시작되지 않음
{: #containers_do_not_start}

{: tsSymptoms}
팟(Pod)이 클러스터에 배치되지만 컨테이너가 시작되지 않습니다.

{: tsCauses}
레지스트리 할당량에 도달하면 컨테이너가 시작되지 않을 수 있습니다.

{: tsResolve}
[{{site.data.keyword.registryshort_notm}}의 스토리지를 비우십시오.](/docs/services/Registry?topic=registry-registry_quota#registry_quota_freeup)

<br />


## 팟(Pod) 다시 시작이 계속 실패하거나 팟(Pod)이 예기치 않게 제거됨
{: #pods_fail}

{: tsSymptoms}
팟(Pod)이 정상이었으나 예기치 않게 제거되거나 다시 시작에서 루프가 발생합니다.

{: tsCauses}
컨테이너가 리소스 한계를 초과했거나 팟(Pod)이 우선순위가 더 높은 팟(Pod)으로 대체되었을 수 있습니다.

{: tsResolve}
컨테이너가 리소스 한계로 인해 제거되었는지 확인하려면 다음을 수행하십시오.
<ol><li>팟(Pod)의 이름을 가져오십시오. 레이블을 사용한 경우에는 이를 포함시켜 결과를 필터링할 수 있습니다.<pre class="pre"><code>kubectl get pods --selector='app=wasliberty'</code></pre></li>
<li>팟(Pod)에 대한 설명을 출력하고 **Restart Count**를 찾으십시오.<pre class="pre"><code>kubectl describe pod</code></pre></li>
<li>팟(Pod)이 짧은 시간 내에 여러 번 다시 시작된 경우에는 해당 상태를 페치하십시오. <pre class="pre"><code>kubectl get pod -o go-template={{range.status.containerStatuses}}{{"Container Name: "}}{{.name}}{{"\r\nLastState: "}}{{.lastState}}{{end}}</code></pre></li>
<li>이유를 검토하십시오. 예를 들어, `OOM Killed`는 "메모리 부족"을 의미하며, 이는 컨테이너가 리소스 한계로 인해 충돌하고 있음을 나타냅니다.</li>
<li>리소스를 채울 수 있도록 클러스터에 용량을 추가하십시오.</li></ol>

<br>

팟(Pod)이 우선순위가 더 높은 팟(Pod)으로 대체되고 있는지 확인하려면 다음을 수행하십시오.
1.  팟(Pod)의 이름을 가져오십시오.

    ```
    kubectl get pods
    ```
    {: pre}

2.  팟(Pod) YAML에 대한 설명을 출력하십시오.

    ```
    kubectl get pod <pod_name> -o yaml
    ```
    {: pre}

3.  `priorityClassName` 필드를 확인하십시오.

    1.  `priorityClassName` 필드 값이 없는 경우 사용자 팟(Pod)의 우선순위 클래스는 `globalDefault`입니다. 클러스터 관리자가 `globalDefault` 우선순위 클래스를 설정하지 않은 경우 기본값은 영(0) 또는 가장 낮은 우선순위입니다. 이보다 우선순위가 더 높은 팟(Pod)은 사용자의 팟(Pod)을 대체하거나 제거할 수 있습니다.

    2.  `priorityClassName` 필드 값이 있는 경우에는 우선순위 클래스를 가져오십시오.

        ```
        kubectl get priorityclass <priority_class_name> -o yaml
        ```
        {: pre}

    3.  `value` 필드를 참고하여 사용자 팟(Pod)의 우선순위를 확인하십시오.

4.  클러스터 내의 기존 우선순위 클래스를 나열하십시오.

    ```
    kubectl get priorityclasses
    ```
    {: pre}

5.  각 우선순위 클래스에 대해 YAML 파일을 가져온 후 `value` 필드를 기록하십시오.

    ```
    kubectl get priorityclass <priority_class_name> -o yaml
    ```
    {: pre}

6.  사용자 팟(Pod)의 우선순위 클래스를 다른 우선순위 클래스 값과 비교하여 우선순위가 더 높은지, 또는 더 낮은지 확인하십시오.

7.  클러스터에 있는 다른 팟(Pod)에 대해 1 - 3단계를 반복하여 이들이 사용 중인 우선순위 클래스를 확인하십시오. 이러한 팟(Pod)의 우선순위 클래스가 사용자의 팟(Pod)보다 높은 경우, 사용자의 팟(Pod)은 자신 및 자신보다 우선순위가 높은 모든 팟(Pod)을 위한 충분한 리소스가 있는 경우가 아니면 프로비저닝되지 않습니다.

8.  클러스터 관리자에게 문의하여 클러스터에 용량을 추가하도록 요청하고 올바른 우선순위 클래스가 지정되었는지 확인하십시오.

<br />


## 업데이트된 구성 값을 사용하여 Helm 차트를 설치할 수 없음
{: #cs_helm_install}

{: tsSymptoms}
`helm install -f config.yaml --namespace=kube-system --name=<release_name> ibm/<chart_name>`을 실행하여 업데이트된 Helm 차트를 설치하려는 경우 `Error: failed to download "ibm/<chart_name>"` 오류 메시지가 표시됩니다.

{: tsCauses}
Helm 인스턴스의 {{site.data.keyword.Bluemix_notm}} 저장소에 대한 URL이 올바르지 않을 수 있습니다.

{: tsResolve}
Helm 차트의 문제점을 해결하려면 다음을 수행하십시오.

1. Helm 인스턴스에서 현재 사용 가능한 저장소를 나열하십시오.

    ```
    helm repo list
    ```
    {: pre}

2. 출력에서 {{site.data.keyword.Bluemix_notm}} 저장소인 `ibm`에 대한 URL이 `https://icr.io/helm/iks-charts`인지 확인하십시오.

    ```
    NAME    URL
    stable  https://kubernetes-charts.storage.googleapis.com
    local   http://127.0.0.1:8888/charts
    ibm     https://icr.io/helm/iks-charts
    ```
    {: screen}

    * URL이 올바르지 않은 경우:

        1. {{site.data.keyword.Bluemix_notm}} 저장소를 제거하십시오.

            ```
            helm repo remove ibm
            ```
            {: pre}

        2. {{site.data.keyword.Bluemix_notm}} 저장소를 다시 추가하십시오.

            ```
            helm repo add iks-charts  https://icr.io/helm/iks-charts
            ```
            {: pre}

    * URL이 올바른 경우, 저장소에서 최신 업데이트를 가져오십시오.

        ```
        helm repo update
        ```
        {: pre}

3. 업데이트를 사용하여 Helm 차트를 설치하십시오.

    ```
    helm install -f config.yaml --namespace=kube-system --name=<release_name> ibm/<chart_name>
    ```
    {: pre}

<br />


## Helm tiller를 설치하거나 내 클러스터의 공용 이미지에서 컨테이너를 배치할 수 없습니다.
{: #cs_tiller_install}

{: tsSymptoms}

Helm tiller를 설치하거나 공용 레지스트리(예: DockerHub)의 이미지를 배치하려는 경우 설치에 실패하고 다음과 유사한 오류가 발생합니다.

```
Failed to pull image "gcr.io/kubernetes-helm/tiller:v2.12.0": rpc error: code = Unknown desc = failed to resolve image "gcr.io/kubernetes-helm/tiller:v2.12.0": no available registry endpoint:
```
{: screen}

{: tsCauses}
사용자 정의 Calico 정책을 지정하거나, 이미지가 저장된 컨테이너 레지스트리에 대한 공용 네트워크 연결을 차단하는 개인 서비스 엔드포인트를 사용하여 사설 전용 클러스터를 작성했을 수 있습니다.

{: tsResolve}
- 사용자 정의 방화벽을 사용하거나 사용자 정의 Calico 정책을 설정하는 경우, 이미지가 저장된 컨테이너 레지스트리와 작업자 노드 사이의 아웃바운드 및 인바운드 네트워크 트래픽을 허용하십시오. 이미지가 {{site.data.keyword.registryshort_notm}}에 저장되어 있으면 [클러스터가 인프라 리소스 및 기타 서비스에 액세스할 수 있도록 허용](/docs/containers?topic=containers-firewall#firewall_outbound)에서 필수 포트를 검토하십시오.
- 개인 서비스 엔드포인트만 사용으로 설정하여 사설 클러스터를 작성한 경우 클러스터의 [공용 서비스 엔드포인트를 사용으로 설정](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_disable)할 수 있습니다. 공용 연결을 열지 않고 사설 클러스터에 Helm 차트를 설치하려는 경우 [Tiller가 포함](/docs/containers?topic=containers-helm#private_local_tiller)되거나 [Tiller가 포함되지 않은](/docs/containers?topic=containers-helm#private_install_without_tiller) Helm을 설치할 수 있습니다.

<br />


## 도움 및 지원 받기
{: #clusters_getting_help}

클러스터에 여전히 문제점이 있습니까?
{: shortdesc}

-  터미널에서 `ibmcloud` CLI 및 플러그인에 대한 업데이트가 사용 가능한 시점을 사용자에게 알려줍니다. 사용 가능한 모든 명령과 플래그를 사용할 수 있도록 반드시 CLI를 최신 상태로 유지하십시오.
-   {{site.data.keyword.Bluemix_notm}}가 사용 가능한지 확인하려면 [{{site.data.keyword.Bluemix_notm}} 상태 페이지 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")를 확인](https://cloud.ibm.com/status?selected=status)하십시오.
-   [{{site.data.keyword.containerlong_notm}} Slack ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://ibm-container-service.slack.com)에 질문을 게시하십시오.
    {{site.data.keyword.Bluemix_notm}} 계정에 대해 IBM ID를 사용 중이 아닌 경우에는 이 Slack에 대한 [초대를 요청](https://bxcs-slack-invite.mybluemix.net/)하십시오.
    {: tip}
-   포럼을 검토하여 다른 사용자에게도 동일한 문제가 발생하는지 여부를 확인하십시오. 포럼을 사용하여 질문을 할 때는 {{site.data.keyword.Bluemix_notm}} 개발 팀이 볼 수 있도록 질문에 태그를 지정하십시오.
    -   {{site.data.keyword.containerlong_notm}}로 클러스터 또는 앱을 개발하거나 배치하는 데 대한 기술적 질문이 있으면 [Stack Overflow![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://stackoverflow.com/questions/tagged/ibm-cloud+containers)에 질문을 게시하고 질문에 `ibm-cloud`, `kubernetes` 및 `containers` 태그를 지정하십시오.
    -   서비스 및 시작하기 지시사항에 대한 질문이 있으면 [IBM Developer Answers ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix) 포럼을 사용하십시오. `ibm-cloud` 및 `containers` 태그를 포함하십시오.
    포럼 사용에 대한 세부사항은 [도움 받기](/docs/get-support?topic=get-support-getting-customer-support#using-avatar)를 참조하십시오.
-   케이스를 열어 IBM 지원 센터에 문의하십시오. IBM 지원 케이스 열기 또는 지원 레벨 및 케이스 심각도에 대해 알아보려면 [지원 문의](/docs/get-support?topic=get-support-getting-customer-support)를 참조하십시오.
문제를 보고할 때 클러스터 ID를 포함하십시오. 클러스터 ID를 가져오려면 `ibmcloud ks clusters`를 실행하십시오. 또한 [{{site.data.keyword.containerlong_notm}} 진단 및 디버그 도구](/docs/containers?topic=containers-cs_troubleshoot#debug_utility)를 사용하여 IBM 지원 센터와 공유할 관련 정보를 클러스터에서 수집하고 내보낼 수도 있습니다.
{: tip}

