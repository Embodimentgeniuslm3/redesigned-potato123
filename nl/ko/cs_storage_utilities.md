---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: kubernetes, iks, local persistent storage

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



# IBM Cloud 스토리지 유틸리티
{: #utilities}

## IBM Cloud Block Storage Attacher 플러그인(베타) 설치
{: #block_storage_attacher}

{{site.data.keyword.Bluemix_notm}} Block Storage Attacher 플러그인을 사용하여 클러스터의 작업자 노드에 원시, 포맷되지 않은 그리고 마운트 해제된 블록 스토리지를 연결합니다.  
{: shortdesc}

예를 들어, [Portworx](/docs/containers?topic=containers-portworx)와 같은 소프트웨어 정의 스토리지 솔루션(SDS)를 사용하여 데이터를 저장하려고 하지만 SDS 사용에 최적화되고 추가 로컬 디스크가 있는 베어메탈 작업자 노드를 사용하려고 하지 않습니다. 비 SDS 작업자 노드에 로컬 디스크를 추가하려면 블록 스토리지 디바이스를 {{site.data.keyword.Bluemix_notm}} 인프라 계정에 수동으로 작성하고 {{site.data.keyword.Bluemix_notm}} Block Volume Attacher를 사용하여 스토리지를 비 SDS 작업자 노드에 연결해야 합니다.

{{site.data.keyword.Bluemix_notm}} Block Volume Attacher 플러그인은 디먼 세트의 일부로 클러스터의 모든 작업자 노드에 팟(Pod)을 작성하고 나중에 블록 스토리지 디바이스를 비 SDS 작업자 노드에 연결하는 데 사용할 Kubernetes 스토리지 클래스를 설정합니다.

{{site.data.keyword.Bluemix_notm}} Block Volume Attacher 플러그인의 업데이트 또는 제거 방법에 대한 지시사항을 찾으십니까? [플러그인 업데이트](#update_block_attacher) 및 [플러그인 제거](#remove_block_attacher)를 참조하십시오.
{: tip}

1.  [지시사항에 따라](/docs/containers?topic=containers-helm#public_helm_install) 로컬 시스템에 Helm 클라이언트를 설치하고 클러스터에 서비스 계정이 있는 Helm 서버(tiller)를 설치하십시오.

2.  Tiller가 서비스 계정으로 설치되어 있는지 확인하십시오.

    ```
    kubectl get serviceaccount -n kube-system | grep tiller
    ```
    {: pre}

    출력 예:

    ```
    NAME                                 SECRETS   AGE
    tiller                               1         2m
    ```
    {: screen}

3. Helm 저장소를 업데이트하여 이 저장소에 있는 모든 Helm 차트의 최신 버전을 검색하십시오.
   ```
   helm repo update
   ```
   {: pre}

4. {{site.data.keyword.Bluemix_notm}} Block Volume Attacher 플러그인을 설치하십시오. 플러그인을 설치하면 사전 정의된 블록 스토리지 클래스가 클러스터에 추가됩니다.
   ```
   helm install iks-charts/ibm-block-storage-attacher --name block-attacher
   ```
   {: pre}

   출력 예:
   ```
   NAME:   block-volume-attacher
   LAST DEPLOYED: Thu Sep 13 22:48:18 2018
   NAMESPACE: default
   STATUS: DEPLOYED

   RESOURCES:
   ==> v1beta1/ClusterRoleBinding
   NAME                             AGE
   ibmcloud-block-storage-attacher  1s

   ==> v1beta1/DaemonSet
   NAME                             DESIRED  CURRENT  READY  UP-TO-DATE  AVAILABLE  NODE SELECTOR  AGE
   ibmcloud-block-storage-attacher  0        0        0      0           0          <none>         1s

   ==> v1/StorageClass
   NAME                 PROVISIONER                AGE
   ibmc-block-attacher  ibm.io/ibmc-blockattacher  1s

   ==> v1/ServiceAccount
   NAME                             SECRETS  AGE
   ibmcloud-block-storage-attacher  1        1s

   ==> v1beta1/ClusterRole
   NAME                             AGE
   ibmcloud-block-storage-attacher  1s

   NOTES:
   Thank you for installing: ibmcloud-block-storage-attacher.   Your release is named: block-volume-attacher

   Please refer Chart README.md file for attaching a block storage
   Please refer Chart RELEASE.md to see the release details/fixes
   ```
   {: screen}

5. {{site.data.keyword.Bluemix_notm}} Block Volume Attacher 디먼 세트가 설치되었는지 확인하십시오.
   ```
   kubectl get pod -n kube-system -o wide | grep attacher
   ```
   {: pre}

   출력 예:
   ```
   ibmcloud-block-storage-attacher-z7cv6           1/1       Running            0          19m
   ```
   {: screen}

   하나 이상의 **ibmcloud-block-storage-attacher** 팟(Pod)이 나타나면 설치가 완료된 것입니다. 팟(Pod)의 수는 클러스터의 작업자 노드 수와 동일합니다. 모든 팟(Pod)이 **실행 중(Running)** 상태여야 합니다.

6. {{site.data.keyword.Bluemix_notm}} Block Volume Attacher에 대한 스토리지 클래스가 작성되었는지 확인하십시오.
   ```
   kubectl get storageclasses | grep attacher
   ```
   {: pre}

   출력 예:
   ```
   ibmc-block-attacher       ibm.io/ibmc-blockattacher   11m
   ```
   {: screen}

### IBM Cloud Block Storage Attacher 플러그인 업데이트
{: #update_block_attacher}

기존 {{site.data.keyword.Bluemix_notm}} Block Storage Attacher 플러그인을 최신 버전으로 업그레이드할 수 있습니다.
{: shortdesc}

1. Helm 저장소를 업데이트하여 이 저장소에 있는 모든 Helm 차트의 최신 버전을 검색하십시오.
   ```
   helm repo update
   ```
   {: pre}

2. 선택사항: 최신 Helm 차트를 로컬 머신에 다운로드하십시오. 그런 다음, 패키지의 압축을 풀고 `release.md` 파일을 검토하여 최신 릴리스 정보를 찾으십시오.
   ```
   helm fetch iks-charts/ibmcloud-block-storage-plugin
   ```
   {: pre}

3. {{site.data.keyword.Bluemix_notm}} Block Storage Attacher 플러그인에 대한 Helm 차트의 이름을 찾으십시오.
   ```
   helm ls | grep ibm-block-storage-attacher
   ```
   {: pre}

   출력 예:
   ```
   <helm_chart_name>	1       	Wed Aug  1 14:55:15 2018	DEPLOYED	ibm-block-storage-attacher-1.0.0	default
   ```
   {: screen}

4. {{site.data.keyword.Bluemix_notm}} Block Storage Attacher를 최신 버전으로 업그레이드하십시오.
   ```
   helm upgrade --force --recreate-pods <helm_chart_name> ibm-block-storage-attacher
   ```
   {: pre}

### IBM Cloud Block Volume Attacher 플러그인 제거
{: #remove_block_attacher}

클러스터에서 {{site.data.keyword.Bluemix_notm}} Block Storage Attacher 플러그인의 프로비저닝과 사용을 원하지 않으면 Helm 차트를 설치 제거할 수 있습니다.
{: shortdesc}

1. {{site.data.keyword.Bluemix_notm}} Block Storage Attacher 플러그인에 대한 Helm 차트의 이름을 찾으십시오.
   ```
   helm ls | grep ibm-block-storage-attacher
   ```
   {: pre}

   출력 예:
   ```
   <helm_chart_name>	1       	Wed Aug  1 14:55:15 2018	DEPLOYED	ibm-block-storage-attacher-1.0.0	default
   ```
   {: screen}

2. Helm 차트를 제거하여 {{site.data.keyword.Bluemix_notm}} Block Storage Attacher 플러그인을 삭제하십시오.
   ```
   helm delete <helm_chart_name> --purge
   ```
   {: pre}

3. {{site.data.keyword.Bluemix_notm}} Block Storage Attacher 팟(Pod)이 제거되었는지 확인하십시오.
   ```
   kubectl get pod -n kube-system -o wide | grep attacher
   ```
   {: pre}

      CLI 출력에 팟(Pod)이 표시되지 않으면 팟(Pod) 제거가 성공한 것입니다.

4. {{site.data.keyword.Bluemix_notm}} Block Storage Attacher 스토리지 클래스가 제거되었는지 확인하십시오.
   ```
   kubectl get storageclasses | grep attacher
   ```
   {: pre}

   CLI 출력에 스토리지 클래스가 표시되지 않으면 스토리지 클래스 제거가 성공한 것입니다.

## 포맷되지 않은 블록 스토리지 자동 프로비저닝 및 작업자 노드가 스토리지에 액세스할 수 있도록 권한 부여
{: #automatic_block}

{{site.data.keyword.Bluemix_notm}} Block Volume Attacher 플러그인을 사용하여 자동으로 클러스터의 모든 작업자 노드에 동일한 구성으로 원시, 포맷되지 않은 그리고 마운트 해제된 블록 스토리지를 추가할 수 있습니다.
{: shortdesc}

{{site.data.keyword.Bluemix_notm}} Block Volume Attacher 플러그인에 포함된 `mkpvyaml` 컨테이너는 클러스터의 모든 작업자 노드를 찾고 {{site.data.keyword.Bluemix_notm}} 인프라 포털에 원시 블록 스토리지를 작성한 후 스토리지에 액세스하도록 작업자 노드에 권한 부여하는 스크립트를 실행하도록 구성되어 있습니다.

다른 블록 스토리지 구성을 추가하려면, 블록 스토리지를 작업자 노드의 서브세트에만 추가하거나 프로비저닝 프로세스를 보다 더 제어할 수 있도록 [수동으로 블록 스토리지를 추가](#manual_block)하십시오.
{: tip}


1. {{site.data.keyword.Bluemix_notm}}에 로그인하고 클러스터가 있는 리소스 그룹을 대상으로 지정하십시오.
   ```
   ibmcloud login
   ```
   {: pre}

2.  {{site.data.keyword.Bluemix_notm}} Storage Utilities 저장소를 복제하십시오.
    ```
    git clone https://github.com/IBM/ibmcloud-storage-utilities.git
    ```
    {: pre}

3. `block-storage-utilities` 디렉토리로 이동하십시오.
   ```
   cd ibmcloud-storage-utilities/block-storage-provisioner
   ```
   {: pre}

4. `yamlgen.yaml` 파일을 열고 클러스터의 모든 작업자 노드에 추가할 블록 스토리지 구성을 지정하십시오.
   ```
   #
   # Can only specify 'performance' OR 'endurance' and associated clause
   #
   cluster:  <cluster_name>    #  Enter the name of your cluster
   region:   <region>          #  Enter the IBM Cloud Kubernetes Service region where you created your cluster
   type:  <storage_type>       #  Enter the type of storage that you want to provision. Choose between "performance" or "endurance"
   offering: storage_as_a_service
   # performance:
   #    - iops:  <iops>
   endurance:
     - tier:  2                #   [0.25|2|4|10]
   size:  [ 20 ]               #   Enter the size of your storage in gigabytes
   ```
   {: codeblock}

   <table>
   <caption>YAML 파일 컴포넌트 이해</caption>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> YAML 파일 컴포넌트 이해</th>
   </thead>
   <tbody>
   <tr>
   <td><code>클러스터</code></td>
   <td>원시 블록 스토리지를 추가할 클러스터의 이름을 입력하십시오. </td>
   </tr>
   <tr>
   <td><code>지역</code></td>
   <td>클러스터를 작성한 {{site.data.keyword.containerlong_notm}} 지역을 입력하십시오. <code>[bxcs] cluster-get --cluster &lt;cluster_name_or_ID&gt;</code>를 실행하여 클러스터의 지역을 찾으십시오.  </td>
   </tr>
   <tr>
   <td><code>type</code></td>
   <td>프로비저닝하고자 하는 스토리지의 유형을 입력하십시오. <code>performance</code> 또는 <code>endurance</code> 중에서 선택하십시오. 자세한 정보는 [블록 스토리지 구성 결정](/docs/containers?topic=containers-block_storage#block_predefined_storageclass)을 참조하십시오.  </td>
   </tr>
   <tr>
   <td><code>performance.iops</code></td>
   <td>`performance` 스토리지를 프로비저닝하려면 IOPS 수를 입력하십시오. 자세한 정보는 [블록 스토리지 구성 결정](/docs/containers?topic=containers-block_storage#block_predefined_storageclass)을 참조하십시오. `endurance` 스토리지를 프로비저닝하려면 이 섹션을 제거하거나 각 행의 시작 부분에 `#`을 추가하여 이 섹션을 주석 처리하십시오.
   </tr>
   <tr>
   <td><code>endurance.tier</code></td>
   <td>`endurance` 스토리지를 프로비저닝하려면 GB당 IOPS 수를 입력하십시오. 예를 들어, `ibmc-block-bronze` 스토리지 클래스에 정의된 대로 블록 스토리지를 프로비저닝하려면 2를 입력하십시오. 자세한 정보는 [블록 스토리지 구성 결정](/docs/containers?topic=containers-block_storage#block_predefined_storageclass)을 참조하십시오. `성능` 스토리지를 프로비저닝하려면 이 섹션을 제거하거나 각 행의 시작 부분에 `#`을 추가하여 이 섹션을 주석 처리하십시오. </td>
   </tr>
   <tr>
   <td><code>size</code></td>
   <td>스토리지이 크기(GB)를 입력하십시오. 스토리지 티어에 대해 지원되는 크기를 찾으려면 [블록 스토리지 구성 결정](/docs/containers?topic=containers-block_storage#block_predefined_storageclass)을 참조하십시오. </td>
   </tr>
   </tbody>
   </table>  

5. IBM Cloud 인프라(SoftLayer) 사용자 이름 및 API 키를 검색하십시오. 사용자 이름과 API 키는 클러스터에 액세스하기 위해 `mkpvyaml` 스크립트에서 사용합니다.
   1. [{{site.data.keyword.Bluemix_notm}} 콘솔![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/)에 로그인하십시오.
   2. 메뉴 ![메뉴 아이콘](../icons/icon_hamburger.svg "메뉴 아이콘")에서 **인프라**를 선택하십시오.
   3. 메뉴 표시줄에서 **계정** > **사용자** > **사용자 목록**을 선택하십시오.
   4. 검색할 사용자 이름 및 API 키를 찾으십시오.
   5. **생성**을 클릭하여 API 키를 생성하거나 **보기**를 클릭하여 기존 API 키를 보십시오. 인프라 사용자 이름 및 API 키를 보여주는 팝업 창이 열립니다.

6. 인증 정보를 환경 변수에 저장하십시오.
   1. 환경 변수를 추가하십시오.
      ```
      export SL_USERNAME=<infrastructure_username>
      ```
      {: pre}

      ```
      export SL_API_KEY=<infrastructure_api_key>
      ```
      {: pre}

   2. 환경 변수가 작성되었는지 확인하십시오.
      ```
        printenv
      ```
      {: pre}

7.  `mkpvyaml` 컨테이너를 빌드하고 실행하십시오. 이미지에서 컨테이너를 실행하면 `mkpvyaml.py` 스크립트가 실행됩니다. 이 스크립트는 블록 스토리지 디바이스를 클러스터의 모든 작업자 노드에 추가하고 각 작업자 노드가 블록 스토리지 디바이스에 액세스할 수 있는 권한을 부여합니다. 스크립트의 끝에 클러스터에서 지속적 볼륨을 작성하기 위해 나중에 사용할 수 있는 `pv-<cluster_name>.yaml` YAML 파일이 생성됩니다.
    1.  `mkpvyaml` 컨테이너를 빌드하십시오.
        ```
        docker build -t mkpvyaml .
        ```
        {: pre}
        출력 예:
        ```
        Sending build context to Docker daemon   29.7kB
        Step 1/16 : FROM ubuntu:18.10
        18.10: Pulling from library/ubuntu
        5940862bcfcd: Pull complete
        a496d03c4a24: Pull complete
        5d5e0ccd5d0c: Pull complete
        ba24b170ddf1: Pull complete
        Digest: sha256:20b5d52b03712e2ba8819eb53be07612c67bb87560f121cc195af27208da10e0
        Status: Downloaded newer image for ubuntu:18.10
         ---> 0bfd76efee03
        Step 2/16 : RUN apt-get update
         ---> Running in 85cedae315ce
        Get:1 http://security.ubuntu.com/ubuntu cosmic-security InRelease [83.2 kB]
        Get:2 http://archive.ubuntu.com/ubuntu cosmic InRelease [242 kB]
        ...
        Step 16/16 : ENTRYPOINT [ "/docker-entrypoint.sh" ]
         ---> Running in 9a6842f3dbe3
        Removing intermediate container 9a6842f3dbe3
         ---> 7926f5384fc7
        Successfully built 7926f5384fc7
        Successfully tagged mkpvyaml:latest
        ```
        {: screen}
    2.  컨테이너를 실행하여 `mkpvyaml.py` 스크립트를 실행하십시오.
        ```
        docker run --rm -v `pwd`:/data -v ~/.bluemix:/config -e SL_API_KEY=$SL_API_KEY -e SL_USERNAME=$SL_USERNAME mkpvyaml
        ```
        {: pre}

        출력 예:
        ```
        Unable to find image 'portworx/iks-mkpvyaml:latest' locally
        latest: Pulling from portworx/iks-mkpvyaml
        72f45ff89b78: Already exists
        9f034a33b165: Already exists
        386fee7ab4d3: Already exists
        f941b4ac6aa8: Already exists
        fe93e194fcda: Pull complete
        f29a13da1c0a: Pull complete
        41d6e46c1515: Pull complete
        e89af7a21257: Pull complete
        b8a7a212d72e: Pull complete
        5e07391a6f39: Pull complete
        51539879626c: Pull complete
        cdbc4e813dcb: Pull complete
        6cc28f4142cf: Pull complete
        45bbaad87b7c: Pull complete
        05b0c8595749: Pull complete
        Digest: sha256:43ac58a8e951994e65f89ed997c173ede1fa26fb41e5e764edfd3fc12daf0120
        Status: Downloaded newer image for portworx/iks-mkpvyaml:latest

        kube-dal10-abc1d23e123456ac8b12a3c1e12b123a4 10.XXX.XX.XX
        Creating Vol of size 20 with type:  endurance
                  Ordering block storage of size: 20 for host: kube-dal10-abc1d23e123456ac8b12a3c1e12b123a4
                  ORDER ID =  30087291
        kube-dal10-abc1d23e123456ac8b12a3c1e12b123a4 10.XXX.XX.XX
        Creating Vol of size 20 with type:  endurance
                  Ordering block storage of size: 20 for host: kube-dal10-abc1d23e123456ac8b12a3c1e12b123a4
                  ORDER ID =  30087293
        kube-dal12-abc1d23e123456ac8b12a3c1e12b456v1 10.XXX.XX.XX
        Creating Vol of size 20 with type:  endurance
                  Ordering block storage of size: 20 for host: kube-dal12-abc1d23e123456ac8b12a3c1e12b456v1
                  ORDER ID =  30085655
                  No volume yet ... for orderID :  30087291
                  No volume yet ... for orderID :  30087291
                  No volume yet ... for orderID :  30087291
                  No volume yet ... for orderID :  30087291
                  Order ID =  30087291 has created VolId =  12345678
                  Granting access to volume: 12345678 for HostIP: 10.XXX.XX.XX
                  Order ID =  30087293 has created VolId =  87654321
                  Granting access to volume: 87654321 for HostIP: 10.XXX.XX.XX
                  Vol 53002831 is not yet ready ...
                  Granting access to volume: 87654321 for HostIP: 10.XXX.XX.XX
                  Order ID =  30085655 has created VolId =  98712345
                  Granting access to volume: 98712345 for HostIP: 10.XXX.XX.XX
        Output file created as :  pv-<cluster_name>.yaml
        ```
        {: screen}

7. [작업자 노드에 블록 스토리지 디바이스를 연결](#attach_block)하십시오.

## 특정 작업자 노드에 블록 스토리지 수동 추가
{: #manual_block}

다른 블록 스토리지 구성을 추가하고 블록 스토리지를 작업자 노드의 서브세트에만 추가하거나 프로비저닝 프로세스를 보다 더 제어할 수 있도록 하려면 이 옵션을 사용합니다.
{: shortdesc}

1. 클러스터의 작업자 노드를 나열하고 블록 스토리지 디바이스를 추가할 비 SDS 작업자 노드의 지역 및 사설 IP 주소를 기록해 두십시오.
   ```
   ibmcloud ks workers --cluster <cluster_name_or_ID>
   ```
   {: pre}

2. 비 SDS 작업자 노드에 추가하려는 블록 스토리지 디바이스의 유형, 크기 및 IOPS의 수를 선택하려면 [블록 스토리지 구성 결정](/docs/containers?topic=containers-block_storage#block_predefined_storageclass)의 3단계와 4단계를 검토하십시오.    

3. 비 SDS 작업자 노드가 있는 동일한 구역에 블록 스토리지 디바이스를 작성하십시오.

   **GB당 2개의 IOPS로 20GB Endurance 블록 스토리지를 프로비저닝하기 위한 예제:**
   ```
   ibmcloud sl block volume-order --storage-type endurance --size 20 --tier 2 --os-type LINUX --datacenter dal10
   ```
   {: pre}

   **100개의 IOPS로 20GB 성능 블록 스토리지를 프로비저닝하기 위한 예제:**
   ```
   ibmcloud sl block volume-order --storage-type performance --size 20 --iops 100 --os-type LINUX --datacenter dal10
   ```
   {: pre}

4. 블록 스토리지 디바이스가 작성되었는지 확인하고 볼륨의 **`id`**를 기록해 두십시오. **참고:** 블록 스토리지 디바이스가 즉시 표시되지 않으면 몇 분 정도 기다리십시오. 그런 다음, 이 명령을 다시 실행하십시오.
   ```
   ibmcloud sl block volume-list
   ```
   {: pre}

   출력 예:
   ```
   id         username          datacenter   storage_type                capacity_gb   bytes_used   ip_addr         lunId   active_transactions
   123456789  IBM02SL1234567-8  dal10        performance_block_storage   20            -            161.12.34.123   0       0   
   ```
   {: screen}

5. 볼륨에 대한 세부사항을 검토하고 **`Target IP`** 및 **`LUN Id`**를 기록해 두십시오.
   ```
   ibmcloud sl block volume-detail <volume_ID>
   ```
   {: pre}

   출력 예:
   ```
   Name                       Value
   ID                         1234567890
   User name                  IBM123A4567890-1
   Type                       performance_block_storage
   Capacity (GB)              20
   LUN Id                     0
   IOPs                       100
   Datacenter                 dal10
   Target IP                  161.12.34.123
   # of Active Transactions   0
   Replicant Count            0
   ```
   {: screen}

6. 비 SDS 작업자 노드에 권한 부여하여 블록 스토리지 디바이스에 액세스하십시오. `<volume_ID>`를 이전에 검색한 블록 스토리지 디바이스의 볼륨 ID로 대체하고 `<private_worker_IP>`는 디바이스를 연결할 비 SDS 작업자 노드의 사설 IP 주소로 대체하십시오.

   ```
   ibmcloud sl block access-authorize <volume_ID> -p <private_worker_IP>
   ```
   {: pre}

   출력 예:
   ```
   The IP address 123456789 was authorized to access <volume_ID>.
   ```
   {: screen}

7. 비 SDS 작업자 노드가 정상적으로 권한 부여되었는지 확인하고 **`host_iqn`**, **`username`** 및 **`password`**를 기록해 두십시오.
   ```
   ibmcloud sl block access-list <volume_ID>
   ```
   {: pre}

   출력 예:
   ```
   ID          name                 type   private_ip_address   source_subnet   host_iqn                                      username   password           allowed_host_id
   123456789   <private_worker_IP>  IP     <private_worker_IP>  -               iqn.2018-09.com.ibm:ibm02su1543159-i106288771   IBM02SU1543159-I106288771   R6lqLBj9al6e2lbp   1146581   
   ```
   {: screen}

   **`host_iqn`**, **`username`** 및 **`password`**가 지정되어 있으면 권한 부여에 성공한 것입니다.

8. [작업자 노드에 블록 스토리지 디바이스를 연결](#attach_block)하십시오.


## 비 SDS 작업자 노드에 원시 블록 스토리지 연결
{: #attach_block}

블록 스토리지 디바이스를 비 SDS 작업자 노드에 연결하려면 {{site.data.keyword.Bluemix_notm}} Block Volume Attacher 스토리지 클래스와 블록 스토리지 디바이스의 세부사항을 사용하여 지속적 스토리지(PV)를 작성해야 합니다.
{: shortdesc}

**시작하기 전에**:
- 비SDS 작업자 노드에 원시, 포맷되지 않고, 마운트 해제된 블록 스토리지를 [자동](#automatic_block) 또는 [수동](#manual_block)으로 작성했는지 확인하십시오.
- [계정에 로그인하십시오. 해당되는 경우, 적절한 리소스 그룹을 대상으로 지정하십시오. 클러스터의 컨텍스트를 설정하십시오.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

**비 SDS 작업자 노드에 원시 블록 스토리지를 연결하려면 다음을 수행하십시오**.
1. PV 작성을 준비하십시오.  
   - **`mkpvyaml` 컨테이너를 사용한 경우:**
     1. `pv-<cluster_name>.yaml` 파일을 여십시오.
        ```
        nano pv-<cluster_name>.yaml
        ```
        {: pre}

     2. PV에 대한 구성을 검토하십시오.

   - **블록 스토리지를 수동으로 추가한 경우:**
     1. `pv.yaml` 파일을 작성하십시오. 다음 명령은 `nano` 편집기로 파일을 작성합니다.
        ```
        nano pv.yaml
        ```
        {: pre}

     2. PV에 블록 스토리지 디바이스의 세부사항을 추가하십시오.
        ```
        apiVersion: v1
        kind: PersistentVolume
        metadata:
          name: <pv_name>
          annotations:
            ibm.io/iqn: "<IQN_hostname>"
            ibm.io/username: "<username>"
            ibm.io/password: "<password>"
            ibm.io/targetip: "<targetIP>"
            ibm.io/lunid: "<lunID>"
            ibm.io/nodeip: "<private_worker_IP>"
            ibm.io/volID: "<volume_ID>"
        spec:
          capacity:
            storage: <size>
          accessModes:
            - ReadWriteOnce
          hostPath:
              path: /
          storageClassName: ibmc-block-attacher
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
      	<td>PV의 이름을 입력하십시오.</td>
      	</tr>
        <tr>
        <td><code>ibm.io/iqn</code></td>
        <td>이전에 검색한 IQN 호스트 이름을 입력하십시오. </td>
        </tr>
        <tr>
        <td><code>ibm.io/username</code></td>
        <td>이전에 검색한 IBM Cloud 인프라(SoftLayer) 사용자 이름을 입력하십시오. </td>
        </tr>
        <tr>
        <td><code>ibm.io/password</code></td>
        <td>이전에 검색한 IBM Cloud 인프라(SoftLayer) 비밀번호를 입력하십시오. </td>
        </tr>
        <tr>
        <td><code>ibm.io/targetip</code></td>
        <td>이전에 검색한 대상 IP를 입력하십시오. </td>
        </tr>
        <tr>
        <td><code>ibm.io/lunid</code></td>
        <td>이전에 검색한 블록 스토리지 디바이스의 LUN ID를 입력하십시오. </td>
        </tr>
        <tr>
        <td><code>ibm.io/nodeip</code></td>
        <td>블록 스토리지 디바이스를 연결할 작업자 노드의 사설 IP 주소를 입력하고 이전에 블록 스토리지 디바이스에 액세스할 수 있도록 권한 부여한 작업자 노드의 사설 IP 주소를 입력하십시오. </td>
        </tr>
        <tr>
          <td><code>ibm.io/volID</code></td>
        <td>이전에 검색한 블록 스토리지 볼륨의 ID를 입력하십시오. </td>
        </tr>
        <tr>
        <td><code>스토리지</code></td>
        <td>이전에 검색한 블록 스토리지 디바이스의 크기를 입력하십시오. 예를 들어, 블록 스토리지 디바이스가 20GB이면 <code>20Gi</code>를 입력하십시오.  </td>
        </tr>
        </tbody>
        </table>
2. PV를 작성하여 블록 스토리지 디바이스를 비 SDS 작업자 노드에 연결하십시오.
   - **`mkpvyaml` 컨테이너를 사용한 경우:**
     ```
     kubectl apply -f pv-<cluster_name>.yaml
     ```
     {: pre}

   - **블록 스토리지를 수동으로 추가한 경우:**
     ```
     kubectl apply -f  block-volume-attacher/pv.yaml
     ```
     {: pre}

3. 블록 스토리지가 작업자 노드에 연결되었는지 확인하십시오.
   ```
   kubectl describe pv <pv_name>
   ```
   {: pre}

   출력 예:
   ```
   Name:            kube-wdc07-cr398f790bc285496dbeb8e9137bc6409a-w1-pv1
   Labels:          <none>
   Annotations:     ibm.io/attachstatus=attached
                    ibm.io/dm=/dev/dm-1
                    ibm.io/iqn=iqn.2018-09.com.ibm:ibm02su1543159-i106288771
                    ibm.io/lunid=0
                    ibm.io/mpath=3600a09803830445455244c4a38754c66
                    ibm.io/nodeip=10.176.48.67
                    ibm.io/password=R6lqLBj9al6e2lbp
                    ibm.io/targetip=161.26.98.114
                    ibm.io/username=IBM02SU1543159-I106288771
                    kubectl.kubernetes.io/last-applied-configuration={"apiVersion":"v1","kind":"PersistentVolume","metadata":{"annotations":{"ibm.io/iqn":"iqn.2018-09.com.ibm:ibm02su1543159-i106288771","ibm.io/lunid":"0"...
   Finalizers:      []
   StorageClass:    ibmc-block-attacher
   Status:          Available
   Claim:
   Reclaim Policy:  Retain
   Access Modes:    RWO
   Capacity:        20Gi
   Node Affinity:   <none>
   Message:
   Source:
       Type:          HostPath (bare host directory volume)
       Path:          /
       HostPathType:
   Events:            <none>
   ```
   {: screen}

   **ibm.io/dm**이 `/dev/dm/1`와 같은 디바이스 ID로 설정되면 블록 스토리지 디바이스는 정상적으로 연결되며 CLI 출력의 **어노테이션** 섹션에 **ibm.io/attachstatus=attached**가 표시됩니다.

볼륨을 분리하려면 PV를 삭제하십시오. 분리된 볼륨은 여전히 특정 작업자 노드에서 액세스할 수 있도록 권한 부여되어 있으며 동일한 작업자 노드에 다른 볼륨을 연결하도록 {{site.data.keyword.Bluemix_notm}} Block Volume Attacher 스토리지 클래스로 새 PV를 작성하면 다시 연결됩니다. 이전에 분리한 볼륨을 다시 연결하지 않으려면 `ibmcloud sl block access-revoke` 명령을 사용하여 분리된 볼륨에 액세스하도록 작업자 노드에 권한을 부여하지 마십시오. 볼륨을 분리해도 IBM Cloud 인프라(SoftLayer) 계정에서 볼륨이 제거되지 않습니다. 볼륨에 대한 비용 청구를 취소하려면 수동으로 [IBM Cloud 인프라(SoftLayer) 계정에서 스토리지를 제거](/docs/containers?topic=containers-cleanup)해야 합니다.
{: note}


