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



# Anwendungsfälle aus der Finanzdienstleistungsbranche für {{site.data.keyword.cloud_notm}}
{: #cs_uc_finance}

Anhand der folgenden Anwendungsfälle wird veranschaulicht, wie Workloads in {{site.data.keyword.containerlong_notm}} von den Vorteilen der hohen Verfügbarkeit, leistungsfähiger Rechenkapazität, einfacher Einrichtung der Cluster zur schnelleren Bereitstellung und der künstlichen Intelligenz von {{site.data.keyword.ibmwatson}} profitieren können.
{: shortdesc}

## Hypothekenbank friert Kosten ein und beschleunigt Einhaltung gesetzlicher Vorschriften
{: #uc_mortgage}

Der Risk Management VP eines Unternehmens für Wohnbauhypotheken ist für die Verarbeitung von 70 Millionen Datensätzen pro Tage verantwortlich, aber das lokale System ist langsam und auch ungenau. Die IT-Ausgaben sind deutlich gestiegen, weil die Hardware schnell veraltet war und nicht vollständig verwendet wurde. Während das Unternehmen auf die Bereitstellung der Hardware wartete, verlangsamte sich die Einhaltung der gesetzlicher Bestimmungen.
{: shortdesc}

Warum {{site.data.keyword.Bluemix_notm}}: Zum Verbessern der Risikoanalyse setzte das Unternehmen auf {{site.data.keyword.containerlong_notm}} und IBM Cloud Analytics-Services, um die Kosten zu reduzieren, die weltweite Verfügbarkeit zu erhöhen und schließlich die Einhaltung gesetzlicher Bestimmungen zu beschleunigen. Bei Verwendung von {{site.data.keyword.containerlong_notm}} in mehreren Regionen können die Analyse-Apps containerisiert und auf dem gesamten Globus bereitgestellt werden, was die Verfügbarkeit verbessert und die Umsetzung lokaler Regelungen beschleunigt. Die Bereitstellungen werden mit gängigen Open-Source-Tools beschleunigt, die Bestandteil von {{site.data.keyword.containerlong_notm}} sind.

{{site.data.keyword.containerlong_notm}} und Schlüsseltechnologien:
* [Horizontale Skalierung](/docs/containers?topic=containers-app#highly_available_apps)
* [Mehrere Regionen für Hochverfügbarkeit](/docs/containers?topic=containers-regions-and-zones#regions-and-zones)
* [Cluster, die den unterschiedlichen Anforderungen an CPU, RAM und Speicher entsprechen](/docs/containers?topic=containers-plan_clusters#shared_dedicated_node)
* [Containersicherheit und -isolierung](/docs/containers?topic=containers-security#security)
* [{{site.data.keyword.cloudant}} zum Beibehalten und Synchronisieren von Daten für Apps](/docs/services/Cloudant?topic=cloudant-getting-started#getting-started)
* [SDK für Node.js](/docs/runtimes/nodejs?topic=Nodejs-nodejs_runtime#nodejs_runtime)

**Die Lösung**

Das Unternehmen begann damit, die Analyse-Apps zu containerisieren und in die Cloud zu stellen. Damit waren die Hardwareprobleme sehr schnell behoben. Ohne großen Aufwand konnten Kubernetes-Cluster entworfen werden, die den hohen Anforderungen an CPU, RAM, Speicher und Sicherheit entsprachen. Und wenn sich die Ergebnisse der Analyse-Apps ändern, kann ohne große Investitionen in die Hardware Rechenleistung hinzugefügt oder reduziert werden. Aufgrund der horizontalen Skalierung von {{site.data.keyword.containerlong_notm}} werden Apps mit der zunehmenden Anzahl der Datensätze skaliert, was zu schnelleren Berichten zur Einhaltung gesetzlicher Bestimmungen führt. Von {{site.data.keyword.containerlong_notm}} werden auf der ganzen Welt flexible Rechenressourcen bereitgestellt, die sicher sind und eine hohe Leistung zur gesamten Nutzung moderner Rechenressourcen bieten.

Jetzt empfangen diese Apps Daten in einem hohen Volumen aus einem {{site.data.keyword.cloudant}}-Data-Warehouse. Durch den cloudbasierten Speicher in {{site.data.keyword.cloudant}} wird eine höhere Verfügbarkeit als auf einem lokalen System sichergestellt. Da die Verfügbarkeit von großer Bedeutung ist, werden die Apps in Rechenzentren auf der ganzen Welt bereitgestellt: zur Gewährleistung der Disaster-Recovery und Optimierung der Latenzzeit.

Außerdem wurden die Risikoanalyse und Einhaltung von Vorschriften beschleunigt. Die Funktionen für Vorhersagen und Risikoanalysen, wie zum Beispiel Monte Carlo-Berechnungen, werden jetzt über iterative agile Bereitstellungen konstant aktualisiert. Die Container-Orchestrierung wird von einer verwalteten Kubernetes-Instanz verwaltet, sodass auch die Betriebskosten sinken. Somit ist auch die Reaktionsfähigkeit der Risikoanalyse für Hypotheken auf die rasanten Veränderungen auf dem Markt gestiegen.

**Kontext: Konformität und Finanzmodellierung für Wohnbauhypotheken**

* Der gestiegene Bedarf an besserem finanziellem Risikomanagement führt zu mehr Beaufsichtigung der Einhaltung gesetzlicher Bestimmungen. Dieselben Anforderungen sind der Grund für die zugehörige Überprüfung der Risikobewertungsprozesse und die Offenlegung einer detaillierteren, integrierten und ergiebigen Berichterstellung zur Einhaltung gesetzlicher Bestimmungen.
* Die wichtigsten Infrastrukturkomponenten für die Finanzmodellierung sind äußerst leistungsfähige Computerverbünde (Computing-Grids)

Die Probleme des Unternehmens sind jetzt die Skalierung und die Bereitstellungszeit.

Die aktuelle Umgebung ist über sieben Jahre alt, befindet sich vor Ort und wird durch begrenzte Rechenleistung, Speicherkapazität und E/A-Kapazität beeinträchtigt.
Serveraktualisierungen sind kostenintensiv und ihre Durchführung ist langwierig.
Die Aktualisierung der Software und der Apps verläuft nach einem inoffiziellen Prozess und ist nicht reproduzierbar.
Die Programmierung des Grids für die Datenverarbeitung mit hoher Leistung ist schwierig. Die API ist für neue Entwickler, die ihre Arbeit gerade aufgenommen haben, zu komplex, und für die Arbeit ist nicht dokumentiertes Wissen erforderlich.
Die Durchführung größerer App-Upgrades dauert zwischen sechs und neuen Monate.

**Lösungsmodell: Bedarfsgerechte Rechenkapazität, Speicherkapazität und E/A-Services, die nach Bedarf in einer öffentlichen Cloud mit sicherem Zugriff auf die Unternehmensassets vor Ort ausgeführt werden**

* Sicherer und skalierbarer Dokumentenspeicher, der strukturierte und unstrukturierte Dokumentabfragen unterstützt
* 'Lift-and-shift' für vorhandene Unternehmensressourcen und Anwendungen, während die Integration für einige lokale Systeme aktiviert ist, die nicht migriert werden
* Lösungen zur Verkürzung der Bereitstellungszeit und Bereitstellung von Standard-DevOps und Überwachungsprozessen zum Beheben von Fehlern, die die Genauigkeit der Berichterstellung beeinträchtigten

**Detaillierte Lösung**

* {{site.data.keyword.containerlong_notm}}
* {{site.data.keyword.cos_full_notm}}
* {{site.data.keyword.sqlquery_notm}} (Spark)
* {{site.data.keyword.cloudant}}
* {{site.data.keyword.SecureGateway}}

{{site.data.keyword.containerlong_notm}} stellt skalierbare Rechenressourcen und die zugehörigen DevOps-Dashboards zum bedarfsgerechten Erstellen, Skalieren und Umrüsten von Apps und Services bereit. Mithilfe standardisierter Container können Apps anfangs von {{site.data.keyword.containerlong_notm}} ohne größere architektonische Änderungen erneut gehostet werden.

Diese Lösung bietet den unmittelbaren Vorteil der Skalierbarkeit. Mithilfe der vielfältigen Bereitstellungs- und Laufzeitobjekte von Kubernetes überwacht das Hypothekenunternehmen zuverlässig die Upgrades der Apps. Auch das Replizieren und Skalieren der Apps ist möglich, von denen definierte Regeln und ein automatisierter Kubernetes-Orchestrator verwendet wird.

{{site.data.keyword.SecureGateway}} wird zum Erstellen einer sicheren Pipeline zu lokalen Datenbanken und Dokumenten für Apps verwendet, die zur Ausführung in {{site.data.keyword.containerlong_notm}} erneut gehostet werden.

{{site.data.keyword.cos_full_notm}} ist für die Speicherung aller unformatiertes Dokumente und Daten vorgesehen, die weitergeleitet werden. Bei Monte-Carlo-Simulationen wird eine Workflow-Pipeline an die Position versetzt, an der Simulationsdaten in Strukturdateien enthalten sind, die in {{site.data.keyword.cos_full_notm}} gespeichert sind. Ein Auslöser zum Starten der Simulation skaliert die Rechenservices in {{site.data.keyword.containerlong_notm}}, um die Daten der Daten für die Simulationsverarbeitung in N Ereignisbuckets aufzuteilen. Von {{site.data.keyword.containerlong_notm}} werden automatisch die N zugehörigen Serviceausführungen skaliert, die Zwischenergebnisse werden in {{site.data.keyword.cos_full_notm}} geschrieben. Diese Ergebnisse werden von weiteren {{site.data.keyword.containerlong_notm}}-Rechenservices zur Erstellung der endgültigen Ergebnisse verarbeitet.

{{site.data.keyword.cloudant}} ist eine moderne NoSQL-Datenbank, die für viele datengesteuerte Anwendungsfälle genutzt werden kann: von Schlüsselwerten bis zu komplexen dokumentorientierten Datenspeichern und Abfragen. Um die zunehmenden Regeln für Berichte zur Einhaltung gesetzlicher Bestimmungen und die Regeln für Berichte zur Unternehmensführung zu verwalten, verwendet das Hypothekenunternehmen {{site.data.keyword.cloudant}} zum Speichern von Dokumenten, die unbearbeiteten Daten mit gesetzlichen Bestimmungen zugeordnet sind, die vom Unternehmen empfangen werden. In {{site.data.keyword.containerlong_notm}} werden Rechenprozesse zum Kompilieren, Verarbeiten und Veröffentlichen der Daten in unterschiedlichen Berichtsformaten ausgelöst. Zwischenergebnisse für die Berichte werden als {{site.data.keyword.cloudant}}-Dokumente gespeichert, sodass vorlagengesteuerte Prozesse zum Erstellen der erforderlichen Berichte verwendet werden können.

**Ergebnisse**

* Komplexe Finanzsimulationen werden jetzt 25 % schneller durchgeführt, als dies vorher mit den vorhandenen lokalen Systemen möglich war.
* Die Zeit für die Bereitstellung verbesserte sich von vorher sechs bis neun Monate auf durchschnittlich ein bis drei Wochen. Ausschlaggebend für diese Verbesserung ist der durch {{site.data.keyword.containerlong_notm}} mögliche strukturiert gesteuerte Prozess für die Lastverlaufssteuerung der App-Container sowie ihre Ersetzung durch neuere Versionen. Wenn Fehler berichtet werden, können die Probleme - wie zum Beispiel die Genauigkeit - schnell behoben werden.
* Die Kosten für die Berichterstellung zur Einhaltung gesetzlicher Vorschriften werden durch eine konsistente, skalierbare Menge an Speicher- und Rechenservices reduziert werden, die mit {{site.data.keyword.containerlong_notm}} und {{site.data.keyword.cloudant}} bereitgestellt werden.
* Nach und nach wurde die Architektur der ursprünglichen Apps, die anfangs mit Lift-and-shift in der Cloud bereitgestellt wurden, in kooperative Microservices geändert, die mithilfe von {{site.data.keyword.containerlong_notm}} ausgeführt werden. Hierdurch wurden die Entwicklung beschleunigt, die Bereitstellungszeit verkürzt und mehr Innovation ermöglicht, weil das Experimentieren einfach geworden war. Außerdem wurden innovative Apps mit neueren Versionen der Microservices freigegeben, um die Vorteile der Markt- und Geschäftsbedingungen (sogenannte situationsbezogene Apps und Microservices) zu nutzen.

## Payment-Technology-Unternehmen vervierfacht Entwicklerproduktivität durch Bereitstellung KI-fähiger Tools für Partner
{: #uc_payment_tech}

In der Entwicklungsabteilung arbeiten Entwickler mit lokalen konventionellen Tools, was die Prototyperstellung verlangsamt, da sie auf die Bereitstellung der Hardware warten müssen.
{: shortdesc}

Warum {{site.data.keyword.Bluemix_notm}}: Bei Verwendung von {{site.data.keyword.containerlong_notm}} werden Berechnungen mit Open-Source-Standardtechnologien durchgeführt. Nach dem Wechsel des Unternehmens zu {{site.data.keyword.containerlong_notm}} können die Entwickler auf DevOps-kompatible Tools zugreifen, zum Beispiel portierbare und einfach gemeinsam nutzbare Container.

Somit können die Entwickler ohne großen Aufwand experimentieren und mit offenen Toolchains schneller Änderungen zu Bereitstellungs- und Testsystemen hinzufügen. Die herkömmlichen Softwareentwicklungstools erhalten ein neues Aussehen, wenn sie Cloud-Services mit künstlicher Intelligenz mit einem Klick zu den Apps hinzufügen.

Schlüsseltechnologien:
* [Cluster, die den unterschiedlichen Anforderungen an CPU, RAM und Speicher entsprechen](/docs/containers?topic=containers-plan_clusters#shared_dedicated_node)
* [Betrugsprävention mit {{site.data.keyword.watson}} AI](https://www.ibm.com/cloud/watson-studio)
* [Native DevOps-Tools, einschließlich offener Toolchains in {{site.data.keyword.contdelivery_full}}](https://www.ibm.com/cloud/garage/toolchains/)
* [SDK für Node.js](/docs/runtimes/nodejs?topic=Nodejs-nodejs_runtime#nodejs_runtime)
* [Anmeldefunktionalität ohne App-Code-Änderungen durch {{site.data.keyword.appid_short_notm}}](/docs/services/appid?topic=appid-getting-started)

**Kontext: Optimierung der Entwicklerproduktivität und viermal schnellere Bereitstellung von KI-Tools für Partner**

* Störungen sind in der Zahlungsmittelindustrie weit verbreitet, gleichzeitig verzeichnet sie im Verbraucher- und B2B-Sektor enorme Zuwachsraten. Die Aktualisierungen der Tools für den Zahlungsverkehr waren jedoch langsam.
* Um betrügerische Transaktionen auf neue und schnellere Arten bekämpfen zu können, sind kognitive Funktionen erforderlich.
* Da die Anzahl der Partner und die zugehörigen Transaktionen zunehmen, steigt auch der Datenverkehr zwischen den Tools an, das Budget für die Infrastruktur muss jedoch zur Maximierung der Ressourceneffizienz reduziert werden.
* Die Gefahren aufgrund unzulänglicher Technik nehmen zu, was sogar dazu führt, dass keine qualitativ hochwertige Software beschafft wird, um mit den Anforderungen des Markts Schritt halten zu können.
* Die Budgets für Investitionen unterliegen einer strikten Kontrolle und bei den IT-Mitarbeitern entsteht der Eindruck, dass weder ausreichend Geld noch Mitarbeiter vorhanden sind, um mit den unternehmensinternen Systemen Test- und Stagingumgebungen zu erstellen.
* Die Bedeutung der Sicherheit nimmt deutlich zu und stellt für die Bereitstellungen eine weitere Belastung dar, die zu noch mehr Verzögerungen führt.

**Die Lösung**

Die Entwicklungsabteilung sieht sich in der dynamischen Zahlungsverkehrsbranche mit vielen Herausforderungen konfrontiert. Regelungen, Verbraucherverhalten, Betrug, Wettbewerber und Marktinfrastrukturen entwickeln sich rasant weiter. Eine schnelle Entwicklung ist somit von großer Bedeutung, um in der Branche weiterhin bestehen zu können.

Das Geschäftsmodell besteht darin, Geschäftspartnern Tools für den Zahlungsverkehr zur Verfügung zu stellen, um diesen Finanzinstituten und anderen Organisationen bei der Bereitstellung von sicheren digitalen Zahlungsverfahren helfen zu können.

Sie benötigen eine Lösung, die den Entwicklern und ihren Geschäftspartnern hilft:
* Front-End für Zahlungsverkehrstools: Gebührensysteme, Zahlungsverfolgung (auch grenzüberschreitend), Einhaltung gesetzlicher Bestimmungen, biometrische Angaben, Überweisung, usw.
* Regulierungsspezifische Funktionen: Jedes Land weist eigene Regulierungen auf, das allgemeine Toolset kann somit ähnlich sein, es gibt jedoch länderspezifische Sonderregelungen
* Entwicklerfreundliche Tools: Sie beschleunigen das Implementieren von Funktionen und Beheben von Fehlern
* Betrugserkennung als Service (Fraud Detection as a Service, FDaaS): Verwendet {{site.data.keyword.watson}} AI, um den häufigen und zunehmenden Betrugsaktionen einen Schritt voraus zu sein

**Lösungsmodell**

Bedarfsgerechte Rechenleistung, DevOps-Tools und künstliche Intelligenz, ausgeführt in einer öffentlichen Cloud mit Zugriff auf Back-End-Zahlungssysteme. Implementieren Sie einen CI/CD-Prozess, um die Bereitstellungszyklen deutlich zu reduzieren.

Technische Lösung:
* {{site.data.keyword.containerlong_notm}}
* {{site.data.keyword.contdelivery_full}}
* IBM Cloud Logging and Monitoring
* {{site.data.keyword.ibmwatson_notm}} for Financial Services
* {{site.data.keyword.appid_full_notm}}

Zunächst wurden die virtuellen Maschinen der Zahlungstools containerisiert und in die Cloud gestellt. Damit waren die Hardwareprobleme sehr schnell behoben. Ohne großen Aufwand konnten Kubernetes-Cluster entworfen werden, die den Anforderungen an CPU, RAM, Speicher und Sicherheit entsprachen. Und wenn an den Zahlungstools Änderungen vorgenommen werden müssen, kann ohne kostenintensive und langsame Hardwarekäufe Rechenleistung hinzugefügt oder reduziert werden.

Aufgrund der horizontalen Skalierung von {{site.data.keyword.containerlong_notm}} werden die Apps mit der zunehmenden Anzahl der Partner skaliert, was zu einem schnelleren Zuwachs führt. Von {{site.data.keyword.containerlong_notm}} werden auf der ganzen Welt flexible Rechenressourcen bereitgestellt, die für die gesamte Nutzung moderner Rechenressourcen hohe Sicherheit bieten.

Für die Unternehmensleitung ist die beschleunigte Entwicklung eine bedeutende Verbesserung. Durch die Verwendung moderner Container können die Entwickler ohne großen Aufwand in den Sprachen ihrer Wahl experimentieren und Änderungen mit Push-Operationen zu Bereitstellungs- und Testsystemen hinzufügen, die in separaten Clustern horizontal skaliert sind. Diese Push-Operationen wurden mit offenen Toolchains und {{site.data.keyword.contdelivery_full}} automatisiert. Die Aktualisierung der Tools zieht sich nicht mehr in langsamen, fehlerträchtigen Erstellungsprozessen hin. Schrittweise Aktualisierungen können für die Tools täglich oder sogar noch häufiger bereitgestellt werden.

Außerdem wurde schnell eine Protokollierung und Überwachung für die Tools in das System integriert, besonders bei Verwendung von {{site.data.keyword.watson}} AI. Die Entwickler verschwenden ihre Zeit nicht mit dem Erstellen komplexer Protokollierungssysteme, nur um in der Lage zu sein, Fehler in den aktiven Systemen zu beheben. Ein wichtiger Faktor zur Reduzierung des Personalaufwands ist, dass Kubernetes von IBM verwaltet wird und sich die Entwickler deswegen auf die Arbeit an besseren Zahlungstools konzentrieren können.

Sicherheit zuerst: Bei Verwendung von Bare-Metal für {{site.data.keyword.containerlong_notm}} verfügen die sensiblen Zahlungstools jetzt über die bekannte Isolation, gleichzeitig aber auch über die Flexibilität der öffentlichen Cloud. Von Bare-Metal wird Trusted Compute zur Überprüfung der zugrunde liegenden Hardware auf Manipulationen bereitgestellt. Suchvorgänge zur Ermittlung von Sicherheitslücken und Malware werden kontinuierlich durchgeführt.

**Schritt 1: Lift-and-shift zum Sichern der Rechenleistung**
* Apps, die hochsensible Daten verwalten, können in {{site.data.keyword.containerlong_notm}} mithilfe von Bare Metal für Trusted Compute ausgeführt werden. Mit Trusted Compute kann die zugrunde liegende Hardware auf Manipulationen überprüft werden.
* Migrieren Sie die Images der virtuellen Maschine auf Container-Images, die in {{site.data.keyword.containerlong_notm}} in der öffentlichen {{site.data.keyword.Bluemix_notm}}-Instanz ausgeführt werden.
* Auf dieser Grundlage werden von Vulnerability Advisor Schwachstellensuchen für Images, Richtlinien, Container und Paketierung auf bekannte Malware durchgeführt.
* Die Kosten für private Rechenzentren bzw. Investitionen vor Ort werden erheblich reduziert und durch ein Modell für Utility-Computing ersetzt, das abhängig vom Workloadbedarf skaliert wird.
* Sie können eine richtlinienbasierte Authentifizierung für Ihre Services und APIs durch eine einfache Ingress-Annotation konsistent durchsetzen. Durch deklarative Sicherheit können Sie die Benutzerauthentifizierung und die Tokenvalidierung mithilfe von {{site.data.keyword.appid_short_notm}} sicherstellen.

**Schritt 2: Operationen und Verbindungen zum Back-End vorhandener Zahlungssysteme**
* Verwenden Sie IBM {{site.data.keyword.SecureGateway}} zur Verwaltung der sicheren Verbindungen zu lokalen Toolsystemen
* Stellen Sie standardisierte DevOps-Dashboards und -Verfahren über Kubernetes bereit
* Nachdem die Entwickler die Apps in den Entwicklungs- und Testclustern erstellt und getestet haben, verwenden Sie die {{site.data.keyword.contdelivery_full}}-Toolchains zum Bereitstellen von Apps in {{site.data.keyword.containerlong_notm}}-Clustern auf der ganzen Welt.
* Die in {{site.data.keyword.containerlong_notm}} integrierten Hochverfügbarkeitstools gleichen die Workload in jeder geografischen Region aus; hierbei sind auch automatische Fehlerbehebung und Lastverteilung eingeschlossen.

**Schritt 3: Betrug analysieren und vermeiden**
* Stellen Sie IBM {{site.data.keyword.watson}} for Financial Services bereit, um Betrug zu ermitteln und zu verhindern.
* Mithilfe der Toolchains und Helm-Bereitstellungstools werden die Apps auch in {{site.data.keyword.containerlong_notm}}-Clustern auf der ganzen Welt bereitgestellt. Somit erfüllen Workloads und Daten die Anforderungen der regionalen Regulierungen.

**Ergebnisse**
* Das Verschieben der vorhandenen monolithischen virtuellen Maschinen in Container, die in der Cloud gehostet wurden, war ein erster Schritt, der es der Entwicklungsabteilung ermöglichte, Kapital und Betriebskosten einzusparen.
* Da die Verwaltung der Infrastruktur von IBM übernommen wurde, hatte das Entwicklerteam mehr Zeit und konnte zehn Mal pro Tag Aktualisierungen bereitstellen.
* Parallel dazu implementierte der Anbieter in bestimmten Zeitfenstern einfache Iterationen, um Beeinträchtigungen durch unzulängliche technische Umstände zu minimieren.
* Abhängig von der jeweiligen Anzahl an verarbeiteten Transaktionen können die zugehörigen Operationen exponentiell skaliert werden.
* Gleichzeitig wurde durch den Einsatz neuer Betrugsanalysen mit {{site.data.keyword.watson}} die Geschwindigkeit bei der Erkennung und Vermeidung erhöht; dies hatte zur Folge, dass Betrugsfälle vier Mal häufiger als im Durchschnitt in der Region reduziert wurden.
