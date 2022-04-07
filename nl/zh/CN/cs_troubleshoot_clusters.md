---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-06"

keywords: kubernetes, iks, ImagePullBackOff, registry, image, failed to pull image,

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


# 集群和工作程序节点故障诊断
{: #cs_troubleshoot_clusters}

在使用 {{site.data.keyword.containerlong}} 时，请考虑对集群和工作程序节点进行故障诊断的以下方法。
{: shortdesc}

<p class="tip">如果您有更常规的问题，请尝试[集群调试](/docs/containers?topic=containers-cs_troubleshoot)。
<br>此外，当进行故障诊断时，您可以使用 [{{site.data.keyword.containerlong_notm}} 诊断和调试工具](/docs/containers?topic=containers-cs_troubleshoot#debug_utility)来运行测试并从集群收集相关信息。</p>

## 由于许可权错误而无法创建集群或管理工作程序节点
{: #cs_credentials}

{: tsSymptoms}
您可尝试通过运行下列其中一个命令来管理新集群或现有集群的工作程序节点。
* 供应工作程序：`ibmcloud ks cluster-create`、`ibmcloud ks worker-pool-rebalance` 或 `ibmcloud ks worker-pool-resize`
* 重新装入工作程序：`ibmcloud ks worker-reload` 或 `ibmcloud ks worker-update`
* 重新引导工作程序：`ibmcloud ks worker-reboot`
* 删除工作程序：`ibmcloud ks cluster-rm`、`ibmcloud ks worker-rm`、`ibmcloud ks worker-pool-rebalance` 或 `ibmcloud ks worker-pool-resize`

但是，您会收到类似于下列其中一项的错误消息。

```
我们无法连接到您的 IBM Cloud Infrastructure (SoftLayer) 帐户。创建标准集群要求您有链接到 IBM Cloud Infrastructure (SoftLayer) 帐户条款的现收现付帐户，或者您已使用 {{site.data.keyword.containerlong_notm}} CLI 设置 {{site.data.keyword.Bluemix_notm}} 基础架构 API 密钥。
```
{: screen}

```
{{site.data.keyword.Bluemix_notm}} Infrastructure 异常：必须具有许可权才能订购“项”。
```
{: screen}

```
找不到工作程序。请复查 {{site.data.keyword.Bluemix_notm}} 基础架构许可权。
```
{: screen}

```
{{site.data.keyword.Bluemix_notm}} Infrastructure 异常：用户没有必需的 {{site.data.keyword.Bluemix_notm}} Infrastructure 许可权来添加服务器
```
{: screen}

```
IAM 令牌交换请求失败：无法创建 IMS 门户网站令牌，因为没有 IMS 帐户链接到所选的 BSS 帐户
```
{: screen}

```
无法使用注册表配置集群。请确保您具有 {{site.data.keyword.registrylong_notm}} 的管理员角色。
```
{: screen}

{: tsCauses}
为区域和资源组设置的基础架构凭证缺少相应的[基础架构许可权](/docs/containers?topic=containers-access_reference#infra)。用户的基础架构许可权最常存储为区域和资源组的 [API 密钥](/docs/containers?topic=containers-users#api_key)。较少见的情况是，如果使用[其他 {{site.data.keyword.Bluemix_notm}} 帐户类型](/docs/containers?topic=containers-users#understand_infra)，那么您可能已 [手动设置基础架构凭证](/docs/containers?topic=containers-users#credentials)。如果您使用其他 IBM Cloud Infrastructure (SoftLayer) 帐户来供应基础架构资源，那么您的帐户中还可能有[孤立集群](#orphaned)。

{: tsResolve}
帐户所有者必须正确设置基础架构帐户凭证。凭证取决于使用的基础架构帐户的类型。

开始之前，[登录到您的帐户。如果适用，请将相应的资源组设定为目标。为集群设置上下文。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  确定用于区域和资源组的基础架构许可权的用户凭证。
    1.  检查集群的区域和资源组的 API 密钥。
        ```
        ibmcloud ks api-key-info --cluster <cluster_name_or_ID>
        ```
        {: pre}

        输出示例：
        ```
        Getting information about the API key owner for cluster <cluster_name>...
        OK
        Name                Email   
        <user_name>         <name@email.com>
        ```
        {: screen}
    2.  检查区域和资源组的基础架构帐户是否已手动设置为使用其他 IBM Cloud Infrastructure (SoftLayer) 帐户。
        ```
        ibmcloud ks credential-get --region <us-south>
        ```
        {: pre}

        **凭证设置为使用其他帐户时的输出示例**。在这种情况下，用户的基础架构凭证将用于目标区域和资源组，即使在上一步中检索到的 API 密钥中存储了其他用户的凭证也是如此。
        ```
        OK
        Infrastructure credentials for user name <1234567_name@email.com> set for resource group <resource_group_name>.
        ```
        {: screen}

        **凭证未设置为使用其他帐户时的输出示例**。在这种情况下，在上一步中检索到的 API 密钥所有者具有用于区域和资源组的基础架构凭证。
        ```
        FAILED
        No credentials set for resource group <resource_group_name>.: The user credentials could not be found. (E0051)
        ```
        {: screen}
2.  验证用户拥有的基础架构许可权。
    1.  列出区域和资源组的建议和必需的基础架构许可权。
        ```
        ibmcloud ks infra-permissions-get --region <region>
        ```
        {: pre}
    2.  确保 [API 密钥的基础架构凭证所有者或手动设置的帐户具有正确的许可权](/docs/containers?topic=containers-users#owner_permissions)。
    3.  如果需要，可以更改区域和资源组的 [API 密钥](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_api_key_reset)或[手动设置](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_set)的基础架构凭证所有者。
3.  测试更改后的许可权是否允许授权用户对集群执行基础架构操作。
    1.  例如，您可尝试删除工作程序节点。
        ```
        ibmcloud ks worker-rm --cluster <cluster_name_or_ID> --worker <worker_node_ID>
        ```
        {: pre}
    2.  检查以确定是否除去了工作程序节点。
        ```
        ibmcloud ks worker-get --cluster <cluster_name_or_ID> --worker <worker_node_ID>
        ```
        {: pre}

        成功除去工作程序节点时的输出示例。由于删除了工作程序节点，因此 `worker-get` 操作会失败。这说明基础架构许可权已正确设置。
        ```
        FAILED
        The specified worker node could not be found. (E0011)
        ```
        {: screen}

    3.  如果未除去工作程序节点，请查看[**状态**和**阶段状态**字段](/docs/containers?topic=containers-cs_troubleshoot#debug_worker_nodes)以及[工作程序节点的常见问题](/docs/containers?topic=containers-cs_troubleshoot#common_worker_nodes_issues)以继续进行调试。
    4.  如果手动设置了凭证，但在基础架构帐户中仍然看不到集群的工作程序节点，那么可以检查是否[集群为孤立集群](#orphaned)。

<br />


## 防火墙阻止 CLI 命令运行
{: #ts_firewall_clis}

{: tsSymptoms}
通过 CLI 运行 `ibmcloud`、`kubectl` 或 `calicoctl` 命令时，这些命令会失败。

{: tsCauses}
您可能具有企业网络策略，这些策略灰阻止通过代理或防火墙从本地系统访问公共端点。

{: tsResolve}
[允许 TCP 访问以使 CLI 命令能够运作](/docs/containers?topic=containers-firewall#firewall_bx)。此任务需要对集群的 [{{site.data.keyword.Bluemix_notm}} IAM **管理员**平台角色](/docs/containers?topic=containers-users#platform)。


## 无法访问集群中的资源
{: #cs_firewall}

{: tsSymptoms}
集群中的工作程序节点无法在专用网络上进行通信时，您可能会看到各种不同的症状。

- 运行 `kubectl exec`、`attach`、`logs`、`proxy` 或 `port-forward` 时的样本错误消息：
  ```
  Error from server: error dialing backend: dial tcp XXX.XXX.XXX:10250: getsockopt: connection timed out
  ```
  {: screen}

- `kubectl proxy` 成功，但 Kubernetes 仪表板不可用时的样本错误消息：
  ```
  timeout on 172.xxx.xxx.xxx
  ```
  {: screen}

- `kubectl proxy` 失败或连接到服务失败时的样本错误消息：
  ```
  Connection refused
  ```
  {: screen}

  ```
  Connection timed out
  ```
  {: screen}

  ```
  Unable to connect to the server: net/http: TLS handshake timeout
  ```
  {: screen}


{: tsCauses}
要访问集群中的资源，工作程序节点必须能够在专用网络上进行通信。您可能已在 IBM Cloud Infrastructure (SoftLayer) 帐户中设置了 Vyatta 或其他防火墙，或者定制了现有防火墙设置。{{site.data.keyword.containerlong_notm}} 需要打开特定 IP 地址和端口，以允许工作程序节点与 Kubernetes 主节点之间进行通信。如果工作程序节点跨多个专区进行分布，那么必须通过启用 VLAN 生成来允许专用网络通信。如果工作程序节点卡在重新装入循环中，那么工作程序节点之间也可能无法进行通信。

{: tsResolve}
1. 列出集群中的工作程序节点，并验证工作程序节点是否未卡在 `Reloading` 状态。
   ```
  ibmcloud ks workers --cluster <cluster_name_or_id>
  ```
   {: pre}

2. 如果您有多专区集群，但您的帐户未启用 VRF，请验证您的帐户是否已[启用 VLAN 生成](/docs/containers?topic=containers-subnets#subnet-routing)。
3. 如果您具有 Vyatta 或定制防火墙设置，请确保已[打开必需的端口](/docs/containers?topic=containers-firewall#firewall_outbound)以允许集群访问基础架构资源和服务。

<br />



## 无法查看或使用集群
{: #cs_cluster_access}

{: tsSymptoms}
* 找不到某个集群。运行 `ibmcloud ks clusters` 时，输出中未列出该集群。
* 无法使用某个集群。运行 `ibmcloud ks cluster-config` 或其他特定于集群的命令时，找不到该集群。


{: tsCauses}
在 {{site.data.keyword.Bluemix_notm}} 中，每个资源都必须位于资源组中。例如，集群 `mycluster` 可能存在于 `default` 资源组中。帐户所有者通过为您分配 {{site.data.keyword.Bluemix_notm}} IAM 平台角色，从而授予您对资源的访问权时，该访问权可以是针对特定资源或针对资源组。授予您对特定资源的访问权时，您无权访问资源组。在这种情况下，您无需将资源组设定为目标就可以使用您有权访问的集群。如果设定为目标的资源组不是集群所在的资源组，那么针对该集群的操作会失败。反之，授予您对资源的访问权以作为对资源组访问权的一部分时，必须将资源组设定为目标，才能使用该组中的集群。如果未将 CLI 会话的目标设定为集群所在的资源组，那么针对该集群的操作会失败。

如果找不到集群或无法使用集群，那么可能会遇到下列其中一个问题：
* 您有权访问集群和集群所在的资源组，但 CLI 会话的目标未设定为集群所在的资源组。
* 您有权访问集群，但无权访问集群所在的资源组。CLI 会话的目标设定为此资源组或其他资源组。
* 您无权访问集群。

{: tsResolve}
要检查用户访问许可权，请执行以下操作：

1. 列出您的所有用户许可权。
    ```
    ibmcloud iam user-policies <your_user_name>
    ```
    {: pre}

2. 检查您是否有权访问集群以及集群所在的资源组。
    1. 查找具有值为集群资源组的 **Resource Group Name** 以及值为 `Policy applies to the resource group` 的 **Memo** 的策略。如果具有此策略，说明您有权访问资源组。例如，以下策略指示用户有权访问 `test-rg` 资源组：
        ```
        Policy ID:   3ec2c069-fc64-4916-af9e-e6f318e2a16c
        Roles:       Viewer
        Resources:
                     Resource Group ID     50c9b81c983e438b8e42b2e8eca04065
                     Resource Group Name   test-rg
                     Memo                  Policy applies to the resource group
        ```
        {: screen}
    2. 查找具有值为集群资源组的 **Resource Group Name**、值为 `containers-kubernetes` 或没有值的 **Service Name**，以及值为 `Policy applies to the resource(s) within the resource group` 的 **Memo** 的策略。如果具有此策略，说明您有权访问集群或有权访问资源组中的所有资源。例如，以下策略指示用户有权访问 `test-rg` 资源组中的集群：
        ```
        Policy ID:   e0ad889d-56ba-416c-89ae-a03f3cd8eeea
        Roles:       Administrator
        Resources:
                     Resource Group ID     a8a12accd63b437bbd6d58fb6a462ca7
                     Resource Group Name   test-rg
                     Service Name          containers-kubernetes
                     Service Instance
                     Region
                     Resource Type
                     Resource
                     Memo                  Policy applies to the resource(s) within the resource group
        ```
        {: screen}
    3. 如果您同时拥有这两个策略，请跳至步骤 4 的第一个项目符号项。如果您没有步骤 2a 中的策略，但确实有步骤 2b 中的策略，请跳至步骤 4 的第二个项目符号项。如果这两个策略您都没有，请继续执行步骤 3。

3. 检查您是否有权访问集群，但无权访问集群所在的资源组。
    1. 查找除了 **Policy ID** 和 **Roles** 字段外不包含其他任何值的策略。如果具有此策略，那么您有权访问集群，这是对整个帐户的访问权的一部分。例如，以下策略指示用户有权访问帐户中的所有资源：
        ```
        Policy ID:   8898bdfd-d520-49a7-85f8-c0d382c4934e
        Roles:       Administrator, Manager
        Resources:
                     Service Name
                     Service Instance
                     Region
                     Resource Type
                     Resource
        ```
        {: screen}
    2. 查找具有值为 `containers-kubernetes` 的 **Service Name** 以及值为集群标识的 **Service Instance** 的策略。您可以通过运行 `ibmcloud ks cluster-get --cluster <cluster_name>` 来查找集群标识。例如，以下策略指示用户有权访问特定集群：
        ```
        Policy ID:   140555ce-93ac-4fb2-b15d-6ad726795d90
        Roles:       Administrator
        Resources:
                     Service Name       containers-kubernetes
                     Service Instance   df253b6025d64944ab99ed63bb4567b6
                     Region
                     Resource Type
                     Resource
        ```
        {: screen}
    3. 如果您拥有其中任何一个策略，请跳至步骤 4 的第二个项目符号点。如果这两个策略您都没有，请跳至步骤 4 的第三个项目符号点。

4. 根据您的访问策略，选择下列其中一个选项。
    * 检查您是否有权访问集群以及集群所在的资源组：
      1. 将资源组设定为目标。**注**：在取消将此资源组设定为目标之前，无法使用其他资源组中的集群。
          ```
          ibmcloud target -g <resource_group>
          ```
          {: pre}

      2. 将集群设定为目标。
          ```
          ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
          ```
          {: pre}

    * 如果您有权访问集群，但无权访问集群所在的资源组：
      1. 不要将资源组设定为目标。如果已经将某个资源组设定为目标，请取消将其设定为目标：
        ```
        ibmcloud target --unset-resource-group
        ```
        {: pre}

      2. 将集群设定为目标。
         ```
        ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
        ```
        {: pre}

    * 如果您无权访问集群：
        1. 请求您的帐户所有者为您分配对该集群的 [{{site.data.keyword.Bluemix_notm}}IAM 平台角色](/docs/containers?topic=containers-users#platform)。
        2. 不要将资源组设定为目标。如果已经将某个资源组设定为目标，请取消将其设定为目标：
          ```
          ibmcloud target --unset-resource-group
          ```
          {: pre}
        3. 将集群设定为目标。
          ```
          ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
          ```
          {: pre}

<br />


## 使用 SSH 访问工作程序节点失败
{: #cs_ssh_worker}

{: tsSymptoms}
使用 SSH 连接无法访问工作程序节点。

{: tsCauses}
通过密码进行 SSH 在工作程序节点上不可用。

{: tsResolve}
将 Kubernetes [`DaemonSet` ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) 用于必须在每个节点上运行的操作，或者将作业用于必须运行的一次性操作。

<br />


## 裸机实例标识与工作程序记录不一致
{: #bm_machine_id}

{: tsSymptoms}
对裸机工作程序节点使用 `ibmcloud ks worker` 命令时，将看到类似于以下内容的消息。

```
实例标识与工作程序记录不一致
```
{: screen}

{: tsCauses}
机器遇到硬件问题时，机器标识可能会与 {{site.data.keyword.containerlong_notm}} 工作程序记录不一致。IBM Cloud Infrastructure (SoftLayer) 解决此问题时，组件可能会在服务无法识别的系统内更改。

{: tsResolve}
要使 {{site.data.keyword.containerlong_notm}} 重新识别到机器，请[重新装入裸机工作程序节点](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)。**注**：重新装入还会更新机器的[补丁版本](/docs/containers?topic=containers-changelog)。

您还可以[删除裸机工作程序节点](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_rm)。**注**：裸机实例按月计费。

<br />


## 无法修改或删除孤立集群中的基础架构
{: #orphaned}

{: tsSymptoms}
不能对集群执行与基础架构相关的命令，例如：
* 添加或除去工作程序节点
* 重新装入或重新引导工作程序节点
* 调整工作程序池大小
* 更新集群

您无法在 IBM Cloud Infrastructure (SoftLayer) 帐户中查看集群工作程序节点。但是，可以更新和管理帐户中的其他集群。

此外，您已验证自己是否具有[正确的基础架构凭证](#cs_credentials)。

{: tsCauses}
集群可能是在不再链接到您的 {{site.data.keyword.containerlong_notm}} 帐户的 IBM Cloud Infrastructure (SoftLayer) 帐户中供应的。该集群即为孤立集群。由于资源位于其他帐户中，因此您没有用于修改资源的基础架构凭证。

请考虑以下场景来了解集群可能变为孤立集群的情况。
1.  您具有 {{site.data.keyword.Bluemix_notm}} 现收现付帐户。
2.  您创建了名为 `Cluster1` 的集群。工作程序节点和其他基础架构资源供应到现收现付帐户随附的基础架构帐户中。
3.  稍后，您发现自己的团队在使用旧的或共享的 IBM Cloud Infrastructure (SoftLayer) 帐户。为此，您使用 `ibmcloud ks credential-set` 命令更改了 IBM Cloud Infrastructure (SoftLayer) 凭证，以使用团队帐户。
4.  您创建了另一个名为 `Cluster2` 的集群。工作程序节点和其他基础架构资源供应到团队基础架构帐户中。
5.  您注意到 `Cluster1` 需要工作程序节点更新或工作程序节点重新装入，或者您只是想通过删除工作程序节点来将其清除。但是，由于 `Cluster1` 已供应到其他基础架构帐户中，因此无法修改其基础架构资源。`Cluster1` 成为孤立集群。
6.  您遵循了以下部分中的解决步骤，但未将基础架构凭证设置回团队帐户。您可以删除 `Cluster1`，但现在 `Cluster2` 会成为孤立集群。
7.  您将基础架构凭证改回创建 `Cluster2` 的团队帐户。现在，您不再有孤立的集群！

<br>

{: tsResolve}
1.  检查您的集群所在区域当前在使用哪个基础架构帐户来供应集群。
    1.  登录到 [{{site.data.keyword.containerlong_notm}} 集群控制台 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/kubernetes/clusters)。
    2.  从表中选择您的集群。
    3.  在**概述**选项卡中，检查**基础架构用户**字段。此字段可帮助确定您的 {{site.data.keyword.containerlong_notm}} 帐户使用的基础架构帐户是否不同于缺省值。
        * 如果看不到**基础架构用户**字段，那么会有一个链接的现收现付帐户，此帐户将相同的凭证用于基础架构和平台帐户。无法修改的集群可能是在其他基础架帐户中供应的。
        * 如果看到**基础架构用户**字段，说明您使用的基础架构帐户与现收现付帐户随附的基础架构帐户不同。这些不同的凭证应用于区域中的所有集群。无法修改的集群可能是在现收现付或其他基础架帐户中供应的。
2.  检查哪个基础架构帐户已用于供应集群。
    1.  在**工作程序节点**选项卡中，选择工作程序节点，并记下其**标识**。
    2.  打开菜单 ![“菜单”图标](../icons/icon_hamburger.svg "“菜单”图标")，然后单击**经典基础架构**。
    3.  在基础架构导航窗格中，单击**设备 > 设备列表**。
    4.  搜索您先前记下的工作程序节点标识。
    5.  如果找不到工作程序节点标识，说明工作程序节点未供应到此基础架构帐户中。请切换到其他基础架构帐户，然后重试。
3.  使用 `ibmcloud ks credential-set` [命令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_set)将基础架构凭证更改为集群工作程序节点供应到其中的帐户，即您在上一步中找到的帐户。如果您不再有权访问基础架构凭证，因而无法获得这些凭证，那么必须打开一个 {{site.data.keyword.Bluemix_notm}} 支持案例来除去孤立集群。
    {: note}
4.  [删除集群](/docs/containers?topic=containers-remove)。
5.  如果需要，请将基础架构凭证重置为先前的帐户。请注意，如果创建集群使用的基础架构帐户不同于切换到的帐户，那么可能会使这些集群变为孤立集群。
    * 要将凭证设置为其他基础架构帐户，请使用 `ibmcloud ks credential-set` [命令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_set)。
    * 要使用 {{site.data.keyword.Bluemix_notm}} 现收现付帐户随附的缺省凭证，请使用 `ibmcloud ks credential-unset --region <region>` [命令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_unset)。

<br />


## `kubectl` 命令超时
{: #exec_logs_fail}

{: tsSymptoms}
如果运行命令，例如，`kubectl exec`、`kubectl attach`、`kubectl proxy`、`kubectl port-forward` 或 `kubectl logs`，那么会看到以下消息。

  ```
  <workerIP>:10250: getsockopt: connection timed out
  ```
  {: screen}

{: tsCauses}
主节点与工作程序节点之间的 OpenVPN 连接工作不正常。

{: tsResolve}
1. 如果有多个 VLAN 用于一个集群，有多个子网位于同一 VLAN 上或有一个多专区集群，那么必须针对 IBM Cloud Infrastructure (SoftLayer) 帐户启用[虚拟路由器功能 (VRF)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud)，从而使工作程序节点可以在专用网络上相互通信。要启用 VRF，请[联系 IBM Cloud Infrastructure (SoftLayer) 客户代表](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#how-you-can-initiate-the-conversion)。如果无法或不想启用 VRF，请启用 [VLAN 生成](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning)。要执行此操作，您需要有**网络 > 管理网络 VLAN 生成**[基础架构许可权](/docs/containers?topic=containers-users#infra_access)，也可以请求帐户所有者来启用 VLAN 生成。要检查是否已启用 VLAN 生成，请使用 `ibmcloud ks vlan-spanning-get --region <region>` [命令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get)。
2. 重新启动 OpenVPN 客户机 pod。
  ```
  kubectl delete pod -n kube-system -l app=vpn
  ```
  {: pre}
3. 如果仍看到相同的错误消息，说明 VPN pod 所在的工作程序节点可能运行状况欠佳。要重新启动 VPN pod 并将其重新安排到其他工作程序节点，请[对工作程序节点执行 cordon、drain 和 reboot](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reboot)（即，对 VPN pod 所在的工作程序节点执行这些命令）。

<br />


## 将服务绑定到集群导致同名错误
{: #cs_duplicate_services}

{: tsSymptoms}
运行 `ibmcloud ks cluster-service-bind --cluster <cluster_name> --namespace <namespace> --service <service_instance_name>` 时，您会看到以下消息。

```
Multiple services with the same name were found.
请运行“ibmcloud service list”以查看可用的 Bluemix 服务实例...
```
{: screen}

{: tsCauses}
多个服务实例可能在不同区域中具有相同名称。

{: tsResolve}
在 `ibmcloud ks cluster-service-bind` 命令中，请使用服务 GUID，而不要使用服务实例名称。

1. [登录到包含要绑定的服务实例的 {{site.data.keyword.Bluemix_notm}} 区域](/docs/containers?topic=containers-regions-and-zones#bluemix_regions)。

2. 获取服务实例的 GUID。
  ```
  ibmcloud service show <service_instance_name> --guid
  ```
  {: pre}

  输出：
  ```
  Invoking 'cf service <service_instance_name> --guid'...
  <service_instance_GUID>
  ```
  {: screen}
3. 再次将服务绑定到集群。
  ```
  ibmcloud ks cluster-service-bind --cluster <cluster_name> --namespace <namespace> --service <service_instance_GUID>
  ```
  {: pre}

<br />


## 将服务绑定到集群导致找不到服务错误
{: #cs_not_found_services}

{: tsSymptoms}
运行 `ibmcloud ks cluster-service-bind --cluster <cluster_name> --namespace <namespace> --service <service_instance_name>` 时，您会看到以下消息。

```
Binding service to a namespace...
FAILED

The specified IBM Cloud service could not be found. If you just created the service, wait a little and then try to bind it again. 要查看可用的 IBM Cloud 服务实例，请运行“ibmcloud service list”。(E0023)
```
{: screen}

{: tsCauses}
要将服务绑定到集群，您必须具有在其中供应服务实例的空间的 Cloud Foundry 开发者用户角色。此外，您必须具有对 {{site.data.keyword.containerlong}} 的 {{site.data.keyword.Bluemix_notm}} IAM 编辑者平台访问权。要访问服务实例，您必须登录到在其中供应该服务实例的空间。

{: tsResolve}

**以用户身份执行以下操作：**

1. 登录到 {{site.data.keyword.Bluemix_notm}}。
   ```
   ibmcloud login
   ```
   {: pre}

2. 将在其中供应服务实例的组织和空间设定为目标。
   ```
   ibmcloud target -o <org> -s <space>
   ```
   {: pre}

3. 通过列出服务实例来验证您是否位于正确的空间中。
   ```
   ibmcloud service list
   ```
   {: pre}

4. 重试绑定服务。如果遇到相同的错误，请联系帐户管理员，并验证您是否具有足够的许可权来绑定服务（请参阅以下帐户管理员步骤）。

**以帐户管理员身份执行以下操作：**

1. 验证遇到此问题的用户是否具有[对 {{site.data.keyword.containerlong}} 的编辑者许可权](/docs/iam?topic=iam-iammanidaccser#edit_existing)。

2. 验证遇到此问题的用户是否具有对在其中供应该服务的[空间的 Cloud Foundry 开发者角色](/docs/iam?topic=iam-mngcf#update_cf_access)。

3. 如果存在正确的许可权，请尝试分配其他许可权，然后重新分配所需的许可权。

4. 稍等几分钟，然后让用户重试绑定服务。

5. 如果这无法解决此问题，说明 {{site.data.keyword.Bluemix_notm}} IAM 许可权不同步，您无法自行解决此问题。请通过打开支持案例来[联系 IBM 支持人员](/docs/get-support?topic=get-support-getting-customer-support)。确保提供集群标识、用户标识和服务实例标识。
   1. 检索集群标识。
      ```
      ibmcloud ks clusters
      ```
      {: pre}

   2. 检索服务实例标识。
      ```
      ibmcloud service show <service_name> --guid
      ```
      {: pre}


<br />


## 将服务绑定到集群将导致服务不支持服务密钥错误
{: #cs_service_keys}

{: tsSymptoms}
运行 `ibmcloud ks cluster-service-bind --cluster <cluster_name> --namespace <namespace> --service <service_instance_name>` 时，您会看到以下消息。

```
This service doesn't support creation of keys
```
{: screen}

{: tsCauses}
{{site.data.keyword.Bluemix_notm}} 中的某些服务（例如，{{site.data.keyword.keymanagementservicelong}}）不支持创建服务凭证，也称为服务密钥。如果不支持服务密钥，那么服务不可绑定到集群。要查找支持创建服务密钥的服务的列表，请参阅[使外部应用程序能够使用 {{site.data.keyword.Bluemix_notm}} 服务](/docs/resources?topic=resources-externalapp#externalapp)。

{: tsResolve}
要集成不支持服务密钥的服务，请检查服务是否提供可用于直接从应用程序访问服务的 API。例如，如果想要使用 {{site.data.keyword.keymanagementservicelong}}，请参阅 [API 引用 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/apidocs/kms?language=curl)。

<br />


## 工作程序节点更新或重新装入后，出现重复的节点和 pod
{: #cs_duplicate_nodes}

{: tsSymptoms}
运行 `kubectl get nodes` 时，看到状态为 **`NotReady`** 的重复工作程序节点。状态为 **`NotReady`** 的工作程序节点具有公共 IP 地址，而状态为 **`Ready`** 的工作程序节点具有专用 IP 地址。

{: tsCauses}
较旧的集群按集群的公共 IP 地址列出工作程序节点。现在，工作程序节点按集群的专用 IP 地址列出。当您重新装入或更新节点时，IP 地址将更改，但对公共 IP 地址的引用将保持不变。

{: tsResolve}
由于存在这些重复项，因此服务不会中断，但您可以从 API 服务器中除去旧的工作程序节点引用。

  ```
  kubectl delete node <node_name1> <node_name2>
  ```
  {: pre}

<br />


## 访问新工作程序节点上的 pod 失败，并返回超时错误
{: #cs_nodes_duplicate_ip}

{: tsSymptoms}
您删除了集群中的工作程序节点，然后添加了工作程序节点。部署 pod 或 Kubernetes 服务时，资源无法访问新创建的工作程序节点，并且连接超时。

{: tsCauses}
如果从集群中删除工作程序节点，然后添加工作程序节点，那么可能会将已删除工作程序节点的专用 IP 地址分配给新工作程序节点。Calico 使用此专用 IP 地址作为标记，并继续尝试访问已删除的节点。

{: tsResolve}
手动更新专用 IP 地址的引用以指向正确的节点。

1.  确认您是否有两个工作程序节点使用相同的**专用 IP** 地址。记下已删除的工作程序的**专用 IP** 和**标识**。

  ```
  ibmcloud ks workers --cluster <cluster_name_or_id>
  ```
  {: pre}

  ```
  ID                                                 Public IP       Private IP       Machine Type   State     Status   Zone   Version
  kube-dal10-cr9b7371a7fcbe46d08e04f046d5e6d8b4-w1   169.xx.xxx.xxx  10.xxx.xx.xxx    b3c.4x16       normal    Ready    dal10      1.13.6
  kube-dal10-cr9b7371a7fcbe46d08e04f046d5e6d8b4-w2   169.xx.xxx.xxx  10.xxx.xx.xxx    b3c.4x16       deleted    -       dal10      1.13.6
  ```
  {: screen}

2.  安装 [Calico CLI](/docs/containers?topic=containers-network_policies#adding_network_policies)。
3.  列出 Calico 中的可用工作程序节点。将 <path_to_file> 替换为 Calico 配置文件的本地路径。

  ```
  calicoctl get nodes --config=filepath/calicoctl.cfg
  ```
  {: pre}

  ```
  NAME
  kube-dal10-cr9b7371a7faaa46d08e04f046d5e6d8b4-w1
  kube-dal10-cr9b7371a7faaa46d08e04f046d5e6d8b4-w2
  ```
  {: screen}

4.  删除 Calico 中的重复工作程序节点。将 NODE_ID 替换为工作程序节点标识。

  ```
  calicoctl delete node NODE_ID --config=<path_to_file>/calicoctl.cfg
  ```
  {: pre}

5.  重新引导未删除的工作程序节点。

  ```
  ibmcloud ks worker-reboot --cluster <cluster_name_or_id> --worker <worker_id>
  ```
  {: pre}


已删除的节点不会再在 Calico 中列出。

<br />




## 因 pod 安全策略导致 pod 部署失败
{: #cs_psp}

{: tsSymptoms}
创建 pod 或运行 `kubectl get events` 来检查 pod 部署后，您会看到类似以下内容的错误消息。

```
无法根据任何 pod 安全策略进行验证
```
{: screen}

{: tsCauses}
[`PodSecurityPolicy` 许可控制器](/docs/containers?topic=containers-psp)会检查尝试创建 pod 的用户或服务帐户（例如部署或 Helm Tiller）的授权。如果没有 pod 安全策略支持用户或服务帐户，那么 `PodSecurityPolicy` 许可控制器会阻止创建 pod。

如果已删除 [{{site.data.keyword.IBM_notm}} 集群管理](/docs/containers?topic=containers-psp#ibm_psp)的其中一个 pod 安全策略资源，那么可能会遇到类似问题。

{: tsResolve}
确保用户或服务帐户通过 pod 安全策略授权。您可能需要[修改现有策略](/docs/containers?topic=containers-psp#customize_psp)。

如果已删除 {{site.data.keyword.IBM_notm}} 集群管理资源，请刷新 Kubernetes 主节点以将其复原。

1.  [登录到您的帐户。如果适用，请将相应的资源组设定为目标。为集群设置上下文。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
2.  刷新 Kubernetes 主节点以将其复原。

    ```
    ibmcloud ks apiserver-refresh
    ```
    {: pre}


<br />




## 集群保持暂挂状态
{: #cs_cluster_pending}

{: tsSymptoms}
部署集群时，集群保持暂挂状态，而不启动。

{: tsCauses}
如果刚刚创建了集群，那么工作程序节点可能仍在配置中。如果您已经等待一段时间，那么可能是 VLAN 无效。

{: tsResolve}

可以尝试下列其中一个解决方案：
  - 通过运行 `ibmcloud ks clusters` 来检查集群的阶段状态。然后检查以确保通过运行 `ibmcloud ks workers --cluster <cluster_name>` 部署了工作程序节点。
  - 检查以确定 VLAN 是否有效。要使 VLAN 有效，必须将 VLAN 与可使用本地磁盘存储来托管工作程序的基础架构相关联。您可以通过运行 `ibmcloud ks vlans --zone <zone>` 来[列出 VLAN](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlans)，如果 VLAN 未显示在列表中，说明 VLAN 无效。请选择其他 VLAN。

<br />


## 集群创建错误，无法从注册表中拉取映像
{: #ts_image_pull_create}

{: tsSymptoms}
创建集群时，收到了类似于以下内容的错误消息。


```
由于无法创建 IAM 访问策略，因此集群无法从 IBM Cloud Container Registry“icr.io”域中拉取映像。请确保您具有对 IBM Cloud Container Registry 的 IAM 管理员平台角色。然后，通过运行“ibmcloud ks cluster-pre-secret-apply”以使用注册表的 IAM 凭证创建映像拉取私钥。
```
{: screen}

{: tsCauses}
在集群创建期间，会为集群创建服务标识，并为该标识分配对 {{site.data.keyword.registrylong_notm}} 的**读取者**服务访问策略。然后，会生成此服务标识的 API 密钥，并将其存储在[映像拉取私钥](/docs/containers?topic=containers-images#cluster_registry_auth)中，以授权集群从 {{site.data.keyword.registrylong_notm}} 拉取映像。

要在集群创建期间为服务标识成功分配**读取者**服务访问策略，您必须具有对 {{site.data.keyword.registrylong_notm}} 的**管理员**平台访问策略。

{: tsResolve}

步骤：
1.  确保帐户所有者为您授予对 {{site.data.keyword.registrylong_notm}} 的**管理员**角色。
    ```
        ibmcloud iam user-policy-create <your_user_email> --service-name container-registry --roles Administrator
        ```
    {: pre}
2.  [使用 `ibmcloud ks cluster-pull-secret-apply` 命令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_pull_secret_apply)以使用相应的注册表凭证重新创建映像拉取私钥。

<br />


## 未能从注册表中拉取映像，发生 `ImagePullBackOff` 或授权错误
{: #ts_image_pull}

{: tsSymptoms}

部署用于从 {{site.data.keyword.registrylong_notm}} 拉取映像的工作负载时，pod 失败，并显示 **`ImagePullBackOff`** 状态。

```
kubectl get pods
```
{: pre}

```
NAME         READY     STATUS             RESTARTS   AGE
<pod_name>   0/1       ImagePullBackOff   0          2m
```
{: screen}

描述 pod 时，您会看到类似于以下内容的认证错误。

```
        kubectl describe pod <pod_name>
        ```
{: pre}

```
Failed to pull image "<region>.icr.io/<namespace>/<image>:<tag>" ... unauthorized: authentication required
Failed to pull image "<region>.icr.io/<namespace>/<image>:<tag>" ... 401 Unauthorized
```
{: screen}

```
Failed to pull image "registry.ng.bluemix.net/<namespace>/<image>:<tag>" ... unauthorized: authentication required
Failed to pull image "registry.ng.bluemix.net/<namespace>/<image>:<tag>" ... 401 Unauthorized
```
{: screen}

{: tsCauses}
集群使用存储在[映像拉取私钥](/docs/containers?topic=containers-images#cluster_registry_auth)中的 API 密钥或令牌来授权集群从 {{site.data.keyword.registrylong_notm}} 拉取映像。缺省情况下，新集群具有使用 API 密钥的映像拉取私钥，以便集群可以从部署到 `default` Kubernetes 名称空间的容器的任何区域 `icr.io` 注册表中拉取映像。如果集群具有使用令牌的映像拉取私钥，那么对 {{site.data.keyword.registrylong_notm}} 的缺省访问权将进一步限制为只能访问使用不推荐的 `<region>.registry.bluemix.net` 域的某些区域注册表。

{: tsResolve}

1.  验证在部署 YAML 文件中使用的映像的名称和标记是否正确。
    ```
    ibmcloud cr images
    ```
    {: pre}
2.  获取失败的 pod 的 pod 配置文件，然后查找 `imagePullSecrets` 部分。
    ```
        kubectl get pod <pod_name> -o yaml
        ```
    {: pre}

输出示例：
        ```
    ...
    imagePullSecrets:
    - name: bluemix-default-secret
    - name: bluemix-default-secret-regional
    - name: bluemix-default-secret-international
    - name: default-us-icr-io
    - name: default-uk-icr-io
    - name: default-de-icr-io
    - name: default-au-icr-io
    - name: default-jp-icr-io
    - name: default-icr-io
    ...
    ```
    {: screen}
3.  如果未列出任何映像拉取私钥，请在名称空间中设置映像拉取私钥。
    1.  [将映像拉取私钥从 `default` Kubernetes 名称空间复制到要部署工作负载的名称空间](/docs/containers?topic=containers-images#copy_imagePullSecret)。
    2.  [将映像拉取私钥添加到此 Kubernetes 名称空间的服务帐户](/docs/containers?topic=containers-images#store_imagePullSecret)，以便名称空间中的所有 pod 都可以使用映像拉取私钥凭证。
4.  如果列出了映像拉取私钥，请确定用于访问容器注册表的凭证类型。
    *   **不推荐**：如果私钥的名称中含有 `bluemix`，请使用注册表令牌向不推荐的 `registry.<region>.bluemix.net` 域名进行认证。请继续[对使用令牌的映像拉取私钥进行故障诊断](#ts_image_pull_token)。
    *   如果私钥的名称中有 `icr`，说明您是使用 API 密钥向 `icr.io` 域名进行认证的。请继续[对使用 API 密钥的映像拉取私钥进行故障诊断](#ts_image_pull_apikey)。
    *   如果您同时具有这两种类型的私钥，说明您使用了这两种认证方法。请继续在部署 YAML 中将 `icr.io` 域名用于容器映像。请继续[对使用 API 密钥的映像拉取私钥进行故障诊断](#ts_image_pull_apikey)。

<br>
<br>

**对使用 API 密钥的映像拉取私钥进行故障诊断**</br>
{: #ts_image_pull_apikey}

如果 pod 配置具有使用 API 密钥的映像拉取私钥，请检查是否正确设置了 API 密钥凭证。
{: shortdesc}

以下步骤假定 API 密钥存储服务标识的凭证。如果将映像拉取私钥设置为使用单个用户的 API 密钥，那么必须验证该用户的 {{site.data.keyword.Bluemix_notm}} IAM 许可权和凭证。
{: note}

1.  通过查看 **Description** 来查找 API 密钥用于映像拉取私钥的服务标识。使用集群创建的服务标识为 `ID for <cluster_name>`，并在 `default` Kubernetes 名称空间中使用。如果已创建其他服务标识（例如，用于访问其他 Kubernetes 名称空间或修改 {{site.data.keyword.Bluemix_notm}} IAM 许可权），您已定制描述。
    ```
    ibmcloud iam service-ids
    ```
    {: pre}

输出示例：
        ```
    UUID                Name               Created At              Last Updated            Description                                                                                                                                                                                         Locked     
    ServiceId-aa11...   <service_ID_name>  2019-02-01T19:01+0000   2019-02-01T19:01+0000   ID for <cluster_name>                                                                                                                                         false   
    ServiceId-bb22...   <service_ID_name>  2019-02-01T19:01+0000   2019-02-01T19:01+0000   Service ID for IBM Cloud Container Registry in Kubernetes cluster <cluster_name> namespace <kube_namespace>                                                                                                                                         false    
    ```
    {: screen}
2.  验证是否为该服务标识至少分配了一个对 [{{site.data.keyword.registryshort_notm}} 的 {{site.data.keyword.Bluemix_notm}} IAM **读取者**服务访问角色策略](/docs/services/Registry?topic=registry-user#create)。 如果服务标识没有**读取者**服务角色，请[编辑 IAM 策略](/docs/iam?topic=iam-serviceidpolicy#access_edit)。如果策略正确，请继续执行下一步以查看凭证是否有效。
    ```
    ibmcloud iam service-policies <service_ID_name>
    ```
    {: pre}

输出示例：
        ```              
    Policy ID:   a111a111-b22b-333c-d4dd-e555555555e5   
    Roles:       Reader   
    Resources:                            
                  Service Name       container-registry      
                  Service Instance         
区域                  
                  Resource Type      namespace      
                  Resource           <registry_namespace>  
    ```
    {: screen}  
3.  检查映像拉取私钥凭证是否有效。
    1.  获取映像拉取私钥配置。如果 pod 不在 `default` 名称空间中，请包含 `-n` 标志。
        ```
        kubectl get secret <image_pull_secret_name> -o yaml [-n <namespace>]
        ```
        {: pre}
    2.  在输出中，复制 `.dockercfg` 字段的 Base64 编码值。
        ```
        apiVersion: v1
        kind: Secret
        data:
          .dockercfg: eyJyZWdp...==
        ...
        ```
        {: screen}
    3.  对 Base64 字符串解码。例如，在 OS X 上，可以运行以下命令。
        ```
        echo -n "<base64_string>" | base64 --decode
        ```
        {: pre}

        输出示例：
        ```
        {"auths":{"<region>.icr.io":{"username":"iamapikey","password":"<password_string>","email":"<name@abc.com>","auth":"<auth_string>"}}}
        ```
        {: screen}
    4.  将映像拉取私钥区域注册表域名与在容器映像中指定的域名进行比较。缺省情况下，新集群具有在 `default` Kubernetes 名称空间中运行的容器的每个区域注册表域名的映像拉取私钥。但是，如果修改了缺省设置，或者使用的是其他 Kubernetes 名称空间，那么您可能没有区域注册表的映像拉取私钥。请[复制映像拉取私钥](/docs/containers?topic=containers-images#copy_imagePullSecret)，即区域注册表域名的映像拉取私钥。
    5.  使用映像拉取私钥中的 `username` 和 `password` 从本地计算机登录到注册表。如果无法登录，那么可能需要修订服务标识。
        ```
        docker login -u iamapikey -p <password_string> <region>.icr.io
        ```
        {: pre}
        1.  为 `default` Kubernetes 名称空间中运行的容器重新创建集群服务标识、{{site.data.keyword.Bluemix_notm}} IAM 策略、API 密钥和映像拉取私钥。
            ```
            ibmcloud ks cluster-pull-secret-apply --cluster <cluster_name_or_ID>
            ```
            {: pre}
        2.  在 `default` Kubernetes 名称空间中重新创建部署。如果仍看到授权错误消息，请使用新映像拉取私钥重复步骤 1-5。如果您仍无法登录，请[在 Slack 上联系 IBM 团队，或者打开一个 {{site.data.keyword.Bluemix_notm}} 支持案例](#clusters_getting_help)。
    6.  如果登录成功，请在本地拉取映像。如果命令失败并返回 `access denied` 错误，说明注册表帐户与集群不在同一 {{site.data.keyword.Bluemix_notm}} 帐户中。请[创建映像拉取私钥以访问其他帐户中的映像](/docs/containers?topic=containers-images#other_registry_accounts)。如果可以将映像拉出到本地计算机，那么说明 API 密钥具有正确的许可权，但集群中的 API 设置不正确。您无法解决此问题。请[在 Slack 上联系 IBM 团队，或者打开一个 {{site.data.keyword.Bluemix_notm}} 支持案例](#clusters_getting_help)。
        ```
        docker pull <region>icr.io/<namespace>/<image>:<tag>
        ```
        {: pre}

<br>
<br>

**不推荐：对使用令牌的映像拉取私钥进行故障诊断**</br>
{: #ts_image_pull_token}

如果 pod 配置具有使用令牌的映像拉取私钥，请检查令牌凭证是否有效。
{: shortdesc}

`registry.bluemix.net` 域名支持使用令牌来授权集群访问 {{site.data.keyword.registrylong_notm}}，但不推荐使用此方法。请改为[使用 API 密钥方法](/docs/containers?topic=containers-images#cluster_registry_auth)来授权集群访问新的 `icr.io` 注册表域名。
{: deprecated}

1.  获取映像拉取私钥配置。如果 pod 不在 `default` 名称空间中，请包含 `-n` 标志。
    ```
    kubectl get secret <image_pull_secret_name> -o yaml [-n <namespace>]
    ```
    {: pre}
2.  在输出中，复制 `.dockercfg` 字段的 Base64 编码值。
    ```
    apiVersion: v1
    kind: Secret
    data:
      .dockercfg: eyJyZWdp...==
    ...
    ```
    {: screen}
3.  对 Base64 字符串解码。例如，在 OS X 上，可以运行以下命令。
    ```
    echo -n "<base64_string>" | base64 --decode
    ```
    {: pre}

输出示例：
        ```
    {"auths":{"registry.<region>.bluemix.net":{"username":"token","password":"<password_string>","email":"<name@abc.com>","auth":"<auth_string>"}}}
    ```
    {: screen}
4.  将注册表域名与在容器映像中指定的域名进行比较。例如，如果映像拉取私钥授权访问 `registry.ng.bluemix.net` 域，但指定的映像存储在 `registry.eu-de.bluemix.net` 中，那么必须针对 `registry.eu-de.bluemix.net` [创建要在映像拉取私钥中使用的令牌](/docs/containers?topic=containers-images#token_other_regions_accounts)。
5.  使用映像拉取私钥中的 `username` 和 `password` 从本地计算机登录到注册表。如果无法登录，说明令牌有您无法解决的问题。请[在 Slack 上联系 IBM 团队，或者打开一个 {{site.data.keyword.Bluemix_notm}} 支持案例](#clusters_getting_help)。
        ```
    docker login -u token -p <password_string> registry.<region>.bluemix.net
    ```
    {: pre}
6.  如果登录成功，请在本地拉取映像。如果命令失败并返回 `access denied` 错误，说明注册表帐户与集群不在同一 {{site.data.keyword.Bluemix_notm}} 帐户中。请[创建映像拉取私钥以访问其他帐户中的映像](/docs/containers?topic=containers-images#token_other_regions_accounts)。如果命令成功，请[在 Slack 上联系 IBM 团队，或者打开一个 {{site.data.keyword.Bluemix_notm}} 支持案例](#clusters_getting_help)。
        ```
    docker pull registry.<region>.bluemix.net/<namespace>/<image>:<tag>
    ```
    {: pre}

<br />


## Pod 保持暂挂状态
{: #cs_pods_pending}

{: tsSymptoms}
运行 `kubectl get pods` 时，可能看到 pod 保持**暂挂**状态。

{: tsCauses}
如果刚刚创建了 Kubernetes 集群，那么工作程序节点可能仍在配置中。

如果这是现有集群：
*  那么可能是集群中没有足够的容量来部署 pod。
*  pod 可能已超出资源请求或限制。

{: tsResolve}
此任务需要对集群的 {{site.data.keyword.Bluemix_notm}} IAM [**管理员**平台角色](/docs/containers?topic=containers-users#platform)以及对所有名称空间的[**管理者**服务角色](/docs/containers?topic=containers-users#platform)。

如果刚创建了 Kubernetes 集群，请运行以下命令并等待工作程序节点初始化。

```
kubectl get nodes
```
{: pre}

如果这是现有集群，请检查集群容量。

1.  使用缺省端口号设置代理。

  ```
  kubectl proxy
  ```
   {: pre}

2.  打开 Kubernetes 仪表板。

  ```
  http://localhost:8001/ui
  ```
  {: pre}

3.  检查集群中是否有足够的容量来部署 pod。

4.  如果集群中没有足够的容量，请调整工作程序池的大小以添加更多节点。

    1.  查看工作程序池的当前大小和机器类型，以决定要调整哪个工作程序池的大小。

        ```
        ibmcloud ks worker-pools
        ```
        {: pre}

    2.  调整工作程序池的大小，以将更多节点添加到池所跨的每个专区。

        ```
        ibmcloud ks worker-pool-resize --worker-pool <worker_pool> --cluster <cluster_name_or_ID> --size-per-zone <workers_per_zone>
        ```
        {: pre}

5.  可选：检查 pod 资源请求。

    1.  确认 `resources.requests` 值不大于工作程序节点的容量。例如，如果 pod 请求 `cpu: 4000m` 或 4 个核心，但是工作程序节点大小为仅 2 个核心，那么无法部署 pod。

        ```
        kubectl get pod <pod_name> -o yaml
        ```
        {: pre}

    2.  如果请求超过可用容量，那么使用可满足请求的工作程序节点[添加新的工作程序池](/docs/containers?topic=containers-add_workers#add_pool)。

6.  如果在完全部署工作程序节点后，pod 仍然保持 **pending** 状态，请查看 [Kubernetes 文档 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/tasks/debug-application-cluster/debug-pod-replication-controller/#my-pod-stays-pending) 以进一步对 pod 的暂挂状态进行故障诊断。

<br />


## 容器不启动
{: #containers_do_not_start}

{: tsSymptoms}
pod 会成功部署到集群，但容器不启动。

{: tsCauses}
当达到注册表限额时，容器可能不会启动。

{: tsResolve}
[在 {{site.data.keyword.registryshort_notm}} 中释放存储器。](/docs/services/Registry?topic=registry-registry_quota#registry_quota_freeup)

<br />


## pod 重新启动一再失败或 pod 被意外除去
{: #pods_fail}

{: tsSymptoms}
pod 运行状况良好，但被意外除去或卡在重新启动循环中。

{: tsCauses}
容器可能超过其资源限制，或者 pod 可能被更高优先级的 pod 所替换。

{: tsResolve}
要查看是否由于资源限制而正在终止某个容器，请执行以下操作：
<ol><li>获取 pod 的名称。如果使用了标签，那么可以包含该标签来过滤结果。<pre class="pre"><code>kubectl get pods --selector='app=wasliberty'</code></pre></li>
<li>描述 pod，并查找**重新启动计数**。<pre class="pre"><code>kubectl describe pod</code></pre></li>
<li>如果 pod 在短时间内多次重新启动，请访存其状态。<pre class="pre"><code>kubectl get pod -o go-template={{range.status.containerStatuses}}{{"Container Name: "}}{{.name}}{{"\r\nLastState: "}}{{.lastState}}{{end}}</code></pre></li>
<li>查看原因。例如，`OOM Killed` 意味着“内存不足”，指示由于资源限制，容器即将崩溃。</li>
<li>向集群添加容量，以便能够满足资源要求。</li></ol>

<br>

要查看 pod 是否被更高优先级的 pod 所替换，请执行以下操作：
1.  获取 pod 的名称。

    ```
    kubectl get pods
    ```
    {: pre}

2.  描述 pod YAML。

    ```
        kubectl get pod <pod_name> -o yaml
        ```
    {: pre}

3.  检查 `priorityClassName` 字段。

    1.  如果没有 `priorityClassName` 字段值，说明您的 pod 具有 `globalDefault` 优先级类。如果集群管理员未设置 `globalDefault` 优先级类，那么缺省值为零 (0) 或最低优先级。具有更高优先级类的任何 pod 都可以抢占或除去您的 pod。

    2.  如果有 `priorityClassName` 字段值，请获取优先级类。

        ```
  kubectl get priorityclass <priority_class_name> -o yaml
  ```
        {: pre}

    3.  记下 `value` 字段以检查 pod 的优先级。

4.  列出集群中的现有优先级类。

    ```
    kubectl get priorityclasses
    ```
    {: pre}

5.  对于每个优先级类，获取 YAML 文件，并记下 `value` 字段。

    ```
  kubectl get priorityclass <priority_class_name> -o yaml
  ```
    {: pre}

6.  将您的 pod 的优先级类值与其他优先级类值进行比较，以查看其优先级是更高还是更低。

7.  对集群中的其他 pod 重复步骤 1 到 3，以检查它们使用的优先级类。如果其他 pod 的优先级类高于您的 pod，那么除非有足够的资源供您的 pod 和具有更高优先级的每个 pod 使用，否则不会供应您的 pod。

8.  请与集群管理员联系，以向集群添加更多容量，并确认分配了正确的优先级类。

<br />


## 无法使用已更新的配置值安装 Helm chart
{: #cs_helm_install}

{: tsSymptoms}
尝试通过运行 `helm install -f config.yaml --namespace=kubbe-system --name=<release_name> ibm/<chart_name>` 来安装更新的 Helm chart 时，收到 `Error: failed to download "ibm/<chart_name>"` 错误消息。

{: tsCauses}
Helm 实例中 {{site.data.keyword.Bluemix_notm}} 存储库的 URL 可能不正确。

{: tsResolve}
要对 Helm chart 进行故障诊断，请执行以下操作：

1. 列出 Helm 实例中当前可用的存储库。

    ```
    helm repo list
    ```
    {: pre}

2. 在输出中，验证 {{site.data.keyword.Bluemix_notm}} 存储库 `ibm` 的 URL 是否为 `https://icr.io/helm/iks-charts`。

    ```
    NAME    URL
    stable  https://kubernetes-charts.storage.googleapis.com
    local   http://127.0.0.1:8888/charts
    ibm     https://icr.io/helm/iks-charts
    ```
    {: screen}

    * 如果该 URL 不正确，请执行以下操作：

        1. 除去 {{site.data.keyword.Bluemix_notm}} 存储库。

            ```
            helm repo remove ibm
            ```
            {: pre}

        2. 重新添加 {{site.data.keyword.Bluemix_notm}} 存储库。

            ```
   helm repo add iks-charts https://icr.io/helm/iks-charts
   ```
            {: pre}

    * 如果该 URL 正确，请从相应存储库中获取最新更新。

        ```
        helm repo update
        ```
        {: pre}

3. 使用更新安装 Helm chart。

    ```
    helm install -f config.yaml --namespace=kube-system --name=<release_name> ibm/<chart_name>
    ```
    {: pre}

<br />


## 无法安装 Helm Tiller 或通过集群中的公共映像部署容器
{: #cs_tiller_install}

{: tsSymptoms}

尝试安装 Helm Tiller 或要通过公共注册表（如 Dockerhub）部署映像时，安装失败，并显示类似于以下内容的错误：

```
Failed to pull image "gcr.io/kubernetes-helm/tiller:v2.12.0": rpc error: code = Unknown desc = failed to resolve image "gcr.io/kubernetes-helm/tiller:v2.12.0": no available registry endpoint:
```
{: screen}

{: tsCauses}
您可能设置了定制防火墙，指定了定制 Calico 策略，或者使用专用服务端点创建了仅专用集群，因而阻止了与存储映像的容器注册表的公用网络连接。

{: tsResolve}
- 如果您有定制防火墙或设置了定制 Calico 策略，请允许工作程序节点与存储映像的容器注册表之间的出站和入站网络流量。如果映像存储在 {{site.data.keyword.registryshort_notm}} 中，请查看[允许集群访问基础架构资源和其他服务](/docs/containers?topic=containers-firewall#firewall_outbound)中的必需端口。
- 如果是通过仅启用专用服务端点创建的专用集群，那么可以为该集群[启用公共服务端点](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_disable)。如果要在不打开公共连接的情况下在专用集群中安装 Helm chart，那么可以[使用 Tiller](/docs/containers?topic=containers-helm#private_local_tiller) 或[不使用 Tiller](/docs/containers?topic=containers-helm#private_install_without_tiller) 来安装 Helm。

<br />


## 获取帮助和支持
{: #clusters_getting_help}

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

