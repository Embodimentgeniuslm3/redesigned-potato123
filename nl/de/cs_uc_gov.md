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



# Anwendungsfälle für Behörden für {{site.data.keyword.Bluemix_notm}}
{: #cs_uc_gov}

Diese Anwendungsfälle verdeutlichen, wie Workloads mithilfe von {{site.data.keyword.containerlong_notm}} von der öffentlichen Cloud profitieren. Diese Workloads verfügen über eine Isolation mit Trusted Compute, befinden sich zur Sicherung der Datenhoheit in globalen Regionen, verwenden das maschinelle Lernen von Watson anstatt neuen Code zu entwickeln und stellen eine Verbindung zu lokalen Datenbanken her.
{: shortdesc}

## Regionale Behörden verbessern Zusammenarbeit und Geschwindigkeit mit Entwicklergemeinschaft, die öffentliche und private Daten kombiniert
{: #uc_data_mashup}

Der Leiter eines Open Government-Programms muss öffentliche Daten gemeinsam mit dem kommunalen und privaten Sektor nutzen, aber die Daten sind in einem monolithischen lokalen System blockiert.
{: shortdesc}

Warum {{site.data.keyword.Bluemix_notm}}: Mit {{site.data.keyword.containerlong_notm}} stellt der Leiter den transformativen Wert der kombinierten öffentlich/privaten Daten bereit. Analog stellt der Service eine öffentliche Cloudplattform bereit, auf der für Microservices ein Refactoring ausgeführt werden kann und die Microservices aus den monolithischen lokalen Apps zugänglich gemacht werden können. Darüber hinaus ermöglicht eine öffentliche Cloud Partnerschaften aus Behörden und Öffentlichkeit die Nutzung externer Cloud-Services und Open-Source-Tools, die die Zusammenarbeit begünstigen.

Schlüsseltechnologien:    
* [Cluster, die den unterschiedlichen Anforderungen an CPU, RAM und Speicher entsprechen](/docs/containers?topic=containers-plan_clusters#shared_dedicated_node)
* [Native DevOps-Tools, einschließlich offener Toolchains in {{site.data.keyword.contdelivery_full}}](https://www.ibm.com/cloud/garage/toolchains/)
* [Bereitstellung von Zugriff auf öffentliche Daten mit {{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-about#about)
* [IBM Cloud Analytics-Plug-and-play-Services](https://www.ibm.com/cloud/analytics)

**Kontext: Behörden verbessern Zusammenarbeit und Geschwindigkeit mit Entwicklergemeinschaft, die öffentliche und private Daten kombiniert**
* Dem Modell der sog. "offenen Verwaltung" (Open Government) gehört die Zukunft, mit ihren lokalen Systemen schafft die oben beschriebene Regierungsbehörde jedoch nicht den Wechsel zu diesem Modell.
* Die Behörde möchte Innovationen unterstützen und die gemeinsame Entwicklung von privatem Sektor, Bürgern und öffentlichen Einrichtungen fördern.
* Unterschiedliche Entwickler aus staatlichen und privaten Organisationen verfügen nicht über eine einheitliche Open-Source-Plattform, über die sie APIs und Daten ohne großen Aufwand gemeinsam nutzen können.
* Die Behördendaten befinden sich in lokalen Systemen, auf die öffentlicher Zugriff nur schwer möglich ist.

**Die Lösung**

Im Rahmen der Transformation zum Open Government-Modell müssen Leistung, Ausfallsicherheit, unterbrechungsfreie Geschäftsabläufe und Sicherheit gewährleistet sein. Da Innovation und gemeinsame Entwicklung voranschreiten, sind Behörden und Bürger von Software, Dienstleistungen und Infrastrukturunternehmen abhängig, die sich um Schutz und Bereitstellung der Daten kümmern.

Um Bürokratie abzubauen und die Beziehung zwischen Behörden und Bürgern auf eine neue Grundlage zu stellen, wurden offene Standards eingeführt, mit denen eine Plattform für gemeinsame Gestaltung eingerichtet wurde:

* Offene Daten - Datenspeicher, der freien Zugriff sowie freie gemeinsame Nutzung und Erweiterung der Daten durch Bürger, Regierungsbehörden und Unternehmen ermöglicht
* Offene APIs - Eine Entwicklungsplattform, auf der APIs von allen Partnern der Behörde bereitgestellt und von allen wiederverwendet werden
* Offene Innovation - Eine Reihe von Cloud-Services, die Entwicklern das schnelle Einbringen von Innovationen ermöglicht anstatt diese manuell codieren zu müssen

Am Anfang verwendet die Behörde {{site.data.keyword.cos_full_notm}} zum Speichern der öffentlichen Daten in der Cloud. Dieser Speicher kann kostenlos verwendet und wiederverwendet werden, kann von allen gemeinsam genutzt werden, nur Zuordnung und Freigabe werden vorausgesetzt. Sensible Daten können vor dem Speichern in der Cloud bereinigt werden. Außerdem werden die Zugriffskontrollen so eingerichtet, dass der neue Datenspeicher von der Cloud gedeckelt wird; von der Behörde können die Machbarkeitsnachweise der erweiterten vorhandenen freien Daten veranschaulicht werden.

Der nächste Schritt der Behörde zur öffentlich-privaten Partnerschaft (Public-Private-Partnership, PPP) war die Einrichtung einer API-Umgebung, die in {{site.data.keyword.apiconnect_long}} gehostet wurde. Auf diese Art machen die Behörden- und Unternehmensentwickler die Daten ohne großen Aufwand über APIs verfügbar. Ihre Ziele sind öffentlich verfügbare REST-APIs, Interoperabilität und die Beschleunigung der App-Integration. Sie verwenden IBM {{site.data.keyword.SecureGateway}}, um Verbindungen zu privaten lokalen Datenquellen herzustellen.

Da die Apps schließlich auf der Basis dieser gemeinsam genutzten APIs in {{site.data.keyword.containerlong_notm}} gehostet werden, kann von den Clustern einfach der Betrieb aufgenommen werden. Anschließend können Entwickler der Behörde, des privaten Sektors und der Regierung ohne großen Aufwand gemeinsam Apps erstellen. Somit müssen sich die Entwickler auf die Codierung und nicht auf die Verwaltung der Infrastruktur konzentrieren. {{site.data.keyword.containerlong_notm}} wurde ausgewählt, weil IBM die Verwaltung der Infrastruktur vereinfacht:
* Verwaltung des Kubernetes-Masters, der Infrastructure as a Service (IaaS) und der Betriebskomponenten, zum Beispiel Ingress und Speicher
* Überwachen von Status und Wiederherstellung für Workerknoten
* Bereitstellung globaler Rechenleistung, damit Entwickler weltweit keine Infrastruktur in den Regionen bereitstellen müssen, in denen Workloads und Daten bereitgestellt werden müssen

Das Verschieben der Rechenworkloads in {{site.data.keyword.Bluemix_notm}} reicht nicht aus. Die Behörden müssen auch eine Umwandlung für Prozesse und Methoden durchführen. Bei Übernahme der Verfahren von IBM Garage Method kann der Anbieter einen agilen und iterantiven Bereitstellungsprozess implementieren, von dem moderne DevOps-Verfahren wie Continuous Integration and Delivery (CI/CD) unterstützt werden.

Ein großer Teil des CI/CD-Prozesses wird in der Cloud mit {{site.data.keyword.contdelivery_full}} automatisiert. Der Anbieter kann die Workflow-Toolchains zum Vorbereiten der Container-Images definieren, eine Überprüfung auf Schwachstellen durchführen und eine Bereitstellung im Kubernetes-Cluster ausführen.

**Lösungsmodell**

Bedarfsgerechte Rechenkapazität, Speicherkapazität und API-Tools in der öffentlichen Cloud mit sicherem Zugriff auf die und von den lokalen Datenquellen.

Technische Lösung:
* {{site.data.keyword.containerlong_notm}}
* {{site.data.keyword.cos_full_notm}} und {{site.data.keyword.cloudant}}
* {{site.data.keyword.apiconnect_long}}
* IBM {{site.data.keyword.SecureGateway}}
* {{site.data.keyword.contdelivery_full}}

**Schritt 1: Daten in der Cloud speichern**
* Von {{site.data.keyword.cos_full_notm}} wird ein Protokolldatenspeicher bereitgestellt, auf den in der öffentlichen Cloud alle zugreifen können.
* Verwenden Sie {{site.data.keyword.cloudant}} mit von Entwicklern bereitgestellten Schlüsseln zum Zwischenspeichern von Daten in der Cloud.
* Verwenden Sie IBM {{site.data.keyword.SecureGateway}} zur Verwaltung der sicheren Verbindungen zu vorhandenen lokalen Datenbanken.

**Schritt 2: Zugriff auf Daten mit APIs bereitstellen**
* Verwenden Sie {{site.data.keyword.apiconnect_long}} für die Plattform mit der API-Umgebung. APIs ermöglichen es dem öffentlichen und privaten Sektor, Daten in ihren Apps zu kombinieren.
* Erstellen Sie Cluster für öffentlich-private Apps, die von APIs gesteuert werden.
* Strukturieren Sie die Apps zu einer Gruppe aus kooperativen Microservices, die in {{site.data.keyword.containerlong_notm}} ausgeführt werden; Basis hierfür sind die Funktionsbereiche der Apps und ihre Abhängigkeiten.
* Stellen Sie die Apps in den Containern bereit, die in {{site.data.keyword.containerlong_notm}} ausgeführt werden. Die in {{site.data.keyword.containerlong_notm}} integrierten Hochverfügbarkeitstools gleichen die Workload aus; hierbei sind auch automatische Fehlerbehebung und Lastverteilung eingeschlossen.
* Stellen Sie standardisierte DevOps-Dashboards über Kubernetes bereit, Open-Source-Tools, mit denen alle Entwickler vertraut sind.

**Schritt 3: Innovationen mit IBM Garage und Cloud-Services umsetzen**
* Übernehmen Sie die agilen und iterativen Entwicklungsverfahren von IBM Garage Method, um häufige Releases der Funktionen, Patches und Korrekturen ohne Ausfallzeit zu ermöglichen.
* Unabhängig davon, ob die Entwickler im öffentlichen oder privaten Sektor arbeiten, erleichtert {{site.data.keyword.contdelivery_full}} die schnelle Bereitstellung einer integrierten Toolchain mit anpassbaren gemeinsam nutzbaren Vorlagen.
* Nachdem die Entwickler die Apps in den Entwicklungs- und Testclustern erstellt und getestet haben, verwenden Sie die {{site.data.keyword.contdelivery_full}}-Toolchains zum Bereitstellen von Apps in Produktionsclustern.
* Mit Watson AI, maschinellem Lernen (ML) und Tools für tiefes Lernen, die über den {{site.data.keyword.Bluemix_notm}}-Katalog verfügbar sind, konzentrieren sich die Entwickler auf Domänenprobleme. Anstatt angepasstem eindeutigem ML-Code wird die ML-Logik in Apps mit Servicebindungen integriert.

**Ergebnisse**
* Von normalerweise langsamen öffentlich-privaten Partnerschaften wird der Betrieb der Apps jetzt schnell in Wochen anstatt Monaten aufgenommen. Von solchen Entwicklungspartnerschaften werden jetzt neue Funktionen und Fehlerkorrekturen bis zu zehn Mal pro Woche bereitgestellt.
* Die Entwicklung wird beschleunigt, wenn alle Teilnehmer bekannte Open-Source-Tools wie Kubernetes verwenden. Die Arbeit wird nicht durch langen Einarbeitungsaufwand  verzögert.
* Aktivitäten, Informationen und Pläne werden den Bürgerinnen und Bürgern sowie dem privaten Sektor transparent bereitgestellt. Und die Bürgerinnen und Bürger werden in behördliche Prozesse, Dienstleistungen und Unterstützung integriert.
* Öffentlich-private Partnerschaften sehen sich mit wahren Herkulesaufgaben konfrontiert, zum Beispiel die Überwachung des Zika-Virus, intelligente elektrische Verteilernetze, Analysen der Verbrechensstatistik und neue Wege in der Universitätsausbildung.

## Großer öffentlicher Hafen sichert Austausch von Hafendaten und Frachtlisten durch Verbindung zwischen öffentlicher und privater Organisation
{: #uc_port}

Eine IT-Führungskraft einer privaten Reederei und ein öffentlich geführter Hafen müssen Verbindungen aufbauen, Anzeigen bereitstellen und die Hafeninformationen sicher austauschen. Für einen Verbindungsaufbau zum Austauschen öffentlicher Hafeninformationen und privater Frachtlisten war jedoch kein einheitliches System vorhanden.
{: shortdesc}

Warum {{site.data.keyword.Bluemix_notm}}: {{site.data.keyword.containerlong_notm}} ermöglicht Partnerschaften aus Behörden und Öffentlichkeit die Nutzung externer Cloud-Services und Open-Source-Tools, die die Zusammenarbeit begünstigen. Von den Containern wurde eine gemeinsam genutzte Plattform bereitgestellt, auf der sowohl der Hafen als auch die Reederei sicher sind, dass die gemeinsamen genutzten Informationen auf einer sicheren Plattform gehostet wurden. Und diese Plattform lässt sich skalieren, vom kleinen System für Entwicklungstests bis zum großen Produktionssystem. Offene Toolchains beschleunigen die Entwicklung durch die Automatisierung der Erstellungen, Tests und Bereitstellungen.

Schlüsseltechnologien:    
* [Cluster, die den unterschiedlichen Anforderungen an CPU, RAM und Speicher entsprechen](/docs/containers?topic=containers-plan_clusters#shared_dedicated_node)
* [Containersicherheit und -isolierung](/docs/containers?topic=containers-security#security)
* [Native DevOps-Tools, einschließlich offener Toolchains in {{site.data.keyword.contdelivery_full}}](https://www.ibm.com/cloud/garage/toolchains/)
* [SDK für Node.js](/docs/runtimes/nodejs?topic=Nodejs-nodejs_runtime#nodejs_runtime)

**Kontext: Ein öffentlicher Hafen sichert den Austausch von Hafendaten und Frachtlisten durch eine Verbindung zwischen einer öffentlichen und einer privaten Organisation.**

* Die unterschiedlichen Entwickler der Behörden und der Reederei verfügen nicht über eine einheitliche Plattform, über die sie zusammenarbeiten können, was die Entwicklungen der Aktualisierungen und Funktionen verlangsamt.
* Da die Entwickler über den ganzen Globus und Organisationsgrenzen hinweg verstreut sind, sind Open-Source-Lösungen und Platform as a Service (PaaS) die beste Option.
* Die Sicherheit ist ein primäres Anliegen und stellt für die Zusammenarbeit eine zusätzliche Herausforderung dar, die Funktionen und Aktualisierungen für die Software erforderlich macht, insbesondere nachdem die Apps in der Produktion sind.
* Just-in-time-Daten bedeuteten, dass die weltweiten Systeme hoch verfügbar sein müssen, um Verzögerungen bei der Übertragung zu reduzieren. Die Zeitpläne für die Schiffsumschlaganlagen werden exakt gesteuert und sind in manchen Fallen inflexibel. Da die Nutzung des World Wide Web zunimmt, kann die Instabilität eine schlechte Benutzererfahrung zur Folge haben.

**Die Lösung**

Der Hafen und die Reederei entwickeln gemeinsam ein einheitliches Handelssystem, um elektronisch die für die Kompatibilität relevante Informationen für die Abfertigung der Waren und Schiffe einmalig und nicht über mehrere Stellen zu übermitteln. Der Inhalt einer bestimmten Sendung kann schnell mithilfe von Frachtlisten- und Zollanwendungen gemeinsam genutzt werden; so wird sichergestellt, dass die gesamten papierbasierte Prozesse von den Behörden für den Hafen elektronisch übertragen und verarbeitet werden.

Deswegen wird eine Partnerschaft gegründet, die Lösungen für das Handelssystem erarbeiten soll:
* Deklarationen - Eine App zum Erfassen der Frachtlisten, digitalen Verarbeitung der üblichen Zollformalitäten und Markierung von Unregelmäßigkeiten für Untersuchung und Vollzug
* Tarife - Eine App zum Berechnen der Tarife, elektronischen Übermitteln der Gebühren an den Spediteur und Empfangen digitaler Zahlungen
* Regulierungen - Flexible und konfigurierbare App, von der die beiden obigen Apps mit den sich ständig ändernden Richtlinien und Regelungen versorgt werden, die Importe, Exporte und Tarifverarbeitung betreffen

Die Entwickler begannen mit dem Bereitstellen der Apps in Containern mit {{site.data.keyword.containerlong_notm}}. Sie erstellten Cluster für eine gemeinsame Entwicklungsumgebung, die es den weltweiten Entwicklern ermöglicht, die App-Verbesserungen schnell gemeinsam zu bereitzustellen. Die Container ermöglichen es jedem Entwicklerteam, die Sprache seiner Wahl zu verwenden.

Sicherheit zuerst: Die IT-Führungskräfte wählten Trusted Compute für Bare-Metal zum Hosten der Cluster. Bei Verwendung von Bare-Metal für {{site.data.keyword.containerlong_notm}} verfügen die sensiblen Zoll-Workloads jetzt über die bekannte Isolation, gleichzeitig aber auch über die Flexibilität der öffentlichen Cloud. Von Bare-Metal wird Trusted Compute zur Überprüfung der zugrunde liegenden Hardware auf Manipulationen bereitgestellt.

Da die Reederei auch mit anderen Häfen arbeiten möchte, ist die App-Sicherheit von entscheidender Bedeutung. Frachtlisten und Zollinformationen sind streng vertraulich. Zur Gewährleistung der Sicherheit werden von Vulnerability Advisor folgende Scans bereitgestellt:
* Scans auf Sicherheitslücken für Images
* Scans für Richtlinien, die auf ISO 27k basieren
* Live-Scans für Container
* Paket-Scans für bekannte Malware

Gleichzeitig kann mit {{site.data.keyword.iamlong}} gesteuert werden, wer über welche Zugriffsberechtigungen auf die Ressourcen verfügt.

Die Entwickler konzentrieren sich mithilfe der vorhandenen Tools auf Domänenprobleme: Anstatt eindeutigen Protokollierungs- und Überwachungscode zu schreiben, integrieren Sie ihn in Apps, indem Sie {{site.data.keyword.Bluemix_notm}}-Services an Cluster binden. Die Entwickler brauchen sich auch nicht mehr um Infrastrukturmanagementaufgaben kümmern, da Kubernetes- und Infrastruktur-Upgrades, die Sicherheit und vieles mehr von IBM verwaltet werden.

**Lösungsmodell**

Bedarfsgerechte Rechenkapazität, Speicherkapazität und Node-Starter-Kits, die nach Bedarf in einer öffentlichen Cloud mit sicherem Zugriff auf die Frachtdaten auf der ganzen Welt ausgeführt werden. Die Berechnung in Clustern ist manipulationssicher und durch Bare-Metal isoliert.  

Technische Lösung:
* {{site.data.keyword.containerlong_notm}} mit Trusted Compute
* {{site.data.keyword.openwhisk}}
* {{site.data.keyword.cloudant}}
* IBM {{site.data.keyword.SecureGateway}}

**Schritt 1: Containerisierte Apps mithilfe von Microservices**
* Verwenden Sie das Node.js-Starter-Kit von IBM für einen Schnelleinstieg in die Entwicklung.
* Strukturieren Sie Apps zu einer Gruppe aus kooperativen Microservices, die in {{site.data.keyword.containerlong_notm}} ausgeführt werden; Basis hierfür sind die Funktionsbereiche der App und ihre Abhängigkeiten.
* Stellen Sie die Apps für Frachtlisten und Zollformalitäten in Containern bereit, die in {{site.data.keyword.containerlong_notm}} ausgeführt werden.
* Stellen Sie standardisierte DevOps-Dashboards über Kubernetes bereit.
* Verwenden Sie IBM {{site.data.keyword.SecureGateway}} zur Verwaltung der sicheren Verbindungen zu vorhandenen lokalen Datenbanken.

**Schritt 2: Globale Verfügbarkeit sicherstellen**
* Nachdem die Entwickler die Apps in ihren Entwicklungs- und Testclustern bereitgestellt haben, verwenden Sie die {{site.data.keyword.contdelivery_full}}-Toolchains und Helm zum Bereitstellen länderspezifischer Apps in Clustern auf der ganzen Welt.
* Workloads und Daten können dann die regionalen Vorschriften erfüllen.
* Die in {{site.data.keyword.containerlong_notm}} integrierten Hochverfügbarkeitstools gleichen die Workload in jeder geografischen Region aus; hierbei sind auch automatische Fehlerbehebung und Lastverteilung eingeschlossen.

**Schritt 3: Gemeinsame Nutzung der Daten**
* {{site.data.keyword.cloudant}} ist eine moderne NoSQL-Datenbank, die für eine Reihe datengesteuerter Anwendungsfälle genutzt werden kann: von Schlüsselwerten bis zu komplexen dokumentorientierten Datenspeichern und Abfragen.
* Zum Minimieren der Abfragen der regionalen Datenbanken wird {{site.data.keyword.cloudant}} verwendet, um die Sitzungsdaten des Benutzers in den Apps zwischenzuspeichern.
* Diese Konfiguration verbessert die durchgehende Benutzerfreundlichkeit und Leistung in allen Apps für {{site.data.keyword.containershort}}.
* Während die Worker-Apps in {{site.data.keyword.containerlong_notm}} lokale Daten analysieren und die Ergebnisse in {{site.data.keyword.cloudant}} speichern, reagiert {{site.data.keyword.openwhisk}} auf Änderungen und bereinigt automatisch die Daten in den eingehenden Datenfeeds.
* Analog können Benachrichtigungen über Lieferungen in eine Region über das Hochladen von Daten ausgelöst werden, sodass alle nachgelagerten Nutzer auf die neuen Daten zugreifen können.

**Ergebnisse**
* Mit IBM Starter-Kits, {{site.data.keyword.containerlong_notm}} und {{site.data.keyword.contdelivery_full}}-Tools kooperieren die weltweiten Entwickler in Organisationen und Behörden. Sie entwickeln mit gängigen und funktionell aufeinander abgestimmten Tools gemeinsam Apps für die Zollformalitäten.
* Mit Microservices wird die Bereitstellungszeit für Patches, Fehlerkorrekturen und neue Funktionen erheblich reduziert. Erstmalige Entwicklungen verlaufen schnell und Aktualisierungen werden häufig 10 Mal pro Woche bereitgestellt.
* Die Reedereikunden und die Verwaltungsbeamten verfügen über Zugriff auf die Daten der Frachtlisten und können die Zolldaten gemeinsam nutzen, während diese den lokalen Regulierungen entsprechen.
* Die Reederei profitiert von der verbesserten logistischen Verwaltung in der Lieferkette: die Kosten sind niedriger und die Umschlagzeiten kürzer.
* 99 % der Zollanmeldungen sind digital und 90 % der Importe werden ohne Bedienereingriff verarbeitet.
