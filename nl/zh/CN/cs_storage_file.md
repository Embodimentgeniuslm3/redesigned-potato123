---

copyright:
  years: 2014, 2019
lastupdated: "2019-05-31"

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



# 在 IBM File Storage for IBM Cloud 上存储数据
{: #file_storage}

{{site.data.keyword.Bluemix_notm}} File Storage 是一种基于 NFS 的持久、快速、灵活的网络连接文件存储器，可以使用 Kubernetes 持久卷 (PV) 添加到应用程序。您可以在具有不同 GB 大小和 IOPS 的预定义存储层之间进行选择，以满足工作负载的需求。要了解 {{site.data.keyword.Bluemix_notm}} File Storage 是否为适合您的存储选项，请参阅[选择存储解决方案](/docs/containers?topic=containers-storage_planning#choose_storage_solution)。有关定价信息，请参阅[计费](/docs/infrastructure/FileStorage?topic=FileStorage-about#billing)。
{: shortdesc}

{{site.data.keyword.Bluemix_notm}} File Storage 仅可用于设置为使用公用网络连接的标准集群。如果集群无法访问公用网络（例如，防火墙后面的专用集群或仅启用了专用服务端点的集群），那么可以在集群运行 Kubernetes V1.13.4_1513、1.12.6_1544、1.11.8_1550 或更高版本时供应文件存储器。NFS 文件存储器实例特定于单个专区。如果您有多专区集群，请考虑[多专区持久性存储选项](/docs/containers?topic=containers-storage_planning#persistent_storage_overview)。
{: important}

## 决定文件存储器配置
{: #file_predefined_storageclass}

{{site.data.keyword.containerlong}} 为文件存储器提供了预定义的存储类，可以使用这些类来供应具有特定配置的文件存储器。
{: shortdesc}

每个存储类指定供应的文件存储器的类型，包括可用大小、IOPS、文件系统和保留策略。  

使用存储类来供应特定类型的存储器后，即无法更改存储设备的类型或保留策略。但是，如果要提高存储容量和性能，那么可以[更改大小和 IOPS](#file_change_storage_configuration)。要更改存储器的类型和保留策略，必须[创建新的存储器实例并复制数据](/docs/containers?topic=containers-kube_concepts#update_storageclass)（从旧存储器实例复制到新存储器实例）。
{: important}

开始之前：[登录到您的帐户。如果适用，请将相应的资源组设定为目标。为集群设置上下文。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

要决定存储器配置，请执行以下操作：

1. 列出 {{site.data.keyword.containerlong}} 中的可用存储类。
    
   ```
    kubectl get storageclasses | grep file
    ```
   {: pre}

   输出示例：
   ```
$ kubectl get storageclasses
    NAME                         TYPE
    ibmc-file-bronze (default)   ibm.io/ibmc-file
    ibmc-file-custom             ibm.io/ibmc-file
    ibmc-file-gold               ibm.io/ibmc-file
    ibmc-file-retain-bronze      ibm.io/ibmc-file
    ibmc-file-retain-custom      ibm.io/ibmc-file
    ibmc-file-retain-gold        ibm.io/ibmc-file
    ibmc-file-retain-silver      ibm.io/ibmc-file
    ibmc-file-silver             ibm.io/ibmc-file
    ```
   {: screen}

2. 查看存储类的配置。
  ```
   kubectl describe storageclass <storageclass_name>
   ```
  {: pre}

   有关每个存储类的更多信息，请参阅[存储类参考](#file_storageclass_reference)。如果找不到要查找的内容，请考虑创建您自己的定制存储类。首先，请查看[定制存储类样本](#file_custom_storageclass)。
   {: tip}

3. 选择要供应的文件存储器的类型。
   - **铜牌级、银牌级和金牌级存储类：**这些存储类供应[耐久性存储器](/docs/infrastructure/FileStorage?topic=FileStorage-about#provisioning-with-endurance-tiers)。通过耐久性存储器，可以选择预定义 IOPS 层的存储器大小（以千兆字节为单位）。
   - **定制存储类：**此存储类供应[性能存储器](/docs/infrastructure/FileStorage?topic=FileStorage-about#provisioning-with-performance)。通过性能存储器，可以控制存储器和 IOPS 的大小。

4. 选择文件存储器的大小和 IOPS。IOPS 的大小和数量定义了 IOPS（每秒输入/输出操作数）的总数，这用于指示存储器的速度。存储器的总 IOPS 越高，处理读写操作的速度越快。
   - **铜牌级、银牌级和金牌级存储类：**这些存储类随附固定数量的 IOPS/千兆字节，并在 SSD 硬盘上供应。IOPS 的总数取决于您选择的存储器大小。您可以在允许的大小范围内选择任意整数的千兆字节（例如，20 Gi、256 Gi、11854 Gi）。要确定 IOPS 的总数，必须将 IOPS 乘以所选大小。例如，如果在随附 4 IOPS/GB 的银牌级存储类中选择 1000 Gi 文件存储器大小，那么存储器总计有 4000 IOPS。
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
   - **定制存储类：**选择此存储类时，可以对所需大小和 IOPS 具有更多控制权。对于大小，可以在允许的大小范围内选择任意整数的千兆字节。选择的大小将确定可供您使用的 IOPS 范围。您可以选择指定范围内是 100 的倍数的 IOPS。您选择的 IOPS 是静态的，不会随存储器大小进行缩放。例如，如果针对 100 IOPS 选择 40 Gi，那么总 IOPS 仍为 100。</br></br>IOPS 与千兆字节的比率还可确定供应的硬盘类型。例如，如果对于 100 IOPS 选择了 500 Gi，那么 IOPS 与千兆字节的比率为 0.2。比率小于或等于 0.3 的存储器在 SATA 硬盘上供应。如果比率大于 0.3，那么会在 SSD 硬盘上供应存储器。  
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
   - 如果要保留数据，请选择 `retain` 存储类。删除 PVC 时，仅会删除 PVC。PV、IBM Cloud Infrastructure (SoftLayer) 帐户中的物理存储设备以及数据仍会存在。要回收存储器并再次在集群中使用，必须除去 PV，并执行[使用现有文件存储器](#existing_file)的步骤。
   - 如果要在删除 PVC 时删除 PV、数据和物理文件存储设备，请选择不带 `retain` 的存储类。

6. 选择是要按小时还是按月计费。有关更多信息，请查看[定价 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/cloud/file-storage/pricing)。缺省情况下，所有文件存储设备都以按小时计费类型进行供应。
   如果选择“每月”计费类型，那么除去持久性存储器时，即使只用了很短的时间，您也仍需支付该持久性存储器一个月的费用。
   {: note}

<br />



## 向应用程序添加文件存储器
{: #add_file}

创建持久卷声明 (PVC) 以便为集群[动态供应](/docs/containers?topic=containers-kube_concepts#dynamic_provisioning)文件存储器。动态供应将自动创建匹配的持久卷 (PV)，并在 IBM Cloud Infrastructure (SoftLayer) 帐户中订购物理存储设备。
{:shortdesc}

开始之前：
- 如果您有防火墙，请针对集群所在专区的 IBM Cloud Infrastructure (SoftLayer) IP 范围[允许流出访问](/docs/containers?topic=containers-firewall#pvc)，以便您可以创建 PVC。
- [决定预定义的存储类](#file_predefined_storageclass)或创建[定制存储类](#file_custom_storageclass)。

要在有状态集内部署文件存储器？有关更多信息，请参阅[在有状态集内使用文件存储器](#file_statefulset)。
{: tip}

要添加文件存储器，请执行以下操作：

1.  创建配置文件以定义持久卷声明 (PVC)，并将配置保存为 `.yaml` 文件。

    - **铜牌级、银牌级或金牌级存储类的示例**：
       以下 `.yaml` 文件创建名为 `mypvc` 的声明，其中存储类为 `"ibmc-file-silver"`，计费类型为 `"monthly"`，千兆字节大小为 `24Gi`。

       ```
       apiVersion: v1
       kind: PersistentVolumeClaim
       metadata:
         name: mypvc
         labels:
           billingType: "monthly"
           region: us-south
           zone: dal13
       spec:
         accessModes:
           - ReadWriteMany
         resources:
           requests:
             storage: 24Gi
         storageClassName: ibmc-file-silver
       ```
       {: codeblock}

    -  **使用定制存储类的示例**：
       以下 `.yaml` 文件创建名为 `mypvc` 的声明，其中存储类为 `ibmc-file-retain-custom`，计费类型为 `"hourly"`，千兆字节大小为 `45Gi`，IOPS 为 `"300"`。

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
           - ReadWriteMany
         resources:
           requests:
             storage: 45Gi
             iops: "300"
         storageClassName: ibmc-file-retain-custom
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
          <td>指定用于计算存储器帐单的频率："monthly" 或 "hourly"。如果未指定计费类型，那么会为存储器供应按小时计费类型。 </td>
       </tr>
       <tr>
       <td><code>metadata.labels.region</code></td>
       <td>可选：指定要在其中供应文件存储器的区域。要连接到存储器，请在集群所在的区域中创建存储器。如果指定了区域，那么还必须指定专区。如果未指定区域，或者找不到指定的区域，那么会在集群所在的区域中创建存储器。</br></br>要获取集群的区域，请运行 `ibmcloud ks cluster-get --cluster <cluster_name_or_ID>` 并在 **Master URL** 中查找区域前缀，例如 `https://c2.eu-de.containers.cloud.ibm.com:11111` 中的 `eu-de`。
       </br></br><strong>提示：</strong>您还可以在[定制存储类](#file_multizone_yaml)中指定区域和专区，而不是在 PVC 中指定这些值。然后，在 PVC 的 <code>metadata.annotations.volume.beta.kubernetes.io/storage-class</code> 部分中使用存储类。如果在存储类和 PVC 中都指定了区域和专区，那么 PVC 中的值优先。</td>
       </tr>
       <tr>
       <td><code>metadata.labels.zone</code></td>
       <td>可选：指定要在其中供应文件存储器的专区。要在应用程序中使用存储器，请在工作程序节点所在的专区中创建存储器。要查看工作程序节点的专区，请运行 <code>ibmcloud ks workers --cluster &lt;cluster_name_or_ID&gt;</code>，并查看 CLI 输出的 <strong>Zone</strong> 列。如果指定了专区，那么还必须指定区域。如果未指定专区或在多专区集群中找不到指定的专区，那么将循环选择专区。</br></br><strong>提示：</strong>您还可以在[定制存储类](#file_multizone_yaml)中指定区域和专区，而不是在 PVC 中指定这些值。然后，在 PVC 的 <code>metadata.annotations.volume.beta.kubernetes.io/storage-class</code> 部分中使用存储类。如果在存储类和 PVC 中都指定了区域和专区，那么 PVC 中的值优先。
</td>
       </tr>
       <tr>
       <td><code>spec.accessMode</code></td>
       <td>指定下列选项之一：<ul><li><strong>ReadWriteMany：</strong>多个 pod 可安装 PVC。所有 pod 可对卷进行读取和写入。</li><li><strong>ReadOnlyMany：</strong>PVC 可由多个 pod 安装。所有 pod 都具有只读访问权。<li><strong>ReadWriteOnce：</strong>PVC 只能由一个 pod 安装。此 pod 可对卷进行读取和写入。</li></ul></td>
       </tr>
       <tr>
       <td><code>spec.resources.requests.storage</code></td>
       <td>输入文件存储器的大小，以千兆字节 (Gi) 为单位。供应存储器后，即不能更改文件存储器的大小。因此，请确保指定与要存储的数据量相匹配的大小。</td>
       </tr>
       <tr>
       <td><code>spec.resources.requests.iops</code></td>
       <td>此选项仅适用于定制存储类 (`ibmc-file-custom / ibmc-file-retain-custom`)。请指定存储器的总 IOPS，在允许范围内选择 100 的倍数。如果选择的 IOPS 不同于列出的 IOPS，那么该 IOPS 会向上舍入。</td>
       </tr>
       <tr>
       <td><code>spec.storageClassName</code></td>
       <td>要用于供应文件存储器的存储类的名称。您可以选择使用其中一个 [IBM 提供的存储类](#file_storageclass_reference)或[创建您自己的存储类](#file_custom_storageclass)。</br> 如果未指定存储类，那么会使用缺省存储类 <code>ibmc-file-bronze</code> 来创建 PV。</br></br><strong>提示：</strong>如果要更改缺省存储类，请运行 <code>kubectl patch storageclass &lt;storageclass&gt; -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'</code>，并将 <code>&lt;storageclass&gt;</code> 替换为存储类的名称。</td>
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
    Access Modes:	RWX
    Events:
      FirstSeen	LastSeen	Count	From								SubObjectPath	Type		Reason			Message
      ---------	--------	-----	----								-------------	--------	------			-------
      3m		3m		1	{ibm.io/ibmc-file 31898035-3011-11e7-a6a4-7a08779efd33 }			Normal		Provisioning		External provisioner is provisioning volume for claim "default/my-persistent-volume-claim"
      3m		1m		10	{persistentvolume-controller }							Normal		ExternalProvisioning	cannot find provisioner "ibm.io/ibmc-file", expecting that a volume for the claim is provisioned either manually or via external software
      1m		1m		1	{ibm.io/ibmc-file 31898035-3011-11e7-a6a4-7a08779efd33 }			Normal		ProvisioningSucceeded	Successfully provisioned volume pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2

    ```
    {: screen}

4.  {: #file_app_volume_mount}要将存储器安装到部署，请创建配置 `.yaml` 文件，并指定绑定该 PV 的 PVC。

    如果应用程序需要非 root 用户写入持久性存储器，或者应用程序需要安装路径由 root 用户拥有，请参阅[添加对 NFS 文件存储器的非 root 用户访问权](/docs/containers?topic=containers-cs_troubleshoot_storage#nonroot)或[启用对 NFS 文件存储器的 root 用户许可权](/docs/containers?topic=containers-cs_troubleshoot_storage#nonroot)。
    {: tip}

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
    <td>在容器中安装卷的目录的绝对路径。写入安装路径的数据会存储在物理文件存储器实例中的<code>根</code>目录下。如果要在不同应用程序之间共享卷，可以为每个应用程序指定[卷子路径 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/storage/volumes/#using-subpath)。</td>
    </tr>
    <tr>
    <td><code>spec.containers.volumeMounts.name</code></td>
    <td>要安装到 pod 中的卷的名称。</td>
    </tr>
    <tr>
    <td><code>volumes.name</code></td>
    <td>要安装到 pod 中的卷的名称。通常此名称与 <code>volumeMounts.name</code> 相同。</td>
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


## 在集群中使用现有文件存储器
{: #existing_file}

如果您具有要在集群中使用的现有物理存储设备，那么可以手动创建 PV 和 PVC，以[静态供应](/docs/containers?topic=containers-kube_concepts#static_provisioning)存储器。
{: shortdesc}

开始之前：
- 确保在现有文件存储器实例所在的专区中至少存在一个工作程序节点。
- [登录到您的帐户。如果适用，请将相应的资源组设定为目标。为集群设置上下文。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

### 步骤1：准备现有存储器。
{: #existing-file-1}

在开始将现有存储器安装到应用程序之前，必须检索 PV 的所有必要信息，并准备存储器以使其可供在集群中访问。  
{: shortdesc}

**对于使用 `retain` 存储类供应的存储器：**</br>
如果使用 `retain` 存储类供应了存储器，然后除去 PVC，那么 PV 和物理存储器设备也不会自动除去。要在集群中复用该存储器，必须首先除去剩余 PV。

要在供应现有存储器的集群之外的其他集群中使用该存储器，请执行[在集群外部创建的存储器](#external_storage)的步骤，以将存储器添加到工作程序节点的子网。
{: tip}

1. 列出现有 PV。
       
   ```
    kubectl get pv
    ```
   {: pre}

   查找属于持久性存储器的 PV。该 PV 处于 `released` 状态。

2. 获取该 PV 的详细信息。
   ```
   kubectl describe pv <pv_name>
   ```
   {: pre}

3. 记下 `CapacityGb`、`storageClass`、`failure-domain.beta.kubernetes.io/region`、`failure-domain.beta.kubernetes.io/zone`、`server` 和 `path`。

4. 除去该 PV。
   ```
   kubectl delete pv <pv_name>
   ```
   {: pre}

5. 验证 PV 是否已除去。
   ```
    kubectl get pv
    ```
   {: pre}

</br>

**对于在集群外部供应的持久性存储器：**</br>
如果要使用先前供应但从未在集群中使用过的现有存储器，那么必须使存储器在工作程序节点所在的子网中可用。

1.  {: #external_storage}在 [IBM Cloud Infrastructure (SoftLayer) 门户网站 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/classic?) 中，单击**存储**。
2.  单击**文件存储器**并从**操作**菜单中，选择**授权主机**。
3.  选择**子网**。
4.  从下拉列表中，选择工作程序节点连接到的专用 VLAN 子网。要查找工作程序节点的子网，请运行 `ibmcloud ks workers --cluster <cluster_name>` 并将工作程序节点的 `Private IP` 与在下拉列表中找到的子网相比较。
5.  单击**提交**。
6.  单击文件存储器的名称。
7.  记录 `Mount Point`、`size` 和 `Location` 字段。`Mount Point` 字段显示为 `<nfs_server>:<file_storage_path>`。

### 步骤 2：创建持久卷 (PV) 和匹配的持久卷声明 (PVC)
{: #existing-file-2}

1.  为 PV 创建存储器配置文件。包含先前检索到的值。

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
       storage: "<size>"
     accessModes:
       - ReadWriteMany
     nfs:
       server: "<nfs_server>"
       path: "<file_storage_path>"
    ```
    {: codeblock}

    <table>
    <caption>了解 YAML 文件的组成部分</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解 YAML 文件的组成部分</th>
    </thead>
    <tbody>
    <tr>
    <td><code>name</code></td>
    <td>输入要创建的 PV 对象的名称。</td>
    </tr>
    <tr>
    <td><code>metadata.labels</code></td>
    <td>输入先前检索到的区域和专区。必须在持久性存储器所在的区域和专区中至少有一个工作程序节点，才能在集群中安装该存储器。如果存储器的 PV 已存在，请向 PV [添加专区和区域标签](/docs/containers?topic=containers-kube_concepts#storage_multizone)。
    </tr>
    <tr>
    <td><code>spec.capacity.storage</code></td>
    <td>输入先前检索到的现有 NFS 文件共享的存储器大小。输入的存储器大小必须以千兆字节为单位，例如 20Gi (20 GB) 或 1000Gi (1 TB)，并且大小必须与现有文件共享的大小相匹配。</td>
    </tr>
    <tr>
    <td><code>spec.accessMode</code></td>
    <td>指定下列选项之一：<ul><li><strong>ReadWriteMany：</strong>多个 pod 可安装 PVC。所有 pod 可对卷进行读取和写入。</li><li><strong>ReadOnlyMany：</strong>PVC 可由多个 pod 安装。所有 pod 都具有只读访问权。<li><strong>ReadWriteOnce：</strong>PVC 只能由一个 pod 安装。此 pod 可对卷进行读取和写入。</li></ul></td>
    </tr>
    <tr>
    <td><code>spec.nfs.server</code></td>
    <td>输入先前检索到的 NFS 文件共享服务器标识。</td>
    </tr>
    <tr>
    <td><code>path</code></td>
    <td>输入先前检索到的 NFS 文件共享的路径。</td>
    </tr>
    </tbody></table>

3.  在集群中创建 PV。
    

    ```
    kubectl apply -f mypv.yaml
    ```
    {: pre}

4.  验证 PV 是否已创建。

    ```
    kubectl get pv
    ```
    {: pre}

5.  创建另一个配置文件以创建 PVC。为了使 PVC 与先前创建的 PV 相匹配，必须为 `storage` 和 `accessMode` 选择相同的值。`storage-class` 字段必须为空。如果其中任何字段与 PV 不匹配，那么会[动态供应](/docs/containers?topic=containers-kube_concepts#dynamic_provisioning)新的 PV 和新的物理存储器实例。

    ```
    kind: PersistentVolumeClaim
    apiVersion: v1
    metadata:
     name: mypvc
    spec:
     accessModes:
       - ReadWriteMany
     resources:
       requests:
         storage: "<size>"
     storageClassName:
    ```
    {: codeblock}

6.  创建 PVC。

    ```
     kubectl apply -f mypvc.yaml
     ```
    {: pre}

7.  验证 PVC 是否已创建并与 PV 绑定。此过程可能需要几分钟时间。

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
    Access Modes: RWX
    Events:
      FirstSeen LastSeen Count From        SubObjectPath Type Reason Message
      --------- -------- ----- ----        ------------- -------- ------ -------
      3m 3m 1 {ibm.io/ibmc-file 31898035-3011-11e7-a6a4-7a08779efd33 } Normal Provisioning External provisioner is provisioning volume for claim "default/my-persistent-volume-claim"
      3m 1m	 10 {persistentvolume-controller } Normal ExternalProvisioning cannot find provisioner "ibm.io/ibmc-file", expecting that a volume for the claim is provisioned either manually or via external software
      1m 1m 1 {ibm.io/ibmc-file 31898035-3011-11e7-a6a4-7a08779efd33 } Normal ProvisioningSucceeded	Successfully provisioned volume pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2
    ```
    {: screen}


您已成功创建 PV，并将其绑定到 PVC。现在，集群用户可以[将 PVC 安装到其部署](#file_app_volume_mount)，并开始对 PV 对象执行读写操作。

<br />



## 在有状态集内使用文件存储器
{: #file_statefulset}

如果您有一个有状态的应用程序（如数据库），那么可以创建有状态集，以使用文件存储器来存储应用程序的数据。或者，可以使用 {{site.data.keyword.Bluemix_notm}} 数据库即服务，并将数据存储在云中。
{: shortdesc}

**向有状态集添加文件存储器时需要了解哪些内容？**</br>要将存储器添加到有状态集，请在有状态集 YAML 的 `volumeClaimTemplates` 部分中指定存储器配置。`volumeClaimTemplates` 是 PVC 的基础，可以包含要供应的文件存储器的存储类和大小或 IOPS。但是，如果要在 `volumeClaimTemplates` 中包含标签，那么 Kubernetes 在创建 PVC 时，不会包含这些标签。您必须改为将标签直接添加到有状态集。

不能同时部署两个有状态集。如果在一个有状态集完全部署之前尝试创建另一个有状态集，那么前一个有状态集的部署可能会导致意外的结果。
{: important}

**如何在特定专区中创建有状态集？**</br>在多专区集群中，可以在有状态集 YAML 的 `spec.selector.matchLabels` 和 `spec.template.metadata.labels` 部分中，指定要在其中创建有状态集的专区和区域。或者，可以将这些标签添加到[定制存储类](/docs/containers?topic=containers-kube_concepts#customized_storageclass)，并在有状态集的 `volumeClaimTemplates` 部分中使用此存储类。

**能否延迟 PV 与有状态 pod 的绑定，直到该 pod 准备就绪？**<br>
可以，您可以为 PVC [创建定制存储类](#file-topology)，其中包含 [`volumeBindingMode: WaitForFirstConsumer` ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/storage/storage-classes/#volume-binding-mode) 字段。

**向有状态集添加文件存储器时有哪些选项可用？**</br>如果要在创建有状态集时自动创建 PVC，请使用[动态供应](#file_dynamic_statefulset)。您还可以选择对有状态集[预先供应 PVC 或使用现有 PVC](#file_static_statefulset)。  

### 动态供应：创建有状态集时创建 PVC
{: #file_dynamic_statefulset}

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

    以下示例显示了如何将 NGINX 部署为具有 3 个副本的有状态集。对于每个副本，将根据 `ibmc-file-retain-bronze` 存储类中的规范供应一个 20 千兆字节的文件存储设备。所有存储设备都在 `dal10` 专区中供应。因为文件存储器无法从其他专区进行访问，所以有状态集的所有副本也将部署到位于 `dal10` 中的工作程序节点上。

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
         storageClassName: ibmc-file-retain-bronze
    ```
    {: codeblock}

  - **具有反亲缘关系规则并延迟文件存储器创建的示例有状态集：**

    以下示例显示了如何将 NGINX 部署为具有 3 个副本的有状态集。有状态集未指定在其中创建文件存储器的区域和专区。有状态集改为使用反亲缘关系规则来确保 pod 跨工作程序节点和专区分布。工作程序节点反亲缘关系通过定义 `app: nginx` 标签来实现。如果具有此标签的 pod 已在工作程序节点上运行，那么此标签指示 Kubernetes 调度程序不要将 pod 安排在此工作程序节点上。`topologykey: failure-domain.beta.kubernetes.io/zone` 标签甚至进一步限制了此反亲缘关系规则：如果工作程序节点所在专区中，有已经运行具有 `app: nginx` 标签的 pod 的工作程序节点，那么此项会阻止 pod 安排在该工作程序节点上。对于每个有状态集 pod，将创建两个 PVC，如 `volumeClaimTemplates` 部分中所定义，但会延迟创建文件存储器实例，直到安排了使用该存储器的有状态集 pod。此设置称为[拓扑感知卷安排](https://kubernetes.io/blog/2018/10/11/topology-aware-volume-provisioning-in-kubernetes/)。

    ```
    apiVersion: storage.k8s.io/v1
    kind: StorageClass
    metadata:
      name: ibmc-file-bronze-delayed
    parameters:
      billingType: hourly
      classVersion: "2"
      iopsPerGB: "2"
      sizeRange: '[20-12000]Gi'
      type: Endurance
    provisioner: ibm.io/ibmc-file
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
          - ReadWriteMany # access mode
          resources:
            requests:
              storage: 20Gi
          storageClassName: ibmc-file-bronze-delayed
      - metadata:
          name: myvol2
        spec:
          accessModes:
          - ReadWriteMany # access mode
          resources:
            requests:
              storage: 20Gi
          storageClassName: ibmc-file-bronze-delayed
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
    <td style="text-align:left">输入要用于有状态集的 pod 管理策略。在以下选项之间进行选择：<ul><li><strong><code>OrderedReady</code></strong>：使用此选项时，有状态集副本会一个接一个地进行部署。例如，如果指定了 3 个副本，那么 Kubernetes 会为第一个副本创建 PVC，然后等待 PVC 绑定后，部署该有状态集副本，接着将 PVC 安装到该副本。此部署完成后，将部署第二个副本。有关此选项的更多信息，请参阅 [`OrderedReady` pod 管理 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/#orderedready-pod-management)。</li><li><strong>并行</strong>：使用此选项时，将同时启动所有有状态集副本的部署。如果应用程序支持对副本进行并行部署，请使用此选项以节省 PVC 和有状态集副本的部署时间。</li></ul></td>
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
    <td style="text-align:left">输入文件存储器的大小，以千兆字节 (Gi) 为单位。</td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.volumeClaimTemplates.spec.resources.</code></br><code>requests.iops</code></td>
    <td style="text-align:left">如果要供应[性能存储器](#file_predefined_storageclass)，请输入 IOPS 数。如果使用耐久性存储类并指定了一些 IOPS，那么将忽略 IOPS 数，而改为使用在存储类中指定的 IOPS。</td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.volumeClaimTemplates.</code></br><code>spec.storageClassName</code></td>
    <td style="text-align:left">输入要使用的存储类。要列出现有存储类，请运行 <code>kubectl get storageclasses | grep file</code>。如果未指定存储类，那么会使用在集群中设置的缺省存储类来创建 PVC。请确保缺省存储类使用的是 <code>ibm.io/ibmc-file</code> 供应程序，以便为有状态集供应文件存储器。</td>
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
{: #file_static_statefulset}

您可以在创建有状态集之前预先供应 PVC，也可以将现有 PVC 用于有状态集。
{: shortdesc}

如果是在[创建有状态集时动态供应 PVC](#file_dynamic_statefulset)，那么 PVC 的名称将根据在有状态集 YAML 文件中使用的值来指定。为了使有状态集使用现有 PVC，PVC 的名称必须与使用动态供应时自动创建的名称相匹配。

开始之前：[登录到您的帐户。如果适用，请将相应的资源组设定为目标。为集群设置上下文。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1. 如果要在创建有状态集之前预供应 PVC，请执行[向应用程序添加文件存储器](#add_file)中的步骤 1-3，以便为每个有状态集副本创建 PVC。确保创建 PVC 时所使用的名称遵循以下格式：`<volume_name>-<statefulset_name>-<replica_number>`。
   - **`<volume_name>`**：使用要在有状态集的 `spec.volumeClaimTemplates.metadata.name` 部分中指定的名称，例如 `nginxvol`。
   - **`<statefulset_name>`**：使用要在有状态集的 `metadata.name` 部分中指定的名称，例如 `nginx_statefulset`。
   - **`<replica_number>`**：输入副本数，起始值为 0。

   例如，如果必须创建 3 个有状态集副本，请使用以下名称创建 3 个 PVC：`nginxvol-nginx_statefulset-0`、`nginxvol-nginx_statefulset-1` 和 `nginxvol-nginx_statefulset-2`。  

   想要为现有文件存储器实例创建 PVC 和 PV？请使用[静态供应](#existing_file)来创建 PVC 和 PV。
   {: tip}

2. 执行[动态供应：创建有状态集时创建 PVC](#file_dynamic_statefulset) 中的步骤来创建有状态集。PVC 的名称遵循格式 `<volume_name>-<statefulset_name>-<replica_number>`。确保在有状态集规范中使用 PVC 名称的以下值：
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
{: #file_change_storage_configuration}

如果要提高存储容量或性能，可以修改现有卷。
{: shortdesc}

有关计费的问题以及要查找如何使用 {{site.data.keyword.Bluemix_notm}} 控制台来修改存储器的步骤，请参阅[扩展文件共享容量](/docs/infrastructure/FileStorage?topic=FileStorage-expandCapacity#expandCapacity)。
{: tip}

1. 列出集群中的 PVC，并记下 **VOLUME** 列中关联 PV 的名称。
   ```
   kubectl get pvc
   ```
   {: pre}

   输出示例：
   ```
   NAME             STATUS    VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS        AGE
   myvol            Bound     pvc-01ac123a-123b-12c3-abcd-0a1234cb12d3   20Gi       RWX            ibmc-file-bronze    147d
   ```
   {: screen}

2. 通过列出 PVC 绑定到的 PV 的详细信息，检索与 PVC 关联的物理文件存储器的 **`StorageType`**、**`volumeId`** 和 **`server`**。将 `<pv_name>` 替换为在先前步骤中检索到的 PV 的名称。存储器类型、卷标识和服务器名称会显示在 CLI 输出的 **`Labels`** 部分中。
   ```
   kubectl describe pv <pv_name>
   ```
   {: pre}

   输出示例：
   ```
   Name:            pvc-4b62c704-5f77-11e8-8a75-b229c11ba64a
   Labels:          CapacityGb=20
                    Datacenter=dal10
                    Iops=2
                    StorageType=ENDURANCE
                    Username=IBM02SEV1543159_6
                    billingType=hourly
                    failure-domain.beta.kubernetes.io/region=us-south
                    failure-domain.beta.kubernetes.io/zone=dal10
                    path=IBM01SEV1234567_8ab12t
                    server=fsf-dal1001g-fz.adn.networklayer.com
                    volumeId=12345678
   ...
   ```
   {: screen}

3. 在 IBM Cloud Infrastructure (SoftLayer) 帐户中修改卷的大小或 IOPS。

   性能存储器的示例：
   ```
   ibmcloud sl file volume-modify <volume_ID> --new-size <size> --new-iops <iops>
   ```
   {: pre}

   耐久性存储器的示例：
   ```
   ibmcloud sl file volume-modify <volume_ID> --new-size <size> --new-tier <iops>
   ```
   {: pre}

   <table>
   <caption>了解命令的组成部分</caption>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解 YAML 文件的组成部分</th>
   </thead>
   <tbody>
   <tr>
   <td><code>&lt;volume_ID&gt;</code></td>
   <td>输入先前检索到的卷的标识。</td>
   </tr>
   <tr>
   <td><code>&lt;new-size&gt;</code></td>
   <td>输入卷的新大小，以千兆字节 (Gi) 为单位。有关有效的大小，请参阅[决定文件存储器配置](#file_predefined_storageclass)。输入的大小必须大于或等于卷的当前大小。如果未指定新的大小，那么将使用卷的当前大小。</td>
   </tr>
   <tr>
   <td><code>&lt;new-iops&gt;</code></td>
   <td>仅适用于性能存储器。输入所需的新 IOPS 数。有关有效的 IOPS，请参阅[决定文件存储器配置](#file_predefined_storageclass)。如果不指定 IOPS，那么将使用当前 IOPS。<p class="note">如果卷的原始 IOPS/GB 比率小于 0.3，那么新的 IOPS/GB 比率必须小于 0.3。如果卷的原始 IOP/GB 比率大于或等于 0.3，那么卷的新 IOP/GB 比率必须大于或等于 0.3。</p> </td>
   </tr>
   <tr>
   <td><code>&lt;new-tier&gt;</code></td>
   <td>仅适用于耐久性存储器。输入所需的新 IOPS/GB 数。有关有效的 IOPS，请参阅[决定文件存储器配置](#file_predefined_storageclass)。如果不指定 IOPS，那么将使用当前 IOPS。<p class="note">如果卷的原始 IOPS/GB 比率小于 0.25，那么新的 IOPS/GB 比率必须小于 0.25。如果卷的原始 IOP/GB 比率大于或等于 0.25，那么卷的新 IOP/GB 比率必须大于或等于 0.25。</p> </td>
   </tr>
   </tbody>
   </table>

   输出示例：
   ```
   Order 31020713 was placed successfully!.
   > Storage as a Service

   > 40 GBs

   > 2 IOPS per GB

   > 20 GB Storage Space (Snapshot Space)

   You may run 'ibmcloud sl file volume-list --order 12345667' to find this file volume after it is ready.
   ```
   {: screen}

4. 如果更改了卷的大小并且是在 pod 中使用该卷，请登录到 pod 以验证新大小。
   1. 列出使用 PVC 的所有 pod。
      ```
      kubectl get pods --all-namespaces -o=jsonpath='{range .items[*]}{"\n"}{.metadata.name}{":\t"}{range .spec.volumes[*]}{.persistentVolumeClaim.claimName}{" "}{end}{end}' | grep "<pvc_name>"
      ```
      {: pre}

      将按以下格式返回 pod：`<pod_name>: <pvc_name>`。
   2. 登录到 pod。
      ```
      kubectl exec -it <pod_name> bash
      ```
      {: pre}

   3. 显示磁盘使用情况统计信息，并查找先前检索到的卷的服务器路径。
      ```
      df -h
      ```
      {: pre}

      输出示例：
      ```
      Filesystem                                                      Size  Used Avail Use% Mounted on
      overlay                                                          99G  4.8G   89G   6% /
      tmpfs                                                            64M     0   64M   0% /dev
      tmpfs                                                           7.9G     0  7.9G   0% /sys/fs/cgroup
      fsf-dal1001g-fz.adn.networklayer.com:/IBM01SEV1234567_6/data01   40G     0   40G   0% /myvol
      ```
      {: screen}


## 更改缺省 NFS 版本
{: #nfs_version}

文件存储器的版本确定了用于与 {{site.data.keyword.Bluemix_notm}} 文件存储服务器通信的协议。缺省情况下，所有文件存储器实例都设置为使用 NFS V4。如果应用程序需要特定版本才能正常运行，那么可以将现有 PV 更改为较旧的 NFS 版本。
{: shortdesc}

要更改缺省 NFS 版本，您可以创建新的存储类以在集群中动态供应文件存储器，或者选择更改已安装到 pod 的现有 PV。

要应用最新的安全性更新并提高性能，请使用缺省 NFS 版本，而不要更改为较旧的 NFS 版本。
{: important}

**要使用特定 NFS 版本来创建定制存储类，请执行以下操作：**
1. 使用要供应的 NFS 版本创建[定制存储类](#nfs_version_class)。
2. 在集群中创建存储类。
   ```
   kubectl apply -f nfsversion_storageclass.yaml
   ```
   {: pre}

3. 验证是否已创建定制存储类。
   ```
    kubectl get storageclasses
    ```
   {: pre}

4. 向[文件存储器](#add_file)供应定制存储类。

**要将现有 PV 更改为使用其他 NFS 版本，请执行以下操作：**

1. 获取要在其中更改 NFS 版本的文件存储器的 PV，并记下该 PV 的名称。
   ```
    kubectl get pv
    ```
   {: pre}

2. 向 PV 添加注释。将 `<version_number>` 替换为您要使用的 NFS 版本。例如，要更改为 NFS V3.0，请输入 **3**。  
   ```
   kubectl patch pv <pv_name> -p '{"metadata": {"annotations":{"volume.beta.kubernetes.io/mount-options":"vers=<version_number>"}}}'
   ```
   {: pre}

3. 删除使用文件存储器的 pod，然后重新创建 pod。
   1. 将 pod YAML 保存到本地计算机。
      ```
      kubect get pod <pod_name> -o yaml > <filepath/pod.yaml>
      ```
      {: pre}

   2. 删除 pod。
      ```
      kubectl deleted pod <pod_name>
      ```
      {: pre}

   3. 重新创建 pod。
      ```
      kubectl apply -f pod.yaml
      ```
      {: pre}

4. 等待 pod 部署。
   ```
   kubectl get pods
   ```
   {: pre}

   状态更改为 `Running` 时，说明 pod 已完全部署。

5. 登录到 pod。
   ```
   kubectl exec -it <pod_name> sh
   ```
   {: pre}

6. 验证文件存储器是否已安装了您先前指定的 NFS 版本。
   ```
   mount | grep "nfs" | awk -F" |," '{ print $5, $8 }'
   ```
   {: pre}

   输出示例：
   ```
   nfs vers=3.0
   ```
   {: screen}

<br />


## 备份和复原数据
{: #file_backup_restore}

文件存储器已供应到集群中的工作程序节点所在的位置。存储器由 IBM 在集群服务器上托管，以在其中某个服务器停止运行时提供可用性。但是，文件存储器不会自动进行备份，因此在整个位置发生故障时可能无法进行访问。为了防止数据丢失或损坏，可以设置定期备份，以便在需要时可用于复原数据。
{: shortdesc}

查看文件存储器的以下备份和复原选项：

<dl>
  <dt>设置定期快照</dt>
  <dd><p>可以[为文件存储器设置定期快照](/docs/infrastructure/FileStorage?topic=FileStorage-snapshots)，这是捕获某个时间点的实例状态的只读映像。要存储快照，必须在文件存储器上请求快照空间。快照会存储在同一专区的现有存储器实例上。如果用户不小心除去了卷中的重要数据，那么可以通过快照来复原数据。</br> <strong>要为卷创建快照，请执行以下操作：</strong><ol><li>[登录到您的帐户。如果适用，请将相应的资源组设定为目标。为集群设置上下文。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)</li><li>登录到 `ibmcloud sl` CLI。<pre class="pre"><code>ibmcloud sl init</code></pre></li><li>列出集群中的现有 PV。<pre class="pre"><code>kubectl get pv</code></pre></li><li>获取要为其创建快照空间的 PV 的详细信息，并记下卷标识、大小和 IOPS。<pre class="pre"><code>kubectl describe pv &lt;pv_name&gt;</code></pre> 可以在 CLI 输出的 <strong>Labels</strong> 部分中找到卷标识、大小和 IOPS。</li><li>使用您在先前步骤中检索到的参数为现有卷创建快照大小。<pre class="pre"><code>ibmcloud sl file snapshot-order &lt;volume_ID&gt; --size &lt;size&gt; --tier &lt;iops&gt;</code></pre></li><li>等待快照大小创建。<pre class="pre"><code>ibmcloud sl file volume-detail &lt;volume_ID&gt;</code></pre>CLI 输出中的 <strong>Snapshot Size (GB)</strong> 从 0 更改为您所订购的大小时，说明已成功供应快照大小。</li><li>为卷创建快照，并记下创建的快照的标识。<pre class="pre"><code>ibmcloud sl file snapshot-create &lt;volume_ID&gt;</code></pre></li><li>验证快照是否已成功创建。<pre class="pre"><code>ibmcloud sl file snapshot-list &lt;volume_ID&gt;</code></pre></li></ol></br><strong>要将数据从快照复原到现有卷，请运行以下命令：</strong><pre class="pre"><code>ibmcloud sl file snapshot-restore &lt;volume_ID&gt; &lt;snapshot_ID&gt;</code></pre></p></dd>
  <dt>将快照复制到其他专区</dt>
 <dd><p>为了保护数据不受专区故障的影响，可以[复制快照](/docs/infrastructure/FileStorage?topic=FileStorage-replication#replication)到其他专区中设置的文件存储器实例。数据只能从主存储器复制到备份存储器。不能将复制的文件存储器实例安装到集群。主存储器发生故障时，可以手动将复制的备份存储器设置为主存储器。然后，可以将其安装到集群。复原主存储器后，可以从备份存储器复原数据。</p></dd>
 <dt>复制存储器</dt>
 <dd><p>可以在原始存储器实例所在的专区中，[复制文件存储器实例](/docs/infrastructure/FileStorage?topic=FileStorage-duplicatevolume#duplicatevolume)。复制项所含的数据是其创建的那个时间点上原始存储器实例的数据。与副本不同，复制项用作独立于原始项的存储器实例。要进行复制，请首先[为卷设置快照](/docs/infrastructure/FileStorage?topic=FileStorage-snapshots)。</p></dd>
  <dt>将数据备份到 {{site.data.keyword.cos_full}}</dt>
  <dd><p>可以使用 [**ibm-backup-restore 映像**](/docs/services/RegistryImages/ibm-backup-restore?topic=RegistryImages-ibmbackup_restore_starter#ibmbackup_restore_starter)来启动集群中的备份和复原 pod。此 pod 包含一个脚本，用于对集群中的任何持久卷声明 (PVC) 运行一次性或定期备份。数据存储在您在某个专区设置的 {{site.data.keyword.cos_full}} 实例中。</p>
  <p>要使数据具有更高可用性，并保护应用程序不受专区故障的影响，请设置第二个 {{site.data.keyword.cos_full}} 实例，并在各个专区之间复制数据。如果需要从 {{site.data.keyword.cos_full}} 实例复原数据，请使用随映像一起提供的复原脚本。</p></dd>
<dt>将数据复制到 pod 和容器，以及从 pod 和容器中复制数据</dt>
<dd><p>可以使用 `kubectl cp` [命令 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/reference/kubectl/overview/#cp) 将文件和目录复制到集群中的 pod 或特定容器，或者从集群中的 pod 或特定容器中复制文件和目录。</p>
<p>开始之前：[登录到您的帐户。如果适用，请将相应的资源组设定为目标。为集群设置上下文。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure) 如果未使用 <code>-c</code> 来指定容器，那么此命令将使用 pod 中的第一个可用容器。</p>
<p>可以通过以下各种方法来使用此命令：</p>
<ul>
<li>将本地机器中的数据复制到集群中的 pod：<pre class="pre"><code>kubectl cp <var>&lt;local_filepath&gt;/&lt;filename&gt;</var> <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;</var></code></pre></li>
<li>将集群的 pod 中的数据复制到本地机器：<pre class="pre"><code>kubectl cp <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;/&lt;filename&gt;</var> <var>&lt;local_filepath&gt;/&lt;filename&gt;</var></code></pre></li>
<li>将数据从本地计算机复制到在集群中的 pod 中运行的特定容器：<pre class="pre"><code>kubectl cp <var>&lt;local_filepath&gt;/&lt;filename&gt;</var> <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;</var> -c <var>&lt;container></var></code></pre></li>
</ul></dd>
  </dl>

<br />


## 存储类参考
{: #file_storageclass_reference}

### 铜牌级
{: #file_bronze}

<table>
<caption>文件存储类：铜牌级</caption>
<thead>
<th>特征</th>
<th>设置</th>
</thead>
<tbody>
<tr>
<td>名称</td>
<td><code>ibmc-file-bronze</code></br><code>ibmc-file-retain-bronze</code></td>
</tr>
<tr>
<td>类型</td>
<td>[耐久性存储器](/docs/infrastructure/FileStorage?topic=FileStorage-about#provisioning-with-endurance-tiers)</td>
</tr>
<tr>
<td>文件系统</td>
<td>NFS</td>
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
<td>按小时</td>
</tr>
<tr>
<td>定价</td>
<td>[定价信息 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/cloud/file-storage/pricing)</td>
</tr>
</tbody>
</table>


### 银牌级
{: #file_silver}

<table>
<caption>文件存储类：银牌级</caption>
<thead>
<th>特征</th>
<th>设置</th>
</thead>
<tbody>
<tr>
<td>名称</td>
<td><code>ibmc-file-silver</code></br><code>ibmc-file-retain-silver</code></td>
</tr>
<tr>
<td>类型</td>
<td>[耐久性存储器](/docs/infrastructure/FileStorage?topic=FileStorage-about#provisioning-with-endurance-tiers)</td>
</tr>
<tr>
<td>文件系统</td>
<td>NFS</td>
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
<td>按小时</li></ul></td>
</tr>
<tr>
<td>定价</td>
<td>[定价信息 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/cloud/file-storage/pricing)</td>
</tr>
</tbody>
</table>

### 金牌级
{: #file_gold}

<table>
<caption>文件存储类：金牌级</caption>
<thead>
<th>特征</th>
<th>设置</th>
</thead>
<tbody>
<tr>
<td>名称</td>
<td><code>ibmc-file-gold</code></br><code>ibmc-file-retain-gold</code></td>
</tr>
<tr>
<td>类型</td>
<td>[耐久性存储器](/docs/infrastructure/FileStorage?topic=FileStorage-about#provisioning-with-endurance-tiers)</td>
</tr>
<tr>
<td>文件系统</td>
<td>NFS</td>
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
<td>按小时</li></ul></td>
</tr>
<tr>
<td>定价</td>
<td>[定价信息 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/cloud/file-storage/pricing)</td>
</tr>
</tbody>
</table>

### 定制
{: #file_custom}

<table>
<caption>文件存储类：定制</caption>
<thead>
<th>特征</th>
<th>设置</th>
</thead>
<tbody>
<tr>
<td>名称</td>
<td><code>ibmc-file-custom</code></br><code>ibmc-file-retain-custom</code></td>
</tr>
<tr>
<td>类型</td>
<td>[性能](/docs/infrastructure/FileStorage?topic=FileStorage-about#provisioning-with-performance)</td>
</tr>
<tr>
<td>文件系统</td>
<td>NFS</td>
</tr>
<tr>
<td>IOPS 和大小</td>
<td><p><strong>大小范围（以千兆字节计）/ IOPS 范围（100 的倍数）</strong></p><ul><li>20-39 Gi / 100-1000 IOPS</li><li>40-79 Gi / 100-2000 IOPS</li><li>80-99 Gi / 100-4000 IOPS</li><li>100-499 Gi / 100-6000 IOPS</li><li>500-999 Gi / 100-10000 IOPS</li><li>1000-1999 Gi / 100-20000 IOPS</li><li>2000-2999 Gi / 200-40000 IOPS</li><li>3000-3999 Gi / 200-48000 IOPS</li><li>4000-7999 Gi / 300-48000 IOPS</li><li>8000-9999 Gi / 500-48000 IOPS</li><li>10000-12000 Gi / 1000-48000 IOPS</li></ul></td>
</tr>
<tr>
<td>硬盘</td>
<td>IOPS 与千兆字节的比率确定供应的硬盘类型。要确定 IOPS 与千兆字节的比率，请将 IOPS 除以存储器的大小。</br></br>示例：</br>对于 100 IOPS，您选择了 500 Gi 存储器。因此，比率为 0.2 (100 IOPS/500 Gi)。</br></br><strong>每个比率的硬盘类型概述：</strong><ul><li>小于或等于 0.3：SATA</li><li>大于 0.3：SSD</li></ul></td>
</tr>
<tr>
<td>计费</td>
<td>按小时</li></ul></td>
</tr>
<tr>
<td>定价</td>
<td>[定价信息 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/cloud/file-storage/pricing)</td>
</tr>
</tbody>
</table>

<br />


## 样本定制存储类
{: #file_custom_storageclass}

您可以创建定制存储类并在 PVC 中使用该存储类。
{: shortdesc}

{{site.data.keyword.containerlong_notm}} 提供了[预定义的存储类](#file_storageclass_reference)，用于供应具有特定层和配置的文件存储器。在某些情况下，您可能希望使用预定义的存储类中未涵盖的其他配置来供应存储器。您可以使用本主题中的示例来找到样本定制存储类。

要创建定制存储类，请参阅[定制存储类](/docs/containers?topic=containers-kube_concepts#customized_storageclass)。然后查看[在 PVC 中使用定制存储类](#add_file)。

### 创建拓扑感知存储器
{: #file-topology}

要在多专区集群中使用文件存储器，必须将 pod 安排在文件存储器实例所在的专区中，以便您可以读取和写入卷。在 Kubernetes 引入拓扑感知卷安排之前，创建 PVC 时，存储器的动态供应自动创建了文件存储器实例。然后，在创建 pod 时，Kubernetes 调度程序会尝试将 pod 部署到文件存储器实例所在的数据中心内的工作程序节点。
{: shortdesc}

在不了解 pod 约束的情况下创建文件存储器实例可能会导致意外结果。例如，可能会由于存储器所在的工作程序节点的资源不足，或者该工作程序节点有污点而不允许安排 pod，导致您的 pod 无法安排到该工作程序节点。使用拓扑感知卷安排时，会延迟文件存储器实例创建，直到创建了使用该存储器的第一个 pod。

只有运行 Kubernetes V1.12 或更高版本的集群上支持拓扑感知卷安排。
{: note}

以下示例显示了如何创建存储类以用于延迟文件存储器实例创建，直到使用此存储器的第一个 pod 准备就绪可安排为止。要延迟创建，必须包含 `volumeBindingMode: WaitForFirstConsumer` 选项。如果不包含此选项，那么 `volumeBindingMode` 会自动设置为 `Immediate`，并将在创建 PVC 时创建文件存储器实例。

- **耐久性文件存储器的示例：**
  ```
  apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
    name: ibmc-file-bronze-delayed
  parameters:
    billingType: hourly
    classVersion: "2"
    iopsPerGB: "2"
    sizeRange: '[20-12000]Gi'
    type: Endurance
  provisioner: ibm.io/ibmc-file
  reclaimPolicy: Delete
  volumeBindingMode: WaitForFirstConsumer
  ```
  {: codeblock}

- **性能文件存储器的示例：**
  ```
  apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
   name: ibmc-file-performance-storageclass
   labels:
     kubernetes.io/cluster-service: "true"
  provisioner: ibm.io/ibmc-file
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

### 为多专区集群指定专区
{: #file_multizone_yaml}

如果要在特定专区中创建文件存储器，可以在定制存储类中指定专区和区域。
{: shortdesc}

如果要在特定专区中[静态供应文件存储器](#existing_file)，请使用定制存储类。在其他所有情况下，请[直接在 PVC 中指定专区](#add_file)。
{: note}

创建定制存储类时，请指定集群和工作程序节点所在的区域和专区。要获取集群的区域，请运行 `ibmcloud ks cluster-get --cluster <cluster_name_or_ID>` 并在 **Master URL** 中查找区域前缀，例如 `https://c2.eu-de.containers.cloud.ibm.com:11111` 中的 `eu-de`。要获取工作程序节点的专区，请运行 `ibmcloud ks workers --cluster <cluster_name_or_ID>`。

- **耐久性文件存储器的示例：**
  ```
  apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
    name: ibmc-file-silver-mycustom-storageclass
    labels:
      kubernetes.io/cluster-service: "true"
  provisioner: ibm.io/ibmc-file
  parameters:
    zone: "dal12"
    region: "us-south"
    type: "Endurance"
    iopsPerGB: "4"
    sizeRange: "[20-12000]Gi"
    reclaimPolicy: "Delete"
    classVersion: "2"
  reclaimPolicy: Delete
  volumeBindingMode: Immediate
  ```
  {: codeblock}

- **性能文件存储器的示例：**
  ```
  apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
   name: ibmc-file-performance-storageclass
   labels:
     kubernetes.io/cluster-service: "true"
  provisioner: ibm.io/ibmc-file
  parameters:
   zone: "dal12"
   region: "us-south"
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
  volumeBindingMode: Immediate
  ```
  {: codeblock}

### 更改缺省 NFS 版本
{: #nfs_version_class}

以下定制存储类允许您定义要供应的 NFS 版本。例如，要供应 NFS V3.0，请将 `<nfs_version>` 替换为 **3.0**。
{: shortdesc}

- **耐久性文件存储器的示例：**
  ```
  apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
    name: ibmc-file-mount
    labels:
      kubernetes.io/cluster-service: "true"
  provisioner: ibm.io/ibmc-file
  parameters:
    type: "Endurance"
    iopsPerGB: "2"
    sizeRange: "[1-12000]Gi"
    reclaimPolicy: "Delete"
    classVersion: "2"
    mountOptions: nfsvers=<nfs_version>
  ```
  {: codeblock}

- **性能文件存储器的示例：**
  ```
  apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
    name: ibmc-file-mount
    labels:
      kubernetes.io/cluster-service: "true"
  provisioner: ibm.io/ibmc-file
  parameters:
   type: "Performance"
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
   mountOptions: nfsvers=<nfs_version>
  ```
  {: codeblock}
