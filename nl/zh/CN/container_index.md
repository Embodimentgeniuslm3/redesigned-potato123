---

copyright:
  years: 2014, 2019
lastupdated: "2019-04-11"

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


# {{site.data.keyword.containerlong_notm}} 入门
{: #container_index}

通过在 Kubernetes 集群中运行的 Docker 容器中部署高可用性应用程序，可迅速开始使用 {{site.data.keyword.containerlong}}。
{:shortdesc}

容器是打包应用程序及其所有依赖项的标准方法，以便您可以无缝地在环境之间移动应用程序。与虚拟机不同，容器不会捆绑操作系统。在容器中只会打包应用程序代码、运行时、系统工具、库和设置。因此容器比虚拟机更轻便、可移植性更高且更高效。


## 集群入门
{: #clusters_gs_index}

想在容器中部署应用程序？稍等一下！请首先创建 Kubernetes 集群。Kubernetes 是一种用于容器的编排工具。使用 Kubernetes，开发者可以通过利用集群的强大功能和灵活性，快速部署高可用性应用程序。
{:shortdesc}

那么什么是集群呢？集群是一组资源、工作程序节点、网络和存储设备，用于使应用程序保持高可用性。拥有集群后，可以将应用程序部署到容器中。

开始之前，请获取适合您的 [{{site.data.keyword.Bluemix_notm}} 帐户](https://cloud.ibm.com/registration) 类型：
* **计费（现收现付或预订）**：可以创建免费集群。还可以供应 IBM Cloud Infrastructure (SoftLayer) 资源以在标准集群中创建和使用。
* **轻量**：无法创建免费或标准集群。请[升级帐户](/docs/account?topic=account-accountfaqs#changeacct)至计费帐户。

要创建免费集群，请执行以下操作：

1.  在 [{{site.data.keyword.Bluemix_notm}} **目录** ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/catalog?category=containers) 中，选择 **{{site.data.keyword.containershort_notm}}**，然后单击**创建**。这将打开集群配置页面。缺省情况下，已选择**免费集群**。

2.  为集群提供唯一名称。

3.  单击**创建集群**。这将创建包含 1 个工作程序节点的工作程序池。供应工作程序节点可能需要几分钟的时间，但您可以在**工作程序节点**选项卡中查看进度。阶段状态达到 `Ready` 时，即可以开始使用集群！

<br>

非常好！您已创建第一个 Kubernetes 集群。下面是有关免费集群的一些详细信息：

*   **机器类型**：免费集群有一个分组到工作程序池的虚拟工作程序节点，其中有 2 个 CPU、4 GB 内存以及 1 个 100 GB 的 SAN 磁盘可供应用程序使用。创建标准集群时，可以选择物理（裸机）或虚拟机，以及各种机器大小。
*   **受管主节点**：工作程序节点由 {{site.data.keyword.IBM_notm}} 拥有的一个专用高可用性 Kubernetes 主节点进行集中监视和管理，该主节点控制和监视集群中的所有 Kubernetes 资源。此外，您可以专注于工作程序节点以及该工作程序节点中部署的应用程序，而不必担心如何管理此主节点。
*   **基础架构资源**：运行集群所需的资源（例如 VLAN 和 IP 地址）在 {{site.data.keyword.IBM_notm}} 拥有的 IBM Cloud Infrastructure (SoftLayer) 帐户中进行管理。创建标准集群时，可在您自己的 IBM Cloud Infrastructure (SoftLayer) 帐户中管理这些资源。创建标准集群时，可以了解有关这些资源及[所需许可权](/docs/containers?topic=containers-users#infra_access)的更多信息。
*   **其他选项**：免费集群会部署到您所选择的地理位置中，但您却无法为其选择部署到哪个专区。要对专区、联网和持久性存储器拥有控制权，请创建标准集群。[了解有关免费和标准集群优点的更多信息](/docs/containers?topic=containers-cs_ov#cluster_types)。

<br>

**接下来要做什么？**</br>
在免费集群到期之前，使用免费集群试用一些功能。

* 浏览[第一个 {{site.data.keyword.containerlong_notm}} 教程](/docs/containers?topic=containers-cs_cluster_tutorial#cs_cluster_tutorial)，以创建 Kubernetes 集群、安装 CLI 或使用 Kubernetes 终端、创建专用注册表、设置集群环境以及向集群添加服务。
* 一鼓作气继续浏览有关将应用程序部署到集群的[第二个 {{site.data.keyword.containerlong_notm}} 教程](/docs/containers?topic=containers-cs_apps_tutorial#cs_apps_tutorial)。
* [创建标准集群](/docs/containers?topic=containers-clusters#clusters_ui)（具有多个节点），以实现更高的可用性。


