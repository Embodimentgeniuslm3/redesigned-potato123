---

copyright:
  years: 2014, 2019
lastupdated: "2019-07-19"

keywords: kubernetes, iks, containers

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


# Defining your Kubernetes strategy
{: #strategy}

With {{site.data.keyword.containerlong}}, you can quickly and securely deploy container workloads for your apps in production. Learn more so that when you plan your cluster strategy, you optimize your setup to make the most of [Kubernetes![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/) automated deploying, scaling, and orchestration management capabilities.
{:shortdesc}

## Moving your workloads to the {{site.data.keyword.cloud_notm}}
{: #cloud_workloads}

You have lots of reasons to move your workloads to {{site.data.keyword.cloud_notm}}: reducing total cost of ownership, increasing high availability for your apps in a secure and compliant environment, scaling up and down in respond to your user demand, and many more. {{site.data.keyword.containerlong_notm}} combines container technology with open source tools, such as Kubernetes so that you can build a cloud-native app that can migrate across different cloud environments, avoiding vendor lock-in.
{:shortdesc}

But how do you get to the cloud? What are your options along the way? And how do you manage your workloads after you get there?

Use this page to learn some strategies for your Kubernetes deployments on {{site.data.keyword.containerlong_notm}}. And always feel free to engage with our team on [Slack. ![External link icon](../icons/launch-glyph.svg "External link icon")](https://ibm-kubernetes-service.slack.com)

Not on slack yet? [Request an invite!](https://bxcs-slack-invite.mybluemix.net/)
{: tip}

### What can I move to the {{site.data.keyword.cloud_notm}}?
{: #move_to_cloud}

With {{site.data.keyword.cloud_notm}}, you have flexibility to create Kubernetes clusters in [off-premises, on-premises, or hybrid cloud environments](/docs/containers?topic=containers-cs_ov#differentiation). The following table provides some examples of what types of workloads that users typically move to the various types of clouds. You might also choose a hybrid approach where you have clusters that run in both environments.
{: shortdesc}

| Workload | {{site.data.keyword.containershort_notm}} off-prem | on-prem |
| --- | --- | --- |
| DevOps enablement tools | <img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" /> | |
| Developing and testing apps | <img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" /> | |
| Apps have major shifts in demand and need to scale rapidly | <img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" /> | |
| Business apps such as CRM, HCM, ERP, and E-commerce | <img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" /> | |
| Collaboration and social tools such as email | <img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" /> | |
| Linux and x86 workloads | <img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" /> | |
| Bare metal and GPU compute resources | <img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" /> | <img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" /> |
| PCI and HIPAA-compliant workloads | <img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" /> | <img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" /> |
| Legacy apps with platform and infrastructure constraints and dependencies | | <img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" /> |
| Proprietary apps with strict designs, licensing, or heavy regulations | | <img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" /> |
{: caption="{{site.data.keyword.cloud_notm}} implementations support your workloads" caption-side="top"}

**Ready to run workloads off-premises in {{site.data.keyword.containerlong_notm}}?**</br>
Great! You're already in our public cloud documentation. Keep reading for more strategy ideas, or hit the ground running by [creating a cluster now](/docs/containers?topic=containers-getting-started).

**Interested in an on-premises cloud?**</br>
Explore the [{{site.data.keyword.cloud_notm}} Private documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.1/kc_welcome_containers.html). If you already have significant investments in IBM technology such as WebSphere Application Server and Liberty, you can optimize your {{site.data.keyword.cloud_notm}} Private modernization strategy with various tools.

**Want to run workloads in both on-premises and off-premises clouds?**</br>
Start with setting up an {{site.data.keyword.cloud_notm}} Private account. Then, see [Using {{site.data.keyword.containerlong_notm}} with {{site.data.keyword.cloud_notm}} Private](/docs/containers?topic=containers-hybrid_iks_icp) to connect your {{site.data.keyword.cloud_notm}} Private environment with a cluster in {{site.data.keyword.cloud_notm}} Public. To manage multiple cloud Kubernetes clusters such as across {{site.data.keyword.cloud_notm}} Public and {{site.data.keyword.cloud_notm}} Private, check out the [IBM Multicloud Manager ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.1.0/mcm/getting_started/introduction.html).

### What kind of apps can I run in {{site.data.keyword.containerlong_notm}}?
{: #app_types}

Your containerized app must be able to run on the supported operating system, Ubuntu 16.64, 18.64. You also want to consider the statefulness of your app.
{: shortdesc}

<dl>
<dt>Stateless apps</dt>
  <dd><p>Stateless apps are preferred for cloud-native environments like Kubernetes. They are simple to migrate and scale because they declare dependencies, store configurations separately from the code, and treat backing services such as databases as attached resources instead of coupled to the app. The app pods do not require persistent data storage or a stable network IP address, and as such, pods can be terminated, rescheduled, and scaled in response to workload demands. The app uses a Database-as-a-Service for persistent data, and NodePort, load balancer, or Ingress services to expose the workload on a stable IP address.</p></dd>
<dt>Stateful apps</dt>
  <dd><p>Stateful apps are more complicated than stateless apps to set up, manage, and scale because the pods require persistent data and a stable network identity. Stateful apps are often databases or other distributed, data-intensive workloads where processing is more efficient closer to the data itself.</p>
  <p>If you want to deploy a stateful app, you need to set up persistent storage and mount a persistent volume to the pod that is controlled by a StatefulSet object. You can choose to add [file](/docs/containers?topic=containers-file_storage#file_statefulset), [block](/docs/containers?topic=containers-block_storage#block_statefulset), or [object](/docs/containers?topic=containers-object_storage#cos_statefulset) storage as the persistent storage for your stateful set. You can also install [Portworx](/docs/containers?topic=containers-portworx) on top of your bare metal worker nodes and use Portworx as a highly available software-defined storage solution to manage persistent storage for your stateful apps. For more information about how stateful sets work, see the [Kubernetes documentation![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/).</p></dd>
</dl>

### What are some guidelines for developing stateless, cloud-native apps?
{: #12factor}

Check out the [Twelve-Factor App ![External link icon](../icons/launch-glyph.svg "External link icon")](https://12factor.net/), a language-agnostic methodology for considering how to develop your app across 12 factors, summarized as follows.
{: shortdesc}

1.  **Codebase**: Use a single codebase in a version control system for your deployments. When you pull an image for your container deployment, specify a tested image tag instead of using `latest`.
2.  **Dependencies**: Explicitly declare and isolate external dependencies.
3.  **Configuration**: Store deployment-specific configuration in environment variables, not in the code.
4.  **Backing services**: Treat backing services, such as data stores or message queues, as attached or replaceable resources.
5.  **App stages**: Build in distinct stages such as `build`, `release`, `run`, with strict separate among them.
6.  **Processes**: Run as one or more stateless processes that share nothing and use [persistent storage](/docs/containers?topic=containers-storage_planning) for saving data.
7.  **Port binding**: Port bindings are self-contained and provide a service endpoint on well-defined host and port.
8.  **Concurrency**: Manage and scale your app through process instances such as replicas and horizontal scaling. Set resource requests and limits for your deployments. Note that Calico network policies cannot limit bandwidth. Instead, consider [Istio](/docs/containers?topic=containers-istio).
9.  **Disposability**: Design your app to be disposable, with minimal startup, graceful shutdown, and toleration for abrupt process terminations. Remember, containers, pods, and even worker nodes are meant to be disposable, so plan your app accordingly.
10.  **Dev-to-prod parity**: Set up a [continuous integration](https://www.ibm.com/cloud/garage/content/code/practice_continuous_integration/) and [continuous delivery](https://www.ibm.com/cloud/garage/content/deliver/practice_continuous_delivery/) pipeline for your app, with minimal difference between the app in development and the app in prod.
11.  **Logs**: Treat logs as event streams: the outer or hosting environment processes and routes log files. **Important**: In {{site.data.keyword.containerlong_notm}}, logs are not turned on by default. To enable, see [Configuring log forwarding](/docs/containers?topic=containers-health#configuring).
12.  **Admin processes**: Keep any one-time admin scripts with your app and run them as a [Kubernetes Job object ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/workloads/controllers/jobs-run-to-completion/) to ensure that the admin scripts run with the same environment as the app itself. For orchestration of larger packages that you want to run in your Kubernetes clusters, consider using a package manager such as [Helm ![External link icon](../icons/launch-glyph.svg "External link icon")](https://helm.sh/).

### I already have an app. How can I migrate it to {{site.data.keyword.containerlong_notm}}?
{: #migrate_containerize}

You can take some general steps to containerize your app as follows.
{: shortdesc}

1.  Use the [Twelve-Factor App ![External link icon](../icons/launch-glyph.svg "External link icon")](https://12factor.net/) as a guide for isolating dependencies, separating processes into separate services, and reducing the statefulness of your app as much as possible.
2.  Find an appropriate base image to use. You can use publicly available images from [Docker Hub ![External link icon](../icons/launch-glyph.svg "External link icon")](https://hub.docker.com/), [public IBM images](/docs/services/Registry?topic=registry-public_images#public_images), or build and manage your own in your private {{site.data.keyword.registryshort_notm}}.
3.  Add to your Docker image only what is necessary to run the app.
4.  Instead of relying on local storage, plan to use persistent storage or cloud database-as-a-service solutions to back up your app's data.
5.  Over time, refactor your app processes into microservices.

For more, see the following tutorials:
*  [Migrating an app from Cloud Foundry to a cluster](/docs/containers?topic=containers-cf_tutorial#cf_tutorial)
*  [Moving a VM-based app to Kubernetes](/docs/tutorials?topic=solution-tutorials-vm-to-containers-and-kubernetes)



Continue with the following topics for more considerations when you move workloads, such as Kubernetes environments, high availability, service discovery, and deployments.

<br />


### What knowledge and technical skills are good to have before I move my apps to {{site.data.keyword.containerlong_notm}}?
{: #knowledge}

Kubernetes is designed to provide capabilities to two main personas, the cluster admin and the app developer. Each persona uses different technical skills to successfully run and deploy apps to a cluster.
{: shortdesc}

**What are a cluster admin's main tasks and technical knowledge?** </br>
As a cluster admin, you are responsible to set up, operate, secure, and manage the {{site.data.keyword.cloud_notm}} infrastructure of your cluster. Typical tasks include:
- Size the cluster to provide enough capacity for your workloads.
- Design a cluster to meet the high availability, disaster recovery, and compliance standards of your company.
- Secure the cluster by setting up user permissions and limiting actions within the cluster to protect your compute resources, your network, and data.
- Plan and manage network communication between infrastructure components to ensure network security, segmentation, and compliance.
- Plan persistent storage options to meet data residency and data protection requirements.

The cluster admin persona must have a broad knowledge that includes compute, network, storage, security, and compliance. In a typical company, this knowledge is spread across multiple specialists, such as System Engineers, System Administrators, Network Engineers, Network Architects, IT Managers, or Security and Compliance Specialists. Consider assigning the cluster admin role to multiple people in your company so that you have the required knowledge to successfully operate your cluster.

**What are an app developer's main tasks and technical skills?** </br>
As a developer, you design, create, secure, deploy, test, run, and monitor cloud-native, containerized apps in a Kubernetes cluster. To create and run these apps, you must be familiar with the concept of microservices, the [12-factor app](#12factor) guidelines, [Docker and containerization principles](https://www.docker.com/), and available [Kubernetes deployment options](/docs/containers?topic=containers-app#plan_apps). If you want to deploy serverless apps, make yourself familiar with [Knative](/docs/containers?topic=containers-cs_network_planning).

Kubernetes and {{site.data.keyword.containerlong_notm}} provide multiple options for how to [expose an app and keep an app private](/docs/containers?topic=containers-cs_network_planning), [add persistent storage](/docs/containers?topic=containers-storage_planning), [integrate other services](/docs/containers?topic=containers-ibm-3rd-party-integrations), and how you can [secure your workloads and protect sensitive data](/docs/containers?topic=containers-security#container). Before you move your app to a cluster in {{site.data.keyword.containerlong_notm}}, verify that you can run your app as a containerized app on the supported Ubuntu 16.64, 18.64 operating system and that Kubernetes and {{site.data.keyword.containerlong_notm}} provide the capabilities that your workload needs.

**Do cluster admins and developers interact with each other?** </br>
Yes. Cluster admins and developers must interact frequently so that cluster admins understand workload requirements to provide this capability in the cluster, and so that developers know about available limitations, integrations, and security principles that they must consider in their app development process.

## Sizing your Kubernetes cluster to support your workload
{: #sizing}

Figuring out how many worker nodes you need in your cluster to support your workload is not an exact science. You might need to test different configurations and adapt. Good thing that you are using {{site.data.keyword.containerlong_notm}}, where you can add and remove worker nodes in response to your workload demands.
{: shortdesc}

To get started on sizing your cluster, ask yourself the following questions.

### How many resources does my app require?
{: #sizing_resources}

First, let's begin with your existing or project workload usage.

1.  Calculate your workload's average CPU and memory usage. For example, you might view the Task Manager on a Windows machine, or run the `top` command on a Mac or Linux. You might also use a metrics service and run reports to calculate workload usage.
2.  Anticipate the number of requests that your workload must serve so that you can decide how many app replicas you want to handle the workload. For example, you might design an app instance to handle 1000 requests per minute and anticipate that your workload must serve 10000 requests per minute. If so, you might decide to make 12 app replicas, with 10 to handle the anticipated amount and an extra 2 for surge capacity.

### What else besides my app might use resources in the cluster?
{: #sizing_other}

Now let's add some other features that you might use.



1.  Consider whether your app pulls large or many images, which can take up local storage on the worker node.
2.  Decide whether you want to [integrate services](/docs/containers?topic=containers-supported_integrations#supported_integrations) into your cluster, such as [Helm](/docs/containers?topic=containers-helm#public_helm_install) or [Prometheus ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/coreos/prometheus-operator/tree/master/contrib/kube-prometheus). These integrated services and add-ons spin up pods that consume cluster resources.

### What type of availability do I want my workload to have?
{: #sizing_availability}

Don't forget that you want your workload to be up as much as possible!

1.  Plan out your strategy for [highly available clusters](/docs/containers?topic=containers-ha_clusters#ha_clusters), such as deciding between single or multizone clusters.
2.  Review [highly available deployments](/docs/containers?topic=containers-app#highly_available_apps) to help decide how you can make your app available.

### How many worker nodes do I need to handle my workload?
{: #sizing_workers}

Now that you have a good idea of what your workload looks like, let's map the estimated usage onto your available cluster configurations.

    *  **Single zone clusters**: Plan to have at least three worker nodes in your cluster. Further, you want one extra node's worth of CPU and memory capacity available within the cluster.
    *  **Multizone clusters**: Plan to have at least two worker nodes per zone, so six nodes across three zones in total. Additionally, plan for the total capacity of your cluster to be at least 150% of your total workload's required capacity, so that if one zone goes down, you have resources available to maintain the workload.
2.  Align the app size and worker node capacity with one of the [available worker node flavors](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes). To see available flavors in a zone, run `ibmcloud ks machine-types <zone>`.
    *   **Don't overload worker nodes**: To avoid your pods competing for CPU or running inefficiently, you must know what resources your apps require so that you can plan the number of worker nodes that you need. For example, if your apps require less resources than the resources that are available on the worker node, you can limit the number of pods that you deploy to one worker node. Keep your worker node at around 75% capacity to leave space for other pods that might need to be scheduled. If your apps require more resources than you have available on your worker node, use a different worker node flavor that can fulfill these requirements. You know that your worker nodes are overloaded when they frequently report back a status of `NotReady` or evict pods due to the lack of memory or other resources.
    *   **Larger vs. smaller worker node flavors**: Larger nodes can be more cost efficient than smaller nodes, particularly for workloads that are designed to gain efficiency when they process on a high-performance machine. However, if a large worker node goes down, you need to be sure that your cluster has enough capacity to gracefully reschedule all the workload pods onto other worker nodes in the cluster. Smaller worker can help you scale more gracefully.
    *   **Replicas of your app**: To determine the number of worker nodes that you want, you can also consider how many replicas of your app that you want to run. For example, if you know that your workload requires 32 CPU cores, and you plan to run 16 replicas of your app, each replica pod needs 2 CPU cores. If you want to run only one app pod per worker node, you can order an appropriate number of worker nodes for your cluster type to support this configuration.
3.  Run performance tests to continue refining the number of worker nodes you need in your cluster, with representative latency, scalability, data set, and workload requirements.
4.  For workloads that need to scale up and down in response to resource requests, set up the [horizontal pod autoscaler](/docs/containers?topic=containers-app#app_scaling) and [cluster worker pool autoscaler](/docs/containers?topic=containers-ca#ca).

<br />


## Structuring your Kubernetes environment
{: #kube_env}

Your {{site.data.keyword.containerlong_notm}} is linked to one IBM Cloud infrastructure portfolio only. Within your account, you can create clusters that are composed of a master and various worker nodes. IBM manages the master, and you can create a mix of worker pools that pool together individual machines of the same flavor, or memory and CPU specs. Within the cluster, you can further organize resources by using namespaces and labels. Choose the right mix of cluster, machine types, and organization strategies so that you can make sure that your teams and workloads get the resources that they need.
{:shortdesc}

### What type of cluster and machine types should I get?
{: #env_flavors}

**Types of clusters**: Decide whether you want a [single zone, multizone, or multiple cluster setup](/docs/containers?topic=containers-ha_clusters#ha_clusters). Multizone clusters are available in [all six worldwide {{site.data.keyword.cloud_notm}} metro regions](/docs/containers?topic=containers-regions-and-zones#zones). Also keep in mind that worker nodes vary by zone.

**Types of worker nodes**: In general, your intensive workloads are more suited to run on bare metal physical machines, whereas for cost-effective testing and development work, you might choose virtual machines on shared or dedicated shared hardware. With bare metal worker nodes, your cluster has a network speed of 10 Gbps and hyper-threaded cores that offer higher throughput. Virtual machines come with a network speed of 1 Gbps and regular cores that do not offer hyper-threading. [Check out the machine isolation and flavors that are available](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes).

### Do I use multiple clusters, or just add more workers to an existing cluster?
{: #env_multicluster}

The number of clusters that you create depends on your workload, company policies and regulations, and what you want to do with the computing resources. You can also review security information about this decision in [Container isolation and security](/docs/containers?topic=containers-security#container).

**Multiple clusters**: You need to set up [a global load balancer](/docs/containers?topic=containers-ha_clusters#multiple_clusters) and copy and apply the same configuration YAML files in each to balance workloads across the clusters. Therefore, multiple clusters are generally more complex to manage, but can help you achieve important goals such as the following.
*  Comply with security policies that require you to isolate workloads.
*  Test how your app runs in a different version of Kubernetes or other cluster software such as Calico.
*  Create a cluster with your app in another region for higher performance for users in that geographical area.
*  Configure user access on the cluster-instance level instead of customizing and managing multiple RBAC policies to control access within a cluster at the namespace level.

**Fewer or single clusters**: Fewer clusters can help you to reduce operational effort and per-cluster costs for fixed resources. Instead of making more clusters, you can add worker pools for different machine types of computing resources available for the app and service components that you want to use. When you develop the app, the resources it uses are in the same zone, or otherwise closely connected in a multizone, so that you can make assumptions about latency, bandwidth, or correlated failures. However, it becomes even more important for you to organize your cluster by using namespaces, resource quotas, and labels.

### How can I set up my resources within the cluster?
{: #env_resources}

<dl>
<dt>Consider your worker node capacity.</dt>
  <dd>To get the most out of your worker node's performance, consider the following:
  <ul><li><strong>Keep up your core strength</strong>: Each machine has a certain number of cores. Depending on your app's workload, set a limit for the number of pods per core, such as 10.</li>
  <li><strong>Avoid node overload</strong>: Similarly, just because a node can contain more than 100 pods doesn't mean that you want it to. Depending on your app's workload, set a limit for the number of pods per node, such as 40.</li>
  <li><strong>Don't tap out your cluster bandwidth</strong>: Keep in mind that network bandwidth on scaling virtual machines is around 1000 Mbps. If you need hundreds of worker nodes in a cluster, split it up into multiple clusters with fewer nodes, or order bare metal nodes.</li>
  <li><strong>Sorting out your services</strong>: Plan out how many services that you need for your workload before you deploy. Networking and port forwarding rules are put into Iptables. If you anticipate a larger number of services, such as more than 5,000 services, split up the cluster into multiple clusters.</li></ul></dd>
<dt>Provision different types of machines for a mix of computing resources.</dt>
  <dd>Everyone likes choices, right? With {{site.data.keyword.containerlong_notm}}, you have [a mix of machine types](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes) that you can deploy: from bare metal for intensive workloads to virtual machines for rapid scaling. Use labels or namespaces to organize deployments to your machines. When you create a deployment, limit it so that your app's pod deploys only on machines with the right mix of resources. For example, you might want to limit a database application to a bare metal machine with a significant amount of local disk storage like the `md1c.28x512.4x4tb`.</dd>
<dt>Set up multiple namespaces when you have multiple teams and projects that share the cluster.</dt>
  <dd><p>Namespaces are kind of like a cluster within the cluster. They are a way to divide up cluster resources by using [resource quotas ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/policy/resource-quotas/) and [default limits ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/tasks/administer-cluster/memory-default-namespace/). When you make new namespaces, be sure to set up proper [RBAC policies](/docs/containers?topic=containers-users#rbac) to control access. For more information, see [Share a cluster with namespaces ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/tasks/administer-cluster/namespaces/) in the Kubernetes documentation.</p>
  <p>If you have a small cluster, a couple dozen users, and resources that are similar (such as different versions of the same software), you probably don't need multiple namespaces. You can use labels instead.</p></dd>
<dt>Set resource quotas so that users in your cluster must use resource requests and limits</dt>
  <dd>To ensure that every team has the necessary resources to deploy services and run apps in the cluster, you must set up [resource quotas](https://kubernetes.io/docs/concepts/policy/resource-quotas/) for every namespace. Resource quotas determine the deployment constraints for a namespace, such as the number of Kubernetes resources that you can deploy, and the amount of CPU and memory that can be consumed by those resources. After you set a quota, users must include resource requests and limits in their deployments.</dd>
<dt>Organize your Kubernetes objects with labels</dt>
  <dd><p>To organize and select your Kubernetes resources such as `pods` or `nodes`, [use Kubernetes labels ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/). By default, {{site.data.keyword.containerlong_notm}} applies some labels, including `arch`, `os`, `region`, `zone`, and `machine-type`.</p>
  <p>Example use cases for labels include [limiting network traffic to edge worker nodes](/docs/containers?topic=containers-edge), [deploying an app to a GPU machine](/docs/containers?topic=containers-app#gpu_app), and [restricting your app workloads![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/) to run on worker nodes that meet certain machine type or SDS capabilities, such as bare metal worker nodes. To see what labels are already applied to a resource, use the <code>kubectl get</code> command with the <code>--show-labels</code> flag. For example:</p>
  <p><pre class="pre"><code>kubectl get node &lt;node_ID&gt; --show-labels</code></pre></p>
  To apply labels to worker nodes, [create your worker pool](/docs/containers?topic=containers-add_workers#add_pool) with labels or [update an existing worker pool](/docs/containers?topic=containers-add_workers#worker_pool_labels).</dd>
</dl>




<br />


## Making your resources highly available
{: #kube_ha}

While no system is entirely failsafe, you can take steps to increase the high availability of your apps and services in {{site.data.keyword.containerlong_notm}}.
{:shortdesc}

Review more information about making resources highly available.
* [Reduce potential points of failure](/docs/containers?topic=containers-ha#ha).
* [Create multizone clusters](/docs/containers?topic=containers-ha_clusters#ha_clusters).
* [Plan highly available deployments](/docs/containers?topic=containers-app#highly_available_apps) that use features such as replica sets and pod anti-affinity across multizones.
* [Run containers that are based on images in a cloud-based public registry](/docs/containers?topic=containers-images).
* [Plan data storage](/docs/containers?topic=containers-storage_planning#persistent_storage_overview). Especially for multizone clusters, consider using a cloud service such as [{{site.data.keyword.cloudant_short_notm}}](/docs/services/Cloudant?topic=cloudant-getting-started#getting-started) or [{{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-about-ibm-cloud-object-storage).
* For multizone clusters, enable a [load balancer service](/docs/containers?topic=containers-loadbalancer#multi_zone_config) or the Ingress [multizone load balancer](/docs/containers?topic=containers-ingress#ingress) to expose your apps publicly.

<br />


## Setting up service discovery
{: #service_discovery}

Each of your pods in your Kubernetes cluster has an IP address. But when you deploy an app to your cluster, you don't want to rely on the pod IP address for service discovery and networking. Pods are removed and replaced frequently and dynamically. Instead, use a Kubernetes service, which represents a group of pods and provides a stable entry point through the service's virtual IP address, called its `cluster IP`. For more information, see the Kubernetes documentation on [Services ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/services-networking/service/#discovering-services).
{:shortdesc}

### Can I customize the Kubernetes cluster DNS provider?
{: #services_dns}

When you create services and pods, they are assigned a DNS name so that your app containers can use the DNS service IP to resolve DNS names. You can customize the pod DNS to specify name servers, searches, and object list options. For more information, see [Configuring the cluster DNS provider](/docs/containers?topic=containers-cluster_dns#cluster_dns).
{: shortdesc}



### How can I make sure that my services are connected to the right deployments and ready to go?
{: #services_connected}

For most services, add a selector to your service `.yaml` file so that it applies to pods that run your apps by that label. Many times when your app first starts up, you don't want it to process requests right away. Add a readiness probe to your deployment so that traffic is only sent to a pod that is considered ready. For an example of a deployment with a service that uses labels and sets a readiness probe, check out this [NGINX YAML](https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/deploy-apps-clusters/nginx_preferredAntiAffinity.yaml).
{: shortdesc}

Sometimes, you don't want the service to use a label. For example, you might have an external database or want to point the service to another service in a different namespace within the cluster. When this happens, you have to manually add an endpoints object and link it to the service.


### How do I control network traffic among the services that run in my cluster?
{: #services_network_traffic}

By default, pods can communicate with other pods in the cluster, but you can block traffic to certain pods or namespaces with network policies. Additionally, if you expose your app externally by using a NodePort, load balancer, or Ingress service, you might want to set up advanced network policies to block traffic. In {{site.data.keyword.containerlong_notm}}, you can use Calico to manage Kubernetes and Calico [network policies to control traffic](/docs/containers?topic=containers-network_policies#network_policies).

If you have various microservices that run across platforms for which you need to connect, manage, and secure network traffic, consider using a service mesh tool such as the [managed Istio add-on](/docs/containers?topic=containers-istio).

You can also [set up edge nodes](/docs/containers?topic=containers-edge#edge) to increase the security and isolation of your cluster by restricting the networking workload to select worker nodes.



### How can I expose my services on the Internet?
{: #services_expose_apps}

You can create three types of services for external networking: NodePort, LoadBalancer, and Ingress. For more information, see [Planning networking services](/docs/containers?topic=containers-cs_network_planning#external).

As you plan how many `Service` objects you need in your cluster, keep in mind that Kubernetes uses `iptables` to handle networking and port forwarding rules. If you run a large number of services in your cluster, such as 5000, performance might be impacted.



## Deploying app workloads to clusters
{: #deployments}

With Kubernetes, you declare many types of objects in YAML configuration files such as pods, deployments, and jobs. These objects describe things like what containerized apps are running, what resources they use, and what policies manage their behavior for restarting, updating, replicating, and more. For more information, see the Kubernetes docs for [Configuration best practices ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/configuration/overview/).
{: shortdesc}

### I thought that I needed to put my app in a container. Now what's all this stuff about pods?
{: #deploy_pods}

A [pod ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/workloads/pods/pod/) is the smallest deployable unit that Kubernetes can manage. You put your container (or a group of containers) into a pod and use the pod configuration file to tell the pod how to run the container and share resources with other pods. All containers that you put into a pod run in a shared context, which means that they share the same virtual or physical machine.
{: shortdesc}

**What to put in a container**: As you think about your application's components, consider whether they have significantly different resource requirements for things like CPU and memory. Could some components run at a best effort, where going down for a little while to divert resources to other areas is acceptable? Is another component customer-facing, so it's critical for it to stay up? Split them up into separate containers. You can always deploy them to the same pod so that they run together in sync.

**What to put in a pod**: The containers for your app don't always have to be in the same pod. In fact, if you have a component that is stateful and difficult to scale, such as a database service, put it in a different pod that you can schedule on a worker node with more resources to handle the workload. If your containers work correctly if they run on different worker nodes, then use multiple pods. If they need to be on the same machine and scale together, group the containers into the same pod.

### So if I can just use a pod, why do I need all these different types of objects?
{: #deploy_objects}

Creating a pod YAML file is easy. You can write one with just a few lines as follows.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx
    ports:
    - containerPort: 80
```
{: codeblock}

But you don't want to stop there. If the node that your pod runs on goes down, then your pod goes down with it and isn't rescheduled. Instead, use a [deployment ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) to support pod rescheduling, replica sets, and rolling updates. A basic deployment is almost as easy to make as a pod. Instead of defining the container in the `spec` by itself, however, you specify `replicas` and a `template` in the deployment `spec`. The template has its own `spec` for the containers within it, such as follows.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 80
```
{: codeblock}

You can keep adding features, such as pod anti-affinity or resource limits, all in the same YAML file.

For a more detailed explanation of different features that you can add to your deployment, take a look at [Making your app deployment YAML file](/docs/containers?topic=containers-app#app_yaml).
{: tip}

### How can I organize my deployments to make them easier to update and manage?
{: #deploy_organize}

Now that you have a good idea of what to include in your deployment, you might wonder how are you going to manage all these different YAML files? Not to mention the objects that they create in your Kubernetes environment!

A few tips for organizing deployment YAML files:
*  Use a version-control system, such as Git.
*  Group closely related Kubernetes objects within a single YAML file. For example, if you are creating a `deployment`, you might also add the `service` file to the YAML. Separate objects with `---` such as follows:
    ```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
    ...
    ---
    apiVersion: v1
    kind: Service
    metadata:
    ...
    ```
    {: codeblock}
*  You can use the `kubectl apply -f` command to apply to an entire directory, not just a single file.
*  Try out the [`kustomize` project](/docs/containers?topic=containers-app#kustomize) that you can use to help write, customize, and reuse your Kubernetes resource YAML configurations.

Within the YAML file, you can use labels or annotations as metadata to manage your deployments.

**Labels**: [Labels ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/) are `key:value` pairs that can be attached to Kubernetes objects such as pods and deployments. They can be whatever you want, and are useful for selecting objects based on the label information. Labels provide the foundation for grouping objects. Some ideas for labels:
* `app: nginx`
* `version: v1`
* `env: dev`

**Annotations**: [Annotations ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/overview/working-with-objects/annotations/) are similar to labels in that they are also `key:value` pairs. They are better for non-identifying information that can be leveraged by tools or libraries, such as holding extra information about where an object came from, how to use the object, pointers to related tracking repos, or a policy about the object. You don't select objects based on annotations.

### What else can I do to prepare my app for deployment?
{: #deploy_prep}

Many things! See [Preparing your containerized app to run in clusters](/docs/containers?topic=containers-app#plan_apps). The topic includes information about:
*  The kinds of apps you can run in Kubernetes, including tips for stateful and stateless apps.
*  Migrating apps to Kubernetes.
*  Sizing your cluster based on your workload requirements.
*  Setting up additional app resources such as IBM services, storage, logging, and monitoring.
*  Using variables within your deployment.
*  Controlling access to your app.

<br />


## Packaging your application
{: #packaging}

If you want to run your app in multiple clusters, public and private environments, or even multiple cloud providers, you might wonder how you can make your deployment strategy work across these environments. With {{site.data.keyword.cloud_notm}} and other open source tools, you can package your application to help automate deployments.
{: shortdesc}

<dl>
<dt>Automate your infrastructure</dt>
  <dd>You can use the open source [Terraform](/docs/terraform?topic=terraform-getting-started#getting-started) tool to automate the provisioning of {{site.data.keyword.cloud_notm}} infrastructure, including Kubernetes clusters. Follow along with this tutorial to [plan, create, and update deployment environments](/docs/tutorials?topic=solution-tutorials-plan-create-update-deployments#plan-create-update-deployments). After you create a cluster, you can also set up the [{{site.data.keyword.containerlong_notm}} cluster autoscaler](/docs/containers?topic=containers-ca) so that your worker pool scales up and down worker nodes in response to your workload's resource requests.</dd>
<dt>Set up a continuous integration and delivery (CI/CD) pipeline</dt>
  <dd>With your app configuration files organized in a source control management system such as Git, you can build your pipeline to test and deploy code to different environments, such as `test` and `prod`. Check out [this tutorial on Continuous Deployment to Kubernetes](/docs/tutorials?topic=solution-tutorials-continuous-deployment-to-kubernetes#continuous-deployment-to-kubernetes).</dd>
<dt>Package your app configuration files</dt>
  <dd>With the [Helm ![External link icon](../icons/launch-glyph.svg "External link icon")](https://helm.sh/docs/) Kubernetes package manager, you can specify all Kubernetes resources that your app requires in a Helm chart. Then, you can use Helm to create the YAML configuration files and deploy these files in your cluster. You can also [integrate {{site.data.keyword.cloud_notm}}-provided Helm charts ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/kubernetes/solutions/helm-charts) to extend your cluster's capabilities, such as with a block storage plug-in.<p class="tip">Are you just looking for an easy way to create YAML file templates? Some people use Helm to do just that, or you might try out other community tools such as [`ytt` ![External link icon](../icons/launch-glyph.svg "External link icon")](https://get-ytt.io/).</p></dd>
</dl>

<br />


## Keeping your app up-to-date
{: #updating}

You put in a lot of effort preparing for the next version of your app. You can use {{site.data.keyword.cloud_notm}} and Kubernetes update tools to make sure that your app is running in a secured cluster environment, as well as to roll out different versions of your app.
{: shortdesc}

### How can I keep my cluster in a supported state?
{: #updating_kube}

Make sure that your cluster runs a [supported Kubernetes version](/docs/containers?topic=containers-cs_versions#cs_versions) at all times. When a new Kubernetes minor version is released, an older version is shortly deprecated after and then becomes unsupported. For more information, see [Updating the Kubernetes master](/docs/containers?topic=containers-update#master) and [worker nodes](/docs/containers?topic=containers-update#worker_node).

### What app update strategies can I use?
{: #updating_apps}

To update your app, you can choose from various strategies such as the following. You might start with a rolling deployment or instantaneous switch before you progress to a more complicated canary deployment.

<dl>
<dt>Rolling deployment</dt>
  <dd>You can use Kubernetes-native functionality to create a `v2` deployment and to gradually replace your previous `v1` deployment. This approach requires that apps are backwards-compatible so that users who are served the `v2` app version do not experience any breaking changes. For more information, see [Managing rolling deployments to update your apps](/docs/containers?topic=containers-app#app_rolling).</dd>
<dt>Instantaneous switch</dt>
  <dd>Also referred to as a blue-green deployment, an instantaneous switch requires double the compute resources to have two versions of an app running at once. With this approach, you can switch your users to the newer version in near real time. Make sure that you use service label selectors (such as `version: green` and `version: blue`) to make sure that requests are sent to the right app version. You can create the new `version: green` deployment, wait until it is ready, and then delete the `version: blue` deployment. Or, you can perform a [rolling update](/docs/containers?topic=containers-app#app_rolling), but set the `maxUnavailable` parameter to `0%` and the `maxSurge` parameter to `100%`.</dd>
<dt>Canary or A/B deployment</dt>
  <dd>A more complex update strategy, a canary deployment is when you pick a percentage of users such as 5% and send them to the new app version. You collect metrics in your logging and monitoring tools on how the new app version performs, do A/B testing, and then roll out the update to more users. As with all deployments, labeling the app (such as `version: stable` and `version: canary`) is critical. To manage canary deployments, you might [install the managed Istio add-on service mesh](/docs/containers?topic=containers-istio#istio), [set up Sysdig monitoring for your cluster](/docs/services/Monitoring-with-Sysdig/tutorials?topic=Sysdig-kubernetes_cluster#kubernetes_cluster), and then use the Istio service mesh for A/B testing as described [in this blog post ![External link icon](../icons/launch-glyph.svg "External link icon")](https://sysdig.com/blog/monitor-istio/). Or, use Knative for canary deployments.</dd>
</dl>

<br />


## Monitoring your cluster performance
{: #monitoring_health}

With effective logging and monitoring of your cluster and apps, you can better understand your environment to optimize resource utilization and troubleshoot issues that might arise. To set up logging and monitoring solutions for your cluster, see [Logging and monitoring](/docs/containers?topic=containers-health#health).
{: shortdesc}

As you set up your logging and monitoring, think about the following considerations.

<dl>
<dt>Collect logs and metrics to determine cluster health</dt>
  <dd>Kubernetes includes a metrics server to help determine basic cluster-level performance. You can review these metrics in your [Kubernetes dashboard](/docs/containers?topic=containers-app#cli_dashboard) or in a terminal by running `kubectl top (pods | nodes)` commands. You might include these commands in your automation.<br><br>
  Forward logs to a log analysis tool so that you can analyze your logs later. Define the verbosity and level of logs that you want to log to avoid storing more logs than you need. Logs can quickly eat up lots of storage, which can impact your app performance and can make log analysis more difficult.</dd>
<dt>Test app performance</dt>
  <dd>After you set up logging and monitoring, conduct performance testing. In a test environment, deliberately create various non-ideal scenarios, such as deleting all worker nodes in a zone to replicate a zonal failure. Review the logs and metrics to check how your app recovers.</dd>
<dt>Prepare for audits</dt>
  <dd>In addition to app logs and cluster metrics, you want to set up activity tracking so that you have an auditable record of who performed what cluster and Kubernetes actions. For more information, see [{{site.data.keyword.cloudaccesstrailshort}}](/docs/containers?topic=containers-at_events#at_events).</dd>
</dl>
