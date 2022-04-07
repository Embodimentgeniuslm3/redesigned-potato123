---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

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



# Helm 차트를 사용하여 서비스 추가
{: #helm}

Helm 차트를 사용하여 복잡한 Kubernetes 앱을 클러스터에 추가할 수 있습니다.
{: shortdesc}

**Helm은 개념 및 사용 방법은 무엇입니까?** </br>
[Helm ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://helm.sh)은 클러스터에서 복잡한 Kubernetes 앱을 정의하고, 설치하고, 업그레이드하는 데 사용하는 Kubernetes 패키지 관리자입니다. Helm 차트는 앱을 빌드하는 Kubernetes 리소스의 YAML 파일을 생성하기 위한 스펙을 패키지합니다. 이러한 Kubernetes 리소스는 클러스터에서 자동으로 적용되고 Helm에 의해 버전이 지정됩니다. 또한 Helm을 사용하여 고유한 앱을 지정하고 패키할 수 있으며 Helm을 통해 Kubernetes 리소스에 대한 YAML 파일을 생성할 수 있습니다.  

클러스터에서 Helm을 사용하려면 로컬 머신에 Helm CLI를 설치하고 Helm을 사용할 모든 클러스터에 Helm 서버 Tiller를 설치해야 합니다.

**{{site.data.keyword.containerlong_notm}}에서 지원되는 Helm 차트는 무엇입니까?** </br>
사용 가능한 Helm 차트의 개요는 [Helm 차트 카탈로그 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/kubernetes/solutions/helm-charts)를 참조하십시오. 이 카탈로그에 나열된 Helm 차트는 다음과 같이 그룹화됩니다. 

- **iks-charts**: {{site.data.keyword.containerlong_notm}}에 허용되는 Helm 차트입니다. 이 저장소의 이름은 `ibm`에서 `iks-charts`로 변경되었습니다.
- **ibm-charts**: {{site.data.keyword.containerlong_notm}} 및 {{site.data.keyword.Bluemix_notm}} Private 클러스터에 허용되는 Helm 차트입니다.
- **kubernetes**: Kubernetes 커뮤니티에서 제공되고 커뮤니티 통제에서 `stable`로 간주되는 Helm 차트입니다. 이 차트는 {{site.data.keyword.containerlong_notm}} 또는 {{site.data.keyword.Bluemix_notm}} Private 클러스터에서 작동하는지 확인되지 않습니다.
- **kubernetes-incubator**: Kubernetes 커뮤니티에서 제공되고 커뮤니티 통제에서 `incubator`로 간주되는 Helm 차트입니다. 이 차트는 {{site.data.keyword.containerlong_notm}} 또는 {{site.data.keyword.Bluemix_notm}} Private 클러스터에서 작동하는지 확인되지 않습니다.

**iks-charts** 및 **ibm-charts** 저장소의 Helm 차트는 {{site.data.keyword.Bluemix_notm}} 지원 조직으로 완전히 통합됩니다. 이 Helm 차트 사용에 대한 질문이나 문제가 있으면 {{site.data.keyword.containerlong_notm}} 지원 채널 중 하나를 사용할 수 있습니다. 자세한 정보는 [도움 및 지원 받기](/docs/containers?topic=containers-cs_troubleshoot_clusters#clusters_getting_help)를 참조하십시오.

**Helm을 사용하기 위한 전제조건은 무엇이며 사설 클러스터에서 Helm을 사용할 수 있습니까?** </br>
Helm 차트를 배치하려면 로컬 시스템에 Helm CLI를 설치하고 클러스터에 Helm 서버(Tiller)를 설치해야 합니다. Tiller 이미지는 공용 Google Container Registry에 저장됩니다. Tiller 설치 중에 이미지에 액세스하려면 클러스터에서 공용 Google Container Registry에 대한 공용 네트워크 연결을 허용해야 합니다. 공용 서비스 엔드포인트를 사용하는 클러스터는 자동으로 이미지에 액세스할 수 있습니다. 사용자 정의 방화벽으로 보호되는 사설 클러스터 또는 개인 서비스 엔드포인트만 사용하도록 설정된 클러스터는 Tiller 이미지에 대한 액세스를 허용하지 않습니다. 대신 [로컬 시스템으로 이미지를 가져오고 {{site.data.keyword.registryshort_notm}}의 네임스페이스에 이미지를 푸시](#private_local_tiller)하거나 [Tiller를 사용하지 않고 Helm 차트를 설치](#private_install_without_tiller)하십시오.


## 공용 액세스 권한이 있는 클러스터에 Helm 설정
{: #public_helm_install}

클러스터에서 공용 서비스 엔드포인트를 사용으로 설정한 경우에는 Google Container Registry의 공용 이미지를 사용하여 Helm 서버 Tiller를 설치할 수 있습니다.
{: shortdesc}

시작하기 전에:
- [계정에 로그인하십시오. 해당되는 경우, 적절한 리소스 그룹을 대상으로 지정하십시오. 클러스터의 컨텍스트를 설정하십시오.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
- `kube-system` 네임스페이스에서 Kubernetes 서비스 계정 및 클러스터 역할 바인딩으로 Tiller를 설치하려면 [`cluster-admin` 역할](/docs/containers?topic=containers-users#access_policies)이 있는지 확인하십시오.

공용 액세스 권한이 있는 클러스터에 Helm을 설치하려면 다음을 수행하십시오.

1. 로컬 시스템에 <a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">Helm CLI <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>를 설치하십시오.

2. 클러스터에서 Kubernetes 서비스 계정을 사용하여 Tiller를 이미 설치했는지 확인하십시오. 
   ```
   kubectl get serviceaccount --all-namespaces | grep tiller
   ```
   {: pre}

   Tiller가 설치된 경우 출력 예:
   ```
   kube-system      tiller                               1         189d
   ```
   {: screen}

   출력 예에는 Kubernetes 네임스페이스 및 Tiller에 대한 서비스 계정의 이름이 포함되어 있습니다. Tiller가 클러스터에서 서비스 계정을 사용하여 설치되지 않은 경우 CLI 출력이 리턴되지 않습니다.

3. **중요**: 클러스터 보안을 유지보수하려면 클러스터에서 서비스 계정 및 클러스터 역할 바인딩을 사용하여 Tiller를 설정하십시오.
   - **Tiller가 서비스 계정을 사용하여 설치된 경우:**
     1. Tiller 서비스 계정에 대한 클러스터 역할 바인딩을 작성하십시오. `<namespace>`를 클러스터에 Tiller가 설치되어 있는 네임스페이스로 대체하십시오.
        ```
        kubectl create clusterrolebinding tiller --clusterrole=cluster-admin --serviceaccount=<namespace>:tiller -n <namespace>
        ```
        {: pre}

     2. Tiller를 업데이트하십시오. `<tiller_service_account_name>`를 이전 단계에서 검색한 Tiller에 대한 Kubernetes 서비스 계정의 이름으로 대체하십시오.
        ```
        helm init --upgrade --service-account <tiller_service_account_name>
        ```
        {: pre}

     3. `tiller-deploy` 팟(Pod)이 클러스터에서 `Running` **상태**인지 확인하십시오.
        ```
        kubectl get pods -n <namespace> -l app=helm
        ```
        {: pre}

        출력 예:

        ```
        NAME                            READY     STATUS    RESTARTS   AGE
        tiller-deploy-352283156-nzbcm   1/1       Running   0          2m
        ```
        {: screen}

   - **Tiller가 서비스 계정을 사용하여 설치되지 않은 경우:**
     1. 클러스터의 `kube-system` 네임스페이스에서 Tiller에 대한 Kubernetes 서비스 계정 및 클러스터 역할 바인딩을 작성하십시오.
        ```
        kubectl create serviceaccount tiller -n kube-system
        ```
        {: pre}

        ```
        kubectl create clusterrolebinding tiller --clusterrole=cluster-admin --serviceaccount=kube-system:tiller -n kube-system
        ```
        {: pre}

     2. Tiller 서비스 계정이 작성되었는지 확인하십시오.
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

     3. Helm CLI를 초기화하고 작성한 서비스 계정을 사용하여 Tiller를 설치하십시오.
        ```
    helm init --service-account tiller
        ```
        {: pre}

     4. `tiller-deploy` 팟(Pod)이 클러스터에서 `Running` **상태**인지 확인하십시오.
        ```
        kubectl get pods -n kube-system -l app=helm
        ```
        {: pre}

        출력 예:
        ```
        NAME                            READY     STATUS    RESTARTS   AGE
        tiller-deploy-352283156-nzbcm   1/1       Running   0          2m
        ```
        {: screen}

4. {{site.data.keyword.Bluemix_notm}} Helm 저장소를 Helm 인스턴스에 추가하십시오.
   ```
   helm repo add iks-charts https://icr.io/helm/iks-charts
   ```
   {: pre}

   ```
   helm repo add ibm-charts https://icr.io/helm/ibm-charts
   ```
   {: pre}

5. 모든 Helm 차트의 최신 버전을 검색하려면 저장소를 업데이트하십시오.
   ```
   helm repo update
   ```
   {: pre}

6. 현재 {{site.data.keyword.Bluemix_notm}} 저장소에서 사용 가능한 Helm 차트를 나열하십시오.
   ```
   helm search iks-charts
   ```
   {: pre}

   ```
   helm search ibm-charts
   ```
   {: pre}

7. 설치할 Helm 차트를 식별하고 Helm 차트 `README`의 지시사항에 따라 클러스터에 Helm 차트를 설치하십시오.


## 사설 클러스터: IBM Cloud Container Registry의 네임스페이스에 Tiller 이미지 푸시
{: #private_local_tiller}

Tiller 이미지를 로컬 시스템으로 가져오고 이미지를 {{site.data.keyword.registryshort_notm}}의 네임스페이스에 푸시하고 {{site.data.keyword.registryshort_notm}}의 이미지를 사용하여 사설 클러스터에 Tiller를 설치하도록 할 수 있습니다.
{: shortdesc}

Tiller를 사용하지 않고 Helm 차트를 설치하려면 [사설 클러스터: Tiller를 사용하지 않고 Helm 차트 설치](#private_install_without_tiller)를 참조하십시오.
{: tip}

시작하기 전에:
- 로컬 시스템에 Docker를 설치하십시오. [{{site.data.keyword.Bluemix_notm}} CLI](/docs/cli?topic=cloud-cli-getting-started)를 설치한 경우 Docker는 이미 설치되어 있습니다.
- [{{site.data.keyword.registryshort_notm}} CLI 플러그인을 설치하고 네임스페이스를 설정](/docs/services/Registry?topic=registry-getting-started#gs_registry_cli_install)하십시오.
- `kube-system` 네임스페이스에서 Kubernetes 서비스 계정 및 클러스터 역할 바인딩으로 Tiller를 설치하려면 [`cluster-admin` 역할](/docs/containers?topic=containers-users#access_policies)이 있는지 확인하십시오.

{{site.data.keyword.registryshort_notm}}을 사용하여 Tiller를 설치하려면 다음을 수행하십시오.

1. 로컬 시스템에 <a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">Helm CLI <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>를 설치하십시오.
2. 설정한 {{site.data.keyword.Bluemix_notm}} 인프라 VPN 터널을 사용하여 사설 클러스터에 연결하십시오.
3. **중요**: 클러스터 보안을 유지보수하려면 [{{site.data.keyword.Bluemix_notm}} `kube-samples` 저장소](https://github.com/IBM-Cloud/kube-samples/blob/master/rbac/serviceaccount-tiller.yaml)에서 다음 YAML 파일을 적용하여 `kube-system` 네임스페이스에 Tiller에 대한 서비스 계정을 작성하고 `tiller-deploy` 팟(Pod)에 대한 Kubernetes RBAC 클러스터 역할 바인딩을 작성하십시오.
    1. [Kubernetes 서비스 계정 및 클러스터 역할 바인딩 YAML 파일을 가져오십시오 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/rbac/serviceaccount-tiller.yaml).

    2. 클러스터에 Kubernetes 리소스를 작성하십시오.
       ```
       kubectl apply -f service-account.yaml
       ```
       {: pre}

       ```
       kubectl apply -f cluster-role-binding.yaml
       ```
       {: pre}

4. 클러스터에 설치할 [Tiller 버전을 찾으십시오 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://console.cloud.google.com/gcr/images/kubernetes-helm/GLOBAL/tiller?gcrImageListsize=30). 특정 버전이 필요하지 않은 경우 최신 버전을 사용하십시오.

5. Tiller 이미지를 공용 Google Container Registry에서 로컬 시스템으로 가져오십시오.
   ```
   docker pull gcr.io/kubernetes-helm/tiller:v2.11.0
   ```
   {: pre}

   출력 예:
   ```
   docker pull gcr.io/kubernetes-helm/tiller:v2.13.0
   v2.13.0: Pulling from kubernetes-helm/tiller
   48ecbb6b270e: Pull complete
   d3fa0712c71b: Pull complete
   bf13a43b92e9: Pull complete
   b3f98be98675: Pull complete
   Digest: sha256:c4bf03bb67b3ae07e38e834f29dc7fd43f472f67cad3c078279ff1bbbb463aa6
   Status: Downloaded newer image for gcr.io/kubernetes-helm/tiller:v2.13.0
   ```
   {: screen}

6. [{{site.data.keyword.registryshort_notm}}에서 네임스페이스에 Tiller 이미지를 푸시](/docs/services/Registry?topic=registry-getting-started#gs_registry_images_pushing)하십시오.

7. 클러스터 내 {{site.data.keyword.registryshort_notm}}의 이미지에 액세스하려면 [기본 네임스페이스에서 `kube-system` 네임스페이스로 이미지 풀 시크릿 복사](/docs/containers?topic=containers-images#copy_imagePullSecret)를 참조하십시오.

8. {{site.data.keyword.registryshort_notm}}의 네임스페이스에 저장한 이미지를 사용하여 사설 클러스터에 Tiller를 설치하십시오.
   ```
   helm init --tiller-image <region>.icr.io/<mynamespace>/<myimage>:<tag> --service-account tiller
   ```
   {: pre}

9. {{site.data.keyword.Bluemix_notm}} Helm 저장소를 Helm 인스턴스에 추가하십시오.
   ```
   helm repo add iks-charts https://icr.io/helm/iks-charts
   ```
   {: pre}

   ```
   helm repo add ibm-charts https://icr.io/helm/ibm-charts
   ```
   {: pre}

10. 모든 Helm 차트의 최신 버전을 검색하려면 저장소를 업데이트하십시오.
    ```
    helm repo update
    ```
    {: pre}

11. 현재 {{site.data.keyword.Bluemix_notm}} 저장소에서 사용 가능한 Helm 차트를 나열하십시오.
    ```
    helm search iks-charts
    ```
    {: pre}

    ```
    helm search ibm-charts
    ```
    {: pre}

12. 설치할 Helm 차트를 식별하고 Helm 차트 `README`의 지시사항에 따라 클러스터에 Helm 차트를 설치하십시오.


## 사설 클러스터: Tiller를 사용하지 않고 Helm 차트 설치
{: #private_install_without_tiller}

사설 클러스터에 Tiller를 설치하지 않으려면, Helm 차트 YAML 파일을 수동으로 작성하고 `kubectl` 명령을 사용하여 적용할 수 있습니다.
{: shortdesc}

이 예제의 단계는 사설 클러스터의 {{site.data.keyword.Bluemix_notm}} Helm 차트 저장소에서 Helm 차트를 설치하는 방법을 보여줍니다. {{site.data.keyword.Bluemix_notm}} Helm 차트 저장소 중 하나에 저장되지 않은 Helm 차트를 설치하려면 이 주제의 지시사항에 따라 Helm 차트에 필요한 YAML 파일을 작성해야 합니다. 또한 공용 컨테이너 레지스트리에서 Helm 차트 이미지를 다운로드하고 이를 {{site.data.keyword.registryshort_notm}}의 네임스페이스에 푸시하고 `values.yaml` 파일을 업데이트하여 {{site.data.keyword.registryshort_notm}}의 이미지를 사용해야 합니다.
{: note}

1. 로컬 시스템에 <a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">Helm CLI <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>를 설치하십시오.
2. 설정한 {{site.data.keyword.Bluemix_notm}} 인프라 VPN 터널을 사용하여 사설 클러스터에 연결하십시오.
3. {{site.data.keyword.Bluemix_notm}} Helm 저장소를 Helm 인스턴스에 추가하십시오.
   ```
   helm repo add iks-charts https://icr.io/helm/iks-charts
   ```
   {: pre}

   ```
   helm repo add ibm-charts https://icr.io/helm/ibm-charts
   ```
   {: pre}

4. 모든 Helm 차트의 최신 버전을 검색하려면 저장소를 업데이트하십시오.
   ```
   helm repo update
   ```
   {: pre}

5. 현재 {{site.data.keyword.Bluemix_notm}} 저장소에서 사용 가능한 Helm 차트를 나열하십시오.
   ```
   helm search iks-charts
   ```
   {: pre}

   ```
    helm search ibm-charts
   ```
   {: pre}

6. 설치하려는 Helm 차트를 식별하고, 로컬 시스템에 Helm 차트를 다운로드하고, Helm 차트의 파일을 압축 해체하십시오. 다음 예제는 클러스터 오토스케일러 버전 1.0.3의 Helm 차트를 다운로드하고 `cluster-autoscaler` 디렉토리에 해당 파일을 압축 해제하는 방법을 보여줍니다.
   ```
   helm fetch iks-charts/ibm-iks-cluster-autoscaler --untar --untardir ./cluster-autoscaler --version 1.0.3
   ```
   {: pre}

7. Helm 차트 파일을 압축 해제한 디렉토리로 이동하십시오.
   ```
   cd cluster-autoscaler
   ```
   {: pre}

8. Helm 차트의 파일을 사용하여 생성하는 YAML 파일에 대한 `output` 디렉토리를 작성하십시오.
   ```
   mkdir output
   ```
   {: pre}

9. `values.yaml` 파일을 열고 Helm 차트 설치 지시사항에 필요한 사항을 변경하십시오.
   ```
   nano ibm-iks-cluster-autoscaler/values.yaml
   ```
   {: pre}

10. 로컬 Helm 설치를 사용하여 Helm 차트에 대한 모든 Kubernetes YAML 파일을 작성하십시오. YAML 파일은 이전에 작성한 `output` 디렉토리에 저장됩니다.
    ```
    helm template --values ./ibm-iks-cluster-autoscaler/values.yaml --output-dir ./output ./ibm-iks-cluster-autoscaler
    ```
    {: pre}

    출력 예:
    ```
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-configmap.yaml
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-service-account-roles.yaml
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-service.yaml
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-deployment.yaml
    ```
    {: screen}

11. 사설 클러스터에 모든 YAML 파일을 배치하십시오.
    ```
    kubectl apply --recursive --filename ./output
    ```
   {: pre}

12. 선택사항: `output` 디렉토리에서 모든 YAML 파일을 제거하십시오.
    ```
    kubectl delete --recursive --filename ./output
    ```
    {: pre}

## 관련된 Helm 링크
{: #helm_links}

다음 링크를 검토하여 추가 Helm 정보를 찾으십시오.
{: shortdesc}

* [Helm 차트 카탈로그 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/kubernetes/solutions/helm-charts)의 {{site.data.keyword.containerlong_notm}}에서 사용할 수 있는 사용 가능한 Helm 차트를 확인하십시오.
* <a href="https://docs.helm.sh/helm/" target="_blank">Helm 문서 <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>에서 Helm 차트를 설정하고 관리하는 데 사용할 수 있는 Helm 명령에 대해 자세히 알아보십시오.
* [Kubernetes Helm 차트를 사용하여 배치 속도를 향상 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://developer.ibm.com/recipes/tutorials/increase-deployment-velocity-with-kubernetes-helm-charts/)시키는 방법에 대해 자세히 알아보십시오.
