---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

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



# 教程：创建 Kubernetes 集群
{: #cs_cluster_tutorial}

通过本教程，您可以在 {{site.data.keyword.containerlong}} 中部署和管理 Kubernetes 集群。了解如何在集群中自动执行对容器化应用程序的部署、操作、缩放和监视。
{:shortdesc}

在本教程系列中，您可以看到虚构的公关公司使用 Kubernetes 功能在 {{site.data.keyword.Bluemix_notm}} 中部署容器化应用程序的方式。利用 {{site.data.keyword.toneanalyzerfull}}，该 PR 公司可以分析他们的新闻稿，并获得反馈。


## 目标
{: #tutorials_objectives}

在第一个教程中，您将充当 PR 公司的网络管理员。您可在 {{site.data.keyword.containerlong_notm}} 中配置定制 Kubernetes 集群，以用于部署和测试应用程序的 Hello World 版本。
{:shortdesc}

-   使用具有 1 个工作程序节点的 1 个工作程序池创建集群。
-   安装 CLI 以在 {{site.data.keyword.registrylong_notm}} 中运行 [Kubernetes 命令 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubectl.docs.kubernetes.io/) 并管理 Docker 映像。
-   在 {{site.data.keyword.registrylong_notm}} 中创建专用映像存储库以存储映像。
-   将 {{site.data.keyword.toneanalyzershort}} 服务添加到集群，以便集群中的任何应用程序都可使用该服务。


## 所需时间
{: #tutorials_time}

40 分钟


## 受众
{: #tutorials_audience}

本教程适用于首次创建 Kubernetes 集群的软件开发者和网络管理员。
{: shortdesc}

## 先决条件
{: #tutorials_prereqs}

-  查看[准备创建集群](/docs/containers?topic=containers-clusters#cluster_prepare)时需要执行的步骤。
-  确保您具有以下访问策略：
    - 对 {{site.data.keyword.containerlong_notm}} 的 [{{site.data.keyword.Bluemix_notm}} IAM **管理员**平台角色](/docs/containers?topic=containers-users#platform)
    - 对 {{site.data.keyword.registrylong_notm}} 的 [{{site.data.keyword.Bluemix_notm}} IAM **管理员**平台角色](/docs/containers?topic=containers-users#platform)
    - 对 {{site.data.keyword.containerlong_notm}} 的[{{site.data.keyword.Bluemix_notm}} IAM **写入者**或**管理者**服务角色](/docs/containers?topic=containers-users#platform)


## 第 1 课：创建集群并设置 CLI
{: #cs_cluster_tutorial_lesson1}

在 {{site.data.keyword.Bluemix_notm}} 控制台中创建 Kubernetes 集群并安装必需的 CLI。
{: shortdesc}

**创建集群**

集群可能需要几分钟时间进行供应，因此请在安装 CLI 之前创建集群。

1.  [在 {{site.data.keyword.Bluemix_notm}} 控制台 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/kubernetes/catalog/cluster/create) 中，创建免费或标准集群，其中包含具有 1 个工作程序节点的 1 个工作程序池。

    您还可以[在 CLI 中创建集群](/docs/containers?topic=containers-clusters#clusters_cli_steps)。
    {: tip}

集群供应后，请安装用于管理集群的以下 CLI：
-   {{site.data.keyword.Bluemix_notm}} CLI 
-   {{site.data.keyword.containerlong_notm}} 插件
-   Kubernetes CLI
-   {{site.data.keyword.registryshort_notm}} 插件

如果要改为使用 {{site.data.keyword.Bluemix_notm}} 控制台，那么在创建集群后，可以在 [Kubernetes 终端](/docs/containers?topic=containers-cs_cli_install#cli_web)中直接通过 Web 浏览器来运行 CLI 命令。
{: tip}

</br>
**安装 CLI 及其必备软件**

1. 安装 [{{site.data.keyword.Bluemix_notm}} CLI ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](/docs/cli?topic=cloud-cli-getting-started)。此安装包括：
  - 基本 {{site.data.keyword.Bluemix_notm}} CLI。用于通过 {{site.data.keyword.Bluemix_notm}} CLI 运行命令的前缀是 `ibmcloud`。
  - {{site.data.keyword.containerlong_notm}} 插件。用于通过 {{site.data.keyword.Bluemix_notm}} CLI 运行命令的前缀是 `ibmcloud ks`。
  - {{site.data.keyword.registryshort_notm}} 插件。使用此插件可在 {{site.data.keyword.registryshort_notm}} 中设置和管理专用映像存储库。用于运行注册表命令的前缀是 `ibmcloud cr`。

2. 登录到 {{site.data.keyword.Bluemix_notm}} CLI。根据提示，输入您的 {{site.data.keyword.Bluemix_notm}} 凭证。
  ```
    ibmcloud login
    ```
  {: pre}

  如果您有联合标识，请使用 `--sso` 标志登录。输入您的用户名，并使用 CLI 输出中提供的 URL 来检索一次性密码。
  {: tip}

3. 遵循提示来选择帐户。

5. 验证各插件是否已正确安装。
  ```
    ibmcloud plugin list
    ```
  {: pre}

  {{site.data.keyword.containerlong_notm}} 插件会在结果中显示为 **container-service**，{{site.data.keyword.registryshort_notm}} 插件会在结果中显示为 **container-registry**。

6. 要将应用程序部署到集群中，请[安装 Kubernetes CLI ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/tasks/tools/install-kubectl/)。用于通过 Kubernetes CLI 来运行命令的前缀是 `kubectl`。

  1. 下载与您计划使用的 Kubernetes 集群 `major.minor` 版本相匹配的 Kubernetes CLI `major.minor` 版本。当前 {{site.data.keyword.containerlong_notm}} 缺省 Kubernetes 版本是 1.13.6。
    - **OS X**：[https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/darwin/amd64/kubectl ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/darwin/amd64/kubectl)
    - **Linux**：[https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/linux/amd64/kubectl ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/linux/amd64/kubectl)
    - **Windows**：[https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/windows/amd64/kubectl.exe ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/windows/amd64/kubectl.exe)

  2. 如果使用的是 OS X 或 Linux，请完成以下步骤。

    1. 将可执行文件移至 `/usr/local/bin` 目录。
      ```
      mv /filepath/kubectl /usr/local/bin/kubectl
      ```
      {: pre}

    2. 确保 `/usr/local/bin` 列在 `PATH` 系统变量中。`PATH` 变量包含操作系统可以在其中找到可执行文件的所有目录。列在 `PATH` 变量中的目录用于不同的用途。`/usr/local/bin` 用于为不属于操作系统的一部分，而是由系统管理员手动安装的软件存储其可执行文件。
      ```
      echo $PATH
      ```
      {: pre}
      示例 CLI 输出：
      ```
      /usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
      ```
      {: screen}

    3. 使文件可执行。
      ```
      chmod +x /usr/local/bin/kubectl
      ```
      {: pre}

非常好！您已成功安装了以下课程和教程的 CLI。接下来，设置集群环境并添加 {{site.data.keyword.toneanalyzershort}} 服务。


## 第 2 课：设置专用注册表
{: #cs_cluster_tutorial_lesson2}

在 {{site.data.keyword.registryshort_notm}} 中设置专用映像存储库，并向 Kubernetes 集群添加私钥，以便应用程序可以访问 {{site.data.keyword.toneanalyzershort}} 服务。
{: shortdesc}

1.  如果集群位于非 `default` 资源组中，请将该资源组设定为目标。要查看每个集群所属的资源组，请运行 `ibmcloud ks clusters`。
   ```
   ibmcloud target -g <resource_group_name>
   ```
   {: pre}

2.  在 {{site.data.keyword.registryshort_notm}} 中设置您自己的专用映像存储库，以安全地存储 Docker 映像并与所有集群用户共享这些映像。{{site.data.keyword.Bluemix_notm}} 中的专用映像存储库由名称空间标识。名称空间用于创建映像存储库的唯一 URL，开发者可使用此 URL 来访问专用 Docker 映像。

    使用容器映像时，请了解有关[确保个人信息安全](/docs/containers?topic=containers-security#pi)的更多信息。

    在此示例中，公关公司希望在 {{site.data.keyword.registryshort_notm}} 中仅创建一个映像存储库，所以他们选择 `pr_firm` 作为其名称空间，用于对其帐户中的所有映像分组。将 &lt;namespace&gt; 替换为您所选择的与教程无关的名称空间。

    ```
    ibmcloud cr namespace-add <namespace>
    ```
    {: pre}

3.  继续执行下一步之前，请验证工作程序节点的部署是否已完成。

    ```
   ibmcloud ks workers --cluster <cluster_name_or_ID>
   ```
    {: pre}

    工作程序节点供应完成时，状态会更改为 **Ready**，这时可以开始绑定 {{site.data.keyword.Bluemix_notm}} 服务。

    ```
    ID                                                 Public IP       Private IP       Machine Type   State    Status   Zone   Version
    kube-mil01-pafe24f557f070463caf9e31ecf2d96625-w1   169.xx.xxx.xxx   10.xxx.xx.xxx   free           normal   Ready    mil01      1.13.6
    ```
    {: screen}

## 第 3 课：设置集群环境
{: #cs_cluster_tutorial_lesson3}

在 CLI 中设置 Kubernetes 集群的上下文。
{: shortdesc}

每次登录到 {{site.data.keyword.containerlong}} CLI 来使用集群时，必须运行这些命令以将集群配置文件的路径设置为会话变量。Kubernetes CLI 使用此变量来查找与 {{site.data.keyword.Bluemix_notm}} 中的集群连接所必需的本地配置文件和证书。

1.  获取命令以设置环境变量并下载 Kubernetes 配置文件。
    ```
    ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
    ```
    {: pre}

    配置文件下载完成后，会显示一个命令，您可以使用该命令将本地 Kubernetes 配置文件的路径设置为环境变量。

    OS X 的示例：
    ```
    export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/pr_firm_cluster/kube-config-prod-par02-pr_firm_cluster.yml
    ```
    {: screen}
    使用的是 Windows PowerShell？请包含 `--powershell` 标志以获取 Windows PowerShell 格式的环境变量。
    {: tip}

2.  复制并粘贴终端中显示的命令，以设置 `KUBECONFIG` 环境变量。

3.  验证是否已正确设置 `KUBECONFIG` 环境变量。

    OS X 的示例：

    ```
        echo $KUBECONFIG
        ```
    {: pre}

    输出：

    ```
        /Users/<user_name>/.bluemix/plugins/container-service/clusters/pr_firm_cluster/kube-config-prod-par02-pr_firm_cluster.yml
        ```
    {: screen}

4.  通过检查 Kubernetes CLI 服务器版本，验证 `kubectl` 命令是否针对您的集群正常运行。

    ```
        kubectl version  --short
        ```
    {: pre}

    输出示例：

    ```
    Client Version: v1.13.6
    Server Version: v1.13.6
    ```
    {: screen}

## 第 4 课：向集群添加服务
{: #cs_cluster_tutorial_lesson4}

通过 {{site.data.keyword.Bluemix_notm}} 服务，您可以利用应用程序中已开发的功能。Kubernetes 集群中部署的任何应用程序都可以使用绑定到该集群的任何 {{site.data.keyword.Bluemix_notm}} 服务。对要用于应用程序的每个 {{site.data.keyword.Bluemix_notm}} 服务，重复以下步骤。
{: shortdesc}

1.  将 {{site.data.keyword.toneanalyzershort}} 服务添加到您的 {{site.data.keyword.Bluemix_notm}} 帐户。将 <service_name> 替换为服务实例的名称。

    将 {{site.data.keyword.toneanalyzershort}} 服务添加到您的帐户时，将显示一条消息表明该服务不是免费的。如果限制 API 调用，那么本教程不会导致 {{site.data.keyword.watson}} 服务收取费用。[查看 {{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}} 服务的定价信息 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/catalog/services/tone-analyzer)。
    {: note}

    ```
    ibmcloud resource service-instance-create <service_name> tone-analyzer standard us-south
    ```
    {: pre}

2.  将 {{site.data.keyword.toneanalyzershort}} 实例绑定到集群的 `default` Kubernetes 名称空间。您可以在以后创建自己的名称空间来管理对 Kubernetes 资源的用户访问权，但现在，请使用 `default` 名称空间。Kubernetes 名称空间不同于先前创建的注册表名称空间。

    ```
    ibmcloud ks cluster-service-bind --cluster <cluster_name> --namespace default --service <service_name>
    ```
    {: pre}

    输出：

    ```
    ibmcloud ks cluster-service-bind pr_firm_cluster default mytoneanalyzer
    Binding service instance to namespace...
    OK
    Namespace:	default
    Secret name:	binding-mytoneanalyzer
    ```
    {: screen}

3.  验证是否已在集群名称空间中创建 Kubernetes 私钥。每个 {{site.data.keyword.Bluemix_notm}} 服务都由一个 JSON 文件进行定义，此文件包含容器用于获取访问权的保密信息，例如 {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM) API 密钥和 URL。为了安全地存储这些信息，将使用 Kubernetes 私钥。在此示例中，私钥所包含的 API 密钥用于访问在您帐户中供应的 {{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}} 实例。

    ```
    kubectl get secrets --namespace=default
    ```
    {: pre}

    输出：

    ```
    NAME                       TYPE                                  DATA      AGE
    binding-mytoneanalyzer     Opaque                                1         1m
    bluemix-default-secret     kubernetes.io/dockercfg               1         1h
    default-token-kf97z        kubernetes.io/service-account-token   3         1h
    ```
    {: screen}

</br>
非常好！您的集群已配置，并且您的本地环境已准备就绪，可以开始将应用程序部署到集群中。

## 接下来要做什么？
{: #tutorials_next}

* 测试您的掌握情况并[进行测验 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://ibmcloud-quizzes.mybluemix.net/containers/cluster_tutorial/quiz.php)！
* 尝试[教程：将应用程序部署到 Kubernetes 集群](/docs/containers?topic=containers-cs_apps_tutorial#cs_apps_tutorial)，以将公关公司的应用程序部署到已创建的集群。
