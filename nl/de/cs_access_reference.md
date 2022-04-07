---

copyright:
  years: 2014, 2019
lastupdated: "2019-04-15"

keywords: kubernetes, iks

subcollection: containers

---

{:new_window: target="blank"}
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


# Benutzerzugriffsberechtigungen
{: #access_reference}

Wenn Sie [Clusterberechtigungen zuordnen](/docs/containers?topic=containers-users), kann es schwierig sein, zu beurteilen, welche Rolle einem Benutzer zugeordnet werden muss. Anhand der Tabellen in den folgenden Abschnitten können Sie das Mindestniveau der Berechtigungen feststellen, die für die Ausführung allgemeiner Tasks in {{site.data.keyword.containerlong}} erforderlich sind.
{: shortdesc}

Seit dem 30. Januar 2019 arbeitet {{site.data.keyword.containerlong_notm}} mit einer neuen Methode zur Berechtigung von Benutzern für {{site.data.keyword.Bluemix_notm}} IAM: [Servicezugriffsrollen](#service). Durch diese Servicerollen wird der Zugriff auf Ressourcen im Cluster, wie zum Beispiel auf Kubernetes-Namensbereiche, erteilt. Weitere Informationen finden Sie im folgenden Blog: [Introducing service roles and namespaces in IAM for more granular control of cluster access ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/blogs/bluemix/2019/02/introducing-service-roles-and-namespaces-in-iam-for-more-granular-control-of-cluster-access/).
{: note}

## {{site.data.keyword.Bluemix_notm}} IAM-Plattformrollen
{: #iam_platform}

{{site.data.keyword.containerlong_notm}} ist für die Verwendung von {{site.data.keyword.Bluemix_notm}} Identity and Access Management-Rollen (IAM-Rollen) konfiguriert. Von den {{site.data.keyword.Bluemix_notm}} IAM-Plattformrollen werden die Aktionen festgelegt, die Benutzer an {{site.data.keyword.Bluemix_notm}}-Ressourcen wie Clustern, Workerknoten und Ingress-Lastausgleichsfunktionen für Anwendungen (ALB) ausführen können. Durch {{site.data.keyword.Bluemix_notm}} IAM-Plattformrollen werden darüber hinaus automatisch auch grundlegende Infrastrukturberechtigungen für Benutzer festgelegt. Informationen zum Festlegen von Plattformrollen finden Sie unter [{{site.data.keyword.Bluemix_notm}} IAM-Plattformberechtigungen zuordnen](/docs/containers?topic=containers-users#platform).
{: shortdesc}

<p class="tip">Weisen Sie {{site.data.keyword.Bluemix_notm}} IAM-Plattformrollen nicht gleichzeitig mit einer Servicerolle zu. Sie müssen Plattform- und Servicerollen separat zuweisen.</p>

Die Tabellen in den folgenden Abschnitten enthalten die jeweiligen Berechtigungen für die Clusterverwaltung, für die Protokollierung und für Ingress-Berechtigungen, die durch die einzelnen {{site.data.keyword.Bluemix_notm}} IAM-Plattformrollen erteilt werden. Die Tabellen sind nach CLI-Befehlsnamen alphabetisch angeordnet.

* [Berechtigungsfreie Aktionen](#none-actions)
* [Aktionen für Anzeigeberechtigte](#view-actions)
* [Editoraktionen](#editor-actions)
* [Operatoraktionen](#operator-actions)
* [Administratoraktionen](#admin-actions)

### Berechtigungsfreie Aktionen
{: #none-actions}

Jedem Benutzer in Ihrem Konto, der den CLI-Befehl ausführt oder den API-Aufruf für die Aktion in der folgenden Tabelle durchführt, wird das Ergebnis angezeigt, auch wenn dem Benutzer keine Berechtigungen zugeordnet sind.
{: shortdesc}

<table>
<caption>Übersicht über die CLI-Befehle und API-Aufrufe, für die keine Berechtigungen in {{site.data.keyword.containerlong_notm}} erforderlich sind</caption>
<thead>
<th id="none-actions-action">Clusterverwaltungsaktion</th>
<th id="none-actions-cli">CLI-Befehl</th>
<th id="none-actions-api">API-Aufruf</th>
</thead>
<tbody>
<tr>
<td>Liste der Versionen anzeigen, die für verwaltete Add-ons in {{site.data.keyword.containerlong_notm}} unterstützt werden.</td>
<td><code>[ibmcloud ks addon-versions](/docs/containers?topic=containers-cs_cli_reference#cs_addon_versions)</code></td>
<td><code>[GET /v1/kube-versions](https://containers.cloud.ibm.com/swagger-api/#!/util/GetAddons)</code></td>
</tr>
<tr>
<td>API-Endpunkt für {{site.data.keyword.containerlong_notm}} als Ziel festlegen oder anzeigen</td>
<td><code>[ibmcloud ks api](/docs/containers?topic=containers-cs_cli_reference#cs_cli_api)</code></td>
<td>-</td>
</tr>
<tr>
<td>Liste der unterstützten Befehle und Parameter anzeigen</td>
<td><code>[ibmcloud ks help](/docs/containers?topic=containers-cs_cli_reference#cs_help)</code></td>
<td>-</td>
</tr>
<tr>
<td>{{site.data.keyword.containerlong_notm}}-Plug-ins initialisieren oder Region angeben, in der Sie Kubernetes-Cluster erstellen oder darauf zugreifen möchten</td>
<td><code>[ibmcloud ks init](/docs/containers?topic=containers-cs_cli_reference#cs_init)</code></td>
<td>-</td>
</tr>
<tr>
<td>Liste der Kubernetes-Versionen anzeigen, die in {{site.data.keyword.containerlong_notm}} unterstützt werden</td>
<td><code>[ibmcloud ks kube-versions](/docs/containers?topic=containers-cs_cli_reference#cs_kube_versions)</code></td>
<td><code>[GET /v1/kube-versions](https://containers.cloud.ibm.com/swagger-api/#!/util/GetKubeVersions)</code></td>
</tr>
<tr>
<td>Liste der für Workerknoten verfügbaren Maschinentypen anzeigen</td>
<td><code>[ibmcloud ks machine-types](/docs/containers?topic=containers-cs_cli_reference#cs_machine_types)</code></td>
<td><code>[GET /v1/datacenters/{datacenter}/machine-types](https://containers.cloud.ibm.com/swagger-api/#!/util/GetDatacenterMachineTypes)</code></td>
</tr>
<tr>
<td>Aktuelle Nachrichten für den Benutzer der IBMid anzeigen</td>
<td><code>[ibmcloud ks messages](/docs/containers?topic=containers-cs_cli_reference#cs_messages)</code></td>
<td><code>[GET /v1/messages](https://containers.cloud.ibm.com/swagger-api/#!/util/GetMessages)</code></td>
</tr>
<tr>
<td>Nach der {{site.data.keyword.containerlong_notm}}-Region suchen, in der Sie sich befinden</td>
<td><code>[ibmcloud ks region](/docs/containers?topic=containers-cs_cli_reference#cs_region)</code></td>
<td>-</td>
</tr>
<tr>
<td>Region für {{site.data.keyword.containerlong_notm}} festlegen</td>
<td><code>[ibmcloud ks region-set](/docs/containers?topic=containers-cs_cli_reference#cs_region-set)</code></td>
<td>-</td>
</tr>
<tr>
<td>Verfügbare Regionen auflisten</td>
<td><code>[ibmcloud ks regions](/docs/containers?topic=containers-cs_cli_reference#cs_regions)</code></td>
<td><code>[GET /v1/regions](https://containers.cloud.ibm.com/swagger-api/#!/util/GetRegions)</code></td>
</tr>
<tr>
<td>Liste der verfügbaren Zonen zum Erstellen eines Clusters anzeigen</td>
<td><code>[ibmcloud ks zones](/docs/containers?topic=containers-cs_cli_reference#cs_datacenters)</code></td>
<td><code>[GET /v1/zones](https://containers.cloud.ibm.com/swagger-api/#!/util/GetZones)</code></td>
</tr>
</tbody>
</table>

### Aktionen für Anzeigeberechtigte
{: #view-actions}

Die Plattformrolle **Anzeigeberechtigter** (Viewer) umfasst die [Aktionen, die keine Berechtigungen erfordern](#none-actions), sowie die Berechtigungen, die in der folgenden Tabelle aufgeführt werden. Mit der Rolle **Anzeigeberechtigter** können Benutzer wie Auditoren oder Zuständige für die Rechnungsstellung die Clusterdetails anzeigen, aber nicht die Infrastruktur ändern.
{: shortdesc}

<table>
<caption>Übersicht über die CLI-Befehle und API-Aufrufe für die Clusterverwaltung, die die Plattformrolle eines Anzeigeberechtigten in {{site.data.keyword.containerlong_notm}} erfordern</caption>
<thead>
<th id="view-actions-mngt">Clusterverwaltungsaktion</th>
<th id="view-actions-cli">CLI-Befehl</th>
<th id="view-actions-api">API-Aufruf</th>
</thead>
<tbody>
<tr>
<td>Name und E-Mail-Adresse für den Eigner des {{site.data.keyword.Bluemix_notm}} IAM-API-Schlüssels für eine Ressourcengruppe und Region anzeigen</td>
<td><code>[ibmcloud ks api-key-info](/docs/containers?topic=containers-cs_cli_reference#cs_api_key_info)</code></td>
<td><code>[GET /v1/logging/{idOrName}/clusterkeyowner](https://containers.cloud.ibm.com/swagger-logging/#!/logging/GetClusterKeyOwner)</code></td>
</tr>
<tr>
<td>Kubernetes-Konfigurationsdaten und -Zertifikate herunterladen, um eine Verbindung zum Cluster herzustellen und `kubectl`-Befehle auszuführen</td>
<td><code>[ibmcloud ks cluster-config](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_config)</code></td>
<td><code>[GET /v1/clusters/{idOrName}/config](https://containers.cloud.ibm.com/swagger-api/#!/clusters/GetClusterConfig)</code></td>
</tr>
<tr>
<td>Informationen zu einem Cluster anzeigen</td>
<td><code>[ibmcloud ks cluster-get](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_get)</code></td>
<td><code>[GET /v1/clusters/{idOrName}](https://containers.cloud.ibm.com/swagger-api/#!/clusters/GetCluster)</code></td>
</tr>
<tr>
<td>Alle Services in allen Namensbereichen auflisten, die an einen Cluster gebunden sind</td>
<td><code>[ibmcloud ks cluster-services](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_services)</code></td>
<td><code>[GET /v1/clusters/{idOrName}/services](https://containers.cloud.ibm.com/swagger-api/#!/clusters/ListServicesForAllNamespaces)</code></td>
</tr>
<tr>
<td>Alle Cluster auflisten</td>
<td><code>[ibmcloud ks clusters](/docs/containers?topic=containers-cs_cli_reference#cs_clusters)</code></td>
<td><code>[GET /v1/clusters](https://containers.cloud.ibm.com/swagger-api/#!/clusters/GetClusters)</code></td>
</tr>
<tr>
<td>Infrastrukturberechtigungsnachweise für das {{site.data.keyword.Bluemix_notm}}-Konto für den Zugriff auf ein anderes Portfolio der IBM Cloud-Infrastruktur (SoftLayer) abrufen</td>
<td><code>[ibmcloud ks credential-get](/docs/containers?topic=containers-cs_cli_reference#cs_credential_get)</code></td><td><code>[GET /v1/credentials](https://containers.cloud.ibm.com/swagger-api/#!/accounts/GetUserCredentials)</code></td>
</tr>
<tr>
<td>Alle an einen bestimmten Namensbereich gebundenen Services auflisten</td>
<td>-</td>
<td><code>[GET /v1/clusters/{idOrName}/services/{namespace}](https://containers.cloud.ibm.com/swagger-api/#!/clusters/ListServicesInNamespace)</code></td>
</tr>
<tr>
<td>Alle an einen Cluster gebundenen benutzerverwalteten Teilnetze auflisten</td>
<td>-</td>
<td><code>[GET /v1/clusters/{idOrName}/usersubnets](https://containers.cloud.ibm.com/swagger-api/#!/clusters/GetClusterUserSubnet)</code></td>
</tr>
<tr>
<td>Verfügbare Teilnetze im Infrastrukturkonto auflisten</td>
<td><code>[ibmcloud ks subnets](/docs/containers?topic=containers-cs_cli_reference#cs_subnets)</code></td>
<td><code>[GET /v1/subnets](https://containers.cloud.ibm.com/swagger-api/#!/properties/ListSubnets)</code></td>
</tr>
<tr>
<td>VLAN-Spanning-Status für das Infrastrukturkonto anzeigen</td>
<td><code>[ibmcloud ks vlan-spanning-get](/docs/containers?topic=containers-cs_cli_reference#cs_vlan_spanning_get)</code></td>
<td><code>[GET /v1/subnets/vlan-spanning](https://containers.cloud.ibm.com/swagger-api/#!/accounts/GetVlanSpanning)</code></td>
</tr>
<tr>
<td>Wenn für nur einen Cluster festgelegt: VLANs auflisten, mit denen der Cluster in einer Zone verbunden ist.</br>Wenn für alle Cluster im Konto festgelegt: Alle in einer Zone verfügbaren VLANs auflisten.</td>
<td><code>[ibmcloud ks vlans](/docs/containers?topic=containers-cs_cli_reference#cs_vlans)</code></td>
<td><code>[GET /v1/datacenters/{datacenter}/vlans](https://containers.cloud.ibm.com/swagger-api/#!/properties/GetDatacenterVLANs)</code></td>
</tr>
<tr>
<td>Alle Webhooks für einen Cluster auflisten</td>
<td>-</td>
<td><code>[GET /v1/clusters/{idOrName}/webhooks](https://containers.cloud.ibm.com/swagger-api/#!/clusters/GetClusterWebhooks)</code></td>
</tr>
<tr>
<td>Informationen zu einem Workerknoten anzeigen</td>
<td><code>[ibmcloud ks worker-get](/docs/containers?topic=containers-cs_cli_reference#cs_worker_get)</code></td>
<td><code>[GET /v1/clusters/{idOrName}/workers/{workerId}](https://containers.cloud.ibm.com/swagger-api/#!/clusters/GetWorkers)</code></td>
</tr>
<tr>
<td>Informationen zu einem Worker-Pool anzeigen</td>
<td><code>[ibmcloud ks worker-pool-get](/docs/containers?topic=containers-cs_cli_reference#cs_worker_pool_get)</code></td>
<td><code>[GET /v1/clusters/{idOrName}/workerpools/{poolidOrName}](https://containers.cloud.ibm.com/swagger-api/#!/clusters/GetWorkerPool)</code></td>
</tr>
<tr>
<td>Alle Worker-Pools in einem Cluster auflisten</td>
<td><code>[ibmcloud ks worker-pools](/docs/containers?topic=containers-cs_cli_reference#cs_worker_pools)</code></td>
<td><code>[GET /v1/clusters/{idOrName}/workerpools](https://containers.cloud.ibm.com/swagger-api/#!/clusters/GetWorkerPools)</code></td>
</tr>
<tr>
<td>Alle Workerknoten in einem Cluster auflisten</td>
<td><code>[ibmcloud ks workers](/docs/containers?topic=containers-cs_cli_reference#cs_workers)</code></td>
<td><code>[GET /v1/clusters/{idOrName}/workers](https://containers.cloud.ibm.com/swagger-api/#!/clusters/GetClusterWorkers)</code></td>
</tr>
</tbody>
</table>

<table>
<caption>Übersicht über CLI-Befehle und API-Aufrufe für Ingress-Aktionen, die die Plattformrolle eines Anzeigeberechtigten in {{site.data.keyword.containerlong_notm}} erfordern</caption>
<thead>
<th id="view-actions-ingress">Ingress-Aktion</th>
<th id="view-actions-cli2">CLI-Befehl</th>
<th id="view-actions-api2">API-Aufruf</th>
</thead>
<tbody>
<tr>
<td>Informationen zu einer Ingress-Lastausgleichsfunktion für Anwendungen (ALB)</td>
<td><code>[ibmcloud ks alb-get](/docs/containers?topic=containers-cs_cli_reference#cs_alb_get)</code></td>
<td><code>[GET /albs/{albId}](https://containers.cloud.ibm.com/swagger-alb-api/#!/alb/GetClusterALB)</code></td>
</tr>
<tr>
<td>ALB-Typen anzeigen, die in der Region unterstützt werden</td>
<td><code>[ibmcloud ks alb-types](/docs/containers?topic=containers-cs_cli_reference#cs_alb_types)</code></td>
<td><code>[GET /albtypes](https://containers.cloud.ibm.com/swagger-alb-api/#!/util/GetAvailableALBTypes)</code></td>
</tr>
<tr>
<td>Alle Ingress-ALBs in einem Cluster auflisten</td>
<td><code>[ibmcloud ks albs](/docs/containers?topic=containers-cs_cli_reference#cs_albs)</code></td>
<td><code>[GET /clusters/{idOrName}](https://containers.cloud.ibm.com/swagger-alb-api/#!/alb/GetClusterALBs)</code></td>
</tr>
</tbody>
</table>


<table>
<caption>Übersicht über CLI-Befehle und API-Aufrufe für Protokollierungsaktionen, die die Plattformrolle eines Anzeigeberechtigten in {{site.data.keyword.containerlong_notm}} erfordern</caption>
<thead>
<th id="view-actions-log">Protokollierungsaktion</th>
<th id="view-actions-cli3">CLI-Befehl</th>
<th id="view-actions-api3">API-Aufruf</th>
</thead>
<tbody>
<tr>
<td>Status für automatische Aktualisierungen des Fluentd-Add-ons anzeigen</td>
<td><code>[ibmcloud ks logging-autoupdate-get](/docs/containers?topic=containers-cs_cli_reference#cs_log_autoupdate_get)</code></td>
<td><code>[GET /v1/logging/{idOrName}/updatepolicy](https://containers.cloud.ibm.com/swagger-logging/#!/logging/GetUpdatePolicy)</code></td>
</tr>
<tr>
<td>Standardprotokollierungsendpunkt für die Zielregion anzeigen</td>
<td>-</td>
<td><code>[GET /v1/logging/{idOrName}/default](https://containers.cloud.ibm.com/swagger-logging/#!/logging/GetDefaultLoggingEndpoint)</code></td>
</tr>
<tr>
<td>Alle Protokollweiterleitungskonfigurationen im Cluster oder für eine Protokollquelle im Cluster auflisten</td>
<td><code>[ibmcloud ks logging-config-get](/docs/containers?topic=containers-cs_cli_reference#cs_logging_get)</code></td>
<td><code>[GET /v1/logging/{idOrName}/loggingconfig](https://containers.cloud.ibm.com/swagger-logging/#!/logging/FetchLoggingConfigs) und [GET /v1/logging/{idOrName}/loggingconfig/{logSource}](https://containers.cloud.ibm.com/swagger-logging/#!/logging/FetchLoggingConfigsForSource)</code></td>
</tr>
<tr>
<td>Informationen zu einer Protokollfilterkonfiguration anzeigen</td>
<td><code>[ibmcloud ks logging-filter-get](/docs/containers?topic=containers-cs_cli_reference#cs_log_filter_view)</code></td>
<td><code>[GET /v1/logging/{idOrName}/filterconfigs/{id}](https://containers.cloud.ibm.com/swagger-logging/#!/filter/FetchFilterConfig)</code></td>
</tr>
<tr>
<td>Alle Protokollierungsfilterkonfigurationen im Cluster auflisten</td>
<td><code>[ibmcloud ks logging-filter-get](/docs/containers?topic=containers-cs_cli_reference#cs_log_filter_view)</code></td>
<td><code>[GET /v1/logging/{idOrName}/filterconfigs](https://containers.cloud.ibm.com/swagger-logging/#!/filter/FetchFilterConfigs)</code></td>
</tr>
</tbody>
</table>

### Editoraktionen
{: #editor-actions}

Die Plattformrolle **Editor** umfasst alle Berechtigungen, die durch die Rolle **Anzeigeberechtigter** erteilt werden, sowie die folgenden Berechtigungen. Mit der Rolle **Editor** können Benutzer wie z. B. Entwickler Services binden, mit Ingress-Ressourcen arbeiten und die Protokollweiterleitung für ihre Apps einrichten, jedoch nicht die Infrastruktur ändern. **Tipp:** Verwenden Sie diese Rolle für Anwendungsentwickler und ordnen Sie die Rolle <a href="#cloud-foundry">Cloud Foundry</a>-**Entwickler** zu.
{: shortdesc}

<table>
<caption>Übersicht über die CLI-Befehle und API-Aufrufe für die Clusterverwaltung, die die Plattformrolle eines Editors in {{site.data.keyword.containerlong_notm}} erfordern</caption>
<thead>
<th id="editor-actions-mngt">Clusterverwaltungsaktion</th>
<th id="editor-actions-cli">CLI-Befehl</th>
<th id="editor-actions-api">API-Aufruf</th>
</thead>
<tbody>
<tr>
<td>Service an einen Cluster binden. **Hinweis:** Die Cloud Foundry-Rolle 'Entwickler' (Developer) in dem Bereich, in dem sich der Service befindet, ist ebenfalls erforderlich.</td>
<td><code>[ibmcloud ks cluster-service-bind](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_service_bind)</code></td>
<td><code>[POST /v1/clusters/{idOrName}/services](https://containers.cloud.ibm.com/swagger-api/#!/clusters/BindServiceToNamespace)</code></td>
</tr>
<tr>
<td>Bindung eines Service an einen Cluster aufheben. **Hinweis:** Die Cloud Foundry-Rolle 'Entwickler' (Developer) in dem Bereich, in dem sich der Service befindet, ist ebenfalls erforderlich.</td>
<td><code>[ibmcloud ks cluster-service-unbind](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_service_unbind)</code></td>
<td><code>[DELETE /v1/clusters/{idOrName}/services/{namespace}/{serviceInstanceId}](https://containers.cloud.ibm.com/swagger-api/#!/clusters/UnbindServiceFromNamespace)</code></td>
</tr>
<tr>
<td>Webhook in einem Cluster erstellen.</td>
<td><code>[ibmcloud ks webhook-create](/docs/containers?topic=containers-cs_cli_reference#cs_webhook_create)</code></td>
<td><code>[POST /v1/clusters/{idOrName}/webhooks](https://containers.cloud.ibm.com/swagger-api/#!/clusters/AddClusterWebhooks)</code></td>
</tr>
</tbody>
</table>

<table>
<caption>Übersicht über CLI-Befehle und API-Aufrufe für Ingress-Aktionen, die die Plattformrolle eines Editors in {{site.data.keyword.containerlong_notm}} erfordern</caption>
<thead>
<th id="editor-actions-ingress">Ingress-Aktion</th>
<th id="editor-actions-cli2">CLI-Befehl</th>
<th id="editor-actions-api2">API-Aufruf</th>
</thead>
<tbody>
<tr>
<td>Automatische Aktualisierungen für das Ingress-ALB-Add-on inaktivieren</td>
<td><code>[ibmcloud ks alb-autoupdate-disable](/docs/containers?topic=containers-cs_cli_reference#cs_alb_autoupdate_disable)</code></td>
<td><code>[PUT /clusters/{idOrName}/updatepolicy](https://containers.cloud.ibm.com/swagger-alb-api/#!/alb/ChangeUpdatePolicy)</code></td>
</tr>
<tr>
<td>Automatische Aktualisierungen für das Ingress-ALB-Add-on aktivieren</td>
<td><code>[ibmcloud ks alb-autoupdate-enable](/docs/containers?topic=containers-cs_cli_reference#cs_alb_autoupdate_enable)</code></td>
<td><code>[PUT /clusters/{idOrName}/updatepolicy](https://containers.cloud.ibm.com/swagger-alb-api/#!/alb/ChangeUpdatePolicy)</code></td>
</tr>
<tr>
<td>Prüfen, ob automatische Aktualisierungen für das Ingress-ALB-Add-on aktiviert sind</td>
<td><code>[ibmcloud ks alb-autoupdate-get](/docs/containers?topic=containers-cs_cli_reference#cs_alb_autoupdate_get)</code></td>
<td><code>[GET /clusters/{idOrName}/updatepolicy](https://containers.cloud.ibm.com/swagger-alb-api/#!/alb/GetUpdatePolicy)</code></td>
</tr>
<tr>
<td>Ingress-ALB aktivieren oder inaktivieren</td>
<td><code>[ibmcloud ks alb-configure](/docs/containers?topic=containers-cs_cli_reference#cs_alb_configure)</code></td>
<td><code>[POST /albs](https://containers.cloud.ibm.com/swagger-alb-api/#!/alb/EnableALB) und [DELETE /albs/{albId}](https://containers.cloud.ibm.com/swagger-alb-api/#/)</code></td>
</tr>
<tr>
<td>Aktualisierung des Ingress-ALB-Add-ons auf den Build zurücksetzen, den die ALB-Pods zuvor ausgeführt haben</td>
<td><code>[ibmcloud ks alb-rollback](/docs/containers?topic=containers-cs_cli_reference#cs_alb_rollback)</code></td>
<td><code>[PUT /clusters/{idOrName}/updaterollback](https://containers.cloud.ibm.com/swagger-alb-api/#!/alb/RollbackUpdate)</code></td>
</tr>
<tr>
<td>Einmalige Aktualisierung der ALB-Pods durch manuelles Aktualisieren des Ingress-ALB-Add-ons erzwingen</td>
<td><code>[ibmcloud ks alb-update](/docs/containers?topic=containers-cs_cli_reference#cs_alb_update)</code></td>
<td><code>[PUT /clusters/{idOrName}/update](https://containers.cloud.ibm.com/swagger-alb-api/#!/alb/UpdateALBs)</code></td>
</tr>
</tbody>
</table>

<table>
<caption>Übersicht über CLI-Befehle und API-Aufrufe für Netzlastausgleichsfunktion (NLB), DNS-Host und Statusprüfmonitor, die die Plattformrolle eines Editors in {{site.data.keyword.containerlong_notm}} erfordern</caption>
<thead>
<th id="lbdns-mgmt">DNS-Aktion für Netzlastausgleichsfunktion (NLB)</th>
<th id="lbdns-cli">CLI-Befehl</th>
<th id="lbdns-api">API-Aufruf</th>
</thead>
<tbody>
<tr>
<td>Eine NLB-IP zu einem vorhandenen NLB-Hostnamen hinzufügen.</td>
<td><code>[ibmcloud ks nlb-dns-add](/docs/containers?topic=containers-cs_cli_reference#cs_nlb-dns-add)</code></td>
<td><code>[PUT /clusters/{idOrName}/add](https://containers.cloud.ibm.com/swagger-dns-api/#!/nlb45dns/UpdateDNSWithIP)</code></td>
</tr>
<tr>
<td>Einen DNS-Hostnamen erstellen, um eine oder mehrere NLB-IPs zu registrieren.</td>
<td><code>[ibmcloud ks nlb-dns-create](/docs/containers?topic=containers-cs_cli_reference#cs_nlb-dns-create)</code></td>
<td><code>[POST /clusters/{idOrName}/register](https://containers.cloud.ibm.com/swagger-dns-api/#!/nlb45dns/RegisterDNSWithIP)</code></td>
</tr>
<tr>
<td>NLB-Hostnamen und -IP-Adressen auflisten, die in einem Cluster registriert sind.</td>
<td><code>[ibmcloud ks nlb-dnss](/docs/containers?topic=containers-cs_cli_reference#cs_nlb-dns-ls)</code></td>
<td><code>[GET /clusters/{idOrName}/list](https://containers.cloud.ibm.com/swagger-dns-api/#!/nlb45dns/ListNLBIPsForSubdomain)</code></td>
</tr>
<tr>
<td>NLB-IP-Adresse aus einem Hostnamen entfernen.</td>
<td><code>[ibmcloud ks nlb-dns-rm](/docs/containers?topic=containers-cs_cli_reference#cs_nlb-dns-rm)</code></td>
<td><code>[DELETE /clusters/{idOrName}/host/{nlbHost}/ip/{nlbIP}/remove](https://containers.cloud.ibm.com/swagger-dns-api/#!/nlb45dns/UnregisterDNSWithIP)</code></td>
</tr>
<tr>
<td>Statusprüfmonitor für einen vorhandenen NLB-Hostnamen in einem Cluster konfigurieren und optional aktivieren.</td>
<td><code>[ibmcloud ks nlb-dns-monitor-configure](/docs/containers?topic=containers-cs_cli_reference#cs_nlb-dns-monitor-configure)</code></td>
<td><code>[POST /health/clusters/{idOrName}/config](https://containers.cloud.ibm.com/swagger-dns-api/#!/nlb45health45monitor/AddNlbDNSHealthMonitor)</code></td>
</tr>
<tr>
<td>Einstellungen für einen vorhandenen Statusprüfmonitor anzeigen.</td>
<td><code>[ibmcloud ks nlb-dns-monitor-get](/docs/containers?topic=containers-cs_cli_reference#cs_nlb-dns-monitor-get)</code></td>
<td><code>[GET /health/clusters/{idOrName}/host/{nlbHost}/config](https://containers.cloud.ibm.com/swagger-dns-api/#!/nlb45health45monitor/GetNlbDNSHealthMonitor)</code></td>
</tr>
<tr>
<td>Vorhandenen Statusprüfmonitor für einen Hostnamen in einem Cluster inaktivieren.</td>
<td><code>[ibmcloud ks nlb-dns-monitor-disable](/docs/containers?topic=containers-cs_cli_reference#cs_nlb-dns-monitor-disable)</code></td>
<td><code>[PUT /clusters/{idOrName}/health](https://containers.cloud.ibm.com/swagger-dns-api/#!/nlb45health45monitor/UpdateNlbDNSHealthMonitor)</code></td>
</tr>
<tr>
<td>Einen von Ihnen konfigurierten Statusprüfmonitor aktivieren.</td>
<td><code>[ibmcloud ks nlb-dns-monitor-enable](/docs/containers?topic=containers-cs_cli_reference#cs_nlb-dns-monitor-enable)</code></td>
<td><code>[PUT /clusters/{idOrName}/health](https://containers.cloud.ibm.com/swagger-dns-api/#!/nlb45health45monitor/UpdateNlbDNSHealthMonitor)</code></td>
</tr>
<tr>
<td>Einstellungen des Statusprüfmonitors für jeden NLB-Hostnamen in einem Cluster auflisten.</td>
<td><code>[ibmcloud ks nlb-dns-monitor-ls](/docs/containers?topic=containers-cs_cli_reference#cs_nlb-dns-monitor-ls)</code></td>
<td><code>[GET /health/clusters/{idOrName}/list](https://containers.cloud.ibm.com/swagger-dns-api/#!/nlb45health45monitor/ListNlbDNSHealthMonitors)</code></td>
</tr>
<tr>
<td>Status der Statusprüfung für die IPs hinter den NLB-Hostnamen in einem Cluster auflisten.</td>
<td><code>[ibmcloud ks nlb-dns-monitor-status](/docs/containers?topic=containers-cs_cli_reference#cs_nlb-dns-monitor-status)</code></td>
<td><code>[GET /health/clusters/{idOrName}/status](https://containers.cloud.ibm.com/swagger-dns-api/#!/nlb45health45monitor/ListNlbDNSHealthMonitorStatus)</code></td>
</tr>
</tbody>
</table>

<table>
<caption>Übersicht über CLI-Befehle und API-Aufrufe für Protokollierungsaktionen, die die Plattformrolle eines Editors in {{site.data.keyword.containerlong_notm}} erfordern</caption>
<thead>
<th id="editor-log">Protokollierungsaktion</th>
<th id="editor-cli3">CLI-Befehl</th>
<th id="editor-api3">API-Aufruf</th>
</thead>
<tbody>
<tr>
<td>Webhook für API-Serveraudit erstellen</td>
<td><code>[ibmcloud ks apiserver-config-set](/docs/containers?topic=containers-cs_cli_reference#cs_apiserver_config_set)</code></td>
<td><code>[PUT /v1/clusters/{idOrName}/apiserverconfigs/auditwebhook](https://containers.cloud.ibm.com/swagger-api/#!/clusters/apiserverconfigs/UpdateAuditWebhook)</code></td>
</tr>
<tr>
<td>Webhook für einen API-Serveraudit löschen</td>
<td><code>[ibmcloud ks apiserver-config-unset](/docs/containers?topic=containers-cs_cli_reference#cs_apiserver_config_unset)</code></td>
<td><code>[DELETE /v1/clusters/{idOrName}/apiserverconfigs/auditwebhook](https://containers.cloud.ibm.com/swagger-api/#!/apiserverconfigs/DeleteAuditWebhook)</code></td>
</tr>
<tr>
<td>Protokollweiterleitungskonfiguration für alle Protokollquellen außer <code>kube-audit</code> erstellen</td>
<td><code>[ibmcloud ks logging-config-create](/docs/containers?topic=containers-cs_cli_reference#cs_logging_create)</code></td>
<td><code>[POST /v1/logging/{idOrName}/loggingconfig/{logSource}](https://containers.cloud.ibm.com/swagger-logging/#!/logging/CreateLoggingConfig)</code></td>
</tr>
<tr>
<td>Anzeige einer Protokollweiterleitungskonfiguration aktualisieren</td>
<td><code>[ibmcloud ks logging-config-refresh](/docs/containers?topic=containers-cs_cli_reference#cs_logging_refresh)</code></td>
<td><code>[PUT /v1/logging/{idOrName}/refresh](https://containers.cloud.ibm.com/swagger-logging/#!/logging/RefreshLoggingConfig)</code></td>
</tr>
<tr>
<td>Protokollweiterleitungskonfiguration für alle Protokollquellen außer <code>kube-audit</code> löschen</td>
<td><code>[ibmcloud ks logging-config-rm](/docs/containers?topic=containers-cs_cli_reference#cs_logging_rm)</code></td>
<td><code>[DELETE /v1/logging/{idOrName}/loggingconfig/{logSource}/{id}](https://containers.cloud.ibm.com/swagger-logging/#!/logging/DeleteLoggingConfig)</code></td>
</tr>
<tr>
<td>Alle Protokollweiterleitungskonfigurationen für einen Cluster löschen</td>
<td>-</td>
<td><code>[DELETE /v1/logging/{idOrName}/loggingconfig](https://containers.cloud.ibm.com/swagger-logging/#!/logging/DeleteLoggingConfigs)</code></td>
</tr>
<tr>
<td>Protokollweiterleitungskonfiguration aktualisieren</td>
<td><code>[ibmcloud ks logging-config-update](/docs/containers?topic=containers-cs_cli_reference#cs_logging_update)</code></td>
<td><code>[PUT /v1/logging/{idOrName}/loggingconfig/{logSource}/{id}](https://containers.cloud.ibm.com/swagger-logging/#!/logging/UpdateLoggingConfig)</code></td>
</tr>
<tr>
<td>Protokollfilterkonfiguration erstellen</td>
<td><code>[ibmcloud ks logging-filter-create](/docs/containers?topic=containers-cs_cli_reference#cs_log_filter_create)</code></td>
<td><code>[POST /v1/logging/{idOrName}/filterconfigs](https://containers.cloud.ibm.com/swagger-logging/#!/filter/CreateFilterConfig)</code></td>
</tr>
<tr>
<td>Protokollfilterkonfiguration löschen</td>
<td><code>[ibmcloud ks logging-filter-rm](/docs/containers?topic=containers-cs_cli_reference#cs_log_filter_delete)</code></td>
<td><code>[DELETE /v1/logging/{idOrName}/filterconfigs/{id}](https://containers.cloud.ibm.com/swagger-logging/#!/filter/DeleteFilterConfig)</code></td>
</tr>
<tr>
<td>Alle Protokollierungsfilterkonfigurationen für den Kubernetes-Cluster löschen</td>
<td>-</td>
<td><code>[DELETE /v1/logging/{idOrName}/filterconfigs](https://containers.cloud.ibm.com/swagger-logging/#!/filter/DeleteFilterConfigs)</code></td>
</tr>
<tr>
<td>Protokollfilterkonfiguration aktualisieren</td>
<td><code>[ibmcloud ks logging-filter-update](/docs/containers?topic=containers-cs_cli_reference#cs_log_filter_update)</code></td>
<td><code>[PUT /v1/logging/{idOrName}/filterconfigs/{id}](https://containers.cloud.ibm.com/swagger-logging/#!/filter/UpdateFilterConfig)</code></td>
</tr>
</tbody>
</table>

### Operatoraktionen
{: #operator-actions}

Die Plattformrolle **Operator** (Bediener) umfasst alle Berechtigungen, die durch die Rolle **Anzeigeberechtigter** erteilt werden, sowie die Berechtigungen in der folgenden Tabelle. Mit der Rolle **Operator** können Benutzer wie Site Reliability Engineers (SREs), DevOps-Entwickler oder Clusteradministratoren Workerknoten und eine Infrastruktur zur Fehlerbehebung hinzufügen, indem sie z. B. einen Workerknoten neu laden. Sie können jedoch den Cluster nicht erstellen oder löschen, die Berechtigungsnachweise nicht ändern und keine clusterweiten Funktionen wie Serviceendpunkte oder verwaltete Add-ons einrichten.
{: shortdesc}

<table>
<caption>Übersicht über die CLI-Befehle und API-Aufrufe für die Clusterverwaltung, die die Plattformrolle eines Operators in {{site.data.keyword.containerlong_notm}} erfordern</caption>
<thead>
<th id="operator-mgmt">Clusterverwaltungsaktion</th>
<th id="operator-cli">CLI-Befehl</th>
<th id="operator-api">API-Aufruf</th>
</thead>
<tbody>
<tr>
<td>Kubernetes-Master aktualisieren</td>
<td><code>[ibmcloud ks apiserver-refresh](/docs/containers?topic=containers-cs_cli_reference#cs_apiserver_refresh)</code></td>
<td><code>[PUT /v1/clusters/{idOrName}/masters](https://containers.cloud.ibm.com/swagger-api/#!/clusters/HandleMasterAPIServer)</code></td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} IAM-Service-ID für den Cluster erstellen, Richtlinie für die Service-ID erstellen, die die Servicezugriffsrolle **Leseberechtigter** (Reader) in {{site.data.keyword.registrylong_notm}} zuordnet, und schließlich API-Schlüssel für die Service-ID erstellen</td>
<td><code>[ibmcloud ks cluster-pull-secret-apply](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_pull_secret_apply)</code></td>
<td>-</td>
</tr>
<tr>
<td>Cluster-Masterknoten erneut starten, um die neuen Änderungen der Kubernetes-API-Konfiguration anzuwenden</td>
<td><code>[ibmcloud ks cluster-refresh](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_refresh)</code></td>
<td><code>[PUT /v1/clusters/{idOrName}/masters](https://containers.cloud.ibm.com/swagger-api/#!/clusters/HandleMasterAPIServer)</code></td>
</tr>
<tr>
<td>Teilnetz einem Cluster hinzufügen</td>
<td><code>[ibmcloud ks cluster-subnet-add](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_subnet_add)</code></td>
<td><code>[PUT /v1/clusters/{idOrName}/subnets/{subnetId}](https://containers.cloud.ibm.com/swagger-api/#!/clusters/AddClusterSubnet)</code></td>
</tr>
<tr>
<td>Teilnetz erstellen</td>
<td><code>[ibmcloud ks cluster-subnet-create](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_subnet_create)</code></td>
<td><code>[POST /v1/clusters/{idOrName}/vlans/{vlanId}](https://containers.cloud.ibm.com/swagger-api/#!/clusters/CreateClusterSubnet)</code></td>
</tr>
<tr>
<td>Cluster aktualisieren</td>
<td><code>[ibmcloud ks cluster-update](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_update)</code></td>
<td><code>[PUT /v1/clusters/{idOrName}](https://containers.cloud.ibm.com/swagger-api/#!/clusters/UpdateCluster)</code></td>
</tr>
<tr>
<td>Benutzerverwaltetes Teilnetz einem Cluster hinzufügen</td>
<td><code>[ibmcloud ks cluster-user-subnet-add](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_user_subnet_add)</code></td>
<td><code>[POST /v1/clusters/{idOrName}/usersubnets](https://containers.cloud.ibm.com/swagger-api/#!/clusters/AddClusterUserSubnet)</code></td>
</tr>
<tr>
<td>Benutzerverwaltetes Teilnetz aus einem Cluster entfernen</td>
<td><code>[ibmcloud ks cluster-user-subnet-rm](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_user_subnet_rm)</code></td>
<td><code>[DELETE /v1/clusters/{idOrName}/usersubnets/{subnetId}/vlans/{vlanId}](https://containers.cloud.ibm.com/swagger-api/#!/clusters/RemoveClusterUserSubnet)</code></td>
</tr>
<tr>
<td>Workerknoten hinzufügen</td>
<td><code>[ibmcloud ks worker-add (veraltet)](/docs/containers?topic=containers-cs_cli_reference#cs_worker_add)</code></td>
<td><code>[POST /v1/clusters/{idOrName}/workers](https://containers.cloud.ibm.com/swagger-api/#!/clusters/AddClusterWorkers)</code></td>
</tr>
<tr>
<td>Worker-Pool erstellen</td>
<td><code>[ibmcloud ks worker-pool-create](/docs/containers?topic=containers-cs_cli_reference#cs_worker_pool_create)</code></td>
<td><code>[POST /v1/clusters/{idOrName}/workerpools](https://containers.cloud.ibm.com/swagger-api/#!/clusters/CreateWorkerPool)</code></td>
</tr>
<tr>
<td>Worker-Pool neu ausgleichen</td>
<td><code>[ibmcloud ks worker-pool-rebalance](/docs/containers?topic=containers-cs_cli_reference#cs_rebalance)</code></td>
<td><code>[PATCH /v1/clusters/{idOrName}/workerpools/{poolidOrName}](https://containers.cloud.ibm.com/swagger-api/#!/clusters/PatchWorkerPool)</code></td>
</tr>
<tr>
<td>Größe eines Worker-Pools ändern</td>
<td><code>[ibmcloud ks worker-pool-resize](/docs/containers?topic=containers-cs_cli_reference#cs_worker_pool_resize)</code></td>
<td><code>[PATCH /v1/clusters/{idOrName}/workerpools/{poolidOrName}](https://containers.cloud.ibm.com/swagger-api/#!/clusters/PatchWorkerPool)</code></td>
</tr>
<tr>
<td>Worker-Pool löschen</td>
<td><code>[ibmcloud ks worker-pool-rm](/docs/containers?topic=containers-cs_cli_reference#cs_worker_pool_rm)</code></td>
<td><code>[DELETE /v1/clusters/{idOrName}/workerpools/{poolidOrName}](https://containers.cloud.ibm.com/swagger-api/#!/clusters/RemoveWorkerPool)</code></td>
</tr>
<tr>
<td>Workerknoten neu starten</td>
<td><code>[ibmcloud ks worker-reboot](/docs/containers?topic=containers-cs_cli_reference#cs_worker_reboot)</code></td>
<td><code>[PUT /v1/clusters/{idOrName}/workers/{workerId}](https://containers.cloud.ibm.com/swagger-api/#!/clusters/UpdateClusterWorker)</code></td>
</tr>
<tr>
<td>Workerknoten erneut laden</td>
<td><code>[ibmcloud ks worker-reload](/docs/containers?topic=containers-cs_cli_reference#cs_worker_reload)</code></td>
<td><code>[PUT /v1/clusters/{idOrName}/workers/{workerId}](https://containers.cloud.ibm.com/swagger-api/#!/clusters/UpdateClusterWorker)</code></td>
</tr>
<tr>
<td>Workerknoten entfernen</td>
<td><code>[ibmcloud ks worker-rm](/docs/containers?topic=containers-cs_cli_reference#cs_worker_rm)</code></td>
<td><code>[DELETE /v1/clusters/{idOrName}/workers/{workerId}](https://containers.cloud.ibm.com/swagger-api/#!/clusters/RemoveClusterWorker)</code></td>
</tr>
<tr>
<td>Workerknoten aktualisieren</td>
<td><code>[ibmcloud ks worker-update](/docs/containers?topic=containers-cs_cli_reference#cs_worker_update)</code></td>
<td><code>[PUT /v1/clusters/{idOrName}/workers/{workerId}](https://containers.cloud.ibm.com/swagger-api/#!/clusters/UpdateClusterWorker)</code></td>
</tr>
<tr>
<td>Zonen einem Worker-Pool hinzufügen</td>
<td><code>[ibmcloud ks zone-add](/docs/containers?topic=containers-cs_cli_reference#cs_zone_add)</code></td>
<td><code>[POST /v1/clusters/{idOrName}/workerpools/{poolidOrName}/zones](https://containers.cloud.ibm.com/swagger-api/#!/clusters/AddWorkerPoolZone)</code></td>
</tr>
<tr>
<td>Netzkonfiguration für eine bestimmte Zone in einem Worker-Pool aktualisieren</td>
<td><code>[ibmcloud ks zone-network-set](/docs/containers?topic=containers-cs_cli_reference#cs_zone_network_set)</code></td>
<td><code>[PATCH /v1/clusters/{idOrName}/workerpools/{poolidOrName}/zones/{zoneid}](https://containers.cloud.ibm.com/swagger-api/#!/clusters/AddWorkerPoolZoneNetwork)</code></td>
</tr>
<tr>
<td>Zone aus einem Worker-Pool entfernen</td>
<td><code>[ibmcloud ks zone-rm](/docs/containers?topic=containers-cs_cli_reference#cs_zone_rm)</code></td>
<td><code>[DELETE /v1/clusters/{idOrName}/workerpools/{poolidOrName}/zones/{zoneid}](https://containers.cloud.ibm.com/swagger-api/#!/clusters/RemoveWorkerPoolZone)</code></td>
</tr>
</tbody>
</table>

### Administratoraktionen
{: #admin-actions}

Die Plattformrolle **Administrator** umfasst alle Berechtigungen, die durch die Rollen **Anzeigeberechtigter**, **Editor** und **Operator** erteilt werden, sowie die folgenden Berechtigungen. Mit der Rolle **Administrator** können Benutzer wie z. B. Cluster- oder Kontoadministratoren Cluster erstellen und löschen oder clusterweite Funktionen wie Serviceendpunkte oder verwaltete Add-ons einrichten. Zum Erstellen solcher Infrastrukturressourcen, wie Workerknotenmaschinen, VLANs und Teilnetze, benötigen Administratorbenutzer entweder die **<a href="#infra">Infrastrukturrolle</a> des **Superuser** oder der API-Schlüssel für die Region muss mit den entsprechenden Berechtigungen eingerichtet sein.
{: shortdesc}

<table>
<caption>Übersicht über die CLI-Befehle und API-Aufrufe für die Clusterverwaltung, die die Plattformrolle eines Administrators in {{site.data.keyword.containerlong_notm}} erfordern</caption>
<thead>
<th id="admin-mgmt">Clusterverwaltungsaktion</th>
<th id="admin-cli">CLI-Befehl</th>
<th id="admin-api">API-Aufruf</th>
</thead>
<tbody>
<tr>
<td>API-Schlüssel für das {{site.data.keyword.Bluemix_notm}}-Konto für den Zugriff auf das verknüpfte Portfolio der IBM Cloud-Infrastruktur (SoftLayer) festlegen.</td>
<td><code>[ibmcloud ks api-key-reset](/docs/containers?topic=containers-cs_cli_reference#cs_api_key_reset)</code></td>
<td><code>[POST /v1/keys](https://containers.cloud.ibm.com/swagger-api/#!/accounts/ResetUserAPIKey)</code></td>
</tr>
<tr>
<td>Verwaltetes Add-on wie Istio oder Knative in einem Cluster inaktvieren.</td>
<td><code>[ibmcloud ks cluster-addon-disable](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_addon_disable)</code></td>
<td><code>[PATCH /v1/clusters/{idOrName}/addons](https://containers.cloud.ibm.com/swagger-api/#!/clusters/ManageClusterAddons)</code></td>
</tr>
<tr>
<td>Verwaltetes Add-on wie Istio oder Knative in einem Cluster aktivieren.</td>
<td><code>[ibmcloud ks cluster-addon-enable](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_addon_enable)</code></td>
<td><code>[PATCH /v1/clusters/{idOrName}/addons](https://containers.cloud.ibm.com/swagger-api/#!/clusters/ManageClusterAddons)</code></td>
</tr>
<tr>
<td>Verwaltetes Add-ons wie Istio oder Knative auflisten, die in einem Cluster aktiviert sind.</td>
<td><code>[ibmcloud ks cluster-addons](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_addons)</code></td>
<td><code>[GET /v1/clusters/{idOrName}/addons](https://containers.cloud.ibm.com/swagger-api/#!/clusters/GetClusterAddons)</code></td>
</tr>
<tr>
<td>Kostenlosen Cluster oder Standardcluster erstellen. **Hinweis:** Die Plattformrolle 'Administrator' für {{site.data.keyword.registrylong_notm}} und die Infrastrukturrolle 'Superuser' sind ebenfalls erforderlich.</td>
<td><code>[ibmcloud ks cluster-create](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_create)</code></td>
<td><code>[POST /v1/clusters](https://containers.cloud.ibm.com/swagger-api/#!/clusters/CreateCluster)</code></td>
</tr>
<tr>
<td>Angegebene Funktion für einen Cluster inaktivieren (z. B. den öffentlichen Serviceendpunkt für den Cluster-Master).</td>
<td><code>[ibmcloud ks cluster-feature-disable](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_feature_disable)</code></td>
<td>-</td>
</tr>
<tr>
<td>Angegebene Funktion für einen Cluster aktivieren (z. B. den privaten Serviceendpunkt für den Cluster-Master).</td>
<td><code>[ibmcloud ks cluster-feature-enable](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_feature_enable)</code></td>
<td>-</td>
</tr>
<tr>
<td>Cluster löschen.</td>
<td><code>[ibmcloud ks cluster-rm](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_rm)</code></td>
<td><code>[DELETE /v1/clusters/{idOrName}](https://containers.cloud.ibm.com/swagger-api/#!/clusters/RemoveCluster)</code></td>
</tr>
<tr>
<td>Infrastrukturberechtigungsnachweise für das {{site.data.keyword.Bluemix_notm}}-Konto für den Zugriff auf ein anderes Portfolio der IBM Cloud-Infrastruktur (SoftLayer) festlegen.</td>
<td><code>[ibmcloud ks credential-set](/docs/containers?topic=containers-cs_cli_reference#cs_credentials_set)</code></td>
<td><code>[POST /v1/credentials](https://containers.cloud.ibm.com/swagger-api/#!/clusters/accounts/StoreUserCredentials)</code></td>
</tr>
<tr>
<td>Infrastrukturberechtigungsnachweise für das {{site.data.keyword.Bluemix_notm}}-Konto für den Zugriff auf ein anderes Portfolio der IBM Cloud-Infrastruktur (SoftLayer) entfernen.</td>
<td><code>[ibmcloud ks credential-unset](/docs/containers?topic=containers-cs_cli_reference#cs_credentials_unset)</code></td>
<td><code>[DELETE /v1/credentials](https://containers.cloud.ibm.com/swagger-api/#!/clusters/accounts/RemoveUserCredentials)</code></td>
</tr>
<tr>
<td>Geheimen Kubernetes-Schlüssel mithilfe von {{site.data.keyword.keymanagementservicefull}} verschlüsseln.</td>
<td><code>[ibmcloud ks key-protect-enable](/docs/containers?topic=containers-cs_cli_reference#cs_messages)</code></td>
<td><code>[POST /v1/clusters/{idOrName}/kms](https://containers.cloud.ibm.com/swagger-api/#!/clusters/CreateKMSConfig)</code></td>
</tr>
</tbody>
</table>

<table>
<caption>Übersicht über CLI-Befehle und API-Aufrufe für Ingress-Aktionen, die die Plattformrolle eines Administrators in {{site.data.keyword.containerlong_notm}} erfordern</caption>
<thead>
<th id="admin-ingress">Ingress-Aktion</th>
<th id="admin-cli2">CLI-Befehl</th>
<th id="admin-api2">API-Aufruf</th>
</thead>
<tbody>
<tr>
<td>Zertifikat aus der {{site.data.keyword.cloudcerts_long_notm}}-Instanz für eine Lastausgleichsfunktion für Anwendungen (ALB) bereitstellen</td>
<td><code>[ibmcloud ks alb-cert-deploy](/docs/containers?topic=containers-cs_cli_reference#cs_alb_cert_deploy)</code></td>
<td><code>[POST /albsecrets](https://containers.cloud.ibm.com/swagger-alb-api/#!/alb/CreateALBSecret) oder [PUT /albsecrets](https://containers.cloud.ibm.com/swagger-alb-api/#!/alb/UpdateALBSecret)</code></td>
</tr>
<tr>
<td>Details zu einem geheimen ALB-Schlüssel in einem Cluster anzeigen</td>
<td><code>[ibmcloud ks alb-cert-get](/docs/containers?topic=containers-cs_cli_reference#cs_alb_cert_get)</code></td>
<td><code>[GET /clusters/{idOrName}/albsecrets](https://containers.cloud.ibm.com/swagger-alb-api/#!/alb/ViewClusterALBSecrets)</code></td>
</tr>
<tr>
<td>Geheimen ALB-Schlüssel aus einem Cluster entfernen</td>
<td><code>[ibmcloud ks alb-cert-rm](/docs/containers?topic=containers-cs_cli_reference#cs_alb_cert_rm)</code></td>
<td><code>[DELETE /clusters/{idOrName}/albsecrets](https://containers.cloud.ibm.com/swagger-alb-api/#!/alb/DeleteClusterALBSecrets)</code></td>
</tr>
<tr>
<td>Alle geheimen ALB-Schlüssel in einem Cluster auflisten</td>
<td><code>[ibmcloud ks alb-certs](/docs/containers?topic=containers-cs_cli_reference#cs_alb_certs)</code></td>
<td>-</td>
</tr>
</tbody>
</table>

<table>
<caption>Übersicht über CLI-Befehle und API-Aufrufe für Protokollierungsaktionen, die die Plattformrolle eines Administrators in {{site.data.keyword.containerlong_notm}} erfordern</caption>
<thead>
<th id="admin-log">Protokollierungsaktion</th>
<th id="admin-cli3">CLI-Befehl</th>
<th id="admin-api3">API-Aufruf</th>
</thead>
<tbody>
<tr>
<td>Automatische Aktualisierungen für das Fluentd-Cluster-Add-on inaktivieren</td>
<td><code>[ibmcloud ks logging-autoupdate-disable](/docs/containers?topic=containers-cs_cli_reference#cs_log_autoupdate_disable)</code></td>
<td><code>[PUT /v1/logging/{idOrName}/updatepolicy](https://containers.cloud.ibm.com/swagger-logging/#!/logging/ChangeUpdatePolicy)</code></td>
</tr>
<tr>
<td>Automatische Aktualisierungen für das Fluentd-Cluster-Add-on aktivieren</td>
<td><code>[ibmcloud ks logging-autoupdate-enable](/docs/containers?topic=containers-cs_cli_reference#cs_log_autoupdate_enable)</code></td>
<td><code>[PUT /v1/logging/{idOrName}/updatepolicy](https://containers.cloud.ibm.com/swagger-logging/#!/logging/ChangeUpdatePolicy)</code></td>
</tr>
<tr>
<td>Snapshot der API-Serverprotokolle in einem {{site.data.keyword.cos_full_notm}}-Bucket erfassen</td>
<td><code>[ibmcloud ks logging-collect](/docs/containers?topic=containers-cs_cli_reference#cs_log_collect)</code></td>
<td>-</td>
</tr>
<tr>
<td>Status des Snapshots der API-Serverprotokolle anzeigen</td>
<td><code>[ibmcloud ks logging-collect-status](/docs/containers?topic=containers-cs_cli_reference#cs_log_collect_status)</code></td>
<td>-</td>
</tr>
<tr>
<td>Protokollweiterleitungskonfiguration für die Protokollquelle <code>kube-audit</code> erstellen</td>
<td><code>[ibmcloud ks logging-config-create](/docs/containers?topic=containers-cs_cli_reference#cs_logging_create)</code></td>
<td><code>[POST /v1/logging/{idOrName}/loggingconfig/{logSource}](https://containers.cloud.ibm.com/swagger-logging/#!/logging/CreateLoggingConfig)</code></td>
</tr>
<tr>
<td>Protokollweiterleitungskonfiguration für die Protokollquelle <code>kube-audit</code> löschen</td>
<td><code>[ibmcloud ks logging-config-rm](/docs/containers?topic=containers-cs_cli_reference#cs_logging_rm)</code></td>
<td><code>[DELETE /v1/logging/{idOrName}/loggingconfig/{logSource}/{id}](https://containers.cloud.ibm.com/swagger-logging/#!/logging/DeleteLoggingConfig)</code></td>
</tr>
</tbody>
</table>

<br />


## {{site.data.keyword.Bluemix_notm}} IAM-Servicerollen
{: #service}

Jedem Benutzer, dem eine {{site.data.keyword.Bluemix_notm}} IAM-Servicezugriffsrolle zugeordnet ist, wird automatisch auch eine entsprechende Rolle für Kubernetes-RBAC (RBAC - rollenbasierte Zugriffssteuerung) in einem bestimmten Namensbereich zugeordnet. Weitere Informationen zu Servicezugriffsrollen finden Sie unter [{{site.data.keyword.Bluemix_notm}} IAM-Servicerollen](/docs/containers?topic=containers-users#platform). Weisen Sie {{site.data.keyword.Bluemix_notm}} IAM-Plattformrollen nicht gleichzeitig mit einer Servicerolle zu. Sie müssen Plattform- und Servicerollen separat zuweisen.{: shortdesc}

Suchen Sie nach Informationen dazu, welche Kubernetes-Aktionen die einzelnen Servicerollen durch RBAC erteilen? Siehe [Kubernetes-Ressourcenberechtigungen für RBAC-Rollen](#rbac_ref). Weitere Informationen zu RBAC-Rollen finden Sie unter [RBAC-Berechtigungen zuordnen](/docs/containers?topic=containers-users#role-binding).
{: tip}

In der folgenden Tabelle sind die Kubernetes-Ressourcenberechtigungen aufgeführt, die durch die einzelnen Servicerollen und die entsprechenden RBAC-Rollen erteilt werden.

<table>
<caption>Kubernetes-Ressourcenberechtigungen für Servicerollen und entsprechende RBAC-Rollen</caption>
<thead>
    <th id="service-role">Servicerolle</th>
    <th id="rbac-role">Entsprechende RBAC-Rolle, Bindung und Bereich</th>
    <th id="kube-perm">Kubernetes-Ressourcenberechtigungen</th>
</thead>
<tbody>
  <tr>
    <td id="service-role-reader" headers="service-role">Leseberechtigter (Reader)</td>
    <td headers="service-role-reader rbac-role">Bei Geltung für nur einen Namensbereich: Die Clusterrolle <strong><code>view</code></strong> wird durch die Rollenbindung <strong><code>ibm-view</code></strong> in diesem Namensbereich angewendet</br><br>Bei Geltung für alle Namensbereiche: Die Clusterrolle <strong><code>view</code></strong> wird durch die Rollenbindung <strong><code>ibm-view</code></strong> in jedem Namensbereich des Clusters angewendet.</td>
    <td headers="service-role-reader kube-perm"><ul>
      <li>Lesezugriff auf Ressourcen in einem Namensbereich</li>
      <li>Kein Lesezugriff auf Rollen und Rollenbindungen oder auf geheime Kubernetes-Schlüssel</li>
      <li>Zugriff auf das Kubernetes-Dashboard zum Anzeigen von Ressourcen in einem Namensbereich</li></ul>
    </td>
  </tr>
  <tr>
    <td id="service-role-writer" headers="service-role">Schreibberechtigter (Writer)</td>
    <td headers="service-role-writer rbac-role">Bei Geltung für nur einen Namensbereich: Die Clusterrolle <strong><code>edit</code></strong> wird durch die Rollenbindung <strong><code>ibm-edit</code></strong> in diesem Namensbereich angewendet</br><br>Bei Geltung für alle Namensbereiche: Die Clusterrolle <strong><code>edit</code></strong> wird durch die Rollenbindung <strong><code>ibm-edit</code></strong> in jedem Namensbereich des Clusters angewendet.</td>
    <td headers="service-role-writer kube-perm"><ul><li>Schreib-/Lesezugriff auf Ressourcen in einem Namensbereich</li>
    <li>Kein Schreib-/Lesezugriff auf Rollen und Rollenbindungen</li>
    <li>Zugriff auf das Kubernetes-Dashboard zum Anzeigen von Ressourcen in einem Namensbereich</li></ul>
    </td>
  </tr>
  <tr>
    <td id="service-role-manager" headers="service-role">Manager</td>
    <td headers="service-role-manager rbac-role">Bei Geltung für nur einen Namensbereich: Die Clusterrolle <strong><code>admin</code></strong> wird durch die Rollenbindung <strong><code>ibm-operate</code></strong> in diesem Namensbereich angewendet</br><br>Bei Geltung für alle Namensbereiche: Die Clusterrolle <strong><code>cluster-admin</code></strong> wird durch die Clusterrollenbindung <strong><code>ibm-admin</code></strong> angewendet, die für alle Namensbereiche gilt.</td>
    <td headers="service-role-manager kube-perm">Bei Geltung für nur einen Namensbereich:
      <ul><li>Schreib-/Lesezugriff auf alle Ressourcen in einem Namensbereich, jedoch nicht auf Ressourcenquoten oder den Namensbereich selbst</li>
      <li>RBAC-Rollen und -Rollenbindungen in einem Namensbereich erstellen</li>
      <li>Zugriff auf das Kubernetes-Dashboard zum Anzeigen aller Ressourcen in einem Namensbereich</li></ul>
    </br>Bei Geltung für alle Namensbereiche:
        <ul><li>Schreib-/Lesezugriff auf alle Ressourcen in jedem Namensbereich</li>
        <li>RBAC-Rollen und -Rollenbindungen in einem Namensbereich oder Clusterrollen und Clusterrollenbindungen in allen Namensbereichen erstellen</li>
        <li>Zugriff auf das Kubernetes-Dashboard</li>
        <li>Ingress-Ressource erstellen, durch die Anwendungen öffentlich verfügbar gemacht werden</li>
        <li>Clustermetriken prüfen, z. B. durch die Befehle <code>kubectl top pods</code>, <code>kubectl top nodes</code> oder <code>kubectl get nodes</code></li></ul>
    </td>
  </tr>
</tbody>
</table>

<br />


## Kubernetes-Ressourcenberechtigungen für RBAC-Rollen
{: #rbac_ref}

Jedem Benutzer, dem eine {{site.data.keyword.Bluemix_notm}} IAM-Servicezugriffsrolle zugeordnet ist, wird automatisch auch eine entsprechende vordefinierte Rolle für Kubernetes-RBAC (RBAC - rollenbasierte Zugriffssteuerung) zugeordnet. Wenn Sie eigene angepasste Kubernetes-RBAC-Rollen verwalten wollen, finden Sie entsprechende Informationen in [Angepasste RBAC-Berechtigungen für Benutzer, Gruppen oder Servicekonten erstellen](/docs/containers?topic=containers-users#rbac).
{: shortdesc}

Fragen Sie sich, ob Sie über die richtigen Berechtigungen zum Ausführen eines bestimmten `kubectl`-Befehls für eine Ressource in einem Namensbereich haben? Probieren Sie den Befehl [`kubectl auth can-i` ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#-em-can-i-em-) aus.
{: tip}

In der folgenden Tabelle sind die Berechtigungen aufgeführt, die durch jede RBAC-Rolle für einzelne Kubernetes-Ressourcen erteilt werden. Für die Berechtigungen wird angegeben, welche Verben ein Benutzer mit der jeweiligen Rolle für die Ressource ausführen kann, wie zum Beispiel: "get", "list", "describe", "create" oder "delete".

<table>
 <caption>Kubernetes-Ressourcenberechtigungen, die durch die vordefinierten RBAC-Rollen erteilt werden</caption>
 <thead>
  <th>Kubernetes-Ressource</th>
  <th><code>view</code></th>
  <th><code>edit</code></th>
  <th><code>admin</code> und <code>cluster-admin</code></th>
 </thead>
<tbody>
<tr>
  <td><code>bindings</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
</tr><tr>
  <td><code>configmaps</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>cronjobs.batch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>daemonsets.apps </code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>daemonsets.extensions</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>deployments.apps</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>deployments.apps/rollback</code></td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>deployments.apps/scale</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>deployments.extensions</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>deployments.extensions/rollback</code></td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>deployments.extensions/scale</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>endpoints</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>events</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
</tr><tr>
  <td><code>horizontalpodautoscalers.autoscaling</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>ingresses.extensions</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>jobs.batch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>limitranges</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
</tr><tr>
  <td><code>localsubjectaccessreviews</code></td>
  <td>-</td>
  <td>-</td>
  <td><code>create</code></td>
</tr><tr>
  <td><code>namespaces</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></br>**nur cluster-admin:** <code>create</code>, <code>delete</code></td>
</tr><tr>
  <td><code>namespaces/status</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
</tr><tr>
  <td><code>networkpolicies</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>networkpolicies.extensions</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>persistentvolumeclaims</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>poddisruptionbudgets.policy</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>pods</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>top</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>pods/attach</code></td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>pods/exec</code></td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>pods/log</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
</tr><tr>
  <td><code>pods/portforward</code></td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>pods/proxy</code></td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>pods/status</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
</tr><tr>
  <td><code>replicasets.apps</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>replicasets.apps/scale</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>replicasets.extensions</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>replicasets.extensions/scale</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>replicationcontrollers</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>replicationcontrollers/scale</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>replicationcontrollers/status</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
</tr><tr>
  <td><code>replicationcontrollers.extensions/scale</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>resourcequotas</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
</tr><tr>
  <td><code>resourcequotas/status</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
</tr><tr>
  <td><code>rolebindings</code></td>
  <td>-</td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>roles</code></td>
  <td>-</td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>secrets</code></td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>serviceaccounts</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code>, <code>impersonate</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code>, <code>impersonate</code></td>
</tr><tr>
  <td><code>Services</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>services/proxy</code></td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>statefulsets.apps</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>statefulsets.apps/scale</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr>
</tbody>
</table>

<br />


## Cloud Foundry-Rollen
{: #cloud-foundry}

Cloud Foundry-Rollen gewähren Zugriff auf Organisationen und Bereiche innerhalb des Kontos. Wenn Sie die Liste der Cloud Foundry-basierten Services in {{site.data.keyword.Bluemix_notm}} anzeigen möchten, führen Sie `ibmcloud service list` aus. Weitere Informationen finden Sie unter den verfügbaren [Organisations- und Benutzerrollen](/docs/iam?topic=iam-cfaccess) oder in den Schritten unter [Cloud Foundry-Zugriff verwalten](/docs/iam?topic=iam-mngcf) in der Dokumentation zu {{site.data.keyword.Bluemix_notm}} Identity and Access Management.
{: shortdesc}

In der folgenden Tabelle werden die Cloud Foundry-Rollen aufgeführt, die für Clusteraktionsberechtigungen erforderlich sind.

<table>
  <caption>Berechtigungen zur Clusterverwaltung für Cloud Foundry-Rollen</caption>
  <thead>
    <th>Cloud Foundry-Rolle</th>
    <th>Berechtigungen der Clusterverwaltung</th>
  </thead>
  <tbody>
  <tr>
    <td>Bereichsrolle: Manager</td>
    <td>Benutzerzugriff auf einen {{site.data.keyword.Bluemix_notm}}-Bereich verwalten</td>
  </tr>
  <tr>
    <td>Bereichsrolle: Entwickler</td>
    <td>
      <ul><li>{{site.data.keyword.Bluemix_notm}}-Serviceinstanzen erstellen</li>
      <li>{{site.data.keyword.Bluemix_notm}}-Serviceinstanzen an Cluster binden</li>
      <li>Protokolle aus einer Protokollweiterleitungskonfiguration des Clusters auf Bereichsebene anzeigen</li></ul>
    </td>
  </tr>
  </tbody>
</table>

## Infrastrukturrollen
{: #infra}

Wenn ein Benutzer mit der Infrastrukturzugriffsrolle **Superuser** [den API-Schlüssel für eine Region und eine Ressourcengruppe festlegt](/docs/containers?topic=containers-users#api_key), werden die Infrastrukturberechtigungen für die anderen Benutzer im Konto mithilfe von {{site.data.keyword.Bluemix_notm}} IAM-Plattformrollen festgelegt. Sie müssen die Berechtigungen der Benutzer für die IBM Cloud-Infrastruktur (SoftLayer) nicht bearbeiten. Verwenden Sie nur die folgende Tabelle zum Anpassen der Benutzerberechtigungen für die IBM Cloud-Infrastruktur (SoftLayer), wenn Sie **Superuser** nicht dem Benutzer zuordnen können, der den API-Schlüssel festgelegt hat. Weitere Informationen finden Sie in [Infrastrukturberechtigungen anpassen](/docs/containers?topic=containers-users#infra_access).
{: shortdesc}

In der folgenden Tabelle werden die Infrastrukturberechtigungen aufgeführt, die zur Ausführung von Gruppen allgemeiner Tasks erforderlich sind.

<table>
 <caption>Allgemein erforderliche Infrastrukturberechtigungen für {{site.data.keyword.containerlong_notm}}</caption>
 <thead>
  <th>Allgemeine Tasks in {{site.data.keyword.containerlong_notm}}</th>
  <th>Erforderliche Infrastrukturberechtigungen nach Kategorie</th>
 </thead>
 <tbody>
   <tr>
     <td><strong>Mindestberechtigungen</strong>: <ul><li>Erstellen Sie einen Cluster.</li></ul></td>
     <td><strong>Geräte</strong>:<ul><li>Details zum virtuellen Server anzeigen</li><li>Server neu starten und IPMI-Systeminformationen anzeigen</li><li>Befehle zum erneuten Laden des Betriebssystems absetzen und Rescue-Kernel initiieren</li></ul><strong>Konto</strong>: <ul><li>Server hinzufügen</li></ul></td>
   </tr>
   <tr>
     <td><strong>Clusterverwaltung</strong>: <ul><li>Cluster erstellen, aktualisieren und löschen.</li><li>Workerknoten hinzufügen, erneut laden und neu starten.</li><li>VLANs anzeigen.</li><li>Teilnetze erstellen-</li><li>Pods und Lastausgleichsservices  bereitstellen.</li></ul></td>
     <td><strong>Support</strong>:<ul><li>Tickets anzeigen</li><li>Tickets hinzufügen</li><li>Tickets bearbeiten</li></ul>
     <strong>Geräte</strong>:<ul><li>Hardwaredetails anzeigen</li><li>Details zum virtuellen Server anzeigen</li><li>Server neu starten und IPMI-Systeminformationen anzeigen</li><li>Befehle zum erneuten Laden des Betriebssystems absetzen und Rescue-Kernel initiieren</li></ul>
     <strong>Netz</strong>:<ul><li>Berechnen mit öffentlichem Netzport hinzufügen</li></ul>
     <strong>Konto</strong>:<ul><li>Server abbrechen</li><li>Server hinzufügen</li></ul></td>
   </tr>
   <tr>
     <td><strong>Speicher</strong>: <ul><li>Persistente Datenträgeranforderungen zur Bereitstellung von persistenten Datenträgern erstellen</li><li>Speicherinfrastrukturressourcen erstellen und verwalten.</li></ul></td>
     <td><strong>Services</strong>:<ul><li>Speicher verwalten</li></ul><strong>Konto</strong>:<ul><li>Speicher hinzufügen</li></ul></td>
   </tr>
   <tr>
     <td><strong>Privates Netz</strong>: <ul><li>Private VLANs für Netzbetrieb im Cluster verwalten</li><li>VPN-Konnektivität für private Netze konfigurieren</li></ul></td>
     <td><strong>Netz</strong>:<ul><li>Teilnetzrouten im Netz verwalten</li></ul></td>
   </tr>
   <tr>
     <td><strong>Öffentliches Netz</strong>:<ul><li>Richten Sie eine öffentliche Lastausgleichsfunktion oder das Ingress-Networking ein, um Apps bereitzustellen.</li></ul></td>
     <td><strong>Geräte</strong>:<ul><li>Hostname/Domäne bearbeiten</li><li>Portsteuerung verwalten</li></ul>
     <strong>Netz</strong>:<ul><li>Berechnen mit öffentlichem Netzport hinzufügen</li><li>Teilnetzrouten im Netz verwalten</li><li>IP-Adressen hinzufügen</li></ul>
     <strong>Services</strong>:<ul><li>DNS, Reverse DNS und WHOIS verwalten</li><li>Zertifikate anzeigen (SSL)</li><li>Zertifikate verwalten (SSL)</li></ul></td>
   </tr>
 </tbody>
</table>
