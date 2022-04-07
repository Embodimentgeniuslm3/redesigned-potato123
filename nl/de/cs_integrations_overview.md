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


# Unterstützte Integrationen für IBM Cloud und Drittanbieter
{: #supported_integrations}

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
  <td>[{{site.data.keyword.Bluemix_notm}} File Storage](docs/infrastructure/FileStorage?topic=FileStorage-getting-started#getting-started) ist ein persistenter, schneller und flexibler, über ein Netz angeschlossener NFS-basierter Dateispeicher, den Sie Ihren Apps durch persistente Kubernetes-Datenträger hinzufügen können. Sie können unter vordefinierten Speichertiers mit GB-Größen und E/A-Operationen pro Sekunde (IOPS) wählen, die die Anforderungen Ihrer Workloads erfüllen. Weitere Informationen zur Bereitstellung von Dateispeicher in einem Cluster finden Sie unter [Daten in {{site.data.keyword.Bluemix_notm}}-Dateispeicher speichern](/docs/containers?topic=containers-file_storage#file_storage).</td>
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
