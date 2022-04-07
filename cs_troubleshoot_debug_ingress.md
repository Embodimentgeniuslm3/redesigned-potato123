---

copyright:
  years: 2014, 2019
lastupdated: "2019-07-09"

keywords: kubernetes, iks, nginx, ingress controller

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



# Debugging Ingress
{: #cs_troubleshoot_debug_ingress}

As you use {{site.data.keyword.containerlong}}, consider these techniques for general Ingress troubleshooting and debugging.
{: shortdesc}

You publicly exposed your app by creating an Ingress resource for your app in your cluster. However, when you try to connect to your app through the ALB's public IP address or subdomain, the connection fails or times out. The steps in the following sections can help you debug your Ingress setup.

Ensure that you define a host in only one Ingress resource. If one host is defined in multiple Ingress resources, the ALB might not forward traffic properly and you might experience errors.
{: tip}

Before you begin, ensure you have the following [{{site.data.keyword.cloud_notm}} IAM access policies](/docs/containers?topic=containers-users#platform):
  - **Editor** or **Administrator** platform role for the cluster
  - **Writer** or **Manager** service role

## Step 1: Run Ingress tests in the {{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool

While you troubleshoot, you can use the {{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool to run Ingress tests and gather pertinent Ingress information from your cluster. To use the debug tool, install the [`ibmcloud-iks-debug` Helm chart ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/kubernetes/helm/iks-charts/ibmcloud-iks-debug):
{: shortdesc}


1. [Set up Helm in your cluster, create a service account for Tiller, and add the `ibm` repository to your Helm instance](/docs/containers?topic=containers-helm).

2. Install the Helm chart to your cluster.
  ```
  helm install iks-charts/ibmcloud-iks-debug --name debug-tool
  ```
  {: pre}


3. Start a proxy server to display the debug tool interface.
  ```
  kubectl proxy --port 8080
  ```
  {: pre}

4. In a web browser, open the debug tool interface URL: http://localhost:8080/api/v1/namespaces/default/services/debug-tool-ibmcloud-iks-debug:8822/proxy/page

5. Select the **ingress** group of tests. Some tests check for potential warnings, errors, or issues, and some tests only gather information that you can reference while you troubleshoot. For more information about the function of each test, click the information icon next to the test's name.

6. Click **Run**.

7. Check the results of each test.
  * If any test fails, click the information icon next to the test's name in the left-hand column for information about how to resolve the issue.
  * You can also use the results of tests that only gather information while you debug your Ingress service in the following sections.

## Step 2: Check for error messages in your Ingress deployment and the ALB pod logs
{: #errors}

Start by checking for error messages in the Ingress resource deployment events and ALB pod logs. These error messages can help you find the root causes for failures and further debug your Ingress setup in the next sections.
{: shortdesc}

1. Check your Ingress resource deployment and look for warning or error messages.
    ```
    kubectl describe ingress <myingress>
    ```
    {: pre}

    In the **Events** section of the output, you might see warning messages about invalid values in your Ingress resource or in certain annotations that you used. Check the [Ingress resource configuration documentation](/docs/containers?topic=containers-ingress#public_inside_4) or the [annotations documentation](/docs/containers?topic=containers-ingress_annotation).

    ```
    Name:             myingress
    Namespace:        default
    Address:          169.xx.xxx.xxx,169.xx.xxx.xxx
    Default backend:  default-http-backend:80 (<none>)
    Rules:
      Host                                             Path  Backends
      ----                                             ----  --------
      mycluster.us-south.containers.appdomain.cloud
                                                       /tea      myservice1:80 (<none>)
                                                       /coffee   myservice2:80 (<none>)
    Annotations:
      custom-port:        protocol=http port=7490; protocol=https port=4431
      location-modifier:  modifier='~' serviceName=myservice1;modifier='^~' serviceName=myservice2
    Events:
      Type     Reason             Age   From                                                            Message
      ----     ------             ----  ----                                                            -------
      Normal   Success            1m    public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Successfully applied ingress resource.
      Warning  TLSSecretNotFound  1m    public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Failed to apply ingress resource.
      Normal   Success            59s   public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Successfully applied ingress resource.
      Warning  AnnotationError    40s   public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Failed to apply ingress.bluemix.net/custom-port annotation. Error annotation format error : One of the mandatory fields not valid/missing for annotation ingress.bluemix.net/custom-port
      Normal   Success            40s   public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Successfully applied ingress resource.
      Warning  AnnotationError    2s    public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Failed to apply ingress.bluemix.net/custom-port annotation. Invalid port 7490. Annotation cannot use ports 7481 - 7490
      Normal   Success            2s    public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Successfully applied ingress resource.
    ```
    {: screen}
{: #check_pods}
2. Check the status of your ALB pods.
    1. Get the ALB pods that are running in your cluster.
        ```
        kubectl get pods -n kube-system | grep alb
        ```
        {: pre}

    2. Make sure that all pods are running by checking the **STATUS** column.

    3. If a pod is not `Running`, you can disable and re-enable the ALB. In the following commands, replace `<ALB_ID>` with the ID of the pod's ALB. For example, if the pod that is not running has the name `public-crb2f60e9735254ac8b20b9c1e38b649a5-alb1-5d6d86fbbc-kxj6z`, the ALB ID is `public-crb2f60e9735254ac8b20b9c1e38b649a5-alb1`.
        ```
        ibmcloud ks alb-configure --albID <ALB_ID> --disable
        ```
        {: pre}

        ```
        ibmcloud ks alb-configure --albID <ALB_ID> --enable
        ```
        {: pre}

3. Check the logs for your ALB.
    1.  Get the IDs of the ALB pods that are running in your cluster.
        ```
        kubectl get pods -n kube-system | grep alb
        ```
        {: pre}

    3. Get the logs for the `nginx-ingress` container on each ALB pod.
        ```
        kubectl logs <ingress_pod_ID> nginx-ingress -n kube-system
        ```
        {: pre}

    4. Look for error messages in the ALB logs.

## Step 3: Ping the ALB subdomain and public IP addresses
{: #ping}

Check the availability of your Ingress subdomain and ALBs' public IP addresses.
{: shortdesc}

1. Get the IP addresses that your public ALBs are listening on.
    ```
    ibmcloud ks albs --cluster <cluster_name_or_ID>
    ```
    {: pre}

    Example output for a multizone cluster with worker nodes in `dal10` and `dal13`:

    ```
    ALB ID                                            Enabled   Status     Type      ALB IP          Zone    Build                          ALB VLAN ID
    private-cr24a9f2caf6554648836337d240064935-alb1   false     disabled   private   -               dal13   ingress:411/ingress-auth:315   2294021
    private-cr24a9f2caf6554648836337d240064935-alb2   false     disabled   private   -               dal10   ingress:411/ingress-auth:315   2234947
    public-cr24a9f2caf6554648836337d240064935-alb1    true      enabled    public    169.62.196.238  dal13   ingress:411/ingress-auth:315   2294019
    public-cr24a9f2caf6554648836337d240064935-alb2    true      enabled    public    169.46.52.222   dal10   ingress:411/ingress-auth:315   2234945
    ```
    {: screen}

    * If a public ALB has no IP address, see [Ingress ALB does not deploy in a zone](/docs/containers?topic=containers-cs_troubleshoot_network#cs_subnet_limit).

2. Check the health of your ALB IPs.

    * For single zone cluster and multizone clusters: Ping the IP address of each public ALB to ensure that each ALB is able to successfully receive packets. If you are using private ALBs, you can ping their IP addresses only from the private network.
        ```
        ping <ALB_IP>
        ```
        {: pre}

        * If the CLI returns a timeout and you have a custom firewall that is protecting your worker nodes, make sure that you allow ICMP in your [firewall](/docs/containers?topic=containers-cs_troubleshoot_clusters#cs_firewall).
        * If there is no firewall that is blocking the pings and the pings still run to timeout, [check the status of your ALB pods](#check_pods).

    * Multizone clusters only: You can use the MZLB health check to determine the status of your ALB IPs. For more information about the MZLB, see [Multizone load balancer (MZLB)](/docs/containers?topic=containers-ingress-about#ingress_components). The MZLB health check is available only for clusters that have the new Ingress subdomain in the format `<cluster_name>.<region_or_zone>.containers.appdomain.cloud`. If your cluster still uses the older format of `<cluster_name>.<region>.containers.mybluemix.net`, [convert your single zone cluster to multizone](/docs/containers?topic=containers-add_workers#add_zone). Your cluster is assigned a subdomain with the new format, but can also continue to use the older subdomain format. Alternatively, you can order a new cluster that is automatically assigned the new subdomain format.

    The following HTTP cURL command uses the `albhealth` host, which is configured by {{site.data.keyword.containerlong_notm}} to return the `healthy` or `unhealthy` status for an ALB IP.
        ```
        curl -X GET http://169.62.196.238/ -H "Host: albhealth.mycluster-12345.us-south.containers.appdomain.cloud"
        ```
        {: pre}

        Example output:
        ```
        healthy
        ```
        {: screen}
        If one or more of the IPs returns `unhealthy`, [check the status of your ALB pods](#check_pods).

3. Get the IBM-provided Ingress subdomain.
    ```
    ibmcloud ks cluster-get --cluster <cluster_name_or_ID> | grep Ingress
    ```
    {: pre}

    Example output:
    ```
    Ingress Subdomain:      mycluster-12345.us-south.containers.appdomain.cloud
    Ingress Secret:         <tls_secret>
    ```
    {: screen}

4. Ensure that the IPs for each public ALB that you got in step 2 of this section are registered with your cluster's IBM-provided Ingress subdomain. For example, in a multizone cluster, the public ALB IP in each zone where you have worker nodes must be registered under the same host name.

    ```
    kubectl get ingress -o wide
    ```
    {: pre}

    Example output:
    ```
    NAME                HOSTS                                                    ADDRESS                        PORTS     AGE
    myingressresource   mycluster-12345.us-south.containers.appdomain.cloud      169.46.52.222,169.62.196.238   80        1h
    ```
    {: screen}

## Step 4: Check your domain mappings and Ingress resource configuration
{: #ts_ingress_config}

1. If you use a custom domain, verify that you used your DNS provider to map the custom domain to the IBM-provided subdomain or the ALB's public IP address. Note that using a CNAME is preferred because IBM provides automatic health checks on the IBM subdomain and removes any failing IPs from the DNS response.
    * IBM-provided subdomain: Check that your custom domain is mapped to the cluster's IBM-provided subdomain in the Canonical Name record (CNAME).
        ```
        host www.my-domain.com
        ```
        {: pre}

        Example output:
        ```
        www.my-domain.com is an alias for mycluster-12345.us-south.containers.appdomain.cloud
        mycluster-12345.us-south.containers.appdomain.cloud has address 169.46.52.222
        mycluster-12345.us-south.containers.appdomain.cloud has address 169.62.196.238
        ```
        {: screen}

    * Public IP address: Check that your custom domain is mapped to the ALB's portable public IP address in the A record. The IPs should match the public ALB IPs that you got in step 1 of the [previous section](#ping).
        ```
        host www.my-domain.com
        ```
        {: pre}

        Example output:
        ```
        www.my-domain.com has address 169.46.52.222
        www.my-domain.com has address 169.62.196.238
        ```
        {: screen}

2. Check the Ingress resource configuration files for your cluster.
    ```
    kubectl get ingress -o yaml
    ```
    {: pre}

    1. Ensure that you define a host in only one Ingress resource. If one host is defined in multiple Ingress resources, the ALB might not forward traffic properly and you might experience errors.

    2. Check that the subdomain and TLS certificate are correct. To find the IBM provided Ingress subdomain and TLS certificate, run `ibmcloud ks cluster-get --cluster <cluster_name_or_ID>`.

    3.  Make sure that your app listens on the same path that is configured in the **path** section of your Ingress. If your app is set up to listen on the root path, use `/` as the path. If incoming traffic to this path must be routed to a different path that your app listens on, use the [rewrite paths](/docs/containers?topic=containers-ingress_annotation#rewrite-path) annotation.

    4. Edit your resource configuration YAML as needed. When you close the editor, your changes are saved and automatically applied.
        ```
        kubectl edit ingress <myingressresource>
        ```
        {: pre}

## Removing an ALB from DNS for debugging
{: #one_alb}

If you can't access your app through a specific ALB IP, you can temporarily remove the ALB from production by disabling its DNS registration. Then, you can use the ALB's IP address to run debugging tests on that ALB.

For example, say you have a multizone cluster in 2 zones, and the 2 public ALBs have IP addresses `169.46.52.222` and `169.62.196.238`. Although the health check is returning healthy for the second zone's ALB, your app isn't directly reachable through it. You decide to remove that ALB's IP address, `169.62.196.238`, from production for debugging. The first zone's ALB IP, `169.46.52.222`, is registered with your domain and continues to route traffic while you debug the second zone's ALB.

1. Get the name of the ALB with the unreachable IP address.
    ```
    ibmcloud ks albs --cluster <cluster_name> | grep <ALB_IP>
    ```
    {: pre}

    For example, the unreachable IP `169.62.196.238` belongs to the ALB `public-cr24a9f2caf6554648836337d240064935-alb1`:
    ```
    ALB ID                                            Enabled   Status     Type      ALB IP           Zone    Build                          ALB VLAN ID
    public-cr24a9f2caf6554648836337d240064935-alb1    false     disabled   private   169.62.196.238   dal13   ingress:411/ingress-auth:315   2294021
    ```
    {: screen}

2. Using the ALB name from the previous step, get the names of the ALB pods. The following command uses the example ALB name from the previous step:
    ```
    kubectl get pods -n kube-system | grep public-cr24a9f2caf6554648836337d240064935-alb1
    ```
    {: pre}

    Example output:
    ```
    public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq   2/2       Running   0          24m
    public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-trqxc   2/2       Running   0          24m
    ```
    {: screen}

3. Disable the health check that runs for all ALB pods. Repeat these steps for each ALB pod that you got in the previous step. The example commands and output in these steps use the first pod, `public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq`.
    1. Log in to the ALB pod and check the `server_name` line in the NGINX configuration file.
        ```
        kubectl exec -ti public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq -n kube-system -c nginx-ingress -- grep server_name /etc/nginx/conf.d/kube-system-alb-health.conf
        ```
        {: pre}

        Example output that confirms the ALB pod is configured with the correct health check host name, `albhealth.<domain>`:
        ```
        server_name albhealth.mycluster-12345.us-south.containers.appdomain.cloud;
        ```
        {: screen}

    2. To remove the IP by disabling the health check, insert `#` in front of the `server_name`. When the `albhealth.mycluster-12345.us-south.containers.appdomain.cloud` virtual host is disabled for the ALB, the automated health check automatically removes the IP from the DNS response.
        ```
        kubectl exec -ti public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq -n kube-system -c nginx-ingress -- sed -i -e 's*server_name*#server_name*g' /etc/nginx/conf.d/kube-system-alb-health.conf
        ```
        {: pre}

    3. Verify that the change was applied.
        ```
        kubectl exec -ti public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq -n kube-system -c nginx-ingress -- grep server_name /etc/nginx/conf.d/kube-system-alb-health.conf
        ```
        {: pre}

        Example output:
        ```
        #server_name albhealth.mycluster-12345.us-south.containers.appdomain.cloud
        ```
        {: screen}

    4. To remove the IP from the DNS registration, reload the NGINX configuration.
        ```
        kubectl exec -ti public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq -n kube-system -c nginx-ingress -- nginx -s reload
        ```
        {: pre}

    5. Repeat these steps for each ALB pod.

4. Now, when you attempt to cURL the `albhealth` host to health check the ALB IP, the check fails.
    ```
    curl -X GET http://169.62.196.238/ -H "Host: albhealth.mycluster-12345.us-south.containers.appdomain.cloud"
    ```
    {: pre}

    Output:
    ```
    <html>
        <head>
            <title>404 Not Found</title>
        </head>
        <body bgcolor="white"><center>
            <h1>404 Not Found</h1>
        </body>
    </html>
    ```
    {: screen}

5. Verify that the ALB IP address is removed from the DNS registration for your domain by checking the Cloudflare server. Note that the DNS registration might take a few minutes to update.
    ```
    host mycluster-12345.us-south.containers.appdomain.cloud ada.ns.cloudflare.com
    ```
    {: pre}

    Example output that confirms that only the healthy ALB IP, `169.46.52.222`, remains in the DNS registration and that the unhealthy ALB IP, `169.62.196.238`, has been removed:
    ```
    mycluster-12345.us-south.containers.appdomain.cloud has address 169.46.52.222
    ```
    {: screen}

6. Now that the ALB IP has been removed from production, you can run debugging tests against your app through it. To test communication to your app through this IP, you can run the following cURL command, replacing the example values with your own values:
    ```
    curl -X GET --resolve mycluster-12345.us-south.containers.appdomain.cloud:443:169.62.196.238 https://mycluster-12345.us-south.containers.appdomain.cloud/
    ```
    {: pre}

    * If everything is configured correctly, you get back the expected response from your app.
    * If you get an error in response, there might be an error in your app or in a configuration that applies only to this specific ALB. Check your app code, your [Ingress resource configuration files](/docs/containers?topic=containers-ingress#public_inside_4), or any other configurations you have applied to only this ALB.

7. After you finish debugging, restore the health check on the ALB pods. Repeat these steps for each ALB pod.
  1. Log in to the ALB pod and remove the `#` from the `server_name`.
    ```
    kubectl exec -ti <pod_name> -n kube-system -c nginx-ingress -- sed -i -e 's*#server_name*server_name*g' /etc/nginx/conf.d/kube-system-alb-health.conf
    ```
    {: pre}

  2. Reload the NGINX configuration so that the health check restoration is applied.
    ```
    kubectl exec -ti <pod_name> -n kube-system -c nginx-ingress -- nginx -s reload
    ```
    {: pre}

9. Now, when you cURL the `albhealth` host to health check the ALB IP, the check returns `healthy`.
    ```
    curl -X GET http://169.62.196.238/ -H "Host: albhealth.mycluster-12345.us-south.containers.appdomain.cloud"
    ```
    {: pre}

10. Verify that the ALB IP address has been restored in the DNS registration for your domain by checking the Cloudflare server. Note that the DNS registration might take a few minutes to update.
    ```
    host mycluster-12345.us-south.containers.appdomain.cloud ada.ns.cloudflare.com
    ```
    {: pre}

    Example output:
    ```
    mycluster-12345.us-south.containers.appdomain.cloud has address 169.46.52.222
    mycluster-12345.us-south.containers.appdomain.cloud has address 169.62.196.238
    ```
    {: screen}

<br />


## Getting help and support
{: #ingress_getting_help}

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

