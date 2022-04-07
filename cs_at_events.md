---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-19"

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



# {{site.data.keyword.cloudaccesstrailshort}} events
{: #at_events}

You can view, manage, and audit user-initiated activities in your {{site.data.keyword.containerlong}} community Kubernetes or OpenShift cluster by using the {{site.data.keyword.cloudaccesstrailshort}} service.
{: shortdesc}

The {{site.data.keyword.containershort_notm}} generates two types of {{site.data.keyword.cloudaccesstrailshort}} events:

* **Cluster management events**:
    * These events are automatically generated and forwarded to {{site.data.keyword.cloudaccesstrailshort}}.
    * You can view these events through the {{site.data.keyword.cloudaccesstrailshort}} **account domain**.

* **Kubernetes API server audit events**:
    * These events are automatically generated, but you must configure your cluster to forward these events to the {{site.data.keyword.cloudaccesstrailshort}} service.
    * You can configure your cluster to send events to the {{site.data.keyword.cloudaccesstrailshort}} **account domain** or to a **space domain**. For more information, see [Sending audit logs](/docs/containers?topic=containers-health#api_forward).

For more information about how the service works, see the [{{site.data.keyword.cloudaccesstrailshort}} documentation](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started). For more information about the Kubernetes actions that are tracked, review the [Kubernetes documentation![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/home/).

{{site.data.keyword.containerlong_notm}} is currently not configured to use {{site.data.keyword.at_full}}. To manage cluster management events and Kubernetes API audit logs, continue to use {{site.data.keyword.cloudaccesstrailfull_notm}} with LogAnalysis.
{: note}

## Finding information for events
{: #kube-find}

You can monitor the activities in your cluster by looking at the logs in the Kibana dashboard.
{: shortdesc}

To monitor administrative activity:

1. Log in to your {{site.data.keyword.cloud_notm}} account.
2. From the catalog, provision an instance of the {{site.data.keyword.cloudaccesstrailshort}} service in the same account as your instance of {{site.data.keyword.containerlong_notm}}.
3. On the **Manage** tab of the {{site.data.keyword.cloudaccesstrailshort}} dashboard, select the account or space domain.
  * **Account logs**: Cluster management events and Kubernetes API server audit events are available in the **account domain** for the {{site.data.keyword.cloud_notm}} region where the events are generated.
  * **Space logs**: If you specified a space when you configured your cluster to forward Kubernetes API server audit events, these events are available in the **space domain** that is associated with the Cloud Foundry space where the {{site.data.keyword.cloudaccesstrailshort}} service is provisioned.
4. Click **View in Kibana**.
5. Set the time frame that you want to view logs for. The default is 24 hours.
6. To narrow your search, you can click the edit icon for `ActivityTracker_Account_Search_in_24h` and add fields in the **Available Fields** column.

To let other users view account and space events, see [Granting permissions to see account events](/docs/services/cloud-activity-tracker/how-to?topic=cloud-activity-tracker-grant_permissions#grant_permissions).
{: tip}

## Tracking cluster management events
{: #cluster-events}

Check out the following list of the cluster management events that are sent to {{site.data.keyword.cloudaccesstrailshort}}.
{: shortdesc}

<table>
<tr>
<th>Action</th>
<th>Description</th></tr><tr>
<td><code>containers-kubernetes.account-credentials.set</code></td>
<td>Infrastructure credentials in a region for a resource group are set.</td></tr><tr>
<td><code>containers-kubernetes.account-credentials.unset</code></td>
<td>Infrastructure credentials in a region for a resource group are unset.</td></tr><tr>
<td><code>containers-kubernetes.alb.create</code></td>
<td>An Ingress ALB is created.</td></tr><tr>
<td><code>containers-kubernetes.alb.delete</code></td>
<td>An Ingress ALB is deleted.</td></tr><tr>
<td><code>containers-kubernetes.alb.get</code></td>
<td>Ingress ALB information is viewed.</td></tr><tr>
<td><code>containers-kubernetes.apikey.reset</code></td>
<td>An API key is reset for a region and resource group.</td></tr><tr>
<td><code>containers-kubernetes.cluster.create</code></td>
<td>A cluster is created.</td></tr><tr>
<td><code>containers-kubernetes.cluster.delete</code></td>
<td>A cluster is deleted.</td></tr><tr>
<td><code>containers-kubernetes.cluster-feature.enable</code></td>
<td>A feature, such as the public or private service endpoint, is enabled on a cluster.</td></tr><tr>
<td><code>containers-kubernetes.cluster.get</code></td>
<td>Cluster information is viewed.</td></tr><tr>
<td><code>containers-kubernetes.logging-config.create</code></td>
<td>A log forwarding configuration is created.</td></tr><tr>
<td><code>containers-kubernetes.logging-config.delete</code></td>
<td>A log forwarding configuration is deleted.</td></tr><tr>
<td><code>containers-kubernetes.logging-config.get</code></td>
<td>Information for a log forwarding configuration is viewed.</td></tr><tr>
<td><code>containers-kubernetes.logging-config.update</code></td>
<td>A log forwarding configuration is updated.</td></tr><tr>
<td><code>containers-kubernetes.logging-config.refresh</code></td>
<td>A log forwarding configuration is refreshed.</td></tr><tr>
<td><code>containers-kubernetes.logging-filter.create</code></td>
<td>A logging filter is created.</td></tr><tr>
<td><code>containers-kubernetes.logging-filter.delete</code></td>
<td>A logging filter is deleted.</td></tr><tr>
<td><code>containers-kubernetes.logging-filter.get</code></td>
<td>Information for a logging filter is viewed.</td></tr><tr>
<td><code>containers-kubernetes.logging-filter.update</code></td>
<td>A logging filter is updated.</td></tr><tr>
<td><code>containers-kubernetes.logging-autoupdate.changed</code></td>
<td>The logging add-on auto updater is enabled or disabled.</td></tr><tr>
<td><code>containers-kubernetes.mzlb.create</code></td>
<td>A multizone load balancer is created.</td></tr><tr>
<td><code>containers-kubernetes.mzlb.delete</code></td>
<td>A multizone load balancer is deleted.</td></tr><tr>
<td><code>containers-kubernetes.service.bind</code></td>
<td>A service is bound to a cluster.</td></tr><tr>
<td><code>containers-kubernetes.service.unbind</code></td>
<td>A service is unbound from a cluster.</td></tr><tr>
<td><code>containers-kubernetes.subnet.add</code></td>
<td>An existing IBM Cloud infrastructure subnet is added to a cluster.</td></tr><tr>
<td><code>containers-kubernetes.subnet.create</code></td>
<td>A subnet is created.</td></tr><tr>
<td><code>containers-kubernetes.usersubnet.add</code></td>
<td>A user-managed subnet is added to a cluster.</td></tr><tr>
<td><code>containers-kubernetes.usersubnet.delete</code></td>
<td>A user-managed subnet is removed from a cluster.</td></tr><tr>
<td><code>containers-kubernetes.version.update</code></td>
<td>The Kubernetes version of a cluster master node is updated.</td></tr><tr>
<td><code>containers-kubernetes.worker.create</code></td>
<td>A worker node is created.</td></tr><tr>
<td><code>containers-kubernetes.worker.delete</code></td>
<td>A worker node is deleted.</td></tr><tr>
<td><code>containers-kubernetes.worker.get</code></td>
<td>Information for a worker node is viewed.</td></tr><tr>
<td><code>containers-kubernetes.worker.reboot</code></td>
<td>A worker node is rebooted.</td></tr><tr>
<td><code>containers-kubernetes.worker.reload</code></td>
<td>A worker node is reloaded.</td></tr><tr>
<td><code>containers-kubernetes.worker.update</code></td>
<td>A worker node is updated.</td></tr>
</table>

## Tracking Kubernetes audit events
{: #kube-events}

Check out the following table for a list of the Kubernetes API server audit events that are sent to {{site.data.keyword.cloudaccesstrailshort}}.
{: shortdesc}

Before you begin: Be sure that your cluster is configured to forward [Kubernetes API audit events](/docs/containers?topic=containers-health#api_forward).

<table>
    <th>Action</th>
    <th>Description</th><tr>
    <td><code>bindings.create</code></td>
    <td>A binding is created.</td></tr><tr>
    <td><code>certificatesigningrequests.create</code></td>
    <td>A request to sign a certificate is created.</td></tr><tr>
    <td><code>certificatesigningrequests.delete</code></td>
    <td>A request to sign a certificate is deleted.</td></tr><tr>
    <td><code>certificatesigningrequests.patch</code></td>
    <td>A request to sign a certificate is patched.</td></tr><tr>
    <td><code>certificatesigningrequests.update</code></td>
    <td>A request to sign a certificate is updated.</td></tr><tr>
    <td><code>clusterbindings.create</code></td>
    <td>A cluster role binding is created.</td></tr><tr>
    <td><code>clusterbindings.deleted</code></td>
    <td>A cluster role binding is deleted.</td></tr><tr>
    <td><code>clusterbindings.patched</code></td>
    <td>A cluster role binding is patched.</td></tr><tr>
    <td><code>clusterbindings.updated</code></td>
    <td>A cluster role binding is updated.</td></tr><tr>
    <td><code>clusterroles.create</code></td>
    <td>A cluster role is created.</td></tr><tr>
    <td><code>clusterroles.deleted</code></td>
    <td>A cluster role is deleted.</td></tr><tr>
    <td><code>clusterroles.patched</code></td>
    <td>A cluster role is patched.</td></tr><tr>
    <td><code>clusterroles.updated</code></td>
    <td>A cluster role is updated.</td></tr><tr>
    <td><code>configmaps.create</code></td>
    <td>A configuration map is created.</td></tr><tr>
    <td><code>configmaps.delete</code></td>
    <td>A configuration map is deleted.</td></tr><tr>
    <td><code>configmaps.patch</code></td>
    <td>A configuration map is patched.</td></tr><tr>
    <td><code>configmaps.update</code></td>
    <td>A configuration map is updated.</td></tr><tr>
    <td><code>controllerrevisions.create</code></td>
    <td>A controller revision is created.</td></tr><tr>
    <td><code>controllerrevisions.delete</code></td>
    <td>A controller revision is deleted.</td></tr><tr>
    <td><code>controllerrevisions.patch</code></td>
    <td>A controller revision is patched.</td></tr><tr>
    <td><code>controllerrevisions.update</code></td>
    <td>A controller revision is updated.</td></tr><tr>
    <td><code>daemonsets.create</code></td>
    <td>A daemon set is created.</td></tr><tr>
    <td><code>daemonsets.delete</code></td>
    <td>A daemon set is deleted.</td></tr><tr>
    <td><code>daemonsets.patch</code></td>
    <td>A daemon set is patched.</td></tr><tr>
    <td><code>daemonsets.update</code></td>
    <td>A daemon set is updated.</td></tr><tr>
    <td><code>deployments.create</code></td>
    <td>A deployment is created.</td></tr><tr>
    <td><code>deployments.delete</code></td>
    <td>A deployment is deleted.</td></tr><tr>
    <td><code>deployments.patch</code></td>
    <td>A deployment is patched.</td></tr><tr>
    <td><code>deployments.update</code></td>
    <td>A deployment is updated.</td></tr><tr>
    <td><code>events.create</code></td>
    <td>An event is created.</td></tr><tr>
    <td><code>events.delete</code></td>
    <td>An event is deleted.</td></tr><tr>
    <td><code>events.patch</code></td>
    <td>An event is patched.</td></tr><tr>
    <td><code>events.update</code></td>
    <td>An event is updated.</td></tr><tr>
    <td><code>externaladmissionhookconfigurations.create</code></td>
    <td>In Kubernetes v1.8, an external admissions hook configuration is created.</td></tr><tr>
    <td><code>externaladmissionhookconfigurations.delete</code></td>
    <td>In Kubernetes v1.8, an external admissions hook configuration is deleted.</td></tr><tr>
    <td><code>externaladmissionhookconfigurations.patch</code></td>
    <td>In Kubernetes v1.8, an external admissions hook configuration is patched.</td></tr><tr>
    <td><code>externaladmissionhookconfigurations.update</code></td>
    <td>In Kubernetes v1.8, an external admissions hook configuration is updated.</td></tr><tr>
    <td><code>horizontalpodautoscalers.create</code></td>
    <td>A horizontal pod autoscaling policy is created.</td></tr><tr>
    <td><code>horizontalpodautoscalers.delete</code></td>
    <td>A horizontal pod autoscaling policy is deleted.</td></tr><tr>
    <td><code>horizontalpodautoscalers.patch</code></td>
    <td>A horizontal pod autoscaling policy is patched.</td></tr><tr>
    <td><code>horizontalpodautoscalers.update</code></td>
    <td>A horizontal pod autoscaling policy is updated.</td></tr><tr>
    <td><code>ingresses.create</code></td>
    <td>An Ingress ALB is created.</td></tr><tr>
    <td><code>ingresses.delete</code></td>
    <td>An Ingress ALB is deleted.</td></tr><tr>
    <td><code>ingresses.patch</code></td>
    <td>An Ingress ALB is patched.</td></tr><tr>
    <td><code>ingresses.update</code></td>
    <td>An Ingress ALB is updated.</td></tr><tr>
    <td><code>jobs.create</code></td>
    <td>A job is created.</td></tr><tr>
    <td><code>jobs.delete</code></td>
    <td>A job is deleted.</td></tr><tr>
    <td><code>jobs.patch</code></td>
    <td>A job is patched.</td></tr><tr>
    <td><code>jobs.update</code></td>
    <td>A job is updated.</td></tr><tr>
    <td><code>localsubjectaccessreviews.create</code></td>
    <td>A local subject access review is created.</td></tr><tr>
    <td><code>limitranges.create</code></td>
    <td>A range limit is created.</td></tr><tr>
    <td><code>limitranges.delete</code></td>
    <td>A range limit is deleted.</td></tr><tr>
    <td><code>limitranges.patch</code></td>
    <td>A range limit is patched.</td></tr><tr>
    <td><code>limitranges.update</code></td>
    <td>A range limit is updated.</td></tr><tr>
    <td><code>mutatingwebhookconfigurations.create</code></td>
    <td>A mutating webhook configuration is created.</td></tr><tr>
    <td><code>mutatingwebhookconfigurations.delete</code></td>
    <td>A mutating webhook configuration is deleted.</td></tr><tr>
    <td><code>mutatingwebhookconfigurations.patch</code></td>
    <td>A mutating webhook configuration is patched.</td></tr><tr>
    <td><code>mutatingwebhookconfigurations.update</code></td>
    <td>A mutating webhook configuration is updated.</td></tr><tr>
    <td><code>namespaces.create</code></td>
    <td>A namespace is created.</td></tr><tr>
    <td><code>namespaces.delete</code></td>
    <td>A namespace is deleted.</td></tr><tr>
    <td><code>namespaces.patch</code></td>
    <td>A namespace is patched.</td></tr><tr>
    <td><code>namespaces.update</code></td>
    <td>A namespace is updated.</td></tr><tr>
    <td><code>networkpolicies.create</code></td>
    <td>A network policy is created.</td></tr><tr><tr>
    <td><code>networkpolicies.delete</code></td>
    <td>A network policy is deleted.</td></tr><tr>
    <td><code>networkpolicies.patch</code></td>
    <td>A network policy is patched.</td></tr><tr>
    <td><code>networkpolicies.update</code></td>
    <td>A network policy is updated.</td></tr><tr>
    <td><code>nodes.create</code></td>
    <td>A node is created.</td></tr><tr>
    <td><code>nodes.delete</code></td>
    <td>A node is deleted.</td></tr><tr>
    <td><code>nodes.patch</code></td>
    <td>A node is patched.</td></tr><tr>
    <td><code>nodes.update</code></td>
    <td>A node is updated.</td></tr><tr>
    <td><code>persistentvolumeclaims.create</code></td>
    <td>A persistent volume claim is created.</td></tr><tr>
    <td><code>persistentvolumeclaims.delete</code></td>
    <td>A persistent volume claim is deleted.</td></tr><tr>
    <td><code>persistentvolumeclaims.patch</code></td>
    <td>A persistent volume claim is patched.</td></tr><tr>
    <td><code>persistentvolumeclaims.update</code></td>
    <td>A persistent volume claim is updated.</td></tr><tr>
    <td><code>persistentvolumes.create</code></td>
    <td>A persistent volume is created.</td></tr><tr>
    <td><code>persistentvolumes.delete</code></td>
    <td>A persistent volume is deleted.</td></tr><tr>
    <td><code>persistentvolumes.patch</code></td>
    <td>A persistent volume is patched.</td></tr><tr>
    <td><code>persistentvolumes.update</code></td>
    <td>A persistent volume is updated.</td></tr><tr>
    <td><code>poddisruptionbudgets.create</code></td>
    <td>A pod disruption budget is created.</td></tr><tr>
    <td><code>poddisruptionbudgets.delete</code></td>
    <td>A pod disruption budget is deleted.</td></tr><tr>
    <td><code>poddisruptionbudgets.patch</code></td>
    <td>A pod disruption budget is patched.</td></tr><tr>
    <td><code>poddisruptionbudgets.update</code></td>
    <td>A pod disruption budget is updated.</td></tr><tr>
    <td><code>podpresets.create</code></td>
    <td>A pod preset is created.</td></tr><tr>
    <td><code>podpresets.deleted</code></td>
    <td>A pod preset is deleted.</td></tr><tr>
    <td><code>podpresets.patched</code></td>
    <td>A pod preset is patched.</td></tr><tr>
    <td><code>podpresets.updated</code></td>
    <td>A pod preset is updated.</td></tr><tr>
    <td><code>pods.create</code></td>
    <td>A pod is created.</td></tr><tr>
    <td><code>pods.delete</code></td>
    <td>A pod is deleted.</td></tr><tr>
    <td><code>pods.patch</code></td>
    <td>A pod is patched.</td></tr><tr>
    <td><code>pods.update</code></td>
    <td>A pod is updated.</td></tr><tr>
    <td><code>podsecuritypolicies.create</code></td>
    <td>A pod security policy is created.</td></tr><tr>
    <td><code>podsecuritypolicies.delete</code></td>
    <td>A pod security policy is deleted.</td></tr><tr>
    <td><code>podsecuritypolicies.patch</code></td>
    <td>A pod security policy is patched.</td></tr><tr>
    <td><code>podsecuritypolicies.update</code></td>
    <td>A pod security policy is updated.</td></tr><tr>
    <td><code>podtemplates.create</code></td>
    <td>A pod template is created.</td></tr><tr>
    <td><code>podtemplates.delete</code></td>
    <td>A pod template is deleted.</td></tr><tr>
    <td><code>podtemplates.patch</code></td>
    <td>A pod template is patched.</td></tr><tr>
    <td><code>podtemplates.update</code></td>
    <td>A pod template is updated.</td></tr><tr>
    <td><code>replicasets.create</code></td>
    <td>A replica set is created.</td></tr><tr>
    <td><code>replicasets.delete</code></td>
    <td>A replica set is deleted.</td></tr><tr>
    <td><code>replicasets.patch</code></td>
    <td>A replica set is patched.</td></tr><tr>
    <td><code>replicasets.update</code></td>
    <td>A replica set is updated.</td></tr><tr>
    <td><code>replicationcontrollers.create</code></td>
    <td>A replication controller is created.</td></tr><tr>
    <td><code>replicationcontrollers.delete</code></td>
    <td>A replication controller is deleted.</td></tr><tr>
    <td><code>replicationcontrollers.patch</code></td>
    <td>A replication controller is patched.</td></tr><tr>
    <td><code>replicationcontrollers.update</code></td>
    <td>A replication controller is updated.</td></tr><tr>
    <td><code>resourcequotas.create</code></td>
    <td>A resource quota is created.</td></tr><tr>
    <td><code>resourcequotas.delete</code></td>
    <td>A resource quota is deleted.</td></tr><tr>
    <td><code>resourcequotas.patch</code></td>
    <td>A resource quota is patched.</td></tr><tr>
    <td><code>resourcequotas.update</code></td>
    <td>A resource quota is updated.</td></tr><tr>
    <td><code>rolebindings.create</code></td>
    <td>A role binding is created.</td></tr><tr>
    <td><code>rolebindings.deleted</code></td>
    <td>A role binding is deleted.</td></tr><tr>
    <td><code>rolebindings.patched</code></td>
    <td>A role binding is patched.</td></tr><tr>
    <td><code>rolebindings.updated</code></td>
    <td>A role binding is updated.</td></tr><tr>
    <td><code>roles.create</code></td>
    <td>A role is created.</td></tr><tr>
    <td><code>roles.deleted</code></td>
    <td>A role is deleted.</td></tr><tr>
    <td><code>roles.patched</code></td>
    <td>A role is patched.</td></tr><tr>
    <td><code>roles.updated</code></td>
    <td>A role is updated.</td></tr><tr>
    <td><code>secrets.create</code></td>
    <td>A secret is created.</td></tr><tr>
    <td><code>secrets.deleted</code></td>
    <td>A secret is deleted.</td></tr><tr>
    <td><code>secrets.get</code></td>
    <td>A secret is viewed.</td></tr><tr>
    <td><code>secrets.patch</code></td>
    <td>A secret is patched.</td></tr><tr>
    <td><code>secrets.updated</code></td>
    <td>A secret is updated.</td></tr><tr>
    <td><code>selfsubjectaccessreviews.create</code></td>
    <td>A self-subject access review is created.</td></tr><tr>
    <td><code>selfsubjectrulesreviews.create</code></td>
    <td>A self-subject rules review is created.</td></tr><tr>
    <td><code>subjectaccessreviews.create</code></td>
    <td>A subject access review is created.</td></tr><tr>
    <td><code>serviceaccounts.create</code></td>
    <td>A service account is created.</td></tr><tr>
    <td><code>serviceaccounts.deleted</code></td>
    <td>A service account is deleted.</td></tr><tr>
    <td><code>serviceaccounts.patch</code></td>
    <td>A service account is patched.</td></tr><tr>
    <td><code>serviceaccounts.updated</code></td>
    <td>A service account is updated.</td></tr><tr>
    <td><code>services.create</code></td>
    <td>A service is created.</td></tr><tr>
    <td><code>services.deleted</code></td>
    <td>A service is deleted.</td></tr><tr>
    <td><code>services.patch</code></td>
    <td>A service is patched.</td></tr><tr>
    <td><code>services.updated</code></td>
    <td>A service is updated.</td></tr><tr>
    <td><code>statefulsets.create</code></td>
    <td>A stateful set is created.</td></tr><tr>
    <td><code>statefulsets.delete</code></td>
    <td>A stateful set is deleted.</td></tr><tr>
    <td><code>statefulsets.patch</code></td>
    <td>A stateful set is patched.</td></tr><tr>
    <td><code>statefulsets.update</code></td>
    <td>A stateful set is updated.</td></tr><tr>
    <td><code>tokenreviews.create</code></td>
    <td>A token review is created.</td></tr><tr>
    <td><code>validatingwebhookconfigurations.create</code></td>
    <td>A webhook configuration validation is created.</td></tr><tr>
    <td><code>validatingwebhookconfigurations.delete</code></td>
    <td>A webhook configuration validation is deleted.</td></tr><tr>
    <td><code>validatingwebhookconfigurations.patch</code></td>
    <td>A webhook configuration validation is patched.</td></tr><tr>
    <td><code>validatingwebhookconfigurations.update</code></td>
    <td>A webhook configuration validation is updated.</td></tr>
</table>
