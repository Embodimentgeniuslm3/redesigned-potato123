---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-04"

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



# 이미지에서 컨테이너 빌드
{: #images}

Docker 이미지는 {{site.data.keyword.containerlong}}를 사용하여 작성하는 모든 컨테이너의 기초가 됩니다.
{:shortdesc}

이미지는 이미지를 빌드하는 지시사항이 포함된 파일인 Dockerfile에서 작성됩니다. Dockerfile은 앱, 해당 앱의 구성 및 그 종속 항목과 같이 개별적으로 저장되는 해당 지시사항의 빌드 아티팩트를 참조할 수 있습니다.

## 이미지 레지스트리 계획
{: #planning_images}

일반적으로 이미지는 공용으로 액세스 가능한 레지스트리(공용 레지스트리) 또는 소규모 사용자 그룹에 대한 제한된 액세스 권한으로 설정된 레지스트리(개인용 레지스트리)에 저장됩니다.
{:shortdesc}

공용 레지스트리(예: Docker Hub)는 클러스터에서 첫 번째 컨테이너화된 앱을 작성하기 위해 Docker 및 Kubernetes를 시작하는 데 사용될 수 있습니다. 그러나 엔터프라이즈 애플리케이션인 경우에는 {{site.data.keyword.registryshort_notm}}에 제공된 레지스트리와 같은 개인용 레지스트리를 사용하여 권한 없는 사용자가 이미지를 사용하고 변경하지 못하도록 보호하십시오. 
개인용 레지스트리에 액세스하기 위한 인증 정보를 클러스터 사용자가 사용할 수 있도록 보장하기 위해,
개인용 레지스트리는 클러스터 관리자에 의해 설정되어야 합니다.


{{site.data.keyword.containerlong_notm}}에서 다중 레지스트리를 사용하여 클러스터에 앱을 배치할 수 있습니다.

|레지스트리|설명|이점|
|--------|-----------|-------|
|[{{site.data.keyword.registryshort_notm}}](/docs/services/Registry?topic=registry-getting-started#getting-started)|이 옵션을 사용하면 이미지를 안전하게 저장하고 이를 클러스터 사용자 간에 공유할 수 있는 {{site.data.keyword.registryshort_notm}}의 사용자 고유의 보안 Docker 이미지 저장소를 설정할 수 있습니다.|<ul><li>계정에서 이미지에 대한 액세스 권한을 관리합니다.</li><li>{{site.data.keyword.IBM_notm}}이 제공하는 이미지와 샘플 앱 {{site.data.keyword.IBM_notm}} Liberty를 상위 이미지로서 사용하고 이에 자체 앱 코드를 추가합니다.</li><li>Vulnerability Advisor에 의해 잠재적 취약점에 대한 이미지의 자동 스캐닝(이를 해결하기 위한 OS 특정 권장사항 포함).</li></ul>|
|기타 개인용 레지스트리|[이미지 풀 시크릿 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/containers/images/)을 작성하여 기존 개인용 레지스트리를 클러스터에 연결합니다. 시크릿은 Kubernetes 시크릿에 레지스트리 URL과 인증 정보를 안전하게 저장하는 데 사용됩니다.|<ul><li>자체 소스(Docker Hub, 조직이 소유하는 레지스트리 또는 기타 프라이빗 클라우드 레지스트리)와는 무관하게 기존 개인용 레지스트리를 사용합니다.</li></ul>|
|[공용 Docker Hub ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://hub.docker.com/){: #dockerhub}|Dockerfile 변경이 필요하지 않은 경우 [Kubernetes 배치 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)에서 Docker Hub의 기존 공용 이미지를 직접 사용하려면 이 옵션을 사용하십시오. <p>**참고:** 이 옵션이 조직의 보안 요구사항을 충족하지 않을 수 있음을 유념하십시오(예: 액세스 관리, 취약성 스캐닝 또는 앱 개인정보 보호정책).</p>|<ul><li>클러스터에 대한 추가 단계가 필요하지 않습니다.</li><li>다양한 오픈 소스 애플리케이션이 포함됩니다.</li></ul>|
{: caption="공용 및 개인용 이미지 레지스트리 옵션" caption-side="top"}

이미지 레지스트리를 설정한 후에 클러스터 사용자는 클러스터에 자신의 앱 배치를 위한 이미지를 사용할 수 있습니다.

컨테이너 이미지에 대해 작업하는 경우 [개인 정보 보호](/docs/containers?topic=containers-security#pi)에 대해 자세히 알아보십시오.

<br />


## 컨테이너 이미지를 위한 신뢰할 수 있는 컨텐츠 설정
{: #trusted_images}

서명되어 {{site.data.keyword.registryshort_notm}}에 저장된 신뢰할 수 있는 이미지로부터 컨테이너를 빌드하고, 서명되지 않거나 취약한 이미지를 사용한 배치를 방지할 수 있습니다.
{:shortdesc}

1.  [이미지를 신뢰할 수 있는 컨텐츠로 서명](/docs/services/Registry?topic=registry-registry_trustedcontent#registry_trustedcontent)하십시오. 이미지에 대한 신뢰를 설정한 후에는 신뢰할 수 있는 컨텐츠와 레지스트리에 이미지를 푸시할 수 있는 서명자를 관리할 수 있습니다.
2.  클러스터 내에서 이미지를 빌드하는 데 서명된 이미지만 사용할 수 있도록 하는 정책을 적용하려면 [컨테이너 이미지 보안 적용(베타)을 추가](/docs/services/Registry?topic=registry-security_enforce#security_enforce)하십시오.
3.  앱을 배치하십시오.
    1. [`default` Kubernetes 네임스페이스에 배치](#namespace)하십시오.
    2. [다른 Kubernetes 네임스페이스에 배치하거나, 다른 {{site.data.keyword.Bluemix_notm}} 지역 또는 계정으로부터 배치](#other)하십시오.

<br />


## {{site.data.keyword.registryshort_notm}} 이미지에서 `default` Kubernetes 네임스페이스로 컨테이너 배치
{: #namespace}

{{site.data.keyword.registryshort_notm}} 네임스페이스에 저장된 개인용 이미지나 IBM 제공 공용 이미지에서 클러스터로 컨테이너를 배치할 수 있습니다. 클러스터가 레지스트리 이미지에 액세스하는 방법에 대한 자세한 정보는 [{{site.data.keyword.registrylong_notm}}에서 이미지를 가져올 수 있는 권한을 클러스터에 부여하는 방법 이해](#cluster_registry_auth)를 참조하십시오.
{:shortdesc}

시작하기 전에:
1. [{{site.data.keyword.registryshort_notm}}에 네임스페이스를 설정하고 이 네임스페이스에 이미지를 푸시](/docs/services/Registry?topic=registry-getting-started#gs_registry_namespace_add)하십시오.
2. [클러스터를 작성](/docs/containers?topic=containers-clusters#clusters_ui)하십시오.
3. **2019년 2월 25일** 이전에 작성된 기존 클러스터를 사용하는 경우, [클러스터를 업데이트하여 API 키 `imagePullSecret`을 사용](#imagePullSecret_migrate_api_key)하십시오.
4. [계정에 로그인하십시오. 해당되는 경우, 적절한 리소스 그룹을 대상으로 지정하십시오. 클러스터의 컨텍스트를 설정하십시오.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

클러스터의 **기본** 네임스페이스에 컨테이너를 배치하려면 다음을 수행하십시오.

1.  `mydeployment.yaml`이라는 이름의 배치 구성 파일을 작성하십시오.
2.  {{site.data.keyword.registryshort_notm}}의 네임스페이스에서 사용할 배치 및 이미지를 정의하십시오.

    ```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: <app_name>-deployment
    spec:
      replicas: 3
      selector:
        matchLabels:
          app: <app_name>
      template:
        metadata:
          labels:
            app: <app_name>
        spec:
          containers:
          - name: <app_name>
            image: <region>.icr.io/<namespace>/<my_image>:<tag>
    ```
    {: codeblock}

    이미지 URL 변수를 사용자의 이미지 정보로 바꾸십시오.
    *  **`<app_name>`**: 앱의 이름입니다.
    *  **`<region>`**: 레지스트리 도메인의 지역 {{site.data.keyword.registryshort_notm}} API 엔드포인트입니다. 로그인한 지역의 도메인 목록을 나열하려면 `ibmcloud cr api`를 실행하십시오.
    *  **`<namespace>`**: 레지스트리 네임스페이스입니다. 네임스페이스 정보를 가져오려면 `ibmcloud cr namespace-list`를 실행하십시오.
    *  **`<my_image>:<tag>`**: 컨테이너를 빌드하는 데 사용할 이미지 및 태그입니다. 레지스트리에서 사용할 수 있는 이미지를 가져오려면 `ibmcloud cr images`를 실행하십시오.

3.  클러스터에 배치를 작성하십시오.

    ```
    kubectl apply -f mydeployment.yaml
    ```
    {: pre}

<br />


## 레지스트리에서 이미지를 가져오도록 클러스터에 대한 권한 부여 방법 이해
{: #cluster_registry_auth}

레지스트리에서 이미지를 가져오기 위해 {{site.data.keyword.containerlong_notm}} 클러스터는 Kubernetes 시크릿의 특수 유형인 `imagePullSecret`을 사용합니다. 이 이미지 풀 시크릿은 컨테이너 레지스트리에 액세스하도록 인증 정보를 저장합니다. 컨테이너 레지스트리는 {{site.data.keyword.registrylong_notm}}의 네임스페이스, 다른 {{site.data.keyword.Bluemix_notm}} 계정 또는 Docker와 같은 기타 사설 레지스트리에 속하는 {{site.data.keyword.registrylong_notm}}의 네임스페이스일 수 있습니다. 클러스터는 {{site.data.keyword.registrylong_notm}}의 네임스페이스에서 이미지를 가져오고 이 이미지의 컨테이너를 클러스터의 `default` Kubernetes 네임스페이스에 배치하도록 설정됩니다. 기타 클러스터 Kubernetes 네임스페이스 또는 기타 레지스트리의 이미지를 가져와야 하는 경우 이미지 풀 시크릿을 설정해야 합니다.
{:shortdesc}

**`default` Kubernetes 네임스페이스에서 이미지를 가져오려면 내 클러스터를 어떻게 설정해야 합니까?**<br>
클러스터를 작성하면 해당 클러스터에는 {{site.data.keyword.registrylong_notm}}에 대한 IAM **독자** 서비스 액세스 역할 정책이 제공된 {{site.data.keyword.Bluemix_notm}} IAM 서비스 ID가 있습니다. 서비스 ID 인증 정보는 클러스터의 이미지 풀 시크릿에 저장된 비만료 API 키에서 위장됩니다. 이미지 풀 시크릿은 `default` Kubernetes 네임스페이스와 이 네임스페이스의 `default` 서비스 계정에 있는 시크릿 목록에 추가됩니다. 이미지 풀 시크릿을 사용하여 배치에서 [글로벌 및 지역 레지스트리](/docs/services/Registry?topic=registry-registry_overview#registry_regions)의 이미지를 가져와(읽기 전용 액세스) `default` Kubernetes 네임스페이스에 컨테이너를 빌드할 수 있습니다. 글로벌 레지스트리는 개별 지역 레지스트리에 저장된 이미지에 대한 서로 다른 참조를 가지는 대신 배치 전반에서 참조할 수 있는 공용 IBM 제공 이미지를 저장합니다. 지역 레지스트리는 개인용 Docker 이미지를 안전하게 저장합니다.

**특정 지역 레지스트리에 대한 가져오기 액세스를 제한할 수 있습니까?**<br>
예, 네임스페이스와 같은 지역 레지스트리 또는 레지스트리 리소스에 대한 **독자** 서비스 액세스 역할을 제한하는 [기존 서비스 ID의 IAM 정책을 편집](/docs/iam?topic=iam-serviceidpolicy#access_edit)할 수 있습니다. 레지스트리 IAM 정책을 사용자 정의하기 전에 [{{site.data.keyword.registrylong_notm}}에 대한 {{site.data.keyword.Bluemix_notm}} IAM 정책을 사용으로 설정해야 합니다](/docs/services/Registry?topic=registry-user#existing_users).

  레지스트리 인증 정보를 더 안전하게 보호하려 하십니까? 클러스터에서 레지스트리 인증 정보를 저장하는 `imagePullSecret`과 같은 Kubernetes secret을 암호화하기 위해 클러스터 관리자에게 [{{site.data.keyword.keymanagementservicefull}}를 사용으로 설정](/docs/containers?topic=containers-encryption#keyprotect)하도록 요청하십시오.
  {: tip}

**`default`가 아닌 Kubernetes 네임스페이스에서 이미지를 가져올 수 있습니까?**<br>
기본적으로는 그렇지 않습니다. 기본 클러스터 설정을 사용하여 {{site.data.keyword.registrylong_notm}} 네임스페이스에 저장된 이미지의 컨테이너를 클러스터의 `default` Kubernetes 네임스페이스에 배치할 수 있습니다. 다른 Kubernetes 네임스페이스 또는 다른 {{site.data.keyword.Bluemix_notm}} 계정에서 이 이미지를 사용하기 위한 [사용자 고유의 이미지 풀 시크릿을 복사 또는 작성하는 옵션](#other)이 있습니다.

**다른 {{site.data.keyword.Bluemix_notm}} 계정에서 이미지를 가져올 수 있습니까?**<br>
예. 사용할 {{site.data.keyword.Bluemix_notm}} 계정에서 API 키를 작성하십시오. 그런 다음 가져올 각 클러스터 및 클러스터 네임스페이스에서 해당 API 키 인증 정보를 저장하는 이미지 풀 시크릿을 작성하십시오. [권한 부여된 서비스 ID API 키를 사용하는 이 예를 따라 진행](#other_registry_accounts)하십시오.

Docker와 같은 비{{site.data.keyword.Bluemix_notm}} 레지스트리를 사용하려면 [기타 개인용 레지스트리에 저장된 이미지에 액세스](#private_images)를 참조하십시오.

**서비스 ID를 위해 API 키가 필요합니까? 내 계정에 대한 서비스 ID의 한계에 도달하면 어떻게 됩니까?**<br>
기본 클러스터 설정은 서비스 ID를 작성하여 이미지 풀 시크릿에 {{site.data.keyword.Bluemix_notm}} IAM API 키 인증 정보를 저장합니다. 그러나 개별 사용자에 대한 API 키를 작성하고 이미지 풀 시크릿에 해당 인증 정보를 저장할 수도 있습니다. [서비스 ID에 대한 IAM 한계](/docs/iam?topic=iam-iam_limits#iam_limits)에 도달하는 경우 기본적으로 클러스터가 서비스 ID 및 이미지 풀 시크릿 없이 작성되고 `icr.io` 레지스트리 도메인에서 이미지를 가져올 수 없습니다. {{site.data.keyword.Bluemix_notm}} IAM 서비스 ID가 아닌 기능 ID와 같은 개별 사용자에 대한 API 키를 사용하여 [고유의 이미지 풀 시크릿을 작성](#other_registry_accounts)해야 합니다.

**내 클러스터 이미지 풀 시크릿은 레지스트리 토큰을 사용합니다. 토큰이 계속해서 작동합니까?**<br>

이전에 [토큰](/docs/services/Registry?topic=registry-registry_access#registry_tokens)을 자동으로 작성하고 이미지 풀 시크릿에 토큰을 저장하여 {{site.data.keyword.registrylong_notm}}에 대한 클러스터 액세스 권한을 부여하는 방법은 지원되지만 더 이상 사용되지 않습니다.
{: deprecated}

토큰은 더 이상 사용되지 않는 `registry.bluemix.net` 레지스트리 도메인에 액세스할 권한을 부여하지만 API 키는 `icr.io` 레지스트리 도메인에 액세스할 권한을 부여합니다. 토큰에서 API 키 기반 인증까지의 전이 기간 중에 일시적으로 토큰 및 API 키 기반 이미지 풀 시크릿이 모두 작성됩니다. 토큰 및 API 키 기반 이미지 풀 시크릿을 모두 사용하여 클러스터는 `default` Kubernetes 네임스페이스의 `registry.bluemix.net` 또는 `icr.io` 도메인에서 이미지를 가져올 수 있습니다.

더 이상 사용되지 않는 토큰 및 `registry.bluemix.net` 도메인에 대한 지원이 중단되기 전에, [`default` Kubernetes 네임스페이스](#imagePullSecret_migrate_api_key) 및 사용할 수 있는 [기타 네임스페이스 또는 계정](#other)에 대한 API 키 메소드를 사용하도록 클러스터 이미지 풀 시크릿을 업데이트하십시오. 그런 다음 `icr.io` 레지스트리 도메인에서 가져올 배치를 업데이트하십시오.

**다른 Kubernetes 네임스페이스에서 이미지 풀 시크릿을 복사하거나 작성하기만 하면 다른 작업은 필요하지 않습니까?**<br>
아닙니다. 작성한 시크릿을 사용하여 이미지를 가져올 수 있도록 컨테이너에 권한이 부여되어야 합니다. 사용자는 네임스페이스의 서비스 계정에 이미지 풀 시크릿을 추가하거나, 각 배치에서 해당 시크릿을 참조할 수 있습니다. 지시사항은 [이미지 풀 시크릿을 사용하여 컨테이너 배치](/docs/containers?topic=containers-images#use_imagePullSecret)를 참조하십시오. 

<br />


## 기존 클러스터를 업데이트하여 API 키 이미지 풀 시크릿 사용
{: #imagePullSecret_migrate_api_key}

새 {{site.data.keyword.containerlong_notm}} 클러스터는 [{{site.data.keyword.registrylong_notm}}에 액세스할 권한을 부여하기 위해 이미지 풀 시크릿](#cluster_registry_auth)에 API 키를 저장합니다. 이러한 이미지 풀 시크릿을 사용하여 `icr.io` 레지스트리 도메인에 저장된 이미지에서 컨테이너를 배치할 수 있습니다. **2019년 2월 25일**전에 작성된 클러스터의 경우, 이미지 풀 시크릿에 레지스트리 토큰 대신 API 키를 저장하도록 클러스터를 업데이트해야 합니다.
{: shortdesc}

**시작하기 전에**:
*   [계정에 로그인하십시오. 해당되는 경우, 적절한 리소스 그룹을 대상으로 지정하십시오. 클러스터의 컨텍스트를 설정하십시오.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
*   다음 권한이 있는지 확인하십시오.
    *   {{site.data.keyword.containerlong_notm}}에 대한 {{site.data.keyword.Bluemix_notm}} IAM **운영자 또는 관리자** 플랫폼 역할. 계정 소유자는 다음을 실행하여 역할을 제공할 수 있습니다.
        ```
        ibmcloud iam user-policy-create <your_user_email> --service-name containers-kubernetes --roles Administrator,Operator
        ```
        {: pre}
    *   모든 지역 및 리소스 그룹에서 {{site.data.keyword.registrylong_notm}}에 대한 {{site.data.keyword.Bluemix_notm}} IAM **관리자** 플랫폼 역할. 계정 소유자는 다음을 실행하여 역할을 제공할 수 있습니다.
        ```
        ibmcloud iam user-policy-create <your_user_email> --service-name container-registry --roles Administrator
        ```
        {: pre}

**`default` Kubernetes 네임스페이스에서 클러스터 이미지 풀 시크릿을 업데이트하려면 다음을 수행하십시오.**
1.  클러스터 ID를 가져오십시오.
    ```
    ibmcloud ks clusters
    ```
    {: pre}
2.  다음 명령을 실행하여 클러스터에 대한 서비스 ID를 작성하고, 서비스 ID에 {{site.data.keyword.registrylong_notm}}에 대한 새 **독자** 서비스 역할을 지정하고, 서비스 ID 인증 정보를 위장하기 위한 API 키를 작성하고, 클러스터에 있는 Kubernetes 이미지 풀 시크릿에 API 키를 저장하십시오. 이미지 풀 시크릿은 `default` Kubernetes 네임스페이스에 있습니다.
    ```
    ibmcloud ks cluster-pull-secret-apply --cluster <cluster_name_or_ID>
    ```
    {: pre}

    이 명령을 실행하면 IAM 인증 정보 및 이미지 풀 시크릿의 작성이 초기화되며 완료하는 데 약간의 시간이 소요될 수 있습니다. 이미지 풀 시크릿이 작성될 때까지 {{site.data.keyword.registrylong_notm}} `icr.io` 도메인에서 이미지를 가져오는 컨테이너를 배치할 수 없습니다.
    {: important}

3.  이미지 풀 시크릿이 클러스터에 작성되었는지 확인하십시오. 각 {{site.data.keyword.registrylong_notm}} 지역에 대해 별도의 이미지 풀 시크릿이 있습니다.
    ```
    kubectl get secrets
    ```
    {: pre}
    출력 예:
    ```
    default-us-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-uk-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-de-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-au-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-jp-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-icr-io                             kubernetes.io/dockerconfigjson        1         16d
    ```
    {: screen}
4.  컨테이너 배치를 업데이트하여 `icr.io` 도메인 이름에서 이미지를 가져오십시오.
5.  선택사항: 방화벽이 있는 경우 사용하는 도메인의 [레지스트리 서브넷에 아웃바운드 네트워크 트래픽을 허용](/docs/containers?topic=containers-firewall#firewall_outbound)했는지 확인하십시오.

**다음에 수행할 작업**
*   `default`가 아닌 Kubernetes 네임스페이스 또는 다른 {{site.data.keyword.Bluemix_notm}} 계정에서 이미지를 가져오려면 [다른 이미지 풀 시크릿을 복사 또는 작성](/docs/containers?topic=containers-images#other)하십시오.
*   네임스페이스 또는 지역과 같은 특정 레지스트리 리소스에 대한 이미지 풀 시크릿 액세스를 제한하려면 다음을 수행하십시오.
    1.  [{{site.data.keyword.registrylong_notm}}에 대한 {{site.data.keyword.Bluemix_notm}} IAM 정책이 사용으로 설정](/docs/services/Registry?topic=registry-user#existing_users)되어 있는지 확인하십시오.
    2.  [서비스 ID에 대한 {{site.data.keyword.Bluemix_notm}} IAM 정책을 편집](/docs/iam?topic=iam-serviceidpolicy#access_edit)하거나 [다른 이미지 풀 시크릿을 작성](/docs/containers?topic=containers-images#other_registry_accounts)하십시오.

<br />


## 이미지 풀 시크릿을 사용하여 다른 클러스터 Kubernetes 네임스페이스, 다른 {{site.data.keyword.Bluemix_notm}} 계정 또는 외부 개인용 레지스트리에 액세스
{: #other}

클러스터에서 사용자의 이미지 풀 시크릿을 설정하여 `default` 이외의 Kubernetes 네임스페이스에 컨테이너를 배치하거나, 다른 {{site.data.keyword.Bluemix_notm}} 계정에 저장된 이미지를 사용하거나, 외부 개인용 레지스트리에 저장된 이미지를 사용하십시오. 또한, 특정 레지스트리 이미지 네임스페이스 또는 조치(예: `push` 또는 `pull`)에 대한 권한을 제한하는 IAM 액세스 정책을 적용할 자신의 고유 이미지 풀 시크릿을 작성할 수도 있습니다.
{:shortdesc}

이미지 풀 시크릿을 작성한 후에는 컨테이너가 해당 시크릿을 사용하여 레지스트리에서 이미지를 가져올 수 있도록 권한을 부여받아야 합니다. 사용자는 네임스페이스의 서비스 계정에 이미지 풀 시크릿을 추가하거나, 각 배치에서 해당 시크릿을 참조할 수 있습니다. 지시사항은 [이미지 풀 시크릿을 사용하여 컨테이너 배치](/docs/containers?topic=containers-images#use_imagePullSecret)를 참조하십시오. 

이미지 풀 시크릿은 사용하도록 지정된 Kubernetes 네임스페이스에만 유효합니다. 컨테이너를 배치하려는 모든 네임스페이스에 대해 이러한 단계를 반복하십시오. [DockerHub](#dockerhub)의 이미지에는 이미지 풀 시크릿이 필요하지 않습니다.
{: tip}

시작하기 전에:

1.  [{{site.data.keyword.registryshort_notm}}에 네임스페이스를 설정하고 이 네임스페이스에 이미지를 푸시](/docs/services/Registry?topic=registry-getting-started#gs_registry_namespace_add)하십시오.
2.  [클러스터를 작성](/docs/containers?topic=containers-clusters#clusters_ui)하십시오.
3.  **2019년 2월 25일** 이전에 작성된 기존 클러스터를 사용하는 경우, [클러스터를 업데이트하여 API 키 이미지 풀 시크릿을 사용](#imagePullSecret_migrate_api_key)하십시오.
4.  [계정에 로그인하십시오. 해당되는 경우, 적절한 리소스 그룹을 대상으로 지정하십시오. 클러스터의 컨텍스트를 설정하십시오.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

<br/>
사용자 고유의 이미지 풀 시크릿을 사용하려면 다음 옵션 중에서 선택하십시오.
- 기본 Kubernetes 네임스페이스에서 클러스터 내의 다른 네임스페이스로 [이미지 풀 시크릿을 복사](#copy_imagePullSecret)합니다.
- [새 IAM API 키 인증 정보를 작성하고 이미지 풀 시크릿에 저장](#other_registry_accounts)하여 다른 {{site.data.keyword.Bluemix_notm}} 계정의 이미지에 액세스하거나 특정 레지스트리 도메인 또는 네임스페이스에 대한 액세스를 제한하는 IAM 정책을 적용합니다.
- [외부 개인용 레지스트리의 이미지에 액세스하는 이미지 풀 시크릿을 작성](#private_images)합니다.

<br/>
배치에 사용할 이미지 풀 시크릿을 네임스페이스에 이미 작성한 경우에는 [작성된 `imagePullSecret`을 사용하여 컨테이너 배치](#use_imagePullSecret)를 참조하십시오.

### 기존 이미지 풀 시크릿 복사
{: #copy_imagePullSecret}

`default` Kubernetes 네임스페이스에 대해 자동으로 작성된 것과 같은 이미지 풀 시크릿을 클러스터 내의 다른 네임스페이스로 복사할 수 있습니다. 예를 들어, 특정 네임스페이스에 대한 액세스를 제한하거나 다른 {{site.data.keyword.Bluemix_notm}} 계정에서 이미지를 가져오기 위해 이 네임스페이스에 대해 다른 {{site.data.keyword.Bluemix_notm}} IAM API 키 인증 정보를 사용하려면 대신 [이미지 풀 시크릿을 작성](#other_registry_accounts)하십시오.
{: shortdesc}

1.  클러스터에서 사용 가능한 Kubernetes 네임스페이스를 나열하거나 사용할 네임스페이스를 작성하십시오.
    ```
    kubectl get namespaces
    ```
    {: pre}

    출력 예:
    ```
    default          Active    79d
    ibm-cert-store   Active    79d
    ibm-system       Active    79d
    istio-system     Active    34d
    kube-public      Active    79d
    kube-system      Active    79d
    ```
    {: screen}

    네임스페이스를 작성하려면 다음을 수행하십시오.
    ```
    kubectl create namespace <namespace_name>
    ```
    {: pre}
2.  {{site.data.keyword.registrylong_notm}}의 `default` Kubernetes 네임스페이스에 있는 기존 이미지 풀 시크릿을 나열하십시오.
    ```
    kubectl get secrets -n default | grep icr
    ```
    {: pre}
    출력 예:
    ```
    default-us-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-uk-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-de-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-au-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-jp-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-icr-io                             kubernetes.io/dockerconfigjson        1         16d
    ```
    {: screen}
3.  `default` 네임스페이스의 이미지 풀 시크릿을 원하는 네임스페이스로 복사하십시오. 새 이미지 풀 시크릿의 이름은 `<namespace_name>-icr-<region>-io`입니다.
    ```
    kubectl get secret default-us-icr-io -o yaml | sed 's/default/<new-namespace>/g' | kubectl -n <new-namespace> create -f -
    ```
    {: pre}
    ```
    kubectl get secret default-uk-icr-io -o yaml | sed 's/default/<new-namespace>/g' | kubectl -n <new-namespace> create -f -
    ```
    {: pre}
    ```
    kubectl get secret default-de-icr-io -o yaml | sed 's/default/<new-namespace>/g' | kubectl -n <new-namespace> create -f -
    ```
    {: pre}
    ```
    kubectl get secret default-au-icr-io -o yaml | sed 's/default/<new-namespace>/g' | kubectl -n <new-namespace> create -f -
    ```
    {: pre}
    ```
    kubectl get secret default-jp-icr-io -o yaml | sed 's/default/<new-namespace>/g' | kubectl -n <new-namespace> create -f -
    ```
    {: pre}
    ```
    kubectl get secret default-icr-io -o yaml | sed 's/default/<new-namespace>/g' | kubectl -n <new-namespace> create -f -
    ```
    {: pre}
4.  시크릿이 작성되었는지 확인하십시오.
    ```
    kubectl get secrets -n <namespace_name>
    ```
    {: pre}
5.  [컨테이너를 배치할 때 네임스페이스의 모든 팟(Pod)에서 이미지 풀 시크릿을 사용할 수 있도록 Kubernetes 서비스 계정에 이미지 풀 시크릿을 추가](#use_imagePullSecret)하십시오. 

### 다른 {{site.data.keyword.Bluemix_notm}} 계정에서 이미지에 대한 추가 제어 또는 액세스를 위해 다른 IAM API 키 인증 정보를 사용하여 이미지 풀 시크릿 작성
{: #other_registry_accounts}

{{site.data.keyword.Bluemix_notm}} IAM 액세스 정책을 사용자 또는 서비스 ID에 지정하여 특정 레지스트리 이미지 네임스페이스 또는 조치(예: `push` 또는 `pull`)에 대한 권한을 제한할 수 있습니다. 그런 다음 API 키를 작성하고 사용자 클러스터에 적합한 이미지 풀 시크릿에 이 레지스트리 인증 정보를 저장하십시오.
{: shortdesc}

예를 들어, 다른 {{site.data.keyword.Bluemix_notm}} 계정의 이미지에 액세스하려면 사용자 또는 서비스 ID의 {{site.data.keyword.registryshort_notm}} 인증 정보를 해당 계정에 저장하는 API 키를 작성하십시오. 그런 다음 클러스터의 계정에서 각 클러스터 및 클러스터 네임스페이스에 대한 이미지 풀 시크릿에 API 키 인증 정보를 저장하십시오.

다음 단계에서는 {{site.data.keyword.Bluemix_notm}} IAM 서비스 ID의 인증 정보를 저장하는 API 키를 작성합니다. 서비스 ID를 사용하는 대신 {{site.data.keyword.Bluemix_notm}} {{site.data.keyword.registryshort_notm}}에 대한 IAM 서비스 액세스 정책이 있는 사용자 ID에 대한 API 키를 작성하려고 할 수 있습니다. 그러나 사용자가 기능 ID이거나 클러스터가 레지스트리에 계속 액세스할 수 있도록 사용자가 퇴사한 경우 계획이 있는지 확인하십시오.
{: note}

1.  클러스터에서 사용 가능한 Kubernetes 네임스페이스를 나열하거나 레지스트리 이미지에서 컨테이너를 배치할 위치에서 사용할 네임스페이스를 작성하십시오.
    ```
    kubectl get namespaces
    ```
    {: pre}

    출력 예:
    ```
    default          Active    79d
    ibm-cert-store   Active    79d
    ibm-system       Active    79d
    istio-system     Active    34d
    kube-public      Active    79d
    kube-system      Active    79d
    ```
    {: screen}

    네임스페이스를 작성하려면 다음을 수행하십시오.
    ```
    kubectl create namespace <namespace_name>
    ```
    {: pre}
2.  이미지 풀 시크릿에서 IAM 정책과 API 키 인증 정보에 사용되는 클러스터에 대한 {{site.data.keyword.Bluemix_notm}} IAM 서비스 ID를 작성하십시오. 서비스 ID에 나중에 검색하는 데 도움이 되는 설명(예: 클러스터 및 네임스페이스 이름 포함)을 제공했는지 확인하십시오.
    ```
    ibmcloud iam service-id-create <cluster_name>-<kube_namespace>-id --description "Service ID for IBM Cloud Container Registry in Kubernetes cluster <cluster_name> namespace <kube_namespace>"
    ```
    {: pre}
3.  {{site.data.keyword.registryshort_notm}}에 액세스 권한을 부여하는 클러스터 서비스 ID에 대한 사용자 정의 {{site.data.keyword.Bluemix_notm}} IAM 정책을 작성하십시오.
    ```
    ibmcloud iam service-policy-create <cluster_service_ID> --roles <service_access_role> --service-name container-registry [--region <IAM_region>] [--resource-type namespace --resource <registry_namespace>]
    ```
    {: pre}
    <table>
    <caption>이 명령의 컴포넌트 이해</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 이 명령의 컴포넌트 이해</th>
    </thead>
    <tbody>
    <tr>
    <td><code><em>&lt;cluster_service_ID&gt;</em></code></td>
    <td>필수. Kubernetes 클러스터에 대해 이전에 작성한 `<cluster_name>-<kube_namespace>-id` 서비스 ID로 대체합니다.</td>
    </tr>
    <tr>
    <td><code>--service-name <em>container-registry</em></code></td>
    <td>필수. IAM 정책 대상이 {{site.data.keyword.registrylong_notm}}가 되도록 `container-registry`를 입력합니다.</td>
    </tr>
    <tr>
    <td><code>--roles <em>&lt;service_access_role&gt;</em></code></td>
    <td>필수. 서비스 ID 액세스를 범위 지정할 [{{site.data.keyword.registrylong_notm}}에 대한 서비스 액세스](/docs/services/Registry?topic=registry-iam#service_access_roles)를 입력합니다. 가능한 값은 `Reader`, `Writer` 및 `Manager`입니다.</td>
    </tr>
    <tr>
    <td><code>--region <em>&lt;IAM_region&gt;</em></code></td>
    <td>선택사항. 액세스 정책을 특정 IAM 지역으로 범위 지정하려면 쉼표로 구분된 목록으로 지역을 입력합니다. 가능한 값은 `au-syd`, `eu-gb`, `eu-de`, `jp-tok`, `us-south` 및 `global`입니다.</td>
    </tr>
    <tr>
    <td><code>--resource-type <em>namespace</em> --resource <em>&lt;registry_namespace&gt;</em></code></td>
    <td>선택사항. 특정 [{{site.data.keyword.registrylong_notm}} 네임스페이스](/docs/services/Registry?topic=registry-registry_overview#registry_namespaces)의 이미지로만 액세스를 제한하려면 자원 유형에 대한 `namespace`를 입력하고 `<registry_namespace>`를 지정하십시오. 레지스트리 네임스페이스를 나열하려면 `ibmcloud cr namespaces`를 실행하십시오.</td>
    </tr>
    </tbody></table>
4.  서비스 ID에 대한 API 키를 작성하십시오. 서비스 키와 유사한 API 키의 이름을 지정하고 이전에 작성한 서비스 ID, ``<cluster_name>-<kube_namespace>-id`를 포함하십시오. 나중에 키를 검색하는 데 도움이 되는 설명을 API 키에 제공해야 합니다.
    ```
    ibmcloud iam service-api-key-create <cluster_name>-<kube_namespace>-key <cluster_name>-<kube_namespace>-id --description "API key for service ID <service_id> in Kubernetes cluster <cluster_name> namespace <kube_namespace>"
    ```
    {: pre}
5.  이전 명령 출력에서 **API 키** 값을 검색하십시오.
    ```
    Please preserve the API key! It cannot be retrieved after it's created.

    Name          <cluster_name>-<kube_namespace>-key   
    Description   key_for_registry_for_serviceid_for_kubernetes_cluster_multizone_namespace_test   
    Bound To      crn:v1:bluemix:public:iam-identity::a/1bb222bb2b33333ddd3d3333ee4ee444::serviceid:ServiceId-ff55555f-5fff-6666-g6g6-777777h7h7hh   
    Created At    2019-02-01T19:06+0000   
    API Key       i-8i88ii8jjjj9jjj99kkkkkkkkk_k9-llllll11mmm1   
    Locked        false   
    UUID          ApiKey-222nn2n2-o3o3-3o3o-4p44-oo444o44o4o4   
    ```
    {: screen}
6.  Kubernetes 이미지 풀 시크릿을 작성하여 클러스터의 네임스페이스에 API 키 인증 정보를 저장하십시오. 레지스트리에서 이 서비스 ID의 IAM 인증 정보를 사용하여 이미지를 가져오려는 각 `icr.io` 도메인, Kubernetes 네임스페이스 및 클러스터에 대해 이 단계를 반복하십시오.
    ```
    kubectl --namespace <kubernetes_namespace> create secret docker-registry <secret_name> --docker-server=<registry_URL> --docker-username=iamapikey --docker-password=<api_key_value> --docker-email=<docker_email>
    ```
    {: pre}

    <table>
    <caption>이 명령의 컴포넌트 이해</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 이 명령의 컴포넌트 이해</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace <em>&lt;kubernetes_namespace&gt;</em></code></td>
    <td>필수. 서비스 ID 이름에 사용한 클러스터의 Kubernetes 네임스페이스를 지정합니다.</td>
    </tr>
    <tr>
    <td><code><em>&lt;secret_name&gt;</em></code></td>
    <td>필수. 이미지 풀 시크릿의 이름을 입력합니다.</td>
    </tr>
    <tr>
    <td><code>--docker-server <em>&lt;registry_URL&gt;</em></code></td>
    <td>필수. URL을 레지스트리 네임스페이스가 설정된 이미지 레지스트리로 설정합니다. 사용 가능한 레지스트리 도메인:<ul>
    <li>AP 북부(도쿄): `jp.icr.io`</li>
    <li>AP 남부(시드니): `au.icr.io`</li>
    <li>중앙 유렵(프랑크푸르트): `de.icr.io`</li>
    <li>영국 남부(런던): `uk.icr.io`</li>
    <li>미국 남부(댈러스): `us.icr.io`</li></ul></td>
    </tr>
    <tr>
    <td><code>--docker-username iamapikey</code></td>
    <td>필수. 개인용 레지스트리에 로그인할 사용자 이름을 입력합니다. {{site.data.keyword.registryshort_notm}}의 경우, 사용자 이름은 <strong><code>iamapikey</code></strong> 값으로 설정됩니다.</td>
    </tr>
    <tr>
    <td><code>--docker-password <em>&lt;token_value&gt;</em></code></td>
    <td>필수. 이전에 검색한 **API 키**의 값을 입력하십시오.</td>
    </tr>
    <tr>
    <td><code>--docker-email <em>&lt;docker-email&gt;</em></code></td>
    <td>필수. Docker 이메일 주소가 있는 경우 입력하십시오. 없는 경우에는 가상의 이메일 주소(예: `a@b.c`)를 입력하십시오. 이 이메일은 Kubernetes 시크릿을 작성하는 데 필요하지만 작성 후에는 사용되지 않습니다.</td>
    </tr>
    </tbody></table>
7.  시크릿이 작성되었는지 확인하십시오. <em>&lt;kubernetes_namespace&gt;</em>를 이미지 풀 시크릿을 작성한 네임스페이스로 대체하십시오.

    ```
    kubectl get secrets --namespace <kubernetes_namespace>
    ```
    {: pre}
8.  [컨테이너를 배치할 때 네임스페이스의 모든 팟(Pod)에서 이미지 풀 시크릿을 사용할 수 있도록 Kubernetes 서비스 계정에 이미지 풀 시크릿을 추가](#use_imagePullSecret)하십시오. 

### 다른 개인용 레지스트리에 저장된 이미지에 액세스
{: #private_images}

개인용 레지스트리가 이미 있는 경우 레지스트리 인증 정보를 Kubernetes 이미지 풀 시크릿에 저장하고 구성 파일에서 이 시크릿을 참조해야 합니다.
{:shortdesc}

시작하기 전에:

1.  [클러스터를 작성](/docs/containers?topic=containers-clusters#clusters_ui)하십시오.
2.  [클러스터에 CLI를 대상으로 지정](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)하십시오.

이미지 풀 시크릿을 작성하려면 다음을 수행하십시오.

1.  개인용 레지스트리 인증 정보를 저장하기 위한 Kubernetes 시크릿을 작성하십시오.

    ```
    kubectl --namespace <kubernetes_namespace> create secret docker-registry <secret_name>  --docker-server=<registry_URL> --docker-username=<docker_username> --docker-password=<docker_password> --docker-email=<docker_email>
    ```
    {: pre}

    <table>
    <caption>이 명령의 컴포넌트 이해</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 이 명령의 컴포넌트 이해</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace <em>&lt;kubernetes_namespace&gt;</em></code></td>
    <td>필수. 시크릿을 사용하고 컨테이너를 배치하려는 클러스터의 Kubernetes 네임스페이스입니다. 클러스터의 모든 네임스페이스를 나열하려면 <code>kubectl get namespaces</code>를 실행하십시오.</td>
    </tr>
    <tr>
    <td><code><em>&lt;secret_name&gt;</em></code></td>
    <td>필수. <code>imagePullSecret</code>에 사용하려는 이름입니다.</td>
    </tr>
    <tr>
    <td><code>--docker-server <em>&lt;registry_URL&gt;</em></code></td>
    <td>필수. 개인용 이미지가 저장된 레지스트리에 대한 URL입니다.</td>
    </tr>
    <tr>
    <td><code>--docker-username <em>&lt;docker_username&gt;</em></code></td>
    <td>필수. 개인용 레지스트리에 로그인하기 위한 사용자 이름입니다.</td>
    </tr>
    <tr>
    <td><code>--docker-password <em>&lt;token_value&gt;</em></code></td>
    <td>필수. 이전에 검색한 레지스트리 토큰의 값입니다.</td>
    </tr>
    <tr>
    <td><code>--docker-email <em>&lt;docker-email&gt;</em></code></td>
    <td>필수. Docker 이메일 주소가 있는 경우 입력하십시오. 없는 경우에는 가상의 이메일 주소(예: `a@b.c`)를 입력하십시오. 이 이메일은 Kubernetes 시크릿을 작성하는 데 필요하지만 작성 후에는 사용되지 않습니다.</td>
    </tr>
    </tbody></table>

2.  시크릿이 작성되었는지 확인하십시오. <em>&lt;kubernetes_namespace&gt;</em>를 `imagePullSecret`을 작성한 네임스페이스의 이름으로 대체하십시오.

    ```
    kubectl get secrets --namespace <kubernetes_namespace>
    ```
    {: pre}

3.  [이미지 풀 시크릿을 참조하는 팟(Pod)을 작성](#use_imagePullSecret)하십시오.

<br />


## 이미지 풀 시크릿을 사용하여 컨테이너 배치
{: #use_imagePullSecret}

팟(Pod) 배치에서 이미지 풀 시크릿을 정의하거나, 서비스 계정을 지정하지 않은 모든 배치에서 이미지 풀 시크릿을 사용할 수 있도록 Kubernetes 서비스 계정에 이를 저장할 수 있습니다.
{: shortdesc}

다음 선택사항 중 하나를 선택하십시오.
* [팟(Pod) 배치의 이미지 풀 시크릿 참조](#pod_imagePullSecret): 기본적으로 네임스페이스 내 모든 팟(Pod)에 대한 레지스트리에 대한 액세스 권한을 부여하지 않으려는 경우에는 이 옵션을 사용하십시오.
* [Kubernetes 서비스 계정에 이미지 풀 시크릿 저장](#store_imagePullSecret): 선택한 Kubernetes 네임스페이스 내 모든 배치에 대한 레지스트리 내 이미지에 대한 액세스 권한을 부여하려면 이 옵션을 사용하십시오.

시작하기 전에:
* `default` 이외의 다른 레지스트리 또는 Kubernetes 네임스페이스에 액세스하는 [이미지 풀 시크릿을 작성](#other)하십시오.
* [클러스터에 CLI를 대상으로 지정](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)하십시오.

### 팟(Pod) 배치의 이미지 풀 시크릿 참조
{: #pod_imagePullSecret}

팟(Pod) 배치의 이미지 풀 시크릿을 참조하는 경우 이미지 풀 시크릿은 이 팟(Pod)에만 유효하며 네임스페이스 내의 다른 팟과 공유할 수 없습니다.
{:shortdesc}

1.  `mypod.yaml`이라는 이름의 팟(Pod) 구성 파일을 작성하십시오.
2.  {{site.data.keyword.registrylong_notm}}의 이미지에 액세스하기 위한 팟(Pod) 및 이미지 풀 시크릿을 정의하십시오.

    개인용 이미지에 액세스하려면 다음과 같이 정의하십시오.
    ```
    apiVersion: v1
    kind: Pod
    metadata:
      name: mypod
    spec:
      containers:
        - name: <container_name>
          image: <region>.icr.io/<namespace_name>/<image_name>:<tag>
      imagePullSecrets:
        - name: <secret_name>
    ```
    {: codeblock}

    {{site.data.keyword.Bluemix_notm}} 공용 이미지에 액세스하려면 다음과 같이 정의하십시오.
    ```
    apiVersion: v1
    kind: Pod
    metadata:
      name: mypod
    spec:
      containers:
        - name: <container_name>
          image: icr.io/<image_name>:<tag>
      imagePullSecrets:
        - name: <secret_name>
    ```
    {: codeblock}

    <table>
    <caption>YAML 파일 컴포넌트 이해</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> YAML 파일 컴포넌트 이해</th>
    </thead>
    <tbody>
    <tr>
    <td><code><em>&lt;container_name&gt;</em></code></td>
    <td>클러스터에 배치할 컨테이너의 이름입니다.</td>
    </tr>
    <tr>
    <td><code><em>&lt;namespace_name&gt;</em></code></td>
    <td>이미지가 저장된 네임스페이스입니다. 사용 가능한 네임스페이스를 나열하려면 `ibmcloud cr namespace-list`를 실행하십시오.</td>
    </tr>
    <tr>
    <td><code><em>&lt;image_name&gt;</em></code></td>
    <td>사용할 이미지의 이름입니다. {{site.data.keyword.Bluemix_notm}} 계정에서 사용 가능한 이미지를 나열하려면 `ibmcloud cr image-list`를 실행하십시오.</td>
    </tr>
    <tr>
    <td><code><em>&lt;tag&gt;</em></code></td>
    <td>사용하려는 이미지의 버전입니다. 태그가 지정되지 않은 경우, 기본적으로 <strong>latest</strong>로 태그가 지정된 이미지가 사용됩니다.</td>
    </tr>
    <tr>
    <td><code><em>&lt;secret_name&gt;</em></code></td>
    <td>이전에 작성한 이미지 풀 시크릿의 이름입니다.</td>
    </tr>
    </tbody></table>

3.  변경사항을 저장하십시오.
4.  클러스터에 배치를 작성하십시오.
    ```
    kubectl apply -f mypod.yaml
    ```
    {: pre}

### 선택한 네임스페이스의 Kubernetes 서비스 계정에 이미지 풀 시크릿 저장
{:#store_imagePullSecret}

모든 네임스페이스에는 `default`라는 Kubernetes 서비스 계정이 있습니다. 이 서비스 계정에 이미지 풀 시크릿을 추가하여 레지스트리의 이미지에 대한 액세스 권한을 부여할 수 있습니다. 서비스 계정을 지정하지 않는 배치는 자동으로 이 네임스페이스의 `default` 서비스 계정을 사용합니다.
{:shortdesc}

1. default 서비스 계정에 대해 이미지 풀 시크릿이 이미 존재하고 있지 않은지 확인하십시오.
   ```
   kubectl describe serviceaccount default -n <namespace_name>
   ```
   {: pre}
   **이미지 풀 시크릿** 항목에 `<none>`이 표시되면 이미지 풀 시크릿이 존재하지 않는 것입니다.  
2. default 서비스 계정에 이미지 풀 시크릿을 추가하십시오.
   - **이미지 풀 시크릿이 정의되어 있지 않은 경우 이미지 풀 시크릿을 추가하려면 다음을 수행하십시오.**
       ```
       kubectl patch -n <namespace_name> serviceaccount/default -p '{"imagePullSecrets":[{"name": "<image_pull_secret_name>"}]}'
       ```
       {: pre}
   - **이미지 풀 시크릿이 이미 정의되어 있는 경우 이미지 풀 시크릿을 추가하려면 다음을 수행하십시오.**
       ```
       kubectl patch -n <namespace_name> serviceaccount/default --type='json' -p='[{"op":"add","path":"/imagePullSecrets/-","value":{"name":"<image_pull_secret_name>"}}]'
       ```
       {: pre}
3. 이미지 풀 시크릿이 default 서비스 계정에 추가되었는지 확인하십시오.
   ```
   kubectl describe serviceaccount default -n <namespace_name>
   ```
   {: pre}

   출력 예:
   ```
   Name:                default
   Namespace:           <namespace_name>
   Labels:              <none>
   Annotations:         <none>
   Image pull secrets:  <image_pull_secret_name>
   Mountable secrets:   default-token-sh2dx
   Tokens:              default-token-sh2dx
   Events:              <none>
   ```
   {: pre}

4. 레지스트리의 이미지로부터 컨테이너를 배치하십시오.
   ```
   apiVersion: v1
   kind: Pod
   metadata:
     name: mypod
   spec:
     containers:
       - name: <container_name>
         image: <region>.icr.io/<namespace_name>/<image_name>:<tag>
   ```
   {: codeblock}

5. 클러스터에 배치를 작성하십시오.
   ```
   kubectl apply -f mypod.yaml
   ```
   {: pre}

<br />


## 더 이상 사용되지 않음: 레지스트리 토큰을 사용하여 {{site.data.keyword.registrylong_notm}} 이미지에서 컨테이너를 배치합니다.
{: #namespace_token}

{{site.data.keyword.registryshort_notm}}의 네임스페이스에 저장된 개인용 이미지나 IBM 제공 공용 이미지에서 클러스터로 컨테이너를 배치할 수 있습니다. 기존 클러스터는 클러스터 `imagePullSecret`에 저장된 레지스트리 [토큰](/docs/services/Registry?topic=registry-registry_access#registry_tokens)을 사용하여 `registry.bluemix.net` 도메인 이름에서 이미지를 가져올 수 있는 액세스 권한을 부여하십시오.
{:shortdesc}

클러스터를 작성할 때 만료되지 않는 레지스트리 토큰과 시크릿이 [가장 근접한 지역 레지스트리와 글로벌 레지스트리](/docs/services/Registry?topic=registry-registry_overview#registry_regions) 둘 다에 대해 자동으로 작성됩니다. 글로벌 레지스트리는 개별 지역 레지스트리에 저장된 이미지에 대한 서로 다른 참조를 가지는 대신 배치 전반에서 참조할 수 있는 공용 IBM 제공 이미지를 저장합니다. 지역 레지스트리는 개인용 Docker 이미지를 안전하게 저장합니다. 토큰을 사용하여 이러한 공용(글로벌 레지스트리) 및 개인용(지역 레지스트리) 이미지로 작업할 수 있도록 {{site.data.keyword.registryshort_notm}}에서 사용자가 설정한 임의의 네임스페이스에 대한 읽기 전용 액세스 권한을 부여합니다.

개별 토큰은 컨테이너화된 앱을 배치할 때 Kubernetes 클러스터에 액세스할 수 있도록 Kubernetes `imagePullSecret`에 저장되어야 합니다. 클러스터가 작성되면 {{site.data.keyword.containerlong_notm}}에서 Kubernetes 이미지 풀 시크릿의 글로벌(IBM 제공 공용 이미지) 및 지역 레지스트리에 대한 토큰을 자동으로 저장합니다. 이미지 풀 시크릿은 `default` Kubernetes 네임스페이스, `kube-system` 네임스페이스 및 해당 네임스페이스의 `default` 서비스 계정에 있는 시크릿 목록에 추가됩니다.

{{site.data.keyword.registrylong_notm}}에 대한 클러스터 액세스 권한을 부여하기 위해 토큰을 사용하는 이 방법은 `registry.bluemix.net` 도메인 이름에 대해서는 지원되지만 더 이상 사용되지 않습니다. 대신 [API 키 메소드를 사용](#cluster_registry_auth)하여 새 `icr.io` 레지스트리 도메인 이름에 대한 클러스터 액세스 권한을 부여하십시오.
{: deprecated}

이미지가 있는 위치 및 컨테이너가 있는 위치에 따라 다른 단계를 수행하여 컨테이너를 배치해야 합니다.
*   [컨테이너를 클러스터와 동일한 지역에 있는 이미지를 사용하여 `default` Kubernetes 네임스페이스에 배치하십시오.](#token_default_namespace)
*   [컨테이너를 `default`가 아닌 다른 Kubernetes 네임스페이스에 배치하십시오.](#token_copy_imagePullSecret)
*   [컨테이너를 다른 지역 또는 클러스터가 아닌 {{site.data.keyword.Bluemix_notm}} 계정에 있는 이미지를 사용하여 배치하십시오.](#token_other_regions_accounts)
*   [개인용, 비 IBM 레지스트리의 이미지를 사용하여 컨테이너를 배치하십시오.](#private_images)

이 초기 설정을 사용하면 {{site.data.keyword.Bluemix_notm}} 계정의 네임스페이스에서 사용할 수 있는 이미지의 컨테이너를 클러스터의 **기본** 네임스페이스에 배치할 수 있습니다. 클러스터의 다른 네임스페이스로 컨테이너를 배치하거나 다른 {{site.data.keyword.Bluemix_notm}} 지역이나 다른 {{site.data.keyword.Bluemix_notm}} 계정에 저장된 이미지를 사용하려면 [클러스터에 대한 사용자 고유의 이미지 풀 시크릿을 작성](#other)해야 합니다.
{: note}

### 더 이상 사용되지 않음: 레지스트리 토큰을 사용하여 `default` Kubernetes 네임스페이스에 이미지 배치
{: #token_default_namespace}

이미지 풀 시크릿에 저장된 레지스트리 토큰을 사용하면 지역 {{site.data.keyword.registrylong_notm}}에서 사용할 수 있는 이미지의 컨테이너를 클러스터의 **default** 네임스페이스에 배치할 수 있습니다.
{: shortdesc}

시작하기 전에:
1. [{{site.data.keyword.registryshort_notm}}에 네임스페이스를 설정하고 이 네임스페이스에 이미지를 푸시](/docs/services/Registry?topic=registry-getting-started#gs_registry_namespace_add)하십시오.
2. [클러스터를 작성](/docs/containers?topic=containers-clusters#clusters_ui)하십시오.
3. [클러스터에 CLI를 대상으로 지정](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)하십시오.

클러스터의 **기본** 네임스페이스에 컨테이너를 배치하려면 구성 파일을 작성하십시오.

1.  `mydeployment.yaml`이라는 이름의 배치 구성 파일을 작성하십시오.
2.  {{site.data.keyword.registryshort_notm}}의 네임스페이스에서 사용하려는 배치 및 이미지를 정의하십시오.

    {{site.data.keyword.registryshort_notm}}의 네임스페이스에서 개인용 이미지 사용:

    ```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: <app_name>-deployment
    spec:
      replicas: 3
      selector:
        matchLabels:
          app: <app_name>
      template:
        metadata:
          labels:
            app: <app_name>
        spec:
          containers:
          - name: <app_name>
            image: registry.<region>.bluemix.net/<namespace>/<my_image>:<tag>
    ```
    {: codeblock}

    **팁:** 네임스페이스 정보를 검색하려면 `ibmcloud cr namespace-list`를 실행하십시오.

3.  클러스터에 배치를 작성하십시오.

    ```
    kubectl apply -f mydeployment.yaml
    ```
    {: pre}

    **팁:** IBM 제공 공용 이미지 중 하나와 같이 기존 구성 파일도 배치할 수 있습니다. 이 예에서는 미국 남부 지역에서 **ibmliberty** 이미지를 사용합니다.

    ```
    kubectl apply -f https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/deploy-apps-clusters/deploy-ibmliberty.yaml
    ```
    {: pre}

### 더 이상 사용되지 않음: 기본 네임스페이스에서 클러스터 내의 다른 네임스페이스로 이미지 풀 시크릿 복사
{: #token_copy_imagePullSecret}

`default` Kubernetes 네임스페이스에 대해 자동으로 자성된 레지스트리 토큰 인증 정보가 있는 이미지 풀 시크릿을 클러스터 내의 다른 네임스페이스로 복사할 수 있습니다.
{: shortdesc}

1. 클러스터에서 사용 가능한 네임스페이스를 나열하십시오.
   ```
   kubectl get namespaces
   ```
   {: pre}

   출력 예:
   ```
   default          Active    79d
   ibm-cert-store   Active    79d
   ibm-system       Active    79d
   istio-system     Active    34d
   kube-public      Active    79d
   kube-system      Active    79d
   ```
   {: screen}

2. 선택사항: 클러스터에 네임스페이스를 작성하십시오.
   ```
   kubectl create namespace <namespace_name>
   ```
   {: pre}

3. `default` 네임스페이스의 이미지 풀 시크릿을 원하는 네임스페이스로 복사하십시오. 새 이미지 풀 시크릿의 이름은 `bluemix-<namespace_name>-secret-regional` 및 `bluemix-<namespace_name>-secret-international`입니다.
   ```
   kubectl get secret bluemix-default-secret-regional -o yaml | sed 's/default/<namespace_name>/g' | kubectl -n <namespace_name> create -f -
   ```
   {: pre}

   ```
   kubectl get secret bluemix-default-secret-international -o yaml | sed 's/default/<namespace_name>/g' | kubectl -n <namespace_name> create -f -
   ```
   {: pre}

4.  시크릿이 작성되었는지 확인하십시오.
    ```
    kubectl get secrets --namespace <namespace_name>
    ```
    {: pre}

5. 네임스페이스에서 [`imagePullSecret`을 사용하여 컨테이너를 배치](#use_imagePullSecret)하십시오.


### 더 이상 사용되지 않음: 다른 {{site.data.keyword.Bluemix_notm}} 지역 또는 계정의 이미지에 액세스하는 이미지 풀 시크릿 작성
{: #token_other_regions_accounts}

다른 {{site.data.keyword.Bluemix_notm}} 지역 또는 계정의 이미지에 액세스하려면 레지스트리 토큰을 작성하고 이미지 풀 시크릿에 인증 정보를 저장해야 합니다.
{: shortdesc}

1.  토큰이 없는 경우 [액세스할 레지스트리의 토큰을 작성](/docs/services/Registry?topic=registry-registry_access#registry_tokens_create)하십시오.
2.  {{site.data.keyword.Bluemix_notm}} 계정의 토큰을 나열하십시오.

    ```
    ibmcloud cr token-list
    ```
    {: pre}

3.  사용하려는 토큰 ID를 기록해 두십시오.
4.  토큰의 값을 검색하십시오. <em>&lt;token_ID&gt;</em>를 이전 단계에서 검색한 토큰의 ID로 대체하십시오.

    ```
    ibmcloud cr token-get <token_id>
    ```
    {: pre}

    사용자의 토큰 값이 CLI 출력의 **토큰** 필드에 표시됩니다.

5.  토큰 정보를 저장하기 위한 Kubernetes 시크릿을 작성하십시오.

    ```
    kubectl --namespace <kubernetes_namespace> create secret docker-registry <secret_name>  --docker-server=<registry_URL> --docker-username=token --docker-password=<token_value> --docker-email=<docker_email>
    ```
    {: pre}

    <table>
    <caption>이 명령의 컴포넌트 이해</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 이 명령의 컴포넌트 이해</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace <em>&lt;kubernetes_namespace&gt;</em></code></td>
    <td>필수. 시크릿을 사용하고 컨테이너를 배치하려는 클러스터의 Kubernetes 네임스페이스입니다. 클러스터의 모든 네임스페이스를 나열하려면 <code>kubectl get namespaces</code>를 실행하십시오.</td>
    </tr>
    <tr>
    <td><code><em>&lt;secret_name&gt;</em></code></td>
    <td>필수. 이미지 풀 시크릿에 사용하려는 이름입니다.</td>
    </tr>
    <tr>
    <td><code>--docker-server <em>&lt;registry_URL&gt;</em></code></td>
    <td>필수. 네임스페이스가 설정된 이미지 레지스트리에 대한 URL입니다.<ul><li>미국 남부 및 미국 동부에 설정된 네임스페이스: <code>registry.ng.bluemix.net</code></li><li>미국 남부에 설정된 네임스페이스: <code>registry.eu-gb.bluemix.net</code></li><li>중앙 유럽(프랑크푸르트)에 설정된 네임스페이스: <code>registry.eu-de.bluemix.net</code></li><li>호주(시드니)에 설정된 네임스페이스: <code>registry.au-syd.bluemix.net</code></li></ul></td>
    </tr>
    <tr>
    <td><code>--docker-username <em>&lt;docker_username&gt;</em></code></td>
    <td>필수. 개인용 레지스트리에 로그인하기 위한 사용자 이름입니다. {{site.data.keyword.registryshort_notm}}의 경우, 사용자 이름은 <strong><code>token</code></strong> 값으로 설정됩니다.</td>
    </tr>
    <tr>
    <td><code>--docker-password <em>&lt;token_value&gt;</em></code></td>
    <td>필수. 이전에 검색한 레지스트리 토큰의 값입니다.</td>
    </tr>
    <tr>
    <td><code>--docker-email <em>&lt;docker-email&gt;</em></code></td>
    <td>필수. Docker 이메일 주소가 있는 경우 입력하십시오. 없는 경우에는 가상의 이메일 주소(예: example a@b.c)를 입력하십시오. 이 이메일은 Kubernetes 시크릿을 작성하기 위해서는 반드시 필요하지만 작성 후에는 사용되지 않습니다.</td>
    </tr>
    </tbody></table>

6.  시크릿이 작성되었는지 확인하십시오. <em>&lt;kubernetes_namespace&gt;</em>를 이미지 풀 시크릿을 작성한 네임스페이스로 대체하십시오.

    ```
    kubectl get secrets --namespace <kubernetes_namespace>
    ```
    {: pre}

7.  네임스페이스에서 [이미지 풀 시크릿을 사용하여 컨테이너를 배치](#use_imagePullSecret)하십시오.
