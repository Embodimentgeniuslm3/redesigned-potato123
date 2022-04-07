---

copyright:
  years: 2014, 2019
lastupdated: "2019-04-18"

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

# Kubernetes-Strategie definieren
{: #strategy}

Mit {{site.data.keyword.containerlong}} können Sie Container-Workloads für Ihre Apps in der Produktion schnell und sicher bereitstellen. Erfahren Sie mehr, um beim Planen Ihrer Clusterstrategie Ihre Konfiguration so zu optimieren, dass Sie die Fähigkeiten von [Kubernetes![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/) zur automatischen Bereitstellung, Skalierung und Orchestrierungsverwaltung bestmöglich nutzen können.
{:shortdesc}

## Workloads nach {{site.data.keyword.Bluemix_notm}} verschieben
{: #cloud_workloads}

Es gibt eine Reihe von Gründen für das Verschieben von Workloads nach {{site.data.keyword.Bluemix_notm}}: Reduktion der Gesamtbetriebskosten, Steigerung der Hochverfügbarkeit für Ihre Apps in einer sicheren und konformen Umgebung, Scale-up und Scale-down entsprechend den Anforderungen Ihrer Benutzer und vieles mehr. {{site.data.keyword.containerlong_notm}} kombiniert die Containertechnologie mit Open-Source-Tools, wie z. B. Kubernetes, sodass Sie eine Cloud-native App erstellen können, die in verschiedene Cloud-Umgebungen migriert werden kann, ohne vom Anbieter gesperrt zu werden.
{:shortdesc}

Wie gelangen Sie nun in die Cloud? Welche Optionen haben Sie auf dem Weg dorthin? Und wie verwalten Sie Ihre Workloads, wenn Sie einmal dort sind?

Auf dieser Seite erfahren Sie mehr über einige Strategien für Ihre Kubernetes-Bereitstellungen auf {{site.data.keyword.containerlong_notm}}. Wenden Sie sich jederzeit gerne an unser Team auf [Slack. ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://ibm-container-service.slack.com)

Noch nicht bei Slack? [Fordern Sie eine Einladung an!](https://bxcs-slack-invite.mybluemix.net/)
{: tip}

### Was kann ich nach {{site.data.keyword.Bluemix_notm}} verschieben?
{: #move_to_cloud}

Mit {{site.data.keyword.Bluemix_notm}} haben Sie die Flexibilität der Auswahl zwischen einem [öffentlichen, privaten oder Hybrid-Cloud-Ansatz](/docs/containers?topic=containers-cs_ov#differentiation) für Ihre Workloads. In der folgenden Tabelle finden Sie einige Beispiele für die Typen von Workloads, die Benutzer in der Regel in die verschiedenen Cloudtypen verschieben.
{: shortdesc}

| Workload | Öffentlich | Privat | Hybrid |
| --- | --- | --- | --- |
| DevOps-Aktivierungstools | <img src="images/confirm.svg" width="32" alt="Funktion verfügbar" style="width:32px;" /> | | |
| Apps entwickeln und testen | <img src="images/confirm.svg" width="32" alt="Funktion verfügbar" style="width:32px;" /> | | |
| Apps haben große Nachfrageschwankungen und müssen schnell skaliert werden können | <img src="images/confirm.svg" width="32" alt="Funktion verfügbar" style="width:32px;" /> | | |
| Business-Apps wie CRM, HCM, ERP und E-Commerce | <img src="images/confirm.svg" width="32" alt="Funktion verfügbar" style="width:32px;" /> | | |
| Zusammenarbeits- und Social Tools wie E-Mail | <img src="images/confirm.svg" width="32" alt="Funktion verfügbar" style="width:32px;" /> | | |
| Linux- und x86-Workloads | <img src="images/confirm.svg" width="32" alt="Funktion verfügbar" style="width:32px;" /> | | |
| Intensive CPU- oder E/A-Kapazitätsanforderungen, die die öffentlichen Maschinentypen überschreiten | | <img src="images/confirm.svg" width="32" alt="Funktion verfügbar" style="width:32px;" /> | |
| Legacy-Apps mit Plattform- und Infrastruktureinschränkungen und -abhängigkeiten | | <img src="images/confirm.svg" width="32" alt="Funktion verfügbar" style="width:32px;" /> | <img src="images/confirm.svg" width="32" alt="Funktion verfügbar" style="width:32px;" /> |
| Proprietäre Apps mit strengen Designs, Lizenzierungsrichtlinien oder strengen Richtlinien | | <img src="images/confirm.svg" width="32" alt="Funktion verfügbar" style="width:32px;" /> | <img src="images/confirm.svg" width="32" alt="Funktion verfügbar" style="width:32px;" /> |
| Skalierung von Apps in der öffentlichen Cloud und Synchronisierung der Daten mit einer lokalen privaten Datenbank | | | <img src="images/confirm.svg" width="32" alt="Funktion verfügbar" style="width:32px;" /> |
{: caption="{{site.data.keyword.Bluemix_notm}}-Implementierungen unterstützen Ihre Workloads" caption-side="top"}

**Bereit für die Ausführung von Workloads in der öffentlichen Cloud?**</br>
Super! Sie befinden sich bereits in unserer öffentlichen Cloud-Dokumentation. Lesen Sie einfach weiter und erfahren Sie von Strategie-Ideen oder steigen Sie direkt ein, indem Sie [jetzt einen Cluster erstellen](/docs/containers?topic=containers-getting-started).

**Interessiert an der privaten Cloud?**</br>
Erkunden Sie die Dokumentation zu [{{site.data.keyword.Bluemix_notm}} Private ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.1/kc_welcome_containers.html). Wenn Sie bereits beträchtliche Investitionen in IBM Technologie wie WebSphere Application Server und Liberty getätigt haben, können Sie Ihre Modernisierungsstrategie für {{site.data.keyword.Bluemix_notm}} Private mit verschiedenen Tools optimieren.
* Um mehr Einblick in Ihre bestehenden Apps und die Unterstützungsumgebung zu erhalten, verwenden Sie den [IBM Transformation Advisor ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_2.1.0/featured_applications/transformation_advisor.html).
* Zur leichteren Bestimmung Ihrer Akzeptanzbereitschaft und zur Bereitstellung einer Roadmap für die Cloud machen Sie Gebrauch von den [{{site.data.keyword.Bluemix_notm}} Advisory Services ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/us-en/marketplace/cloud-consulting-services).
* Hilfe für Entwickler bei der Erstellung von Services für {{site.data.keyword.Bluemix_notm}} Private finden Sie unter [IBM Microclimate ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/us-en/marketplace/microclimate).
* Bei der Mehrfachbereitstellung in der Cloud erwägen Sie die Verwendung des [{{site.data.keyword.Bluemix_notm}} Automation Manager ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/us-en/marketplace/cognitive-automation).
* Informationen zur Verwaltung mehrerer Cloud-Kubernetes-Cluster beispielsweise über {{site.data.keyword.Bluemix_notm}} Public und {{site.data.keyword.Bluemix_notm}} Private hinweg finden Sie in [IBM Multicloud Manager ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.1.0/mcm/getting_started/introduction.html).

**Sie möchten sowohl die öffentliche als auch die private Cloud nutzen?**</br>
Beginnen Sie mit der Einrichtung eines {{site.data.keyword.Bluemix_notm}} Private-Kontos. Danach lesen Sie den Abschnitt [{{site.data.keyword.containerlong_notm}} mit {{site.data.keyword.Bluemix_notm}} Private](/docs/containers?topic=containers-hybrid_iks_icp) verwenden, um die {{site.data.keyword.Bluemix_notm}} Private-Umgebung mit einem Cluster in {{site.data.keyword.Bluemix_notm}} Public zu verbinden.

### Welche Art von Apps kann ich in {{site.data.keyword.containerlong_notm}} ausführen?
{: #app_types}

Ihre containerisierte App muss auf dem unterstützten Betriebssystem, Ubuntu 16.64, 18.64, ausgeführt werden können. Sie sollten auch den Status Ihrer App berücksichtigen.
{: shortdesc}

<dl>
<dt>Statusunabhängige Apps</dt>
  <dd><p>Statusunabhängige Apps werden für Cloud-native Umgebungen wie Kubernetes bevorzugt. Sie sind einfach zu migrieren und skalieren, weil sie Abhängigkeiten deklarieren, Konfigurationen getrennt vom Code speichern und Sicherungsservices wie Datenbanken als angeschlossene Ressourcen und nicht als an die App gekoppelt behandeln. Die App-Pods benötigen keine persistente Datenspeicherung oder eine stabile IP-Netzadresse. Deshalb können die Pods beendet, neu terminiert und skaliert werden, um auf Workloadanforderungen zu reagieren. Die App verwendet einen Database as a Service für persistente Daten sowie NodePort-, Load-Balancer- oder Ingress-Services, um die Workload für eine stabile IP-Adresse zugänglich zu machen.</p></dd>
<dt>Statusabhängige Apps</dt>
  <dd><p>Statusabhängige Apps sind komplizierter einzurichten, zu verwalten und zu skalieren als statusunabhängige Apps, da die Pods persistente Daten und eine stabile Netzidentität benötigen. Statusabhängige Anwendungen sind häufig Datenbanken oder andere verteilte, datenintensive Workloads, bei denen die Verarbeitung effizienter und näher an den Daten selbst ist.</p>
  <p>Wenn Sie eine statusabhängige App bereitstellen möchten, müssen Sie persistenten Speicher einrichten und einen persistenten Datenträger an den Pod anhängen, der von einem StatefulSet-Objekt kontrolliert wird. Sie können wahlweise einen [Datei](/docs/containers?topic=containers-file_storage#file_statefulset)-, [Block](/docs/containers?topic=containers-block_storage#block_statefulset)- oder [Objekt](/docs/containers?topic=containers-object_storage#cos_statefulset)-Speicher als persistenten Speicher für Ihre statusabhängige Gruppe hinzufügen. Sie können darüber hinaus [Portworx](/docs/containers?topic=containers-portworx) über Ihre Bare-Metal-Workerknoten installieren und Portworx als hoch verfügbare softwaredefinierte Speicherlösung zur Verwaltung von persistentem Speicher für Ihre statusabhängigen Apps verwenden. Weitere Informationen zur Funktionsweise von statusabhängigen Gruppen finden Sie in der [Kubernetes-Dokumentation![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/).</p></dd>
</dl>

### Welche Leitlinien gibt es für die Entwiclung von statusunabhängigen, Cloud-nativen Apps?
{: #12factor}

Sehen Sie sich die [Zwölf-Faktor-App ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://12factor.net/) an, eine sprachunabhängige Methodik für die Entwicklung Ihrer App über 12 Faktoren, die sich wie folgt zusammenfassen lässt.
{: shortdesc}

1.  **Codebasis**: Verwenden Sie eine einzelne Codebasis in einem Versionssteuerungssystem für Ihre Bereitstellungen. Wenn Sie ein Image für Ihre Containerbereitstellung extrahieren, geben Sie einen getesteten Image-Tag anstelle von `latest` an.
2.  **Abhängigkeiten**: Deklarieren und isolieren Sie explizit externe Abhängigkeiten.
3.  **Konfiguration**: Speichern Sie die bereitstellungsspezifische Konfiguration in Umgebungsvariablen, nicht im Code.
4.  Sicherungsservices**: Behandeln Sie Sicherungsservices wie z. B. Datenspeicher oder Nachrichtenwarteschlangen als angeschlossene oder ersetzbare Ressourcen.
5.  **App-Phasen**: Führen Sie die Erstellung in streng voneinander abgegrenzten Phasen aus, wie `build`, `release`, `run`.
6.  **Prozesse**: Führen Sie einen oder mehrere statusunabhängige Prozesse aus, die nichts gemeinsam nutzen, und verwenden Sie [persistenten Speicher](/docs/containers?topic=containers-storage_planning) zum Speichern von Daten.
7.  **Portbindung**: Portbindungen sind eigenständig und stellen einen Serviceendpunkt für einen klar definierten Host und Port bereit.
8.  **Gleichzeitigkeit**: Verwalten und skalieren Sie Ihre App über Prozessinstanzen wie Replikate und horizontale Skalierung. Legen Sie Ressourcenanforderungen und Grenzwerte für Ihre Bereitstellungen fest. Beachten Sie, dass Calico-Netzrichtlinien die Bandbreite nicht begrenzen können. Ziehen Sie stattdessen [Istio](/docs/containers?topic=containers-istio) in Betracht.
9.  **Löschbarkeit**: Konstruieren Sie Ihre App so, dass sie gelöscht werden kann, minimale Startkapazitäten benötigt, sich ordnungsgemäß beenden lässt und abrupte Prozessbeendigungen toleriert. Denken Sie daran, dass Container, Pods und sogar Workerknoten löschbar sein sollten, und planen Sie Ihre App entsprechend.
10.  **Dev-Prod-Vergleichbarkeit**: Richten Sie eine Pipeline für [kontinuierliche Integration](https://www.ibm.com/cloud/garage/content/code/practice_continuous_integration/) und eine für [kontinuierliche Entwicklung](https://www.ibm.com/cloud/garage/content/deliver/practice_continuous_delivery/) für Ihre App ein, wobei zwischen der App in Entwicklung und der App in Produktion nur minimale Unterschiede bestehen sollten.
11.  **Protokolle**: Behandeln Sie Protokolle als Ereignisströme: Die äußere oder Hostingumgebung verarbeitet Protokolldateien und leitet sie weiter. **Wichtig**: In {{site.data.keyword.containerlong_notm}} sind die Protokolle standardmäßig nicht aktiviert. Informationen zum Aktivieren finden Sie im Abschnitt [Protokollweiterleitung konfigurieren](/docs/containers?topic=containers-health#configuring).
12.  **Verwaltungsprozesse**: Behalten Sie in der App alle Einmal-Administratorscripts als [Kubernetes-Jobobjekt ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/workloads/controllers/jobs-run-to-completion/) bei, um sicherzustellen, dass die Administratorscripts mit derselben Umgebung ausgeführt werden können wie die App selbst. Für das Orchestrieren größerer Pakete, die in Ihren Kubernetes-Clustern ausgeführt werden sollen, erwägen Sie die Verwendung eines Paketmanagers wie [Helm ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://helm.sh/).

### Ich habe bereits eine App. Wie kann ich sie nach {{site.data.keyword.containerlong_notm}} migrieren?
{: #migrate_containerize}

Die folgenden allgemeinen Schritte können Sie ausführen, um Ihre App zu containerisieren.
{: shortdesc}

1.  Verwenden Sie die [Zwölf-Faktor-App ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://12factor.net/) als Leitfaden zum Isolieren von Abhängigkeiten, Trennen von Prozessen in separate Services und Reduzieren der Statusabhängigkeit Ihrer App so weit es möglich ist.
2.  Suchen Sie ein geeignetes Basisimage, das verwendet werden soll. Sie können öffentlich verfügbare Images aus [Docker Hub ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://hub.docker.com/) oder [öffentliche IBM Images](/docs/services/Registry?topic=registry-public_images#public_images) verwenden oder Ihre eigenen Images in Ihrer privaten {{site.data.keyword.registryshort_notm}}-Instanz erstellen und verwalten.
3.  Fügen Sie dem Docker-Image nur die für die Ausführung der App erforderlichen Informationen hinzu.
4.  Anstatt auf dem lokalen Speicher aufzubauen, planen Sie die Verwendung von persistentem Speicher oder von Cloud-Database as a Service-Lösungen, um die Daten Ihrer App zu sichern.
5.  Im Laufe der Zeit refaktorieren Sie Ihre App-Prozesse in Microservices.

Weitere Informationen finden Sie in den folgenden Lernprogrammen:
*  [App aus Cloud Foundry in einen Cluster migrieren](/docs/containers?topic=containers-cf_tutorial#cf_tutorial)
*  [VM-basierte App nach Kubernetes verschieben](/docs/tutorials?topic=solution-tutorials-vm-to-containers-and-kubernetes)



Fahren Sie mit den folgenden Themen fort, um beim Verschieben von Workloads weitere Aspekte zu berücksichtigen, z. B. Kubernetes-Umgebungen, Hochverfügbarkeit, Serviceerkennung und Bereitstellungen.

<br />


## Dimensionierung des Kubernetes-Clusters für die Unterstützung Ihrer Workload
{: #sizing}

Herauszufinden, wie viele Workerknoten Sie in Ihrem Cluster benötigen, um Ihre Workload zu unterstützen, ist keine exakte Wissenschaft. Möglicherweise müssen Sie unterschiedliche Konfigurationen testen und anpassen. {{site.data.keyword.containerlong_notm}} bietet Ihnen den Vorteil, dass Sie Workerknoten als Antwort auf Ihre Workloadanforderungen hinzufügen und entfernen können.
{: shortdesc}

Zu Beginn der Dimensionierung Ihres Clusters stellen Sie sich die folgenden Fragen.

### Wie viele Ressourcen benötigt meine App?
{: #sizing_resources}

Zunächst beginnen wir mit Ihrer bestehenden oder für Ihr Projekt geplanten Auslastung.

1.  Berechnen Sie die durchschnittliche CPU- und Speichernutzung für Ihre Workload. Auf einem Windows-System können Sie z. B. den Task-Manager anzeigen oder auf einem Mac- oder Linux-System den Befehl `top` ausführen. Sie können auch einen Metrikservice verwenden und Berichte ausführen, um die Auslastung zu berechnen.
2.  Antizipieren Sie die Anzahl der Anforderungen, die Ihre Workload bedienen muss, damit Sie entscheiden können, wie viele App-Replikate die Workload verwalten sollen. Sie können zum Beispiel eine App-Instanz entwerfen, um 1000 Anforderungen pro Minute zu verarbeiten, und vorausnehmen, dass Ihre Workload 10000 Anforderungen pro Minute bedienen muss. Wenn dies der Fall ist, können Sie sich entscheiden, 12 App-Replikate zu machen, wobei 10 die erwartete Menge und weitere 2 die Bedarfsspitzen verarbeiten sollen.

### Was außer meiner App kann im Cluster ebenfalls Ressourcen verwenden?
{: #sizing_other}

Nun werden weitere Funktionen hinzugefügt, die Sie vielleicht gebrauchen können.



1.  Überlegen Sie, ob Ihre App große oder viele Images extrahiert, die lokalen Speicher auf dem Workerknoten belegen können.
2.  Entscheiden Sie, ob Sie in Ihren Cluster [Services integrieren](/docs/containers?topic=containers-supported_integrations#supported_integrations) möchten, z. B. [Helm](/docs/containers?topic=containers-helm#public_helm_install) oder [Prometheus ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/coreos/prometheus-operator/tree/master/contrib/kube-prometheus). Diese integrierten Services und Add-ons nehmen Pods in Anspruch, die Clusterressourcen verbrauchen.

### Welche Art von Verfügbarkeit soll meine Workload haben?
{: #sizing_availability}

Vergessen Sie nicht, dass Ihre Workload so weit wie möglich steigen soll!

1.  Planen Sie Ihre Strategie für [hoch verfügbare Cluster](/docs/containers?topic=containers-plan_clusters#ha_clusters), z. B. bei der Entscheidung zwischen Einzel- oder Mehrzonenclustern.
2.  Lesen Sie die Informationen zu [hoch verfügbaren Bereitstellungen](/docs/containers?topic=containers-app#highly_available_apps) als Entscheidungshilfe, auf welche Weise Sie Ihre App verfügbar machen.

### Wie viele Workerknoten brauche ich, um meine Workload zu bewältigen?
{: #sizing_workers}

Nachdem Sie nun eine gute Vorstellung davon haben, wie Ihre Workload aussieht, ordnen Sie die geschätzte Nutzung Ihren verfügbaren Clusterkonfigurationen zu.

1.  Schätzen Sie die maximale Kapazität des Workerknotens, die vom Typ Ihres Clusters abhängt. Verplanen Sie die Kapazität des Workerknotens nicht vollständig, denn es müssen auch Bedarfsspitzen oder andere temporäre Ereignisse berücksichtigt werden.
    *  **Einzonencluster**: Planen Sie mindestens drei Workerknoten für Ihren Cluster. Weiterhin benötigen Sie genügend im Cluster verfügbare CPU- und Speicherkapazität für einen zusätzlichen Knoten.
    *  **Mehrzonencluster**: Planen Sie mindestens zwei Workerknoten pro Zone, also insgesamt 6 Knoten, verteilt auf 3 Zonen. Planen Sie außerdem für die Gesamtkapazität Ihres Clusters mindestens 150%der erforderlichen Gesamtkapazität für die Workload, sodass Sie, wenn eine Zone ausfällt, Ressourcen für die Aufrechterhaltung der Workload zur Verfügung haben.
2.  Richten Sie die App-Größe und Workerknotenkapazität an einem der [verfügbaren Workerknotentypen](/docs/containers?topic=containers-plan_clusters#shared_dedicated_node) aus. Zum Anzeigen der verfügbaren Typen innerhalb einer Zone führen Sie `ibmcloud ks machine-types <zone>` aus.
    *   **Workerknoten nicht überladen**: Um zu vermeiden, dass Ihre Pods um die CPU konkurrieren oder ineffizient ausgeführt werden, müssen Sie wissen, welche Ressourcen Ihre Apps benötigen, sodass Sie die Anzahl der benötigten Workerknoten planen können. Wenn Ihre Apps beispielsweise weniger Ressourcen benötigen als auf dem Workerknoten verfügbar sind, können Sie die Anzahl der Pods begrenzen, die Sie auf einem Workerknoten bereitstellen. Belassen Sie Ihren Workerknoten bei einer Kapazität von rund 75 %, um Platz für andere Pods zu lassen, die möglicherweise geplant werden müssen. Wenn Ihre Apps mehr Ressourcen benötigen, als Sie auf Ihrem Workerknoten zur Verfügung haben, verwenden Sie einen anderen Workerknotentyp, der diese Anforderungen erfüllen kann. Sie wissen, dass Ihre Workerknoten überlastet sind, wenn sie regelmäßig den Status `NotReady` (Nicht bereit) aufweisen oder Pods aufgrund von unzureichendem Speicher oder fehlenden anderen Ressourcen entfernt werden müssen.
    *   **Größere oder kleinere Workerknotentypen**: Größere Knoten können kosteneffizienter sein als kleinere Knoten, besonders bei Workloads, die so konstruiert sind, dass sie bei der Verarbeitung auf Hochleistungsmaschinen eine erhöhte Effizienz an den Tag legen. Wenn allerdings ein großer Workerknoten inaktiv wird, müssen Sie sicherstellen, dass Ihr Cluster genügend Kapazität hat, um alle Workload-Pods ordnungsgemäß auf andere Workerknoten im Cluster umzuplanen. Kleinere Worker können Ihnen helfen, sinnvoller zu skalieren.
    *   **Replikate Ihrer App**: Um die Anzahl der gewünschten Workerknoten zu ermitteln, können Sie auch die Anzahl der Replikate Ihrer App berücksichtigen, die Sie ausführen möchten. Wenn Sie beispielsweise wissen, dass Ihre Workload 32 CPU-Kerne benötigt, und Sie planen, 16 Replikate Ihrer App auszuführen, benötigt jeder Replikat-Pod 2 CPU-Kerne. Wenn Sie pro Workerknoten nur eine App-Pod ausführen möchten, können Sie eine entsprechende Anzahl von Workerknoten für Ihren Clustertyp bestellen, um diese Konfiguration zu unterstützen.
3.  Führen Sie Leistungstests aus, um die Anzahl der benötigten Workerknoten in Ihrem Cluster mit repräsentativer Latenzzeit, Skalierbarkeit, Datenbestand und Workloadanforderungen genauer zu ermitteln.
4.  Für Workloads, die in Reaktion auf Ressourcenanforderungen auf- und abskaliert werden müssen, konfigurieren Sie den [horizontalen Pod-Autoscaler](/docs/containers?topic=containers-app#app_scaling) und den [Cluster-Worker-Pool-Autoscaler](/docs/containers?topic=containers-ca#ca).

<br />


## Kubernetes-Umgebung strukturieren
{: #kube_env}

Ihre {{site.data.keyword.containerlong_notm}}-Instanz ist mit nur einem IBM Cloud-Infrastrukturportfolio (SoftLayer) verbunden. Innerhalb Ihres Kontos können Sie Cluster erstellen, die sich aus einem Master und verschiedenen Workerknoten zusammensetzen. IBM verwaltet den Master und Sie können eine Mischung aus Worker-Pools erstellen, die aus einzelnen Maschinen desselben Typs oder Speicher- und CPU-Spezifikationen einen Pool bilden. Innerhalb des Clusters können Sie Ressourcen mithilfe von Namensbereichen und Bezeichnungen noch genauer verwalten. Wählen Sie die richtige Mischung aus Cluster, Maschinentypen und Organisationsstrategien, sodass Sie sicherstellen können, dass Ihre Teams und Workloads die benötigten Ressourcen erhalten.
{:shortdesc}

### Welche Cluster- und Maschinentypen soll ich abrufen?
{: #env_flavors}

**Clustertypen**: Entscheiden Sie, ob Sie eine [Einzelzonenkonfiguration, Mehrzonenkonfiguration oder Konfiguration mit mehreren Clustern benötigen](/docs/containers?topic=containers-plan_clusters#ha_clusters). Mehrzonencluster sind in [allen sechs weltweiten {{site.data.keyword.Bluemix_notm}}-Metropolenregionen](/docs/containers?topic=containers-regions-and-zones#zones) verfügbar. Denken Sie auch daran, dass die Workerknoten je nach Zone variieren.

**Typen von Workerknoten**: Im Allgemeinen eignen sich Ihre intensiven Workloads eher für die Ausführung auf physischen Bare-Metal-Maschinen, während für kosteneffektive Tests und Entwicklungsarbeiten virtuelle Maschinen auf gemeinsam genutzter oder dedizierter gemeinsam genutzter Hardware gewählt werden könnten. Bei Bare-Metal-Workerknoten verfügt Ihr Cluster über eine Netzgeschwindigkeit von 10 Gb/s und Hyperthreading-Kerne, die einen höheren Durchsatz bieten. Virtuelle Maschinen verfügen über eine Netzgeschwindigkeit von 1 Gb/s und reguläre Kerne, die kein Hyper-Threading bieten. [Informieren Sie sich über die Isolierung von Maschinen und die verfügbaren Typen](/docs/containers?topic=containers-plan_clusters#planning_worker_nodes).

### Soll ich mehrere Cluster verwenden oder einfach weitere Worker zu einem vorhandenen Cluster hinzufügen?
{: #env_multicluster}

Die Anzahl der Cluster, die Sie erstellen, ist von Ihrer Workload, den Unternehmensrichtlinien und -verordnungen sowie den Ressourcen abhängig, die Sie mit den IT-Ressourcen ausführen möchten. Über die Sicherheitsinformationen zu dieser Entscheidung können Sie sich auch unter [Containerisolation und Sicherheit](/docs/containers?topic=containers-security#container) informieren.

**Mehrere Cluster**: Sie müssen [eine globale Lastausgleichsfunktion](/docs/containers?topic=containers-plan_clusters#multiple_clusters) einrichten und darin jeweils dieselben YAML-Konfigurationsdateien anwenden, um die Workloads über die Cluster hinweg gleichmäßig zu verteilen. Daher sind mehrere Cluster in der Regel komplexer in der Verwaltung; sie können aber Ihnen helfen, wichtige Ziele wie die folgenden zu erreichen.
*  Einhalten von Sicherheitsrichtlinien, die eine Isolierung von Workloads erfordern.
*  Testen Sie, wie Ihre App in einer anderen Version von Kubernetes oder anderen Clustersoftware wie z. B. Calico ausgeführt wird.
*  Erstellen Sie einen Cluster mit Ihrer App in einer anderen Region, um eine höhere Leistung für Benutzer in diesem geografischen Bereich zu schaffen.
*  Konfigurieren Sie den Benutzerzugriff auf der Ebene der Clusterinstanz, anstatt mehrere RBAC-Richtlinien anzupassen und zu verwalten und damit den Zugriff innerhalb eines Clusters auf Ebene des Namensbereichs zu steuern.

**Weniger oder Einzelcluster**: Mit weniger Clustern können Sie den operativen Aufwand und die Kosten pro Cluster für feste Ressourcen reduzieren. Anstatt weitere Cluster zu bilden, können Sie Worker-Pools für verschiedene Maschinentypen von IT-Ressourcen hinzufügen, die für die zu verwendenden App- und Servicekomponenten verfügbar sind. Wenn Sie die App entwickeln, befinden sich die Ressourcen, die sie verwendet, in derselben Zone oder sind auf andere Weise in einer Mehrfachzone eng verbunden, sodass Sie Annahmen zu Latenzzeiten, Bandbreite oder korrelierten Fehlern machen können. Es wird jedoch noch wichtiger, dass Sie Ihren Cluster mithilfe von Namensbereichen, Ressourcenquoten und Bezeichnungen organisieren.

### Wie kann ich meine Ressourcen innerhalb des Clusters einrichten?
{: #env_resources}

<dl>
<dt>Berücksichtigen Sie die Kapazität Ihres Workerknotens.</dt>
  <dd>Beachten Sie Folgendes, um die Leistung Ihres Workerknotens optimal zu nutzen:
  <ul><li><strong>Behalten Sie die Stärke Ihrer Kerne bei</strong>: Jede Maschine besitzt eine bestimmte Anzahl von Kernen. Legen Sie abhängig von der Workload Ihrer App eine Begrenzung für die Anzahl der Pods pro Kern fest, z. B. 10.</li>
  <li><strong>Überlastung des Knotens vermeiden</strong>: Auch hier gilt: Nur weil ein Knoten mehr als 100 Pods enthalten kann, bedeutet dies nicht, dass es für Sie wünschenswert ist. Legen Sie abhängig von der Workload Ihrer App eine Begrenzung für die Anzahl der Pods pro Knoten fest, z. B. 40.</li>
  <li><strong>Nutzen Sie die Bandbreite Ihres Clusters nicht vollständig aus</strong>: Denken Sie daran, dass die Netzbandbreite für das Skalieren virtueller Maschinen bei ca. 1000 MB/s liegt. Wenn Sie in einem Cluster mehrere Hundert Workerknoten benötigen, teilen Sie diese auf mehrere Cluster mit weniger Knoten auf oder bestellen Sie Bare-Metal-Knoten.</li>
  <li><strong>Sortieren Sie Ihre Services</strong>: Planen Sie, wie viele Services Sie für Ihre Workload benötigen, bevor Sie irgendetwas bereitstellen. Netzbetriebs- und Portweiterleitungsregeln werden in 'Iptables' gestellt. Wenn Sie von einer größeren Anzahl an Services, z. B. über 5000 Services, ausgehen, teilen Sie den Cluster in mehrere Cluster auf.</li></ul></dd>
<dt>Stellen Sie verschiedene Typen von Maschinen bereit, um eine Mischung aus verschiedenen Rechenressourcen zu erhalten.</dt>
  <dd>Jeder möchte die Wahl haben, oder? Mit {{site.data.keyword.containerlong_notm}} haben Sie [eine Mischung von Maschinentypen](/docs/containers?topic=containers-plan_clusters#planning_worker_nodes), die Sie bereistellen können: von Bare-Metal für intensive Workloads bis hin zu virtuellen Maschinen für schnelles Skalieren. Verwenden Sie Bezeichnungen oder Namensbereiche, um Bereitstellungen auf Ihren Maschinen zu organisieren. Wenn Sie etwas bereitstellen, begrenzen Sie es so, dass der Pod Ihrer App nur auf Maschinen mit der richtigen Mischung von Ressourcen bereitstellt. Sie können beispielsweise eine Datenbankanwendung auf eine Bare-Metal-Maschine begrenzen, die einen großen lokalen Plattenspeicher hat, wie `md1c.28x512.4x4tb`.</dd>
<dt>Richten Sie mehrere Namensbereiche ein, wenn Sie mehrere Teams und Projekte haben, die den Cluster gemeinsam nutzen.</dt>
  <dd><p>Namensbereiche sind wie ein Cluster innerhalb des Clusters. Mit ihnen können Sie Clusterressourcen über [Ressourcenquoten ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/policy/resource-quotas/) und [Standardgrenzwerte ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/tasks/administer-cluster/memory-default-namespace/) aufteilen. Wenn Sie neue Namensbereiche erstellen, achten Sie darauf, dass Sie die richtigen [RBAC-Richtlinien](/docs/containers?topic=containers-users#rbac) für die Zugriffssteuerung einrichten. Weitere Informationen finden Sie unter [Cluster über Namensbereiche gemeinsam nutzen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/tasks/administer-cluster/namespaces/) in der Kubernetes-Dokumentation.</p>
  <p>Wenn Sie einen kleinen Cluster haben, nur ein paar Dutzend Benutzer und einander ähnliche Ressourcen (wie z. B. verschiedene Versionen derselben Software), benötigen Sie wahrscheinlich keine Vielzahl an Namensbereichen. Stattdessen können Sie Bezeichnungen verwenden.</p></dd>
<dt>Legen Sie Ressourcenquoten fest, damit die Benutzer in Ihrem Cluster Ressourcenanforderungen und Ressourcengrenzwerte anwenden müssen.</dt>
  <dd>Um sicherzustellen, dass jedes Team über die erforderlichen Ressourcen zum Bereitstellen von Services und Ausführen von Apps im Cluster verfügt, müssen Sie für jeden Namensbereich [Ressourcenquoten](https://kubernetes.io/docs/concepts/policy/resource-quotas/) festlegen. Ressourcenquoten legen die Bereitstellungsbedingungen für einen Namensbereich fest, z. B. die Anzahl der bereitzustellenden Kubernetes-Ressourcen sowie die Menge an CPU und Speicher, die von diesen Ressourcen verbraucht werden kann. Nachdem Sie eine Beschränkung festgelegt haben, müssen Benutzer Ressourcenanforderungen und -begrenzungen in ihre Bereitstellungen aufnehmen.</dd>
<dt>Organisieren Sie Ihre Kubernetes-Objekte mit Bezeichnungen</dt>
  <dd><p>Zum Organisieren und Auswählen Ihrer Kubernetes-Ressourcen, wie `Pods` oder `Knoten`, [verwenden Sie Kubernetes-Bezeichnungen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/). Standardmäßig wendet {{site.data.keyword.containerlong_notm}} einige Bezeichnungen an, darunter `arch`, `os`, `region`, `zone` und `machine-type`.</p>
  <p>Zu den Beispielanwendungsfällen für Bezeichnungen gehören das [Begrenzen des Netzverkehrs auf Edge-Workerknoten](/docs/containers?topic=containers-edge), das [Bereitstellen einer App auf einer GPU-Maschine](/docs/containers?topic=containers-app#gpu_app) und das [Beschränken Ihrer App-Workloads ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/) auf die Ausführung nur auf Workerknoten, die bestimmte Maschinentypen oder SDS-Fähigkeiten haben, z. B. Bare-Metal-Workerknoten. Um zu sehen, welche Bezeichnungen bereits auf eine Ressource angewendet werden, verwenden Sie den Befehl <code>kubectl get</code> mit dem Flag <code>--show-labels</code>. Beispiel:</p>
  <p><pre class="pre"><code>kubectl get node &lt;node_ID&gt; --show-labels</code></pre></p></dd>
</dl>




<br />


## Ressourcen hoch verfügbar machen
{: #kube_ha}

Es ist zwar kein System vollkommen ausfallsicher, aber mit einigen Schritten können Sie die Hochverfügbarkeit Ihrer Apps und Services in {{site.data.keyword.containerlong_notm}} erhöhen.
{:shortdesc}

Lesen Sie mehr darüber, wie Sie Ihre Ressourcen hoch verfügbar machen können.
* [Mögliche Fehlerquellen reduzieren](/docs/containers?topic=containers-ha#ha).
* [Mehrzonencluster erstellen](/docs/containers?topic=containers-plan_clusters#ha_clusters).
* [Hoch verfügbare Bereitstellungen planen](/docs/containers?topic=containers-app#highly_available_apps), die Funktionen wie Replikatgruppen oder Anti-Affinität für Pods über mehrere Zonen verwenden.
* [Container auf Grundlage von Images in einer cloudbasierten öffentlichen Registry ausführen](/docs/containers?topic=containers-images).
* [Datenspeicherung planen](/docs/containers?topic=containers-storage_planning#persistent_storage_overview). Besonders bei Mehrzonenclustern erwägen Sie die Verwendung eines Cloud-Service wie z. B. [{{site.data.keyword.cloudant_short_notm}}](/docs/services/Cloudant?topic=cloudant-getting-started#getting-started) oder [{{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-about#about).
* Für Mehrzonencluster aktivieren Sie einen [Lastausgleichsservice](/docs/containers?topic=containers-loadbalancer#multi_zone_config) oder die Ingress [Lastausgleichsfunktion für mehrere Zonen](/docs/containers?topic=containers-ingress#ingress), um Ihre Apps öffentlich zugänglich zu machen.

<br />


## Serviceerkennung einrichten
{: #service_discovery}

Jeder Ihrer Pods in Ihrem Kubernetes-Cluster verfügt über eine IP-Adresse. Wenn Sie jedoch eine App in Ihrem Cluster bereitstellen, sollten die Serviceerkennung und der Netzbetrieb nicht auf der IP-Adresse Ihres Pods beruhen. Pods werden häufig und dynamisch entfernt oder ersetzt. Verwenden Sie vielmehr einen Kubernetes-Service, der eine Gruppe von Pods darstellt und einen stabilen Eingangspunkt über die virtuelle IP-Adresse des Service, die `Cluster-IP`, bietet. Weitere Informationen finden Sie in der Kubernetes-Dokumentation zu [Services ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/services-networking/service/#discovering-services).
{:shortdesc}

### Kann ich den DNS-Provider für den Kubernetes-Cluster anpassen?
{: #services_dns}

Wenn Sie Services und Pods erstellen, wird diesen ein DNS-Name zugewiesen, sodass Ihre App-Container die DNS-Service-IP verwenden können, um DNS-Namen aufzulösen. Sie können den Pod-DNS mittels der Angabe von Namensservern, Suchen und Objektlistenoptionen anpassen. Weitere Informationen finden Sie unter [Cluster-DNS-Provider konfigurieren](/docs/containers?topic=containers-cluster_dns#cluster_dns).
{: shortdesc}



### Wie stelle ich sicher, dass meine Services mit den richtigen Bereitstellungen verbunden und betriebsbereit sind?
{: #services_connected}

Bei den meisten Services fügen Sie Ihrer `.yaml`-Servicedatei einen Selektor hinzu, sodass sie auf Pods angewendet wird, die Ihre Apps mit dieser Bezeichnung ausführen. In vielen Fällen soll Ihre App beim ersten Starten nicht sofort Anforderungen verarbeiten. Fügen Sie Ihrer Bereitstellung eine Bereitschaftsprüfung hinzu, sodass Datenverkehr nur an einen Pod gesendet wird, der als bereit eingestuft wurde. Ein Beispiel für eine Bereitstellung mit einem Service, der Bezeichnungen verwendet und eine Bereitschaftsprüfung festlegt, finden Sie unter [NGINX YAML](https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/deploy-apps-clusters/nginx_preferredAntiAffinity.yaml).
{: shortdesc}

In einigen Fällen soll der Service keine Bezeichnung verwenden. Zum Beispiel haben Sie eine externe Datenbank haben oder möchten, dass der Service auf einen anderen Service in einem anderen Namensbereich innerhalb des Clusters verweist. In diesem Fall müssen Sie manuell ein Endpunktobjekt hinzufügen und es mit dem Service verknüpfen.


### Wie steuere ich den Netzverkehr unter den Services, die in meinem Cluster ausgeführt werden?
{: #services_network_traffic}

Standardmäßig können Pods mit anderen Pods im Cluster kommunizieren. Sie können aber den Datenverkehr zu bestimmten Pods oder Namensbereichen mithilfe von Netzrichtlinien blockieren. Wenn Sie Ihre App extern über einen NodePort-, Load-Balancer- oder Ingress-Service zugänglich machen, möchten Sie vielleicht zusätzlich erweiterte Netzrichtlinien einrichten, die den Datenverkehr blockieren. In {{site.data.keyword.containerlong_notm}} können Sie Calico verwenden, um die Kubernetes- und Calico-[Netzrichtlinien zur Steuerung des Datenverkehrs](/docs/containers?topic=containers-network_policies#network_policies) zu verwalten.

Wenn Sie über verschiedene Microservices verfügen, die auf mehreren Plattformen ausgeführt werden, für die Sie den Netzverkehr herstellen, verwalten und sichern müssen, ziehen Sie ein Servicenetztool wie z. B. das [verwaltete Istio-Add-on](/docs/containers?topic=containers-istio) in Betracht.

Sie können auch [Edge-Knoten einrichten](/docs/containers?topic=containers-edge#edge), um die Sicherheit und Isolation Ihres Clusters zu erhöhen, indem Sie die Netzauslastung auf die Auswahl der Worker-Knoten beschränken.



### Wie mache ich meine Services im Internet zugänglich?
{: #services_expose_apps}

Sie können drei Typen von Services für den externen Netzbetrieb erstellen: NodePort, LoadBalancer und Ingress. Weitere Informationen hierzu finden Sie unter [Netzservices planen](/docs/containers?topic=containers-cs_network_planning#external).

Bei der Planung, wie viele `Service`-Objekte Sie in Ihrem Cluster benötigen, berücksichtigen Sie, dass Kubernetes für die Bearbeitung von Netzbetriebs- und Portweiterleitungsregeln `iptables` verwendet. Wenn Sie in Ihrem Cluster eine große Anzahl von Services, vielleicht 5000, ausführen, kann die Leistung beeinträchtigt sein.



## App-Workloads in Clustern bereitstellen
{: #deployments}

Mit Kubernetes deklarieren Sie viele Typen von Objekten in YAML-Konfigurationsdateien, z. B. Pods, Bereitstellungen und Jobs. Diese Objekte beschreiben beispielsweise, welche containerisierten Apps ausgeführt werden, welche Ressourcen sie verwenden und von welchen Richtlinien ihr Verhalten für erneutes Starten, Aktualisieren, Replizieren und vieles mehr verwaltet wird. Weitere Informationen finden Sie in der Kubernetes-Dokumentation zu den [Best Practices für die Konfiguration ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/configuration/overview/).
{: shortdesc}

### Ich dachte, ich müsste meine App in einem Container anordnen. Warum ist jetzt von Pods die Rede?
{: #deploy_pods}

Ein [Pod ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/workloads/pods/pod/) ist die kleinste bereitstellbare Einheit, die Kubernetes verwalten kann. Sie legen Ihren Container (oder eine Gruppe von Containern) in einen Pod und verwenden die Pod-Konfigurationsdatei, um dem Pod mitzuteilen, wie der Container ausgeführt werden und wie er Ressourcen mit anderen Pods gemeinsam nutzen soll. Was immer Sie in einen Pod einlegen, läuft in einem gemeinsamen Kontext, d.h. sie arbeiten synchron auf derselben virtuellen oder physischen Maschine.
{: shortdesc}

**Was in einem Container angeordnet wird**: Überlegen Sie für jede Komponente Ihrer Anwendung, ob sie deutlich andere Ressourcenanforderungen in Hinblick auf CPU und Speicher hat als die anderen. Könnten einige Komponenten auch mit geringerem Aufwand betrieben werden, sodass Ressourcen in andere Bereiche umgeleitet werden können? Bildet eine Komponente eine Schnittstelle zum Kunden, weshalb sie unbedingt laufen muss? Teilen Sie sie in separate Container auf. Sie können sie immer im selben Pod bereitstellen, sodass sie synchron laufen.

**Was in einem Pod angeordnet wird**: Die Container für Ihre App müssen sich immer im selben Pod befinden. Wenn Sie eine Komponente haben, die statusabhängig und schwer skalierbar ist, wie z. B. ein Datenbankservice, stellen Sie sie in einen anderen Pod, den Sie auf einem Workerknoten mit mehr Ressourcen zur Verarbeitung der Workload planen können. Wenn Ihre Container auch dann ordnungsgemäß funktionieren, wenn sie auf verschiedenen Workerknoten ausgeführt werden, verwenden Sie mehrere Pods. Wenn sie auf derselben Maschine ausgeführt und zusammen skaliert werden müssen, gruppieren Sie die Container im selben Pod.

### Wenn ich also einfach einen Pod verwenden kann, wozu dann alle diese unterschiedlichen Objekttypen?
{: #deploy_objects}

Die Erstellung einer YAML-Datei für einen Pod ist einfach. Sie können eine wie folgt mit nur wenigen Zeilen schreiben.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx
    ports:
    - containerPort: 80
```
{: codeblock}

Aber das ist noch nicht alles. Wenn der Knoten, auf dem der Pod ausgeführt wird, ausfällt, fällt Ihr Pod zusammen mit dem Knoten aus und wird nicht neu geplant. Verwenden Sie stattdessen eine [Bereitstellung ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/), um das erneute Planen des Pods, Replikatgruppen und rollierende Aktualisierungen zu unterstützen. Eine Basisbereitstellung ist fast so einfach wie ein Pod. Statt den Container in der `Spezifikation` selbst zu definieren, geben Sie jedoch `Replikate` und eine `Vorlage` in `Spezifikation` der Bereitstellung an. Die Vorlage verfügt über eine eigene `Spezifikation` für die Container darin, die z. B. wie folgt aussehen kann.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 80
```
{: codeblock}

Sie können Features wie Anti-Affinität für den Pod oder Ressourcengrenzwerte alle in derselben YAML-Datei hinzufügen.

Eine ausführlichere Erläuterung der verschiedenen Funktionen, die Sie Ihrer Bereitstellung hinzufügen können, finden Sie unter [YAML-Datei für Ihre App-Bereitstellung erstellen](/docs/containers?topic=containers-app#app_yaml).
{: tip}

### Wie kann ich meine Bereitstellungen so organisieren, dass sie sich einfacher aktualisieren und verwalten lassen?
{: #deploy_organize}

Nachdem Sie nun eine gute Vorstellung davon haben, was Sie in Ihre Bereitstellung einbeziehen können, fragen Sie sich vielleicht, wie Sie alle diese unterschiedlichen YAML-Dateien verwalten sollen. Ganz zu schweigen von den Objekten, die diese in Ihrer Kubernetes-Umgebung erzeugen.

Einige Tipps zum Organisieren der YAML-Dateien für Ihre Bereitstellung:
*  Verwenden Sie ein System zur Versionssteuerung wie z. B. Git.
*  Gruppieren Sie eng zusammengehörige Kubernetes-Ojbkete in einer einzigen YAML-Datei. Wenn Sie beispielsweise eine `Bereitstellung` erstellen, können Sie auch die `Service`-Datei zur YAML hinzufügen. Trennen Sie Objekte wie folgt mit `---`:
    ```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
    ...
    ---
    apiVersion: v1
    kind: Service
    metadata:
    ...
    ```
    {: codeblock}
*  Für die Anwendung auf ein gesamtes Verzeichnis und nicht nur auf eine einzelne Datei können Sie den Befehl `kubectl anwenden -f` verwenden.

Innerhalb der YAML-Datei können Sie Bezeichnungen oder Annotationen als Metadaten verwenden, um Ihre Bereitstellungen zu verwalten.

**Bezeichnungen**: [Bezeichnungen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/) sind `key:value`-Paare (Schlüsssel/Wert-Paare), die an Kubernetes-Objekte angehängt werden können, wie z. B. Pods oder Bereitstellungen. Dabei kann es sich um Elemente aller Art handeln. Sie sind hilfreich bei der Auswahl von Objekten auf Grundlage der Bezeichnungsinformationen. Bezeichnungen bilden die Grundlage für die Gruppierung von Objekten. Einige Ideen für Bezeichnungen:
* `app: nginx`
* `version: v1`
* `env: dev`

**Annotationen**: [Annotationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/overview/working-with-objects/annotations/) ähneln Bezeichnungen insofern, als es sich bei ihnen ebenfalls um `key:value`-Paare handelt. Sie eignen sich besser für nicht identifizierende Informationen, die von Tools oder Bibliotheken genutzt werden können, wie z. B. das Halten zusätzlicher Informationen darüber, woher ein Objekt stammt, wie das Objekt verwendet wird, Zeiger auf zugehörige Aufzeichnungsrepositorys oder eine Richtlinie über das Objekt. Es werden keine Objekte auf der Basis von Annotationen ausgewählt.

### Was kann ich sonst tun, um meine App für die Bereitstellung vorzubereiten?
{: #deploy_prep}

Viele Dinge! Siehe [Containerisierte App für die Ausführung in Clustern vorbereiten](/docs/containers?topic=containers-app#plan_apps). Das Thema enthält Informationen zu folgenden Punkten:
*  Die Arten von Apps, die Sie in Kubernetes ausführen können, einschließlich Tipps für statusabhängige und statusunabhängige Apps.
*  Die Migration von Apps nach Kubernetes.
*  Die Dimensionierung des Clusters auf Grundlage Ihrer Workloadanforderungen.
*  Das Einrichten zusätzlicher App-Ressourcen wie z. B. IBM Services, Speicher, Protokollierung und Überwachung.
*  Die Verwendung von Variablen innerhalb Ihrer Bereitstellung.
*  Die Steuerung des Zugriffs auf Ihre App.

<br />


## Anwendung paketieren
{: #packaging}

Wenn Sie Ihre App in mehreren Clustern, öffentlichen und privaten Umgebungen oder sogar mit mehreren Cloud-Providern ausführen möchten, fragen Sie sich vielleicht, wie Sie in solchen Umgebungen Ihre Bereitstellungsstrategie gestalten können. Mit {{site.data.keyword.Bluemix_notm}} und anderen Open-Source-Tools können Sie Ihre Anwendung paketieren und damit die Automatisierung von Bereitstellungen unterstützen.
{: shortdesc}

<dl>
<dt>Infrastruktur automatisieren</dt>
  <dd>Sie können das Open-Source-Tool [Terraform](/docs/terraform?topic=terraform-getting-started#getting-started) verwenden, um die Bereitstellung der {{site.data.keyword.Bluemix_notm}}-Infrastruktur, einschließlich Kubernetes-Clustern, zu automatisieren. Folgen Sie diesem Lernprogramm, um [Bereitstellungsumgebungen zu planen, zu erstellen und zu aktualisieren](/docs/tutorials?topic=solution-tutorials-plan-create-update-deployments#plan-create-update-deployments). Nachdem Sie einen Cluster erstellt haben, können Sie auch den [{{site.data.keyword.containerlong_notm}}-Cluster-Autoscaler](/docs/containers?topic=containers-ca) so konfigurieren, dass Ihr Worker-Pool die Workerknoten in Abhängigkeit von den Ressourcenanforderungen Ihrer Workload auf- oder abskaliert.</dd>
<dt>Pipeline für kontinuierliche Integration und Continuous Delivery (CI/CD) einrichten</dt>
  <dd>Mit Ihren App-Konfigurationsdateien, die in einem Quellcodeverwaltungssystem wie Git organisiert sind, können Sie Ihre Pipeline erstellen und damit Code in verschiedenen Umgebungen, z. B. `test` und `prod`, testen und bereitstellen. Informieren Sie sich hierzu in [diesem Lernprogramm zu Continuous Deployment in Kubernetes](/docs/tutorials?topic=solution-tutorials-continuous-deployment-to-kubernetes#continuous-deployment-to-kubernetes).</dd>
<dt>App-Konfigurationsdateien paketieren</dt>
  <dd>Mit dem Kubernetes-Paketmanager [Helm ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://helm.sh/docs/) können Sie alle Kubernetes-Ressourcen, die Ihre App erfordert, in einem Helm-Diagramm angeben. Anschließend können Sie mit Helm die YAML-Konfigurationsdateien erstellen und diese Dateien in Ihrem Cluster bereitstellen. Sie können auch [von {{site.data.keyword.Bluemix_notm}} bereitgestellte Helm-Diagramme integrieren ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/kubernetes/solutions/helm-charts), um die Fähigkeiten Ihres Clusters zu erweitern, z. B. durch ein Blockspeicher-Plug-in.<p class="tip">Suchen Sie nach einer einfachen Möglichkeit, YAML-Dateivorlagen zu erstellen? Einige Benutzer verwenden Helm genau dafür. Sie können aber auch andere Community-Tools wie [`ytt`![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://get-ytt.io/) ausprobieren.</p></dd>
</dl>

<br />


## App auf dem neuesten Stand halten
{: #updating}

Sie haben sehr viel Arbeit in die Vorbereitung der nächsten Version Ihrer App gesteckt. Verwenden Sie die Aktualisierungs-Tools von {{site.data.keyword.Bluemix_notm}} und Kubernetes, um sicherzustellen, dass Ihre App in einer sicheren Clusterumgebung ausgeführt wird, und führen Sie Rollouts für die verschiedenen Versionen Ihrer App durch.
{: shortdesc}

### Wie sorge ich dafür, dass mein Cluster unterstützt bleibt?
{: #updating_kube}

Stellen Sie sicher, dass Ihr Cluster jederzeit eine [unterstützte Kubernetes-Version](/docs/containers?topic=containers-cs_versions#cs_versions) ausführt. Wenn eine neue Nebenversion von Kubernetes freigegeben wird, ist eine ältere Version kurze Zeit später veraltet und wird dann nicht mehr unterstützt. Weitere Informationen hierzu finden Sie unter [Kubernetes-Master aktualisieren](/docs/containers?topic=containers-update#master) und [Workerknoten](/docs/containers?topic=containers-update#worker_node).

### Welche App-Aktualisierungsstrategien kann ich anwenden?
{: #updating_apps}

Um Ihre App zu aktualisieren, können Sie aus einer Vielzahl von Strategien auswählen, wie z. B. den folgenden. Sie können mit einer rollierenden Bereitstellung oder sofortigen Umschaltung beginnen, bevor Sie sich mit einer komplexeren Canary-Bereitstellung beschäftigen.

<dl>
<dt>Rollierende Bereitstellung</dt>
  <dd>Sie können Kubernetes-native Funktionen verwenden, um eine `v2`-Bereitstellung zu erstellen und die vorherige `v1`-Bereitstellung nach und nach zu ersetzen. Dieser Ansatz setzt voraus, dass die Apps rückwärtskompatibel sind, sodass Benutzer, denen die `v2`-Version der App bereitgestellt wird, nicht vor problematischen Veränderungen stehen. Weitere Informationen finden Sie unter [Laufende Bereitstellungen zum Aktualisieren der Apps verwalten](/docs/containers?topic=containers-app#app_rolling).</dd>
<dt>Sofortige Umschaltung</dt>
  <dd>Eine sofortige Umschaltung, auch als Blue-Green Deployment bezeichnet, erfordert die doppelten Rechenressourcen, um zwei Versionen einer App gleichzeitig auszuführen. Mit diesem Ansatz können Sie Ihre Benutzer nahezu in Echtzeit auf die neuere Version umschalten. Stellen Sie sicher, dass Sie Selektoren für Servicebezeichnungen verwenden (z. B. `version: green` oder `version: blue`), damit Anforderungen an die richtige App-Version gesendet werden. Sie können die neue Bereitstellung mit `Version: green` erstellen, warten, bis sie bereit ist, und anschließend die Bereitstellung mit `Version: blue` löschen. Alternativ können Sie eine [rollierende Aktualisierung](/docs/containers?topic=containers-app#app_rolling) durchführen, dabei aber den Parameter `maxUnavailable` auf `0%` und den Parameter `maxSurge` auf `100%` setzen.</dd>
<dt>Canary- oder A/B-Bereitstellung</dt>
  <dd>Eine Canary-Bereitstellung ist eine komplexere Aktualisierungsstrategie und besteht darin, dass Sie einen Prozentsatz von Benutzern wie z. B. 5% wählen und diese an die neue App-Version senden. Sie erfassen Metriken in Ihren Protokollierungs- und Überwachungstools zur Leistung der neuen App-Version, führen A/B-Tests durch und implementieren dann die Aktualisierung für weitere Benutzer. Wie bei allen Bereitstellungen ist die Bezeichnung der App (z. B. mit `version: stable` und `version: canary`) von entscheidender Bedeutung. Zur Verwaltung von Canary-Bereitstellungen könnten Sie [das Servicenetz für das veraltete Istio-Add-on installieren](/docs/containers?topic=containers-istio#istio), [Sysdig-Überwachung für Ihren Cluster einrichten](/docs/services/Monitoring-with-Sysdig/tutorials?topic=Sysdig-kubernetes_cluster#kubernetes_cluster) und anschließend das Istio-Servicenetz für A/B-Tests verwenden, wie [in diesem Blog-Post beschrieben ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://sysdig.com/blog/monitor-istio/).</dd>
</dl>

<br />


## Clusterleistung überwachen
{: #monitoring_health}

Durch die effektive Protokollierung und Überwachung Ihres Clusters und Ihrer Apps können Sie Ihre Umgebung besser verstehen, die Ressourcennutzung entsprechend optimieren und eventuelle Probleme beheben. Informationen zum Einrichten von Protokollierungs- und Überwachungslösungen für Ihren Cluster finden Sie unter [Protokollierung und Überwachung](/docs/containers?topic=containers-health#health).
{: shortdesc}

Berücksichtigen Sie bei der Einrichtung Ihrer Protokollierung und Überwachung die folgenden Aspekte.

<dl>
<dt>Protokolle und Metriken erfassen, um die Clustersicherheit zu ermitteln</dt>
  <dd>Kubernetes enthält einen Metrikserver, der die Ermittlung der grundlegenden Leistung auf Clusterebene unterstützt. Diese Metriken finden Sie in Ihrem [Kubernetes-Dashboard](/docs/containers?topic=containers-app#cli_dashboard) oder in einem Terminal, indem Sie die Befehle `kubectl top (pods | nodes)` ausführen. Sie können diese Befehle in Ihre Automatisierung aufnehmen.<br><br>
  Leiten Sie Protokolle an ein Protokollanalysetool weiter, damit Sie Ihre Protokolle später analysieren können. Definieren Sie die Ausführlichkeit und die Protokollierungsstufe, um zu vermeiden, dass mehr Protokolle als benötigt gespeichert werden. Protokolle können schnell sehr viel Speicherplatz einnehmen, was sich auf Ihre App-Leistung auswirken und die Protokollanalyse erschweren kann.</dd>
<dt>Leistung der Test-App</dt>
  <dd>Nachdem Sie die Protokollierung und Überwachung eingerichtet haben, führen Sie Leistungstests durch. In einer Testumgebung erstellen Sie bewusst verschiedene nicht ideale Szenarios, z. B. das Löschen aller Workerknoten in einer Zone, um einen Zonenfehler zu replizieren. Überprüfen Sie die Protokolle und Metriken, um zu sehen, wie Ihre App wiederhergestellt wird.</dd>
<dt>Vorbereitung auf Audits</dt>
  <dd>Zusätzlich zu den App-Protokollen und Clustermetriken möchten Sie die Aktivitätenverfolgung so konfigurieren, dass Sie einen überprüfbaren Datensatz darüber erhalten, wer welche Cluster- und Kubernetes-Aktionen ausgeführt hat. Weitere Informationen hierzu finden Sie unter [{{site.data.keyword.cloudaccesstrailshort}}](/docs/containers?topic=containers-at_events#at_events).</dd>
</dl>
