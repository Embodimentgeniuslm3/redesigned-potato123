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



# 混合式雲端
{: #hybrid_iks_icp}

如果您有「{{site.data.keyword.Bluemix}} 專用」帳戶，則可以將它與精選 {{site.data.keyword.Bluemix_notm}} 服務（包括 {{site.data.keyword.containerlong}}）搭配使用。如需相關資訊，請參閱 [{{site.data.keyword.Bluemix_notm}} 專用及 IBM 公用雲端的混合式經驗 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](http://ibm.biz/hybridJune2018) 上的部落格。
{: shortdesc}

您瞭解 [{{site.data.keyword.Bluemix_notm}} 供應項目](/docs/containers?topic=containers-cs_ov#differentiation)，並針對[要在雲端上執行的工作負載](/docs/containers?topic=containers-strategy#cloud_workloads)開發了 Kubernetes 策略。現在，可以使用 strongSwan VPN 服務或 {{site.data.keyword.BluDirectLink}} 來連接公用雲端和專用雲端。

* [strongSwan VPN 服務](#hybrid_vpn)根據業界標準網際網路通訊協定安全 (IPSec) 通訊協定套組，透過網際網路提供安全的端對端通訊通道，進而安全地將 Kubernetes 叢集連接到內部部署網路。
* 透過 [{{site.data.keyword.Bluemix_notm}} Direct Link](#hybrid_dl)，您可以在遠端網路環境和 {{site.data.keyword.containerlong_notm}} 之間建立直接專用連線，而無需透過公用網際網路進行遞送。

連接公用雲端和專用雲端後，可以[在公用容器中重複使用專用套件](#hybrid_ppa_importer)。

## 使用 strongSwan VPN 連接公用及專用雲端
{: #hybrid_vpn}

建立公用 Kubernetes 叢集與「{{site.data.keyword.Bluemix}} 專用」實例之間的 VPN 連線功能，以容許雙向通訊。
{: shortdesc}

1.  建立標準叢集，讓 {{site.data.keyword.Bluemix_notm}} Public 中有 {{site.data.keyword.containerlong}}，或使用現有的叢集。若要建立叢集，請從下列選項中進行選擇：
    - [透過主控台或 CLI 建立標準叢集](/docs/containers?topic=containers-clusters#clusters_ui)。
    - [使用 Cloud Automation Manager (CAM) 利用預先定義的範本來建立叢集 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/support/knowledgecenter/SS2L37_2.1.0.3/cam_deploy_IKS.html)。當您使用 CAM 部署叢集時，會自動為您安裝 Helm Tiller。

2.  在 {{site.data.keyword.containerlong_notm}} 叢集裡，[遵循指示來設定 strongSwan IPSec VPN 服務](/docs/containers?topic=containers-vpn#vpn_configure)。

    *  對於[步驟 2](/docs/containers?topic=containers-vpn#strongswan_2)，請注意：

       * 您在 {{site.data.keyword.containerlong_notm}} 叢集裡設定的 `local.id` 必須符合您稍後在 {{site.data.keyword.Bluemix}} Private 叢集裡設定為 `remote.id` 的 ID。
       * 您在 {{site.data.keyword.containerlong_notm}} 叢集裡設定的 `remote.id` 必須符合您稍後在 {{site.data.keyword.Bluemix}} Private 叢集裡設定為 `local.id` 的 ID。
       * 您在 {{site.data.keyword.containerlong_notm}} 叢集裡設定的 `preshared.secret` 必須符合您稍後在 {{site.data.keyword.Bluemix}} Private 叢集裡設定為 `preshared.secret` 的密碼。

    *  對於[步驟 3](/docs/containers?topic=containers-vpn#strongswan_3)，請配置**入埠** VPN 連線的 strongSwan。

       ```
       ipsec.auto: add
       loadBalancerIP: <portable_public_IP>
       ```
       {: codeblock}

3.  記下您設為 `loadbalancerIP` 的可攜式公用 IP 位址。

    ```
    kubectl get svc vpn-strongswan
    ```
    {: pre}

4.  [在 {{site.data.keyword.Bluemix_notm}} 專用中建立叢集 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/installing/installing.html)。

5.  在「{{site.data.keyword.Bluemix_notm}} 專用」叢集裡，部署 strongSwan IPSec VPN 服務。

    1.  [完成 strongSwan IPSec VPN 暫行解決方法 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/support/knowledgecenter/SS2L37_2.1.0.3/cam_strongswan.html)。

    2.  在專用叢集裡[設定 strongSwan VPN Helm 圖表![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/app_center/create_release.html)。

        *  在配置參數中，將**遠端閘道**欄位設為您已設為 {{site.data.keyword.containerlong_notm}} 叢集的 `loadbalancerIP` 的可攜式公用 IP 位址的值。

           ```
           Operation at startup: start
           ...
           Remote gateway: <portable_public_IP>
           ...
           ```
           {: codeblock}

        *  請記住，專用 `local.id` 必須符合公用 `remote.id`，專用 `remote.id` 必須符合公用 `local.id`，而專用和公用的 `preshared.secret` 值必須相符。

        現在，您可以起始從 {{site.data.keyword.Bluemix_notm}} Private 叢集到 {{site.data.keyword.containerlong_notm}} 叢集的連線。

7.  [測試叢集之間的 VPN 連線](/docs/containers?topic=containers-vpn#vpn_test)。

8.  針對每一個您要連接的叢集，重複這些步驟。

**下一步為何？**

*   [在公用叢集裡執行授權軟體映像檔](#hybrid_ppa_importer)。
*   若要管理多個雲端 Kubernetes 叢集（例如，跨 {{site.data.keyword.Bluemix_notm}} Public 和 {{site.data.keyword.Bluemix_notm}} Private），請查看 [IBM Multicloud Manager ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.1.0/mcm/getting_started/introduction.html)。


## 使用 {{site.data.keyword.Bluemix_notm}} Direct Link 連接公用雲端和專用雲端
{: #hybrid_dl}

透過 [{{site.data.keyword.BluDirectLink}}](/docs/infrastructure/direct-link?topic=direct-link-about-ibm-cloud-direct-link)，您可以在遠端網路環境和 {{site.data.keyword.containerlong_notm}} 之間建立直接專用連線，而無需透過公用網際網路進行路由。
{: shortdesc}

若要連接公用雲端和內部部署 {{site.data.keyword.Bluemix}} Private 實例，可以使用以下四個供應項目之一：
* {{site.data.keyword.Bluemix_notm}} Direct Link Connect
* {{site.data.keyword.Bluemix_notm}} Direct Link Exchange
* {{site.data.keyword.Bluemix_notm}} Direct Link Dedicated
* {{site.data.keyword.Bluemix_notm}} Direct Link Dedicated Hosting

若要選擇 {{site.data.keyword.Bluemix_notm}} Direct Link 供應項目並設定 {{site.data.keyword.Bluemix_notm}} Direct Link 連線，請參閱 {{site.data.keyword.Bluemix_notm}} Direct Link 文件中的[開始使用 {{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link#how-do-i-know-which-type-of-ibm-cloud-direct-link-i-need-)。

**下一步為何？**</br>
* [在公用叢集裡執行授權軟體映像檔](#hybrid_ppa_importer)。
* 若要管理多個雲端 Kubernetes 叢集（例如，跨 {{site.data.keyword.Bluemix_notm}} Public 和 {{site.data.keyword.Bluemix_notm}} Private），請查看 [IBM Multicloud Manager ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.1.0/mcm/getting_started/introduction.html)。

<br />


## 在公用 Kubernetes 容器中執行 {{site.data.keyword.Bluemix_notm}} 專用映像檔
{: #hybrid_ppa_importer}

您可以執行精選授權 IBM 產品，而這些產品是針對「{{site.data.keyword.Bluemix_notm}} 公用」的叢集裡的「{{site.data.keyword.Bluemix_notm}} 專用」進行包裝。  
{: shortdesc}

[IBM Passport Advantage ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www-01.ibm.com/software/passportadvantage/index.html) 中提供授權軟體。若要在「{{site.data.keyword.Bluemix_notm}} 公用」的叢集裡使用此軟體，您必須下載軟體，並擷取映像檔，然後將映像檔上傳至 {{site.data.keyword.registryshort}} 中的名稱空間。與您計劃使用軟體的環境無關，您必須先取得產品的必要授權。

下表是您可在「{{site.data.keyword.Bluemix_notm}} 公用」的叢集裡使用的可用「{{site.data.keyword.Bluemix_notm}} 專用」產品的概觀。

| 產品名稱 | 版本 | 產品編號 |
| --- | --- | --- |
| IBM Db2 Direct Advanced Edition Server | 11.1 | CNU3TML |
| IBM Db2 Advanced Enterprise Server Edition Server | 11.1 | CNU3SML |
| IBM MQ Advanced | 9.1.0.0、9.1.1,0、9.1.2.0 | - |
| IBM WebSphere Application Server Liberty | 16.0.0.3 | Docker Hub 映像檔 |
{: caption="表. 支援在「{{site.data.keyword.Bluemix_notm}} 公用」中使用的「{{site.data.keyword.Bluemix_notm}} 專用」產品" caption-side="top"}

開始之前：
- [安裝 {{site.data.keyword.registryshort}} CLI 外掛程式 (`ibmcloud cr`)](/docs/services/Registry?topic=registry-registry_setup_cli_namespace#cli_namespace_registry_cli_install)。
- [在 {{site.data.keyword.registryshort}}](/docs/services/Registry?topic=registry-registry_setup_cli_namespace#registry_namespace_setup) 中設定名稱空間，或藉由執行 `ibmcloud cr namespaces` 來擷取現有名稱空間。
- [將 `kubectl` CLI 的目標設為叢集](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)。
- [在叢集裡安裝 Helm CLI 並設定 Tiller](/docs/containers?topic=containers-helm#public_helm_install)。

若要在 {{site.data.keyword.Bluemix_notm}} Public 的叢集裡部署 {{site.data.keyword.Bluemix_notm}} Private 映像檔，請執行下列動作：

1.  遵循 [{{site.data.keyword.registryshort}} 文件](/docs/services/Registry?topic=registry-ts_index#ts_ppa)中的步驟以從 IBM Passport Advantage 下載授權軟體，並將映像檔推送至名稱空間，然後在叢集裡安裝 Helm 圖表。

    **針對 IBM WebSphere Application Server Liberty**：

    1.  使用 [Docker Hub 映像檔 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://hub.docker.com/_/websphere-liberty/)，而非從 IBM Passport Advantage 取得映像檔。如需取得正式作業授權的指示，請參閱[將映像檔從 Docker Hub 升級至正式作業映像檔 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/WASdev/ci.docker/tree/master/ga/production-upgrade)。

    2.  遵循 [Liberty Helm 圖表指示 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/support/knowledgecenter/en/SSEQTP_liberty/com.ibm.websphere.wlp.doc/ae/rwlp_icp_helm.html)。

2.  驗證 Helm 圖表的 **STATUS** 顯示為 `DEPLOYED`。否則，等待幾分鐘，然後再試一次試。
    ```
    helm status <helm_chart_name>
    ```
    {: pre}

3.  如需如何配置產品以及使用產品與叢集搭配的相關資訊，請參閱產品特定文件。

    - [IBM Db2 Direct Advanced Edition Server ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/support/knowledgecenter/en/SSEPGG_11.1.0/com.ibm.db2.luw.licensing.doc/doc/c0070181.html)
    - [IBM MQ Advanced ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/support/knowledgecenter/en/SSFKSJ_9.0.0/com.ibm.mq.helphome.v90.doc/WelcomePagev9r0.html)
    - [IBM WebSphere Application Server Liberty ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/support/knowledgecenter/en/SSEQTP_liberty/as_ditamaps/was900_welcome_liberty.html)
