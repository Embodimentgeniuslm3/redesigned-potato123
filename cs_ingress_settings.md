---

copyright:
  years: 2014, 2019
lastupdated: "2019-07-22"

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



# Modifying default Ingress behavior
{: #ingress-settings}

After you expose your apps by creating an Ingress resource, you can further configure the Ingress ALBs in your cluster by setting the following options.
{: shortdesc}

## Opening ports in the Ingress ALB
{: #opening_ingress_ports}

By default, only ports 80 and 443 are exposed in the Ingress ALB. To expose other ports, you can edit the `ibm-cloud-provider-ingress-cm` configmap resource.
{: shortdesc}

1. Edit the configuration file for the `ibm-cloud-provider-ingress-cm` configmap resource.
    ```
    kubectl edit cm ibm-cloud-provider-ingress-cm -n kube-system
    ```
    {: pre}

2. Add a <code>data</code> section and specify public ports `80`, `443`, and any other ports you want to expose separated by a semi-colon (;).

    By default, ports 80 and 443 are open. If you want to keep 80 and 443 open, you must also include them in addition to any other ports you specify in the `public-ports` field. Any port that is not specified is closed. If you enabled a private ALB, you must also specify any ports that you want to keep open in the `private-ports` field.
    {: important}

    ```
    apiVersion: v1
    data:
      public-ports: "80;443;<port3>"
      private-ports: "80;443;<port4>"
    kind: ConfigMap
    metadata:
      name: ibm-cloud-provider-ingress-cm
      namespace: kube-system
    ```
    {: codeblock}

    Example that keeps ports `80`, `443`, and `9443` open:
    ```
    apiVersion: v1
    data:
      public-ports: "80;443;9443"
    kind: ConfigMap
    metadata:
      name: ibm-cloud-provider-ingress-cm
      namespace: kube-system
    ```
    {: screen}

3. Save the configuration file.

4. Verify that the configmap changes were applied.
  ```
  kubectl get cm ibm-cloud-provider-ingress-cm -n kube-system -o yaml
  ```
  {: pre}

5. Optional:
  * Access an app via a non-standard TCP port that you opened by using the [`tcp-ports`](/docs/containers?topic=containers-ingress_annotation#tcp-ports) annotation.
  * Change the default ports for HTTP (port 80) and HTTPS (port 443) network traffic to a port that you opened by using the [`custom-port`](/docs/containers?topic=containers-ingress_annotation#custom-port) annotation.

For more information about configmap resources, see the [Kubernetes documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/).

<br />


## Preserving the source IP address
{: #preserve_source_ip}

By default, the source IP address of the client request is not preserved. When a client request to your app is sent to your cluster, the request is routed to a pod for the load balancer service that exposes the ALB. If no app pod exists on the same worker node as the load balancer service pod, the load balancer forwards the request to an app pod on a different worker node. The source IP address of the package is changed to the public IP address of the worker node where the app pod is running.
{: shortdesc}

To preserve the original source IP address of the client request, you can enable [source IP preservation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/tutorials/services/source-ip/#source-ip-for-services-with-typeloadbalancer). Preserving the client’s IP is useful, for example, when app servers have to apply security and access-control policies.

If you [disable an ALB](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_configure), any source IP changes you make to the load balancer service that exposes the ALB are lost. When you re-enable the ALB, you must enable source IP again.
{: note}

To enable source IP preservation, edit the load balancer service that exposes an Ingress ALB:

1. Enable source IP preservation for a single ALB or for all the ALBs in your cluster.
    * To set up source IP preservation for a single ALB:
        1. Get the ID of the ALB for which you want to enable source IP. The ALB services have a format similar to `public-cr18e61e63c6e94b658596ca93d087eed9-alb1` for a public ALB or `private-cr18e61e63c6e94b658596ca93d087eed9-alb1` for a private ALB.
            ```
            kubectl get svc -n kube-system | grep alb
            ```
            {: pre}

        2. Open the YAML for the load balancer service that exposes the ALB.
            ```
            kubectl edit svc <ALB_ID> -n kube-system
            ```
            {: pre}

        3. Under **`spec`**, change the value of **`externalTrafficPolicy`** from `Cluster` to `Local`.

        4. Save and close the configuration file. The output is similar to the following:

            ```
            service "public-cr18e61e63c6e94b658596ca93d087eed9-alb1" edited
            ```
            {: screen}
    * To set up source IP preservation for all public ALBs in your cluster, run the following command:
        ```
        kubectl get svc -n kube-system | grep alb | awk '{print $1}' | grep "^public" | while read alb; do kubectl patch svc $alb -n kube-system -p '{"spec":{"externalTrafficPolicy":"Local"}}'; done
        ```
        {: pre}

        Example output:
        ```
        "public-cr18e61e63c6e94b658596ca93d087eed9-alb1", "public-cr17e61e63c6e94b658596ca92d087eed9-alb2" patched
        ```
        {: screen}

    * To set up source IP preservation for all private ALBs in your cluster, run the following command:
        ```
        kubectl get svc -n kube-system | grep alb | awk '{print $1}' | grep "^private" | while read alb; do kubectl patch svc $alb -n kube-system -p '{"spec":{"externalTrafficPolicy":"Local"}}'; done
        ```
        {: pre}

        Example output:
        ```
        "private-cr18e61e63c6e94b658596ca93d087eed9-alb1", "private-cr17e61e63c6e94b658596ca92d087eed9-alb2" patched
        ```
        {: screen}

2. Verify that the source IP is being preserved in your ALB pods logs.
    1. Get the ID of a pod for the ALB that you modified.
        ```
        kubectl get pods -n kube-system | grep alb
        ```
        {: pre}

    2. Open the logs for that ALB pod. Verify that the IP address for the `client` field is the client request IP address instead of the load balancer service IP address.
        ```
        kubectl logs <ALB_pod_ID> nginx-ingress -n kube-system
        ```
        {: pre}

3. Now, when you look up the headers for the requests that are sent to your back-end app, you can see the client IP address in the `x-forwarded-for` header.

4. If you no longer want to preserve the source IP, you can revert the changes that you made to the service.
    * To revert source IP preservation for your public ALBs:
        ```
        kubectl get svc -n kube-system | grep alb | awk '{print $1}' | grep "^public" | while read alb; do kubectl patch svc $alb -n kube-system -p '{"spec":{"externalTrafficPolicy":"Cluster"}}'; done
        ```
        {: pre}
    * To revert source IP preservation for your private ALBs:
        ```
        kubectl get svc -n kube-system | grep alb | awk '{print $1}' | grep "^private" | while read alb; do kubectl patch svc $alb -n kube-system -p '{"spec":{"externalTrafficPolicy":"Cluster"}}'; done
        ```
        {: pre}

<br />


## Configuring SSL protocols and SSL ciphers at the HTTP level
{: #ssl_protocols_ciphers}

Enable SSL protocols and ciphers at the global HTTP level by editing the `ibm-cloud-provider-ingress-cm` configmap.
{:shortdesc}

To comply with the PCI Security Standards Council mandate, the Ingress service disables TLS 1.0 and 1.1 by default with the upcoming version update of the Ingress ALB pods on 23 January 2019. The update rolls out automatically to all {{site.data.keyword.containerlong_notm}} clusters that have not opted out from automatic ALB updates. If the clients that connect to your apps support TLS 1.2, no action is required. If you still have legacy clients that require TLS 1.0 or 1.1 support, you must manually enable the required TLS versions. You can override the default setting to use TLS 1.1 or 1.0 protocols by following the steps in this section. For more information about how to see the TLS versions that your clients use to access your apps, see this [{{site.data.keyword.cloud_notm}} blog post](https://www.ibm.com/blogs/bluemix/2018/11/ibm-cloud-kubernetes-service-alb-update-tls-1-0-and-1-1-disabled-by-default/).
{: important}

When you specify the enabled protocols for all hosts, the TLSv1.1 and TLSv1.2 parameters (1.1.13, 1.0.12) work only when OpenSSL 1.0.1 or higher is used. The TLSv1.3 parameter (1.13.0) works only when OpenSSL 1.1.1 built with TLSv1.3 support is used.
{: note}

To edit the configmap to enable SSL protocols and ciphers:

1. Edit the configuration file for the `ibm-cloud-provider-ingress-cm` configmap resource.

    ```
    kubectl edit cm ibm-cloud-provider-ingress-cm -n kube-system
    ```
    {: pre}

2. Add the SSL protocols and ciphers. Format ciphers according to the [OpenSSL library cipher list format ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.openssl.org/docs/man1.0.2/apps/ciphers.html).

   ```
   apiVersion: v1
   data:
     ssl-protocols: "TLSv1 TLSv1.1 TLSv1.2"
     ssl-ciphers: "HIGH:!aNULL:!MD5"
   kind: ConfigMap
   metadata:
     name: ibm-cloud-provider-ingress-cm
     namespace: kube-system
   ```
   {: codeblock}

3. Save the configuration file.

4. Verify that the configmap changes were applied.

   ```
   kubectl get cm ibm-cloud-provider-ingress-cm -n kube-system -o yaml
   ```
   {: pre}

<br />




## Tuning ALB performance
{: #perf_tuning}

To optimize performance of your Ingress ALBs, you can change the default settings according to your needs.
{: shortdesc}

### Adding ALB socket listeners to every worker node
{: #reuse-port}

Increase the number of ALB socket listeners from one per cluster to one per worker node by using the `reuse-port` Ingress directive.
{: shortdesc}

When the `reuse-port` option is disabled, a single listening socket notifies workers about incoming connections and all worker nodes attempt to take the connection. But when `reuse-port` is enabled, one socket listener exists per worker node for each ALB IP address and port combination. Instead of each worker node attempting to take the connection, the Linux kernel determines which available socket listener gets the connection. Lock contention between workers is reduced, which can improve performance. For more information about the benefits and drawbacks of the `reuse-port` directive, see [this NGINX blog post ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.nginx.com/blog/socket-sharding-nginx-release-1-9-1/).

You can scale the listeners by editing the `ibm-cloud-provider-ingress-cm` Ingress configmap.

1. Edit the configuration file for the `ibm-cloud-provider-ingress-cm` configmap resource.
    ```
    kubectl edit cm ibm-cloud-provider-ingress-cm -n kube-system
    ```
    {: pre}

2. In the `metadata` section, add `reuse-port: "true"`. Example:
   ```
   apiVersion: v1
   data:
     private-ports: 80;443;9443
     public-ports: 80;443
   kind: ConfigMap
   metadata:
     creationTimestamp: "2018-09-28T15:53:59Z"
     name: ibm-cloud-provider-ingress-cm
     namespace: kube-system
     resourceVersion: "24648820"
     selfLink: /api/v1/namespaces/kube-system/configmaps/ibm-cloud-provider-ingress-cm
     uid: b6ca0c36-c336-11e8-bf8c-bee252897df5
     reuse-port: "true"
   ```
   {: codeblock}

3. Save the configuration file.

4. Verify that the configmap changes were applied.

   ```
   kubectl get cm ibm-cloud-provider-ingress-cm -n kube-system -o yaml
   ```
   {: pre}

### Enabling log buffering and flush timeout
{: #access-log}

By default, the Ingress ALB logs each request as it arrives. If you have an environment that is heavily used, logging each request as it arrives can greatly increase disk I/O utilization. To avoid continuous disk I/O, you can enable log buffering and flush timeout for the ALB by editing the `ibm-cloud-provider-ingress-cm` Ingress configmap. When buffering is enabled, instead of performing a separate write operation for each log entry, the ALB buffers a series of entries and writes them to the file together in a single operation.
{: shortdesc}

1. Create and Edit the configuration file for the `ibm-cloud-provider-ingress-cm` configmap resource.

    ```
    kubectl edit cm ibm-cloud-provider-ingress-cm -n kube-system
    ```
    {: pre}

2. Edit the configmap.
    1. Enable log buffering by adding the `access-log-buffering` field and setting it to `"true"`.

    2. Set the threshold for when the ALB should write buffer contents to the log.
        * Time interval: Add the `flush-interval` field and set it to how often the ALB should write to the log. For example, if the default value of `5m` is used, the ALB writes buffer contents to the log once every 5 minutes.
        * Buffer size: Add the `buffer-size` field and set it to how much log memory can be held in the buffer before the ALB writes the buffer contents to the log. For example, if the default value of `100KB` is used, the ALB writes buffer contents to the log every time the buffer reaches 100kb of log content.
        * Time interval or buffer size: When both `flush-interval` and `buffer-size` are set, the ALB writes buffer content to the log based on whichever threshold parameter is met first.

    ```
    apiVersion: v1
    kind: ConfigMap
    data:
      access-log-buffering: "true"
      flush-interval: "5m"
      buffer-size: "100KB"
    metadata:
      name: ibm-cloud-provider-ingress-cm
      ...
    ```
   {: codeblock}

3. Save the configuration file.

4. Verify that your ALB is configured with the access log changes.

   ```
   kubectl logs -n kube-system <ALB_ID> -c nginx-ingress
   ```
   {: pre}

### Changing the number or duration of keepalive connections
{: #keepalive_time}

Keepalive connections can have a major impact on performance by reducing the CPU and network usage that is needed to open and close connections. To optimize the performance of your ALBs, you can change the maximum number of keepalive connections between the ALB and the client and how long the keepalive connections can last.
{: shortdesc}

1. Edit the configuration file for the `ibm-cloud-provider-ingress-cm` configmap resource.

    ```
    kubectl edit cm ibm-cloud-provider-ingress-cm -n kube-system
    ```
    {: pre}

2. Change the values of `keep-alive-requests` and `keep-alive`.
    * `keep-alive-requests`: The number of keepalive client connections that can stay open to the Ingress ALB. The default is `4096`.
    * `keep-alive`: The timeout, in seconds, during which the keepalive client connection stays open to the Ingress ALB. The default is `8s`.
   ```
   apiVersion: v1
   data:
     keep-alive-requests: "4096"
     keep-alive: "8s"
   kind: ConfigMap
   metadata:
     name: ibm-cloud-provider-ingress-cm
     namespace: kube-system
   ```
   {: codeblock}

3. Save the configuration file.

4. Verify that the configmap changes were applied.

   ```
   kubectl get cm ibm-cloud-provider-ingress-cm -n kube-system -o yaml
   ```
   {: pre}

### Changing the pending connections backlog
{: #backlog}

You can decrease the default backlog setting for how many pending connections can wait in the server queue.
{: shortdesc}

In the `ibm-cloud-provider-ingress-cm` Ingress configmap, the `backlog` field sets the maximum number of pending connections that can wait in the server queue. By default, `backlog` is set to `32768`. You can override the default by editing the Ingress configmap.

1. Edit the configuration file for the `ibm-cloud-provider-ingress-cm` configmap resource.

    ```
    kubectl edit cm ibm-cloud-provider-ingress-cm -n kube-system
    ```
    {: pre}

2. Change the value of `backlog` from `32768` to a lower value. The value must be equal to or lesser than 32768.

   ```
   apiVersion: v1
   data:
     backlog: "32768"
   kind: ConfigMap
   metadata:
     name: ibm-cloud-provider-ingress-cm
     namespace: kube-system
   ```
   {: codeblock}

3. Save the configuration file.

4. Verify that the configmap changes were applied.

   ```
   kubectl get cm ibm-cloud-provider-ingress-cm -n kube-system -o yaml
   ```
   {: pre}

### Tuning kernel performance
{: #ingress_kernel}

To optimize performance of your Ingress ALBs, you can also [change the Linux kernel `sysctl` parameters on worker nodes](/docs/containers?topic=containers-kernel). Worker nodes are automatically provisioned with optimized kernel tuning, so change these settings only if you have specific performance optimization requirements.
{: shortdesc}

<br />

