---

copyright:
  years: 2014, 2019
lastupdated: "2019-04-16"

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



# Zuständigkeiten bei der Verwendung von {{site.data.keyword.containerlong_notm}}
{: #responsibilities_iks}

Erfahren Sie mehr über die Zuständigkeiten und Nutzungsbedingungen bei der Clusterverwaltung, wenn Sie {{site.data.keyword.containerlong}} verwenden.
{:shortdesc}

## Zuständigkeiten bei der Clusterverwaltung
{: #responsibilities}

IBM stellt Ihnen eine Cloudplattform für Unternehmen bereit, auf der Sie Apps zusammen mit {{site.data.keyword.Bluemix_notm}} DevOps-, AI-, Daten- und Sicherheitsservices bereitstellen können. Sie selbst entscheiden, wie Sie Ihre Apps und Services in der Cloud einrichten, integrieren und betreiben.
{:shortdesc}

<table summary="Die Tabelle zeigt die Verantwortlichkeiten von IBM und von Ihnen. Die Zeilen enthalten von links nach rechts gesehen Symbole für die jeweilige Verantwortlichkeit in Spalte eins und eine Beschreibung in Spalte zwei.">
<caption>Verantwortlichkeiten von IBM und von Ihnen</caption>
  <thead>
  <th colspan=2>Verantwortlichkeiten nach Typ</th>
  </thead>
  <tbody>
    <tr>
    <td align="center"><img src="images/icon_clouddownload.svg" alt="Symbol einer Wolke mit nach unten zeigendem Pfeil"/><br>Cloud-Infrastruktur</td>
    <td>
    **Leistungsumfang**:
    <ul><li>Bereitstellung eines vollständig verwalteten, dedizierten Hochverfügbarkeits-Masters in einem sicheren Infrastrukturkonto, dessen Eigner IBM ist, für jeden Cluster.</li>
    <li>Bereitstellung von Workerknoten in Ihrem Konto der IBM Cloud-Infrastruktur (SoftLayer).</li>
    <li>Einrichtung von Clusterverwaltungskomponenten wie VLANS und Lastausgleichsfunktionen.</li>
    <li>Erfüllen von Anforderungen nach weiterer Infrastruktur, wie z. B. zum Hinzufügen und Entfernen von Workerknoten, Erstellen von Standardteilnetzen und Bereitstellung von Speicherdatenträgern als Antwort auf Anforderungen für persistente Datenträger.</li>
    <li>Integration von bestellten Infrastrukturressourcen für die automatische Arbeit mit Ihrer Clusterarchitektur und Verfügbarkeit für Ihre bereitgestellten Apps und Workloads.</li></ul>
    <br><br>
    **Verantwortlichkeiten des Kunden**:
    <ul><li>Verwendung der bereitgestellten API-, CLI- und Konsolentools zum Anpassen der [Rechen](/docs/containers?topic=containers-clusters#clusters)- und [Speicher](/docs/containers?topic=containers-storage_planning#storage_planning)-Kapazität und der [Netzkonfiguration](/docs/containers?topic=containers-cs_network_cluster#cs_network_cluster) entsprechend den Anforderungen Ihrer Workloads.</li></ul><br><br>
    </td>
     </tr>
     <tr>
     <td align="center"><img src="images/icon_tools.svg" alt="Symbol eines Schraubenschlüssels"/><br>Verwalteter Cluster</td>
     <td>
     **Leistungsumfang**:
    <ul><li>Bereitstellung einer Tool-Suite zur Automatisierung der Clusterverwaltung, wie z. B. {{site.data.keyword.containerlong_notm}}-[API ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://containers.cloud.ibm.com/swagger-api/), -[CLI-Plug-in](/docs/containers?topic=containers-cs_cli_reference#cs_cli_reference) oder -[Konsole ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/kubernetes/clusters).</li>
     <li>Automatische Anwendung von Betriebssystem, Version und Sicherheit des Kubernetes Master-Patches. Verfügbarmachen von Haupt- und Nebenversionen, die Sie anwenden können.</li>
     <li>Verwalten und Wiederherstellen aktiver {{site.data.keyword.containerlong_notm}}- und Kubernetes-Komponenten im Cluster, z. B. Ingress-Lastausgleichsfunktion für Anwendungen oder Dateispeicher-Plug-in.</li>
     <li>Sicherung und Wiederherstellung von Daten in 'etcd', z. B. Ihre Konfigurationsdateien für Kubernetes-Workloads</li>
     <li>Einrichten einer OpenVPN-Verbindung zwischen Master- und Workerknoten bei der Erstellung des Clusters.</li>
     <li>Überwachen und Berichten des Status der Master- und Workerknoten in den verschiedenen Schnittstellen.</li>
     <li>Bereitstellung von Haupt-, Neben und Patchversionen für Betriebssystem, Version und Sicherheit des Workerknotens.</li>
     <li>Erfüllen von Automatisierungsanforderungen für die Aktualisierung und Wiederherstellung von Workerknoten. Bereitstellung der optionalen [automatischen Workerknoten-Wiederherstellung](/docs/containers?topic=containers-health#autorecovery).</li>
     <li>Bereitstellung von Tools wie z. B. [Cluster-Autoscaler](/docs/containers?topic=containers-ca#ca) zur Erweiterung Ihrer Clusterinfrastruktur.</li>
     </ul>
     <br><br>
     **Verantwortlichkeiten des Kunden**:
    <ul>
     <li>Verwendung der API-, CLI- oder Konsolentools für die [Anwendung](/docs/containers?topic=containers-update#update) der bereitgestellten Haupt- und Nebenversionen des Kubernetes-Masters sowie Haupt-, Neben- und Patchversionen für Workerknoten.</li>
     <li>Verwendung der API-, CLI- oder Konsolentools für die [Wiederherstellung](/docs/containers?topic=containers-cs_troubleshoot#cs_troubleshoot) Ihrer Infrastrukturressourcen oder Einrichtung und Konfiguration der optionalen [automatischen Workerknoten-Wiederherstellung](/docs/containers?topic=containers-health#autorecovery).</li></ul>
     <br><br></td>
      </tr>
    <tr>
      <td align="center"><img src="images/icon_locked.svg" alt="Schloss-Symbol"/><br>Umgebung mit umfassender Sicherheit</td>
      <td>
      **Leistungsumfang**:
    <ul>
      <li>Verwalten von Kontrollmechanismen entsprechend [verschiedenen Konformitätsstandards der Branche](/docs/containers?topic=containers-faqs#standards), z. B. PCI DSS.</li>
      <li>Überwachen, Isolieren und Wiederherstellen des Cluster-Masters.</li>
      <li>Bereitstellung hoch verfügbarer Replikate der Komponenten Kubernetes-Master-API-Server, 'etcd', Scheduler und Controller-Manager zum Schutz vor einem Masterausfall.</li>
      <li>Automatische Anwendung von Patchversionen für die Mastersicherheit sowie Bereitstellung von Patchversionen für die Workerknotensicherheit.</li>
      <li>Aktivierung bestimmter Sicherheitseinstellungen wie z. B. verschlüsselte Datenträger auf Workerknoten.</li>
      <li>Inaktivieren bestimmter unsicherer Aktionen für Workerknoten, z. B. Verhindern, dass Benutzer über eine SSH-Verbindung auf den Host zugreifen.</li>
      <li>Verschlüsseln der Kommunikation zwischen Master- und Worker-Knoten mit TLS.</li>
      <li>Bereitstellung von CIS-kompatiblen Linux-Images für Workerknoten-Betriebssysteme.</li>
      <li>Kontinuierliche Überwachung von Master- und Workerknoten-Images zur Erkennung von Sicherheitslücken und Fehlern bei der Einhaltung von Sicherheitsbestimmungen.</li>
      <li>Bereitstellung von Workerknoten mit zwei lokalen SSD-Datenpartitionen mit 256-Bit-AES-Verschlüsselung.</li>
      <li>Bereitstellung von Optionen für die Cluster-Netzkonnektivität, z. B. öffentliche und private Serviceendpunkte.</li>
      <li>Bereitstellung von Optionen für die Berechnung der Isolation, z. B. dedizierte virtuelle Maschinen, Bare-Metal und Bare-Metal mit Trusted Compute.</li>
      <li>Integration der rollenbasierten Zugriffssteuerung (RBAC) von Kubernetes mit {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM).</li>
      </ul>
      <br><br>
      **Verantwortlichkeiten des Kunden**:
    <ul>
      <li>Verwendung der API-, CLI- oder Konsolentools für die Anwendung der bereitgestellten [Sicherheitspatchversionen](/docs/containers?topic=containers-changelog#changelog) auf Ihre Workerknoten.</li>
      <li>Auswahl der Konfiguration für Ihr [Clusternetz](/docs/containers?topic=containers-cs_network_ov#cs_network_ov) und Konfiguration weiterer [Sicherheitseinstellungen](/docs/containers?topic=containers-security#security) entsprechend den Sicherheits- und Complianceanforderungen Ihrer Workloads. Gegebenenfalls Konfiguration Ihrer [Firewall](/docs/containers?topic=containers-firewall#firewall).</li></ul>
      <br><br></td>
      </tr>
      
      <tr>
        <td align="center"><img src="images/icon_code.svg" alt="Symbol mit Codeklammern"/><br>App-Orchestrierung</td>
        <td>
        **Leistungsumfang**:
    <ul>
        <li>Bereitstellung von Clustern mit installierten Kubernetes-Komponenten, sodass Sie auf die Kubernetes-API zugreifen können.</li>
        <li>Bereitstellung einer Anzahl verwalteter Add-ons zur Erweiterung der Fähigkeiten Ihrer App, z. B. [Istio](/docs/containers?topic=containers-istio#istio) oder [Knative](/docs/containers?topic=containers-knative_tutorial#knative_tutorial). Die Wartung wird für Sie vereinfacht, da IBM die Installation und Aktualisierungen für die verwalteten Add-ons bereitstellt.</li>
        <li>Bereitstellung der Clusterintegration mit ausgewählten Partnerschaftstechnologien von Drittanbietern, z. B. {{site.data.keyword.la_short}}, {{site.data.keyword.mon_short}} oder Portworx.</li>
        <li>Bereitstellung von Automatisierung, um die Servicebindung für andere {{site.data.keyword.Bluemix_notm}}-Services zu aktivieren</li>
        <li>Erstellung von Clustern mit geheimen Schlüsseln für Image-Pull-Operationen, sodass Ihre Bereitstellungen im Kubernetes-Namensbereich `default` Images aus {{site.data.keyword.registrylong_notm}} extrahieren können.</li>
        <li>Bereitstellung von Speicherklassen und Plug-ins, um persistente Datenträger für die Verwendung mit Ihren Apps zu unterstützen.</li>
        <li>Erstellung von Clustern mit IP-Teilnetzadressen, die dafür reserviert sind, Anwendungen extern zugänglich zu machen.</li>
        <li>Unterstützung nativer öffentlicher und privater Kubernetes-Lastausgleichsfunktionen und Ingress-Routen, um Services extern zugänglich zu machen.</li>
        </ul>
        <br><br>
        **Verantwortlichkeiten des Kunden**:
    <ul>
        <li>Verwendung der bereitgestellten Tools und Funktionen für die [Konfiguration und Bereitstellung](/docs/containers?topic=containers-app#app), [Einrichtung von Berechtigungen](/docs/containers?topic=containers-users#users), [Integration mit anderen Services](/docs/containers?topic=containers-supported_integrations#supported_integrations), für [externen Service](/docs/containers?topic=containers-cs_network_planning#cs_network_planning), die [Statusüberwachung](/docs/containers?topic=containers-health#health), [Speicherung, Sicherung und Wiederherstellung von Daten](/docs/containers?topic=containers-storage_planning#storage_planning) und sonstige Verwaltung Ihrer [hoch verfügbaren](/docs/containers?topic=containers-ha#ha) und widerstandsfähigen Workloads.</li>
        </ul>
        </td>
        </tr>
  </tbody>
  </table>

<br />


## Unsachgemäßer Gebrauch von {{site.data.keyword.containerlong_notm}}
{: #terms}

Der unsachgemäße Gebrauch von {{site.data.keyword.containerlong_notm}} ist untersagt.
{:shortdesc}

Ein unsachgemäßer Gebrauch umfasst:

*   Illegale Aktivitäten
*   Verteilung und Ausführung von Malware
*   {{site.data.keyword.containerlong_notm}} beschädigen oder Verwendung von {{site.data.keyword.containerlong_notm}} durch einen anderen Benutzer stören
*   Schäden verursachen oder Verwendung durch einen anderen Benutzer stören
*   Unbefugter Zugriff auf Services oder Systeme
*   Unberechtigte Änderung von Services oder Systemen
*   Verletzung von Rechten anderer Benutzer

Alle Nutzungsbedingungen finden Sie unter [Bedingungen für Cloud-Services](https://cloud.ibm.com/docs/overview/terms-of-use/notices.html#terms).
