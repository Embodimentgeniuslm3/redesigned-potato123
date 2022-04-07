---

copyright:
  years: 2014, 2019
lastupdated: "2019-07-10"

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


# Tuning performance
{: #kernel}

If you have specific performance optimization requirements, you can change the default settings for some cluster components in {{site.data.keyword.containerlong}}.
{: shortdesc}

If you choose to change the default settings, you are doing so at your own risk. You are responsible for running tests against any changed settings and for any potential disruptions caused by the changed settings in your environment.
{: important}

## Optimizing worker node performance
{: #worker}

If you have specific performance optimization requirements, you can change the default settings for the Linux kernel `sysctl` parameters on worker nodes.
{: shortdesc}

Worker nodes are automatically provisioned with optimized kernel performance, but you can change the default settings by applying a custom [Kubernetes `DaemonSet` ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) object to your cluster. The daemon set alters the settings for all existing worker nodes and applies the settings to any new worker nodes that are provisioned in the cluster. No pods are affected.

You must have the [**Manager** {{site.data.keyword.cloud_notm}} IAM service role](/docs/containers?topic=containers-users#platform) for all namespaces to run the sample privileged `initContainer`. After the containers for the deployments are initialized, the privileges are dropped.
{: note}

1. Save the following daemon set in a file named `worker-node-kernel-settings.yaml`. In the `spec.template.spec.initContainers` section, add the fields and values for the `sysctl` parameters that you want to tune. This example daemon set changes the default maximum number of connections that are allowed in the environment via the `net.core.somaxconn` setting and the ephemeral port range via the `net.ipv4.ip_local_port_range` setting.
    ```
    apiVersion: apps/v1
    kind: DaemonSet
    metadata:
      name: kernel-optimization
      namespace: kube-system
      labels:
        tier: management
        app: kernel-optimization
    spec:
      selector:
        matchLabels:
          name: kernel-optimization
      template:
        metadata:
          labels:
            name: kernel-optimization
        spec:
          hostNetwork: true
          hostPID: true
          hostIPC: true
          initContainers:
            - command:
                - sh
                - -c
                - sysctl -w net.ipv4.ip_local_port_range="1025 65535"; sysctl -w net.core.somaxconn=32768;
              image: alpine:3.6
              imagePullPolicy: IfNotPresent
              name: sysctl
              resources: {}
              securityContext:
                privileged: true
                capabilities:
                  add:
                    - NET_ADMIN
              volumeMounts:
                - name: modifysys
                  mountPath: /sys
          containers:
            - resources:
                requests:
                  cpu: 0.01
              image: alpine:3.6
              name: sleepforever
              command: ["/bin/sh", "-c"]
              args:
                - >
                  while true; do
                    sleep 100000;
                  done
          volumes:
            - name: modifysys
              hostPath:
                path: /sys
    ```
    {: codeblock}

2. Apply the daemon set to your worker nodes. The changes are applied immediately.
    ```
    kubectl apply -f worker-node-kernel-settings.yaml
    ```
    {: pre}

<br />

To revert your worker nodes' `sysctl` parameters to the default values set by {{site.data.keyword.containerlong_notm}}:

1. Delete the daemon set. The `initContainers` that applied the custom settings are removed.
    ```
    kubectl delete ds kernel-optimization
    ```
    {: pre}

2. [Reboot all worker nodes in the cluster](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reboot). The worker nodes come back online with the default values applied.

<br />


## Optimizing pod performance
{: #pod}

If you have specific performance workload demands, you can change the default settings for the Linux kernel `sysctl` parameters on pod network namespaces.
{: shortdesc}

To optimize kernel settings for app pods, you can insert an [`initContainer` ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/) patch into the `pod/ds/rs/deployment` YAML for each deployment. The `initContainer` is added to each app deployment that is in the pod network namespace for which you want to optimize performance.

Before you begin, ensure you have the [**Manager** {{site.data.keyword.cloud_notm}} IAM service role](/docs/containers?topic=containers-users#platform) for all namespaces to run the sample privileged `initContainer`. After the containers for the deployments are initialized, the privileges are dropped.

1. Save the following `initContainer` patch in a file named `pod-patch.yaml` and add the fields and values for the `sysctl` parameters that you want to tune. This example `initContainer` changes the default maximum number of connections allowed in the environment via the `net.core.somaxconn` setting and the ephemeral port range via the `net.ipv4.ip_local_port_range` setting.
    ```
    spec:
      template:
        spec:
          initContainers:
          - command:
            - sh
            - -c
            - sysctl -e -w net.core.somaxconn=32768;  sysctl -e -w net.ipv4.ip_local_port_range="1025 65535";
            image: alpine:3.6
            imagePullPolicy: IfNotPresent
            name: sysctl
            resources: {}
            securityContext:
              privileged: true
    ```
    {: codeblock}

2. Patch each of your deployments.
    ```
    kubectl patch deployment <deployment_name> --patch pod-patch.yaml
    ```
    {: pre}

3. If you changed the `net.core.somaxconn` value in the kernel settings, most apps can automatically use the updated value. However, some apps might require you to manually change the corresponding value in your app code to match the kernel value. For example, if you're tuning the performance of a pod where an NGINX app is running, you must change the value of the `backlog` field in the NGINX app code to match. For more information, see this [NGINX blog post ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.nginx.com/blog/tuning-nginx/).

<br />


## Adjusting cluster metrics provider resources
{: #metrics}

Your cluster's metrics provider (`metrics-server` in Kubernetes 1.12 and later, or `heapster` in earlier versions) configurations are optimized for clusters with 30 or less pods per worker node. If your cluster has more pods per worker node, the metrics provider `metrics-server` or `heapster` main container for the pod might restart frequently with an error message such as `OOMKilled`.

The metrics provider pod also has a `nanny` container that scales the `metrics-server` or `heapster` main container's resource requests and limits in response to the number of worker nodes in the cluster. You can change the default resources by editing the metrics provider's configmap.

Before you begin: [Log in to your account. If applicable, target the appropriate resource group. Set the context for your cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  Open the cluster metrics provider configmap YAML.
    *  For `metrics-server`:
       ```
       kubectl get configmap metrics-server-config -n kube-system -o yaml
       ```
       {: pre}
    *  For `heapster`:
       ```
       kubectl get configmap heapster-config -n kube-system -o yaml
       ```
       {: pre}
    Example output:
    ```
    apiVersion: v1
    data:
      NannyConfiguration: |-
        apiVersion: nannyconfig/v1alpha1
        kind: NannyConfiguration
    kind: ConfigMap
    metadata:
      annotations:
        armada-service: cruiser-kube-addons
        version: --
      creationTimestamp: 2018-10-09T20:15:32Z
      labels:
        addonmanager.kubernetes.io/mode: EnsureExists
        kubernetes.io/cluster-service: "true"
      name: heapster-config
      namespace: kube-system
      resourceVersion: "526"
      selfLink: /api/v1/namespaces/kube-system/configmaps/heapster-config
      uid: 11a1aaaa-bb22-33c3-4444-5e55e555e555
    ```
    {: screen}

2.  Add the `memoryPerNode` field to the configmap in the `data.NannyConfiguration` section. The default value for both `metrics-server` and `heapster` is set to `4Mi`.
    ```
    apiVersion: v1
    data:
      NannyConfiguration: |-
        apiVersion: nannyconfig/v1alpha1
        kind: NannyConfiguration
        memoryPerNode: 5Mi
    kind: ConfigMap
    ...
    ```
    {: codeblock}

3.  Apply your changes.
    ```
    kubectl apply -f heapster-config.yaml
    ```
    {: pre}

4.  Monitor the metrics provider pods to see if containers continue to be restarted due to an `OOMKilled` error message. If so, repeat these steps and increase the `memoryPerNode` size until the pod is stable.

Want to tune more settings? Check out the [Kubernetes Add-on resizer configuration docs ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/autoscaler/tree/master/addon-resizer#addon-resizer-configuration) for more ideas.
{: tip}
