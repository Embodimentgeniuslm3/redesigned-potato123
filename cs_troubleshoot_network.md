---

copyright:
  years: 2014, 2019
lastupdated: "2019-07-18"

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



# Troubleshooting cluster networking
{: #cs_troubleshoot_network}

As you use {{site.data.keyword.containerlong}}, consider these techniques for troubleshooting cluster networking.
{: shortdesc}

Having trouble connecting to your app through Ingress? Try [debugging Ingress](/docs/containers?topic=containers-cs_troubleshoot_debug_ingress).
{: tip}

While you troubleshoot, you can use the [{{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool](/docs/containers?topic=containers-cs_troubleshoot#debug_utility) to run tests and gather pertinent networking, Ingress, and strongSwan information from your cluster.
{: tip}

## Cannot connect to an app via a network load balancer (NLB) service
{: #cs_loadbalancer_fails}

{: tsSymptoms}
You publicly exposed your app by creating an NLB service in your cluster. When you tried to connect to your app by using the public IP address of the NLB, the connection failed or timed out.

{: tsCauses}
Your NLB service might not be working properly for one of the following reasons:

-   The cluster is a free cluster or a standard cluster with only one worker node.
-   The cluster is not fully deployed yet.
-   The configuration script for your NLB service includes errors.

{: tsResolve}
To troubleshoot your NLB service:

1.  Check that you set up a standard cluster that is fully deployed and has at least two worker nodes to ensure high availability for your NLB service.

  ```
  ibmcloud ks workers --cluster <cluster_name_or_ID>
  ```
  {: pre}

    In your CLI output, make sure that the **Status** of your worker nodes displays **Ready** and that the **Machine Type** shows a machine type other than **free**.

2. For version 2.0 NLBs: Ensure that you complete the [NLB 2.0 prerequisites](/docs/containers?topic=containers-loadbalancer-v2#ipvs_provision).

3. Check the accuracy of the configuration file for your NLB service.
    * Version 2.0 NLBs:
        ```
        apiVersion: v1
        kind: Service
        metadata:
          name: myservice
          annotations:
            service.kubernetes.io/ibm-load-balancer-cloud-provider-enable-features: "ipvs"
        spec:
          type: LoadBalancer
          selector:
            <selector_key>:<selector_value>
          ports:
           - protocol: TCP
             port: 8080
          externalTrafficPolicy: Local
        ```
        {: screen}

        1. Check that you defined **LoadBalancer** as the type for your service.
        2. Check that you included the `service.kubernetes.io/ibm-load-balancer-cloud-provider-enable-features: "ipvs"` annotation.
        3. In the `spec.selector` section of the LoadBalancer service, ensure that the `<selector_key>` and `<selector_value>` is the same as the key/value pair that you used in the `spec.template.metadata.labels` section of your deployment YAML. If labels do not match, the **Endpoints** section in your LoadBalancer service displays **<none>** and your app is not accessible from the internet.
        4. Check that you used the **port** that your app listens on.
        5. Check that you set `externalTrafficPolicy` to `Local`.

    * Version 1.0 NLBs:
        ```
        apiVersion: v1
        kind: Service
        metadata:
          name: myservice
        spec:
          type: LoadBalancer
          selector:
            <selector_key>:<selector_value>
          ports:
           - protocol: TCP
             port: 8080
        ```
        {: screen}

        1. Check that you defined **LoadBalancer** as the type for your service.
        2. In the `spec.selector` section of the LoadBalancer service, ensure that the `<selector_key>` and `<selector_value>` is the same as the key/value pair that you used in the `spec.template.metadata.labels` section of your deployment YAML. If labels do not match, the **Endpoints** section in your LoadBalancer service displays **<none>** and your app is not accessible from the internet.
        3. Check that you used the **port** that your app listens on.

3.  Check your NLB service and review the **Events** section to find potential errors.

    ```
    kubectl describe service <myservice>
    ```
    {: pre}

    Look for the following error messages:

    <ul><li><pre class="screen"><code>Clusters with one node must use services of type NodePort</code></pre></br>To use the NLB service, you must have a standard cluster with at least two worker nodes.</li>
    <li><pre class="screen"><code>No cloud provider IPs are available to fulfill the NLB service request. Add a portable subnet to the cluster and try again</code></pre></br>This error message indicates that no portable public IP addresses are left to be allocated to your NLB service. Refer to <a href="/docs/containers?topic=containers-subnets#subnets">Adding subnets to clusters</a> to find information about how to request portable public IP addresses for your cluster. After portable public IP addresses are available to the cluster, the NLB service is automatically created.</li>
    <li><pre class="screen"><code>Requested cloud provider IP <cloud-provider-ip> is not available. The following cloud provider IPs are available: <available-cloud-provider-ips></code></pre></br>You defined a portable public IP address for your load balancer YAML by using the **`loadBalancerIP`** section, but this portable public IP address is not available in your portable public subnet. In the **`loadBalancerIP`** section your configuration script, remove the existing IP address and add one of the available portable public IP addresses. You can also remove the **`loadBalancerIP`** section from your script so that an available portable public IP address can be allocated automatically.</li>
    <li><pre class="screen"><code>No available nodes for NLB services</code></pre>You do not have enough worker nodes to deploy an NLB service. One reason might be that you deployed a standard cluster with more than one worker node, but the provisioning of the worker nodes failed.</li>
    <ol><li>List available worker nodes.</br><pre class="pre"><code>kubectl get nodes</code></pre></li>
    <li>If at least two available worker nodes are found, list the worker node details.</br><pre class="pre"><code>ibmcloud ks worker-get --cluster &lt;cluster_name_or_ID&gt; --worker &lt;worker_ID&gt;</code></pre></li>
    <li>Make sure that the public and private VLAN IDs for the worker nodes that were returned by the <code>kubectl get nodes</code> and the <code>ibmcloud ks worker-get</code> commands match.</li></ol></li></ul>

4.  If you use a custom domain to connect to your NLB service, make sure that your custom domain is mapped to the public IP address of your NLB service.
    1.  Find the public IP address of your NLB service.
        ```
        kubectl describe service <service_name> | grep "LoadBalancer Ingress"
        ```
        {: pre}

    2.  Check that your custom domain is mapped to the portable public IP address of your NLB service in the Pointer record (PTR).

<br />


## Cannot connect to an app via Ingress
{: #cs_ingress_fails}

{: tsSymptoms}
You publicly exposed your app by creating an Ingress resource for your app in your cluster. When you tried to connect to your app by using the public IP address or subdomain of the Ingress application load balancer (ALB), the connection failed or timed out.

{: tsResolve}
First, check that your cluster is fully deployed and has at least 2 worker nodes available per zone to ensure high availability for your ALB.
```
ibmcloud ks workers --cluster <cluster_name_or_ID>
```
{: pre}

In your CLI output, make sure that the **Status** of your worker nodes displays **Ready** and that the **Machine Type** shows a machine type other than **free**.

* If your standard cluster is fully deployed and has at least 2 worker nodes per zone, but no **Ingress Subdomain** is available, see [Cannot get a subdomain for Ingress ALB](/docs/containers?topic=containers-cs_troubleshoot_network#cs_subnet_limit).
* For other issues, troubleshoot your Ingress setup by following the steps in [Debugging Ingress](/docs/containers?topic=containers-cs_troubleshoot_debug_ingress).
<br />


## Ingress application load balancer (ALB) secret issues
{: #cs_albsecret_fails}

{: tsSymptoms}
After you deploy an Ingress application load balancer (ALB) secret to your cluster by using the `ibmcloud ks alb-cert-deploy` command, the `Description` field is not updating with the secret name when you view your certificate in {{site.data.keyword.cloudcerts_full_notm}}.

When you list information about the ALB secret, the status says `*_failed`. For example, `create_failed`, `update_failed`, `delete_failed`.

{: tsResolve}
Review the following reasons why the ALB secret might fail and the corresponding troubleshooting steps:

<table>
<caption>Troubleshooting Ingress application load balancer secrets</caption>
 <thead>
 <th>Why it's happening</th>
 <th>How to fix it</th>
 </thead>
 <tbody>
 <tr>
 <td>You do not have the required access roles to download and update certificate data.</td>
 <td>Check with your account Administrator to assign you the following {{site.data.keyword.cloud_notm}} IAM roles:<ul><li>The **Manager** and **Writer** service roles for your {{site.data.keyword.cloudcerts_full_notm}} instance. For more information, see <a href="/docs/services/certificate-manager?topic=certificate-manager-managing-service-access-roles#managing-service-access-roles">Managing service access</a> for {{site.data.keyword.cloudcerts_short}}.</li><li>The <a href="/docs/containers?topic=containers-users#platform">**Administrator** platform role</a> for the cluster.</li></ul></td>
 </tr>
 <tr>
 <td>The certificate CRN provided at time of create, update, or remove does not belong to the same account as the cluster.</td>
 <td>Check that the certificate CRN you provided is imported to an instance of the {{site.data.keyword.cloudcerts_short}} service that is deployed in the same account as your cluster.</td>
 </tr>
 <tr>
 <td>The certificate CRN provided at time of create is incorrect.</td>
 <td><ol><li>Check the accuracy of the certificate CRN string you provide.</li><li>If the certificate CRN is found to be accurate, then try to update the secret: <code>ibmcloud ks alb-cert-deploy --update --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></li><li>If this command results in the <code>update_failed</code> status, then remove the secret: <code>ibmcloud ks alb-cert-rm --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt;</code></li><li>Deploy the secret again: <code>ibmcloud ks alb-cert-deploy --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></li></ol></td>
 </tr>
 <tr>
 <td>The certificate CRN provided at time of update is incorrect.</td>
 <td><ol><li>Check the accuracy of the certificate CRN string you provide.</li><li>If the certificate CRN is found to be accurate, then remove the secret: <code>ibmcloud ks alb-cert-rm --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt;</code></li><li>Deploy the secret again: <code>ibmcloud ks alb-cert-deploy --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></li><li>Try to update the secret: <code>ibmcloud ks alb-cert-deploy --update --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></li></ol></td>
 </tr>
 <tr>
 <td>The {{site.data.keyword.cloudcerts_long_notm}} service is experiencing downtime.</td>
 <td>Check that your {{site.data.keyword.cloudcerts_short}} service is up and running.</td>
 </tr>
 <tr>
 <td>Your imported secret has the same name as the IBM-provided Ingress secret.</td>
 <td>Rename your secret. You can check the name of the IBM-provided Ingress secret by running `ibmcloud ks cluster-get --cluster <cluster_name_or_ID> | grep Ingress`.</td>
 </tr>
 </tbody></table>

<br />


## Cannot get a subdomain for Ingress ALB, ALB does not deploy in a zone, or cannot deploy a load balancer
{: #cs_subnet_limit}

{: tsSymptoms}
* No Ingress subdomain: When you run `ibmcloud ks cluster-get --cluster <cluster>`, your cluster is in a `normal` state but no **Ingress Subdomain** is available.
* An ALB does not deploy in a zone: When you have a multizone cluster and run `ibmcloud ks albs --cluster <cluster>`, no ALB is deployed in a zone. For example, if you have worker nodes in 3 zones, you might see an output similar to the following in which a public ALB did not deploy to the third zone.
  ```
  ALB ID                                            Enabled    Status     Type      ALB IP           Zone    Build                          ALB VLAN ID
  private-cr96039a75fddb4ad1a09ced6699c88888-alb1   false      disabled   private   -                dal10   ingress:411/ingress-auth:315   2294021
  private-cr96039a75fddb4ad1a09ced6699c88888-alb2   false      disabled   private   -                dal12   ingress:411/ingress-auth:315   2234947
  private-cr96039a75fddb4ad1a09ced6699c88888-alb3   false      disabled   private   -                dal13   ingress:411/ingress-auth:315   2234943
  public-cr96039a75fddb4ad1a09ced6699c88888-alb1    true       enabled    public    169.xx.xxx.xxx   dal10   ingress:411/ingress-auth:315   2294019
  public-cr96039a75fddb4ad1a09ced6699c88888-alb2    true       enabled    public    169.xx.xxx.xxx   dal12   ingress:411/ingress-auth:315   2234945
  ```
  {: screen}
* Cannot deploy a load balancer: When you describe the `ibm-cloud-provider-vlan-ip-config` configmap, you might see an error message similar to the following example output.
  ```
  kubectl get cm ibm-cloud-provider-vlan-ip-config
  ```
  {: pre}
  ```
  Warning  CreatingLoadBalancerFailed ... ErrorSubnetLimitReached: There are already the maximum number of subnets permitted in this VLAN.
  ```
  {: screen}

{: tsCauses}
In standard clusters, the first time that you create a cluster in a zone, a public VLAN and a private VLAN in that zone are automatically provisioned for you in your IBM Cloud infrastructure account. In that zone, 1 public portable subnet is requested on the public VLAN that you specify and 1 private portable subnet is requested on the private VLAN that you specify. For {{site.data.keyword.containerlong_notm}}, VLANs have a limit of 40 subnets. If the cluster's VLAN in a zone already reached that limit, the **Ingress Subdomain** fails to provision, the public Ingress ALB for that zone fails to provision, or you might not have a portable public IP address available to create a network load balancer (NLB).

To view how many subnets a VLAN has:
1.  From the [IBM Cloud infrastructure console](https://cloud.ibm.com/classic?), select **Network** > **IP Management** > **VLANs**.
2.  Click the **VLAN Number** of the VLAN that you used to create your cluster. Review the **Subnets** section to see whether 40 or more subnets exist.

{: tsResolve}
If you need a new VLAN, order one by [contacting {{site.data.keyword.cloud_notm}} support](/docs/infrastructure/vlans?topic=vlans-ordering-premium-vlans#ordering-premium-vlans). Then, [create a cluster](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_create) that uses this new VLAN.

If you have another VLAN that is available, you can [set up VLAN spanning](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning) in your existing cluster. After, you can add new worker nodes to the cluster that use the other VLAN with available subnets. To check if VLAN spanning is already enabled, use the `ibmcloud ks vlan-spanning-get --region <region>` [command](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get).

If you are not using all the subnets in the VLAN, you can reuse subnets on the VLAN by adding them to your cluster.
1. Check that the subnet that you want to use is available.
  <p class="note">The infrastructure account that you use might be shared across multiple {{site.data.keyword.cloud_notm}} accounts. In this case, even if you run the `ibmcloud ks subnets` command to see subnets with **Bound Clusters**, you can see information only for your clusters. Check with the infrastructure account owner to make sure that the subnets are available and not in use by any other account or team.</p>

2. Use the [`ibmcloud ks cluster-subnet-add` command](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_subnet_add) to make an existing subnet available to your cluster.

3. Verify that the subnet was successfully created and added to your cluster. The subnet CIDR is listed in the **Subnet VLANs** section.
    ```
    ibmcloud ks cluster-get --showResources <cluster_name_or_ID>
    ```
    {: pre}

    In this example output, a second subnet was added to the `2234945` public VLAN:
    ```
    Subnet VLANs
    VLAN ID   Subnet CIDR          Public   User-managed
    2234947   10.xxx.xx.xxx/29     false    false
    2234945   169.xx.xxx.xxx/29    true     false
    2234945   169.xx.xxx.xxx/29    true     false
    ```
    {: screen}

4. Verify that the portable IP addresses from the subnet that you added are used for the ALBs or load balancers in your cluster. It might take several minutes for the services to use the portable IP addresses from the newly-added subnet.
  * No Ingress subdomain: Run `ibmcloud ks cluster-get --cluster <cluster>` to verify that the **Ingress Subdomain** is populated.
  * An ALB does not deploy in a zone: Run `ibmcloud ks albs --cluster <cluster>` to verify that the missing ALB is deployed.
  * Cannot deploy a load balancer: Run `kubectl get svc -n kube-system` to verify that the load balancer has an **EXTERNAL-IP**.

<br />


## Connection via WebSocket closes after 60 seconds
{: #cs_ingress_websocket}

{: tsSymptoms}
Your Ingress service exposes an app that uses a WebSocket. However, the connection between a client and your WebSocket app closes when no traffic is sent between them for 60 seconds.

{: tsCauses}
The connection to your WebSocket app might drop after 60 seconds of inactivity for one of the following reasons:

* Your Internet connection has a proxy or firewall that doesn't tolerate long connections.
* A timeout in the ALB to the WebSocket app terminates the connection.

{: tsResolve}
To prevent the connection from closing after 60 seconds of inactivity:

1. If you connect to your WebSocket app through a proxy or firewall, make sure the proxy or firewall isn't configured to automatically terminate long connections.

2. To keep the connection alive, you can increase the value of the timeout or set up a heartbeat in your app.
<dl><dt>Change the timeout</dt>
<dd>Increase the value of the `proxy-read-timeout` in your ALB configuration. For example, to change the timeout from `60s` to a larger value like `300s`, add this [annotation](/docs/containers?topic=containers-ingress_annotation#connection) to your Ingress resource file: `ingress.bluemix.net/proxy-read-timeout: "serviceName=<service_name> timeout=300s"`. The timeout is changed for all public ALBs in your cluster.</dd>
<dt>Set up a heartbeat</dt>
<dd>If you don't want to change the ALB's default read timeout value, set up a heartbeat in your WebSocket app. When you set up a heartbeat protocol by using a framework like [WAMP ![External link icon](../icons/launch-glyph.svg "External link icon")](https://wamp-proto.org/), the app's upstream server periodically sends a "ping" message on a timed interval and the client responds with a "pong" message. Set the heartbeat interval to 58 seconds or less so that the "ping/pong" traffic keeps the connection open before the 60-second timeout is enforced.</dd></dl>

<br />


## Source IP preservation fails when using tainted nodes
{: #cs_source_ip_fails}

{: tsSymptoms}
You enabled source IP preservation for a [version 1.0 load balancer](/docs/containers?topic=containers-loadbalancer#node_affinity_tolerations) or [Ingress ALB](/docs/containers?topic=containers-ingress-settings#preserve_source_ip) service by changing `externalTrafficPolicy` to `Local` in the service's configuration file. However, no traffic reaches the back-end service for your app.

{: tsCauses}
When you enable source IP preservation for load balancer or Ingress ALB services, the source IP address of the client request is preserved. The service forwards traffic to app pods on the same worker node only to ensure that the request packet's IP address isn't changed. Typically, load balancer or Ingress ALB service pods are deployed to the same worker nodes that the app pods are deployed to. However, some situations exist where the service pods and app pods might not be scheduled onto the same worker node. If you use [Kubernetes taints ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/) on worker nodes, any pods that don't have a taint toleration are prevented from running on the tainted worker nodes. Source IP preservation might not be working based on the type of taint you used:

* **Edge node taints**: You [added the `dedicated=edge` label](/docs/containers?topic=containers-edge#edge_nodes) to two or more worker nodes on each public VLAN in your cluster to ensure that Ingress and load balancer pods deploy to those worker nodes only. Then, you also [tainted those edge nodes](/docs/containers?topic=containers-edge#edge_workloads) to prevent any other workloads from running on edge nodes. However, you didn't add an edge node affinity rule and toleration to your app deployment. Your app pods can't be scheduled on the same tainted nodes as the service pods, and no traffic reaches the back-end service for your app.

* **Custom taints**: You used custom taints on several nodes so that only app pods with that taint toleration can deploy to those nodes. You added affinity rules and tolerations to the deployments of your app and load balancer or Ingress service so that their pods deploy to only those nodes. However, `ibm-cloud-provider-ip` `keepalived` pods that are automatically created in the `ibm-system` namespace ensure that the load balancer pods and the app pods are always scheduled onto the same worker node. These `keepalived` pods don't have the tolerations for the custom taints that you used. They can't be scheduled on the same tainted nodes that your app pods are running on, and no traffic reaches the back-end service for your app.

{: tsResolve}
Resolve the issue by choosing one of the following options:

* **Edge node taints**: To ensure that your load balancer and app pods deploy to tainted edge nodes, [add edge node affinity rules and tolerations to your app deployment](/docs/containers?topic=containers-loadbalancer#lb_edge_nodes). Load balancer and Ingress ALB pods have these affinity rules and tolerations by default.

* **Custom taints**: Remove custom taints that the `keepalived` pods don't have tolerations for. Instead, you can [label worker nodes as edge nodes, and then taint those edge nodes](/docs/containers?topic=containers-edge).

If you complete one of the above options but the `keepalived` pods are still not scheduled, you can get more information about the `keepalived` pods:

1. Get the `keepalived` pods.
    ```
    kubectl get pods -n ibm-system
    ```
    {: pre}

2. In the output, look for `ibm-cloud-provider-ip` pods that have a **Status** of `Pending`. Example:
    ```
    ibm-cloud-provider-ip-169-61-XX-XX-55967b5b8c-7zv9t     0/1       Pending   0          2m        <none>          <none>
    ibm-cloud-provider-ip-169-61-XX-XX-55967b5b8c-8ptvg     0/1       Pending   0          2m        <none>          <none>
    ```
    {:screen}

3. Describe each `keepalived` pod and look for the **Events** section. Address any error or warning messages that are listed.
    ```
    kubectl describe pod ibm-cloud-provider-ip-169-61-XX-XX-55967b5b8c-7zv9t -n ibm-system
    ```
    {: pre}

<br />


## Cluster service DNS resolution sometimes fails with CoreDNS but not KubeDNS
{: #coredns_issues}

{: tsSymptoms}
Your app sometimes fails to resolve DNS names for cluster services. The failures occur only when CoreDNS, not KubeDNS, is the [configured cluster DNS provider](/docs/containers?topic=containers-cluster_dns). You might see error messages similar to the following.

```
Name or service not known
```
{: screen}

```
No address associated with hostname or similar
```
{: screen}

{: tsCauses}
CoreDNS caching for cluster services behaves differently than the previous cluster DNS provider, KubeDNS, which can cause issues for apps that use an older DNS client.

{: tsResolve}
Update your app to use a newer DNS client. For example, if your app image is Ubuntu 14.04, the older DNS client results in periodic failures. When you update the image to Ubuntu 16.04, the DNS client works.

You can also remove the cache plug-in configurations from the `coredns` configmap in the `kube-system` namespace. For more information on customizing CoreDNS, see [Customizing the cluster DNS provider](/docs/containers?topic=containers-cluster_dns#dns_customize).

<br />


## Cannot establish VPN connectivity with the strongSwan Helm chart
{: #cs_vpn_fails}

{: tsSymptoms}
When you check VPN connectivity by running `kubectl exec  $STRONGSWAN_POD -- ipsec status`, you do not see a status of `ESTABLISHED`, or the VPN pod is in an `ERROR` state or continues to crash and restart.

{: tsCauses}
Your Helm chart configuration file has incorrect values, missing values, or syntax errors.

{: tsResolve}
When you try to establish VPN connectivity with the strongSwan Helm chart, it is likely that the VPN status will not be `ESTABLISHED` the first time. You might need to check for several types of issues and change your configuration file accordingly. To troubleshoot your strongSwan VPN connectivity:

1. [Test and verify the strongSwan VPN connectivity](/docs/containers?topic=containers-vpn#vpn_test) by running the five Helm tests that are included in the strongSwan chart definition.

2. If you are unable to establish VPN connectivity after running the Helm tests, you can run the VPN debugging tool that is packaged inside of the VPN pod image.

    1. Set the `STRONGSWAN_POD` environment variable.

        ```
        export STRONGSWAN_POD=$(kubectl get pod -l app=strongswan,release=vpn -o jsonpath='{ .items[0].metadata.name }')
        ```
        {: pre}

    2. Run the debugging tool.

        ```
        kubectl exec  $STRONGSWAN_POD -- vpnDebug
        ```
        {: pre}

        The tool outputs several pages of information as it runs various tests for common networking issues. Output lines that begin with `ERROR`, `WARNING`, `VERIFY`, or `CHECK` indicate possible errors with the VPN connectivity.

    <br />


## Cannot install a new strongSwan Helm chart release
{: #cs_strongswan_release}

{: tsSymptoms}
You modify your strongSwan Helm chart and try to install your new release by running `helm install -f config.yaml --name=vpn ibm/strongswan`. However, you see the following error:
```
Error: release vpn failed: deployments.extensions "vpn-strongswan" already exists
```
{: screen}

{: tsCauses}
This error indicates that the previous release of the strongSwan chart was not completely uninstalled.

{: tsResolve}

1. Delete the previous chart release.
    ```
    helm delete --purge vpn
    ```
    {: pre}

2. Delete the deployment for the previous release. Deletion of the deployment and associated pod takes up to 1 minute.
    ```
    kubectl delete deploy vpn-strongswan
    ```
    {: pre}

3. Verify that the deployment has been deleted. The deployment `vpn-strongswan` does not appear in the list.
    ```
    kubectl get deployments
    ```
    {: pre}

4. Re-install the updated strongSwan Helm chart with a new release name.
    ```
    helm install -f config.yaml --name=vpn ibm/strongswan
    ```
    {: pre}

<br />


## strongSwan VPN connectivity fails after you add or delete worker nodes
{: #cs_vpn_fails_worker_add}

{: tsSymptoms}
You previously established a working VPN connection by using the strongSwan IPSec VPN service. However, after you added or deleted a worker node on your cluster, you experience one or more of the following symptoms:

* You do not have a VPN status of `ESTABLISHED`
* You cannot access new worker nodes from your on-prem network
* You cannot access the remote network from pods that are running on new worker nodes

{: tsCauses}
If you added a worker node to a worker pool:

* The worker node was provisioned on a new private subnet that is not exposed over the VPN connection by your existing `localSubnetNAT` or `local.subnet` settings
* VPN routes cannot be added to the worker node because the worker has taints or labels that are not included in your existing `tolerations` or `nodeSelector` settings
* The VPN pod is running on the new worker node, but the public IP address of that worker node is not allowed through the on-premises firewall

If you deleted a worker node:

* That worker node was the only node where a VPN pod was running, due to restrictions on certain taints or labels in your existing `tolerations` or `nodeSelector` settings

{: tsResolve}
Update the Helm chart values to reflect the worker node changes:

1. Delete the existing Helm chart.

    ```
    helm delete --purge <release_name>
    ```
    {: pre}

2. Open the configuration file for your strongSwan VPN service.

    ```
    helm inspect values ibm/strongswan > config.yaml
    ```
    {: pre}

3. Check the following settings and change the settings to reflect the deleted or added worker nodes as necessary.

    If you added a worker node:

    <table>
    <caption>Worker node settings</caption?
     <thead>
     <th>Setting</th>
     <th>Description</th>
     </thead>
     <tbody>
     <tr>
     <td><code>localSubnetNAT</code></td>
     <td>The added worker might be deployed on a new, different private subnet than the other existing subnets that other worker nodes are on. If you use subnet NAT to remap your cluster's private local IP addresses and the worker was added on a new subnet, add the new subnet CIDR to this setting.</td>
     </tr>
     <tr>
     <td><code>nodeSelector</code></td>
     <td>If you previously limited VPN pod deployment to workers with a specific label, ensure the added worker node also has that label.</td>
     </tr>
     <tr>
     <td><code>tolerations</code></td>
     <td>If the added worker node is tainted, then change this setting to allow the VPN pod to run on all tainted workers with any taints or specific taints.</td>
     </tr>
     <tr>
     <td><code>local.subnet</code></td>
     <td>The added worker might be deployed on a new, different private subnet than the existing subnets that other workers are on. If your apps are exposed by NodePort or LoadBalancer services on the private network and the apps are on the added worker, add the new subnet CIDR to this setting. **Note**: If you add values to `local.subnet`, check the VPN settings for the on-premises subnet to see whether they also must be updated.</td>
     </tr>
     </tbody></table>

    If you deleted a worker node:

    <table>
    <caption>Worker node settings</caption>
     <thead>
     <th>Setting</th>
     <th>Description</th>
     </thead>
     <tbody>
     <tr>
     <td><code>localSubnetNAT</code></td>
     <td>If you use subnet NAT to remap specific private local IP addresses, remove any IP addresses from this setting that are from the old worker. If you use subnet NAT to remap entire subnets and no workers remain on a subnet, remove that subnet CIDR from this setting.</td>
     </tr>
     <tr>
     <td><code>nodeSelector</code></td>
     <td>If you previously limited VPN pod deployment to a single worker and that worker was deleted, change this setting to allow the VPN pod to run on other workers.</td>
     </tr>
     <tr>
     <td><code>tolerations</code></td>
     <td>If the worker that you deleted was not tainted, but the only workers that remain are tainted, change this setting to allow the VPN pod to run on workers with any taints or specific taints.
     </td>
     </tr>
     </tbody></table>

4. Install the new Helm chart with your updated values.

    ```
    helm install -f config.yaml --name=<release_name> ibm/strongswan
    ```
    {: pre}

5. Check the chart deployment status. When the chart is ready, the **STATUS** field near the top of the output has a value of `DEPLOYED`.

    ```
    helm status <release_name>
    ```
    {: pre}

6. In some cases, you might need to change your on-prem settings and your firewall settings to match the changes you made to the VPN configuration file.

7. Start the VPN.
    * If the VPN connection is initiated by the cluster (`ipsec.auto` is set to `start`), start the VPN on the on-prem gateway, and then start the VPN on the cluster.
    * If the VPN connection is initiated by the on-prem gateway (`ipsec.auto` is set to `auto`), start the VPN on the cluster, and then start the VPN on the on-prem gateway.

8. Set the `STRONGSWAN_POD` environment variable.

    ```
    export STRONGSWAN_POD=$(kubectl get pod -l app=strongswan,release=<release_name> -o jsonpath='{ .items[0].metadata.name }')
    ```
    {: pre}

9. Check the status of the VPN.

    ```
    kubectl exec  $STRONGSWAN_POD -- ipsec status
    ```
    {: pre}

    * If the VPN connection has a status of `ESTABLISHED`, the VPN connection was successful. No further action is needed.

    * If you are still having connection issues, see [Cannot establish VPN connectivity with the strongSwan Helm chart](#cs_vpn_fails) to further troubleshoot your VPN connection.

<br />



## Cannot retrieve Calico network policies
{: #cs_calico_fails}

{: tsSymptoms}
When you try to view Calico network policies in your cluster by running `calicoctl get policy`, you get one of the following unexpected results or error messages:
- An empty list
- A list of old Calico v2 policies instead of v3 policies
- `Failed to create Calico API client: syntax error in calicoctl.cfg: invalid config file: unknown APIVersion 'projectcalico.org/v3'`

When you try to view Calico network policies in your cluster by running `calicoctl get GlobalNetworkPolicy`, you get one of the following unexpected results or error messages:
- An empty list
- `Failed to create Calico API client: syntax error in calicoctl.cfg: invalid config file: unknown APIVersion 'v1'`
- `Failed to create Calico API client: syntax error in calicoctl.cfg: invalid config file: unknown APIVersion 'projectcalico.org/v3'`
- `Failed to get resources: Resource type 'GlobalNetworkPolicy' is not supported`

{: tsCauses}
To use Calico policies, four factors must all align: your cluster Kubernetes version, Calico CLI version, Calico configuration file syntax, and view policy commands. One or more of these factors is not at the correct version.

{: tsResolve}
You must use the v3.3 or later Calico CLI, `calicoctl.cfg` v3 configuration file syntax, and the `calicoctl get GlobalNetworkPolicy` and `calicoctl get NetworkPolicy` commands.

To ensure that all Calico factors align:

1. [Install and configure a version 3.3 or later Calico CLI](/docs/containers?topic=containers-network_policies#cli_install).
2. Ensure that any policies you create and want to apply to your cluster use [Calico v3 syntax ![External link icon](../icons/launch-glyph.svg "External link icon")](https://docs.projectcalico.org/v3.3/reference/calicoctl/resources/networkpolicy). If you have an existing policy `.yaml` or `.json` file in Calico v2 syntax, you can convert it to Calico v3 syntax by using the [`calicoctl convert` command ![External link icon](../icons/launch-glyph.svg "External link icon")](https://docs.projectcalico.org/v3.3/reference/calicoctl/commands/convert).
3. To [view policies](/docs/containers?topic=containers-network_policies#view_policies), ensure that you use `calicoctl get GlobalNetworkPolicy` for global policies and `calicoctl get NetworkPolicy --namespace <policy_namespace>` for policies that are scoped to specific namespaces.

<br />


## Cannot add worker nodes due to an invalid VLAN ID
{: #suspended}

{: tsSymptoms}
Your {{site.data.keyword.cloud_notm}} account was suspended, or all worker nodes in your cluster were deleted. After the account is reactivated, you cannot add worker nodes when you try to resize or rebalance your worker pool. You see an error message similar to the following:

```
SoftLayerAPIError(SoftLayer_Exception_Public): Could not obtain network VLAN with id #123456.
```
{: screen}

{: tsCauses}
When an account is suspended, the worker nodes within the account are deleted. If a cluster has no worker nodes, IBM Cloud infrastructure reclaims the associated public and private VLANs. However, the cluster worker pool still has the previous VLAN IDs in its metadata and uses these unavailable IDs when you rebalance or resize the pool. The nodes fail to create because the VLANs are no longer associated with the cluster.

{: tsResolve}

You can [delete your existing worker pool](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_rm), then [create a new worker pool](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_create).

Alternatively, you can keep your existing worker pool by ordering new VLANs and using these to create new worker nodes in the pool.

Before you begin: [Log in to your account. If applicable, target the appropriate resource group. Set the context for your cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  To get the zones that you need new VLAN IDs for, note the **Location** in the following command output. **Note**: If your cluster is a multizone, you need VLAN IDs for each zone.

    ```
    ibmcloud ks clusters
    ```
    {: pre}

2.  Get a new private and public VLAN for each zone that your cluster is in by [contacting {{site.data.keyword.cloud_notm}} support](/docs/infrastructure/vlans?topic=vlans-ordering-premium-vlans#ordering-premium-vlans).

3.  Note the new private and public VLAN IDs for each zone.

4.  Note the name of your worker pools.

    ```
    ibmcloud ks worker-pools --cluster <cluster_name_or_ID>
    ```
    {: pre}

5.  Use the `zone-network-set` [command](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_zone_network_set) to change the worker pool network metadata.

    ```
    ibmcloud ks zone-network-set --zone <zone> --cluster <cluster_name_or_ID> -- worker-pools <worker-pool> --private-vlan <private_vlan_ID> --public-vlan <public_vlan_ID>
    ```
    {: pre}

6.  **Multizone cluster only**: Repeat **Step 5** for each zone in your cluster.

7.  Rebalance or resize your worker pool to add worker nodes that use the new VLAN IDs. For example:

    ```
    ibmcloud ks worker-pool-resize --cluster <cluster_name_or_ID> --worker-pool <worker_pool> --size-per-zone <number_of_workers_per_zone>
    ```
    {: pre}

8.  Verify that your worker nodes are created.

    ```
    ibmcloud ks workers --cluster <cluster_name_or_ID> --worker-pool <worker_pool>
    ```
    {: pre}

<br />



## Getting help and support
{: #network_getting_help}

Still having issues with your cluster?
{: shortdesc}

-  In the terminal, you are notified when updates to the `ibmcloud` CLI and plug-ins are available. Be sure to keep your CLI up-to-date so that you can use all available commands and flags.
-   To see whether {{site.data.keyword.cloud_notm}} is available, [check the {{site.data.keyword.cloud_notm}} status page ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/status?selected=status).
-   Post a question in the [{{site.data.keyword.containerlong_notm}} Slack ![External link icon](../icons/launch-glyph.svg "External link icon")](https://ibm-container-service.slack.com).
    If you are not using an IBM ID for your {{site.data.keyword.cloud_notm}} account, [request an invitation](https://cloud.ibm.com/kubernetes/slack) to this Slack.
    {: tip}
-   Review the forums to see whether other users ran into the same issue. When you use the forums to ask a question, tag your question so that it is seen by the {{site.data.keyword.cloud_notm}} development teams.
    -   If you have technical questions about developing or deploying clusters or apps with {{site.data.keyword.containerlong_notm}}, post your question on [Stack Overflow ![External link icon](../icons/launch-glyph.svg "External link icon")](https://stackoverflow.com/questions/tagged/ibm-cloud+containers) and tag your question with `ibm-cloud`, `kubernetes`, and `containers`.
    -   For questions about the service and getting started instructions, use the [IBM Developer Answers ![External link icon](../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix) forum. Include the `ibm-cloud` and `containers` tags.
    See [Getting help](/docs/get-support?topic=get-support-getting-customer-support#using-avatar) for more details about using the forums.
-   Contact IBM Support by opening a case. To learn about opening an IBM support case, or about support levels and case severities, see [Contacting support](/docs/get-support?topic=get-support-getting-customer-support).
When you report an issue, include your cluster ID. To get your cluster ID, run `ibmcloud ks clusters`. You can also use the [{{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool](/docs/containers?topic=containers-cs_troubleshoot#debug_utility) to gather and export pertinent information from your cluster to share with IBM Support.
{: tip}

