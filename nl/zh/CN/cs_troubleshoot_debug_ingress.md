---

copyright:
  years: 2014, 2019
lastupdated: "2019-05-31"

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


# 调试 Ingress
{: #cs_troubleshoot_debug_ingress}

在使用 {{site.data.keyword.containerlong}} 时，请考虑对 Ingress 进行常规故障诊断和调试的以下方法。
{: shortdesc}

您已通过为集群中的应用程序创建 Ingress 资源来向公众公开应用程序。但尝试通过 ALB 的公共 IP 地址或子域连接到应用程序时，连接失败或超时。以下各部分中的步骤可帮助您调试 Ingress 设置。

确保一个主机仅在一个 Ingress 资源中进行定义。如果一个主机在多个 Ingress 资源中进行定义，那么 ALB 可能无法正确转发流量，并且您可能会遇到错误。
{: tip}

开始之前，请确保您具有以下 [{{site.data.keyword.Bluemix_notm}} IAM 访问策略](/docs/containers?topic=containers-users#platform)：
  - 对集群的**编辑者**或**管理员**平台角色
  - **写入者**或**管理者**服务角色

## 步骤 1：在 {{site.data.keyword.containerlong_notm}} 诊断和调试工具中运行 Ingress 测试

进行故障诊断时，可以使用 {{site.data.keyword.containerlong_notm}} 诊断和调试工具来运行 Ingress 测试并从集群收集相关 Ingress 信息。要使用调试工具，请安装 [`ibmcloud-iks-debug` Helm chart ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/kubernetes/helm/iks-charts/ibmcloud-iks-debug)：
{: shortdesc}


1. [在集群中设置 Helm，为 Tiller 创建服务帐户，然后将 `ibm` 存储库添加到 Helm 实例](/docs/containers?topic=containers-helm)。

2. 将 Helm chart 安装到集群。
  ```
  helm install iks-charts/ibmcloud-iks-debug --name debug-tool
  ```
  {: pre}


3. 启动代理服务器以显示调试工具接口。
  ```
  kubectl proxy --port 8080
  ```
  {: pre}

4. 在 Web 浏览器中，打开调试工具接口 URL：http://localhost:8080/api/v1/namespaces/default/services/debug-tool-ibmcloud-iks-debug:8822/proxy/page

5. 选择 **ingress** 测试组。一些测试检查是否存在潜在警告、错误或问题，一些测试仅收集在故障诊断期间可以参考的信息。有关每个测试的功能的更多信息，请单击测试名称旁边的“信息”图标。

6. 单击**运行**。

7. 检查每个测试的结果。
  * 如果任何测试失败，请单击左侧列中测试名称旁边的“信息”图标，以获取有关如何解决此问题的信息。
  * 对于仅用于在以下各部分中调试 Ingress 服务期间收集信息的测试，还可以使用这些测试的结果。

## 步骤 2：检查 Ingress 部署和 ALB pod 日志中的错误消息
{: #errors}

首先检查 Ingress 资源部署事件和 ALB pod 日志中的错误消息。这些错误消息可帮助您找到故障的根本原因，并在后续各部分中进一步调试 Ingress 设置。
{: shortdesc}

1. 检查 Ingress 资源部署，并查找警告或错误消息。
    ```
  kubectl describe ingress <myingress>
  ```
    {: pre}

    在输出的 **Events** 部分中，您可能会看到警告消息，提醒您所使用的 Ingress 资源或某些注释中有无效的值。请检查 [Ingress 资源配置文档](/docs/containers?topic=containers-ingress#public_inside_4)或[注释文档](/docs/containers?topic=containers-ingress_annotation)。

    ```
    Name:             myingress
    Namespace:        default
    Address:          169.xx.xxx.xxx,169.xx.xxx.xxx
    Default backend:  default-http-backend:80 (<none>)
    Rules:
      Host                                             Path  Backends      ----                                             ----  --------
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
2. 检查 ALB pod 的状态。
    1. 获取正在集群中运行的 ALB pod。
        ```
        kubectl get pods -n kube-system | grep alb
        ```
        {: pre}

    2. 通过检查 **STATUS** 列来确保所有 pod 都在运行。

    3. 如果 pod 的状态不为 `Running`，那么可以禁用并重新启用 ALB。在以下命令中，将 `<ALB_ID>` 替换为 pod 的 ALB 的标识。例如，如果未运行的 pod 名称为 `public-crb2f60e9735254ac8b20b9c1e38b649a5-alb1-5d6d86fbbc-kxj6z`，那么 ALB 标识为 `public-crb2f60e9735254ac8b20b9c1e38b649a5-alb1`。
        ```
        ibmcloud ks alb-configure --albID <ALB_ID> --disable
        ```
        {: pre}

        ```
        ibmcloud ks alb-configure --albID <ALB_ID> --enable
        ```
        {: pre}

3. 检查 ALB 的日志。
    1.  获取正在集群中运行的 ALB pod 的标识。
        ```
        kubectl get pods -n kube-system | grep alb
        ```
        {: pre}

    3. 获取每个 ALB pod 上 `nginx-ingress` 容器的日志。
        ```
      kubectl logs <ingress_pod_ID> nginx-ingress -n kube-system
      ```
        {: pre}

    4. 在 ALB 日志中查找错误消息。

## 步骤 3：对 ALB 子域和公共 IP 地址执行 ping 操作
{: #ping}

检查 Ingress 子域和 ALB 的公共 IP 地址的可用性。
{: shortdesc}

1. 获取公共 ALB 正在侦听的 IP 地址。
    ```
    ibmcloud ks albs --cluster <cluster_name_or_ID>
    ```
    {: pre}

    `dal10` 和 `dal13` 中具有工作程序节点的多专区集群的输出示例：

    ```
    ALB ID                                            Enabled   Status     Type      ALB IP          Zone    Build                          ALB VLAN ID
    private-cr24a9f2caf6554648836337d240064935-alb1   false     disabled   private   -               dal13   ingress:411/ingress-auth:315   2294021
    private-cr24a9f2caf6554648836337d240064935-alb2   false     disabled   private   -               dal10   ingress:411/ingress-auth:315   2234947
    public-cr24a9f2caf6554648836337d240064935-alb1    true      enabled    public    169.62.196.238  dal13   ingress:411/ingress-auth:315   2294019
    public-cr24a9f2caf6554648836337d240064935-alb2    true      enabled    public    169.46.52.222   dal10   ingress:411/ingress-auth:315   2234945
    ```
    {: screen}

    * 如果公共 ALB 没有 IP 地址，请参阅 [Ingress ALB 未部署在专区中](/docs/containers?topic=containers-cs_troubleshoot_network#cs_subnet_limit)。

2. 检查 ALB IP 的运行状况。

    * 对于单专区集群和多专区集群：对每个公共 ALB 的 IP 地址执行 ping 操作，以确保每个 ALB 都能够成功接收包。如果使用的是专用 ALB，那么只能从专用网络对其 IP 地址执行 ping 操作。
        ```
ping <ALB_IP>
      ```
        {: pre}

        * 如果 CLI 返回超时并且您具有保护工作程序节点的定制防火墙，请确保在[防火墙](/docs/containers?topic=containers-cs_troubleshoot_clusters#cs_firewall)中允许 ICMP。
        * 如果没有防火墙阻止 ping 操作，并且 ping 操作一直运行到超时，请[检查 ALB pod 的状态](#check_pods)。

    * 仅多专区集群：可以使用 MZLB 运行状况检查来确定 ALB IP 的阶段状态。有关 MZLB 的更多信息，请参阅[多专区负载均衡器 (MZLB)](/docs/containers?topic=containers-ingress#planning)。MZLB 运行状况检查仅可用于具有以下格式的新 Ingress 子域的集群：`<cluster_name>.<region_or_zone>.containers.appdomain.cloud`。如果集群使用的仍是旧格式 `<cluster_name>.<region>.containers.mybluemix.net`，请[将单专区集群转换为多专区集群](/docs/containers?topic=containers-add_workers#add_zone)。将为集群分配采用新格式的子域，但也可以继续使用较旧的子域格式。或者，可以对自动分配了新的子域格式的新集群进行排序。
    

    以下 HTTP cURL 命令使用 `albhealth` 主机，该主机由 {{site.data.keyword.containerlong_notm}} 配置为返回 ALB IP 的 `healthy` 或 `unhealthy` 阶段状态。
            ```
            curl -X GET http://169.62.196.238/ -H "Host: albhealth.mycluster-12345.us-south.containers.appdomain.cloud"
    ```
        {: pre}

        输出示例：
        ```
        运行正常
            ```
        {: screen}
            如果一个或多个 IP 返回 `unhealthy`，请[检查 ALB pod 的状态](#check_pods)。

3. 获取 IBM 提供的 Ingress 子域。
    ```
    ibmcloud ks cluster-get --cluster <cluster_name_or_ID> | grep Ingress
    ```
    {: pre}

    输出示例：
    ```
    Ingress Subdomain:      mycluster-12345.us-south.containers.appdomain.cloud
    Ingress Secret:         <tls_secret>
    ```
    {: screen}

4. 确保在此部分的步骤 2 中获取的每个公共 ALB 的 IP 都已向集群的 IBM 提供的 Ingress 子域注册。例如，在多专区集群中，具有工作程序节点的每个专区中的公共 ALB IP 必须在同一主机名下注册。

    ```
    kubectl get ingress -o wide
    ```
    {: pre}

    输出示例：
    ```
    NAME                HOSTS                                                    ADDRESS                        PORTS     AGE
    myingressresource   mycluster-12345.us-south.containers.appdomain.cloud      169.46.52.222,169.62.196.238   80        1h
    ```
    {: screen}

## 步骤 4：检查域映射和 Ingress 资源配置
{: #ts_ingress_config}

1. 如果使用定制域，请验证是否使用了 DNS 提供程序将该定制域映射到 IBM 提供的子域或 ALB 的公共 IP 地址。请注意，使用 CNAME 是首选项，因为 IBM 会在 IBM 子域上提供自动运行状况检查，并从 DNS 响应中除去任何失败的 IP。
    * IBM 提供的子域：检查定制域是否映射到规范名称记录 (CNAME) 中集群的 IBM 提供的子域。
        ```
        host www.my-domain.com
        ```
        {: pre}

        输出示例：
        ```
        www.my-domain.com is an alias for mycluster-12345.us-south.containers.appdomain.cloud
        mycluster-12345.us-south.containers.appdomain.cloud has address 169.46.52.222
        mycluster-12345.us-south.containers.appdomain.cloud has address 169.62.196.238
        ```
        {: screen}

    * 公共 IP 地址：检查定制域是否映射到 A 记录中 ALB 的可移植公共 IP 地址。这些 IP 应该与在[上一部分](#ping)的步骤 1 中获取的公共 ALB IP 相匹配。
        ```
        host www.my-domain.com
        ```
        {: pre}

        输出示例：
        ```
        www.my-domain.com has address 169.46.52.222
        www.my-domain.com has address 169.62.196.238
        ```
        {: screen}

2. 检查集群的 Ingress 资源配置文件。
    ```
    kubectl get ingress -o yaml
    ```
    {: pre}

    1. 确保一个主机仅在一个 Ingress 资源中进行定义。如果一个主机在多个 Ingress 资源中进行定义，那么 ALB 可能无法正确转发流量，并且您可能会遇到错误。


    2. 检查子域和 TLS 证书是否正确。要查找 IBM 提供的 Ingress 子域和 TLS 证书，请运行 `ibmcloud ks cluster-get --cluster <cluster_name_or_ID>`。

    3.  确保应用程序侦听的是在 Ingress 的 **path** 部分中配置的路径。如果应用程序设置为侦听根路径，请使用 `/` 作为路径。如果流至此路径的入局流量必须路由到应用程序侦听的其他路径，请使用[重写路径](/docs/containers?topic=containers-ingress_annotation#rewrite-path)注释。

    4. 根据需要编辑资源配置 YAML。关闭编辑器时，会保存并自动应用更改。
        ```
        kubectl edit ingress <myingressresource>
        ```
        {: pre}

## 从 DNS 除去 ALB 以进行调试
{: #one_alb}

如果无法通过特定 ALB IP 访问应用程序，那么可以通过禁用 ALB 的 DNS 注册来临时从生产环境除去 ALB。然后，可以使用该 ALB 的 IP 地址对该 ALB 运行调试测试。

例如，假设在 2 个专区中有一个多专区集群，并且 2 个公共 ALB 具有 IP 地址 `169.46.52.222` 和 `169.62.196.238`。尽管运行状况检查对于第二个专区的 ALB 返回 healthy，但仍然无法直接通过它访问应用程序。为此，您决定从生产环境中除去 ALB 的 IP 地址 `169.62.196.238`，以进行调试。第一个专区的 ALB IP `169.46.52.222` 已向域注册，并且在您调试第二个专区的 ALB 时，会继续路由流量。

1. 获取具有不可访问 IP 地址的 ALB 的名称。
    ```
    ibmcloud ks albs --cluster <cluster_name> | grep <ALB_IP>
    ```
    {: pre}

    例如，不可访问的 IP `169.62.196.238` 属于 ALB `public-cr24a9f2caf6554648836337d240064935-alb1`：
    ```
    ALB ID                                            Enabled   Status     Type      ALB IP           Zone    Build                          ALB VLAN ID
    public-cr24a9f2caf6554648836337d240064935-alb1    false     disabled   private   169.62.196.238   dal13   ingress:411/ingress-auth:315   2294021
    ```
    {: screen}

2. 使用上一步中的 ALB 名称来获取 ALB pod 的名称。以下命令使用上一步中的示例 ALB 名称：
    ```
    kubectl get pods -n kube-system | grep public-cr24a9f2caf6554648836337d240064935-alb1
    ```
    {: pre}

    输出示例：
    ```
    public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq   2/2       Running   0          24m
    public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-trqxc   2/2       Running   0          24m
    ```
    {: screen}

3. 禁用对所有 ALB pod 运行的运行状况检查。对上一步中获得的每个 ALB pod 重复这些步骤。这些步骤中的示例命令和输出使用的是第一个 pod `public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq`。
    1. 登录到 ALB pod，并检查 NGINX 配置文件中的 `server_name` 行。
        ```
        kubectl exec -ti public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq -n kube-system -c nginx-ingress -- grep server_name /etc/nginx/conf.d/kube-system-alb-health.conf
        ```
        {: pre}

        以下输出示例确认 ALB pod 已配置了正确的运行状况检查主机名 `albhealth.<domain>`：
        ```
        server_name albhealth.mycluster-12345.us-south.containers.appdomain.cloud;
        ```
        {: screen}

    2. 要通过禁用运行状况检查来除去此 IP，请在 `server_name` 前面插入 `#`。禁用 ALB 的 `albhealth.mycluster-12345.us-south.containers.appdomain.cloud` 虚拟主机后，自动运行状况检查会自动从 DNS 响应中除去相应 IP。
        ```
        kubectl exec -ti public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq -n kube-system -c nginx-ingress -- sed -i -e 's*server_name*#server_name*g' /etc/nginx/conf.d/kube-system-alb-health.conf
        ```
        {: pre}

    3. 验证是否已应用更改。
        ```
        kubectl exec -ti public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq -n kube-system -c nginx-ingress -- grep server_name /etc/nginx/conf.d/kube-system-alb-health.conf
        ```
        {: pre}

        输出示例：
        ```
        #server_name albhealth.mycluster-12345.us-south.containers.appdomain.cloud
        ```
        {: screen}

    4. 要从 DNS 注册中除去 IP，请重新装入 NGINX 配置。
        ```
        kubectl exec -ti public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq -n kube-system -c nginx-ingress -- nginx -s reload
        ```
        {: pre}

    5. 对每个 ALB pod 重复这些步骤。

4. 现在，尝试通过 cURL 使 `albhealth` 主机对 ALB IP 进行运行状况检查时，检查会失败。
    ```
    curl -X GET http://169.62.196.238/ -H "Host: albhealth.mycluster-12345.us-south.containers.appdomain.cloud"
    ```
    {: pre}

    输出：
    ```
    <html>
        <head>
            <title>404 找不到</title>
        </head>
        <body bgcolor="white"><center>
            <h1>404 找不到</h1>
        </body>
    </html>
    ```
    {: screen}

5. 通过检查 Cloudflare 服务器，验证是否已从域的 DNS 注册中除去 ALB IP 地址。请注意，DNS 注册可能需要几分钟时间来更新。
    ```
    host mycluster-12345.us-south.containers.appdomain.cloud ada.ns.cloudflare.com
    ```
    {: pre}

    以下输出示例确认仅运行正常的 ALB IP `169.46.52.222` 保留在 DNS 注册中，运行状况欠佳的 ALB IP `169.62.196.238` 已除去：
    ```
    mycluster-12345.us-south.containers.appdomain.cloud has address 169.46.52.222
    ```
    {: screen}

6. 现在已从生产环境中除去该 ALB IP，因此可以通过该 IP 对应用程序运行调试测试。要通过此 IP 测试与应用程序的通信，可以运行以下 cURL 命令，并将示例值替换为您自己的值：
    ```
    curl -X GET --resolve mycluster-12345.us-south.containers.appdomain.cloud:443:169.62.196.238 https://mycluster-12345.us-south.containers.appdomain.cloud/
    ```
    {: pre}

    * 如果已正确配置所有内容，那么您将获得从应用程序返回的预期响应。
    * 如果在响应中获得错误，说明应用程序中或仅适用于此特定 ALB 的配置中可能存在错误。请检查应用程序代码、[Ingress 资源配置文件](/docs/containers?topic=containers-ingress#public_inside_4)或仅应用于此 ALB 的其他任何配置。

7. 完成调试后，请对 ALB pod 复原运行状况检查。对每个 ALB pod 重复这些步骤。
  1. 登录到 ALB pod，并从 `server_name` 中除去 `#`。
    ```
    kubectl exec -ti <pod_name> -n kube-system -c nginx-ingress -- sed -i -e 's*#server_name*server_name*g' /etc/nginx/conf.d/kube-system-alb-health.conf
    ```
    {: pre}

  2. 重新装入 NGINX 配置，以便应用运行状况检查复原。
     ```
    kubectl exec -ti <pod_name> -n kube-system -c nginx-ingress -- nginx -s reload
    ```
    {: pre}

9. 现在，通过 cURL 使 `albhealth` 主机对 ALB IP 进行运行状况检查时，检查会返回 `healthy`。
    ```
    curl -X GET http://169.62.196.238/ -H "Host: albhealth.mycluster-12345.us-south.containers.appdomain.cloud"
    ```
    {: pre}

10. 通过检查 Cloudflare 服务器，验证该 ALB IP 地址是否已在域的 DNS 注册中复原。请注意，DNS 注册可能需要几分钟时间来更新。
    ```
    host mycluster-12345.us-south.containers.appdomain.cloud ada.ns.cloudflare.com
    ```
    {: pre}

    输出示例：
    ```
    mycluster-12345.us-south.containers.appdomain.cloud has address 169.46.52.222
    mycluster-12345.us-south.containers.appdomain.cloud has address 169.62.196.238
    ```
    {: screen}

<br />


## 获取帮助和支持
{: #ingress_getting_help}

集群仍然有问题吗？
{: shortdesc}

-  在终端中，当有 `ibmcloud` CLI 和插件的更新可用时，您会收到通知。务必使 CLI 保持最新，以便您可以使用所有可用的命令和标志。
-   要查看 {{site.data.keyword.Bluemix_notm}} 是否可用，请[检查 {{site.data.keyword.Bluemix_notm}} 状态页面 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/status?selected=status)。
-   在 [{{site.data.keyword.containerlong_notm}} Slack ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://ibm-container-service.slack.com) 中发布问题。
    如果未将 IBM 标识用于 {{site.data.keyword.Bluemix_notm}} 帐户，请针对此 Slack [请求邀请](https://bxcs-slack-invite.mybluemix.net/)。
    {: tip}
-   请复查论坛，以查看是否有其他用户遇到相同的问题。使用论坛进行提问时，请使用适当的标记来标注您的问题，以方便 {{site.data.keyword.Bluemix_notm}} 开发团队识别。
    -   如果您有关于使用 {{site.data.keyword.containerlong_notm}} 开发或部署集群或应用程序的技术问题，请在 [Stack Overflow ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://stackoverflow.com/questions/tagged/ibm-cloud+containers) 上发布您的问题，并使用 `ibm-cloud`、`kubernetes` 和 `containers` 标记您的问题。
    -   有关服务的问题和入门指示信息，请使用 [IBM Developer Answers ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix) 论坛。请加上 `ibm-cloud` 和 `containers` 标记。
    有关使用论坛的更多详细信息，请参阅[获取帮助](/docs/get-support?topic=get-support-getting-customer-support#using-avatar)。
-   通过打开案例来联系 IBM 支持人员。要了解有关打开 IBM 支持案例或有关支持级别和案例严重性的信息，请参阅[联系支持人员](/docs/get-support?topic=get-support-getting-customer-support)。报告问题时，请包含集群标识。要获取集群标识，请运行 `ibmcloud ks clusters`。您还可以使用 [{{site.data.keyword.containerlong_notm}} 诊断和调试工具](/docs/containers?topic=containers-cs_troubleshoot#debug_utility)从集群收集相关信息并导出这些信息，以便与 IBM 支持人员共享。
{: tip}

