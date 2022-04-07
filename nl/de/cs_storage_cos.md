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



# Daten in IBM Cloud Object Storage speichern
{: #object_storage}

[{{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-about#about) ist ein persistenter, hoch verfügbarer Speicher, den Sie mithilfe des {{site.data.keyword.cos_full_notm}}-Plug-ins an Apps anhängen können, die in einem Kubernetes-Cluster ausgeführt werden. Das Plug-in ist ein Kubernetes-Flex-Volume-Plug-in, das in der Cloud befindliche {{site.data.keyword.cos_short}}-Buckets mit Pods in Ihrem Cluster verbindet. Die Informationen, die mit {{site.data.keyword.cos_full_notm}} gespeichert werden, werden als bewegte und ruhende Daten verschlüsselt, über mehrere geografische Standorte verteilt und über HTTP durch eine REST-API zugänglich gemacht.

Zum Herstellen einer Verbindung zu {{site.data.keyword.cos_full_notm}} muss Ihr Cluster über einen öffentlichen Netzzugriff verfügen, um sich mithilfe von {{site.data.keyword.Bluemix_notm}} Identity and Access Management zu authentifizieren. Wenn Sie einen nur privaten Cluster haben, können Sie mit dem privaten Serviceendpunkt für {{site.data.keyword.cos_full_notm}} kommunizieren, wenn Sie die Plug-in-Version `1.0.3` oder höher installieren und Ihre {{site.data.keyword.cos_full_notm}}-Serviceinstanz für die HMAC-Authentifizierung einrichten. Wenn Sie die HMAC-Authentifizierung nicht verwenden wollen, müssen Sie allen ausgehenden Netzverkehr über Port 443 öffnen, damit das Plug-in in einem privaten Cluster ordnungsgemäß funktioniert.
{: important}

## Objektspeicher-Serviceinstanz erstellen
{: #create_cos_service}

Bevor Sie mit der Verwendung von Objektspeicher in Ihrem Cluster beginnen, müssen Sie eine {{site.data.keyword.cos_full_notm}}-Serviceinstanz in Ihrem Konto bereitstellen.
{: shortdesc}

Das {{site.data.keyword.cos_full_notm}}-Plug-in ist zur Arbeit mit einem beliebigen s3-API-Endpunkt konfiguriert. Sie könnten zum Beispiel einen lokalen Cloud Object Storage-Server wie [Minio](https://cloud.ibm.com/kubernetes/solutions/helm-charts/ibm-charts/ibm-minio-objectstore) verwenden oder eine Verbindung zu einem s3-API-Endpunkt herstellen wollen, der bei einem anderen Cloud-Provider eingerichtet ist, anstatt eine {{site.data.keyword.cos_full_notm}}-Serviceinstanz zu verwenden.

Führen Sie die folgenden Schritte aus, um eine {{site.data.keyword.cos_full_notm}}-Serviceinstanz zu erstellen. Wenn Sie planen, einen lokalen Cloud Object Storage-Server oder einen anderen s3-API-Endpunkt zu verwenden, finden Sie Informationen zur Einrichtung Ihrer Cloud Object Storage-Instanz in der Dokumentation des Providers.

1. Stellen Sie eine {{site.data.keyword.cos_full_notm}}-Serviceinstanz bereit.
   1.  Öffnen Sie die [{{site.data.keyword.cos_full_notm}}-Katalogseite](https://cloud.ibm.com/catalog/services/cloud-object-storage).
   2.  Geben Sie einen Namen für Ihre Serviceinstanz ein (zum Beispiel `cos-backup`) und wählen Sie dieselbe Ressourcengruppe wie die aus, in der sich der Cluster befindet. Wenn Sie die Ressourcengruppe des Clusters anzeigen möchten, führen Sie den Befehl `ibmcloud ks cluster-get --cluster <clustername_oder_-id>` aus.   
   3.  Sehen Sie sich die [Planoptionen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api) mit den Preisinformationen an und wählen Sie einen Plan aus.
   4.  Klicken Sie auf **Erstellen**. Die Seite mit den Servicedetails wird geöffnet.
2. {: #service_credentials}Rufen Sie die {{site.data.keyword.cos_full_notm}}-Serviceberechtigungsnachweise ab.
   1.  Klicken Sie in der Navigation auf der Seite mit den Servicedetails auf **Serviceberechtigungsnachweise**.
   2.  Klicken Sie auf **Neuer Berechtigungsnachweis**. Ein Dialogfeld wird angezeigt.
   3.  Geben Sie einen Namen für Ihre Berechtigungsnachweise ein.
   4.  Wählen Sie im Dropdown-Menü **Rolle** die Option `Schreibberechtigter` oder `Manager` aus. Wenn Sie `Leseberechtigter` auswählen, können Sie nicht die Berechtigungsnachweise verwenden, um Buckets in {{site.data.keyword.cos_full_notm}} zu erstellen und Daten zu schreiben.
   5.  Optional: Geben Sie `{"HMAC":true}` in **Inline-Konfigurationsparameter hinzufügen (optional):** ein, um zusätzliche HMAC-Berechtigungsnachweise für den {{site.data.keyword.cos_full_notm}}-Service zu erstellen. Durch die HMAC-Authentifizierung erhält die OAuth2-Authentifizierung eine zusätzliche Sicherheitsebene, da verhindert wird, dass verfallene oder zufällig erstellte OAuth2-Tokens verwendet werden. **Wichtig:** Wenn Sie einen nur privaten Cluster ohne öffentlichen Zugriff haben, müssen Sie die HMAC-Authentifizierung verwenden, damit Sie auf den {{site.data.keyword.cos_full_notm}}-Service über das private Netz zugreifen können.
   6.  Klicken Sie auf **Hinzufügen**. Ihre neuen Berechtigungsnachweise werden in der Tabelle **Serviceberechtigungsnachweise** aufgelistet.
   7.  Klicken Sie auf **Berechtigungsnachweise anzeigen**.
   8.  Notieren Sie sich den **apikey**, um OAuth2-Tokens für die Authentifizierung beim {{site.data.keyword.cos_full_notm}}-Service zu verwenden. Beachten Sie bei der HMAC-Authentifizierung im Abschnitt **cos_hmac_keys** die Angaben **access_key_id** und **secret_access_key**.
3. [Speichern Sie Ihre Serviceberechtigungsnachweise in einem geheimen Kubernetes-Schlüssel in dem Cluster](#create_cos_secret), um den Zugriff auf Ihre {{site.data.keyword.cos_full_notm}}-Serviceinstanz zu ermöglichen.

## Geheimen Schlüssel für die Berechtigungsnachweise des Objektspeicherservice erstellen
{: #create_cos_secret}

Um auf Ihre {{site.data.keyword.cos_full_notm}}-Serviceinstanz für das Lesen und Schreiben von Daten zugreifen zu können, müssen Sie die Serviceberechtigungsnachweise in einem geheimen Kubernetes-Schlüssel sicher speichern. Das {{site.data.keyword.cos_full_notm}}-Plug-in verwendet diese Berechtigungsnachweise für alle Lese- und Schreiboperationen in Ihrem Bucket.
{: shortdesc}

Führen Sie die folgenden Schritte aus, um einen geheimen Kubernetes-Schlüssel für die Berechtigungsnachweise einer {{site.data.keyword.cos_full_notm}}-Serviceinstanz zu erstellen. Wenn Sie planen, einen lokalen Cloud Object Storage-Server oder einen anderen s3-API-Endpunkt zu verwenden, erstellen Sie einen geheimen Kubernetes-Schlüssel mit den entsprechenden Berechtigungsnachweisen.

Vorbereitende Schritte: [Melden Sie sich an Ihrem Konto an. Geben Sie als Ziel die entsprechende Region und, sofern zutreffend, die Ressourcengruppe an. Legen Sie den Kontext für den Cluster fest.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1. Rufen Sie **apikey** oder **access_key_id** und **secret_access_key** Ihrer [ {{site.data.keyword.cos_full_notm}}-Serviceberechtigungsnachweise](#service_credentials) ab.

2. Rufen Sie die **GUID** Ihrer {{site.data.keyword.cos_full_notm}}-Serviceinstanz ab.
   ```
   ibmcloud resource service-instance <servicename> | grep GUID
   ```
   {: pre}
   
3. Erstellen Sie einen geheimen Kubernetes-Schlüssel, um Ihre Berechtigungsnachweise zu speichern. Wenn Sie Ihren geheimen Schlüssel erstellen, werden alle Werte automatisch in Base64 codiert. 
   
   **Beispiel für die Verwendung des API-Schlüssels:**
   ```
   kubectl create secret generic cos-write-access --type=ibm/ibmc-s3fs --from-literal=api-key=<api_key> --from-literal=service-instance-id=<service_instance_guid>
   ```
   {: pre}
   
   **Beispiel für HMAC-Authentifizierung:**
   ```
   kubectl create secret generic cos-write-access --type=ibm/ibmc-s3fs --from-literal=access-key=<access_key_ID> --from-literal=secret-key=<secret_access_key>    
   ```
   {: pre}

   <table>
   <caption>Erklärung der Befehlskomponenten</caption>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der Befehlskomponenten</th>
   </thead>
   <tbody>
   <tr>
   <td><code>api-key</code></td>
   <td>Geben Sie den API-Schlüssel ein, den Sie zuvor aus Ihren {{site.data.keyword.cos_full_notm}}-Serviceberechtigungsnachweisen abgerufen haben. Wenn Sie die HMAC-Authentifizierung verwenden möchten, geben Sie stattdessen <code>access-key</code> und <code>secret-key</code> an.  </td>
   </tr>
   <tr>
   <td><code>access-key</code></td>
   <td>Geben Sie die Zugriffsschlüssel-ID ein, die Sie zuvor aus Ihren {{site.data.keyword.cos_full_notm}}-Serviceberechtigungsnachweisen abgerufen haben. Wenn Sie die OAuth2-Authentifizierung verwenden möchten, geben Sie stattdessen <code>api-key</code> an.</td>
   </tr>
   <tr>
   <td><code>secret-key</code></td>
   <td>Geben Sie den geheimen Zugriffsschlüssel ein, den Sie zuvor aus Ihren {{site.data.keyword.cos_full_notm}}-Serviceberechtigungsnachweisen abgerufen haben. Wenn Sie die OAuth2-Authentifizierung verwenden möchten, geben Sie stattdessen <code>api-key</code> an.</td>
   </tr>
   <tr>
   <td><code>service-instance-id</code></td>
   <td>Geben Sie die GUID der {{site.data.keyword.cos_full_notm}}-Serviceinstanz ein, die Sie zuvor abgerufen haben. </td>
   </tr>
   </tbody>
   </table>

4. Stellen Sie sicher, dass der geheime Schlüssel in Ihrem Namensbereich erstellt wird.
   ```
   kubectl get secret
   ```
   {: pre}

5. [Installieren Sie das {{site.data.keyword.cos_full_notm}}-Plug-in](#install_cos) oder, falls Sie das Plug-in bereits installiert haben, [entscheiden Sie über die Konfiguration]( #configure_cos) für Ihr {{site.data.keyword.cos_full_notm}}-Bucket.

## IBM Cloud Object Storage-Plug-in installieren
{: #install_cos}

Installieren Sie das {{site.data.keyword.cos_full_notm}}-Plug-in mit einem Helm-Diagramm, um vordefinierte Speicherklassen für {{site.data.keyword.cos_full_notm}} einzurichten. Mit diesen Speicherklassen können Sie einen PVC zum Bereitstellen von {{site.data.keyword.cos_full_notm}} für Ihre Apps erstellen.
{: shortdesc}

Suchen Sie nach Anweisungen zum Aktualisieren oder Entfernen des {{site.data.keyword.cos_full_notm}}-Plug-ins? Weitere Informationen hierzu finden Sie in den Abschnitten [Plug-in aktualisieren](#update_cos_plugin) und [Plug-in entfernen](#remove_cos_plugin).
{: tip}

Vorbereitende Schritte: [Melden Sie sich an Ihrem Konto an. Geben Sie als Ziel die entsprechende Region und, sofern zutreffend, die Ressourcengruppe an. Legen Sie den Kontext für den Cluster fest.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1. Stellen Sie sicher, dass auf dem Workerknoten das neueste Patch für die Nebenversion angewendet wird.
   1. Listen Sie die aktuelle Patchversion der Workerknoten auf.
      ```
      ibmcloud ks workers --cluster <clustername_oder_-id>
      ```
      {: pre}

      Beispielausgabe:
      ```
      OK
      ID                                                  Public IP        Private IP     Machine Type           State    Status   Zone    Version
      kube-dal10-crb1a23b456789ac1b20b2nc1e12b345ab-w26   169.xx.xxx.xxx    10.xxx.xx.xxx   b3c.4x16.encrypted     normal   Ready    dal10   1.12.7_1523*
      ```
      {: screen}

      Wenn auf dem Workerknoten nicht die neueste Patchversion angewendet wird, wird ein Stern (`*`) in der Spalte **Version** der Ausgabe der Befehlszeilenschnittstelle angezeigt.

   2. Überprüfen Sie das [Versionsänderungsprotokoll](/docs/containers?topic=containers-changelog#changelog) und suchen Sie nach Änderungen, die in der neuesten Patchversion enthalten sind.

   3. Wenden Sie die neueste Patchversion durch erneutes Laden des Workerknotens an. Gehen Sie gemäß den Anweisungen für den Befehl [ibmcloud ks worker-reload](/docs/containers?topic=containers-cs_cli_reference#cs_worker_reload) vor, um sorgfältig alle aktiven Pods auf dem Workerknoten erneut zu planen, bevor der Workerknoten erneut geladen wird. Beachten Sie, dass Ihre Workerknotenmaschine während eines Neuladens mit dem neuen Image aktualisiert wird und dass dabei Daten gelöscht werden, die nicht [außerhalb des Workerknotens gespeichert sind](/docs/containers?topic=containers-storage_planning#persistent_storage_overview).

2.  [Befolgen Sie die Anweisungen](/docs/containers?topic=containers-helm#public_helm_install) zum Installieren des Helm-Clients auf Ihrer lokalen Maschine und installieren Sie den Helm-Server (tiller) mit einem Servicekonto in Ihrem Cluster.

    Die Installation des Helm-Servers Tiller erfordert eine öffentliche Netzverbindung zur öffentlichen Google-Container-Registry. Wenn Ihr Cluster auf das öffentliche Netz nicht zugreifen kann, wie dies zum Beispiel bei einem privaten Cluster hinter einer Firewall oder bei einem Cluster mit nur einem aktivierten privaten Serviceendpunkt der Fall ist, haben Sie die Wahl, [">das Tiller-Image auf Ihre lokale Maschine zu extrahieren und durch eine Push-Operation in Ihren Namensbereich in {{site.data.keyword.registryshort_notm}}](/docs/containers?topic=containers-helm#private_local_tiller) zu übertragen oder [das Helm-Diagramm ohne Verwendung von Tiller zu installieren](/docs/containers?topic=containers-helm#private_install_without_tiller).
    {: note}

3.  Überprüfen Sie, ob 'tiller' mit einem Servicekonto installiert ist.

    ```
    kubectl get serviceaccount -n kube-system tiller
    ```
    {: pre}

    Beispielausgabe:

    ```
    NAME                                 SECRETS   AGE
    tiller                               1         2m
    ```
    {: screen}

4. Fügen Sie das {{site.data.keyword.Bluemix_notm}}-Helm-Repository zu Ihrem Cluster hinzu.
   ```
   helm repo add iks-charts https://icr.io/helm/iks-charts
   ```
   {: pre}

5. Aktualisieren Sie das Helm-Repository, um die aktuelle Version aller Helm-Diagramme in diesem Repository abzurufen.
   ```
   helm repo update
   ```
   {: pre}

6. Laden Sie die Helm-Diagramme herunter und entpacken Sie die Diagramme im aktuellen Verzeichnis.
   ```
   helm fetch --untar iks-charts/ibmcloud-object-storage-plugin
   ```
   {: pre}

7. Wenn Sie eine Mac OS- oder Linux-Distribution verwenden, installieren Sie das {{site.data.keyword.cos_full_notm}}-Helm-Plug-in `ibmc`. Das Plug-in wird verwendet, um automatisch Ihre Clusterposition abzurufen und den API-Endpunkt für Ihre {{site.data.keyword.cos_full_notm}}-Buckets in Ihren Speicherklassen festzulegen. Wenn Sie Windows als Betriebssystem verwenden, fahren Sie mit dem nächsten Schritt fort.
   1. Installieren Sie das Helm-Plug-in.
      ```
      helm plugin install ibmcloud-object-storage-plugin/helm-ibmc
      ```
      {: pre}

      Beispielausgabe:
      ```
      Installed plugin: ibmc
      ```
      {: screen}

   2. Stellen Sie sicher, dass das Plug-in `ibmc` erfolgreich installiert wird.
      ```
      helm ibmc --help
      ```
      {: pre}

      Beispielausgabe:
      ```
      Install or upgrade Helm charts in IBM K8S Service

      Available Commands:
       helm ibmc install [CHART][flags]              Install a Helm chart
       helm ibmc upgrade [RELEASE][CHART] [flags]    Upgrades the release to a new version of the Helm chart

      Available Flags:
    --verbos                      (Optional) Verbosity intensifies... ...
       -f, --values valueFiles       (Optional) specify values in a YAML file (can specify multiple) (default [])
       -h, --help                    (Optional) This text.
       -u, --update                  (Optional) Update this plugin to the latest version

      Beispielverwendung:
       helm ibmc install iks-charts/ibmcloud-object-storage-plugin -f ./ibmcloud-object-storage-plugin/ibm/values.yaml
      ```
      {: screen}

      Wenn die Ausgabe den Fehler `Error: fork/exec /home/iksadmin/.helm/plugins/helm-ibmc/ibmc.sh: permission denied` anzeigt, führen Sie den Befehl `chmod 755 ~/.helm/plugins/helm-ibmc/ibmc.sh` aus. Führen Sie anschließend den Befehl `helm ibmc --help` erneut aus.
      {: tip}

8. Optional: Begrenzen Sie den Zugriff des {{site.data.keyword.cos_full_notm}}-Plug-ins auf geheime Kubernetes-Schlüssel, die Ihre {{site.data.keyword.cos_full_notm}}-Serviceberechtigungsnachweise enthalten. Standardmäßig ist das Plug-in berechtigt, auf alle geheimen Kubernetes-Schlüssel in Ihrem Cluster zuzugreifen.
   1. [Erstellen Sie Ihre {{site.data.keyword.cos_full_notm}}-Serviceinstanz](#create_cos_service).
   2. [Speichern Sie Ihre {{site.data.keyword.cos_full_notm}}-Serviceberechtigungsnachweise in einem geheimen Kubernetes-Schlüssel](#create_cos_secret).
   3. Navigieren Sie zum Verzeichnis `templates` und listen Sie die verfügbaren Dateien auf.  
      ```
      cd ibmcloud-object-storage-plugin/templates && ls
      ```
      {: pre}

   4. Öffnen Sie die Datei `provisioner-sa.yaml` und suchen Sie nach der Clusterrollendefinition (`ClusterRole`) mit dem Namen `ibmcloud-object-storage-secret-reader`.
   6. Fügen Sie den Namen des zuvor erstellten geheimen Schlüssels zur Liste der geheimen Schlüssel hinzu, auf die das Plug-in zugreifen kann (im Abschnitt `resourceNames`).
      ```
      kind: ClusterRole
      apiVersion: rbac.authorization.k8s.io/v1beta1
      metadata:
        name: ibmcloud-object-storage-secret-reader
      rules:
      - apiGroups: [""]
        resources: ["secrets"]
        resourceNames: ["<name_des_geheimen_schlüssels1>","<name_des_geheimen_schlüssels2>"]
        verbs: ["get"]
      ```
      {: codeblock}
   7. Speichern Sie Ihre Änderungen.

9. Installieren Sie das {{site.data.keyword.cos_full_notm}}-Plug-in. Wenn Sie das Plug-in installieren, werden vordefinierte Speicherklassen zu Ihrem Cluster hinzugefügt.

   - **Für Mac OS und Linux:**
     - Falls Sie den vorherigen Schritt übersprungen haben, führen Sie die Installation ohne Einschränkung für bestimmte geheime Kubernetes-Schlüssel durch.
       ```
       helm ibmc install iks-charts/ibmcloud-object-storage-plugin --name ibmcloud-object-storage-plugin -f ibmcloud-object-storage-plugin/ibm/values.yaml
       ```
       {: pre}

     - Falls Sie den vorherigen Schritt ausgeführt haben, führen Sie die Installation mit einer Einschränkung für bestimmte geheime Kubernetes-Schlüssel durch.
       ```
       helm ibmc install ./ibmcloud-object-storage-plugin --name ibmcloud-object-storage-plugin -f ibmcloud-object-storage-plugin/ibm/values.yaml
       ```
       {: pre}

   - **Für Windows:
    **
     1. Rufen Sie die Zone ab, in der der Cluster bereitgestellt wird, und speichern Sie die Zone in einer Umgebungsvariablen.
        ```
        export DC_NAME=$(kubectl get cm cluster-info -n kube-system -o jsonpath='{.data.cluster-config\.json}' | grep datacenter | awk -F ': ' '{print $2}' | sed 's/\"//g' |sed 's/,//g')
        ```
        {: pre}

     2. Stellen Sie sicher, dass die Umgebungsvariable festgelegt ist.
        ```
        printenv
        ```
        {: pre}

     3. Installieren Sie das Helm-Diagramm.
        - Falls Sie den vorherigen Schritt übersprungen haben, führen Sie die Installation ohne Einschränkung für bestimmte geheime Kubernetes-Schlüssel durch.
          ```
          helm install iks-charts/ibmcloud-object-storage-plugin --set dcname="$DC_NAME" --name ibmcloud-object-storage-plugin -f ibmcloud-object-storage-plugin/ibm/values.yaml
          ```
          {: pre}

        - Falls Sie den vorherigen Schritt ausgeführt haben, führen Sie die Installation mit einer Einschränkung für bestimmte geheime Kubernetes-Schlüssel durch.
          ```
          helm install ./ibmcloud-object-storage-plugin --set dcname="$DC_NAME" --name ibmcloud-object-storage-plugin -f ibmcloud-object-storage-plugin/ibm/values.yaml
          ```
          {: pre}

   Beispielausgabe:
   ```
   Installing the Helm chart
   DC: dal10  Chart: ibm/ibmcloud-object-storage-plugin
   NAME:   mewing-duck
   LAST DEPLOYED: Mon Jul 30 13:12:59 2018
   NAMESPACE: default
   STATUS: DEPLOYED

   RESOURCES:
   ==> v1/Pod(related)
   NAME                                             READY  STATUS             RESTARTS  AGE
   ibmcloud-object-storage-driver-hzqp9             0/1    ContainerCreating  0         1s
   ibmcloud-object-storage-driver-jtdb9             0/1    ContainerCreating  0         1s
   ibmcloud-object-storage-driver-tl42l             0/1    ContainerCreating  0         1s
   ibmcloud-object-storage-plugin-7d87fbcbcc-wgsn8  0/1    ContainerCreating  0         1s

   ==> v1/StorageClass
   NAME                                  PROVISIONER       AGE
   ibmc-s3fs-cold-cross-region           ibm.io/ibmc-s3fs  1s
   ibmc-s3fs-cold-regional               ibm.io/ibmc-s3fs  1s
   ibmc-s3fs-flex-cross-region           ibm.io/ibmc-s3fs  1s
   ibmc-s3fs-flex-perf-cross-region      ibm.io/ibmc-s3fs  1s
   ibmc-s3fs-flex-perf-regional          ibm.io/ibmc-s3fs  1s
   ibmc-s3fs-flex-regional               ibm.io/ibmc-s3fs  1s
   ibmc-s3fs-standard-cross-region       ibm.io/ibmc-s3fs  1s
   ibmc-s3fs-standard-perf-cross-region  ibm.io/ibmc-s3fs  1s
   ibmc-s3fs-standard-perf-regional      ibm.io/ibmc-s3fs  1s
   ibmc-s3fs-standard-regional           ibm.io/ibmc-s3fs  1s
   ibmc-s3fs-vault-cross-region          ibm.io/ibmc-s3fs  1s
   ibmc-s3fs-vault-regional              ibm.io/ibmc-s3fs  1s

   ==> v1/ServiceAccount
   NAME                            SECRETS  AGE
   ibmcloud-object-storage-driver  1        1s
   ibmcloud-object-storage-plugin  1        1s

   ==> v1beta1/ClusterRole
   NAME                                   AGE
   ibmcloud-object-storage-secret-reader  1s
   ibmcloud-object-storage-plugin         1s

   ==> v1beta1/ClusterRoleBinding
   NAME                                   AGE
   ibmcloud-object-storage-plugin         1s
   ibmcloud-object-storage-secret-reader  1s

   ==> v1beta1/DaemonSet
   NAME                            DESIRED  CURRENT  READY  UP-TO-DATE  AVAILABLE  NODE SELECTOR  AGE
   ibmcloud-object-storage-driver  3        3        0      3           0          <none>         1s

   ==> v1beta1/Deployment
   NAME                            DESIRED  CURRENT  UP-TO-DATE  AVAILABLE  AGE
   ibmcloud-object-storage-plugin  1        1        1           0          1s

   NOTES:
   Thank you for installing: ibmcloud-object-storage-plugin.   Your release is named: mewing-duck

   Please refer Chart README.md file for creating a sample PVC.
   Please refer Chart RELEASE.md to see the release details/fixes.
   ```
   {: screen}

10. Stellen Sie sicher, dass das Plug-in ordnungsgemäß installiert wurde.
   ```
   kubectl get pod -n kube-system -o wide | grep object
   ```
   {: pre}

   Beispielausgabe:
   ```
   ibmcloud-object-storage-driver-9n8g8                              1/1       Running   0          2m
   ibmcloud-object-storage-plugin-7c774d484b-pcnnx                   1/1       Running   0          2m
   ```
   {: screen}

   Die Installation ist erfolgreich, wenn ein Pod des Typs `ibmcloud-object-storage-plugin` und ein oder mehrere Pods des Typs `ibmcloud-object-storage-driver` angezeigt werden. Die Anzahl der Pods des Typs `ibmcloud-object-storage-driver` entspricht der Anzahl der Workerknoten in Ihrem Cluster. Alle Pods müssen den Status `Aktiv` aufweisen, damit das Plug-in ordnungsgemäß ausgeführt werden kann. Wenn die Pods fehlschlagen, führen Sie `kubectl describe pod -n kube-system <pod_name>` aus, um die eigentliche Ursache des Fehlers zu finden.

11. Stellen Sie sicher, dass die Speicherklassen erfolgreich erstellt werden.
    ```
    kubectl get storageclass | grep s3
    ```
    {: pre}

    Beispielausgabe:
    ```
    ibmc-s3fs-cold-cross-region            ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-cold-regional                ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-flex-cross-region            ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-flex-perf-cross-region       ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-flex-perf-regional           ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-flex-regional                ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-standard-cross-region        ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-standard-perf-cross-region   ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-standard-perf-regional       ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-standard-regional            ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-vault-cross-region           ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-vault-regional               ibm.io/ibmc-s3fs   8m
    ```
    {: screen}

12. Wiederholen Sie die Schritte für alle Cluster, bei denen Sie auf {{site.data.keyword.cos_full_notm}}-Buckets zugreifen möchten.

### IBM Cloud Object Storage-Plug-in aktualisieren
{: #update_cos_plugin}

Sie können ein Upgrade des vorhandenen {{site.data.keyword.cos_full_notm}}-Plug-ins auf die aktuelle Version durchführen.
{: shortdesc}

1. Aktualisieren Sie das {{site.data.keyword.Bluemix_notm}}-Helm-Repository, um die aktuelle Version aller Helm-Diagramme in diesem Repository abzurufen.
   ```
   helm repo update
   ```
   {: pre}

2. Wenn Sie Mac OS oder eine Linux-Distribution verwenden, aktualisieren Sie das Helm-Plug-in `ibmc` für {{site.data.keyword.cos_full_notm}} auf die neueste Version.
   ```
   helm ibmc --update
   ```
   {: pre}

3. Laden Sie das neueste {{site.data.keyword.cos_full_notm}}-Helm-Diagramm auf Ihre lokale Maschine herunter und extrahieren Sie das Paket, um die Datei `release.md` zu öffnen, in der die neuesten Releaseinformationen enthalten sind.
   ```
   helm fetch --untar iks-charts/ibmcloud-object-storage-plugin
   ```

4. Ermitteln Sie den Installationsnamen Ihres Helm-Diagramms.
   ```
   helm ls | grep ibmcloud-object-storage-plugin
   ```
   {: pre}

   Beispielausgabe:
   ```
   <name_des_helm-diagramms> 	1       	Mon Sep 18 15:31:40 2017	DEPLOYED	ibmcloud-object-storage-plugin-1.0.0	default
   ```
   {: screen}

5. Führen Sie ein Upgrade für das {{site.data.keyword.cos_full_notm}}-Helm-Diagramm auf die neueste Version durch.
   ```   
   helm ibmc upgrade <name_des_helm-diagramms> iks-charts/ibmcloud-object-storage-plugin --force --recreate-pods -f ./ibmcloud-object-storage-plugin/ibm/values.yaml
   ```
   {: pre}

6. Überprüfen Sie, ob das Upgrade für `ibmcloud-object-storage-plugin` erfolgreich durchgeführt wurde.  
   ```
   kubectl rollout status deployment/ibmcloud-object-storage-plugin -n kube-system
   ```
   {: pre}

   Das Upgrade des Plug-ins wurde erfolgreich ausgeführt, wenn die Nachricht `deployment "ibmcloud-object-storage-plugin" successfully rolled out` in der CLI-Ausgabe angezeigt wird.

7. Stellen Sie sicher, dass das Upgrade für `ibmcloud-object-storage-driver` erfolgreich durchgeführt wird.
   ```
   kubectl rollout status ds/ibmcloud-object-storage-driver -n kube-system
   ```
   {: pre}

   Das Upgrade wurde erfolgreich ausgeführt, wenn die Nachricht `daemon set "ibmcloud-object-storage-driver" successfully rolled out` in der CLI-Ausgabe angezeigt wird.

8. Stellen Sie sicher, dass sich die {{site.data.keyword.cos_full_notm}}-Pods im Status `Aktiv` befinden.
   ```
   kubectl get pods -n kube-system -o wide | grep object-storage
   ```
   {: pre}


### IBM Cloud Object Storage-Plug-in entfernen
{: #remove_cos_plugin}

Wenn Sie in Ihrem Cluster {{site.data.keyword.cos_full_notm}} nicht einrichten und verwenden möchten, können Sie die Helm-Diagramme deinstallieren.
{: shortdesc}

Durch das Entfernen des Plug-ins werden keine vorhandenen PVCs, PVs oder Daten entfernt. Wenn Sie das Plug-in entfernen, werden alle zugehörigen Pods und Dämongruppen aus Ihrem Cluster entfernt. Nachdem Sie das Plug-in entfernt haben, können Sie keinen neuen {{site.data.keyword.cos_full_notm}} für Ihren Cluster einrichten oder vorhandene PVCs und PVs verwenden, es sei denn, Sie konfigurieren Ihre App so, dass sie die {{site.data.keyword.cos_full_notm}}-API direkt verwendet.
{: important}

Vorbereitende Schritte:

- [Geben Sie als Ziel Ihrer CLI den Cluster an](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).
- Stellen Sie sicher, dass in Ihrem Cluster keine PVCs oder persistenten Datenträger vorhanden sind, die {{site.data.keyword.cos_full_notm}} verwenden. Um alle Pods aufzulisten, die einen bestimmten PVC anhängen, führen Sie folgenden Befehl aus: `kubectl get pods --all-namespaces -o=jsonpath='{range .items[*]}{"\n"}{.metadata.name}{":\t"}{range .spec.volumes[*]}{.persistentVolumeClaim.claimName}{" "}{end}{end}' | grep "<pvc_name>"`.

Gehen Sie wie folgt vor, um das Plug-in zu entfernen:

1. Suchen Sie den Installationsnamen Ihres Helm-Diagramms.
   ```
   helm ls | grep ibmcloud-object-storage-plugin
   ```
   {: pre}

   Beispielausgabe:
   ```
   <name_des_helm-diagramms> 	1       	Mon Sep 18 15:31:40 2017	DEPLOYED	ibmcloud-object-storage-plugin-1.0.0	default
   ```
   {: screen}

2. Löschen Sie das {{site.data.keyword.cos_full_notm}}-Plug-in, indem Sie das Helm-Diagramm entfernen.
   ```
   helm delete --purge <name_des_helm-diagramms>
   ```
   {: pre}

3. Stellen Sie sicher, dass die {{site.data.keyword.cos_full_notm}}-Pods entfernt werden.
   ```
   kubectl get pod -n kube-system | grep object-storage
   ```
   {: pre}

   Das Entfernen der Pods war erfolgreich, wenn in Ihrer CLI-Ausgabe keine Pods angezeigt werden.

4. Stellen Sie sicher, dass die Speicherklassen entfernt werden.
   ```
   kubectl get storageclasses | grep s3
   ```
   {: pre}

   Das Entfernen der Speicherklassen war erfolgreich, wenn in Ihrer CLI-Ausgabe keine Speicherklassen angezeigt werden.

5. Wenn Sie Mac OS oder eine Linux-Distribution verwenden, entfernen Sie das Helm-Plug-in `ibmc`. Wenn Sie Windows verwenden, ist dieser Schritt nicht erforderlich.
   1. Entfernen Sie das Plug-in `ibmc`.
      ```
      rm -rf ~/.helm/plugins/helm-ibmc
      ```
      {: pre}

   2. Stellen Sie sicher, dass das Plug-in `ibmc` entfernt wurde.
      ```
      helm plugin list
      ```
      {: pre}

      Beispielausgabe:
     ```
     NAME	VERSION	DESCRIPTION
     ```
     {: screen}

     Das Entfernen des Plug-ins `ibmc` war erfolgreich, wenn das Plug-in `ibmc` nicht in der CLI-Ausgabe aufgeführt ist.


## Entscheidung über Objektspeicherkonfiguration treffen
{: #configure_cos}

{{site.data.keyword.containerlong_notm}} stellt vordefinierte Speicherklassen zur Verfügung, die Sie verwenden können, um Buckets mit einer bestimmten Konfiguration zu erstellen.
{: shortdesc}

1. Listen Sie die in {{site.data.keyword.containerlong_notm}} verfügbaren Speicherklassen auf.
   ```
   kubectl get storageclasses | grep s3
   ```
   {: pre}

   Beispielausgabe:
   ```
   ibmc-s3fs-cold-cross-region            ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-cold-regional                ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-flex-cross-region            ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-flex-perf-cross-region       ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-flex-perf-regional           ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-flex-regional                ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-standard-cross-region        ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-standard-perf-cross-region   ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-standard-perf-regional       ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-standard-regional            ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-vault-cross-region           ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-vault-regional               ibm.io/ibmc-s3fs   8m
   ```
   {: screen}

2. Wählen Sie eine Speicherklasse aus, die Ihren Datenzugriffsanforderungen entspricht. Die Speicherklasse bestimmt die [Preise ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link") ](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api) für die Speicherkapazität, die Lese- und Schreiboperationen und die Ausgangsbandbreite für ein Bucket. Welche Option für Ihr Szenario am besten geeignet ist, hängt davon ab, wie häufig Daten gelesen und in Ihre Serviceinstanz geschrieben werden.
   - **Standard**: Diese Option wird für Daten verwendet, auf die häufig zugegriffen wird ('Hot Data'). Gängige Anwendungsfälle sind Web-Apps oder Mobile Apps.
   - **Vault**: Diese Option wird für Workloads oder Daten verwendet, auf die selten zugegriffen wird - z. B. einmal im Monat oder weniger ('Cool Data'). Gängige Anwendungsfälle sind Archive, kurzfristige Datenaufbewahrung, Aufbewahrung digitaler Assets, Bandwechsel oder Disaster-Recovery.
   - **Cold**: Diese Option wird für Daten verwendet, auf die selten (alle 90 Tage oder weniger) zugegriffen wird, oder auf inaktive Daten ('Cold Data'). Gängige Anwendungsfälle sind Archive, langfristige Sicherung, Langzeitdaten, die Sie für Compliance-Zwecke aufbewahren, oder Workloads und Apps, auf die selten zugegriffen wird.
   - **Flex**: Diese Option wird für Workloads und Daten verwendet, die keinem bestimmten Verwendungsmuster folgen, oder die zu groß sind, um ein Verwendungsmuster zu bestimmen oder vorherzusagen. **Tipp:** Sehen Sie sich diesen [Blog ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/blogs/bluemix/2017/03/interconnect-2017-changing-rules-storage/) an, um mehr darüber zu erfahren, wie die Flex-Speicherklasse im Vergleich zu konventionellen Speicherschichten (Storage Tiers) funktioniert.   

3. Entscheiden Sie über die Ausfallsicherheit für die Daten, die in Ihrem Bucket gespeichert sind.
   - **Cross-region**: Bei dieser Option werden Ihre Daten für höchste Verfügbarkeit über drei Regionen in einem geografischen Gebiet gespeichert. Wenn Sie über Workloads verfügen, die über Regionen verteilt sind, werden Anforderungen an den nächsten regionalen Endpunkt weitergeleitet. Der API-Endpunkt für das geografische Gebiet wird automatisch durch das Helm-Plug-in `ibmc` festgelegt, das Sie zuvor basierend auf dem Standort installiert haben, an dem sich Ihr Cluster befindet. Wenn sich Ihr Cluster beispielsweise in der Region `Vereinigte Staaten (Süden)` befindet, werden Ihre Speicherklassen zur Verwendung des API-Endpunkt `US GEO` für Ihre Buckets konfiguriert. Weitere Informationen finden Sie unter [Regionen und Endpunkte](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-endpoints#endpoints).  
   - **Regional**: Mit dieser Option werden Ihre Daten über mehrere Zonen innerhalb einer Region repliziert. Wenn Sie Workloads haben, die sich in derselben Region befinden, stellen Sie eine geringere Latenz und eine bessere Leistung als bei einer regionsübergreifenden Konfiguration fest. Der regionale Endpunkt wird automatisch durch das Helm-Plug-in `ibm` festgelegt, das Sie zuvor basierend auf dem Standort installiert haben, an dem sich Ihr Cluster befindet. Wenn sich Ihr Cluster beispielsweise in der Region `Vereinigte Staaten (Süden)` befindet, wurden Ihre Speicherklassen zur Verwendung des API-Endpunkt `US South` für Ihre Buckets konfiguriert. Weitere Informationen finden Sie unter [Regionen und Endpunkte](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-endpoints#endpoints).

4. Überprüfen Sie für die Speicherklasse die detaillierte {{site.data.keyword.cos_full_notm}}-Bucketkonfiguration.
   ```
   kubectl describe storageclass <name_der_speicherklasse>
   ```
   {: pre}

   Beispielausgabe:
   ```
   Name:                  ibmc-s3fs-standard-cross-region
   IsDefaultClass:        No
   Annotations:           <none>
   Provisioner:           ibm.io/ibmc-s3fs
   Parameters:            ibm.io/chunk-size-mb=16,ibm.io/curl-debug=false,ibm.io/debug-level=warn,ibm.io/iam-endpoint=https://iam.bluemix.net,ibm.io/kernel-cache=true,ibm.io/multireq-max=20,ibm.io/object-store-endpoint=https://s3-api.dal-us-geo.objectstorage.service.networklayer.com,ibm.io/object-store-storage-class=us-standard,ibm.io/parallel-count=2,ibm.io/s3fs-fuse-retry-count=5,ibm.io/stat-cache-size=100000,ibm.io/tls-cipher-suite=AESGCM
   AllowVolumeExpansion:  <unset>
   MountOptions:          <none>
   ReclaimPolicy:         Delete
   VolumeBindingMode:     Immediate
   Events:                <none>
   ```
   {: screen}

   <table>
   <caption>Erklärung der Speicherklassendetails</caption>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der YAML-Dateikomponenten</th>
   </thead>
   <tbody>
   <tr>
   <td><code>ibm.io/chunk-size-mb</code></td>
   <td>Die Größe eines Datenblocks (in Megabyte), der in {{site.data.keyword.cos_full_notm}} gelesen oder geschrieben wird. Speicherklassen mit <code>perf</code> im Namen werden mit 52 Megabyte eingerichtet. Speicherklassen ohne <code>perf</code> im Namen verwenden Blöcke von 16 Megabyte. Wenn Sie zum Beispiel eine Datei mit einer Größe von 1 GB lesen möchten, liest das Plug-in diese Datei in mehreren Blöcken von 16 oder 52 Megabyte. </td>
   </tr>
   <tr>
   <td><code>ibm.io/curl-debug</code></td>
   <td>Aktivierung der Protokollierung von Anforderungen, die an die {{site.data.keyword.cos_full_notm}}-Serviceinstanz gesendet werden. Bei Aktivierung werden Protokolle an `syslog` gesendet und Sie können [die Protokolle an einen externen Protokollierungsserver weiterleiten](/docs/containers?topic=containers-health#logging). Standardmäßig sind alle Speicherklassen auf <strong>false</strong> eingestellt, um diese Protokollierungsfunktion zu inaktivieren. </td>
   </tr>
   <tr>
   <td><code>ibm.io/debug-level</code></td>
   <td>Die Protokollierungsstufe, die vom {{site.data.keyword.cos_full_notm}}-Plug-in festgelegt wird. Alle Speicherklassen werden mit der Protokollierungsstufe <strong>WARN</strong> eingerichtet. </td>
   </tr>
   <tr>
   <td><code>ibm.io/iam-endpoint</code></td>
   <td>Der API-Endpunkt für {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM). </td>
   </tr>
   <tr>
   <td><code>ibm.io/kernel-cache</code></td>
   <td>Aktivierung oder Inaktivierung des Kernelpuffercache für den Datenträgermountpunkt. Wenn diese Funktion aktiviert ist, werden Daten, die aus {{site.data.keyword.cos_full_notm}} gelesen werden, im Kernel-Cache gespeichert, um einen schnellen Lesezugriff auf Ihre Daten zu ermöglichen. Wenn diese Funktion inaktiviert ist, werden Daten nicht in den Cache gestellt und immer aus {{site.data.keyword.cos_full_notm}} gelesen. Der Kernel-Cache wird bei den Speicherklassen <code>Standard</code> und <code>Flex</code> aktiviert und bei den Speicherklassen <code>Cold</code> und <code>Vault</code> inaktiviert. </td>
   </tr>
   <tr>
   <td><code>ibm.io/multireq-max</code></td>
   <td>Die maximale Anzahl paralleler Anforderungen, die an die {{site.data.keyword.cos_full_notm}}-Serviceinstanz gesendet werden können, um Dateien in einem einzelnen Verzeichnis aufzulisten. Alle Speicherklassen werden mit maximal 20 parallelen Anforderungen konfiguriert.  </td>
   </tr>
   <tr>
   <td><code>ibm.io/object-store-endpoint</code></td>
   <td>Der API-Endpunkt, der für den Zugriff auf das Bucket in Ihrer {{site.data.keyword.cos_full_notm}}-Serviceinstanz verwendet werden soll. Der Endpunkt wird automatisch auf der Basis der Region des Clusters festgelegt. **Hinweis**: Wenn Sie auf ein vorhandenes Bucket zugreifen möchten, das sich in einer anderen als der Region befindet, in der Ihr Cluster enthalten ist, müssen Sie eine [angepasste Speicherklasse](/docs/containers?topic=containers-kube_concepts#customized_storageclass) erstellen und den API-Endpunkt für Ihr Bucket verwenden.</td>
   </tr>
   <tr>
   <td><code>ibm.io/object-store-storage-class</code></td>
   <td>Der Name der Speicherklasse. </td>
   </tr>
   <tr>
   <td><code>ibm.io/parallel-count</code></td>
   <td>Die maximale Anzahl paralleler Anforderungen, die an die {{site.data.keyword.cos_full_notm}}-Serviceinstanz für eine einzelne Lese- oder Schreiboperation gesendet werden können. Speicherklassen mit <code>perf</code> im Namen werden mit maximal 20 parallelen Anforderungen eingerichtet. Speicherklassen ohne <code>perf</code> im Namen werden standardmäßig mit zwei parallelen Anforderungen eingerichtet.  </td>
   </tr>
   <tr>
   <td><code>ibm.io/s3fs-fuse-retry-count</code></td>
   <td>Die maximale Anzahl der Versuche für eine Lese- oder Schreiboperation, bevor die Operation als nicht erfolgreich angesehen wird. Alle Speicherklassen werden mit maximal fünf Versuchen eingerichtet.  </td>
   </tr>
   <tr>
   <td><code>ibm.io/stat-cache-size</code></td>
   <td>Die maximale Anzahl der Datensätze, die im {{site.data.keyword.cos_full_notm}}-Metadatencache gespeichert werden. Jeder Datensatz kann bis zu 0,5 Kilobyte belegen. Alle Speicherklassen legen die maximale Anzahl an Datensätzen standardmäßig auf 100000 fest. </td>
   </tr>
   <tr>
   <td><code>ibm.io/tls-cipher-suite</code></td>
   <td>Die TLS-Cipher-Suite, die verwendet werden muss, wenn über den HTTPS-Endpunkt eine Verbindung zu {{site.data.keyword.cos_full_notm}} hergestellt wird. Der Wert für die Cipher-Suite muss dem [OpenSSL-Format ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link") ](https://www.openssl.org/docs/man1.0.2/apps/ciphers.html) entsprechen. Alle Speicherklassen verwenden standardmäßig die Cipher-Suite <strong><code>AESGCM</code></strong>.  </td>
   </tr>
   </tbody>
   </table>

   Weitere Informationen zu den einzelnen Speicherklassen finden Sie in der [Speicherklassenreferenz](#cos_storageclass_reference). Wenn Sie einen der voreingestellten Werte ändern wollen, müssen Sie Ihre eigene [angepasste Speicherklasse](/docs/containers?topic=containers-kube_concepts#customized_storageclass) erstellen.
   {: tip}

5. Entscheiden Sie sich für einen Namen für Ihr Bucket. Der Name eines Buckets muss in {{site.data.keyword.cos_full_notm}} eindeutig sein. Sie können auch angeben, dass ein Name für Ihr Bucket automatisch mithilfe des {{site.data.keyword.cos_full_notm}}-Plug-ins erstellt werden soll. Um Daten in einem Bucket zu organisieren, können Sie Unterverzeichnisse erstellen.

   Die Speicherklasse, die Sie zuvor ausgewählt haben, bestimmt die Preisgestaltung für das gesamte Bucket. Sie können für Unterverzeichnisse keine unterschiedlichen Speicherklassen definieren. Wenn Sie Daten mit unterschiedlichen Zugriffsanforderungen speichern möchten, sollten Sie das Erstellen mehrerer Buckets mithilfe mehrerer PVCs in Betracht ziehen.
   {: note}

6. Wählen Sie diese Option aus, wenn die Daten und das Bucket nach dem Löschen des Clusters oder des Persistent Volume Claim (PVC) beibehalten werden sollen. Wenn Sie den PVC löschen, wird der PV immer gelöscht. Sie können angeben, ob die Daten und das Bucket automatisch gelöscht werden sollen, wenn Sie den PVC löschen. Ihre {{site.data.keyword.cos_full_notm}}-Serviceinstanz ist unabhängig von der Aufbewahrungsrichtlinie, die Sie für Ihre Daten ausgewählt haben, und wird nie entfernt, wenn Sie einen PVC löschen.

Nachdem Sie sich nun für die gewünschte Konfiguration entschieden haben, können Sie [einen PVC erstellen](#add_cos), um {{site.data.keyword.cos_full_notm}} einzurichten.

## Objektspeicher zu Apps hinzufügen
{: #add_cos}

Sie können einen Persistent Volume Claim (PVC) erstellen, um {{site.data.keyword.cos_full_notm}} für Ihren Cluster einzurichten.
{: shortdesc}

Abhängig von den Einstellungen, die Sie im PVC auswählen, können Sie {{site.data.keyword.cos_full_notm}} auf folgende Weise einrichten:
- [Dynamische Einrichtung](/docs/containers?topic=containers-kube_concepts#dynamic_provisioning): Wenn Sie den PVC erstellen, wird der entsprechende persistente Datenträger (Persistent Volume, PV) und das Bucket in Ihrer {{site.data.keyword.cos_full_notm}}-Serviceinstanz automatisch erstellt.
- [Statische Einrichtung](/docs/containers?topic=containers-kube_concepts#static_provisioning): Sie können auf ein vorhandenes Bucket in Ihrer {{site.data.keyword.cos_full_notm}}-Serviceinstanz in Ihrem PVC verweisen. Wenn Sie den PVC erstellen, wird nur der übereinstimmende PV automatisch erstellt und mit vorhandenen Bucket in {{site.data.keyword.cos_full_notm}} verknüpft.

Vorbereitende Schritte:
- [Erstellen Sie Ihre {{site.data.keyword.cos_full_notm}}-Serviceinstanz und bereiten Sie sie vor](#create_cos_service).
- [Erstellen Sie einen geheimen Schlüssel, um Ihre {{site.data.keyword.cos_full_notm}}-Serviceberechtigungsnachweise](#create_cos_secret) zu speichern.
- [Entscheiden Sie über die Konfiguration Ihres {{site.data.keyword.cos_full_notm}}](#configure_cos).

Gehen Sie wie folgt vor, um {{site.data.keyword.cos_full_notm}} zu Ihrem Cluster hinzuzufügen:

1. Erstellen Sie eine Konfigurationsdatei, um Ihren Persistent Volume Claim (PVC) zu definieren.
   ```
   kind: PersistentVolumeClaim
   apiVersion: v1
   metadata:
     name: <pvc_name>
     namespace: <namensbereich>
     annotations:
       ibm.io/auto-create-bucket: "<true_oder_false>"
       ibm.io/auto-delete-bucket: "<true_oder_false>"
       ibm.io/bucket: "<bucket-name>"
       ibm.io/object-path: "<bucket-unterverzeichnis>"
       ibm.io/secret-name: "<name_des_geheimen_schlüssels>"
   spec:
     accessModes:
       - ReadWriteOnce
     resources:
       requests:
         storage: 8Gi # Enter a fictitious value
     storageClassName: <speicherklasse>
   ```
   {: codeblock}

   <table>
   <caption>Erklärung der Komponenten der YAML-Datei</caption>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der YAML-Dateikomponenten</th>
   </thead>
   <tbody>
   <tr>
   <td><code>metadata.name</code></td>
   <td>Geben Sie den Namen des PVC ein.</td>
   </tr>
   <tr>
   <td><code>metadata.namespace</code></td>
   <td>Geben Sie den Namensbereich ein, in dem der PVC erstellt werden soll. Der PVC muss in demselben Namensbereich erstellt werden, in dem Sie den geheimen Kubernetes-Schlüssel für Ihre {{site.data.keyword.cos_full_notm}}-Serviceberechtigungsnachweise erstellt haben und wo Sie Ihren Pod ausführen möchten. </td>
   </tr>
   <tr>
   <td><code>ibm.io/auto-create-bucket</code></td>
   <td>Wählen Sie eine der beiden folgenden Optionen aus: <ul><li><strong>true</strong>: Wenn Sie den PVC erstellen, werden der PV und das Bucket in Ihrer {{site.data.keyword.cos_full_notm}}-Serviceinstanz automatisch erstellt. Wählen Sie diese Option aus, um ein neues Bucket in Ihrer {{site.data.keyword.cos_full_notm}}-Serviceinstanz zu erstellen. </li><li><strong>false</strong>: Wählen Sie diese Option aus, wenn Sie auf Daten in einem vorhandenen Bucket zugreifen möchten. Wenn Sie den PVC erstellen, wird der PV automatisch erstellt und mit dem Bucket verknüpft, den Sie in <code>ibm.io/bucket</code> angeben.</td>
   </tr>
   <tr>
   <td><code>ibm.io/auto-delete-bucket</code></td>
   <td>Wählen Sie eine der beiden folgenden Optionen aus: <ul><li><strong>true</strong>: Ihre Daten, das Bucket und der PV werden automatisch entfernt, wenn Sie den PVC löschen. Ihre {{site.data.keyword.cos_full_notm}}-Serviceinstanz bleibt bestehen und wird nicht gelöscht. Wenn Sie diese Option auf <strong>true</strong> setzen, müssen Sie <code>ibm.io/auto-create-bucket: true</code> und <code>ibm.io/bucket: ""</code> festlegen, sodass Ihr Bucket automatisch mit einem Namen im Format <code>tmp-s3fs-xxxx</code> erstellt wird. </li><li><strong>false</strong>: Wenn Sie den PVC löschen, wird der PV automatisch gelöscht, aber Ihre Daten und das Bucket in Ihrer {{site.data.keyword.cos_full_notm}}-Serviceinstanz bleiben erhalten. Um auf Ihre Daten zugreifen zu können, müssen Sie einen neuen PVC mit dem Namen des vorhandenen Buckets erstellen. </li></ul>
   <tr>
   <td><code>ibm.io/bucket</code></td>
   <td>Wählen Sie eine der beiden folgenden Optionen aus: <ul><li>Wenn <code>ibm.io/auto-create-bucket</code> auf <strong>true</strong> gesetzt ist: Geben Sie den Namen des Buckets ein, das Sie in {{site.data.keyword.cos_full_notm}} erstellen möchten. Wenn zusätzlich <code>ibm.io/auto-delete-bucket</code> auf <strong>true</strong> gesetzt wird, müssen Sie dieses Feld leer lassen, um dem Bucket automatisch einen Namen im Format <code>tmp-s3fs-xxxx</code> zuzuweisen. Der Name muss in {{site.data.keyword.cos_full_notm}} eindeutig sein. </li><li>Wenn <code>ibm.io/auto-create-bucket</code> auf <strong>false</strong> gesetzt wird: Geben Sie den Namen des vorhandenen Buckets ein, auf das Sie im Cluster zugreifen möchten. </li></ul> </td>
   </tr>
   <tr>
   <td><code>ibm.io/object-path</code></td>
   <td>Optional: Geben Sie den Namen des vorhandenen Unterverzeichnisses in Ihrem Bucket ein, das angehängt werden soll. Verwenden Sie diese Option, wenn nur ein Unterverzeichnis und nicht das gesamte Bucket angehängt werden soll. Um ein Unterverzeichnis anzuhängen, müssen Sie <code>ibm.io/auto-create-bucket: "false"</code> festlegen und den Namen des Buckets in <code>ibm.io/bucket</code> angeben. </li></ul> </td>
   </tr>
   <tr>
   <td><code>ibm.io/secret-name</code></td>
   <td>Geben Sie den Namen des geheimen Schlüssels ein, der die {{site.data.keyword.cos_full_notm}}-Berechtigungsnachweise enthält, die Sie zuvor erstellt haben. </td>
   </tr>
   <tr>
   <td><code>resources.requests.storage</code></td>
   <td>Eine fiktive Größe für Ihren {{site.data.keyword.cos_full_notm}}-Bucket in Gigabyte. Die Größe ist für Kubernetes erforderlich, wird jedoch in {{site.data.keyword.cos_full_notm}} nicht beachtet. Sie können eine beliebige Größe angeben. Der tatsächliche Bereich, den Sie in {{site.data.keyword.cos_full_notm}} verwenden, kann anders sein und wird auf der Basis der [Preistabelle ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api) in Rechnung gestellt. </td>
   </tr>
   <tr>
   <td><code>spec.storageClassName</code></td>
   <td>Wählen Sie eine der beiden folgenden Optionen aus: <ul><li>Wenn <code>ibm.io/auto-create-bucket</code> auf <strong>true</strong> gesetzt ist, geben Sie die Speicherklasse ein, die Sie für das neue Bucket verwenden möchten. </li><li>Wenn <code>ibm.io/auto-create-bucket</code> auf <strong>false</strong> gesetzt ist, geben Sie die Speicherklasse ein, die Sie zum Erstellen des vorhandenen Buckets verwendet haben. </br></br>Wenn Sie das Bucket manuell in Ihrer {{site.data.keyword.cos_full_notm}}-Serviceinstanz erstellt haben oder wenn Sie sich nicht an die von Ihnen verwendete Speicherklasse erinnern können, suchen Sie die Serviceinstanz im {{site.data.keyword.Bluemix}}-Dashboard und prüfen Sie die <strong>Klasse</strong> und den <strong>Standort</strong> Ihres vorhandenen Bucket. Verwenden Sie anschließend die entsprechende [Speicherklasse](#cos_storageclass_reference). <p class="note">Der {{site.data.keyword.cos_full_notm}}-API-Endpunkt, der in Ihrer Speicherklasse definiert ist, basiert auf der Region, in der sich Ihr Cluster befindet. Wenn Sie auf ein Bucket zugreifen möchten, das sich in einer anderen als der Region befindet, in der Ihr Cluster enthalten ist, müssen Sie eine [angepasste Speicherklasse](/docs/containers?topic=containers-kube_concepts#customized_storageclass) erstellen und den entsprechenden API-Endpunkt für Ihr Bucket verwenden.</p></li></ul>  </td>
   </tr>
   </tbody>
   </table>

2. Erstellen Sie den PVC.
   ```
   kubectl apply -f filepath/pvc.yaml
   ```
   {: pre}

3. Überprüfen Sie, ob Ihr PVC erstellt und an den PV gebunden wurde.
   ```
   kubectl get pvc
   ```
   {: pre}

   Beispielausgabe:
   ```
   NAME                  STATUS    VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS                     AGE
   s3fs-test-pvc         Bound     pvc-b38b30f9-1234-11e8-ad2b-t910456jbe12   8Gi        RWO            ibmc-s3fs-standard-cross-region  1h
   ```
   {: screen}

4. Optional: Wenn Sie als Benutzer ohne Rootberechtigung auf Ihre Daten zugreifen möchten oder Dateien zu einem vorhandenen {{site.data.keyword.cos_full_notm}}-Bucket direkt über die Konsole oder die API hinzugefügt haben, stellen Sie sicher, dass den [Dateien die richtige Berechtigung zugewiesen wird](/docs/containers?topic=containers-cs_troubleshoot_storage#cos_nonroot_access), sodass Ihre App die Dateien nach Bedarf erfolgreich lesen und aktualisieren kann.

4.  {: #cos_app_volume_mount}Erstellen Sie zum Anhängen des persistenten Datenträgers an Ihre Bereitstellung eine `.yaml`-Konfigurationsdatei und geben Sie den Persistent Volume Claim (PVC) an, der den PV (Persistent Volume) bindet.

    ```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: <bereitstellungsname>
      labels:
        app: <bereitstellungsbezeichnung>
    spec:
      selector:
        matchLabels:
          app: <app-name>
      template:
        metadata:
          labels:
            app: <app-name>
        spec:
          containers:
          - image: <imagename>
            name: <containername>
            securityContext:
              runAsUser: <nicht-rootbenutzer>
              fsGroup: <nicht-rootbenutzer> #gilt nur für Cluster mit Kubernetes Version 1.13 oder höher
            volumeMounts:
            - name: <datenträgername>
              mountPath: /<dateipfad>
          volumes:
          - name: <datenträgername>
            persistentVolumeClaim:
              claimName: <pvc-name>
    ```
    {: codeblock}

    <table>
    <caption>Erklärung der Komponenten der YAML-Datei</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der YAML-Dateikomponenten</th>
    </thead>
    <tbody>
        <tr>
    <td><code>metadata.labels.app</code></td>
    <td>Eine Bezeichnung für die Bereitstellung.</td>
      </tr>
      <tr>
        <td><code>spec.selector.matchLabels.app</code> <br/> <code>spec.template.metadata.labels.app</code></td>
        <td>Eine Bezeichnung für Ihre App.</td>
      </tr>
    <tr>
    <td><code>template.metadata.labels.app</code></td>
    <td>Eine Bezeichnung für die Bereitstellung.</td>
      </tr>
    <tr>
    <td><code>spec.containers.image</code></td>
    <td>Der Name des Images, das Sie verwenden möchten. Um die in Ihrem {{site.data.keyword.registryshort_notm}}-Konto verfügbaren Images aufzulisten, führen Sie den Befehl `ibmcloud cr image-list` aus.</td>
    </tr>
    <tr>
    <td><code>spec.containers.name</code></td>
    <td>Der Name des Containers, den Sie in Ihrem Cluster bereitstellen möchten.</td>
    </tr>
    <tr>
    <td><code>spec.containers.securityContext.runAsUser</code></td>
    <td>Optional: Wenn Sie die App als Benutzer ohne Rootberechtigung in einem Cluster mit Kubernetes Version 1.12 oder darunter ausführen möchten, geben Sie den [Sicherheitskontext ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/) für Ihren Pod an, indem Sie den Benutzer ohne Rootberechtigung definieren, ohne `fsGroup` gleichzeitig in der YAML-Datei für die Bereitstellung zu definieren. Wenn Sie `fsGroup` festlegen, wird das {{site.data.keyword.cos_full_notm}}-Plug-in ausgelöst, das die Gruppenberechtigungen für alle Dateien in einem Bucket aktualisiert, wenn der Pod bereitgestellt wird. Das Aktualisieren der Berechtigungen ist eine Schreiboperation und wirkt sich auf die Leistung aus. Abhängig von der Anzahl der Dateien, die Sie haben, kann das Aktualisieren der Berechtigungen möglicherweise verhindern, dass Ihr Pod gestartet wird und in den Status <code>Aktiv</code> wechselt. </br></br>Wenn Sie einen Cluster haben, der Kubernetes Version 1.13 oder höher ausführt, und die Plug-in-Version 1.0.4 oder höher von {{site.data.keyword.Bluemix_notm}} Object Storage verwenden, können Sie den Eigner des Mountpunkts s3fs ändern. Zum Ändern des Eigners geben Sie den Sicherheitskontext an, indem Sie `runAsUser` und `fsGroup` auf die Benutzer-ID ohne Rootberechtigung setzen, die Eigner des s3fs-Mountpunkts sein soll. Stimmen diese beiden Werte nicht überein, ist der Mountpunkt automatisch der Eigner des Benutzers `root`. </td>
    </tr>
    <tr>
    <td><code>spec.containers.volumeMounts.mountPath</code></td>
    <td>Der absolute Pfad des Verzeichnisses, in dem der Datenträger innerhalb des Containers angehängt wird. Wenn Sie einen Datenträger zwischen verschiedenen Apps gemeinsam nutzen möchten, können Sie [Unterpfade für Datenträger  ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/storage/volumes/#using-subpath) für jede Ihrer Apps angeben.</td>
    </tr>
    <tr>
    <td><code>spec.containers.volumeMounts.name</code></td>
    <td>Der Name des Datenträgers, der an Ihren Pod angehängt werden soll.</td>
    </tr>
    <tr>
    <td><code>volumes.name</code></td>
    <td>Der Name des Datenträgers, der an Ihren Pod angehängt werden soll. Normalerweise ist dieser Name deckungsgleich mit <code>volumeMounts/name</code>.</td>
    </tr>
    <tr>
    <td><code>volumes.persistentVolumeClaim.claimName</code></td>
    <td>Der Name des PVC, der den zu verwendenden persistenten Datenträger bindet. </td>
    </tr>
    </tbody></table>

5.  Erstellen Sie die Bereitstellung.
     ```
     kubectl apply -f <lokaler_yaml-pfad>
     ```
     {: pre}

6.  Überprüfen Sie, dass der persistente Datenträger erfolgreich angehängt wurde.

     ```
     kubectl describe deployment <bereitstellungsname>
     ```
     {: pre}

     Der Mountpunkt wird im Feld **Volume Mounts** und der Datenträger wird im Feld **Volumes** angegeben.

     ```
      Volume Mounts:
          /var/run/secrets/kubernetes.io/serviceaccount from default-token-tqp61 (ro)
          /volumemount from myvol (rw)
     ...
     Volumes:
      myvol:
        Type: PersistentVolumeClaim (Referenz auf einen PersistentVolumeClaim im gleichen Namensbereich)
        ClaimName: mypvc
        ReadOnly: false
     ```
     {: screen}

7. Stellen Sie sicher, dass Sie Daten in Ihre {{site.data.keyword.cos_full_notm}}-Serviceinstanz schreiben können.
   1. Melden Sie sich bei dem Pod an, der Ihren PV anhängt.
      ```
      kubectl exec <podname> -it bash
      ```
      {: pre}

   2. Navigieren Sie zu Ihrem Datenträgermountpfad, den Sie in Ihrer App-Bereitstellung definiert haben.
   3. Erstellen Sie eine Textdatei.
      ```
      echo "Dies ist ein Test" > test.txt
      ```
      {: pre}

   4. Navigieren Sie vom {{site.data.keyword.Bluemix}}-Dashboard zu Ihrer {{site.data.keyword.cos_full_notm}}-Serviceinstanz.
   5. Wählen Sie **Buckets** im Menü aus.
   6. Öffnen Sie Ihr Bucket und stellen Sie sicher, dass die von Ihnen erstellte Datei `test.txt` angezeigt wird.


## Objektspeicher in statusabhängiger Gruppe verwenden
{: #cos_statefulset}

Wenn Sie über eine statusabhängige App wie zum Beispiel eine Datenbank verfügen, können Sie statusabhängige Gruppen erstellen, von denen {{site.data.keyword.cos_full_notm}} zum Speichern der App-Daten verwendet wird. Alternativ können Sie eine {{site.data.keyword.Bluemix_notm}}-Database as a Service verwenden, zum Beispiel {{site.data.keyword.cloudant_short_notm}}, und die Daten in der Cloud speichern.
{: shortdesc}

Vorbereitende Schritte:
- [Erstellen Sie Ihre {{site.data.keyword.cos_full_notm}}-Serviceinstanz und bereiten Sie sie vor](#create_cos_service).
- [Erstellen Sie einen geheimen Schlüssel, um Ihre {{site.data.keyword.cos_full_notm}}-Serviceberechtigungsnachweise](#create_cos_secret) zu speichern.
- [Entscheiden Sie über die Konfiguration Ihres {{site.data.keyword.cos_full_notm}}](#configure_cos).

Gehen Sie wie folgt vor, um eine statusabhängige Gruppe bereitzustellen, von der Objektspeicher verwendet wird

1. Erstellen Sie eine Konfigurationsdatei für die statusabhängige Gruppe und den Service, den Sie verwenden, um die statusabhängige Gruppe zugänglich zu machen. In den folgenden Beispielen wird veranschaulicht, wie NGINX als statusabhängige Gruppe mit drei Replikaten bereitgestellt wird; hierbei wird von jedem Replikat jeweils ein separates Bucket verwendet oder von allen Replikaten wird ein gemeinsames Bucket verwendet.

   **Beispiel für die Erstellung einer statusabhängigen Gruppe mit drei Replikaten, von denen ein separates Bucket verwendet wird**:
   ```
   apiVersion: v1
   kind: Service
   metadata:
     name: nginx-v01
     namespace: default
     labels:
       app: nginx-v01 # must match spec.template.metadata.labels and spec.selector.matchLabels in stateful set YAML
   spec:
     ports:
     - port: 80
       name: web
     clusterIP: None
     selector:
       app: nginx-v01 # must match spec.template.metadata.labels and spec.selector.matchLabels in stateful set YAML
   ---
   apiVersion: apps/v1
   kind: StatefulSet
   metadata:
     name: web-v01
     namespace: default
   spec:
     selector:
       matchLabels:
         app: nginx-v01 # must match spec.template.metadata.labels in stateful set YAML and metadata.labels in service YAML
     serviceName: "nginx-v01"
     replicas: 3
     template:
       metadata:
         labels:
           app: nginx-v01 # must match spec.selector.matchLabels in stateful set YAML and metadata.labels in service YAML
       spec:
         terminationGracePeriodSeconds: 10
         containers:
         - name: nginx
           image: k8s.gcr.io/nginx-slim:0.8
           ports:
           - containerPort: 80
             name: web
           volumeMounts:
           - name: mypvc
             mountPath: /usr/share/nginx/html
     volumeClaimTemplates:
     - metadata:
         name: mypvc
         annotations:
           ibm.io/auto-create-bucket: "true"
           ibm.io/auto-delete-bucket: "true"
           ibm.io/bucket: ""
           ibm.io/secret-name: mysecret
           volume.beta.kubernetes.io/storage-class: ibmc-s3fs-standard-perf-cross-region
           volume.beta.kubernetes.io/storage-provisioner: ibm.io/ibmc-s3fs
       spec:
         accessModes: [ "ReadWriteOnce" ]
         storageClassName: "ibmc-s3fs-standard-perf-cross-region"
         resources:
           requests:
             storage: 1Gi
   ```
   {: codeblock}

   **Beispiel für die Erstellung einer statusabhängigen Gruppe mit 3 Replikaten, von denen dasselbe Bucket `mybucket` gemeinsam verwendet wird**:
   ```
   apiVersion: v1
   kind: Service
   metadata:
     name: nginx-v01
     namespace: default
     labels:
       app: nginx-v01 # must match spec.template.metadata.labels and spec.selector.matchLabels in stateful set YAML
   spec:
     ports:
     - port: 80
       name: web
     clusterIP: None
     selector:
       app: nginx-v01 # must match spec.template.metadata.labels and spec.selector.matchLabels in stateful set YAML
   ---
   apiVersion: apps/v1
   kind: StatefulSet
   metadata:
     name: web-v01
     namespace: default
   spec:
     selector:
       matchLabels:
         app: nginx-v01 # must match spec.template.metadata.labels in stateful set YAML and metadata.labels in service YAML
     serviceName: "nginx-v01"
     replicas: 3
     template:
       metadata:
         labels:
           app: nginx-v01 # must match spec.selector.matchLabels in stateful set YAML and metadata.labels in service YAML
       spec:
         terminationGracePeriodSeconds: 10
         containers:
         - name: nginx
           image: k8s.gcr.io/nginx-slim:0.8
           ports:
           - containerPort: 80
             name: web
           volumeMounts:
           - name: mypvc
             mountPath: /usr/share/nginx/html
     volumeClaimTemplates:
     - metadata:
         name: mypvc
         annotations:
           ibm.io/auto-create-bucket: "false"
           ibm.io/auto-delete-bucket: "false"
           ibm.io/bucket: mybucket
           ibm.io/secret-name: mysecret
           volume.beta.kubernetes.io/storage-class: ibmc-s3fs-standard-perf-cross-region
           volume.beta.kubernetes.io/storage-provisioner: ibm.io/ibmc-s3fs
       spec:
         accessModes: [ "ReadOnlyMany" ]
         storageClassName: "ibmc-s3fs-standard-perf-cross-region"
         resources:
           requests:
             storage: 1Gi
   ```
   {: codeblock}


   <table>
    <caption>Erklärung der Bestandteile einer YAML-Datei für eine statusabhängige Gruppe</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der Bestandteile einer YAML-Datei für eine statusabhängige Gruppe</th>
    </thead>
    <tbody>
    <tr>
    <td style="text-align:left"><code>metadata.name</code></td>
    <td style="text-align:left">Geben Sie einen Namen für die statusabhängige Gruppe ein. Der eingegebene Name wird zum Erstellen des Namens für den PVC im folgenden Format verwendet: <code>&lt;datenträgername&gt;-&lt;name_der_statusabhängigen_gruppe&gt;-&lt;replikatnummer&gt;</code>. </td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.serviceName</code></td>
    <td style="text-align:left">Geben Sie den Namen des Service ein, der verwendet werden soll, um die statusabhängige Gruppe zugänglich zu machen. </td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.replicas</code></td>
    <td style="text-align:left">Geben Sie die Anzahl der Replikate für die statusabhängige Gruppe ein. </td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.selector.matchLabels</code></td>
    <td style="text-align:left">Geben Sie alle Bezeichnungen ein, die Sie in die statusabhängige Gruppe und in den PVC einschließen möchten. Bezeichnungen, die Sie in <code>volumeClaimTemplates</code> der statusabhängigen Gruppe einschließen, werden von Kubernetes nicht erkannt. Stattdessen müssen Sie diese Bezeichnungen in den Abschnitten <code>spec.selector.matchLabels</code> und <code>spec.template.metadata.labels</code> der YAML-Daten für die statusabhängige Gruppe definieren. Wenn Sie sicherstellen möchten, dass alle Replikate einer statusabhängigen Gruppe in den Lastausgleich des Service einbezogen werden, schließen Sie dieselbe Bezeichnung, die Sie im Abschnitt <code>spec.selector</code> verwendet haben, in die YAML-Datei des Service ein. </td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.template.metadata.labels</code></td>
    <td style="text-align:left">Geben Sie dieselben Bezeichnungen ein, die Sie zum Abschnitt <code>spec.selector.matchLabels</code> der YAML-Datei der statusabhängigen Gruppe hinzugefügt haben. </td>
    </tr>
    <tr>
    <td><code>spec.template.spec.</code></br><code>terminationGracePeriodSeconds</code></td>
    <td>Geben Sie die Anzahl der Sekunden ein, die <code>kubelet</code> zum ordnungsgemäßen Beenden des Pods zur Verfügung stehen sollen, von dem das Replikat der statusabhängigen Gruppe ausgeführt wird. Weitere Informationen finden Sie in [Pods löschen![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/tasks/run-application/force-delete-stateful-set-pod/#delete-pods). </td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.volumeClaimTemplates.</code></br><code>metadata.name</code></td>
    <td style="text-align:left">Geben Sie einen Namen für Ihren Datenträger ein. Verwenden Sie denselben Namen, den Sie im Abschnitt <code>spec.containers.volumeMount.name</code> definiert haben. Der hier eingegebene Name wird zum Erstellen des Namens für den PVC im folgenden Format verwendet: <code>&lt;datenträgername&gt;-&lt;name_der_statusabhängigen_gruppe&gt;-&lt;replikatnummer&gt;</code>. </td>
    </tr>
    <tr>
    <td><code>spec.volumeClaimTemplates.metadata</code></br><code>annotions.ibm.io/auto-create-bucket</code></td>
    <td>Wählen Sie eine der beiden folgenden Optionen aus: <ul><li><strong>true:</strong> Wählen Sie diese Option aus, um automatisch ein Bucket für jedes Replikat der statusabhängigen Gruppe zu erstellen. </li><li><strong>false:</strong> Wählen Sie diese Option aus, wenn ein vorhandenes Bucket von den Replikaten der statusabhängigen Gruppe gemeinsam verwendet werden soll. Stellen Sie sicher, dass der Name des Buckets im Abschnitt <code>spec.volumeClaimTemplates.metadata.annotions.ibm.io/bucket</code> der YAML-Datei der statusabhängigen Gruppe definiert ist.</li></ul></td>
    </tr>
    <tr>
    <td><code>spec.volumeClaimTemplates.metadata</code></br><code>annotions.ibm.io/auto-delete-bucket</code></td>
    <td>Wählen Sie eine der beiden folgenden Optionen aus: <ul><li><strong>true:</strong> Ihre Daten, das Bucket und der PV werden automatisch entfernt, wenn Sie den PVC löschen. Ihre {{site.data.keyword.cos_full_notm}}-Serviceinstanz bleibt bestehen und wird nicht gelöscht. Wenn Sie diese Option auf 'true' setzen, müssen Sie <code>ibm.io/auto-create-bucket: true</code> und <code>ibm.io/bucket: ""</code> festlegen, sodass das Bucket automatisch mit einem Namen im Format <code>tmp-s3fs-xxxx</code> erstellt wird. </li><li><strong>false:</strong> Wenn Sie den PVC löschen, wird der PV automatisch gelöscht, aber Ihre Daten und das Bucket in Ihrer {{site.data.keyword.cos_full_notm}}-Serviceinstanz bleiben erhalten. Um auf Ihre Daten zugreifen zu können, müssen Sie einen neuen PVC mit dem Namen des vorhandenen Buckets erstellen.</li></ul></td>
    </tr>
    <tr>
    <td><code>spec.volumeClaimTemplates.metadata</code></br><code>annotions.ibm.io/bucket</code></td>
    <td>Wählen Sie eine der beiden folgenden Optionen aus: <ul><li><strong>Wenn <code>ibm.io/auto-create-bucket</code> auf 'true' gesetzt ist:</strong> Geben Sie den Namen des Buckets ein, das Sie in {{site.data.keyword.cos_full_notm}} erstellen möchten. Wenn zusätzlich <code>ibm.io/auto-delete-bucket</code> auf <strong>true</strong> gesetzt wird, müssen Sie dieses Feld leer lassen, um dem Bucket automatisch einen Namen im Format 'tmp-s3fs-xxxx' zuzuweisen. Der Name muss in {{site.data.keyword.cos_full_notm}} eindeutig sein.</li><li><strong>Wenn <code>ibm.io/auto-create-bucket</code> auf 'false' gesetzt wird:</strong> Geben Sie den Namen des vorhandenen Buckets ein, auf das Sie im Cluster zugreifen möchten.</li></ul></td>
    </tr>
    <tr>
    <td><code>spec.volumeClaimTemplates.metadata</code></br><code>annotions.ibm.io/secret-name</code></td>
    <td>Geben Sie den Namen des geheimen Schlüssels ein, der die {{site.data.keyword.cos_full_notm}}-Berechtigungsnachweise enthält, die Sie zuvor erstellt haben.</td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.volumeClaimTemplates.metadata.</code></br><code>annotations.volume.beta.</code></br><code>kubernetes.io/storage-class</code></td>
    <td style="text-align:left">Geben Sie die Speicherklasse ein, die Sie verwenden möchten. Wählen Sie eine der beiden folgenden Optionen aus: <ul><li><strong>Wenn <code>ibm.io/auto-create-bucket</code> auf 'true' gesetzt ist:</strong> Geben Sie die Speicherklasse ein, die Sie für das neue Bucket verwenden möchten.</li><li><strong>Wenn <code>ibm.io/auto-create-bucket</code> auf 'false' gesetzt ist:</strong> Geben Sie die Speicherklasse ein, die Sie zum Erstellen des vorhandenen Buckets verwendet haben. </li></ul></br> Zum Auflisten vorhandener Speicherklassen führen Sie <code>kubectl get storageclasses | grep s3</code> aus. Wenn Sie keine Speicherklasse angeben, wird der PVC mit der Standardspeicherklasse erstellt, die im Cluster festgelegt wurde. Stellen Sie sicher, dass von der Standardspeicherklasse der Bereitsteller <code>ibm.io/ibmc-s3fs</code> verwendet wird, damit die statusabhängige Gruppe mit Objektspeicher bereitgestellt wird.</td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.volumeClaimTemplates.</code></br><code>spec.storageClassName</code></td>
    <td>Geben Sie dieselbe Speicherklasse ein, die Sie im Abschnitt <code>spec.volumeClaimTemplates.metadata.annotations.volume.beta.kubernetes.io/storage-class</code> der YAML-Datei der statusabhängigen Gruppe eingegeben haben.  </td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.volumeClaimTemplates.spec.</code></br><code>resource.requests.storage</code></td>
    <td>Geben Sie eine fiktive Größe für das {{site.data.keyword.cos_full_notm}}-Bucket in Gigabyte ein. Die Größe ist für Kubernetes erforderlich, wird jedoch in {{site.data.keyword.cos_full_notm}} nicht beachtet. Sie können eine beliebige Größe angeben. Der tatsächliche Bereich, den Sie in {{site.data.keyword.cos_full_notm}} verwenden, kann anders sein und wird auf der Basis der [Preistabelle ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api) in Rechnung gestellt.</td>
    </tr>
    </tbody></table>


## Daten sichern und wiederherstellen
{: #cos_backup_restore}

{{site.data.keyword.cos_full_notm}} ist für eine hohe Lebensdauer Ihrer Daten konfiguriert und bietet so Schutz vor Datenverlust. Sie finden das SLA in den [ {{site.data.keyword.cos_full_notm}}-Servicebedingungen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www-03.ibm.com/software/sla/sladb.nsf/sla/bm-7857-03).
{: shortdesc}

{{site.data.keyword.cos_full_notm}} stellt kein Versionsprotokoll für Ihre Daten zur Verfügung. Wenn Sie ältere Versionen Ihrer Daten aufbewahren und auf sie zugreifen müssen, müssen Sie Ihre App so einrichten, dass sie die Datenhistorie verwaltet, oder Sie müssen alternative Sicherungslösungen implementieren. Sie können Ihre {{site.data.keyword.cos_full_notm}}-Daten beispielsweise in der lokalen Datenbank speichern oder Bänder zur Archivierung Ihrer Daten verwenden.
{: note}

## Speicherklassenreferenz
{: #cos_storageclass_reference}

### Standard
{: #standard}

<table>
<caption>Objektspeicherklasse: Standard</caption>
<thead>
<th>Merkmale</th>
<th>Einstellung</th>
</thead>
<tbody>
<tr>
<td>Name</td>
<td><code>ibmc-s3fs-standard-cross-region</code></br><code>ibmc-s3fs-standard-perf-cross-region</code></br><code>ibmc-s3fs-standard-regional</code></br><code>ibmc-s3fs-standard-perf-regional</code></td>
</tr>
<tr>
<td>Standardendpunkt für Ausfallsicherheit</td>
<td>Der Endpunkt für die Ausfallsicherheit wird basierend auf dem Standort, an dem sich Ihr Cluster befindet, automatisch festgelegt. Weitere Informationen finden Sie unter [Regionen und Endpunkte](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-endpoints#endpoints). </td>
</tr>
<tr>
<td>Blockgröße</td>
<td>Speicherklassen ohne `perf`: 16 MB</br>Speicherklassen mit `perf`: 52 MB</td>
</tr>
<tr>
<td>Kernel-Cache</td>
<td>Aktiviert</td>
</tr>
<tr>
<td>Abrechnung</td>
<td>Monatlich</td>
</tr>
<tr>
<td>Preisstruktur</td>
<td>[Preise ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api)</td>
</tr>
</tbody>
</table>

### Vault
{: #Vault}

<table>
<caption>Objektspeicherklasse: Vault</caption>
<thead>
<th>Merkmale</th>
<th>Einstellung</th>
</thead>
<tbody>
<tr>
<td>Name</td>
<td><code>ibmc-s3fs-vault-cross-region</code></br><code>ibmc-s3fs-vault-regional</code></td>
</tr>
<tr>
<td>Standardendpunkt für Ausfallsicherheit</td>
<td>Der Endpunkt für die Ausfallsicherheit wird basierend auf dem Standort, an dem sich Ihr Cluster befindet, automatisch festgelegt. Weitere Informationen finden Sie unter [Regionen und Endpunkte](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-endpoints#endpoints). </td>
</tr>
<tr>
<td>Blockgröße</td>
<td>16 MB</td>
</tr>
<tr>
<td>Kernel-Cache</td>
<td>Inaktiviert</td>
</tr>
<tr>
<td>Abrechnung</td>
<td>Monatlich</td>
</tr>
<tr>
<td>Preisstruktur</td>
<td>[Preise ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api)</td>
</tr>
</tbody>
</table>

### Cold
{: #cold}

<table>
<caption>Objektspeicherklasse: Cold</caption>
<thead>
<th>Merkmale</th>
<th>Einstellung</th>
</thead>
<tbody>
<tr>
<td>Name</td>
<td><code>ibmc-s3fs-flex-cross-region</code></br><code>ibmc-s3fs-flex-perf-cross-region</code></br><code>ibmc-s3fs-flex-regional</code></br><code>ibmc-s3fs-flex-perf-regional</code></td>
</tr>
<tr>
<td>Standardendpunkt für Ausfallsicherheit</td>
<td>Der Endpunkt für die Ausfallsicherheit wird basierend auf dem Standort, an dem sich Ihr Cluster befindet, automatisch festgelegt. Weitere Informationen finden Sie unter [Regionen und Endpunkte](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-endpoints#endpoints). </td>
</tr>
<tr>
<td>Blockgröße</td>
<td>16 MB</td>
</tr>
<tr>
<td>Kernel-Cache</td>
<td>Inaktiviert</td>
</tr>
<tr>
<td>Abrechnung</td>
<td>Monatlich</td>
</tr>
<tr>
<td>Preisstruktur</td>
<td>[Preise ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api)</td>
</tr>
</tbody>
</table>

### Flex
{: #flex}

<table>
<caption>Objektspeicherklasse: Flex</caption>
<thead>
<th>Merkmale</th>
<th>Einstellung</th>
</thead>
<tbody>
<tr>
<td>Name</td>
<td><code>ibmc-s3fs-cold-cross-region</code></br><code>ibmc-s3fs-flex-perf-cross-region</code></br><code>ibmc-s3fs-cold-regional</code></br><code>ibmc-s3fs-flex-perf-regional</code></td>
</tr>
<tr>
<td>Standardendpunkt für Ausfallsicherheit</td>
<td>Der Endpunkt für die Ausfallsicherheit wird basierend auf dem Standort, an dem sich Ihr Cluster befindet, automatisch festgelegt. Weitere Informationen finden Sie unter [Regionen und Endpunkte](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-endpoints#endpoints). </td>
</tr>
<tr>
<td>Blockgröße</td>
<td>Speicherklassen ohne `perf`: 16 MB</br>Speicherklassen mit `perf`: 52 MB</td>
</tr>
<tr>
<td>Kernel-Cache</td>
<td>Aktiviert</td>
</tr>
<tr>
<td>Abrechnung</td>
<td>Monatlich</td>
</tr>
<tr>
<td>Preisstruktur</td>
<td>[Preise ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api)</td>
</tr>
</tbody>
</table>
