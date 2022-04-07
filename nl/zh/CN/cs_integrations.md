---

copyright:
  years: 2014, 2019
lastupdated: "2019-04-09"

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


# 集成服务
{: #integrations}

您可以将各种外部服务和目录服务用于 {{site.data.keyword.containerlong}} 中的标准 Kubernetes 集群。
{:shortdesc}


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
<td>在 Kubernetes 集群上部署和管理您自己的 Cloud Foundry 平台，以开发、打包、部署和管理云本机应用程序，并利用 {{site.data.keyword.Bluemix_notm}} 生态系统将其他服务绑定到应用程序。创建 {{site.data.keyword.cfee_full_notm}} 实例时，必须通过选择工作程序节点的机器类型和 VLAN 来配置 Kubernetes 集群。随后，会为集群供应 {{site.data.keyword.containerlong_notm}}，并且 {{site.data.keyword.cfee_full_notm}} 会自动部署到集群。有关如何设置 {{site.data.keyword.cfee_full_notm}} 的更多信息，请参阅[入门教程](/docs/cloud-foundry?topic=cloud-foundry-getting-started#getting-started)。</td>
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
<td> <a href="https://helm.sh" target="_blank">Helm <img src="../icons/launch-glyph.svg" alt="外部链接图标"></a> 是 Kubernetes 软件包管理器。可以创建新的 Helm 图表或使用预先存在的 Helm 图表来定义、安装和升级在 {{site.data.keyword.containerlong_notm}} 集群中运行的复杂 Kubernetes 应用程序。<p>有关更多信息，请参阅[在 {{site.data.keyword.containerlong_notm}} 中设置 Helm](/docs/containers?topic=containers-integrations#helm)。</p></td>
</tr>
<tr>
<td>{{site.data.keyword.contdelivery_full}}</td>
<td>使用工具链自动构建应用程序并将容器部署到 Kubernetes 集群。有关设置信息，请参阅博客<a href="https://developer.ibm.com/recipes/tutorials/deploy-kubernetes-pods-to-the-bluemix-container-service-using-devops-pipelines/" target="_blank">使用 DevOps 管道将 Kubernetes pod 部署到 {{site.data.keyword.containerlong_notm}} <img src="../icons/launch-glyph.svg" alt="外部链接图标"></a>。</td>
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
<td>[Knative ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/knative/docs) 是一种开放式源代码平台，由 IBM、Google、Pivotal、Red Hat、Cisco 等公司联合开发，目标是扩展 Kubernetes 的功能，帮助您在 Kubernetes 集群上创建以源代码为中心的现代容器化无服务器应用程序。该平台在不同编程语言和框架之间使用一致的方法，对构建、部署和管理 Kubernetes 中工作负载的操作工作进行抽象化处理，以便开发者可以专注于最重要的事项：源代码。有关更多信息，请参阅[教程：使用受管 Knative 在 Kubernetes 集群中运行无服务器应用程序](/docs/containers?topic=containers-knative_tutorial#knative_tutorial)。</td>
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
<td>通过 Grafana 来分析日志，以监视集群中进行的管理活动。有关服务的更多信息，请参阅 [Activity Tracker](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started-with-cla) 文档。有关可以跟踪的事件类型的更多信息，请参阅 [Activity Tracker 事件](/docs/containers?topic=containers-at_events)。</td>
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
<td>Weave Scope 提供了 Kubernetes 集群内资源（包括服务、pod、容器、进程、节点等等）的可视图。此外，Weave Scope 还提供了 CPU 和内存的交互式度量值，以及用于跟踪和执行到容器中的多种工具。<p>有关更多信息，请参阅[使用 Weave Scope 和 {{site.data.keyword.containerlong_notm}} 可视化 Kubernetes 集群资源](/docs/containers?topic=containers-integrations#weavescope)。</p></li></ol>
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
  <tr id="appid">
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
  <td>使用 {{site.data.keyword.cos_short}} 存储的数据经过加密，分散在多个地理位置，并使用 REST API 通过 HTTP 进行访问。您可以使用 [ibm-backup-restore 映像](/docs/services/RegistryImages/ibm-backup-restore?topic=RegistryImages-ibmbackup_restore_starter)来配置服务，以便为集群中的数据生成一次性备份或安排的备份。有关该服务的常规信息，请参阅 <a href="/docs/services/cloud-object-storage?topic=cloud-object-storage-about-ibm-cloud-object-storage" target="_blank">{{site.data.keyword.cos_short}} 文档 <img src="../icons/launch-glyph.svg" alt="外部链接图标"></a>。</td>
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


## 将 {{site.data.keyword.Bluemix_notm}} 服务添加到集群
{: #adding_cluster}

添加 {{site.data.keyword.Bluemix_notm}} 服务以使用区域中的额外功能增强 Kubernetes 集群，例如，Watson AI、数据、安全性和物联网 (IoT)。
{:shortdesc}

只能绑定支持服务密钥的服务。要查找支持服务密钥的服务的列表，请参阅[使外部应用程序能够使用 {{site.data.keyword.Bluemix_notm}} 服务](/docs/resources?topic=resources-externalapp#externalapp)。
{: note}

开始之前：
- 确保您具有以下角色：
    - 对集群的 [{{site.data.keyword.Bluemix_notm}} IAM **编辑者**或**管理员**平台角色](/docs/containers?topic=containers-users#platform)
    - 对要绑定服务的名称空间的 [{{site.data.keyword.Bluemix_notm}} IAM **写入者**或**管理者**服务角色](/docs/containers?topic=containers-users#platform)。
    - 对要使用的空间的 [Cloud Foundry **开发者**角色](/docs/iam?topic=iam-mngcf#mngcf)。
- [登录到您的帐户。将相应的区域和（如果适用）资源组设定为目标。设置集群的上下文](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)。

要将 {{site.data.keyword.Bluemix_notm}} 服务添加到集群：

1. [创建 {{site.data.keyword.Bluemix_notm}} 服务的实例](/docs/resources?topic=resources-externalapp#externalapp)。
    * 某些 {{site.data.keyword.Bluemix_notm}} 服务仅在精选区域中可用。仅当服务在与您的集群相同的区域中可用时，才可将服务绑定到集群。此外，如果想要在华盛顿专区中创建服务实例，必须使用 CLI。
    * 必须在集群所在的资源组中创建服务实例。只能在一个资源组中创建资源，并且在此之后无法更改资源组。

2. 检查创建的服务类型，并记下服务实例**名称**。
   - **Cloud Foundry 服务：**
     ```
    ibmcloud service list
    ```
     {: pre}

     输出示例：
     ```
     name                         service           plan    bound apps   last operation
     <cf_service_instance_name>   <service_name>    spark                create succeeded
     ```
     {: screen}

  - **{{site.data.keyword.Bluemix_notm}} 启用 IAM 的服务：**
     ```
    ibmcloud resource service-instances
    ```
     {: pre}

     输出示例：
     ```
     Name                          Location   State    Type               Tags
     <iam_service_instance_name>   <region>   active   service_instance
     ```
     {: screen}

   您还可以在仪表板中查看作为 **Cloud Foundry 服务**和**服务**的不同服务类型。

3. 确定要用于添加服务的集群名称空间。在以下选项之间进行选择。
     ```
        kubectl get namespaces
        ```
     {: pre}

4.  使用 `ibmcloud ks cluster-service-bind` [命令](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_service_bind)将服务添加到集群。对于 {{site.data.keyword.Bluemix_notm}} 启用 IAM 的服务，确保使用先前创建的 Cloud Foundry 别名。
    对于启用 IAM 的服务，您还可以使用缺省**写入者**服务访问角色，或者使用 `--role` 标志来指定服务访问角色。以下命令将为服务实例创建服务密钥，或者可以包含 `--key` 标志以使用现有服务密钥凭证。如果使用 `--key` 标志，请不要包含 `--role` 标志。
    ```
    ibmcloud ks cluster-service-bind --cluster <cluster_name_or_ID> --namespace <namespace> --service <service_instance_name> [--key <service_instance_key>] [--role <IAM_service_role>]
    ```
    {: pre}

    服务成功添加到集群后，将创建集群私钥，用于保存服务实例的凭证。将在 etcd 中自动加密私钥以保护数据。

    输出示例：
    ```
    ibmcloud ks cluster-service-bind --cluster mycluster --namespace mynamespace --service cleardb
    Binding service instance to namespace...
    OK
    Namespace:	     mynamespace
    Secret name:     binding-<service_instance_name>
    ```
    {: screen}

5.  验证 Kubernetes 私钥中的服务凭证。
    1. 获取私钥的详细信息并记录 **binding** 值。**binding** 值采用 base64 编码，并且以 JSON 格式保存服务实例的凭证。
       ```
       kubectl get secrets binding-<service_instance_name> --namespace=<namespace> -o yaml
       ```
       {: pre}

       输出示例：
       ```
       apiVersion: v1
       data:
         binding: <binding>
       kind: Secret
       metadata:
         annotations:
           service-instance-id: 1111aaaa-a1aa-1aa1-1a11-111aa111aa11
           service-key-id: 2b22bb2b-222b-2bb2-2b22-b22222bb2222
         creationTimestamp: 2018-08-07T20:47:14Z
         name: binding-<service_instance_name>
         namespace: <namespace>
         resourceVersion: "6145900"
         selfLink: /api/v1/namespaces/default/secrets/binding-mycloudant
         uid: 33333c33-3c33-33c3-cc33-cc33333333c
       type: Opaque
       ```
       {: screen}

    2. 对绑定值进行解码。
       ```
       echo "<binding>" | base64 -D
       ```
       {: pre}

       输出示例：
       ```
       {"apikey":"KL34Ys893284NGJEPFjgrioJ12NElpow","host":"98765aab-9ce1-7tr3-ba87-bfbab6e6d9d6-bluemix.cloudant.com","iam_apikey_description":"Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:cloudantnosqldb:us-south:a/1234g56789cfe8e6388dd2ec098:98746cw-43d7-49ce-947a-d8fe3eebb381::","iam_apikey_name":"auto-generated-apikey-1234abcde-987f-3t64-9d96-d13775ec5663","iam_role_crn":"crn:v1:bluemix:public:iam::::serviceRole:Writer","iam_serviceid_crn":"crn:v1:bluemix:public:iam-identity::a/1234567890brasge5htn2ec098::serviceid:ServiceId-12345vgh-6c4c-ytr12-af6b-467d30d6ef44","password":"jfiavhui12484fnivhuo472nvei23913c3ff","port":443,"url":"https://25c73aac-9ce1-4c24-ba98-bfbab6e6d9d6-bluemix:ugvioev823inreuiegn43donvri29989wiu9t22@25c73aac-9ce1-4c24-ba98-abdrjio123562lnsb-bluemix.cloudant.com","username":"123b45da-9ce1-4c24-ab12-rinwnwub1294-bluemix"}
       ```
       {: screen}

    3. 可选：在 {{site.data.keyword.Bluemix_notm}} 仪表板中，将在先前步骤中解码的服务凭证与针对服务实例找到的服务凭证进行比较。

6. 现在，您的服务已绑定到集群，必须配置应用程序以[访问 Kubernetes 私钥中的服务凭证](#adding_app)。


## 从应用程序访问服务凭证
{: #adding_app}

要从应用程序访问 {{site.data.keyword.Bluemix_notm}} 服务实例，必须使存储在 Kubernetes 私钥中的服务凭证可用于应用程序。
{: shortdesc}

服务实例凭证采用 base64 编码并且以 JSON 格式存储在私钥中。要访问私钥中的数据，请从以下选项中进行选择：
- [将私钥作为卷安装到 pod](#mount_secret)
- [在环境变量中引用私钥](#reference_secret)
<br>
想要使私钥更安全吗？请要求集群管理员在集群中[启用 {{site.data.keyword.keymanagementservicefull}}](/docs/containers?topic=containers-encryption#keyprotect)，以加密新私钥和现有私钥，例如用于存储 {{site.data.keyword.Bluemix_notm}} 服务实例凭证的私钥。
{: tip}

开始前：

-  确保您具有对 `kube-system` 名称空间的 [{{site.data.keyword.Bluemix_notm}} IAM **写入者**或**管理者**服务角色](/docs/containers?topic=containers-users#platform)。
- [登录到您的帐户。将相应的区域和（如果适用）资源组设定为目标。设置集群的上下文](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)。
- [向集群添加 {{site.data.keyword.Bluemix_notm}} 服务](#adding_cluster)。

### 将私钥作为卷安装到 pod
{: #mount_secret}

将私钥作为卷安装到 pod 时，会将名为 `binding` 的文件存储在卷安装目录中。JSON 格式的 `binding` 文件包含访问 {{site.data.keyword.Bluemix_notm}} 服务所需的全部信息和凭证。
{: shortdesc}

1.  列出集群中的可用私钥并记下私钥的 **name**。查找类型为 **Opaque** 的私钥。如果存在多个私钥，请联系集群管理员来确定正确的服务私钥。

    ```
    kubectl get secrets
    ```
    {: pre}

    输出示例：

    ```
    NAME                                    TYPE                                  DATA      AGE
    binding-<service_instance_name>         Opaque                                1         3m

    

    ```
    {: screen}

2.  针对 Kubernetes 部署创建 YAML 文件并将私钥作为卷安装到 pod。
    ```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      labels:
        app: secret-test
      name: secret-test
      namespace: <my_namespace>
    spec:
      selector:
        matchLabels:
          app: secret-test
      replicas: 1
      template:
        metadata:
          labels:
            app: secret-test
        spec:
          containers:
          - image: registry.bluemix.net/ibmliberty:latest
            name: secret-test
            volumeMounts:
            - mountPath: <mount_path>
              name: <volume_name>
          volumes:
          - name: <volume_name>
            secret:
              defaultMode: 420
              secretName: binding-<service_instance_name>
    ```
    {: codeblock}

    <table>
    <caption>了解 YAML 文件的组成部分</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解 YAML 文件的组成部分</th>
    </thead>
    <tbody>
    <tr>
    <td><code>volumeMounts.mountPath</code></td>
    <td>在容器中安装卷的目录的绝对路径。</td>
    </tr>
    <tr>
    <td><code>volumeMounts.name</code></br><code>volumes.name</code></td>
    <td>要安装到 pod 中的卷的名称。</td>
    </tr>
    <tr>
    <td><code>secret.defaultMode</code></td>
    <td>私钥的读写许可权。使用 `420` 可设置只读许可权。</td>
    </tr>
    <tr>
    <td><code>secret.secretName</code></td>
    <td>先前步骤中记录的私钥的名称。</td>
    </tr></tbody></table>

3.  创建 pod 并安装私钥作为卷。
    ```
    kubectl apply -f secret-test.yaml
    ```
    {: pre}

4.  验证 pod 是否已创建。
    ```
    kubectl get pods
    ```
    {: pre}

    示例 CLI 输出：

    ```
    NAME                           READY     STATUS    RESTARTS   AGE
    secret-test-1111454598-gfx32   1/1       Running   0          1m
    ```
    {: screen}

5.  访问服务凭证。
    1. 登录到 pod。
       ```
       kubectl exec <pod_name> -it bash
       ```
       {: pre}

    2. 浏览至先前定义的卷安装路径并列出卷安装路径中的文件。
       ```
       cd <volume_mountpath> && ls
       ```
       {: pre}

       输出示例：
       ```
       binding
       ```
       {: screen}

       `binding` 文件包含存储在 Kubernetes 私钥中的服务凭证。

    4. 查看服务凭证。凭证以 JSON 格式存储为键值对。
       ```
       cat binding
       ```
       {: pre}

       输出示例：
       ```
       {"apikey":"KL34Ys893284NGJEPFjgrioJ12NElpow","host":"98765aab-9ce1-7tr3-ba87-bfbab6e6d9d6-bluemix.cloudant.com","iam_apikey_description":"Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:cloudantnosqldb:us-south:a/1234g56789cfe8e6388dd2ec098:98746cw-43d7-49ce-947a-d8fe3eebb381::","iam_apikey_name":"auto-generated-apikey-1234abcde-987f-3t64-9d96-d13775ec5663","iam_role_crn":"crn:v1:bluemix:public:iam::::serviceRole:Writer","iam_serviceid_crn":"crn:v1:bluemix:public:iam-identity::a/1234567890brasge5htn2ec098::serviceid:ServiceId-12345vgh-6c4c-ytr12-af6b-467d30d6ef44","password":"jfiavhui12484fnivhuo472nvei23913c3ff","port":443,"url":"https://25c73aac-9ce1-4c24-ba98-bfbab6e6d9d6-bluemix:ugvioev823inreuiegn43donvri29989wiu9t22@25c73aac-9ce1-4c24-ba98-abdrjio123562lnsb-bluemix.cloudant.com","username":"123b45da-9ce1-4c24-ab12-rinwnwub1294-bluemix"}
       ```
       {: screen}

    5. 配置应用程序以解析 JSON 内容，并检索访问服务所需的信息。


### 在环境变量中引用私钥
{: #reference_secret}

您可以将来自 Kubernetes 私钥的服务凭证和其他键值对作为环境变量添加到部署中。
{: shortdesc}

1. 列出集群中的可用私钥并记下私钥的 **name**。查找类型为 **Opaque** 的私钥。如果存在多个私钥，请联系集群管理员来确定正确的服务私钥。

    ```
    kubectl get secrets
    ```
    {: pre}

    输出示例：

    ```
    NAME                                    TYPE                                  DATA      AGE
    binding-<service_instance_name>         Opaque                                1         3m

    ```
    {: screen}

2. 获取私钥的详细信息，以查找可以引用为 pod 中的环境变量的潜在键值对。服务凭证存储在私钥的 `binding` 密钥中。
   ```
   kubectl get secrets binding-<service_instance_name> --namespace=<namespace> -o yaml
   ```
   {: pre}

   输出示例：
   ```
   apiVersion: v1
   data:
     binding: <binding>
   kind: Secret
   metadata:
     annotations:
       service-instance-id: 7123acde-c3ef-4ba2-8c52-439ac007fa70
       service-key-id: 9h30dh8a-023f-4cf4-9d96-d12345ec7890
     creationTimestamp: 2018-08-07T20:47:14Z
     name: binding-<service_instance_name>
     namespace: <namespace>
     resourceVersion: "6145900"
     selfLink: /api/v1/namespaces/default/secrets/binding-mycloudant
     uid: 12345a31-9a83-11e8-ba83-cd49014748f
   type: Opaque
   ```
   {: screen}

3. 为 Kubernetes 部署创建 YAML 文件，并指定引用 `binding` 密钥的环境变量。
   ```
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     labels:
       app: secret-test
     name: secret-test
     namespace: <my_namespace>
   spec:
     selector:
       matchLabels:
         app: secret-test
     template:
       metadata:
         labels:
           app: secret-test
       spec:
         containers:
         - image: registry.bluemix.net/ibmliberty:latest
           name: secret-test
           env:
           - name: BINDING
             valueFrom:
               secretKeyRef:
                 name: binding-<service_instance_name>
                 key: binding
     ```
     {: codeblock}

     <table>
     <caption>了解 YAML 文件的组成部分</caption>
     <thead>
     <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解 YAML 文件的组成部分</th>
     </thead>
     <tbody>
     <tr>
     <td><code>containers.env.name</code></td>
     <td>环境变量的名称。</td>
     </tr>
     <tr>
     <td><code>env.valueFrom.secretKeyRef.name</code></td>
     <td>先前步骤中记录的私钥的名称。</td>
     </tr>
     <tr>
     <td><code>env.valueFrom.secretKeyRef.key</code></td>
     <td>属于私钥的一部分并且您想要在环境变量中引用的密钥。要引用服务凭证，必须使用 <strong>binding</strong> 密钥。</td>
     </tr>
     </tbody></table>

4. 创建引用私钥的 `binding` 密钥作为环境变量的 pod。
   ```
    kubectl apply -f secret-test.yaml
    ```
   {: pre}

5. 验证 pod 是否已创建。
   ```
   kubectl get pods
   ```
   {: pre}

   示例 CLI 输出：
   ```
    NAME                           READY     STATUS    RESTARTS   AGE
    secret-test-1111454598-gfx32   1/1       Running   0          1m
    ```
   {: screen}

6. 验证是否已正确设置环境变量。
   1. 登录到 pod。
      ```
      kubectl exec <pod_name> -it bash
      ```
      {: pre}

   2. 列出 pod 中的所有环境变量。
      ```
      env
      ```
      {: pre}

      输出示例：
       ```
      BINDING={"apikey":"KL34Ys893284NGJEPFjgrioJ12NElpow","host":"98765aab-9ce1-7tr3-ba87-bfbab6e6d9d6-bluemix.cloudant.com","iam_apikey_description":"Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:cloudantnosqldb:us-south:a/1234g56789cfe8e6388dd2ec098:98746cw-43d7-49ce-947a-d8fe3eebb381::","iam_apikey_name":"auto-generated-apikey-1234abcde-987f-3t64-9d96-d13775ec5663","iam_role_crn":"crn:v1:bluemix:public:iam::::serviceRole:Writer","iam_serviceid_crn":"crn:v1:bluemix:public:iam-identity::a/1234567890brasge5htn2ec098::serviceid:ServiceId-12345vgh-6c4c-ytr12-af6b-467d30d6ef44","password":"jfiavhui12484fnivhuo472nvei23913c3ff","port":443,"url":"https://25c73aac-9ce1-4c24-ba98-bfbab6e6d9d6-bluemix:ugvioev823inreuiegn43donvri29989wiu9t22@25c73aac-9ce1-4c24-ba98-abdrjio123562lnsb-bluemix.cloudant.com","username":"123b45da-9ce1-4c24-ab12-rinwnwub1294-bluemix"}
      ```
      {: screen}

7. 配置应用程序以读取环境变量，并解析 JSON 内容以检索访问服务所需的信息。

   Python 中的示例代码：
   ```
   if os.environ.get('BINDING'):
        credentials = json.loads(os.environ.get('BINDING'))
   ```
   {: codeblock}

## 在 {{site.data.keyword.containerlong_notm}} 中设置 Helm
{: #helm}

[Helm ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://helm.sh) 是 Kubernetes 软件包管理器。可以创建 Helm 图表或使用预先存在的 Helm 图表来定义、安装和升级在 {{site.data.keyword.containerlong_notm}} 集群中运行的复杂 Kubernetes 应用程序。
{:shortdesc}

要部署 Helm chart，必须在本地计算机上安装 Helm CLI，并在集群中安装 Helm 服务器 Tiller。Tiller 的映像存储在公共 Google Container Registry 中。要在 Tiller 安装期间访问该映像，集群必须允许与公共 Google Container Registry 的公用网络连接。启用了公共服务端点的集群可以自动访问该映像。使用定制防火墙进行保护的专用集群或仅启用了专用服务端点的集群不允许访问 Tiller 映像。您可以改为[将映像拉取到本地计算机，并将映像推送到 {{site.data.keyword.registryshort_notm}} 中的名称空间](#private_local_tiller)，或者[安装 Helm chart（不使用 Tiller）](#private_install_without_tiller)。
{: note}

### 在具有公共访问权的集群中设置 Helm
{: #public_helm_install}

如果集群已启用公共服务端点，那么可以使用 Google Container Registry 中的公共映像来安装 Tiller。
{: shortdesc}

开始之前：[登录到您的帐户。将相应的区域和（如果适用）资源组设定为目标。设置集群的上下文](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)。

1. 安装 <a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">Helm CLI <img src="../icons/launch-glyph.svg" alt="外部链接图标"></a>。

2. **重要信息**：要维护集群安全性，请通过应用 [{{site.data.keyword.Bluemix_notm}} `kube-samples` 存储库](https://github.com/IBM-Cloud/kube-samples/blob/master/rbac/serviceaccount-tiller.yaml)中的以下 `.yaml` 文件，在 `kube-system` 名称空间中为 Tiller 创建服务帐户，为 `tiller-deploy` pod 创建 Kubernetes RBAC 集群角色绑定。**注**：要在 `kube-system` 名称空间中安装具有服务帐户和集群角色绑定的 Tiller，您必须具有 [`cluster-admin` 角色](/docs/containers?topic=containers-users#access_policies)。
    ```
    kubectl apply -f https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/rbac/serviceaccount-tiller.yaml
    ```
    {: pre}

3. 使用创建的服务帐户来初始化 Helm 并安装 Tiller。

    ```
    helm init --service-account tiller
    ```
    {: pre}

4.  验证安装是否成功。
    1.  验证 Tiller 服务帐户是否已创建。
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

    2.  验证 `tiller-deploy` pod 在集群中的 **Status** 是否为 `Running`。
        ```
        kubectl get pods -n kube-system -l app=helm
        ```
        {: pre}

        输出示例：

        ```
    NAME                            READY     STATUS    RESTARTS   AGE
    tiller-deploy-352283156-nzbcm   1/1       Running   0          2m
    ```
        {: screen}

5. 向 Helm 实例添加 {{site.data.keyword.Bluemix_notm}} Helm 存储库。
   ```
   helm repo add ibm https://registry.bluemix.net/helm/ibm
   ```
   {: pre}

   ```
    helm repo add ibm-charts https://registry.bluemix.net/helm/ibm-charts
    ```
   {: pre}

6. 更新存储库以检索所有 Helm chart 的最新版本。
   ```
        helm repo update
        ```
   {: pre}

7. 列出 {{site.data.keyword.Bluemix_notm}} 存储库中当前可用的 Helm 图表。
   ```
    helm search ibm
    ```
   {: pre}

   ```
    helm search ibm-charts
    ```
   {: pre}

8. 确定要安装的 Helm chart，并遵循该 Helm chart `README` 中的指示信息在集群中安装 Helm chart。


### 专用集群：将 Tiller 映像推送到 {{site.data.keyword.registryshort_notm}} 中的专用注册表
{: #private_local_tiller}

您可以将 Tiller 映像拉取到本地计算机，将其推送到 {{site.data.keyword.registryshort_notm}} 中的名称空间，然后让 Helm 使用 {{site.data.keyword.registryshort_notm}} 中的映像来安装 Tiller。
{: shortdesc}

开始之前：
- 在本地计算机上安装 Docker。如果已安装 [{{site.data.keyword.Bluemix_notm}} CLI](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli)，那么 Docker 已经安装。
- [安装 {{site.data.keyword.registryshort_notm}} CLI 插件并设置名称空间](/docs/services/Registry?topic=registry-getting-started#gs_registry_cli_install)。

要使用 {{site.data.keyword.registryshort_notm}} 安装 Tiller，请执行以下操作：

1. 在本地计算机上安装 <a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">Helm CLI <img src="../icons/launch-glyph.svg" alt="外部链接图标"></a>。
2. 使用设置的 {{site.data.keyword.Bluemix_notm}} 基础架构 VPN 隧道连接到专用集群。
3. **重要信息**：要维护集群安全性，请通过应用 [{{site.data.keyword.Bluemix_notm}} `kube-samples` 存储库](https://github.com/IBM-Cloud/kube-samples/blob/master/rbac/serviceaccount-tiller.yaml)中的以下 `.yaml` 文件，在 `kube-system` 名称空间中为 Tiller 创建服务帐户，为 `tiller-deploy` pod 创建 Kubernetes RBAC 集群角色绑定。**注**：要在 `kube-system` 名称空间中安装具有服务帐户和集群角色绑定的 Tiller，您必须具有 [`cluster-admin` 角色](/docs/containers?topic=containers-users#access_policies)。
    
    1. [获取 Kubernetes 服务帐户和集群角色绑定 YAML 文件 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/rbac/serviceaccount-tiller.yaml)。

    2. 在集群中创建 Kubernetes 资源。
       ```
       kubectl apply -f service-account.yaml
       ```
       {: pre}

       ```
       kubectl apply -f cluster-role-binding.yaml
       ```
       {: pre}

4. [查找 Tiller 版本 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://console.cloud.google.com/gcr/images/kubernetes-helm/GLOBAL/tiller?gcrImageListsize=30)]，即要在集群中安装的 Tiller 版本。如果不需要特定版本，请使用最新版本。

5. 将 Tiller 映像拉取到本地计算机。
   ```
   docker pull gcr.io/kubernetes-helm/tiller:v2.11.0
   ```
   {: pre}

   输出示例：
   ```
   docker pull gcr.io/kubernetes-helm/tiller:v2.13.0
   v2.13.0: Pulling from kubernetes-helm/tiller
   48ecbb6b270e: Pull complete
   d3fa0712c71b: Pull complete
   bf13a43b92e9: Pull complete
   b3f98be98675: Pull complete
   Digest: sha256:c4bf03bb67b3ae07e38e834f29dc7fd43f472f67cad3c078279ff1bbbb463aa6
   Status: Downloaded newer image for gcr.io/kubernetes-helm/tiller:v2.13.0
   ```
   {: screen}

6. [将 Tiller 映像推送到 {{site.data.keyword.registryshort_notm}} 中的名称空间](/docs/services/Registry?topic=registry-getting-started#gs_registry_images_pushing)。

7. [将用于访问 {{site.data.keyword.registryshort_notm}} 的映像拉取私钥从缺省名称空间复制到 `kube-system` 名称空间](/docs/containers?topic=containers-images#copy_imagePullSecret)。

8. 使用存储在 {{site.data.keyword.registryshort_notm}} 的名称空间中的映像，在专用集群中安装 Tiller。
   ```
   helm init --tiller-image <region>.icr.io/<mynamespace>/<myimage>:<tag> --service-account tiller
   ```
   {: pre}

9. 向 Helm 实例添加 {{site.data.keyword.Bluemix_notm}} Helm 存储库。
   ```
   helm repo add ibm https://registry.bluemix.net/helm/ibm
   ```
   {: pre}

   ```
    helm repo add ibm-charts https://registry.bluemix.net/helm/ibm-charts
    ```
   {: pre}

10. 更新存储库以检索所有 Helm chart 的最新版本。
    ```
        helm repo update
        ```
    {: pre}

11. 列出 {{site.data.keyword.Bluemix_notm}} 存储库中当前可用的 Helm 图表。
    ```
    helm search ibm
    ```
    {: pre}

    ```
    helm search ibm-charts
    ```
    {: pre}

12. 确定要安装的 Helm chart，并遵循该 Helm chart `README` 中的指示信息在集群中安装 Helm chart。

### 专用集群：安装 Helm chart（不使用 Tiller）
{: #private_install_without_tiller}

如果您不想在专用集群中安装 Tiller，那么可以使用 `kubectl` 命令手动创建 Helm chart YAML 文件并应用这些文件。
{: shortdesc}

此示例中的步骤显示了如何在专用集群中安装 {{site.data.keyword.Bluemix_notm}} Helm chart 存储库中的 Helm chart。如果要安装的 Helm chart 未存储在其中一个 {{site.data.keyword.Bluemix_notm}} Helm chart 存储库中，那么必须遵循本主题中的指示信息为 Helm chart 创建 YAML 文件。此外，必须从公共容器注册表下载 Helm chart 映像，将其推送到 {{site.data.keyword.registryshort_notm}} 中的名称空间，然后更新 `values.yaml` 文件才能在 {{site.data.keyword.registryshort_notm}} 中使用该映像。
{: note}

1. 在本地计算机上安装 <a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">Helm CLI <img src="../icons/launch-glyph.svg" alt="外部链接图标"></a>。
2. 使用设置的 {{site.data.keyword.Bluemix_notm}} 基础架构 VPN 隧道连接到专用集群。
3. 向 Helm 实例添加 {{site.data.keyword.Bluemix_notm}} Helm 存储库。
   ```
   helm repo add ibm https://registry.bluemix.net/helm/ibm
   ```
   {: pre}

   ```
    helm repo add ibm-charts https://registry.bluemix.net/helm/ibm-charts
    ```
   {: pre}

4. 更新存储库以检索所有 Helm chart 的最新版本。
   ```
        helm repo update
        ```
   {: pre}

5. 列出 {{site.data.keyword.Bluemix_notm}} 存储库中当前可用的 Helm 图表。
   ```
    helm search ibm
    ```
   {: pre}

   ```
    helm search ibm-charts
    ```
   {: pre}

6. 确定要安装的 Helm chart，将 Helm chart 下载到本地计算机，然后解压缩 Helm chart 的文件。以下示例显示了如何下载集群自动缩放器 V1.0.3 的 Helm chart，并将文件解压缩到 `cluster-autoscaler` 目录中。
   ```
   helm fetch ibm/ibm-iks-cluster-autoscaler --untar --untardir ./cluster-autoscaler --version 1.0.3
   ```
   {: pre}

7. 导航至解压缩 Helm chart 文件的目录。
   ```
   cd cluster-autoscaler
   ```
   {: pre}

8. 使用 Helm chart 中的文件为生成的 YAML 文件创建 `output` 目录。
   ```
   mkdir output
   ```
   {: pre}

9. 打开 `values.yaml` 文件，并根据 Helm chart 安装指示信息的要求进行任何更改。
   ```
   nano ibm-iks-cluster-autoscaler/values.yaml
   ```
   {: pre}

10. 使用本地 Helm 安装为 Helm chart 创建所有 Kubernetes YAML 文件。这些 YAML 文件会存储在先前创建的 `output` 目录中。
    ```
    helm template --values ./ibm-iks-cluster-autoscaler/values.yaml --output-dir ./output ./ibm-iks-cluster-autoscaler
    ```
    {: pre}

    输出示例：
    ```
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-configmap.yaml
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-service-account-roles.yaml
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-service.yaml
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-deployment.yaml
    ```
    {: screen}

11. 将所有 YAML 文件部署到专用集群。
    ```
    kubectl apply --recursive --filename ./output
    ```
   {: pre}

12. 可选：从 `output` 目录中除去所有 YAML 文件。
    ```
    kubectl delete --recursive --filename ./output
    ```
    {: pre}


### 相关 Helm 链接
{: #helm_links}

* 要使用 strongSwan Helm chart，请参阅[使用 strongSwan IPSec VPN 服务 Helm chart 设置 VPN 连接](/docs/containers?topic=containers-vpn#vpn-setup)。
* 在控制台中查看可在 [Helm chart 目录 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/kubernetes/solutions/helm-charts) 中与 {{site.data.keyword.Bluemix_notm}} 配合使用的可用 Helm chart。
* 在 <a href="https://docs.helm.sh/helm/" target="_blank">Helm 文档 <img src="../icons/launch-glyph.svg" alt="外部链接图标"></a> 中了解有关用于设置和管理 Helm 图表的 Helm 命令的更多信息。
* 了解有关如何[利用 Kubernetes Helm 图表提高部署速度 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://developer.ibm.com/recipes/tutorials/increase-deployment-velocity-with-kubernetes-helm-charts/) 的更多信息。

## 可视化 Kubernetes 集群资源
{: #weavescope}

Weave Scope 提供了 Kubernetes 集群内资源（包括服务、pod、容器等等）的可视图。此外，Weave Scope 还提供了 CPU 和内存的交互式度量值，以及用于跟踪和执行到容器中的多种工具。
{:shortdesc}

开始之前：

-   切勿在公用因特网上公开您的集群信息。完成以下步骤以安全地部署 Weave Scope 并在本地从 Web 浏览器对其进行访问。
-   如果还没有标准集群，请[创建标准集群](/docs/containers?topic=containers-clusters#clusters_ui)。Weave Scope 可以是 CPU 密集型，尤其是应用程序。请对更大型的标准集群（而不是免费集群）运行 Weave Scope。
-  确保您对所有名称空间具有[**管理者** {{site.data.keyword.Bluemix_notm}} IAM 服务角色](/docs/containers?topic=containers-users#platform)。
-   [登录到您的帐户。将相应的区域和（如果适用）资源组设定为目标。设置集群的上下文](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)。


要将 Weave Scope 用于集群，请执行以下操作：
2.  在集群中部署其中一个提供的 RBAC 许可权配置文件。

    启用读/写许可权：

    ```
    kubectl apply -f "https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/weave-scope/weave-scope-rbac.yaml"
    ```
    {: pre}

    启用只读许可权：

    ```
    kubectl apply --namespace weave -f "https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/weave-scope/weave-scope-rbac-readonly.yaml"
    ```
    {: pre}

    输出：

    ```
    clusterrole "weave-scope-mgr" created
    clusterrolebinding "weave-scope-mgr-role-binding" created
    ```
    {: screen}

3.  部署 Weave Scope 服务，此服务专供集群 IP 地址访问。

    <pre class="pre">
    <code>kubectl apply --namespace weave -f "https://cloud.weave.works/k8s/scope.yaml?k8s-version=$(kubectl version | base64 | tr -d '&bsol;n')"</code>
    </pre>

    输出：

    ```
    serviceaccount "weave-scope" created
    deployment "weave-scope-app" created
    service "weave-scope-app" created
    daemonset "weave-scope-agent" created
    ```
    {: screen}

4.  运行端口转发命令以在计算机上打开该服务。下次访问 Weave Scope 时，可以运行以下端口转发命令，而不用再次完成先前的配置步骤。

    ```
    kubectl port-forward -n weave "$(kubectl get -n weave pod --selector=weave-scope-component=app -o jsonpath='{.items..metadata.name}')" 4040
    ```
    {: pre}

    输出：

    ```
Forwarding from 127.0.0.1:4040 -> 4040
    Forwarding from [::1]: :1]:4040 -> 4040
    Handling connection for 4040
    ```
    {: screen}

5.  打开浏览器并转至 `http://localhost:4040`。未部署缺省组件时，将看到下图。可以选择查看集群中 Kubernetes 资源的拓扑图或表。

     <img src="images/weave_scope.png" alt="Weave Scope 中的示例拓扑" style="width:357px;" />


[了解有关 Weave Scope 功能的更多信息 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.weave.works/docs/scope/latest/features/)。

<br />

