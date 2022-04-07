---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

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



# 하이브리드 클라우드
{: #hybrid_iks_icp}

{{site.data.keyword.Bluemix}} Private 계정이 있으면 {{site.data.keyword.containerlong}}를 포함하여 선택된 {{site.data.keyword.Bluemix_notm}} 서비스에서 이를 사용할 수 있습니다. 자세한 정보는 [{{site.data.keyword.Bluemix_notm}} Private 및 IBM 퍼블릭 클라우드의 하이브리드 환경 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](http://ibm.biz/hybridJune2018)의 블로그를 참조하십시오.
{: shortdesc}

사용자는 [{{site.data.keyword.Bluemix_notm}} 오퍼링](/docs/containers?topic=containers-cs_ov#differentiation) 및 [클라우드에서 실행될 워크로드](/docs/containers?topic=containers-strategy#cloud_workloads)에 대해 개발된 Kubernetes 전략을 이해하고 있습니다. 이제 strongSwan VPN 서비스 또는 {{site.data.keyword.BluDirectLink}}를 사용하여 퍼블릭 또는 프라이빗 클라우드에 연결할 수 있습니다. 

* [strongSwan VPN 서비스](#hybrid_vpn)는 업계 표준 IPSec(Internet Protocol Security) 프로토콜 스위트를 기반으로 하는, 인터넷을 통한 안전한 엔드-투-엔드 통신 채널을 통해 Kubernetes 클러스터를 온프레미스 네트워크에 안전하게 연결합니다. 
* [{{site.data.keyword.Bluemix_notm}} Direct Link](#hybrid_dl)를 사용하면 공용 인터넷을 통한 라우팅 없이 원격 네트워크 환경과 {{site.data.keyword.containerlong_notm}} 간에 개인용 직접 연결을 작성할 수 있습니다. 

퍼블릭 및 프라이빗 클라우드를 연결하고 나면 [공용 컨테이너에 대해 개인용 패키지를 재사용](#hybrid_ppa_importer)할 수 있습니다. 

## 퍼블릭 및 프라이빗 클라우드를 strongSwan VPN과 연결
{: #hybrid_vpn}

공용 Kubernetes 클러스터와 {{site.data.keyword.Bluemix}} Private 인스턴스 간에 VPN 연결을 설정하여 양방향 통신을 허용합니다.
{: shortdesc}

1.  {{site.data.keyword.Bluemix_notm}} 퍼블릭의 {{site.data.keyword.containerlong}}에서 표준 클러스터를 작성하거나 기존 클러스터를 사용하십시오. 클러스터를 작성하려면 다음 옵션 중에서 선택하십시오.
    - [콘솔 또는 CLI에서 표준 클러스터를 작성](/docs/containers?topic=containers-clusters#clusters_ui)합니다.
    - [CAM(Cloud Automation Manager)을 사용하여 사전 정의된 템플리트로 클러스터를 작성 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/support/knowledgecenter/SS2L37_2.1.0.3/cam_deploy_IKS.html)합니다. CAM을 사용하여 클러스터를 배치하면 Helm Tiller가 사용자를 위해 자동으로 설치됩니다.

2.  {{site.data.keyword.containerlong_notm}} 클러스터에서 [지시사항에 따라 strongSwan IPSec VPN 서비스를 설정](/docs/containers?topic=containers-vpn#vpn_configure)하십시오.

    *  [2단계](/docs/containers?topic=containers-vpn#strongswan_2)의 경우 다음을 유념하십시오.

       * {{site.data.keyword.containerlong_notm}} 클러스터에서 설정하는 `local.id`는 {{site.data.keyword.Bluemix}} Private 클러스터에서 나중에 `remote.id`로 설정하는 것과 일치해야 합니다.
       * {{site.data.keyword.containerlong_notm}} 클러스터에서 설정하는 `remote.id`는 {{site.data.keyword.Bluemix}} Private 클러스터에서 나중에 `local.id`로 설정하는 것과 일치해야 합니다.
       * {{site.data.keyword.containerlong_notm}} 클러스터에서 설정하는 `preshared.secret`는 {{site.data.keyword.Bluemix}} Private 클러스터에서 나중에 `preshared.secret`으로 설정하는 것과 일치해야 합니다.

    *  [3단계](/docs/containers?topic=containers-vpn#strongswan_3)의 경우, **인바운드** VPN 연결에 대해 strongSwan을 고려하십시오.

       ```
       ipsec.auto: add
       loadBalancerIP: <portable_public_IP>
       ```
       {: codeblock}

3.  `loadbalancerIP`로 설정한 포터블 공인 IP 주소를 기록해 두십시오.

    ```
    kubectl get svc vpn-strongswan
    ```
    {: pre}

4.  [{{site.data.keyword.Bluemix_notm}} Private에서 클러스터를 작성 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/installing/installing.html)하십시오.

5.  {{site.data.keyword.Bluemix_notm}} Private 클러스터에서 strongSwan IPSec VPN 서비스를 배치하십시오.

    1.  [strongSwan IPSec VPN 임시 해결책을 완료 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/support/knowledgecenter/SS2L37_2.1.0.3/cam_strongswan.html)하십시오.

    2.  사설 클러스터에서 [strongSwan VPN Helm 차트를 설정 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/app_center/create_release.html)하십시오.

        *  구성 매개변수에서 {{site.data.keyword.containerlong_notm}} 클러스터의 `loadbalancerIP`로 설정한 포터블 공인 IP 주소의 값으로 **원격 게이트웨이** 필드를 설정하십시오.

           ```
           Operation at startup: start
           ...
           Remote gateway: <portable_public_IP>
           ...
           ```
           {: codeblock}

        *  개인용 `local.id`는 공용 `remote.id`와 일치해야 하고 개인용 `remote.id`는 공용 `local.id`와 일치해야 하며 개인용 및 공용의 `preshared.secret` 값은 일치해야 한다는 점을 유념하십시오.

        이제 {{site.data.keyword.Bluemix_notm}} Private 클러스터에서 {{site.data.keyword.containerlong_notm}} 클러스터로의 연결을 시작할 수 있습니다.

7.  클러스터 간에 [VPN 연결을 테스트](/docs/containers?topic=containers-vpn#vpn_test)하십시오.

8.  연결할 각각의 클러스터에 대해 이러한 단계를 반복하십시오.

**다음에 수행할 작업**

*   [공용 클러스터에서 라이센싱된 소프트웨어 이미지를 실행](#hybrid_ppa_importer)하십시오.
*   {{site.data.keyword.Bluemix_notm}} Public 및 {{site.data.keyword.Bluemix_notm}} Private 등에서 다중 클라우드 Kubernetes 클러스터를 관리하려면 [IBM Multicloud Manager ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.1.0/mcm/getting_started/introduction.html)를 확인하십시오.


## {{site.data.keyword.Bluemix_notm}} Direct Link를 사용하여 퍼블릭 및 프라이빗 클라우드 연결
{: #hybrid_dl}

[{{site.data.keyword.BluDirectLink}}](/docs/infrastructure/direct-link?topic=direct-link-about-ibm-cloud-direct-link)를 사용하면 공용 인터넷을 통한 라우팅 없이 원격 네트워크 환경과 {{site.data.keyword.containerlong_notm}} 간에 개인용 직접 연결을 작성할 수 있습니다.
{: shortdesc}

퍼블릭 클라우드와 온프레미스 {{site.data.keyword.Bluemix}} Private 인스턴스를 연결하려는 경우에는 다음 네 가지 오퍼링 중 하나를 사용할 수 있습니다. 
* {{site.data.keyword.Bluemix_notm}} Direct Link Connect
* {{site.data.keyword.Bluemix_notm}} Direct Link Exchange
* {{site.data.keyword.Bluemix_notm}} Direct Link Dedicated
* {{site.data.keyword.Bluemix_notm}} Direct Link Dedicated Hosting

{{site.data.keyword.Bluemix_notm}} Direct Link 오퍼링을 선택하고 {{site.data.keyword.Bluemix_notm}} Direct Link 연결을 설정하려면 {{site.data.keyword.Bluemix_notm}} Direct Link 문서의 [{{site.data.keyword.Bluemix_notm}} Direct Link 시작하기](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link#how-do-i-know-which-type-of-ibm-cloud-direct-link-i-need-)를 참조하십시오. 

**다음에 수행할 작업**</br>
* [공용 클러스터에서 라이센싱된 소프트웨어 이미지를 실행](#hybrid_ppa_importer)하십시오.
* {{site.data.keyword.Bluemix_notm}} Public 및 {{site.data.keyword.Bluemix_notm}} Private 등에서 다중 클라우드 Kubernetes 클러스터를 관리하려면 [IBM Multicloud Manager ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.1.0/mcm/getting_started/introduction.html)를 확인하십시오.

<br />


## 공용 Kubernetes 컨테이너에서 {{site.data.keyword.Bluemix_notm}} Private 이미지 실행
{: #hybrid_ppa_importer}

{{site.data.keyword.Bluemix_notm}} 퍼블릭의 클러스터에서 {{site.data.keyword.Bluemix_notm}} Private용으로 패키징된 선택되고 라이센싱된 IBM 제품을 실행할 수 있습니다.  
{: shortdesc}

라이센싱된 소프트웨어는 [IBM Passport Advantage ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www-01.ibm.com/software/passportadvantage/index.html)에서 사용 가능합니다. {{site.data.keyword.Bluemix_notm}} 퍼블릭의 클러스터에서 이 소프트웨어를 사용하려면 소프트웨어를 다운로드하고 이미지를 추출한 후에 해당 이미지를 {{site.data.keyword.registryshort}}의 네임스페이스로 업로드하십시오. 소프트웨어를 사용하고자 하는 환경과 무관하게, 사용자는 우선 제품에 대한 필수 라이센스를 받아야 합니다.

다음 표는 {{site.data.keyword.Bluemix_notm}} 퍼블릭의 클러스터에서 사용할 수 있는 사용 가능한 {{site.data.keyword.Bluemix_notm}} Private 제품의 개요입니다.

|제품 이름 |버전 |부품 번호 |
| --- | --- | --- |
| IBM Db2 Direct Advanced Edition Server | 11.1 | CNU3TML |
| IBM Db2 Advanced Enterprise Server Edition Server | 11.1 | CNU3SML |
| IBM MQ Advanced | 9.1.0.0, 9.1.1,0, 9.1.2.0 | - |
| IBM WebSphere Application Server Liberty | 16.0.0.3 | Docker Hub 이미지 |
{: caption="표. {{site.data.keyword.Bluemix_notm}} 퍼블릭에서 사용할 수 있도록 지원되는 {{site.data.keyword.Bluemix_notm}} Private 제품." caption-side="top"}

시작하기 전에:
- [{{site.data.keyword.registryshort}} CLI 플러그인(`ibmcloud cr`)을 설치](/docs/services/Registry?topic=registry-registry_setup_cli_namespace#cli_namespace_registry_cli_install)하십시오.
- [ {{site.data.keyword.registryshort}}의 네임스페이스를 설정](/docs/services/Registry?topic=registry-registry_setup_cli_namespace#registry_namespace_setup)하거나 `ibmcloud cr namespaces`를 실행하여 기존 네임스페이스를 검색하십시오.
- [클러스터에 `kubectl` CLI를 대상으로 지정](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)하십시오.
- [Helm CLI를 설치하고 클러스터에서 Tiller를 설정](/docs/containers?topic=containers-helm#public_helm_install)하십시오.

{{site.data.keyword.Bluemix_notm}} 퍼블릭의 클러스터에서 {{site.data.keyword.Bluemix_notm}} Private 이미지를 배치하려면 다음을 수행하십시오.

1.  [{{site.data.keyword.registryshort}} 문서](/docs/services/Registry?topic=registry-ts_index#ts_ppa)의 단계에 따라 IBM Passport Advantage에서 라이센싱된 소프트웨어를 다운로드하고 이미지를 네임스페이스에 푸시한 후에 클러스터에 Helm 차트를 설치하십시오.

    **IBM WebSphere Application Server Liberty의 경우**:

    1.  IBM Passport Advantage에서 이미지를 가져오는 대신에 [Docker Hub 이미지 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://hub.docker.com/_/websphere-liberty/)를 사용하십시오. 프로덕션 라이센스 가져오기에 대한 지시사항은 [Docker Hub의 이미지를 프로덕션 이미지로 업그레이드 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/WASdev/ci.docker/tree/master/ga/production-upgrade)를 참조하십시오.

    2.  [Liberty Helm 차트 지시사항 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/support/knowledgecenter/en/SSEQTP_liberty/com.ibm.websphere.wlp.doc/ae/rwlp_icp_helm.html)을 따르십시오.

2.  Helm 차트의 **상태**가 `DEPLOYED`를 표시하는지 확인하십시오. 그렇지 않은 경우에는 잠시 기다린 후 다시 시도하십시오.
    ```
    helm status <helm_chart_name>
    ```
    {: pre}

3.  클러스터에서 제품을 구성하고 사용하는 방법에 대한 자세한 정보는 제품 특정 문서를 참조하십시오.

    - [IBM Db2 Direct Advanced Edition Server ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/support/knowledgecenter/en/SSEPGG_11.1.0/com.ibm.db2.luw.licensing.doc/doc/c0070181.html)
    - [IBM MQ Advanced ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/support/knowledgecenter/en/SSFKSJ_9.0.0/com.ibm.mq.helphome.v90.doc/WelcomePagev9r0.html)
    - [IBM WebSphere Application Server Liberty ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/support/knowledgecenter/en/SSEQTP_liberty/as_ditamaps/was900_welcome_liberty.html)
