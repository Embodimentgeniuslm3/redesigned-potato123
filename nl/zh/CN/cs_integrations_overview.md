---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: kubernetes, iks, helm

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
{:preview: .preview}f


# 支持的 IBM Cloud 和第三方集成
{: #supported_integrations}

您可以将各种外部服务和目录服务用于 {{site.data.keyword.containerlong}} 中的标准 Kubernetes 集群。
{:shortdesc}

## 常用集成
{: #popular_services}

<table summary="该表显示了可以添加到集群且 {{site.data.keyword.containerlong_notm}} 用户常用的可用服务。每行从左到右阅读，其中第一列是服务名称，第二列是服务描述。">
<caption>常用服务</caption>
<thead>
<tr>
<th>服务</th>
<th>类别</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.cloudaccesstrailfull}}</td>
<td>集群活动日志</td>
<td>通过 Grafana 来分析日志，以监视集群中进行的管理活动。有关服务的更多信息，请参阅 [Activity Tracker](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started) 文档。有关可以跟踪的事件类型的更多信息，请参阅 [Activity Tracker 事件](/docs/containers?topic=containers-at_events)。</td>
</tr>
<tr>
<td>{{site.data.keyword.appid_full}}</td>
<td>认证</td>
<td>使用 [{{site.data.keyword.appid_short}}](/docs/services/appid?topic=appid-getting-started) 并要求用户登录，提高了应用程序的安全级别。要向应用程序认证 Web 或 API HTTP/HTTPS 请求，可以使用 [{{site.data.keyword.appid_short_notm}} 认证 Ingress 注释](/docs/containers?topic=containers-ingress_annotation#appid-auth)将 {{site.data.keyword.appid_short_notm}} 与 Ingress 服务集成。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix}} Block Storage</td>
<td>块存储器</td>
<td>[{{site.data.keyword.Bluemix_notm}} Block Storage](/docs/infrastructure/BlockStorage?topic=BlockStorage-getting-started#getting-started) 是一种持久的高性能 iSCSI 存储器，可以使用 Kubernetes 持久卷 (PV) 添加到应用程序。使用块存储器可在单个专区中部署有状态的应用程序，或者用于部署为单个 pod 的高性能存储器。有关如何在集群中供应块存储器的更多信息，请参阅[在 {{site.data.keyword.Bluemix_notm}} Block Storage 上存储数据](/docs/containers?topic=containers-block_storage#block_storage)。</td>
</tr>
<tr>
<td>{{site.data.keyword.cloudcerts_full}}</td>
<td>TLS 证书</td>
<td>可以使用 <a href="/docs/services/certificate-manager?topic=certificate-manager-getting-started#getting-started" target="_blank">{{site.data.keyword.cloudcerts_long}} <img src="../icons/launch-glyph.svg" alt="外部链接图标"></a> 来存储和管理应用程序的 SSL 证书。有关更多信息，请参阅 <a href="https://www.ibm.com/blogs/bluemix/2018/01/use-ibm-cloud-certificate-manager-ibm-cloud-container-service-deploy-custom-domain-tls-certificates/" target="_blank">Use {{site.data.keyword.cloudcerts_long_notm}} with {{site.data.keyword.containerlong_notm}} to deploy custom domain TLS Certificates <img src="../icons/launch-glyph.svg" alt="外部链接图标"></a>。</td>
</tr>
<tr>
<td>{{site.data.keyword.registrylong}}</td>
<td>容器映像</td>
<td>设置您自己的安全 Docker 映像存储库，在其中可以安全地存储映像并在集群用户之间共享这些映像。有关更多信息，请参阅 <a href="/docs/services/Registry?topic=registry-getting-started" target="_blank">{{site.data.keyword.registrylong}} 文档 <img src="../icons/launch-glyph.svg" alt="外部链接图标"></a>。</td>
</tr>
<tr>
<td>{{site.data.keyword.contdelivery_full}}</td>
<td>构建自动化</td>
<td>使用工具链自动构建应用程序并将容器部署到 Kubernetes 集群。有关设置的更多信息，请参阅博客 <a href="https://developer.ibm.com/recipes/tutorials/deploy-kubernetes-pods-to-the-bluemix-container-service-using-devops-pipelines/" target="_blank">Deploy Kubernetes pods to the {{site.data.keyword.containerlong_notm}} using DevOps Pipelines <img src="../icons/launch-glyph.svg" alt="外部链接图标"></a>。</td>
</tr>
<tr>
<td>{{site.data.keyword.datashield_full}} (Beta)</td>
<td>内存加密</td>
<td>可以使用 <a href="/docs/services/data-shield?topic=data-shield-getting-started#getting-started" target="_blank">{{site.data.keyword.datashield_short}} <img src="../icons/launch-glyph.svg" alt="外部链接图标"></a> 来加密数据内存。{{site.data.keyword.datashield_short}} 与 Intel® Software Guard Extensions (SGX) 和 Fortanix® 技术相集成，以便保护使用中的 {{site.data.keyword.Bluemix_notm}} 容器工作负载代码和数据。应用程序代码和数据在 CPU 固化的相关可调度单元组中运行，这是工作程序节点上用于保护应用程序关键方面的可信内存区域，有助于使代码和数据保持机密性且不被修改。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix}} File Storage</td>
<td>文件存储器</td>
<td>[{{site.data.keyword.Bluemix_notm}} File Storage](/docs/infrastructure/FileStorage?topic=FileStorage-getting-started#getting-started) 是一种基于 NFS 的持久、快速、灵活的网络连接文件存储器，可以使用 Kubernetes 持久卷添加到应用程序。您可以在具有不同 GB 大小和 IOPS 的预定义存储层之间进行选择，以满足工作负载的需求。有关如何在集群中供应文件存储器的更多信息，请参阅[在 {{site.data.keyword.Bluemix_notm}} File Storage 上存储数据](/docs/containers?topic=containers-file_storage#file_storage)。</td>
</tr>
<tr>
<td>{{site.data.keyword.keymanagementservicefull}}</td>
<td>数据加密</td>
<td>通过启用 {{site.data.keyword.keymanagementserviceshort}}，可对集群中的 Kubernetes 私钥进行加密。对 Kubernetes 私钥进行加密可防止未经授权的用户访问敏感集群信息。<br>要进行设置，请参阅<a href="/docs/containers?topic=containers-encryption#keyprotect">使用 {{site.data.keyword.keymanagementserviceshort}} 加密 Kubernetes 私钥</a>。<br>有关更多信息，请参阅 <a href="/docs/services/key-protect?topic=key-protect-getting-started-tutorial" target="_blank">{{site.data.keyword.keymanagementserviceshort}} 文档 <img src="../icons/launch-glyph.svg" alt="外部链接图标"></a>。</td>
</tr>
<tr>
<td>{{site.data.keyword.la_full}}</td>
<td>集群和应用程序日志</td>
<td>通过将 LogDNA 作为第三方服务部署到工作程序节点来管理 pod 容器中的日志，从而将日志管理功能添加到集群。有关更多信息，请参阅[使用 {{site.data.keyword.loganalysisfull_notm}} 通过 LogDNA 管理 Kubernetes 集群日志](/docs/services/Log-Analysis-with-LogDNA/tutorials?topic=LogDNA-kube#kube)。</td>
</tr>
<tr>
<td>{{site.data.keyword.mon_full}}</td>
<td>集群和应用程序度量值</td>
<td>通过将 Sysdig 作为第三方服务部署到工作程序节点，以将度量值转发到 {{site.data.keyword.monitoringlong}}，从而从运营角度了解应用程序的性能和运行状况。有关更多信息，请参阅[分析在 Kubernetes 集群中部署的应用程序的度量值](/docs/services/Monitoring-with-Sysdig/tutorials?topic=Sysdig-kubernetes_cluster#kubernetes_cluster)。</td>
</tr>
<tr>
<td>{{site.data.keyword.cos_full}}</td>
<td>对象存储器</td>
<td>使用 {{site.data.keyword.cos_short}} 存储的数据经过加密，分散在多个地理位置，并使用 REST API 通过 HTTP 进行访问。您可以使用 [ibm-backup-restore 映像](/docs/services/RegistryImages/ibm-backup-restore?topic=RegistryImages-ibmbackup_restore_starter)来配置服务，以便为集群中的数据生成一次性备份或安排的备份。有关该服务的更多信息，请参阅 <a href="/docs/services/cloud-object-storage?topic=cloud-object-storage-about" target="_blank">{{site.data.keyword.cos_short}} 文档 <img src="../icons/launch-glyph.svg" alt="外部链接图标"></a>。</td>
</tr>
<tr>
<td>Istio on {{site.data.keyword.containerlong_notm}}</td>
<td>微服务管理</td>
<td><a href="https://www.ibm.com/cloud/info/istio" target="_blank">Istio <img src="../icons/launch-glyph.svg" alt="外部链接图标"></a> 是一种开放式源代码服务，开发者可用于连接、保护、管理和监视云编排平台上的微服务网络（也称为服务网）。Istio on {{site.data.keyword.containerlong}} 通过受管附加组件，可一步将 Istio 安装到集群中。只需单击一次，您就可以获取所有 Istio 核心组件，执行其他跟踪、监视和可视化，并使 BookInfo 样本应用程序启动并运行。首先，请参阅[使用受管 Istio 附加组件 (beta)](/docs/containers?topic=containers-istio)。 </td>
</tr>
<tr>
<td>Knative</td>
<td>无服务器应用程序</td>
<td>[Knative ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/knative/docs) 是一种开放式源代码平台，由 IBM、Google、Pivotal、Red Hat、Cisco 等公司联合开发，目标是扩展 Kubernetes 的功能，帮助您基于 Kubernetes 集群来创建以源代码为中心的现代容器化无服务器应用程序。对于不同的编程语言和框架，该平台会使用一致的方法来抽象化处理 Kubernetes 中有关构建、部署和管理工作负载的操作负担，让开发者可以专注于最重要的事情：源代码。有关更多信息，请参阅[使用 Knative 部署无服务器应用程序](/docs/containers?topic=containers-serverless-apps-knative)。</td>
</tr>
<tr>
<td>Portworx</td>
<td>存储有状态应用程序</td>
<td>[Portworx ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://portworx.com/products/introduction/) 是一种高可用性软件定义的存储解决方案，可用于管理容器化数据库和其他有状态应用程序的持久存储，或者在多个专区的 pod 之间共享数据。您可以使用 Helm chart 来安装 Portworx，并使用 Kubernetes 持久卷为应用程序供应存储器。有关如何在集群中设置 Portworx 的更多信息，请参阅[使用 Portworx 在软件定义的存储 (SDS) 上存储数据](/docs/containers?topic=containers-portworx#portworx)。</td>
</tr>
<tr>
<td>Razee</td>
<td>部署自动化</td>
<td>[Razee ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://razee.io/) 是一个开放式源代码项目，可自动执行并管理 Kubernetes 资源在不同集群、环境和云提供者中的部署，并帮助您直观显示资源的部署信息，以便可以监视应用过程，并更快地找到部署问题。有关 Razee 以及如何在集群中设置 Razee 以自动执行部署过程的更多信息，请参阅 [Razee 文档 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/razee-io/Razee)。</td>
</tr>
</tbody>
</table>

<br />


## DevOps 服务
{: #devops_services}

<table summary="该表显示了可以添加到集群以增加额外 DevOps 功能的可用服务。每行从左到右阅读，其中第一列是服务名称，第二列是服务描述。">
<caption>DevOps 服务</caption>
<thead>
<tr>
<th>服务</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.cfee_full_notm}}</td>
<td>基于 Kubernetes 集群来部署和管理您自己的 Cloud Foundry 平台，以开发、打包、部署和管理云本机应用程序，并利用 {{site.data.keyword.Bluemix_notm}} 生态系统将其他服务绑定到应用程序。创建 {{site.data.keyword.cfee_full_notm}} 实例时，必须通过选择工作程序节点的机器类型和 VLAN 来配置 Kubernetes 集群。随后，会为集群供应 {{site.data.keyword.containerlong_notm}}，并且 {{site.data.keyword.cfee_full_notm}} 会自动部署到集群。有关如何设置 {{site.data.keyword.cfee_full_notm}} 的更多信息，请参阅[入门教程](/docs/cloud-foundry?topic=cloud-foundry-getting-started#getting-started)。</td>
</tr>
<tr>
<td>Codeship</td>
<td>可以使用 <a href="https://codeship.com" target="_blank">Codeship <img src="../icons/launch-glyph.svg" alt="外部链接图标"></a> 来持续集成和交付容器。有关更多信息，请参阅 <a href="https://www.ibm.com/blogs/bluemix/2017/10/using-codeship-pro-deploy-workloads-ibm-container-service/" target="_blank">Using Codeship Pro To Deploy Workloads to {{site.data.keyword.containerlong_notm}} <img src="../icons/launch-glyph.svg" alt="外部链接图标"></a>。</td>
</tr>
<tr>
<td>Grafeas</td>
<td>[Grafeas ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://grafeas.io) 是一种开放式源代码 CI/CD 服务，提供了在软件供应链过程中检索、存储和交换元数据的通用方法。例如，如果将 Grafeas 集成到应用程序构建过程中，那么 Grafeas 可以存储有关构建请求发起者、漏洞扫描结果和质量保证签核的信息，以便您可以就应用程序能否部署到生产环境做出明智的决策。这些元数据可以用于审计，也可以用于证明软件供应链合规性。</td>
</tr>
<tr>
<td>Helm</td>
<td> <a href="https://helm.sh" target="_blank">Helm <img src="../icons/launch-glyph.svg" alt="外部链接图标"></a> 是 Kubernetes 软件包管理器。可以创建新的 Helm chart 或使用预先存在的 Helm chart 来定义、安装和升级在 {{site.data.keyword.containerlong_notm}} 集群中运行的复杂 Kubernetes 应用程序。<p>有关更多信息，请参阅[在 {{site.data.keyword.containerlong_notm}} 中设置 Helm](/docs/containers?topic=containers-helm)。</p></td>
</tr>
<tr>
<td>{{site.data.keyword.contdelivery_full}}</td>
<td>使用工具链自动构建应用程序并将容器部署到 Kubernetes 集群。有关设置的更多信息，请参阅博客 <a href="https://developer.ibm.com/recipes/tutorials/deploy-kubernetes-pods-to-the-bluemix-container-service-using-devops-pipelines/" target="_blank">Deploy Kubernetes pods to the {{site.data.keyword.containerlong_notm}} using DevOps Pipelines <img src="../icons/launch-glyph.svg" alt="外部链接图标"></a>。</td>
</tr>
<tr>
<td>Istio on {{site.data.keyword.containerlong_notm}}</td>
<td><a href="https://www.ibm.com/cloud/info/istio" target="_blank">Istio <img src="../icons/launch-glyph.svg" alt="外部链接图标"></a> 是一种开放式源代码服务，开发者可用于连接、保护、管理和监视云编排平台上的微服务网络（也称为服务网）。Istio on {{site.data.keyword.containerlong}} 通过受管附加组件，可一步将 Istio 安装到集群中。只需单击一次，您就可以获取所有 Istio 核心组件，执行其他跟踪、监视和可视化，并使 BookInfo 样本应用程序启动并运行。首先，请参阅[使用受管 Istio 附加组件 (beta)](/docs/containers?topic=containers-istio)。 </td>
</tr>
<tr>
<td>Jenkins X</td>
<td>Jenkins X 是一种 Kubernetes 本机持续集成和持续交付平台，可用于自动执行构建过程。有关如何在 {{site.data.keyword.containerlong_notm}} 上安装 Jenkins X 的更多信息，请参阅 [Introducing the Jenkins X open source project ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/blogs/bluemix/2018/08/installing-jenkins-x-on-ibm-cloud-kubernetes-service/)。</td>
</tr>
<tr>
<td>Knative</td>
<td>[Knative ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/knative/docs) 是一种开放式源代码平台，由 IBM、Google、Pivotal、Red Hat、Cisco 等公司联合开发，目标是扩展 Kubernetes 的功能，帮助您基于 Kubernetes 集群来创建以源代码为中心的现代容器化无服务器应用程序。对于不同的编程语言和框架，该平台会使用一致的方法来抽象化处理 Kubernetes 中有关构建、部署和管理工作负载的操作负担，让开发者可以专注于最重要的事情：源代码。有关更多信息，请参阅[使用 Knative 部署无服务器应用程序](/docs/containers?topic=containers-serverless-apps-knative)。</td>
</tr>
<tr>
<td>Razee</td>
<td>[Razee ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://razee.io/) 是一个开放式源代码项目，可自动执行并管理 Kubernetes 资源在不同集群、环境和云提供者中的部署，并帮助您直观显示资源的部署信息，以便可以监视应用过程，并更快地找到部署问题。有关 Razee 以及如何在集群中设置 Razee 以自动执行部署过程的更多信息，请参阅 [Razee 文档 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/razee-io/Razee)。</td>
</tr>
</tbody>
</table>

<br />


## 混合云服务
{: #hybrid_cloud_services}

<table summary="该表显示了可以将集群连接到内部部署数据中心的可用服务。每行从左到右阅读，其中第一列是服务名称，第二列是服务描述。">
<caption>混合云服务</caption>
<thead>
<tr>
<th>服务</th>
<th>描述</th>
</tr>
</thead>
<tbody>
  <tr>
    <td>{{site.data.keyword.BluDirectLink}}</td>
    <td>[{{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-about-ibm-cloud-direct-link) 允许您在远程网络环境和 {{site.data.keyword.containerlong_notm}} 之间创建直接专用连接，而无需通过公用因特网进行路由。必须实现混合工作负载、跨提供者工作负载、大型或频繁数据传输或者专用工作负载时，{{site.data.keyword.Bluemix_notm}} Direct Link 产品非常有用。要选择 {{site.data.keyword.Bluemix_notm}} Direct Link 产品并设置 {{site.data.keyword.Bluemix_notm}} Direct Link 连接，请参阅 {{site.data.keyword.Bluemix_notm}} Direct Link 文档中的 [{{site.data.keyword.Bluemix_notm}} Direct Link 入门](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link#how-do-i-know-which-type-of-ibm-cloud-direct-link-i-need-)。</td>
  </tr>
<tr>
  <td>strongSwan IPSec VPN 服务</td>
  <td>设置 [strongSwan IPSec VPN 服务 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.strongswan.org/about.html)，以将 Kubernetes 集群与内部部署网络安全连接。strongSwan IPSec VPN 服务基于业界标准因特网协议安全性 (IPSec) 协议组，通过因特网提供安全的端到端通信信道。要在集群与内部部署网络之间设置安全连接，请在集群的 pod 中直接[配置和部署 strongSwan IPSec VPN 服务](/docs/containers?topic=containers-vpn#vpn-setup)。</td>
  </tr>
  </tbody>
  </table>

<br />


## 日志记录和监视服务
{: #health_services}
<table summary="该表显示了可以添加到集群以增加额外日志记录和监视功能的可用服务。每行从左到右阅读，其中第一列是服务名称，第二列是服务描述。">
<caption>日志记录和监视服务</caption>
<thead>
<tr>
<th>服务</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>CoScale</td>
<td>使用 <a href="https://www.newrelic.com/coscale" target="_blank">CoScale <img src="../icons/launch-glyph.svg" alt="外部链接图标"></a> 可监视工作程序节点、容器、副本集、复制控制器和服务。有关更多信息，请参阅 <a href="https://www.ibm.com/blogs/bluemix/2017/06/monitoring-ibm-bluemix-container-service-coscale/" target="_blank">Monitoring {{site.data.keyword.containerlong_notm}} with CoScale <img src="../icons/launch-glyph.svg" alt="外部链接图标"></a>。</td>
</tr>
<tr>
<td>Datadog</td>
<td>使用 <a href="https://www.datadoghq.com/" target="_blank">Datadog <img src="../icons/launch-glyph.svg" alt="外部链接图标"></a> 可监视集群并查看基础架构和应用程序性能度量值。有关更多信息，请参阅 <a href="https://www.ibm.com/blogs/bluemix/2017/07/monitoring-ibm-bluemix-container-service-datadog/" target="_blank">Monitoring {{site.data.keyword.containerlong_notm}} with Datadog <img src="../icons/launch-glyph.svg" alt="外部链接图标"></a>。</td>
</tr>
<tr>
<td>{{site.data.keyword.cloudaccesstrailfull}}</td>
<td>通过 Grafana 来分析日志，以监视集群中进行的管理活动。有关服务的更多信息，请参阅 [Activity Tracker](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started) 文档。有关可以跟踪的事件类型的更多信息，请参阅 [Activity Tracker 事件](/docs/containers?topic=containers-at_events)。</td>
</tr>
<tr>
<td>{{site.data.keyword.la_full_notm}}</td>
<td>通过将 LogDNA 作为第三方服务部署到工作程序节点来管理 pod 容器中的日志，从而将日志管理功能添加到集群。有关更多信息，请参阅[使用 {{site.data.keyword.loganalysisfull_notm}} 通过 LogDNA 管理 Kubernetes 集群日志](/docs/services/Log-Analysis-with-LogDNA/tutorials?topic=LogDNA-kube#kube)。</td>
</tr>
<tr>
<td>{{site.data.keyword.mon_full_notm}}</td>
<td>通过将 Sysdig 作为第三方服务部署到工作程序节点，以将度量值转发到 {{site.data.keyword.monitoringlong}}，从而从运营角度了解应用程序的性能和运行状况。有关更多信息，请参阅[分析在 Kubernetes 集群中部署的应用程序的度量值](/docs/services/Monitoring-with-Sysdig/tutorials?topic=Sysdig-kubernetes_cluster#kubernetes_cluster)。</td>
</tr>
<tr>
<td>Instana</td>
<td> <a href="https://www.instana.com/" target="_blank">Instana <img src="../icons/launch-glyph.svg" alt="外部链接图标"></a> 通过 GUI 自动发现和映射应用程序，从而提供基础架构和应用程序性能监视。Instana 会捕获向应用程序发出的每一个请求，您可以利用这些信息进行故障诊断并执行根本原因分析，以防止问题再次发生。请查看有关<a href="https://www.instana.com/blog/precise-visibility-applications-ibm-bluemix-container-service/" target="_blank">在 {{site.data.keyword.containerlong_notm}} 中部署 Instana <img src="../icons/launch-glyph.svg" alt="外部链接图标"></a> 的博客帖子，以了解更多信息。</td>
</tr>
<tr>
<td>Prometheus</td>
<td>Prometheus 是一个开放式源代码监视、日志记录和警报工具，专为 Kubernetes 而设计。Prometheus 基于 Kubernetes 日志记录信息来检索有关集群、工作程序节点和部署运行状况的详细信息。针对集群中运行中的每个容器，都会收集 CPU、内存、I/O 和网络活动。可以在定制查询或警报中使用收集的数据，以监视集群中的性能和工作负载。

<p>要使用 Prometheus，请遵循 <a href="https://github.com/coreos/prometheus-operator/tree/master/contrib/kube-prometheus" target="_blank">CoreOS 指示信息 <img src="../icons/launch-glyph.svg" alt="外部链接图标"></a>。</p>
</td>
</tr>
<tr>
<td>Sematext</td>
<td>使用 <a href="https://sematext.com/" target="_blank">Sematext <img src="../icons/launch-glyph.svg" alt="外部链接图标"></a> 可查看容器化应用程序的度量值和日志。有关更多信息，请参阅 <a href="https://www.ibm.com/blogs/bluemix/2017/09/monitoring-logging-ibm-bluemix-container-service-sematext/" target="_blank">Monitoring and logging for containers with Sematext <img src="../icons/launch-glyph.svg" alt="外部链接图标"></a>。</td>
</tr>
<tr>
<td>Splunk</td>
<td>使用 Splunk Connect for Kubernetes，可在 Splunk 中导入和搜索 Kubernetes 日志记录、对象和度量数据。Splunk Connect for Kubernetes 是由 Helm chart（用于将 Splunk 支持的 Flunk 部署部署到 Kubernetes 集群）、Splunk 构建的 Fluentd HTTP Event Collector (HEC) 插件（用于发送日志和元数据）以及度量值部署（用于捕获集群度量值）构成的集合。有关更多信息，请参阅 <a href="https://www.ibm.com/blogs/bluemix/2019/02/solving-business-problems-with-splunk-on-ibm-cloud-kubernetes-service/" target="_blank">Solving Business Problems with Splunk on {{site.data.keyword.containerlong_notm}} <img src="../icons/launch-glyph.svg" alt="外部链接图标"></a>。</td>
</tr>
<tr>
<td>Weave Scope</td>
<td>[Weave Scope ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.weave.works/oss/scope/) 提供了 Kubernetes 集群内资源（包括服务、 pod、容器、进程、节点等等）的直观图。此外，Weave Scope 还提供了 CPU 和内存的交互式度量值，以及用于跟踪和执行到容器中的多种工具。</li></ol>
</td>
</tr>
</tbody>
</table>

<br />



## 安全服务
{: #security_services}

想要全面了解如何将 {{site.data.keyword.Bluemix_notm}} 安全服务与集群集成吗？请查看[将端到端安全性应用于云应用程序教程](/docs/tutorials?topic=solution-tutorials-cloud-e2e-security)。
{: shortdesc}

<table summary="该表显示了可以添加到集群以增加额外安全功能的可用服务。每行从左到右阅读，其中第一列是服务名称，第二列是服务描述。">
<caption>安全服务</caption>
<thead>
<tr>
<th>服务</th>
<th>描述</th>
</tr>
</thead>
<tbody>
  <tr>
    <td>{{site.data.keyword.appid_full}}</td>
    <td>使用 [{{site.data.keyword.appid_short}}](/docs/services/appid?topic=appid-getting-started) 并要求用户登录，提高了应用程序的安全级别。要向应用程序认证 Web 或 API HTTP/HTTPS 请求，可以使用 [{{site.data.keyword.appid_short_notm}} 认证 Ingress 注释](/docs/containers?topic=containers-ingress_annotation#appid-auth)将 {{site.data.keyword.appid_short_notm}} 与 Ingress 服务集成。</td>
  </tr>
<tr>
<td>Aqua 安全性</td>
  <td>作为<a href="/docs/services/va?topic=va-va_index" target="_blank">漏洞顾问程序</a>的补充，可以使用 <a href="https://www.aquasec.com/" target="_blank">Aqua 安全性 <img src="../icons/launch-glyph.svg" alt="外部链接图标"></a>，通过减少允许应用程序执行的操作来提高容器部署的安全性。有关更多信息，请参阅 <a href="https://blog.aquasec.com/securing-container-deployments-on-bluemix-with-aqua-security" target="_blank">Securing container deployments on {{site.data.keyword.Bluemix_notm}} with Aqua Security <img src="../icons/launch-glyph.svg" alt="外部链接图标"></a>。</td>
</tr>
<tr>
<td>{{site.data.keyword.cloudcerts_full}}</td>
<td>可以使用 <a href="/docs/services/certificate-manager?topic=certificate-manager-getting-started#getting-started" target="_blank">{{site.data.keyword.cloudcerts_long}} <img src="../icons/launch-glyph.svg" alt="外部链接图标"></a> 来存储和管理应用程序的 SSL 证书。有关更多信息，请参阅 <a href="https://www.ibm.com/blogs/bluemix/2018/01/use-ibm-cloud-certificate-manager-ibm-cloud-container-service-deploy-custom-domain-tls-certificates/" target="_blank">Use {{site.data.keyword.cloudcerts_long_notm}} with {{site.data.keyword.containerlong_notm}} to deploy custom domain TLS Certificates <img src="../icons/launch-glyph.svg" alt="外部链接图标"></a>。</td>
</tr>
<tr>
  <td>{{site.data.keyword.datashield_full}} (Beta)</td>
  <td>可以使用 <a href="/docs/services/data-shield?topic=data-shield-getting-started#getting-started" target="_blank">{{site.data.keyword.datashield_short}} <img src="../icons/launch-glyph.svg" alt="外部链接图标"></a> 来加密数据内存。{{site.data.keyword.datashield_short}} 与 Intel® Software Guard Extensions (SGX) 和 Fortanix® 技术相集成，以便保护使用中的 {{site.data.keyword.Bluemix_notm}} 容器工作负载代码和数据。应用程序代码和数据在 CPU 固化的相关可调度单元组中运行，这是工作程序节点上用于保护应用程序关键方面的可信内存区域，有助于使代码和数据保持机密性且不被修改。</td>
</tr>
<tr>
  <td>{{site.data.keyword.registrylong}}</td>
  <td>设置您自己的安全 Docker 映像存储库，在其中可以安全地存储映像并在集群用户之间共享这些映像。有关更多信息，请参阅 <a href="/docs/services/Registry?topic=registry-getting-started" target="_blank">{{site.data.keyword.registrylong}} 文档 <img src="../icons/launch-glyph.svg" alt="外部链接图标"></a>。</td>
</tr>
<tr>
  <td>{{site.data.keyword.keymanagementservicefull}}</td>
  <td>通过启用 {{site.data.keyword.keymanagementserviceshort}}，可对集群中的 Kubernetes 私钥进行加密。对 Kubernetes 私钥进行加密可防止未经授权的用户访问敏感集群信息。<br>要进行设置，请参阅<a href="/docs/containers?topic=containers-encryption#keyprotect">使用 {{site.data.keyword.keymanagementserviceshort}} 加密 Kubernetes 私钥</a>。<br>有关更多信息，请参阅 <a href="/docs/services/key-protect?topic=key-protect-getting-started-tutorial" target="_blank">{{site.data.keyword.keymanagementserviceshort}} 文档 <img src="../icons/launch-glyph.svg" alt="外部链接图标"></a>。</td>
</tr>
<tr>
<td>NeuVector</td>
<td>使用 <a href="https://neuvector.com/" target="_blank">NeuVector <img src="../icons/launch-glyph.svg" alt="外部链接图标"></a> 可通过云本机防火墙来保护容器。有关更多信息，请参阅 <a href="https://www.ibm.com/us-en/marketplace/neuvector-container-security" target="_blank">NeuVector Container Security <img src="../icons/launch-glyph.svg" alt="外部链接图标"></a>。</td>
</tr>
<tr>
<td>Twistlock</td>
<td>作为<a href="/docs/services/va?topic=va-va_index" target="_blank">漏洞顾问程序</a>的补充，可以使用 <a href="https://www.twistlock.com/" target="_blank">Twistlock <img src="../icons/launch-glyph.svg" alt="外部链接图标"></a> 来管理防火墙、威胁防御和事件响应。有关更多信息，请参阅 <a href="https://www.ibm.com/blogs/bluemix/2017/07/twistlock-ibm-bluemix-container-service/" target="_blank">Twistlock on {{site.data.keyword.containerlong_notm}} <img src="../icons/launch-glyph.svg" alt="外部链接图标"></a>。</td>
</tr>
</tbody>
</table>

<br />



## 存储服务
{: #storage_services}
<table summary="该表显示了可以添加到集群以增加持久性存储功能的可用服务。每行从左到右阅读，其中第一列是服务名称，第二列是服务描述。">
<caption>存储服务</caption>
<thead>
<tr>
<th>服务</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
  <td>Heptio Velero</td>
  <td>可以使用 <a href="https://github.com/heptio/velero" target="_blank">Heptio Velero <img src="../icons/launch-glyph.svg" alt="外部链接图标"></a> 来备份和复原集群资源和持久卷。有关更多信息，请参阅 Heptio Velero <a href="https://github.com/heptio/velero/blob/release-0.9/docs/use-cases.md" target="_blank">Use cases for disaster recovery and cluster migration <img src="../icons/launch-glyph.svg" alt="外部链接图标"></a>。</td>
</tr>
<tr>
  <td>{{site.data.keyword.Bluemix_notm}} Block Storage</td>
  <td>[{{site.data.keyword.Bluemix_notm}} Block Storage](/docs/infrastructure/BlockStorage?topic=BlockStorage-getting-started#getting-started) 是一种持久的高性能 iSCSI 存储器，可以使用 Kubernetes 持久卷 (PV) 添加到应用程序。使用块存储器可在单个专区中部署有状态的应用程序，或者用于部署为单个 pod 的高性能存储器。有关如何在集群中供应块存储器的更多信息，请参阅[在 {{site.data.keyword.Bluemix_notm}} Block Storage 上存储数据](/docs/containers?topic=containers-block_storage#block_storage)。</td>
  </tr>
<tr>
  <td>{{site.data.keyword.cos_full}}</td>
  <td>使用 {{site.data.keyword.cos_short}} 存储的数据经过加密，分散在多个地理位置，并使用 REST API 通过 HTTP 进行访问。您可以使用 [ibm-backup-restore 映像](/docs/services/RegistryImages/ibm-backup-restore?topic=RegistryImages-ibmbackup_restore_starter)来配置服务，以便为集群中的数据生成一次性备份或安排的备份。有关该服务的更多信息，请参阅 <a href="/docs/services/cloud-object-storage?topic=cloud-object-storage-about" target="_blank">{{site.data.keyword.cos_short}} 文档 <img src="../icons/launch-glyph.svg" alt="外部链接图标"></a>。</td>
</tr>
  <tr>
  <td>{{site.data.keyword.Bluemix_notm}} File Storage</td>
  <td>[{{site.data.keyword.Bluemix_notm}} File Storage](/docs/infrastructure/FileStorage?topic=FileStorage-getting-started#getting-started) 是一种基于 NFS 的持久、快速、灵活的网络连接文件存储器，可以使用 Kubernetes 持久卷添加到应用程序。您可以在具有不同 GB 大小和 IOPS 的预定义存储层之间进行选择，以满足工作负载的需求。有关如何在集群中供应文件存储器的更多信息，请参阅[在 {{site.data.keyword.Bluemix_notm}} File Storage 上存储数据](/docs/containers?topic=containers-file_storage#file_storage)。</td>
  </tr>
  <tr>
    <td>Portworx</td>
    <td>[Portworx ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://portworx.com/products/introduction/) 是一种高可用性软件定义的存储解决方案，可用于管理容器化数据库和其他有状态应用程序的持久存储，或者在多个专区的 pod 之间共享数据。您可以使用 Helm chart 来安装 Portworx，并使用 Kubernetes 持久卷为应用程序供应存储器。有关如何在集群中设置 Portworx 的更多信息，请参阅[使用 Portworx 在软件定义的存储 (SDS) 上存储数据](/docs/containers?topic=containers-portworx#portworx)。</td>
  </tr>
</tbody>
</table>

<br />


## 数据库服务
{: #database_services}

<table summary="该表显示了可以添加到集群以增加数据库功能的可用服务。每行从左到右阅读，其中第一列是服务名称，第二列是服务描述。">
<caption>数据库服务</caption>
<thead>
<tr>
<th>服务</th>
<th>描述</th>
</tr>
</thead>
<tbody>
  <tr>
    <td>{{site.data.keyword.blockchainfull_notm}} Platform 2.0 Beta</td>
    <td>在 {{site.data.keyword.containerlong_notm}} 上部署和管理您自己的 {{site.data.keyword.blockchainfull_notm}} Platform。通过 {{site.data.keyword.blockchainfull_notm}} Platform 2.0，您可以托管 {{site.data.keyword.blockchainfull_notm}} 网络，或创建可加入其他 {{site.data.keyword.blockchainfull_notm}} 2.0 网络的组织。有关如何在 {{site.data.keyword.containerlong_notm}} 中设置 {{site.data.keyword.blockchainfull_notm}} 的更多信息，请参阅[关于 {{site.data.keyword.blockchainfull_notm}} Platform Free 2.0 Beta](/docs/services/blockchain?topic=blockchain-ibp-console-overview#ibp-console-overview)。</td>
  </tr>
<tr>
  <td>云数据库</td>
  <td>可以在各种 {{site.data.keyword.Bluemix_notm}} 数据库服务（如 {{site.data.keyword.composeForMongoDB_full}} 或 {{site.data.keyword.cloudantfull}}）之间进行选择，以在集群中部署高度可用、可缩放的数据库解决方案。有关可用云数据库的列表，请参阅 [{{site.data.keyword.Bluemix_notm}} 目录 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/catalog?category=databases)。</td>
  </tr>
  </tbody>
  </table>
