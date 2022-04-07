---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: kubernetes, iks, local persistent storage

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



# IBM Cloud 存储实用程序
{: #utilities}

## 安装 IBM Cloud Block Storage Attacher 插件 (beta)
{: #block_storage_attacher}

使用 {{site.data.keyword.Bluemix_notm}} Block Storage Attacher 插件，可以将未格式化且未安装的原始块存储器连接到集群中的工作程序节点。
  
{: shortdesc}

例如，您想要使用软件定义的存储 (SDS) 解决方案（如 [Portworx](/docs/containers?topic=containers-portworx)）来存储数据，但不想使用已针对 SDS 使用情况进行优化并随附额外本地磁盘的裸机工作程序节点。要向非 SDS 工作程序节点添加本地磁盘，必须在 {{site.data.keyword.Bluemix_notm}} 基础架构帐户中手动创建块存储设备，然后使用 {{site.data.keyword.Bluemix_notm}} Block Volume Attacher 将该存储器连接到非 SDS 工作程序节点。

{{site.data.keyword.Bluemix_notm}} Block Volume Attachcher 插件会在集群中的每个工作程序节点上创建 pod 以作为守护程序集的一部分，还会设置一个 Kubernetes 存储类，稍后您需要使用此存储类将块存储设备连接到非 SDS 工作程序节点。

要了解有关如何更新或除去 {{site.data.keyword.Bluemix_notm}} Block Volume Attacher 插件的指示信息吗？请参阅[更新插件](#update_block_attacher)和[除去插件](#remove_block_attacher)。
{: tip}

1.  [按照指示信息](/docs/containers?topic=containers-helm#public_helm_install)在本地计算机上安装 Helm 客户机，然后使用服务帐户在集群中安装 Helm 服务器 (Tiller)。

2.  验证是否已使用服务帐户安装 Tiller。

    ```
    kubectl get serviceaccount -n kube-system | grep tiller
    ```
    {: pre}

    输出示例：

    ```
    NAME                                 SECRETS   AGE
    tiller                               1         2m
    ```
    {: screen}

3. 更新 Helm 存储库以在此存储库中检索所有最新版本的 Helm chart。
   ```
        helm repo update
        ```
   {: pre}

4. 安装 {{site.data.keyword.Bluemix_notm}} Block Volume Attacher 插件。安装该插件时，会将预定义的块存储类添加到集群中。
   ```
   helm install iks-charts/ibm-block-storage-attacher --name block-attacher
   ```
   {: pre}

   输出示例：
   ```
   NAME:   block-volume-attacher
   LAST DEPLOYED: Thu Sep 13 22:48:18 2018
   NAMESPACE: default
   STATUS: DEPLOYED

   RESOURCES:
   ==> v1beta1/ClusterRoleBinding
   NAME                             AGE
   ibmcloud-block-storage-attacher  1s

   ==> v1beta1/DaemonSet
   NAME                             DESIRED  CURRENT  READY  UP-TO-DATE  AVAILABLE  NODE SELECTOR  AGE
   ibmcloud-block-storage-attacher  0        0        0      0           0          <none>         1s

   ==> v1/StorageClass
   NAME                 PROVISIONER                AGE
   ibmc-block-attacher  ibm.io/ibmc-blockattacher  1s

   ==> v1/ServiceAccount
   NAME                             SECRETS  AGE
   ibmcloud-block-storage-attacher  1        1s

   ==> v1beta1/ClusterRole
   NAME                             AGE
   ibmcloud-block-storage-attacher  1s

   NOTES:
   Thank you for installing: ibmcloud-block-storage-attacher.   Your release is named: block-volume-attacher

   Please refer Chart README.md file for attaching a block storage
   Please refer Chart RELEASE.md to see the release details/fixes
   ```
   {: screen}

5. 验证 {{site.data.keyword.Bluemix_notm}} Block Volume Attacher 守护程序集是否已成功安装。
   ```
   kubectl get pod -n kube-system -o wide | grep attacher
   ```
   {: pre}

   输出示例：
   ```
   ibmcloud-block-storage-attacher-z7cv6           1/1       Running            0          19m
   ```
   {: screen}

   如果看到一个或多个 **ibmcloud-block-storage-attacher** pod，那么表明安装成功。pod 数等于集群中的工作程序节点数。所有 pod 都必须处于 **Running** 状态。

6. 验证 {{site.data.keyword.Bluemix_notm}} Block Volume Attacher 的存储类是否已成功创建。
   ```
   kubectl get storageclasses | grep attacher
   ```
   {: pre}

   输出示例：
   ```
   ibmc-block-attacher       ibm.io/ibmc-blockattacher   11m
   ```
   {: screen}

### 更新 IBM Cloud Block Storage Attacher 插件
{: #update_block_attacher}

您可以将现有 {{site.data.keyword.Bluemix_notm}} Block Storage Attacher 插件升级到最新版本。
{: shortdesc}

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

3. 查找 {{site.data.keyword.Bluemix_notm}} Block Storage Attacher 插件的 Helm chart 名称。
   ```
   helm ls | grep ibm-block-storage-attacher
   ```
   {: pre}

   输出示例：
   ```
   <helm_chart_name>	1       	Wed Aug  1 14:55:15 2018	DEPLOYED	ibm-block-storage-attacher-1.0.0	default
   ```
   {: screen}

4. 将 {{site.data.keyword.Bluemix_notm}} Block Storage Attacher 升级到最新版本。
   ```
   helm upgrade --force --recreate-pods <helm_chart_name> ibm-block-storage-attacher
   ```
   {: pre}

### 除去 IBM Cloud Block Volume Attacher 插件
{: #remove_block_attacher}

如果不想在集群中供应和使用 {{site.data.keyword.Bluemix_notm}} Block Storage Attacher 插件，那么可以卸载 Helm chart。
{: shortdesc}

1. 查找 {{site.data.keyword.Bluemix_notm}} Block Storage Attacher 插件的 Helm chart 名称。
   ```
   helm ls | grep ibm-block-storage-attacher
   ```
   {: pre}

   输出示例：
   ```
   <helm_chart_name>	1       	Wed Aug  1 14:55:15 2018	DEPLOYED	ibm-block-storage-attacher-1.0.0	default
   ```
   {: screen}

2. 通过除去 Helm chart 来删除 {{site.data.keyword.Bluemix_notm}} Block Storage Attacher 插件。
   ```
   helm delete <helm_chart_name> --purge
   ```
   {: pre}

3. 验证 {{site.data.keyword.Bluemix_notm}} Block Storage Attacher 插件 pod 是否已除去。
   ```
   kubectl get pod -n kube-system -o wide | grep attacher
   ```
   {: pre}

   如果 CLI 输出中未显示任何 pod，那么表明已成功除去 pod。

4. 验证 {{site.data.keyword.Bluemix_notm}} Block Storage Attacher 存储类是否已除去。
   ```
   kubectl get storageclasses | grep attacher
   ```
   {: pre}

   如果 CLI 输出中未显示任何存储类，那么表明已成功除去存储类。

## 自动供应未格式化的块存储器并授权工作程序节点访问该存储器
{: #automatic_block}

您可以使用 {{site.data.keyword.Bluemix_notm}} Block Volume Attacher 插件将具有相同配置、未格式化且未安装的原始块存储器添加到集群中的所有工作程序节点。
{: shortdesc}

{{site.data.keyword.Bluemix_notm}} Block Volume Attacher 插件中包含 `mkpvyaml` 容器，其配置为通过运行脚本来查找集群中的所有工作程序节点，在 {{site.data.keyword.Bluemix_notm}} 基础架构门户网站中创建原始块存储器，然后授权工作程序节点访问该存储器。

要添加不同的块存储器配置，将块存储器仅添加到一部分的工作程序节点，或者要拥有对供应过程的更多控制权，请选择[手动添加块存储器](#manual_block)。
{: tip}


1. 登录到 {{site.data.keyword.Bluemix_notm}}，然后指定集群所在的目标资源组。
   ```
   ibmcloud login
   ```
   {: pre}

2.  克隆 {{site.data.keyword.Bluemix_notm}} 存储实用程序存储库。
    ```
    git clone https://github.com/IBM/ibmcloud-storage-utilities.git
    ```
    {: pre}

3. 导航至 `block-storage-utilities` 目录。
   ```
   cd ibmcloud-storage-utilities/block-storage-provisioner
   ```
   {: pre}

4. 打开 `yamlgen.yaml` 文件，然后指定要添加到集群中每个工作程序节点的块存储器配置。
   ```
   #
   # Can only specify 'performance' OR 'endurance' and associated clause
   #
   cluster:  <cluster_name>    #  Enter the name of your cluster
   region:   <region>          #  Enter the IBM Cloud Kubernetes Service region where you created your cluster
   type:  <storage_type>       #  Enter the type of storage that you want to provision. Choose between "performance" or "endurance"
   offering: storage_as_a_service
   # performance:
   #    - iops:  <iops>
   endurance:
     - tier:  2                #   [0.25|2|4|10]
   size:  [ 20 ]               #   Enter the size of your storage in gigabytes
   ```
   {: codeblock}

   <table>
   <caption>了解 YAML 文件的组成部分</caption>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解 YAML 文件的组成部分</th>
   </thead>
   <tbody>
   <tr>
   <td><code>cluster</code></td>
   <td>输入要在其中添加原始块存储器的集群的名称。</td>
   </tr>
   <tr>
   <td><code>region</code></td>
   <td>输入在其中创建了集群的 {{site.data.keyword.containerlong_notm}} 区域。要查找集群的区域，可以运行 <code>[bxcs] cluster-get --cluster &lt;cluster_name_or_ID&gt;</code>。</td>
   </tr>
   <tr>
   <td><code>type</code></td>
   <td>选择要供应的存储器的类型。可以选择 <code>performance</code> 或 <code>endurance</code>。有关更多信息，请参阅[决定块存储器配置](/docs/containers?topic=containers-block_storage#block_predefined_storageclass)。</td>
   </tr>
   <tr>
   <td><code>performance.iops</code></td>
   <td>如果要供应 `performance` 存储器，请输入 IOPS 数。有关更多信息，请参阅[决定块存储器配置](/docs/containers?topic=containers-block_storage#block_predefined_storageclass)。如果要供应 `endurance` 存储器，请除去此部分，或通过在每行开头添加 `#` 来注释掉此部分。
   </tr>
   <tr>
   <td><code>endurance.tier</code></td>
   <td>如果要供应 `endurance` 存储器，请输入 IOPS 数/千兆字节。例如，如果要根据 `ibmc-block-bronze` 存储类中的定义来供应块存储器，请输入 2。有关更多信息，请参阅[决定块存储器配置](/docs/containers?topic=containers-block_storage#block_predefined_storageclass)。如果要供应 `performance` 存储器，请除去此部分，或通过在每行开头添加 `#` 来注释掉此部分。</td>
   </tr>
   <tr>
   <td><code>size</code></td>
   <td>输入存储器的大小（以千兆字节为单位）。要查找存储层支持的大小，请参阅[决定块存储器配置](/docs/containers?topic=containers-block_storage#block_predefined_storageclass)。</td>
   </tr>
   </tbody>
   </table>  

5. 检索 IBM Cloud Infrastructure (SoftLayer) 用户名和 API 密钥。`mkpvyaml` 脚本会使用该用户名和 API 密钥来访问集群。
   1. 登录到 [{{site.data.keyword.Bluemix_notm}} 控制台 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/)。
   2. 在菜单 ![“菜单”图标](../icons/icon_hamburger.svg "“菜单”图标") 中，选择**基础架构**。
   3. 在菜单栏中，选择**帐户** > **用户** > **用户列表**。
   4. 查找要检索其用户名和 API 密钥的用户。
   5. 单击**生成**以生成 API 密钥，或单击**查看**以查看现有 API 密钥。这将打开一个弹出窗口，其中显示基础架构用户名和 API 密钥。

6. 将凭证存储到环境变量中。
   1. 添加环境变量。
      ```
      export SL_USERNAME=<infrastructure_username>
      ```
      {: pre}

      ```
      export SL_API_KEY=<infrastructure_api_key>
      ```
      {: pre}

   2. 验证环境变量是否已成功创建。
      ```
        printenv
        ```
      {: pre}

7.  构建并运行 `mkpvyaml` 容器。通过映像运行容器时，会执行 `mkpvyaml.py` 脚本。该脚本会将块存储设备添加到集群中的每个工作程序节点，并授权每个工作程序节点访问该块存储设备。在脚本末尾，将为您生成 `pv-<cluster_name>.yaml` YAML 文件，以供您稍后用于在集群中创建持久卷。
    1.  构建 `mkpvyaml` 容器。
        ```
        docker build -t mkpvyaml .
        ```
        {: pre}
        输出示例：
        ```
        Sending build context to Docker daemon   29.7kB
        Step 1/16 : FROM ubuntu:18.10
        18.10: Pulling from library/ubuntu
        5940862bcfcd: Pull complete
        a496d03c4a24: Pull complete
        5d5e0ccd5d0c: Pull complete
        ba24b170ddf1: Pull complete
        Digest: sha256:20b5d52b03712e2ba8819eb53be07612c67bb87560f121cc195af27208da10e0
        Status: Downloaded newer image for ubuntu:18.10
         ---> 0bfd76efee03
        Step 2/16 : RUN apt-get update
         ---> Running in 85cedae315ce
        Get:1 http://security.ubuntu.com/ubuntu cosmic-security InRelease [83.2 kB]
        Get:2 http://archive.ubuntu.com/ubuntu cosmic InRelease [242 kB]
        ...
        Step 16/16 : ENTRYPOINT [ "/docker-entrypoint.sh" ]
         ---> Running in 9a6842f3dbe3
        Removing intermediate container 9a6842f3dbe3
         ---> 7926f5384fc7
        Successfully built 7926f5384fc7
        Successfully tagged mkpvyaml:latest
        ```
        {: screen}
    2.  运行容器以执行 `mkpvyaml.py` 脚本。
        ```
        docker run --rm -v `pwd`:/data -v ~/.bluemix:/config -e SL_API_KEY=$SL_API_KEY -e SL_USERNAME=$SL_USERNAME mkpvyaml
        ```
        {: pre}

        输出示例：
        ```
        Unable to find image 'portworx/iks-mkpvyaml:latest' locally
        latest: Pulling from portworx/iks-mkpvyaml
        72f45ff89b78: Already exists
        9f034a33b165: Already exists
        386fee7ab4d3: Already exists
        f941b4ac6aa8: Already exists
        fe93e194fcda: Pull complete
        f29a13da1c0a: Pull complete
        41d6e46c1515: Pull complete
        e89af7a21257: Pull complete
        b8a7a212d72e: Pull complete
        5e07391a6f39: Pull complete
        51539879626c: Pull complete
        cdbc4e813dcb: Pull complete
        6cc28f4142cf: Pull complete
        45bbaad87b7c: Pull complete
        05b0c8595749: Pull complete
        Digest: sha256:43ac58a8e951994e65f89ed997c173ede1fa26fb41e5e764edfd3fc12daf0120
        Status: Downloaded newer image for portworx/iks-mkpvyaml:latest

        kube-dal10-abc1d23e123456ac8b12a3c1e12b123a4 10.XXX.XX.XX
        Creating Vol of size 20 with type:  endurance
                  Ordering block storage of size: 20 for host: kube-dal10-abc1d23e123456ac8b12a3c1e12b123a4
                  ORDER ID =  30087291
        kube-dal10-abc1d23e123456ac8b12a3c1e12b123a4 10.XXX.XX.XX
        Creating Vol of size 20 with type:  endurance
                  Ordering block storage of size: 20 for host: kube-dal10-abc1d23e123456ac8b12a3c1e12b123a4
                  ORDER ID =  30087293
        kube-dal12-abc1d23e123456ac8b12a3c1e12b456v1 10.XXX.XX.XX
        Creating Vol of size 20 with type:  endurance
                  Ordering block storage of size: 20 for host: kube-dal12-abc1d23e123456ac8b12a3c1e12b456v1
                  ORDER ID =  30085655
                  No volume yet ... for orderID :  30087291
                  No volume yet ... for orderID :  30087291
                  No volume yet ... for orderID :  30087291
                  No volume yet ... for orderID :  30087291
                  Order ID =  30087291 has created VolId =  12345678
                  Granting access to volume: 12345678 for HostIP: 10.XXX.XX.XX
                  Order ID =  30087293 has created VolId =  87654321
                  Granting access to volume: 87654321 for HostIP: 10.XXX.XX.XX
                  Vol 53002831 is not yet ready ...
                  Granting access to volume: 87654321 for HostIP: 10.XXX.XX.XX
                  Order ID =  30085655 has created VolId =  98712345
                  Granting access to volume: 98712345 for HostIP: 10.XXX.XX.XX
        Output file created as :  pv-<cluster_name>.yaml
        ```
        {: screen}

7. [将块存储设备连接到工作程序节点](#attach_block)。

## 手动将块存储器添加到特定工作程序节点
{: #manual_block}

要添加不同的块存储器配置，将块存储器仅添加到一部分的工作程序节点，或者要拥有对供应过程的更多控制权，请使用此选项。
{: shortdesc}

1. 列出集群中的工作程序节点，并记下要在其中添加块存储设备的非 SDS 工作程序节点的专用 IP 地址和专区。
   ```
   ibmcloud ks workers --cluster <cluster_name_or_ID>
   ```
   {: pre}

2. 查看[决定块存储器配置](/docs/containers?topic=containers-block_storage#block_predefined_storageclass)中的步骤 3 和 4，以选择要添加到非 SDS 工作程序节点的块存储设备的类型、大小和 IOPS 数。    

3. 在非 SDS 工作程序节点所在的专区中创建块存储设备。

   **供应 20 GB、2 IOPS/GB 耐久性块存储器的示例：**
   ```
   ibmcloud sl block volume-order --storage-type endurance --size 20 --tier 2 --os-type LINUX --datacenter dal10
   ```
   {: pre}

   **供应 20 GB、100 IOPS 性能块存储器的示例：**
   ```
   ibmcloud sl block volume-order --storage-type performance --size 20 --iops 100 --os-type LINUX --datacenter dal10
   ```
   {: pre}

4. 验证块存储设备是否已创建，并记下卷的 **`id`**。**注：**如果未立即看到块存储设备，请等待几分钟。然后，重新运行此命令。
   ```
   ibmcloud sl block volume-list 
   ```
   {: pre}

   输出示例：
   ```
   id         username          datacenter   storage_type                capacity_gb   bytes_used   ip_addr         lunId   active_transactions
   123456789  IBM02SL1234567-8  dal10        performance_block_storage   20            -            161.12.34.123   0       0
   ```
   {: screen}

5. 查看卷的详细信息，并记下 **`Target IP`** 和 **`LUN Id`**。
   ```
   ibmcloud sl block volume-detail <volume_ID>
   ```
   {: pre}

   输出示例：
   ```
   Name                       Value
   ID                         1234567890
   User name                  IBM123A4567890-1
   Type                       performance_block_storage
   Capacity (GB)              20
   LUN Id                     0
   IOPs                       100
   Datacenter                 dal10
   Target IP                  161.12.34.123
   # of Active Transactions   0
   Replicant Count            0
   ```
   {: screen}

6. 授权非 SDS 工作程序节点访问块存储设备。将 `<volume_ID>` 替换为先前检索到的块存储设备的卷标识，并将 `<private_worker_IP>` 替换为要在其中连接设备的非 SDS 工作程序节点的专用 IP 地址。

   ```
   ibmcloud sl block access-authorize <volume_ID> -p <private_worker_IP>
   ```
   {: pre}

   输出示例：
   ```
   The IP address 123456789 was authorized to access <volume_ID>.
   ```
   {: screen}

7. 验证是否已成功授权非 SDS 工作程序节点，并记下 **`host_iqn`**、**`username`** 和 **`password`**。
   ```
   ibmcloud sl block access-list <volume_ID>
   ```
   {: pre}

   输出示例：
   ```
   ID          name                 type   private_ip_address   source_subnet   host_iqn                                      username   password           allowed_host_id
   123456789   <private_worker_IP>  IP     <private_worker_IP>  -               iqn.2018-09.com.ibm:ibm02su1543159-i106288771   IBM02SU1543159-I106288771   R6lqLBj9al6e2lbp   1146581
   ```
   {: screen}

   如果分配了 **`host_iqn`**、**`username`** 和 **`password`**，那么表明授权成功。

8. [将块存储设备连接到工作程序节点](#attach_block)。


## 将原始块存储器连接到非 SDS 工作程序节点
{: #attach_block}

要将块存储设备连接到非 SDS 工作程序节点，必须使用 {{site.data.keyword.Bluemix_notm}} Block Volume Attacher 存储类和块存储设备详细信息来创建持久卷 (PV)。
{: shortdesc}

**开始之前**：
- 确保已[自动](#automatic_block)或[手动](#manual_block)创建要用于非 SDS 工作程序节点的未格式化且未安装的原始块存储器。
- [登录到您的帐户。如果适用，请将相应的资源组设定为目标。为集群设置上下文。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

**要将原始块存储器连接到非 SDS 工作程序节点，请执行以下操作**：
1. 准备创建 PV。  
   - **如果使用的是 `mkpvyaml` 容器：**
     1. 打开 `pv-<cluster_name>.yaml` 文件。
        ```
        nano pv-<cluster_name>.yaml
        ```
        {: pre}

     2. 查看 PV 的配置。

   - **如果是手动添加的块存储器：**
     1. 创建 `pv.yaml` 文件。以下命令会使用 `nano` 编辑器来创建该文件。
        ```
        nano pv.yaml
        ```
        {: pre}

     2. 将块存储设备的详细信息添加到 PV。
        ```
        apiVersion: v1
        kind: PersistentVolume
        metadata:
          name: <pv_name>
          annotations:
            ibm.io/iqn: "<IQN_hostname>"
            ibm.io/username: "<username>"
            ibm.io/password: "<password>"
            ibm.io/targetip: "<targetIP>"
            ibm.io/lunid: "<lunID>"
            ibm.io/nodeip: "<private_worker_IP>"
            ibm.io/volID: "<volume_ID>"
        spec:
          capacity:
            storage: <size>
          accessModes:
            - ReadWriteOnce
          hostPath:
              path: /
          storageClassName: ibmc-block-attacher
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
      	<td>输入 PV 的名称。</td>
      	</tr>
        <tr>
        <td><code>ibm.io/iqn</code></td>
        <td>输入先前检索到的 IQN 主机名。</td>
        </tr>
        <tr>
        <td><code>ibm.io/username</code></td>
        <td>输入先前检索到的 IBM Cloud Infrastructure (SoftLayer) 用户名。</td>
        </tr>
        <tr>
        <td><code>ibm.io/password</code></td>
        <td>输入先前检索到的 IBM Cloud Infrastructure (SoftLayer) 密码。</td>
        </tr>
        <tr>
        <td><code>ibm.io/targetip</code></td>
        <td>输入先前检索到的目标 IP。</td>
        </tr>
        <tr>
        <td><code>ibm.io/lunid</code></td>
        <td>输入先前检索到的块存储设备的 LUN 标识。</td>
        </tr>
        <tr>
        <td><code>ibm.io/nodeip</code></td>
        <td>输入要在其中连接块存储设备并且先前授权其访问块存储设备的工作程序节点的专用 IP 地址。</td>
        </tr>
        <tr>
          <td><code>ibm.io/volID</code></td>
        <td>输入先前检索到的块存储卷的标识。</td>
        </tr>
        <tr>
        <td><code>storage</code></td>
        <td>输入先前创建的块存储设备的大小。例如，如果块存储设备为 20 千兆字节，请输入 <code>20Gi</code>。</td>
        </tr>
        </tbody>
        </table>
2. 创建 PV 以将块存储设备连接到非 SDS 工作程序节点。
   - **如果使用的是 `mkpvyaml` 容器：**
     ```
     kubectl apply -f pv-<cluster_name>.yaml
     ```
     {: pre}

   - **如果是手动添加的块存储器：**
     ```
     kubectl apply -f  block-volume-attacher/pv.yaml
     ```
     {: pre}

3. 验证块存储器是否已成功连接到工作程序节点。
   ```
   kubectl describe pv <pv_name>
   ```
   {: pre}

   输出示例：
   ```
   Name:            kube-wdc07-cr398f790bc285496dbeb8e9137bc6409a-w1-pv1
   Labels:          <none>
   Annotations:     ibm.io/attachstatus=attached
                    ibm.io/dm=/dev/dm-1
                    ibm.io/iqn=iqn.2018-09.com.ibm:ibm02su1543159-i106288771
                    ibm.io/lunid=0
                    ibm.io/mpath=3600a09803830445455244c4a38754c66
                    ibm.io/nodeip=10.176.48.67
                    ibm.io/password=R6lqLBj9al6e2lbp
                    ibm.io/targetip=161.26.98.114
                    ibm.io/username=IBM02SU1543159-I106288771
                    kubectl.kubernetes.io/last-applied-configuration={"apiVersion":"v1","kind":"PersistentVolume","metadata":{"annotations":{"ibm.io/iqn":"iqn.2018-09.com.ibm:ibm02su1543159-i106288771","ibm.io/lunid":"0"...
   Finalizers:      []
   StorageClass:    ibmc-block-attacher
   Status:          Available
   Claim:
   Reclaim Policy:  Retain
   Access Modes:    RWO
   Capacity:        20Gi
   Node Affinity:   <none>
   Message:
   Source:
       Type:          HostPath (bare host directory volume)
       Path:          /
       HostPathType:
   Events:            <none>
   ```
   {: screen}

   如果 **ibm.io/dm** 设置为设备标识（如 `/dev/dm/1`），并且您可以在 CLI 输出的 **Annotations** 部分中看到 **ibm.io/attachstatus=attached**，那么表明块存储设备已成功连接。

如果想要拆离卷，可以删除 PV。已拆离的卷仍可由特定工作程序节点进行访问，当您使用 {{site.data.keyword.Bluemix_notm}} Block Volume Attacher 存储类创建新的 PV 以将其他卷连接到同一工作程序节点时，会重新连接已拆离的卷。要避免重新连接旧的已拆离卷，请使用 `ibmcloud sl block access-revoke` 命令取消工作程序节点对已拆离卷的访问权。拆离卷并不会从 IBM Cloud Infrastructure (SoftLayer) 帐户中除去该卷。要取消该卷的计费，必须手动[从 IBM Cloud Infrastructure (SoftLayer) 帐户中除去存储器](/docs/containers?topic=containers-cleanup)。
{: note}


