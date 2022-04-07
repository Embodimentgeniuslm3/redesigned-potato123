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
{:tip: .tip}rwo
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:preview: .preview}



# 在 IBM Block Storage for IBM Cloud 上存储数据
{: #block_storage}

{{site.data.keyword.Bluemix_notm}} Block Storage 是一种持久的高性能 iSCSI 存储器，可以使用 Kubernetes 持久卷 (PV) 添加到应用程序。您可以在具有不同 GB 大小和 IOPS 的预定义存储层之间进行选择，以满足工作负载的需求。要了解 {{site.data.keyword.Bluemix_notm}} Block Storage 是否为适合您的存储选项，请参阅[选择存储解决方案](/docs/containers?topic=containers-storage_planning#choose_storage_solution)。有关定价信息，请参阅[计费](/docs/infrastructure/BlockStorage?topic=BlockStorage-About#billing)。
{: shortdesc}

{{site.data.keyword.Bluemix_notm}} Block Storage 仅可用于标准集群。如果集群无法访问公用网络（例如，防火墙后面的专用集群或仅启用了专用服务端点的集群），请确保已安装 {{site.data.keyword.Bluemix_notm}} Block Storage 插件 V1.3.0 或更高版本，以通过专用网络连接到块存储器实例。块存储器实例特定于单个专区。如果您有多专区集群，请考虑[多专区持久性存储选项](/docs/containers?topic=containers-storage_planning#persistent_storage_overview)。
{: important}

## 在集群中安装 {{site.data.keyword.Bluemix_notm}} Block Storage 插件
{: #install_block}

安装 {{site.data.keyword.Bluemix_notm}} Block Storage 插件和 Helm chart，以便为块存储器设置预定义的存储类。可以使用这些存储类来创建用于为应用程序供应块存储器的 PVC。
{: shortdesc}

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

2.  [按照指示信息](/docs/containers?topic=containers-helm#public_helm_install)在本地计算机上安装 Helm 客户机，然后使用服务帐户在集群中安装 Helm 服务器 (Tiller)。

    安装 Helm 服务器 Tiller 需要与公共 Google Container Registry 的公用网络连接。如果集群无法访问公用网络（例如，防火墙后面的专用集群或仅启用了专用服务端点的集群），那么可以选择[将 Tiller 映像拉出到本地计算机，并将映像推送到 {{site.data.keyword.registryshort_notm}} 中的名称空间](/docs/containers?topic=containers-helm#private_local_tiller)，或者[安装 Helm chart（不使用 Tiller）](/docs/containers?topic=containers-helm#private_install_without_tiller)。
    {: note}

3.  验证是否已使用服务帐户安装 Tiller。

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

4. 在要使用 {{site.data.keyword.Bluemix_notm}} Block Storage 插件的集群中添加 {{site.data.keyword.Bluemix_notm}} Helm chart 存储库。
   ```
   helm repo add iks-charts https://icr.io/helm/iks-charts
   ```
   {: pre}

5. 更新 Helm 存储库以在此存储库中检索所有最新版本的 Helm chart。
   ```
   helm repo update
   ```
   {: pre}

6. 安装 {{site.data.keyword.Bluemix_notm}} Block Storage 插件。安装该插件时，会将预定义的块存储类添加到集群中。
   ```
   helm install iks-charts/ibmcloud-block-storage-plugin 
   ```
   {: pre}

   输出示例：
   ```
   NAME:   bald-olm
   LAST DEPLOYED: Wed Apr 18 10:02:55 2018
   NAMESPACE: default
   STATUS: DEPLOYED

   RESOURCES:
   ==> v1beta1/DaemonSet
   NAME                           DESIRED  CURRENT  READY  UP-TO-DATE  AVAILABLE  NODE SELECTOR  AGE
   ibmcloud-block-storage-driver  0        0        0      0           0          <none>         0s

   ==> v1beta1/Deployment
   NAME                           DESIRED  CURRENT  UP-TO-DATE  AVAILABLE  AGE
   ibmcloud-block-storage-plugin  1        0        0           0          0s

   ==> v1/StorageClass
   NAME                      PROVISIONER        AGE
   ibmc-block-bronze         ibm.io/ibmc-block  0s
   ibmc-block-custom         ibm.io/ibmc-block  0s
   ibmc-block-gold           ibm.io/ibmc-block  0s
   ibmc-block-retain-bronze  ibm.io/ibmc-block  0s
   ibmc-block-retain-custom  ibm.io/ibmc-block  0s
   ibmc-block-retain-gold    ibm.io/ibmc-block  0s
   ibmc-block-retain-silver  ibm.io/ibmc-block  0s
   ibmc-block-silver         ibm.io/ibmc-block  0s

   ==> v1/ServiceAccount
   NAME                           SECRETS  AGE
   ibmcloud-block-storage-plugin  1        0s

   ==> v1beta1/ClusterRole
   NAME                           AGE
   ibmcloud-block-storage-plugin  0s

   ==> v1beta1/ClusterRoleBinding
   NAME                           AGE
   ibmcloud-block-storage-plugin  0s

   NOTES:
   Thank you for installing: ibmcloud-block-storage-plugin.   Your release is named: bald-olm
   ```
   {: screen}

7. 验证安装是否已成功。
   ```
   kubectl get pod -n kube-system | grep block
   ```
   {: pre}

   输出示例：
   ```
   ibmcloud-block-storage-driver-kh4mt                              1/1       Running   0          27d       10.118.98.19   10.118.98.19
   ibmcloud-block-storage-plugin-58c5f9dc86-pbl4t                   1/1       Running   0          14d       172.21.0.204   10.118.98.19
   ```
   {: screen}

   看到一个 `ibmcloud-block-storage-plugin` pod 以及一个或多个 `ibmcloud-block-storage-driver` pod 时，说明安装成功。`ibmcloud-block-storage-driver` pod 的数量等于集群中的工作程序节点数。所有 pod 都必须处于 **Running** 状态。

8. 验证块存储器的存储类是否已添加到集群。
   ```
   kubectl get storageclasses | grep block
   ```
   {: pre}

   输出示例：
   ```
   ibmc-block-bronze            ibm.io/ibmc-block
   ibmc-block-custom            ibm.io/ibmc-block
   ibmc-block-gold              ibm.io/ibmc-block
   ibmc-block-retain-bronze     ibm.io/ibmc-block
   ibmc-block-retain-custom     ibm.io/ibmc-block
   ibmc-block-retain-gold       ibm.io/ibmc-block
   ibmc-block-retain-silver     ibm.io/ibmc-block
   ibmc-block-silver            ibm.io/ibmc-block
   ```
   {: screen}

9. 对要供应块存储器的每个集群，重复这些步骤。

现在可以继续[创建 PVC](#add_block)，以用于为应用程序供应块存储器。


### 更新 {{site.data.keyword.Bluemix_notm}} Block Storage 插件
现在，可以将现有 {{site.data.keyword.Bluemix_notm}} Block Storage 插件升级到最新版本。
{: shortdesc}

开始之前：[登录到您的帐户。如果适用，请将相应的资源组设定为目标。为集群设置上下文。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1. 更新 Helm 存储库以在此存储库中检索所有最新版本的 Helm chart。
   ```
   helm repo update
   ```
   {: pre}

2. 可选：将最新 Helm chart 下载到本地计算机。然后，解压缩包并查看 `release.md` 文件，以了解最新的发行版信息。
   ```
   helm fetch iks-charts/ibmcloud-block-storage-plugin
   ```
   {: pre}

3. 找到已安装在集群中的块存储器 Helm chart 的名称。
   ```
   helm ls | grep ibmcloud-block-storage-plugin
   ```
   {: pre}

   输出示例：
   ```
   myhelmchart 	1       	Mon Sep 18 15:31:40 2017	DEPLOYED	ibmcloud-block-storage-plugin-0.1.0	default
   ```
   {: screen}

4. 将 {{site.data.keyword.Bluemix_notm}} Block Storage 插件升级到最新版本。
   ```
   helm upgrade --force --recreate-pods <helm_chart_name>  iks-charts/ibmcloud-block-storage-plugin
   ```
   {: pre}

5. 可选：更新插件时，会取消设置`缺省`存储类。如果要将缺省存储类设置为您选择的存储类，请运行以下命令。
   ```
   kubectl patch storageclass <storageclass> -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'
   ```
   {: pre}


### 除去 {{site.data.keyword.Bluemix_notm}} Block Storage 插件
如果不想在集群中供应和使用 {{site.data.keyword.Bluemix_notm}} Block Storage，那么可以卸载 Helm chart。
{: shortdesc}

除去该插件不会除去现有 PVC、PV 或数据。除去该插件时，将从集群中除去所有相关的 pod 和守护程序集。除去该插件后，无法为集群供应新的块存储器，也无法使用现有块存储器 PVC 和 PV。
{: important}

开始之前：
- [登录到您的帐户。如果适用，请将相应的资源组设定为目标。为集群设置上下文。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
- 确保集群中没有任何使用块存储器的 PVC 或 PV。

要除去该插件，请执行以下操作：

1. 找到已安装在集群中的块存储器 Helm chart 的名称。
   ```
   helm ls | grep ibmcloud-block-storage-plugin
   ```
   {: pre}

   输出示例：
   ```
   myhelmchart 	1       	Mon Sep 18 15:31:40 2017	DEPLOYED	ibmcloud-block-storage-plugin-0.1.0	default
   ```
   {: screen}

2. 删除 {{site.data.keyword.Bluemix_notm}} Block Storage 插件。
   ```
   helm delete <helm_chart_name>
   ```
   {: pre}

3. 验证块存储器 pod 是否已除去。
   ```
   kubectl get pod -n kube-system | grep ibmcloud-block-storage-plugin
   ```
   {: pre}
   如果 CLI 输出中未显示任何 pod，那么表明已成功除去 pod。

4. 验证块存储器的存储类是否已除去。
   ```
   kubectl get storageclasses | grep block
   ```
   {: pre}
   如果 CLI 输出中未显示任何存储类，说明存储类除去操作成功。

<br />



## 决定块存储器配置
{: #block_predefined_storageclass}

{{site.data.keyword.containerlong}} 为块存储器提供了预定义的存储类，可以使用这些类来供应具有特定配置的块存储器。
{: shortdesc}

每个存储类指定供应的块存储器的类型，包括可用大小、IOPS、文件系统和保留策略。  

确保仔细选择存储配置，以便有足够的容量来存储数据。使用存储类来供应特定类型的存储器后，即无法更改存储设备的大小、类型、IOPS 或保留策略。如果需要更多存储器或需要具有不同配置的存储器，那么必须[创建新的存储器实例并复制数据](/docs/containers?topic=containers-kube_concepts#update_storageclass)，即把旧存储器实例中的数据复制到新存储器实例。
{: important}

1. 列出 {{site.data.keyword.containerlong}} 中的可用存储类。
    ```
   kubectl get storageclasses | grep block
   ```
    {: pre}

    输出示例：
    ```
    $ kubectl get storageclasses
    NAME                         TYPE
    ibmc-block-custom            ibm.io/ibmc-block
    ibmc-block-bronze            ibm.io/ibmc-block
    ibmc-block-gold              ibm.io/ibmc-block
    ibmc-block-silver            ibm.io/ibmc-block
    ibmc-block-retain-bronze     ibm.io/ibmc-block
    ibmc-block-retain-silver     ibm.io/ibmc-block
    ibmc-block-retain-gold       ibm.io/ibmc-block
    ibmc-block-retain-custom     ibm.io/ibmc-block
    ```
    {: screen}

2. 查看存储类的配置。
   ```
   kubectl describe storageclass <storageclass_name>
   ```
   {: pre}

   有关每个存储类的更多信息，请参阅[存储类参考](#block_storageclass_reference)。如果找不到要查找的内容，请考虑创建您自己的定制存储类。首先，请查看[定制存储类样本](#block_custom_storageclass)。
   {: tip}

3. 选择要供应的块存储器的类型。
   - **铜牌级、银牌级和金牌级存储类：**这些存储类供应[耐久性存储器](/docs/infrastructure/BlockStorage?topic=BlockStorage-About#provendurance)。通过耐久性存储器，可以选择预定义 IOPS 层的存储器大小（以千兆字节为单位）。
   - **定制存储类：**此存储类供应[性能存储器](/docs/infrastructure/BlockStorage?topic=BlockStorage-About#provperformance)。通过性能存储器，可以控制存储器和 IOPS 的大小。

4. 选择块存储器的大小和 IOPS。IOPS 的大小和数量定义了 IOPS（每秒输入/输出操作数）的总数，这用于指示存储器的速度。存储器的总 IOPS 越高，处理读写操作的速度越快。
   - **铜牌级、银牌级和金牌级存储类：**这些存储类随附固定数量的 IOPS/千兆字节，并在 SSD 硬盘上供应。IOPS 的总数取决于您选择的存储器大小。您可以在允许的大小范围内选择任意整数的千兆字节（例如，20 Gi、256 Gi、11854 Gi）。要确定 IOPS 的总数，必须将 IOPS 乘以所选大小。例如，如果在随附 4 IOPS/GB 的银牌级存储类中选择 1000 Gi 块存储器大小，那么存储器总计有 4000 IOPS。  
     <table>
         <caption>存储类大小范围和 IOPS/千兆字节表</caption>
         <thead>
         <th>存储类</th>
         <th>IOPS/千兆字节</th>
         <th>大小范围（千兆字节）</th>
         </thead>
         <tbody>
         <tr>
         <td>铜牌级</td>
         <td>2 IOPS/GB</td>
         <td>20-12000 Gi</td>
         </tr>
         <tr>
         <td>银牌级</td>
         <td>4 IOPS/GB</td>
         <td>20-12000 Gi</td>
         </tr>
         <tr>
         <td>金牌级</td>
         <td>10 IOPS/GB</td>
         <td>20-4000 Gi</td>
         </tr>
         </tbody></table>
   - **定制存储类：**选择此存储类时，可以对所需大小和 IOPS 具有更多控制权。对于大小，可以在允许的大小范围内选择任意整数的千兆字节。选择的大小将确定可供您使用的 IOPS 范围。您可以选择指定范围内是 100 的倍数的 IOPS。您选择的 IOPS 是静态的，不会随存储器大小进行缩放。例如，如果选择了 40 Gi 和 100 IOPS，那么总 IOPS 仍为 100。</br></br>IOPS 与千兆字节的比率还决定了为您供应的硬盘类型。例如，如果对于 100 IOPS 选择了 500 Gi，那么 IOPS 与千兆字节的比率为 0.2。比率小于或等于 0.3 的存储器在 SATA 硬盘上供应。如果比率大于 0.3，那么会在 SSD 硬盘上供应存储器。
     <table>
         <caption>定制存储类大小范围和 IOPS 表</caption>
         <thead>
         <th>大小范围（千兆字节）</th>
         <th>IOPS 范围（100 的倍数）</th>
         </thead>
         <tbody>
         <tr>
         <td>20-39 Gi</td>
         <td>100-1000 IOPS</td>
         </tr>
         <tr>
         <td>40-79 Gi</td>
         <td>100-2000 IOPS</td>
         </tr>
         <tr>
         <td>80-99 Gi</td>
         <td>100-4000 IOPS</td>
         </tr>
         <tr>
         <td>100-499 Gi</td>
         <td>100-6000 IOPS</td>
         </tr>
         <tr>
         <td>500-999 Gi</td>
         <td>100-10000 IOPS</td>
         </tr>
         <tr>
         <td>1000-1999 Gi</td>
         <td>100-20000 IOPS</td>
         </tr>
         <tr>
         <td>2000-2999 Gi</td>
         <td>200-40000 IOPS</td>
         </tr>
         <tr>
         <td>3000-3999 Gi</td>
         <td>200-48000 IOPS</td>
         </tr>
         <tr>
         <td>4000-7999 Gi</td>
         <td>300-48000 IOPS</td>
         </tr>
         <tr>
         <td>8000-9999 Gi</td>
         <td>500-48000 IOPS</td>
         </tr>
         <tr>
         <td>10000-12000 Gi</td>
         <td>1000-48000 IOPS</td>
         </tr>
         </tbody></table>

5. 选择在删除集群或持久卷声明 (PVC) 后是否要保留数据。
   - 如果要保留数据，请选择 `retain` 存储类。删除 PVC 时，仅会删除 PVC。PV、IBM Cloud Infrastructure (SoftLayer) 帐户中的物理存储设备以及数据仍会存在。要回收存储器并再次在集群中使用，必须除去 PV，并执行[使用现有块存储器](#existing_block)的步骤。
   - 如果要在删除 PVC 时删除 PV、数据和物理块存储设备，请选择不带 `retain` 的存储类。

6. 选择是要按小时还是按月计费。有关更多信息，请查看[定价 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/cloud/block-storage/pricing)。缺省情况下，所有块存储设备都以按小时计费类型进行供应。

<br />



## 向应用程序添加块存储器
{: #add_block}

创建持久卷声明 (PVC) 以便为集群[动态供应](/docs/containers?topic=containers-kube_concepts#dynamic_provisioning)块存储器。动态供应将自动创建匹配的持久卷 (PV)，并在 IBM Cloud Infrastructure (SoftLayer) 帐户中订购实际存储设备。
{:shortdesc}

块存储器随附 `ReadWriteOnce` 访问方式。一次只能将其安装到集群中一个工作程序节点上的一个 pod。
{: note}

开始之前：
- 如果您有防火墙，请针对集群所在专区的 IBM Cloud Infrastructure (SoftLayer) IP 范围[允许流出访问](/docs/containers?topic=containers-firewall#pvc)，以便您可以创建 PVC。
- 安装 [{{site.data.keyword.Bluemix_notm}} Block Storage 插件](#install_block)。
- [决定预定义的存储类](#block_predefined_storageclass)或创建[定制存储类](#block_custom_storageclass)。

要在有状态集内部署块存储器？有关更多信息，请参阅[在有状态集内使用块存储器](#block_statefulset)。
{: tip}

要添加块存储器，请执行以下操作：

1.  创建配置文件以定义持久卷声明 (PVC)，并将配置保存为 `.yaml` 文件。

    -  **铜牌级、银牌级或金牌级存储类的示例**：
       以下 `.yaml` 文件创建名为 `mypvc` 的声明，其中存储类为 `"ibmc-block-silver"`，计费类型为 `"hourly"`，千兆字节大小为 `24Gi`。

       ```
       apiVersion: v1
       kind: PersistentVolumeClaim
       metadata:
         name: mypvc
         labels:
           billingType: "hourly"
	       region: us-south
           zone: dal13
       spec:
         accessModes:
           - ReadWriteOnce
         resources:
           requests:
             storage: 24Gi
	     storageClassName: ibmc-block-silver
       ```
       {: codeblock}

    -  **使用定制存储类的示例**：
       以下 `.yaml` 文件创建名为 `mypvc` 的声明，其中存储类为 `ibmc-block-retain-custom`，计费类型为 `"hourly"`，千兆字节大小为 `45Gi`，IOPS 为 `"300"`。

       ```
       apiVersion: v1
       kind: PersistentVolumeClaim
       metadata:
         name: mypvc
         labels:
           billingType: "hourly"
	       region: us-south
           zone: dal13
       spec:
         accessModes:
           - ReadWriteOnce
         resources:
           requests:
             storage: 45Gi
             iops: "300"
	     storageClassName: ibmc-block-retain-custom
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
         <td><code>metadata.labels.billingType</code></td>
         <td>指定用于计算存储器帐单的频率："monthly" 或 "hourly"。缺省值为 "hourly"。</td>
       </tr>
       <tr>
       <td><code>metadata.labels.region</code></td>
       <td>指定要在其中供应块存储器的区域。如果指定了区域，那么还必须指定专区。如果未指定区域，或者找不到指定的区域，那么会在集群所在的区域中创建存储器。<p class="note">仅 IBM Cloud Block Storage V1.0.1 或更高版本的插件支持此选项。对于更旧的插件版本，如果您有多专区集群，那么将循环选择供应存储器的专区，以在所有专区中均匀均衡卷请求。要为存储器指定专区，可以首先创建[定制存储类](#block_multizone_yaml)。然后，使用定制存储类创建 PVC。</p></td>
       </tr>
       <tr>
       <td><code>metadata.labels.zone</code></td>
	<td>指定要在其中供应块存储器的专区。如果指定了专区，那么还必须指定区域。如果未指定专区或在多专区集群中找不到指定的专区，那么将循环选择专区。<p class="note">仅 IBM Cloud Block Storage V1.0.1 或更高版本的插件支持此选项。对于更旧的插件版本，如果您有多专区集群，那么将循环选择供应存储器的专区，以在所有专区中均匀均衡卷请求。要为存储器指定专区，可以首先创建[定制存储类](#block_multizone_yaml)。然后，使用定制存储类创建 PVC。</p></td>
	</tr>
        <tr>
        <td><code>spec.resources.requests.storage</code></td>
        <td>输入块存储器的大小，以千兆字节 (Gi) 为单位。供应存储器后，即不能更改块存储器的大小。因此，请确保指定与要存储的数据量相匹配的大小。</td>
        </tr>
        <tr>
        <td><code>spec.resources.requests.iops</code></td>
        <td>此选项仅适用于定制存储类 (`ibmc-block-custom / ibmc-block-retain-custom`)。请指定存储器的总 IOPS，在允许范围内选择 100 的倍数。如果选择的 IOPS 不同于列出的 IOPS，那么该 IOPS 会向上舍入。</td>
        </tr>
	<tr>
	<td><code>spec.storageClassName</code></td>
	<td>要用于供应块存储器的存储类的名称。您可以选择使用其中一个 [IBM 提供的存储类](#block_storageclass_reference)或[创建您自己的存储类](#block_custom_storageclass)。</br> 如果未指定存储类，那么会使用缺省存储类 <code>ibmc-file-bronze</code> 来创建 PV。<p>**提示：**如果要更改缺省存储类，请运行 <code>kubectl patch storageclass &lt;storageclass&gt; -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'</code>，并将 <code>&lt;storageclass&gt;</code> 替换为存储类的名称。</p></td>
	</tr>
        </tbody></table>

    如果要使用定制存储类，请使用相应的存储类名称、有效的 IOPS 和大小来创建 PVC。   
    {: tip}

2.  创建 PVC。

    ```
     kubectl apply -f mypvc.yaml
     ```
    {: pre}

3.  验证 PVC 是否已创建并与 PV 绑定。此过程可能需要几分钟时间。

    ```
    kubectl describe pvc mypvc
    ```
    {: pre}

    输出示例：

    ```
    Name:		mypvc
    Namespace:	default
    StorageClass:	""
    Status:		Bound
    Volume:		pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2
    Labels:		<none>
    Capacity:	20Gi
    Access Modes:	RWO
    Events:
      FirstSeen	LastSeen	Count	From								SubObjectPath	Type		Reason			Message
      ---------	--------	-----	----								-------------	--------	------			-------
      3m		3m		1	{ibm.io/ibmc-block 31898035-3011-11e7-a6a4-7a08779efd33 }			Normal		Provisioning		External provisioner is provisioning volume for claim "default/my-persistent-volume-claim"
      3m		1m		10	{persistentvolume-controller }							Normal		ExternalProvisioning	cannot find provisioner "ibm.io/ibmc-block", expecting that a volume for the claim is provisioned either manually or via external software
      1m		1m		1	{ibm.io/ibmc-block 31898035-3011-11e7-a6a4-7a08779efd33 }			Normal		ProvisioningSucceeded	Successfully provisioned volume pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2

    ```
    {: screen}

4.  {: #block_app_volume_mount}要将 PV 安装到部署，请创建配置 `.yaml` 文件，并指定绑定该 PV 的 PVC。

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
    <td><code>spec.containers.volumeMounts.mountPath</code></td>
    <td>在容器中安装卷的目录的绝对路径。写入安装路径的数据会存储在物理块存储器实例中的根目录下。如果要在不同应用程序之间共享卷，可以为每个应用程序指定[卷子路径 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/storage/volumes/#using-subpath)。</td>
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

<br />




## 在集群中使用现有块存储器
{: #existing_block}

如果您具有要在集群中使用的现有物理存储设备，那么可以手动创建 PV 和 PVC，以[静态供应](/docs/containers?topic=containers-kube_concepts#static_provisioning)存储器。
{: shortdesc}

在开始将现有存储器安装到应用程序之前，必须检索 PV 的所有必要信息。  

### 步骤 1：检索现有块存储器的信息
{: #existing-block-1}

1.  检索或生成 IBM Cloud Infrastructure (SoftLayer) 帐户的 API 密钥。
    1. 登录到 [IBM Cloud Infrastructure (SoftLayer) 门户网站 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/classic?)。
    2. 选择**帐户**，再选择**用户**，然后选择**用户列表**。
    3. 找到您的用户标识。
    4. 在 **API 密钥**列中，单击**生成**以生成 API 密钥，或者单击**查看**以查看现有 API 密钥。
2.  检索 IBM Cloud Infrastructure (SoftLayer) 帐户的 API 用户名。
    1. 从**用户列表**菜单中，选择您的用户标识。
    2. 在 **API 访问信息**部分中，找到您的 **API 用户名**。
3.  登录到 IBM Cloud Infrastructure CLI 插件。
    ```
    ibmcloud sl init
    ```
    {: pre}

4.  选择使用 IBM Cloud Infrastructure (SoftLayer) 帐户的用户名和 API 密钥进行认证。
5.  输入在先前步骤中检索到的用户名和 API 密钥。
6.  列出可用的块存储设备。
    ```
   ibmcloud sl block volume-list 
   ```
    {: pre}

    输出示例：
    ```
    id         username            datacenter   storage_type              capacity_gb   bytes_used   ip_addr         lunId   active_transactions
    38642141   IBM02SEL1543159-1   dal10        endurance_block_storage   20            -            169.xx.xxx.xxx   170     0
    ```
    {: screen}

7.  记下要安装到集群的块存储设备的 `id`、`ip_addr`、`capacity_gb`、`datacenter` 和 `lunId`。**注：**要将现有存储器安装到集群，必须在存储器所在的专区中有工作程序节点。要验证工作程序节点的专区，请运行 `ibmcloud ks workers --cluster <cluster_name_or_ID>`。

### 步骤 2：创建持久卷 (PV) 和匹配的持久卷声明 (PVC)
{: #existing-block-2}

1.  可选：如果您具有使用 `retain` 存储类供应的存储器，那么在除去 PVC 时，不会除去 PV 和物理存储设备。要在集群中复用存储器，必须首先除去 PV。
    1. 列出现有 PV。
       ```
       kubectl get pv
       ```
       {: pre}

       查找属于持久性存储器的 PV。该 PV 处于 `released` 状态。

    2. 除去该 PV。
       ```
   kubectl delete pv <pv_name>
   ```
       {: pre}

    3. 验证 PV 是否已除去。
       ```
       kubectl get pv
       ```
       {: pre}

2.  为 PV 创建配置文件。包含先前检索到的块存储器 `id`、`ip_addr`、`capacity_gb`、`datacenter` 和 `lunIdID`。

    ```
    apiVersion: v1
    kind: PersistentVolume
    metadata:
      name: mypv
      labels:
         failure-domain.beta.kubernetes.io/region: <region>
         failure-domain.beta.kubernetes.io/zone: <zone>
    spec:
      capacity:
        storage: "<storage_size>"
      accessModes:
        - ReadWriteOnce
      flexVolume:
        driver: "ibm/ibmc-block"
        fsType: "<fs_type>"
        options:
          "Lun": "<lun_ID>"
          "TargetPortal": "<IP_address>"
          "VolumeID": "<volume_ID>"
          "volumeName": "<volume_name>"
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
    <td>输入要创建的 PV 的名称。</td>
    </tr>
    <tr>
    <td><code>metadata.labels</code></td>
    <td>输入先前检索到的区域和专区。必须在持久性存储器所在的区域和专区中至少有一个工作程序节点，才能在集群中安装该存储器。如果存储器的 PV 已存在，请向 PV [添加专区和区域标签](/docs/containers?topic=containers-kube_concepts#storage_multizone)。
    </tr>
    <tr>
    <td><code>spec.flexVolume.fsType</code></td>
    <td>输入为现有块存储器配置的文件系统类型。选择 <code>ext4</code> 或 <code>xfs</code>。如果未指定此选项，PV 将缺省为 <code>ext4</code>。如果定义了错误的 `fsType`，那么 PV 创建会成功，但将 PV 安装到 pod 会失败。</td></tr>	    
    <tr>
    <td><code>spec.capacity.storage</code></td>
    <td>输入在先前步骤中检索到的现有块存储器的存储器大小作为 <code>capacity-gb</code>。存储器大小必须以千兆字节为单位，例如 20Gi (20 GB) 或 1000Gi (1 TB)。</td>
    </tr>
    <tr>
    <td><code>flexVolume.options.Lun</code></td>
    <td>输入先前检索到的块存储器的 LUN 标识作为 <code>lunId</code>。</td>
    </tr>
    <tr>
    <td><code>flexVolume.options.TargetPortal</code></td>
    <td>输入先前检索到的块存储器的 IP 地址作为 <code>ip_addr</code>。</td>
    </tr>
    <tr>
	    <td><code>flexVolume.options.VolumeId</code></td>
	    <td>输入先前检索到的块存储器的标识作为 <code>id</code>。</td>
	    </tr>
	    <tr>
		    <td><code>flexVolume.options.volumeName</code></td>
		    <td>输入卷的名称。</td>
	    </tr>
    </tbody></table>

3.  在集群中创建 PV。
    ```
    kubectl apply -f mypv.yaml
    ```
    {: pre}

4. 验证 PV 是否已创建。
    ```
    kubectl get pv
    ```
    {: pre}

5. 创建另一个配置文件以创建 PVC。为了使 PVC 与先前创建的 PV 相匹配，必须为 `storage` 和 `accessMode` 选择相同的值。`storage-class` 字段必须为空。如果其中任何字段与 PV 不匹配，那么会改为自动创建新的 PV。

     ```
     kind: PersistentVolumeClaim
     apiVersion: v1
     metadata:
      name: mypvc
     spec:
      accessModes:
        - ReadWriteOnce
      resources:
        requests:
          storage: "<storage_size>"
      storageClassName:
     ```
     {: codeblock}

6.  创建 PVC。
     ```
     kubectl apply -f mypvc.yaml
     ```
     {: pre}

7.  验证 PVC 是否已创建并与先前创建的 PV 绑定。此过程可能需要几分钟时间。
     ```
    kubectl describe pvc mypvc
    ```
     {: pre}

     输出示例：

     ```
     Name: mypvc
     Namespace: default
     StorageClass:	""
     Status: Bound
     Volume: pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2
     Labels: <none>
     Capacity: 20Gi
     Access Modes: RWO
     Events:
       FirstSeen LastSeen Count From        SubObjectPath Type Reason Message
       --------- -------- ----- ----        ------------- -------- ------ -------
       3m 3m 1 {ibm.io/ibmc-block 31898035-3011-11e7-a6a4-7a08779efd33 } Normal Provisioning External provisioner is provisioning volume  for claim "default/my-persistent-volume-claim"
       3m 1m	 10 {persistentvolume-controller } Normal ExternalProvisioning cannot find provisioner "ibm.io/ibmc-block", expecting that  a volume for the claim is provisioned either manually or via external software
       1m 1m 1 {ibm.io/ibmc-block 31898035-3011-11e7-a6a4-7a08779efd33 } Normal ProvisioningSucceeded	Successfully provisioned volume  pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2
     ```
     {: screen}

您已成功创建 PV，并将其绑定到 PVC。现在，集群用户可以向自己的部署[安装 PVC](#block_app_volume_mount)，并开始对 PV 执行读写操作。

<br />



## 在有状态集内使用块存储器
{: #block_statefulset}

如果您有一个有状态的应用程序（如数据库），那么可以创建有状态集，以使用块存储器来存储应用程序的数据。或者，可以使用 {{site.data.keyword.Bluemix_notm}} 数据库即服务，并将数据存储在云中。
{: shortdesc}

**向有状态集添加块存储器时需要了解哪些内容？**</br>要将存储器添加到有状态集，请在有状态集 YAML 的 `volumeClaimTemplates` 部分中指定存储器配置。`volumeClaimTemplates` 是 PVC 的基础，可以包含要供应的块存储器的存储类和大小或 IOPS。但是，如果要在 `volumeClaimTemplates` 中包含标签，那么 Kubernetes 在创建 PVC 时，不会包含这些标签。您必须改为将标签直接添加到有状态集。

不能同时部署两个有状态集。如果在一个有状态集完全部署之前尝试创建另一个有状态集，那么前一个有状态集的部署可能会导致意外的结果。
{: important}

**如何在特定专区中创建有状态集？**</br>在多专区集群中，可以在有状态集 YAML 的 `spec.selector.matchLabels` 和 `spec.template.metadata.labels` 部分中，指定要在其中创建有状态集的专区和区域。或者，可以将这些标签添加到[定制存储类](/docs/containers?topic=containers-kube_concepts#customized_storageclass)，并在有状态集的 `volumeClaimTemplates` 部分中使用此存储类。

**能否延迟 PV 与有状态 pod 的绑定，直到该 pod 准备就绪？**<br>
可以，您可以为 PVC [创建定制存储类](#topology_yaml)，其中包含 [`volumeBindingMode: WaitForFirstConsumer` ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/storage/storage-classes/#volume-binding-mode) 字段。

**向有状态集添加块存储器时有哪些选项可用？**</br>如果要在创建有状态集时自动创建 PVC，请使用[动态供应](#block_dynamic_statefulset)。您还可以选择对有状态集[预先供应 PVC 或使用现有 PVC](#block_static_statefulset)。  

### 动态供应：创建有状态集时创建 PVC
{: #block_dynamic_statefulset}

如果要在创建有状态集时自动创建 PVC，请使用此选项。
{: shortdesc}

开始之前：[登录到您的帐户。如果适用，请将相应的资源组设定为目标。为集群设置上下文。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1. 验证集群中所有现有的有状态集是否已完全部署。如果某个有状态集仍在进行部署，那么无法开始创建有状态集。您必须等待集群中的所有有状态集完全部署，以避免发生意外结果。
   1. 列出集群中现有的有状态集。
      ```
      kubectl get statefulset --all-namespaces
      ```
      {: pre}

      输出示例：
    ```
      NAME              DESIRED   CURRENT   AGE
      mystatefulset     3         3         6s
      ```
      {: screen}

   2. 查看每个有状态集的 **Pods Status**，以确保有状态集部署完成。  
      ```
      kubectl describe statefulset <statefulset_name>
      ```
      {: pre}

      输出示例：
    ```
      Name:               nginx
      Namespace:          default
      CreationTimestamp:  Fri, 05 Oct 2018 13:22:41 -0400
      Selector:           app=nginx,billingType=hourly,region=us-south,zone=dal10
      Labels:             app=nginx
                          billingType=hourly
                          region=us-south
                          zone=dal10
      Annotations:        kubectl.kubernetes.io/last-applied-configuration={"apiVersion":"apps/v1","kind":"StatefulSet","metadata":{"annotations":{},"name":"nginx","namespace":"default"},"spec":{"podManagementPolicy":"Par...
      Replicas:           3 desired | 3 total
      Pods Status:        0 Running / 3 Waiting / 0 Succeeded / 0 Failed
      Pod Template:
        Labels:  app=nginx
                 billingType=hourly
                 region=us-south
                 zone=dal10
      ...
      ```
      {: screen}

      在 CLI 输出的 **Replicas** 部分中找到的副本数等于 **Pods Status** 部分中 **Running** pod 的数目时，说明有状态集已完全部署。如果有状态集尚未完全部署，请等待部署完成，然后再继续。

2. 为有状态集和用于公开有状态集的服务创建配置文件。

   - **指定专区的有状态集示例：**

     以下示例显示了如何将 NGINX 部署为具有 3 个副本的有状态集。对于每个副本，将根据 `ibmc-block-retain-bronze` 存储类中定义的规范供应一个 20 千兆字节的块存储设备。所有存储设备都在 `dal10` 专区中供应。因为块存储器无法从其他专区进行访问，所以有状态集的所有副本也将部署到位于 `dal10` 中的工作程序节点上。

     ```
     apiVersion: v1
     kind: Service
     metadata:
      name: nginx
      labels:
        app: nginx
     spec:
      ports:
      - port: 80
        name: web
      clusterIP: None
      selector:
        app: nginx
     ---
     apiVersion: apps/v1
     kind: StatefulSet
     metadata:
      name: nginx
     spec:
      serviceName: "nginx"
      replicas: 3
      podManagementPolicy: Parallel
      selector:
        matchLabels:
          app: nginx
          billingType: "hourly"
          region: "us-south"
          zone: "dal10"
      template:
        metadata:
          labels:
            app: nginx
            billingType: "hourly"
            region: "us-south"
            zone: "dal10"
        spec:
          containers:
          - name: nginx
            image: k8s.gcr.io/nginx-slim:0.8
            ports:
            - containerPort: 80
              name: web
            volumeMounts:
            - name: myvol
              mountPath: /usr/share/nginx/html
      volumeClaimTemplates:
      - metadata:
          name: myvol
        spec:
          accessModes:
          - ReadWriteOnce
          resources:
            requests:
              storage: 20Gi
              iops: "300" #required only for performance storage
	      storageClassName: ibmc-block-retain-bronze
     ```
     {: codeblock}

   - **具有反亲缘关系规则并延迟块存储器创建的示例有状态集：**

     以下示例显示了如何将 NGINX 部署为具有 3 个副本的有状态集。有状态集未指定在其中创建块存储器的区域和专区。有状态集改为使用反亲缘关系规则来确保 pod 跨工作程序节点和专区分布。通过定义 `topologykey: failure-domain.beta.kubernetes.io/zone`，如果工作程序节点与具有 `app: nginx` 标签的 pod 位于同一专区中，那么 Kubernetes 调度程序无法将 pod 安排在该工作程序节点上。对于每个有状态集 pod，将创建两个 PVC，如 `volumeClaimTemplates` 部分中所定义，但会延迟创建块存储器实例，直到安排了使用该存储器的有状态集 pod。此设置称为[拓扑感知卷安排](https://kubernetes.io/blog/2018/10/11/topology-aware-volume-provisioning-in-kubernetes/)。

     ```
     apiVersion: storage.k8s.io/v1
     kind: StorageClass
     metadata:
       name: ibmc-block-bronze-delayed
     parameters:
       billingType: hourly
       classVersion: "2"
       fsType: ext4
       iopsPerGB: "2"
       sizeRange: '[20-12000]Gi'
       type: Endurance
     provisioner: ibm.io/ibmc-block
     reclaimPolicy: Delete
     volumeBindingMode: WaitForFirstConsumer
     ---
     apiVersion: v1
     kind: Service
     metadata:
       name: nginx
       labels:
         app: nginx
     spec:
       ports:
       - port: 80
         name: web
       clusterIP: None
       selector:
         app: nginx
     ---
     apiVersion: apps/v1
     kind: StatefulSet
     metadata:
       name: web
     spec:
       serviceName: "nginx"
       replicas: 3
       podManagementPolicy: "Parallel"
       selector:
         matchLabels:
           app: nginx
       template:
         metadata:
           labels:
             app: nginx
         spec:
           affinity:
             podAntiAffinity:
               preferredDuringSchedulingIgnoredDuringExecution:
               - weight: 100
                 podAffinityTerm:
                   labelSelector:
                     matchExpressions:
                     - key: app
                       operator: In
                       values:
                       - nginx
                   topologyKey: failure-domain.beta.kubernetes.io/zone
           containers:
           - name: nginx
             image: k8s.gcr.io/nginx-slim:0.8
             ports:
             - containerPort: 80
               name: web
             volumeMounts:
             - name: www
               mountPath: /usr/share/nginx/html
             - name: wwwww
               mountPath: /tmp1
       volumeClaimTemplates:
       - metadata:
           name: myvol1
         spec:
           accessModes:
           - ReadWriteOnce # access mode
           resources:
             requests:
               storage: 20Gi
	       storageClassName: ibmc-block-bronze-delayed
       - metadata:
           name: myvol2
         spec:
           accessModes:
           - ReadWriteOnce # access mode
           resources:
             requests:
               storage: 20Gi
	       storageClassName: ibmc-block-bronze-delayed
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
     <td style="text-align:left"><code>spec.podManagementPolicy</code></td>
     <td style="text-align:left">输入要用于有状态集的 pod 管理策略。在以下选项之间进行选择：<ul><li><strong>`OrderedReady`：</strong>使用此选项时，有状态集副本会一个接一个地进行部署。例如，如果指定了 3 个副本，那么 Kubernetes 会为第一个副本创建 PVC，然后等待 PVC 绑定后，部署该有状态集副本，接着将 PVC 安装到该副本。此部署完成后，将部署第二个副本。有关此选项的更多信息，请参阅 [`OrderedReady` pod 管理 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/#orderedready-pod-management)。</li><li><strong>并行</strong>：使用此选项时，将同时启动所有有状态集副本的部署。如果应用程序支持对副本进行并行部署，请使用此选项以节省 PVC 和有状态集副本的部署时间。</li></ul></td>
     </tr>
     <tr>
     <td style="text-align:left"><code>spec.selector.matchLabels</code></td>
     <td style="text-align:left">输入要包括在有状态集和 PVC 中的所有标签。Kubernetes 无法识别在有状态集的 <code>volumeClaimTemplates</code> 中包含的标签。您可能想要包含的样本标签包括：<ul><li><code><strong>region</strong></code> 和 <code><strong>zone</strong></code>：如果要在一个特定专区中创建所有有状态集副本和 PVC，请添加这两个标签。您还可以在使用的存储类中指定专区和区域。如果未指定专区和区域，并且您有多专区集群，那么将循环选择供应存储器的专区，以在所有专区中均匀均衡卷请求。</li><li><code><strong>billingType</strong></code>：输入要用于 PVC 的计费类型。选择 <code>hourly</code> 或 <code>monthly</code>。如果未指定此标签，那么会使用 hourly 计费类型来创建所有 PVC。</li></ul></td>
     </tr>
     <tr>
     <td style="text-align:left"><code>spec.template.metadata.labels</code></td>
     <td style="text-align:left">输入已添加到 <code>spec.selector.matchLabels</code> 部分的标签。</td>
     </tr>
     <tr>
     <td style="text-align:left"><code>spec.template.spec.affinity</code></td>
     <td style="text-align:left">指定反亲缘关系规则，以确保有状态集 pod 跨工作程序节点和专区分布。此示例显示了反亲缘关系规则，其中有状态集 pod 不希望安排在运行具有 `app: nginx` 标签的 pod 的工作程序节点上。`topologykey: failure-domain.beta.kubernetes.io/zone` 甚至进一步限制了此反亲缘关系规则：如果工作程序节点与具有 `app: nginx` 标签的 pod 位于同一专区中，那么此项会阻止 pod 安排在该工作程序节点上。通过使用此反亲缘关系规则，可以跨工作程序节点和专区实现反亲缘关系。</td>
     </tr>
     <tr>
     <td style="text-align:left"><code>spec.volumeClaimTemplates.metadata.name</code></td>
     <td style="text-align:left">输入卷的名称。使用在 <code>spec.containers.volumeMount.name</code> 部分中定义的名称。在此输入的名称将用于创建格式如下的 PVC 名称：<code>&lt;volume_name&gt;-&lt;statefulset_name&gt;-&lt;replica_number&gt;</code>。</td>
     </tr>
     <tr>
     <td style="text-align:left"><code>spec.volumeClaimTemplates.spec.resources.</code></br><code>requests.storage</code></td>
     <td style="text-align:left">输入块存储器的大小，以千兆字节 (Gi) 为单位。</td>
     </tr>
     <tr>
     <td style="text-align:left"><code>spec.volumeClaimTemplates.spec.resources.</code></br><code>requests.iops</code></td>
     <td style="text-align:left">如果要供应[性能存储器](#block_predefined_storageclass)，请输入 IOPS 数。如果使用耐久性存储类并指定了一些 IOPS，那么将忽略 IOPS 数，而改为使用在存储类中指定的 IOPS。</td>
     </tr>
     <tr>
     <td style="text-align:left"><code>spec.volumeClaimTemplates.</code></br><code>spec.storageClassName</code></td>
     <td style="text-align:left">输入要使用的存储类。要列出现有存储类，请运行 <code>kubectl get storageclasses | grep block</code>。如果未指定存储类，那么会使用在集群中设置的缺省存储类来创建 PVC。请确保缺省存储类使用的是 <code>ibm.io/ibmc-block</code> 供应程序，以便为有状态集供应块存储器。</td>
     </tr>
     </tbody></table>

4. 创建有状态集。
   ```
   kubectl apply -f statefulset.yaml
   ```
   {: pre}

5. 等待有状态集进行部署。
   ```
   kubectl describe statefulset <statefulset_name>
   ```
   {: pre}

   要查看 PVC 的当前状态，请运行 `kubectl get pvc`。PVC 名称的格式为 `<volume_name>-<statefulset_name>-<replica_number>`。
   {: tip}

### 静态供应：将现有 PVC 用于有状态集
{: #block_static_statefulset}

您可以在创建有状态集之前预先供应 PVC，也可以将现有 PVC 用于有状态集。
{: shortdesc}

如果是在[创建有状态集时动态供应 PVC](#block_dynamic_statefulset)，那么 PVC 的名称将根据在有状态集 YAML 文件中使用的值来指定。为了使有状态集使用现有 PVC，PVC 的名称必须与使用动态供应时自动创建的名称相匹配。

开始之前：[登录到您的帐户。如果适用，请将相应的资源组设定为目标。为集群设置上下文。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1. 如果要在创建有状态集之前为该有状态集预供应 PVC，请执行[向应用程序添加块存储器](#add_block)中的步骤 1-3，以便为每个有状态集副本创建 PVC。确保创建 PVC 时所使用的名称遵循以下格式：`<volume_name>-<statefulset_name>-<replica_number>`。
   - **`<volume_name>`**：使用要在有状态集的 `spec.volumeClaimTemplates.metadata.name` 部分中指定的名称，例如 `nginxvol`。
   - **`<statefulset_name>`**：使用要在有状态集的 `metadata.name` 部分中指定的名称，例如 `nginx_statefulset`。
   - **`<replica_number>`**：输入副本数，起始值为 0。

   例如，如果必须创建 3 个有状态集副本，请使用以下名称创建 3 个 PVC：`nginxvol-nginx_statefulset-0`、`nginxvol-nginx_statefulset-1` 和 `nginxvol-nginx_statefulset-2`。  

   想要为现有存储设备创建 PVC 和 PV？请使用[静态供应](#existing_block)来创建 PVC 和 PV。

2. 执行[动态供应：创建有状态集时创建 PVC](#block_dynamic_statefulset) 中的步骤来创建有状态集。PVC 的名称遵循格式 `<volume_name>-<statefulset_name>-<replica_number>`。确保在有状态集规范中使用 PVC 名称的以下值：
   - **`spec.volumeClaimTemplates.metadata.name`**：输入 PVC 名称的 `<volume_name>`。
   - **`metadata.name`**：输入 PVC 名称的 `<statefulset_name>`。
   - **`spec.replicas`**：输入要为有状态集创建的副本数。副本数必须等于早先创建的 PVC 数。

   如果 PVC 位于不同专区中，那么不要在有状态集内包含区域或专区标签。
   {: note}

3. 验证是否在有状态集副本 pod 中使用了 PVC。
   1. 列出集群中的 pod。识别属于有状态集的 pod。
      ```
      kubectl get pods
      ```
      {: pre}

   2. 验证现有 PVC 是否已安装到有状态集副本。请在 CLI 输出的 **`Volumes`** 部分中查看 **`ClaimName`**。
      ```
        kubectl describe pod <pod_name>
        ```
      {: pre}

      输出示例：
    ```
      Name:           nginx-0
      Namespace:      default
      Node:           10.xxx.xx.xxx/10.xxx.xx.xxx
      Start Time:     Fri, 05 Oct 2018 13:24:59 -0400
      ...
      Volumes:
        myvol:
          Type:       PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
          ClaimName:  myvol-nginx-0
     ...
      ```
      {: screen}

<br />


## 更改现有存储设备的大小和 IOPS
{: #block_change_storage_configuration}

如果要提高存储容量或性能，可以修改现有卷。
{: shortdesc}

有关计费的问题以及要查找如何使用 {{site.data.keyword.Bluemix_notm}} 控制台来修改存储器的步骤，请参阅[扩展块存储器容量](/docs/infrastructure/BlockStorage?topic=BlockStorage-expandingcapacity#expandingcapacity)和[调整 IOPS](/docs/infrastructure/BlockStorage?topic=BlockStorage-adjustingIOPS)。通过控制台进行的更新不会反映在持久卷 (PV) 中。要将此信息添加到 PV，请运行 `kubectl patch pv <pv_name>`，并在 PV 的 **Labels** 和 **Annotation** 部分中手动更新大小和 IOPS。
{: tip}

1. 列出集群中的 PVC，并记下 **VOLUME** 列中关联 PV 的名称。
   ```
   kubectl get pvc
   ```
   {: pre}

   输出示例：
   ```
   NAME             STATUS    VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS        AGE
   myvol            Bound     pvc-01ac123a-123b-12c3-abcd-0a1234cb12d3   20Gi       RWO            ibmc-block-bronze    147d
   ```
   {: screen}

2. 如果要更改块存储器的 IOPS 和大小，请先在 PV 的 `metadata.labels.IOPS` 部分中编辑 IOPS。可以更改为更低或更高的 IOPS 值。确保输入您拥有的存储类型支持的 IOPS。例如，如果具有 4 IOPS 的耐久性块存储器，那么可以将 IOPS 更改为 2 或 10。有关更多支持的 IOPS 值，请参阅[决定块存储器配置](/docs/containers?topic=containers-block_storage#block_predefined_storageclass)。
   ```
   kubectl edit pv <pv_name>
   ```
   {: pre}

   要通过 CLI 更改 IOPS，还必须更改块存储器的大小。如果要仅更改 IOPS，而不更改大小，那么必须[通过控制台请求 IOPS 更改](/docs/infrastructure/BlockStorage?topic=BlockStorage-adjustingIOPS)。
   {: note}

3. 编辑 PVC 并在 PVC 的 `spec.resources.requests.storage` 部分中添加新大小。可以更改为更大的大小，但不能超过存储类设置的最大容量。不能减小现有存储器的大小。要查看存储类的可用大小，请参阅[决定块存储器配置](/docs/containers?topic=containers-block_storage#block_predefined_storageclass)。
   ```
   kubectl edit pvc <pvc_name>
   ```
   {: pre}

4. 验证是否请求了卷扩展。在 CLI 输出的 **Conditions** 部分中看到 `FileSystemResizePending` 消息时，说明成功请求了卷扩展。 
   ```
   kubectl describe pvc <pvc_name>
   ```
   {: pre}

   输出示例：
   ```
   ...
   Conditions:
   Type                      Status  LastProbeTime                     LastTransitionTime                Reason  Message
   ----                      ------  -----------------                 ------------------                ------  -------
   FileSystemResizePending   True    Mon, 01 Jan 0001 00:00:00 +0000   Thu, 25 Apr 2019 15:52:49 -0400           Waiting for user to (re-)start a pod to finish file system resize of volume on node.
   ```
   {: screen}

5. 列出安装了 PVC 的所有 pod。如果 pod 安装了 PVC，那么会自动处理卷扩展。如果 pod 未安装 PVC，那么必须将 PVC 安装到 pod，以便可以处理卷扩展。 
   ```
      kubectl get pods --all-namespaces -o=jsonpath='{range .items[*]}{"\n"}{.metadata.name}{":\t"}{range .spec.volumes[*]}{.persistentVolumeClaim.claimName}{" "}{end}{end}' | grep "<pvc_name>"
      ```
   {: pre}

   将按以下格式返回安装的 pod：`<pod_name>: <pvc_name>`。

6. 如果 pod 未安装 PVC，请[创建 pod 或部署并安装 PVC](/docs/containers?topic=containers-block_storage#add_block)。如果 pod 安装了 PVC，请继续执行下一步。 

7. 监视卷扩展状态。在 CLI 输出中看到 `"message":"Success"` 消息时，说明卷扩展完成。
   ```
   kubectl get pv <pv_name> -o go-template=$'{{index .metadata.annotations "ibm.io/volume-expansion-status"}}\n'
   ```
   {: pre}

   输出示例：
   ```
   {"size":50,"iops":500,"orderid":38832711,"start":"2019-04-30T17:00:37Z","end":"2019-04-30T17:05:27Z","status":"complete","message":"Success"}
   ```
   {: screen}

8. 验证 CLI 输出的 **Labels** 部分中的大小和 IOPS 是否已更改。
   ```
   kubectl describe pv <pv_name>
   ```
   {: pre}

   输出示例：
   ```
   ...
   Labels:       CapacityGb=50
                 Datacenter=dal10
                 IOPS=500
   ```
   {: screen}


## 备份和复原数据
{: #block_backup_restore}

块存储器已供应到集群中的工作程序节点所在的位置。存储器由 IBM 在集群服务器上托管，以在其中某个服务器停止运行时提供可用性。但是，块存储器不会自动进行备份，因此在整个位置发生故障时可能无法进行访问。为了防止数据丢失或损坏，可以设置定期备份，以便在需要时可用于复原数据。
{: shortdesc}

查看块存储器的以下备份和复原选项：

<dl>
  <dt>设置定期快照</dt>
  <dd><p>可以[为块存储器设置定期快照](/docs/infrastructure/BlockStorage?topic=BlockStorage-snapshots#snapshots)，这是捕获某个时间点的实例状态的只读映像。要存储快照，必须在块存储器上请求快照空间。快照会存储在同一专区的现有存储器实例上。如果用户不小心除去了卷中的重要数据，那么可以通过快照来复原数据。</br></br> <strong>要为卷创建快照，请执行以下操作：</strong><ol><li>[登录到您的帐户。如果适用，请将相应的资源组设定为目标。为集群设置上下文。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)</li><li>登录到 `ibmcloud sl` CLI。<pre class="pre"><code>ibmcloud sl init</code></pre></li><li>列出集群中的现有 PV。<pre class="pre"><code>kubectl get pv</code></pre></li><li>获取要为其创建快照空间的 PV 的详细信息，并记下卷标识、大小和 IOPS。<pre class="pre"><code>kubectl describe pv &lt;pv_name&gt;</code></pre> 大小和 IOPS 会显示在 CLI 输出的 <strong>Labels</strong> 部分中。要查找卷标识，请查看 CLI 输出的 <code>ibm.io/network-storage-id</code> 注释。</li><li>使用您在先前步骤中检索到的参数为现有卷创建快照大小。<pre class="pre"><code>ibmcloud sl block snapshot-order &lt;volume_ID&gt; --size &lt;size&gt; --tier &lt;iops&gt;</code></pre></li><li>等待快照大小创建。<pre class="pre"><code>ibmcloud sl block volume-detail &lt;volume_ID&gt;</code></pre>CLI 输出中的 <strong>Snapshot Size (GB)</strong> 从 0 更改为您所订购的大小时，说明已成功供应快照大小。</li><li>为卷创建快照，并记下创建的快照的标识。<pre class="pre"><code>ibmcloud sl block snapshot-create &lt;volume_ID&gt;</code></pre></li><li>验证快照是否已成功创建。<pre class="pre"><code>ibmcloud sl block snapshot-list &lt;volume_ID&gt;</code></pre></li></ol></br><strong>要将数据从快照复原到现有卷，请运行以下命令：</strong><pre class="pre"><code>ibmcloud sl block snapshot-restore &lt;volume_ID&gt; &lt;snapshot_ID&gt;</code></pre></p></dd>
  <dt>将快照复制到其他专区</dt>
 <dd><p>为了保护数据不受专区故障的影响，可以[复制快照](/docs/infrastructure/BlockStorage?topic=BlockStorage-replication#replication)到其他专区中设置的块存储器实例。数据只能从主存储器复制到备份存储器。不能将复制的块存储器实例安装到集群。主存储器发生故障时，可以手动将复制的备份存储器设置为主存储器。然后，可以将其安装到集群。复原主存储器后，可以从备份存储器复原数据。</p></dd>
 <dt>复制存储器</dt>
 <dd><p>可以在原始存储器实例所在的专区中[复制块存储器实例](/docs/infrastructure/BlockStorage?topic=BlockStorage-duplicatevolume#duplicatevolume)。复制项所含的数据是其创建的那个时间点上原始存储器实例的数据。与副本不同，复制项用作独立于原始项的存储器实例。要进行复制，请首先为卷设置快照。</p></dd>
  <dt>将数据备份到 {{site.data.keyword.cos_full}}</dt>
  <dd><p>可以使用 [**ibm-backup-restore 映像**](/docs/services/RegistryImages/ibm-backup-restore?topic=RegistryImages-ibmbackup_restore_starter#ibmbackup_restore_starter)来启动集群中的备份和复原 pod。此 pod 包含一个脚本，用于对集群中的任何持久卷声明 (PVC) 运行一次性或定期备份。数据存储在您在某个专区设置的 {{site.data.keyword.cos_full}} 实例中。</p><p class="note">块存储器使用 RWO 访问方式进行安装。此访问权仅允许一次将一个 pod 安装到块存储器。要备份数据，必须从存储器中卸载应用程序 pod，将其安装到备份 pod，备份数据，然后将存储器重新安装到应用程序 pod。</p>
要使数据具有更高可用性，并保护应用程序不受专区故障的影响，请设置第二个 {{site.data.keyword.cos_short}} 实例，并在各个专区之间复制数据。如果需要从 {{site.data.keyword.cos_short}} 实例复原数据，请使用随映像一起提供的复原脚本。</dd>
<dt>将数据复制到 pod 和容器，以及从 pod 和容器中复制数据</dt>
<dd><p>可以使用 `kubectl cp` [命令 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/reference/kubectl/overview/#cp) 将文件和目录复制到集群中的 pod 或特定容器，或者从集群中的 pod 或特定容器中复制文件和目录。</p>
<p>开始之前：[登录到您的帐户。如果适用，请将相应的资源组设定为目标。为集群设置上下文。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
如果未使用 <code>-c</code> 来指定容器，那么此命令将使用 pod 中的第一个可用容器。</p>
<p>可以通过以下各种方法来使用此命令：</p>
<ul>
<li>将本地机器中的数据复制到集群中的 pod：<pre class="pre"><code>kubectl cp <var>&lt;local_filepath&gt;/&lt;filename&gt;</var> <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;</var></code></pre></li>
<li>将集群的 pod 中的数据复制到本地机器：<pre class="pre"><code>kubectl cp <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;/&lt;filename&gt;</var> <var>&lt;local_filepath&gt;/&lt;filename&gt;</var></code></pre></li>
<li>将数据从本地计算机复制到在集群中的 pod 中运行的特定容器：<pre class="pre"><code>kubectl cp <var>&lt;local_filepath&gt;/&lt;filename&gt;</var> <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;</var> -c <var>&lt;container&gt;</var></code></pre></li>
</ul></dd>
  </dl>

<br />


## 存储类参考
{: #block_storageclass_reference}

### 铜牌级
{: #block_bronze}

<table>
<caption>块存储类：铜牌级</caption>
<thead>
<th>特征</th>
<th>设置</th>
</thead>
<tbody>
<tr>
<td>名称</td>
<td><code>ibmc-block-bronze</code></br><code>ibmc-block-retain-bronze</code></td>
</tr>
<tr>
<td>类型</td>
<td>[耐久性存储器](/docs/infrastructure/BlockStorage?topic=BlockStorage-About#provendurance)</td>
</tr>
<tr>
<td>文件系统</td>
<td>ext4</td>
</tr>
<tr>
<td>IOPS/千兆字节</td>
<td>2</td>
</tr>
<tr>
<td>大小范围（千兆字节）</td>
<td>20-12000 Gi</td>
</tr>
<tr>
<td>硬盘</td>
<td>SSD</td>
</tr>
<tr>
<td>计费</td>
<td>缺省计费类型取决于 {{site.data.keyword.Bluemix_notm}} Block Storage 插件的版本：<ul><li> V1.0.1 和更高版本：按小时</li><li>V1.0.0 和更低版本：按月</li></ul></td>
</tr>
<tr>
<td>定价</td>
<td>[定价信息 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/cloud/block-storage/pricing)</td>
</tr>
</tbody>
</table>


### 银牌级
{: #block_silver}

<table>
<caption>块存储类：银牌级</caption>
<thead>
<th>特征</th>
<th>设置</th>
</thead>
<tbody>
<tr>
<td>名称</td>
<td><code>ibmc-block-silver</code></br><code>ibmc-block-retain-silver</code></td>
</tr>
<tr>
<td>类型</td>
<td>[耐久性存储器](/docs/infrastructure/BlockStorage?topic=BlockStorage-About#provendurance)</td>
</tr>
<tr>
<td>文件系统</td>
<td>ext4</td>
</tr>
<tr>
<td>IOPS/千兆字节</td>
<td>4</td>
</tr>
<tr>
<td>大小范围（千兆字节）</td>
<td>20-12000 Gi</td>
</tr>
<tr>
<td>硬盘</td>
<td>SSD</td>
</tr>
<tr>
<td>计费</td>
<td>缺省计费类型取决于 {{site.data.keyword.Bluemix_notm}} Block Storage 插件的版本：<ul><li> V1.0.1 和更高版本：按小时</li><li>V1.0.0 和更低版本：按月</li></ul></td>
</tr>
<tr>
<td>定价</td>
<td>[定价信息 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/cloud/block-storage/pricing)</td>
</tr>
</tbody>
</table>

### 金牌级
{: #block_gold}

<table>
<caption>块存储类：金牌级</caption>
<thead>
<th>特征</th>
<th>设置</th>
</thead>
<tbody>
<tr>
<td>名称</td>
<td><code>ibmc-block-gold</code></br><code>ibmc-block-retain-gold</code></td>
</tr>
<tr>
<td>类型</td>
<td>[耐久性存储器](/docs/infrastructure/BlockStorage?topic=BlockStorage-About#provendurance)</td>
</tr>
<tr>
<td>文件系统</td>
<td>ext4</td>
</tr>
<tr>
<td>IOPS/千兆字节</td>
<td>10</td>
</tr>
<tr>
<td>大小范围（千兆字节）</td>
<td>20-4000 Gi</td>
</tr>
<tr>
<td>硬盘</td>
<td>SSD</td>
</tr>
<tr>
<td>计费</td>
<td>缺省计费类型取决于 {{site.data.keyword.Bluemix_notm}} Block Storage 插件的版本：<ul><li> V1.0.1 和更高版本：按小时</li><li>V1.0.0 和更低版本：按月</li></ul></td>
</tr>
<tr>
<td>定价</td>
<td>[定价信息 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/cloud/block-storage/pricing)</td>
</tr>
</tbody>
</table>

### 定制
{: #block_custom}

<table>
<caption>块存储类：定制</caption>
<thead>
<th>特征</th>
<th>设置</th>
</thead>
<tbody>
<tr>
<td>名称</td>
<td><code>ibmc-block-custom</code></br><code>ibmc-block-retain-custom</code></td>
</tr>
<tr>
<td>类型</td>
<td>[性能](/docs/infrastructure/BlockStorage?topic=BlockStorage-About#provperformance)</td>
</tr>
<tr>
<td>文件系统</td>
<td>ext4</td>
</tr>
<tr>
<td>IOPS 和大小</td>
<td><strong>大小范围（以千兆字节计）/ IOPS 范围（100 的倍数）</strong><ul><li>20-39 Gi / 100-1000 IOPS</li><li>40-79 Gi / 100-2000 IOPS</li><li>80-99 Gi / 100-4000 IOPS</li><li>100-499 Gi / 100-6000 IOPS</li><li>500-999 Gi / 100-10000 IOPS</li><li>1000-1999 Gi / 100-20000 IOPS</li><li>2000-2999 Gi / 200-40000 IOPS</li><li>3000-3999 Gi / 200-48000 IOPS</li><li>4000-7999 Gi / 300-48000 IOPS</li><li>8000-9999 Gi / 500-48000 IOPS</li><li>10000-12000 Gi / 1000-48000 IOPS</li></ul></td>
</tr>
<tr>
<td>硬盘</td>
<td>IOPS 与千兆字节的比率确定供应的硬盘类型。要确定 IOPS 与千兆字节的比率，请将 IOPS 除以存储器的大小。</br></br>示例：</br>对于 100 IOPS，您选择了 500 Gi 存储器。因此，比率为 0.2 (100 IOPS/500 Gi)。</br></br><strong>每个比率的硬盘类型概述：</strong><ul><li>小于或等于 0.3：SATA</li><li>大于 0.3：SSD</li></ul></td>
</tr>
<tr>
<td>计费</td>
<td>缺省计费类型取决于 {{site.data.keyword.Bluemix_notm}} Block Storage 插件的版本：<ul><li> V1.0.1 和更高版本：按小时</li><li>V1.0.0 和更低版本：按月</li></ul></td>
</tr>
<tr>
<td>定价</td>
<td>[定价信息 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/cloud/block-storage/pricing)</td>
</tr>
</tbody>
</table>

<br />


## 样本定制存储类
{: #block_custom_storageclass}

您可以创建定制存储类并在 PVC 中使用该存储类。
{: shortdesc}

{{site.data.keyword.containerlong_notm}} 提供了[预定义的存储类](#block_storageclass_reference)，用于供应具有特定层和配置的块存储器。在某些情况下，您可能希望使用预定义的存储类中未涵盖的其他配置来供应存储器。您可以使用本主题中的示例来找到样本定制存储类。

要创建定制存储类，请参阅[定制存储类](/docs/containers?topic=containers-kube_concepts#customized_storageclass)。然后查看[在 PVC 中使用定制存储类](#add_block)。

### 创建拓扑感知存储器
{: #topology_yaml}

要在多专区集群中使用块存储器，必须将 pod 安排在块存储器实例所在的专区中，以便您可以读取和写入卷。在 Kubernetes 引入拓扑感知卷安排之前，创建 PVC 时，存储器的动态供应会自动创建块存储器实例。然后，在创建 pod 时，Kubernetes 调度程序会尝试将 pod 部署到块存储器实例所在的数据中心。
{: shortdesc}

在不了解 pod 约束的情况下创建块存储器实例可能会导致意外结果。例如，可能会由于存储器所在的工作程序节点的资源不足，或者该工作程序节点有污点而不允许安排 pod，导致您的 pod 无法安排到该工作程序节点。使用拓扑感知卷安排时，会延迟块存储器实例创建，直到创建了使用该存储器的第一个 pod。

只有运行 Kubernetes V1.12 或更高版本的集群上支持拓扑感知卷安排。要使用此功能，请确保已安装 {{site.data.keyword.Bluemix_notm}} Block Storage 插件 V1.2.0 或更高版本。
{: note}

以下示例显示了如何创建存储类以用于延迟块存储器实例创建，直到使用此存储器的第一个 pod 准备就绪可安排为止。要延迟创建，必须包含 `volumeBindingMode: WaitForFirstConsumer` 选项。如果不包含此选项，那么 `volumeBindingMode` 会自动设置为 `Immediate`，并将在创建 PVC 时创建块存储器实例。

- **耐久性块存储器的示例：**
  ```
  apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
    name: ibmc-block-bronze-delayed
  parameters:
    billingType: hourly
    classVersion: "2"
    fsType: ext4
    iopsPerGB: "2"
    sizeRange: '[20-12000]Gi'
    type: Endurance
  provisioner: ibm.io/ibmc-block
  reclaimPolicy: Delete
  volumeBindingMode: WaitForFirstConsumer
  ```
  {: codeblock}

- **性能块存储器的示例：**
  ```
  apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
   name: ibmc-block-performance-storageclass
   labels:
     kubernetes.io/cluster-service: "true"
  provisioner: ibm.io/ibmc-block
  parameters:
   billingType: "hourly"
   classVersion: "2"
   sizeIOPSRange: |-
     "[20-39]Gi:[100-1000]"
     "[40-79]Gi:[100-2000]"
     "[80-99]Gi:[100-4000]"
     "[100-499]Gi:[100-6000]"
     "[500-999]Gi:[100-10000]"
     "[1000-1999]Gi:[100-20000]"
     "[2000-2999]Gi:[200-40000]"
     "[3000-3999]Gi:[200-48000]"
     "[4000-7999]Gi:[300-48000]"
     "[8000-9999]Gi:[500-48000]"
     "[10000-12000]Gi:[1000-48000]"
   type: "Performance"
  reclaimPolicy: Delete
  volumeBindingMode: WaitForFirstConsumer
  ```
  {: codeblock}

### 指定专区和区域
{: #block_multizone_yaml}

如果要在特定专区中创建块存储器，可以在定制存储类中指定专区和区域。
{: shortdesc}

如果使用的是 {{site.data.keyword.Bluemix_notm}} Block Storage V1.0.0 插件，或者要在特定专区中[静态供应块存储器](#existing_block)，请使用定制存储类。在其他所有情况下，请[直接在 PVC 中指定专区](#add_block)。
{: note}

以下 `.yaml` 文件定制基于 `ibm-block-silver` 非保留存储类的存储类：`type` 为 `"Endurance"`，`iopsPerGB` 为 `4`，`sizeRange` 为 `"[20-12000]Gi"`，`reclaimPolicy` 设置为 `"Delete"`。专区指定为 `dal12`。要将其他存储类用作您的基础，请参阅[存储类参考](#block_storageclass_reference)。

在集群和工作程序节点所在的区域和专区中创建存储类。要获取集群的区域，请运行 `ibmcloud ks cluster-get --cluster <cluster_name_or_ID>` 并在 **Master URL** 中查找区域前缀，例如 `https://c2.eu-de.containers.cloud.ibm.com:11111` 中的 `eu-de`。要获取工作程序节点的专区，请运行 `ibmcloud ks workers --cluster <cluster_name_or_ID>`。

- **耐久性块存储器的示例：**
  ```
  apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
    name: ibmc-block-silver-mycustom-storageclass
    labels:
      kubernetes.io/cluster-service: "true"
  provisioner: ibm.io/ibmc-block
  parameters:
    zone: "dal12"
    region: "us-south"
    type: "Endurance"
    iopsPerGB: "4"
    sizeRange: "[20-12000]Gi"
  reclaimPolicy: "Delete"
  ```
  {: codeblock}

- **性能块存储器的示例：**
  ```
  apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
    name: ibmc-block-performance-storageclass
    labels:
      kubernetes.io/cluster-service: "true"
  provisioner: ibm.io/ibmc-block
  parameters:
    zone: "dal12"
    region: "us-south"
    type: "Performance"
    sizeIOPSRange: |-
      "[20-39]Gi:[100-1000]"
      "[40-79]Gi:[100-2000]"
      "[80-99]Gi:[100-4000]"
      "[100-499]Gi:[100-6000]"
      "[500-999]Gi:[100-10000]"
      "[1000-1999]Gi:[100-20000]"
      "[2000-2999]Gi:[200-40000]"
      "[3000-3999]Gi:[200-48000]"
      "[4000-7999]Gi:[300-48000]"
      "[8000-9999]Gi:[500-48000]"
      "[10000-12000]Gi:[1000-48000]"
  reclaimPolicy: "Delete"
  ```
  {: codeblock}

### 使用 `XFS` 文件系统安装块存储器
{: #xfs}

以下示例创建使用 `XFS` 文件系统来供应块存储器的存储类。
{: shortdesc}

- **耐久性块存储器的示例：**
  ```
  apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
    name: ibmc-block-custom-xfs
    labels:
      addonmanager.kubernetes.io/mode: Reconcile
  provisioner: ibm.io/ibmc-block
  parameters:
    type: "Endurance"
    iopsPerGB: "4"
    sizeRange: "[20-12000]Gi"
    fsType: "xfs"
  reclaimPolicy: "Delete"
  ```

- **性能块存储器的示例：**
  ```
   apiVersion: storage.k8s.io/v1
   kind: StorageClass
   metadata:
     name: ibmc-block-custom-xfs
     labels:
       addonmanager.kubernetes.io/mode: Reconcile
   provisioner: ibm.io/ibmc-block
   parameters:
     type: "Performance"
     sizeIOPSRange: |-
       [20-39]Gi:[100-1000]
       [40-79]Gi:[100-2000]
       [80-99]Gi:[100-4000]
       [100-499]Gi:[100-6000]
       [500-999]Gi:[100-10000]
       [1000-1999]Gi:[100-20000]
       [2000-2999]Gi:[200-40000]
       [3000-3999]Gi:[200-48000]
       [4000-7999]Gi:[300-48000]
       [8000-9999]Gi:[500-48000]
       [10000-12000]Gi:[1000-48000]
     fsType: "xfs"
     reclaimPolicy: "Delete"
     classVersion: "2"
   ```
  {: codeblock}

<br />

