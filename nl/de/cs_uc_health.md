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



# Anwendungsfälle aus dem Gesundheitswesen für {{site.data.keyword.cloud_notm}}
{: #cs_uc_health}

Diese Anwendungsfälle verdeutlichen, wie Workloads für {{site.data.keyword.containerlong_notm}} von der öffentlichen Cloud profitieren. Sie ermöglichen eine sichere Rechenleistung auf isolierten Bare-Metal-Maschinen, eine leichte Bereitstellung der Cluster für schnellere Entwicklung, die Migration von virtuellen Maschinen und die gemeinsame Datennutzung in Cloud-Datenbanken.
{: shortdesc}

## Ein Anbieter aus dem Gesundheitswesen migriert Workloads von ineffizienten virtuellen Maschinen auf einfach zu betreibende Container für Berichterstellungs- und Patientensysteme.
{: #uc_migrate}

Eine IT-Führungskraft eines Anbieters aus dem Gesundheitswesen ist für lokale Systeme zum Erstellen von Geschäftsberichten und lokale Patientensysteme zuständig. Die Zyklen für die Verbesserung dieser Systeme sind langsam, was dazu führt, dass auch die Servicequalität für die Patienten stagniert.
{: shortdesc}

Warum {{site.data.keyword.cloud_notm}}: Da er den Patientenservice verbessern wollte, informierte sich der Anbieter über {{site.data.keyword.containerlong_notm}} und {{site.data.keyword.contdelivery_full}}, um damit auf einer sicheren Plattform die IT-Ausgaben zu reduzieren und die Entwicklung zu beschleunigen. Für die stark genutzten SaaS-Systeme des Anbieters, die sowohl für die Systeme zum Speichern der Patientendatensätze als auch für die Apps zum Erstellen der Geschäftsberichte verwendet wurden, waren häufig Aktualisierungen erforderlich. Doch die lokale Umgebung verhinderte eine agile Entwicklung. Außerdem wollte der Provider den steigenden Personalkosten und dem sinkenden Budget entgegenwirken.

Schlüsseltechnologien:
* [Cluster, die den unterschiedlichen Anforderungen an CPU, RAM und Speicher entsprechen](/docs/containers?topic=containers-plan_clusters#shared_dedicated_node)
* [Horizontale Skalierung](/docs/containers?topic=containers-app#highly_available_apps)
* [Containersicherheit und -isolierung](/docs/containers?topic=containers-security#security)
* [Native DevOps-Tools, einschließlich offener Toolchains in {{site.data.keyword.contdelivery_full}}](https://www.ibm.com/cloud/garage/toolchains/)
* [SDK für Node.js](/docs/runtimes/nodejs?topic=Nodejs-nodejs_runtime#nodejs_runtime)
* [Anmeldefunktionalität ohne App-Code-Änderungen durch {{site.data.keyword.appid_short_notm}}](/docs/services/appid?topic=appid-getting-started)

Zunächst wurden die SaaS-Systeme containerisiert und in die Cloud gestellt. In diesem ersten Schritt wurde die überdimensionierte Hardware in einem privaten Rechenzentrum durch eine anpassbare Rechenleistung ersetzt, wodurch der IT-Betrieb, die Wartung und die Energiekosten reduziert wurden. Zum Hosten der SaaS-Systeme wurden einfach Kubernetes-Cluster entworfen, die an die jeweiligen Anforderungen an CPU, RAM und Speicher angepasst wurden. Ein weiterer Faktor für niedrigere Personalkosten ist die Tatsache, dass Kubernetes von IBM verwaltet wird, sodass sich der Anbieter auf die Bereitstellung eines besseren Kundendienstes konzentrieren kann.

Für die IT-Führungskraft ist die beschleunigte Entwicklung eine bedeutende Verbesserung. Durch die Verschiebung in die öffentliche Cloud können die Entwickler ohne großen Aufwand mit dem Node.js-SDK experimentieren und Änderungen mit Push-Operationen zu Bereitstellungs- und Testsystemen hinzufügen, die mit separaten Clustern horizontal skaliert werden. Diese Push-Operationen wurden mit offenen Toolchains und {{site.data.keyword.contdelivery_full}} automatisiert. Die Aktualisierungen des SaaS-Systems werden nicht mehr in langsamen, fehleranfälligen Erstellungsprozessen durchgeführt. Die schrittweisen Aktualisierungen können den Benutzern von den Entwicklern täglich oder sogar noch häufiger bereitgestellt werden.  Außerdem wurde schnell eine Protokollierung und Überwachung für die SaaS-Systeme in das System integriert, besonders zur Interaktion der Front-End- und Back-End-Berichte der Patienten. Die Entwickler verschwenden ihre Zeit nicht mit dem Erstellen komplexer Protokollierungssysteme, nur um in der Lage zu sein, Fehler in den aktiven Systemen zu beheben.

Sicherheit zuerst: Bei Verwendung von Bare-Metal für {{site.data.keyword.containerlong_notm}} verfügen die sensiblen Patienten-Workloads jetzt über die übliche Isolation, gleichzeitig aber auch über die Flexibilität der öffentlichen Cloud. Von Bare-Metal wird Trusted Compute zur Überprüfung der zugrunde liegenden Hardware auf Manipulationen bereitgestellt. Hierfür werden von Vulnerability Advisor Scans zur Verfügung gestellt:
* Scans auf Sicherheitslücken für Images
* Scans für Richtlinien, die auf ISO 27k basieren
* Live-Scans für Container
* Paket-Scans für bekannte Malware

Sichere Patientendaten sorgen für zufriedene Patienten.

**Kontext: Workload-Migration für Anbieter aus dem Gesundheitswesen**

* Eine unzulängliche Technik und lange Release-Zyklen behindern die geschäftskritische Patientenverwaltungs- und Berichterstellungssysteme des Anbieters.
* Die angepassten Back-Office- und Front-Office-Apps werden lokal in monolithischen Images der virtuellen Maschinen bereitgestellt.
* Die Prozesse, Methoden und Tools müssen modernisiert werden, aber das Wissen für die Herangehensweise fehlt.
* Die Gefahren aufgrund unzulänglicher Technik nehmen zu, was sogar dazu führt, dass keine qualitativ hochwertige Software beschafft wird, um mit den Anforderungen des Markts Schritt halten zu können.
* Die Sicherheit ist eine große Herausforderung und erschwert die Bereitstellung noch mehr, was sogar zu noch mehr Verzögerungen führt.
* Die Budgets für Investitionen unterliegen einer strikten Kontrolle und bei den IT-Mitarbeitern entsteht der Eindruck, dass weder ausreichend Geld noch Mitarbeiter vorhanden sind, um mit den unternehmensinternen Systemen die erforderlichen Test- und Stagingumgebungen zu erstellen.

**Lösungsmodell**

Bedarfsgerechte Rechenkapazität, Speicherkapazität und E/A-Services in der öffentlichen Cloud mit sicherem Zugriff auf die lokalen Unternehmensassets. Implementieren Sie einen CI/CD-Prozess und weitere Teile von IBM Garage Method, um die Bereitstellungszyklen deutlich zu reduzieren.

**Schritt 1: Rechenplattform sichern**
* Apps, die hochsensible Patientendaten verwalten, können in {{site.data.keyword.containerlong_notm}} mithilfe von Bare Metal für Trusted Compute ausgeführt werden.
* Mit Trusted Compute kann die zugrunde liegende Hardware auf Manipulationen überprüft werden.
* Auf dieser Grundlage werden von Vulnerability Advisor Schwachstellensuchen für Images, Richtlinien, Container und Paketierung auf bekannte Malware durchgeführt.
* Sie können eine richtlinienbasierte Authentifizierung für Ihre Services und APIs durch eine einfache Ingress-Annotation konsistent durchsetzen. Durch deklarative Sicherheit können Sie die Benutzerauthentifizierung und die Tokenvalidierung mithilfe von {{site.data.keyword.appid_short_notm}} sicherstellen.

**Schritt 2: Lift-and-shift**
* Migrieren Sie die Images der virtuellen Maschine auf Container-Images, die in {{site.data.keyword.containerlong_notm}} in der öffentlichen Cloud ausgeführt werden.
* Stellen Sie standardisierte DevOps-Dashboards und -Verfahren über Kubernetes bereit.
* Aktivieren Sie die bedarfsgerechte Skalierung der Rechenleistung für Batch-Workloads und weitere Back-Office-Workloads, die nicht häufig ausgeführt werden.
* Verwenden Sie {{site.data.keyword.SecureGatewayfull}} zur Verwaltung der sicheren Verbindungen zum lokalen DBMS.
* Die Kosten für private Rechenzentren bzw. Investitionen vor Ort werden erheblich reduziert und durch ein Modell für Utility-Computing ersetzt, das abhängig vom Workloadbedarf skaliert wird.

**Schritt 3: Microservices und Garage Method**
* Organisieren Sie die Apps in einer Gruppe aus kooperativen Microservices. Diese Gruppe wird in {{site.data.keyword.containerlong_notm}} abhängig von den Funktionsbereichen der App mit den meisten Qualitätsproblemen ausgeführt.
* Verwenden Sie {{site.data.keyword.cloudant}} mit den vom Kunden bereitgestellten Schlüsseln zum Zwischenspeichern der Daten in der Cloud.
* Wenden Sie die CI/CD-Verfahren (CI/CD - Continuous Integration and Delivery) an, damit die Entwickler die Versionen und Releases eines Microservice bei Bedarf nach eigenen Plänen steuern können. {{site.data.keyword.contdelivery_full}} bietet Workflow-Toolchains für den CI/CD-Prozess sowie Imageerstellung und Schwachstellensuche für Container-Images.
* Übernehmen Sie die agilen und iterativen Entwicklungsverfahren von IBM Garage Method, um häufige Releases neuer Funktionen, Patches und Korrekturen ohne Ausfallzeit zu ermöglichen.

**Technische Lösung**
* {{site.data.keyword.containerlong_notm}}
* {{site.data.keyword.cloudant}}
* {{site.data.keyword.SecureGatewayfull}}
* {{site.data.keyword.appid_short_notm}}

Die sensibelsten Workloads können in Clustern in {{site.data.keyword.containerlong_notm}} auf Bare-Metal-Maschinen gehostet werden.  So wird eine sichere Rechenplattform bereitgestellt, in der automatisch Hardware und Laufzeitcode auf Schwachstellen durchsucht werden. Mithilfe standardisierter Container-Technologie können Apps anfangs von {{site.data.keyword.containerlong_notm}} ohne größere architektonische Änderungen erneut gehostet werden. Diese Änderung bietet den unmittelbaren Vorteil der Skalierbarkeit.

Die Apps können mit definierten Regeln und dem automatisierten Kubernetes-Orchestrator repliziert und skaliert werden. {{site.data.keyword.containerlong_notm}} stellt skalierbare Rechenressourcen und die zugehörigen DevOps-Dashboards zum bedarfsgerechten Erstellen, Skalieren und Umrüsten von Apps und Services bereit. Mit den Bereitstellungs- und Laufzeitobjekten von Kubernetes kann der Anbieter die Upgrades für die Apps zuverlässig überwachen und verwalten.

{{site.data.keyword.SecureGatewayfull}} wird zum Erstellen einer sicheren Pipeline zu lokalen Datenbanken und Dokumenten für Apps verwendet, die zur Ausführung in {{site.data.keyword.containerlong_notm}} erneut gehostet werden.

{{site.data.keyword.cloudant}} ist eine moderne NoSQL-Datenbank, die für eine Reihe datengesteuerter Anwendungsfälle genutzt werden kann: von Schlüsselwerten bis zu komplexen dokumentorientierten Datenspeichern und Abfragen. Zum Minimieren der Abfragen des Back-Office-RDBMS wird {{site.data.keyword.cloudant}} verwendet, um die Sitzungsdaten des Benutzers in den Apps zwischenzuspeichern. Diese Auswahlmöglichkeiten verbessern die durchgehende Benutzerfreundlichkeit und Leistung in allen Apps für {{site.data.keyword.containerlong_notm}}.

Das Verschieben der Rechenworkloads in {{site.data.keyword.cloud_notm}} reicht nicht aus. Der Anbieter muss auch eine Umwandlung der Prozesse und Methoden durchführen. Bei Übernahme der Verfahren von IBM Garage Method kann der Anbieter einen agilen und iterativen Bereitstellungsprozess implementieren, von dem moderne DevOps-Verfahren wie Continuous Integration and Delivery (CI/CD) unterstützt werden.

Ein großer Teil des CI/CD-Prozesses wird in der Cloud unter Verwendung des IBM Service 'Continuous Delivery' automatisiert. Der Anbieter kann die Workflow-Toolchains zum Vorbereiten der Container-Images definieren, eine Überprüfung auf Schwachstellen durchführen und eine Bereitstellung im Kubernetes-Cluster ausführen.

**Ergebnisse**
* Das Verschieben der vorhandenen monolithischen virtuellen Maschinen in Container, die in der Cloud gehostet werden, war ein erster Schritt, der es dem Anbieter ermöglichte, Kapitalkosten zu sparen und das Erlernen moderner DevOps-Verfahren zu beginnen.
* Durch das Umorganisieren der monolithischen Apps in differenzierte Microservices wird die Bereitstellungszeit für Patches, Fehlerkorrekturen und neue Funktionen erheblich reduziert.
* Parallel dazu implementierte der Anbieter in bestimmten Zeitfenstern einfache Iterationen, um Beeinträchtigungen durch unzulängliche technische Umstände zu minimieren.

## Gemeinnützige Forschungseinrichtung hostet vertrauliche Daten sicher und weitet Forschungszusammenarbeit mit Partnern aus
{: #uc_research}

In der Entwicklungsabteilung einer gemeinnützigen medizinischen Forschungseinrichtung können die Forscher aus Industrie und Wissenschaft die Forschungsdaten nur mit großen Aufwand gemeinsam nutzen. Aufgrund regionaler Regelungen zur Einhaltung von Vorschriften und zentralisierter Datenbanken ist ihre Arbeit auf Bereiche aufgeteilt und auf der Welt verstreut.
{: shortdesc}

Warum {{site.data.keyword.cloud_notm}}: {{site.data.keyword.containerlong_notm}} bietet eine sichere Rechenleistung zum Hosten sensibler Daten und einer leistungsfähigen Datenverarbeitung auf einer offenen Plattform. Diese globale Plattform wird in nahe gelegenen Regionen gehostet. Somit ist sie an lokale Regelungen gebunden, die bei den Patienten und Forschern das Vertrauen schaffen, dass ihre Daten lokal geschützt sind und zu einer Verbesserung der Gesundheitslage beitragen.

Schlüsseltechnologien:
* [Durch intelligentes Planen werden die Workloads nach Bedarf ausgeführt](/docs/containers?topic=containers-regions-and-zones#regions-and-zones)
* [{{site.data.keyword.cloudant}} zum Beibehalten und Synchronisieren von Daten für Apps](/docs/services/Cloudant?topic=cloudant-getting-started#getting-started)
* [Schwachstellensuche und Isolation für Workloads](/docs/services/Registry?topic=va-va_index#va_index)
* [Native DevOps-Tools, einschließlich offener Toolchains in {{site.data.keyword.contdelivery_full}}](https://www.ibm.com/cloud/garage/toolchains/)
* [{{site.data.keyword.openwhisk}} zum Bereinigen der Daten und Benachrichtigen der Forscher über Änderungen an der Datenstruktur](/docs/openwhisk?topic=cloud-functions-openwhisk_cloudant#openwhisk_cloudant)

**Kontext: Sicheres Hosten und gemeinsames Nutzen von Krankheitsdaten für gemeinnützige Forschungseinrichtung**

* Unterschiedliche Gruppen von Forschern aus verschiedenen Institutionen verfügen nicht über eine einheitliche Möglichkeit zum gemeinsamen Nutzen von Daten, was die Zusammenarbeit verlangsamt.
* Sicherheitsprobleme erschweren die Zusammenarbeit und führen dazu, dass noch weniger Forschungsdaten gemeinsam genutzt werden.
* Da die Entwickler und Forscher über den ganzen Globus und Organisationsgrenzen hinweg verstreut sind, sind PaaS (Platform as a Service) und SaaS (Software as a Service) die beste Option für jede Benutzergruppe.
* Aufgrund von regional unterschiedlichen Gesundheitsvorschriften müssen manche Daten und ein Teil der Datenverarbeitung in der jeweiligen Region verbleiben.

**Die Lösung**

Die gemeinnützige Forschungseinrichtung möchte weltweit verteilte Krebsforschungsdaten zusammenfassen. Deswegen wird eine Abteilung gegründet, die Lösungen für die Forscher erarbeiten soll:
* Einpflegen - Apps zum Einpflegen der Forschungsdaten. Forscher verwenden heutzutage Arbeitsblätter, Dokumente, kommerzielle Produkte sowie proprietäre oder selbst entwickelte Datenbanken zum Aufzeichnen von Forschungsergebnissen. Es ist unwahrscheinlich, dass sich diese Situation durch den Versuch der gemeinnützigen Forschungseinrichtung zum Zentralisieren der Datenanalyse ändern wird.
* Anonymisieren - Apps zum Anonymisieren der Daten. Sensible personenbezogene Daten müssen entfernt werden, damit regionale Gesundheitsvorschriften eingehalten werden.
* Analysieren - Apps zum Analysieren der Daten. Die grundlegende Vorgehensweise besteht darin, die Daten in einem regulären Format zu speichern und anschließend mithilfe von künstlicher Intelligenz, maschinellem Lernen, einfacher Regression, usw. abzufragen und zu verarbeiten.

Die Forscher müssen sich einem regionalen Cluster anschließen und die Daten in die Apps eingepflegt, danach transformiert und anonymisiert werden:
1. Synchronisieren der anonymisierten Daten in den regionalen Clustern oder Senden der Daten an einen zentralen Datenspeicher
2. Verarbeiten der Daten unter Verwendung von maschinellem Lernen, zum Beispiel mit PyTorch, auf Bare-Metal-Workerknoten, von denen Grafik-Verarbeitungseinheiten bereitgestellt werden

**Einpflegen** {{site.data.keyword.cloudant}} wird in jedem regionalen Cluster verwendet, in dem die umfangreichen Datendokumente der Forscher gespeichert werden; Abrufen und Verarbeiten ist bei Bedarf möglich. Von {{site.data.keyword.cloudant}} werden ruhende Daten und Daten bei der Übertragung verschlüsselt, was den regionalen Gesetzen für Datenschutz entspricht.

{{site.data.keyword.openwhisk}} wird zum Erstellen von Verarbeitungsfunktionen verwendet, von denen die Forschungsdaten eingepflegt und als strukturierte Datendokumente in {{site.data.keyword.cloudant}} gespeichert werden. {{site.data.keyword.SecureGatewayfull}} stellt für {{site.data.keyword.openwhisk}} eine einfache Möglichkeit für sicheren und geschützten Zugriff auf lokale Daten bereit.

Die Web-Apps in den regionalen Clustern werden in Node.js für die manuelle Dateneingabe der Ergebnisse, Schemadefinitionen und die Zugehörigkeit der Forschungseinrichtungen entwickelt. IBM Key Protect trägt zum Sichern des Zugriffs auf die {{site.data.keyword.cloudant}}-Daten bei und von IBM Vulnerability Advisor werden die App-Container und Images auf Sicherheitsrisiken gescannt.

**Anonymisieren** Bei jedem Speichern eines neuen Datendokuments in {{site.data.keyword.cloudant}} wird ein Ereignis ausgelöst, die Daten werden von einer Cloud-Funktion anonymisiert und die sensiblen personenbezogenen Daten werden aus dem Datendokument entfernt. Diese anonymisierten Datendokumente werden getrennt von den "unaufbereiteten" Daten gespeichert, die eingepflegt werden; sie sind die einzigen Dokumente, die über die Regionsgrenzen hinweg zu Analysezwecken gemeinsam genutzt werden.

**Analysieren** Da Frameworks für maschinelles Lernen äußerst rechenintensiv sind, richtet die gemeinnützige Einrichtung einen globalen Verarbeitungscluster aus Bare-Metal-Workerknoten ein. Diesem globalen Verarbeitungscluster ist eine aggregierte {{site.data.keyword.cloudant}}-Datenbank mit den anonymisierten Daten zugeordnet. Von einem Cron-Job wird regelmäßig eine Cloud-Funktion ausgelöst, um anonymisierte Datendokumente aus den regionalen Rechenzentren in die {{site.data.keyword.cloudant}}-Instanz des globalen Verarbeitungsclusters zu übertragen.

Vom Rechencluster wird das Framework für maschinelles Lernen unter Verwendung von PyTorch ausgeführt; die Apps für maschinelles Lernen wurden mit Python zum Analysieren der aggregierten Daten geschrieben. Zusätzlich zu den Apps für maschinelles Lernen entwickeln die Forscher in der Verbundgruppe auch ihre eigenen Apps, die im globalen Cluster veröffentlicht und ausgeführt werden können.

Von der gemeinnützigen Einrichtung werden auch Apps bereitgestellt, die nicht auf Bare-Metal-Knoten im globalen Cluster ausgeführt werden. Von den Apps werden die aggregierten Daten und die Ausgabe der Apps für maschinelles Lernen angezeigt und extrahiert. Auf diese Apps kann von einem öffentlichen Endpunkt aus zugegriffen werden, der über das API-Gateway vor Zugriffen aus dem Internet geschützt ist. Danach können Forscher und Datenanalysten aus der ganzen Welt Datenbestände herunterladen und ihre eigenen Analysen durchführen.

**Forschungs-Workloads auf {{site.data.keyword.containerlong_notm}} hosten**

Die Entwickler begannen mit dem Bereitstellen der SaaS-Apps zur gemeinsamen Nutzung der Forschungsergebnisse in Containern mit {{site.data.keyword.containerlong_notm}}. Sie erstellten Cluster für eine Entwicklungsumgebung, die es den weltweiten Entwicklern ermöglicht, die App-Verbesserungen schnell gemeinsam bereitzustellen.

Sicherheit zuerst: Die Führungskraft aus der Entwicklungsabteilung wählt Trusted Compute für Bare-Metal zum Hosten der Forschungscluster aus. Bei Verwendung von Bare-Metal für {{site.data.keyword.containerlong_notm}} verfügen die sensiblen Forschungs-Workloads jetzt über die übliche Isolation, gleichzeitig aber auch über die Flexibilität der öffentlichen Cloud. Von Bare-Metal wird Trusted Compute zur Überprüfung der zugrunde liegenden Hardware auf Manipulationen bereitgestellt. Da die gemeinnützige Einrichtung auch über eine Partnerschaft mit Pharmaunternehmen verfügt, ist die Sicherheit der Apps von entscheidender Bedeutung. Die Konkurrenz ist groß und Wirtschaftsspionage ein ernstes Problem. Zur Gewährleistung der Sicherheit werden von Vulnerability Advisor folgende Scans bereitgestellt:
* Scans auf Sicherheitslücken für Images
* Scans für Richtlinien, die auf ISO 27k basieren
* Live-Scans für Container
* Paket-Scans für bekannte Malware

Sichere Forschungs-Apps führen zu einer höheren Beteiligung bei klinischen Studien.

Um eine globale Verfügbarkeit zu erreichen, werden die Entwicklungs-, Test- und Produktionssysteme auf der ganzen Welt in mehreren Rechenzentren bereitgestellt. Zur Gewährleistung der hohen Verfügbarkeit wird eine Kombination aus Clustern in mehreren geografischen Regionen sowie Mehrzonencluster verwendet. Die Forschungs-App kann unter Einhaltung der EU-Bestimmungen ohne großen Aufwand in den Clustern der Region Frankfurt bereitgestellt werden. Außerdem wird die App in Clustern in den USA bereitgestellt, um eine lokale Verfügbarkeit und Wiederherstellung zu gewährleisten. Auch die Forschungs-Workload wird auf Mehrzonencluster in der Region Frankfurt verteilt, um die Verfügbarkeit der Europäischen App und einen effizienten Lastausgleich für die Workload sicherzustellen. Da die Forscher sensible Daten mit der App für die gemeinsame Nutzung von Forschungsdaten hochladen, werden die Cluster der App in Regionen gehostet, in denen strengere Regelungen gelten.

Die Entwickler konzentrieren sich mithilfe der vorhandenen Tools auf Domänenprobleme: Anstatt eindeutigen Code für maschinelles Lernen (ML) zu schreiben, wird durch das Binden der {{site.data.keyword.cloud_notm}}-Services an die Cluster eine ML-Logik in die Apps integriert. Die Entwickler brauchen sich auch nicht mehr um Infrastrukturmanagementaufgaben kümmern, da Kubernetes- und Infrastruktur-Upgrades, die Sicherheit und vieles mehr von IBM verwaltet werden.

**Die Lösung**

Bedarfsgerechte Rechenkapazität, Speicherkapazität und Node.js-Starter-Kits, die in einer öffentlichen Cloud mit sicherem Zugriff auf die Forschungsdaten auf der ganzen Welt ausgeführt werden. Die Berechnung in Clustern ist manipulationssicher und durch Bare-Metal isoliert.

Technische Lösung:
* {{site.data.keyword.containerlong_notm}} mit Trusted Compute
* {{site.data.keyword.openwhisk}}
* {{site.data.keyword.cloudant}}
* {{site.data.keyword.SecureGatewayfull}}

**Schritt 1: Containerisierte Apps mithilfe von Microservices**
* Verwenden Sie das Node.js-Starter-Kit von IBM für einen Schnelleinstieg in die Entwicklung.
* Strukturieren Sie die Apps in einer Gruppe aus kooperativen Microservices in {{site.data.keyword.containerlong_notm}}; Basis hierfür sind die Funktionsbereiche der App und ihre Abhängigkeiten.
* Stellen Sie die Forschungs-Apps in den Containern in {{site.data.keyword.containerlong_notm}} bereit.
* Stellen Sie standardisierte DevOps-Dashboards über Kubernetes bereit.
* Aktivieren Sie die bedarfsgerechte Skalierung der Rechenleistung für Batch-Workloads und weitere Forschungs-Workloads, die nicht häufig ausgeführt werden.
* Verwenden Sie {{site.data.keyword.SecureGatewayfull}} zur Verwaltung der sicheren Verbindungen zu vorhandenen lokalen Datenbanken.

**Schritt 2: Sichere und leistungsfähige Rechenkapazität verwenden**
* Apps für maschinelles Lernen (ML), für die eine höhere Rechenleistung erforderlich ist, werden mithilfe von {{site.data.keyword.containerlong_notm}} auf Bare-Metal-Maschinen gehostet. Da dieser ML-Cluster zentralisiert ist, fallen in den regionalen Clustern keine Ausgaben für Bare-Metal-Workerknoten an; auch Kubernetes-Bereitstellungen sind einfacher.
* Apps, die hochsensible Klinikdaten verwalten, können über {{site.data.keyword.containerlong_notm}} mithilfe von Bare-Metal für Trusted Compute gehostet werden.
* Mit Trusted Compute kann die zugrunde liegende Hardware auf Manipulationen überprüft werden. Auf dieser Grundlage werden von Vulnerability Advisor Schwachstellensuchen für Images, Richtlinien, Container und Paketierung auf bekannte Malware durchgeführt.

**Schritt 3: Globale Verfügbarkeit sicherstellen**
* Nachdem die Entwickler die Apps in den Entwicklungs- und Testclustern erstellt und getestet haben, verwenden sie die IBM CI/CD-Toolchains zum Bereitstellen von Apps in Clustern auf der ganzen Welt.
* Die in {{site.data.keyword.containerlong_notm}} integrierten Hochverfügbarkeitstools gleichen die Workload in jeder geografischen Region aus; hierbei sind auch automatische Fehlerbehebung und Lastverteilung eingeschlossen.
* Mithilfe der Toolchains und Helm-Bereitstellungstools werden die Apps auch in Clustern auf der ganzen Welt bereitgestellt; die Workloads und Daten erfüllen die regionalen Vorschriften.

**Schritt 4: Gemeinsame Nutzung der Daten**
* {{site.data.keyword.cloudant}} ist eine moderne NoSQL-Datenbank, die für eine Reihe datengesteuerter Anwendungsfälle genutzt werden kann: von Schlüsselwerten bis zu komplexen dokumentorientierten Datenspeichern und Abfragen.
* Zum Minimieren der Abfragen der regionalen Datenbanken wird {{site.data.keyword.cloudant}} verwendet, um die Sitzungsdaten des Benutzers in den Apps zwischenzuspeichern.
* Diese Methode verbessert die durchgehende Benutzerfreundlichkeit und Leistung in allen Apps für {{site.data.keyword.containerlong_notm}}.
* Während die Worker-Apps in {{site.data.keyword.containerlong_notm}} lokale Daten analysieren und die Ergebnisse in {{site.data.keyword.cloudant}} speichern, reagiert {{site.data.keyword.openwhisk}} auf Änderungen und bereinigt automatisch die Daten in eingehenden Datenfeeds.
* Analog können Benachrichtigungen über Forschungserfolge in einer Region über das Hochladen der Daten ausgelöst werden, sodass alle Forscher von den neuen Daten profitieren können.

**Ergebnisse**
* Mit Starter-Kits, {{site.data.keyword.containerlong_notm}} und IBM CI/CD-Tools arbeiten globale Entwickler institutsübergreifend und entwickeln mit gängigen und kompatiblen Tools gemeinsam Forschungs-Apps.
* Mit Microservices wird die Bereitstellungszeit für Patches, Fehlerkorrekturen und neue Funktionen erheblich reduziert. Erstmalige Entwicklungen verlaufen schnell und Aktualisierungen sind häufig.
* Die Forscher verfügen über Zugriff auf klinische Daten und können diese klinischen Daten gemeinsam nutzen, während diese den lokalen Regelungen entsprechen.
* Die Patienten, die an medizinischen Forschungsprogrammen teilnehmen, sind zuversichtlich, dass ihre Daten sicher sind und einen Beitrag zur Forschung leisten, wenn sie von großen Forschungsteams gemeinsam genutzt werden.
