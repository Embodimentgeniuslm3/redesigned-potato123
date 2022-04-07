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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}



# Fehlerbehebung für Clusternetze
{: #cs_troubleshoot_network}

Ziehen Sie bei der Verwendung von {{site.data.keyword.containerlong}} die folgenden Verfahren für die Fehlerbehebung für Clusternetze in Betracht.
{: shortdesc}

Haben Sie Schwierigkeiten, über Ingress eine Verbindung zu Ihrer App herzustellen? Versuchen Sie, [Ingress zu debuggen](/docs/containers?topic=containers-cs_troubleshoot_debug_ingress).
{: tip}

Bei der Fehlerbehebung können Sie [{{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool](/docs/containers?topic=containers-cs_troubleshoot#debug_utility) verwenden, um Tests durchzuführen und relevante Informationen zum Netzbetrieb sowie StrongSwan-Informationen aus Ihrem Cluster zu erfassen.
{: tip}

## Verbindung mit einer App über einen Netzausgleichsfunktions- (NLB-) Service kann nicht hergestellt werden
{: #cs_loadbalancer_fails}

{: tsSymptoms}
Sie haben Ihre App öffentlich zugänglich gemacht, indem Sie einen NLB-Service in Ihrem Cluster erstellt haben. Als Sie versuchten, eine Verbindung mit Ihrer App über die öffentliche IP-Adresse der NLB herzustellen, ist die Verbindung fehlgeschlagen oder sie hat das zulässige Zeitlimit überschritten.

{: tsCauses}
Ihr NLB-Service funktioniert möglicherweise aus einem der folgenden Gründe nicht ordnungsgemäß:

-   Der Cluster ist ein kostenloser Cluster oder Standardcluster mit nur einem Workerknoten.
-   Der Cluster ist noch nicht vollständig bereitgestellt.
-   Das Konfigurationsscript für Ihren NLB-Service enthält Fehler.

{: tsResolve}
Gehen Sie wie folgt vor, um Fehler in Ihrem NLB-Service zu beheben:

1.  Prüfen Sie, ob Sie einen Standardcluster einrichten, der vollständig bereitgestellt ist und mindestens zwei Workerknoten umfasst, um Hochverfügbarkeit für Ihren NLB-Service sicherzustellen.

  ```
  ibmcloud ks workers --cluster <clustername_oder_-id>
  ```
  {: pre}

    Stellen Sie in Ihrer CLI-Ausgabe sicher, dass der **Status** Ihrer Workerknoten **Ready** (bereit) lautet und dass für **Machine Type** (Maschinentyp) etwas anderes als **free** (frei) anzeigt wird.

2. Für NLBs der Version 2.0: Stellen Sie sicher, dass Sie die [Voraussetzungen für die NLB der Version 2.0](/docs/containers?topic=containers-loadbalancer#ipvs_provision) erfüllen.

3. Prüfen Sie die Richtigkeit der Konfigurationsdatei für Ihren NLB-Service.
    * NLBs der Version 2.0:
        ```
        apiVersion: v1
        kind: Service
        metadata:
          name: myservice
          annotations:
            service.kubernetes.io/ibm-load-balancer-cloud-provider-enable-features: "ipvs"
        spec:
          type: LoadBalancer
          selector:
            <selektorschlüssel>:<selektorwert>
          ports:
           - protocol: TCP
             port: 8080
          externalTrafficPolicy: Local
        ```
        {: screen}

        1. Überprüfen Sie, dass Sie **LoadBalancer** als Typ für Ihren Service definiert haben.
        2. Überprüfen Sie, dass Sie die Annotation `service.kubernetes.io/ibm-load-balancer-cloud-provider-enable-features: "ipvs"` eingeschlossen haben.
        3. Stellen Sie im Abschnitt `spec.selector` des LoadBalancer-Service sicher, dass `<selektorschlüssel>` und `<selektorwert>` mit dem Schlüssel/Wert-Paar übereinstimmen, das Sie im Abschnitt `spec.template.metadata.labels` Ihrer YAML-Bereitstellungsdatei verwendet haben. Wenn die Bezeichnungen nicht übereinstimmen, zeigt der Abschnitt zu den Endpunkten (**Endpoints**) in Ihrem LoadBalancer-Service **<none>** an und Ihre App ist über das Internet nicht zugänglich.
        4. Überprüfen Sie, ob Sie den **Port** verwendet haben, den Ihre App überwacht.
        5. Überprüfen Sie, dass `externalTrafficPolicy` auf `Local` gesetzt ist.

    * NLBs der Version 1.0:
        ```
        apiVersion: v1
    kind: Service
    metadata:
      name: myservice
    spec:
      type: LoadBalancer
      selector:
        <selektorschlüssel>:<selektorwert>
      ports:
           - protocol: TCP
             port: 8080
        ```
        {: screen}

        1. Überprüfen Sie, dass Sie **LoadBalancer** als Typ für Ihren Service definiert haben.
        2. Stellen Sie im Abschnitt `spec.selector` des LoadBalancer-Service sicher, dass `<selektorschlüssel>` und `<selektorwert>` mit dem Schlüssel/Wert-Paar übereinstimmen, das Sie im Abschnitt `spec.template.metadata.labels` Ihrer YAML-Bereitstellungsdatei verwendet haben. Wenn die Bezeichnungen nicht übereinstimmen, zeigt der Abschnitt zu den Endpunkten (**Endpoints**) in Ihrem LoadBalancer-Service **<none>** an und Ihre App ist über das Internet nicht zugänglich.
        3. Überprüfen Sie, ob Sie den **Port** verwendet haben, den Ihre App überwacht.

3.  Prüfen Sie Ihren NLB-Service und suchen Sie im Abschnitt zu den Ereignissen (**Events**) nach möglichen Fehlern.

    ```
    kubectl describe service <mein_service>
    ```
    {: pre}

    Suchen Sie nach den folgenden Fehlernachrichten:

    <ul><li><pre class="screen"><code>Clusters with one node must use services of type NodePort</code></pre></br>Um den NLB-Service verwenden zu können, müssen Sie über einen Standardcluster mit mindestens zwei Workerknoten verfügen.</li>
    <li><pre class="screen"><code>No cloud provider IPs are available to fulfill the NLB service request. Add a portable subnet to the cluster and try again</code></pre></br>Diese Fehlernachricht weist darauf hin, dass keine portierbaren öffentlichen IP-Adressen mehr verfügbar sind, die Ihrem NLB-Service zugeordnet werden können. Informationen zum Anfordern portierbarer IP-Adressen für Ihren Cluster finden Sie unter <a href="/docs/containers?topic=containers-subnets#subnets">Clustern Teilnetze hinzufügen</a>. Nachdem portierbare öffentliche IP-Adressen im Cluster verfügbar gemacht wurden, wird der NLB-Service automatisch erstellt.</li>
    <li><pre class="screen"><code>Requested cloud provider IP <cloud-provider-ip> is not available. The following cloud provider IPs are available: <available-cloud-provider-ips></code></pre></br>Sie haben eine portierbare öffentliche IP-Adresse für Ihre Lastausgleichsfunktions-YAML mithilfe des Abschnitts **`loadBalancerIP`** definiert, aber diese portierbare öffentliche IP-Adresse ist in Ihrem portierbaren öffentlichen Teilnetz nicht verfügbar. Entfernen Sie im Abschnitt **`loadBalancerIP`** des Konfigurationsscripts die vorhandene IP-Adresse und fügen Sie eine der vorhandenen portierbaren öffentlichen IP-Adressen hinzu. Sie können auch den Abschnitt **`loadBalancerIP`** aus Ihrem Script entfernen, damit eine verfügbare portierbare öffentliche IP-Adresse automatisch zugeordnet werden kann.</li>
    <li><pre class="screen"><code>No available nodes for NLB services</code></pre>Sie verfügen nicht über ausreichend Workerknoten, um einen NLB-Service bereitzustellen. Ein Grund kann sein, dass Sie einen Standardcluster mit mehr als einem Workerknoten bereitgestellt haben, aber die Bereitstellung der Workerknoten ist fehlgeschlagen.</li>
    <ol><li>Verfügbare Workerknoten auflisten.</br><pre class="pre"><code>kubectl get nodes</code></pre></li>
    <li>Werden mindestens zwei verfügbare Workerknoten gefunden, listen Sie die Details der Workerknoten auf.</br><pre class="pre"><code>ibmcloud ks worker-get --cluster &lt;clustername_oder_-id&gt; --worker &lt;worker-id&gt;</code></pre></li>
    <li>Stellen Sie sicher, dass die öffentlichen und privaten VLAN-IDs für die Workerknoten, die von den Befehlen <code>kubectl get nodes</code> und <code>ibmcloud ks worker-get</code> zurückgegeben wurden, übereinstimmen.</li></ol></li></ul>

4.  Wenn Sie eine angepasste Domäne verwenden, um Ihren NLB-Service zu verbinden, stellen Sie sicher, dass Ihre angepasste Domäne der öffentlichen IP-Adresse Ihres NLB-Service zugeordnet ist.
    1.  Suchen Sie nach der öffentlichen IP-Adresse Ihres NLB-Service.
        ```
        kubectl describe service <servicename> | grep "LoadBalancer Ingress"
        ```
        {: pre}

    2.  Prüfen Sie, ob Ihre angepasste Domäne der portierbaren öffentlichen IP-Adresse Ihres NLB-Service im Zeigerdatensatz (PTR) zugeordnet ist.

<br />


## Verbindung mit einer App über Ingress kann nicht hergestellt werden
{: #cs_ingress_fails}

{: tsSymptoms}
Sie haben Ihre App öffentlich zugänglich gemacht, indem Sie eine Ingress-Ressource für Ihre App in Ihrem Cluster erstellt haben. Als Sie versuchten, eine Verbindung mit Ihrer App über die öffentliche IP-Adresse oder Unterdomäne der Ingress-Lastausgleichsfunktion für Anwendungen (ALB) herzustellen, ist die Verbindung fehlgeschlagen oder sie hat das zulässige Zeitlimit überschritten.

{: tsResolve}
Überprüfen Sie zunächst, ob Ihr Cluster vollständig bereitgestellt ist und mindestens zwei Workerknoten pro Zone verfügbar sind, um eine hohe Verfügbarkeit für Ihren ALB sicherzustellen.
```
ibmcloud ks workers --cluster <clustername_oder_-id>
```
{: pre}

Stellen Sie in Ihrer CLI-Ausgabe sicher, dass der **Status** Ihrer Workerknoten **Ready** (bereit) lautet und dass für **Machine Type** (Maschinentyp) etwas anderes als **free** (frei) anzeigt wird.

* Wenn Ihr Standardcluster vollständig bereitgestellt ist und mindestens 2 Workerknoten pro Zone aufweist, aber keine **Ingress-Unterdomäne** verfügbar ist, finden Sie weitere Informationen hierzu im Abschnitt [Es kann keine Unterdomäne für die Ingress-ALB abgerufen werden](/docs/containers?topic=containers-cs_troubleshoot_network#cs_subnet_limit).
* Bei anderen Problemen können Sie die Ingress-Konfigurationsfehler beheben, indem Sie die Schritte im Abschnitt [Debugging für Ingress](/docs/containers?topic=containers-cs_troubleshoot_debug_ingress) befolgen.

<br />


## Fehler beim geheimen Schlüssel für die Ingress-Lastausgleichsfunktion für Anwendungen (ALB)
{: #cs_albsecret_fails}

{: tsSymptoms}
Nach der Bereitstellung eines geheimen Schlüssels für die Ingress-Lastausgleichsfunktion für Anwendungen (ALB) in Ihrem Cluster durch den Befehl `ibmcloud ks alb-cert-deploy` wird das Beschreibungsfeld `Description` nicht mit dem Namen des geheimen Schlüssels aktualisiert, wenn Sie Ihr Zertifikat in {{site.data.keyword.cloudcerts_full_notm}} anzeigen.

Beim Auflisten von Informationen zum geheimen Schlüssel der Lastausgleichsfunktion für Anwendungen wird der Status `*_failed` angezeigt. Beispiel: `create_failed`, `update_failed` oder `delete_failed`.

{: tsResolve}
Überprüfen Sie die folgenden möglichen Ursachen für ein Fehlschlagen des geheimen Schlüssels der Lastausgleichsfunktion für Anwendungen sowie die entsprechenden Fehlerbehebungsschritte:

<table>
<caption>Fehlerbehebung für geheime Schlüssel der Ingress-Lastausgleichsfunktion für Anwendungen (Ingress-ALB)</caption>
 <thead>
 <th>Mögliche Ursache</th>
 <th>Fehlerbehebungsmaßnahme</th>
 </thead>
 <tbody>
 <tr>
 <td>Sie verfügen nicht über die erforderlichen Zugriffsrollen für das Herunterladen und Aktualisieren von Zertifikatsdaten.</td>
 <td>Bitten Sie Ihren Kontoadministrator, Ihnen die folgenden {{site.data.keyword.Bluemix_notm}} IAM-Rollen zuzuweisen:<ul><li>Die Servicerollen **Manager** und **Schreibberechtigter** für Ihre {{site.data.keyword.cloudcerts_full_notm}}-Instanz. Weitere Informationen finden Sie unter <a href="/docs/services/certificate-manager?topic=certificate-manager-managing-service-access-roles#managing-service-access-roles">Servicezugriff verwalten</a> für {{site.data.keyword.cloudcerts_short}}.</li><li>Die <a href="/docs/containers?topic=containers-users#platform">Plattformrolle **Administrator**</a> für den Cluster.</li></ul></td>
 </tr>
 <tr>
 <td>Die CRN des Zertifikats, die zum Zeitpunkt der Erstellung, Aktualisierung oder Entfernung angegeben wurde, gehört nicht zu demselben Konto wie der Cluster.</td>
 <td>Vergewissern Sie sich, dass die von Ihnen angegebene CRN des Zertifikats in eine Instanz des {{site.data.keyword.cloudcerts_short}}-Service importiert wurde, die in demselben Konto wie Ihr Cluster bereitgestellt wird.</td>
 </tr>
 <tr>
 <td>Die zum Zeitpunkt der Erstellung angegebene CRN des Zertifikats ist falsch.</td>
 <td><ol><li>Überprüfen Sie die Richtigkeit der von Ihnen angegebenen Zeichenfolge für die CRN des Zertifikats.</li><li>Falls die CRN des Zertifikats als korrekt befunden wird, versuchen Sie den geheimen Schlüssel <code>ibmcloud ks alb-cert-deploy --update --cluster &lt;clustername_oder_-id&gt; --secret-name &lt;name_des_geheimen_schlüssels&gt; --cert-crn &lt;crn_des_zertifikats&gt;</code> zu aktualisieren.</li><li>Wenn dieser Befehl zum Status <code>update_failed</code> führt, entfernen Sie den geheimen Schlüssel: <code>ibmcloud ks alb-cert-rm --cluster &lt;clustername_oder_-id&gt; --secret-name &lt;name_des_geheimen_schlüssels&gt;</code></li><li>Stellen Sie den geheimen Schlüssel erneut bereit: <code>ibmcloud ks alb-cert-deploy --cluster &lt;clustername_oder_-id&gt; --secret-name &lt;name_des_geheimen_schlüssels&gt; --cert-crn &lt;crn_des_zertifikats&gt;</code></li></ol></td>
 </tr>
 <tr>
 <td>Die zum Zeitpunkt der Aktualisierung angegebene CRN des Zertifikats ist falsch.</td>
 <td><ol><li>Überprüfen Sie die Richtigkeit der von Ihnen angegebenen Zeichenfolge für die CRN des Zertifikats.</li><li>Wenn die CRN des Zertifikats als korrekt befunden wurde, entfernen Sie den geheimen Schlüssel: <code>ibmcloud ks alb-cert-rm --cluster &lt;clustername_oder_-id&gt; --secret-name &lt;name_des_geheimen_schlüssels&gt;</code></li><li>Stellen Sie den geheimen Schlüssel erneut bereit: <code>ibmcloud ks alb-cert-deploy --cluster &lt;clustername_oder_-id&gt; --secret-name &lt;name_des_geheimen_schlüssels&gt; --cert-crn &lt;crn_des_zertifikats&gt;</code></li><li>Versuchen Sie, den geheimen Schlüssel zu aktualisieren: <code>ibmcloud ks alb-cert-deploy --update --cluster &lt;clustername_oder_-id&gt; --secret-name &lt;name_des_geheimen_schlüssels&gt; --cert-crn &lt;crn_des_zertifikats&gt;</code></li></ol></td>
 </tr>
 <tr>
 <td>Der {{site.data.keyword.cloudcerts_long_notm}}-Service ist ausgefallen.</td>
 <td>Vergewissern Sie sich, dass Ihr {{site.data.keyword.cloudcerts_short}}-Service betriebsbereit ist.</td>
 </tr>
 <tr>
 <td>Ihr importierter geheimer Schlüssel hat denselben Namen wie der von IBM bereitgestellte geheime Ingress-Schlüssel.</td>
 <td>Benennen Sie Ihren geheimen Schlüssel um. Sie können den Namen des von IBM bereitgestellten geheimen Ingress-Schlüssels mit dem Befehl `ibmcloud ks cluster-get --cluster <clustername_oder_-id> | grep Ingress` prüfen.</td>
 </tr>
 </tbody></table>

<br />


## Es kann keine Unterdomäne für die Ingress-ALB abgerufen werden
{: #cs_subnet_limit}

{: tsSymptoms}
Wenn Sie den Befehl `ibmcloud ks cluster-get --cluster <cluster>` ausführen, hat Ihr Cluster den Status `normal`, jedoch ist keine Ingress-Unterdomäne (**Ingress Subdomain**) verfügbar.

Es wird möglicherweise eine Fehlernachricht ähnlich der folgenden angezeigt.

```
There are already the maximum number of subnets permitted in this VLAN.
```
{: screen}

{: tsCauses}
Wenn Sie bei Standardclustern in einer Zone zum ersten Mal einen Cluster erstellen, werden in dieser Zone automatisch ein öffentliches und ein privates VLAN für Sie in Ihrem Konto der IBM Cloud-Infrastruktur (SoftLayer) bereitgestellt. In dieser Zone wird ein öffentliches portierbares Teilnetz für das von Ihnen angegebene öffentliche VLAN und ein privates portierbares Teilnetz für das von Ihnen angegebene private VLAN angefordert. Für {{site.data.keyword.containerlong_notm}} haben VLANs ein Limit von 40 Teilnetzen. Wenn das VLAN des Clusters dieses Limit in einer Zone bereits erreicht hat, kann die **Ingress-Unterdomäne** nicht bereitgestellt werden.

Gehen Sie wie folgt vor, um die Anzahl von Teilnetzen eines VLAN anzuzeigen:
1.  Wählen Sie in der [IBM Cloud Infrastruktur-Konsole (SoftLayer)](https://cloud.ibm.com/classic?) die Optionen **Netz** > **IP-Management** > **VLANs** aus.
2.  Klicken Sie auf die **VLAN-Nummer** des VLANs, mit dem Sie Ihren Cluster erstellt haben. Überprüfen Sie den Abschnitt **Teilnetze**, um zu sehen, ob mehr als 40 Teilnetze vorhanden sind.

{: tsResolve}
Wenn Sie ein neues VLAN benötigen, fordern Sie eines an, indem Sie den [{{site.data.keyword.Bluemix_notm}}-Support kontaktieren](/docs/infrastructure/vlans?topic=vlans-ordering-premium-vlans#ordering-premium-vlans). [Erstellen Sie dann einen Cluster](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_create), der dieses neue VLAN verwendet.

Wenn Sie bereits ein verfügbares VLAN haben, können Sie [VLAN-Spanning](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning) in Ihrem vorhandenen Cluster konfigurieren. Anschließend können Sie dem Cluster neue Workerknoten hinzufügen, die das andere VLAN mit verfügbaren Teilnetzen verwenden. Zum Prüfen, ob das VLAN-Spanning bereits aktiviert ist, verwenden Sie den [Befehl](/docs/containers?topic=containers-cs_cli_reference#cs_vlan_spanning_get) `ibmcloud ks vlan-spanning-get`.

Wenn Sie nicht alle Teilnetze im VLAN verwenden, können Sie Teilnetze im Cluster wiederverwenden.
1.  Prüfen Sie, ob die Teilnetze, die Sie verwenden möchten, verfügbar sind.

    Das Infrastrukturkonto, das Sie verwenden, wird möglicherweise von mehreren {{site.data.keyword.Bluemix_notm}}-Konten gemeinsam verwendet. Ist dies der Fall, können Sie nur Informationen zu Ihren Clustern anzeigen, selbst wenn Sie den Befehl `ibmcloud ks subnets` ausführen, um Teilnetze mit **gebundenen Clustern** zu sehen. Besprechen Sie sich mit dem Eigner des Infrastrukturkontos, um sicherzustellen, dass die Teilnetze verfügbar sind und nicht von einem anderen Konto oder Team verwendet werden.
    {: note}

2.  [Erstellen Sie einen Cluster](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_create) mit der Option `--no-subnet`, sodass der Service nicht versucht, neue Teilnetze zu erstellen. Geben Sie die Zone und das VLAN an, in denen sich die für eine Wiederverwendung verfügbaren Teilnetze befinden.

3.  Verwenden Sie den [Befehl](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_subnet_add) `ibmcloud ks cluster-subnet-add`, um vorhandene Teilnetze zu Ihrem Cluster hinzuzufügen. Weitere Informationen finden Sie unter [Angepasste und vorhandene Teilnetze in Kubernetes-Clustern hinzufügen oder daraus entfernen](/docs/containers?topic=containers-subnets#subnets_custom).

<br />


## Ingress-ALB wird in einer Zone nicht bereitgestellt
{: #cs_multizone_subnet_limit}

{: tsSymptoms}
Wenn Sie für ein Mehrzonencluster den Befehl `ibmcloud ks albs <cluster>` ausführen, werden keine ALBs in Zonen bereitgestellt. Wenn Sie z. B. Workerknoten in drei Zonen haben, wird möglicherweise eine Ausgabe ähnlich der folgenden angezeigt, aus der deutlich wird, dass in der dritten Zone keine öffentliche ALB (Lastausgleichsfunktion für Anwendungen) bereitgestellt wurde.
```
ALB ID                                            Status     Type      ALB IP           Zone    Build
private-cr96039a75fddb4ad1a09ced6699c88888-alb1   disabled   private   -                dal10   ingress:350/ingress-auth:192
private-cr96039a75fddb4ad1a09ced6699c88888-alb2   disabled   private   -                dal12   ingress:350/ingress-auth:192
private-cr96039a75fddb4ad1a09ced6699c88888-alb3   disabled   private   -                dal13   ingress:350/ingress-auth:192
public-cr96039a75fddb4ad1a09ced6699c88888-alb1    enabled    public    169.xx.xxx.xxx  dal10   ingress:350/ingress-auth:192
public-cr96039a75fddb4ad1a09ced6699c88888-alb2    enabled    public    169.xx.xxx.xxx  dal12   ingress:350/ingress-auth:192
```
{: screen}

{: tsCauses}
In jeder Zone wird ein öffentliches portierbares Teilnetz für das von Ihnen angegebene öffentliche VLAN und ein privates portierbares Teilnetz für das von Ihnen angegebene private VLAN angefordert. Für {{site.data.keyword.containerlong_notm}} haben VLANs ein Limit von 40 Teilnetzen. Wenn das öffentliche VLAN des Clusters dieses Limit in einer Zone bereits erreicht hat, kann die öffentliche Ingress-ALB für diese Zone nicht bereitgestellt werden.

{: tsResolve}
Informationen zum Überprüfen der Anzahl der Teilnetze in einem VLAN und die Schritte zum Abrufen eines anderen VLANs finden Sie im Abschnitt [Unterdomäne für Ingress-ALB kann nicht abgerufen werden](#cs_subnet_limit).

<br />


## Verbindung über WebSocket schließt nach 60 Sekunden
{: #cs_ingress_websocket}

{: tsSymptoms}
Ihr Ingress-Service macht eine App verfügbar, die ein WebSocket verwendet. Die Verbindung zwischen einem Client und Ihrer WebSocket-App wird jedoch geschlossen, wenn für 60 Sekunden kein Datenverkehr zwischen ihnen übertragen wird.

{: tsCauses}
Die Verbindung zu Ihrer WebSocket-App kann aus einem der folgenden Gründe nach 60 Sekunden Inaktivität unterbrochen werden:

* Ihre Internetverbindung beinhaltet einen Proxy oder eine Firewall, der bzw. die eine lange Verbindung nicht toleriert.
* Eine Zeitlimitüberschreitung im ALB an die WebSocket-App beendet die Verbindung.

{: tsResolve}
Gehen Sie wie folgt vor, um zu verhindern, dass die Verbindung nach 60 Sekunden Inaktivität schließt:

1. Wenn Sie über einen Proxy oder eine Firewall eine Verbindung zu Ihrer WebSocket-App herstellen, müssen Sie sicherstellen, dass der Proxy oder die Firewall nicht so konfiguriert ist, dass lange Verbindungen automatisch beendet werden.

2. Um die Verbindung aktiv zu halten, können Sie den Wert des Zeitlimits erhöhen oder einen Heartbeat (Überwachungssignal) in Ihrer App einrichten.
<dl><dt>Zeitlimit ändern</dt>
<dd>Erhöhen Sie den Wert von `proxy-read-timeout` in der ALB-Konfiguration. Wenn Sie beispielsweise das Zeitlimit `60s` in einen höheren Wert wie `300s` ändern möchten, fügen Sie die folgende [Annotation](/docs/containers?topic=containers-ingress_annotation#connection) zur Ingress-Ressourcendatei hinzu: `ingress.bluemix.net/proxy-read-timeout: "serviceName=<service_name> timeout=300s"`. Das Zeitlimit wird für alle öffentlichen ALBs in Ihrem Cluster geändert.</dd>
<dt>Heartbeat einrichten</dt>
<dd>Wenn Sie den Standardwert für das Lesezeitlimit von ALB nicht ändern möchten, müssen Sie in der WebSocket-App einen Heartbeat einrichten. Wenn Sie ein Heartbeatprotokoll einrichten, indem Sie ein Framework wie [WAMP![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link") ](https://wamp-proto.org/) verwenden, sendet der Upstream-Server der App regelmäßig eine "Ping"-Nachricht in einem vorgegebenen Zeitintervall und der Client antwortet mit einer "Pong" -Nachricht. Legen Sie das Heartbeatintervall auf 58 Sekunden oder weniger fest, sodass der "Ping/Pong"-Datenverkehr die Verbindung offen hält, bevor das 60-Sekunden-Zeitlimit durchgesetzt wird.</dd></dl>

<br />


## Beibehaltung der Quellen-IP schlägt bei Verwendung eines mit einem Taint versehenen Knoten fehl
{: #cs_source_ip_fails}

{: tsSymptoms}
Sie haben die Beibehaltung der Quellen-IP für den Service für die [Lastausgleichsfunktion der Version 1.0](/docs/containers?topic=containers-loadbalancer#node_affinity_tolerations) oder für die [Ingress-ALB](/docs/containers?topic=containers-ingress#preserve_source_ip) durch Ändern des Werts für `externalTrafficPolicy` in `Local` in der Konfigurationsdatei des Service aktiviert. An den Back-End-Service für die App wird jedoch kein Datenverkehr geleitet.

{: tsCauses}
Wenn Sie die Beibehaltung der Quellen-IP für den Lastausgleichsservice oder für die Ingress-ALB (ALB - Lastausgleichsfunktion für Anwendungen) aktivieren, wird die Quellen-IP-Adresse der Clientanforderung beibehalten. Der Datenverkehr wird vom Service nur an App-Pods auf demselben Workerknoten weitergeleitet, um sicherzustellen, dass die IP-Adresse des Anforderungspakets nicht geändert wird. In der Regel werden Pods für den Lastausgleichsservice oder den Ingress-ALB-Service auf den Workerknoten bereitgestellt, auf denen die App-Pods bereitgestellt werden. Es kann jedoch vorkommen, dass die Service-Pods und die App-Pods nicht auf demselben Workerknoten geplant sind. Falls Sie auf den Workerknoten [Kubernetes-Taints ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/) verwenden, wird dadurch verhindert, dass Pods ohne Tolerierungen auf Workerknoten mit beliebigen Bezeichnungen ausgeführt werden. Abhängig vom jeweils verwendeten Taint-Typ kann es vorkommen, dass die Beibehaltung der Quellen-IP nicht funktioniert:

* **Edge-Knoten-Taints:** Sie haben [die Bezeichnung `dedicated=edge`](/docs/containers?topic=containers-edge#edge_nodes) zu mehreren Workerknoten in jedem öffentlichen VLAN im Cluster hinzugefügt, um sicherzustellen, dass die Ingress- und Lastausgleichsfunktions-Pods nur auf diesen Workerknoten bereitgestellt werden. Danach haben Sie auch [auf diese Edge-Knoten ein Taint angewendet](/docs/containers?topic=containers-edge#edge_workloads), um zu vermeiden, dass andere Workloads auf Edge-Knoten ausgeführt werden. Sie haben jedoch keine Affinitätsregel und Tolerierung für den Edge-Knoten zur App-Bereitstellung hinzugefügt. Die App-Pods können nicht auf denselben mit Taints versehenen Knoten geplant werden, auf denen die Service-Pods ausgeführt werden, und der Datenverkehr erreicht nicht den Back-End-Service für die App.

* **Angepasste Taints:** Sie haben angepasste Taints auf mehreren Knoten verwendet, sodass nur App-Pods mit dieser Taint-Tolerierung auf diesen Knoten bereitgestellt werden können. Da Sie Affinitätsregeln und Tolerierungen zu den Bereitstellungen der App und zum Lastausgleichsservice oder den Ingress-Service hinzugefügt haben, können die zugehörigen Pods nur auf diesen Knoten bereitgestellt werden. Durch `ibm-cloud-provider-ip`-`keepalived`-Pods, die automatisch im Namensbereich `ibm-system` erstellt werden, wird jedoch sichergestellt, dass die Pods für die Lastausgleichsfunktion und die App-Pods immer auf demselben Workerknoten geplant werden. Diese `keepalived`-Pods weisen nicht die Tolerierungen für die angepassten Taints auf, die Sie verwendet haben. Sie können nicht auf denselben mit Taints versehenen Knoten geplant werden, auf denen die App-Pods ausgeführt werden, und der Datenverkehr erreicht nicht den Back-End-Service für die App.

{: tsResolve}
Beheben Sie das Problem unter Verwendung einer der beiden Optionen:

* **Edge-Knoten-Taints:** Wenn Sie sicherstellen möchten, dass die Pods für die Lastausgleichsfunktion und die App auf mit Taints versehenen Edge-Knoten bereitgestellt werden, [fügen Sie Affinitätsregeln und Tolerierungen für den Edge-Knoten zur App-Bereitstellung hinzu](/docs/containers?topic=containers-loadbalancer#lb_edge_nodes). Pods für die Lastausgleichsfunktion und Ingress-ALBs weisen diese Affinitätsregeln und Tolerierungen standardmäßig auf.

* **Angepasste Taints:** Entfernen Sie angepasste Taints, für die die `keepalived`-Pods nicht über Tolerierungen verfügen. Stattdessen können Sie [Workerknoten als Edge-Knoten bezeichnen und anschließend Taints auf diese Edge-Knoten anwenden](/docs/containers?topic=containers-edge).

Falls Sie eine der oben aufgeführten Optionen durchgeführt haben, die `keepalived`-Pods aber nicht nicht geplant werden, können Sie weitere Informationen zu `keepalived`-Pods abrufen:

1. Rufen Sie `keepalived`-Pods ab.
    ```
    kubectl get pods -n ibm-system
    ```
    {: pre}

2. Suchen Sie in der Ausgabe nach `ibm-cloud-provider-ip`-Pods, die den **Status** `Pending` aufweisen. Beispiel:
    ```
    ibm-cloud-provider-ip-169-61-XX-XX-55967b5b8c-7zv9t     0/1       Pending   0          2m        <none>          <none>
    ibm-cloud-provider-ip-169-61-XX-XX-55967b5b8c-8ptvg     0/1       Pending   0          2m        <none>          <none>
    ```
    {:screen}

3. Beschreiben Sie jeden `keepalived`-Pod und suchen Sie den Abschnitt zu den Ereignissen (**Events**). Beheben Sie aufgelistete Fehler- oder Warnnachrichten.
    ```
    kubectl describe pod ibm-cloud-provider-ip-169-61-XX-XX-55967b5b8c-7zv9t -n ibm-system
    ```
    {: pre}

<br />


## VPN-Konnektivität kann nicht mit dem StrongSwan-Helm-Diagramm erstellt werden
{: #cs_vpn_fails}

{: tsSymptoms}
Wenn Sie die VPN-Konnektivität überprüfen, indem Sie den Befehl `kubectl exec  $STRONGSWAN_POD -- ipsec status` ausführen, sehen Sie den Status `ESTABLISHED` nicht oder der VPN-Pod befindet sich im Fehlerstatus `ERROR` oder er schlägt immer wieder fehl und startet neu.

{: tsCauses}
Ihre Helm-Diagrammkonfigurationsdatei weist falsche oder fehlende Werte auf oder es liegen Syntaxfehler vor.

{: tsResolve}
Wenn Sie versuchen, VPN-Konnektivität mit dem StrongSwan-Helm-Diagramm zu erstellen, ist es wahrscheinlich, dass der Status beim ersten Mal nicht `ESTABLISHED` lautet. Möglicherweise müssen Sie mehrere Problemtypen überprüfen und Ihre Konfigurationsdatei entsprechend ändern. Gehen Sie wie folgt vor, um Fehler in Ihrer StrongSwan-VPN-Konnektivität zu beheben:

1. [Testen und verifizieren Sie die strongSwan-VPN-Konnektivität](/docs/containers?topic=containers-vpn#vpn_test), indem Sie die fünf Helm-Tests ausführen, die in der StrongSwan-Diagrammdefinition enthalten sind.

2. Wenn Sie die VPN-Konnektivität nach der Ausführung der Helm-Tests nicht herstellen können, können Sie das VPN-Debugging-Tool ausführen, das mit dem VPN-Pod-Image bereitgestellt wird.

    1. Legen Sie die Umgebungsvariable `STRONGSWAN_POD` fest.

        ```
        export STRONGSWAN_POD=$(kubectl get pod -l app=strongswan,release=vpn -o jsonpath='{ .items[0].metadata.name }')
        ```
        {: pre}

    2. Führen Sie das Debugging-Tool aus.

        ```
        kubectl exec  $STRONGSWAN_POD -- vpnDebug
        ```
        {: pre}

        Das Tool gibt bei seiner Ausführung mehrere Seiten mit Informationen aus, da es verschiedene Tests für allgemeine Netzprobleme ausführt. Ausgabezeilen, die mit `ERROR`, `WARNING`, `VERIFY` oder `CHECK` beginnen, weisen auf mögliche Fehler bei der VPN-Verbindung hin.

    <br />


## Neues Release des StrongSwan-Helm-Diagramms kann nicht installiert werden
{: #cs_strongswan_release}

{: tsSymptoms}
Sie ändern Ihr StrongSwan-Helm-Diagramm und versuchen Ihr neues Release zu installieren, indem Sie `helm install -f config.yaml --name=vpn ibm/strongswan` ausführen. Es wird jedoch der folgende Fehler angezeigt:
```
Error: release vpn failed: deployments.extensions "vpn-strongswan" already exists
```
{: screen}

{: tsCauses}
Dieser Fehler gibt an, dass das Vorgängerrelease des StrongSwan-Diagramms nicht vollständig deinstalliert war.

{: tsResolve}

1. Löschen Sie das vorherige Diagrammrelease.
    ```
    helm delete --purge vpn
    ```
    {: pre}

2. Löschen Sie die Bereitstellung des Vorgängerrelease. Das Löschen der Bereitstellung und des zugehörigen Pods dauert bis zu einer Minute.
    ```
    kubectl delete deploy vpn-strongswan
    ```
    {: pre}

3. Überprüfen Sie, dass die Bereitstellung gelöscht wurde. Die Bereitstellung `vpn-strongswan` wird in der Liste nicht angezeigt.
    ```
    kubectl get deployments
    ```
    {: pre}

4. Installieren Sie das aktualisierte StrongSwan-Helm-Diagramm mit einem neuen Releasenamen.
    ```
    helm install -f config.yaml --name=vpn ibm/strongswan
    ```
    {: pre}

<br />


## StrongSwan-VPN-Konnektivität schlägt nach Hinzufügen oder Löschen von Workerknoten fehl
{: #cs_vpn_fails_worker_add}

{: tsSymptoms}
Sie haben zuvor eine funktionierende VPN-Verbindung mithilfe des StrongSwan-IPSec-VPN-Service hergestellt. Nachdem Sie einen Workerknoten in Ihrem Cluster hinzugefügt oder gelöscht haben, tritt jedoch mindestens eines der folgenden Symptome auf:

* Der VPN-Status lautet nicht `ESTABLISHED`.
* Sie können von Ihrem lokalen Netz im Unternehmen nicht auf einen neuen Workerknoten zugreifen.
* Sie können von Pods, die auf neuen Workerknoten ausgeführt werden, nicht auf das ferne Netz zugreifen.

{: tsCauses}
Wenn Sie einem Worker-Pool einen Workerknoten hinzugefügt haben:

* Der Workerknoten wurde auf einem neuen privaten Teilnetz bereitgestellt, das nicht über die VPN-Verbindung durch Ihre vorhandenen `localSubnetNAT`- oder `local.subnet`-Einstellungen zugänglich gemacht wurde.
* VPN-Routen können nicht zum Workerknoten hinzugefügt werden, weil der Worker Taints oder Bezeichnungen aufweist, die nicht in Ihren vorhandenen `tolerations`- oder `nodeSelector`-Einstellungen enthalten sind.
* Der VPN-Pod ist auf dem neuen Workerknoten aktiv, aber die öffentliche IP-Adresse des Workerknotens ist über die lokale Firewall des Unternehmens nicht zulässig.

Wenn Sie einen Workerknoten gelöscht haben:

* Der Workerknoten war aufgrund von vorhandenen `tolerations`- oder `nodeSelector`-Einstellungen der einzige Knoten, in dem der VPN-Pod aktiv war.

{: tsResolve}
Aktualisieren Sie die Helm-Diagrammwerte, um die Änderungen im Workerknoten abzubilden.

1. Löschen Sie das vorhandene Helm-Diagramm.

    ```
    helm delete --purge <releasename>
    ```
    {: pre}

2. Öffnen Sie die Konfigurationsdatei für Ihren StrongSwan-VPN-Service.

    ```
    helm inspect values ibm/strongswan > config.yaml
    ```
    {: pre}

3. Überprüfen Sie die folgenden Einstellungen und nehmen Sie bei Bedarf Änderungen vor, um die gelöschten oder hinzugefügten Workerknoten abzubilden.

    Wenn Sie einen Workerknoten hinzugefügt haben:

    <table>
    <caption>Workerknoteneinstellungen</caption?
     <thead>
     <th>Einstellung</th>
     <th>Beschreibung</th>
     </thead>
     <tbody>
     <tr>
     <td><code>localSubnetNAT</code></td>
     <td>Der hinzugefügte Worker kann in einem neuen, anderen privaten Teilnetz bereitgestellt werden als die anderen vorhandenen Teilnetze, auf denen sich andere Workerknoten befinden. Wenn Sie die Teilnetz-NAT (subnetNAT) verwenden, um die privaten lokalen IP-Adressen Ihres Clusters erneut zuzuordnen, und wenn der Worker auf einem neuen Teilnetz hinzugefügt wurde, fügen Sie die neue Teilnetz-CIDR zu dieser Einstellung hinzu.</td>
     </tr>
     <tr>
     <td><code>nodeSelector</code></td>
     <td>Wenn Sie zuvor die Bereitstellung des VPN-Pods auf Worker mit einer bestimmten Bezeichnung begrenzt haben, stellen Sie sicher, dass der hinzugefügte Worker auch diese Bezeichnung aufweist.</td>
     </tr>
     <tr>
     <td><code>tolerations</code></td>
     <td>Wenn der hinzugefügte Workerknoten über einen Taint verfügt, ändern Sie diese Einstellung so, dass der VPN-Pod auf allen Workern mit beliebigen Bezeichnungen und bestimmten Bezeichnungen ausgeführt wird.</td>
     </tr>
     <tr>
     <td><code>local.subnet</code></td>
     <td>Der hinzugefügte Worker kann in einem neuen, anderen privaten Teilnetz bereitgestellt werden als die vorhandenen Teilnetze, auf denen sich andere Worker befinden. Wenn Ihre Apps von NodePort- oder LoadBalancer-Services im privaten Netz zugänglich gemacht werden und sich auf dem hinzugefügten Worker befinden, dann fügen Sie die neue Teilnetz-CIDR zu dieser Einstellung hinzu. **Hinweis**: Wenn Sie Werte zum `lokalen Teilnetz` hinzufügen, überprüfen Sie die VPN-Einstellungen für das lokale Teilnetz im Unternehmen, um zu sehen, ob diese ebenfalls aktualisiert werden müssen.</td>
     </tr>
     </tbody></table>

    Wenn Sie einen Workerknoten gelöscht haben:

    <table>
    <caption>Einstellungen für Workerknoten</caption>
     <thead>
     <th>Einstellung</th>
     <th>Beschreibung</th>
     </thead>
     <tbody>
     <tr>
     <td><code>localSubnetNAT</code></td>
     <td>Wenn Sie die Teilnetz-NAT (subnetNAT) verwenden, um bestimmte private, lokale IP-Adressen erneut zuzuordnen, entfernen Sie alle IP-Adressen aus dieser Einstellung, die vom alten Worker stammen. Wenn Sie die Teilnetz-NAT verwenden, um ganze Teilnetze erneut zuzuordnen, und sich keine Worker mehr im Teilnetz befinden, entfernen Sie das Teilnetz-CIDR aus dieser Einstellung.</td>
     </tr>
     <tr>
     <td><code>nodeSelector</code></td>
     <td>Wenn Sie zuvor die Bereitstellung des VPN-Pods auf einen einzelnen Worker begrenzt haben und dieser Worker gelöscht wurde, ändern Sie diese Einstellung so, dass der VPN-Pod auf anderen Workern ausgeführt werden kann.</td>
     </tr>
     <tr>
     <td><code>tolerations</code></td>
     <td>Wenn der von Ihnen gelöschte Worker mit keinem Taint versehen war, alle anderen verbliebenen Worker jedoch schon, ändern Sie die Einstellung so, dass der VPN-Pod auf Workern mit beliebigen Taints oder bestimmten Taints ausgeführt werden kann.
     </td>
     </tr>
     </tbody></table>

4. Installieren Sie das neue Helm-Diagramm mit Ihren aktualisierten Werten.

    ```
    helm install -f config.yaml --name=<releasename> ibm/strongswan
    ```
    {: pre}

5. Prüfen Sie den Status der Diagrammbereitstellung. Sobald das Diagramm bereit ist, hat das Feld **STATUS** oben in der Ausgabe den Wert `DEPLOYED`.

    ```
    helm status <releasename>
    ```
    {: pre}

6. In einigen Fällen müssen Sie möglicherweise Ihre lokalen Einstellungen im Unternehmen und Ihre Firewalleinstellungen ändern, damit diese mit den Änderungen übereinstimmen, die Sie in der VPN-Konfigurationsdatei vorgenommen haben.

7. Starten Sie das VPN.
    * Falls die VPN-Verbindung von einem Cluster initialisiert wird (für `ipsec.auto` ist `start` festgelegt), starten Sie das VPN auf einem lokalen Gateway im Unternehmen und starten Sie dann das VPN auf dem Cluster.
    * Falls die VPN-Verbindung von einem lokalen Gateway im Unternehmen initialisiert wird (für `ipsec.auto` ist `auto` festgelegt), starten Sie das VPN auf dem Cluster und anschließend starten Sie das VPN auf dem lokalen Gateway im Unternehmen.

8. Legen Sie die Umgebungsvariable `STRONGSWAN_POD` fest.

    ```
    export STRONGSWAN_POD=$(kubectl get pod -l app=strongswan,release=<releasename> -o jsonpath='{ .items[0].metadata.name }')
    ```
    {: pre}

9. Prüfen Sie den Status des VPN.

    ```
    kubectl exec $STRONGSWAN_POD -- ipsec status
    ```
    {: pre}

    * Sobald die VPN-Verbindung den Status `ESTABLISHED` erreicht hat, war die VPN-Verbindung erfolgreich. Es ist keine weitere Aktion erforderlich.

    * Wenn Sie immer noch Verbindungsprobleme haben, lesen Sie die Informationen im Abschnitt [VPN-Konnektivität mit StrongSwan-Helm-Diagramm kann nicht erstellt werden](#cs_vpn_fails), um weiter nach Fehlern bei der VPN-Verbindung zu suchen.

<br />



## Calico-Netzrichtlinien können nicht abgerufen werden
{: #cs_calico_fails}

{: tsSymptoms}
Wenn Sie versuchen, Calico-Netzrichtlinien im Cluster durch Ausführen des Befehls `calicoctl get policy` anzuzeigen, führt dies zu den folgenden unerwarteten Ergebnissen oder Fehlermeldungen:
- Eine leere Liste.
- Eine Liste alter Calico-Richtlinien der Version 2 anstelle der Version 3.
- `Failed to create Calico API client: syntax error in calicoctl.cfg: invalid config file: unknown APIVersion 'projectcalico.org/v3'`

Wenn Sie versuchen, Calico-Netzrichtlinien im Cluster durch Ausführen des Befehls `calicoctl get GlobalNetworkPolicy` anzuzeigen, führt dies zu den folgenden unerwarteten Ergebnissen oder Fehlermeldungen:
- Eine leere Liste.
- `Failed to create Calico API client: syntax error in calicoctl.cfg: invalid config file: unknown APIVersion 'v1'`
- `Failed to create Calico API client: syntax error in calicoctl.cfg: invalid config file: unknown APIVersion 'projectcalico.org/v3'`
- `Failed to get resources: Resource type 'GlobalNetworkPolicy' is not supported`

{: tsCauses}
Bei der Verwendung von Calico-Richtlinien müssen vier Faktoren aufeinander abgestimmt werden: Die Version des Kubernetes-Cluster, die Version der Calico-CLI, die Syntax der Calico-Konfigurationsdatei und Befehle zum Anzeigen von Richtlinien (view policy). Einer oder mehrere dieser Faktoren entsprechen nicht den Vorgaben.

{: tsResolve}
Sie müssen die Calico-CLI Version 3.3 oder höher, die Version 3-Syntax für die Konfigurationsdatei `calicoctl.cfg` sowie die Befehle `calicoctl get GlobalNetworkPolicy` und `calicoctl get NetworkPolicy` verwenden.

Gehen Sie wie folgt vor, um sicherzustellen, dass alle Calico-Faktoren aufeinander abgestimmt sind:

1. [Installieren und konfigurieren Sie die Calico-CLI der Version 3.3 oder höher](/docs/containers?topic=containers-network_policies#cli_install).
2. Stellen Sie sicher, dass alle Richtlinien, die Sie erstellen und auf den Cluster anwenden möchten, die [Calico Version 3-Syntax ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.projectcalico.org/v3.3/reference/calicoctl/resources/networkpolicy) verwenden. Wenn eine bestehende `.yaml`- oder `.json`-Richtliniendatei mit der Calicao Version 2-Syntax vorliegt, können Sie sie mithilfe des Befehls [`calicoctl convert` ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.projectcalico.org/v3.3/reference/calicoctl/commands/convert) in die Calico Version 3-Syntax konvertieren.
3. Stellen Sie zum [Anzeigen von Richtlinien](/docs/containers?topic=containers-network_policies#view_policies) sicher, dass Sie den Befehl `calicoctl get GlobalNetworkPolicy` für globale Richtlinien und den Befehl `calicoctl get NetworkPolicy --namespace <policy_namespace>` für Richtlinien verwenden, die sich auf bestimmte Namensbereiche beziehen.

<br />


## Workerknoten können aufgrund einer ungültigen VLAN-ID nicht hinzugefügt werden
{: #suspended}

{: tsSymptoms}
Ihr {{site.data.keyword.Bluemix_notm}}-Konto wurde ausgesetzt oder alle Workerknoten in Ihrem Cluster wurden gelöscht. Nachdem der Reaktivierung des Kontos können Sie keine Workerknoten hinzufügen, wenn Sie versuchen, die Größe des Worker-Pools zu ändern oder seine Last auszugleichen. Es wird eine Fehlernachricht ähnlich der folgenden angezeigt:

```
SoftLayerAPIError(SoftLayer_Exception_Public): Could not obtain network VLAN with id #123456.
```
{: screen}

{: tsCauses}
Wenn ein Konto ausgesetzt wird, werden die in dem Konto vorhandenen Workerknoten gelöscht. Wenn ein Cluster keine Workerknoten hat, fordert die IBM Cloud-Infrastruktur (SoftLayer) das zugeordnete öffentliche und private VLAN zurück. Der Worker-Pool des Clusters enthält jedoch noch die früheren VLAN-IDs in seinen Metadaten und verwendet diese nicht verfügbaren IDs, wenn Sie den Pool neu ausgleichen oder seine Größe ändern. Das Erstellen der Knoten schlägt fehl, weil die VLANs dem Cluster nicht mehr zugeordnet sind.

{: tsResolve}

Sie können [Ihren vorhandenen Worker-Pool löschen](/docs/containers?topic=containers-cs_cli_reference#cs_worker_pool_rm) und anschließend [einen neuen Worker-Pool erstellen](/docs/containers?topic=containers-cs_cli_reference#cs_worker_pool_create).

Alternativ können Sie Ihren vorhandenen Worker-Pool beibehalten, indem Sie neue VLANs bestellen und sie zum Erstellen neuer Worker-Pools im Pool zu verwenden.

Vorbereitende Schritte: [Melden Sie sich an Ihrem Konto an. Geben Sie als Ziel die entsprechende Region und, sofern zutreffend, die Ressourcengruppe an. Legen Sie den Kontext für den Cluster fest.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  Wenn Sie die Zonen abrufen möchten, für die Sie neue VLAN-IDs benötigen, notieren Sie sich den Standort (**Standort**), der in der Ausgabe des folgenden Befehls angezeigt wird. **Hinweis**: Wenn Ihr Cluster ein Mehrzonencluster ist, benötigen Sie VLAN-IDs für jede der einzelnen Zonen.

    ```
    ibmcloud ks clusters
    ```
    {: pre}

2.  Rufen Sie für jede Zone, in der sich Ihr Cluster befindet, ein neues privates und ein öffentliches VLAN ab, indem Sie sich an den [den {{site.data.keyword.Bluemix_notm}}-Support](/docs/infrastructure/vlans?topic=vlans-ordering-premium-vlans#ordering-premium-vlans) wenden.

3.  Notieren Sie die IDs des neuen privaten und des öffentlichen VLANs der einzelnen Zonen.

4.  Notieren Sie die Namen Ihrer Worker-Pools.

    ```
    ibmcloud ks worker-pools --cluster <clustername_oder_-id>
    ```
    {: pre}

5.  Verwenden Sie den [Befehl](/docs/containers?topic=containers-cs_cli_reference#cs_zone_network_set) `zone-network-set`, um die Netz-Metadaten des Worker-Pools zu ändern.

    ```
    ibmcloud ks zone-network-set --zone <zone> --cluster <clustername_oder_-id> -- worker-pools <worker-pool> --private-vlan <id_des_privaten_vlans> --public-vlan <id_des_öffentlichen_vlans>
    ```
    {: pre}

6.  **Nur Mehrzonencluster**: Wiederholen Sie **Schritt 5** für jede Zone Ihres Clusters.

7.  Gleichen Sie Ihren Worker-Pool neu aus oder ändern Sie seine Größe, um Worker-Pools hinzuzufügen, die die neuen VLAN-IDs verwenden. Beispiel:

    ```
    ibmcloud ks worker-pool-resize --cluster <clustername_oder_-id> --worker-pool <worker-pool> --size-per-zone <anzahl_worker_pro_zone>
    ```
    {: pre}

8.  Überprüfen Sie, dass die Workerknoten erstellt wurden.

    ```
    ibmcloud ks workers --cluster <clustername_oder_id> --worker-pool <worker-pool>
    ```
    {: pre}

<br />



## Hilfe und Unterstützung anfordern
{: #network_getting_help}

Haben Sie noch immer Probleme mit Ihrem Cluster?
{: shortdesc}

-  Sie werden im Terminal benachrichtigt, wenn Aktualisierungen für die `ibmcloud`-CLI und -Plug-ins verfügbar sind. Halten Sie Ihre CLI stets aktuell, sodass Sie alle verfügbaren Befehle und Flags verwenden können.
-   [Überprüfen Sie auf der {{site.data.keyword.Bluemix_notm}}-Statusseite ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/status?selected=status), ob {{site.data.keyword.Bluemix_notm}} verfügbar ist.
-   Veröffentlichen Sie eine Frage im [{{site.data.keyword.containerlong_notm}}-Slack ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://ibm-container-service.slack.com).
    Wenn Sie keine IBM ID für Ihr {{site.data.keyword.Bluemix_notm}}-Konto verwenden, [fordern Sie eine Einladung](https://bxcs-slack-invite.mybluemix.net/) zu diesem Slack an.
    {: tip}
-   Suchen Sie in entsprechenden Foren, ob andere Benutzer auf das gleiche Problem
gestoßen sind. Versehen Sie Ihre Fragen in den Foren mit Tags, um sie für das Entwicklungsteam
von {{site.data.keyword.Bluemix_notm}} erkennbar zu machen.
    -   Wenn Sie technische Fragen zur Entwicklung oder Bereitstellung von Clustern oder Apps mit {{site.data.keyword.containerlong_notm}} haben, veröffentlichen Sie Ihre Frage auf [Stack Overflow ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://stackoverflow.com/questions/tagged/ibm-cloud+containers) und versehen Sie sie mit den Tags `ibm-cloud`, `kubernetes` und `containers`.
    -   Verwenden Sie bei Fragen zum Service und zu ersten Schritten das Forum [IBM Developer Answers ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix). Geben Sie die Tags `ibm-cloud` und `containers` an.
    Weitere Details zur Verwendung der Foren finden Sie unter [Hilfe anfordern](/docs/get-support?topic=get-support-getting-customer-support#using-avatar).
-   Wenden Sie sich an den IBM Support, indem Sie einen Fall öffnen. Informationen zum Öffnen eines IBM Supportfalls oder zu Supportstufen und zu Prioritätsstufen von Fällen finden Sie unter [Support kontaktieren](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support).
Geben Sie beim Melden eines Problems Ihre Cluster-ID an. Führen Sie den Befehl `ibmcloud ks clusters` aus, um Ihre Cluster-ID abzurufen. Sie können außerdem [{{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool](/docs/containers?topic=containers-cs_troubleshoot#debug_utility) verwenden, um relevante Informationen aus Ihrem Cluster zu erfassen und zu exportieren, um sie dem IBM Support zur Verfügung zu stellen.
{: tip}

