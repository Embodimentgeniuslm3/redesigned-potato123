---

copyright:
  years: 2014, 2019
lastupdated: "2019-04-18"

keywords: kubernetes, iks, lb2.0, nlb, health check

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



# Basis- und DSR-Lastausgleich mit Netzlastausgleichsfunktionen (NLB)
{: #loadbalancer}

Machen Sie einen Port zugänglich und verwenden Sie eine portierbare IP-Adresse für die Layer 4-Netzlastausgleichsfunktion (NLB), um auf eine containerisierte App zuzugreifen.
{:shortdesc}

Wählen Sie für den Einstieg eine der folgenden Optionen aus:

<img src="images/cs_loadbalancer_imagemap.png" usemap="#image-map" alt="Diese Imagemap stellt Quick Links zu Konfigurationsabschnitten auf dieser Seite bereit.">
<map name="image-map">
    <area target="" alt="Übersicht" title="Übersicht" href="#lb_overview" coords="35,44,175,72" shape="rect">
    <area target="" alt="Vergleich der Lastausgleichsfunktionen der Version 1.0 und der Version 2.0" title="Vergleich der Lastausgleichsfunktionen der Version 1.0 und der Version 2.0" href="#comparison" coords="34,83,173,108" shape="rect">
    <area target="" alt="Hostnamen für Lastausgleichsfunktion registrieren" title="Hostnamen für Lastausgleichsfunktion registrieren" href="#loadbalancer_hostname" coords="33,119,174,146" shape="rect">
    <area target="" alt="v2.0: Komponenten und Architektur (Beta)" title="v2.0: Komponenten und Architektur (Beta)" href="#planning_ipvs" coords="273,45,420,72" shape="rect">
    <area target="" alt="v2.0: Voraussetzungen" title="v2.0: Voraussetzungen" href="#ipvs_provision" coords="277,85,417,108" shape="rect">
    <area target="" alt="v2.0: Lastausgleichsfunktion der Version 2.0 in einem Mehrzonencluster konfigurieren" title="v2.0: Lastausgleichsfunktion der Version 2.0 in einem Mehrzonencluster konfigurieren" href="#ipvs_multi_zone_config" coords="276,122,417,147" shape="rect">
    <area target="" alt="v2.0: Lastausgleichsfunktion der Version 2.0 in einem Einzelzonencluster konfigurieren" title="v2.0: Lastausgleichsfunktion der Version 2.0 in einem Einzelzonencluster konfigurieren" href="#ipvs_single_zone_config" coords="277,156,419,184" shape="rect">
    <area target="" alt="v2.0: Planungsalgorithmen" title="v2.0: Planungsalgorithmen" href="#scheduling" coords="276,196,419,220" shape="rect">
    <area target="" alt="v1.0: Komponenten und Architektur" title="v1.0: Komponenten und Architektur" href="#v1_planning" coords="519,47,668,74" shape="rect">
    <area target="" alt="v1.0: Lastausgleichsfunktion der Version 1.0 in einem Mehrzonencluster konfigurieren" title="v1.0: Lastausgleichsfunktion der Version 1.0 in einem Mehrzonencluster konfigurieren" href="#multi_zone_config" coords="520,85,667,110" shape="rect">
    <area target="" alt="v1.0: Lastausgleichsfunktion der Version 1.0 in einem Einzelzonencluster konfigurieren" title="v1.0: Lastausgleichsfunktion der Version 1.0 in einem Einzelzonencluster konfigurieren" href="#lb_config" coords="520,122,667,146" shape="rect">
    <area target="" alt="v1.0: Beibehaltung von Quellen-IP-Adressen aktivieren" title="v1.0: Beibehaltung von Quellen-IP-Adressen aktivieren" href="#node_affinity_tolerations" coords="519,157,667,194" shape="rect">
</map>
</br>

Für einen schnellen Einstieg können Sie den folgenden Befehl ausführen, mit dem eine Lastausgleichsfunktion 1.0 erstellt wird:
```
kubectl expose deploy my-app --port=80 --target-port=8080 --type=LoadBalancer --name my-lb-svc
```
{: pre}

## Übersicht
{: #lb_overview}

Wenn Sie einen Standardcluster erstellen, stellt {{site.data.keyword.containerlong}} automatisch ein portierbares öffentliches Teilnetz und ein portierbares privates Teilnetz bereit.
{: shortdesc}

* Das portierbare öffentliche Teilnetz stellt fünf verwendbare IP-Adressen bereit. Eine portierbare öffentliche IP-Adresse wird von der [öffentlichen Ingress-Standard-ALB](/docs/containers?topic=containers-ingress) verwendet. Die verbleibenden vier portierbaren öffentlichen IP-Adressen können verwendet werden, um einzelne Apps dem Internet zugänglich zu machen, indem öffentliche Netzausgleichsfunktions- (NLB-) Services erstellt werden.
* Das portierbare private Teilnetz stellt fünf verwendbare IP-Adressen bereit. Eine portierbare private IP-Adresse wird von der [privaten Ingress-Standard-ALB](/docs/containers?topic=containers-ingress#private_ingress) verwendet. Die verbleibenden vier portierbaren privaten IP-Adressen können verwendet werden, um einzelne Apps einem privaten Netz zugänglich zu machen, indem private Services für die Lastausgleichsfunktion erstellt werden.

Portierbare öffentliche und private IP-Adressen sind statische variable IPs und ändern sich nicht, wenn ein Workerknoten entfernt wird. Wenn der Workerknoten, auf dem sich die NLB-IP-Adresse befindet, entfernt wird, wird die IP-Adresse von einem Keepalive-Dämon, der die IP kontinuierlich überwacht, automatisch in einen anderen Workerknoten verschoben. Sie können Ihrer NLB jeden beliebigen Port zuweisen. Die NLB fungiert als externer Einstiegspunkt für eingehende Anforderungen an die App. Wenn Sie vom Internet aus auf die NLB zugreifen möchten, können Sie die öffentliche IP-Adresse der NLB in Verbindung mit der zugewiesenen Portnummer im Format `<ip-adresse>:<port>` verwenden. Sie können DNS-Einträge für NLBs auch erstellen, indem Sie die NLB-IP-Adressen mit Hostnamen registrieren.

Wenn Sie eine App mithilfe eines NLB-Service zugänglich machen, wird Ihre App automatisch auch über die NodePorts des Service bereitgestellt. Auf [Knotenports (NodePorts)](/docs/containers?topic=containers-nodeport) kann über jede öffentliche und private IP-Adresse jedes Workerknotens innerhalb des Clusters zugegriffen werden. Informationen zum Blockieren des Datenverkehrs an Knotenports während der Verwendung einer NLB finden Sie im Abschnitt [Eingehenden Datenverkehr an Netzlastausgleichsfunktion (NLB) oder NodePort-Services steuern](/docs/containers?topic=containers-network_policies#block_ingress).

<br />


## Vergleich der NLBs der Versionen 1.0 und 2.0
{: #comparison}

Wenn Sie eine NLB erstellen, haben Sie die Wahl zwischen Version 1.0 und Version 2.0. Beachten Sie, dass die Version 2.0 der NLBs eine Betaversion ist.
{: shortdesc}

**Was haben die NLBs der Versionen 1.0 und 2.0 gemeinsam?**

Die NLBs der Versionen 1.0 und 2.0 sind beides Layer 4-Lastausgleichsfunktionen, die nur im Linux-Kernelbereich aktiv sein können. Beide Versionen werden innerhalb des Clusters ausgeführt und verwenden Ressourcen von Workerknoten. Deshalb ist die verfügbare Kapazität der NLBs immer Ihrem eigenen Cluster zugeordnet. Darüber hinaus beenden beide Versionen von NLBs nicht die Verbindung. Stattdessen leiten sie Verbindungen an einen App-Pod weiter.

**Worin unterscheiden sich NLBs der Versionen 1.0 und 2.0?**

Wenn ein Client eine Anforderung an Ihre App sendet, leitet die NLB Anforderungspakete an die IP-Adresse des Workerknotens weiter, auf dem ein App-Pod vorhanden ist. NLBs der Version 1.0 verwenden NAT (Network Address Translation, Netzadressumsetzung), um die Quellen-IP-Adresse des Anforderungspakets in die IP des Workerknotens umzuschreiben, auf dem ein Lastausgleichsfunktions-Pod vorhanden ist. Wenn der Workerknoten das App-Antwortpaket zurückgibt, wird die IP des Workerknotens, auf dem die NLB vorhanden ist, verwendet. Die NLB muss dann das Antwortpaket an den Client senden. Um zu verhindern, dass die IP-Adresse neu geschrieben wird, können Sie die [Beibehaltung der Quellen-IP aktivieren](#node_affinity_tolerations). Die Beibehaltung der Quellen-IP erfordert jedoch, dass Lastausgleichsfunktions-Pods und App-Pods auf demselben Worker ausgeführt werden, sodass die Anforderung nicht an einen anderen Worker weitergeleitet werden muss. Sie müssen Knotenaffinität und -tolerierungen zu App-Pods hinzufügen.

Im Gegensatz zu den NLBs der Version 1.0 verwenden die NLBs der Version 2.0 NAT nicht, wenn Anforderungen an App-Pods an andere Worker weitergeleitet werden. Wenn eine NLB der Version 2.0 eine Clientanforderung weiterleitet, verwendet sie IP über IP (IPIP), um das ursprüngliche Anforderungspaket in ein anderes, neues Paket einzubinden. Dieses einbindende IPIP-Paket hat eine Quellen-IP des Workerknotens, auf dem sich der Lastausgleichsfunktions-Pod befindet. Dadurch kann das ursprüngliche Anforderungspaket die Client-IP-Adresse als Quellen-IP-Adresse beibehalten. Der Workerknoten verwendet dann DSR (Direct Server Return), um das App-Antwortpaket an die Client-IP zu senden. Das Antwortpaket überspringt die NLB und wird direkt an den Client gesendet, wodurch der Umfang des Datenverkehrs reduziert wird, den die NLB verarbeiten muss.

<br />


## v1.0: Komponenten und Architektur
{: #v1_planning}

Die TCP/UDP-Netzlastausgleichsfunktion (NLB) der Version 1.0 verwendet 'Iptables', ein Linux-Kernel-Feature, für den Lastausgleich von Anforderungen in den Pods einer App.
{: shortdesc}

### Datenfluss in einem Einzelzonencluster
{: #v1_single}

Das folgende Diagramm veranschaulicht, wie eine NLB der Version 1.0 die Kommunikation vom Internet an eine App in einem Einzelzonencluster leitet.
{: shortdesc}

<img src="images/cs_loadbalancer_planning.png" width="410" alt="App in {{site.data.keyword.containerlong_notm}} über eine NLB 1.0 zugänglich machen" style="width:410px; border-style: none"/>

1. Eine Anforderung an Ihre App verwendet die öffentliche IP-Adresse der NLB und den zugeordneten Port auf dem Workerknoten.

2. Die Anforderung wird automatisch an die interne Cluster-IP-Adresse und den internen Port der NLB weitergeleitet. Auf die interne Cluster-IP-Adresse kann nur aus dem Cluster selbst heraus zugegriffen werden.

3. `kube-proxy` leitet die Anforderung an den NLB-Service für die App weiter.

4. Die Anforderung wird an die private IP-Adresse des App-Pods weitergeleitet. Die Quellen-IP-Adresse des Anforderungspakets wird in die öffentliche IP-Adresse des Workerknotens geändert, auf dem der App-Pod ausgeführt wird. Wenn mehrere App-Instanzen im Cluster bereitgestellt werden, leitet die NLB die Anforderungen zwischen den App-Pods weiter.

### Datenfluss in einem Mehrzonencluster
{: #v1_multi}

Das folgende Diagramm veranschaulicht, wie eine Netzlastausgleichsfunktion (NLB) der Version 1.0 die Kommunikation vom Internet an eine App in einem Mehrzonencluster leitet.
{: shortdesc}

<img src="images/cs_loadbalancer_planning_multizone.png" width="500" alt="NLB 1.0 für den Lastausgleich von Apps in Mehrzonenclustern verwenden" style="width:500px; border-style: none"/>

Standardmäßig werden NLBs der Version 1.0 nur in einer Zone konfiguriert. Um eine hohe Verfügbarkeit zu erreichen, müssen Sie eine NLB der Version 1.0 in jeder Zone bereitstellen, in der sich App-Instanzen befinden. Anforderungen werden von den NLBs in verschiedenen Zonen in einem Umlaufzyklus bearbeitet. Darüber hinaus leitet jede NLB Anforderungen an die App-Instanzen in ihrer eigenen Zone und an App-Instanzen in anderen Zonen weiter.

<br />


## V1.0: NLB der Version 1.0 in einem Mehrzonencluster konfigurieren
{: #multi_zone_config}

**Vorbereitende Schritte**:
* Um öffentliche Netzlastausgleichsfunktionen in mehreren Zonen zu erstellen, muss mindestens ein öffentliches VLAN portierbare Teilnetze aufweisen, die in jeder Zone verfügbar sind. Um private NLBs in mehreren Zonen zu erstellen, muss mindestens ein privates VLAN portierbare Teilnetze aufweisen, die in jeder Zone verfügbar sind. Sie können Teilnetze hinzufügen, indem Sie die Schritte im Abschnitt [Teilnetze für Cluster konfigurieren](/docs/containers?topic=containers-subnets) ausführen.
* Wenn Sie den Datenaustausch im Netz auf Edge-Workerknoten beschränken, müssen Sie sicherstellen, dass in jeder Zone mindestens zwei [Edge-Workerknoten](/docs/containers?topic=containers-edge#edge) aktiviert sind, sodass NLBs gleichmäßig bereitgestellt werden können.
* Aktivieren Sie [VLAN-Spanning](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning) für Ihr IBM Cloud-Infrastrukturkonto (SoftLayer), damit die Workerknoten in dem privaten Netz miteinander kommunizieren können. Um diese Aktion durchführen zu können, müssen Sie über die [Infrastrukturberechtigung](/docs/containers?topic=containers-users#infra_access) **Netz > VLAN-Spanning im Netz verwalten** verfügen oder Sie können den Kontoeigner bitten, diese zu aktivieren. Zum Prüfen, ob das VLAN-Spanning bereits aktiviert ist, verwenden Sie den [Befehl](/docs/containers?topic=containers-cs_cli_reference#cs_vlan_spanning_get) `ibmcloud ks vlan-spanning-get`.
* Stellen Sie sicher, dass Sie die [{{site.data.keyword.Bluemix_notm}} IAM-Servicerolle **Schreibberechtigter** oder **Manager**](/docs/containers?topic=containers-users#platform) für den Namensbereich `default` innehaben.


Gehen Sie wie folgt vor, um eine NLB der Version 1.0 in einem Mehrzonencluster einzurichten:
1.  [Stellen Sie die App für den Cluster bereit](/docs/containers?topic=containers-app#app_cli). Stellen Sie sicher, dass Sie zur Bereitstellung im Metadatenabschnitt der Konfigurationsdatei eine Bezeichnung hinzufügen. Diese Bezeichnung ist zur Identifizierung aller Pods erforderlich, in denen Ihre App ausgeführt wird, damit sie in den Lastenausgleich aufgenommen werden können.

2.  Erstellen Sie einen Lastausgleichsservice für die App, die Sie über das öffentliche Internet oder ein privates Netz zugänglich machen wollen.
  1. Erstellen Sie eine Servicekonfigurationsdatei namens `myloadbalancer.yaml` (Beispiel).
  2. Definieren Sie einen Lastausgleichsservice für die App, die Sie zugänglich machen möchten. Sie können eine Zone, ein VLAN und eine IP-Adresse angeben.

      ```
      apiVersion: v1
      kind: Service
      metadata:
        name: myloadbalancer
        annotations:
          service.kubernetes.io/ibm-load-balancer-cloud-provider-ip-type: <public_oder_private>
          service.kubernetes.io/ibm-load-balancer-cloud-provider-zone: "<zone>"
          service.kubernetes.io/ibm-load-balancer-cloud-provider-vlan: "<vlan-id>"
      spec:
        type: LoadBalancer
        selector:
          <selektorschlüssel>: <selektorwert>
        ports:
         - protocol: TCP
             port: 8080
          loadBalancerIP: <ip-adresse>
      ```
      {: codeblock}

      <table>
      <caption>Erklärung der Komponenten der YAML-Datei</caption>
      <thead>
      <th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der YAML-Dateikomponenten</th>
      </thead>
      <tbody>
      <tr>
        <td><code>service.kubernetes.io/ibm-load-balancer-cloud-provider-ip-type:</code>
        <td>Annotation zum Angeben des Typs einer Lastausgleichsfunktion: <code>private</code> oder <code>public</code>.</td>
      </tr>
      <tr>
        <td><code>service.kubernetes.io/ibm-load-balancer-cloud-provider-zone:</code>
        <td>Annotation zum Angeben der Zone, in der der Lastausgleichsservice bereitgestellt wird. Um Zonen anzuzeigen, führen Sie den Befehl <code>ibmcloud ks zones</code> aus.</td>
      </tr>
      <tr>
        <td>`service.kubernetes.io/ibm-load-balancer-cloud-provider-vlan:`
        <td>Annotation zum Angeben eines VLAN, in dem der Lastausgleichsservice bereitgestellt wird. Um VLANs anzuzeigen, führen Sie <code>ibmcloud ks vlans --zone <zone></code> aus.</td>
      </tr>
      <tr>
        <td><code>selector</code></td>
        <td>Der Bezeichnungsschlüssel (<em>&lt;selektorschlüssel&gt;</em>) und der Wert (<em>&lt;selektorwert&gt;</em>), den Sie im Abschnitt <code>spec.template.metadata.labels</code> der YAML-Bereitstellungsdatei für Ihre App verwendet haben.</td>
      </tr>
      <tr>
        <td><code>port</code></td>
        <td>Der Port, den der Service überwacht.</td>
      </tr>
      <tr>
        <td><code>loadBalancerIP</code></td>
        <td>Optional: Um eine private Lastausgleichsfunktion zu erstellen oder um eine bestimmte portierbare IP-Adresse für eine öffentliche Lastausgleichsfunktion zu verwenden, geben Sie die IP-Adresse an, die Sie verwenden wollen. Die IP-Adresse muss sich in dem VLAN und in der Zone befinden, die Sie in den Annotationen angeben. Wenn Sie keine IP-Adresse angeben, geschieht Folgendes:<ul><li>Wenn sich Ihr Cluster in einem öffentlichen VLAN befindet, dann wird eine portierbare öffentliche IP-Adresse verwendet. Die meisten Cluster befinden sich in einem öffentlichen VLAN.</li><li>Wenn sich Ihr Cluster nur in einem privaten VLAN befindet, wird eine portierbare private IP-Adresse verwendet.</li></ul></td>
      </tr>
      </tbody></table>

      Beispielkonfigurationsdatei zum Erstellen eines privaten NLB-Service der Version 1.0, der eine angegebene IP-Adresse im privaten VLAN `2234945` in `dal12` verwendet:

      ```
      apiVersion: v1
      kind: Service
      metadata:
        name: myloadbalancer
        annotations:
          service.kubernetes.io/ibm-load-balancer-cloud-provider-ip-type: private
          service.kubernetes.io/ibm-load-balancer-cloud-provider-zone: "dal12"
          service.kubernetes.io/ibm-load-balancer-cloud-provider-vlan: "2234945"
      spec:
        type: LoadBalancer
        selector:
          app: nginx
        ports:
         - protocol: TCP
           port: 8080
        loadBalancerIP: 172.21.xxx.xxx
      ```
      {: codeblock}

  3. Optional: Stellen Sie sicher, dass Ihre NLB nur für einen begrenzten Bereich von IP-Adressen verfügbar ist, indem Sie die IP-Adressen im Feld `spec.loadBalancerSourceRanges` angeben. `loadBalancerSourceRanges` wird von `kube-proxy` in Ihrem Cluster durch Iptables-Regeln auf Workerknoten implementiert. Weitere Informationen enthält die [Kubernetes-Dokumentation ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/tasks/access-application-cluster/configure-cloud-provider-firewall/).

  4. Erstellen Sie den Service in Ihrem Cluster.

      ```
      kubectl apply -f myloadbalancer.yaml
      ```
      {: pre}

3. Stellen Sie sicher, dass der NLB-Service erfolgreich erstellt wurde. Es kann ein paar Minuten dauern, bis der Service erstellt wurde und die App verfügbar ist.

    ```
    kubectl describe service myloadbalancer
    ```
    {: pre}

    CLI-Beispielausgabe:

    ```
    Name:                   myloadbalancer
    Namespace:              default
    Labels:                 <none>
    Selector:               app=liberty
    Type:                   LoadBalancer
    Zone:                   dal10
    IP:                     172.21.xxx.xxx
    LoadBalancer Ingress:   169.xx.xxx.xxx
    Port:                   <unset> 8080/TCP
    NodePort:               <unset> 32040/TCP
    Endpoints:              172.30.xxx.xxx:8080
    Session Affinity:       None
    Events:
      FirstSeen	LastSeen	Count	From			SubObjectPath	Type	 Reason			          Message
      ---------	--------	-----	----			-------------	----	 ------			          -------
      10s		    10s		    1	    {service-controller }	  Normal CreatingLoadBalancer	Creating load balancer
      10s		    10s		    1	    {service-controller }		Normal CreatedLoadBalancer	Created load balancer
    ```
    {: screen}

    Die IP-Adresse für **LoadBalancer Ingress** ist die portierbare IP-Adresse, die dem NLB-Service zugewiesen wurde.

4.  Wenn Sie eine öffentliche NLB erstellt haben, dann greifen Sie über das Internet auf Ihre App zu.
    1.  Öffnen Sie Ihren bevorzugten Web-Browser.
    2.  Geben Sie die portierbare öffentliche IP-Adresse der NLB und des Ports ein.

        ```
        http://169.xx.xxx.xxx:8080
        ```
        {: codeblock}    

5. Wiederholen Sie die Schritte 2 bis 4, um eine NLB der Version 1.0 in allen Zonen bereitzustellen.    

6. Wenn Sie die [Beibehaltung der Quellen-IP für eine NLB der Version NLB 1.0](#node_affinity_tolerations) wählen, stellen Sie sicher, dass App-Pods auf den Edge-Workerknoten geplant werden, indem Sie [Edge-Knoten-Affinität zu App-Pods hinzufügen](#lb_edge_nodes). App-Pods müssen auf Edge-Knoten geplant werden, um eingehende Anforderungen empfangen zu können.

7. Optional: Ein Lastausgleichsservice macht Ihre App auch über die NodePort-Instanzen des Service verfügbar. Auf [Knotenports (NodePorts)](/docs/containers?topic=containers-nodeport) kann über jede öffentliche und private IP-Adresse für jeden Knoten innerhalb des Clusters zugegriffen werden. Informationen zum Blockieren des Datenverkehrs an Knotenports während der Verwendung eines NLB-Service finden Sie im Abschnitt [Eingehenden Datenverkehr an Netzlastausgleichsfunktion (NLB) oder NodePort-Services steuern](/docs/containers?topic=containers-network_policies#block_ingress).

Als Nächstes können Sie [einen NLB-Hostnamen registrieren](#loadbalancer_hostname).

<br />


## V1.0: NLB der Version 1.0 in einem Einzelzonencluster konfigurieren
{: #lb_config}

**Vorbereitende Schritte**:
* Es muss eine portierbare öffentliche oder private IP-Adresse verfügbar sein, die dem Netzlastausgleichsfunktions- (NLB-) Service zugewiesen werden kann. Weitere Informationen finden Sie unter [Teilnetze für Cluster konfigurieren](/docs/containers?topic=containers-subnets).
* Stellen Sie sicher, dass Sie die [{{site.data.keyword.Bluemix_notm}} IAM-Servicerolle **Schreibberechtigter** oder **Manager**](/docs/containers?topic=containers-users#platform) für den Namensbereich `default` innehaben.

Gehen Sie wie folgt vor, um einen NLB-Service der Version 1.0 in einem Einzelzonencluster zu erstellen:

1.  [Stellen Sie die App für den Cluster bereit](/docs/containers?topic=containers-app#app_cli). Stellen Sie sicher, dass Sie zur Bereitstellung im Metadatenabschnitt der Konfigurationsdatei eine Bezeichnung hinzufügen. Diese Bezeichnung ist zur Identifizierung aller Pods erforderlich, in denen Ihre App ausgeführt wird, damit sie in den Lastenausgleich aufgenommen werden können.
2.  Erstellen Sie einen Lastausgleichsservice für die App, die Sie über das öffentliche Internet oder ein privates Netz zugänglich machen wollen.
    1.  Erstellen Sie eine Servicekonfigurationsdatei namens `myloadbalancer.yaml` (Beispiel).

    2.  Definieren Sie einen Lastausgleichsservice für die App, die Sie zugänglich machen möchten.
        ```
        apiVersion: v1
        kind: Service
        metadata:
          name: myloadbalancer
          annotations:
            service.kubernetes.io/ibm-load-balancer-cloud-provider-ip-type: <public_oder_private>
            service.kubernetes.io/ibm-load-balancer-cloud-provider-vlan: "<vlan-id>"
        spec:
          type: LoadBalancer
          selector:
            <selektorschlüssel>: <selektorwert>
          ports:
           - protocol: TCP
           port: 8080
        loadBalancerIP: <ip-adresse>
        externalTrafficPolicy: Local
        ```
        {: codeblock}

        <table>
        <caption>Erklärung der Komponenten der YAML-Datei</caption>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der YAML-Dateikomponenten</th>
        </thead>
        <tbody>
        <tr>
          <td>`service.kubernetes.io/ibm-load-balancer-cloud-provider-ip-type:`
          <td>Annotation zum Angeben des Typs einer Lastausgleichsfunktion: <code>private</code> oder <code>public</code>.</td>
        </tr>
        <tr>
          <td>`service.kubernetes.io/ibm-load-balancer-cloud-provider-vlan:`
          <td>Annotation zum Angeben eines VLAN, in dem der Lastausgleichsservice bereitgestellt wird. Um VLANs anzuzeigen, führen Sie <code>ibmcloud ks vlans --zone <zone></code> aus.</td>
        </tr>
        <tr>
          <td><code>selector</code></td>
          <td>Der Bezeichnungsschlüssel (<em>&lt;selektorschlüssel&gt;</em>) und der Wert (<em>&lt;selektorwert&gt;</em>), den Sie im Abschnitt <code>spec.template.metadata.labels</code> der YAML-Bereitstellungsdatei für Ihre App verwendet haben.</td>
        </tr>
        <tr>
          <td><code>port</code></td>
          <td>Der Port, den der Service überwacht.</td>
        </tr>
        <tr>
          <td><code>loadBalancerIP</code></td>
          <td>Optional: Um eine private Lastausgleichsfunktion zu erstellen oder um eine bestimmte portierbare IP-Adresse für eine öffentliche Lastausgleichsfunktion zu verwenden, geben Sie die IP-Adresse an, die Sie verwenden wollen. Die IP-Adresse muss sich in dem VLAN befinden, das Sie in den Annotationen angeben. Wenn Sie keine IP-Adresse angeben, geschieht Folgendes:<ul><li>Wenn sich Ihr Cluster in einem öffentlichen VLAN befindet, dann wird eine portierbare öffentliche IP-Adresse verwendet. Die meisten Cluster befinden sich in einem öffentlichen VLAN.</li><li>Wenn sich Ihr Cluster nur in einem privaten VLAN befindet, wird eine portierbare private IP-Adresse verwendet.</li></ul></td>
        </tr>
        </tbody></table>

        Beispielkonfigurationsdatei zum Erstellen eines privaten NLB-Service der Version 1.0, der eine angegebene IP-Adresse im privaten VLAN `2234945` verwendet:

        ```
        apiVersion: v1
        kind: Service
        metadata:
          name: myloadbalancer
          annotations:
            service.kubernetes.io/ibm-load-balancer-cloud-provider-ip-type: private
            service.kubernetes.io/ibm-load-balancer-cloud-provider-vlan: "2234945"
        spec:
          type: LoadBalancer
          selector:
            app: nginx
          ports:
           - protocol: TCP
             port: 8080
          loadBalancerIP: 172.21.xxx.xxx
        ```
        {: codeblock}

    3. Optional: Stellen Sie sicher, dass Ihre NLB nur für einen begrenzten Bereich von IP-Adressen verfügbar ist, indem Sie die IP-Adressen im Feld `spec.loadBalancerSourceRanges` angeben. `loadBalancerSourceRanges` wird von `kube-proxy` in Ihrem Cluster durch Iptables-Regeln auf Workerknoten implementiert. Weitere Informationen enthält die [Kubernetes-Dokumentation ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/tasks/access-application-cluster/configure-cloud-provider-firewall/).

    4.  Erstellen Sie den Service in Ihrem Cluster.

        ```
        kubectl apply -f myloadbalancer.yaml
        ```
        {: pre}

3.  Stellen Sie sicher, dass der NLB-Service erfolgreich erstellt wurde. Es kann ein paar Minuten dauern, bis der Service erstellt wurde und die App verfügbar ist.

    ```
    kubectl describe service myloadbalancer
    ```
    {: pre}

    CLI-Beispielausgabe:

    ```
    Name:                   myloadbalancer
    Namespace:              default
    Labels:                 <none>
    Selector:               app=liberty
    Type:                   LoadBalancer
    Location:               dal10
    IP:                     172.21.xxx.xxx
    LoadBalancer Ingress:   169.xx.xxx.xxx
    Port:                   <unset> 8080/TCP
    NodePort:               <unset> 32040/TCP
    Endpoints:              172.30.xxx.xxx:8080
    Session Affinity:       None
    Events:
      FirstSeen	LastSeen	Count	From			SubObjectPath	Type	 Reason			          Message
      ---------	--------	-----	----			-------------	----	 ------			          -------
      10s		    10s		    1	    {service-controller }	  Normal CreatingLoadBalancer	Creating load balancer
      10s		    10s		    1	    {service-controller }		Normal CreatedLoadBalancer	Created load balancer
    ```
    {: screen}

    Die IP-Adresse für **LoadBalancer Ingress** ist die portierbare IP-Adresse, die dem NLB-Service zugewiesen wurde.

4.  Wenn Sie eine öffentliche NLB erstellt haben, dann greifen Sie über das Internet auf Ihre App zu.
    1.  Öffnen Sie Ihren bevorzugten Web-Browser.
    2.  Geben Sie die portierbare öffentliche IP-Adresse der NLB und des Ports ein.

        ```
        http://169.xx.xxx.xxx:8080
        ```
        {: codeblock}

5. Wenn Sie die [Beibehaltung der Quellen-IP für eine NLB der Version NLB 1.0](#node_affinity_tolerations) wählen, stellen Sie sicher, dass App-Pods auf den Edge-Workerknoten geplant werden, indem Sie [Edge-Knoten-Affinität zu App-Pods hinzufügen](#lb_edge_nodes). App-Pods müssen auf Edge-Knoten geplant werden, um eingehende Anforderungen empfangen zu können.

6. Optional: Ein Lastausgleichsservice macht Ihre App auch über die NodePort-Instanzen des Service verfügbar. Auf [Knotenports (NodePorts)](/docs/containers?topic=containers-nodeport) kann über jede öffentliche und private IP-Adresse für jeden Knoten innerhalb des Clusters zugegriffen werden. Informationen zum Blockieren des Datenverkehrs an Knotenports während der Verwendung eines NLB-Service finden Sie im Abschnitt [Eingehenden Datenverkehr an Netzlastausgleichsfunktion (NLB) oder NodePort-Services steuern](/docs/containers?topic=containers-network_policies#block_ingress).

Als Nächstes können Sie [einen NLB-Hostnamen registrieren](#loadbalancer_hostname).

<br />


## v1.0: Beibehaltung von Quellen-IP-Adressen aktivieren
{: #node_affinity_tolerations}

Dieses Feature ist nur für Netzlastausgleichsfunktionen (NLBs) der Version 1.0 verfügbar. Die Quellen-IP-Adresse von Clientanforderungen wird in den NLBs der Version 2.0 standardmäßig beibehalten.
{: note}

Wenn eine Clientanforderung an Ihre App an Ihren Cluster gesendet wird, empfängt ein Pod des Lastausgleichsservice die Anforderung. Wenn ein App-Pod nicht auf demselben Workerknoten vorhanden ist wie der Lastausgleichsfunktions-Pod, leitet die NLB die Anforderung an einen App-Pod auf einem anderen Workerknoten weiter. Die Quellen-IP-Adresse des Pakets wird in die öffentliche IP-Adresse des Workerknotens geändert, auf dem der Pod des Lastausgleichsservice ausgeführt wird.
{: shortdesc}

Um die ursprüngliche Quellen-IP-Adresse der Clientanforderung beizubehalten, können Sie die [Quellen-IP ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/tasks/access-application-cluster/create-external-load-balancer/#preserving-the-client-source-ip) für Lastausgleichsservices aktivieren. Die TCP-Verbindung wird bis zu den App-Pods fortgesetzt, sodass die App die tatsächliche IP-Adresse des Initiators sehen kann. Das Beibehalten der IP des Clients ist nützlich, z. B. wenn App-Server Sicherheits- und Zugriffssteuerungsrichtlinien genügen müssen.

Nachdem Sie die Quellen-IP aktiviert haben, müssen Lastausgleichsfunktions-Pods Anforderungen an App-Pods weiterleiten, die auf demselben Workerknoten bereitgestellt sind. In der Regel werden auch Lastausgleichsfunktions-Pods auf den Workerknoten bereitgestellt, auf denen sich die App-Pods befinden. Es kann jedoch vorkommen, dass die Lastausgleichsfunktions-Pods und die App-Pods nicht auf demselben Workerknoten geplant sind:

* Sie verfügen Edge-Knoten, auf die ein Taint angewendet wurde und auf denen deshalb nur Lastausgleichsfunktions-Pods bereitgestellt werden können. App-Pods dürfen auf diesen Knoten nicht bereitgestellt werden.
* Ihr Cluster ist mit mehreren öffentlichen oder privaten VLANs verbunden und Ihre App-Pods werden unter Umständen auf Workerknoten bereitgestellt, die nur mit einem VLAN verbunden sind. Lastausgleichsfunktions-Pods lassen sich möglicherweise nicht auf solchen Workerknoten bereitstellen, weil die NLB-IP-Adresse mit einem anderen VLAN als die Workerknoten verbunden ist.

Um zu erzwingen, dass Ihre App auf bestimmten Workerknoten bereitgestellt wird, auf denen auch Lastausgleichsfunktions-Pods bereitgestellt werden können, müssen Sie Affinitätsregeln und Tolerierungen zu Ihrer App-Bereitstellung hinzufügen.

### Affinitätsregeln und Tolerierungen für Edge-Knoten hinzufügen
{: #lb_edge_nodes}

Wenn Sie [Workerknoten als Edge-Knoten kennzeichnen](/docs/containers?topic=containers-edge#edge_nodes) und zudem [Taints auf Edge-Knoten anwenden](/docs/containers?topic=containers-edge#edge_workloads), werden Lastausgleichsfunktions-Pods nur auf diesen Edge-Knoten bereitgestellt und App-Pods können nicht auf Edge-Knoten implementiert werden. Wenn die Quellen-IP für den NLB-Service aktiviert ist, können die Lastausgleichsfunktions-Pods auf den Edge-Knoten eingehende Anforderungen nicht an die App-Pods auf anderen Workerknoten weiterleiten.
{:shortdesc}

Um zu erzwingen, dass Ihre App-Pods auf Edge-Knoten bereitgestellt werden, fügen Sie eine [Affinitätsregel ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/#node-affinity-beta-feature) für den Edge-Knoten und eine [Tolerierung ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/#concepts) zur App-Bereitstellung hinzu.

Beispiel für die YAML-Bereitstellungsdatei mit Edge-Knoten-Affinität und -Tolerierung:

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: with-node-affinity
spec:
  selector:
    matchLabels:
      <label_name>: <label_value>
  template:
    spec:
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: dedicated
                operator: In
                values:
                - edge
      tolerations:
        - key: dedicated
          value: edge
...
```
{: codeblock}

In beiden Abschnitten **affinity** und **tolerations** ist der Wert `dedicated` für `key` und der Wert `edge` für `value` angegeben.

### Affinitätsregeln für mehrere öffentliche oder private VLANs hinzufügen
{: #edge_nodes_multiple_vlans}

Wenn Ihr Cluster mit mehreren öffentlichen oder privaten VLANs verbunden ist, werden Ihre App-Pods unter Umständen auf Workerknoten bereitgestellt, die nur mit einem VLAN verbunden sind. Wenn die NLB-IP-Adresse mit einem anderen VLAN als diese Workerknoten verbunden ist, werden die Lastausgleichsfunktions-Pods nicht auf diesen Workerknoten bereitgestellt.
{:shortdesc}

Wenn die Quellen-IP aktiviert ist, planen Sie App-Pods auf Workerknoten, die dasselbe VLAN wie die NLB-IP-Adresse aufweisen, indem Sie eine Affinitätsregel zur App-Bereitstellung hinzufügen.

Vorbereitende Schritte: [Melden Sie sich an Ihrem Konto an. Geben Sie als Ziel die entsprechende Region und, sofern zutreffend, die Ressourcengruppe an. Legen Sie den Kontext für den Cluster fest.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1. Rufen Sie die IP-Adresse des NLB-Service ab. Suchen Sie im Feld **LoadBalancer Ingress** nach der IP-Adresse.
    ```
    kubectl describe service <name_des_lastausgleichsservice>
    ```
    {: pre}

2. Rufen Sie die VLAN-ID ab, mit der Ihr NLB-Service verbunden ist.

    1. Listen Sie portierbare öffentliche VLANs für Ihren Cluster auf.
        ```
        ibmcloud ks cluster-get --cluster <clustername_oder_-id> --showResources
        ```
        {: pre}

        Beispielausgabe:
        ```
        ...

        Subnet VLANs
        VLAN ID   Subnet CIDR       Public   User-managed
        2234947   10.xxx.xx.xxx/29  false    false
        2234945   169.36.5.xxx/29   true     false
        ```
        {: screen}

    2. Suchen Sie in der Ausgabe unter **Subnet VLANs** nach der Teilnetz-CIDR, die mit der NLB-IP-Adresse übereinstimmt, die Sie zuvor abgerufen haben, und notieren Sie sich die VLAN-ID.

        Wenn die IP-Adresse des NLB-Service beispielsweise `169.36.5.xxx` lautet, ist das passende Teilnetz in der Beispielausgabe aus dem vorherigen Schritt `169.36.5.xxx/29`. Die VLAN-ID, mit der das Teilnetz verbunden ist, lautet `2234945`.

3. [Fügen Sie eine Affinitätsregel ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/#node-affinity-beta-feature) zu der App-Bereitstellung für die VLAN-ID hinzu, die Sie sich im vorherigen Schritt notiert haben.

    Wenn Sie beispielsweise über mehrere VLANs verfügen, aber Ihre App-Pods nur auf Workerknoten im öffentlichen VLAN `2234945` bereitstellen möchten.

    ```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: with-node-affinity
    spec:
      selector:
        matchLabels:
          <label_name>: <label_value>
      template:
        spec:
          affinity:
            nodeAffinity:
              requiredDuringSchedulingIgnoredDuringExecution:
                nodeSelectorTerms:
                - matchExpressions:
                  - key: publicVLAN
                    operator: In
                    values:
                    - "2234945"
    ...
    ```
    {: codeblock}

    In der YAML-Beispieldatei enthält der Abschnitt **affinity** den Wert `publicVLAN` für `key` und den Wert `"2234945"` für `value`.

4. Wenden Sie die aktualisierte Bereitstellungskonfigurationsdatei an.
    ```
    kubectl apply -f with-node-affinity.yaml
    ```
    {: pre}

5. Überprüfen Sie, dass die App-Pods auf Workerknoten bereitgestellt werden, die mit dem designierten VLAN verbunden sind.

    1. Listen Sie die Pods in Ihrem Cluster auf. Ersetzen Sie `<selector>` durch die Bezeichnung, die Sie für die App verwendet haben.
        ```
        kubectl get pods -o wide app=<selektor>
        ```
        {: pre}

        Beispielausgabe:
        ```
        NAME                   READY     STATUS              RESTARTS   AGE       IP               NODE
        cf-py-d7b7d94db-vp8pq  1/1       Running             0          15d       172.30.xxx.xxx   10.176.48.78
        ```
        {: screen}

    2. Geben Sie in der Ausgabe einen Pod für Ihre App an. Notieren Sie sich die Knoten-ID (**NODE**) des Workerknotens, auf dem sich der Pod befindet.

        In der Beispielausgabe aus dem vorherigen Schritt befindet sich der App-Pod `cf-py-d7b7d94db-vp8pq` auf dem Workerknoten `10.176.48.78`.

    3. Listen Sie die Details für den Workerknoten auf.

        ```
        kubectl describe node <workerknoten-ID>
        ```
        {: pre}

        Beispielausgabe:

        ```
        Name:                   10.xxx.xx.xxx
        Role:
        Labels:                 arch=amd64
                                beta.kubernetes.io/arch=amd64
                                beta.kubernetes.io/os=linux
                                failure-domain.beta.kubernetes.io/region=us-south
                                failure-domain.beta.kubernetes.io/zone=dal10
                                ibm-cloud.kubernetes.io/encrypted-docker-data=true
                                kubernetes.io/hostname=10.xxx.xx.xxx
                                privateVLAN=2234945
                                publicVLAN=2234967
        ...
        ```
        {: screen}

    4. Überprüfen Sie im Abschnitt **Labels** der Ausgabe, dass es sich bei dem öffentlichen oder privaten VLAN um das VLAN handelt, das Sie in den vorherigen Schritten angegeben haben.

<br />


## v2.0: Komponenten und Architektur (Beta)
{: #planning_ipvs}

Die Funktionen der Netzlastausgleichsfunktion (NLB) Version 2.0 sind Beta-Funktionen. Um eine NLB der Version 2.0 zu verwenden, müssen Sie [den Master und die Workerknoten Ihres Clusters aktualisieren](/docs/containers?topic=containers-update), sodass sie Kubernetes Version 1.12 oder höher ausführen.
{: note}

Die NLB der Version 2.0 ist eine Layer 4-Lastausgleichsfunktion, die den IPVS (IP Virtual Server) des Linux-Kernels verwendet. Die NLB 2.0 unterstützt TCP und UDP, wird vor mehreren Workerknoten ausgeführt und verwendet IPIP-Tunneling (IP über IP), um Datenverkehr zu verteilen, der über eine einzelne NLB-IP-Adresse für alle diese Workerknoten eingeht.

Wünschen Sie weitere Details zu den Bereitstellungsmustern für Lastausgleich, die in {{site.data.keyword.containerlong_notm}} verfügbar sind? Schauen Sie sich diesen [Blogeintrag ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/blogs/bluemix/2018/10/ibm-cloud-kubernetes-service-deployment-patterns-for-maximizing-throughput-and-availability/) an.
{: tip}

### Datenfluss in einem Einzelzonencluster
{: #ipvs_single}

Das folgende Diagramm veranschaulicht, wie eine NLB der Version 2.0 die Kommunikation vom Internet an eine App in einem Einzelzonencluster leitet.
{: shortdesc}

<img src="images/cs_loadbalancer_ipvs_planning.png" width="600" alt="App in {{site.data.keyword.containerlong_notm}} über eine NLB der Version 2.0 verfügbar machen" style="width:600px; border-style: none"/>

1. Eine Clientanforderung an Ihre App verwendet die öffentliche IP-Adresse der NLB und den zugeordneten Port auf dem Workerknoten. In diesem Beispiel hat die NLB die virtuelle IP-Adresse '169.61.23.130', die sich derzeit auf dem Worker 10.73.14.25 befindet.

2. Die NLB kapselt das Clientanforderungspaket (im Bild als "CR" gekennzeichnet) in ein IPIP-Paket (gekennzeichnet als "IPIP") ein. Das Clientanforderungspaket behält die Client-IP als Quellen-IP-Adresse bei. Das einschließende IPIP-Paket verwendet die IP des Workers 10.73.14.25 als Quellen-IP-Adresse.

3. Die NLB leitet das IPIP-Paket an einen Worker weiter, auf dem sich ein App-Pod befindet, 10.73.14.26. Wenn mehrere App-Instanzen im Cluster bereitgestellt werden, leitet die NLB die Anforderungen zwischen den Workern weiter, auf denen App-Pods bereitgestellt sind.

4. Worker 10.73.14.26 entpackt das einschließende IPIP-Paket und dann das Clientanforderungspaket. Das Clientanforderungspaket wird an den App-Pod auf diesem Workerknoten weitergeleitet.

5. Worker 10.73.14.26 verwendet dann die Quellen-IP-Adresse aus dem ursprünglichen Anforderungspaket, die Client-IP, um das Antwortpaket des App-Pods direkt an den Client zurückzugeben.

### Datenfluss in einem Mehrzonencluster
{: #ipvs_multi}

Der Datenfluss durch ein Mehrzonencluster folgt demselben Pfad wie [Datenverkehr durch ein Einzelzonencluster](#ipvs_single). In einem Mehrzonencluster leitet die NLB Anforderungen an die App-Instanzen in ihrer eigenen Zone und an App-Instanzen in anderen Zonen weiter. Das folgende Diagramm veranschaulicht, wie NLBs der Version 2.0 in den einzelnen Zonen Datenverkehr vom Internet an eine App in einem Mehrzonencluster weiterleiten.
{: shortdesc}

<img src="images/cs_loadbalancer_ipvs_multizone.png" alt="App in {{site.data.keyword.containerlong_notm}} über eine NLB der Version 2.0 zugänglich machen" style="width:500px; border-style: none"/>

Standardmäßig ist jede NLB der Version 2.0 nur in einer Zone konfiguriert. Sie können eine hohe Verfügbarkeit erreichen, indem Sie eine NLB der Version 2.0 in jeder Zone bereitstellen, in der sich App-Instanzen befinden.

<br />


## v2.0: Voraussetzungen
{: #ipvs_provision}

Sie können eine vorhandene NLB der Version 1.0 nicht auf eine Lastausgleichsfunktion der Version 2.0 aktualisieren. Sie müssen eine neue NLB mit der Version 2.0 erstellen. Beachten Sie, dass Sie NLBs der Version 1.0 und 2.0 gleichzeitig in einem Cluster ausführen können.
{: shortdesc}

Bevor Sie eine NLB der Version 2.0 erstellen, müssen Sie die folgenden vorausgesetzten Schritte ausführen.

1. [Aktualisieren Sie die Master- und Workerknoten Ihres Clusters](/docs/containers?topic=containers-update) auf Kubernetes Version 1.12 oder höher.

2. Damit Ihre NLB der Version 2.0 Anforderungen an App-Pods in mehreren Zonen weiterleiten kann, öffnen Sie einen Supportfall, um eine Konfigurationseinstellung für Ihre VLANs anzufordern. **Wichtig**: Sie müssen diese Konfiguration für alle öffentlichen VLANs anfordern. Wenn Sie ein neues zugeordnetes VLAN anfordern, müssen Sie ein weiteres Ticket für dieses VLAN öffnen.
    1. Melden Sie sich bei der [{{site.data.keyword.Bluemix_notm}}-Konsole](https://cloud.ibm.com/) an.
    2. Klicken Sie in der Menüleiste auf **Support**, klicken Sie auf die Registerkarte **Fälle verwalten** und klicken Sie auf **Neuen Fall erstellen**.
    3. Geben Sie in die Fallfelder die folgenden Informationen ein:
       * Wählen Sie für den Typ von Support die Option **Technisch** aus.
       * Wählen Sie für die Kategorie die Option **VLAN Spanning** aus.
       * Geben Sie den Betreff **Frage zu öffentlichem VLAN-Netz** ein.
    4. Fügen Sie die folgenden Informationen zur Beschreibung hinzu: "Konfigurieren Sie das Netz bitte so, dass eine Kapazitätsaggregation in den öffentlichen VLANs, die diesem Konto zugeordnet sind, zulässig ist. Das Referenzticket für diese Anforderung lautet: https://control.softlayer.com/support/tickets/63859145".
    5. Klicken Sie auf **Übergeben**.

3. Aktivieren Sie eine [VRF-Funktion (Virtual Router Function)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) für Ihr IBM Cloud-Infrastrukturkonto (SoftLayer). Zur Aktivierung von VRF [wenden Sie sich an Ihren Ansprechpartner für die IBM Cloud-Infrastruktur (SoftLayer)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#how-you-can-initiate-the-conversion). Wenn Sie VRF nicht aktivieren können oder wollen, aktivieren Sie das [VLAN-Spanning](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning). Wenn VRF- oder VLAN-Spanning aktiviert ist, kann die NLB der Version 2.0 Pakete an verschiedene Teilnetz im Konto weiterleiten.

4. Wenn Sie [Calico-Netzrichtlinien des Typs 'Pre-DNAT'](/docs/containers?topic=containers-network_policies#block_ingress) verwenden, um Datenverkehr an die IP-Adresse einer NLB der Version 2.0 weiterzuleiten, müssen Sie die Felder `applyOnForward: true` und `doNotTrack: true` dem Abschnitt `spec` in den Richtlinien hinzufügen und das Feld `preDNAT: true` daraus entfernen. `applyOnForward: true` stellt sicher, dass die Calico-Richtlinie beim Einschließen und Weiterleiten auf den Datenverkehr angewendet wird. `doNotTrack: true` stellt sicher, dass die Workerknoten DSR verwenden können, um ein Antwortpaket direkt an den Client zurückzugeben, ohne dass die Verbindung verfolgt werden muss. Wenn Sie beispielsweise eine Calico-Richtlinie verwenden, um Datenverkehr von nur einer spezifischen IP-Adresse an die NLB-IP-Adresse in eine Whitelist zu schreiben, sieht die Richtlinie wie folgt aus:
    ```
    apiVersion: projectcalico.org/v3
    kind: GlobalNetworkPolicy
    metadata:
      name: whitelist
    spec:
      applyOnForward: true
      doNotTrack: true
      ingress:
      - action: Allow
        destination:
          nets:
          - <ip_der_lastausgleichsfunktion>/32
          ports:
          - 80
        protocol: TCP
        source:
          nets:
          - <clientadresse>/32
      selector: ibm.role=='worker_public'
      order: 500
      types:
      - Ingress
    ```
    {: screen}

Jetzt können Sie die Schritte unter [NLB der Version 2.0 in einem Mehrzonencluster konfigurieren](#ipvs_multi_zone_config) bzw. [in einem Einzelzonencluster konfigurieren](#ipvs_single_zone_config) ausführen.

<br />


## V2.0: NLB der Version 2.0 in einem Mehrzonencluster konfigurieren
{: #ipvs_multi_zone_config}

**Vorbereitende Schritte**:

* **Wichtig**: Erfüllen Sie die [Voraussetzungen für die NLB der Version 2.0](#ipvs_provision).
* Um öffentliche NLBs in mehreren Zonen zu erstellen, muss mindestens ein öffentliches VLAN portierbare Teilnetze aufweisen, die in jeder Zone verfügbar sind. Um private NLBs in mehreren Zonen zu erstellen, muss mindestens ein privates VLAN portierbare Teilnetze aufweisen, die in jeder Zone verfügbar sind. Sie können Teilnetze hinzufügen, indem Sie die Schritte im Abschnitt [Teilnetze für Cluster konfigurieren](/docs/containers?topic=containers-subnets) ausführen.
* Wenn Sie den Datenaustausch im Netz auf Edge-Workerknoten beschränken, müssen Sie sicherstellen, dass in jeder Zone mindestens zwei [Edge-Workerknoten](/docs/containers?topic=containers-edge#edge) aktiviert sind, sodass NLBs gleichmäßig bereitgestellt werden können.
* Stellen Sie sicher, dass Sie die [{{site.data.keyword.Bluemix_notm}} IAM-Servicerolle **Schreibberechtigter** oder **Manager**](/docs/containers?topic=containers-users#platform) für den Namensbereich `default` innehaben.


Gehen Sie wie folgt vor, um eine NLB der Version 2.0 in einem Mehrzonencluster einzurichten:
1.  [Stellen Sie die App für den Cluster bereit](/docs/containers?topic=containers-app#app_cli). Stellen Sie sicher, dass Sie zur Bereitstellung im Metadatenabschnitt der Konfigurationsdatei eine Bezeichnung hinzufügen. Diese Bezeichnung ist zur Identifizierung aller Pods erforderlich, in denen Ihre App ausgeführt wird, damit sie in den Lastenausgleich aufgenommen werden können.

2.  Erstellen Sie einen Lastausgleichsservice für die App, die Sie über das öffentliche Internet oder ein privates Netz zugänglich machen wollen.
  1. Erstellen Sie eine Servicekonfigurationsdatei namens `myloadbalancer.yaml` (Beispiel).
  2. Definieren Sie einen Lastausgleichsservice für die App, die Sie zugänglich machen möchten. Sie können eine Zone, ein VLAN und eine IP-Adresse angeben.

      ```
      apiVersion: v1
      kind: Service
      metadata:
        name: myloadbalancer
        annotations:
          service.kubernetes.io/ibm-load-balancer-cloud-provider-ip-type: <public_oder_private>
          service.kubernetes.io/ibm-load-balancer-cloud-provider-zone: "<zone>"
          service.kubernetes.io/ibm-load-balancer-cloud-provider-vlan: "<vlan-id>"
          service.kubernetes.io/ibm-load-balancer-cloud-provider-enable-features: "ipvs"
          service.kubernetes.io/ibm-load-balancer-cloud-provider-scheduler: "<algorithmus>"
      spec:
        type: LoadBalancer
        selector:
          <selektorschlüssel>: <selektorwert>
        ports:
         - protocol: TCP
           port: 8080
        loadBalancerIP: <ip-adresse>
        externalTrafficPolicy: Local
      ```
      {: codeblock}

      <table>
      <caption>Erklärung der Komponenten der YAML-Datei</caption>
      <thead>
      <th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der YAML-Dateikomponenten</th>
      </thead>
      <tbody>
      <tr>
        <td><code>service.kubernetes.io/ibm-load-balancer-cloud-provider-ip-type:</code>
        <td>Annotation zum Angeben des Typs einer Lastausgleichsfunktion: <code>private</code> oder <code>public</code>.</td>
      </tr>
      <tr>
        <td><code>service.kubernetes.io/ibm-load-balancer-cloud-provider-zone:</code>
        <td>Annotation zum Angeben der Zone, in der der Lastausgleichsservice bereitgestellt wird. Um Zonen anzuzeigen, führen Sie den Befehl <code>ibmcloud ks zones</code> aus.</td>
      </tr>
      <tr>
        <td>`service.kubernetes.io/ibm-load-balancer-cloud-provider-vlan:`
        <td>Annotation zum Angeben eines VLAN, in dem der Lastausgleichsservice bereitgestellt wird. Um VLANs anzuzeigen, führen Sie <code>ibmcloud ks vlans --zone <zone></code> aus.</td>
      </tr>
      <tr>
        <td><code>service.kubernetes.io/ibm-load-balancer-cloud-provider-enable-features: "ipvs"</code>
        <td>Annotation zum Angeben einer Lastausgleichsfunktion der Version 2.0.</td>
      </tr>
      <tr>
        <td><code>service.kubernetes.io/ibm-load-balancer-cloud-provider-scheduler:</code>
        <td>Optional: Annotation zum Angeben eines Planungsalgorithmus. Akzeptierte Werte sind <code>"rr"</code> für Round Robin (Standard) oder <code>"sh"</code> für Source Hashing. Weitere Informationen finden Sie unter [v2.0: Planungsalgorithmen](#scheduling).</td>
      </tr>
      <tr>
        <td><code>selector</code></td>
        <td>Der Bezeichnungsschlüssel (<em>&lt;selektorschlüssel&gt;</em>) und der Wert (<em>&lt;selektorwert&gt;</em>), den Sie im Abschnitt <code>spec.template.metadata.labels</code> der YAML-Bereitstellungsdatei für Ihre App verwendet haben.</td>
      </tr>
      <tr>
        <td><code>port</code></td>
        <td>Der Port, den der Service überwacht.</td>
      </tr>
      <tr>
        <td><code>loadBalancerIP</code></td>
        <td>Optional: Um eine private NLB zu erstellen oder um eine bestimmte portierbare IP-Adresse für eine öffentliche NLB zu verwenden, geben Sie die IP-Adresse an, die Sie verwenden wollen. Die IP-Adresse muss sich in der Zone und dem VLAN befinden, die Sie in den Annotationen angeben. Wenn Sie keine IP-Adresse angeben, geschieht Folgendes:<ul><li>Wenn sich Ihr Cluster in einem öffentlichen VLAN befindet, dann wird eine portierbare öffentliche IP-Adresse verwendet. Die meisten Cluster befinden sich in einem öffentlichen VLAN.</li><li>Wenn sich Ihr Cluster nur in einem privaten VLAN befindet, wird eine portierbare private IP-Adresse verwendet.</li></ul></td>
      </tr>
      <tr>
        <td><code>externalTrafficPolicy: Local</code></td>
        <td>Setzen Sie dies auf <code>Local</code>.</td>
      </tr>
      </tbody></table>

      Beispielkonfigurationsdatei zum Erstellen eines NLB-Service der Version 2.0 in `dal12`, die den Round Robin-Planungsalgorithmus verwendet.

      ```
      apiVersion: v1
      kind: Service
      metadata:
        name: myloadbalancer
        annotations:
          service.kubernetes.io/ibm-load-balancer-cloud-provider-zone: "dal12"
          service.kubernetes.io/ibm-load-balancer-cloud-provider-enable-features: "ipvs"
          service.kubernetes.io/ibm-load-balancer-cloud-provider-scheduler: "rr"
      spec:
        type: LoadBalancer
        selector:
          app: nginx
        ports:
         - protocol: TCP
           port: 8080
        externalTrafficPolicy: Local
      ```
      {: codeblock}

  3. Optional: Stellen Sie sicher, dass Ihre NLB nur für einen begrenzten Bereich von IP-Adressen verfügbar ist, indem Sie die IP-Adressen im Feld `spec.loadBalancerSourceRanges` angeben. `loadBalancerSourceRanges` wird von `kube-proxy` in Ihrem Cluster durch Iptables-Regeln auf Workerknoten implementiert. Weitere Informationen enthält die [Kubernetes-Dokumentation ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/tasks/access-application-cluster/configure-cloud-provider-firewall/).

  4. Erstellen Sie den Service in Ihrem Cluster.

      ```
      kubectl apply -f myloadbalancer.yaml
      ```
      {: pre}

3. Stellen Sie sicher, dass der NLB-Service erfolgreich erstellt wurde. Es kann einige Minuten dauern, bis der NLB-Service ordnungsgemäß erstellt wurde und die App verfügbar ist.

    ```
    kubectl describe service myloadbalancer
    ```
    {: pre}

    CLI-Beispielausgabe:

    ```
    Name:                   myloadbalancer
    Namespace:              default
    Labels:                 <none>
    Selector:               app=liberty
    Type:                   LoadBalancer
    Zone:                   dal10
    IP:                     172.21.xxx.xxx
    LoadBalancer Ingress:   169.xx.xxx.xxx
    Port:                   <unset> 8080/TCP
    NodePort:               <unset> 32040/TCP
    Endpoints:              172.30.xxx.xxx:8080
    Session Affinity:       None
    Events:
      FirstSeen	LastSeen	Count	From			SubObjectPath	Type	 Reason			          Message
      ---------	--------	-----	----			-------------	----	 ------			          -------
      10s		    10s		    1	    {service-controller }	  Normal CreatingLoadBalancer	Creating load balancer
      10s		    10s		    1	    {service-controller }		Normal CreatedLoadBalancer	Created load balancer
    ```
    {: screen}

    Die IP-Adresse für **LoadBalancer Ingress** ist die portierbare IP-Adresse, die dem NLB-Service zugewiesen wurde.

4.  Wenn Sie eine öffentliche NLB erstellt haben, dann greifen Sie über das Internet auf Ihre App zu.
    1.  Öffnen Sie Ihren bevorzugten Web-Browser.
    2.  Geben Sie die portierbare öffentliche IP-Adresse der NLB und des Ports ein.

        ```
        http://169.xx.xxx.xxx:8080
        ```
        {: codeblock}

5. Um eine hohe Verfügbarkeit zu erreichen, wiederholen Sie die Schritte 2 bis 4, um eine NLB der Version 2.0 in jeder Zone bereitstellen, in der sich App-Instanzen befinden.

6. Optional: Ein NLB-Service macht Ihre App auch über die NodePort-Instanzen des Service verfügbar. Auf [Knotenports (NodePorts)](/docs/containers?topic=containers-nodeport) kann über jede öffentliche und private IP-Adresse für jeden Knoten innerhalb des Clusters zugegriffen werden. Informationen zum Blockieren des Datenverkehrs an Knotenports während der Verwendung eines NLB-Service finden Sie im Abschnitt [Eingehenden Datenverkehr an Netzlastausgleichsfunktion (NLB) oder NodePort-Services steuern](/docs/containers?topic=containers-network_policies#block_ingress).

Als Nächstes können Sie [einen NLB-Hostnamen registrieren](#loadbalancer_hostname).

<br />


## V2.0: NLB der Version 2.0 in einem Einzelzonencluster konfigurieren
{: #ipvs_single_zone_config}

**Vorbereitende Schritte**:

* **Wichtig**: Erfüllen Sie die [Voraussetzungen für die NLB der Version 2.0](#ipvs_provision).
* Es muss eine portierbare öffentliche oder private IP-Adresse verfügbar sein, die dem NLB-Service zugewiesen werden kann. Weitere Informationen finden Sie unter [Teilnetze für Cluster konfigurieren](/docs/containers?topic=containers-subnets).
* Stellen Sie sicher, dass Sie die [{{site.data.keyword.Bluemix_notm}} IAM-Servicerolle **Schreibberechtigter** oder **Manager**](/docs/containers?topic=containers-users#platform) für den Namensbereich `default` innehaben.

Gehen Sie wie folgt vor, um einen NLB-Service der Version 2.0 in einem Einzelzonencluster zu erstellen:

1.  [Stellen Sie die App für den Cluster bereit](/docs/containers?topic=containers-app#app_cli). Stellen Sie sicher, dass Sie zur Bereitstellung im Metadatenabschnitt der Konfigurationsdatei eine Bezeichnung hinzufügen. Diese Bezeichnung ist zur Identifizierung aller Pods erforderlich, in denen Ihre App ausgeführt wird, damit sie in den Lastenausgleich aufgenommen werden können.
2.  Erstellen Sie einen Lastausgleichsservice für die App, die Sie über das öffentliche Internet oder ein privates Netz zugänglich machen wollen.
    1.  Erstellen Sie eine Servicekonfigurationsdatei namens `myloadbalancer.yaml` (Beispiel).

    2.  Definieren Sie einen Lastausgleichsservice der Version 2.0 für die App, die Sie zugänglich machen möchten.
        ```
        apiVersion: v1
        kind: Service
        metadata:
          name: myloadbalancer
          annotations:
            service.kubernetes.io/ibm-load-balancer-cloud-provider-ip-type: <public_oder_private>
            service.kubernetes.io/ibm-load-balancer-cloud-provider-vlan: "<vlan-id>"
            service.kubernetes.io/ibm-load-balancer-cloud-provider-enable-features: "ipvs"
            service.kubernetes.io/ibm-load-balancer-cloud-provider-scheduler: "<algorithmus>"
        spec:
          type: LoadBalancer
          selector:
            <selektorschlüssel>: <selektorwert>
          ports:
           - protocol: TCP
           port: 8080
        loadBalancerIP: <ip-adresse>
        externalTrafficPolicy: Local
        ```
        {: codeblock}

        <table>
        <caption>Erklärung der Komponenten der YAML-Datei</caption>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der YAML-Dateikomponenten</th>
        </thead>
        <tbody>
        <tr>
          <td>`service.kubernetes.io/ibm-load-balancer-cloud-provider-ip-type:`
          <td>Annotation zum Angeben des Typs einer Lastausgleichsfunktion: <code>private</code> oder <code>public</code>.</td>
        </tr>
        <tr>
          <td>`service.kubernetes.io/ibm-load-balancer-cloud-provider-vlan:`
          <td>Optional: Annotation zum Angeben eines VLAN, in dem der Lastausgleichsservice bereitgestellt wird. Um VLANs anzuzeigen, führen Sie <code>ibmcloud ks vlans --zone <zone></code> aus.</td>
        </tr>
        <tr>
          <td><code>service.kubernetes.io/ibm-load-balancer-cloud-provider-enable-features: "ipvs"</code>
          <td>Annotation zum Angeben einer Lastausgleichsfunktion der Version 2.0.</td>
        </tr>
        <tr>
          <td><code>service.kubernetes.io/ibm-load-balancer-cloud-provider-scheduler:</code>
          <td>Optional: Annotation zum Angeben eines Planungsalgorithmus. Akzeptierte Werte sind <code>"rr"</code> für Round Robin (Standard) oder <code>"sh"</code> für Source Hashing. Weitere Informationen finden Sie unter [v2.0: Planungsalgorithmen](#scheduling).</td>
        </tr>
        <tr>
          <td><code>selector</code></td>
          <td>Der Bezeichnungsschlüssel (<em>&lt;selektorschlüssel&gt;</em>) und der Wert (<em>&lt;selektorwert&gt;</em>), den Sie im Abschnitt <code>spec.template.metadata.labels</code> der YAML-Bereitstellungsdatei für Ihre App verwendet haben.</td>
        </tr>
        <tr>
          <td><code>port</code></td>
          <td>Der Port, den der Service überwacht.</td>
        </tr>
        <tr>
          <td><code>loadBalancerIP</code></td>
          <td>Optional: Um eine private NLB zu erstellen oder um eine bestimmte portierbare IP-Adresse für eine öffentliche NLB zu verwenden, geben Sie die IP-Adresse an, die Sie verwenden wollen. Die IP-Adresse muss sich in dem VLAN befinden, das Sie in den Annotationen angeben. Wenn Sie keine IP-Adresse angeben, geschieht Folgendes:<ul><li>Wenn sich Ihr Cluster in einem öffentlichen VLAN befindet, dann wird eine portierbare öffentliche IP-Adresse verwendet. Die meisten Cluster befinden sich in einem öffentlichen VLAN.</li><li>Wenn sich Ihr Cluster nur in einem privaten VLAN befindet, wird eine portierbare private IP-Adresse verwendet.</li></ul></td>
        </tr>
        <tr>
          <td><code>externalTrafficPolicy: Local</code></td>
          <td>Setzen Sie dies auf <code>Local</code>.</td>
        </tr>
        </tbody></table>

    3.  Optional: Stellen Sie sicher, dass Ihre NLB nur für einen begrenzten Bereich von IP-Adressen verfügbar ist, indem Sie die IP-Adressen im Feld `spec.loadBalancerSourceRanges` angeben. `loadBalancerSourceRanges` wird von `kube-proxy` in Ihrem Cluster durch Iptables-Regeln auf Workerknoten implementiert. Weitere Informationen enthält die [Kubernetes-Dokumentation ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/tasks/access-application-cluster/configure-cloud-provider-firewall/).

    4.  Erstellen Sie den Service in Ihrem Cluster.

        ```
        kubectl apply -f myloadbalancer.yaml
        ```
        {: pre}

3.  Stellen Sie sicher, dass der NLB-Service erfolgreich erstellt wurde. Es kann ein paar Minuten dauern, bis der Service erstellt wurde und die App verfügbar ist.

    ```
    kubectl describe service myloadbalancer
    ```
    {: pre}

    CLI-Beispielausgabe:

    ```
    Name:                   myloadbalancer
    Namespace:              default
    Labels:                 <none>
    Selector:               app=liberty
    Type:                   LoadBalancer
    Location:               dal10
    IP:                     172.21.xxx.xxx
    LoadBalancer Ingress:   169.xx.xxx.xxx
    Port:                   <unset> 8080/TCP
    NodePort:               <unset> 32040/TCP
    Endpoints:              172.30.xxx.xxx:8080
    Session Affinity:       None
    Events:
      FirstSeen	LastSeen	Count	From			SubObjectPath	Type	 Reason			          Message
      ---------	--------	-----	----			-------------	----	 ------			          -------
      10s		    10s		    1	    {service-controller }	  Normal CreatingLoadBalancer	Creating load balancer
      10s		    10s		    1	    {service-controller }		Normal CreatedLoadBalancer	Created load balancer
    ```
    {: screen}

    Die IP-Adresse für **LoadBalancer Ingress** ist die portierbare IP-Adresse, die dem NLB-Service zugewiesen wurde.

4.  Wenn Sie eine öffentliche NLB erstellt haben, dann greifen Sie über das Internet auf Ihre App zu.
    1.  Öffnen Sie Ihren bevorzugten Web-Browser.
    2.  Geben Sie die portierbare öffentliche IP-Adresse der NLB und des Ports ein.

        ```
        http://169.xx.xxx.xxx:8080
        ```
        {: codeblock}

5. Optional: Ein NLB-Service macht Ihre App auch über die NodePort-Instanzen des Service verfügbar. Auf [Knotenports (NodePorts)](/docs/containers?topic=containers-nodeport) kann über jede öffentliche und private IP-Adresse für jeden Knoten innerhalb des Clusters zugegriffen werden. Informationen zum Blockieren des Datenverkehrs an Knotenports während der Verwendung eines NLB-Service finden Sie im Abschnitt [Eingehenden Datenverkehr an Netzlastausgleichsfunktion (NLB) oder NodePort-Services steuern](/docs/containers?topic=containers-network_policies#block_ingress).

Als Nächstes können Sie [einen NLB-Hostnamen registrieren](#loadbalancer_hostname).

<br />


## v2.0: Planungsalgorithmen
{: #scheduling}

Planungsalgorithmen bestimmen, wie eine NLB der Version 2.0 Ihren App-Pods Netzverbindungen zuordnet. Wenn Clientanforderungen in Ihrem Cluster eingehen, leitet die NLB die Anforderungspakete auf der Basis des Planungsalgorithmus an Workerknoten weiter. Um einen Planungsalgorithmus zu verwenden, geben Sie den zugehörigen Keepalived-Kurznamen in der Planungsannotation der Konfigurationsdatei Ihres NLB-Service an: `service.kubernetes.io/ibm-load-balancer-cloud-provider-scheduler: "rr"`. In der folgenden Liste finden Sie die Planungsalgorithmen, die in {{site.data.keyword.containerlong_notm}} unterstützt werden. Wenn Sie keinen Planungsalgorithmus angeben, wird der Round-Robin-Algorithmus standardmäßig verwendet. Weitere Informationen finden Sie in der [Keepalived-Dokumentation ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](http://www.Keepalived.org/doc/scheduling_algorithms.html).
{: shortdesc}

### Unterstützte Planungsalgorithmen
{: #scheduling_supported}

<dl>
<dt>Round Robin (<code>rr</code>)</dt>
<dd>Die NLB durchläuft zyklisch die Liste von App-Pods, wenn Verbindungen an Workerknoten weitergeleitet werden, wobei jeder App-Pod gleich behandelt wird. Round Robin ist der Standardplanungsalgorithmus für NLBs der Version 2.0.</dd>
<dt>Source Hashing (<code>sh</code>)</dt>
<dd>Die NLB generiert einen Hashschlüssel basierend auf der Quellen-IP-Adresse des Clientanforderungspakets. Anschließend sucht die NLB den Hashschlüssel in einer statisch zugeordneten Hashtabelle und leitet die Anforderung an den App-Pod weiter, der die Hashes dieses Bereichs verarbeitet. Dieser Algorithmus stellt sicher, dass Anforderungen von einem bestimmten Client immer an denselben App-Pod übertragen werden.</br>**Note**: Kubernetes verwendet 'iptables'-Regeln, die dafür sorgen, dass Anforderungen an einen zufälligen Pod auf dem Worker gesendet werden. Um diesen Planungsalgorithmus zu verwenden, müssen Sie sicherstellen, dass pro Workerknoten nicht mehr als ein Pod Ihrer App bereitgestellt wird. Beispiel: Wenn alle Pods die Bezeichnung <code>run=&lt;app_name&gt;</code>, haben, fügen Sie die folgende Anti-Affinitäts-Regel zum Abschnitt <code>spec</code> Ihrer App-Bereitstellung hinzu:</br>
<pre class="codeblock">
<code>
    spec:
      affinity:
        podAntiAffinity:
          preferredDuringSchedulingIgnoredDuringExecution:
          - weight: 100
        podAffinityTerm:
          labelSelector:
            matchExpressions:
                - key: run
                  operator: In
                  values:
                  - <APP_NAME>
              topologyKey: kubernetes.io/hostname</code></pre>

Sie finden das vollständige Beispiel in [diesem Blog zu IBM Cloud-Bereitstellungsmustern ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/blogs/bluemix/2018/10/ibm-cloud-kubernetes-service-deployment-patterns-4-multi-zone-cluster-app-exposed-via-loadbalancer-aggregating-whole-region-capacity/).</dd>
</dl>

### Nicht unterstützte Planungsalgorithmen
{: #scheduling_unsupported}

<dl>
<dt>Destination Hashing (<code>dh</code>)</dt>
<dd>Das Ziel des Pakets, d. h. die IP-Adresse und der Port der NLB, wird verwendet, um zu bestimmen, welcher Workerknoten die eingehende Anforderung verarbeitet. Allerdings ändern sich die IP-Adresse und der Port für NLBs in {{site.data.keyword.containerlong_notm}} nicht. Die NLB ist gezwungen, die Anforderung innerhalb desselben Workerknotens zu behalten, auf dem sie sich befindet, sodass nur die App-Pods eines einzelnen Workerknotens alle eingehenden Anforderungen verarbeiten.</dd>
<dt>Dynamische Verbindungszählalgorithmen</dt>
<dd>Die folgenden Algorithmen sind von der dynamischen Zählung der Verbindungen zwischen Clients und NLBs abhängig. Da DSR (Direct Service Return) jedoch verhindert, dass Pods von NLBs der Version 2.0 im Rückgabepaketpfad vorhanden sind, merken sich NLBs keine eingerichteten Verbindungen.<ul>
<li>Least Connection (<code>lc</code>)</li>
<li>Locality-Based Least Connection (<code>lblc</code>)</li>
<li>Locality-Based Least Connection with Replication (<code>lblcr</code>)</li>
<li>Never Queue (<code>nq</code>)</li>
<li>Shortest Expected Delay (<code>seq</code>)</li></ul></dd>
<dt>Gewichtete Pod-Algorithmen</dt>
<dd>Die folgenden Algorithmen hängen von gewichteten App-Pods ab. In {{site.data.keyword.containerlong_notm}} haben aber alle App-Pods dieselbe Gewichtung für den Lastausgleich.<ul>
<li>Weighted Least Connection (<code>wlc</code>)</li>
<li>Weighted Round Robin (<code>wrr</code>)</li></ul></dd></dl>

<br />


## NLB-Hostnamen registrieren
{: #loadbalancer_hostname}

Nachdem Sie Netzlastausgleichsfunktionen (NLBs) konfiguriert haben, können Sie DNS-Einträge für die NLB-IPs erstellen, indem Sie Hostnamen erstellen. Sie können auch TCP/HTTP(S)-Monitore einrichten, die den Status der NLB-IP-Adressen hinter den einzelnen Hostnamen überprüfen.
{: shortdesc}

<dl>
<dt>Hostname</dt>
<dd>Wenn Sie eine öffentliche NLB in einem Einzel- oder Mehrzonencluster erstellen, machen Sie Ihre App dem Internet zugänglich, indem Sie einen Hostnamen für die NLB-IP-Adresse erstellen. Zusätzlich übernimmt {{site.data.keyword.Bluemix_notm}} für Sie die Generierung und Pflege des Platzhalter-SSL-Zertifikats für den Hostnamen. <p>In Mehrzonenclustern können Sie einen Hostnamen erstellen und die NLB-IP-Adresse in jeder Zone zum DNS-Eintrag dieses Hostnamens hinzufügen. Wenn Sie beispielsweise NLBs für Ihre App in drei Zonen in der Region 'Vereinigte Staaten (Süden)' bereitgestellt haben, können Sie den Hostnamen `mycluster-a1b2cdef345678g9hi012j3kl4567890-0001.us-south.containers.appdomain.cloud` für die drei NLB-IP-Adressen erstellen. Wenn ein Benutzer auf den Hostnamen Ihrer App zugreift, greift der Client zufällig auf eine dieser IPs zu und die Anforderung wird an diese NLB gesendet. </p>
Beachten Sie, dass Sie derzeit keine Hostnamen für private NLBs erstellen können.</dd>
<dt>Statusprüfmonitor</dt>
<dd>Aktivieren Sie Statusprüfungen für die NLB-IP-Adressen hinter einem einzelnen Hostnamen, um festzustellen, ob sie verfügbar sind. Wenn Sie einen Monitor für Ihren Hostnamen aktivieren, wird damit die NLB-IP überwacht und die Ergebnisse der DNS-Suche werden auf Grundlage dieser Statusprüfungen aktualisiert. Wenn Ihre NLBs beispielsweise die IP-Adressen `1.1.1.1`, `2.2.2.2` und `3.3.3.3` haben, gibt eine normale Operation der DNS-Suche nach Ihrem Hostnamen alle drei IPs zurück, von denen der Client zufällig auf eine zugreift. Wenn die NLB mit der IP-Adresse `3.3.3.3` aus irgendeinem Grund nicht verfügbar ist, z. B. wegen eines Zonenfehlers, dann schlägt die Statusprüfung für diese IP fehl. Der Monitor entfernt die fehlgeschlagene IP aus dem Hostnamen und die DNS-Suche gibt nur die einwandfreien IPs `1.1.1.1` und `2.2.2.2` zurück.</dd>
</dl>

Sie können alle Hostnamen anzeigen, die für NLB-IPs in Ihrem Cluster registriert sind, indem Sie den folgenden Befehl ausführen.
```
ibmcloud ks nlb-dnss --cluster <clustername_oder_-id>
```
{: pre}

</br>

### NLB-IPs mit einem DNS-Hostnamen registrieren
{: #loadbalancer_hostname_dns}

Machen Sie Ihre App dem öffentlichen Internet verfügbar, indem Sie einen Hostnamen für die IP-Adresse der Netzlastausgleichsfunktion (NLB) erstellen.
{: shortdesc}

Vorbereitende Schritte:
* Überprüfen Sie die folgenden Aspekte und Einschränkungen.
  * Sie können Hostnamen für öffentliche NLBs der Version 1.0 und 2.0 erstellen.
  * Sie können derzeit keine Hostnamen für private NLBs erstellen.
  * Sie können bis zu 128 Host-Namen registrieren. Diese Begrenzung kann auf Anforderung aufgehoben werden, indem ein [Supportfall](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support) geöffnet wird.
* [Erstellen Sie eine NLB für Ihre App in einem Einzelzonencluster](#lb_config) oder [erstellen Sie NLBs in jeder Zone eines Mehrzonenclusters](#multi_zone_config).

Gehen Sie wie folgt vor, um einen Hostnamen für eine oder mehrere NLB-IP-Adressen zu erstellen:

1. Rufen Sie die Adresse **EXTERNAL-IP** für Ihre NLB ab. Wenn Sie in jeder Zone eines Mehrzonenclusters eine NLB haben, die eine App zugänglich machen, rufen Sie die IPs für jede NLB ab.
  ```
  kubectl get svc
  ```
  {: pre}

  In der folgenden Beispielausgabe sind die externen IPs (**EXTERNAL-IP**) der NLB `168.2.4.5` und `88.2.4.5`.
  ```
  NAME             TYPE           CLUSTER-IP       EXTERNAL-IP       PORT(S)                AGE
  lb-myapp-dal10   LoadBalancer   172.21.xxx.xxx   168.2.4.5         1883:30303/TCP         6d
  lb-myapp-dal12   LoadBalancer   172.21.xxx.xxx   88.2.4.5          1883:31303/TCP         6d
  ```
  {: screen}

2. Registrieren Sie die IP, indem Sie einen DNS-Hostnamen erstellen. Beachten Sie, dass Sie zunächst den Hostnamen mit nur einer IP-Adresse erstellen können. 
  ```
  ibmcloud ks nlb-dns-create --cluster <clustername_oder_-id> --ip <NLB_IP>
  ```
  {: pre}

3. Stellen Sie sicher, dass der Hostname erstellt wurde.
  ```
  ibmcloud ks nlb-dnss --cluster <clustername_oder_-id>
  ```
  {: pre}

  Beispielausgabe:
  ```
  Hostname                                                                                IP(s)              Health Monitor   SSL Cert Status           SSL Cert Secret Name
  mycluster-a1b2cdef345678g9hi012j3kl4567890-0001.us-south.containers.appdomain.cloud     ["168.2.4.5"]      None             created                   <certificate>
  ```
  {: screen}

4. Wenn in einem Mehrzonencluster in jeder Zone eine NLB vorhanden sind, die eine App zugänglich macht, fügen Sie die IPs der anderen NLBs zum Hostnamen hinzu. Beachten Sie, dass Sie den folgenden Befehl für jede IP-Adresse ausführen müssen, die hinzugefügt werden soll.
  ```
  ibmcloud ks nlb-dns-add --cluster <clustername_oder_-id> --ip <IP-adresse> --nlb-host <hostname>
  ```
  {: pre}

5. Stellen Sie sicher, dass die IPs mit Ihrem Hostnamen registriert sind, indem Sie eine Suche nach `host` oder `ns` durchführen.
  Beispielbefehl:
  ```
  host mycluster-a1b2cdef345678g9hi012j3kl4567890-0001.us-south.containers.appdomain.cloud
  ```
  {: pre}

  Beispielausgabe:
  ```
  mycluster-a1b2cdef345678g9hi012j3kl4567890-0001.us-south.containers.appdomain.cloud has address 88.2.4.5  
  mycluster-a1b2cdef345678g9hi012j3kl4567890-0001.us-south.containers.appdomain.cloud has address 168.2.4.5
  ```
  {: screen}

6. Geben Sie in einem Web-Browser die ULR ein, um über den von Ihnen erstellten Hostnamen auf Ihre App zuzugreifen.

Als Nächstes können Sie [Statusprüfungen für einen Hostnamen durch Erstellen eines Statusmonitors aktivieren](#loadbalancer_hostname_monitor).

</br>

### Informationen zum Hostnamensformat
{: #loadbalancer_hostname_format}

Hostnamen für NLBs folgen dem folgenden Format: `<cluster_name>-<globally_unique_account_HASH>-0001.<region>.containers.appdomain.cloud`.
{: shortdesc}

Zum Beispiel kann ein Hostname, den Sie für eine NLB erstellen, etwa folgendermaßen aussehen: `mycluster-a1b2cdef345678g9hi012j3kl4567890-0001.us-south.containers.appdomain.cloud`. In der folgenden Tabelle sind die einzelnen Komponenten des Hostnamens beschrieben.

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Informationen zum Format des LB-Hostnamens</th>
</thead>
<tbody>
<tr>
<td><code>&lt;clustername&gt;</code></td>
<td>Der Name Ihres Clusters.
<ul><li>Wenn der Clustername maximal 26 Zeichen enthält, wird der ganze Clustername eingeschlossen und nicht geändert: <code>myclustername</code>.</li>
<li>Enthält der Clustername 26 oder mehr Zeichen und ist er innerhalb dieser Region eindeutig, werden nur die ersten 24 Zeichen des Clusternamens verwendet: <code>myveryverylongclusternam</code>.</li>
<li>Enthält der Clustername 26 oder mehr Zeichen und es existiert innerhalb dieser Region ein weiterer Cluster desselben Namens, werden nur die ersten 17 Zeichen des Clusternamens verwendet und ein Strich mit 6 zufällig ausgewählten Zeichen wird hinzugefügt: <code>myveryverylongclu-ABC123</code>.</li></ul>
</td>
</tr>
<tr>
<td><code>&lt;globally_unique_account_HASH&gt;</code></td>
<td>Ein global eindeutiger Hash wird für Ihr {{site.data.keyword.Bluemix_notm}}-Konto erstellt. Alle Hostnamen, die Sie für NLBs in Clustern in Ihrem Konto erstellen, verwenden diesen global eindeutigen Hash.</td>
</tr>
<tr>
<td><code>0001</code></td>
<td>
Das erste und zweite Zeichen, <code>00</code>, geben den Namen eines öffentlichen Hosts an. Das dritte und das vierte Zeichen, z. B. <code>01</code> oder eine andere Zahl, dienen als Zähler für jeden Hostnamen, den Sie erstellen.</td>
</tr>
<tr>
<td><code>&lt;region&gt;</code></td>
<td>Die Region, in der der Cluster erstellt wird.</td>
</tr>
<tr>
<td><code>containers.appdomain.cloud</code></td>
<td>Die Unterdomäne für {{site.data.keyword.containerlong_notm}}-Hostnamen.</td>
</tr>
</tbody>
</table>

</br>

### Statusprüfungen für einen Hostnamen durch Erstellen eines Statusprüfmonitors aktivieren
{: #loadbalancer_hostname_monitor}

Aktivieren Sie Statusprüfungen für die NLB-IP-Adressen hinter einem einzelnen Hostnamen, um festzustellen, ob sie verfügbar sind.
{: shortdesc}

Bevor Sie beginnen, [registrieren Sie NLB-IPs mit einem DNS-Hostnamen](#loadbalancer_hostname_dns).

1. Rufen Sie den Namen Ihres Hostnamens ab. Beachten Sie in der Ausgabe, dass der Host den Monitor-**Status** `Unkonfiguriert` aufweist.
  ```
  ibmcloud ks nlb-dns-monitor-ls --cluster <clustername_oder_-id>
  ```
  {: pre}

  Beispielausgabe:
  ```
  Hostname                                                                                   Status         Type    Port   Path
  mycluster-a1b2cdef345678g9hi012j3kl4567890-0001.us-south.containers.appdomain.cloud        Unconfigured   N/A     0      N/A
  ```
  {: screen}

2. Erstellen Sie einen Statusprüfmonitor für den Hostnamen. Wenn Sie keinen Konfigurationsparameter einschließen, wird der Standardwert verwendet.
  ```
  ibmcloud ks nlb-dns-monitor-configure --cluster <cluster_name_or_id> --nlb-host <host_name> --enable --desc <description> --type <type> --method <method> --path <path> --timeout <timeout> --retries <retries> --interval <interval> --port <port> --expected-body <expected-body> --expected-codes <expected-codes> --follows-redirects <true> --allows-insecure <true>
  ```
  {: pre}

  <table>
  <caption>Erklärung der Bestandteile dieses Befehls</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der Bestandteile dieses Befehls</th>
  </thead>
  <tbody>
  <tr>
  <td><code>--cluster &lt;clustername_oder_-id&gt;</code></td>
  <td>Erforderlich: Der Name oder die ID des Clusters, in dem der Hostname registriert ist.</td>
  </tr>
  <tr>
  <td><code>--nlb-host &lt;hostname&gt;</code></td>
  <td>Erforderlich: Der Hostname, für den ein Statusprüfmonitor aktiviert werden soll.</td>
  </tr>
  <tr>
  <td><code>--enable</code></td>
  <td>Erforderlich: Aktivieren Sie den Statusprüfmonitor für den Hostnamen.</td>
  </tr>
  <tr>
  <td><code>--description &lt;description&gt;</code></td>
  <td>Eine Beschreibung des Statusprüfmonitors.</td>
  </tr>
  <tr>
  <td><code>--type &lt;type&gt;</code></td>
  <td>Das für die Statusprüfung zu verwendende Protokoll: <code>HTTP</code>, <code>HTTPS</code> oder <code>TCP</code>. Standardwert: <code>HTTP</code></td>
  </tr>
  <tr>
  <td><code>--method &lt;method&gt;</code></td>
  <td>Die für die Statusprüfung zu verwendende Methode. Standardwert für <code>type</code> <code>HTTP</code> und <code>HTTPS</code>: <code>GET</code>. Standardwert für <code>type</code> <code>TCP</code>: <code>connection_established</code></td>
  </tr>
  <tr>
  <td><code>--path &lt;path&gt;</code></td>
  <td>Wenn <code>type</code> auf <code>HTTPS</code> festgelegt ist: der Endpunktpfad, anhand dessen die Statusprüfung erfolgt. Standardwert: <code>/</code></td>
  </tr>
  <tr>
  <td><code>--timeout &lt;timeout&gt;</code></td>
  <td>Das Zeitlimit in Sekunden, bevor die IP-Adresse als fehlerhaft eingestuft wird. Standardwert: <code>5</code></td>
  </tr>
  <tr>
  <td><code>--retries &lt;retries&gt;</code></td>
  <td>Bei einer Zeitlimitüberschreitung ist dies die Anzahl der Versuche, bis die IP als fehlerhaft eingestuft wird. Neuversuche werden sofort ausgeführt. Standardwert: <code>2</code></td>
  </tr>
  <tr>
  <td><code>--interval &lt;interval&gt;</code></td>
  <td>Das Intervall (in Sekunden) zwischen den einzelnen Statusprüfungen. Kurze Intervalle können die Failover-Zeit verbessern, erhöhen allerdings die Auslastung der IPs. Standardwert: <code>60</code></td>
  </tr>
  <tr>
  <td><code>--port &lt;port&gt;</code></td>
  <td>Die Portnummer, zu der eine Verbindung zwecks Statusprüfung hergestellt werden soll. Wenn <code>type</code> auf <code>TCP</code> gesetzt ist, ist dieser Parameter erforderlich. Wenn <code>type</code> auf <code>HTTP</code> oder <code>HTTPS</code> gesetzt ist, definieren Sie den Port nur dann, wenn Sie einen anderen Port als Port 80 für HTTP bzw. 443 für HTTPS verwenden. Standardwert für TCP: <code>0</code>. Standardwert für HTTP: <code>80</code>. Standardwert für HTTPS: <code>443</code>.</td>
  </tr>
  <tr>
  <td><code>--expected-body &lt;expected-body&gt;</code></td>
  <td>Wenn <code>type</code> auf <code>HTTP</code> oder <code>HTTPS</code> gesetzt ist: eine Unterzeichenfolge ohne Beachtung der Groß-/Kleinschreibung, nach der die Statusprüfung im Antworthauptteil sucht. Wenn diese Zeichenfolge nicht gefunden wird, wird die IP-Adresse als fehlerhaft eingestuft.</td>
  </tr>
  <tr>
  <td><code>--expected-codes &lt;expected-codes&gt;</code></td>
  <td>Wenn <code>type</code> auf <code>HTTP</code> oder <code>HTTPS</code> gesetzt ist: HTTP-Codes, nach denen die Statusprüfung in der Antwort sucht. Wenn der HTTP-Code nicht gefunden wird, wird die IP-Adresse als fehlerhaft eingestuft. Standardwert: <code>2xx</code></td>
  </tr>
  <tr>
  <td><code>--allows-insecure &lt;true&gt;</code></td>
  <td>Wenn <code>type</code> auf <code>HTTP</code> oder <code>HTTPS</code> gesetzt ist: auf <code>true</code> setzen, damit das Zertifikat nicht validiert wird.</td>
  </tr>
  <tr>
  <td><code>--follows-redirects &lt;true&gt;</code></td>
  <td>Wenn <code>type</code> auf <code>HTTP</code> oder <code>HTTPS</code> gesetzt ist: auf <code>true</code> setzen, um allen Weiterleitungen zu folgen, die von der IP zurückgegeben werden.</td>
  </tr>
  </tbody>
  </table>

  Beispielbefehl:
  ```
  ibmcloud ks nlb-dns-monitor-configure --cluster mycluster --nlb-host mycluster-a1b2cdef345678g9hi012j3kl4567890-0001.us-south.containers.appdomain.cloud --enable --desc "Login page monitor" --type HTTPS --method GET --path / --timeout 5 --retries 2 --interval 60 --expected-body "healthy" --expected-codes 2xx --follows-redirects true
  ```
  {: pre}

3. Überprüfen Sie, ob der Statusprüfmonitor mit den korrekten Einstellungen konfiguriert ist.
  ```
  ibmcloud ks nlb-dns-monitor-get --cluster <clustername_oder_-id> --nlb-host <hostname>
  ```
  {: pre}

  Beispielausgabe:
  ```
  <placeholder - still want to test this one>
  ```
  {: screen}

4. Zeigen Sie den Status der Statusprüfung für die NLB-IPs hinter Ihrem Hostnamen an.
  ```
  ibmcloud ks nlb-dns-monitor-status --cluster <clustername_oder_-id> --nlb-host <hostname>
  ```
  {: pre}

  Beispielausgabe:
  ```
  Hostname                                                                                IP          Health Monitor   H.Monitor Status
  mycluster-a1b2cdef345678g9hi012j3kl4567890-0001.us-south.containers.appdomain.cloud     168.2.4.5   Enabled          Healthy
  mycluster-a1b2cdef345678g9hi012j3kl4567890-0001.us-south.containers.appdomain.cloud     88.2.4.5    Enabled          Healthy
  ```
  {: screen}

</br>

### IPs- und Monitore aktualisieren und von Hostnamen entfernen
{: #loadbalancer_hostname_delete}

Sie können NLB-IP-Adressen zu Hostnamen, die Sie generiert haben, hinzufügen und aus diesen entfernen. Sie können bei Bedarf auch Statusprüfmonitore für Hostnamen inaktivieren und aktivieren.
{: shortdesc}

**NLB-IPs**

Wenn Sie später weitere NLBs in anderen Zonen Ihres Clusters hinzufügen, um dieselbe App zugänglich zu machen, können Sie die NLB-IPs dem vorhandenen Hostnamen hinzufügen. Beachten Sie, dass Sie den folgenden Befehl für jede IP-Adresse ausführen müssen, die hinzugefügt werden soll.
```
ibmcloud ks nlb-dns-add --cluster <cluster_name_or_id> --ip <IP_address> --nlb-host <host_name>
```
{: pre}

Sie können auch IP-Adressen von NLBs entfernen, die nicht mehr bei einem Hostnamen registriert sein sollen. Beachten Sie, dass Sie den folgenden Befehl für jede IP-Adresse ausführen müssen, die Sie entfernen möchten. Wenn Sie alle IPs aus einem Hostnamen entfernen, ist der Hostname immer noch vorhanden, nur sind ihm keine IPs zugeordnet. 
```
ibmcloud ks nlb-dns-rm --cluster <clustername_oder_-id> --ip <ip1,ip2> --nlb-host <hostname>
```
{: pre}

</br>

**Statusprüfmonitore**

Wenn Sie die Konfiguration Ihres Statusprüfmonitors ändern müssen, können Sie einzelne Einstellungen ändern. Schließen Sie Flags nur für die Einstellungen ein, die geändert werden sollen.
```
ibmcloud ks nlb-dns-monitor-configure --cluster <cluster_name_or_id> --nlb-host <host_name> --desc <description> --type <type> --method <method> --path <path> --timeout <timeout> --retries <retries> --interval <interval> --port <port> --expected-body <expected-body> --expected-codes <expected-codes> --follows-redirects <true> --allows-insecure <true>
```
{: pre}

Sie können den Statusprüfmonitor für einen Hostnamen jederzeit inaktivieren, indem Sie den folgenden Befehl ausführen:
```
ibmcloud ks nlb-dns-monitor-disable --cluster <clustername_oder_-id> --nlb-host <hostname>
```
{: pre}

Führen Sie den folgenden Befehl aus, um die Überwachung eines Hostnamens erneut zu aktivieren:
```
ibmcloud ks nlb-dns-monitor-enable --cluster <clustername_oder_-id> --nlb-host <hostname>
```
{: pre}
