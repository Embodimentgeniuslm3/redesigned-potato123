---

copyright:
  years: 2014, 2019
lastupdated: "2019-05-31"

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



# IBM File Storage for IBM Cloud에 데이터 저장
{: #file_storage}

{{site.data.keyword.Bluemix_notm}} File Storage는 Kubernetes 지속적 볼륨(PV)을 사용하여 앱에 추가할 수 있는 지속적이고 빠르며 유연한 네트워크 연결 NFS 기반 파일 스토리지입니다. 워크로드의 요구사항을 충족하는 GB 크기와 IOPS를 사용하여 사전정의된 스토리지 계층 중에서 선택할 수 있습니다. {{site.data.keyword.Bluemix_notm}} File Storage가 사용자에게 적합한 올바른 스토리지 옵션인지 확인하려면 [스토리지 솔루션 선택](/docs/containers?topic=containers-storage_planning#choose_storage_solution)을 참조하십시오. 가격 정보는 [비용 청구](/docs/infrastructure/FileStorage?topic=FileStorage-about#billing)를 참조하십시오.
{: shortdesc}

{{site.data.keyword.Bluemix_notm}} File Storage는 공용 네트워크 연결로 설정되는 표준 클러스터에만 사용 가능합니다. 클러스터가 방화벽으로 보호되는 사설 클러스터 또는 개인 서비스 엔드포인트가 사용으로 설정된 클러스터와 같이 공용 네트워크에 액세스할 수 없는 경우, 클러스터가 Kubernetes 버전 1.13.4_1513, 1.12.6_1544, 1.11.8_1550 이상을 실행하면 파일 스토리지를 프로비저닝할 수 있습니다. NFS 파일 스토리지 인스턴스는 단일 구역에 한정됩니다. 다중 구역 클러스터가 있는 경우에는 [다중 구역 지속적 스토리지 옵션](/docs/containers?topic=containers-storage_planning#persistent_storage_overview)을 고려하십시오.
{: important}

## 파일 스토리지 구성에 대한 결정
{: #file_predefined_storageclass}

{{site.data.keyword.containerlong}}는 특정 구성으로 파일 스토리지를 프로비저닝하는 데 사용할 수 있는 파일 스토리지의 사전 정의된 스토리지 클래스를 제공합니다.
{: shortdesc}

모든 스토리지 클래스는 사용 가능한 크기, IOPS, 파일 시스템 및 보유 정책을 포함하여 사용자가 프로비저닝하는 파일 스토리지의 유형을 지정합니다.  

스토리지 클래스를 사용하여 특정 유형의 스토리지를 프로비저닝한 후에는 스토리지 디바이스에 대한 유형 또는 보유 정책을 변경할 수 없습니다. 그러나 스토리지 용량과 성능을 늘리려는 경우에는 [크기 및 IOPS를 변경](#file_change_storage_configuration)할 수 있습니다. 스토리지의 유형 및 보유 정책을 변경하려면 [새 스토리지 인스턴스를 작성하고 이전 스토리지 인스턴스의 데이터를 새 스토리지 인스턴스로 복사](/docs/containers?topic=containers-kube_concepts#update_storageclass)해야 합니다.
{: important}

시작하기 전에: [계정에 로그인하십시오. 해당되는 경우, 적절한 리소스 그룹을 대상으로 지정하십시오. 클러스터의 컨텍스트를 설정하십시오.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

스토리지 구성을 결정하려면 다음을 수행하십시오.

1. {{site.data.keyword.containerlong}}에서 사용 가능한 스토리지 클래스를 나열하십시오.
   ```
    kubectl get storageclasses | grep file
   ```
   {: pre}

   출력 예:
   ```
$ kubectl get storageclasses
    NAME                         TYPE
    ibmc-file-bronze (default)   ibm.io/ibmc-file
    ibmc-file-custom             ibm.io/ibmc-file
    ibmc-file-gold               ibm.io/ibmc-file
    ibmc-file-retain-bronze      ibm.io/ibmc-file
    ibmc-file-retain-custom      ibm.io/ibmc-file
    ibmc-file-retain-gold        ibm.io/ibmc-file
    ibmc-file-retain-silver      ibm.io/ibmc-file
    ibmc-file-silver             ibm.io/ibmc-file
   ```
   {: screen}

2. 스토리지 클래스의 구성을 검토하십시오.
  ```
   kubectl describe storageclass <storageclass_name>
  ```
  {: pre}

   각 스토리지 클래스에 대한 자세한 정보는 [스토리지 클래스 참조](#file_storageclass_reference)를 참조하십시오. 원하는 항목을 찾지 못한 경우에는 사용자 정의된 스토리지 클래스의 작성을 고려하십시오. 시작하려면 [사용자 정의된 스토리지 클래스 샘플](#file_custom_storageclass)을 체크아웃하십시오.
   {: tip}

3. 프로비저닝하고자 하는 파일 스토리지의 유형을 선택하십시오.
   - **브론즈, 실버 및 골드 스토리지 클래스:** 이러한 스토리지 클래스는 [Endurance 스토리지](/docs/infrastructure/FileStorage?topic=FileStorage-about#provisioning-with-endurance-tiers)를 프로비저닝합니다. Endurance 스토리지를 사용하면 사전 정의된 IOPS 티어에서 기가바이트 단위로 스토리지의 크기를 선택할 수 있습니다.
   - **사용자 정의 스토리지 클래스:** 이 스토리지 클래스는 [성능 스토리지](/docs/infrastructure/FileStorage?topic=FileStorage-about#provisioning-with-performance)를 프로비저닝합니다. 성능 스토리지를 사용하면 IOPS 및 스토리지의 크기에 대한 추가적인 제어가 가능합니다.

4. 파일 스토리지의 크기와 IOPS를 선택하십시오. IOPS의 크기와 수는 스토리지의 속도에 대한 지표의 역할을 하는 IOPS(Input/output Operations Per Second) 수의 총계를 정의합니다. 스토리지에 총 IOPS가 많을 수록 읽기/쓰기 오퍼레이션의 처리 속도가 빨라집니다.
   - **브론즈, 실버 및 골드 스토리지 클래스:** 이 스토리지 클래스는 기가바이트당 고정된 수의 IOPS가 제공되며 SSD 하드 디스크에 프로비저닝됩니다. IOPS 수의 총계는 선택하는 스토리지의 크기에 따라 다릅니다. 허용된 크기 범위 내에서 GB 단위의 정수를 선택할 수 있습니다(예: 20Gi, 256Gi 또는 11854Gi). IOPS 수의 총계를 판별하려면 IOPS를 선택된 크기와 곱해야 합니다. 예를 들어, GB당 4 IOPS가 제공되는 실버 스토리지 클래스에서 1000Gi 파일 스토리지 크기를 선택하는 경우에는 스토리지에 총 4000 IOPS가 있습니다.
     <table>
         <caption>스토리지 클래스 크기 범위 및 GB당 IOPS의 표</caption>
         <thead>
         <th>스토리지 클래스</th>
         <th>GB당 IOPS</th>
         <th>크기 범위(GB)</th>
         </thead>
         <tbody>
         <tr>
         <td>브론즈</td>
         <td>2IOPS/GB</td>
         <td>20 - 12000Gi</td>
         </tr>
         <tr>
         <td>실버</td>
         <td>4IOPS/GB</td>
         <td>20 - 12000Gi</td>
         </tr>
         <tr>
         <td>골드</td>
         <td>10 IOPS/GB</td>
         <td>20 - 4000Gi</td>
         </tr>
         </tbody></table>
   - **사용자 정의 스토리지 클래스:** 이 스토리지 클래스를 선택하면 원하는 IOPS와 크기에 대해 추가적인 제어가 가능합니다. 크기의 경우, 허용된 크기 범위 내에서 GB 단위의 정수를 선택할 수 있습니다. 사용자가 선택하는 크기에 따라 사용 가능한 IOPS 범위가 결정됩니다. 지정된 범위 내에 있는 100의 배수인 IOPS를 선택할 수 있습니다. 선택하는 IOPS는 정적이며 스토리지의 크기에 따라 스케일링되지 않습니다. 예를 들어, 100 IOPS와 함께 40Gi를 선택하면 총 IOPS는 100을 유지합니다.</br></br> IOPS 대 기가바이트 비율은 사용자를 위해 프로비저닝되는 하드 디스크의 유형도 결정합니다. 예를 들어, 100 IOPS의 500Gi를 보유 중이면 IOPS 대 기가바이트 비율은 0.2입니다. 비율이 0.3 이하인 스토리지는 SATA 하드 디스크에서 프로비저닝됩니다. 비율이 0.3을 초과하는 스토리지는 SSD 하드 디스크에서 프로비저닝됩니다.  
     <table>
         <caption>스토리지 클래스 크기 범위 및 IOPS의 표</caption>
         <thead>
         <th>크기 범위(GB)</th>
         <th>100의 배수로 된 IOPS 범위</th>
         </thead>
         <tbody>
         <tr>
         <td>20 - 39Gi</td>
         <td>100 - 1000IOPS</td>
         </tr>
         <tr>
         <td>40 - 79Gi</td>
         <td>100 - 2000IOPS</td>
         </tr>
         <tr>
         <td>80 - 99Gi</td>
         <td>100 - 4000IOPS</td>
         </tr>
         <tr>
         <td>100 - 499Gi</td>
         <td>100 - 6000IOPS</td>
         </tr>
         <tr>
         <td>500 - 999Gi</td>
         <td>100 - 10000IOPS</td>
         </tr>
         <tr>
         <td>1000 - 1999Gi</td>
         <td>100 - 20000IOPS</td>
         </tr>
         <tr>
         <td>2000 - 2999Gi</td>
         <td>200 - 40000IOPS</td>
         </tr>
         <tr>
         <td>3000 - 3999Gi</td>
         <td>200 - 48000IOPS</td>
         </tr>
         <tr>
         <td>4000 - 7999Gi</td>
         <td>300 - 48000IOPS</td>
         </tr>
         <tr>
         <td>8000 - 9999Gi</td>
         <td>500 - 48000IOPS</td>
         </tr>
         <tr>
         <td>10000 - 12000Gi</td>
         <td>1000 - 48000IOPS</td>
         </tr>
         </tbody></table>

5. 클러스터 또는 지속적 볼륨 클레임(PVC)이 삭제된 후에 데이터를 보존하고자 하는지를 선택하십시오.
   - 데이터를 보존하려면 `retain` 스토리지 클래스를 선택하십시오. PVC를 삭제하면 PVC만 삭제됩니다. PV, IBM Cloud 인프라(SoftLayer) 계정의 실제 스토리지 디바이스 및 데이터는 여전히 존재합니다. 스토리지를 재확보하고 이를 클러스터에서 다시 사용하려면 PV를 제거하고 [기존 파일 스토리지 사용](#existing_file)의 단계를 따라야 합니다.
   - PVC를 삭제할 때 PV, 데이터 및 실제 파일 스토리지 디바이스가 삭제되도록 하려면 `retain` 없이 스토리지 클래스를 선택하십시오.

6. 시간별 또는 월별로 청구되기를 원하는지 선택하십시오. 자세한 정보는 [가격 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/cloud/file-storage/pricing)을 확인하십시오. 기본적으로, 모든 파일 스토리지 디바이스는 시간별 비용 청구 유형으로 프로비저닝됩니다.
   월별 비용 청구 유형을 선택하는 경우, 지속적 스토리지를 제거하면 짧은 기간만 사용해도 이에 대해 여전히 월별 비용을 지불하게 됩니다.
   {: note}

<br />



## 앱에 파일 스토리지 추가
{: #add_file}

지속적 볼륨 클레임(PVC)을 작성하여 클러스터에 대한 파일 스토리지를 [동적으로 프로비저닝](/docs/containers?topic=containers-kube_concepts#dynamic_provisioning)할 수 있습니다. 동적 프로비저닝은 일치하는 지속적 볼륨(PV)을 자동으로 작성하고 IBM Cloud 인프라(SoftLayer) 계정에서 실제 스토리지 디바이스를 주문합니다.
{:shortdesc}

시작하기 전에:
- 방화벽이 있는 경우에는 PVC를 작성할 수 있도록 클러스터가 있는 구역의 IBM Cloud 인프라(SoftLayer) IP 범위에 대해 [egress 액세스를 허용](/docs/containers?topic=containers-firewall#pvc)하십시오.
- [사전 정의된 스토리지 클래스를 결정](#file_predefined_storageclass)하거나 [사용자 정의된 스토리지 클래스](#file_custom_storageclass)를 작성하십시오.

파일 스토리지를 Stateful 세트에 배치하려 하십니까? 자세한 정보는 [Stateful 세트에서의 파일 스토리지 사용](#file_statefulset)을 참조하십시오.
{: tip}

파일 스토리지를 추가하려면 다음을 수행하십시오.

1.  지속적 볼륨 클레임(PVC)을 정의하는 구성 파일을 작성하고 이 구성을 `.yaml` 파일로 저장하십시오.

    - **브론즈, 실버, 골드 스토리지 클래스의 예**:
       다음 `.yaml` 파일은 `"ibmc-file-silver"` 스토리지 클래스의 이름이 `mypvc`이고 `"monthly"`로 비용이 청구되며 GB 크기가 `24Gi`인 클레임을 작성합니다.

       ```
       apiVersion: v1
       kind: PersistentVolumeClaim
       metadata:
         name: mypvc
         labels:
           billingType: "monthly"
           region: us-south
           zone: dal13
       spec:
         accessModes:
           - ReadWriteMany
         resources:
           requests:
             storage: 24Gi
         storageClassName: ibmc-file-silver
       ```
       {: codeblock}

    -  **사용자 정의 스토리지 클래스 사용의 예**:
       다음 `.yaml` 파일은 스토리지 클래스 `ibmc-file-retain-custom`의 이름이 `mypvc`이고 `"hourly"`로 비용이 청구되며 GB 크기가 `45Gi`이고 IOPS가 `"300"`인 클레임을 작성합니다.

       ```
       apiVersion: v1
       kind: PersistentVolumeClaim
       metadata:
         name: mypvc
         labels:
           billingType: "hourly"
           region: us-south
           zone: dal13
       spec:
         accessModes:
           - ReadWriteMany
         resources:
           requests:
             storage: 45Gi
             iops: "300"
         storageClassName: ibmc-file-retain-custom
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
         <td><code>metadata.labels.billingType</code></td>
          <td>스토리지 요금이 계산되는 빈도를 "월별" 또는 "시간별"로 지정하십시오. 비용 청구 유형을 지정하지 않으면 스토리지가 시간별 비용 청구 유형으로 프로비저닝됩니다. </td>
       </tr>
       <tr>
       <td><code>metadata.labels.region</code></td>
       <td>선택사항: 파일 스토리지를 프로비저닝할 지역을 지정하십시오. 스토리지에 연결하려면 클러스터가 있는 동일한 지역에서 스토리지를 작성하십시오. 지역을 지정하는 경우에는 구역도 지정해야 합니다. 지역을 지정하지 않거나 지정된 지역을 찾을 수 없는 경우, 스토리지는 클러스터와 동일한 지역에 작성됩니다.
       </br></br>클러스터의 지역을 가져오려면 `ibmcloud ks cluster-get --cluster <cluster_name_or_ID>`를 실행하여 **마스터 URL**에서 지역 접두부를 찾으십시오(예: `https://c2.eu-de.containers.cloud.ibm.com:11111`에서 `eu-de`).
       </br></br><strong>팁:</strong> PVC의 지역과 구역을 지정하는 대신, [사용자 정의된 스토리지 클래스](#file_multizone_yaml)에서 이러한 값을 지정할 수도 있습니다. 그리고 PVC의 <code>metadata.annotations.volume.beta.kubernetes.io/storage-class</code> 섹션의 스토리지 클래스를 사용하십시오. 지역과 구역이 스토리지 클래스 및 PVC에서 지정된 경우에는 PVC의 값이 우선합니다. </td>
       </tr>
       <tr>
       <td><code>metadata.labels.zone</code></td>
       <td>선택사항: 파일 스토리지를 프로비저닝할 구역을 지정하십시오. 앱에서 스토리지를 사용하려면 작업자 노드가 있는 동일한 구역에서 스토리지를 작성하십시오. 작업자 노드의 구역을 보려면 <code>ibmcloud ks workers --cluster &lt;cluster_name_or_ID&gt;</code>를 실행하고 CLI 출력의 <strong>구역</strong> 열을 검토하십시오. 구역을 지정하는 경우에는 지역도 지정해야 합니다. 구역을 지정하지 않거나 지정된 구역을 다중 구역 클러스터에서 찾을 수 없으면 구역이 라운드 로빈 기반으로 선택됩니다. </br></br><strong>팁:</strong> PVC의 지역과 구역을 지정하는 대신, [사용자 정의된 스토리지 클래스](#file_multizone_yaml)에서 이러한 값을 지정할 수도 있습니다. 그리고 PVC의 <code>metadata.annotations.volume.beta.kubernetes.io/storage-class</code> 섹션의 스토리지 클래스를 사용하십시오. 지역과 구역이 스토리지 클래스 및 PVC에서 지정된 경우에는 PVC의 값이 우선합니다.
</td>
       </tr>
       <tr>
       <td><code>spec.accessMode</code></td>
       <td>다음 옵션 중 하나를 지정하십시오. <ul><li><strong>ReadWriteMany:</strong> PVC가 여러 팟(Pod)에 의해 마운트될 수 있습니다. 모든 팟(Pod)은 볼륨에서 읽고 쓸 수 있습니다. </li><li><strong>ReadOnlyMany:</strong> PVC가 여러 팟(Pod)에 의해 마운트될 수 있습니다. 모든 팟(Pod)에는 읽기 전용 액세스 권한이 있습니다. <li><strong>ReadWriteOnce:</strong> PVC가 하나의 팟(Pod)에 의해서만 마운트될 수 있습니다. 이 팟(Pod)은 볼륨에서 읽고 쓸 수 있습니다. </li></ul></td>
       </tr>
       <tr>
       <td><code>spec.resources.requests.storage</code></td>
       <td>파일 스토리지의 크기를 GB(Gi)로 입력하십시오. 스토리지가 프로비저닝된 후에는 파일 스토리지의 크기를 변경할 수 없습니다. 저장할 데이터의 크기와 일치하도록 크기를 지정해야 합니다.</td>
       </tr>
       <tr>
       <td><code>spec.resources.requests.iops</code></td>
       <td>이 옵션은 사용자 정의 스토리지 클래스(`ibmc-file-custom / ibmc-file-retain-custom`)에만 사용 가능합니다. 허용 가능한 범위 내에서 100의 배수를 선택하여 스토리지에 대한 총 IOPS를 지정하십시오. 나열된 것과 이외의 IOPS를 선택하면 IOPS가 올림됩니다.</td>
       </tr>
       <tr>
       <td><code>spec.storageClassName</code></td>
       <td>파일 스토리지를 프로비저닝하는 데 사용할 스토리지 클래스의 이름입니다. [IBM 제공 스토리지 클래스](#file_storageclass_reference) 중 하나를 사용하거나 [사용자 고유의 스토리지 클래스를 작성](#file_custom_storageclass)할 수 있습니다. </br> 스토리지 클래스를 지정하지 않으면 기본 스토리지 클래스 <code>ibmc-file-bronze</code>로 PV가 작성됩니다. </br></br><strong>팁:</strong> 기본 스토리지 클래스를 변경하려면 <code>kubectl patch storageclass &lt;storageclass&gt; -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'</code>를 실행하고 <code>&lt;storageclass&gt;</code>를 스토리지 클래스의 이름으로 대체하십시오.</td>
       </tr>
       </tbody></table>

    사용자 정의된 스토리지 클래스를 사용하려면 해당 스토리지 클래스 이름, 올바른 IOPS 및 크기를 사용하여 PVC를 작성하십시오.   
    {: tip}

2.  PVC를 작성하십시오.

    ```
    kubectl apply -f mypvc.yaml
    ```
    {: pre}

3.  PVC가 작성되고 PV에 바인딩되는지 확인하십시오. 이 프로세스에는 몇 분 정도 소요될 수 있습니다.

    ```
    kubectl describe pvc mypvc
    ```
    {: pre}

    출력 예:

    ```
    Name:		mypvc
    Namespace:	default
    StorageClass:	""
    Status:		Bound
    Volume:		pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2
    Labels:		<none>
    Capacity:	20Gi
    Access Modes:	RWX
    Events:
      FirstSeen	LastSeen	Count	From								SubObjectPath	Type		Reason			Message
      ---------	--------	-----	----								-------------	--------	------			-------
      3m		3m		1	{ibm.io/ibmc-file 31898035-3011-11e7-a6a4-7a08779efd33 }			Normal		Provisioning		External provisioner is provisioning volume for claim "default/my-persistent-volume-claim"
      3m		1m		10	{persistentvolume-controller }							Normal		ExternalProvisioning	cannot find provisioner "ibm.io/ibmc-file", expecting that a volume for the claim is provisioned either manually or via external software
      1m		1m		1	{ibm.io/ibmc-file 31898035-3011-11e7-a6a4-7a08779efd33 }			Normal		ProvisioningSucceeded	Successfully provisioned volume pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2

    ```
    {: screen}

4.  {: #file_app_volume_mount}스토리지를 배치에 마운트하려면 구성 `.yaml` 파일을 작성하고 PV를 바인드하는 PVC를 지정하십시오.

    루트 이외의 사용자가 지속적 스토리지에 쓰도록 요구하는 앱이나 루트 사용자가 마운트 경로를 소유하도록 요구하는 앱이 있는 경우에는 [NFS 파일 스토리지에 비루트 사용자 액세스 추가](/docs/containers?topic=containers-cs_troubleshoot_storage#nonroot) 또는 [NFS 파일 스토리지에 대한 루트 권한 사용](/docs/containers?topic=containers-cs_troubleshoot_storage#nonroot)을 참조하십시오.
    {: tip}

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
    <td><code>spec.containers.volumeMounts.mountPath</code></td>
    <td>컨테이너 내에서 볼륨이 마운트되는 디렉토리의 절대 경로입니다. 마운트 경로에 쓰여진 데이터는 실제 파일 스토리지 인스턴스의 <code>root</code> 디렉토리 아래에 저장됩니다. 다른 앱 간에 볼륨을 공유하려는 경우 각각의 앱에 대해 [볼륨 하위 경로![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/storage/volumes/#using-subpath)를 지정할 수 있습니다.  </td>
    </tr>
    <tr>
    <td><code>spec.containers.volumeMounts.name</code></td>
    <td>팟(Pod)에 마운트할 볼륨의 이름입니다.</td>
    </tr>
    <tr>
    <td><code>volumes.name</code></td>
    <td>팟(Pod)에 마운트할 볼륨의 이름입니다. 일반적으로 이 이름은 <code>volumeMounts.name</code>과 동일합니다.</td>
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

<br />


## 클러스터의 기존 파일 스토리지 사용
{: #existing_file}

클러스터에서 사용할 기존의 실제 스토리지 디바이스가 있는 경우, 스토리지를 [정적으로 프로비저닝](/docs/containers?topic=containers-kube_concepts#static_provisioning)하기 위해 PV 및 PVC를 수동으로 작성할 수 있습니다.
{: shortdesc}

시작하기 전에:
- 기존 파일 스토리지 인스턴스와 동일한 구역에 존재하는 최소한 하나의 작업자 노드를 보유 중인지 확인하십시오.
- [계정에 로그인하십시오. 해당되는 경우, 적절한 리소스 그룹을 대상으로 지정하십시오. 클러스터의 컨텍스트를 설정하십시오.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

### 1단계: 기존 스토리지 준비.
{: #existing-file-1}

기존 스토리지를 앱에 마운트하려면 우선 PV에 대한 모든 필수 정보를 검색하고 클러스터에서 액세스 가능하도록 스토리지를 준비해야 합니다.  
{: shortdesc}

**`retain` 스토리지 클래스로 프로비저닝된 스토리지의 경우:** </br>
`retain` 스토리지 클래스로 스토리지를 프로비저닝하고 PVC를 제거하는 경우에는 PV 및 실제 스토리지 디바이스가 자동으로 제거되지 않습니다. 클러스터의 스토리지를 재사용하려면 우선 나머지 PV를 제거해야 합니다.

프로비저닝된 클러스터와는 다른 클러스터에 있는 기존 스토리지를 사용하려면 [클러스터 외부에서 작성된 스토리지](#external_storage)의 단계에 따라 스토리지를 작업자 노드의 서브넷에 추가하십시오.
{: tip}

1. 기존 PV를 나열하십시오.
   ```
   kubectl get pv
   ```
   {: pre}

   지속적 스토리지에 속하는 PV를 찾으십시오. PV는 `released` 상태입니다.

2. PV의 세부사항을 가져오십시오.
   ```
   kubectl describe pv <pv_name>
   ```
   {: pre}

3. `CapacityGb`, `storageClass`, `failure-domain.beta.kubernetes.io/region`, `failure-domain.beta.kubernetes.io/zone`, `server` 및 `path`를 기록해 두십시오.

4. PV를 제거하십시오.
   ```
   kubectl delete pv <pv_name>
   ```
   {: pre}

5. PV가 제거되었는지 확인하십시오.
   ```
   kubectl get pv
   ```
   {: pre}

</br>

**클러스터 외부에서 프로비저닝된 지속적 스토리지의 경우:** </br>
이전에 프로비저닝했지만 이전에 클러스터에서 사용된 적이 없는 기존 스토리지를 사용하려면, 작업자 노드와 동일한 서브넷에서 해당 스토리지가 사용 가능하도록 해야 합니다.

1.  {: #external_storage}[IBM Cloud 인프라(SoftLayer) 포털 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/classic?)에서 **스토리지**를 클릭하십시오.
2.  **조치** 메뉴에서 **File Storage**를 클릭하고 **호스트에 권한 부여**를 선택하십시오.
3.  **서브넷**을 선택하십시오.
4.  드롭 다운 목록에서 작업자 노드가 연결된 사설 VLAN 서브넷을 선택하십시오. 작업자 노드의 서브넷을 찾으려면 `ibmcloud ks workers --cluster <cluster_name>`을 실행하고 작업자 노드의 `Private IP`를 드롭 다운 목록에서 찾은 서브넷과 비교하십시오.
5.  **제출**을 클릭하십시오.
6.  파일 스토리지의 이름을 클릭하십시오.
7.  `Mount Point`, `size` 및 `Location` 필드를 기록해 두십시오. `Mount Point` 필드는 `<nfs_server>:<file_storage_path>`로 표시됩니다.

### 2단계: 지속적 볼륨(PV) 및 일치하는 지속적 볼륨 클레임(PVC) 작성
{: #existing-file-2}

1.  PV에 대한 스토리지 구성 파일을 작성하십시오. 이전에 검색한 값을 포함하십시오.

    ```
    apiVersion: v1
    kind: PersistentVolume
    metadata:
     name: mypv
     labels:
        failure-domain.beta.kubernetes.io/region: <region>
        failure-domain.beta.kubernetes.io/zone: <zone>
    spec:
     capacity:
       storage: "<size>"
     accessModes:
       - ReadWriteMany
     nfs:
       server: "<nfs_server>"
       path: "<file_storage_path>"
    ```
    {: codeblock}

    <table>
    <caption>YAML 파일 컴포넌트 이해</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> YAML 파일 컴포넌트 이해</th>
    </thead>
    <tbody>
    <tr>
    <td><code>name</code></td>
    <td>작성할 PV 오브젝트의 이름을 입력하십시오.</td>
    </tr>
    <tr>
    <td><code>metadata.labels</code></td>
    <td>이전에 검색한 지역 및 구역을 입력하십시오. 클러스터에서 스토리지를 마운트하려면 지속적 스토리지와 동일한 지역 및 구역에 작업자 노드가 최소한 하나 이상 있어야 합니다. 스토리지에 대한 PV가 이미 존재하는 경우에는 PV에 [구역 및 지역 레이블을 추가](/docs/containers?topic=containers-kube_concepts#storage_multizone)하십시오.
    </tr>
    <tr>
    <td><code>spec.capacity.storage</code></td>
    <td>이전에 검색한 기존 NFS 파일 공유의 스토리지 크기를 입력하십시오. 스토리지 크기는 기가바이트(예: 20Gi(20GB) 또는 1000Gi(1TB))로 기록되어야 하며 그 크기는 기존 파일 공유의 크기와 일치해야 합니다.</td>
    </tr>
    <tr>
    <td><code>spec.accessMode</code></td>
    <td>다음 옵션 중 하나를 지정하십시오. <ul><li><strong>ReadWriteMany:</strong> PVC가 여러 팟(Pod)에 의해 마운트될 수 있습니다. 모든 팟(Pod)은 볼륨에서 읽고 쓸 수 있습니다. </li><li><strong>ReadOnlyMany:</strong> PVC가 여러 팟(Pod)에 의해 마운트될 수 있습니다. 모든 팟(Pod)에는 읽기 전용 액세스 권한이 있습니다. <li><strong>ReadWriteOnce:</strong> PVC가 하나의 팟(Pod)에 의해서만 마운트될 수 있습니다. 이 팟(Pod)은 볼륨에서 읽고 쓸 수 있습니다. </li></ul></td>
    </tr>
    <tr>
    <td><code>spec.nfs.server</code></td>
    <td>이전에 검색한 NFS 파일 공유 서버 ID를 입력하십시오.</td>
    </tr>
    <tr>
    <td><code>path</code></td>
    <td>이전에 검색한 기존 NFS 파일 공유에 대한 경로를 입력하십시오.</td>
    </tr>
    </tbody></table>

3.  클러스터에 PV를 작성하십시오.

    ```
    kubectl apply -f mypv.yaml
    ```
    {: pre}

4.  PV가 작성되었는지 확인하십시오.

    ```
    kubectl get pv
    ```
    {: pre}

5.  다른 구성 파일을 작성하여 PVC를 작성하십시오. PVC가 이전에 작성한 PV와 일치하려면 `storage` 및 `accessMode`에 대해 동일한 값을 선택해야 합니다. `storage-class` 필드는 비어 있어야 합니다. 이러한 필드가 PV와 일치하지 않으면 새 PV 및 새 실제 스토리지 인스턴스가 [동적으로 프로비저닝](/docs/containers?topic=containers-kube_concepts#dynamic_provisioning)됩니다.

    ```
    kind: PersistentVolumeClaim
    apiVersion: v1
    metadata:
     name: mypvc
    spec:
     accessModes:
       - ReadWriteMany
     resources:
       requests:
         storage: "<size>"
     storageClassName:
    ```
    {: codeblock}

6.  PVC 사용자를 작성하십시오.

    ```
    kubectl apply -f mypvc.yaml
    ```
    {: pre}

7.  PVC가 작성되고 PV에 바인딩되는지 확인하십시오. 이 프로세스에는 몇 분 정도 소요될 수 있습니다.

    ```
    kubectl describe pvc mypvc
    ```
    {: pre}

    출력 예:

    ```
    Name: mypvc
    Namespace: default
    StorageClass:	""
    Status: Bound
    Volume: pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2
    Labels: <none>
    Capacity: 20Gi
    Access Modes: RWX
    Events:
      FirstSeen LastSeen Count From        SubObjectPath Type Reason Message
      --------- -------- ----- ----        ------------- -------- ------ -------
      3m 3m 1 {ibm.io/ibmc-file 31898035-3011-11e7-a6a4-7a08779efd33 } Normal Provisioning External provisioner is provisioning volume for claim "default/my-persistent-volume-claim"
      3m 1m	 10 {persistentvolume-controller } Normal ExternalProvisioning cannot find provisioner "ibm.io/ibmc-file", expecting that a volume for the claim is provisioned either manually or via external software
      1m 1m 1 {ibm.io/ibmc-file 31898035-3011-11e7-a6a4-7a08779efd33 } Normal ProvisioningSucceeded	Successfully provisioned volume pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2
    ```
    {: screen}


PV를 작성하여 PVC에 바인딩했습니다. 이제 클러스터 사용자는 자신의 배치에 [PVC를 마운트](#file_app_volume_mount)하고 PV 오브젝트에서 읽거나 쓰기를 시작할 수 있습니다.

<br />



## Stateful 세트에서의 파일 스토리지 사용
{: #file_statefulset}

데이터베이스와 같은 Stateful 앱이 있는 경우에는 앱의 데이터를 저장하기 위해 파일 스토리지를 사용하는 Stateful 세트를 작성할 수 있습니다. 또는, {{site.data.keyword.Bluemix_notm}} DBaaS(Database-as-a-Service)를 사용하여 데이터를 클라우드에 저장할 수 있습니다.
{: shortdesc}

**Stateful 세트에 파일 스토리지를 추가할 때는 어떤 사항을 알아야 합니까?** </br>
Stateful 세트에 스토리지를 추가하려는 경우에는 Stateful 세트 YAML의 `volumeClaimTemplates` 섹션에 스토리지 구성을 지정합니다. `volumeClaimTemplates`는 PVC의 기반이며 프로비저닝할 파일 스토리지의 스토리지 클래스와 크기 또는 IOPS를 포함할 수 있습니다. 그러나 `volumeClaimTemplates`에 레이블을 포함하려는 경우 Kubernetes는 PVC를 작성할 때 이러한 레이블을 포함하지 않습니다. 대신 사용자가 직접 Stateful 세트에 해당 레이블을 추가해야 합니다.

동시에 두 Stateful 세트를 배치할 수는 없습니다. 한 Stateful 세트가 완전히 배치되기 전에 다른 세트를 작성하려 시도하면 Stateful 세트 배치 작업에서 예기치 않은 결과가 발생할 수 있습니다.
{: important}

**특정 구역에서 Stateful 세트를 작성하는 방법은 무엇입니까?** </br>
다중 구역 클러스터에서는 Stateful 세트 YAML의 `spec.selector.matchLabels` 및 `spec.template.metadata.labels` 섹션에서 Stateful 세트를 작성할 구역 및 지역을 지정해야 합니다. 또는, 이러한 레이블을 [사용자 정의된 스토리지 클래스](/docs/containers?topic=containers-kube_concepts#customized_storageclass)에 추가하고 이 스토리지 클래스를 Stateful 세트의 `volumeClaimTemplates` 섹션에서 사용할 수 있습니다.

**팟(Pod)이 준비될 때까지 내 Stateful 팟(Pod)에 PV의 바인딩을 지연시킬 수 있습니까?**<br>
예, [`volumeBindingMode: WaitForFirstConsumer` ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/storage/storage-classes/#volume-binding-mode) 필드가 포함된 PVC에 대한 [사용자 정의 스토리지 클래스를 작성](#file-topology)할 수 있습니다.

**Stateful 세트에 파일 스토리지를 추가할 때는 어떤 선택사항이 있습니까?** </br>
Stateful 세트를 작성할 때 자동으로 PVC를 작성하려는 경우에는 [동적 프로비저닝](#file_dynamic_statefulset)을 사용하십시오. Stateful 세트에 대해 [PVC를 사전 프로비저닝하거나 기존 PVC를 사용](#file_static_statefulset)하도록 선택할 수도 있습니다.  

### 동적 프로비저닝: Stateful 세트를 작성할 때 PVC 작성
{: #file_dynamic_statefulset}

Stateful 세트를 작성할 때 PVC를 자동으로 작성하려면 이 선택사항을 사용하십시오.
{: shortdesc}

시작하기 전에: [계정에 로그인하십시오. 해당되는 경우, 적절한 리소스 그룹을 대상으로 지정하십시오. 클러스터의 컨텍스트를 설정하십시오.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1. 클러스터 내의 모든 기존 Stateful 세트가 완전히 배치되었는지 확인하십시오. 특정 Stateful 세트가 여전히 배치 중인 경우에는 Stateful 세트 작성을 시작할 수 없습니다. 예기치 않은 결과를 방지하려면 클러스터 내의 모든 Stateful 세트가 완전히 배치될 때까지 기다려야 합니다.
   1. 클러스터에 있는 기존 Stateful 세트를 나열하십시오.
      ```
      kubectl get statefulset --all-namespaces
      ```
      {: pre}

      출력 예:
      ```
      NAME              DESIRED   CURRENT   AGE
      mystatefulset     3         3         6s
      ```
      {: screen}

   2. 각 Stateful 세트의 **Pods Status**를 보고 해당 Stateful 세트의 배치가 완료되었는지 확인하십시오.  
      ```
      kubectl describe statefulset <statefulset_name>
      ```
      {: pre}

      출력 예:
      ```
      Name:               nginx
      Namespace:          default
      CreationTimestamp:  Fri, 05 Oct 2018 13:22:41 -0400
      Selector:           app=nginx,billingType=hourly,region=us-south,zone=dal10
      Labels:             app=nginx
                          billingType=hourly
                          region=us-south
                          zone=dal10
      Annotations:        kubectl.kubernetes.io/last-applied-configuration={"apiVersion":"apps/v1","kind":"StatefulSet","metadata":{"annotations":{},"name":"nginx","namespace":"default"},"spec":{"podManagementPolicy":"Par...
      Replicas:           3 desired | 3 total
      Pods Status:        0 Running / 3 Waiting / 0 Succeeded / 0 Failed
      Pod Template:
        Labels:  app=nginx
                 billingType=hourly
                 region=us-south
                 zone=dal10
      ...
      ```
      {: screen}

      CLI 출력의 **Replicas** 섹션이 **Pods Status** 섹션의 **Running** 팟(Pod) 수와 동일하면 Stateful 세트가 완전히 배치된 것입니다. Stateful 세트가 아직 완전히 배치되지 않은 경우에는 진행하기 전에 배치가 완료되기를 기다리십시오.

2. Stateful 세트에 대한 구성 파일과 이 Stateful 세트를 노출하는 데 사용하는 서비스를 작성하십시오.

  - **구역을 지정하는 Stateful 세트 예제**

    다음 예는 NGINX를 3개의 복제본을 포함하는 Stateful 세트로 배치하는 방법을 보여줍니다. 각 복제본에 대해, `ibmc-file-retain-bronze` 스토리지 클래스의 스펙에 따라 20기가바이트의 파일 스토리지 디바이스가 프로비저닝됩니다. 모든 스토리지는 `dal10` 구역에서 프로비저닝됩니다. 파일 스토리지는 다른 구역에서 액세스할 수 없으므로, Stateful 세트의 모든 복제본 또한 `dal10`에 있는 작업자 노드에 배치됩니다.

    ```
    apiVersion: v1
    kind: Service
    metadata:
     name: nginx
     labels:
       app: nginx
    spec:
     ports:
     - port: 80
       name: web
     clusterIP: None
     selector:
       app: nginx
    ---
    apiVersion: apps/v1
    kind: StatefulSet
    metadata:
     name: nginx
    spec:
     serviceName: "nginx"
     replicas: 3
     podManagementPolicy: Parallel
     selector:
       matchLabels:
         app: nginx
         billingType: "hourly"
         region: "us-south"
         zone: "dal10"
     template:
       metadata:
         labels:
           app: nginx
           billingType: "hourly"
           region: "us-south"
           zone: "dal10"
       spec:
         containers:
         - name: nginx
           image: k8s.gcr.io/nginx-slim:0.8
           ports:
           - containerPort: 80
             name: web
           volumeMounts:
           - name: myvol
             mountPath: /usr/share/nginx/html
     volumeClaimTemplates:
     - metadata:
         name: myvol
       spec:
         accessModes:
         - ReadWriteOnce
         resources:
           requests:
             storage: 20Gi
             iops: "300" #required only for performance storage
         storageClassName: ibmc-file-retain-bronze
    ```
    {: codeblock}

  - **반친화성 규칙이 있고 파일 스토리지 작성을 지연하는 Stateful 세트 예제:**

    다음 예는 NGINX를 3개의 복제본을 포함하는 Stateful 세트로 배치하는 방법을 보여줍니다. Stateful 세트는 파일 스토리지가 작성된 지역 및 구역을 영역을 지정하지 않습니다. 대신, Stateful 세트는 반친화성 규칙을 사용하여 팟(Pod)이 작업자 노드와 구역에 걸쳐 분산되도록 합니다. 작업자 노드 반친화성은 `app: nginx` 레이블을 정의하여 구현됩니다. 이 레이블은 동일한 레이블의 팟(Pod)이 이 작업자 노드에서 이미 실행되는 경우 작업자 노드에서 팟(Pod)을 스케줄하지 않도록 Kubernetes에 지시합니다. `topologykey: failure-domain.beta.kubernetes.io/zone` 레이블은 이 반친화성 규칙을 더 제한하며 `app: nginx` 레이블의 팟(Pod)을 이미 실행 중인 작업자 노드와 동일한 구역에 있는 작업자 노드에서 스케줄되지 않도록 합니다. 각 Stateful 세트 팟(Pod)에 대해 두 개의 PVC가 `volumeClaimTemplates` 섹션에 정의된 대로 작성되지만 파일 스토리지 인스턴스의 작성은 스토리지를 사용하는 Stateful 세트 팟(Pod)이 스케줄될 때까지 지연됩니다. 이 설정을 [토폴로지 인식 볼륨 스케줄링](https://kubernetes.io/blog/2018/10/11/topology-aware-volume-provisioning-in-kubernetes/)이라고 합니다.

    ```
    apiVersion: storage.k8s.io/v1
    kind: StorageClass
    metadata:
      name: ibmc-file-bronze-delayed
    parameters:
      billingType: hourly
      classVersion: "2"
      iopsPerGB: "2"
      sizeRange: '[20-12000]Gi'
      type: Endurance
    provisioner: ibm.io/ibmc-file
    reclaimPolicy: Delete
    volumeBindingMode: WaitForFirstConsumer
    ---
    apiVersion: v1
    kind: Service
    metadata:
      name: nginx
      labels:
        app: nginx
    spec:
      ports:
      - port: 80
        name: web
      clusterIP: None
      selector:
        app: nginx
    ---
    apiVersion: apps/v1
    kind: StatefulSet
    metadata:
      name: web
    spec:
      serviceName: "nginx"
      replicas: 3
      podManagementPolicy: "Parallel"
      selector:
        matchLabels:
          app: nginx
      template:
        metadata:
          labels:
            app: nginx
        spec:
          affinity:
            podAntiAffinity:
              preferredDuringSchedulingIgnoredDuringExecution:
              - weight: 100
            podAffinityTerm:
              labelSelector:
                matchExpressions:
                    - key: app
                  operator: In
                  values:
                      - nginx
                  topologyKey: failure-domain.beta.kubernetes.io/zone
          containers:
          - name: nginx
            image: k8s.gcr.io/nginx-slim:0.8
            ports:
            - containerPort: 80
              name: web
            volumeMounts:
            - name: www
              mountPath: /usr/share/nginx/html
            - name: wwwww
              mountPath: /tmp1
      volumeClaimTemplates:
      - metadata:
          name: myvol1
        spec:
          accessModes:
          - ReadWriteMany # access mode
          resources:
            requests:
              storage: 20Gi
          storageClassName: ibmc-file-bronze-delayed
      - metadata:
          name: myvol2
        spec:
          accessModes:
          - ReadWriteMany # access mode
          resources:
            requests:
              storage: 20Gi
          storageClassName: ibmc-file-bronze-delayed
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
    <td style="text-align:left"><code>spec.podManagementPolicy</code></td>
    <td style="text-align:left">Stateful 세트에 대해 사용할 팟(Pod) 관리 정책을 입력하십시오. 다음 선택사항 중 하나를 선택하십시오. <ul><li><strong><code>OrderedReady</code></strong>: 이 선택사항을 사용하면 Stateful 세트 복제본이 순서대로 배치됩니다. 예를 들어 3개의 복제본을 지정한 경우, Kubernetes는 첫 번째 복제본의 PVC를 작성하고, 이 PVC가 바인드될 때까지 기다리고, Stateful 세트 복제본을 배치하고, PVC를 복제본에 마운트합니다. 이 배치가 완료되고 나면 두 번째 복제본이 배치됩니다. 이 선택사항에 대한 자세한 정보는 [`OrderedReady` Pod Management ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/#orderedready-pod-management)를 참조하십시오. </li><li><strong>Parallel: </strong>이 선택사항을 사용하면 모든 Stateful 세트 복제본의 배치가 동시에 시작됩니다. 앱에서 복제본의 병렬 배치를 지원하는 경우에는 PVC 및 Stateful 세트 복제본의 배치 시간을 절약하기 위해 이 선택사항을 사용하십시오. </li></ul></td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.selector.matchLabels</code></td>
    <td style="text-align:left">Stateful 세트 및 PVC에 포함시킬 모든 레이블을 입력하십시오. Stateful 세트의 <code>volumeClaimTemplates</code>에 포함하는 레이블은 Kubernetes가 인식하지 않습니다. 포함시킬 수 있는 레이블의 샘플로는 다음과 같은 항목이 있습니다. <ul><li><code><strong>region</strong></code> 및 <code><strong>zone</strong></code>: 모든 Stateful 세트 복제본 및 PVC를 하나의 특정 구역에서 작성하려는 경우에는 두 레이블을 모두 추가하십시오. 사용하는 스토리지 클래스에 구역 및 지역을 지정할 수도 있습니다. 다중 구역 클러스터를 보유한 상태에서 구역 및 지역을 지정하지 않으면 모든 구역 간에 볼륨 요청의 균등한 밸런스를 유지하기 위해 스토리지가 프로비저닝되는 구역이 라운드 로빈 기반으로 선택됩니다.</li><li><code><strong>billingType</strong></code>: PVC에 사용할 비용 청구 유형을 입력하십시오. <code>hourly</code> 또는 <code>monthly</code> 중에서 선택하십시오. 이 레이블을 지정하지 않으면 모든 PVC가 시간별 비용 청구 유형으로 작성됩니다. </li></ul></td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.template.metadata.labels</code></td>
    <td style="text-align:left"><code>spec.selector.matchLabels</code> 섹션에 추가한 것과 동일한 레이블을 입력하십시오. </td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.template.spec.affinity</code></td>
    <td style="text-align:left">Stateful 세트 팟(Pod)이 작업자 노드 및 구역에 분산되도록 반친화성 규칙을 지정합니다. 이 예는 Stateful 세트 팟(Pod)이 `app: nginx` 레이블이 있는 팟(Pod)이 실행되는 작업자 노드에서 스케줄되지 않는 것을 선호하는 반친화성 규칙을 보여줍니다. `topologykey: failure-domain.beta.kubernetes.io/zone`은 이 반친화성 규칙을 더 제한하며 작업자 노드가 `app: nginx` 레이블이 있는 팟(Pod)과 동일한 구역에 있는 경우 팟(Pod)이 작업자 노드에서 스케줄되지 않도록 합니다. 이 반친화성 규칙을 사용하면 작업자 노드 및 구역에 대해 반친화성을 구현할 수 있습니다. </td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.volumeClaimTemplates.metadata.name</code></td>
    <td style="text-align:left">볼륨 이름을 입력하십시오. <code>spec.containers.volumeMount.name</code> 섹션에 정의한 것과 동일한 이름을 사용하십시오. 여기에 입력하는 이름은 <code>&lt;volume_name&gt;-&lt;statefulset_name&gt;-&lt;replica_number&gt;</code> 형식으로 PVC의 이름을 작성하는 데 사용됩니다. </td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.volumeClaimTemplates.spec.resources.</code></br><code>requests.storage</code></td>
    <td style="text-align:left">파일 스토리지의 크기를 GB(Gi) 단위로 입력하십시오.</td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.volumeClaimTemplates.spec.resources.</code></br><code>requests.iops</code></td>
    <td style="text-align:left">[성능 스토리지](#file_predefined_storageclass)를 프로비저닝하려면 IOPS 수를 입력하십시오. Endurance 스토리지 클래스를 사용하면서 IOPS 수를 지정하면 IOPS 수가 무시됩니다. 대신 스토리지 클래스에 지정된 IOPS가 사용됩니다.  </td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.volumeClaimTemplates.</code></br><code>spec.storageClassName</code></td>
    <td style="text-align:left">사용할 스토리지 클래스를 입력하십시오. 기존 스토리지 클래스를 나열하려면 <code>kubectl get storageclasses | grep file</code>을 실행하십시오. 스토리지 클래스를 지정하지 않으면 PVC가 클러스터에 설정된 기본 스토리지 클래스를 사용하여 작성됩니다. Stateful 세트가 파일 스토리지를 사용하여 프로비저닝되도록 기본 스토리지 클래스가 <code>ibm.io/ibmc-file</code> 프로비저너를 사용하는지 확인하십시오.</td>
    </tr>
    </tbody></table>

4. Stateful 세트를 작성하십시오.
   ```
   kubectl apply -f statefulset.yaml
   ```
   {: pre}

5. Stateful 세트가 배치될 때까지 기다리십시오.
   ```
   kubectl describe statefulset <statefulset_name>
   ```
   {: pre}

   PVC의 현재 상태를 보려면 `kubectl get pvc`를 실행하십시오. PVC의 이름은 `<volume_name>-<statefulset_name>-<replica_number>`로 형식화됩니다.
   {: tip}

### 정적 프로비저닝: Stateful 세트와 함께 기존 PVC 사용
{: #file_static_statefulset}

Stateful 세트를 작성하기 전에 PVC를 사전 프로비저닝하거나 기존 PVC를 Stateful 세트와 함께 사용할 수 있습니다.
{: shortdesc}

[Stateful 세트를 작성할 때 동적으로 PVC를 프로비저닝](#file_dynamic_statefulset)하는 경우, PVC의 이름은 Stateful 세트 YAML 파일에 사용한 값에 따라 지정됩니다. Stateful 세트가 기존 PVC를 사용하기 위해서는 PVC의 이름이 동적 프로비저닝을 사용할 때 자동으로 작성되는 이름과 일치해야 합니다.

시작하기 전에: [계정에 로그인하십시오. 해당되는 경우, 적절한 리소스 그룹을 대상으로 지정하십시오. 클러스터의 컨텍스트를 설정하십시오.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1. Stateful 세트를 작성하기 전에 사전 프로비저닝하려는 경우, [앱에 파일 스토리지 추가](#add_file)의 1 - 3단계를 따라 각 Stateful 세트 복제본에 대한 PVC를 작성하십시오. 반드시 다음 형식을 따르는 이름을 사용하여 PVC를 작성해야 합니다. `<volume_name>-<statefulset_name>-<replica_number>`
   - **`<volume_name>`**: Stateful 세트의 `spec.volumeClaimTemplates.metadata.name` 섹션에 지정할 이름을 사용하십시오(예: `nginxvol`).
   - **`<statefulset_name>`**: Stateful 세트의 `metadata.name` 섹션에 지정할 이름을 사용하십시오(예: `nginx_statefulset`).
   - **`<replica_number>`**: 복제본의 번호(0부터 시작)를 입력하십시오.

   예를 들어, 3개의 Stateful 세트 복제본을 작성해야 하는 경우에는 `nginxvol-nginx_statefulset-0`, `nginxvol-nginx_statefulset-1` 및 `nginxvol-nginx_statefulset-2`와 같은 이름을 가진 3개의 PVC를 작성하십시오.  

   기존 파일 스토리지 인스턴스에 대한 PVC 및 PV를 작성하시겠습니까? [정적 프로비저닝](#existing_file)을 사용하여 PVC 및 PV를 작성하십시오.
   {: tip}

2. [동적 프로비저닝: Stateful 세트를 작성할 때 PVC 작성](#file_dynamic_statefulset)의 단계에 따라 Stateful 세트를 작성하십시오. PVC의 이름은 `<volume_name>-<statefulset_name>-<replica_number>` 형식을 따릅니다. Stateful 세트 스펙에는 자신이 사용하는 PVC 이름의 다음 값을 사용해야 합니다.
   - **`spec.volumeClaimTemplates.metadata.name`**: PVC 이름의 `<volume_name>`을 입력하십시오.
   - **`metadata.name`**: PVC 이름의 `<statefulset_name>`을 입력하십시오.
   - **`spec.replicas`**: Stateful 세트에 대해 작성할 복제본의 수를 입력하십시오. 복제본의 수는 이전에 작성한 PVC의 수와 동일해야 합니다.

   PVC가 다른 구역에 있는 경우, Stateful 세트에 지역 또는 구역 레이블을 포함하지 마십시오.
   {: note}

3. Stateful 세트 복제본 팟(Pod)에서 PVC가 사용되었는지 확인하십시오.
   1. 클러스터 내의 팟(Pod)을 나열하십시오. 자신의 Stateful 세트에 속한 팟(Pod)을 식별하십시오.
      ```
      kubectl get pods
      ```
      {: pre}

   2. 기존 PVC가 Stateful 세트 복제본에 마운트되었는지 확인하십시오. CLI 출력의 **`Volumes`** 섹션에 있는 **`ClaimName`**을 검토하십시오.
      ```
        kubectl describe pod <pod_name>
      ```
      {: pre}

      출력 예:
      ```
      Name:           nginx-0
      Namespace:      default
      Node:           10.xxx.xx.xxx/10.xxx.xx.xxx
      Start Time:     Fri, 05 Oct 2018 13:24:59 -0400
      ...
      Volumes:
        myvol:
          Type:       PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
          ClaimName:  myvol-nginx-0
     ...
      ```
      {: screen}

<br />


## 기존 스토리지 디바이스의 크기 및 IOPS 변경
{: #file_change_storage_configuration}

스토리지 용량 또는 성능을 높이기 위해 기존 볼륨을 수정할 수 있습니다.
{: shortdesc}

비용 청구에 대한 질문이 있거나 {{site.data.keyword.Bluemix_notm}} 콘솔을 사용한 스토리지 수정 방법에 대한 단계를 찾으려면 [파일 공유 용량 확장](/docs/infrastructure/FileStorage?topic=FileStorage-expandCapacity#expandCapacity)을 참조하십시오.
{: tip}

1. 클러스터의 PVC를 나열하고 **VOLUME** 열에서 연관된 PV의 이름을 기록해 두십시오.
   ```
kubectl get pvc
   ```
   {: pre}

   출력 예:
   ```
   NAME             STATUS    VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS        AGE
   myvol            Bound     pvc-01ac123a-123b-12c3-abcd-0a1234cb12d3   20Gi       RWX            ibmc-file-bronze    147d
   ```
   {: screen}

2. PVC가 바인딩된 PV의 세부사항을 나열하여 PVC와 연관된 실제 파일 스토리지의 **`StorageType`**, **`volumeId`** 및 **`서버`**를 검색하십시오. `<pv_name>`을 이전 단계에서 검색한 PV의 이름으로 대체하십시오. 스토리지 유형, 볼륨 ID 및 서버 이름이 CLI 출력의 **`레이블`** 섹션에 표시됩니다.
   ```
   kubectl describe pv <pv_name>
   ```
   {: pre}

   출력 예:
   ```
   Name:            pvc-4b62c704-5f77-11e8-8a75-b229c11ba64a
   Labels:          CapacityGb=20
                    Datacenter=dal10
                    Iops=2
                    StorageType=ENDURANCE
                    Username=IBM02SEV1543159_6
                    billingType=hourly
                    failure-domain.beta.kubernetes.io/region=us-south
                    failure-domain.beta.kubernetes.io/zone=dal10
                    path=IBM01SEV1234567_8ab12t
                    server=fsf-dal1001g-fz.adn.networklayer.com
                    volumeId=12345678
   ...
   ```
   {: screen}

3. IBM Cloud 인프라(SoftLayer) 계정에서 볼륨의 크기 또는 IOPS를 수정하십시오.

   성능 스토리지의 예:
   ```
   ibmcloud sl file volume-modify <volume_ID> --new-size <size> --new-iops <iops>
   ```
   {: pre}

   endurance 스토리지의 예:
   ```
   ibmcloud sl file volume-modify <volume_ID> --new-size <size> --new-tier <iops>
   ```
   {: pre}

   <table>
   <caption>명령의 컴포넌트 이해</caption>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> YAML 파일 컴포넌트 이해</th>
   </thead>
   <tbody>
   <tr>
   <td><code>&lt;volume_ID&gt;</code></td>
   <td>이전에 검색한 볼륨의 ID를 입력하십시오.</td>
   </tr>
   <tr>
   <td><code>&lt;new-size&gt;</code></td>
   <td>볼륨의 새 크기(Gi)를 입력하십시오. 올바른 크기에 대해서는 [파일 스토리지 구성 결정](#file_predefined_storageclass)을 참조하십시오. 입력된 크기는 볼륨의 현재 크기보다 크거나 같아야 합니다. 새 크기를 지정하지 않으면 볼륨의 현재 크기가 사용됩니다. </td>
   </tr>
   <tr>
   <td><code>&lt;new-iops&gt;</code></td>
   <td>성능 스토리지에만 해당됩니다. 원하는 새 IOPS 수를 입력하십시오. 올바른 IOPS에 대해서는 [파일 스토리지 구성 결정](#file_predefined_storageclass)을 참조하십시오. IOPS를 지정하지 않으면 현재 IOPS가 사용됩니다. <p class="note">볼륨의 원래 IOPS/GB 비율이 0.3 미만인 경우, 새 IOPS/GB 비율은 0.3 미만이어야 합니다. 볼륨의 원래 IOPS/GB 비율이 0.3보다 크거나 같은 경우, 볼륨의 새 IOPS/GB 비율은 0.3보다 크거나 같아아야 합니다.</p> </td>
   </tr>
   <tr>
   <td><code>&lt;new-tier&gt;</code></td>
   <td>endurance 스토리지에만 해당됩니다. 원하는 GB당 새 IOPS 수를 입력하십시오. 올바른 IOPS에 대해서는 [파일 스토리지 구성 결정](#file_predefined_storageclass)을 참조하십시오. IOPS를 지정하지 않으면 현재 IOPS가 사용됩니다. <p class="note">볼륨의 원래 IOPS/GB 비율이 0.25 미만인 경우, 새 IOPS/GB 비율은 0.25 미만이어야 합니다. 볼륨의 원래 IOPS/GB 비율이 0.25보다 크거나 같은 경우, 볼륨의 새 IOPS/GB 비율은 0.25보다 크거나 같아아야 합니다.</p> </td>
   </tr>
   </tbody>
   </table>

   출력 예:
   ```
   Order 31020713 was placed successfully!.
   > Storage as a Service

   > 40 GBs

   > 2 IOPS per GB

   > 20 GB Storage Space (Snapshot Space)

   You may run 'ibmcloud sl file volume-list --order 12345667' to find this file volume after it is ready.
   ```
   {: screen}

4. 볼륨의 크기를 변경했으며 팟(Pod)의 볼륨을 사용하는 경우에는 팟(Pod)에 로그인하여 새 크기를 확인하십시오.
   1. PVC를 사용하는 모든 팟(Pod)을 나열하십시오.
      ```
      kubectl get pods --all-namespaces -o=jsonpath='{range .items[*]}{"\n"}{.metadata.name}{":\t"}{range .spec.volumes[*]}{.persistentVolumeClaim.claimName}{" "}{end}{end}' | grep "<pvc_name>"
      ```
      {: pre}

      팟(Pod)은 다음 형식으로 리턴됩니다. `<pod_name>: <pvc_name>`
   2. 팟(Pod)에 로그인하십시오.
      ```
      kubectl exec -it <pod_name> bash
      ```
      {: pre}

   3. 디스크 사용 통계를 표시하고 이전에 검색한 볼륨의 서버 경로를 찾으십시오.
      ```
      df -h
      ```
      {: pre}

      출력 예:
      ```
      Filesystem                                                      Size  Used Avail Use% Mounted on
      overlay                                                          99G  4.8G   89G   6% /
      tmpfs                                                            64M     0   64M   0% /dev
      tmpfs                                                           7.9G     0  7.9G   0% /sys/fs/cgroup
      fsf-dal1001g-fz.adn.networklayer.com:/IBM01SEV1234567_6/data01   40G     0   40G   0% /myvol
      ```
      {: screen}


## 기본 NFS 버전 변경
{: #nfs_version}

파일 스토리지의 버전은 {{site.data.keyword.Bluemix_notm}} 파일 스토리지 서버와 통신하는 데 사용되는 프로토콜을 판별합니다. 기본적으로 모든 파일 스토리지 인스턴스는 NFS 버전 4를 사용하여 설정됩니다. 앱이 제대로 작동하려면 특정 버전이 필요한 경우 기존 PV를 이전 NFS 버전으로 변경할 수 있습니다.
{: shortdesc}

기본 NFS 버전을 변경하려면 클러스터에서 파일 스토리지를 동적으로 프로비저닝하도록 새 스토리지 클래스를 작성하거나 팟(Pod)에 마운트된 기존 PV를 변경하도록 선택할 수 있습니다.

최신 보안 업데이트를 적용하고 성능을 향상시키려면 기본 NFS 버전을 사용하고 이전 NFS 버전으로 변경하지 마십시오.
{: important}

**특정 NFS 버전을 사용하여 사용자 정의된 스토리지 클래스를 작성하려면 다음을 수행하십시오.**
1. 프로비저닝하고자 하는 NFS 버전으로 [사용자 정의된 스토리지 클래스](#nfs_version_class)를 작성하십시오.
2. 클러스터에 스토리지 클래스를 작성하십시오.
   ```
   kubectl apply -f nfsversion_storageclass.yaml
   ```
   {: pre}

3. 사용자 정의된 스토리지 클래스가 작성되었는지 확인하십시오.
   ```
   kubectl get storageclasses
   ```
   {: pre}

4. 사용자 정의된 스토리지 클래스로 [파일 스토리지](#add_file)를 프로비저닝하십시오.

**다른 NFS 버전을 사용하도록 기존 PV를 변경하려면 다음을 수행하십시오.**

1. NFS 버전을 변경할 파일 스토리지의 PV를 가져오고 PV의 이름을 기록해 두십시오.
   ```
   kubectl get pv
   ```
   {: pre}

2. PV에 어노테이션을 추가하십시오. `<version_number>`를 사용할 NFS 버전으로 대체하십시오. 예를 들어, NFS 버전 3.0으로 변경하려면 **3**을 입력하십시오.  
   ```
   kubectl patch pv <pv_name> -p '{"metadata": {"annotations":{"volume.beta.kubernetes.io/mount-options":"vers=<version_number>"}}}'
   ```
   {: pre}

3. 파일 스토리지를 사용하는 팟(Pod)을 삭제하고 팟(Pod)을 다시 작성하십시오.
   1. 팟(Pod) YAML을 로컬 머신에 저장하십시오.
      ```
      kubect get pod <pod_name> -o yaml > <filepath/pod.yaml>
      ```
      {: pre}

   2. 팟(Pod)을 삭제하십시오.
      ```
      kubectl deleted pod <pod_name>
      ```
      {: pre}

   3. 팟(Pod)을 다시 작성하십시오.
      ```
      kubectl apply -f pod.yaml
      ```
      {: pre}

4. 팟(Pod)이 배치될 때까지 기다리십시오.
   ```
   kubectl get pods
   ```
   {: pre}

   상태가 `Running`으로 변경되면 팟(Pod)이 완전히 배치된 것입니다.

5. 팟(Pod)에 로그인하십시오.
   ```
   kubectl exec -it <pod_name> sh
   ```
   {: pre}

6. 파일 스토리지가 이전에 지정한 NFS 버전으로 마운트되었는지 확인하십시오.
   ```
   mount | grep "nfs" | awk -F" |," '{ print $5, $8 }'
   ```
   {: pre}

   출력 예:
   ```
   nfs vers=3.0
   ```
   {: screen}

<br />


## 데이터 백업 및 복원
{: #file_backup_restore}

파일 스토리지는 클러스터의 작업자 노드와 동일한 위치로 프로비저닝됩니다. 이 스토리지는 서버의 작동이 중지되는 경우에 가용성을 제공하기 위해 IBM에 의해 클러스터된 서버에서 호스팅됩니다. 그러나 파일 스토리지는 자동으로 백업되지 않으며 전체 위치에서 장애가 발생하면 액세스가 불가능할 수 있습니다. 데이터가 유실되거나 손상되지 않도록 하기 위해, 필요한 경우 데이터를 복원하는 데 사용할 수 있는 주기적 백업을 설정할 수 있습니다.
{: shortdesc}

파일 스토리지에 대한 다음의 백업 및 복원 옵션을 검토하십시오.

<dl>
  <dt>주기적 스냅샷 설정</dt>
  <dd><p>특정 시점에 인스턴스 상태를 캡처하는 읽기 전용 이미지인 [파일 스토리지에 대한 주기적 스냅샷을 설정](/docs/infrastructure/FileStorage?topic=FileStorage-snapshots)할 수 있습니다. 스냅샷을 저장하려면 파일 스토리지의 스냅샷 영역을 요청해야 합니다. 스냅샷은 동일한 구역 내의 기본 스토리지 인스턴스에 저장됩니다. 사용자가 실수로 볼륨에서 중요한 데이터를 제거한 경우 스냅샷에서 데이터를 복원할 수 있습니다.</br> <strong>볼륨에 대한 스냅샷을 작성하려면 다음을 수행하십시오. </strong><ol><li>[계정에 로그인하십시오. 해당되는 경우, 적절한 리소스 그룹을 대상으로 지정하십시오. 클러스터의 컨텍스트를 설정하십시오.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)</li><li>`ibmcloud sl` CLI에 로그인하십시오. <pre class="pre"><code>ibmcloud sl init</code></pre></li><li>클러스터에 있는 기존 PV를 나열하십시오. <pre class="pre"><code>kubectl get pv</code></pre></li><li>스냅샷 영역을 작성할 PV에 대한 세부사항을 가져오고 볼륨 ID, 크기 및 IOPS를 기록해 두십시오. <pre class="pre"><code>kubectl describe pv &lt;pv_name&gt;</code></pre> 볼륨 ID, 크기 및 IOPS는 CLI 출력의 <strong>Labels</strong> 섹션에서 찾을 수 있습니다. </li><li>이전 단계에서 검색한 매개변수를 사용하여 기존 볼륨의 스냅샷 크기를 작성하십시오. <pre class="pre"><code>ibmcloud sl file snapshot-order &lt;volume_ID&gt; --size &lt;size&gt; --tier &lt;iops&gt;</code></pre></li><li>스냅샷 크기가 작성될 때까지 기다리십시오. <pre class="pre"><code>ibmcloud sl file volume-detail &lt;volume_ID&gt;</code></pre>CLI 출력의 <strong>Snapshot Size (GB)</strong>가 0에서 주문한 크기로 변경된 경우 스냅샷 크기가 성공적으로 프로비저닝된 것입니다. </li><li>볼륨에 대한 스냅샷을 작성하고 작성된 스냅샷의 ID를 기록해 두십시오. <pre class="pre"><code>ibmcloud sl file snapshot-create &lt;volume_ID&gt;</code></pre></li><li>스냅샷이 작성되었는지 확인하십시오. <pre class="pre"><code>ibmcloud sl file snapshot-list &lt;volume_ID&gt;</code></pre></li></ol></br><strong>스냅샷의 데이터를 기본 볼륨에 복원하려면 다음을 수행하십시오. </strong><pre class="pre"><code>ibmcloud sl file snapshot-restore &lt;volume_ID&gt; &lt;snapshot_ID&gt;</code></pre></p></dd>
  <dt>다른 구역으로 스냅샷 복제</dt>
 <dd><p>구역 장애로부터 데이터를 보호하기 위해 다른 구역에서 설정된 파일 스토리지 인스턴스로 [스냅샷을 복제](/docs/infrastructure/FileStorage?topic=FileStorage-replication#replication)할 수 있습니다. 데이터는 기본 스토리지에서 백업 스토리지로만 복제할 수 있습니다. 복제된 파일 스토리지 인스턴스를 클러스터에 마운트할 수는 없습니다. 기본 스토리지에서 장애가 발생하는 경우에는 복제된 백업 스토리지가 기본 스토리지가 되도록 수동으로 설정할 수 있습니다. 그런 다음 클러스터에 이를 추가할 수 있습니다. 기본 스토리지가 복원되고 나면 백업 스토리지로부터 데이터를 복원할 수 있습니다.</p></dd>
 <dt>스토리지 복제(duplicate)</dt>
 <dd><p>원본 스토리지 인스턴스와 동일한 구역에서 [파일 스토리지 인스턴스를 복제](/docs/infrastructure/FileStorage?topic=FileStorage-duplicatevolume#duplicatevolume)할 수 있습니다. 복제본(duplicate)에는 복제본(duplicate)을 작성한 시점의 원본 스토리지 인스턴스와 동일한 데이터가 저장되어 있습니다. 복제본(replica)과 다르게 복제본(duplicate)은 원본과 별개인 스토리지 인스턴스로 사용하십시오. 복제하려면 우선 [볼륨에 대한 스냅샷을 설정](/docs/infrastructure/FileStorage?topic=FileStorage-snapshots)하십시오.</p></dd>
  <dt>{{site.data.keyword.cos_full}}로 데이터 백업</dt>
  <dd><p>[**ibm-backup-restore 이미지**](/docs/services/RegistryImages/ibm-backup-restore?topic=RegistryImages-ibmbackup_restore_starter#ibmbackup_restore_starter)를 사용하여 클러스터에서 백업을 회전하고 팟(Pod)을 복원할 수 있습니다. 이 팟(Pod)에는 클러스터의 지속적 볼륨 클레임(PVC)에 대한 일회성 또는 주기적 백업을 실행하는 스크립트가 포함되어 있습니다. 데이터는 구역에 설정된 {{site.data.keyword.cos_full}} 인스턴스에 저장됩니다.</p>
  <p>데이터의 고가용성을 개선하고 구역 장애로부터 앱을 보호하려면 두 번째 {{site.data.keyword.cos_full}} 인스턴스를 설정하고 구역 간에 데이터를 복제하십시오. {{site.data.keyword.cos_full}} 인스턴스에서 데이터를 복원해야 하는 경우 이미지와 함께 제공된 복원 스크립트를 사용하십시오.</p></dd>
<dt>팟(Pod) 및 컨테이너에서 데이터 복사</dt>
<dd><p>`kubectl cp` [명령 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/reference/kubectl/overview/#cp)을 사용하여 클러스터의 팟(Pod) 또는 특정 컨테이너에서 파일 및 디렉토리를 복사할 수 있습니다.</p>
<p>시작하기 전에: [계정에 로그인하십시오. 해당되는 경우, 적절한 리소스 그룹을 대상으로 지정하십시오. 클러스터의 컨텍스트를 설정하십시오.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure) <code>-c</code>를 사용하여 컨테이너를 지정하지 않는 경우 이 명령은 팟(Pod)의 사용 가능한 첫 번째 컨테이너를 사용합니다.</p>
<p>이 명령은 다양한 방식으로 사용할 수 있습니다.</p>
<ul>
<li>로컬 머신에서 클러스터의 팟(Pod)으로 데이터 복사: <pre class="pre"><code>kubectl cp <var>&lt;local_filepath&gt;/&lt;filename&gt;</var> <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;</var></code></pre></li>
<li>클러스터의 팟(Pod)에서 로컬 머신으로 데이터 복사: <pre class="pre"><code>kubectl cp <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;/&lt;filename&gt;</var> <var>&lt;local_filepath&gt;/&lt;filename&gt;</var></code></pre></li>
<li>로컬 시스템의 데이터를 클러스터의 팟에서 실행되는 특정 컨테이너로 복사: <pre class="pre"><code>kubectl cp <var>&lt;local_filepath&gt;/&lt;filename&gt;</var> <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;</var> -c <var>&lt;container></var></code></pre></li>
</ul></dd>
  </dl>

<br />


## 스토리지 클래스 참조
{: #file_storageclass_reference}

### 브론즈
{: #file_bronze}

<table>
<caption>파일 스토리지 클래스: 브론즈</caption>
<thead>
<th>특성</th>
<th>설정</th>
</thead>
<tbody>
<tr>
<td>이름</td>
<td><code>ibmc-file-bronze</code></br><code>ibmc-file-retain-bronze</code></td>
</tr>
<tr>
<td>유형</td>
<td>[Endurance 스토리지](/docs/infrastructure/FileStorage?topic=FileStorage-about#provisioning-with-endurance-tiers)</td>
</tr>
<tr>
<td>파일 시스템</td>
<td>NFS</td>
</tr>
<tr>
<td>GB당 IOPS</td>
<td>2</td>
</tr>
<tr>
<td>크기 범위(GB)</td>
<td>20 - 12000Gi</td>
</tr>
<tr>
<td>하드 디스크</td>
<td>SSD</td>
</tr>
<tr>
<td>비용 청구</td>
<td>시간별</td>
</tr>
<tr>
<td>가격</td>
<td>[가격 정보![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/cloud/file-storage/pricing)</td>
</tr>
</tbody>
</table>


### 실버
{: #file_silver}

<table>
<caption>파일 스토리지 클래스: 실버</caption>
<thead>
<th>특성</th>
<th>설정</th>
</thead>
<tbody>
<tr>
<td>이름</td>
<td><code>ibmc-file-silver</code></br><code>ibmc-file-retain-silver</code></td>
</tr>
<tr>
<td>유형</td>
<td>[Endurance 스토리지](/docs/infrastructure/FileStorage?topic=FileStorage-about#provisioning-with-endurance-tiers)</td>
</tr>
<tr>
<td>파일 시스템</td>
<td>NFS</td>
</tr>
<tr>
<td>GB당 IOPS</td>
<td>4</td>
</tr>
<tr>
<td>크기 범위(GB)</td>
<td>20 - 12000Gi</td>
</tr>
<tr>
<td>하드 디스크</td>
<td>SSD</td>
</tr>
<tr>
<td>비용 청구</td>
<td>시간별</li></ul></td>
</tr>
<tr>
<td>가격</td>
<td>[가격 정보![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/cloud/file-storage/pricing)</td>
</tr>
</tbody>
</table>

### 골드
{: #file_gold}

<table>
<caption>파일 스토리지 클래스: 골드</caption>
<thead>
<th>특성</th>
<th>설정</th>
</thead>
<tbody>
<tr>
<td>이름</td>
<td><code>ibmc-file-gold</code></br><code>ibmc-file-retain-gold</code></td>
</tr>
<tr>
<td>유형</td>
<td>[Endurance 스토리지](/docs/infrastructure/FileStorage?topic=FileStorage-about#provisioning-with-endurance-tiers)</td>
</tr>
<tr>
<td>파일 시스템</td>
<td>NFS</td>
</tr>
<tr>
<td>GB당 IOPS</td>
<td>10</td>
</tr>
<tr>
<td>크기 범위(GB)</td>
<td>20 - 4000Gi</td>
</tr>
<tr>
<td>하드 디스크</td>
<td>SSD</td>
</tr>
<tr>
<td>비용 청구</td>
<td>시간별</li></ul></td>
</tr>
<tr>
<td>가격</td>
<td>[가격 정보![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/cloud/file-storage/pricing)</td>
</tr>
</tbody>
</table>

### 사용자 정의
{: #file_custom}

<table>
<caption>파일 스토리지 클래스: 사용자 정의</caption>
<thead>
<th>특성</th>
<th>설정</th>
</thead>
<tbody>
<tr>
<td>이름</td>
<td><code>ibmc-file-custom</code></br><code>ibmc-file-retain-custom</code></td>
</tr>
<tr>
<td>유형</td>
<td>[성능](/docs/infrastructure/FileStorage?topic=FileStorage-about#provisioning-with-performance)</td>
</tr>
<tr>
<td>파일 시스템</td>
<td>NFS</td>
</tr>
<tr>
<td>IOPS 및 크기</td>
<td><p><strong>크기 범위(기가바이트) / IOPS 범위(100의 배수)</strong></p><ul><li>20-39Gi / 100-1000 IOPS</li><li>40-79Gi / 100-2000 IOPS</li><li>80-99Gi / 100-4000 IOPS</li><li>100-499Gi / 100-6000 IOPS</li><li>500-999Gi / 100-10000 IOPS</li><li>1000-1999Gi / 100-20000 IOPS</li><li>2000-2999Gi / 200-40000 IOPS</li><li>3000-3999Gi / 200-48000 IOPS</li><li>4000-7999Gi / 300-48000 IOPS</li><li>8000-9999Gi / 500-48000 IOPS</li><li>10000-12000Gi / 1000-48000 IOPS</li></ul></td>
</tr>
<tr>
<td>하드 디스크</td>
<td>IOPS 대 기가바이트 비율은 프로비저닝되는 하드 디스크의 유형을 결정합니다. IOPS 대 기가바이트 비율을 결정하려면 IOPS를 스토리지의 크기로 나누십시오. </br></br>예: </br>100 IOPS의 500Gi 스토리지를 선택합니다. 비율은 0.2(100 IOPS/500Gi)입니다. </br></br><strong>비율당 하드 디스크 유형의 개요:</strong><ul><li>0.3 이하: SATA</li><li>0.3 초과: SSD</li></ul></td>
</tr>
<tr>
<td>비용 청구</td>
<td>시간별</li></ul></td>
</tr>
<tr>
<td>가격</td>
<td>[가격 정보![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/cloud/file-storage/pricing)</td>
</tr>
</tbody>
</table>

<br />


## 사용자 정의된 샘플 스토리지 클래스
{: #file_custom_storageclass}

사용자 정의된 스토리지 클래스를 작성하고 PVC에서 해당 스토리지 클래스를 사용할 수 있습니다.
{: shortdesc}

{{site.data.keyword.containerlong_notm}}는 특정 계층 및 구성으로 파일 스토리지를 프로비저닝하는 데 사용할 수 있는 [사전 정의된 스토리지 클래스](#file_storageclass_reference)를 제공합니다. 일부 경우에는 사전 정의된 스토리지 클래스에 포함되지 않은 구성으로 스토리지를 프로비저닝하려 할 수도 있습니다. 이 주제의 예를 사용하여 사용자 정의된 스토리지 클래스의 샘플을 찾아볼 수 있습니다.

사용자 정의된 스토리지 클래스를 작성하려면 [스토리지 클래스 사용자 정의](/docs/containers?topic=containers-kube_concepts#customized_storageclass)를 참조하십시오. 그 후 [PVC에서 사용자 정의된 스토리지 클래스를 사용](#add_file)하십시오.

### 토폴로지 인식 스토리지 작성
{: #file-topology}

다중 구역 클러스터에서 파일 스토리지를 사용하려면 볼륨을 읽고 쓸 수 있도록 파일 스토리지 인스턴스와 동일한 구역에서 팟(Pod)이 스케줄되어야 합니다. Kubernetes가 토폴로지 인식 볼륨 스케줄링을 도입하기 전에 스토리지를 동적으로 프로비저닝하면 PVC가 작성될 때 파일 스토리지 인스턴스가 자동으로 작성되었습니다. 그런 다음, 팟(Pod)을 작성할 때 Kubernetes 스케줄러는 파일 스토리지 인스턴스와 동일한 데이터 센터의 작업자 노드에 팟(Pod)을 배치하려고 했습니다.
{: shortdesc}

팟(Pod)의 제한조건을 모르고 파일 스토리지 인스턴스를 작성하면 원하지 않는 결과가 발생할 수 있습니다. 예를 들어, 작업자 노드에 리소스가 충분하지 않거나 작업자 노드가 오염되어 팟이 스케줄될 수 없어 팟(Pod)이 스토리지와 동일한 작업자 노드에 스케줄되지 않을 수도 있습니다. 토폴로지 인식 볼륨 스케줄링을 사용하면 스토리지를 사용하는 첫 번째 팟(Pod)이 작성될 때까지 파일 스토리지 인스턴스가 지연됩니다.

토폴로지 인식 볼륨 스케줄링은 Kubernetes 버전 1.12 이상을 실행하는 클러스터에서 지원됩니다.
{: note}

다음 예제는 이 스토리지를 사용하는 첫 번째 팟(Pod)이 스케줄될 준비가 될 때까지 파일 스토리지 인스턴스 작성을 지연시키는 스토리지 클래스를 작성하는 방법을 보여줍니다. 작성을 지연하려면 `volumeBindingMode: WaitForFirstConsumer` 옵션을 포함해야 합니다. 이 옵션을 포함하지 않으면 `volumeBindingMode`가 `Immediate`로 자동 설정되며 PVC를 작성할 때 파일 스토리지 인스턴스가 작성됩니다.

- **Endurance 파일 스토리지의 예:**
  ```
  apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
    name: ibmc-file-bronze-delayed
  parameters:
    billingType: hourly
    classVersion: "2"
    iopsPerGB: "2"
    sizeRange: '[20-12000]Gi'
    type: Endurance
  provisioner: ibm.io/ibmc-file
  reclaimPolicy: Delete
  volumeBindingMode: WaitForFirstConsumer
  ```
  {: codeblock}

- **성능 파일 스토리지의 예:**
  ```
  apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
   name: ibmc-file-performance-storageclass
   labels:
     kubernetes.io/cluster-service: "true"
  provisioner: ibm.io/ibmc-file
  parameters:
   billingType: "hourly"
   classVersion: "2"
   sizeIOPSRange: |-
     "[20-39]Gi:[100-1000]"
     "[40-79]Gi:[100-2000]"
     "[80-99]Gi:[100-4000]"
     "[100-499]Gi:[100-6000]"
     "[500-999]Gi:[100-10000]"
     "[1000-1999]Gi:[100-20000]"
     "[2000-2999]Gi:[200-40000]"
     "[3000-3999]Gi:[200-48000]"
     "[4000-7999]Gi:[300-48000]"
     "[8000-9999]Gi:[500-48000]"
     "[10000-12000]Gi:[1000-48000]"
   type: "Performance"
  reclaimPolicy: Delete
  volumeBindingMode: WaitForFirstConsumer
  ```
  {: codeblock}

### 다중 구역 클러스터에 대한 구역 지정
{: #file_multizone_yaml}

특정 구역에 파일 스토리지를 작성하려는 경우 사용자 정의 스토리지 클래스에 구역과 지역을 지정할 수 있습니다.
{: shortdesc}

특정 구역에서 [정적으로 파일 스토리지를 프로비저닝](#existing_file)하려는 경우에는 사용자 정의된 스토리지 클래스를 사용하십시오. 그 외의 모든 경우에는 [PVC에 구역을 직접 지정](#add_file)하십시오.
{: note}

사용자 정의된 스토리지 클래스를 작성하는 경우 클러스터 및 작업자 노드가 있는 동일한 지역 및 구역을 지정하십시오. 클러스터의 지역을 가져오려면 `ibmcloud ks cluster-get --cluster <cluster_name_or_ID>`를 실행하여 **마스터 URL**에서 지역 접두부를 찾으십시오(예: `https://c2.eu-de.containers.cloud.ibm.com:11111`에서 `eu-de`). 작업자 노드의 구역을 가져오려면 `ibmcloud ks workers --cluster <cluster_name_or_ID>`를 실행하십시오.

- **Endurance 파일 스토리지의 예:**
  ```
  apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
    name: ibmc-file-silver-mycustom-storageclass
    labels:
      kubernetes.io/cluster-service: "true"
  provisioner: ibm.io/ibmc-file
  parameters:
    zone: "dal12"
    region: "us-south"
    type: "Endurance"
    iopsPerGB: "4"
    sizeRange: "[20-12000]Gi"
    reclaimPolicy: "Delete"
    classVersion: "2"
  reclaimPolicy: Delete
  volumeBindingMode: Immediate
  ```
  {: codeblock}

- **성능 파일 스토리지의 예:**
  ```
  apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
   name: ibmc-file-performance-storageclass
   labels:
     kubernetes.io/cluster-service: "true"
  provisioner: ibm.io/ibmc-file
  parameters:
   zone: "dal12"
   region: "us-south"
   billingType: "hourly"
   classVersion: "2"
   sizeIOPSRange: |-
     "[20-39]Gi:[100-1000]"
     "[40-79]Gi:[100-2000]"
     "[80-99]Gi:[100-4000]"
     "[100-499]Gi:[100-6000]"
     "[500-999]Gi:[100-10000]"
     "[1000-1999]Gi:[100-20000]"
     "[2000-2999]Gi:[200-40000]"
     "[3000-3999]Gi:[200-48000]"
     "[4000-7999]Gi:[300-48000]"
     "[8000-9999]Gi:[500-48000]"
     "[10000-12000]Gi:[1000-48000]"
   type: "Performance"
  reclaimPolicy: Delete
  volumeBindingMode: Immediate
  ```
  {: codeblock}

### 기본 NFS 버전 변경
{: #nfs_version_class}

다음 사용자 정의된 스토리지 클래스는 프로비저닝하려는 NFS 버전을 정의할 수 있도록 허용합니다. 예를 들어, NFS 버전 3.0을 프로비저닝하려면 `<nfs_version>`을 **3.0**으로 대체하십시오.
{: shortdesc}

- **Endurance 파일 스토리지의 예:**
  ```
  apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
    name: ibmc-file-mount
    labels:
      kubernetes.io/cluster-service: "true"
  provisioner: ibm.io/ibmc-file
  parameters:
    type: "Endurance"
    iopsPerGB: "2"
    sizeRange: "[1-12000]Gi"
    reclaimPolicy: "Delete"
    classVersion: "2"
    mountOptions: nfsvers=<nfs_version>
  ```
  {: codeblock}

- **성능 파일 스토리지의 예:**
  ```
  apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
    name: ibmc-file-mount
    labels:
      kubernetes.io/cluster-service: "true"
  provisioner: ibm.io/ibmc-file
  parameters:
   type: "Performance"
   classVersion: "2"
   sizeIOPSRange: |-
     "[20-39]Gi:[100-1000]"
     "[40-79]Gi:[100-2000]"
     "[80-99]Gi:[100-4000]"
     "[100-499]Gi:[100-6000]"
     "[500-999]Gi:[100-10000]"
     "[1000-1999]Gi:[100-20000]"
     "[2000-2999]Gi:[200-40000]"
     "[3000-3999]Gi:[200-48000]"
     "[4000-7999]Gi:[300-48000]"
     "[8000-9999]Gi:[500-48000]"
     "[10000-12000]Gi:[1000-48000]"
   mountOptions: nfsvers=<nfs_version>
  ```
  {: codeblock}
