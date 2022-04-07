---

copyright:
  years: 2014, 2019
lastupdated: "2019-04-15"

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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}



# Fehlerbehebung für die Protokollierung und Überwachung
{: #cs_troubleshoot_health}

Ziehen Sie bei der Verwendung von {{site.data.keyword.containerlong}} die folgenden Verfahren für die Fehlerbehebung bei der Verwendung von Protokollierung und Überwachung in Betracht.
{: shortdesc}

Wenn Sie ein allgemeineres Problem haben, testen Sie das [Cluster-Debugging](/docs/containers?topic=containers-cs_troubleshoot).
{: tip}

## Protokolle werden nicht angezeigt
{: #cs_no_logs}

{: tsSymptoms}
Beim Zugriff auf das Kibana-Dashboard werden Ihre Protokolle nicht angezeigt.

{: tsResolve}
Überprüfen Sie die folgenden möglichen Ursachen für nicht angezeigte Clusterprotokolle sowie die entsprechenden Fehlerbehebungsschritte:

<table>
<caption>Nicht angezeigte Protokolle zur Fehlerbehebung</caption>
  <col width="40%">
  <col width="60%">
  <thead>
    <tr>
      <th>Mögliche Ursache</th>
      <th>Fehlerbehebungsmaßnahme</th>
    </tr>
 </thead>
 <tbody>
  <tr>
    <td>Es ist keine Protokollierungskonfiguration eingerichtet.</td>
    <td>Damit Protokolle gesendet werden, müssen Sie eine Protokollierungskonfiguration erstellen. Informationen dazu finden Sie unter <a href="/docs/containers?topic=containers-health#logging">Clusterprotokollierung konfigurieren</a>.</td>
  </tr>
  <tr>
    <td>Der Cluster weist nicht den Status <code>Normal</code> auf.</td>
    <td>Informationen darüber, wie Sie den Status des Clusters überprüfen können, finden Sie unter <a href="/docs/containers?topic=containers-cs_troubleshoot#debug_clusters">Cluster debuggen</a>.</td>
  </tr>
  <tr>
    <td>Die Quote für den Protokollspeicher ist erschöpft.</td>
    <td>Informationen darüber, wie Sie die Protokollspeichergrenze erhöhen können, finden Sie in der <a href="/docs/services/CloudLogAnalysis/troubleshooting?topic=cloudloganalysis-error_msgs">{{site.data.keyword.loganalysislong_notm}}-Dokumentation</a>.</td>
  </tr>
  <tr>
    <td>Wenn Sie beim Erstellen des Clusters einen Bereich angegeben haben, verfügt der Kontoeigner nicht über die Berechtigungen eines Managers, Entwicklers oder Prüfers für diesen Bereich.</td>
      <td>Gehen Sie wie folgt vor, um die Zugriffsberechtigungen für den Kontoeigner zu ändern:
      <ol><li>Führen Sie den folgenden Befehl aus, um den Kontoeigner des Clusters zu ermitteln: <code>ibmcloud ks api-key-info</code>.</li>
      <li>Informationen darüber, wie dem betreffenden Kontoeigner die {{site.data.keyword.containerlong_notm}}-Zugriffsberechtigung eines Managers, Entwicklers oder Prüfers für den Bereich zugeordnet werden kann, finden Sie unter <a href="/docs/containers?topic=containers-users">Clusterzugriff verwalten</a>.</li>
      <li>Führen Sie den folgenden Befehl aus, um das Protokollierungstoken nach Änderung der Berechtigungen zu aktualisieren: <code>ibmcloud ks logging-config-refresh --cluster &lt;clustername_oder_-id&gt;</code>.</li></ol></td>
    </tr>
    <tr>
      <td>Sie haben eine Anwendungsprotokollierungskonfiguration mit einer symbolischen Verbindung in Ihrem App-Pfad.</td>
      <td><p>Damit Protokolle gesendet werden, müssen Sie einen absoluten Pfad in Ihrer Protokollierungskonfiguration verwenden. Andernfalls können die Protokolle nicht gelesen werden. Falls Ihr Pfad an Ihren Workerknoten angehängt ist, wurde dadurch möglicherweise die symbolische Verbindung erstellt.</p> <p>Beispiel: Falls der angegebene Pfad <code>/usr/local/<b>spark</b>/work/app-0546/0/stderr</code> lautet, aber die Protokolle an <code>/usr/local/<b>spark-1.0-hadoop-1.2</b>/work/app-0546/0/stderr</code> gesendet werden, können sie nicht gelesen werden.</p></td>
    </tr>
  </tbody>
</table>

Um Änderungen zu testen, die Sie während der Fehlerbehebung vorgenommen haben, können Sie *Noisy* auf einem Workerknoten in Ihrem Cluster bereitstellen. Dabei handelt es sich um einen Beispiel-Pod, der mehrere Protokollereignisse generiert.

Vorbereitende Schritte: [Melden Sie sich an Ihrem Konto an. Geben Sie als Ziel die entsprechende Region und, sofern zutreffend, die Ressourcengruppe an. Legen Sie den Kontext für den Cluster fest.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1. Erstellen Sie die Konfigurationsdatei `deploy-noisy.yaml`.
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

2. Führen Sie die Konfigurationsdatei in dem Kontext des Clusters aus.
    ```
    kubectl apply -f noisy.yaml
    ```
    {:pre}

3. Nach einigen Minuten werden die Protokolle im Kibana-Dashboard angezeigt. Zum Zugriff auf das Kibana-Dashboard müssen Sie eine der folgenden URLs aufrufen und dann das {{site.data.keyword.Bluemix_notm}}-Konto, in dem Sie den Cluster erstellt haben, auswählen. Wenn Sie beim Erstellen des Clusters einen Bereich angegeben haben, wechseln Sie stattdessen zu diesem Bereich.
    - Vereinigte Staaten (Süden) und Vereinigte Staaten (Osten): `https://logging.ng.bluemix.net`
    - Vereinigtes Königreich (Süden): `https://logging.eu-gb.bluemix.net`
    - Mitteleuropa: `https://logging.eu-fra.bluemix.net`
    - Asiatisch-pazifischer Raum (Süden): `https://logging.au-syd.bluemix.net`

<br />


## Im Kubernetes-Dashboard werden keine Nutzungsdiagramme angezeigt
{: #cs_dashboard_graphs}

{: tsSymptoms}
Beim Zugriff auf das Kubernetes-Dashboard werden keine Nutzungsdiagramme angezeigt.

{: tsCauses}
Nach einer Clusteraktualisierung oder dem Neustart eines Workerknotens kann es vorkommen, dass der Pod `kube-dashboard` nicht aktualisiert wird.

{: tsResolve}
Löschen Sie den Pod `kube-dashboard`, um einen Neustart zu erzwingen. Der Pod wird mit RBAC-Richtlinien erneut erstellt, um die Informationen zur Auslastung von `heapster` abzurufen.

  ```
  kubectl delete pod -n kube-system $(kubectl get pod -n kube-system --selector=k8s-app=kubernetes-dashboard -o jsonpath='{.items..metadata.name}')
  ```
  {: pre}

<br />


## Protokollkontingent (Quote) ist zu niedrig
{: #quota}

{: tsSymptoms}
Sie richten eine Protokollierungskonfiguration in Ihrem Cluster ein, um Protokolle an {{site.data.keyword.loganalysisfull}} weiterzuleiten. Wenn Sie Protokolle anzeigen, wird die folgende oder eine ähnliche Fehlernachricht angezeigt:

```
You have reached the daily quota that is allocated to the Bluemix space {Space GUID} for the IBM® Cloud Log Analysis instance {Instance GUID}. Your current daily allotment is XXX for Log Search storage, which is retained for a period of 3 days, during which it can be searched for in Kibana. This does not affect your log retention policy in Log Collection storage. To upgrade your plan so that you can store more data in Log Search storage per day, upgrade the Log Analysis service plan for this space. For more information about service plans and how to upgrade your plan, see Plans.
```
{: screen}

{: tsResolve}
Prüfen Sie die folgenden Ursachen, aus denen Sie Ihr Protokollkontingent erschöpfen, sowie die entsprechenden Schritte zur Fehlerbehebung:

<table>
<caption>Fehlerbehebung bei Problemen mit Protokollspeicher</caption>
  <col width="40%">
  <col width="60%">
  <thead>
    <tr>
      <th>Mögliche Ursache</th>
      <th>Fehlerbehebungsmaßnahme</th>
    </tr>
 </thead>
 <tbody>
  <tr>
    <td>Einer oder mehrere der Pods generieren eine große Menge an Protokollen.</td>
    <td>Sie können Protokollspeicherplatz freigeben, indem Sie verhindern, dass Protokolle aus bestimmten Pods weitergeleitet werden. Erstellen Sie einen [Protokollfilter](/docs/containers?topic=containers-health#filter-logs) für diese Pods.</td>
  </tr>
  <tr>
    <td>Sie überschreiten die tägliche Zuordnung von 500 MB an Protokollspeicher für den Lite-Plan.</td>
    <td>[Berechnen Sie zuerst das Suchkontingent und die tägliche Nutzung](/docs/services/CloudLogAnalysis/how-to?topic=cloudloganalysis-quota) für Ihre Protokolldomäne. Anschließend können Sie Ihr Protokollspeicherkontingent erhöhen, indem Sie [Ihren {{site.data.keyword.loganalysisshort_notm}}-Serviceplan aufstocken](/docs/services/CloudLogAnalysis/how-to?topic=cloudloganalysis-change_plan#change_plan).</td>
  </tr>
  <tr>
    <td>Sie überschreiten das Protokollspeicherkontingent für Ihren aktuellen bezahlten Plan.</td>
    <td>[Berechnen Sie zuerst das Suchkontingent und die tägliche Nutzung](/docs/services/CloudLogAnalysis/how-to?topic=cloudloganalysis-quota) für Ihre Protokolldomäne. Anschließend können Sie Ihr Protokollspeicherkontingent erhöhen, indem Sie [Ihren {{site.data.keyword.loganalysisshort_notm}}-Serviceplan aufstocken](/docs/services/CloudLogAnalysis/how-to?topic=cloudloganalysis-change_plan#change_plan).</td>
  </tr>
  </tbody>
</table>

<br />


## Protokollzeilen sind zu lang
{: #long_lines}

{: tsSymptoms}
Sie richten eine Protokollierungskonfiguration in Ihrem Cluster ein, um Protokolle an {{site.data.keyword.loganalysisfull_notm}} weiterzuleiten. Wenn Sie Protokolle anzeigen, sehen Sie eine sehr lange Protokollnachricht. Außerdem ist es möglich, dass in Kibana nur die letzten 600 - 700 Zeichen der Protokollnachricht zu sehen sind.

{: tsCauses}
Eine lange Protokollnachricht kann aufgrund ihrer Länge abgeschnitten werden, bevor sie von Fluentd erfasst wird, sodass das Protokoll von Fluentd möglicherweise nicht ordnungsgemäß analysiert wird, bevor es an {{site.data.keyword.loganalysisshort_notm}} weitergeleitet wird.

{: tsResolve}
Zur Begrenzung der Zeilenlänge können Sie Ihre eigene Protokollfunktion so konfigurieren, dass eine maximale Länge für den Stack-Trace (`stack_trace`) in jedem Protokoll festgelegt wird. Beispiel: Wenn Sie Log4j als Protokollfunktion verwenden, können Sie ein [`EnhancedPatternLayout` ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](http://logging.apache.org/log4j/1.2/apidocs/org/apache/log4j/EnhancedPatternLayout.html) verwenden, um den Stack-Trace (`stack_trace`) auf 15 KB zu begrenzen.

## Hilfe und Unterstützung anfordern
{: #health_getting_help}

Haben Sie noch immer Probleme mit Ihrem Cluster?
{: shortdesc}

-  Sie werden im Terminal benachrichtigt, wenn Aktualisierungen für die `ibmcloud`-CLI und -Plug-ins verfügbar sind. Halten Sie Ihre CLI stets aktuell, sodass Sie alle verfügbaren Befehle und Flags verwenden können.
-   [Überprüfen Sie auf der {{site.data.keyword.Bluemix_notm}}-Statusseite ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/status?selected=status), ob {{site.data.keyword.Bluemix_notm}} verfügbar ist.
-   Veröffentlichen Sie eine Frage im [{{site.data.keyword.containerlong_notm}}-Slack ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://ibm-container-service.slack.com).
    Wenn Sie keine IBM ID für Ihr {{site.data.keyword.Bluemix_notm}}-Konto verwenden, [fordern Sie eine Einladung](https://bxcs-slack-invite.mybluemix.net/) zu diesem Slack an.
    {: tip}
-   Suchen Sie in entsprechenden Foren, ob andere Benutzer auf das gleiche Problem
gestoßen sind. Versehen Sie Ihre Fragen in den Foren mit Tags, um sie für das Entwicklungsteam
von {{site.data.keyword.Bluemix_notm}} erkennbar zu machen.
    -   Wenn Sie technische Fragen zur Entwicklung oder Bereitstellung von Clustern oder Apps mit {{site.data.keyword.containerlong_notm}} haben, veröffentlichen Sie Ihre Frage auf [Stack Overflow ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://stackoverflow.com/questions/tagged/ibm-cloud+containers) und versehen Sie sie mit den Tags `ibm-cloud`, `kubernetes` und `containers`.
    -   Verwenden Sie bei Fragen zum Service und zu ersten Schritten das Forum [IBM Developer Answers ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix). Geben Sie die Tags `ibm-cloud` und `containers` an.
    Weitere Details zur Verwendung der Foren finden Sie unter [Hilfe anfordern](/docs/get-support?topic=get-support-getting-customer-support#using-avatar).
-   Wenden Sie sich an den IBM Support, indem Sie einen Fall öffnen. Informationen zum Öffnen eines IBM Supportfalls oder zu Supportstufen und zu Prioritätsstufen von Fällen finden Sie unter [Support kontaktieren](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support).
Geben Sie beim Melden eines Problems Ihre Cluster-ID an. Führen Sie den Befehl `ibmcloud ks clusters` aus, um Ihre Cluster-ID abzurufen. Sie können außerdem [{{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool](/docs/containers?topic=containers-cs_troubleshoot#debug_utility) verwenden, um relevante Informationen aus Ihrem Cluster zu erfassen und zu exportieren, um sie dem IBM Support zur Verfügung zu stellen.
{: tip}

