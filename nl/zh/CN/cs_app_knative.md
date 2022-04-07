---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

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


# 使用 Knative 部署无服务器应用程序
{: #serverless-apps-knative}

了解如何在 {{site.data.keyword.containerlong_notm}} 的 Kubernetes 集群中安装和使用 Knative。
{: shortdesc}

**什么是 Knative？为什么要使用 Knative？**</br>
[Knative](https://github.com/knative/docs) 是一种开放式源代码平台，由 IBM、Google、Pivotal、Red Hat、Cisco 等公司联合开发。目标是扩展 Kubernetes 的功能，以帮助您基于 Kubernetes 集群来创建以源代码为中心的现代容器化无服务器应用程序。该平台旨在满足当下的开发者的需求，因为他们必须决定要在云中运行哪种类型的应用程序：12 因子应用程序、容器或函数。每种应用程序都需要一个针对自己量身定制的开放式源代码或专有解决方案：对于 12 因子应用程序，需要 Cloud Foundry；对于容器，需要 Kubernetes；对于函数，需要 OpenWhisk 及其他解决方案。过去，开发者必须决定要遵循哪种方法，这会导致在需要组合不同类型的应用程序时不够灵活且略显复杂。  

对于不同的编程语言和框架，Knative 会使用一致的方法来抽象化处理 Kubernetes 中有关构建、部署和管理工作负载的操作负担，让开发者可以专注于最重要的事情：源代码。您可以使用已经熟悉的成熟 buildpack，例如 Cloud Foundry、Kaniko、Dockerfile、Bazel 等。通过与 Istio 集成，Knative 可确保您能够轻松地在因特网上公开无服务器和容器化工作负载，并对其进行监视和控制，还可确保在传输期间对数据加密。

**Knative 是如何运作的？**</br>
Knative 随附三个主要组件（或称为_原语_），可帮助您在 Kubernetes 集群中构建、部署和管理无服务器应用程序：

- **Build**：`Build` 原语支持创建一组从源代码到容器映像的应用程序构建步骤。试想一下，您使用一个简单的构建模板，在其中可指定用于查找应用程序代码的源存储库以及要在其中托管映像的容器注册表。只需使用一个命令，即可指示 Knative 采用此构建模板，拉取源代码，创建映像并将其推送到容器注册表，以便可以在容器中使用该映像。
- **Serving**：`Serving` 原语可帮助您将无服务器应用程序部署为 Knative 服务，还可自动对其进行缩放，有时甚至可缩减至零个实例。为了公开无服务器容器化工作负载，Knative 将使用 Istio。安装受管 Knative 附加组件时，还会自动安装受管 Istio 附加组件。
通过使用 Istio 的流量管理和智能路由功能，您可以控制将哪些流量路由到特定版本的服务，从而使开发者轻松地测试和应用新的应用程序版本或执行 A-B 测试。
- **Eventing**：通过使用 `Eventing` 原语，您可以创建其他服务可预订的触发器或事件流。例如，您可能希望每次将代码推送到 GitHub 主存储库时，都能开始新的应用程序构建。或者，您希望仅当温度低于冰点时，才运行无服务器应用程序。例如，`Eventing` 原语可以集成到 CI/CD 管道中来自动构建和部署应用程序，以防发生特定事件。

**什么是 Managed Knative on {{site.data.keyword.containerlong_notm}}（试验性）附加组件？** </br> Managed Knative on {{site.data.keyword.containerlong_notm}} 是一个[受管附加组件](/docs/containers?topic=containers-managed-addons#managed-addons)，用于将 Knative 和 Istio 直接与 Kubernetes 集群集成。附加组件中的 Knative 和 Istio 版本已经过 IBM 测试，可在 {{site.data.keyword.containerlong_notm}} 中使用。有关受管附加组件的更多信息，请参阅[使用受管附加组件添加服务](/docs/containers?topic=containers-managed-addons#managed-addons)。

**是否存在任何限制？** </br> 如果在集群中安装了[容器映像安全性强制实施程序许可控制器](/docs/services/Registry?topic=registry-security_enforce#security_enforce)，那么无法在集群中启用受管 Knative 附加组件。

## 在集群中设置 Knative
{: #knative-setup}

Knative 是基于 Istio 构建的，可确保您能够在集群内和因特网上公开无服务器和容器化工作负载。通过 Istio，还可以监视和控制服务之间的网络流量，并确保在传输期间对数据加密。安装受管 Knative 附加组件时，还会自动安装受管 Istio 附加组件。
{: shortdesc}

开始之前：
-  [安装 IBM Cloud CLI、{{site.data.keyword.containerlong_notm}} 插件和 Kubernetes CLI](/docs/containers?topic=containers-cs_cli_install#cs_cli_install_steps)。确保所安装的 `kubectl` CLI 版本与集群的 Kubernetes 版本相匹配。
-  [创建具有至少 3 个工作程序节点的标准集群，每个节点有 4 个核心和 16 GB 内存 (`b3c.4x16`) 或更高配置](/docs/containers?topic=containers-clusters#clusters_ui)。此外，集群和工作程序节点必须至少运行最低受支持的 Kubernetes 版本，您可以通过运行 `ibmcloud ks addon-versions --addon knative` 来查看版本。
-  确保您具有对 {{site.data.keyword.containerlong_notm}} 的 [{{site.data.keyword.Bluemix_notm}} IAM **写入者**或**管理者**服务角色](/docs/containers?topic=containers-users#platform)。
-  [将 CLI 的目标指定为集群](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)。
</br>

要在集群中安装 Knative，请执行以下操作：

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

2. 验证 Istio 是否已成功安装。9 个 Istio 服务的所有 pod 以及 Prometheus 的 pod 都必须处于 `Running` 阶段状态。
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

4. 验证是否 Knative `Serving` 组件的所有 pod 都处于 `Running` 状态。
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

## 使用 Knative 服务部署无服务器应用程序
{: #knative-deploy-app}

在集群中设置 Knative 后，可以将无服务器应用程序部署为 Knative 服务。
{: shortdesc}

**什么是 Knative 服务？**</br>
要使用 Knative 部署应用程序，必须指定 Knative `服务`资源。Knative 服务由 Knative `Serving` 原语进行管理，用于负责管理工作负载的整个生命周期。创建服务时，Knative `Serving` 原语会自动为无服务器应用程序创建一个版本，并将此版本添加到服务的修订历史记录。系统会从 Ingress 子域中为无服务器应用程序分配公共 URL，格式为 `<knative_service_name>.<namespace>.<ingress_subdomain>`，可用于通过因特网访问应用程序。此外，系统还会为应用程序分配专用主机名，格式为 `<knative_service_name>.<namespace>.cluster.local`，可用于从集群内访问应用程序。

**创建 Knative 服务时，后台会执行哪些操作？**</br>
创建 Knative 服务时，应用程序会自动部署为集群中的 Kubernetes pod，并使用 Kubernetes 服务进行公开。为分配公共主机名，Knative 将使用 IBM 提供的 Ingress 子域和 TLS 证书。入局网络流量根据 IBM 提供的缺省 Ingress 路由规则进行路由。

**如何应用新版本应用程序？**</br>
更新 Knative 服务时，会创建新版本的无服务器应用程序。为此版本分配的公共和专用主机名与先前版本的相同。缺省情况下，所有入局网络流量都会路由到最新版本的应用程序。但是，您还可以指定希望路由到特定应用程序版本的入局网络流量的百分比，以便可以执行 A-B 测试。可以同时在两个应用程序版本（应用程序的当前版本和要应用的新版本）之间拆分入局网络流量。  

**可以使用自带定制域和 TLS 证书吗？** </br>
可以更改 Istio Ingress 网关的配置映射和 Ingress 路由规则，以在为无服务器应用程序分配主机名时使用定制域名和 TLS 证书。有关更多信息，请参阅[设置定制域名和证书](#knative-custom-domain-tls)。

要将无服务器应用程序部署为 Knative 服务，请执行以下操作：

1. 在 Knative 中，使用 Go 为第一个无服务器 [`Hello World`](https://hub.docker.com/r/ibmcom/kn-helloworld) 应用程序创建 YAML 文件。在向样本应用程序发送请求时，该应用程序会读取环境变量 `TARGET` 并输出 `"Hello ${TARGET}!"`. 如果此环境变量为空，那么将返回 `"Hello World!"`。

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
    <td>可选：要在其中将应用程序部署为 Knative 服务的 Kubernetes 名称空间。缺省情况下，所有服务都会部署到 <code>default</code> Kubernetes 名称空间。</td>
    </tr>
    <tr>
    <td><code>spec.container.image</code></td>
    <td>存储映像的容器注册表的 URL。在此示例中，所部署的 Knative Hello World 应用程序会存储在 Docker Hub 的 <code>ibmcom</code> 名称空间中。</td>
    </tr>
    <tr>
    <td><code>spec.container.env</code></td>
    <td>可选：希望 Knative 服务具有的环境变量的列表。在此示例中，样本应用程序会读取环境变量 <code>TARGET</code> 的值，并在您向应用程序发送请求时，以 <code>"Hello ${TARGET}!"</code>. 如果未提供任何值，那么样本应用程序会返回 <code>"Hello World!"</code>.  </td>
    </tr>
    </tbody>
    </table>

2. 在集群中创建 Knative 服务。
   ```
   kubectl apply -f service.yaml
   ```
   {: pre}

   输出示例：
   ```
   service.serving.knative.dev "kn-helloworld" created
   ```
   {: screen}

3. 验证 Knative 服务是否已创建。在 CLI 输出中，可以查看分配给无服务器应用程序的公共 **DOMAIN**。**LATESTCREATED** 和 **LATESTREADY** 列显示上次创建的应用程序版本和当前部署的版本，格式为 `<knative_service_name>-<version>`。分配给应用程序的版本是随机字符串值。在此示例中，无服务器应用程序的版本为 `rjmwt`。更新服务时，会创建新版本的应用程序，并为该版本分配新的随机字符串。  
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

4. 通过向分配给应用程序的公共 URL 发送请求，试用 `Hello World` 应用程序。
   ```
   curl -v <public_app_url>
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
      Hello Go Sample v1!* Connection #0 to host kn-helloworld.default.mycluster.us-south.containers.appdomain.cloud left intact
   ```
   {: screen}

5. 列出为 Knative 服务创建的 pod 数。在本主题的示例中，部署了一个 pod，其中包含两个容器。其中一个容器运行 `Hello World` 应用程序，另一个容器是侧柜，用于运行 Istio 和 Knative 监视和日志记录工具。
   ```
   kubectl get pods
   ```
   {: pre}

   输出示例：
   ```
   NAME                                             READY     STATUS    RESTARTS   AGE
   kn-helloworld-rjmwt-deployment-55db6bf4c5-2vftm  2/2      Running   0          16s
   ```
   {: screen}

6. 等待几分钟，以允许 Knative 对 pod 进行缩减。Knative 会在每次处理入局工作负载时估算必须运行的 pod 数。如果未收到任何网络流量，Knative 会自动缩减 pod，有时甚至会缩减至零个 pod，如此示例中所示。

   要了解 Knative 如何扩展 pod 吗？请尝试增加应用程序的工作负载，例如使用[基于云的简单负载测试程序](https://loader.io/)等工具。
   {: tip}
   ```
   kubectl get pods
   ```
   {: pre}

   如果看不到任何 `kn-helloworld` pod，那么表明 Knative 已将应用程序缩减为零个 pod。

7. 更新 Knative 服务样本，并为 `TARGET` 环境变量输入其他值。

   服务 YAML 示例：
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

8. 将更改应用于服务。更改配置时，Knative 会自动创建新版本的应用程序。
   ```
   kubectl apply -f service.yaml
   ```
   {: pre}

9. 验证是否部署了新版本的应用程序。在 CLI 输出中，可以在 **LATESTCREATED** 列中看到应用程序的新版本。在 **LATESTREADY** 列中看到相同的应用程序版本时，说明应用程序已全部设置并准备就绪，可在分配的公共 URL 上接收入局网络流量。
   ```
      kubectl get ksvc/kn-helloworld
      ```
   {: pre}

   输出示例：
   ```
   NAME            DOMAIN                                                                LATESTCREATED         LATESTREADY           READY   REASON
   kn-helloworld   kn-helloworld.default.mycluster.us-south.containers.appdomain.cloud   kn-helloworld-ghyei   kn-helloworld-ghyei   True
   ```
   {: screen}

9. 向应用程序发出新请求，以验证是否已应用更改。
   ```
   curl -v <service_domain>
   ```

   输出示例：
   ```
   ...
   Hello Mr. Smith!
   ```
   {: screen}

10. 验证 Knative 是否已重新扩展 pod 来应对增加的网络流量。
    ```
    kubectl get pods
    ```

    输出示例：
    ```
    NAME                                              READY     STATUS    RESTARTS   AGE
    kn-helloworld-ghyei-deployment-55db6bf4c5-2vftm   2/2       Running   0          16s
    ```
    {: screen}

11. 可选：清除 Knative 服务。
    ```
    kubectl delete -f service.yaml
    ```
    {: pre}


## 设置定制域名和证书
{: #knative-custom-domain-tls}

您可以配置 Knative，以从配置为使用 TLS 的您自己的定制域中分配主机名。
{: shortdesc}

缺省情况下，系统会从 Ingress 子域中为每个应用程序分配公共子域，格式为 `<knative_service_name>.<namespace>.<ingress_subdomain>`，可用于通过因特网访问应用程序。此外，系统还会为应用程序分配专用主机名，格式为 `<knative_service_name>.<namespace>.cluster.local`，可用于从集群内访问应用程序。如果要从您拥有的定制域分配主机名，那么可以更改 Knative 配置映射以改为使用定制域。

1. 创建定制域。要注册定制域，请使用您的域名服务 (DNS) 提供程序或 [ DNS](/docs/infrastructure/dns?topic=dns-getting-started)。
2. 配置域以将入局网络流量路由到 IBM 提供的 Ingress 网关。在以下选项之间进行选择：
   - 通过将 IBM 提供的域指定为规范名称记录 (CNAME)，定义定制域的别名。要查找 IBM 提供的 Ingress 域，请运行 `ibmcloud ks cluster-get --cluster <cluster_name>` 并查找 **Ingress 子域**字段。使用 CNAME 为首选，因为 IBM 在 IBM 子域上提供自动运行状况检查并从 DNS 响应除去任何失败的 IP。
   - 通过将 Ingress 网关的可移植公共 IP 地址添加为记录，将定制域映射到该 IP 地址。要查找 Ingress 网关的公共 IP 地址，请运行 `nslookup <ingress_subdomain>`。
3. 购买用于定制域的正式通配符 TLS 证书。如果要购买多个 TLS 证书，请确保每个证书的 [CN ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://support.dnsimple.com/articles/what-is-common-name/) 都是不同的。
4. 创建用于证书和密钥的 Kubernetes 私钥。
   1. 将证书和密钥编码为 Base64，并将 Base64 编码的值保存在新文件中。
      ```
      openssl base64 -in tls.key -out tls.key.base64
      ```
      {: pre}

   2. 查看证书和密钥的 Base64 编码值。
      ```
      cat tls.key.base64
      ```
      {: pre}

   3. 使用证书和密钥创建私钥 YAML 文件。
      ```
      apiVersion: v1
      kind: Secret
      metadata:
        name: mydomain-ssl
      type: Opaque
      data:
        tls.crt: <client_certificate>
        tls.key: <client_key>
      ```
      {: codeblock}

   4. 在集群中创建证书。
      ```
      kubectl create -f secret.yaml
      ```
      {: pre}

5. 打开集群的 `istio-system` 名称空间中的 `iks-knative-ingress` Ingress 资源，以开始对其进行编辑。
   ```
   kubectl edit ingress iks-knative-ingress -n istio-system
   ```
   {: pre}

6. 更改 Ingress 的缺省路由规则。
   - 将定制通配符域添加到 `spec.rules.host` 部分，以便将来自定制域和任何子域的所有入局网络流量路由到 `istio-ingressgateway`。
   - 配置定制通配符域的所有主机，以使用先前在 `spec.tls.hosts` 部分中创建的 TLS 私钥。

   示例 Ingress：
   ```
   apiVersion: extensions/v1beta1
   kind: Ingress
   metadata:
     name: iks-knative-ingress
     namespace: istio-system
   spec:
     rules:
     - host: '*.mydomain'
       http:
         paths:
         - backend:
             serviceName: istio-ingressgateway
             servicePort: 80
           path: /
     tls:
     - hosts:
       - '*.mydomain'
       secretName: mydomain-ssl
   ```
   {: codeblock}

   `spec.rules.host` 和 `spec.tls.hosts` 部分是列表，可以包含多个定制域和 TLS 证书。
   {: tip}

7. 修改 Knative `config-domain` 配置映射，以使用定制域将主机名分配给新的 Knative 服务。
   1. 打开 `config-domain` 配置映射以开始对其进行编辑。
      ```
      kubectl edit configmap config-domain -n knative-serving
      ```
      {: pre}

   2. 在配置映射的 `data` 部分中指定定制域，并除去为集群设置的缺省域。
      - **从定制域中为所有 Knative 服务分配主机名的示例**：
        ```
        apiVersion: v1
        kind: ConfigMap
        data:
          <custom_domain>: ""
        metadata:
          name: config-domain
          namespace: knative-serving
        ```
        {: codeblock}

        通过将 `""` 添加到定制域，系统会从定制域中为创建的所有 Knative 服务分配主机名。  

      - **从定制域中为所选 Knative 服务分配主机名的示例**：
        ```
        apiVersion: v1
        kind: ConfigMap
        data:
          <custom_domain>: |
            selector:
              app: sample
          mycluster.us-south.containers.appdomain.cloud: ""
        metadata:
          name: config-domain
          namespace: knative-serving
        ```
        {: codeblock}

        要从定制域中仅为所选 Knative 服务分配主机名，请将 `data.selector` 标签键和值添加到配置映射。在此示例中，将从定制域中为具有标签 `app: sample` 的所有服务分配主机名。确保还具有要分配给没有 `app: sample` 标签的其他所有应用程序的域名。在此示例中，使用的是 IBM 提供的缺省域 `mycluster.us-south.containers.appdomain.cloud`。
    3. 保存更改。

Ingress 路由规则和 Knative 配置映射全部设置后，可以使用定制域和 TLS 证书来创建 Knative 服务。

## 从一个 Knative 服务访问另一个 Knative 服务
{: #knative-access-service}

可以通过对分配给 Knative 服务的 URL 进行 REST API 调用，从其他 Knative 服务访问您的 Knative 服务。
{: shortdesc}

1. 列出集群中的所有 Knative 服务。
   ```
   kubectl get ksvc --all-namespaces
   ```
   {: pre}

2. 检索分配给 Knative 服务的 **DOMAIN**。
   ```
   kubect get ksvc/<knative_service_name>
   ```
   {: pre}

   输出示例：
   ```
   NAME        DOMAIN                                                            LATESTCREATED     LATESTREADY       READY   REASON
   myservice   myservice.default.mycluster.us-south.containers.appdomain.cloud   myservice-rjmwt   myservice-rjmwt   True
   ```
   {: screen}

3. 使用域名来实现 REST API 调用以访问 Knative 服务。此 REST API 调用必须是为其创建 Knative 服务的应用程序的一部分。如果要访问的 Knative 服务分配有本地 URL（格式为 `<service_name>.<namespace>.svc.cluster.local`），那么 Knative 会在集群内部网络中保留 REST API 请求。

   使用 Go 编写的示例代码片段：
   ```go
   resp, err := http.Get("<knative_service_domain_name>")
   if err != nil {
       fmt.Fprintf(os.Stderr, "Error: %s\n", err)
   } else if resp.Body != nil {
       body, _ := ioutil.ReadAll(resp.Body)
       fmt.Printf("Response: %s\n", string(body))
   }
   ```
   {: codeblock}

## 常用 Knative 服务设置
{: #knative-service-settings}

查看常用 Knative 服务设置，在开发无服务器应用程序时，您可能会发现这些设置非常有用。
{: shortdesc}

- [设置最小和最大 pod 数](#knative-min-max-pods)
- [指定每个 pod 的最大请求数](#max-request-per-pod)
- [创建仅专用无服务器应用程序](#knative-private-only)
- [强制 Knative 服务重新拉取容器映像](#knative-repull-image)

### 设置最小和最大 pod 数
{: #knative-min-max-pods}

可以使用注释指定要为应用程序运行的最小和最大 pod 数。例如，如果您不希望 Knative 将应用程序缩减为零个实例，请将最小 pod 数设置为 1。
{: shortdesc}

```
apiVersion: serving.knative.dev/v1alpha1
kind: Service
metadata:
  name: ...
spec:
  runLatest:
    configuration:
      revisionTemplate:
        metadata:
          annotations:
            autoscaling.knative.dev/minScale: "1"
            autoscaling.knative.dev/maxScale: "100"
        spec:
          container:
            image:
...
```
{: codeblock}

<table>
<caption>了解 YAML 文件的组成部分</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解 YAML 文件的组成部分</th>
</thead>
<tbody>
<tr>
<td><code>autoscaling.knative.dev/minScale</code></td>
<td>输入要在集群中运行的最小 pod 数。Knative 无法将应用程序缩减为低于设置的数量，即使应用程序未收到任何网络流量时也是如此。缺省 pod 数为零。</td>
</tr>
<tr>
<td><code>autoscaling.knative.dev/maxScale</code></td>
<td>输入要在集群中运行的最大 pod 数。Knative 无法将应用程序扩展为高于设置的数量，即使您有当前应用程序实例可以处理的更多请求。</td>
</tr>
</tbody>
</table>

### 指定每个 pod 的最大请求数
{: #max-request-per-pod}

可以指定在 Knative 考虑扩展应用程序实例之前，应用程序实例可以接收和处理的最大请求数。例如，如果将最大请求数设置为 1，那么应用程序实例一次可以接收一个请求。如果第二个请求在第一个请求完全处理之前到达，那么 Knative 会扩展一个实例。

```
apiVersion: serving.knative.dev/v1alpha1
kind: Service
metadata:
  name: myservice
spec:
  runLatest:
    configuration:
      revisionTemplate:
        spec:
          container:
            image: myimage
          containerConcurrency: 1
....
```
{: codeblock}

<table>
<caption>了解 YAML 文件的组成部分</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解 YAML 文件的组成部分</th>
</thead>
<tbody>
<tr>
<td><code>containerConcurrency</code></td>
<td>输入在 Knative 考虑扩展应用程序实例之前，应用程序实例一次可以接收的最大请求数。</td>
</tr>
</tbody>
</table>

### 创建仅专用无服务器应用程序
{: #knative-private-only}

缺省情况下，系统会从 Istio Ingress 子域中为每个 Knative 服务分配公共路径，另外还会分配专用路径，格式为 `<service_name>.<namespace>.cluster.local`。可以使用公共路径通过公用网络来访问应用程序。如果要使服务保持专用，可以将 `serving.knative.dev/visibility` 标签添加到 Knative 服务。此标签指示 Knative 仅将专用主机名分配给您的服务。
{: shortdesc}

```
apiVersion: serving.knative.dev/v1alpha1
kind: Service
metadata:
  name: myservice
  labels:
    serving.knative.dev/visibility: cluster-local
spec:
  runLatest:
    configuration:
      revisionTemplate:
        spec:
          container:
            image:
...
```
{: codeblock}

<table>
<caption>了解 YAML 文件的组成部分</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解 YAML 文件的组成部分</th>
</thead>
<tbody>
<tr>
<td><code>serving.knative.dev/visibility</code></td>
  <td>如果添加 <code>serving.knative.dev/visibility: cluster-local</code> 标签，那么将仅为服务分配格式为 <code>&lt;service_name&gt;.&lt;namespace&gt;.cluster.local</code> 的专用路径。可以使用专用主机名从集群内访问服务，但无法通过公用网络访问服务。</td>
</tr>
</tbody>
</table>

### 强制 Knative 服务重新拉取容器映像
{: #knative-repull-image}

Knative 的当前实现未提供强制 Knative `Serving` 组件重新拉取容器映像的标准方法。要从注册表中重新拉取映像，请在以下选项之间进行选择：

- **修改 Knative 服务 `revisionTemplate`**：Knative 服务的 `revisionTemplate` 用于创建 Knative服务的修订版。如果修改了此修订版模板，例如添加 `repullFlag` 注释，那么 Knative 必须为应用程序创建新的修订版。在创建修订版的过程中，Knative 必须检查容器映像更新。设置 `imagePullPolicy: Always` 时，Knative 无法在集群中使用映像高速缓存，而是必须从容器注册表中拉取映像。
   ```
   apiVersion: serving.knative.dev/v1alpha1
   kind: Service
   metadata:
     name: <service_name>
   spec:
     runLatest:
       configuration:
         revisionTemplate:
           metadata:
             annotations:
               repullFlag: 123
           spec:
             container:
               image: <image_name>
               imagePullPolicy: Always
    ```
    {: codeblock}

    每次要创建服务的新修订版时，都必须更改 `repulFlag` 值，以从容器注册表中拉取最新的映像版本。请确保对每个修订版使用唯一值，以避免由于两个完全相同的 Knative 服务配置而导致 Knative 使用旧映像版本。  
    {: note}

- **使用标记来创建唯一容器映像**：可以对创建的每个容器映像使用唯一标记，并在 Knative 服务 `container.image` 配置中引用此映像。在以下示例中，`v1` 用作映像标记。要强制 Knative 从容器注册表中拉取新映像，必须更改映像标记。例如，使用 `v2` 作为新的映像标记。
  ```
  apiVersion: serving.knative.dev/v1alpha1
  kind: Service
  metadata:
    name: <service_name>
  spec:
    runLatest:
      configuration:
          spec:
            container:
              image: myapp:v1
              imagePullPolicy: Always
    ```
    {: codeblock}


## 相关链接  
{: #knative-related-links}

- 尝试通过 [Knative 研讨会 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/IBM/knative101/tree/master/workshop) 来了解如何将您的第一个 `Node.js` Fibonacci 应用程序部署到集群。
  - 探索如何使用 Knative `Build` 原语通过 GitHub 中的 Dockerfile 构建映像，并自动将该映像推送到 {{site.data.keyword.registrylong_notm}} 中的名称空间。  
  - 了解如何设置网络流量的路由，使其从 IBM 提供的 Ingress 子域流至 Knative 提供的 Istio Ingress 网关。
  - 应用新版本的应用程序，并使用 Istio 来控制路由到每个应用程序版本的流量。
- 探索 [Knative `Eventing` ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/knative/docs/tree/master/eventing/samples) 样本。
- 使用 [Knative 文档 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/knative/docs) 了解有关 Knative 的更多信息。
