---

copyright:
  years: 2014, 2019
lastupdated: "2019-07-19"

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



# Hybrid cloud
{: #hybrid_iks_icp}

If you have an {{site.data.keyword.Bluemix}} Private account, you can use it with select {{site.data.keyword.cloud_notm}} services, including {{site.data.keyword.containerlong}}. For more information, see the blog on [Hybrid experience across {{site.data.keyword.cloud_notm}} Private and IBM Public Cloud ![External link icon](../icons/launch-glyph.svg "External link icon")](http://ibm.biz/hybridJune2018).
{: shortdesc}

You understand the [{{site.data.keyword.cloud_notm}} offerings](/docs/containers?topic=containers-cs_ov#differentiation) and developed your Kubernetes strategy for what [workloads to run on the cloud](/docs/containers?topic=containers-strategy#cloud_workloads). Now, you can connect your public and private cloud by using the strongSwan VPN service or {{site.data.keyword.BluDirectLink}}.

* The [strongSwan VPN service](#hybrid_vpn) securely connects your Kubernetes cluster with an on-premises network through a secure end-to-end communication channel over the internet that is based on the industry-standard Internet Protocol Security (IPSec) protocol suite.
* With [{{site.data.keyword.cloud_notm}} Direct Link](#hybrid_dl), you can create a direct, private connection between your remote network environments and {{site.data.keyword.containerlong_notm}} without routing over the public internet.

After you connect your public and private cloud, you can [reuse your private packages for public containers](#hybrid_ppa_importer).

## Connecting your public and private cloud with the strongSwan VPN
{: #hybrid_vpn}

Establish VPN connectivity between your public Kubernetes cluster and your {{site.data.keyword.Bluemix}} Private instance to allow two-way communication.
{: shortdesc}

1.  Create a standard cluster with {{site.data.keyword.containerlong}} in {{site.data.keyword.cloud_notm}} Public or use an existing one. To create a cluster, choose between the following options:
    - [Create a standard cluster from the console or CLI](/docs/containers?topic=containers-clusters#clusters_ui).
    - [Use the Cloud Automation Manager (CAM) to create a cluster by using a pre-defined template ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/support/knowledgecenter/SS2L37_2.1.0.3/cam_deploy_IKS.html). When you deploy a cluster with CAM, the Helm tiller is automatically installed for you.

2.  In your {{site.data.keyword.containerlong_notm}} cluster, [follow the instructions to set up the strongSwan IPSec VPN service](/docs/containers?topic=containers-vpn#vpn_configure).

    *  For [Step 2](/docs/containers?topic=containers-vpn#strongswan_2), note that:

       * The `local.id` that you set in your {{site.data.keyword.containerlong_notm}} cluster must match what you later set as the `remote.id` in your {{site.data.keyword.Bluemix}} Private cluster.
       * The `remote.id` that you set in your {{site.data.keyword.containerlong_notm}} cluster must match what you later set as the `local.id` in your {{site.data.keyword.Bluemix}} Private cluster.
       * The `preshared.secret` that you set in your {{site.data.keyword.containerlong_notm}} cluster must match what you later set as the `preshared.secret` in your {{site.data.keyword.Bluemix}} Private cluster.

    *  For [Step 3](/docs/containers?topic=containers-vpn#strongswan_3), configure strongSwan for an **inbound** VPN connection.

       ```
       ipsec.auto: add
       loadBalancerIP: <portable_public_IP>
       ```
       {: codeblock}

3.  Note the portable public IP address that you set as the `loadbalancerIP`.

    ```
    kubectl get svc vpn-strongswan
    ```
    {: pre}

4.  [Create a cluster in {{site.data.keyword.cloud_notm}} Private ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/installing/installing.html).

5.  In your {{site.data.keyword.cloud_notm}} Private cluster, deploy the strongSwan IPSec VPN service.

    1.  [Complete the strongSwan IPSec VPN workarounds ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/support/knowledgecenter/SS2L37_2.1.0.3/cam_strongswan.html).

    2.  [Set up the strongSwan VPN Helm chart ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/app_center/create_release.html) in your private cluster.

        *  In the configuration parameters, set the **Remote gateway** field to the value of the portable public IP address that you set as the `loadbalancerIP` of your {{site.data.keyword.containerlong_notm}} cluster.

           ```
           Operation at startup: start
           ...
           Remote gateway: <portable_public_IP>
           ...
           ```
           {: codeblock}

        *  Remember that the private `local.id` must match the public `remote.id`, the private `remote.id` must match the public `local.id`, and the `preshared.secret` values for private and public must match.

        Now, you can initiate a connection from the {{site.data.keyword.cloud_notm}} Private cluster to the {{site.data.keyword.containerlong_notm}} cluster.

6.  [Test the VPN connection](/docs/containers?topic=containers-vpn#vpn_test) between your clusters.

7.  Repeat these steps for each cluster that you want to connect.

**What's next?**

*   [Run your licensed software images in public clusters](#hybrid_ppa_importer).
*   To manage multiple cloud Kubernetes clusters such as across {{site.data.keyword.cloud_notm}} Public and {{site.data.keyword.cloud_notm}} Private, check out the [IBM Multicloud Manager ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.1.0/mcm/getting_started/introduction.html).


## Connecting your public and private cloud with {{site.data.keyword.cloud_notm}} Direct Link
{: #hybrid_dl}

With [{{site.data.keyword.BluDirectLink}}](/docs/infrastructure/direct-link?topic=direct-link-about-ibm-cloud-direct-link), you can create a direct, private connection between your remote network environments and {{site.data.keyword.containerlong_notm}} without routing over the public internet.
{: shortdesc}

To connect your public cloud and your on-premises {{site.data.keyword.Bluemix}} Private instance, you can use one of the four offerings:
* {{site.data.keyword.cloud_notm}} Direct Link Connect
* {{site.data.keyword.cloud_notm}} Direct Link Exchange
* {{site.data.keyword.cloud_notm}} Direct Link Dedicated
* {{site.data.keyword.cloud_notm}} Direct Link Dedicated Hosting

To choose an {{site.data.keyword.cloud_notm}} Direct Link offering and set up an {{site.data.keyword.cloud_notm}} Direct Link connection, see [Get Started with {{site.data.keyword.cloud_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link#how-do-i-know-which-type-of-ibm-cloud-direct-link-i-need-) in the {{site.data.keyword.cloud_notm}} Direct Link documentation.

**What's next?**</br>
* [Run your licensed software images in public clusters](#hybrid_ppa_importer).
* To manage multiple cloud Kubernetes clusters such as across {{site.data.keyword.cloud_notm}} Public and {{site.data.keyword.cloud_notm}} Private, check out the [IBM Multicloud Manager ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.1.0/mcm/getting_started/introduction.html).

<br />


## Running {{site.data.keyword.cloud_notm}} Private images in public Kubernetes containers
{: #hybrid_ppa_importer}

You can run select licensed IBM products that were packaged for {{site.data.keyword.cloud_notm}} Private in a cluster in {{site.data.keyword.cloud_notm}} Public by using the PPA importer tool.  
{: shortdesc}


Licensed software is available in [IBM Passport Advantage ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www-01.ibm.com/software/passportadvantage/index.html). To use this software in a cluster in {{site.data.keyword.cloud_notm}} Public, you must download the software, extract the image, and upload the image to your namespace in {{site.data.keyword.registryshort}} by using the PPA importer tool, the `ibmcloud cr ppa-archive-load` command. Independent of the environment where you plan to use the software, you must obtain the required license for the product first.

The following table is an overview of available {{site.data.keyword.cloud_notm}} Private products that you can use in your cluster in {{site.data.keyword.cloud_notm}} Public.

| Product Name | Version | Part Number |
| --- | --- | --- |
| IBM Db2 Direct Advanced Edition Server | 11.1 | CNU3TML |
| IBM Db2 Advanced Enterprise Server Edition Server | 11.1 | CNU3SML |
| IBM MQ Advanced | 9.1.0.0, 9.1.1,0, 9.1.2.0 | - |
| IBM WebSphere Application Server Liberty | 16.0.0.3 | Docker Hub image |
{: caption="Table. Supported {{site.data.keyword.cloud_notm}} Private products to be used in {{site.data.keyword.cloud_notm}} Public." caption-side="top"}

Before you begin:
- [Install the {{site.data.keyword.registryshort}} CLI plug-in (`ibmcloud cr`)](/docs/services/Registry?topic=registry-registry_setup_cli_namespace#cli_namespace_registry_cli_install).
- [Set up a namespace in {{site.data.keyword.registryshort}}](/docs/services/Registry?topic=registry-registry_setup_cli_namespace#registry_namespace_setup) or retrieve your existing namespace by running `ibmcloud cr namespaces`.
- [Target your `kubectl` CLI to your cluster](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).
- [Install the Helm CLI and set up tiller in your cluster](/docs/containers?topic=containers-helm#public_helm_install).

To deploy an {{site.data.keyword.cloud_notm}} Private image in a cluster in {{site.data.keyword.cloud_notm}} Public:

1.  Follow the steps in the [{{site.data.keyword.registryshort}} documentation](/docs/services/Registry?topic=registry-ts_index#ts_ppa) to download the licensed software from IBM Passport Advantage, push the image to your namespace, and install the Helm chart in your cluster.

    **For IBM WebSphere Application Server Liberty**:

    1.  Instead of obtaining the image from IBM Passport Advantage, use the [Docker Hub image ![External link icon](../icons/launch-glyph.svg "External link icon")](https://hub.docker.com/_/websphere-liberty/). For instructions on getting a production license, see [Upgrading the image from Docker Hub to a production image ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/WASdev/ci.docker/tree/master/ga/production-upgrade).

    2.  Follow the [Liberty Helm chart instructions ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/support/knowledgecenter/en/SSEQTP_liberty/com.ibm.websphere.wlp.doc/ae/rwlp_icp_helm.html).

2.  Verify that the **STATUS** of the Helm chart shows `DEPLOYED`. If not, wait a few minutes, and then try again.
    ```
    helm status <helm_chart_name>
    ```
    {: pre}

3.  For more information about how to configure and use the product with your cluster, refer to the product-specific documentation.

    - [IBM Db2 Direct Advanced Edition Server ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/support/knowledgecenter/en/SSEPGG_11.1.0/com.ibm.db2.luw.licensing.doc/doc/c0070181.html)
    - [IBM MQ Advanced ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/support/knowledgecenter/en/SSFKSJ_9.0.0/com.ibm.mq.helphome.v90.doc/WelcomePagev9r0.html)
    - [IBM WebSphere Application Server Liberty ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/support/knowledgecenter/en/SSEQTP_liberty/as_ditamaps/was900_welcome_liberty.html)
