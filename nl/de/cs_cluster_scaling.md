---

copyright:
  years: 2014, 2019
lastupdated: "2019-04-17"

keywords: kubernetes, iks, node scaling

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
{:gif: data-image-type='gif'}



# Cluster skalieren
{: #ca}

Mit dem {{site.data.keyword.containerlong_notm}}-Plug-in `ibm-iks-cluster-autoscaler` können Sie die Worker-Pools in Ihrem Cluster automatisch skalieren, um die Anzahl von Workerknoten im Worker-Pool je nach Kapazitätsbedarf der geplanten Workloads zu erhöhen oder zu verringern. Das Plug-in `ibm-iks-cluster-autoscaler` basiert auf dem [Kubernetes-Projekt 'Cluster-Autoscaler' ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/autoscaler/tree/master/cluster-autoscaler).
{: shortdesc}

Bevorzugen Sie eine automatische Skalierung Ihrer Pods? Weitere Informationen dazu finden Sie unter [Apps skalieren](/docs/containers?topic=containers-app#app_scaling).
{: tip}

Der Cluster-Autoscaler ist für Standardcluster verfügbar, die mit öffentlicher Netzkonnektivität eingerichtet sind. Wenn Ihr Cluster auf das öffentliche Netz nicht zugreifen kann, wie dies zum Beispiel bei einem privaten Cluster hinter einer Firewall oder bei einem Cluster mit nur einem aktivierten privaten Serviceendpunkt der Fall ist, können Sie den Cluster-Autoscaler in Ihrem Cluster nicht verwenden.
{: important}

## Informationen über Scale-up- und Scale-down-Operationen
{: #ca_about}

Der Cluster-Autoscaler prüft den Cluster in regelmäßigen Abständen, um die Anzahl der Workerknoten in den Worker-Pools, die er verwaltet, als Reaktion auf Ihre Workloadressourcenanforderungen und entsprechend den von Ihnen konfigurierten angepassten Einstellungen, wie zum Beispiel Scanintervalle, anzupassen. Der Cluster-Autoscaler prüft jede Minute auf die folgenden Situationen.
{: shortdesc}

*   **Anstehende Pods für Scale-up:** Ein Pod gilt als anstehend, wenn nicht ausreichende Rechenressourcen vorhanden sind, um den Pod auf einem Workerknoten zu planen. Wenn der Cluster-Autoscaler anstehende Pods erkennt, fügt der Autoscaler Workerknoten durch ein gleichmäßiges Scale-up über Zonen hinweg hinzu, um die Workloadressourcenanforderungen zu erfüllen.
*   **Nicht ausgelastete Workerknoten für Scale-down:** Standardmäßig werden Workerknoten, die über 10 Minuten oder länger mit weniger als 50% der angeforderten Gesamtrechenressourcen arbeiten und deren Workloads auf anderen Workerknoten neu geplant werden können, als nicht ausgelastet betrachtet. Wenn der Cluster-Autoscaler nicht ausgelastete Workerknoten erkennt, führt er ein Scale-down Ihrer Workerknoten um jeweils einen pro Mal durch, sodass nur die Rechenressourcen vorhanden sind, die benötigt werden. Falls Sie dies wünschen, können Sie den standardmäßig verwendeten Auslastungsschwellenwert von 50% für 10 Minuten für das Scale-down [anpassen](/docs/containers?topic=containers-ca#ca_chart_values).

Das Prüfen (Scannen) und Durchführen von Scale-up- und Scale-down-Operationen erfolgt in regelmäßigen Intervallen im Verlauf der Zeit und kann abhängig von der Anzahl der Workerknoten auch eine längere Zeit (z. B. 30 Minuten) dauern.

Der Cluster-Autoscaler passt die Anzahl von Workerknoten an, indem er die [Ressourcenanforderungen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/), die Sie für Ihre Bereitstellungen definieren, und nicht die tatsächliche Nutzung von Workerknoten betrachtet. Falls Ihre Pods und Bereitstellungen keine angemessenen Mengen von Ressourcen anfordern, müssen Sie die Konfigurationsdateien der Pods anpassen. Diese Anpassung kann der Cluster-Autoscaler nicht für Sie vornehmen. Beachten Sie darüber hinaus, dass Workerknoten einen Teil ihrer Rechenressourcen für die grundlegende Clusterfunktionalität, für Standard-Add-ons und angepasste [Add-ons](/docs/containers?topic=containers-update#addons) sowie für [Ressourcenreserven](/docs/containers?topic=containers-plan_clusters#resource_limit_node) verwenden.
{: note}

<br>
**Wie werden Scale-ups und Scale-downs durchgeführt?**<br>
Im Allgemeinen berechnet der Cluster-Autoscaler die Anzahl der Workerknoten, die Ihr Cluster zur Ausführung seiner Workload benötigt. Die Durchführung von Scale-up- oder Scale-down-Operationen für den Cluster hängt von zahlreichen Faktoren ab, wie zum Beispiel den folgenden.
*   Die minimale und maximale Workerknotengröße pro Zone, die Sie festlegen.
*   Ihre Ressourcenanforderungen für anstehende Pods und bestimmte Metadaten, die Sie der Workload zuordnen, wie zum Beispiel Anti-Affinität, Bezeichnungen zur Platzierung von Pods nur auf bestimmten Maschinentypen oder [Budgets für den Podausfall (Pod Disruption Budgets)![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/workloads/pods/disruptions/).
*   Die Worker-Pools, die der Cluster-Autoscaler verwaltet, in einem [Mehrzonencluster](/docs/containers?topic=containers-plan_clusters#multizone) potenziell auch über Zonen hinweg.
*   Die [angepassten Helmdiagramm-Werte](#ca_chart_values), die festgelegt sind, wie z. B. das Überspringen der Workerknoten zum Löschen, wenn der lokale Speicher verwendet wird.

Weitere Informationen finden Sie in den FAQs zu Kubernetes Cluster Autoscaler zu der Frage [Wie funktioniert ein Scale-up? ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#how-does-scale-up-work) sowie [Wie funktioniert ein Scale-down? ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#how-does-scale-down-work).

<br>

**Kann ich die Funktionsweise von Scale-ups und Scale-downs ändern?**<br>
Sie können die Einstellungen anpassen oder andere Kubernetes-Ressourcen verwenden, um die Funktionsweise von Scale-ups und Scale-downs zu ändern.
*   **Scale-up**: [Passen Sie die Cluster-Autoscaler-Helmdiagrammwerte an](#ca_chart_values), z. B. `scanInterval`, `expander`, `skipNodes` oder `maxNodeProvisionTime`. Informieren Sie sich über die Methoden zum [Überbereitstellen von Workerknoten](#ca_scaleup), damit Sie die Workerknoten vertikal skalieren können, bevor einem Worker-Pool die Ressourcen ausgehen. Sie können auch [Kubernetes-Pod-Budget-Unterbrechungen und Pod-Prioritätsgrenzen einrichten](#scalable-practices-apps), um die Funktionsweise des Scale-ups zu ändern.
*   **Scale-down**: [Passen Sie die Cluster-Autoscaler-Helmdiagrammwerte an](#ca_chart_values), z. B. `scaleDownUnneededTime`, `scaleDownDelayAfterAdd`, `scaleDownDelayAfterDelete` oder `scaleDownUtilizationThreshold`.

<br>
**Wie unterscheidet sich dieses Verhalten von Worker-Pools, die nicht vom Cluster-Autoscaler verwaltet werden?**<br>
Wenn Sie einen [Worker-Pool erstellen](/docs/containers?topic=containers-clusters#add_pool), geben Sie an, wie viele Workerknoten pro Zone der Worker-Pool enthält. Der Worker-Pool verwaltet diese Anzahl von Workerknoten, bis Sie seine [Größe ändern](/docs/containers?topic=containers-cs_cli_reference#cs_worker_pool_resize) oder den Worker-Pool [neu ausgleichen](/docs/containers?topic=containers-cs_cli_reference#cs_rebalance). Der Worker-Pool fügt keine Workerknoten für Sie hinzu und entfernt auch keine Workerknoten. Wenn Sie mehr Pods haben, als geplant werden können, verbleiben die Pods im anstehenden Status (Wartestatus), bis Sie die Größe des Worker-Pools ändern.

Wenn Sie den Cluster-Autoscaler für einen Worker-Pool aktivieren, werden Scale-up- und Scale-down-Operationen für Workerknoten als Reaktion auf Ihre Einstellungen der Podspezifikationen und auf Ressourcenanforderungen durchgeführt. Sie brauchen den Worker-Pool nicht manuell in der Größe zu ändern oder neu auszugleichen.

<br>
**Kann ich mir ein Beispiel für die Skalierung durch den Cluster-Autoscaler ansehen?**<br>
Betrachten Sie die folgende Abbildung eines Beispiels für Scale-up- und Scale-down-Operationen für den Cluster.

_Abbildung: Automatische Durchführung von Scale-ups und Scale-downs._
![Automatische Durchführung von Scale-ups und Scale-downs (GIF-Abbildung)](images/cluster-autoscaler-x3.gif){: gif}

1.  Der Cluster besitzt vier Workerknoten in zwei Worker-Pools, die auf zwei Zonen verteilt sind. Jeder Pool enthält einen Workerknoten pro Zone, jedoch hat **Worker Pool A** den Maschinentyp `u2c.2x4` und **Worker Pool B** den Maschinentyp `b2c.4x16`. Ihre gesamten Rechenressourcen betragen grob 10 Cores (2 Cores x 2 Workerknoten für **Worker Pool A** und 4 Cores x 2 Workerknoten für **Worker Pool B**). Ihr Cluster führt zurzeit eine Workload aus, die 6 dieser 10 Cores anfordert. Beachten Sie, dass weitere Rechenressourcen auf jedem Workerknoten durch die [reservierten Ressourcen](/docs/containers?topic=containers-plan_clusters#resource_limit_node) belegt werden, die zur Ausführung des Clusters, der Workerknoten und vorhandener Add-ons, wie zum Beispiel den Cluster-Autoscaler, erforderlich sind.
2.  Der Cluster-Autoscaler ist so konfiguriert, dass er beide Worker-Pools mit den folgenden Minimal- und Maximalgrößen pro Zone verwaltet:
    *  **Worker Pool A:** `minSize=1`, `maxSize=5`.
    *  **Worker Pool B:** `minSize=1`, `maxSize=2`.
3.  Sie planen Bereitstellungen, die 14 zusätzliche Podreplikate einer App erfordern, die 1 CPU-Core pro Replikat anfordert. Ein Podreplikat kann auf den aktuellen Ressourcen bereitgestellt werden, die anderen 13 sind anstehend.
4.  Der Cluster-Autoscaler führt Scale-ups Ihrer Workerknoten innerhalb dieser Beschränkungen durch, um die zusätzlichen Ressourcenanforderungen der 13 Podreplikate zu unterstützen.
    *  **Worker Pool A:** 7 Workerknoten werden im Umlaufverfahren so gleichmäßig wie möglich auf die Zonen verteilt hinzugefügt. Die Workerknoten erhöhen die Rechenkapazität des Clusters um ca. 14 Cores (2 Cores x 7 Workerknoten).
    *  **Worker Pool B:** 2 Workerknoten werden gleichmäßig auf die Zonen verteilt hinzufügt, sodass die maximale Größe (`maxSize`) von 2 Workerknoten pro Zone erreicht wird. Die Workerknoten erhöhen die Kapazität des Clusters um ca. 8 Cores (4 Cores x 2 Workerknoten).
5.  Die 20 Pods mit 1-Core-Anforderungen werden wie folgt auf die Workerknoten verteilt. Beachten Sie, dass die Pods für Ihre Workload nicht alle verfügbaren Rechenressourcen eines Workerknotens nutzen können, da Workerknoten Ressourcenreserven und Pods haben, die ausgeführt werden, um Standardclusterfunktionen abzudecken. Beispiel: Die Workerknoten vom Typ `b2c.4x16` haben zwar 4 Cores, jedoch können nur 3 Pods, die jeweils das Minimum von 1 Core anfordern, auf den Workerknoten geplant werden.
    <table summary="Eine Tabelle, die die Verteilung einer Workload im skalierten Cluster beschreibt.">
    <caption>Workloadverteilung in einem skalierten Cluster.</caption>
    <thead>
    <tr>
      <th>Worker-Pool</th>
      <th>Zone</th>
      <th>Typ</th>
      <th>Anzahl Workerknoten</th>
      <th>Anzahl Pods</th>
    </tr>
    </thead>
    <tbody>
    <tr>
      <td>A</td>
      <td>dal10</td>
      <td>u2c.2x4</td>
      <td>4 Knoten</td>
      <td>3 Pods</td>
    </tr>
    <tr>
      <td>A</td>
      <td>dal12</td>
      <td>u2c.2x4</td>
      <td>5 Knoten</td>
      <td>5 Pods</td>
    </tr>
    <tr>
      <td>B</td>
      <td>dal10</td>
      <td>b2c.4x16</td>
      <td>2 Knoten</td>
      <td>6 Pods</td>
    </tr>
    <tr>
      <td>B</td>
      <td>dal12</td>
      <td>b2c.4x16</td>
      <td>2 Knoten</td>
      <td>6 Pods</td>
    </tr>
    </tbody>
    </table>
6.  Sie benötigen die zusätzliche Workload nicht mehr, sodass Sie die Bereitstellung löschen. Nach kurzer Zeit erkennt der Cluster-Autoscaler, dass Ihr Cluster nicht mehr alle Rechenressourcen benötigt, und beginnt mit einem Scale-down der Workerknoten, indem er jeweils einen gleichzeitig entfernt.
7.  Für Ihre Worker-Pools wird ein Scale-down durchgeführt. Der Cluster-Autoscaler scannt die Worker-Pools in regelmäßigen Abständen auf anstehende Podressourcenanforderungen und nicht ausgelastete Workerknoten, um ein Scale-up oder Scale-down für die Worker-Pools durchzuführen.

## Praktiken zur skalierbaren Bereitstellung folgen
{: #scalable-practices}

Nutzen Sie den Cluster-Autoscaler optimal, indem Sie die folgenden Strategien für Ihren Workerknoten und Ihre Workload-Bereitstellungsstrategien anwenden. Weitere Informationen finden Sie in den [FAQs zu Kubernetes Cluster Autoscaler ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md).
{: shortdesc}

[Probieren Sie den Cluster-Autoscaler](#ca_helm) mit einigen Test-Workloads aus, um ein Gefühl dafür zu bekommen, [wie Scale-ups und Scale-downs funktionieren](#ca_about), welche [angepassten Werte](#ca_chart_values) vielleicht konfiguriert werden sollten und für andere Aspekte, die für Sie relevant sind, wie z. B. die [Überbereitstellung](#ca_scaleup) von Workerknoten oder das [Einschränken von Apps](#ca_limit_pool). Anschließend bereinigen Sie Ihre Testumgebung und planen, diese angepassten Werte und zusätzliche Einstellungen in eine Neuinstallation des Cluster-Autoscalers einzubeziehen.

### Kann ich mehrere Worker-Pools gleichzeitig automatisch skalieren?
{: #scalable-practices-multiple}
Ja, nachdem Sie das Helm-Diagramm installiert haben, können Sie in der [Konfigurationszuordnung](#ca_cm) auswählen, welche Worker-Pools im Cluster automatisch skaliert werden sollen. Sie können pro Cluster nur ein Helm-Diagramm `ibm-iks-cluster-autoscaler` ausführen.
{: shortdesc}

### Wie kann ich sicherstellen, dass der Cluster-Autoscaler darauf reagiert, welche Ressourcen meine App benötigt?
{: #scalable-practices-resrequests}

Der Cluster-Autoscaler skaliert Ihren Cluster in Reaktion auf die [Ressourcenanforderungen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/) Ihrer Workload. Geben Sie daher [Ressourcenanforderungen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/) für alle Bereitstellungen an, weil der Cluster-Autoscaler anhand der Ressourcenanforderungen berechnet, wie viele Workerknoten zur Ausführung der Workload benötigt werden. Beachten Sie, dass die automatische Skalierung auf der Nutzung der Rechenkapazität basiert, die Ihre Workloadkonfiguration anfordert, und keine anderen Faktoren wie zum Beispiel Maschinenkosten einkalkuliert.
{: shortdesc}

### Kann ich einen Worker-Pool auf null (0) Knoten herunterskalieren?
{: #scalable-practices-zero}

Nein, Sie können den Cluster-Autoscaler nicht auf `0` herunterskalieren (`minSize`). Zudem müssen Sie, wenn Sie die Lastausgleichsfunktionen für Anwendungen (ALBs) im Cluster nicht [inaktivieren](/docs/containers?topic=containers-cs_cli_reference#cs_alb_configure), die Einstellung für `minSize` auf `2` Workerknoten pro Zone festlegen, sodass die ALB-Pods zwecks Hochverfügbarkeit verteilt werden können.
{: shortdesc}

### Kann ich meine Bereitstellungen für die automatische Skalierung optimieren?
{: #scalable-practices-apps}

Ja, Sie können Ihrer Bereitstellung mehrere Kubernetes-Funktionen hinzufügen, um anzupassen, auf welche Weise der Cluster-Autoscaler Ihre Ressourcenanforderungen bei der Skalierung berücksichtigt.
{: shortdesc}
*   Verwenden Sie [Budgets für den Podausfall (Pod Disruption Budgets) ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/workloads/pods/disruptions/), um abruptes Neuplanen oder Löschen Ihrer Pods zu vermeiden.
*   Wenn Sie die Podpriorität nutzen, können Sie die [Prioritätsgrenze (Cutoff) bearbeiten ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#how-does-cluster-autoscaler-work-with-pod-priority-and-preemption), um die Typen von Priorität zu ändern, die ein Scale-up auslösen. Standardmäßig gilt für die Prioritätsgrenze der Wert null (`0`).

### Kann ich für automatisch skalierte Worker-Pools Taints und Tolerierungen verwenden?
{: #scalable-practices-taints}

Da Taints nicht auf Worker-Pool-Ebene angewendet werden können, wenden Sie keine [Taints auf Workerknoten an](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/), um unerwartete Ergebnisse zu vermeiden. Wenn Sie zum Beispiel eine Workload bereitstellen, die von den Workerknoten mit Taints nicht toleriert wird, werden die Workerknoten nicht für ein Scale-up in Betracht gezogen und es werden möglicherweise mehr Workerknoten bestellt, auch wenn der Cluster über genügend Kapazität verfügt. Die Workerknoten mit Taints werden jedoch weiterhin als nicht ausgelastet bewertet, wenn sie weniger als die durch den Schwellenwert (Standardwert: 50%) angegebenen Ressourcen nutzen. In diesem Fall werden sie für ein Scale-down in Betracht gezogen.
{: shortdesc}

### Warum sind meine automatisch skalierten Worker-Pools nicht ausgeglichen?
{: #scalable-practices-unbalanced}

Während eines Scale-ups gleicht der Cluster-Autoscaler Knoten mit einer zugelassenen Differenz von plus oder minus einem (+/- 1) Workerknoten über Zonen hinweg aus. Ihre anstehenden Workloads fordern möglicherweise nicht genügend Kapazität an, um alle Zonen auszugleichen. Wenn Sie in einem solchen Fall die Worker-Pools manuell ausgleichen wollen, [aktualisieren Sie die Konfigurationszuordnung (Configmap) Ihres Cluster-Autoscalers](#ca_cm), um den nicht ausgeglichenen Worker-Pool zu entfernen. Führen Sie anschließend den [Befehl](/docs/containers?topic=containers-cs_cli_reference#cs_rebalance) `ibmcloud ks worker-pool-rebalance` aus und fügen Sie den Worker-Pool der Konfigurationszuordnung des Cluster-Autoscalers wieder hinzu.
{: shortdesc}


### Warum kann ich die Größe meines Worker-Pools nicht ändern oder den Worker-Pool nicht erneut ausgleichen?
{: #scalable-practices-resize}

Wenn der Cluster-Autoscaler für Worker-Pools aktiviert ist, können Sie Ihre Worker-Pools nicht [in der Größe ändern](/docs/containers?topic=containers-cs_cli_reference#cs_worker_pool_resize) oder [neu ausgleichen](/docs/containers?topic=containers-cs_cli_reference#cs_rebalance). Sie müssen die [Konfigurationszuordnung (Configmap) bearbeiten](#ca_cm), um die Minimal- und Maximalgrößen von Worker-Pools zu ändern oder die automatische Clusterskalierung für einen bestimmten Worker-Pool zu inaktivieren. Verwenden Sie nicht den [Befehl](/docs/containers?topic=containers-cs_cli_reference#cs_worker_rm) `ibmcloud ks worker-rm`, um einzelne Workerknoten aus dem Worker-Pool zu entfernen, da dies zu einer ungleichmäßigen Workerknotenverteilung im Worker-Pool führen kann.
{: shortdesc}

Darüber hinaus ist zu beachten, dass die Größe von Worker-Pools nicht manuell geändert werden kann, wenn Sie die Worker-Pools vor dem Deinstallieren des Helm-Diagramms `ibm-iks-cluster-autoscaler` nicht inaktivieren. Installieren Sie in diesem Fall das Helm-Diagramm `ibm-iks-cluster-autoscaler` erneut, [bearbeiten Sie die Konfigurationszuordnung](#ca_cm), um den Worker-Pool zu inaktivieren, und wiederholen Sie den Versuch.

<br />


## Helm-Diagramm für den Cluster-Autoscaler im Cluster bereitstellen
{: #ca_helm}

Installieren Sie das {{site.data.keyword.containerlong_notm}}-Cluster-Autoscaler-Plug-in mit einem Helm-Diagramm, um Worker-Pools in Ihrem Cluster automatisch zu skalieren.
{: shortdesc}

**Vorbereitende Schritte**:

1.  [Installieren Sie die erforderliche CLI und die erforderlichen Plug-ins:](/docs/cli?topic=cloud-cli-ibmcloud-cli)
    *  {{site.data.keyword.Bluemix_notm}} CLI (`ibmcloud`)
    *  {{site.data.keyword.containerlong_notm}}-Plug-in (`ibmcloud ks`)
    *  {{site.data.keyword.registrylong_notm}}-Plug-in (`ibmcloud cr`)
    *  Kubernetes (`kubectl`)
    *  Helm (`helm`)
2.  [Erstellen Sie einen Standardcluster](/docs/containers?topic=containers-clusters#clusters_ui), der mit **Kubernetes Version 1.12 oder höher** ausgeführt wird.
3.   [Melden Sie sich an Ihrem Konto an. Geben Sie als Ziel die entsprechende Region und, sofern zutreffend, die Ressourcengruppe an. Legen Sie den Kontext für den Cluster fest.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
4.  Vergewissern Sie sich, dass Ihre {{site.data.keyword.Bluemix_notm}} Identity and Access Management-Berechtigungsnachweise im Cluster gespeichert sind. Der Cluster-Autoscaler verwendet diesen geheimen Schlüssel zur Authentifizierung.
    ```
    kubectl get secrets -n kube-system | grep storage-secret-store
    ```
    {: pre}
5.  Der Cluster-Autoscaler kann nur Worker-Pools skalieren, die die Bezeichnung (Label) `ibm-cloud.kubernetes.io/worker-pool-id` haben.
    1.  Prüfen Sie, ob Ihr Worker-Pool die erforderliche Bezeichnung hat.
        ```
        ibmcloud ks worker-pool-get --cluster <clustername_oder_-id> --worker-pool <worker-poolname_oder_-id> | grep Labels
        ```
        {: pre}

        Beispielausgabe für einen Worker-Pool mit der Bezeichnung:
        ```
        Labels:             ibm-cloud.kubernetes.io/worker-pool-id=a1aa111111b22b22cc3c3cc444444d44-4d555e5
        ```
        {: screen}
    2.  Falls Ihr Worker-Pool die erforderliche Bezeichnung nicht hat, [fügen Sie einen neuen Worker-Pool hinzu](/docs/containers?topic=containers-clusters#add_pool) und verwenden Sie diesen Worker-Pool mit dem Cluster-Autoscaler.


<br>
**Gehen Sie wie folgt vor, um das Plug-in `ibm-iks-cluster-autoscaler` in Ihrem Cluster zu installieren:**

1.  [Befolgen Sie die Anweisungen](/docs/containers?topic=containers-helm#public_helm_install) zum Installieren des Clients von **Helm Version 2.11 oder höher** auf Ihrer lokalen Maschine und installieren Sie den Helm-Server (tiller) mit einem Servicekonto in Ihrem Cluster.
2.  Überprüfen Sie, ob 'tiller' mit einem Servicekonto installiert ist.

    ```
    kubectl get serviceaccount -n kube-system tiller
    ```
    {: pre}

    Beispielausgabe:

    ```
    NAME                                 SECRETS   AGE
    tiller                               1         2m
    ```
    {: screen}
3.  Fügen Sie das Helm-Repository (repo) an der Position des Helm-Diagramms für den Cluster-Autoscaler hinzu und aktualisieren Sie es.
    ```
    helm repo add iks-charts https://icr.io/helm/iks-charts
    ```
    {: pre}
    ```
    helm repo update
    ```
    {: pre}
4.  Installieren Sie das Helm-Diagramm für den Cluster-Autoscaler im Namensbereich `kube-system` Ihres Clusters.

    Während der Installation haben Sie die Option, die [Einstellungen des Cluster-Autoscalers weiter anzupassen](#ca_chart_values), z. B. die Wartezeit bis zum Durchführen von Scale-ups oder Scale-downs von Workerknoten.
    {: tip}

    ```
    helm install iks-charts/ibm-iks-cluster-autoscaler --namespace kube-system --name ibm-iks-cluster-autoscaler
    ```
    {: pre}

    Beispielausgabe:
    ```
    NAME:   ibm-iks-cluster-autoscaler
    LAST DEPLOYED: Thu Nov 29 13:43:46 2018
    NAMESPACE: kube-system
    STATUS: DEPLOYED

    RESOURCES:
    ==> v1/ClusterRole
    NAME                       AGE
    ibm-iks-cluster-autoscaler  1s

    ==> v1/Pod(related)

    NAME                                        READY  STATUS             RESTARTS  AGE
    ibm-iks-cluster-autoscaler-67c8f87b96-qbb6c  0/1    ContainerCreating  0         1s

    ==> v1/ConfigMap

    NAME              AGE
    iks-ca-configmap  1s

    ==> v1/ClusterRoleBinding
    ibm-iks-cluster-autoscaler  1s

    ==> v1/Role
    ibm-iks-cluster-autoscaler  1s

    ==> v1/RoleBinding
    ibm-iks-cluster-autoscaler  1s

    ==> v1/Service
    ibm-iks-cluster-autoscaler  1s

    ==> v1beta1/Deployment
    ibm-iks-cluster-autoscaler  1s

    ==> v1/ServiceAccount
    ibm-iks-cluster-autoscaler  1s

    NOTES:
    Thank you for installing: ibm-iks-cluster-autoscaler. Your release is named: ibm-iks-cluster-autoscaler

    For more information about using the cluster autoscaler, refer to the chart README.md file.
    ```
    {: screen}

5.  Überprüfen Sie, ob die Installation erfolgreich war.

    1.  Überprüfen Sie, ob der Cluster-Autoscaler-Pod den Status **Running** (Aktiv) hat.
        ```
        kubectl get pods --namespace=kube-system | grep ibm-iks-cluster-autoscaler
        ```
        {: pre}
        Beispielausgabe:
        ```
        ibm-iks-cluster-autoscaler-8497bfc968-dbn7w   1/1       Running   0          9m
        ```
        {: screen}
    2.  Überprüfen Sie, ob der Cluster-Autoscaler-Service erstellt wurde.
        ```
        kubectl get service --namespace=kube-system | grep ibm-iks-cluster-autoscaler
        ```
        {: pre}
        Beispielausgabe:
        ```
        ibm-iks-cluster-autoscaler   ClusterIP   172.21.xxx.xx    <none>        8085/TCP        9m
        ```
        {: screen}

6.  Wiederholen Sie diese Schritte für jeden Cluster, in dem Sie den Cluster-Autoscaler bereitstellen wollen.

7.  Informationen zum Starten der Skalierung für Ihre Worker-Pools finden Sie unter [Konfiguration für den Cluster-Autoscaler aktualisieren](#ca_cm).

<br />


## Konfigurationszuordnung für den Cluster-Autoscaler aktualisieren, um die Skalierung zu aktivieren
{: #ca_cm}

Aktualisieren Sie die Konfigurationszuordnung (Configmap) für den Cluster-Autoscaler, um die automatische Skalierung von Workerknoten in Ihren Worker-Pools entsprechend den Minimal- und Maximalwerten, die Sie festlegen, zu aktivieren.
{: shortdesc}

Nach der Bearbeitung der Konfigurationszuordnung zur Aktivierung eines Worker-Pools beginnt der Cluster-Autoscaler mit der Skalierung als Reaktion auf Ihre Workloadanforderungen. Dementsprechend ist es nicht möglich, die [Größe der Worker-Pools zu ändern](/docs/containers?topic=containers-cs_cli_reference#cs_worker_pool_resize) oder die [Worker-Pools neu auszugleichen](/docs/containers?topic=containers-cs_cli_reference#cs_rebalance). Das Prüfen (Scannen) und Durchführen von Scale-up- und Scale-down-Operationen erfolgt in regelmäßigen Intervallen im Verlauf der Zeit und kann abhängig von der Anzahl der Workerknoten auch eine längere Zeit (z. B. 30 Minuten) dauern. Wenn Sie später den [Cluster-Autoscaler entfernen](#ca_rm) wollen, müssen Sie jeden Worker-Pool in der Konfigurationszuordnung zuerst inaktivieren.
{: note}

**Vorbereitende Schritte**:
*  [Installieren Sie das Plug-in `ibm-iks-cluster-autoscaler`](#ca_helm).
*  [Melden Sie sich an Ihrem Konto an. Geben Sie als Ziel die entsprechende Region und, sofern zutreffend, die Ressourcengruppe an. Legen Sie den Kontext für den Cluster fest.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

**Gehen Sie wie folgt vor, um die Konfigurationszuordnung und Werte für den Cluster-Autoscaler zu aktualisieren:**

1.  Bearbeiten Sie die YAML-Datei, die die Konfigurationszuordnung (Configmap) für den Cluster-Autoscaler enthält.
    ```
    kubectl edit cm iks-ca-configmap -n kube-system -o yaml
    ```
    {: pre}
    Beispielausgabe:
    ```
    apiVersion: v1
    data:
      workerPoolsConfig.json: |
        [
         {"name": "default","minSize": 1,"maxSize": 2,"enabled":false}
        ]
    kind: ConfigMap
    metadata:
      creationTimestamp: 2018-11-29T18:43:46Z
      name: iks-ca-configmap
      namespace: kube-system
      resourceVersion: "2227854"
      selfLink: /api/v1/namespaces/kube-system/configmaps/iks-ca-configmap
      uid: b45d047b-f406-11e8-b7f0-82ddffc6e65e
    ```
    {: screen}
2.  Bearbeiten Sie die Konfigurationszuordnung mit den Parametern, um zu definieren, wie der Cluster-Autoscaler Ihren Worker-Pool skalieren soll. Beachten Sie, dass Sie, wenn Sie die Lastausgleichsfunktionen für Anwendungen (ALBs) in Ihrem Standardcluster nicht [inaktiviert](/docs/containers?topic=containers-cs_cli_reference#cs_alb_configure) haben, den Wert des Parameters `minSize` in `2` pro Zone ändern müssen, damit die ALB-Pods für Hochverfügbarkeitszwecke verteilt werden können.

    <table>
    <caption>Parameter der Konfigurationszuordnung für den Cluster-Autoscaler</caption>
    <thead>
    <th id="parameter-with-default">Parameter mit Standardwert</th>
    <th id="parameter-with-description">Beschreibung</th>
    </thead>
    <tbody>
    <tr>
    <td id="parameter-name" headers="parameter-with-default">`"name": "default"`</td>
    <td headers="parameter-name parameter-with-description">Ersetzen Sie `"default"` durch den Namen oder die ID des Worker-Pools, den Sie skalieren wollen. Zum Auflisten der Worker-Pools führen Sie den Befehl `ibmcloud ks worker-pools --cluster <clustername_oder_-id>` aus.<br><br>
    Wenn Sie mehrere Worker-Pools verwalten möchten, kopieren Sie die JSON-Zeile wie folgt in durch Kommas getrennte Zeilen. <pre class="codeblock">[
     {"name": "default","minSize": 1,"maxSize": 2,"enabled":false},
     {"name": "Pool2","minSize": 2,"maxSize": 5,"enabled":true}
    ]</pre><br><br>
    **Hinweis:** Der Cluster-Autoscaler kann nur Worker-Pools skalieren, die die Bezeichnung (Label) `ibm-cloud.kubernetes.io/worker-pool-id` haben. Zum Prüfen, ob Ihr Worker-Pool die erforderliche Bezeichnung hat, führen Sie den folgenden Befehl aus: `ibmcloud ks worker-pool-get --cluster <clustername_oder_-id> --worker-pool <worker-pool-name_oder_-id> | grep Labels`. Falls Ihr Worker-Pool die erforderliche Bezeichnung nicht hat, [fügen Sie einen neuen Worker-Pool hinzu](/docs/containers?topic=containers-clusters#add_pool) und verwenden Sie diesen Worker-Pool mit dem Cluster-Autoscaler.</td>
    </tr>
    <tr>
    <td id="parameter-minsize" headers="parameter-with-default">`"minSize": 1`</td>
    <td headers="parameter-minsize parameter-with-description">Geben Sie die minimale Anzahl von Workerknoten pro Zone an, die zu jeder Zeit im Worker-Pool vorhanden sein soll. Der Wert muss 2 oder größer sein, damit Ihre ALB-Pods für hohe Verfügbarkeit verteilt werden können. Wenn Sie die ALB in Ihrem Standardcluster [inaktiviert](/docs/containers?topic=containers-cs_cli_reference#cs_alb_configure) haben, können Sie den Parameter auf den Wert `1` setzen.</td>
    </tr>
    <tr>
    <td id="parameter-maxsize" headers="parameter-with-default">`"maxSize": 2`</td>
    <td headers="parameter-maxsize parameter-with-description">Geben Sie die maximale Anzahl von Workerknoten pro Zone an, die im Worker-Pool vorhanden sein soll. Der Wert muss größer oder gleich dem Wert sein, den Sie für den Parameter `minSize` festlegen.</td>
    </tr>
    <tr>
    <td id="parameter-enabled" headers="parameter-with-default">`"enabled": false`</td>
    <td headers="parameter-enabled parameter-with-description">Setzen Sie den Parameter auf den Wert `true`, damit der Cluster-Autoscaler die Skalierung für den Worker-Pool verwaltet. Setzen Sie den Parameter auf den Wert `false`, um die Skalierung des Worker-Pools durch den Cluster-Autoscaler zu inaktivieren.<br><br>
    Wenn Sie später den [Cluster-Autoscaler entfernen](#ca_rm) wollen, müssen Sie jeden Worker-Pool in der Konfigurationszuordnung zuerst inaktivieren.</td>
    </tr>
    </tbody>
    </table>
3.  Speichern Sie die Konfigurationsdatei.
4.  Rufen Sie Ihren Cluster-Autoscaler-Pod ab.
    ```
    kubectl get pods -n kube-system
    ```
    {: pre}
5.  Prüfen Sie den Abschnitt **`Events`** des Cluster-Autoscaler-Pods auf ein Ereignis **`ConfigUpdated`**, um sich zu vergewissern, dass die Konfigurationszuordnung erfolgreich aktualisiert wurde. Die Ereignisnachricht für Ihre Konfigurationszuordnung hat das folgende Format: `minSize:maxSize:PoolName:<SUCCESS|FAILED>:error message`.

    ```
    kubectl describe pod -n kube-system <cluster-autoscaler-pod>
    ```
    {: pre}

    Beispielausgabe:
    ```
		Name:               ibm-iks-cluster-autoscaler-857c4d9d54-gwvc6
		Namespace:          kube-system
		...
		Events:
		Type     Reason         Age   From                                        Message
		----     ------         ----  ----                                        -------

		Normal  ConfigUpdated  3m    ibm-iks-cluster-autoscaler-857c4d9d54-gwvc6  {"1:3:default":"SUCCESS:"}
    ```
    {: screen}

## Konfigurationswerte für das Helm-Diagramm des Cluster-Autoscalers anpassen
{: #ca_chart_values}

Sie können die Einstellungen für den Cluster-Autoscaler anpassen, wie zum Beispiel die Zeit, die der Cluster-Autoscaler wartet, bevor er ein Scale-up oder Scale-down von Workerknoten durchführt.
{: shortdesc}

**Vorbereitende Schritte**:
*  [Melden Sie sich an Ihrem Konto an. Geben Sie als Ziel die entsprechende Region und, sofern zutreffend, die Ressourcengruppe an. Legen Sie den Kontext für den Cluster fest.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
*  [Installieren Sie das Plug-in `ibm-iks-cluster-autoscaler`](#ca_helm).

**Gehen Sie wie folgt vor, um die Werte für den Cluster-Autoscaler zu aktualisieren:**

1.  Prüfen Sie die Konfigurationswerte für das Helm-Diagramm des Cluster-Autoscalers. Der Cluster-Autoscaler wird mit Standardeinstellungen bereitgestellt. Sie können jedoch einige Werte, wie zum Beispiel die Intervalle für Scale-down oder das Scannen, in Abhängigkeit davon ändern, wie häufig Sie Ihre Cluster-Workloads ändern.
    ```
    helm get values ibm-iks-cluster-autoscaler -a
    ```
    {: pre}

    Beispielausgabe:
    ```
    expander: least-waste
    image:
      pullPolicy: Always
      repository: registry.ng.bluemix.net/armada-master/ibmcloud-cluster-autoscaler
      tag: dev1
    maxNodeProvisionTime: 120m
    resources:
      limits:
        cpu: 300m
        memory: 300Mi
      requests:
        cpu: 100m
        memory: 100Mi
    scaleDownDelayAfterAdd: 10m
    scaleDownDelayAfterDelete: 10m
    scaleDownUtilizationThreshold: 0.5
    scaleDownUnneededTime: 10m
    scanInterval: 1m
    skipNodes:
      withLocalStorage: true
      withSystemPods: true
    ```
    {: codeblock}

    <table>
    <caption>Konfigurationswerte für den Cluster-Autoscaler</caption>
    <thead>
    <th>Parameter</th>
    <th>Beschreibung</th>
    <th>Standardwert</th>
    </thead>
    <tbody>
    <tr>
    <td>Parameter `api_route`</td>
    <td>Legen Sie den [{{site.data.keyword.containerlong_notm}}-API-Endpunkt](/docs/containers?topic=containers-cs_cli_reference#cs_cli_api) für die Region fest, in der sich Ihr Cluster befindet.</td>
    <td>Kein Standardwert; verwendet die Zielregion, in der sich Ihr Cluster befindet.</td>
    </tr>
    <tr>
    <td>Parameter `expander`</td>
    <td>Geben Sie an, wie der Cluster-Autoscaler den zu skalierenden Worker-Pool ermittelt, wenn Sie mehrere Worker-Pools haben. Mögliche Werte:
    <ul><li>`random`: Wählt zufällig zwischen den Methoden `most-pods` und `least-waste` aus.</li>
    <li>`most-pods`: Wählt den Worker-Pool aus, der bei einem Scale-up die meisten Pods planen kann. Verwenden Sie diese Methode, wenn Sie `nodeSelector` verwenden, um sicherzustellen, dass Pods auf bestimmten Workerknoten landen.</li>
    <li>`least-waste`: Wählt den Worker-Pool aus, der die geringste ungenutzte CPU-Kapazität aufweist, oder, bei Worker-Pools mit gleichen Werten, den Worker-Pool, der nach dem Scale-up die geringste ungenutzte Speicherkapazität aufweist. Verwenden Sie diese Methode, wenn Sie über mehrere Worker-Pools mit Maschinentypen großer CPU- und Speicherkapatzitäten verfügen und diese größeren Maschinen nur verwenden wollen, wenn anstehende Pods große Kapazitäten an Ressourcen benötigen.</li></ul></td>
    <td>random</td>
    </tr>
    <tr>
    <td>Parameter `image.repository`</td>
    <td>Geben Sie das zu verwendende Docker-Image für den Cluster-Autoscaler an.</td>
    <td>`registry.bluemix.net/ibm/ibmcloud-cluster-autoscaler`</td>
    </tr>
    <tr>
    <td>Parameter `image.pullPolicy`</td>
    <td>Geben Sie an, wann das Docker-Image zu extrahieren ist. Mögliche Werte:
    <ul><li>`Always`: Extrahiert das Image bei jedem Start des Pods.</li>
    <li>`IfNotPresent`: Extrahiert das Image nur, wenn es nicht bereits lokal vorhanden ist.</li>
    <li>`Never`: Geht davon aus, dass das Image lokal vorhanden ist und extrahiert es nie.</li></ul></td>
    <td>Always</td>
    </tr>
    <tr>
    <td>Parameter `maxNodeProvisionTime`</td>
    <td>Legt die Höchstdauer in Minuten fest, die ein Workerknoten aufwenden kann, um mit dem Bereitstellen zu beginnen, bevor der Cluster-Autoscaler die Scale-up-Anforderung abbricht.</td>
    <td>120m</td>
    </tr>
    <tr>
    <td>Parameter `resources.limits.cpu`</td>
    <td>Legt die maximale CPU-Kapazität für Workerknoten fest, die der Pod `ibm-iks-cluster-autoscaler` nutzen kann.</td>
    <td>300m</td>
    </tr>
    <tr>
    <td>Parameter `resources.limits.memory`</td>
    <td>Legt die maximale Speicherkapazität für Workerknoten fest, die der Pod `ibm-iks-cluster-autoscaler` nutzen kann.</td>
    <td>300Mi</td>
    </tr>
    <tr>
    <td>Parameter `resources.requests.cpu`</td>
    <td>Legt die minimale CPU-Kapazität für Workerknoten fest, mit der der Pod `ibm-iks-cluster-autoscaler` startet.</td>
    <td>100m</td>
    </tr>
    <tr>
    <td>Parameter `resources.requests.memory`</td>
    <td>Legt die maximale Speicherkapazität für Workerknoten fest, mit der der Pod `ibm-iks-cluster-autoscaler` startet.</td>
    <td>100Mi</td>
    </tr>
    <tr>
    <td>Parameter `scaleDownUnneededTime`</td>
    <td>Legt die Zeitdauer in Minuten fest, die ein Workerknoten nicht benötigt worden sein muss, bevor er durch ein Scale-down entfernt werden kann.</td>
    <td>10m</td>
    </tr>
    <tr>
    <td>Parameter `scaleDownDelayAfterAdd`, `scaleDownDelayAfterDelete`</td>
    <td>Legt die Zeitdauer in Minuten fest, die der Cluster-Autoscaler nach einem Scale-up (`add`) oder Scale-down (`delete`) wartet, bevor er die Skalierungsaktionen erneut startet.</td>
    <td>10m</td>
    </tr>
    <tr>
    <td>Parameter `scaleDownUtilizationThreshold`</td>
    <td>Legt den Auslastungsschwellenwert für Workerknoten fest. Wenn die Auslastung eines Workerknotens unter den Schwellenwert fällt, kommt der Workerknoten für ein Scale-down in Betracht. Die Workerknotenauslastung wird als Quotient aus der Summe der CPU- und Speicherressourcen, die von allen Pods angefordert werden, die auf dem Workerknoten ausgeführt werden, dividiert durch die Ressourcenkapazität des Workerknotens berechnet.</td>
    <td>0.5</td>
    </tr>
    <tr>
    <td>Parameter `scanInterval`</td>
    <td>Legt die Häufigkeit in Minuten fest, mit der der Cluster-Autoscaler auf Workloadauslastung prüft (scannt), die ein Scale-up oder Scale-down auslöst.</td>
    <td>1m</td>
    </tr>
    <tr>
    <td>Parameter `skipNodes.withLocalStorage`</td>
    <td>Bei `true` werden Workerknoten, die Pods haben, die Daten im lokalen Speicher speichern, nicht durch ein Scale-down entfernt.</td>
    <td>true</td>
    </tr>
    <tr>
    <td>Parameter `skipNodes.withSystemPods`</td>
    <td>Bei `true` werden Workerknoten, die Pods in `kube-system` haben, nicht durch ein Scale-down entfernt. Setzen Sie den Wert nicht auf `false`, da ein Scale-down von Pods in `kube-system` zu unerwarteten Ergebnissen führen kann.</td>
    <td>true</td>
    </tr>
    </tbody>
    </table>
2.  Zum Ändern von Cluster-Autoscaler-Konfigurationswerten aktualisieren Sie das Helm-Diagramm mit den neuen Werten. Schließen Sie das Flag `--recreate-pods` ein, sodass alle vorhandenen Cluster-Autosscaler-Pods neu erstellt werden und die Änderungen an der angepassten Einstellung übernehmen.
    ```
    helm upgrade --set scanInterval=2m ibm-iks-cluster-autoscaler iks-charts/ibm-iks-cluster-autoscaler -i --recreate-pods
    ```
    {: pre}

    Führen Sie zum Zurücksetzen des Diagramms auf die Standardwerte den folgenden Befehl aus:
    ```
    helm upgrade --reset-values ibm-iks-cluster-autoscaler iks-charts/ibm-iks-cluster-autoscaler --recreate-pods
    ```
    {: pre}
3.  Rufen Sie zum Überprüfen Ihrer Änderungen die Werte des Helm-Diagramms erneut ab.
    ```
    helm get values ibm-iks-cluster-autoscaler -a
    ```
    {: pre}
    

## Apps auf die Ausführung nur auf bestimmten automatisch skalierten Worker-Pools beschränken
{: #ca_limit_pool}

Zur Beschränkung einer Podbereitstellung auf einen bestimmten Worker-Pool, der durch den Cluster-Autoscaler verwaltet wird, verwenden Sie Bezeichnungen (Labels) und die Angabe `nodeSelector`.
{: shortdesc}

**Vorbereitende Schritte**:
*  [Installieren Sie das Plug-in `ibm-iks-cluster-autoscaler`](#ca_helm).
*  [Melden Sie sich an Ihrem Konto an. Geben Sie als Ziel die entsprechende Region und, sofern zutreffend, die Ressourcengruppe an. Legen Sie den Kontext für den Cluster fest.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

**Gehen Sie wie folgt vor, um Pods auf das Ausführen bestimmter automatisch skalierter Worker-Pools zu beschränken:**

1.  Erstellen Sie den Worker-Pool mit der Bezeichnung, die Sie verwenden wollen.
    ```
    ibmcloud ks worker-pool-create --name <name> --cluster <clustername_oder_-id> --machine-type <maschinentyp> --size-per-zone <anzahl_workerknoten> --labels <schlüssel>=<wert>
    ```
    {: pre}
2.  [Add the worker pool to the cluster autoscaler configuration](#ca_cm).
3.  Stimmen Sie in der Podspezifikationsvorlage (spec template) den Wert von `nodeSelector` auf die Bezeichnung ab, die Sie in Ihrem Worker-Pool verwendet haben.
    ```
    ...
    spec:
      containers:
      - name: nginx
        image: nginx
        imagePullPolicy: IfNotPresent
      nodeSelector:
        app: nginx
    ...
    ```
    {: codeblock}
4.  Stellen Sie den Pod bereit. Durch die übereinstimmende Bezeichnung wird der Pod auf einem Workerknoten geplant, der sich in dem Worker-Pool mit der Bezeichnung befindet.
    ```
    kubectl apply -f pod.yaml
    ```
    {: pre}

<br />


## Scale-up von Workerknoten durchführen, bevor der Worker-Pool unzureichende Ressourcen hat
{: #ca_scaleup}

Wie im Abschnitt [Funktionsweise des Cluster-Autoscalers](#ca_about) und in den [FAQs zu Kubernetes Cluster Autoscaler ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md) erläutert, führt der Cluster-Autoscaler ein Scale-up Ihrer Worker-Pools als Reaktion auf die durch Ihre Workload angeforderten Ressourcen aus den verfügbaren Ressourcen des Worker-Pools durch. Es kann jedoch wünschenswert sein, dass der Cluster-Autoscaler Workerknoten durch ein Scale-up hinzufügt, bevor der Worker-Pool über zu wenig Ressourcen verfügt. In diesem Fall muss Ihre Workload nicht so lange auf die Bereitstellung von Workerknoten warten, weil der Worker-Pool bereits durch ein Scale-up vergrößert wurde, um die Ressourcenanforderungen zu erfüllen.
{: shortdesc}

Der Cluster-Autoscaler unterstützt eine frühzeitige Skalierung (Überbereitstellung) von Worker-Pools nicht. Sie haben jedoch die Möglichkeit, andere  Kubernetes-Ressourcen mit Verwaltung durch den Cluster-Autoscaler zu konfigurieren, um eine frühzeitige Skalierung zu erreichen.

<dl>
  <dt><strong>Pausenpods</strong></dt>
  <dd>Sie können eine Bereitstellung erstellen, durch die [Pausencontainer ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://stackoverflow.com/questions/48651269/what-are-the-pause-containers) in Pods mit bestimmten Ressourcenanforderungen bereitgestellt werden, und der Bereitstellung eine niedrige Podpriorität zuweisen. Wenn diese Ressourcen von Workloads höherer Priorität benötigt werden, wird der Pausenpod präemptiv vom Workerknoten entfernt und wird zu einem anstehenden Pod. Dieses Ereignis löst ein Scale-up durch den Cluster-Autoscaler aus.<br><br>Weitere Informationen zur Einrichtung einer Pausenpodbereitstellung finden Sie unter [Kubernetes FAQ ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#how-can-i-configure-overprovisioning-with-cluster-autoscaler). Sie können dieses [Beispiel für eine Konfigurationsdatei zur Überbereitstellung ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/IBM-Cloud/kube-samples/blob/master/ibm-ks-cluster-autoscaler/overprovisioning-autoscaler.yaml) verwenden, um die Prioritätsklasse, das Servicekonto und Bereitstellungen zu erstellen.<p class="note">Wenn Sie diese Methode verwenden, stellen Sie sicher, dass Sie mit der Funktionsweise der [Podpriorität](/docs/containers?topic=containers-pod_priority#pod_priority) und der Vorgehensweise zum Festlegen der Podpriorität für Ihre Bereitstellungen vertraut sind. Wenn der Pausenpod zum Beispiel nicht genügend Ressourcen für einen Pod höherer Priorität zur Verfügung hat, wird der Pod nicht präemptiv entfernt. Die Workload mit der höheren Priorität verbleibt im anstehenden Status, sodass ein Scale-up durch den Cluster-Autoscaler ausgelöst wird. Allerdings erfolgt in diesem Fall die automatische Skalierung nicht frühzeitig, da die auszuführende Workload aufgrund unzureichender Ressourcen nicht ausgeführt werden kann.</p></dd>

  <dt><strong>Horizontale automatische Podskalierung (HPA - Horizontal Pod Autoscaling)</strong></dt>
  <dd>Da die horizontale automatische Podskalierung auf der durchschnittlichen CPU-Auslastung der Pods basiert, wird die Begrenzung der CPU-Auslastung, die Sie festlegen, erreicht, bevor der Worker-Pool tatsächlich zu wenig Ressourcen hat. Es werden mehr Pods angefordert, was wiederum ein Scale-up des Worker-Pools durch den Cluster-Autoscaler auslöst.<br><br>Weitere Informationen zur Einrichtung der horizontalen automatischen Podskalierung (HPA) finden Sie in der [Kubernetes-Dokumentation ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale-walkthrough/).</dd>
</dl>

<br />


## Helm-Diagramm für den Cluster-Autoscaler aktualisieren
{: #ca_helm_up}

Sie können das vorhandene Helm-Diagramm für den Cluster-Autoscaler auf die neueste Version aktualisieren. Zum Prüfen Ihrer aktuellen Helm-Diagrammversion führen Sie den folgenden Befehl aus: `helm ls | grep cluster-autoscaler`.
{: shortdesc}

Aktualisierung auf das neueste Helm-Diagramm von Version 1.0.2 oder früher? [Folgen Sie diesen Anweisungen](#ca_helm_up_102).
{: note}

Vorbereitende Schritte: [Melden Sie sich an Ihrem Konto an. Geben Sie als Ziel die entsprechende Region und, sofern zutreffend, die Ressourcengruppe an. Legen Sie den Kontext für den Cluster fest.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  Aktualisieren Sie das Helm-Repository, um die aktuelle Version aller Helm-Diagramme in diesem Repository abzurufen.
    ```
    helm repo update
    ```
    {: pre}

2.  Optional: Laden Sie das aktuellste Helm-Diagramm auf Ihre lokale Maschine herunter. Extrahieren Sie anschließend das Paket und überprüfen Sie die Datei `release.md` auf die neuesten Releaseinformationen.
    ```
    helm fetch iks-charts/ibm-iks-cluster-autoscaler
    ```
    {: pre}

3.  Ermitteln Sie den Namen des Helm-Diagramms für den Cluster-Autoscaler, das Sie in Ihrem Cluster installiert haben.
    ```
    helm ls | grep cluster-autoscaler
    ```
    {: pre}

    Beispielausgabe:
    ```
    myhelmchart 	1       	Mon Jan  7 14:47:44 2019	DEPLOYED	ibm-ks-cluster-autoscaler-1.0.0  	kube-system
    ```
    {: screen}

4.  Aktualisieren Sie das Helm-Diagramm für den Cluster-Autoscaler auf die neueste Version.
    ```
    helm upgrade --force --recreate-pods <helm_chart_name>  iks-charts/ibm-iks-cluster-autoscaler
    ```
    {: pre}

5.  Stellen Sie sicher, dass in der [Konfigurationszuordnung (Configmap) des Cluster-Autoscalers](#ca_cm) im Abschnitt `workerPoolsConfig.json` der Parameter `"enabled": true` für die Worker-Pools festgelegt ist, die Sie skalieren wollen.
    ```
    kubectl describe cm iks-ca-configmap -n kube-system
    ```
    {: pre}

    Beispielausgabe:
    ```
    Name:         iks-ca-configmap
    Namespace:    kube-system
    Labels:       <none>
    Annotations:  kubectl.kubernetes.io/last-applied-configuration={"apiVersion":"v1","data":{"workerPoolsConfig.json":"[\n {\"name\": \"docs\",\"minSize\": 1,\"maxSize\": 3,\"enabled\":true}\n]\n"},"kind":"ConfigMap",...

    Data
    ====
    workerPoolsConfig.json:
    ----
    [
     {"name": "docs","minSize": 1,"maxSize": 3,"enabled":true}
    ]

    Events:  <none>
    ```
    {: screen}

### Aktualisierung auf das neueste Helm-Diagramm von Version 1.0.2 oder früher durchführen
{: #ca_helm_up_102}

Die neueste Helm-Diagrammversion für den Cluster-Autoscaler erfordert ein vollständiges Entfernen der zuvor installierten Versionen des Helm-Diagramms für den Cluster-Autoscaler. Wenn Sie das Helm-Diagramm der Version 1.0.2 oder einer früheren Version installiert haben, deinstallieren Sie diese Version zuerst, bevor Sie das neueste Helm-Diagramm für den Cluster-Autoscaler installieren.
{: shortdesc}

Vorbereitende Schritte: [Melden Sie sich an Ihrem Konto an. Geben Sie als Ziel die entsprechende Region und, sofern zutreffend, die Ressourcengruppe an. Legen Sie den Kontext für den Cluster fest.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  Rufen Sie die Konfigurationszuordnung Ihres Cluster-Autoscalers ab.
    ```
    kubectl get cm iks-ca-configmap -n kube-system -o yaml > iks-ca-configmap.yaml
    ```
    {: pre}
2.  Entfernen Sie alle Worker-Pools aus der Konfigurationszuordnung, indem Sie den Wert für `"enabled"` auf `false` setzen.
    ```
    kubectl edit cm iks-ca-configmap -n kube-system
    ```
    {: pre}
3.  Wenn sie angepasste Einstellungen auf das Helm-Diagramm angewendet haben, notieren Sie die angepassten Einstellungen.
    ```
    helm get values ibm-ks-cluster-autoscaler -a
    ```
    {: pre}
4.  Deinstallieren Sie das aktuelle Helm-Diagramm.
    ```
    helm delete --purge ibm-ks-cluster-autoscaler
    ```
    {: pre}
5.  Aktualisieren Sie das Helm-Diagramm-Repository, um die neueste Helm-Diagrammversion für den Cluster-Autoscaler abzurufen.
    ```
    helm repo update
    ```
    {: pre}
6.  Installieren Sie das neueste Helm-Diagramm für den Cluster-Autoscaler. Wenden Sie alle angepassten Einstellungen, die Sie zuvor verwendet haben, mit dem Flag `--set` an (Beispiel: `scanInterval=2m`).
    ```
    helm install  iks-charts/ibm-iks-cluster-autoscaler --namespace kube-system --name ibm-iks-cluster-autoscaler [--set <custom_settings>]
    ```
    {: pre}
7.  Wenden Sie die Konfigurationszuordnung des Cluster-Autoscalers an, die Sie zuvor abgerufen haben, um die automatische Skalierung für Ihre Worker-Pools zu aktivieren.
    ```
    kubectl apply -f iks-ca-configmap.yaml
    ```
    {: pre}
8.  Rufen Sie Ihren Cluster-Autoscaler-Pod ab.
    ```
    kubectl get pods -n kube-system
    ```
    {: pre}
9.  Prüfen Sie den Abschnitt **`Events`** des Cluster-Autoscaler-Pods auf ein Ereignis **`ConfigUpdated`**, um sich zu vergewissern, dass die Konfigurationszuordnung erfolgreich aktualisiert wurde. Die Ereignisnachricht für Ihre Konfigurationszuordnung hat das folgende Format: `minSize:maxSize:PoolName:<SUCCESS|FAILED>:error message`.
    ```
    kubectl describe pod -n kube-system <cluster-autoscaler-pod>
    ```
    {: pre}

    Beispielausgabe:
    ```
		Name:               ibm-iks-cluster-autoscaler-857c4d9d54-gwvc6
		Namespace:          kube-system
		...
		Events:
		Type     Reason         Age   From                                        Message
		----     ------         ----  ----                                        -------

		Normal  ConfigUpdated  3m    ibm-iks-cluster-autoscaler-857c4d9d54-gwvc6  {"1:3:default":"SUCCESS:"}
    ```
    {: screen}

<br />


## Cluster-Autoscaler entfernen
{: #ca_rm}

Wenn Ihre Worker-Pools nicht automatisch skaliert werden sollen, können Sie das Helm-Diagramm für den Cluster-Autoscaler deinstallieren. Nach dem Entfernen müssen Sie die Worker-Pools manuell [in der Größe ändern](/docs/containers?topic=containers-cs_cli_reference#cs_worker_pool_resize) oder [neu ausgleichen](/docs/containers?topic=containers-cs_cli_reference#cs_rebalance).
{: shortdesc}

Vorbereitende Schritte: [Melden Sie sich an Ihrem Konto an. Geben Sie als Ziel die entsprechende Region und, sofern zutreffend, die Ressourcengruppe an. Legen Sie den Kontext für den Cluster fest.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  Entfernen Sie in der [Konfigurationszuordnung des Cluster-Autoscalers](#ca_cm) den Worker-Pool, indem Sie `"enabled"` auf den Wert `false` setzen.
    ```
    kubectl edit cm iks-ca-configmap -n kube-system
    ```
    {: pre}
    Beispielausgabe:
    ```
    apiVersion: v1
    data:
      workerPoolsConfig.json: |
        [
         {"name": "default","minSize": 1,"maxSize": 2,"enabled":false},
         {"name":"mypool","minSize":1,"maxSize":5,"enabled":false}
        ]
    kind: ConfigMap
    ...
    ```
2.  Listen Sie Ihre vorhandenen Helm-Diagramme auf und notieren Sie den Namen des Cluster-Autoscalers.
    ```
    helm ls
    ```
    {: pre}
3.  Entfernen Sie das vorhandene Helm-Diagramm aus Ihrem Cluster.
    ```
    helm delete --purge <ibm-iks-cluster-autoscaler-name>
    ```
    {: pre}
