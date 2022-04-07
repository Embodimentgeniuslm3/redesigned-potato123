---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-03"

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


# IBM Cloud Object Storage에 데이터 저장
{: #object_storage}

[{{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-about)는 {{site.data.keyword.cos_full_notm}} 플러그인을 사용하여 Kubernetes 클러스터에서 실행되는 앱에 마운트할 수 있는 지속적 고가용성 스토리지 입니다. 플러그인은 Cloud {{site.data.keyword.cos_short}} 버킷을 클러스터의 팟(pod)에 연결하는 Kubernetes Flex-Volume 플러그인입니다. {{site.data.keyword.cos_full_notm}}에 저장된 정보는 전환하고 저장하는 상태에서 암호화되어 여러 지리적 위치에 분산되며 REST API를 사용하여 HTTP를 통해 액세스할 수 있습니다.
{: shortdesc}

{{site.data.keyword.cos_full_notm}}에 연결하려면 클러스터에는 {{site.data.keyword.Bluemix_notm}} Identity and Access Management로 인증하기 위한 공용 네트워크 액세스 권한이 필요합니다. 개인 전용 클러스터가 있는 경우 플러그인 버전 `1.0.3` 이상을 설치하고 HMAC 인증용 {{site.data.keyword.cos_full_notm}} 서비스 인스턴스를 설정하면 {{site.data.keyword.cos_full_notm}} 개인 서비스 엔드포인트와 통신할 수 있습니다. HMAC 인증을 사용하지 않으려는 경우, 사설 클러스터에서 플러그인이 제대로 작동하려면 포트 443의 모든 아웃바운드 네트워크 트래픽을 열어야 합니다.
{: important}

버전 1.0.5에서는 {{site.data.keyword.cos_full_notm}} 플러그인의 이름이 `ibmcloud-object-storage-plugin`에서 `ibm-object-storage-plugin`으로 변경됩니다. 플러그인의 새 버전을 설치하려면 [이전 Helm 차트 설치를 설치 제거](#remove_cos_plugin)한 후 [새 {{site.data.keyword.cos_full_notm}} 플러그인 버전에서 Helm 차트를 다시 설치](#install_cos)하십시오.
{: note}

## 오브젝트 스토리지 서비스 인스턴스 작성
{: #create_cos_service}

클러스터에서 오브젝트 스토리지 사용을 시작하려면 우선 계정에서 {{site.data.keyword.cos_full_notm}} 서비스 인스턴스를 프로비저닝해야 합니다.
{: shortdesc}

{{site.data.keyword.cos_full_notm}} 플러그인은 s3 API 엔드포인트와 함께 작동하도록 구성되어 있습니다. 예를 들어, [Minio](https://cloud.ibm.com/kubernetes/solutions/helm-charts/ibm-charts/ibm-minio-objectstore)와 같은 로컬 Cloud Object Storage 서버를 사용하거나 {{site.data.keyword.cos_full_notm}} 서비스 인스턴스를 사용하는 대신 다른 클라우드 제공자에서 설정한 s3 API 엔드포인트에 연결할 수 있습니다.

다음 단계를 수행하여 {{site.data.keyword.cos_full_notm}} 서비스 인스턴스를 작성하십시오. 로컬 Cloud Object Storage 서버 또는 다른 서비스 API 엔드포인트를 사용하려는 경우, 제공자 문서를 참조하여 Cloud Object Storage를 설정하십시오.

1. {{site.data.keyword.cos_full_notm}} 서비스 인스턴스를 배치하십시오.
   1.  [{{site.data.keyword.cos_full_notm}} 카탈로그 페이지](https://cloud.ibm.com/catalog/services/cloud-object-storage)를 여십시오.
   2.  서비스 인스턴스의 이름(예: `cos-backup`)을 입력하고 클러스터가 속한 것과 동일한 리소스 그룹을 선택하십시오. 클러스터의 리소스 그룹을 보려면 `ibmcloud ks cluster-get --cluster <cluster_name_or_ID>`를 실행하십시오.   
   3.  가격 정보에 대해 [플랜 옵션 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api)을 검토하고 플랜을 선택하십시오.
   4.  **작성**을 클릭하십시오. 서비스 세부사항 페이지가 열립니다.
2. {: #service_credentials}{{site.data.keyword.cos_full_notm}} 서비스 인증 정보를 검색하십시오.
   1.  서비스 세부사항 페이지의 탐색에서 **서비스 인증 정보**를 클릭하십시오.
   2.  **새 인증 정보**를 클릭하십시오. 대화 상자가 표시됩니다.
   3.  인증 정보의 이름을 입력하십시오.
   4.  **역할** 드롭 다운에서 `Writer` 또는 `Manager`를 선택하십시오. `Reader`를 선택하면 인증 정보를 사용하여 {{site.data.keyword.cos_full_notm}}에서 버킷을 작성하고 이에 데이터를 쓸 수 없습니다.
   5.  선택사항: **인라인 구성 매개변수 추가(선택사항)**에서 `{"HMAC":true}`를 입력하여 {{site.data.keyword.cos_full_notm}} 서비스에 대한 추가 HMAC 인증 정보를 작성하십시오. HMAC 인증은 만료되거나 무작위로 작성된 OAuth2 토큰의 오용을 방지함으로써 OAuth2 인증에 보안 계층을 추가합니다. **중요**: 공용 액세스가 없는 개인 전용 클러스터가 있는 경우, 사설 네트워크를 통해 {{site.data.keyword.cos_full_notm}} 서비스에 액세스할 수 있도록 HMAC 인증을 사용해야 합니다.
   6.  **추가**를 클릭하십시오. 새 인증 정보가 **서비스 인증 정보** 테이블에 나열됩니다.
   7.  **인증 정보 보기**를 클릭하십시오.
   8.  {{site.data.keyword.cos_full_notm}} 서비스의 인증에 OAuth2 토큰을 사용하려면 **apikey**를 기록해 두십시오. HMAC 인증의 경우에는 **cos_hmac_keys** 섹션에서 **access_key_id** 및 **secret_access_key**를 기록해 두십시오.
3. {{site.data.keyword.cos_full_notm}} 서비스 인스턴스에 액세스가 가능하도록 [클러스터 내의 Kubernetes 시크릿에 서비스 인증 정보를 저장](#create_cos_secret)하십시오.

## 오브젝트 스토리지 서비스 인증 정보의 시크릿 작성
{: #create_cos_secret}

{{site.data.keyword.cos_full_notm}} 서비스 인스턴스에 액세스하여 데이터를 읽고 쓰려면 Kubernetes 시크릿에 서비스 인증 정보를 안전하게 저장해야 합니다. {{site.data.keyword.cos_full_notm}} 플러그인은 버킷에 대한 모든 읽기 또는 쓰기 오퍼레이션에 이러한 인증 정보를 사용합니다.
{: shortdesc}

다음 단계를 수행하여 {{site.data.keyword.cos_full_notm}} 서비스 인스턴스의 인증 정보에 대한 Kubernetes 시크릿을 작성하십시오. 로컬 Cloud Object Storage 서버 또는 다른 s3 API 엔드포인트를 사용하려는 경우 적절한 인증 정보를 사용하여 Kubernetes 시크릿을 작성하십시오.

시작하기 전에: [계정에 로그인하십시오. 해당되는 경우, 적절한 리소스 그룹을 대상으로 지정하십시오. 클러스터의 컨텍스트를 설정하십시오.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1. [{{site.data.keyword.cos_full_notm}} 서비스 인증 정보](#service_credentials)의 **apikey** 또는 **access_key_id** 및 **secret_access_key**를 검색하십시오.

2. {{site.data.keyword.cos_full_notm}} 서비스 인스턴스의 **GUID**를 가져오십시오.
   ```
   ibmcloud resource service-instance <service_name> | grep GUID
   ```
   {: pre}

3. 서비스 인증 정보를 저장하기 위한 Kubernetes 시크릿을 작성하십시오. 시크릿을 작성하는 경우 모든 값이 base64로 자동 인코딩됩니다.

   **API 키 사용의 예:**
   ```
   kubectl create secret generic cos-write-access --type=ibm/ibmc-s3fs --from-literal=api-key=<api_key> --from-literal=service-instance-id=<service_instance_guid>
   ```
   {: pre}

   **HMAC 인증의 예:**
   ```
   kubectl create secret generic cos-write-access --type=ibm/ibmc-s3fs --from-literal=access-key=<access_key_ID> --from-literal=secret-key=<secret_access_key>    
   ```
   {: pre}

   <table>
   <caption>명령 컴포넌트 이해</caption>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 명령 컴포넌트 이해</th>
   </thead>
   <tbody>
   <tr>
   <td><code>api-key</code></td>
   <td>이전에 {{site.data.keyword.cos_full_notm}} 서비스 인증 정보에서 검색한 API 키를 입력하십시오. HMAC 인증을 사용하려면 <code>access-key</code> 및 <code>secret-key</code>를 대신 지정하십시오.  </td>
   </tr>
   <tr>
   <td><code>access-key</code></td>
   <td>이전에 {{site.data.keyword.cos_full_notm}} 서비스 인증 정보에서 검색한 액세스 키 ID를 입력하십시오. OAuth2 인증을 사용하려면 <code>api-key</code>를 대신 지정하십시오.  </td>
   </tr>
   <tr>
   <td><code>secret-key</code></td>
   <td>이전에 {{site.data.keyword.cos_full_notm}} 서비스 인증 정보에서 검색한 시크릿 액세스 키를 입력하십시오. OAuth2 인증을 사용하려면 <code>api-key</code>를 대신 지정하십시오.</td>
   </tr>
   <tr>
   <td><code>service-instance-id</code></td>
   <td>이전에 검색한 {{site.data.keyword.cos_full_notm}} 서비스 인스턴스의 GUID를 입력하십시오. </td>
   </tr>
   </tbody>
   </table>

4. 시크릿이 네임스페이스에서 작성되었는지 확인하십시오.
   ```
   kubectl get secret
   ```
   {: pre}

5. [{{site.data.keyword.cos_full_notm}} 플러그인을 설치](#install_cos)하십시오. 또는 플러그인을 이미 설치한 경우에는 {{site.data.keyword.cos_full_notm}} 버킷에 대한 [구성을 결정]( #configure_cos)하십시오.

## IBM Cloud Object Storage 플러그인 설치
{: #install_cos}

Helm 차트로 {{site.data.keyword.cos_full_notm}} 플러그인을 설치하여 {{site.data.keyword.cos_full_notm}}에 대한 사전 정의된 스토리지 클래스를 설정하십시오. 이러한 스토리지 클래스를 사용하여 앱의 {{site.data.keyword.cos_full_notm}}를 프로비저닝하기 위한 PVC를 작성할 수 있습니다.
{: shortdesc}

{{site.data.keyword.cos_full_notm}} 플러그인의 업데이트 또는 제거 방법에 대한 지시사항을 찾으십니까? [플러그인 업데이트](#update_cos_plugin) 및 [플러그인 제거](#remove_cos_plugin)를 참조하십시오.
{: tip}

시작하기 전에: [계정에 로그인하십시오. 해당되는 경우, 적절한 리소스 그룹을 대상으로 지정하십시오. 클러스터의 컨텍스트를 설정하십시오.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1. 작업자 노드가 부 버전에 대한 최신 패치를 적용하는지 확인하십시오.
   1. 작업자 노드의 현재 패치 버전을 나열하십시오.
      ```
      ibmcloud ks workers --cluster <cluster_name_or_ID>
      ```
      {: pre}

      출력 예:
      ```
      OK
      ID                                                  Public IP        Private IP     Machine Type           State    Status   Zone    Version
      kube-dal10-crb1a23b456789ac1b20b2nc1e12b345ab-w26   169.xx.xxx.xxx    10.xxx.xx.xxx   b3c.4x16.encrypted     normal   Ready    dal10   1.13.6_1523*
      ```
      {: screen}

      작업자 노드가 최신 패치 버전을 적용하지 않는 경우에는 CLI 출력의 **Version** 열에 별표(`*`)가 표시됩니다.

   2. 최신 패치 버전에 포함된 변경사항을 찾으려면 [버전 변경 로그](/docs/containers?topic=containers-changelog#changelog)를 검토하십시오.

   3. 작업자 노드를 다시 로드하여 최신 패치 버전을 적용하십시오. 작업자 노드를 다시 로드하기 전에 작업자 노드에서 실행 중인 팟(Pod)을 단계적으로 다시 스케줄하려면 [ibmcloud ks worker-reload 명령](/docs/containers?topic=containers-cs_cli_reference#cs_worker_reload)의 지시사항을 따르십시오. 다시 로드하는 중에 작업자 노드 머신은 최신 이미지로 업데이트되며 [작업자 노드의 외부에 저장](/docs/containers?topic=containers-storage_planning#persistent_storage_overview)되지 않은 경우 데이터가 삭제됨을 유념하십시오.

2.  Helm 서버인 Tiller를 사용하거나 사용하지 않고 {{site.data.keyword.cos_full_notm}} 플러그인을 설치할지 여부를 선택하십시오. 그런 다음 [지시사항을 따라](/docs/containers?topic=containers-helm#public_helm_install) 로컬 머신에 Helm 클라이언트를 설치하고, 서비스 계정으로 클러스터에 Tiller를 설치하십시오(Tiller를 사용할 경우). 

3. Tiller를 사용하여 플러그인을 설치할 경우 Tiller가 서비스 계정으로 설치되어 있는지 확인하십시오.
   ```
   kubectl get serviceaccount -n kube-system tiller
   ```
   {: pre}
   
   출력 예:
   ```
    NAME                                 SECRETS   AGE
    tiller                               1         2m
   ```
   {: screen}

4. 클러스터에 {{site.data.keyword.Bluemix_notm}} Helm 저장소를 추가하십시오.
   ```
   helm repo add iks-charts https://icr.io/helm/iks-charts
   ```
   {: pre}

4. Helm 저장소를 업데이트하여 이 저장소에 있는 모든 Helm 차트의 최신 버전을 검색하십시오.
   ```
   helm repo update
   ```
   {: pre}

5. Helm 차트를 다운로드하고 현재 디렉토리에서 차트의 압축을 푸십시오.
   ```
   helm fetch --untar iks-charts/ibm-object-storage-plugin
   ```
   {: pre}

7. OS X 또는 Linux 배포판을 사용하는 경우에는 {{site.data.keyword.cos_full_notm}} Helm 플러그인 `ibmc`를 설치하십시오. 플러그인을 사용하면 클러스터 위치를 자동 검색하고 스토리지 클래스의 {{site.data.keyword.cos_full_notm}} 버킷에 대한 API 엔드포인트를 설정할 수 있습니다. Windows를 운영 체제로 사용하는 경우에는 다음 단계로 진행하십시오.
   1. Helm 플러그인을 설치하십시오.
      ```
      helm plugin install ./ibm-object-storage-plugin/helm-ibmc
      ```
      {: pre}

      출력 예:
      ```
      설치된 플러그인: ibmc
      ```
      {: screen}
      
      `Error: plugin already exists` 오류가 표시되면 `rm -rf ~/.helm/plugins/helm-ibmc`를 실행하여 `ibmc` Helm 플러그인을 제거하십시오.
      {: tip}

   2. `ibmc` 플러그인이 성공적으로 설치되었는지 확인하십시오.
      ```
      helm ibmc --help
      ```
      {: pre}

      출력 예:
      ```
      Install or upgrade Helm charts in IBM K8S Service(IKS) and IBM Cloud Private(ICP)

      Available Commands:
         helm ibmc install [CHART][flags]                      Install a Helm chart
         helm ibmc upgrade [RELEASE][CHART] [flags]            Upgrade the release to a new version of the Helm chart
         helm ibmc template [CHART][flags] [--apply|--delete]  Install/uninstall a Helm chart without tiller

      Available Flags:
         -h, --help                    (Optional) This text.
         -u, --update                  (Optional) Update this plugin to the latest version

      Example Usage:
         With Tiller:
             Install:   helm ibmc install iks-charts/ibm-object-storage-plugin --name ibm-object-storage-plugin
         Without Tiller:
             Install:   helm ibmc template iks-charts/ibm-object-storage-plugin --apply
             Dry-run:   helm ibmc template iks-charts/ibm-object-storage-plugin
             Uninstall: helm ibmc template iks-charts/ibm-object-storage-plugin --delete

      Note:
         1. It is always recommended to install latest version of ibm-object-storage-plugin chart.
         2. It is always recommended to have 'kubectl' client up-to-date.
      ```
      {: screen}

      출력에 `Error: fork/exec /home/iksadmin/.helm/plugins/helm-ibmc/ibmc.sh: permission denied` 오류가 표시되면 `chmod 755 ~/.helm/plugins/helm-ibmc/ibmc.sh`를 실행하십시오. 그런 다음, `helm ibmc --help`를 다시 실행하십시오.
      {: tip}

8. 선택사항: {{site.data.keyword.cos_full_notm}} 서비스 인증 정보가 보관된 Kubernetes 시크릿만 액세스하도록 {{site.data.keyword.cos_full_notm}} 플러그인을 제한하십시오. 기본적으로, 플러그인은 클러스터의 모든 Kubernetes 시크릿에 액세스할 권한이 있습니다.
   1. [{{site.data.keyword.cos_full_notm}} 서비스 인스턴스를 작성](#create_cos_service)하십시오.
   2. [Kubernetes 시크릿에 {{site.data.keyword.cos_full_notm}} 서비스 인증 정보를 저장](#create_cos_secret)하십시오.
   3. `templates` 디렉토리로 이동하여 사용 가능한 파일을 나열하십시오.  
      ```
      cd ibm-object-storage-plugin/templates && ls
      ```
      {: pre}

   4. `provisioner-sa.yaml` 파일을 열고 `ibmcloud-object-storage-secret-reader` `ClusterRole` 정의를 찾으십시오.
   6. `resourceNames` 섹션에서 플러그인이 액세스할 수 있는 시크릿의 목록에 이전에 작성한 시크릿의 이름을 추가하십시오.
      ```
      kind: ClusterRole
      apiVersion: rbac.authorization.k8s.io/v1beta1
      metadata:
        name: ibmcloud-object-storage-secret-reader
      rules:
      - apiGroups: [""]
        resources: ["secrets"]
        resourceNames: ["<secret_name1>","<secret_name2>"]
        verbs: ["get"]
      ```
      {: codeblock}
   7. 변경사항을 저장하십시오.

9. {{site.data.keyword.cos_full_notm}} 플러그인을 설치하십시오. 플러그인을 설치하면 사전 정의된 스토리지 클래스가 클러스터에 추가됩니다.

   - **OS X 및 Linux의 경우:**
     - 이전 단계를 건너뛴 경우에는 특정 Kubernetes 시크릿에 대한 제한사항 없이 설치하십시오.</br>
       **Tiller 포함 안함**:
       ```
       helm ibmc template iks-charts/ibm-object-storage-plugin --apply
       ```
       {: pre}
       
       **Tiller 포함**:
       ```
       helm ibmc install iks-charts/ibm-object-storage-plugin --name ibm-object-storage-plugin
       ```
       {: pre}

     - 이전 단계를 완료한 경우에는 특정 Kubernetes 시크릿에 대한 제한사항을 적용하여 설치하십시오.</br>
       **Tiller 포함 안함**:
       ```
       cd ../..
       helm ibmc template ./ibm-object-storage-plugin --apply 
       ```
       {: pre}
       
       **Tiller 포함**:
       ```
       cd ../..
       helm ibmc install ./ibm-object-storage-plugin --name ibm-object-storage-plugin
       ```
       {: pre}

   - **Windows의 경우:**
     1. 클러스터가 배치되는 구역을 검색하고 해당 구역을 환경 변수에 저장하십시오.
        ```
        export DC_NAME=$(kubectl get cm cluster-info -n kube-system -o jsonpath='{.data.cluster-config\.json}' | grep datacenter | awk -F ': ' '{print $2}' | sed 's/\"//g' |sed 's/,//g')
        ```
        {: pre}

     2. 환경 변수가 설정되었는지 확인하십시오.
        ```
        printenv
        ```
        {: pre}

     3. Helm 차트를 설치하십시오.
        - 이전 단계를 건너뛴 경우에는 특정 Kubernetes 시크릿에 대한 제한사항 없이 설치하십시오.</br>
       **Tiller 포함 안함**:
       ```
          helm ibmc template iks-charts/ibm-object-storage-plugin --apply
          ```
          {: pre}
          
          **Tiller 포함**:
       ```
          helm ibmc install iks-charts/ibm-object-storage-plugin --name ibm-object-storage-plugin
          ```
          {: pre}

        - 이전 단계를 완료한 경우에는 특정 Kubernetes 시크릿에 대한 제한사항을 적용하여 설치하십시오.</br>
       **Tiller 포함 안함**:
       ```
          cd ../..
          helm ibmc template ./ibm-object-storage-plugin --apply 
          ```
          {: pre}
          
          **Tiller 포함**:
       ```
          cd ../..
          helm ibmc install ./ibm-object-storage-plugin --name ibm-object-storage-plugin
          ```
          {: pre}


   Tiller를 사용하지 않고 설치한 경우 출력 예:
   ```
   Rendering the Helm chart templates...
   DC: dal10
   Chart: iks-charts/ibm-object-storage-plugin
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-cold-cross-region.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-cold-regional.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-flex-cross-region.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-flex-perf-cross-region.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-flex-perf-regional.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-flex-regional.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-standard-cross-region.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-standard-perf-cross-region.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-standard-perf-regional.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-standard-regional.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-vault-cross-region.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-vault-regional.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/flex-driver-sa.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/provisioner-sa.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/flex-driver.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/tests/check-driver-install.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/provisioner.yaml
   Installing the Helm chart...
   serviceaccount/ibmcloud-object-storage-driver created
   daemonset.apps/ibmcloud-object-storage-driver created
   storageclass.storage.k8s.io/ibmc-s3fs-cold-cross-region created
   storageclass.storage.k8s.io/ibmc-s3fs-cold-regional created
   storageclass.storage.k8s.io/ibmc-s3fs-flex-cross-region created
   storageclass.storage.k8s.io/ibmc-s3fs-flex-perf-cross-region created
   storageclass.storage.k8s.io/ibmc-s3fs-flex-perf-regional created
   storageclass.storage.k8s.io/ibmc-s3fs-flex-regional created
   storageclass.storage.k8s.io/ibmc-s3fs-standard-cross-region created
   storageclass.storage.k8s.io/ibmc-s3fs-standard-perf-cross-region created
   storageclass.storage.k8s.io/ibmc-s3fs-standard-perf-regional created
   storageclass.storage.k8s.io/ibmc-s3fs-standard-regional created
   storageclass.storage.k8s.io/ibmc-s3fs-vault-cross-region created
   storageclass.storage.k8s.io/ibmc-s3fs-vault-regional created
   serviceaccount/ibmcloud-object-storage-plugin created
   clusterrole.rbac.authorization.k8s.io/ibmcloud-object-storage-plugin created
   clusterrole.rbac.authorization.k8s.io/ibmcloud-object-storage-secret-reader created
   clusterrolebinding.rbac.authorization.k8s.io/ibmcloud-object-storage-plugin created
   clusterrolebinding.rbac.authorization.k8s.io/ibmcloud-object-storage-secret-reader created
   deployment.apps/ibmcloud-object-storage-plugin created
   pod/ibmcloud-object-storage-driver-test created
   ```
   {: screen}

10. 플러그인이 올바르게 설치되었는지 확인하십시오.
    ```
    kubectl get pod -n kube-system -o wide | grep object
    ```
    {: pre}

    출력 예:
    ```
    ibmcloud-object-storage-driver-9n8g8                              1/1       Running   0          2m
    ibmcloud-object-storage-plugin-7c774d484b-pcnnx                   1/1       Running   0          2m
    ```
    {: screen}

    하나의 `ibmcloud-object-storage-plugin` 팟(Pod)과 하나 이상의 `ibmcloud-object-storage-driver` 팟(Pod)이 나타나면 설치가 완료된 것입니다. `ibmcloud-object-storage-driver` 팟(Pod)의 수는 클러스터의 작업자 노드 수와 동일합니다. 플러그인이 제대로 작동하려면 모든 팟(Pod)이 `Running` 상태여야 합니다. 팟(Pod)이 실패하는 경우에는 `kubectl describe pod -n kube-system <pod_name>`을 실행하여 실패의 근본 원인을 찾으십시오.

11. 스토리지 클래스가 성공적으로 작성되었는지 확인하십시오.
    ```
    kubectl get storageclass | grep s3
    ```
    {: pre}

    출력 예:
    ```
    ibmc-s3fs-cold-cross-region            ibm.io/ibmc-s3fs   8m
    ibmc-s3fs-cold-regional                ibm.io/ibmc-s3fs   8m
    ibmc-s3fs-flex-cross-region            ibm.io/ibmc-s3fs   8m
    ibmc-s3fs-flex-perf-cross-region       ibm.io/ibmc-s3fs   8m
    ibmc-s3fs-flex-perf-regional           ibm.io/ibmc-s3fs   8m
    ibmc-s3fs-flex-regional                ibm.io/ibmc-s3fs   8m
    ibmc-s3fs-standard-cross-region        ibm.io/ibmc-s3fs   8m
    ibmc-s3fs-standard-perf-cross-region   ibm.io/ibmc-s3fs   8m
    ibmc-s3fs-standard-perf-regional       ibm.io/ibmc-s3fs   8m
    ibmc-s3fs-standard-regional            ibm.io/ibmc-s3fs   8m
    ibmc-s3fs-vault-cross-region           ibm.io/ibmc-s3fs   8m
    ibmc-s3fs-vault-regional               ibm.io/ibmc-s3fs   8m
    ```
    {: screen}

12. 사용자가 {{site.data.keyword.cos_full_notm}} 버킷에 액세스할 모든 클러스터에 대해 단계를 반복하십시오.

### IBM Cloud Object Storage 플러그인 업데이트
{: #update_cos_plugin}

기존 {{site.data.keyword.cos_full_notm}} 플러그인을 후속 버전으로 업그레이드할 수 있습니다.
{: shortdesc}

1. 이전에 이름이 `ibmcloud-object-storage-plugin`인 Helm 차트의 버전 1.0.4 이하를 설치한 경우 클러스터에서 이 Helm 설치를 제거하십시오. 그런 다음 Helm 차트를 다시 설치하십시오. 
   1. {{site.data.keyword.cos_full_notm}} Helm 차트의 이전 버전이 클러스터에 설치되어 있는지 확인하십시오.   
      ```
      helm ls | grep ibmcloud-object-storage-plugin
      ```
      {: pre}

      출력 예:
      ```
      ibmcloud-object-storage-plugin	1       	Mon Sep 18 15:31:40 2017	DEPLOYED	ibmcloud-object-storage-plugin-1.0.4	default
      ```
      {: screen}

   2. 이름이 `ibmcloud-object-storage-plugin`인 Helm 차트의 버전 1.0.4 이하를 설치한 경우 클러스터에서 Helm 차트를 제거하십시오. 이름이 `ibm-object-storage-plugin`인 Helm 차트의 버전 1.0.5 이상을 설치한 경우 2단계로 진행하십시오.
      ```
      helm delete --purge ibmcloud-object-storage-plugin
      ```
      {: pre}

   3. {{site.data.keyword.cos_full_notm}} 플러그인의 최신 버전을 설치하려면 [{{site.data.keyword.cos_full_notm}} 플러그인 설치](#install_cos)의 단계를 따르십시오.

2. {{site.data.keyword.Bluemix_notm}} Helm 저장소를 업데이트하여 이 저장소에 있는 모든 Helm 차트의 최신 버전을 검색하십시오.
   ```
   helm repo update
   ```
   {: pre}

3. OS X 또는 Linux 배포판을 사용하는 경우에는 {{site.data.keyword.cos_full_notm}} `ibmc` Helm 플러그인을 최신 버전으로 업데이트하십시오.
   ```
   helm ibmc --update
   ```
   {: pre}

4. 최신 {{site.data.keyword.cos_full_notm}} Helm 차트를 로컬 머신에 다운로드하고 패키지의 압축을 푼 후에 `release.md` 파일을 검토하여 최신 릴리스 정보를 찾으십시오.
   ```
   helm fetch --untar iks-charts/ibm-object-storage-plugin
   ```
   {: pre}

5. 플러그인을 업그레이드하십시오.</br>
   **Tiller 포함 안함**: 
   ```
   helm ibmc template iks-charts/ibm-object-storage-plugin --update
   ```
   {: pre}
     
   **Tiller 포함**: 
   1. Helm 차트의 설치 이름을 찾으십시오.
      ```
      helm ls | grep ibm-object-storage-plugin
      ```
      {: pre}

      출력 예:
      ```
      <helm_chart_name> 	1       	Mon Sep 18 15:31:40 2017	DEPLOYED	ibm-object-storage-plugin-1.0.5	default
      ```
      {: screen}

   2. {{site.data.keyword.cos_full_notm}} Helm 차트를 최신 버전으로 업그레이드하십시오.
      ```   
      helm ibmc upgrade <helm_chart_name> iks-charts/ibm-object-storage-plugin --force --recreate-pods -f
      ```
      {: pre}

6. `ibmcloud-object-storage-plugin`이 성공적으로 업그레이드되었는지 확인하십시오.  
   ```
   kubectl rollout status deployment/ibmcloud-object-storage-plugin -n kube-system
   ```
   {: pre}

   CLI 출력에 `deployment "ibmcloud-object-storage-plugin" successfully rolled out`이 나타나면 플러그인의 업그레이드가 완료된 것입니다.

7. `ibmcloud-object-storage-driver`가 성공적으로 업그레이드되었는지 확인하십시오.
   ```
   kubectl rollout status ds/ibmcloud-object-storage-driver -n kube-system
   ```
   {: pre}

   CLI 출력에 `daemon set "ibmcloud-object-storage-driver" successfully rolled out`이 나타나면 플러그인의 업그레이드가 완료된 것입니다.

8. {{site.data.keyword.cos_full_notm}} 팟(Pod)이 `Running` 상태인지 확인하십시오.
   ```
   kubectl get pods -n kube-system -o wide | grep object-storage
   ```
   {: pre}


### IBM Cloud Object Storage 플러그인 제거
{: #remove_cos_plugin}

클러스터에서 {{site.data.keyword.cos_full_notm}}의 프로비저닝과 사용을 원하지 않으면 플러그인을 설치 제거할 수 있습니다.
{: shortdesc}

이 플러그인을 제거해도 기존 PVC, PV 또는 데이터는 제거되지 않습니다. 플러그인을 제거할 때는 모든 관련 팟(Pod) 및 디먼 세트만 클러스터에서 제거됩니다. {{site.data.keyword.cos_full_notm}} API를 직접 사용하도록 앱을 구성하지 않는 한, 플러그인 제거 이후에 기존 PVC 및 PV를 사용하거나 클러스터의 새 {{site.data.keyword.cos_full_notm}}를 프로비저닝할 수 없습니다.
{: important}

시작하기 전에:

- [CLI에 클러스터를 대상으로 지정](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)하십시오.
- {{site.data.keyword.cos_full_notm}}를 사용하는 클러스터에 PVC 또는 PV가 없는지 확인하십시오. 특정 PVC를 마운트하는 모든 팟(Pod)을 나열하려면 다음을 실행하십시오. `kubectl get pods --all-namespaces -o=jsonpath='{range .items[*]}{"\n"}{.metadata.name}{":\t"}{range .spec.volumes[*]}{.persistentVolumeClaim.claimName}{" "}{end}{end}' | grep "<pvc_name>"`

플러그인을 제거하려면 다음을 수행하십시오.

1. 클러스터에서 플러그인을 제거하십시오.</br>
   **Tiller 포함**: 
   1. Helm 차트의 설치 이름을 찾으십시오.
      ```
      helm ls | grep object-storage-plugin
      ```
      {: pre}

      출력 예:
      ```
      <helm_chart_name> 	1       	Mon Sep 18 15:31:40 2017	DEPLOYED	ibmcloud-object-storage-plugin-1.0.0	default
      ```
      {: screen}

   2. Helm 차트를 제거하여 {{site.data.keyword.cos_full_notm}} 플러그인을 삭제하십시오.
      ```
   helm delete --purge <helm_chart_name>
      ```
      {: pre}

   **Tiller 포함 안함**: 
   ```
   helm ibmc template iks-charts/ibm-object-storage-plugin --delete
   ```
   {: pre}

2. {{site.data.keyword.cos_full_notm}} 팟(Pod)이 제거되었는지 확인하십시오.
   ```
   kubectl get pod -n kube-system | grep object-storage
   ```
   {: pre}

      CLI 출력에 팟(Pod)이 표시되지 않으면 팟(Pod) 제거가 성공한 것입니다.

3. 스토리지 클래스가 제거되었는지 확인하십시오.
   ```
   kubectl get storageclasses | grep s3
   ```
   {: pre}

      CLI 출력에 스토리지 클래스가 표시되지 않으면 스토리지 클래스 제거가 성공한 것입니다.

4. OS X 또는 Linux 배포판을 사용하는 경우에는 `ibmc` Helm 플러그인을 제거하십시오. Windows를 사용하는 경우에는 이 단계가 필요하지 않습니다.
   1. `ibmc` 플러그인을 제거하십시오.
      ```
      rm -rf ~/.helm/plugins/helm-ibmc
      ```
      {: pre}

   2. `ibmc` 플러그인이 제거되었는지 확인하십시오.
      ```
      helm plugin list
      ```
      {: pre}

      출력 예:
     ```
        NAME	VERSION	DESCRIPTION
     ```
     {: screen}

     `ibmc` 플러그인이 CLI 출력에 나열되지 않으면 `ibmc` 플러그인이 성공적으로 제거된 것입니다.


## 오브젝트 스토리지 구성 결정
{: #configure_cos}

{{site.data.keyword.containerlong_notm}}는 특정 구성의 버킷을 작성하는 데 사용할 수 있는 사전 정의된 스토리지 클래스를 제공합니다.
{: shortdesc}

1. {{site.data.keyword.containerlong_notm}}에서 사용 가능한 스토리지 클래스를 나열하십시오.
   ```
   kubectl get storageclasses | grep s3
   ```
   {: pre}

   출력 예:
   ```
   ibmc-s3fs-cold-cross-region            ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-cold-regional                ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-flex-cross-region            ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-flex-perf-cross-region       ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-flex-perf-regional           ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-flex-regional                ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-standard-cross-region        ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-standard-perf-cross-region   ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-standard-perf-regional       ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-standard-regional            ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-vault-cross-region           ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-vault-regional               ibm.io/ibmc-s3fs   8m
   ```
   {: screen}

2. 데이터 액세스 요구사항에 맞는 스토리지 클래스를 선택하십시오. 스토리지 클래스는 스토리지 용량, 읽기/쓰기 오퍼레이션 및 버킷에 대한 아웃바운드 대역폭에 대한 [가격 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api)을 판별합니다. 사용자에게 적합한 옵션은 서비스 인스턴스에 대한 데이터 읽기/쓰기의 빈도를 기반으로 합니다.
   - **표준**: 이 옵션은 자주 액세스되는 핫(hot) 데이터에 사용됩니다. 일반적인 유스 케이스는 웹 또는 모바일 앱입니다.
   - **Vault**: 이 옵션은 자주 액세스되지 않는 워크로드 또는 쿨(cool) 데이터에 사용됩니다(예: 1개월에 1회 이하). 일반적인 유스 케이스는 아카이브, 단기 데이터 보유, 디지털 자산 보존, 테이프 교체 및 재해 복구입니다.
   - **Cold**: 이 옵션은 거의 액세스되지 않는 콜드(cold) 데이터(매 90일마다 또는 이보다 덜 액세스됨) 또는 비활성 데이터에 사용됩니다. 일반적인 유스 케이스는 아카이브, 장기 백업, 준수용으로 보관되는 히스토리 데이터 또는 거의 액세스되지 않는 워크로드와 앱입니다.
   - **Flex**: 이 옵션은 특정 사용 패턴을 따르지 않는 또는 너무 커서 사용 패턴의 결정이나 예측이 어려운 워크로드와 데이터에 사용됩니다. 
**팁:** 일반 스토리지 계층과 비교하여 Flex 스토리지 클래스의 작동 방법에 대해 알아보려면 이 [블로그 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/blogs/bluemix/2017/03/interconnect-2017-changing-rules-storage/)를 확인하십시오.   

3. 버킷에 저장된 데이터의 복원성 레벨을 결정하십시오.
   - **교차 지역(Cross-region)**: 이 옵션을 사용하면 데이터가 고가용성을 위해 지리적 위치 내의 3개 지역에 걸쳐 저장됩니다. 지역 간에 분산된 워크로드가 있으면 최인접 지역 엔드포인트로 요청이 라우트됩니다. 지리적 위치에 대한 API 엔드포인트는 클러스터가 있는 위치를 기반으로 이전에 설치된 `ibmc` Helm 플러그인에 의해 자동으로 설정됩니다. 예를 들어, 클러스터가 `US South`에 있으면 버킷에 대해 `US GEO` API 엔드포인트를 사용하도록 스토리지 클래스가 구성됩니다. 자세한 정보는 [지역 및 엔드포인트](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-endpoints#endpoints)를 참조하십시오.  
   - **지역적(Regional)**: 이 옵션을 사용하면 데이터가 한 지역 내의 다중 구역 간에 복제됩니다. 동일한 지역에 있는 워크로드가 있는 경우에는 교차 지역 설정보다 낮은 대기 시간과 더 높은 성능이 나타납니다. 지역적 엔드포인트는 클러스터가 있는 위치를 기반으로 이전에 설치된 `ibm` Helm 플러그인에 의해 자동으로 설정됩니다. 예를 들어, 클러스터가 `US South`에 있으면 버킷의 지역적 엔드포인트로서 `US South`를 사용하도록 스토리지 클래스가 구성됩니다. 자세한 정보는 [지역 및 엔드포인트](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-endpoints#endpoints)를 참조하십시오.

4. 스토리지 클래스의 세부 {{site.data.keyword.cos_full_notm}} 버킷 구성을 검토하십시오.
   ```
   kubectl describe storageclass <storageclass_name>
   ```
   {: pre}

   출력 예:
   ```
   Name:                  ibmc-s3fs-standard-cross-region
   IsDefaultClass:        No
   Annotations:           <none>
   Provisioner:           ibm.io/ibmc-s3fs
   Parameters:            ibm.io/chunk-size-mb=16,ibm.io/curl-debug=false,ibm.io/debug-level=warn,ibm.io/iam-endpoint=https://iam.bluemix.net,ibm.io/kernel-cache=true,ibm.io/multireq-max=20,ibm.io/object-store-endpoint=https://s3-api.dal-us-geo.objectstorage.service.networklayer.com,ibm.io/object-store-storage-class=us-standard,ibm.io/parallel-count=2,ibm.io/s3fs-fuse-retry-count=5,ibm.io/stat-cache-size=100000,ibm.io/tls-cipher-suite=AESGCM
   AllowVolumeExpansion:  <unset>
   MountOptions:          <none>
   ReclaimPolicy:         Delete
   VolumeBindingMode:     Immediate
   Events:                <none>
   ```
   {: screen}

   <table>
   <caption>스토리지 클래스 세부사항 이해</caption>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> YAML 파일 컴포넌트 이해</th>
   </thead>
   <tbody>
   <tr>
   <td><code>ibm.io/chunk-size-mb</code></td>
   <td>{{site.data.keyword.cos_full_notm}}에서 읽거나 이에 쓴 데이터 청크의 크기(MB)입니다. 자체 이름에 <code>perf</code>가 있는 스토리지 클래스는 52MB로 설정됩니다. 자체 이름에 <code>perf</code>가 없는 스토리지 클래스는 16MB 청크를 사용합니다. 예를 들어, 1GB인 파일을 읽으려는 경우 플러그인은 16 또는 52MB 청크의 배수로 이 파일을 읽습니다. </td>
   </tr>
   <tr>
   <td><code>ibm.io/curl-debug</code></td>
   <td>{{site.data.keyword.cos_full_notm}} 서비스 인스턴스로 전송된 요청의 로깅을 사용합니다. 이를 사용하면 로그가 `syslog`로 전송되며 사용자가 [로그를 외부 로깅 서버로 전달](/docs/containers?topic=containers-health#logging)할 수 있습니다. 기본적으로 모든 스토리지 클래스는 이 로깅 기능을 사용하지 않도록 <strong>false</strong>로 설정됩니다. </td>
   </tr>
   <tr>
   <td><code>ibm.io/debug-level</code></td>
   <td>{{site.data.keyword.cos_full_notm}} 플러그인에서 설정한 로깅 레벨입니다. 모든 스토리지 클래스는 <strong>WARN</strong> 로깅 레벨로 설정되어 있습니다. </td>
   </tr>
   <tr>
   <td><code>ibm.io/iam-endpoint</code></td>
   <td>{{site.data.keyword.Bluemix_notm}} IAM(Identity and Access Management)에 대한 API 엔드포인트입니다. </td>
   </tr>
   <tr>
   <td><code>ibm.io/kernel-cache</code></td>
   <td>볼륨 마운트 지점에 대해 커널 버퍼 캐시를 사용하거나 사용하지 않습니다. 이를 사용하면 데이터에 대한 빠른 읽기 액세스를 보장할 수 있도록 {{site.data.keyword.cos_full_notm}}에서 읽은 데이터가 커널 캐시에 저장됩니다. 이를 사용하지 않으면 데이터가 캐시되지 않으며 항상 {{site.data.keyword.cos_full_notm}}에서 데이터를 읽습니다. 커널 캐시는 <code>standard</code> 및 <code>flex</code> 스토리지 클래스에 사용되며, <code>cold</code> 및 <code>vault</code> 스토리지 클래스에는 사용되지 않습니다. </td>
   </tr>
   <tr>
   <td><code>ibm.io/multireq-max</code></td>
   <td>단일 디렉토리의 파일을 나열하기 위해 {{site.data.keyword.cos_full_notm}} 서비스 인스턴스에 전송될 수 있는 병렬 요청의 최대 수입니다. 모든 스토리지 클래스는 최대 20개의 병렬 요청으로 설정됩니다.  </td>
   </tr>
   <tr>
   <td><code>ibm.io/object-store-endpoint</code></td>
   <td>{{site.data.keyword.cos_full_notm}} 서비스 인스턴스의 버킷에 액세스하는 데 사용할 API 엔드포인트입니다. 엔드포인트는 클러스터의 지역에 따라 자동으로 설정됩니다. **참고**: 클러스터가 있는 지역이 아닌 다른 지역에 있는 기존 버킷에 액세스하려면 [사용자 정의 스토리지 클래스](/docs/containers?topic=containers-kube_concepts#customized_storageclass)를 작성하고 버킷의 API 엔드포인트를 사용해야 합니다.</td>
   </tr>
   <tr>
   <td><code>ibm.io/object-store-storage-class</code></td>
   <td>스토리지 클래스의 이름입니다. </td>
   </tr>
   <tr>
   <td><code>ibm.io/parallel-count</code></td>
   <td>단일 읽기 또는 쓰기 오퍼레이션에 대해 {{site.data.keyword.cos_full_notm}} 서비스 인스턴스에 전송될 수 있는 병렬 요청의 최대 수입니다. 자체 이름에 <code>perf</code>가 있는 스토리지 클래스는 최대 20개의 병렬 요청으로 설정됩니다. <code>perf</code>가 없는 스토리지 클래스는 기본적으로 2개의 병렬 요청으로 설정됩니다.  </td>
   </tr>
   <tr>
   <td><code>ibm.io/s3fs-fuse-retry-count</code></td>
   <td>오퍼레이션이 실패로 간주되기 전에 읽기 및 쓰기 오퍼레이션의 최대 재시도 횟수입니다. 모든 스토리지 클래스는 최대 5번의 재시도로 설정됩니다.  </td>
   </tr>
   <tr>
   <td><code>ibm.io/stat-cache-size</code></td>
   <td>{{site.data.keyword.cos_full_notm}} 메타데이터 캐시에 보관되는 최대 레코드 수입니다. 모든 레코드는 0.5KB까지 가능합니다. 모든 스토리지 클래스는 기본적으로 최대 레코드 수를 100000으로 설정합니다. </td>
   </tr>
   <tr>
   <td><code>ibm.io/tls-cipher-suite</code></td>
   <td>{{site.data.keyword.cos_full_notm}}에 대한 연결이 HTTPS 엔드포인트를 통해 설정될 때 사용되어야 하는 TLS 암호 스위트입니다. 암호 스위트의 값은 [OpenSSL 형식 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.openssl.org/docs/man1.0.2/apps/ciphers.html)을 따라야 합니다. 모든 스토리지 클래스는 기본적으로 <strong><code>AESGCM</code></strong> 암호 스위트를 사용합니다.  </td>
   </tr>
   </tbody>
   </table>

   각 스토리지 클래스에 대한 자세한 정보는 [스토리지 클래스 참조](#cos_storageclass_reference)를 참조하십시오. 사전 설정된 값을 변경하려면 자체 [사용자 정의된 스토리지 클래스](/docs/containers?topic=containers-kube_concepts#customized_storageclass)를 작성하십시오.
   {: tip}

5. 버킷의 이름을 결정하십시오. 버킷 이름은 {{site.data.keyword.cos_full_notm}}에서 고유해야 합니다. {{site.data.keyword.cos_full_notm}} 플러그인에 의한 버킷 이름 자동 작성을 선택할 수도 있습니다. 버킷의 데이터를 구성하기 위해 서브디렉토리를 작성할 수 있습니다.

   이전에 선택한 스토리지 클래스는 전체 버킷의 가격을 결정합니다. 서브디렉토리에 대해 상이한 스토리지 클래스를 정의할 수 없습니다. 서로 다른 액세스 요구사항의 데이터를 저장하려면 다중 PVC를 사용한 다중 버킷 작성을 고려하십시오.
   {: note}

6. 클러스터 또는 지속적 볼륨 클레임(PVC)이 삭제된 후에 데이터와 버킷을 보관하고자 하는지를 선택하십시오. PVC를 삭제하면 PV가 항상 삭제됩니다. PVC를 삭제할 때 데이터와 버킷도 자동으로 삭제할지 여부를 선택할 수 있습니다. {{site.data.keyword.cos_full_notm}} 서비스 인스턴스는 데이터에 대해 선택한 보관 정책과는 무관하며, 이는 PVC 삭제 시에 제거되지 않습니다.

이제 원하는 구성을 결정했으므로 {{site.data.keyword.cos_full_notm}}를 프로비저닝하기 위한 [PVC 작성](#add_cos) 준비가 되었습니다.

## 앱에 오브젝트 스토리지 추가
{: #add_cos}

클러스터에 대해 {{site.data.keyword.cos_full_notm}}를 프로비저닝하기 위한 지속적 볼륨 클레임(PVC)을 작성하십시오.
{: shortdesc}

PVC에서 선택하는 설정에 따라 다음 방법으로 {{site.data.keyword.cos_full_notm}}를 프로비저닝할 수 있습니다.
- [동적 프로비저닝](/docs/containers?topic=containers-kube_concepts#dynamic_provisioning): PVC를 작성하면 일치하는 지속적 볼륨(PV) 및 {{site.data.keyword.cos_full_notm}} 서비스 인스턴스의 버킷이 자동으로 작성됩니다.
- [정적 프로비저닝](/docs/containers?topic=containers-kube_concepts#static_provisioning): PVC의 {{site.data.keyword.cos_full_notm}} 서비스 인스턴스에서 기존 버킷을 참조할 수 있습니다. PVC를 작성하면 일치하는 PV만 자동으로 작성되며 {{site.data.keyword.cos_full_notm}}의 기존 버킷에 링크됩니다.

시작하기 전에:
- [{{site.data.keyword.cos_full_notm}} 서비스 인스턴스를 작성하고 준비](#create_cos_service)하십시오.
- [{{site.data.keyword.cos_full_notm}} 서비스 인증 정보를 저장할 시크릿을 작성](#create_cos_secret)하십시오.
- [{{site.data.keyword.cos_full_notm}}의 구성을 결정](#configure_cos)하십시오.

{{site.data.keyword.cos_full_notm}}를 클러스터에 추가하려면 다음을 수행하십시오.

1. 지속적 볼륨 클레임(PVC)을 정의하기 위한 구성 파일을 작성하십시오.
   ```
   kind: PersistentVolumeClaim
   apiVersion: v1
   metadata:
     name: <pvc_name>
     namespace: <namespace>
     annotations:
       ibm.io/auto-create-bucket: "<true_or_false>"
       ibm.io/auto-delete-bucket: "<true_or_false>"
       ibm.io/bucket: "<bucket_name>"
       ibm.io/object-path: "<bucket_subdirectory>"
       ibm.io/secret-name: "<secret_name>"
       ibm.io/endpoint: "https://<s3fs_service_endpoint>"
   spec:
     accessModes:
       - ReadWriteOnce
     resources:
       requests:
         storage: 8Gi # Enter a fictitious value
     storageClassName: <storage_class>
   ```
   {: codeblock}

   <table>
   <caption>YAML 파일 컴포넌트 이해</caption>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> YAML 파일 컴포넌트 이해</th>
   </thead>
   <tbody>
   <tr>
   <td><code>metadata.name</code></td>
   <td>PVC의 이름을 입력하십시오.</td>
   </tr>
   <tr>
   <td><code>metadata.namespace</code></td>
   <td>PVC가 작성될 네임스페이스를 입력하십시오. {{site.data.keyword.cos_full_notm}} 서비스 인증 정보의 Kubernetes 시크릿이 작성되었으며 팟(Pod)이 실행될 동일 네임스페이스에서 PVC를 작성해야 합니다. </td>
   </tr>
   <tr>
   <td><code>ibm.io/auto-create-bucket</code></td>
   <td>다음 선택사항 중 하나를 선택하십시오. <ul><li><strong>true</strong>: PVC를 작성하면 {{site.data.keyword.cos_full_notm}} 서비스 인스턴스의 버킷 및 PV가 자동으로 작성됩니다. {{site.data.keyword.cos_full_notm}} 서비스 인스턴스의 새 버킷을 작성하려면 이 옵션을 선택하십시오. </li><li><strong>false</strong>: 기존 버킷의 데이터에 액세스하려면 이 옵션을 선택하십시오. PVC를 작성하면 PV가 자동으로 작성되며 <code>ibm.io/bucket</code>에서 지정된 버킷에 링크됩니다.</td>
   </tr>
   <tr>
   <td><code>ibm.io/auto-delete-bucket</code></td>
   <td>다음 선택사항 중 하나를 선택하십시오. <ul><li><strong>true</strong>: PVC를 삭제할 때 데이터, 버킷 및 PV가 자동으로 제거됩니다. {{site.data.keyword.cos_full_notm}} 서비스 인스턴스는 그대로 유지되며 삭제되지 않습니다. 이 옵션을 <strong>true</strong>로 설정하도록 선택한 경우에는 <code>tmp-s3fs-xxxx</code> 형식의 이름으로 버킷이 자동 작성되도록 <code>ibm.io/auto-create-bucket: true</code> 및 <code>ibm.io/bucket: ""</code>을 설정해야 합니다. </li><li><strong>false</strong>: PVC를 삭제하면 PV는 자동으로 삭제되지만 {{site.data.keyword.cos_full_notm}} 서비스 인스턴스의 데이터와 버킷은 그대로 유지됩니다. 데이터에 액세스하려면 기존 버킷의 이름으로 새 PVC를 작성해야 합니다. </li></ul>
   <tr>
   <td><code>ibm.io/bucket</code></td>
   <td>다음 선택사항 중 하나를 선택하십시오. <ul><li><code>ibm.io/auto-create-bucket</code>이 <strong>true</strong>로 설정된 경우: {{site.data.keyword.cos_full_notm}}에서 작성한 버킷의 이름을 입력하십시오. 또한 <code>ibm.io/auto-delete-bucket</code>이 <strong>true</strong>로 설정된 경우, 사용자는 버킷에 <code>tmp-s3fs-xxxx</code> 형식의 이름을 자동으로 지정할 수 있도록 이 필드를 공백 상태로 두어야 합니다. 이름은 {{site.data.keyword.cos_full_notm}}에서 고유해야 합니다. </li><li>
<code>ibm.io/auto-create-bucket</code>이 <strong>false</strong>로 설정된 경우: 클러스터에서 액세스할 기존 버킷의 이름을 입력하십시오. </li></ul> </td>
   </tr>
   <tr>
   <td><code>ibm.io/object-path</code></td>
   <td>선택사항: 마운트할 버킷의 기존 서브디렉토리 이름을 입력하십시오. 전체 버킷이 아닌 서브디렉토리만 마운트하려면 이 옵션을 사용하십시오. 서브디렉토리를 마운트하려면 <code>ibm.io/auto-create-bucket: "false"</code>를 설정하고 <code>ibm.io/bucket</code>의 버킷 이름을 제공해야 합니다. </li></ul> </td>
   </tr>
   <tr>
   <td><code>ibm.io/secret-name</code></td>
   <td>이전에 작성한 {{site.data.keyword.cos_full_notm}} 인증 정보를 보유하는 시크릿의 이름을 입력하십시오. </td>
   </tr>
   <tr>
  <td><code>ibm.io/endpoint</code></td>
  <td>클러스터와 다른 위치에 {{site.data.keyword.cos_full_notm}} 서비스 인스턴스를 작성한 경우 사용할 {{site.data.keyword.cos_full_notm}} 서비스 인스턴스의 사설 또는 공용 서비스 엔드포인트를 입력하십시오. 사용 가능한 서비스 엔드포인트의 개요는 [추가 엔드포인트 정보](/docs/services/cloud-object-storage?topic=cloud-object-storage-advanced-endpoints)를 참조하십시오. 기본적으로 <code>ibmc</code> Helm 플러그인은 자동으로 클러스터 위치를 검색하고 클러스터 위치와 일치하는 {{site.data.keyword.cos_full_notm}} 개인 서비스 엔드포인트를 사용하여 스토리지 클래스를 작성합니다. 클러스터가 메트로 도시 구역(예: `dal10`)에 위치한 경우 메트로 도시의 {{site.data.keyword.cos_full_notm}} 개인 서비스 엔드포인트(이 경우에서는 Dallas)가 사용됩니다. 스토리지 클래스의 서비스 엔드포인트가 서비스 인스턴스의 서비스 엔드포인트와 일치하는지 확인하려면 `kubectl describe storageclass <storageclassname>`을 실행하십시오. 개인 서비스 엔드포인트의 경우 `https://<s3fs_private_service_endpoint>`의 형식, 공용 서비스 엔드포인트의 경우 `http://<s3fs_public_service_endpoint>` 형식으로 서비스 엔드포인트를 입력해야 합니다. 스토리지 클래스의 서비스 엔드포인트가 {{site.data.keyword.cos_full_notm}} 서비스 인스턴스의 서비스 엔드포인트와 일치하는 경우 PVC YAML 파일에 <code>ibm.io/endpoint</code> 옵션을 포함하지 마십시오. </td>
  </tr>
   <tr>
   <td><code>resources.requests.storage</code></td>
   <td>{{site.data.keyword.cos_full_notm}} 버킷의 가상 크기(기가바이트)입니다. Kubernetes에서는 이 크기가 필요하지만, {{site.data.keyword.cos_full_notm}}에서는 무시됩니다. 원하는 크기를 입력할 수 있습니다. {{site.data.keyword.cos_full_notm}}에서 사용되는 실제 영역은 다를 수 있으며, [가격 테이블 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api)을 기반으로 비용이 청구됩니다. </td>
   </tr>
   <tr>
   <td><code>spec.storageClassName</code></td>
   <td>다음 선택사항 중 하나를 선택하십시오. <ul><li><code>ibm.io/auto-create-bucket</code>이 <strong>true</strong>로 설정된 경우: 새 버킷에 사용할 스토리지 클래스를 입력하십시오. </li><li><code>ibm.io/auto-create-bucket</code>이 <strong>false</strong>로 설정된 경우: 기존 버킷의 작성에 사용된 스토리지 클래스를 입력하십시오. </br></br>{{site.data.keyword.cos_full_notm}} 서비스 인스턴스에서 버킷을 수동으로 작성한 경우 또는 사용하던 스토리지 클래스가 기억나지 않는 경우에는 {{site.data.keyword.Bluemix}} 대시보드에서 서비스 인스턴스를 찾고 기존 버킷의 <strong>클래스</strong> 및 <strong>위치</strong>를 검토하십시오. 그리고 적합한 [스토리지 클래스](#cos_storageclass_reference)를 사용하십시오. <p class="note">스토리지 클래스에서 설정된 {{site.data.keyword.cos_full_notm}} API 엔드포인트는 클러스터가 있는 지역을 기반으로 합니다. 클러스터가 있는 지역이 아닌 다른 지역에 있는 버킷에 액세스하려면 [사용자 정의 스토리지 클래스](/docs/containers?topic=containers-kube_concepts#customized_storageclass)를 작성하고 버킷에 대한 적합한 API 엔드포인트를 사용해야 합니다.</p></li></ul>  </td>
   </tr>
   </tbody>
   </table>

2. PVC를 작성하십시오.
   ```
   kubectl apply -f filepath/pvc.yaml
   ```
   {: pre}

3. PVC가 작성되고 PV에 바인딩되는지 확인하십시오.
   ```
   kubectl get pvc
   ```
   {: pre}

   출력 예:
   ```
   NAME                  STATUS    VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS                     AGE
   s3fs-test-pvc         Bound     pvc-b38b30f9-1234-11e8-ad2b-t910456jbe12   8Gi        RWO            ibmc-s3fs-standard-cross-region  1h
   ```
   {: screen}

4. 선택사항: 루트가 아닌 사용자로 데이터에 액세스할 계획이거나 콘솔 또는 API를 사용하여 기존 {{site.data.keyword.cos_full_notm}} 버킷에 직접 파일을 추가한 경우에는 필요 시에 앱이 파일을 성공적으로 읽고 업데이트할 수 있도록 [파일에 올바른 권한이 지정되어 있는지](/docs/containers?topic=containers-cs_troubleshoot_storage#cos_nonroot_access) 확인하십시오.

4.  {: #cos_app_volume_mount}PV를 배치에 마운트하려면 구성 `.yaml` 파일을 작성하고 PV를 바인드하는 PVC를 지정하십시오.

    ```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: <deployment_name>
      labels:
        app: <deployment_label>
    spec:
      selector:
        matchLabels:
          app: <app_name>
      template:
        metadata:
          labels:
            app: <app_name>
        spec:
          containers:
          - image: <image_name>
            name: <container_name>
            securityContext:
              runAsUser: <non_root_user>
              fsGroup: <non_root_user> #only applicable for clusters that run Kubernetes version 1.13 or later
            volumeMounts:
            - name: <volume_name>
              mountPath: /<file_path>
          volumes:
          - name: <volume_name>
            persistentVolumeClaim:
              claimName: <pvc_name>
    ```
    {: codeblock}

    <table>
    <caption>YAML 파일 컴포넌트 이해</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> YAML 파일 컴포넌트 이해</th>
    </thead>
    <tbody>
        <tr>
    <td><code>metadata.labels.app</code></td>
    <td>배치의 레이블입니다.</td>
      </tr>
      <tr>
        <td><code>spec.selector.matchLabels.app</code> <br/> <code>spec.template.metadata.labels.app</code></td>
        <td>앱의 레이블입니다.</td>
      </tr>
    <tr>
    <td><code>template.metadata.labels.app</code></td>
    <td>배치의 레이블입니다.</td>
      </tr>
    <tr>
    <td><code>spec.containers.image</code></td>
    <td>사용하려는 이미지의 이름입니다. {{site.data.keyword.registryshort_notm}} 계정에서 사용 가능한 이미지를 나열하려면 `ibmcloud cr image-list`를 실행하십시오.</td>
    </tr>
    <tr>
    <td><code>spec.containers.name</code></td>
    <td>클러스터에 배치하려는 컨테이너의 이름입니다.</td>
    </tr>
    <tr>
    <td><code>spec.containers.securityContext.runAsUser</code></td>
    <td>선택사항: Kubernetes 버전 1.12 이하를 실행하는 클러스터에서 루트가 아닌 사용자로 앱을 실행하려면 동시에 배치 YAML에 `fsGroup`을 설정하지 않고 루트가 아닌 사용자를 정의하여 팟(Pod)에 대한 [보안 컨텍스트 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/)를 지정하십시오. `fsGroup`을 설정하면 팟(Pod) 배치 시에 버킷의 모든 파일에 대한 그룹 권한을 업데이트하도록 {{site.data.keyword.cos_full_notm}} 플러그인이 트리거됩니다. 권한 업데이트는 쓰기 오퍼레이션이며 성능에 영향을 줍니다. 보유한 파일의 수에 따라서는 권한 업데이트 때문에 팟(Pod)이 구동되어 <code>Running</code> 상태가 되지 못할 수 있습니다. </br></br>Kubernetes 버전 1.13 이상을 실행하고 {{site.data.keyword.Bluemix_notm}} Object Storage 플러그인 버전 1.0.4 이상을 실행하는 클러스터가 있는 경우 s3fs 마운트 지점의 소유자를 변경할 수 있습니다. 소유자를 변경하려면 `runAsUser` 및 `fsGroup`을 s3fs 마운트 지점을 소유할 동일한 비루트 사용자 ID로 설정하여 보안 컨텍스트를 지정하십시오. 두 값이 일치하지 않으면 마운트 지점은 자동으로 `root` 사용자가 소유합니다.  </td>
    </tr>
    <tr>
    <td><code>spec.containers.volumeMounts.mountPath</code></td>
    <td>컨테이너 내에서 볼륨이 마운트되는 디렉토리의 절대 경로입니다. 다른 앱 간에 볼륨을 공유하려는 경우 각각의 앱에 대해 [볼륨 하위 경로![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/storage/volumes/#using-subpath)를 지정할 수 있습니다.</td>
    </tr>
    <tr>
    <td><code>spec.containers.volumeMounts.name</code></td>
    <td>팟(Pod)에 마운트할 볼륨의 이름입니다.</td>
    </tr>
    <tr>
    <td><code>volumes.name</code></td>
    <td>팟(Pod)에 마운트할 볼륨의 이름입니다. 일반적으로 이 이름은 <code>volumeMounts/name</code>과 동일합니다.</td>
    </tr>
    <tr>
    <td><code>volumes.persistentVolumeClaim.claimName</code></td>
    <td>사용하려는 PV를 바인드하는 PVC의 이름입니다. </td>
    </tr>
    </tbody></table>

5.  배치를 작성하십시오.
     ```
     kubectl apply -f <local_yaml_path>
     ```
     {: pre}

6.  PV가 성공적으로 마운트되었는지 확인하십시오.

     ```
     kubectl describe deployment <deployment_name>
     ```
     {: pre}

     마운트 지점은 **Volume Mounts** 필드에 있고 볼륨은 **Volumes** 필드에 있습니다.

     ```
      Volume Mounts:
           /var/run/secrets/kubernetes.io/serviceaccount from default-token-tqp61 (ro)
           /volumemount from myvol (rw)
     ...
     Volumes:
       myvol:
         Type:	PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
         ClaimName:	mypvc
         ReadOnly:	false
     ```
     {: screen}

7. {{site.data.keyword.cos_full_notm}} 서비스 인스턴스에 데이터를 쓸 수 있는지 확인하십시오.
   1. PV를 마운트하는 팟(Pod)에 로그인하십시오.
      ```
      kubectl exec <pod_name> -it bash
      ```
      {: pre}

   2. 앱 배치에서 정의한 볼륨 마운트 경로로 이동하십시오.
   3. 텍스트 파일을 작성하십시오.
      ```
      echo "This is a test" > test.txt
      ```
      {: pre}

   4. {{site.data.keyword.Bluemix}} 대시보드에서 {{site.data.keyword.cos_full_notm}} 서비스 인스턴스로 이동하십시오.
   5. 메뉴에서 **버킷**을 선택하십시오.
   6. 버킷을 열고 작성된 `test.txt`가 나타나는지 확인하십시오.


## Stateful 세트에서의 오브젝트 스토리지 사용
{: #cos_statefulset}

데이터베이스와 같은 stateful 앱이 있는 경우에는 앱의 데이터를 저장하기 위해 {{site.data.keyword.cos_full_notm}}를 사용하는 Stateful 세트를 작성할 수 있습니다. 또는, {{site.data.keyword.cloudant_short_notm}}와 같은 {{site.data.keyword.Bluemix_notm}} DBaaS(Database-as-a-Service)를 사용하여 데이터를 클라우드에 저장할 수 있습니다.
{: shortdesc}

시작하기 전에:
- [{{site.data.keyword.cos_full_notm}} 서비스 인스턴스를 작성하고 준비](#create_cos_service)하십시오.
- [{{site.data.keyword.cos_full_notm}} 서비스 인증 정보를 저장할 시크릿을 작성](#create_cos_secret)하십시오.
- [{{site.data.keyword.cos_full_notm}}의 구성을 결정](#configure_cos)하십시오.

오브젝트 스토리지를 사용하는 Stateful 세트를 배치하려면 다음을 수행하십시오.

1. Stateful 세트에 대한 구성 파일과 이 Stateful 세트를 노출하는 데 사용하는 서비스를 작성하십시오. 다음 예는 NGINX를 3개의 복제본을 포함하는 Stateful 세트(각 복제본이 별도의 버킷을 사용하거나 모든 복제본이 동일한 버킷을 공유)로 배치하는 방법을 보여줍니다.

   **3개의 복제본을 포함하는 Stateful 세트 작성 예(각 복제본이 별도의 버킷 사용)**:
   ```
   apiVersion: v1
   kind: Service
   metadata:
     name: nginx-v01
     namespace: default
     labels:
       app: nginx-v01 # must match spec.template.metadata.labels and spec.selector.matchLabels in stateful set YAML
   spec:
     ports:
     - port: 80
       name: web
     clusterIP: None
     selector:
       app: nginx-v01 # must match spec.template.metadata.labels and spec.selector.matchLabels in stateful set YAML
   ---
   apiVersion: apps/v1
   kind: StatefulSet
   metadata:
     name: web-v01
     namespace: default
   spec:
     selector:
       matchLabels:
         app: nginx-v01 # must match spec.template.metadata.labels in stateful set YAML and metadata.labels in service YAML
     serviceName: "nginx-v01"
     replicas: 3
     template:
       metadata:
         labels:
           app: nginx-v01 # must match spec.selector.matchLabels in stateful set YAML and metadata.labels in service YAML
       spec:
         terminationGracePeriodSeconds: 10
         containers:
         - name: nginx
           image: k8s.gcr.io/nginx-slim:0.8
           ports:
           - containerPort: 80
             name: web
           volumeMounts:
           - name: mypvc
             mountPath: /usr/share/nginx/html
     volumeClaimTemplates:
     - metadata:
         name: mypvc
         annotations:
           ibm.io/auto-create-bucket: "true"
           ibm.io/auto-delete-bucket: "true"
           ibm.io/bucket: ""
           ibm.io/secret-name: mysecret 
           volume.beta.kubernetes.io/storage-class: ibmc-s3fs-standard-perf-cross-region 
           volume.beta.kubernetes.io/storage-provisioner: ibm.io/ibmc-s3fs
       spec:
         accessModes: [ "ReadWriteOnce" ]
         storageClassName: "ibmc-s3fs-standard-perf-cross-region"
         resources:
           requests:
             storage: 1Gi
   ```
   {: codeblock}

   **3개의 복제본을 포함하는 Stateful 세트 작성 예(모든 복제본이 `mybucket`이라는 동일한 버킷 공유)**:
   ```
   apiVersion: v1
   kind: Service
   metadata:
     name: nginx-v01
     namespace: default
     labels:
       app: nginx-v01 # must match spec.template.metadata.labels and spec.selector.matchLabels in stateful set YAML
   spec:
     ports:
     - port: 80
       name: web
     clusterIP: None
     selector:
       app: nginx-v01 # must match spec.template.metadata.labels and spec.selector.matchLabels in stateful set YAML
   --- 
   apiVersion: apps/v1
   kind: StatefulSet
   metadata:
     name: web-v01
     namespace: default
   spec:
     selector:
       matchLabels:
         app: nginx-v01 # must match spec.template.metadata.labels in stateful set YAML and metadata.labels in service YAML
     serviceName: "nginx-v01"
     replicas: 3 
     template:
       metadata:
         labels:
           app: nginx-v01 # must match spec.selector.matchLabels in stateful set YAML and metadata.labels in service YAML
       spec:
         terminationGracePeriodSeconds: 10
         containers:
         - name: nginx
           image: k8s.gcr.io/nginx-slim:0.8
           ports:
           - containerPort: 80
             name: web
           volumeMounts:
           - name: mypvc
             mountPath: /usr/share/nginx/html
     volumeClaimTemplates:
     - metadata:
         name: mypvc
         annotations:
           ibm.io/auto-create-bucket: "false"
           ibm.io/auto-delete-bucket: "false"
           ibm.io/bucket: mybucket
           ibm.io/secret-name: mysecret
           volume.beta.kubernetes.io/storage-class: ibmc-s3fs-standard-perf-cross-region 
           volume.beta.kubernetes.io/storage-provisioner: ibm.io/ibmc-s3fs
       spec:
         accessModes: [ "ReadOnlyMany" ]
         storageClassName: "ibmc-s3fs-standard-perf-cross-region"
         resources:
           requests:
             storage: 1Gi
   ```
   {: codeblock}


   <table>
    <caption>Stateful 세트 YAML 파일 컴포넌트 이해</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> Stateful 세트 YAML 파일 컴포넌트 이해</th>
    </thead>
    <tbody>
    <tr>
    <td style="text-align:left"><code>metadata.name</code></td>
    <td style="text-align:left">Stateful 세트의 이름을 입력하십시오. 입력하는 이름은 <code>&lt;volume_name&gt;-&lt;statefulset_name&gt;-&lt;replica_number&gt;</code> 형식으로 PVC의 이름을 작성하는 데 사용됩니다. </td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.serviceName</code></td>
    <td style="text-align:left">Stateful 세트를 노출하는 데 사용할 서비스의 이름을 입력하십시오. </td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.replicas</code></td>
    <td style="text-align:left">Stateful 세트의 복제본 수를 입력하십시오. </td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.selector.matchLabels</code></td>
    <td style="text-align:left">Stateful 세트 및 PVC에 포함시킬 모든 레이블을 입력하십시오. Stateful 세트의 <code>volumeClaimTemplates</code>에 포함하는 레이블은 Kubernetes가 인식하지 않습니다. 대신 Stateful 세트 YAML의 <code>spec.selector.matchLabels</code> 및 <code>spec.template.metadata.labels</code> 섹션에서 이러한 레이블을 정의해야 합니다. 서비스의 로드 밸런싱에 모든 Stateful 세트 복제본이 포함되도록 하려면 서비스 YAML의 <code>spec.selector</code> 섹션에서 사용한 것과 동일한 레이블을 포함하십시오. </td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.template.metadata.labels</code></td>
    <td style="text-align:left">Stateful 세트 YAML의 <code>spec.selector.matchLabels</code> 섹션에 추가한 것과 동일한 레이블을 입력하십시오. </td>
    </tr>
    <tr>
    <td><code>spec.template.spec.</code></br><code>terminationGracePeriodSeconds</code></td>
    <td><code>kubelet</code>이 Stateful 세트 복제본을 실행하는 팟(Pod)을 단계적으로 종료할 수 있도록 허용할 시간(초)을 입력하십시오. 자세한 정보는 [Delete Pods ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/tasks/run-application/force-delete-stateful-set-pod/#delete-pods)를 참조하십시오. </td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.volumeClaimTemplates.</code></br><code>metadata.name</code></td>
    <td style="text-align:left">볼륨 이름을 입력하십시오. <code>spec.containers.volumeMount.name</code> 섹션에 정의한 것과 동일한 이름을 사용하십시오. 여기에 입력하는 이름은 <code>&lt;volume_name&gt;-&lt;statefulset_name&gt;-&lt;replica_number&gt;</code> 형식으로 PVC의 이름을 작성하는 데 사용됩니다. </td>
    </tr>
    <tr>
    <td><code>spec.volumeClaimTemplates.metadata</code></br><code>annotions.ibm.io/auto-create-bucket</code></td>
    <td>다음 선택사항 중 하나를 선택하십시오. <ul><li><strong>true: </strong>각 Stateful 세트 복제본에 대해 자동으로 버킷을 작성하려면 이 옵션을 선택하십시오. </li><li><strong>false: </strong>전체 Stateful 세트 복제본이 기존 버킷을 공유하도록 하려면 이 옵션을 선택하십시오. Stateful 세트 YAML의 <code>spec.volumeClaimTemplates.metadata.annotions.ibm.io/bucket</code> 섹션에 버킷의 이름을 반드시 정의해야 합니다.</li></ul></td>
    </tr>
    <tr>
    <td><code>spec.volumeClaimTemplates.metadata</code></br><code>annotions.ibm.io/auto-delete-bucket</code></td>
    <td>다음 선택사항 중 하나를 선택하십시오. <ul><li><strong>true: </strong>PVC를 삭제할 때 데이터, 버킷 및 PV가 자동으로 제거됩니다. {{site.data.keyword.cos_full_notm}} 서비스 인스턴스는 그대로 유지되며 삭제되지 않습니다. 이 옵션을 true로 설정하도록 선택한 경우에는 <code>tmp-s3fs-xxxx</code> 형식의 이름으로 버킷이 자동 작성되도록 <code>ibm.io/auto-create-bucket: true</code> 및 <code>ibm.io/bucket: ""</code>을 설정해야 합니다. </li><li><strong>false: </strong>PVC를 삭제하면 PV는 자동으로 삭제되지만 {{site.data.keyword.cos_full_notm}} 서비스 인스턴스의 데이터와 버킷은 그대로 유지됩니다. 데이터에 액세스하려면 기존 버킷의 이름으로 새 PVC를 작성해야 합니다.</li></ul></td>
    </tr>
    <tr>
    <td><code>spec.volumeClaimTemplates.metadata</code></br><code>annotions.ibm.io/bucket</code></td>
    <td>다음 선택사항 중 하나를 선택하십시오. <ul><li><strong><code>ibm.io/auto-create-bucket</code>이 true로 설정된 경우: </strong>{{site.data.keyword.cos_full_notm}}에서 작성한 버킷의 이름을 입력하십시오. <code>ibm.io/auto-delete-bucket</code>도 <strong>true</strong>로 설정된 경우에는 버킷에 tmp-s3fs-xxxx 형식의 이름을 자동으로 지정할 수 있도록 이 필드를 공백 상태로 두어야 합니다. 이름은 {{site.data.keyword.cos_full_notm}}에서 고유해야 합니다.</li><li><strong><code>ibm.io/auto-create-bucket</code>이 false로 설정된 경우: </strong>클러스터에서 액세스할 기존 버킷의 이름을 입력하십시오.</li></ul></td>
    </tr>
    <tr>
    <td><code>spec.volumeClaimTemplates.metadata</code></br><code>annotions.ibm.io/secret-name</code></td>
    <td>이전에 작성한 {{site.data.keyword.cos_full_notm}} 인증 정보를 보유하는 시크릿의 이름을 입력하십시오.</td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.volumeClaimTemplates.metadata.</code></br><code>annotations.volume.beta.</code></br><code>kubernetes.io/storage-class</code></td>
    <td style="text-align:left">사용할 스토리지 클래스를 입력하십시오. 다음 선택사항 중 하나를 선택하십시오. <ul><li><strong><code>ibm.io/auto-create-bucket</code>이 true로 설정된 경우: </strong>새 버킷에 사용할 스토리지 클래스를 입력하십시오.</li><li><strong><code>ibm.io/auto-create-bucket</code>이 false로 설정된 경우: </strong>기존 버킷을 작성하는 데 사용한 스토리지 클래스를 입력하십시오. </li></ul></br> 기존 스토리지 클래스를 나열하려면 <code>kubectl get storageclasses | grep s3</code>을 실행하십시오. 스토리지 클래스를 지정하지 않으면 PVC가 클러스터에 설정된 기본 스토리지 클래스를 사용하여 작성됩니다. Stateful 세트가 오브젝트 스토리지를 사용하여 프로비저닝되도록 기본 스토리지 클래스가 <code>ibm.io/ibmc-s3fs</code> 프로비저너를 사용하는지 확인하십시오.</td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.volumeClaimTemplates.</code></br><code>spec.storageClassName</code></td>
    <td>Stateful 세트 YAML의 <code>spec.volumeClaimTemplates.metadata.annotations.volume.beta.kubernetes.io/storage-class</code> 섹션에 입력한 것과 동일한 스토리지 클래스를 입력하십시오.  </td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.volumeClaimTemplates.spec.</code></br><code>resource.requests.storage</code></td>
    <td>{{site.data.keyword.cos_full_notm}} 버킷의 가상 크기(기가바이트)를 입력하십시오. Kubernetes에서는 이 크기가 필요하지만, {{site.data.keyword.cos_full_notm}}에서는 무시됩니다. 원하는 크기를 입력할 수 있습니다. {{site.data.keyword.cos_full_notm}}에서 사용되는 실제 영역은 다를 수 있으며, [가격 테이블 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api)을 기반으로 비용이 청구됩니다.</td>
    </tr>
    </tbody></table>


## 데이터 백업 및 복원
{: #cos_backup_restore}

{{site.data.keyword.cos_full_notm}}가 데이터에 대한 높은 내구성을 제공하도록 설정되어 있으므로, 이는 데이터가 유실되지 않도록 방지합니다. [{{site.data.keyword.cos_full_notm}} 서비스 이용 약관 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www-03.ibm.com/software/sla/sladb.nsf/sla/bm-7857-03)에서 SLA를 찾을 수 있습니다.
{: shortdesc}

{{site.data.keyword.cos_full_notm}}에서는 데이터에 대한 버전 히스토리를 제공하지 않습니다. 데이터의 이전 버전을 유지보수하고 액세스해야 하는 경우에는 데이터 히스토리를 관리하거나 대체 백업 솔루션을 구현하도록 앱을 설정해야 합니다. 예를 들어, 사용자가 온프레미스 데이터베이스에 {{site.data.keyword.cos_full_notm}} 데이터를 저장하거나 테이프를 사용하여 데이터를 아카이브하고자 할 수 있습니다.
{: note}

## 스토리지 클래스 참조
{: #cos_storageclass_reference}

### 표준
{: #standard}

<table>
<caption>오브젝트 스토리지 클래스: 표준</caption>
<thead>
<th>특성</th>
<th>설정</th>
</thead>
<tbody>
<tr>
<td>이름</td>
<td><code>ibmc-s3fs-standard-cross-region</code></br><code>ibmc-s3fs-standard-perf-cross-region</code></br><code>ibmc-s3fs-standard-regional</code></br><code>ibmc-s3fs-standard-perf-regional</code></td>
</tr>
<tr>
<td>기본 복원성 엔드포인트</td>
<td>복원성 엔드포인트는 클러스터가 있는 위치에 따라 자동으로 설정됩니다. 자세한 정보는 [지역 및 엔드포인트](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-endpoints#endpoints)를 참조하십시오. </td>
</tr>
<tr>
<td>청크 크기</td>
<td>`perf`가 없는 스토리지 클래스: 16MB</br>`perf`가 있는 스토리지 클래스: 52MB</td>
</tr>
<tr>
<td>커널 캐시</td>
<td>사용</td>
</tr>
<tr>
<td>비용 청구</td>
<td>매월</td>
</tr>
<tr>
<td>가격</td>
<td>[가격 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api)</td>
</tr>
</tbody>
</table>

### Vault
{: #Vault}

<table>
<caption>오브젝트 스토리지 클래스: vault</caption>
<thead>
<th>특성</th>
<th>설정</th>
</thead>
<tbody>
<tr>
<td>이름</td>
<td><code>ibmc-s3fs-vault-cross-region</code></br><code>ibmc-s3fs-vault-regional</code></td>
</tr>
<tr>
<td>기본 복원성 엔드포인트</td>
<td>복원성 엔드포인트는 클러스터가 있는 위치에 따라 자동으로 설정됩니다. 자세한 정보는 [지역 및 엔드포인트](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-endpoints#endpoints)를 참조하십시오. </td>
</tr>
<tr>
<td>청크 크기</td>
<td>16MB</td>
</tr>
<tr>
<td>커널 캐시</td>
<td>사용 안함</td>
</tr>
<tr>
<td>비용 청구</td>
<td>매월</td>
</tr>
<tr>
<td>가격</td>
<td>[가격 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api)</td>
</tr>
</tbody>
</table>

### Cold
{: #cold}

<table>
<caption>오브젝트 스토리지 클래스: cold</caption>
<thead>
<th>특성</th>
<th>설정</th>
</thead>
<tbody>
<tr>
<td>이름</td>
<td><code>ibmc-s3fs-flex-cross-region</code></br><code>ibmc-s3fs-flex-perf-cross-region</code></br><code>ibmc-s3fs-flex-regional</code></br><code>ibmc-s3fs-flex-perf-regional</code></td>
</tr>
<tr>
<td>기본 복원성 엔드포인트</td>
<td>복원성 엔드포인트는 클러스터가 있는 위치에 따라 자동으로 설정됩니다. 자세한 정보는 [지역 및 엔드포인트](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-endpoints#endpoints)를 참조하십시오. </td>
</tr>
<tr>
<td>청크 크기</td>
<td>16MB</td>
</tr>
<tr>
<td>커널 캐시</td>
<td>사용 안함</td>
</tr>
<tr>
<td>비용 청구</td>
<td>매월</td>
</tr>
<tr>
<td>가격</td>
<td>[가격 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api)</td>
</tr>
</tbody>
</table>

### Flex
{: #flex}

<table>
<caption>오브젝트 스토리지 클래스: flex</caption>
<thead>
<th>특성</th>
<th>설정</th>
</thead>
<tbody>
<tr>
<td>이름</td>
<td><code>ibmc-s3fs-cold-cross-region</code></br><code>ibmc-s3fs-flex-perf-cross-region</code></br><code>ibmc-s3fs-cold-regional</code></br><code>ibmc-s3fs-flex-perf-regional</code></td>
</tr>
<tr>
<td>기본 복원성 엔드포인트</td>
<td>복원성 엔드포인트는 클러스터가 있는 위치에 따라 자동으로 설정됩니다. 자세한 정보는 [지역 및 엔드포인트](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-endpoints#endpoints)를 참조하십시오. </td>
</tr>
<tr>
<td>청크 크기</td>
<td>`perf`가 없는 스토리지 클래스: 16MB</br>`perf`가 있는 스토리지 클래스: 52MB</td>
</tr>
<tr>
<td>커널 캐시</td>
<td>사용</td>
</tr>
<tr>
<td>비용 청구</td>
<td>매월</td>
</tr>
<tr>
<td>가격</td>
<td>[가격 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api)</td>
</tr>
</tbody>
</table>
