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



# Anwendungsfälle aus dem Transportwesen für {{site.data.keyword.cloud_notm}}
{: #cs_uc_transport}

Anhand der folgenden Anwendungsfälle wird hervorgehoben, wie Workloads in {{site.data.keyword.containerlong_notm}} von Toolchains für schnelle App-Aktualisierungen und Bereitstellungen in mehreren Regionen auf der Welt profitieren können. Gleichzeitig können diese Workloads eine Verbindung zu vorhandenen Back-End-Systemen herstellen, Watson AI für die Personalisierung verwenden und mit {{site.data.keyword.messagehub_full}} auf IoT-Daten zugreifen.

{: shortdesc}

## Reederei erhöht die Verfügbarkeit weltweiter Systeme für Geschäftspartnernetzwerk
{: #uc_shipping}

Eine IT-Führungskraft ist für die weltweiten Streckenführungs- und Planungssysteme verantwortlich, mit denen die Partner interagieren. Die Partner benötigen von diesen Systemen, die auf IoT-Einheitendaten zugreifen, zeitnahe Informationen. Die Hochverfügbarkeit für diese Systeme konnte jedoch nicht weltweit skaliert werden.
{: shortdesc}

Warum {{site.data.keyword.cloud_notm}}: {{site.data.keyword.containerlong_notm}} skaliert containerisierte Apps mit extrem hoher Verfügbarkeit, um dem wachsenden Bedarf gerecht zu werden. Die App-Bereitstellungen werden 40 Mal pro Tag durchgeführt zu, wenn die Entwickler ohne großen Aufwand experimentieren; auf diese Art werden Änderungen schnell zu Bereitstellungs- und Testsystemen hinzugefügt. Die IoT-Plattform erleichtert den Zugriff auf IoT-Daten.

Schlüsseltechnologien:    
* [Mehrere Regionen für Geschäftspartnernetzwerk ](/docs/containers?topic=containers-regions-and-zones#regions-and-zones)
* [Horizontale Skalierung](/docs/containers?topic=containers-app#highly_available_apps)
* [Offene Toolchains in {{site.data.keyword.contdelivery_full}}](https://www.ibm.com/cloud/garage/toolchains/)
* [Cloud-Services für Innovation](https://www.ibm.com/cloud/products/#analytics)
* [{{site.data.keyword.messagehub_full}} für die Zuführung von Ereignisdaten zu Apps](/docs/services/EventStreams?topic=eventstreams-about#about)

**Kontext: Reederei erhöht Verfügbarkeit weltweiter Systeme für Geschäftspartnernetzwerk**

* Regionale Unterschiede in der Logistik machten es schwierig, der wachsenden Anzahl an Partnern in mehreren Ländern Rechnung zu tragen. So müssen zum Beispiel eindeutige Regelungen und die Transitlogistik beachtet werden, für die vom Unternehmen konsistente Datensätze über Grenzen hinweg verwaltet werden müssen.
* Just-in-time-Daten bedeuteten, dass die weltweiten Systeme hoch verfügbar sein müssen, um Verzögerungen bei der Übertragung zu reduzieren. Die Zeitpläne für die Schiffsumschlaganlagen werden exakt gesteuert und sind in manchen Fallen inflexibel. Da die Nutzung des World Wide Web zunimmt, kann die Instabilität eine schlechte Benutzererfahrung zur Folge haben.
* Entwickler müssen die Apps ständig weiterentwickeln, aufgrund der traditionellen Tools dauerte es jedoch lange, Aktualisierungen und Funktionen häufig bereitzustellen.  

**Die Lösung**

Die Reederei muss die Zeitpläne für die Verschiffung, die Lagerbestände und die Zollpapiere kohäsiv verwalten. Danach können die Position der Sendungen, der Lieferumfang und die Lieferpläne mit den Kunden gemeinsam genutzt werden. Somit ist es nicht mehr nötig, zu schätzen, wann eine Ware (zum Beispiel ein Gerät, Bekleidung oder ein Produkt) eintrifft, sodass die Kunden der Reederei diese Informationen an ihre Kunden übermitteln können.

Die Lösung besteht aus den folgenden wichtigen Bestandteilen:
1. Das Streaming der Daten von IoT-Geräten für jeden Versandcontainer: Frachtlisten und Standort
2. Zollpapiere, die digital mit den jeweiligen Häfen und Transitpartnern gemeinsam genutzt werden (einschließlich Zugriffskontrolle)
3. Eine App für Reedereikunden, die Ankunftsdaten für Transportgut zusammenfasst und überträgt, einschließlich APIs für Reedereikunden zum erneuten Verwenden der Sendungsdaten in ihren eigenen Einzelhandels- und Business-to-Business-Apps

Damit die Reederei mit globalen Partnern zusammenarbeiten konnte, mussten am System für Routenwahl und Terminplanung lokale Änderungen vorgenommen werden, um Sprache, Regulierungen und eindeutige Hafenlogistikdaten jeder einzelnen Region einzufügen. {{site.data.keyword.containerlong_notm}} bietet eine globale Abdeckung in mehreren Regionen, darunter Nordamerika, Europa, Asien und Australien, sodass die Apps den Bedarf seiner Partner im jeweiligen Land widerspiegeln.

Von den IoT-Geräten werden Daten gestreamt, die von {{site.data.keyword.messagehub_full}} an die Apps für die regionalen Häfen und jeweiligen Datenspeicher für Zollabfertigung und Containerfrachtlisten verteilt werden. {{site.data.keyword.messagehub_full}} ist der Eintrittspunkt für IoT-Ereignisse. Von ihm werden die Ereignisse abhängig von der verwalteten Konnektivität, die von der Watson IoT-Plattform angeboten wird, an {{site.data.keyword.messagehub_full}} weitergegeben.

Sobald sich die Ereignisse in {{site.data.keyword.messagehub_full}} befinden, werden sie dauerhaft für die sofortige Verwendung in den Apps für den Hafentransit und für weitere Verwendungen in der Zukunft gespeichert. Von den Apps mit der niedrigsten Latenzzeit werden die Daten aus dem Ereignisstrom direkt in Echtzeit verarbeitet ({{site.data.keyword.messagehub_full}}). Von anderen zukünftigen Apps, wie zum Beispiel den Analysetools, können die Daten aus dem Ereignisspeicher mit {{site.data.keyword.cos_full}} im Stapelbetrieb verarbeitet werden.

Da die Sendungsdaten mit den Kunden des Unternehmens gemeinsam genutzt werden, stellen die Entwickler sicher, dass die Kunden APIs zum Darstellen der Sendungsdaten in ihren eigenen Apps verwenden können. Beispiele für solche Apps sind Apps für mobile Ortung oder E-Commerce-Lösungen. Die Entwickler arbeiten auch daran, Apps für die regionalen Häfen zu erstellen und zu pflegen, von denen Zolldatensätze und Frachtlisten der Sendungen erfasst und verbreitet werden. Somit müssen sie sich auf die Codierung und nicht auf die Verwaltung der Infrastruktur konzentrieren. Und schließlich wurde {{site.data.keyword.containerlong_notm}} ausgewählt, weil IBM die Verwaltung der Infrastruktur vereinfacht:
* Verwaltung des Kubernetes-Masters, von Infrastructure as a Service (IaaS) und der Betriebskomponenten, wie zum Beispiel Ingress und Speicher
* Überwachen von Status und Wiederherstellung für Workerknoten
* Bereitstellung globaler Rechenleistung, damit Entwickler nicht für die Infrastruktur in den mehreren Regionen verantwortlich sind, in denen sich Workloads und Daten befinden müssen

Um eine globale Verfügbarkeit zu erreichen, wurden die Entwicklungs-, Test- und Produktionssysteme auf der ganzen Welt in mehreren Rechenzentren bereitgestellt. Zur Gewährleistung der hohen Verfügbarkeit wird eine Kombination aus mehreren Clustern in unterschiedlichen geografischen Regionen sowie Mehrzonenclustern verwendet. Somit können die Hafen-Apps ohne großen Aufwand so bereitgestellt werden, dass die Geschäftsanforderungen erfüllt werden:
* In Clustern in Frankfurt zur Einhaltung der lokalen EU-Regelungen
* In Clustern in den USA, um die lokale Verfügbarkeit und die Wiederherstellung bei Fehlern sicherzustellen

Auch die Workloads wurden auf Mehrzonencluster in Frankfurt verteilt, um die Verfügbarkeit der Europäischen Version der App und einen effizienten Lastausgleich für die Workload sicherzustellen. Da von jeder Region mit der Hafen-App eindeutige Daten hochgeladen werden, werden die Cluster der App in Regionen gehostet, in denen die Latenzzeit niedrig ist.

Für Entwickler kann ein großer Teil des fortlaufenden Integrations- und Bereitstellungsprozesses (CI/CD-Prozess) mit {{site.data.keyword.contdelivery_full}} automatisiert werden. Das Unternehmen kann die Workflow-Toolchains zum Vorbereiten der Container-Images definieren, eine Überprüfung auf Schwachstellen durchführen und eine Bereitstellung im Kubernetes-Cluster ausführen.

**Lösungsmodell**

Bedarfsgerechte Rechenkapazität, Speicherkapazität und ein Ereignismanagement, das nach Bedarf in einer öffentlichen Cloud mit Zugriff auf die Sendungsdaten auf der ganzen Welt ausgeführt wird.

Technische Lösung:
* {{site.data.keyword.containerlong_notm}}
* {{site.data.keyword.messagehub_full}}
* {{site.data.keyword.cos_full}}
* {{site.data.keyword.contdelivery_full}}

**Schritt 1: Containerisierte Apps mithilfe von Microservices**

* Integrieren Sie Apps in einer Gruppe aus kooperativen Microservices in {{site.data.keyword.containerlong_notm}} auf Grundlage von Funktionsbereichen der App und deren Abhängigkeiten.
* Stellen Sie die Apps in den Containern in {{site.data.keyword.containerlong_notm}} bereit.
* Stellen Sie standardisierte DevOps-Dashboards über Kubernetes bereit.
* Aktivieren Sie die bedarfsgerechte Skalierung der Rechenleistung für Batch-Workloads und Lagerbestands-Workloads, die nicht häufig ausgeführt werden.
* Verwenden Sie {{site.data.keyword.messagehub_full}} zum Verwalten der Streaming-Daten von den IoT-Geräten.

**Schritt 2: Globale Verfügbarkeit sicherstellen**
* Die in {{site.data.keyword.containerlong_notm}} integrierten Hochverfügbarkeitstools gleichen die Workload in jeder geografischen Region aus; hierbei sind auch automatische Fehlerbehebung und Lastverteilung eingeschlossen.
* Für Lastausgleich, Firewalls und DNS wird IBM Cloud Internet Services verwendet.
* Mithilfe der Toolchains und Helm-Bereitstellungstools werden die Apps auch in Clustern auf der ganzen Welt bereitgestellt; somit erfüllen die Workloads und Daten die regionalen Vorschriften.

**Schritt 3: Daten gemeinsam nutzen**
* Von {{site.data.keyword.cos_full}} und {{site.data.keyword.messagehub_full}} wird ein echtzeitorientierter Protokolldatenspeicher bereitgestellt.
* APIs ermöglichen den Kunden des versendenden Unternehmens die gemeinsame Nutzung von Daten in ihren Apps.

**Schritt 4: Kontinuierlich bereitstellen**
* {{site.data.keyword.contdelivery_full}} dient Entwicklern als Unterstützung bei der schnellen Bereitstellung einer integrierten Toolchain mithilfe anpassbarer und gemeinsam nutzbarer Vorlagen unter Verwendung von IBM Tools, Tools von Drittanbietern und Open-Source-Tools. Automatisieren Sie Erstellungen und Tests und kontrollieren Sie die Qualität durch Analysen.
* Nachdem die Entwickler die Apps in den Entwicklungs- und Testclustern erstellt und getestet haben, verwenden Sie die IBM CI/CD-Toolchains zum Bereitstellen von Apps in Clustern auf der ganzen Welt.
* {{site.data.keyword.containerlong_notm}} bietet eine einfache Durchführung von Rollouts und Rollbacks für die Apps; angepasste Apps werden unter Verwendung des intelligenten Routings und Lastausgleichs von Istio zwecks Einhaltung regionaler Anforderungen bereitgestellt.

**Ergebnisse**

* Mit {{site.data.keyword.containerlong_notm}} und CI/CD-Tools von IBM werden regionale Versionen der Apps in der Nähe der physischen Einheiten gehostet, von denen sie Daten erfassen.
* Mit Microservices wird die Bereitstellungszeit für Patches, Fehlerkorrekturen und neue Funktionen erheblich reduziert. Erstmalige Entwicklungen verlaufen schnell und Aktualisierungen sind häufig.
* Die Kunden der Reederei verfügen über Echtzeitzugriff auf die Positionen der Sendungen, Lieferpläne und sogar die genehmigten Hafendaten.
* Da die Transitpartner an den verschiedenen Versandterminals über die Frachtlisten und und Sendungsdetails verfügen, verläuft die Logistik vor Ort schneller und es kommt zu weniger Verzögerungen.
* Abgesehen von diesem Szenario [sind Maersk und IBM ein Joint Venture eingegangen](https://www.ibm.com/press/us/en/pressrelease/53602.wss), um internationale Lieferketten mit Blockchain zu verbessern.

## Fluglinie stellt innovative Human Resources-Site für Mitarbeiterleistungen in weniger als drei Wochen bereit
{: #uc_airline}

Ein Chief Human Resources Officer (CHRO) benötigt eine neue Human Resources-Site für Mitarbeiterleistungen mit einem innovativen Chatbot, aufgrund der derzeitigen Entwicklungstools und der momentanen Plattform dauert es jedoch lange, bis die Apps bereitgestellt werden können. Hinzu kommen lange Wartezeiten für die Beschaffung der Hardware.
{: shortdesc}

Warum {{site.data.keyword.cloud_notm}}: Mit {{site.data.keyword.containerlong_notm}} kann die Rechenleistung in kurzer Zeit bereitgestellt werden. Somit können die Entwickler ohne großen Aufwand experimentieren und mit offenen Toolchains schneller Änderungen zu Bereitstellungs- und Testsystemen hinzufügen. Die Arbeit mit ihren herkömmlichen Softwareentwicklungstools verläuft deutlich schneller, wenn sie IBM Watson Assistant hinzufügen. Die neue Leistungsseite wurde in weniger als 3 Wochen erstellt.

Schlüsseltechnologien:    
* [Cluster, die den unterschiedlichen Anforderungen an CPU, RAM und Speicher entsprechen](/docs/containers?topic=containers-plan_clusters#shared_dedicated_node)
* [Chatbot-Service, der auf Watson basiert](https://developer.ibm.com/code/patterns/create-cognitive-banking-chatbot/)
* [Native DevOps-Tools, einschließlich offener Toolchains in {{site.data.keyword.contdelivery_full}}](https://www.ibm.com/cloud/garage/toolchains/)
* [SDK für Node.js](/docs/runtimes/nodejs?topic=Nodejs-nodejs_runtime#nodejs_runtime)

**Kontext: Schnelle Erstellung und Bereitstellung einer innovativen Human Resources-Site für Mitarbeiterleistungen in weniger als drei Wochen**
* Die Zunahme der Mitarbeiter und sich ändernde Personalpolitik haben zur Folge, dass eine ganz neue Site für die jährlichen Bewerbungen erforderlich wird.
* Die neue Personalpolitik soll den vorhandenen Mitarbeitern mithilfe interaktiver Funktionen wie einem Chatbot mitgeteilt werden.
* Aufgrund der Zunahme an Mitarbeitern stieg der Datenverkehr auf der Site, das Budget für ihre Infrastruktur bleibt jedoch niedrig.
* Das Human Resources-Team stand unter Druck, die neuen Funktionen schnell bereitzustellen und die Änderungen an den Leistungen in letzter Minute häufig zu veröffentlichen.
* Da die Bewerbungsfrist zwei Wochen dauert, darf es nicht zu einer Ausfallzeit für die App kommen.

**Die Lösung**

Mit einer offenen Kultur möchte die Fluglinie den Menschen in den Mittelpunkt stellen. Der Personalvorstand ist sich bewusst, dass es für die Rentabilität der Fluglinie von Vorteil ist, wenn es ihr gelingt Talente zu belohnen und zu halten. Aus diesem Grund ist die jährliche Anwerbung neuer Mitarbeiter ein wichtiger Aspekt bei der Förderung einer auf den Mitarbeiter ausgerichteten Unternehmenskultur.

Es ist eine Lösung erforderlich, die Vorteile für Entwicklern und Benutzer bietet:
* Front-End für vorhandene Leistungen: Versicherung, Bildungsangebote, Wellness und vieles mehr
* Regional spezifische Features: Da für jedes Land eine eigene Personalpolitik gilt, können sich die Seiten ähnlich sehen, für jede Region werden jedoch spezifische Leistungen angeboten
* Entwicklerfreundliche Tools: Das Implementieren von Funktionen und Beheben von Fehlern wird beschleunigt
* Chatbot: Bereitstellung authentischer Gespräche über Leistungen und einer effizienten Beantwortung von Benutzeranforderungen und -fragen

Technische Lösung:
* {{site.data.keyword.containerlong_notm}}
* IBM Watson Assistant und Watson Tone Analyzer
* {{site.data.keyword.contdelivery_full}}
* IBM Cloud Logging and Monitoring
* {{site.data.keyword.SecureGatewayfull}}
* {{site.data.keyword.appid_full_notm}}

Für CHRO ist die beschleunigte Entwicklung eine bedeutende Verbesserung. Das Team beginnt zunächst damit, die Apps zu containerisieren und in die Cloud zu stellen. Durch die Verwendung moderner Container können die Entwickler ohne großen Aufwand mit dem Node.js-SDK experimentieren und Änderungen zu Bereitstellungs- und Testsystemen mit Push-Operationen hinzufügen, die mit separaten Clustern horizontal skaliert werden. Diese Push-Operationen wurden mit offenen Toolchains und {{site.data.keyword.contdelivery_full}} automatisiert. Die Aktualisierung der Human Resources-Site zieht sich nicht mehr in langsamen, fehlerträchtigen Erstellungsprozessen hin. Schrittweise Aktualisierungen können für die Site täglich oder sogar noch häufiger bereitgestellt werden.  Darüber hinaus ist die Protokollierung und Überwachung der Human Resources-Site schnell integriert, insbesondere in Bezug auf die Art und Weise, wie von der Site personalisierte Daten aus Back-End-Leistungssystemen extrahiert werden. Die Entwickler verschwenden ihre Zeit nicht mit dem Erstellen komplexer Protokollierungssysteme, nur um in der Lage zu sein, Fehler in den aktiven Systemen zu beheben. Entwickler müssen nicht zu Experten in der Cloudsicherheit werden; mithilfe von {{site.data.keyword.appid_full_notm}} können Sie ohne großen Aufwand eine richtliniengesteuerte Authentifizierung erzwingen.

Mit {{site.data.keyword.containerlong_notm}} wurde die überdimensionierte Hardware in einem privaten Rechenzentrum durch anpassbare Rechenleistung ersetzt, wodurch der IT-Betrieb, die Wartung und die Energiekosten reduziert wurden. Zum Hosten der Human Resources-Site konnten ohne großen Aufwand Kubernetes-Cluster entworfen werden, die an die Anforderungen an CPU, RAM und Speicher angepasst wurden. Ein weiterer Faktor für niedrigere Personalkosten ist die Tatsache, dass Kubernetes von IBM verwaltet wird, sodass sich die Entwickler auf die Bereitstellung einer besseren Mitarbeitererfahrung für die Antragsverarbeitung konzentrieren kann.

{{site.data.keyword.containerlong_notm}} stellt skalierbare Rechenressourcen und die zugehörigen DevOps-Dashboards zum bedarfsgerechten Erstellen, Skalieren und Umrüsten von Apps und Services bereit. Mit standardisierter Containertechnologie können Apps schnell entwickelt und in mehreren Entwicklungs-, Test- und Produktionsumgebungen gemeinsam genutzt werden. Diese Konfiguration bietet den unmittelbaren Vorteil der Skalierbarkeit. Mithilfe der vielfältigen Bereitstellungs- und Laufzeitobjekte von Kubernetes kann das Human Resources-Team die Upgrades der Apps zuverlässig überwachen und verwalten. Die Apps können mithilfe von definierten Regeln und dem automatisierten Kubernetes Orchestrator auch repliziert und skaliert werden.

**Schritt 1: Container, Microservices und Garage Methode**
* Die Apps werden in einer Gruppe aus kooperativen Microservices erstellt, die in {{site.data.keyword.containerlong_notm}} ausgeführt werden. Die Architektur stellt die Funktionsbereiche der App mit den meisten Qualitätsproblemen dar.
* Stellen Sie die Apps in einem Container in {{site.data.keyword.containerlong_notm}} bereit, der kontinuierlich mit IBM Vulnerability Advisor gescannt wird.
* Stellen Sie standardisierte DevOps-Dashboards über Kubernetes bereit.
* Übernehmen Sie die wichtigen agilen und iterativen Entwicklungsverfahren in IBM Garage Method, um häufige Releases neuer Funktionen, Patches und Korrekturen ohne Ausfallzeit zu ermöglichen.

**Schritt 2: Verbindungen zum vorhandenen Leistungs-Back-End**
* {{site.data.keyword.SecureGatewayfull}} wird verwendet, um einen sicheren Tunnel zu lokalen Systemen aufzubauen, von denen die Leistungssysteme gehostet werden.  
* Aufgrund der Kombination aus den lokalen Daten und {{site.data.keyword.containerlong_notm}} kann auf sensible Daten zugegriffen werden, während gleichzeitig die Vorschriften eingehalten werden.
* Da die Chatbot-Gespräche erfasst werden und in die Personalpolitik eingehen, kann auf der Leistungsseite festgestellt werden, welche Leistungen beliebter und weniger beliebt sind; so können auch gezielte Verbesserungen für schlecht verlaufende Initiativen vorgenommen werden.

**Schritt 3: Chatbot und Personalisierung**
* IBM Watson Assistant bietet Tools, mit denen schnell das Gerüst für einen Chatbot erstellt werden kann, von dem den Benutzern die richtigen Leistungen bereitgestellt werden können.
* Watson Tone Analyzer sorgt dafür, dass die Kunden mit den Chatbot-Gesprächen zufrieden sind und bei Bedarf auf eine persönliche Interaktion mit einem Berater zurückgreifen können.

**Schritt 4: Kontinuierlich weltweit bereitstellen**
* {{site.data.keyword.contdelivery_full}} dient Entwicklern als Unterstützung bei der schnellen Bereitstellung einer integrierten Toolchain mithilfe anpassbarer und gemeinsam nutzbarer Vorlagen unter Verwendung von IBM Tools, Tools von Drittanbietern und Open-Source-Tools. Automatisieren Sie Erstellungen und Tests und kontrollieren Sie die Qualität durch Analysen.
* Nachdem die Entwickler die Apps in den Entwicklungs- und Testclustern erstellt und getestet haben, verwenden Sie die IBM CI/CD-Toolchains zum Bereitstellen von Apps in Produktionsclustern auf der ganzen Welt.
* {{site.data.keyword.containerlong_notm}} bietet eine einfache Durchführung von Rollouts und Rollbacks für die Apps. Angepasste Apps werden unter Verwendung des intelligenten Routings und Lastausgleichs von Istio bereitgestellt, um regionale Anforderungen zu erfüllen.
* Die in {{site.data.keyword.containerlong_notm}} integrierten Hochverfügbarkeitstools gleichen die Workload in jeder geografischen Region aus; hierbei sind auch automatische Fehlerbehebung und Lastverteilung eingeschlossen.

**Ergebnisse**
* Mit Tools wie dem Chatbot hat das Human Resources-Team seinen Mitarbeitern bewiesen, dass Innovation für das Unternehmen nicht nur ein Modeschlagwort, sondern tatsächlich Bestandteil der Unternehmenskultur ist.
* Die Authentizität und die benutzerspezifischen Anpassungen auf der Site haben den geänderten Erwartungen heutigen Fluglinienmitarbeiter Rechnung getragen.
* Die in der letzten Minute noch vorgenommenen Aktualisierungen an der Human Resources-Site, einschließlich der Aktualisierungen durch die Chatbot-Gespräche der Mitarbeiter, verliefen schnell, weil die Entwickler mindestens 10 Mal täglich Änderungen vorgenommen hatten.
* Da die Verwaltung der Infrastruktur von IBM übernommen wurde, hatte das Entwicklerteam mehr Zeit und konnte die Site in nur drei Wochen bereitstellen.
