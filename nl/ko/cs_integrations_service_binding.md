---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-05"

keywords: kubernetes, iks, helm, without tiller, private cluster tiller, integrations, helm chart

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



# IBM Cloud 서비스 바인딩을 사용하여 서비스 추가
{: #service-binding}

Watson AI, 데이터, 보안 및 IoT(Internet of Things) 등의 분야에서 추가 기능으로 Kubernetes 클러스터를 개선하려면 {{site.data.keyword.Bluemix_notm}} 서비스를 추가하십시오.
{:shortdesc}

**내 클러스터에 바인드할 수 있는 서비스 유형은 무엇입니까?** </br>
{{site.data.keyword.Bluemix_notm}} 서비스를 클러스터에 추가하는 경우 {{site.data.keyword.Bluemix_notm}} Identity and Access Management(IAM)가 사용으로 설정된 서비스와 Cloud Foundry를 기반으로 하는 서비스 중에서 선택할 수 있습니다. IAM 사용 서비스는 보다 세부적인 액세스 제어를 제공하고 {{site.data.keyword.Bluemix_notm}} 리소스 그룹에서 관리될 수 있습니다. Cloud Foundry 서비스는 Cloud Foundry 조직 및 공간에 추가되어야 하며 리소스 그룹에 추가될 수 없습니다. Cloud Foundry 서비스 인스턴스에 대한 액세스를 제어하기 위해 Cloud Foundry 역할을 사용합니다. IAM 사용 서비스 및 Cloud Foundry 서비스에 대한 자세한 정보는 [리소스는 무엇입니까?](/docs/resources?topic=resources-resource#resource)를 참조하십시오.

지원되는 {{site.data.keyword.Bluemix_notm}} 서비스 목록을 찾으려면 [{{site.data.keyword.Bluemix_notm}} 카탈로그](https://cloud.ibm.com/catalog)를 참조하십시오.

**{{site.data.keyword.Bluemix_notm}} 서비스 바인딩은 무엇입니까?**</br>
서비스 바인딩은 {{site.data.keyword.Bluemix_notm}} 서비스에 대한 서비스 인증 정보를 작성하고 클러스터의 Kubernetes 시크릿에 이 인증 정보를 저장할 수 있는 빠른 방법입니다. 클러스터에 서비스를 바인드하려면 우선 서비스의 인스턴스를 프로비저닝해야 합니다. 그런 다음 `ibmcloud ks cluster-service-bind` [명령](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_service_bind)을 사용하여 서비스 인증 정보 및 Kubernetes 시크릿을 작성합니다. Kubernetes 시크릿은 데이터 보호를 위해 etcd에서 자동으로 암호화됩니다.

secret을 더 안전하게 보호하려 하십니까? 클러스터에서 {{site.data.keyword.Bluemix_notm}} 서비스 인스턴스의 인증 정보를 저장하는 secret과 같은 기존 및 신규 secret을 암호화하기 위해 클러스터 관리자에게 [{{site.data.keyword.keymanagementservicefull}}를 사용으로 설정](/docs/containers?topic=containers-encryption#keyprotect)하도록 요청하십시오.
{: tip}

**내 클러스터에서 모든 {{site.data.keyword.Bluemix_notm}} 서비스를 사용할 수 있습니까?**</br>
서비스 인증 정보를 자동으로 작성하고 Kubernetes 시크릿에 저장할 수 있도록 서비스 키를 지원하는 서비스에 대한 서비스 바인딩만 사용할 수 있습니다. 서비스 키를 지원하는 서비스의 목록을 찾으려면 [외부 앱이 {{site.data.keyword.Bluemix_notm}} 서비스를 사용할 수 있도록 허용](/docs/resources?topic=resources-externalapp#externalapp)을 참조하십시오.

서비스 키를 지원하지 않는 서비스는 일반적으로 앱에서 사용할 수 있는 API를 제공합니다. 서비스 바인딩 메소드는 앱에 대한 API 액세스를 자동으로 설정하지 않습니다. 서비스의 API 문서를 검토하고 앱에서 API 인터페이스를 구현해야 합니다.

## 클러스터에 IBM Cloud 서비스 추가
{: #bind-services}

{{site.data.keyword.Bluemix_notm}} 서비스 바인딩을 사용하여 {{site.data.keyword.Bluemix_notm}} 서비스에 대한 서비스 인증 정보를 자동으로 작성하고 Kubernetes 시크릿에 이 인증 정보를 저장합니다.
{: shortdesc}

시작하기 전에:
- 다음 역할을 보유하고 있는지 확인하십시오.
    - 서비스를 바인드할 클러스터에 대한 [**편집자** 또는 **관리자** {{site.data.keyword.Bluemix_notm}} IAM 플랫폼 액세스 역할](/docs/containers?topic=containers-users#platform)
    - 서비스를 바인드할 Kubernetes 네임스페이스에 대한 [**작성자** 또는 **관리자** {{site.data.keyword.Bluemix_notm}} IAM 서비스 역할](/docs/containers?topic=containers-users#platform)
    - Cloud Foundry 서비스: 서비스를 프로비저닝하려는 영역에 대한 [**Developer** Cloud Foundry 역할](/docs/iam?topic=iam-mngcf#mngcf)
- [계정에 로그인하십시오. 해당되는 경우, 적절한 리소스 그룹을 대상으로 지정하십시오. 클러스터의 컨텍스트를 설정하십시오.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

클러스터에 {{site.data.keyword.Bluemix_notm}} 서비스를 추가하려면 다음을 수행하십시오.

1. [{{site.data.keyword.Bluemix_notm}} 서비스의 인스턴스를 작성](/docs/resources?topic=resources-externalapp#externalapp)하십시오.
    * 몇몇 {{site.data.keyword.Bluemix_notm}} 서비스는 일부 지역에서만 사용 가능합니다. 서비스가 클러스터와 동일한 지역에서 사용 가능한 경우에만 클러스터에 서비스를 바인드할 수 있습니다. 또한 워싱턴 DC 구역에서 서비스 인스턴스를 작성하려면 CLI를 사용해야 합니다.
    * **IAM 사용 서비스의 경우**: 서비스 인스턴스는 클러스터와 동일한 리소스 그룹에 작성해야 합니다. 서비스는 하나의 리소스 그룹에서만 작성할 수 있으며 이후에는 변경할 수 없습니다.

2. 작성한 서비스 유형을 확인하고 서비스 인스턴스 **이름**을 기록해 두십시오.
   - **Cloud Foundry 서비스:**
     ```
     ibmcloud service list
     ```
     {: pre}

     출력 예:
     ```
     name                         service           plan    bound apps   last operation
     <cf_service_instance_name>   <service_name>    spark                create succeeded
     ```
     {: screen}

  - **{{site.data.keyword.Bluemix_notm}}IAM 사용 서비스:**
     ```
     ibmcloud resource service-instances
     ```
     {: pre}

     출력 예:
     ```
     Name                          Location   State    Type               Tags
     <iam_service_instance_name>   <region>   active   service_instance
     ```
     {: screen}

   **Cloud Foundry 서비스** 및 **서비스**로서 {{site.data.keyword.Bluemix_notm}} 대시보드에서 다른 서비스 유형을 볼 수도 있습니다.

3. 서비스를 추가하는 데 사용할 클러스터 네임스페이스를 식별하십시오.
   ```
   kubectl get namespaces
   ```
   {: pre}

4. 서비스를 클러스터에 바인드하여 서비스에 대한 서비스 인증 정보를 작성하고 Kubernetes 시크릿에 인증 정보를 저장하십시오. 기존 서비스 인증 정보가 있는 경우 `--key` 플래그를 사용하여 인증 정보를 지정하십시오. IAM 사용 서비스의 경우 인증 정보는 자동으로 **작성자** 서비스 액세스 역할로 작성되지만 `--role` 플래그를 사용하여 다른 서비스 액세스 역할을 지정할 수 있습니다. `--key` 플래그를 사용하지 않는 경우, `--role` 플래그를 포함하지 마십시오.
   ```
    ibmcloud ks cluster-service-bind --cluster <cluster_name_or_ID> --namespace <namespace> --service <service_instance_name> [--key <service_instance_key>] [--role <IAM_service_role>]
   ```
   {: pre}

   서비스 인증 정보의 작성에 성공하면 이름이 `binding-<service_instance_name>`인 Kubernetes 시크릿이 작성됩니다.  

   출력 예:
   ```
   ibmcloud ks cluster-service-bind --cluster mycluster --namespace mynamespace --service cleardb
   Binding service instance to namespace...
   OK
   Namespace:	     mynamespace
   Secret name:     binding-<service_instance_name>
   ```
   {: screen}

5. Kubernetes 시크릿의 서비스 인증 정보를 확인하십시오.
   1. 시크릿의 세부사항을 가져오고 **바인딩** 값을 기록해 두십시오. **바인딩** 값은 base64 인코딩되어 있으며, JSON 형식으로 서비스 인스턴스의 인증 정보를 보관합니다.
      ```
       kubectl get secrets binding-<service_instance_name> --namespace=<namespace> -o yaml
      ```
      {: pre}

      출력 예:
      ```
      apiVersion: v1
      data:
        binding: <binding>
      kind: Secret
      metadata:
        annotations:
          service-instance-id: 1111aaaa-a1aa-1aa1-1a11-111aa111aa11
          service-key-id: 2b22bb2b-222b-2bb2-2b22-b22222bb2222
        creationTimestamp: 2018-08-07T20:47:14Z
        name: binding-<service_instance_name>
        namespace: <namespace>
        resourceVersion: "6145900"
        selfLink: /api/v1/namespaces/default/secrets/binding-mycloudant
        uid: 33333c33-3c33-33c3-cc33-cc33333333c
      type: Opaque
      ```
      {: screen}

   2. 바인딩 값을 디코딩하십시오.
      ```
      echo "<binding>" | base64 -D
      ```
      {: pre}

      출력 예:
      ```
      {"apikey":"KL34Ys893284NGJEPFjgrioJ12NElpow","host":"98765aab-9ce1-7tr3-ba87-bfbab6e6d9d6-bluemix.cloudant.com","iam_apikey_description":"Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:cloudantnosqldb:us-south:a/1234g56789cfe8e6388dd2ec098:98746cw-43d7-49ce-947a-d8fe3eebb381::","iam_apikey_name":"auto-generated-apikey-1234abcde-987f-3t64-9d96-d13775ec5663","iam_role_crn":"crn:v1:bluemix:public:iam::::serviceRole:Writer","iam_serviceid_crn":"crn:v1:bluemix:public:iam-identity::a/1234567890brasge5htn2ec098::serviceid:ServiceId-12345vgh-6c4c-ytr12-af6b-467d30d6ef44","password":"jfiavhui12484fnivhuo472nvei23913c3ff","port":443,"url":"https://25c73aac-9ce1-4c24-ba98-bfbab6e6d9d6-bluemix:ugvioev823inreuiegn43donvri29989wiu9t22@25c73aac-9ce1-4c24-ba98-abdrjio123562lnsb-bluemix.cloudant.com","username":"123b45da-9ce1-4c24-ab12-rinwnwub1294-bluemix"}
      ```
      {: screen}

   3. 선택사항: 이전 단계에서 디코딩한 서비스 인증 정보를 {{site.data.keyword.Bluemix_notm}} 대시보드에서 서비스 인스턴스에 대해 찾은 서비스 인증 정보와 비교하십시오.

6. 이제 서비스가 클러스터에 바인드되었으므로 [Kubernetes 시크릿의 서비스 인증 정보에 액세스](#adding_app)하도록 앱을 구성해야 합니다.


## 앱에서 서비스 인증 정보에 액세스
{: #adding_app}

앱에서 {{site.data.keyword.Bluemix_notm}} 서비스 인스턴스에 액세스하려면 Kubernetes 시크릿에 저장된 서비스 인증 정보를 앱에서 사용할 수 있도록 해야 합니다.
{: shortdesc}

서비스 인스턴스의 인증 정보는 base64 인코딩되어 있으며, JSON 형식으로 시크릿 내에 저장됩니다. 시크릿의 데이터에 액세스하려면 다음 옵션 중에서 선택하십시오.
- [볼륨으로서 시크릿을 팟(Pod)에 마운트](#mount_secret)
- [환경 변수의 시크릿 참조](#reference_secret)
<br>

시작하기 전에:
-  `kube-system` 네임스페이스에 대해 [**작성자** 또는 **관리자** {{site.data.keyword.Bluemix_notm}} IAM 서비스 역할](/docs/containers?topic=containers-users#platform)이 있는지 확인하십시오.
- [계정에 로그인하십시오. 해당되는 경우, 적절한 리소스 그룹을 대상으로 지정하십시오. 클러스터의 컨텍스트를 설정하십시오.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
- [{{site.data.keyword.Bluemix_notm}} 서비스를 클러스터에 추가](#bind-services)하십시오.

### 볼륨으로서 시크릿을 팟(Pod)에 마운트
{: #mount_secret}

볼륨으로서 시크릿을 팟(Pod)에 마운트하면 이름이 `binding`인 파일이 볼륨 마운트 디렉토리에 저장됩니다. JSON 형식의 `binding` 파일에는 {{site.data.keyword.Bluemix_notm}} 서비스에 액세스하는 데 필요한 모든 정보와 인증 정보가 포함되어 있습니다.
{: shortdesc}

1.  클러스터의 사용 가능한 시크릿을 나열하고 시크릿의 **이름**을 기록해 두십시오. **오파크** 유형의 시크릿을 찾으십시오. 다수의 시크릿이 존재하면 클러스터 관리자에게 문의하여 올바른 서비스 시크릿을 식별하십시오.
    ```
    kubectl get secrets
    ```
    {: pre}

    출력 예:
    ```
    NAME                              TYPE            DATA      AGE
    binding-<service_instance_name>   Opaque          1         3m
    ```
    {: screen}

2.  Kubernetes 배치를 위한 YAML 파일을 작성하고 볼륨으로서 시크릿을 팟(Pod)에 마운트하십시오.
    ```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      labels:
        app: secret-test
      name: secret-test
      namespace: <my_namespace>
    spec:
      selector:
        matchLabels:
          app: secret-test
      replicas: 1
      template:
        metadata:
          labels:
            app: secret-test
        spec:
          containers:
          - image: icr.io/ibmliberty:latest
            name: secret-test
            volumeMounts:
            - mountPath: <mount_path>
              name: <volume_name>
          volumes:
          - name: <volume_name>
            secret:
              defaultMode: 420
              secretName: binding-<service_instance_name>
    ```
    {: codeblock}

    <table>
    <caption>YAML 파일 컴포넌트 이해</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> YAML 파일 컴포넌트 이해</th>
    </thead>
    <tbody>
    <tr>
    <td><code>volumeMounts.mountPath</code></td>
    <td>컨테이너 내에서 볼륨이 마운트되는 디렉토리의 절대 경로입니다.</td>
    </tr>
    <tr>
    <td><code>volumeMounts.name</code></br><code>volumes.name</code></td>
    <td>팟(Pod)에 마운트할 볼륨의 이름입니다.</td>
    </tr>
    <tr>
    <td><code>secret.defaultMode</code></td>
    <td>시크릿에 대한 읽기 및 쓰기 권한입니다. 읽기 전용 권한을 설정하려면 `420`을 사용하십시오. </td>
    </tr>
    <tr>
    <td><code>secret.secretName</code></td>
    <td>이전 단계에서 기록해 둔 시크릿의 이름입니다.</td>
    </tr></tbody></table>

3.  팟(Pod)을 작성하고 볼륨으로서 시크릿을 마운트하십시오.
    ```
    kubectl apply -f secret-test.yaml
    ```
    {: pre}

4.  팟(Pod)이 작성되었는지 확인하십시오.
    ```
    kubectl get pods
    ```
    {: pre}

    CLI 출력 예:

    ```
    NAME                           READY     STATUS    RESTARTS   AGE
    secret-test-1111454598-gfx32   1/1       Running   0          1m
    ```
    {: screen}

5.  서비스 인증 정보에 액세스하십시오.
    1. 팟(Pod)에 로그인하십시오.
       ```
       kubectl exec <pod_name> -it bash
       ```
       {: pre}

    2. 이전에 정의한 볼륨 마운트 경로로 이동하고 볼륨 마운트 경로의 파일을 나열하십시오.
       ```
       cd <volume_mountpath> && ls
       ```
       {: pre}

       출력 예:
       ```
       binding
       ```
       {: screen}

       `binding` 파일에는 Kubernetes 시크릿에 저장된 서비스 인증 정보가 포함되어 있습니다.

    4. 서비스 인증 정보를 보십시오. 인증 정보는 JSON 형식의 키 값 쌍으로 저장됩니다.
       ```
       cat binding
       ```
       {: pre}

       출력 예:
       ```
       {"apikey":"KL34Ys893284NGJEPFjgrioJ12NElpow","host":"98765aab-9ce1-7tr3-ba87-bfbab6e6d9d6-bluemix.cloudant.com","iam_apikey_description":"Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:cloudantnosqldb:us-south:a/1234g56789cfe8e6388dd2ec098:98746cw-43d7-49ce-947a-d8fe3eebb381::","iam_apikey_name":"auto-generated-apikey-1234abcde-987f-3t64-9d96-d13775ec5663","iam_role_crn":"crn:v1:bluemix:public:iam::::serviceRole:Writer","iam_serviceid_crn":"crn:v1:bluemix:public:iam-identity::a/1234567890brasge5htn2ec098::serviceid:ServiceId-12345vgh-6c4c-ytr12-af6b-467d30d6ef44","password":"jfiavhui12484fnivhuo472nvei23913c3ff","port":443,"url":"https://25c73aac-9ce1-4c24-ba98-bfbab6e6d9d6-bluemix:ugvioev823inreuiegn43donvri29989wiu9t22@25c73aac-9ce1-4c24-ba98-abdrjio123562lnsb-bluemix.cloudant.com","username":"123b45da-9ce1-4c24-ab12-rinwnwub1294-bluemix"}
       ```
       {: screen}

    5. JSON 컨텐츠를 구문 분석하고 서비스에 액세스하는 데 필요한 정보를 검색하도록 앱을 구성하십시오.

### 환경 변수의 시크릿 참조
{: #reference_secret}

서비스 인증 정보와 Kubernetes 시크릿의 기타 키 값 쌍을 환경 변수로서 배치에 추가할 수 있습니다.
{: shortdesc}

1. 클러스터의 사용 가능한 시크릿을 나열하고 시크릿의 **이름**을 기록해 두십시오. **오파크** 유형의 시크릿을 찾으십시오. 다수의 시크릿이 존재하면 클러스터 관리자에게 문의하여 올바른 서비스 시크릿을 식별하십시오.
   ```
    kubectl get secrets
   ```
   {: pre}

   출력 예:
   ```
    NAME                              TYPE            DATA      AGE
    binding-<service_instance_name>   Opaque          1         3m
   ```
   {: screen}

2. 시크릿의 세부사항을 가져와서 팟(Pod)에서 환경 변수로서 참조할 수 있는 잠재적 키 값 쌍을 찾으십시오. 서비스 인증 정보는 시크릿의 `binding` 키에 저장됩니다.
   ```
   kubectl get secrets binding-<service_instance_name> --namespace=<namespace> -o yaml
   ```
   {: pre}

   출력 예:
   ```
   apiVersion: v1
   data:
     binding: <binding>
   kind: Secret
   metadata:
     annotations:
       service-instance-id: 7123acde-c3ef-4ba2-8c52-439ac007fa70
       service-key-id: 9h30dh8a-023f-4cf4-9d96-d12345ec7890
     creationTimestamp: 2018-08-07T20:47:14Z
     name: binding-<service_instance_name>
     namespace: <namespace>
     resourceVersion: "6145900"
     selfLink: /api/v1/namespaces/default/secrets/binding-mycloudant
     uid: 12345a31-9a83-11e8-ba83-cd49014748f
   type: Opaque
   ```
   {: screen}

3. Kubernetes 배치를 위한 YAML 파일을 작성하고 `binding` 키를 참조하는 환경 변수를 지정하십시오.
   ```
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     labels:
       app: secret-test
     name: secret-test
     namespace: <my_namespace>
   spec:
     selector:
       matchLabels:
         app: secret-test
     template:
       metadata:
         labels:
           app: secret-test
       spec:
         containers:
         - image: icr.io/ibmliberty:latest
           name: secret-test
           env:
           - name: BINDING
             valueFrom:
               secretKeyRef:
                 name: binding-<service_instance_name>
                 key: binding
     ```
     {: codeblock}

     <table>
     <caption>YAML 파일 컴포넌트 이해</caption>
     <thead>
     <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> YAML 파일 컴포넌트 이해</th>
     </thead>
     <tbody>
     <tr>
     <td><code>containers.env.name</code></td>
     <td>환경 변수의 이름입니다.</td>
     </tr>
     <tr>
     <td><code>env.valueFrom.secretKeyRef.name</code></td>
     <td>이전 단계에서 기록해 둔 시크릿의 이름입니다.</td>
     </tr>
     <tr>
     <td><code>env.valueFrom.secretKeyRef.key</code></td>
     <td>시크릿의 일부이며 사용자가 환경 변수에서 참조하고자 하는 키입니다. 서비스 인증 정보를 참조하려면 <strong>바인딩</strong> 키를 사용해야 합니다.  </td>
     </tr>
     </tbody></table>

4. 환경 변수로서 시크릿의 `binding` 키를 참조하는 팟(Pod)을 작성하십시오.
   ```
    kubectl apply -f secret-test.yaml
   ```
   {: pre}

5. 팟(Pod)이 작성되었는지 확인하십시오.
   ```
   kubectl get pods
   ```
   {: pre}

   CLI 출력 예:
   ```
   NAME                           READY     STATUS    RESTARTS   AGE
   secret-test-1111454598-gfx32   1/1       Running   0          1m
   ```
   {: screen}

6. 환경 변수가 올바르게 설정되었는지 확인하십시오.
   1. 팟(Pod)에 로그인하십시오.
      ```
      kubectl exec <pod_name> -it bash
      ```
      {: pre}

   2. 팟(Pod)의 모든 환경 변수를 나열하십시오.
      ```
      env
      ```
      {: pre}

      출력 예:
      ```
      BINDING={"apikey":"KL34Ys893284NGJEPFjgrioJ12NElpow","host":"98765aab-9ce1-7tr3-ba87-bfbab6e6d9d6-bluemix.cloudant.com","iam_apikey_description":"Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:cloudantnosqldb:us-south:a/1234g56789cfe8e6388dd2ec098:98746cw-43d7-49ce-947a-d8fe3eebb381::","iam_apikey_name":"auto-generated-apikey-1234abcde-987f-3t64-9d96-d13775ec5663","iam_role_crn":"crn:v1:bluemix:public:iam::::serviceRole:Writer","iam_serviceid_crn":"crn:v1:bluemix:public:iam-identity::a/1234567890brasge5htn2ec098::serviceid:ServiceId-12345vgh-6c4c-ytr12-af6b-467d30d6ef44","password":"jfiavhui12484fnivhuo472nvei23913c3ff","port":443,"url":"https://25c73aac-9ce1-4c24-ba98-bfbab6e6d9d6-bluemix:ugvioev823inreuiegn43donvri29989wiu9t22@25c73aac-9ce1-4c24-ba98-abdrjio123562lnsb-bluemix.cloudant.com","username":"123b45da-9ce1-4c24-ab12-rinwnwub1294-bluemix"}
      ```
      {: screen}

7. 환경 변수를 읽고 JSON 컨텐츠를 구문 분석하여 서비스에 액세스하는 데 필요한 정보를 검색하도록 앱을 구성하십시오.

   Python의 예제 코드:
   ```python
   if os.environ.get('BINDING'):
        credentials = json.loads(os.environ.get('BINDING'))
   ```
   {: codeblock}

8. 선택사항: 예방 조치로 `BINDING` 환경변수가 올바르게 설정되지 않은 경우 오류 처리를 앱에 추가하십시오.

   Java의 예제 코드:
   ```java
   if (System.getenv("BINDING") == null) {
    throw new RuntimeException("Environment variable 'SECRET' is not set!");
   }
   ```
   {: codeblock}

   Node.js의 예제 코드:
   ```js
   if (!process.env.BINDING) {
    console.error('ENVIRONMENT variable "BINDING" is not set!');
    process.exit(1);
   }
   ```
   {: codeblock}
