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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}



# 클러스터 스토리지 문제점 해결
{: #cs_troubleshoot_storage}

{{site.data.keyword.containerlong}}를 사용할 때 클러스터 스토리지 관련 문제점을 해결하려면 이러한 기술을 고려하십시오.
{: shortdesc}

더 일반적인 문제점이 있는 경우에는 [클러스터 디버깅](/docs/containers?topic=containers-cs_troubleshoot)을 시도해 보십시오.
{: tip}


## 지속적 스토리지 실패 디버깅
{: #debug_storage}

지속적 스토리지를 디버그하기 위한 옵션을 검토하고 실패의 근본 원인을 찾습니다.
{: shortdesc}

1. 최신 {{site.data.keyword.Bluemix_notm}} 및 {{site.data.keyword.containerlong_notm}} 플러그인 버전을 사용하는지 확인하십시오. 
   ```
   ibmcloud update
   ```
   {: pre}
   
   ```
   ibmcloud plugin repo-plugins
   ```
   {: pre}

2. 로컬 머신에서 실행되는 `kubectl` CLI 버전이 클러스터에 설치된 Kubernetes 버전과 일치하는지 확인하십시오.  
   1. 클러스터 및 로컬 머신에 설치된 `kubectl` CLI 버전을 표시하십시오.
      ```
      kubectl version
      ```
      {: pre} 
   
      출력 예: 
      ```
      Client Version: version.Info{Major:"1", Minor:"13", GitVersion:"v1.13.5", GitCommit:"641856db18352033a0d96dbc99153fa3b27298e5", GitTreeState:"clean", BuildDate:"2019-03-25T15:53:57Z", GoVersion:"go1.12.1", Compiler:"gc", Platform:"darwin/amd64"}
      Server Version: version.Info{Major:"1", Minor:"13", GitVersion:"v1.13.5+IKS", GitCommit:"e15454c2216a73b59e9a059fd2def4e6712a7cf0", GitTreeState:"clean", BuildDate:"2019-04-01T10:08:07Z", GoVersion:"go1.11.5", Compiler:"gc", Platform:"linux/amd64"}
      ```
      {: screen}
    
      CLI 버전은 클라이언트 및 서버의 `GitVersion`에서 동일한 버전을 볼 수 있는 경우 일치합니다. 서버용 버전의 `+IKS` 부분을 무시할 수 있습니다.
   2. 로컬 머신 및 클러스터의 `kubectl` CLI 버전이 일치하지 않으면 [클러스터를 업데이트](/docs/containers?topic=containers-update)하거나 [로컬 머신에 서로 다른 CLI 버전을 설치](/docs/containers?topic=containers-cs_cli_install#kubectl)하십시오. 

3. 블록 스토리지, 오브젝트 스토리지 및 Portworx만 해당: [Kubernetes 서비스 계정을 사용하여 Helm 서버 Tiller를 설치](/docs/containers?topic=containers-helm#public_helm_install)했는지 확인하십시오. 

4. 블록 스토리지, 오브젝트 스토리지 및 Portworx만 해당: 플러그인에 적합한 최신 Helm 차트 버전을 설치했는지 확인하십시오. 
   
   **블록 및 오브젝트 스토리지**: 
   
   1. Helm 차트 저장소를 업데이트하십시오.
      ```
   helm repo update
      ```
      {: pre}
      
   2. `iks-charts` 저장소에 Helm 차트를 나열하십시오.
      ```
    helm search iks-charts
      ```
      {: pre}
      
      출력 예: 
      ```
      iks-charts/ibm-block-storage-attacher          	1.0.2        A Helm chart for installing ibmcloud block storage attach...
      iks-charts/ibm-iks-cluster-autoscaler          	1.0.5        A Helm chart for installing the IBM Cloud cluster autoscaler
      iks-charts/ibm-object-storage-plugin           	1.0.6        A Helm chart for installing ibmcloud object storage plugin  
      iks-charts/ibm-worker-recovery                 	1.10.46      IBM Autorecovery system allows automatic recovery of unhe...
      ...
      ```
      {: screen}
      
   3. 클러스터에 설치된 Helm 차트를 나열하고 설치한 버전을 사용 가능한 버전과 비교하십시오.
      ```
   helm ls
      ```
      {: pre}
      
   4. 최신 버전이 사용 가능한 경우 이 버전을 설치하십시오. 지시사항은 [{{site.data.keyword.Bluemix_notm}} Block Storage 플러그인 업데이트](/docs/containers?topic=containers-block_storage#updating-the-ibm-cloud-block-storage-plug-in) 및 [{{site.data.keyword.cos_full_notm}} 플러그인 업데이트](/docs/containers?topic=containers-object_storage#update_cos_plugin)를 참조하십시오. 
   
   **Portworx**: 
   
   1. 사용 가능한 [최신 Helm 차트 버전 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/portworx/helm/tree/master/charts/portworx)을 찾으십시오.  
   
   2. 클러스터에 설치된 Helm 차트를 나열하고 설치한 버전을 사용 가능한 버전과 비교하십시오.
      ```
   helm ls
      ```
      {: pre}
   
   3. 최신 버전이 사용 가능한 경우 이 버전을 설치하십시오. 지시사항은 [클러스터에서 Portworx 업데이트](/docs/containers?topic=containers-portworx#update_portworx)를 참조하십시오. 
   
5. 스토리지 드라이버 및 플러그인 팟(Pod)의 상태가 **실행 중**인지 확인하십시오. 
   1. `kube-system` 네임스페이스에 팟(Pod)을 나열하십시오.
      ```
    kubectl get pods -n kube-system 
      ```
      {: pre}
      
   2. 팟(Pod)의 상태가 **실행 중**이 아니면 팟(Pod)에 대한 세부사항을 가져와서 근본 원인을 찾으십시오. 팟(Pod)의 상태에 따라 다음 명령을 모두 실행하지 못할 수 있습니다.
```
      kubectl describe pod <pod_name> -n kube-system
      ```
      {: pre}
      
      ```
      kubectl logs <pod_name> -n kube-system
      ```
      {: pre}
      
   3. `kubectl describe pod` 명령의 CLI 출력에 대한 **이벤트** 섹션 및 최신 로그를 분석하여 오류에 대한 근본 원인을 찾으십시오.  
   
6. PVC가 프로비저닝되었는지 확인하십시오.  
   1. PVC의 상태를 확인하십시오. PVC의 상태가 **바인딩됨**이면 PVC가 프로비저닝된 것입니다.
      ```
    kubectl get pvc
      ```
      {: pre}
      
   2. PVC의 상태가 **보류 중**이면 오류를 검색하여 PVC가 보류 중인 이유를 알아보십시오.
      ```
      kubectl describe pvc <pvc_name>
      ```
      {: pre}
      
   3. PVC 작성 중에 발생할 수 있는 공통 오류를 검토하십시오.  
      - [파일 스토리지 및 블록 스토리지: PVC 상태는 보류 중임](#file_pvc_pending)
      - [오브젝트 스토리지: PVC 상태는 보류 중임](#cos_pvc_pending)
   
7. 스토리지 인스턴스를 마운트하는 팟(Pod)이 배치되었는지 확인하십시오.  
   1. 클러스터 내의 팟(Pod)을 나열하십시오. 팟(Pod)의 상태가 **실행 중**이면 팟(Pod)이 프로비저닝된 것입니다.
      ```
      kubectl get pods
      ```
      {: pre}
      
   2. 팟(Pod)에 대한 세부사항을 가져와서 오류가 CLI 출력의 **이벤트** 섹션에 표시되어 있는지 확인하십시오.
      ```
        kubectl describe pod <pod_name>
      ```
      {: pre}
   
   3. 앱의 로그를 검색하고 오류 메시지를 볼 수 있는지 확인하십시오.
      ```
      kubectl logs <pod_name>
      ```
      {: pre}
   
   4. PVC를 앱에 마운트할 때 발생할 수 있는 공통 오류를 검토하십시오.  
      - [파일 스토리지: 앱이 PVC에 액세스하거나 쓸 수 없음](#file_app_failures)
      - [블록 스토리지: 앱이 PVC에 액세스하거나 쓸 수 없음](#block_app_failures)
      - [오브젝트 스토리지: 루트가 아닌 사용자로 파일 액세스에 실패함](#cos_nonroot_access)
      

## 파일 스토리지 및 블록 스토리지: PVC 상태는 보류 중임
{: #file_pvc_pending}

{: tsSymptoms}
PVC를 작성하고 `kubectl get pvc <pvc_name>`를 실행하는 경우 얼마 동안 기다린 후에도 PVC의 상태는 **보류 중**입니다.  

{: tsCauses}
PVC 작성 및 바인딩 중에 많은 다른 태스크가 파일 및 블록 스토리지 플러그인에 의해 실행됩니다. 각 태스크는 실패할 수 있으며 다른 오류 메시지가 발생할 수 있습니다. 

{: tsResolve}

1. PVC의 상태가 **보류 중**인 이유에 대한 근본 원인을 찾으십시오.  
   ```
   kubectl describe pvc <pvc_name> -n <namespace>
   ```
   {: pre}
   
2. 공통 오류 메시지 설명 및 해결 방법을 검토하십시오. 
   
   <table>
   <thead>
     <th>오류 메시지</th>
     <th>설명</th>
     <th>해결 단계</th>
  </thead>
  <tbody>
    <tr>
      <td><code>User doesn't have permissions to create or manage Storage</code></br></br><code>Failed to find any valid softlayer credentials in configuration file</code></br></br><code>Storage with the order ID %d could not be created after retrying for %d seconds.</code></br></br><code>Unable to locate datacenter with name <datacenter_name>.</code></td>
      <td>IAM API 키 또는 클러스터의 `storage-secret-store` Kubernetes 시크릿에 저장된 IBM Cloud 인프라(SoftLayer) API 키에는 지속적 스토리지를 프로비저닝하는 데 필요한 모든 권한이 없습니다. </td>
      <td>[누락된 권한으로 인한 PVC 작성 실패](#missing_permissions)를 참조하십시오. </td>
    </tr>
    <tr>
      <td><code>Your order will exceed the maximum number of storage volumes allowed. Please contact Sales</code></td>
      <td>모든 {{site.data.keyword.Bluemix_notm}} 계정은 작성될 수 있는 최대 스토리지 인스턴스 수로 설정됩니다. PVC를 작성하면 최대 스토리지 인스턴스 수를 초과합니다. </td>
      <td>PVC를 작성하려면 다음 옵션 중에서 선택하십시오.<ul><li>사용하지 않는 PVC를 제거하십시오.</li><li>스토리지 할당량을 늘리려면 [지원 케이스를 열어](/docs/get-support?topic=get-support-getting-customer-support) {{site.data.keyword.Bluemix_notm}} 계정 소유자에게 요청하십시오. </li></ul> </td>
    </tr>
    <tr>
      <td><code>Unable to find the exact ItemPriceIds(type|size|iops) for the specified storage</code> </br></br><code>Failed to place storage order with the storage provider</code></td>
      <td>PVC에 지정한 스토리지 크기 및 IOPS는 선택한 스토리지 유형에서 지원되지 않으며 지정된 스토리지 클래스에서 사용할 수 없습니다.</td>
      <td>사용할 스토리지 클래스에 대해 지원되는 스토리지 크기와 IOPS를 찾으려면 [파일 스토리지 구성 결정](/docs/containers?topic=containers-file_storage#file_predefined_storageclass) 및 [블록 스토리지 구성 결정](/docs/containers?topic=containers-block_storage#block_predefined_storageclass)을 검토하십시오. 크기 및 IOPS를 정정하고 PVC를 다시 작성하십시오. </td>
    </tr>
    <tr>
  <td><code>Failed to find the datacenter name in configuration file</code></td>
      <td>PVC에 지정한 데이터 센터는 존재하지 않습니다. </td>
  <td>사용 가능한 데이터 센터를 나열하려면 <code>ibmcloud ks locations</code>를 실행하십시오. PVC에 있는 데이터 센터를 정정하고 PVC를 다시 작성하십시오. </td>
    </tr>
    <tr>
  <td><code>Failed to place storage order with the storage provider</code></br></br><code>Storage with the order ID 12345 could not be created after retrying for xx seconds. </code></br></br><code>Failed to do subnet authorizations for the storage 12345.</code><code>Storage 12345 has ongoing active transactions and could not be completed after retrying for xx seconds.</code></td>
  <td>스토리지 크기, IOPS 및 스토리지 유형은 선택한 스토리지 클래스와 호환되지 않을 수 있거나 {{site.data.keyword.Bluemix_notm}} 인프라 API 엔드포인트는 현재 사용할 수 없습니다. </td>
  <td>사용할 스토리지 클래스 및 스토리지 유형에 대해 지원되는 스토리지 크기와 IOPS를 찾으려면 [파일 스토리지 구성 결정](/docs/containers?topic=containers-file_storage#file_predefined_storageclass) 및 [블록 스토리지 구성 결정](/docs/containers?topic=containers-block_storage#block_predefined_storageclass)을 검토하십시오. 그런 다음 PVC를 삭제한 후 다시 작성하십시오. </td>
  </tr>
  <tr>
  <td><code>Failed to find the storage with storage id 12345. </code></td>
  <td>Kubernetes 정적 프로비저닝을 사용하여 기존 스토리지 인스턴스에 대한 PVC를 작성하려고 하지만 지정한 스토리지 인스턴스를 찾을 수 없습니다.</td>
  <td>클러스터에서 기존 [파일 스토리지](/docs/containers?topic=containers-file_storage#existing_file) 또는 [블록 스토리지](/docs/containers?topic=containers-block_storage#existing_block)를 프로비저닝하려면 지시사항을 따르고 기존 스토리지 인스턴스에 대한 올바른 정보를 검색해야 합니다. 그런 다음 PVC를 삭제한 후 다시 작성하십시오. </td>
  </tr>
  <tr>
  <td><code>Storage type not provided, expected storage type is `Endurance` or `Performance`. </code></td>
  <td>사용자 정의 스토리지 클래스를 작성했으며 지원되지 않는 스토리지 유형을 지정했습니다.</td>
  <td>사용자 정의 스토리지 클래스를 업데이트하여 스토리지 유형으로 `Endurance` 또는 `Performance`를 지정하십시오. 사용자 정의 스토리지 클래스에 대한 예를 찾으려면 [파일 스토리지](/docs/containers?topic=containers-file_storage#file_custom_storageclass) 및 [블록 스토리지](/docs/containers?topic=containers-block_storage#block_custom_storageclass)에 대한 샘플 사용자 정의 스토리지 클래스를 참조하십시오. </td>
  </tr>
  </tbody>
  </table>
  
## 파일 스토리지: 앱이 PVC에 액세스하거나 쓸 수 없음
{: #file_app_failures}

PVC를 팟(Pod)에 마운트하는 경우 PVC에 액세스하거나 쓸 때 오류가 발생할 수 있습니다.
{: shortdesc}

1. 클러스터 내의 팟(Pod)을 나열하고 팟(Pod)의 상태를 검토하십시오.  
   ```
   kubectl get pods
   ```
   {: pre}
   
2. 앱이 PVC에 액세스하거나 쓸 수 없는 이유에 대한 근본 원인을 찾으십시오. 
   ```
   kubectl describe pod <pod_name>
   ```
   {: pre}
   
   ```
   kubectl logs <pod_name>
   ```
   {: pre}

3. PVC를 팟(Pod)에 마운트할 때 발생할 수 있는 공통 오류를 검토하십시오.  
   <table>
   <thead>
     <th>증상 또는 오류 메시지</th>
     <th>설명</th>
     <th>해결 단계</th>
  </thead>
  <tbody>
    <tr>
      <td>팟(Pod)의 상태는 <strong>ContainerCreating</strong>입니다. </br></br><code>MountVolume.SetUp failed for volume ... read-only file system</code></td>
      <td>{{site.data.keyword.Bluemix_notm}} 인프라 백엔드에서 네트워크 문제가 발생했습니다. 데이터를 보호하고 데이터 손상을 방지하기 위해 {{site.data.keyword.Bluemix_notm}}는 NFS 파일 공유에 대한 쓰기 오퍼레이션이 발생하지 않도록 자동으로 파일 스토리지 서버의 연결을 끊습니다. </td>
      <td>[파일 스토리지: 작업자 노드에 대한 파일 시스템이 읽기 전용으로 변경됨](#readonly_nodes)</td>
      </tr>
      <tr>
  <td><code>write-permission</code> </br></br><code>do not have required permission</code></br></br><code>cannot create directory '/bitnami/mariadb/data': Permission denied </code></td>
  <td>배치에서 NFS 파일 스토리지 마운트 경로를 소유하기 위해 루트가 아닌 사용자를 지정했습니다. 기본적으로, 루트가 아닌 사용자에게는 NFS 지원 스토리지의 볼륨 마운트 경로에 대한 쓰기 권한이 없습니다. </td>
  <td>파일 스토리지: 루트가 아닌 사용자가 NFS 파일 스토리지 마운트 경로를 소유하면 앱에 장애가 발생함</td>
  </tr>
  <tr>
  <td>NFS 파일 스토리지 마운트 경로를 소유하기 위해 루트가 아닌 사용자를 지정하거나 지정된 루트가 아닌 사용자 ID로 Helm 차트를 배치한 후 사용자가 마운트된 스토리지에 쓸 수 없습니다.</td>
  <td>배치 또는 Helm 차트 구성은 팟(Pod)의 <code>fsGroup</code>(그룹 ID) 및 <code>runAsUser</code>(사용자 ID)에 대한 보안 컨텍스트를 지정합니다. </td>
  <td>[파일 스토리지: 지속적 스토리지에 루트가 아닌 사용자 액세스를 추가할 수 없음](#cs_storage_nonroot)을 참조하십시오. </td>
  </tr>
</tbody>
</table>

### 파일 스토리지: 작업자 노드의 파일 시스템이 읽기 전용으로 변경됨
{: #readonly_nodes}

{: tsSymptoms}
{: #stuck_creating_state}
다음 증상 중 하나가 표시될 수 있습니다.
- `kubectl get pods -o wide`를 실행할 때 동일한 작업자 노드에서 실행 중인 여러 팟(Pod)이 계속 `ContainerCreating` 상태에 있음을 알게 됩니다.
- `kubectl describe` 명령을 실행할 때 **Events** 섹션에 `MountVolume.SetUp failed for volume ... read-only file system` 오류가 표시됩니다.

{: tsCauses}
작업자 노드의 파일 시스템은 읽기 전용입니다.

{: tsResolve}
1.  작업자 노드 또는 컨테이너에 저장될 수 있는 데이터를 백업하십시오.
2.  기존 작업자 노드에 대한 단기 수정사항의 경우 작업자 노드를 다시 로드하십시오.
    <pre class="pre"><code>ibmcloud ks worker-reload --cluster &lt;cluster_name&gt; --worker &lt;worker_ID&gt;</code></pre>

장기 수정사항의 경우에는 [작업자 풀의 머신 유형을 업데이트](/docs/containers?topic=containers-update#machine_type)하십시오.

<br />



### 파일 스토리지: 루트가 아닌 사용자가 NFS 파일 스토리지 마운트 경로를 소유하면 앱에 장애가 발생함
{: #nonroot}

{: tsSymptoms}
배치에 [NFS 스토리지를 추가](/docs/containers?topic=containers-file_storage#file_app_volume_mount)한 후 컨테이너의 배치가 실패합니다. 컨테이너의 로그를 검색할 때 다음 과 같은 오류가 표시될 수 있습니다. 팟(Pod)에 장애가 발생하여 다시 로드 순환에서 벗어나지 못합니다.

```
write-permission
```
{: screen}

```
do not have required permission
```
{: screen}

```
cannot create directory '/bitnami/mariadb/data': Permission denied
```
{: screen}

{: tsCauses}
기본적으로, 루트가 아닌 사용자에게는 NFS 지원 스토리지의 볼륨 마운트 경로에 대한 쓰기 권한이 없습니다. Jenkins 및 Nexus3과 같은 일부 일반적인 앱 이미지에서는 Dockerfile에 마운트 경로를 소유하는 루트가 아닌 사용자를 지정합니다. 이 Dockerfile에서 컨테이너를 작성하는 경우 마운트 경로에 루트가 아닌 사용자에 대한 권한이 충분하지 않기 때문에 컨테이너의 작성에 실패합니다. 쓰기 권한을 부여하기 위해 마운트 경로 권한을 변경하기 전에 Dockerfile 파일을 수정하여 임시로 루트가 아닌 사용자를 루트 사용자 그룹에 추가하거나 init 컨테이너를 사용할 수 있습니다.

Helm 차트를 사용하여 이미지를 배치하는 경우 init 컨테이너를 사용하도록 Helm 배치를 편집하십시오.
{:tip}



{: tsResolve}
배치에 [init 컨테이너 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/)를 포함하는 경우 컨테이너 내부의 볼륨 마운트 경로에 대해 Dockerfile 쓰기 권한에 지정된 루트가 아닌 사용자를 제공할 수 있습니다. 앱 컨테이너가 시작되기 전에 init 컨테이너가 시작됩니다. init 컨테이너는 컨테이너 내부에 볼륨 마운트 경로를 작성하고 올바른(루트가 아닌) 사용자가 소유하도록 마운트 경로를 변경한 후 닫습니다. 그런 다음, 마운트 경로에 쓰기를 수행해야 하는 루트가 아닌 사용자가 포함된 앱 컨테이너가 시작됩니다. 루트가 아닌 사용자가 경로를 이미 소유하고 있기 때문에 마운트 경로 쓰기에 성공합니다. init 컨테이너를 사용하지 않을 경우 Dockerfile을 수정하여 루트가 아닌 사용자 액세스를 NFS 파일 스토리지에 추가할 수 있습니다.


시작하기 전에: [계정에 로그인하십시오. 해당되는 경우, 적절한 리소스 그룹을 대상으로 지정하십시오. 클러스터의 컨텍스트를 설정하십시오.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  앱의 Dockerfile을 열고 볼륨 마운트 경로에 대한 쓰기 권한을 제공하려는 사용자로부터 사용자 ID(UID) 및 그룹 ID(GID)를 가져오십시오. Jenkins Dockerfile의 예에서 정보는 다음과 같습니다.
    - UID: `1000`
    - GID: `1000`

    **예제**:

    ```
    FROM openjdk:8-jdk

    RUN apt-get update &&apt-get install -y git curl &&rm -rf /var/lib/apt/lists/*

    ARG user=jenkins
    ARG group=jenkins
    ARG uid=1000
    ARG gid=1000
    ARG http_port=8080
    ARG agent_port=50000

    ENV JENKINS_HOME /var/jenkins_home
    ENV JENKINS_SLAVE_AGENT_PORT ${agent_port}
    ...
    ```
    {:screen}

2.  지속적 볼륨 클레임(PVC)을 작성하여 지속적 스토리지를 앱에 추가하십시오. 이 예에서는 `ibmc-file-bronze` 스토리지 클래스를 사용합니다. 사용 가능한 스토리지 클래스를 검토하려면 `kubectl get storageclasses`를 실행하십시오.

    ```
    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
      name: mypvc
      annotations:
        volume.beta.kubernetes.io/storage-class: "ibmc-file-bronze"
    spec:
      accessModes:
        - ReadWriteMany
      resources:
        requests:
          storage: 20Gi
    ```
    {: codeblock}

3.  PVC를 작성하십시오.

    ```
    kubectl apply -f mypvc.yaml
    ```
    {: pre}

4.  배치 `.yaml` 파일에서 init 컨테이너를 추가하십시오. 이전에 검색한 UID 및 GID를 포함하십시오.

    ```
    initContainers:
    - name: initcontainer # Or replace the name
      image: alpine:latest
      command: ["/bin/sh", "-c"]
      args:
        - chown <UID>:<GID> /mount; # Replace UID and GID with values from the Dockerfile
      volumeMounts:
      - name: volume # Or you can replace with any name
        mountPath: /mount # Must match the mount path in the args line
    ```
    {: codeblock}

    Jenkins 배치의 **예**:

    ```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: my_pod
    spec:
      replicas: 1
      selector:
        matchLabels:
          app: jenkins      
      template:
        metadata:
          labels:
            app: jenkins
        spec:
          containers:
          - name: jenkins
            image: jenkins
            volumeMounts:
            - mountPath: /var/jenkins_home
              name: volume
          volumes:
          - name: volume
            persistentVolumeClaim:
              claimName: mypvc
          initContainers:
          - name: permissionsfix
            image: alpine:latest
            command: ["/bin/sh", "-c"]
            args:
              - chown 1000:1000 /mount;
            volumeMounts:
            - name: volume
              mountPath: /mount
    ```
    {: codeblock}

5.  팟(Pod)을 작성하고 PVC를 팟(Pod)에 마운트하십시오.

    ```
    kubectl apply -f my_pod.yaml
    ```
    {: pre}

6. 볼륨이 팟(Pod)에 정상적으로 마운트되었는지 확인하십시오. 팟(Pod) 이름 및 **컨테이너/마운트** 경로를 기록해 두십시오.

    ```
    kubectl describe pod <my_pod>
    ```
    {: pre}

    **출력 예**:

    ```
    Name:       mypod-123456789
    Namespace:	default
    ...
    Init Containers:
    ...
    Mounts:
      /mount from volume (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-cp9f0 (ro)
    ...
    Containers:
      jenkins:
        Container ID:
        Image:		jenkins
        Image ID:
        Port:		  <none>
        State:		Waiting
          Reason:		PodInitializing
        Ready:		False
        Restart Count:	0
        Environment:	<none>
        Mounts:
          /var/jenkins_home from volume (rw)
          /var/run/secrets/kubernetes.io/serviceaccount from default-token-cp9f0 (ro)
    ...
    Volumes:
      myvol:
        Type:	PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
        ClaimName:	mypvc
        ReadOnly:	  false

    ```
    {: screen}

7.  이전에 기록해 놓은 팟(Pod) 이름을 사용하여 팟(Pod)에 로그인하십시오.

    ```
    kubectl exec -it <my_pod-123456789> /bin/bash
    ```
    {: pre}

8. 컨테이너의 마운트 경로에 대한 권한을 확인하십시오. 예를 들어, 마운트 경로는 `/var/jenkins_home`입니다.

    ```
    ls -ln /var/jenkins_home
    ```
    {: pre}

    **출력 예**:

    ```
    jenkins@mypod-123456789:/$ ls -ln /var/jenkins_home
    total 12
    -rw-r--r-- 1 1000 1000  102 Mar  9 19:58 copy_reference_file.log
    drwxr-xr-x 2 1000 1000 4096 Mar  9 19:58 init.groovy.d
    drwxr-xr-x 9 1000 1000 4096 Mar  9 20:16 war
    ```
    {: screen}

    이 출력은 Dockerfile의 GID 및 UID(이 예에서 `1000` 및 `1000`)가 컨테이너 내부의 마운트 경로를 소유함을 표시합니다.

<br />


### 파일 스토리지: 지속적 스토리지에 루트가 아닌 사용자 액세스를 추가할 수 없음
{: #cs_storage_nonroot}

{: tsSymptoms}
[지속적 스토리지에 루트가 아닌 사용자 액세스를 추가](#nonroot)하거나 지정된 루트가 아닌 사용자 ID로 Helm 차트를 배치한 후 사용자가 마운트된 스토리지에 쓸 수 없습니다.

{: tsCauses}
배치 또는 Helm 차트 구성은 팟(Pod)의 `fsGroup`(그룹 ID) 및 `runAsUser`(사용자 ID)에 대한 [보안 컨텍스트](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/)를 지정합니다. 현재 {{site.data.keyword.containerlong_notm}}는 `fsGroup` 스펙을 지원하지 않으며 `0`으로 설정된 `runAsUser`(루트 권한)만 지원합니다.

{: tsResolve}
이미지, 배치 또는 Helm 차트 구성 파일 및 다시 배치에서 `fsGroup` 및 `runAsUser`에 대한 구성의 `securityContext` 필드를 제거하십시오. `nobody`에서 마운트 경로의 소유권을 변경해야 하는 경우 [루트가 아닌 사용자 액세스 추가](#nonroot)를 수행하십시오. [루트가 아닌 `initContainer`](#nonroot)를 추가한 후에는 팟(Pod) 레벨이 아니라 컨테이너 레벨에서 `runAsUser`를 설정하십시오.

<br />




## 블록 스토리지: 앱이 PVC에 액세스하거나 쓸 수 없음
{: #block_app_failures}

PVC를 팟(Pod)에 마운트하는 경우 PVC에 액세스하거나 쓸 때 오류가 발생할 수 있습니다.
{: shortdesc}

1. 클러스터 내의 팟(Pod)을 나열하고 팟(Pod)의 상태를 검토하십시오.  
   ```
   kubectl get pods
   ```
   {: pre}
   
2. 앱이 PVC에 액세스하거나 쓸 수 없는 이유에 대한 근본 원인을 찾으십시오. 
   ```
        kubectl describe pod <pod_name>
   ```
   {: pre}
   
   ```
   kubectl logs <pod_name>
   ```
   {: pre}

3. PVC를 팟(Pod)에 마운트할 때 발생할 수 있는 공통 오류를 검토하십시오.  
   <table>
   <thead>
     <th>증상 또는 오류 메시지</th>
     <th>설명</th>
     <th>해결 단계</th>
  </thead>
  <tbody>
    <tr>
      <td>팟(Pod)의 상태는 <strong>ContainerCreating</strong> 또는 <strong>CrashLoopBackOff</strong>입니다.</br></br><code>MountVolume.SetUp failed for volume ... read-only.</code></td>
      <td>{{site.data.keyword.Bluemix_notm}} 인프라 백엔드에서 네트워크 문제가 발생했습니다. 데이터를 보호하고 데이터 손상을 방지하기 위해 {{site.data.keyword.Bluemix_notm}}는 블록 스토리지 인스턴스에 대한 쓰기 오퍼레이션이 발생하지 않도록 자동으로 블록 스토리지 서버의 연결을 끊습니다. </td>
      <td>[블록 스토리지: 블록 스토리지가 읽기 전용으로 변경됨](#readonly_block)을 참조하십시오.</td>
      </tr>
      <tr>
  <td><code>failed to mount the volume as "ext4", it already contains xfs. Mount error: mount failed: exit status 32</code> </td>
        <td><code>XFS</code> 파일 시스템을 사용하여 설정된 기존 블록 스토리지 인스턴스를 마운트하려고 합니다. PVC와 일치하는 PV를 작성한 경우 <code>ext4</code>를 지정했거나 파일 시스템을 지정하지 않았습니다. PV에서 지정하는 파일 시스템은 블록 스토리지 인스턴스에 설정된 파일 시스템과 동일해야 합니다. </td>
  <td> [블록 스토리지: 파일 시스템 오류로 인해 기존 블록 스토리지를 팟(Pod)에 마운트할 수 없음](#block_filesystem)을 참조하십시오. </td>
  </tr>
</tbody>
</table>

### 블록 스토리지: 블록 스토리지가 읽기 전용으로 변경됨
{: #readonly_block}

{: tsSymptoms}
다음과 같은 증상이 표시될 수 있습니다.
- `kubectl get pods -o wide`를 실행할 때 동일한 작업자 노드에서 여러 팟(Pod)이 계속 `ContainerCreating` 또는 `CrashLoopBackOff` 상태에 있음을 알게 됩니다. 이러한 모든 팟(Pod)은 동일한 블록 스토리지 인스턴스를 사용합니다.
- `kubectl describe pod` 명령을 실행할 때 **이벤트** 섹션에 `MountVolume.SetUp failed for volume ... read-only` 오류가 표시됩니다.

{: tsCauses}
팟(Pod)이 볼륨에 기록하는 동안 네트워크 오류가 발생하면 IBM Cloud 인프라(SoftLayer)는 볼륨을 읽기 전용 모드로 변경하여 볼륨에 있는 데이터가 손상되지 않도록 보호합니다. 이 볼륨을 사용하는 팟(Pod)은 볼륨에 계속 기록할 수 없으며 실패합니다.

{: tsResolve}
1. 클러스터에 설치한 {{site.data.keyword.Bluemix_notm}} Block Storage 플러그인 버전을 확인하십시오.
   ```
   helm ls
   ```
   {: pre}

2. [{{site.data.keyword.Bluemix_notm}} Block Storage 플러그인의 최신 버전](https://cloud.ibm.com/kubernetes/solutions/helm-charts/ibm/ibmcloud-block-storage-plugin)을 사용하는지 확인하십시오. 그렇지 않은 경우에는 [플러그인을 업데이트](/docs/containers?topic=containers-block_storage#updating-the-ibm-cloud-block-storage-plug-in)하십시오.
3. 팟(Pod)에 대해 Kubernetes 배치를 사용한 경우 팟(Pod)을 제거하고 Kubernetes가 이를 다시 작성하도록 하여 실패한 팟(Pod)을 다시 시작하십시오. 배치를 사용하지 않은 경우, `kubectl get pod <pod_name> -o yaml >pod.yaml`을 실행하여 팟(Pod)을 작성하는 데 사용된 YAML 파일을 검색하십시오. 그런 다음, 팟(Pod)을 삭제하고 수동으로 다시 작성하십시오.
    ```
      kubectl delete pod <pod_name>
    ```
    {: pre}

4. 팟(Pod)을 다시 작성하여 문제가 해결되는지 확인하십시오. 그렇지 않으면, 작업자 노드를 다시 로드하십시오.
   1. 팟(Pod)이 실행되는 작업자 노드를 찾아 작업자 노드에 지정된 사설 IP 주소를 기록해 두십시오.
      ```
      kubectl describe pod <pod_name> | grep Node
      ```
      {: pre}

      출력 예:
      ```
      Node:               10.75.XX.XXX/10.75.XX.XXX
      Node-Selectors:  <none>
      ```
      {: screen}

   2. 이전 단계의 사설 IP 주소를 사용하여 작업자 노드의 **ID**를 검색하십시오.
      ```
   ibmcloud ks workers --cluster <cluster_name_or_ID>
      ```
      {: pre}

   3. 올바르게 [작업자 노드를 다시 로드](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)하십시오.


<br />


### 블록 스토리지: 파일 시스템 오류로 인해 기존 블록 스토리지를 팟(Pod)에 마운트할 수 없음
{: #block_filesystem}

{: tsSymptoms}
`kubectl describe pod <pod_name>`을 실행할 때 다음 오류가 표시됩니다.
```
failed to mount the volume as "ext4", it already contains xfs. Mount error: mount failed: exit status 32
```
{: screen}

{: tsCauses}
`XFS` 파일 시스템을 사용하여 설정된 기존 블록 스토리지 디바이스가 있습니다. 이 디바이스를 팟(Pod)에 마운트하기 위해 `spec/flexVolume/fsType`에서 `ext4`를 파일 시스템으로 지정하거나 파일 시스템을 지정하지 않은 [PV를 작성](/docs/containers?topic=containers-block_storage#existing_block)했습니다. 파일 시스템이 정의되지 않으면 기본적으로 PV가 `ext4`로 설정됩니다.
PV가 성공적으로 작성되고 기존 블록 스토리지 인스턴스에 링크되었습니다. 그러나 일치하는 PVC를 사용하여 PV를 클러스터에 마운트하려고 하면 볼륨 마운트에 실패합니다. `ext4` 파일 시스템을 사용하는 `XFS` 블록 스토리지 인스턴스를 팟(Pod)에 마운트할 수 없습니다.

{: tsResolve}
기존 PV의 파일 시스템을 `ext4`에서 `XFS`로 업데이트하십시오.

1. 클러스터에 있는 기존 PV를 나열하고 기존 블록 스토리지 인스턴스에 사용한 PV의 이름을 기록해 두십시오.
   ```
   kubectl get pv
   ```
   {: pre}

2. 로컬 머신에 PV YAML을 저장하십시오.
   ```
   kubectl get pv <pv_name> -o yaml > <filepath/xfs_pv.yaml>
   ```
   {: pre}

3. YAML 파일을 열고 `fsType`을 `ext4`에서 `xfs`로 변경하십시오.
4. 클러스터의 PV를 대체하십시오.
   ```
   kubectl replace --force -f <filepath/xfs_pv.yaml>
   ```
   {: pre}

5. PV를 마운트한 팟(Pod)에 로그인하십시오.
   ```
   kubectl exec -it <pod_name> sh
   ```
   {: pre}

6. 파일 시스템이 `XFS`로 변경되었는지 확인하십시오.
   ```
   df -Th
   ```
   {: pre}

   출력 예:
   ```
   Filesystem Type Size Used Avail Use% Mounted on /dev/mapper/3600a098031234546d5d4c9876654e35 xfs 20G 33M 20G 1% /myvolumepath
   ```
   {: screen}

<br />



## 오브젝트 스토리지: {{site.data.keyword.cos_full_notm}} `ibmc` Helm 플러그인 설치에 실패함
{: #cos_helm_fails}

{: tsSymptoms}
{{site.data.keyword.cos_full_notm}} `ibmc` Helm 플러그인을 설치하면 다음 오류 중 하나로 설치에 실패합니다.
```
Error: symlink /Users/ibm/ibmcloud-object-storage-plugin/helm-ibmc /Users/ibm/.helm/plugins/helm-ibmc: file exists
```
{: screen}

```
Error: fork/exec /home/iksadmin/.helm/plugins/helm-ibmc/ibmc.sh: permission denied
```
{: screen}

{: tsCauses}
`ibmc` Helm 플러그인이 설치되면 `./helm/plugins/helm-ibmc` 디렉토리에서 `ibmc` Helm 플러그인이 시스템에 위치한 디렉토리(일반적으로 `./ibmcloud-object-storage-plugin/helm-ibmc`에 있음)로 symlink가 작성됩니다. 로컬 시스템에서 `ibmc` Helm 플러그인을 제거하거나 `ibmc` Helm 플러그인 디렉토리를 다른 위치로 이동하는 경우, symlink가 제거되지 않습니다.

`permission denied` 오류가 표시되면 `ibmc` Helm 플러그인 명령을 실행하기 위해 `ibmc.sh` bash 파일에 필요한 `read`, `write` 및 `execute` 권한이 없는 것입니다.  

{: tsResolve}

**symlink 오류의 경우**: 

1. {{site.data.keyword.cos_full_notm}} Helm 플러그인을 제거하십시오.
   ```
   rm -rf ~/.helm/plugins/helm-ibmc
   ```
   {: pre}

2. `ibmc` Helm 플러그인 및 {{site.data.keyword.cos_full_notm}} 플러그인을 다시 설치하려면 [문서](/docs/containers?topic=containers-object_storage#install_cos)를 따르십시오. 

**권한 오류의 경우**: 

1. `ibmc` 플러그인에 대한 권한을 변경하십시오. 
   ```
   chmod 755 ~/.helm/plugins/helm-ibmc/ibmc.sh
   ```
   {: pre}
   
2. `ibm` Helm 플러그인을 시도해보십시오. 
   ```
   helm ibmc --help
   ```
   {: pre}
   
3. [{{site.data.keyword.cos_full_notm}} 플러그인 설치를 계속하십시오](/docs/containers?topic=containers-object_storage#install_cos). 


<br />


## 오브젝트 스토리지: PVC 상태는 보류 중임
{: #cos_pvc_pending}

{: tsSymptoms}
PVC를 작성하고 `kubectl get pvc <pvc_name>`를 실행하는 경우 얼마 동안 기다린 후에도 PVC의 상태는 **보류 중**입니다.  

{: tsCauses}
PVC 작성 및 바인딩 중에 많은 다른 태스크가 {{site.data.keyword.cos_full_notm}} 플러그인에 의해 실행됩니다. 각 태스크는 실패할 수 있으며 다른 오류 메시지가 발생할 수 있습니다. 

{: tsResolve}

1. PVC의 상태가 **보류 중**인 이유에 대한 근본 원인을 찾으십시오.  
   ```
   kubectl describe pvc <pvc_name> -n <namespace>
   ```
   {: pre}
   
2. 공통 오류 메시지 설명 및 해결 방법을 검토하십시오. 
   
   <table>
   <thead>
     <th>오류 메시지</th>
     <th>설명</th>
     <th>해결 단계</th>
  </thead>
  <tbody>
    <tr>
      <td>`User doesn't have permissions to create or manage Storage`</td>
      <td>IAM API 키 또는 클러스터의 `storage-secret-store` Kubernetes 시크릿에 저장된 IBM Cloud 인프라(SoftLayer) API 키에는 지속적 스토리지를 프로비저닝하는 데 필요한 모든 권한이 없습니다. </td>
      <td>[누락된 권한으로 인한 PVC 작성 실패](#missing_permissions)를 참조하십시오. </td>
    </tr>
    <tr>
      <td>`cannot get credentials: cannot get secret <secret_name>: secrets "<secret_name>" not found`</td>
      <td>{{site.data.keyword.cos_full_notm}} 서비스 인증 정보를 보유하는 Kubernetes 시크릿은 PVC 또는 팟(Pod)과 동일한 네임스페이스에 없습니다. </td>
      <td>[Kubernetes 시크릿을 찾을 수 없으므로 PVC 또는 팟(Pod) 작성에 실패함](#cos_secret_access_fails)을 참조하십시오.</td>
    </tr>
    <tr>
      <td>`cannot get credentials: Wrong Secret Type.Provided secret of type XXXX.Expected type ibm/ibmc-s3fs`</td>
      <td>{{site.data.keyword.cos_full_notm}} 서비스 인스턴스에 대해 작성한 Kubernetes 시크릿에는 `type: ibm/ibmc-s3fs`가 포함되지 않습니다.</td>
      <td>`type`을 `ibm/ibmc-s3fs`로 추가하거나 변경하려면 {{site.data.keyword.cos_full_notm}} 인증 정보를 보유하는 Kubernetes 시크릿을 편집하십시오. </td>
    </tr>
    <tr>
      <td>`Bad value for ibm.io/object-store-endpoint XXXX: scheme is missing. Must be of the form http://<hostname> or https://<hostname>`</br> </br> `Bad value for ibm.io/iam-endpoint XXXX: scheme is missing. Must be of the form http://<hostname< or https://<hostname>`</td>
      <td>s3fs API 또는 IAM API 엔드포인트의 형식은 올바르지 않거나 클러스터 위치를 기반으로 s3fs API 엔드포인트를 검색할 수 없습니다. </td>
      <td>[올바르지 않은 s3fs API 엔드포인트로 인해 PVC 작성에 실패함](#cos_api_endpoint_failure)을 참조하십시오.</td>
    </tr>
    <tr>
      <td>`object-path cannot be set when auto-create is enabled`</td>
      <td>`ibm.io/object-path` 어노테이션을 사용하여 PVC에 마운트할 버킷의 기존 서브디렉토리를 지정했습니다. 서브디렉토리를 설정한 경우 버킷 자동 작성 기능을 사용 안함으로 설정해야 합니다.</td>
      <td>PVC에서 `ibm.io/auto-create-bucket: "false"`를 설정하고 `ibm.io/bucket`에서 기존 버킷 이름을 제공하십시오.</td>
    </tr>
    <tr>
    <td>`bucket auto-create must be enabled when bucket auto-delete is enabled`</td>
    <td>PVC에서 PCV 제거 시 자동으로 데이터, 버킷 및 PV를 삭제하도록 `ibm.io/auto-delete-bucket: true`를 설정합니다. 이 옵션을 사용하려면 동시에 `ibm.io/auto-create-bucket`을 <strong>true</strong>로 설정하고 `ibm.io/bucket`을 `""`로 설정해야 합니다. </td>
    <td>PVC에서 `tmp-s3fs-xxxx` 형식의 이름으로 버킷이 자동 작성되도록 `ibm.io/auto-create-bucket: true` 및 `ibm.io/bucket: ""`을 설정하십시오.</td>
    </tr>
    <tr>
    <td>`bucket cannot be set when auto-delete is enabled`</td>
    <td>PVC에서 PCV 제거 시 자동으로 데이터, 버킷 및 PV를 삭제하도록 `ibm.io/auto-delete-bucket: true`를 설정합니다. 이 옵션을 사용하려면 동시에 `ibm.io/auto-create-bucket`을 <strong>true</strong>로 설정하고 `ibm.io/bucket`을 `""`로 설정해야 합니다. </td>
    <td>PVC에서 `tmp-s3fs-xxxx` 형식의 이름으로 버킷이 자동 작성되도록 `ibm.io/auto-create-bucket: true` 및 `ibm.io/bucket: ""`을 설정하십시오.</td>
    </tr>
    <tr>
    <td>`cannot create bucket using API key without service-instance-id`</td>
    <td>{{site.data.keyword.cos_full_notm}} 서비스 인스턴스에 액세스하는 데 IAM API 키를 사용할 경우 API 키 및 {{site.data.keyword.cos_full_notm}} 서비스 인스턴스 ID를 Kubernetes 시크릿에 저장해야 합니다. </td>
    <td>[오브젝트 스토리지 서비스 인증 정보의 시크릿 작성](/docs/containers?topic=containers-object_storage#create_cos_secret)을 참조하십시오. </td>
    </tr>
    <tr>
      <td>`object-path “<subdirectory_name>” not found inside bucket <bucket_name>`</td>
      <td>`ibm.io/object-path` 어노테이션을 사용하여 PVC에 마운트할 버킷의 기존 서브디렉토리를 지정했습니다. 이 서브디렉토리는 지정한 버킷에서 찾을 수 없습니다. </td>
      <td>`ibm.io/object-path`에 지정한 서브디렉토리가 `ibm.io/bucket`에 지정한 버킷에 존재하는지 확인하십시오. </td>
    </tr>
        <tr>
          <td>`BucketAlreadyExists: The requested bucket name is not available. The bucket namespace is shared by all users of the system. Please select a different name and try again.`</td>
          <td>동시에 `ibm.io/auto-create-bucket: true`를 설정하고 버킷 이름을 지정했거나 {{site.data.keyword.cos_full_notm}}에 이미 존재하는 버킷 이름을 지정했습니다. 버킷 이름은 {{site.data.keyword.cos_full_notm}}의 모든 서비스 인스턴스 및 지역에서 고유해야 합니다.  </td>
          <td>`ibm.io/auto-create-bucket: false`를 설정하고 {{site.data.keyword.cos_full_notm}}에 고유한 버킷 이름을 제공해야 합니다. 사용자에 적합한 버킷 이름을 자동으로 작성하도록 {{site.data.keyword.cos_full_notm}} 플러그인을 사용할 경우 `ibm.io/auto-create-bucket: true` 및 `ibm.io/bucket: ""`을 설정하십시오. 버킷은 `tmp-s3fs-xxxx` 형식의 고유한 이름으로 작성됩니다. </td>
        </tr>
        <tr>
          <td>`cannot access bucket <bucket_name>: NotFound: Not Found`</td>
          <td>작성하지 않은 버킷에 액세스를 시도했거나 지정한 스토리지 클래스 및 s3fs API 엔드포인트가 버킷 작성 시 사용한 스토리지 클래스 및 s3fs API 엔드포인트와 일치하지 않습니다.</td>
          <td>[기존 버켓에 액세스할 수 없음](#cos_access_bucket_fails)을 참조하십시오. </td>
        </tr>
        <tr>
          <td>`Put https://s3-api.dal-us-geo.objectstorage.service.networklayer.com/<bucket_name>: net/http: invalid header field value "AWS4-HMAC-SHA256 Credential=1234a12a123a123a1a123aa1a123a123\n\n/20190412/us-standard/s3/aws4_request, SignedHeaders=host;x-amz-content-sha256;x-amz-date, Signature=12aa1abc123456aabb12aas12aa123456sb123456abc" for key Authorization`</td>
          <td>Kubernetes 시크릿의 값이 base64로 올바르게 인코딩되지 않았습니다. </td>
          <td>Kubernetes 시크릿의 값을 검토하고 올바른 값으로 base64를 인코딩하십시오. [`kubectl create secret`](/docs/containers?topic=containers-object_storage#create_cos_secret) 명령을 사용하여 새 시크릿을 작성하고 Kubernetes를 통해 자동으로 값을 base64로 인코딩할 수도 있습니다. </td>
        </tr>
        <tr>
          <td>`cannot access bucket <bucket_name>: Forbidden: Forbidden`</td>
          <td>`ibm.io/auto-create-bucket: false`를 지정하고 작성하지 않은 버킷에 액세스를 시도했거나 HMAC 인증 정보가 올바르지 않은 {{site.data.keyword.cos_full_notm}}의 키 ID에 액세스를 시도했습니다. </td>
          <td>작성하지 않은 버킷에 액세스할 수 없습니다. `ibm.io/auto-create-bucket: true` 및 `ibm.io/bucket: ""`을 설정하여 대신 새 버킷을 작성하십시오. 버킷의 소유자인 경우 [잘못된 인증 정보 또는 액세스 거부로 인해 PVC 작성에 실패함](#cred_failure)을 참조하여 인증 정보를 확인하십시오. </td>
        </tr>
        <tr>
          <td>`cannot create bucket <bucket_name>: AccessDenied: Access Denied`</td>
          <td>{{site.data.keyword.cos_full_notm}}에서 버킷을 자동으로 작성하도록 `ibm.io/auto-create-bucket: true`를 지정했으나 Kubernetes 시크릿에서 제공한 인증 정보에는 **독자** IAM 서비스 액세스 역할이 지정됩니다. 이 역할이 지정되면 {{site.data.keyword.cos_full_notm}}에서 버킷을 작성할 수 없습니다. </td>
          <td>[잘못된 인증 정보 또는 액세스 거부로 인해 PVC 작성에 실패함](#cred_failure)을 참조하십시오. </td>
        </tr>
        <tr>
          <td>`cannot create bucket <bucket_name>: AccessForbidden: Access Forbidden`</td>
          <td>`ibm.io/auto-create-bucket: true`를 지정하고 `ibm.io/bucket`에서 기존 버킷 이름을 제공했습니다. 또한 Kubernetes 시크릿에서 제공한 인증 정보에는 **독자** IAM 서비스 액세스 역할이 지정됩니다. 이 역할이 지정되면 {{site.data.keyword.cos_full_notm}}에서 버킷을 작성할 수 없습니다. </td>
          <td>기존 버킷을 사용하려면 `ibm.io/auto-create-bucket: "false"`를 설정하고 `ibm.io/bucket`에서 기존 버킷 이름을 제공하십시오. 기존 Kubernetes 시크릿을 사용하여 버킷을 자동으로 작성하려면 `ibm.io/bucket: ""`을 설정하고 [잘못된 인증 정보 또는 액세스 거부로 인해 PVC 작성에 실패함](#cred_failure)을 따라 Kubernetes 시크릿의 인증 정보를 확인하십시오. </td>
        </tr>
        <tr>
          <td>`cannot create bucket <bucket_name>: SignatureDoesNotMatch: The request signature we calculated does not match the signature you provided. Check your AWS Secret Access Key and signing method. For more information, see REST Authentication and SOAP Authentication for details`</td>
          <td>Kubernetes 시크릿에서 제공한 HMAC 인증 정보의 {{site.data.keyword.cos_full_notm}} 시크릿 액세스 키가 올바르지 않습니다. </td>
          <td>[잘못된 인증 정보 또는 액세스 거부로 인해 PVC 작성에 실패함](#cred_failure)을 참조하십시오. </td>
        </tr>
        <tr>
          <td>`cannot create bucket <bucket_name>: InvalidAccessKeyId: The AWS Access Key ID you provided does not exist in our records`</td>
          <td>Kubernetes 시크릿에서 제공한 HMAC 인증 정보의 {{site.data.keyword.cos_full_notm}} 액세스 키 ID 또는 시크릿 액세스 키가 올바르지 않습니다. </td>
          <td>[잘못된 인증 정보 또는 액세스 거부로 인해 PVC 작성에 실패함](#cred_failure)을 참조하십시오. </td>
        </tr>
        <tr>
          <td>`cannot create bucket <bucket_name>: CredentialsEndpointError: failed to load credentials` </br> </br> `cannot access bucket <bucket_name>: CredentialsEndpointError: failed to load credentials`</td>
          <td>IAM 인증 정보의 {{site.data.keyword.cos_full_notm}} API 키 및 {{site.data.keyword.cos_full_notm}} 서비스 인스턴스의 GUID가 올바르지 않습니다. </td>
          <td>[잘못된 인증 정보 또는 액세스 거부로 인해 PVC 작성에 실패함](#cred_failure)을 참조하십시오. </td>
        </tr>
  </tbody>
    </table>
    

### 오브젝트 스토리지: Kubernetes 시크릿을 찾을 수 없으므로 PVC 또는 팟(Pod) 작성에 실패함
{: #cos_secret_access_fails}

{: tsSymptoms}
PVC를 작성하거나 PVC를 마운트하는 팟(Pod)을 배치할 때 작성이나 배치에 실패합니다.

- PVC 작성 실패에 대한 오류 메시지의 예:
  ```
  cannot get credentials: cannot get secret tsecret-key: secrets "secret-key" not found
  ```
  {: screen}

- 팟(Pod) 작성 실패에 대한 오류 메시지의 예:
  ```
  persistentvolumeclaim "pvc-3" not found (repeated 3 times)
  ```
  {: screen}

{: tsCauses}
{{site.data.keyword.cos_full_notm}} 서비스 인증 정보, PVC 및 팟(Pod)이 저장된 Kubernetes 시크릿이 모두 동일한 Kubernetes 네임스페이스에 있지 않습니다. 시크릿이 PVC 또는 팟(Pod)과 다른 네임스페이스에 배치되면 시크릿에 액세스할 수 없습니다.

{: tsResolve}
이 태스크에서는 네임스페이스에 대해 [**작성자** 또는 **관리자** {{site.data.keyword.Bluemix_notm}} IAM 서비스 역할](/docs/containers?topic=containers-users#platform)이 필요합니다.

1. 클러스터의 시크릿을 나열하고 {{site.data.keyword.cos_full_notm}} 서비스 인스턴스의 Kubernetes 시크릿이 작성된 Kubernetes 네임스페이스를 검토하십시오. 시크릿은 `ibm/ibmc-s3fs`를 **유형**으로 표시해야 합니다.
   ```
   kubectl get secrets --all-namespaces
   ```
   {: pre}

2. PVC 및 팟(Pod)의 YAML 구성 파일을 검사하여 동일한 네임스페이스가 사용되었는지 확인하십시오. 시크릿이 존재하는 네임스페이스와는 다른 네임스페이스에 팟(Pod)을 배치하려면 해당 네임스페이스에서 [다른 시크릿을 작성](/docs/containers?topic=containers-object_storage#create_cos_secret)하십시오.

3. PVC를 작성하거나 적절한 네임스페이스에 팟(Pod)을 배치하십시오.

<br />


### 오브젝트 스토리지: 잘못된 인증 정보 또는 액세스 거부로 인해 PVC 작성에 실패함
{: #cred_failure}

{: tsSymptoms}
PVC를 작성할 때 다음 중 하나와 유사한 오류 메시지가 나타납니다.

```
SignatureDoesNotMatch: The request signature we calculated does not match the signature you provided. Check your AWS Secret Access Key and signing method. For more information, see REST Authentication and SOAP Authentication for details.
```
{: screen}

```
AccessDenied: Access Denied status code: 403
```
{: screen}

```
CredentialsEndpointError: failed to load credentials
```
{: screen}

```
InvalidAccessKeyId: The AWS Access Key ID you provided does not exist in our records`
```
{: screen}

```
cannot access bucket <bucket_name>: Forbidden: Forbidden
```
{: screen}


{: tsCauses}
서비스 인스턴스에 액세스하기 위해 사용하는 {{site.data.keyword.cos_full_notm}} 서비스 인증 정보가 올바르지 않거나 버킷에 대한 읽기 액세스만 허용합니다.

{: tsResolve}
1. 서비스 세부사항 페이지의 탐색에서 **서비스 인증 정보**를 클릭하십시오.
2. 인증 정보를 찾은 후에 **인증 정보 보기**를 클릭하십시오.
3. **iam_role_crn** 섹션에서 `Writer` 또는 `Manager` 역할을 보유하는지 확인하십시오. 올바른 역할이 없는 경우에는 올바른 권한으로 새 {{site.data.keyword.cos_full_notm}} 서비스 인증 정보를 작성해야 합니다.  
4. 역할이 올바르면 Kubernetes 시크릿의 올바른 **access_key_id** 및 **secret_access_key**를 사용 중인지 확인하십시오.  
5. [업데이트된 **access_key_id** 및 **secret_access_key**를 사용하여 새 시크릿을 작성](/docs/containers?topic=containers-object_storage#create_cos_secret)하십시오. 


<br />


### 오브젝트 스토리지: 잘못된 s3fs 또는 IAM API 엔드포인트로 인해 PVC 작성에 실패함
{: #cos_api_endpoint_failure}

{: tsSymptoms}
PVC 작성 시 PVC의 상태는 보류 중입니다. `kubectl describe pvc <pvc_name>` 명령을 실행하면 다음 오류 메시지 중 하나가 표시됩니다.  

```
Bad value for ibm.io/object-store-endpoint XXXX: scheme is missing. Must be of the form http://<hostname> or https://<hostname>
```
{: screen}

```
Bad value for ibm.io/iam-endpoint XXXX: scheme is missing. Must be of the form http://<hostname> or https://<hostname>
```
{: screen}

{: tsCauses}
사용할 버킷에 대한 s3fs API 엔드포인트의 형식이 올바르지 않거나 클러스터가 {{site.data.keyword.containerlong_notm}}에서 지원되지만 아직 {{site.data.keyword.cos_full_notm}} 플러그인에서 지원되지 않는 위치에 배치됩니다.  

{: tsResolve}
1. {{site.data.keyword.cos_full_notm}} 플러그인 설치 중에 `ibmc` Helm 플러그인으로 스토리지 클래스에 자동으로 지정된 s3fs API 엔드포인트를 확인하십시오. 엔드포인트는 클러스터가 배치된 위치에 따라 달라집니다.   
   ```
   kubectl get sc ibmc-s3fs-standard-regional -o yaml | grep object-store-endpoint
   ```
   {: pre}

   명령으로 `ibm.io/object-store-endpoint: NA`가 리턴되면 클러스터는 {{site.data.keyword.containerlong_notm}}에서 지원되지만 아직 {{site.data.keyword.cos_full_notm}} 플러그인에서 지원되지 않는 위치에 배치됩니다. 위치를 {{site.data.keyword.containerlong_notm}}에 추가하려면 Slack에 공개적으로 질문을 게시하거나 {{site.data.keyword.Bluemix_notm}} 지원 케이스를 여십시오. 자세한 정보는 [도움 및 지원 받기](/docs/containers?topic=containers-cs_troubleshoot#ts_getting_help)를 참조하십시오. 
   
2. PVC에서 `ibm.io/endpoint` 어노테이션을 사용하여 s3fs API 엔드포인트를 수동으로 추가했거나 `ibm.io/iam-endpoint` 어노테이션을 사용하여 IAM API 엔드포인트를 수동으로 추가한 경우 `https://<s3fs_api_endpoint>` 및 `https://<iam_api_endpoint>`의 형식으로 엔드포인트를 추가했는지 확인하십시오. 어노테이션은 {{site.data.keyword.cos_full_notm}} 스토리지 클래스의 `ibmc` 플러그인에서 자동으로 설정된 API 엔드포인트를 겹쳐씁니다.  
   ```
   kubectl describe pvc <pvc_name>
   ```
   {: pre}
   
<br />


### 오브젝트 스토리지: 기존 버킷에 액세스할 수 없음
{: #cos_access_bucket_fails}

{: tsSymptoms}
PVC를 작성할 때 {{site.data.keyword.cos_full_notm}}의 버킷에 액세스할 수 없습니다. 다음과 유사한 오류 메시지가 표시될 수 있습니다.

```
Failed to provision volume with StorageClass "ibmc-s3fs-standard-regional": pvc:1b2345678b69175abc98y873e2:cannot access bucket <bucket_name>: NotFound: Not Found
```
{: screen}

{: tsCauses}
올바르지 않은 스토리지 클래스를 사용하여 기존 버킷에 액세스했거나 작성하지 않은 버킷에 액세스를 시도했을 수 있습니다. 작성하지 않은 버킷에 액세스할 수 없습니다.  

{: tsResolve}
1. [{{site.data.keyword.Bluemix_notm}} 대시보드 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/)에서 {{site.data.keyword.cos_full_notm}} 서비스 인스턴스를 선택하십시오.
2. **버킷**을 선택하십시오.
3. 기존 버킷의 **클래스** 및 **위치** 정보를 검토하십시오.
4. 적합한 [스토리지 클래스](/docs/containers?topic=containers-object_storage#cos_storageclass_reference)를 선택하십시오.
5. `ibm.io/auto-create-bucket: false`를 설정하고 기존 버킷의 올바른 이름을 제공해야 합니다.  

<br />


## 오브젝트 스토리지: 루트가 아닌 사용자로 파일 액세스에 실패함
{: #cos_nonroot_access}

{: tsSymptoms}
콘솔 또는 REST API를 사용하여 {{site.data.keyword.cos_full_notm}} 서비스 인스턴스에 파일을 업로드했습니다. 앱 배치에서 `runAsUser`로 정의된 루트가 아닌 사용자로 이러한 파일에 액세스를 시도하면 파일에 대한 액세스가 거부됩니다.

{: tsCauses}
Linux에서는 파일 또는 디렉토리에 3개의 액세스 그룹(`Owner`, `Group` 및 `Other`)이 있습니다. 콘솔 또는 REST API를 사용하여 {{site.data.keyword.cos_full_notm}}에 파일을 업로드하면 `Owner`, `Group` 및 `Other`에 대한 권한이 제거됩니다. 각 파일의 권한은 다음과 같이 나타날 수 있습니다.

```
d--------- 1 root root 0 Jan 1 1970 <file_name>
```
{: screen}

{{site.data.keyword.cos_full_notm}} 플러그인을 사용하여 파일을 업로드하면 파일에 대한 권한이 유지되며 변경되지 않습니다.

{: tsResolve}
루트가 아닌 사용자로 파일에 액세스하려면 해당 루트가 아닌 사용자에게 해당 파일에 대한 읽기 및 쓰기 권한이 있어야 합니다. 팟(Pod) 배치의 일부로서 파일에 대한 권한을 변경하려면 쓰기 권한이 필요합니다. {{site.data.keyword.cos_full_notm}}는 쓰기 워크로드용으로 디자인되지 않았습니다. 팟(Pod) 배치 중에 권한을 업데이트하면 팟(Pod)이 `Running` 상태가 되지 못할 수 있습니다.

이 문제를 해결하려면 PVC를 앱 팟(Pod)에 마운트하기 전에 다른 팟(Pod)을 작성하여 루트가 아닌 사용자에 대한 올바른 권한을 설정하십시오.

1. 버킷에서 파일의 권한을 확인하십시오.
   1. `test-permission` 팟(Pod)의 구성 파일을 작성하고 파일 이름을 `test-permission.yaml`로 지정하십시오.
      ```
      apiVersion: v1
      kind: Pod
      metadata:
        name: test-permission
      spec:
        containers:
        - name: test-permission
          image: nginx
          volumeMounts:
          - name: cos-vol
            mountPath: /test
        volumes:
        - name: cos-vol
          persistentVolumeClaim:
            claimName: <pvc_name>
      ```
      {: codeblock}

   2. `test-permission` 팟(Pod)을 작성하십시오.
      ```
      kubectl apply -f test-permission.yaml
      ```
      {: pre}

   3. 팟(Pod)에 로그인하십시오.
      ```
      kubectl exec test-permission -it bash
      ```
      {: pre}

   4. 마운트 경로로 이동하고 파일에 대한 권한을 나열하십시오.
      ```
      cd test && ls -al
      ```
      {: pre}

      출력 예:
      ```
      d--------- 1 root root 0 Jan 1 1970 <file_name>
      ```
      {: screen}

2. 팟(Pod)을 삭제하십시오.
   ```
   kubectl delete pod test-permission
   ```
   {: pre}

3. 파일의 권한 정정에 사용되는 팟(Pod)의 구성 파일을 작성하고 이름을 `fix-permission.yaml`로 지정하십시오.
   ```
   apiVersion: v1
   kind: Pod
   metadata:
     name: fix-permission 
     namespace: <namespace>
   spec:
     containers:
     - name: fix-permission
       image: busybox
       command: ['sh', '-c']
       args: ['chown -R <nonroot_userID> <mount_path>/*; find <mount_path>/ -type d -print -exec chmod u=+rwx,g=+rx {} \;']
       volumeMounts:
       - mountPath: "<mount_path>"
         name: cos-volume
     volumes:
     - name: cos-volume
       persistentVolumeClaim:
         claimName: <pvc_name>
    ```
    {: codeblock}

3. `fix-permission` 팟(Pod)을 작성하십시오.
   ```
   kubectl apply -f fix-permission.yaml
   ```
   {: pre}

4. 팟(Pod)이 `Completed` 상태가 될 때까지 대기하십시오.  
   ```
   kubectl get pod fix-permission
   ```
   {: pre}

5. `fix-permission` 팟(Pod)을 삭제하십시오.
   ```
   kubectl delete pod fix-permission
   ```
   {: pre}

5. 권한 확인을 위해 이전에 사용된 `test-permission` 팟(Pod)을 재작성하십시오.
   ```
   kubectl apply -f test-permission.yaml
   ```
   {: pre}

5. 파일에 대한 권한이 업데이트되었는지 확인하십시오.
   1. 팟(Pod)에 로그인하십시오.
      ```
      kubectl exec test-permission -it bash
      ```
      {: pre}

   2. 마운트 경로로 이동하고 파일에 대한 권한을 나열하십시오.
      ```
      cd test && ls -al
      ```
      {: pre}

      출력 예:
      ```
      -rwxrwx--- 1 <nonroot_userID> root 6193 Aug 21 17:06 <file_name>
      ```
      {: screen}

6. `test-permission` 팟(Pod)을 삭제하십시오.
   ```
   kubectl delete pod test-permission
   ```
   {: pre}

7. 루트가 아닌 사용자로 PVC를 앱에 마운트하십시오.

   동시에 배치 YAML에서 `fsGroup` 설정 없이 `runAsUser`로서 루트가 아닌 사용자를 정의하십시오. `fsGroup`을 설정하면 팟(Pod) 배치 시에 버킷의 모든 파일에 대한 그룹 권한을 업데이트하도록 {{site.data.keyword.cos_full_notm}} 플러그인이 트리거됩니다. 권한 업데이트는 쓰기 오퍼레이션이며, 이로 인해 팟(Pod)이 `Running` 상태가 되지 못할 수 있습니다.
   {: important}

{{site.data.keyword.cos_full_notm}} 서비스 인스턴스에서 올바른 파일 권한을 설정한 후에는 콘솔 또는 REST API를 사용하여 파일을 업로드하지 마십시오. {{site.data.keyword.cos_full_notm}} 플러그인을 사용하여 파일을 서비스 인스턴스에 추가하십시오.
{: tip}

<br />


   
## 누락된 권한으로 인한 PVC 작성 실패
{: #missing_permissions}

{: tsSymptoms}
PVC 작성 시 PVC의 상태는 보류 중입니다. `kubectl describe pvc <pvc_name>`를 실행할 때 다음과 유사한 오류 메시지가 표시됩니다.  

```
User doesn't have permissions to create or manage Storage
```
{: screen}

{: tsCauses}
IAM API 키 또는 클러스터의 `storage-secret-store` Kubernetes 시크릿에 저장된 IBM Cloud 인프라(SoftLayer) API 키에는 지속적 스토리지를 프로비저닝하는 데 필요한 모든 권한이 없습니다.  

{: tsResolve}
1. 클러스터의 `storage-secret-store` Kubernetes 시크릿에 저장된 IBM Cloud 인프라(SoftLayer) API 키 또는 IAM 키를 검색하고 올바른 API 키가 사용되는지 확인하십시오.  
   ```
   kubectl get secret storage-secret-store -n kube-system -o yaml | grep slclient.toml: | awk '{print $2}' | base64 --decode
   ```
   {: pre}
   
   출력 예: 
   ```
   [Bluemix]
   iam_url = "https://iam.bluemix.net"
   iam_client_id = "bx"
   iam_client_secret = "bx"
   iam_api_key = "<iam_api_key>"
   refresh_token = ""
   pay_tier = "paid"
   containers_api_route = "https://us-south.containers.bluemix.net"

   [Softlayer]
   encryption = true
   softlayer_username = ""
   softlayer_api_key = ""
   softlayer_endpoint_url = "https://api.softlayer.com/rest/v3"
   softlayer_iam_endpoint_url = "https://api.softlayer.com/mobile/v3"
   softlayer_datacenter = "dal10"
   ```
   {: screen}
   
   IAM API 키는 CLI 출력의 `Bluemix.iam_api_key` 섹션에 나열되어 있습니다. 동시에 `Softlayer.softlayer_api_key`가 비어 있으면 인프라 권한을 결정하는 데 IAM API 키가 사용됩니다. IAM API 키는 리소스 그룹 및 지역에서 **관리자** 플랫폼 역할이 필요한 첫 번째 조치를 수행하는 사용자가 자동으로 설정합니다. 서로 다른 API 키가 `Softlayer.softlayer_api_key`에 설정되면 이 키는 IAM API 키보다 우선합니다. 클러스터 관리자가 `ibmcloud ks credentials-set` 명령을 실행하는 경우 `Softlayer.softlayer_api_key`가 설정됩니다.  
   
2. 인증 정보를 변경하려면 사용되는 API 키를 업데이트하십시오.  
    1.  IAM API 키를 업데이트하려면 `ibmcloud ks api-key-reset` [명령](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_api_key_reset)을 사용하십시오. IBM Cloud 인프라(SoftLayer) 키를 업데이트하려면 `ibmcloud ks credential-set` [명령](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_set)을 사용하십시오.
    2. `storage-secret-store` Kubernetes 시크릿이 업데이트될 때까지 약 15분 동안 기다린 후 키가 업데이트되었는지 확인하십시오.
       ```
       kubectl get secret storage-secret-store -n kube-system -o yaml | grep slclient.toml: | awk '{print $2}' | base64 --decode
       ```
       {: pre}
   
3. API 키가 올바른 경우 키에 지속적 스토리지를 프로비저닝할 수 있는 올바른 권한이 있는지 확인하십시오. 
   1. API 키의 권한을 확인하려면 계정 소유자에게 문의하십시오.  
   2. 계정 소유자로서 {{site.data.keyword.Bluemix_notm}} 콘솔의 탐색에서 **관리** > **액세스(IAM)**를 선택하십시오. 
   3. **사용자**를 선택하고 API 키를 사용할 사용자를 찾으십시오.  
   4. 조치 메뉴에서 **사용자 세부사항 관리**를 선택하십시오. 
   5. **클래식 인프라** 탭으로 이동하십시오. 
   6. **계정** 카테고리를 펼치고 **스토리지 추가/업그레이드(StorageLayer)** 권한이 지정되었는지 확인하십시오. 
   7. **서비스** 카테고리를 펼치고 **스토리지 관리** 권한이 지정되었는지 확인하십시오. 
4. 실패한 PVC를 제거하십시오. 
   ```
   kubectl delete pvc <pvc_name>
   ```
   {: pre}
   
5. PVC를 다시 작성하십시오. 
   ```
   kubectl apply -f pvc.yaml
   ```
   {: pre}


## 도움 및 지원 받기
{: #storage_getting_help}

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

