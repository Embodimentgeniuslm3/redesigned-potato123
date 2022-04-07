---

copyright:
  years: 2014, 2019
lastupdated: "2019-04-11"

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


# 教程：使用受管 Knative 在 Kubernetes 集群中运行无服务器应用程序
{: #knative_tutorial}

通过本教程，您可以了解如何在 {{site.data.keyword.containerlong_notm}} 的 Kubernetes 集群中安装 Knative。
{: shortdesc}

**什么是 Knative？为什么要使用 Knative？**</br>
[Knative](https://github.com/knative/docs) 是一种开放式源代码平台，由 IBM、Google、Pivotal、Red Hat、Cisco 等公司联合开发，目标是扩展 Kubernetes 的功能，帮助您在 Kubernetes 集群上创建以源代码为中心的现代容器化和无服务器应用程序。该平台旨在满足如今开发者的需求，即必须决定要在云中运行哪种类型的应用程序：12 因子应用程序、容器或功能。每种类型的应用程序都需要一个针对这些应用程序定制的开放式源代码或专有解决方案：对于 12 因子应用程序，需要 Cloud Foundry；对于容器，需要 Kubernetes；对于功能，需要 OpenWhisk 等。过去，开发者必须决定要遵循哪种方法，这会导致在必须组合不同类型的应用程序时，缺乏灵活性且复杂性高。  

Knative 在不同编程语言和框架之间使用一致的方法，对构建、部署和管理 Kubernetes 中工作负载的操作工作进行抽象化处理，以便开发者可以专注于最重要的事项：源代码。您可以使用已经熟悉的成熟构建包，例如 Cloud Foundry、Kaniko、Dockerfile、Bazel 等。通过与 Istio 集成，Knative 可确保无服务器和容器化工作负载可以轻松地在因特网上公开、进行监视和控制，并且数据在传输期间会加密。

**Knative 是如何运作的？**</br>
Knative 随附 3 个主要组件（或称为_原语_），可帮助您在 Kubernetes 集群中构建、部署和管理无服务器应用程序：

- **Build：**`Build` 原语支持创建一组步骤，用于执行从源代码到容器映像的应用程序构建过程。假设使用一个简单的构建模板，在其中指定用于查找应用程序代码的源存储库以及要在其中托管映像的容器注册表。仅使用一个命令，就可以指示 Knative 采用此构建模板，拉取源代码，创建映像并将其推送到容器注册表，从而使您可以在容器中使用该映像。
- **Serving：**`Serving` 原语可帮助将无服务器应用程序部署为 Knative 服务，并自动对其进行缩放，甚至可缩减至零个实例。通过使用 Istio 的流量管理和智能路由功能，您可以控制将哪些流量路由到特定版本的服务，从而使开发者轻松测试和应用新的应用程序版本或执行 A-B 测试。
- **Eventing：**通过使用 `Eventing` 原语，可以创建其他服务可预订的触发器或事件流。例如，您可能希望每次将代码推送到 GitHub 主存储库时，都启动应用程序的新构建。或者，您希望仅当温度低于冰点时，才运行无服务器应用程序。`Eventing` 原语可以集成到 CI/CD 管道中，以便在发生特定事件时自动构建和部署应用程序。

**什么是 Managed Knative on {{site.data.keyword.containerlong_notm}}（试验性）附加组件？** </br> Managed Knative on {{site.data.keyword.containerlong_notm}} 是一个受管附加组件，用于将 Istio 与 Kubernetes 集群直接集成。附加组件中的 Knative 和 Istio 版本已由 IBM 进行测试，支持在 {{site.data.keyword.containerlong_notm}} 中使用。有关受管附加组件的更多信息，请参阅[使用受管附加组件添加服务](/docs/containers?topic=containers-managed-addons#managed-addons)。

**是否存在任何限制？** </br> 如果在集群中安装了[容器映像安全性强制实施程序许可控制器](/docs/services/Registry?topic=registry-security_enforce#security_enforce)，那么无法在集群中启用受管 Knative 附加组件。

听起来很不错？请遵循本教程在 {{site.data.keyword.containerlong_notm}} 中开始使用 Knative。

## 目标
{: #knative_objectives}

- 了解有关 Knative 和 Knative 原语的基础知识。  
- 在集群中安装受管 Knative 和受管 Istio 附加组件。
- 使用 Knative 部署第一个无服务器应用程序，然后使用 Knative `Serving` 原语在因特网上公开该应用程序。
- 探索 Knative 缩放和修订功能。

## 所需时间
{: #knative_time}

30 分钟

## 受众
{: #knative_audience}

本教程适用于有兴趣了解如何使用 Knative 在 Kubernetes 集群中部署无服务器应用程序的开发者，以及希望了解如何在集群中设置 Knative 的集群管理员。

## 先决条件
{: #knative_prerequisites}

-  [安装 IBM Cloud CLI、{{site.data.keyword.containerlong_notm}} 插件和 Kubernetes CLI](/docs/containers?topic=containers-cs_cli_install#cs_cli_install_steps)。确保安装与集群的 Kubernetes 版本相匹配的 `kubectl` CLI 版本。
-  [创建具有至少 3 个工作程序节点的集群，每个节点有 4 个核心和 16 GB 内存 (`b3c.4x16`) 或更高配置](/docs/containers?topic=containers-clusters#clusters_cli)。每个工作程序节点都必须运行 Kubernetes V1.12 或更高版本。
-  确保您具有对 {{site.data.keyword.containerlong_notm}} 的 [{{site.data.keyword.Bluemix_notm}} IAM **写入者**或**管理者**服务角色](/docs/containers?topic=containers-users#platform)。
-  [设定 CLI 的目标为集群](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)。

## 第 1 课：设置受管 Knative 附加组件
{: #knative_setup}

Knative 基于 Istio 而构建，可确保无服务器和容器化工作负载可以在集群内和因特网上公开。通过 Istio，还可以监视和控制服务之间的网络流量，并确保数据在传输期间加密。安装受管 Knative 附加组件时，还会自动安装受管 Istio 附加组件。
{: shortdesc}

1. 在集群中启用受管 Knative 附加组件。在集群中启用 Knative 时，会在集群中安装 Istio 和所有 Knative 组件。
   ```
   ibmcloud ks cluster-addon-enable knative --cluster <cluster_name_or_ID> -y
   ```
   {: pre}

   输出示例：
   ```
   Enabling add-on knative for cluster knative...
   OK
   ```
   {: screen}

   所有 Knative 组件的安装可能需要几分钟时间才能完成。

2. 验证 Istio 是否已成功安装。9 个 Istio 服务的所有 pod 以及 Prometheus 的 pod 必须处于 `Running` 状态。
   ```
   kubectl get pods --namespace istio-system
   ```
   {: pre}

   输出示例：
   ```
    NAME                                       READY     STATUS      RESTARTS   AGE
    istio-citadel-748d656b-pj9bw               1/1       Running     0          2m
    istio-egressgateway-6c65d7c98d-l54kg       1/1       Running     0          2m
    istio-galley-65cfbc6fd7-bpnqx              1/1       Running     0          2m
    istio-ingressgateway-f8dd85989-6w6nj       1/1       Running     0          2m
    istio-pilot-5fd885964b-l4df6               2/2       Running     0          2m
    istio-policy-56f4f4cbbd-2z2bk              2/2       Running     0          2m
    istio-sidecar-injector-646655c8cd-rwvsx    1/1       Running     0          2m
    istio-statsd-prom-bridge-7fdbbf769-8k42l   1/1       Running     0          2m
    istio-telemetry-8687d9d745-mwjbf           2/2       Running     0          2m
    prometheus-55c7c698d6-f4drj                1/1       Running     0          2m
    ```
   {: screen}

3. 可选：如果要对 `default` 名称空间中的所有应用程序使用 Istio，请将 `istio-injection=enabled` 标签添加到该名称空间。每个无服务器应用程序 pod 都必须运行 Envoy 代理侧柜，这样应用程序才能包含在 Istio 服务网中。此标签允许 Istio 在新的应用程序部署中自动修改 pod 模板规范，以便创建使用 Envoy 代理侧柜容器的 pod。
  ```
  kubectl label namespace default istio-injection=enabled
  ```
  {: pre}

4. 验证是否所有 Knative 组件都已成功安装。
   1. 验证是否 Knative `Serving` 组件的所有 pod 都处于 `Running` 状态。  
      ```
      kubectl get pods --namespace knative-serving
      ```
      {: pre}

      输出示例：
        ```
      NAME                          READY     STATUS    RESTARTS   AGE
      activator-598b4b7787-ps7mj    2/2       Running   0          21m
      autoscaler-5cf5cfb4dc-mcc2l   2/2       Running   0          21m
      controller-7fc84c6584-qlzk2   1/1       Running   0          21m
      webhook-7797ffb6bf-wg46v      1/1       Running   0          21m
      ```
      {: screen}

   2. 验证是否 Knative `Build` 组件的所有 pod 都处于 `Running` 状态。  
      ```
      kubectl get pods --namespace knative-build
      ```
      {: pre}

      输出示例：
        ```
      NAME                                READY     STATUS    RESTARTS   AGE
      build-controller-79cb969d89-kdn2b   1/1       Running   0          21m
      build-webhook-58d685fc58-twwc4      1/1       Running   0          21m
      ```
      {: screen}

   3. 验证是否 Knative `Eventing` 组件的所有 pod 都处于 `Running` 状态。
      ```
      kubectl get pods --namespace knative-eventing
      ```
      {: pre}

      输出示例：

      ```
      NAME                                            READY     STATUS    RESTARTS   AGE
      eventing-controller-847d8cf969-kxjtm            1/1       Running   0          22m
      in-memory-channel-controller-59dd7cfb5b-846mn   1/1       Running   0          22m
      in-memory-channel-dispatcher-67f7497fc-dgbrb    2/2       Running   1          22m
      webhook-7cfff8d86d-vjnqq                        1/1       Running   0          22m
      ```
      {: screen}

   4. 验证是否 Knative `Sources` 组件的所有 pod 都处于 `Running` 状态。
      ```
      kubectl get pods --namespace knative-sources
      ```
      {: pre}

      输出示例：
        ```
      NAME                   READY     STATUS    RESTARTS   AGE
      controller-manager-0   1/1       Running   0          22m
      ```
      {: screen}

   5. 验证是否 Knative `Monitoring` 组件的所有 pod 都处于 `Running` 状态。
      ```
      kubectl get pods --namespace knative-monitoring
      ```
      {: pre}

      输出示例：
        ```
      NAME                                  READY     STATUS                 RESTARTS   AGE
      elasticsearch-logging-0               1/1       Running                0          22m
      elasticsearch-logging-1               1/1       Running                0          21m
      grafana-79cf95cc7-mw42v               1/1       Running                0          21m
      kibana-logging-7f7b9698bc-m7v6r       1/1       Running                0          22m
      kube-state-metrics-768dfff9c5-fmkkr   4/4       Running                0          21m
      node-exporter-dzlp9                   2/2       Running                0          22m
      node-exporter-hp6gv                   2/2       Running                0          22m
      node-exporter-hr6vs                   2/2       Running                0          22m
      prometheus-system-0                   1/1       Running                0          21m
      prometheus-system-1                   1/1       Running                0          21m
      ```
      {: screen}

太棒了！现在，Knative 和 Istio 均已安装，您可以将第一个无服务器应用程序部署到集群。

## 第 2 课：将无服务器应用程序部署到集群
{: #deploy_app}

在本课中，您将使用 Go 部署第一个无服务器 [`Hello World`](https://hub.docker.com/r/ibmcom/kn-helloworld) 应用程序。 向样本应用程序发送请求时，该应用程序会读取环境变量 `TARGET` 并显示 `"Hello ${TARGET}!"`. 如果此环境变量为空，那么将返回 `"Hello World!"`。
{: shortdesc}

1. 在 Knative 中为第一个无服务器 `Hello World` 应用程序创建 YAML 文件。要使用 Knative 部署应用程序，必须指定 Knative 服务资源。服务由 Knative `Serving` 原语进行管理，用于负责管理工作负载的整个生命周期。服务可确保每个部署都具有 Knative 修订版、路径和配置。更新服务时，会创建新版本的应用程序，并将其添加到服务的修订历史记录中。Knative 路径可确保将应用程序的每个修订版映射到网络端点，以便您可以控制将多少网络流量路由到特定修订版。Knative 配置会保存特定修订版的设置，以便您始终可以回滚到较旧的修订版或在修订版之间进行切换。有关 Knative `Serving` 资源的更多信息，请参阅 [Knative 文档](https://github.com/knative/docs/tree/master/serving)。
   ```
   apiVersion: serving.knative.dev/v1alpha1
   kind: Service
   metadata:
     name: kn-helloworld
     namespace: default
   spec:
     runLatest:
       configuration:
         revisionTemplate:
           spec:
             container:
               image: docker.io/ibmcom/kn-helloworld
               env:
               - name: TARGET
                 value: "Go Sample v1"
    ```
    {: codeblock}

    <table>
    <caption>了解 YAML 文件的组成部分</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解 YAML 文件的组成部分</th>
    </thead>
    <tbody>
    <tr>
    <td><code>metadata.name</code></td>
    <td>Knative 服务的名称。</td>
    </tr>
    <tr>
    <td><code>metadata.namespace</td>
    <td>要在其中将应用程序部署为 Knative 服务的 Kubernetes 名称空间。</td>
    </tr>
    <tr>
    <td><code>spec.container.image</code></td>
    <td>存储映像的容器注册表的 URL。在此示例中，部署的是存储在 Docker Hub 的 <code>ibmcom</code> 名称空间中的 Knative Hello World 应用程序。</td>
    </tr>
    <tr>
    <td><code>spec.container.env</code></td>
    <td>希望 Knative 服务具有的环境变量的列表。在此示例中，样本应用程序会读取环境变量 <code>TARGET</code> 的值，并在您向应用程序发送请求时，以 <code>"Hello ${TARGET}!"</code>. 如果未提供任何值，那么样本应用程序会返回 <code>"Hello World!"</code>.  </td>
    </tr>
    </tbody>
    </table>

2. 在集群中创建 Knative 服务。创建服务时，Knative `Serving` 原语将为应用程序创建不可变的修订版、Knative 路由、Ingress 路由规则、Kubernetes 服务、Kubernetes pod 和负载均衡器。系统已从 Ingress 子域中为应用程序分配子域，格式为 `<knative_service_name>.<namespace>.<ingress_subdomain>`，可用于通过因特网访问应用程序。
   ```
   kubectl apply -f service.yaml
   ```
   {: pre}

   输出示例：
   ```
   service.serving.knative.dev "kn-helloworld" created
   ```
   {: screen}

3. 验证 pod 是否已创建。pod 包含两个容器。一个容器运行 `Hello World` 应用程序，另一个容器是侧柜，用于运行 Istio 和 Knative 监视和日志记录工具。pod 分配有 `00001` 修订版号。
   ```
   kubectl get pods
   ```
   {: pre}

   输出示例：
   ```
   NAME                                              READY     STATUS    RESTARTS   AGE
   kn-helloworld-00001-deployment-55db6bf4c5-2vftm   2/2       Running   0          16s
   ```
   {: screen}

4. 试用 `Hello World` 应用程序。
   1. 获取分配给 Knative 服务的缺省域。如果更改了 Knative 服务的名称，或者将应用程序部署到了其他名称空间，请在查询中更新这些值。
      ```
      kubectl get ksvc/kn-helloworld
      ```
      {: pre}

      输出示例：
        ```
      NAME            DOMAIN                                                                LATESTCREATED         LATESTREADY           READY   REASON
      kn-helloworld   kn-helloworld.default.mycluster.us-south.containers.appdomain.cloud   kn-helloworld-rjmwt   kn-helloworld-rjmwt   True
      ```
      {: screen}

   2. 使用上一步中检索到的子域对应用程序发出请求。
      ```
      curl -v <service_domain>
      ```
      {: pre}

      输出示例：
        ```
      * Rebuilt URL to: kn-helloworld.default.mycluster.us-south.containers.appdomain.cloud/
      *   Trying 169.46.XX.XX...
      * TCP_NODELAY set
      * Connected to kn-helloworld.default.mycluster.us-south.containers.appdomain.cloud (169.46.XX.XX) port 80 (#0)
      > GET / HTTP/1.1
      > Host: kn-helloworld.default.mycluster.us-south.containers.appdomain.cloud
      > User-Agent: curl/7.54.0
      > Accept: */*
      >
      < HTTP/1.1 200 OK
      < Date: Thu, 21 Mar 2019 01:12:48 GMT
      < Content-Type: text/plain; charset=utf-8
      < Content-Length: 20
      < Connection: keep-alive
      < x-envoy-upstream-service-time: 17
      <
      Hello Go Sample v1!
      * Connection #0 to host kn-helloworld.default.mycluster.us-south.containers.appdomain.cloud left intact
      ```
      {: screen}

5. 等待几分钟，以允许 Knative 对 pod 进行缩减。Knative 会估算处理入局工作负载时必须运行的 pod 数。如果未收到任何网络流量，Knative 会自动缩减 pod，甚至会缩减至零个 pod，如此示例中所示。

   要了解 Knative 如何扩展 pod 吗？请尝试增加应用程序的工作负载，例如使用[基于云的简单负载测试程序](https://loader.io/)等工具。
   {: tip}
   ```
   kubectl get pods
   ```
   {: pre}

   如果看不到任何 `kn-helloworld` pod，Knative 会将应用程序缩减为零个 pod。

6. 更新 Knative 服务样本，并为 `TARGET` 环境变量输入其他值。

   示例服务 YAML：
   ```
   apiVersion: serving.knative.dev/v1alpha1
   kind: Service
   metadata:
     name: kn-helloworld
     namespace: default
   spec:
     runLatest:
       configuration:
         revisionTemplate:
           spec:
             container:
               image: docker.io/ibmcom/kn-helloworld
               env:
               - name: TARGET
                 value: "Mr. Smith"
    ```
    {: codeblock}

7. 将更改应用于服务。更改配置时，Knative 会自动创建新修订版，分配新路径，并且缺省情况下会指示 Istio 将入局网络流量路由到最新修订版。
   ```
   kubectl apply -f service.yaml
   ```
   {: pre}

8. 向应用程序发出新请求，以验证是否已应用更改。
   ```
   curl -v <service_domain>
   ```

   输出示例：
   ```
   ...
   Hello Mr. Smith!
   ```
   {: screen}

9. 验证 Knative 是否再次扩展了 pod，以应对增加的网络流量。pod 会分配有 `00002` 修订版号。您可以使用修订版号来引用应用程序的特定版本，例如要指示 Istio 拆分入局流量以流至两个修订版时。
   ```
   kubectl get pods
   ```

   输出示例：
   ```
   NAME                                              READY     STATUS    RESTARTS   AGE
   kn-helloworld-00002-deployment-55db6bf4c5-2vftm   3/3       Running   0          16s
   ```
   {: screen}

10. 可选：清除 Knative 服务。
    ```
    kubectl delete -f service.yaml
    ```
    {: pre}

太棒了！您已成功将第一个 Knative 应用程序部署到集群，并探索了 Knative `Serving` 原语的修订版和缩放功能。


## 接下来要做什么？   
{: #whats-next}

- 试用此 [Knative 研讨会 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/IBM/knative101/tree/master/workshop)，将您的第一个 `Node.js` 斐波那契数列应用程序部署到集群。
  - 探索如何使用 Knative `Build` 原语通过 GitHub 中的 Dockerfile 构建映像，并自动将该映像推送到 {{site.data.keyword.registrylong_notm}} 中的名称空间。  
  - 了解如何设置网络流量的路由，使其从 IBM 提供的 Ingress 子域流至 Knative 提供的 Istio Ingress 网关。
  - 应用新版本的应用程序，并使用 Istio 来控制路由到每个应用程序版本的流量。
- 探索 [Knative `Eventing` ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/knative/docs/tree/master/eventing/samples) 样本。
- 使用 [Knative 文档 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/knative/docs) 了解有关 Knative 的更多信息。
