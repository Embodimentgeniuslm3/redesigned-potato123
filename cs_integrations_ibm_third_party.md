---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: kubernetes, iks, helm

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


# IBM Cloud services and third-party integrations
{: #ibm-3rd-party-integrations}

You can use {{site.data.keyword.cloud_notm}} platform and infrastructure services, and other third-party integrations to add extra capabilities to your cluster.
{: shortdesc}

## IBM Cloud services
{: #ibm-cloud-services}

Review the following information to see how {{site.data.keyword.cloud_notm}} platform and infrastructure services are integrated with {{site.data.keyword.containerlong_notm}} and how you can use them in your cluster.
{: shortdesc}

### IBM Cloud platform services
{: #platform-services}

All {{site.data.keyword.cloud_notm}} platform services that support service keys can be integrated by using {{site.data.keyword.containerlong_notm}} [service binding](/docs/containers?topic=containers-service-binding).
{: shortdesc}

Service binding is a quick way to create service credentials for an {{site.data.keyword.cloud_notm}} service and store these credentials in a Kubernetes secret in your cluster. The Kubernetes secret is automatically encrypted in etcd to protect your data. Your apps can use the credentials in the secret to access your {{site.data.keyword.cloud_notm}} service instance.

Services that do not support service keys usually provide an API that you can directly use in your app.

To find an overview of popular {{site.data.keyword.cloud_notm}} services, see [Popular integrations](/docs/containers?topic=containers-supported_integrations#popular_services).

### IBM Cloud infrastructure services
{: #infrastructure-services}

Because {{site.data.keyword.containerlong_notm}} lets you create a Kubernetes cluster that is based on {{site.data.keyword.cloud_notm}} infrastructure, some infrastructure services, such as Virtual Servers, Bare Metal Servers, or VLANs are fully integrated into {{site.data.keyword.containerlong_notm}}. You create and work with these services instances by using the {{site.data.keyword.containerlong_notm}} API, CLI, or console.
{: shortdesc}

Supported persistent storage solutions, such as {{site.data.keyword.cloud_notm}} File Storage, {{site.data.keyword.cloud_notm}} Block Storage, or {{site.data.keyword.cos_full}} are integrated as Kubernetes flex drivers and can be set up by using [Helm charts](/docs/containers?topic=containers-helm). The Helm chart automatically sets up Kubernetes storage classes, the storage provider, and the storage driver in your cluster. You can use the storage classes to provision persistent storage by using persistent volume claims (PVCs).

To secure your cluster network or connect to an on-prem data center, you can configure one of the following options:
- [strongSwan IPSec VPN Service](/docs/containers?topic=containers-vpn#vpn-setup)
- [{{site.data.keyword.BluDirectLink}}](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link)
- [Virtual Router Appliance (VRA)](/docs/containers?topic=containers-vpn#vyatta)
- [Fortigate Security Appliance (FSA)](/docs/services/vmwaresolutions/services?topic=vmware-solutions-fsa_considerations)

## Kubernetes community and open source integrations
{: #kube-community-tools}

Because you own the standard clusters that you create in {{site.data.keyword.containerlong_notm}}, you can choose to install third-party solutions to add extra capabilities to your cluster.
{: shortdesc}

Some open source technologies, such as Knative, Istio, LogDNA, Sysdig, or Portworx are tested by IBM and provided as managed add-ons, Helm charts, or {{site.data.keyword.cloud_notm}} services that are operated by the service provider in partnership with IBM. These open source tools are fully integrated into the {{site.data.keyword.cloud_notm}} billing and support system.

You can install other open source tools in your cluster, but these tools might not be managed, supported, or verified to work in {{site.data.keyword.containerlong_notm}}.

### Integrations operated in partnership
{: #open-source-partners}

For more information about {{site.data.keyword.containerlong_notm}} partners and the benefit of each solution that they provide, see [{{site.data.keyword.containerlong_notm}} partners](/docs/containers?topic=containers-service-partners).

### Managed add-ons
{: #cluster-add-ons}
{{site.data.keyword.containerlong_notm}} integrates popular open source integrations, such as [Knative](/docs/containers?topic=containers-serverless-apps-knative) or [Istio](/docs/containers?topic=containers-istio) by using [managed add-ons](/docs/containers?topic=containers-managed-addons). Managed add-ons are an easy way to install an open source tool in your cluster that is tested by IBM and approved to be used in {{site.data.keyword.containerlong_notm}}.

Managed add-ons are fully integrated into the {{site.data.keyword.cloud_notm}} support organization. If you have a question or an issue with using the managed add-ons, you can use one of the I{{site.data.keyword.containerlong_notm}} support channels. For more information, see [Getting help and support](/docs/containers?topic=containers-cs_troubleshoot_clusters#clusters_getting_help).

If the tool that you add to your cluster incurs costs, these costs are automatically integrated and listed as part of your monthly {{site.data.keyword.cloud_notm}} billing. The billing cycle is determined by {{site.data.keyword.cloud_notm}} depending on when you enabled the add-on in your cluster.

### Other third-party integrations
{: #kube-community-helm}

You can install any third-party open source tool that integrates with Kubernetes. For example, the Kubernetes community designates certain Helm charts `stable` or `incubator`. Note that these charts or tools are not verified to work in {{site.data.keyword.containerlong_notm}}. If the tool requires a license, you must purchase a license before you use the tool. For an overview of available Helm charts from the Kubernetes community, see the `kubernetes` and `kubernetes-incubator` repositories in the [Helm charts ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/kubernetes/solutions/helm-charts) catalog.
{: shortdesc}

Any costs that incur by using a third-party open source integration are not included in your monthly {{site.data.keyword.cloud_notm}} bill.

Installing third-party open source integrations or Helm charts from the Kubernetes community might change the default cluster configuration and can bring your cluster into an unsupported state. If you run into an issue with using any of these tools, consult the Kubernetes community or the service provider directly.
{: important}
