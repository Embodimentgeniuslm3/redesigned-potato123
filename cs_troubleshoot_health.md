---

copyright:
  years: 2014, 2019
lastupdated: "2019-07-10"

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


# Troubleshooting logging and monitoring 
{: #cs_troubleshoot_health}

As you use {{site.data.keyword.containerlong}}, consider these techniques for troubleshooting issues with logging and monitoring.
{: shortdesc}

If you have a more general issue, try out [cluster debugging](/docs/containers?topic=containers-cs_troubleshoot).
{: tip}

## Logs do not appear
{: #cs_no_logs}

{: tsSymptoms}
When you access the Kibana dashboard, your logs do not display.

{: tsResolve}
Review the following reasons why your cluster logs are not appearing and the corresponding troubleshooting steps:

<table>
<caption>Troubleshooting logs that do not display</caption>
  <col width="40%">
  <col width="60%">
  <thead>
    <tr>
      <th>Why it's happening</th>
      <th>How to fix it</th>
    </tr>
 </thead>
 <tbody>
  <tr>
    <td>No logging configuration is set up.</td>
    <td>In order for logs to be sent, you must create a logging configuration. To do so, see <a href="/docs/containers?topic=containers-health#logging">Configuring cluster logging</a>.</td>
  </tr>
  <tr>
    <td>The cluster is not in a <code>Normal</code> state.</td>
    <td>To check the state of your cluster, see <a href="/docs/containers?topic=containers-cs_troubleshoot#debug_clusters">Debugging clusters</a>.</td>
  </tr>
  <tr>
    <td>The log storage quota was met.</td>
    <td>To increase your log storage limits, see the <a href="/docs/services/CloudLogAnalysis/troubleshooting?topic=cloudloganalysis-error_msgs">{{site.data.keyword.loganalysislong_notm}} documentation</a>.</td>
  </tr>
  <tr>
    <td>If you specified a space at cluster creation, the account owner does not have Manager, Developer, or Auditor permissions to that space.</td>
      <td>To change access permissions for the account owner:
      <ol><li>To find out who the account owner for the cluster is, run <code>ibmcloud ks api-key-info --cluster &lt;cluster_name_or_ID&gt;</code>.</li>
      <li>To grant that account owner Manager, Developer, or Auditor {{site.data.keyword.containerlong_notm}} access permissions to the space, see <a href="/docs/containers?topic=containers-users">Managing cluster access</a>.</li>
      <li>To refresh the logging token after permissions are changed, run <code>ibmcloud ks logging-config-refresh --cluster &lt;cluster_name_or_ID&gt;</code>.</li></ol></td>
    </tr>
    <tr>
      <td>You have a logging configuration for your app with a symlink in your app path.</td>
      <td><p>In order for logs to be sent, you must use an absolute path in your logging configuration or the logs cannot be read. If your path is mounted to your worker node, it might create a symlink.</p> <p>Example: If the specified path is <code>/usr/local/<b>spark</b>/work/app-0546/0/stderr</code> but the logs go to <code>/usr/local/<b>spark-1.0-hadoop-1.2</b>/work/app-0546/0/stderr</code>, then the logs cannot be read.</p></td>
    </tr>
  </tbody>
</table>

To test changes you made during troubleshooting, you can deploy *Noisy*, a sample pod that produces several log events, to a worker node in your cluster.

Before you begin: [Log in to your account. If applicable, target the appropriate resource group. Set the context for your cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1. Create the `deploy-noisy.yaml` configuration file.
    ```
    apiVersion: v1
    kind: Pod
    metadata:
      name: noisy
    spec:
      containers:
      - name: noisy
        image: ubuntu:16.04
        command: ["/bin/sh"]
        args: ["-c", "while true; do sleep 10; echo 'Hello world!'; done"]
        imagePullPolicy: "Always"
      ```
      {: codeblock}

2. Run the configuration file in the cluster's context.
    ```
    kubectl apply -f noisy.yaml
    ```
    {:pre}

3. After a few minutes, you can view your logs in the Kibana dashboard. To access the Kibana dashboard, go to one of the following URLs and select the {{site.data.keyword.cloud_notm}} account where you created the cluster. If you specified a space at cluster creation, go to that space instead.
    - US-South and US-East: `https://logging.ng.bluemix.net`
    - UK-South: `https://logging.eu-gb.bluemix.net`
    - EU-Central: `https://logging.eu-fra.bluemix.net`
    - AP-South: `https://logging.au-syd.bluemix.net`

<br />


## Kubernetes dashboard does not display utilization graphs
{: #cs_dashboard_graphs}

{: tsSymptoms}
When you access the Kubernetes dashboard, utilization graphs do not display.

{: tsCauses}
Sometimes after a cluster update or worker node reboot, the `kube-dashboard` pod does not update.

{: tsResolve}
Delete the `kube-dashboard` pod to force a restart. The pod is re-created with RBAC policies to access `heapster` for utilization information.

  ```
  kubectl delete pod -n kube-system $(kubectl get pod -n kube-system --selector=k8s-app=kubernetes-dashboard -o jsonpath='{.items..metadata.name}')
  ```
  {: pre}

<br />


## Log quota is too low
{: #quota}

{: tsSymptoms}
You set up a logging configuration in your cluster to forward logs to {{site.data.keyword.loganalysisfull}}. When you view logs, you see an error message that is similar to the following:

```
You have reached the daily quota that is allocated to the Bluemix space {Space GUID} for the IBM® Cloud Log Analysis instance {Instance GUID}. Your current daily allotment is XXX for Log Search storage, which is retained for a period of 3 days, during which it can be searched for in Kibana. This does not affect your log retention policy in Log Collection storage. To upgrade your plan so that you can store more data in Log Search storage per day, upgrade the Log Analysis service plan for this space. For more information about service plans and how to upgrade your plan, see Plans.
```
{: screen}

{: tsResolve}
Review the following reasons why you are hitting your log quota and the corresponding troubleshooting steps:

<table>
<caption>Troubleshooting log storage issues</caption>
  <col width="40%">
  <col width="60%">
  <thead>
    <tr>
      <th>Why it's happening</th>
      <th>How to fix it</th>
    </tr>
 </thead>
 <tbody>
  <tr>
    <td>One or more pods produces a high number of logs.</td>
    <td>You can free up log storage space by preventing the logs from specific pods from being forwarded. Create a [logging filter](/docs/containers?topic=containers-health#filter-logs) for these pods.</td>
  </tr>
  <tr>
    <td>You exceed the 500 MB daily allotment for log storage for the Lite plan.</td>
    <td>First, [calculate the search quota and daily usage](/docs/services/CloudLogAnalysis/how-to?topic=cloudloganalysis-quota) of your logs domain. Then, you can increase your log storage quota by [upgrading your {{site.data.keyword.loganalysisshort_notm}} service plan](/docs/services/CloudLogAnalysis/how-to?topic=cloudloganalysis-change_plan#change_plan).</td>
  </tr>
  <tr>
    <td>You are exceeding the log storage quota for your current paid plan.</td>
    <td>First, [calculate the search quota and daily usage](/docs/services/CloudLogAnalysis/how-to?topic=cloudloganalysis-quota) of your logs domain. Then, you can increase your log storage quota by [upgrading your {{site.data.keyword.loganalysisshort_notm}} service plan](/docs/services/CloudLogAnalysis/how-to?topic=cloudloganalysis-change_plan#change_plan).</td>
  </tr>
  </tbody>
</table>

<br />


## Log lines are too long
{: #long_lines}

{: tsSymptoms}
You set up a logging configuration in your cluster to forward logs to {{site.data.keyword.loganalysisfull_notm}}. When you view logs, you see a long log message. Additionally, in Kibana, you might be able to see only the last 600 - 700 characters of the log message.

{: tsCauses}
A long log message might be truncated due to its length before it is collected by Fluentd, so the log might not be parsed correctly by Fluentd before it is forwarded to {{site.data.keyword.loganalysisshort_notm}}.

{: tsResolve}
To limit line length, you can configure your own logger to have a maximum length for the `stack_trace` in each log. For example, if you are using Log4j for your logger, you can use an [`EnhancedPatternLayout` ![External link icon](../icons/launch-glyph.svg "External link icon")](http://logging.apache.org/log4j/1.2/apidocs/org/apache/log4j/EnhancedPatternLayout.html) to limit the `stack_trace` to 15KB.

## Getting help and support
{: #health_getting_help}

Still having issues with your cluster?
{: shortdesc}

-  In the terminal, you are notified when updates to the `ibmcloud` CLI and plug-ins are available. Be sure to keep your CLI up-to-date so that you can use all available commands and flags.
-   To see whether {{site.data.keyword.cloud_notm}} is available, [check the {{site.data.keyword.cloud_notm}} status page ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/status?selected=status).
-   Post a question in the [{{site.data.keyword.containerlong_notm}} Slack ![External link icon](../icons/launch-glyph.svg "External link icon")](https://ibm-container-service.slack.com).
    If you are not using an IBM ID for your {{site.data.keyword.cloud_notm}} account, [request an invitation](https://cloud.ibm.com/kubernetes/slack) to this Slack.
    {: tip}
-   Review the forums to see whether other users ran into the same issue. When you use the forums to ask a question, tag your question so that it is seen by the {{site.data.keyword.cloud_notm}} development teams.
    -   If you have technical questions about developing or deploying clusters or apps with {{site.data.keyword.containerlong_notm}}, post your question on [Stack Overflow ![External link icon](../icons/launch-glyph.svg "External link icon")](https://stackoverflow.com/questions/tagged/ibm-cloud+containers) and tag your question with `ibm-cloud`, `kubernetes`, and `containers`.
    -   For questions about the service and getting started instructions, use the [IBM Developer Answers ![External link icon](../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix) forum. Include the `ibm-cloud` and `containers` tags.
    See [Getting help](/docs/get-support?topic=get-support-getting-customer-support#using-avatar) for more details about using the forums.
-   Contact IBM Support by opening a case. To learn about opening an IBM support case, or about support levels and case severities, see [Contacting support](/docs/get-support?topic=get-support-getting-customer-support).
When you report an issue, include your cluster ID. To get your cluster ID, run `ibmcloud ks clusters`. You can also use the [{{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool](/docs/containers?topic=containers-cs_troubleshoot#debug_utility) to gather and export pertinent information from your cluster to share with IBM Support.
{: tip}

