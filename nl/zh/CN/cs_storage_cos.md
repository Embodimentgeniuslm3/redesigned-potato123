---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-03"

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


# 在 IBM Cloud Object Storage 上存储数据
{: #object_storage}

[{{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-about) 是一种持久的高可用性存储器，可以使用 {{site.data.keyword.cos_full_notm}} 插件安装到在 Kubernetes 集群中运行的应用程序。这是一种 Kubernetes Flex-Volume 插件，用于将 Cloud {{site.data.keyword.cos_short}} 存储区连接到集群中的 pod。使用 {{site.data.keyword.cos_full_notm}} 存储的信息会进行动态和静态加密，分布在多个地理位置，并使用 REST API 通过 HTTP 进行访问。
{: shortdesc}

要连接到 {{site.data.keyword.cos_full_notm}}，集群需要公用网络访问权通过 {{site.data.keyword.Bluemix_notm}} Identity and Access Management 进行认证。如果您有仅专用集群，那么安装插件 V`1.0.3` 或更高版本后，可与 {{site.data.keyword.cos_full_notm}} 专用服务端点进行通信，并可设置 {{site.data.keyword.cos_full_notm}} 服务实例进行 HMAC 认证。如果不想使用 HMAC 认证，那么必须在端口 443 上打开所有出站网络流量，该插件才能在专用集群中正常工作。
{: important}

对于 V1.0.5，{{site.data.keyword.cos_full_notm}} 插件已从 `ibmcloud-object-storage-plugin` 重命名为 `ibm-object-storage-plugin`。要安装新版本的插件，必须[卸载旧的 Helm chart 安装](#remove_cos_plugin)，然后[使用新的 {{site.data.keyword.cos_full_notm}} 插件版本重新安装 Helm chart](#install_cos)。
{: note}

## 创建 Object Storage 服务实例
{: #create_cos_service}

您必须在自己的帐户中供应 {{site.data.keyword.cos_full_notm}} 服务实例后，才能开始在集群中使用对象存储器。
{: shortdesc}

{{site.data.keyword.cos_full_notm}} 插件配置为使用任何 s3 API 端点。例如，您可能希望使用本地 Cloud Object Storage 服务器（如 [Minio](https://cloud.ibm.com/kubernetes/solutions/helm-charts/ibm-charts/ibm-minio-objectstore)），或者连接到在不同云提供者处设置的 s3 API 端点，而不使用 {{site.data.keyword.cos_full_notm}} 服务实例。

要创建 {{site.data.keyword.cos_full_notm}} 服务实例，请执行以下步骤。如果计划使用本地 Cloud Object Storage 服务器或其他 s3 API 端点，请参阅提供者文档来设置 Cloud Object Storage 实例。

1. 部署 {{site.data.keyword.cos_full_notm}} 服务实例。
   1.  打开 [{{site.data.keyword.cos_full_notm}} 目录页面](https://cloud.ibm.com/catalog/services/cloud-object-storage)。
   2.  输入服务实例的名称（例如，`cos-backup`），然后选择集群所在的资源组。要查看集群的资源组，请运行 `ibmcloud ks cluster-get --cluster <cluster_name_or_ID>`。   
   3.  查看[套餐选项 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api) 以获取定价信息，并选择套餐。
   4.  单击**创建**。这将打开服务详细信息页面。
2. {: #service_credentials}检索 {{site.data.keyword.cos_full_notm}} 服务凭证。
   1.  在服务详细信息页面上的导航中，单击**服务凭证**。
   2.  单击**新建凭证**。这将显示一个对话框。
   3.  输入凭证的名称。
   4.  从**角色**下拉列表中，选择`写入者`或`管理者`。选择`读取者`时，您将无法使用凭证在 {{site.data.keyword.cos_full_notm}} 中创建存储区并向其写入数据。
   5.  可选：在**添加内联配置参数（可选）**中，输入 `{"HMAC":true}` 来为 {{site.data.keyword.cos_full_notm}} 服务创建其他 HMAC 凭证。HMAC 认证通过防止误用到期或随机创建的 OAuth2 令牌，为 OAuth2 认证添加了一层额外的安全性。**重要信息**：如果您拥有的是没有公共访问权的仅专用集群，那么必须使用 HMAC 认证，这样才能通过专用网络来访问 {{site.data.keyword.cos_full_notm}} 服务。
   6.  单击**添加**。新凭证会在**服务凭证**表中列出。
   7.  单击**查看凭证**。
   8.  记下 **apikey** 以使用 OAuth2 令牌向 {{site.data.keyword.cos_full_notm}} 服务进行认证。对于 HMAC 认证，请记下 **cos_hmac_keys** 部分中的 **access_key_id** 和 **secret_access_key**。
3. [将服务凭证存储在集群内的 Kubernetes 私钥中](#create_cos_secret)，以启用对 {{site.data.keyword.cos_full_notm}} 服务实例的访问。

## 为 Object Storage 服务凭证创建私钥
{: #create_cos_secret}

要访问 {{site.data.keyword.cos_full_notm}} 服务实例来读写数据，必须将服务凭证安全地存储在 Kubernetes 私钥中。{{site.data.keyword.cos_full_notm}} 插件会将这些凭证用于存储区的每个读或写操作。
{: shortdesc}

要为 {{site.data.keyword.cos_full_notm}} 服务实例的凭证创建 Kubernetes 私钥，请执行以下步骤。如果计划使用本地 Cloud Object Storage 服务器或其他 s3 API 端点，请使用相应的凭证创建 Kubernetes 私钥。

开始之前：[登录到您的帐户。如果适用，请将相应的资源组设定为目标。为集群设置上下文。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1. 检索 [{{site.data.keyword.cos_full_notm}} 服务凭证](#service_credentials)的 **apikey** 或者 **access_key_id** 和 **secret_access_key**。

2. 获取 {{site.data.keyword.cos_full_notm}} 服务实例的 **GUID**。
   ```
   ibmcloud resource service-instance <service_name> | grep GUID
   ```
   {: pre}

3. 创建 Kubernetes 私钥以用于存储服务凭证。创建私钥时，所有值都会自动编码为 Base64。

   **使用 API 密钥的示例：**
   ```
   kubectl create secret generic cos-write-access --type=ibm/ibmc-s3fs --from-literal=api-key=<api_key> --from-literal=service-instance-id=<service_instance_guid>
   ```
   {: pre}

   **HMAC 认证的示例：**
   ```
   kubectl create secret generic cos-write-access --type=ibm/ibmc-s3fs --from-literal=access-key=<access_key_ID> --from-literal=secret-key=<secret_access_key>    
   ```
   {: pre}

   <table>
   <caption>了解命令的组成部分</caption>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解命令的组成部分</th>
   </thead>
   <tbody>
   <tr>
   <td><code>api-key</code></td>
   <td>输入先前从 {{site.data.keyword.cos_full_notm}} 服务凭证中检索到的 API 密钥。如果要使用 HMAC 认证，请改为指定 <code>access-key</code> 和 <code>secret-key</code>。</td>
   </tr>
   <tr>
   <td><code>access-key</code></td>
   <td>输入先前从 {{site.data.keyword.cos_full_notm}} 服务凭证中检索到的访问密钥标识。如果要使用 OAuth2 认证，请改为指定 <code>api-key</code>。</td>
   </tr>
   <tr>
   <td><code>secret-key</code></td>
   <td>输入先前从 {{site.data.keyword.cos_full_notm}} 服务凭证中检索到的访问密钥。如果要使用 OAuth2 认证，请改为指定 <code>api-key</code>。</td>
   </tr>
   <tr>
   <td><code>service-instance-id</code></td>
   <td>输入先前检索到的 {{site.data.keyword.cos_full_notm}} 服务实例的 GUID。</td>
   </tr>
   </tbody>
   </table>

4. 验证是否已在名称空间中创建私钥。
   ```
   kubectl get secret
   ```
   {: pre}

5. [安装 {{site.data.keyword.cos_full_notm}} 插件](#install_cos)，或者如果已安装该插件，请针对 {{site.data.keyword.cos_full_notm}} 存储区[决定配置]( #configure_cos)。

## 安装 IBM Cloud Object Storage 插件
{: #install_cos}

通过 Helm chart 来安装 {{site.data.keyword.cos_full_notm}} 插件，以便为 {{site.data.keyword.cos_full_notm}} 设置预定义的存储类。可以使用这些存储类来创建用于为应用程序供应 {{site.data.keyword.cos_full_notm}} 的 PVC。
{: shortdesc}

在查找有关如何更新或除去 {{site.data.keyword.cos_full_notm}} 插件的指示信息？请参阅[更新插件](#update_cos_plugin)和[除去插件](#remove_cos_plugin)。
{: tip}

开始之前：[登录到您的帐户。如果适用，请将相应的资源组设定为目标。为集群设置上下文。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1. 确保工作程序节点应用次版本的最新补丁。
   1. 列出工作程序节点的当前补丁版本。
      ```
   ibmcloud ks workers --cluster <cluster_name_or_ID>
   ```
      {: pre}

      输出示例：
        ```
      OK
      ID                                                  Public IP        Private IP     Machine Type           State    Status   Zone    Version
      kube-dal10-crb1a23b456789ac1b20b2nc1e12b345ab-w26   169.xx.xxx.xxx    10.xxx.xx.xxx   b3c.4x16.encrypted     normal   Ready    dal10   1.13.6_1523*
      ```
      {: screen}

      如果工作程序节点未应用最新补丁版本，那么在 CLI 输出的 **Version** 列中会显示一个星号 (`*`)。

   2. 查看[版本更改日志](/docs/containers?topic=containers-changelog#changelog)，以查找最新补丁版本中包含的更改。

   3. 通过重新装入工作程序节点来应用最新的补丁版本。请遵循 [ibmcloud ks worker-reload 命令](/docs/containers?topic=containers-cs_cli_reference#cs_worker_reload)中的指示信息执行操作，以便在重新装入工作程序节点之前，正常重新安排工作程序节点上任何正在运行的 pod。请注意，在重新装入期间，工作程序节点机器将使用最新映像进行更新，并且如果数据未[存储在工作程序节点外部](/docs/containers?topic=containers-storage_planning#persistent_storage_overview)，那么将删除数据。

2.  选择是要安装使用还是不使用 Helm 服务器 Tiller 的 {{site.data.keyword.cos_full_notm}} 插件。然后，[按照指示信息](/docs/containers?topic=containers-helm#public_helm_install)在本地计算机上安装 Helm 客户机以及（如果要使用 Tiller）使用服务帐户在集群中安装 Tiller。

3. 如果要安装使用 Tiller 的插件，请验证 Tiller 是否已随服务帐户一起安装。
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

4. 将 {{site.data.keyword.Bluemix_notm}} Helm 存储库添加到集群。
   ```
   helm repo add iks-charts https://icr.io/helm/iks-charts
   ```
   {: pre}

4. 更新 Helm 存储库以在此存储库中检索所有最新版本的 Helm chart。
   ```
   helm repo update
   ```
   {: pre}

5. 下载 Helm chart，并将这些图表解包到当前目录。
   ```
   helm fetch --untar iks-charts/ibm-object-storage-plugin
   ```
   {: pre}

7. 如果使用的是 OS X 或 Linux 分发版，请安装 {{site.data.keyword.cos_full_notm}} Helm 插件 `ibmc`。该插件用于自动检索集群位置，并在存储类中为 {{site.data.keyword.cos_full_notm}} 存储区设置 API 端点。如果使用的是 Windows 操作系统，请继续执行下一步。
   1. 安装 Helm 插件。
      ```
      helm plugin install ./ibm-object-storage-plugin/helm-ibmc
      ```
      {: pre}

      输出示例：
        ```
      Installed plugin: ibmc
      ```
      {: screen}
      
      如果看到错误 `Error: plugin already exists`，请通过运行 `rm -rf ~/.helm/plugins/helm-ibmc` 来除去 `ibmc` Helm 插件。
      {: tip}

   2. 验证 `ibmc` 插件是否已成功安装。
      ```
   helm ibmc --help
   ```
      {: pre}

      输出示例：
        ```
      Install or upgrade Helm charts in IBM K8S Service(IKS) and IBM Cloud Private(ICP)

      Available Commands:
         helm ibmc install [CHART][flags]                      Install a Helm chart
         helm ibmc upgrade [RELEASE][CHART] [flags]            Upgrade the release to a new version of the Helm chart
         helm ibmc template [CHART][flags] [--apply|--delete]  Install/uninstall a Helm chart without tiller

      Available Flags:
         -h, --help                    (Optional) This text.
         -u, --update                  （可选）将此插件更新到最新版本

   

      Example Usage:
         With Tiller:
             Install:   helm ibmc install iks-charts/ibm-object-storage-plugin --name ibm-object-storage-plugin
         Without Tiller:
             Install:   helm ibmc template iks-charts/ibm-object-storage-plugin --apply
             Dry-run:   helm ibmc template iks-charts/ibm-object-storage-plugin
             Uninstall: helm ibmc template iks-charts/ibm-object-storage-plugin --delete

      Note:
         1. It is always recommended to install latest version of ibm-object-storage-plugin chart.
         2. It is always recommended to have 'kubectl' client up-to-date.
      ```
      {: screen}

      如果输出显示错误 `Error: fork/exec /home/iksadmin/.helm/plugins/helm-ibmc/ibmc.sh: permission denied`，请运行 `chmod 755 ~/.helm/plugins/helm-ibmc/ibmc.sh`。然后，重新运行 `helm ibmc --help`。
      {: tip}

8. 可选：将 {{site.data.keyword.cos_full_notm}} 插件限制为仅访问用于保存 {{site.data.keyword.cos_full_notm}} 服务凭证的 Kubernetes 私钥。缺省情况下，该插件有权访问集群中的所有 Kubernetes 私钥。
   1. [创建 {{site.data.keyword.cos_full_notm}} 服务实例](#create_cos_service)。
   2. [将 {{site.data.keyword.cos_full_notm}} 服务凭证存储在 Kubernetes 私钥中](#create_cos_secret)。
   3. 浏览到 `templates` 目录并列出可用文件。  
      ```
      cd ibm-object-storage-plugin/templates && ls
      ```
      {: pre}

   4. 打开 `provisioner-sa.yaml` 文件，并查找 `ibmcloud-object-storage-secret-reader` `ClusterRole` 定义。
   6. 将先前创建的私钥的名称添加到 `resourceNames` 部分中该插件有权访问的密码的列表。
      ```
      kind: ClusterRole
      apiVersion: rbac.authorization.k8s.io/v1beta1
      metadata:
        name: ibmcloud-object-storage-secret-reader
      rules:
      - apiGroups: [""]
          resources: ["secrets"]
        resourceNames: ["<secret_name1>","<secret_name2>"]
        verbs: ["get"]
      ```
      {: codeblock}
   7. 保存更改。

9. 安装 {{site.data.keyword.cos_full_notm}} 插件。安装该插件时，会将预定义的存储类添加到集群中。

   - **对于 OS X 和 Linux：**
     - 如果跳过了前一个步骤，那么安装时不会限制为使用特定 Kubernetes 私钥。</br>
       **不使用 Tiller**：
       ```
       helm ibmc template iks-charts/ibm-object-storage-plugin --apply
       ```
       {: pre}
       
       **使用 Tiller**：
       ```
       helm ibmc install iks-charts/ibm-object-storage-plugin --name ibm-object-storage-plugin
       ```
       {: pre}

     - 如果完成了前一个步骤，那么安装时会限制为使用特定 Kubernetes 私钥。</br>
       **不使用 Tiller**：
       ```
       cd ../..
       helm ibmc template ./ibm-object-storage-plugin --apply 
       ```
       {: pre}
       
       **使用 Tiller**：
       ```
       cd ../..
       helm ibmc install ./ibm-object-storage-plugin --name ibm-object-storage-plugin
       ```
       {: pre}

   - **对于 Windows：**
     1. 检索部署了集群的专区，并将该专区存储在环境变量中。
        ```
        export DC_NAME=$(kubectl get cm cluster-info -n kube-system -o jsonpath='{.data.cluster-config\.json}' | grep datacenter | awk -F ': ' '{print $2}' | sed 's/\"//g' |sed 's/,//g')
        ```
        {: pre}

     2. 验证环境变量是否已设置。
        ```
        printenv
        ```
        {: pre}

     3. 安装 Helm chart。
        - 如果跳过了前一个步骤，那么安装时不会限制为使用特定 Kubernetes 私钥。</br>
          **不使用 Tiller**：
          ```
          helm ibmc template iks-charts/ibm-object-storage-plugin --apply
          ```
          {: pre}
          
          **使用 Tiller**：
          ```
          helm ibmc install iks-charts/ibm-object-storage-plugin --name ibm-object-storage-plugin
          ```
          {: pre}

        - 如果完成了前一个步骤，那么安装时会限制为使用特定 Kubernetes 私钥。</br>
          **不使用 Tiller**：
          ```
          cd ../..
          helm ibmc template ./ibm-object-storage-plugin --apply 
          ```
          {: pre}
          
          **使用 Tiller**：
          ```
          cd ../..
          helm ibmc install ./ibm-object-storage-plugin --name ibm-object-storage-plugin
          ```
          {: pre}


   不使用 Tiller 进行安装的示例输出：
   ```
   Rendering the Helm chart templates...
   DC: dal10
   Chart: iks-charts/ibm-object-storage-plugin
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-cold-cross-region.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-cold-regional.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-flex-cross-region.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-flex-perf-cross-region.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-flex-perf-regional.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-flex-regional.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-standard-cross-region.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-standard-perf-cross-region.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-standard-perf-regional.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-standard-regional.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-vault-cross-region.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-vault-regional.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/flex-driver-sa.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/provisioner-sa.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/flex-driver.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/tests/check-driver-install.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/provisioner.yaml
   Installing the Helm chart...
   serviceaccount/ibmcloud-object-storage-driver created
   daemonset.apps/ibmcloud-object-storage-driver created
   storageclass.storage.k8s.io/ibmc-s3fs-cold-cross-region created
   storageclass.storage.k8s.io/ibmc-s3fs-cold-regional created
   storageclass.storage.k8s.io/ibmc-s3fs-flex-cross-region created
   storageclass.storage.k8s.io/ibmc-s3fs-flex-perf-cross-region created
   storageclass.storage.k8s.io/ibmc-s3fs-flex-perf-regional created
   storageclass.storage.k8s.io/ibmc-s3fs-flex-regional created
   storageclass.storage.k8s.io/ibmc-s3fs-standard-cross-region created
   storageclass.storage.k8s.io/ibmc-s3fs-standard-perf-cross-region created
   storageclass.storage.k8s.io/ibmc-s3fs-standard-perf-regional created
   storageclass.storage.k8s.io/ibmc-s3fs-standard-regional created
   storageclass.storage.k8s.io/ibmc-s3fs-vault-cross-region created
   storageclass.storage.k8s.io/ibmc-s3fs-vault-regional created
   serviceaccount/ibmcloud-object-storage-plugin created
   clusterrole.rbac.authorization.k8s.io/ibmcloud-object-storage-plugin created
   clusterrole.rbac.authorization.k8s.io/ibmcloud-object-storage-secret-reader created
   clusterrolebinding.rbac.authorization.k8s.io/ibmcloud-object-storage-plugin created
   clusterrolebinding.rbac.authorization.k8s.io/ibmcloud-object-storage-secret-reader created
   deployment.apps/ibmcloud-object-storage-plugin created
   pod/ibmcloud-object-storage-driver-test created
   ```
   {: screen}

10. 验证插件是否已正确安装。
    ```
    kubectl get pod -n kube-system -o wide | grep object
    ```
    {: pre}

    输出示例：
    ```
       ibmcloud-object-storage-driver-9n8g8                              1/1       Running   0          2m
   ibmcloud-object-storage-plugin-7c774d484b-pcnnx                   1/1       Running   0          2m
   ```
    {: screen}

    看到一个 `ibmcloud-object-storage-plugin` pod 以及一个或多个 `ibmcloud-object-storage-driver` pod 时，说明安装成功。`ibmcloud-object-storage-driver` pod 的数量等于集群中的工作程序节点数。所有 pod 都必须处于 `Running` 状态，插件才能正常运行。如果 pod 发生故障，请运行 `kubectl describe pod -n kube-system <pod_name>` 来查找故障的根本原因。

11. 验证存储类是否已成功创建。
    ```
   kubectl get storageclass | grep s3
   ```
    {: pre}

    输出示例：
    ```
    ibmc-s3fs-cold-cross-region            ibm.io/ibmc-s3fs   8m
    ibmc-s3fs-cold-regional                ibm.io/ibmc-s3fs   8m
    ibmc-s3fs-flex-cross-region            ibm.io/ibmc-s3fs   8m
    ibmc-s3fs-flex-perf-cross-region       ibm.io/ibmc-s3fs   8m
    ibmc-s3fs-flex-perf-regional           ibm.io/ibmc-s3fs   8m
    ibmc-s3fs-flex-regional                ibm.io/ibmc-s3fs   8m
    ibmc-s3fs-standard-cross-region        ibm.io/ibmc-s3fs   8m
    ibmc-s3fs-standard-perf-cross-region   ibm.io/ibmc-s3fs   8m
    ibmc-s3fs-standard-perf-regional       ibm.io/ibmc-s3fs   8m
    ibmc-s3fs-standard-regional            ibm.io/ibmc-s3fs   8m
    ibmc-s3fs-vault-cross-region           ibm.io/ibmc-s3fs   8m
    ibmc-s3fs-vault-regional               ibm.io/ibmc-s3fs   8m
    ```
    {: screen}

12. 对要访问 {{site.data.keyword.cos_full_notm}} 存储区的所有集群重复这些步骤。

### 更新 IBM Cloud Object Storage 插件
{: #update_cos_plugin}

可以将现有 {{site.data.keyword.cos_full_notm}} 插件升级到最新版本。
{: shortdesc}

1. 如果先前安装了名为 `ibmcloud-object-storage-plugin` 的 Helm chart V1.0.4 或更低版本，请从集群中除去此 Helm 安装。然后，重新安装 Helm chart。
   1. 检查集群中是否安装了旧版本的 {{site.data.keyword.cos_full_notm}} Helm chart。  
      ```
   helm ls | grep ibmcloud-object-storage-plugin
   ```
      {: pre}

      输出示例：
        ```
      ibmcloud-object-storage-plugin	1       	Mon Sep 18 15:31:40 2017	DEPLOYED	ibmcloud-object-storage-plugin-1.0.4	default
      ```
      {: screen}

   2. 如果有名为 `ibmcloud-object-storage-plugin` 的 Helm chart V1.0.4 或更低版本，请从集群中除去此 Helm chart。如果有名为 `ibm-object-storage-plugin` 的 Helm chart V1.0.5 或更高版本，请继续执行步骤 2。
      ```
      helm delete --purge ibmcloud-object-storage-plugin
      ```
      {: pre}

   3. 执行[安装 {{site.data.keyword.cos_full_notm}} 插件](#install_cos)中的步骤，以安装最新版本的 {{site.data.keyword.cos_full_notm}} 插件。

2. 更新 {{site.data.keyword.Bluemix_notm}} Helm 存储库以检索此存储库中最新版本的所有 Helm chart。
   ```
   helm repo update
   ```
   {: pre}

3. 如果使用的是 OS X 或 Linux 分发版，请将 Helm 插件 {{site.data.keyword.cos_full_notm}} `ibmc` 更新为最新版本。
   ```
   helm ibmc --update
   ```
   {: pre}

4. 将最新 {{site.data.keyword.cos_full_notm}} Helm chart 下载到本地计算机，然后解压缩该包来查看 `release.md` 文件，以了解最新发行版信息。
   ```
   helm fetch --untar iks-charts/ibm-object-storage-plugin
   ```
   {: pre}

5. 升级该插件。</br>
   **不使用 Tiller**： 
   ```
   helm ibmc template iks-charts/ibm-object-storage-plugin --update
   ```
   {: pre}
     
   **使用 Tiller**： 
   1. 查找 Helm chart 的安装名称。
      ```
      helm ls | grep ibm-object-storage-plugin
      ```
      {: pre}

      输出示例：
        ```
      <helm_chart_name> 	1       	Mon Sep 18 15:31:40 2017	DEPLOYED	ibm-object-storage-plugin-1.0.5	default
      ```
      {: screen}

   2. 将 {{site.data.keyword.cos_full_notm}} Helm chart 升级到最新版本。
      ```   
      helm ibmc upgrade <helm_chart_name> iks-charts/ibm-object-storage-plugin --force --recreate-pods -f
      ```
      {: pre}

6. 验证 `ibmcloud-object-storage-plugin` 是否已成功升级。  
   ```
   kubectl rollout status deployment/ibmcloud-object-storage-plugin -n kube-system
   ```
   {: pre}

   在 CLI 输出中看到 `deployment "ibmcloud-object-storage-plugin" successfully rolled out` 时，说明该插件升级成功。

7. 验证 `ibmcloud-object-storage-driver` 是否已成功升级。
   ```
   kubectl rollout status ds/ibmcloud-object-storage-driver -n kube-system
   ```
   {: pre}

   在 CLI 输出中看到 `daemon set "ibmcloud-object-storage-driver" successfully rolled out` 时，说明升级成功。

8. 验证 {{site.data.keyword.cos_full_notm}} pod 是否处于 `Running` 状态。
   ```
   kubectl get pods -n kube-system -o wide | grep object-storage
   ```
   {: pre}


### 除去 IBM Cloud Object Storage 插件
{: #remove_cos_plugin}

如果不想在集群中供应和使用 {{site.data.keyword.cos_full_notm}}，那么可以卸载该插件。
{: shortdesc}

除去该插件不会除去现有 PVC、PV 或数据。除去该插件时，将从集群中除去所有相关的 pod 和守护程序集。除非直接将应用程序配置为使用 {{site.data.keyword.cos_full_notm}} API，否则无法为集群供应新的 {{site.data.keyword.cos_full_notm}}，也无法使用现有 PVC 和 PV。
{: important}

开始之前：

- [设定 CLI 的目标为集群](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)。
- 确保集群中没有任何使用 {{site.data.keyword.cos_full_notm}} 的 PVC 或 PV。要列出安装特定 PVC 的所有 pod，请运行 `kubectl get pods --all-namespaces-o=jsonpath='{range .items[*]}{"\n"}{.metadata.name}{":\t"}{range .spec.volumes[*]}{.persistentVolumeClaim.claimName}{" "}{end}{end}' | grep "<pvc_name>"`。

要除去该插件，请执行以下操作：

1. 从集群中除去该插件。</br>
   **使用 Tiller**： 
   1. 查找 Helm chart 的安装名称。
      ```
      helm ls | grep object-storage-plugin
      ```
      {: pre}

      输出示例：
        ```
      <helm_chart_name> 	1       	Mon Sep 18 15:31:40 2017	DEPLOYED	ibmcloud-object-storage-plugin-1.0.0	default
      ```
      {: screen}

   2. 通过除去 Helm chart 来删除 {{site.data.keyword.cos_full_notm}} 插件。
      ```
      helm delete --purge <helm_chart_name>
      ```
      {: pre}

   **不使用 Tiller**： 
   ```
   helm ibmc template iks-charts/ibm-object-storage-plugin --delete
   ```
   {: pre}

2. 验证 {{site.data.keyword.cos_full_notm}} pod 是否已除去。
   ```
   kubectl get pod -n kube-system | grep object-storage
   ```
   {: pre}

      如果 CLI 输出中未显示任何 pod，那么表明已成功除去 pod。

3. 验证存储类是否已除去。
   ```
   kubectl get storageclasses | grep s3
   ```
   {: pre}

      如果 CLI 输出中未显示任何存储类，说明存储类除去操作成功。

4. 如果使用的是 OS X 或 Linux 分发版，请除去 Helm 插件 `ibmc`。如果使用的是 Windows，那么此步骤不是必需的。
   1. 除去 `ibmc` 插件。
      ```
   rm -rf ~/.helm/plugins/helm-ibmc
   ```
      {: pre}

   2. 验证 `ibmc` 插件是否已除去。
      ```
   helm plugin list
   ```
      {: pre}

      输出示例：
        ```
        NAME	VERSION	DESCRIPTION
   ```
     {: screen}

     如果 `ibmc` 插件未列在 CLI 输出中，说明 `ibmc` 插件已成功除去。


## 决定 Object Storage 配置
{: #configure_cos}

{{site.data.keyword.containerlong_notm}} 提供了预定义的存储类，可以使用这些类来创建具有特定配置的存储区。
{: shortdesc}

1. 列出 {{site.data.keyword.containerlong_notm}} 中的可用存储类。
    
   ```
   kubectl get storageclasses | grep s3
   ```
   {: pre}

   输出示例：
   ```
   ibmc-s3fs-cold-cross-region            ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-cold-regional                ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-flex-cross-region            ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-flex-perf-cross-region       ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-flex-perf-regional           ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-flex-regional                ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-standard-cross-region        ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-standard-perf-cross-region   ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-standard-perf-regional       ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-standard-regional            ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-vault-cross-region           ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-vault-regional               ibm.io/ibmc-s3fs   8m
   ```
   {: screen}

2. 选择符合您的数据访问需求的存储类。存储类将根据存储容量、读和写操作数以及存储区的出站带宽来确定[定价 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api)。适合您的选项取决于从服务实例读取数据以及将数据写入服务实例的频率。
   - **Standard**：此选项用于频繁访问的热数据。常见用例是 Web 或移动应用程序。
   - **Vault**：此选项用于不经常访问（例如，每月一次或更低频率）的工作负载或冷数据。常见用例是归档、短期数据保留、数字资产保留、磁带更换和灾难恢复。
   - **Cold**：此选项用于很少访问（每 90 天一次或更低频率）的冷数据或不活动数据。常见用例是归档、长期备份、为合规性保留的历史数据或很少访问的工作负载和应用程序。
   - **Flex**：此选项用于未遵循特定使用模式或过大而无法确定或预测使用模式的工作负载和数据。**提示：**请查看此[博客 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/blogs/bluemix/2017/03/interconnect-2017-changing-rules-storage/)，以了解 Flex 存储类与传统存储层工作方式的比较。   

3. 决定用于存储区中所存储数据的弹性级别。
   - **跨区域**：通过此选项，数据将在一个地理位置的三个区域中进行存储，以实现最高可用性。如果您具有跨区域分布的工作负载，那么会将请求路由到离您最近的区域端点。地理位置的 API 端点由先前安装的 `ibmc` Helm 插件根据集群所在位置自动设置。例如，如果集群位于 `US South`，那么存储类会配置为将 `US GEO` API 端点用于存储区。有关更多信息，请参阅[区域和端点](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-endpoints#endpoints)。  
   - **区域**：使用此选项时，数据会在一个区域内的多个专区之间进行复制。如果您的工作负载位于同一区域中，那么会看到相比跨区域设置，等待时间更短，性能更好。区域端点由先前安装的 `ibm` Helm 插件根据集群所在位置自动设置。例如，如果集群位于 `US South`，那么存储类会配置为将 `US South` 用作存储区的区域端点。有关更多信息，请参阅[区域和端点](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-endpoints#endpoints)。

4. 查看存储类的详细 {{site.data.keyword.cos_full_notm}} 存储区配置。
   ```
   kubectl describe storageclass <storageclass_name>
   ```
   {: pre}

   输出示例：
   ```
   Name:                  ibmc-s3fs-standard-cross-region
   IsDefaultClass:        No
   Annotations:           <none>
   Provisioner:           ibm.io/ibmc-s3fs
   Parameters:            ibm.io/chunk-size-mb=16,ibm.io/curl-debug=false,ibm.io/debug-level=warn,ibm.io/iam-endpoint=https://iam.bluemix.net,ibm.io/kernel-cache=true,ibm.io/multireq-max=20,ibm.io/object-store-endpoint=https://s3-api.dal-us-geo.objectstorage.service.networklayer.com,ibm.io/object-store-storage-class=us-standard,ibm.io/parallel-count=2,ibm.io/s3fs-fuse-retry-count=5,ibm.io/stat-cache-size=100000,ibm.io/tls-cipher-suite=AESGCM
   AllowVolumeExpansion:  <unset>
   MountOptions:          <none>
   ReclaimPolicy:         Delete
   VolumeBindingMode:     Immediate
   Events:                <none>
   ```
   {: screen}

   <table>
   <caption>了解存储类的详细信息</caption>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解 YAML 文件的组成部分</th>
   </thead>
   <tbody>
   <tr>
   <td><code>ibm.io/chunk-size-mb</code></td>
   <td>从 {{site.data.keyword.cos_full_notm}} 读取或向其写入的数据区块大小（以兆字节为单位）。名称中具有 <code>perf</code> 的存储类设置为 52 兆字节。名称中没有 <code>perf</code> 的存储类使用 16 兆字节区块。例如，如果要读取 1 GB 的文件，那么该插件会以多个 16 兆字节或 52 兆字节区块的形式来读取此文件。</td>
   </tr>
   <tr>
   <td><code>ibm.io/curl-debug</code></td>
   <td>对发送到 {{site.data.keyword.cos_full_notm}} 服务实例的请求启用日志记录。如果启用了此项，日志将发送到 `syslog`，并且可以[将日志转发到外部日志记录服务器](/docs/containers?topic=containers-health#logging)。缺省情况下，所有存储类都设置为 <strong>false</strong> 以禁用此日志记录功能。</td>
   </tr>
   <tr>
   <td><code>ibm.io/debug-level</code></td>
   <td>由 {{site.data.keyword.cos_full_notm}} 插件设置的日志记录级别。所有存储类都设置有 <strong>WARN</strong> 日志记录级别。</td>
   </tr>
   <tr>
   <td><code>ibm.io/iam-endpoint</code></td>
   <td>{{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM) 的 API 端点。</td>
   </tr>
   <tr>
   <td><code>ibm.io/kernel-cache</code></td>
   <td>为卷安装点启用或禁用内核缓冲区高速缓存。如果启用了此项，那么从 {{site.data.keyword.cos_full_notm}} 读取的数据将存储在内核高速缓存中，以确保对数据进行快速读访问。如果禁用了此项，那么不会对数据进行高速缓存，并且始终从 {{site.data.keyword.cos_full_notm}} 中读取数据。对于 <code>standard</code> 和 <code>flex</code> 存储类，内核高速缓存已启用；对于 <code>cold</code> 和 <code>vault</code> 存储类，内核高速缓存已禁用。</td>
   </tr>
   <tr>
   <td><code>ibm.io/multireq-max</code></td>
   <td>可以发送到 {{site.data.keyword.cos_full_notm}} 服务实例以列出单个目录中文件的最大并行请求数。所有存储类都设置为最多 20 个并行请求。</td>
   </tr>
   <tr>
   <td><code>ibm.io/object-store-endpoint</code></td>
   <td>用于访问 {{site.data.keyword.cos_full_notm}} 服务实例中存储区的 API 端点。此端点是根据集群的区域自动设置的。**注**：如果要访问非集群所在区域中的现有存储区，那么必须创建[定制存储类](/docs/containers?topic=containers-kube_concepts#customized_storageclass)并对存储区使用此 API 端点。</td>
   </tr>
   <tr>
   <td><code>ibm.io/object-store-storage-class</code></td>
   <td>存储类的名称。</td>
   </tr>
   <tr>
   <td><code>ibm.io/parallel-count</code></td>
   <td>可以发送到 {{site.data.keyword.cos_full_notm}} 服务实例以执行单次读或写操作的最大并行请求数。名称中具有 <code>perf</code> 的存储类设置为最多 20 个并行请求。名称中没有 <code>perf</code> 的存储类缺省情况下设置为 2 个并行请求。</td>
   </tr>
   <tr>
   <td><code>ibm.io/s3fs-fuse-retry-count</code></td>
   <td>在操作被视为不成功之前读或写操作的最大重试次数。所有存储类都已设置为最多 5 次重试。</td>
   </tr>
   <tr>
   <td><code>ibm.io/stat-cache-size</code></td>
   <td>{{site.data.keyword.cos_full_notm}} 元数据高速缓存中保存的最大记录数。每个记录最多可占用 0.5 千字节。缺省情况下，所有存储类的最大记录数均设置为 100000。</td>
   </tr>
   <tr>
   <td><code>ibm.io/tls-cipher-suite</code></td>
   <td>通过 HTTPS 端点建立与 {{site.data.keyword.cos_full_notm}} 的连接时必须使用的 TLS 密码套件。密码套件的值必须遵循 [OpenSSL 格式 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.openssl.org/docs/man1.0.2/apps/ciphers.html)。缺省情况下，所有存储类都使用 <strong><code>AESGCM</code></strong> 密码套件。</td>
   </tr>
   </tbody>
   </table>

   有关每个存储类的更多信息，请参阅[存储类参考](#cos_storageclass_reference)。如果要更改任何预设置的值，请创建自己的[定制存储类](/docs/containers?topic=containers-kube_concepts#customized_storageclass)。
   {: tip}

5. 决定存储区的名称。存储区的名称在 {{site.data.keyword.cos_full_notm}} 中必须唯一。您还可以选择通过 {{site.data.keyword.cos_full_notm}} 插件自动创建存储区的名称。要在存储区中组织数据，可以创建子目录。

   您先前选择的存储类确定了整个存储区的定价。不能为子目录定义其他存储类。如果要存储具有不同访问需求的数据，请考虑使用多个 PVC 来创建多个存储区。
   {: note}

6. 选择在删除集群或持久卷声明 (PVC) 后是否要保留数据和存储区。删除 PVC 时，始终会删除 PV。您可以选择是否在删除 PVC 时，还要自动删除数据和存储区。{{site.data.keyword.cos_full_notm}} 服务实例与为数据选择的保留策略无关，并且在删除 PVC 时绝不会除去服务实例。

现在，您已决定好所需的配置，因此您已准备就绪，可[创建 PVC](#add_cos) 来供应 {{site.data.keyword.cos_full_notm}}。

## 向应用程序添加 Object Storage
{: #add_cos}

创建持久卷声明 (PVC) 以便为集群供应 {{site.data.keyword.cos_full_notm}}。
{: shortdesc}

根据在 PVC 中选择的设置，可以通过以下方式供应 {{site.data.keyword.cos_full_notm}}：
- [动态供应](/docs/containers?topic=containers-kube_concepts#dynamic_provisioning)：创建 PVC 时，将在 {{site.data.keyword.cos_full_notm}} 服务实例中自动创建匹配的持久卷 (PV) 和存储区。
- [静态供应](/docs/containers?topic=containers-kube_concepts#static_provisioning)：可以在 PVC 中引用 {{site.data.keyword.cos_full_notm}} 服务实例中的现有存储区。创建 PVC 时，将仅自动创建匹配的 PV，并将其链接到 {{site.data.keyword.cos_full_notm}} 中的现有存储区。

开始之前：
- [创建并准备 {{site.data.keyword.cos_full_notm}} 服务实例](#create_cos_service)。
- [创建私钥以存储 {{site.data.keyword.cos_full_notm}} 服务凭证](#create_cos_secret)。
- [决定 {{site.data.keyword.cos_full_notm}} 的配置](#configure_cos)。

要向集群添加 {{site.data.keyword.cos_full_notm}}，请执行以下操作：

1. 创建配置文件以定义持久卷声明 (PVC)。
   ```
   kind: PersistentVolumeClaim
   apiVersion: v1
   metadata:
     name: <pvc_name>
     namespace: <namespace>
     annotations:
       ibm.io/auto-create-bucket: "<true_or_false>"
       ibm.io/auto-delete-bucket: "<true_or_false>"
       ibm.io/bucket: "<bucket_name>"
       ibm.io/object-path: "<bucket_subdirectory>"
       ibm.io/secret-name: "<secret_name>"
       ibm.io/endpoint: "https://<s3fs_service_endpoint>"
   spec:
     accessModes:
       - ReadWriteOnce
     resources:
       requests:
         storage: 8Gi # Enter a fictitious value
     storageClassName: <storage_class>
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
   <td>输入 PVC 的名称。</td>
   </tr>
   <tr>
   <td><code>metadata.namespace</code></td>
   <td>输入要在其中创建 PVC 的名称空间。必须在创建了 Kubernetes 私钥用于 {{site.data.keyword.cos_full_notm}} 服务凭证以及要在其中运行 pod 的名称空间中创建 PVC。</td>
   </tr>
   <tr>
   <td><code>ibm.io/auto-create-bucket</code></td>
   <td>在以下选项之间进行选择：<ul><li><strong>true</strong>：创建 PVC 时，将在 {{site.data.keyword.cos_full_notm}} 服务实例中自动创建 PV 和存储区。选择此选项可在 {{site.data.keyword.cos_full_notm}} 服务实例中创建新存储区。</li><li><strong>false</strong>：如果要访问现有存储区中的数据，请选择此选项。创建 PVC 时，会自动创建 PV，并将其链接到您在 <code>ibm.io/bucket</code> 中指定的存储区。</td>
   </tr>
   <tr>
   <td><code>ibm.io/auto-delete-bucket</code></td>
   <td>在以下选项之间进行选择：<ul><li><strong>true</strong>：删除 PVC 时，会自动除去数据、存储区和 PV。{{site.data.keyword.cos_full_notm}} 服务实例会保留而不删除。如果选择将此选项设置为 <strong>true</strong>，那么必须设置 <code>ibm.io/auto-create-bucket: true</code> 和 <code>ibm.io/bucket: ""</code>，以便使用格式为 <code>tmp-s3fs-xxxx</code> 的名称自动创建存储区。</li><li><strong>false</strong>：删除 PVC 时，将自动删除 PV，但 {{site.data.keyword.cos_full_notm}} 服务实例中的数据和存储区会保留。要访问数据，必须使用现有存储区的名称来创建新的 PVC。</li></ul>
   <tr>
   <td><code>ibm.io/bucket</code></td>
   <td>在以下选项之间进行选择：<ul><li>如果 <code>ibm.io/auto-create-bucket</code> 设置为 <strong>true</strong>：输入要在 {{site.data.keyword.cos_full_notm}} 中创建的存储区的名称。如果 <code>ibm.io/auto-delete-bucket</code> 也设置为 <strong>true</strong>，那么必须将此字段保留为空，以自动为存储区指定格式为 <code>tmp-s3fs-xxxx</code> 的名称。该名称在 {{site.data.keyword.cos_full_notm}} 中必须唯一。</li><li>如果 <code>ibm.io/auto-create-bucket</code> 设置为 <strong>false</strong>：输入要在集群中访问的现有存储区的名称。</li></ul> </td>
   </tr>
   <tr>
   <td><code>ibm.io/object-path</code></td>
   <td>可选：输入存储区中要安装的现有子目录的名称。如果要仅安装子目录，而不是安装整个存储区，请使用此选项。要安装子目录，必须设置 <code>ibm.io/auto-create-bucket: "false"</code>，并在 <code>ibm.io/bucket</code> 中提供存储区的名称。</li></ul> </td>
   </tr>
   <tr>
   <td><code>ibm.io/secret-name</code></td>
   <td>输入用于保存先前创建的 {{site.data.keyword.cos_full_notm}} 凭证的私钥的名称。</td>
   </tr>
   <tr>
  <td><code>ibm.io/endpoint</code></td>
  <td>如果 {{site.data.keyword.cos_full_notm}} 服务实例的创建位置与集群所在的位置不同，请输入要使用的 {{site.data.keyword.cos_full_notm}} 服务实例的专用或公共服务端点。有关可用服务端点的概述，请参阅[其他端点信息](/docs/services/cloud-object-storage?topic=cloud-object-storage-advanced-endpoints)。缺省情况下，<code>ibmc</code> Helm 插件会自动检索集群位置，并使用与集群位置匹配的 {{site.data.keyword.cos_full_notm}} 专用服务端点来创建存储类。如果集群位于其中一个大城市专区（例如，`dal10`）中，那么将使用该大城市（在本例中为达拉斯）的 {{site.data.keyword.cos_full_notm}} 专用服务端点。要验证存储类中的服务端点是否与服务实例的服务端点相匹配，请运行 `kubectl describe storageclass <storageclassname>`。确保以 `https://<s3fs_private_service_endpoint>`（对于专用服务端点）或 `http://<s3fs_public_service_endpoint>`（对于公共服务端点）格式输入服务端点。如果存储类中的服务端点与 {{site.data.keyword.cos_full_notm}} 服务实例的服务端点相匹配，请不要在 PVC YAML 文件中包含 <code>ibm.io/endpoint</code> 选项。</td>
  </tr>
   <tr>
   <td><code>resources.requests.storage</code></td>
   <td>{{site.data.keyword.cos_full_notm}} 存储区的虚构大小（以千兆字节为单位）。Kubernetes 需要此大小，但在 {{site.data.keyword.cos_full_notm}} 中并不考虑此大小。所以您可以输入所需的任意大小。您在 {{site.data.keyword.cos_full_notm}} 中使用的实际空间可能有所不同，会根据[定价表 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api) 计费。</td>
   </tr>
   <tr>
   <td><code>spec.storageClassName</code></td>
   <td>在以下选项之间进行选择：<ul><li>如果 <code>ibm.io/auto-create-bucket</code> 设置为 <strong>true</strong>：输入要用于新存储区的存储类。</li><li>如果 <code>ibm.io/auto-create-bucket</code> 设置为 <strong>false</strong>：输入已用于创建现有存储区的存储类。</br></br>如果在 {{site.data.keyword.cos_full_notm}} 服务实例中手动创建了存储区，或者无法记住所使用的存储类，请在 {{site.data.keyword.Bluemix}}“仪表板”中查找服务实例，然后查看现有存储区的<strong>类</strong>和<strong>位置</strong>。然后，使用相应的[存储类](#cos_storageclass_reference)。<p class="note">存储类中设置的 {{site.data.keyword.cos_full_notm}} API 端点基于集群所在的区域。如果要访问非集群所在区域中的存储区，那么必须创建[定制存储类](/docs/containers?topic=containers-kube_concepts#customized_storageclass)并对存储区使用相应的 API 端点。</p></li></ul>  </td>
   </tr>
   </tbody>
   </table>

2. 创建 PVC。
   ```
   kubectl apply -f filepath/pvc.yaml
   ```
   {: pre}

3. 验证 PVC 是否已创建并与 PV 绑定。
   ```
   kubectl get pvc
   ```
   {: pre}

   输出示例：
   ```
   NAME                  STATUS    VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS                     AGE
   s3fs-test-pvc         Bound     pvc-b38b30f9-1234-11e8-ad2b-t910456jbe12   8Gi        RWO            ibmc-s3fs-standard-cross-region  1h
   ```
   {: screen}

4. 可选：如果计划使用非 root 用户访问数据，或者通过直接使用控制台或 API 将文件添加到现有 {{site.data.keyword.cos_full_notm}} 存储区，请确保[文件分配有正确的许可权](/docs/containers?topic=containers-cs_troubleshoot_storage#cos_nonroot_access)，以便应用程序可以根据需要成功地读取和更新文件。

4.  {: #cos_app_volume_mount}要将 PV 安装到部署，请创建配置 `.yaml` 文件，并指定绑定该 PV 的 PVC。

    ```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: <deployment_name>
      labels:
        app: <deployment_label>
    spec:
      selector:
        matchLabels:
          app: <app_name>
      template:
        metadata:
          labels:
            app: <app_name>
        spec:
          containers:
          - image: <image_name>
            name: <container_name>
            securityContext:
              runAsUser: <non_root_user>
              fsGroup: <non_root_user> #仅适用于运行 Kubernetes V1.13 或更高版本的集群
            volumeMounts:
            - name: <volume_name>
              mountPath: /<file_path>
          volumes:
          - name: <volume_name>
            persistentVolumeClaim:
              claimName: <pvc_name>
    ```
    {: codeblock}

    <table>
    <caption>了解 YAML 文件的组成部分</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解 YAML 文件的组成部分</th>
    </thead>
    <tbody>
        <tr>
    <td><code>metadata.labels.app</code></td>
    <td>部署的标签。</td>
      </tr>
      <tr>
        <td><code>spec.selector.matchLabels.app</code> <br/> <code>spec.template.metadata.labels.app</code></td>
        <td>应用程序的标签。</td>
      </tr>
    <tr>
    <td><code>template.metadata.labels.app</code></td>
    <td>部署的标签。</td>
      </tr>
    <tr>
    <td><code>spec.containers.image</code></td>
    <td>要使用的映像的名称。要列出 {{site.data.keyword.registryshort_notm}} 帐户中的可用映像，请运行 `ibmcloud cr image-list`。</td>
    </tr>
    <tr>
    <td><code>spec.containers.name</code></td>
    <td>要部署到集群的容器的名称。</td>
    </tr>
    <tr>
    <td><code>spec.containers.securityContext.runAsUser</code></td>
    <td>可选：要以非 root 用户身份在运行 Kubernetes V1.12 或更低版本的集群中运行应用程序，请通过定义非 root 用户为 pod 指定[安全上下文 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/)，而不在部署 YAML 中同时设置 `fsGroup`。设置 `fsGroup` 将触发 {{site.data.keyword.cos_full_notm}} 插件以在部署 pod 时更新存储区中所有文件的组许可权。更新许可权是一种写操作，会影响性能。根据您拥有的文件数量，更新许可权可能会阻止 pod 启动并进入 <code>Running</code> 状态。</br></br>如果有运行 Kubernetes V1.13 或更高版本的集群以及 {{site.data.keyword.Bluemix_notm}} Object Storage V1.0.4 或更高版本插件，那么可以更改 s3fs 安装点的所有者。要更改所有者，请通过将 `runAsUser` 和 `fsGroup` 设置为要拥有 s3fs 安装点的相同非 root 用户标识，从而指定安全上下文。如果这两个值不匹配，那么安装点会自动由 `root` 用户拥有。</td>
    </tr>
    <tr>
    <td><code>spec.containers.volumeMounts.mountPath</code></td>
    <td>在容器中安装卷的目录的绝对路径。如果要在不同应用程序之间共享卷，可以为每个应用程序指定[卷子路径 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/storage/volumes/#using-subpath)。</td>
    </tr>
    <tr>
    <td><code>spec.containers.volumeMounts.name</code></td>
    <td>要安装到 pod 中的卷的名称。</td>
    </tr>
    <tr>
    <td><code>volumes.name</code></td>
    <td>要安装到 pod 中的卷的名称。通常此名称与 <code>volumeMounts/name</code> 相同。</td>
    </tr>
    <tr>
    <td><code>volumes.persistentVolumeClaim.claimName</code></td>
    <td>绑定要使用的 PV 的 PVC 的名称。</td>
    </tr>
    </tbody></table>

5.  创建部署。
     ```
    kubectl apply -f <local_yaml_path>
    ```
     {: pre}

6.  验证 PV 是否已成功安装。

     ```
    kubectl describe deployment <deployment_name>
    ```
     {: pre}

     安装点位于 **Volume Mounts** 字段中，卷位于 **Volumes** 字段中。

     ```
      Volume Mounts:
           /var/run/secrets/kubernetes.io/serviceaccount from default-token-tqp61 (ro)
           /volumemount from myvol (rw)
     ...
     Volumes:
       myvol:
         Type:	PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
         ClaimName:	mypvc
         ReadOnly:	false
     ```
     {: screen}

7. 验证是否可以将数据写入 {{site.data.keyword.cos_full_notm}} 服务实例。
   1. 登录到安装 PV 的 pod。
      ```
      kubectl exec <pod_name> -it bash
      ```
      {: pre}

   2. 浏览到在应用程序部署中定义的卷安装路径。
   3. 创建文本文件。
      ```
      echo "This is a test" > test.txt
      ```
      {: pre}

   4. 在 {{site.data.keyword.Bluemix}}“仪表板”中，浏览到 {{site.data.keyword.cos_full_notm}} 服务实例。
   5. 在菜单中，选择**存储区**。
   6. 打开存储区，并验证是否可以看到所创建的 `test.txt`。


## 在有状态集内使用对象存储器
{: #cos_statefulset}

如果您有一个有状态的应用程序（如数据库），那么可以创建有状态集，以使用 {{site.data.keyword.cos_full_notm}} 来存储应用程序的数据。或者，可以使用 {{site.data.keyword.Bluemix_notm}} 数据库即服务（例如，{{site.data.keyword.cloudant_short_notm}}），并将数据存储在云中。
{: shortdesc}

开始之前：
- [创建并准备 {{site.data.keyword.cos_full_notm}} 服务实例](#create_cos_service)。
- [创建私钥以存储 {{site.data.keyword.cos_full_notm}} 服务凭证](#create_cos_secret)。
- [决定 {{site.data.keyword.cos_full_notm}} 的配置](#configure_cos)。

部署使用对象存储器的有状态集：

1. 为有状态集和用于公开有状态集的服务创建配置文件。以下示例显示了如何将 NGINX 部署为有 3 个副本的有状态集，其中每个副本使用单独的存储区，或者所有副本共享同一存储区。

   **示例：创建有 3 个副本的有状态集，其中每个副本使用单独的存储区**：
   ```
   apiVersion: v1
   kind: Service
   metadata:
     name: nginx-v01
     namespace: default
     labels:
       app: nginx-v01 # must match spec.template.metadata.labels and spec.selector.matchLabels in stateful set YAML
   spec:
     ports:
     - port: 80
       name: web
     clusterIP: None
     selector:
       app: nginx-v01 # must match spec.template.metadata.labels and spec.selector.matchLabels in stateful set YAML
   ---
   apiVersion: apps/v1
   kind: StatefulSet
   metadata:
     name: web-v01
     namespace: default
   spec:
     selector:
       matchLabels:
         app: nginx-v01 # must match spec.template.metadata.labels in stateful set YAML and metadata.labels in service YAML
     serviceName: "nginx-v01"
     replicas: 3 
     template:
       metadata:
         labels:
           app: nginx-v01 # must match spec.selector.matchLabels in stateful set YAML and metadata.labels in service YAML
       spec:
         terminationGracePeriodSeconds: 10
         containers:
         - name: nginx
           image: k8s.gcr.io/nginx-slim:0.8
           ports:
           - containerPort: 80
             name: web
           volumeMounts:
           - name: mypvc
             mountPath: /usr/share/nginx/html
     volumeClaimTemplates:
     - metadata:
         name: mypvc
         annotations:
           ibm.io/auto-create-bucket: "true"
           ibm.io/auto-delete-bucket: "true"
           ibm.io/bucket: ""
           ibm.io/secret-name: mysecret 
           volume.beta.kubernetes.io/storage-class: ibmc-s3fs-standard-perf-cross-region 
           volume.beta.kubernetes.io/storage-provisioner: ibm.io/ibmc-s3fs
       spec:
         accessModes: [ "ReadWriteOnce" ]
         storageClassName: "ibmc-s3fs-standard-perf-cross-region"
         resources:
           requests:
             storage: 1Gi
   ```
   {: codeblock}

   **示例：创建有 3 个副本的有状态集，其中所有副本共享同一存储区 `mybucket`**：
   ```
   apiVersion: v1
   kind: Service
   metadata:
     name: nginx-v01
     namespace: default
     labels:
       app: nginx-v01 # must match spec.template.metadata.labels and spec.selector.matchLabels in stateful set YAML
   spec:
     ports:
     - port: 80
       name: web
     clusterIP: None
     selector:
       app: nginx-v01 # must match spec.template.metadata.labels and spec.selector.matchLabels in stateful set YAML
   --- 
   apiVersion: apps/v1
   kind: StatefulSet
   metadata:
     name: web-v01
     namespace: default
   spec:
     selector:
       matchLabels:
         app: nginx-v01 # must match spec.template.metadata.labels in stateful set YAML and metadata.labels in service YAML
     serviceName: "nginx-v01"
     replicas: 3 
     template:
       metadata:
         labels:
           app: nginx-v01 # must match spec.selector.matchLabels in stateful set YAML and metadata.labels in service YAML
       spec:
         terminationGracePeriodSeconds: 10
         containers:
         - name: nginx
           image: k8s.gcr.io/nginx-slim:0.8
           ports:
           - containerPort: 80
             name: web
           volumeMounts:
           - name: mypvc
             mountPath: /usr/share/nginx/html
     volumeClaimTemplates:
     - metadata:
         name: mypvc
         annotations:
           ibm.io/auto-create-bucket: "false"
           ibm.io/auto-delete-bucket: "false"
           ibm.io/bucket: mybucket
           ibm.io/secret-name: mysecret
           volume.beta.kubernetes.io/storage-class: ibmc-s3fs-standard-perf-cross-region 
           volume.beta.kubernetes.io/storage-provisioner: ibm.io/ibmc-s3fs
       spec:
         accessModes: [ "ReadOnlyMany" ]
         storageClassName: "ibmc-s3fs-standard-perf-cross-region"
         resources:
           requests:
             storage: 1Gi
   ```
   {: codeblock}


   <table>
    <caption>了解有状态集 YAML 文件的组成部分</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解有状态集 YAML 文件的组成部分</th>
    </thead>
    <tbody>
    <tr>
    <td style="text-align:left"><code>metadata.name</code></td>
    <td style="text-align:left">输入有状态集的名称。输入的名称将用于创建格式如下的 PVC 名称：<code>&lt;volume_name&gt;-&lt;statefulset_name&gt;-&lt;replica_number&gt;</code>。</td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.serviceName</code></td>
    <td style="text-align:left">输入要用于公开有状态集的服务的名称。</td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.replicas</code></td>
    <td style="text-align:left">输入有状态集的副本数。</td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.selector.matchLabels</code></td>
    <td style="text-align:left">输入要包括在有状态集和 PVC 中的所有标签。Kubernetes 无法识别在有状态集的 <code>volumeClaimTemplates</code> 中包含的标签。必须改为在有状态集 YAML 的 <code>spec.selector.matchLabels</code> 和 <code>spec.template.metadata.labels</code> 部分中定义这些标签。要确保将所有有状态集副本都包含在服务的负载均衡中，请包含服务 YAML 的 <code>spec.selector</code> 部分中使用的标签。</td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.template.metadata.labels</code></td>
    <td style="text-align:left">输入已添加到有状态集 YAML 的 <code>spec.selector.matchLabels</code> 部分的标签。</td>
    </tr>
    <tr>
    <td><code>spec.template.spec.</code></br><code>terminationGracePeriodSeconds</code></td>
    <td>输入供 <code>kubelet</code> 正常终止运行有状态集副本的 pod 的时间（秒）。有关更多信息，请参阅[删除 pod ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/tasks/run-application/force-delete-stateful-set-pod/#delete-pods)。</td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.volumeClaimTemplates.</code></br><code>metadata.name</code></td>
    <td style="text-align:left">输入卷的名称。使用在 <code>spec.containers.volumeMount.name</code> 部分中定义的名称。在此输入的名称将用于创建格式如下的 PVC 名称：<code>&lt;volume_name&gt;-&lt;statefulset_name&gt;-&lt;replica_number&gt;</code>。</td>
    </tr>
    <tr>
    <td><code>spec.volumeClaimTemplates.metadata</code></br><code>annotions.ibm.io/auto-create-bucket</code></td>
    <td>在以下选项之间进行选择：<ul><li><strong>true</strong>：选择此选项可自动为每个有状态集副本创建存储区。</li><li><strong>false：</strong>如果要在有状态集副本之间共享现有存储区，请选择此选项。确保在有状态集 YAML 的 <code>spec.volumeClaimTemplates.metadata.annotions.ibm.io/bucket</code> 部分中定义存储区的名称。</li></ul></td>
    </tr>
    <tr>
    <td><code>spec.volumeClaimTemplates.metadata</code></br><code>annotions.ibm.io/auto-delete-bucket</code></td>
    <td>在以下选项之间进行选择：<ul><li><strong>true</strong>：删除 PVC 时，会自动除去数据、存储区和 PV。{{site.data.keyword.cos_full_notm}} 服务实例会保留而不删除。如果选择将此选项设置为 true，那么必须设置 <code>ibm.io/auto-create-bucket: true</code> 和 <code>ibm.io/bucket: ""</code>，以便使用格式为 <code>tmp-s3fs-xxxx</code> 的名称自动创建存储区。</li><li><strong>false</strong>：删除 PVC 时，将自动删除 PV，但 {{site.data.keyword.cos_full_notm}} 服务实例中的数据和存储区会保留。要访问数据，必须使用现有存储区的名称来创建新的 PVC。</li></ul></td>
    </tr>
    <tr>
    <td><code>spec.volumeClaimTemplates.metadata</code></br><code>annotions.ibm.io/bucket</code></td>
    <td>在以下选项之间进行选择：<ul><li><strong>如果 <code>ibm.io/auto-create-bucket</code> 设置为 true</strong>：输入要在 {{site.data.keyword.cos_full_notm}} 中创建的存储区的名称。如果 <code>ibm.io/auto-delete-bucket</code> 也设置为 <strong>true</strong>，那么必须将此字段保留为空，以自动为存储区指定格式为 tmp-s3fs-xxxx 的名称。该名称在 {{site.data.keyword.cos_full_notm}} 中必须唯一。</li><li><strong>如果 <code>ibm.io/auto-create-bucket</code> 设置为 false</strong>：输入要在集群中访问的现有存储区的名称。</li></ul></td>
    </tr>
    <tr>
    <td><code>spec.volumeClaimTemplates.metadata</code></br><code>annotions.ibm.io/secret-name</code></td>
    <td>输入用于保存先前创建的 {{site.data.keyword.cos_full_notm}} 凭证的私钥的名称。</td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.volumeClaimTemplates.metadata.</code></br><code>annotations.volume.beta.</code></br><code>kubernetes.io/storage-class</code></td>
    <td style="text-align:left">输入要使用的存储类。在以下选项之间进行选择：<ul><li><strong>如果 <code>ibm.io/auto-create-bucket</code> 设置为 true</strong>：输入要用于新存储区的存储类。</li><li><strong>如果 <code>ibm.io/auto-create-bucket</code> 设置为 false</strong>：输入已用于创建现有存储区的存储类。</li></ul></br>  要列出现有存储类，请运行 <code>kubectl get storageclasses | grep s3</code>。如果未指定存储类，那么会使用在集群中设置的缺省存储类来创建 PVC。请确保缺省存储类使用的是 <code>ibm.io/ibmc-s3fs</code> 供应程序，以便为有状态集供应对象存储器。</td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.volumeClaimTemplates.</code></br><code>spec.storageClassName</code></td>
    <td>输入在有状态集 YAML 的 <code>spec.volumeClaimTemplates.metadata.annotations.volume.beta.kubernetes.io/storage-class</code> 部分中所输入的存储类。</td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.volumeClaimTemplates.spec.</code></br><code>resource.requests.storage</code></td>
    <td>输入 {{site.data.keyword.cos_full_notm}} 存储区的虚构大小（以千兆字节为单位）。Kubernetes 需要此大小，但在 {{site.data.keyword.cos_full_notm}} 中并不考虑此大小。所以您可以输入所需的任意大小。您在 {{site.data.keyword.cos_full_notm}} 中使用的实际空间可能有所不同，会根据[定价表 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api) 计费。</td>
    </tr>
    </tbody></table>


## 备份和复原数据
{: #cos_backup_restore}

{{site.data.keyword.cos_full_notm}} 设置为向数据提供高耐久性，以便保护数据以免丢失。您可以在 [{{site.data.keyword.cos_full_notm}} 服务术语 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www-03.ibm.com/software/sla/sladb.nsf/sla/bm-7857-03) 中找到 SLA。
{: shortdesc}

{{site.data.keyword.cos_full_notm}} 不提供数据的版本历史记录。如果需要维护和访问较旧版本的数据，您必须设置应用程序来管理数据的历史记录或实施备用备份解决方案。例如，您可能希望将 {{site.data.keyword.cos_full_notm}} 数据存储在内部部署数据库中，或者使用磁带来归档数据。
{: note}

## 存储类参考
{: #cos_storageclass_reference}

### 标准
{: #standard}

<table>
<caption>Object Storage 类：Standard</caption>
<thead>
<th>特征</th>
<th>设置</th>
</thead>
<tbody>
<tr>
<td>名称</td>
<td><code>ibmc-s3fs-standard-cross-region</code></br><code>ibmc-s3fs-standard-perf-cross-region</code></br><code>ibmc-s3fs-standard-regional</code></br><code>ibmc-s3fs-standard-perf-regional</code></td>
</tr>
<tr>
<td>缺省弹性端点</td>
<td>弹性端点是根据集群所在位置自动设置的。有关更多信息，请参阅[区域和端点](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-endpoints#endpoints)。</td>
</tr>
<tr>
<td>区块大小</td>
<td>不含 `perf` 的存储类：16 MB</br>含有 `perf` 的存储类：52 MB</td>
</tr>
<tr>
<td>内核高速缓存</td>
<td>已启用</td>
</tr>
<tr>
<td>计费</td>
<td>按月</td>
</tr>
<tr>
<td>定价</td>
<td>[定价 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api)</td>
</tr>
</tbody>
</table>

### Vault
{: #Vault}

<table>
<caption>Object Storage 类：Vault</caption>
<thead>
<th>特征</th>
<th>设置</th>
</thead>
<tbody>
<tr>
<td>名称</td>
<td><code>ibmc-s3fs-vault-cross-region</code></br><code>ibmc-s3fs-vault-regional</code></td>
</tr>
<tr>
<td>缺省弹性端点</td>
<td>弹性端点是根据集群所在位置自动设置的。有关更多信息，请参阅[区域和端点](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-endpoints#endpoints)。</td>
</tr>
<tr>
<td>区块大小</td>
<td>16 MB</td>
</tr>
<tr>
<td>内核高速缓存</td>
<td>已禁用</td>
</tr>
<tr>
<td>计费</td>
<td>按月</td>
</tr>
<tr>
<td>定价</td>
<td>[定价 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api)</td>
</tr>
</tbody>
</table>

### Cold
{: #cold}

<table>
<caption>Object Storage 类：Cold</caption>
<thead>
<th>特征</th>
<th>设置</th>
</thead>
<tbody>
<tr>
<td>名称</td>
<td><code>ibmc-s3fs-flex-cross-region</code></br><code>ibmc-s3fs-flex-perf-cross-region</code></br><code>ibmc-s3fs-flex-regional</code></br><code>ibmc-s3fs-flex-perf-regional</code></td>
</tr>
<tr>
<td>缺省弹性端点</td>
<td>弹性端点是根据集群所在位置自动设置的。有关更多信息，请参阅[区域和端点](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-endpoints#endpoints)。</td>
</tr>
<tr>
<td>区块大小</td>
<td>16 MB</td>
</tr>
<tr>
<td>内核高速缓存</td>
<td>已禁用</td>
</tr>
<tr>
<td>计费</td>
<td>按月</td>
</tr>
<tr>
<td>定价</td>
<td>[定价 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api)</td>
</tr>
</tbody>
</table>

### Flex
{: #flex}

<table>
<caption>Object Storage 类：Flex</caption>
<thead>
<th>特征</th>
<th>设置</th>
</thead>
<tbody>
<tr>
<td>名称</td>
<td><code>ibmc-s3fs-cold-cross-region</code></br><code>ibmc-s3fs-flex-perf-cross-region</code></br><code>ibmc-s3fs-cold-regional</code></br><code>ibmc-s3fs-flex-perf-regional</code></td>
</tr>
<tr>
<td>缺省弹性端点</td>
<td>弹性端点是根据集群所在位置自动设置的。有关更多信息，请参阅[区域和端点](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-endpoints#endpoints)。</td>
</tr>
<tr>
<td>区块大小</td>
<td>不含 `perf` 的存储类：16 MB</br>含有 `perf` 的存储类：52 MB</td>
</tr>
<tr>
<td>内核高速缓存</td>
<td>已启用</td>
</tr>
<tr>
<td>计费</td>
<td>按月</td>
</tr>
<tr>
<td>定价</td>
<td>[定价 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api)</td>
</tr>
</tbody>
</table>
