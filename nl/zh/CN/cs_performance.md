---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-05"

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


# 调整性能
{: #kernel}

如果您具有特定的性能优化需求，那么可以在 {{site.data.keyword.containerlong}} 中更改某些集群组件的缺省设置。
{: shortdesc}

如果选择更改缺省设置，由此带来的风险由您自行承担。对任何已更改的设置运行测试是您的责任，并且对环境中因更改的设置而导致的任何潜在中断也由您负责。
{: important}

## 优化工作程序节点性能
{: #worker}

如果您具有特定的性能优化需求，那么可以更改工作程序节点上 Linux 内核 `sysctl` 参数的缺省设置。
{: shortdesc}

系统会自动向工作程序节点供应优化的内核性能，但您可以通过将定制 [Kubernetes `守护程序集` ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) 对象应用于集群来更改缺省设置。守护程序集将更改所有现有工作程序节点的设置，并将设置应用于集群中供应的任何新工作程序节点。这不会影响任何 pod。

您必须具有对所有名称空间的 [{{site.data.keyword.Bluemix_notm}} IAM **管理者**服务角色](/docs/containers?topic=containers-users#platform)，才能运行样本特权 `initContainer`。在初始化这些部署的容器之后，将删除这些特权。
{: note}

1. 将以下守护程序集保存在名为 `worker-node-kernel-settings.yaml` 的文件中。在 `spec.template.spec.initContainers` 部分中，添加要调整的 `sysctl` 参数的字段和值。此示例守护程序集通过 `net.core.somaxconn` 设置来更改环境中允许的缺省最大连接数，并通过 `net.ipv4.ip_local_port_range` 设置来更改临时端口范围。
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

2. 将守护程序集应用于工作程序节点。这将立即应用更改。
    ```
    kubectl apply -f worker-node-kernel-settings.yaml
    ```
    {: pre}

<br />

要将工作程序节点的 `sysctl` 参数还原为 {{site.data.keyword.containerlong_notm}} 设置的缺省值，请执行以下操作：

1. 删除守护程序集。这将除去应用了定制设置的 `initContainer`。
    ```
    kubectl delete ds kernel-optimization
    ```
    {: pre}

2. [重新引导集群中的所有工作程序节点](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reboot)。工作程序节点恢复为应用了缺省值的联机状态。

<br />


## 优化 pod 性能
{: #pod}

如果您具有特定的性能工作负载需求，那么可以更改 pod 网络名称空间上 Linux 内核 `sysctl` 参数的缺省设置。
{: shortdesc}

要优化应用程序 pod 的内核设置，可以在每个部署的 `pod/ds/rs/deployment` YAML 中插入 [`initContainer ` ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/)。`initContainer` 将添加到 pod 网络名称空间中要优化其性能的每个应用程序部署。

开始之前，确保您具有对所有名称空间的 [{{site.data.keyword.Bluemix_notm}} IAM **管理者**服务角色](/docs/containers?topic=containers-users#platform)，才能运行样本特权 `initContainer`。在初始化这些部署的容器之后，将删除这些特权。

1. 将以下 `initContainer` 补丁保存在名为 `pod-patch.yaml` 的文件中，并为要调整的 `sysctl` 参数添加字段和值。此示例 `initContainer` 通过 `net.core.somaxconn` 设置来更改环境中允许的缺省最大连接数，并通过 `net.ipv4.ip_local_port_range` 设置来更改临时端口范围。
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

2. 修补每个部署。
    ```
    kubectl patch deployment <deployment_name> --patch pod-patch.yaml
    ```
    {: pre}

3. 如果在内核设置中更改了 `net.core.somaxconn` 值，那么大多数应用程序可以自动使用更新后的值。但是，某些应用程序可能会要求您手动更改应用程序代码中的对应值以与内核值相匹配。例如，如果要调整在运行 NGINX 应用程序的 pod 的性能，那么必须更改 NGINX 应用程序代码中 `backlog` 字段的值以进行匹配。有关更多信息，请参阅此 [NGINX 博客帖子 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.nginx.com/blog/tuning-nginx/)。

<br />


## 调整集群度量值提供程序资源
{: #metrics}

集群的度量值提供程序（在 Kubernetes 1.12 和更高版本中为 `metrics-server`，在更低版本中为 `heapster`）配置针对每个工作程序节点不超过 30 个 pod 的集群进行了优化。如果集群的每个工作程序节点具有更多 pod，那么用于 pod 的度量值提供程序 `metrics-server` 或 `heapter` 主容器可能会频繁重新启动，并返回错误消息，例如 `OOMKilled`。

度量值提供程序 pod 还有一个 `nanny` 容器，用于对 `metrics-server` 或 `heapster` 主容器的资源请求和限制进行调整，以响应集群中工作程序节点的数量。您可以通过编辑度量值提供程序的配置映射来更改缺省资源。

开始之前：[登录到您的帐户。如果适用，请将相应的资源组设定为目标。为集群设置上下文。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  打开集群度量值提供程序配置映射 YAML。
    *  对于 `metrics-server`：
       ```
       kubectl get configmap metrics-server-config -n kube-system -o yaml
       ```
       {: pre}
    *  对于 `heapster`：
       ```
       kubectl get configmap heapster-config -n kube-system -o yaml
       ```
       {: pre}
    输出示例：
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

2.  将 `memoryPerNode` 字段添加到 `data.NannyConfiguration` 部分中的配置映射。`metrics-server` 和 `heapster` 的缺省值设置为 `4Mi`。
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

3.  应用更改。
    ```
    kubectl apply -f heapster-config.yaml
    ```
    {: pre}

4.  监视度量值提供程序 pod 以了解容器是否由于 `OOMKilled` 错误消息而持续重新启动。如果是，请重复上述步骤来增大 `memoryPerNode` 大小，直至 pod 稳定。

要调整更多设置吗？请查看 [Kubernetes Add-on resizer configuration 文档 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/kubernetes/autoscaler/tree/master/addon-resizer#addon-resizer-configuration) 以了解更多构想。
{: tip}
