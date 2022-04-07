---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: kubernetes, iks, ibmcloud, ic, ks, kubectl

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



# CLI 및 API 설정
{: #cs_cli_install}

{{site.data.keyword.containerlong}} CLI 또는 API를 사용하여 Kubernetes 클러스터를 작성하고 관리할 수 있습니다.
{:shortdesc}

## IBM Cloud CLI 및 플러그인 설치
{: #cs_cli_install_steps}

{{site.data.keyword.containerlong_notm}}에서 Kubernetes 클러스터를 작성해서 관리하며 컨테이너화된 앱을 클러스터에 배치하기 위해 필요한 CLI를 설치하십시오.
{:shortdesc}

이 태스크에는 다음 CLI 및 플러그인을 설치하기 위한 정보가 포함되어 있습니다.

-   {{site.data.keyword.Bluemix_notm}} CLI
-   {{site.data.keyword.containerlong_notm}} 플러그인
-   {{site.data.keyword.registryshort_notm}} 플러그인

대신 {{site.data.keyword.Bluemix_notm}} 콘솔을 사용할 경우 클러스터가 작성된 후 [Kubernetes 터미널](#cli_web)의 웹 브라우저에서 직접 CLI 명령을 실행할 수 있습니다.
{: tip}

<br>
CLI를 설치하려면 다음을 수행하십시오.

1.  [{{site.data.keyword.Bluemix_notm}} CLI ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](/docs/cli?topic=cloud-cli-getting-started#idt-prereq)를 설치하십시오. 이 설치에는 다음이 포함됩니다.
    -   기본 {{site.data.keyword.Bluemix_notm}} CLI(`ibmcloud`). 
    -   {{site.data.keyword.containerlong_notm}} 플러그인(`ibmcloud ks`). 
    -   {{site.data.keyword.registryshort_notm}} 플러그인(`ibmcloud cr`). 이 플러그인을 사용하여 IBM이 호스팅하는 멀티 테넌트, 고가용성 및 확장 가능한 개인용 이미지 레지스트리에서 사용자 고유의 네임스페이스를 설정할 수 있으며 Docker 이미지를 저장하고 이를 다른 사용자와 공유할 수 있습니다. Docker 이미지는 클러스터로 컨테이너를 배치하는 데 필요합니다.
    -   기본 버전(1.13.6)에 대응하는 Kubernetes CLI(`kubectl`). <p class="note">다른 버전을 실행하는 클러스터를 사용하려는 경우에는 [해당 Kubernetes CLI 버전을 별도로 설치](#kubectl)해야 할 수 있습니다. (OpenShift) 클러스터가 있는 경우에는 [`oc` 및 `kubectl` CLI를 함께 설치](#cli_oc)합니다. </p>
    -   Helm CLI(`helm`). Helm 차트를 통해 클러스터에 {{site.data.keyword.Bluemix_notm}} 서비스 및 복합 앱을 설치하기 위한 패키지 관리자로 Helm을 사용할 수 있습니다. Helm을 사용할 각 클러스터에서는 여전히 [Helm을 설정](/docs/containers?topic=containers-helm)해야 합니다. 

    CLI를 많이 사용할 계획입니까? [{{site.data.keyword.Bluemix_notm}} CLI에 대한 쉘 자동 완성 기능을 사용(Linux/MacOS에만 해당)](/docs/cli/reference/ibmcloud?topic=cloud-cli-shell-autocomplete#shell-autocomplete-linux)해 보십시오.
    {: tip}

2.  {{site.data.keyword.Bluemix_notm}} CLI에 로그인하십시오. 프롬프트가 표시되면 {{site.data.keyword.Bluemix_notm}} 인증 정보를 입력하십시오.
    ```
    ibmcloud login
    ```
    {: pre}

    연합 ID가 있는 경우에는 `ibmcloud login --sso`를 사용하여 {{site.data.keyword.Bluemix_notm}} CLI에 로그인하십시오. 사용자 이름을 입력하고 CLI 출력에서 제공된 URL을 사용하여 일회성 패스코드를 검색하십시오. `--sso` 옵션을 사용하지 않으면 로그인에 실패하고 `--sso` 옵션을 사용하면 성공하는 경우에는 연합 ID를 보유하고 있다는 것입니다.
    {: tip}

3.  {{site.data.keyword.containerlong_notm}} 플러그인 및 {{site.data.keyword.registryshort_notm}} 플러그인이 올바르게 설치되었는지 확인하십시오.
    ```
    ibmcloud plugin list
    ```
    {: pre}

    출력 예:
    ```
    Listing installed plug-ins...

    Plugin Name                            Version   Status        
    container-registry                     0.1.373     
    container-service/kubernetes-service   0.3.23   
    ```
    {: screen}

이러한 CLI에 대한 참조 정보는 해당 도구의 문서를 참조하십시오.

-   [`ibmcloud` 명령](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_cli#ibmcloud_cli)
-   [`ibmcloud ks` 명령](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli)
-   [`ibmcloud cr` 명령](/docs/services/Registry?topic=container-registry-cli-plugin-containerregcli)

## Kubernetes CLI(`kubectl`) 설치
{: #kubectl}

Kubernetes 대시보드의 로컬 버전을 보고 클러스터에 앱을 배치하려면 Kubernetes CLI(`kubectl`)를 설치하십시오. 최신 `kubectl`의 고정 버전이 기본 {{site.data.keyword.Bluemix_notm}} CLI와 함께 설칟됩니다. 그러나 클러스터에 대해 작업하려면 사용하려는 Kubernetes 클러스터 `major.minor` 버전과 일치하는 Kubernetes CLI `major.minor` 버전을 대신 설치해야 합니다. 최소한 클러스터의 `major.minor` 버전과 일치하지 않는 `kubectl` CLI 버전을 사용하면 예상치 못한 결과가 발생할 수 있습니다. 예를 들어, 서버 버전과 두 버전(n +/- 2) 이상 차이 나는 `kubectl` 클라이언트 버전을 [Kubernetes는 지원하지 않습니다 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/setup/version-skew-policy/). Kubernetes 클러스터 및 CLI 버전을 최신 상태로 유지해야 합니다.
{: shortdesc}

OpenShift 클러스터를 사용하고 계십니까? 이러한 경우에는 `kubectl`과 함께 제공되는 OpenShift Origin CLI(`oc`)를 설치하십시오. Red Hat OpenShift on IBM Cloud 클러스터와 Ubuntu 기본 {{site.data.keyword.containershort_notm}} 클러스터를 모두 보유하고 있는 경우에는 반드시 자신의 클러스터 `major.minor` Kubernetes 버전에 대응하는 `kubectl` 바이너리 파일을 사용하십시오.
{: tip}

1.  클러스터가 이미 있는 경우에는 클라이언트 `kubectl` CLI의 버전이 클러스터 API 서버의 버전과 일치하는지 확인하십시오. 
    1.  [계정에 로그인하십시오. 해당되는 경우, 적절한 리소스 그룹을 대상으로 지정하십시오. 클러스터의 컨텍스트를 설정하십시오.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
    2.  클라이언트와 서버의 버전을 비교하십시오. 클라이언트의 버전이 서버와 일치하지 않는 경우에는 다음 단계로 진행하십시오. 버전이 일치하는 경우에는 이미 적절한 `kubectl` 버전을 설치한 것입니다.
        ```
        kubectl version --short
        ```
        {: pre}
2.  사용하려는 Kubernetes 클러스터 `major.minor` 버전과 일치하는 Kubernetes CLI `major.minor` 버전을 다운로드하십시오. 현재 {{site.data.keyword.containerlong_notm}} 기본 Kubernetes 버전은 1.13.6입니다. 
    -   **OS X**: [https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/darwin/amd64/kubectl ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/darwin/amd64/kubectl)
    -   **Linux**: [https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/linux/amd64/kubectl ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/linux/amd64/kubectl)
    -   **Windows**: {{site.data.keyword.Bluemix_notm}} CLI와 동일한 디렉토리에 Kubernetes CLI를 설치하십시오. 이 설정을 사용하면 나중에 명령을 실행할 때 일부 파일 경로 변경이 필요하지 않습니다. [https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/windows/amd64/kubectl.exe ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/windows/amd64/kubectl.exe)

3.  OS X 또는 Linux를 사용하는 경우 다음 단계를 완료하십시오. 
    1.  실행 파일을 `/usr/local/bin` 디렉토리로 이동하십시오.
        ```
        mv /<filepath>/kubectl /usr/local/bin/kubectl
        ```
        {: pre}

    2.  `/usr/local/bin`이 `PATH` 시스템 변수에 나열되어 있는지 확인하십시오. `PATH` 변수에는 운영 체제가 실행 파일을 찾을 수 있는 모든 디렉토리가 포함되어 있습니다. `PATH` 변수에 나열된 디렉토리는 서로 다른 용도로 사용됩니다. `/usr/local/bin`은 시스템 관리자가 수동으로 설치했으며 운영 체제의 일부가 아닌 소프트웨어의 실행 파일을 저장하는 데 사용됩니다.
        ```
        echo $PATH
        ```
        {: pre}
        CLI 출력 예:
        ```
        /usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
        ```
        {: screen}

    3.  파일을 실행 가능하도록 설정하십시오.
        ```
        chmod +x /usr/local/bin/kubectl
        ```
        {: pre}
4.  **선택사항**: [`kubectl` 명령에 대한 자동 완성 기능을 사용으로 설정 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/tasks/tools/install-kubectl/#enabling-shell-autocompletion)하십시오. 단계는 사용하는 쉘에 따라 달라집니다. 

다음으로 [{{site.data.keyword.containerlong_notm}}의 CLI에서 Kubernetes 클러스터 작성](/docs/containers?topic=containers-clusters#clusters_cli_steps)을 시작하십시오. 

Kubernetes CLI에 대한 자세한 정보는 [`kubectl` 참조 문서 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubectl.docs.kubernetes.io/)를 참조하십시오.
{: note}

<br />


## OpenShift Origin CLI(`oc`) 미리보기 베타 설치
{: #cli_oc}

[Red Hat OpenShift on IBM Cloud](/docs/containers?topic=containers-openshift_tutorial)는 OpenShift 클러스터를 테스트하기 위한 베타로서 사용 가능합니다.
{: preview}

OpenShift 대시보드의 로컬 버전을 보고 Red Hat OpenShift on IBM Cloud 클러스터에 앱을 배치하려면 OpenShift Origin CLI(`oc`)를 설치하십시오. `oc` CLI는 대응하는 Kubernetes CLI(`kubectl`) 버전을 포함합니다. 자세한 정보는 [OpenShift 문서 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.openshift.com/container-platform/3.11/cli_reference/get_started_cli.html)를 참조하십시오.
{: shortdesc}

Red Hat OpenShift on IBM Cloud 클러스터와 Ubuntu 기본 {{site.data.keyword.containershort_notm}} 클러스터를 둘 다 사용하고 계십니까? `oc` CLI는 `oc` 바이너리 및 `kubectl` 바이너리와 함께 제공되지만 다른 클러스터에서는 다른 Kubernetes 버전(예: OpenShift는 1.11, Ubuntu는 1.13.6)을 실행할 수도 있습니다. 반드시 자신의 클러스터 `major.minor` Kubernetes 버전에 대응하는 `kubectl` 바이너리를 사용하십시오.
{: note}

1.  자신의 로컬 운영 체제 및 OpenShift 버전에 맞는 [OpenShift Origin CLI를 다운로드 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.okd.io/download.html)하십시오. 현재 기본 OpenShift 버전은 3.11입니다. 

2.  Mac OS 또는 Linux를 사용하는 경우 다음 단계를 완료하여 바이너리를 `PATH` 시스템 변수에 추가하십시오. Windows를 사용하는 경우에는 `oc` CLI를 {{site.data.keyword.Bluemix_notm}} CLI와 동일한 디렉토리에 설치하십시오. 이 설정을 사용하면 나중에 명령을 실행할 때 일부 파일 경로 변경이 필요하지 않습니다.
    1.  `oc` 및 `kubectl` 실행 파일을 `/usr/local/bin` 디렉토리로 이동하십시오.
        ```
        mv /<filepath>/oc /usr/local/bin/oc && mv /<filepath>/kubectl /usr/local/bin/kubectl
        ```
        {: pre}

    2.  `/usr/local/bin`이 `PATH` 시스템 변수에 나열되어 있는지 확인하십시오. `PATH` 변수에는 운영 체제가 실행 파일을 찾을 수 있는 모든 디렉토리가 포함되어 있습니다. `PATH` 변수에 나열된 디렉토리는 서로 다른 용도로 사용됩니다. `/usr/local/bin`은 시스템 관리자가 수동으로 설치했으며 운영 체제의 일부가 아닌 소프트웨어의 실행 파일을 저장하는 데 사용됩니다.
        ```
        echo $PATH
        ```
        {: pre}
        CLI 출력 예:
        ```
        /usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
        ```
        {: screen}
3.  **선택사항**: [`kubectl` 명령에 대한 자동 완성 기능을 사용으로 설정 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/tasks/tools/install-kubectl/#enabling-shell-autocompletion)하십시오. 단계는 사용하는 쉘에 따라 달라집니다. 해당 단계를 반복하여 `oc` 명령에 대한 자동 완성 기능을 사용으로 설정할 수 있습니다. 예를 들어 Linux의 bash에서는 `kubectl completion bash >/etc/bash_completion.d/kubectl` 대신 `oc completion bash >/etc/bash_completion.d/oc_completion`을 실행할 수 있습니다. 

다음으로 [Red Hat OpenShift on IBM Cloud 클러스터(미리보기) 작성](/docs/containers?topic=containers-openshift_tutorial)을 시작하십시오. 

OpenShift Origin CLI에 대한 자세한 정보는 [`oc` 명령 문서 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.openshift.com/container-platform/3.11/cli_reference/basic_cli_operations.html)를 참조하십시오.
{: note}

<br />


## 컴퓨터의 컨테이너에서 CLI 실행
{: #cs_cli_container}

각 CLI를 개별적으로 컴퓨터에 설치하는 대신 컴퓨터에서 실행되는 컨테이너에 CLI를 설치할 수 있습니다.
{:shortdesc}

시작하기 전에 [Docker를 설치 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.docker.com/community-edition#/download)하여 이미지를 로컬로 빌드하고 실행하십시오. Windows 8 이하를 사용 중인 경우 [Docker Toolbox ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.docker.com/toolbox/toolbox_install_windows/)를 대신 설치할 수 있습니다.

1. 제공된 Dockerfile에서 이미지를 작성하십시오.

    ```
    docker build -t <image_name> https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/install-clis-container/Dockerfile
    ```
    {: pre}

2. 이미지를 로컬로 컨테이너로 배치하고 로컬 파일에 액세스하기 위한 볼륨을 마운트하십시오.

    ```
    docker run -it -v /local/path:/container/volume <image_name>
    ```
    {: pre}

3. 대화식 쉘에서 `ibmcloud ks` 및 `kubectl` 명령 실행을 시작하십시오. 저장할 데이터를 작성하는 경우 마운트한 볼륨에 해당 데이터를 저장하십시오. 쉘을 종료하면 컨테이너가 중지됩니다.

<br />



## `kubectl`을 실행하도록 CLI 구성
{: #cs_cli_configure}

Kubernetes CLI와 함께 제공되는 명령을 사용하여 {{site.data.keyword.Bluemix_notm}}에서 클러스터를 관리할 수 있습니다.
{:shortdesc}

Kubernetes 1.13.6에서 사용 가능한 모든 `kubectl` 명령은 {{site.data.keyword.Bluemix_notm}}의 클러스터에서 사용할 수 있도록 지원됩니다. 클러스터를 작성한 후 로컬 CLI에 대한 컨텍스트를 환경 변수가 있는 해당 클러스터로 설정하십시오. 그런 다음, Kubernetes `kubectl` 명령을 실행하여 {{site.data.keyword.Bluemix_notm}}에서 클러스터 관련 작업을 수행할 수 있습니다.

`kubectl` 명령을 실행하려면 우선 다음을 수행하십시오.
* [필수 CLI를 설치](#cs_cli_install)하십시오.
* [클러스터를 작성](/docs/containers?topic=containers-clusters#clusters_cli_steps)하십시오.
* Kubernetes 리소스로 작업할 수 있도록 적절한 Kubernetes RBAC 역할을 권한 부여하는 [서비스 역할](/docs/containers?topic=containers-users#platform)이 있는지 확인하십시오. 서비스 역할이 있지만 플랫폼 역할이 없는 경우 클러스터를 나열하려면 클러스터 관리자에게 클러스터 이름과 ID, 또는 **뷰어** 플랫폼 역할을 제공받아야 합니다.

`kubectl` 명령을 사용하려면 다음을 수행하십시오.

1.  {{site.data.keyword.Bluemix_notm}} CLI에 로그인하십시오. 프롬프트가 표시되면 {{site.data.keyword.Bluemix_notm}} 인증 정보를 입력하십시오.

    ```
    ibmcloud login
    ```
    {: pre}

    연합 ID가 있는 경우에는 `ibmcloud login --sso`를 사용하여 {{site.data.keyword.Bluemix_notm}} CLI에 로그인하십시오. 사용자 이름을 입력하고 CLI 출력에서 제공된 URL을 사용하여 일회성 패스코드를 검색하십시오. `--sso` 옵션을 사용하지 않으면 로그인에 실패하고 `--sso` 옵션을 사용하면 성공하는 경우에는 연합 ID를 보유하고 있다는 것입니다.
    {: tip}

2.  {{site.data.keyword.Bluemix_notm}} 계정을 선택하십시오. 여러 {{site.data.keyword.Bluemix_notm}} 조직에 지정된 경우에는 Kubernetes 클러스터가 작성된 조직을 선택하십시오. 클러스터는 조직마다 고유하지만 {{site.data.keyword.Bluemix_notm}} 영역에는 독립적입니다. 따라서 영역을 선택할 필요가 없습니다.

3.  기본 외의 리소스 그룹에 클러스터를 작성하고 이에 대해 작업하려면 해당 리소스 그룹을 대상으로 지정하십시오. 각 클러스터가 속하는 리소스 그룹을 보려면 `ibmcloud ks clusters`를 실행하십시오. **참고**: 해당 리소스 그룹에 대해 [**Viewer** 액세스 권한](/docs/containers?topic=containers-users#platform)을 갖고 있어야 합니다.
    ```
    ibmcloud target -g <resource_group_name>
    ```
    {: pre}

4.  클러스터의 이름을 가져오려면 계정에 있는 모든 클러스터를 나열하십시오. {{site.data.keyword.Bluemix_notm}} IAM 서비스 역할만 있고 클러스터를 볼 수 없는 경우 클러스터 관리자에게 IAM 플랫폼 **뷰어** 역할 또는 클러스터 이름과 ID를 요청하십시오.

    ```
    ibmcloud ks clusters
    ```
    {: pre}

5.  작성한 클러스터를 이 세션에 대한 컨텍스트로 설정하십시오. 클러스터 관련 작업을 수행할 때마다 다음 구성 단계를 완료하십시오.
    1.  환경 변수를 설정하기 위한 명령을 가져오고 Kubernetes 구성 파일을 다운로드하십시오. <p class="tip">Windows PowerShell을 사용하고 계십니까? `--powershell` 플래그를 포함하여 Windows PowerShell 형식으로 된 환경 변수를 가져오십시오.
    </p>
        ```
        ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
        ```
        {: pre}

        구성 파일을 다운로드하고 나면 환경 변수로서 경로를 로컬 Kubernetes 구성 파일로 설정하는 데 사용할 수 있는 명령이 표시됩니다.

        예:

        ```
        export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/mycluster/kube-config-prod-dal10-mycluster.yml
        ```
        {: screen}

    2.  `KUBECONFIG` 환경 변수를 설정하려면 터미널에 표시되는 명령을 복사하여 붙여넣으십시오.

        **Mac 또는 Linux 사용자**: `ibmcloud ks cluster-config` 명령을 실행하고 `KUBECONFIG` 환경 변수를 복사하는 대신에 `ibmcloud ks cluster-config --export <cluster-name>`를 실행할 수 있습니다. 쉘에 따라 `eval $(ibmcloud ks cluster-config --export <cluster-name>)`를 실행하여 쉘을 설정할 수 있습니다.
        {: tip}

    3.  `KUBECONFIG` 환경 변수가 올바르게 설정되었는지 확인하십시오.

        예:

        ```
        echo $KUBECONFIG
        ```
        {: pre}

        출력:
        ```
        /Users/<user_name>/.bluemix/plugins/container-service/clusters/mycluster/kube-config-prod-dal10-mycluster.yml
        ```
        {: screen}

6.  Kubernetes CLI 서버 버전을 확인하여 `kubectl` 명령이 올바르게 실행되는지 확인하십시오.

    ```
    kubectl version  --short
    ```
    {: pre}

    출력 예:

    ```
    Client Version: v1.13.6
    Server Version: v1.13.6
    ```
    {: screen}

이제 `kubectl` 명령을 실행하여 {{site.data.keyword.Bluemix_notm}}에서 클러스터를 관리할 수 있습니다. 명령의 전체 목록은 [Kubernetes 문서 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubectl.docs.kubernetes.io/)를 참조하십시오.

**팁:** Windows를 사용 중이며 Kubernetes CLI가 {{site.data.keyword.Bluemix_notm}} CLI와 동일한 디렉토리에 설치되지 않은 경우, `kubectl` 명령을 정상적으로 실행하려면 Kubernetes CLI가 설치된 경로로 디렉토리를 변경해야 합니다.


<br />




## CLI 업데이트
{: #cs_cli_upgrade}

새 기능을 사용하기 위해 CLI를 주기적으로 업데이트하고자 할 수 있습니다.
{:shortdesc}

이 태스크에는 다음의 CLI를 업데이트하기 위한 정보가 포함되어 있습니다.

-   {{site.data.keyword.Bluemix_notm}} CLI 버전 0.8.0 이상
-   {{site.data.keyword.containerlong_notm}} 플러그인
-   Kubernetes CLI 버전 1.13.6 이상
-   {{site.data.keyword.registryshort_notm}} 플러그인

<br>
CLI를 업데이트하려면 다음을 수행하십시오.

1.  {{site.data.keyword.Bluemix_notm}} CLI를 업데이트하십시오. [최신 버전 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](/docs/cli?topic=cloud-cli-getting-started)을 다운로드하고 설치 프로그램을 실행하십시오.

2. {{site.data.keyword.Bluemix_notm}} CLI에 로그인하십시오. 프롬프트가 표시되면 {{site.data.keyword.Bluemix_notm}} 인증 정보를 입력하십시오.

    ```
    ibmcloud login
    ```
    {: pre}

     연합 ID가 있는 경우에는 `ibmcloud login --sso`를 사용하여 {{site.data.keyword.Bluemix_notm}} CLI에 로그인하십시오. 사용자 이름을 입력하고 CLI 출력에서 제공된 URL을 사용하여 일회성 패스코드를 검색하십시오. `--sso` 옵션을 사용하지 않으면 로그인에 실패하고 `--sso` 옵션을 사용하면 성공하는 경우에는 연합 ID를 보유하고 있다는 것입니다.
     {: tip}

3.  {{site.data.keyword.containerlong_notm}} 플러그인을 업데이트하십시오.
    1.  {{site.data.keyword.Bluemix_notm}} 플러그인 저장소에서 업데이트를 설치하십시오.

        ```
        ibmcloud plugin update container-service 
        ```
        {: pre}

    2.  다음 명령을 실행하고 설치된 플러그인의 목록을 확인하여 플러그인 설치를 확인하십시오.

        ```
        ibmcloud plugin list
        ```
        {: pre}

        {{site.data.keyword.containerlong_notm}} 플러그인은 container-service로 결과에 표시됩니다.

    3.  CLI를 초기화하십시오.

        ```
        ibmcloud ks init
        ```
        {: pre}

4.  [Kubernetes CLI를 업데이트](#kubectl)하십시오.

5.  {{site.data.keyword.registryshort_notm}} 플러그인을 업데이트하십시오.
    1.  {{site.data.keyword.Bluemix_notm}} 플러그인 저장소에서 업데이트를 설치하십시오.

        ```
        ibmcloud plugin update container-registry 
        ```
        {: pre}

    2.  다음 명령을 실행하고 설치된 플러그인의 목록을 확인하여 플러그인 설치를 확인하십시오.

        ```
        ibmcloud plugin list
        ```
        {: pre}

        레지스트리 플러그인이 container-registry로 결과에 표시됩니다.

<br />


## CLI 설치 제거
{: #cs_cli_uninstall}

CLI가 더 이상 필요하지 않으면 이를 설치 제거할 수 있습니다.
{:shortdesc}

이 태스크에는 다음의 CLI를 제거하기 위한 정보가 포함되어 있습니다.


-   {{site.data.keyword.containerlong_notm}} 플러그인
-   Kubernetes CLI
-   {{site.data.keyword.registryshort_notm}} 플러그인

CLI를 설치 제거하려면 다음을 수행하십시오.

1.  {{site.data.keyword.containerlong_notm}} 플러그인을 설치 제거하십시오.

    ```
    ibmcloud plugin uninstall container-service
    ```
    {: pre}

2.  {{site.data.keyword.registryshort_notm}} 플러그인을 설치 제거하십시오.

    ```
    ibmcloud plugin uninstall container-registry
    ```
    {: pre}

3.  다음 명령을 실행하고 설치된 플러그인의 목록을 확인하여 플러그인이 설치 제거되었는지 확인하십시오.

    ```
    ibmcloud plugin list
    ```
    {: pre}

    container-service 및 container-registry 플러그인이 결과에 표시되지 않습니다.

<br />


## 웹 브라우저에서 Kubernetes 터미널 사용(베타)
{: #cli_web}

Kubernetes 터미널을 사용하면 {{site.data.keyword.Bluemix_notm}} CLI를 사용하여 웹 브라우저에서 클러스터를 직접 관리할 수 있습니다.
{: shortdesc}

Kubernetes 터미널은 베타 {{site.data.keyword.containerlong_notm}} 추가 기능으로 릴리스되고 사용자 피드백 및 추가 테스트로 인해 변경될 수 있습니다. 예상치 않은 부작용을 방지하려면 프로덕션 클러스터에서 이 기능을 사용하지 마십시오.
{: important}

{{site.data.keyword.Bluemix_notm}} 콘솔에서 클러스터 대시보드를 사용하여 클러스터를 관리하지만 추가 고급 구성 변경사항을 빠르게 수행할 경우 Kubernetes 터미널의 웹 브라우저에서 직접 CLI 명령을 실행할 수 있습니다. Kubernetes 터미널은 기본 [{{site.data.keyword.Bluemix_notm}} CLI ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](/docs/cli?topic=cloud-cli-getting-started), {{site.data.keyword.containerlong_notm}} 플러그인 및 {{site.data.keyword.registryshort_notm}} 플러그인으로 사용 가능합니다. 또한 터미널 컨텍스트는 이미 Kubernetes `kubectl` 명령을 실행하여 클러스터 관련 작업을 수행할 수 있도록 작업 중인 클러스터로 이미 설정되어 있습니다.

로컬로 다운로드하고 편집하는 파일은 Kubernetes 터미널에 임시로 저장되고 세션에서 지속되지 않습니다.
{: note}

Kubernetes 터미널을 설치하고 실행하려면 다음을 수행하십시오.

1.  [{{site.data.keyword.Bluemix_notm}} 콘솔](https://cloud.ibm.com/)에 로그인하십시오.
2.  메뉴 표시줄에서 사용할 계정을 선택하십시오.
3.  메뉴 ![메뉴 아이콘](../icons/icon_hamburger.svg "메뉴 아이콘")에서 **Kubernetes**를 클릭하십시오.
4.  **클러스터** 페이지에서 액세스하려는 클러스터를 클릭하십시오.
5.  클러스터 세부사항 페이지에서 **터미널** 단추를 클릭하십시오.
6.  **설치**를 클릭하십시오. 터미널 추가 기능을 설치하는 데 몇 분 정도 걸릴 수 있습니다.
7.  **터미널** 단추를 다시 클릭하십시오. 터미널이 브라우저에서 열립니다.

그런 다음 **터미널** 단추를 클릭하면 간단히 Kubernetes 터미널을 실행할 수 있습니다.

<br />


## API로 클러스터 배치 자동화
{: #cs_api}

{{site.data.keyword.containerlong_notm}} API를 사용하여 Kubernetes 클러스터의 작성, 배치 및 관리를 자동화할 수 있습니다.
{:shortdesc}

{{site.data.keyword.containerlong_notm}} API는 헤더 정보가 필요합니다. 이 헤더 정보는 API 요청에 사용자가 제공해야 하며 사용하려는 API에 따라 다를 수 있습니다. API에 헤더 정보가 필요한지 판별하려면 [{{site.data.keyword.containerlong_notm}} API 문서 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://us-south.containers.cloud.ibm.com/swagger-api)를 참조하십시오.

{{site.data.keyword.containerlong_notm}}의 인증을 수행하려면 {{site.data.keyword.Bluemix_notm}} 인증 정보를 사용하여 생성되었으며 클러스터가 작성된 {{site.data.keyword.Bluemix_notm}} 계정 ID가 포함된 {{site.data.keyword.Bluemix_notm}} IAM(Identity and Access Management) 토큰을 제공해야 합니다. {{site.data.keyword.Bluemix_notm}}의 인증 방법에 따라 {{site.data.keyword.Bluemix_notm}} IAM 토큰의 작성을 자동화하기 위한 다음 옵션 중에서 선택할 수 있습니다.

[API Swagger JSON 파일 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://containers.cloud.ibm.com/global/swagger-global-api/swagger.json)을 사용하여 자동화 작업의 일부로서 API와 상호 작용할 수 있는 클라이언트를 생성할 수 있습니다.
{: tip}

<table summary="1열의 입력 매개변수 및 2열의 값을 사용한 ID 유형 및 옵션입니다.">
<caption>ID 유형 및 옵션</caption>
<thead>
<th>{{site.data.keyword.Bluemix_notm}} ID</th>
<th>내 옵션</th>
</thead>
<tbody>
<tr>
<td>비연합 ID</td>
<td><ul><li><strong>{{site.data.keyword.Bluemix_notm}} API 키 생성:</strong> {{site.data.keyword.Bluemix_notm}} 사용자 이름 및 비밀번호 사용의 대안으로 <a href="/docs/iam?topic=iam-userapikey#create_user_key" target="_blank">{{site.data.keyword.Bluemix_notm}} API 키를 사용</a>할 수 있습니다. {{site.data.keyword.Bluemix_notm}} API 키는 해당 API 키가 생성된 {{site.data.keyword.Bluemix_notm}} 계정에 종속됩니다. {{site.data.keyword.Bluemix_notm}} API 키를 동일한 {{site.data.keyword.Bluemix_notm}} IAM 토큰의 다른 계정 ID와 결합할 수 없습니다. {{site.data.keyword.Bluemix_notm}} API 키의 기반이 되는 계정 이외의 계정으로 작성된 클러스터에 액세스하려면 계정에 로그인하여 새 API 키를 생성해야 합니다.</li>
<li><strong>{{site.data.keyword.Bluemix_notm}} 사용자 이름 및 비밀번호:</strong> 이 주제의 단계에 따라 {{site.data.keyword.Bluemix_notm}} IAM 액세스 토큰의 작성을 완전히 자동화할 수 있습니다.</li></ul>
</tr>
<tr>
<td>연합 ID</td>
<td><ul><li><strong>{{site.data.keyword.Bluemix_notm}} API 키 생성:</strong> <a href="/docs/iam?topic=iam-userapikey#create_user_key" target="_blank">{{site.data.keyword.Bluemix_notm}} API 키</a>는 해당 API 키가 생성된 {{site.data.keyword.Bluemix_notm}} 계정에 종속됩니다. {{site.data.keyword.Bluemix_notm}} API 키를 동일한 {{site.data.keyword.Bluemix_notm}} IAM 토큰의 다른 계정 ID와 결합할 수 없습니다. {{site.data.keyword.Bluemix_notm}} API 키의 기반이 되는 계정 이외의 계정으로 작성된 클러스터에 액세스하려면 계정에 로그인하여 새 API 키를 생성해야 합니다.</li>
<li>
<strong>일회성 패스코드 사용: </strong> 일회성 패스코드를 사용하여 {{site.data.keyword.Bluemix_notm}}의 인증을 수행하는 경우에는 일회성 패스코드의 검색을 위해 웹 브라우저와의 수동 상호작용이 필요하므로 {{site.data.keyword.Bluemix_notm}} IAM 토큰 작성을 완전히 자동화할 수 없습니다. {{site.data.keyword.Bluemix_notm}} IAM 토큰 작성을 완전히 자동화하려면 대신 {{site.data.keyword.Bluemix_notm}} API 키를 작성해야 합니다.</ul></td>
</tr>
</tbody>
</table>

1.  {{site.data.keyword.Bluemix_notm}} IAM 액세스 토큰을 작성하십시오. 요청에 포함되는 본문 정보는 사용하는 {{site.data.keyword.Bluemix_notm}} 인증 방법에 따라 다릅니다.

    ```
    POST https://iam.bluemix.net/identity/token
    ```
    {: codeblock}

    <table summary="1열의 입력 매개변수 및 2열의 값을 사용하여 IAM 토큰을 검색하기 위한 입력 매개변수입니다.">
    <caption>IAM 토큰을 가져오는 입력 매개변수</caption>
    <thead>
        <th>입력 매개변수</th>
        <th>값</th>
    </thead>
    <tbody>
    <tr>
    <td>헤더</td>
    <td><ul><li>Content-Type: application/x-www-form-urlencoded</li> <li>Authorization: Basic Yng6Yng=<p><strong>참고</strong>: <code>Yng6Yng=</code>는 사용자 이름 <strong>bx</strong> 및 비밀번호 <strong>bx</strong>에 대한 URL 인코딩된 권한과 동일합니다.</p></li></ul>
    </td>
    </tr>
    <tr>
    <td>{{site.data.keyword.Bluemix_notm}} 사용자 이름 및 비밀번호에 대한 본문</td>
    <td><ul><li>`grant_type: password`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`username`: 사용자의 {{site.data.keyword.Bluemix_notm}} 사용자 이름입니다.</li>
    <li>`password`: 사용자의 {{site.data.keyword.Bluemix_notm}} 비밀번호입니다.</li>
    <li>`uaa_client_id: cf`</li>
    <li>`uaa_client_secret:`</br><strong>참고</strong>: 지정된 값이 없는 <code>uaa_client_secret</code> 키를 추가하십시오.</li></ul></td>
    </tr>
    <tr>
    <td>{{site.data.keyword.Bluemix_notm}} API 키에 대한 본문</td>
    <td><ul><li>`grant_type: urn:ibm:params:oauth:grant-type:apikey`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`apikey`: 사용자의 {{site.data.keyword.Bluemix_notm}} API 키입니다.</li>
    <li>`uaa_client_id: cf`</li>
    <li>`uaa_client_secret:` </br><strong>참고</strong>: 지정된 값이 없는 <code>uaa_client_secret</code> 키를 추가하십시오.</li></ul></td>
    </tr>
    <tr>
    <td>{{site.data.keyword.Bluemix_notm}} 일회성 패스코드에 대한 본문</td>
      <td><ul><li><code>grant_type: urn:ibm:params:oauth:grant-type:passcode</code></li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`passcode`: 사용자의 {{site.data.keyword.Bluemix_notm}} 일회성 패스코드입니다. `ibmcloud login --sso`를 실행하고 CLI 출력의 지시사항에 따라 웹 브라우저를 사용하여 일회성 패스코드를 검색하십시오.</li>
    <li>`uaa_client_id: cf`</li>
    <li>`uaa_client_secret:` </br><strong>참고</strong>: 지정된 값이 없는 <code>uaa_client_secret</code> 키를 추가하십시오.</li></ul>
    </td>
    </tr>
    </tbody>
    </table>

    API 키 사용의 출력 예:

    ```
    {
    "access_token": "<iam_access_token>",
    "refresh_token": "<iam_refresh_token>",
    "uaa_token": "<uaa_token>",
    "uaa_refresh_token": "<uaa_refresh_token>",
    "token_type": "Bearer",
    "expires_in": 3600,
    "expiration": 1493747503
    "scope": "ibm openid"
    }

    ```
    {: screen}

    API 출력의 **access_token** 필드에서 {{site.data.keyword.Bluemix_notm}} IAM 토큰을 찾을 수 있습니다. 다음 단계에서 추가 헤더 정보를 검색할 수 있도록 {{site.data.keyword.Bluemix_notm}} IAM 토큰을 기록해 두십시오.

2.  작업하려는 {{site.data.keyword.Bluemix_notm}} 계정의 ID를 검색하십시오. `<iam_access_token>`을 이전 단계에 있는 API 출력의 **access_token** 필드에서 검색한 {{site.data.keyword.Bluemix_notm}} IAM 토큰으로 대체하십시오. API 출력의 **resources/metadata/guid** 필드에서 {{site.data.keyword.Bluemix_notm}} 계정의 ID를 찾을 수 있습니다.

    ```
    GET https://accountmanagement.ng.bluemix.net/v1/accounts
    ```
    {: codeblock}

    <table summary="1열의 입력 매개변수 및 2열의 값을 사용하여 {{site.data.keyword.Bluemix_notm}} 계정 ID를 가져오는 입력 매개변수입니다.">
    <caption>{{site.data.keyword.Bluemix_notm}} 계정 ID를 가져오는 입력 매개변수</caption>
    <thead>
  	<th>입력 매개변수</th>
  	<th>값</th>
    </thead>
    <tbody>
  	<tr>
  		<td>헤더</td>
      <td><ul><li><code>Content-Type: application/json</code></li>
        <li>`Authorization: bearer <iam_access_token>`</li>
        <li><code>Accept: application/json</code></li></ul></td>
  	</tr>
    </tbody>
    </table>

    출력 예:

    ```
    {
      "total_results": 3,
      "total_pages": 1,
      "prev_url": null,
      "next_url": null,
      "resources":
        {
          "metadata": {
            "guid": "<account_ID>",
            "url": "/v1/accounts/<account_ID>",
            "created_at": "2016-01-07T18:55:09.726Z",
            "updated_at": "2017-04-28T23:46:03.739Z",
            "origin": "BSS"
    ...
    ```
    {: screen}

3.  작업하려는  {{site.data.keyword.Bluemix_notm}} 인증 정보 및 계정 ID가 포함된 새 {{site.data.keyword.Bluemix_notm}} IAM 토큰을 생성하십시오.

    {{site.data.keyword.Bluemix_notm}} API 키를 사용하는 경우에는 API 키가 작성된 {{site.data.keyword.Bluemix_notm}} 계정 ID를 사용해야 합니다. 다른 계정에서 클러스터에 액세스하려면 이 계정에 로그인하고 이 계정을 기반으로 하는 {{site.data.keyword.Bluemix_notm}} API 키를 작성하십시오.
    {: note}

    ```
    POST https://iam.bluemix.net/identity/token
    ```
    {: codeblock}

    <table summary="1열의 입력 매개변수 및 2열의 값을 사용하여 IAM 토큰을 검색하기 위한 입력 매개변수입니다.">
    <caption>IAM 토큰을 가져오는 입력 매개변수</caption>
    <thead>
        <th>입력 매개변수</th>
        <th>값</th>
    </thead>
    <tbody>
    <tr>
    <td>헤더</td>
    <td><ul><li>`Content-Type: application/x-www-form-urlencoded`</li> <li>`Authorization: Basic Yng6Yng=`<p><strong>참고</strong>: <code>Yng6Yng=</code>는 사용자 이름 <strong>bx</strong> 및 비밀번호 <strong>bx</strong>에 대한 URL 인코딩된 권한과 동일합니다.</p></li></ul>
    </td>
    </tr>
    <tr>
    <td>{{site.data.keyword.Bluemix_notm}} 사용자 이름 및 비밀번호에 대한 본문</td>
    <td><ul><li>`grant_type: password`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`username`: 사용자의 {{site.data.keyword.Bluemix_notm}} 사용자 이름입니다. </li>
    <li>`password`: 사용자의 {{site.data.keyword.Bluemix_notm}} 비밀번호입니다. </li>
    <li>`uaa_client_ID: cf`</li>
    <li>`uaa_client_secret:` </br><strong>참고</strong>: 지정된 값이 없는 <code>uaa_client_secret</code> 키를 추가하십시오.</li>
    <li>`bss_account`: 이전 단계에서 검색한 {{site.data.keyword.Bluemix_notm}} 계정 ID입니다.</li></ul>
    </td>
    </tr>
    <tr>
    <td>{{site.data.keyword.Bluemix_notm}} API 키에 대한 본문</td>
    <td><ul><li>`grant_type: urn:ibm:params:oauth:grant-type:apikey`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`apikey`: 사용자의 {{site.data.keyword.Bluemix_notm}} API 키입니다.</li>
    <li>`uaa_client_ID: cf`</li>
    <li>`uaa_client_secret:` </br><strong>참고</strong>: 지정된 값이 없는 <code>uaa_client_secret</code> 키를 추가하십시오.</li>
    <li>`bss_account`: 이전 단계에서 검색한 {{site.data.keyword.Bluemix_notm}} 계정 ID입니다.</li></ul>
      </td>
    </tr>
    <tr>
    <td>{{site.data.keyword.Bluemix_notm}} 일회성 패스코드에 대한 본문</td>
    <td><ul><li>`grant_type: urn:ibm:params:oauth:grant-type:passcode`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`passcode`: 사용자의 {{site.data.keyword.Bluemix_notm}} 패스코드입니다. </li>
    <li>`uaa_client_ID: cf`</li>
    <li>`uaa_client_secret:` </br><strong>참고</strong>: 지정된 값이 없는 <code>uaa_client_secret</code> 키를 추가하십시오.</li>
    <li>`bss_account`: 이전 단계에서 검색한 {{site.data.keyword.Bluemix_notm}} 계정 ID입니다.</li></ul></td>
    </tr>
    </tbody>
    </table>

    출력 예:

    ```
    {
      "access_token": "<iam_token>",
      "refresh_token": "<iam_refresh_token>",
      "token_type": "Bearer",
      "expires_in": 3600,
      "expiration": 1493747503
    }

    ```
    {: screen}

    **access_token**에서 {{site.data.keyword.Bluemix_notm}} IAM 토큰을 찾고 API 출력의 **refresh_token** 필드에서 새로 고치기 토큰을 찾을 수 있습니다.

4.  사용 가능한 {{site.data.keyword.containerlong_notm}} 지역을 나열하고 작업하려는 지역을 선택하십시오. 이전 단계에서 IAM 액세스 토큰 및 새로 고치기 토큰을 사용하여 헤더 정보를 빌드하십시오.
    ```
    GET https://containers.cloud.ibm.com/v1/regions
    ```
    {: codeblock}

    <table summary="1열의 입력 매개변수 및 2열의 값을 사용하여 {{site.data.keyword.containerlong_notm}} 지역을 검색하는 입력 매개변수입니다.">
    <caption>{{site.data.keyword.containerlong_notm}} 지역을 검색하는 입력 매개변수</caption>
    <thead>
    <th>입력 매개변수</th>
    <th>값</th>
    </thead>
    <tbody>
    <tr>
    <td>헤더</td>
    <td><ul><li>`Authorization: bearer <iam_token>`</li>
    <li>`X-Auth-Refresh-Token: <refresh_token>`</li></ul></td>
    </tr>
    </tbody>
    </table>

    출력 예:
    ```
    {
    "regions": [
        {
            "name": "ap-north",
            "alias": "jp-tok",
            "cfURL": "api.au-syd.bluemix.net",
            "freeEnabled": false
        },
        {
            "name": "ap-south",
            "alias": "au-syd",
            "cfURL": "api.au-syd.bluemix.net",
            "freeEnabled": true
        },
        {
            "name": "eu-central",
            "alias": "eu-de",
            "cfURL": "api.eu-de.bluemix.net",
            "freeEnabled": true
        },
        {
            "name": "uk-south",
            "alias": "eu-gb",
            "cfURL": "api.eu-gb.bluemix.net",
            "freeEnabled": true
        },
        {
            "name": "us-east",
            "alias": "us-east",
            "cfURL": "api.ng.bluemix.net",
            "freeEnabled": false
        },
        {
            "name": "us-south",
            "alias": "us-south",
            "cfURL": "api.ng.bluemix.net",
            "freeEnabled": true
        }
    ]
    }
    ```
    {: screen}

5.  선택한 {{site.data.keyword.containerlong_notm}} 지역의 모든 클러스터를 나열하십시오. [클러스터에 대한 Kubernetes API 요청을 실행](#kube_api)하려면 클러스터의 **ID** 및 **지역**을 기록해 두십시오.

     ```
     GET https://containers.cloud.ibm.com/v1/clusters
     ```
     {: codeblock}

     <table summary="1열의 입력 매개변수 및 2열의 값을 사용하여 {{site.data.keyword.containerlong_notm}} API와 작동하는 입력 매개변수입니다.">
     <caption>{{site.data.keyword.containerlong_notm}} API와 작동하는 입력 매개변수</caption>
     <thead>
     <th>입력 매개변수</th>
     <th>값</th>
     </thead>
     <tbody>
     <tr>
     <td>헤더</td>
     <td><ul><li>`Authorization: bearer <iam_token>`</li>
       <li>`X-Auth-Refresh-Token: <refresh_token>`</li><li>`X-Region: <region>`</li></ul></td>
     </tr>
     </tbody>
     </table>

5.  지원되는 API 목록은 [{{site.data.keyword.containerlong_notm}} API 문서 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://containers.cloud.ibm.com/global/swagger-global-api)를 검토하십시오.

<br />


## Kubernetes API를 사용하여 클러스터 관련 작업 수행
{: #kube_api}

[Kubernetes API ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/reference/using-api/api-overview/)를 사용하여 {{site.data.keyword.containerlong_notm}}의 클러스터와 상호작용할 수 있습니다.
{: shortdesc}

다음 지시사항에는 Kubernetes 마스터의 공용 서비스 엔드포인트에 대해 연결하기 위한 클러스터의 공용 네트워크 액세스 권한이 필요합니다.
{: note}

1. [API로 클러스터 배치 자동화](#cs_api)의 단계를 수행하여 {{site.data.keyword.Bluemix_notm}} IAM 액세스 토큰, 새로 고치기 토큰, Kubernetes API 요청을 실행할 클러스터의 ID 및 클러스터가 위치한 {{site.data.keyword.containerlong_notm}} 지역을 검색하십시오.

2. {{site.data.keyword.Bluemix_notm}} IAM 위임 새로 고치기 토큰을 검색하십시오.
   ```
    POST https://iam.bluemix.net/identity/token
   ```
   {: codeblock}

   <table summary="1열의 입력 매개변수 및 2열의 값을 사용하여 IAM 위임 새로 고치기 토큰을 가져오는 입력 매개변수입니다.">
   <caption>IAM 위임 새로 고치기 토큰을 가져오는 입력 매개변수 </caption>
   <thead>
   <th>입력 매개변수</th>
   <th>값</th>
   </thead>
   <tbody>
   <tr>
   <td>헤더</td>
   <td><ul><li>`Content-Type: application/x-www-form-urlencoded`</li> <li>`Authorization: Basic Yng6Yng=`</br><strong>참고</strong>: <code>Yng6Yng=</code>는 사용자 이름 <strong>bx</strong> 및 비밀번호 <strong>bx</strong>에 대한 URL 인코딩된 권한과 동일합니다.</li><li>`cache-control: no-cache`</li></ul>
   </td>
   </tr>
   <tr>
   <td>본문</td>
   <td><ul><li>`delegated_refresh_token_expiry: 600`</li>
   <li>`receiver_client_ids: kube`</li>
   <li>`response_type: delegated_refresh_token` </li>
   <li>`refresh_token`: 사용자의 {{site.data.keyword.Bluemix_notm}} IAM 새로 고치기 토큰입니다. </li>
   <li>`grant_type: refresh_token`</li></ul></td>
   </tr>
   </tbody>
   </table>

   출력 예:
   ```
   {
    "delegated_refresh_token": <delegated_refresh_token>
   }
   ```
   {: screen}

3. 이전 단계의 위임된 새로 고치기 토큰을 사용하여 {{site.data.keyword.Bluemix_notm}} IAM ID, IAM 액세스 및 IAM 새로 고치기 토큰을 검색하십시오. API 출력에서는 **id_token** 필드에서 IAM ID 토큰을 찾고, **access_token** 필드에서 IAM 액세스 토큰을 찾고, **refresh_token** 필드에서 IAM 새로 고치기 토큰을 찾을 수 있습니다.
   ```
    POST https://iam.bluemix.net/identity/token
   ```
   {: codeblock}

   <table summary="1열의 입력 매개변수 및 2열의 값을 사용하여 IAM ID 및 IAM 액세스 토큰을 가져오는 입력 매개변수입니다.">
   <caption>IAM ID 및 IAM 액세스 토큰을 가져오는 입력 매개변수</caption>
   <thead>
   <th>입력 매개변수</th>
   <th>값</th>
   </thead>
   <tbody>
   <tr>
   <td>헤더</td>
   <td><ul><li>`Content-Type: application/x-www-form-urlencoded`</li> <li>`Authorization: Basic a3ViZTprdWJl`</br><strong>참고</strong>: <code>a3ViZTprdWJl</code>는 사용자 이름 <strong><code>kube</code></strong> 및 비밀번호 <strong><code>kube</code></strong>에 대한 URL 인코딩된 권한과 동일합니다.</li><li>`cache-control: no-cache`</li></ul>
   </td>
   </tr>
   <tr>
   <td>본문</td>
   <td><ul><li>`refresh_token`: {{site.data.keyword.Bluemix_notm}} IAM 위임 새로 고치기 토큰입니다. </li>
   <li>`grant_type: urn:ibm:params:oauth:grant-type:delegated-refresh-token`</li></ul></td>
   </tr>
   </tbody>
   </table>

   출력 예:
   ```
   {
    "access_token": "<iam_access_token>",
    "id_token": "<iam_id_token>",
    "refresh_token": "<iam_refresh_token>",
    "token_type": "Bearer",
    "expires_in": 3600,
    "expiration": 1553629664,
    "scope": "ibm openid containers-kubernetes"
   }
   ```
   {: screen}

4. IAM 액세스 토큰, IAM ID 토큰, IAM 새로 고치기 토큰 및 클러스터가 있는 {{site.data.keyword.containerlong_notm}} 지역을 사용하여 Kubernetes 마스터의 공용 URL을 검색하십시오. API 출력의 **`publicServiceEndpointURL`**에서 URL을 찾을 수 있습니다.
   ```
   GET https://containers.cloud.ibm.com/v1/clusters/<cluster_ID>
   ```
   {: codeblock}

   <table summary="1열의 입력 매개변수 및 2열의 값을 사용하여 Kubernetes 마스터에 대한 공용 서비스 엔드포인트를 가져오는 입력 매개변수입니다.">
   <caption>Kubernetes 마스터의 공용 서비스 엔드포인트를 가져오는 입력 매개변수</caption>
   <thead>
   <th>입력 매개변수</th>
   <th>값</th>
   </thead>
   <tbody>
   <tr>
   <td>헤더</td>
     <td><ul><li>`Authorization`: 사용자의 {{site.data.keyword.Bluemix_notm}} IAM 액세스 토큰입니다.</li><li>`X-Auth-Refresh-Token`: 사용자의 {{site.data.keyword.Bluemix_notm}} IAM 새로 고치기 토큰입니다.</li><li>`X-Region`: [API로 클러스터 배치 자동화](#cs_api)에서 `GET https://containers.cloud.ibm.com/v1/clusters` API로 검색한 클러스터의 {{site.data.keyword.containerlong_notm}} 지역입니다. </li></ul>
   </td>
   </tr>
   <tr>
   <td>경로</td>
   <td>`<cluster_ID>:`: [API로 클러스터 배치 자동화](#cs_api)에서 `GET https://containers.cloud.ibm.com/v1/clusters` API로 검색한 클러스터의 ID입니다.      </td>
   </tr>
   </tbody>
   </table>

   출력 예:
   ```
   {
    "location": "Dallas",
    "dataCenter": "dal10",
    "multiAzCapable": true,
    "vlans": null,
    "worker_vlans": null,
    "workerZones": [
        "dal10"
    ],
    "id": "1abc123b123b124ab1234a1a12a12a12",
    "name": "mycluster",
    "region": "us-south",
    ...
    "publicServiceEndpointURL": "https://c7.us-south.containers.cloud.ibm.com:27078"
   }
   ```
   {: screen}

5. 이전에 검색한 IAM ID 토큰을 사용하여 클러스터에 대한 Kubernetes API 요청을 실행하십시오. 예를 들어, 클러스터에서 실행되는 Kubernetes 버전을 나열하십시오.

   API 테스트 프레임워크에서 SSL 인증서 확인을 사용으로 설정한 경우 이 기능을 사용 안함으로 설정해야 합니다.
   {: tip}

   ```
   GET <publicServiceEndpointURL>/version
   ```
   {: codeblock}

   <table summary="1열의 입력 매개변수 및 2열의 값을 사용하여 클러스터에서 실행되는 Kubernetes 버전을 보는 입력 매개변수입니다.">
   <caption>클러스터에서 실행되는 Kubernetes 버전을 보는 입력 매개변수 </caption>
   <thead>
   <th>입력 매개변수</th>
   <th>값</th>
   </thead>
   <tbody>
   <tr>
   <td>헤더</td>
   <td>`Authorization: bearer <id_token>`</td>
   </tr>
   <tr>
   <td>경로</td>
   <td>`<publicServiceEndpointURL>`: 이전 단계에서 검색한 Kubernetes 마스터의 **`publicServiceEndpointURL`**입니다.      </td>
   </tr>
   </tbody>
   </table>

   출력 예:
   ```
   {
    "major": "1",
    "minor": "13",
    "gitVersion": "v1.13.4+IKS",
    "gitCommit": "c35166bd86eaa91d17af1c08289ffeab3e71e11e",
    "gitTreeState": "clean",
    "buildDate": "2019-03-21T10:08:03Z",
    "goVersion": "go1.11.5",
    "compiler": "gc",
    "platform": "linux/amd64"
   }
   ```
   {: screen}

6. 최신 Kubernetes 버전에 대해 지원되는 API 목록을 찾으려면 [Kubernetes API 문서 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크아이콘 ")](https://kubernetes.io/docs/reference/kubernetes-api/)를 검토하십시오. 클러스터의 Kubernetes 버전과 일치하는 API 문서를 사용해야 합니다. 최신 Kubernetes 버전을 사용하지 않는 경우 URL 끝에 버전을 추가하십시오. 예를 들어, 버전 1.12용 API 문서에 액세스하려면 `v1.12`를 추가하십시오.


## API를 사용하여 {{site.data.keyword.Bluemix_notm}} IAM 액세스 토큰을 새로 고치고 새 새로 고치기 토큰 가져오기
{: #cs_api_refresh}

API를 통해 발행된 모든 {{site.data.keyword.Bluemix_notm}} IAM(Identity and Access Management) 액세스 토큰은 한 시간 후에 만료됩니다. {{site.data.keyword.Bluemix_notm}} API에 대한 액세스가 보장되도록 사용자는 정기적으로 액세스 토큰을 새로 고쳐야 합니다. 동일한 단계를 사용하여 새 새로 고치기 토큰을 얻을 수 있습니다.
{:shortdesc}

시작하기 전에, 새 액세스 토큰의 요청에 사용할 수 있는 {{site.data.keyword.Bluemix_notm}} API 키 또는 {{site.data.keyword.Bluemix_notm}} IAM 새로 고치기 토큰이 있는지 확인하십시오.
- **새로 고치기 토큰:** [{{site.data.keyword.Bluemix_notm}} API로 클러스터 작성 및 관리 프로세스 자동화](#cs_api)의 지시사항을 따르십시오.
- **API 키:** 다음과 같이 [{{site.data.keyword.Bluemix_notm}} ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/) API 키를 검색하십시오.
   1. 메뉴 표시줄에서 **관리** > **액세스(IAM)**를 클릭하십시오.
   2. **사용자** 페이지를 클릭한 후 자신을 선택하십시오.
   3. **API 키** 분할창에서 **IBM Cloud API 키 작성**을 클릭하십시오.
   4. API 키의 **이름** 및 **설명**을 입력하고 **작성**을 클릭하십시오.
   4. **표시**를 클릭하여 생성된 API를 보십시오.
   5. 새 {{site.data.keyword.Bluemix_notm}} IAM 액세스 토큰의 검색에 사용할 수 있도록 API 키를 복사하십시오.

{{site.data.keyword.Bluemix_notm}} IAM 토큰을 작성하거나 새 새로 고치기 토큰을 얻으려는 경우에는 다음 단계를 사용하십시오.

1.  새로 고치기 토큰이나 {{site.data.keyword.Bluemix_notm}} API 키를 사용하여 새 {{site.data.keyword.Bluemix_notm}} IAM 액세스 토큰을 생성하십시오.
    ```
    POST https://iam.bluemix.net/identity/token
    ```
    {: codeblock}

    <table summary="1열의 입력 매개변수 및 2열의 값을 사용한 새 IAM 토큰의 입력 매개변수입니다.">
    <caption>새 {{site.data.keyword.Bluemix_notm}} IAM 토큰의 입력 매개변수</caption>
    <thead>
    <th>입력 매개변수</th>
    <th>값</th>
    </thead>
    <tbody>
    <tr>
    <td>헤더</td>
    <td><ul><li>`Content-Type: application/x-www-form-urlencoded`</li>
      <li>`Authorization: Basic Yng6Yng=`</br></br><strong>참고:</strong> <code>Yng6Yng=</code>은 사용자 이름 <strong>bx</strong>와 비밀번호 <strong>bx</strong>에 대한 URL 인코딩된 권한과 동일합니다.</li></ul></td>
    </tr>
    <tr>
    <td>새로 고치기 토큰을 사용하는 경우의 본문</td>
    <td><ul><li>`grant_type: refresh_token`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`refresh_token:` 사용자의 {{site.data.keyword.Bluemix_notm}} IAM 새로 고치기 토큰입니다. </li>
    <li>`uaa_client_ID: cf`</li>
    <li>`uaa_client_secret:`</li>
    <li>`bss_account:` 사용자의 {{site.data.keyword.Bluemix_notm}} 계정 ID입니다. </li></ul><strong>참고</strong>: 지정된 값이 없는 <code>uaa_client_secret</code> 키를 추가하십시오.</td>
    </tr>
    <tr>
      <td>{{site.data.keyword.Bluemix_notm}} API 키를 사용하는 경우의 본문</td>
      <td><ul><li>`grant_type: urn:ibm:params:oauth:grant-type:apikey`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`apikey:` 사용자의 {{site.data.keyword.Bluemix_notm}} API 키입니다. </li>
    <li>`uaa_client_ID: cf`</li>
        <li>`uaa_client_secret:`</li></ul><strong>참고:</strong> 지정된 값이 없는 <code>uaa_client_secret</code> 키를 추가하십시오.</td>
    </tr>
    </tbody>
    </table>

    API 출력 예:

    ```
    {
      "access_token": "<iam_token>",
      "refresh_token": "<iam_refresh_token>",
      "uaa_token": "<uaa_token>",
      "uaa_refresh_token": "<uaa_refresh_token>",
      "token_type": "Bearer",
      "expires_in": 3600,
      "expiration": 1493747503
    }

    ```
    {: screen}

    **access_token**에서 새 {{site.data.keyword.Bluemix_notm}} IAM 토큰을 찾고 API 출력의 **refresh_token** 필드에서 새로 고치기 토큰을 찾을 수 있습니다.

2.  이전 단계의 토큰을 사용하여 [{{site.data.keyword.containerlong_notm}} API 문서 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://containers.cloud.ibm.com/global/swagger-global-api)에 대한 작업을 진행하십시오.

<br />


## CLI를 사용하여 {{site.data.keyword.Bluemix_notm}} IAM 액세스 토큰을 새로 고치고 새 새로 고치기 토큰 가져오기
{: #cs_cli_refresh}

새 CLI 세션을 시작하거나 현재 CLI 세션에서 24시간이 만료된 경우에는 `ibmcloud ks cluster-config --cluster <cluster_name>`을 실행하여 클러스터의 컨텍스트를 설정해야 합니다. 이 명령으로 클러스터의 컨텍스트를 설정하면 Kubernetes 클러스터의 `kubeconfig` 파일이 다운로드됩니다. 또한 {{site.data.keyword.Bluemix_notm}} IAM(Identity and Access Management) ID 토큰과 새로 고치기 토큰이 인증 제공을 위해 발행됩니다.
{: shortdesc}

**ID 토큰**: CLI를 통해 발행된 모든 IAM ID 토큰은 한 시간 후에 만료됩니다. ID 토큰이 만료되면 ID 토큰 새로 고치기를 위해 새로 고치기 토큰이 토큰 제공자에게 전송됩니다. 사용자의 인증이 새로 고쳐지며 사용자는 계속해서 클러스터에 대해 명령을 실행할 수 있습니다.

**새로 고치기 토큰**: 새로 고치기 토큰은 30일마다 만료됩니다. 새로 고치기 토큰이 만료되면 ID 토큰을 새로 고칠 수 없으며 사용자는 CLI에서 명령 실행을 계속할 수 없습니다. `ibmcloud ks cluster-config --cluster <cluster_name>`을 실행하여 새 새로 고치기 토큰을 가져올 수 있습니다. 또한 이 명령은 ID 토큰을 새로 고칩니다.
