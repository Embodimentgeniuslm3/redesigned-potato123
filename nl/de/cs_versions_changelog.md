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



# Änderungsprotokoll der Version
{: #changelog}

Zeigen Sie Informationen zu Versionsänderungen für Hauptversions-, Nebenversions- und Patchaktualisierungen an, die für Ihre {{site.data.keyword.containerlong}}-Kubernetes-Cluster verfügbar sind. Diese Änderungen umfassen Updates für Kubernetes und {{site.data.keyword.Bluemix_notm}} Provider-Komponenten.
{:shortdesc}

Weitere Informationen zu Hauptversionen, Nebenversionen und Patchversionen sowie Vorbereitungsaktionen zwischen Nebenversionen finden Sie unter [Kubernetes-Versionen](/docs/containers?topic=containers-cs_versions).
{: tip}

Weitere Informationen zu Änderungen seit der vorherigen Version finden Sie in den Änderungsprotokollen.
-  Version 1.13, [Änderungsprotokoll](#113_changelog).
-  Version 1.12, [Änderungsprotokoll](#112_changelog).
-  Version 1.11, [Änderungsprotokoll](#111_changelog).
-  **Veraltet:** Version 1.10, [Änderungsprotokoll](#110_changelog).
-  [Archiv](#changelog_archive) der Änderungsprotokolle nicht unterstützter Versionen.

Manche Änderungsprotokolle sind für _Fixpacks für Workerknoten_ konzipiert und können nur auf Workerknoten angewendet werden. Sie müssen [diese Patches anwenden](/docs/containers?topic=containers-cs_cli_reference#cs_worker_update), um die Einhaltung der Sicherheitsbestimmungen für die Workerknoten sicherzustellen. Diese Fixpacks für Worker-Knoten können eine höhere Version als der Master haben, da einige Build-Fixpacks für die Worker-Knoten spezifisch sind. Manche Änderungsprotokolle sind für _Master-Fixpacks_ konzipiert und können nur auf den Cluster-Master angewendet werden. Es kann vorkommen, dass Master-Fixpacks nicht automatisch angewendet werden. Sie können [sie manuell anwenden](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_update). Weitere Informationen zu Patchtypen finden Sie unter [Aktualisierungstypen](/docs/containers?topic=containers-cs_versions#update_types).
{: note}

</br>

## Version 1.13, Änderungsprotokoll
{: #113_changelog}

### Änderungsprotokoll für Workerknoten-Fixpack 1.13.5_1518, veröffentlicht am 15. April 2019
{: #1135_1518}

In der folgenden Tabelle finden Sie die Änderungen, die im Workerknoten-Fixpack 1.13.5_1518 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.13.5_1517 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.13.5_1517</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu-Pakete</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Es wurden Aktualisierungen an installierten Ubuntu-Paketen, darunter `systemd` für [CVE-2019-3842 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-3842.html), vorgenommen.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für 1.13.5_1517, veröffentlicht am 8. April 2019
{: #1135_1517}

In der folgenden Tabelle finden Sie die Änderungen, die in Patch 1.13.5_1517 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.13.4_1516 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.13.4_1516</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>Version 3.4.0</td>
<td>Version 3.4.4</td>
<td>Lesen Sie die [Calico-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.projectcalico.org/v3.4/releases/#v344). Die Aktualisierung behebt [CVE-2019-9946 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9946).</td>
</tr>
<tr>
<td>Hochverfügbarkeitsproxy des Cluster-Masters</td>
<td>1.8.12-alpine</td>
<td>1.9.6-alpine</td>
<td>Lesen Sie die [HAProxy-Releaseinformationen![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.haproxy.org/download/1.9/src/CHANGELOG). Die Aktualisierung behebt [CVE-2018-0732 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732), [CVE-2018-0734 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734), [CVE-2018-0737 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737), [CVE-2018-5407 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407), [CVE-2019-1543 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543) und [CVE-2019-1559 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.13.4-86</td>
<td>v1.13.5-107</td>
<td>Es wurde eine Aktualisierung zur Unterstützung von Kubernetes 1.13.5 und Calico 3.4.4 vorgenommen.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.13.4</td>
<td>v1.13.5</td>
<td>Lesen Sie die [Kubernetes-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/kubernetes/releases/tag/v1.13.5).</td>
</tr>
<tr>
<td>Trusted Compute-Agent</td>
<td>a02f765</td>
<td>e132aa4</td>
<td>Das Image für [CVE-2017-12447 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-12447) wurde aktualisiert.</td>
</tr>
<tr>
<td>Ubuntu 16.04 Kernel</td>
<td>4s.4.0-143 generisch</td>
<td>4.4.0-145 generisch</td>
<td>Es wurden Workerknoten-Images mit Kernelaktualisierung für [CVE-2019-9213 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html) aktualisiert.</td>
</tr>
<tr>
<td>Ubuntu 18.04 Kernel</td>
<td>4.15.0-46 generisch</td>
<td>4.15.0-47 generisch</td>
<td>Es wurden Workerknoten-Images mit Kernelaktualisierung für [CVE-2019-9213 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html) wurden aktualisiert.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten-Fixpack 1.13.4_1516, veröffentlicht am 1. April 2019
{: #1134_1516}

In der folgenden Tabelle finden Sie die Änderungen, die im Workerknoten-Fixpack 1.13.4_1516 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.13.4_1515 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.13.4_1515</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ressourcenauslastung von Workerknoten</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Es wurden die Speicherreservierungen für 'kubelet' und 'containerd' erhöht, damit diesen Komponenten nicht die Ressourcen ausgehen. Weitere Informationen finden Sie unter [Reserven für Workerknotenressourcen](/docs/containers?topic=containers-plan_clusters#resource_limit_node).</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Master-Fixpack 1.13.4_1515, veröffentlicht am 26. März 2019
{: #1134_1515}

In der folgenden Tabelle finden Sie die Änderungen, die im Master-Fixpack 1.13.4_1515 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.13.4_1513 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.13.4_1513</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Cluster-DNS-Konfiguration</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Es wurden Fehler beim Aktualisierungsprozess von Kubernetes Version 1.11 behoben, um zu verhindern, dass bei der Aktualisierung der Cluster-DNS-Provider zu CoreDNS gewechselt wird. Sie können auch nach der Aktualisierung noch [CoreDNS als Cluster-DNS-Provider einrichten](/docs/containers?topic=containers-cluster_dns#set_coredns).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage-Plug-in</td>
<td>345</td>
<td>346</td>
<td>Das Image für [CVE-2019-9741 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741) wurde aktualisiert.</td>
</tr>
<tr>
<td>Key Management Service-Provider</td>
<td>166</td>
<td>167</td>
<td>Korrigiert die sporadisch auftretenden Fehler `context deadline exceeded` und `timeout` bei der Verwaltung von geheimen Kubernetes-Schlüsseln. Zusätzlich werden Aktualisierungen am Schlüsselmanagementservice (KMS), bei denen vorhandene geheime Kubernetes-Schlüssel unverschlüsselt bleiben könnten, korrigiert. Die Aktualisierung enthält einen Fix für [CVE-2019-9741 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
<tr>
<td>Lastausgleichsfunktion und Monitor für die Lastausgleichsfunktion für {{site.data.keyword.Bluemix_notm}} Provider</td>
<td>143</td>
<td>146</td>
<td>Das Image für [CVE-2019-9741 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741) wurde aktualisiert.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für 1.13.4_1513, veröffentlicht am 20. März 2019
{: #1134_1513}

In der folgenden Tabelle finden Sie die Änderungen, die in Patch 1.13.4_1513 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.13.4_1510 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.13.4_1510</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Cluster-DNS-Konfiguration</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Es wurde ein Fehler behoben, der dazu führen kann, dass Operationen des Cluster-Masters (z. B. `refresh` oder `update`) fehlschlagen, wenn ein Scale-down für das nicht genutzte Cluster-DNS durchgeführt wird.</td>
</tr>
<tr>
<td>Konfiguration des Hochverfügbarkeitsproxys des Cluster-Masters</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Die Konfiguration wurde aktualisiert, um sporadisch auftretende Fehler der Verbindung zum Cluster-Master besser aufzufangen.</td>
</tr>
<tr>
<td>CoreDNS-Konfiguration</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Die CoreDNS-Konfiguration wurde aktualisiert, um [mehrere Corefiles ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://coredns.io/2017/07/23/corefile-explained/) nach der Aktualisierung der Kubernetes-Version von 1.12 des Clusters zu unterstützen.</td>
</tr>
<tr>
<td>containerd</td>
<td>1.2.4</td>
<td>1.2.5</td>
<td>Lesen Sie die [containerd-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/containerd/containerd/releases/tag/v1.2.5). Die Aktualisierung enthält einen verbesserten Fix für [CVE-2019-5736 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5736).</td>
</tr>
<tr>
<td>GPU-Einheiten-Plug-in und Installationsprogramm</td>
<td>e32d51c</td>
<td>9ff3fda</td>
<td>GPU-Treiber wurden auf [418.43 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.nvidia.com/object/unix.html) aktualisiert. Die Aktualisierung enthält einen Fix für [CVE-2019-9741 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9741.html).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage-Plug-in</td>
<td>344</td>
<td>345</td>
<td>Es wurde Unterstützung für [private Serviceendpunkte](/docs/containers?topic=containers-cs_network_cluster#set-up-private-se) hinzugefügt.</td>
</tr>
<tr>
<td>Kernel</td>
<td>4.4.0-141</td>
<td>4.4.0-143</td>
<td>Workerknoten-Images mit Kernelaktualisierung für [CVE-2019-6133 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-6133.html) wurden aktualisiert.</td>
</tr>
<tr>
<td>Key Management Service-Provider</td>
<td>136</td>
<td>166</td>
<td>Das Image für [CVE-2018-16890 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16890), [CVE-2019-3822 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3822) und [CVE-2019-3823 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3823) wurde aktualisiert.</td>
</tr>
<tr>
<td>Trusted Compute-Agent</td>
<td>5f3d092</td>
<td>a02f765</td>
<td>Das Image für [CVE-2018-10779 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10779), [CVE-2018-12900 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-12900), [CVE-2018-17000 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-17000), [CVE-2018-19210 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-19210), [CVE-2019-6128 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6128) und [CVE-2019-7663 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-7663) wurde aktualisiert.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für 1.13.4_1510, veröffentlicht am 4. März 2019
{: #1134_1510}

In der folgenden Tabelle finden Sie die Änderungen, die in Patch 1.13.4_1510 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.13.2_1509 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.13.2_1509</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Cluster-DNS-Provider</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Die Speicherbegrenzung für Kubernetes-DNS- und CoreDNS-Pod-Speicher wurde von `170Mi` auf `400Mi` erhöht, um mehr Cluster-Services verarbeiten zu können.</td>
</tr>
<tr>
<td>GPU-Einheiten-Plug-in und Installationsprogramm</td>
<td>eb3a259</td>
<td>e32d51c</td>
<td>Die Images für [CVE-2019-6454 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454) wurden aktualisiert.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.13.2-62</td>
<td>v1.13.4-86</td>
<td>Wurde für die Unterstützung von Kubernetes 1.13.4 aktualisiert. Behebt regelmäßig auftretende Konnektivitätsprobleme für Lastausgleichsfunktionen der Version 1.0, die `externalTrafficPolicy` auf den Wert `local` setzen. Ereignisse der Lastausgleichsfunktion von Version 1.0 und 2.0 wurden zur Verwendung der neuesten {{site.data.keyword.Bluemix_notm}}-Dokumentationslinks aktualisiert.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage-Plug-in</td>
<td>342</td>
<td>344</td>
<td>Basisbetriebssystem für das Image von Fedora in Alpine geändert. Das Image für [CVE-2019-6486 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) wurde aktualisiert.</td>
</tr>
<tr>
<td>Key Management Service-Provider</td>
<td>122</td>
<td>136</td>
<td>Das Clientzeitlimit für {{site.data.keyword.keymanagementservicefull_notm}} wurde erhöht. Das Image für [CVE-2019-6486 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) wurde aktualisiert.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.13.2</td>
<td>v1.13.4</td>
<td>Lesen Sie die [Kubernetes-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/kubernetes/releases/tag/v1.13.4). Die Aktualisierung löst [CVE-2019-6486 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) und [CVE-2019-1002100 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1002100).</td>
</tr>
<tr>
<td>Kubernetes-Konfiguration</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Die Einstellung `ExperimentalCriticalPodAnnotation=true` wurde der Option `--feature-gates` hinzugefügt. Diese Einstellung hilft bei der Migration von Pods von der veralteten Annotation `scheduler.alpha.kubernetes.io/critical-pod` auf die [Unterstützung für die Kubernetes-Podpriorität](/docs/containers?topic=containers-pod_priority#pod_priority).</td>
</tr>
<tr>
<td>Lastausgleichsfunktion und Monitor für die Lastausgleichsfunktion für {{site.data.keyword.Bluemix_notm}} Provider</td>
<td>132</td>
<td>143</td>
<td>Das Image für [CVE-2019-6486 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) wurde aktualisiert.</td>
</tr>
<tr>
<td>OpenVPN-Client und -Server</td>
<td>2.4.6-r3-IKS-13</td>
<td>2.4.6-r3-IKS-25</td>
<td>Das Image für [CVE-2019-1559 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559) wurde aktualisiert.</td>
</tr>
<tr>
<td>Trusted Compute-Agent</td>
<td>1ea5ad3</td>
<td>5f3d092</td>
<td>Das Image für [CVE-2019-6454 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454) wurde aktualisiert.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten-Fixpack 1.13.2_1509, veröffentlicht am 27. Februar 2019
{: #1132_1509}

In der folgenden Tabelle finden Sie die Änderungen, die im Workerknoten-Fixpack 1.13.2_1509 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.13.2_1508 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.13.2_1508</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-141</td>
<td>4.4.0-142</td>
<td>Workerknoten-Images mit Kernelaktualisierung für [CVE-2018-19407 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-142.168/changelog) wurden aktualisiert.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten-Fixpack 1.13.2_1508, veröffentlicht am 15. Februar 2019
{: #1132_1508}

In der folgenden Tabelle finden Sie die Änderungen, die im Workerknoten-Fixpack 1.13.2_1508 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.13.2_1507 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.13.2_1507</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Konfiguration des Hochverfügbarkeitsproxys des Cluster-Masters</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Die Podkonfigurationeinstellung `spec.priorityClassName` wurde in den Wert `system-node-critical` geändert und `spec.priority` wurde auf den Wert `2000001000` gesetzt.</td>
</tr>
<tr>
<td>containerd</td>
<td>1.2.2</td>
<td>1.2.4</td>
<td>Lesen Sie die [containerd-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/containerd/containerd/releases/tag/v1.2.4). Die Aktualisierung behebt [CVE-2019-5736 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5736).</td>
</tr>
<tr>
<td>Konfiguration von Kubernetes `kubelet`</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Das Feature-Gate `ExperimentalCriticalPodAnnotation` wurde aktiviert, um das Entfernen kritischer statischer Pods zu vermeiden. Die Option `event-qps` wurde auf `0` gesetzt, um das Erstellen von Ereignissen für Ratenbegrenzung zu vermeiden.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für 1.13.2_1507, veröffentlicht am 5. Februar 2019
{: #1132_1507}

In der folgenden Tabelle finden Sie die Änderungen, die in Patch 1.13.2_1507 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.12.4_1535 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.12.4_1535</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>Version 3.3.1</td>
<td>Version 3.4.0</td>
<td>Lesen Sie die [Calico-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.projectcalico.org/v3.4/releases/#v340).</td>
</tr>
<tr>
<td>Cluster-DNS-Provider</td>
<td>n.z.</td>
<td>n.z.</td>
<td>CoreDNS ist jetzt der Standard-Cluster-DNS-Provider für neue Cluster. Wenn Sie einen vorhandenen Cluster, der KubeDNS als Cluster-DNS-Provider verwendet, auf 1.13 aktualisieren, wird KubeDNS weiterhin als Cluster-DNS-Provider verwendet. Sie können stattdessen jedoch auch [CoreDNS verwenden](/docs/containers?topic=containers-cluster_dns#dns_set).</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.5</td>
<td>1.2.2</td>
<td>Lesen Sie die [containerd-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/containerd/containerd/releases/tag/v1.2.2).</td>
</tr>
<tr>
<td>CoreDNS</td>
<td>1.2.2</td>
<td>1.2.6</td>
<td>Lesen Sie die [CoreDNS-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/coredns/coredns/releases/tag/v1.2.6). Die CoreDNS-Konfiguration wurde außerdem aktualisiert, um [mehrere Corefiles zu unterstützen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://coredns.io/2017/07/23/corefile-explained/).</td>
</tr>
<tr>
<td>etcd</td>
<td>Version 3.3.1</td>
<td>Version 3.3.11</td>
<td>Lesen Sie die [etcd-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/coreos/etcd/releases/v3.3.11). Weiterhin wurden die unterstützten Cipher-Suites für 'etcd' jetzt auf eine Untergruppe mit starker Verschlüsselung (128 Bit oder mehr) eingeschränkt.</td>
</tr>
<tr>
<td>GPU-Einheiten-Plug-in und Installationsprogramm</td>
<td>13fdc0d</td>
<td>eb3a259</td>
<td>Die Images für [CVE-2019-3462 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) und [CVE-2019-6486 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) wurden aktualisiert.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.12.4-118</td>
<td>v1.13.2-62</td>
<td>Wurde für die Unterstützung von Kubernetes 1.13.2 aktualisiert. Außerdem wurde `calicoctl` auf Version 3.4.0 aktualisiert. Unnötige Konfigurationsaktualisierungen auf Lastausgleichsfunktionen der Version 2.0 bei Änderungen des Workerknotenstatus behoben.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage-Plug-in</td>
<td>338</td>
<td>342</td>
<td>Das Dateispeicher-Plug-in wurde wie folgt aktualisiert:
<ul><li>Unterstützt dynamisches Bereitstellen mit [für die Datenträgertopologie sensitiver Planung](/docs/containers?topic=containers-file_storage#file-topology).</li>
<li>Ignoriert Fehler beim Löschen von PersistentVolumeClaims (PVC), wenn der Speicher bereits gelöscht ist.</li>
<li>Fügt fehlgeschlagenen PVCs eine Fehlernachrichtenannotation hinzu.</li>
<li>Optimiert die Einstellungen für die Auswahl des leitenden Service (Leader) und für den Resynchronisationszeitraum der Speicherbereitstellungscontrollers und erhöht das Bereitstellungszeitlimit von 30 Minuten auf 1 Stunde.</li>
<li>Prüft Benutzerberechtigungen vor dem Start der Bereitstellung.</li>
<li>Fügt den Bereitstellungen `ibm-file-plugin` und `ibm-storage-watcher` im Namensbereich `kube-system` eine Tolerierung `CriticalAddonsOnly` hinzu.</li></ul></td>
</tr>
<tr>
<td>Key Management Service-Provider</td>
<td>111</td>
<td>122</td>
<td>Es wurde Wiederholungslogik hinzugefügt, um vorübergehende Ausfälle zu vermeiden, wenn geheime Kubernetes-Schlüssel von {{site.data.keyword.keymanagementservicefull_notm}} verwaltet werden.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.4</td>
<td>v1.13.2</td>
<td>Lesen Sie die [Kubernetes-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/kubernetes/releases/tag/v1.13.2).</td>
</tr>
<tr>
<td>Kubernetes-Konfiguration</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Die Konfiguration der Auditrichtlinie des Kubernetes-API-Servers wurde aktualisiert, um Protokollierungsmetadaten für `cluster-admin`-Anforderungen und eine Protokollierung des Anforderungshauptteils von Workloadanforderungen `create`, `update` und `patch` einzuschließen.</td>
</tr>
<tr>
<td>Automatische Kubernetes DNS-Skalierung</td>
<td>1.2.0</td>
<td>1.3.0</td>
<td>Lesen Sie die [Releaseinformationen zur automatischen Kubernetes-DNS-Skalierung ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes-incubator/cluster-proportional-autoscaler/releases/tag/1.3.0).</td>
</tr>
<tr>
<td>OpenVPN-Client</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>Das Image für [CVE-2018-0734 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) und [CVE-2018-5407 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407) wurde aktualisiert. Der Bereitstellung `vpn` im Namensbereich `kube-system` wurde eine Tolerierung `CriticalAddonsOnly` hinzugefügt. Darüber hinaus wird die Podkonfiguration jetzt aus einem geheimen Schlüssel und nicht aus einer Konfigurationszuordnung (Configmap) abgerufen.</td>
</tr>
<tr>
<td>OpenVPN-Server</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>Das Image für [CVE-2018-0734 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) und [CVE-2018-5407 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407) wurde aktualisiert.</td>
</tr>
<tr>
<td>systemd</td>
<td>230</td>
<td>229</td>
<td>Sicherheitspatch für [CVE-2018-16864 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16864).</td>
</tr>
</tbody>
</table>

<br />


## Version 1.12, Änderungsprotokoll
{: #112_changelog}

Überprüfen Sie das Änderungsprotokoll der Version 1.12.
{: shortdesc}

### Änderungsprotokoll für Workerknoten-Fixpack 1.12.7_1549, veröffentlicht am 15. April 2019
{: #1127_1549}

In der folgenden Tabelle finden Sie die Änderungen, die im Workerknoten-Fixpack 1.12.7_1549 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.12.7_1548 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.12.7_1548</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu-Pakete</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Es wurden Aktualisierungen an installierten Ubuntu-Paketen, darunter `systemd` für [CVE-2019-3842 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-3842.html), vorgenommen.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für 1.12.7_1548, veröffentlicht am 8. April 2019
{: #1127_1548}

In der folgenden Tabelle finden Sie die Änderungen, die in Patch 1.12.7_1548 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.12.6_1547 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.12.6_1547</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>Version 3.3.1</td>
<td>Version 3.3.6</td>
<td>Lesen Sie die [Calico-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.projectcalico.org/v3.3/releases/#v336). Die Aktualisierung behebt [CVE-2019-9946 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9946).</td>
</tr>
<tr>
<td>Hochverfügbarkeitsproxy des Cluster-Masters</td>
<td>1.8.12-alpine</td>
<td>1.9.6-alpine</td>
<td>Lesen Sie die [HAProxy-Releaseinformationen![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.haproxy.org/download/1.9/src/CHANGELOG). Die Aktualisierung behebt [CVE-2018-0732 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732), [CVE-2018-0734 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734), [CVE-2018-0737 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737), [CVE-2018-5407 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407), [CVE-2019-1543 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543) und [CVE-2019-1559 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.12.6-157</td>
<td>Version 1.12.7-180</td>
<td>Es wurde eine Aktualisierung zur Unterstützung von Kubernetes 1.12.7 und Calico 3.3.6 vorgenommen.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.6</td>
<td>Version 1.12.7</td>
<td>Lesen Sie die [Kubernetes-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.7).</td>
</tr>
<tr>
<td>Trusted Compute-Agent</td>
<td>a02f765</td>
<td>e132aa4</td>
<td>Das Image für [CVE-2017-12447 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-12447) wurde aktualisiert.</td>
</tr>
<tr>
<td>Ubuntu 16.04 Kernel</td>
<td>4s.4.0-143 generisch</td>
<td>4.4.0-145 generisch</td>
<td>Es wurden Workerknoten-Images mit Kernelaktualisierung für [CVE-2019-9213 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html) aktualisiert.</td>
</tr>
<tr>
<td>Ubuntu 18.04 Kernel</td>
<td>4.15.0-46 generisch</td>
<td>4.15.0-47 generisch</td>
<td>Es wurden Workerknoten-Images mit Kernelaktualisierung für [CVE-2019-9213 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html) aktualisiert.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten-Fixpack 1.12.6_1547, veröffentlicht am 1. April 2019
{: #1126_1547}

In der folgenden Tabelle finden Sie die Änderungen, die im Workerknoten-Fixpack 1.12.6_1547 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.12.6_1546 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.12.6_1546</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ressourcenauslastung von Workerknoten</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Es wurden die Speicherreservierungen für 'kubelet' und 'containerd' erhöht, damit diesen Komponenten nicht die Ressourcen ausgehen. Weitere Informationen finden Sie unter [Reserven für Workerknotenressourcen](/docs/containers?topic=containers-plan_clusters#resource_limit_node).</td>
</tr>
</tbody>
</table>


### Änderungsprotokoll für Master-Fixpack 1.12.6_1546, veröffentlicht am 26. März 2019
{: #1126_1546}

In der folgenden Tabelle finden Sie die Änderungen, die im Master-Fixpack 1.12.6_1546 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.12.6_1544 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.12.6_1544</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage-Plug-in</td>
<td>345</td>
<td>346</td>
<td>Das Image für [CVE-2019-9741 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741) wurde aktualisiert.</td>
</tr>
<tr>
<td>Key Management Service-Provider</td>
<td>166</td>
<td>167</td>
<td>Korrigiert die sporadisch auftretenden Fehler `context deadline exceeded` und `timeout` bei der Verwaltung von geheimen Kubernetes-Schlüsseln. Zusätzlich werden Aktualisierungen am Schlüsselmanagementservice (KMS), bei denen vorhandene geheime Kubernetes-Schlüssel unverschlüsselt bleiben könnten, korrigiert. Die Aktualisierung enthält einen Fix für [CVE-2019-9741 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
<tr>
<td>Lastausgleichsfunktion und Monitor für die Lastausgleichsfunktion für {{site.data.keyword.Bluemix_notm}} Provider</td>
<td>143</td>
<td>146</td>
<td>Das Image für [CVE-2019-9741 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741) wurde aktualisiert.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für 1.12.6_1544, veröffentlicht am 20. März 2019
{: #1126_1544}

In der folgenden Tabelle finden Sie die Änderungen, die in Patch 1.12.6_1544 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.12.6_1541 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.12.6_1541</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Cluster-DNS-Konfiguration</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Es wurde ein Fehler behoben, der dazu führen kann, dass Operationen des Cluster-Masters (z. B. `refresh` oder `update`) fehlschlagen, wenn ein Scale-down für das nicht genutzte Cluster-DNS durchgeführt wird.</td>
</tr>
<tr>
<td>Konfiguration des Hochverfügbarkeitsproxys des Cluster-Masters</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Die Konfiguration wurde aktualisiert, um sporadisch auftretende Fehler der Verbindung zum Cluster-Master besser aufzufangen.</td>
</tr>
<tr>
<td>CoreDNS-Konfiguration</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Die CoreDNS-Konfiguration wurde aktualisiert, um [mehrere Corefiles ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://coredns.io/2017/07/23/corefile-explained/) zu unterstützen.</td>
</tr>
<tr>
<td>GPU-Einheiten-Plug-in und Installationsprogramm</td>
<td>e32d51c</td>
<td>9ff3fda</td>
<td>GPU-Treiber wurden auf [418.43 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.nvidia.com/object/unix.html) aktualisiert. Die Aktualisierung enthält einen Fix für [CVE-2019-9741 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9741.html).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage-Plug-in</td>
<td>344</td>
<td>345</td>
<td>Es wurde Unterstützung für [private Serviceendpunkte](/docs/containers?topic=containers-cs_network_cluster#set-up-private-se) hinzugefügt.</td>
</tr>
<tr>
<td>Kernel</td>
<td>4.4.0-141</td>
<td>4.4.0-143</td>
<td>Workerknoten-Images mit Kernelaktualisierung für [CVE-2019-6133 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-6133.html) wurden aktualisiert.</td>
</tr>
<tr>
<td>Key Management Service-Provider</td>
<td>136</td>
<td>166</td>
<td>Das Image für [CVE-2018-16890 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16890), [CVE-2019-3822 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3822) und [CVE-2019-3823 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3823) wurde aktualisiert.</td>
</tr>
<tr>
<td>Trusted Compute-Agent</td>
<td>5f3d092</td>
<td>a02f765</td>
<td>Das Image für [CVE-2018-10779 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10779), [CVE-2018-12900 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-12900), [CVE-2018-17000 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-17000), [CVE-2018-19210 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-19210), [CVE-2019-6128 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6128) und [CVE-2019-7663 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-7663) wurde aktualisiert.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für 1.12.6_1541, veröffentlicht am 4. März 2019
{: #1126_1541}

In der folgenden Tabelle finden Sie die Änderungen, die in Patch 1.12.6_1541 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.12.5_1540 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.12.5_1540</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Cluster-DNS-Provider</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Die Speicherbegrenzung für Kubernetes-DNS- und CoreDNS-Pod-Speicher wurde von `170Mi` auf `400Mi` erhöht, um mehr Cluster-Services verarbeiten zu können.</td>
</tr>
<tr>
<td>GPU-Einheiten-Plug-in und Installationsprogramm</td>
<td>eb3a259</td>
<td>e32d51c</td>
<td>Die Images für [CVE-2019-6454 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454) wurden aktualisiert.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.12.5-137</td>
<td>v1.12.6-157</td>
<td>Wurde für die Unterstützung von Kubernetes 1.12.6 aktualisiert. Behebt regelmäßig auftretende Konnektivitätsprobleme für Lastausgleichsfunktionen der Version 1.0, die `externalTrafficPolicy` auf den Wert `local` setzen. Ereignisse der Lastausgleichsfunktion von Version 1.0 und 2.0 wurden zur Verwendung der neuesten {{site.data.keyword.Bluemix_notm}}-Dokumentationslinks aktualisiert.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage-Plug-in</td>
<td>342</td>
<td>344</td>
<td>Basisbetriebssystem für das Image von Fedora in Alpine geändert. Das Image für [CVE-2019-6486 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) wurde aktualisiert.</td>
</tr>
<tr>
<td>Key Management Service-Provider</td>
<td>122</td>
<td>136</td>
<td>Das Clientzeitlimit für {{site.data.keyword.keymanagementservicefull_notm}} wurde erhöht. Das Image für [CVE-2019-6486 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) wurde aktualisiert.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.5</td>
<td>v1.12.6</td>
<td>Lesen Sie die [Kubernetes-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.6). Die Aktualisierung löst [CVE-2019-6486 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) und [CVE-2019-1002100 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1002100).</td>
</tr>
<tr>
<td>Kubernetes-Konfiguration</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Die Einstellung `ExperimentalCriticalPodAnnotation=true` wurde der Option `--feature-gates` hinzugefügt. Diese Einstellung hilft bei der Migration von Pods von der veralteten Annotation `scheduler.alpha.kubernetes.io/critical-pod` auf die [Unterstützung für die Kubernetes-Podpriorität](/docs/containers?topic=containers-pod_priority#pod_priority).</td>
</tr>
<tr>
<td>Lastausgleichsfunktion und Monitor für die Lastausgleichsfunktion für {{site.data.keyword.Bluemix_notm}} Provider</td>
<td>132</td>
<td>143</td>
<td>Das Image für [CVE-2019-6486 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) wurde aktualisiert.</td>
</tr>
<tr>
<td>OpenVPN-Client und -Server</td>
<td>2.4.6-r3-IKS-13</td>
<td>2.4.6-r3-IKS-25</td>
<td>Das Image für [CVE-2019-1559 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559) wurde aktualisiert.</td>
</tr>
<tr>
<td>Trusted Compute-Agent</td>
<td>1ea5ad3</td>
<td>5f3d092</td>
<td>Das Image für [CVE-2019-6454 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454) wurde aktualisiert.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten-Fixpack 1.12.5_1540, veröffentlicht am 27. Februar 2019
{: #1125_1540}

In der folgenden Tabelle finden Sie die Änderungen, die im Workerknoten-Fixpack 1.12.5_1540 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.12.5_1538 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.12.5_1538</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-141</td>
<td>4.4.0-142</td>
<td>Workerknoten-Images mit Kernelaktualisierung für [CVE-2018-19407 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-142.168/changelog) wurden aktualisiert.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten-Fixpack 1.12.5_1538, veröffentlicht am 15. Februar 2019
{: #1125_1538}

In der folgenden Tabelle finden Sie die Änderungen, die im Workerknoten-Fixpack 1.12.5_1538 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.12.5_1537 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.12.5_1537</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Konfiguration des Hochverfügbarkeitsproxys des Cluster-Masters</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Die Podkonfigurationeinstellung `spec.priorityClassName` wurde in den Wert `system-node-critical` geändert und `spec.priority` wurde auf den Wert `2000001000` gesetzt.</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.5</td>
<td>1.1.6</td>
<td>Lesen Sie die [containerd-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/containerd/containerd/releases/tag/v1.1.6). Die Aktualisierung behebt [CVE-2019-5736 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5736).</td>
</tr>
<tr>
<td>Konfiguration von Kubernetes `kubelet`</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Das Feature-Gate `ExperimentalCriticalPodAnnotation` wurde aktiviert, um das Entfernen kritischer statischer Pods zu vermeiden.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für 1.12.5_1537, veröffentlicht am 5. Februar 2019
{: #1125_1537}

In der folgenden Tabelle finden Sie die Änderungen, die in Patch 1.12.5_1537 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.12.4_1535 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.12.4_1535</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>etcd</td>
<td>Version 3.3.1</td>
<td>Version 3.3.11</td>
<td>Lesen Sie die [etcd-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/coreos/etcd/releases/v3.3.11). Weiterhin wurden die unterstützten Cipher-Suites für 'etcd' jetzt auf eine Untergruppe mit starker Verschlüsselung (128 Bit oder mehr) eingeschränkt.</td>
</tr>
<tr>
<td>GPU-Einheiten-Plug-in und Installationsprogramm</td>
<td>13fdc0d</td>
<td>eb3a259</td>
<td>Die Images für [CVE-2019-3462 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) und [CVE-2019-6486 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) wurden aktualisiert.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.12.4-118</td>
<td>v1.12.5-137</td>
<td>Wurde für die Unterstützung von Kubernetes 1.12.5 aktualisiert. Außerdem wurde `calicoctl` auf Version 3.3.1 aktualisiert. Unnötige Konfigurationsaktualisierungen auf Lastausgleichsfunktionen der Version 2.0 bei Änderungen des Workerknotenstatus behoben.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage-Plug-in</td>
<td>338</td>
<td>342</td>
<td>Das Dateispeicher-Plug-in wurde wie folgt aktualisiert:
<ul><li>Unterstützt dynamisches Bereitstellen mit [für die Datenträgertopologie sensitiver Planung](/docs/containers?topic=containers-file_storage#file-topology).</li>
<li>Ignoriert Fehler beim Löschen von PersistentVolumeClaims (PVC), wenn der Speicher bereits gelöscht ist.</li>
<li>Fügt fehlgeschlagenen PVCs eine Fehlernachrichtenannotation hinzu.</li>
<li>Optimiert die Einstellungen für die Auswahl des leitenden Service (Leader) und für den Resynchronisationszeitraum der Speicherbereitstellungscontrollers und erhöht das Bereitstellungszeitlimit von 30 Minuten auf 1 Stunde.</li>
<li>Prüft Benutzerberechtigungen vor dem Start der Bereitstellung.</li></ul></td>
</tr>
<tr>
<td>Key Management Service-Provider</td>
<td>111</td>
<td>122</td>
<td>Es wurde Wiederholungslogik hinzugefügt, um vorübergehende Ausfälle zu vermeiden, wenn geheime Kubernetes-Schlüssel von {{site.data.keyword.keymanagementservicefull_notm}} verwaltet werden.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.4</td>
<td>v1.12.5</td>
<td>Lesen Sie die [Kubernetes-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.5).</td>
</tr>
<tr>
<td>Kubernetes-Konfiguration</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Die Konfiguration der Auditrichtlinie des Kubernetes-API-Servers wurde aktualisiert, um Protokollierungsmetadaten für `cluster-admin`-Anforderungen und eine Protokollierung des Anforderungshauptteils von Workloadanforderungen `create`, `update` und `patch` einzuschließen.</td>
</tr>
<tr>
<td>OpenVPN-Client</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>Das Image für [CVE-2018-0734 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) und [CVE-2018-5407 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407) wurde aktualisiert. Darüber hinaus wird die Podkonfiguration jetzt aus einem geheimen Schlüssel und nicht aus einer Konfigurationszuordnung (Configmap) abgerufen.</td>
</tr>
<tr>
<td>OpenVPN-Server</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>Das Image für [CVE-2018-0734 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) und [CVE-2018-5407 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407) wurde aktualisiert.</td>
</tr>
<tr>
<td>systemd</td>
<td>230</td>
<td>229</td>
<td>Sicherheitspatch für [CVE-2018-16864 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16864).</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten-Fixpack 1.12.4_1535, veröffentlicht am 28. Januar 2019
{: #1124_1535}

In der folgenden Tabelle finden Sie die Änderungen, die im Workerknoten-Fixpack 1.12.4_1535 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.12.4_1534 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.12.4_1534</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu-Pakete</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Installierte Ubuntu-Pakete, einschließlich `apt` für [CVE-2019-3462 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) und [USN-3863-1 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://usn.ubuntu.com/3863-1), wurden aktualisiert.</td>
</tr>
</tbody>
</table>


### Änderungsprotokoll für 1.12.4_1534, veröffentlicht am 21. Januar 2019
{: #1124_1534}

In der folgenden Tabelle finden Sie die Änderungen, die in Patch 1.12.3_1534 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.12.3_1533 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.12.3_1533</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>Version 1.12.3-91</td>
<td>v1.12.4-118</td>
<td>Wurde für die Unterstützung von Kubernetes 1.12.4 aktualisiert.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>Version 1.12.3</td>
<td>v1.12.4</td>
<td>Lesen Sie die [Kubernetes-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.4).</td>
</tr>
<tr>
<td>Kubernetes Add-on Resizer</td>
<td>1.8.1</td>
<td>1.8.4</td>
<td>Lesen Sie die [Releaseinformationen zu Kubernetes Add-on Resizer ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/autoscaler/releases/tag/addon-resizer-1.8.4).</td>
</tr>
<tr>
<td>Kubernetes-Dashboard</td>
<td>Version 1.8.3</td>
<td>Version 1.10.1</td>
<td>Lesen Sie die [Releaseinformationen zum Kubernetes-Dashboard ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/dashboard/releases/tag/v1.10.1). Die Aktualisierung behebt [CVE-2018-18264 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-18264).</td>
</tr>
<tr>
<td>GPU-Installationsprogramm</td>
<td>390.12</td>
<td>410.79</td>
<td>Die installierten GPU-Treiber wurden auf 410.79 aktualisiert.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten-Fixpack 1.12.3_1533, veröffentlicht am 7. Januar 2019
{: #1123_1533}

In der folgenden Tabelle finden Sie die Änderungen, die im Workerknoten-Fixpack 1.12.3_1533 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.12.3_1532 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.12.3_1532</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-139</td>
<td>4.4.0-141</td>
<td>Aktualisierte Workerknoten-Images mit Kernelaktualisierung für [CVE-2017-5753, CVE-2018-18690 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-141.167/changelog).</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten-Fixpack 1.12.3_1532, veröffentlicht am 17. Dezember 2018
{: #1123_1532}

In der folgenden Tabelle finden Sie die Änderungen, die im Workerknoten-Fixpack 1.12.2_1532 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.12.3_1531 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.12.3_1531</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu-Pakete</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Aktualisierungen an installierten Ubuntu-Paketen.</td>
</tr>
</tbody>
</table>


### Änderungsprotokoll für 1.12.3_1531, veröffentlicht am 5. Dezember 2018
{: #1123_1531}

In der folgenden Tabelle finden Sie die Änderungen, die in Patch 1.12.3_1531 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.12.2_1530 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.12.2_1530</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>Version 1.12.2-68</td>
<td>Version 1.12.3-91</td>
<td>Wurde für die Unterstützung von Kubernetes 1.12.3 aktualisiert.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>Version 1.12.2</td>
<td>Version 1.12.3</td>
<td>Lesen Sie die [Kubernetes-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.3). Die Aktualisierung behebt [CVE-2018-1002105 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/kubernetes/issues/71411).</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten-Fixpack 1.12.2_1530, veröffentlicht am 4. Dezember 2018
{: #1122_1530}

In der folgenden Tabelle finden Sie die Änderungen, die im Workerknoten-Fixpack 1.12.2_1530 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.12.2_1529 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.12.2_1529</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ressourcenauslastung von Workerknoten</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Es wurden dedizierte 'cgroups' für 'kubelet' und 'containerd' hinzugefügt, damit diesen Komponenten nicht die Ressourcen ausgehen. Weitere Informationen finden Sie unter [Reserven für Workerknotenressourcen](/docs/containers?topic=containers-plan_clusters#resource_limit_node).</td>
</tr>
</tbody>
</table>



### Änderungsprotokoll für 1.12.2_1529, veröffentlicht am 27. November 2018
{: #1122_1529}

In der folgenden Tabelle finden Sie die Änderungen, die in Patch 1.12.2_1529 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.12.2_1528 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.12.2_1528</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>Version 3.2.1</td>
<td>Version 3.3.1</td>
<td>Lesen Sie die [Calico-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.projectcalico.org/v3.3/releases/#v331). Die Aktualisierung behebt [Tigera Technical Advisory TTA-2018-001 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.projectcalico.org/security-bulletins/).</td>
</tr>
<tr>
<td>Cluster-DNS-Konfiguration</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Es wurde ein Fehler behoben, der dazu führen konnte, dass sowohl die Kubernetes-DNS- als auch die CoreDNS-Pods nach der Clustererstellung oder nach Aktualisierungsoperationen ausgeführt werden.</td>
</tr>
<tr>
<td>containerd</td>
<td>Version 1.2.0</td>
<td>Version 1.1.5</td>
<td>Lesen Sie die [containerd-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/containerd/containerd/releases/tag/v1.1.5). 'containerd' wurde aktualisiert, um einen Deadlock zu beheben, der verhindern kann, dass [Pods beendet werden ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/containerd/containerd/issues/2744).</td>
</tr>
<tr>
<td>OpenVPN-Client und -Server</td>
<td>2.4.4-r1-6</td>
<td>2.4.6-r3-IKS-8</td>
<td>Das Image für [CVE-2018-0732 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732) und [CVE-2018-0737 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737) wurde aktualisiert.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten-Fixpack 1.12.2_1528, veröffentlicht am 19. November 2018
{: #1122_1528}

In der folgenden Tabelle finden Sie die Änderungen, die im Workerknoten-Fixpack 1.12.2_1528 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.12.2_1527 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.12.2_1527</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-137</td>
<td>4.4.0-139</td>
<td>Workerknoten-Images mit Kernelaktualisierung für [CVE-2018-7755 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-139.165/changelog) wurden aktualisiert.</td>
</tr>
</tbody>
</table>


### Änderungsprotokoll für 1.12.2_1527, veröffentlicht am 7. November 2018
{: #1122_1527}

In der folgenden Tabelle finden Sie die Änderungen, die in Patch 1.12.2_1527 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.11.3_1533 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.11.3_1533</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico-Konfiguration</td>
<td>n.z.</td>
<td>n.z.</td>
<td>`calico-*`-Calico-Pods im Namensbereich `kube-system` legen jetzt CPU- und Speicherressourcenanforderungen für alle Container fest.</td>
</tr>
<tr>
<td>Cluster-DNS-Provider</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Kubernetes-DNS (KubeDNS) bleibt der Standard-Cluster-DNS-Provider. Sie haben jetzt aber die Möglichkeit, den [Cluster-DNS-Provider in CoreDNS zu ändern](/docs/containers?topic=containers-cluster_dns#dns_set).</td>
</tr>
<tr>
<td>Provider von Clustermetriken</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Der Kubernetes-Metrik-Server ersetzt den Kubernetes-Heapster (veraltet seit Kubernetes-Version 1.8) als Provider von Clustermetriken. Aktionselemente finden Sie unter [der Vorbereitungsaktion `metrics-server`](/docs/containers?topic=containers-cs_versions#metrics-server).</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.4</td>
<td>1.2.0</td>
<td>Lesen Sie die [containerd-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/containerd/containerd/releases/tag/v1.2.0).</td>
</tr>
<tr>
<td>CoreDNS</td>
<td>n.z.</td>
<td>1.2.2</td>
<td>Lesen Sie die [CoreDNS-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/coredns/coredns/releases/tag/v1.2.2).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>Version 1.11.3</td>
<td>Version 1.12.2</td>
<td>Lesen Sie die [Kubernetes-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.2).</td>
</tr>
<tr>
<td>Kubernetes-Konfiguration</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Es wurden drei neue IBM Pod-Sicherheitsrichtlinien und die ihnen zugeordneten Clusterrollen hinzugefügt. Weitere Informationen finden Sie unter [Erklärung der Standardressourcen für das IBM Cluster-Management](/docs/containers?topic=containers-psp#ibm_psp).</td>
</tr>
<tr>
<td>Kubernetes-Dashboard</td>
<td>Version 1.8.3</td>
<td>Version 1.10.0</td>
<td>Lesen Sie die [Releaseinformationen zum Kubernetes-Dashboard ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/dashboard/releases/tag/v1.10.0).<br><br>
Wenn Sie über `kubectl proxy` auf das Dashboard zugreifen, wird die Schaltfläche zum Überspringen (**SKIP**) von der Anmeldeseite entfernt. [Verwenden Sie stattdessen ein **Token** für die Anmeldung](/docs/containers?topic=containers-app#cli_dashboard). Außerdem können Sie jetzt die Anzahl von Kubernetes-Dashboard-Pods nach oben skalieren, indem Sie `kubectl -n kube-system scale deploy kubernetes-dashboard --replicas=3` ausführen.</td>
</tr>
<tr>
<td>Kubernetes-DNS</td>
<td>1.14.10</td>
<td>1.14.13</td>
<td>Lesen Sie die [Kubernetes-DNS-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/dns/releases/tag/1.14.13).</td>
</tr>
<tr>
<td>Kubernetes-Metrik-Server</td>
<td>n.z.</td>
<td>Version 0.3.1</td>
<td>Lesen Sie die [Releaseinformationen zum Kubernetes-Metrik-Server ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes-incubator/metrics-server/releases/tag/v0.3.1).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>Version 1.11.3-118</td>
<td>Version 1.12.2-68</td>
<td>Wurde für die Unterstützung von Kubernetes 1.12 aktualisiert. Zusätzliche Änderungen sind:
<ul><li>Pods der Lastausgleichsfunktion (`ibm-cloud-provider-ip-*` im Namensbereich `ibm-system`) legen jetzt CPU- und Speicherressourcenanforderungen fest.</li>
<li>Die Annotation `service.kubernetes.io/ibm-load-balancer-cloud-provider-vlan` wird hinzugefügt, um das VLAN anzugeben, in dem der Lastausgleichsservice bereitgestellt ist. Um verfügbare VLANs in Ihrem Cluster anzuzeigen, führen Sie diesen Befehl aus: `ibmcloud ks vlans --zone <zone>`.</li>
<li>Eine neue [Lastausgleichsfunktion 2.0](/docs/containers?topic=containers-loadbalancer#planning_ipvs) ist jetzt als Betaversion verfügbar.</li></ul></td>
</tr>
<tr>
<td>OpenVPN-Clientkonfiguration</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Der OpenVPN-Client `vpn-* pod` im Namensbereich `kube-system` legt jetzt CPU- und Speicherressourcenanforderungen fest.</td>
</tr>
</tbody>
</table>

## Version 1.11, Änderungsprotokoll
{: #111_changelog}

Überprüfen Sie das Änderungsprotokoll der Version 1.11.

### Änderungsprotokoll für Workerknoten-Fixpack 1.11.9_1555, veröffentlicht am 15. April 2019
{: #1119_1555}

In der folgenden Tabelle finden Sie die Änderungen, die im Workerknoten-Fixpack 1.11.9_1555 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.11.9_1554 vorgenommene Änderungen">
<caption>Änderungen seit 1.11.9_1554</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu-Pakete</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Es wurden Aktualisierungen an installierten Ubuntu-Paketen, darunter `systemd` für [CVE-2019-3842 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-3842.html), vorgenommen.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für 1.11.9_1554, veröffentlicht am 8. April 2019
{: #1119_1554}

In der folgenden Tabelle finden Sie die Änderungen, die in Patch 1.11.9_1554 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.11.8_1553 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.11.8_1553</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>Version 3.3.1</td>
<td>Version 3.3.6</td>
<td>Lesen Sie die [Calico-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.projectcalico.org/v3.3/releases/#v336). Die Aktualisierung behebt [CVE-2019-9946 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9946).</td>
</tr>
<tr>
<td>Hochverfügbarkeitsproxy des Cluster-Masters</td>
<td>1.8.12-alpine</td>
<td>1.9.6-alpine</td>
<td>Lesen Sie die [HAProxy-Releaseinformationen![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.haproxy.org/download/1.9/src/CHANGELOG). Die Aktualisierung behebt [CVE-2018-0732 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732), [CVE-2018-0734 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734), [CVE-2018-0737 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737), [CVE-2018-5407 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407), [CVE-2019-1543 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543) und [CVE-2019-1559 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.11.8-219</td>
<td>Version 1.11.9-241</td>
<td>Es wurde eine Aktualisierung zur Unterstützung von Kubernetes 1.11.9 und Calico 3.3.6 vorgenommen.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.8</td>
<td>Version 1.11.9</td>
<td>Lesen Sie die [Kubernetes-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.9).</td>
</tr>
<tr>
<td>Kubernetes-DNS</td>
<td>1.14.10</td>
<td>1.14.13</td>
<td>Lesen Sie die [Kubernetes-DNS-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/dns/releases/tag/1.14.13).</td>
</tr>
<tr>
<td>Trusted Compute-Agent</td>
<td>a02f765</td>
<td>e132aa4</td>
<td>Das Image für [CVE-2017-12447 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-12447) wurde aktualisiert.</td>
</tr>
<tr>
<td>Ubuntu 16.04 Kernel</td>
<td>4s.4.0-143 generisch</td>
<td>4.4.0-145 generisch</td>
<td>Es wurden Workerknoten-Images mit Kernelaktualisierung für [CVE-2019-9213 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html) aktualisiert.</td>
</tr>
<tr>
<td>Ubuntu 18.04 Kernel</td>
<td>4.15.0-46 generisch</td>
<td>4.15.0-47 generisch</td>
<td>Es wurden Workerknoten-Images mit Kernelaktualisierung für [CVE-2019-9213 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html) aktualisiert.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten-Fixpack 1.11.8_1553, veröffentlicht am 1. April 2019
{: #1118_1553}

In der folgenden Tabelle finden Sie die Änderungen, die im Workerknoten-Fix 1.11.8_1553 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.11.8_1552 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.11.8_1552</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ressourcenauslastung von Workerknoten</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Es wurden die Speicherreservierungen für 'kubelet' und 'containerd' erhöht, damit diesen Komponenten nicht die Ressourcen ausgehen. Weitere Informationen finden Sie unter [Reserven für Workerknotenressourcen](/docs/containers?topic=containers-plan_clusters#resource_limit_node).</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Master-Fixpack 1.11.8_1552, veröffentlicht am 26. März 2019
{: #1118_1552}

In der folgenden Tabelle finden Sie die Änderungen, die im Master-Fixpack 1.11.8_1552 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.11.8_1550 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.11.8_1550</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage-Plug-in</td>
<td>345</td>
<td>346</td>
<td>Das Image für [CVE-2019-9741 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741) wurde aktualisiert.</td>
</tr>
<tr>
<td>Key Management Service-Provider</td>
<td>166</td>
<td>167</td>
<td>Korrigiert die sporadisch auftretenden Fehler `context deadline exceeded` und `timeout` bei der Verwaltung von geheimen Kubernetes-Schlüsseln. Zusätzlich werden Aktualisierungen am Schlüsselmanagementservice (KMS), bei denen vorhandene geheime Kubernetes-Schlüssel unverschlüsselt bleiben könnten, korrigiert. Die Aktualisierung enthält einen Fix für [CVE-2019-9741 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
<tr>
<td>Lastausgleichsfunktion und Monitor für die Lastausgleichsfunktion für {{site.data.keyword.Bluemix_notm}} Provider</td>
<td>143</td>
<td>146</td>
<td>Das Image für [CVE-2019-9741 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741) wurde aktualisiert.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für 1.11.8_1550, veröffentlicht am 20. März 2019
{: #1118_1550}

In der folgenden Tabelle finden Sie die Änderungen, die in Patch 1.11.8_1550 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.11.8_1547 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.11.8_1547</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Konfiguration des Hochverfügbarkeitsproxys des Cluster-Masters</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Die Konfiguration wurde aktualisiert, um sporadisch auftretende Fehler der Verbindung zum Cluster-Master besser aufzufangen.</td>
</tr>
<tr>
<td>GPU-Einheiten-Plug-in und Installationsprogramm</td>
<td>e32d51c</td>
<td>9ff3fda</td>
<td>GPU-Treiber wurden auf [418.43 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.nvidia.com/object/unix.html) aktualisiert. Die Aktualisierung enthält einen Fix für [CVE-2019-9741 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9741.html).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage-Plug-in</td>
<td>344</td>
<td>345</td>
<td>Es wurde Unterstützung für [private Serviceendpunkte](/docs/containers?topic=containers-cs_network_cluster#set-up-private-se) hinzugefügt.</td>
</tr>
<tr>
<td>Kernel</td>
<td>4.4.0-141</td>
<td>4.4.0-143</td>
<td>Workerknoten-Images mit Kernelaktualisierung für [CVE-2019-6133 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-6133.html) wurden aktualisiert.</td>
</tr>
<tr>
<td>Key Management Service-Provider</td>
<td>136</td>
<td>166</td>
<td>Das Image für [CVE-2018-16890 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16890), [CVE-2019-3822 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3822) und [CVE-2019-3823 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3823) wurde aktualisiert.</td>
</tr>
<tr>
<td>Trusted Compute-Agent</td>
<td>5f3d092</td>
<td>a02f765</td>
<td>Das Image für [CVE-2018-10779 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10779), [CVE-2018-12900 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-12900), [CVE-2018-17000 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-17000), [CVE-2018-19210 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-19210), [CVE-2019-6128 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6128) und [CVE-2019-7663 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-7663) wurde aktualisiert.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für 1.11.8_1547, veröffentlicht am 4. März 2019
{: #1118_1547}

In der folgenden Tabelle finden Sie die Änderungen, die in Patch 1.11.8_1547 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.11.7_1546 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.11.7_1546</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>GPU-Einheiten-Plug-in und Installationsprogramm</td>
<td>eb3a259</td>
<td>e32d51c</td>
<td>Die Images für [CVE-2019-6454 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454) wurden aktualisiert.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.11.7-198</td>
<td>v1.11.8-219</td>
<td>Wurde für die Unterstützung von Kubernetes 1.11.8 aktualisiert. Behebt regelmäßig auftretende Konnektivitätsprobleme für Lastausgleichsfunktionen, die `externalTrafficPolicy` auf den Wert `local` setzen. Ereignisse der Lastausgleichsfunktion wurden zur Verwendung der neuesten {{site.data.keyword.Bluemix_notm}}-Dokumentationslinks aktualisiert.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage-Plug-in</td>
<td>342</td>
<td>344</td>
<td>Basisbetriebssystem für das Image von Fedora in Alpine geändert. Das Image für [CVE-2019-6486 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) wurde aktualisiert.</td>
</tr>
<tr>
<td>Key Management Service-Provider</td>
<td>122</td>
<td>136</td>
<td>Das Clientzeitlimit für {{site.data.keyword.keymanagementservicefull_notm}} wurde erhöht. Das Image für [CVE-2019-6486 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) wurde aktualisiert.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.7</td>
<td>v1.11.8</td>
<td>Lesen Sie die [Kubernetes-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.8). Die Aktualisierung löst [CVE-2019-6486 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) und [CVE-2019-1002100 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1002100).</td>
</tr>
<tr>
<td>Kubernetes-Konfiguration</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Die Einstellung `ExperimentalCriticalPodAnnotation=true` wurde der Option `--feature-gates` hinzugefügt. Diese Einstellung hilft bei der Migration von Pods von der veralteten Annotation `scheduler.alpha.kubernetes.io/critical-pod` auf die [Unterstützung für die Kubernetes-Podpriorität](/docs/containers?topic=containers-pod_priority#pod_priority).</td>
</tr>
<tr>
<td>Kubernetes-DNS</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Die Speicherbegrenzung für Kubernetes-DNS-Pod-Speicher wurde von `170Mi` auf `400Mi` erhöht, um mehr Cluster-Services verarbeiten zu können.</td>
</tr>
<tr>
<td>Lastausgleichsfunktion und Monitor für die Lastausgleichsfunktion für {{site.data.keyword.Bluemix_notm}} Provider</td>
<td>132</td>
<td>143</td>
<td>Das Image für [CVE-2019-6486 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) wurde aktualisiert.</td>
</tr>
<tr>
<td>OpenVPN-Client und -Server</td>
<td>2.4.6-r3-IKS-13</td>
<td>2.4.6-r3-IKS-25</td>
<td>Das Image für [CVE-2019-1559 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559) wurde aktualisiert.</td>
</tr>
<tr>
<td>Trusted Compute-Agent</td>
<td>1ea5ad3</td>
<td>5f3d092</td>
<td>Das Image für [CVE-2019-6454 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454) wurde aktualisiert.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten-Fixpack 1.11.7_1546, veröffentlicht am 27. Februar 2019
{: #1117_1546}

In der folgenden Tabelle finden Sie die Änderungen, die im Workerknoten-Fixpack 1.11.7_1546 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.11.7_1546 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.11.7_1546</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-141</td>
<td>4.4.0-142</td>
<td>Workerknoten-Images mit Kernelaktualisierung für [CVE-2018-19407 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-142.168/changelog) wurden aktualisiert.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten-Fixpack 1.11.7_1544, veröffentlicht am 15. Februar 2019
{: #1117_1544}

In der folgenden Tabelle finden Sie die Änderungen, die im Workerknoten-Fixpack 1.11.7_1544 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.11.7_1543 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.11.7_1543</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Konfiguration des Hochverfügbarkeitsproxys des Cluster-Masters</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Die Podkonfigurationeinstellung `spec.priorityClassName` wurde in den Wert `system-node-critical` geändert und `spec.priority` wurde auf den Wert `2000001000` gesetzt.</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.5</td>
<td>1.1.6</td>
<td>Lesen Sie die [containerd-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/containerd/containerd/releases/tag/v1.1.6). Die Aktualisierung behebt [CVE-2019-5736 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5736).</td>
</tr>
<tr>
<td>Konfiguration von Kubernetes `kubelet`</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Das Feature-Gate `ExperimentalCriticalPodAnnotation` wurde aktiviert, um das Entfernen kritischer statischer Pods zu vermeiden.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für 1.11.7_1543, veröffentlicht am 5. Februar 2019
{: #1117_1543}

In der folgenden Tabelle finden Sie die Änderungen, die in Patch 1.11.7_1543 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.11.6_1541 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.11.6_1541</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>etcd</td>
<td>Version 3.3.1</td>
<td>Version 3.3.11</td>
<td>Lesen Sie die [etcd-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/coreos/etcd/releases/v3.3.11). Weiterhin wurden die unterstützten Cipher-Suites für 'etcd' jetzt auf eine Untergruppe mit starker Verschlüsselung (128 Bit oder mehr) eingeschränkt.</td>
</tr>
<tr>
<td>GPU-Einheiten-Plug-in und Installationsprogramm</td>
<td>13fdc0d</td>
<td>eb3a259</td>
<td>Die Images für [CVE-2019-3462 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) und [CVE-2019-6486 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) wurden aktualisiert.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.11.6-180</td>
<td>v1.11.7-198</td>
<td>Wurde für die Unterstützung von Kubernetes 1.11.7 aktualisiert. Außerdem wurde `calicoctl` auf Version 3.3.1 aktualisiert. Unnötige Konfigurationsaktualisierungen auf Lastausgleichsfunktionen der Version 2.0 bei Änderungen des Workerknotenstatus behoben.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage-Plug-in</td>
<td>338</td>
<td>342</td>
<td>Das Dateispeicher-Plug-in wurde wie folgt aktualisiert:
<ul><li>Unterstützt dynamisches Bereitstellen mit [für die Datenträgertopologie sensitiver Planung](/docs/containers?topic=containers-file_storage#file-topology).</li>
<li>Ignoriert Fehler beim Löschen von PersistentVolumeClaims (PVC), wenn der Speicher bereits gelöscht ist.</li>
<li>Fügt fehlgeschlagenen PVCs eine Fehlernachrichtenannotation hinzu.</li>
<li>Optimiert die Einstellungen für die Auswahl des leitenden Service (Leader) und für den Resynchronisationszeitraum der Speicherbereitstellungscontrollers und erhöht das Bereitstellungszeitlimit von 30 Minuten auf 1 Stunde.</li>
<li>Prüft Benutzerberechtigungen vor dem Start der Bereitstellung.</li></ul></td>
</tr>
<tr>
<td>Key Management Service-Provider</td>
<td>111</td>
<td>122</td>
<td>Es wurde Wiederholungslogik hinzugefügt, um vorübergehende Ausfälle zu vermeiden, wenn geheime Kubernetes-Schlüssel von {{site.data.keyword.keymanagementservicefull_notm}} verwaltet werden.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.6</td>
<td>v1.11.7</td>
<td>Lesen Sie die [Kubernetes-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.7).</td>
</tr>
<tr>
<td>Kubernetes-Konfiguration</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Die Konfiguration der Auditrichtlinie des Kubernetes-API-Servers wurde aktualisiert, um Protokollierungsmetadaten für `cluster-admin`-Anforderungen und eine Protokollierung des Anforderungshauptteils von Workloadanforderungen `create`, `update` und `patch` einzuschließen.</td>
</tr>
<tr>
<td>OpenVPN-Client</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>Das Image für [CVE-2018-0734 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) und [CVE-2018-5407 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407) wurde aktualisiert. Darüber hinaus wird die Podkonfiguration jetzt aus einem geheimen Schlüssel und nicht aus einer Konfigurationszuordnung (Configmap) abgerufen.</td>
</tr>
<tr>
<td>OpenVPN-Server</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>Das Image für [CVE-2018-0734 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) und [CVE-2018-5407 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407) wurde aktualisiert.</td>
</tr>
<tr>
<td>systemd</td>
<td>230</td>
<td>229</td>
<td>Sicherheitspatch für [CVE-2018-16864 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16864).</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten-Fixpack 1.11.6_1541, veröffentlicht am 28. Januar 2019
{: #1116_1541}

In der folgenden Tabelle finden Sie die Änderungen, die im Workerknoten-Fixpack 1.11.6_1541 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.11.6_1540 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.11.6_1540</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu-Pakete</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Installierte Ubuntu-Pakete, einschließlich `apt` für [CVE-2019-3462 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) / [USN-3863-1 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://usn.ubuntu.com/3863-1).</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für 1.11.6_1540, veröffentlicht am 21. Januar 2019
{: #1116_1540}

In der folgenden Tabelle finden Sie die Änderungen, die in Patch 1.11.6_1540 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.11.5_1539 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.11.5_1539</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>Version 1.11.5-152</td>
<td>v1.11.6-180</td>
<td>Wurde für die Unterstützung von Kubernetes 1.11.6 aktualisiert.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>Version 1.11.5</td>
<td>v1.11.6</td>
<td>Lesen Sie die [Kubernetes-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.6).</td>
</tr>
<tr>
<td>Kubernetes Add-on Resizer</td>
<td>1.8.1</td>
<td>1.8.4</td>
<td>Lesen Sie die [Releaseinformationen zu Kubernetes Add-on Resizer ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/autoscaler/releases/tag/addon-resizer-1.8.4).</td>
</tr>
<tr>
<td>Kubernetes-Dashboard</td>
<td>Version 1.8.3</td>
<td>Version 1.10.1</td>
<td>Lesen Sie die [Releaseinformationen zum Kubernetes-Dashboard ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/dashboard/releases/tag/v1.10.1). Die Aktualisierung behebt [CVE-2018-18264 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-18264).<br><br>Wenn Sie über `kubectl proxy` auf das Dashboard zugreifen, wird die Schaltfläche zum Überspringen (**SKIP**) von der Anmeldeseite entfernt. [Verwenden Sie stattdessen ein **Token** für die Anmeldung](/docs/containers?topic=containers-app#cli_dashboard).</td>
</tr>
<tr>
<td>GPU-Installationsprogramm</td>
<td>390.12</td>
<td>410.79</td>
<td>Die installierten GPU-Treiber wurden auf 410.79 aktualisiert.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten-Fixpack 1.11.5_1539, veröffentlicht am 7. Januar 2019
{: #1115_1539}

In der folgenden Tabelle finden Sie die Änderungen, die im Workerknoten-Fixpack 1.11.5_1539 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.11.5_1538 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.11.5_1538</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-139</td>
<td>4.4.0-141</td>
<td>Aktualisierte Workerknoten-Images mit Kernelaktualisierung für [CVE-2017-5753, CVE-2018-18690 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-141.167/changelog).</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten-Fixpack 1.11.5_1538, veröffentlicht am 17. Dezember 2018
{: #1115_1538}

In der folgenden Tabelle finden Sie die Änderungen, die im Workerknoten-Fixpack 1.11.5_1538 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.11.5_1537 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.11.5_1537</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu-Pakete</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Aktualisierungen an installierten Ubuntu-Paketen.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für 1.11.5_1537, veröffentlicht am 5. Dezember 2018
{: #1115_1537}

In der folgenden Tabelle finden Sie die Änderungen, die in Patch 1.11.5_1537 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.11.4_1536 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.11.4_1536</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>Version 1.11.4-142</td>
<td>Version 1.11.5-152</td>
<td>Wurde für die Unterstützung von Kubernetes 1.11.5 aktualisiert.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>Version 1.11.4</td>
<td>Version 1.11.5</td>
<td>Lesen Sie die [Kubernetes-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.5). Die Aktualisierung behebt [CVE-2018-1002105 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/kubernetes/issues/71411).</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten-Fixpack 1.11.4_1536, veröffentlicht am 4. Dezember 2018
{: #1114_1536}

In der folgenden Tabelle finden Sie die Änderungen, die im Workerknoten-Fixpack 1.11.4_1536 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.11.4_1535 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.11.4_1535</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ressourcenauslastung von Workerknoten</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Es wurden dedizierte 'cgroups' für 'kubelet' und 'containerd' hinzugefügt, damit diesen Komponenten nicht die Ressourcen ausgehen. Weitere Informationen finden Sie unter [Reserven für Workerknotenressourcen](/docs/containers?topic=containers-plan_clusters#resource_limit_node).</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für 1.11.4_1535, veröffentlicht am 27. November 2018
{: #1114_1535}

In der folgenden Tabelle finden Sie die Änderungen, die in Patch 1.11.4_1535 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.11.3_1534 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.11.3_1534</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>Version 3.2.1</td>
<td>Version 3.3.1</td>
<td>Lesen Sie die [Calico-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.projectcalico.org/v3.3/releases/#v331). Die Aktualisierung behebt [Tigera Technical Advisory TTA-2018-001 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.projectcalico.org/security-bulletins/).</td>
</tr>
<tr>
<td>containerd</td>
<td>Version 1.1.4</td>
<td>Version 1.1.5</td>
<td>Lesen Sie die [containerd-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/containerd/containerd/releases/tag/v1.1.5). 'containerd' wurde aktualisiert, um einen Deadlock zu beheben, der verhindern kann, dass [Pods beendet werden ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/containerd/containerd/issues/2744).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>Version 1.11.3-127</td>
<td>Version 1.11.4-142</td>
<td>Wurde für die Unterstützung von Kubernetes 1.11.4 aktualisiert.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>Version 1.11.3</td>
<td>Version 1.11.4</td>
<td>Lesen Sie die [Kubernetes-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.4).</td>
</tr>
<tr>
<td>OpenVPN-Client und -Server</td>
<td>2.4.4-r1-6</td>
<td>2.4.6-r3-IKS-8</td>
<td>Das Image für [CVE-2018-0732 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732) und [CVE-2018-0737 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737) wurde aktualisiert.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten-Fixpack 1.11.3_1534, veröffentlicht am 19. November 2018
{: #1113_1534}

In der folgenden Tabelle finden Sie die Änderungen, die im Workerknoten-Fixpack 1.11.3_1534 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.11.3_1533 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.11.3_1533</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-137</td>
<td>4.4.0-139</td>
<td>Workerknoten-Images mit Kernelaktualisierung für [CVE-2018-7755 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-139.165/changelog) wurden aktualisiert.</td>
</tr>
</tbody>
</table>


### Änderungsprotokoll für 1.11.3_1533, veröffentlicht am 7. November 2018
{: #1113_1533}

In der folgenden Tabelle finden Sie die Änderungen, die in Patch 1.11.3_1533 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.11.3_1531 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.11.3_1531</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Aktualisierung der Hochverfügbarkeit des Cluster-Masters</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Die Aktualisierung auf hoch verfügbare Master für Cluster, die Zugangs-Webhooks (z. B. `initializerconfigurations`, `mutatingwebhookconfigurations` oder `validatingwebhookconfigurations`) verwenden, wurde korrigiert. Sie können diese Webhooks mit Helm-Diagrammen verwenden, z. B. für [Container Image Security Enforcement](/docs/services/Registry?topic=registry-security_enforce#security_enforce).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.11.3-100</td>
<td>Version 1.11.3-127</td>
<td>Die Annotation `service.kubernetes.io/ibm-load-balancer-cloud-provider-vlan` wurde hinzugefügt, um das VLAN anzugeben, in dem der Lastausgleichsservice bereitgestellt ist. Um verfügbare VLANs in Ihrem Cluster anzuzeigen, führen Sie diesen Befehl aus: `ibmcloud ks vlans --zone <zone>`.</td>
</tr>
<tr>
<td>TPM-aktivierter Kernel</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Bare-Metal-Workerknoten mit TPM-Chips für Trusted Compute verwenden den Ubuntu-Standardkernel, bis die Vertrauensbeziehung eingerichtet ist. Wenn Sie die [Vertrauensbeziehung](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_feature_enable) in einem vorhandenen Cluster aktivieren, müssen Sie alle vorhandenen Bare-Metal-Workerknoten mit TPM-Chips [neu laden](/docs/containers?topic=containers-cs_cli_reference#cs_worker_reload). Um zu prüfen, ob ein Bare-Metal-Workerknoten über einen TPM-Chip verfügt, prüfen Sie das Feld **Trustable**, nachdem Sie den [Befehl](/docs/containers?topic=containers-cs_cli_reference#cs_machine_types) `ibmcloud ks machine-types --zone` ausgeführt haben.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Master-Fixpack 1.11.3_1531, veröffentlicht am 1. November 2018
{: #1113_1531_ha-master}

In der folgenden Tabelle finden Sie die Änderungen, die im Master-Fixpack 1.11.3_1531 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.11.3_1527 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.11.3_1527</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Cluster-Master</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Die Cluster-Master-Konfiguration wurde aktualisiert, um die Hochverfügbarkeit zu erhöhen. Cluster haben jetzt drei Kubernetes-Masterreplikate, die im Rahmen einer Hochverfügbarkeitskonfiguration eingerichtet werden, wobei jeder Master auf einem separaten physischen Host bereitgestellt wird. Und wenn sich Ihr Cluster in einer Zone mit mehreren Zonen befindet, werden die Master über Zonen verteilt.<br>Informationen zu den Aktionen, die Sie ausführen müssen, finden Sie unter [Aktualisierung auf hoch verfügbare Cluster-Master](/docs/containers?topic=containers-cs_versions#ha-masters). Diese Vorbereitungsaktionen finden Anwendung:<ul>
<li>Wenn Sie über eine Firewall oder über angepasste Calico-Netzrichtlinien verfügen.</li>
<li>Wenn Sie die Host-Ports `2040` oder `2041` auf Ihren Workerknoten verwenden.</li>
<li>Wenn Sie die IP-Adresse des Cluster-Masters für den clusterinternen Zugriff auf den Master verwendet haben.</li>
<li>Wenn eine Automatisierung vorhanden ist, die die Calico-API oder -CLI (`calicoctl`) aufruft, z. B. um Calico-Richtlinien zu erstellen.</li>
<li>Wenn Sie Kubernetes- oder Calico-Netzrichtlinien zum Steuern des Pod-Egress-Zugriffs auf den Master verwenden.</li></ul></td>
</tr>
<tr>
<td>Hochverfügbarkeitsproxy des Cluster-Masters</td>
<td>n.z.</td>
<td>1.8.12-alpine</td>
<td>Es wurde ein Pod `ibm-master-proxy-*` für den clientseitigen Lastausgleich auf allen Workerknoten hinzugefügt, sodass jeder Workerknotenclient Anforderungen an ein verfügbares Hochverfügbarkeitsmaster-Replikat weiterleiten kann.</td>
</tr>
<tr>
<td>etcd</td>
<td>Version 3.2.18</td>
<td>Version 3.3.1</td>
<td>Lesen Sie die [etcd-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/coreos/etcd/releases/v3.3.1).</td>
</tr>
<tr>
<td>Verschlüsseln von Daten in 'etcd'</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Bisher wurden 'etcd'-Daten in der NFS-Dateispeicherinstanz eines Masters gespeichert, in der ruhende Daten verschlüsselt werden. Jetzt werden 'etcd'-Daten auf der lokalen Platte des Masters gespeichert und in {{site.data.keyword.cos_full_notm}} gesichert. Daten werden während des Transits an {{site.data.keyword.cos_full_notm}} verschlüsselt und wenn sie ruhen. Die 'etcd'-Daten auf der lokalen Platte des Masters sind jedoch nicht verschlüsselt. Wenn die lokalen 'etcd'-Daten Ihres Masters verschlüsselt werden sollen, [aktivieren Sie {{site.data.keyword.keymanagementservicelong_notm}} in Ihrem Cluster](/docs/containers?topic=containers-encryption#keyprotect).</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten-Fixpack 1.11.3_1531, veröffentlicht am 26. Oktober 2018
{: #1113_1531}

In der folgenden Tabelle finden Sie die Änderungen, die im Workerknoten-Fixpack 1.11.3_1531 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.11.3_1525 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.11.3_1525</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Behandlung von Betriebssystem-Interrupts</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Der Systemdämon für Interruptanforderungen wurde durch einen leistungsfähigeren Interrupt-Handler ersetzt.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Master-Fixpack 1.11.3_1527, veröffentlicht am 15. Oktober 2018
{: #1113_1527}

In der folgenden Tabelle finden Sie die Änderungen, die im Master-Fixpack 1.11.3_1527 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.11.3_1524 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.11.3_1524</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico-Konfiguration</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Korrigiert Bereitschaftsprüfung für Container `calico-node` zur besseren Verarbeitung von Knotenfehlern.</td>
</tr>
<tr>
<td>Clusteraktualisierung</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Korrigiert Problem beim Aktualisieren von Cluster-Add-ons, wenn der Master von einer nicht unterstützten Version aktualisiert wird.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten-Fixpack 1.11.3_1525, veröffentlicht am 10. Oktober 2018
{: #1113_1525}

In der folgenden Tabelle finden Sie die Änderungen, die im Workerknoten-Fixpack 1.11.3_1525 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.11.3_1524 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.11.3_1524</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-133</td>
<td>4.4.0-137</td>
<td>Aktualisierte Workerknoten-Images mit Kernelaktualisierung für [CVE-2018-14633,CVE-2018-17182 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-137.163/changelog).</td>
</tr>
<tr>
<td>Inaktives Sitzungszeitlimit</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Legen Sie für das Zeitlimit für inaktive Sitzungen 5 Minuten fest, damit die Anforderungen erfüllt werden.</td>
</tr>
</tbody>
</table>


### Änderungsprotokoll für 1.11.3_1524, veröffentlicht am 2. Oktober 2018
{: #1113_1524}

In der folgenden Tabelle finden Sie die Änderungen, die in Patch 1.11.3_1524 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.11.3_1521 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.11.3_1521</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>containerd</td>
<td>1.1.3</td>
<td>1.1.4</td>
<td>Lesen Sie die [containerd-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/containerd/containerd/releases/tag/v1.1.4).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.11.3-91</td>
<td>v1.11.3-100</td>
<td>Der Dokumentationslink zu den Fehlernachrichten für die Lastausgleichsfunktion wurde aktualisiert.</td>
</tr>
<tr>
<td>IBM File Storage-Klassen</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Der doppelte Parameter `reclaimPolicy` wurde in den IBM File Storage-Klassen entfernt.<br><br>
Außerdem bleibt die IBM File Storage-Standardklasse beim Aktualisieren des Cluster-Masters unverändert. Falls Sie die Standardspeicherklasse ändern möchten, führen Sie `kubectl patch storageclass <storageclass> -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'` aus und ersetzen `<storageclass>` durch den Namen der Speicherklasse.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für 1.11.3_1521, veröffentlicht am 20. September 2018
{: #1113_1521}

In der folgenden Tabelle finden Sie die Änderungen, die in Patch 1.11.3_1521 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.11.2_1516 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.11.2_1516</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>Version 1.11.2-71</td>
<td>v1.11.3-91</td>
<td>Wurde für die Unterstützung von Kubernetes 1.11.3 aktualisiert.</td>
</tr>
<tr>
<td>IBM File Storage-Klassen</td>
<td>n.z.</td>
<td>n.z.</td>
<td>`mountOptions` wurde in den IBM File Storage-Klassen entfernt, damit der Standardwert verwendet wird, der vom Workerknoten bereitgestellt wird.<br><br>
Außerdem bleibt die IBM File Storage-Standardklasse beim Aktualisieren des Cluster-Masters `ibmc-file-bronze`. Falls Sie die Standardspeicherklasse ändern möchten, führen Sie `kubectl patch storageclass <storageclass> -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'` aus und ersetzen `<storageclass>` durch den Namen der Speicherklasse.</td>
</tr>
<tr>
<td>Key Management Service-Provider</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Die Möglichkeit zum Verwenden des Kubernetes-KMS-Providers (KMS - Key Management Service, Schlüsselmanagementservice) im Cluster wurde hinzugefügt, damit {{site.data.keyword.keymanagementservicefull}} unterstützt wird. Wenn Sie [{{site.data.keyword.keymanagementserviceshort}} im Cluster aktivieren](/docs/containers?topic=containers-encryption#keyprotect), werden alle geheimen Kubernetes-Schlüssel verschlüsselt.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>Version 1.11.2</td>
<td>Version 1.11.3</td>
<td>Lesen Sie die [Kubernetes-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.3).</td>
</tr>
<tr>
<td>Automatische Kubernetes DNS-Skalierung</td>
<td>1.1.2-r2</td>
<td>1.2.0</td>
<td>Lesen Sie die [Releaseinformationen zur automatischen Kubernetes-DNS-Skalierung ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes-incubator/cluster-proportional-autoscaler/releases/tag/1.2.0).</td>
</tr>
<tr>
<td>Protokollrotation</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Von Zeitgebern des Typs `cronjobs` wurde zu `systemd` gewechselt, um ein Fehlschlagen von `logrotate` auf Workerknoten zu vermeiden, die nicht innerhalb von 90 Tagen erneute geladen oder aktualisiert werden. **Hinweis**: In allen früheren Versionen für dieses Unterrelease war die primäre Platte mit Daten gefüllt, nachdem der Cron-Job fehlgeschlagen war, weil die Protokolle nicht rotiert wurden. Der Cron-Job schlägt fehl, nachdem der Workerknoten 90 Tage aktiv ist, ohne erneut aktualisiert oder geladen zu werden. Wenn die gesamte Speicherkapazität der primären Platte durch Protokolle erschöpft ist, wechselt der Workerknoten in den Status 'Fehlgeschlagen'. Der Fehler auf dem Workerknoten kann mit dem [command](/docs/containers?topic=containers-cs_cli_reference#cs_worker_reload) `ibmcloud ks worker-reload` oder dem [Befehl](/docs/containers?topic=containers-cs_cli_reference#cs_worker_update) `ibmcloud ks worker-update` behoben werden.</td>
</tr>
<tr>
<td>Ablauf des Rootkennworts</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Rootkennwörter für Workerknoten laufen aus Konformitätsgründen nach 90 Tagen ab. Wenn für die Automatisierungstools eine Anmeldung am Workerknoten erforderlich ist oder wenn sie auf Cron-Jobs angewiesen sind, die als Root ausgeführt werden, können Sie den Ablauf der Kennwortgültigkeit inaktivieren, indem Sie sich am Workerknoten anmelden und `chage -M -1 root` ausführen. **Hinweis**: Falls Konformitätsanforderungen für die Sicherheit gelten, die eine Ausführung als Root oder eine Entfernung des Ablaufs der Kennwortgültigkeit verhindert, inaktivieren Sie nicht das Ablaufdatum. Stattdessen können Sie die Workerknoten mindestens alle 90 Tage [aktualisieren](/docs/containers?topic=containers-cs_cli_reference#cs_worker_update) oder [erneut laden](/docs/containers?topic=containers-cs_cli_reference#cs_worker_reload).</td>
</tr>
<tr>
<td>Laufzeitkomponenten für Workerknoten (`kubelet`, `kube-proxy`, `containerd`)</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Entfernte Abhängigkeiten der Laufzeitkomponenten auf der primären Platte. Diese Erweiterung verhindert das Fehlschlagen von Workerknoten, wenn die Speicherkapazität auf der primären Platte erschöpft ist.</td>
</tr>
<tr>
<td>systemd</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Bereinigen Sie die temporären Mounteinheiten in regelmäßigen Abständen, um zu verhindern, dass sie unbegrenzt werden. Diese Aktion ist für [Kubernetes-Fehler 57345 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/kubernetes/issues/57345) vorgesehen.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für 1.11.2_1516, veröffentlicht am 4. September 2018
{: #1112_1516}

In der folgenden Tabelle finden Sie die Änderungen, die in Patch 1.11.2_1516 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.11.2_1514 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.11.2_1514</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>Version 3.1.3</td>
<td>Version 3.2.1</td>
<td>Lesen Sie die [Calico-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.projectcalico.org/v3.2/releases/#v321).</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.2</td>
<td>1.1.3</td>
<td>Lesen Sie die [`containerd`-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/containerd/containerd/releases/tag/v1.1.3).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>Version 1.11.2-60</td>
<td>Version 1.11.2-71</td>
<td>Die Cloud-Provider-Konfiguration wurde geändert, um die Verarbeitung von Aktualisierungen für die Lastausgleichsfunktionsservices zu verbessern, wenn `externalTrafficPolicy` auf `local` eingestellt ist.</td>
</tr>
<tr>
<td>Konfiguration des {{site.data.keyword.Bluemix_notm}} File Storage-Plug-ins</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Die NFS-Standardversion wurde aus den Mountoptionen in den von IBM bereitgestellten Dateispeicherklassen entfernt. Das Betriebssystem des Hosts vereinbart die NFS-Version jetzt mit dem NFS-Server der IBM Cloud-Infrastruktur (SoftLayer). Wenn Sie eine bestimmte NFS-Version manuell festlegen oder die NFS-Version Ihres PV ändern, die vom Betriebssystem des Hosts vereinbart wurde, finden Sie Informationen hierzu im Abschnitt [NFS-Standardversion ändern](/docs/containers?topic=containers-file_storage#nfs_version_class).</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten-Fixpack 1.11.2_1514, veröffentlicht am 23. August 2018
{: #1112_1514}

In der folgenden Tabelle finden Sie die Änderungen, die im Workerknoten-Fixpack 1.11.2_1514 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.11.2_1513 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.11.2_1513</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>`systemd`</td>
<td>229</td>
<td>230</td>
<td>Aktualisierung von `systemd` zur Korrektur des `cgroup`-Lecks.</td>
</tr>
<tr>
<td>Kernel</td>
<td>4.4.0-127</td>
<td>4.4.0-133</td>
<td>Aktualisierte Workerknoten-Images mit Kernelaktualisierung für [CVE-2018-3620,CVE-2018-3646 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://usn.ubuntu.com/3741-1/).</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für 1.11.2_1513, veröffentlicht am 14. August 2018
{: #1112_1513}

In der folgenden Tabelle finden Sie die Änderungen, die in Patch 1.11.2_1513 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.10.5_1518 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.10.5_1518</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>containerd</td>
<td>n.z.</td>
<td>1.1.2</td>
<td>`containerd` ersetzt Docker als neue Containerlaufzeit für Kubernetes. Lesen Sie die [`containerd`-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/containerd/containerd/releases/tag/v1.1.2). Informationen zu den Aktionen, die Sie ausführen müssen, finden Sie unter [Aktualisieren auf `containerd` als Containerlaufzeit](/docs/containers?topic=containers-cs_versions#containerd).</td>
</tr>
<tr>
<td>Docker</td>
<td>n.z.</td>
<td>n.z.</td>
<td>`containerd` ersetzt Docker zur Verbesserung der Leistung als neue Containerlaufzeit für Kubernetes. Informationen zu den Aktionen, die Sie ausführen müssen, finden Sie unter [Aktualisieren auf `containerd` als Containerlaufzeit](/docs/containers?topic=containers-cs_versions#containerd).</td>
</tr>
<tr>
<td>etcd</td>
<td>Version 3.2.14</td>
<td>Version 3.2.18</td>
<td>Lesen Sie die [etcd-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/coreos/etcd/releases/v3.2.18).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>Version 1.10.5-118</td>
<td>Version 1.11.2-60</td>
<td>Wurde für die Unterstützung von Kubernetes 1.11 aktualisiert. Darüber hinaus verwenden die Pods der Lastausgleichsfunktion jetzt die neue Podprioritätsklasse `ibm-app-cluster-critical`.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage-Plug-in</td>
<td>334</td>
<td>338</td>
<td>Aktualisierung von `incubator` auf Version 1.8. Der Dateispeicher wird für die von Ihnen ausgewählte Zone bereitgestellt. Sie können die Bezeichnungen einer vorhandenen (statischen) PV-Instanz nicht aktualisieren, es sei denn, Sie verwenden einen Mehrzonencluster und müssen die [Regions- und Zonenbezeichnungen hinzufügen](/docs/containers?topic=containers-kube_concepts#storage_multizone).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>Version 1.10.5</td>
<td>Version 1.11.2</td>
<td>Lesen Sie die [Kubernetes-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.2).</td>
</tr>
<tr>
<td>Kubernetes-Konfiguration</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Aktualisierung der OpenID Connect-Konfiguration für den Kubernetes-API-Server zur Unterstützung von {{site.data.keyword.Bluemix_notm}} Identity and Access Management-Zugriffsgruppen. Der Option `--enable-admission-plugins` wurde `Priority` für den Kubernetes-API-Server des Clusters hinzugefügt und der Cluster wurde für die Unterstützung der Podpriorität konfiguriert. Weitere Informationen finden Sie unter:
<ul><li>[{{site.data.keyword.Bluemix_notm}}IAM-Zugriffsgruppen](/docs/containers?topic=containers-users#rbac)</li>
<li>[Podpriorität konfigurieren](/docs/containers?topic=containers-pod_priority#pod_priority)</li></ul></td>
</tr>
<tr>
<td>Kubernetes Heapster</td>
<td>Version 1.5.2</td>
<td>Version 1.5.4</td>
<td>Höhere Ressourcengrenzen für den Container `heapster-nanny`. Lesen Sie die [Kubernetes Heapster-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/heapster/releases/tag/v1.5.4).</td>
</tr>
<tr>
<td>Protokollierungskonfiguration</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Als Containerprotokollverzeichnis wird nun `/var/log/pods/` anstelle von `/var/lib/docker/containers/` verwendet.</td>
</tr>
</tbody>
</table>

<br />


## Veraltet: Version 1.10, Änderungsprotokoll
{: #110_changelog}

Überprüfen Sie das Änderungsprotokoll der Version 1.10.
{: shortdesc}

Kubernetes Version 1.10 ist veraltet und wird ab dem 15. Mai 2019 nicht mehr unterstützt. [Überprüfen Sie die mögliche Auswirkung](/docs/containers?topic=containers-cs_versions#cs_versions) jeder einzelnen Aktualisierung einer Kubernetes-Version und [aktualisieren Sie Ihre Cluster](/docs/containers?topic=containers-update#update) dann sofort mindestens auf Version 1.11.
{: deprecated}

### Änderungsprotokoll für Workerknoten-Fixpack 1.10.13_1556, veröffentlicht am 15. April 2019
{: #11013_1556}

In der folgenden Tabelle finden Sie die Änderungen, die im Workerknoten-Fixpack 1.10.13_1556 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.10.13_1555 vorgenommene Änderungen">
<caption>Änderungen seit 1.10.13_1555</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu-Pakete</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Es wurden Aktualisierungen an installierten Ubuntu-Paketen, darunter `systemd` für [CVE-2019-3842 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-3842.html), vorgenommen.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für 1.10.13_1555, veröffentlicht am 8. April 2019
{: #11013_1555}

In der folgenden Tabelle finden Sie die Änderungen, die in Patch 1.10.13_1555 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.10.13_1554 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.10.13_1554</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Hochverfügbarkeitsproxy des Cluster-Masters</td>
<td>1.8.12-alpine</td>
<td>1.9.6-alpine</td>
<td>Lesen Sie die [HAProxy-Releaseinformationen![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.haproxy.org/download/1.9/src/CHANGELOG). Die Aktualisierung behebt [CVE-2018-0732 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732), [CVE-2018-0734 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734), [CVE-2018-0737 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737), [CVE-2018-5407 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407), [CVE-2019-1543 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543) und [CVE-2019-1559 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559).</td>
</tr>
<tr>
<td>Kubernetes-DNS</td>
<td>1.14.10</td>
<td>1.14.13</td>
<td>Lesen Sie die [Kubernetes-DNS-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/dns/releases/tag/1.14.13).</td>
</tr>
<tr>
<td>Trusted Compute-Agent</td>
<td>a02f765</td>
<td>e132aa4</td>
<td>Das Image für [CVE-2017-12447 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-12447) wurde aktualisiert.</td>
</tr>
<tr>
<td>Ubuntu 16.04 Kernel</td>
<td>4s.4.0-143 generisch</td>
<td>4.4.0-145 generisch</td>
<td>Es wurden Workerknoten-Images mit Kernelaktualisierung für [CVE-2019-9213 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html) aktualisiert.</td>
</tr>
<tr>
<td>Ubuntu 18.04 Kernel</td>
<td>4.15.0-46 generisch</td>
<td>4.15.0-47 generisch</td>
<td>Es wurden Workerknoten-Images mit Kernelaktualisierung für [CVE-2019-9213 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html) aktualisiert.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten-Fixpack 1.10.13_1554, veröffentlicht am 1. April 2019
{: #11013_1554}

In der folgenden Tabelle finden Sie die Änderungen, die im Workerknoten-Fix 1.10.13_1554 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.10.13_1553 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.10.13_1553</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ressourcenauslastung von Workerknoten</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Es wurden die Speicherreservierungen für 'kubelet' und 'containerd' erhöht, damit diesen Komponenten nicht die Ressourcen ausgehen. Weitere Informationen finden Sie unter [Reserven für Workerknotenressourcen](/docs/containers?topic=containers-plan_clusters#resource_limit_node).</td>
</tr>
</tbody>
</table>


### Änderungsprotokoll für Master-Fixpack 1.10.13_1553, veröffentlicht am 26. März 2019
{: #11118_1553}

In der folgenden Tabelle finden Sie die Änderungen, die im Master-Fixpack 1.10.13_1553 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.10.13_1551 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.10.13_1551</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage-Plug-in</td>
<td>345</td>
<td>346</td>
<td>Das Image für [CVE-2019-9741 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741) wurde aktualisiert.</td>
</tr>
<tr>
<td>Key Management Service-Provider</td>
<td>166</td>
<td>167</td>
<td>Korrigiert die sporadisch auftretenden Fehler `context deadline exceeded` und `timeout` bei der Verwaltung von geheimen Kubernetes-Schlüsseln. Zusätzlich werden Aktualisierungen am Schlüsselmanagementservice (KMS), bei denen vorhandene geheime Kubernetes-Schlüssel unverschlüsselt bleiben könnten, korrigiert. Die Aktualisierung enthält einen Fix für [CVE-2019-9741 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
<tr>
<td>Lastausgleichsfunktion und Monitor für die Lastausgleichsfunktion für {{site.data.keyword.Bluemix_notm}} Provider</td>
<td>143</td>
<td>146</td>
<td>Das Image für [CVE-2019-9741 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741) wurde aktualisiert.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für 1.10.13_1551, veröffentlicht am 20. März 2019
{: #11013_1551}

In der folgenden Tabelle finden Sie die Änderungen, die in Patch 1.10.13_1551 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.10.13_1548 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.10.13_1548</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Konfiguration des Hochverfügbarkeitsproxys des Cluster-Masters</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Die Konfiguration wurde aktualisiert, um sporadisch auftretende Fehler der Verbindung zum Cluster-Master besser aufzufangen.</td>
</tr>
<tr>
<td>GPU-Einheiten-Plug-in und Installationsprogramm</td>
<td>e32d51c</td>
<td>9ff3fda</td>
<td>GPU-Treiber wurden auf [418.43 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.nvidia.com/object/unix.html) aktualisiert. Die Aktualisierung enthält einen Fix für [CVE-2019-9741 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9741.html).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage-Plug-in</td>
<td>344</td>
<td>345</td>
<td>Es wurde Unterstützung für [private Serviceendpunkte](/docs/containers?topic=containers-cs_network_cluster#set-up-private-se) hinzugefügt.</td>
</tr>
<tr>
<td>Kernel</td>
<td>4.4.0-141</td>
<td>4.4.0-143</td>
<td>Workerknoten-Images mit Kernelaktualisierung für [CVE-2019-6133 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-6133.html) wurden aktualisiert.</td>
</tr>
<tr>
<td>Key Management Service-Provider</td>
<td>136</td>
<td>166</td>
<td>Das Image für [CVE-2018-16890 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16890), [CVE-2019-3822 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3822) und [CVE-2019-3823 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3823) wurde aktualisiert.</td>
</tr>
<tr>
<td>Trusted Compute-Agent</td>
<td>5f3d092</td>
<td>a02f765</td>
<td>Das Image für [CVE-2018-10779 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10779), [CVE-2018-12900 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-12900), [CVE-2018-17000 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-17000), [CVE-2018-19210 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-19210), [CVE-2019-6128 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6128) und [CVE-2019-7663 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-7663) wurde aktualisiert.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für 1.10.13_1548, veröffentlicht am 4. März 2019
{: #11013_1548}

In der folgenden Tabelle finden Sie die Änderungen, die in Patch 1.10.13_1548 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.10.12_1546 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.10.12_1546</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>GPU-Einheiten-Plug-in und Installationsprogramm</td>
<td>eb3a259</td>
<td>e32d51c</td>
<td>Die Images für [CVE-2019-6454 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454) wurden aktualisiert.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.10.12-252</td>
<td>v1.10.13-288</td>
<td>Wurde für die Unterstützung von Kubernetes 1.10.13 aktualisiert. Behebt regelmäßig auftretende Konnektivitätsprobleme für Lastausgleichsfunktionen, die `externalTrafficPolicy` auf den Wert `local` setzen. Ereignisse der Lastausgleichsfunktion wurden zur Verwendung der neuesten {{site.data.keyword.Bluemix_notm}}-Dokumentationslinks aktualisiert.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage-Plug-in</td>
<td>342</td>
<td>344</td>
<td>Basisbetriebssystem für das Image von Fedora in Alpine geändert. Das Image für [CVE-2019-6486 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) wurde aktualisiert.</td>
</tr>
<tr>
<td>Key Management Service-Provider</td>
<td>122</td>
<td>136</td>
<td>Das Clientzeitlimit für {{site.data.keyword.keymanagementservicefull_notm}} wurde erhöht. Das Image für [CVE-2019-6486 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) wurde aktualisiert.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.10.12</td>
<td>v1.10.13</td>
<td>Lesen Sie die [Kubernetes-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.13).</td>
</tr>
<tr>
<td>Kubernetes-DNS</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Die Speicherbegrenzung für Kubernetes-DNS-Pod-Speicher wurde von `170Mi` auf `400Mi` erhöht, um mehr Cluster-Services verarbeiten zu können.</td>
</tr>
<tr>
<td>Lastausgleichsfunktion und Monitor für die Lastausgleichsfunktion für {{site.data.keyword.Bluemix_notm}} Provider</td>
<td>132</td>
<td>143</td>
<td>Das Image für [CVE-2019-6486 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) wurde aktualisiert.</td>
</tr>
<tr>
<td>OpenVPN-Client und -Server</td>
<td>2.4.6-r3-IKS-13</td>
<td>2.4.6-r3-IKS-25</td>
<td>Das Image für [CVE-2019-1559 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559) wurde aktualisiert.</td>
</tr>
<tr>
<td>Trusted Compute-Agent</td>
<td>1ea5ad3</td>
<td>5f3d092</td>
<td>Das Image für [CVE-2019-6454 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454) wurde aktualisiert.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten-Fixpack 1.10.12_1546, veröffentlicht am 27. Februar 2019
{: #11012_1546}

In der folgenden Tabelle finden Sie die Änderungen, die im Workerknoten-Fixpack 1.10.12_1546 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.10.12_1544 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.10.12_1544</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-141</td>
<td>4.4.0-142</td>
<td>Workerknoten-Images mit Kernelaktualisierung für [CVE-2018-19407 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-142.168/changelog) wurden aktualisiert.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten-Fixpack 1.10.12_1544, veröffentlicht am 15. Februar 2019
{: #11012_1544}

In der folgenden Tabelle finden Sie die Änderungen, die im Workerknoten-Fixpack 1.10.12_1544 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.10.12_1543 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.10.12_1543</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Docker</td>
<td>18.06.1-ce</td>
<td>18.06.2-ce</td>
<td>Lesen Sie die [Releaseinformationen zu Docker Community Edition ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/docker/docker-ce/releases/tag/v18.06.2-ce). Die Aktualisierung behebt [CVE-2019-5736 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5736).</td>
</tr>
<tr>
<td>Konfiguration von Kubernetes `kubelet`</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Das Feature-Gate `ExperimentalCriticalPodAnnotation` wurde aktiviert, um das Entfernen kritischer statischer Pods zu vermeiden.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für 1.10.12_1543, veröffentlicht am 5. Februar 2019
{: #11012_1543}

In der folgenden Tabelle finden Sie die Änderungen, die in Patch 1.10.12_1543 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.10.12_1541 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.10.12_1541</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>etcd</td>
<td>Version 3.3.1</td>
<td>Version 3.3.11</td>
<td>Lesen Sie die [etcd-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/coreos/etcd/releases/v3.3.11). Weiterhin wurden die unterstützten Cipher-Suites für 'etcd' jetzt auf eine Untergruppe mit starker Verschlüsselung (128 Bit oder mehr) eingeschränkt.</td>
</tr>
<tr>
<td>GPU-Einheiten-Plug-in und Installationsprogramm</td>
<td>13fdc0d</td>
<td>eb3a259</td>
<td>Die Images für [CVE-2019-3462 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) und [CVE-2019-6486 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) wurden aktualisiert.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage-Plug-in</td>
<td>338</td>
<td>342</td>
<td>Das Dateispeicher-Plug-in wurde wie folgt aktualisiert:
<ul><li>Unterstützt dynamisches Bereitstellen mit [für die Datenträgertopologie sensitiver Planung](/docs/containers?topic=containers-file_storage#file-topology).</li>
<li>Ignoriert Fehler beim Löschen von PersistentVolumeClaims (PVC), wenn der Speicher bereits gelöscht ist.</li>
<li>Fügt fehlgeschlagenen PVCs eine Fehlernachrichtenannotation hinzu.</li>
<li>Optimiert die Einstellungen für die Auswahl des leitenden Service (Leader) und für den Resynchronisationszeitraum der Speicherbereitstellungscontrollers und erhöht das Bereitstellungszeitlimit von 30 Minuten auf 1 Stunde.</li>
<li>Prüft Benutzerberechtigungen vor dem Start der Bereitstellung.</li></ul></td>
</tr>
<tr>
<td>Key Management Service-Provider</td>
<td>111</td>
<td>122</td>
<td>Es wurde Wiederholungslogik hinzugefügt, um vorübergehende Ausfälle zu vermeiden, wenn geheime Kubernetes-Schlüssel von {{site.data.keyword.keymanagementservicefull_notm}} verwaltet werden.</td>
</tr>
<tr>
<td>Kubernetes-Konfiguration</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Die Konfiguration der Auditrichtlinie des Kubernetes-API-Servers wurde aktualisiert, um Protokollierungsmetadaten für `cluster-admin`-Anforderungen und eine Protokollierung des Anforderungshauptteils von Workloadanforderungen `create`, `update` und `patch` einzuschließen.</td>
</tr>
<tr>
<td>OpenVPN-Client</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>Das Image für [CVE-2018-0734 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) und [CVE-2018-5407 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407) wurde aktualisiert. Darüber hinaus wird die Podkonfiguration jetzt aus einem geheimen Schlüssel und nicht aus einer Konfigurationszuordnung (Configmap) abgerufen.</td>
</tr>
<tr>
<td>OpenVPN-Server</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>Das Image für [CVE-2018-0734 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) und [CVE-2018-5407 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407) wurde aktualisiert.</td>
</tr>
<tr>
<td>systemd</td>
<td>230</td>
<td>229</td>
<td>Sicherheitspatch für [CVE-2018-16864 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16864).</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten-Fixpack 1.10.12_1541, veröffentlicht am 28. Januar 2019
{: #11012_1541}

In der folgenden Tabelle finden Sie die Änderungen, die im Workerknoten-Fixpack 1.10.12_1541 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.10.12_1540 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.10.12_1540</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu-Pakete</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Installierte Ubuntu-Pakete, einschließlich `apt` für [CVE-2019-3462 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) und [USN-3863-1 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://usn.ubuntu.com/3863-1), wurden aktualisiert.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für 1.10.12_1540, veröffentlicht am 21. Januar 2019
{: #11012_1540}

In der folgenden Tabelle finden Sie die Änderungen, die in Patch 1.10.12_1540 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.10.11_1538 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.10.11_1538</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>Version 1.10.11-219</td>
<td>v1.10.12-252</td>
<td>Wurde für die Unterstützung von Kubernetes 1.10.12 aktualisiert.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>Version 1.10.11</td>
<td>v1.10.12</td>
<td>Lesen Sie die [Kubernetes-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.12).</td>
</tr>
<tr>
<td>Kubernetes Add-on Resizer</td>
<td>1.8.1</td>
<td>1.8.4</td>
<td>Lesen Sie die [Releaseinformationen zu Kubernetes Add-on Resizer ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/autoscaler/releases/tag/addon-resizer-1.8.4).</td>
</tr>
<tr>
<td>Kubernetes-Dashboard</td>
<td>Version 1.8.3</td>
<td>Version 1.10.1</td>
<td>Lesen Sie die [Releaseinformationen zum Kubernetes-Dashboard ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/dashboard/releases/tag/v1.10.1). Die Aktualisierung behebt [CVE-2018-18264 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-18264).<br><br>Wenn Sie über `kubectl proxy` auf das Dashboard zugreifen, wird die Schaltfläche zum Überspringen (**SKIP**) von der Anmeldeseite entfernt. [Verwenden Sie stattdessen ein **Token** für die Anmeldung](/docs/containers?topic=containers-app#cli_dashboard).</td>
</tr>
<tr>
<td>GPU-Installationsprogramm</td>
<td>390.12</td>
<td>410.79</td>
<td>Die installierten GPU-Treiber wurden auf 410.79 aktualisiert.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten-Fixpack 1.10.11_1538, veröffentlicht am 7. Januar 2019
{: #11011_1538}

In der folgenden Tabelle finden Sie die Änderungen, die im Workerknoten-Fixpack 1.10.11_1538 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.10.11_1537 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.10.11_1537</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-139</td>
<td>4.4.0-141</td>
<td>Aktualisierte Workerknoten-Images mit Kernelaktualisierung für [CVE-2017-5753, CVE-2018-18690 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-141.167/changelog).</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten-Fixpack 1.10.11_1537, veröffentlicht am 17. Dezember 2018
{: #11011_1537}

In der folgenden Tabelle finden Sie die Änderungen, die im Workerknoten-Fixpack 1.10.11_1537 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.10.11_1536 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.10.11_1536</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu-Pakete</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Aktualisierungen an installierten Ubuntu-Paketen.</td>
</tr>
</tbody>
</table>


### Änderungsprotokoll für 1.10.11_1536, veröffentlicht am 4. Dezember 2018
{: #11011_1536}

In der folgenden Tabelle finden Sie die Änderungen, die in Patch 1.10.11_1536 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.10.8_1532 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.10.8_1532</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>Version 3.2.1</td>
<td>Version 3.3.1</td>
<td>Lesen Sie die [Calico-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.projectcalico.org/v3.3/releases/#v331). Die Aktualisierung behebt [Tigera Technical Advisory TTA-2018-001 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.projectcalico.org/security-bulletins/).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>Version 1.10.8-197</td>
<td>Version 1.10.11-219</td>
<td>Wurde für die Unterstützung von Kubernetes 1.10.11 aktualisiert.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>Version 1.10.8</td>
<td>Version 1.10.11</td>
<td>Lesen Sie die [Kubernetes-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.11). Die Aktualisierung behebt [CVE-2018-1002105 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/kubernetes/issues/71411).</td>
</tr>
<tr>
<td>OpenVPN-Client und -Server</td>
<td>2.4.4-r1-6</td>
<td>2.4.6-r3-IKS-8</td>
<td>Das Image für [CVE-2018-0732 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732) und [CVE-2018-0737 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737) wurde aktualisiert.</td>
</tr>
<tr>
<td>Ressourcenauslastung von Workerknoten</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Es wurden dedizierte 'cgroups' für 'kubelet' und 'docker' hinzugefügt, damit diesen Komponenten nicht die Ressourcen ausgehen. Weitere Informationen finden Sie unter [Reserven für Workerknotenressourcen](/docs/containers?topic=containers-plan_clusters#resource_limit_node).</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten-Fixpack 1.10.8_1532, veröffentlicht am 27. November 2018
{: #1108_1532}

In der folgenden Tabelle finden Sie die Änderungen, die im Workerknoten-Fixpack 1.10.8_1532 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.10.8_1531 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.10.8_1531</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Docker</td>
<td>17.06.2</td>
<td>18.06.1</td>
<td>Lesen Sie die [Docker-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.docker.com/engine/release-notes/#18061-ce).</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten-Fixpack 1.10.8_1531, veröffentlicht am 19. November 2018
{: #1108_1531}

In der folgenden Tabelle finden Sie die Änderungen, die im Workerknoten-Fixpack 1.10.8_1531 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.10.8_1530 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.10.8_1530</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-137</td>
<td>4.4.0-139</td>
<td>Workerknoten-Images mit Kernelaktualisierung für [CVE-2018-7755 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-139.165/changelog) wurden aktualisiert.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für 1.10.8_1530, veröffentlicht am 7. November 2018
{: #1108_1530_ha-master}

In der folgenden Tabelle finden Sie die Änderungen, die in Patch 1.10.8_1530 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.10.8_1528 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.10.8_1528</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Cluster-Master</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Die Cluster-Master-Konfiguration wurde aktualisiert, um die Hochverfügbarkeit zu erhöhen. Cluster haben jetzt drei Kubernetes-Masterreplikate, die im Rahmen einer Hochverfügbarkeitskonfiguration eingerichtet werden, wobei jeder Master auf einem separaten physischen Host bereitgestellt wird. Und wenn sich Ihr Cluster in einer Zone mit mehreren Zonen befindet, werden die Master über Zonen verteilt.<br>Informationen zu den Aktionen, die Sie ausführen müssen, finden Sie unter [Aktualisierung auf hoch verfügbare Cluster-Master](/docs/containers?topic=containers-cs_versions#ha-masters). Diese Vorbereitungsaktionen finden Anwendung:<ul>
<li>Wenn Sie über eine Firewall oder über angepasste Calico-Netzrichtlinien verfügen.</li>
<li>Wenn Sie die Host-Ports `2040` oder `2041` auf Ihren Workerknoten verwenden.</li>
<li>Wenn Sie die IP-Adresse des Cluster-Masters für den clusterinternen Zugriff auf den Master verwendet haben.</li>
<li>Wenn eine Automatisierung vorhanden ist, die die Calico-API oder -CLI (`calicoctl`) aufruft, z. B. um Calico-Richtlinien zu erstellen.</li>
<li>Wenn Sie Kubernetes- oder Calico-Netzrichtlinien zum Steuern des Pod-Egress-Zugriffs auf den Master verwenden.</li></ul></td>
</tr>
<tr>
<td>Hochverfügbarkeitsproxy des Cluster-Masters</td>
<td>n.z.</td>
<td>1.8.12-alpine</td>
<td>Es wurde ein Pod `ibm-master-proxy-*` für den clientseitigen Lastausgleich auf allen Workerknoten hinzugefügt, sodass jeder Workerknotenclient Anforderungen an ein verfügbares Hochverfügbarkeitsmaster-Replikat weiterleiten kann.</td>
</tr>
<tr>
<td>etcd</td>
<td>Version 3.2.18</td>
<td>Version 3.3.1</td>
<td>Lesen Sie die [etcd-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/coreos/etcd/releases/v3.3.1).</td>
</tr>
<tr>
<td>Verschlüsseln von Daten in 'etcd'</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Bisher wurden 'etcd'-Daten in der NFS-Dateispeicherinstanz eines Masters gespeichert, in der ruhende Daten verschlüsselt werden. Jetzt werden 'etcd'-Daten auf der lokalen Platte des Masters gespeichert und in {{site.data.keyword.cos_full_notm}} gesichert. Daten werden während des Transits an {{site.data.keyword.cos_full_notm}} verschlüsselt und wenn sie ruhen. Die 'etcd'-Daten auf der lokalen Platte des Masters sind jedoch nicht verschlüsselt. Wenn die lokalen 'etcd'-Daten Ihres Masters verschlüsselt werden sollen, [aktivieren Sie {{site.data.keyword.keymanagementservicelong_notm}} in Ihrem Cluster](/docs/containers?topic=containers-encryption#keyprotect).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>Version 1.10.8-172</td>
<td>Version 1.10.8-197</td>
<td>Die Annotation `service.kubernetes.io/ibm-load-balancer-cloud-provider-vlan` wurde hinzugefügt, um das VLAN anzugeben, in dem der Lastausgleichsservice bereitgestellt ist. Um verfügbare VLANs in Ihrem Cluster anzuzeigen, führen Sie diesen Befehl aus: `ibmcloud ks vlans --zone <zone>`.</td>
</tr>
<tr>
<td>TPM-aktivierter Kernel</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Bare-Metal-Workerknoten mit TPM-Chips für Trusted Compute verwenden den Ubuntu-Standardkernel, bis die Vertrauensbeziehung eingerichtet ist. Wenn Sie die [Vertrauensbeziehung](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_feature_enable) in einem vorhandenen Cluster aktivieren, müssen Sie alle vorhandenen Bare-Metal-Workerknoten mit TPM-Chips [neu laden](/docs/containers?topic=containers-cs_cli_reference#cs_worker_reload). Um zu prüfen, ob ein Bare-Metal-Workerknoten über einen TPM-Chip verfügt, prüfen Sie das Feld **Trustable**, nachdem Sie den [Befehl](/docs/containers?topic=containers-cs_cli_reference#cs_machine_types) `ibmcloud ks machine-types --zone` ausgeführt haben.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten-Fixpack 1.10.8_1528, veröffentlicht am 26. Oktober 2018
{: #1108_1528}

In der folgenden Tabelle finden Sie die Änderungen, die im Workerknoten-Fixpack 1.10.8_1528 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.10.8_1527 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.10.8_1527</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Behandlung von Betriebssystem-Interrupts</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Der Systemdämon für Interruptanforderungen wurde durch einen leistungsfähigeren Interrupt-Handler ersetzt.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Master-Fixpack 1.10.8_1527, veröffentlicht am 15. Oktober 2018
{: #1108_1527}

In der folgenden Tabelle finden Sie die Änderungen, die im Master-Fixpack 1.10.8_1527 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.10.8_1524 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.10.8_1524</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico-Konfiguration</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Korrigiert Bereitschaftsprüfung für Container `calico-node` zur besseren Verarbeitung von Knotenfehlern.</td>
</tr>
<tr>
<td>Clusteraktualisierung</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Korrigiert Problem beim Aktualisieren von Cluster-Add-ons, wenn der Master von einer nicht unterstützten Version aktualisiert wird.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten-Fixpack 1.10.8_1525, veröffentlicht am 10. Oktober 2018
{: #1108_1525}

In der folgenden Tabelle finden Sie die Änderungen, die im Workerknoten-Fixpack 1.10.8_1525 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.10.8_1524 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.10.8_1524</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-133</td>
<td>4.4.0-137</td>
<td>Aktualisierte Workerknoten-Images mit Kernelaktualisierung für [CVE-2018-14633,CVE-2018-17182 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-137.163/changelog).</td>
</tr>
<tr>
<td>Inaktives Sitzungszeitlimit</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Legen Sie für das Zeitlimit für inaktive Sitzungen 5 Minuten fest, damit die Anforderungen erfüllt werden.</td>
</tr>
</tbody>
</table>


### Änderungsprotokoll für 1.10.8_1524, veröffentlicht am 2. Oktober 2018
{: #1108_1524}

In der folgenden Tabelle finden Sie die Änderungen, die in Patch 1.10.8_1524 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.10.7_1520 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.10.7_1520</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Key Management Service-Provider</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Die Möglichkeit zum Verwenden des Kubernetes-KMS-Providers (KMS - Key Management Service, Schlüsselmanagementservice) im Cluster wurde hinzugefügt, damit {{site.data.keyword.keymanagementservicefull}} unterstützt wird. Wenn Sie [{{site.data.keyword.keymanagementserviceshort}} im Cluster aktivieren](/docs/containers?topic=containers-encryption#keyprotect), werden alle geheimen Kubernetes-Schlüssel verschlüsselt.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>Version 1.10.7</td>
<td>Version 1.10.8</td>
<td>Lesen Sie die [Kubernetes-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.8).</td>
</tr>
<tr>
<td>Automatische Kubernetes DNS-Skalierung</td>
<td>1.1.2-r2</td>
<td>1.2.0</td>
<td>Lesen Sie die [Releaseinformationen zur automatischen Kubernetes-DNS-Skalierung ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes-incubator/cluster-proportional-autoscaler/releases/tag/1.2.0).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>Version 1.10.7-146</td>
<td>Version 1.10.8-172</td>
<td>Wurde für die Unterstützung von Kubernetes 1.10.8 aktualisiert. Der Dokumentationslink zu den Fehlernachrichten für die Lastausgleichsfunktion wurde auch aktualisiert.</td>
</tr>
<tr>
<td>IBM File Storage-Klassen</td>
<td>n.z.</td>
<td>n.z.</td>
<td>`mountOptions` wurde in den IBM File Storage-Klassen entfernt, damit der Standardwert verwendet wird, der vom Workerknoten bereitgestellt wird. Der doppelte Parameter `reclaimPolicy` wurde in den IBM File Storage-Klassen entfernt.<br><br>
Außerdem bleibt die IBM File Storage-Standardklasse beim Aktualisieren des Cluster-Masters unverändert. Falls Sie die Standardspeicherklasse ändern möchten, führen Sie `kubectl patch storageclass <storageclass> -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'` aus und ersetzen `<storageclass>` durch den Namen der Speicherklasse.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten-Fixpack 1.10.7_1521, veröffentlicht am 20. September 2018
{: #1107_1521}

In der folgenden Tabelle finden Sie die Änderungen, die im Workerknoten-Fixpack 1.10.7_1521 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.10.7_1520 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.10.7_1520</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Protokollrotation</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Von Zeitgebern des Typs `cronjobs` wurde zu `systemd` gewechselt, um ein Fehlschlagen von `logrotate` auf Workerknoten zu vermeiden, die nicht innerhalb von 90 Tagen erneute geladen oder aktualisiert werden. **Hinweis**: In allen früheren Versionen für dieses Unterrelease war die primäre Platte mit Daten gefüllt, nachdem der Cron-Job fehlgeschlagen war, weil die Protokolle nicht rotiert wurden. Der Cron-Job schlägt fehl, nachdem der Workerknoten 90 Tage aktiv ist, ohne erneut aktualisiert oder geladen zu werden. Wenn die gesamte Speicherkapazität der primären Platte durch Protokolle erschöpft ist, wechselt der Workerknoten in den Status 'Fehlgeschlagen'. Der Fehler auf dem Workerknoten kann mit dem [command](/docs/containers?topic=containers-cs_cli_reference#cs_worker_reload) `ibmcloud ks worker-reload` oder dem [Befehl](/docs/containers?topic=containers-cs_cli_reference#cs_worker_update) `ibmcloud ks worker-update` behoben werden.</td>
</tr>
<tr>
<td>Laufzeitkomponenten für Workerknoten (`kubelet`, `kube-proxy`, `docker`)</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Entfernte Abhängigkeiten der Laufzeitkomponenten auf der primären Platte. Diese Erweiterung verhindert das Fehlschlagen von Workerknoten, wenn die Speicherkapazität auf der primären Platte erschöpft ist.</td>
</tr>
<tr>
<td>Ablauf des Rootkennworts</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Rootkennwörter für Workerknoten laufen aus Konformitätsgründen nach 90 Tagen ab. Wenn für die Automatisierungstools eine Anmeldung am Workerknoten erforderlich ist oder wenn sie auf Cron-Jobs angewiesen sind, die als Root ausgeführt werden, können Sie den Ablauf der Kennwortgültigkeit inaktivieren, indem Sie sich am Workerknoten anmelden und `chage -M -1 root` ausführen. **Hinweis**: Falls Konformitätsanforderungen für die Sicherheit gelten, die eine Ausführung als Root oder eine Entfernung des Ablaufs der Kennwortgültigkeit verhindert, inaktivieren Sie nicht das Ablaufdatum. Stattdessen können Sie die Workerknoten mindestens alle 90 Tage [aktualisieren](/docs/containers?topic=containers-cs_cli_reference#cs_worker_update) oder [erneut laden](/docs/containers?topic=containers-cs_cli_reference#cs_worker_reload).</td>
</tr>
<tr>
<td>systemd</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Bereinigen Sie die temporären Mounteinheiten in regelmäßigen Abständen, um zu verhindern, dass sie unbegrenzt werden. Diese Aktion ist für [Kubernetes-Fehler 57345 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/kubernetes/issues/57345) vorgesehen.</td>
</tr>
<tr>
<td>Docker</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Inaktiviert die Docker-Standard-Bridge, sodass der IP-Bereich `172.17.0.0/ 16` jetzt für private Routen verwendet wird. Wenn die Docker-Container in Workerknoten erstellt werden, weil `docker`-Befehle direkt auf dem Host ausgeführt werden oder weil ein Pod verwendet wird, von dem der Docker-Socket angehängt wird, wählen Sie eine der folgenden Optionen aus.<ul><li>Wenn Sie die externe Netzkonnektivität sicherstellen möchten, wenn Sie den Container erstellen, führen Sie `docker build --network host` aus.</li>
<li>Wenn Sie explizit ein Netz erstellen möchten, das beim Erstellen des Containers verwendet werden soll, führen Sie `docker network create` aus, und verwenden Sie anschließend dieses Netz.</li></ul>
**Hinweis**: Sind Abhängigkeiten vom Docker-Socket oder direkt von Docker vorhanden? [Aktualisieren Sie auf `containerd` anstatt auf `docker` als Containerlaufzeit](/docs/containers?topic=containers-cs_versions#containerd), sodass die Cluster auf die Ausführung von Kubernetes Version 1.11 oder eine aktuellere Version vorbereitet sind.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für 1.10.7_1520, veröffentlicht am 4. September 2018
{: #1107_1520}

In der folgenden Tabelle finden Sie die Änderungen, die in Patch 1.10.7_1520 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.10.5_1519 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.10.5_1519</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>Version 3.1.3</td>
<td>Version 3.2.1</td>
<td>Lesen Sie die Calico-[Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.projectcalico.org/v3.2/releases/#v321).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>Version 1.10.5-118</td>
<td>Version 1.10.7-146</td>
<td>Wurde für die Unterstützung von Kubernetes 1.10.7 aktualisiert. Darüber hinaus wurde die Cloud-Provider-Konfiguration geändert, um die Verarbeitung von Aktualisierungen der Services für die Lastausgleichsfunktion zu verbessern, wenn für `externalTrafficPolicy` der Wert `local` eingestellt ist.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage-Plug-in</td>
<td>334</td>
<td>338</td>
<td>Aktualisierung des Inkubators auf Version 1.8. Der Dateispeicher wird für die von Ihnen ausgewählte Zone bereitgestellt. Sie können die Bezeichnungen einer vorhandenen (statischen) PV-Instanz nicht aktualisieren, es sei denn, Sie verwenden einen Mehrzonencluster und müssen die Regions- und Zonenbezeichnungen hinzufügen.<br><br> Die NFS-Standardversion wurde aus den Mountoptionen in den von IBM bereitgestellten Dateispeicherklassen entfernt. Das Betriebssystem des Hosts vereinbart die NFS-Version jetzt mit dem NFS-Server der IBM Cloud-Infrastruktur (SoftLayer). Wenn Sie eine bestimmte NFS-Version manuell festlegen oder die NFS-Version Ihres PV ändern, die vom Betriebssystem des Hosts vereinbart wurde, finden Sie Informationen hierzu im Abschnitt [NFS-Standardversion ändern](/docs/containers?topic=containers-file_storage#nfs_version_class).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>Version 1.10.5</td>
<td>Version 1.10.7</td>
<td>Lesen Sie die Kubernetes-[Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.7).</td>
</tr>
<tr>
<td>Konfiguration von Kubernetes Heapster</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Höhere Ressourcengrenzen für den Container `heapster-nanny`.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten-Fixpack 1.10.5_1519, veröffentlicht am 23. August 2018
{: #1105_1519}

In der folgenden Tabelle finden Sie die Änderungen, die im Workerknoten-Fixpack 1.10.5_1519 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.10.5_1518 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.10.5_1518</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>`systemd`</td>
<td>229</td>
<td>230</td>
<td>Aktualisierung von `systemd` zur Korrektur des `cgroup`-Lecks.</td>
</tr>
<tr>
<td>Kernel</td>
<td>4.4.0-127</td>
<td>4.4.0-133</td>
<td>Aktualisierte Workerknoten-Images mit Kernelaktualisierung für [CVE-2018-3620,CVE-2018-3646 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://usn.ubuntu.com/3741-1/).</td>
</tr>
</tbody>
</table>


### Änderungsprotokoll für Workerknoten-Fixpack 1.10.5_1518, veröffentlicht am 13. August 2018
{: #1105_1518}

In der folgenden Tabelle finden Sie die Änderungen, die im Workerknoten-Fixpack 1.10.5_1518 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.10.5_1517 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.10.5_1517</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu-Pakete</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Aktualisierungen an installierten Ubuntu-Paketen.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für 1.10.5_1517, veröffentlicht am 27. Juli 2018
{: #1105_1517}

In der folgenden Tabelle finden Sie die Änderungen, die in Patch 1.10.5_1517 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.10.3_1514 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.10.3_1514</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>Version 3.1.1</td>
<td>Version 3.1.3</td>
<td>Lesen Sie die Calico-[Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.projectcalico.org/v3.1/releases/#v313).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>Version 1.10.3-85</td>
<td>Version 1.10.5-118</td>
<td>Wurde für die Unterstützung von Kubernetes 1.10.5 aktualisiert. Darüber hinaus enthalten `create failure`-Ereignisse des LoadBalancer-Service nun alle Fehler portierbarer Teilnetze.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage-Plug-in</td>
<td>320</td>
<td>334</td>
<td>Das Zeitlimit bei der Erstellung persistenter Datenträger wurde von 15 auf 30 Minuten erhöht. Der Standardabrechnungstyp wurde in 'Stündlich' (`hourly`) geändert. Es wurden Mountoptionen zu den vordefinierten Speicherklassen hinzugefügt. In der NFS-Dateispeicherinstanz in Ihrem Konto der IBM Cloud-Infrastruktur (SoftLayer) wurde das Format des Felds **Anmerkungen** in JSON geändert und der Kubernetes-Namensbereich hinzugefügt, in dem der persistente Datenträger bereitgestellt ist. Für die Unterstützung von Mehrzonenclustern wurden persistenten Datenträgern Zonen- und Regionsbezeichnungen hinzugefügt.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>Version 1.10.3</td>
<td>Version 1.10.5</td>
<td>Lesen Sie die Kubernetes-[Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.5).</td>
</tr>
<tr>
<td>Kernel</td>
<td>n.z.</td>
<td>n.z.</td>
<td>An den Netzeinstellungen für Workerknoten wurden kleinere Verbesserungen vorgenommen, um Netzarbeitslasten mit hoher Leistung zu unterstützen.</td>
</tr>
<tr>
<td>OpenVPN-Client</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Die `vpn`-Bereitstellung des OpenVPN-Clients, die im Namensbereich `kube-system` ausgeführt wird, wird nun von `addon-manager` von Kubernetes verwaltet.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten-Fixpack 1.10.3_1514, veröffentlicht 3. Juli 2018
{: #1103_1514}

In der folgenden Tabelle finden Sie die Änderungen, die im Workerknoten-Fixpack 1.10.3_1514 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.10.3_1513 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.10.3_1513</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Optimiertes `sysctl` für Netzauslastungen mit hoher Leistung.</td>
</tr>
</tbody>
</table>


### Änderungsprotokoll für Workerknoten-Fixpack 1.10.3_1513, veröffentlicht 21. Juni 2018
{: #1103_1513}

In der folgenden Tabelle finden Sie die Änderungen, die im Workerknoten-Fixpack 1.10.3_1513 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.10.3_1512 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.10.3_1512</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Docker</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Bei nicht verschlüsselten Maschinentypen wird die sekundäre Platte bereinigt, indem Sie beim erneuten Laden oder Aktualisieren des Workerknotens ein neues Dateisystem abrufen.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für 1.10.3_1512, veröffentlicht 12. Juni 2018
{: #1103_1512}

In der folgenden Tabelle finden Sie die Änderungen, die in Patch 1.10.3_1512 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.10.1_1510 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.10.1_1510</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>Version 1.10.1</td>
<td>Version 1.10.3</td>
<td>Lesen Sie die Kubernetes-[Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.3).</td>
</tr>
<tr>
<td>Kubernetes-Konfiguration</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Der Option `--enable-admission-plugins` wurde `PodSecurityPolicy` für den Kubernetes-API-Server des Clusters hinzugefügt und der Cluster wurde für die Unterstützung von Sicherheitsrichtlinien für Pods konfiguriert. Weitere Informationen finden Sie unter [Sicherheitsrichtlinien für Pods konfigurieren](/docs/containers?topic=containers-psp).</td>
</tr>
<tr>
<td>Kubelet-Konfiguration</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Es wurde die Option `--authentication-token-webhook` aktiviert, um das Trägertoken und das Servicekontotoken der API zur Authentifizierung am HTTPS-Endpunkt `kubelet` zu unterstützen.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>Version 1.10.1-52</td>
<td>Version 1.10.3-85</td>
<td>Wurde für die Unterstützung von Kubernetes 1.10.3 aktualisiert.</td>
</tr>
<tr>
<td>OpenVPN-Client</td>
<td>n.z.</td>
<td>n.z.</td>
<td>`livenessProbe` wurde der `vpn`-Bereitstellung des OpenVPN-Clients hinzugefügt, die im Namensbereich `kube-system` ausgeführt wird.</td>
</tr>
<tr>
<td>Kernelaktualisierung</td>
<td>4.4.0-116</td>
<td>4.4.0-127</td>
<td>Neue Workerknoten-Images mit Kernelaktualisierung für [CVE-2018-3639 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-3639).</td>
</tr>
</tbody>
</table>



### Änderungsprotokoll für Workerknoten-Fixpack 1.10.1_1510, veröffentlicht am 18. Mai 2018
{: #1101_1510}

In der folgenden Tabelle finden Sie die Änderungen, die im Workerknoten-Fixpack 1.10.1_1510 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.10.1_1509 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.10.1_1509</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Fix für einen Programmfehler, der bei der Verwendung des Blockspeicher-Plug-ins auftrat.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten Fixpack 1.10.1_1509, veröffentlicht am 16. Mai 2018
{: #1101_1509}

In der folgenden Tabelle finden Sie die Änderungen, die im Workerknoten-Fixpack 1.10.1_1509 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.10.1_1508 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.10.1_1508</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Die im Stammverzeichnis `kubelet` gespeicherten Daten werden nun auf einer größeren sekundären Platte der Maschine mit dem Workerknoten gespeichert.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für 1.10.1_1508, veröffentlicht am 1. Mai 2018
{: #1101_1508}

In der folgenden Tabelle finden Sie die Änderungen, die in Patch 1.10.1_1508 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.9.7_1510 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.9.7_1510</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>Version 2.6.5</td>
<td>Version 3.1.1</td>
<td>Lesen Sie die Calico-[Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.projectcalico.org/v3.1/releases/#v311).</td>
</tr>
<tr>
<td>Kubernetes Heapster</td>
<td>Version 1.5.0</td>
<td>Version 1.5.2</td>
<td>Lesen Sie die Kubernetes Heapster-[Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/heapster/releases/tag/v1.5.2).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>Version 1.9.7</td>
<td>Version 1.10.1</td>
<td>Lesen Sie die Kubernetes-[Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.1).</td>
</tr>
<tr>
<td>Kubernetes-Konfiguration</td>
<td>n.z.</td>
<td>n.z.</td>
<td><code>StorageObjectInUseProtection</code> wurde zur Option <code>--enable-admission-plugins</code> für den Kubernetes-API-Server des Clusters hinzugefügt.</td>
</tr>
<tr>
<td>Kubernetes-DNS</td>
<td>1.14.8</td>
<td>1.14.10</td>
<td>Lesen Sie die Kubernetes-DNS-[Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/dns/releases/tag/1.14.10).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>Version 1.9.7-102</td>
<td>Version 1.10.1-52</td>
<td>Wurde für die Unterstützung von Kubernetes 1.10 aktualisiert.</td>
</tr>
<tr>
<td>GPU-Unterstützung</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Unterstützung für [Container-Workloads der Graphics Processing Unit (GPU)](/docs/containers?topic=containers-app#gpu_app) ist jetzt für die Terminierung und Ausführung verfügbar. Eine Liste der verfügbaren GPU-Maschinentypen finden Sie unter [Hardware für Workerknoten](/docs/containers?topic=containers-plan_clusters#shared_dedicated_node). Weitere Informationen finden Sie in der Kubernetes-Dokumentation zum [Terminieren von GPUs ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/tasks/manage-gpus/scheduling-gpus/).</td>
</tr>
</tbody>
</table>

<br />


## Archiv
{: #changelog_archive}

Nicht unterstützte Kubernetes-Versionen:
*  [Version 1.9](#19_changelog)
*  [Version 1.8](#18_changelog)
*  [Version 1.7](#17_changelog)

### Änderungsprotokoll der Version 1.9 (nicht mehr unterstützt seit 27. Dezember 2018)
{: #19_changelog}

Überprüfen Sie das Änderungsprotokoll der Version 1.9.

### Änderungsprotokoll für Workerknoten-Fixpack 1.9.11_1539, veröffentlicht am 17. Dezember 2018
{: #1911_1539}

In der folgenden Tabelle finden Sie die Änderungen, die im Workerknoten-Fixpack 1.9.11_1539 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.9.11_1538 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.9.11_1538</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu-Pakete</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Aktualisierungen an installierten Ubuntu-Paketen.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten-Fixpack 1.9.11_1538, veröffentlicht am 4. Dezember 2018
{: #1911_1538}

In der folgenden Tabelle finden Sie die Änderungen, die im Workerknoten-Fixpack 1.9.11_1538 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.9.11_1537 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.9.11_1537</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ressourcenauslastung von Workerknoten</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Es wurden dedizierte 'cgroups' für 'kubelet' und 'docker' hinzugefügt, damit diesen Komponenten nicht die Ressourcen ausgehen. Weitere Informationen finden Sie unter [Reserven für Workerknotenressourcen](/docs/containers?topic=containers-plan_clusters#resource_limit_node).</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten-Fixpack 1.9.11_1537, veröffentlicht am 27. November 2018
{: #1911_1537}

In der folgenden Tabelle finden Sie die Änderungen, die im Workerknoten-Fixpack 1.9.11_1537 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.9.11_1536 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.9.11_1536</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Docker</td>
<td>17.06.2</td>
<td>18.06.1</td>
<td>Lesen Sie die [Docker-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.docker.com/engine/release-notes/#18061-ce).</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für 1.9.11_1536, veröffentlicht am 19. November 2018
{: #1911_1536}

In der folgenden Tabelle finden Sie die Änderungen, die in Patch 1.9.11_1536 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.9.10_1532 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.9.10_1532</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>Version 2.6.5</td>
<td>Version 2.6.12</td>
<td>Lesen Sie die [Calico-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.projectcalico.org/v2.6/releases/#v2612). Die Aktualisierung behebt [Tigera Technical Advisory TTA-2018-001 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.projectcalico.org/security-bulletins/).</td>
</tr>
<tr>
<td>Kernel</td>
<td>4.4.0-137</td>
<td>4.4.0-139</td>
<td>Workerknoten-Images mit Kernelaktualisierung für [CVE-2018-7755 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-139.165/changelog) wurden aktualisiert.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>Version 1.9.10</td>
<td>Version 1.9.11</td>
<td>Lesen Sie die [Kubernetes-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.11).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}}</td>
<td>Version 1.9.10-219</td>
<td>Version 1.9.11-249</td>
<td>Wurde für die Unterstützung von Kubernetes 1.9.11 aktualisiert.</td>
</tr>
<tr>
<td>OpenVPN-Client und -Server</td>
<td>2.4.4-r2</td>
<td>2.4.6-r3-IKS-8</td>
<td>Das Image für [CVE-2018-0732 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732) und [CVE-2018-0737 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737) wurde aktualisiert.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten-Fix 1.9.10_1532, veröffentlicht am 7. November 2018
{: #1910_1532}

In der folgenden Tabelle finden Sie die Änderungen, die im Workerknoten-Fixpack 1.9.11_1532 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.9.10_1531 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.9.10_1531</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>TPM-aktivierter Kernel</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Bare-Metal-Workerknoten mit TPM-Chips für Trusted Compute verwenden den Ubuntu-Standardkernel, bis die Vertrauensbeziehung eingerichtet ist. Wenn Sie die [Vertrauensbeziehung](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_feature_enable) in einem vorhandenen Cluster aktivieren, müssen Sie alle vorhandenen Bare-Metal-Workerknoten mit TPM-Chips [neu laden](/docs/containers?topic=containers-cs_cli_reference#cs_worker_reload). Um zu prüfen, ob ein Bare-Metal-Workerknoten über einen TPM-Chip verfügt, prüfen Sie das Feld **Trustable**, nachdem Sie den [Befehl](/docs/containers?topic=containers-cs_cli_reference#cs_machine_types) `ibmcloud ks machine-types --zone` ausgeführt haben.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten-Fixpack 1.9.10_1531, veröffentlicht am 26. Oktober 2018
{: #1910_1531}

In der folgenden Tabelle finden Sie die Änderungen, die im Workerknoten-Fixpack 1.9.10_1531 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.9.10_1530 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.9.10_1530</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Behandlung von Betriebssystem-Interrupts</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Der Systemdämon für Interruptanforderungen wurde durch einen leistungsfähigeren Interrupt-Handler ersetzt.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Master-Fixpack 1.9.10_1530, veröffentlicht am 15. Oktober 2018
{: #1910_1530}

In der folgenden Tabelle finden Sie die Änderungen, die im Workerknoten-Fixpack 1.9.10_1530 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.9.10_1527 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.9.10_1527</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Clusteraktualisierung</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Korrigiert Problem beim Aktualisieren von Cluster-Add-ons, wenn der Master von einer nicht unterstützten Version aktualisiert wird.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten-Fixpack 1.9.10_1528, veröffentlicht am 10. Oktober 2018
{: #1910_1528}

In der folgenden Tabelle finden Sie die Änderungen, die im Workerknoten-Fixpack 1.9.10_1528 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.9.10_1527 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.9.10_1527</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-133</td>
<td>4.4.0-137</td>
<td>Aktualisierte Workerknoten-Images mit Kernelaktualisierung für [CVE-2018-14633,CVE-2018-17182 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-137.163/changelog).</td>
</tr>
<tr>
<td>Inaktives Sitzungszeitlimit</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Legen Sie für das Zeitlimit für inaktive Sitzungen 5 Minuten fest, damit die Anforderungen erfüllt werden.</td>
</tr>
</tbody>
</table>


### Änderungsprotokoll für 1.9.10_1527, veröffentlicht am 2. Oktober 2018
{: #1910_1527}

In der folgenden Tabelle finden Sie die Änderungen, die in Patch 1.9.10_1527 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.9.10_1523 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.9.10_1523</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>Version 1.9.10-192</td>
<td>Version 1.9.10-219</td>
<td>Der Dokumentationslink zu den Fehlernachrichten für die Lastausgleichsfunktion wurde aktualisiert.</td>
</tr>
<tr>
<td>IBM File Storage-Klassen</td>
<td>n.z.</td>
<td>n.z.</td>
<td>`mountOptions` wurde in den IBM File Storage-Klassen entfernt, damit der Standardwert verwendet wird, der vom Workerknoten bereitgestellt wird. Der doppelte Parameter `reclaimPolicy` wurde in den IBM File Storage-Klassen entfernt.<br><br>
Außerdem bleibt die IBM File Storage-Standardklasse beim Aktualisieren des Cluster-Masters unverändert. Falls Sie die Standardspeicherklasse ändern möchten, führen Sie `kubectl patch storageclass <storageclass> -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'` aus und ersetzen `<storageclass>` durch den Namen der Speicherklasse.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten-Fixpack 1.9.10_1524, veröffentlicht am 20. September 2018
{: #1910_1524}

In der folgenden Tabelle finden Sie die Änderungen, die im Workerknoten-Fixpack 1.9.10_1524 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.9.10_1523 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.9.10_1523</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Protokollrotation</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Von Zeitgebern des Typs `cronjobs` wurde zu `systemd` gewechselt, um ein Fehlschlagen von `logrotate` auf Workerknoten zu vermeiden, die nicht innerhalb von 90 Tagen erneute geladen oder aktualisiert werden. **Hinweis**: In allen früheren Versionen für dieses Unterrelease war die primäre Platte mit Daten gefüllt, nachdem der Cron-Job fehlgeschlagen war, weil die Protokolle nicht rotiert wurden. Der Cron-Job schlägt fehl, nachdem der Workerknoten 90 Tage aktiv ist, ohne erneut aktualisiert oder geladen zu werden. Wenn die gesamte Speicherkapazität der primären Platte durch Protokolle erschöpft ist, wechselt der Workerknoten in den Status 'Fehlgeschlagen'. Der Fehler auf dem Workerknoten kann mit dem [command](/docs/containers?topic=containers-cs_cli_reference#cs_worker_reload) `ibmcloud ks worker-reload` oder dem [Befehl](/docs/containers?topic=containers-cs_cli_reference#cs_worker_update) `ibmcloud ks worker-update` behoben werden.</td>
</tr>
<tr>
<td>Laufzeitkomponenten für Workerknoten (`kubelet`, `kube-proxy`, `docker`)</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Entfernte Abhängigkeiten der Laufzeitkomponenten auf der primären Platte. Diese Erweiterung verhindert das Fehlschlagen von Workerknoten, wenn die Speicherkapazität auf der primären Platte erschöpft ist.</td>
</tr>
<tr>
<td>Ablauf des Rootkennworts</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Rootkennwörter für Workerknoten laufen aus Konformitätsgründen nach 90 Tagen ab. Wenn für die Automatisierungstools eine Anmeldung am Workerknoten erforderlich ist oder wenn sie auf Cron-Jobs angewiesen sind, die als Root ausgeführt werden, können Sie den Ablauf der Kennwortgültigkeit inaktivieren, indem Sie sich am Workerknoten anmelden und `chage -M -1 root` ausführen. **Hinweis**: Falls Konformitätsanforderungen für die Sicherheit gelten, die eine Ausführung als Root oder eine Entfernung des Ablaufs der Kennwortgültigkeit verhindert, inaktivieren Sie nicht das Ablaufdatum. Stattdessen können Sie die Workerknoten mindestens alle 90 Tage [aktualisieren](/docs/containers?topic=containers-cs_cli_reference#cs_worker_update) oder [erneut laden](/docs/containers?topic=containers-cs_cli_reference#cs_worker_reload).</td>
</tr>
<tr>
<td>systemd</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Bereinigen Sie die temporären Mounteinheiten in regelmäßigen Abständen, um zu verhindern, dass sie unbegrenzt werden. Diese Aktion ist für [Kubernetes-Fehler 57345 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/kubernetes/issues/57345) vorgesehen.</td>
</tr>
<tr>
<td>Docker</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Inaktiviert die Docker-Standard-Bridge, sodass der IP-Bereich `172.17.0.0/ 16` jetzt für private Routen verwendet wird. Wenn die Docker-Container in Workerknoten erstellt werden, weil `docker`-Befehle direkt auf dem Host ausgeführt werden oder weil ein Pod verwendet wird, von dem der Docker-Socket angehängt wird, wählen Sie eine der folgenden Optionen aus.<ul><li>Wenn Sie die externe Netzkonnektivität sicherstellen möchten, wenn Sie den Container erstellen, führen Sie `docker build --network host` aus.</li>
<li>Wenn Sie explizit ein Netz erstellen möchten, das beim Erstellen des Containers verwendet werden soll, führen Sie `docker network create` aus, und verwenden Sie anschließend dieses Netz.</li></ul>
**Hinweis**: Sind Abhängigkeiten vom Docker-Socket oder direkt von Docker vorhanden? [Aktualisieren Sie auf `containerd` anstatt auf `docker` als Containerlaufzeit](/docs/containers?topic=containers-cs_versions#containerd), sodass die Cluster auf die Ausführung von Kubernetes Version 1.11 oder eine aktuellere Version vorbereitet sind.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für 1.9.10_1523, veröffentlicht am 4. September 2018
{: #1910_1523}

In der folgenden Tabelle finden Sie die Änderungen, die in Patch 1.9.10_1523 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.9.9_1522 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.9.9_1522</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>Version 1.9.9-167</td>
<td>Version 1.9.10-192</td>
<td>Wurde für die Unterstützung von Kubernetes 1.9.10 aktualisiert. Darüber hinaus wurde die Cloud-Provider-Konfiguration geändert, um die Verarbeitung von Aktualisierungen der Services für die Lastausgleichsfunktion zu verbessern, wenn für `externalTrafficPolicy` der Wert `local` eingestellt ist.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage-Plug-in</td>
<td>334</td>
<td>338</td>
<td>Aktualisierung des Inkubators auf Version 1.8. Der Dateispeicher wird für die von Ihnen ausgewählte Zone bereitgestellt. Sie können die Bezeichnungen einer vorhandenen (statischen) PV-Instanz nicht aktualisieren, es sei denn, Sie verwenden einen Mehrzonencluster und müssen die Regions- und Zonenbezeichnungen hinzufügen.<br><br>Die NFS-Standardversion wurde aus den Mountoptionen in den von IBM bereitgestellten Dateispeicherklassen entfernt. Das Betriebssystem des Hosts vereinbart die NFS-Version jetzt mit dem NFS-Server der IBM Cloud-Infrastruktur (SoftLayer). Wenn Sie eine bestimmte NFS-Version manuell festlegen oder die NFS-Version Ihres PV ändern, die vom Betriebssystem des Hosts vereinbart wurde, finden Sie Informationen hierzu im Abschnitt [NFS-Standardversion ändern](/docs/containers?topic=containers-file_storage#nfs_version_class).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>Version 1.9.9</td>
<td>Version 1.9.10</td>
<td>Lesen Sie die Kubernetes-[Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.10).</td>
</tr>
<tr>
<td>Konfiguration von Kubernetes Heapster</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Höhere Ressourcengrenzen für den Container `heapster-nanny`.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten-Fixpack 1.9.9_1522, veröffentlicht am 23. August 2018
{: #199_1522}

In der folgenden Tabelle finden Sie die Änderungen, die im Workerknoten-Fixpack 1.9.9_1522 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.9.9_1521 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.9.9_1521</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>`systemd`</td>
<td>229</td>
<td>230</td>
<td>Aktualisierung von `systemd` zur Korrektur des `cgroup`-Lecks.</td>
</tr>
<tr>
<td>Kernel</td>
<td>4.4.0-127</td>
<td>4.4.0-133</td>
<td>Aktualisierte Workerknoten-Images mit Kernelaktualisierung für [CVE-2018-3620,CVE-2018-3646 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://usn.ubuntu.com/3741-1/).</td>
</tr>
</tbody>
</table>


### Änderungsprotokoll für Workerknoten-Fixpack 1.9.9_1521, veröffentlicht am 13. August 2018
{: #199_1521}

In der folgenden Tabelle finden Sie die Änderungen, die im Workerknoten-Fixpack 1.9.9_1521 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.9.9_1520 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.9.9_1520</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu-Pakete</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Aktualisierungen an installierten Ubuntu-Paketen.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für 1.9.9_1520, veröffentlicht 27. Juli 2018
{: #199_1520}

In der folgenden Tabelle finden Sie die Änderungen, die in Patch 1.9.9_1520 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.9.8_1517 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.9.8_1517</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>Version 1.9.8-141</td>
<td>Version 1.9.9-167</td>
<td>Wurde für die Unterstützung von Kubernetes 1.9.9 aktualisiert. Darüber hinaus enthalten `create failure`-Ereignisse des LoadBalancer-Service nun alle Fehler portierbarer Teilnetze.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage-Plug-in</td>
<td>320</td>
<td>334</td>
<td>Das Zeitlimit bei der Erstellung persistenter Datenträger wurde von 15 auf 30 Minuten erhöht. Der Standardabrechnungstyp wurde in 'Stündlich' (`hourly`) geändert. Es wurden Mountoptionen zu den vordefinierten Speicherklassen hinzugefügt. In der NFS-Dateispeicherinstanz in Ihrem Konto der IBM Cloud-Infrastruktur (SoftLayer) wurde das Format des Felds **Anmerkungen** in JSON geändert und der Kubernetes-Namensbereich hinzugefügt, in dem der persistente Datenträger bereitgestellt ist. Für die Unterstützung von Mehrzonenclustern wurden persistenten Datenträgern Zonen- und Regionsbezeichnungen hinzugefügt.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>Version 1.9.8</td>
<td>Version 1.9.9</td>
<td>Lesen Sie die Kubernetes-[Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.9).</td>
</tr>
<tr>
<td>Kernel</td>
<td>n.z.</td>
<td>n.z.</td>
<td>An den Netzeinstellungen für Workerknoten wurden kleinere Verbesserungen vorgenommen, um Netzarbeitslasten mit hoher Leistung zu unterstützen.</td>
</tr>
<tr>
<td>OpenVPN-Client</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Die `vpn`-Bereitstellung des OpenVPN-Clients, die im Namensbereich `kube-system` ausgeführt wird, wird nun von `addon-manager` von Kubernetes verwaltet.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten-Fixpack 1.9.8_1517, veröffentlicht am 3. Juli 2018
{: #198_1517}

In der folgenden Tabelle finden Sie die Änderungen, die im Workerknoten-Fixpack 1.9.8_1517 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.9.8_1516 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.9.8_1516</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Optimiertes `sysctl` für Netzauslastungen mit hoher Leistung.</td>
</tr>
</tbody>
</table>


### Änderungsprotokoll für Workerknoten-Fixpack 1.9.8_1516, veröffentlicht am 21. Juni 2018
{: #198_1516}

In der folgenden Tabelle finden Sie die Änderungen, die im Workerknoten-Fixpack 1.9.8_1516 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.9.8_1515 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.9.8_1515</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Docker</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Bei nicht verschlüsselten Maschinentypen wird die sekundäre Platte bereinigt, indem Sie beim erneuten Laden oder Aktualisieren des Workerknotens ein neues Dateisystem abrufen.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für 1.9.8_1515, veröffentlicht 19. Juni 2018
{: #198_1515}

In der folgenden Tabelle finden Sie die Änderungen, die in Patch 1.9.8_1515 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.9.7_1513 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.9.7_1513</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>Version 1.9.7</td>
<td>Version 1.9.8</td>
<td>Lesen Sie die [Kubernetes-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.8).</td>
</tr>
<tr>
<td>Kubernetes-Konfiguration</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Der Option `--admission-control` wurde `PodSecurityPolicy` für den Kubernetes-API-Server des Clusters hinzugefügt und der Cluster wurde für die Unterstützung von Sicherheitsrichtlinien für Pods konfiguriert. Weitere Informationen finden Sie unter [Sicherheitsrichtlinien für Pods konfigurieren](/docs/containers?topic=containers-psp).</td>
</tr>
<tr>
<td>IBM Cloud-Provider</td>
<td>Version 1.9.7-102</td>
<td>Version 1.9.8-141</td>
<td>Wurde für die Unterstützung von Kubernetes 1.9.8 aktualisiert.</td>
</tr>
<tr>
<td>OpenVPN-Client</td>
<td>n.z.</td>
<td>n.z.</td>
<td><code>livenessProbe</code> wurde der <code>vpn</code>-Bereitstellung des OpenVPN-Clients hinzugefügt, die im Namensbereich <code>kube-system</code> ausgeführt wird.</td>
</tr>
</tbody>
</table>


### Änderungsprotokoll für Workerknoten-Fixpack 1.9.7_1513, veröffentlicht am 11. Juni 2018
{: #197_1513}

In der folgenden Tabelle finden Sie die Änderungen, die im Workerknoten-Fixpack 1.9.7_1513 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.9.7_1512 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.9.7_1512</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernelaktualisierung</td>
<td>4.4.0-116</td>
<td>4.4.0-127</td>
<td>Neue Workerknoten-Images mit Kernelaktualisierung für [CVE-2018-3639 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-3639).</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten-Fixpack 1.9.7_1512, veröffentlicht am 18. Mai 2018
{: #197_1512}

In der folgenden Tabelle finden Sie die Änderungen, die im Workerknoten-Fixpack 1.9.7_1512 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.9.7_1511 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.9.7_1511</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Fix für einen Programmfehler, der bei der Verwendung des Blockspeicher-Plug-ins auftrat.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten Fixpack 1.9.7_1511, veröffentlicht am 16. Mai 2018
{: #197_1511}

In der folgenden Tabelle finden Sie die Änderungen, die im Workerknoten-Fixpack 1.9.7_1511 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.9.7_1510 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.9.7_1510</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Die im Stammverzeichnis `kubelet` gespeicherten Daten werden nun auf einer größeren sekundären Platte der Maschine mit dem Workerknoten gespeichert.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für 1.9.7_1510, veröffentlicht am 30. April 2018
{: #197_1510}

In der folgenden Tabelle finden Sie die Änderungen, die in Patch 1.9.7_1510 enthalten sind.
{: shortdesc}

<table summary="Seit Version 1.9.3_1506 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.9.3_1506</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>Version 1.9.3</td>
<td>Version 1.9.7	</td>
<td><p>Lesen Sie die [Kubernetes-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.7). Dieses Release befasst sich mit den Schwachstellen [CVE-2017-1002101 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002101) und [CVE-2017-1002102 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002102).</p><p><strong>Hinweis</strong>: `secret`, `configMap`, `downwardAPI` und projizierte Datenträger werden nun als schreibgeschützte Datenträger angehängt. Bisher konnten Apps Daten in diese Datenträger schreiben, aber das System konnte die Daten möglicherweise automatisch zurücksetzen. Wenn Ihre Apps das bisherige unsichere Verhalten erwarten, ändern Sie sie entsprechend.</p></td>
</tr>
<tr>
<td>Kubernetes-Konfiguration</td>
<td>n.z.</td>
<td>n.z.</td>
<td>`admissionregistration.k8s.io/v1alpha1=true` wurde zur Option `--runtime-config` für den Kubernetes-API-Server des Clusters hinzugefügt.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>Version 1.9.3-71</td>
<td>Version 1.9.7-102</td>
<td>`NodePort`- und `LoadBalancer`-Services unterstützen jetzt [das Beibehalten der Client-Quellen-IP](/docs/containers?topic=containers-loadbalancer#node_affinity_tolerations) durch Festlegen von `service.spec.externalTrafficPolicy` auf `Local`.</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>Korrigiert die [Edge-Knoten](/docs/containers?topic=containers-edge#edge)-Tolerierungskonfiguration für ältere Cluster.</td>
</tr>
</tbody>
</table>

<br />


### Änderungsprotokoll Version 1.8 (nicht unterstützt)
{: #18_changelog}

Überprüfen Sie die folgenden Änderungen.

### Änderungsprotokoll für Workerknoten-Fixpack 1.8.15_1521, veröffentlicht am 20. September 2018
{: #1815_1521}

<table summary="Seit Version 1.8.15_1520 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.8.15_1520</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Protokollrotation</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Von Zeitgebern des Typs `cronjobs` wurde zu `systemd` gewechselt, um ein Fehlschlagen von `logrotate` auf Workerknoten zu vermeiden, die nicht innerhalb von 90 Tagen erneute geladen oder aktualisiert werden. **Hinweis**: In allen früheren Versionen für dieses Unterrelease war die primäre Platte mit Daten gefüllt, nachdem der Cron-Job fehlgeschlagen war, weil die Protokolle nicht rotiert wurden. Der Cron-Job schlägt fehl, nachdem der Workerknoten 90 Tage aktiv ist, ohne erneut aktualisiert oder geladen zu werden. Wenn die gesamte Speicherkapazität der primären Platte durch Protokolle erschöpft ist, wechselt der Workerknoten in den Status 'Fehlgeschlagen'. Der Fehler auf dem Workerknoten kann mit dem [command](/docs/containers?topic=containers-cs_cli_reference#cs_worker_reload) `ibmcloud ks worker-reload` oder dem [Befehl](/docs/containers?topic=containers-cs_cli_reference#cs_worker_update) `ibmcloud ks worker-update` behoben werden.</td>
</tr>
<tr>
<td>Laufzeitkomponenten für Workerknoten (`kubelet`, `kube-proxy`, `docker`)</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Entfernte Abhängigkeiten der Laufzeitkomponenten auf der primären Platte. Diese Erweiterung verhindert das Fehlschlagen von Workerknoten, wenn die Speicherkapazität auf der primären Platte erschöpft ist.</td>
</tr>
<tr>
<td>Ablauf des Rootkennworts</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Rootkennwörter für Workerknoten laufen aus Konformitätsgründen nach 90 Tagen ab. Wenn für die Automatisierungstools eine Anmeldung am Workerknoten erforderlich ist oder wenn sie auf Cron-Jobs angewiesen sind, die als Root ausgeführt werden, können Sie den Ablauf der Kennwortgültigkeit inaktivieren, indem Sie sich am Workerknoten anmelden und `chage -M -1 root` ausführen. **Hinweis**: Falls Konformitätsanforderungen für die Sicherheit gelten, die eine Ausführung als Root oder eine Entfernung des Ablaufs der Kennwortgültigkeit verhindert, inaktivieren Sie nicht das Ablaufdatum. Stattdessen können Sie die Workerknoten mindestens alle 90 Tage [aktualisieren](/docs/containers?topic=containers-cs_cli_reference#cs_worker_update) oder [erneut laden](/docs/containers?topic=containers-cs_cli_reference#cs_worker_reload).</td>
</tr>
<tr>
<td>systemd</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Bereinigen Sie die temporären Mounteinheiten in regelmäßigen Abständen, um zu verhindern, dass sie unbegrenzt werden. Diese Aktion ist für [Kubernetes-Fehler 57345 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/kubernetes/issues/57345) vorgesehen.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten-Fixpack 1.8.15_1520, veröffentlicht am 23. August 2018
{: #1815_1520}

<table summary="Seit Version 1.9.9_1519 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.8.15_1519</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>`systemd`</td>
<td>229</td>
<td>230</td>
<td>Aktualisierung von `systemd` zur Korrektur des `cgroup`-Lecks.</td>
</tr>
<tr>
<td>Kernel</td>
<td>4.4.0-127</td>
<td>4.4.0-133</td>
<td>Aktualisierte Workerknoten-Images mit Kernelaktualisierung für [CVE-2018-3620,CVE-2018-3646 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://usn.ubuntu.com/3741-1/).</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten-Fixpack 1.8.15_1519, veröffentlicht am 13. August 2018
{: #1815_1519}

<table summary="Seit Version 1.9.9_1518 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.8.15_1518</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu-Pakete</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Aktualisierungen an installierten Ubuntu-Paketen.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für 1.8.15_1518, veröffentlicht 27. Juli 2018
{: #1815_1518}

<table summary="Seit Version 1.8.13_1516 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.8.13_1516</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>Version 1.8.13-176</td>
<td>Version 1.8.15-204</td>
<td>Wurde für die Unterstützung von Kubernetes 1.8.15 aktualisiert. Darüber hinaus enthalten `create failure`-Ereignisse des LoadBalancer-Service nun alle Fehler portierbarer Teilnetze.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage-Plug-in</td>
<td>320</td>
<td>334</td>
<td>Das Zeitlimit bei der Erstellung persistenter Datenträger wurde von 15 auf 30 Minuten erhöht. Der Standardabrechnungstyp wurde in 'Stündlich' (`hourly`) geändert. Es wurden Mountoptionen zu den vordefinierten Speicherklassen hinzugefügt. In der NFS-Dateispeicherinstanz in Ihrem Konto der IBM Cloud-Infrastruktur (SoftLayer) wurde das Format des Felds **Anmerkungen** in JSON geändert und der Kubernetes-Namensbereich hinzugefügt, in dem der persistente Datenträger bereitgestellt ist. Für die Unterstützung von Mehrzonenclustern wurden persistenten Datenträgern Zonen- und Regionsbezeichnungen hinzugefügt.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>Version 1.8.13</td>
<td>Version 1.8.15</td>
<td>Lesen Sie die Kubernetes-[Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/kubernetes/releases/tag/v1.8.15).</td>
</tr>
<tr>
<td>Kernel</td>
<td>n.z.</td>
<td>n.z.</td>
<td>An den Netzeinstellungen für Workerknoten wurden kleinere Verbesserungen vorgenommen, um Netzarbeitslasten mit hoher Leistung zu unterstützen.</td>
</tr>
<tr>
<td>OpenVPN-Client</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Die `vpn`-Bereitstellung des OpenVPN-Clients, die im Namensbereich `kube-system` ausgeführt wird, wird nun von `addon-manager` von Kubernetes verwaltet.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten-Fixpack 1.8.13_1516, veröffentlicht am 3. Juli 2018
{: #1813_1516}

<table summary="Seit Version 1.8.13_1515 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.8.13_1515</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Optimiertes `sysctl` für Netzauslastungen mit hoher Leistung.</td>
</tr>
</tbody>
</table>


### Änderungsprotokoll für Workerknoten-Fixpack 1.8.13_1515, veröffentlicht am 21. Juni 2018
{: #1813_1515}

<table summary="Seit Version 1.8.13_1514 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.8.13_1514</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Docker</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Bei nicht verschlüsselten Maschinentypen wird die sekundäre Platte bereinigt, indem Sie beim erneuten Laden oder Aktualisieren des Workerknotens ein neues Dateisystem abrufen.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für 1.8.13_1514, veröffentlicht 19. Juni 2018
{: #1813_1514}

<table summary="Seit Version 1.8.11_1512 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.8.11_1512</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>Version 1.8.11</td>
<td>Version 1.8.13</td>
<td>Lesen Sie die [Kubernetes-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/kubernetes/releases/tag/v1.8.13).</td>
</tr>
<tr>
<td>Kubernetes-Konfiguration</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Der Option `--admission-control` wurde `PodSecurityPolicy` für den Kubernetes-API-Server des Clusters hinzugefügt und der Cluster wurde für die Unterstützung von Sicherheitsrichtlinien für Pods konfiguriert. Weitere Informationen finden Sie unter [Sicherheitsrichtlinien für Pods konfigurieren](/docs/containers?topic=containers-psp).</td>
</tr>
<tr>
<td>IBM Cloud-Provider</td>
<td>Version 1.8.11-126</td>
<td>Version 1.8.13-176</td>
<td>Wurde für die Unterstützung von Kubernetes 1.8.13 aktualisiert.</td>
</tr>
<tr>
<td>OpenVPN-Client</td>
<td>n.z.</td>
<td>n.z.</td>
<td><code>livenessProbe</code> wurde der <code>vpn</code>-Bereitstellung des OpenVPN-Clients hinzugefügt, die im Namensbereich <code>kube-system</code> ausgeführt wird.</td>
</tr>
</tbody>
</table>


### Änderungsprotokoll für Workerknoten-Fixpack 1.8.11_1512, veröffentlicht am 11. Juni 2018
{: #1811_1512}

<table summary="Seit Version 1.8.11_1511 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.8.11_1511</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernelaktualisierung</td>
<td>4.4.0-116</td>
<td>4.4.0-127</td>
<td>Neue Workerknoten-Images mit Kernelaktualisierung für [CVE-2018-3639 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-3639).</td>
</tr>
</tbody>
</table>


### Änderungsprotokoll für Workerknoten Fixpack 1.8.11_1511, veröffentlicht am 18. Mai 2018
{: #1811_1511}

<table summary="Seit Version 1.8.11_1510 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.8.11_1510</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Fix für einen Programmfehler, der bei der Verwendung des Blockspeicher-Plug-ins auftrat.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten Fixpack 1.8.11_1510, veröffentlicht am 16. Mai 2018
{: #1811_1510}

<table summary="Seit Version 1.8.11_1509 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.8.11_1509</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Die im Stammverzeichnis `kubelet` gespeicherten Daten werden nun auf einer größeren sekundären Platte der Maschine mit dem Workerknoten gespeichert.</td>
</tr>
</tbody>
</table>


### Änderungsprotokoll für 1.8.11_1509, veröffentlicht am 19. April 2018
{: #1811_1509}

<table summary="Seit Version 1.8.8_1507 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.8.8_1507</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.8.8</td>
<td>v1.8.11	</td>
<td><p>Lesen Sie die [Kubernetes-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/kubernetes/releases/tag/v1.8.11). Dieses Release befasst sich mit den Schwachstellen [CVE-2017-1002101 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002101) und [CVE-2017-1002102 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002102).</p><p>`secret`, `configMap`, `downwardAPI` und projizierte Datenträger werden jetzt nur schreibgeschützt angehängt. Bisher konnten Apps Daten in diese Datenträger schreiben, aber das System konnte die Daten möglicherweise automatisch zurücksetzen. Wenn Ihre Apps das bisherige unsichere Verhalten erwarten, ändern Sie sie entsprechend.</p></td>
</tr>
<tr>
<td>Containerimage anhalten</td>
<td>3.0</td>
<td>3.1</td>
<td>Entfernt geerbte verwaiste Zombie-Prozesse.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>Version 1.8.8-86</td>
<td>Version 1.8.11-126</td>
<td>`NodePort`- und `LoadBalancer`-Services unterstützen jetzt [das Beibehalten der Client-Quellen-IP](/docs/containers?topic=containers-loadbalancer#node_affinity_tolerations) durch Festlegen von `service.spec.externalTrafficPolicy` auf `Local`.</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>Korrigiert die [Edge-Knoten](/docs/containers?topic=containers-edge#edge)-Tolerierungskonfiguration für ältere Cluster.</td>
</tr>
</tbody>
</table>

<br />


### Version 1.7, Änderungsprotokoll (nicht unterstützt)
{: #17_changelog}

Überprüfen Sie die folgenden Änderungen.

#### Änderungsprotokoll für Workerknoten-Fixpack 1.7.16_1514, veröffentlicht am 11. Juni 2018
{: #1716_1514}

<table summary="Seit Version 1.7.16_1513 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.7.16_1513</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernelaktualisierung</td>
<td>4.4.0-116</td>
<td>4.4.0-127</td>
<td>Neue Workerknoten-Images mit Kernelaktualisierung für [CVE-2018-3639 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-3639).</td>
</tr>
</tbody>
</table>

#### Änderungsprotokoll für Workerknoten Fixpack 1.7.16_1513, veröffentlicht am 18. Mai 2018
{: #1716_1513}

<table summary="Seit Version 1.7.16_1512 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.7.16_1512</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Fix für einen Programmfehler, der bei der Verwendung des Blockspeicher-Plug-ins auftrat.</td>
</tr>
</tbody>
</table>

#### Änderungsprotokoll für Workerknoten Fixpack 1.7.16_1512, veröffentlicht am 16. Mai 2018
{: #1716_1512}

<table summary="Seit Version 1.7.16_1511 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.7.16_1511</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Die im Stammverzeichnis `kubelet` gespeicherten Daten werden nun auf einer größeren sekundären Platte der Maschine mit dem Workerknoten gespeichert.</td>
</tr>
</tbody>
</table>

#### Änderungsprotokoll für 1.7.16_1511, veröffentlicht am 19. April 2018
{: #1716_1511}

<table summary="Seit Version 1.7.4_1509 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.7.4_1509</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.7.4</td>
<td>v1.7.16	</td>
<td><p>Lesen Sie die [Kubernetes-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/kubernetes/releases/tag/v1.7.16). Dieses Release befasst sich mit den Schwachstellen [CVE-2017-1002101 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002101) und [CVE-2017-1002102 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002102).</p><p>`secret`, `configMap`, `downwardAPI` und projizierte Datenträger werden jetzt nur schreibgeschützt angehängt. Bisher konnten Apps Daten in diese Datenträger schreiben, aber das System konnte die Daten möglicherweise automatisch zurücksetzen. Wenn Ihre Apps das bisherige unsichere Verhalten erwarten, ändern Sie sie entsprechend.</p></td>
</tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.7.4-133</td>
<td>v1.7.16-17</td>
<td>`NodePort`- und `LoadBalancer`-Services unterstützen jetzt [das Beibehalten der Client-Quellen-IP](/docs/containers?topic=containers-loadbalancer#node_affinity_tolerations) durch Festlegen von `service.spec.externalTrafficPolicy` auf `Local`.</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>Korrigiert die [Edge-Knoten](/docs/containers?topic=containers-edge#edge)-Tolerierungskonfiguration für ältere Cluster.</td>
</tr>
</tbody>
</table>
