---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: kubernetes, iks, helm, without tiller, private cluster tiller, integrations, helm chart

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



# 使用 Helm chart 添加服务
{: #helm}

您可以使用 Helm chart 向集群添加复杂的 Kubernetes 应用程序。
{: shortdesc}

**什么是 Helm？如何使用 Helm？**</br>
[Helm ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://helm.sh) 是一种 Kubernetes 软件包管理器，使用 Helm chart 来定义、安装和升级集群中的复杂 Kubernetes 应用程序。Helm chart 用于对规范打包，这些规范可为构建应用程序的 Kubernetes 资源生成 YAML 文件。这些 Kubernetes 资源会在集群中自动应用，并由 Helm 分配版本。您还可以使用 Helm 来指定和打包您自己的应用程序，并让 Helm 为 Kubernetes 资源生成 YAML 文件。  

要在集群中使用 Helm，必须在本地计算机上安装 Helm CLI，并在要使用 Helm 的每个集群中安装 Helm 服务器 Tiller。

**{{site.data.keyword.containerlong_notm}} 中支持哪些 Helm chart？**</br>
有关可用 Helm chart 的概述，请参阅 [Helm chart 目录 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/kubernetes/solutions/helm-charts)。此目录中列出的 Helm chart 分组如下：

- **iks-charts**：核准用于 {{site.data.keyword.containerlong_notm}} 的 Helm chart。此存储库的名称已从 `ibm` 更改为 `iks-charts`。
- **ibm-charts**：核准用于 {{site.data.keyword.containerlong_notm}} 和 {{site.data.keyword.Bluemix_notm}} Private 集群的 Helm chart。
- **kubernetes**：由 Kubernetes 社区提供，并被该社区监管视为`稳定`的 Helm chart。这些 chart 是否适合用于 {{site.data.keyword.containerlong_notm}} 或 {{site.data.keyword.Bluemix_notm}} Private 集群并未经过验证。
- **kubernetes-incubator**：由 Kubernetes 社区提供，并被该社区监管视为`孵化器`的 Helm chart。这些 chart 是否适合用于 {{site.data.keyword.containerlong_notm}} 或 {{site.data.keyword.Bluemix_notm}} Private 集群并未经过验证。

**iks-charts** 和 **ibm-charts** 存储库中的 Helm chart 已完全集成到 {{site.data.keyword.Bluemix_notm}} 支持组织中。如果您在使用这些 Helm chart 时有疑问或遇到问题，可以使用其中一个 {{site.data.keyword.containerlong_notm}} 支持通道。有关更多信息，请参阅[获取帮助和支持](/docs/containers?topic=containers-cs_troubleshoot_clusters#clusters_getting_help)。

**使用 Helm 的先决条件是什么？可以在专用集群中使用 Helm 吗？**</br>
要部署 Helm chart，必须在本地计算机上安装 Helm CLI，并在集群中安装 Helm 服务器 Tiller。Tiller 的映像存储在公共 Google Container Registry 中。要在 Tiller 安装期间访问该映像，集群必须允许与公共 Google Container Registry 的公用网络连接。启用了公共服务端点的集群可以自动访问该映像。使用定制防火墙进行保护的专用集群或仅启用了专用服务端点的集群不允许访问 Tiller 映像。您可以改为[将映像拉取到本地计算机，并将映像推送到 {{site.data.keyword.registryshort_notm}} 中的名称空间](#private_local_tiller)，或者[安装不使用 Tiller 的 Helm chart](#private_install_without_tiller)。



## 在具有公共访问权的集群中设置 Helm
{: #public_helm_install}

如果集群启用了公共服务端点，那么可以使用 Google Container Registry 中的公共映像来安装 Helm 服务器 Tiller。
{: shortdesc}

开始之前：
- [登录到您的帐户。如果适用，请将相应的资源组设定为目标。为集群设置上下文。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
- 要在 `kube-system` 名称空间中使用 Kubernetes 服务帐户和集群角色绑定来安装 Tiller，请确保您具有 [`cluster-admin` 角色](/docs/containers?topic=containers-users#access_policies)。

要在具有公共访问权的集群中安装 Helm，请执行以下操作：

1. 在本地计算机上安装 <a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">Helm CLI <img src="../icons/launch-glyph.svg" alt="外部链接图标"></a>。

2. 检查是否已在集群中使用 Kubernetes 服务帐户安装 Tiller。
   ```
   kubectl get serviceaccount --all-namespaces | grep tiller
   ```
   {: pre}

   安装了 Tiller 时的输出示例：
   ```
   kube-system      tiller                               1         189d
   ```
   {: screen}

   输出示例包含 Kubernetes 名称空间和 Tiller 服务帐户的名称。如果 Tiller 不是使用集群中的服务帐户安装的，那么不会返回 CLI 输出。

3. **重要信息**：要维护集群安全性，请使用集群中的服务帐户和集群角色绑定来设置 Tiller。
   - **如果使用服务帐户安装了 Tiller：**
     1. 为 Tiller 服务帐户创建集群角色绑定。将 `<namespace>` 替换为集群中安装了 Tiller 的名称空间。
        ```
        kubectl create clusterrolebinding tiller --clusterrole=cluster-admin --serviceaccount=<namespace>:tiller -n <namespace>
        ```
        {: pre}

     2. 更新 Tiller。将 `<tiller_service_account_name>` 替换为在上一步中检索到的用于 Tiller 的 Kubernetes 服务帐户。
        ```
        helm init --upgrade --service-account <tiller_service_account_name>
        ```
        {: pre}

     3. 验证 `tiller-deploy` pod 在集群中的 **Status** 是否为 `Running`。
        ```
        kubectl get pods -n <namespace> -l app=helm
        ```
        {: pre}

        输出示例：

        ```
    NAME                            READY     STATUS    RESTARTS   AGE
    tiller-deploy-352283156-nzbcm   1/1       Running   0          2m
    ```
        {: screen}

   - **如果未使用服务帐户安装 Tiller：**
     1. 在集群的 `kube-system` 名称空间中为 Tiller 创建 Kubernetes 服务帐户和集群角色绑定。
        ```
        kubectl create serviceaccount tiller -n kube-system
        ```
        {: pre}

        ```
        kubectl create clusterrolebinding tiller --clusterrole=cluster-admin --serviceaccount=kube-system:tiller -n kube-system
        ```
        {: pre}

     2. 验证 Tiller 服务帐户是否已创建。
        ```
        kubectl get serviceaccount -n kube-system tiller
        ```
        {: pre}

        输出示例：
        ```
                NAME                                 SECRETS   AGE
        tiller                               1         2m
        ```
        {: screen}

     3. 使用创建的服务帐户来初始化 Helm CLI 并在集群中安装 Tiller。
        ```
    helm init --service-account tiller
    ```
        {: pre}

     4. 验证 `tiller-deploy` pod 在集群中的 **Status** 是否为 `Running`。
        ```
        kubectl get pods -n kube-system -l app=helm
        ```
        {: pre}

        输出示例：
        ```
            NAME                            READY     STATUS    RESTARTS   AGE
    tiller-deploy-352283156-nzbcm   1/1       Running   0          2m
    ```
        {: screen}

4. 向 Helm 实例添加 {{site.data.keyword.Bluemix_notm}} Helm 存储库。
   ```
   helm repo add iks-charts https://icr.io/helm/iks-charts
   ```
   {: pre}

   ```
   helm repo add ibm-charts https://icr.io/helm/ibm-charts
   ```
   {: pre}

5. 更新存储库以检索所有 Helm chart 的最新版本。
   ```
        helm repo update
        ```
   {: pre}

6. 列出 {{site.data.keyword.Bluemix_notm}} 存储库中当前可用的 Helm chart。
   ```
   helm search iks-charts
   ```
   {: pre}

   ```
    helm search ibm-charts
    ```
   {: pre}

7. 确定要安装的 Helm chart，并遵循该 Helm chart `README` 中的指示信息在集群中安装 Helm chart。


## 专用集群：将 Tiller 映像推送到 IBM Cloud Container Registry 中的名称空间
{: #private_local_tiller}

您可以将 Tiller 映像拉取到本地计算机，将该映像推送到 {{site.data.keyword.registryshort_notm}} 中的名称空间，然后使用 {{site.data.keyword.registryshort_notm}} 中的映像在专用集群中安装 Tiller。
{: shortdesc}

如果要在不使用 Tiller 的情况下安装 Helm chart，请参阅[专用集群：安装 Helm chart（不使用 Tiller）](#private_install_without_tiller)。
{: tip}

开始之前：
- 在本地计算机上安装 Docker。如果已安装 [{{site.data.keyword.Bluemix_notm}} CLI](/docs/cli?topic=cloud-cli-getting-started)，那么 Docker 已经安装。
- [安装 {{site.data.keyword.registryshort_notm}} CLI 插件并设置名称空间](/docs/services/Registry?topic=registry-getting-started#gs_registry_cli_install)。
- 要在 `kube-system` 名称空间中使用 Kubernetes 服务帐户和集群角色绑定来安装 Tiller，请确保您具有 [`cluster-admin` 角色](/docs/containers?topic=containers-users#access_policies)。

要使用 {{site.data.keyword.registryshort_notm}} 安装 Tiller，请执行以下操作：

1. 在本地计算机上安装 <a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">Helm CLI <img src="../icons/launch-glyph.svg" alt="外部链接图标"></a>。
2. 使用设置的 {{site.data.keyword.Bluemix_notm}} 基础架构 VPN 隧道连接到专用集群。
3. **重要信息**：要维护集群安全性，请通过应用 [{{site.data.keyword.Bluemix_notm}} `kube-samples` 存储库](https://github.com/IBM-Cloud/kube-samples/blob/master/rbac/serviceaccount-tiller.yaml)中的以下 YAML 文件，在 `kube-system` 名称空间中为 Tiller 创建服务帐户，为 `tiller-deploy` pod 创建 Kubernetes RBAC 集群角色绑定。
    1. [获取 Kubernetes 服务帐户和集群角色绑定 YAML 文件 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/rbac/serviceaccount-tiller.yaml)。

    2. 在集群中创建 Kubernetes 资源。
       ```
       kubectl apply -f service-account.yaml
       ```
       {: pre}

       ```
       kubectl apply -f cluster-role-binding.yaml
       ```
       {: pre}

4. [查找 Tiller 版本 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://console.cloud.google.com/gcr/images/kubernetes-helm/GLOBAL/tiller?gcrImageListsize=30)，即要在集群中安装的 Tiller 版本。如果不需要特定版本，请使用最新版本。

5. 将 Tiller 映像从公共 Google Container Registry 拉取到本地计算机。
   ```
   docker pull gcr.io/kubernetes-helm/tiller:v2.11.0
   ```
   {: pre}

   输出示例：
   ```
   docker pull gcr.io/kubernetes-helm/tiller:v2.13.0
   v2.13.0: Pulling from kubernetes-helm/tiller
   48ecbb6b270e: Pull complete
   d3fa0712c71b: Pull complete
   bf13a43b92e9: Pull complete
   b3f98be98675: Pull complete
   Digest: sha256:c4bf03bb67b3ae07e38e834f29dc7fd43f472f67cad3c078279ff1bbbb463aa6
   Status: Downloaded newer image for gcr.io/kubernetes-helm/tiller:v2.13.0
   ```
   {: screen}

6. [将 Tiller 映像推送到 {{site.data.keyword.registryshort_notm}} 中的名称空间](/docs/services/Registry?topic=registry-getting-started#gs_registry_images_pushing)。

7. 要从集群内部访问 {{site.data.keyword.registryshort_notm}} 中的映像，请[将映像拉取私钥从 default 名称空间复制到 `kube-system` 名称空间](/docs/containers?topic=containers-images#copy_imagePullSecret)。

8. 使用存储在 {{site.data.keyword.registryshort_notm}} 的名称空间中的映像，在专用集群中安装 Tiller。
   ```
   helm init --tiller-image <region>.icr.io/<mynamespace>/<myimage>:<tag> --service-account tiller
   ```
   {: pre}

9. 向 Helm 实例添加 {{site.data.keyword.Bluemix_notm}} Helm 存储库。
   ```
   helm repo add iks-charts https://icr.io/helm/iks-charts
   ```
   {: pre}

   ```
   helm repo add ibm-charts https://icr.io/helm/ibm-charts
   ```
   {: pre}

10. 更新存储库以检索所有 Helm chart 的最新版本。
    ```
        helm repo update
        ```
    {: pre}

11. 列出 {{site.data.keyword.Bluemix_notm}} 存储库中当前可用的 Helm chart。
    ```
    helm search iks-charts
    ```
    {: pre}

    ```
    helm search ibm-charts
    ```
    {: pre}

12. 确定要安装的 Helm chart，并遵循该 Helm chart `README` 中的指示信息在集群中安装 Helm chart。


## 专用集群：安装不使用 Tiller 的 Helm chart
{: #private_install_without_tiller}

如果您不想在专用集群中安装 Tiller，那么可以使用 `kubectl` 命令手动创建 Helm chart YAML 文件并应用这些文件。
{: shortdesc}

此示例中的步骤显示了如何在专用集群中安装 {{site.data.keyword.Bluemix_notm}} Helm chart 存储库中的 Helm chart。如果要安装的 Helm chart 未存储在其中一个 {{site.data.keyword.Bluemix_notm}} Helm chart 存储库中，那么必须遵循本主题中的指示信息为 Helm chart 创建 YAML 文件。此外，必须从公共容器注册表下载 Helm chart 映像，将其推送到 {{site.data.keyword.registryshort_notm}} 中的名称空间，然后更新 `values.yaml` 文件才能在 {{site.data.keyword.registryshort_notm}} 中使用该映像。
{: note}

1. 在本地计算机上安装 <a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">Helm CLI <img src="../icons/launch-glyph.svg" alt="外部链接图标"></a>。
2. 使用设置的 {{site.data.keyword.Bluemix_notm}} 基础架构 VPN 隧道连接到专用集群。
3. 向 Helm 实例添加 {{site.data.keyword.Bluemix_notm}} Helm 存储库。
   ```
   helm repo add iks-charts https://icr.io/helm/iks-charts
   ```
   {: pre}

   ```
   helm repo add ibm-charts https://icr.io/helm/ibm-charts
   ```
   {: pre}

4. 更新存储库以检索所有 Helm chart 的最新版本。
   ```
        helm repo update
        ```
   {: pre}

5. 列出 {{site.data.keyword.Bluemix_notm}} 存储库中当前可用的 Helm chart。
   ```
   helm search iks-charts
   ```
   {: pre}

   ```
    helm search ibm-charts
    ```
   {: pre}

6. 确定要安装的 Helm chart，将 Helm chart 下载到本地计算机，然后解压缩 Helm chart 的文件。以下示例显示了如何下载集群自动缩放器 V1.0.3 的 Helm chart，并将文件解压缩到 `cluster-autoscaler` 目录中。
   ```
   helm fetch iks-charts/ibm-iks-cluster-autoscaler --untar --untardir ./cluster-autoscaler --version 1.0.3
   ```
   {: pre}

7. 导航至解压缩 Helm chart 文件的目录。
   ```
   cd cluster-autoscaler
   ```
   {: pre}

8. 使用 Helm chart 中的文件为生成的 YAML 文件创建 `output` 目录。
   ```
   mkdir output
   ```
   {: pre}

9. 打开 `values.yaml` 文件，并根据 Helm chart 安装指示信息的要求进行任何更改。
   ```
   nano ibm-iks-cluster-autoscaler/values.yaml
   ```
   {: pre}

10. 使用本地 Helm 安装为 Helm chart 创建所有 Kubernetes YAML 文件。这些 YAML 文件会存储在先前创建的 `output` 目录中。
    ```
    helm template --values ./ibm-iks-cluster-autoscaler/values.yaml --output-dir ./output ./ibm-iks-cluster-autoscaler
    ```
    {: pre}

    输出示例：
    ```
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-configmap.yaml
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-service-account-roles.yaml
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-service.yaml
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-deployment.yaml
    ```
    {: screen}

11. 将所有 YAML 文件部署到专用集群。
    ```
    kubectl apply --recursive --filename ./output
    ```
   {: pre}

12. 可选：从 `output` 目录中除去所有 YAML 文件。
    ```
    kubectl delete --recursive --filename ./output
    ```
    {: pre}

## 相关 Helm 链接
{: #helm_links}

查看以下链接以了解其他 Helm 信息。
{: shortdesc}

* 在 [Helm Chart 目录 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/kubernetes/solutions/helm-charts) 中查看可以在 {{site.data.keyword.containerlong_notm}} 中使用的可用 Helm chart。
* 在 <a href="https://docs.helm.sh/helm/" target="_blank">Helm 文档 <img src="../icons/launch-glyph.svg" alt="外部链接图标"></a> 中了解有关可用于设置和管理 Helm chart 的 Helm 命令的更多信息。
* 了解有关如何[利用 Kubernetes Helm chart 提高部署速度 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://developer.ibm.com/recipes/tutorials/increase-deployment-velocity-with-kubernetes-helm-charts/) 的更多信息。
