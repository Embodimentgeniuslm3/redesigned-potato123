---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-04"

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


# {{site.data.keyword.cloudaccesstrailshort}}-Ereignisse
{: #at_events}

Mithilfe des {{site.data.keyword.cloudaccesstrailshort}}-Service können Sie vom Benutzer initiierte Aktivitäten in Ihrem {{site.data.keyword.containerlong_notm}}-Cluster anzeigen, verwalten und prüfen.
{: shortdesc}

Von {{site.data.keyword.containershort_notm}} werden zwei Typen von {{site.data.keyword.cloudaccesstrailshort}}-Ereignissen generiert:

* **Ereignisse der Clusterverwaltung**:
    * Diese Ereignisse werden automatisch generiert und an {{site.data.keyword.cloudaccesstrailshort}} weitergeleitet.
    * Sie können diese Ereignisse in der {{site.data.keyword.cloudaccesstrailshort}}-**Kontodomäne** anzeigen.

* **Ereignisse des Kubernetes-API-Serveraudits**:
    * Diese Ereignisse werden automatisch generiert, Sie müssen den Cluster jedoch so konfigurieren, dass diese Ereignisse an den {{site.data.keyword.cloudaccesstrailshort}}-Service weitergeleitet werden.
    * Sie können den Cluster so konfigurieren, dass Ereignisse an die {{site.data.keyword.cloudaccesstrailshort}}-**Kontodomäne** oder an eine **Bereichsdomäne** gesendet werden. Weitere Informationen finden Sie unter [Auditprotokoll senden](/docs/containers?topic=containers-health#api_forward).

Weitere Informationen zur Arbeitsweise des Service finden Sie in der [{{site.data.keyword.cloudaccesstrailshort}}-Dokumentation](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started-with-cla). Weitere Informationen zu Kubernetes-Aktionen, die aufgezeichnet werden, finden Sie in der [Kubernetes-Dokumentation ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/home/).

## Informationen zu Ereignissen suchen
{: #kube-find}

Sie können die Aktivitäten in Ihrem Cluster überwachen, indem Sie sich die Protokolle im Kibana-Dashboard ansehen.
{: shortdesc}

So überwachen Sie die Verwaltungsaktivität:

1. Melden Sie sich bei Ihrem {{site.data.keyword.Bluemix_notm}}-Konto an.
2. Stellen Sie über den Katalog eine Instanz des {{site.data.keyword.cloudaccesstrailshort}}-Service im selben Konto wie Ihre Instanz von {{site.data.keyword.containerlong_notm}} bereit.
3. Wählen Sie in der Registerkarte **Verwalten** des {{site.data.keyword.cloudaccesstrailshort}}-Dashboards die Konto- oder Bereichsdomäne aus.
  * **Kontoprotokolle:** Ereignisse der Clusterverwaltung und Ereignisse von Kubernetes-API-Serveraudits sind in der **Kontodomäne** für die {{site.data.keyword.Bluemix_notm}}-Region verfügbar, in der die Ereignisse generiert werden.
  * **Bereichsprotokolle:** Falls Sie einen Bereich angegeben haben, als Sie für den Cluster konfiguriert haben, dass Ereignisse von Kubernetes-API-Serveraudits weitergeleitet werden sollen, sind diese Ereignisse in der **Bereichsdomäne** verfügbar, die dem Cloud Foundry-Bereich zugeordnet ist, in dem der {{site.data.keyword.cloudaccesstrailshort}}-Service bereitgestellt wird.
4. Klicken Sie auf **In Kibana anzeigen**.
5. Legen Sie den Zeitrahmen fest, für den Sie Protokolle anzeigen möchten. Der Standardwert ist 24 Stunden.
6. Zum Eingrenzen der Suche können Sie auf das Bearbeitungssymbol für `ActivityTracker_Account_Search_in_24h` klicken und Felder in der Spalte **Verfügbare Felder** hinzufügen.

Die Informationen im Abschnitt [Berechtigungen zur Anzeige von Kontoereignissen erteilen](/docs/services/cloud-activity-tracker/how-to?topic=cloud-activity-tracker-grant_permissions#grant_permissions) helfen Ihnen dabei, wenn Sie andere Benutzer berechtigen wollen, Konto- und Bereichsereignisse anzuzeigen.
{: tip}

## Ereignisse der Clusterverwaltung aufzeichnen
{: #cluster-events}

In der folgenden Tabelle finden Sie eine Liste der Ereignisse der Clusterverwaltung, die an {{site.data.keyword.cloudaccesstrailshort}} gesendet werden.
{: shortdesc}

<table>
<tr>
<th>Aktion</th>
<th>Beschreibung</th></tr><tr>
<td><code>containers-kubernetes.account-credentials.set</code></td>
<td>Die Infrastrukturberechtigungsnachweise in einer Region für eine Ressourcengruppe werden festgelegt.</td></tr><tr>
<td><code>containers-kubernetes.account-credentials.unset</code></td>
<td>Die Festlegung der Infrastrukturberechtigungsnachweise in einer Region für eine Ressourcengruppe wird rückgängig gemacht.</td></tr><tr>
<td><code>containers-kubernetes.alb.create</code></td>
<td>Eine Ingress-ALB wird erstellt.</td></tr><tr>
<td><code>containers-kubernetes.alb.delete</code></td>
<td>Eine Ingress-ALB wird gelöscht.</td></tr><tr>
<td><code>containers-kubernetes.alb.get</code></td>
<td>Informationen zu einer Ingress-ALB werden angezeigt.</td></tr><tr>
<td><code>containers-kubernetes.apikey.reset</code></td>
<td>Ein API-Schlüssel für eine Region und eine Ressourcengruppe wird zurückgesetzt.</td></tr><tr>
<td><code>containers-kubernetes.cluster.create</code></td>
<td>Ein Cluster wird erstellt.</td></tr><tr>
<td><code>containers-kubernetes.cluster.delete</code></td>
<td>Ein Cluster wird gelöscht.</td></tr><tr>
<td><code>containers-kubernetes.cluster-feature.enable</code></td>
<td>Ein Feature, wie z. B. 'Trusted Compute' für Bare-Metal-Workerknoten, wird in einem Cluster aktiviert.</td></tr><tr>
<td><code>containers-kubernetes.cluster.get</code></td>
<td>Informationen zu einem Cluster werden angezeigt.</td></tr><tr>
<td><code>containers-kubernetes.logging-config.create</code></td>
<td>Eine Protokollweiterleitungskonfiguration wird erstellt.</td></tr><tr>
<td><code>containers-kubernetes.logging-config.delete</code></td>
<td>Eine Protokollweiterleitungskonfiguration wird gelöscht.</td></tr><tr>
<td><code>containers-kubernetes.logging-config.get</code></td>
<td>Informationen zu einer Protokollweiterleitungskonfiguration werden angezeigt.</td></tr><tr>
<td><code>containers-kubernetes.logging-config.update</code></td>
<td>Für eine Protokollweiterleitungskonfiguration wird ein Update ausgeführt.</td></tr><tr>
<td><code>containers-kubernetes.logging-config.refresh</code></td>
<td>Eine Protokollweiterleitungskonfiguration wird aktualisiert.</td></tr><tr>
<td><code>containers-kubernetes.logging-filter.create</code></td>
<td>Ein Protokollierungsfilter wird erstellt.</td></tr><tr>
<td><code>containers-kubernetes.logging-filter.delete</code></td>
<td>Ein Protokollierungsfilter wird gelöscht.</td></tr><tr>
<td><code>containers-kubernetes.logging-filter.get</code></td>
<td>Informationen zu einem Protokollierungsfilter werden angezeigt.</td></tr><tr>
<td><code>containers-kubernetes.logging-filter.update</code></td>
<td>Ein Protokollierungsfilter wird aktualisiert.</td></tr><tr>
<td><code>containers-kubernetes.logging-autoupdate.changed</code></td>
<td>Die automatische Aktualisierungskomponente für das Protokollierungs-Add-on wird aktiviert oder inaktiviert.</td></tr><tr>
<td><code>containers-kubernetes.mzlb.create</code></td>
<td>Eine Lastausgleichsfunktion für mehrere Zonen wird erstellt.</td></tr><tr>
<td><code>containers-kubernetes.mzlb.delete</code></td>
<td>Eine Lastausgleichsfunktion für mehrere Zonen wird gelöscht.</td></tr><tr>
<td><code>containers-kubernetes.service.bind</code></td>
<td>Ein Service wird an einen Cluster gebunden.</td></tr><tr>
<td><code>containers-kubernetes.service.unbind</code></td>
<td>Die Bindung eines Service an einen Cluster wird aufgehoben.</td></tr><tr>
<td><code>containers-kubernetes.subnet.add</code></td>
<td>Ein vorhandenes Teilnetz der IBM Cloud-Infrastruktur (SoftLayer) wird zum Cluster hinzugefügt.</td></tr><tr>
<td><code>containers-kubernetes.subnet.create</code></td>
<td>Ein Teilnetz wird erstellt.</td></tr><tr>
<td><code>containers-kubernetes.usersubnet.add</code></td>
<td>Ein benutzerverwaltetes Teilnetz wird zu einem Cluster hinzugefügt.</td></tr><tr>
<td><code>containers-kubernetes.usersubnet.delete</code></td>
<td>Ein benutzerverwaltetes Teilnetz wird aus einem Cluster entfernt.</td></tr><tr>
<td><code>containers-kubernetes.version.update</code></td>
<td>Die Kubernetes-Version eines Cluster-Master-Knotens wird aktualisiert.</td></tr><tr>
<td><code>containers-kubernetes.worker.create</code></td>
<td>Ein Workerknoten wird erstellt.</td></tr><tr>
<td><code>containers-kubernetes.worker.delete</code></td>
<td>Ein Workerknoten wird gelöscht.</td></tr><tr>
<td><code>containers-kubernetes.worker.get</code></td>
<td>Informationen zu einem Workerknoten werden angezeigt.</td></tr><tr>
<td><code>containers-kubernetes.worker.reboot</code></td>
<td>Ein Workerknoten wird erneut gestartet.</td></tr><tr>
<td><code>containers-kubernetes.worker.reload</code></td>
<td>Ein Workerknoten wird erneut geladen.</td></tr><tr>
<td><code>containers-kubernetes.worker.update</code></td>
<td>Ein Workerknoten wird aktualisiert.</td></tr>
</table>

## Kubernetes-Auditereignisse aufzeichnen
{: #kube-events}

In der folgenden Tabelle finden Sie eine Liste der Ereignisse von Kubernetes-API-Serveraudits, die an {{site.data.keyword.cloudaccesstrailshort}} gesendet werden.
{: shortdesc}

Vorbereitende Schritte: Stellen Sie sicher, dass der Cluster für die Weiterleitung von [Kubernetes-API-Auditereignissen](/docs/containers?topic=containers-health#api_forward) konfiguriert ist.

<table>
  <tr>
    <th>Aktion</th>
    <th>Beschreibung</th>
  </tr>
  <tr>
    <td><code>bindings.create</code></td>
    <td>Eine Bindung wird erstellt.</td>
  </tr>
  <tr>
    <td><code>certificatesigningrequests.create</code></td>
    <td>Eine Anforderung zum Signieren eines Zertifikats wird erstellt.</td>
  </tr>
  <tr>
    <td><code>certificatesigningrequests.delete</code></td>
    <td>Eine Anforderung zum Signieren eines Zertifikats wird gelöscht.</td>
  </tr>
  <tr>
    <td><code>certificatesigningrequests.patch</code></td>
    <td>Eine Anforderung zum Signieren eines Zertifikats wird korrigiert.</td>
  </tr>
  <tr>
    <td><code>certificatesigningrequests.update</code></td>
    <td>Eine Anforderung zum Signieren eines Zertifikats wird aktualisiert.</td>
  </tr>
  <tr>
    <td><code>clusterbindings.create</code></td>
    <td>Eine Clusterrollenbindung wird erstellt.</td>
  </tr>
  <tr>
    <td><code>clusterbindings.deleted</code></td>
    <td>Eine Clusterrollenbindung wird gelöscht.</td>
  </tr>
  <tr>
    <td><code>clusterbindings.patched</code></td>
    <td>Eine Clusterrollenbindung wird korrigiert.</td>
  </tr>
  <tr>
    <td><code>clusterbindings.updated</code></td>
    <td>Eine Clusterrollenbindung wird aktualisiert.</td>
  </tr>
  <tr>
    <td><code>clusterroles.create</code></td>
    <td>Eine Clusterrolle wird erstellt.</td>
  </tr>
  <tr>
    <td><code>clusterroles.deleted</code></td>
    <td>Eine Clusterrolle wird gelöscht.</td>
  </tr>
  <tr>
    <td><code>clusterroles.patched</code></td>
    <td>Eine Clusterrolle wird korrigiert.</td>
  </tr>
  <tr>
    <td><code>clusterroles.updated</code></td>
    <td>Eine Clusterrolle wird aktualisiert.</td>
  </tr>
  <tr>
    <td><code>configmaps.create</code></td>
    <td>Eine Konfigurationszuordnung wird erstellt.</td>
  </tr>
  <tr>
    <td><code>configmaps.delete</code></td>
    <td>Eine Konfigurationszuordnung wird gelöscht.</td>
  </tr>
  <tr>
    <td><code>configmaps.patch</code></td>
    <td>Eine Konfigurationszuordnung wird korrigiert.</td>
  </tr>
  <tr>
    <td><code>configmaps.update</code></td>
    <td>Eine Konfigurationszuordnung wird aktualisiert.</td>
  </tr>
  <tr>
    <td><code>controllerrevisions.create</code></td>
    <td>Eine Controllerrevision wird erstellt.</td>
  </tr>
  <tr>
    <td><code>controllerrevisions.delete</code></td>
    <td>Eine Controllerrevision wird gelöscht.</td>
  </tr>
  <tr>
    <td><code>controllerrevisions.patch</code></td>
    <td>Eine Controllerrevision wird korrigiert.</td>
  </tr>
  <tr>
    <td><code>controllerrevisions.update</code></td>
    <td>Eine Controllerrevision wird aktualisiert.</td>
  </tr>
  <tr>
    <td><code>daemonsets.create</code></td>
    <td>Eine Dämongruppe wird erstellt.</td>
  </tr>
  <tr>
    <td><code>daemonsets.delete</code></td>
    <td>Eine Dämongruppe wird gelöscht.</td>
  </tr>
  <tr>
    <td><code>daemonsets.patch</code></td>
    <td>Eine Dämongruppe wird korrigiert.</td>
  </tr>
  <tr>
    <td><code>daemonsets.update</code></td>
    <td>Eine Dämongruppe wird aktualisiert.</td>
  </tr>
  <tr>
    <td><code>deployments.create</code></td>
    <td>Eine Bereitstellung wird erstellt.</td>
  </tr>
  <tr>
    <td><code>deployments.delete</code></td>
    <td>Eine Bereitstellung wird gelöscht.</td>
  </tr>
  <tr>
    <td><code>deployments.patch</code></td>
    <td>Eine Bereitstellung wird korrigiert.</td>
  </tr>
  <tr>
    <td><code>deployments.update</code></td>
    <td>Eine Bereitstellung wird aktualisiert.</td>
  </tr>
  <tr>
    <td><code>events.create</code></td>
    <td>Ein Ereignis wird erstellt.</td>
  </tr>
  <tr>
    <td><code>events.delete</code></td>
    <td>Ein Ereignis wird gelöscht.</td>
  </tr>
  <tr>
    <td><code>events.patch</code></td>
    <td>Ein Ereignis wird korrigiert.</td>
  </tr>
  <tr>
    <td><code>events.update</code></td>
    <td>Ein Ereignis wird aktualisiert.</td>
  </tr>
  <tr>
    <td><code>externaladmissionhookconfigurations.create</code></td>
    <td>In Kubernetes Version 1.8 wird eine Konfiguration für External Admission Webhooks erstellt.</td>
  </tr>
  <tr>
    <td><code>externaladmissionhookconfigurations.delete</code></td>
    <td>In Kubernetes Version 1.8 wird eine Konfiguration für External Admission Webhooks gelöscht.</td>
  </tr>
  <tr>
    <td><code>externaladmissionhookconfigurations.patch</code></td>
    <td>In Kubernetes Version 1.8 wird eine Konfiguration für External Admission Webhooks korrigiert.</td>
  </tr>
  <tr>
    <td><code>externaladmissionhookconfigurations.update</code></td>
    <td>In Kubernetes Version 1.8 wird eine Konfiguration für External Admission Webhooks aktualisiert.</td>
  </tr>
  <tr>
    <td><code>horizontalpodautoscalers.create</code></td>
    <td>Eine Richtlinie zur horizontalen Autoskalierung von Pods wird erstellt.</td>
  </tr>
  <tr>
    <td><code>horizontalpodautoscalers.delete</code></td>
    <td>Eine Richtlinie zur horizontalen Autoskalierung von Pods wird gelöscht.</td>
  </tr>
  <tr>
    <td><code>horizontalpodautoscalers.patch</code></td>
    <td>Eine Richtlinie zur horizontalen Autoskalierung von Pods wird korrigiert.</td>
  </tr>
  <tr>
    <td><code>horizontalpodautoscalers.update</code></td>
    <td>Eine Richtlinie zur horizontalen Autoskalierung von Pods wird aktualisiert.</td>
  </tr>
  <tr>
    <td><code>ingresses.create</code></td>
    <td>Eine Ingress-ALB wird erstellt.</td>
  </tr>
  <tr>
    <td><code>ingresses.delete</code></td>
    <td>Eine Ingress-ALB wird gelöscht.</td>
  </tr>
  <tr>
    <td><code>ingresses.patch</code></td>
    <td>Eine Ingress-ALB wird korrigiert.</td>
  </tr>
  <tr>
    <td><code>ingresses.update</code></td>
    <td>Eine Ingress-ALB wird aktualisiert.</td>
  </tr>
  <tr>
    <td><code>jobs.create</code></td>
    <td>Ein Job wird erstellt.</td>
  </tr>
  <tr>
    <td><code>jobs.delete</code></td>
    <td>Ein Job wird gelöscht.</td>
  </tr>
  <tr>
    <td><code>jobs.patch</code></td>
    <td>Ein Job wird korrigiert.</td>
  </tr>
  <tr>
    <td><code>jobs.update</code></td>
    <td>Ein Job wird aktualisiert.</td>
  </tr>
  <tr>
    <td><code>localsubjectaccessreviews.create</code></td>
    <td>Eine Zugriffsprüfung für lokale Subjekte wird erstellt.</td>
  </tr>
  <tr>
    <td><code>limitranges.create</code></td>
    <td>Ein Bereichsgrenzwert wird erstellt.</td>
  </tr>
  <tr>
    <td><code>limitranges.delete</code></td>
    <td>Ein Bereichsgrenzwert wird gelöscht.</td>
  </tr>
  <tr>
    <td><code>limitranges.patch</code></td>
    <td>Ein Bereichsgrenzwert wird korrigiert.</td>
  </tr>
  <tr>
    <td><code>limitranges.update</code></td>
    <td>Eine Bereichsgrenze wird aktualisiert.</td>
  </tr>
  <tr>
    <td><code>mutatingwebhookconfigurations.create</code></td>
    <td>In Kubernetes Version 1.9 und höher wird eine Mutation der Webhook-Konfiguration erstellt.</td>
  </tr>
  <tr>
    <td><code>mutatingwebhookconfigurations.delete</code></td>
    <td>In Kubernetes Version 1.9 und höher wird eine Mutation der Webhook-Konfiguration gelöscht.</td>
  </tr>
  <tr>
    <td><code>mutatingwebhookconfigurations.patch</code></td>
    <td>In Kubernetes Version 1.9 und höher wird eine Mutation der Webhook-Konfiguration korrigiert.</td>
  </tr>
  <tr>
    <td><code>mutatingwebhookconfigurations.update</code></td>
    <td>In Kubernetes Version 1.9 und höher wird eine Mutation der Webhook-Konfiguration aktualisiert.</td>
  </tr>
  <tr>
    <td><code>namespaces.create</code></td>
    <td>Ein Namensbereich wird erstellt.</td>
  </tr>
  <tr>
    <td><code>namespaces.delete</code></td>
    <td>Ein Namensbereich wird gelöscht.</td>
  </tr>
  <tr>
    <td><code>namespaces.patch</code></td>
    <td>Ein Namensbereich wird korrigiert.</td>
  </tr>
  <tr>
    <td><code>namespaces.update</code></td>
    <td>Ein Namensbereich wird aktualisiert.</td>
  </tr>
  <tr>
    <td><code>networkpolicies.create</code></td>
    <td>Eine Netzrichtlinie wird erstellt.</td>
  </tr>
  <tr>
    <td><code>networkpolicies.delete</code></td>
    <td>Eine Netzrichtlinie wird gelöscht.</td>
  </tr>
  <tr>
    <td><code>networkpolicies.patch</code></td>
    <td>Eine Netzrichtlinie wird korrigiert.</td>
  </tr>
  <tr>
    <td><code>networkpolicies.update</code></td>
    <td>Eine Netzrichtlinie wird aktualisiert.</td>
  </tr>
  <tr>
    <td><code>nodes.create</code></td>
    <td>Ein Knoten wird erstellt.</td>
  </tr>
  <tr>
    <td><code>nodes.delete</code></td>
    <td>Ein Knoten wird gelöscht.</td>
  </tr>
  <tr>
    <td><code>nodes.patch</code></td>
    <td>Ein Knoten wird korrigiert.</td>
  </tr>
  <tr>
    <td><code>nodes.update</code></td>
    <td>Ein Knoten wird aktualisiert.</td>
  </tr>
  <tr>
    <td><code>persistentvolumeclaims.create</code></td>
    <td>Ein Persistent Volume Claim (PVC) wird erstellt.</td>
  </tr>
  <tr>
    <td><code>persistentvolumeclaims.delete</code></td>
    <td>Ein Persistent Volume Claim (PVC) wird gelöscht.</td>
  </tr>
  <tr>
    <td><code>persistentvolumeclaims.patch</code></td>
    <td>Ein Persistent Volume Claim (PVC) wird korrigiert.</td>
  </tr>
  <tr>
    <td><code>persistentvolumeclaims.update</code></td>
    <td>Ein Persistent Volume Claim (PVC) wird aktualisiert.</td>
  </tr>
  <tr>
    <td><code>persistentvolumes.create</code></td>
    <td>Ein persistenter Datenträger (PV) wird erstellt.</td>
  </tr>
  <tr>
    <td><code>persistentvolumes.delete</code></td>
    <td>Ein persistenter Datenträger wird gelöscht.</td>
  </tr>
  <tr>
    <td><code>persistentvolumes.patch</code></td>
    <td>Ein persistenter Datenträger wird korrigiert.</td>
  </tr>
  <tr>
    <td><code>persistentvolumes.update</code></td>
    <td>Ein persistenter Datenträger wird aktualisiert.</td>
  </tr>
  <tr>
    <td><code>poddisruptionbudgets.create</code></td>
    <td>Ein Budget für den Pod-Ausfall wird erstellt.</td>
  </tr>
  <tr>
    <td><code>poddisruptionbudgets.delete</code></td>
    <td>Ein Budget für den Pod-Ausfall wird gelöscht.</td>
  </tr>
  <tr>
    <td><code>poddisruptionbudgets.patch</code></td>
    <td>Ein Budget für den Pod-Ausfall wird korrigiert.</td>
  </tr>
  <tr>
    <td><code>poddisruptionbudgets.update</code></td>
    <td>Ein Budget für den Pod-Ausfall wird aktualisiert.</td>
  </tr>
  <tr>
    <td><code>podpresets.create</code></td>
    <td>Eine Pod-Voreinstellung wird erstellt.</td>
  </tr>
  <tr>
    <td><code>podpresets.deleted</code></td>
    <td>Eine Pod-Voreinstellung wird gelöscht.</td>
  </tr>
  <tr>
    <td><code>podpresets.patched</code></td>
    <td>Eine Pod-Voreinstellung wird korrigiert.</td>
  </tr>
  <tr>
    <td><code>podpresets.updated</code></td>
    <td>Eine Pod-Voreinstellung wird aktualisiert.</td>
  </tr>
  <tr>
    <td><code>pods.create</code></td>
    <td>Ein Pod wird erstellt.</td>
  </tr>
  <tr>
    <td><code>pods.delete</code></td>
    <td>Ein Pod wird gelöscht.</td>
  </tr>
  <tr>
    <td><code>pods.patch</code></td>
    <td>Ein Pod wird korrigiert.</td>
  </tr>
  <tr>
    <td><code>pods.update</code></td>
    <td>Ein Pod wird aktualisiert.</td>
  </tr>
  <tr>
    <td><code>podsecuritypolicies.create</code></td>
    <td>Es wird eine Pod-Sicherheitsrichtlinie erstellt.</td>
  </tr>
  <tr>
    <td><code>podsecuritypolicies.delete</code></td>
    <td>Es wird eine Pod-Sicherheitsrichtlinie gelöscht.</td>
  </tr>
  <tr>
    <td><code>podsecuritypolicies.patch</code></td>
    <td>Es wird eine Pod-Sicherheitsrichtlinie gepatcht.</td>
  </tr>
  <tr>
    <td><code>podsecuritypolicies.update</code></td>
    <td>Es wird eine Pod-Sicherheitsrichtlinie aktualisiert.</td>
  </tr>
  <tr>
    <td><code>podtemplates.create</code></td>
    <td>Eine Pod-Vorlage wird erstellt.</td>
  </tr>
  <tr>
    <td><code>podtemplates.delete</code></td>
    <td>Eine Pod-Vorlage wird gelöscht.</td>
  </tr>
  <tr>
    <td><code>podtemplates.patch</code></td>
    <td>Eine Pod-Vorlage wird korrigiert.</td>
  </tr>
  <tr>
    <td><code>podtemplates.update</code></td>
    <td>Eine Pod-Vorlage wird aktualisiert.</td>
  </tr>
  <tr>
    <td><code>replicasets.create</code></td>
    <td>Eine Replikatgruppe wird erstellt.</td>
  </tr>
  <tr>
    <td><code>replicasets.delete</code></td>
    <td>Eine Replikatgruppe wird gelöscht.</td>
  </tr>
  <tr>
    <td><code>replicasets.patch</code></td>
    <td>Eine Replikatgruppe wird korrigiert.</td>
  </tr>
  <tr>
    <td><code>replicasets.update</code></td>
    <td>Eine Replikatgruppe wird aktualisiert.</td>
  </tr>
  <tr>
    <td><code>replicationcontrollers.create</code></td>
    <td>Ein Replikationscontroller wird erstellt.</td>
  </tr>
  <tr>
    <td><code>replicationcontrollers.delete</code></td>
    <td>Ein Replikationscontroller wird gelöscht.</td>
  </tr>
  <tr>
    <td><code>replicationcontrollers.patch</code></td>
    <td>Ein Replikationscontroller wird korrigiert.</td>
  </tr>
  <tr>
    <td><code>replicationcontrollers.update</code></td>
    <td>Ein Replikationscontroller wird aktualisiert.</td>
  </tr>
  <tr>
    <td><code>resourcequotas.create</code></td>
    <td>Eine Ressourcenquote wird erstellt.</td>
  </tr>
  <tr>
    <td><code>resourcequotas.delete</code></td>
    <td>Eine Ressourcenquote wird gelöscht.</td>
  </tr>
  <tr>
    <td><code>resourcequotas.patch</code></td>
    <td>Eine Ressourcenquote wird korrigiert.</td>
  </tr>
  <tr>
    <td><code>resourcequotas.update</code></td>
    <td>Eine Ressourcenquote wird aktualisiert.</td>
  </tr>
  <tr>
    <td><code>rolebindings.create</code></td>
    <td>Eine Rollenbindung wird erstellt.</td>
  </tr>
  <tr>
    <td><code>rolebindings.deleted</code></td>
    <td>Eine Rollenbindung wird gelöscht.</td>
  </tr>
  <tr>
    <td><code>rolebindings.patched</code></td>
    <td>Eine Rollenbindung wird korrigiert.</td>
  </tr>
  <tr>
    <td><code>rolebindings.updated</code></td>
    <td>Eine Rollenbindung wird aktualisiert.</td>
  </tr>
  <tr>
    <td><code>roles.create</code></td>
    <td>Eine Rolle wird erstellt.</td>
  </tr>
  <tr>
    <td><code>roles.deleted</code></td>
    <td>Eine Rolle wird gelöscht.</td>
  </tr>
  <tr>
    <td><code>roles.patched</code></td>
    <td>Eine Rolle wird korrigiert.</td>
  </tr>
  <tr>
    <td><code>roles.updated</code></td>
    <td>Eine Rolle wird aktualisiert.</td>
  </tr>
  <tr>
    <td><code>secrets.create</code></td>
    <td>Ein geheimer Schlüssel wird erstellt.</td>
  </tr>
  <tr>
    <td><code>secrets.deleted</code></td>
    <td>Ein geheimer Schlüssel wird gelöscht.</td>
  </tr>
  <tr>
    <td><code>secrets.get</code></td>
    <td>Ein geheimer Schlüssel wird angezeigt.</td>
  </tr>
  <tr>
    <td><code>secrets.patch</code></td>
    <td>Ein geheimer Schlüssel wird korrigiert.</td>
  </tr>
  <tr>
    <td><code>secrets.updated</code></td>
    <td>Ein geheimer Schlüssel wird aktualisiert.</td>
  </tr>
  <tr>
    <td><code>selfsubjectaccessreviews.create</code></td>
    <td>Eine Zugriffsprüfung für Self-Subjekte wird erstellt.</td>
  </tr>
  <tr>
    <td><code>selfsubjectrulesreviews.create</code></td>
    <td>Eine Regelprüfung für Self-Subjekte wird erstellt.</td>
  </tr>
  <tr>
    <td><code>subjectaccessreviews.create</code></td>
    <td>Eine Zugriffsprüfung für Subjekte wird erstellt.</td>
  </tr>
  <tr>
    <td><code>serviceaccounts.create</code></td>
    <td>Ein Servicekonto wird erstellt.</td>
  </tr>
  <tr>
    <td><code>serviceaccounts.deleted</code></td>
    <td>Ein Servicekonto wird gelöscht.</td>
  </tr>
  <tr>
    <td><code>serviceaccounts.patch</code></td>
    <td>Ein Servicekonto wird korrigiert.</td>
  </tr>
  <tr>
    <td><code>serviceaccounts.updated</code></td>
    <td>Ein Servicekonto wird aktualisiert.</td>
  </tr>
  <tr>
    <td><code>services.create</code></td>
    <td>Ein Service wird erstellt.</td>
  </tr>
  <tr>
    <td><code>services.deleted</code></td>
    <td>Ein Service wird gelöscht.</td>
  </tr>
  <tr>
    <td><code>services.patch</code></td>
    <td>Ein Service wird korrigiert.</td>
  </tr>
  <tr>
    <td><code>services.updated</code></td>
    <td>Ein Service wird aktualisiert.</td>
  </tr>
  <tr>
    <td><code>statefulsets.create</code></td>
    <td>Eine statusabhängige Gruppe wird erstellt.</td>
  </tr>
  <tr>
    <td><code>statefulsets.delete</code></td>
    <td>Eine statusabhängige Gruppe wird gelöscht.</td>
  </tr>
  <tr>
    <td><code>statefulsets.patch</code></td>
    <td>Eine statusabhängige Gruppe wird korrigiert.</td>
  </tr>
  <tr>
    <td><code>statefulsets.update</code></td>
    <td>Eine statusabhängige Gruppe wird aktualisiert.</td>
  </tr>
  <tr>
    <td><code>tokenreviews.create</code></td>
    <td>Eine Tokenprüfung wird erstellt.</td>
  </tr>
  <tr>
    <td><code>validatingwebhookconfigurations.create</code></td>
    <td>In Kubernetes Version 1.9 und höher wird eine Validierung der Webhook-Konfiguration erstellt.</td>
  </tr>
  <tr>
    <td><code>validatingwebhookconfigurations.delete</code></td>
    <td>In Kubernetes Version 1.9 und höher wird eine Validierung der Webhook-Konfiguration gelöscht.</td>
  </tr>
  <tr>
    <td><code>validatingwebhookconfigurations.patch</code></td>
    <td>In Kubernetes Version 1.9 und höher wird eine Validierung der Webhook-Konfiguration korrigiert.</td>
  </tr>
  <tr>
    <td><code>validatingwebhookconfigurations.update</code></td>
    <td>In Kubernetes Version 1.9 und höher wird die Validierung der Webhook-Konfiguration aktualisiert .</td>
  </tr>
</table>
