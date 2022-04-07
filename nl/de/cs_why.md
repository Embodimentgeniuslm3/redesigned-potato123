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



# Warum {{site.data.keyword.containerlong_notm}}?
{: #cs_ov}

{{site.data.keyword.containerlong}} bietet durch die Kombination von Docker-Containern, der Kubernetes-Technologie, einer intuitiven Benutzererfahrung und integrierter Sicherheit und Isolation leistungsfähige Tools, um die Bereitstellung, den Betrieb, das Skalieren und die Überwachung von containerisierten Apps in einem Cluster aus Rechenhosts zu automatisieren. Zertifizierungsinformationen finden Sie unter [Compliance in {{site.data.keyword.Bluemix_notm}} ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/cloud/compliance).
{:shortdesc}


## Vorteile durch die Verwendung des Service
{: #benefits}

Cluster werden auf Rechenhosts bereitgestellt, die native Kubernetes-Funktionalität und durch {{site.data.keyword.IBM_notm}} hinzugefügte Funktionen zur Verfügung stellen.
{:shortdesc}

|Vorteil|Beschreibung|
|-------|-----------|
|Single-Tenant-Kubernetes-Cluster mit Isolation der Rechen-, Netz- und Speicherinfrastruktur|<ul><li>Erstellen Sie Ihre eigene angepasste Infrastruktur, die den Anforderungen Ihrer Organisation entspricht.</li><li>Ermöglicht die Einrichtung eines dedizierten und geschützten Kubernetes-Masters sowie von Workerknoten, virtuellen Netzen und Speicher unter Nutzung der von IBM Cloud Infrastructure (SoftLayer) bereitgestellten Ressourcen.</li><li>Der komplett verwaltete Kubernetes-Master, der kontinuierlich von {{site.data.keyword.IBM_notm}} überwacht und aktualisiert wird, um Ihren Cluster verfügbar zu halten.</li><li>Option zum Bereitstellen von Workerknoten als Bare-Metal-Server mit Trusted Compute.</li><li>Ermöglicht das Speichern persistenter Daten, die gemeinsame Nutzung von Daten durch Kubernetes-Pods und bei Bedarf die Wiederherstellung von Daten mit dem integrierten und sicheren Datenträgerservice.</li><li>Bietet volle Unterstützung für alle nativen Kubernetes-APIs.</li></ul>|
| Mehrzonencluster zur Verstärkung der Hochverfügbarkeit | <ul><li>Workerknoten desselben Maschinentyps (CPU, Speicher, virtuell oder physisch) können einfach mit Worker-Pools verwaltet werden.</li><li>Schutz gegen Zonenausfälle, indem Knoten gleichmäßig über ausgewählte Zonen verteilt werden und indem für Ihre Apps Podbereitstellungen mit Anti-Affinität verwendet werden.</li><li>Verringern Sie Ihre Kosten, indem Sie Mehrzonencluster verwenden, anstatt die Ressourcen in einem separaten Cluster zu duplizieren.</li><li>Profitieren Sie von dem App-übergreifenden automatischen Lastausgleich durch die 'Lastausgleichsfunktion für mehrere Zonen', die für Sie in jeder Zone des Clusters automatisch eingerichtet wird.</li></ul>|
| Hoch verfügbare Master | <ul><li>Reduzieren Sie Clusterausfallzeiten, beispielsweise während Aktualisierungen des Masters, mithilfe von hoch verfügbaren Mastern, die beim Erstellen eines Clusters automatisch bereitgestellt werden.</li><li>Verteilen Sie Ihre Master über mehrere Zonen in einem [Mehrzonencluster](/docs/containers?topic=containers-plan_clusters#multizone), um Ihren Cluster vor Zonenausfällen zu schützen.</li></ul> |
|Einhaltung von Sicherheitsbestimmungen für Images mit Vulnerability Advisor|<ul><li>Richten Sie Ihr eigenes Repository in unserer geschützten privaten Docker-Image-Registry ein, in der Images gespeichert und von allen Benutzern der Organisation gemeinsam genutzt werden.</li><li>Profitieren Sie durch das automatische Scannen von Images in Ihrer privaten {{site.data.keyword.Bluemix_notm}}-Registry.</li><li>Ermöglicht die Überprüfung von Empfehlungen für das im Image verwendete Betriebssystem, um potenzielle Schwachstellen zu beheben.</li></ul>|
|Kontinuierliche Überwachung des Clusterzustands|<ul><li>Über das Cluster-Dashboard können Sie den Zustand Ihrer Cluster, Workerknoten und Containerbereitstellungen rasch anzeigen und verwalten.</li><li>Ermöglicht die Feststellung detaillierter Metriken zur Auslastung mit {{site.data.keyword.monitoringlong}} und die rasche Erweiterung des Clusters als Reaktion auf die Arbeitslast.</li><li>Stellt detaillierte Protokollinformationen zu Clusteraktivitäten über den {{site.data.keyword.loganalysislong}} bereit.</li></ul>|
|Sichere Offenlegung von Apps gegenüber der Allgemeinheit|<ul><li>Sie haben die Auswahl zwischen einer öffentlichen IP-Adresse, einer von {{site.data.keyword.IBM_notm}} bereitgestellten Route oder Ihrer eigenen angepassten Domäne, um aus dem Internet auf Services in Ihrem Cluster zuzugreifen.</li></ul>|
|Integration des {{site.data.keyword.Bluemix_notm}}-Service|<ul><li>Ermöglicht das Hinzufügen zusätzlicher Funktionalität zu Ihrer App durch die Integration von {{site.data.keyword.Bluemix_notm}}-Services wie zum Beispiel Watson-APIs, Blockchain, Datenservices oder Internet of Things.</li></ul>|
{: caption="Vorteile von {{site.data.keyword.containerlong_notm}}" caption-side="top"}

Bereit loszulegen? Erkunden Sie das [Lernprogramm zum Erstellen eines Kubernetes-Clusters](/docs/containers?topic=containers-cs_cluster_tutorial#cs_cluster_tutorial).

<br />


## Vergleich von Angeboten und ihren Kombinationen
{: #differentiation}

Sie können {{site.data.keyword.containerlong_notm}} in {{site.data.keyword.Bluemix_notm}} Public, in {{site.data.keyword.Bluemix_notm}} Private oder in einem Hybridsetup ausführen.
{:shortdesc}


<table>
<caption>Unterschiede zwischen den {{site.data.keyword.containerlong_notm}}-Konfigurationen</caption>
<col width="22%">
<col width="78%">
 <thead>
 <th>{{site.data.keyword.containerlong_notm}}-Setup</th>
 <th>Beschreibung</th>
 </thead>
 <tbody>
 <tr>
 <td>{{site.data.keyword.Bluemix_notm}} Public
 </td>
 <td>Mit {{site.data.keyword.Bluemix_notm}} Public auf [gemeinsam genutzter oder dedizierter Hardware oder auf Bare-Metal-Maschinen](/docs/containers?topic=containers-plan_clusters#shared_dedicated_node) können Sie Ihre Apps mithilfe von {{site.data.keyword.containerlong_notm}} in Clustern in der Cloud hosten. Sie können auch einen Cluster mit Worker-Pools in mehreren Zonen erstellen, um die Hochverfügbarkeit für Ihre Apps zu verstärken. {{site.data.keyword.containerlong_notm}} unter {{site.data.keyword.Bluemix_notm}} Public bietet durch die Kombination von Docker-Containern, der Kubernetes-Technologie, einer intuitiven Benutzererfahrung und integrierter Sicherheit und Isolation leistungsfähige Tools, um die Bereitstellung, den Betrieb, das Skalieren und die Überwachung von containerisierten Apps in einem Cluster aus Rechenhosts zu automatisieren.<br><br>Weitere Informationen finden Sie unter [{{site.data.keyword.containerlong_notm}}-Technologie](/docs/containers?topic=containers-ibm-cloud-kubernetes-service-technology).
 </td>
 </tr>
 <tr>
 <td>{{site.data.keyword.Bluemix_notm}} Private
 </td>
 <td>{{site.data.keyword.Bluemix_notm}} Private ist eine Anwendungsplattform, die lokal auf Ihren Maschinen installiert werden kann. Sie können Kubernetes in {{site.data.keyword.Bluemix_notm}} Private verwenden, wenn Sie lokale, containerisierte Apps in Ihrer eigenen kontrollierten Umgebung hinter einer Firewall entwickeln und verwalten müssen. <br><br>Weitere Informationen finden Sie unter [{{site.data.keyword.Bluemix_notm}} Private – Produktdokumentation ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_1.2.0/kc_welcome_containers.html).
 </td>
 </tr>
 <tr>
 <td>Hybridsetup
 </td>
 <td>Hybrid steht für die kombinierte Nutzung von Services, die in {{site.data.keyword.Bluemix_notm}} Public und anderen lokal ausgeführten Services ausgeführt werden, z. B. eine App in {{site.data.keyword.Bluemix_notm}} Private. Beispiele für ein Hybridsetup: <ul><li>Bereitstellung eines Clusters mit {{site.data.keyword.containerlong_notm}} in {{site.data.keyword.Bluemix_notm}} Public, aber Verbindung dieses Clusters mit einer lokalen Datenbank.</li><li>Bereitstellung eines Clusters mit {{site.data.keyword.containerlong_notm}} in {{site.data.keyword.Bluemix_notm}} Private und Implementierung einer App in diesem Cluster. Diese App kann jedoch einen {{site.data.keyword.ibmwatson}}-Service wie {{site.data.keyword.toneanalyzershort}} in {{site.data.keyword.Bluemix_notm}} Public nutzen.</li></ul><br>Um die Kommunikation zwischen Services zu aktivieren, die in {{site.data.keyword.Bluemix_notm}} Public und lokale ausgeführten Services ausgeführt werden, müssen Sie eine [VPN-Verbindung einrichten](/docs/containers?topic=containers-vpn). Weitere Informationen finden Sie unter [{{site.data.keyword.containerlong_notm}} mit {{site.data.keyword.Bluemix_notm}} Private verwenden](/docs/containers?topic=containers-hybrid_iks_icp).
 </td>
 </tr>
 </tbody>
</table>

<br />


## Vergleich von kostenlosen Clustern und Standardclustern
{: #cluster_types}

Sie können einen kostenlosen Cluster oder beliebig viele Standardcluster erstellen. Testen Sie kostenlose Cluster, um sich mit einigen Kubernetes-Leistungsmerkmalen vertraut zu machen. Oder erstellen Sie Standardcluster, um das vollständige Leistungsspektrum von Kubernetes zum Bereitstellen von Apps zu nutzen. Freie Cluster werden nach 30 Tagen automatisch gelöscht.
{:shortdesc}

Wenn Sie einen kostenlosen Cluster haben und ein Upgrade auf einen Standardcluster durchführen wollen, können Sie [einen Standardcluster erstellen](/docs/containers?topic=containers-clusters#clusters_ui). Stellen Sie anschließend alle YAML-Dateien für die Kubernetes-Ressourcen, die Sie mit Ihrem kostenlosen Cluster erstellt haben, im Standardcluster bereit.

|Merkmale|Kostenlose Cluster|Standardcluster|
|---------------|-------------|-----------------|
|[Netzbetrieb in Clustern](/docs/containers?topic=containers-security#network)|<img src="images/confirm.svg" width="32" alt="Funktion verfügbar" style="width:32px;" />|<img src="images/confirm.svg" width="32" alt="Funktion verfügbar" style="width:32px;" />|
|[Zugriff auf Apps über ein öffentliches Netz mithilfe eines NodePort-Service auf eine nicht stabile IP-Adresse](/docs/containers?topic=containers-nodeport)|<img src="images/confirm.svg" width="32" alt="Funktion verfügbar" style="width:32px;" />|<img src="images/confirm.svg" width="32" alt="Funktion verfügbar" style="width:32px;" />|
|[Benutzerzugriffsverwaltung](/docs/containers?topic=containers-users#access_policies)|<img src="images/confirm.svg" width="32" alt="Funktion verfügbar" style="width:32px;" />|<img src="images/confirm.svg" width="32" alt="Funktion verfügbar" style="width:32px;" />|
|[Zugriff auf den {{site.data.keyword.Bluemix_notm}}-Service von Cluster und Apps](/docs/containers?topic=containers-service-binding#bind-services)|<img src="images/confirm.svg" width="32" alt="Funktion verfügbar" style="width:32px;" />|<img src="images/confirm.svg" width="32" alt="Funktion verfügbar" style="width:32px;" />|
|[Plattenspeicher auf Workerknoten für nicht persistenten Speicher](/docs/containers?topic=containers-storage_planning#non_persistent_overview)|<img src="images/confirm.svg" width="32" alt="Funktion verfügbar" style="width:32px;" />|<img src="images/confirm.svg" width="32" alt="Funktion verfügbar" style="width:32px;" />|
| [Möglichkeit zum Erstellen eines Clusters in jeder {{site.data.keyword.containerlong_notm}}-Region](/docs/containers?topic=containers-regions-and-zones) | | <img src="images/confirm.svg" width="32" alt="Funktion verfügbar" style="width:32px;" /> |
|[Mehrzonencluster zur Verstärkung der Hochverfügbarkeit](/docs/containers?topic=containers-plan_clusters#multizone) | |<img src="images/confirm.svg" width="32" alt="Funktion verfügbar" style="width:32px;" />|
| Replizierte Master für höhere Verfügbarkeit | | <img src="images/confirm.svg" width="32" alt="Funktion verfügbar" style="width:32px;" /> |
|[Skalierbare Anzahl von Workerknoten zur Steigerung der Kapazität](/docs/containers?topic=containers-app#app_scaling)| |<img src="images/confirm.svg" width="32" alt="Funktion verfügbar" style="width:32px;" />|
|[Persistenter NFS-dateibasierter Speicher mit Datenträgern](/docs/containers?topic=containers-file_storage#file_storage)| |<img src="images/confirm.svg" width="32" alt="Funktion verfügbar" style="width:32px;" />|
|[Öffentlicher oder privater App-Zugriff auf eine stabile IP-Adresse durch einen Netzausgleichsfunktions- (NLB-) Service](/docs/containers?topic=containers-loadbalancer)| |<img src="images/confirm.svg" width="32" alt="Funktion verfügbar" style="width:32px;" />|
|[Öffentlicher App-Zugriff auf eine stabile IP-Adresse und eine anpassbare URL durch einen Ingress-Service](/docs/containers?topic=containers-ingress#planning)| |<img src="images/confirm.svg" width="32" alt="Funktion verfügbar" style="width:32px;" />|
|[Portierbare öffentliche IP-Adressen](/docs/containers?topic=containers-subnets#review_ip)| |<img src="images/confirm.svg" width="32" alt="Funktion verfügbar" style="width:32px;" />|
|[Protokollierung und Überwachung](/docs/containers?topic=containers-health#logging)| |<img src="images/confirm.svg" width="32" alt="Funktion verfügbar" style="width:32px;" />|
|[Option zum Bereitstellen der Workerknoten auf physischen Servern (Bare-Metal-Servern)](/docs/containers?topic=containers-plan_clusters#shared_dedicated_node) | |<img src="images/confirm.svg" width="32" alt="Funktion verfügbar" style="width:32px;" />|
|[Option zum Bereitstellen von Bare-Metal-Workern mit Trusted Compute](/docs/containers?topic=containers-plan_clusters#shared_dedicated_node) | |<img src="images/confirm.svg" width="32" alt="Funktion verfügbar" style="width:32px;" />|
{: caption="Merkmale von kostenlosen Clustern und Standardclustern" caption-side="top"}
