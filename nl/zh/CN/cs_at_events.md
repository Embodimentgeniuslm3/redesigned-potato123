---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-31"

keywords: kubernetes, iks, audit

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



# {{site.data.keyword.cloudaccesstrailshort}} 事件
{: #at_events}

您可以使用 {{site.data.keyword.cloudaccesstrailshort}} 服务来查看、管理和审计 {{site.data.keyword.containerlong_notm}} 集群中用户启动的活动。
{: shortdesc}

{{site.data.keyword.containershort_notm}} 会生成两种类型的 {{site.data.keyword.cloudaccesstrailshort}} 事件：

* **集群管理事件**：
    * 这些事件会自动生成并转发到 {{site.data.keyword.cloudaccesstrailshort}}。
    * 可以通过 {{site.data.keyword.cloudaccesstrailshort}} **帐户域**来查看这些事件。

* **Kubernetes API 服务器审计事件**：
    * 这些事件会自动生成，您需要对集群进行配置，才能将这些事件转发到 {{site.data.keyword.cloudaccesstrailshort}} 服务。
    * 可以配置集群以将事件发送到 {{site.data.keyword.cloudaccesstrailshort}} **帐户域**或**空间域**。有关更多信息，请参阅[发送审计日志](/docs/containers?topic=containers-health#api_forward)。

有关服务如何运作的更多信息，请参阅 [{{site.data.keyword.cloudaccesstrailshort}} 文档](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started)。有关跟踪的 Kubernetes 操作的更多信息，请参阅 [Kubernetes 文档 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/home/)。

目前，{{site.data.keyword.containerlong_notm}} 未配置为使用 {{site.data.keyword.at_full}}。要管理集群管理事件和 Kubernetes API 审计日志，请继续将 {{site.data.keyword.cloudaccesstrailfull_notm}} 与 Log Analysis 配合使用。
{: note}

## 查找事件的信息
{: #kube-find}

您可以通过在 Kibana 仪表板中查看日志来监视集群中的活动。
{: shortdesc}

要监视管理活动，请执行以下操作：

1. 登录到 {{site.data.keyword.Bluemix_notm}} 帐户。
2. 在目录中，在您的 {{site.data.keyword.containerlong_notm}} 实例所在的帐户下供应 {{site.data.keyword.cloudaccesstrailshort}} 服务实例。
3. 在 {{site.data.keyword.cloudaccesstrailshort}} 仪表板的**管理**选项卡上，选择帐户或空间域。
  * **帐户日志**：集群管理事件和 Kubernetes API 服务器审计事件可在生成这些事件所在 {{site.data.keyword.Bluemix_notm}} 区域的**帐户域**中找到。
  * **空间日志**：如果在配置集群如何转发 Kubernetes API 服务器审计事件时指定了空间，那么这些事件可在与 {{site.data.keyword.cloudaccesstrailshort}} 服务供应所在 Cloud Foundry 空间相关联的**空间域**中找到。
4. 单击**在 Kibana 中查看**。
5. 设置要查看日志的时间范围。缺省值为 24 小时。
6. 要缩小搜索范围，可以单击 `ActivityTracker_Account_Search_in_24h` 的编辑图标，然后在**可用字段**列中添加字段。

要允许其他用户查看帐户和空间事件，请参阅[授予查看帐户事件的许可权](/docs/services/cloud-activity-tracker/how-to?topic=cloud-activity-tracker-grant_permissions#grant_permissions)。
{: tip}

## 跟踪集群管理事件
{: #cluster-events}

要了解发送到 {{site.data.keyword.cloudaccesstrailshort}} 的集群管理事件，请查看以下列表。
{: shortdesc}

<table>
<tr>
<th>操作</th>
<th>描述</th></tr><tr>
<td><code>containers-kubernetes.account-credentials.set</code></td>
<td>在区域中为资源组设置了基础架构凭证。</td></tr><tr>
<td><code>containers-kubernetes.account-credentials.unset</code></td>
<td>在区域中为资源组取消设置了基础架构凭证。</td></tr><tr>
<td><code>containers-kubernetes.alb.create</code></td>
<td>创建了 Ingress ALB。</td></tr><tr>
<td><code>containers-kubernetes.alb.delete</code></td>
<td>删除了 Ingress ALB。</td></tr><tr>
<td><code>containers-kubernetes.alb.get</code></td>
<td>查看了 Ingress ALB 信息。</td></tr><tr>
<td><code>containers-kubernetes.apikey.reset</code></td>
<td>为区域和资源组重置了 API 密钥。</td></tr><tr>
<td><code>containers-kubernetes.cluster.create</code></td>
<td>创建了集群。</td></tr><tr>
<td><code>containers-kubernetes.cluster.delete</code></td>
<td>删除了集群。</td></tr><tr>
<td><code>containers-kubernetes.cluster-feature.enable</code></td>
<td>在集群上启用了一项功能，例如裸机工作程序节点的可信计算。</td></tr><tr>
<td><code>containers-kubernetes.cluster.get</code></td>
<td>查看了集群信息。</td></tr><tr>
<td><code>containers-kubernetes.logging-config.create</code></td>
<td>创建了日志转发配置。</td></tr><tr>
<td><code>containers-kubernetes.logging-config.delete</code></td>
<td>删除了日志转发配置。</td></tr><tr>
<td><code>containers-kubernetes.logging-config.get</code></td>
<td>查看了日志转发配置的信息。</td></tr><tr>
<td><code>containers-kubernetes.logging-config.update</code></td>
<td>更新了日志转发配置。</td></tr><tr>
<td><code>containers-kubernetes.logging-config.refresh</code></td>
<td>刷新了日志转发配置。</td></tr><tr>
<td><code>containers-kubernetes.logging-filter.create</code></td>
<td>创建了日志记录过滤器。</td></tr><tr>
<td><code>containers-kubernetes.logging-filter.delete</code></td>
<td>删除了日志记录过滤器。</td></tr><tr>
<td><code>containers-kubernetes.logging-filter.get</code></td>
<td>查看了日志记录过滤器的信息。</td></tr><tr>
<td><code>containers-kubernetes.logging-filter.update</code></td>
<td>更新了日志记录过滤器。</td></tr><tr>
<td><code>containers-kubernetes.logging-autoupdate.changed</code></td>
<td>启用或禁用了日志记录附加组件自动更新程序。</td></tr><tr>
<td><code>containers-kubernetes.mzlb.create</code></td>
<td>创建了多专区负载均衡器。</td></tr><tr>
<td><code>containers-kubernetes.mzlb.delete</code></td>
<td>删除了多专区负载均衡器。</td></tr><tr>
<td><code>containers-kubernetes.service.bind</code></td>
<td>将服务绑定到了集群。</td></tr><tr>
<td><code>containers-kubernetes.service.unbind</code></td>
<td>取消了服务与集群的绑定。</td></tr><tr>
<td><code>containers-kubernetes.subnet.add</code></td>
<td>将现有 IBM Cloud Infrastructure (SoftLayer) 子网添加到了集群。</td></tr><tr>
<td><code>containers-kubernetes.subnet.create</code></td>
<td>创建了子网。</td></tr><tr>
<td><code>containers-kubernetes.usersubnet.add</code></td>
<td>将用户管理的子网添加到了集群。</td></tr><tr>
<td><code>containers-kubernetes.usersubnet.delete</code></td>
<td>从集群中除去了用户管理的子网。</td></tr><tr>
<td><code>containers-kubernetes.version.update</code></td>
<td>更新了集群主节点的 Kubernetes 版本。</td></tr><tr>
<td><code>containers-kubernetes.worker.create</code></td>
<td>创建了工作程序节点。</td></tr><tr>
<td><code>containers-kubernetes.worker.delete</code></td>
<td>删除了工作程序节点。</td></tr><tr>
<td><code>containers-kubernetes.worker.get</code></td>
<td>查看了工作程序节点的信息。</td></tr><tr>
<td><code>containers-kubernetes.worker.reboot</code></td>
<td>重新引导了工作程序节点。</td></tr><tr>
<td><code>containers-kubernetes.worker.reload</code></td>
<td>重新装入了工作程序节点。</td></tr><tr>
<td><code>containers-kubernetes.worker.update</code></td>
<td>更新了工作程序节点。</td></tr>
</table>

## 跟踪 Kubernetes 审计事件
{: #kube-events}

要了解发送到 {{site.data.keyword.cloudaccesstrailshort}} 的 Kubernetes API 服务器审计事件，请查看以下列表。
{: shortdesc}

开始之前：请确保已配置集群如何转发 [Kubernetes API 审计事件](/docs/containers?topic=containers-health#api_forward)。

<table>
    <th>操作</th>
    <th>描述</th><tr>
    <td><code>bindings.create</code></td>
    <td>创建了绑定。</td></tr><tr>
    <td><code>certificatesigningrequests.create</code></td>
    <td>创建了证书签名请求。</td></tr><tr>
    <td><code>certificatesigningrequests.delete</code></td>
    <td>删除了证书签名请求。</td></tr><tr>
    <td><code>certificatesigningrequests.patch</code></td>
    <td>修补了证书签名请求。</td></tr><tr>
    <td><code>certificatesigningrequests.update</code></td>
    <td>更新了证书签名请求。</td></tr><tr>
    <td><code>clusterbindings.create</code></td>
    <td>创建了集群角色绑定。</td></tr><tr>
    <td><code>clusterbindings.deleted</code></td>
    <td>删除了集群角色绑定。</td></tr><tr>
    <td><code>clusterbindings.patched</code></td>
    <td>修补了集群角色绑定。</td></tr><tr>
    <td><code>clusterbindings.updated</code></td>
    <td>更新了集群角色绑定。</td></tr><tr>
    <td><code>clusterroles.create</code></td>
    <td>创建了集群角色。</td></tr><tr>
    <td><code>clusterroles.deleted</code></td>
    <td>删除了集群角色。</td></tr><tr>
    <td><code>clusterroles.patched</code></td>
    <td>修补了集群角色。</td></tr><tr>
    <td><code>clusterroles.updated</code></td>
    <td>更新了集群角色。</td></tr><tr>
    <td><code>configmaps.create</code></td>
    <td>创建了配置映射。</td></tr><tr>
    <td><code>configmaps.delete</code></td>
    <td>删除了配置映射。</td></tr><tr>
    <td><code>configmaps.patch</code></td>
    <td>修补了配置映射。</td></tr><tr>
    <td><code>configmaps.update</code></td>
    <td>更新了配置映射。</td></tr><tr>
    <td><code>controllerrevisions.create</code></td>
    <td>创建了控制器修订版。</td></tr><tr>
    <td><code>controllerrevisions.delete</code></td>
    <td>删除了控制器修订版。</td></tr><tr>
    <td><code>controllerrevisions.patch</code></td>
    <td>修补了控制器修订版。</td></tr><tr>
    <td><code>controllerrevisions.update</code></td>
    <td>更新了控制器修订版。</td></tr><tr>
    <td><code>daemonsets.create</code></td>
    <td>创建了守护程序集。</td></tr><tr>
    <td><code>daemonsets.delete</code></td>
    <td>删除了守护程序集。</td></tr><tr>
    <td><code>daemonsets.patch</code></td>
    <td>修补了守护程序集。</td></tr><tr>
    <td><code>daemonsets.update</code></td>
    <td>更新了守护程序集。</td></tr><tr>
    <td><code>deployments.create</code></td>
    <td>创建了部署。</td></tr><tr>
    <td><code>deployments.delete</code></td>
    <td>删除了部署。</td></tr><tr>
    <td><code>deployments.patch</code></td>
    <td>修补了部署。</td></tr><tr>
    <td><code>deployments.update</code></td>
    <td>更新了部署。</td></tr><tr>
    <td><code>events.create</code></td>
    <td>创建了事件。</td></tr><tr>
    <td><code>events.delete</code></td>
    <td>删除了事件。</td></tr><tr>
    <td><code>events.patch</code></td>
    <td>修补了事件。</td></tr><tr>
    <td><code>events.update</code></td>
    <td>更新了事件。</td></tr><tr>
    <td><code>externaladmissionhookconfigurations.create</code></td>
    <td>在 Kubernetes V1.8 中，创建了外部许可 Hook 配置。</td></tr><tr>
    <td><code>externaladmissionhookconfigurations.delete</code></td>
    <td>在 Kubernetes V1.8 中，删除了外部许可 Hook 配置。</td></tr><tr>
    <td><code>externaladmissionhookconfigurations.patch</code></td>
    <td>在 Kubernetes V1.8 中，修补了外部许可 Hook 配置。</td></tr><tr>
    <td><code>externaladmissionhookconfigurations.update</code></td>
    <td>在 Kubernetes V1.8 中，更新了外部许可 Hook 配置。</td></tr><tr>
    <td><code>horizontalpodautoscalers.create</code></td>
    <td>创建了水平 pod 自动缩放策略。</td></tr><tr>
    <td><code>horizontalpodautoscalers.delete</code></td>
    <td>删除了水平 pod 自动缩放策略。</td></tr><tr>
    <td><code>horizontalpodautoscalers.patch</code></td>
    <td>修补了水平 pod 自动缩放策略。</td></tr><tr>
    <td><code>horizontalpodautoscalers.update</code></td>
    <td>更新了水平 pod 自动缩放策略。</td></tr><tr>
    <td><code>ingresses.create</code></td>
    <td>创建了 Ingress ALB。</td></tr><tr>
    <td><code>ingresses.delete</code></td>
    <td>删除了 Ingress ALB。</td></tr><tr>
    <td><code>ingresses.patch</code></td>
    <td>修补了 Ingress ALB。</td></tr><tr>
    <td><code>ingresses.update</code></td>
    <td>更新了 Ingress ALB。</td></tr><tr>
    <td><code>jobs.create</code></td>
    <td>创建了作业。</td></tr><tr>
    <td><code>jobs.delete</code></td>
    <td>删除了作业。</td></tr><tr>
    <td><code>jobs.patch</code></td>
    <td>修补了作业。</td></tr><tr>
    <td><code>jobs.update</code></td>
    <td>更新了作业。</td></tr><tr>
    <td><code>localsubjectaccessreviews.create</code></td>
    <td>创建了本地主题访问权复查。</td></tr><tr>
    <td><code>limitranges.create</code></td>
    <td>创建了范围限制。</td></tr><tr>
    <td><code>limitranges.delete</code></td>
    <td>删除了范围限制。</td></tr><tr>
    <td><code>limitranges.patch</code></td>
    <td>修补了范围限制。</td></tr><tr>
    <td><code>limitranges.update</code></td>
    <td>更新了范围限制。</td></tr><tr>
    <td><code>mutatingwebhookconfigurations.create</code></td>
    <td>创建了更改 Webhook 配置。</td></tr><tr>
    <td><code>mutatingwebhookconfigurations.delete</code></td>
    <td>删除了更改 Webhook 配置。</td></tr><tr>
    <td><code>mutatingwebhookconfigurations.patch</code></td>
    <td>修补了更改 Webhook 配置。</td></tr><tr>
    <td><code>mutatingwebhookconfigurations.update</code></td>
    <td>更新了更改 Webhook 配置。</td></tr><tr>
    <td><code>namespaces.create</code></td>
    <td>创建了名称空间。</td></tr><tr>
    <td><code>namespaces.delete</code></td>
    <td>删除了名称空间。</td></tr><tr>
    <td><code>namespaces.patch</code></td>
    <td>修补了名称空间。</td></tr><tr>
    <td><code>namespaces.update</code></td>
    <td>更新了名称空间。</td></tr><tr>
    <td><code>networkpolicies.create</code></td>
    <td>创建了网络策略。</td></tr><tr><tr>
    <td><code>networkpolicies.delete</code></td>
    <td>删除了网络策略。</td></tr><tr>
    <td><code>networkpolicies.patch</code></td>
    <td>修补了网络策略。</td></tr><tr>
    <td><code>networkpolicies.update</code></td>
    <td>更新了网络策略。</td></tr><tr>
    <td><code>nodes.create</code></td>
    <td>创建了节点。</td></tr><tr>
    <td><code>nodes.delete</code></td>
    <td>删除了节点。</td></tr><tr>
    <td><code>nodes.patch</code></td>
    <td>修补了节点。</td></tr><tr>
    <td><code>nodes.update</code></td>
    <td>更新了节点。</td></tr><tr>
    <td><code>persistentvolumeclaims.create</code></td>
    <td>创建了持久卷声明。</td></tr><tr>
    <td><code>persistentvolumeclaims.delete</code></td>
    <td>删除了持久卷声明。</td></tr><tr>
    <td><code>persistentvolumeclaims.patch</code></td>
    <td>修补了持久卷声明。</td></tr><tr>
    <td><code>persistentvolumeclaims.update</code></td>
    <td>更新了持久卷声明。</td></tr><tr>
    <td><code>persistentvolumes.create</code></td>
    <td>创建了持久卷。</td></tr><tr>
    <td><code>persistentvolumes.delete</code></td>
    <td>删除了持久卷。</td></tr><tr>
    <td><code>persistentvolumes.patch</code></td>
    <td>修补了持久卷。</td></tr><tr>
    <td><code>persistentvolumes.update</code></td>
    <td>更新了持久卷。</td></tr><tr>
    <td><code>poddisruptionbudgets.create</code></td>
    <td>创建了 pod 中断预算。</td></tr><tr>
    <td><code>poddisruptionbudgets.delete</code></td>
    <td>删除了 pod 中断预算。</td></tr><tr>
    <td><code>poddisruptionbudgets.patch</code></td>
    <td>修补了 pod 中断预算。</td></tr><tr>
    <td><code>poddisruptionbudgets.update</code></td>
    <td>更新了 pod 中断预算。</td></tr><tr>
    <td><code>podpresets.create</code></td>
    <td>创建了 pod 预设。</td></tr><tr>
    <td><code>podpresets.deleted</code></td>
    <td>删除了 pod 预设。</td></tr><tr>
    <td><code>podpresets.patched</code></td>
    <td>修补了 pod 预设。</td></tr><tr>
    <td><code>podpresets.updated</code></td>
    <td>更新了 pod 预设。</td></tr><tr>
    <td><code>pods.create</code></td>
    <td>创建了 pod。</td></tr><tr>
    <td><code>pods.delete</code></td>
    <td>删除了 pod。</td></tr><tr>
    <td><code>pods.patch</code></td>
    <td>修补了 pod。</td></tr><tr>
    <td><code>pods.update</code></td>
    <td>更新了 pod。</td></tr><tr>
    <td><code>podsecuritypolicies.create</code></td>
    <td>创建了 pod 安全策略。</td></tr><tr>
    <td><code>podsecuritypolicies.delete</code></td>
    <td>删除了 pod 安全策略。</td></tr><tr>
    <td><code>podsecuritypolicies.patch</code></td>
    <td>修补了 pod 安全策略。</td></tr><tr>
    <td><code>podsecuritypolicies.update</code></td>
    <td>更新了 pod 安全策略。</td></tr><tr>
    <td><code>podtemplates.create</code></td>
    <td>创建了 pod 模板。</td></tr><tr>
    <td><code>podtemplates.delete</code></td>
    <td>删除了 pod 模板。</td></tr><tr>
    <td><code>podtemplates.patch</code></td>
    <td>修补了 pod 模板。</td></tr><tr>
    <td><code>podtemplates.update</code></td>
    <td>更新了 pod 模板。</td></tr><tr>
    <td><code>replicasets.create</code></td>
    <td>创建了副本集。</td></tr><tr>
    <td><code>replicasets.delete</code></td>
    <td>删除了副本集。</td></tr><tr>
    <td><code>replicasets.patch</code></td>
    <td>修补了副本集。</td></tr><tr>
    <td><code>replicasets.update</code></td>
    <td>更新了副本集。</td></tr><tr>
    <td><code>replicationcontrollers.create</code></td>
    <td>创建了复制控制器。</td></tr><tr>
    <td><code>replicationcontrollers.delete</code></td>
    <td>删除了复制控制器。</td></tr><tr>
    <td><code>replicationcontrollers.patch</code></td>
    <td>修补了复制控制器。</td></tr><tr>
    <td><code>replicationcontrollers.update</code></td>
    <td>更新了复制控制器。</td></tr><tr>
    <td><code>resourcequotas.create</code></td>
    <td>创建了资源配额。</td></tr><tr>
    <td><code>resourcequotas.delete</code></td>
    <td>删除了资源配额。</td></tr><tr>
    <td><code>resourcequotas.patch</code></td>
    <td>修补了资源配额。</td></tr><tr>
    <td><code>resourcequotas.update</code></td>
    <td>更新了资源配额。</td></tr><tr>
    <td><code>rolebindings.create</code></td>
    <td>创建了角色绑定。</td></tr><tr>
    <td><code>rolebindings.deleted</code></td>
    <td>删除了角色绑定。</td></tr><tr>
    <td><code>rolebindings.patched</code></td>
    <td>修补了角色绑定。</td></tr><tr>
    <td><code>rolebindings.updated</code></td>
    <td>更新了角色绑定。</td></tr><tr>
    <td><code>roles.create</code></td>
    <td>创建了角色。</td></tr><tr>
    <td><code>roles.deleted</code></td>
    <td>删除了角色。</td></tr><tr>
    <td><code>roles.patched</code></td>
    <td>修补了角色。</td></tr><tr>
    <td><code>roles.updated</code></td>
    <td>更新了角色。</td></tr><tr>
    <td><code>secrets.create</code></td>
    <td>创建了私钥。</td></tr><tr>
    <td><code>secrets.deleted</code></td>
    <td>删除了私钥。</td></tr><tr>
    <td><code>secrets.get</code></td>
    <td>查看了私钥。</td></tr><tr>
    <td><code>secrets.patch</code></td>
    <td>修补了私钥。</td></tr><tr>
    <td><code>secrets.updated</code></td>
    <td>更新了私钥。</td></tr><tr>
    <td><code>selfsubjectaccessreviews.create</code></td>
    <td>创建了自助主题访问权复查。</td></tr><tr>
    <td><code>selfsubjectrulesreviews.create</code></td>
    <td>创建了自助主题规则复查。</td></tr><tr>
    <td><code>subjectaccessreviews.create</code></td>
    <td>创建了主题访问权复查。</td></tr><tr>
    <td><code>serviceaccounts.create</code></td>
    <td>创建了服务帐户。</td></tr><tr>
    <td><code>serviceaccounts.deleted</code></td>
    <td>删除了服务帐户。</td></tr><tr>
    <td><code>serviceaccounts.patch</code></td>
    <td>修补了服务帐户。</td></tr><tr>
    <td><code>serviceaccounts.updated</code></td>
    <td>更新了服务帐户。</td></tr><tr>
    <td><code>services.create</code></td>
    <td>创建了服务。</td></tr><tr>
    <td><code>services.deleted</code></td>
    <td>删除了服务。</td></tr><tr>
    <td><code>services.patch</code></td>
    <td>修补了服务。</td></tr><tr>
    <td><code>services.updated</code></td>
    <td>更新了服务。</td></tr><tr>
    <td><code>statefulsets.create</code></td>
    <td>创建了有状态集。</td></tr><tr>
    <td><code>statefulsets.delete</code></td>
    <td>删除了有状态集。</td></tr><tr>
    <td><code>statefulsets.patch</code></td>
    <td>修补了有状态集。</td></tr><tr>
    <td><code>statefulsets.update</code></td>
    <td>更新了有状态集。</td></tr><tr>
    <td><code>tokenreviews.create</code></td>
    <td>创建了令牌复查。</td></tr><tr>
    <td><code>validatingwebhookconfigurations.create</code></td>
    <td>创建了 Webhook 配置验证。</td></tr><tr>
    <td><code>validatingwebhookconfigurations.delete</code></td>
    <td>删除了 Webhook 配置验证。</td></tr><tr>
    <td><code>validatingwebhookconfigurations.patch</code></td>
    <td>修补了 Webhook 配置验证。</td></tr><tr>
    <td><code>validatingwebhookconfigurations.update</code></td>
    <td>更新了 Webhook 配置验证。</td></tr>
</table>
