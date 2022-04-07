---

copyright:
  years: 2014, 2019
lastupdated: "2019-07-09"

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


# Using the managed Istio add-on (beta)
{: #istio}

Istio on {{site.data.keyword.containerlong}} provides a seamless installation of Istio, automatic updates and lifecycle management of Istio control plane components, and integration with platform logging and monitoring tools.
{: shortdesc}

With one click, you can get all Istio core components, additional tracing, monitoring, and visualization, and the BookInfo sample app up and running. Istio on {{site.data.keyword.containerlong_notm}} is offered as a managed add-on, so {{site.data.keyword.cloud_notm}} automatically keeps all your Istio components up-to-date.

## Understanding Istio on {{site.data.keyword.containerlong_notm}}
{: #istio_ov}

### What is Istio?
{: #istio_ov_what_is}

[Istio ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/cloud/info/istio) is an open service mesh platform to connect, secure, control, and observe microservices on cloud platforms such as Kubernetes in {{site.data.keyword.containerlong_notm}}.
{:shortdesc}

When you shift monolith applications to a distributed microservice architecture, a set of new challenges arises such as how to control the traffic of your microservices, do dark launches and canary rollouts of your services, handle failures, secure the service communication, observe the services, and enforce consistent access policies across the fleet of services. To address these difficulties, you can leverage a service mesh. A service mesh provides a transparent and language-independent network for connecting, observing, securing, and controlling the connectivity between microservices. Istio provides insights and control over the service mesh by so that you can manage network traffic, load balance across microservices, enforce access policies, verify service identity, and more.

For example, by using Istio in your microservice mesh can help you:
- Achieve better visibility into the apps that run in your cluster
- Deploy canary versions of apps and control the traffic that is sent to them
- Enable automatic encryption of data that is transferred between microservices
- Enforce rate limiting and attribute-based whitelist and blacklist policies

An Istio service mesh is composed of a data plane and a control plane. The data plane consists of Envoy proxy sidecars in each app pod, which mediate communication between microservices. The control plane consists of Pilot, Mixer telemetry and policy, and Citadel, which apply Istio configurations in your cluster. For more information about each of these components, see the [`istio` add-on description](#istio_components).

### What is Istio on {{site.data.keyword.containerlong_notm}} (beta)?
{: #istio_ov_addon}

Istio on {{site.data.keyword.containerlong_notm}} is offered as a managed add-on that integrates Istio directly with your Kubernetes cluster.
{: shortdesc}

The managed Istio add-on is classified as beta and might be unstable or change frequently. Beta features also might not provide the same level of performance or compatibility that generally available features provide and are not intended for use in a production environment.
{: note}

**What does this look like in my cluster?**</br>
When you install the Istio add-on, the Istio control and data planes use the VLANs that your cluster is already connected to. Configuration traffic flows over the private network within your cluster, and does not require you to open any additional ports or IP addresses in your firewall. If you expose your Istio-managed apps with an Istio Gateway, external traffic requests to the apps flow over the public VLAN.

**How does the update process work?**</br>
The Istio version in the managed add-on is tested by {{site.data.keyword.cloud_notm}} and approved for the use in {{site.data.keyword.containerlong_notm}}. To update your Istio components to the most recent version of Istio supported by {{site.data.keyword.containerlong_notm}}, you can follow the steps in [Updating managed add-ons](/docs/containers?topic=containers-managed-addons#updating-managed-add-ons).  

If you need to use the latest version of Istio or customize your Istio installation, you can install the open source version of Istio by following the steps in the [Quick Start with {{site.data.keyword.cloud_notm}} tutorial ![External link icon](../icons/launch-glyph.svg "External link icon")](https://istio.io/docs/setup/kubernetes/quick-start-ibm/).
{: tip}

**Are there any limitations?** </br>
You cannot enable the managed Istio add-on in your cluster if you installed the [container image security enforcer admission controller](/docs/services/Registry?topic=registry-security_enforce#security_enforce) in your cluster.

<br />


## What can I install?
{: #istio_components}

Istio on {{site.data.keyword.containerlong_notm}} is offered as three managed add-ons in your cluster.
{: shortdesc}

<dl>
<dt>Istio (`istio`)</dt>
<dd>Installs the core components of Istio, including Prometheus. For more information about any of the following control plane components, see the [Istio documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://istio.io/docs/concepts/what-is-istio/).
  <ul><li>`Envoy` proxies inbound and outbound traffic for all services in the mesh. Envoy is deployed as a sidecar container in the same pod as your app container.</li>
  <li>`Mixer` provides telemetry collection and policy controls.<ul>
    <li>Telemetry pods are enabled with a Prometheus endpoint, which aggregates all telemetry data from the Envoy proxy sidecars and services in your app pods.</li>
    <li>Policy pods enforce access control, including rate limiting and applying whitelist and blacklist policies.</li></ul>
  </li>
  <li>`Pilot` provides service discovery for the Envoy sidecars and configures the traffic management routing rules for sidecars.</li>
  <li>`Citadel` uses identity and credential management to provide service-to-service and end-user authentication.</li>
  <li>`Galley` validates configuration changes for the other Istio control plane components.</li>
</ul></dd>
<dt>Istio extras (`istio-extras`)</dt>
<dd>Optional: Installs [Grafana ![External link icon](../icons/launch-glyph.svg "External link icon")](https://grafana.com/), [Jaeger ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.jaegertracing.io/), and [Kiali ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.kiali.io/) to provide extra monitoring, tracing, and visualization for Istio.</dd>
<dt>BookInfo sample app (`istio-sample-bookinfo`)</dt>
<dd>Optional: Deploys the [BookInfo sample application for Istio ![External link icon](../icons/launch-glyph.svg "External link icon")](https://istio.io/docs/examples/bookinfo/). This deployment includes the base demo setup and the default destination rules so that you can try out Istio's capabilities immediately.</dd>
</dl>

<br>
You can always see which Istio add-ons are enabled in your cluster by running the following command:
```
ibmcloud ks cluster-addons --cluster <cluster_name_or_ID>
```
{: pre}

<br />


## Installing the Istio add-ons
{: #istio_install}

Install Istio managed add-ons in an existing cluster.
{: shortdesc}

**Before you begin**</br>
* Ensure that you have the [**Writer** or **Manager** {{site.data.keyword.cloud_notm}} IAM service role](/docs/containers?topic=containers-users#platform) for {{site.data.keyword.containerlong_notm}}.
* [Create or use an existing standard cluster with at least 3 worker nodes that each have 4 cores and 16 GB memory (`b3c.4x16`) or more](/docs/containers?topic=containers-clusters#clusters_ui). Additionally, the cluster and worker nodes must run at least the minimum supported version of Kubernetes, which you can review by running `ibmcloud ks addon-versions --addon istio`.
* If you use an existing cluster and you previously installed Istio in the cluster by using the IBM Helm chart or through another method, [clean up that Istio installation](#istio_uninstall_other).

### Installing managed Istio add-ons from the console
{: #istio_install_ui}

1. In your [cluster dashboard ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/kubernetes/clusters), click the name of the cluster where you want to install the Istio add-ons.

2. Click the **Add-ons** tab.

3. On the Managed Istio card, click **Install**.

4. Select the **Istio** check box, and optionally the **Istio Extras** and the **Istio Sample** check boxes.

5. Click **Install**.

6. On the Managed Istio card, verify that the add-ons you enabled are listed.

### Installing managed Istio add-ons from the CLI
{: #istio_install_cli}

1. [Target the CLI to your cluster](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).

2. Enable the `istio` add-on and optionally the `istio-extras` and `istio-sample-bookinfo` add-ons.
  * `istio`:
    ```
    ibmcloud ks cluster-addon-enable istio --cluster <cluster_name_or_ID>
    ```
    {: pre}
  * `istio-extras`:
    ```
    ibmcloud ks cluster-addon-enable istio-extras --cluster <cluster_name_or_ID>
    ```
    {: pre}
  * `istio-sample-bookinfo`:
    ```
    ibmcloud ks cluster-addon-enable istio-sample-bookinfo --cluster <cluster_name_or_ID>
    ```
    {: pre}

3. Verify that the managed Istio add-ons that you installed are enabled in this cluster.
  ```
  ibmcloud ks cluster-addons --cluster <cluster_name_or_ID>
  ```
  {: pre}

  Example output:
  ```
  Name                      Version
  istio                     1.1.7
  istio-extras              1.1.7
  istio-sample-bookinfo     1.1.7
  ```
  {: screen}

4. You can also check out the individual components of each add-on in your cluster.
  - Components of `istio` and `istio-extras`: Ensure that the Istio services and their corresponding pods are deployed.
    ```
    kubectl get svc -n istio-system
    ```
    {: pre}

    ```
    NAME                     TYPE           CLUSTER-IP       EXTERNAL-IP                                                                    AGE
    grafana                  ClusterIP      172.21.98.154    <none>          3000/TCP                                                       2m
    istio-citadel            ClusterIP      172.21.221.65    <none>          8060/TCP,9093/TCP                                              2m
    istio-egressgateway      ClusterIP      172.21.46.253    <none>          80/TCP,443/TCP                                                 2m
    istio-galley             ClusterIP      172.21.125.77    <none>          443/TCP,9093/TCP                                               2m
    istio-ingressgateway     LoadBalancer   172.21.230.230   169.46.56.125   80:31380/TCP,443:31390/TCP,31400:31400/TCP,5011:31323/TCP,
                                                                              8060:32483/TCP,853:32628/TCP,15030:31601/TCP,15031:31915/TCP  2m
    istio-pilot              ClusterIP      172.21.171.29    <none>          15010/TCP,15011/TCP,8080/TCP,9093/TCP                          2m
    istio-policy             ClusterIP      172.21.140.180   <none>          9091/TCP,15004/TCP,9093/TCP                                    2m
    istio-sidecar-injector   ClusterIP      172.21.248.36    <none>          443/TCP                                                        2m
    istio-telemetry          ClusterIP      172.21.204.173   <none>          9091/TCP,15004/TCP,9093/TCP,42422/TCP                          2m
    jaeger-agent             ClusterIP      None             <none>          5775/UDP,6831/UDP,6832/UDP                                     2m
    jaeger-collector         ClusterIP      172.21.65.195    <none>          14267/TCP,14268/TCP                                            2m
    jaeger-query             ClusterIP      172.21.171.199   <none>          16686/TCP                                                      2m
    kiali                    ClusterIP      172.21.13.35     <none>          20001/TCP                                                      2m
    prometheus               ClusterIP      172.21.105.229   <none>          9090/TCP                                                       2m
    tracing                  ClusterIP      172.21.125.177   <none>          80/TCP                                                         2m
    zipkin                   ClusterIP      172.21.1.77      <none>          9411/TCP                                                       2m
    ```
    {: screen}

    ```
    kubectl get pods -n istio-system
    ```
    {: pre}

    ```
    NAME                                      READY   STATUS    RESTARTS   AGE
    grafana-76dcdfc987-94ldq                  1/1     Running   0          2m
    istio-citadel-869c7f9498-wtldz            1/1     Running   0          2m
    istio-egressgateway-69bb5d4585-qxxbp      1/1     Running   0          2m
    istio-galley-75d7b5bdb9-c9d9n             1/1     Running   0          2m
    istio-ingressgateway-5c8764db74-gh8xg     1/1     Running   0          2m
    istio-pilot-55fd7d886f-vv6fb              2/2     Running   0          2m
    istio-policy-6bb6f6ddb9-s4c8t             2/2     Running   0          2m
    istio-sidecar-injector-7d9845dbb7-r8nq5   1/1     Running   0          2m
    istio-telemetry-7695b4c4d4-tlvn8          2/2     Running   0          2m
    istio-tracing-55bbf55878-z4rd2            1/1     Running   0          2m
    kiali-77566cc66c-kh6lm                    1/1     Running   0          2m
    prometheus-5d5cb44877-lwrqx               1/1     Running   0          2m
    ```
    {: screen}

  - Components of `istio-sample-bookinfo`: Ensure that the BookInfo microservices and their corresponding pods are deployed.
    ```
    kubectl get svc -n default
    ```
    {: pre}

    ```
    NAME                      TYPE           CLUSTER-IP       EXTERNAL-IP    PORT(S)          AGE
    details                   ClusterIP      172.21.19.104    <none>         9080/TCP         2m
    productpage               ClusterIP      172.21.168.196   <none>         9080/TCP         2m
    ratings                   ClusterIP      172.21.11.131    <none>         9080/TCP         2m
    reviews                   ClusterIP      172.21.117.164   <none>         9080/TCP         2m
    ```
    {: screen}

    ```
    kubectl get pods -n default
    ```
    {: pre}

    ```
    NAME                                     READY     STATUS      RESTARTS   AGE
    details-v1-6865b9b99d-7v9h8              2/2       Running     0          2m
    productpage-v1-f8c8fb8-tbsz9             2/2       Running     0          2m
    ratings-v1-77f657f55d-png6j              2/2       Running     0          2m
    reviews-v1-6b7f6db5c5-fdmbq              2/2       Running     0          2m
    reviews-v2-7ff5966b99-zflkv              2/2       Running     0          2m
    reviews-v3-5df889bcff-nlmjp              2/2       Running     0          2m
    ```
    {: screen}

<br />


## Trying out the BookInfo sample app
{: #istio_bookinfo}

The BookInfo add-on (`istio-sample-bookinfo`) deploys the [BookInfo sample application for Istio ![External link icon](../icons/launch-glyph.svg "External link icon")](https://istio.io/docs/examples/bookinfo/) into the `default` namespace. This deployment includes the base demo setup and the default destination rules so that you can try out Istio's capabilities immediately.
{: shortdesc}

The four BookInfo microservices include:
* `productpage` calls the `details` and `reviews` microservices to populate the page.
* `details` contains book information.
* `reviews` contains book reviews and calls the `ratings` microservice.
* `ratings` contains book ranking information that accompanies a book review.

The `reviews` microservice has multiple versions:
* `v1` does not call the `ratings` microservice.
* `v2` calls the `ratings` microservice and displays ratings as 1 to 5 black stars.
* `v3` calls the `ratings` microservice and displays ratings as 1 to 5 red stars.

The deployment YAMLs for each of these microservices are modified so that Envoy sidecar proxies are pre-injected as containers into the microservices' pods before they are deployed. For more information about manual sidecar injection, see the [Istio documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://istio.io/docs/setup/kubernetes/sidecar-injection/). The BookInfo app is also already exposed on a public IP ingress address by an Istio Gateway. Although the BookInfo app can help you get started, the app is not meant for production use.

### Publicly accessing BookInfo
{: #istio_access_bookinfo}

Before you begin, [install the `istio`, `istio-extras`, and `istio-sample-bookinfo` managed add-ons](#istio_install) in a cluster.

1. Get the public address for your cluster.
  1. Set the ingress host.
    ```
    export INGRESS_HOST=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.status.loadBalancer.ingress[0].ip}')
    ```
    {: pre}

  2. Set the ingress port.
    ```
    export INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="http2")].port}')
    ```
    {: pre}

  3. Create a `GATEWAY_URL` environment variable that uses the ingress host and port.
     ```
     export GATEWAY_URL=$INGRESS_HOST:$INGRESS_PORT
     ```
     {: pre}

2. Curl the `GATEWAY_URL` variable to check that the BookInfo app is running. A `200` response means that the BookInfo app is running properly with Istio.
   ```
   curl -o /dev/null -s -w "%{http_code}\n" http://${GATEWAY_URL}/productpage
   ```
   {: pre}

3.  View the BookInfo web page in a browser.

    Mac OS or Linux:
    ```
    open http://$GATEWAY_URL/productpage
    ```
    {: pre}

    Windows:
    ```
    start http://$GATEWAY_URL/productpage
    ```
    {: pre}

4. Try refreshing the page several times. Different versions of the reviews section round-robin through red stars, black stars, and no stars.

### Exposing BookInfo by using an IBM-provided host name
{: #istio_expose_bookinfo}

When you enable the BookInfo add-on in your cluster, the Istio gateway `bookinfo-gateway` is created for you. The gateway uses Istio virtual service and destination rules to configure a load balancer, `istio-ingressgateway`, that publicly exposes the BookInfo app. In the following steps, you create a host name for the `istio-ingressgateway` load balancer IP address through which you can publicly access BookInfo.
{: shortdesc}

1. Register the IP address for the `istio-ingressgateway` load balancer by creating a DNS host name.
  ```
  ibmcloud ks nlb-dns-create --cluster <cluster_name_or_id> --ip $INGRESS_HOST
  ```
  {: pre}

2. Verify that the host name is created.
  ```
  ibmcloud ks nlb-dnss --cluster <cluster_name_or_id>
  ```
  {: pre}

  Example output:
  ```
  Hostname                                                                                IP(s)              Health Monitor   SSL Cert Status           SSL Cert Secret Name
  mycluster-a1b2cdef345678g9hi012j3kl4567890-0001.us-south.containers.appdomain.cloud     ["168.1.1.1"]      None             created                   <certificate>
  ```
  {: screen}

3. In a web browser, open the BookInfo product page.
  ```
  http://<host_name>/productpage
  ```
  {: codeblock}

4. Try refreshing the page several times. The requests to `http://<host_name>/productpage` are received by the ALB and are forwarded to the Istio gateway load balancer. The different versions of the `reviews` microservice are still returned randomly because the Istio gateway manages the virtual service and destination routing rules for microservices.


### Understanding what happened
{: #istio_bookinfo_understanding}

The BookInfo sample demonstrates how three of Istio's traffic management components work together to route ingress traffic to the app.
{: shortdesc}

<dl>
<dt>`Gateway`</dt>
<dd>The `bookinfo-gateway` [Gateway ![External link icon](../icons/launch-glyph.svg "External link icon")](https://istio.io/docs/reference/config/networking/v1alpha3/gateway/) describes a load balancer, the `istio-ingressgateway` service in the `istio-system` namespace that acts as the ingress entry point for HTTP/TCP traffic for BookInfo. Istio configures the load balancer to listen for incoming requests to Istio-managed apps on the ports that are defined in the gateway configuration file.
</br></br>To see the configuration file for the BookInfo gateway, run the following command.
<pre class="pre"><code>kubectl get gateway bookinfo-gateway -o yaml</code></pre></dd>

<dt>`VirtualService`</dt>
<dd>The `bookinfo` [`VirtualService` ![External link icon](../icons/launch-glyph.svg "External link icon")](https://istio.io/docs/reference/config/networking/v1alpha3/virtual-service/) defines the rules that control how requests are routed within the service mesh by defining microservices as `destinations`. In the `bookinfo` virtual service, the `/productpage` URI of a request is routed to the `productpage` host on port `9080`. In this way, all requests to the BookInfo app are routed first to the `productpage` microservice, which then calls the other microservices of BookInfo.
</br></br>To see the virtual service rule that is applied to BookInfo, run the following command.
<pre class="pre"><code>kubectl get virtualservice bookinfo -o yaml</code></pre></dd>

<dt>`DestinationRule`</dt>
<dd>After the gateway routes the request according to virtual service rule, the `details`, `productpage`, `ratings`, and `reviews` [`DestinationRules` ![External link icon](../icons/launch-glyph.svg "External link icon")](https://istio.io/docs/reference/config/networking/v1alpha3/destination-rule/) define policies that are applied to the request when it reaches a microservice. For example, when you refresh the BookInfo product page, the changes that you see are the result of the `productpage` microservice randomly calling different versions, `v1`, `v2`, and `v3`, of the `reviews` microservice. The versions are selected randomly because the `reviews` destination rule gives equal weight to the `subsets`, or the named versions, of the microservice. These subsets are used by the virtual service rules when traffic is routed to specific versions of the service.
</br></br>To see the destination rules that are applied to BookInfo, run the following command.
<pre class="pre"><code>kubectl describe destinationrules</code></pre></dd>
</dl>

</br>

<br />


## Logging, monitoring, tracing, and visualizing Istio
{: #istio_health}

To log, monitor, trace, and visualize your apps that are managed by Istio on {{site.data.keyword.containerlong_notm}}, you can launch the Grafana, Jaeger, and Kiali dashboards that are installed in the `istio-extras` add-on or deploy LogDNA and Sysdig as third-party services to your worker nodes.
{: shortdesc}

### Launching the Grafana, Jaeger, and Kiali dashboards
{: #istio_health_extras}

The Istio extras add-on (`istio-extras`) installs [Grafana ![External link icon](../icons/launch-glyph.svg "External link icon")](https://grafana.com/), [Jaeger ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.jaegertracing.io/), and [Kiali ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.kiali.io/). Launch the dashboards for each of these services to provide extra monitoring, tracing, and visualization for Istio.
{: shortdesc}

Before you begin, [install the `istio` and `istio-extras` managed add-ons](#istio_install) in a cluster.

**Grafana**</br>
1. Start Kubernetes port forwarding for the Grafana dashboard.
  ```
  kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=grafana -o jsonpath='{.items[0].metadata.name}') 3000:3000 &
  ```
  {: pre}

2. To open the Istio Grafana dashboard, go to the following URL: http://localhost:3000/dashboard/db/istio-mesh-dashboard. If you installed the [BookInfo add-on](#istio_bookinfo), the Istio dashboard shows metrics for the traffic that you generated when you refreshed the product page a few times. For more information about using the Istio Grafana dashboard, see [Viewing the Istio Dashboard ![External link icon](../icons/launch-glyph.svg "External link icon")](https://istio.io/docs/tasks/telemetry/using-istio-dashboard/) in the Istio open source documentation.

</br>
**Jaeger**</br>

1. By default, Istio generates trace spans for 1 out of every 100 requests, which is a sampling rate of 1%. You must send at least 100 requests before the first trace is visible. To send 100 requests to the `productpage` service of the [BookInfo add-on](#istio_bookinfo), run the following command.
  ```
  for i in `seq 1 100`; do curl -s -o /dev/null http://$GATEWAY_URL/productpage; done
  ```
  {: pre}

2. Start Kubernetes port forwarding for the Jaeger dashboard.
  ```
  kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=jaeger -o jsonpath='{.items[0].metadata.name}') 16686:16686 &
  ```
  {: pre}

3. To open the Jaeger UI, go to the following URL: http://localhost:16686.

4. If you installed the BookInfo add-on, you can select `productpage` from the **Service** list and click **Find Traces**. Traces for the traffic that you generated when you refreshed the product page a few times are shown. For more information about using Jaeger with Istio, see [Generating traces using the BookInfo sample ![External link icon](../icons/launch-glyph.svg "External link icon")](https://istio.io/docs/tasks/telemetry/distributed-tracing/#generating-traces-using-the-bookinfo-sample) in the Istio open source documentation.

</br>
**Kiali**</br>

1. Start Kubernetes port forwarding for the Kiali dashboard.
  ```
  kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=kiali -o jsonpath='{.items[0].metadata.name}') 20001:20001 &
  ```
  {: pre}

2. To open the Kiali UI, go to the following URL: http://localhost:20001/kiali/console.

3. Enter `admin` for both the user name and passphrase. For more information about using Kiali to visualize your Istio-managed microservices, see [Generating a service graph ![External link icon](../icons/launch-glyph.svg "External link icon")](https://archive.istio.io/v1.0/docs/tasks/telemetry/kiali/#generating-a-service-graph) in the Istio open source documentation.

### Setting up logging with {{site.data.keyword.la_full_notm}}
{: #istio_health_logdna}

Seamlessly manage logs for your app container and the Envoy proxy sidecar container in each pod by deploying LogDNA to your worker nodes to forward logs to {{site.data.keyword.loganalysislong}}.
{: shortdesc}

To use [{{site.data.keyword.la_full_notm}}](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-about), you deploy a logging agent to every worker node in your cluster. This agent collects logs with the extension `*.log` and extensionless files that are stored in the `/var/log` directory of your pod from all namespaces, including `kube-system`. These logs include logs from your app container and the Envoy proxy sidecar container in each pod. The agent then forwards the logs to the {{site.data.keyword.la_full_notm}} service.

To get started, set up LogDNA for your cluster by following the steps in [Managing Kubernetes cluster logs with {{site.data.keyword.la_full_notm}}](/docs/services/Log-Analysis-with-LogDNA/tutorials?topic=LogDNA-kube#kube).




### Setting up monitoring with {{site.data.keyword.mon_full_notm}}
{: #istio_health_sysdig}

Gain operational visibility into the performance and health of your Istio-managed apps by deploying Sysdig to your worker nodes to forward metrics to {{site.data.keyword.monitoringlong}}.
{: shortdesc}

With Istio on {{site.data.keyword.containerlong_notm}}, the managed `istio` add-on installs Prometheus into your cluster. The `istio-mixer-telemetry` pods in your cluster are annotated with a Prometheus endpoint so that Prometheus can aggregate all telemetry data for your pods. When you deploy a Sysdig agent to every worker node in your cluster, Sysdig is already automatically enabled to detect and scrape the data from these Prometheus endpoints to display them in your {{site.data.keyword.cloud_notm}} monitoring dashboard.

Since all of the Prometheus work is done, all that is left for you is to deploy Sysdig in your cluster.

1. Set up Sysdig by following the steps in [Analyzing metrics for an app that is deployed in a Kubernetes cluster](/docs/services/Monitoring-with-Sysdig/tutorials?topic=Sysdig-kubernetes_cluster#kubernetes_cluster).

2. [Launch the Sysdig UI ![External link icon](../icons/launch-glyph.svg "External link icon")](/docs/services/Monitoring-with-Sysdig/tutorials?topic=Sysdig-kubernetes_cluster#kubernetes_cluster_step3).

3. Click **Add new dashboard**.

4. Search for `Istio` and select one of Sysdig's predefined Istio dashboards.

For more information about referencing metrics and dashboards, monitoring Istio internal components, and monitoring Istio A/B deployments and canary deployments, check out the [How to monitor Istio, the Kubernetes service mesh ![External link icon](../icons/launch-glyph.svg "External link icon")](https://sysdig.com/blog/monitor-istio/). Look for the section called "Monitoring Istio: reference metrics and dashboards" blog post.

<br />


## Including apps in the Istio service mesh by setting up sidecar injection
{: #istio_sidecar}

Ready to manage your own apps by using Istio? Before you deploy your app, you must first decide how you want to inject the Envoy proxy sidecars into app pods.
{: shortdesc}

Each app pod must be running an Envoy proxy sidecar so that the microservices can be included in the service mesh. You can make sure that sidecars are injected into each app pod automatically or manually. For more information about sidecar injection, see the [Istio documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://istio.io/docs/setup/kubernetes/sidecar-injection/).

### Enabling automatic sidecar injection
{: #istio_sidecar_automatic}

When automatic sidecar injection is enabled, a namespace listens for any new deployments and automatically modifies the pod template specification so that app pods are created with Envoy proxy sidecar containers. Enable automatic sidecar injection for a namespace when you plan to deploy multiple apps that you want to integrate with Istio into that namespace. Automatic sidecar injection is not enabled for any namespaces by default in the Istio managed add-on.

To enable automatic sidecar injection for a namespace:

1. Get the name of the namespace where you want to deploy Istio-managed apps.
  ```
  kubectl get namespaces
  ```
  {: pre}

2. Label the namespace as `istio-injection=enabled`.
  ```
  kubectl label namespace <namespace> istio-injection=enabled
  ```
  {: pre}

3. Deploy apps into the labeled namespace, or redeploy apps that are already in the namespace.
  * To deploy an app into the labeled namespace:
    ```
    kubectl apply <myapp>.yaml --namespace <namespace>
    ```
    {: pre}
  * To redeploy an app that is already deployed in that namespace, delete the app pod so that it is redeployed with the injected sidecar.
    ```
    kubectl delete pod -l app=<myapp>
    ```
    {: pre}

5. If you did not create a service to expose your app, create a Kubernetes service. Your app must be exposed by a Kubernetes service to be included as a microservice in the Istio service mesh. Ensure that you follow the [Istio requirements for pods and services ![External link icon](../icons/launch-glyph.svg "External link icon")](https://istio.io/docs/setup/kubernetes/spec-requirements/).

  1. Define a service for the app.
    ```
    apiVersion: v1
    kind: Service
    metadata:
      name: myappservice
    spec:
      selector:
        <selector_key>: <selector_value>
      ports:
       - protocol: TCP
         port: 8080
    ```
    {: codeblock}

    <table>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the service YAML file components</th>
    </thead>
    <tbody>
    <tr>
    <td><code>selector</code></td>
    <td>Enter the label key (<em>&lt;selector_key&gt;</em>) and value (<em>&lt;selector_value&gt;</em>) pair that you want to use to target the pods where your app runs.</td>
     </tr>
     <tr>
     <td><code>port</code></td>
     <td>The port that the service listens on.</td>
     </tr>
     </tbody></table>

  2. Create the service in your cluster. Ensure that the service deploys into the same namespace as the app.
    ```
    kubectl apply -f myappservice.yaml -n <namespace>
    ```
    {: pre}

The app pods are now integrated into your Istio service mesh because they have the Istio sidecar container that runs alongside your app container.

### Manually injecting sidecars
{: #istio_sidecar_manual}

If you do not want to enable automatic sidecar injection for a namespace, you can manually inject the sidecar into a deployment YAML. Inject sidecars manually when apps are running in namespaces alongside other deployments that you do not want sidecars automatically injected into.

To manually inject sidecars into a deployment:

1. Download the `istioctl` client.
  ```
  curl -L https://git.io/getLatestIstio | ISTIO_VERSION=1.1.7 sh -
  ```

2. Navigate to the Istio package directory.
  ```
  cd istio-1.1.7
  ```
  {: pre}

3. Inject the Envoy sidecar into your app deployment YAML.
  ```
  istioctl kube-inject -f <myapp>.yaml | kubectl apply -f -
  ```
  {: pre}

4. Deploy your app.
  ```
  kubectl apply <myapp>.yaml
  ```
  {: pre}

5. If you did not create a service to expose your app, create a Kubernetes service. Your app must be exposed by a Kubernetes service to be included as a microservice in the Istio service mesh. Ensure that you follow the [Istio requirements for pods and services ![External link icon](../icons/launch-glyph.svg "External link icon")](https://istio.io/docs/setup/kubernetes/spec-requirements/).

  1. Define a service for the app.
    ```
    apiVersion: v1
    kind: Service
    metadata:
      name: myappservice
    spec:
      selector:
        <selector_key>: <selector_value>
      ports:
       - protocol: TCP
         port: 8080
    ```
    {: codeblock}

    <table>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the service YAML file components</th>
    </thead>
    <tbody>
    <tr>
    <td><code>selector</code></td>
    <td>Enter the label key (<em>&lt;selector_key&gt;</em>) and value (<em>&lt;selector_value&gt;</em>) pair that you want to use to target the pods where your app runs.</td>
     </tr>
     <tr>
     <td><code>port</code></td>
     <td>The port that the service listens on.</td>
     </tr>
     </tbody></table>

  2. Create the service in your cluster. Ensure that the service deploys into the same namespace as the app.
    ```
    kubectl apply -f myappservice.yaml -n <namespace>
    ```
    {: pre}

The app pods are now integrated into your Istio service mesh because they have the Istio sidecar container that runs alongside your app container.

<br />


## Exposing Istio-managed apps by using an IBM-provided host name
{: #istio_expose}

Publicly expose your Istio-managed apps by creating a DNS entry for the `istio-ingressgateway` load balancer and configuring the load balancer to forward traffic to your app.
{: shortdesc}

In the following steps, you set up a host name through which your users can access your app by creating the following resources:
* A gateway that is called `my-gateway`. This gateway acts as the public entry point to your apps and uses the existing `istio-ingressgateway` load balancer service to expose your app.
* A virtual service that is called `my-virtual-service`. `my-gateway` uses the rules that you define in `my-virtual-service` to route traffic to your app.
* A host name for the `istio-ingressgateway` load balancer. All user requests to the host name are forwarded to your app according to your `my-virtual-service` routing rules. For more information about registering DNS host names in {{site.data.keyword.containerlong_notm}}, including information about setting up custom health checks for host names, see [Registering an NLB host name](/docs/containers?topic=containers-loadbalancer_hostname).

**Before you begin:**
1. [Install the `istio` managed add-on](#istio_install) in a cluster.
2. Install the `istioctl` client.
  1. Download `istioctl`.
    ```
    curl -L https://git.io/getLatestIstio | sh -
    ```
  2. Navigate to the Istio package directory.
    ```
    cd istio-1.1.7
    ```
    {: pre}
3. [Set up sidecar injection for your app microservices, deploy the app microservices into a namespace, and create Kubernetes services for the app microservices so that they can be included in the Istio service mesh](#istio_sidecar).

</br>
**To publicly expose your Istio-managed apps with a host name:**

1. Create a gateway. This sample gateway uses the `istio-ingressgateway` load balancer service to expose port 80 for HTTP. Replace `<namespace>` with the namespace where your Istio-managed microservices are deployed. If your microservices listen on a different port than `80`, add that port. For more information about gateway YAML components, see the [Istio reference documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://istio.io/docs/reference/config/networking/v1alpha3/gateway/).
  ```
  apiVersion: networking.istio.io/v1alpha3
  kind: Gateway
  metadata:
    name: my-gateway
    namespace: <namespace>
  spec:
    selector:
      istio: ingressgateway
    servers:
    - port:
        name: http
        number: 80
        protocol: HTTP
      hosts:
      - '*'
  ```
  {: codeblock}

2. Apply the gateway in the namespace where your Istio-managed microservices are deployed.
  ```
  kubectl apply -f my-gateway.yaml -n <namespace>
  ```
  {: pre}

3. Create a virtual service that uses the `my-gateway` gateway and defines routing rules for your app microservices. For more information about virtual service YAML components, see the [Istio reference documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://istio.io/docs/reference/config/networking/v1alpha3/virtual-service/).
  ```
  apiVersion: networking.istio.io/v1alpha3
  kind: VirtualService
  metadata:
    name: my-virtual-service
    namespace: <namespace>
  spec:
    gateways:
    - my-gateway
    hosts:
    - '*'
    http:
    - match:
      - uri:
          exact: /<service_path>
      route:
      - destination:
          host: <service_name>
          port:
            number: 80
  ```
  {: codeblock}

  <table>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
  </thead>
  <tbody>
  <tr>
  <td><code>namespace</code></td>
  <td>Replace <em>&lt;namespace&gt;</em> with the namespace where your Istio-managed microservices are deployed.</td>
  </tr>
  <tr>
  <td><code>gateways</code></td>
  <td>The <code>my-gateway</code> is specified so that the gateway can apply these virtual service routing rules to the <code>istio-ingressgateway</code> load balancer.<td>
  </tr>
  <tr>
  <td><code>http.match.uri.exact</code></td>
  <td>Replace <em>&lt;service_path&gt;</em> with the path that your entrypoint microservice listens on. For example, in the BookInfo app, the path is defined as <code>/productpage</code>.</td>
  </tr>
  <tr>
  <td><code>http.route.destination.host</code></td>
  <td>Replace <em>&lt;service_name&gt;</em> with the name of your entrypoint microservice. For example, in the BookInfo app, <code>productpage</code> served as the entrypoint microservice that called the other app microservices.</td>
  </tr>
  <tr>
  <td><code>http.route.destination.port.number</code></td>
  <td>If your microservice listens on a different port, replace <em>&lt;80&gt;</em> with the port.</td>
  </tr>
  </tbody></table>

4. Apply the virtual service rules in the namespace where your Istio-managed microservice is deployed.
  ```
  kubectl apply -f my-virtual-service.yaml -n <namespace>
  ```
  {: pre}

5. Get the **EXTERNAL-IP** address for the `istio-ingressgateway` load balancer.
  ```
  kubectl get svc -n istio-system
  ```
  {: pre}

  In the following example output, the **EXTERNAL-IP** is `168.1.1.1`.
  ```
  NAME                     TYPE           CLUSTER-IP       EXTERNAL-IP                                                                    AGE
  ...
  istio-ingressgateway     LoadBalancer   172.21.XXX.XXX   169.1.1.1       80:31380/TCP,443:31390/TCP,31400:31400/TCP,5011:31323/TCP,
                                                                            8060:32483/TCP,853:32628/TCP,15030:31601/TCP,15031:31915/TCP  22m
  ```
  {: screen}

6. Register the `istio-ingressgateway` load balancer IP by creating a DNS host name.
  ```
  ibmcloud ks nlb-dns-create --cluster <cluster_name_or_id> --ip <LB_IP>
  ```
  {: pre}

7. Verify that the host name is created.
  ```
  ibmcloud ks nlb-dnss --cluster <cluster_name_or_id>
  ```
  {: pre}

  Example output:
  ```
  Hostname                                                                                IP(s)              Health Monitor   SSL Cert Status           SSL Cert Secret Name
  mycluster-a1b2cdef345678g9hi012j3kl4567890-0001.us-south.containers.appdomain.cloud     ["168.1.1.1"]      None             created                   <certificate>
  ```
  {: screen}

8. In a web browser, verify that traffic is routed to your Istio-managed microservices by entering the URL of the app microservice.
  ```
  http://<host_name>/<service_path>
  ```
  {: codeblock}

Looking for even more fine-grained control over routing? To create rules that are applied after the load balancer routes traffic to each microservice, such as rules for sending traffic to different versions of one microservice, you can create and apply [`DestinationRules` ![External link icon](../icons/launch-glyph.svg "External link icon")](https://istio.io/docs/reference/config/networking/v1alpha3/destination-rule/).
{: tip}

<br />


## Securing Istio-managed apps with {{site.data.keyword.appid_short_notm}}
{: #app-id}

By using the App Identity and Access adapter, you can centralize all of your identity management with a single instance of {{site.data.keyword.appid_full}}. The adapter can be configured to work with any OIDC compliant identity provider, which enables the adapter to control authentication and authorization policies in all environments including frontend and backend applications. To use this feature, you do not need to make changes to your code or redeploy your app. To get started, see [Securing multicloud apps with Istio](/docs/services/appid?topic=appid-istio-adapter) in the {{site.data.keyword.appid_short_notm}} documentation.

<br />


## Updating the Istio add-ons
{: #istio_update}

The Istio version in the managed Istio add-on is tested by {{site.data.keyword.cloud_notm}} and approved for the use in {{site.data.keyword.containerlong_notm}}. To update your Istio components to the most recent version of Istio supported by {{site.data.keyword.containerlong_notm}}, see [Updating managed add-ons](/docs/containers?topic=containers-managed-addons#updating-managed-add-ons).
{: shortdesc}

## Uninstalling Istio
{: #istio_uninstall}

If you're finished working with Istio, you can clean up the Istio resources in your cluster by uninstalling Istio add-ons.
{:shortdesc}

The `istio` add-on is a dependency for the `istio-extras`, `istio-sample-bookinfo`, and [`knative`](/docs/containers?topic=containers-serverless-apps-knative) add-ons. The `istio-extras` add-on is a dependency for the `istio-sample-bookinfo` add-on.
{: important}

**Optional**: Any resources that you created or modified in the `istio-system` namespace and all Kubernetes resources that were automatically generated by custom resource definitions (CRDs) are removed. If you want to keep these resources, save them before you uninstall the `istio` add-ons.
1. Save any resources, such as configuration files for any services or apps, that you created or modified in the `istio-system` namespace.
   Example command:
   ```
   kubectl get pod <pod_name> -o yaml -n istio-system
   ```
   {: pre}

2. Save the Kubernetes resources that were automatically generated by CRDs in `istio-system` to a YAML file on your local machine.
   1. Get the CRDs in `istio-system`.
      ```
      kubectl get crd -n istio-system
      ```
      {: pre}

   2. Save any resources created from these CRDs.

 ### Uninstalling managed Istio add-ons from the console
 {: #istio_uninstall_ui}

 1. In your [cluster dashboard ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/kubernetes/clusters), click the name of the cluster where you want to remove the Istio add-ons.

 2. Click the **Add-ons** tab.

 3. On the Managed Istio card, click the Action menu icon.

 4. Click **Uninstall**. All managed Istio add-ons are disabled in this cluster and all Istio resources in this cluster are removed.

 5. On the Managed Istio card, verify that the add-ons you uninstalled are no longer listed.

### Uninstalling managed Istio add-ons from the CLI
{: #istio_uninstall_cli}

1. Disable the `istio-sample-bookinfo` add-on.
  ```
  ibmcloud ks cluster-addon-disable istio-sample-bookinfo --cluster <cluster_name_or_ID>
  ```
  {: pre}

2. Disable the `istio-extras` add-on.
  ```
  ibmcloud ks cluster-addon-disable istio-extras --cluster <cluster_name_or_ID>
  ```
  {: pre}

3. Disable the `istio` add-on.
  ```
  ibmcloud ks cluster-addon-disable istio --cluster <cluster_name_or_ID> -f
  ```
  {: pre}

4. Verify that all managed Istio add-ons are disabled in this cluster. No Istio add-ons are returned in the output.
  ```
  ibmcloud ks cluster-addons --cluster <cluster_name_or_ID>
  ```
  {: pre}

<br />


### Uninstalling other Istio installations in your cluster
{: #istio_uninstall_other}

If you previously installed Istio in the cluster by using the IBM Helm chart or through another method, clean up that Istio installation before you enable the managed Istio add-ons in the cluster. To check whether Istio is already in a cluster, run `kubectl get namespaces` and look for the `istio-system` namespace in the output.
{: shortdesc}

- If you installed Istio by using the {{site.data.keyword.cloud_notm}} Istio Helm chart:
  1. Uninstall the Istio Helm deployment.
    ```
    helm del istio --purge
    ```
    {: pre}

  2. If you used Helm 2.9 or earlier, delete the extra job resource.
    ```
    kubectl -n istio-system delete job --all
    ```
    {: pre}

- If you installed Istio manually or used the Istio community Helm chart, see the [Istio uninstall documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://istio.io/docs/setup/kubernetes/quick-start/#uninstall-istio-core-components).
* If you previously installed BookInfo in the cluster, clean up those resources.
  1. Change the directory to the Istio file location.
    ```
    cd <filepath>/istio-1.1.7
    ```
    {: pre}

  2. Delete all BookInfo services, pods, and deployments in the cluster.
    ```
    samples/bookinfo/platform/kube/cleanup.sh
    ```
    {: pre}

<br />


## What's next?
{: #istio_next}

* To explore Istio further, you can find more guides in the [Istio documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://istio.io/).
* Take the [Cognitive Class: Getting started with Microservices with Istio and IBM Cloud Kubernetes Service ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cognitiveclass.ai/courses/get-started-with-microservices-istio-and-ibm-cloud-container-service/). **Note**: You can skip the Istio installation section of this course.
* Check out this blog post on using [Vistio ![External link icon](../icons/launch-glyph.svg "External link icon")](https://itnext.io/vistio-visualize-your-istio-mesh-using-netflixs-vizceral-b075c402e18e) to visualize your Istio service mesh.
