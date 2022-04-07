---

copyright:
  years: 2014, 2019
lastupdated: "2019-07-18"

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


# Setting pod priority
{: #pod_priority}

With Kubernetes pod priority and preemption, you can configure priority classes to indicate the relative priority of a pod. The Kubernetes scheduler takes into consideration the priority of a pod and can even preempt (remove) pods with lower priority to make room on a worker node for higher priority pods. Your {{site.data.keyword.containerlong}} clusters that run Kubernetes version 1.11.2 or later support the `Priority` admission controller that enforces these classes.
{: shortdesc}

**Why do I set pod priority?**</br>
As a cluster administrator, you want to control which pods are more critical to your cluster workload. Priority classes can help you control the Kubernetes scheduler decisions to favor higher priority pods over lower priority pods. The scheduler can even preempt (remove) lower priority pods that are running so that pending higher priority pods can be scheduled.

By setting pod priority, you can help prevent lower priority workloads from impacting critical workloads in your cluster, especially in cases where the cluster starts to reach its resource capacity.

Make sure that you have [set up proper user access](/docs/containers?topic=containers-users#users) to your cluster, and if applicable, [pod security policies](/docs/containers?topic=containers-psp#psp). Access and pod security policies can help prevent untrusted users from deploying high priority pods that prevent other pods from scheduling.
{: tip}

{: #priority_scheduling}
**How does priority scheduling and preemption work?**</br>
In general, pending pods that have a higher priority are scheduled before lower prioritized pods. If you do not have enough resources left in your worker nodes, the scheduler can preempt (remove) pods to free up enough resources for the higher prioritized pods to be scheduled. Preemption is also affected by graceful termination periods, pod disruption budgets, and worker node affinity.

If you do not specify a priority for your pod deployment, the default is set to the priority class that is set as the `globalDefault` . If you do not have a `globalDefault` priority class, the default priority for all pods is zero (`0`). By default, {{site.data.keyword.containerlong_notm}} does not set a `globalDefault`, so the pod default priority is zero.

To understand how pod priority and scheduler work together, consider the scenarios in the following figure. You must place prioritized pods on worker nodes with available resources. Otherwise, high priority pods in your cluster can remain in pending at the same time that existing pods are removed, such as in Scenario 3.

_Figure: Pod priority scenarios_
<img src="images/pod-priority.png" width="500" alt="Pod priority scenarios" style="width:500px; border-style: none"/>

1.  Three pods with high, medium, and low priority are pending scheduling. The scheduler finds an available worker node with room for all three pods, and schedules them in order of priority, with the highest priority pod scheduled first.
2.  Three pods with high, medium, and low priority are pending scheduling. The scheduler finds an available worker node, but the worker node has only enough resources to support the high and medium priority pods. The low-priority pod is not scheduled and it remains in pending.
3.  Two pods with high and medium priority are pending scheduling. A third pod with low priority exists on an available worker node. However, the worker node does not have enough resources to schedule any of the pending pods. The scheduler preempts, or removes, the low-priority pod, which returns the pod to a pending state. Then, the scheduler tries to schedule the high priority pod. However, the worker node does not have enough resources to schedule the high priority pod, and instead, the scheduler schedules the medium priority pod.

**For more information**: See the Kubernetes documentation on [pod priority and preemption ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/configuration/pod-priority-preemption/).

**Can I disable the pod priority admission controller?**</br>
No. If you don't want to use pod priority, don't set a `globalDefault` or include a priority class in your pod deployments. Every pod defaults to zero, except the cluster-critical pods that IBM deploys with the [default priority classes](#default_priority_class). Because pod priority is relative, this basic setup ensures that the cluster-critical pods are prioritized for resources, and schedules any other pods by following the existing scheduling policies that you have in place.

**How do resource quotas affect pod priority?**</br>
You can use pod priority in combination with resource quotas, including [quota scopes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/policy/resource-quotas/#quota-scopes) for clusters that run Kubernetes 1.12 or later. With quota scopes, you can set up your resource quotas to account for pod priority. Higher priority pods get to consume system resources that are limited by the resource quota before lower priority pods.

## Understanding default priority classes
{: #default_priority_class}

Your {{site.data.keyword.containerlong_notm}} clusters come with some priority classes by default.
{: shortdesc}

Do not modify the default classes, which are used to properly manage your cluster. You can use these classes in your app deployments, or [create your own priority classes](#create_priority_class).
{: important}

The following table describes the priority classes that are in your cluster by default and why they are used.

| Name | Set by | Priority Value | Purpose |
|---|---|---|
| `system-node-critical` | Kubernetes | 2000001000 | Select pods that are deployed into the `kube-system` namespace when you create the cluster use this priority class to protect critical functionality for worker nodes, such as for networking, storage, logging, monitoring, and metrics pods. |
| `system-cluster-critical` | Kubernetes | 2000000000 | Select pods that are deployed into the `kube-system` namespace when you create the cluster use this priority class to protect critical functionality for clusters, such as for networking, storage, logging, monitoring, and metrics pods. |
| `ibm-app-cluster-critical` | IBM | 900000000 | Select pods that are deployed into the `ibm-system` namespace when you create the cluster use this priority class to protect critical functionality for apps, such as the load balancer pods. |
{: caption="Default priority classes that you must not modify" caption-side="top"}

You can check which pods use the priority classes by running the following command.

```
kubectl get pods --all-namespaces -o custom-columns=NAME:.metadata.name,PRIORITY:.spec.priorityClassName
```
{: pre}

## Creating a priority class
{: #create_priority_class}

To set pod priority, you need to use a priority class.
{: shortdesc}

Before you begin:
* [Log in to your account. If applicable, target the appropriate resource group. Set the context for your cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
* Ensure that you have the [**Writer** or **Manager** {{site.data.keyword.cloud_notm}} IAM service role](/docs/containers?topic=containers-users#platform) for the `default` namespace.
* [Create](/docs/containers?topic=containers-clusters#clusters_ui) or [update](/docs/containers?topic=containers-update#update) your cluster to Kubernetes version 1.11 or later.

To use a priority class:

1.  Optional: Use an existing priority class as a template for the new class.

    1.  List existing priority classes.

        ```
        kubectl get priorityclasses
        ```
        {: pre}

    2.  Choose the priority class that you want to copy and create a local YAML file.

        ```
        kubectl get priorityclass <priority_class> -o yaml > Downloads/priorityclass.yaml
        ```
        {: pre}

2.  Make your priority class YAML file.

    ```yaml
    apiVersion: scheduling.k8s.io/v1alpha1
    kind: PriorityClass
    metadata:
      name: <priority_class_name>
    value: <1000000>
    globalDefault: <false>
    description: "Use this class for XYZ service pods only."
    ```
    {: codeblock}

    <table>
    <caption>Understanding the YAML file components</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
    </thead>
    <tbody>
    <tr>
    <td><code>metadata.name</code></td>
    <td>Required: The name of the priority class that you want to create.</td>
    </tr>
    <tr>
    <td><code>value</code></td>
    <td>Required: Enter an integer less than or equal to 1 billion (1000000000). The higher the value, the higher the priority. Values are relative to the values of other priority classes in the cluster. Reserve very high numbers for system critical pods that you do not want to be preempted (removed). </br></br>For example, the [default cluster-critical priority classes](#default_priority_class) range in value from 900000000-2000001000, so enter a value less than these numbers for new priority classes so that nothing is prioritized higher than these pods.</td>
    </tr>
    <tr>
    <td><code>globalDefault</code></td>
    <td>Optional: Set the field to `true` to make this priority class the global default that is applied to every pod that is scheduled without a `priorityClassName` value. Only one priority class in your cluster can be set as the global default. If there is no global default, pods with no `priorityClassName` specified have a priority of zero (`0`).</br></br>
    The [default priority classes](#default_priority_class) do not set a `globalDefault`. If you created other priority classes in your cluster, you can check to make sure that they do not set a `globalDefault` by running `kubectl describe priorityclass <name>`.</td>
    </tr>
    <tr>
    <td><code>description</code></td>
    <td>Optional: Tell users why to use this priority class. Enclose the string in quotations (`""`).</td>
    </tr></tbody></table>

3.  Create the priority class in your cluster.

    ```
    kubectl apply -f filepath/priorityclass.yaml
    ```
    {: pre}

4.  Verify that the priority class is created.

    ```
    kubectl get priorityclasses
    ```
    {: pre}

Great! You created a priority class. Let your team know about the priority class and which priority class, if any, that they must use for their pod deployments.  

## Assigning priority to your pods
{: #prioritize}

Assign a priority class to your pod spec to set the pod's priority within your {{site.data.keyword.containerlong_notm}} cluster. If your pods existed before priority classes became available with Kubernetes version 1.11, you must edit the pod YAML files to assign the pods a priority.
{: shortdesc}

Before you begin:
* [Log in to your account. If applicable, target the appropriate resource group. Set the context for your cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
* Ensure that you have the [**Writer** or **Manager** {{site.data.keyword.cloud_notm}} IAM service role](/docs/containers?topic=containers-users#platform) in the namespace that you want to deploy the pods to.
* [Create](/docs/containers?topic=containers-clusters#clusters_ui) or [update](/docs/containers?topic=containers-update#update) your cluster to Kubernetes version 1.11 or later.
* [Understand how priority scheduling works](#priority_scheduling), as priority can preempt existing pods and affect how your cluster's resources are consumed.

To assign priority to your pods:

1.  Check the importance of other deployed pods so that you can choose the right priority class for your pods in relation to what already is deployed.

    1.  View the priority classes that other pods in the namespace use.

        ```
        kubectl get pods -n <namespace> -o custom-columns=NAME:.metadata.name,PRIORITY:.spec.priorityClassName
        ```
        {: pre}

    2.  Get the details of the priority class and note the **value** number. Pods with higher numbers are prioritized before pods with lower numbers. Repeat this step for each priority class that you want to review.

        ```
        kubectl describe priorityclass <priorityclass_name>
        ```
        {: pre}

2.  Get the priority class that you want to use, or [create your own priority class](#create_priority_class).

    ```
    kubectl get priorityclasses
    ```
    {: pre}

3.  In your pod spec, add the `priorityClassName` field with the name of the priority class that you retrieved in the previous step.

    ```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: ibmliberty
    spec:
      replicas: 1
      selector:
        matchLabels:
          app: ibmliberty
      template:
        metadata:
          labels:
            app: ibmliberty
        spec:
          containers:
          - name: ibmliberty
            image: icr.io/ibmliberty:latest
            ports:
            - containerPort: 9080
          priorityClassName: <priorityclass_name>
    ```
    {: codeblock}

4.  Create your prioritized pods in the namespace that you want to deploy them to.

    ```
    kubectl apply -f filepath/pod-deployment.yaml
    ```
    {: pre}
