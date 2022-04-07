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



# 混合云
{: #hybrid_iks_icp}

如果您有 {{site.data.keyword.Bluemix}} Private 帐户，那么可以将其用于精选 {{site.data.keyword.Bluemix_notm}} 服务，包括 {{site.data.keyword.containerlong}}。有关更多信息，请参阅[跨 {{site.data.keyword.Bluemix_notm}} Private 和 IBM Public Cloud 的混合体验 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](http://ibm.biz/hybridJune2018)。
{: shortdesc}

了解 [{{site.data.keyword.Bluemix_notm}} 产品](/docs/containers?topic=containers-cs_ov#differentiation)，并为[在云上运行的工作负载](/docs/containers?topic=containers-strategy#cloud_workloads)制定 Kubernetes 策略。现在，可以使用 strongSwan VPN 服务或 {{site.data.keyword.BluDirectLink}} 来连接公共云和私有云。

* [strongSwan VPN 服务](#hybrid_vpn)基于业界标准因特网协议安全性 (IPSec) 协议组，通过因特网提供安全的端到端通信信道，从而安全地将 Kubernetes 集群连接到内部部署网络。
* 通过 [{{site.data.keyword.Bluemix_notm}} Direct Link](#hybrid_dl)，您可以在远程网络环境和 {{site.data.keyword.containerlong_notm}} 之间创建直接专用连接，而无需通过公用因特网进行路由。

连接公共云和私有云后，可以[在公共容器中复用专用包](#hybrid_ppa_importer)。

## 使用 strongSwan VPN 连接公共云和私有云
{: #hybrid_vpn}

在公共 Kubernetes 集群与 {{site.data.keyword.Bluemix}} Private 实例之间建立 VPN 连接，以允许双向通信。
{: shortdesc}

1.  在 {{site.data.keyword.Bluemix_notm}} Public 中通过 {{site.data.keyword.containerlong}} 创建标准集群，或使用现有集群。要创建集群，请从以下选项中进行选择：
    - [通过控制台或 CLI 创建标准集群](/docs/containers?topic=containers-clusters#clusters_ui)。
    - [使用 Cloud Automation Manager (CAM) 通过预定义模板创建集群 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/support/knowledgecenter/SS2L37_2.1.0.3/cam_deploy_IKS.html)。使用 CAM 部署集群时，会自动安装 Helm Tiller。

2.  在 {{site.data.keyword.containerlong_notm}} 集群中，[按照指示信息来设置 strongSwan IPSec VPN 服务](/docs/containers?topic=containers-vpn#vpn_configure)。

    *  对于[步骤 2](/docs/containers?topic=containers-vpn#strongswan_2)，请注意：

       * 在 {{site.data.keyword.containerlong_notm}} 集群中设置的 `local.id` 必须与稍后在 {{site.data.keyword.Bluemix}} Private 集群中设置为 `remote.id` 的内容相匹配。
       * 在 {{site.data.keyword.containerlong_notm}} 集群中设置的 `remote.id` 必须与稍后在 {{site.data.keyword.Bluemix}} Private 集群中设置为 `local.id` 的内容相匹配。
       * 在 {{site.data.keyword.containerlong_notm}} 集群中设置的 `preshared.secret` 必须与稍后在 {{site.data.keyword.Bluemix}} Private 集群中设置为 `preshared.secret` 的内容相匹配。

    *  对于[步骤 3](/docs/containers?topic=containers-vpn#strongswan_3)，请为**入站** VPN 连接配置 strongSwan。

       ```
       ipsec.auto: add
       loadBalancerIP: <portable_public_IP>
       ```
       {: codeblock}

3.  记下您设置为 `loadbalancerIP` 的可移植公共 IP 地址。

    ```
    kubectl get svc vpn-strongswan
    ```
    {: pre}

4.  [在 {{site.data.keyword.Bluemix_notm}} Private 中创建集群 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/installing/installing.html)。

5.  在 {{site.data.keyword.Bluemix_notm}} Private 集群中，部署 strongSwan IPSec VPN 服务。

    1.  [完成 strongSwan IPSec VPN 变通方法 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/support/knowledgecenter/SS2L37_2.1.0.3/cam_strongswan.html)。

    2.  在专用集群中[设置 strongSwan VPN Helm chart ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/app_center/create_release.html)。

        *  在配置参数中，将**远程网关**字段设置为您设置为 {{site.data.keyword.containerlong_notm}} 集群的 `loadbalancerIP` 的可移植公共 IP 地址的值。

           ```
           Operation at startup: start
           ...
           Remote gateway: <portable_public_IP>
           ...
           ```
           {: codeblock}

        *  请记住，专用 `local.id` 必须与公共 `remote.id` 相匹配，专用 `remote.id` 必须与公共 `local.id` 相匹配，并且专用和公共的 `preshared.secret` 值必须相匹配。

        现在，可以启动从 {{site.data.keyword.Bluemix_notm}} Private 集群到 {{site.data.keyword.containerlong_notm}} 集群的连接。

7.  [测试集群之间的 VPN 连接](/docs/containers?topic=containers-vpn#vpn_test)。

8.  针对要连接的每个集群，重复上述步骤。

**接下来要做什么？**

*   [在公共集群中运行许可软件映像](#hybrid_ppa_importer)。
*   要管理多个云 Kubernetes 集群（如跨 {{site.data.keyword.Bluemix_notm}} Public 和 {{site.data.keyword.Bluemix_notm}} Private），请查看 [IBM Multicloud Manager ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.1.0/mcm/getting_started/introduction.html)。


## 使用 {{site.data.keyword.Bluemix_notm}} Direct Link 连接公共云和私有云
{: #hybrid_dl}

通过 [{{site.data.keyword.BluDirectLink}}](/docs/infrastructure/direct-link?topic=direct-link-about-ibm-cloud-direct-link)，您可以在远程网络环境和 {{site.data.keyword.containerlong_notm}} 之间创建直接专用连接，而无需通过公用因特网进行路由。
{: shortdesc}

要连接公共云和内部部署 {{site.data.keyword.Bluemix}} Private 实例，可以使用以下四个产品之一：
* {{site.data.keyword.Bluemix_notm}} Direct Link Connect
* {{site.data.keyword.Bluemix_notm}} Direct Link Exchange
* {{site.data.keyword.Bluemix_notm}} Direct Link Dedicated
* {{site.data.keyword.Bluemix_notm}} Direct Link Dedicated Hosting

要选择 {{site.data.keyword.Bluemix_notm}} Direct Link 产品并设置 {{site.data.keyword.Bluemix_notm}} Direct Link 连接，请参阅 {{site.data.keyword.Bluemix_notm}} Direct Link 文档中的 [{{site.data.keyword.Bluemix_notm}} Direct Link 入门](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link#how-do-i-know-which-type-of-ibm-cloud-direct-link-i-need-)。

**接下来要做什么？**</br>
* [在公共集群中运行许可软件映像](#hybrid_ppa_importer)。
* 要管理多个云 Kubernetes 集群（如跨 {{site.data.keyword.Bluemix_notm}} Public 和 {{site.data.keyword.Bluemix_notm}} Private），请查看 [IBM Multicloud Manager ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.1.0/mcm/getting_started/introduction.html)。

<br />


## 在公共 Kubernetes 容器中运行 {{site.data.keyword.Bluemix_notm}} Private 映像
{: #hybrid_ppa_importer}

在 {{site.data.keyword.Bluemix_notm}} Public 的集群中，可以运行针对 {{site.data.keyword.Bluemix_notm}} Private 打包的 IBM 精选许可产品。  
{: shortdesc}

许可的软件可在 [IBM Passport Advantage ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www-01.ibm.com/software/passportadvantage/index.html) 上找到。要在 {{site.data.keyword.Bluemix_notm}} Public 的集群中使用此软件，必须下载此软件，解压缩映像，然后将映像上传到 {{site.data.keyword.registryshort}} 中的名称空间。不管您计划在何种环境中使用此软件，都必须首先获取必需的产品许可证。

下表提供了可在 {{site.data.keyword.Bluemix_notm}} Public 的集群中使用的可用 {{site.data.keyword.Bluemix_notm}} Private 产品的概览。

| 产品名称|版本|部件号|
| --- | --- | --- |
|IBM Db2 Direct Advanced Edition Server|11.1|CNU3TML|
|IBM Db2 Advanced Enterprise Server Edition Server|11.1|CNU3SML|
|IBM MQ Advanced|9.1.0.0，9.1.1.0，9.1.2.0| - |
|IBM WebSphere Application Server Liberty|16.0.0.3|Docker Hub 映像|
{: caption="表. 可在 {{site.data.keyword.Bluemix_notm}} Public 中使用的受支持 {{site.data.keyword.Bluemix_notm}} Private 产品。" caption-side="top"}

开始之前：
- [安装 {{site.data.keyword.registryshort}} CLI 插件 (`ibmcloud cr`)](/docs/services/Registry?topic=registry-registry_setup_cli_namespace#cli_namespace_registry_cli_install)。
- [在 {{site.data.keyword.registryshort}} 中设置名称空间](/docs/services/Registry?topic=registry-registry_setup_cli_namespace#registry_namespace_setup)，或通过运行 `ibmcloud cr namespaces` 来检索现有名称空间。
- [将 `kubectl` CLI 的目标指定为集群](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)。
- [在集群中安装 Helm CLI 并设置 Tiller](/docs/containers?topic=containers-helm#public_helm_install)。

要在 {{site.data.keyword.Bluemix_notm}} Public 的集群中部署 {{site.data.keyword.Bluemix_notm}} Private 映像，请执行以下操作：

1.  执行 [{{site.data.keyword.registryshort}} 文档](/docs/services/Registry?topic=registry-ts_index#ts_ppa)中的步骤从 IBM Passport Advantage 下载许可软件，将映像推送到名称空间，然后在集群中安装 Helm chart。

    **对于 IBM WebSphere Application Server Liberty**：

    1.  不要从 IBM Passport Advantage 获取映像，请改为使用 [Docker Hub 映像 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://hub.docker.com/_/websphere-liberty/)。有关获取生产许可证的指示信息，请参阅[将映像从 Docker Hub 升级为生产映像 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/WASdev/ci.docker/tree/master/ga/production-upgrade)。

    2.  按照 [Liberty Helm chart 指示信息 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/support/knowledgecenter/en/SSEQTP_liberty/com.ibm.websphere.wlp.doc/ae/rwlp_icp_helm.html) 进行操作。

2.  验证 Helm chart 的 **STATUS** 是否显示 `DEPLOYED`。如果未显示，请等待几分钟，然后重试。
    ```
    helm status <helm_chart_name>
    ```
    {: pre}

3.  有关如何配置产品并将其用于集群的更多信息，请参阅特定于产品的文档。

    - [IBM Db2 Direct Advanced Edition Server ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/support/knowledgecenter/en/SSEPGG_11.1.0/com.ibm.db2.luw.licensing.doc/doc/c0070181.html)
    - [IBM MQ Advanced ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/support/knowledgecenter/en/SSFKSJ_9.0.0/com.ibm.mq.helphome.v90.doc/WelcomePagev9r0.html)
    - [IBM WebSphere Application Server Liberty ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/support/knowledgecenter/en/SSEQTP_liberty/as_ditamaps/was900_welcome_liberty.html)
