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



# VPN-Konnektivität einrichten
{: #vpn}

Mit der VPN-Konnektivität können Sie Apps in einem Kubernetes-Cluster unter {{site.data.keyword.containerlong}} sicher mit einem lokalen Netz verbinden. Sie können auch Apps, die nicht in Ihrem Cluster enthalten sind, mit Apps verbinden, die Teil Ihres Clusters sind.
{:shortdesc}

Um eine Verbindung Ihrer Workerknoten und Apps mit einem lokalen Rechenzentrum einzurichten, können Sie eine der folgenden Optionen konfigurieren.

- **StrongSwan-IPSec-VPN-Service**: Sie können einen [StrongSwan-IPSec-VPN-Service ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.strongswan.org/about.html) konfigurieren, der Ihren Kubernetes-Cluster sicher mit einem lokalen Netz verbindet. Der StrongSwan-IPSec-VPN-Service stellt einen sicheren End-to-End-Kommunikationskanal über das Internet bereit, der auf der standardisierten IPSec-Protokollsuite (IPSec – Internet Protocol Security) basiert. Um eine sichere Verbindung zwischen Ihrem Cluster und einem lokalen Netz einzurichten, [konfigurieren und implementieren Sie den StrongSwan-IPSec-VPN-Service](#vpn-setup) direkt in einem Pod in Ihrem Cluster.

- **Virtual Router Appliance (VRA) oder Fortigate Security Appliance (FSA)**: Sie können eine [VRA (Vyatta)](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-about-the-vra) oder [FSA](/docs/services/vmwaresolutions/services?topic=vmware-solutions-fsa_considerations) einrichten, um einen IPSec-VPN-Endpunkt zu konfigurieren. Diese Option ist hilfreich, wenn der Cluster größer ist, Sie über ein einzelnes VPN auf mehrere Cluster zugreifen möchten oder Sie ein routenbasiertes VPN benötigen. Informationen zum Konfigurieren einer VRA finden Sie unter [VPN-Konnektivität mit VRA konfigurieren](#vyatta).

## Helm-Diagramm für StrongSwan-IPSec-VPN-Service
{: #vpn-setup}

Verwenden Sie ein Helm-Diagramm, um den StrongSwan-IPSec-VPN-Service innerhalb eines Kubernetes-Pods zu konfigurieren und bereitzustellen.
{:shortdesc}

Da strongSwan in Ihren Cluster integriert ist, benötigen Sie keine externe Gateway-Einheit. Beim Einrichten der VPN-Konnektivität werden automatisch Routen auf allen Workerknoten im Cluster konfiguriert. Diese Routen ermöglichen eine bidirektionale Konnektivität über den VPN-Tunnel zwischen Pods auf allen Workerknoten und dem fernen System. Das folgende Diagramm zeigt beispielsweise, wie eine App in {{site.data.keyword.containerlong_notm}} mit einem lokalen Server über eine strongSwan-VPN-Verbindung kommunizieren kann:

<img src="images/cs_vpn_strongswan.png" width="700" alt="Machen Sie eine App in {{site.data.keyword.containerlong_notm}} zugänglich, indem Sie einen Netzlastausgleichsfunktion (NLB) verwenden" style="width:700px; border-style: none"/>

1. Eine App in Ihrem Cluster, `myapp`, empfängt eine Anforderung von einem Ingress- oder LoadBalancer-Service und muss eine sichere Verbindung mit Daten in Ihrem lokalen Netz herstellen.

2. Die Anforderung an das lokale Rechenzentrum wird an den IPSec-strongSwan-VPN-Pod weitergeleitet. Die IP-Zieladresse wird verwendet, um zu bestimmen, welche Netzpakete an den IPSec-strongSwan-VPN-Pod gesendet werden sollen.

3. Die Anforderung wird verschlüsselt und über den VPN-Tunnel an das lokale Rechenzentrum gesendet.

4. Die eingehende Anforderung passiert die lokale Firewall und wird an den VPN-Tunnelendpunkt (Router) zugestellt, wo sie entschlüsselt wird.

5. Der VPN-Tunnelendpunkt (Router) leitet die Anforderung abhängig von der in Schritt 2 angegebenen Ziel-IP-Adresse an den lokalen Server oder den Mainframe weiter. Die erforderlichen Daten werden auf dieselbe Weise über die VPN-Verbindung zurück an `myapp` gesendet.

## Überlegungen zum StrongSwan-VPN-Service
{: #strongswan_limitations}

Prüfen Sie folgende Überlegungen und Einschränkungen, bevor Sie das StrongSwan-Helm-Diagramm verwenden.
{: shortdesc}

* Für das StrongSwan-Helm-Diagramm ist erforderlich, dass die NAT-Traversierung (NAT – Network Address Translation) vom fernen VPN-Endpunkt aktiviert wird. Für die NAT-Traversierung ist neben dem IPSec-UDP-Standardport 500 der UDP-Port 4500 erforderlich. Für beide UDP-Ports muss eine eventuell konfigurierte Firewall Durchlass gewähren.
* Das StrongSwan-Helm-Diagramm unterstützt keine routenbasierten IPSec-VPNs.
* Das StrongSwan-Helm-Diagramm unterstützt IPSec-VPNs, die vorab bekannte verteilte Schlüssel verwenden, unterstützt jedoch nicht IPSec-VPNs, die Zertifikate erfordern.
* Das StrongSwan-Helm-Diagramm lässt nicht zu, dass mehrere Cluster und andere IaaS-Ressourcen eine einzige VPN-Verbindung gemeinsam nutzen.
* Das StrongSwan-Helm-Diagramm wird innerhalb des Clusters als Kubernetes-Pod ausgeführt. Die Speicher- und Netzverwendung von Kubernetes und anderen Pods, die im Cluster ausgeführt werden, wirkt sich auf die VPN-Leistung aus. Wenn Sie eine leistungskritische Umgebung haben, sollten Sie in Betracht ziehen, eine VPN-Lösung zu verwenden, die außerhalb des Clusters auf dedizierter Hardware ausgeführt wird.
* Das StrongSwan-Helm-Diagramm führt einen einzelnen VPN-Pod als IPSec-Tunnel-Endpunkt aus. Wenn der Pod fehlschlägt, startet der Cluster den Pod erneut. Es kann jedoch sein, dass eine kurze Ausfallzeit auftritt, während der neue Pod gestartet wird und die VPN-Verbindung neu eingerichtet wird. Wenn für Sie eine schnellere Fehlerbehebung oder eine ausgefeiltere Hochverfügbarkeitslösung erforderlich ist, sollten Sie in Betracht ziehen, eine VPN-Lösung zu verwenden, die außerhalb des Clusters auf dedizierter Hardware ausgeführt wird.
* Das StrongSwan-Helm-Diagramm stellt keine Messwerte oder eine Überwachung des Netzwerkverkehrs bereit, der über die VPN-Verbindung fließt. Eine Liste der unterstützten Überwachungstools finden Sie im Abschnitt [Protokollierungs- und Überwachungsservices](/docs/containers?topic=containers-supported_integrations#health_services).

<br />


## StrongSwan-VPN in einem Mehrzonencluster konfigurieren
{: #vpn_multizone}

Mehrzonencluster stellen hohe Verfügbarkeit für Apps für den Fall eines Ausfalls bereit, indem sie App-Instanzen auf Workerknoten in mehreren Zonen verfügbar machen. Allerdings ist die Konfiguration des StrongSwan-VPN-Service in einem Mehrzonencluster komplexer als die Konfiguration von StrongSwan in einem Einzelzonencluster.
{: shortdesc}

Bevor Sie StrongSwan in einem Mehrzonencluster konfigurieren, versuchen Sie zunächst, ein StrongSwan-Helm-Diagramm in einem Einzelzonencluster bereitzustellen. Wenn Sie zu Anfang eine VPN-Verbindung zwischen einem Einzelzonencluster und einem lokalen Netzwerk einrichten, können Sie Firewalleinstellungen für das ferne Netz einfacher ermitteln, die für die Konfiguration von StrongSwan in mehreren Zonen wichtig sind:
* Einige ferne VPN-Endpunkte haben Einstellungen wie `leftid` oder `rightid` in der Datei `ipsec.conf`. Wenn Sie diese Einstellungen haben, prüfen Sie, ob Sie die Einstellung `leftid` auf die IP-Adresse des VPN-IPSec-Tunnels setzen müssen.
*	Wenn die Verbindung aus dem fernen Netz in den Cluster eingehend ist, prüfen Sie, ob der ferne VPN-Endpunkt die VPN-Verbindung zu einer anderen IP-Adresse wiederherstellen kann, wenn ein Fehler der Lastausgleichsfunktion in einer Zone auftritt.

Wählen Sie eine der folgenden Optionen aus, um mit StrongSwan in einem Mehrzonencluster zu beginnen.
* Wenn Sie eine ausgehende VPN-Verbindung verwenden können, haben Sie die Möglichkeit, nur eine StrongSwan-VPN-Bereitstellung zu konfigurieren. Weitere Informationen finden Sie unter [Einzelne ausgehende VPN-Verbindung aus einem Mehrzonencluster konfigurieren](#multizone_one_outbound).
* Wenn Sie eine eingehende VPN-Verbindung benötigen, variieren die Konfigurationseinstellungen, die Sie verwenden können, abhängig davon, ob der ferne VPN-Endpunkt so konfiguriert werden kann, dass er die VPN-Verbindung zu einer anderen öffentlichen IP-Adresse der Lastausgleichsfunktion wiederherstellt, wenn ein Ausfall erkannt wird.
  * Wenn der ferne VPN-Endpunkt die VPN-Verbindung automatisch zu einer anderen IP-Adresse wiederherstellen kann, haben Sie die Möglichkeit, nur eine StrongSwan-VPN-Bereitstellung zu konfigurieren. Weitere Informationen finden Sie unter [Nur eine eingehende VPN-Verbindung zu einem Mehrzonencluster konfigurieren](#multizone_one_inbound).
  * Wenn der ferne VPN-Endpunkt die VPN-Verbindung nicht automatisch zu einer anderen IP-Adresse wiederherstellen kann, müssen Sie einen separaten eingehenden StrongSwan-VPN-Service in jeder Zone bereitstellen. Weitere Informationen finden Sie unter [VPN-Verbindung in jeder Zone eines Mehrzonenclusters konfigurieren](#multizone_multiple).

Versuchen Sie, Ihre Umgebung so einzurichten, dass Sie nur eine StrongSwan-VPN-Bereitstellung für eine ausgehende und eine eingehende VPN-Verbindung zu Ihrem Mehrzonencluster benötigen. Wenn Sie separate StrongSwan-VPNs in jeder Zone einrichten müssen, stellen Sie sicher, dass Sie planen, wie diese zusätzliche Komplexität und die erhöhte Ressourcennutzung verwaltet werden sollen.
{: note}

### Einzelne ausgehende VPN-Verbindung aus einem Mehrzonencluster konfigurieren
{: #multizone_one_outbound}

Die einfachste Lösung für die Konfiguration des StrongSwan-VPN-Service in einem Mehrzonencluster besteht in der Verwendung einer einzelnen VPN-Verbindung, die zwischen verschiedenen Workerknoten über alle Verfügbarkeitszonen in Ihrem Cluster hinweg gemeinsam genutzt wird.
{: shortdesc}

Wenn die VPN-Verbindung aus dem Mehrzonencluster ausgeht, ist nur eine StrongSwan-Bereitstellung erforderlich. Wenn ein Workerknoten entfernt wird oder eine Ausfallzeit erfährt, plant `kubelet` den VPN-Pod auf einem neuen Workerknoten erneut. Wenn eine Verfügbarkeitszone nicht verfügbar wird, plant `kubelet` den VPN-Pod auf einem neuen Workerknoten in einer anderen Zone erneut.

1. [Konfigurieren Sie nur ein StrongSwan-VPN-Helm-Diagramm](/docs/containers?topic=containers-vpn#vpn_configure). Wenn Sie die Schritte in diesem Abschnitt ausführen, stellen Sie sicher, dass Sie die folgenden Einstellungen angeben:
    - `ipsec.auto`: Ändern Sie den Wert in `start`. Verbindungen gehen von dem Cluster aus.
    - `loadBalancerIP`: Geben Sie keine IP-Adresse an. Lassen Sie diese Einstellung leer.
    - `zoneLoadBalancer`: Geben Sie eine öffentliche IP-Adresse für die Lastausgleichsfunktion für jede Zone an, in der Sie Workerknoten haben. [Sie können Ihre verfügbaren öffentlichen IP-Adressen zur Überprüfung anzeigen](/docs/containers?topic=containers-subnets#review_ip) oder [eine bereits verwendete IP-Adresse freigeben](/docs/containers?topic=containers-subnets#free). Da der StrongSwan-VPN-Pod auf einem Workerknoten in einer beliebigen Zone geplant werden kann, stellt diese Liste von IP-Adressen sicher, dass eine IP-Adresse für die Lastausgleichsfunktion in jeder Zone verwendet werden kann, in der der VPN-Pod geplant wird.
    - `connectUsingLoadBalancerIP`: Setzen Sie den Wert auf `true`. Wenn der StrongSwan-VPN-Pod auf einem Workerknoten geplant wird, wählt der StrongSwan-Service die IP-Adresse der Lastausgleichsfunktion aus, die sich in derselben Zone befindet, und verwendet diese IP-Adresse, um eine ausgehende Verbindung herzustellen.
    - `local.id`: Geben Sie einen festen Wert an, der von Ihrem fernen VPN-Endpunkt unterstützt wird. Wenn der ferne VPN-Endpunkt erfordert, dass Sie die Option `local.id` (Wert für `leftid` in der Datei `ipsec.conf`) auf die öffentliche IP-Adresse des VPN-IPSec-Tunnels setzen, geben Sie für `local.id` den Wert `%loadBalancerIP` an. Dieser Wert konfiguriert automatisch den Wert für `leftid` in der Datei `ipsec.conf` mit der IP-Adresse der Lastausgleichsfunktion, die für die Verbindung verwendet wird.

2. Lassen Sie in der Ihrer fernen Netzfirewall eingehende IPSec-VPN-Verbindungen von den öffentlichen IP-Adressen zu, die Sie in der Einstellung `zoneLoadBalancer` aufgelistet haben.

3. Konfigurieren Sie den fernen VPN-Endpunkt so, dass eine eingehende VPN-Verbindung von jeder der möglichen IP-Adressen der Lastausgleichsfunktion zugelassen wird, die Sie in der Einstellung `zoneLoadBalancer` aufgelistet haben.

### Einzelne eingehende VPN-Verbindung zu einem Mehrzonencluster konfigurieren
{: #multizone_one_inbound}

Wenn Sie eingehende VPN-Verbindungen benötigen und der ferne VPN-Endpunkt automatisch die VPN-Verbindung zu einer anderen IP-Adresse wiederherstellen kann, wenn ein Fehler erkannt wird, können Sie eine einzelne eingehende VPN-Verbindung verwenden, die zwischen verschiedenen Workerknoten in allen Verfügbarkeitszonen in Ihrem Cluster gemeinsam genutzt werden kann.
{: shortdesc}

Der ferne VPN-Endpunkt kann die VPN-Verbindung zu jeder der StrongSwan-Lastausgleichsfunktionen in jeder der Zonen herstellen. Die eingehende Anforderung wird an den VPN-Pod unabhängig davon gesendet, in welcher Zone sich er VPN-Pod befindet. Antworten aus dem VPN-Pod werden über die ursprüngliche Lastausgleichsfunktion an den fernen VPN-Endpunkt gesendet. Diese Option stellt hohe Verfügbarkeit sicher, weil `kubelet` den VPN-Pod auf einem neuen Workerknoten plant, wenn der Workerknoten entfernt wird oder irgendwie ausfällt. Außerdem kann der ferne VPN-Endpunkt bei einem Ausfall der Verfügbarkeitszone die VPN-Verbindung zu der IP-Adresse der Lastausgleichsfunktion in einer anderen Zone wiederherstellen, sodass der VPN-Pod weiterhin erreichbar bleibt.

1. [Konfigurieren Sie nur ein StrongSwan-VPN-Helm-Diagramm](/docs/containers?topic=containers-vpn#vpn_configure). Wenn Sie die Schritte in diesem Abschnitt ausführen, stellen Sie sicher, dass Sie die folgenden Einstellungen angeben:
    - `ipsec.auto`: Ändern Sie den Wert in `add`. Verbindungen gehen in den Cluster ein.
    - `loadBalancerIP`: Geben Sie keine IP-Adresse an. Lassen Sie diese Einstellung leer.
    - `zoneLoadBalancer`: Geben Sie eine öffentliche IP-Adresse für die Lastausgleichsfunktion für jede Zone an, in der Sie Workerknoten haben. [Sie können Ihre verfügbaren öffentlichen IP-Adressen zur Überprüfung anzeigen](/docs/containers?topic=containers-subnets#review_ip) oder [eine bereits verwendete IP-Adresse freigeben](/docs/containers?topic=containers-subnets#free).
    - `local.id`: Wenn der ferne VPN-Endpunkt erfordert, dass Sie die Option `local.id` (Wert für `leftid` in der Datei `ipsec.conf`) auf die öffentliche IP-Adresse des VPN-IPSec-Tunnels setzen, geben Sie für `local.id` den Wert `%loadBalancerIP` an. Dieser Wert konfiguriert automatisch den Wert für `leftid` in der Datei `ipsec.conf` mit der IP-Adresse der Lastausgleichsfunktion, die für die Verbindung verwendet wird.

2. Lassen Sie in der Ihrer fernen Netzfirewall ausgehende IPSec-VPN-Verbindungen zu den öffentlichen IP-Adressen zu, die Sie in der Einstellung `zoneLoadBalancer` aufgelistet haben.

### Eingehende VPN-Verbindung in jede Zone eines Mehrzonenclusters konfigurieren
{: #multizone_multiple}

Wenn Sie eingehende VPN-Verbindungen benötigen und der ferne VPN-Endpunkt die Verbindung nicht zu einer anderen IP-Adresse wiederherstellen kann, müssen Sie in jeder Zone einen separaten StrongSwan-VPN-Service bereitstellen.
{: shortdesc}

Der ferne VPN-Endpunkt muss aktualisiert werden, um eine separate VPN-Verbindung zu einer Lastausgleichsfunktion in jeder der Zonen herzustellen. Darüber hinaus müssen Sie zonenspezifische Einstellungen auf dem fernen VPN-Endpunkt konfigurieren, sodass jede dieser VPN-Verbindungen eindeutig ist. Stellen Sie sicher, dass diese mehreren VPN-Verbindungen zu allen Zeiten aktiv bleiben.

Nach der Bereitstellung aller Helm-Diagramme wird jede StrongSwan-VPN-Bereitstellung als Kubernetes-Lastausgleichsservice in der richtigen Zone gestartet. Eingehende Anforderungen an diese öffentliche IP-Adresse werden an den VPN-Pod weitergeleitet, der sich ebenfalls in derselben Zone befindet. Wenn die Zone ausfällt, bleiben die VPN-Verbindungen, die in den anderen Zonen hergestellt wurden, davon unberührt.

1. [Konfigurieren Sie ein StrongSwan-VPN-Helm-Diagramm](/docs/containers?topic=containers-vpn#vpn_configure) für jede Zone. Wenn Sie die Schritte in diesem Abschnitt ausführen, stellen Sie sicher, dass Sie die folgenden Einstellungen angeben:
    - `loadBalancerIP`: Geben Sie eine verfügbare öffentliche IP-Adresse für die Lastausgleichsfunktion an, die sich in der Zone befindet, in der Sie diesen StrongSwan-Service bereitstellen. [Sie können Ihre verfügbaren öffentlichen IP-Adressen zur Überprüfung anzeigen](/docs/containers?topic=containers-subnets#review_ip) oder [eine bereits verwendete IP-Adresse freigeben](/docs/containers?topic=containers-subnets#free).
    - `zoneSelector`: Geben Sie die Zone an, in der der VPN-Pod geplant werden soll.
    - Weitere Einstellungen wie `zoneSpecificRoutes`, `remoteSubnetNAT`, `localSubnetNAT` oder `enableSingleSourceIP` sind möglicherweise erforderlich, je nachdem, welche Ressourcen über das VPN zugänglich sein müssen. Weitere Informationen finden Sie im nächsten Schritt.

2. Konfigurieren Sie zonenspezifische Einstellungen auf beiden Seiten des VPN-Tunnels, um sicherzustellen, dass jede VPN-Verbindung eindeutig ist. Abhängig von den Ressourcen, die über das VPN zugänglich sein müssen, haben Sie zwei Optionen, die Verbindungen unterscheidbar zu machen:
    * Wenn Pods im Cluster auf Services über das ferne Standortnetz zugreifen müssen:
      - `zoneSpecificRoutes`: Setzen Sie den Wert auf `true`. Durch diese Einstellung wird die VPN-Verbindung auf eine einzelne Zone im Cluster beschränkt. Pods in einer bestimmte Zone verwenden nur die VPN-Verbindung, die für diese bestimmte Zone eingerichtet wurde. Diese Lösung verringert die Anzahl der StrongSwan-Pods, die zur Unterstützung mehrerer VPNs in einem Mehrzonencluster erforderlich sind, verbessert die VPN-Leistung, weil der VPN-Datenverkehr nur an Workerknoten fließt, die sich in der aktuellen Zone befinden, und stellt sicher, dass die VPN-Konnektivität jeder Zone von der VPN-Konnektivität, den ausgefallenen Pods sowie Zonenausfällen in anderen Zonen nicht betroffen wird. Beachten Sie, dass Sie die Einstellung `remoteSubnetNAT` nicht konfigurieren müssen. Mehrere VPNs, die die Einstellung `zoneSpecificRoutes` verwenden, können denselben Wert für `remote.subnet` haben, weil das Routing auf Zonenebene eingerichtet wird.
      - `enableSingleSourceIP`: Setzen Sie den Wert auf `true` und setzen Sie `local.subnet` auf eine einzelne /32 IP-Adresse. Diese Kombination von Einstellungen verbirgt alle privaten IP-Adressen des Clusters hinter einer einzelnen /32 IP-Adresse. Durch diese eindeutige /32 IP-Adresse kann das ferne Standortnetz Antworten über die richtige VPN-Verbindung an den richtigen Pod im Cluster, der die Anforderung eingeleitet hat, zurücksenden. Beachten Sie, dass die einzelne /32 IP-Adresse, die für die Option `local.subnet` konfiguriert wird, in jeder StrongSwan-VPN-Konfiguration eindeutig sein muss.
    * Wenn Anwendungen im fernen Standortnetz auf Services im Cluster zugreifen müssen:    
      - `localSubnetNAT`: Stellen Sie sicher, dass eine Anwendung im fernen Standortnetz eine bestimmte VPN-Verbindung auswählen kann, um Datenverkehr an den Cluster zu senden und vom Cluster zu empfangen. Verwenden Sie in jeder StrongSwan-Helm-Konfiguration die Einstellung `localSubnetNAT`, um die Clusterressourcen eindeutig anzugeben, auf die von der fernen Anwendung am Standort zugegriffen werden kann. Da mehrere VPNs vom fernen Standortnetz zum Cluster hergestellt werden, müssen Sie der Anwendung im Standortnetz Logik hinzufügen, sodass sie das zu verwendende VPN auswählen kann, wenn sie auf Services im Cluster zugreift. Beachten Sie, dass die Services im Cluster abhängig von der Konfiguration für `localSubetNAT` in jeder StrongSwan-VPN-Konfiguration über mehrere verschiedene Teilnetze zugänglich sind.
      - `remoteSubnetNAT`: Stellen Sie sicher, dass ein Pod in Ihrem Cluster dieselbe VPN-Verbindung verwendet, um Datenverkehr an das ferne Netz zurückzugeben. Ordnen Sie in jeder StrongSwan-Bereitstellungsdatei das ferne Standortnetz einem eindeutigen Teilnetz mit der Einstellung `remoteSubetNAT` zu. Datenverkehr, der von einem Pod im Cluster aus einem VPN-spezifischen fernen Teilnetz (`remoteSubnetNAT`) empfangen wird, wird an dasselbe ferne VPN-spezifische Teilnetz (`remoteSubnetNAT`) und dann über dieselbe VPN-Verbindung zurückgesendet.

3. Konfigurieren Sie die Software für den fernen VPN-Endpunkt so, dass eine separate VPN-Verbindung zu der IP-Adresse der Lastausgleichsfunktion in jeder Zone hergestellt wird.

<br />


## StrongSwan-Helm-Diagramm konfigurieren
{: #vpn_configure}

Bevor Sie das StrongSwan-Helm-Diagramm installieren, müssen Sie eine Entscheidung bezüglich Ihrer StrongSwan-Konfiguration treffen.
{: shortdesc}

Vorbereitende Schritte:
* [Installieren Sie ein IPSec-VPN-Gateway in Ihrem lokalen Rechenzentrum](/docs/infrastructure/iaas-vpn?topic=VPN-setup-ipsec-vpn#setup-ipsec-connection).
* Stellen Sie sicher, dass Sie die [{{site.data.keyword.Bluemix_notm}} IAM-Servicerolle **Schreibberechtigter** oder **Manager**](/docs/containers?topic=containers-users#platform) für den Namensbereich `default` innehaben.
* [Melden Sie sich an Ihrem Konto an. Geben Sie als Ziel die entsprechende Region und, sofern zutreffend, die Ressourcengruppe an. Legen Sie den Kontext für den Cluster fest.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
  * **Hinweis:** Alle StrongSwan-Konfigurationen werden in Standardclustern zugelassen. Wenn Sie einen kostenlosen Cluster verwenden, können Sie nur eine ausgehende VPN-Verbindung in [Schritt 3](#strongswan_3) auswählen. Eingehende VPN-Verbindungen erfordern eine Lastausgleichsfunktion im Cluster. Lastausgleichsfunktionen sind für kostenlose Cluster jedoch nicht verfügbar.

### Schritt 1: StrongSwan-Helm-Diagramm abrufen
{: #strongswan_1}

Installieren Sie Helm und holen Sie sich das StrongSwan-Helm-Diagramm, um mögliche Konfigurationen anzuzeigen.
{: shortdesc}

1.  [Befolgen Sie die Anweisungen](/docs/containers?topic=containers-helm#public_helm_install) zum Installieren des Helm-Clients auf Ihrer lokalen Maschine, installieren Sie den Helm-Server (tiller) mit einem Servicekonto und fügen Sie das {{site.data.keyword.Bluemix_notm}}-Helm-Repository hinzu. Beachten Sie, dass Helm version 2.8 oder höher erforderlich ist.

2.  Überprüfen Sie, ob 'tiller' mit einem Servicekonto installiert ist.

    ```
    kubectl get serviceaccount -n kube-system | grep tiller
    ```
    {: pre}

    Beispielausgabe:

    ```
    NAME                                 SECRETS   AGE
    tiller                               1         2m
    ```
    {: screen}

3. Speichern Sie die Standardkonfigurationseinstellungseinstellungen für das StrongSwan-Helm-Diagramm in einer lokalen YAML-Datei.

    ```
    helm inspect values iks-charts/strongswan > config.yaml
    ```
    {: pre}

4. Öffnen Sie die Datei `config.yaml`.

### Schritt 2: Grundlegende IPSec-Einstellungen konfigurieren
{: #strongswan_2}

Ändern Sie zum Steuern der Einrichtung der VPN-Verbindung die folgenden grundlegenden IPSec-Einstellungen.
{: shortdesc}

Weitere Informationen zu den einzelnen Einstellungen finden Sie in der Dokumentation, die in der Datei `config.yaml` für das Helm-Diagramm bereitgestellt wird.
{: tip}

1. Wenn Ihr lokaler VPN-Tunnelendpunkt `ikev2` als Protokoll für die Initialisierung der Verbindung nicht unterstützt, ändern Sie den Wert von `ipsec.keyexchange` in `ikev1`.
2. Legen Sie als `ipsec.esp` die Liste von ESP-Verschlüsselungs-/Authentifizierungsalgorithmen fest, die Ihr lokaler VPN-Tunnelendpunkt für die Verbindung verwendet.
    * Wenn `ipsec.keyexchange` auf `ikev1` festgelegt ist, muss diese Einstellung angegeben werden.
    * Wenn `ipsec.keyexchange` auf `ikev2` festgelegt ist, ist diese Einstellung optional.
    * Wenn Sie diese Einstellung leer lassen, wird der Standardwert für strongSwan-Algorithmen `aes128-sha1,3des-sha1` für die Verbindung verwendet.
3. Legen Sie als `ipsec.ike` die Liste von IKE/ISAKMP-SA-Verschlüsselungs-/Authentifizierungsalgorithmen fest, die Ihr lokaler VPN-Tunnelendpunkt für die Verbindung verwendet. Die Algorithmen müssen im Format `encryption-integrity[-prf]-dhgroup` angegeben werden.
    * Wenn `ipsec.keyexchange` auf `ikev1` festgelegt ist, muss diese Einstellung angegeben werden.
    * Wenn `ipsec.keyexchange` auf `ikev2` festgelegt ist, ist diese Einstellung optional.
    * Wenn Sie diese Einstellung leer lassen, wird der Standardwert für strongSwan-Algorithmen `aes128-sha1-modp2048,3des-sha1-modp1536` für die Verbindung verwendet.
4. Ändern Sie den Wert von `local.id` in eine beliebige Zeichenfolge, die Sie zum Identifizieren der lokalen Seite des Kubernetes-Clusters verwenden möchten, die Ihr VPN-Tunnelendpunkt verwendet. Die Standardeinstellung ist `ibm-cloud`. Für einige VPN-Implementierungen ist es erforderlich, dass Sie für den lokalen Endpunkt die öffentliche IP-Adresse verwenden.
5. Ändern Sie den Wert von `remote.id` in eine beliebige Zeichenfolge, die Sie zum Identifizieren der fernen lokalen Seite verwenden möchten, die Ihr VPN-Tunnelendpunkt verwendet. Die Standardeinstellung ist `on-prem`. Für einige VPN-Implementierungen ist es erforderlich, dass Sie für den fernen Endpunkt die öffentliche IP-Adresse verwenden.
6. Ändern Sie den Wert von `preshared.secret` in den vorab verteilten geheimen Schlüssel, den Ihr lokaler VPN-Tunnelendpunkt für die Verbindung verwendet. Dieser Wert wird in `ipsec.secrets` gespeichert.
7. Optional: Legen Sie für `remote.privateIPtoPing` eine beliebige private IP-Adresse im fernen Teilnetz fest, die im Rahmen des Helm-Tests zur Konnektivitätsprüfung mit Ping überprüft wird.

### Schritt 3: Eingehende oder ausgehende VPN-Verbindung auswählen
{: #strongswan_3}

Wenn Sie eine StrongSwan-VPN-Verbindung konfigurieren, wählen Sie aus, ob die VPN-Verbindung für den Cluster eingehend oder ausgehend ist.
{: shortdesc}

<dl>
<dt>Eingehend</dt>
<dd>Der lokale VPN-Endpunkt des fernen Netzes initialisiert die VPN-Verbindung und der Cluster ist für die Verbindung empfangsbereit.</dd>
<dt>Ausgehend</dt>
<dd>Der Cluster initialisiert die VPN-Verbindung und der lokale VPN-Endpunkt des fernen Netzes ist für die Verbindung empfangsbereit.</dd>
</dl>

Wenn Sie einen kostenlosen Cluster verwenden, können Sie nur eine ausgehende VPN-Verbindung auswählen. Eingehende VPN-Verbindungen erfordern eine Lastausgleichsfunktion im Cluster. Lastausgleichsfunktionen sind für kostenlose Cluster jedoch nicht verfügbar.

Ändern Sie zum Einrichten einer eingehenden VPN-Verbindung folgende Einstellungen:
1. Überprüfen Sie, dass für `ipsec.auto` die Einstellung `add` festgelegt ist.
2. Optional: Legen Sie für `loadBalancerIP` eine portierbare öffentliche IP-Adresse fest, die für den StrongSwan-VPN-Service gilt. Die Angabe einer IP-Adresse ist nützlich, wenn Sie eine fixe IP-Adresse benötigen, z. B. wenn Sie festlegen müssen, welche IP-Adressen von einer lokalen Firewall zugelassen werden. Der Cluster muss über mindestens eine verfügbare öffentliche IP-Adresse der Lastausgleichsfunktion verfügen. [Sie können Ihre verfügbaren öffentlichen IP-Adressen zur Überprüfung anzeigen](/docs/containers?topic=containers-subnets#review_ip) oder [eine bereits verwendete IP-Adresse freigeben](/docs/containers?topic=containers-subnets#free).
    * Wenn Sie diese Einstellung leer lassen, wird eine der verfügbaren und portierbaren öffentlichen IP-Adressen verwendet.
    * Sie müssen auch die öffentliche IP-Adresse konfigurieren, die Sie für den VPN-Endpunkt des Clusters im lokalen VPN-Endpunkt auswählen oder die diesem VPN-Endpunkt des Clusters im lokalen VPN-Endpunkt zugewiesen ist.

Ändern Sie zum Einrichten einer ausgehenden VPN-Verbindung folgende Einstellungen:
1. Ändern Sie `ipsec.auto` in `start`.
2. Legen Sie für `remote.gateway` die öffentliche IP-Adresse für den lokalen VPN-Endpunkt im fernen Netz fest.
3. Wählen Sie als IP-Adresse für den VPN-Endpunkt des Clusters eine der folgenden Optionen:
    * **Öffentliche IP-Adresse des privaten Gateways des Clusters**: Wenn Ihre Workerknoten nur mit einem privaten VLAN verbunden sind, wird die ausgehende VPN-Anforderung über das private Gateway weitergeleitet, um das Internet zu erreichen. Die öffentliche IP-Adresse des privaten Gateways wird für die VPN-Verbindung verwendet.
    * **Öffentliche IP-Adresse des Workerknotens, auf dem der StrongSwan-Pod ausgeführt wird**: Wenn der Workerknoten, auf dem der StrongSwan-Pod ausgeführt wird, mit einem öffentlichen VLAN verbunden ist, wird für die VPN-Verbindung die öffentliche IP-Adresse des Workerknotens verwendet.
        <br>
        * Wenn der StrongSwan-Pod gelöscht und auf einem anderen Workerknoten im Cluster neu geplant wird, ändert sich die öffentliche IP-Adresse des VPNs. Der lokale VPN-Endpunkt des fernen Netzes muss zulassen, dass die VPN-Verbindung über die öffentliche IP-Adresse eines der Workerknoten des Clusters hergestellt wird.
        * Wenn der ferne VPN-Endpunkt VPN-Verbindungen über mehrere öffentliche IP-Adressen nicht handhaben kann, begrenzen Sie die Knoten, auf denen der StrongSwan-VPN-Pod bereitgestellt wird. Legen Sie für `nodeSelector` die IP-Adressen bestimmter Workerknoten oder eine Workerknotenbezeichnung fest. Der Wert `kubernetes.io/hostname: 10.232.xx.xx` lässt beispielsweise nur zu, dass der VPN-Pod auf diesem Workerknoten bereitgestellt wird. Der Wert `strongswan: vpn` beschränkt die Ausführung des VPN-Pods auf alle Workerknoten mit dieser Bezeichnung. Sie können eine beliebige Workerknotenbezeichnung auswählen. Verwenden Sie `strongswan: <release_name>`, um zuzulassen, dass mit unterschiedlichen Helm-Diagramm-Bereitstellungen auch unterschiedliche Workerknoten verwendet werden können. Wählen Sie für hohe Verfügbarkeit mindestens zwei Workerknoten aus.
    * **Öffentliche IP-Adresse des StrongSwan-Service**: Zum Herstellen einer Verbindung mithilfe der IP-Adresse des StrongSwan-VPN-Service legen Sie für `connectUsingLoadBalancerIP` die Einstellung `true` fest. Die IP-Adresse des StrongSwan-Service ist entweder eine portierbare öffentliche IP-Adresse, die Sie in der Einstellung `loadBalancerIP` angeben können, oder eine verfügbare portierbare öffentliche IP-Adresse, die dem Service automatisch zugeordnet wird.
        <br>
        * Wenn Sie wählen, eine IP-Adresse mithilfe der Einstellung `loadBalancerIP` auszuwählen, muss der Cluster über mindestens eine verfügbare öffentliche IP-Adresse der Lastausgleichsfunktion verfügen. [Sie können Ihre verfügbaren öffentlichen IP-Adressen zur Überprüfung anzeigen](/docs/containers?topic=containers-subnets#review_ip) oder [eine bereits verwendete IP-Adresse freigeben](/docs/containers?topic=containers-subnets#free).
        * Alle Workerknoten des Clusters müssen sich im selben öffentlichen VLAN befinden. Andernfalls müssen Sie die Einstellung `nodeSelector` verwenden, um sicherzustellen, dass der VPN-Pod auf einem Workerknoten in demselben öffentliche VLAN wie `loadBalancerIP` bereitgestellt wird.
        * Wenn für `connectUsingLoadBalancerIP` die Einstellung `true` festgelegt wurde und für `ipsec.keyexchange` die Einstellung `ikev1` gewählt wurde, müssen Sie für `enableServiceSourceIP` die Einstellung `true` festlegen.

### Schritt 4: Über die VPN-Verbindung auf Clusterressourcen zugreifen
{: #strongswan_4}

Ermitteln Sie, welche Clusterressourcen für das ferne Netz über die VPN-Verbindung zugänglich sein müssen.
{: shortdesc}

1. Fügen Sie die CIDRs mindestens eines Cluster-Teilnetzes zur Einstellung `local.subnet` hinzu. Sie müssen die CIDRs des lokalen Teilnetzes im lokalen VPN-Endpunkt konfigurieren. Diese Liste kann die folgenden Teilnetze enthalten:  
    * Die Teilnetz-CIDR des Kubernetes-Pods: `172.30.0.0/16`. Die bidirektionale Kommunikation ist zwischen allen Cluster-Pods und jedem der Hosts in den Teilnetzen des fernen Netzes aktiviert, die Sie in der Einstellung `remote.subnet` auflisten. Wenn Sie aus Sicherheitsgründen verhindern müssen, dass einer der Hosts des fernen Teilnetzes (`remote.subnet`) auf Cluster-Pods zugreift, dürfen Sie das Teilnetz des Kubernetes-Pods nicht der Einstellung `local.subnet` hinzufügen.
    * Die Teilnetz-CIDR des Kubernetes-Service: `172.21.0.0/16`. Service-IP-Adressen bieten die Möglichkeit, mehrere App-Pods verfügbar zu machen, die auf mehreren Workerknoten mit einer einzigen IP bereitgestellt sind.
    * Wenn Ihre Apps über einen NodePort-Service im privaten Netz oder in einer privaten Ingress-ALB (ALB – Application Load Balancer) zugänglich gemacht werden, müssen Sie die private Teilnetz-CIDR des Workerknotens hinzufügen. Rufen Sie die ersten drei Oktette der privaten IP-Adresse Ihres Workers ab, indem Sie `ibmcloud ks worker <cluster_name>` ausführen. Beispiel: Bei `10.176.48.xx` notieren Sie sich `10.176.48`. Rufen Sie als Nächstes die private Teilnetz-CIDR Ihre Workers ab, indem Sie den folgenden Befehl ausführen. Ersetzen Sie dabei `<xxx.yyy.zz>` durch das zuvor abgerufene Oktett: `ibmcloud sl subnet list | grep <xxx.yyy.zzz>`. **Hinweis**: Wenn ein Workerknoten in einem neuen privaten Teilnetz hinzugefügt wird, müssen Sie die neue private Teilnetz-CIDR zur `local.subnet`-Einstellung und zum lokalen VPN-Endpunkt hinzufügen. Anschließend muss die VPN-Verbindung erneut gestartet werden.
    * Wenn Sie Apps über LoadBalancer-Services im privaten Netz zugänglich machen, müssen Sie die private benutzerverwaltete Teilnetz-CIDR des Clusters hinzufügen. Führen Sie zum Ermitteln dieser Werte den Befehl `ibmcloud ks cluster-get --cluster <clustername> --showResources` aus. Suchen Sie im Abschnitt **VLANs** nach CIDRs, die für **Public** den Wert `false` aufweisen. **Hinweis**: Wenn `ipsec.keyexchange` auf `ikev1` festgelegt ist, können Sie nur ein Teilnetz angeben. Sie können jedoch die Einstellung `localSubnetNAT` verwenden, um mehrere Teilnetze eines Clusters zu einem einzigen Teilnetz zu kombinieren.

2. Optional: Ordnen Sie Teilnetze eines Clusters erneut zu, indem Sie die Einstellung `localSubnetNAT` verwenden. Network Address Translation (NAT) für Teilnetze bietet eine Ausweichlösung für Teilnetzkonflikte zwischen dem Clusternetz und dem lokalen fernen Netz. Sie können NAT verwenden, um die privaten lokalen IP-Teilnetze des Clusters, das Pod-Teilnetz (172.30.0.0/16) oder das Teilnetz des Pod-Service (172.21.0.0/16) zu einem anderen privaten Teilnetz zuzuordnen. Der VPN-Tunnel erkennt erneut zugeordnete IP-Teilnetze, die an Stelle der ursprünglichen Teilnetze treten. Die erneute Zuordnung erfolgt vor dem Senden der Pakete über den VPN-Tunnel sowie nach dem Eintreffen der Pakete aus dem VPN-Tunnel. Sie können sowohl erneut zugeordnete als auch nicht erneut zugeordnete Teilnetze gleichzeitig über VPN bereitstellen. Um NAT aktivieren zu können, können Sie entweder ein vollständiges Teilnetz oder einzelne IP-Adressen hinzufügen.
    * Wenn Sie ein vollständiges Teilnetz im Format `10.171.42.0/24=10.10.10.0/24` hinzufügen, erfolgt die erneute Zuordnung 1-zu-1: Alle IP-Adressen im internen Teilnetz werden einem externen Teilnetz zugeordnet und umgekehrt.
    * Wenn Sie einzelne IP-Adressen im Format `10.171.42.17/32=10.10.10.2/32,10.171.42.29/32=10.10.10.3/32` zuordnen, werden nur diese internen IP-Adressen den angegebenen externen IP-Adressen zugeordnet.

3. Optional für StrongSwan-Helm-Diagramme der Version 2.2.0 und höher: Verbergen Sie alle Cluster-IP-Adressen hinter einer einzigen IP-Adresse, indem Sie für `enableSingleSourceIP` die Einstellung `true` festlegen. Diese Option stellt eine der sichersten Konfigurationen für die VPN-Verbindung bereit, da keine Verbindungen vom fernen Netz zurück in den Cluster zulässig sind.
    <br>
    * Diese Einstellung erfordert, dass der gesamte Datenfluss über die VPN-Verbindung ausgehend sein muss; dies ist unabhängig davon, ob die VPN-Verbindung vom Cluster oder vom fernen Netz eingerichtet wird.
    * Für `local.subnet` muss lediglich ein /32-Teilnetz festgelegt werden.

4. Optional für StrongSwan-Helm-Diagramme der Version 2.2.0 und höher: Aktivieren Sie den StrongSwan-Service, sodass eingehende Anforderungen des fernen Netzes an einen Service weitergeleitet werden, der sich außerhalb des Clusters befindet. Verwenden Sie dazu die Einstellung `localNonClusterSubnet`.
    <br>
    * Der nicht zum Cluster gehörende Service muss im selben privaten Netz oder in einem privaten Netz vorhanden sein, das für die Workerknoten erreichbar ist.
    * Der nicht zum Cluster gehörende Workerknoten kann keinen Datenverkehr zum fernen Netz über die VPN-Verbindung einleiten, aber der nicht zum Cluster gehörende Knoten kann das Ziel eingehender Anforderungen des fernen Netzes sein.
    * Sie müssen die CIDRs der nicht zum Cluster gehörenden Teilnetze in der Einstellung `local.subnet` auflisten.

### Schritt 5: Über die VPN-Verbindung auf ferne Netzressourcen zugreifen
{: #strongswan_5}

Ermitteln Sie, welche fernen Netzressourcen für das Cluster über die VPN-Verbindung zugänglich sein müssen.
{: shortdesc}

1. Fügen Sie die CIDRs mindestens eines lokalen privaten Teilnetzes zur Einstellung `remote.subnet` hinzu. **Hinweis**: Wenn `ipsec.keyexchange` auf `ikev1` festgelegt ist, können Sie nur ein Teilnetz angeben.
2. Optional für StrongSwan-Helm-Diagramme der Version 2.2.0 und höher: Ordnen Sie Teilnetze des fernen Netzes neu zu, indem Sie die Einstellung `remoteSubnetNAT` verwenden. Network Address Translation (NAT) für Teilnetze bietet eine Ausweichlösung für Teilnetzkonflikte zwischen dem Clusternetz und dem lokalen fernen Netz. Sie können NAT verwenden, um die IP-Teilnetze des fernen Netzes einem anderen privaten Teilnetz zuzuordnen. Die erneute Zuordnung erfolgt vor dem Senden der Pakete über den VPN-Tunnel. Pods im Cluster erkennen die erneut zugeordneten IP-Teilnetze, die an Stelle der ursprünglichen Teilnetze treten. Bevor die Pods Daten über den VPN-Tunnel zurücksenden, wird das neu zugeordnete IP-Teilnetz wieder auf das tatsächliche Teilnetz zurückgestellt, das vom fernen Netz verwendet wird. Sie können sowohl erneut zugeordnete als auch nicht erneut zugeordnete Teilnetze gleichzeitig über VPN bereitstellen.

### Schritt 6 (optional): Überwachung mit der Slack-Webhook-Integration aktivieren
{: #strongswan_6}

Um den Status des StrongSwan-VPN zu überwachen, können Sie einen Webhook zum automatischen Posten von VPN-Konnektivitätsnachrichten an einen Slack-Kanal einrichten.
{: shortdesc}

1. Melden Sie sich bei Ihrem Slack-Arbeitsbereich an.

2. Rufen Sie die [App-Seite für eingehende Webhooks ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://slack.com/apps/A0F7XDUAZ-incoming-webhooks) auf.

3. Klicken Sie auf **Installationsanforderung**. Falls diese App nicht in Ihrer Slack-Konfiguration aufgeführt ist, wenden Sie sich an den Eigner Ihres Slack-Arbeitsbereichs.

4. Nachdem Ihre Installationsanforderung genehmigt wurde, klicken Sie auf **Konfiguration hinzufügen**.

5. Wählen Sie einen Slack-Kanal aus oder erstellen Sie einen neuen Kanal, an den die VPN-Nachrichten gesendet werden sollen.

6. Kopieren Sie die Webhook-URL, die generiert wird. Das URL-Format sieht wie folgt aus:
  ```
  https://hooks.slack.com/services/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
  ```
  {: screen}

7. Um zu verifizieren, dass der Slack-Webhook installiert wurde, senden Sie eine Testnachricht an Ihre Webhook-URL, indem Sie den folgenden Befehl ausführen:
    ```
    curl -X POST -H 'Content-type: application/json' -d '{"text":"VPN-Testnachricht"}' <webhook-URL>
    ```
    {: pre}

8. Wechseln Sie zu dem Slack-Kanal, den Sie ausgewählt haben, um zu verifizieren, dass die Testnachricht erfolgreich ist.

9. Konfigurieren Sie in der Datei `config.yaml` für das Helm-Diagramm den Webhook zum Überwachen Ihrer VPN-Verbindung.
    1. Ändern Sie `monitoring.enable` in `true`.
    2. Fügen Sie private IP-Adressen oder HTTP-Endpunkte in dem fernen Teilnetz hinzu, für die Sie sicherstellen möchten, dass sie über die VPN-Verbindung zu `monitoring.privateIPs` oder `monitoring.httpEndpoints` erreichbar sind. Sie können beispielsweise die IP aus der Einstellung `remote.privateIPtoPing` zu `monitoring.privateIPs` hinzufügen.
    3. Fügen Sie die Webhook-URL zu `monitoring.slackWebhook` hinzu.
    4. Ändern Sie andere optionale Überwachungseinstellungen (`monitoring`) nach Bedarf.

### Schritt 7: Helm-Diagramm bereitstellen
{: #strongswan_7}

Stellen Sie das StrongSwan-Helm-Diagramm in Ihrem Cluster mit den Konfigurationen, die Sie zuvor ausgewählt haben, bereit.
{: shortdesc}

1. Beachten Sie die Dokumentation, die für jede Einstellung im Helm-Diagramm bereitgestellt ist, wenn Sie weitere erweiterte Einstellungen konfigurieren müssen.

3. Speichern Sie die aktualisierte Datei `config.yaml`.

4. Installieren Sie das Helm-Diagramm auf Ihrem Cluster mit der aktualisierten Datei `config.yaml`.

    Wenn Sie über mehrere VPN-Bereitstellungen in einem einzelnen Cluster verfügen, können Sie Namenskonflikte vermeiden und zwischen Ihren Bereitstellungen unterscheiden, indem Sie aussagekräftigere Releasenamen als `vpn` verwenden. Um das Abschneiden des Releasenamens zu vermeiden, begrenzen Sie den Releasenamen auf maximal 35 Zeichen.
    {: tip}

    ```
    helm install -f config.yaml --name=vpn iks-charts/strongswan
    ```
    {: pre}

5. Prüfen Sie den Status der Diagrammbereitstellung. Sobald das Diagramm bereit ist, hat das Feld **STATUS** oben in der Ausgabe den Wert `DEPLOYED`.

    ```
    helm status vpn
    ```
    {: pre}

6. Sobald das Diagramm bereitgestellt ist, überprüfen Sie, dass die aktualisierten Einstellungen in der Datei `config.yaml` verwendet wurden.

    ```
    helm get values vpn
    ```
    {: pre}

## StrongSwan-VPN-Konnektivität testen und überprüfen
{: #vpn_test}

Nachdem Sie Ihr Helm-Diagramm bereitgestellt haben, testen Sie die VPN-Konnektivität.
{:shortdesc}

1. Wenn das VPN im lokalen Gateway nicht aktiv ist, starten Sie das VPN.

2. Legen Sie die Umgebungsvariable `STRONGSWAN_POD` fest.

    ```
    export STRONGSWAN_POD=$(kubectl get pod -l app=strongswan,release=vpn -o jsonpath='{ .items[0].metadata.name }')
    ```
    {: pre}

3. Prüfen Sie den Status des VPN. Der Status `ESTABLISHED` bedeutet, dass die VPN-Verbindung erfolgreich war.

    ```
    kubectl exec $STRONGSWAN_POD -- ipsec status
    ```
    {: pre}

    Beispielausgabe:

    ```
    Security Associations (1 up, 0 connecting):
    k8s-conn[1]: ESTABLISHED 17 minutes ago, 172.30.xxx.xxx[ibm-cloud]...192.xxx.xxx.xxx[on-premises]
    k8s-conn{2}: INSTALLED, TUNNEL, reqid 12, ESP in UDP SPIs: c78cb6b1_i c5d0d1c3_o
    k8s-conn{2}: 172.21.0.0/16 172.30.0.0/16 === 10.91.152.xxx/26
    ```
    {: screen}

    * Wenn Sie versuchen, die VPN-Konnektivität mit dem StrongSwan-Helm-Diagramm zu erstellen, ist es wahrscheinlich, dass das VPN beim ersten Mal nicht den Status `ESTABLISHED` aufweist. Unter Umständen müssen Sie die Einstellungen des lokalen VPN-Endpunkts prüfen und die Konfigurationsdatei mehrmals ändern, bevor die Verbindung erfolgreich ist:
        1. Führen Sie den folgenden Befehl aus: `helm delete --purge <release_name>`
        2. Korrigieren Sie die falschen Werte in der Konfigurationsdatei.
        3. Führen Sie folgenden Befehl aus: `helm install -f config.yaml --name=<release_name> ibm/strongswan`
      Sie können im nächsten Schritt noch weitere Prüfungen vornehmen.

    * Falls der VPN-Pod den Status `ERROR` aufweist oder immer wieder ausfällt und neu startet, kann dies an der Parametervalidierung der `ipsec.conf`-Einstellungen in der Konfigurationszuordnung des Diagramms liegen.
        1. Prüfen Sie, ob dies der Fall ist, indem Sie mithilfe des Befehls `kubectl logs $STRONGSWAN_POD` in den Protokollen des StrongSwan-Pods nach Validierungsfehlern suchen.
        2. Wenn Gültigkeitsfehler vorhanden sind, führen Sie folgenden Befehl aus: `helm delete --purge <release_name>`
        3. Korrigieren Sie die falschen Werte in der Konfigurationsdatei.
        4. Führen Sie folgenden Befehl aus: `helm install -f config.yaml --name=<release_name> ibm/strongswan`

4. Sie können die VPN-Konnektivität weiter testen, indem Sie die fünf Helm-Tests ausführen, die in der StrongSwan-Diagrammdefinition enthalten sind.

    ```
    helm test vpn
    ```
    {: pre}

    * Wenn alle Tests erfolgreich sind, wurde Ihre StrongSwan-VPN-Verbindung erfolgreich konfiguriert.
    * Wenn ein Test fehlschlägt, fahren Sie mit dem nächsten Schritt fort.

5. Zeigen Sie die Ausgabe eines fehlgeschlagenen Tests an, indem Sie die Protokolle des Testpods anzeigen.

    ```
    kubectl logs <testprogramm>
    ```
    {: pre}

    Einige der Tests haben als Voraussetzungen optionale Einstellungen in der VPN-Konfiguration. Wenn einige der Tests fehlschlagen, kann dies abhängig davon, ob Sie diese optionalen Einstellungen angegeben haben, zulässig sein. Suchen Sie in der folgenden Tabelle nach Informationen zu den einzelnen Tests und warum ein Test fehlschlagen könnte.
    {: note}

    {: #vpn_tests_table}
    <table>
    <caption>Erklärung der Helm-VPN-Konnektivitätstests</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der Helm-VPN-Konnektivitätstests</th>
    </thead>
    <tbody>
    <tr>
    <td><code>vpn-strongswan-check-config</code></td>
    <td>Überprüft die Syntax der Datei <code>ipsec.conf</code>, die von der Datei <code>config.yaml</code> generiert wird. Dieser Test kann aufgrund von falschen Werten in der Datei <code>config.yaml</code> fehlschlagen.</td>
    </tr>
    <tr>
    <td><code>vpn-strongswan-check-state</code></td>
    <td>Überprüft, ob die VPN-Verbindung den Status <code>ESTABLISHED</code> aufweist. Dieser Test kann aus folgenden Gründen fehlschlagen:<ul><li>Unterschiede zwischen den Werten in der Datei <code>config.yaml</code> und den lokalen VPN-Endpunkteinstellungen im Unternehmen.</li><li>Wenn der Cluster empfangsbereit ist (im Modus "listen") (für <code>ipsec.auto</code> ist <code>add</code> festgelegt), ist die Verbindung auf der lokalen Unternehmensseite nicht eingerichtet.</li></ul></td>
    </tr>
    <tr>
    <td><code>vpn-strongswan-ping-remote-gw</code></td>
    <td>Überprüfen Sie die öffentliche IP-Adresse <code>remote.gateway</code>, die Sie in der Datei <code>config.yaml</code> konfiguriert haben mit einem Pingsignal, Wenn die VPN-Verbindung den Status <code>ESTABLISHED</code> aufweist, können Sie das Ergebnis dieses Tests ignorieren. Wenn die VPN-Verbindung nicht den Status <code>ESTABLISHED</code> aufweist, kann dieser Test aus den folgenden Gründen fehlschlagen:<ul><li>Sie haben keine IP-Adresse für das lokalen VPN-Gateway im Unternehmen angegeben. Falls für <code>ipsec.auto</code> die Einstellung <code>start</code> festgelegt ist, ist die IP-Adresse <code>remote.gateway</code> erforderlich.</li><li>ICMP-Pakete (Ping) werden von der Firewall geblockt.</li></ul></td>
    </tr>
    <tr>
    <td><code>vpn-strongswan-ping-remote-ip-1</code></td>
    <td>Die private IP-Adresse <code>remote.privateIPtoPing</code> des lokalen VPN-Gateways im Unternehmen wird vom VPN-Pod im Cluster mit einem Pingsignal überprüft. Dieser Test kann aus folgenden Gründen fehlschlagen:<ul><li>Sie haben keine IP-Adresse <code>remote.privateIPtoPing</code> angegeben. Wenn Sie absichtlich keine IP-Adresse angegeben haben, ist dieser Fehler akzeptabel.</li><li>Sie haben das Teilnetz-CIDR für den Cluster-Pod, <code>172.30.0.0/16</code>, nicht in der Liste <code>local.subnet</code> angegeben.</li></ul></td>
    </tr>
    <tr>
    <td><code>vpn-strongswan-ping-remote-ip-2</code></td>
    <td>Die private IP-Adresse <code>remote.privateIPtoPing</code> des lokalen VPN-Gateways im Unternehmen wird vom Workerknoten im Cluster mit einem Pingsignal überprüft. Dieser Test kann aus folgenden Gründen fehlschlagen:<ul><li>Sie haben keine IP-Adresse <code>remote.privateIPtoPing</code> angegeben. Wenn Sie absichtlich keine IP-Adresse angegeben haben, ist dieser Fehler akzeptabel.</li><li>Sie haben das private Teilnetz-CIDR für den Workerknoten im Cluster nicht in der Liste <code>local.subnet</code> angegeben.</li></ul></td>
    </tr>
    </tbody></table>

6. Löschen Sie das Helm-Diagramm.

    ```
    helm delete --purge vpn
    ```
    {: pre}

7. Öffnen Sie die Datei `config.yaml` und korrigieren Sie die falschen Werte.

8. Speichern Sie die aktualisierte Datei `config.yaml`.

9. Installieren Sie das Helm-Diagramm auf Ihrem Cluster mit der aktualisierten Datei `config.yaml`. Die aktualisierten Eigenschaften werden in einer Konfigurationszuordnung für Ihr Diagramm gespeichert.

    ```
    helm install -f config.yaml --name=<releasename> ibm/strongswan
    ```
    {: pre}

10. Prüfen Sie den Status der Diagrammbereitstellung. Sobald das Diagramm bereit ist, hat das Feld **STATUS** oben in der Ausgabe den Wert `DEPLOYED`.

    ```
    helm status vpn
    ```
    {: pre}

11. Sobald das Diagramm bereitgestellt ist, überprüfen Sie, dass die aktualisierten Einstellungen in der Datei `config.yaml` verwendet wurden.

    ```
    helm get values vpn
    ```
    {: pre}

12. Bereinigen Sie die aktuellen Testpods.

    ```
    kubectl get pods -a -l app=strongswan-test
    ```
    {: pre}

    ```
    kubectl delete pods -l app=strongswan-test
    ```
    {: pre}

13. Führen Sie die Tests erneut aus.

    ```
    helm test vpn
    ```
    {: pre}

<br />


## StrongSwan-VPN-Datenverkehr nach Namensbereich oder Workerknoten beschränken
{: #limit}

Wenn Sie über ein Single-Tenant-Cluster verfügen oder wenn Sie über ein Multi-Tenant-Cluster verfügen, in dem Clusterressourcen von den Tenants gemeinsam verwendet werden, können Sie den [VPN-Datenverkehr für die einzelnen StrongSwan-Bereitstellungen auf Pods in bestimmten Namensbereichen beschränken](#limit_namespace). Wenn Sie über ein Multi-Tenant-Cluster verfügen, in dem Clusterressourcen ganz bestimmten Tenants zugeordnet sind, können Sie den [VPN-Datenverkehr für die einzelnen StrongSwan-Bereitstellungen auf die den einzelnen Tenants zugeordneten Workerknoten beschränken](#limit_worker).
{: shortdesc}

### StrongSwan-VPN-Datenverkehr nach Namensbereich
{: #limit_namespace}

Wenn Sie über ein Single-Tenant-Cluster verfügen oder über ein Multi-Tenant-Cluster verfügen, können Sie den VPN-Datenverkehr auf Pods in bestimmten Namensbereichen beschränken.
{: shortdesc}

Nehmen wir beispielsweise an, dass Pods nur in einem bestimmten Namensbereich, `my-secure-namespace`, Daten über das VPN senden und empfangen sollen. Sie möchten nicht, dass Pods in anderen Namensbereichen, z. B. `kube-system`, `ibm-system` oder `default` auf Ihr lokales Netz zugreifen. Um den VPN-Datenverkehr auf `my-secure-namespace` zu beschränken, können Sie globale Calico-Netzrichtlinien erstellen.

Prüfen Sie die folgenden Überlegungen und Einschränkungen, bevor Sie diese Lösung einsetzen.
* Sie müssen das StrongSwan-Helm-Diagramm nicht im angegebenen Namensbereich bereitstellen. Der StrongSwan-VPN-Pod und die Routen-Dämongruppe können im Namensbereich `kube-system` oder in einem beliebigen anderen Namensbereich bereitgestellt werden. Wenn das StrongSwan-VPN nicht im angegebenen Namensbereich bereitgestellt wird, schlägt der Helm-Test für `vpn-strongswan-ping-remote-ip-1` fehl. Dieser Fehler wird erwartet und ist zulässig. Der Test setzt ein Pingsignal an die private IP-Adresse `remote.privateIPtoPing` des lokalen VPN-Gateways von einem VPN-Pod ab, der sich nicht in dem Namensbereich mit direktem Zugriff auf das ferne Teilnetz befindet. Der VPN-Pod kann jedoch weiterhin Datenverkehr an Pods in den Namensbereichen weiterleiten, die über Routen zu dem fernen Teilnetz verfügen, und der Datenverkehr kann weiter ordnungsgemäß fließen. Der VPN-Status lautet immer noch `ESTABLISHED` und die Pods im angegebenen Namensbereich können über das VPN eine Verbindung herstellen.

* Die globalen Netzrichtlinien von Calico in den folgenden Schritten verhindern nicht, dass Kubernetes-Pods, die Hostnetzbetrieb verwenden, Daten über das VPN senden und empfangen. Wenn ein Pod mit Hostnetzbetrieb konfiguriert wird, kann die in dem Pod ausgeführte App an den Netzschnittstellen des Workerknotens, auf dem sie sich befindet, empfangsbereit sein. Diese Hostnetzbetrieb-Pods können in allen Namensbereichen vorhanden sein. Um zu bestimmen, welche Pods Hostnetzbetrieb haben, führen Sie `kubectl get pods --all-namespaces -o wide` aus und suchen Sie nach Pods, die nicht die Pod-IP-Adresse `172.30.0.0/16` aufweisen. Wenn Sie ausschließen möchten, dass Hostnetzbetrieb-Pods Daten über das VPN senden und empfangen, können Sie die folgenden Optionen in Ihrer Bereitstellungsdatei `values.yaml` festlegen: `local.subnet: 172.30.0.0/16` und `enablePodSNAT: false`. Diese Konfigurationseinstellungen machen alle Kubernetes-Pods über die VPN-Verbindung zum fernen Netz zugänglich. Es sind jedoch nur die Pods über das VPN erreichbar, die sich im angegebenen sicheren Namensbereich befinden.

Vorbereitende Schritte:
* [Stellen Sie das StrongSwan-Helm-Diagramm bereit](#vpn_configure) und [stellen Sie sicher, dass die VPN-Konnektivität ordnungsgemäß funktioniert](#vpn_test).
* [Installieren und konfigurieren Sie die Calico-CLI](/docs/containers?topic=containers-network_policies#cli_install).

Gehen Sie wie folgt vor, um den VPN-Datenverkehr auf einen bestimmten Namensbereich zu beschränken:

1. Erstellen Sie eine globale Calico-Netzrichtlinie namens `allow-non-vpn-outbound.yaml`. Diese Richtlinie ermöglicht es allen Namensbereichen, weiterhin ausgehenden Datenverkehr an alle Ziele zu senden, mit Ausnahme des fernen Teilnetzes, auf das das StrongSwan-VPN zugreift. Ersetzen Sie `<remote.subnet>` durch das fernes Teilnetz `remote.subnet`, das Sie in der Helm-Konfigurationsdatei `values.yaml` angegeben haben. Informationen zum Angeben mehrerer ferner Teilnetze finden Sie in der [Calico-Dokumentation ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.projectcalico.org/v3.3/reference/calicoctl/resources/globalnetworkpolicy).
    ```yaml
    apiVersion: projectcalico.org/v3
    kind: GlobalNetworkPolicy
    metadata:
      name: allow-non-vpn-outbound
    spec:
      selector: has(projectcalico.org/namespace)
      egress:
      - action: Allow
        destination:
          notNets:
          - <remote.subnet>
      order: 900
      types:
      - Egress
    ```
    {: codeblock}

2. Wenden Sie die Richtlinie an.

    ```
    calicoctl apply -f allow-non-vpn-outbound.yaml --config=filepath/calicoctl.cfg
    ```
    {: pre}

3. Erstellen Sie eine weitere globale Calico-Netzrichtlinie namens `allow-vpn-from-namespace.yaml`. Diese Richtlinie ermöglicht es nur einem bestimmten Namensbereich, ausgehenden Datenverkehr an das ferne Teilnetz zu senden, auf das das StrongSwan-VPN zugreift. Ersetzen Sie `<namespace>` durch den Namensbereich, der auf das VPN zugreifen kann, und `<remote.subnet>` durch das ferne Teilnetz `remote.subnet`, das Sie in der Helm-Konfigurationsdatei `values.yaml` angegeben haben. Informationen zum Angeben mehrerer Namensbereiche oder ferner Teilnetze finden Sie in der [Calico-Dokumentation ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.projectcalico.org/v3.3/reference/calicoctl/resources/globalnetworkpolicy).
    ```yaml
    apiVersion: projectcalico.org/v3
    kind: GlobalNetworkPolicy
    metadata:
      name: allow-vpn-from-namespace
    spec:
      selector: projectcalico.org/namespace == "<namensbereich>"
      egress:
      - action: Allow
        destination:
          nets:
          - <remote.subnet>
      order: 900
      types:
      - Egress
    ```
    {: codeblock}

4. Wenden Sie die Richtlinie an.

    ```
    calicoctl apply -f allow-vpn-from-namespace.yaml --config=filepath/calicoctl.cfg
    ```
    {: pre}

5. Verifizieren Sie, dass die globalen Netzrichtlinien in Ihrem Cluster erstellt werden.
    ```
    calicoctl get GlobalNetworkPolicy -o wide --config=filepath/calicoctl.cfg
    ```
    {: pre}

### StrongSwan-VPN-Datenverkehr nach Workerknoten beschränken
{: #limit_worker}

Wenn Sie über mehrere StrongSwan-VPN-Bereitstellungen in einem Multi-Tenant-Cluster verfügen, können Sie den VPN-Datenverkehr für jede Bereitstellung auf bestimmte Workerknoten beschränken, die den einzelnen Tenants zugeordnet sind.
{: shortdesc}

Wenn Sie ein StrongSwan-Helm-Diagramm bereitstellen, wird eine StrongSwan-VPN-Bereitstellung erstellt. Die StrongSwan-VPN-Pods werden auf allen Workerknoten ohne Taints bereitgestellt. Außerdem wird eine Kubernetes-Dämongruppe erstellt. Diese Dämongruppe konfiguriert automatisch Routen auf allen Workerknoten ohne Taint in dem Cluster zu den jeweiligen fernen Teilnetzen. Der StrongSwan-VPN-Pod verwendet die Routen auf Workerknoten, um Anforderungen an das ferne Teilnetz in dem lokalen Netz weiterzuleiten.

Routen werden nicht für Knoten mit Taint konfiguriert, es sei denn, Sie geben den Taint in der Einstellung für Tolerierungen (`tolerations`) in der Datei `value.yaml` ein. Indem Sie Workerknoten mit Taints versehen, können Sie verhindern, dass auf diesen Workern VPN-Routen konfiguriert werden. Anschließend können Sie den Taint in der Einstellung für Tolerierungen (`tolerations`) nur für die VPN-Bereitstellung angeben, die Sie auf den Workern mit Taint zulassen möchten. Auf diese Weise verwenden die StrongSwan-VPN-Pods für die Bereitstellung des Helm-Diagramms eines Tenants nur die Routen auf den Workerknoten dieses Tenants, um Datenverkehr über die VPN-Verbindung an das ferne Teilnetz weiterzuleiten.

Prüfen Sie die folgenden Überlegungen und Einschränkungen, bevor Sie diese Lösung einsetzen.
* Standardmäßig platziert Kubernetes App-Pods auf allen verfügbaren Workerknoten ohne Taints. Um sicherzustellen, dass diese Lösung ordnungsgemäß funktioniert, muss jeder Tenant zunächst sicherstellen, dass ihre App-Pods nur auf Workern bereitgestellt werden, die mit einem Taint für den richtigen Tenant versehen sind. Außerdem muss jeder Workerknoten mit Taint auch über eine Tolerierung verfügen, damit die App-Pods auf dem Knoten platziert werden können. Weitere Informationen zu Taints und Tolerierungen finden Sie in der [Kubernetes-Dokumentation ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/).
* Clusterressourcen werden möglicherweise nicht optimal genutzt, weil keiner der Tenants App-Pods auf den gemeinsam genutzten Knoten ohne Taints platzieren kann.

In den folgenden Schritten zum Einschränken des StrongSwan-VPN-Datenverkehrs nach Workerknoten wird dieses Beispielszenario verwendet: Angenommen, Sie verfügen über einen Multi-Tenant-Cluster von {{site.data.keyword.containerlong_notm}} mit sechs Workerknoten. Der Cluster unterstützt Tenant A und Tenant B. Sie können die Workerknoten auf folgende Weise mit Taints versehen:
* Zwei Workerknoten haben Taints, sodass nur Tenant A-Pods auf den Workern geplant sind.
* Zwei Workerknoten haben Taints, sodass nur Tenant B-Pods auf den Workern geplant sind.
* Zwei Workerknoten haben keine Taints, weil mindestens zwei Workerknoten erforderlich sind, damit die StrongSwan-VPN-Pods und die IP der Lastausgleichsfunktion ausgeführt werden können.

Gehen Sie wie folgt vor, um den VPN-Datenverkehr auf Knoten mit Taints für jeden Tenant zu beschränken:

1. Um den VPN-Datenverkehr nur auf Worker zu beschränken, die in diesem Beispiel Tenant A zugeordnet sind, geben Sie die folgende Tolerierung (`toleration`) in der Datei `values.yaml` für das StrongSwan-Helm-Diagramm des Tenants A an:
    ```
    tolerations:
     - key: dedicated
       operator: "Equal"
       value: "tenantA"
       effect: "NoSchedule"
    ```
    {: codeblock}
    Diese Tolerierung lässt zu, dass die Routen-Dämongruppe auf den beiden Workerknoten ausgeführt wird, die mit dem Taint `dedicated="tenantA"` versehen sind, ebenso wie auf den Workerknoten ohne Taints. Die StrongSwan-VPN-Pods für diese Bereitstellung werden auf den beiden Workerknoten ohne Taints ausgeführt.

2. Um den VPN-Datenverkehr nur auf Worker zu beschränken, die in diesem Beispiel Tenant B zugeordnet sind, geben Sie die folgende Tolerierung (`toleration`) in der Datei `values.yaml` für das StrongSwan-Helm-Diagramm des Tenants B an:
    ```
    tolerations:
     - key: dedicated
       operator: "Equal"
       value: "tenantB"
       effect: "NoSchedule"
    ```
    {: codeblock}
    Diese Tolerierung lässt zu, dass die Routen-Dämongruppe auf den beiden Workerknoten ausgeführt wird, die mit dem Taint `dedicated="tenantB"` versehen sind, ebenso wie auf den Workerknoten ohne Taints. Die StrongSwan-VPN-Pods für diese Bereitstellung werden auch auf den beiden Workerknoten ohne Taints ausgeführt.

<br />


## Aktualisieren Sie das StrongSwan-Helm-Diagramm
{: #vpn_upgrade}

Stellen Sie sicher, dass Ihr StrongSwan-Helm-Diagramm aktuell ist, indem Sie es aktualisieren.
{:shortdesc}

Gehen Sie wie folge vor, um das StrongSwan-Helm-Diagramm auf die neueste Version zu aktualisieren.

  ```
  helm upgrade -f config.yaml <releasename> ibm/strongswan
  ```
  {: pre}

Das strongSwan 2.0.0-Helm-Diagramm ist nicht mit Calico Version 3 oder Kubernetes Version 1.10 kompatibel. Bevor Sie [Ihr Cluster auf 1.10 aktualisieren](/docs/containers?topic=containers-cs_versions#cs_v110), aktualisieren Sie zunächst StrongSwan auf das Helm-Diagramm der Version 2.2.0 oder höher, das abwärtskompatibel zu Calico 2.6 und Kubernetes 1.9 ist. Als Nächstes löschen Sie Ihr StrongSwan-Helm-Diagramm. Nach der Aktualisierung können Sie das Diagramm dann erneut installieren.
{:tip}

## StrongSwan-IPSec-VPN-Service inaktivieren
{: vpn_disable}

Sie können die VPN-Verbindung inaktivieren, indem Sie das Helm-Diagramm löschen .
{:shortdesc}

  ```
  helm delete --purge <releasename>
  ```
  {: pre}

<br />


## Virtual Router Appliance verwenden
{: #vyatta}

[Virtual Router Appliance (VRA)](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-about-the-vra) stellt das aktuelle Vyatta 5600-Betriebssystem für x86-Bare-Metal-Server bereit. Sie können eine VRA-Instanz als VPN-Gateway verwenden, um eine sichere Verbindung zu einem lokalen Netz herzustellen.
{:shortdesc}

Der gesamte öffentliche und private Netzverkehr, der in die Cluster-VLANs eintritt oder sie verlässt, wird über eine VRA geleitet. Sie können die VRA als VPN-Endpunkt einsetzen, um einen verschlüsselten IPSec-Tunnel zwischen Servern in IBM Cloud Infrastructure- (SoftLayer) und lokalen Ressourcen zu erstellen. Das folgende Diagramm zeigt beispielsweise, wie eine App auf einem ausschließlich privaten Workerknoten in {{site.data.keyword.containerlong_notm}} mit einem lokalen Server über eine VRA-VPN-Verbindung kommunizieren kann:

<img src="images/cs_vpn_vyatta.png" width="725" alt="App in {{site.data.keyword.containerlong_notm}} mithilfe einer Lastausgleichsfunktion zugänglich machen" style="width:725px; border-style: none"/>

1. Eine App in Ihrem Cluster, `myapp2`, empfängt eine Anforderung von einem Ingress- oder LoadBalancer-Service und muss eine sichere Verbindung mit Daten in Ihrem lokalen Netz herstellen.

2. Da `myapp2` sich auf einem Workerknoten in einem rein privaten VLAN befindet, fungiert die VRA als sichere Verbindung zwischen den Workerknoten und dem lokalen Netz. Die VRA verwendet die IP-Zieladresse, um zu bestimmen, welche Netzpakete an das lokale Netz gesendet werden sollen.

3. Die Anforderung wird verschlüsselt und über den VPN-Tunnel an das lokale Rechenzentrum gesendet.

4. Die eingehende Anforderung passiert die lokale Firewall und wird an den VPN-Tunnelendpunkt (Router) zugestellt, wo sie entschlüsselt wird.

5. Der VPN-Tunnelendpunkt (Router) leitet die Anforderung abhängig von der in Schritt 2 angegebenen Ziel-IP-Adresse an den lokalen Server oder den Mainframe weiter. Die erforderlichen Daten werden auf dieselbe Weise über die VPN-Verbindung zurück an `myapp2` gesendet.

Führen Sie die folgenden Schritte aus, um eine Virtual Router Appliance einzurichten:

1. [Bestellen Sie eine Virtual Router Appliance (VRA)](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-getting-started-with-ibm-virtual-router-appliance).

2. [Konfigurieren Sie das private VLAN auf der VRA](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-managing-your-vlans).

3. Um eine VPN-Verbindung mithilfe der VRA zu aktivieren, [konfigurieren Sie VRRP auf der VRA](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-working-with-high-availability-and-vrrp#high-availability-vpn-with-vrrp).

Wenn Sie über eine vorhandene Router Appliance verfügen und dann einen Cluster hinzufügen, werden die neuen portierbaren Teilnetze, die für den Cluster bestellt sind, nicht in der Router Appliance konfiguriert. Um Netzservices verwenden zu können, müssen Sie die Weiterleitung zwischen Teilnetzen im selben VLAN aktivieren, indem Sie [VLAN-Spanning aktivieren](/docs/containers?topic=containers-subnets#subnet-routing). Zum Prüfen, ob das VLAN-Spanning bereits aktiviert ist, verwenden Sie den [Befehl](/docs/containers?topic=containers-cs_cli_reference#cs_vlan_spanning_get) `ibmcloud ks vlan-spanning-get`.
{: important}
