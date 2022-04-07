---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-04"

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



# 基于映像构建容器
{: #images}

Docker 映像是使用 {{site.data.keyword.containerlong}} 所创建的每一个容器的基础。
{:shortdesc}

映像通过 Dockerfile 进行创建，此文件包含用于构建映像的指令。Dockerfile 可能会在其单独存储的指令中引用构建工件，例如应用程序、应用程序配置及其依赖项。


## 规划映像注册表
{: #planning_images}

映像通常会存储在可以公共访问的注册表（公共注册表）中，或者存储在设置为仅供一小组用户访问的注册表（专用注册表）中。
{:shortdesc}

开始使用 Docker 和 Kubernetes 在集群中创建第一个容器化应用程序时，可以使用公共注册表（如 Docker Hub）。但是对于企业应用程序，请使用专用注册表（如在 {{site.data.keyword.registryshort_notm}} 中提供的注册表），以保护映像不被未经授权的用户使用和更改。专用注册表必须由集群管理员设置，以确保用于访问专用注册表的凭证可供集群用户使用。



可以通过 {{site.data.keyword.containerlong_notm}} 使用多个注册表将应用程序部署到集群。

|注册表|描述|优点|
|--------|-----------|-------|
|[{{site.data.keyword.registryshort_notm}}](/docs/services/Registry?topic=registry-getting-started#getting-started)|使用此选项，可以在 {{site.data.keyword.registryshort_notm}} 中设置您自己的安全 Docker 映像存储库，在其中可以安全地存储映像并在集群用户之间共享这些映像。|<ul><li>管理对您帐户中映像的访问权。</li><li>将 {{site.data.keyword.IBM_notm}} 提供的映像和样本应用程序（如 {{site.data.keyword.IBM_notm}} Liberty）用作父映像，并向其添加您自己的应用程序代码。</li><li>漏洞顾问程序会自动扫描映像以确定是否有潜在漏洞，包括特定于操作系统的漏洞修复建议。</li></ul>|
|其他任何专用注册表|通过创建[映像拉取私钥 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/containers/images/)，将任何现有专用注册表连接到集群。私钥用于将注册表 URL 和凭证安全地保存在 Kubernetes 私钥中。|<ul><li>独立于源（Docker Hub、组织拥有的注册表或其他云专用注册表）使用现有专用注册表。</li></ul>|
|[公共 Docker Hub ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://hub.docker.com/){: #dockerhub}|使用此选项可在不需要 Dockerfile 更改时，在 [Kubernetes 部署 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) 中直接通过 Docker Hub 使用现有公共映像。<p>**注**：请记住，此选项可能不满足您组织的安全需求，如访问管理、漏洞扫描或应用程序隐私。</p>|<ul><li>无需为集群进行其他设置。</li><li>包含各种开放式源代码应用程序。</li></ul>|
{: caption="公共和专用映像注册表选项" caption-side="top"}

设置映像注册表后，集群用户可以使用映像将应用程序部署到集群。

使用容器映像时，请了解有关[确保个人信息安全](/docs/containers?topic=containers-security#pi)的更多信息。

<br />


## 为容器映像设置可信内容
{: #trusted_images}

可以基于已签名并存储在 {{site.data.keyword.registryshort_notm}} 中的可信映像来构建容器，并阻止未签名或易受攻击的映像中的部署。
{:shortdesc}

1.  [对可信内容的映像签名](/docs/services/Registry?topic=registry-registry_trustedcontent#registry_trustedcontent)。设置映像的信任后，可以管理可信内容和可以将映像推送到注册表的签署者。
2.  要强制实施仅签名映像可用于在集群中构建容器的策略，请[添加 Container Image Security Enforcement (beta)](/docs/services/Registry?topic=registry-security_enforce#security_enforce)。
3.  部署应用程序。
    1. [部署到 `default` Kubernetes 名称空间](#namespace)。
    2. [部署到其他 Kubernetes 名称空间，或从其他 {{site.data.keyword.Bluemix_notm}} 区域或帐户进行部署](#other)。

<br />


## 将容器从 {{site.data.keyword.registryshort_notm}} 映像部署到 `default` Kubernetes 名称空间
{: #namespace}

可以通过 IBM 提供的公共映像将容器部署到集群，也可以通过存储在 {{site.data.keyword.registryshort_notm}} 名称空间中的专用映像来部署。有关集群如何访问注册表映像的更多信息，请参阅[了解如何授权集群从 {{site.data.keyword.registrylong_notm}} 中拉取映像](#cluster_registry_auth)。
{:shortdesc}

开始之前：
1. [在 {{site.data.keyword.registryshort_notm}} 中设置名称空间，并将映像推送到此名称空间](/docs/services/Registry?topic=registry-getting-started#gs_registry_namespace_add)。
2. [创建集群](/docs/containers?topic=containers-clusters#clusters_ui)。
3. 如果具有在 **2019 年 2 月 25 日**之前创建的现有集群，请[更新集群以使用 API 密钥 `imagePullSecret`](#imagePullSecret_migrate_api_key)。
4. [登录到您的帐户。如果适用，请将相应的资源组设定为目标。为集群设置上下文。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

要将容器部署到集群的 **default** 名称空间，请执行以下操作：

1.  创建名为 `mydeployment.yaml` 的部署配置文件。
2.  定义部署以及要从 {{site.data.keyword.registryshort_notm}} 中您的名称空间使用的映像。

    ```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: <app_name>-deployment
    spec:
      replicas: 3
      selector:
        matchLabels:
          app: <app_name>
      template:
        metadata:
          labels:
            app: <app_name>
        spec:
          containers:
          - name: <app_name>
            image: <region>.icr.io/<namespace>/<my_image>:<tag>
    ```
    {: codeblock}

    将映像 URL 变量替换为映像的信息：
    *  **`<app_name>`**：应用程序的名称。
    *  **`<region>`**：注册表域的区域 {{site.data.keyword.registryshort_notm}} API 端点。要列出您登录到的区域的域，请运行 `ibmcloud cr api`。
    *  **`<namespace>`**：注册表名称空间。要获取名称空间信息，请运行 `ibmcloud cr namespace-list`。
    *  **`<my_image>:<tag>`**：要用于构建容器的映像和标记。要获取注册表中可用的映像，请运行 `ibmcloud cr images`。

3.  在集群中创建部署。

    ```
    kubectl apply -f mydeployment.yaml
    ```
    {: pre}

<br />


## 了解如何授权集群从注册表中拉取映像
{: #cluster_registry_auth}

为了从注册表中拉取映像，{{site.data.keyword.containerlong_notm}} 集群会使用一种特殊类型的 Kubernetes 私钥，即 `imagePullSecret`。此映像拉取私钥存储了用于访问容器注册表的凭证。容器注册表可以是 {{site.data.keyword.registrylong_notm}} 中您的名称空间、{{site.data.keyword.registrylong_notm}} 中属于其他 {{site.data.keyword.Bluemix_notm}} 帐户的名称空间，或者其他任何专用注册表（如 Docker）。您的集群设置为从 {{site.data.keyword.registrylong_notm}} 中您的名称空间中拉取映像，并将基于这些映像的容器部署到集群中的 `default` Kubernetes 名称空间。如果需要拉取其他集群 Kubernetes 名称空间中或其他注册表中的映像，那么必须设置映像拉取私钥。
{:shortdesc}

**集群如何设置为从 `default` Kubernetes 名称空间中拉取映像？**<br>
创建集群时，集群会具有 {{site.data.keyword.Bluemix_notm}} IAM 服务标识，此标识已被授予对 {{site.data.keyword.registrylong_notm}} 的 IAM **读取者**服务访问角色策略。在存储于集群的映像拉取私钥中的非到期 API 密钥中，会模拟服务标识凭证。映像拉取私钥会添加到 `default` Kubernetes 名称空间以及此名称空间的 `default` 服务帐户中的私钥列表。使用映像拉取私钥时，部署可以在[全局和区域注册表](/docs/services/Registry?topic=registry-registry_overview#registry_regions)中拉取（只读访问）映像，用于在 `default` Kubernetes 名称空间中构建容器。全局注册表用于安全地存储 IBM 提供的公共映像，您可以在不同部署中引用这些映像，而不用分别引用存储在各个区域注册表中的映像。区域注册表用于安全地存储您自己的专用 Docker 映像。


**可以将拉取访问权限制为只能访问特定区域注册表吗？**<br>
可以，可以[编辑服务标识的现有 IAM 策略](/docs/iam?topic=iam-serviceidpolicy#access_edit)，以将**读取者**服务访问角色限制为只能访问该区域注册表或注册表资源（例如，名称空间）。必须[启用对 {{site.data.keyword.registrylong_notm}} 的 {{site.data.keyword.Bluemix_notm}} IAM 策略](/docs/services/Registry?topic=registry-user#existing_users)后，才能定制注册表 IAM 策略。

  想要使注册表凭证更安全吗？请要求集群管理员在集群中[启用 {{site.data.keyword.keymanagementservicefull}}](/docs/containers?topic=containers-encryption#keyprotect)，以加密集群中的 Kubernetes 私钥，例如用于存储注册表凭证的 `imagePullSecret`。
  {: tip}

**可以拉取非 `default` Kubernetes 名称空间中的映像吗？**<br>
缺省情况下不能。使用缺省集群设置时，可以将基于 {{site.data.keyword.registrylong_notm}} 名称空间中所存储任何映像的容器部署到集群的 `default` Kubernetes 名称空间。要在其他 Kubernetes 名称空间或其他 {{site.data.keyword.Bluemix_notm}} 帐户中使用这些映像，[可选择复制或创建您自己的映像拉取私钥](#other)。

**可以从其他 {{site.data.keyword.Bluemix_notm}} 帐户拉取映像吗？**<br>
可以，在要使用的 {{site.data.keyword.Bluemix_notm}} 帐户中创建 API 密钥。然后，在要从中拉取映像的每个集群和集群名称空间中创建一个映像拉取私钥，用于存储这些 API 密钥凭证。请[遵循使用授权服务标识 API 密钥的此示例](#other_registry_accounts)。

要使用非 {{site.data.keyword.Bluemix_notm}} 注册表（如 Docker），请参阅[访问存储在其他专用注册表中的映像](#private_images)。

**API 密钥是否需要对应于服务标识？如果达到帐户的服务标识数限制，会发生什么情况？**<br>
缺省集群设置会创建服务标识，用于将 {{site.data.keyword.Bluemix_notm}} IAM API 密钥凭证存储在映像拉取私钥中。但是，您还可以为单个用户创建 API 密钥，并将这些凭证存储在一个映像拉取私钥中。如果达到[服务标识的 IAM 限制](/docs/iam?topic=iam-iam_limits#iam_limits)，那么您的集群将在不使用服务标识和映像拉取私钥的情况下进行创建，并且缺省情况下无法从 `icr.io` 注册表域中拉取映像。您必须[创建自己的映像拉取私钥](#other_registry_accounts)，但创建时须使用单个用户（例如功能标识，而不是 {{site.data.keyword.Bluemix_notm}} IAM 服务标识）的 API 密钥。

**我的集群映像拉取私钥使用的是注册表令牌。令牌还仍然有效吗？**<br>

支持通过自动创建[令牌](/docs/services/Registry?topic=registry-registry_access#registry_tokens)并将令牌存储在映像拉取私钥中来授权集群访问 {{site.data.keyword.registrylong_notm}} 的先前方法，但不推荐使用此方法。
{: deprecated}

令牌用于授权访问不推荐使用的 `registry.bluemix.net` 注册表域，而 API 密钥用于授权访问 `icr.io` 注册表域。在从基于令牌的认证转换为基于 API 密钥的认证期间，将创建基于令牌和基于 API 密钥的映像拉取私钥一次。通过基于令牌和基于 API 密钥的映像拉取私钥，集群可以从 `registry.bluemix.net` 中拉取映像，或从 `default` Kubernetes 名称空间中的 `icr.io` 域中拉取映像。

在不推荐使用的令牌和 `registry.bluemix.net` 域变为不受支持之前，请更新集群映像拉取私钥，以将 API 密钥方法用于 [`default` Kubernetes 名称空间](#imagePullSecret_migrate_api_key)以及可能使用的[其他任何名称空间或帐户](#other)。然后，将部署更新为从 `icr.io` 注册表域中进行拉取。

**在另一个 Kubernetes 名称空间中复制或创建映像拉取私钥之后，即完成操作了吗？**<br>
还没完。容器必须有权使用所创建的私钥来拉取映像。可以将映像拉取私钥添加到该名称空间的服务帐户，或者在每个部署中引用此私钥。有关指示信息，请参阅[使用映像拉取私钥部署容器](/docs/containers?topic=containers-images#use_imagePullSecret)。

<br />


## 更新现有集群以使用 API 密钥映像拉取私钥
{: #imagePullSecret_migrate_api_key}

新 {{site.data.keyword.containerlong_notm}} 集群会[在映像拉取私钥中存储 API 密钥，以授权访问 {{site.data.keyword.registrylong_notm}}](#cluster_registry_auth)。使用这些映像拉取私钥时，可以通过存储在 `icr.io` 注册表域中的映像来部署容器。对于在 **2019 年 2 月 25 日**之前创建的集群，必须更新集群以在映像拉取私钥中存储 API 密钥，而不存储注册表令牌。
{: shortdesc}

**开始之前**：
*   [登录到您的帐户。如果适用，请将相应的资源组设定为目标。为集群设置上下文。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
*   确保您具有以下许可权：
    *   对 {{site.data.keyword.containerlong_notm}} 的 {{site.data.keyword.Bluemix_notm}} IAM **操作员或管理员**平台角色。帐户所有者可以通过运行以下命令为您授予角色：
        ```
        ibmcloud iam user-policy-create <your_user_email> --service-name containers-kubernetes --roles Administrator,Operator
        ```
        {: pre}
    *   在所有区域和资源组中，对 {{site.data.keyword.registrylong_notm}} 的 {{site.data.keyword.Bluemix_notm}} IAM **管理员**平台角色。帐户所有者可以通过运行以下命令为您授予角色：
        ```
        ibmcloud iam user-policy-create <your_user_email> --service-name container-registry --roles Administrator
        ```
        {: pre}

**要更新 `default` Kubernetes 名称空间中的集群映像拉取私钥，请执行以下操作**：
1.  获取集群标识。
    ```
    ibmcloud ks clusters
    ```
    {: pre}
2.  运行以下命令为集群创建服务标识，为服务标识分配对 {{site.data.keyword.registrylong_notm}} 的 IAM **读取者**服务角色，创建 API 密钥以模拟服务标识凭证，然后将 API 密钥存储在集群中的 Kubernetes 映像拉取私钥中。映像拉取私钥位于 `default` Kubernetes 名称空间中。
   ```
    ibmcloud ks cluster-pull-secret-apply --cluster <cluster_name_or_ID>
    ```
    {: pre}

    运行此命令时，IAM 凭证和映像拉取私钥的创建操作将启动，并且可能需要一些时间才能完成。在创建映像拉取私钥之前，无法部署用于从 {{site.data.keyword.registrylong_notm}} `icr.io` 域中拉取映像的容器。
    {: important}

3.  验证是否在集群中创建了映像拉取私钥。请注意，您对每个 {{site.data.keyword.registrylong_notm}} 区域都有单独的映像拉取私钥。
    ```
    kubectl get secrets
    ```
    {: pre}
输出示例：
    ```
    default-us-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-uk-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-de-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-au-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-jp-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-icr-io                             kubernetes.io/dockerconfigjson        1         16d
    ```
    {: screen}
4.  更新容器部署以从 `icr.io` 域名中拉取映像。
5.  可选：如果您有防火墙，请确保对于使用的域，[允许出站网络流量流至注册表子网](/docs/containers?topic=containers-firewall#firewall_outbound)。

**接下来要做什么？**
*   要拉取非 `default` Kubernetes 名称空间中的映像或从其他 {{site.data.keyword.Bluemix_notm}} 帐户中拉取映像，请[复制或创建其他映像拉取私钥](/docs/containers?topic=containers-images#other)。
*   要将映像拉取私钥限制为只能访问特定注册表资源（例如，名称空间或区域），请执行以下操作：
    1.  确保[对 {{site.data.keyword.registrylong_notm}} 的 {{site.data.keyword.Bluemix_notm}} IAM 策略已启用](/docs/services/Registry?topic=registry-user#existing_users)。
    2.  针对服务标识[编辑 {{site.data.keyword.Bluemix_notm}} IAM 策略](/docs/iam?topic=iam-serviceidpolicy#access_edit)，或[创建其他映像拉取私钥](/docs/containers?topic=containers-images#other_registry_accounts)。

<br />


## 使用映像拉取私钥访问其他集群 Kubernetes 名称空间、其他 {{site.data.keyword.Bluemix_notm}} 帐户或外部专用注册表
{: #other}

在集群中设置您自己的映像拉取私钥，以将容器部署到非 `default` Kubernetes 名称空间，使用存储在其他 {{site.data.keyword.Bluemix_notm}} 帐户中的映像，或使用存储在外部专用注册表中的映像。此外，您还可以创建自己的映像拉取私钥来应用 IAM 访问策略，以将许可权作用对象限制为特定注册表映像名称空间或操作（例如，`push` 或 `pull`）。
{:shortdesc}

创建映像拉取私钥后，容器必须使用该私钥才有权从注册表中拉取映像。可以将映像拉取私钥添加到该名称空间的服务帐户，或者在每个部署中引用此私钥。有关指示信息，请参阅[使用映像拉取私钥部署容器](/docs/containers?topic=containers-images#use_imagePullSecret)。

映像拉取私钥仅对于创建用于的 Kubernetes 名称空间有效。对要部署容器的每个名称空间，重复这些步骤。来自 [DockerHub](#dockerhub) 的映像不需要映像拉取私钥。
{: tip}

开始之前：

1.  [在 {{site.data.keyword.registryshort_notm}} 中设置名称空间，并将映像推送到此名称空间](/docs/services/Registry?topic=registry-getting-started#gs_registry_namespace_add)。
2.  [创建集群](/docs/containers?topic=containers-clusters#clusters_ui)。
3.  如果具有在 **2019 年 2 月 25 日**之前创建的现有集群，请[更新集群以使用 API 密钥映像拉取私钥](#imagePullSecret_migrate_api_key)。
4.  [登录到您的帐户。如果适用，请将相应的资源组设定为目标。为集群设置上下文。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

<br/>
要使用您自己的映像拉取私钥，请从以下选项中进行选择：
- 从 default Kubernetes 名称空间[复制映像拉取私钥](#copy_imagePullSecret)至集群中的其他名称空间。
- [创建新的 IAM API 密钥凭证并将其存储在映像拉取私钥中](#other_registry_accounts)，以访问其他 {{site.data.keyword.Bluemix_notm}} 帐户中的映像，或应用 IAM 策略来限制对特定注册表域或名称空间的访问权。
- [创建映像拉取私钥以访问外部专用注册表中的映像](#private_images)。

<br/>
如果已经在名称空间中创建了要在部署中使用的映像拉取私钥，请参阅[使用创建的 `imagePullSecret` 部署容器](#use_imagePullSecret)。

### 复制现有映像拉取私钥
{: #copy_imagePullSecret}

可以将映像拉取私钥（例如自动为 `default` Kubernetes 名称空间创建的映像拉取私钥）复制到集群中的其他名称空间。如果要将其他 {{site.data.keyword.Bluemix_notm}} IAM API 密钥凭证用于此名称空间，例如限制对特定名称空间的访问权，或从其他 {{site.data.keyword.Bluemix_notm}} 帐户中拉取映像，请改为[创建映像拉取私钥](#other_registry_accounts)。
{: shortdesc}

1.  列出集群中可用的 Kubernetes 名称空间，或者创建要使用的名称空间。
    ```
        kubectl get namespaces
        ```
    {: pre}

    输出示例：
    ```
    default          Active    79d
    ibm-cert-store   Active    79d
    ibm-system       Active    79d
    istio-system     Active    34d
    kube-public      Active    79d
    kube-system      Active    79d
    ```
    {: screen}

    要创建名称空间，请运行以下命令：
    ```
            kubectl create namespace <namespace_name>
        ```
    {: pre}
2.  列出 {{site.data.keyword.registrylong_notm}} 的 `default` Kubernetes 名称空间中的现有映像拉取私钥。
    ```
    kubectl get secrets -n default | grep icr
    ```
    {: pre}
输出示例：
    ```
    default-us-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-uk-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-de-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-au-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-jp-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-icr-io                             kubernetes.io/dockerconfigjson        1         16d
    ```
    {: screen}
3.  将每个映像拉取私钥从 `default` 名称空间复制到您选择的名称空间。新映像拉取私钥的名称为 `<namespace_name>-icr-<region>-io`。
    ```
    kubectl get secret default-us-icr-io -o yaml | sed 's/default/<new-namespace>/g' | kubectl -n <new-namespace> create -f -
    ```
    {: pre}
    ```
    kubectl get secret default-uk-icr-io -o yaml | sed 's/default/<new-namespace>/g' | kubectl -n <new-namespace> create -f -
    ```
    {: pre}
    ```
    kubectl get secret default-de-icr-io -o yaml | sed 's/default/<new-namespace>/g' | kubectl -n <new-namespace> create -f -
    ```
    {: pre}
    ```
    kubectl get secret default-au-icr-io -o yaml | sed 's/default/<new-namespace>/g' | kubectl -n <new-namespace> create -f -
    ```
    {: pre}
    ```
    kubectl get secret default-jp-icr-io -o yaml | sed 's/default/<new-namespace>/g' | kubectl -n <new-namespace> create -f -
    ```
    {: pre}
    ```
    kubectl get secret default-icr-io -o yaml | sed 's/default/<new-namespace>/g' | kubectl -n <new-namespace> create -f -
    ```
    {: pre}
4.  验证私钥是否已成功创建。
    ```
    kubectl get secrets -n <namespace_name>
    ```
    {: pre}
5.  [向 Kubernetes 服务帐户添加映像拉取私钥，以便在部署容器时，名称空间中的任何 pod 都可以使用映像拉取私钥](#use_imagePullSecret)。

### 创建具有不同 IAM API 密钥凭证的映像拉取私钥，以加强对其他 {{site.data.keyword.Bluemix_notm}} 帐户中映像的控制权或访问权
{: #other_registry_accounts}

可以将 {{site.data.keyword.Bluemix_notm}} IAM 访问策略分配给用户或服务标识，以将许可权作用对象限制为特定注册表映像名称空间或操作（例如，`push` 或 `pull`）。然后，创建 API 密钥并将这些注册表凭证存储在集群的映像拉取私钥中。
{: shortdesc}

例如，要访问其他 {{site.data.keyword.Bluemix_notm}} 帐户中的映像，请创建 API 密钥，用于存储该帐户中用户或服务标识的 {{site.data.keyword.registryshort_notm}} 凭证。然后，在集群的帐户中，将这些 API 密钥凭证保存在每个集群和集群名称空间的映像拉取私钥中。

以下步骤创建 API 密钥，用于存储 {{site.data.keyword.Bluemix_notm}} IAM 服务标识的凭证。您可能希望为具有对 {{site.data.keyword.registryshort_notm}} 的 {{site.data.keyword.Bluemix_notm}} IAM 服务访问策略的用户标识创建 API 密钥，而不使用服务标识。但是，请确保用户是功能标识，或者有计划来应对用户离开的情况，以便集群始终可以访问注册表。
{: note}

1.  列出集群中可用的 Kubernetes 名称空间，或者创建要使用的名称空间，在此名称空间中，将通过注册表映像部署容器。
    ```
        kubectl get namespaces
        ```
    {: pre}

    输出示例：
    ```
    default          Active    79d
    ibm-cert-store   Active    79d
    ibm-system       Active    79d
    istio-system     Active    34d
    kube-public      Active    79d
    kube-system      Active    79d
    ```
    {: screen}

    要创建名称空间，请运行以下命令：
    ```
            kubectl create namespace <namespace_name>
        ```
    {: pre}
2.  为集群创建 {{site.data.keyword.Bluemix_notm}} IAM 服务标识，此标识用于映像拉取私钥中的 IAM 策略和 API 密钥凭证。请确保为服务标识提供描述，以帮助您日后检索服务标识，例如包含集群名称和名称空间名称。
    ```
    ibmcloud iam service-id-create <cluster_name>-<kube_namespace>-id --description "Service ID for IBM Cloud Container Registry in Kubernetes cluster <cluster_name> namespace <kube_namespace>"
    ```
    {: pre}
3.  为集群服务标识创建定制 {{site.data.keyword.Bluemix_notm}} IAM 策略，以授予对 {{site.data.keyword.registryshort_notm}} 的访问权。
    ```
    ibmcloud iam service-policy-create <cluster_service_ID> --roles <service_access_role> --service-name container-registry [--region <IAM_region>] [--resource-type namespace --resource <registry_namespace>]
    ```
    {: pre}
    <table>
    <caption>了解此命令的组成部分</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解此命令的组成部分</th>
    </thead>
    <tbody>
    <tr>
    <td><code><em>&lt;cluster_service_ID&gt;</em></code></td>
    <td>必需。替换为先前为 Kubernetes 集群创建的 `<cluster_name>-<kube_namespace>-id` 服务标识。</td>
    </tr>
    <tr>
    <td><code>--service-name <em>container-registry</em></code></td>
    <td>必需。输入 `container-registry`，以便 IAM 策略可用于 {{site.data.keyword.registrylong_notm}}。</td>
    </tr>
    <tr>
    <td><code>--roles <em>&lt;service_access_role&gt;</em></code></td>
    <td>必需。输入要将服务标识访问权的作用域限定到的 [{{site.data.keyword.registrylong_notm}} 服务访问角色](/docs/services/Registry?topic=registry-iam#service_access_roles)。可能的值为 `Reader`（读取者）、`Writer`（写入者）和 `Manager`（管理者）。</td>
    </tr>
    <tr>
    <td><code>--region <em>&lt;IAM_region&gt;</em></code></td>
    <td>可选。如果要将访问策略的作用域限定为特定 IAM 区域，请以逗号分隔列表格式输入各区域。可能的值为 `au-syd`、`eu-gb`、`eu-de`、`jp-tok`、`us-south` 和 `global`。</td>
    </tr>
    <tr>
    <td><code>--resource-type <em>namespace</em> --resource <em>&lt;registry_namespace&gt;</em></code></td>
    <td>可选。如果要将访问权限制为仅访问特定 [{{site.data.keyword.registrylong_notm}} 名称空间](/docs/services/Registry?topic=registry-registry_overview#registry_namespaces)中的映像，请输入 `namespace` 作为资源类型，并指定 `<registry_namespace>`。要列出注册表名称空间，请运行 `ibmcloud cr namespaces`。</td>
    </tr>
    </tbody></table>
4.  为服务标识创建 API 密钥。将 API 密钥命名为类似于服务标识，并包含先前创建的服务标识 ``<cluster_name>-<kube_namespace>-id`。请确保为 API 密钥提供描述，以帮助您日后检索该密钥。
    ```
    ibmcloud iam service-api-key-create <cluster_name>-<kube_namespace>-key <cluster_name>-<kube_namespace>-id --description "API key for service ID <service_id> in Kubernetes cluster <cluster_name> namespace <kube_namespace>"
    ```
    {: pre}
5.  从上一个命令的输出中检索 **API 密钥**值。
    ```
    请保存 API 密钥！API 密钥创建后，即无法对其进行检索。

    Name          <cluster_name>-<kube_namespace>-key   
    Description   key_for_registry_for_serviceid_for_kubernetes_cluster_multizone_namespace_test   
    Bound To      crn:v1:bluemix:public:iam-identity::a/1bb222bb2b33333ddd3d3333ee4ee444::serviceid:ServiceId-ff55555f-5fff-6666-g6g6-777777h7h7hh   
    Created At    2019-02-01T19:06+0000   
    API Key       i-8i88ii8jjjj9jjj99kkkkkkkkk_k9-llllll11mmm1   
    Locked        false   
    UUID          ApiKey-222nn2n2-o3o3-3o3o-4p44-oo444o44o4o4   
    ```
    {: screen}
6.  创建 Kubernetes 映像拉取私钥，以将 API 密钥凭证存储在集群的名称空间中。对于要使用此服务标识的 IAM 凭证从注册表中拉取映像的每个 `icr.io` 域、Kubernetes 名称空间和集群，重复此步骤。
    ```
    kubectl --namespace <kubernetes_namespace> create secret docker-registry <secret_name> --docker-server=<registry_URL> --docker-username=iamapikey --docker-password=<api_key_value> --docker-email=<docker_email>
    ```
    {: pre}

    <table>
    <caption>了解此命令的组成部分</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解此命令的组成部分</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace <em>&lt;kubernetes_namespace&gt;</em></code></td>
    <td>必需。指定用于服务标识名称的集群的 Kubernetes 名称空间。</td>
    </tr>
    <tr>
    <td><code><em>&lt;secret_name&gt;</em></code></td>
    <td>必需。输入映像拉取私钥的名称。</td>
    </tr>
    <tr>
    <td><code>--docker-server <em>&lt;registry_URL&gt;</em></code></td>
    <td>必需。设置在其中设置注册表名称空间的映像注册表的 URL。可用注册表域如下：<ul>
    <li>亚太地区北部（东京）：`jp.icr.io`</li>
    <li>亚太地区南部（悉尼）：`au.icr.io`</li>
    <li>欧洲中部（法兰克福）：`de.icr.io`</li>
    <li>英国南部（伦敦）：`uk.icr.io`</li>
    <li>美国南部（达拉斯）：`us.icr.io`</li></ul></td>
    </tr>
    <tr>
    <td><code>--docker-username iamapikey</code></td>
    <td>必需。输入用于登录到专用注册表的用户名。对于 {{site.data.keyword.registryshort_notm}}，用户名设置为值 <strong><code>iamapikey</code></strong>。</td>
    </tr>
    <tr>
    <td><code>--docker-password <em>&lt;token_value&gt;</em></code></td>
    <td>必需。输入先前检索到的 **API 密钥**值。</td>
    </tr>
    <tr>
    <td><code>--docker-email <em>&lt;docker-email&gt;</em></code></td>
    <td>必需。如果您有 Docker 电子邮件地址，请输入该地址。如果没有，请输入虚构的电子邮件地址，例如 `a@b.c`。此电子邮件对于创建 Kubernetes 私钥是必需的，但在创建后不会再使用此电子邮件。</td>
    </tr>
    </tbody></table>
7.  验证私钥是否已成功创建。将 <em>&lt;kubernetes_namespace&gt;</em> 替换为在其中创建映像拉取私钥的名称空间。

    ```
        kubectl get secrets --namespace <kubernetes_namespace>
    ```
    {: pre}
8.  [向 Kubernetes 服务帐户添加映像拉取私钥，以便在部署容器时，名称空间中的任何 pod 都可以使用映像拉取私钥](#use_imagePullSecret)。

### 访问存储在其他专用注册表中的映像
{: #private_images}

如果您已经拥有专用注册表，那么必须将相应的注册表凭证存储在 Kubernetes 映像拉取私钥中，并在配置文件中引用此私钥。
{:shortdesc}

开始之前：

1.  [创建集群](/docs/containers?topic=containers-clusters#clusters_ui)。
2.  [设定 CLI 的目标为集群](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)。

要创建映像拉取私钥，请执行以下操作：

1.  创建 Kubernetes 私钥以用于存储专用注册表凭证。

    ```
    kubectl --namespace <kubernetes_namespace> create secret docker-registry <secret_name>  --docker-server=<registry_URL> --docker-username=<docker_username> --docker-password=<docker_password> --docker-email=<docker_email>
    ```
    {: pre}

    <table>
    <caption>了解此命令的组成部分</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解此命令的组成部分</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace <em>&lt;kubernetes_namespace&gt;</em></code></td>
    <td>必需。要使用私钥并将容器部署到的集群的 Kubernetes 名称空间。运行 <code>kubectl get namespaces</code> 可列出集群中的所有名称空间。</td>
    </tr>
    <tr>
    <td><code><em>&lt;secret_name&gt;</em></code></td>
    <td>必需。要用于 <code>imagePullSecret</code> 的名称。</td>
    </tr>
    <tr>
    <td><code>--docker-server <em>&lt;registry_URL&gt;</em></code></td>
    <td>必需。要在其中存储专用映像的注册表的 URL。</td>
    </tr>
    <tr>
    <td><code>--docker-username <em>&lt;docker_username&gt;</em></code></td>
    <td>必需。用于登录到专用注册表的用户名。</td>
    </tr>
    <tr>
    <td><code>--docker-password <em>&lt;token_value&gt;</em></code></td>
    <td>必需。先前检索到的注册表令牌的值。</td>
    </tr>
    <tr>
    <td><code>--docker-email <em>&lt;docker-email&gt;</em></code></td>
    <td>必需。如果您有 Docker 电子邮件地址，请输入该地址。如果没有，请输入虚构的电子邮件地址，例如 `a@b.c`。此电子邮件对于创建 Kubernetes 私钥是必需的，但在创建后不会再使用此电子邮件。</td>
    </tr>
    </tbody></table>

2.  验证私钥是否已成功创建。将 <em>&lt;kubernetes_namespace&gt;</em> 替换为在其中创建 `imagePullSecret` 的名称空间的名称。


    ```
    kubectl get secrets --namespace <kubernetes_namespace>
    ```
    {: pre}

3.  [创建引用映像拉取私钥的 pod](#use_imagePullSecret)。

<br />


## 使用映像拉取私钥来部署容器
{: #use_imagePullSecret}

可以在 pod 部署中定义映像拉取私钥，或将映像拉取私钥存储在 Kubernetes 服务帐户中，使其可用于未指定服务帐户的所有部署。
{: shortdesc}

在以下选项之间进行选择：
* [在 pod 部署中引用映像拉取私钥](#pod_imagePullSecret)：如果您不希望在缺省情况下为名称空间中的所有 pod 授予对注册表的访问权，请使用此选项。
* [将映像拉取私钥存储在 Kubernetes 服务帐户中](#store_imagePullSecret)：使用此选项可为所选 Kubernetes 名称空间中的所有部署授予对注册表中映像的访问权。

开始之前：
* [创建映像拉取私钥](#other)以访问其他注册表或非 `default` Kubernetes 名称空间中的映像。
* [设定 CLI 的目标为集群](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)。

### 在 pod 部署中引用映像拉取私钥
{: #pod_imagePullSecret}

在 pod 部署中引用映像拉取私钥时，映像拉取私钥仅对该 pod 有效，而不能在名称空间中的各 pod 之间共享。
{:shortdesc}

1.  创建名为 `mypod.yaml` 的 pod 配置文件。
2.  定义要用于访问 {{site.data.keyword.registrylong_notm}} 中映像的 pod 和映像拉取私钥。

    访问专用映像：
    ```
    apiVersion: v1
    kind: Pod
    metadata:
      name: mypod
    spec:
      containers:
        - name: <container_name>
          image: <region>.icr.io/<namespace_name>/<image_name>:<tag>
      imagePullSecrets:
        - name: <secret_name>
    ```
    {: codeblock}

    访问 {{site.data.keyword.Bluemix_notm}} 公共映像：
    ```
    apiVersion: v1
    kind: Pod
    metadata:
      name: mypod
    spec:
      containers:
        - name: <container_name>
          image: icr.io/<image_name>:<tag>
      imagePullSecrets:
        - name: <secret_name>
    ```
    {: codeblock}

    <table>
    <caption>了解 YAML 文件的组成部分</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解 YAML 文件的组成部分</th>
    </thead>
    <tbody>
    <tr>
    <td><code><em>&lt;container_name&gt;</em></code></td>
    <td>要部署到集群的容器的名称。</td>
    </tr>
    <tr>
    <td><code><em>&lt;namespace_name&gt;</em></code></td>
    <td>存储映像的名称空间。要列出可用名称空间，请运行 `ibmcloud cr namespace-list`。</td>
    </tr>
    <tr>
    <td><code><em>&lt;image_name&gt;</em></code></td>
    <td>要使用的映像的名称。要列出 {{site.data.keyword.Bluemix_notm}} 帐户中的可用映像，请运行 `ibmcloud cr image-list`。</td>
    </tr>
    <tr>
    <td><code><em>&lt;tag&gt;</em></code></td>
    <td>要使用的映像的版本。如果未指定标记，那么缺省情况下会使用标记为 <strong>latest</strong> 的映像。</td>
    </tr>
    <tr>
    <td><code><em>&lt;secret_name&gt;</em></code></td>
    <td>先前创建的映像拉取私钥的名称。</td>
    </tr>
    </tbody></table>

3.  保存更改。
4.  在集群中创建部署。
    ```
        kubectl apply -f mypod.yaml
        ```
    {: pre}

### 将映像拉取私钥存储在所选名称空间的 Kubernetes 服务帐户中
{:#store_imagePullSecret}

每个名称空间都具有一个名为 `default` 的 Kubernetes 服务帐户。可以将映像拉取私钥添加到此服务帐户，以授予对注册表中映像的访问权。未指定服务帐户的部署将自动对此名称空间使用 `default` 服务帐户。
{:shortdesc}

1. 检查 default 服务帐户是否已存在映像拉取私钥。
   ```
   kubectl describe serviceaccount default -n <namespace_name>
   ```
   {: pre}
   在**映像拉取私钥**条目中显示 `<none>` 时，说明不存在任何映像拉取私钥。  
2. 将映像拉取私钥添加到 default 服务帐户。
   - **未定义映像拉取私钥时，添加映像拉取私钥：**
       ```
       kubectl patch -n <namespace_name> serviceaccount/default -p '{"imagePullSecrets":[{"name": "<image_pull_secret_name>"}]}'
       ```
       {: pre}
   - **已定义映像拉取私钥时，添加映像拉取私钥：**
       ```
       kubectl patch -n <namespace_name> serviceaccount/default --type='json' -p='[{"op":"add","path":"/imagePullSecrets/-","value":{"name":"<image_pull_secret_name>"}}]'
       ```
       {: pre}
3. 验证映像拉取私钥是否已添加到 default 服务帐户。
   ```
   kubectl describe serviceaccount default -n <namespace_name>
   ```
   {: pre}

   输出示例：
   ```
   Name:                default
   Namespace:           <namespace_name>
   Labels:              <none>
   Annotations:         <none>
   Image pull secrets:  <image_pull_secret_name>
   Mountable secrets:   default-token-sh2dx
   Tokens:              default-token-sh2dx
   Events:              <none>
   ```
   {: pre}

4. 从注册表中的映像部署容器。
   ```
   apiVersion: v1
   kind: Pod
   metadata:
     name: mypod
   spec:
     containers:
       - name: <container_name>
         image: <region>.icr.io/<namespace_name>/<image_name>:<tag>
   ```
   {: codeblock}

5. 在集群中创建部署。
   ```
        kubectl apply -f mypod.yaml
        ```
   {: pre}

<br />


## 不推荐：使用注册表令牌通过 {{site.data.keyword.registrylong_notm}} 映像部署容器
{: #namespace_token}

可以通过 IBM 提供的公共映像将容器部署到集群，也可以通过存储在 {{site.data.keyword.registryshort_notm}} 的名称空间中的专用映像来部署。
现有集群使用存储在集群 `imagePullSecret` 中的注册表[令牌](/docs/services/Registry?topic=registry-registry_access#registry_tokens)来授予从 `registry.bluemix.net` 域名中拉取映像的访问权。
{:shortdesc}

创建集群时，会自动为[离得最近的区域注册表和全局注册表](/docs/services/Registry?topic=registry-registry_overview#registry_regions)创建不到期的注册表令牌和私钥。全局注册表用于安全地存储 IBM 提供的公共映像，您可以在不同部署中引用这些映像，而不用分别引用存储在各个区域注册表中的映像。区域注册表用于安全地存储您自己的专用 Docker 映像。
令牌用于授予对 {{site.data.keyword.registryshort_notm}} 中设置的任一名称空间的只读访问权，以便您可以使用这些公共（全局注册表）映像和专用（区域注册表）映像。

每个令牌必须存储在 Kubernetes `imagePullSecret` 中，才能在部署容器化应用程序时供 Kubernetes 集群访问。创建集群时，{{site.data.keyword.containerlong_notm}} 会自动将全局（IBM 提供的公共映像）和区域注册表的令牌存储在 Kubernetes 映像拉取私钥中。映像拉取私钥会添加到 `default` Kubernetes 名称空间、`kube-system` 名称空间以及这些名称空间的 `default` 服务帐户中的私钥列表。

`registry.bluemix.net` 域名支持使用令牌来授权集群访问 {{site.data.keyword.registrylong_notm}}，但不推荐使用此方法。请改为[使用 API 密钥方法](#cluster_registry_auth)来授权集群访问新的 `icr.io` 注册表域名。
{: deprecated}

根据映像的位置和容器的位置，必须通过执行不同的步骤来部署容器。
*   [使用位于集群所在区域中的映像，将容器部署到 `default` Kubernetes 名称空间](#token_default_namespace)
*   [将容器部署到非 `default` Kubernetes 名称空间](#token_copy_imagePullSecret)
*   [使用不在集群所在区域或 {{site.data.keyword.Bluemix_notm}} 帐户中的映像来部署容器](#token_other_regions_accounts)
*   [使用非 IBM 专用注册表中的映像来部署容器](#private_images)

使用此初始设置时，可以通过 {{site.data.keyword.Bluemix_notm}} 帐户的名称空间中可用的任何映像，将容器部署到集群的**缺省**名称空间。要将容器部署到集群的其他名称空间，或者要使用存储在其他 {{site.data.keyword.Bluemix_notm}} 区域或其他 {{site.data.keyword.Bluemix_notm}} 帐户中的映像，必须[为集群创建您自己的映像拉取私钥](#other)。
{: note}

### 不推荐：使用注册表令牌将映像部署到 `default` Kubernetes 名称空间
{: #token_default_namespace}

使用存储在映像拉取私钥中的注册表令牌时，可以通过区域 {{site.data.keyword.registrylong_notm}} 中可用的任何映像，将容器部署到集群的 **default** 名称空间中。
{: shortdesc}

开始之前：
1. [在 {{site.data.keyword.registryshort_notm}} 中设置名称空间，并将映像推送到此名称空间](/docs/services/Registry?topic=registry-getting-started#gs_registry_namespace_add)。
2. [创建集群](/docs/containers?topic=containers-clusters#clusters_ui)。
3. [设定 CLI 的目标为集群](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)。

要将容器部署到集群的**缺省**名称空间，请创建配置文件。

1.  创建名为 `mydeployment.yaml` 的部署配置文件。
2.  定义部署以及要在 {{site.data.keyword.registryshort_notm}} 的名称空间中使用的映像。

    要使用 {{site.data.keyword.registryshort_notm}} 的名称空间中的专用映像，请使用以下内容：


    ```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: <app_name>-deployment
    spec:
      replicas: 3
      selector:
        matchLabels:
          app: <app_name>
      template:
        metadata:
          labels:
            app: <app_name>
        spec:
          containers:
          - name: <app_name>
            image: registry.<region>.bluemix.net/<namespace>/<my_image>:<tag>
    ```
    {: codeblock}

    **提示**：要检索名称空间信息，请运行 `ibmcloud cr namespace-list`。

3.  在集群中创建部署。

    ```
    kubectl apply -f mydeployment.yaml
    ```
    {: pre}

    **提示：**您还可以部署现有配置文件，如 IBM 提供的其中一个公共映像。此示例使用美国南部区域中的 **ibmliberty** 映像。

    ```
    kubectl apply -f https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/deploy-apps-clusters/deploy-ibmliberty.yaml
    ```
    {: pre}

### 不推荐：将基于令牌的映像拉取私钥从 default 名称空间复制到集群中的其他名称空间
{: #token_copy_imagePullSecret}

可以将自动为 `default` Kubernetes 名称空间创建的具有注册表令牌凭证的映像拉取私钥复制到集群中的其他名称空间。
{: shortdesc}

1. 列出集群中的可用名称空间。
   ```
        kubectl get namespaces
        ```
   {: pre}

   输出示例：
   ```
   default          Active    79d
   ibm-cert-store   Active    79d
   ibm-system       Active    79d
   istio-system     Active    34d
   kube-public      Active    79d
   kube-system      Active    79d
   ```
   {: screen}

2. 可选：在集群中创建名称空间。
   ```
        kubectl create namespace <namespace_name>
        ```
   {: pre}

3. 将映像拉取私钥从 `default` 名称空间复制到您选择的名称空间。新映像拉取私钥的名称为 `bluemix-<namespace_name>-secret-regional` and `bluemix-<namespace_name>-secret-international`。
   ```
   kubectl get secret bluemix-default-secret-regional -o yaml | sed 's/default/<namespace_name>/g' | kubectl -n <namespace_name> create -f -
   ```
   {: pre}

   ```
   kubectl get secret bluemix-default-secret-international -o yaml | sed 's/default/<namespace_name>/g' | kubectl -n <namespace_name> create -f -
   ```
   {: pre}

4.  验证私钥是否已成功创建。
    ```
    kubectl get secrets --namespace <namespace_name>
    ```
    {: pre}

5. 在名称空间中[使用 `imagePullSecret` 部署容器](#use_imagePullSecret)。


### 不推荐：创建基于令牌的映像拉取私钥以访问其他 {{site.data.keyword.Bluemix_notm}} 区域和帐户中的映像
{: #token_other_regions_accounts}

要访问其他 {{site.data.keyword.Bluemix_notm}} 区域或帐户中的映像，必须创建注册表令牌，并将凭证保存在映像拉取私钥中。
{: shortdesc}

1.  如果还没有令牌，请[为要访问的注册表创建令牌](/docs/services/Registry?topic=registry-registry_access#registry_tokens_create)。
2.  列出您的 {{site.data.keyword.Bluemix_notm}} 帐户中的令牌。

    ```
    ibmcloud cr token-list
    ```
    {: pre}

3.  记下要使用的令牌标识。
4.  检索令牌的值。将 <em>&lt;token_ID&gt;</em> 替换为在上一步中检索到的令牌的标识。

    ```
    ibmcloud cr token-get <token_id>
    ```
    {: pre}

    令牌值会显示在 CLI 输出的**令牌**字段中。

5.  创建 Kubernetes 私钥以用于存储令牌信息。

    ```
    kubectl --namespace <kubernetes_namespace> create secret docker-registry <secret_name>  --docker-server=<registry_URL> --docker-username=token --docker-password=<token_value> --docker-email=<docker_email>
    ```
    {: pre}

    <table>
    <caption>了解此命令的组成部分</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解此命令的组成部分</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace <em>&lt;kubernetes_namespace&gt;</em></code></td>
    <td>必需。要使用私钥并将容器部署到的集群的 Kubernetes 名称空间。运行 <code>kubectl get namespaces</code> 可列出集群中的所有名称空间。</td>
    </tr>
    <tr>
    <td><code><em>&lt;secret_name&gt;</em></code></td>
    <td>必需。要用于映像拉取私钥的名称。</td>
    </tr>
    <tr>
    <td><code>--docker-server <em>&lt;registry_URL&gt;</em></code></td>
    <td>必需。在其中设置名称空间的映像注册表的 URL。<ul><li>对于在美国南部和美国东部设置的名称空间：<code>registry.ng.bluemix.net</code></li><li>对于在英国南部设置的名称空间：<code>registry.eu-gb.bluemix.net</code></li><li>对于在欧洲中部（法兰克福）设置的名称空间：<code>registry.eu-de.bluemix.net</code></li><li>对于在澳大利亚（悉尼）设置的名称空间：<code>registry.au-syd.bluemix.net</code></li></ul></td>
    </tr>
    <tr>
    <td><code>--docker-username <em>&lt;docker_username&gt;</em></code></td>
    <td>必需。用于登录到专用注册表的用户名。对于 {{site.data.keyword.registryshort_notm}}，用户名设置为值 <strong><code>token</code></strong>。</td>
    </tr>
    <tr>
    <td><code>--docker-password <em>&lt;token_value&gt;</em></code></td>
    <td>必需。先前检索到的注册表令牌的值。</td>
    </tr>
    <tr>
    <td><code>--docker-email <em>&lt;docker-email&gt;</em></code></td>
    <td>必需。如果您有 Docker 电子邮件地址，请输入该地址。如果没有，请输入虚构的电子邮件地址，例如 a@b.c。此电子邮件对于创建 Kubernetes 私钥是必需的，但在创建后不会再使用此电子邮件。</td>
    </tr>
    </tbody></table>

6.  验证私钥是否已成功创建。将 <em>&lt;kubernetes_namespace&gt;</em> 替换为在其中创建映像拉取私钥的名称空间。

    ```
    kubectl get secrets --namespace <kubernetes_namespace>
    ```
    {: pre}

7.  在名称空间中[使用映像拉取私钥部署容器](#use_imagePullSecret)。
