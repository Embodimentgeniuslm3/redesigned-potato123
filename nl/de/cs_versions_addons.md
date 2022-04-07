---

copyright:
  years: 2014, 2019
lastupdated: "2019-04-16"

keywords: kubernetes, iks, nginx, ingress controller

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



# Änderungsprotokoll für Cluster-Add-ons

Ihr {{site.data.keyword.containerlong}}-Cluster wird mit Add-ons ausgeliefert, die automatisch von IBM aktualisiert werden. Sie können automatische Aktualisierungen für manche Add-ons auch inaktivieren und sie separat über die Master- und Worker-Knoten aktualisieren. In den Tabellen in den folgenden Abschnitten finden Sie eine Zusammenfassung der Änderungen für jede Version.
{: shortdesc}

## Änderungsprotokoll für Ingress-ALB-Add-on
{: #alb_changelog}

Zeigen Sie Änderungen der Buildversion für das Add-on der Ingress-Lastausgleichsfunktion für Anwendungen (ALB) in Ihren {{site.data.keyword.containerlong_notm}}-Clustern an.
{:shortdesc}

Wenn das Ingress-ALB-Add-on aktualisiert wird, werden die Container `nginx-ingress` und `ingress-auth` in allen ALB-Pods auf die aktuelle Buildversion aktualisiert. Standardmäßig sind automatische Aktualisierungen für das Add-on aktiviert, aber Sie können die automatischen Aktualisierungen inaktivieren und das Add-on manuell aktualisieren. Weitere Informationen finden Sie unter [Ingress-Lastausgleichsfunktion für Anwendungen aktualisieren](/docs/containers?topic=containers-update#alb).

In der folgenden Tabelle finden Sie eine Zusammenfassung der Änderungen für jeden Build des Ingress-ALB-Add-ons.

<table summary="Übersicht über Buildänderungen für das Add-on der Ingress-Lastausgleichsfunktion für Anwendungen">
<caption>Änderungsprotokoll für das Add-on der Ingress-Lastausgleichsfunktion für Anwendungen</caption>
<col width="12%">
<col width="12%">
<col width="41%">
<col width="35%">
<thead>
<tr>
<th>Build `nginx-ingress`/`ingress-auth`</th>
<th>Freigabedatum</th>
<th>Unterbrechungsfreie Änderungen</th>
<th>Änderungen mit Unterbrechung</th>
</tr>
</thead>
<tbody>
<tr>
<td>411 / 315</td>
<td>15. April 2019</td>
<td>Aktualisiert den Wert für den Ablauf des {{site.data.keyword.appid_full}}-Cookies entsprechend dem Wert für den Ablauf des Zugriffstokens.</td>
<td>-</td>
</tr>
<tr>
<td>411 / 306</td>
<td>22. März 2019</td>
<td>Aktualisiert Go auf Version 1.12.1.</td>
<td>-</td>
</tr>
<tr>
<td>410 / 305</td>
<td>18. März 2019</td>
<td><ul>
<li>Behebt Schwachstellen für Image-Scans.</li>
<li>Verbessert die Protokollierung für {{site.data.keyword.appid_full_notm}}.</li>
</ul></td>
<td>-</td>
</tr>
<tr>
<td>408 / 304</td>
<td>5. März 2019</td>
<td>-</td>
<td>Behebt Fehler in der Autorisierungsintegration in Bezug auf Abmeldefunktionalität, Tokenablauf und Autorisierungscallbacks `OAuth`. Diese Korrekturen werden nur implementiert, wenn Sie die {{site.data.keyword.appid_full_notm}}-Autorisierung durch die Annotation [`appid-auth`](/docs/containers?topic=containers-ingress_annotation#appid-auth) aktiviert haben. Zur Implementierung dieser Korrekturen werden zusätzliche Header hinzugefügt, die die Gesamtgröße der Header erhöhen. Abhängig von der Größe der eigenen Header und der Gesamtgröße von Antworten müssen Sie möglicherweise von Ihnen verwendete [Annotationen für Proxypuffer](/docs/containers?topic=containers-ingress_annotation#proxy-buffer) anpassen.</td>
</tr>
<tr>
<td>406 / 301</td>
<td>19. Februar 2019</td>
<td><ul>
<li>Aktualisiert Go auf Version 1.11.5.</li>
<li>Behebt Schwachstellen für Image-Scans.</li>
</ul></td>
<td>-</td>
</tr>
<tr>
<td>404 / 300</td>
<td>31. Januar 2019</td>
<td>Aktualisiert Go auf Version 1.11.4.</td>
<td>-</td>
</tr>
<tr>
<td>403 / 295</td>
<td>23. Januar 2019</td>
<td><ul>
<li>Aktualisiert die NGINX-Version von ALBs auf 1.15.2.</li>
<li>Von IBM bereitgestellte TLS-Zertifikate werden jetzt automatisch 37 Tage, und nicht 7 Tage, vor Ablauf verlängert.</li>
<li>Fügt {{site.data.keyword.appid_full_notm}}-Abmeldefunktionalität hinzu: Wenn das Präfix `/logout` in einem {{site.data.keyword.appid_full_notm}}-Pfad vorhanden ist, werden Cookies entfernt und der Benutzer wird an die Anmeldeseite zurückverwiesen.</li>
<li>Fügt einen Header zu {{site.data.keyword.appid_full_notm}}-Anforderungen zu internen Verfolgungszwecken hinzu.</li>
<li>Aktualisiert die {{site.data.keyword.appid_short_notm}}-Anweisung 'location', sodass die Annotation `app-id` in Verbindung mit den Annotationen `proxy-buffers`, `proxy-buffer-size` und `proxy-busy-buffer-size` verwendet werden kann.</li>
<li>Behebt einen Programmfehler, sodass Informationsprotokolle nicht mit der Bezeichnung für Fehler ('error') versehen werden.</li>
</ul></td>
<td>Inaktiviert TLS 1.0 und 1.1 standardmäßig. Wenn die Clients, die eine Verbindung zu Ihren Apps herstellen, TLS 1.2 unterstützen, ist keine Aktion erforderlich. Wenn Sie noch ältere Clients haben, die eine Unterstützung von TLS 1.0 oder 1.1 erfordern, müssen Sie die erforderlichen TLS-Versionen manuell aktivieren, indem Sie [diese Schritte ausführen](/docs/containers?topic=containers-ingress#ssl_protocols_ciphers). Weitere Informationen zum Anzeigen der TLS-Versionen, die Ihre Clients für den Zugriff auf Ihre Apps verwenden, finden Sie in diesem [{{site.data.keyword.Bluemix_notm}}-Blogeintrag](https://www.ibm.com/blogs/bluemix/2018/11/ibm-cloud-kubernetes-service-alb-update-tls-1-0-and-1-1-disabled-by-default/).</td>
</tr>
<tr>
<td>393 / 291</td>
<td>9. Januar 2019</td>
<td>Fügt Unterstützung für mehrere {{site.data.keyword.appid_full_notm}}-Instanzen hinzu.</td>
<td>-</td>
</tr>
<tr>
<td>393/282</td>
<td>29. November 2018</td>
<td>Verbessert die Leistung für {{site.data.keyword.appid_full_notm}}.</td>
<td>-</td>
</tr>
<tr>
<td>384/246</td>
<td>14. November 2018</td>
<td>Verbessert die Protokollierungs- und Abmeldefeatures für {{site.data.keyword.appid_full_notm}}.</td>
<td>Ersetzt das selbst signierte Zertifikat für `*.containers.mybluemix.net` durch das signierte Zertifikat von LetsEncrypt, das automatisch für den Cluster generiert und von diesem verwendet wird. Das selbst signierte Zertifikat `*.containers.mybluemix.net` wird entfernt.</td>
</tr>
<tr>
<td>350/192</td>
<td>5. November 2018</td>
<td>Fügt Unterstützung für die Aktivierung und Inaktivierung der automatischen Aktualisierungen des Ingress-ALB-Add-ons hinzu.</td>
<td>-</td>
</tr>
</tbody>
</table>
