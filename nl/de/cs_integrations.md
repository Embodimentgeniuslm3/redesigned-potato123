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


# Services integrieren
{: #integrations}

Sie können verschiedene externe Services und Katalogservices mit einem Kubernetes-Standardcluster in {{site.data.keyword.containerlong}} verwenden.
{:shortdesc}


## DevOps-Services
{: #devops_services}
<table summary="Die Tabelle enthält verfügbare Services, die Sie Ihrem Cluster hinzufügen können, um zusätzliche DevOps-Funktionen hinzuzufügen. Die Zeilen sind von links nach rechts zu lesen, wobei der Name des Service in Spalte 1 und eine Beschreibung des Service in Spalte 2 enthalten sind.">
<caption>DevOps-Services</caption>
<thead>
<tr>
<th>Service</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.cfee_full_notm}}</td>
<td>Mit diesem Service können Sie Ihre eigene Cloud Foundry-Plattform in einer Kubernetes-Clusterumgebung bereitstellen und verwalten, um cloudnative Apps zu entwickeln, zu paketieren, bereitzustellen und zu verwalten. Darüber hinaus können Sie die {{site.data.keyword.Bluemix_notm}}-Umgebung zum Binden weiterer Services an Ihre Apps nutzen. Wenn Sie eine {{site.data.keyword.cfee_full_notm}}-Instanz erstellen, müssen Sie Ihren Kubernetes-Cluster konfigurieren, indem Sie den Maschinentyp und die VLANs für Ihre Workerknoten auswählen. Ihr Cluster wird dann mit {{site.data.keyword.containerlong_notm}} ausgestattet und {{site.data.keyword.cfee_full_notm}} wird automatisch in Ihrem Cluster bereitgestellt. Weitere Informationen zur Einrichtung von {{site.data.keyword.cfee_full_notm}} finden Sie im [Lernprogramm zur Einführung](/docs/cloud-foundry?topic=cloud-foundry-getting-started#getting-started). </td>
</tr>
<tr>
<td>Codeship</td>
<td>Mit <a href="https://codeship.com" target="_blank">Codeship <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a> können Sie die kontinuierliche Integration und Bereitstellung von Containern vorantreiben. Weitere Informationen finden Sie unter <a href="https://www.ibm.com/blogs/bluemix/2017/10/using-codeship-pro-deploy-workloads-ibm-container-service/" target="_blank">Using Codeship Pro To Deploy Workloads to {{site.data.keyword.containerlong_notm}} <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a>. </td>
</tr>
<tr>
<td>Grafeas</td>
<td>[Grafeas ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://grafeas.io) ist ein Open-Source-CI/CD-Service, der eine einheitliche Verfahrensweise zum Abrufen, Speichern und Austauschen von Metadaten während des Software-Supply-Chain-Prozesses zur Verfügung stellt. Wenn Sie Grafeas zum Beispiel in Ihren Buildprozess für Apps integrieren, kann Grafeas Informationen zum Initiator der Buildanforderung, zu Ergebnissen von Schwachstellenprüfungen und zur Qualitätssicherungsgenehmigung speichern, sodass Sie eine fundierte Entscheidung treffen können, ob eine App für den Produktionseinsatz bereitgestellt werden kann. Sie können diese Metadaten in Audits oder zum Nachweis der Einhaltung von Vorschriften für Ihre Software-Supply-Chain verwenden. </td>
</tr>
<tr>
<td>Helm</td>
<td> <a href="https://helm.sh" target="_blank">Helm <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a> ist ein Kubernetes-Paketmanager. Sie können neue Helm-Diagramme zum Definieren, Installieren und Durchführen von Upgrades für komplexe Kubernetes-Anwendungen, die in {{site.data.keyword.containerlong_notm}}-Clustern ausgeführt werden, erstellen. <p>Weitere Informationen finden Sie unter [Helm in {{site.data.keyword.containerlong_notm}} konfigurieren](/docs/containers?topic=containers-integrations#helm).</p></td>
</tr>
<tr>
<td>{{site.data.keyword.contdelivery_full}}</td>
<td>Automatisieren Sie die App-Builds und die Containerbereitstellungen in Kubernetes-Clustern mithilfe einer Toolchain. Informationen zur Konfiguration finden Sie in dem Blog <a href="https://developer.ibm.com/recipes/tutorials/deploy-kubernetes-pods-to-the-bluemix-container-service-using-devops-pipelines/" target="_blank">Deploy Kubernetes pods to the {{site.data.keyword.containerlong_notm}} using DevOps Pipelines <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a>. </td>
</tr>
<tr>
<td>Istio on {{site.data.keyword.containerlong_notm}}</td>
<td><a href="https://www.ibm.com/cloud/info/istio" target="_blank">Istio <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a> ist ein Open-Source-Service, der Entwicklern eine Möglichkeit zum Verbinden, Sichern, Verwalten und Überwachen eines Netzes von Microservices (auch als Servicenetz bezeichnet) auf Cloudorchestrierungsplattformen bietet. Istio on {{site.data.keyword.containerlong}} ermöglicht eine Installation von Istio durch ein verwaltetes Add-on in Ihrem Cluster in nur einem Schritt. Durch einen Klick können Sie alle Istio-Kernkomponenten, zusätzliche Trace-, Überwachungs- und Visualisierungsfunktionen sowie die betriebsbereite Beispielapp 'BookInfo' abrufen. Informationen zum Einstieg finden Sie unter [Verwaltetes Istio-Add-on verwenden (Beta)](/docs/containers?topic=containers-istio).</td>
</tr>
<tr>
<td>Jenkins X</td>
<td>Jenkins X ist eine Kubernetes-native Plattform für kontinuierliche Integration und Continuous Delivery, die Sie zum Automatisieren Ihres Erstellungsprozesses verwenden können. Weitere Informationen zur Installation in {{site.data.keyword.containerlong_notm}} finden Sie in der [Einführung in das Open-Source-Projekt Jenkins X ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/blogs/bluemix/2018/08/installing-jenkins-x-on-ibm-cloud-kubernetes-service/).</td>
</tr>
<tr>
<td>Knative</td>
<td>[Knative ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/knative/docs) ist eine Open-Source-Plattform, die von IBM, Google, Pivotal, Red Hat, Cisco und anderen dazu entwickelt wurde, die Funktionalität von Kubernetes zu erweitern, um Sie bei der Erstellung moderner, quellenzentrierter, containerisierter und serverunabhängiger Apps auf der Basis eines Kubernetes-Clusters zu unterstützen. Die Plattform nutzt ein konsistentes, Programmiersprachen und Frameworks übergreifendes Konzept, um den Betriebsaufwand für die Builderstellung, Bereitstellung und Verwaltung von Workloads in Kubernetes zusammenzufassen, sodass Entwickler sich auf das Wichtigste konzentrieren können: den Quellcode. Weitere Informationen finden Sie im [Lernprogramm: Verwaltetes Knative-Add-on zum Ausführen serverunabhängiger Apps in Kubernetes-Clustern verwenden](/docs/containers?topic=containers-knative_tutorial#knative_tutorial). </td>
</tr>
</tbody>
</table>

<br />



## Protokollierungs- und Überwachungsservices
{: #health_services}
<table summary="Die Tabelle enthält verfügbare Services, die Sie Ihrem Cluster hinzufügen können, um zusätzliche Protokollierungs- und Überwachungsfunktionen hinzuzufügen. Die Zeilen sind von links nach rechts zu lesen, wobei der Name des Service in Spalte 1 und eine Beschreibung des Service in Spalte 2 enthalten sind.">
<caption>Protokollierungs- und Überwachungsservices</caption>
<thead>
<tr>
<th>Service</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>CoScale</td>
<td>Überwachen Sie Workerknoten, Container, Replikatgruppen, Replikationscontroller und Services mit <a href="https://www.newrelic.com/coscale" target="_blank">CoScale <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a>. Weitere Informationen finden Sie unter <a href="https://www.ibm.com/blogs/bluemix/2017/06/monitoring-ibm-bluemix-container-service-coscale/" target="_blank">Monitoring {{site.data.keyword.containerlong_notm}} with CoScale <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a>. </td>
</tr>
<tr>
<td>Datadog</td>
<td>Überwachen Sie Ihren Cluster und zeigen Sie Metriken für die Infrastruktur- und Anwendungsleistung mit <a href="https://www.datadoghq.com/" target="_blank">Datadog <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a> an. Weitere Informationen finden Sie unter <a href="https://www.ibm.com/blogs/bluemix/2017/07/monitoring-ibm-bluemix-container-service-datadog/" target="_blank">Monitoring {{site.data.keyword.containerlong_notm}} with Datadog <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a>. </td>
</tr>
<tr>
<td>{{site.data.keyword.cloudaccesstrailfull}}</td>
<td>Überwachen Sie die in Ihrem Cluster vorgenommenen Verwaltungsaktivitäten, indem Sie die Protokolle über Grafana analysieren. Weitere Informationen zum Service finden Sie in der Dokumentation zu [Activity Tracker](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started-with-cla). Weitere Informationen zu den Typen von Ereignissen, die Sie überwachen können, finden Sie im Abschnitt [Activity Tracker-Ereignisse](/docs/containers?topic=containers-at_events).</td>
</tr>
<tr>
<td>{{site.data.keyword.la_full_notm}}</td>
<td>Fügen Sie Ihrem Cluster Protokollmanagementfunktionen hinzu, indem Sie LogDNA als Drittanbieterservice auf Ihren Workerknoten implementieren, um Protokolle aus Ihren Pod-Containern zu verwalten. Weitere Informationen finden Sie unter [Kubernetes-Clusterprotokolle mit {{site.data.keyword.loganalysisfull_notm}} und LogDNA verwalten](/docs/services/Log-Analysis-with-LogDNA/tutorials?topic=LogDNA-kube#kube).</td>
</tr>
<tr>
<td>{{site.data.keyword.mon_full_notm}}</td>
<td>Gewinnen Sie betriebliche Einblicke in die Leistung und den Allgemeinzustand Ihrer Apps, indem Sie Sysdig als Drittanbieterservice auf Ihren Workerknoten bereitstellen, um Metriken an {{site.data.keyword.monitoringlong}} weiterzuleiten. Weitere Informationen finden Sie unter [Metriken für eine App analysieren, die in einem Kubernetes-Cluster bereitgestellt wurde](/docs/services/Monitoring-with-Sysdig/tutorials?topic=Sysdig-kubernetes_cluster#kubernetes_cluster). </td>
</tr>
<tr>
<td>Instana</td>
<td> <a href="https://www.instana.com/" target="_blank">Instana <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a> bietet eine Leistungsüberwachung von Infrastrukturen und Apps über eine grafische Benutzerschnittstelle, die automatisch Apps erkennt und zuordnet. Instana erfasst alle Anforderungen an Ihre Apps und ermöglicht Ihnen damit die Durchführung von Fehler- und Ursachenanalysen, um zu vermeiden, dass Probleme erneut auftreten. Lesen Sie dazu den Blogeintrag zur <a href="https://www.instana.com/blog/precise-visibility-applications-ibm-bluemix-container-service/" target="_blank">Bereitstellung von Instana in {{site.data.keyword.containerlong_notm}} <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a>, um weitere Informationen zu erhalten.</td>
</tr>
<tr>
<td>Prometheus</td>
<td>Prometheus ist ein Open-Source-Tool für die Überwachung, Protokollierung und Benachrichtigung bei Alerts, das speziell für Kubernetes entwickelt wurde. Prometheus ruft auf Grundlage der Kubernetes-Protokollierungsinformationen detaillierte Informationen zu Cluster, Workerknoten und Bereitstellungszustand ab. Informationen zu CPU, Speicher, Ein-/Ausgabe und Netzaktivität werden für alle Container erfasst, die in einem Cluster ausgeführt werden. Sie können die erfassten Daten in angepassten Abfragen oder Alerts verwenden, um die Leistung und Workloads im Cluster zu überwachen.

<p>Um Prometheus zu verwenden, befolgen Sie die <a href="https://github.com/coreos/prometheus-operator/tree/master/contrib/kube-prometheus" target="_blank">CoreOS-Anweisungen <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a>.</p>
</td>
</tr>
<tr>
<td>Sematext</td>
<td>Zeigen Sie Metriken und Protokolle für Ihre containerisierten Anwendungen mithilfe von <a href="https://sematext.com/" target="_blank">Sematext <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a> an. Weitere Informationen finden Sie unter <a href="https://www.ibm.com/blogs/bluemix/2017/09/monitoring-logging-ibm-bluemix-container-service-sematext/" target="_blank">Monitoring and logging for containers with Sematext <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a>. </td>
</tr>
<tr>
<td>Splunk</td>
<td>Mit Splunk Connect for Kubernetes können Sie Ihre Protokoll-, Objekt- und Metrikdaten in Splunk importieren und durchsuchen. Splunk Connect for Kubernetes ist eine Sammlung von Helm-Diagrammen, die eine von Splunk unterstützte Bereitstellung von Fluentd in Ihrem Kubernetes-Cluster, ein von Splunk erstelltes Plug-in 'Fluentd HTTP Event Collector (HEC)' zum Senden von Protokollen und Metadaten sowie eine Bereitstellung von Metriken, die Ihre Clustermetriken erfasst, bereitstellt. Weitere Informationen finden Sie im Abschnitt zur <a href="https://www.ibm.com/blogs/bluemix/2019/02/solving-business-problems-with-splunk-on-ibm-cloud-kubernetes-service/" target="_blank">Lösung von Geschäftsproblemen mit Splunk in {{site.data.keyword.containerlong_notm}} <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a>.</td>
</tr>
<tr>
<td>Weave Scope</td>
<td>Weave Scope liefert eine grafisch orientierte Diagrammdarstellung Ihrer Ressourcen in einem Kubernetes-Cluster unter Einbeziehung von Services, Pods, Containern, Prozessen, Knoten und vielem mehr. Weave Scope stellt interaktive Metriken für CPU und Speicher bereit und bietet Tools, um 'tail'- und 'exec'-Aufrufe in einem Container durchzuführen.<p>Weitere Informationen finden Sie unter [Kubernetes-Clusterressourcen mit Weave Scope und {{site.data.keyword.containerlong_notm}} grafisch darstellen](/docs/containers?topic=containers-integrations#weavescope).</p></li></ol>
</td>
</tr>
</tbody>
</table>

<br />



## Sicherheitsservices
{: #security_services}

Möchten Sie einen umfassenden Überblick über die Integration von {{site.data.keyword.Bluemix_notm}}-Sicherheitsservices im Cluster? Überprüfen Sie die Informationen im [Lernprogramm zum Anwenden durchgängiger Sicherheit in einer Cloudanwendung](/docs/tutorials?topic=solution-tutorials-cloud-e2e-security).
{: shortdesc}

<table summary="Die Tabelle enthält verfügbare Services, die Sie Ihrem Cluster hinzufügen können, um zusätzliche Sicherheitsfunktionen hinzuzufügen. Die Zeilen sind von links nach rechts zu lesen, wobei der Name des Service in Spalte 1 und eine Beschreibung des Service in Spalte 2 enthalten sind.">
<caption>Sicherheitsservices</caption>
<thead>
<tr>
<th>Service</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
  <tr id="appid">
    <td>{{site.data.keyword.appid_full}}</td>
    <td>Fügen Sie Ihren Apps mit [{{site.data.keyword.appid_short}}](/docs/services/appid?topic=appid-getting-started) eine Sicherheitsebene hinzu, indem Sie Benutzer dazu verpflichten, sich anzumelden. Um Web- oder API-HTTP/HTTPS-Anforderungen in Ihrer App zu authentifizieren, können Sie {{site.data.keyword.appid_short_notm}} in Ihren Ingress-Service integrieren, indem Sie die [{{site.data.keyword.appid_short_notm}}Annotation zur Ingress-Authentifizierung](/docs/containers?topic=containers-ingress_annotation#appid-auth) verwenden.</td>
  </tr>
<tr>
<td>Aqua Security</td>
  <td>Sie können <a href="/docs/services/va?topic=va-va_index" target="_blank">Vulnerability Advisor</a> durch <a href="https://www.aquasec.com/" target="_blank">Aqua Security <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a> ergänzen, um die Sicherheit von Containerbereitstellungen zu optimieren, indem Sie die zulässigen Funktionen für Ihre Apps einschränken. Weitere Informationen finden Sie in der Veröffentlichung <a href="https://blog.aquasec.com/securing-container-deployments-on-bluemix-with-aqua-security" target="_blank">Securing container deployments on {{site.data.keyword.Bluemix_notm}} with Aqua Security <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a>. </td>
</tr>
<tr>
<td>{{site.data.keyword.cloudcerts_full}}</td>
<td>Sie können <a href="/docs/services/certificate-manager?topic=certificate-manager-getting-started#getting-started" target="_blank">{{site.data.keyword.cloudcerts_long}} <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a> verwenden, um SSL-Zertifikate für Ihre Apps zu speichern und zu verwalten. Weitere Informationen finden Sie unter <a href="https://www.ibm.com/blogs/bluemix/2018/01/use-ibm-cloud-certificate-manager-ibm-cloud-container-service-deploy-custom-domain-tls-certificates/" target="_blank">Use {{site.data.keyword.cloudcerts_long_notm}} with {{site.data.keyword.containerlong_notm}} to deploy custom domain TLS Certificates <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a>. </td>
</tr>
<tr>
  <td>{{site.data.keyword.datashield_full}} (Beta)</td>
  <td>Sie können <a href="/docs/services/data-shield?topic=data-shield-getting-started#getting-started" target="_blank">{{site.data.keyword.datashield_short}} <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a> verwenden, um Ihren Datenspeicher zu verschlüsseln. {{site.data.keyword.datashield_short}} ist in Intel® Software Guard Extensions (SGX) und Fortanix®-Technologie integriert, sodass der Workload-Code und die Daten Ihres {{site.data.keyword.Bluemix_notm}}-Containers während der Nutzung geschützt werden. Der App-Code und die Daten werden in speziellen CPU-Schutzenklaven ausgeführt. Dabei handelt es sich um vertrauenswürdige Speicherbereiche auf dem Workerknoten, die kritische Aspekte der App schützen und dabei helfen, Code und Daten vertraulich und unverändert zu halten.</td>
</tr>
<tr>
  <td>{{site.data.keyword.registrylong}}</td>
  <td>Richten Sie Ihr eigenes gesichertes Docker-Image-Repository ein, in dem Sie Images sicher speichern und mit Clusterbenutzern teilen können. Weitere Informationen finden Sie in der <a href="/docs/services/Registry?topic=registry-getting-started" target="_blank">{{site.data.keyword.registrylong}}-Dokumentation <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a>.</td>
</tr>
<tr>
  <td>{{site.data.keyword.keymanagementservicefull}}</td>
  <td>Verschlüsseln Sie die geheimen Kubernetes-Schlüssel im Cluster durch die Aktivierung von {{site.data.keyword.keymanagementserviceshort}}. Das Verschlüsseln der geheimen Kubernetes-Schlüssel verhindert, dass nicht berechtigte Benutzer auf sensible Clusterinformationen zugreifen können.<br>Informationen zum Einrichten finden Sie in <a href="/docs/containers?topic=containers-encryption#keyprotect">Geheime Kubernetes-Schlüssel mit {{site.data.keyword.keymanagementserviceshort}} verschlüsseln</a>.<br>Weitere Informationen finden Sie in der <a href="/docs/services/key-protect?topic=key-protect-getting-started-tutorial" target="_blank">{{site.data.keyword.keymanagementserviceshort}}-Dokumentation <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a>.</td>
</tr>
<tr>
<td>NeuVector</td>
<td>Schützen Sie Container durch eine native Cloud-Firewall mithilfe von <a href="https://neuvector.com/" target="_blank">NeuVector <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a>. Weitere Informationen finden Sie unter <a href="https://www.ibm.com/us-en/marketplace/neuvector-container-security" target="_blank">NeuVector Container Security <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a>. </td>
</tr>
<tr>
<td>Twistlock</td>
<td>Sie können <a href="/docs/services/va?topic=va-va_index" target="_blank">Vulnerability Advisor</a> durch <a href="https://www.twistlock.com/" target="_blank">Twistlock <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a> ergänzen, um Firewalls, den Schutz vor Bedrohungen und die Behebung von Störfällen zu verwalten. Weitere Informationen finden Sie unter <a href="https://www.ibm.com/blogs/bluemix/2017/07/twistlock-ibm-bluemix-container-service/" target="_blank">Twistlock on {{site.data.keyword.containerlong_notm}} <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a>. </td>
</tr>
</tbody>
</table>

<br />



## Speicherservices
{: #storage_services}
<table summary="Die Tabelle enthält verfügbare Services, die Sie Ihrem Cluster hinzufügen können, um persistente Speicherfunktionen hinzuzufügen. Die Zeilen sind von links nach rechts zu lesen, wobei der Name des Service in Spalte 1 und eine Beschreibung des Service in Spalte 2 enthalten sind.">
<caption>Speicherservices</caption>
<thead>
<tr>
<th>Service</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
  <td>Heptio Velero</td>
  <td>Sie können <a href="https://github.com/heptio/velero" target="_blank">Heptio Velero <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a> verwenden, um Clusterressourcen und persistente Datenträger (PVs - Persistent Volumes) zu sichern und wiederherzustellen. Weitere Informationen finden Sie in der Heptio Velero-Veröffentlichung zu <a href="https://github.com/heptio/velero/blob/release-0.9/docs/use-cases.md" target="_blank">Anwendungsfällen für Disaster-Recovery und Clustermigration <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a>.</td>
</tr>
<tr>
  <td>{{site.data.keyword.Bluemix_notm}}-Blockspeicher</td>
  <td>[{{site.data.keyword.Bluemix_notm}} Block Storage](/docs/infrastructure/BlockStorage?topic=BlockStorage-getting-started#getting-started) ist ein persistenter, hochleistungsfähiger iSCSI-Speicher, den Sie Ihren Apps durch persistente Kubernetes-Datenträger (PVs) hinzufügen können. Verwenden Sie Blockspeicher, um statusabhängige Apps in einer einzelnen Zone bereitzustellen, oder als Hochleistungsspeicher für einzelne Pods. Weitere Informationen zur Bereitstellung von Blockspeicher in einem Cluster finden Sie unter [Daten in {{site.data.keyword.Bluemix_notm}}-Blockspeicher speichern](/docs/containers?topic=containers-block_storage#block_storage).</td>
  </tr>
<tr>
  <td>{{site.data.keyword.cos_full}}</td>
  <td>In {{site.data.keyword.cos_short}} gespeicherte Daten werden verschlüsselt und über mehrere geografische Regionen verteilt. Auf sie kann über HTTP mithilfe einer REST-API zugegriffen werden. Sie können den Befehl [ibm-backup-restore image](/docs/services/RegistryImages/ibm-backup-restore?topic=RegistryImages-ibmbackup_restore_starter) verwenden, um den Service so zu konfigurieren, dass er einmalige oder geplante Sicherungen von Daten in Ihrem Cluster ausführt. Allgemeine Informationen zu dem Service finden Sie in der <a href="/docs/services/cloud-object-storage?topic=cloud-object-storage-about-ibm-cloud-object-storage" target="_blank">{{site.data.keyword.cos_short}}-Dokumentation <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a>.</td>
</tr>
  <tr>
  <td>{{site.data.keyword.Bluemix_notm}} File Storage</td>
  <td>[{{site.data.keyword.Bluemix_notm}} File Storage](/docs/infrastructure/FileStorage?topic=FileStorage-getting-started#getting-started) ist ein persistenter, schneller und flexibler, über ein Netz angeschlossener NFS-basierter Dateispeicher, den Sie Ihren Apps durch persistente Kubernetes-Datenträger hinzufügen können. Sie können unter vordefinierten Speichertiers mit GB-Größen und E/A-Operationen pro Sekunde (IOPS) wählen, die die Anforderungen Ihrer Workloads erfüllen. Weitere Informationen zur Bereitstellung von Dateispeicher in einem Cluster finden Sie unter [Daten in {{site.data.keyword.Bluemix_notm}}-Dateispeicher speichern](/docs/containers?topic=containers-file_storage#file_storage).</td>
  </tr>
  <tr>
    <td>Portworx</td>
    <td>[Portworx ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://portworx.com/products/introduction/) ist eine Lösung für hoch verfügbaren, durch Software definierten Speicher, die Sie zur Verwaltung von persistentem Speicher für Ihre containerisierten Datenbanken und andere statusabhängigen Apps oder zur zonenübergreifenden gemeinsamen Nutzung von Daten zwischen Pods einsetzen können. Sie können Portworx mit einem Helm-Diagramm installieren und Speicher für Ihre Apps durch persistente Kubernetes-Datenträger bereitstellen. Weitere Informationen zur Einrichtung von Portworx in Ihrem Cluster finden Sie unter [Daten in softwaredefiniertem Speicher (SDS) mit Portworx speichern](/docs/containers?topic=containers-portworx#portworx).</td>
  </tr>
</tbody>
</table>

<br />


## Datenbankservices
{: #database_services}

<table summary="Die Tabelle enthält verfügbare Services, die Sie Ihrem Cluster hinzufügen können, um Datenbankfunktionalität hinzuzufügen. Die Zeilen sind von links nach rechts zu lesen, wobei der Name des Service in Spalte 1 und eine Beschreibung des Service in Spalte 2 enthalten sind.">
<caption>Datenbankservices</caption>
<thead>
<tr>
<th>Service</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
  <tr>
    <td>{{site.data.keyword.blockchainfull_notm}} Platform 2.0 Beta</td>
    <td>Sie können eine eigene {{site.data.keyword.blockchainfull_notm}} Platform on {{site.data.keyword.containerlong_notm}} bereitstellen und verwalten. Mit {{site.data.keyword.blockchainfull_notm}} Platform 2.0 können Sie {{site.data.keyword.blockchainfull_notm}}-Netze betreiben oder Organisationen erstellen, die an anderen {{site.data.keyword.blockchainfull_notm}} 2.0-Netzen teilnehmen können. Weitere Informationen zur Einrichtung von {{site.data.keyword.blockchainfull_notm}} in {{site.data.keyword.containerlong_notm}} finden Sie in den [Informationen zu {{site.data.keyword.blockchainfull_notm}} Platform Free 2.0 Beta](/docs/services/blockchain?topic=blockchain-ibp-console-overview#ibp-console-overview).</td>
  </tr>
<tr>
  <td>Cloud-Datenbanken</td>
  <td>Sie haben die Auswahl unter einer ganzen Reihe von {{site.data.keyword.Bluemix_notm}}-Datenbankservices, wie zum Beispiel {{site.data.keyword.composeForMongoDB_full}} oder {{site.data.keyword.cloudantfull}}, um hoch verfügbare und skalierbare Datenbanklösungen in Ihrem Cluster bereitzustellen. Eine Liste der verfügbaren Cloud-Datenbanken finden Sie im [{{site.data.keyword.Bluemix_notm}}-Katalog ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/catalog?category=databases).  </td>
  </tr>
  </tbody>
  </table>


## {{site.data.keyword.Bluemix_notm}}-Services zu Clustern hinzufügen
{: #adding_cluster}

Fügen Sie {{site.data.keyword.Bluemix_notm}}-Services hinzu, um Ihren Kubernetes-Cluster mit zusätzlichen Funktionen in Bereichen wie Watson AI, Daten, Sicherheit und Internet of Things (IoT) zu erweitern.
{:shortdesc}

Sie können nur Services binden, die Serviceschlüssel unterstützen. Eine Liste mit Services, die Serviceschlüssel unterstützen, finden Sie im Abschnitt [Externen Apps die Verwendung von {{site.data.keyword.Bluemix_notm}}-Services ermöglichen](/docs/resources?topic=resources-externalapp#externalapp).
{: note}

Vorbereitende Schritte:
- Stellen Sie sicher, dass Sie die folgenden Rollen innehaben:
    - [{{site.data.keyword.Bluemix_notm}} IAM-Servicerolle **Editor** oder **Administrator**](/docs/containers?topic=containers-users#platform) für den Cluster.
    - [{{site.data.keyword.Bluemix_notm}} IAM-Servicerolle **Schreibberechtigter** oder **Manager**](/docs/containers?topic=containers-users#platform) für den Namensbereich, in dem Sie den Service binden wollen.
    - [Cloud Foundry-Rolle **Entwickler** (Developer)](/docs/iam?topic=iam-mngcf#mngcf) für den Bereich, den Sie verwenden wollen.
- [Melden Sie sich an Ihrem Konto an. Geben Sie als Ziel die entsprechende Region und, sofern zutreffend, die Ressourcengruppe an. Legen Sie den Kontext für den Cluster fest.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

Gehen Sie wie folgt vor, um Ihrem Cluster einen {{site.data.keyword.Bluemix_notm}}-Service hinzuzufügen:

1. [Erstellen Sie eine Instanz des {{site.data.keyword.Bluemix_notm}}-Service](/docs/resources?topic=resources-externalapp#externalapp).
    * Bestimmte {{site.data.keyword.Bluemix_notm}}-Services sind nur in ausgewählten Regionen verfügbar. Sie können einen Service nur an Ihren Cluster binden, wenn der Service in derselben Region wie Ihr Cluster verfügbar ist. Wenn Sie eine Serviceinstanz in der Zone 'Washington DC' erstellen wollen, müssen Sie außerdem die CLI verwenden.
    * Sie müssen die Serviceinstanz in derselben Ressourcengruppe wie den Cluster erstellen. Eine Ressource kann in nur einer Ressourcengruppe erstellt werden; eine Änderung ist danach nicht mehr möglich.

2. Überprüfen Sie den Servicetyp, den Sie erstellt haben, und notieren Sie die Serviceinstanz **Name**.
   - **Cloud Foundry-Services:**
     ```
     ibmcloud service list
     ```
     {: pre}

     Beispielausgabe:
     ```
     name                         service           plan    bound apps   last operation
     <cf_serviceinstanzname>      <servicename>     spark                create succeeded
     ```
     {: screen}

  - **{{site.data.keyword.Bluemix_notm}}IAM-fähige Services:**
     ```
     ibmcloud resource service-instances
     ```
     {: pre}

     Beispielausgabe:
     ```
     Name                          Location   State    Type               Tags
     <iam-serviceinstanzname>      <region>   active   service_instance
     ```
     {: screen}

   Sie können die verschiedenen Servicetypen auch in Ihrem Dashboard als **Cloud Foundry Services** und **Services** sehen.

3. Geben Sie den Clusternamensbereich an, den Sie verwenden wollen, um Ihren Service hinzuzufügen. Wählen Sie eine der folgenden Optionen aus.
     ```
     kubectl get namespaces
     ```
     {: pre}

4.  Fügen Sie den Service Ihrem Cluster mit dem [Befehl](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_service_bind) `ibmcloud ks cluster-service-bind` hinzu. Stellen Sie bei {{site.data.keyword.Bluemix_notm}} IAM-fähigen Services sicher, dass Sie den Cloud Foundry-Aliasnamen verwenden, den Sie zuvor erstellt haben. Für IAM-fähige Services können Sie außerdem die Standardservicezugriffsrolle **Schreibberechtigter** (Writer) verwenden oder die Servicezugriffsrolle mit dem Flag `--role` angeben. Der Befehl erstellt einen Serviceschlüssel für die Serviceinstanz. Alternativ können Sie das Flag `--key` mit angeben, um vorhandene Berechtigungsnachweise für den Serviceschlüssel zu verwenden. Wenn Sie das Flag `--key` verwenden, geben Sie das Flag `--role` nicht an.
    ```
    ibmcloud ks cluster-service-bind --cluster <clustername_oder-id> --namespace <namensbereich> --service <serviceinstanzname> [--key <serviceinstanzschlüssel>] [--role <iam-servicerolle>]
    ```
    {: pre}

    Wenn der Service Ihrem Cluster erfolgreich hinzugefügt wurde, wird ein geheimer Schlüssel für den Cluster erstellt, der die Berechtigungsnachweise Ihrer Serviceinstanz enthält. Die geheimen Schlüssel werden automatisch in 'etcd' verschlüsselt, um Ihre Daten zu schützen.

    Beispielausgabe:
    ```
    ibmcloud ks cluster-service-bind --cluster mycluster --namespace mynamespace --service cleardb
    Binding service instance to namespace...
    OK
    Namespace:	     mynamespace
    Secret name:     binding-<serviceinstanzname>
    ```
    {: screen}

5.  Überprüfen Sie die Serviceberechtigungsnachweise in Ihrem geheimen Kubernetes-Schlüssel.
    1. Rufen Sie die Details des geheimen Schlüssels ab und notieren Sie den **binding**-Wert. Der **binding**-Wert ist Base64-codiert und enthält die Berechtigungsnachweise für Ihre Serviceinstanz im JSON-Format.
       ```
       kubectl get secrets binding-<serviceinstanzname> --namespace=<namensbereich> -o yaml
       ```
       {: pre}

       Beispielausgabe:
       ```
       apiVersion: v1
       data:
         binding: <bindung>
       kind: Secret
       metadata:
         annotations:
           service-instance-id: 1111aaaa-a1aa-1aa1-1a11-111aa111aa11
           service-key-id: 2b22bb2b-222b-2bb2-2b22-b22222bb2222
         creationTimestamp: 2018-08-07T20:47:14Z
         name: binding-<serviceinstanzname>
         namespace: <namensbereich>
         resourceVersion: "6145900"
         selfLink: /api/v1/namespaces/default/secrets/binding-mycloudant
         uid: 33333c33-3c33-33c3-cc33-cc33333333c
       type: Opaque
       ```
       {: screen}

    2. Decodieren Sie den 'bindung'-Wert.
       ```
       echo "<bindung>" | base64 -D
       ```
       {: pre}

       Beispielausgabe:
       ```
       {"apikey":"KL34Ys893284NGJEPFjgrioJ12NElpow","host":"98765aab-9ce1-7tr3-ba87-bfbab6e6d9d6-bluemix.cloudant.com","iam_apikey_description":"Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:cloudantnosqldb:us-south:a/1234g56789cfe8e6388dd2ec098:98746cw-43d7-49ce-947a-d8fe3eebb381::","iam_apikey_name":"auto-generated-apikey-1234abcde-987f-3t64-9d96-d13775ec5663","iam_role_crn":"crn:v1:bluemix:public:iam::::serviceRole:Writer","iam_serviceid_crn":"crn:v1:bluemix:public:iam-identity::a/1234567890brasge5htn2ec098::serviceid:ServiceId-12345vgh-6c4c-ytr12-af6b-467d30d6ef44","password":"jfiavhui12484fnivhuo472nvei23913c3ff","port":443,"url":"https://25c73aac-9ce1-4c24-ba98-bfbab6e6d9d6-bluemix:ugvioev823inreuiegn43donvri29989wiu9t22@25c73aac-9ce1-4c24-ba98-abdrjio123562lnsb-bluemix.cloudant.com","username":"123b45da-9ce1-4c24-ab12-rinwnwub1294-bluemix"}
       ```
       {: screen}

    3. Optional: Vergleichen Sie die Serviceberechtigungsnachweise, die Sie im vorherigen Schritt decodiert haben, mit den Serviceberechtigungsnachweisen, die Sie für Ihre Serviceinstanz im {{site.data.keyword.Bluemix_notm}}-Dashboard finden.

6. Ihr Service ist nun an Ihren Cluster gebunden und Sie müssen Ihre App für den [Zugriff auf die Serviceberechtigungsnachweise im geheimen Kubernetes-Schlüssel](#adding_app) konfigurieren.


## Über Apps auf Serviceberechtigungsnachweise zugreifen
{: #adding_app}

Um über Ihre App auf eine {{site.data.keyword.Bluemix_notm}}-Serviceinstanz zuzugreifen, müssen Sie die im geheimen Kubernetes-Schlüssel gespeicherten Serviceberechtigungsnachweise für Ihre App verfügbar machen.
{: shortdesc}

Die Berechtigungsnachweise einer Serviceinstanz sind Base64-codiert und werden in Ihrem geheimen Schlüssel im JSON-Format gespeichert. Wählen Sie eine der folgenden Optionen aus, um auf die Daten in Ihrem geheimen Schlüssel zuzugreifen:
- [Geheimen Schlüssel als Datenträger an Ihren Pod anhängen](#mount_secret)
- [Auf den geheimen Schlüssel in Umgebungsvariablen verweisen](#reference_secret)
<br>
Sollen die geheimen Schlüssel noch sicherer werden? Wenden Sie sich zur [Aktivierung von {{site.data.keyword.keymanagementservicefull}}](/docs/containers?topic=containers-encryption#keyprotect) im Cluster an Ihren Clusteradministrator, um neue und vorhandene geheime Schlüssel zu verschlüsseln, zum Beispiel den geheimen Schlüssel, in dem die Berechtigungsnachweise der {{site.data.keyword.Bluemix_notm}}-Serviceinstanzen enthalten sind.
{: tip}

Vorbereitende Schritte:
-  Stellen Sie sicher, dass Sie die [{{site.data.keyword.Bluemix_notm}} IAM-Servicerolle **Schreibberechtigter** oder **Manager**](/docs/containers?topic=containers-users#platform) für den Namensbereich `kube-system` innehaben.
- [Melden Sie sich an Ihrem Konto an. Geben Sie als Ziel die entsprechende Region und, sofern zutreffend, die Ressourcengruppe an. Legen Sie den Kontext für den Cluster fest.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
- [Fügen Sie einen {{site.data.keyword.Bluemix_notm}}-Service zu Ihrem Cluster hinzu](#adding_cluster).

### Geheimen Schlüssel als Datenträger an Ihren Pod anhängen
{: #mount_secret}

Wenn Sie den geheimen Schlüssel als Datenträger an Ihren Pod anhängen, wird eine Datei mit dem Namen `binding` im Mountverzeichnis des Datenträgers gespeichert. Die Datei `binding` im JSON-Format enthält sämtliche Informationen und Berechtigungsnachweise, die Sie benötigen, um auf den {{site.data.keyword.Bluemix_notm}}-Service zuzugreifen.
{: shortdesc}

1.  Listen Sie die verfügbaren geheimen Schlüssel in Ihrem Cluster auf und notieren Sie den **Namen** Ihres geheimen Schlüssels. Suchen Sie nach einem Schlüssel des Typs **Opaque**. Sollten mehrere geheime Schlüssel vorhanden sein, wenden Sie sich an Ihren Clusteradministrator, damit dieser den geheimen Schlüssel für den gewünschten Service ermittelt.

    ```
    kubectl get secrets
    ```
    {: pre}

    Beispielausgabe:

    ```
    NAME                              TYPE            DATA      AGE
    binding-<serviceinstanzname>      Opaque          1         3m

    ```
    {: screen}

2.  Erstellen Sie eine YAML-Datei für Ihre Kubernetes-Bereitstellung und hängen Sie den geheimen Schlüssel als Datenträger an Ihren Pod an.
    ```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      labels:
        app: secret-test
      name: secret-test
      namespace: <mein_namensbereich>
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
            - mountPath: <mountpfad>
              name: <datenträgername>
          volumes:
          - name: <datenträgername>
            secret:
              defaultMode: 420
              secretName: binding-<serviceinstanzname>
    ```
    {: codeblock}

    <table>
    <caption>Erklärung der Komponenten der YAML-Datei</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der YAML-Dateikomponenten</th>
    </thead>
    <tbody>
    <tr>
    <td><code>volumeMounts.mountPath</code></td>
    <td>Der absolute Pfad des Verzeichnisses, in dem der Datenträger innerhalb des Containers angehängt wird.</td>
    </tr>
    <tr>
    <td><code>volumeMounts.name</code></br><code>volumes.name</code></td>
    <td>Der Name des Datenträgers, der an Ihren Pod angehängt werden soll.</td>
    </tr>
    <tr>
    <td><code>secret.defaultMode</code></td>
    <td>Die Lese- und Schreibberechtigungen für den geheimen Schlüssel. Verwenden Sie `420`, um Leseberechtigungen festzulegen. </td>
    </tr>
    <tr>
    <td><code>secret.secretName</code></td>
    <td>Der Name des geheimen Schlüssels, den Sie im vorherigen Schritt notiert haben.</td>
    </tr></tbody></table>

3.  Erstellen Sie den Pod und hängen Sie den geheimen Schlüssel als Datenträger an.
    ```
    kubectl apply -f secret-test.yaml
    ```
    {: pre}

4.  Stellen Sie sicher, dass der Pod erstellt wurde.
    ```
    kubectl get pods
    ```
    {: pre}

    CLI-Beispielausgabe:

    ```
    NAME                           READY     STATUS    RESTARTS   AGE
    secret-test-1111454598-gfx32   1/1       Running   0          1m
    ```
    {: screen}

5.  Greifen Sie auf die Serviceberechtigungsnachweise zu.
    1. Melden Sie sich beim Pod an.
       ```
       kubectl exec <podname> -it bash
       ```
       {: pre}

    2. Navigieren Sie zu Ihrem Datenträgermountpfad, den Sie zuvor definiert haben, und listen Sie die Dateien in Ihrem Datenträgermountpfad auf.
       ```
       cd <datenträger-mountpfad> && ls
       ```
       {: pre}

       Beispielausgabe:
       ```
       binding
       ```
       {: screen}

       Die `binding`-Datei enthält die Serviceberechtigungsnachweise, die Sie im geheimen Kubernetes-Schlüssel gespeichert haben.

    4. Zeigen Sie die Serviceberechtigungsnachweise an. Die Berechtigungsnachweise werden als Schlüssel/Wert-Paare im JSON-Format gespeichert.
       ```
       cat binding
       ```
       {: pre}

       Beispielausgabe:
       ```
       {"apikey":"KL34Ys893284NGJEPFjgrioJ12NElpow","host":"98765aab-9ce1-7tr3-ba87-bfbab6e6d9d6-bluemix.cloudant.com","iam_apikey_description":"Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:cloudantnosqldb:us-south:a/1234g56789cfe8e6388dd2ec098:98746cw-43d7-49ce-947a-d8fe3eebb381::","iam_apikey_name":"auto-generated-apikey-1234abcde-987f-3t64-9d96-d13775ec5663","iam_role_crn":"crn:v1:bluemix:public:iam::::serviceRole:Writer","iam_serviceid_crn":"crn:v1:bluemix:public:iam-identity::a/1234567890brasge5htn2ec098::serviceid:ServiceId-12345vgh-6c4c-ytr12-af6b-467d30d6ef44","password":"jfiavhui12484fnivhuo472nvei23913c3ff","port":443,"url":"https://25c73aac-9ce1-4c24-ba98-bfbab6e6d9d6-bluemix:ugvioev823inreuiegn43donvri29989wiu9t22@25c73aac-9ce1-4c24-ba98-abdrjio123562lnsb-bluemix.cloudant.com","username":"123b45da-9ce1-4c24-ab12-rinwnwub1294-bluemix"}
       ```
       {: screen}

    5. Konfigurieren Sie Ihre App, um den JSON-Inhalt zu parsen und die Informationen abzurufen, die Sie für den Zugriff auf den Service benötigen.


### Auf den geheimen Schlüssel in Umgebungsvariablen verweisen
{: #reference_secret}

Sie können die Serviceberechtigungsnachweise und andere Schlüssel/Wert-Paare aus Ihrem geheimen Kubernetes-Schlüssel als Umgebungsvariablen zur Ihrer Bereitstellung hinzufügen.
{: shortdesc}

1. Listen Sie die verfügbaren geheimen Schlüssel in Ihrem Cluster auf und notieren Sie den **Namen** Ihres geheimen Schlüssels. Suchen Sie nach einem Schlüssel des Typs **Opaque**. Sollten mehrere geheime Schlüssel vorhanden sein, wenden Sie sich an Ihren Clusteradministrator, damit dieser den geheimen Schlüssel für den gewünschten Service ermittelt.

    ```
    kubectl get secrets
    ```
    {: pre}

    Beispielausgabe:

    ```
    NAME                              TYPE            DATA      AGE
    binding-<serviceinstanzname>      Opaque          1         3m
    ```
    {: screen}

2. Rufen Sie die Details Ihres geheimen Schlüssels ab, um potenzielle Schlüssel/Wert-Paare zu finden, auf die Sie als Umgebungsvariablen in Ihrem Pod verweisen können. Die Serviceberechtigungsnachweise werden im Schlüssel `bindung` Ihres geheimen Schlüssels gespeichert.
   ```
   kubectl get secrets binding-<serviceinstanzname> --namespace=<namensbereich> -o yaml
   ```
   {: pre}

   Beispielausgabe:
   ```
   apiVersion: v1
   data:
     binding: <bindung>
   kind: Secret
   metadata:
     annotations:
       service-instance-id: 7123acde-c3ef-4ba2-8c52-439ac007fa70
       service-key-id: 9h30dh8a-023f-4cf4-9d96-d12345ec7890
     creationTimestamp: 2018-08-07T20:47:14Z
     name: binding-<serviceinstanzname>
     namespace: <namensbereich>
     resourceVersion: "6145900"
     selfLink: /api/v1/namespaces/default/secrets/binding-mycloudant
     uid: 12345a31-9a83-11e8-ba83-cd49014748f
   type: Opaque
   ```
   {: screen}

3. Erstellen Sie eine YAML-Datei für Ihre Kubernetes-Bereitstellung und geben Sie eine Umgebungsvariable an, die auf den Schlüssel `binding` verweist.
   ```
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     labels:
       app: secret-test
     name: secret-test
     namespace: <mein_namensbereich>
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
                 name: binding-<serviceinstanzname>
                 key: binding
     ```
     {: codeblock}

     <table>
     <caption>Erklärung der Komponenten der YAML-Datei</caption>
     <thead>
     <th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der YAML-Dateikomponenten</th>
     </thead>
     <tbody>
     <tr>
     <td><code>containers.env.name</code></td>
     <td>Der Name Ihrer Umgebungsvariablen.</td>
     </tr>
     <tr>
     <td><code>env.valueFrom.secretKeyRef.name</code></td>
     <td>Der Name des geheimen Schlüssels, den Sie im vorherigen Schritt notiert haben.</td>
     </tr>
     <tr>
     <td><code>env.valueFrom.secretKeyRef.key</code></td>
     <td>Der Schlüssel, der Teil Ihres geheimen Schlüssels ist und auf den Sie in Ihrer Umgebungsvariablen verweisen möchten. Wenn Sie auf die Serviceberechtigungsnachweise verweisen möchten, müssen Sie den Schlüssel <strong>binding</strong> verwenden.  </td>
     </tr>
     </tbody></table>

4. Erstellen Sie den Pod, der auf den Schlüssel `binding` des geheimen Schlüssels als Umgebungsvariable verweist.
   ```
   kubectl apply -f secret-test.yaml
   ```
   {: pre}

5. Stellen Sie sicher, dass der Pod erstellt wurde.
   ```
   kubectl get pods
   ```
   {: pre}

   CLI-Beispielausgabe:
   ```
   NAME                           READY     STATUS    RESTARTS   AGE
    secret-test-1111454598-gfx32   1/1       Running   0          1m
   ```
   {: screen}

6. Stellen Sie sicher, dass die Umgebungsvariable richtig festgelegt ist.
   1. Melden Sie sich beim Pod an.
      ```
      kubectl exec <podname> -it bash
      ```
      {: pre}

   2. Listen Sie alle Umgebungsvariablen im Pod auf.
      ```
      env
      ```
      {: pre}

      Beispielausgabe:
      ```
      BINDING={"apikey":"KL34Ys893284NGJEPFjgrioJ12NElpow","host":"98765aab-9ce1-7tr3-ba87-bfbab6e6d9d6-bluemix.cloudant.com","iam_apikey_description":"Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:cloudantnosqldb:us-south:a/1234g56789cfe8e6388dd2ec098:98746cw-43d7-49ce-947a-d8fe3eebb381::","iam_apikey_name":"auto-generated-apikey-1234abcde-987f-3t64-9d96-d13775ec5663","iam_role_crn":"crn:v1:bluemix:public:iam::::serviceRole:Writer","iam_serviceid_crn":"crn:v1:bluemix:public:iam-identity::a/1234567890brasge5htn2ec098::serviceid:ServiceId-12345vgh-6c4c-ytr12-af6b-467d30d6ef44","password":"jfiavhui12484fnivhuo472nvei23913c3ff","port":443,"url":"https://25c73aac-9ce1-4c24-ba98-bfbab6e6d9d6-bluemix:ugvioev823inreuiegn43donvri29989wiu9t22@25c73aac-9ce1-4c24-ba98-abdrjio123562lnsb-bluemix.cloudant.com","username":"123b45da-9ce1-4c24-ab12-rinwnwub1294-bluemix"}
      ```
      {: screen}

7. Konfigurieren Sie Ihre App zum Lesen der Umgebungsvariablen und zum Parsen des JSON-Inhalts, um die Informationen abzurufen, die Sie für den Zugriff auf den Service benötigen.

   Beispielcode in Python:
   ```
   if os.environ.get('BINDING'):
        credentials = json.loads(os.environ.get('BINDING'))
   ```
   {: codeblock}

## Helm in {{site.data.keyword.containerlong_notm}} konfigurieren
{: #helm}

[Helm ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://helm.sh) ist ein Kubernetes-Paketmanager. Sie können Helm-Diagramme zum Definieren, Installieren und Durchführen von Upgrades für komplexe Kubernetes-Anwendungen, die in {{site.data.keyword.containerlong_notm}}-Clustern ausgeführt werden, erstellen.
{:shortdesc}

Zum Bereitstellen von Helm-Diagrammen müssen Sie die Helm-CLI auf Ihrer lokalen Maschine installieren und den Helm-Server Tiller in Ihrem Cluster installieren. Das Image für Tiller ist in der öffentlichen Google-Container-Registry gespeichert. Für den Zugriff auf das Image während der Tiller-Installation muss Ihr Cluster öffentliche Netzkonnektivität zur öffentlichen Google-Container-Registry zulassen. Cluster, für die der öffentliche Serviceendpunkt aktiviert ist, können automatisch auf das Image zugreifen. Private Cluster, die durch eine angepasste Firewall geschützt werden, oder Cluster, für die nur der private Serviceendpunkt aktiviert ist, lassen keinen Zugriff auf das Tiller-Image zu. Stattdessen können Sie das [Image auf Ihre lokale Maschine extrahieren und durch eine Push-Operation in Ihren Namensbereich in {{site.data.keyword.registryshort_notm}} übertragen](#private_local_tiller) oder [Helm-Diagramme ohne Tiller installieren](#private_install_without_tiller).
{: note}

### Helm in einem Cluster mit öffentlichem Zugriff einrichten
{: #public_helm_install}

Wenn für Ihren Cluster der öffentliche Serviceendpunkt aktiviert ist, können Sie Tiller mithilfe des öffentlichen Image in der Google-Container-Registry installieren.
{: shortdesc}

Vorbereitende Schritte: [Melden Sie sich an Ihrem Konto an. Geben Sie als Ziel die entsprechende Region und, sofern zutreffend, die Ressourcengruppe an. Legen Sie den Kontext für den Cluster fest.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1. Installieren Sie die <a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">Helm-CLI <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a>.

2. **Wichtig**: Erstellen Sie zur Gewährleistung der Clustersicherheit ein Servicekonto für Tiller im Namensbereich `kube-system` und eine RBAC-Clusterrollenbindung für Kubernetes für den Pod `tiller-deploy`; verwenden Sie hierzu die folgende `.yaml`-Datei aus dem [{{site.data.keyword.Bluemix_notm}}-Repository `kube-samples`](https://github.com/IBM-Cloud/kube-samples/blob/master/rbac/serviceaccount-tiller.yaml). **Hinweis:** Wenn Sie Tiller mit dem Servicekonto und der Clusterrollenbindung im Namensbereich `kube-system` installieren möchten, müssen Sie über die [Rolle `cluster-admin` verfügen](/docs/containers?topic=containers-users#access_policies).
    ```
    kubectl apply -f https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/rbac/serviceaccount-tiller.yaml
    ```
    {: pre}

3. Initialisieren Sie Helm und installieren Sie Tiller mit dem erstellten Servicekonto.

    ```
    helm init --service-account tiller
    ```
    {: pre}

4.  Überprüfen Sie, ob die Installation erfolgreich war.
    1.  Überprüfen Sie, ob das Tiller-Servicekonto erstellt wurde.
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

    2.  Überprüfen Sie, ob der Pod `tiller-deploy` in Ihrem Cluster den **Status** `Running` aufweist.
        ```
        kubectl get pods -n kube-system -l app=helm
        ```
        {: pre}

        Beispielausgabe:

        ```
        NAME                            READY     STATUS    RESTARTS   AGE
        tiller-deploy-352283156-nzbcm   1/1       Running   0          2m
        ```
        {: screen}

5. Fügen Sie Ihrer Helm-Instanz die {{site.data.keyword.Bluemix_notm}}-Helm-Repositorys hinzu.
   ```
   helm repo add ibm  https://registry.bluemix.net/helm/ibm
   ```
   {: pre}

   ```
   helm repo add ibm-charts https://registry.bluemix.net/helm/ibm-charts
   ```
   {: pre}

6. Aktualisieren Sie die Repositorys, um die neuesten Versionen aller Helm-Diagramme abzurufen.
   ```
   helm repo update
   ```
   {: pre}

7. Listen Sie die aktuell in den {{site.data.keyword.Bluemix_notm}}-Repositorys verfügbaren Helm-Diagramme auf.
   ```
   helm search ibm
   ```
   {: pre}

   ```
   helm search ibm-charts
   ```
   {: pre}

8. Ermitteln Sie das Helm-Diagramm, das Sie installieren wollen, und befolgen Sie die Anweisungen in der Datei `README` des Helm-Diagramms, um es in Ihrem Cluster zu installieren.


### Private Cluster: Tiller-Image durch Push-Operation in die private Registry in {{site.data.keyword.registryshort_notm}} übertragen
{: #private_local_tiller}

Sie können das Tiller-Image auf Ihre lokale Maschine extrahieren, in Ihren Namensbereich in {{site.data.keyword.registryshort_notm}} durch eine Push-Operation übertragen und Helm die Installation von Tiller mit dem Image in {{site.data.keyword.registryshort_notm}} durchführen lassen.
{: shortdesc}

Vorbereitende Schritte:
- Installieren Sie Docker auf Ihrer lokalen Maschine. Wenn Sie die [{{site.data.keyword.Bluemix_notm}}-CLI](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli) installiert haben, ist Docker bereits installiert.
- [Installieren Sie das {{site.data.keyword.registryshort_notm}}-CLI-Plug-in und richten Sie einen Namensbereich ein](/docs/services/Registry?topic=registry-getting-started#gs_registry_cli_install).

Gehen Sie wie folgt vor, um Tiller mithilfe von {{site.data.keyword.registryshort_notm}} zu installieren:

1. Installieren Sie die <a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">Helm-CLI <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a> auf Ihrer lokalen Maschine.
2. Stellen Sie eine Verbindung zu Ihrem privaten Cluster durch den VPN-Tunnel für die {{site.data.keyword.Bluemix_notm}}-Infrastruktur her, den Sie eingerichtet haben.
3. **Wichtig**: Erstellen Sie zur Gewährleistung der Clustersicherheit ein Servicekonto für Tiller im Namensbereich `kube-system` und eine RBAC-Clusterrollenbindung für Kubernetes für den Pod `tiller-deploy`; verwenden Sie hierzu die folgende `.yaml`-Datei aus dem [{{site.data.keyword.Bluemix_notm}}-Repository `kube-samples`](https://github.com/IBM-Cloud/kube-samples/blob/master/rbac/serviceaccount-tiller.yaml). **Hinweis:** Wenn Sie Tiller mit dem Servicekonto und der Clusterrollenbindung im Namensbereich `kube-system` installieren möchten, müssen Sie über die [Rolle `cluster-admin` verfügen](/docs/containers?topic=containers-users#access_policies).
    1. [Rufen Sie die YAML-Dateien für das Kubernetes-Servicekonto und die Clusterrollenbindung ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link") ab](https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/rbac/serviceaccount-tiller.yaml).

    2. Erstellen Sie Kubernetes-Ressourcen in Ihrem Cluster.
       ```
       kubectl apply -f service-account.yaml
       ```
       {: pre}

       ```
       kubectl apply -f cluster-role-binding.yaml
       ```
       {: pre}

4. [Suchen Sie die Version von Tiller ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://console.cloud.google.com/gcr/images/kubernetes-helm/GLOBAL/tiller?gcrImageListsize=30), die Sie in Ihrem Cluster installieren wollen. Wenn Sie keine bestimmte Version benötigen, verwenden Sie die neueste Version.

5. Extrahieren Sie das Tiller-Image durch eine Pull-Operation auf Ihre lokale Maschine.
   ```
   docker pull gcr.io/kubernetes-helm/tiller:v2.11.0
   ```
   {: pre}

   Beispielausgabe:
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

6. [Übertragen Sie das Tiller-Image durch eine Push-Operation in {{site.data.keyword.registryshort_notm}}](/docs/services/Registry?topic=registry-getting-started#gs_registry_images_pushing).

7. [Kopieren Sie den geheimen Schlüssel für Image-Pull-Operationen für den Zugriff auf {{site.data.keyword.registryshort_notm}} aus dem Standardnamensbereich in den Namensbereich `kube-system`](/docs/containers?topic=containers-images#copy_imagePullSecret).

8. Installieren Sie Tiller in Ihrem privaten Cluster, indem Sie das Image verwenden, das Sie in Ihrem Namensbereich in {{site.data.keyword.registryshort_notm}} gespeichert haben.
   ```
   helm init --tiller-image <region>.icr.io/<mein_namensbereich>/<mein_image>:<tag> --service-account tiller
   ```
   {: pre}

9. Fügen Sie Ihrer Helm-Instanz die {{site.data.keyword.Bluemix_notm}}-Helm-Repositorys hinzu.
   ```
   helm repo add ibm  https://registry.bluemix.net/helm/ibm
   ```
   {: pre}

   ```
   helm repo add ibm-charts https://registry.bluemix.net/helm/ibm-charts
   ```
   {: pre}

10. Aktualisieren Sie die Repositorys, um die neuesten Versionen aller Helm-Diagramme abzurufen.
    ```
    helm repo update
    ```
    {: pre}

11. Listen Sie die aktuell in den {{site.data.keyword.Bluemix_notm}}-Repositorys verfügbaren Helm-Diagramme auf.
    ```
    helm search ibm
    ```
    {: pre}

    ```
    helm search ibm-charts
    ```
    {: pre}

12. Ermitteln Sie das Helm-Diagramm, das Sie installieren wollen, und befolgen Sie die Anweisungen in der Datei `README` des Helm-Diagramms, um es in Ihrem Cluster zu installieren.

### Private Cluster: Helm-Diagramme ohne Tiller installieren
{: #private_install_without_tiller}

Wenn Sie Tiller in Ihrem privaten Cluster nicht installieren wollen, können Sie die YAML-Dateien des Helm-Diagramms manuell erstellen und mithilfe von `kubectl`-Befehlen anwenden.
{: shortdesc}

Die Schritte in diesem Beispiel demonstrieren, wie Helm-Diagramme aus den Repositorys für {{site.data.keyword.Bluemix_notm}}-Helm-Diagramme in Ihrem privaten Cluster installiert werden. Wenn Sie ein Helm-Diagramm installieren wollen, das nicht in einem der Repositorys für {{site.data.keyword.Bluemix_notm}}-Helm-Diagramme gespeichert ist, müssen Sie die Anweisungen in diesem Abschnitt ausführen, um die YAML-Dateien für Ihr Helm-Diagramm zu erstellen. Darüber hinaus müssen Sie das Helm-Diagramm-Image aus der öffentlichen Container-Registry herunterladen, in Ihren Namensbereich in {{site.data.keyword.registryshort_notm}} durch eine Push-Operation übertragen und die Datei `values.yaml` aktualisieren, um das Image in {{site.data.keyword.registryshort_notm}} zu verwenden.
{: note}

1. Installieren Sie die <a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">Helm-CLI <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a> auf Ihrer lokalen Maschine.
2. Stellen Sie eine Verbindung zu Ihrem privaten Cluster durch den VPN-Tunnel für die {{site.data.keyword.Bluemix_notm}}-Infrastruktur her, den Sie eingerichtet haben.
3. Fügen Sie Ihrer Helm-Instanz die {{site.data.keyword.Bluemix_notm}}-Helm-Repositorys hinzu.
   ```
   helm repo add ibm  https://registry.bluemix.net/helm/ibm
   ```
   {: pre}

   ```
   helm repo add ibm-charts https://registry.bluemix.net/helm/ibm-charts
   ```
   {: pre}

4. Aktualisieren Sie die Repositorys, um die neuesten Versionen aller Helm-Diagramme abzurufen.
   ```
   helm repo update
   ```
   {: pre}

5. Listen Sie die aktuell in den {{site.data.keyword.Bluemix_notm}}-Repositorys verfügbaren Helm-Diagramme auf.
   ```
   helm search ibm
   ```
   {: pre}

   ```
   helm search ibm-charts
   ```
   {: pre}

6. Ermitteln Sie das Helm-Diagramm, das Sie installieren wollen, laden Sie das Helm-Diagramm auf Ihre lokale Maschine herunter und entpacken Sie die Dateien Ihres Helm-Diagramms. Das folgende Beispiel zeigt, wie das Helm-Diagramm für den Cluster-Autoscaler Version 1.0.3 heruntergeladen und die Dateien in einem Verzeichnis `cluster-autoscaler` entpackt werden.
   ```
   helm fetch ibm/ibm-iks-cluster-autoscaler --untar --untardir ./cluster-autoscaler --version 1.0.3
   ```
   {: pre}

7. Wechseln Sie in das Verzeichnis, in dem Sie die Dateien des Helm-Diagramms entpackt haben.
   ```
   cd cluster-autoscaler
   ```
   {: pre}

8. Erstellen Sie ein Verzeichnis `output` für die YAML-Dateien, die Sie mithilfe der Dateien in Ihrem Helm-Diagramm generieren.
   ```
   mkdir output
   ```
   {: pre}

9. Öffnen Sie die Datei `values.yaml` und nehmen Sie die Änderungen vor, die entsprechend den Installationsanweisungen für das Helm-Diagramm erforderlich sind.
   ```
   nano ibm-iks-cluster-autoscaler/values.yaml
   ```
   {: pre}

10. Verwenden Sie Ihre lokale Helm-Installation, um alle Kubernetes-YAML-Dateien  für Ihr Helm-Diagramm zu erstellen. Die YAML-Dateien werden im Verzeichnis `output` gespeichert, das Sie zuvor erstellt haben.
    ```
    helm template --values ./ibm-iks-cluster-autoscaler/values.yaml --output-dir ./output ./ibm-iks-cluster-autoscaler
    ```
    {: pre}

    Beispielausgabe:
    ```
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-configmap.yaml
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-service-account-roles.yaml
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-service.yaml
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-deployment.yaml
    ```
    {: screen}

11. Stellen Sie alle YAML-Dateien in Ihrem privaten Cluster bereit.
    ```
    kubectl apply --recursive --filename ./output
    ```
   {: pre}

12. Optional: Entfernen Sie alle YAML-Dateien aus dem Verzeichnis `output`.
    ```
    kubectl delete --recursive --filename ./output
    ```
    {: pre}


### Zugehörige Helm-Links
{: #helm_links}

* Informationen zur Verwendung von StrongSwan finden Sie unter [Einrichtung von VPN-Konnektivität mit dem Helm-Diagramm des StrongSwan-IPSec-VPN-Service](/docs/containers?topic=containers-vpn#vpn-setup).
* Zeigen Sie verfügbare Helm-Diagramme an, die Sie mit {{site.data.keyword.Bluemix_notm}} im [Helm-Diagrammkatalog ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/kubernetes/solutions/helm-charts) in der Konsole verwenden können.
* Weitere Informationen zu den Helm-Befehlen zum Konfigurieren und Verwalten von Helm-Diagrammen finden Sie in der <a href="https://docs.helm.sh/helm/" target="_blank">Helm-Dokumentation <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a>.
* Über den folgenden Link finden Sie weitere Informationen zur Vorgehensweise bei der [Erhöhung der Bereitstellungsgeschwindigkeit mit Kubernetes-Helm-Diagrammen![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://developer.ibm.com/recipes/tutorials/increase-deployment-velocity-with-kubernetes-helm-charts/).

## Kubernetes-Clusterressourcen grafisch darstellen
{: #weavescope}

Weave Scope liefert eine grafisch orientierte Diagrammdarstellung Ihrer Ressourcen in einem Kubernetes-Cluster unter Einbeziehung von Services, Pods, Containern und vielem mehr. Weave Scope stellt interaktive Metriken für CPU und Speicher bereit und bietet Tools, um 'tail'- und 'exec'-Aufrufe in einem Container durchzuführen.
{:shortdesc}

Vorbereitende Schritte:

-   Denken Sie daran, dass Sie keine Clusterinformationen im öffentlichen Internet offenlegen dürfen. Führen Sie diese Schritte aus, um Weave Scope sicher bereitzustellen und dann lokal über einen Web-Browser auf den Service zuzugreifen.
-   Wenn Sie nicht bereits über einen verfügen, [erstellen Sie einen Standardcluster](/docs/containers?topic=containers-clusters#clusters_ui). Weave Scope kann die CPU stark belasten, insbesondere die App. Führen Sie Weave Scope mit den größeren Standardclustern aus und nicht mit kostenlosen Clustern.
-  Stellen Sie sicher, dass Sie über die [{{site.data.keyword.Bluemix_notm}} IAM-Servicerolle **Manager**](/docs/containers?topic=containers-users#platform) für alle Namensbereiche verfügen.
-   [Melden Sie sich an Ihrem Konto an. Geben Sie als Ziel die entsprechende Region und, sofern zutreffend, die Ressourcengruppe an. Legen Sie den Kontext für den Cluster fest.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)


Gehen Sie wie folgt vor, um Weave Scope mit einem Cluster zu verwenden:
2.  Stellen Sie eine der zur Verfügung gestellten Konfigurationsdatei für die RBAC-Berechtigungen im Cluster bereit.

    Gehen Sie wie folgt vor, um die Schreib-/Leseberechtigung zu aktivieren:

    ```
    kubectl apply -f "https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/weave-scope/weave-scope-rbac.yaml"
    ```
    {: pre}

    Gehen Sie wie folgt vor, um die Schreibberechtigung zu aktivieren:

    ```
    kubectl apply --namespace weave -f "https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/weave-scope/weave-scope-rbac-readonly.yaml"
    ```
    {: pre}

    Ausgabe:

    ```
    clusterrole "weave-scope-mgr" created
    clusterrolebinding "weave-scope-mgr-role-binding" created
    ```
    {: screen}

3.  Stellen Sie den Weave Scope-Service bereit, auf den über die IP-Adresse des Clusters privat zugegriffen werden kann.

    <pre class="pre">
    <code>kubectl apply --namespace weave -f "https://cloud.weave.works/k8s/scope.yaml?k8s-version=$(kubectl version | base64 | tr -d '&bsol;n')"</code>
    </pre>

    Ausgabe:

    ```
    serviceaccount "weave-scope" created
    deployment "weave-scope-app" created
    service "weave-scope-app" created
    daemonset "weave-scope-agent" created
    ```
    {: screen}

4.  Führen Sie einen Befehl zur Portweiterleitung aus, um den Service auf dem Computer zu öffnen. Beim nächsten Zugriff auf Weave Scope können Sie diesen Befehl zur Portweiterleitung ausführen, ohne wieder die vorherigen Konfigurationsschritte durchführen zu müssen.

    ```
    kubectl port-forward -n weave "$(kubectl get -n weave pod --selector=weave-scope-component=app -o jsonpath='{.items..metadata.name}')" 4040
    ```
    {: pre}

    Ausgabe:

    ```
    Forwarding from 127.0.0.1:4040 -> 4040
    Forwarding from [::1]: :1]:4040 -> 4040
    Handling connection for 4040
    ```
    {: screen}

5.  Öffnen Sie Ihren Browser mit der Adresse `http://localhost:4040`. Ohne die Bereitstellung der Standardkomponenten wird das folgende Diagramm angezeigt. Sie können entweder Topologiediagramme oder Tabellen in den Kubernetes-Ressourcen im Cluster anzeigen.

     <img src="images/weave_scope.png" alt="Beispieltopologie von Weave Scope" style="width:357px;" />


[Weitere Informationen zu den Weave Scope-Funktionen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.weave.works/docs/scope/latest/features/).

<br />

