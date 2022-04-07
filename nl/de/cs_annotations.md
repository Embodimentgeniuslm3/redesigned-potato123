---

copyright:
  years: 2014, 2019
lastupdated: "2019-04-15"

keywords: kubernetes, iks, ingress

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


# Ingress mit Annotationen anpassen
{: #ingress_annotation}

Um Ihrer Lastausgleichsfunktion für Ingress-Anwendungen (ALB) Funktionalität hinzuzufügen, können Sie Annotationen als Metadaten in einer Ingress-Ressource angeben.
{: shortdesc}

Bevor Sie Annotationen verwenden, stellen Sie sicher, dass Sie Ihre Ingress-Servicekonfiguration ordnungsgemäß eingerichtet haben. Führen Sie dazu die Schritte unter [HTTPS-Lastausgleich mit Ingress-Lastausgleichsfunktionen für Anwendungen (ALB)](/docs/containers?topic=containers-ingress) aus. Sobald Sie die Ingress-ALB mit einer Basiskonfiguration eingerichtet haben, können Sie deren Funktionalität durch das Hinzufügen von Annotationen zur Ingress-Ressourcendatei erweitern.
{: note}

<table>
<caption>Allgemeine Annotationen</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>Allgemeine Annotationen</th>
<th>Name</th>
<th>Beschreibung</th>
</thead>
<tbody>
<tr>
<td><a href="#custom-errors">Angepasste Fehleraktionen</a></td>
<td><code>custom-errors, custom-error-actions</code></td>
<td>Angepasste Aktionen angeben, die die ALB für bestimmte HTTP-Fehler ausführen kann.</td>
</tr>
<tr>
<td><a href="#location-snippets">Positionssnippets</a></td>
<td><code>location-snippets</code></td>
<td>Angepasste Positionsblockkonfiguration für einen Service hinzufügen.</td>
</tr>
<tr>
<td><a href="#alb-id">Privates ALB-Routing</a></td>
<td><code>ALB-ID</code></td>
<td>Eingehende Anforderungen an Ihre Apps mit einer privaten ALB weiterleiten.</td>
</tr>
<tr>
<td><a href="#server-snippets">Server-Snippets</a></td>
<td><code>server-snippets</code></td>
<td>Angepasste Serverblockkonfiguration hinzufügen.</td>
</tr>
</tbody></table>

<br>

<table>
<caption>Annotationen für Verbindungen</caption>
<col width="20%">
<col width="20%">
<col width="60%">
 <thead>
 <th>Annotationen für Verbindungen</th>
 <th>Name</th>
 <th>Beschreibung</th>
  </thead>
  <tbody>
  <tr>
  <td><a href="#proxy-connect-timeout">Angepasste Verbindungs- und Lesezeitlimits</a></td>
  <td><code>proxy-connect-timeout, proxy-read-timeout</code></td>
  <td>Zeitraum festlegen, über den die Lastausgleichsfunktion für Anwendungen auf das Herstellen einer Verbindung mit bzw. auf das Lesen von Daten aus der Back-End-App warten soll, bis die Back-End-App als nicht verfügbar betrachtet wird.</td>
  </tr>
  <tr>
  <td><a href="#keepalive-requests">Keepalive-Anforderungen</a></td>
  <td><code>keepalive-requests</code></td>
  <td>Maximale Anzahl von Anforderungen festlegen, die über eine Keepalive-Verbindung bedient werden können.</td>
  </tr>
  <tr>
  <td><a href="#keepalive-timeout">Keepalive-Zeitlimit</a></td>
  <td><code>keepalive-timeout</code></td>
  <td>Maximale Zeitspanne festlegen, die eine Keepalive-Verbindung auf dem Server geöffnet bleibt.</td>
  </tr>
  <tr>
  <td><a href="#proxy-next-upstream-config">Proxy zu nächstem Upstream</a></td>
  <td><code>proxy-next-upstream-config</code></td>
  <td>Festlegen, wann die ALB eine Anforderung an den nächsten Upstream-Server übergeben kann.</td>
  </tr>
  <tr>
  <td><a href="#sticky-cookie-services">Sitzungsaffinität mit Cookies</a></td>
  <td><code>sticky-cookie-services</code></td>
  <td>Eingehenden Netzverkehr mithilfe eines permanenten Cookies immer an denselben Upstream-Server weiterleiten.</td>
  </tr>
  <tr>
  <td><a href="#upstream-fail-timeout">Upstream-Fehlerzeitlimit</a></td>
  <td><code>upstream-fail-timeout</code></td>
  <td>Festlegen, wie lange die ALB versuchen kann, eine Verbindung zum Server herzustellen, bevor der Server als nicht verfügbar betrachtet wird.</td>
  </tr>
  <tr>
  <td><a href="#upstream-keepalive">Keepalive-Verbindungen für Upstream-Server</a></td>
  <td><code>upstream-keepalive</code></td>
  <td>Maximale Anzahl der inaktiven Keepalive-Verbindungen für einen Upstream-Server festlegen.</td>
  </tr>
  <tr>
  <td><a href="#upstream-max-fails">Maximale Anzahl Upstream-Fehler</a></td>
  <td><code>upstream-max-fails</code></td>
  <td>Maximale Anzahl nicht erfolgreicher Versuche festlegen, mit dem Server zu kommunizieren, bevor der Server als nicht verfügbar betrachtet wird.</td>
  </tr>
  </tbody></table>

<br>

  <table>
  <caption>Annotationen für HTTPS- und TLS/SSL-Authentifizierung</caption>
  <col width="20%">
  <col width="20%">
  <col width="60%">
  <thead>
  <th>Annotationen für HTTPS- und TLS/SSL-Authentifizierung</th>
  <th>Name</th>
  <th>Beschreibung</th>
  </thead>
  <tbody>
  <tr>
  <td><a href="#custom-port">Angepasste HTTP- und HTTPS-Ports</a></td>
  <td><code>custom-port</code></td>
  <td>Standardports für den HTTP-Netzverkehr (Port 80) und den HTTPS-Netzverkehr (Port 443) ändern.</td>
  </tr>
  <tr>
  <td><a href="#redirect-to-https">HTTP-Weiterleitungen an HTTPS</a></td>
  <td><code>redirect-to-https</code></td>
  <td>Unsichere HTTP-Anforderungen an Ihre Domäne an HTTPS weiterleiten.</td>
  </tr>
  <tr>
  <td><a href="#hsts">HTTP Strict Transport Security (HSTS)</a></td>
  <td><code>hsts</code></td>
  <td>Browser so einstellen, dass nur über HTTPS auf die Domäne zugegriffen wird.</td>
  </tr>
  <tr>
  <td><a href="#mutual-auth">Gegenseitige Authentifizierung</a></td>
  <td><code>mutual-auth</code></td>
  <td>Gegenseitige Authentifizierung für die Lastausgleichsfunktion für Anwendungen (ALB) konfigurieren.</td>
  </tr>
  <tr>
  <td><a href="#ssl-services">Unterstützung für SSL-Services</a></td>
  <td><code>ssl-services</code></td>
  <td>Unterstützung für SSL-Services zulassen, um den Datenverkehr zu Ihren Upstream-Apps, für die HTTPS erforderlich ist, zu verschlüsseln.</td>
  </tr>
  <tr>
  <td><a href="#tcp-ports">TCP-Ports</a></td>
  <td><code>tcp-ports</code></td>
  <td>Zugriff auf eine App über einen nicht standardmäßigen TCP-Port.</td>
  </tr>
  </tbody></table>

<br>

<table>
<caption>Pfadroutingannotationen</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>Pfadroutingannotationen</th>
<th>Name</th>
<th>Beschreibung</th>
</thead>
<tbody>
<tr>
<td><a href="#proxy-external-service">Externe Services</a></td>
<td><code>proxy-external-service</code></td>
<td>Pfaddefinitionen zu externen Services wie beispielsweise einem in {{site.data.keyword.Bluemix_notm}} gehosteten Service hinzufügen.</td>
</tr>
<tr>
<td><a href="#location-modifier">Positionsmodifikator</a></td>
<td><code>location-modifier</code></td>
<td>Die Art und Weise ändern, in der die ALB die Anforderungs-URI mit dem App-Pfad abgleicht.</td>
</tr>
<tr>
<td><a href="#rewrite-path">Pfade neu schreiben</a></td>
<td><code>rewrite-path</code></td>
<td>Eingehenden Netzverkehr an einen anderen Pfad weiterleiten, den Ihre Back-End-App überwacht.</td>
</tr>
</tbody></table>

<br>

<table>
<caption>Annotationen für Proxypuffer</caption>
<col width="20%">
<col width="20%">
<col width="60%">
 <thead>
 <th>Annotationen für Proxypuffer</th>
 <th>Name</th>
 <th>Beschreibung</th>
 </thead>
 <tbody>
 <tr>
<td><a href="#large-client-header-buffers">Puffer für große Client-Header</a></td>
<td><code>large-client-header-buffers</code></td>
<td>Die maximale Anzahl und Größe der Puffer festlegen, die große Clientanforderungsheader lesen.</td>
</tr>
 <tr>
 <td><a href="#proxy-buffering">Pufferung von Clientantwortdaten</a></td>
 <td><code>proxy-buffering</code></td>
 <td>Pufferung einer Clientantwort in der Lastausgleichsfunktion für Anwendungen (ALB) inaktivieren, während die Antwort an den Client gesendet wird.</td>
 </tr>
 <tr>
 <td><a href="#proxy-buffers">Proxy-Puffer</a></td>
 <td><code>proxy-buffers</code></td>
 <td>Anzahl und Größe der Puffer festlegen, mit denen eine Antwort für eine einzelne Verbindung von einem Proxy-Server gelesen wird.</td>
 </tr>
 <tr>
 <td><a href="#proxy-buffer-size">Puffergröße des Proxys</a></td>
 <td><code>proxy-buffer-size</code></td>
 <td>Größe des Puffers festlegen, der den ersten Teil der Antwort liest, die vom Proxy-Server empfangen wird.</td>
 </tr>
 <tr>
 <td><a href="#proxy-busy-buffers-size">Größe belegter Puffer des Proxys</a></td>
 <td><code>proxy-busy-buffers-size</code></td>
 <td>Größe für Proxypuffer festlegen, die belegt sein können.</td>
 </tr>
 </tbody></table>

<br>

<table>
<caption>Annotationen für Anforderungen und Antworten</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>Annotationen für Anforderungen und Antworten</th>
<th>Name</th>
<th>Beschreibung</th>
</thead>
<tbody>
<tr>
<td><a href="#add-host-port">Server-Port zum Host-Header hinzufügen</a></td>
<td><code>add-host-port</code></td>
<td>Server-Port zum Host für Routenanforderungen hinzufügen.</td>
</tr>
<tr>
<td><a href="#client-max-body-size">Größe des Hauptteils der Clientanforderung</a></td>
<td><code>client-max-body-size</code></td>
<td>Die maximale Größe des Hauptteils anpassen, den der Client als Teil der Anforderung senden kann.</td>
</tr>
<tr>
<td><a href="#proxy-add-headers">Zusätzlicher Clientanforderungs- oder -antwortheader</a></td>
<td><code>proxy-add-headers, response-add-headers</code></td>
<td>Einer Clientanforderung Headerinformationen hinzufügen, bevor Sie die Anforderung an Ihre Back-End-App weiterleiten, bzw. einer Clientantwort, bevor Sie die Antwort an den Client senden.</td>
</tr>
<tr>
<td><a href="#response-remove-headers">Entfernen des Clientantwortheaders</a></td>
<td><code>response-remove-headers</code></td>
<td>Headerinformationen aus einer Clientantwort entfernen, bevor die Antwort an den Client weitergeleitet wird.</td>
</tr>
</tbody></table>

<br>

<table>
<caption>Annotationen für Servicegrenzwerte</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>Annotationen für Servicegrenzwerte</th>
<th>Name</th>
<th>Beschreibung</th>
</thead>
<tbody>
<tr>
<td><a href="#global-rate-limit">Grenzwerte für globale Rate</a></td>
<td><code>global-rate-limit</code></td>
<td>Verarbeitungsrate für Anforderungen und Anzahl der Verbindungen anhand eines definierten Schlüssels für alle Services begrenzen.</td>
</tr>
<tr>
<td><a href="#service-rate-limit">Grenzwerte für Servicerate</a></td>
<td><code>service-rate-limit</code></td>
<td>Verarbeitungsrate für Anforderungen und Anzahl der Verbindungen anhand eines definierten Schlüssels für bestimmte Services begrenzen.</td>
</tr>
</tbody></table>

<br>

<table>
<caption>Annotationen für Benutzerauthentifizierung</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>Annotationen für Benutzerauthentifizierung</th>
<th>Name</th>
<th>Beschreibung</th>
</thead>
<tbody>
<tr>
<td><a href="#appid-auth">{{site.data.keyword.appid_short}}-Authentifizierung</a></td>
<td><code>appid-auth</code></td>
<td>{{site.data.keyword.appid_full}} zur Authentifizierung bei Ihrer App verwenden.</td>
</tr>
</tbody></table>

<br>

## Allgemeine Annotationen
{: #general}

### Angepasste Fehleraktionen (`custom-errors`, `custom-error-actions`)
{: #custom-errors}

Angepasste Aktionen angeben, die die ALB für bestimmte HTTP-Fehler ausführen kann.
{: shortdesc}

**Beschreibung**</br>
Zur Behandlung bestimmter HTTP-Fehler, die möglicherweise auftreten, können Sie angepasste Fehleraktionen einrichten, die die ALB ausgeführt soll.

* Die Annotation `custom-errors` definiert den Servicenamen, den zu behandelnden HTTP-Fehler und den Namen der Fehleraktion, die die ALB ausführt, wenn der angegebene HTTP-Fehler für den Service auftritt.
* Die Annotation `custom-error-actions` definiert die angepassten Fehleraktionen in NGINX-Code-Snippets.

Beispiel: In der Annotation `custom-errors` können Sie die ALB zur Behandlung von HTTP-Fehlern `401` HTTP für `app1` einrichten, indem Sie eine angepasste Fehleraktion mit dem Namen `/errorAction401` zurückgeben. Anschließend können Sie in der Annotation `custom-error-actions` ein Code-Snippet mit dem Namen `/errorAction401` definieren, sodass die ALB eine angepasste Fehlerseite an den Client zurückgibt.</br>

Sie können die Annotation `custom-errors` außerdem dazu verwenden, den Client an einen Fehlerservice umzuleiten, den Sie verwalten. Sie müssen den Pfad zu diesem Fehlerservice im Abschnitt `paths` der Ingress-Ressourcendatei definieren.

**YAML-Beispiel für eine Ingress-Ressource**</br>

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/custom-errors: "serviceName=<app1> httpError=<401> errorActionName=</errorAction401>;serviceName=<app2> httpError=<403> errorActionName=</errorPath>"
    ingress.bluemix.net/custom-error-actions: |
         errorActionName=</errorAction401>
         #Example custom error snippet
         proxy_pass http://example.com/forbidden.html;
         <EOS>
  spec:
    tls:
    - hosts:
      - mydomain
      secretName: mysecret
    rules:
    - host: mydomain
      http:
        paths:
        - path: /path1
          backend:
            serviceName: app1
            servicePort: 80
        - path: /path2
          backend:
            serviceName: app2
            servicePort: 80
        - path: </errorPath>
          backend:
            serviceName: <error-svc>
            servicePort: 80
```
{: codeblock}

<table>
<caption>Erklärung der Komponenten von Annotationen</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der Komponenten von Annotationen</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Ersetzen Sie <code>&lt;<em>app1</em>&gt;</code> durch den Namen des Kubernetes-Service, auf den sich die angepasste Fehlerbehandlung bezieht. Die angepasste Fehlerbehandlung gilt nur für die bestimmten Pfade, die genau diesen vorausgehenden Service (Upstream-Service) verwenden. Wenn Sie keinen Servicenamen festlegen, werden die angepassten Fehlerbehandlungen auf alle Servicepfade angewendet.</td>
</tr>
<tr>
<td><code>httpError</code></td>
<td>Ersetzen Sie <code>&lt;<em>401</em>&gt;</code> durch den HTTP-Fehlercode, den Sie mit einer angepassten Fehleraktion behandeln wollen.</td>
</tr>
<tr>
<td><code>errorActionName</code></td>
<td>Ersetzen Sie <code>&lt;<em>/errorAction401</em>&gt;</code> durch den Namen einer angepassten Fehleraktion, die ausgeführt werden soll, oder durch einen Fehlerservice.<ul>
<li>Wenn Sie den Namen einer angepassten Fehleraktion angeben, müssen Sie diese Fehleraktion in einem Code-Snippet in der Annotation <code>custom-error-actions</code> definieren. Im YAML-Beispiel verwendet <code>app1</code> die Fehleraktion <code>/errorAction401</code>, die in dem Snippet in der Annotation <code>custom-error-actions</code> definiert ist.</li>
<li>Wenn Sie den Pfad zu einem Fehlerservice angeben, müssen Sie den Fehlerpfad und den Namen des Fehlerservice im Abschnitt <code>paths</code> angeben. Im YAML-Beispiel verwendet <code>app2</code> den Fehlerpfad <code>/errorPath</code>, der am Ende des Abschnitts <code>paths</code> definiert ist.</li></ul></td>
</tr>
<tr>
<td><code>ingress.bluemix.net/custom-error-actions</code></td>
<td>Definieren Sie eine angepasste Fehleraktion, die von der ALB für den Service und den angegebenen HTTP-Fehler ausgeführt wird. Verwenden Sie ein NGINX-Code-Snippet und schließen Sie jedes Snippet mit <code>&lt;EOS&gt;</code> ab. Im YAML-Beispiel übergibt die ALB eine angepasste Fehlerseite mit dem Pfad <code>http://example.com/forbidden.html</code> an den Client, wenn ein Fehler <code>401</code> für <code>app1</code> auftritt.</td>
</tr>
</tbody></table>

<br />


### Positionssnippets (`location-snippets`)
{: #location-snippets}

Angepasste Positionsblockkonfiguration für einen Service hinzufügen.
{:shortdesc}

**Beschreibung**</br>
Ein Serverblock ist eine NGINX-Anweisung, die die Konfiguration für den virtuellen ALB-Server definiert. Ein Positionsblock ist eine NGINX-Anweisung, die innerhalb des Serverblocks definiert ist. Positionsblöcke definieren, wie Ingress den Anforderungs-URI verarbeitet, oder den Teil der Anforderung, der nach dem Domänennamen oder der IP-Adresse und dem Port kommt.

Wenn ein Serverblock eine Anforderung empfängt, gleicht der Positionsblock den URI mit einem Pfad ab und die Anforderung wird an die IP-Adresse des Pods weitergeleitet, auf dem die App implementiert ist. Wenn Sie die Annotation `location-snippets` verwenden, können Sie die Art und Weise ändern, wie der Positionsblock Anforderungen an bestimmte Services weiterleitet.

Wenn Sie stattdessen den Serverblock als Ganzes ändern möchten, hilft Ihnen die Annotation [`server-snippets`](#server-snippets) weiter.

Zum Anzeigen von Server- und Positionsblöcken in der NGINX-Konfigurationsdatei führen Sie den folgenden Befehl für einen Ihrer ALB-Pods aus: `kubectl exec -ti <alb_pod> -n kube-system -c nginx-ingress -- cat ./etc/nginx/default-<ingress_resource_name>.conf`
{: tip}

**YAML-Beispiel für eine Ingress-Ressource**</br>

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/location-snippets: |
      serviceName=<myservice1>
      # Example location snippet
      proxy_request_buffering off;
      rewrite_log on;
      proxy_set_header "x-additional-test-header" "location-snippet-header";
      <EOS>
      serviceName=<myservice2>
      proxy_set_header Authorization "";
      <EOS>
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080
```
{: codeblock}

<table>
<caption>Erklärung der Komponenten von Annotationen</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der Komponenten von Annotationen</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Ersetzen Sie <code>&lt;<em>mein_service</em>&gt;</code> durch den Namen des Service, den Sie für Ihre App erstellt haben.</td>
</tr>
<tr>
<td>Positionssnippet</td>
<td>Geben Sie das Konfigurationssnippet an, das Sie für den angegebenen Service verwenden möchten. Dieses Beispielsnippet für den Service <code>mein_service1</code> konfiguriert den Positionsblock, um die Pufferung von Proxy-Anforderungen zu blockieren, Protokollumschreibvorgänge zu aktivieren und zusätzliche Header festzulegen, wenn er eine Anforderung an den Service weiterleitet. Das Beispielsnippet für den Service <code>mein_service2</code> legt einen leeren Header <code>Authorization</code> fest. Jedes Positionssnippet muss auf den Wert <code>&lt;EOS&gt;</code> enden.</td>
</tr>
</tbody></table>

<br />


### Private ALB-Weiterleitung (`ALB-ID`)
{: #alb-id}

Eingehende Anforderungen an Ihre Apps mit einer privaten ALB weiterleiten.
{:shortdesc}

**Beschreibung**</br>
Wählen Sie statt der öffentlichen eine private Lastausgleichsfunktion für Anwendungen (ALB) für die Weiterleitung von eingehenden Anforderungen aus.

**YAML-Beispiel für eine Ingress-Ressource**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/ALB-ID: "<private_ALB_ID_1>;<private_ALB_ID_2>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080
```
{: codeblock}

<table>
<caption>Erklärung der Komponenten von Annotationen</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der Komponenten von Annotationen</th>
</thead>
<tbody>
<tr>
<td><code>&lt;private_ALB-ID&gt;</code></td>
<td>Die ID für Ihre private ALB. Führen Sie den Befehl <code>ibmcloud ks albs --cluster &lt;mein_cluster&gt;</code> aus, um nach der privaten ALB-ID zu suchen.<p>
Wenn Sie einen Mehrzonencluster mit mehreren privaten ALB aktiviert haben, können Sie eine Liste mit ALB-IDs bereitstellen, die durch <code>;</code> voneinander getrennt sind. Beispiel: <code>ingress.bluemix.net/ALB-ID: &lt;private_ALB_ID_1&gt;;&lt;private_ALB_ID_2&gt;;&lt;private_ALB_ID_3&gt;</code></p>
</td>
</tr>
</tbody></table>

<br />


### Server-Snippets (`server-snippets`)
{: #server-snippets}

Fügen Sie eine angepasste Serverblockkonfiguration hinzu.
{:shortdesc}

**Beschreibung**</br>
Ein Serverblock ist eine NGINX-Anweisung, die die Konfiguration für den virtuellen ALB-Server definiert. Wenn Sie ein angepasstes Konfigurationssnippet in der Annotation `server-snippets` angeben, können Sie die Art und Weise ändern, wie die ALB Anforderungen auf Serverebene verarbeitet.

Zum Anzeigen von Server- und Positionsblöcken in der NGINX-Konfigurationsdatei führen Sie den folgenden Befehl für einen Ihrer ALB-Pods aus: `kubectl exec -ti <alb_pod> -n kube-system -c nginx-ingress -- cat ./etc/nginx/default-<ingress_resource_name>.conf`
{: tip}

**YAML-Beispiel für eine Ingress-Ressource**</br>

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/server-snippets: |
      # Example snippet
      location = /health {
      return 200 'Healthy';
      add_header Content-Type text/plain;
      }
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080
```
{: codeblock}

<table>
<caption>Erklärung der Komponenten von Annotationen</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der Komponenten von Annotationen</th>
</thead>
<tbody>
<tr>
<td>Server-Snippet</td>
<td>Geben Sie das Konfigurationssnippet an, das Sie verwenden möchten. Dieses Beispielsnippet gibt einen Positionsblock für die Bearbeitung von <code>/health</code>-Anforderungen an. Der Positionsblock ist so konfiguriert, dass er eine Antwort zum einwandfreien Zustand zurückgibt und einen Header hinzufügt, wenn er eine Anforderung weiterleitet.</td>
</tr>
</tbody>
</table>

Mithilfe der Annotation `server-snippets` können Sie einen Header für alle Serviceantworten auf Serverebene hinzufügen:
{: tip}

```
annotations:
  ingress.bluemix.net/server-snippets: |
    add_header <header1> <value1>;
```
{: codeblock}

<br />


## Annotationen für Verbindungen
{: #connection}

Mithilfe von Verbindungsannotationen können Sie ändern, wie die ALB eine Verbindung zu der Back-End-App und den Upstream-Servern herstellt, und Zeitlimits oder eine maximale Anzahl von Keepalive-Verbindungen festlegen, bevor die App oder der Server als nicht verfügbar angenommen wird.
{: shortdesc}

### Angepasste Verbindungs- und Lesezeitlimits (`proxy-connect-timeout`, `proxy-read-timeout`)
{: #proxy-connect-timeout}

Legen Sie den Zeitraum fest, über den die Lastausgleichsfunktion für Anwendungen auf das Herstellen einer Verbindung mit bzw. auf das Lesen von Daten aus der Back-End-App warten soll, bis die Back-End-App als nicht verfügbar betrachtet wird.
{:shortdesc}

**Beschreibung**</br>
Wenn eine Clientanforderung an die Ingress-Lastausgleichsfunktion für Anwendungen (Ingress-ALB) gesendet wird, wird von der ALB eine Verbindung mit der Back-End-App geöffnet. Standardmäßig wartet die Lastausgleichsfunktion für Anwendungen (ALB) 60 Sekunden auf eine Antwort von einer Back-End-App. Falls die Back-End-App nicht innerhalb von 60 Sekunden antwortet, wird die Verbindungsanforderung abgebrochen und die Back-End-App als nicht verfügbar angenommen.

Nachdem die Lastausgleichsfunktion für Anwendungen (ALB) mit der Back-End-App verbunden wurde, werden Antwortdaten aus der Back-End-App von der ALB gelesen. Zwischen zwei Leseoperationen wartet die Lastausgleichsfunktion für Anwendungen (ALB) maximal 60 Sekunden auf Daten aus der Back-End-App. Falls die Back-End-App innerhalb von 60 Sekunden keine Daten sendet, wird die Verbindung zur Back-End-App gekappt und die App wird als nicht verfügbar angenommen.

Ein Verbindungszeitlimit und Lesezeitlimit von 60 Sekunden ist die Standardeinstellung auf einem Proxy und sollte in der Regel nicht geändert werden.

Falls die Verfügbarkeit Ihrer App nicht durchgehend gewährleistet ist oder Ihre App aufgrund hoher Workloads langsam antwortet, können Sie die Verbindungs- und Lesezeitlimits nach oben korrigieren. Denken Sie daran, dass sich das Erhöhen des Zeitlimits negativ auf die Leistung der Lastausgleichsfunktion für Anwendungen (ALB) auswirken kann, da die Verbindung mit der Back-End-App geöffnet bleiben muss, bis das Zeitlimit erreicht wurde.

Sie können andererseits das Zeitlimit nach unten korrigieren, um die Leistung der Lastausgleichsfunktion für Anwendungen (ALB) zu verbessern. Stellen Sie sicher, dass Ihre Back-End-App Anforderungen auch bei höheren Workloads innerhalb des angegebenen Zeitlimits verarbeiten kann.

**YAML-Beispiel für eine Ingress-Ressource**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/proxy-connect-timeout: "serviceName=<myservice> timeout=<connect_timeout>"
   ingress.bluemix.net/proxy-read-timeout: "serviceName=<myservice> timeout=<read_timeout>"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080
```
{: codeblock}

<table>
<caption>Erklärung der Komponenten von Annotationen</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der Komponenten von Annotationen</th>
</thead>
<tbody>
<tr>
<td><code>&lt;verbindungszeitlimit&gt;</code></td>
<td>Die Anzahl der Sekunden oder Minuten, die gewartet werden soll, bis eine Verbindung zur Back-End-App hergestellt wird, z. B. <code>65s</code> oder <code>1m</code>. Ein Verbindungszeitlimit kann nicht größer als 75 Sekunden sein.</td>
</tr>
<tr>
<td><code>&lt;lesezeitlimit&gt;</code></td>
<td>Die Anzahl der Sekunden oder Minuten, die auf das Lesen von Daten aus der Back-End-App gewartet werden soll. Beispiel: <code>65s</code> oder <code>2m</code>.
</tr>
</tbody></table>

<br />


### Keepalive-Anforderungen (`keepalive-requests`)
{: #keepalive-requests}

**Beschreibung**</br>
Legt die maximale Anzahl von Anforderungen fest, die über eine Keepalive-Verbindung bedient werden können.

**YAML-Beispiel für eine Ingress-Ressource**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/keepalive-requests: "serviceName=<myservice> requests=<max_requests>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: <myservice>
          servicePort: 8080
```
{: codeblock}

<table>
<caption>Erklärung der Komponenten von Annotationen</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der Komponenten von Annotationen</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Ersetzen Sie <code>&lt;<em>mein_service</em>&gt;</code> durch den Namen des Kubernetes-Service, den Sie für Ihre App erstellt haben. Dieser Parameter ist optional. Die Konfiguration wird auf alle Services in der Ingress-Unterdomäne angewendet, wenn kein Service angegeben wird. Wird der Parameter bereitgestellt, werden die Keepalive-Anforderung für den angegebenen Service festgelegt. Wird der Parameter nicht bereitgestellt, werden die Keepalive-Anforderungen auf der Serverebene der Datei <code>nginx.conf</code> für alle Services festgelegt, für die die Keepalive-Anforderungen nicht konfiguriert sind.</td>
</tr>
<tr>
<td><code>requests</code></td>
<td>Ersetzen Sie <code>&lt;<em>max._anzahl_anforderungen</em>&gt;</code> durch die maximale Anzahl der Anforderungen, die über eine einzige Keepalive-Verbindung bedient werden können.</td>
</tr>
</tbody></table>

<br />


### Keepalive-Zeitlimit (`keepalive-timeout`)
{: #keepalive-timeout}

**Beschreibung**</br>
Legt die maximale Zeitspanne fest, über die eine Keepalive-Verbindung auf dem Server geöffnet bleibt.

**YAML-Beispiel für eine Ingress-Ressource**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/keepalive-timeout: "serviceName=<myservice> timeout=<time>s"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080
```
{: codeblock}

<table>
<caption>Erklärung der Komponenten von Annotationen</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der Komponenten von Annotationen</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Ersetzen Sie <code>&lt;<em>mein_service</em>&gt;</code> durch den Namen des Kubernetes-Service, den Sie für Ihre App erstellt haben. Dieser Parameter ist optional. Wird der Parameter bereitgestellt, wird das Keepalive-Zeitlimit für den angegebenen Service festgelegt. Wird der Parameter nicht bereitgestellt, wird das Keepalive-Zeitlimit auf der Serverebene der Datei <code>nginx.conf</code> für alle Services festgelegt, für die das Keepalive-Zeitlimit nicht konfiguriert ist.</td>
</tr>
<tr>
<td><code>timeout</code></td>
<td>Ersetzen Sie <code>&lt;<em>zeit</em>&gt;</code> durch den Zeitraum in Sekunden. Beispiel:<code>timeout=20s</code>. Der Wert <code>0</code> inaktiviert die Keepalive-Verbindungen für den Client.</td>
</tr>
</tbody></table>

<br />


### Proxy zu nächstem Upstream (`proxy-next-upstream-config`)
{: #proxy-next-upstream-config}

Festlegen, wann die ALB eine Anforderung an den nächsten Upstream-Server übergeben kann.
{:shortdesc}

**Beschreibung**</br>
Die Ingress-Lastausgleichsfunktion für Anwendungen (Ingress-ALB) fungiert als Proxy zwischen der Client-App und Ihrer App. Für einige App-Konfigurationen müssen Sie unter Umständen mehrere Upstream-Server bereitstellen, die eingehende Clientanforderungen von der ALB verarbeiten. Manchmal kann der Proxy-Server, den die ALB verwendet, keine Verbindung mit einem Upstream-Server herstellen, den die App verwendet. Die ALB kann dann versuchen, eine Verbindung mit dem nächsten Upstream-Server herzustellen, um die Anforderung stattdessen an ihn zu übergeben. Sie können die Annotation `proxy-next-upstream-config` verwenden, um festzulegen, in welchen Fällen, wie lange und wie oft die ALB versuchen kann, eine Anforderung an den nächsten Upstream-Server zu übergeben.

Wenn Sie `proxy-next-upstream-config` verwenden, ist immer ein Zeitlimit konfiguriert. Fügen Sie deshalb `timeout=true` nicht zu dieser Annotation hinzu.
{: note}

**YAML-Beispiel für eine Ingress-Ressource**</br>

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/proxy-next-upstream-config: "serviceName=<myservice1> retries=<tries> timeout=<time> error=true http_502=true; serviceName=<myservice2> http_403=true non_idempotent=true"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice1
          servicePort: 80
```
{: codeblock}

<table>
<caption>Erklärung der Komponenten von Annotationen</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der Komponenten von Annotationen</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Ersetzen Sie <code>&lt;<em>mein_service</em>&gt;</code> durch den Namen des Kubernetes-Service, den Sie für Ihre App erstellt haben.</td>
</tr>
<tr>
<td><code>retries</code></td>
<td>Ersetzen Sie <code>&lt;<em>versuche</em>&gt;</code> durch die maximale Anzahl von Versuchen, die der ALB zur Verfügung stehen, um eine Anforderung an den nächsten Upstream-Server zu übergeben. Diese Anzahl beinhaltet die ursprüngliche Anforderung. Um diese Einschränkung zu inaktivieren, geben Sie <code>0</code> an. Wenn Sie keinen Wert angeben, wird der Standardwert <code>0</code> verwendet.
</td>
</tr>
<tr>
<td><code>timeout</code></td>
<td>Ersetzen Sie <code>&lt;<em>zeit</em>&gt;</code> durch die Höchstdauer (in Sekunden), die die ALB versucht, eine Anforderung an den nächsten Upstream-Server zu übergeben. Geben Sie beispielsweise <code>30s</code> ein, um eine Dauer von 30 Sekunden festzulegen. Um diese Einschränkung zu inaktivieren, geben Sie <code>0</code> an. Wenn Sie keinen Wert angeben, wird der Standardwert <code>0</code> verwendet.
</td>
</tr>
<tr>
<td><code>error</code></td>
<td>Hat dieser Parameter den Wert <code>true</code>, übergibt die ALB eine Anforderung an den nächsten Upstream-Server, wenn ein Fehler auftritt, während gleichzeitig eine Verbindung mit dem ersten Upstream-Server eingerichtet, eine Anforderung an ihn übergeben oder der Antwortheader gelesen wird.
</td>
</tr>
<tr>
<td><code>invalid_header</code></td>
<td>Hat dieser Parameter <code>true</code>, übergibt die ALB eine Anforderung an den nächsten Upstream-Server, wenn der erste Upstream-Server eine leere oder ungültige Antwort zurückgibt.
</td>
</tr>
<tr>
<td><code>http_502</code></td>
<td>Hat dieser Parameter den Wert <code>true</code>, übergibt die ALB eine Anforderung an den nächsten Upstream-Server, wenn der erste Upstream-Server eine Antwort mit dem Code 502 zurückgibt. Sie können die folgenden HTTP-Antwortcodes bestimmen: <code>500</code>, <code>502</code>, <code>503</code>, <code>504</code>, <code>403</code>, <code>404</code>, <code>429</code>.
</td>
</tr>
<tr>
<td><code>non_idempotent</code></td>
<td>Hat dieser Parameter den Wert <code>true</code>, kann die ALB Anforderungen mit einer 'non-idempotent'-Methode an den nächsten Upstream-Server übergeben. Standardmäßig übergibt die ALB diese Anforderungen nicht an den nächsten Upstream-Server.
</td>
</tr>
<tr>
<td><code>off</code></td>
<td>Um zu verhindern, dass die ALB Anforderungen an den nächsten Upstream-Server übergibt, setzen Sie diesen Parameter auf <code>true</code>.
</td>
</tr>
</tbody></table>

<br />


### Sitzungsaffinität mit Cookies (`sticky-cookie-services`)
{: #sticky-cookie-services}

Verwenden Sie die permanente Cookie-Annotation, um Ihrer Lastausgleichsfunktion für Anwendungen (ALB) Sitzungsaffinität hinzuzufügen und eingehenden Netzverkehr immer an denselben Upstream-Server weiterzuleiten.
{:shortdesc}

**Beschreibung**</br>
Um eine hohe Verfügbarkeit zu erreichen, müssen Sie bei einigen App-Konfigurationen unter Umständen mehrere Upstream-Server bereitstellen, die eingehende Clientanforderungen verarbeiten. Wenn ein Client eine Verbindung mit Ihrer Back-End-App herstellt, kann ein Client für die Dauer einer Sitzung bzw. für die Zeit, die für den Abschluss einer Task erforderlich ist, von demselben Upstream-Server bedient werden. Sie können Ihre Lastausgleichsfunktion für Anwendungen (ALB) so konfigurieren, dass Sitzungsaffinität sichergestellt ist, indem Sie eingehenden Netzverkehr immer an denselben Upstream-Server weiterleiten.

Jeder Client, der eine Verbindung mit Ihrer Back-End-App herstellt, wird durch die Lastausgleichsfunktion für Anwendungen (ALB) einem der verfügbaren Upstream-Server zugewiesen. Die Lastausgleichsfunktion für Anwendungen (ALB) erstellt ein Sitzungscookie, das in der App des Clients gespeichert wird und das in die Headerinformationen jeder Anforderung zwischen der ALB und dem Client eingeschlossen wird. Die Informationen im Cookie stellen sicher, dass alle Anforderungen während der gesamten Sitzung von demselben Upstream-Server verarbeitet werden.

Die Nutzung von affinen Sitzungen kann die Komplexität erhöhen und die Verfügbarkeit herabsetzen. Sie haben zum Beispiel einen HTTP-Server, der einen Sitzungsstatus für eine einleitende Verbindung verwaltet, sodass der HTTP-Service nur nachfolgende Anforderungen mit demselben Sitzungsstatuswert akzeptiert. Dies kann jedoch eine einfache horizontale Skalierung des HTTP-Service verhindern. Ziehen Sie in Betracht, eine externe Datenbank wie Redis oder Memcached zum Speichern des Sitzungswerts für HTTP-Anforderungen zu verwenden, sodass Sie den Sitzungsstatus über mehrere Server hinweg verwalten können.
{: note}

Wenn Sie mehrere Services einschließen, verwenden Sie ein Semikolon (;) zum Trennen der Services.

**YAML-Beispiel für eine Ingress-Ressource**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/sticky-cookie-services: "serviceName=<myservice1> name=<cookie_name1> expires=<expiration_time1> path=<cookie_path1> hash=<hash_algorithm1>;serviceName=<myservice2> name=<cookie_name2> expires=<expiration_time2> path=<cookie_path2> hash=<hash_algorithm2>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /service1_path
        backend:
          serviceName: <myservice1>
          servicePort: 8080
      - path: /service2_path
        backend:
          serviceName: <myservice2>
          servicePort: 80
```
{: codeblock}

<table>
<caption>Erklärung der Komponenten von Annotationen</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der Komponenten von Annotationen</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Ersetzen Sie <code>&lt;<em>mein_service</em>&gt;</code> durch den Namen des Kubernetes-Service, den Sie für Ihre App erstellt haben.</td>
</tr>
<tr>
<td><code>name</code></td>
<td>Ersetzen Sie <code>&lt;<em>cookiename</em>&gt;</code> durch den Namen eines permanenten Cookies, das während einer Sitzung erstellt wird.</td>
</tr>
<tr>
<td><code>expires</code></td>
<td>Ersetzen Sie <code>&lt;<em>ablaufzeit</em>&gt;</code> durch den Zeitraum in Sekunden (s), Minuten (m) oder Stunden (h), nach dem das permanente Cookie abläuft. Diese Zeit ist unabhängig von der Benutzeraktivität. Nachdem das Cookie abgelaufen ist, wird es durch den Web-Browser des Clients gelöscht und nicht mehr an die Lastausgleichsfunktion für Anwendungen (ALB) gesendet. Um beispielsweise eine Ablaufzeit von einer Sekunde, einer Minute oder einer Stunde festzulegen, geben Sie <code>1s</code>, <code>1m</code> oder <code>1h</code> ein.</td>
</tr>
<tr>
<td><code>path</code></td>
<td>Ersetzen Sie <code>&lt;<em>cookiepfad</em>&gt;</code> durch den Pfad, der an die Ingress-Unterdomäne angehängt werden soll und der angibt, für welche Domänen und Unterdomänen das Cookie an die Lastausgleichsfunktion für Anwendungen (ALB) gesendet wird. Wenn Ihre Ingress-Domäne beispielsweise <code>www.myingress.com</code> ist und Sie das Cookie in jeder Clientanforderung senden möchten, müssen Sie <code>path=/</code> festlegen. Wenn Sie das Cookie nur für <code>www.myingress.com/myapp</code> und alle zugehörigen Unterdomänen senden möchten, müssen Sie <code>path=/myapp</code> festlegen.</td>
</tr>
<tr>
<td><code>hash</code></td>
<td>Ersetzen Sie <code>&lt;<em>hashalgorithmus</em>&gt;</code> durch den Hashalgorithmus, der die Informationen im Cookie schützt. Nur <code>sha1</code> wird unterstützt. SHA1 erstellt eine Hashsumme auf der Grundlage der Informationen im Cookie und hängt die Hashsumme an das Cookie an. Der Server kann die Informationen im Cookie entschlüsseln und die Datenintegrität bestätigen.</td>
</tr>
</tbody></table>

<br />


### Upstream-Fehlerzeitlimit (`upstream-fail-timeout`)
{: #upstream-fail-timeout}

Legen Sie fest, wie lange die ALB versuchen kann, eine Verbindung zum Server herzustellen.
{:shortdesc}

**Beschreibung**</br>
Legen Sie fest, wie lange die ALB versuchen kann, eine Verbindung zu einem Server herzustellen, bevor der Server als nicht verfügbar betrachtet wird. Damit ein Server als nicht verfügbar betrachtet wird, muss die ALB die maximale Anzahl fehlgeschlagener Verbindungsversuche innerhalb der Zeit erreichen, die durch die Annotation [`upstream-max-fails`](#upstream-max-fails) festgelegt ist. Diese Zeit bestimmt auch, wie lange der Server als nicht verfügbar betrachtet wird.

**YAML-Beispiel für eine Ingress-Ressource**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/upstream-fail-timeout: "serviceName=<myservice> fail-timeout=<fail_timeout>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080
```
{: codeblock}

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der Komponenten von Annotationen</th>
</thead>
<tbody>
<tr>
<td><code>serviceName (optional)</code></td>
<td>Ersetzen Sie <code>&lt;<em>mein_service</em>&gt;</code> durch den Namen des Kubernetes-Service, den Sie für Ihre App erstellt haben.</td>
</tr>
<tr>
<td><code>fail-timeout</code></td>
<td>Ersetzen Sie <code>&lt;<em>fail_timeout</em>&gt;</code> durch die Zeit, die die ALB versuchen kann, eine Verbindung zu einem Server herzustellen, bevor der Server als nicht verfügbar betrachtet wird. Der Standardwert ist <code>10s</code>. Die Zeit muss in Sekunden angegeben werden.</td>
</tr>
</tbody></table>

<br />


### Keepalive-Verbindungen für Upstream-Server (`upstream-keepalive`)
{: #upstream-keepalive}

Maximale Anzahl der inaktiven Keepalive-Verbindungen für einen Upstream-Server festlegen.
{:shortdesc}

**Beschreibung**</br>
Legt die maximale Anzahl inaktiver Keepalive-Verbindungen zum Upstream-Server für einen angegebenen Service fest. Der Upstream-Server verfügt standardmäßig über 64 inaktive Keepalive-Verbindungen.

**YAML-Beispiel für eine Ingress-Ressource**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/upstream-keepalive: "serviceName=<myservice> keepalive=<max_connections>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080
```
{: codeblock}

<table>
<caption>Erklärung der Komponenten von Annotationen</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der Komponenten von Annotationen</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Ersetzen Sie <code>&lt;<em>mein_service</em>&gt;</code> durch den Namen des Kubernetes-Service, den Sie für Ihre App erstellt haben.</td>
</tr>
<tr>
<td><code>keepalive</code></td>
<td>Ersetzen Sie <code>&lt;<em>max._anzahl_verbindungen</em>&gt;</code> durch die maximal zulässige Anzahl der inaktiven Keepalive-Verbindungen zum Upstream-Server. Der Standardwert ist <code>64</code>. Bei Angabe des Werts <code>0</code> werden Keepalive-Verbindungen zum Upstream-Server für den angegebenen Service inaktiviert.</td>
</tr>
</tbody></table>

<br />


### Maximale Anzahl Upstream-Fehler (`upstream-max-fails`)
{: #upstream-max-fails}

Legen Sie die maximale Anzahl nicht erfolgreicher Versuche fest, mit dem Server zu kommunizieren.
{:shortdesc}

**Beschreibung**</br>
Legt die maximale Anzahl der fehlgeschlagenen Versuche der ALB zum Herstellen einer Verbindung fest, bevor der Server als nicht verfügbar betrachtet wird. Damit der Server als nicht verfügbar betrachtet wird, muss die ALB die maximale Anzahl innerhalb der Zeit erreichen, die durch die Annotation [`upstream-fail-timeout`](#upstream-fail-timeout) festgelegt ist. Die Zeitdauer, für die der Server als nicht verfügbar betrachtet wird, wird auch durch die Annotation `upstream-fail-timeout` festgelegt.

**YAML-Beispiel für eine Ingress-Ressource**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/upstream-max-fails: "serviceName=<myservice> max-fails=<max_fails>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080
```
{: codeblock}

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der Komponenten von Annotationen</th>
</thead>
<tbody>
<tr>
<td><code>serviceName (optional)</code></td>
<td>Ersetzen Sie <code>&lt;<em>mein_service</em>&gt;</code> durch den Namen des Kubernetes-Service, den Sie für Ihre App erstellt haben.</td>
</tr>
<tr>
<td><code>max-fails</code></td>
<td>Ersetzen Sie <code>&lt;<em>max_fails</em>&gt;</code> durch die maximale Anzahl nicht erfolgreicher Versuche der ALB, mit dem Server zu kommunizieren. Der Standardwert ist <code>1</code>. Der Wert <code>0</code> inaktiviert die Annotation.</td>
</tr>
</tbody></table>

<br />


## Annotationen für HTTPS- und TLS/SSL-Authentifizierung
{: #https-auth}

Mithilfe von HTTPS- und TLS/SSL-Authentifizierungsannotationen können Sie Ihre ALB für HTTPS-Datenverkehr konfigurieren, HTTPS-Standardports ändern, SSL-Verschlüsselung für Datenverkehr aktivieren, der an Ihre Back-End-Apps gesendet wird, oder eine gegenseitige Authentifizierung konfigurieren.
{: shortdesc}

### Angepasste HTTP- und HTTPS-Ports (`custom-port`)
{: #custom-port}

Änderung der Standardports für den HTTP-Netzverkehr (Port 80) und den HTTPS-Netzverkehr (Port 443).
{:shortdesc}

**Beschreibung**</br>
Standardmäßig ist die Ingress-Lastausgleichsfunktion für Anwendungen (ALB) so konfiguriert, dass sie für eingehenden HTTP-Netzverkehr an Port 80 und für eingehenden HTTPS-Netzverkehr an Port 443 empfangsbereit ist. Sie können die Standardports ändern, um die Sicherheit der Domäne Ihrer Lastausgleichsfunktion für Anwendungen (ALB) zu verbessern oder um ausschließlich einen HTTPS-Port zu aktivieren.

Um die gegenseitige Authentifizierung an einem Port zu ermöglichen, [konfigurieren Sie die Lastausgleichsfunktion für Anwendungen (ALB) zum Öffnen des gültigen Ports](/docs/containers?topic=containers-ingress#opening_ingress_ports) und geben Sie den Port dann in der Annotation [`mutual-auth` an](#mutual-auth). Verwenden Sie die Annotation `custom-port` nicht, um einen Port für die gegenseitige Authentifizierung anzugeben.
{: note}

**YAML-Beispiel für eine Ingress-Ressource**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/custom-port: "protocol=<protocol1> port=<port1>;protocol=<protocol2> port=<port2>"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080
```
{: codeblock}

<table>
<caption>Erklärung der Komponenten von Annotationen</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der Komponenten von Annotationen</th>
 </thead>
 <tbody>
 <tr>
 <td><code>&lt;protokoll&gt;</code></td>
 <td>Geben Sie <code>http</code> oder <code>https</code> ein, um den Standardport für eingehenden HTTP- bzw. HTTPS-Netzverkehr zu ändern.</td>
 </tr>
 <tr>
 <td><code>&lt;port&gt;</code></td>
 <td>Geben Sie die Portnummer ein, die für den eingehenden HTTP- oder HTTPS-Datenaustausch im Netz verwendet werden soll.  <p class="note">Wenn für HTTP oder HTTPS ein angepasster Port angegeben wird, dann sind die Standardports für HTTP und auch für HTTPS nicht mehr gültig. Um beispielsweise den Standardport für HTTPS in 8443 zu ändern, für HTTP jedoch weiterhin den Standardport zu verwenden, müssen Sie für beide Protokolle angepasste Ports festlegen: <code>custom-port: "protocol=http port=80; protocol=https port=8443"</code>.</p></td>
 </tr>
</tbody></table>


**Syntax**</br>
1. Überprüfen Sie offene Ports für Ihre ALB.
  ```
  kubectl get service -n kube-system
  ```
  {: pre}

  Die Ausgabe in der Befehlszeilenschnittstelle (CLI) ähnelt der folgenden:
  ```
  NAME                                             TYPE           CLUSTER-IP     EXTERNAL-IP    PORT(S)                      AGE
  public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx    169.xx.xxx.xxx 80:30416/TCP,443:32668/TCP   109d
  ```
  {: screen}

2. Öffnen Sie die ALB-Konfigurationszuordnung.
  ```
  kubectl edit configmap ibm-cloud-provider-ingress-cm -n kube-system
  ```
  {: pre}

3. Fügen Sie die nicht dem Standard entsprechenden HTTP- und HTTPS-Ports zur Konfigurationszuordnung hinzu. Ersetzen Sie `<port>` durch den HTTP- oder HTTPS-Port, der geöffnet werden soll.
  <p class="note">Standardmäßig sind Port 80 und 443 geöffnet. Wenn Port 80 und 443 geöffnet bleiben sollen, müssen Sie sie neben allen anderen TCP-Ports einschließen, die Sie im Feld `public-ports` angegeben haben. Wenn Sie eine private Lastausgleichsfunktion für Anwendungen aktiviert haben, müssen Sie auch alle Ports im Feld `private-ports` angeben, die geöffnet bleiben sollen. Weitere Informationen finden Sie im Abschnitt [Ports für den Ingress-Lastenausgleich öffnen](/docs/containers?topic=containers-ingress#opening_ingress_ports).</p>
  ```
  apiVersion: v1
  kind: ConfigMap
  data:
    public-ports: <port1>;<port2>
  metadata:
    creationTimestamp: 2017-08-22T19:06:51Z
    name: ibm-cloud-provider-ingress-cm
    namespace: kube-system
    resourceVersion: "1320"
    selfLink: /api/v1/namespaces/kube-system/configmaps/ibm-cloud-provider-ingress-cm
    uid: <uid>
  ```
  {: codeblock}

4. Überprüfen Sie, ob Ihre ALB erneut mit den nicht dem Standard entsprechenden Ports konfiguriert wurde.
  ```
  kubectl get service -n kube-system
  ```
  {: pre}

  Die Ausgabe in der Befehlszeilenschnittstelle (CLI) ähnelt der folgenden:
  ```
  NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP    PORT(S)                           AGE
  public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx    169.xx.xxx.xxx <port1>:30776/TCP,<port2>:30412/TCP   109d
  ```
  {: screen}

5. Konfigurieren Sie Ingress zur Verwendung der nicht dem Standard entsprechenden Ports bei der Weiterleitung von eingehendem Netzverkehr an Ihre Services. Verwenden Sie die Annotation in der YAML-Beispieldatei in dieser Referenz. 

6. Aktualisieren Sie Ihre ALB-Konfiguration.
  ```
  kubectl apply -f myingress.yaml
  ```
  {: pre}

7. Öffnen Sie Ihren bevorzugten Web-Browser, um auf Ihre App zuzugreifen. Beispiel: `https://<ibmdomain>:<port>/<service_path>/`

<br />


### HTTP-Weiterleitungen an HTTPS (`redirect-to-https`)
{: #redirect-to-https}

Konvertierung von unsicheren HTTP-Clientanforderungen in HTTPS.
{:shortdesc}

**Beschreibung**</br>
Sie konfigurieren Ihre Ingress-Lastausgleichsfunktion für Anwendungen (Ingress-ALB) für die Sicherung Ihrer Domäne mit dem von IBM bereitgestellten TLS-Zertifikat oder Ihrem angepassten TLS-Zertifikat. Manche Benutzer versuchen unter Umständen, auf Ihre Apps zuzugreifen, indem Sie eine unsichere `http`-Anforderung an Ihre ALB-Domäne senden; z. B. `http://www.myingress.com`, anstatt `https` zu verwenden. Sie können die 'redirect'-Annotation verwenden, um unsichere HTTP-Anforderungen immer in HTTPS zu konvertieren. Wenn Sie diese Annotation nicht verwenden, werden unsichere HTTP-Anforderungen nicht standardmäßig in HTTPS-Anforderungen konvertiert und machen unter Umständen vertrauliche Daten ohne Verschlüsselung öffentlich zugänglich.


Das Umadressieren von HTTP-Anforderungen in HTTPS ist standardmäßig inaktiviert.

**YAML-Beispiel für eine Ingress-Ressource**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/redirect-to-https: "True"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080
```
{: codeblock}

<br />


### HTTP Strict Transport Security (`hsts`)
{: #hsts}

**Beschreibung**</br>
HSTS weist den Browser an, nur über HTTPS auf eine Domäne zuzugreifen. Selbst wenn der Benutzer einen einfachen HTTP-Link eingibt oder einem solchen folgt, aktualisiert der Browser die Verbindung strikt auf HTTPS:

**YAML-Beispiel für eine Ingress-Ressource**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/hsts: enabled=true maxAge=<31536000> includeSubdomains=true
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /service1_path
        backend:
          serviceName: myservice1
          servicePort: 8443
      - path: /service2_path
        backend:
          serviceName: myservice2
          servicePort: 8444
          ```
{: codeblock}

<table>
<caption>Erklärung der Komponenten von Annotationen</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der Komponenten von Annotationen</th>
</thead>
<tbody>
<tr>
<td><code>enabled</code></td>
<td>Verwenden Sie <code>true</code>, um HSTS zu aktivieren.</td>
</tr>
<tr>
<td><code>maxAge</code></td>
<td>Ersetzen Sie <code>&lt;<em>31536000</em>&gt;</code> durch eine ganze Zahl, die angibt, wie viele Sekunden ein Browser Sendeanforderungen direkt an HTTPS zwischenspeichert. Der Standardwert ist <code>31536000</code>, was 1 Jahr entspricht.</td>
</tr>
<tr>
<td><code>includeSubdomains</code></td>
<td>Verwenden Sie <code>true</code>, um dem Browser mitzuteilen, dass die HSTS-Richtlinie auch für alle Unterdomänen der aktuellen Domäne gilt. Der Standard ist <code>true</code>. </td>
</tr>
</tbody></table>

<br />


### Gegenseitige Authentifizierung (`mutual-auth`)
{: #mutual-auth}

Gegenseitige Authentifizierung für die Lastausgleichsfunktion für Anwendungen (ALB) konfigurieren.
{:shortdesc}

**Beschreibung**</br>
Konfigurieren Sie die gegenseitige Authentifizierung für Downstream-Datenverkehr für die Ingress-ALB. Der externe Client authentifiziert den Server und der Server authentifiziert den Client anhand von Zertifikaten. Die gegenseitige Authentifizierung wird auch als zertifikatsbasierte Authentifizierung oder Zweiwegeauthentifizierung bezeichnet.

Verwenden Sie die Annotation `mutual-auth` für die SSL-Terminierung zwischen dem Client und der Ingress-Lastausgleichsfunktion für Anwendungen. Verwenden Sie die [Annotation `ssl-services`](#ssl-services) für die SSL-Terminierung zwischen der Ingress-Lastausgleichsfunktion für Anwendungen und der Back-End-App.

Die Annotation für gegenseitige Authentifizierung validiert Clientzertifikate. Zur Weiterleitung von Clientzertifikaten in einem Header für die Anwendungen zur Handhabung von Berechtigungen können Sie die folgende Annotation [`proxy-add-headers` verwenden](#proxy-add-headers): `"ingress.bluemix.net/proxy-add-headers": "serviceName=router-set {\n X-Forwarded-Client-Cert $ssl_client_escaped_cert;\n}\n"`
{: tip}

**Voraussetzungen**</br>

* Sie müssen über einen gültigen geheimen Schlüssel für die gegenseitige Authentifizierung verfügen, der das erforderliche Zertifikat `ca.crt` enthält. Informationen zum Erstellen eines geheimen Schlüssels für gegenseitige Authentifizierung finden Sie in den Schritten am Ende dieses Abschnitts.
* Um die gegenseitige Authentifizierung an einem anderen Port als 443 zu ermöglichen, [konfigurieren Sie die Lastausgleichsfunktion für Anwendungen (ALB) zum Öffnen des gültigen Ports](/docs/containers?topic=containers-ingress#opening_ingress_ports) und geben Sie den Port dann in dieser Annotation an. Verwenden Sie die Annotation `custom-port` nicht, um einen Port für die gegenseitige Authentifizierung anzugeben.

**YAML-Beispiel für eine Ingress-Ressource**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/mutual-auth: "secretName=<mysecret> port=<port> serviceName=<servicename1>,<servicename2>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080
```
{: codeblock}

<table>
<caption>Erklärung der Komponenten von Annotationen</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der Komponenten von Annotationen</th>
</thead>
<tbody>
<tr>
<td><code>secretName</code></td>
<td>Ersetzen Sie <code>&lt;<em>mein_geheimer_schlüssel</em>&gt;</code> durch einen Namen für die Ressource mit dem geheimen Schlüssel.</td>
</tr>
<tr>
<td><code>port</code></td>
<td>Ersetzen Sie <code>&lt;<em>port</em>&gt;</code> durch die ALB-Portnummer.</td>
</tr>
<tr>
<td><code>serviceName</code></td>
<td>Ersetzen Sie <code>&lt;<em>servicename</em>&gt;</code> durch den Namen mindestens einer Ingress-Ressource. Dieser Parameter ist optional.</td>
</tr>
</tbody></table>

**Gehen Sie wie folgt vor, um einen geheimen Schlüssel für die gegenseitige Authentifizierung zu erstellen:**

1. Generieren Sie ein Zertifikat und einen Schlüssel der Zertifizierungsstelle über Ihren Zertifikatsanbieter. Wenn Sie über eine eigene Domäne verfügen, kaufen Sie ein offizielles TLS-Zertifikat für Ihre Domäne. Stellen Sie sicher, dass der [allgemeine Name (CN, Common Name) ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://support.dnsimple.com/articles/what-is-common-name/) für jedes Zertifikat anders ist.
    Zu Testzwecken können Sie ein selbst signiertes Zertifikat mit OpenSSL erstellen. Weitere Informationen erhalten Sie in diesem [Lernprogramm zu selbst signierten SSL-Zertifikaten ![gegenseitige Authentifizierung](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.akadia.com/services/ssh_test_certificate.html) oder in diesem [Lernprogramm zur gegenseitigen Authentifizierung, das das Erstellen einer eigenen CA ![gegenseitige Authentifizierung](../icons/launch-glyph.svg "Symbol für externen Link")](https://blog.codeship.com/how-to-set-up-mutual-tls-authentication/) beinhaltet.
    {: tip}
2. [Konvertieren Sie das Zertifikat in Base-64 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.base64encode.org/).
3. Erstellen Sie anhand des Zertifikats eine YAML-Datei für geheime Schlüssel.
   ```
   apiVersion: v1
     kind: Secret
     metadata:
       name: ssl-my-test
     type: Opaque
     data:
       ca.crt: <zertifikat_der_zertifizierungsstelle>
   ```
   {: codeblock}
4. Erstellen Sie das Zertifikat als einen geheimen Kubernetes-Schlüssel.
   ```
   kubectl create -f ssl-my-test
   ```
   {: pre}

<br />


### Unterstützung für SSL-Services (`ssl-services`)
{: #ssl-services}

Lassen Sie HTTPS-Anforderungen zu und verschlüsseln Sie Datenverkehr zu Ihren Upstream-Apps.
{:shortdesc}

**Beschreibung**</br>
Wenn Ihre Ingress-Ressourcenkonfiguration über einen TLS-Abschnitt verfügt, kann die Ingress-ALB mit HTTPS gesicherte URL-Anforderungen an Ihre App verarbeiten. Standardmäßig beendet die ALB die TLS-Terminierung und entschlüsselt die Anforderung, bevor der Datenverkehr an Ihre Apps mithilfe des HTTP-Protokolls weitergeleitet wird. Wenn Sie über Apps verfügen, die das HTTPS-Protokoll benötigen und für die der Datenverkehr verschlüsselt sein muss, verwenden Sie die Annotation `ssl-services`. Mit der Annotation `ssl-services` beendet die ALB die externe TLS-Verbindung und erstellt dann eine neue SSL-Verbindung zwischen der ALB und dem App-Pod. Der Datenverkehr wird erneut verschlüsselt, bevor er an die Upstream-Pods gesendet wird.

Wenn Ihre Back-End-App TLS verarbeiten kann und Sie weitere Sicherheitsfunktionalität hinzufügen möchten, können Sie eine unidirektionale oder gegenseitige Authentifizierung hinzufügen, indem Sie ein Zertifikat bereitstellen, das in einem geheimen Schlüssel enthalten ist.

Verwenden Sie die Annotation `ssl-services` für die SSL-Terminierung zwischen der Ingress-Lastausgleichsfunktion für Anwendungen und der Back-End-App. Verwenden Sie die [Annotation `mutual-auth`](#mutual-auth) für die SSL-Terminierung zwischen dem Client und der Ingress-Lastausgleichsfunktion für Anwendungen.
{: tip}

**YAML-Beispiel für eine Ingress-Ressource**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: <myingressname>
  annotations:
    ingress.bluemix.net/ssl-services: ssl-service=<myservice1> ssl-secret=<service1-ssl-secret>;ssl-service=<myservice2> ssl-secret=<service2-ssl-secret>
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /service1_path
        backend:
          serviceName: myservice1
          servicePort: 8443
      - path: /service2_path
        backend:
          serviceName: myservice2
          servicePort: 8444
          ```
{: codeblock}

<table>
<caption>Erklärung der Komponenten von Annotationen</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der Komponenten von Annotationen</th>
</thead>
<tbody>
<tr>
<td><code>ssl-service</code></td>
<td>Ersetzen Sie <code>&lt;<em>mein_service</em>&gt;</code> durch den Namen des Service, der HTTPS erfordert. Der Datenverkehr wird von der Lastausgleichsfunktion für Anwendungen (ALB) zu diesem App-Service verschlüsselt.</td>
</tr>
<tr>
<td><code>ssl-secret</code></td>
<td>Wenn Ihre Back-End-App TLS verarbeiten kann und Sie weitere Sicherheitsfunktionalität hinzufügen möchten, ersetzen Sie <code>&lt;<em>service-ssl-secret</em>&gt;</code> durch den geheimen Schlüssel mit unidirektionaler oder gegenseitiger Authentifizierung für den Service.<ul><li>Wenn Sie einen geheimen Schlüssel mit unidirektionaler Authentifizierung bereitstellen, muss er <code>trusted.crt</code> vom Upstream-Server enthalten. Informationen zum Erstellen eines unidirektionalen geheimen Schlüssels finden Sie in den Schritten am Ende dieses Abschnitts.</li><li>Wenn Sie einen geheimen Schlüssel mit gegenseitiger Authentifizierung bereitstellen, muss der Wert die erforderlichen Angaben für <code>client.crt</code> und <code>client.key</code> enthalten, die Ihre App vom Client erwartet. Informationen zum Erstellen eines geheimen Schlüssels für gegenseitige Authentifizierung finden Sie in den Schritten am Ende dieses Abschnitts.</li></ul><p class="important">Wenn Sie keinen geheimen Schlüssel angeben, sind unsichere Verbindungen zulässig. Sie können auf den geheimen Schlüssel verzichten, wenn Sie die Verbindung testen möchten und keine Zertifikate vorliegen oder wenn Ihre Zertifikate abgelaufen sind und Sie unsichere Verbindungen zulassen möchten.</p></td>
</tr>
</tbody></table>


**Gehen Sie wie folgt vor, um einen geheimen Schlüssel für eine unidirektionale Authentifizierung zu erstellen:**

1. Rufen Sie das Zertifikat und den Schlüssel der Zertifizierungsstelle (CA) aus dem Upstream-Server sowie ein SSL-Clientzertifikat ab. Die IBM ALB basiert auf NGINX, sodass das Stammzertifikat, das Zwischenzertifikat und das Back-End-Zertifikat erforderlich sind. Weitere Informationen finden Sie in den [NGINX-Dokumenten ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.nginx.com/nginx/admin-guide/security-controls/securing-http-traffic-upstream/).
2. [Konvertieren Sie das Zertifikat in Base-64 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.base64encode.org/).
3. Erstellen Sie anhand des Zertifikats eine YAML-Datei für geheime Schlüssel.
   ```
   apiVersion: v1
     kind: Secret
     metadata:
       name: ssl-my-test
     type: Opaque
     data:
       trusted.crt: <zertifikat_der_zertifizierungsstelle>
   ```
   {: codeblock}

   Um die gegenseitige Authentifizierung auch für den Upstream-Datenverkehr zu erzwingen, können Sie neben dem Wert für `trusted.crt` im Datenabschnitt auch Werte für `client.crt` und `client.key` angeben.
   {: tip}

4. Erstellen Sie das Zertifikat als einen geheimen Kubernetes-Schlüssel.
   ```
   kubectl create -f ssl-my-test
   ```
   {: pre}

</br>
**Gehen Sie wie folgt vor, um einen geheimen Schlüssel für die gegenseitige Authentifizierung zu erstellen:**

1. Generieren Sie ein Zertifikat und einen Schlüssel der Zertifizierungsstelle über Ihren Zertifikatsanbieter. Wenn Sie über eine eigene Domäne verfügen, kaufen Sie ein offizielles TLS-Zertifikat für Ihre Domäne. Stellen Sie sicher, dass der [allgemeine Name (CN, Common Name) ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://support.dnsimple.com/articles/what-is-common-name/) für jedes Zertifikat anders ist.
    Zu Testzwecken können Sie ein selbst signiertes Zertifikat mit OpenSSL erstellen. Weitere Informationen erhalten Sie in diesem [Lernprogramm zu selbst signierten SSL-Zertifikaten ![gegenseitige Authentifizierung](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.akadia.com/services/ssh_test_certificate.html) oder in diesem [Lernprogramm zur gegenseitigen Authentifizierung, das das Erstellen einer eigenen CA ![gegenseitige Authentifizierung](../icons/launch-glyph.svg "Symbol für externen Link")](https://blog.codeship.com/how-to-set-up-mutual-tls-authentication/) beinhaltet.
    {: tip}
2. [Konvertieren Sie das Zertifikat in Base-64 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.base64encode.org/).
3. Erstellen Sie anhand des Zertifikats eine YAML-Datei für geheime Schlüssel.
   ```
   apiVersion: v1
     kind: Secret
     metadata:
       name: ssl-my-test
     type: Opaque
     data:
       ca.crt: <zertifikat_der_zertifizierungsstelle>
   ```
   {: codeblock}
4. Erstellen Sie das Zertifikat als einen geheimen Kubernetes-Schlüssel.
   ```
   kubectl create -f ssl-my-test
   ```
   {: pre}

<br />


### TCP-Ports (`tcp-ports`)
{: #tcp-ports}

Zugriff auf eine App über einen nicht standardmäßigen TC-Port.
{:shortdesc}

**Beschreibung**</br>
Verwenden Sie diese Annotation für eine App, die eine Workload für TCP-Datenströme ausführt.

<p class="note">Die Lastausgleichsfunktion für Anwendungen (ALB) arbeitet im Durchgriffsmodus und leitet den Datenverkehr an Back-End-Apps weiter. Die SSL-Terminierung wird in diesem Fall nicht unterstützt. Die TLS-Verbindung wird nicht beendet und der Durchgriff erfolgt ungehindert.</p>

**YAML-Beispiel für eine Ingress-Ressource**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/tcp-ports: "serviceName=<myservice> ingressPort=<ingress_port> servicePort=<service_port>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 80
```
{: codeblock}

<table>
<caption>Erklärung der Komponenten von Annotationen</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der Komponenten von Annotationen</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Ersetzen Sie <code>&lt;<em>mein_service</em>&gt;</code> durch den Namen des Kubernetes-Service, auf den über einen nicht standardmäßigen TCP-Port zugegriffen werden soll.</td>
</tr>
<tr>
<td><code>ingressPort</code></td>
<td>Ersetzen Sie <code>&lt;<em>ingress-port</em>&gt;</code> durch den TCP-Port, an dem Sie auf Ihre App zugreifen wollen.</td>
</tr>
<tr>
<td><code>servicePort</code></td>
<td>Dieser Parameter ist optional. Wenn ein Wert bereitgestellt wird, wird der Port durch diesen Wert ersetzt, bevor der Datenverkehr an die Back-End-App gesendet wird. Andernfalls entspricht der Port weiterhin dem Ingress-Port. Wenn Sie diesen Parameter nicht festlegen möchten, können Sie ihn aus der Konfiguration entfernen. </td>
</tr>
</tbody></table>


**Syntax**</br>
1. Überprüfen Sie offene Ports für Ihre ALB.
  ```
  kubectl get service -n kube-system
  ```
  {: pre}

  Die Ausgabe in der Befehlszeilenschnittstelle (CLI) ähnelt der folgenden:
  ```
  NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP      PORT(S)                      AGE
  public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx    169.xx.xxx.xxx   80:30416/TCP,443:32668/TCP   109d
  ```
  {: screen}

2. Öffnen Sie die ALB-Konfigurationszuordnung.
  ```
  kubectl edit configmap ibm-cloud-provider-ingress-cm -n kube-system
  ```
  {: pre}

3. Fügen Sie die TCP-Ports zur Konfigurationszuordnung hinzu. Ersetzen Sie `<port>` durch die TCP-Ports, die geöffnet werden sollen.
  Standardmäßig sind Port 80 und 443 geöffnet. Wenn Port 80 und 443 geöffnet bleiben sollen, müssen Sie sie neben allen anderen TCP-Ports einschließen, die Sie im Feld `public-ports` angegeben haben. Wenn Sie eine private Lastausgleichsfunktion für Anwendungen aktiviert haben, müssen Sie auch alle Ports im Feld `private-ports` angeben, die geöffnet bleiben sollen. Weitere Informationen finden Sie im Abschnitt [Ports für den Ingress-Lastenausgleich öffnen](/docs/containers?topic=containers-ingress#opening_ingress_ports).
  {: note}
  ```
  apiVersion: v1
  kind: ConfigMap
  data:
    public-ports: 80;443;<port1>;<port2>
  metadata:
    creationTimestamp: 2017-08-22T19:06:51Z
    name: ibm-cloud-provider-ingress-cm
    namespace: kube-system
    resourceVersion: "1320"
    selfLink: /api/v1/namespaces/kube-system/configmaps/ibm-cloud-provider-ingress-cm
    uid: <uid>
   ```
  {: codeblock}

4. Überprüfen Sie, ob Ihre ALB erneut mit den TCP-Ports konfiguriert wurde.
  ```
  kubectl get service -n kube-system
  ```
  {: pre}
  Die Ausgabe in der Befehlszeilenschnittstelle (CLI) ähnelt der folgenden:
  ```
  NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP      PORT(S)                               AGE
  public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx    169.xx.xxx.xxx   <port1>:30776/TCP,<port2>:30412/TCP   109d
  ```
  {: screen}

5. Konfigurieren Sie die ALB für den Zugriff auf Ihre App über einen nicht standardmäßigen TCP-Port. Verwenden Sie die Annotation `tcp-ports` in der YAML-Beispieldatei in dieser Referenz.

6. Erstellen Sie entweder eine ALB-Ressource oder aktualisieren Sie Ihre vorhandene ALB-Konfiguration.
  ```
  kubectl apply -f myingress.yaml
  ```
  {: pre}

7. Führen Sie den Befehl 'curl' für die Ingress-Unterdomäne aus, um auf Ihre App zuzugreifen. Beispiel: `curl <domain>:<ingressPort>`

<br />


## Pfadroutingannotationen
{: #path-routing}

Die Ingress-ALB leitet Datenverkehr an die Pfade, über die die Back-End-Apps empfangsbereit sind. Mit Pfadroutingannotationen können Sie konfigurieren, wie die ALB Datenverkehr an Ihre Apps leitet.
{: shortdesc}

### Externe Services (`proxy-external-service`)
{: #proxy-external-service}

Hinzufügen von Pfaddefinitionen zu externen Services, wie beispielsweise in {{site.data.keyword.Bluemix_notm}} gehosteten Services.
{:shortdesc}

**Beschreibung**</br>
Fügen Sie Pfaddefinitionen zu externen Services hinzu. Verwenden Sie diese Annotation nur dann, wenn Ihre App für einen externen Service, nicht für einen Back-End-Service, betrieben wird. Wenn Sie diese Annotation verwenden, um eine externe Serviceroute zu erstellen, dann werden zusammen mit ihr nur die Annotationen `client-max-body-size`, `proxy-read-timeout`, `proxy-connect-timeout` und `proxy-buffering` unterstützt. Darüber hinaus werden keine weiteren Annotationen zusammen mit `proxy-external-service` unterstützt.

Es ist nicht möglich, mehrere Hosts für einen einzelnen Service und Pfad anzugeben.
{: note}

**YAML-Beispiel für eine Ingress-Ressource**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/proxy-external-service: "path=<mypath> external-svc=https:<external_service> host=<mydomain>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 80
```
{: codeblock}

<table>
<caption>Erklärung der Komponenten von Annotationen</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der Komponenten von Annotationen</th>
</thead>
<tbody>
<tr>
<td><code>path</code></td>
<td>Ersetzen Sie <code>&lt;<em>mein_pfad</em>&gt;</code> durch den Pfad, an dem der externe Service empfangsbereit ist.</td>
</tr>
<tr>
<td><code>external-svc</code></td>
<td>Ersetzen Sie <code>&lt;<em>externer_service</em>&gt;</code> durch den externen Service, der aufgerufen werden soll. Beispiel: <code>https://&lt;mein_service&gt;.&lt;region&gt;.mybluemix.net</code>.</td>
</tr>
<tr>
<td><code>host</code></td>
<td>Ersetzen Sie <code>&lt;<em>meine_domäne</em>&gt;</code> durch die Hostdomäne für den externen Service.</td>
</tr>
</tbody></table>

<br />


### Positionsmodifikator (`location-modifier`)
{: #location-modifier}

Die Art und Weise ändern, in der die ALB die Anforderungs-URI mit dem App-Pfad abgleicht.
{:shortdesc}

**Beschreibung**</br>
Standardmäßig verarbeiten ALBs die Pfade, auf denen Apps empfangsbereit sind, als Präfixe. Wenn eine ALB eine Anforderung an eine App empfängt, prüft die ALB die Ingress-Ressource auf einen Pfad (als Präfix), der mit dem Anfang der Anforderungs-URI übereinstimmt. Wenn eine Übereinstimmung gefunden wurde, wird die Anforderung an die IP-Adresse des Pods weitergeleitet, in dem die App bereitgestellt ist.

Die Annotation `location-modifier` ändert die Art und Weise, in der die ALB nach Übereinstimmungen sucht, indem die Positionsblockkonfiguration geändert wird. Der Positionsblock bestimmt, wie Anforderungen für den App-Pfad gehandhabt werden.

Um reguläre Ausdruckspfade (regex) zu bearbeiten, ist diese Annotation erforderlich.
{: note}

**Unterstützte Modifikatoren**</br>
<table>
<caption>Unterstützte Modifikatoren</caption>
<col width="10%">
<col width="90%">
<thead>
<th>Modifikator</th>
<th>Beschreibung</th>
</thead>
<tbody>
<tr>
<td><code>=</code></td>
<td>Der Modifikator 'Gleichheitszeichen' bewirkt, dass die ALB nur exakte Übereinstimmungen auswählt. Wenn eine exakte Übereinstimmung gefunden wird, wird die Suche gestoppt, und der übereinstimmende Pfad wird ausgewählt.<br>Wenn Ihre App beispielsweise an <code>/tea</code> empfangsbereit ist, wählt die Lastausgleichsfunktion für Anwendungen (ALB) beim Abgleich einer Anforderung mit Ihrer App nur genaue <code>/tea</code>-Pfade aus.</td>
</tr>
<tr>
<td><code>~</code></td>
<td>Der Modifikator 'Tilde' bewirkt, dass die ALB während der Suche nach Übereinstimmungen Pfade als regex-Pfade betrachtet, bei denen die Groß-/Kleinschreibung beachtet werden muss.<br>Wenn Ihre App beispielsweise am Verzeichnis <code>/coffee</code> empfangsbereit ist, kann die Lastausgleichsfunktion für Anwendungen (ALB) beim Abgleich einer Anforderung mit Ihrer App Pfade des Typs <code>/ab/coffee</code> oder <code>/123/coffee</code> auswählen, obwohl die Pfade nicht explizit für Ihre App angegeben sind.</td>
</tr>
<tr>
<td><code>~\*</code></td>
<td>Der Modifikator 'Tilde' gefolgt von einem Modifikator 'Stern' bewirkt, dass die ALB während der Suche nach Übereinstimmungen Pfade als regex-Pfade betrachtet, bei denen die Groß-/Kleinschreibung nicht beachtet werden muss.<br>Wenn Ihre App beispielsweise am Verzeichnis <code>/coffee</code> empfangsbereit ist, kann die Lastausgleichsfunktion für Anwendungen (ALB) beim Abgleich einer Anforderung mit Ihrer App Pfade des Typs <code>/ab/Coffee</code> oder <code>/123/COFFEE</code> auswählen, obwohl die Pfade nicht explizit für Ihre App angegeben sind.</td>
</tr>
<tr>
<td><code>^~</code></td>
<td>Der Modifikator 'Zirkumflex' gefolgt von einem Modifikator 'Tilde' bewirkt, dass die ALB keinen regex-Pfad, sondern die beste nicht-regex-Übereinstimmung auswählt.</td>
</tr>
</tbody>
</table>


**YAML-Beispiel für eine Ingress-Ressource**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/location-modifier: "modifier='<location_modifier>' serviceName=<myservice1>;modifier='<location_modifier>' serviceName=<myservice2>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 80
```
{: codeblock}

<table>
<caption>Erklärung der Komponenten von Annotationen</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der Komponenten von Annotationen</th>
</thead>
<tbody>
<tr>
<td><code>modifier</code></td>
<td>Ersetzen Sie <code>&lt;<em>standortmodifikator</em>&gt;</code> durch den Positionsmodifikator, den Sie für den Pfad verwenden möchten. Unterstützte Modifikatoren sind <code>'='</code>, <code>'~'</code>, <code>'~\*'</code> und <code>'^~'</code>. Sie müssen die Modifikatoren in einfache Anführungszeichen setzen.</td>
</tr>
<tr>
<td><code>serviceName</code></td>
<td>Ersetzen Sie <code>&lt;<em>mein_service</em>&gt;</code> durch den Namen des Kubernetes-Service. dem Sie für Ihre App erstellt haben.</td>
</tr>
</tbody></table>

<br />


### Pfade neu schreiben (`rewrite-path`)
{: #rewrite-path}

Leiten Sie eingehenden Netzverkehr für den Pfad in der Domäne einer Lastausgleichsfunktion für Anwendungen (ALB) an einen anderen Pfad weiter, über den die Back-End-App empfangsbereit ist.
{:shortdesc}

**Beschreibung**</br>
Die Domäne der Ingress-Lastausgleichsfunktion für Anwendungen (Ingress-ALB) leitet eingehenden Netzverkehr für `mykubecluster.us-south.containers.appdomain.cloud/beans` an Ihre App weiter. Ihre App überwacht jedoch `/coffee` und nicht `/beans`. Zum Weiterleiten des eingehenden Netzverkehrs an Ihre App fügen Sie eine Annotation zum erneuten Schreiben (rewrite) zur Konfigurationsdatei der Ingress-Ressource hinzu. Dadurch wird sichergestellt, dass der an `/beans` eingehende Netzverkehr durch den Pfad `/coffee` an Ihre App weitergeleitet wird. Wenn Sie mehrere Services einschließen, verwenden Sie nur ein Semikolon (;) zum Trennen der Services.

**YAML-Beispiel für eine Ingress-Ressource**</br>

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/rewrite-path: "serviceName=<myservice1> rewrite=<target_path1>;serviceName=<myservice2> rewrite=<target_path2>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /beans
        backend:
          serviceName: myservice1
          servicePort: 80
```
{: codeblock}

<table>
<caption>Erklärung der Komponenten von Annotationen</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der Komponenten von Annotationen</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Ersetzen Sie <code>&lt;<em>mein_service</em>&gt;</code> durch den Namen des Kubernetes-Service, den Sie für Ihre App erstellt haben.</td>
</tr>
<tr>
<td><code>rewrite</code></td>
<td>Ersetzen Sie <code>&lt;<em>zielpfad</em>&gt;</code> durch den Pfad, über den Ihre App empfangsbereit ist. Der eingehende Netzverkehr in der Domäne der Lastausgleichsfunktion für Anwendungen (ALB) wird mithilfe dieses Pfads an den Kubernetes-Service weitergeleitet. Die meisten Apps überwachen keinen bestimmten Pfad, sondern verwenden den Rootpfad und einen bestimmten Port. In dem Beispiel für <code>mykubecluster.us-south.containers.appdomain.cloud/beans</code> ist der durch 'rewrite' neu angegebene Pfad <code>/coffee</code>. <p class= "note">Wenn Sie diese Datei anwenden und die URL eine Antwort <code>404</code> anzeigt, ist es möglich, dass Ihre Back-End-App einen Pfad überwacht, der auf `/` endet. Wiederholen Sie den Versuch, indem Sie ein abschließendes Zeichen `/` an dieses Rewrite-Feld anfügen. Wenden Sie die Datei anschließend an und probieren Sie die URL erneut aus.</p></td>
</tr>
</tbody></table>

<br />


## Annotationen für Proxypuffer
{: #proxy-buffer}

Die Ingress-Lastausgleichsfunktion für Anwendungen (Ingress-ALB) fungiert als Proxy zwischen Ihrer Back-End-App und dem Client-Web-Browser. Mit Annotationen für Proxypuffer können Sie konfigurieren, wie Daten in Ihrer ALB beim Senden oder Empfangen von Datenpaketen gepuffert werden.
{: shortdesc}

### Puffer für große Client-Header (`large-client-header-buffers`)
{: #large-client-header-buffers}

Die maximale Anzahl und Größe der Puffer festlegen, die große Clientanforderungsheader lesen.
{:shortdesc}

**Beschreibung**</br>
Puffer, die große Clientanforderungsheader lesen, werden nur nach Bedarf zugeordnet: Wenn eine Verbindung nach der Verarbeitung des Anforderungsendes in den Keepalive-Status übergegangen ist, werden diese Puffer freigegeben. Standardmäßig sind `4` Puffer vorhanden und die Puffergröße ist gleich `8K` Byte. Wenn eine Anforderungszeile die festgelegte maximale Größe eines Puffers überschreitet, wird der HTTP-Fehler `414 Request-URI Too Large` an den Client zurückgegeben. Wenn außerdem ein Anforderungsfeld die festgelegte maximale Größe eines Puffers überschreitet, wird der Fehler `400 Bad Request Large` an den Client zurückgegeben. Sie können die maximale Anzahl und Größe der Puffer anpassen, die zum Lesen großer Clientanforderungsheader verwendet werden.

**YAML-Beispiel für eine Ingress-Ressource**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/large-client-header-buffers: "number=<number> size=<size>"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080
```
{: codeblock}

<table>
<caption>Erklärung der Komponenten von Annotationen</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der Komponenten von Annotationen</th>
 </thead>
 <tbody>
 <tr>
 <td><code>&lt;anzahl&gt;</code></td>
 <td>Die maximale Anzahl an Puffern, die zum Lesen großer Clientanforderungsheader zugeordnet werden sollte. Um beispielsweise 4 Puffer zu verwenden, legen Sie <code>4</code> fest.</td>
 </tr>
 <tr>
 <td><code>&lt;größe&gt;</code></td>
 <td>Die maximale Größe der Puffer, die große Clientanforderungsheader lesen. Um beispielsweise 16 Kilobyte zu verwenden, definieren Sie <code>16k</code>. Die Größe muss mit einem <code>k</code> für Kilobyte oder <code>m</code> für Megabyte enden.</td>
 </tr>
</tbody></table>

<br />


### Pufferung von Clientantwortdaten (`proxy-buffering`)
{: #proxy-buffering}

Verwenden Sie die 'buffer'-Annotation, um das Speichern von Antwortdaten in der Lastausgleichsfunktion für Anwendungen (ALB) während des Sendens von Daten an den Client zu inaktivieren.
{:shortdesc}

**Beschreibung**</br>
Die Ingress-Lastausgleichsfunktion für Anwendungen (Ingress-ALB) fungiert als Proxy zwischen Ihrer Back-End-App und dem Client-Web-Browser. Wenn eine Antwort von der Back-End-App an den Client gesendet wird, werden die Antwortdaten standardmäßig in der Lastausgleichsfunktion für Anwendungen (ALB) gepuffert. Die Lastausgleichsfunktion für Anwendungen (ALB) verarbeitet die Clientantwort als Proxy und beginnt mit dem Senden der Antwort an den Client (in der Geschwindigkeit des Clients). Nachdem alle Daten aus der Back-End-App von der Lastausgleichsfunktion für Anwendungen (ALB) empfangen wurden, wird die Verbindung zur Back-End-App gekappt. Die Verbindung von der Lastausgleichsfunktion für Anwendungen (ALB) zum Client bleibt geöffnet, bis der Client alle Daten empfangen hat.

Wenn die Pufferung von Antwortdaten auf der ALB inaktiviert ist, werden Daten sofort von der ALB an den Client gesendet. Der Client muss eingehende Daten in der Geschwindigkeit der Lastausgleichsfunktion für Anwendungen (ALB) verarbeiten können. Falls der Client zu langsam ist, gehen unter Umständen Daten verloren.

Die Pufferung von Antwortdaten auf der ALB ist standardmäßig aktiviert.

**YAML-Beispiel für eine Ingress-Ressource**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/proxy-buffering: "enabled=<false> serviceName=<myservice1>"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080
```
{: codeblock}

<table>
<caption>Erklärung der Komponenten von Annotationen</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der Komponenten von Annotationen</th>
</thead>
<tbody>
<tr>
<td><code>enabled</code></td>
<td>Um die Pufferung von Antwortdaten für die Lastausgleichsfunktion für Anwendungen (ALB) zu inaktivieren, setzen Sie diesen Wert auf <code>false</code>.</td>
</tr>
<tr>
<td><code>serviceName</code></td>
<td>Ersetzen Sie <code><em>&lt;mein_service1&gt;</em></code> durch den Namen des Kubernetes-Service, den Sie für Ihre App erstellt haben. Trennen Sie mehrere Services durch ein Semikolon (;). Dieses Feld ist optional. Wenn Sie keinen Servicenamen angeben, verwenden alle Services diese Annotation.</td>
</tr>
</tbody></table>

<br />


### Proxypuffer (`proxy-buffers`)
{: #proxy-buffers}

Konfiguration der Anzahl und Größe der Proxypuffer für die Lastausgleichsfunktion für Anwendungen (ALB).
{:shortdesc}

**Beschreibung**</br>
Legen Sie die Anzahl und Größe der Puffer fest, die eine Antwort für eine einzelne Verbindung von einem Proxy-Server lesen. Die Konfiguration wird auf alle Services in der Ingress-Unterdomäne angewendet, wenn kein Service angegeben wird. Wenn beispielsweise eine Konfiguration wie `serviceName=SERVICE number=2 size=1k` angegeben wird, wird '1k' auf den Service angewendet. Wenn eine Konfiguration wie `number=2 size=1k` angegeben wird, wird '1k' auf alle Services in der Ingress-Unterdomäne angewendet.</br>
<p class="tip">Wenn Sie die Fehlernachricht `upstream sent too big header while reading response header from upstream` erhalten, hat der Upstream-Server in Ihrem Back-End eine Headergröße größer als die Standardbegrenzung gesendet. Erhöhen Sie die Größe für `proxy-buffers` und [`proxy-buffer-size`](#proxy-buffer-size).</p>

**YAML-Beispiel für eine Ingress-Ressource**</br>

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: proxy-ingress
 annotations:
   ingress.bluemix.net/proxy-buffers: "serviceName=<myservice> number=<number_of_buffers> size=<size>"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080
```
{: codeblock}

<table>
<caption>Erklärung der Komponenten von Annotationen</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der Komponenten von Annotationen</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Ersetzen Sie <code>&lt;<em>mein_service</em>&gt;</code> durch den Namen eines Service, der 'proxy-buffers' anwenden soll.</td>
</tr>
<tr>
<td><code>number</code></td>
<td>Ersetzen Sie <code>&lt;<em>anzahl_puffer</em>&gt;</code> durch eine Zahl. Beispiel: <code>2</code>.</td>
</tr>
<tr>
<td><code>size</code></td>
<td>Ersetzen Sie <code>&lt;<em>größe</em>&gt;</code> durch die Größe der einzelnen Puffer in Kilobyte (k oder K). Beispiel: <code>1K</code>.</td>
</tr>
</tbody>
</table>

<br />


### Puffergröße des Proxys (`proxy-buffer-size`)
{: #proxy-buffer-size}

Konfiguration der Größe des Proxypuffers, der den ersten Teil der Antwort liest.
{:shortdesc}

**Beschreibung**</br>
Legt die Größe des Puffers fest, der den ersten Teil der Antwort liest, die vom Proxy-Server empfangen wird. Dieser Teil der Antwort enthält normalerweise einen kleinen Antwortheader. Die Konfiguration wird auf alle Services in der Ingress-Unterdomäne angewendet, wenn kein Service angegeben wird. Wenn beispielsweise eine Konfiguration wie `serviceName=SERVICE size=1k` angegeben wird, wird '1k' auf den Service angewendet. Wenn eine Konfiguration wie `size=1k` angegeben wird, wird '1k' auf alle Services in der Ingress-Unterdomäne angewendet.

Wenn Sie die Fehlernachricht `upstream sent too big header while reading response header from upstream` erhalten, hat der Upstream-Server in Ihrem Back-End eine Headergröße größer als die Standardbegrenzung gesendet. Erhöhen Sie die Größe für `proxy-buffer-size` und [`proxy-buffers`](#proxy-buffers).
{: tip}

**YAML-Beispiel für eine Ingress-Ressource**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: proxy-ingress
 annotations:
   ingress.bluemix.net/proxy-buffer-size: "serviceName=<myservice> size=<size>"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080
```
{: codeblock}

<table>
<caption>Erklärung der Komponenten von Annotationen</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der Komponenten von Annotationen</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Ersetzen Sie <code>&lt;<em>mein_service</em>&gt;</code> durch den Namen eines Service, der 'proxy-buffers-size' anwenden soll.</td>
</tr>
<tr>
<td><code>size</code></td>
<td>Ersetzen Sie <code>&lt;<em>größe</em>&gt;</code> durch die Größe der einzelnen Puffer in Kilobyte (k oder K). Beispiel: <code>1K</code>. Informationen zur Berechnung der geeigneten Größe finden Sie in [diesem Blogbeitrag ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.getpagespeed.com/server-setup/nginx/tuning-proxy_buffer_size-in-nginx). </td>
</tr>
</tbody></table>

<br />


### Größe belegter Puffer des Proxys (`proxy-busy-buffers-size`)
{: #proxy-busy-buffers-size}

Konfiguration der Größe für Proxy-Puffer, die belegt sein können.
{:shortdesc}

**Beschreibung**</br>
Begrenzt die Größe aller Puffer, die eine Antwort an den Client senden, noch bevor die Antwort vollständig gelesen wurde. In der Zwischenzeit können die verbleibenden Puffer die Antwort lesen und bei Bedarf einen Teil der Antwort in einer temporären Datei puffern. Die Konfiguration wird auf alle Services in der Ingress-Unterdomäne angewendet, wenn kein Service angegeben wird. Wenn beispielsweise eine Konfiguration wie `serviceName=SERVICE size=1k` angegeben wird, wird '1k' auf den Service angewendet. Wenn eine Konfiguration wie `size=1k` angegeben wird, wird '1k' auf alle Services in der Ingress-Unterdomäne angewendet.

**YAML-Beispiel für eine Ingress-Ressource**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: proxy-ingress
 annotations:
   ingress.bluemix.net/proxy-busy-buffers-size: "serviceName=<myservice> size=<size>"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080
         ```
{: codeblock}

<table>
<caption>Erklärung der Komponenten von Annotationen</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der Komponenten von Annotationen</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Ersetzen Sie <code>&lt;<em>mein_service</em>&gt;</code> durch den Namen eines Service, der 'proxy-busy-buffers-size' anwenden soll.</td>
</tr>
<tr>
<td><code>size</code></td>
<td>Ersetzen Sie <code>&lt;<em>größe</em>&gt;</code> durch die Größe der einzelnen Puffer in Kilobyte (k oder K). Beispiel: <code>1K</code>.</td>
</tr>
</tbody></table>

<br />


## Annotationen für Anforderungen und Antworten
{: #request-response}

Anforderungs- und Antwortannotationen verwenden, um Headerinformationen zu Client- und Serveranforderungen hinzuzufügen oder daraus zu entfernen und um die Größe des Hauptteils zu ändern, den der Client senden kann.
{: shortdesc}

### Server-Port zum Host-Header hinzufügen (`add-host-port`)
{: #add-host-port}

Server-Port zur Clientanforderung hinzufügen, bevor die Anforderung an Ihre Back-End-App weitergeleitet wird.
{: shortdesc}

**Beschreibung**</br>
Fügt dem Host-Header einer Clientanforderung den `:server_port` hinzu, bevor die Anforderung an Ihre Back-End-App weitergeleitet wird.

**YAML-Beispiel für eine Ingress-Ressource**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/add-host-port: "enabled=<true> serviceName=<myservice>"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080
```
{: codeblock}

<table>
<caption>Erklärung der Komponenten von Annotationen</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der Komponenten von Annotationen</th>
</thead>
<tbody>
<tr>
<td><code>enabled</code></td>
<td>Zur Aktivierung der Einstellung von 'server_port' für die Unterdomäne setzen Sie diesen Wert auf <code>true</code>.</td>
</tr>
<tr>
<td><code>serviceName</code></td>
<td>Ersetzen Sie <code><em>&lt;mein_service&gt;</em></code> durch den Namen des Kubernetes-Service, den Sie für Ihre App erstellt haben. Trennen Sie mehrere Services durch ein Semikolon (;). Dieses Feld ist optional. Wenn Sie keinen Servicenamen angeben, verwenden alle Services diese Annotation.</td>
</tr>
</tbody></table>

<br />


### Zusätzlicher Clientanforderungs- oder Clientantwortheader (`proxy-add-headers`, `response-add-headers`)
{: #proxy-add-headers}

Hinzufügen von zusätzlichen Headerinformationen zu einer Clientanforderung, bevor die Anforderung an die Back-End-App gesendet wird, bzw. zu einer Clientantwort, bevor die Antwort an den Client gesendet wird.
{:shortdesc}

**Beschreibung**</br>
Die Ingress-Lastausgleichsfunktion für Anwendungen (Ingress-ALB) fungiert als Proxy zwischen der Client-App und Ihrer Back-End-App. Clientanforderungen, die an die Lastausgleichsfunktion für Anwendungen (ALB) gesendet werden, werden (als Proxy) verarbeitet und in eine neue Anforderung umgesetzt, die anschließend an Ihre Back-End-App gesendet wird. In ähnlicher Weise werden die an die ALB gesendeten Back-End-App-Antworten verarbeitet (als Proxy) und in eine neue Antwort gestellt, die dann an den Client gesendet wird. Beim Senden einer Anforderung als Proxy oder Antwort werden HTTP-Headerinformationen entfernt, z. B. der Benutzername, der ursprünglich vom Client oder der Back-End-App gesendet wurde.

Wenn Ihre Back-End-App HTTP-Headerinformationen erfordert, können Sie die Annotation `proxy-add-headers` verwenden, um Headerinformationen zur Clientanforderung hinzuzufügen, bevor die Anforderung von der ALB an die Back-End-App weitergeleitet wird. Wenn die Client-Web-App HTTP-Headerinformationen erfordert, können Sie die Annotation `response-add-headers` verwenden, um Headerinformationen zur Antwort hinzuzufügen, bevor die  Antwort von der ALB an die Client-Web-App weitergeleitet wird.<br>

Sie müssen möglicherweise die folgenden X-Forward-Headerinformationen zur Anforderung hinzufügen, bevor sie an Ihre App weitergeleitet wird.
```
proxy_set_header Host $host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-Proto $scheme;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
```
{: screen}

Verwenden Sie die Annotation `proxy-add-headers` wie folgt, um die X-Forward-Headerinformationen der Anforderung hinzuzufügen, die an Ihre App gesendet wurde:
```
ingress.bluemix.net/proxy-add-headers: |
  serviceName=<myservice1> {
  Host $host;
  X-Real-IP $remote_addr;
  X-Forwarded-Proto $scheme;
  X-Forwarded-For $proxy_add_x_forwarded_for;
  }
```
{: codeblock}

</br>

Die Annotation `response-add-headers` unterstützt keine globalen Header für alle Services. Um einen Header für alle Serviceantworten auf Serverebene hinzuzufügen, können Sie die [Annotation `server-snippets`](#server-snippets) verwenden:
{: tip}
```
annotations:
  ingress.bluemix.net/server-snippets: |
    add_header <header1> <value1>;
```
{: pre}
</br>

**YAML-Beispiel für eine Ingress-Ressource**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/proxy-add-headers: |
      serviceName=<myservice1> {
      <header1> <value1>;
      <header2> <value2>;
      }
      serviceName=<myservice2> {
      <header3> <value3>;
      }
    ingress.bluemix.net/response-add-headers: |
      serviceName=<myservice1> {
      <header1>:<value1>;
      <header2>:<value2>;
      }
      serviceName=<myservice2> {
      <header3>:<value3>;
      }
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /service1_path
        backend:
          serviceName: <myservice1>
          servicePort: 8080
      - path: /service2_path
        backend:
          serviceName: <myservice2>
          servicePort: 80
```
{: codeblock}

<table>
<caption>Erklärung der Komponenten von Annotationen</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der Komponenten von Annotationen</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Ersetzen Sie <code>&lt;<em>mein_service</em>&gt;</code> durch den Namen des Kubernetes-Service, den Sie für Ihre App erstellt haben.</td>
</tr>
<tr>
<td><code>&lt;header&gt;</code></td>
<td>Der Schlüssel der Headerinformationen, die der Clientanforderung bzw. der Clientantwort hinzugefügt werden sollen.</td>
</tr>
<tr>
<td><code>&lt;wert&gt;</code></td>
<td>Der Wert der Headerinformationen, die der Clientanforderung bzw. der Clientantwort hinzugefügt werden sollen.</td>
</tr>
</tbody></table>

<br />


### Entfernen des Clientantwortheaders (`response-remove-headers`)
{: #response-remove-headers}

Entfernen von Headerinformationen, die in der Clientantwort von der Back-End-App eingeschlossen sind, bevor die Antwort an den Client gesendet wird.
{:shortdesc}

**Beschreibung**</br>
Die Ingress-Lastausgleichsfunktion für Anwendungen (Ingress-ALB) fungiert als Proxy zwischen Ihrer Back-End-App und dem Client-Web-Browser. Clientantworten von der Back-End-App, die an die Lastausgleichsfunktion für Anwendungen (ALB) gesendet werden, werden (als Proxy) verarbeitet und in eine neue Antwort umgesetzt, die anschließend von der Lastausgleichsfunktion für Anwendungen (ALB) an Ihren Client-Web-Browser gesendet wird. Auch wenn beim Senden einer Antwort als Proxy die HTTP-Headerinformationen entfernt werden, die ursprünglich von der Back-End-App gesendet wurden, entfernt dieser Prozess möglicherweise nicht alle Back-End-App-spezifischen Header. Entfernen Sie Headerinformationen aus einer Clientantwort, bevor die Antwort von der Lastausgleichsfunktion für Anwendungen (ALB) an den Client-Web-Browser weitergeleitet wird.

**YAML-Beispiel für eine Ingress-Ressource**</br>

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/response-remove-headers: |
      serviceName=<myservice1> {
      "<header1>";
      "<header2>";
      }
      serviceName=<myservice2> {
      "<header3>";
      }
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /service1_path
        backend:
          serviceName: <myservice1>
          servicePort: 8080
      - path: /service2_path
        backend:
          serviceName: <myservice2>
          servicePort: 80
```
{: codeblock}

<table>
<caption>Erklärung der Komponenten von Annotationen</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der Komponenten von Annotationen</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Ersetzen Sie <code>&lt;<em>mein_service</em>&gt;</code> durch den Namen des Kubernetes-Service, den Sie für Ihre App erstellt haben.</td>
</tr>
<tr>
<td><code>&lt;header&gt;</code></td>
<td>Der Schlüssel des Headers, der aus der Clientantwort entfernt werden soll.</td>
</tr>
</tbody></table>

<br />


### Größe des Hauptteils der Clientanforderung (`client-max-body-size`)
{: #client-max-body-size}

Die maximale Größe des Hauptteils anpassen, den der Client als Teil der Anforderung senden kann.
{:shortdesc}

**Beschreibung**</br>
Um die erwartete Leistung beizubehalten, ist die maximale Größe des Clientanforderungshauptteils auf 1 Megabyte festgelegt. Wenn eine Clientanforderung mit einer das Limit überschreitenden Größe des Hauptteils an die Ingress-Lastausgleichsfunktion für Anwendungen (Ingress-ALB) gesendet wird und der Client das Unterteilen von Daten nicht zulässt, gibt die Lastausgleichsfunktion für Anwendungen (ALB) die HTTP-Antwort 413 (Anforderungsentität zu groß) an den Client zurück. Eine Verbindung zwischen dem Client und der Lastausgleichsfunktion für Anwendungen (ALB) ist erst möglich, wenn der Anforderungshauptteil verkleinert wird. Wenn der Client das Unterteilen in mehrere Blöcke zulässt, werden die Daten in Pakete von 1 Megabyte aufgeteilt und an die Lastausgleichsfunktion für Anwendungen (ALB) gesendet.

Wenn Sie Clientanforderungen mit einer Hauptteilgröße über 1 Megabyte erwarten, können Sie die maximale Größe des Hauptteils erhöhen. Beispiel: Sie möchten, dass Ihr Client große Dateien hochladen kann. Das Erhöhen der maximalen Größe des Anforderungshauptteils kann sich negativ auf die Leistung Ihrer Lastausgleichsfunktion für Anwendungen (ALB) auswirken, weil die Verbindung mit dem Client geöffnet bleiben muss, bis die Anforderung empfangen wird.

Einige Client-Web-Browser können die HTTP-Antwortnachricht 413 nicht ordnungsgemäß anzeigen.
{: note}

**YAML-Beispiel für eine Ingress-Ressource**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/client-max-body-size: size=<size>
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080
```
{: codeblock}

<table>
<caption>Erklärung der Komponenten von Annotationen</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der Komponenten von Annotationen</th>
</thead>
<tbody>
<tr>
<td><code>&lt;größe&gt;</code></td>
<td>Die maximal zulässige Größe des Hauptteils der Clientantwort. Um die maximale Größe beispielsweise auf 200 Megabyte festzulegen, definieren Sie <code>200m</code>. Sie können die Größe auf '0' festlegen, um die Prüfung der Größe des Clientanforderungshauptteils zu inaktivieren.</td>
</tr>
</tbody></table>

<br />


## Annotationen für Servicegrenzwerte
{: #service-limit}

Mit Annotationen für Servicegrenzwerte können Sie die Standardverarbeitungsrate von Anforderungen und die Anzahl von Verbindungen ändern, die von einer einzelnen IP-Adresse stammen.
{: shortdesc}

### Grenzwerte für globale Rate (`global-rate-limit`)
{: #global-rate-limit}

Begrenzung der Verarbeitungsrate für Anforderungen und Anzahl der Verbindungen anhand eines definierten Schlüssels für alle Services.
{:shortdesc}

**Beschreibung**</br>
Begrenzt für alle Services durch einen definierten Schlüssel die Verarbeitungsrate für Anforderungen und die Anzahl der Verbindungen, die von einer einzelnen IP-Adresse für alle Pfade der ausgewählten Back-End-Systeme kommen.

**YAML-Beispiel für eine Ingress-Ressource**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/global-rate-limit: "key=<key> rate=<rate> conn=<number-of-connections>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080
```
{: codeblock}

<table>
<caption>Erklärung der Komponenten von Annotationen</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der Komponenten von Annotationen</th>
</thead>
<tbody>
<tr>
<td><code>key</code></td>
<td>Unterstützte Werte: `location`, `$http_`-Header und `$uri`. Um einen globalen Grenzwert für eingehende Anforderungen basierend auf der Zone oder dem Service festzulegen, verwenden Sie `key=location`. Um einen globalen Grenzwert für eingehende Anforderungen basierend auf dem Header festzulegen, verwenden Sie `X-USER-ID key=$http_x_user_id`.</td>
</tr>
<tr>
<td><code>rate</code></td>
<td>Ersetzen Sie <code>&lt;<em>rate</em>&gt;</code> durch die Verarbeitungsrate. Geben Sie einen Wert als Rate pro Sekunde (r/s) oder Rate pro Minute (r/m) an. Beispiel: <code>50r/m</code>.</td>
</tr>
<tr>
<td><code>conn</code></td>
<td>Ersetzen Sie <code>&lt;<em>number-of-connections</em>&gt;</code> durch die Anzahl der Verbindungen.</td>
</tr>
</tbody></table>

<br />


### Grenzwerte für Servicerate (`service-rate-limit`)
{: #service-rate-limit}

Begrenzung der Verarbeitungsrate für Anforderungen und Anzahl der Verbindungen für bestimmte Services.
{:shortdesc}

**Beschreibung**</br>
Begrenzt für bestimmte Services durch einen definierten Schlüssel die Verarbeitungsrate für Anforderungen und die Anzahl der Verbindungen, die von einer einzelnen IP-Adresse kommen, für alle Pfade der ausgewählten Back-End-Systeme.

**YAML-Beispiel für eine Ingress-Ressource**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/service-rate-limit: "serviceName=<myservice> key=<key> rate=<rate> conn=<number_of_connections>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080
```
{: codeblock}

<table>
<caption>Erklärung der Komponenten von Annotationen</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der Komponenten von Annotationen</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Ersetzen Sie <code>&lt;<em>mein_service</em>&gt;</code> durch den Namen des Service, für den Sie die Verarbeitungsrate begrenzen wollen.</li>
</tr>
<tr>
<td><code>key</code></td>
<td>Unterstützte Werte: `location`, `$http_`-Header und `$uri`. Um einen globalen Grenzwert für eingehende Anforderungen basierend auf der Zone oder dem Service festzulegen, verwenden Sie `key=location`. Um einen globalen Grenzwert für eingehende Anforderungen basierend auf dem Header festzulegen, verwenden Sie `X-USER-ID key=$http_x_user_id`.</td>
</tr>
<tr>
<td><code>rate</code></td>
<td>Ersetzen Sie <code>&lt;<em>rate</em>&gt;</code> durch die Verarbeitungsrate. Um eine Rate pro Sekunde zu definieren, verwenden Sie 'r/s': <code>10r/s</code>. Um eine Rate pro Minute zu definieren, verwenden Sie 'r/m': <code>50r/m</code>.</td>
</tr>
<tr>
<td><code>conn</code></td>
<td>Ersetzen Sie <code>&lt;<em>number-of-connections</em>&gt;</code> durch die Anzahl der Verbindungen.</td>
</tr>
</tbody></table>

<br />


## Annotationen für Benutzerauthentifizierung
{: #user-authentication}

Verwenden Sie Annotationen für die Benutzerauthentifizierung, wenn Sie {{site.data.keyword.appid_full_notm}} zur Authentifizierung bei Ihren Apps nutzen wollen.
{: shortdesc}

### {{site.data.keyword.appid_short_notm}}-Authentifizierung (`appid-auth`)
{: #appid-auth}

{{site.data.keyword.appid_full_notm}} zur Authentifizierung bei Ihrer App verwenden.
{:shortdesc}

**Beschreibung**</br>
Authentifiziert Web- oder API-HTTP/HTTPS-Anforderungen mit {{site.data.keyword.appid_short_notm}}.

Wenn Sie den Anforderungstyp auf 'web' setzen, wird eine Webanforderung geprüft, die ein {{site.data.keyword.appid_short_notm}}-Zugriffstoken enthält. Wenn die Tokenprüfung fehlschlägt, wird die Webanforderung zurückgewiesen. Wenn die Anforderung kein Zugriffstoken enthält, wird die Anforderung an die {{site.data.keyword.appid_short_notm}}-Anmeldeseite umgeleitet. Damit die {{site.data.keyword.appid_short_notm}}-Webauthentifizierung funktioniert, müssen Cookies im Browser des Benutzers aktiviert sein.

Wenn Sie den Anforderungstyp auf 'api' setzen, wird eine API-Anforderung geprüft, die ein {{site.data.keyword.appid_short_notm}}-Zugriffstoken enthält. Wenn die Anforderung kein Zugriffstoken enthält, wird die Fehlernachricht 401: "Unauthorized" an den Benutzer zurückgegeben.

Aus Sicherheitsgründen unterstützt die {{site.data.keyword.appid_short_notm}}-Authentifizierung nur Back-Ends mit aktiviertem TLS/SSL.
{: note}

**YAML-Beispiel für eine Ingress-Ressource**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/appid-auth: "bindSecret=<bind_secret> namespace=<namespace> requestType=<request_type> serviceName=<myservice> [idToken=false]"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080
```
{: codeblock}

<table>
<caption>Erklärung der Komponenten von Annotationen</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der Komponenten von Annotationen</th>
</thead>
<tbody>
<tr>
<td><code>bindSecret</code></td>
<td>Ersetzen Sie <em><code>&lt;geheimer_bindungsschlüssel&gt;</code></em> durch den geheimen Kubernetes-Schlüssel, der den geheimen Bindungsschlüssel für Ihre {{site.data.keyword.appid_short_notm}}-Serviceinstanz speichert.</td>
</tr>
<tr>
<td><code>namespace</code></td>
<td>Ersetzen Sie <em><code>&lt;namensbereich&gt;</code></em> durch den Namensbereich des geheimen Bindungsschlüssels. In diesem Feld wird standardmäßig der `Standardname` für den Namensbereich verwendet.</td>
</tr>
<tr>
<td><code>requestType</code></td>
<td>Ersetzen Sie <code><em>&lt;anforderungstyp&gt;</em></code> durch den Anforderungstyp, den Sie an {{site.data.keyword.appid_short_notm}} senden wollen. Gültige Werte sind `web` oder `api`. Der Standardwert ist `api`.</td>
</tr>
<tr>
<td><code>serviceName</code></td>
<td>Ersetzen Sie <code><em>&lt;mein_service&gt;</em></code> durch den Namen des Kubernetes-Service, den Sie für Ihre App erstellt haben. Dieses Feld ist erforderlich. Wenn ein Servicename nicht enthalten ist, wird die Annotation für alle Services aktiviert.  Wenn ein Servicename enthalten ist, wird die Annotation nur für diesen Service aktiviert. Trennen Sie mehrere Services durch ein Komma (,).</td>
</tr>
<tr>
<td><code>idToken=false</code></td>
<td>Optional: Liberty-OIDC-Client kann den Zugriff und das Identitätstoken nicht zeitgleich parsen. Wenn Sie mit Liberty arbeiten, setzen Sie diesen Wert auf 'false', sodass das Identitätstoken nicht an den Liberty-Server gesendet wird.</td>
</tr>
</tbody></table>

**Syntax**</br>

Da die App {{site.data.keyword.appid_short_notm}} für die Authentifizierung verwendet, müssen Sie eine {{site.data.keyword.appid_short_notm}}-Instanz bereitstellen, die Instanz mit gültigen Umleitungs-URIs konfigurieren und einen geheimen Bindungsschlüssel generieren, indem Sie die Instanz an den Cluster binden.

1. Wählen Sie eine vorhandene Instanz aus oder erstellen Sie eine neue {{site.data.keyword.appid_short_notm}}-Instanz.
  * Um eine vorhandene Instanz zu verwenden, stellen Sie sicher, dass der Name der Serviceinstanz keine Leerzeichen enthält. Um Leerzeichen zu entfernen, wählen Sie das Menü für mehr Optionen neben dem Namen der Serviceinstanz und anschließend **Service umbenennen** aus.
  * Gehen Sie wie folgt vor, um eine [neue {{site.data.keyword.appid_short_notm}}-Instanz](https://cloud.ibm.com/catalog/services/app-id) bereitzustellen:
      1. Ersetzen Sie den im Feld **Servicename** automatisch eingetragenen Wert durch Ihren eigenen eindeutigen Namen für die Serviceinstanz. Der Name der Serviceinstanz darf keine Leerzeichen enthalten.
      2. Wählen Sie die Region aus, in der Ihr Cluster implementiert ist.
      3. Klicken Sie auf **Erstellen**.

2. Fügen Sie Umleitungs-URLs für Ihre App hinzu. Eine Umleitungs-URL ist der Callback-Endpunkt Ihrer App. Um Phishing-Attacken zu verhindern, validiert die App-ID die Anforderungs-URL gegen die Whitelist von Umleitungs-URLs.
  1. Navigieren Sie in der {{site.data.keyword.appid_short_notm}}-Managementkonsole zu **Identitätsprovider > Verwalten**.
  2. Stellen Sie sicher, dass Sie einen Identitätsprovider ausgewählt haben. Wenn kein Identitätsprovider ausgewählt ist, wird der Benutzer nicht authentifiziert, sondern es wird ein Zugriffstoken für den anonymen Zugriff auf die App ausgegeben.
  3. Fügen Sie im Feld **Web-Weiterleitungs-URLs** Weiterleitungs-URLs für Ihre App im Format `http://<hostname>/<app_path>/appid_callback` oder `https://<hostname>/<app_path>/appid_callback` hinzu.

    {{site.data.keyword.appid_full_notm}} stellt eine Abmeldefunktion bereit: Wenn `/logout` in Ihrem {{site.data.keyword.appid_full_notm}}-Pfad vorhanden ist, werden Cookies entfernt und der Benutzer wird an die Anmeldeseite zurückverwiesen. Zur Verwendung dieser Funktion müssen Sie `/appid_logout` an Ihre Domäne im Format `https://<hostname>/<app_path>/appid_logout` anhängen und diese URL in der Liste der Umleitungs-URLs verwenden.
    {: note}

3. Binden Sie die {{site.data.keyword.appid_short_notm}}-Serviceinstanz an den Cluster. Der Befehl erstellt einen Serviceschlüssel für die Serviceinstanz. Alternativ können Sie das Flag `--key` mit angeben, um vorhandene Berechtigungsnachweise für den Serviceschlüssel zu verwenden.
  ```
  ibmcloud ks cluster-service-bind --cluster <clustername_oder_ID> --namespace <namensbereich> --service <serviceinstanzname> [--key <serviceinstanzschlüssel>]
  ```
  {: pre}
  Wenn der Service Ihrem Cluster erfolgreich hinzugefügt wurde, wird ein geheimer Schlüssel für den Cluster erstellt, der die Berechtigungsnachweise Ihrer Serviceinstanz enthält. CLI-Beispielausgabe:
  ```
  ibmcloud ks cluster-service-bind --cluster mycluster --namespace mynamespace --service appid1
    Binding service instance to namespace...
    OK
    Namespace:    mynamespace
    Secret name:  binding-<serviceinstanzname>
  ```
  {: screen}

4. Rufen Sie den geheimer Schlüssel ab, der im Namensbereich Ihres Clusters erstellt wurde.
  ```
  kubectl get secrets --namespace=<namensbereich>
  ```
  {: pre}

5. Verwenden Sie den geheimen Bindungsschlüssel und den Namensbereich des Clusters, um die Annotation `appid-auth` zu Ihrer Ingress-Ressource hinzuzufügen.
