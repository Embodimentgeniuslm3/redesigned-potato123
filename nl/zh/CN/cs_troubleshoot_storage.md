---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-05"

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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}



# 集群存储故障诊断
{: #cs_troubleshoot_storage}

在使用 {{site.data.keyword.containerlong}} 时，请考虑对集群存储进行故障诊断的以下方法。
{: shortdesc}

如果您有更常规的问题，请尝试[集群调试](/docs/containers?topic=containers-cs_troubleshoot)。
{: tip}


## 调试持久性存储器故障
{: #debug_storage}

复查可用于调试持久性存储器的选项，并查找失败的根本原因。
{: shortdesc}

1. 验证是否使用的是最新的 {{site.data.keyword.Bluemix_notm}} 和 {{site.data.keyword.containerlong_notm}} 插件版本。 
   ```
   ibmcloud update
   ```
   {: pre}
   
   ```
   ibmcloud plugin repo-plugins
   ```
   {: pre}

2. 验证在本地计算机上运行的 `kubectl` CLI 版本是否与集群中安装的 Kubernetes 版本相匹配。 
   1. 显示在集群中和本地计算机上安装的 `kubectl` CLI 版本。
      ```
      kubectl version
      ```
      {: pre} 
   
      输出示例：
      ```
      Client Version: version.Info{Major:"1", Minor:"13", GitVersion:"v1.13.5", GitCommit:"641856db18352033a0d96dbc99153fa3b27298e5", GitTreeState:"clean", BuildDate:"2019-03-25T15:53:57Z", GoVersion:"go1.12.1", Compiler:"gc", Platform:"darwin/amd64"}
      Server Version: version.Info{Major:"1", Minor:"13", GitVersion:"v1.13.5+IKS", GitCommit:"e15454c2216a73b59e9a059fd2def4e6712a7cf0", GitTreeState:"clean", BuildDate:"2019-04-01T10:08:07Z", GoVersion:"go1.11.5", Compiler:"gc", Platform:"linux/amd64"}
      ```
      {: screen}
    
      如果在 `GitVersion` 中看到客户机和服务器的版本相同，说明 CLI 版本匹配。可以忽略服务器版本中的 `+IKS` 部分。
   2. 如果本地计算机上和集群中的 `kubectl` CLI 版本不匹配，请[更新集群](/docs/containers?topic=containers-update)或[在本地计算机上安装其他 CLI 版本](/docs/containers?topic=containers-cs_cli_install#kubectl)。 

3. 仅限块存储器、对象存储器和 Portworx：确保已[使用 Kubernetes 服务帐户安装 Helm 服务器 Tiller](/docs/containers?topic=containers-helm#public_helm_install)。 

4. 仅限块存储器、对象存储器和 Portworx：确保已为插件安装最新的 Helm chart 版本。 
   
   **块存储器和对象存储器**： 
   
   1. 更新 Helm chart 存储库。
      ```
        helm repo update
        ```
      {: pre}
      
   2. 列出 `iks-charts` 存储库中的 Helm chart。
      ```
   helm search iks-charts
   ```
      {: pre}
      
      输出示例：
      ```
      iks-charts/ibm-block-storage-attacher          	1.0.2        A Helm chart for installing ibmcloud block storage attach...
      iks-charts/ibm-iks-cluster-autoscaler          	1.0.5        A Helm chart for installing the IBM Cloud cluster autoscaler
      iks-charts/ibm-object-storage-plugin           	1.0.6        A Helm chart for installing ibmcloud object storage plugin  
      iks-charts/ibm-worker-recovery                 	1.10.46      IBM Autorecovery system allows automatic recovery of unhe...
      ...
      ```
      {: screen}
      
   3. 列出集群中已安装的 Helm chart，并将安装的版本与可用版本进行比较。
      ```
   helm ls
   ```
      {: pre}
      
   4. 如果有更新版本可用，请安装此版本。有关指示信息，请参阅[更新 {{site.data.keyword.Bluemix_notm}} Block Storage 插件](/docs/containers?topic=containers-block_storage#updating-the-ibm-cloud-block-storage-plug-in)和[更新 {{site.data.keyword.cos_full_notm}} 插件](/docs/containers?topic=containers-object_storage#update_cos_plugin)。 
   
   **Portworx**： 
   
   1. 查找可用的[最新 Helm chart 版本 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/portworx/helm/tree/master/charts/portworx)。 
   
   2. 列出集群中已安装的 Helm chart，并将安装的版本与可用版本进行比较。
      ```
   helm ls
   ```
      {: pre}
   
   3. 如果有更新版本可用，请安装此版本。有关指示信息，请参阅[更新集群中的 Portworx](/docs/containers?topic=containers-portworx#update_portworx)。 
   
5. 验证存储器驱动程序和插件 pod 的状态是否为 **Running**。 
   1. 列出 `kube-system` 名称空间中的 pod。
      ```
    kubectl get pods -n kube-system
    ```
      {: pre}
      
   2. 如果 pod 未显示 **Running** 状态，请获取 pod 的更多详细信息以查找根本原因。根据 pod 的阶段状态，您可能无法执行以下所有命令。
      ```
      kubectl describe pod <pod_name> -n kube-system
      ```
      {: pre}
      
      ```
      kubectl logs <pod_name> -n kube-system
      ```
      {: pre}
      
   3. 分析 `kubectl describe pod` 命令的 CLI 输出中的 **Events** 部分以及最新日志，以查找错误的根本原因。 
   
6. 检查 PVC 是否已成功供应。 
   1. 检查 PVC 的状态。如果 PVC 显示的状态为 **Bound**，说明 PVC 已成功供应。
      ```
   kubectl get pvc
   ```
      {: pre}
      
   2. 如果 PVC 的状态显示为 **Pending**，请检索导致 PVC 保持暂挂状态的错误。
      ```
      kubectl describe pvc <pvc_name>
      ```
      {: pre}
      
   3. 查看 PVC 创建期间可能发生的常见错误。 
      - [文件存储器和块存储器：PVC 保持暂挂状态](#file_pvc_pending)
      - [对象存储器：PVC 保持暂挂状态](#cos_pvc_pending)
   
7. 检查安装存储器实例的 pod 是否已成功部署。 
   1. 列出集群中的 pod。如果 pod 显示的状态为 **Running**，说明 pod 已成功供应。
      ```
      kubectl get pods
      ```
      {: pre}
      
   2. 获取 pod 的详细信息，并检查 CLI 输出的 **Events** 部分中是否显示有错误。
      ```
        kubectl describe pod <pod_name>
        ```
      {: pre}
   
   3. 检索应用程序的日志，并检查是否可以看到任何错误消息。
      ```
      kubectl logs <pod_name>
      ```
      {: pre}
   
   4. 查看将 PVC 安装到应用程序时可能发生的常见错误。 
      - [文件存储器：应用程序无法访问或写入 PVC](#file_app_failures)
      - [块存储器：应用程序无法访问或写入 PVC](#block_app_failures)
      - [Object Storage：使用非 root 用户访问文件失败](#cos_nonroot_access)
      

## 文件存储器和块存储器：PVC 保持暂挂状态
{: #file_pvc_pending}

{: tsSymptoms}
创建 PVC 并运行 `kubectl get pvc <pvc_name>` 时，PVC 始终保持 **Pending** 状态，即使等待了一段时间后也是如此。 

{: tsCauses}
在 PVC 创建和绑定期间，File Storage 和 Block Storage 插件会执行许多不同的任务。每个任务都可能会失败并导致不同的错误消息。

{: tsResolve}

1. 查找导致 PVC 保持 **Pending** 状态的根本原因。 
   ```
   kubectl describe pvc <pvc_name> -n <namespace>
   ```
   {: pre}
   
2. 查看常见错误消息描述和解决方法。
   
   <table>
   <thead>
     <th>错误消息</th>
     <th>描述</th>
     <th>解决步骤</th>
  </thead>
  <tbody>
    <tr>
      <td><code>User doesn't have permissions to create or manage Storage</code></br></br><code>Failed to find any valid softlayer credentials in configuration file</code></br></br><code>Storage with the order ID %d could not be created after retrying for %d seconds.</code></br></br><code>Unable to locate datacenter with name <datacenter_name>.</code></td>
      <td>存储在集群的 `storage-secret-store` Kubernetes 私钥中的 IAM API 密钥或 IBM Cloud Infrastructure (SoftLayer) API 密钥没有供应持久性存储器所需的所有许可权。</td>
      <td>请参阅[由于缺少许可权，PVC 创建失败](#missing_permissions)。</td>
    </tr>
    <tr>
      <td><code>Your order will exceed the maximum number of storage volumes allowed. Please contact Sales</code></td>
      <td>每个 {{site.data.keyword.Bluemix_notm}} 帐户都设置有最多可创建的存储器实例数。通过创建 PVC，创建的实例数可超过最大存储实例数。</td>
      <td>要创建 PVC，请从以下选项中进行选择。<ul><li>除去所有未使用的 PVC。</li><li>要求 {{site.data.keyword.Bluemix_notm}} 帐户所有者通过[开具支持案例](/docs/get-support?topic=get-support-getting-customer-support)来增加存储配额。</li></ul> </td>
    </tr>
    <tr>
      <td><code>Unable to find the exact ItemPriceIds(type|size|iops) for the specified storage</code> </br></br><code>Failed to place storage order with the storage provider</code></td>
      <td>PVC 中指定的存储器大小和 IOPS 不受您选择的存储器类型支持，因此无法用于指定的存储类。</td>
      <td>查看[决定文件存储器配置](/docs/containers?topic=containers-file_storage#file_predefined_storageclass)和[决定块存储器配置](/docs/containers?topic=containers-block_storage#block_predefined_storageclass)，以查找要使用的存储类支持的存储器大小和 IOPS。更正大小和 IOPS，然后重新创建 PVC。</td>
    </tr>
    <tr>
  <td><code>Failed to find the datacenter name in configuration file</code></td>
      <td>在 PVC 中指定的数据中心不存在。</td>
  <td>运行 <code>ibmcloud ks locations</code> 以列出可用的数据中心。更正 PVC 中的数据中心，然后重新创建 PVC。</td>
    </tr>
    <tr>
  <td><code>Failed to place storage order with the storage provider</code></br></br><code>Storage with the order ID 12345 could not be created after retrying for xx seconds. </code></br></br><code>Failed to do subnet authorizations for the storage 12345.</code><code>Storage 12345 has ongoing active transactions and could not be completed after retrying for xx seconds.</code></td>
  <td>存储器大小、IOPS 和存储类型可能与您选择的存储类不兼容，或者 {{site.data.keyword.Bluemix_notm}} 基础架构 API 端点当前不可用。</td>
  <td>查看[决定文件存储器配置](/docs/containers?topic=containers-file_storage#file_predefined_storageclass)和[决定块存储器配置](/docs/containers?topic=containers-block_storage#block_predefined_storageclass)，以查找要使用的存储类和存储类型支持的存储器大小和 IOPS。然后，删除并重新创建 PVC。</td>
  </tr>
  <tr>
  <td><code>Failed to find the storage with storage id 12345. </code></td>
  <td>您希望使用 Kubernetes 静态供应来为现有存储实例创建 PVC，但找不到指定的存储器实例。</td>
  <td>遵循指示信息在集群中供应现有[文件存储器](/docs/containers?topic=containers-file_storage#existing_file)或[块存储器](/docs/containers?topic=containers-block_storage#existing_block)，并确保检索到现有存储器实例的正确信息。然后，删除并重新创建 PVC。</td>
  </tr>
  <tr>
  <td><code>Storage type not provided, expected storage type is `Endurance` or `Performance`. </code></td>
  <td>您创建了定制存储类，但指定的存储类型不受支持。</td>
  <td>更新定制存储类，以指定 `Endurance` 或 `Performance` 作为存储类型。要查找定制存储类的示例，请参阅[文件存储器](/docs/containers?topic=containers-file_storage#file_custom_storageclass)和[块存储器](/docs/containers?topic=containers-block_storage#block_custom_storageclass)的样本定制存储类。</td>
  </tr>
  </tbody>
  </table>
  
## 文件存储器：应用程序无法访问或写入 PVC
{: #file_app_failures}

将 PVC 安装到 pod 后，在访问或写入 PVC 时可能会遇到错误。
{: shortdesc}

1. 列出集群中的 pod 并查看 pod 的状态。 
   ```
   kubectl get pods
   ```
   {: pre}
   
2. 查找导致应用程序无法访问或写入 PVC 的根本原因。
   ```
        kubectl describe pod <pod_name>
        ```
   {: pre}
   
   ```
   kubectl logs <pod_name>
   ```
   {: pre}

3. 查看将 PVC 安装到 pod 时可能发生的常见错误。 
   <table>
   <thead>
     <th>症状或错误消息</th>
     <th>描述</th>
     <th>解决步骤</th>
  </thead>
  <tbody>
    <tr>
      <td>pod 卡在 <strong>ContainerCreating</strong> 状态。</br></br><code>MountVolume.SetUp failed for volume ... read-only file system</code></td>
      <td>{{site.data.keyword.Bluemix_notm}} 基础架构后端遇到网络问题。为了保护数据和避免数据损坏，{{site.data.keyword.Bluemix_notm}} 会自动断开文件存储器服务器的连接，以阻止对 NFS 文件共享执行写操作。</td>
      <td>请参阅[文件存储器：工作程序节点的文件系统更改为只读](#readonly_nodes)</td>
      </tr>
      <tr>
  <td><code>write-permission</code> </br></br><code>do not have required permission</code></br></br><code>cannot create directory '/bitnami/mariadb/data': Permission denied </code></td>
  <td>在部署中，指定了非 root 用户来拥有 NFS 文件存储器安装路径。缺省情况下，非 root 用户在支持 NFS 的存储器的卷安装路径上没有写许可权。</td>
  <td>请参阅[文件存储器：非 root 用户拥有 NFS 文件存储器安装路径时，应用程序发生故障](#nonroot)</td>
  </tr>
  <tr>
  <td>在指定非 root 用户来拥有 NFS 文件存储器安装路径或使用指定的非 root 用户标识部署 Helm chart 后，用户无法写入安装的存储器。</td>
  <td>部署或 Helm chart 配置为 pod 的 <code>fsGroup</code>（组标识）和 <code>runAsUser</code>（用户标识）指定了安全上下文。</td>
  <td>请参阅[文件存储器：添加持久性存储器的非 root 用户访问权失败](#cs_storage_nonroot)</td>
  </tr>
</tbody>
</table>

### 文件存储器：工作程序节点的文件系统更改为只读
{: #readonly_nodes}

{: tsSymptoms}
{: #stuck_creating_state}
您可能会看到下列其中一个症状：
- 当您运行 `kubectl get pods -o wide` 时，您会看到在同一工作程序节点上运行的多个 pod 陷入 `ContainerCreating` 状态。
- 运行 `kubectl describe` 命令时，在 **Events** 部分中看到以下错误：`MountVolume.SetUp failed for volume ... read-only file system`。

{: tsCauses}
工作程序节点上的文件系统是只读的。

{: tsResolve}
1.  备份可能存储在工作程序节点或容器中的任何数据。
2.  要对现有工作程序节点进行短时间修订，请重新装入工作程序节点。<pre class="pre"><code>ibmcloud ks worker-reload --cluster &lt;cluster_name&gt; --worker &lt;worker_ID&gt;</code></pre>

对于长期生效的修订，请[更新工作程序池的机器类型](/docs/containers?topic=containers-update#machine_type)。

<br />



### 文件存储器：非 root 用户拥有 NFS 文件存储器安装路径时，应用程序发生故障
{: #nonroot}

{: tsSymptoms}
向部署[添加 NFS 存储器](/docs/containers?topic=containers-file_storage#file_app_volume_mount)后，容器的部署失败。检索容器的日志时，您可能会看到如下所示的错误。pod 发生故障，并且卡在重新装入循环中。

```
write-permission
```
{: screen}

```
do not have required permission
```
{: screen}

```
cannot create directory '/bitnami/mariadb/data': Permission denied
```
{: screen}

{: tsCauses}
缺省情况下，非 root 用户在支持 NFS 的存储器的卷安装路径上没有写许可权。一些公共应用程序映像（例如，Jenkins 和 Nexus3）会在 Dockerfile 中指定拥有安装路径的非 root 用户。通过此 Dockerfile 创建容器时，由于安装路径上非 root 用户的许可权不足，创建容器会失败。要授予写许可权，可以修改 Dockerfile，以在更改安装路径许可权之前将非 root 用户临时添加到 root 用户组，或者使用 init 容器。

如果使用 Helm chart 来部署映像，请编辑 Helm 部署以使用 init 容器。
{:tip}



{: tsResolve}
在部署中包含 [init 容器 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/) 时，可以向 Dockerfile 中指定的非 root 用户授予对容器内卷安装路径的写许可权。init 容器会在应用程序容器启动之前启动。init 容器在容器内创建卷安装路径，将安装路径更改为由正确的（非 root）用户拥有，然后关闭。随后，应用程序容器将以必须写入安装路径的非 root 用户身份启动。由于该路径已由非 root 用户拥有，因此写入安装路径成功。如果您不想使用 init 容器，那么可以修改 Dockerfile 以添加对 NFS 文件存储器的非 root 用户访问权。


开始之前：[登录到您的帐户。如果适用，请将相应的资源组设定为目标。为集群设置上下文。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  打开应用程序的 Dockerfile，并从要向其授予卷安装路径的写入者许可权的用户获取用户标识 (UID) 和组标识 (GID)。在来自 Jenkins Dockerfile 的示例中，信息为：
    - UID：`1000`
    - GID：`1000`

    **示例**：

    ```
    FROM openjdk:8-jdk

    

    RUN apt-get update &&apt-get install -y git curl &&rm -rf /var/lib/apt/lists/*

    

    ARG user=jenkins
    ARG group=jenkins
    ARG uid=1000
    ARG gid=1000
    ARG http_port=8080
    ARG agent_port=50000

    

    ENV JENKINS_HOME /var/jenkins_home
    ENV JENKINS_SLAVE_AGENT_PORT ${agent_port}
    ...
    ```
    {:screen}

2.  通过创建持久卷声明 (PVC) 将持久性存储器添加到应用程序。此示例使用 `ibmc-file-bronze` 存储类。要查看可用的存储类，请运行 `kubectl get storageclasses`。

    ```
apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
      name: mypvc
      annotations:
        volume.beta.kubernetes.io/storage-class: "ibmc-file-bronze"
    spec:
      accessModes:
        - ReadWriteMany
      resources:
        requests:
          storage: 20Gi
    ```
    {: codeblock}

3.  创建 PVC。

    ```
     kubectl apply -f mypvc.yaml
     ```
    {: pre}

4.  在部署 `.yaml` 文件中，添加 init 容器。包含您先前检索到的 UID 和 GID。

    ```
initContainers:
    - name: initcontainer # Or replace the name
      image: alpine:latest
      command: ["/bin/sh", "-c"]
      args:
        - chown <UID>:<GID> /mount; # Replace UID and GID with values from the Dockerfile
      volumeMounts:
      - name: volume # Or you can replace with any name
        mountPath: /mount # Must match the mount path in the args line
    ```
    {: codeblock}

    Jenkins 部署的**示例**：

    ```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: my_pod
    spec:
      replicas: 1
      selector:
        matchLabels:
          app: jenkins      
      template:
        metadata:
          labels:
            app: jenkins
        spec:
          containers:
          - name: jenkins
            image: jenkins
            volumeMounts:
            - mountPath: /var/jenkins_home
              name: volume
          volumes:
          - name: volume
            persistentVolumeClaim:
              claimName: mypvc
          initContainers:
          - name: permissionsfix
            image: alpine:latest
            command: ["/bin/sh", "-c"]
            args:
              - chown 1000:1000 /mount;
            volumeMounts:
            - name: volume
              mountPath: /mount
    ```
    {: codeblock}

5.  创建 pod 并将 PVC 安装到该 pod。

    ```
    kubectl apply -f my_pod.yaml
    ```
    {: pre}

6. 验证卷是否已成功安装到 pod。记下 pod 名称和 **Containers/Mounts** 路径。

    ```
    kubectl describe pod <my_pod>
    ```
    {: pre}

    **输出示例**：

    ```
    Name:		    mypod-123456789
    Namespace:	default
    ...
    Init Containers:
    ...
    Mounts:
      /mount from volume (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-cp9f0 (ro)
    ...
    Containers:
      jenkins:
        Container ID:
        Image:		jenkins
        Image ID:
        Port:		  <none>
        State:		Waiting
          Reason:		PodInitializing
        Ready:		False
        Restart Count:	0
        Environment:	<none>
        Mounts:
          /var/jenkins_home from volume (rw)
          /var/run/secrets/kubernetes.io/serviceaccount from default-token-cp9f0 (ro)
    ...
    Volumes:
      myvol:
        Type:	PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
        ClaimName:	mypvc
        ReadOnly:	  false

    ```
    {: screen}

7.  使用先前记下的 pod 名称登录到 pod。

    ```
    kubectl exec -it <my_pod-123456789> /bin/bash
    ```
    {: pre}

8. 验证容器安装路径的许可权。在示例中，安装路径为 `/var/jenkins_home`。

    ```
    ls -ln /var/jenkins_home
    ```
    {: pre}

    **输出示例**：

    ```
    jenkins@mypod-123456789:/$ ls -ln /var/jenkins_home
    total 12
    -rw-r--r-- 1 1000 1000  102 Mar  9 19:58 copy_reference_file.log
    drwxr-xr-x 2 1000 1000 4096 Mar  9 19:58 init.groovy.d
    drwxr-xr-x 9 1000 1000 4096 Mar  9 20:16 war
    ```
    {: screen}

    此输出显示 Dockerfile 中的 GID 和 UID（在此示例中为 `1000` 和 `1000`）拥有容器内的安装路径。

<br />


### 文件存储器：添加持久性存储器的非 root 用户访问权失败
{: #cs_storage_nonroot}

{: tsSymptoms}
在[添加持久性存储器的非 root 用户访问权](#nonroot)或使用指定的非 root 用户标识部署 Helm chart 后，用户无法写入安装的存储器。

{: tsCauses}
部署或 Helm chart 配置为 pod 的 `fsGroup`（组标识）和 `runAsUser`（用户标识）指定了[安全上下文](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/)。目前，{{site.data.keyword.containerlong_notm}} 不支持 `fsGroup` 规范，而仅支持将 `runAsUser` 设置为 `0`（root 用户许可权）。

{: tsResolve}
从映像、部署或 Helm chart 配置文件中除去配置的 `fsGroup` 和 `runAsUser` 的 `securityContext` 字段，然后重新部署。如果需要将安装路径的所有权从 `nobody` 更改为其他值，请[添加非 root 用户访问权](#nonroot)。添加[非 root 用户 `initContainer`](#nonroot) 后，请在容器级别（而非 pod 级别）设置 `runAsUser`。

<br />




## 块存储器：应用程序无法访问或写入 PVC
{: #block_app_failures}

将 PVC 安装到 pod 后，在访问或写入 PVC 时可能会遇到错误。
{: shortdesc}

1. 列出集群中的 pod 并查看 pod 的状态。 
   ```
   kubectl get pods
   ```
   {: pre}
   
2. 查找导致应用程序无法访问或写入 PVC 的根本原因。
   ```
        kubectl describe pod <pod_name>
        ```
   {: pre}
   
   ```
   kubectl logs <pod_name>
   ```
   {: pre}

3. 查看将 PVC 安装到 pod 时可能发生的常见错误。 
   <table>
   <thead>
     <th>症状或错误消息</th>
     <th>描述</th>
     <th>解决步骤</th>
  </thead>
  <tbody>
    <tr>
      <td>pod 卡在 <strong>ContainerCreating</strong> 或 <strong>CrashLoopBackOff</strong> 状态。</br></br><code>MountVolume.SetUp failed for volume ... read-only.</code></td>
      <td>{{site.data.keyword.Bluemix_notm}} 基础架构后端遇到网络问题。为了保护数据和避免数据损坏，{{site.data.keyword.Bluemix_notm}} 自动断开了块存储器服务器的连接，以阻止对块存储器实例执行写操作。</td>
      <td>请参阅[块存储器：块存储器更改为只读](#readonly_block)</td>
      </tr>
      <tr>
  <td><code>failed to mount the volume as "ext4", it already contains xfs. Mount error: mount failed: exit status 32</code> </td>
        <td>您要安装设置为使用 <code>XFS</code> 文件系统的现有块存储器实例。创建 PV 和匹配的 PVC 时，指定了 <code>ext4</code> 或未指定文件系统。在 PV 中指定的文件系统必须与块存储器实例中设置的文件系统相同。</td>
  <td>请参阅[块存储器：由于文件系统不正确，将现有块存储器安装到 pod 失败](#block_filesystem)</td>
  </tr>
</tbody>
</table>

### 块存储器：块存储器更改为只读
{: #readonly_block}

{: tsSymptoms}
您可能看到以下症状：
- 运行 `kubectl get pods -o wide` 时，您会看到同一工作程序节点上的多个 pod 卡在 `ContainerCreating` 或 `CrashLoopBackOff` 状态。所有这些 pod 都使用的是同一块存储器实例。
- 运行 `kubectl describe pod` 命令时，在 **Events** 部分中看到以下错误：`MountVolume.SetUp failed for volume ... read-only`。

{: tsCauses}
如果在 pod 写入卷时发生网络错误，IBM Cloud Infrastructure (SoftLayer) 会通过将此卷更改为只读方式，从而保护卷上的数据不被损坏。使用此卷的 pod 无法继续写入卷而失败。

{: tsResolve}
1. 检查集群中安装的 {{site.data.keyword.Bluemix_notm}} Block Storage 插件的版本。
   ```
   helm ls
   ```
   {: pre}

2. 验证是否使用的是[最新版本的 {{site.data.keyword.Bluemix_notm}} Block Storage 插件](https://cloud.ibm.com/kubernetes/solutions/helm-charts/ibm/ibmcloud-block-storage-plugin)。如果不是，请[更新插件](/docs/containers?topic=containers-block_storage#updating-the-ibm-cloud-block-storage-plug-in)。
3. 如果对 pod 使用的是 Kubernetes 部署，请通过除去失败的 pod 并允许 Kubernetes 重新创建该 pod，从而重新启动该 pod。如果未使用部署，请通过运行 `kubectl get pod <pod_name> -o yaml >pod.yaml` 来检索用于创建 pod 的 YAML 文件。然后，删除并手动重新创建该 pod。
    ```
      kubectl delete pod <pod_name>
      ```
    {: pre}

4. 检查是否重新创建 pod 解决了该问题。如果没有解决问题，请重新装入工作程序节点。
   1. 查找运行 pod 的工作程序节点，并记下分配给工作程序节点的专用 IP 地址。
      ```
      kubectl describe pod <pod_name> | grep Node
      ```
      {: pre}

      输出示例：
      ```
      Node:               10.75.XX.XXX/10.75.XX.XXX
      Node-Selectors:  <none>
      ```
      {: screen}

   2. 使用上一步中的专用 IP 地址来检索工作程序节点的**标识**。
      ```
   ibmcloud ks workers --cluster <cluster_name_or_ID>
   ```
      {: pre}

   3. 正常[重新装入工作程序节点](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)。


<br />


### 块存储器：由于文件系统不正确，将现有块存储器安装到 pod 失败
{: #block_filesystem}

{: tsSymptoms}
运行 `kubectl describe pod <pod_name>` 时，您会看到以下错误：
```
failed to mount the volume as "ext4", it already contains xfs. Mount error: mount failed: exit status 32
```
{: screen}

{: tsCauses}
您具有设置为使用 `XFS` 文件系统的现有块存储设备。为了将此设备安装到 pod，您已[创建 PV](/docs/containers?topic=containers-block_storage#existing_block)，其中在 `spec/flexVolume/fsType` 部分中指定 `ext4` 作为文件系统，或者未指定任何文件系统。如果未定义任何文件系统，PV 将缺省为 `ext4`。PV 已成功创建，并且已链接到现有块存储器实例。但是，尝试使用匹配的 PVC 将 PV 安装到集群时，卷安装失败。无法将使用 `ext4` 文件系统的 `XFS` 块存储器实例安装到 pod。

{: tsResolve}
将现有 PV 中的文件系统从 `ext4` 更新为 `XFS`。

1. 列出集群中的现有 PV，并记下用于现有块存储器实例的 PV 的名称。
   ```
   kubectl get pv
   ```
   {: pre}

2. 将 PV YAML 保存到本地计算机。
   ```
   kubectl get pv <pv_name> -o yaml > <filepath/xfs_pv.yaml>
   ```
   {: pre}

3. 打开该 YAML 文件，并将 `fsType` 从 `ext4` 更改为 `xfs`。
4. 替换集群中的 PV。
   ```
   kubectl replace --force -f <filepath/xfs_pv.yaml>
   ```
   {: pre}

5. 登录到安装了 PV 的 pod。
   ```
   kubectl exec -it <pod_name> sh
   ```
   {: pre}

6. 验证文件系统是否已更改为 `XFS`。
   ```
   df -Th
   ```
   {: pre}

   输出示例：
   ```
   Filesystem Type Size Used Avail Use% Mounted on /dev/mapper/3600a098031234546d5d4c9876654e35 xfs 20G 33M 20G 1% /myvolumepath
   ```
   {: screen}

<br />



## Object Storage：安装 {{site.data.keyword.cos_full_notm}} `ibmc` Helm 插件失败
{: #cos_helm_fails}

{: tsSymptoms}
在安装 {{site.data.keyword.cos_full_notm}} `ibmc` Helm 插件时，安装失败，发生下列其中一个错误：
```
Error: symlink /Users/ibm/ibmcloud-object-storage-plugin/helm-ibmc /Users/ibm/.helm/plugins/helm-ibmc: file exists
```
{: screen}

```
Error: fork/exec /home/iksadmin/.helm/plugins/helm-ibmc/ibmc.sh: permission denied
```
{: screen}

{: tsCauses}
在安装 `ibmc` Helm 插件时，将创建从 `./helm/plugins/helm-ibmc` 目录到 `ibmc` Helm 插件位于本地系统上的目录（通常位于 `./ibmcloud-object-storage-plugin/helm-ibmc` 中）的符号链接。在从本地系统除去 `ibmc` Helm 插件时，或者将 `ibmc` Helm 插件目录移至其他位置时，不会除去符号链接。

如果您看到 `permission denied` 错误，说明您对 `ibmc.sh` bash 文件没有必需的 `read`、`write` 和 `execute` 许可权，因此无法执行 `ibmc` Helm 插件命令。 

{: tsResolve}

**对于符号链接错误**： 

1. 除去 {{site.data.keyword.cos_full_notm}} Helm 插件。
   ```
   rm -rf ~/.helm/plugins/helm-ibmc
   ```
   {: pre}

2. 遵循[文档](/docs/containers?topic=containers-object_storage#install_cos)以重新安装 `ibmc` Helm 插件和 {{site.data.keyword.cos_full_notm}} 插件。

**对于许可权错误**： 

1. 更改 `ibmc` 插件的许可权。 
   ```
   chmod 755 ~/.helm/plugins/helm-ibmc/ibmc.sh
   ```
   {: pre}
   
2. 试用 `ibm` Helm 插件。 
   ```
   helm ibmc --help
   ```
   {: pre}
   
3. [继续安装 {{site.data.keyword.cos_full_notm}} 插件](/docs/containers?topic=containers-object_storage#install_cos)。 


<br />


## 对象存储器：PVC 保持暂挂状态
{: #cos_pvc_pending}

{: tsSymptoms}
创建 PVC 并运行 `kubectl get pvc <pvc_name>` 时，PVC 始终保持 **Pending** 状态，即使等待了一段时间后也是如此。 

{: tsCauses}
在 PVC 创建和绑定期间，{{site.data.keyword.cos_full_notm}} 插件会执行许多不同的任务。每个任务都可能会失败并导致不同的错误消息。

{: tsResolve}

1. 查找导致 PVC 保持 **Pending** 状态的根本原因。 
   ```
   kubectl describe pvc <pvc_name> -n <namespace>
   ```
   {: pre}
   
2. 查看常见错误消息描述和解决方法。
   
   <table>
   <thead>
     <th>错误消息</th>
     <th>描述</th>
     <th>解决步骤</th>
  </thead>
  <tbody>
    <tr>
      <td>`User doesn't have permissions to create or manage Storage`</td>
      <td>存储在集群的 `storage-secret-store` Kubernetes 私钥中的 IAM API 密钥或 IBM Cloud Infrastructure (SoftLayer) API 密钥没有供应持久性存储器所需的所有许可权。</td>
      <td>请参阅[由于缺少许可权，PVC 创建失败](#missing_permissions)。</td>
    </tr>
    <tr>
      <td>`cannot get credentials: cannot get secret <secret_name>: secrets "<secret_name>" not found`</td>
      <td>用于保存 {{site.data.keyword.cos_full_notm}} 服务凭证的 Kubernetes 私钥不在 PVC 或 pod 所在的名称空间中。</td>
      <td>请参阅[由于找不到 Kubernetes 私钥，PVC 或 pod 创建失败](#cos_secret_access_fails)。</td>
    </tr>
    <tr>
      <td>`cannot get credentials: Wrong Secret Type.Provided secret of type XXXX.Expected type ibm/ibmc-s3fs`</td>
      <td>为 {{site.data.keyword.cos_full_notm}} 服务实例创建的 Kubernetes 私钥不包含 `type: ibm/ibmc-s3fs`。</td>
      <td>编辑用于保存 {{site.data.keyword.cos_full_notm}} 凭证的 Kubernetes 私钥，以添加或更改 `type` 为 `ibm/ibmc-s3fs`。</td>
    </tr>
    <tr>
      <td>`Bad value for ibm.io/object-store-endpoint XXXX: scheme is missing. Must be of the form http://<hostname> or https://<hostname>`</br> </br> `Bad value for ibm.io/iam-endpoint XXXX: scheme is missing. Must be of the form http://<hostname< or https://<hostname>`</td>
      <td>s3fs API 或 IAM API 端点的格式错误，或者无法根据集群位置检索到 s3fs API 端点。</td>
      <td>请参阅[由于错误的 s3fs API 端点，PVC 创建失败](#cos_api_endpoint_failure)。</td>
    </tr>
    <tr>
      <td>`object-path cannot be set when auto-create is enabled`</td>
      <td>在存储区中使用 `ibm.io/object-path` 注释指定了要安装到 PVC 的现有子目录。如果设置了子目录，那么必须禁用存储区自动创建功能。</td>
      <td>在 PVC 中，设置 `ibm.io/auto-create-bucket: "false"`，并在 `ibm.io/bucket` 中提供现有存储区的名称。</td>
    </tr>
    <tr>
    <td>`bucket auto-create must be enabled when bucket auto-delete is enabled`</td>
    <td>在 PVC 中，设置了 `ibm.io/auto-delete-bucket: true` 以在除去 PVC 时自动删除数据、存储区和 PV。此选项需要将 `ibm.io/auto-create-bucket` 设置为 <strong>true</strong>，并同时将 `ibm.io/bucket` 设置为 `""`。</td>
    <td>在 PVC 中，设置 `ibm.io/auto-create-bucket: true` 和 `ibm.io/bucket: ""`，以便使用格式为 `tmp-s3fs-xxxx` 的名称自动创建存储区。</td>
    </tr>
    <tr>
    <td>`bucket cannot be set when auto-delete is enabled`</td>
    <td>在 PVC 中，设置了 `ibm.io/auto-delete-bucket: true` 以在除去 PVC 时自动删除数据、存储区和 PV。此选项需要将 `ibm.io/auto-create-bucket` 设置为 <strong>true</strong>，并同时将 `ibm.io/bucket` 设置为 `""`。</td>
    <td>在 PVC 中，设置 `ibm.io/auto-create-bucket: true` 和 `ibm.io/bucket: ""`，以便使用格式为 `tmp-s3fs-xxxx` 的名称自动创建存储区。</td>
    </tr>
    <tr>
    <td>`cannot create bucket using API key without service-instance-id`</td>
    <td>如果要使用 IAM API 密钥来访问 {{site.data.keyword.cos_full_notm}} 服务实例，您必须在 Kubernetes 私钥中存储 API 密钥和 {{site.data.keyword.cos_full_notm}} 服务实例标识。</td>
    <td>请参阅[为 Object Storage 服务凭证创建私钥](/docs/containers?topic=containers-object_storage#create_cos_secret)。</td>
    </tr>
    <tr>
      <td>`object-path “<subdirectory_name>” not found inside bucket <bucket_name>`</td>
      <td>在存储区中使用 `ibm.io/object-path` 注释指定了要安装到 PVC 的现有子目录。在指定的存储区中找不到此子目录。</td>
      <td>验证在 `ibm.io/bucket` 中指定的存储区中是否存在 `ibm.io/object-path` 中指定的子目录。</td>
    </tr>
        <tr>
          <td>`BucketAlreadyExists: The requested bucket name is not available. The bucket namespace is shared by all users of the system. Please select a different name and try again.`</td>
          <td>您设置了 `ibm.io/auto-create-bucket: true` 并同时指定了存储区名称，或者指定了 {{site.data.keyword.cos_full_notm}} 中已存在的存储区名称。存储区名称在 {{site.data.keyword.cos_full_notm}} 中的所有服务实例和区域中必须唯一。</td>
          <td>确保设置 `ibm.io/auto-create-bucket: false`，并确保提供在 {{site.data.keyword.cos_full_notm}} 中唯一的存储区名称。如果要使用 {{site.data.keyword.cos_full_notm}} 插件自动创建存储区名称，请设置 `ibm.io/auto-create-bucket: true` 和 `ibm.io/bucket: ""`。这将使用格式为 `tmp-s3fs-xxxx` 的唯一名称来创建存储区。</td>
        </tr>
        <tr>
          <td>`cannot access bucket <bucket_name>: NotFound: Not Found`</td>
          <td>您尝试访问的是未创建的存储区，或者指定的存储类和 s3fs API 端点与创建存储区时使用的存储类和 s3fs API 端点不匹配。</td>
          <td>请参阅[无法访问现有存储区](#cos_access_bucket_fails)。</td>
        </tr>
        <tr>
          <td>`Put https://s3-api.dal-us-geo.objectstorage.service.networklayer.com/<bucket_name>: net/http: invalid header field value "AWS4-HMAC-SHA256 Credential=1234a12a123a123a1a123aa1a123a123\n\n/20190412/us-standard/s3/aws4_request, SignedHeaders=host;x-amz-content-sha256;x-amz-date, Signature=12aa1abc123456aabb12aas12aa123456sb123456abc" for key Authorization`</td>
          <td>Kubernetes 私钥中的值未正确编码为 Base64。</td>
          <td>查看 Kubernetes 私钥中的值，并将每个值编码为 Base64。您还可以使用 [`kubectl create secret`](/docs/containers?topic=containers-object_storage#create_cos_secret) 命令来创建新私钥，并让 Kubernetes 自动将值编码为 Base64。</td>
        </tr>
        <tr>
          <td>`cannot access bucket <bucket_name>: Forbidden: Forbidden`</td>
          <td>您指定了 `ibm.io/auto-create-bucket: false`，并且尝试访问并未创建的存储区，或者 {{site.data.keyword.cos_full_notm}} HMAC 凭证的服务访问密钥或访问密钥标识不正确。</td>
          <td>无法访问并未创建的存储区。请通过设置 `ibm.io/auto-create-bucket: true` 和 `ibm.io/bucket: ""` 来创建新的存储区。如果您是存储区的所有者，请参阅[由于错误的凭证或访问被拒绝，PVC 创建失败](#cred_failure)来检查凭证。</td>
        </tr>
        <tr>
          <td>`cannot create bucket <bucket_name>: AccessDenied: Access Denied`</td>
          <td>您指定了 `ibm.io/auto-create-bucket: true` 以在 {{site.data.keyword.cos_full_notm}} 中自动创建存储区，但为 Kubernetes 私钥中提供的凭证分配的是 IAM **读取者**服务访问角色。此角色不允许在 {{site.data.keyword.cos_full_notm}} 中创建存储区。</td>
          <td>请参阅[由于错误的凭证或访问被拒绝，PVC 创建失败](#cred_failure)。</td>
        </tr>
        <tr>
          <td>`cannot create bucket <bucket_name>: AccessForbidden: Access Forbidden`</td>
          <td>您指定了 `ibm.io/auto-create-bucket: true`，并在 `ibm.io/bucket` 中提供了现有存储区的名称。此外，还为 Kubernetes 私钥中提供的凭证分配了 IAM **读取者**服务访问角色。此角色不允许在 {{site.data.keyword.cos_full_notm}} 中创建存储区。</td>
          <td>要使用现有存储区，请设置 `ibm.io/auto-create-bucket: false`，并在 `ibm.io/bucket` 中提供现有存储区的名称。要使用现有 Kubernetes 私钥自动创建存储区，请设置 `ibm.io/bucket: ""`，并遵循[由于错误的凭证或访问被拒绝，PVC 创建失败](#cred_failure)，以验证 Kubernetes 私钥中的凭证。</td>
        </tr>
        <tr>
          <td>`cannot create bucket <bucket_name>: SignatureDoesNotMatch: The request signature we calculated does not match the signature you provided. Check your AWS Secret Access Key and signing method. For more information, see REST Authentication and SOAP Authentication for details`</td>
          <td>Kubernetes 私钥中提供的 HMAC 凭证的 {{site.data.keyword.cos_full_notm}} 私钥访问密钥不正确。</td>
          <td>请参阅[由于错误的凭证或访问被拒绝，PVC 创建失败](#cred_failure)。</td>
        </tr>
        <tr>
          <td>`cannot create bucket <bucket_name>: InvalidAccessKeyId: The AWS Access Key ID you provided does not exist in our records`</td>
          <td>Kubernetes 私钥中提供的 HMAC 凭证的 {{site.data.keyword.cos_full_notm}} 访问密钥标识或私钥访问密钥不正确。</td>
          <td>请参阅[由于错误的凭证或访问被拒绝，PVC 创建失败](#cred_failure)。</td>
        </tr>
        <tr>
          <td>`cannot create bucket <bucket_name>: CredentialsEndpointError: failed to load credentials` </br> </br> `cannot access bucket <bucket_name>: CredentialsEndpointError: failed to load credentials`</td>
          <td>IAM 凭证的 {{site.data.keyword.cos_full_notm}} API 密钥和 {{site.data.keyword.cos_full_notm}} 服务实例的 GUID 不正确。</td>
          <td>请参阅[由于错误的凭证或访问被拒绝，PVC 创建失败](#cred_failure)。</td>
        </tr>
  </tbody>
    </table>
    

### Object Storage：由于找不到 Kubernetes 私钥，PVC 或 pod 创建失败
{: #cos_secret_access_fails}

{: tsSymptoms}
在创建 PVC 或部署安装 PVC 的 pod 时，创建或部署失败。

- PVC 创建失败的示例错误消息：
  ```
  cannot get credentials: cannot get secret tsecret-key: secrets "secret-key" not found
  ```
  {: screen}

- pod 创建失败的示例错误消息：
  ```
  persistentvolumeclaim "pvc-3" not found (repeated 3 times)
  ```
  {: screen}

{: tsCauses}
存储 {{site.data.keyword.cos_full_notm}} 服务凭证的 Kubernetes 私钥、PVC 和 pod 不是全都位于同一 Kubernetes 名称空间中。在将私钥部署到与 PVC 或 pod 不同的名称空间时，无法访问私钥。

{: tsResolve}
此任务需要对所有名称空间的 [{{site.data.keyword.Bluemix_notm}} IAM **写入者**或**管理者**服务角色](/docs/containers?topic=containers-users#platform)。

1. 列出集群中的私钥并查看创建 {{site.data.keyword.cos_full_notm}} 服务实例的 Kubernetes 私钥的 Kubernetes 名称空间。私钥必须将 `ibm/ibmc-s3fs` 显示为 **Type**。
   ```
   kubectl get secrets --all-namespaces
   ```
   {: pre}

2. 检查 PVC 和 pod 的 YAML 配置文件以验证使用相同的名称空间。如果不想在私钥所在的名称空间中部署 pod，请在要部署 pod 的名称空间中[创建另一个私钥](/docs/containers?topic=containers-object_storage#create_cos_secret)。

3. 在相应的名称空间中创建 PVC 或部署 pod。

<br />


### Object Storage：由于错误的凭证或访问被拒绝，PVC 创建失败
{: #cred_failure}

{: tsSymptoms}
在创建 PVC 时，看到类似于以下其中一项的错误消息：

```
SignatureDoesNotMatch: The request signature we calculated does not match the signature you provided. Check your AWS Secret Access Key and signing method. For more information, see REST Authentication and SOAP Authentication for details.
```
{: screen}

```
AccessDenied: Access Denied status code: 403 
```
{: screen}

```
CredentialsEndpointError: failed to load credentials
```
{: screen}

```
InvalidAccessKeyId: The AWS Access Key ID you provided does not exist in our records`
```
{: screen}

```
cannot access bucket <bucket_name>: Forbidden: Forbidden
```
{: screen}


{: tsCauses}
用于访问服务实例的 {{site.data.keyword.cos_full_notm}} 服务凭证可能错误，或者仅允许对存储区执行读访问。

{: tsResolve}
1. 在服务详细信息页面上的导航中，单击**服务凭证**。
2. 查找凭证，然后单击**查看凭证**。
3. 在 **iam_role_crn** 部分中，验证您是否具有 `Writer` 或 `Manager` 角色。如果没有正确的角色，那么必须使用正确的许可权创建新的 {{site.data.keyword.cos_full_notm}} 服务凭证。 
4. 如果角色正确，请验证在 Kubernetes 私钥中是否使用了正确的 **access_key_id** 和 **secret_access_key**。 
5. [使用更新的 **access_key_id** 和 **secret_access_key** 创建新私钥](/docs/containers?topic=containers-object_storage#create_cos_secret)。 


<br />


### 对象存储器：由于错误的 s3fs 或 IAM API 端点，PVC 创建失败
{: #cos_api_endpoint_failure}

{: tsSymptoms}
创建 PVC 时，PVC 保持暂挂状态。运行 `kubectl describe pvc <pvc_name>` 命令后，您将看到下列其中一条错误消息： 

```
Bad value for ibm.io/object-store-endpoint XXXX: scheme is missing. Must be of the form http://<hostname> or https://<hostname>
```
{: screen}

```
Bad value for ibm.io/iam-endpoint XXXX: scheme is missing. Must be of the form http://<hostname> or https://<hostname>
```
{: screen}

{: tsCauses}
要使用的存储区的 s3fs API 端点的格式可能错误，或者集群部署在 {{site.data.keyword.containerlong_notm}} 中支持，但 {{site.data.keyword.cos_full_notm}} 插件尚不支持的位置。 

{: tsResolve}
1. 检查在 {{site.data.keyword.cos_full_notm}} 插件安装期间 `ibmc` Helm 插件自动分配给存储类的 s3fs API 端点。该端点基于集群的部署位置。  
   ```
   kubectl get sc ibmc-s3fs-standard-regional -o yaml | grep object-store-endpoint
   ```
   {: pre}

   如果命令返回 `ibm.io/object-store-endpoint: NA`，说明集群部署在 {{site.data.keyword.containerlong_notm}} 中支持，但 {{site.data.keyword.cos_full_notm}} 插件尚不支持的位置。要将该位置添加到 {{site.data.keyword.containerlong_notm}}，请在公共 Slack 中发布问题，或开具 {{site.data.keyword.Bluemix_notm}} 支持案例。有关更多信息，请参阅[获取帮助和支持](/docs/containers?topic=containers-cs_troubleshoot#ts_getting_help)。 
   
2. 如果在 PVC 中手动添加了带有 `ibm.io/endpoint` 注释的 s3fs API 端点或带有 `ibm.io/iam-endpoint` 注释的 IAM API 端点，请确保添加了格式为 `https://<s3fs_api_endpoint>` 和 `https://<iam_api_endpoint>` 的端点。注释会覆盖 {{site.data.keyword.cos_full_notm}} 存储类中的 `ibmc` 插件自动设置的 API 端点。 
   ```
   kubectl describe pvc <pvc_name>
   ```
   {: pre}
   
<br />


### Object Storage：无法访问现有存储区
{: #cos_access_bucket_fails}

{: tsSymptoms}
在创建 PVC 时，无法访问 {{site.data.keyword.cos_full_notm}} 中的存储区。您会看到类似于以下内容的错误消息：

```
Failed to provision volume with StorageClass "ibmc-s3fs-standard-regional": pvc:1b2345678b69175abc98y873e2:cannot access bucket <bucket_name>: NotFound: Not Found
```
{: screen}

{: tsCauses}
您可能使用了错误的存储类来访问现有存储区，或者尝试访问未创建的存储区。无法访问并未创建的存储区。 

{: tsResolve}
1. 从 [{{site.data.keyword.Bluemix_notm}} 仪表板 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/)，选择 {{site.data.keyword.cos_full_notm}} 服务实例。
2. 选择**存储区**。
3. 查看现有存储区的**类**和**位置**信息。
4. 选择相应的[存储类](/docs/containers?topic=containers-object_storage#cos_storageclass_reference)。
5. 确保设置 `ibm.io/auto-create-bucket: false`，并确保提供现有存储区的正确名称。 

<br />


## Object Storage：使用非 root 用户访问文件失败
{: #cos_nonroot_access}

{: tsSymptoms}
您已使用控制台或 REST API 将文件上传到 {{site.data.keyword.cos_full_notm}} 服务实例。在尝试使用通过 `runAsUser` 在应用程序部署中定义的非 root 用户访问这些文件时，将拒绝对文件的访问。

{: tsCauses}
在 Linux 中，文件或目录具有 3 个访问组：`Owner`、`Group` 和 `Other`。在使用控制台或 REST API 将文件上传到 {{site.data.keyword.cos_full_notm}} 时，将除去 `Owner`、`Group` 和 `Other` 的许可权。每个文件的许可权如下所示：

```
d--------- 1 root root 0 Jan 1 1970 <file_name>
```
{: screen}

在使用 {{site.data.keyword.cos_full_notm}} 插件上传文件时，将保留文件的许可权而不进行更改。

{: tsResolve}
要使用非 root 用户访问文件，非 root 用户必须具有文件的读写许可权。作为 pod 部署的一部分更改文件的许可权需要写操作。{{site.data.keyword.cos_full_notm}} 不是针对写工作负载而设计的。在 pod 部署期间更新许可权可能阻止 pod 进入 `Running` 状态。

要解决此问题，请在将 PVC 安装到应用程序 pod 之前，创建另一个 pod 来为非 root 用户设置正确的许可权。

1. 检查存储区中文件的许可权。
   1. 针对 `test-permission` pod 创建配置文件并将文件命名为 `test-permission.yaml`。
      ```
      apiVersion: v1
      kind: Pod
      metadata:
        name: test-permission
      spec:
        containers:
        - name: test-permission
          image: nginx
          volumeMounts:
          - name: cos-vol
            mountPath: /test
        volumes:
        - name: cos-vol
          persistentVolumeClaim:
            claimName: <pvc_name>
      ```
      {: codeblock}

   2. 创建 `test-permission` pod。
      ```
      kubectl apply -f test-permission.yaml
      ```
      {: pre}

   3. 登录到 pod。
      ```
      kubectl exec test-permission -it bash
      ```
      {: pre}

   4. 浏览到安装路径并列出文件的许可权。
      ```
      cd test && ls -al
      ```
      {: pre}

      输出示例：
      ```
      d--------- 1 root root 0 Jan 1 1970 <file_name>
      ```
      {: screen}

2. 删除 pod。
      
   ```
   kubectl delete pod test-permission
   ```
   {: pre}

3. 针对要用于纠正文件许可权的 pod 创建配置文件，并将其命名为 `fix-permission.yaml`。
   ```
   apiVersion: v1
   kind: Pod
   metadata:
     name: fix-permission 
     namespace: <namespace>
   spec:
     containers:
     - name: fix-permission
       image: busybox
       command: ['sh', '-c']
       args: ['chown -R <nonroot_userID> <mount_path>/*; find <mount_path>/ -type d -print -exec chmod u=+rwx,g=+rx {} \;']
       volumeMounts:
       - mountPath: "<mount_path>"
         name: cos-volume
     volumes:
     - name: cos-volume
       persistentVolumeClaim:
         claimName: <pvc_name>
    ```
    {: codeblock}

3. 创建 `fix-permission` pod。
   ```
   kubectl apply -f fix-permission.yaml
   ```
   {: pre}

4. 等待 pod 进入 `Completed` 状态。  
   ```
   kubectl get pod fix-permission
   ```
   {: pre}

5. 删除 `fix-permission` pod。
   ```
   kubectl delete pod fix-permission
   ```
   {: pre}

5. 重新创建先前用于检查许可权的 `test-permission` pod。
   ```
   kubectl apply -f test-permission.yaml
   ```
   {: pre}

5. 验证是否更新了文件的许可权。
   1. 登录到 pod。
      ```
      kubectl exec test-permission -it bash
      ```
      {: pre}

   2. 浏览到安装路径并列出文件的许可权。
      ```
      cd test && ls -al
      ```
      {: pre}

      输出示例：
      ```
      -rwxrwx--- 1 <nonroot_userID> root 6193 Aug 21 17:06 <file_name>
      ```
      {: screen}

6. 删除 `test-permission` pod。
   ```
   kubectl delete pod test-permission
   ```
   {: pre}

7. 使用非 root 用户将 PVC 安装到应用程序。

   将非 root 用户定义为 `runAsUser`，同时不在部署 YAML 中设置 `fsGroup`。设置 `fsGroup` 将触发 {{site.data.keyword.cos_full_notm}} 插件以在部署 pod 时更新存储区中所有文件的组许可权。更新许可权是写操作，可能阻止 pod 进入 `Running` 状态。
   {: important}

在 {{site.data.keyword.cos_full_notm}} 服务实例中设置正确的文件许可权后，请勿通过使用控制台或 REST API 来上传文件。使用 {{site.data.keyword.cos_full_notm}} 插件以将文件添加到服务实例。
{: tip}

<br />


   
## 由于缺少许可权，PVC 创建失败
{: #missing_permissions}

{: tsSymptoms}
创建 PVC 时，PVC 保持暂挂状态。运行 `kubectl describe pvc <pvc_name>` 时，您会看到类似于以下内容的错误消息： 

```
User doesn't have permissions to create or manage Storage
```
{: screen}

{: tsCauses}
存储在集群的 `storage-secret-store` Kubernetes 私钥中的 IAM API 密钥或 IBM Cloud Infrastructure (SoftLayer) API 密钥没有供应持久性存储器所需的所有许可权。 

{: tsResolve}
1. 检索存储在集群的 `storage-secret-store` Kubernetes 私钥中的 IAM 密钥或 IBM Cloud Infrastructure (SoftLayer) API 密钥，并验证是否使用了正确的 API 密钥。 
   ```
   kubectl get secret storage-secret-store -n kube-system -o yaml | grep slclient.toml: | awk '{print $2}' | base64 --decode
   ```
   {: pre}
   
   输出示例： 
   ```
   [Bluemix]
   iam_url = "https://iam.bluemix.net"
   iam_client_id = "bx"
   iam_client_secret = "bx"
   iam_api_key = "<iam_api_key>"
   refresh_token = ""
   pay_tier = "paid"
   containers_api_route = "https://us-south.containers.bluemix.net"

   [Softlayer]
   encryption = true
   softlayer_username = ""
   softlayer_api_key = ""
   softlayer_endpoint_url = "https://api.softlayer.com/rest/v3"
   softlayer_iam_endpoint_url = "https://api.softlayer.com/mobile/v3"
   softlayer_datacenter = "dal10"
   ```
   {: screen}
   
   IAM API 密钥会列在 CLI 输出的 `Bluemix.iam_api_key` 部分中。如果此时 `Softlayer.softlayer_api_key` 为空，那么会使用 IAM API 密钥来确定您的基础架构许可权。IAM API 密钥由运行第一个操作的用户自动设置，该用户需要在资源组和区域中具有 IAM **管理员**平台角色。如果在 `Softlayer.softlayer_api_key` 中设置了不同的 API 密钥，那么此密钥优先于 IAM API 密钥。集群管理员运行 `ibmcloud ks credentials-set` 命令时，将设置 `Softlayer.softlayer_api_key`。 
   
2. 如果要更改凭证，请更新使用的 API 密钥。 
    1.  要更新 IAM API 密钥，请使用 `ibmcloud ks api-key-reset` [命令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_api_key_reset)。要更新 IBM Cloud Infrastructure (SoftLayer) 密钥，请使用 `ibmcloud ks credential-set` [命令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_set)。
    2. 等待 `storage-secret-store` Kubernetes 私钥更新，此过程大约需要 10 到 15 分钟，然后验证密钥是否已更新。
       ```
       kubectl get secret storage-secret-store -n kube-system -o yaml | grep slclient.toml: | awk '{print $2}' | base64 --decode
       ```
       {: pre}
   
3. 如果 API 密钥正确，请验证该密钥是否具有供应持久性存储器的正确许可权。
   1. 联系帐户所有者以验证 API 密钥的许可权。 
   2. 作为帐户所有者，从 {{site.data.keyword.Bluemix_notm}} 控制台的导航中，选择**管理** > **访问权 (IAM)**。
   3. 选择**用户**，并找到要使用其 API 密钥的用户。 
   4. 从“操作”菜单中，选择**管理用户详细信息**。 
   5. 转至**经典基础架构**选项卡。 
   6. 展开**帐户**类别，并验证是否已分配**添加/升级存储器 (StorageLayer)** 许可权。 
   7. 展开**服务**类别，并验证是否已分配**存储器管理**许可权。 
4. 除去失败的 PVC。 
   ```
   kubectl delete pvc <pvc_name>
   ```
   {: pre}
   
5. 重新创建 PVC。 
   ```
   kubectl apply -f pvc.yaml
   ```
   {: pre}


## 获取帮助和支持
{: #storage_getting_help}

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

