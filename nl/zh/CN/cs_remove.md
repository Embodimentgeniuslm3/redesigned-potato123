---

copyright:
  years: 2014, 2019
lastupdated: "2019-05-31"

keywords: kubernetes, iks, clusters, worker nodes, worker pools, delete

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
{:gif: data-image-type='gif'}


# 除去集群
{: #remove}

对于使用计费帐户创建的免费和标准集群，不再需要这些集群时，必须手动将其除去，以便这些集群不再耗用资源。
{:shortdesc}

<p class="important">
不会为持久性存储器中的集群或数据创建备份。删除集群时，可以选择删除持久性存储器。如果选择删除持久性存储器，那么使用 `delete` 存储类供应的持久性存储器将从 IBM Cloud Infrastructure (SoftLayer) 中永久删除。如果是使用 `retain` 存储类供应的持久性存储器，并且选择删除存储器，那么将删除集群、PV 和 PVC，但 IBM Cloud Infrastructure (SoftLayer) 帐户中的持久性存储器实例会保留。</br></br>除去集群时，还会除去创建集群时自动供应的任何子网，以及使用 `ibmcloud ks cluster-subnet-create` 命令创建的任何子网。但是，如果是使用 `ibmcloud ks cluster-subnet-add 命令`以手动方式将现有子网添加到集群的，那么不会从 IBM Cloud Infrastructure (SoftLayer) 帐户中除去这些子网，并且您可以在其他集群中复用这些子网。</p>

开始之前：
* 记下集群标识。您可能需要集群标识来调查和除去未随集群一起自动删除的相关 IBM Cloud Infrastructure (SoftLayer) 资源。
* 如果要删除持久性存储器中的数据，请[了解删除选项](/docs/containers?topic=containers-cleanup#cleanup)。
* 确保您具有 [{{site.data.keyword.Bluemix_notm}} IAM **管理员**平台角色](/docs/containers?topic=containers-users#platform)。

要除去集群，请执行以下操作：

1. 可选：在 CLI 中，将集群中所有数据的副本保存到本地 YAML 文件。
  ```
  kubectl get all --all-namespaces -o yaml
  ```
  {: pre}

2. 除去集群。
  - 在 {{site.data.keyword.Bluemix_notm}} 控制台中
    1.  选择集群，然后单击**更多操作...** 菜单中的**删除**。

  - 通过 {{site.data.keyword.Bluemix_notm}} CLI
    1.  列出可用的集群。


        ```
        ibmcloud ks clusters
        ```
        {: pre}

    2.  删除集群。

        ```
        ibmcloud ks cluster-rm --cluster <cluster_name_or_ID>
        ```
        {: pre}

    3.  遵循提示并选择是否删除集群资源，包括容器、pod、绑定的服务、持久性存储器和私钥。
      - **持久性存储器**：持久性存储器为数据提供了高可用性。如果使用[现有文件共享](/docs/containers?topic=containers-file_storage#existing_file)创建了持久卷声明，那么在删除集群时无法删除该文件共享。必须日后从 IBM Cloud Infrastructure (SoftLayer) 产品服务组合中手动删除此文件共享。

          受每月计费周期的影响，无法在一个月的最后一天删除持久卷声明。如果在一个月的最后一天删除持久卷声明，那么删除操作会保持暂挂，直到下个月开始再执行。
          {: note}

后续步骤：
- 运行 `ibmcloud ks cluster` 命令时，已除去的集群不再列在可用集群列表中之后，您可以复用该集群的名称。
- 如果保留子网，那么可以[在新集群中复用子网](/docs/containers?topic=containers-subnets#subnets_custom)，也可以日后从 IBM Cloud Infrastructure (SoftLayer) 产品服务组合中手动删除这些子网。
- 如果保留了持久性存储器，那么日后可以通过 {{site.data.keyword.Bluemix_notm}} 控制台中的 IBM Cloud Infrastructure (SoftLayer) 仪表板来[删除存储器](/docs/containers?topic=containers-cleanup#cleanup)。
