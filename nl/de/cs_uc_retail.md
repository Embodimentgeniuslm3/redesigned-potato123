---

copyright:
  years: 2014, 2019
lastupdated: "2019-04-01"

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



# Anwendungsfälle aus dem Einzelhandel für {{site.data.keyword.cloud_notm}}
{: #cs_uc_retail}

Anhand der folgenden Anwendungsfälle wird hervorgehoben, wie Workloads in {{site.data.keyword.containerlong_notm}} von einer Analyse für Markterkenntnisse, Bereitstellungen in mehreren Regionen auf der Welt und einem Bestandsmanagement mit {{site.data.keyword.messagehub_full}} und Objektspeicher profitieren.
{: shortdesc}

## Traditionelles Einzelhandelsunternehmen nutzt Daten mithilfe von APIs gemeinsam mit globalen Geschäftspartnern zur Stärkung des Omni-Channel-Vertriebs
{: #uc_data-share}

Der Leiter eines Geschäftsbereichs muss die Anzahl der Vertriebskanäle erhöhen, das Einzelhandelssystem ist jedoch in einem lokalen Rechenzentrum isoliert. Die Konkurrenz verfügt über globale Geschäftspartner für das Cross-Selling und Up-Selling ihrer Produkte: traditionell und online.
{: shortdesc}

Warum {{site.data.keyword.cloud_notm}}: Von {{site.data.keyword.containerlong_notm}} wird ein Ökosystem in der öffentlichen Cloud bereitgestellt, in dem neue Geschäftspartner und weitere externe Player über Container mithilfe von APIs gemeinsam Apps und Daten entwickeln können. Sobald sich das Einzelhandelssystem in der öffentlichen Cloud befindet, wird die gemeinsame Datennutzung optimiert und Entwicklung neuer Apps beschleunigt. Die Bereitstellungen der Apps nehmen zu, wenn die Entwickler ohne großen Aufwand experimentieren und mit Toolchains schneller Änderungen zu Bereitstellungs- und Testsystemen hinzufügen.

{{site.data.keyword.containerlong_notm}} und Schlüsseltechnologien:
* [Cluster, die den unterschiedlichen Anforderungen an CPU, RAM und Speicher entsprechen](/docs/containers?topic=containers-plan_clusters#shared_dedicated_node)
* [{{site.data.keyword.cos_full}} zum Beibehalten und Synchronisieren von Daten für Apps](/docs/tutorials?topic=solution-tutorials-pub-sub-object-storage#pub-sub-object-storage)
* [Native DevOps-Tools, einschließlich offener Toolchains in {{site.data.keyword.contdelivery_full}}](https://www.ibm.com/cloud/garage/toolchains/)

**Kontext: Ein Einzelhandelsunternehmen nutzt Daten mithilfe von APIs gemeinsam mit globalen Geschäftspartnern zur Stärkung des Omni-Channel-Vertriebs**

* Der Einzelhändler unterliegt einem sehr starken Wettbewerbsdruck. Zum einen muss der Einzelhändler dafür sorgen, dass die Übergänge zu neuen Produkten und Vertriebswegen für den Kunden nicht allzu komplex erscheinen. So muss zum Beispiel die Differenzierung der Produkte verbessert werden. Zum anderen muss es für die Kunden einfacher sein, zwischen den Marken hin- und herzuwechseln.
* Diese Möglichkeit zum Wechseln zwischen den Marken bedeutet, dass für das gesamte Einzelhandelssystem Verbindungen zu den jeweiligen Geschäftspartnern erforderlich sind. Außerdem kann die Cloud einen zusätzlichen Wert durch den Kontakt zu Geschäftspartnern, Kunden und anderen externen Playern mit sich bringen.
* Extrem umsatzstarke Benutzertermine wie der schwarze Freitag (Black Friday) sind eine enorme Belastung für Onlinesysteme und zwingen das Einzelhandelsunternehmen zur Bereitstellung zusätzlicher Recheninfrastruktur.
* Die Entwickler des Einzelhandelsunternehmens mussten ständig Apps weiterentwickeln, aber die traditionellen Tools verlangsamten ihre Fähigkeit, Aktualisierungen und Funktionen häufig bereitzustellen, besonders dann, wenn sie mit Geschäftspartnerteams zusammenarbeiten.  

**Die Lösung**

Zur Steigerung der Kundenbindung und der Bruttogewinnspanne muss das Einkaufserlebnis der Kunden verbessert werden. Das traditionelle Verkaufsmodell des Einzelhandelsunternehmens litt unter dem mangelnden Geschäftspartnerwarenbestand für Cross-Selling und Up-Selling. Die Kunden wünschen mehr Komfort, damit sie zusammengehörige Artikel schnell finden können, zum Beispiel Yoga-Hosen und Yoga-Matten.

Das Einzelhandelsunternehmen muss den Kunden auch nützliche Inhalte wie Produktinformationen, alternative Produktinformationen, Rezensionen und eine Anzeige des Echtzeitbestands zur Verfügung stellen. Und die Kunden möchten all diese Informationen sowohl online als auch im Laden vor Ort über eigene persönliche Mobilgeräte sowie vom Verkaufspersonal erhalten, das mit Mobilgeräten ausgestattet ist.

Die Lösung besteht aus den folgenden wichtigen Bestandteilen:
* Lagerbestand: Eine App für das direkte Geschäftsumfeld der Partner, von der der Lagerbestand zusammengefasst und übertragen wird, besonders die Einführungen neuer Produkte, einschließlich APIs für Geschäftspartner zur erneuten Verwendung in ihren eigenen Einzelhandels- und B2B-Apps
* Cross-Selling und Up-Selling: Eine App, von der Gelegenheiten für Cross-Selling und Up-Selling mit APIs bereitgestellt werden, die in unterschiedlichen E-Commerce-Apps und mobilen Apps verwendet werden kann
* Entwicklungsumgebung: Kubernetes-Cluster für Entwicklungs-, Test- und Produktionssysteme verbessern die Zusammenarbeit und gemeinsame Datennutzung zwischen dem Einzelhandelsunternehmen und seinen Geschäftspartnern

Damit das Einzelhandelsunternehmen mit globalen Geschäftspartnern zusammenarbeiten kann, müssen an den APIs für den Lagerbestand Änderungen vorgenommen werden, damit die Sprache und die Marktvorgaben für jede einzelne Region angepasst werden können. {{site.data.keyword.containerlong_notm}} bietet eine Abdeckung in mehreren Regionen, darunter Nordamerika, Europa, Asien und Australien; hierbei spiegeln die APIs die Bedürfnisse der einzelnen Länder wieder und stellen eine kurze Latenzzeit für API-Aufrufe sicher.

Eine weitere Voraussetzung ist, dass die gemeinsame Nutzung der Bestandsdaten mit den Kunden der Geschäftspartner und den Unternehmen möglich ist. Mit den APIs für die Bestandsdaten können die Entwickler Informationen in Apps bereitstellen, zum Beispiel als mobile Bestandsdaten-Apps oder E-Commerce-Lösungen für das World Wide Web. Die Entwickler sind auch mit dem Aufbau und der Pflege der primären E-Commerce-Site beschäftigt. Somit müssen sie sich auf die Codierung und nicht auf die Verwaltung der Infrastruktur konzentrieren.

Und schließlich wurde {{site.data.keyword.containerlong_notm}} ausgewählt, weil IBM die Verwaltung der Infrastruktur vereinfacht:
* Verwaltung des Kubernetes-Masters, von Infrastructure as a Service (IaaS) und der Betriebskomponenten, wie zum Beispiel Ingress und Speicher
* Überwachen von Status und Wiederherstellung für Workerknoten
* Bereitstellung globaler Rechenleistung, damit Entwickler über eine Hardwareinfrastruktur in den Regionen verfügen, in denen sich Workloads und Daten befinden müssen

Darüber hinaus wird die Protokollierung und Überwachung der API-Microservices, besonders das Extrahieren personalisierter Daten aus Back-End-Systemen, ohne großen Aufwand in {{site.data.keyword.containerlong_notm}} integriert. Die Entwickler verschwenden ihre Zeit nicht mit dem Erstellen komplexer Protokollierungssysteme, nur um in der Lage zu sein, Fehler in den aktiven Systemen zu beheben.

{{site.data.keyword.messagehub_full}} dient als Plattform für Just-in-time-Ereignisse zum Integrieren der sich schnell ändernden Informationen von den Bestandssystemen der Geschäftspartner in {{site.data.keyword.cos_full}}.

**Lösungsmodell**

Bedarfsgerechte Rechenkapazität, Speicherkapazität und ein Ereignismanagement, das nach Bedarf in einer öffentlichen Cloud mit Zugriff auf die Bestandsdaten auf der ganzen Welt ausgeführt wird.

Technische Lösung:
* {{site.data.keyword.containerlong_notm}}
* {{site.data.keyword.messagehub_full}}
* {{site.data.keyword.cos_full}}
* {{site.data.keyword.contdelivery_full}}
* IBM Cloud Logging and Monitoring
* {{site.data.keyword.appid_short_notm}}

**Schritt 1: Containerisierte Apps mithilfe von Microservices**
* Strukturieren Sie Apps zu einer Gruppe aus kooperativen Microservices, die in {{site.data.keyword.containerlong_notm}} ausgeführt werden; Basis hierfür sind die Funktionsbereiche der App und ihre Abhängigkeiten.
* Stellen Sie die Apps in Container-Images bereit, die in {{site.data.keyword.containerlong_notm}} ausgeführt werden.
* Stellen Sie standardisierte DevOps-Dashboards über Kubernetes bereit.
* Aktivieren Sie die bedarfsgerechte Skalierung der Rechenleistung für Batch-Workloads und weitere Lagerbestands-Workloads, die nicht häufig ausgeführt werden.

**Schritt 2: Globale Verfügbarkeit sicherstellen**
* Die in {{site.data.keyword.containerlong_notm}} integrierten Hochverfügbarkeitstools gleichen die Workload in jeder geografischen Region aus; hierbei sind auch automatische Fehlerbehebung und Lastverteilung eingeschlossen.
* Für Lastausgleich, Firewalls und DNS wird IBM Cloud Internet Services verwendet.
* Mithilfe der Toolchains und Helm-Bereitstellungstools werden die Apps auch in Clustern auf der ganzen Welt bereitgestellt; somit entsprechen die Workloads und Daten den regionalen Anforderungen, besonders bei der Personalisierung.

**Schritt 3: Benutzer verstehen**
* {{site.data.keyword.appid_short_notm}} stellt Anmeldefunktionalität bereit, ohne dass App-Code geändert werden muss.
* Nachdem sich Benutzer angemeldet haben, können Sie mit {{site.data.keyword.appid_short_notm}} Profile erstellen und das Benutzererlebnis Ihrer Anwendung personalisieren.

**Schritt 4: Daten gemeinsam nutzen**
* Von {{site.data.keyword.cos_full}} und {{site.data.keyword.messagehub_full}} wird ein echtzeitorientierter Protokolldatenspeicher bereitgestellt, sodass für Cross-Selling-Angebote der verfügbare Lagerbestand der Geschäftspartner dargestellt wird.
* APIs ermöglichen es den Geschäftspartnern des Einzelhandelsunternehmens, Daten in ihren E-Commerce-Apps und B2B-Apps gemeinsam zu nutzen.

**Schritt 5: Kontinuierlich bereitstellen**
* Das Debugging der gemeinsam entwickelten APIs wird einfacher, wenn sie zu den IBM Cloud Logging and Monitoring-Tools hinzugefügt werden, auf die verschiedene Entwickler zugreifen können, weil sie cloudbasiert sind.
* {{site.data.keyword.contdelivery_full}} dient Entwicklern als Unterstützung bei der schnellen Bereitstellung einer integrierten Toolchain mithilfe anpassbarer und gemeinsam nutzbarer Vorlagen unter Verwendung von IBM Tools, Tools von Drittanbietern und Open-Source-Tools. Automatisieren Sie Erstellungen und Tests und kontrollieren Sie die Qualität durch Analysen.
* Nachdem die Entwickler die Apps in den Entwicklungs- und Testclustern erstellt und getestet haben, verwenden Sie die IBM CI/CD-Toolchains (CI/CD - Continuous Integration and Delivery) zum Bereitstellen von Apps in Clustern auf der ganzen Welt.
* {{site.data.keyword.containerlong_notm}} bietet eine einfache Durchführung von Rollouts und Rollbacks für die Apps; angepasste Apps werden unter Verwendung des intelligenten Routings und Lastausgleichs von Istio zum Testen von Kampagnen bereitgestellt.

**Ergebnisse**
* Mit Microservices wird die Bereitstellungszeit für Patches, Fehlerkorrekturen und neue Funktionen erheblich reduziert. Die erstmalige weltweite Entwicklung verläuft schnell und Aktualisierungen werden bis zu 40 Mal pro Woche bereitgestellt.
* Der Einzelhändler und seine Geschäftspartner verfügen mithilfe von APIs über direkten Zugriff auf die Bestandsverfügbarkeit und Bereitstellungszeitpläne.
* Mit {{site.data.keyword.containerlong_notm}} und den IBM CI/CD-Tools können Kampagnen mit A-B-Versionen der Apps getestet werden.
* {{site.data.keyword.containerlong_notm}} bietet eine skalierbare Rechenleistung, sodass die Workloads für Lagerbestand und Cross-Selling in Zeiträumen mit hohem Umsatz zunehmen können, zum Beispiel in den Herbstferien.

## Konventioneller Lebensmittelhändler steigert Kundenaufkommen und Vertrieb mit digitalen Erkenntnissen
{: #uc_grocer}

Ein Chief Marketing Officer (CMO) muss das Kundenaufkommen in den Geschäften durch Betonung des Einkaufserlebnisses in den Geschäften um 20 % steigern. Der Umsatz verlagert sich zunehmend zu großen Mitbewerbern aus dem Einzelhandel und dem Online-Einzelhandel. Gleichzeitig muss der CMO den Lagerbestand ohne Preisabschläge reduzieren, da eine zu lange Lagerhaltung Millionen an Kapital bindet.
{: shortdesc}

Warum {{site.data.keyword.cloud_notm}}: {{site.data.keyword.containerlong_notm}} bietet leicht bereitzustellende Rechenkapazität, mit deren Hilfe Entwickler schnell Cloud Analytics-Services für Erkenntnisse zum Einkaufsverhalten und der Anpassungsfähigkeit des digitalen Markts hinzufügen können.

Schlüsseltechnologien:    
* [Horizontale Skalierung zur Beschleunigung der Entwicklung](/docs/containers?topic=containers-app#highly_available_apps)
* [Cluster, die den unterschiedlichen Anforderungen an CPU, RAM und Speicher entsprechen](/docs/containers?topic=containers-plan_clusters#shared_dedicated_node)
* [Erkenntnisse über Markttrends mit Watson Discovery](https://www.ibm.com/watson/services/discovery/)
* [Native DevOps-Tools, einschließlich offener Toolchains in {{site.data.keyword.contdelivery_full}}](https://www.ibm.com/cloud/garage/toolchains/)
* [Bestandsmanagement mit {{site.data.keyword.messagehub_full}}](/docs/services/EventStreams?topic=eventstreams-about#about)

**Kontext: Konventioneller Lebensmittelhändler steigert Kundenaufkommen und Vertrieb mit digitalen Erkenntnissen**

* Der Wettbewerbsdruck von Online-Einzelhändlern und großen Einzelhandelsunternehmen stellt für die Einzelhandelsmodelle herkömmlicher Lebensmittelgeschäfte eine große Herausforderung dar. Die Umsätze sinken, Nachweis hierfür ist die abnehmende Besucherfrequenz in den Geschäften vor Ort.
* Das Kundentreueprogramm braucht einen Schub und eine innovative Idee für die gedruckten Coupons an der Kasse. Daher müssen Entwickler die zugehörigen Apps ständig weiterentwickeln, aufgrund der traditionellen Tools dauert es jedoch lange, Aktualisierungen und Funktionen häufig bereitzustellen.  
* Bestimmte hochwertige Lagerbestände verändern sich nicht wie erwartet, obwohl die Liebhaber guten Essens in den Märkten der Großstädte immer mehr zuzunehmen scheinen.

**Die Lösung**

Der Lebensmittelhändler benötigt eine App zum Steigern der Konversion und des Filialverkehrs, um neue Umsätze zu generieren und in einer Plattform für wiederverwendbare Cloud-Analysen für den Aufbau der Kundenloyalität zu sorgen. Das im Geschäft geplante Einkaufserlebnis kann ein Ereignis mit einem Dienstleistungs- oder Produktanbieter sein, das basierend auf der Affinität für das spezielle Ereignis sowohl für Stammkunden als auch neue Kunden attraktiv ist. Das Geschäft und der Geschäftspartner bieten dann Anreize für die Teilnahme an dem Ereignis und für den Kauf der Produkte vom Geschäft oder vom Geschäftspartner.  

Nach der Veranstaltung wird bei den Kunden um den Kauf der notwendigen Produkte geworben, sodass sie die vorgeführte Aktivität in Zukunft selbst wiederholen können. Die anvisierte Kundenzufriedenheit wird anhand der Einlösung der Anreize und der Neuanmeldung der Stammkunden gemessen. Mit einer Kombination aus einem individuell abgestimmten Marketingereignis und einem Tool zum Verfolgen der Einkäufe im Geschäft kann festgestellt werden, ob die beabsichtigte Kundenerfahrung zum Kauf des Produkts geführt hat. Alle diese Aktionen führen zu mehr Datenverkehr und Konversionen.

Beispiel: Ein lokaler Koch besucht das Geschäft, um zu demonstrieren, wie ein Gourmet-Gericht gekocht wird. Das Geschäft bietet Anreize für die Teilnahme. So wird zum Beispiel eine kostenlose Vorspeise aus dem Restaurant des Kochs sowie ein zusätzlicher Anreiz zum Kauf der Zutaten für das vorgeführte Gericht angeboten (zum Beispiel 20 &euro; Rabatt auf einen Einkauf ab 150 &euro; Warenwert).

Die Lösung besteht aus den folgenden wichtigen Bestandteilen:
1. Analyse des Lagerbestands: Die Veranstaltungen im Geschäft (Rezepte, Zutatenlisten und Produktstandorte) sind auf den Verkauf der Bestände mit geringer Umschlaghäufigkeit ausgerichtet.
2. Eine mobile App für Kundentreue bietet zielgerichtetes Marketing mit digitalen Coupons, Einkaufslisten, Produktbeständen (Preise, Verfügbarkeit) auf einem Plan des Geschäfts sowie soziale Teilhabe.
3. Von einer Social-Media-Analyse werden persönliche Daten durch Ermittlung der Kundenvorlieben in Bezug auf Trends bereitgestellt: Küchen, Köche und Zutaten. In der Analyse werden regionale Trends mit den Aktivitäten der Person auf Twitter, Pinterest und Instagram verknüpft.
4. Entwicklerfreundliche Tools beschleunigen das Implementieren von Funktionen und Beheben von Fehlern.

Back-End-Lagerbestandssysteme für Produktbestand, Warennachschub und Produktvorhersage verfügen über eine Fülle an Informationen, aber moderne Analysen können Aufschluss über einen besseren Vertrieb hochwertiger Produkte geben. Mit einer Kombination aus {{site.data.keyword.cloudant}} und IBM Streaming Analytics kann der CMO die optimale Platzierung der Zutaten für die Veranstaltungen im Geschäft ermitteln.

{{site.data.keyword.messagehub_full}} dient als Plattform für Just-in-time-Ereignisse zum Integrieren der sich schnell ändernden Informationen von den Bestandssystemen in IBM Streaming Analytics.

Mithilfe einer Social-Media-Analyse und Watson Discovery (Persönlichkeits- und Tonfallanalyse) werden auch Informationen zu Trends für die Analyse des Lagerbestands bereitgestellt, um die Produktvorhersage zu verbessern.

Von der mobilen App für Kundentreue werden detaillierte Personalisierungsinformationen bereitgestellt, besonders wenn die Kunden ihre Funktionen für soziale Teilhaben nutzen, zum Beispiel beim Posting von Rezepten.

Zusätzlich zur mobilen App sind die Entwickler mit dem Aufbau und der Pflege der bestehenden Kundentreue-App beschäftigt, die an herkömmliche Coupons für die Kasse gebunden ist. Somit müssen sie sich auf die Codierung und nicht auf die Verwaltung der Infrastruktur konzentrieren. Und schließlich wurde {{site.data.keyword.containerlong_notm}} ausgewählt, weil IBM die Verwaltung der Infrastruktur vereinfacht:
* Verwaltung des Kubernetes-Masters, von Infrastructure as a Service (IaaS) und der Betriebskomponenten, wie zum Beispiel Ingress und Speicher
* Überwachen von Status und Wiederherstellung für Workerknoten
* Bereitstellung globaler Rechenkapazitäten, damit die Entwickler nicht für die Einrichtung der Infrastruktur in den Rechenzentren zuständig sind

**Lösungsmodell**

Bedarfsgerechte Rechenkapazität, Speicherkapazität und ein Ereignismanagement, das nach Bedarf in einer öffentlichen Cloud mit Zugriff auf Back-End-ERP-Systeme ausgeführt wird

Technische Lösung:
* {{site.data.keyword.containerlong_notm}}
* {{site.data.keyword.messagehub_full}}
* {{site.data.keyword.cloudant}}
* IBM Streaming Analytics
* IBM Watson Discovery

**Schritt 1: Containerisierte Apps mithilfe von Microservices**

* Strukturieren Sie die Bestandsanalyse und die mobilen Apps in Microservices und stellen Sie diese in Containern in {{site.data.keyword.containerlong_notm}} bereit.
* Stellen Sie standardisierte DevOps-Dashboards über Kubernetes bereit.
* Skalieren Sie die bedarfsgesteuerte Rechenleistung für Batch-Workloads und weitere Lagerbestands-Workloads, die weniger häufig ausgeführt werden.

** Schritt 2: Lagerbestand und Trends analysieren**
* {{site.data.keyword.messagehub_full}} dient als Plattform für Just-in-time-Ereignisse zum Integrieren der sich schnell ändernden Informationen von den Bestandssystemen in IBM Streaming Analytics.
* Die Social-Media-Analyse mit Watson Discovery und die Daten der Lagerbestandssysteme werden in IBM Streaming Analytics integriert, um Empfehlungen für die Sortimentierung und das Marketing zu erhalten.

**Schritt 3: Werbeaktionen mit mobiler Kundentreue-App bereitstellen**
* Schnelleinstieg in die Bereitstellung der mobilen App mit dem IBM Mobile-Starter-Kit und weiteren IBM Mobile-Services, wie zum Beispiel {{site.data.keyword.appid_full_notm}}.
* Werbeaktionen werden in Form von Coupons und anderen Berechtigungen an die mobile App der Benutzer gesendet. Die Werbeaktionen sind das Ergebnis aus der Analyse des Lagerbestands, der sozialen Medien und weiterer Back-End-Systeme.
* Das Speichern von Werberezepten auf mobilen Geräten und Konversionen (eingelöste Kassencoupons) werden zur weiteren Analyse in die ERP-Systeme eingespeist.

**Ergebnisse**
* Mit {{site.data.keyword.containerlong_notm}} wird die Bereitstellungszeit für Patches, Fehlerkorrekturen und neue Funktionen durch die Microservices erheblich reduziert. Erstmalige Entwicklungen verlaufen schnell und Aktualisierungen sind häufig.
* Da das Kundenaufkommen und der Umsatz in den Geschäften steigen, werden die Geschäften selbst zu einem Unterscheidungsmerkmal.
* Gleichzeitig führten neue Erkenntnisse aus der sozialen und kognitiven Analyse zu einer Senkung der Betriebsausgaben für den Lagerbestand.
* Die soziale Teilhabe in der mobilen App trägt auch dazu bei, dass sich neue Kunden mit dem Geschäft identifizieren; sie helfen dem Geschäft auch beim Marketing gegenüber neuen Kunden.
