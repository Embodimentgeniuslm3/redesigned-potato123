---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: kubernetes, iks, oks, iro, openshift, red hat, red hat openshift, rhos

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

# 튜토리얼: Red Hat OpenShift on IBM Cloud 클러스터(베타) 작성
{: #openshift_tutorial}

Red Hat OpenShift on IBM Cloud는 OpenShift 클러스터를 테스트하기 위한 베타로서 사용 가능합니다. 베타 기간 중에는 {{site.data.keyword.containerlong}}의 모든 기능을 사용할 수는 없습니다. 또한 작성하는 OpenShift 베타 클러스터는 베타 종료 후 30일 동안만 유지되며 Red Hat OpenShift on IBM Cloud는 GA(General Availability)됩니다.
{: preview}

**Red Hat OpenShift on IBM Cloud 베타**를 통해 OpenShift 컨테이너 오케스트레이션 플랫폼 소프트웨어가 설치된 상태에서 제공되는 작업자 노드가 포함된 {{site.data.keyword.containerlong_notm}} 클러스터를 작성할 수 있습니다. 앱 배치를 위해 Red Hat Enterprise Linux에 실행되는 [OpenShift 도구 및 카탈로그 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.openshift.com/container-platform/3.11/welcome/index.html)와 함께 클러스터 인프라 환경에 적합한 모든 [관리 {{site.data.keyword.containerlong_notm}}의 이점](/docs/containers?topic=containers-responsibilities_iks)을 누리십시오.
{: shortdesc}

OpenShift 작업자 노드는 표준 클러스터에만 사용할 수 있습니다. Red Hat OpenShift on IBM Cloud는 Kubernetes 버전 1.11을 포함한 OpenShift 버전 3.11만 지원합니다.
{: note}

## 목표
{: #openshift_objectives}

이 튜토리얼 학습에서 외부 사용자가 서비스에 액세스할 수 있도록 표준 Red Hat OpenShift on IBM Cloud 클러스터를 작성하고, OpenShift 콘솔을 열고, 기본 제공 OpenShift 컴포넌트에 액세스하고, OpenShift 프로젝트에서 {{site.data.keyword.Bluemix_notm}} 서비스를 사용하는 앱을 배치하고, OpenShift 라우트에 앱을 노출합니다.
{: shortdesc}

이 페이지에는 OpenShift 클러스터 아키텍처에 대한 정보, 베타 제한사항 및 피드백을 제공하고 도움을 받는 방법도 포함되어 있습니다. 

## 소요 시간
{: #openshift_time}
45분

## 대상
{: #openshift_audience}

이 튜토리얼은 Red Hat OpenShift on IBM Cloud 클러스터 작성 방법을 알아보려고 하는 클러스터 관리자를 대상으로 합니다.
{: shortdesc}

## 전제조건
{: #openshift_prereqs}

*   다음 {{site.data.keyword.Bluemix_notm}} IAM 액세스 정책이 있는지 확인하십시오. 
    *   {{site.data.keyword.containerlong_notm}}의 [**관리자** 플랫폼 역할](/docs/containers?topic=containers-users#platform)
    *   {{site.data.keyword.containerlong_notm}}의 [**작성자** 또는 **관리자** 서비스 역할](/docs/containers?topic=containers-users#platform)
    *   {{site.data.keyword.registrylong_notm}}의 [**관리자** 플랫폼 역할](/docs/containers?topic=containers-users#platform)
*    {{site.data.keyword.Bluemix_notm}} 지역 및 리소스 그룹의 [API 키](/docs/containers?topic=containers-users#api_key)가 올바른 인프라 권한 즉, **수퍼유저** 또는 클러스터를 작성할 수 있는 [최소한의 역할](/docs/containers?topic=containers-access_reference#infra)로 설정되는지 확인하십시오. 
*   명령행 도구를 설치하십시오. 
    *   [{{site.data.keyword.Bluemix_notm}} CLI(`ibmcloud`), {{site.data.keyword.containershort_notm}} 플러그인(`ibmcloud ks`) 및 {{site.data.keyword.registryshort_notm}} 플러그인(`ibmcloud cr`)](/docs/containers?topic=containers-cs_cli_install#cs_cli_install_steps)을 설치하십시오.
    *   [OpenShift Origin(`oc`) 및 Kubernetes (`kubectl`) CLI](/docs/containers?topic=containers-cs_cli_install#cli_oc)를 설치하십시오.

<br />


## 아키텍처 개요
{: #openshift_architecture}

다음 다이어그램 및 표에서는 Red Hat OpenShift on IBM Cloud 아키텍처에서 설정되는 기본 컴포넌트에 대해 설명합니다.
{: shortdesc}

![Red Hat OpenShift on IBM Cloud 클러스터 아키텍처](images/cs_org_ov_both_ses_rhos.png)

| 마스터 컴포넌트 | 설명 |
|:-----------------|:-----------------|
| 복제본 | OpenShift Kubernetes API 서버 및 etcd 데이터 저장소를 포함한 마스터 컴포넌트에는 세 개의 복제본이 있으며 보다 높은 가용성을 위해 구역에 분산됩니다(다중 지역 메트로에 위치한 경우). 마스터 컴포넌트는 8시간마다 백업됩니다. |
| `rhos-api` | Kubernetes API 서버는 작업자 노드에서 마스터로의 모든 클러스터 관리 요청에 대한 기본 시작점 역할을 수행합니다. API 서버는 팟(Pod) 또는 서비스 등의 Kubernetes 리소스의 상태를 변경하는 요청을 유효성 검증하고 처리하며, 이 상태를 etcd 데이터 저장소에 저장합니다.|
| `openvpn-server` | OpenVPN 서버는 OpenVPN 클라이언트와 함께 작업하여 마스터를 작업자 노드에 안전하게 연결합니다. 이 연결은 팟(Pod) 및 서비스에 대한 `apiserver proxy` 호출과 kubelet에 대한 `kubectl exec`, `attach` 및 `logs` 호출을 지원합니다.|
| `etcd` | etcd는 서비스, 배치 및 팟(Pod) 등 클러스터의 모든 Kubernetes 리소스의 상태를 저장하는 고가용성의 키 값 저장소입니다. etcd의 데이터는 IBM이 관리하는 암호화된 스토리지 인스턴스에 백업됩니다.|
| `rhos-controller` | OpenShift 제어기 관리자는 새로 작성된 팟(Pod)을 감시하고 용량, 성능 요구사항, 정책 제한조건, 반친화성 스펙 및 워크로드 요구사항을 기반으로 이를 배치할 위치를 결정합니다. 요구사항과 일치하는 작업자 노드를 찾을 수 없으면 팟(Pod)이 클러스터에 배치되지 않습니다. 제어기는 복제본 세트 등의 클러스터 리소스의 상태를 감시합니다. 리소스의 상태가 변경되는 경우(예: 복제본 세트의 팟(Pod)이 작동 중지됨), 제어기 관리자는 정정 조치를 시작하여 필수 상태를 얻습니다. `rhos-controller`는 기본 Kubernetes 구성에서 스케줄러 및 제어기 관리자로 작동합니다. |
| `cloud-controller-manager` | 클라우드 제어기 관리자는 클라우드 제공자별 컴포넌트(예: {{site.data.keyword.Bluemix_notm}} 로드 밸런서)를 관리합니다. |
{: caption="표 1. OpenShift 마스터 컴포넌트." caption-side="top"}
{: #rhos-components-1}
{: tab-title="Master"}
{: tab-group="rhos-components"}
{: class="simple-tab-table"}

| 작업자 노드 컴포넌트 | 설명 |
|:-----------------|:-----------------|
| 운영 체제 | Red Hat OpenShift on IBM Cloud 작업자 노드는 Red Hat Enterprise Linux 7(RHEL 7) 운영 체제에서 실행됩니다. |
| 프로젝트 | OpenShift는 리소스를 어노테이션이 포함된 Kubernetes 네임스페이스인 프로젝트로 구성하며, 기본 Kubernetes 클러스터보다 훨씬 더 많은 컴포넌트를 포함하여 OpenShift 기능(예: 카탈로그)을 실행합니다. 프로젝트의 컴포넌트 선택은 다음 행에 설명되어 있습니다. 자세한 정보는 [프로젝트 및 사용자 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.openshift.com/container-platform/3.11/architecture/core_concepts/projects_and_users.html)를 참조하십시오.|
| `kube-system` | 이 네임스페이스에는 작업자 노드에서 Kubernetes를 실행하는 데 사용되는 많은 컴포넌트가 포함되어 있습니다. <ul><li>**`ibm-master-proxy`**: `ibm-master-proxy`는 작업자 노드에서 고가용성 마스터 복제본의 IP 주소로 요청을 전달하는 디먼 세트입니다. 단일 구역 클러스터의 마스터에는 3개의 복제본이 있습니다. 다중 구역 가능 구역에 있는 클러스터의 경우, 마스터에는 구역 간에 전개된 3개의 복제본이 있습니다. 고가용성 로드 밸런서는 마스터 도메인 이름에 대한 요청을 마스터 복제본에 전달합니다. </li><li>**`openvpn-client`**: OpenVPN 클라이언트는 OpenVPN 서버와 함께 작업하여 마스터를 작업자 노드에 안전하게 연결합니다. 이 연결은 팟(Pod) 및 서비스에 대한 `apiserver proxy` 호출과 kubelet에 대한 `kubectl exec`, `attach` 및 `logs` 호출을 지원합니다.</li><li>**`kubelet`**: kubelet는 모든 작업자 노드에서 실행되는 작업자 노드 에이전트이며, 작업자 노드에서 실행되는 팟(Pod)의 상태를 모니터링하고 Kubernetes API 서버가 전송하는 이벤트를 감시하는 역할을 담당합니다. 이벤트를 기반으로, kubelet는 팟(Pod)을 작성 또는 제거하고 활동 상태 및 준비 상태 프로브를 보장하며 팟(Pod)의 상태를 다시 Kubernetes API 서버에 보고합니다.</li><li>**`calico`**: Calico는 클러스터에 대한 네트워크 정책을 관리하고, 컨테이너 네트워크 연결, IP 주소 지정 및 네트워크 트래픽 제어를 관리하도록 일부 컴포넌트를 포함합니다. </li><li>**기타 컴포넌트**: `kube-system` 네임스페이스는 IBM 제공 리소스(예: 파일 및 블록 스토리지에 대한 스토리지 플러그인, Ingress 애플리케이션 로드 밸런서(ALB), `fluentd` 로깅 및 `keepalived`)를 관리하도록 컴포넌트도 포함합니다. </li></ul>|
| `ibm-system` | 이 네임스페이스에는 앱 팟(Pod)에 대한 요청을 위해 상태 검사 및 계층 4 로드 밸런싱을 제공하도록 `keepalived`에 대해 작업하는  `ibm-cloud-provider-ip` 배치도 포함됩니다. |
| `kube-proxy-and-dns`| 이 네임스페이스에는 작업자 노드에 설정되는 `iptables` 규칙 및 클러스터에 들어가거나 클러스터를 나가도록 허용되는 프록시 요청에 대해 수신 네트워크 트래픽을 유효성 검증하도록 컴포넌트가 포함됩니다. |
| `default` | 이 네임스페이스는 네임스페이스를 지정하거나 Kubernetes 리소스에 대한 프로젝트를 작성하지 않는 경우 사용됩니다. 또한 기본 네임스페이스에는 OpenShift 클러스터를 지원하기 위한 다음 컴포넌트가 포함됩니다. <ul><li>**`router`**: OpenShift는 외부 클라이언트가 서비스에 도달할 수 있도록 [라우트 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.openshift.com/container-platform/3.11/architecture/networking/routes.html)를 사용하여 호스트 이름에 앱의 서비스를 노출합니다. 라우터는 서비스를 호스트 이름에 맵핑합니다. </li><li>**`docker-registry`** 및 **`registry-console`**: OpenShift는 콘솔을 통해 로컬로 이미지를 관리하고 보기 위해 사용할 수 있는 내부 [컨테이너 이미지 레지스트리 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.openshift.com/container-platform/3.11/install_config/registry/index.html)를 제공합니다. 또는 사설 {{site.data.keyword.registrylong_notm}}를 설정할 수 있습니다. </li></ul>|
| 기타 프로젝트 | 기타 컴포넌트는 기본적으로 로깅, 모니터링 및 OpenShift 콘솔과 같은 기능을 사용으로 설정하도록 다양한 네임스페이스에 설치됩니다. <ul><li><code>ibm-cert-store</code></li><li><code>kube-public</code></li><li><code>kube-service-catalog</code></li><li><code>openshift</code></li><li><code>openshift-ansible-service-broker</code></li><li><code>openshift-console</code></li><li><code>openshift-infra</code></li><li><code>openshift-monitoring</code></li><li><code>openshift-node</code></li><li><code>openshift-template-service-broker</code></li><li><code>openshift-web-console</code></li></ul>|
{: caption="표 2. OpenShift 작업자 노드 컴포넌트." caption-side="top"}
{: #rhos-components-2}
{: tab-title="Worker nodes"}
{: tab-group="rhos-components"}
{: class="simple-tab-table"}

<br />


## 학습 1: Red Hat OpenShift on IBM Cloud 클러스터 작성
{: #openshift_create_cluster}

[콘솔](#openshift_create_cluster_console) 또는 [CLI](#openshift_create_cluster_cli)를 사용하여 {{site.data.keyword.containerlong_notm}}에서 Red Hat OpenShift on IBM Cloud 클러스터를 작성할 수 있습니다. 클러스터 작성 시 설정되는 클러스터에 대해 알아보려면 [아키텍처 개요](#openshift_architecture)를 참조하십시오. OpenShift는 표준 클러스터에만 사용할 수 있습니다. [자주 묻는 질문(FAQ)](/docs/containers?topic=containers-faqs#charges)에서 표준 클러스터의 가격에 대해 알아볼 수 있습니다.
{:shortdesc}

**default** 리소스 그룹에서만 클러스터를 작성할 수 있습니다. 베타 기간 중에 작성하는 OpenShift 클러스터는 베타 종료 후 30일 동안 유지되며 Red Hat OpenShift on IBM Cloud는 GA(General Availability)됩니다..
{: important}

### 콘솔에서 클러스터 작성
{: #openshift_create_cluster_console}

{{site.data.keyword.containerlong_notm}} 콘솔에서 표준 OpenShift 클러스터를 작성합니다.
{: shortdesc}

시작하기 전에 클러스터를 작성할 수 있는 적합한 권한을 갖추도록 [전제조건을 완료](#openshift_prereqs)하십시오. 

1.  클러스터를 작성하십시오.
    1.  [{{site.data.keyword.Bluemix_notm}} 계정![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/)에 로그인하십시오.
    2.  햄버거 메뉴 ![햄버거 메뉴 아이콘](../icons/icon_hamburger.svg "햄버거 메뉴 아이콘")에서 **Kubernetes**를 선택한 후 **클러스터 작성**을 선택하십시오.
    3.  클러스터 설정 세부사항 및 이름을 선택하십시오. 베타의 경우 OpenShift 클러스터는 워싱턴, DC 및 런던 데이터 센터에 위치한 표준 클러스터로만 사용 가능합니다. 
        *   **플랜 선택**의 경우 **표준**을 선택하십시오.
        *   **리소스 그룹**의 경우 **default**를 사용해야 합니다.
        *   **위치**의 경우 지역을 **북미** 또는 **유럽**을 선택하고, **단일 구역** 또는 **다중 구역** 가용성을 선택한 후 **워싱턴, DC** 또는 **런던** 작업자 구역을 선택하십시오. 
        *   **기본 작업자 풀**의 경우 **OpenShift** 클러스터 버전을 선택하십시오. Red Hat OpenShift on IBM Cloud는 Kubernetes 버전 1.11을 포함한 OpenShift 버전 3.11만 지원합니다. 최소 네 개의 코어, 16GB RAM을 포함하여 이상적으로 작업자 노드에 사용할 수 있는 특성을 선택하십시오. 
        *   구역당 작성할 작업자 노드 수(예: 3)를 설정하십시오. 
    4.  완료하려면 **클러스터 작성**을 클릭하십시오. <p class="note">클러스터 작성을 완료하는 데 약간의 시간이 소요될 수 있습니다. 클러스터 상태가 **정상**을 나타내면 클러스터 네트워크 및 로드 밸런싱 컴포넌트는 OpenShift 웹 콘솔 및 기타 라우트에 사용하는 클러스터 도메인을 배치하고 업데이트하는 데 약 10분 이상이 소요됩니다. 다음 단계로 계속 진행하려면 **Ingress 하위 도메인**이 `<cluster_name>.<region>.containers.appdomain.cloud`의 패턴을 따르는지 확인하여 클러스터가 완료될 때까지 기다십시오. </p>
2.  클러스터 세부사항 페이지에서 **OpenShift 웹 콘솔**을 클릭하십시오.
3.  OpenShift 컨테이너 플랫폼 메뉴 표시줄의 드롭 다운 메뉴에서 **애플리케이션 콘솔**을 클릭하십시오. 애플리케이션 콘솔에는 클러스터의 모든 프로젝트 네임스페이스가 나열됩니다. 네임스페이스로 이동하여 애플리케이션, 빌드 및 기타 Kubernetes 리소스를 볼 수 있습니다. 
4.  터미널에서 작업하여 다음 학습을 완료하려면 프로파일 **IAM#user.name@email.com > 로그인 명령 복사**를 클릭하십시오. CLI를 통해 인증하려면 복사된 `oc` 로그인 명령을 터미널에 붙여넣으십시오. 

### CLI에서 클러스터 작성
{: #openshift_create_cluster_cli}

{{site.data.keyword.Bluemix_notm}} CLI를 사용하여 표준 OpenShift 클러스터를 작성하십시오.
{: shortdesc}

시작하기 전에 클러스터, `ibmcloud` CLI 및 플러그인, `oc` 및 `kubectl` CLI를 작성할 수 있는 적합한 권한을 갖추도록 [전제조건을 완료](#openshift_prereqs)하십시오. 

1.  OpenShift 클러스터를 작성하기 위해 설정한 계정에 로그인하십시오. **us-east** 또는 **eu-gb** 지역과 **default** 리소스 그룹을 대상으로 지정하십시오. 연합 계정이 있는 경우, `--sso` 플래그를 포함하십시오.
    ```
    ibmcloud login -r (us-east|eu-gb) -g default [--sso]
    ```
    {: pre}
2.  클러스터를 작성하십시오.
    ```
    ibmcloud ks cluster-create --name <name> --location <zone> --kube-version <openshift_version> --machine-type <worker_node_flavor> --workers <number_of_worker_nodes_per_zone> --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID>
    ```
    {: pre}

    워싱턴, DC에서 네 개의 코어와 16GB 메모리가 있는 세 개의 작업자 노드를 포함하여 클러스터를 작성하는 명령 예제입니다. 

    ```
    ibmcloud ks cluster-create --name my_openshift --location wdc04 --kube-version 3.11_openshift --machine-type b3c.4x16.encrypted  --workers 3 --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID>
    ```
    {: pre}

    <table summary="표에서 첫 번째 행은 두 열 모두에 걸쳐 있습니다. 나머지 행은 왼쪽에서 오른쪽 방향으로 읽어야 하며, 명령 컴포넌트는 1열에 있고 일치하는 설명은 2열에 있습니다.">
    <caption>cluster-create 컴포넌트</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 이 명령의 컴포넌트 이해</th>
    </thead>
    <tbody>
    <tr>
    <td><code>cluster-create</code></td>
    <td>{{site.data.keyword.Bluemix_notm}} 계정에 클래식 인프라 클러스터를 작성하는 명령입니다.</td>
    </tr>
    <tr>
    <td><code>--name <em>&lt;name&gt;</em></code></td>
    <td>클러스터 이름을 입력하십시오. 이름은 문자로 시작해야 하며 35자 이하의 문자, 숫자 및 하이픈(-)을 포함할 수 있습니다. {{site.data.keyword.Bluemix_notm}} 지역에서 고유한 이름을 사용하십시오. </td>
    </tr>
    <tr>
    <td><code>--location <em>&lt;zone&gt;</em></code></td>
    <td>클러스터를 작성할 구역을 지정하십시오. 베타의 경우 사용 가능한 구역은 `wdc04, wdc06, wdc07, lon04, lon05,` 또는 `lon06`입니다.</p></td>
    </tr>
    <tr>
      <td><code>--kube-version <em>&lt;openshift_version&gt;</em></code></td>
      <td>지원되는 OpenShift 버전을 선택해야 합니다. OpenShift 버전에는 기본 Kubernetes Ubuntu 클러스터에서 사용 가능한 Kubernetes 버전과는 다른 Kubernetes 버전이 포함되어 있습니다. 사용 가능한 OpenShift 버전을 나열하려면 `ibmcloud ks versions`를 실행하십시오. 최신 패치 버전을 사용하여 클러스터를 작성하려면 ` 3.11_openshift`와 같은 주 버전 및 부 버전만 지정하면 됩니다. <br><br>Red Hat OpenShift on IBM Cloud는 Kubernetes 버전 1.11을 포함한 OpenShift 버전 3.11만 지원합니다.
</td>
    </tr>
    <tr>
    <td><code>--machine-type <em>&lt;worker_node_flavor&gt;</em></code></td>
    <td>머신 유형을 선택합니다. 공유 또는 전용 하드웨어에서 가상 머신으로서 또는 베어메탈에서 실제 머신으로서 작업자 노드를 배치할 수 있습니다. 사용 가능한 실제 및 가상 머신 유형은 클러스터가 배치되는 구역에 따라 다양합니다. 사용 가능한 머신 유형을 나열하려면 `ibmcloud ks machine-types --zone <zone>`을 실행하십시오.</td>
    </tr>
    <tr>
    <td><code>--workers <em>&lt;number_of_worker_nodes_per_zone&gt;</em></code></td>
    <td>클러스터에 포함시킬 작업자 노드의 수입니다. 클러스터에 기본 컴포넌트를 실행하고 고가용성을 위해 충분한 리소스를 보유하도록 최소 세 개의 작업자 노드를 지정하려고 할 수 있습니다. <code>--workers</code> 옵션이 지정되지 않은 경우 하나의 작업자 노드가 작성됩니다.</td>
    </tr>
    <tr>
    <td><code>--public-vlan <em>&lt;public_vlan_id&gt;</em></code></td>
    <td>해당 구역에 대한 IBM Cloud 인프라(SoftLayer) 계정에서 공용 VLAN이 이미 설정되어 있으면 공용 VLAN의 ID를 입력하십시오. 사용 가능한 VLAN을 확인하려면 `ibmcloud ks vlans --zone <zone>`을 실행하십시오.<br><br>계정에 공용 VLAN이 없는 경우 이 옵션을 지정하지 마십시오. {{site.data.keyword.containerlong_notm}}에서 사용자를 위해 자동으로 공용 VLAN을 작성합니다.</td>
    </tr>
    <tr>
    <td><code>--private-vlan <em>&lt;private_vlan_id&gt;</em></code></td>
    <td>해당 구역에 대한 IBM Cloud 인프라(SoftLayer) 계정에서 사설 VLAN이 이미 설정되어 있으면 공용 VLAN의 ID를 입력하십시오. 사용 가능한 VLAN을 확인하려면 `ibmcloud ks vlans --zone <zone>`을 실행하십시오.<br><br>계정에 사설 VLAN이 없는 경우 이 옵션을 지정하지 마십시오. {{site.data.keyword.containerlong_notm}}에서 사용자를 위해 자동으로 사설 VLAN을 작성합니다.</td>
    </tr>
    </tbody></table>
3.  클러스터 세부사항을 나열하십시오. 클러스터 **상태**를 검토하고, **Ingress 하위 도메인**을 확인하고, **마스터 URL**을 기록해 두십시오.<p class="note">클러스터 작성을 완료하는 데 약간의 시간이 소요될 수 있습니다. 클러스터 상태가 **정상**을 나타내면 클러스터 네트워크 및 로드 밸런싱 컴포넌트는 OpenShift 웹 콘솔 및 기타 라우트에 사용하는 클러스터 도메인을 배치하고 업데이트하는 데 약 10분 이상이 소요됩니다. 다음 단계로 계속 진행하려면 **Ingress 하위 도메인**이 `<cluster_name>.<region>.containers.appdomain.cloud`의 패턴을 따르는지 확인하여 클러스터가 완료될 때까지 기다십시오. </p>
    ```
    ibmcloud ks cluster-get --cluster <cluster_name_or_ID>
    ```
    {: pre}
4.  클러스터에 연결하려면 구성 파일을 다운로드하십시오.
    ```
    ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
    ```
    {: pre}

    구성 파일 다운로드가 완료되면 환경 변수로서 경로를 로컬 Kubernetes 구성 파일로 설정하는 데 복사하여 붙여넣을 수 있는 명령이 표시됩니다.

    OS X에 대한 예:

    ```
    export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-<datacenter>-<cluster_name>.yml
    ```
    {: screen}
5.  브라우저에서 **마스터 URL**의 주소로 이동하고 `/console`을 추가하십시오. 예를 들면, `https://c0.containers.cloud.ibm.com:23652/console`입니다.
6.  프로파일 **IAM#user.name@email.com > 로그인 명령 복사**를 클릭하십시오. CLI를 통해 인증하려면 복사된 `oc` 로그인 명령을 터미널에 붙여넣으십시오. <p class="tip">나중에 OpenShift 콘솔에 액세스하려면 클러스터 마스터 URL을 저장하십시오. 대신, 이후 세션에서는 `cluster-config` 단계를 건너뛰고 콘솔에서 로그인 명령을 복사할 수 있습니다. </p>
7.  버전을 확인하여 클러스터에서 `oc` 명령이 올바르게 실행되는지 확인하십시오.

    ```
    oc version
    ```
    {: pre}

    출력 예:

    ```
    oc v3.11.0+0cbc58b
    kubernetes v1.11.0+d4cacc0
    features: Basic-Auth

    Server https://c0.containers.cloud.ibm.com:23652
    openshift v3.11.98
    kubernetes v1.11.0+d4cacc0
    ```
    {: screen}

    관리자 권한이 필요한 오퍼레이션(예: 클러스터에 모든 작업자 노드 또는 팟(Pod) 나열)을 수행할 수 없으면 `ibmcloud ks cluster-config --cluster <cluster_name_or_ID> --admin` 명령을 실행하여 클러스터 관리자에 적합한 TLS 인증서 및 권한 파일을 다운로드하십시오.
    {: tip}

<br />


## 학습 2: 기본 제공 OpenShift 서비스에 액세스
{: #openshift_access_oc_services}

Red Hat OpenShift on IBM Cloud는 OpenShift 콘솔, Prometheus 및 Grafana와 같은 클러스터 운영을 지원하는 데 사용할 수 있는 기본 제공 서비스와 함께 제공됩니다. 베타의 경우 이 서비스에 액세스하기 위해 [라우트 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.openshift.com/container-platform/3.11/architecture/networking/routes.html)의 로컬 호스트를 사용할 수 있습니다. 기본 라우트 도메인 이름은 `<service_name>-<namespace>.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud`의 클러스터 특정 패턴을 따릅니다.
{:shortdesc}

[콘솔](#openshift_services_console) 또는 [CLI](#openshift_services_cli)에서 기본 제공 OpenShift 서비스 라우트에 액세스할 수 있습니다. 프로젝트의 Kubernetes 리소스를 탐색하기 위해 콘솔을 사용하려고 할 수 있습니다. CLI를 사용하여 프로젝트 간의 라우트와 같은 리소스를 나열할 수 있습니다. 

### 콘솔에서 기본 제공 OpenShift 서비스에 액세스
{: #openshift_services_console}
1.  OpenShift 웹 콘솔에서는 OpenShift 컨테이너 플랫폼 메뉴 표시줄의 드롭 다운 메뉴에서 **애플리케이션 콘솔**을 클릭하십시오. 
2.  **기본값** 프로젝트를 선택한 후 탐색 분할창에서 **애플리케이션 > 팟(Pod)**을 클릭하십시오. 
3.  **라우터** 팟(Pod)이 **실행 중** 상태인지 확인하십시오. 라우터는 외부 네트워크 트래픽의 Ingress 지점으로 작동합니다. 라우터를 사용하여 라우터의 외부 IP 주소에 클러스터의 서비스를 공개적으로 노출하기 위해 라우터를 사용할 수 있습니다. 라우터는 사설 IP에서만 청취하는 앱 팟(Pod)과 다르게 공용 호스트 네트워크 인터페이스에서 청취합니다. 라우터는 라우트 호스트 이름과 연관시킨 서비스로 식별되는 앱 팟(Pod)의 IP로 라우트 호스트 이름에 대한 외부 요청을 프록시합니다. 
4.  **기본값** 프로젝트 탐색 분할창에서 **애플리케이션 > 배치**를 클릭한 후 **registry-console** 배치를 클릭하십시오. 내부 레지스트리 콘솔을 열려면 외부에서 액세스할 수 있도록 제공자 URL을 업데이트해야 합니다.
    1.  **registry-console** 세부사항 페이지의 **환경** 탭에서 **OPENSHIFT_OAUTH_PROVIDER_URL** 필드를 찾으십시오.  
    2. 값 필드에서 `c1` 뒤에 `-e`를 추가하십시오(예: `https://c1-e.eu-gb.containers.cloud.ibm.com:20399`). 
    3. **저장**을 클릭하십시오. 이제 레지스트리 콘솔 배치는 클러스터 마스터의 공용 API 엔드포인트를 통해 액세스할 수 있습니다. 
    4.  **기본값** 프로젝트 탐색 분할창에서 **애플리케이션 > 라우트**를 클릭하십시오. 레지스트리 콘솔을 열려면 **호스트 이름** 값을 클릭하십시오(예: `https://registry-console-default.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud`).<p class="note">베타의 경우 레지스트리 콘솔은 자체 서명된 TLS 인증서를 사용합니다. 이에 따라 레지스트리 콘솔에 도달하기 위해 진행하도록 선택해야 합니다. Google Chrome에서 **고급 > <cluster_master_URL로 진행>**을 클릭하십시오. 다른 브라우저에도 유사한 옵션이 있습니다. 이 설정을 진행할 수 없으면 개인용 브라우저에서 URL을 열어보십시오. </p>
5.  OpenShift 컨테이너 플랫폼 메뉴 표시줄의 드롭 다운 메뉴에서 **클러스터 콘솔**을 클릭하십시오.
6.  탐색 분할창에서 **모니터링**을 펼치십시오.
7.  **대시보드**와 같은 액세스할 기본 제공 모니터링 도구를 클릭하십시오. Grafana 라우트가 열립니다(예: `https://grafana-openshift-monitoring.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud`).<p class="note">처음 호스트 이름에 액세스하는 경우 인증해야 할 수 있습니다. 예를 들어, **OpenShift에서 로그인**을 클릭하고 IAM ID에 액세스할 수 있도록 권한을 부여하여 이를 수행합니다. </p>

### CLI에서 기본 제공 OpenShift 서비스에 액세스
{: #openshift_services_cli}

1.  OpenShift 웹 콘솔에서 프로파일 **IAM#user.name@email.com > 로그인 명령 복사**를 클릭하고 로그인 명령을 터미널에 붙여넣어서 인증하십시오.
    ```
    oc login https://c1-e.<region>.containers.cloud.ibm.com:<port> --token=<access_token>
    ```
    {: pre}
2.  라우터가 배치되었는지 확인하십시오. 라우터는 외부 네트워크 트래픽의 Ingress 지점으로 작동합니다. 라우터를 사용하여 라우터의 외부 IP 주소에 클러스터의 서비스를 공개적으로 노출하기 위해 라우터를 사용할 수 있습니다. 라우터는 사설 IP에서만 청취하는 앱 팟(Pod)과 다르게 공용 호스트 네트워크 인터페이스에서 청취합니다. 라우터는 라우트 호스트 이름과 연관시킨 서비스로 식별되는 앱 팟(Pod)의 IP로 라우트 호스트 이름에 대한 외부 요청을 프록시합니다.
    ```
    oc get svc router -n default
    ```
    {: pre}

    출력 예:
    ```
    NAME      TYPE           CLUSTER-IP               EXTERNAL-IP     PORT(S)                      AGE
    router    LoadBalancer   172.21.xxx.xxx   169.xx.xxx.xxx   80:30399/TCP,443:32651/TCP                      5h
    ```
    {: screen}
2.  액세스할 서비스 라우트의 **호스트/포트** 호스트 이름을 가져오십시오. 예를 들어, 클러스터의 리소스 사용량에 대한 메트릭을 확인하기 위해 Grafana 대시보드에 액세스하려고 할 수 있습니다. 기본 라우트 도메인 이름은 `<service_name>-<namespace>.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud`의 클러스터 특정 패턴을 따릅니다.
    ```
    oc get route --all-namespaces
    ```
    {: pre}

    출력 예:
    ```
    NAMESPACE                          NAME                HOST/PORT                                                                    PATH                  SERVICES            PORT               TERMINATION          WILDCARD
    default                            registry-console    registry-console-default.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud                              registry-console    registry-console   passthrough          None
    kube-service-catalog               apiserver           apiserver-kube-service-catalog.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud                        apiserver           secure             passthrough          None
    openshift-ansible-service-broker   asb-1338            asb-1338-openshift-ansible-service-broker.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud            asb                 1338               reencrypt            None
    openshift-console                  console             console.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud                                              console             https              reencrypt/Redirect   None
    openshift-monitoring               alertmanager-main   alertmanager-main-openshift-monitoring.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud                alertmanager-main   web                reencrypt            None
    openshift-monitoring               grafana             grafana-openshift-monitoring.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud                          grafana             https              reencrypt            None
    openshift-monitoring               prometheus-k8s      prometheus-k8s-openshift-monitoring.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud                   prometheus-k8s      web                reencrypt
    ```
    {: screen}
3.  **레지스트리 일회성 업데이트**: 인터넷에서 내부 레지스트리 콘솔에 액세스할 수 있도록 하려면 OpenShift 제공자 URL로 클러스터 마스터의 공용 API 엔드포인트를 사용하도록 `registry-console` 배치를 편집하십시오. 공용 API 엔드포인트에서는 형식이 사설 API 엔드포인트와 동일하지만 URL에서 추가 `-e`를 포함합니다.
    ```
    oc edit deploy registry-console -n default
    ```
    {: pre}
    
    `Pod Template.Containers.registry-console.Environment.OPENSHIFT_OAUTH_PROVIDER_URL` 필드에서 `c1` 뒤에 `-e`를 추가하십시오(예: `https://ce.eu-gb.containers.cloud.ibm.com:20399`).
    ```
    Name:                   registry-console
    Namespace:              default
    ...
    Pod Template:
      Labels:  name=registry-console
      Containers:
       registry-console:
        Image:      registry.eu-gb.bluemix.net/armada-master/iksorigin-registrconsole:v3.11.98-6
        ...
        Environment:
          OPENSHIFT_OAUTH_PROVIDER_URL:  https://c1-e.eu-gb.containers.cloud.ibm.com:20399
          ...
    ```
    {: screen}
4.  웹 브라우저에서 액세스할 라우트를 여십시오(예: `https://grafana-openshift-monitoring.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud`). 처음 호스트 이름에 액세스하는 경우 인증해야 할 수 있습니다. 예를 들어, **OpenShift에서 로그인**을 클릭하고 IAM ID에 액세스할 수 있도록 권한을 부여하여 이를 수행합니다. 

<br>
이제 기본 제공 OpenShift 앱을 사용할 수 있습니다! 예를 들어, Grafana를 사용하는 경우 네임스페이스 CPU 사용량 또는 기타 그래프를 확인할 수 있습니다. 기타 기본 제공 도구에 액세스하려면 해당 라우트 호스트 이름을 여십시오. 

<br />


## 학습 3: OpenShift 클러스터에 앱 배치
{: #openshift_deploy_app}

Red Hat OpenShift on IBM Cloud를 사용하여 새 앱을 작성하고 외부 사용자가 사용할 수 있도록 OpenShift 라우터를 통해 앱 서비스를 노출할 수 있습니다.
{: shortdesc}

마지막 학습을 중단하고 새 터미널을 시작한 경우 클러스터에 다시 로그인해야 합니다. `https://<master_URL>/console`에서 OpenShift 콘솔을 여십시오. 예를 들면, `https://c0.containers.cloud.ibm.com:23652/console`입니다. 그런 다음 프로파일 **IAM#user.name@email.com > 로그인 명령 복사**를 클릭하고 복사된 `oc` 로그인 명령을 터미널에 붙여넣어서 CLI를 통해 인증하십시오.
{: tip}

1.  Hello World 앱에 적합한 프로젝트를 작성하십시오. 프로젝트는 추가 어노테이션이 포함된 Kubernetes 네임스페이스의 OpenShift 버전입니다.
    ```
    oc new-project hello-world
    ```
    {: pre}
2.  [소스 코드에서 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/IBM/container-service-getting-started-wt) 샘플 앱을 빌드하십시오. OpenShift `new-app` 명령을 사용하여 이미지를 빌드할 Dockerfile 및 앱 코드가 포함된 원격 저장소의 디렉토리를 참조할 수 있습니다. 명령으로 이미지가 빌드되고, 로컬 Docker 레지스트리에 이미지가 저장되고, 앱 배치 구성(`dc`) 및 서비스(`svc`)가 작성됩니다. 새 앱 작성에 대한 자세한 정보는 [OpenShift 문서 참조 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.openshift.com/container-platform/3.11/dev_guide/application_lifecycle/new_app.html)를 참조하십시오.
    ```
    oc new-app --name hello-world https://github.com/IBM/container-service-getting-started-wt --context-dir="Lab 1"
    ```
    {: pre}
3.  샘플 Hello World 앱 컴포넌트가 작성되었는지 확인하십시오.
    1.  브라우저의 레지스트리 콘솔에 액세스하여 클러스터의 기본 제공 Docker 레지스트리에 있는 **hello-world** 이미지를 확인하십시오. 이전 학습에서 설명된 대로 `-e`를 포함하여 레지스트리 콘솔 제공자 URL을 업데이트했는지 확인하십시오.
        ```
        https://registry-console-default.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud/
        ```
        {: codeblock}
    2.  **hello-world** 서비스를 나열하고 서비스 이름을 기록해 두십시오. 라우터가 외부 트래픽 요청을 앱에 전달할 수 있도록 내부 클러스터 IP 주소의 트래픽을 청취합니다(서비스에 대한 라우트를 작성하지 않은 경우).
        ```
        oc get svc -n hello-world
        ```
        {: pre}

        출력 예:
        ```
        NAME          TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)    AGE
        hello-world   ClusterIP   172.21.xxx.xxx   <nones>       8080/TCP   31m
        ```
        {: screen}
    3.  팟(Pod)을 나열하십시오. 이름에 `build`가 포함된 팟(Pod)은 새 앱 빌드 프로세스의 일부로서 **완료됨** 상태인 작업입니다. **hello-world** 팟(Pod) 상태가 **실행 중**인지 확인하십시오.
        ```
        oc get pods -n hello-world
        ```
        {: pre}

        출력 예:
        ```
        NAME                READY     STATUS             RESTARTS   AGE
        hello-world-9cv7d   1/1       Running            0          30m
        hello-world-build   0/1       Completed          0          31m
        ```
        {: screen}
4.  공용으로 {{site.data.keyword.toneanalyzershort}} 서비스에 액세스할 수 있도록 라우트를 설정하십시오. 기본적으로 호스트 이름의 형식은 `<service_name>-<namespace>.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud`입니다. 호스트 이름을 사용자 정의할 경우 `--hostname=<hostname>` 플래그를 포함하십시오. 
    1.  **hello-world** 서비스에 대한 라우트를 작성하십시오.
        ```
        oc create route edge --service=hello-world -n hello-world
        ```
        {: pre}
    2.  **Host/Port** 출력에서 라우트 호스트 이름 주소를 가져오십시오.
        ```
        oc get route -n hello-world
        ```
        {: pre}
        출력 예:
        ```
        NAME          HOST/PORT                         PATH                                        SERVICES      PORT       TERMINATION   WILDCARD
        hello-world   hello-world-hello.world.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud    hello-world   8080-tcp   edge/Allow    None
        ```
        {: screen}
5.  앱에 액세스하십시오.
    ```
    curl https://hello-world-hello-world.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud
    ```
    {: pre}
    
    출력 예:
    ```
    Hello world from hello-world-9cv7d! Your app is up and running in a cluster!
    ```
    {: screen}
6.  **선택사항** 이 학습에서 작성한 리소스를 정리하기 위해 각 앱에 지정된 레이블을 사용할 수 있습니다. 
    1.  `hello-world` 프로젝트의 각 앱에 대한 모든 리소스를 나열하십시오.
        ```
        oc get all -l app=hello-world -o name -n hello-world
        ```
        {: pre}
        출력 예:
        ```
        pod/hello-world-1-dh2ff
        replicationcontroller/hello-world-1
        service/hello-world
        deploymentconfig.apps.openshift.io/hello-world
        buildconfig.build.openshift.io/hello-world
        build.build.openshift.io/hello-world-1
        imagestream.image.openshift.io/hello-world
        imagestream.image.openshift.io/node
        route.route.openshift.io/hello-world
        ```
        {: screen}
    2.  작성한 모든 리소스를 삭제하십시오.
        ```
        oc delete all -l app=hello-world -n hello-world
        ```
        {: pre}



<br />


## 학습 4: 클러스터를 모니터하기 위한 LogDNA 및 Sysdig 추가 기능 설정
{: #openshift_logdna_sysdig}

기본적으로 OpenShift는 기본 Kubernetes가 아닌 좀 더 강력한 [보안 컨텍스트 제한사항(SCC) ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.openshift.com/container-platform/3.11/admin_guide/manage_scc.html)을 설정하므로, 기본 Kubernetes에서 사용하는 일부 앱 또는 클러스터 추가 기능은 동일한 방식으로 OpenShift에 배치할 수 없음을 알 수 있습니다. 특히 `root` 사용자 또는 권한 있는 컨테이너로 실행하려면 많은 이미지가 필요하며, 기본적으로 OpenShift에서는 허용되지 않습니다. 이 학습에서는 두 개의 인기 있는 {{site.data.keyword.containerlong_notm}} 추가 기능인 {{site.data.keyword.la_full_notm}} 및 {{site.data.keyword.mon_full_notm}}를 사용하기 위해 팟(Pod) 스펙에서 권한 있는 보안 계정을 작성하고 `securityContext`를 업데이트하여 기본 SCC를 수정하는 방법을 알아볼 수 있습니다.
{: shortdesc}

시작하기 전에 관리자로 클러스터에 로그인하십시오. 
1.  `https://<master_URL>/console`에서 OpenShift 콘솔을 여십시오. 예를 들면, `https://c0.containers.cloud.ibm.com:23652/console`입니다.
2.  프로파일 **IAM#user.name@email.com > 로그인 명령 복사**를 클릭하고 복사된 `oc` 로그인 명령을 터미널에 붙여넣어서 CLI를 통해 인증하십시오. 
3.  클러스터에 대한 관리자 구성 파일을 다운로드하십시오.
    ```
    ibmcloud ks cluster-config --cluster <cluster_name_or_ID> --admin
    ```
    {: pre}
    
    구성 파일 다운로드가 완료되면 환경 변수로서 경로를 로컬 Kubernetes 구성 파일로 설정하는 데 복사하여 붙여넣을 수 있는 명령이 표시됩니다.

    OS X에 대한 예:

    ```
    export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-<datacenter>-<cluster_name>.yml
    ```
    {: screen}
4.  [{{site.data.keyword.la_short}}](#openshift_logdna) 및 [{{site.data.keyword.mon_short}}](#openshift_sysdig)를 설정하려면 학습을 진행하십시오. 

### 학습 4a: LogDNA 설정
{: #openshift_logdna}

{{site.data.keyword.la_full_notm}}를 위한 프로젝트 및 권한 있는 서비스 계정을 설정하십시오. 그런 다음 {{site.data.keyword.Bluemix_notm}} 계정에서 {{site.data.keyword.la_short}} 인스턴스를 작성하십시오. {{site.data.keyword.la_short}} 인스턴스를 OpenShift 클러스터와 통합하려면 루트로 실행하기 위해 권한 있는 서비스 계정을 사용하도록 배치된 디먼 세트를 수정해야 합니다.
{: shortdesc}

1.  LogDNA를 위한 프로젝트 및 권한 있는 서비스 계정을 설정하십시오. 
    1.  클러스터 관리자로서 `logdna` 프로젝트를 작성하십시오.
        ```
        oc adm new-project logdna
        ```
        {: pre}
    2.  작성하는 후속 리소스가 `logdna` 프로젝트 네임스페이스에 있도록 프로젝트를 대상으로 지정하십시오.
        ```
        oc project logdna
        ```
        {: pre}
    3.  `logdna` 프로젝트를 위한 서비스 계정을 작성하십시오.
        ```
        oc create serviceaccount logdna
        ```
        {: pre}
    4.  권한 있는 보안 컨텍스트 제한조건을 `logdna` 프로젝트를 위한 서비스 계정에 추가하십시오. <p class="note">`privileged` SCC 정책이 서비스 계정에 제공하는 권한을 확인하려면 `oc describe scc privileged`를 실행하십시오. SCC에 대한 자세한 정보는 [OpenShift 문서 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.openshift.com/container-platform/3.11/admin_guide/manage_scc.html)를 참조하십시오.</p>
        ```
        oc adm policy add-scc-to-user privileged -n logdna -z logdna
        ```
        {: pre}
2.  클러스터와 동일한 리소스 그룹에 {{site.data.keyword.la_full_notm}} 인스턴스를 작성하십시오. 로그의 보존 기간을 결정하는 가격 책정 플랜(예: 0일동안 로그를 보존하는 `lite`)을 선택하십시오. 지역은 클러스터의 지역과 일치하지 않아도 됩니다. 자세한 정보는 [인스턴스 프로비저닝](/docs/services/Log-Analysis-with-LogDNA/tutorials?topic=LogDNA-provision) 및 [가격 책정 플랜](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-about#overview_pricing_plans)을 참조하십시오.
    ```
    ibmcloud resource service-instance-create <service_instance_name> logdna (lite|7-days|14-days|30-days) <region> [-g <resource_group>]
    ```
    {: pre}
    
    명령 예:
    ```
    ibmcloud resource service-instance-create logdna-openshift logdna lite us-south
    ```
    {: pre}
    
    출력에서 `crn:v1:bluemix:public:logdna:<region>:<ID_string>::` 형식으로 된 서비스 인스턴스 **ID**를 기록해 두십시오.
    ```
    서비스 인스턴스 <name>이 작성되었습니다.
                 
    Name:         <name>   
    ID:           crn:v1:bluemix:public:logdna:<region>:<ID_string>::   
    GUID:         <guid>   
    Location:     <region>   
    ...
    ```
    {: screen}    
3.  {{site.data.keyword.la_short}} 인스턴스 유입 키를 가져오십시오. LogDNA 유입 키는 LogDNA 유입 서버에 대한 보안 웹 소켓을 열고 {{site.data.keyword.la_short}} 서비스로 로깅 에이전트를 인증하는 데 사용됩니다. 
    1.  LogDNA 인스턴스에 대한 서비스 키를 작성하십시오.
        ```
        ibmcloud resource service-key-create <key_name> Administrator --instance-id <logdna_instance_ID>
        ```
        {: pre}
    2.  서비스 키의 **ingestion_key**를 기록해 두십시오.
        ```
        ibmcloud resource service-key <key_name>
        ```
        {: pre}
        
        출력 예:
        ```
        Name:          <key_name>  
        ID:            crn:v1:bluemix:public:logdna:<region>:<ID_string>::    
        Created At:    Thu Jun  6 21:31:25 UTC 2019   
        State:         active   
        Credentials:                                   
                       apikey:                   <api_key_value>      
                       iam_apikey_description:   Auto-generated for key <ID>     
                       iam_apikey_name:          <key_name>       
                       iam_role_crn:             crn:v1:bluemix:public:iam::::role:Administrator      
                       iam_serviceid_crn:        crn:v1:bluemix:public:iam-identity::<ID_string>     
                       ingestion_key:            111a11aa1a1a11a1a11a1111aa1a1a11    
        ```
        {: screen}
4.  Kubernetes 시크릿을 작성하여 서비스 인스턴스에 대한 LogDNA 유입 키를 저장하십시오.
    ```
    oc create secret generic logdna-agent-key --from-literal=logdna-agent-key=<logDNA_ingestion_key>
    ```
    {: pre}
5.  Kubernetes 디먼 세트를 작성하여 Kubernetes 클러스터의 모든 작업자 노드에 LogDNA 에이전트를 배치하십시오. LogDNA 에이전트는 팟(Pod)의 `/var/log` 디렉토리에 저장되는 `*.log` 확장자와 확장자 없는 파일을 사용하여 로그를 수집합니다. 기본적으로 로그는 `kube-system`을 포함한 모든 네임스페이스에서 수집되며 자동으로 {{site.data.keyword.la_short}} 서비스에 전달됩니다.
    ```
    oc create -f https://assets.us-south.logging.cloud.ibm.com/clients/logdna-agent-ds.yaml
    ```
    {: pre}
6.  LogDNA 에이전트 디먼 세트 구성을 편집하여 이전에 작성한 서비스 계정을 참조하고 보안 컨텍스트를 권한이 부여된 보안 컨텍스트로 설정하십시오.
    ```
    oc edit ds logdna-agent
    ```
    {: pre}
    
    구성 파일에서 다음 스펙을 추가하십시오. 
    *   `spec.template.spec`에서 `serviceAccount: logdna`를 추가하십시오.
    *   `spec.template.spec.containers`에서 `securityContext: privileged: true`를 추가하십시오.
    *   `us-south` 이외의 지역에서 {{site.data.keyword.la_short}} 인스턴스를 작성한 경우 `<region>`으로 `LDAPIHOST` 및 `LDLOGHOST`에 대한 `spec.template.spec.containers.env` 환경 변수 값을 업데이트하십시오. 

    출력 예:
    ```
    apiVersion: extensions/v1beta1
    kind: DaemonSet
    spec:
      ...
      template:
        ...
        spec:
          serviceAccount: logdna
          containers:
          - securityContext:
              privileged: true
            ...
            env:
            - name: LOGDNA_AGENT_KEY
              valueFrom:
                secretKeyRef:
                  key: logdna-agent-key
                  name: logdna-agent-key
            - name: LDAPIHOST
              value: api.<region>.logging.cloud.ibm.com
            - name: LDLOGHOST
              value: logs.<region>.logging.cloud.ibm.com
          ...
    ```
    {: screen}
7.  각 노드의 `logdna-agent` 팟(Pod)의 상태가 **실행 중**인지 확인하십시오.
    ```
    oc get pods
    ```
    {: pre}
8.  [{{site.data.keyword.Bluemix_notm}} 관찰 가능성 > 로깅 콘솔](https://cloud.ibm.com/observe/logging)의 {{site.data.keyword.la_short}} 인스턴스에 대한 행에서 **LogDNA 보기**를 클릭하십시오. LogDNA 대시보드가 열립니다. 이제 로그 분석을 시작할 수 있습니다. 

{{site.data.keyword.la_short}} 사용 방법에 대한 자세한 정보는 [다음 단계 문서](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-kube#kube_next_steps)를 참조하십시오.

### 학습 4b: Sysdig 설정
{: #openshift_sysdig}

{{site.data.keyword.Bluemix_notm}} 계정에서 {{site.data.keyword.mon_full_notm}} 인스턴스를 작성하십시오. {{site.data.keyword.mon_short}} 인스턴스를 OpenShift 클러스터와 통합하려면 Sysdig 에이전트를 위한 프로젝트 및 권한 있는 서비스 계정을 작성하는 스크립트를 실행해야 합니다.
{: shortdesc}

1.  클러스터와 동일한 리소스 그룹에 {{site.data.keyword.mon_full_notm}} 인스턴스를 작성하십시오. 로그의 보존 기간을 결정하는 가격 책정 플랜(예: `lite`)을 선택하십시오. 지역은 클러스터의 지역과 일치하지 않아도 됩니다. 자세한 정보는 [인스턴스 프로비저닝](/docs/services/Monitoring-with-Sysdig?topic=Sysdig-provision)을 참조하십시오.
    ```
    ibmcloud resource service-instance-create <service_instance_name> sysdig-monitor (lite|graduated-tier) <region> [-g <resource_group>]
    ```
    {: pre}
    
    명령 예:
    ```
    ibmcloud resource service-instance-create sysdig-openshift sysdig-monitor lite us-south
    ```
    {: pre}
    
    출력에서 `crn:v1:bluemix:public:logdna:<region>:<ID_string>::` 형식으로 된 서비스 인스턴스 **ID**를 기록해 두십시오.
    ```
    서비스 인스턴스 <name>이 작성되었습니다.
                 
    Name:         <name>   
    ID:           crn:v1:bluemix:public:sysdig-monitor:<region>:<ID_string>::   
    GUID:         <guid>   
    Location:     <region>   
    ...
    ```
    {: screen}    
2.  {{site.data.keyword.mon_short}} 인스턴스 액세스 키를 가져오십시오. Sysdig 액세스 키는 Sysdig 유입 서버에 대한 보안 웹 소켓을 열고 {{site.data.keyword.mon_short}} 서비스로 모니터링 에이전트를 인증하는 데 사용됩니다. 
    1.  Sysdig 인스턴스에 대한 서비스 키를 작성하십시오.
        ```
        ibmcloud resource service-key-create <key_name> Administrator --instance-id <sysdig_instance_ID>
        ```
        {: pre}
    2.  서비스 키의 **Sysdig 액세스 키** 및 **Sysdig 콜렉터 엔드포인트**를 기록해 두십시오.
        ```
        ibmcloud resource service-key <key_name>
        ```
        {: pre}
        
        출력 예:
        ```
        Name:          <key_name>  
        ID:            crn:v1:bluemix:public:sysdig-monitor:<region>:<ID_string>::    
        Created At:    Thu Jun  6 21:31:25 UTC 2019   
        State:         active   
        Credentials:                                   
                       Sysdig Access Key:           11a1aa11-aa1a-1a1a-a111-11a11aa1aa11      
                       Sysdig Collector Endpoint:   ingest.<region>.monitoring.cloud.ibm.com      
                       Sysdig Customer Id:          11111      
                       Sysdig Endpoint:             https://<region>.monitoring.cloud.ibm.com  
                       apikey:                   <api_key_value>      
                       iam_apikey_description:   Auto-generated for key <ID>     
                       iam_apikey_name:          <key_name>       
                       iam_role_crn:             crn:v1:bluemix:public:iam::::role:Administrator      
                       iam_serviceid_crn:        crn:v1:bluemix:public:iam-identity::<ID_string>       
        ```
        {: screen}
3.  권한 있는 서비스 계정 및 Kubernetes 디먼 세트로 `ibm-observe` 프로젝트를 설정하도록 스크립트를 실행하여 Kubernetes 클러스터의 모든 작업자 노드에 Sysdig 에이전트를 배치하십시오. Sysdig 에이전트는 작업자 노드 CPU 사용량, 작업자 노드 메모리 사용량, 컨테이너 간의 HTTP 트래픽 및 몇 가지 인프라 컴포넌트에 대한 데이터와 같은 메트릭을 수집합니다.  

    다음 명령에서 <sysdig_access_key> 및 <sysdig_collector_endpoint>를 이전에 작성한 서비스 키의 값으로 대체하십시오. <tag>의 경우 메트릭이 발생하는 환경을 식별할 수 있도록 태그를 Sysdig 에이전트(예: `role:service,location:us-south`)와 연관시킬 수 있습니다. 

    ```
    curl -sL https://ibm.biz/install-sysdig-k8s-agent | bash -s -- -a <sysdig_access_key> -c <sysdig_collector_endpoint> -t <tag> -ac 'sysdig_capture_enabled: false' --openshift
    ```
    {: pre}
    
    출력 예: 
    ```
    * Detecting operating system
    * Downloading Sysdig cluster role yaml
    * Downloading Sysdig config map yaml
    * Downloading Sysdig daemonset v2 yaml
    * Creating project: ibm-observe
    * Creating sysdig-agent serviceaccount in project: ibm-observe
    * Creating sysdig-agent access policies
    * Creating sysdig-agent secret using the ACCESS_KEY provided
    * Retreiving the IKS Cluster ID and Cluster Name
    * Setting cluster name as <cluster_name>
    * Setting ibm.containers-kubernetes.cluster.id 1fbd0c2ab7dd4c9bb1f2c2f7b36f5c47
    * Updating agent configmap and applying to cluster
    * Setting tags
    * Setting collector endpoint
    * Adding additional configuration to dragent.yaml
    * Enabling Prometheus
    configmap/sysdig-agent created
    * Deploying the sysdig agent
    daemonset.extensions/sysdig-agent created
    ```
    {: screen}
        
4.  각 노드의 `sydig-agent` 팟(Pod)에서 **1/1** 팟(Pod)이 준비 상태이며 각 팟(Pod)에 **실행 중** 상태가 있음을 나타내는지 확인하십시오.
    ```
    oc get pods
    ```
    {: pre}
    
    출력 예:
    ```
    NAME                 READY     STATUS    RESTARTS   AGE
    sysdig-agent-qrbcq   1/1       Running   0          1m
    sysdig-agent-rhrgz   1/1       Running   0          1m
    ```
    {: screen}
5.  [{{site.data.keyword.Bluemix_notm}} 관찰 가능성 > 모니터링 콘솔](https://cloud.ibm.com/observe/logging)의 {{site.data.keyword.mon_short}} 인스턴스에 대한 행에서 **Sysdig 보기**를 클릭하십시오. Sysdig 대시보드가 열립니다. 이제 클러스터 메트릭 분석을 시작할 수 있습니다. 

{{site.data.keyword.mon_short}} 사용 방법에 대한 자세한 정보는 [다음 단계 문서](/docs/services/Monitoring-with-Sysdig?topic=Sysdig-kubernetes_cluster#kubernetes_cluster_next_steps)를 참조하십시오.

### 선택사항: 정리
{: #openshift_logdna_sysdig_cleanup}

클러스터와 {{site.data.keyword.Bluemix_notm}} 계정에서 {{site.data.keyword.la_short}} 및 {{site.data.keyword.mon_short}} 인스턴스를 제거하십시오. [지속적 스토리지](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-archiving)에 로그 및 메트릭을 저장하지 않으면 계정에서 인스턴스를 삭제한 후 이 정보에 액세스할 수 없습니다.
{: shortdesc}

1.  사용자가 작성한 프로젝트를 제거하여 클러스터에서 {{site.data.keyword.la_short}} 및 {{site.data.keyword.mon_short}} 인스턴스를 정리하십시오. 프로젝트를 삭제하면 서비스 계정 및 디먼 세트와 같은 리소스도 제거됩니다.
    ```
    oc delete project logdna
    ```
    {: pre}
    ```
    oc delete project ibm-observe
    ```
    {: pre}
2.  {{site.data.keyword.Bluemix_notm}} 계정에서 인스턴스를 제거하십시오. 
    *   [{{site.data.keyword.la_short}} 인스턴스 제거](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-remove)
    *   [{{site.data.keyword.mon_short}} 인스턴스 제거](/docs/services/Monitoring-with-Sysdig?topic=Sysdig-remove)

<br />


## 제한사항
{: #openshift_limitations}

Red Hat OpenShift on IBM Cloud 베타는 다음 제한사항과 함께 릴리스됩니다.
{: shortdesc}

**클러스터**:
*   무료 클러스터가 아닌 표준 클러스터만 작성할 수 있습니다. 
*   사용 가능한 위치는 두 개의 다중 구역 메트로 지역 즉, 워싱턴, DC 및 런던입니다. 지원되는 구역은 `wdc04, wdc06, wdc07, lon04, lon05,` 및 `lon06`입니다.
*   다중 운영 체제(예: OpenShift on Red Hat Enterprise Linux 및 기본 Kubernetes on Ubuntu)에서 실행되는 작업자 노드가 포함된 클러스터를 작성할 수 없습니다.
*   Kubernetes 버전 1.12 이상이 필요하므로 [클러스터 오토스케일러](/docs/containers?topic=containers-ca)가 지원되지 않습니다. OpenShift 3.11에는 Kubernetes 버전 1.11만 포함됩니다.



**스토리지**:
*   {{site.data.keyword.Bluemix_notm}} 파일, 블록 및 클라우드 오브젝트 스토리지가 지원됩니다. Portworx 소프트웨어 정의 스토리지(SDS)는 지원되지 않습니다.
*   {{site.data.keyword.Bluemix_notm}} NFS 파일 스토리지가 Linux 사용자 권한을 구성하므로 파일 스토리지 사용 시 오류가 발생할 수 있습니다. 해당 경우 [OpenShift 보안 컨텍스트 제한사항 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.openshift.com/container-platform/3.11/admin_guide/manage_scc.html)을 구성하거나 다른 스토리지 유형을 사용해야 할 수 있습니다. 

**네트워킹**:
*   Calico는 OpenShift SDN 대신 네트워킹 정책 제공자로 사용됩니다.

**추가 기능, 통합 및 기타 서비스**:
*   {{site.data.keyword.containerlong_notm}} 추가 기능(예: Istio, Knative 및 Kubernetes 터미널)은 사용할 수 없습니다.
*   Helm 차트는 OpenShift 클러스터에서 작동하도록 인증되지 않았습니다(단, {{site.data.keyword.Bluemix_notm}} Object Storage 제외).
*   클러스터는 {{site.data.keyword.registryshort_notm}} `icr.io` 도메인에 대한 이미지 풀 시크릿으로 배치되지 않습니다. [자체 이미지 풀 시크릿을 작성](/docs/containers?topic=containers-images#other_registry_accounts)하거나, 대신 OpenShift 클러스터를 위한 기본 제공 Docker 레지스트리를 사용할 수 있습니다. 

**앱**:
*   기본적으로 OpenShift는 기본 Kubernetes가 아닌 더욱 강력한 보안 설정을 설정합니다. 자세한 정보는 [보안 컨텍스트 제한사항(SCC) 관리 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.openshift.com/container-platform/3.11/admin_guide/manage_scc.html)에 대한 OpenShift 문서를 참조하십시오 .
*   예를 들어, 상태가 `CrashLoopBackOff`인 팟(Pod)에서 루트로 실행되도록 구성된 앱이 실패할 수 있습니다. 이 문제를 해결하려면 기본 보안 컨텍스트 제한사항을 수정하거나 루트로 실행되지 않는 이미지를 사용할 수 있습니다. 
*   기본적으로 OpenShift는 로컬 Docker 레지스트리를 사용하여 설정됩니다. 원격 사설 {{site.data.keyword.registrylong_notm}} `icr.io` 도메인 이름에 저장된 이미지를 사용할 경우 자체적으로 각 글로벌 및 지역 레지스트리에 대한 시크릿을 작성해야 합니다. `default` 네임스페이스에서 이미지를 가져올 네임스페이스로 [`default-<region>-icr-io` 시크릿 복사](/docs/containers?topic=containers-images#copy_imagePullSecret) 또는 [자체 시크릿 작성](/docs/containers?topic=containers-images#other_registry_accounts)을 사용할 수 있습니다. 그런 다음 배치 구성 또는 네임스페이스 서비스 계정으로 [이미지 풀 시크릿을 추가](/docs/containers?topic=containers-images#use_imagePullSecret)하십시오.
*   OpenShift 콘솔은 Kubernetes 대시보드 대신 사용됩니다.

<br />


## 다음에 수행할 작업
{: #openshift_next}

앱 및 라우팅 서비스 관련 작업에 대한 자세한 정보는 [OpenShift 개발자 안내서](https://docs.openshift.com/container-platform/3.11/dev_guide/index.html)를 참조하십시오.

<br />


## 피드백 및 질문
{: #openshift_support}

베타 기간 중에는 Red Hat OpenShift on IBM Cloud 클러스터는 IBM Support 및 Red Hat Support가 적용되지 않습니다. 모든 지원은 GA(General Availability)에 대비하여 제품을 평가하는 데 도움을 주기 위해 제공됩니다.
{: important}

모든 질문 또는 피드백은 Slack에 게시하십시오. 
*   외부 사용자인 경우 [#openshift ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://ibm-container-service.slack.com/messages/CKCJLJCH4) 채널에 게시하십시오. 
*   IBM 직원인 경우 [#iks-openshift-users ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://ibm-argonauts.slack.com/messages/CJH0UPN2D) 채널을 사용하십시오.

{{site.data.keyword.Bluemix_notm}} 계정에 대해 IBM ID를 사용 중이 아닌 경우에는 이 Slack에 대한 [초대를 요청](https://bxcs-slack-invite.mybluemix.net/)하십시오.
{: tip}
