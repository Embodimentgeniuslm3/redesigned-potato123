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


# Bringing your own Ingress controller
{: #ingress-user_managed}

Bring your own Ingress controller to run on {{site.data.keyword.cloud_notm}} and leverage an IBM-provided host name and TLS certificate.
{: shortdesc}

The IBM-provided Ingress application load balancers (ALBs) are based on NGINX controllers that you can configure by using [custom {{site.data.keyword.cloud_notm}} annotations](/docs/containers?topic=containers-ingress_annotation). Depending on what your app requires, you might want to configure your own custom Ingress controller. When you bring your own Ingress controller instead of using the IBM-provided Ingress ALB, you are responsible for supplying the controller image, maintaining the controller, updating the controller, and any security-related updates to keep your Ingress controller free from vulnerabilities. **Note**: Bringing your own Ingress controller is supported only for providing public external access to your apps and is not supported for providing private external access.

You have 2 options for bringing your own Ingress controller:
* [Create a network load balancer (NLB) to expose your custom Ingress controller deployment, and then create a host name for the NLB IP address](#user_managed_nlb). {{site.data.keyword.cloud_notm}} provides the host name and takes care of generating and maintaining a wildcard SSL certificate for the host name for you. For more information about IBM-provided NLB DNS host names, see [Registering an NLB host name](/docs/containers?topic=containers-loadbalancer_hostname).
* [Disable the IBM-provided ALB deployment and use the load balancer service that exposed the ALB and the DNS registration for the IBM-provided Ingress subdomain](#user_managed_alb). This option permits you to leverage the Ingress subdomain and TLS certificate that are already assigned to your cluster.

## Expose your Ingress controller by creating an NLB and a host name
{: #user_managed_nlb}

Create a network load balancer (NLB) to expose your custom Ingress controller deployment, and then create a host name for the NLB IP address.
{: shortdesc}

1. Get the configuration file for your Ingress controller ready. For example, you can use the [cloud-generic NGINX community Ingress controller ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/ingress-nginx/tree/master/deploy/cloud-generic). If you use the community controller, edit the `kustomization.yaml` file by following these steps.
  1. Replace the `namespace: ingress-nginx` with `namespace: kube-system`.
  2. In the `commonLabels` section, replace the `app.kubernetes.io/name: ingress-nginx` and `app.kubernetes.io/part-of: ingress-nginx` labels with one `app: ingress-nginx` label.

2. Deploy your own Ingress controller. For example, to use the cloud-generic NGINX community Ingress controller, run the following command.
    ```
    kubectl apply --kustomize . -n kube-system
    ```
    {: pre}

3. Define a load balancer to expose your custom Ingress deployment.
    ```
    apiVersion: v1
    kind: Service
    metadata:
      name: my-lb-svc
    spec:
      type: LoadBalancer
      selector:
        app: ingress-nginx
      ports:
       - protocol: TCP
         port: 8080
      externalTrafficPolicy: Local
    ```
    {: codeblock}

4. Create the service in your cluster.
    ```
    kubectl apply -f my-lb-svc.yaml
    ```
    {: pre}

5. Get the **EXTERNAL-IP** address for the load balancer.
    ```
    kubectl get svc my-lb-svc -n kube-system
    ```
    {: pre}

    In the following example output, the **EXTERNAL-IP** is `168.1.1.1`.
    ```
    NAME         TYPE           CLUSTER-IP       EXTERNAL-IP      PORT(S)            AGE
    my-lb-svc    LoadBalancer   172.21.xxx.xxx   168.1.1.1        80:30104/TCP       2m
    ```
    {: screen}

6. Register the load balancer IP address by creating a DNS host name.
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

8. Optional: [Enable health checks on the host name by creating a health monitor](/docs/containers?topic=containers-loadbalancer_hostname#loadbalancer_hostname_monitor).

9. Deploy any other resources that are required by your custom Ingress controller, such as the configmap.

10. Create Ingress resources for your apps. You can use the Kubernetes documentation to create [an Ingress resource file ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/services-networking/ingress/) and use [annotations ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.github.io/ingress-nginx/user-guide/nginx-configuration/annotations/).
  <p class="tip">If you continue to use IBM-provided ALBs concurrently with your custom Ingress controller in one cluster, you can create separate Ingress resources for your ALBs and custom controller. In the [Ingress resource that you create to apply to the IBM ALBs only](/docs/containers?topic=containers-ingress#ingress_expose_public), add the annotation <code>kubernetes.io/ingress.class: "iks-nginx"</code>.</p>

11. Access your app by using the load balancer host name that you found in step 7 and the path that your app listens on that you specified in the Ingress resource file.
  ```
  https://<load_blanacer_host_name>/<app_path>
  ```
  {: codeblock}

## Expose your Ingress controller by using the existing load balancer and Ingress subdomain
{: #user_managed_alb}

Disable the IBM-provided ALB deployment and use the load balancer service that exposed the ALB and the DNS registration for the IBM-provided Ingress subdomain.
{: shortdesc}

1. Get the ID of the default public ALB. The public ALB is formatted similar to `public-cr18e61e63c6e94b658596ca93d087eed9-alb1`.
    ```
    kubectl get svc -n kube-system | grep alb
    ```
    {: pre}

2. Disable the default public ALB. The `--disable-deployment` flag disables the IBM-provided ALB deployment, but doesn't remove the DNS registration for the IBM-provided Ingress subdomain or the load balancer service that is used to expose the Ingress controller.
    ```
    ibmcloud ks alb-configure --albID <ALB_ID> --disable-deployment
    ```
    {: pre}

3. Get the configuration file for your Ingress controller ready. For example, you can use the [cloud-generic NGINX community Ingress controller ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/ingress-nginx/tree/master/deploy/cloud-generic). If you use the community controller, edit the `kustomization.yaml` file by following these steps.
  1. Replace the `namespace: ingress-nginx` with `namespace: kube-system`.
  2. In the `commonLabels` section, replace the `app.kubernetes.io/name: ingress-nginx` and `app.kubernetes.io/part-of: ingress-nginx` labels with one `app: ingress-nginx` label.
  3. In the `SERVICE_NAME` variable, replace `name: ingress-nginx` with `name: <ALB_ID>`. For example, the ALB ID from step 1 might look like `name: public-cr18e61e63c6e94b658596ca93d087eed9-alb1`.

4. Deploy your own Ingress controller. For example, to use the cloud-generic NGINX community Ingress controller, run the following command. **Important**: To continue to use the load balancer service exposing the controller and the IBM-provided Ingress subdomain, your controller must be deployed in the `kube-system` namespace.
    ```
    kubectl apply --kustomize . -n kube-system
    ```
    {: pre}

5. Get the label on your custom Ingress deployment.
    ```
    kubectl get deploy <ingress-controller-name> -n kube-system --show-labels
    ```
    {: pre}

    In the following example output, the label value is `ingress-nginx`:
    ```
    NAME                       DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE       LABELS
    nginx-ingress-controller   1         1         1            1           1m        app=ingress-nginx
    ```
    {: screen}

6. Using the ALB ID you got in step 1, open the load balancer service that exposes the IBM ALB.
    ```
    kubectl edit svc <ALB_ID> -n kube-system
    ```
    {: pre}

7. Update the load balancer service to point to your custom Ingress deployment. Under `spec/selector`, remove the ALB ID from the `app` label and add the label for your own Ingress controller that you got in step 5.
    ```
    apiVersion: v1
    kind: Service
    metadata:
      ...
    spec:
      clusterIP: 172.21.xxx.xxx
      externalTrafficPolicy: Cluster
      loadBalancerIP: 169.xx.xxx.xxx
      ports:
      - name: http
        nodePort: 31070
        port: 80
        protocol: TCP
        targetPort: 80
      - name: https
        nodePort: 31854
        port: 443
        protocol: TCP
        targetPort: 443
      selector:
        app: <custom_controller_label>
      ...
    ```
    {: codeblock}
    1. Optional: By default, the load balancer service allows traffic on port 80 and 443. If your custom Ingress controller requires a different set of ports, add those ports to the `ports` section.

8. Save and close the configuration file. The output is similar to the following:
    ```
    service "public-cr18e61e63c6e94b658596ca93d087eed9-alb1" edited
    ```
    {: screen}

9. Verify that the ALB `Selector` now points to your controller.
    ```
    kubectl describe svc <ALB_ID> -n kube-system
    ```
    {: pre}

    Example output:
    ```
    Name:                     public-cre58bff97659a4f41bc927362d5a8ee7a-alb1
    Namespace:                kube-system
    Labels:                   app=public-cre58bff97659a4f41bc927362d5a8ee7a-alb1
    Annotations:              service.kubernetes.io/ibm-ingress-controller-public=169.xx.xxx.xxx
                              service.kubernetes.io/ibm-load-balancer-cloud-provider-zone=wdc07
    Selector:                 app=ingress-nginx
    Type:                     LoadBalancer
    IP:                       172.21.xxx.xxx
    IP:                       169.xx.xxx.xxx
    LoadBalancer Ingress:     169.xx.xxx.xxx
    Port:                     port-443  443/TCP
    TargetPort:               443/TCP
    NodePort:                 port-443  30087/TCP
    Endpoints:                172.30.xxx.xxx:443
    Port:                     port-80  80/TCP
    TargetPort:               80/TCP
    NodePort:                 port-80  31865/TCP
    Endpoints:                172.30.xxx.xxx:80
    Session Affinity:         None
    External Traffic Policy:  Cluster
    Events:                   <none>
    ```
    {: screen}

10. Deploy any other resources that are required by your custom Ingress controller, such as the configmap.

11. If you have a multizone cluster, repeat these steps for each ALB.

12. If you have a multizone cluster, you must configure a health check. The Cloudflare DNS healthcheck endpoint, `albhealth.<clustername>.<region>.containers.appdomain.com`, expects a `200` response with a body of `healthy` in the response. If no health check is set up to return `200` and `healthy`, the health check removes any ALB IP addresses from the DNS pool. You can either edit the existing healthcheck resource, or create your own.
  * To edit the existing healthcheck resource:
      1. Open the `alb-health` resource.
        ```
        kubectl edit ingress alb-health --namespace kube-system
        ```
        {: pre}
      2. In the `metadata.annotations` section, change the `ingress.bluemix.net/server-snippets` annotation name to the annotation that your controller supports. For example, you might use the `nginx.ingress.kubernetes.io/server-snippet` annotation. Do not change the content of the server snippet.
      3. Save and close the file. Your changes are automatically applied.
  * To create your own healthcheck resource, ensure that the following snippet is returned to Cloudflare:
    ```
    { return 200 'healthy'; add_header Content-Type text/plain; }
    ```
    {: codeblock}

13. Create Ingress resources for your apps by following the steps in [Exposing apps that are inside your cluster to the public](/docs/containers?topic=containers-ingress#ingress_expose_public).

Your apps are now exposed by your custom Ingress controller. To restore the IBM-provided ALB deployment, re-enable the ALB. The ALB is redeployed, and the load balancer service is automatically reconfigured to point to the ALB.

```
ibmcloud ks alb-configure --albID <alb ID> --enable
```
{: pre}
