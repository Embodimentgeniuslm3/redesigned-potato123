---

copyright:
  years: 2014, 2019
lastupdated: "2019-04-17"

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


# Sensible Informationen im Cluster schützen
{: #encryption}

Schützen Sie sensible Clusterinformationen, um Datenintegrität zu gewährleisten und zu verhindern, dass Ihre Daten nicht berechtigten Benutzern zugänglich gemacht werden.
{: shortdesc}

Sie können sensible Daten auf verschiedenen Ebenen in Ihrem Cluster einrichten, die jeweils entsprechend geschützt werden müssen.
- **Clusterebene:** Clusterkonfigurationsdaten werden in der etcd-Komponente Ihres Kubernetes-Masters gespeichert. Daten in etcd werden auf dem lokalen Datenträger des Kubernetes-Masters gespeichert und in {{site.data.keyword.cos_full_notm}} gesichert. Daten werden während des Transits an {{site.data.keyword.cos_full_notm}} verschlüsselt und wenn sie ruhen. Sie können die Verschlüsselung für Ihre etcd-Daten auf der lokalen Festplatte Ihres Kubernetes-Masters aktivieren, indem Sie für Ihren Cluster die [{{site.data.keyword.keymanagementservicelong_notm}}-Verschlüsselung aktivieren](/docs/containers?topic=containers-encryption#encryption). Die etcd-Daten für Cluster, auf denen eine frühere Version von Kubernetes ausgeführt wird, werden auf einem verschlüsselten Datenträger gespeichert, der von IBM verwaltet und täglich gesichert wird.
- **Anwendungsebene:** Speichern Sie sensible Informationen wie Berechtigungsnachweise oder Schlüssel beim Bereitstellen einer App nicht in der YAML-Konfigurationsdatei, in Konfigurationszuordnungen oder Scripts. Verwenden Sie stattdessen [geheime Kubernetes-Schlüssel ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/configuration/secret/). Sie können auch [Daten in geheimen Kubernetes-Schlüsseln verschlüsseln](#keyprotect), um zu verhindern, dass nicht berechtigte Benutzer auf sensible Clusterinformationen zugreifen können.

Weitere Informationen zum Schützen des Clusters finden Sie unter [Sicherheit für {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-security#security).

<img src="images/cs_encrypt_ov.png" width="700" alt="Übersicht über die Clusterverschlüsselung" style="width:700px; border-style: none"/>

_Abbildung: Übersicht über die Datenverschlüsselung in einem Cluster_

1.  **etcd**: etcd ist die Komponente des Masters, in der die Daten Ihrer Kubernetes-Ressourcen gespeichert werden, z. B. die `.yaml`-Dateien und geheimen Schlüssel der Objektkonfiguration. Daten in etcd werden auf dem lokalen Datenträger des Kubernetes-Masters gespeichert und in {{site.data.keyword.cos_full_notm}} gesichert. Daten werden während des Transits an {{site.data.keyword.cos_full_notm}} verschlüsselt und wenn sie ruhen. Sie können die Verschlüsselung für Ihre etcd-Daten auf der lokalen Festplatte Ihres Kubernetes-Masters aktivieren, indem Sie für Ihren Cluster die [{{site.data.keyword.keymanagementservicelong_notm}}-Verschlüsselung aktivieren](#keyprotect). Die etcd-Daten in Clustern, auf denen eine frühere Version von Kubernetes ausgeführt wird, werden auf einem verschlüsselten Datenträger gespeichert, der von IBM verwaltet und täglich gesichert wird. Wenn etcd-Daten an einen Pod gesendet werden, werden sie über TLS verschlüsselt, um den Datenschutz und die Datenintegrität sicherzustellen.
2.  **Sekundäre Platte des Workerknotens**: Auf der sekundären Platte Ihres Workerknotens werden das Containerdateisystem und lokal extrahierte Images gespeichert. Die Platte wird mit 256-Bit-AES und mit einem LUKS-Verschlüsselungsschlüssel mit verschlüsselt, der auf dem Workerknoten eindeutig ist und als geheimer Schlüssel in etcd gespeichert ist, verwaltet von IBM. Wenn Sie Ihre Workerknoten erneut laden oder aktualisieren, werden die LUKS-Schlüssel turnusmäßig gewechselt.
3.  **Speicher**: Sie können Daten speichern, indem Sie [persistenten Datei-, Block- oder Objektspeicher einrichten](/docs/containers?topic=containers-storage_planning#persistent_storage_overview). Die Speicherinstanzen der IBM Cloud-Infrastruktur (SoftLayer) speichern die Daten auf verschlüsselten Festplatten, sodass Ihre ruhenden Daten verschlüsselt sind. Und wenn Sie sich für Objektspeicher entscheiden, werden Ihre Daten auch während des Transits verschlüsselt.
4.  **{{site.data.keyword.Bluemix_notm}}-Services**: Sie können [{{site.data.keyword.Bluemix_notm}}-Services in Ihren Cluster integrieren](/docs/containers?topic=containers-service-binding#bind-services), zum Beispiel {{site.data.keyword.registryshort_notm}} oder {{site.data.keyword.watson}}. Die Serviceberechtigungsnachweise werden in einem geheimen Schlüssel in etcd gespeichert. Ihre App kann auf den geheimen Schlüssel zugreifen, indem der geheime Schlüssel als Datenträger angehängt wird oder indem er als Umgebungsvariable in [Ihrer Implementierung](/docs/containers?topic=containers-app#secret) angegeben wird.
5.  **{{site.data.keyword.keymanagementserviceshort}}**: Wenn Sie [{{site.data.keyword.keymanagementserviceshort}}](#keyprotect) in Ihrem Cluster aktivieren, wird ein eingeschlossener Datenverschlüsselungsschlüssel in etcd gespeichert. Der Datenverschlüsselungsschlüssel verschlüsselt die geheimen Schlüssel in Ihrem Cluster, einschließlich der Serviceberechtigungsnachweise und des LUKS-Schlüssels. Da sich der Rootschlüssel in Ihrer {{site.data.keyword.keymanagementserviceshort}}-Instanz befindet, haben Sie die Kontrolle über den Zugriff auf Ihre verschlüsselten geheimen Schlüssel. Die {{site.data.keyword.keymanagementserviceshort}}-Schlüssel werden durch mit FIPS 140-2 Level 2 zertifizierte Hardwaresicherheitsmodule (HSM) gesichert, die gegen Informationsdiebstahl schützen. Weitere Informationen zur Funktionsweise der {{site.data.keyword.keymanagementserviceshort}}-Verschlüsselung finden Sie unter [Umschlagverschlüsselung](/docs/services/key-protect/concepts?topic=key-protect-envelope-encryption#envelope-encryption).

## Erläuterungen zur Verwendung von geheimen Schlüsseln
{: #secrets}

Geheime Kubernetes-Schlüssel stellen eine sichere Methode zum Speichern vertraulicher Informationen wie Benutzernamen, Kennwörter oder Schlüssel dar. Wenn Sie vertrauliche Informationen verschlüsseln müssen, [aktivieren Sie {{site.data.keyword.keymanagementserviceshort}}](#keyprotect), um die geheimen Schlüssel zu verschlüsseln. Weitere Informationen darüber, was Sie in geheimen Schlüsseln speichern können, finden Sie in der [Dokumentation zu Kubernetes ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/configuration/secret/).
{:shortdesc}

Überprüfen Sie die folgenden Tasks, für die geheime Schlüssel erforderlich sind.

### Service zu einem Cluster hinzufügen
{: #secrets_service}

Wenn Sie einen Service an einen Cluster binden, müssen Sie keinen geheimen Schlüssel erstellen, um die Serviceberechtigungsnachweise zu speichern. Ein geheimer Schlüssel wird automatisch für Sie erstellt. Weitere Informationen hierzu finden Sie unter [{{site.data.keyword.Bluemix_notm}}-Services zu Clustern hinzufügen](/docs/containers?topic=containers-service-binding#bind-services).
{: shortdesc}

### Datenverkehr zur App mit geheimen TLS-Schlüsseln verschlüsseln
{: #secrets_tls}

Die Lastausgleichsfunktion für Anwendungen verteilt die Lasten des HTTP-Netzverkehrs auf die Apps in Ihrem Cluster. Um auch einen Lastausgleich für eingehende HTTPS-Verbindungen durchführen zu können, können Sie die Lastausgleichsfunktion so konfigurieren, dass der Netzverkehr entschlüsselt und die entschlüsselte Anforderung an die Apps weitergeleitet wird, die in Ihrem Cluster zugänglich sind. Weitere Informationen finden Sie in der [Ingress-Dokumentation zur Konfiguration](/docs/containers?topic=containers-ingress#public_inside_3).
{: shortdesc}

Wenn Sie außerdem über Apps verfügen, für die das HTTPS-Protokoll erforderlich ist und der Datenverkehr verschlüsselt werden muss, können Sie geheime Schlüssel für die unidirektionale oder gegenseitige Authentifizierung mit der Annotation `ssl-services` verwenden. Weitere Informationen finden Sie in der [Ingress-Dokumentation zu Annotationen](/docs/containers?topic=containers-ingress_annotation#ssl-services).

### Auf Registry mit Berechtigungsnachweisen zugreifen, die in einem geheimer Kubernetes-Schlüssel für Image-Pull-Operationen gespeichert sind
{: #imagepullsecret}

Wenn Sie einen Cluster erstellen, werden die geheimen Schlüssel für die {{site.data.keyword.registrylong}}-Berechtigungsnachweise automatisch im Kubernetes-Namensbereich `default` erstellt. Sie müssen jedoch einen [eigenen geheimen Schlüssel für Image-Pull-Operationen für den Cluster erstellen](/docs/containers?topic=containers-images#other), wenn Sie in den folgenden Situationen einen Container bereitstellen möchten.
* Auf der Grundlage eines Image in der {{site.data.keyword.registryshort_notm}}-Registry für einen anderen Kubernetes-Namensbereich als `default`.
* Auf der Grundlage eines Image in der {{site.data.keyword.registryshort_notm}}-Registry, die in einer abweichenden {{site.data.keyword.Bluemix_notm}}-Region oder in einem anderem {{site.data.keyword.Bluemix_notm}}-Konto gespeichert ist.
* Auf der Grundlage eines Image, das in einer externen privaten Registry gespeichert ist.

<br />


## Lokale Festplatte und geheime Schlüssel des Kubernetes-Masters mithilfe von {{site.data.keyword.keymanagementserviceshort}} verschlüsseln (Beta)
{: #keyprotect}

Sie können die etcd-Komponente in Ihrem Kubernetes-Master und Ihre geheimen Kubernetes-Schlüssel mithilfe von [{{site.data.keyword.keymanagementservicefull}} ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](/docs/services/key-protect?topic=key-protect-getting-started-tutorial) als Kubernetes-[KMS-Provider (KMS - Key Management Service, Schlüsselmanagementservice) ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/tasks/administer-cluster/kms-provider/) in Ihrem Cluster schützen. Der KMS-Provider ist in der Kubernetes-Version 1.11 eine Alpha-Funktion, was die Integration von {{site.data.keyword.keymanagementserviceshort}} zu einem Beta-Release in {{site.data.keyword.containerlong_notm}} macht.
{: shortdesc}

Ihre Clusterkonfiguration und geheimen Kubernetes-Schlüssel werden standardmäßig in der etcd-Komponente des von IBM verwalteten Kubernetes-Masters gespeichert. Ihre Workerknoten verfügen auch über sekundäre Platten, die mithilfe von LUKS-Schlüsseln verschlüsselt werden, die von IBM verwaltet und als geheime Schlüssel in etcd gespeichert werden. Daten in etcd werden auf dem lokalen Datenträger des Kubernetes-Masters gespeichert und in {{site.data.keyword.cos_full_notm}} gesichert. Daten werden während des Transits an {{site.data.keyword.cos_full_notm}} verschlüsselt und wenn sie ruhen. Die Daten in der etcd-Komponente auf der lokalen Festplatte Ihres Kubernetes-Masters werden jedoch erst automatisch verschlüsselt, wenn Sie die {{site.data.keyword.keymanagementserviceshort}}-Verschlüsselung für Ihren Cluster aktivieren. Die etcd-Daten für Cluster, auf denen eine frühere Version von Kubernetes ausgeführt wird, werden auf einem verschlüsselten Datenträger gespeichert, der von IBM verwaltet und täglich gesichert wird.

Wenn Sie {{site.data.keyword.keymanagementserviceshort}} im Cluster aktivieren, wird Ihr eigener Rootschlüssel verwendet, um Daten in etcd zu verschlüsseln, einschließlich der geheimen LUKS-Schlüssel. Sie erhalten mehr Kontrolle über Ihre sensiblen Daten, wenn Sie die geheimen Schlüssel mit Ihrem Rootschlüssel verschlüsseln. Wenn Sie eine eigene Verschlüsselung verwenden, fügen Sie damit eine weitere Sicherheitsebene zu Ihren etcd-Daten und den geheimen Kubernetes-Schlüsseln hinzu und können genauer steuern, wer auf sensible Clusterinformationen zugreifen kann. Falls Sie den Zugriff auf etcd oder die geheimen Schlüssel irreversibel entfernen müssen, können Sie den Rootschlüssel löschen.

Löschen Sie keine Rootschlüssel in Ihrer {{site.data.keyword.keymanagementserviceshort}}-Instanz. Löschen Sie keine Schlüssel, auch dann nicht, wenn Sie rotieren, um einen neuen Schlüssel zu verwenden. Wenn Sie einen Rootschlüssel löschen, können Sie auf die Daten in etcd oder Daten aus den geheimen Schlüsseln in Ihrem Cluster nicht mehr zugreifen oder diese entfernen.
{: important}

Vorbereitende Schritte:
* [Melden Sie sich an Ihrem Konto an. Geben Sie als Ziel die entsprechende Region und, sofern zutreffend, die Ressourcengruppe an. Legen Sie den Kontext für den Cluster fest.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
* Stellen Sie sicher, dass im Cluster die Kubernetes-Version 1.11.3_1521 oder höher ausgeführt wird; setzen Sie hierzu `ibmcloud ks cluster-get --cluster <clustername_oder_-id>` ab und überprüfen Sie das Feld **Version**.
* Stellen Sie sicher, dass Sie die [{{site.data.keyword.Bluemix_notm}} IAM-Plattformrolle **Administrator**](/docs/containers?topic=containers-users#platform) für den Cluster innehaben.
* Stellen Sie sicher, dass der API-Schlüssel, der für die Region festgelegt ist, in der sich der Cluster befindet, zur Verwendung von Key Protect berechtigt ist. Führen Sie zum Überprüfen des API-Schlüsseleigners, dessen Berechtigungsnachweise für die Region gespeichert sind, den Befehl `ibmcloud ks api-key-info --cluster <clustername_oder_-id>` aus.

Gehen Sie wie folgt vor, um {{site.data.keyword.keymanagementserviceshort}} zu aktivieren oder die Instanz bzw. den Rootschlüssel zu aktualisieren, von der bzw. dem die geheimen Schlüssel im Cluster verschlüsselt werden:

1.  [Erstellen Sie eine {{site.data.keyword.keymanagementserviceshort}}-Instanz](/docs/services/key-protect?topic=key-protect-provision#provision).

2.  Rufen Sie die Serviceinstanz-ID ab.

    ```
    ibmcloud resource service-instance <kp-instanzname> | grep GUID
    ```
    {: pre}

3.  [Erstellen Sie einen Rootschlüssel](/docs/services/key-protect?topic=key-protect-create-root-keys#create-root-keys). Der Rootschlüssel wird standardmäßig ohne Ablaufdatum erstellt.

    Muss ein Ablaufdatum erstellt werden, weil interne Sicherheitsrichtlinien dies erforderlich machen? [Erstellen Sie den Rootschlüssel mit der API](/docs/services/key-protect?topic=key-protect-create-root-keys#create-root-key-api) und schließen Sie den Parameter `expirationDate` ein. **Wichtig:** Vor dem Ablaufen des Rootschlüssels müssen Sie diese Schritte wiederholen, um den den Cluster so zu aktualisieren, dass ein neuer Rootschlüssel verwendet wird. Andernfalls können Sie Ihre geheimen Schlüssel nicht entschlüsseln.
    {: tip}

4.  Notieren Sie die [Rootschlüssel-**ID**](/docs/services/key-protect?topic=key-protect-view-keys#view-keys-gui).

5.  Rufen Sie den [{{site.data.keyword.keymanagementserviceshort}}-Endpunkt](/docs/services/key-protect?topic=key-protect-regions#service-endpoints) der Instanz ab.

6.  Rufen Sie den Namen des Clusters ab, für den Sie {{site.data.keyword.keymanagementserviceshort}} aktivieren möchten.

    ```
    ibmcloud ks clusters
    ```
    {: pre}

7.  Aktivieren Sie {{site.data.keyword.keymanagementserviceshort}} im Cluster. Geben Sie die Flags mit den Informationen ein, die Sie zuvor abgerufen haben. Die Ausführung des Aktivierungsprozesses kann einige Zeit dauern.

    ```
    ibmcloud ks key-protect-enable --cluster <clustername_oder_-id> --key-protect-url <kp-endpunkt> --key-protect-instance <kp-instanz-id> --crk <kp-rootschlüssel-id>
    ```
    {: pre}

8.  Während der Aktivierung sind Sie möglicherweise nicht in der Lage, auf den Kubernetes-Master zuzugreifen, zum Beispiel um YAML-Konfigurationen für Bereitstellungen zu aktualisieren. Prüfen Sie in der Ausgabe des folgenden Befehls, ob für **Master Status** der Wert **Ready** angegeben wird.
    ```
    ibmcloud ks cluster-get <clustername_oder_-id>
    ```
    {: pre}

    Beispielausgabe, wenn die Aktivierung in Bearbeitung ist:
    ```
    Name:                   <clustername>   
    ID:                     <cluster-id>   
    ...
    Master Status:          Key Protect feature enablement in progress.  
    ```
    {: screen}

    Beispielausgabe, wenn der Master bereit ist:
    ```
    Name:                   <clustername>   
    ID:                     <cluster-id>   
    ...
    Master Status:          Ready (1 min ago)   
    ```
    {: screen}

    Nach der Aktivierung von {{site.data.keyword.keymanagementserviceshort}} im Cluster werden Daten in `etcd`, vorhandene geheime Schlüssel und neue geheime Schlüssel, die im Cluster erstellt werden, automatisch unter Verwendung des {{site.data.keyword.keymanagementserviceshort}}-Rootschlüssels verschlüsselt.

9.  Optional: Zum Rotieren Ihres Schlüssels wiederholen Sie diese Schritte mit einer neuen Rootschlüssel-ID. Der neue Rootschlüssel wird zusammen mit dem vorherigen Rootschlüssel zur Clusterkonfiguration hinzugefügt, sodass vorhandene verschlüsselte Daten geschützt bleiben.

Löschen Sie keine Rootschlüssel in Ihrer {{site.data.keyword.keymanagementserviceshort}}-Instanz. Löschen Sie keine Schlüssel, auch dann nicht, wenn Sie rotieren, um einen neuen Schlüssel zu verwenden. Wenn Sie einen Rootschlüssel löschen, können Sie auf die Daten in etcd oder Daten aus den geheimen Schlüsseln in Ihrem Cluster nicht mehr zugreifen oder diese entfernen.
{: important}


## Daten mit IBM Cloud Data Shield verschlüsseln (Beta)
{: #datashield}

{{site.data.keyword.datashield_short}} ist in Intel® Software Guard Extensions (SGX) und Fortanix®-Technologie integriert, sodass der Workload-Code und die Daten Ihres {{site.data.keyword.Bluemix_notm}}-Containers während der Nutzung geschützt werden. Der App-Code und die Daten werden in speziellen CPU-Schutzenklaven ausgeführt. Dabei handelt es sich um vertrauenswürdige Speicherbereiche auf dem Workerknoten, die kritische Aspekte der App schützen und dabei helfen, Code und Daten vertraulich und unverändert zu halten.
{: shortdesc}

Wenn es um den Schutz von Daten geht, gehört Verschlüsselung zu den gängigsten und wirksamsten Kontrollmechanismen. Allerdings müssen die Daten bei jedem Schritt ihres Lebenszyklus verschlüsselt werden. Daten durchlaufen in ihrem Lebenszyklus drei Phasen: als ruhende Daten, als bewegte Daten und als Daten im Gebrauch. Als ruhende und bewegte Daten werden Daten gemeinhin bezeichnet, wenn sie als gespeicherte Daten und als Daten während des Transports geschützt werden. Um diesen Schutz noch um einen Schritt zu erweitern, können Sie Daten jetzt im Gebrauch verschlüsseln.

Wenn Sie oder Ihr Unternehmen aufgrund interner Richtlinien, behördlicher Regelungen oder branchenspezifischer Konformitätsanforderungen eine Sicherheitsstufe für Daten benötigen, kann Sie diese Lösung beim Wechsel zur Cloud unterstützen. Beispiele für Lösungen beziehen sich auf Finanzdienstleister und Einrichtungen des Gesundheitswesen oder auf Länder mit behördlichen Richtlinien, die eine On-Premises-Cloud-Lösung erfordern.

Stellen Sie für den Einstieg einen SGX-fähigen Bare-Metal-Worker-Cluster mit dem Maschinentyp 'mb2c.4x32' bereit und machen Sie sich mit den Informationen in der [Dokumentation zu {{site.data.keyword.datashield_short}}](/docs/services/data-shield?topic=data-shield-getting-started#getting-started) vertraut.
