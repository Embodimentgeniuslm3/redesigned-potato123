---

copyright:
  years: 2014, 2019
lastupdated: "2019-07-19"

keywords: kubernetes, iks, lb2.0, nlb

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



# About NLBs
{: #loadbalancer-about}

When you create a standard cluster, {{site.data.keyword.containerlong}} automatically provisions a portable public subnet and a portable private subnet.
{: shortdesc}

* The portable public subnet provides 5 usable IP addresses. 1 portable public IP address is used by the default [public Ingress ALB](/docs/containers?topic=containers-ingress). The remaining 4 portable public IP addresses can be used to expose single apps to the internet by creating public network load balancer services, or NLBs.
* The portable private subnet provides 5 usable IP addresses. 1 portable private IP address is used by the default [private Ingress ALB](/docs/containers?topic=containers-ingress#private_ingress). The remaining 4 portable private IP addresses can be used to expose single apps to a private network by creating private load balancer services, or NLBs.

Portable public and private IP addresses are static floating IPs and do not change when a worker node is removed. If the worker node that the NLB IP address is on is removed, a Keepalived daemon that constantly monitors the IP automatically moves the IP to another worker node. You can assign any port to your NLB. The NLB serves as the external entry point for incoming requests for the app. To access the NLB from the internet, you can use the public IP address of your NLB and the assigned port in the format `<IP_address>:<port>`. You can also create DNS entries for NLBs by registering the NLB IP addresses with host names.

When you expose an app with an NLB service, your app is automatically made available over the service's NodePorts too. [NodePorts](/docs/containers?topic=containers-nodeport) are accessible on every public and private IP address of every worker node within the cluster. To block traffic to NodePorts while you are using an NLB, see [Controlling inbound traffic to network load balancer (NLB) or NodePort services](/docs/containers?topic=containers-network_policies#block_ingress).

<br />


## Comparison of basic and DSR load balancing in version 1.0 and 2.0 NLBs
{: #comparison}

When you create an NLB, you can choose a version 1.0 NLB, which performs basic oad balancing, or version 2.0 NLB, which performs direct server return (DSR) load balancing. Note that version 2.0 NLBs are in beta.
{: shortdesc}

**How are versions 1.0 and 2.0 NLBs similar?**

Version 1.0 and 2.0 NLBs are both Layer 4 load balancers that exist in the Linux kernel space. Both versions run inside the cluster, and use worker node resources. Therefore, the available capacity of the NLBs is always dedicated to your own cluster. Additionally, both versions of NLBs do not terminate the connection. Instead, they forward connections to an app pod.

**How are versions 1.0 and 2.0 NLBs different?**

When a client sends a request to your app, the NLB routes request packets to the worker node IP address where an app pod exists. Version 1.0 NLBs use network address translation (NAT) to rewrite the request packet's source IP address to the IP of worker node where a load balancer pod exists. When the worker node returns the app response packet, it uses that worker node IP where the NLB exists. The NLB must then send the response packet to the client. To prevent the IP address from being rewritten, you can [enable source IP preservation](/docs/containers?topic=containers-loadbalancer#node_affinity_tolerations). However, source IP preservation requires load balancer pods and app pods to run on the same worker so that the request doesn't have to be forwarded to another worker. You must add node affinity and tolerations to app pods. For more information about basic load balancing with version 1.0 NLBs, see [v1.0: Components and architecture of basic load balancing](#v1_planning).

As opposed to version 1.0 NLBs, version 2.0 NLBs don't use NAT when forwarding requests to app pods on other workers. When an NLB 2.0 routes a client request, it uses IP over IP (IPIP) to encapsulate the original request packet into another packet. This encapsulating IPIP packet has a source IP of the worker node where the load balancer pod is, which allows the original request packet to preserve the client IP as its source IP address. The worker node then uses direct server return (DSR) to send the app response packet to the client IP. The response packet skips the NLB and is sent directly to the client, decreasing the amount of traffic that the NLB must handle. For more information about DSR load balancing with version 2.0 NLBs, see [v2.0: Components and architecture of DSR load balancing](#planning_ipvs).

<br />


## Components and architecture of an NLB 1.0
{: #v1_planning}

The TCP/UDP network load balancer (NLB) 1.0 uses Iptables, a Linux kernel feature to load balance requests across an app's pods.
{: shortdesc}

### Traffic flow in a single-zone cluster
{: #v1_single}

The following diagram shows how an NLB 1.0 directs communication from the internet to an app in a single-zone cluster.
{: shortdesc}

<img src="images/cs_loadbalancer_planning.png" width="410" alt="Expose an app in {{site.data.keyword.containerlong_notm}} by using an NLB 1.0" style="width:410px; border-style: none"/>

1. A request to your app uses the public IP address of your NLB and the assigned port on the worker node.

2. The request is automatically forwarded to the NLB service's internal cluster IP address and port. The internal cluster IP address is accessible inside the cluster only.

3. `kube-proxy` routes the request to the NLB service for the app.

4. The request is forwarded to the private IP address of the app pod. The source IP address of the request package is changed to the public IP address of the worker node where the app pod is running. If multiple app instances are deployed in the cluster, the NLB routes the requests between the app pods.

### Traffic flow in a multizone cluster
{: #v1_multi}

The following diagram shows how a network load balancer (NLB) 1.0 directs communication from the internet to an app in a multizone cluster.
{: shortdesc}

<img src="images/cs_loadbalancer_planning_multizone.png" width="500" alt="Use an NLB 1.0 to load balance apps in multizone clusters" style="width:500px; border-style: none"/>

By default, each NLB 1.0 is set up in one zone only. To achieve high availability, you must deploy an NLB 1.0 in every zone where you have app instances. Requests are handled by the NLBs in various zones in a round-robin cycle. Additionally, each NLB routes requests to the app instances in its own zone and to app instances in other zones.

<br />


## Components and architecture of an NLB 2.0 (beta)
{: #planning_ipvs}

Network load balancer (NLB) 2.0 capabilities are in beta. To use an NLB 2.0, you must [update your cluster's master and worker nodes](/docs/containers?topic=containers-update) to Kubernetes version 1.12 or later.
{: note}

The NLB 2.0 is a Layer 4 load balancer that uses the Linux kernel's IP Virtual Server (IPVS). The NLB 2.0 supports TCP and UDP, runs in front of multiple worker nodes, and uses IP over IP (IPIP) tunneling to distribute traffic that arrives to a single NLB IP address across those worker nodes.

Want more details about the load-balancing deployment patterns that are available in {{site.data.keyword.containerlong_notm}}? Check out this [blog post ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/blogs/bluemix/2018/10/ibm-cloud-kubernetes-service-deployment-patterns-for-maximizing-throughput-and-availability/).
{: tip}

### Traffic flow in a single-zone cluster
{: #ipvs_single}

The following diagram shows how an NLB 2.0 directs communication from the internet to an app in a single zone cluster.
{: shortdesc}

<img src="images/cs_loadbalancer_ipvs_planning.png" width="600" alt="Expose an app in {{site.data.keyword.containerlong_notm}} by using a version 2.0 NLB" style="width:600px; border-style: none"/>

1. A client request to your app uses the public IP address of your NLB and the assigned port on the worker node. In this example, the NLB has a virtual IP address of 169.61.23.130, which is currently on worker 10.73.14.25.

2. The NLB encapsulates the client request packet (labeled as "CR" in the image) inside an IPIP packet (labeled as "IPIP"). The client request packet retains the client IP as its source IP address. The IPIP encapsulating packet uses the worker 10.73.14.25 IP as its source IP address.

3. The NLB routes the IPIP packet to a worker that an app pod is on, 10.73.14.26. If multiple app instances are deployed in the cluster, the NLB routes the requests between the workers where app pods are deployed.

4. Worker 10.73.14.26 unpacks the IPIP encapsulating packet, and then unpacks the client request packet. The client request packet is forwarded to the app pod on that worker node.

5. Worker 10.73.14.26 then uses the source IP address from the original request packet, the client IP, to return the app pod's response packet directly to the client.

### Traffic flow in a multizone cluster
{: #ipvs_multi}

The traffic flow through a multizone cluster follows the same path as [traffic through a single zone cluster](#ipvs_single). In a multizone cluster, the NLB routes requests to the app instances in its own zone and to app instances in other zones. The following diagram shows how version 2.0 NLBs in each zone direct traffic from the internet to an app in a multizone cluster.
{: shortdesc}

<img src="images/cs_loadbalancer_ipvs_multizone.png" alt="Expose an app in {{site.data.keyword.containerlong_notm}} by using an NLB 2.0" style="width:500px; border-style: none"/>

By default, each version 2.0 NLB is set up in one zone only. You can achieve higher availability by deploying a version 2.0 NLB in every zone where you have app instances.
