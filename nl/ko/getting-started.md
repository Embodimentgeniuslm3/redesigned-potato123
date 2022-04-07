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


# 시작하기 튜토리얼
{: #getting-started}

Kubernetes 클러스터에서 실행되는 Docker 컨테이너에 고가용성 앱을 배치하여 {{site.data.keyword.containerlong}}를 진행하십시오.
{:shortdesc}

컨테이너는 환경 간에 앱을 원활하게 이동시킬 수 있도록 앱 및 해당 종속성을 모두 패키징하는 표준 방식입니다.  가상 머신과 달리 컨테이너는 운영 체제를 함께 제공하지 않습니다. 앱 코드, 런타임, 시스템 도구, 라이브러리 및 설정만 컨테이너 내에 패키징됩니다. 컨테이너는 가상 머신보다 경량이고 휴대하기 쉬우며 효율적입니다.

## 클러스터 시작하기
{: #clusters_gs}

컨테이너에 앱을 배치하시겠습니까? 기다리십시오. 먼저 Kubernetes 클러스터 작성으로 시작하십시오. Kubernetes는 컨테이너를 위한 오케스트레이션 도구입니다. Kubernetes를 사용하면 개발자는 클러스터의 처리 능력과 유연성을 사용하여 고가용성 앱을 신속하게 배치할 수 있습니다.
{:shortdesc}

클러스터는 무엇입니까? 클러스터는 앱의 고가용성을 유지시키는 리소스, 작업자 노드, 네트워크 및 스토리지 디바이스의 세트입니다. 클러스터가 있으면 컨테이너에 앱을 배치할 수 있습니다.

시작하기 전에 올바른 [{{site.data.keyword.Bluemix_notm}} 계정](https://cloud.ibm.com/registration) 유형을 가져오십시오.
* **청구 가능(종량과금제 또는 구독)**: 무료 클러스터를 작성할 수 있습니다. 표준 클러스터에서 작성 및 사용하기 위한 IBM Cloud 인프라(SoftLayer) 리소스를 프로비저닝할 수도 있습니다.
* **Lite**: 무료 또는 표준 클러스터를 작성할 수 없습니다. 청구 가능한 계정으로 [계정을 업그레이드](/docs/account?topic=account-accountfaqs#changeacct)하십시오.

무료 클러스터를 작성하려면 다음을 수행하십시오.

1.  [{{site.data.keyword.Bluemix_notm}} **카탈로그** ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/catalog?category=containers)에서 **{{site.data.keyword.containershort_notm}}**를 선택하고 **작성**을 클릭하십시오. 클러스터 구성 페이지가 열립니다. 기본적으로 **무료 클러스터**가 선택되어 있습니다.

2.  클러스터에 고유 이름을 지정하십시오.

3.  **클러스터 작성**을 클릭하십시오. 1개의 작업자 노드가 포함된 작업자 풀이 작성됩니다. 작업자 노드는 프로비저닝에 수 분이 걸릴 수 있지만 **작업자 노드** 탭에서 진행상태를 볼 수 있습니다. `Ready` 상태가 되면 클러스터에 대한 작업을 시작할 수 있습니다.

<br>

잘하셨습니다! 첫 번째 Kubernetes 클러스터를 작성했습니다. 다음은 무료 클러스터에 대한 일부 세부사항입니다.

*   **머신 유형**: 무료 클러스터에는 앱이 사용할 수 있는 2개 CPU, 4GB 메모리 및 단일 100GB SAN 디스크가 있는 작업자 풀로 그룹화된 하나의 가상 작업자 노드가 있습니다. 표준 클러스터를 작성할 때 다양한 머신 크기로 실제(베어메탈) 머신 또는 가상 머신 간에 선택할 수 있습니다.
*   **관리 마스터**: 클러스터에서 모든 Kubernetes 리소스를 제어하고 모니터링하는 전용 및 고가용성의 {{site.data.keyword.IBM_notm}} 소유 Kubernetes 마스터가 작업자 노드를 중앙에서 모니터링하고 관리합니다. 사용자는 이 마스터를 관리하기 위해 염려할 필요가 없으며 작업자 노드에 배치된 작업자 노드 및 앱에만 집중할 수 있습니다.
*   **인프라 리소스**: VLAN 및 IP 주소와 같은 클러스터를 실행하는 데 필요한 리소스는 {{site.data.keyword.IBM_notm}} 소유 IBM Cloud 인프라(SoftLayer) 계정에서 관리됩니다. 표준 클러스터를 작성할 때 고유의 IBM Cloud 인프라(SoftLayer) 계정에서 이러한 리소스를 관리합니다. 표준 클러스터를 작성할 때 이러한 리소스 및 [필요한 권한](/docs/containers?topic=containers-users#infra_access)에 대하여 더 자세히 알 수 있습니다.
*   **기타 옵션**: 무료 클러스터는 사용자가 선택한 지역 내에 배치되지만 구역은 선택할 수 없습니다. 구역, 네트워킹 및 지속적 스토리지에 대한 제어를 위해서는 표준 클러스터를 작성하십시오. [무료 클러스터 및 표준 클러스터의 이점에 대해 자세히 알아보십시오](/docs/containers?topic=containers-cs_ov#cluster_types).

<br>

**다음에 수행할 작업**</br>
만료되기 전에 무료 클러스터로 일부 작업을 시도해 보십시오.

* Kubernetes 클러스터 작성, CLI 설치, Kubernetes 터미널 사용, 개인용 레지스트리 작성, 클러스터 환경 설정 및 클러스터에 서비스 추가에 대해서는 [첫 번째 {{site.data.keyword.containerlong_notm}} 튜토리얼](/docs/containers?topic=containers-cs_cluster_tutorial#cs_cluster_tutorial)을 살펴보십시오.
* 클러스터에 앱 배치에 대한 [두 번째 {{site.data.keyword.containerlong_notm}} 튜토리얼](/docs/containers?topic=containers-cs_apps_tutorial#cs_apps_tutorial)을 계속 진행하십시오.
* 고가용성을 위해 다중 노드의 [표준 클러스터를 작성](/docs/containers?topic=containers-clusters#clusters_ui)하십시오.


