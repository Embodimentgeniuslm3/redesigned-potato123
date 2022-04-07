---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-11"

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



# 从集群中除去持久性存储器
{: #cleanup}

在集群中设置持久性存储器时，有三个主要组件：Kubernetes 持久卷声明 (PVC)（用于请求存储器）、Kubernetes 持久卷 (PV)（将安装到 pod，并在 PVC 中进行描述）和 IBM Cloud Infrastructure (SoftLayer) 实例（例如，NFS 文件存储器或块存储器）。所有这三个组件可能需要分别删除，具体取决于存储器的创建方式。
{:shortdesc}

## 清除持久性存储器
{: #storage_remove}

了解删除选项：

**我已删除集群。还必须删除其他任何内容才能除去持久性存储器吗？**</br>
视情况而定。删除集群时，会删除 PVC 和 PV。但是，您可以选择是否除去 IBM Cloud Infrastructure (SoftLayer) 中的关联存储器实例。如果选择不除去，那么存储器实例就会仍然存在。此外，如果删除的是状态为运行状况不佳的集群，那么即使选择除去存储器，存储器也可能会仍然存在。请按照指示信息进行操作，尤其是有关如何在 IBM Cloud Infrastructure (SoftLayer) 中[删除存储器实例](#sl_delete_storage)的步骤。

**可以通过删除 PVC 来除去所有存储器吗？**</br>
有时可以。如果是[动态创建持久性存储器](/docs/containers?topic=containers-kube_concepts#dynamic_provisioning)，并且选择其名称中不含 `retain` 的存储类，那么删除 PVC 时，会同时删除 PV 和 IBM Cloud Infrastructure (SoftLayer) 存储器实例。

在其他所有情况下，请按照指示信息来检查 PVC、PV 和物理存储设备的阶段状态，然后根据需要分别进行删除。

**删除存储器后，我还需要为该存储器付费吗？**</br>
这取决于删除的内容和计费类型。如果删除 PVC 和 PV，但未删除 IBM Cloud Infrastructure (SoftLayer) 帐户中的实例，那么该实例仍然存在，因此仍需要为此付费。必须删除所有内容才能避免继续付费。此外，在 PVC 中指定 `billingType` 时，可以选择 `hourly` 或 `monthly`。如果选择 `monthly`，那么实例将按月收费。删除实例后，仍需要为该月的剩余时间付费。


<p class="important">清除持久性存储器时，将删除其中存储的所有数据。如果需要数据的副本，请备份[文件存储器](/docs/containers?topic=containers-file_storage#file_backup_restore)或[块存储器](/docs/containers?topic=containers-block_storage#block_backup_restore)。</p>

开始之前：[登录到您的帐户。如果适用，请将相应的资源组设定为目标。为集群设置上下文。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

要清除持久数据，请执行以下操作：

1.  列出集群中的 PVC，并记下 PVC 的 **`NAME`**、**`STORAGECLASS`** 以及绑定到该 PVC 并显示为 **`VOLUME`** 的 PV 的名称。
    ```
    kubectl get pvc
    ```
    {: pre}

    输出示例：
    ```
    NAME                  STATUS    VOLUME                                     CAPACITY   ACCESSMODES   STORAGECLASS            AGE
    claim1-block-bronze   Bound     pvc-06886b77-102b-11e8-968a-f6612bb731fb   20Gi       RWO           ibmc-block-bronze       78d
    claim-file-bronze     Bound     pvc-457a2b96-fafc-11e7-8ff9-b6c8f770356c   4Gi        RWX           ibmc-file-bronze-retain 105d
    claim-file-silve      Bound     pvc-1efef0ba-0c48-11e8-968a-f6612bb731fb   24Gi       RWX           ibmc-file-silver        83d
    ```
    {: screen}

2. 查看存储类的 **`ReclaimPolicy`** 和 **`billingType`**。
   ```
   kubectl describe storageclass <storageclass_name>
   ```
   {: pre}

   如果回收策略显示 `Delete`，那么在除去 PVC 时会除去 PV 和物理存储器。如果回收策略显示 `Retain`，或者所供应的存储器不具有存储类，那么在除去 PVC 时不会除去 PV 和物理存储器。PVC、PV 和物理存储器必须分别进行除去。

   如果存储器按月收费，那么即使在计费周期结束之前除去了存储器，也仍需要按整月付费。
   {: important}

3. 除去安装了 PVC 的所有 pod。
   1. 列出安装了 PVC 的所有 pod。
      ```
      kubectl get pods --all-namespaces -o=jsonpath='{range .items[*]}{"\n"}{.metadata.name}{":\t"}{range .spec.volumes[*]}{.persistentVolumeClaim.claimName}{" "}{end}{end}' | grep "<pvc_name>"
      ```
      {: pre}

      输出示例：
    ```
      blockdepl-12345-prz7b:	claim1-block-bronze  
      ```
      {: screen}

      如果 CLI 输出中未返回任何 pod，那么说明没有 pod 使用 PVC。

   2. 除去使用 PVC 的 pod。如果 pod 是部署的一部分，请除去该部署。
      ```
      kubectl delete pod <pod_name>
      ```
      {: pre}

   3. 验证 pod 是否已除去。
      ```
      kubectl get pods
      ```
      {: pre}

4. 除去 PVC。
   ```
   kubectl delete pvc <pvc_name>
   ```
   {: pre}

5. 查看 PV 的阶段状态。使用先前检索到的显示为 **`VOLUME`** 的 PV 的名称。
   ```
   kubectl get pv <pv_name>
   ```
   {: pre}

   除去 PVC 时，会释放绑定到该 PVC 的 PV。如果 PV 是自动删除的，那么该 PV 会进入 `Deleting` 状态；如果必须手动删除 PV，那么该 PV 会进入 `Released` 状态，具体取决于存储器的供应方式。**注**：对于自动删除的 PV，在删除之前，阶段状态可能会短暂地显示为 `Released`。请在几分钟后重新运行该命令以查看该 PV 是否已除去。

6. 如果 PV 未删除，请手动除去该 PV。
   ```
   kubectl delete pv <pv_name>
   ```
   {: pre}

7. 验证 PV 是否已除去。
   ```
   kubectl get pv
   ```
   {: pre}

8. {: #sl_delete_storage}列出 PV 指向的物理存储器实例，并记下物理存储器实例的 **`id`**。

   **文件存储器：**
   ```
   ibmcloud sl file volume-list --columns id  --columns notes | grep <pv_name>
   ```
   {: pre}
   **块存储器：**
   ```
   ibmcloud sl block volume-list --columns id --columns notes | grep <pv_name>
   ```
   {: pre}

   如果除去集群后无法检索 PV 的名称，请将 `grep <pv_name>` 替换为 `grep <cluster_id>` 以列出与该集群关联的所有存储设备。
   {: tip}

   输出示例：
   ```
   id         notes   
   12345678   {"plugin":"ibm-file-plugin-5b55b7b77b-55bb7","region":"us-south","cluster":"aa1a11a1a11b2b2bb22b22222c3c3333","type":"Endurance","ns":"default","pvc":"mypvc","pv":"pvc-d979977d-d79d-77d9-9d7d-d7d97ddd99d7","storageclass":"ibmc-file-gold"}
   ```
   {: screen}

   了解 **Notes** 字段信息：
   *  **`"plugin":"ibm-file-plugin-5b55b7b77b-55bb7"`**：集群使用的存储器插件。
   *  **`"region":"us-south"`**：集群所在的区域。
   *  **`"cluster":"aa1a11a1a11b2b2bb22b22222c3c3333"`**：与存储器实例关联的集群标识。
   *  **`"type":"Endurance"`**：文件存储器或块存储器的类型（`Endurance` 或 `Performance`）。
   *  **`"ns":"default"`**：将存储器实例部署到的名称空间。
   *  **`"pvc":"mypvc"`**：与存储器实例关联的 PVC 的名称。
   *  **`"pv":"pvc-d979977d-d79d-77d9-9d7d-d7d97ddd99d7"`**：与存储器实例关联的 PV。
   *  **`"storageclass":"ibmc-file-gold"`**：存储类的类型：铜牌级、银牌级、金牌级或定制。

9. 除去物理存储器实例。

   **文件存储器：**
   ```
   ibmcloud sl file volume-cancel <filestorage_id>
   ```
   {: pre}

   **块存储器：**
   ```
   ibmcloud sl block volume-cancel <blockstorage_id>
   ```
   {: pre}

9. 验证物理存储器实例是否已除去。删除过程最长可能需要几天时间才能完成。

   **文件存储器：**
   ```
   ibmcloud sl file volume-list 
   ```
   {: pre}
   **块存储器：**
   ```
   ibmcloud sl block volume-list
   ```
   {: pre}
