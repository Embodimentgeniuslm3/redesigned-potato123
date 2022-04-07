---

copyright:
  years: 2014, 2019
lastupdated: "2019-04-15"

keywords: kubernetes, iks, clusters, worker nodes, worker pools, delete

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
{:gif: data-image-type='gif'}


# Cluster und Workerknoten einrichten
{: #clusters}
Erstellen Sie Cluster und fügen Sie Workerknoten hinzu, um die Clusterkapazität in {{site.data.keyword.containerlong}} zu erhöhen. Machen Sie sich noch mit der Anwendung vertraut? Sehen Sie sich dazu das [Lernprogramm zum Erstellen von Kubernetes-Clustern](/docs/containers?topic=containers-cs_cluster_tutorial#cs_cluster_tutorial) an.
{: shortdesc}

## Erstellung von Clustern vorbereiten
{: #cluster_prepare}

Mit {{site.data.keyword.containerlong_notm}} können Sie eine hoch verfügbare und sichere Umgebung für Apps mit vielen zusätzlichen Funktionen erstellen, die entweder integriert sind oder konfiguriert werden können. Aufgrund der vielen Möglichkeiten, über die Sie bei der Verwendung von Clustern verfügen, müssen Sie auch viele Entscheidungen treffen, wenn Sie einen Cluster erstellen. Anhand der folgenden Schritte wird beschrieben, was Sie bei der Einrichtung des Kontos, der Berechtigungen, der Ressourcengruppen, des VLAN-Spannings, der Clusterkonfiguration für Zone und Hardware sowie der Rechnungsinformationen berücksichtigen müssen.
{: shortdesc}

Die Liste besteht aus zwei Teilen:
*  **Kontoebene:** Hierbei handelt es sich um Vorbereitungen, deren Änderung nach der Ausführung durch den Kontoadministrator nicht bei jeder Erstellung eines Clusters erforderlich ist. Es kann jedoch vorkommen, dass Sie bei jeder Erstellung eines Clusters überprüfen möchten, ob der aktuelle Status der Kontoebene dem gewünschten Status entspricht.
*  **Clusterebene:** Hierbei handelt es sich um Vorbereitungen, die sich bei jeder Erstellung eines Clusters auf Ihren Cluster auswirken.

### Kontoebene
{: #prepare_account_level}

Befolgen Sie die Schritte zur Vorbereitung Ihres {{site.data.keyword.Bluemix_notm}}-Kontos für {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

1.  [Erstellen Sie ein belastbares Konto oder führen Sie für Ihr Konto ein Upgrade auf ein belastbares Konto durch ({{site.data.keyword.Bluemix_notm}}nutzungsabhängig oder Abonnement)](https://cloud.ibm.com/registration/).
2.  [Konfigurieren Sie einen {{site.data.keyword.containerlong_notm}}-API-Schlüssel](/docs/containers?topic=containers-users#api_key) in den Regionen, in denen Sie Cluster erstellen möchten. Ordnen Sie den API-Schlüssel den entsprechenden Berechtigungen zum Erstellen von Clustern hinzu:
    *  Die Rolle **Superuser** für die IBM Cloud-Infrastruktur (SoftLayer).
    *  Die Plattformmanagementrolle **Administrator** für {{site.data.keyword.containerlong_notm}} auf Kontoebene.
    *  Die Plattformmanagementrolle **Administrator** für {{site.data.keyword.registrylong_notm}} auf Kontoebene. Wenn Ihr Konto vor dem 4. Oktober 2018 erstellt wurde, müssen Sie [{{site.data.keyword.Bluemix_notm}} IAM-Richtlinien für {{site.data.keyword.registryshort_notm}} aktivieren](/docs/services/Registry?topic=registry-user#existing_users). Mit IAM-Richtlinien können Sie den Zugriff auf Ressourcen wie Registry-Namensbereiche steuern.

    Sind Sie der Kontoeigner? Sie verfügen bereits über die erforderlichen Berechtigungen. Wenn Sie einen Cluster erstellen, wird der API-Schlüssel für diese Region und Ressourcengruppe mit den Berechtigungsnachweisen festgelegt.
    {: tip}

3.  Wenn vom Konto mehrere Ressourcengruppen verwendet werden, ermitteln Sie die Strategie Ihres Kontos für die [Verwaltung von Ressourcengruppen](/docs/containers?topic=containers-users#resource_groups). 
    *  Der Cluster wird in der Ressourcengruppe erstellt, die Sie während der Anmeldung an {{site.data.keyword.Bluemix_notm}} als Ziel angeben. Wenn Sie keine Ressourcengruppe als Ziel angeben, wird automatisch die Standardressourcengruppe als Ziel verwendet.
    *  Wenn Sie einen Cluster in einer anderen Ressourcengruppe als der Standardressourcengruppe erstellen möchten, benötigen Sie mindestens die Rolle **Anzeigeberechtigter** für die Ressourcengruppe. Falls Sie über keine Rolle für die Ressourcengruppe verfügen, aber ein **Administrator** für den Service in der Ressourcengruppe sind, wird der Cluster in der Standardressourcengruppe erstellt.
    *  Die Ressourcengruppe eines Clusters kann nicht geändert werden. Wenn Sie außerdem den [Befehl](/docs/containers-cli-plugin?topic=containers-cli-plugin-cs_cli_reference#cs_cluster_service_bind) `ibmcloud ks cluster-service-bind` verwenden müssen, um eine [Integration mit einem {{site.data.keyword.Bluemix_notm}}-Service durchzuführen](/docs/containers?topic=containers-service-binding#bind-services), muss sich dieser Service in derselben Ressourcengruppe befinden wie der Cluster. Services, die keine Ressourcengruppen verwenden, wie {{site.data.keyword.registrylong_notm}}, oder die keine Servicebindung benötigen, wie {{site.data.keyword.la_full_notm}}, funktionieren auch dann, wenn sich der Cluster in einer anderen Ressourcengruppe befindet.
    *  Wenn Sie beabsichtigen, [{{site.data.keyword.monitoringlong_notm}} für Metriken](/docs/containers?topic=containers-health#view_metrics) zu verwenden, planen Sie, dem Cluster einen Namen zu geben, der in allen Ressourcengruppen und Regionen des Kontos eindeutig ist, um Konflikte bei Metriknamen zu vermeiden.

4.  Richten Sie Ihr IBM Cloud-Infrastrukturnetz (SoftLayer) ein. Sie können unter folgenden Optionen auswählen:
    *  **VRF-aktiviert:** Mit Virtual Routing and Rorwarding (VRF) und der zugehörigen Separationstechnologie für mehrfache Isolation können Sie öffentliche und private Serviceendpunkte für die Kommunikation mit Ihrem Kubernetes-Master in Clustern verwenden, die Kubernetes Version 1.11 oder höher ausführen. Bei Verwendung des [privaten Serviceendpunkts](/docs/containers?topic=containers-cs_network_ov#cs_network_ov_master_private) verbleibt die Kommunikation zwischen dem Kubernetes-Master und Ihren Workerknoten im privaten VLAN. Wenn Sie `kubectl`-Befehle über Ihre lokale Maschine für Ihren Cluster ausführen wollen, müssen Sie mit demselben privaten VLAN verbunden sein, in dem sich auch Ihr Kubernetes-Master befindet. Wenn Ihre Apps für das Internet zugänglich gemacht werden sollen, müssen Ihre Workerknoten mit einem öffentlichen VLAN verbunden werden, sodass eingehender Netzverkehr an Ihre Apps weitergeleitet werden kann. Zur Ausführung von `kubectl`-Befehlen für Ihren Cluster über das Internet können Sie den öffentlichen Serviceendpunkt verwenden. Mit dem öffentlichen Serviceendpunkt wird Netzverkehr über das öffentliche VLAN geleitet und durch einen OpenVPN-Tunnel geschützt. Zur Verwendung privater Serviceendpunkte müssen Sie Ihr Konto für VRF und Serviceendpunkte aktivieren, wozu das Öffnen eines Supportfalls für die IBM Cloud-Infrastruktur (SoftLayer) erforderlich ist. Weitere Informationen finden Sie in der [Übersicht über VRF in {{site.data.keyword.Bluemix_notm}}](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) und im Abschnitt [Konto für Serviceendpunkte aktivieren](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started).
    *  **Nicht-VRF:** Wenn Sie VRF für Ihr Konto nicht aktivieren wollen oder können, können Ihre Workerknoten automatisch eine Verbindung zum Kubernetes-Master über den [öffentlichen Serviceendpunkt](/docs/containers?topic=containers-cs_network_ov#cs_network_ov_master_public) herstellen. Zum Schutz dieser Kommunikation richtet {{site.data.keyword.containerlong_notm}} bei der Erstellung des Clusters automatisch eine OpenVPN-Verbindung zwischen dem Kubernetes-Master und den Workerknoten ein. Wenn Sie über mehrere VLANs für einen Cluster, mehrere Teilnetze in demselben VLAN oder einen Cluster mit mehreren Zonen verfügen, müssen Sie [VLAN-Spanning](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning) für Ihr Konto für die IBM Cloud-Infrastruktur (SoftLayer) aktivieren, damit die Workerknoten in dem privaten Netz miteinander kommunizieren können. Um diese Aktion durchführen zu können, müssen Sie über die [Infrastrukturberechtigung](/docs/containers?topic=containers-users#infra_access) **Netz > VLAN-Spanning im Netz verwalten** verfügen oder Sie können den Kontoeigner bitten, diese zu aktivieren. Zum Prüfen, ob das VLAN-Spanning bereits aktiviert ist, verwenden Sie den [Befehl](/docs/containers?topic=containers-cs_cli_reference#cs_vlan_spanning_get) `ibmcloud ks vlan-spanning-get`.

### Clusterebene
{: #prepare_cluster_level}

Befolgen Sie die Schritte zur Vorbereitung des Setups Ihres Clusters.
{: shortdesc}

1.  Stellen Sie sicher, dass Sie über die Plattformrolle **Administrator** für {{site.data.keyword.containerlong_notm}} verfügen. Damit Ihr Cluster Images aus der privaten Registry extrahieren kann, benötigen Sie auch die **Administrator**-Plattformrolle für {{site.data.keyword.registrylong_notm}}.
    1.  Klicken Sie in der Menüleiste der [{{site.data.keyword.Bluemix_notm}}-Konsole ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/) auf **Verwalten > Zugriff (IAM)**.
    2.  Klicken Sie auf die Seite **Benutzer** und anschließend in der Tabelle auf Ihren eigenen Benutzernamen.
    3.  Bestätigen Sie in der Registerkarte **Zugriffsrichtlinien**, dass Ihre **Rolle** **Administrator** ist. Sie können **Administrator** für alle Ressourcen im Konto oder zumindest für {{site.data.keyword.containershort_notm}} sein. **Hinweis:** Falls Sie über die Rolle **Administrator** für {{site.data.keyword.containershort_notm}} nur in einer Ressourcengruppe oder Region und nicht im gesamten Konto verfügen, müssen Sie mindestens auf Kontoebene über die Rolle **Anzeigeberechtigter** verfügen, um die VLANs des Kontos anzeigen zu können.
    <p class="tip">Stellen Sie sicher, dass Ihr Kontoadministrator Ihnen die **Administrator**-Plattformrolle nicht zur gleichen Zeit wie eine Servicerolle zugewiesen hat. Sie müssen Plattform- und Servicerollen separat zuweisen.</p>
2.  Entscheiden Sie sich zwischen einem [kostenlosen Cluster und einem Standardcluster](/docs/containers?topic=containers-cs_ov#cluster_types). Sie können einen kostenlosen Cluster erstellen, um 30 Tage lang einige der Leistungsmerkmale zu testen. Oder Sie können umfassend anpassbare Standardcluster mit Ihrer gewünschten Hardwareisolation erstellen. Erstellen Sie einen Standardcluster, um weitere Vorteile nutzen zu können und die Clusterleistung besser steuern zu können.
3.  [Planen Sie die Konfiguration des Clusters.](/docs/containers?topic=containers-plan_clusters#plan_clusters)
    *  Entscheiden Sie, ob ein Cluster aus einer [einzelnen Zone](/docs/containers?topic=containers-plan_clusters#single_zone) oder aus [mehreren Zonen](/docs/containers?topic=containers-plan_clusters#multizone) erstellt werden soll. Beachten Sie, dass Cluster aus mehreren Zonen nur an ausgewählten Standorten verfügbar sind.
    *  Wenn Sie einen Cluster erstellen möchten, der nicht öffentlich zugänglich ist, überprüfen Sie die zusätzlichen [Schritte für private Cluster](/docs/containers?topic=containers-plan_clusters#private_clusters).
    *  Wählen Sie den Typ der [Hardware und Isolation](/docs/containers?topic=containers-plan_clusters#shared_dedicated_node) aus, den Sie für die Workerknoten des Clusters verwenden möchten; entscheiden Sie hierbei auch, ob virtuelle Maschinen oder Bare-Metal-Maschinen verwendet werden sollen.
4.  Für Standardcluster können Sie in der {{site.data.keyword.Bluemix_notm}}-Konsole [die Kosten schätzen](/docs/billing-usage?topic=billing-usage-cost#cost). Weitere Informationen zu Gebühren, die möglicherweise nicht in den Schätzungen berücksichtigt werden, finden Sie unter [Preisstruktur und Abrechnung](/docs/containers?topic=containers-faqs#charges).
5.  Wenn Sie den Cluster in einer Umgebung hinter einer Firewall erstellen, [lassen Sie ausgehenden Netzdatenverkehr an die öffentlichen und privaten IP-Adressen](/docs/containers?topic=containers-firewall#firewall_outbound) für die {{site.data.keyword.Bluemix_notm}}-Services zu, deren Einsatz Sie planen.
<br>
<br>

**Womit möchten Sie fortfahren?**
* [Cluster über die {{site.data.keyword.Bluemix_notm}}-Konsole erstellen](#clusters_ui)
* [Cluster über die {{site.data.keyword.Bluemix_notm}}-CLI erstellen](#clusters_cli)

## Cluster über die {{site.data.keyword.Bluemix_notm}}-Konsole erstellen
{: #clusters_ui}

Der Zweck des Kubernetes-Clusters besteht darin, eine Gruppe von Ressourcen, Knoten, Netzen und Speichereinheiten zu definieren, die die Hochverfügbarkeit von Apps sicherstellen. Bevor Sie eine App bereitstellen können, müssen Sie zunächst einen Cluster erstellen und die Definitionen für die Workerknoten in diesem Cluster festlegen.
{:shortdesc}

Wollen Sie einen Cluster erstellen, der [Serviceendpunkte](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started) für die [Kommunikation zwischen Master- und Workerknoten](/docs/containers?topic=containers-cs_network_ov#cs_network_ov_master) verwendet? Sie müssen den Cluster [über die CLI](#clusters_cli) erstellen.
{: note}

### Kostenlosen Cluster erstellen
{: #clusters_ui_free}

Sie können sich mit Ihrem kostenlosen Cluster mit der Funktionsweise von {{site.data.keyword.containerlong_notm}} vertraut machen. Mit kostenlosen Clustern können Sie die Terminologie kennenlernen, das Lernprogramm ausführen und sich mit der Anwendung vertraut machen, bevor Sie den Sprung zu Standardclustern auf Produktebene wagen. Auch bei einem belastbaren Konto erhalten Sie einen kostenlosen Cluster.
{: shortdesc}

Kostenlose Cluster sind 30 Tage lang gültig. Nach Ablauf dieser Zeit verfällt der Cluster und der Cluster und seine Daten werden gelöscht. Die gelöschten Daten werden nicht von {{site.data.keyword.Bluemix_notm}} gesichert und können nicht wiederhergestellt werden. Stellen Sie sicher, dass alle wichtigen Daten gesichert werden.
{: note}

1. [Bereiten Sie sich darauf vor, einen Cluster zu erstellen](#cluster_prepare), damit Sie über das richtige {{site.data.keyword.Bluemix_notm}}-Konto-Setup und die korrekten Benutzerberechtigungen verfügen, und treffen Sie eine Entscheidung bezüglich des Cluster-Setups und der Ressourcengruppe, die Sie verwenden möchten.
2. Wählen Sie im [Katalog ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/catalog?category=containers) den Eintrag **{{site.data.keyword.containershort_notm}}** aus, um ein Cluster zu erstellen.
3. Wählen Sie eine Region aus, in der der Cluster bereitgestellt werden soll. Ihr Cluster wird in einer Zone innerhalb dieser Region erstellt.
4. Wählen Sie den **kostenlosen** Clusterplan aus.
5. Geben Sie Ihrem Cluster einen Namen. Der Name muss mit einem Buchstaben beginnen, darf Buchstaben, Ziffern und den Bindestrich (-) enthalten und darf maximal 35 Zeichen lang sein. Der vollständig qualifizierte Domänenname für die Ingress-Unterdomäne setzt sich aus dem Clusternamen und der Region zusammen, in der der Cluster bereitgestellt wird. Um sicherzustellen, dass die Ingress-Unterdomäne innerhalb einer Region eindeutig ist, wird der Clustername möglicherweise abgeschnitten und es wird ein beliebiger Wert innerhalb des Ingress-Domänennamens angehängt.

6. Klicken Sie auf **Cluster einrichten**. Standardmäßig wird ein Worker-Pool mit einem Workerknoten erstellt. Auf der Registerkarte **Workerknoten** können Sie den Fortschritt der Bereitstellung des Workerknotens überprüfen. Nach Abschluss der Bereitstellung können Sie auf der Registerkarte **Übersicht** sehen, dass Ihr Cluster bereit ist. Beachten Sie, dass selbst bei Bereitschaft des Clusters einige seiner Teile, die von anderen Services wie den geheimen Ingress-Schlüsseln oder geheimen Schlüsseln für Registry-Image-Pull-Operationen verwendet werden, möglicherweise noch in Verarbeitung sind.

    Wenn die eindeutige ID oder der Domänenname geändert wird, die/der während der Erstellung zugeordnet wird, kann der Kubernetes-Master Ihren Cluster nicht mehr verwalten.
    {: note}

</br>

### Standardcluster erstellen
{: #clusters_ui_standard}

1. [Bereiten Sie sich darauf vor, einen Cluster zu erstellen](#cluster_prepare), damit Sie über das richtige {{site.data.keyword.Bluemix_notm}}-Konto-Setup und die korrekten Benutzerberechtigungen verfügen, und treffen Sie eine Entscheidung bezüglich des Cluster-Setups und der Ressourcengruppe, die Sie verwenden möchten.
2. Wählen Sie im [Katalog ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/catalog?category=containers) den Eintrag **{{site.data.keyword.containershort_notm}}** aus, um ein Cluster zu erstellen.
3. Wählen Sie eine Ressourcengruppe aus, in der der Cluster erstellt werden soll.
  **Hinweis**:
    * Ein Cluster kann nur in einer Ressourcengruppe erstellt werden; nach der Erstellung des Clusters kann seine Ressourcengruppe nicht geändert werden.
    * In der Standardressourcengruppe werden automatisch freie Cluster erstellt.
    * Wenn Sie Cluster in einer anderen Ressourcengruppe als der Standardressourcengruppe erstellen möchten, müssen Sie mindestens über die [Rolle **Anzeigeberechtigter**](/docs/containers?topic=containers-users#platform) für die Ressourcengruppe verfügen.
4. Wählen Sie einen [{{site.data.keyword.Bluemix_notm}}-Standort](/docs/containers?topic=containers-regions-and-zones#regions-and-zones) aus, an dem Ihr Cluster bereitgestellt werden soll. Wählen Sie den Standort aus, der Ihrem Standort am nächsten ist, um eine optimale Leistung zu erhalten. Wenn Sie eine Zone auswählen, die sich außerhalb Ihres Landes befindet, müssen Sie daran denken, dass Sie vor dem Speichern der Daten möglicherweise eine entsprechende Berechtigung benötigen.
5. Wählen Sie den **Standard**-Cluster-Plan aus. Bei einem Standardcluster erhalten Sie Zugriff auf Features wie beispielsweise mehrere Workerknoten für eine hoch verfügbare Umgebung.
6. Geben Sie Ihre Zonendetails ein.
    1. Wählen Sie für die Verfügbarkeit die Option für **Einzelzonencluster** oder **Mehrzonencluster** aus. In einem Mehrzonencluster wird der Masterknoten in einer auf mehrere Zonen ausgelegten Zone bereitgestellt und die Ressourcen Ihres Clusters werden auf mehrere Zonen verteilt. Ihre Auswahl kann durch die Region begrenzt werden.
    2. Wählen Sie die Zonen aus, in denen der Cluster als Host für den Cluster verwendet werden soll. Sie müssen mindestens eine Zone auswählen, können jedoch so viele Zonen auswählen, wie Sie möchten. Wenn Sie mehr als eine Zone auswählen, werden die Workerknoten auf die Zonen verteilt, die Sie auswählen, wodurch eine höhere Verfügbarkeit erzielt werden kann. Wenn Sie nur eine Zone auswählen, [fügen Sie Zonen zu Ihrem Cluster hinzu](#add_zone), nachdem dieser erstellt wurde.
    3. Wählen Sie ein öffentliches VLAN (optional) und ein privates VLAN (erforderlich) in Ihrem IBM Cloud-Infrastrukturkonto (SoftLayer) aus. Workerknoten kommunizieren miteinander, indem sie das private VLAN verwenden. Um mit dem Kubernetes-Master kommunizieren zu können, müssen Sie die öffentliche Konnektivität für Ihren Workerknoten konfigurieren.  Wenn Sie in dieser Zone kein privates oder öffentliches VLAN haben, geben Sie diese Option nicht an. Ein privates und ein öffentliches VLAN werden automatisch für Sie erstellt. Wenn Sie über vorhandene VLANs verfügen und kein öffentliches VLAN angeben, sollten Sie eine Firewall konfigurieren, z. B. eine [Virtual Router Appliance](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-about-the-vra#about-the-vra). Sie können dasselbe VLAN für mehrere Cluster verwenden.
        Wenn Workerknoten nur mit einem privaten VLAN eingerichtet werden, müssen Sie die Kommunikation zwischen Workerknoten und Cluster-Master zulassen, indem Sie den [privaten Serviceendpunkt aktivieren](/docs/containers?topic=containers-cs_network_ov#cs_network_ov_master_private) oder eine [Gateway-Einheit konfigurieren](/docs/containers?topic=containers-cs_network_ov#cs_network_ov_master_gateway).
        {: note}

7. Konfigurieren Sie Ihren Standard-Worker-Pool. Worker-Pools sind Gruppen von Workerknoten, die dieselbe Konfiguration aufweisen. Sie können auch noch zu einem späteren Zeitpunkt weitere Worker-Pools hinzufügen.

    1. Wählen Sie einen Typ von Hardwareisolation aus. Virtuell wird auf Stundenbasis berechnet und Bare-Metal wird monatlich in Rechnung gestellt.

        - **Virtuell - Dediziert**: Ihre Workerknoten werden in einer Infrastruktur gehostet, die Ihrem Konto vorbehalten ist. Ihre physischen Ressourcen sind vollständig isoliert.

        - **Virtuell - Gemeinsam genutzt**: Infrastrukturressourcen wie der Hypervisor und physische Hardware werden von Ihnen und anderen IBM Kunden gemeinsam genutzt, aber jeder Workerknoten ist ausschließlich für Sie zugänglich. Obwohl diese Option in den meisten Fällen ausreicht und kostengünstiger ist, sollten Sie die Leistungs- und Infrastrukturanforderungen anhand der Richtlinien Ihres Unternehmens prüfen.

        - **Bare-Metal**: Bare-Metal-Server werden monatlich abgerechnet und nach Bestellung manuell von der IBM Cloud-Infrastruktur (SoftLayer) bereitgestellt, daher kann die Ausführung mehr als einen Arbeitstag dauern. Bare-Metal-Server eignen sich am besten für Hochleistungsanwendungen, die mehr Ressourcen und Hoststeuerung erfordern. Sie können auch [Trusted Compute](/docs/containers?topic=containers-security#trusted_compute) aktivieren, um Ihre Workerknoten auf Manipulation zu überprüfen. Trusted Compute ist für ausgewählte Bare-Metal-Maschinentypen verfügbar. Beispielsweise unterstützen die GPU-Typen `mgXc` Trusted Compute nicht. Wenn Sie Trusted Compute während der Clustererstellung nicht aktivieren, dies jedoch später nachholen möchten, können Sie den [Befehl](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_feature_enable) `ibmcloud ks feature-enable` verwenden. Nachdem Sie Trusted Compute aktiviert haben, können Sie es später nicht mehr inaktivieren.

        Stellen Sie sicher, dass Sie eine Bare-Metal-Maschine bereitstellen wollen. Da die Abrechnung monatlich erfolgt, wird Ihnen ein voller Monat auch dann berechnet, wenn Sie versehentlich bestellen und sofort danach Ihre Bestellung wieder stornieren.
        {:tip}

    2. Wählen Sie einen Maschinentyp aus. Der Maschinentyp definiert die Menge an virtueller CPU, Hauptspeicher und Festplattenspeicher, die in jedem Workerknoten eingerichtet wird und allen Containern zur Verfügung steht. Die verfügbaren Bare-Metal- und virtuellen Maschinentypen variieren je nach Zone, in der Sie den Cluster implementieren. Nachdem Sie Ihre Cluster erstellt haben, können Sie unterschiedliche Maschinentypen hinzufügen, indem Sie einen Worker oder Pool zum Cluster hinzufügen.

    3. Geben Sie die Anzahl der Workerknoten an, die Sie im Cluster benötigen. Die von Ihnen eingegebene Anzahl der Worker wird über die Anzahl der von Ihnen ausgewählten Zonen repliziert. Wenn Sie beispielsweise über zwei Zonen verfügen und drei Workerknoten auswählen, bedeutet das, dass sechs Knoten bereitgestellt werden und drei Knoten in jeder Zone vorhanden sind.

8. Geben Sie dem Cluster einen eindeutigen Namen. **Hinweis**: Wenn die eindeutige ID oder der Domänenname geändert wird, die/der während der Erstellung zugeordnet wird, kann der Kubernetes-Master Ihren Cluster nicht mehr verwalten.
9. Wählen Sie die Kubernetes-API-Serverversion für den Cluster-Masterknoten aus.
10. Klicken Sie auf **Cluster einrichten**. Ein Worker-Pool wird mit der Anzahl der Worker erstellt, die Sie angegeben haben. Auf der Registerkarte **Workerknoten** können Sie den Fortschritt der Bereitstellung des Workerknotens überprüfen. Nach Abschluss der Bereitstellung können Sie auf der Registerkarte **Übersicht** sehen, dass Ihr Cluster bereit ist. Beachten Sie, dass selbst bei Bereitschaft des Clusters einige seiner Teile, die von anderen Services wie den geheimen Ingress-Schlüsseln oder geheimen Schlüsseln für Registry-Image-Pull-Operationen verwendet werden, möglicherweise noch in Verarbeitung sind.

**Womit möchten Sie fortfahren?**

Wenn der Cluster betriebsbereit ist, können Sie sich mit den folgenden Tasks vertraut machen:

-   Wenn Sie den Cluster in einer mehrzonenfähigen Zone erstellt haben, verteilen Sie Workerknoten durch [Hinzufügen einer Zone zum Cluster](#add_zone).
-   [Installieren Sie die CLIs](/docs/containers?topic=containers-cs_cli_install#cs_cli_install) oder [starten Sie das Kubernetes-Terminal, um die CLIs direkt in Ihrem Web-Browser zu verwenden](/docs/containers?topic=containers-cs_cli_install#cli_web), um mit der Arbeit mit Ihrem Cluster zu beginnen.
-   [Stellen Sie eine App in Ihrem Cluster bereit.](/docs/containers?topic=containers-app#app_cli)
-   [Richten Sie Ihre eigene private Registry in {{site.data.keyword.Bluemix_notm}} ein, um Docker-Images zu speichern und gemeinsam mit anderen Benutzern zu verwenden.](/docs/services/Registry?topic=registry-getting-started)
-   Wenn Sie über eine Firewall verfügen, müssen Sie unter Umständen [die erforderlichen Ports öffnen](/docs/containers?topic=containers-firewall#firewall), um `ibmcloud`-, `kubectl`- oder `calicotl`-Befehle zu verwenden, um abgehenden Datenverkehr von Ihrem Cluster zu ermöglichen oder um eingehenden Datenverkehr für Netzservices zuzulassen.
-   [Richten Sie den Cluster-Autoscaler ein](/docs/containers?topic=containers-ca#ca), um Workerknoten in Ihren Worker-Pools automatisch entsprechend den Workloadressourcenanforderungen hinzuzufügen oder zu entfernen.
-   Sie können steuern, wer in Ihrem Cluster Pods mit [Pod-Sicherheitsrichtlinien](/docs/containers?topic=containers-psp) erstellen kann.

<br />


## Cluster über die {{site.data.keyword.Bluemix_notm}}-CLI erstellen
{: #clusters_cli}

Der Zweck des Kubernetes-Clusters besteht darin, eine Gruppe von Ressourcen, Knoten, Netzen und Speichereinheiten zu definieren, die die Hochverfügbarkeit von Apps sicherstellen. Bevor Sie eine App bereitstellen können, müssen Sie zunächst einen Cluster erstellen und die Definitionen für die Workerknoten in diesem Cluster festlegen.
{:shortdesc}

### Beispiele für `ibmcloud ks cluster-create`-CLI-Befehle
{: #clusters_cli_samples}

Haben Sie zuvor einen Cluster erstellt und suchen Sie nur schnell nach Beispielbefehlen? Versuchen Sie es mit diesen Beispielen.
*  **Kostenloser Cluster:**
   ```
   ibmcloud ks cluster-create --name mein_cluster
   ```
   {: pre}
*  **Standardcluster, gemeinsam genutzte virtuelle Maschine:**
   ```
   ibmcloud ks cluster-create --name my_cluster --zone dal10 --machine-type b3c.4x16 --hardware shared --workers 3 --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID>
   ```
   {: pre}
*  **Standardcluster, Bare-Metal-Maschine**:
   ```
   ibmcloud ks cluster-create --name mein_cluster --zone dal10 --machine-type mb2c.4x32 --hardware dedicated --workers 3 --public-vlan <öffentliche_vlan-id> --private-vlan <private_vlan-id>
   ```
   {: pre}
*  **Standardcluster, virtuelle Maschine mit [öffentlichem und privatem Serviceendpunkt](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started) in VRF-aktivierten Konten:**
   ```
   ibmcloud ks cluster-create --name my_cluster --zone dal10 --machine-type b3c.4x16 --hardware shared --workers 3 --public-service-endpoint --private-service-endpoint --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID>
   ```
   {: pre}
*   **Standardcluster für ausschließlich privates VLAN und ausschließlich private Serviceendpunkte**. Weitere Informationen zur Konfiguration eines privaten Clusternetzes finden Sie unter [Clusternetz nur mit einem privaten VLAN einrichten](/docs/containers?topic=containers-cs_network_cluster#setup_private_vlan).
    ```
    ibmcloud ks cluster-create --name my_cluster --zone dal10 --machine-type b3c.4x16 --hardware shared --workers 3 --private-service-endpoint --private-vlan <private_VLAN_ID> --private-only
    ```
    {: pre}

### Schritte zum Erstellen eines Clusters in der CLI
{: #clusters_cli_steps}

Installieren Sie zunächst die {{site.data.keyword.Bluemix_notm}}-CLI und das [{{site.data.keyword.containerlong_notm}}-Plug-in](/docs/containers?topic=containers-cs_cli_install#cs_cli_install).

Gehen Sie wie folgt vor, um einen Cluster zu erstellen:

1. [Bereiten Sie sich darauf vor, einen Cluster zu erstellen](#cluster_prepare), damit Sie über das richtige {{site.data.keyword.Bluemix_notm}}-Konto-Setup und die korrekten Benutzerberechtigungen verfügen, und treffen Sie eine Entscheidung bezüglich des Cluster-Setups und der Ressourcengruppe, die Sie verwenden möchten.

2.  Melden Sie sich an der {{site.data.keyword.Bluemix_notm}}-CLI an.

    1.  Melden Sie sich an und geben Sie Ihre {{site.data.keyword.Bluemix_notm}}-Berechtigungsnachweise ein, wenn Sie dazu aufgefordert werden.

        ```
        ibmcloud login
        ```
        {: pre}

        Falls Sie über eine eingebundene ID verfügen, geben Sie `ibmcloud login --sso` ein, um sich an der Befehlszeilenschnittstelle von {{site.data.keyword.Bluemix_notm}} anzumelden. Geben Sie Ihren Benutzernamen ein und verwenden Sie die bereitgestellte URL in Ihrer CLI-Ausgabe, um Ihren einmaligen Kenncode abzurufen. Bei Verwendung einer eingebundenen ID schlägt die Anmeldung ohne die Option `--sso` fehl, mit der Option `--sso` ist sie erfolgreich.
        {: tip}

    2. Wenn Sie über mehrere {{site.data.keyword.Bluemix_notm}}-Konten verfügen, dann wählen Sie das Konto aus, unter dem Sie Ihren Kubernetes-Cluster erstellen wollen.

    3.  Wenn Sie Cluster in einer Ressourcengruppe erstellen möchten, die nicht mit der Standardressourcengruppe identisch ist, geben Sie die entsprechende Ressourcengruppe als Ziel an.
      **Hinweis**:
        * Ein Cluster kann nur in einer Ressourcengruppe erstellt werden; nach der Erstellung des Clusters kann seine Ressourcengruppe nicht geändert werden.
        * Sie müssen mindestens über die [Rolle **Anzeigeberechtigter**](/docs/containers?topic=containers-users#platform) für die Ressourcengruppe verfügen.
        * In der Standardressourcengruppe werden automatisch freie Cluster erstellt.
      ```
      ibmcloud target -g <ressourcengruppenname>
      ```
      {: pre}

    4. **Kostenlose Cluster:**: Wenn Sie einen kostenlosen Cluster in einer bestimmten Region erstellen möchten, müssen Sie diese Region als Ziel festlegen, indem Sie `ibmcloud ks region-set` ausführen.

4.  Erstellen Sie einen Cluster. Standardcluster können in jeder Region und jeder verfügbaren Zone erstellt werden. Kostenlose Cluster können in der Region erstellt werden, die Sie mit dem Befehl `ibmcloud ks region-set` als Ziel festgelegt haben; die Zone können Sie jedoch nicht auswählen.

    1.  **Standardcluster**: Überprüfen Sie, welche Zonen verfügbar sind. Welche Zonen angezeigt werden, hängt von der {{site.data.keyword.containerlong_notm}}-Region ab, bei der Sie angemeldet sind. Damit sich Ihr Cluster über Zonen erstreckt, müssen Sie den Cluster in einer [mehrzonenfähigen Zone](/docs/containers?topic=containers-regions-and-zones#zones) erstellen.
        ```
        ibmcloud ks zones
        ```
        {: pre}
        Wenn Sie eine Zone auswählen, die sich außerhalb Ihres Landes befindet, müssen Sie daran denken, dass Sie möglicherweise eine Berechtigung benötigen, bevor Daten physisch in einem fremden Land gespeichert werden können.
        {: Hinweis}

    2.  **Standardcluster**: Wählen Sie eine Zone aus und prüfen Sie, welche Maschinentypen in dieser Zone verfügbar sind. Der Maschinentyp gibt an, welche virtuellen oder physischen Rechenhosts jedem Workerknoten zur Verfügung stehen.

        -  Zeigen Sie das Feld **Servertyp** an, um zwischen virtuellen oder physischen Maschinen (Bare-Metal-Maschinen) auszuwählen.
        -  **Virtuell**: Die Abrechnung erfolgt stündlich. Virtuelle Maschinen werden auf gemeinsam genutzter oder dedizierter Hardware bereitgestellt.
        -  **Physische Maschinen**: Bare-Metal-Server werden monatlich abgerechnet und nach Bestellung manuell von der IBM Cloud-Infrastruktur (SoftLayer) bereitgestellt, daher kann die Ausführung mehr als einen Arbeitstag dauern. Bare-Metal-Server eignen sich am besten für Hochleistungsanwendungen, die mehr Ressourcen und Hoststeuerung erfordern.
        - **Physische Maschinen mit Trusted Compute**: Sie können auch auswählen, dass [Trusted Compute](/docs/containers?topic=containers-security#trusted_compute) aktiviert wird, um Ihre Bare-Metal-Workerknoten auf Manipulation zu überprüfen. Trusted Compute ist für ausgewählte Bare-Metal-Maschinentypen verfügbar. Beispielsweise unterstützen die GPU-Typen `mgXc` Trusted Compute nicht. Wenn Sie Trusted Compute während der Clustererstellung nicht aktivieren, dies jedoch später nachholen möchten, können Sie den [Befehl](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_feature_enable) `ibmcloud ks feature-enable` verwenden. Nachdem Sie Trusted Compute aktiviert haben, können Sie es später nicht mehr inaktivieren.
        -  **Maschinentypen**: Um zu entscheiden, welcher Maschinentyp implementiert werden soll, überprüfen Sie die Kombination aus Kern, Hauptspeicher und Speicher der [verfügbaren Hardware für Workerknoten](/docs/containers?topic=containers-plan_clusters#shared_dedicated_node). Nachdem Sie den Cluster erstellt haben, können Sie verschiedene physische oder virtuelle Maschinentypen hinzufügen, indem Sie [einen Worker-Pool hinzufügen](#add_pool).

           Stellen Sie sicher, dass Sie eine Bare-Metal-Maschine bereitstellen wollen. Da die Abrechnung monatlich erfolgt, wird Ihnen ein voller Monat auch dann berechnet, wenn Sie versehentlich bestellen und sofort danach Ihre Bestellung wieder stornieren.
           {:tip}

        ```
        ibmcloud ks machine-types <zone>
        ```
        {: pre}

    3.  **Standardcluster**: Prüfen Sie, ob in der IBM Cloud-Infrastruktur (SoftLayer) bereits ein öffentliches und ein privates VLAN für dieses Konto vorhanden ist.

        ```
        ibmcloud ks vlans --zone <zone>
        ```
        {: pre}

        ```
        ID        Name                Number   Type      Router  
        1519999   vlan   1355     private   bcr02a.dal10  
        1519898   vlan   1357     private   bcr02a.dal10 
        1518787   vlan   1252     public   fcr02a.dal10 
        1518888   vlan   1254     public    fcr02a.dal10
        ```
        {: screen}

        Falls bereits ein öffentliches oder privates VLAN vorhanden ist, notieren Sie sich die passenden Router. Private VLAN-Router beginnen immer mit <code>bcr</code> (Back-End-Router) und öffentliche VLAN-Router immer mit <code>fcr</code> (Front-End-Router). Wenn Sie einen Cluster erstellen und die öffentlichen und privaten VLANs angeben, müssen die Zahlen- und Buchstabenkombinationen nach diesen Präfixen übereinstimmen. In der Beispielausgabe können alle privaten VLANs mit allen öffentlichen VLANs verwendet werden, weil alle Router `02a.dal10` enthalten.

        Sie müssen Ihre Workerknoten mit einem privaten VLAN verbinden und können sie optional auch mit einem öffentliches VLAN verbinden. **Hinweis:** Wenn Workerknoten nur mit einem privaten VLAN eingerichtet werden, müssen Sie die Kommunikation zwischen Workerknoten und Cluster-Master zulassen, indem Sie den [privaten Serviceendpunkt aktivieren](/docs/containers?topic=containers-cs_network_ov#cs_network_ov_master_private) oder eine [Gateway-Einheit konfigurieren](/docs/containers?topic=containers-cs_network_ov#cs_network_ov_master_gateway).

    4.  **Kostenlose und Standardcluster**: Führen Sie den Befehl `cluster-create` aus. Sie können einen kostenlosen Cluster wählen, der einen Workerknoten mit zwei virtuellen CPUs (vCPUs) und 4 GB Hauptspeicher umfasst und automatisch nach 30 Tagen gelöscht wird. Wenn Sie einen Standardcluster erstellen, werden die Platten der Workerknoten standardmäßig mit 256-Bit-AES verschlüsselt, die zugehörige Hardware wird von mehreren IBM Kunden gemeinsam genutzt und es wird nach Nutzungsstunden abgerechnet. </br>Beispiel für einen Standardcluster. Geben Sie die Optionen für den Cluster an:

        ```
        ibmcloud ks cluster-create --zone dal10 --machine-type b3c.4x16 --hardware <shared_or_dedicated> --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID> --workers 3 --name <cluster_name> --kube-version <major.minor.patch> [--private-service-endpoint][--public-service-endpoint] [--disable-disk-encrypt][--trusted]
        ```
        {: pre}

        Beispiel für einen kostenlosen Cluster. Geben Sie den Clusternamen an:

        ```
        ibmcloud ks cluster-create --name mein_cluster
        ```
        {: pre}

        <table>
        <caption>Komponenten von 'cluster-create'</caption>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der Bestandteile dieses Befehls</th>
        </thead>
        <tbody>
        <tr>
        <td><code>cluster-create</code></td>
        <td>Der Befehl zum Erstellen eines Clusters in Ihrer {{site.data.keyword.Bluemix_notm}}-Organisation.</td>
        </tr>
        <tr>
        <td><code>--zone <em>&lt;zone&gt;</em></code></td>
        <td>**Standardcluster**: Ersetzen Sie <em>&lt;zone&gt;</em> durch die {{site.data.keyword.Bluemix_notm}}-Zonen-ID, für die Sie den Cluster erstellen möchten. Verfügbare Zonen hängen von der {{site.data.keyword.containerlong_notm}}-Region ab, bei der Sie angemeldet sind.<p class="note">Die Cluster-Workerknoten werden in dieser Zone bereitgestellt. Damit sich Ihr Cluster über Zonen erstreckt, müssen Sie den Cluster in einer [mehrzonenfähigen Zone](/docs/containers?topic=containers-regions-and-zones#zones) erstellen. Nachdem der Cluster erstellt wurde, können Sie [eine Zone zum Cluster hinzufügen](#add_zone).</p></td>
        </tr>
        <tr>
        <td><code>--machine-type <em>&lt;maschinentyp&gt;</em></code></td>
        <td>**Standardcluster**: Wählen Sie einen Maschinentyp aus. Sie können Ihre Workerknoten als virtuelle Maschinen auf gemeinsam genutzter oder dedizierter Hardware oder als physische Maschinen auf Bare-Metal-Systemen bereitstellen. Die Typen der verfügbaren physischen und virtuellen Maschinen variieren je nach der Zone, in der Sie den Cluster implementieren. Weitere Informationen finden Sie in der Dokumentation zum [Befehl](/docs/containers?topic=containers-cs_cli_reference#cs_machine_types) `ibmcloud ks machine-type`. Bei kostenlosen Clustern muss kein Maschinentyp definiert werden.</td>
        </tr>
        <tr>
        <td><code>--hardware <em>&lt;shared_oder_dedicated&gt;</em></code></td>
        <td>**Standardcluster**: : Die Stufe der Hardwareisolation für Ihren Workerknoten. Verwenden Sie 'dedicated', wenn Sie verfügbare physische Ressourcen haben möchten, die nur Sie nutzen können, oder 'shared', um zuzulassen, dass physische Ressourcen mit anderen IBM Kunden gemeinsam genutzt werden können. Die Standardeinstellung ist 'shared'. Dieser Wert ist für VM-Standardcluster optional und steht für kostenlose Cluster nicht zur Verfügung. Geben Sie für Bare-Metal-Maschinentypen `dedicated` an.</td>
        </tr>
        <tr>
        <td><code>--public-vlan <em>&lt;öffentliche_vlan-id&gt;</em></code></td>
        <td><ul>
          <li>**Kostenlose Cluster**: Sie müssen kein öffentliches VLAN definieren. Ihr kostenloser Cluster wird automatisch mit einem öffentlichen VLAN von IBM verbunden.</li>
          <li>**Standardcluster**: Wenn für diese Zone bereits ein öffentliches VLAN in Ihrem IBM Cloud-Infrastrukturkonto (SoftLayer) eingerichtet ist, geben Sie die ID des öffentlichen VLAN ein. Wenn Sie Ihre Workerknoten nur mit einem private VLAN verbinden möchten, geben Sie diese Option nicht an.
          <p>Private VLAN-Router beginnen immer mit <code>bcr</code> (Back-End-Router) und öffentliche VLAN-Router immer mit <code>fcr</code> (Front-End-Router). Wenn Sie einen Cluster erstellen und die öffentlichen und privaten VLANs angeben, müssen die Zahlen- und Buchstabenkombinationen nach diesen Präfixen übereinstimmen.</p>
          <p class="note">Wenn Workerknoten nur mit einem privaten VLAN eingerichtet werden, müssen Sie die Kommunikation zwischen Workerknoten und Cluster-Master zulassen, indem Sie den [privaten Serviceendpunkt aktivieren](/docs/containers?topic=containers-cs_network_ov#cs_network_ov_master_private) oder eine [Gateway-Einheit konfigurieren](/docs/containers?topic=containers-cs_network_ov#cs_network_ov_master_gateway).</p></li>
        </ul></td>
        </tr>
        <tr>
        <td><code>--private-vlan <em>&lt;private_vlan-id&gt;</em></code></td>
        <td><ul><li>**Kostenlose Cluster**: Sie müssen kein privates VLAN definieren. Ihr kostenloser Cluster wird automatisch mit einem privaten VLAN von IBM verbunden.</li><li>**Standardcluster**: Wenn für diese Zone bereits ein privates VLAN in Ihrem IBM Cloud-Infrastrukturkonto (SoftLayer) eingerichtet ist, geben Sie die ID des privaten VLAN ein. Wenn Sie noch nicht über ein privates VLAN für dieses Konto verfügen, geben Sie diese Option nicht an. {{site.data.keyword.containerlong_notm}} erstellt automatisch ein privates VLAN für Sie.<p>Private VLAN-Router beginnen immer mit <code>bcr</code> (Back-End-Router) und öffentliche VLAN-Router immer mit <code>fcr</code> (Front-End-Router). Wenn Sie einen Cluster erstellen und die öffentlichen und privaten VLANs angeben, müssen die Zahlen- und Buchstabenkombinationen nach diesen Präfixen übereinstimmen.</p></li>
        <li>Zum Erstellen eines [Clusters nur mit privaten VLANs](/docs/containers?topic=containers-cs_network_cluster#setup_private_vlan) schließen Sie das Flag `--private-vlan` und das Flag `--private-only` ein, womit Sie Ihre Auswahl bestätigen. Die Flags `--public-vlan` und `--public-service-endpoint` dürfen **nicht** eingeschlossen werden. Beachten Sie, dass Sie zum Aktivieren der Verbindung zwischen Ihren Master- und Workerknoten entweder das Flag `--private-service-endpoint` einschließen oder eine eigene Gateway-Appliance konfigurieren müssen.</li></ul></td>
        </tr>
        <tr>
        <td><code>--name <em>&lt;name&gt;</em></code></td>
        <td>**Kostenlose und Standardcluster**: Ersetzen Sie <em>&lt;name&gt;</em> durch den Namen Ihres Clusters. Der Name muss mit einem Buchstaben beginnen, darf Buchstaben, Ziffern und den Bindestrich (-) enthalten und darf maximal 35 Zeichen lang sein. Der vollständig qualifizierte Domänenname für die Ingress-Unterdomäne setzt sich aus dem Clusternamen und der Region zusammen, in der der Cluster bereitgestellt wird. Um sicherzustellen, dass die Ingress-Unterdomäne innerhalb einer Region eindeutig ist, wird der Clustername möglicherweise abgeschnitten und es wird ein beliebiger Wert innerhalb des Ingress-Domänennamens angehängt.
</td>
        </tr>
        <tr>
        <td><code>--workers <em>&lt;anzahl&gt;</em></code></td>
        <td>**Standardcluster**: Die Anzahl der Workerknoten, die im Cluster eingebunden werden sollen. Wird die Option <code>--workers</code> nicht angegeben, wird 1 Workerknoten erstellt.</td>
        </tr>
        <tr>
          <td><code>--kube-version <em>&lt;major.minor.patch&gt;</em></code></td>
          <td>**Standardcluster**: Die Kubernetes-Version für den Cluster-Masterknoten. Dieser Wert ist optional. Wenn die Version nicht angegeben ist, wird der Cluster mit dem Standard für unterstützte Kubernetes-Versionen erstellt. Führen Sie den Befehl <code>ibmcloud ks kube-versions</code> aus, um die verfügbaren Versionen anzuzeigen.
</td>
        </tr>
        <tr>
        <td><code>--private-service-endpoint</code></td>
        <td>**Standardcluster in [VRF-aktivierten Konten](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started)**: Aktivieren Sie den [privaten Serviceendpunkt](/docs/containers?topic=containers-cs_network_ov#cs_network_ov_master_private), sodass Ihr Kubernetes-Master und die Workerknoten über das private VLAN kommunizieren können. Darüber hinaus können Sie den öffentlichen Serviceendpunkt mit dem Flag `--public-service-endpoint` aktivieren, um auf Ihren Cluster über das Internet zugreifen zu können. Wenn Sie nur den privaten Serviceendpunkt aktivieren, müssen Sie mit dem privaten VLAN verbunden sein, um mit Ihrem Kubernetes-Master zu kommunizieren. Nach dem Aktivieren eines privaten Serviceendpunkts können Sie diesen später nicht mehr inaktivieren.<br><br>Nach dem Erstellen des Clusters können Sie den Endpunkt mit dem Befehl `ibmcloud ks cluster-get <clustername_oder_-id>` abrufen.</td>
        </tr>
        <tr>
        <td><code>--public-service-endpoint</code></td>
        <td>**Standardcluster**: Aktivieren Sie den [öffentlichen Serviceendpunkt](/docs/containers?topic=containers-cs_network_ov#cs_network_ov_master_public), sodass auf Ihren Kubernetes-Master über das öffentliche Netz zugegriffen werden kann, zum Beispiel um `kubectl`-Befehle über Ihr Terminal auszuführen. Wenn Sie außerdem das Flag `--private-service-endpoint` angeben, erfolgt die [Kommunikation zwischen Master- und Workerknoten](/docs/containers?topic=containers-cs_network_ov#cs_network_ov_master_both) über das private Netz in VRF-aktivierten Konten. Sie können den öffentlichen Serviceendpunkt später inaktivieren, wenn Sie einen rein privaten Cluster wünschen.<br><br>Nach dem Erstellen des Clusters können Sie den Endpunkt mit dem Befehl `ibmcloud ks cluster-get <clustername_oder_-id>` abrufen.</td>
        </tr>
        <tr>
        <td><code>--disable-disk-encrypt</code></td>
        <td>**Kostenloser und Standardcluster**: Workerknoten verfügen standardmäßig über eine [Plattenverschlüsselung](/docs/containers?topic=containers-security#encrypted_disk) (256-Bit-AES). Wenn Sie die Verschlüsselung inaktivieren möchten, schließen Sie diese Option ein.</td>
        </tr>
        <tr>
        <td><code>--trusted</code></td>
        <td>**Standard-Bare-Metal-Cluster**: Aktivieren Sie [Trusted Compute](/docs/containers?topic=containers-security#trusted_compute), um Ihre Bare-Metal-Workerknoten auf Manipulation zu überprüfen. Trusted Compute ist für ausgewählte Bare-Metal-Maschinentypen verfügbar. Beispielsweise unterstützen die GPU-Typen `mgXc` Trusted Compute nicht. Wenn Sie Trusted Compute während der Clustererstellung nicht aktivieren, dies jedoch später nachholen möchten, können Sie den [Befehl](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_feature_enable) `ibmcloud ks feature-enable` verwenden. Nachdem Sie Trusted Compute aktiviert haben, können Sie es später nicht mehr inaktivieren.</td>
        </tr>
        </tbody></table>

5.  Prüfen Sie, ob die Erstellung des Clusters angefordert wurde. Bei virtuellen Maschinen kann es einige Minuten dauern, bis die Workerknotenmaschinen angewiesen werden und der Cluster in Ihrem Konto eingerichtet und bereitgestellt wird. Physische Bare-Metal-Maschinen werden durch einen manuelle Interaktion mit der IBM Cloud-Infrastruktur (SoftLayer) bereitgestellt, daher kann die Ausführung mehr als einen Arbeitstag dauern.

    ```
    ibmcloud ks clusters
    ```
    {: pre}

    Nach Abschluss der Bereitstellung Ihres Clusters wird der Status des Clusters in **deployed** (bereitgestellt) geändert.

    ```
    Name         ID                                   State      Created          Workers   Zone       Version     Resource Group Name
    my_cluster   paf97e8843e29941b49c598f516de72101   deployed   20170201162433   1         mil01      1.12.7      Default
    ```
    {: screen}

6.  Überprüfen Sie den Status der Workerknoten.

    ```
    ibmcloud ks workers --cluster <clustername_oder_-id>
    ```
    {: pre}

    Wenn die Workerknoten bereit sind, wechselt der Zustand (State) zu **Normal**, während für den Status die Angabe **Bereit** angezeigt wird. Wenn der Knotenstatus **Ready** (bereit) lautet, können Sie auf den Cluster zugreifen. Beachten Sie, dass selbst bei Bereitschaft des Clusters einige seiner Teile, die von anderen Services wie den geheimen Ingress-Schlüsseln oder geheimen Schlüsseln für Registry-Image-Pull-Operationen verwendet werden, möglicherweise noch in Verarbeitung sind.

    Jedem Workerknoten werden eine eindeutige Workerknoten-ID und ein Domänenname zugewiesen, die nach dem Erstellen des Clusters nicht manuell geändert werden dürfen. Wenn die ID oder der Domänenname geändert wird, kann der Kubernetes-Master Ihren Cluster nicht mehr verwalten.
    {: important}

    ```
    ID                                                 Public IP       Private IP      Machine Type   State    Status   Zone        Version     Resource Group Name
    kube-mil01-paf97e8843e29941b49c598f516de72101-w1   169.xx.xxx.xxx  10.xxx.xx.xxx   free           normal   Ready    mil01       1.12.7      Default
    ```
    {: screen}

7.  Legen Sie den von Ihnen erstellten Cluster als Kontext für diese Sitzung fest. Führen Sie diese Konfigurationsschritte jedes Mal aus, wenn Sie mit Ihrem Cluster arbeiten.
    1.  Ermitteln Sie den Befehl zum Festlegen der Umgebungsvariablen und laden Sie die Kubernetes-Konfigurationsdateien herunter.

        ```
        ibmcloud ks cluster-config --cluster <clustername_oder_-id>
        ```
        {: pre}

        Wenn der Download der Konfigurationsdateien abgeschlossen ist, wird ein Befehl angezeigt, den Sie verwenden können, um den Pfad zu der lokalen Kubernetes-Konfigurationsdatei als Umgebungsvariable festzulegen.

        Beispiel für OS X:

        ```
        export KUBECONFIG=/Users/<benutzername>/.bluemix/plugins/container-service/clusters/mycluster/kube-config-prod-dal10-mycluster.yml
        ```
        {: screen}

    2.  Kopieren Sie den Befehl, der in Ihrem Terminal angezeigt wird, um die Umgebungsvariable `KUBECONFIG` festzulegen.
    3.  Stellen Sie sicher, dass die Umgebungsvariable `KUBECONFIG` richtig eingestellt ist.

        Beispiel für OS X:

        ```
        echo $KUBECONFIG
        ```
        {: pre}

        Ausgabe:

        ```
        /Users/<benutzername>/.bluemix/plugins/container-service/clusters/mycluster/kube-config-prod-dal10-mycluster.yml

        ```
        {: screen}

8.  Starten Sie Ihr Kubernetes-Dashboard über den Standardport `8001`.
    1.  Legen Sie die Standardportnummer für den Proxy fest.

        ```
        kubectl proxy
        ```
        {: pre}

        ```
        Starting to serve on 127.0.0.1:8001
        ```
        {: screen}

    2.  Öffnen Sie die folgende URL in einem Web-Browser, um das Kubernetes-Dashboard anzuzeigen:

        ```
        http://localhost:8001/ui
        ```
        {: codeblock}


**Womit möchten Sie fortfahren?**

-   Wenn Sie den Cluster in einer mehrzonenfähigen Zone erstellt haben, verteilen Sie Workerknoten durch [Hinzufügen einer Zone zum Cluster](#add_zone).
-   [Stellen Sie eine App in Ihrem Cluster bereit.](/docs/containers?topic=containers-app#app_cli)
-   [Verwalten Sie Ihren Cluster über die Befehlszeile `kubectl`. ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubectl.docs.kubernetes.io/)
-   [Richten Sie Ihre eigene private Registry in {{site.data.keyword.Bluemix_notm}} ein, um Docker-Images zu speichern und gemeinsam mit anderen Benutzern zu verwenden.](/docs/services/Registry?topic=registry-getting-started)
- Wenn Sie über eine Firewall verfügen, müssen Sie unter Umständen [die erforderlichen Ports öffnen](/docs/containers?topic=containers-firewall#firewall), um `ibmcloud`-, `kubectl`- oder `calicotl`-Befehle zu verwenden, um abgehenden Datenverkehr von Ihrem Cluster zu ermöglichen oder um eingehenden Datenverkehr für Netzservices zuzulassen.
-   [Richten Sie den Cluster-Autoscaler ein](/docs/containers?topic=containers-ca#ca), um Workerknoten in Ihren Worker-Pools automatisch entsprechend den Workloadressourcenanforderungen hinzuzufügen oder zu entfernen.
-  Sie können steuern, wer in Ihrem Cluster Pods mit [Pod-Sicherheitsrichtlinien](/docs/containers?topic=containers-psp) erstellen kann.

<br />



## Workerknoten und Zonen zu Clustern hinzufügen
{: #add_workers}

Wenn Sie die Verfügbarkeit Ihrer Apps erhöhen möchten, können Sie Workerknoten zu einer vorhandenen Zone oder mehreren vorhandenen Zonen in Ihrem Cluster hinzufügen. Um Ihre Apps vor Zonenausfällen zu schützen, können Sie Zonen zu Ihrem Cluster hinzufügen.
{:shortdesc}

Wenn Sie einen Cluster erstellen, werden die Workerknoten in einem Worker-Pool bereitgestellt. Nach der Clustererstellung können Sie weitere Workerknoten zu einem Pool hinzufügen, indem Sie die Größe ändern oder weitere Worker-Pools hinzufügen. Der Worker-Pool ist standardmäßig in einer Zone vorhanden. Cluster, die über einen Worker-Pool in nur einer Zone verfügen, werden als Einzelzonencluster bezeichnet. Wenn Sie dem Cluster weitere Zonen hinzufügen, ist der Worker-Pool zonenübergreifend vorhanden. Cluster, die über einen Worker-Pool verfügen, der über mehrere Zonen verteilt ist, werden als Mehrzonencluster bezeichnet.

Wenn Sie über einen Mehrzonencluster verfügen, müssen seine Workerknotenressourcen ausgeglichen sein. Stellen Sie sicher, dass alle Worker-Pools über dieselben Zonen verteilt sind und fügen Sie Worker hinzu oder entfernen Sie sie, indem Sie die Größe der Pools ändern, anstatt einzelne Knoten hinzuzufügen.
{: tip}

Bevor Sie beginnen, stellen Sie sicher, dass Sie die [{{site.data.keyword.Bluemix_notm}} IAM-Plattformrolle **Operator** oder **Administrator**](/docs/containers?topic=containers-users#platform) innehaben. Wählen Sie dann einen der folgenden Abschnitte aus:
  * [Workerknoten durch Ändern der Größe eines vorhandenen Worker-Pools in Ihrem Cluster hinzufügen](#resize_pool)
  * [Workerknoten durch Hinzufügen eines Worker-Pools zu Ihrem Cluster hinzufügen](#add_pool)
  * [Eine Zone zu Ihrem Cluster hinzufügen und die Workerknoten in den Worker-Pools über mehrere Zonen replizieren](#add_zone)
  * [Einen eigenständigen Workerknoten zu einem Cluster hinzufügen (veraltet)](#standalone)

Nach der Einrichtung Ihres Worker-Pools können Sie den [Cluster-Autoscaler einrichten](/docs/containers?topic=containers-ca#ca), um Workerknoten in Ihren Worker-Pools automatisch entsprechend den Workloadressourcenanforderungen hinzuzufügen oder zu entfernen.
{:tip}

### Workerknoten durch Ändern der Größe eines vorhandenen Worker-Pools hinzufügen
{: #resize_pool}

Sie können die Anzahl der Workerknoten in Ihrem Cluster erhöhen oder verringern, indem Sie die Größe eines vorhandenen Worker-Pools ändern, unabhängig davon, ob sich der Worker-Pool in einer Zone befindet oder über mehrere Zonen verteilt ist.
{: shortdesc}

Angenommen, Sie verfügen über einen Cluster mit einem Worker-Pool, der drei Workerknoten pro Zone aufweist.
* Wenn es sich bei dem Cluster um einen Einzelzonencluster handelt und er sich in `dal10` befindet, dann verfügt der Worker-Pool über drei Workerknoten in `dal10`. Der Cluster verfügt über insgesamt drei Workerknoten.
* Wenn es sich bei dem Cluster um einen Mehrzonencluster handelt und er sich in `dal10` und `dal12` befindet, dann verfügt der Worker-Pool über drei Workerknoten in `dal10` und drei Workerknoten in `dal12`. Der Cluster verfügt über insgesamt sechs Workerknoten.

Bei Bare-Metal-Worker-Pools sollten Sie beachten, dass die Rechnungsstellung monatlich erfolgt. Je nachdem, wie Sie die Größe eines Pools verändern, wirkt sich dies auf die Kosten für den Monat aus.
{: tip}

Ändern Sie die Anzahl der Workerknoten, die der Worker-Pool in jeder Zone bereitstellt, um die Größe des Worker-Pools zu ändern:

1. Rufen Sie den Namen des Worker-Pools ab, dessen Größe Sie ändern möchten.
    ```
    ibmcloud ks worker-pools --cluster <clustername_oder_-id>
    ```
    {: pre}

2. Ändern Sie die Größe des Worker-Pools, indem Sie die Anzahl der Workerknoten angeben, die Sie in jeder Zone bereitstellen möchten. Der Mindestwert ist '1'.
    ```
    ibmcloud ks worker-pool-resize --cluster <clustername_oder_-id> --worker-pool <poolname>  --size-per-zone <anzahl_der_worker_pro_zone>
    ```
    {: pre}

3. Stellen Sie sicher, dass die Größe des Worker-Pools geändert wurde.
    ```
    ibmcloud ks workers --cluster <clustername_oder_-id> --worker-pool <poolname>
    ```
    {: pre}

    Beispielausgabe für einen Worker-Pool, der sich in zwei Zonen (`dal10` und `dal12`) befindet und dessen Größe auf zwei Workerknoten pro Zone geändert wird:
    ```
    ID                                                 Public IP        Private IP      Machine Type      State    Status  Zone    Version
    kube-dal10-crb20b637238ea471f8d4a8b881aae4962-w7   169.xx.xxx.xxx   10.xxx.xx.xxx   b3c.4x16          normal   Ready   dal10   1.8.6_1504
    kube-dal10-crb20b637238ea471f8d4a8b881aae4962-w8   169.xx.xxx.xxx   10.xxx.xx.xxx   b3c.4x16          normal   Ready   dal10   1.8.6_1504
    kube-dal12-crb20b637238ea471f8d4a8b881aae4962-w9   169.xx.xxx.xxx   10.xxx.xx.xxx   b3c.4x16          normal   Ready   dal12   1.8.6_1504
    kube-dal12-crb20b637238ea471f8d4a8b881aae4962-w10  169.xx.xxx.xxx   10.xxx.xx.xxx   b3c.4x16          normal   Ready   dal12   1.8.6_1504
    ```
    {: screen}

### Workerknoten durch Erstellen eines neuen Worker-Pools hinzufügen
{: #add_pool}

Sie können Workerknoten zu Ihrem Cluster hinzufügen, indem Sie einen neuen Worker-Pool erstellen.
{:shortdesc}

1. Rufen Sie die **Workerzonen** Ihres Clusters ab und wählen Sie die Zone aus, in der Sie die Workerknoten in Ihrem Workerpool bereitstellen möchten. Wenn Sie über ein Einzelzonencluster verfügen, müssen Sie die Zone verwenden, die im Feld **Workerzonen** angegeben ist. Bei Mehrzonenclustern können Sie eine der vorhandenen **Workerzonen** Ihres Clusters auswählen oder einen der [Standorte in einer Mehrzonen-Metropole](/docs/containers?topic=containers-regions-and-zones#zones) für die Region hinzufügen, in der sich Ihr Cluster befindet. Verfügbare Zonen können Sie durch Ausführen von `ibmcloud ks zones` auflisten.
   ```
   ibmcloud ks cluster-get --cluster <clustername_oder_-id>
   ```
   {: pre}

   Beispielausgabe:
   ```
   ...
   Worker Zones: dal10, dal12, dal13
   ```
   {: screen}

2. Listen Sie für jede Zone private und öffentliche VLANs auf. Notieren Sie sich das private und das öffentliche VLAN, das Sie verwenden möchten. Wenn Sie nicht über ein privates oder ein öffentliches VLAN verfügen, wird das VLAN automatisch für Sie erstellt, wenn Sie eine Zone zu Ihrem Worker-Pool hinzufügen.
   ```
   ibmcloud ks vlans --zone <zone>
   ```
   {: pre}

3.  Überprüfen Sie für jede Zone die [verfügbaren Maschinentypen für die Workerknoten](/docs/containers?topic=containers-plan_clusters#shared_dedicated_node).

    ```
    ibmcloud ks machine-types <zone>
    ```
    {: pre}

4. Erstellen Sie einen Worker-Pool. Falls Sie einen Bare-Metal-Worker-Pool bereitstellen, geben Sie `--hardware dedicated` an.
   ```
   ibmcloud ks worker-pool-create --name <poolname> --cluster <clustername_oder_-id> --machine-type <maschinentyp> --size-per-zone <anzahl_der_worker_pro_zone> --hardware <dediziert_oder_gemeinsam_genutzt>
   ```
   {: pre}

5. Stellen Sie sicher, dass der Worker-Pool erstellt wird.
   ```
   ibmcloud ks worker-pools --cluster <clustername_oder_-id>
   ```
   {: pre}

6. Standardmäßig wird durch das Hinzufügen eines Worker-Pools ein Pool ohne Zonen erstellt. Um Workerknoten in einer Zone bereitzustellen, müssen Sie die Zonen zum Worker-Pool hinzufügen, die Sie vorher abgerufen haben. Wenn Sie die Workerknoten auf mehrere Zonen verteilen möchten, wiederholen Sie diesen Befehl für jede einzelne Zone.  
   ```
   ibmcloud ks zone-add --zone <zone> --cluster <clustername_oder_-id> --worker-pools <poolname> --private-vlan <private_vlan-id> --public-vlan <öffentliche_vlan-id>
   ```
   {: pre}

7. Stellen Sie sicher, dass die Workerknoten in der Zone bereitgestellt werden, die Sie hinzugefügt haben. Die Workerknoten sind bereit, wenn der Status von **provision_pending** zu **normal** wechselt.
   ```
   ibmcloud ks workers --cluster <clustername_oder_-id> --worker-pool <poolname>
   ```
   {: pre}

   Beispielausgabe:
   ```
   ID                                                 Public IP        Private IP      Machine Type      State    Status  Zone    Version
   kube-dal10-crb20b637238ea471f8d4a8b881aae4962-w7   169.xx.xxx.xxx   10.xxx.xx.xxx   b3c.4x16          provision_pending   Ready   dal10   1.8.6_1504
   kube-dal10-crb20b637238ea471f8d4a8b881aae4962-w8   169.xx.xxx.xxx   10.xxx.xx.xxx   b3c.4x16          provision_pending   Ready   dal10   1.8.6_1504
   ```
   {: screen}

### Workerknoten durch Hinzufügen einer Zone zu einem Worker-Pool hinzufügen
{: #add_zone}

Sie können Ihren Cluster über mehrere Zonen in einer Region verteilen, indem Sie eine Zone zu dem vorhandenen Worker-Pool hinzufügen.
{:shortdesc}

Wenn Sie eine Zone zu einem Worker-Pool hinzufügen, werden die in Ihrem Worker-Pool definierten Workerknoten in der neuen Zone bereitgestellt und für die zukünftige Planung von Arbeitslasten berücksichtigt. {{site.data.keyword.containerlong_notm}} fügt automatisch die Bezeichnung `failure-domain.beta.kubernetes.io/region` für die Region und die Bezeichnung `failure-domain.beta.kubernetes.io/zone` für die Zone für jeden Workerknoten hinzu. Der Kubernetes-Scheduler verwendet diese Bezeichnungen, um Pods auf Zonen innerhalb derselben Region zu verteilen.

Wenn Sie über mehrere Worker-Pools in Ihrem Cluster verfügen, fügen Sie die Zone zu allen Pools hinzu, sodass die Workerknoten gleichmäßig über den Cluster hinweg verteilt sind.

Vorbemerkungen:
*  Wenn Sie eine Zone zu Ihrem Worker-Pool hinzufügen möchten, muss sich Ihr Worker-Pool in einer [mehrzonenfähigen Zone](/docs/containers?topic=containers-regions-and-zones#zones) befinden. Wenn sich der Worker-Pool nicht in einer solchen Zone befindet, sollten Sie [einen neuen Worker-Pool erstellen](#add_pool).
*  Wenn Sie über mehrere VLANs für einen Cluster, mehrere Teilnetze in demselben VLAN oder einen Cluster mit mehreren Zonen verfügen, müssen Sie eine [VRF-Funktion (Virtual Router Function)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) für Ihr Konto für die IBM Cloud-Infrastruktur (SoftLayer) aktivieren, damit die Workerknoten über das private Netz miteinander kommunizieren können. Zur Aktivierung von VRF [wenden Sie sich an Ihren Ansprechpartner für die IBM Cloud-Infrastruktur (SoftLayer)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#how-you-can-initiate-the-conversion). Wenn Sie VRF nicht aktivieren können oder wollen, aktivieren Sie das [VLAN-Spanning](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning). Um diese Aktion durchführen zu können, müssen Sie über die [Infrastrukturberechtigung](/docs/containers?topic=containers-users#infra_access) **Netz > VLAN-Spanning im Netz verwalten** verfügen oder Sie können den Kontoeigner bitten, diese zu aktivieren. Zum Prüfen, ob das VLAN-Spanning bereits aktiviert ist, verwenden Sie den [Befehl](/docs/containers?topic=containers-cs_cli_reference#cs_vlan_spanning_get) `ibmcloud ks vlan-spanning-get`.

Gehen Sie wie folgt vor, um eine Zone mit Workerknoten zum Worker-Pool hinzuzufügen:

1. Listen Sie die verfügbaren Zonen auf und wählen Sie die Zone aus, die Sie Ihrem Worker-Pool hinzufügen möchten. Die Zone, die Sie auswählen, muss eine mehrzonenfähige Zone sein.
   ```
   ibmcloud ks zones
   ```
   {: pre}

2. Listen Sie die verfügbaren VLANs in dieser Zone auf. Wenn Sie nicht über ein privates oder ein öffentliches VLAN verfügen, wird das VLAN automatisch für Sie erstellt, wenn Sie eine Zone zu Ihrem Worker-Pool hinzufügen.
   ```
   ibmcloud ks vlans --zone <zone>
   ```
   {: pre}

3. Listen Sie die Worker-Pools in Ihrem Cluster auf und notieren Sie deren Namen.
   ```
   ibmcloud ks worker-pools --cluster <clustername_oder_-id>
   ```
   {: pre}

4. Fügen Sie die Zone zu Ihrem Worker-Pool hinzu. Wenn Sie mehrere Worker-Pools haben, fügen Sie die Zone allen Worker-Pools hinzu, sodass Ihr Cluster in allen Zonen ausgeglichen ist. Ersetzen Sie `<pool1_id_or_name,pool2_id_or_name,...>` durch die Namen aller Worker-Pools in einer durch Kommas getrennten Liste.

    Es muss ein privates und ein öffentliches VLAN vorhanden sein, bevor Sie eine Zone zu mehreren Worker-Pools hinzufügen können. Wenn Sie nicht über ein privates und ein öffentliches VLAN in dieser Zone verfügen, fügen Sie zuerst die Zone zu einem Worker-Pool hinzu, sodass ein privates und ein öffentliches VLAN für Sie erstellt wird. Anschließend können Sie die Zone zu anderen Worker-Pools hinzufügen, indem Sie das private und das öffentliche VLAN angeben, das für Sie erstellt wurde.
    {: note}

   Wenn Sie verschiedene VLANs für unterschiedliche Worker-Pools verwenden möchten, wiederholen Sie diesen Befehl für jedes VLAN und die entsprechenden Worker-Pools. Alle neuen Workerknoten werden den von Ihnen angegebenen VLANs hinzugefügt, die VLANs für alle vorhandenen Workerknoten werden jedoch nicht geändert.
   {: tip}
   ```
   ibmcloud ks zone-add --zone <zone> --cluster <clustername_oder_-id> --worker-pools <pool1-name,pool2-name,...> --private-vlan <private_vlan-id> --public-vlan <öffentliche_vlan-id>
   ```
   {: pre}

5. Überprüfen Sie, ob die Zone Ihrem Cluster hinzugefügt wurde. Suchen Sie die hinzugefügte Zone im Feld für die **Workerzonen** der Ausgabe. Beachten Sie, dass die Gesamtanzahl der Worker im Feld für die **Worker** sich erhöht hat, da neue Workerknoten in der hinzugefügten Zone bereitgestellt werden.
  ```
  ibmcloud ks cluster-get --cluster <clustername_oder_-id>
  ```
  {: pre}

  Beispielausgabe:
  ```
  Name:                           mycluster
  ID:                             df253b6025d64944ab99ed63bb4567b6
  State:                          normal
  Created:                        2018-09-28T15:43:15+0000
  Location:                       dal10
  Master URL:                     https://c3.<region>.containers.cloud.ibm.com:30426
  Public Service Endpoint URL:    https://c3.<region>.containers.cloud.ibm.com:30426
  Private Service Endpoint URL:   https://c3-private.<region>.containers.cloud.ibm.com:31140
  Master Location:                Dallas
  Master Status:                  Ready (21 hours ago)
  Ingress Subdomain:              mycluster.us-south.containers.appdomain.cloud
  Ingress Secret:                 mycluster
  Workers:                        6
  Worker Zones:                   dal10, dal12
  Version:                        1.11.3_1524
  Owner:                          owner@email.com
  Monitoring Dashboard:           ...
  Resource Group ID:              a8a12accd63b437bbd6d58fb6a462ca7
  Resource Group Name:            Default
  ```
  {: screen}  

### Veraltet: Eigenständige Workerknoten hinzufügen
{: #standalone}

Wenn Sie über einen Cluster verfügen, der vor der Einführung von Worker-Pools erstellt wurde, können Sie die veralteten Befehle verwenden, um eigenständige Workerknoten hinzuzufügen.
{: deprecated}

Wenn Sie über einen Cluster verfügen, der nach der Einführung von Worker-Pools erstellt wurde, können Sie keine eigenständigen Workerknoten hinzufügen. Stattdessen können Sie [einen Worker-Pool erstellen](#add_pool), [die Größe eines vorhandenen Worker-Pools ändern](#resize_pool) oder [eine Zone zu einem Worker-Pool hinzufügen](#add_zone), um Workerknoten zu Ihrem Cluster hinzuzufügen.
{: note}

1. Listen Sie die verfügbaren Zonen auf und wählen Sie die Zone aus, in der Sie Workerknoten hinzufügen möchten.
   ```
   ibmcloud ks zones
   ```
   {: pre}

2. Listen Sie die verfügbaren VLANs in dieser Zone auf und notieren Sie ihre ID.
   ```
   ibmcloud ks vlans --zone <zone>
   ```
   {: pre}

3. Listen Sie die verfügbaren Maschinentypen in der Zone auf.
   ```
   ibmcloud ks machine-types --zone <zone>
   ```
   {: pre}

4. Fügen Sie eigenständige Workerknoten zum Cluster hinzu. Geben Sie für Bare-Metal-Maschinentypen `dedicated` an.
   ```
   ibmcloud ks worker-add --cluster <clustername_oder_-id> --number <anzahl_der_workerknoten> --public-vlan <öffentliche_vlan-id> --private-vlan <private_vlan-id> --machine-type <maschinentyp> --hardware <shared_oder_dedicated>
   ```
   {: pre}

5. Stellen Sie sicher, dass die Workerknoten erstellt werden.
   ```
   ibmcloud ks workers --cluster <clustername_oder_-id>
   ```
   {: pre}

<br />


## Clusterstatus anzeigen
{: #states}

Überprüfen Sie den Status eines Kubernetes-Clusters, um Informationen zur Verfügbarkeit und Kapazität des Clusters sowie zu möglichen Problemen, die aufgetreten sein können, zu erhalten.
{:shortdesc}

Wenn Sie Informationen zu einem bestimmten Cluster anzeigen möchten, zum Beispiel Zonen, Serviceendpunkt-URLs, Ingress-Unterdomäne, Version, Eigner und Überwachungsdashboard, verwenden Sie den [Befehl](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_get) `ibmcloud ks cluster-get <clustername_oder_-id>`. Schließen Sie das Flag `--showResources` ein, um weitere Clusterressourcen, wie zum Beispiel Add-ons für Speicherpods oder Teilnetz-VLANs für öffentliche und private IPs anzuzeigen.

Sie können den aktuellen Clusterstatus anzeigen, indem Sie den Befehl `ibmcloud ks clusters` ausführen. Der Status wird im entsprechenden **Statusfeld** angezeigt. Informationen zum Beheben von Fehlern bei Clustern und Workerknoten finden Sie im Abschnitt [Fehlerbehebung bei Clustern](/docs/containers?topic=containers-cs_troubleshoot#debug_clusters).

<table summary="Jede Tabellenzeile sollte von links nach rechts gelesen werden, wobei der Clusterstatus in der ersten Spalte und eine Beschreibung in der zweiten Spalte angegeben ist.">
<caption>Clusterstatus</caption>
   <thead>
   <th>Clusterstatus</th>
   <th>Beschreibung</th>
   </thead>
   <tbody>
<tr>
   <td>Aborted (Abgebrochen)</td>
   <td>Das Löschen des Clusters wird vom Benutzer angefordert, bevor der Kubernetes Master bereitgestellt ist. Nachdem das Löschen des Clusters abgeschlossen ist, wird der Cluster aus dem Dashboard entfernt. Wenn Ihr Cluster in diesem Status lange Zeit blockiert ist, öffnen Sie einen [{{site.data.keyword.Bluemix_notm}}-Supportfall](/docs/containers?topic=containers-cs_troubleshoot#ts_getting_help).</td>
   </tr>
 <tr>
     <td>Critical (Kritisch)</td>
     <td>Der Kubernetes-Master kann nicht erreicht werden oder alle Workerknoten in dem Cluster sind inaktiv. </td>
    </tr>
   <tr>
     <td>Delete failed (Löschen fehlgeschlagen)</td>
     <td>Der Kubernetes Masterknoten oder mindestens ein Workerknoten kann nicht gelöscht werden.  </td>
   </tr>
   <tr>
     <td>Deleted (Gelöscht)</td>
     <td>Der Cluster wird gelöscht, aber noch nicht aus dem Dashboard entfernt. Wenn Ihr Cluster in diesem Status lange Zeit blockiert ist, öffnen Sie einen [{{site.data.keyword.Bluemix_notm}}-Supportfall](/docs/containers?topic=containers-cs_troubleshoot#ts_getting_help). </td>
   </tr>
   <tr>
   <td>Deleting (Löschen)</td>
   <td>Der Cluster wird gelöscht, und die Clusterinfrastruktur wird abgebaut. Der Zugriff auf den Cluster ist nicht möglich.  </td>
   </tr>
   <tr>
     <td>Deploy failed (Bereitstellung fehlgeschlagen)</td>
     <td>Die Bereitstellung des Kubernetes-Masters konnte nicht abgeschlossen werden. Sie können diesen Status nicht auflösen. Wenden Sie sich an den Support für IBM Cloud, indem Sie einen [{{site.data.keyword.Bluemix_notm}}-Supportfall](/docs/containers?topic=containers-cs_troubleshoot#ts_getting_help) öffnen.</td>
   </tr>
     <tr>
       <td>Deploying (Wird bereitgestellt)</td>
       <td>Der Kubernetes-Master ist noch nicht vollständig implementiert. Sie können auf Ihren Cluster nicht zugreifen. Warten Sie, bis Ihr Cluster vollständig bereitgestellt wurde, und überprüfen Sie dann den Status Ihres Clusters.</td>
      </tr>
      <tr>
       <td>Normal</td>
       <td>Alle Workerknoten in einem Cluster sind betriebsbereit. Sie können auf den Cluster zugreifen und Apps auf dem Cluster bereitstellen. Dieser Status wird als einwandfreier Zustand betrachtet und erfordert keine Aktion von Ihnen.<p class="note">Auch wenn die Workerknoten ordnungsgemäß funktionieren, bedürfen andere Infrastrukturressourcen wie [Netz](/docs/containers?topic=containers-cs_troubleshoot_network) und [Speicher](/docs/containers?topic=containers-cs_troubleshoot_storage) möglicherweise Ihrer Aufmerksamkeit. Wenn Sie den Cluster gerade erstellt haben, befinden sich einige Teile des Clusters, die von anderen Services wie z. B. geheimen Ingress-Schlüsseln oder geheimen Schlüsseln für Registry-Image-Pull-Operationen verwendet werden, möglicherweise noch in Bearbeitung.</p></td>
    </tr>
      <tr>
       <td>Pending (Anstehend)</td>
       <td>Der Kubernetes-Master ist bereitgestellt. Die Workerknoten werden gerade eingerichtet und sind noch nicht im Cluster verfügbar. Sie können auf den Cluster zugreifen, aber Sie können keine Apps auf dem Cluster bereitstellen.  </td>
     </tr>
   <tr>
     <td>Requested (Angefordert)</td>
     <td>Eine Anforderung zum Erstellen des Clusters und zum Bestellen der Infrastruktur für die Kubernetes Master- und Workerknoten wird gesendet. Wenn die Bereitstellung des Clusters gestartet wird, ändert sich der Clusterstatus in <code>Deploying</code> (Wird bereitgestellt). Wenn Ihr Cluster lange Zeit im Status <code>Requested</code> (Angefordert) blockiert ist, öffnen Sie einen [{{site.data.keyword.Bluemix_notm}}-Supportfall](/docs/containers?topic=containers-cs_troubleshoot#ts_getting_help). </td>
   </tr>
   <tr>
     <td>Updating (Aktualisierung)</td>
     <td>Der Kubernetes API-Server, der in Ihrem Kubernetes-Master ausgeführt wird, wird auf eine neue Version der Kubernetes-API aktualisiert. Während der Aktualisierung können Sie weder auf den Cluster zugreifen noch Änderungen am Cluster vornehmen. Workerknoten, Apps und Ressourcen, die vom Benutzer bereitgestellt wurden, werden nicht geändert und weiterhin ausgeführt. Warten Sie, bis die Aktualisierung abgeschlossen ist, und überprüfen Sie dann den Status Ihres Clusters. </td>
   </tr>
    <tr>
       <td>Warning (Warnung)</td>
       <td>Mindestens ein Workerknoten in dem Cluster ist nicht verfügbar, aber andere Workerknoten sind verfügbar und können die Workload übernehmen. </td>
    </tr>
   </tbody>
 </table>


<br />


## Cluster entfernen
{: #remove}

Kostenlose Cluster und Standardcluster, die mit einem belastbaren Konto erstellt wurden, müssen manuell entfernt werden, wenn sie nicht mehr benötigt werden, damit sie nicht länger Ressourcen verbrauchen.
{:shortdesc}

<p class="important">
In Ihrem persistenten Speicher werden keine Sicherungen Ihres Clusters oder Ihrer Daten erstellt. Wenn Sie einen Cluster löschen, können Sie auswählen, Ihren persistenten Speicher zu löschen. Persistenter Speicher, den Sie mit einer Speicherklasse mit der Angabe `delete` bereitgestellt haben, wird in der IBM Cloud-Infrastruktur (SoftLayer) permanent gelöscht, wenn Sie auswählen, Ihren persistenten Speicher zu löschen. Wenn Sie Ihren persistenten Speicher mit einer Speicherklasse mit der Angabe `retain` bereitgestellt haben und auswählen, Ihren Speicher zu löschen, werden der Cluster, der persistente Datenträger (PV) und der PersistentVolumeClaim (PVC) gelöscht, während die persistente Speicherinstanz in Ihrem Konto für die IBM Cloud-Infrastruktur (SoftLayer) bestehen bleibt.</br>
</br>Wenn Sie einen Cluster entfernen, entfernen Sie auch alle Teilnetze, die automatisch bereitgestellt wurden, als Sie den Cluster erstellt haben, und solche, die Sie mithilfe des Befehls `ibmcloud ks cluster-subnet-create` erstellt haben. Wenn Sie jedoch vorhandene Teilnetze manuell mithilfe des Befehls `ibmcloud ks cluster-subnet-add command` zu Ihrem Cluster hinzugefügt haben, werden diese Teilnetze nicht aus Ihrem IBM Cloud-Infrastrukturkonto (SoftLayer) entfernt und können in anderen Clustern wiederverwendet werden.</p>

Vorbemerkungen:
* Notieren Sie sich Ihre Cluster-ID. Möglicherweise benötigen Sie die Cluster-ID, um zugehörige Ressourcen der IBM Cloud-Infrastruktur (SoftLayer) zu untersuchen und zu entfernen, die nicht automatisch mit Ihrem Cluster gelöscht werden.
* Wenn Sie die Daten in Ihrem persistenten Speicher löschen möchten, [sollten Sie mit den Löschoptionen vertraut sein](/docs/containers?topic=containers-cleanup#cleanup).
* Stellen Sie sicher, dass Sie die [{{site.data.keyword.Bluemix_notm}} IAM-Plattformrolle **Administrator**](/docs/containers?topic=containers-users#platform) innehaben.

Gehen Sie wie folgt vor, um einen Cluster zu entfernen:

-   Vorgehensweise bei Verwendung der {{site.data.keyword.Bluemix_notm}}-Konsole:
    1.  Wählen Sie Ihren Cluster aus und klicken Sie im Menü **Weitere Aktionen...** auf **Löschen**.

-   Vorgehensweise bei Verwendung der {{site.data.keyword.Bluemix_notm}}-CLI:
    1.  Listen Sie die verfügbaren Cluster auf.

        ```
        ibmcloud ks clusters
        ```
        {: pre}

    2.  Löschen Sie den Cluster.

        ```
        ibmcloud ks cluster-rm --cluster <clustername_oder_-id>
        ```
        {: pre}

    3.  Befolgen Sie die Eingabeaufforderungen und wählen Sie aus, ob Clusterressourcen, darunter Container, Pods, gebundene Services, persistenter Speicher und geheime Schlüssel, gelöscht werden sollen.
      - **Persistenter Speicher**: Persistenter Speicher stellt Hochverfügbarkeit für Ihre Daten bereit. Wenn Sie mithilfe einer [vorhandenen Dateifreigabe](/docs/containers?topic=containers-file_storage#existing_file) einen Persistent Volume Claim (PVC) erstellt haben, dann können Sie beim Löschen des Clusters die Dateifreigabe nicht löschen. Sie müssen die Dateifreigabe zu einem späteren Zeitpunkt manuell aus Ihrem Portfolio der IBM Cloud-Infrastruktur (SoftLayer) löschen.

          Bedingt durch den monatlichen Abrechnungszyklus kann ein Persistent Volume Claim (PVC) nicht am letzten Tag des Monats gelöscht werden. Wenn Sie den Persistent Volume Claim am letzten Tag des Monats entfernen, verbleibt die Löschung bis zum Beginn des nächsten Monats in einem anstehenden Zustand.
          {: note}

Nächste Schritte:
- Nachdem ein Cluster nicht länger in der Liste verfügbarer Cluster enthalten ist, wenn Sie den Befehl `ibmcloud ks clusters` ausführen, können Sie den Namen eines entfernten Clusters wiederverwenden.
- Wenn Sie die Teilnetze behalten, können Sie [sie in einem neuen Cluster wiederverwenden](/docs/containers?topic=containers-subnets#subnets_custom) oder manuell zu einem späteren Zeitpunkt aus dem Portfolio der IBM Cloud-Infrastruktur (SoftLayer) löschen.
- Wenn Sie den persistenten Speicher beibehalten haben, können Sie den [Speicher](/docs/containers?topic=containers-cleanup#cleanup) später über das Dashboard der IBM Cloud-Infrastruktur (SoftLayer) in der {{site.data.keyword.Bluemix_notm}}-Konsole löschen.
