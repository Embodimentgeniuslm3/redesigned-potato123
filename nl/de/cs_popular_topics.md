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



# Beliebte Themen für {{site.data.keyword.containerlong_notm}}
{: #cs_popular_topics}

Bleiben Sie auf dem neuesten Stand bei den Entwicklungen für {{site.data.keyword.containerlong}}. Erfahren Sie mehr zu neuen Features, zu verschiedenen Tricks und einigen beliebten Themen, die andere Entwickler aktuell nützlich finden.
{:shortdesc}



## Beliebte Themen im April 2019
{: #apr19}

<table summary="Die Tabelle zeigt beliebte Themen. Die Zeilen sind von links nach rechts zu lesen. In Spalte 1 finden Sie das Datum, den Titel der Funktion in Zeile 2 und eine Beschreibung in Zeile 3.">
<caption>Beliebte Themen im Bereich Container und Kubernetes-Cluster im April 2019</caption>
<thead>
<th>Datum</th>
<th>Titel</th>
<th>Beschreibung</th>
</thead>
<tbody>
<tr>
<td>15. April 2019</td>
<td>[Hostnamen für Netzlastausgleichsfunktion (NLB) registrieren](/docs/containers?topic=containers-loadbalancer#loadbalancer_hostname)</td>
<td>Nachdem Sie öffentliche Netzlastausgleichsfunktionen (NLBs) eingerichtet haben, um Ihre Apps dem Internet zugänglich zu machen, können Sie jetzt DNS-Einträge für die NLB-IPs erstellen, indem Sie Hostnamen erstellen. {{site.data.keyword.Bluemix_notm}} übernimmt für Sie die Generierung und Pflege des Platzhalter-SSL-Zertifikats für den Hostnamen. Sie können auch TCP/HTTP(S)-Monitore einrichten, die den Status der NLB-IP-Adressen hinter den einzelnen Hostnamen überprüfen.</td>
</tr>
<tr>
<td>8. April 2019</td>
<td>[Kubernetes-Terminal in Ihrem Web-Browser (Beta)](/docs/containers?topic=containers-cs_cli_install#cli_web)</td>
<td>Wenn Sie Ihre Cluster mit dem Cluster-Dashboard in der {{site.data.keyword.Bluemix_notm}}-Konsole verwalten, aber recht bald zu fortgeschritteneren Konfigurationsänderungen übergehen möchten, können Sie CLI-Befehle jetzt direkt über Ihren Webbrowser im Kubernetes-Terminal ausführen. Starten Sie auf der Detailseite für einen Cluster das Kubernetes-Terminal, indem Sie auf die Schaltfläche **Terminal** klicken. Beachten Sie, dass das Kubernetes-Terminal als Beta-Add-on freigegeben wird und nicht für die Verwendung in Produktionsclustern vorgesehen ist.</td>
</tr>
<tr>
<td>4. April 2019</td>
<td>[Hoch verfügbare Master jetzt in Sydney](/docs/containers?topic=containers-regions-and-zones#zones)</td>
<td>Wenn Sie an einem Standort in einer Mehrzonen-Metropole, jetzt auch einschließlich Sydney, [einen Cluster erstellen](/docs/containers?topic=containers-clusters#clusters_ui), werden die Replikate des Kubernetes-Masters zwecks Hochverfügbarkeit über mehrere Zonen verteilt.</td>
</tr>
</tbody></table>

## Beliebte Themen im März 2019
{: #mar19}

<table summary="Die Tabelle zeigt beliebte Themen. Die Zeilen sind von links nach rechts zu lesen. In Spalte 1 finden Sie das Datum, den Titel der Funktion in Zeile 2 und eine Beschreibung in Zeile 3.">
<caption>Beliebte Themen im Bereich Container und Kubernetes-Cluster im März 2019</caption>
<thead>
<th>Datum</th>
<th>Titel</th>
<th>Beschreibung</th>
</thead>
<tbody>
<tr>
<td>21. März 2019</td>
<td>Einführung privater Serviceendpunkte für Ihren Kubernetes-Cluster-Master</td>
<td>{{site.data.keyword.containerlong_notm}} richtet Ihren Cluster standardmäßig mit Zugriff auf ein öffentliches VLAN und ein privates VLAN ein. Zuvor mussten Sie, wenn Sie einen [Cluster nur mit einem privaten VLAN](/docs/containers?topic=containers-plan_clusters#private_clusters) haben wollten, eine Gateway-Appliance einrichten, um die Verbindung der Workerknoten des Clusters mit dem Master herzustellen. Jetzt können Sie den privaten Serviceendpunkt verwenden. Wenn der private Serviceendpunkt aktiviert ist, fließt der gesamte Datenverkehr zwischen den Workerknoten und dem Master über das private Netz, ohne dass eine Gateway-Appliance erforderlich ist. Neben dieser zusätzlichen Sicherheit ist ein- und ausgehender Datenverkehr im privaten Netz außerdem [unbegrenzt und gebührenfrei ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/cloud/bandwidth). Sie können trotzdem einen öffentlichen Serviceendpunkt für den sicheren Zugriff auf Ihren Kubernetes-Master über das Internet behalten, um zum Beispiel `kubectl`-Befehle ausführen zu können, ohne sich dabei im privaten Netz zu befinden.<br><br>
Zur Verwendung privater Serviceendpunkte müssen Sie die [VRF-Funktion](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) und [Serviceendpunkte](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started) für Ihr Konto für die IBM Cloud-Infrastruktur (SoftLayer) aktivieren. Ihr Cluster muss Kubernetes Version 1.11 oder höher ausführen. Wenn Ihr Cluster eine frühere Kubernetes-Version ausführt, [führen Sie eine Aktualisierung auf mindestens Version 1.11 durch](/docs/containers?topic=containers-update#update). Weitere Informationen finden Sie über die folgenden Links:<ul>
<li>[Kommunikation zwischen Workerknoten und Master mit Serviceendpunkten](/docs/containers?topic=containers-cs_network_ov#cs_network_ov_master)</li>
<li>[Privaten Serviceendpunkt einrichten](/docs/containers?topic=containers-cs_network_cluster#set-up-private-se)</li>
<li>[Von öffentlichem zu privatem Serviceendpunkt wechseln](/docs/containers?topic=containers-cs_network_cluster#migrate-to-private-se)</li>
<li>Wenn Sie eine Firewall im privaten Netz haben: [Private IP-Adressen für {{site.data.keyword.containerlong_notm}}, {{site.data.keyword.registrylong_notm}} und andere {{site.data.keyword.Bluemix_notm}}-Services hinzufügen](/docs/containers?topic=containers-firewall#firewall_outbound)</li>
</ul>
<p class="important">Wenn Sie zu einem Cluster nur mit einem privaten Serviceendpunkt wechseln, stellen Sie sicher, dass Ihr Cluster weiterhin mit anderen {{site.data.keyword.Bluemix_notm}}-Services, die Sie verwenden, kommunizieren kann. [Portworx-SDS-Speicher](/docs/containers?topic=containers-portworx#portworx) und [Cluster-Autoscaler](/docs/containers?topic=containers-ca#ca) unterstützen ausschließlich private Serviceendpunkte nicht. Verwenden Sie stattdessen einen Cluster mit öffentlichen und privaten Serviceendpunkten. [NFS-basierter Dateispeicher](/docs/containers?topic=containers-file_storage#file_storage) wird unterstützt, wenn auf Ihrem Cluster Kubernetes Version 1.13.4_1513, 1.12.6_1544, 1.11.8_1550, 1.10.13_1551 oder höher ausgeführt wird.</p>
</td>
</tr>
<tr>
<td>7. März 2019</td>
<td>[Cluster-Autoscaler wechselt von Beta-Phase zu allgemeiner Verfügbarkeit (GA)](/docs/containers?topic=containers-ca#ca)</td>
<td>Der Cluster-Autoscaler ist jetzt allgemein verfügbar. Installieren Sie das Helm-Plug-in und beginnen Sie mit der automatischen Skalierung der Worker-Pools in Ihrem Cluster, um die Anzahl von Workerknoten entsprechend den Kapazitätsanforderungen Ihrer geplanten Workloads zu erhöhen oder zu verringern.<br><br>
Benötigen Sie Hilfe oder wünschen Sie Feedback zum Cluster-Autoscaler? Wenn Sie ein externer Benutzer sind, [registrieren Sie sich für den öffentlichen Slack ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://bxcs-slack-invite.mybluemix.net/) und senden Ihre Beiträge über den Kanal [#cluster-autoscaler ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://ibm-container-service.slack.com/messages/CF6APMLBB). Als IBM Mitarbeiter senden Sie Ihre Beiträge über den [internen Slack-Kanal ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://ibm-argonauts.slack.com/messages/C90D3KZUL).</td>
</tr>
</tbody></table>




## Beliebte Themen im Februar 2019
{: #feb19}

<table summary="Die Tabelle zeigt beliebte Themen. Die Zeilen sind von links nach rechts zu lesen. In Spalte 1 finden Sie das Datum, den Titel der Funktion in Zeile 2 und eine Beschreibung in Zeile 3.">
<caption>Beliebte Themen im Bereich Container und Kubernetes-Cluster im Februar 2019</caption>
<thead>
<th>Datum</th>
<th>Titel</th>
<th>Beschreibung</th>
</thead>
<tbody>
<tr>
<td>25. Februar</td>
<td>Differenziertere Steuerung für das Extrahieren von Images aus {{site.data.keyword.registrylong_notm}}</td>
<td>Wenn Sie Ihre Workloads in {{site.data.keyword.containerlong_notm}}-Clustern bereitstellen wollen, können Ihre Container Images jetzt aus den [neuen Domänennamen `icr.io` für {{site.data.keyword.registrylong_notm}}](/docs/services/Registry?topic=registry-registry_overview#registry_regions) extrahieren. Darüber hinaus können Sie differenzierte Zugriffsrichtlinien in {{site.data.keyword.Bluemix_notm}} IAM zur Steuerung des Zugriffs auf Ihre Images verwenden. Weitere Informationen finden Sie unter [Berechtigung des Clusters zum Extrahieren von Images](/docs/containers?topic=containers-images#cluster_registry_auth).</td>
</tr>
<tr>
<td>21. Februar</td>
<td>[Zone in Mexiko verfügbar](/docs/containers?topic=containers-regions-and-zones#regions-and-zones)</td>
<td>Mexiko (`mex01`) ist eine neue Zone für Cluster in der Region 'Vereinigte Staaten (Süden)'. Wenn Sie über eine Firewall verfügen, stellen Sie sicher, dass Sie [die Firewall-Ports](/docs/containers?topic=containers-firewall#firewall_outbound) für diese Zone und die anderen Zonen in der Region öffnen, in der sich Ihr Cluster befindet.</td>
</tr>
<tr><td>6. Februar 2019</td>
<td>[Istio on {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-istio)</td>
<td>Istio on {{site.data.keyword.containerlong_notm}} stellt eine nahtlose Installation von Istio, automatische Updates und Lebenszyklusmanagementfunktionen für Komponenten der Istio-Steuerebene bereit und ermöglicht die Integration in Protokollierungs- und Überwachungstools für die Plattform. Durch einen Klick können Sie alle Istio-Kernkomponenten, zusätzliche Trace-, Überwachungs- und Visualisierungsfunktionen sowie die betriebsbereite Beispielapp 'BookInfo' abrufen. Istio on {{site.data.keyword.containerlong_notm}} wird als verwaltetes Add-on angeboten, sodass {{site.data.keyword.Bluemix_notm}} Ihre Istio-Komponenten automatisch auf aktuellem Stand hält.</td>
</tr>
<tr>
<td>6. Februar 2019</td>
<td>[Knative on {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-knative_tutorial)</td>
<td>Knative ist eine Open-Source-Plattform, die die Funktionalität von Kubernetes erweitert, um Sie bei der Erstellung moderner, quellenzentrierter, containerisierter und serverunabhängiger Apps auf der Basis eines Kubernetes-Clusters zu unterstützen. Managed Knative on {{site.data.keyword.containerlong_notm}} ist ein verwaltetes Add-on, das Knative und Istio direkt in Ihren Kubernetes-Cluster integriert. Die Knative- und die Istio-Version in dem Add-on werden von IBM getestet und für die Verwendung in {{site.data.keyword.containerlong_notm}} unterstützt.</td>
</tr>
</tbody></table>


## Beliebte Themen im Januar 2019
{: #jan19}

<table summary="Die Tabelle zeigt beliebte Themen. Die Zeilen sind von links nach rechts zu lesen. In Spalte 1 finden Sie das Datum, den Titel der Funktion in Zeile 2 und eine Beschreibung in Zeile 3.">
<caption>Beliebte Themen im Bereich Container und Kubernetes-Cluster im Januar 2019</caption>
<thead>
<th>Datum</th>
<th>Titel</th>
<th>Beschreibung</th>
</thead>
<tbody>
<tr>
<td>30. Januar</td>
<td>{{site.data.keyword.Bluemix_notm}} IAM-Servicezugriffsrollen und Kubernetes-Namensbereiche</td>
<td>{{site.data.keyword.containerlong_notm}} unterstützt jetzt {{site.data.keyword.Bluemix_notm}} IAM-[Servicezugriffsrollen](/docs/containers?topic=containers-access_reference#service). Diese Servicezugriffsrollen orientieren sich an [Kubernetes-RBAC-Rollen](/docs/containers?topic=containers-users#role-binding), um Benutzer für die Ausführung von `kubectl`-Aktionen im Cluster zu berechtigen, um Kubernetes-Ressourcen wie Pods oder Bereitstellungen zu verwalten. Des Weiteren können Sie durch {{site.data.keyword.Bluemix_notm}} IAM-Servicezugriffsregeln den [Benutzerzugriff auf einen bestimmten Kubernetes-Namensbereich im Cluster einschränken](/docs/containers?topic=containers-users#platform). [Plattformzugriffsrollen](/docs/containers?topic=containers-access_reference#iam_platform) werden jetzt dazu verwendet, Benutzer für die Ausführung von `ibmcloud ks`-Aktionen zur Verwaltung Ihrer Clusterinfrastruktur, wie zum Beispiel Workerknoten, zu berechtigen.<br><br>{{site.data.keyword.Bluemix_notm}} IAM-Servicezugriffsrollen werden automatisch vorhandenen {{site.data.keyword.containerlong_notm}}-Konten und -Clustern auf Grundlage der Berechtigungen hinzugefügt, die Ihre Benutzer zuvor durch IAM-Plattformzugriffsrollen hatten. Ab jetzt können Sie mit IAM Benutzergruppen erstellen, den Benutzergruppen Benutzer hinzufügen und den Gruppen Plattform- oder Servicezugriffsrollen zuweisen. Weitere Informationen finden Sie im folgenden Blog: [Introducing service roles and namespaces in IAM for more granular control of cluster access ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/blogs/bluemix/2019/02/introducing-service-roles-and-namespaces-in-iam-for-more-granular-control-of-cluster-access/).</td>
</tr>
<tr>
<td>8. Januar</td>
<td>[Cluster-Autoscaler (Vorschau - Beta)](/docs/containers?topic=containers-ca#ca)</td>
<td>Sie können die Worker-Pools in Ihrem Cluster automatisch skalieren, um die Anzahl von Workerknoten im Worker-Pool je nach Kapazitätsbedarf der geplanten Workloads zu erhöhen oder zu verringern. Um diese Beta-Version ausprobieren zu können, müssen Sie Zugriff auf die Whitelist anfordern.</td>
</tr>
<tr>
<td>8. Januar</td>
<td>[{{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool](/docs/containers?topic=containers-cs_troubleshoot#debug_utility)</td>
<td>Finden Sie es manchmal schwierig, alle YAML-Dateien und sonstigen Informationen zusammenzustellen, die Sie zur Untersuchung eines Problems benötigen? Probieren Sie das {{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool aus, das eine grafische Benutzerschnittstelle für die Zusammenstellung von Cluster-, Bereitstellungs- und Netzinformationen bereitstellt.</td>
</tr>
</tbody></table>

## Beliebte Themen im Dezember 2018
{: #dec18}

<table summary="Die Tabelle zeigt beliebte Themen. Die Zeilen sind von links nach rechts zu lesen. In Spalte 1 finden Sie das Datum, den Titel der Funktion in Zeile 2 und eine Beschreibung in Zeile 3.">
<caption>Beliebte Themen im Bereich Container und Kubernetes-Cluster im Dezember 2018</caption>
<thead>
<th>Datum</th>
<th>Titel</th>
<th>Beschreibung</th>
</thead>
<tbody>
<tr>
<td>11. Dezember</td>
<td>SDS-Unterstützung mit Portworx</td>
<td>Mit Portworx können Sie persistenten Speicher für containerisierten Datenbanken und andere statusabhängigen Apps verwalten oder Daten zwischen Pods über mehrere Zonen hinweg nutzen. [Portworx ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://portworx.com/products/introduction/) ist eine hoch verfügbare SDS-Lösung (Software-Defined Storage), die den lokalen Blockspeicher Ihrer Workerknoten verwaltet, um eine einheitliche persistente Speicherebene für Ihre Apps zu erstellen. Portworx stellt durch eine workerknotenübergreifende Datenträgerreplikation für jeden Datenträger auf Containerebene eine zonenübergreifende Datenpersistenz und Datenverfügbarkeit sicher. Weitere Informationen finden Sie unter [Daten in softwaredefiniertem Speicher (SDS) mit Portworx speichern](/docs/containers?topic=containers-portworx#portworx).    </td>
</tr>
<tr>
<td>6. Dezember</td>
<td>{{site.data.keyword.mon_full_notm}}</td>
<td>Gewinnen Sie betriebliche Einblicke in die Leistung und den Allgemeinzustand Ihrer Apps, indem Sie Sysdig als Drittanbieterservice auf Ihren Workerknoten bereitstellen, um Metriken an {{site.data.keyword.monitoringlong}} weiterzuleiten. Weitere Informationen finden Sie unter [Metriken für eine App analysieren, die in einem Kubernetes-Cluster bereitgestellt wurde](/docs/services/Monitoring-with-Sysdig/tutorials?topic=Sysdig-kubernetes_cluster#kubernetes_cluster). **Hinweis**: Wenn Sie {{site.data.keyword.mon_full_notm}} mit Clustern verwenden, auf denen Kubernetes Version 1.11 oder höher ausgeführt wird, werden nicht alle Containermetriken erfasst, da Sysdig derzeit `containerd` nicht unterstützt.</td>
</tr>
<tr>
<td>4. Dezember</td>
<td>[Reserven für Workerknotenressourcen](/docs/containers?topic=containers-plan_clusters#resource_limit_node)</td>
<td>Stellen Sie so viele Apps bereit, dass Sie besorgt sind, Sie könnten Ihre Clusterkapazität überschreiten? Reserven für Workerknotenressourcen und Kubernetes-Bereinigungen (Evictions) schützen die Rechenkapazität Ihres Clusters, sodass Ihr Cluster über genügend Ressourcen verfügt, um die Ausführung fortzusetzen. Fügen Sie einfach einige weitere Workerknoten hinzu und Ihre Pods werden automatisch auf diesen Workerknoten erneut geplant. Reserven für Workerknotenressourcen werden in den neuesten [Patchänderungen der Version](/docs/containers?topic=containers-changelog#changelog) aktualisiert.</td>
</tr>
</tbody></table>

## Beliebte Themen im November 2018
{: #nov18}

<table summary="Die Tabelle zeigt beliebte Themen. Die Zeilen sind von links nach rechts zu lesen. In Spalte 1 finden Sie das Datum, den Titel der Funktion in Zeile 2 und eine Beschreibung in Zeile 3.">
<caption>Beliebte Themen im Bereich Container und Kubernetes-Cluster im November 2018</caption>
<thead>
<th>Datum</th>
<th>Titel</th>
<th>Beschreibung</th>
</thead>
<tbody>
<tr>
<td>29. November</td>
<td>[Zone in Chennai verfügbar](/docs/containers?topic=containers-regions-and-zones)</td>
<td>Chennai in Indien ist eine neue Zone für Cluster in der Region 'Asien-Pazifik (Norden)'. Wenn Sie über eine Firewall verfügen, stellen Sie sicher, dass Sie [die Firewall-Ports](/docs/containers?topic=containers-firewall#firewall) für diese Zone und die anderen Zonen in der Region öffnen, in der sich Ihr Cluster befindet.</td>
</tr>
<tr>
<td>27. November</td>
<td>{{site.data.keyword.la_full_notm}}</td>
<td>Fügen Sie Ihrem Cluster Protokollmanagementfunktionen hinzu, indem Sie LogDNA als Drittanbieterservice auf Ihren Workerknoten implementieren, um Protokolle aus Ihren Pod-Containern zu verwalten. Weitere Informationen finden Sie unter [Kubernetes-Clusterprotokolle mit {{site.data.keyword.loganalysisfull_notm}} und LogDNA verwalten](/docs/services/Log-Analysis-with-LogDNA/tutorials?topic=LogDNA-kube#kube).</td>
</tr>
<tr>
<td>7. November</td>
<td>Netzlastausgleichsfunktion 2.0 (Beta)</td>
<td>Sie haben jetzt die Wahl zwischen [NLB 1.0 oder 2.0](/docs/containers?topic=containers-loadbalancer#planning_ipvs), um Ihre Cluster-Apps sicher öffentlich zugänglich zu machen.</td>
</tr>
<tr>
<td>7. November</td>
<td>Kubernetes Version 1.12 verfügbar</td>
<td>Sie können jetzt Cluster aktualisieren oder erstellen, die [Kubernetes Version 1.12](/docs/containers?topic=containers-cs_versions#cs_v112) ausführen! Cluster der Version 1.12 werden standardmäßig mit hoch verfügbaren Kubernetes-Mastern bereitgestellt.</td>
</tr>
<tr>
<td>7. November</td>
<td>Hoch verfügbare Master</td>
<td>Hoch verfügbare Master sind für Cluster verfügbar, die Kubernetes Version 1.10 ausführen! Alle im vorherigen Eintrag für Cluster der Version 1.11 beschriebenen Vorteile gelten auch für Cluster der Version 1.10. Dies gilt auch für die [Vorbereitungsschritte](/docs/containers?topic=containers-cs_versions#110_ha-masters), die Sie ausführen müssen.</td>
</tr>
<tr>
<td>1. November</td>
<td>Hoch verfügbare Master in Clustern, die Kubernetes Version 1.11 ausführen</td>
<td>In einer Einzelzone ist Ihr Master hochverfügbar und umfasst Replikate auf separaten physischen Hosts für Ihren Kubernetes-API-Server, für 'etcd', für Ihren Scheduler und Ihren Controller-Manager, um beispielsweise vor Ausfällen während der Aktualisierung eines Clusters zu schützen. Wenn sich Ihr Cluster in einer mehrzonenfähigen Zone befindet, wird Ihr hoch verfügbarer Master über Zonen verteilt, um besser vor einem Zonenausfall zu schützen.<br>Informationen zu den Aktionen, die Sie ausführen müssen, finden Sie unter [Aktualisierung auf hoch verfügbare Cluster-Master](/docs/containers?topic=containers-cs_versions#ha-masters). Diese Vorbereitungsaktionen finden Anwendung:<ul>
<li>Wenn Sie über eine Firewall oder über angepasste Calico-Netzrichtlinien verfügen.</li>
<li>Wenn Sie die Host-Ports `2040` oder `2041` auf Ihren Workerknoten verwenden.</li>
<li>Wenn Sie die IP-Adresse des Cluster-Masters für den clusterinternen Zugriff auf den Master verwendet haben.</li>
<li>Wenn eine Automatisierung vorhanden ist, die die Calico-API oder -CLI (`calicoctl`) aufruft, z. B. um Calico-Richtlinien zu erstellen.</li>
<li>Wenn Sie Kubernetes- oder Calico-Netzrichtlinien zum Steuern des Pod-Egress-Zugriffs auf den Master verwenden.</li></ul></td>
</tr>
</tbody></table>

## Beliebte Themen im Oktober 2018
{: #oct18}

<table summary="Die Tabelle zeigt beliebte Themen. Die Zeilen sind von links nach rechts zu lesen. In Spalte 1 finden Sie das Datum, den Titel der Funktion in Zeile 2 und eine Beschreibung in Zeile 3.">
<caption>Beliebte Themen im Bereich Container und Kubernetes-Cluster im Oktober 2018</caption>
<thead>
<th>Datum</th>
<th>Titel</th>
<th>Beschreibung</th>
</thead>
<tbody>
<tr>
<td>25. Oktober</td>
<td>[Zone in Mailand verfügbar](/docs/containers?topic=containers-regions-and-zones)</td>
<td>Mailand (Italien) ist eine neue Zone für gebührenpflichtige Cluster in der Region 'Mitteleuropa'. Bisher war Mailand nur für kostenlose Cluster verfügbar. Wenn Sie über eine Firewall verfügen, stellen Sie sicher, dass Sie [die Firewall-Ports](/docs/containers?topic=containers-firewall#firewall) für diese Zone und die anderen Zonen in der Region öffnen, in der sich Ihr Cluster befindet.</td>
</tr>
<tr>
<td>22. Oktober</td>
<td>[Neuer Mehrzonenstandort `lon05` in London](/docs/containers?topic=containers-regions-and-zones#zones)</td>
<td>Der Standort London in einer Mehrzonen-Metropole ersetzt die Zone `lon02` durch die neue Zone `lon05`, die mehr Infrastrukturressource bietet als `lon02`. Erstellen Sie neue Mehrzonencluster mit `lon05`. Vorhandene Cluster mit `lon02` werden unterstützt, aber von neuen Mehrzonenclustern muss stattdessen `lon05` verwendet werden.</td>
</tr>
<tr>
<td>05. Oktober</td>
<td>Integration mit {{site.data.keyword.keymanagementservicefull}} (Beta)</td>
<td>Sie können geheime Kubernetes-Schlüssel im Cluster verschlüsseln, indem Sie [{{site.data.keyword.keymanagementserviceshort}} (Beta) aktivieren](/docs/containers?topic=containers-encryption#keyprotect).</td>
</tr>
<tr>
<td>04. Oktober</td>
<td>[{{site.data.keyword.registrylong}} ist jetzt in {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM)](/docs/services/Registry?topic=registry-iam#iam) integriert.</td>
<td>Sie können {{site.data.keyword.Bluemix_notm}} IAM zum Steuern des Zugriffs auf Registrierungsressourcen verwenden, zum Beispiel zum Extrahieren, Hinzufügen oder Erstellen von Images. Wenn Sie einen Cluster erstellen, erstellen Sie auch ein Registry-Token, damit der Cluster mit der Registry arbeiten kann. Somit benötigen Sie die Plattformmanagementrolle **Administrator** der Registry auf Kontoebene zum Erstellen eines Clusters. Informationen zum Aktivieren von {{site.data.keyword.Bluemix_notm}} IAM für ein Registry-Konto finden Sie unter [Richtliniendurchsetzung für vorhandene Benutzer aktivieren](/docs/services/Registry?topic=registry-user#existing_users).</td>
</tr>
<tr>
<td>01. Oktober</td>
<td>[Ressourcengruppen](/docs/containers?topic=containers-users#resource_groups)</td>
<td>Sie können Ressourcengruppen verwenden, um {{site.data.keyword.Bluemix_notm}}-Ressourcen in Pipelines, Abteilungen oder andere Gruppierungen aufzuteilen, um das Zuordnen des Zugriffs und der Messverwendung zu erleichtern. Jetzt unterstützt {{site.data.keyword.containerlong_notm}} die Erstellung von Clustern in der Gruppe `default` oder einer anderen Ressourcengruppe, die Sie erstellen.</td>
</tr>
</tbody></table>

## Beliebte Themen im September 2018
{: #sept18}

<table summary="Die Tabelle zeigt beliebte Themen. Die Zeilen sind von links nach rechts zu lesen. In Spalte 1 finden Sie das Datum, den Titel der Funktion in Zeile 2 und eine Beschreibung in Zeile 3.">
<caption>Beliebte Themen im Bereich Container und Kubernetes-Cluster im September 2018</caption>
<thead>
<th>Datum</th>
<th>Titel</th>
<th>Beschreibung</th>
</thead>
<tbody>
<tr>
<td>25. September</td>
<td>[Neue Zonen verfügbar](/docs/containers?topic=containers-regions-and-zones)</td>
<td>Jetzt haben Sie noch mehr Optionen zum Bereitstellen von Apps.
<ul><li>Willkommen in San Jose, in dem es jetzt mit `sjc03` und `sjc04` zwei neue Zonen in der Region 'Vereinigte Staaten (Süden)' gibt. Wenn Sie eine Firewall haben, stellen Sie sicher, dass Sie [die Firewall-Ports](/docs/containers?topic=containers-firewall#firewall) für diese Zone und die anderen Zonen in der Region öffnen, in der sich Ihr Cluster befindet.</li>
<li>Mit den beiden neuen Zonen `tok04` und `tok05` können Sie jetzt in der Region 'Asien-Pazifik (Norden)' in Tokio [Mehrzonencluster erstellen](/docs/containers?topic=containers-plan_clusters#multizone).</li></ul></td>
</tr>
<tr>
<td>5. September</td>
<td>[Zone in Oslo verfügbar](/docs/containers?topic=containers-regions-and-zones)</td>
<td>Oslo (Norwegen) ist eine neue Zone in der EU-Zentralregion. Wenn Sie eine Firewall haben, stellen Sie sicher, dass Sie [die Firewall-Ports](/docs/containers?topic=containers-firewall#firewall) für diese Zone und die anderen Zonen in der Region öffnen, in der sich Ihr Cluster befindet.</td>
</tr>
</tbody></table>

## Beliebte Themen im August 2018
{: #aug18}

<table summary="Die Tabelle zeigt beliebte Themen. Die Zeilen sind von links nach rechts zu lesen. In Spalte 1 finden Sie das Datum, den Titel der Funktion in Zeile 2 und eine Beschreibung in Zeile 3.">
<caption>Beliebte Themen im Bereich Container und Kubernetes-Cluster im August 2018</caption>
<thead>
<th>Datum</th>
<th>Titel</th>
<th>Beschreibung</th>
</thead>
<tbody>
<tr>
<td>31. August</td>
<td>{{site.data.keyword.cos_full_notm}} ist jetzt in {{site.data.keyword.containerlong}} integriert.</td>
<td>Verwenden Sie Kubernetes-native Persistent Volume Claims (PVC) zur Bereitstellung von {{site.data.keyword.cos_full_notm}} für Ihren Cluster. {{site.data.keyword.cos_full_notm}} wird am besten für leseintensive Workloads verwendet und wenn Sie Daten in mehreren Zonen in einem Mehrzonencluster speichern möchten. Beginnen Sie mit der [Erstellung einer {{site.data.keyword.cos_full_notm}}-Serviceinstanz](/docs/containers?topic=containers-object_storage#create_cos_service) und der [Installation des {{site.data.keyword.cos_full_notm}}-Plug-ins](/docs/containers?topic=containers-object_storage#install_cos) in Ihrem Cluster. </br></br>Sind Sie nicht sicher, welche Speicherlösung für Sie die richtige sein könnte? Beginnen Sie [hier](/docs/containers?topic=containers-storage_planning#choose_storage_solution), um Ihre Daten zu analysieren und die geeignete Speicherlösung für Ihre Daten auszuwählen. </td>
</tr>
<tr>
<td>14. August</td>
<td>Sie können Ihre Cluster auf Kubernetes Version 1.11 aktualisieren, um eine Podpriorität zuzuweisen.</td>
<td>Nachdem Sie Ihren Cluster auf [Kubernetes Version 1.11](/docs/containers?topic=containers-cs_versions#cs_v111) aktualisiert haben, können Sie die Vorteile neuer Funktionen nutzen, z. B. höhere Containerlaufzeitleistung mit `containerd` oder [Zuweisen einer Podpriorität](/docs/containers?topic=containers-pod_priority#pod_priority).</td>
</tr>
</tbody></table>

## Beliebte Themen im Juli 2018
{: #july18}

<table summary="Die Tabelle zeigt beliebte Themen. Die Zeilen sind von links nach rechts zu lesen. In Spalte 1 finden Sie das Datum, den Titel der Funktion in Zeile 2 und eine Beschreibung in Zeile 3.">
<caption>Beliebte Themen im Bereich Container und Kubernetes-Cluster im Juli 2018</caption>
<thead>
<th>Datum</th>
<th>Titel</th>
<th>Beschreibung</th>
</thead>
<tbody>
<tr>
<td>30. Juli</td>
<td>[Verwenden Sie Ihren eigenen Ingress-Controller](/docs/containers?topic=containers-ingress#user_managed)</td>
<td>Haben Sie sehr spezifische Sicherheitsanforderungen oder andere angepasste Anforderungen für den Ingress-Controller Ihres Clusters? Ist dies der Fall, können Sie anstelle des Standardcontrollers einen eigenen Ingress-Controller ausführen.</td>
</tr>
<tr>
<td>10. Juli</td>
<td>Einführung von Mehrzonenclustern</td>
<td>Sie möchten die Clusterverfügbarkeit verbessern? Nun können Sie Ihren Cluster über mehrere Zonen hinweg in ausgewählten Metrobereichen verwenden. Weitere Informationen hierzu finden Sie im Abschnitt [Mehrzonencluster in {{site.data.keyword.containerlong_notm}} erstellen](/docs/containers?topic=containers-plan_clusters#multizone).</td>
</tr>
</tbody></table>

## Beliebte Themen im Juni 2018
{: #june18}

<table summary="Die Tabelle zeigt beliebte Themen. Die Zeilen sind von links nach rechts zu lesen. In Spalte 1 finden Sie das Datum, den Titel der Funktion in Zeile 2 und eine Beschreibung in Zeile 3.">
<caption>Beliebte Themen im Bereich Container und Kubernetes-Cluster im Juni 2018</caption>
<thead>
<th>Datum</th>
<th>Titel</th>
<th>Beschreibung</th>
</thead>
<tbody>
<tr>
<td>13. Juni</td>
<td>Der Befehlsname `bx` der CLI wird in `ic` geändert</td>
<td>Wenn Sie die neueste Version der {{site.data.keyword.Bluemix_notm}}-CLI herunterladen, führen Sie jetzt Befehle aus, indem Sie das Präfix `ic` anstelle von `bx` verwenden. Listen Sie beispielsweise Ihre Cluster auf, indem Sie `ibmcloud ks clusters` ausführen.</td>
</tr>
<tr>
<td>12. Juni</td>
<td>[Sicherheitsrichtlinien für Pods](/docs/containers?topic=containers-psp)</td>
<td>Für Cluster, die Kubernetes 1.10.3 oder höher ausführen, können Sie
Pod-Sicherheitsrichtlinien konfigurieren, um zu festzulegen, wer Pods in {{site.data.keyword.containerlong_notm}} erstellen und aktualisieren darf.</td>
</tr>
<tr>
<td>6. Juni</td>
<td>[TLS-Unterstützung für die von IBM bereitgestellte Platzhalterunterdomäne](/docs/containers?topic=containers-ingress#wildcard_tls)</td>
<td>Für Cluster, die am oder nach dem 6. Juni 2018 erstellt wurden, ist das von IBM bereitgestellte TLS-Zertifikat für Ingress-Unterdomänen nun ein Platzhalterzertifikat und kann für die registrierte Platzhalterunterdomäne verwendet werden. Für Cluster, die vor dem 6. Juni 2018 erstellt wurden, wird das TLS-Zertifikat auf ein Platzhalterzertifikat aktualisiert, wenn das aktuelle TLS-Zertifikat verlängert wird.</td>
</tr>
</tbody></table>

## Beliebte Themen im Mai 2018
{: #may18}


<table summary="Die Tabelle zeigt beliebte Themen. Die Zeilen sind von links nach rechts zu lesen. In Spalte 1 finden Sie das Datum, den Titel der Funktion in Zeile 2 und eine Beschreibung in Zeile 3.">
<caption>Beliebte Themen im Bereich Container und Kubernetes-Cluster im Mai 2018</caption>
<thead>
<th>Datum</th>
<th>Titel</th>
<th>Beschreibung</th>
</thead>
<tbody>
<tr>
<td>24. Mai</td>
<td>[Neues Format für Ingress-Unterdomänen](/docs/containers?topic=containers-ingress)</td>
<td>Clustern, die nach dem 24. Mai erstellt wurden, wird eine Ingress-Unterdomäne in dem neuen Format <code>&lt;clustername&gt;.&lt;region&gt;.containers.appdomain.cloud</code> zugewiesen. Wenn Sie Ihre Apps über Ingress zugänglich machen, können Sie mithilfe der neuen Unterdomänen über das Internet auf die Apps zugreifen.</td>
</tr>
<tr>
<td>14. Mai</td>
<td>[Aktualisierung: Workloads auf Bare-Metal-GPU-Maschinentypen weltweit](/docs/containers?topic=containers-app#gpu_app)</td>
<td>Wenn Sie über einen [Bare-Metal-GPU-Maschinentyp (GPU)](/docs/containers?topic=containers-plan_clusters#shared_dedicated_node) auf Ihren Cluster verfügen, können Sie rechenintensive Apps terminieren. Der GPU-Workerknoten kann die Workload Ihrer App sowohl auf der CPU als auch der GPU zur Leistungssteigerung verarbeiten.</td>
</tr>
<tr>
<td>3. Mai</td>
<td>[Container Image Security Enforcement (Beta)](/docs/services/Registry?topic=registry-security_enforce#security_enforce)</td>
<td>Benötigt Ihr Team ein wenig zusätzliche Unterstützung dabei herauszufinden, für welches Image in den App-Containern eine Pull-Operation durchgeführt werden soll? Mit Container Image Security Enforcement (Beta) können Sie Container-Images überprüfen, bevor sie bereitgestellt werden. Diese Option ist für Cluster verfügbar, die Kubernetes 1.9 oder höher ausführen.</td>
</tr>
<tr>
<td>1. Mai</td>
<td>[Kubernetes-Dashboard über die Konsole bereitstellen](/docs/containers?topic=containers-app#cli_dashboard)</td>
<td>Wollten Sie schon immer einmal mit nur einem Klick auf das Kubernetes-Dashboard zugreifen? Sehen Sie sich dazu die Schaltfläche **Kubernetes-Dashboard** in der {{site.data.keyword.Bluemix_notm}}-Konsole genauer an.</td>
</tr>
</tbody></table>




## Beliebte Themen im April 2018
{: #apr18}

<table summary="Die Tabelle zeigt beliebte Themen. Die Zeilen sind von links nach rechts zu lesen. In Spalte 1 finden Sie das Datum, den Titel der Funktion in Zeile 2 und eine Beschreibung in Zeile 3.">
<caption>Beliebte Themen im Bereich Container und Kubernetes-Cluster im April 2018</caption>
<thead>
<th>Datum</th>
<th>Titel</th>
<th>Beschreibung</th>
</thead>
<tbody>
<tr>
<td>17. April</td>
<td>{{site.data.keyword.Bluemix_notm}}-Blockspeicher</td>
<td>Installieren Sie das {{site.data.keyword.Bluemix_notm}}-Blockspeicher-[Plug-in](/docs/containers?topic=containers-block_storage#install_block), um persistente Daten in einem Blockspeicher zu speichern. Anschließend können Sie einen [neuen](/docs/containers?topic=containers-block_storage#add_block) Blockspeicher für Ihren Cluster erstellen oder einen [vorhandenen](/docs/containers?topic=containers-block_storage#existing_block) Blockspeicher verwenden.</td>
</tr>
<tr>
<td>13. April</td>
<td>[Neues Lernprogramm zum Migrieren von Cloud Foundry-Apps in Cluster](/docs/containers?topic=containers-cf_tutorial#cf_tutorial)</td>
<td>Haben Sie eine Cloud Foundry-App? Hier erfahren Sie, wie Sie denselben Code aus dieser App in einem Container implementieren, der in einem Kubernetes-Cluster ausgeführt wird.</td>
</tr>
<tr>
<td>5. April</td>
<td>[Protokolle filtern](/docs/containers?topic=containers-health#filter-logs)</td>
<td>Filtern Sie spezifische Protokolle heraus, damit sie nicht weitergeleitet werden. Protokolle können nach einem bestimmten Namensbereich, Containernamen, nach einer Protokollebene und einer Nachrichtenzeichenfolge gefiltert werden.</td>
</tr>
</tbody></table>

## Beliebte Themen im März 2018
{: #mar18}

<table summary="Die Tabelle zeigt beliebte Themen. Die Zeilen sind von links nach rechts zu lesen. In Spalte 1 finden Sie das Datum, den Titel der Funktion in Zeile 2 und eine Beschreibung in Zeile 3.">
<caption>Beliebte Themen im Bereich Container und Kubernetes-Cluster im März 2018</caption>
<thead>
<th>Datum</th>
<th>Titel</th>
<th>Beschreibung</th>
</thead>
<tbody>
<tr>
<td>16. März</td>
<td>[Bereitstellung eines Bare-Metal-Clusters mit Trusted Compute](/docs/containers?topic=containers-plan_clusters#shared_dedicated_node)</td>
<td>Erstellen Sie ein Bare-Metal-Cluster, das [Kubernetes Version 1.9](/docs/containers?topic=containers-cs_versions#cs_v19) oder eine höhere Version ausführt und aktivieren Sie Trusted Compute, um Ihre Workerknoten auf Manipulation zu überprüfen.</td>
</tr>
<tr>
<td>14. März</td>
<td>[Sichere Anmeldung bei {{site.data.keyword.appid_full}}](/docs/containers?topic=containers-supported_integrations#appid)</td>
<td>Verbessern Sie Ihre Apps, die in {{site.data.keyword.containerlong_notm}} ausgeführt werden, in dem Sie verlangen, dass Benutzer sich anmelden.</td>
</tr>
<tr>
<td>13. März</td>
<td>[Zone in São Paulo verfügbar](/docs/containers?topic=containers-regions-and-zones)</td>
<td>São Paulo in Brasilien ist nun eine neue Zone in der Region 'Vereinigte Staaten (Süden)'. Wenn Sie eine Firewall haben, stellen Sie sicher, dass Sie [die Firewall-Ports](/docs/containers?topic=containers-firewall#firewall) für diese Zone und die anderen Zonen in der Region öffnen, in der sich Ihr Cluster befindet.</td>
</tr>
</tbody></table>

## Beliebte Themen im Februar 2018
{: #feb18}

<table summary="Die Tabelle zeigt beliebte Themen. Die Zeilen sind von links nach rechts zu lesen. In Spalte 1 finden Sie das Datum, den Titel der Funktion in Zeile 2 und eine Beschreibung in Zeile 3.">
<caption>Beliebte Themen im Bereich Container und Kubernetes-Cluster im Februar 2018</caption>
<thead>
<th>Datum</th>
<th>Titel</th>
<th>Beschreibung</th>
</thead>
<tbody>
<tr>
<td>27. Februar</td>
<td>HVM-Images (HVM, Hardware Virtual Machine) für Workerknoten</td>
<td>Erhöhen Sie die E/A-Leistung für Ihre Workloads mit HVM-Images. Aktivieren Sie sie auf jedem vorhandenen Workerknoten mit dem [Befehl](/docs/containers?topic=containers-cs_cli_reference#cs_worker_reload) `ibmcloud ks worker-reload` oder mit dem [Befehl](/docs/containers?topic=containers-cs_cli_reference#cs_worker_update) `ibmcloud ks worker-update`.</td>
</tr>
<tr>
<td>26. Februar</td>
<td>[Automatische KubeDNS-Skalierung](https://kubernetes.io/docs/tasks/administer-cluster/dns-horizontal-autoscaling/)</td>
<td>KubeDNS führt nun eine Skalierung mit Ihrem Cluster aus, wenn dieser größer wird. Sie können die Skalierungsfaktoren anpassen, indem Sie den folgenden Befehl verwenden: `kubectl -n kube-system edit cm kube-dns-autoscaler`.</td>
</tr>
<tr>
<td>23. Februar</td>
<td>In der Webkonsole nach [Protokollierung](/docs/containers?topic=containers-health#view_logs) und [Messwerten](/docs/containers?topic=containers-health#view_metrics) suchen</td>
<td>Sie können Protokolle und Messwerte mit einer verbesserten Webbenutzerschnittstelle bequem in Ihrem Cluster und seinen Komponenten anzeigen. Auf der Detailseite des Clusters finden Sie Informationen zum Zugriff.</td>
</tr>
<tr>
<td>20. Februar</td>
<td>Verschlüsselte Images und [-signierter, vertrauenswürdiger Inhalt](/docs/services/Registry?topic=registry-registry_trustedcontent#registry_trustedcontent)</td>
<td>In {{site.data.keyword.registryshort_notm}} können Sie Images signieren und verschlüsseln, um deren Integrität zu gewährleisten, wenn sie in Ihrem Registry-Namensbereich gespeichert werden. Führen Sie die Containerinstanzen aus, indem Sie nur vertrauenswürdige Inhalte verwenden.</td>
</tr>
<tr>
<td>19. Februar</td>
<td>[strongSwan-IPSec-VPN konfigurieren](/docs/containers?topic=containers-vpn#vpn-setup)</td>
<td>Stellen Sie schnell das strongSwan-IPSec-VPN-Helm-Diagramm bereit, um eine sichere Verbindung ohne Virtual Router Appliance zu Ihrem {{site.data.keyword.containerlong_notm}}-Cluster in Ihrem Rechenzentrum vor Ort herzustellen.</td>
</tr>
<tr>
<td>14. Februar</td>
<td>[Zone in Seoul verfügbar](/docs/containers?topic=containers-regions-and-zones)</td>
<td>Gerade rechtzeitig für Olympia wird ein Kubernetes-Cluster in Seoul in der Region 'Asien-Pazifik (Norden)' bereitgestellt. Wenn Sie eine Firewall haben, stellen Sie sicher, dass Sie [die Firewall-Ports](/docs/containers?topic=containers-firewall#firewall) für diese Zone und die anderen Zonen in der Region öffnen, in der sich Ihr Cluster befindet.</td>
</tr>
<tr>
<td>8. Februar</td>
<td>[Kubernetes 1.9 aktualisieren](/docs/containers?topic=containers-cs_versions#cs_v19)</td>
<td>Überprüfen Sie die Änderungen, die an Ihren Clustern vorgenommen werden müssen, bevor Sie auf Kubernetes 1.9 aktualisieren.</td>
</tr>
</tbody></table>

## Beliebte Themen im Januar 2018
{: #jan18}

<table summary="Die Tabelle zeigt beliebte Themen. Die Zeilen sind von links nach rechts zu lesen. In Spalte 1 finden Sie das Datum, den Titel der Funktion in Zeile 2 und eine Beschreibung in Zeile 3.">
<caption>Beliebte Themen im Bereich Container und Kubernetes-Cluster im Januar 2018</caption>
<thead>
<th>Datum</th>
<th>Titel</th>
<th>Beschreibung</th>
</thead>
<tbody>
<td>25. Januar</td>
<td>[Global Registry verfügbar](/docs/services/Registry?topic=registry-registry_overview#registry_regions)</td>
<td>Mit {{site.data.keyword.registryshort_notm}} können Sie die globale Registry `registry.bluemix.net` verwenden, um von IBM bereitgestellte öffentliche Images zu extrahieren.</td>
</tr>
<tr>
<td>23. Januar</td>
<td>[Zonen in Singapur und Montreal verfügbar](/docs/containers?topic=containers-regions-and-zones)</td>
<td>Singapur und Montreal sind Zonen, die in den {{site.data.keyword.containerlong_notm}}-Regionen 'Asien-Pazifik (Norden)' und 'Vereinigte Staaten (Osten)' verfügbar sind. Wenn Sie eine Firewall haben, stellen Sie sicher, dass Sie [die Firewall-Ports](/docs/containers?topic=containers-firewall#firewall) für diese Zonen und die anderen Zonen in der Region Ihres Clusters öffnen.</td>
</tr>
<tr>
<td>8 Januar</td>
<td>[Funktional erweiterte Typen verfügbar](/docs/containers?topic=containers-cs_cli_reference#cs_machine_types)</td>
<td>Die virtuellen Maschinentypen der Serie 2 umfassen lokalen SSD-Speicher und Plattenverschlüsselung. [Verschieben Sie Ihre Workloads](/docs/containers?topic=containers-update#machine_type) auf diese Typen, um die Leistung und Stabilität zu verbessern.</td>
</tr>
</tbody></table>

## Tauschen Sie sich mit Entwicklern mit ähnlichen Zuständigkeitsbereichen in Slack aus
{: #slack}

Sie können die Diskussionen anderer verfolgen und Ihre Fragen im [{{site.data.keyword.containerlong_notm}}-Slack stellen. ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://ibm-container-service.slack.com)
{:shortdesc}

Wenn Sie keine IBM ID für Ihr {{site.data.keyword.Bluemix_notm}}-Konto verwenden, [fordern Sie eine Einladung](https://bxcs-slack-invite.mybluemix.net/) zu diesem Slack an.
{: tip}
