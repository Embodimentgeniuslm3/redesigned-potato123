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


# Protokollierung und Überwachung von Ingress
{: #ingress_health}

Passen Sie die Protokollierung an und richten Sie die Überwachung ein, um Unterstützung bei der Fehlerbehebung zu erhalten und um die Leistung Ihrer Ingress-Konfiguration zu verbessern.
{: shortdesc}

## Ingress-Protokolle anzeigen
{: #ingress_logs}

Wenn Sie Ingress-Fehler beheben oder die Ingress-Aktivität überwachen wollen, können Sie die Ingress-Protokolle prüfen.
{: shortdesc}

Für Ihre Ingress-ALBs werden automatisch Protokolle erfasst. Um die ALB-Protokolle anzuzeigen, können Sie zwischen zwei Optionen wählen.
* [Erstellen Sie eine Protokollierungskonfiguration für den Ingress-Service](/docs/containers?topic=containers-health#configuring) in Ihrem Cluster.
* Überprüfen Sie die Protokolle über die CLI. **Hinweis:** Sie müssen mindestens über die [{{site.data.keyword.Bluemix_notm}} IAM-Servicerolle **Leseberechtigter** (Reader)](/docs/containers?topic=containers-users#platform) für den Namensbereich `kube-system` verfügen.
    1. Rufen Sie die ID eines Pods für eine ALB ab.
        ```
        kubectl get pods -n kube-system | grep alb
        ```
        {: pre}

    2. Öffnen Sie die Protokolle für diesen ALB-Pod.
        ```
        kubectl logs <id_des_alb-pods> nginx-ingress -n kube-system
        ```
        {: pre}

</br>Der Ingress-Standardprotokollinhalt wird in JSON formatiert und zeigt allgemeine Felder an, in denen die Verbindungssitzung zwischen einem Client und Ihrer App beschrieben wird. Ein Beispielprotokoll mit den Standardfeldern sieht wie folgt aus:

```
{"time_date": "2018-08-21T17:33:19+00:00", "client": "108.162.248.42", "host": "albhealth.multizone.us-south.containers.appdomain.cloud", "scheme": "http", "request_method": "GET", "request_uri": "/", "request_id": "c2bcce90cf2a3853694da9f52f5b72e6", "status": 200, "upstream_addr": "192.168.1.1:80", "upstream_status": 200, "request_time": 0.005, "upstream_response_time": 0.005, "upstream_connect_time": 0.000, "upstream_header_time": 0.005}
```
{: screen}

<table>
<caption>Erklärung der Felder im Ingress-Standardprotokollformat</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der Felder im Ingress-Standardprotokollformat</th>
</thead>
<tbody>
<tr>
<td><code>"time_date": "$time_iso8601"</code></td>
<td>Die Ortszeit im ISO-8601-Standardformat, wenn das Protokoll geschrieben wird.</td>
</tr>
<tr>
<td><code>"client": "$remote_addr"</code></td>
<td>Die IP-Adresse des Anforderungspakets, das der Client an Ihre App gesendet hat. Diese IP kann sich in folgenden Situationen ändern:<ul><li>Wenn an Ihren Cluster eine Clientanforderung für Ihre App gesendet wird, wird die Anforderung an einen Pod für den Lastausgleichsservice weitergeleitet, der die Lastausgleichsfunktion zugänglich macht. Wenn ein App-Pod nicht auf demselben Workerknoten vorhanden ist wie der Lastausgleichsservice-Pod, leitet die Lastausgleichsfunktion die Anforderung an einen App-Pod auf einem anderen Workerknoten weiter. Die Quellen-IP-Adresse des Anforderungspakets wird in die öffentliche IP-Adresse des Workerknotens geändert, auf dem der App-Pod ausgeführt wird.</li><li>Wenn die [Beibehaltung der Quellen-IP aktiviert ist](/docs/containers?topic=containers-ingress#preserve_source_ip), wird stattdessen die ursprüngliche IP-Adresse der Clientanforderung an Ihre App aufgezeichnet.</li></ul></td>
</tr>
<tr>
<td><code>"host": "$http_host"</code></td>
<td>Der Host oder die Unterdomäne, über den/die Ihre Apps zugänglich sind. Diese Unterdomäne wird in den Ingress-Ressourcendateien für Ihre ALBs konfiguriert.</td>
</tr>
<tr>
<td><code>"scheme": "$scheme"</code></td>
<td>Die Art der Anforderung: <code>HTTP</code> oder <code>HTTPS</code>.</td>
</tr>
<tr>
<td><code>"request_method": "$request_method"</code></td>
<td>Die Methode des Anforderungsaufrufs an die Back-End-App, z. B. <code>GET</code> oder <code>POST</code>.</td>
</tr>
<tr>
<td><code>"request_uri": "$uri"</code></td>
<td>Der ursprüngliche Anforderungs-URI zu Ihrem App-Pfad. ALBs verarbeiten die Pfade, auf denen Apps als Präfixe empfangsbereit sind. Wenn eine ALB eine Anforderung von einem Client an eine App empfängt, überprüft der ALB die Ingress-Ressource auf einen Pfad (als Präfix), der mit dem Pfad im Anforderungs-URI übereinstimmt.</td>
</tr>
<tr>
<td><code>"request_id": "$request_id"</code></td>
<td>Eine eindeutige Anforderungs-ID, die aus 10 beliebigen Bytes generiert wird.</td>
</tr>
<tr>
<td><code>"status": $status</code></td>
<td>Der Statuscode für die Verbindungssitzung.<ul>
<li><code>200</code>: Die Sitzung wurde erfolgreich abgeschlossen.</li>
<li><code>400</code>: Clientdaten können nicht geparst werden.</li>
<li><code>403</code>: Zugriff verboten; z. B. wenn der Zugriff auf bestimmte Client-IP-Adressen beschränkt ist.</li>
<li><code>500</code>: Interner Serverfehler.</li>
<li><code>502</code>: Fehlerhaftes Gateway; z. B. wenn ein Upstream-Server nicht ausgewählt oder erreicht werden kann.</li>
<li><code>503</code>: Service nicht verfügbar; z. B. wenn der Zugriff durch die Anzahl der Verbindungen begrenzt ist.</li>
</ul></td>
</tr>
<tr>
<td><code>"upstream_addr": "$upstream_addr"</code></td>
<td>Die IP-Adresse und der Port oder der Pfad zu dem UNIX-Domänen-Socket des Upstream-Servers. Wenn mehrere Server während der Anforderungsverarbeitung kontaktiert werden, werden ihre Adressen durch Kommas getrennt: <code>"192.168.1.1:80, 192.168.1.2:80, unix:/tmp/sock"</code>. Wenn die Anforderung intern von einer Servergruppe zu einer anderen umgeleitet wird, werden die Serveradressen aus verschiedenen Gruppen durch Doppelpunkte voneinander getrennt: <code>"192.168.1.1:80, 192.168.1.2:80, unix:/tmp/sock : 192.168.10.1:80, 192.168.10.2:80"</code>. Wenn die ALB keinen Server auswählen kann, wird stattdessen der Name der Servergruppe protokolliert.</td>
</tr>
<tr>
<td><code>"upstream_status": $upstream_status</code></td>
<td>Der Statuscode der Antwort, die vom Upstream-Server für die Back-End-App abgerufen wurde (z. B. HTTP-Standardantwortcodes). Statuscodes mehrerer Antworten werden durch Kommas und Doppelpunkte wie Adressen in der Variablen <code>$upstream_addr</code> voneinander getrennt. Wenn die ALB keinen Server auswählen kann, wird der Statuscode 502 (Fehlerhaftes Gateway) protokolliert.</td>
</tr>
<tr>
<td><code>"request_time": $request_time</code></td>
<td>Die Anforderungsverarbeitungszeit, gemessen in Sekunden mit einer Millisekundenauflösung. Diese Zeit beginnt, wenn die ALB die ersten Bytes der Clientanforderung liest, und sie endet, wenn die ALB die letzten Bytes der Antwort an den Client sendet. Das Protokoll wird unmittelbar nach dem Ende der Anforderungsverarbeitungszeit geschrieben.</td>
</tr>
<tr>
<td><code>"upstream_response_time": $upstream_response_time</code></td>
<td>Die Zeit, die die ALB benötigt, um die Antwort vom Upstream-Server für die Back-End-App zu empfangen (gemessen in Sekunden mit Millisekundenauflösung). Zeitangaben mehrerer Antworten werden durch Kommas und Doppelpunkte wie Adressen in der Variablen <code>$upstream_addr</code> voneinander getrennt.</td>
</tr>
<tr>
<td><code>"upstream_connect_time": $upstream_connect_time</code></td>
<td>Die Zeit, die die ALB benötigt, um eine Verbindung mit dem Upstream-Server für die Back-End-App herzustellen (gemessen in Sekunden mit Millisekundenauflösung). Wenn TLS/SSL in Ihrer Ingress-Ressourcenkonfiguration aktiviert ist, beinhaltet diese Zeit auch die Zeit, die für den Handshake benötigt wird. Zeitangaben mehrerer Verbindungen werden durch Kommas und Doppelpunkte wie Adressen in der Variablen <code>$upstream_addr</code> voneinander getrennt.</td>
</tr>
<tr>
<td><code>"upstream_header_time": $upstream_header_time</code></td>
<td>Die Zeit, die die ALB benötigt, um den Antwortheader vom Upstream-Server für die Back-End-App zu empfangen (gemessen in Sekunden mit Millisekundenauflösung). Zeitangaben mehrerer Verbindungen werden durch Kommas und Doppelpunkte wie Adressen in der Variablen <code>$upstream_addr</code> voneinander getrennt.</td>
</tr>
</tbody></table>

## Ingress-Protokollinhalt und -format anpassen
{: #ingress_log_format}

Sie können den Inhalt und das Format von Protokollen anpassen, die für die Ingress-ALB gesammelt werden.
{:shortdesc}

Standardmäßig sind Ingress-Protokolle in JSON formatiert und zeigen allgemeine Protokollfelder an. Sie können jedoch auch ein angepasstes Protokollformat erstellen, indem Sie auswählen, welche Protokollkomponenten weitergeleitet und wie die Komponenten in der Protokollausgabe angeordnet werden.

Stellen Sie zunächst sicher, dass Sie die [{{site.data.keyword.Bluemix_notm}} IAM-Servicerolle **Schreibberechtigter** (Writer) oder **Manager**](/docs/containers?topic=containers-users#platform) für den Namensbereich `kube-system` innehaben.

1. Bearbeiten Sie die Konfigurationsdatei für die Konfigurationszuordnungsressource (Configmap) `ibm-cloud-provider-ingress-cm`.

    ```
    kubectl edit cm ibm-cloud-provider-ingress-cm -n kube-system
    ```
    {: pre}

2. Fügen Sie einen Abschnitt <code>data</code> hinzu. Fügen Sie das Feld `log-format` und optional das Feld `log-format-escape-json` hinzu.

    ```
    apiVersion: v1
    data:
      log-format: '{<schlüssel1>: <protokollvariable1>, <schlüssel2>: <protokollvariable2>, <schlüssel3>: <protokollvariable3>}'
      log-format-escape-json: "true"
    kind: ConfigMap
    metadata:
      name: ibm-cloud-provider-ingress-cm
      namespace: kube-system
    ```
    {: codeblock}

    <table>
    <caption>Komponenten der YAML-Datei</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der Protokollformatkonfiguration</th>
    </thead>
    <tbody>
    <tr>
    <td><code>log-format</code></td>
    <td>Ersetzen Sie <code>&lt;schlüssel&gt;</code> durch den Namen für die Protokollkomponente und <code>&lt;protokollvariable&gt;</code> durch eine Variable für die Protokollkomponente, die Sie in Protokolleinträgen erfassen wollen. Sie können Text und Interpunktion einschließen, die in dem Protokolleintrag enthalten sein sollen, wie zum Beispiel Anführungszeichen um Zeichenfolgewerte und Kommas zum Trennen von Protokollkomponenten. Beispiel: Eine Formatierung einer Komponente wie <code>request: "$request"</code> generiert den folgenden Inhalt in einen Protokolleintrag: <code>request: "GET / HTTP/1.1"</code>. Eine Liste aller verwendbaren Variablen finden Sie im <a href="http://nginx.org/en/docs/varindex.html">NGINX-Variablenindex</a>.<br><br>Zum Protokollieren eines zusätzlichen Headers wie <em>x-custom-ID</em> fügen Sie dem angepassten Protokollinhalt das folgende Schlüssel/Wert-Paar hinzu: <br><pre class="codeblock"><code>customID: $http_x_custom_id</code></pre> <br>Bindestriche (<code>-</code>) werden in Unterstreichungszeichen (<code>_</code>) konvertiert und <code>_$http_</code> muss dem angepassten Headernamen vorangestellt werden.</td>
    </tr>
    <tr>
    <td><code>log-format-escape-json</code></td>
    <td>Optional: Protokolle werden standardmäßig im Textformat generiert. Zum Generieren von Protokollen im JSON-Format fügen Sie das Feld <code>log-format-escape-json</code> hinzu und verwenden den Wert <code>true</code>.</td>
    </tr>
    </tbody></table>

    Ihr Protokollformat könnte zum Beispiel die folgenden Variablen enthalten:
    ```
    apiVersion: v1
    data:
      log-format: '{remote_address: $remote_addr, remote_user: "$remote_user",
                    time_date: [$time_local], request: "$request",
                    status: $status, http_referer: "$http_referer",
                    http_user_agent: "$http_user_agent",
                    request_id: $request_id}'
    kind: ConfigMap
    metadata:
      name: ibm-cloud-provider-ingress-cm
      namespace: kube-system
    ```
    {: screen}

    Ein Protokolleintrag in diesem Format sieht zum Beispiel wie folgt aus:
    ```
    remote_address: 127.0.0.1, remote_user: "dbmanager", time_date: [30/Mar/2018:18:52:17 +0000], request: "GET / HTTP/1.1", status: 401, http_referer: "-", http_user_agent: "Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:47.0) Gecko/20100101 Firefox/47.0", request_id: a02b2dea9cf06344a25611c1d7ad72db
    ```
    {: screen}

    Wenn Sie ein abgepasstes Protokollformat erstellen wollen, das auf dem Standardformat für ALB-Protokolle basiert, ändern Sie den folgenden Abschnitt nach Bedarf und fügen Sie ihn Ihrer Konfigurationszuordnung (Configmap) hinzu:
    ```
    apiVersion: v1
    data:
      log-format: '{"time_date": "$time_iso8601", "client": "$remote_addr",
                    "host": "$http_host", "scheme": "$scheme",
                    "request_method": "$request_method", "request_uri": "$uri",
                    "request_id": "$request_id", "status": $status,
                    "upstream_addr": "$upstream_addr", "upstream_status":
                    $upstream_status, "request_time": $request_time,
                    "upstream_response_time": $upstream_response_time,
                    "upstream_connect_time": $upstream_connect_time,
                    "upstream_header_time": $upstream_header_time}'
      log-format-escape-json: "true"
    kind: ConfigMap
    metadata:
      name: ibm-cloud-provider-ingress-cm
      namespace: kube-system
    ```
    {: codeblock}

4. Speichern Sie die Konfigurationsdatei.

5. Stellen Sie sicher, dass die Änderungen an der Konfigurationszuordnung angewendet wurden.

   ```
   kubectl get cm ibm-cloud-provider-ingress-cm -n kube-system -o yaml
   ```
   {: pre}

4. Zum Anzeigen der Ingress-ALB-Protokolle sind zwei Optionen verfügbar.
    * [Erstellen Sie eine Protokollierungskonfiguration für den Ingress-Service](/docs/containers?topic=containers-health#logging) in Ihrem Cluster.
    * Überprüfen Sie die Protokolle von der CLI.
        1. Rufen Sie die ID eines Pods für eine ALB ab.
            ```
            kubectl get pods -n kube-system | grep alb
            ```
            {: pre}

        2. Öffnen Sie die Protokolle für diesen ALB-Pod. Stellen Sie sicher, dass die Protokolle dem aktualisierten Format entsprechen.
            ```
            kubectl logs <id_des_alb-pods> nginx-ingress -n kube-system
            ```
            {: pre}

<br />


## Ingress-ALB überwachen
{: #ingress_monitoring}

Sie können Ihre ALBs überwachen, indem Sie eine Exportfunktion für Metriken und den Prometheus-Agenten in Ihrem Cluster bereitstellen.
{: shortdesc}

Die Exportfunktion für ALB-Metriken verwendet die NGINX-Anweisung `vhost_traffic_status_zone`, um Metrikdaten aus dem Endpunkt `/status/format/json` in jedem Ingress-ALB-Pod zu erfassen. Die Exportfunktion für Metriken formatiert jedes Datenfeld in der JSON-Datei automatisch in eine Metrik um, die von Prometheus gelesen werden kann. Anschließend nimmt ein Prometheus-Agent die von der Exportfunktion generierten Metriken auf und stellt sie auf einem Prometheus-Dashboard dar.

### Helm-Diagramm für Metriken-Exportfunktion installieren
{: #metrics-exporter}

Installieren Sie das Helm-Diagramm für Metriken-Exportfunktionen, um eine ALB in Ihrem Cluster zu überwachen.
{: shortdesc}

Die Pods mit der Exportfunktion für ALB-Metriken müssen in denselben Workerknoten bereitgestellt werden, in denen auch Ihre ALBs bereitgestellt sind. Wenn Ihre ALBs in Edge-Workerknoten ausgeführt werden und diese Edge-Workerknoten mit Taints versehen sind, um andere Workloadbereitstellungen zu verhindern, können die Pods mit den Exportfunktionen für Metriken nicht geplant werden. Sie müssen die Taints entfernen, indem Sie den Befehl `kubectl taint node <knotenname> dedicated:NoSchedule- dedicated:NoExecute-` ausführen.
{: note}

1.  **Wichtig:** [Befolgen Sie die Anweisungen](/docs/containers?topic=containers-helm#public_helm_install) zur Installation des Helm-Clients auf Ihrer lokalen Maschine, installieren Sie den Helm-Server (Tiller) mit einem Servicekonto und fügen Sie die {{site.data.keyword.Bluemix_notm}} Helm-Repositorys hinzu.

2. Installieren Sie das Helm-Diagramm `ibmcloud-alb-metrics-exporter` in Ihrem Cluster. Dieses Helm-Diagramm stellt eine Exportfunktion für ALB-Metriken bereit und erstellt das Servicekonto `alb-metrics-service-account` im Namensbereich `kube-system`. Ersetzen Sie <alb-id> durch die ID der ALB, für die Sie Metriken erfassen wollen. Zum Anzeigen der IDs für die ALBs in Ihrem Cluster führen Sie den Befehl <code>ibmcloud ks albs --cluster &lt;clustername&gt;</code> aus.
  Sie müssen für jede zu überwachende ALB ein Diagramm bereitstellen.
  {: note}
  ```
  helm install iks-charts/ibmcloud-alb-metrics-exporter --name ibmcloud-alb-metrics-exporter --set metricsNameSpace=kube-system --set albId=<alb-ID>
  ```
  {: pre}

3. Prüfen Sie den Status der Diagrammbereitstellung. Sobald das Diagramm bereit ist, hat das Feld **STATUS** oben in der Ausgabe den Wert `DEPLOYED`.
  ```
  helm status ibmcloud-alb-metrics-exporter
  ```
  {: pre}

4. Überprüfen Sie, ob die `ibmcloud-alb-metrics-exporter`-Pods ausgeführt werden.
  ```
  kubectl get pods -n kube-system -o wide
  ```
  {:pre}

  Beispielausgabe:
  ```
  NAME                                             READY     STATUS      RESTARTS   AGE       IP               NODE
  ...
  alb-metrics-exporter-868fddf777-d49l5            1/1       Running     0          19s       172.30.xxx.xxx   10.xxx.xx.xxx
  alb-metrics-exporter-868fddf777-pf7x5            1/1       Running     0          19s       172.30.xxx.xxx   10.xxx.xx.xxx
  ```
  {:screen}

5. Optional: [Installieren Sie den Prometheus-Agenten](#prometheus-agent), der die von der Exportfunktion generierten Metriken aufnimmt und in einem Prometheus-Dashboard darstellt.

### Helm-Diagramm für Prometheus-Agenten installieren
{: #prometheus-agent}

Nach der Installation der [Metriken-Exportfunktion](#metrics-exporter) können Sie das Helm-Diagramm für den Prometheus-Agenten installieren, der die von der Exportfunktion generierten Metriken aufnimmt und in einem Prometheus-Dashboard darstellt.
{: shortdesc}

1. Laden Sie die TAR-Datei für das Helm-Diagramm für die Metriken-Exportfunktion von https://icr.io/helm/iks-charts/charts/ibmcloud-alb-metrics-exporter-1.0.7.tgz herunter.

2. Navigieren Sie zum Prometheus-Unterordner.
  ```
  cd ibmcloud-alb-metrics-exporter-1.0.7.tar/ibmcloud-alb-metrics-exporter/subcharts/prometheus
  ```
  {: pre}

3. Installieren Sie das Prometheus-Helm-Diagramm in Ihrem Cluster. Ersetzen Sie <ingress-unterdomäne> durch die Ingress-Unterdomäne für Ihren Cluster. Die URL für das Prometheus-Dashboard ist eine Kombination aus der Prometheus-Standard-Unterdomäne `prom-dash` und Ihrer Ingress-Unterdomäne, z. B. `prom-dash.mycluster-12345.us-south.containers.appdomain.cloud`. Zum Ermitteln der Ingress-Unterdomäne für Ihren Cluster führen Sie den Befehl <code>ibmcloud ks cluster-get --cluster &lt;clustername&gt;</code> aus.
  ```
  helm install --name prometheus . --set nameSpace=kube-system --set hostName=prom-dash.<ingress_subdomain>
  ```
  {: pre}

4. Prüfen Sie den Status der Diagrammbereitstellung. Sobald das Diagramm bereit ist, hat das Feld **STATUS** oben in der Ausgabe den Wert `DEPLOYED`.
    ```
    helm status prometheus
    ```
    {: pre}

5. Überprüfen Sie, ob der `prometheus`-Pod ausgeführt wird.
    ```
    kubectl get pods -n kube-system -o wide
    ```
    {:pre}

    Beispielausgabe:
    ```
    NAME                                             READY     STATUS      RESTARTS   AGE       IP               NODE
    alb-metrics-exporter-868fddf777-d49l5            1/1       Running     0          19s       172.30.xxx.xxx   10.xxx.xx.xxx
    alb-metrics-exporter-868fddf777-pf7x5            1/1       Running     0          19s       172.30.xxx.xxx   10.xxx.xx.xxx
    prometheus-9fbcc8bc7-2wvbk                       1/1       Running     0          1m        172.30.xxx.xxx   10.xxx.xx.xxx
    ```
    {:screen}

6. Geben Sie in einem Browser die URL für das Prometheus-Dashboard ein. Dieser Hostname hat das Format `prom-dash.mycluster-12345.us-south.containers.appdomain.cloud`. Das Prometheus-Dashboard für Ihre ALB wird geöffnet.

7. Lesen Sie die weiteren Informationen zu den Metriken für [ALB](#alb_metrics), [server](#server_metrics) und [upstream](#upstream_metrics), die im Dashboard aufgelistet werden.

### ALB-Metriken
{: #alb_metrics}

Der Pod `alb-metrics-exporter` formatiert jedes Datenfeld in der JSON-Datei automatisch in eine Metrik um, die von Prometheus gelesen werden kann. In ALB-Metriken werden Daten zu den Verbindungen und Antworten erfasst, die von der ALB verarbeitet werden.
{: shortdesc}

ALB-Metriken haben das Format `kube_system_<ALB-ID>_<METRIKNAME> <WERT>`. Beispiel: Wenn eine ALB 23 Antworten mit Statuscode der Ebene '2xx' empfängt, wird die Metrik im Format `kube_system_public_crf02710f54fcc40889c301bfd6d5b77fe_alb1_totalHandledRequest {.. metric="2xx"} 23` dargestellt, wobei `metric` die Prometheus-Bezeichnung ist.

In der folgenden Tabelle sind die unterstützten ALB-Metriknamen mit den Metrikbezeichnungen im Format `<alb-metrikname>_<metrikbezeichnung>` aufgeführt.
<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Unterstützte ALB-Metriken</th>
</thead>
<tbody>
<tr>
<td><code>connections_reading</code></td>
<td>Die Gesamtzahl lesender Clientverbindungen.</td>
</tr>
<tr>
<td><code>connections_accepted</code></td>
<td>Die Gesamtzahl akzeptierter Clientverbindungen.</td>
</tr>
<tr>
<td><code>connections_active</code></td>
<td>Die Gesamtzahl aktiver Clientverbindungen.</td>
</tr>
<tr>
<td><code>connections_handled</code></td>
<td>Die Gesamtzahl verarbeiteter Clientverbindungen.</td>
</tr>
<tr>
<td><code>connections_requests</code></td>
<td>Die Gesamtzahl angeforderter Clientverbindungen.</td>
</tr>
<tr>
<td><code>connections_waiting</code></td>
<td>Die Gesamtzahl wartender Clientverbindungen.</td>
</tr>
<tr>
<td><code>connections_writing</code></td>
<td>Die Gesamtzahl schreibender Clientverbindungen.</td>
</tr>
<tr>
<td><code>totalHandledRequest_1xx</code></td>
<td>Die Anzahl von Antworten mit Statuscodes 1xx.</td>
</tr>
<tr>
<td><code>totalHandledRequest_2xx</code></td>
<td>Die Anzahl von Antworten mit Statuscodes 2xx.</td>
</tr>
<tr>
<td><code>totalHandledRequest_3xx</code></td>
<td>Die Anzahl von Antworten mit Statuscodes 3xx.</td>
</tr>
<tr>
<td><code>totalHandledRequest_4xx</code></td>
<td>Die Anzahl von Antworten mit Statuscodes 4xx.</td>
</tr>
<tr>
<td><code>totalHandledRequest_5xx</code></td>
<td>Die Anzahl von Antworten mit Statuscodes 5xx.</td>
</tr>
<tr>
<td><code>totalHandledRequest_total</code></td>
<td>Die Gesamtzahl von Clientanforderungen, die von Clients empfangen wurden.</td>
  </tr></tbody>
</table>

### Servermetriken
{: #server_metrics}

Der Pod `alb-metrics-exporter` formatiert jedes Datenfeld in der JSON-Datei automatisch in eine Metrik um, die von Prometheus gelesen werden kann. In Servermetriken werden Daten in der Unterdomäne erfasst, die in einer Ingress-Ressource definiert ist, wie zum Beispiel: `dev.demostg1.stg.us.south.containers.appdomain.cloud`.
{: shortdesc}

Servermetriken haben das Format `kube_system_server_<ALB-ID>_<SUBTYP>_<SERVERNAME>_<METRIKNAME> <WERT>`.

Die Komponenten `<SERVERNAME>_<METRIKNAME>` werden als Bezeichnungen (Labels) formatiert. Beispiel: `albId="dev_demostg1_us-south_containers_appdomain_cloud",metric="out"`

Wenn der Server zum Beispiel insgesamt 22319 Byte an Clients gesendet hat, wird die Metrik wie folgt formatiert:
```
kube_system_server_public_cra6a6eb9e897e41c4a5e58f957b417aec_alb1_bytes{albId="dev_demostg1_us-south_containers_appdomain_cloud",app="alb-metrics-exporter",instance="172.30.140.68:9913",job="kubernetes-pods",kubernetes_namespace="kube-system",kubernetes_pod_name="alb-metrics-exporter-7d495d785c-8wfw4",metric="out",pod_template_hash="3805183417"} 22319
```
{: screen}

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung des Formats für Servermetriken</th>
</thead>
<tbody>
<tr>
<td><code>&lt;ALB-ID&gt;</code></td>
<td>Die ALB-ID. Im obigen Beispiel ist die ALB-ID <code>public\_cra6a6eb9e897e41c4a5e58f957b417aec\_alb1</code>.</td>
</tr>
<tr>
<td><code>&lt;SUBTYP&gt;</code></td>
<td>Der Subtyp der Metrik. Jeder Subtyp entspricht einem oder mehreren Metriknamen.
<ul>
<li><code>bytes</code> und <code>processing\_time</code> entsprechen den Metriken <code>in</code> und <code>out</code>.</li>
<li><code>cache</code> entspricht den Metriken <code>bypass</code>, <code>expired</code>, <code>hit</code>, <code>miss</code>, <code>revalidated</code>, <code>scare</code>, <code>stale</code> und <code>updating</code>.</li>
<li><code>requests</code> entspricht den Metriken <code>requestMsec</code>, <code>1xx</code>, <code>2xx</code>, <code>3xx</code>, <code>4xx</code>, <code>5xx</code> und <code>total</code>.</li></ul>
Im obigen Beispiel ist der Subtyp <code>bytes</code>.</td>
</tr>
<tr>
<td><code>&lt;SERVERNAME&gt;</code></td>
<td>Der Name des Servers, der in der Ingress-Ressource definiert ist. Aus Gründen der Kompatibilität mit Prometheus werden Punkte (<code>.</code>) durch Unterstreichungszeichen <code>(\_)</code> ersetzt. Im obigen Beispiel ist der Servername <code>dev_demostg1_stg_us_south_containers_appdomain_cloud</code>.</td>
</tr>
<tr>
<td><code>&lt;METRIKNAME&gt;</code></td>
<td>Der Name des erfassten Metriktyps. Eine Liste der Metriknamen finden Sie in der folgenden Tabelle "Unterstützte Servermetriken". Im obigen Beispiel ist der Metrikname <code>out</code>.</td>
</tr>
<tr>
<td><code>&lt;WERT&gt;</code></td>
<td>Der Wert der erfassten Metrik. Im obigen Beispiel ist der Wert <code>22319</code>.</td>
</tr>
</tbody></table>

In der folgenden Tabelle sind die unterstützten Servermetriknamen aufgelistet.

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Unterstützte Servermetriken</th>
</thead>
<tbody>
<tr>
<td><code>in</code></td>
<td>Die Gesamtzahl Byte, die von Clients empfangen wurden.</td>
</tr>
<tr>
<td><code>out</code></td>
<td>Die Gesamtzahl Byte, die an Clients gesendet wurden.</td>
</tr>
<tr>
<td><code>bypass</code></td>
<td>Die Häufigkeit (Anzahl der Male), mit der ein im Cache speicherbares Element vom ursprünglichen Server abgerufen wurde, weil es den Schwellenwert für das Speichern im Cache (z. B. Anzahl Anforderungen) nicht erfüllt hat.</td>
</tr>
<tr>
<td><code>expired</code></td>
<td>Die Häufigkeit, mit der ein Element im Cache gefunden wurde, jedoch nicht ausgewählt wurde, weil es abgelaufen war.</td>
</tr>
<tr>
<td><code>hit</code></td>
<td>Die Häufigkeit, mit der ein gültiges Element im Cache ausgewählt wurde.</td>
</tr>
<tr>
<td><code>miss</code></td>
<td>Die Häufigkeit, mit der kein gültiges Element im Cache gefunden wurde und der Server das Element vom ursprünglichen Server abgerufen hat.</td>
</tr>
<tr>
<td><code>revalidated</code></td>
<td>Die Häufigkeit, mit der ein abgelaufenes Element im Cache wieder gültig gemacht wurde.</td>
</tr>
<tr>
<td><code>scarce</code></td>
<td>Die Häufigkeit, mit der selten verwendete Elemente oder Elemente niedriger Priorität aus dem Cache entfernt wurden, um knappen Hauptspeicher freizugeben.</td>
</tr>
<tr>
<td><code>stale</code></td>
<td>Die Häufigkeit, mit der ein abgelaufenes Element im Cache gefunden wurde, das Element jedoch im Cache ausgewählt wurde, weil eine andere Anforderung den Server veranlasste, das Element vom ursprünglichen Server abzurufen.</td>
</tr>
<tr>
<td><code>updating</code></td>
<td>Die Häufigkeit, mit der veralteter Inhalt aktualisiert wurde.</td>
</tr>
<tr>
<td><code>requestMsec</code></td>
<td>Die durchschnittlichen Anforderungsverarbeitungszeiten in Millisekunden.</td>
</tr>
<tr>
<td><code>1xx</code></td>
<td>Die Anzahl von Antworten mit Statuscodes 1xx.</td>
</tr>
<tr>
<td><code>2xx</code></td>
<td>Die Anzahl von Antworten mit Statuscodes 2xx.</td>
</tr>
<tr>
<td><code>3xx</code></td>
<td>Die Anzahl von Antworten mit Statuscodes 3xx.</td>
</tr>
<tr>
<td><code>4xx</code></td>
<td>Die Anzahl von Antworten mit Statuscodes 4xx.</td>
</tr>
<tr>
<td><code>5xx</code></td>
<td>Die Anzahl von Antworten mit Statuscodes 5xx.</td>
</tr>
<tr>
<td><code>total</code></td>
<td>Die Gesamtzahl von Antworten mit Statuscodes.</td>
  </tr></tbody>
</table>

### Upstream-Metriken
{: #upstream_metrics}

Der Pod `alb-metrics-exporter` formatiert jedes Datenfeld in der JSON-Datei automatisch in eine Metrik um, die von Prometheus gelesen werden kann. In Upstream-Metriken werden Daten in dem Back-End-Service erfasst, der in einer Ingress-Ressource definiert ist.
{: shortdesc}

Upstream-Metriken werden auf zwei Arten formatiert.
* [Typ 1](#type_one) schließt den Upstream-Servicenamen ein.
* [Typ 2](#type_two) schließt den Upstream-Servicenamen und eine bestimmte Upstream-Pod-IP-Adresse ein.

#### Upstream-Metriken vom Typ 1
{: #type_one}

Upstream-Metriken vom Typ 1 haben das Format `kube_system_upstream_<ALB-ID>_<SUBTYP>_<UPSTREAM-NAME>_<METRIKNAME> <WERT>`.
{: shortdesc}

`<UPSTREAM-NAME>_<METRIKNAME>` werden als Bezeichnungen (Labels) formatiert. Beispiel: `albId="default-cafe-ingress-dev_demostg1_us-south_containers_appdomain_cloud-coffee-svc",metric="in"`

Wenn der Upstream-Service zum Beispiel insgesamt 1227 Byte aus der ALB empfangen hat, wird die Metrik wie folgt formatiert:
```
kube_system_upstream_public_cra6a6eb9e897e41c4a5e58f957b417aec_alb1_bytes{albId="default-cafe-ingress-dev_demostg1_us-south_containers_appdomain_cloud-coffee-svc",app="alb-metrics-exporter",instance="172.30.140.68:9913",job="kubernetes-pods",kubernetes_namespace="kube-system",kubernetes_pod_name="alb-metrics-exporter-7d495d785c-8wfw4",metric="in",pod_template_hash="3805183417"} 1227
```
{: screen}

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung des Formats für Upstream-Metriken vom Typ 1</th>
</thead>
<tbody>
<tr>
<td><code>&lt;ALB-ID&gt;</code></td>
<td>Die ALB-ID. Im obigen Beispiel ist die ALB-ID <code>public\_crf02710f54fcc40889c301bfd6d5b77fe\_alb1</code>.</td>
</tr>
<tr>
<td><code>&lt;SUBTYP&gt;</code></td>
<td>Der Subtyp der Metrik. Unterstützte Werte sind <code>bytes</code>, <code>processing\_time</code> und <code>requests</code>. Im obigen Beispiel ist der Subtyp <code>bytes</code>.</td>
</tr>
<tr>
<td><code>&lt;UPSTREAM-NAME&gt;</code></td>
<td>Der Name des Upstream-Service, der in der Ingress-Ressource definiert ist. Aus Gründen der Kompatibilität mit Prometheus werden Punkte (<code>.</code>) durch Unterstreichungszeichen <code>(\_)</code> ersetzt. Im obigen Beispiel ist der Upstream-Name <code>default-cafe-ingress-dev_demostg1_us-south_containers_appdomain_cloud-coffee-svc</code>.</td>
</tr>
<tr>
<td><code>&lt;METRIKNAME&gt;</code></td>
<td>Der Name des erfassten Metriktyps. Eine Liste der Metriknamen finden Sie in der folgenden Tabelle "Unterstützte Upstream-Metriken vom Typ 1". Im obigen Beispiel ist der Metrikname <code>in</code>.</td>
</tr>
<tr>
<td><code>&lt;WERT&gt;</code></td>
<td>Der Wert der erfassten Metrik. Im obigen Beispiel ist der Wert <code>1227</code>.</td>
</tr>
</tbody></table>

In der folgenden Tabelle sind die unterstützten Upstream-Metriknamen vom Typ 1 aufgelistet.

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Unterstützte Upstream-Metriken vom Typ 1</th>
</thead>
<tbody>
<tr>
<td><code>in</code></td>
<td>Die Gesamtzahl Byte, die vom ALB-Server empfangen wurden.</td>
</tr>
<tr>
<td><code>out</code></td>
<td>Die Gesamtzahl Byte, die an den ALB-Server gesendet wurden.</td>
</tr>
<tr>
<td><code>1xx</code></td>
<td>Die Anzahl von Antworten mit Statuscodes 1xx.</td>
</tr>
<tr>
<td><code>2xx</code></td>
<td>Die Anzahl von Antworten mit Statuscodes 2xx.</td>
</tr>
<tr>
<td><code>3xx</code></td>
<td>Die Anzahl von Antworten mit Statuscodes 3xx.</td>
</tr>
<tr>
<td><code>4xx</code></td>
<td>Die Anzahl von Antworten mit Statuscodes 4xx.</td>
</tr>
<tr>
<td><code>5xx</code></td>
<td>Die Anzahl von Antworten mit Statuscodes 5xx.</td>
</tr>
<tr>
<td><code>total</code></td>
<td>Die Gesamtzahl von Antworten mit Statuscodes.</td>
  </tr></tbody>
</table>

#### Upstream-Metriken vom Typ 2
{: #type_two}

Upstream-Metriken vom Typ 2 haben das Format `kube_system_upstream_<ALB-ID>_<METRIKNAME>_<UPSTREAM-NAME>_<POD-IP> <WERT>`.
{: shortdesc}

`<UPSTREAM-NAME>_<POD-IP>` werden als Bezeichnungen (Labels) formatiert. Beispiel: `albId="default-cafe-ingress-dev_dev_demostg1_us-south_containers_appdomain_cloud-tea-svc",backend="172_30_75_6_80"`

Wenn der Upstream-Service zum Beispiel eine durchschnittliche Anforderungsverarbeitungszeit (einschließlich Upstream) von 40 Millisekunden hat, wird die Metrik wie folgt formatiert:
```
kube_system_upstream_public_cra6a6eb9e897e41c4a5e58f957b417aec_alb1_requestMsec{albId="default-cafe-ingress-dev_dev_demostg1_us-south_containers_appdomain_cloud-tea-svc",app="alb-metrics-exporter",backend="172_30_75_6_80",instance="172.30.75.3:9913",job="kubernetes-pods",kubernetes_namespace="kube-system",kubernetes_pod_name="alb-metrics-exporter-7d495d785c-swkls",pod_template_hash="3805183417"} 40
```

{: screen}

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung des Formats für Upstream-Metriken vom Typ 2</th>
</thead>
<tbody>
<tr>
<td><code>&lt;ALB-ID&gt;</code></td>
<td>Die ALB-ID. Im obigen Beispiel ist die ALB-ID <code>public\_cra6a6eb9e897e41c4a5e58f957b417aec\_alb1</code>.</td>
</tr>
<td><code>&lt;UPSTREAM-NAME&gt;</code></td>
<td>Der Name des Upstream-Service, der in der Ingress-Ressource definiert ist. Aus Gründen der Kompatibilität mit Prometheus werden Punkte (<code>.</code>) durch Unterstreichungszeichen (<code>\_</code>) ersetzt. Im obigen Beispiel ist der Upstream-Name <code>demostg1\_stg\_us\_south\_containers\_appdomain\_cloud\_tea\_svc</code>.</td>
</tr>
<tr>
<td><code>&lt;POD-IP&gt;</code></td>
<td>Die IP-Adresse und der Port eines bestimmten Upstream-Service-Pods. Aus Gründen der Kompatibilität mit Prometheus werden Punkte (<code>.</code>) und Doppelpunkte (<code>:</code>) durch Unterstreichungszeichen <code>(_)</code> ersetzt. Im obigen Beispiel ist die IP-Adresse des Upstream-Pods <code>172_30_75_6_80</code>.</td>
</tr>
<tr>
<td><code>&lt;METRIKNAME&gt;</code></td>
<td>Der Name des erfassten Metriktyps. Eine Liste der Metriknamen finden Sie in der folgenden Tabelle "Unterstützte Upstream-Metriken vom Typ 2". Im obigen Beispiel ist der Metrikname <code>requestMsec</code>.</td>
</tr>
<tr>
<td><code>&lt;WERT&gt;</code></td>
<td>Der Wert der erfassten Metrik. Im obigen Beispiel ist der Wert <code>40</code>.</td>
</tr>
</tbody></table>

In der folgenden Tabelle sind die unterstützten Upstream-Metriknamen vom Typ 2 aufgelistet.

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Unterstützte Upstream-Metriken vom Typ 2</th>
</thead>
<tbody>
<tr>
<td><code>requestMsec</code></td>
<td>Die durchschnittlichen Anforderungsverarbeitungszeiten, einschließlich Upstream, in Millisekunden.</td>
</tr>
<tr>
<td><code>responseMsec</code></td>
<td>Die durchschnittliche Antwortverarbeitungszeiten nur für Upstream in Millisekunden.</td>
  </tr></tbody>
</table>

<br />


## Größe der gemeinsam genutzten Speicherzone für die Erfassung von Ingress-Metriken erhöhen
{: #vts_zone_size}

Gemeinsam genutzte Speicherzonen sind so definiert, dass Workerprozesse Informationen wie Cache, Sitzungspersistenz und Ratenlimit gemeinsam nutzen können. Eine gemeinsam genutzte Speicherzone, die als virtuelle Host-Verkehrsstatuszone bezeichnet wird, wird für Ingress eingerichtet, um Messdaten für eine ALB zu erfassen.
{:shortdesc}

In der Ingress-Konfigurationszuordnung `ibm-cloud-provider-ingress-cm` legt das Feld `vts-status-zone-size` die Größe der gemeinsam genutzten Speicherzone für die Messdatenerfassung fest. Standardmäßig ist der Wert für `vts-status-zone-size` auf `10m` gesetzt. Wenn Sie über eine große Umgebung verfügen, die mehr Speicher für die Erfassung von Messwerten benötigt, können Sie den Standardwert überschreiben, um stattdessen einen größeren Wert zu verwenden, indem Sie die folgenden Schritte ausführen.

Stellen Sie zunächst sicher, dass Sie die [{{site.data.keyword.Bluemix_notm}} IAM-Servicerolle **Schreibberechtigter** (Writer) oder **Manager**](/docs/containers?topic=containers-users#platform) für den Namensbereich `kube-system` innehaben.

1. Bearbeiten Sie die Konfigurationsdatei für die Konfigurationszuordnungsressource (Configmap) `ibm-cloud-provider-ingress-cm`.

    ```
    kubectl edit cm ibm-cloud-provider-ingress-cm -n kube-system
    ```
    {: pre}

2. Ändern Sie den Wert für `vts-status-zone-size` von `10m` in einen größeren Wert.

   ```
   apiVersion: v1
   data:
     vts-status-zone-size: "10m"
   kind: ConfigMap
   metadata:
     name: ibm-cloud-provider-ingress-cm
     namespace: kube-system
   ```
   {: codeblock}

3. Speichern Sie die Konfigurationsdatei.

4. Stellen Sie sicher, dass die Änderungen an der Konfigurationszuordnung angewendet wurden.

   ```
   kubectl get cm ibm-cloud-provider-ingress-cm -n kube-system -o yaml
   ```
   {: pre}
