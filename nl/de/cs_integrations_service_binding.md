---

copyright:
  years: 2014, 2019
lastupdated: "2019-04-04"

keywords: kubernetes, iks, helm, without tiller, private cluster tiller, integrations, helm chart

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


# Services durch Binden von IBM Cloud-Services hinzufügen
{: #service-binding}

Fügen Sie {{site.data.keyword.Bluemix_notm}}-Services hinzu, um Ihren Kubernetes-Cluster mit zusätzlichen Funktionen in Bereichen wie Watson AI, Daten, Sicherheit und Internet of Things (IoT) zu erweitern.
{:shortdesc}

**Welche Arten von Services kann ich an meinen Cluster binden?** </br>
Wenn Sie {{site.data.keyword.Bluemix_notm}}-Services zu Ihrem Cluster hinzufügen, können Sie zwischen Services mit Fähigkeit für {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM) und Services, die auf Cloud Foundry basieren, wählen. IAM-fähige Services bieten eine differenziertere Zugriffssteuerung und können in einer {{site.data.keyword.Bluemix_notm}}-Ressourcengruppe verwaltet werden. Cloud Foundry-Services müssen zu einer Cloud Foundry-Organisation und einem Bereich hinzugefügt werden und können keiner Ressourcengruppe hinzugefügt werden. Zum Steuern des Zugriffs auf Ihre Cloud Foundry-Serviceinstanz verwenden Sie Cloud Foundry-Rollen. Weitere Informationen zu IAM-fähigen Services und Cloud Foundry-Services finden Sie unter [Was ist eine Ressource?](/docs/resources?topic=resources-resource#resource).

Eine Liste der unterstützten {{site.data.keyword.Bluemix_notm}}-Services finden Sie im [{{site.data.keyword.Bluemix_notm}}-Katalog](https://cloud.ibm.com/catalog).

**Was ist eine {{site.data.keyword.Bluemix_notm}}-Servicebindung?**</br>
Die Servicebindung ist eine schnelle Methode zum Erstellen von Serviceberechtigungsnachweisen für einen {{site.data.keyword.Bluemix_notm}}-Service und zum Speichern dieser Berechtigungsnachweise in einem geheimen Kubernetes-Schlüssel in Ihrem Cluster. Zum Binden eines Service an Ihren Cluster müssen Sie zuerst eine Instanz des Service bereitstellen. Anschließend verwenden Sie den Befehl `ibmcloud ks cluster-service-bind`[](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_service_bind), um die Serviceberechtigungsnachweise und den geheimen Kubernetes-Schlüssel zu erstellen. Der geheime Kubernetes-Schlüssel wird automatisch in 'etcd' verschlüsselt, um Ihre Daten zu schützen.

Sollen die geheimen Schlüssel noch sicherer werden? Wenden Sie sich zur [Aktivierung von {{site.data.keyword.keymanagementservicefull}}](/docs/containers?topic=containers-encryption#keyprotect) im Cluster an Ihren Clusteradministrator, um neue und vorhandene geheime Schlüssel zu verschlüsseln, zum Beispiel den geheimen Schlüssel, in dem die Berechtigungsnachweise der {{site.data.keyword.Bluemix_notm}}-Serviceinstanzen enthalten sind.
{: tip}

**Kann ich alle {{site.data.keyword.Bluemix_notm}}-Services in meinem Cluster verwenden?**</br>
Sie können die Servicebindung nur für Services verwenden, die Serviceschlüssel unterstützen, sodass die Serviceberechtigungsnachweise automatisch erstellt und in einem geheimen Kubernetes-Schlüssel gespeichert werden können. Eine Liste der Services, die Serviceschlüssel unterstützen, finden Sie im Abschnitt [Externen Apps die Verwendung von {{site.data.keyword.Bluemix_notm}}-Services ermöglichen](/docs/resources?topic=resources-externalapp#externalapp).

Services, die keine Serviceschlüssel unterstützen, stellen in der Regel eine API bereit, die Sie in Ihrer App verwenden können. Die Servicebindungsmethode konfiguriert den API-Zugriff für Ihre App nicht automatisch. Lesen Sie die API-Dokumentation zu Ihrem Service und implementieren Sie die API-Schnittstelle in Ihrer App.

## IBM Cloud-Services zu Clustern hinzufügen
{: #bind-services}

Verwenden Sie die {{site.data.keyword.Bluemix_notm}}-Servicebindung, um automatisch Serviceberechtigungsnachweise für Ihre {{site.data.keyword.Bluemix_notm}}-Services zu erstellen und diese in einem geheimen Kubernetes-Schlüssel zu speichern.
{: shortdesc}

Vorbereitende Schritte:
- Stellen Sie sicher, dass Sie die folgenden Rollen innehaben:
    - [Die {{site.data.keyword.Bluemix_notm}} IAM-Plattform-Zugriffsrolle](/docs/containers?topic=containers-users#platform) **Editor** oder **Administrator** für den Cluster, in dem Sie einen Service binden möchten
    - [Die {{site.data.keyword.Bluemix_notm}} IAM-Servicerolle](/docs/containers?topic=containers-users#platform) **Schreibberechtigter** oder **Manager** für den Kubernetes-Namensbereich, in dem Sie den Service binden möchten
    - Für Cloud Foundry-Services: [Die Cloud Foundry-Rolle](/docs/iam?topic=iam-mngcf#mngcf) **Entwickler** für den Bereich, in dem Sie den Service bereitstellen möchten
- [Melden Sie sich an Ihrem Konto an. Geben Sie als Ziel die entsprechende Region und, sofern zutreffend, die Ressourcengruppe an. Legen Sie den Kontext für den Cluster fest.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

Gehen Sie wie folgt vor, um Ihrem Cluster einen {{site.data.keyword.Bluemix_notm}}-Service hinzuzufügen:

1. [Erstellen Sie eine Instanz des {{site.data.keyword.Bluemix_notm}}-Service](/docs/resources?topic=resources-externalapp#externalapp).
    * Bestimmte {{site.data.keyword.Bluemix_notm}}-Services sind nur in ausgewählten Regionen verfügbar. Sie können einen Service nur an Ihren Cluster binden, wenn der Service in derselben Region wie Ihr Cluster verfügbar ist. Wenn Sie eine Serviceinstanz in der Zone 'Washington DC' erstellen wollen, müssen Sie außerdem die CLI verwenden.
    * **Für IAM-fähige Services**: Sie müssen die Serviceinstanz in derselben Ressourcengruppe wie den Cluster erstellen. Ein Service kann in nur einer Ressourcengruppe erstellt werden; eine Änderung ist danach nicht mehr möglich.

2. Überprüfen Sie den Servicetyp, den Sie erstellt haben, und notieren Sie die Serviceinstanz **Name**.
   - **Cloud Foundry-Services:**
     ```
     ibmcloud service list
     ```
     {: pre}

     Beispielausgabe:
     ```
     name                         service           plan    bound apps   last operation
     <cf_serviceinstanzname>      <servicename>     spark                create succeeded
     ```
     {: screen}

  - **{{site.data.keyword.Bluemix_notm}}IAM-fähige Services:**
     ```
     ibmcloud resource service-instances
     ```
     {: pre}

     Beispielausgabe:
     ```
     Name                          Location   State    Type               Tags
     <iam-serviceinstanzname>      <region>   active   service_instance
     ```
     {: screen}

   Sie können die verschiedenen Servicetypen auch in Ihrem {{site.data.keyword.Bluemix_notm}}-Dashboard als **Cloud Foundry-Services** und **Services** sehen.

3. Geben Sie den Clusternamensbereich an, den Sie verwenden wollen, um Ihren Service hinzuzufügen.
   ```
   kubectl get namespaces
   ```
   {: pre}

4. Binden Sie den Service an Ihren Cluster, um die Serviceberechtigungsnachweise für Ihren Service zu erstellen und die Berechtigungsnachweise in einem geheimen Kubernetes-Schlüssel zu speichern. Wenn Sie bereits über Serviceberechtigungsnachweise verfügen, verwenden Sie das Flag `--key`, um die Berechtigungsnachweise anzugeben. Für IAM-fähige Services werden die Berechtigungsnachweise automatisch mit der Servicezugriffsrolle **Schreibberechtigter** erstellt. Sie können aber auch das Flag `--role` verwenden, um eine andere Servicezugriffsrolle anzugeben. Wenn Sie das Flag `--key` verwenden, geben Sie das Flag `--role` nicht an.
   ```
   ibmcloud ks cluster-service-bind --cluster <clustername_oder-id> --namespace <namensbereich> --service <serviceinstanzname> [--key <serviceinstanzschlüssel>] [--role <iam-servicerolle>]
   ```
   {: pre}

   Wenn die Erstellung der Serviceberechtigungsnachweise erfolgreich ist, wird ein geheimer Kubernetes-Schlüssel mit dem Namen `binding-<service_instance_name>` erstellt.  

   Beispielausgabe:
   ```
   ibmcloud ks cluster-service-bind --cluster mycluster --namespace mynamespace --service cleardb
    Binding service instance to namespace...
    OK
    Namespace:	     mynamespace
    Secret name:     binding-<serviceinstanzname>
   ```
   {: screen}

5. Überprüfen Sie die Serviceberechtigungsnachweise in Ihrem geheimen Kubernetes-Schlüssel.
   1. Rufen Sie die Details des geheimen Schlüssels ab und notieren Sie den **binding**-Wert. Der **binding**-Wert ist Base64-codiert und enthält die Berechtigungsnachweise für Ihre Serviceinstanz im JSON-Format.
      ```
      kubectl get secrets binding-<serviceinstanzname> --namespace=<namensbereich> -o yaml
      ```
      {: pre}

      Beispielausgabe:
      ```
      apiVersion: v1
       data:
         binding: <bindung>
       kind: Secret
       metadata:
         annotations:
           service-instance-id: 1111aaaa-a1aa-1aa1-1a11-111aa111aa11
           service-key-id: 2b22bb2b-222b-2bb2-2b22-b22222bb2222
         creationTimestamp: 2018-08-07T20:47:14Z
         name: binding-<serviceinstanzname>
         namespace: <namensbereich>
         resourceVersion: "6145900"
         selfLink: /api/v1/namespaces/default/secrets/binding-mycloudant
         uid: 33333c33-3c33-33c3-cc33-cc33333333c
       type: Opaque
      ```
      {: screen}

   2. Decodieren Sie den 'bindung'-Wert.
      ```
      echo "<bindung>" | base64 -D
      ```
      {: pre}

      Beispielausgabe:
      ```
      {"apikey":"KL34Ys893284NGJEPFjgrioJ12NElpow","host":"98765aab-9ce1-7tr3-ba87-bfbab6e6d9d6-bluemix.cloudant.com","iam_apikey_description":"Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:cloudantnosqldb:us-south:a/1234g56789cfe8e6388dd2ec098:98746cw-43d7-49ce-947a-d8fe3eebb381::","iam_apikey_name":"auto-generated-apikey-1234abcde-987f-3t64-9d96-d13775ec5663","iam_role_crn":"crn:v1:bluemix:public:iam::::serviceRole:Writer","iam_serviceid_crn":"crn:v1:bluemix:public:iam-identity::a/1234567890brasge5htn2ec098::serviceid:ServiceId-12345vgh-6c4c-ytr12-af6b-467d30d6ef44","password":"jfiavhui12484fnivhuo472nvei23913c3ff","port":443,"url":"https://25c73aac-9ce1-4c24-ba98-bfbab6e6d9d6-bluemix:ugvioev823inreuiegn43donvri29989wiu9t22@25c73aac-9ce1-4c24-ba98-abdrjio123562lnsb-bluemix.cloudant.com","username":"123b45da-9ce1-4c24-ab12-rinwnwub1294-bluemix"}
      ```
      {: screen}

   3. Optional: Vergleichen Sie die Serviceberechtigungsnachweise, die Sie im vorherigen Schritt decodiert haben, mit den Serviceberechtigungsnachweisen, die Sie für Ihre Serviceinstanz im {{site.data.keyword.Bluemix_notm}}-Dashboard finden.

6. Ihr Service ist nun an Ihren Cluster gebunden und Sie müssen Ihre App für den [Zugriff auf die Serviceberechtigungsnachweise im geheimen Kubernetes-Schlüssel](#adding_app) konfigurieren.


## Über Apps auf Serviceberechtigungsnachweise zugreifen
{: #adding_app}

Um über Ihre App auf eine {{site.data.keyword.Bluemix_notm}}-Serviceinstanz zuzugreifen, müssen Sie die im geheimen Kubernetes-Schlüssel gespeicherten Serviceberechtigungsnachweise für Ihre App verfügbar machen.
{: shortdesc}

Die Berechtigungsnachweise einer Serviceinstanz sind Base64-codiert und werden in Ihrem geheimen Schlüssel im JSON-Format gespeichert. Wählen Sie eine der folgenden Optionen aus, um auf die Daten in Ihrem geheimen Schlüssel zuzugreifen:
- [Geheimen Schlüssel als Datenträger an Ihren Pod anhängen](#mount_secret)
- [Auf den geheimen Schlüssel in Umgebungsvariablen verweisen](#reference_secret)
<br>

Vorbereitende Schritte:
-  Stellen Sie sicher, dass Sie die [{{site.data.keyword.Bluemix_notm}} IAM-Servicerolle **Schreibberechtigter** oder **Manager**](/docs/containers?topic=containers-users#platform) für den Namensbereich `kube-system` innehaben.
- [Melden Sie sich an Ihrem Konto an. Geben Sie als Ziel die entsprechende Region und, sofern zutreffend, die Ressourcengruppe an. Legen Sie den Kontext für den Cluster fest.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
- [Fügen Sie einen {{site.data.keyword.Bluemix_notm}}-Service zu Ihrem Cluster hinzu](#bind-services).

### Geheimen Schlüssel als Datenträger an Ihren Pod anhängen
{: #mount_secret}

Wenn Sie den geheimen Schlüssel als Datenträger an Ihren Pod anhängen, wird eine Datei mit dem Namen `binding` im Mountverzeichnis des Datenträgers gespeichert. Die Datei `binding` im JSON-Format enthält sämtliche Informationen und Berechtigungsnachweise, die Sie benötigen, um auf den {{site.data.keyword.Bluemix_notm}}-Service zuzugreifen.
{: shortdesc}

1.  Listen Sie die verfügbaren geheimen Schlüssel in Ihrem Cluster auf und notieren Sie den **Namen** Ihres geheimen Schlüssels. Suchen Sie nach einem Schlüssel des Typs **Opaque**. Sollten mehrere geheime Schlüssel vorhanden sein, wenden Sie sich an Ihren Clusteradministrator, damit dieser den geheimen Schlüssel für den gewünschten Service ermittelt.
    ```
    kubectl get secrets
    ```
    {: pre}

    Beispielausgabe:
    ```
    NAME                              TYPE            DATA      AGE
    binding-<serviceinstanzname>      Opaque          1         3m
    ```
    {: screen}

2.  Erstellen Sie eine YAML-Datei für Ihre Kubernetes-Bereitstellung und hängen Sie den geheimen Schlüssel als Datenträger an Ihren Pod an.
    ```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      labels:
        app: secret-test
      name: secret-test
      namespace: <mein_namensbereich>
    spec:
      selector:
        matchLabels:
          app: secret-test
      replicas: 1
      template:
        metadata:
          labels:
            app: secret-test
        spec:
          containers:
          - image: registry.bluemix.net/ibmliberty:latest
            name: secret-test
            volumeMounts:
            - mountPath: <mountpfad>
              name: <datenträgername>
          volumes:
          - name: <datenträgername>
            secret:
              defaultMode: 420
              secretName: binding-<serviceinstanzname>
    ```
    {: codeblock}

    <table>
    <caption>Erklärung der Komponenten der YAML-Datei</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der YAML-Dateikomponenten</th>
    </thead>
    <tbody>
    <tr>
    <td><code>volumeMounts.mountPath</code></td>
    <td>Der absolute Pfad des Verzeichnisses, in dem der Datenträger innerhalb des Containers angehängt wird.</td>
    </tr>
    <tr>
    <td><code>volumeMounts.name</code></br><code>volumes.name</code></td>
    <td>Der Name des Datenträgers, der an Ihren Pod angehängt werden soll.</td>
    </tr>
    <tr>
    <td><code>secret.defaultMode</code></td>
    <td>Die Lese- und Schreibberechtigungen für den geheimen Schlüssel. Verwenden Sie `420`, um Leseberechtigungen festzulegen. </td>
    </tr>
    <tr>
    <td><code>secret.secretName</code></td>
    <td>Der Name des geheimen Schlüssels, den Sie im vorherigen Schritt notiert haben.</td>
    </tr></tbody></table>

3.  Erstellen Sie den Pod und hängen Sie den geheimen Schlüssel als Datenträger an.
    ```
    kubectl apply -f secret-test.yaml
    ```
    {: pre}

4.  Stellen Sie sicher, dass der Pod erstellt wurde.
    ```
    kubectl get pods
    ```
    {: pre}

    CLI-Beispielausgabe:

    ```
    NAME                           READY     STATUS    RESTARTS   AGE
    secret-test-1111454598-gfx32   1/1       Running   0          1m
    ```
    {: screen}

5.  Greifen Sie auf die Serviceberechtigungsnachweise zu.
    1. Melden Sie sich beim Pod an.
       ```
       kubectl exec <podname> -it bash
       ```
       {: pre}

    2. Navigieren Sie zu Ihrem Datenträgermountpfad, den Sie zuvor definiert haben, und listen Sie die Dateien in Ihrem Datenträgermountpfad auf.
       ```
       cd <datenträger-mountpfad> && ls
       ```
       {: pre}

       Beispielausgabe:
       ```
       binding
       ```
       {: screen}

       Die `binding`-Datei enthält die Serviceberechtigungsnachweise, die Sie im geheimen Kubernetes-Schlüssel gespeichert haben.

    4. Zeigen Sie die Serviceberechtigungsnachweise an. Die Berechtigungsnachweise werden als Schlüssel/Wert-Paare im JSON-Format gespeichert.
       ```
       cat binding
       ```
       {: pre}

       Beispielausgabe:
       ```
       {"apikey":"KL34Ys893284NGJEPFjgrioJ12NElpow","host":"98765aab-9ce1-7tr3-ba87-bfbab6e6d9d6-bluemix.cloudant.com","iam_apikey_description":"Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:cloudantnosqldb:us-south:a/1234g56789cfe8e6388dd2ec098:98746cw-43d7-49ce-947a-d8fe3eebb381::","iam_apikey_name":"auto-generated-apikey-1234abcde-987f-3t64-9d96-d13775ec5663","iam_role_crn":"crn:v1:bluemix:public:iam::::serviceRole:Writer","iam_serviceid_crn":"crn:v1:bluemix:public:iam-identity::a/1234567890brasge5htn2ec098::serviceid:ServiceId-12345vgh-6c4c-ytr12-af6b-467d30d6ef44","password":"jfiavhui12484fnivhuo472nvei23913c3ff","port":443,"url":"https://25c73aac-9ce1-4c24-ba98-bfbab6e6d9d6-bluemix:ugvioev823inreuiegn43donvri29989wiu9t22@25c73aac-9ce1-4c24-ba98-abdrjio123562lnsb-bluemix.cloudant.com","username":"123b45da-9ce1-4c24-ab12-rinwnwub1294-bluemix"}
       ```
       {: screen}

    5. Konfigurieren Sie Ihre App, um den JSON-Inhalt zu parsen und die Informationen abzurufen, die Sie für den Zugriff auf den Service benötigen.

### Auf den geheimen Schlüssel in Umgebungsvariablen verweisen
{: #reference_secret}

Sie können die Serviceberechtigungsnachweise und andere Schlüssel/Wert-Paare aus Ihrem geheimen Kubernetes-Schlüssel als Umgebungsvariablen zur Ihrer Bereitstellung hinzufügen.
{: shortdesc}

1. Listen Sie die verfügbaren geheimen Schlüssel in Ihrem Cluster auf und notieren Sie den **Namen** Ihres geheimen Schlüssels. Suchen Sie nach einem Schlüssel des Typs **Opaque**. Sollten mehrere geheime Schlüssel vorhanden sein, wenden Sie sich an Ihren Clusteradministrator, damit dieser den geheimen Schlüssel für den gewünschten Service ermittelt.
   ```
   kubectl get secrets
   ```
   {: pre}

   Beispielausgabe:
   ```
   NAME                              TYPE            DATA      AGE
    binding-<serviceinstanzname>      Opaque          1         3m
   ```
   {: screen}

2. Rufen Sie die Details Ihres geheimen Schlüssels ab, um potenzielle Schlüssel/Wert-Paare zu finden, auf die Sie als Umgebungsvariablen in Ihrem Pod verweisen können. Die Serviceberechtigungsnachweise werden im Schlüssel `bindung` Ihres geheimen Schlüssels gespeichert.
   ```
   kubectl get secrets binding-<serviceinstanzname> --namespace=<namensbereich> -o yaml
   ```
   {: pre}

   Beispielausgabe:
   ```
   apiVersion: v1
   data:
     binding: <bindung>
   kind: Secret
   metadata:
     annotations:
       service-instance-id: 7123acde-c3ef-4ba2-8c52-439ac007fa70
       service-key-id: 9h30dh8a-023f-4cf4-9d96-d12345ec7890
     creationTimestamp: 2018-08-07T20:47:14Z
     name: binding-<serviceinstanzname>
     namespace: <namensbereich>
     resourceVersion: "6145900"
     selfLink: /api/v1/namespaces/default/secrets/binding-mycloudant
     uid: 12345a31-9a83-11e8-ba83-cd49014748f
   type: Opaque
   ```
   {: screen}

3. Erstellen Sie eine YAML-Datei für Ihre Kubernetes-Bereitstellung und geben Sie eine Umgebungsvariable an, die auf den Schlüssel `binding` verweist.
   ```
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     labels:
       app: secret-test
     name: secret-test
     namespace: <mein_namensbereich>
   spec:
     selector:
       matchLabels:
         app: secret-test
     template:
       metadata:
         labels:
           app: secret-test
       spec:
         containers:
         - image: registry.bluemix.net/ibmliberty:latest
           name: secret-test
           env:
           - name: BINDING
             valueFrom:
               secretKeyRef:
                 name: binding-<serviceinstanzname>
                 key: binding
     ```
     {: codeblock}

     <table>
     <caption>Erklärung der Komponenten der YAML-Datei</caption>
     <thead>
     <th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der YAML-Dateikomponenten</th>
     </thead>
     <tbody>
     <tr>
     <td><code>containers.env.name</code></td>
     <td>Der Name Ihrer Umgebungsvariablen.</td>
     </tr>
     <tr>
     <td><code>env.valueFrom.secretKeyRef.name</code></td>
     <td>Der Name des geheimen Schlüssels, den Sie im vorherigen Schritt notiert haben.</td>
     </tr>
     <tr>
     <td><code>env.valueFrom.secretKeyRef.key</code></td>
     <td>Der Schlüssel, der Teil Ihres geheimen Schlüssels ist und auf den Sie in Ihrer Umgebungsvariablen verweisen möchten. Wenn Sie auf die Serviceberechtigungsnachweise verweisen möchten, müssen Sie den Schlüssel <strong>binding</strong> verwenden.  </td>
     </tr>
     </tbody></table>

4. Erstellen Sie den Pod, der auf den Schlüssel `binding` des geheimen Schlüssels als Umgebungsvariable verweist.
   ```
   kubectl apply -f secret-test.yaml
   ```
   {: pre}

5. Stellen Sie sicher, dass der Pod erstellt wurde.
   ```
   kubectl get pods
   ```
   {: pre}

   CLI-Beispielausgabe:
   ```
   NAME                           READY     STATUS    RESTARTS   AGE
    secret-test-1111454598-gfx32   1/1       Running   0          1m
   ```
   {: screen}

6. Stellen Sie sicher, dass die Umgebungsvariable richtig festgelegt ist.
   1. Melden Sie sich beim Pod an.
      ```
      kubectl exec <podname> -it bash
      ```
      {: pre}

   2. Listen Sie alle Umgebungsvariablen im Pod auf.
      ```
      env
      ```
      {: pre}

      Beispielausgabe:
      ```
      BINDING={"apikey":"KL34Ys893284NGJEPFjgrioJ12NElpow","host":"98765aab-9ce1-7tr3-ba87-bfbab6e6d9d6-bluemix.cloudant.com","iam_apikey_description":"Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:cloudantnosqldb:us-south:a/1234g56789cfe8e6388dd2ec098:98746cw-43d7-49ce-947a-d8fe3eebb381::","iam_apikey_name":"auto-generated-apikey-1234abcde-987f-3t64-9d96-d13775ec5663","iam_role_crn":"crn:v1:bluemix:public:iam::::serviceRole:Writer","iam_serviceid_crn":"crn:v1:bluemix:public:iam-identity::a/1234567890brasge5htn2ec098::serviceid:ServiceId-12345vgh-6c4c-ytr12-af6b-467d30d6ef44","password":"jfiavhui12484fnivhuo472nvei23913c3ff","port":443,"url":"https://25c73aac-9ce1-4c24-ba98-bfbab6e6d9d6-bluemix:ugvioev823inreuiegn43donvri29989wiu9t22@25c73aac-9ce1-4c24-ba98-abdrjio123562lnsb-bluemix.cloudant.com","username":"123b45da-9ce1-4c24-ab12-rinwnwub1294-bluemix"}
      ```
      {: screen}

7. Konfigurieren Sie Ihre App zum Lesen der Umgebungsvariablen und zum Parsen des JSON-Inhalts, um die Informationen abzurufen, die Sie für den Zugriff auf den Service benötigen.

   Beispielcode in Python:
   ```python
   if os.environ.get('BINDING'):
        credentials = json.loads(os.environ.get('BINDING'))
   ```
   {: codeblock}

8. Optional: Fügen Sie der App als Vorsichtsmaßnahme eine Fehlerbehandlung hinzu für den Fall, dass die Umgebungsvariable `BINDING` nicht ordnungsgemäß festgelegt ist. 
   
   Beispielcode in Java: 
   ```java
   if (System.getenv("BINDING") == null) {
    throw new RuntimeException("Environment variable 'SECRET' is not set!");
   }
   ```
   {: codeblock}
   
   Beispielcode in Node.js:
   ```js
   if (!process.env.BINDING) {
    console.error('ENVIRONMENT variable "BINDING" is not set!');
    process.exit(1);
   }
   ```
   {: codeblock}

