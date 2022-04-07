---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: kubernetes, iks, helm

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

# 使用受管附加组件添加服务
{: #managed-addons}

使用受管附加组件可快速向集群添加开放式源代码技术。
{: shortdesc}

**什么是受管附加组件？**</br>
通过受管 {{site.data.keyword.containerlong_notm}} 附加组件，可以轻松使用开放式源代码功能（如 Istio 或 Knative）来增强集群功能。您添加到集群的开放式源代码工具版本由 IBM 进行测试，并核准在 {{site.data.keyword.containerlong_notm}} 中使用。

**计费和支持如何适用于受管附加组件？**</br>
受管附加组件完全集成到 {{site.data.keyword.Bluemix_notm}} 支持组织中。如果您在使用受管附加组件时有疑问或遇到问题，可以使用其中一个 {{site.data.keyword.containerlong_notm}} 支持通道。有关更多信息，请参阅[获取帮助和支持](/docs/containers?topic=containers-cs_troubleshoot_clusters#clusters_getting_help)。

如果添加到集群的工具发生了成本，这些成本会自动合并，并列入 {{site.data.keyword.containerlong_notm}} 计费中。计费周期由 {{site.data.keyword.Bluemix_notm}} 根据您在集群中启用附加组件的时间来确定。

**我需要考虑哪些限制？**</br>
如果在集群中安装了[容器映像安全性强制实施程序许可控制器](/docs/services/Registry?topic=registry-security_enforce#security_enforce)，那么无法在集群中启用受管附加组件。

## 添加受管附加组件
{: #adding-managed-add-ons}

要在集群中启用受管附加组件，请使用 [`ibmcloud ks cluster-addon-enable <addon_name> --cluster <cluster_name_or_ID>` 命令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_addon_enable)。启用受管附加组件时，支持的工具版本（包括所有 Kubernetes 资源）将自动安装在集群中。请参阅每个受管附加组件的文档，以了解集群必须满足才能安装受管附加组件的先决条件。

有关每个附加组件的先决条件的更多信息，请参阅：

- [Istio](/docs/containers?topic=containers-istio#istio)
- [Knative](/docs/containers?topic=containers-serverless-apps-knative)

## 更新受管附加组件
{: #updating-managed-add-ons}

每个受管附加组件的各个版本都由 {{site.data.keyword.Bluemix_notm}} 进行测试，并核准在 {{site.data.keyword.containerlong_notm}} 中使用。要将附加组件的组件更新为 {{site.data.keyword.containerlong_notm}} 支持的最新版本，请使用以下步骤。
{: shortdesc}

1. 检查附加组件是否为最新版本。可以更新使用 `* (<version> latest)` 表示的任何附加组件。
   ```
   ibmcloud ks cluster-addons --cluster <mycluster>
   ```
   {: pre}

   输出示例：
   ```
   OK
   Name      Version
   istio     1.1.5
   knative   0.5.2
   ```
   {: screen}

2. 将创建或修改的任何资源（如任何服务或应用程序的配置文件）保存在附加组件生成的名称空间中。例如，Istio 附加组件使用的是 `istio-system`，Knative 附加组件使用的是 `knative-serving`、`knative-monitoring`、`knative-eventing` 和 `knative-build`。
   示例命令：
   ```
   kubectl get pod <pod_name> -o yaml -n istio-system
   ```
   {: pre}

3. 将在受管附加组件的名称空间中自动生成的 Kubernetes 资源保存到本地计算机上的 YAML 文件中。这些资源是使用定制资源定义 (CRD) 生成的。
   1. 从附加组件使用的名称空间中获取附加组件的 CRD。例如，对于 Istio 附加组件，请从 `istio-system` 名称空间获取 CRD。
      ```
      kubectl get crd -n istio-system
      ```
      {: pre}

   2. 保存通过这些 CRD 创建的任何资源。

4. 对于 Knative 为可选：如果修改了以下任何资源，请获取 YAML 文件并将其保存到本地计算机。如果修改了其中任何资源，但希望改为使用安装的缺省值，那么可以删除该资源。几分钟后，该资源会使用安装的缺省值进行重新创建。
  <table summary="Knative 资源表">
  <caption>Knative 资源</caption>
  <thead><tr><th>资源名称</th><th>资源类型</th><th>名称空间</th></tr></thead>
  <tbody>
  <tr><td><code>config-autoscaler</code></td><td>ConfigMap</td><td><code>knative-serving</code></td></tr>
  <tr><td><code>config-controller</code></td><td>ConfigMap</td><td><code>knative-serving</code></td></tr>
  <tr><td><code>config-domain</code></td><td>ConfigMap</td><td><code>knative-serving</code></td></tr>
  <tr><td><code>config-gc</code></td><td>ConfigMap</td><td><code>knative-serving</code></td></tr>
  <tr><td><code>config-istio</code></td><td>ConfigMap</td><td><code>knative-serving</code></td></tr>
  <tr><td><code>config-logging</code></td><td>ConfigMap</td><td><code>knative-serving</code></td></tr>
  <tr><td><code>config-network</code></td><td>ConfigMap</td><td><code>knative-serving</code></td></tr>
  <tr><td><code>config-observability</code></td><td>ConfigMap</td><td><code>knative-serving</code></td></tr>
  <tr><td><code>kaniko</code></td><td>BuildTemplate</td><td><code>default</code></td></tr>
  <tr><td><code>iks-knative-ingress</code></td><td>Ingress</td><td><code>istio-system</code></td></tr>
  </tbody></table>

  示例命令：
  ```
  kubectl get cm config-autoscaler -n knative-serving -o yaml > config-autoscaler.yaml
  ```
  {: pre}

5. 如果启用了 `istio-sample-bookinfo` 和 `istio-extras` 附加组件，请将其禁用。
   1. 禁用 `istio-sample-bookinfo` 附加组件。
      ```
      ibmcloud ks cluster-addon-disable istio-sample-bookinfo --cluster <cluster_name_or_ID>
      ```
      {: pre}

   2. 禁用 `istio-extras` 附加组件。
      ```
      ibmcloud ks cluster-addon-disable istio-extras --cluster <cluster_name_or_ID>
      ```
      {: pre}

6. 禁用附加组件。
   ```
   ibmcloud ks cluster-addon-disable <add-on_name> --cluster <cluster_name_or_ID> -f
   ```
   {: pre}

7. 在继续执行下一步之前，请验证是除去了附加组件名称空间内附加组件中的资源，还是除去了附加组件名称空间本身。
   * 例如，如果更新了 `istio-extras` 附加组件，那么可以验证是否从 `istio-system` 名称空间中除去了 `grafana`、`kiali` 和 `jaeger-*` 服务。
     ```
   kubectl get svc -n istio-system
   ```
     {: pre}
   * 例如，如果更新了 `knative` 附加组件，那么可以验证是否删除了 `knative-serving`、`knative-monitoring`、`knative-eventing`、`knative-build` 和 `istio-system` 名称空间。名称空间在被删除前的几分钟内，其 **STATUS** 可能为 `Terminating`。
     ```
     kubectl get namespaces -w
     ```
     {: pre}

8. 选择要更新到的附加组件版本。
   ```
   ibmcloud ks addon-versions
   ```
   {: pre}

9. 重新启用附加组件。使用 `--version` 标志指定要安装的版本。如果未指定版本，那么将安装缺省版本。
   ```
   ibmcloud ks cluster-addon-enable <add-on_name> --cluster <cluster_name_or_ID> --version <version>
   ```
   {: pre}

10. 应用在步骤 2 中保存的 CRD 资源。
    ```
    kubectl apply -f <file_name> -n <namespace>
    ```
    {: pre}

11. 对于 Knative 为可选：如果在步骤 3 中保存了任何资源，请重新应用这些资源。
    示例命令：
    ```
    kubectl apply -f config-autoscaler.yaml -n knative-serving
    ```
    {: pre}

12. 对于 Istio 为可选：重新启用 `istio-extras` 和 `istio-sample-bookinfo` 附加组件。对于这些附加组件，请使用与 `istio` 附加组件相同的版本。
    1. 启用 `istio-extras` 附加组件。
       ```
       ibmcloud ks cluster-addon-enable istio-extras --cluster <cluster_name_or_ID> --version <version>
       ```
       {: pre}

    2. 启用 `istio-sample-bookinfo` 附加组件。
       ```
       ibmcloud ks cluster-addon-enable istio-sample-bookinfo --cluster <cluster_name_or_ID> --version <version>
       ```
       {: pre}

13. 对于 Istio 为可选：如果在网关配置文件中使用 TLS 部分，那么必须删除并重新创建网关，以便 Envoy 可以访问这些私钥。
  ```
  kubectl delete gateway mygateway
  kubectl create -f mygateway.yaml
  ```
  {: pre}
