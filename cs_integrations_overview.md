---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-20"

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


# Supported IBM Cloud and third-party integrations
{: #supported_integrations}

You can use various external services and catalog services with a standard Kubernetes cluster in {{site.data.keyword.containerlong}}.
{:shortdesc}

## Popular integrations
{: #popular_services}

<table summary="The table shows available services that you can add to your cluster and that are popular among {{site.data.keyword.containerlong_notm}} users. Rows are to be read from the left to right, with the name of the service in column one, and a description of the service in column two.">
<caption>Popular services</caption>
<thead>
<tr>
<th>Service</th>
<th>Category</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.cloudaccesstrailfull}}</td>
<td>Cluster activity logs</td>
<td>Monitor the administrative activity that is made in your cluster by analyzing logs through Grafana. For more information about the service, see the [Activity Tracker](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started) documentation. For more information about the types of events that you can track, see [Activity Tracker events](/docs/containers?topic=containers-at_events).</td>
</tr>
<tr>
<td>{{site.data.keyword.appid_full}}</td>
<td>Authentication</td>
<td>Add a level of security to your apps with [{{site.data.keyword.appid_short}}](/docs/services/appid?topic=appid-getting-started) by requiring users to sign in. To authenticate web or API HTTP/HTTPS requests to your app, you can integrate {{site.data.keyword.appid_short_notm}} with your Ingress service by using the [{{site.data.keyword.appid_short_notm}} authentication Ingress annotation](/docs/containers?topic=containers-ingress_annotation#appid-auth).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix}} Block Storage</td>
<td>Block storage</td>
<td>[{{site.data.keyword.cloud_notm}} Block Storage](/docs/infrastructure/BlockStorage?topic=BlockStorage-getting-started#getting-started) is persistent, high-performance iSCSI storage that you can add to your apps by using Kubernetes persistent volumes (PVs). Use block storage to deploy stateful apps in a single zone or as high-performance storage for single pods. For more information about how to provision block storage in your cluster, see [Storing data on {{site.data.keyword.cloud_notm}} Block Storage](/docs/containers?topic=containers-block_storage#block_storage)</td>
</tr>
<tr>
<td>{{site.data.keyword.cloudcerts_full}}</td>
<td>TLS certificates</td>
<td>You can use <a href="/docs/services/certificate-manager?topic=certificate-manager-getting-started#getting-started" target="_blank">{{site.data.keyword.cloudcerts_long}} <img src="../icons/launch-glyph.svg" alt="External link icon"></a> to store and manage SSL certificates for your apps. For more information, see <a href="https://www.ibm.com/blogs/bluemix/2018/01/use-ibm-cloud-certificate-manager-ibm-cloud-container-service-deploy-custom-domain-tls-certificates/" target="_blank">Use {{site.data.keyword.cloudcerts_long_notm}} with {{site.data.keyword.containerlong_notm}} to deploy custom domain TLS Certificates <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. </td>
</tr>
<tr>
<td>{{site.data.keyword.registrylong}}</td>
<td>Container images</td>
<td>Set up your own secured Docker image repository where you can safely store and share images between cluster users. For more information, see the <a href="/docs/services/Registry?topic=registry-getting-started" target="_blank">{{site.data.keyword.registrylong}} documentation <img src="../icons/launch-glyph.svg" alt="External link icon"></a>.</td>
</tr>
<tr>
<td>{{site.data.keyword.contdelivery_full}}</td>
<td>Build automation</td>
<td>Automate your app builds and container deployments to Kubernetes clusters by using a toolchain. For more information about the setup, see the blog <a href="https://developer.ibm.com/recipes/tutorials/deploy-kubernetes-pods-to-the-bluemix-container-service-using-devops-pipelines/" target="_blank">Deploy Kubernetes pods to the {{site.data.keyword.containerlong_notm}} using DevOps Pipelines <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. </td>
</tr>
<tr>
<td>{{site.data.keyword.datashield_full}} (Beta)</td>
<td>Memory encryption</td>
<td>You can use <a href="/docs/services/data-shield?topic=data-shield-getting-started#getting-started" target="_blank">{{site.data.keyword.datashield_short}} <img src="../icons/launch-glyph.svg" alt="External link icon"></a> to encrypt your data memory. {{site.data.keyword.datashield_short}} is integrated with Intel® Software Guard Extensions (SGX) and Fortanix® technology so that your {{site.data.keyword.cloud_notm}} container workload code and data are protected in use. The app code and data run in CPU-hardened enclaves, which are trusted areas of memory on the worker node that protect critical aspects of the app, which helps to keep the code and data confidential and unmodified.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix}} File Storage</td>
<td>File storage</td>
<td>[{{site.data.keyword.cloud_notm}} File Storage](/docs/infrastructure/FileStorage?topic=FileStorage-getting-started#getting-started) is persistent, fast, and flexible network-attached, NFS-based file storage that you can add to your apps by using Kubernetes persistent volumes. You can choose between predefined storage tiers with GB sizes and IOPS that meet the requirements of your workloads. For more information about how to provision file storage in your cluster, see [Storing data on {{site.data.keyword.cloud_notm}} File Storage](/docs/containers?topic=containers-file_storage#file_storage).</td>
</tr>
<tr>
<td>{{site.data.keyword.keymanagementservicefull}}</td>
<td>Data encryption</td>
<td>Encrypt the Kubernetes secrets that are in your cluster by enabling {{site.data.keyword.keymanagementserviceshort}}. Encrypting your Kubernetes secrets prevents unauthorized users from accessing sensitive cluster information.<br>To set up, see <a href="/docs/containers?topic=containers-encryption#keyprotect">Encrypting Kubernetes secrets by using {{site.data.keyword.keymanagementserviceshort}}</a>.<br>For more information, see the <a href="/docs/services/key-protect?topic=key-protect-getting-started-tutorial" target="_blank">{{site.data.keyword.keymanagementserviceshort}} documentation <img src="../icons/launch-glyph.svg" alt="External link icon"></a>.</td>
</tr>
<tr>
<td>{{site.data.keyword.la_full}}</td>
<td>Cluster and app logs</td>
<td>Add log management capabilities to your cluster by deploying LogDNA as a third-party service to your worker nodes to manage logs from your pod containers. For more information, see [Managing Kubernetes cluster logs with {{site.data.keyword.loganalysisfull_notm}} with LogDNA](/docs/services/Log-Analysis-with-LogDNA/tutorials?topic=LogDNA-kube#kube).</td>
</tr>
<tr>
<td>{{site.data.keyword.mon_full}}</td>
<td>Cluster and app metrics</td>
<td>Gain operational visibility into the performance and health of your apps by deploying Sysdig as a third-party service to your worker nodes to forward metrics to {{site.data.keyword.monitoringlong}}. For more information, see [Analyzing metrics for an app that is deployed in a Kubernetes cluster](/docs/services/Monitoring-with-Sysdig/tutorials?topic=Sysdig-kubernetes_cluster#kubernetes_cluster). </td>
</tr>
<tr>
<td>{{site.data.keyword.cos_full}}</td>
<td>Object storage</td>
<td>Data that is stored with {{site.data.keyword.cos_short}} is encrypted and dispersed across multiple geographic locations, and accessed over HTTP by using a REST API. You can use the [ibm-backup-restore image](/docs/services/RegistryImages/ibm-backup-restore?topic=RegistryImages-ibmbackup_restore_starter) to configure the service to make one-time or scheduled backups for data in your clusters. For more information about the service, see the <a href="/docs/services/cloud-object-storage?topic=cloud-object-storage-about-ibm-cloud-object-storage" target="_blank">{{site.data.keyword.cos_short}} documentation <img src="../icons/launch-glyph.svg" alt="External link icon"></a>.</td>
</tr>
<tr>
<td>Istio on {{site.data.keyword.containerlong_notm}}</td>
<td>Microservice management</td>
<td><a href="https://www.ibm.com/cloud/info/istio" target="_blank">Istio <img src="../icons/launch-glyph.svg" alt="External link icon"></a> is an open source service that gives developers a way to connect, secure, manage, and monitor a network of microservices, also known a service mesh, on cloud orchestration platforms. Istio on {{site.data.keyword.containerlong}} provides a one-step installation of Istio into your cluster through a managed add-on. With one click, you can get all Istio core components, additional tracing, monitoring, and visualization, and the BookInfo sample app up and running. To get started, see [Using the managed Istio add-on (beta)](/docs/containers?topic=containers-istio).</td>
</tr>
<tr>
<td>Knative</td>
<td>Serverless apps</td>
<td>[Knative ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/knative/docs) is an open source platform that was developed by IBM, Google, Pivotal, Red Hat, Cisco, and others with the goal of extending the capabilities of Kubernetes to help you create modern, source-centric containerized, and serverless apps on top of your Kubernetes cluster. The platform uses a consistent approach across programming languages and frameworks to abstract the operational burden of building, deploying, and managing workloads in Kubernetes so that developers can focus on what matters most to them: the source code. For more information, see [Deploying serverless apps with Knative](/docs/containers?topic=containers-serverless-apps-knative). </td>
</tr>
<tr>
<td>Portworx</td>
<td>Storage for stateful apps</td>
<td>[Portworx ![External link icon](../icons/launch-glyph.svg "External link icon")](https://portworx.com/products/introduction/) is a highly available software-defined storage solution that you can use to manage persistent storage for your containerized databases and other stateful apps, or to share data between pods across multiple zones. You can install Portworx with a Helm chart and provision storage for your apps by using Kubernetes persistent volumes. For more information about how to set up Portworx in your cluster, see [Storing data on software-defined storage (SDS) with Portworx](/docs/containers?topic=containers-portworx#portworx).</td>
</tr>
<tr>
<td>Razee</td>
<td>Deployment automation</td>
<td>[Razee ![External link icon](../icons/launch-glyph.svg "External link icon")](https://razee.io/) is an open-source project that automates and manages the deployment of Kubernetes resources across clusters, environments, and cloud providers, and helps you to visualize deployment information for your resources so that you can monitor the rollout process and find deployment issues more quickly. For more information about Razee and how to set up Razee in your cluster to automate your deployment process, see the [Razee documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/razee-io/Razee).</td>
</tr>
</tbody>
</table>

<br />


## DevOps services
{: #devops_services}

<table summary="The table shows available services that you can add to your cluster to add extra DevOps capabilities. Rows are to be read from the left to right, with the name of the service in column one, and a description of the service in column two.">
<caption>DevOps services</caption>
<thead>
<tr>
<th>Service</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.cfee_full_notm}}</td>
<td>Deploy and manage your own Cloud Foundry platform on top of a Kubernetes cluster to develop, package, deploy, and manage cloud-native apps, and leverage the {{site.data.keyword.cloud_notm}} ecosystem to bind additional services to your apps. When you create an {{site.data.keyword.cfee_full_notm}} instance, you must configure your Kubernetes cluster by choosing the machine type and VLANs for your worker nodes. Your cluster is then provisioned with {{site.data.keyword.containerlong_notm}} and {{site.data.keyword.cfee_full_notm}} is automatically deployed to your cluster. For more information about how to set up {{site.data.keyword.cfee_full_notm}}, see the [Getting started tutorial](/docs/cloud-foundry?topic=cloud-foundry-getting-started#getting-started). </td>
</tr>
<tr>
<td>Codeship</td>
<td>You can use <a href="https://codeship.com" target="_blank">Codeship <img src="../icons/launch-glyph.svg" alt="External link icon"></a> for the continuous integration and delivery of containers. For more information, see <a href="https://www.ibm.com/blogs/bluemix/2017/10/using-codeship-pro-deploy-workloads-ibm-container-service/" target="_blank">Using Codeship Pro To Deploy Workloads to {{site.data.keyword.containerlong_notm}} <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. </td>
</tr>
<tr>
<td>Grafeas</td>
<td>[Grafeas ![External link icon](../icons/launch-glyph.svg "External link icon")](https://grafeas.io) is an open source CI/CD service that provides a common way for how to retrieve, store, and exchange metadata during the software supply chain process. For example, if you integrate Grafeas into your app build process, Grafeas can store information about the initiator of the build request, vulnerability scan results, and quality assurance sign-off so that you can make an informed decision if an app can be deployed to production. You can use this metadata in audits or to prove compliance for your software supply chain. </td>
</tr>
<tr>
<td>Helm</td>
<td> <a href="https://helm.sh" target="_blank">Helm <img src="../icons/launch-glyph.svg" alt="External link icon"></a> is a Kubernetes package manager. You can create new Helm charts or use preexisting Helm charts to define, install, and upgrade complex Kubernetes applications that run in {{site.data.keyword.containerlong_notm}} clusters. <p>For more information, see [Setting up Helm in {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-helm).</p></td>
</tr>
<tr>
<td>{{site.data.keyword.contdelivery_full}}</td>
<td>Automate your app builds and container deployments to Kubernetes clusters by using a toolchain. For more information about the setup, see the blog <a href="https://developer.ibm.com/recipes/tutorials/deploy-kubernetes-pods-to-the-bluemix-container-service-using-devops-pipelines/" target="_blank">Deploy Kubernetes pods to the {{site.data.keyword.containerlong_notm}} using DevOps Pipelines <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. </td>
</tr>
<tr>
<td>Istio on {{site.data.keyword.containerlong_notm}}</td>
<td><a href="https://www.ibm.com/cloud/info/istio" target="_blank">Istio <img src="../icons/launch-glyph.svg" alt="External link icon"></a> is an open source service that gives developers a way to connect, secure, manage, and monitor a network of microservices, also known a service mesh, on cloud orchestration platforms. Istio on {{site.data.keyword.containerlong}} provides a one-step installation of Istio into your cluster through a managed add-on. With one click, you can get all Istio core components, additional tracing, monitoring, and visualization, and the BookInfo sample app up and running. To get started, see [Using the managed Istio add-on (beta)](/docs/containers?topic=containers-istio).</td>
</tr>

<tr>
<td>Knative</td>
<td>[Knative ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/knative/docs) is an open source platform that was developed by IBM, Google, Pivotal, Red Hat, Cisco, and others with the goal of extending the capabilities of Kubernetes to help you create modern, source-centric containerized, and serverless apps on top of your Kubernetes cluster. The platform uses a consistent approach across programming languages and frameworks to abstract the operational burden of building, deploying, and managing workloads in Kubernetes so that developers can focus on what matters most to them: the source code. For more information, see [Deploying serverless apps with Knative](/docs/containers?topic=containers-serverless-apps-knative). </td>
</tr>
<tr>
<td>Razee</td>
<td>[Razee ![External link icon](../icons/launch-glyph.svg "External link icon")](https://razee.io/) is an open-source project that automates and manages the deployment of Kubernetes resources across clusters, environments, and cloud providers, and helps you to visualize deployment information for your resources so that you can monitor the rollout process and find deployment issues more quickly. For more information about Razee and how to set up Razee in your cluster to automate your deployment process, see the [Razee documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/razee-io/Razee).</td>
</tr>
</tbody>
</table>

<br />


## Hybrid cloud services
{: #hybrid_cloud_services}

<table summary="The table shows available services that you can use to connect your cluster to on-premises data centers. Rows are to be read from the left to right, with the name of the service in column one, and a description of the service in column two.">
<caption>Hybrid cloud services</caption>
<thead>
<tr>
<th>Service</th>
<th>Description</th>
</tr>
</thead>
<tbody>
  <tr>
    <td>{{site.data.keyword.BluDirectLink}}</td>
    <td>With [{{site.data.keyword.cloud_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-about-ibm-cloud-direct-link), you can create a direct, private connection between your remote network environments and {{site.data.keyword.containerlong_notm}} without routing over the public internet. The {{site.data.keyword.cloud_notm}} Direct Link offerings are useful when you must implement hybrid workloads, cross-provider workloads, large or frequent data transfers, or private workloads. To choose an {{site.data.keyword.cloud_notm}} Direct Link offering and set up an {{site.data.keyword.cloud_notm}} Direct Link connection, see [Get Started with {{site.data.keyword.cloud_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link#how-do-i-know-which-type-of-ibm-cloud-direct-link-i-need-) in the {{site.data.keyword.cloud_notm}} Direct Link documentation.</td>
  </tr>
<tr>
  <td>strongSwan IPSec VPN Service</td>
  <td>Set up a [strongSwan IPSec VPN service ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.strongswan.org/about.html) that securely connects your Kubernetes cluster with an on-premises network. The strongSwan IPSec VPN service provides a secure end-to-end communication channel over the internet that is based on the industry-standard Internet Protocol Security (IPSec) protocol suite. To set up a secure connection between your cluster and an on-premises network, [configure and deploy the strongSwan IPSec VPN service](/docs/containers?topic=containers-vpn#vpn-setup) directly in a pod in your cluster.</td>
  </tr>
  </tbody>
  </table>

<br />


## Logging and monitoring services
{: #health_services}
<table summary="The table shows available services that you can add to your cluster to add extra logging and monitoring capabilities. Rows are to be read from the left to right, with the name of the service in column one, and a description of the service in column two.">
<caption>Logging and monitoring services</caption>
<thead>
<tr>
<th>Service</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>CoScale</td>
<td>Monitor worker nodes, containers, replica sets, replication controllers, and services with <a href="https://www.newrelic.com/coscale" target="_blank">CoScale <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. For more information, see <a href="https://www.ibm.com/blogs/bluemix/2017/06/monitoring-ibm-bluemix-container-service-coscale/" target="_blank">Monitoring {{site.data.keyword.containerlong_notm}} with CoScale <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. </td>
</tr>
<tr>
<td>Datadog</td>
<td>Monitor your cluster and view infrastructure and application performance metrics with <a href="https://www.datadoghq.com/" target="_blank">Datadog <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. For more information, see <a href="https://www.ibm.com/blogs/bluemix/2017/07/monitoring-ibm-bluemix-container-service-datadog/" target="_blank">Monitoring {{site.data.keyword.containerlong_notm}} with Datadog <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. </td>
</tr>
<tr>
<td>{{site.data.keyword.cloudaccesstrailfull}}</td>
<td>Monitor the administrative activity that is made in your cluster by analyzing logs through Grafana. For more information about the service, see the [Activity Tracker](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started) documentation. For more information about the types of events that you can track, see [Activity Tracker events](/docs/containers?topic=containers-at_events).</td>
</tr>
<tr>
<td>{{site.data.keyword.la_full_notm}}</td>
<td>Add log management capabilities to your cluster by deploying LogDNA as a third-party service to your worker nodes to manage logs from your pod containers. For more information, see [Managing Kubernetes cluster logs with {{site.data.keyword.loganalysisfull_notm}} with LogDNA](/docs/services/Log-Analysis-with-LogDNA/tutorials?topic=LogDNA-kube#kube).</td>
</tr>
<tr>
<td>{{site.data.keyword.mon_full_notm}}</td>
<td>Gain operational visibility into the performance and health of your apps by deploying Sysdig as a third-party service to your worker nodes to forward metrics to {{site.data.keyword.monitoringlong}}. For more information, see [Analyzing metrics for an app that is deployed in a Kubernetes cluster](/docs/services/Monitoring-with-Sysdig/tutorials?topic=Sysdig-kubernetes_cluster#kubernetes_cluster). </td>
</tr>
<tr>
<td>Instana</td>
<td> <a href="https://www.instana.com/" target="_blank">Instana <img src="../icons/launch-glyph.svg" alt="External link icon"></a> provides infrastructure and app performance monitoring with a GUI that automatically discovers and maps your apps. Instana captures every request to your apps, which you can use to troubleshoot and perform root cause analysis to prevent the problems from happening again. Check out the blog post about <a href="https://www.instana.com/blog/precise-visibility-applications-ibm-bluemix-container-service/" target="_blank">deploying Instana in {{site.data.keyword.containerlong_notm}} <img src="../icons/launch-glyph.svg" alt="External link icon"></a> to learn more.</td>
</tr>
<tr>
<td>Prometheus</td>
<td>Prometheus is an open source monitoring, logging, and alerting tool that was designed for Kubernetes. Prometheus retrieves detailed information about the cluster, worker nodes, and deployment health based on Kubernetes logging information. CPU, memory, I/O, and network activity is collected for each container that runs in a cluster. You can use the collected data in custom queries or alerts to monitor performance and workloads in your cluster.

<p>To use Prometheus, follow the <a href="https://github.com/coreos/prometheus-operator/tree/master/contrib/kube-prometheus" target="_blank">CoreOS instructions <img src="../icons/launch-glyph.svg" alt="External link icon"></a>.</p>
</td>
</tr>
<tr>
<td>Sematext</td>
<td>View metrics and logs for your containerized applications by using <a href="https://sematext.com/" target="_blank">Sematext <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. For more information, see <a href="https://www.ibm.com/blogs/bluemix/2017/09/monitoring-logging-ibm-bluemix-container-service-sematext/" target="_blank">Monitoring and logging for containers with Sematext <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. </td>
</tr>
<tr>
<td>Splunk</td>
<td>Import and search your Kubernetes logging, object, and metrics data in Splunk by using Splunk Connect for Kubernetes. Splunk Connect for Kubernetes is a collection of Helm charts that deploy a Splunk-supported deployment of Fluentd to your Kubernetes cluster, a Splunk-built Fluentd HTTP Event Collector (HEC) plug-in to send logs and metadata, and a metrics deployment that captures your cluster metrics. For more information, see <a href="https://www.ibm.com/blogs/bluemix/2019/02/solving-business-problems-with-splunk-on-ibm-cloud-kubernetes-service/" target="_blank">Solving Business Problems with Splunk on {{site.data.keyword.containerlong_notm}} <img src="../icons/launch-glyph.svg" alt="External link icon"></a>.</td>
</tr>
<tr>
<td>Weave Scope</td>
<td>[Weave Scope ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.weave.works/oss/scope/) provides a visual diagram of your resources within a Kubernetes cluster, including services, pods, containers, processes, nodes, and more. Weave Scope provides interactive metrics for CPU and memory and also provides tools to tail and exec into a container.</li></ol>
</td>
</tr>
</tbody>
</table>

<br />



## Security services
{: #security_services}

Want a comprehensive view of how to integrate {{site.data.keyword.cloud_notm}} security services with your cluster? Check out the [Apply end-to-end security to a cloud application tutorial](/docs/tutorials?topic=solution-tutorials-cloud-e2e-security).
{: shortdesc}

<table summary="The table shows available services that you can add to your cluster to add extra security capabilities. Rows are to be read from the left to right, with the name of the service in column one, and a description of the service in column two.">
<caption>Security services</caption>
<thead>
<tr>
<th>Service</th>
<th>Description</th>
</tr>
</thead>
<tbody>
  <tr>
    <td>{{site.data.keyword.appid_full}}</td>
    <td>Add a level of security to your apps with [{{site.data.keyword.appid_short}}](/docs/services/appid?topic=appid-getting-started) by requiring users to sign in. To authenticate web or API HTTP/HTTPS requests to your app, you can integrate {{site.data.keyword.appid_short_notm}} with your Ingress service by using the [{{site.data.keyword.appid_short_notm}} authentication Ingress annotation](/docs/containers?topic=containers-ingress_annotation#appid-auth).</td>
  </tr>
<tr>
<td>Aqua Security</td>
  <td>As a supplement to <a href="/docs/services/va?topic=va-va_index" target="_blank">Vulnerability Advisor</a>, you can use <a href="https://www.aquasec.com/" target="_blank">Aqua Security <img src="../icons/launch-glyph.svg" alt="External link icon"></a> to improve the security of container deployments by reducing what your app is allowed to do. For more information, see <a href="https://blog.aquasec.com/securing-container-deployments-on-bluemix-with-aqua-security" target="_blank">Securing container deployments on {{site.data.keyword.cloud_notm}} with Aqua Security <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. </td>
</tr>
<tr>
<td>{{site.data.keyword.cloudcerts_full}}</td>
<td>You can use <a href="/docs/services/certificate-manager?topic=certificate-manager-getting-started#getting-started" target="_blank">{{site.data.keyword.cloudcerts_long}} <img src="../icons/launch-glyph.svg" alt="External link icon"></a> to store and manage SSL certificates for your apps. For more information, see <a href="https://www.ibm.com/blogs/bluemix/2018/01/use-ibm-cloud-certificate-manager-ibm-cloud-container-service-deploy-custom-domain-tls-certificates/" target="_blank">Use {{site.data.keyword.cloudcerts_long_notm}} with {{site.data.keyword.containerlong_notm}} to deploy custom domain TLS Certificates <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. </td>
</tr>
<tr>
  <td>{{site.data.keyword.datashield_full}} (Beta)</td>
  <td>You can use <a href="/docs/services/data-shield?topic=data-shield-getting-started#getting-started" target="_blank">{{site.data.keyword.datashield_short}} <img src="../icons/launch-glyph.svg" alt="External link icon"></a> to encrypt your data memory. {{site.data.keyword.datashield_short}} is integrated with Intel® Software Guard Extensions (SGX) and Fortanix® technology so that your {{site.data.keyword.cloud_notm}} container workload code and data are protected in use. The app code and data run in CPU-hardened enclaves, which are trusted areas of memory on the worker node that protect critical aspects of the app, which helps to keep the code and data confidential and unmodified.</td>
</tr>
<tr>
  <td>{{site.data.keyword.registrylong}}</td>
  <td>Set up your own secured Docker image repository where you can safely store and share images between cluster users. For more information, see the <a href="/docs/services/Registry?topic=registry-getting-started" target="_blank">{{site.data.keyword.registrylong}} documentation <img src="../icons/launch-glyph.svg" alt="External link icon"></a>.</td>
</tr>
<tr>
  <td>{{site.data.keyword.keymanagementservicefull}}</td>
  <td>Encrypt the Kubernetes secrets that are in your cluster by enabling {{site.data.keyword.keymanagementserviceshort}}. Encrypting your Kubernetes secrets prevents unauthorized users from accessing sensitive cluster information.<br>To set up, see <a href="/docs/containers?topic=containers-encryption#keyprotect">Encrypting Kubernetes secrets by using {{site.data.keyword.keymanagementserviceshort}}</a>.<br>For more information, see the <a href="/docs/services/key-protect?topic=key-protect-getting-started-tutorial" target="_blank">{{site.data.keyword.keymanagementserviceshort}} documentation <img src="../icons/launch-glyph.svg" alt="External link icon"></a>.</td>
</tr>
<tr>
<td>NeuVector</td>
<td>Protect containers with a cloud-native firewall by using <a href="https://neuvector.com/" target="_blank">NeuVector <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. For more information, see <a href="https://www.ibm.com/us-en/marketplace/neuvector-container-security" target="_blank">NeuVector Container Security <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. </td>
</tr>
<tr>
<td>Twistlock</td>
<td>As a supplement to <a href="/docs/services/va?topic=va-va_index" target="_blank">Vulnerability Advisor</a>, you can use <a href="https://www.twistlock.com/" target="_blank">Twistlock <img src="../icons/launch-glyph.svg" alt="External link icon"></a> to manage firewalls, threat protection, and incident response. For more information, see <a href="https://www.ibm.com/blogs/bluemix/2017/07/twistlock-ibm-bluemix-container-service/" target="_blank">Twistlock on {{site.data.keyword.containerlong_notm}} <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. </td>
</tr>
</tbody>
</table>

<br />



## Storage services
{: #storage_services}
<table summary="The table shows available services that you can add to your cluster to add persistent storage capabilities. Rows are to be read from the left to right, with the name of the service in column one, and a description of the service in column two.">
<caption>Storage services</caption>
<thead>
<tr>
<th>Service</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>Heptio Velero</td>
  <td>You can use <a href="https://github.com/heptio/velero" target="_blank">Heptio Velero <img src="../icons/launch-glyph.svg" alt="External link icon"></a> to back up and restore cluster resources and persistent volumes. For more information, see the Heptio Velero <a href="https://github.com/heptio/velero/blob/release-0.9/docs/use-cases.md" target="_blank">Use cases for disaster recovery and cluster migration <img src="../icons/launch-glyph.svg" alt="External link icon"></a>.</td>
</tr>
<tr>
  <td>{{site.data.keyword.cloud_notm}} Block Storage</td>
  <td>[{{site.data.keyword.cloud_notm}} Block Storage](/docs/infrastructure/BlockStorage?topic=BlockStorage-getting-started#getting-started) is persistent, high-performance iSCSI storage that you can add to your apps by using Kubernetes persistent volumes (PVs). Use block storage to deploy stateful apps in a single zone or as high-performance storage for single pods. For more information about how to provision block storage in your cluster, see [Storing data on {{site.data.keyword.cloud_notm}} Block Storage](/docs/containers?topic=containers-block_storage#block_storage)</td>
  </tr>
<tr>
  <td>{{site.data.keyword.cos_full}}</td>
  <td>Data that is stored with {{site.data.keyword.cos_short}} is encrypted and dispersed across multiple geographic locations, and accessed over HTTP by using a REST API. You can use the [ibm-backup-restore image](/docs/services/RegistryImages/ibm-backup-restore?topic=RegistryImages-ibmbackup_restore_starter) to configure the service to make one-time or scheduled backups for data in your clusters. For more information about the service, see the <a href="/docs/services/cloud-object-storage?topic=cloud-object-storage-about-ibm-cloud-object-storage" target="_blank">{{site.data.keyword.cos_short}} documentation <img src="../icons/launch-glyph.svg" alt="External link icon"></a>.</td>
</tr>
  <tr>
  <td>{{site.data.keyword.cloud_notm}} File Storage</td>
  <td>[{{site.data.keyword.cloud_notm}} File Storage](/docs/infrastructure/FileStorage?topic=FileStorage-getting-started#getting-started) is persistent, fast, and flexible network-attached, NFS-based file storage that you can add to your apps by using Kubernetes persistent volumes. You can choose between predefined storage tiers with GB sizes and IOPS that meet the requirements of your workloads. For more information about how to provision file storage in your cluster, see [Storing data on {{site.data.keyword.cloud_notm}} File Storage](/docs/containers?topic=containers-file_storage#file_storage).</td>
  </tr>
  <tr>
    <td>Portworx</td>
    <td>[Portworx ![External link icon](../icons/launch-glyph.svg "External link icon")](https://portworx.com/products/introduction/) is a highly available software-defined storage solution that you can use to manage persistent storage for your containerized databases and other stateful apps, or to share data between pods across multiple zones. You can install Portworx with a Helm chart and provision storage for your apps by using Kubernetes persistent volumes. For more information about how to set up Portworx in your cluster, see [Storing data on software-defined storage (SDS) with Portworx](/docs/containers?topic=containers-portworx#portworx).</td>
  </tr>
</tbody>
</table>

<br />


## Database services
{: #database_services}

<table summary="The table shows available services that you can add to your cluster to add database capabilities. Rows are to be read from the left to right, with the name of the service in column one, and a description of the service in column two.">
<caption>Database services</caption>
<thead>
<tr>
<th>Service</th>
<th>Description</th>
</tr>
</thead>
<tbody>
  <tr>
    <td>{{site.data.keyword.blockchainfull_notm}} Platform 2.0 beta</td>
    <td>Deploy and manage your own {{site.data.keyword.blockchainfull_notm}} Platform on {{site.data.keyword.containerlong_notm}}. With {{site.data.keyword.blockchainfull_notm}} Platform 2.0, you can host {{site.data.keyword.blockchainfull_notm}} networks or create organizations that can join other {{site.data.keyword.blockchainfull_notm}} 2.0 networks. For more information about how to set up {{site.data.keyword.blockchainfull_notm}} in {{site.data.keyword.containerlong_notm}}, see [About {{site.data.keyword.blockchainfull_notm}} Platform free 2.0 beta](/docs/services/blockchain?topic=blockchain-ibp-console-overview#ibp-console-overview).</td>
  </tr>
<tr>
  <td>Cloud databases</td>
  <td>You can choose between various {{site.data.keyword.cloud_notm}} database services, such as {{site.data.keyword.composeForMongoDB_full}} or {{site.data.keyword.cloudantfull}} to deploy highly available and scalable database solutions in your cluster. For a list of available cloud databases, see the [{{site.data.keyword.cloud_notm}} catalog ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/catalog?category=databases).  </td>
  </tr>
  </tbody>
  </table>
