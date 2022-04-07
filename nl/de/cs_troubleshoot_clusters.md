---

copyright:
  years: 2014, 2019
lastupdated: "2019-04-18"

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


# Fehlerbehebung für Cluster und Workerknoten
{: #cs_troubleshoot_clusters}

Ziehen Sie bei der Verwendung von {{site.data.keyword.containerlong}} die folgenden Verfahren für die Fehlerbehebung Ihrer Cluster und Workerknoten in Betracht.
{: shortdesc}

<p class="tip">Wenn Sie ein allgemeineres Problem haben, testen Sie das [Cluster-Debugging](/docs/containers?topic=containers-cs_troubleshoot).<br>Zudem können Sie bei der Fehlerbehebung [{{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool](/docs/containers?topic=containers-cs_troubleshoot#debug_utility) verwenden, um Tests durchzuführen und relevante Informationen aus Ihrem Cluster zu erfassen.</p>

## Erstellen eines Clusters oder Verwalten von Workerknoten aufgrund von Berechtigungsfehlern nicht möglich
{: #cs_credentials}

{: tsSymptoms}
Sie versuchen, Workerknoten für einen neuen oder einen vorhandenen Cluster mit einem der folgenden Befehle zu verwalten.
* Workerknoten bereitstellen: `ibmcloud ks cluster-create`, `ibmcloud ks worker-pool-rebalance` oder `ibmcloud ks worker-pool-resize`
* Workerknoten erneut laden: `ibmcloud ks worker-reload` oder `ibmcloud ks worker-update`
* Workerknoten erneut starten: `ibmcloud ks worker-reboot`
* Workerknoten löschen: `ibmcloud ks cluster-rm`, `ibmcloud ks worker-rm`, `ibmcloud ks worker-pool-rebalance` oder `ibmcloud ks worker-pool-resize`

Sie empfangen jedoch eine Fehlernachricht ähnlich einer der folgenden.

```
Es konnte keine Verbindung zu Ihrem Konto der IBM Cloud-Infrastruktur (SoftLayer) hergestellt werden.
Zur Erstellung eines Standardclusters ist es erforderlich, dass Sie entweder über ein nutzungsabhängiges Konto
mit einer Verbindung zum Konto der IBM Cloud-Infrastruktur (SoftLayer) verfügen oder
dass Sie die {{site.data.keyword.containerlong_notm}}-CLI
zum Einrichten der API-Schlüssel für die {{site.data.keyword.Bluemix_notm}}-Infrastruktur verwendet haben.
```
{: screen}

```
{{site.data.keyword.Bluemix_notm}} Infrastructure Exception:
'Item' must be ordered with permission.
```
{: screen}

```
{{site.data.keyword.Bluemix_notm}} Infrastructure Exception:
The user does not have the necessary {{site.data.keyword.Bluemix_notm}}
Infrastructure permissions to add servers
```
{: screen}

```
IAM token exchange request failed: Cannot create IMS portal token, as no IMS account is linked to the selected BSS account
```
{: screen}

```
Der Cluster konnte nicht mit der Registry konfiguriert werden. Stellen Sie sicher, dass Sie über die Administratorrolle für {{site.data.keyword.registrylong_notm}} verfügen.
```
{: screen}

{: tsCauses}
Sie verfügen nicht über die korrekten Berechtigungen zum Erstellen eines Clusters. Zum Erstellen eines Clusters benötigen Sie die folgenden Berechtigungen:
*  Die Rolle **Superuser** für die IBM Cloud-Infrastruktur (SoftLayer) oder mindestens [diese Infrastruktur-Mindestberechtigungen](/docs/containers?topic=containers-access_reference#infra).
*  Die Plattformmanagementrolle **Administrator** für {{site.data.keyword.containerlong_notm}} auf Kontoebene.
*  Die Plattformmanagementrolle **Administrator** für {{site.data.keyword.registrylong_notm}} auf Kontoebene. Schränken Sie Richtlinien für {{site.data.keyword.registryshort_notm}} nicht auf Ressourcengruppenebene ein. Wenn Sie vor dem 4. Oktober 2018 mit der Verwendung von {{site.data.keyword.registrylong_notm}} begonnen haben, stellen Sie sicher, dass Sie die [{{site.data.keyword.Bluemix_notm}} IAM-Richtliniendurchsetzung aktivieren](/docs/services/Registry?topic=registry-user#existing_users).

Aufgrund von infrastrukturbezogenen Fehlern wurde nutzungsabhängigen {{site.data.keyword.Bluemix_notm}}-Konten, die nach der Aktivierung der automatischen Kontoverknüpfung erstellt wurden, bereits Zugriff auf das Portfolio der IBM Cloud-Infrastruktur (SoftLayer) gewährt. Somit können Sie Infrastrukturressourcen für Ihren Cluster ohne weitere Konfiguration kaufen. Wenn Sie über ein gültiges nutzungsabhängiges Konto verfügen und diese Fehlernachricht erhalten, verwenden Sie möglicherweise nicht die richtigen Berechtigungsnachweise für das Konto der IBM Cloud-Infrastruktur (SoftLayer), um auf Infrastrukturressourcen zuzugreifen.

Benutzer mit anderen {{site.data.keyword.Bluemix_notm}}-Kontotypen müssen ihre Konten konfigurieren, um Standardcluster zu erstellen. Beispiele für den Fall, dass Sie einen anderen Kontotyp haben:
* Sie haben ein IBM Cloud Infrastructure-Konto (SoftLayer), das älter als Ihr {{site.data.keyword.Bluemix_notm}}-Plattformkonto ist, und Sie möchten es weiterhin verwenden.
* Sie möchten ein anderes IBM Cloud-Infrastrukturkonto (SoftLayer) verwenden, in dem die Infrastrukturressourcen eingerichtet werden sollen. Sie können beispielsweise ein {{site.data.keyword.Bluemix_notm}}-Teamkonto einrichten, um ein anderes Infrastrukturkonto für Abrechnungszwecke zu verwenden.

Falls Sie ein abweichendes IBM Cloud-Infrastrukturkonto (SoftLayer) zum Bereitstellen von Infrastrukturressourcen verwenden, verfügen Sie in Ihrem Konto möglicherweise auch über [verwaiste Cluster](#orphaned).

{: tsResolve}
Der Kontoeigner muss die Berechtigungsnachweise für das Infrastrukturkonto ordnungsgemäß einrichten. Die Berechtigungsnachweise richten sich nach dem Typ des von Ihnen verwendeten Infrastrukturkontos.

1.  Überprüfen Sie, ob Sie Zugriff auf ein Infrastrukturkonto haben. Melden Sie sich bei der [{{site.data.keyword.Bluemix_notm}}-Konsole![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/) an. Klicken Sie im Menü ![Menüsymbol](../icons/icon_hamburger.svg "Menüsymbol") auf **Klassische Infrastruktur**. Wenn ein Menü angezeigt wird, haben Sie Zugriff auf ein Infrastrukturkonto. Haben Sie keinen Zugriff, wird die Option zum Durchführen eines Upgrades Ihres Kontos angezeigt.
2.  Überprüfen Sie, ob Ihr Cluster ein anderes Infrastrukturkonto verwendet als das zusammen mit Ihrem nutzungsabhängigen Konto gelieferte Konto.
    1.  Klicken Sie im Menü ![Menüsymbol](../icons/icon_hamburger.svg "Menüsymbol") auf **Kubernetes > Cluster**.
    2.  Wählen Sie Ihren Cluster in der Tabelle aus.
    3.  Suchen Sie auf der Registerkarte **Übersicht** nach dem Feld **Infrastrukturbenutzer**.
        * Wenn das Feld **Infrastrukturbenutzer** nicht angezeigt wird, verfügen Sie über ein verlinktes nutzungsabhängiges Konto, das für Ihre Infrastruktur- und Plattformkonten dieselben Berechtigungsnachweise verwendet.
        * Wenn ein Feld für den **Infrastrukturbenutzer** angezeigt wird, verwendet der Cluster ein anderes Infrastrukturkonto als das zusammen mit Ihrem nutzungsabhängigen Konto gelieferte Konto. Diese verschiedenen Berechtigungsnachweise gelten für alle Cluster in der Region.
3.  Entscheiden Sie, welche Art von Konto Sie verwenden möchten, um festzustellen, wie Sie das Problem mit der Infrastrukturberechtigung beheben können. Für die meisten Benutzer ist das standardmäßig verknüpfte nutzungsabhängige Konto ausreichend.
    *  Verknüpftes nutzungsabhängiges {{site.data.keyword.Bluemix_notm}}-Konto: [Stellen Sie sicher, dass der API-Schlüssel mit den korrekten Berechtigungen konfiguriert ist](/docs/containers?topic=containers-users#default_account). Wenn Ihr Cluster ein anderes Infrastrukturkonto verwendet, müssen Sie diese Berechtigungsnachweise im Rahmen des Prozesses inaktivieren.
    *  Andere {{site.data.keyword.Bluemix_notm}}-Plattform- und Infrastrukturkonten: Überprüfen Sie, ob Sie auf das Infrastruktur-Portfolio zugreifen können und dass [die Berechtigungsnachweise für das Infrastrukturkonto mit den richtigen Berechtigungen eingerichtet sind](/docs/containers?topic=containers-users#credentials).
4.  Wenn die Workerknoten des Clusters nicht im Infrastrukturkonto angezeigt werden, sollte Sie überprüfen, ob der [Cluster verwaist ist](#orphaned).

<br />


## Firewall verhindert das Ausführen von CLI-Befehlen
{: #ts_firewall_clis}

{: tsSymptoms}
Wenn Sie die Befehle `ibmcloud`, `kubectl` oder `calicoctl` über die Befehlszeilenschnittstelle ausführen, schlagen sie fehl.

{: tsCauses}
Möglicherweise verhindern Unternehmensnetzrichtlinien den Zugriff von Ihrem lokalen System auf öffentliche Endpunkte über Proxys oder Firewalls.

{: tsResolve}
[Lassen Sie TCP-Zugriff zu, damit die CLI-Befehle ausgeführt werden können](/docs/containers?topic=containers-firewall#firewall_bx). Diese Task setzt voraus, dass die [{{site.data.keyword.Bluemix_notm}} IAM-Plattformrolle **Administrator**](/docs/containers?topic=containers-users#platform) für den Cluster zugewiesen ist.


## Zugriff auf Ressourcen im Cluster nicht möglich
{: #cs_firewall}

{: tsSymptoms}
Wenn die Workerknoten in Ihrem Cluster nicht über das private Netz kommunizieren können, dann stellen Sie möglicherweise eine Reihe unterschiedlicher Symptome fest.

- Beispielfehlernachricht bei Ausführung von `kubectl exec`, `attach`, `logs`, `proxy` oder `port-forward`:
  ```
  Error from server: error dialing backend: dial tcp XXX.XXX.XXX:10250: getsockopt: connection timed out
  ```
  {: screen}

- Beispielfehlernachricht, wenn `kubectl proxy` erfolgreich ist, aber das Kubernetes-Dashboard nicht verfügbar ist:
  ```
  timeout on 172.xxx.xxx.xxx
  ```
  {: screen}

- Beispielfehlernachricht, wenn `kubectl proxy` fehlschlägt oder die Verbindung zu Ihrem Service nicht hergestellt werden kann:
  ```
  Connection refused
  ```
  {: screen}

  ```
  Connection timed out
  ```
  {: screen}

  ```
  Unable to connect to the server: net/http: TLS handshake timeout
  ```
  {: screen}


{: tsCauses}
Für den Zugriff auf Ressourcen im Cluster müssen Ihre Workerknoten in der Lage sein, über das private Netz zu kommunizieren. Sie haben möglicherweise Vyatta oder eine andere Firewall eingerichtet oder Ihre vorhandenen Firewalleinstellungen in Ihrem Konto für die IBM Cloud-Infrastruktur (SoftLayer) angepasst. {{site.data.keyword.containerlong_notm}} erfordert, dass bestimmte IP-Adressen und Ports geöffnet sind, damit die Kommunikation vom Workerknoten zum Kubernetes-Master und umgekehrt möglich ist. Wenn Ihre Workerknoten über mehrere Zonen verteilt sind, müssen Sie die Kommunikation über das private Netz durch das VLAN-Spanning einrichten. Die Kommunikation zwischen Workerknoten kann außerdem dann unmöglich sein, wenn Ihre Workerknoten in einer Neuladeschleife blockiert sind.

{: tsResolve}
1. Listen Sie die Workerknoten in Ihrem Cluster auf und überprüfen Sie, ob sie nicht im Status `Reloading` (wird erneut geladen) blockiert sind.
   ```
   ibmcloud ks workers <clustername_oder_-id>
   ```
   {: pre}

2. Wenn Sie einen Mehrzonencluster haben und Ihr Konto nicht für VRF aktiviert ist, überprüfen Sie, ob das [VLAN-Spanning](/docs/containers?topic=containers-subnets#subnet-routing) für Ihr Konto aktiviert ist.
3. Wenn Sie Vyatta oder angepasste Firewalleinstellungen haben, stellen Sie sicher, dass Sie die [erforderlichen Ports geöffnet](/docs/containers?topic=containers-firewall#firewall_outbound) haben, um zuzulassen, dass der Cluster auf die Infrastrukturressourcen und -services zugreift.

<br />



## Anzeigen eines Clusters oder Arbeiten mit Cluster nicht möglich
{: #cs_cluster_access}

{: tsSymptoms}
* Sie können einen Cluster nicht finden. Wenn Sie `ibmcloud ks clusters` ausführen, wird der Cluster nicht in der Ausgabe aufgeführt.
* Sie können nicht mit einem Cluster arbeiten. Wenn Sie `ibmcloud ks cluster-config` oder andere clusterspezifische Befehle ausführen, wird der Cluster nicht gefunden.


{: tsCauses}
In {{site.data.keyword.Bluemix_notm}} muss jede Ressource in einer Ressourcengruppe enthalten sein. Beispiel: Der Cluster `mycluster` ist möglicherweise in der Ressourcengruppe `default` enthalten. Wenn Ihnen der Kontoeigner Zugriff auf Ressourcen erteilt, indem er Ihnen eine {{site.data.keyword.Bluemix_notm}} IAM-Plattformrolle zuordnet, können Sie über Zugriff auf eine bestimmte Ressource oder Ressourcengruppe verfügen. Wenn Ihnen Zugriff auf eine bestimmte Ressource erteilt wird, verfügen Sie nicht über Zugriff auf die Ressourcengruppe. In diesem Fall müssen Sie nicht ein Ziel für die Ressourcengruppe angeben, um mit den Clustern zu arbeiten, auf die Sie zugreifen können. Wenn Sie eine andere Ressourcengruppe als die Gruppe angeben, in der sich der Cluster befindet, können Aktionen für diesen Cluster fehlschlagen. Wenn Sie im umgekehrten Fall im Rahmen des Zugriffs auf eine Ressourcengruppe Zugriff auf eine Ressource erhalten, müssen Sie eine Ressourcengruppe als Ziel für die Arbeit mit einem Cluster in dieser Gruppe angeben. Falls Sie die CLI-Sitzung nicht als Ziel für die Ressourcengruppe angeben, in der der Cluster enthalten ist, schlagen Aktionen für diesen Cluster fehl.

Wenn Sie einen Cluster nicht finden oder nicht mit einem Cluster arbeiten können, ist möglicherweise eines der folgenden Probleme aufgetreten:
* Sie haben Zugriff auf den Cluster und die Ressourcengruppe, in der sich der Cluster befindet, aber Ihre CLI-Sitzung ist nicht als Ziel für die Ressourcengruppe angegeben, in der sich der Cluster befindet.
* Sie haben Zugriff auf den Cluster, aber nicht als Teil der Ressourcengruppe, in der sich der Cluster befindet. Ihre CLI-Sitzung ist für diese oder eine andere Ressourcengruppe als Ziel angegeben.
* Sie verfügen nicht über Zugriff auf den Cluster.

{: tsResolve}
Gehen Sie wie folgt vor, um Ihre Zugriffsberechtigungen zu überprüfen:

1. Listen Sie alle Benutzerberechtigungen auf.
    ```
    ibmcloud iam user-policies <ihr_benutzername>
    ```
    {: pre}

2. Überprüfen Sie, ob Sie über Zugriff auf den Cluster und die Ressourcengruppe verfügen, in der sich der Cluster befindet.
    1. Suchen Sie eine Richtlinie, die für **Ressourcengruppenname** den Wert der Ressourcengruppe des Clusters und für **Memo** den Wert `Richtlinie gilt für die Ressourcengruppe` aufweist. Wenn diese Richtlinie vorhanden ist, verfügen Sie über Zugriff auf die Ressourcengruppe. Die folgende Richtlinie gibt zum Beispiel an, dass ein Benutzer über Zugriff auf die Ressourcengruppe `test-rg` verfügt:
        ```
        Policy ID:   3ec2c069-fc64-4916-af9e-e6f318e2a16c
        Roles:       Viewer
        Resources:
                     Resource Group ID     50c9b81c983e438b8e42b2e8eca04065
                     Resource Group Name   test-rg
                     Memo                  Policy applies to the resource group
        ```
        {: screen}
    2. Suchen Sie die Richtlinie, die für **Ressourcengruppenname** den Wert der Ressourcengruppe des Clusters, für **Servicename** den Wert `containera-kubernetes` oder keinen Wert und für **Memo** den Wert `Richtlinie gilt für Ressource(n) in der Ressourcengruppe` aufweist. Wenn diese Richtlinie vorhanden ist, verfügen Sie über Zugriff auf die Cluster oder auf alle Ressourcen in der Ressourcengruppe. Die folgende Richtlinie gibt zum Beispiel an, dass ein Benutzer über Zugriff auf Cluster in der Ressourcengruppe `test-rg` verfügt:
        ```
        Policy ID:   e0ad889d-56ba-416c-89ae-a03f3cd8eeea
        Roles:       Administrator
        Resources:
                     Resource Group ID     a8a12accd63b437bbd6d58fb6a462ca7
                     Resource Group Name   test-rg
                     Service Name          containers-kubernetes
                     Service Instance
                     Region
                     Resource Type
                     Resource
                     Memo                  Policy applies to the resource(s) within the resource group
        ```
        {: screen}
    3. Wenn Sie über diese beiden Richtlinien verfügen, fahren Sie unter Schritt 4 mit dem ersten Listenpunkt fort. Wenn Sie nicht über die Richtlinie aus Schritt 2a verfügen, aber über die Richtlinie aus Schritt 2b, fahren Sie unter Schritt 4 mit dem zweiten Listenpunkt fort. Wenn Sie über keine der beiden Richtlinien verfügen, fahren Sie mit Schritt 3 fort.

3. Überprüfen, ob Sie über Zugriff auf den Cluster verfügen, aber nicht im Rahmen des Zugriffs auf die Ressourcengruppe, in der sich der Cluster befindet.
    1. Suchen Sie die Richtlinie, die neben den Feldern **Richtlinien-ID** und **Rollen** keine Werte aufweist. Wenn Sie über diese Richtlinie verfügen, haben Sie im Rahmen des Zugriffs auf das gesamte Konto auch Zugriff auf den Cluster. Die folgende Richtlinie gibt zum Beispiel an, dass ein Benutzer über Zugriff auf alle Ressourcen im Konto verfügt:
        ```
        Policy ID:   8898bdfd-d520-49a7-85f8-c0d382c4934e
        Roles:       Administrator, Manager
        Resources:
                     Service Name
                     Service Instance
                     Region
                     Resource Type
                     Resource
        ```
        {: screen}
    2. Suchen Sie eine Richtlinie, die für **Servicename** den Wert `container-kubernetes` und für **Serviceinstanz** den Wert der Cluster-ID aufweist. Die Cluster-ID können Sie mit dem Befehl `ibmcloud ks cluster-get --cluster <clustername>` ermitteln. Die folgende Richtlinie gibt zum Beispiel an, dass ein Benutzer über Zugriff auf einen bestimmten Cluster verfügt:
        ```
        Policy ID:   140555ce-93ac-4fb2-b15d-6ad726795d90
        Roles:       Administrator
        Resources:
                     Service Name       containers-kubernetes
                     Service Instance   df253b6025d64944ab99ed63bb4567b6
                     Region
                     Resource Type
                     Resource
        ```
        {: screen}
    3. Falls Sie über eine dieser Richtlinien verfügen, überspringen Sie den zweiten Listenpunkt von Schritt 4; falls Sie über keine dieser Richtlinien verfügen, überspringen Sie den dritten Listenpunkt von Schritt 4.

4. Wählen Sie abhängig von den Zugriffsrichtlinien eine der folgenden Optionen aus.
    * Falls Sie über Zugriff auf den Cluster und die Ressourcengruppe verfügen, in der sich der Cluster befindet:
      1. Geben Sie als Ziel die Ressourcengruppe an. **Hinweis:** Sie können erst mit Clustern in anderen Ressourcengruppen arbeiten, wenn Sie die Angabe dieser Ressourcengruppe als Ziel zurücknehmen.
          ```
          ibmcloud target -g <ressourcengruppe>
          ```
          {: pre}

      2. Geben Sie den Cluster als Ziel an.
          ```
          ibmcloud ks cluster-config --cluster <clustername_oder_-id>
          ```
          {: pre}

    * Falls Sie über Zugriff auf den Cluster, aber  nicht auf die Ressourcengruppe verfügen, in der sich der Cluster befindet:
      1. Geben Sie keine Ressourcengruppe als Ziel an. Falls Sie bereits eine Ressourcengruppe als Ziel angegeben haben, machen Sie diese Angabe rückgängig.
        ```
        ibmcloud target -g none
        ```
        {: pre}
        Dieser Befehl schlägt fehl, weil keine Ressourcengruppe mit dem Namen `none` vorhanden ist. Die Angabe der aktuellen Ressourcengruppe als Ziel wird jedoch rückgängig gemacht, wenn der Befehl fehlschlägt.

      2. Geben Sie den Cluster als Ziel an.
        ```
        ibmcloud ks cluster-config --cluster <clustername_oder_-id>
        ```
        {: pre}

    * Wenn Sie keinen Zugriff auf den Cluster haben:
        1. Wenden Sie sich für die Zuordnung einer [{{site.data.keyword.Bluemix_notm}}IAM-Plattformrolle](/docs/containers?topic=containers-users#platform) für diesen Cluster an Ihren Kontoeigner.
        2. Geben Sie keine Ressourcengruppe als Ziel an. Falls Sie bereits eine Ressourcengruppe als Ziel angegeben haben, machen Sie diese Angabe rückgängig.
          ```
          ibmcloud target -g none
          ```
          {: pre}
          Dieser Befehl schlägt fehl, weil keine Ressourcengruppe mit dem Namen `none` vorhanden ist. Die Angabe der aktuellen Ressourcengruppe als Ziel wird jedoch rückgängig gemacht, wenn der Befehl fehlschlägt.
        3. Geben Sie den Cluster als Ziel an.
          ```
          ibmcloud ks cluster-config --cluster <clustername_oder_-id>
          ```
          {: pre}

<br />


## Zugreifen auf Workerknoten mit SSH schlägt fehl
{: #cs_ssh_worker}

{: tsSymptoms}
Es ist kein Zugriff auf Ihren Workerknoten über eine SSH-Verbindung möglich.

{: tsCauses}
Auf den Workerknoten ist SSH nach Kennwort nicht verfügbar.

{: tsResolve}
Verwenden Sie ein Kubernetes-[`DaemonSet` ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) für Aktionen, die Sie für jeden Knoten ausführen müssen, und verwenden Sie Jobs für Aktionen, die Sie einmalig ausführen müssen.

<br />


## ID der Bare-Metal-Instanz ist mit Workerdatensätzen inkonsistent
{: #bm_machine_id}

{: tsSymptoms}
Wenn Sie für Ihren Bare-Metal-Workerknoten `ibmcloud ks worker`-Befehle ausführen, wird eine Nachricht ähnlich der folgenden angezeigt.

```
Instance ID inconsistent with worker records
```
{: screen}

{: tsCauses}
Die Maschinen-ID und der {{site.data.keyword.containerlong_notm}}-Workerdatensatz können inkonsistent werden, wenn die Maschine Hardwareprobleme hat. Wenn die IBM Cloud-Infrastruktur (SoftLayer) dieses Problem behebt, kann sich eine Komponente innerhalb des Systems ändern, die der Service nicht identifiziert.

{: tsResolve}
Damit {{site.data.keyword.containerlong_notm}} die Maschine erneut identifizieren kann, [laden Sie den Bare-Metal-Workerknoten erneut](/docs/containers?topic=containers-cs_cli_reference#cs_worker_reload). **Hinweis**: Durch das erneute Laden wird außerdem die [Patchversion](/docs/containers?topic=containers-changelog) der Maschine aktualisiert.

Sie können auch [den Bare-Metal-Workerknoten löschen](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_rm). **Hinweis**: Die Abrechnung für Bare-Metal-Instanzen erfolgt monatlich.

<br />


## Ändern oder Löschen der Infrastruktur in verwaistem Cluster nicht möglich
{: #orphaned}

{: tsSymptoms}
Sie können im Cluster infrastrukturbezogene Befehle wie zum Beispiel die folgenden nicht ausführen:
* Workerknoten hinzufügen oder entfernen
* Workerknoten neu starten oder erneut laden
* Größe von Worker-Pools ändern
* Cluster aktualisieren

Sie können die Cluster-Workerknoten im IBM Cloud-Infrastrukturkonto (SoftLayer) nicht anzeigen. Sie können jedoch andere Cluster in dem Konto aktualisieren und verwalten.

Außerdem haben Sie sichergestellt, dass Sie über die [richtigen Berechtigungsnachweise für die Infrastruktur](#cs_credentials) verfügen.

{: tsCauses}
Der Cluster kann in einem IBM Cloud-Infrastrukturkonto (SoftLayer) bereitgestellt werden, das nicht mehr mit dem {{site.data.keyword.containerlong_notm}}-Konto verknüpft ist. Der Cluster ist verwaist. Da sich die Ressourcen in einem abweichenden Konto befinden, verfügen Sie nicht über die Berechtigungsnachweisen für die Infrastruktur zum Ändern der Ressourcen.

Im folgenden Szenario wird veranschaulicht, wie es dazu kommen kann, dass ein Cluster verwaist.
1.  Sie verfügen über ein nutzungsabhängiges {{site.data.keyword.Bluemix_notm}}-Konto.
2.  Sie erstellen einen Cluster mit dem Namen `Cluster1`. Die Workerknoten und weitere Infrastrukturressource werden im Infrastrukturkonto angegeben, das mit dem nutzungsabhängigen Konto verknüpft ist.
3.  Später stellen Sie fest, dass von Ihrem Team ein traditionelles oder gemeinsam genutztes IBM Cloud-Infrastrukturkonto (SoftLayer) verwendet wird. Sie verwenden den Befehl `ibmcloud ks credential-set`, um die Berechtigungsnachweise der IBM Cloud-Infrastruktur (SoftLayer) zum Verwenden des Teamkontos zu ändern.
4.  Sie erstellen einen weiteren Cluster mit dem Namen `Cluster2`. Die Workerknoten und weitere Infrastrukturressource werden im Infrastrukturkonto des Teams angegeben.
5.  Sie bemerken, dass für `Cluster1` eine Aktualisierung bzw. ein erneutes Laden der Workerknoten erforderlich ist oder Sie möchten einfach eine Bereinigung zum Löschen durchführen. Da `Cluster1` jedoch in einem unterschiedlichen Infrastrukturkonto angegeben wurde, können Sie seine Infrastrukturressource nicht ändern. `Cluster1` ist verwaist.
6.  Gehen Sie gemäß den folgenden Schritten zur Lösung dieses Problems vor, machen Sie jedoch nicht die Berechtigungsnachweise für die Infrastruktur für das Teamkonto rückgängig. Sie können zwar `Cluster1` löschen, dann ist jedoch `Cluster2` verwaist.
7.  Sie können die Berechtigungsnachweise für die Infrastruktur wieder in die des Teamkontos ändern, von dem `Cluster2` erstellt wurde. Jetzt verfügen Sie nicht mehr über einen verwaisten Cluster.

<br>

{: tsResolve}
1.  Überprüfen Sie, welches Infrastrukturkonto von der Region, in der sich der Cluster befindet, derzeit zum Bereitstellen der Cluster verwendet wird.
    1.  Melden Sie sich bei der [{{site.data.keyword.containerlong_notm}}-Clusterkonsole ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/kubernetes/clusters) an.
    2.  Wählen Sie Ihren Cluster in der Tabelle aus.
    3.  Suchen Sie auf der Registerkarte **Übersicht** nach dem Feld **Infrastrukturbenutzer**. Mithilfe dieses Felds können Sie feststellen, ob vom {{site.data.keyword.containerlong_notm}}-Konto ein anderes Infrastrukturkonto als das Standardkonto verwendet wird.
        * Wenn das Feld **Infrastrukturbenutzer** nicht angezeigt wird, verfügen Sie über ein verlinktes nutzungsabhängiges Konto, das für Ihre Infrastruktur- und Plattformkonten dieselben Berechtigungsnachweise verwendet. Der Cluster, der nicht geändert werden kann, kann in einem anderen Infrastrukturkonto bereitgestellt werden.
        * Wenn ein Feld für den **Infrastrukturbenutzer** angezeigt wird, verwenden Sie ein anderes Infrastrukturkonto als das, das mit dem nutzungsabhängigen Konto verknüpft ist. Diese verschiedenen Berechtigungsnachweise gelten für alle Cluster in der Region. Der Cluster, der nicht geändert werden kann, kann in einem nutzungsabhängigen Konto oder einem anderen Infrastrukturkonto bereitgestellt werden.
2.  Überprüfen Sie, welches Infrastrukturkonto zum Bereitstellen des Clusters verwendet wurde.
    1.  Wählen Sie in der Registerkarte **Workerknoten** einen Workerknoten aus und notieren Sie seine **ID**.
    2.  Öffnen Sie das Menü ![Menüsymbol](../icons/icon_hamburger.svg "Menüsymbol") und klicken Sie auf **Klassische Infrastruktur**.
    3.  Klicken Sie im Infrastrukturnavigationsbereich auf **Geräte > Geräteliste**.
    4.  Suchen Sie die Workerknoten-ID, die Sie vorher notiert haben.
    5.  Wenn Sie die Workerknoten-ID nicht finden, wird der Workerknoten in diesem Infrastrukturkonto nicht bereitgestellt. Wechseln Sie zu einem anderen Infrastrukturkonto und versuchen Sie es erneut.
3.  Verwenden Sie den [Befehl](/docs/containers?topic=containers-cs_cli_reference#cs_credentials_set) `ibmcloud ks credential-set`, um die Berechtigungsnachweise für die Infrastruktur in die Berechtigungsnachweise des Kontos zu ändern, in dem die Workerknoten des Clusters bereitgestellt werden, die Sie im vorherigen Schritt ermittelt haben.
    Falls Sie nicht mehr über Zugriff auf die Berechtigungsnachweise für die Infrastruktur verfügen und diese auch nicht abrufen können, müssen Sie einen {{site.data.keyword.Bluemix_notm}}-Supportfall öffnen, um den verwaisten Cluster zu entfernen.
    {: note}
4.  [Löschen Sie den Cluster](/docs/containers?topic=containers-clusters#remove).
5.  Setzen Sie die Berechtigungsnachweise für die Infrastruktur bei Bedarf auf die des vorherigen Kontos zurück. Beachten Sie hierbei Folgendes: Wenn Sie Cluster mit einem anderen Infrastrukturkonto als dem Konto, zu dem Sie gewechselt sind, erstellt haben, können diese Cluster verwaisen.
    * Wenn Sie die Berechtigungsnachweise eines anderen Infrastrukturkontos festlegen möchten, verwenden Sie den [Befehl](/docs/containers?topic=containers-cs_cli_reference#cs_credentials_set) `ibmcloud ks credential-set`.
    * Wenn Sie die Standardberechtigungsnachweise verwenden möchten, die mit Ihrem nutzungsabhängigen {{site.data.keyword.Bluemix_notm}}-Konto bereitgestellt werden, verwenden Sie den [Befehl](/docs/containers?topic=containers-cs_cli_reference#cs_credentials_unset) `ibmcloud ks credential-unset`.

<br />


## `kubectl`-Befehle überschreiten Zeitlimit
{: #exec_logs_fail}

{: tsSymptoms}
Wenn Sie Befehle wie `kubectl exec`, `kubectl attach`, `kubectl proxy`, `kubectl port-forward` oder `kubectl logs` ausführen, wird die folgende Nachricht angezeigt:

  ```
  <worker-ip>:10250: getsockopt: connection timed out
  ```
  {: screen}

{: tsCauses}
Die OpenVPN-Verbindung zwischen dem Masterknoten und dem Workerknoten funktioniert möglicherweise nicht ordnungsgemäß.

{: tsResolve}
1. Wenn Sie über mehrere VLANs für einen Cluster, mehrere Teilnetze in demselben VLAN oder einen Cluster mit mehreren Zonen verfügen, müssen Sie eine [VRF-Funktion (Virtual Router Function)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) für Ihr Konto für die IBM Cloud-Infrastruktur (SoftLayer) aktivieren, damit die Workerknoten über das private Netz miteinander kommunizieren können. Zur Aktivierung von VRF [wenden Sie sich an Ihren Ansprechpartner für die IBM Cloud-Infrastruktur (SoftLayer)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#how-you-can-initiate-the-conversion). Wenn Sie VRF nicht aktivieren können oder wollen, aktivieren Sie das [VLAN-Spanning](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning). Um diese Aktion durchführen zu können, müssen Sie über die [Infrastrukturberechtigung](/docs/containers?topic=containers-users#infra_access) **Netz > VLAN-Spanning im Netz verwalten** verfügen oder Sie können den Kontoeigner bitten, diese zu aktivieren. Zum Prüfen, ob das VLAN-Spanning bereits aktiviert ist, verwenden Sie den [Befehl](/docs/containers?topic=containers-cs_cli_reference#cs_vlan_spanning_get) `ibmcloud ks vlan-spanning-get`.
2. Starten Sie den OpenVPN-Client-Pod erneut.
  ```
  kubectl delete pod -n kube-system -l app=vpn
  ```
  {: pre}
3. Wenn weiterhin dieselbe Fehlernachricht angezeigt wird, kann es sein, dass der Workerknoten, auf dem sich der VPN-Pod befindet, nicht ordnungsgemäß funktioniert. Um den VPN-Pod erneut zu starten und ihn für einen anderen Workerknoten zu terminieren, [riegeln Sie den Workerknoten, auf dem sich der VPN-Pod befindet, ab, entleeren sie ihn und starten Sie ihn neu](/docs/containers?topic=containers-cs_cli_reference#cs_worker_reboot).

<br />


## Bindung eines Service an einen Cluster führt zu Fehler wegen identischer Namen
{: #cs_duplicate_services}

{: tsSymptoms}
Wenn Sie `ibmcloud ks cluster-service-bind <cluster_name> <namespace> <service_instance_name>` ausführen, wird die folgende Nachricht angezeigt:

```
Multiple services with the same name were found.
Run 'ibmcloud service list' to view available Bluemix service instances...
```
{: screen}

{: tsCauses}
Unter Umständen haben mehrere Serviceinstanzen in unterschiedlichen Regionen denselben Namen.

{: tsResolve}
Verwenden Sie im Befehl `ibmcloud ks cluster-service-bind` die GUID des Service anstelle des Serviceinstanznamens.

1. [Melden Sie sich in der Region an, in der sich die Serviceinstanz befindet, die gebunden werden soll.](/docs/containers?topic=containers-regions-and-zones#bluemix_regions)

2. Rufen Sie die GUID für die Serviceinstanz ab.
  ```
  ibmcloud service show <serviceinstanzname> --guid
  ```
  {: pre}

  Ausgabe:
  ```
  Invoking 'cf service <serviceinstanzname> --guid'...
  <serviceinstanz-GUID>
  ```
  {: screen}
3. Binden Sie den Service erneut an den Cluster.
  ```
  ibmcloud ks cluster-service-bind <clustername> <namensbereich> <serviceinstanz-guid>
  ```
  {: pre}

<br />


## Bindung eines Service an einen Cluster führt zu einem Fehler des Typs 'Service nicht gefunden'
{: #cs_not_found_services}

{: tsSymptoms}
Wenn Sie `ibmcloud ks cluster-service-bind <cluster_name> <namespace> <service_instance_name>` ausführen, wird die folgende Nachricht angezeigt:

```
Binding service to a namespace...
FAILED

The specified IBM Cloud service could not be found. If you just created the service, wait a little and then try to bind it again. To view available IBM Cloud service instances, run 'ibmcloud service list'. (E0023)
```
{: screen}

{: tsCauses}
Um Services an einen Cluster zu binden, müssen Sie über die Benutzerrolle des Cloud Foundry-Entwicklers für den Bereich verfügen, in dem die Serviceinstanz bereitgestellt wurde. Außerdem müssen Sie über Plattformzugriff für {{site.data.keyword.Bluemix_notm}} IAM-Editor auf {{site.data.keyword.containerlong}} verfügen. Um auf die Serviceinstanz zugreifen zu können, müssen Sie an dem Bereich angemeldet sein, in dem die Serviceinstanz bereitgestellt wurde.

{: tsResolve}

**Als Benutzer:**

1. Melden Sie sich bei {{site.data.keyword.Bluemix_notm}} an.
   ```
   ibmcloud login
   ```
   {: pre}

2. Geben Sie als Ziel die Organisation und den Bereich an, wo die Serviceinstanz bereitgestellt wird.
   ```
   ibmcloud target -o <organisation> -s <bereich>
   ```
   {: pre}

3. Überprüfen Sie, ob Sie sich in dem richtigen Bereich befinden; listen Sie hierzu die Serviceinstanzen auf.
   ```
   ibmcloud service list
   ```
   {: pre}

4. Versuchen Sie, den Service erneut zu binden. Wenn der gleiche Fehler angezeigt wird, wenden Sie sich an den Kontoadministrator und überprüfen Sie, dass Sie über ausreichende Berechtigungen zum Binden von Services verfügen (siehe die folgenden Schritte für den Kontoadministrator).

**Als Kontoadministrator:**

1. Stellen Sie sicher, dass der Benutzer, bei dem dieses Problem auftritt, über [Editorberechtigungen für {{site.data.keyword.containerlong}}](/docs/iam?topic=iam-iammanidaccser#edit_existing) verfügt.

2. Stellen Sie sicher, dass der Benutzer, bei dem dieses Problem auftritt, über die [Cloud Foundry-Entwicklerrolle für den Bereich](/docs/iam?topic=iam-mngcf#update_cf_access) verfügt, für den der Service bereitgestellt wird.

3. Wenn die korrekten Berechtigungen vorhanden sind, versuchen Sie, eine andere Berechtigung zuzuweisen und dann die erforderliche Berechtigung erneut zuzuweisen.

4. Warten Sie einige Minuten und lassen Sie dann den Benutzer versuchen, den Service erneut zu binden.

5. Wenn dadurch das Problem nicht behoben wird, sind die {{site.data.keyword.Bluemix_notm}} IAM-Berechtigungen nicht synchron und Sie können das Problem nicht selbst lösen. [Wenden Sie sich an den IBM Support](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support), indem Sie einen Supportfall öffnen. Stellen Sie sicher, dass Sie die Cluster-ID, die Benutzer-ID und die Serviceinstanz-ID bereitstellen.
   1. Rufen Sie die Cluster-ID ab.
      ```
      ibmcloud ks clusters
      ```
      {: pre}

   2. Rufen Sie die Serviceinstanz-ID ab.
      ```
      ibmcloud service show <servicename> --guid
      ```
      {: pre}


<br />


## Bindung eines Service an einen Cluster führt zu einem Fehler des Typs 'Service unterstützt Serviceschlüssel nicht'
{: #cs_service_keys}

{: tsSymptoms}
Wenn Sie `ibmcloud ks cluster-service-bind <cluster_name> <namespace> <service_instance_name>` ausführen, wird die folgende Nachricht angezeigt:

```
Dieser Service unterstützt nicht das Erstellen von Schlüsseln.
```
{: screen}

{: tsCauses}
Bestimmte Services in {{site.data.keyword.Bluemix_notm}}, wie z. B. {{site.data.keyword.keymanagementservicelong}}, unterstützen nicht das Erstellen von Serviceberechtigungsnachweisen, die auch als Serviceschlüssel bezeichnet werden. Ohne die Unterstützung von Serviceschlüsseln kann ein Cluster nicht gebunden werden. Informationen zum Suchen einer Liste von Services, die das Erstellen von Serviceschlüsseln unterstützen, finden Sie unter [Externen Apps die Verwendung von {{site.data.keyword.Bluemix_notm}}-Services ermöglichen](/docs/resources?topic=resources-externalapp#externalapp).

{: tsResolve}
Wenn Sie Services integrieren möchten, die keine Serviceschlüssel unterstützen, überprüfen Sie, ob der Service eine API bereitstellt, die Sie für den direkten Zugriff auf den Service über Ihre App verwenden können. Wenn Sie zum Beispiel {{site.data.keyword.keymanagementservicelong}} verwenden möchten, finden Sie weitere Informationen hierzu in der [API-Referenz ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/apidocs/kms?language=curl).

<br />


## Nachdem ein Workerknoten aktualisiert oder erneut geladen wurde, werden doppelte Knoten und Pods angezeigt
{: #cs_duplicate_nodes}

{: tsSymptoms}
Wenn Sie `kubectl get nodes` ausführen, werden doppelte Workerknoten mit dem Status **`NotReady`** (Nicht bereit) angezeigt. Die Workerknoten mit dem Status **`NotReady`** verfügen über öffentliche IP-Adressen, während die Workerknoten mit dem Status **`Ready`** (Bereit) private IP-Adressen haben.

{: tsCauses}
Bei älteren Clustern wurden Workerknoten anhand der öffentlichen IP-Adresse des Clusters aufgeführt. Nun werden Workerknoten über die private IP-Adresse des Clusters aufgelistet. Wenn Sie einen Knoten erneut laden oder aktualisieren, dann wird die IP-Adresse geändert, der Verweis auf die öffentliche IP-Adresse bleibt jedoch erhalten.

{: tsResolve}
Es treten keine Serviceunterbrechungen aufgrund dieser Duplikate auf, Sie können die alten Workerknotenverweise jedoch vom API-Server entfernen.

  ```
  kubectl delete node <knotenname1> <knotenname2>
  ```
  {: pre}

<br />


## Zugriff auf einen Pod auf einem Workerknoten schlägt mit einer Zeitlimitüberschreitung fehl
{: #cs_nodes_duplicate_ip}

{: tsSymptoms}
Sie haben einen Workerknoten in Ihrem Cluster gelöscht und dann einen Workerknoten hinzugefügt. Wenn Sie einen Pod oder einen Kubernetes-Service bereitgestellt haben, kann die Ressource nicht auf den neu erstellten Workerknoten zugreifen, und die Verbindung überschreitet das Zeitlimit.

{: tsCauses}
Wenn Sie einen Workerknoten aus Ihrem Cluster löschen und dann einen Workerknoten hinzufügen, kann es sein, dass der neue Workerknoten der privaten IP-Adresse des gelöschten Workerknotens zugeordnet wird. Calico verwendet diese private IP-Adresse als Tag und versucht weiterhin, den gelöschten Knoten zu erreichen.

{: tsResolve}
Aktualisieren Sie die Referenz der privaten IP-Adresse manuell, um auf den korrekten Knoten zu zeigen.

1.  Stellen Sie sicher, dass Sie zwei Workerknoten mit derselben **privaten IP**-Adresse haben. Notieren Sie sich die **private IP** und die **ID** des gelöschten Workers.

  ```
  ibmcloud ks workers --cluster <clustername_oder_-id>
  ```
  {: pre}

  ```
  ID                                                 Public IP       Private IP       Machine Type   State     Status   Zone   Version
  kube-dal10-cr9b7371a7fcbe46d08e04f046d5e6d8b4-w1   169.xx.xxx.xxx  10.xxx.xx.xxx    b3c.4x16       normal    Ready    dal10      1.12.7
  kube-dal10-cr9b7371a7fcbe46d08e04f046d5e6d8b4-w2   169.xx.xxx.xxx  10.xxx.xx.xxx    b3c.4x16       deleted    -       dal10      1.12.7
  ```
  {: screen}

2.  Installieren Sie die [Calico-CLI](/docs/containers?topic=containers-network_policies#adding_network_policies).
3.  Listen Sie die verfügbaren Workerknoten in Calico auf. Ersetzen Sie <dateipfad> durch den lokalen Pfad zur Calico-Konfigurationsdatei.

  ```
  calicoctl get nodes --config=filepath/calicoctl.cfg
  ```
  {: pre}

  ```
  NAME
  kube-dal10-cr9b7371a7faaa46d08e04f046d5e6d8b4-w1
  kube-dal10-cr9b7371a7faaa46d08e04f046d5e6d8b4-w2
  ```
  {: screen}

4.  Löschen Sie den doppelten Workerknoten in Calico. Ersetzen Sie KNOTEN-ID durch die Workerknoten-ID.

  ```
  calicoctl delete node KNOTEN-ID --config=<dateipfad>/calicoctl.cfg
  ```
  {: pre}

5.  Starten Sie den Workerknoten, der nicht gelöscht wurde, neu.

  ```
  ibmcloud ks worker-reboot --cluster <clustername_oder_-id> --worker <worker-id>
  ```
  {: pre}


Der gelöschte Knoten wird nicht mehr in Calico aufgelistet.

<br />




## Bereitstellen von Pods schlägt wegen einer Sicherheitsrichtlinie für Pods fehl
{: #cs_psp}

{: tsSymptoms}
Nach dem Erstellen eines Pods oder der Ausführung von `kubectl get events` zum Überprüfen, ob ein Pod bereitgestellt wurde, wird eine Fehlernachricht ähnlich der folgenden angezeigt.

```
unable to validate against any pod security policy
```
{: screen}

{: tsCauses}
[Der Zugangscontroller `PodSecurityPolicy`](/docs/containers?topic=containers-psp) überprüft die Autorisierung des Benutzers oder Servicekontos (z. B. eine Bereitstellung oder 'tiller' von Helm), der bzw. das versuchte, den Pod zu erstellen. Wenn der Benutzer oder das Servicekonto von keiner Sicherheitsrichtlinie für Pods unterstützt wird, verhindert der Zugangscontroller `PodSecurityPolicy` die Erstellung der Pods.

Wenn Sie für die [{{site.data.keyword.IBM_notm}}-Clusterverwaltung](/docs/containers?topic=containers-psp#ibm_psp) eine der Ressourcen für Sicherheitsrichtlinien für Pods gelöscht haben, treten möglicherweise ähnliche Probleme auf.

{: tsResolve}
Stellen Sie sicher, dass der Benutzer oder das Servicekonto durch eine Pod-Sicherheitsrichtlinie berechtigt ist. Möglicherweise müssen Sie eine [vorhandene Richtlinie ändern](/docs/containers?topic=containers-psp#customize_psp).

Wenn Sie eine {{site.data.keyword.IBM_notm}} Clusterverwaltungsressource gelöscht haben, müssen Sie den Kubernetes-Master aktualisieren, um ihn wiederherzustellen.

1.  [Melden Sie sich an Ihrem Konto an. Geben Sie als Ziel die entsprechende Region und, sofern zutreffend, die Ressourcengruppe an. Legen Sie den Kontext für den Cluster fest.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
2.  Aktualisieren Sie den Kubernetes-Master, um ihn wiederherzustellen.

    ```
    ibmcloud ks apiserver-refresh
    ```
    {: pre}


<br />




## Cluster verbleibt im Wartestatus ('Pending')
{: #cs_cluster_pending}

{: tsSymptoms}
Wenn Sie Ihren Cluster bereitstellen, bleibt dieser im Wartestatus und startet nicht.

{: tsCauses}
Wenn Sie den Cluster gerade erst erstellt haben, werden die Workerknoten möglicherweise noch konfiguriert. Wenn Sie bereits eine Weile warten, ist das VLAN möglicherweise nicht gültig.

{: tsResolve}

Sie können eine der folgenden Lösungen ausprobieren:
  - Überprüfen Sie den Status Ihres Clusters, indem Sie den Befehl `ibmcloud ks clusters` ausführen. Überprüfen Sie anschließend durch Ausführen des Befehls `ibmcloud ks workers --cluster <clustername>`, ob Ihre Workerknoten bereitgestellt wurden.
  - Überprüfen Sie, ob Ihr VLAN gültig ist. Damit ein VLAN gültig ist, muss es einer Infrastruktur zugeordnet sein, die einen Worker mit lokalem Plattenspeicher hosten kann. Sie können [Ihre VLANs auflisten](/docs/containers?topic=containers-cs_cli_reference#cs_vlans), indem Sie den Befehl `ibmcloud ks vlans --zone <zone>` ausführen. Wenn das VLAN nicht in der Liste aufgeführt wird, ist es nicht gültig. Wählen Sie ein anderes VLAN aus.

<br />




## Image kann nicht aus der Registry extrahiert werden
{: #ts_image_pull}

{: tsSymptoms}

Wenn Sie eine Workload bereitstellen, die ein Image aus {{site.data.keyword.registrylong_notm}} extrahiert, tritt für Ihren Pod ein Fehler mit dem Status **`ImagePullBackOff`** auf.

```
kubectl get pods
```
{: pre}

```
NAME         READY     STATUS             RESTARTS   AGE
<podname>    0/1       ImagePullBackOff   0          2m
```
{: screen}

Wenn Sie den Pod beschreiben, empfangen Sie Authentifizierungsfehler ähnlich den folgenden.

```
kubectl describe pod <podname>
```
{: pre}

```
Failed to pull image "<region>.icr.io/<namespace>/<image>:<tag>" ... unauthorized: authentication required
Failed to pull image "<region>.icr.io/<namespace>/<image>:<tag>" ... 401 Unauthorized
```
{: screen}

```
Failed to pull image "registry.ng.bluemix.net/<namespace>/<image>:<tag>" ... unauthorized: authentication required
Failed to pull image "registry.ng.bluemix.net/<namespace>/<image>:<tag>" ... 401 Unauthorized
```
{: screen}

{: tsCauses}
Ihr Cluster verwendet einen API-Schlüssel oder ein Token, der bzw. das in einem [geheimen Schlüssel für Image-Pull-Operationen (Image Pull Secret)](/docs/containers?topic=containers-images#cluster_registry_auth) gespeichert ist, um den Cluster zum Extrahieren von Images aus {{site.data.keyword.registrylong_notm}} zu berechtigen. Standardmäßig haben neue Cluster geheime Schlüssel für Image-Pull-Operationen, die API-Schlüssel verwenden, sodass der Cluster Images aus einer beliebigen regionalen `icr.io`-Registry für Container extrahieren kann, die im Kubernetes-Namensbereich `default` bereitgestellt wurden. Wenn der Cluster einen geheimen Schlüssel für Image-Pull-Operationen hat, der ein Token verwendet, ist der Standardzugriff auf {{site.data.keyword.registrylong_notm}} auf bestimmte regionale Registrys eingeschränkt, die die veralteten `<region>.registry.bluemix.net`-Domänen verwenden.

{: tsResolve}

1.  Stellen Sie sicher, dass Sie den richtigen Namen und den richtigen Tag des Image in Ihrer YAML-Bereitstellungsdatei verwenden.
    ```
    ibmcloud cr images
    ```
    {: pre}
2.  Rufen Sie die Podkonfigurationsdatei für einen fehlerhaften Pod ab und suchen Sie nach dem Abschnitt für geheime Schlüssel für Image-Pull-Operationen (`imagePullSecrets`).
    ```
    kubectl get pod <podname> -o yaml
    ```
    {: pre}

    Beispielausgabe:
    ```
    ...
    imagePullSecrets:
    - name: bluemix-default-secret
    - name: bluemix-default-secret-regional
    - name: bluemix-default-secret-international
    - name: default-us-icr-io
    - name: default-uk-icr-io
    - name: default-de-icr-io
    - name: default-au-icr-io
    - name: default-jp-icr-io
    - name: default-icr-io
    ...
    ```
    {: screen}
3.  Wenn keine geheime Schlüssel für Image-Pull-Operationen aufgelistet werden, richten Sie einen solchen in Ihrem Namensbereich ein.
    1.  [Kopieren Sie den geheimen Schlüssel für Image-Pull-Operationen aus dem Kubernetes-Namensbereich `default` in den Namensbereich, in dem Sie Ihre Workload bereitstellen wollen](/docs/containers?topic=containers-images#copy_imagePullSecret).
    2.  [Fügen Sie den geheimen Schlüssel für Image-Pull-Operationen dem Servicekonto für diesen Kubernetes-Namensbereich hinzu](/docs/containers?topic=containers-images#store_imagePullSecret), sodass alle Pods in dem Namensbereich die Berechtigungsnachweise des geheimen Schlüssels für Image-Pull-Operationen verwenden können.
4.  Wenn geheime Schlüssel für Image-Pull-Operationen aufgelistet werden, ermitteln Sie, welchen Typ von Berechtigungsnachweisen Sie für den Zugriff auf die Container-Registry verwenden.
    *   **Veraltet:** Wenn der geheime Schlüssel `bluemix` in seinem Namen enthält, verwenden Sie ein Registry-Token, um sich mit den veralteten Domänennamen `registry.<region>.bluemix.net` zu authentifizieren. Fahren Sie mit dem Abschnitt [Fehlerbehebung für geheime Schlüssel für Image-Pull-Operationen, die Tokens verwenden](#ts_image_pull_token) fort.
    *   Wenn der geheime Schlüssel `icr` in seinem Namen enthält, verwenden Sie einen API-Schlüssel, um sich mit den Domänennamen `icr.io` zu authentifizieren. Fahren Sie mit dem Abschnitt [Fehlerbehebung für geheime Schlüssel für Image-Pull-Operationen, die API-Schlüssel verwenden](#ts_image_pull_apikey) fort.
    *   Wenn Sie beide Typen von geheimen Schlüsseln haben, verwenden Sie beide Authentifizierungsmethoden. Verwenden Sie in Zukunft die Domänennamen `icr.io` in Ihren YAML-Bereitstellungsdateien für das Container-Image. Fahren Sie mit dem Abschnitt [Fehlerbehebung für geheime Schlüssel für Image-Pull-Operationen, die API-Schlüssel verwenden](#ts_image_pull_apikey) fort.

<br>
<br>

**Fehlerbehebung für geheime Schlüssel für Image-Pull-Operationen, die API-Schlüssel verwenden**</br>
{: #ts_image_pull_apikey}

Wenn Ihre Podkonfiguration einen geheimen Schlüssel für Image-Pull-Operationen hat, der einen API-Schlüssel verwendet, überprüfen Sie, ob die Berechtigungsnachweise des API-Schlüssels korrekt festgelegt sind.
{: shortdesc}

In den folgenden Schritten wird davon ausgegangen, dass der API-Schlüssel die Berechtigungsnachweise für eine Service-ID speichert. Wenn Sie Ihren geheimen Schlüssel für Image-Pull-Operationen so eingerichtet haben, dass er einen API-Schlüssel eines einzelnen Benutzers verwendet, müssen Sie die {{site.data.keyword.Bluemix_notm}} IAM-Berechtigungen und Berechtigungsnachweise dieses Benutzers überprüfen.
{: note}

1.  Suchen Sie die Service-ID, die der API-Schlüssel für den geheimen Schlüssel für Image-Pull-Operationen verwendet, indem Sie die Beschreibung (**Description**) prüfen. Die Beschreibung der Service-ID, die mit dem Cluster erstellt wurde, lautet `ID for <clustername>` und die Service-ID wird im Kubernetes-Namensbereich `default` verwendet. Falls Sie eine andere Service-ID zum Beispiel für den Zugriff auf einen anderen Kubernetes-Namensbereich oder zum Ändern der {{site.data.keyword.Bluemix_notm}} IAM-Berechtigungen erstellt haben, haben Sie die Beschreibung angepasst.
    ```
    ibmcloud iam service-ids
    ```
    {: pre}

    Beispielausgabe:
    ```
    UUID                Name               Created At              Last Updated            Description                                                                                                                                                                                         Locked
    ServiceId-aa11...   <service-id-name>  2019-02-01T19:01+0000   2019-02-01T19:01+0000   ID for <clustername>                                                                                                                                          false
    ServiceId-bb22...   <service-id-name>  2019-02-01T19:01+0000   2019-02-01T19:01+0000   Service ID for IBM Cloud Container Registry in Kubernetes cluster <clustername> namespace <kube-namespace>                                                                                                                                          false
    ```
    {: screen}
2.  Stellen Sie sicher, dass der Service-ID mindestens eine [Richtlinie für die {{site.data.keyword.Bluemix_notm}} IAM-Servicezugriffsrolle **Leseberechtigter** (Reader) für {{site.data.keyword.registryshort_notm}}](/docs/services/Registry?topic=registry-user#create) zugewiesen ist. Wenn die Service-ID die Servicerolle **Leseberechtigter** (Reader) nicht hat, [bearbeiten Sie die IAM-Richtlinien](/docs/iam?topic=iam-serviceidpolicy#access_edit). Wenn die Richtlinien korrekt sind, fahren Sie mit dem nächsten Schritt fort, um zu prüfen, ob die Berechtigungsnachweise gültig sind.
    ```
    ibmcloud iam service-policies <service-id-name>
    ```
    {: pre}

    Beispielausgabe:
    ```
    Policy ID:   a111a111-b22b-333c-d4dd-e555555555e5
    Roles:       Reader
    Resources:
                  Service Name       container-registry
                  Service Instance
                  Region
                  Resource Type      namespace
                  Resource           <registry-namensbereich>
    ```
    {: screen}
3.  Prüfen Sie, ob die Berechtigungsnachweise des geheimen Schlüssels für Image-Pull-Operationen gültig sind.
    1.  Rufen Sie die Konfiguration des geheimen Schlüssels für Image-Pull-Operationen ab. Wenn sich der Pod nicht im Namensbereich `default` befindet, geben Sie das Flag `-n` mit an.
        ```
        kubectl get secret <name_des_geheimen_schlüssels_für_image-pull-operationen> -o yaml [-n <namensbereich>]
        ```
        {: pre}
    2.  Kopieren Sie in der Ausgabe den base64-codierten Wert des Felds `.dockercfg`.
        ```
        apiVersion: v1
        kind: Secret
        data:
          .dockercfg: eyJyZWdp...==
        ...
        ```
        {: screen}
    3.  Decodieren Sie die Base64-Zeichenfolge. Beispiel: Unter OS X können Sie den folgenden Befehl ausführen.
        ```
        echo -n "<base64-zeichenfolge>" | base64 --decode
        ```
        {: pre}

        Beispielausgabe:
        ```
        {"auths":{"<region>.icr.io":{"username":"iamapikey","password":"<kennwortzeichenfolge>","email":"<name@abc.com>","auth":"<auth-zeichenfolge>"}}}
        ```
        {: screen}
    4.  Vergleichen Sie den Domänennamen aus der regionalen Registry des geheimen Schlüssels für Image-Pull-Operationen mit dem Domänennamen, den Sie im Container-Image angegeben haben. Standardmäßig haben neue Cluster geheime Schlüssel für Image-Pull-Operationen für jeden Domänennamen in der regionalen Registry für Container, die im Kubernetes-Namensbereich `default` ausgeführt werden. Wenn Sie die Standardeinstellungen jedoch geändert haben oder einen anderen Kubernetes-Namensbereich verwenden, haben Sie möglicherweise keinen geheimen Schlüssel für Image-Pull-Operationen für die regionale Registry. [Kopieren Sie einen geheimen Schlüssel für Image-Pull-Operationen](/docs/containers?topic=containers-images#copy_imagePullSecret) für den Domänennamen in der regionalen Registry.
    5.  Melden Sie sich von Ihrem lokalen System aus bei der Registry mit den Werten für `username` und `password` aus Ihrem geheimen Schlüssel für Image-Pull-Operationen an. Wenn Sie sich nicht anmelden können, müssen Sie möglicherweise die Service-ID korrigieren.
        ```
        docker login -u iamapikey -p <kennwortzeichenfolge> <region>.icr.io
        ```
        {: pre}
        1.  Erstellen Sie die Cluster-Service-ID, die {{site.data.keyword.Bluemix_notm}} IAM-Richtlinien, den API-Schlüssel und die geheimen Schlüssel für Image-Pull-Operationen für Container neu, die im Kubernetes-Namensbereich `default` ausgeführt werden.
            ```
            ibmcloud ks cluster-pull-secret-apply --cluster <clustername_oder_-id>
            ```
            {: pre}
        2.  Erstellen Sie Ihre Bereitstellung im Kubernetes-Namensbereich `default` neu. Wenn weiterhin eine Berechtigungsfehlernachricht angezeigt wird, wiederholen Sie die Schritte 1 bis 5 mit den neuen geheimen Schlüsseln für Image-Pull-Operationen. Wenn Sie sich auch weiterhin nicht anmelden können, [wenden Sie sich an das IBM Team auf Slack oder öffnen Sie einen {{site.data.keyword.Bluemix_notm}}-Supportfall](#clusters_getting_help).
    6.  Wenn die Anmeldung erfolgreich ist, extrahieren Sie ein Image auf Ihre lokale Maschine. Schlägt der Befehl mit einem Fehler 'Zugriff verweigert' (`access denied`) fehl, handelt es sich bei dem Registry-Konto um ein anderes {{site.data.keyword.Bluemix_notm}}-Konto als das, in dem sich Ihr Cluster befindet. [Erstellen Sie einen geheimen Schlüssel für Image-Pull-Operationen (imagePullSecret), um auf Images in dem anderen Konto zuzugreifen](/docs/containers?topic=containers-images#other_registry_accounts). Wenn Sie ein Image auf Ihre lokale Maschine extrahieren können, verfügt Ihr API-Schlüssel zwar über die entsprechenden Berechtigungen, aber die API-Konfiguration in Ihrem Cluster ist nicht korrekt. Sie können dieses Problem nicht lösen. [Wenden Sie sich an das IBM Team auf Slack oder öffnen Sie einen {{site.data.keyword.Bluemix_notm}}-Supportfall](#clusters_getting_help).
        ```
        docker pull <region>icr.io/<namensbereich>/<image>:<tag>
        ```
        {: pre}

<br>
<br>

**Veraltet: Fehlerbehebung für geheime Schlüssel für Image-Pull-Operationen, die Tokens verwenden**</br>
{: #ts_image_pull_token}

Wenn Ihre Podkonfiguration einen geheimen Schlüssel für Image-Pull-Operationen hat, der ein Token verwendet, überprüfen Sie, ob die Berechtigungsnachweise des Tokens gültig sind.
{: shortdesc}

Diese Methode der Verwendung eines Tokens für die Autorisierung des Clusterzugriffs auf {{site.data.keyword.registrylong_notm}} wird für Domänennamen mit `registry.bluemix.net` unterstützt, ist jedoch veraltet. [Verwenden Sie stattdessen die Methode mit dem API-Schlüssel](/docs/containers?topic=containers-images#cluster_registry_auth), um den Clusterzugriff auf die neuen Registry-Domänennamen mit `icr.io` zu autorisieren.
{: deprecated}

1.  Rufen Sie die Konfiguration des geheimen Schlüssels für Image-Pull-Operationen ab. Wenn sich der Pod nicht im Namensbereich `default` befindet, geben Sie das Flag `-n` mit an.
    ```
    kubectl get secret <name_des_geheimen_schlüssels_für_image-pull-operationen> -o yaml [-n <namensbereich>]
    ```
    {: pre}
2.  Kopieren Sie in der Ausgabe den base64-codierten Wert des Felds `.dockercfg`.
    ```
    apiVersion: v1
    kind: Secret
    data:
      .dockercfg: eyJyZWdp...==
    ...
    ```
    {: screen}
3.  Decodieren Sie die Base64-Zeichenfolge. Beispiel: Unter OS X können Sie den folgenden Befehl ausführen.
    ```
    echo -n "<base64-zeichenfolge>" | base64 --decode
    ```
    {: pre}

    Beispielausgabe:
    ```
    {"auths":{"registry.<region>.bluemix.net":{"username":"token","password":"<kennwortzeichenfolge>","email":"<name@abc.com>","auth":"<auth-zeichenfolge>"}}}
    ```
    {: screen}
4.  Vergleichen Sie den Domänennamen aus der Registry mit dem Domänennamen, den Sie im Container-Image angegeben haben. Beispiel: Wenn der geheime Schlüssel für Image-Pull-Operationen den Zugriff auf die Domäne `registry.ng.bluemix.net` berechtigt, Sie jedoch ein Image angegeben haben, das in der Domäne `registry.eu-de.bluemix.net` gespeichert ist, müssen Sie für `registry.eu-de.bluemix.net` ein [Token zur Verwendung in einem geheimen Schlüssel für Image-Pull-Operationen erstellen](/docs/containers?topic=containers-images#token_other_regions_accounts).
5.  Melden Sie sich von Ihrem lokalen System aus bei der Registry mit den Werten für `username` und `password` aus dem geheimen Schlüssel für Image-Pull-Operationen an. Wenn Sie sich nicht anmelden können, hat das Token ein Problem, das Sie nicht lösen können. [Wenden Sie sich an das IBM Team auf Slack oder öffnen Sie einen {{site.data.keyword.Bluemix_notm}}-Supportfall](#clusters_getting_help).
    ```
    docker login -u token -p <kennwortzeichenfolge> registry.<region>.bluemix.net
    ```
    {: pre}
6.  Wenn die Anmeldung erfolgreich ist, extrahieren Sie ein Image auf Ihre lokale Maschine. Schlägt der Befehl mit einem Fehler 'Zugriff verweigert' (`access denied`) fehl, handelt es sich bei dem Registry-Konto um ein anderes {{site.data.keyword.Bluemix_notm}}-Konto als das, in dem sich Ihr Cluster befindet. [Erstellen Sie einen geheimen Schlüssel für Image-Pull-Operationen (imagePullSecret), um auf Images in dem anderen Konto zuzugreifen](/docs/containers?topic=containers-images#token_other_regions_accounts). Wenn der Befehl ein Image erfolgreich auf Ihre lokale Maschine extrahiert, [wenden Sie sich an das IBM Team auf Slack oder öffnen Sie einen {{site.data.keyword.Bluemix_notm}}-Supportfall](#clusters_getting_help).
    ```
    docker pull registry.<region>.bluemix.net/<namensbereich>/<image>:<tag>
    ```
    {: pre}

<br />


## Pods verbleiben im Status 'Pending' (Anstehend)
{: #cs_pods_pending}

{: tsSymptoms}
Beim Ausführen von `kubectl get pods` verbleiben Pods weiterhin im Status **Pending** (Anstehend).

{: tsCauses}
Wenn Sie den Kubernetes-Cluster gerade erst erstellt haben, werden die Workerknoten möglicherweise noch konfiguriert.

Falls dieser Cluster bereits vorhanden ist:
*  Möglicherweise ist in Ihrem Cluster nicht ausreichend Kapazität verfügbar, um den Pod bereitzustellen.
*  Der Pod kann eine Ressourcenanforderung oder einen Grenzwert überschritten haben.

{: tsResolve}
Diese Task setzt voraus, dass die {{site.data.keyword.Bluemix_notm}} IAM-Plattformrolle [**Administrator**](/docs/containers?topic=containers-users#platform) für den Cluster und die [Servicerolle **Manager**](/docs/containers?topic=containers-users#platform) für alle Namensbereiche zugewiesen sind.

Führen Sie den folgenden Befehl aus, wenn Sie den Kubernetes-Cluster gerade erst erstellt haben. Warten Sie, bis die Workerknoten initialisiert werden.

```
kubectl get nodes
```
{: pre}

Falls dieser Cluster bereits vorhanden ist, prüfen Sie Ihre Clusterkapazität.

1.  Legen Sie die Standardportnummer für den Proxy fest.

  ```
  kubectl proxy
  ```
   {: pre}

2.  Öffnen Sie das Kubernetes-Dashboard.

  ```
  http://localhost:8001/ui
  ```
  {: pre}

3.  Überprüfen Sie, ob in Ihrem Cluster ausreichend Kapazität verfügbar ist, um Ihren Pod bereitzustellen.

4.  Wenn Ihr Cluster nicht über ausreichend Kapazität verfügt, ändern Sie die Größe des Worker-Pools, um weitere Knoten hinzuzufügen.

    1.  Prüfen Sie die aktuellen Größen und Maschinentypen Ihrer Worker-Pools, um zu entscheiden, für welche Worker-Pools eine Größenänderung durchgeführt werden soll.

        ```
        ibmcloud ks worker-pools
        ```
        {: pre}

    2.  Ändern Sie die Größe der Worker-Pools, um den einzelnen Zonen, die der Pool umfasst, weitere Knoten hinzuzufügen.

        ```
        ibmcloud ks worker-pool-resize <worker-pool> --cluster <clustername_oder_-id> --size-per-zone <worker_pro_zone>
        ```
        {: pre}

5.  Optional: Überprüfen Sie Ihre Podressourcenanforderungen.

    1.  Vergewissern Sie sich, dass die Werte für `resources.requests` nicht größer sind als die Kapazität des Workerknotens. Beispiel: Wenn die Podanforderung `cpu: 4000m` - oder 4 Kerne - beträgt, der Workerknoten aber nur 2 Kerne umfasst, kann der Pod nicht bereitgestellt werden.

        ```
        kubectl get pod <podname> -o yaml
        ```
        {: pre}

    2.  Wenn die Anforderung die verfügbare Kapazität überschreitet, [fügen Sie einen neuen Worker-Pool](/docs/containers?topic=containers-clusters#add_pool) mit Workerknoten hinzu, die die Anforderung erfüllen können.

6.  Falls Ihre Pods auch weiterhin im Status **Pending** (Anstehend) verweilen, obwohl der Workerknoten voll bereitgestellt wurde, ziehen Sie die [Kubernetes-Dokumentation ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/tasks/debug-application-cluster/debug-pod-replication-controller/#my-pod-stays-pending) zurate, um die Ursache für den andauernden Status 'Pending' Ihres Pods zu ermitteln und den Fehler zu beheben.

<br />


## Container werden nicht gestartet
{: #containers_do_not_start}

{: tsSymptoms}
Die Pods wurden erfolgreich auf den Clustern bereitgestellt, aber die Container können nicht gestartet werden.

{: tsCauses}
Die Container werden möglicherweise nicht gestartet, wenn die Registry-Quote erreicht ist.

{: tsResolve}
[Geben Sie Speicherplatz in {{site.data.keyword.registryshort_notm}} frei.](/docs/services/Registry?topic=registry-registry_quota#registry_quota_freeup)

<br />


## Neustarts von Pods schlagen wiederholt fehl oder Pods werden unerwartet entfernt
{: #pods_fail}

{: tsSymptoms}
Der Pod befand sich in einwandfreiem Zustand, wird aber unerwartet entfernt oder bleibt in einer Neustartschleife stecken.

{: tsCauses}
Es kann sein, dass die Ressourcengrenzwerte der Container überschritten werden oder dass die Pods durch Pods mit höherer Priorität ersetzt wurden.

{: tsResolve}
Gehen Sie wie folgt vor, um festzustellen, ob ein Container aufgrund einer Überschreitung eines Ressourcengrenzwerts gestoppt wird:
<ol><li>Rufen Sie den Namen des Pods ab. Falls Sie eine Bezeichnung verwenden, können Sie diese zum Filtern der Ergebnisse verwenden.<pre class="pre"><code>kubectl get pods --selector='app=wasliberty'</code></pre></li>
<li>Beschreiben Sie den Pod und suchen Sie nach dem **Zähler für Neustart** (Restart Count).<pre class="pre"><code>kubectl describe pod</code></pre></li>
<li>Rufen Sie den Status des Pods ab, wenn er in einem kurzen Zeitraum oft erneut gestartet wird. <pre class="pre"><code>kubectl get pod -o go-template={{range.status.containerStatuses}}{{"Container Name: "}}{{.name}}{{"\r\nLastState: "}}{{.lastState}}{{end}}</code></pre></li>
<li>Überprüfen Sie die Ursache. Beispiel: `OOM Killed` bedeutet, dass nicht ausreichend Speicher vorhanden war und der Container aufgrund eines Ressourcengrenzwerts gestoppt wurde.</li>
<li>Fügen Sie Kapazität zum Cluster hinzu, damit die Ressourcenanforderungen erfüllt werden können.</li></ol>

<br>

Gehen Sie wie folgt vor, um festzustellen, ob der Pod durch Pods mit höherer Priorität ersetzt wurde:
1.  Rufen Sie den Namen des Pods ab.

    ```
    kubectl get pods
    ```
    {: pre}

2.  Beschreiben Sie Ihre Pod-YAML.

    ```
    kubectl get pod <podname> -o yaml
    ```
    {: pre}

3.  Überprüfen Sie das Feld `priorityClassName`.

    1.  Wenn für das Feld `priorityClassName` kein Wert angegeben ist, weist der Pod die Prioritätsklasse `globalDefault` auf. Wenn der Clusteradministrator nicht die Prioritätsklasse `globalDefault` festgelegt hat, ist der Standardwert null (0) oder die niedrigste Priorität. Der Pod kann von jedem Pod mit einer höheren Priorität zurückgestellt oder entfernt werden.

    2.  Wenn für das Feld `priorityClassName` ein Wert angegeben ist, rufen Sie die Prioritätsklasse ab.

        ```
        kubectl get priorityclass <name_der_prioritätsklasse> -o yaml
        ```
        {: pre}

    3.  Notieren Sie den Wert für das Feld `Value`, um die Priorität des Pods zu überprüfen.

4.  Listen Sie vorhandene Prioritätsklassen im Cluster auf.

    ```
    kubectl get priorityclasses
    ```
    {: pre}

5.  Rufen Sie für jede Prioritätsklasse die YAML-Datei ab und notieren Sie den Wert im Feld `Value`.

    ```
    kubectl get priorityclass <name_der_prioritätsklasse> -o yaml
    ```
    {: pre}

6.  Vergleichen Sie den Prioritätsklassenwert des Pos mit den anderen Prioritätsklassenwerten, um festzustellen, ob seine Priorität höher oder niedriger ist.

7.  Wiederholen Sie die Schritte 1 bis 3 für andere Pods im Cluster, um zu überprüfen, welche Prioritätsklasse von ihnen verwendet wird. Wenn die Prioritätsklasse der anderen Pods höher ist als die Ihres Pods, wird Ihr Pod erst bereitgestellt, wenn genügend Ressourcen für Ihren Pod und alle Pods mit höherer Priorität vorhanden sind.

8.  Wenden Sie sich an den Clusteradministrator, um weitere Kapazität zum Cluster hinzuzufügen und zu bestätigen, dass die richtigen Prioritätsklassen zugeordnet sind.

<br />


## Helm-Diagramm kann nicht mit aktualisierten Konfigurationswerten installiert werden
{: #cs_helm_install}

{: tsSymptoms}
Wenn Sie versuchen, ein aktualisiertes Helm-Diagramm zu installieren, indem Sie den Befehl `helm install -f config.yaml --namespace=kube-system --name=<release_name> ibm/<chart_name>` ausführen, erhalten Sie die Fehlernachricht `Error: failed to download "ibm/<chart_name>"`.

{: tsCauses}
Die URL für das {{site.data.keyword.Bluemix_notm}}-Repository in Ihrer Helm-Instanz ist möglicherweise nicht korrekt.

{: tsResolve}
Gehen Sie wie folgt vor, um Fehler in Ihrem Helm-Diagramm zu beheben:

1. Listen Sie die Repository auf, die aktuell in Ihrer Helm-Instanz verfügbar sind.

    ```
    helm repo list
    ```
    {: pre}

2. Überprüfen Sie in der Ausgabe, ob die URL für das {{site.data.keyword.Bluemix_notm}}-Repository `ibm` wie folgt lautet: `https://registry.bluemix.net/helm/ibm`.

    ```
    NAME    URL
    stable  https://kubernetes-charts.storage.googleapis.com
    local   http://127.0.0.1:8888/charts
    ibm     https://registry.bluemix.net/helm/ibm
    ```
    {: screen}

    * Falls die URL falsch ist:

        1. Entfernen Sie das {{site.data.keyword.Bluemix_notm}}-Repository.

            ```
            helm repo remove ibm
            ```
            {: pre}

        2. Fügen Sie das {{site.data.keyword.Bluemix_notm}}-Repository erneut hinzu.

            ```
            helm repo add ibm  https://registry.bluemix.net/helm/ibm
            ```
            {: pre}

    * Wenn die URL richtig ist, rufen Sie die aktuellsten Aktualisierung vom Repository ab:

        ```
        helm repo update
        ```
        {: pre}

3. Installieren Sie das Helm-Diagramm mit Ihren Aktualisierungen.

    ```
    helm install -f config.yaml --namespace=kube-system --name=<releasename> ibm/<diagrammname>
    ```
    {: pre}

<br />


## Installieren von Helm-Tiller oder Bereitstellen von Containern aus öffentlichen Images im Cluster nicht möglich
{: #cs_tiller_install}

{: tsSymptoms}

Wenn Sie versuchen, Tiller von Helm zu installieren oder Images aus öffentlichen Registrys, wie zum Beispiel DockerHub, bereitzustellen, schlägt die Installation mit einem Fehler ähnlich dem folgenden fehl:

```
Failed to pull image "gcr.io/kubernetes-helm/tiller:v2.12.0": rpc error: code = Unknown desc = failed to resolve image "gcr.io/kubernetes-helm/tiller:v2.12.0": no available registry endpoint:
```
{: screen}

{: tsCauses}
Sie haben möglicherweise eine angepasste Firewall eingerichtet, angepasste Calico-Richtlinien angegeben oder einen rein privaten Cluster durch die Verwendung des privaten Serviceendpunkts erstellt, die die öffentliche Netzkonnektivität zu der Container-Registry blockieren, in der das Image gespeichert ist.

{: tsResolve}
- Wenn Sie eine angepasste Firewall haben oder angepasste Calico-Richtlinien festgelegt haben, lassen Sie aus- und eingehenden Netzdatenverkehr zwischen Ihren Workerknoten und der Container-Registry zu, in der das Image gespeichert ist. Wenn das Image in {{site.data.keyword.registryshort_notm}} gespeichert ist, prüfen Sie die Informationen zu den erforderlichen Ports unter [Zugriff des Clusters auf Infrastrukturressourcen und andere Services ermöglichen](/docs/containers?topic=containers-firewall#firewall_outbound).
- Wenn Sie einen privaten Cluster erstellt haben, indem Sie nur den privaten Serviceendpunkt aktiviert haben, können Sie [den öffentlichen Serviceendpunkt für Ihren Cluster aktivieren](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_feature_disable). Wenn Sie Helm-Diagramme in einem privaten Cluster installieren wollen, ohne eine öffentliche Verbindung zu öffnen, können Sie Helm [mit Tiller](/docs/containers?topic=containers-helm#private_local_tiller) oder [ohne Tiller](/docs/containers?topic=containers-helm#private_install_without_tiller) installieren.

<br />


## Hilfe und Unterstützung anfordern
{: #clusters_getting_help}

Haben Sie noch immer Probleme mit Ihrem Cluster?
{: shortdesc}

-  Sie werden im Terminal benachrichtigt, wenn Aktualisierungen für die `ibmcloud`-CLI und -Plug-ins verfügbar sind. Halten Sie Ihre CLI stets aktuell, sodass Sie alle verfügbaren Befehle und Flags verwenden können.
-   [Überprüfen Sie auf der {{site.data.keyword.Bluemix_notm}}-Statusseite ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/status?selected=status), ob {{site.data.keyword.Bluemix_notm}} verfügbar ist.
-   Veröffentlichen Sie eine Frage im [{{site.data.keyword.containerlong_notm}}-Slack ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://ibm-container-service.slack.com).
    Wenn Sie keine IBM ID für Ihr {{site.data.keyword.Bluemix_notm}}-Konto verwenden, [fordern Sie eine Einladung](https://bxcs-slack-invite.mybluemix.net/) zu diesem Slack an.
    {: tip}
-   Suchen Sie in entsprechenden Foren, ob andere Benutzer auf das gleiche Problem gestoßen sind. Versehen Sie Ihre Fragen in den Foren mit Tags, um sie für das Entwicklungsteam von {{site.data.keyword.Bluemix_notm}} erkennbar zu machen.
    -   Wenn Sie technische Fragen zur Entwicklung oder Bereitstellung von Clustern oder Apps mit {{site.data.keyword.containerlong_notm}} haben, veröffentlichen Sie Ihre Frage auf [Stack Overflow ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://stackoverflow.com/questions/tagged/ibm-cloud+containers) und versehen Sie sie mit den Tags `ibm-cloud`, `kubernetes` und `containers`.
    -   Verwenden Sie bei Fragen zum Service und zu ersten Schritten das Forum [IBM Developer Answers ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix). Geben Sie die Tags `ibm-cloud` und `containers` an.
    Weitere Details zur Verwendung der Foren finden Sie unter [Hilfe anfordern](/docs/get-support?topic=get-support-getting-customer-support#using-avatar).
-   Wenden Sie sich an den IBM Support, indem Sie einen Fall öffnen. Informationen zum Öffnen eines IBM Supportfalls oder zu Supportstufen und zu Prioritätsstufen von Fällen finden Sie unter [Support kontaktieren](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support).
Geben Sie beim Melden eines Problems Ihre Cluster-ID an. Führen Sie den Befehl `ibmcloud ks clusters` aus, um Ihre Cluster-ID abzurufen. Sie können außerdem [{{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool](/docs/containers?topic=containers-cs_troubleshoot#debug_utility) verwenden, um relevante Informationen aus Ihrem Cluster zu erfassen und zu exportieren, um sie dem IBM Support zur Verfügung zu stellen.
{: tip}
