---

copyright:
  years: 2014, 2019
lastupdated: "2019-04-05"

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


# Fehlerbehebung für Clusterspeicher
{: #cs_troubleshoot_storage}

Ziehen Sie bei der Verwendung von {{site.data.keyword.containerlong}} die folgenden Verfahren für die Fehlerbehebung für Clusterspeicher in Betracht.
{: shortdesc}

Wenn Sie ein allgemeineres Problem haben, testen Sie das [Cluster-Debugging](/docs/containers?topic=containers-cs_troubleshoot).
{: tip}

## In einem Mehrzonencluster schlägt das Anhängen eines persistenten Datenträgers an einen Pod fehl
{: #mz_pv_mount}

{: tsSymptoms}
Ihr Cluster war zuvor ein Einzelzonencluster mit eigenständigen Workerknoten, die sich nicht in Worker-Pools befanden. Sie haben erfolgreich einen Persistent Volume Claim (PVC) angehängt, der den persistenten Datenträger (PV) beschrieben hat, der für die Bereitstellung des Pods Ihrer App verwendet werden soll. Da Sie nun über Worker-Pools verfügen und Ihrem Cluster Zonen hinzugefügt haben, schlägt das Anhängen des persistenten Datenträgers (PV) an einen Pod jedoch fehl.

{: tsCauses}
Bei Mehrzonenclustern müssen die persistenten Datenträger über folgende Bezeichnungen verfügen, damit Pods nicht versuchen, Datenträger in einer anderen Zone anzuhängen.
* `failure-domain.beta.kubernetes.io/region`
* `failure-domain.beta.kubernetes.io/zone`

Neue Cluster mit Worker-Pools, die mehrere Zonen umfassen können, kennzeichnen die persistenten Datenträger standardmäßig. Wenn Sie Ihre Cluster vor der Einführung von Worker-Pools erstellt haben, müssen Sie die Bezeichnungen manuell hinzufügen.

{: tsResolve}
[Aktualisieren Sie die persistenten Datenträger in Ihrem Cluster mit der Region und den Zonenbezeichnungen](/docs/containers?topic=containers-kube_concepts#storage_multizone).

<br />


## Dateispeicher: Dateisysteme für Workerknoten werden schreibgeschützt
{: #readonly_nodes}

{: tsSymptoms}
{: #stuck_creating_state}
Sie bemerken möglicherweise eines der folgenden Symptome:
- Wenn Sie den Befehl `kubectl get pods -o wide` ausführen, dann können Sie erkennen, dass mehrere Pods, die auf demselben Workerknoten ausgeführt werden, im Zustand `ContainerCreating` blockiert sind.
- Wenn Sie den Befehl `kubectl describe` ausführen, sehen Sie den folgenden Fehler im Abschnitt **Ereignisse** `MountVolume.SetUp failed for volume ... read-only file system`.

{: tsCauses}
Das Dateisystem auf dem Workerknoten ist schreibgeschützt.

{: tsResolve}
1.  Erstellen Sie eine Sicherungskopie aller Daten, die möglicherweise auf dem Workerknoten oder in Ihren Containern gespeichert werden.
2.  Laden Sie den Workerknoten erneut, um eine kurzfristige Programmkorrektur für den vorhandenen Workerknoten zu erreichen.
    <pre class="pre"><code>ibmcloud ks worker-reload --cluster &lt;clustername&gt; --worker &lt;worker-id&gt;</code></pre>

Für eine langfristige Programmkorrektur müssen Sie [den Maschinentyp Ihres Worker-Pools aktualisieren](/docs/containers?topic=containers-update#machine_type).

<br />



## Dateispeicher: App schlägt fehl, wenn ein Benutzer ohne Rootberechtigung Eigner des NFS-Dateispeicher-Mountpfads ist
{: #nonroot}

{: tsSymptoms}
Nach dem [Hinzufügen von NFS-Speicher](/docs/containers?topic=containers-file_storage#file_app_volume_mount) zu Ihrer Bereitstellung schlägt die Bereitstellung Ihres Containers fehl. Wenn Sie die Protokolle für Ihren Container abrufen, werden möglicherweise Fehler wie die folgenden angezeigt. Der Pod schlägt fehl und bleibt in einer Neuladeschleife stecken.

```
write-permission
```
{: screen}

```
do not have required permission
```
{: screen}

```
cannot create directory '/bitnami/mariadb/data': Permission denied
```
{: screen}

{: tsCauses}
Standardmäßig haben Benutzer ohne Rootberechtigung keinen Schreibzugriff auf den Datenträgermountpfad für NFS-gesicherte Speicher. Einige allgemeine App-Images, wie z. B. Jenkins und Nexus3, geben einen Benutzer ohne Rootberechtigung an, der Eigner des Mountpfads in der Dockerfile ist. Wenn Sie einen Container aus dieser Dockerfile erstellen, schlägt die Erstellung des Containers aufgrund unzureichender Berechtigungen für den Benutzer ohne Rootberechtigung auf dem Mountpfad fehl. Um Schreibberechtigung zu erteilen, können Sie die Dockerfile so ändern, dass der Benutzer ohne Rootberechtigung temporär zur Stammbenutzergruppe hinzugefügt wird, bevor die Mountpfadberechtigungen geändert werden, oder Sie verwenden einen Init-Container.

Wenn Sie ein Helm-Diagramm verwenden, um das Image bereitzustellen, bearbeiten Sie die Helm-Bereitstellung, um einen Init-Container zu verwenden.
{:tip}



{: tsResolve}
Wenn Sie einen [Init-Container![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/) in Ihre Bereitstellung einschließen, können Sie einem Benutzer ohne Rootberechtigung, der in Ihrer Dockerfile angegeben ist, Schreibberechtigungen für den Datenträgermountpfad innerhalb des Containers erteilen. Der Init-Container startet, bevor Ihr App-Container startet. Der Init-Container erstellt den Datenträgermountpfad innerhalb des Containers, ändert den Mountpfad, sodass der richtige Benutzer (ohne Rootberechtigung) Eigner ist, und schließt den Pfad wieder. Anschließend wird Ihr App-Container mit dem Benutzer ohne Rootberechtigung gestartet, der in den Mountpfad schreiben muss. Da der Benutzer ohne Rootberechtigung bereits Eigner des Pfads ist, ist das Schreiben in den Mountpfad erfolgreich. Wenn Sie keinen Init-Container verwenden möchten, können Sie die Dockerfile ändern, um Benutzern ohne Rootberechtigung Zugriff auf den NFS-Dateispeicher hinzuzufügen.


Vorbereitende Schritte: [Melden Sie sich an Ihrem Konto an. Geben Sie als Ziel die entsprechende Region und, sofern zutreffend, die Ressourcengruppe an. Legen Sie den Kontext für den Cluster fest.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  Öffnen Sie die Dockerfile für Ihre App und rufen Sie die Benutzer-ID (UID) und die Benutzer-ID (GID) von dem Benutzer ab, dem Sie Schreibzugriff auf den Datenträgermountpfad erteilen möchten. Im Beispiel einer Jenkins-Dockerfile lautet die Information:
    - UID: `1000`
    - GID: `1000`

    **Beispiel**:

    ```
    FROM openjdk:8-jdk

    RUN apt-get update &&apt-get install -y git curl &&rm -rf /var/lib/apt/lists/*

    ARG user=jenkins
    ARG group=jenkins
    ARG uid=1000
    ARG gid=1000
    ARG http_port=8080
    ARG agent_port=50000

    ENV JENKINS_HOME /var/jenkins_home
    ENV JENKINS_SLAVE_AGENT_PORT ${agent_port}
    ...
    ```
    {:screen}

2.  Fügen Sie persistenten Speicher zu Ihrer App hinzu, indem Sie einen PVC (Persistent Volume Claim) erstellen. Dieses Beispiel verwendet die Speicherklasse `ibmc-file-bronze`. Um verfügbare Speicherklassen zu überprüfen, führen Sie den folgenden Befehl aus: `kubectl get storageclasses`.

    ```
    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
      name: mypvc
      annotations:
        volume.beta.kubernetes.io/storage-class: "ibmc-file-bronze"
    spec:
      accessModes:
        - ReadWriteMany
      resources:
        requests:
          storage: 20Gi
    ```
    {: codeblock}

3.  Erstellen Sie den PVC.

    ```
    kubectl apply -f mypvc.yaml
    ```
    {: pre}

4.  Fügen Sie in Ihrer `.yaml`-Bereitstellungsdatei den Init-Container hinzu. Beziehen Sie die UID und GID ein, die Sie zuvor abgerufen haben.

    ```
    initContainers:
    - name: initcontainer # Kann durch einen beliebigen Namen ersetzt werden
      image: alpine:latest
      command: ["/bin/sh", "-c"]
      args:
        - chown <UID>:<GID> /mount; # Ersetzen Sie UID und GID durch Werte aus der Dockerfile
      volumeMounts:
      - name: volume # Kann durch einen beliebigen Namen ersetzt werden
        mountPath: /mount # Muss mit dem Mountpfad in der args-Zeile übereinstimmen
    ```
    {: codeblock}

    **Beispiel** für eine Jenkins-Bereitstellung

    ```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: my_pod
    spec:
      replicas: 1
      selector:
        matchLabels:
          app: jenkins      
      template:
        metadata:
          labels:
            app: jenkins
        spec:
          containers:
          - name: jenkins
            image: jenkins
            volumeMounts:
            - mountPath: /var/jenkins_home
              name: volume
          volumes:
          - name: volume
            persistentVolumeClaim:
              claimName: mypvc
          initContainers:
          - name: permissionsfix
            image: alpine:latest
            command: ["/bin/sh", "-c"]
            args:
              - chown 1000:1000 /mount;
            volumeMounts:
            - name: volume
              mountPath: /mount
    ```
    {: codeblock}

5.  Erstellen Sie den Pod und hängen Sie den PVC an Ihren Pod an.

    ```
    kubectl apply -f mein_pod.yaml
    ```
    {: pre}

6. Überprüfen Sie, ob der Datenträger erfolgreich an Ihren Pod angehängt wurde. Notieren Sie den Namen des Pod und den **Containers/Mounts**-Pfad.

    ```
    kubectl describe pod <mein_pod>
    ```
    {: pre}

    **Beispielausgabe**:

    ```
    Name:		    mypod-123456789
    Namespace:	default
    ...
    Init Containers:
    ...
    Mounts:
      /mount from volume (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-cp9f0 (ro)
    ...
    Containers:
      jenkins:
        Container ID:
        Image:		jenkins
        Image ID:
        Port:		  <none>
        State:		Waiting
          Reason:		PodInitializing
        Ready:		False
        Restart Count:	0
        Environment:	<none>
        Mounts:
          /var/jenkins_home from volume (rw)
          /var/run/secrets/kubernetes.io/serviceaccount from default-token-cp9f0 (ro)
    ...
    Volumes:
      myvol:
        Type: PersistentVolumeClaim (Referenz auf einen PersistentVolumeClaim im gleichen Namensbereich)
        ClaimName: mypvc
        ReadOnly: false

    ```
    {: screen}

7.  Melden Sie sich beim Pod an, indem Sie den Podnamen verwenden, den Sie zuvor notiert haben.

    ```
    kubectl exec -it <mein_pod-123456789> /bin/bash
    ```
    {: pre}

8. Überprüfen Sie die Berechtigungen des Mountpfads Ihres Containers. In dem Beispiel ist der Mountpfad `/var/jenkins_home`.

    ```
    ls -ln /var/jenkins_home
    ```
    {: pre}

    **Beispielausgabe**:

    ```
    jenkins@mypod-123456789:/$ ls -ln /var/jenkins_home
    total 12
    -rw-r--r-- 1 1000 1000  102 Mar  9 19:58 copy_reference_file.log
    drwxr-xr-x 2 1000 1000 4096 Mar  9 19:58 init.groovy.d
    drwxr-xr-x 9 1000 1000 4096 Mar  9 20:16 war
    ```
    {: screen}

    Diese Ausgabe zeigt, dass die GID und UID von Ihrer Dockerfile (in diesem Beispiel `1000` und `1000`) Eigner des Mountpfads innerhalb des Containers sind.

<br />


## Dateispeicher: Hinzufügen von Zugriff für Benutzer ohne Rootberechtigung auf persistente Speicher ist fehlgeschlagen
{: #cs_storage_nonroot}

{: tsSymptoms}
Nach dem [Hinzufügen von Zugriff für Benutzer ohne Rootberechtigung auf persistenten Speicher](#nonroot) oder nach dem Bereitstellen eines Helm-Diagramms, in dem ein Benutzer ohne Rootberechtigung angegeben ist, kann der Benutzer nicht in den angehängten Speicher schreiben.

{: tsCauses}
Die Bereitstellungs- oder Helm-Diagrammkonfiguration gibt den [Sicherheitskontext](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/) für `fsGroup` (Gruppen-ID) und `runAsUser` (Benutzer-ID) des Pods an. Aktuell unterstützt {{site.data.keyword.containerlong_notm}} die Spezifikation `fsGroup` nicht, sondern nur `runAsUser` mit der Festlegung als `0` (Rootberechtigungen).

{: tsResolve}
Entfernen Sie in der Konfiguration die `securityContext`-Felder für `fsGroup` und `runAsUser` aus dem Image, der Bereitstellungs- oder Helm-Diagrammkonfigurationsdatei und nehmen Sie die Bereitstellung erneut vor. Wenn Sie die Eigentumsrechte des Mountpfads von `nobody` ändern müssen, [fügen Sie Zugriff für Benutzer ohne Rootberechtigung hinzu](#nonroot). Nachdem Sie den [non-root `initContainer`](#nonroot) hinzugefügt haben, legen Sie `runAsUser` auf Containerebene fest, nicht Podebene.

<br />




## Blockspeicher: Blockspeicher wird schreibgeschützt
{: #readonly_block}

{: tsSymptoms}
Sie bemerken möglicherweise die folgenden Symptome:
- Wenn Sie den Befehl `kubectl get pods -o wide` ausführen, können Sie erkennen, dass mehrere Pods auf demselben Workerknoten  im Zustand `ContainerCreating` oder `CrashLoopBackOff` blockiert sind. Alle diese Pods verwenden dieselbe Blockspeicherinstanz.
- Wenn Sie den Befehl `kubectl describe pod` ausführen, sehen Sie den folgenden Fehler im Abschnitt **Events**: `MountVolume.SetUp failed for volume ... read-only`.

{: tsCauses}
Wenn ein Netzfehler auftritt, während ein Pod auf einen Datenträger schreibt, schützt die IBM Cloud-Infrastruktur (SoftLayer) die Daten auf dem Datenträger gegen Beschädigung, indem der Datenträger in den Schreibschutzmodus versetzt wird. Pods, die diesen Datenträger verwenden, können keine Daten mehr auf den Datenträger schreiben und empfangen einen Fehler.

{: tsResolve}
1. Prüfen Sie die Version des {{site.data.keyword.Bluemix_notm}} Block Storage-Plug-ins, das in Ihrem Cluster installiert ist.
   ```
   helm ls
   ```
   {: pre}

2. Überprüfen Sie, ob Sie die [neueste Version des {{site.data.keyword.Bluemix_notm}} Block Storage-Plug-ins](https://cloud.ibm.com/kubernetes/solutions/helm-charts/ibm/ibmcloud-block-storage-plugin) verwenden. Ist dies nicht der Fall, [aktualisieren Sie Ihr Plug-in](/docs/containers?topic=containers-block_storage#updating-the-ibm-cloud-block-storage-plug-in).
3. Wenn Sie eine Kubernetes-Bereitstellung für Ihren Pod verwendet haben, starten Sie den fehlgeschlagenen Pod erneut, indem Sie den Pod entfernen und Kubernetes den Pod erneut erstellen lassen. Wenn Sie keine Bereitstellung verwendet haben, rufen Sie die YAML-Datei ab, die zum Erstellen Ihres Pods verwendet wurde, indem Sie den Befehl `kubectl get pod <podname> -o yaml >pod.yaml` ausführen. Löschen Sie anschließend den Pod und erstellen Sie ihn neu.
    ```
    kubectl delete pod <podname>
    ```
    {: pre}

4. Prüfen Sie, ob die erneute Erstellung Ihres Pods das Problem behoben hat. Falls nicht, laden Sie den Workerknoten neu.
   1. Ermitteln Sie den Workerknoten, auf dem Ihr Pod ausgeführt wird, und notieren Sie die private IP-Adresse, die Ihrem Workerknoten zugeordnet ist.
      ```
      kubectl describe pod <podname> | grep Node
      ```
      {: pre}

      Beispielausgabe:
      ```
      Node:               10.75.XX.XXX/10.75.XX.XXX
      Node-Selectors:  <none>
      ```
      {: screen}

   2. Rufen Sie die **ID** Ihres Workerknotens ab, indem Sie die private IP-Adresse aus dem vorherigen Schritt verwenden.
      ```
      ibmcloud ks workers --cluster <clustername_oder_-id>
      ```
      {: pre}

   3. [Laden Sie den Workerknoten ordnungsgemäß neu](/docs/containers?topic=containers-cs_cli_reference#cs_worker_reload).


<br />


## Blockspeicher: Das Anhängen eines vorhandenen Blockspeichers an einen Pod schlägt aufgrund des falschen Dateisystems fehl
{: #block_filesystem}

{: tsSymptoms}
Bei der Ausführung des Befehls `kubectl describe pod <pod_name>` wird der folgende Fehler angezeigt:
```
failed to mount the volume as "ext4", it already contains xfs. Mount error: mount failed: exit status 32
```
{: screen}

{: tsCauses}
Sie verfügen über eine vorhandene Blockspeichereinheit, die für ein `XFS`-Dateisystem konfiguriert ist. Um diese Einheit an Ihren Pod anzuhängen, haben Sie [einen persistenten Datenträger (PV) erstellt](/docs/containers?topic=containers-block_storage#existing_block), der `ext4` als Ihr Dateisystem oder kein Dateisystem im Abschnitt `spec/flexVolume/fsType` angegeben hat. Wenn kein Dateisystem definiert ist, nimmt der persistente Datenträger standardmäßig den Wert `ext4` ein.
Der persistente Datenträger wurde erfolgreich erstellt und mit der vorhandenen Blockspeicherinstanz verknüpft. Wenn Sie jedoch versuchen, den persistenten Datenträger mithilfe eines PVC an den Cluster anzuhängen, schlägt dieser Vorgang fehl. Sie können die `XFS`-Blockspeicherinstanz nicht mit einem `ext4`-Dateisystem an den Pod anhängen.

{: tsResolve}
Aktualisieren Sie das Dateisystem im vorhandenen persistenten Datenträger von `ext4` auf `XFS`.

1. Listen Sie die vorhandenen persistenten Datenträger in Ihrem Cluster auf und notieren Sie sich den Namen des persistenten Datenträgers, den Sie für die vorhandene Blockspeicherinstanz verwendet haben.
   ```
   kubectl get pv
   ```
   {: pre}

2. Speichern Sie die YALM-Datei des persistenten Datenträgers auf der lokalen Maschine.
   ```
   kubectl get pv <pv-name> -o yaml > <dateipfad/xfs_pv.yaml>
   ```
   {: pre}

3. Öffnen Sie die YALM-Datei und ändern Sie den Wert für `fsType` von `ext4` in `xfs`.
4. Ersetzen Sie den persistenten Datenträger im Cluster.
   ```
   kubectl replace --force -f <dateipfad/xfs_pv.yaml>
   ```
   {: pre}

5. Melden Sie sich bei dem Pod an, an den Sie den persistenten Datenträger angehängt haben.
   ```
   kubectl exec -it <pod-name> sh
   ```
   {: pre}

6. Stellen Sie sicher, dass das Dateisystem in `XFS` geändert wurde.
   ```
   df -Th
   ```
   {: pre}

   Beispielausgabe:
   ```
   Filesystem Type Size Used Avail Use% Mounted on /dev/mapper/3600a098031234546d5d4c9876654e35 xfs 20G 33M 20G 1% /myvolumepath
   ```
   {: screen}

<br />



## Objektspeicher: Installation des Helm-Plug-ins {{site.data.keyword.cos_full_notm}} `ibmc` schlägt fehl
{: #cos_helm_fails}

{: tsSymptoms}
Wenn Sie das Helm-Plug-in {{site.data.keyword.cos_full_notm}} `ibmc` installieren, schlägt die Installation mit dem folgenden Fehler fehl:
```
Error: symlink /Users/ibm/ibmcloud-object-storage-plugin/helm-ibmc /Users/ibm/.helm/plugins/helm-ibmc: file exists
```
{: screen}

{: tsCauses}
Bei der Installation des Helm-Plug-ins `ibmc` wird eine symbolische Verbindung aus dem Verzeichnis `./helm/plugins/helm-ibmc` zu dem Verzeichnis hergestellt, in dem sich das Helm-Plug-in `ibmc` auf Ihrem lokalen System befindet, das normalerweise in `./ibmcloud-object-storage-plugin/helm-ibmc` enthalten ist. Wenn Sie das Helm-Plug-in `ibmc` von Ihrem lokalen System entfernen oder das Helm-Plug-in-Verzeichnis `ibmc` an eine andere Position verschieben, wird die symbolische Verbindung nicht entfernt.

{: tsResolve}
1. Entfernen Sie das {{site.data.keyword.cos_full_notm}}-Helm-Plug-in.
   ```
   rm -rf ~/.helm/plugins/helm-ibmc
   ```
   {: pre}

2. [Installieren Sie {{site.data.keyword.cos_full_notm}}](/docs/containers?topic=containers-object_storage#install_cos).

<br />


## Objektspeicher: PVC- oder Poderstellung schlägt fehl, weil der geheime Kubernetes-Schlüssel nicht gefunden wurde
{: #cos_secret_access_fails}

{: tsSymptoms}
Wenn Sie Ihren PVC erstellen oder einen Pod bereitstellen, der den PVC anhängt, schlägt die Erstellung oder Bereitstellung fehl.

- Beispielfehlernachricht für einen Fehler bei der PVC-Erstellung:
  ```
  pvc-3:1b23159vn367eb0489c16cain12345:cannot get credentials: cannot get secret tsecret-key: secrets "secret-key" not found
  ```
  {: screen}

- Beispielfehlernachricht für einen Fehler bei der Erstellung eines Pods:
  ```
  persistentvolumeclaim "pvc-3" not found (repeated 3 times)
  ```
  {: screen}

{: tsCauses}
Der geheime Kubernetes-Schlüssel, in dem Sie Ihre {{site.data.keyword.cos_full_notm}}-Serviceberechtigungsnachweise, den PVC und den Pod speichern, befinden sich nicht alle im selben Kubernetes-Namensbereich. Wenn der geheime Schlüssel in einem anderen Namensbereich bereitgestellt wird als Ihr PVC oder Pod, kann nicht auf den geheimen Schlüssel zugegriffen werden.

{: tsResolve}
Diese Task erfordert die [{{site.data.keyword.Bluemix_notm}} IAM-Servicerolle **Schreibberechtigter** oder **Manager**](/docs/containers?topic=containers-users#platform) für alle Namensbereiche.

1. Listen Sie die geheimen Schlüssel in Ihrem Cluster auf und überprüfen Sie den Kubernetes-Namensbereich, in dem der geheime Kubernetes-Schlüssel für Ihre {{site.data.keyword.cos_full_notm}}-Serviceinstanz erstellt wird. Der geheime Schlüssel muss `ibm/ibmc-s3fs` als **Typ** aufweisen.
   ```
   kubectl get secrets --all-namespaces
   ```
   {: pre}

2. Überprüfen Sie die YAML-Konfigurationsdatei auf Ihren PVC und Pod, um zu ermitteln, ob Sie denselben Namensbereich verwendet haben. Wenn Sie einen Pod in einem anderen Namensbereich als dem mit dem geheimen Schlüssel bereitstellen wollen, müssen Sie [einen weiteren geheimen Schlüssel](/docs/containers?topic=containers-object_storage#create_cos_secret) in diesem anderen Namensbereich erstellen.

3. Erstellen Sie den PVC oder stellen Sie den Pod in dem gewünschten Namensbereich bereit.

<br />


## Objektspeicher: PVC-Erstellung schlägt wegen falscher Berechtigungsnachweise fehl oder Zugriff verweigert
{: #cred_failure}

{: tsSymptoms}
Wenn Sie den PVC erstellen, wird eine Fehlernachricht ähnlich der folgenden angezeigt:

```
SignatureDoesNotMatch: The request signature we calculated does not match the signature you provided. Check your AWS Secret Access Key and signing method. For more information, see REST Authentication and SOAP Authentication for details.
```
{: screen}

```
AccessDenied: Access Denied status code: 403
```
{: screen}

```
CredentialsEndpointError: failed to load credentials
```
{: screen}

{: tsCauses}
Die {{site.data.keyword.cos_full_notm}}-Serviceberechtigungsnachweise, die Sie für den Zugriff auf die Serviceinstanz verwenden, sind möglicherweise falsch oder lassen nur Lesezugriff auf das Bucket zu.

{: tsResolve}
1. Klicken Sie in der Navigation auf der Seite mit den Servicedetails auf **Serviceberechtigungsnachweise**.
2. Suchen Sie nach Ihren Berechtigungsnachweisen und klicken Sie anschließend auf **Berechtigungsnachweise anzeigen**.
3. Stellen Sie sicher, dass die richtigen Werte für **access_key_id** und **secret_access_key** in Ihrem geheimen Kubernetes-Schlüssel enthalten sind. Falls dies nicht der Fall ist, aktualisieren Sie den geheimen Kubernetes-Schlüssel.
   1. Rufen Sie die YAML-Datei ab, die Sie zum Erstellen des geheimen Schlüssels verwendet haben.
      ```
      kubectl get secret <geheimer_schlüssel> -o yaml
      ```
      {: pre}

   2. Aktualisieren Sie die Werte für **access_key_id** und **secret_access_key**.
   3. Aktualisieren Sie den geheimen Schlüssel.
      ```
      kubectl apply -f secret.yaml
      ```
      {: pre}

4. Überprüfen Sie im Abschnitt **iam_role_crn**, ob Sie über die Rolle `Schreibberechtigter` oder `Manager` verfügen. Wenn Sie nicht über die richtige Rolle verfügen, müssen Sie [neue {{site.data.keyword.cos_full_notm}}-Serviceberechtigungsnachweise mit den richtigen Berechtigungen erstellen](/docs/containers?topic=containers-object_storage#create_cos_service). Aktualisieren Sie anschließend Ihren vorhandenen geheimen Schlüssel oder [erstellen Sie einen neuen geheimen Schlüssel](/docs/containers?topic=containers-object_storage#create_cos_secret) mit Ihren neuen Serviceberechtigungsnachweisen.

<br />


## Objektspeicher: Zugriff auf ein vorhandenes Bucket nicht möglich

{: tsSymptoms}
Wenn Sie den PVC erstellen, kann auf das Bucket in {{site.data.keyword.cos_full_notm}} nicht zugegriffen werden. Es wird eine Fehlernachricht ähnlich der folgenden angezeigt:

```
Failed to provision volume with StorageClass "ibmc-s3fs-standard-regional": pvc:1b2345678b69175abc98y873e2:cannot access bucket <bucketname>: NotFound: Not Found
```
{: screen}

{: tsCauses}
Sie haben möglicherweise die falsche Speicherklasse verwendet, um auf das vorhandene Bucket zuzugreifen, oder Sie haben versucht, auf ein Bucket zuzugreifen, das Sie nicht erstellt haben.

{: tsResolve}
1. Wählen Sie im [{{site.data.keyword.Bluemix_notm}}-Dashboard ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/) Ihre {{site.data.keyword.cos_full_notm}}-Serviceinstanz aus.
2. Wählen Sie **Buckets** aus.
3. Überprüfen Sie die Informationen zu **Klasse** und **Standort** für Ihr vorhandenes Bucket.
4. Wählen Sie die entsprechende [Speicherklasse](/docs/containers?topic=containers-object_storage#cos_storageclass_reference) aus.

<br />


## Objektspeicher: Zugriff auf Dateien mit einem Benutzer ohne Rootberechtigung schlägt fehl
{: #cos_nonroot_access}

{: tsSymptoms}
Sie haben Dateien über die Konsole oder die REST-API in Ihre {{site.data.keyword.cos_full_notm}}-Serviceinstanz hochgeladen. Wenn Sie versuchen, auf diese Dateien als Benutzer ohne Rootberechtigung zuzugreifen, den Sie in der App-Bereitstellung mit `runAsUser` definiert haben, wird der Zugriff auf die Dateien verweigert.

{: tsCauses}
Unter Linux hat eine Datei oder ein Verzeichnis drei Zugriffsgruppen: `Eigner`, `Gruppe` und `Andere`. Wenn Sie eine Datei über die Konsole oder die REST-API in {{site.data.keyword.cos_full_notm}} hochladen, werden die Berechtigungen für `Eigner`, `Gruppe` und `Andere` entfernt. Die Berechtigung der einzelnen Dateien sieht wie folgt aus:

```
d--------- 1 root root 0 Jan 1 1970 <dateiname>
```
{: screen}

Wenn Sie eine Datei mithilfe des {{site.data.keyword.cos_full_notm}}-Plug-ins hochladen, werden die Berechtigungen für die Datei beibehalten und nicht geändert.

{: tsResolve}
Um auf die Datei als Benutzer ohne Rootberechtigung zugreifen zu können, muss der Benutzer ohne Rootberechtigung über Lese- und Schreibberechtigungen für die Datei verfügen. Wenn Sie die Berechtigung für eine Datei im Rahmen Ihrer Podbereitstellung ändern, ist eine Schreiboperation erforderlich. {{site.data.keyword.cos_full_notm}} ist nicht für Schreibworkloads konzipiert. Durch die Aktualisierung der Berechtigungen während der Podbereitstellung kann verhindert werden, dass Ihr Pod in den Status `Aktiv` wechselt.

Um dieses Problem zu beheben, müssen Sie vor dem Anhängen des PVC an Ihren App-Pod einen weiteren Pod erstellen, um die richtige Berechtigung für den Benutzer ohne Rootberechtigung festzulegen.

1. Überprüfen Sie die Berechtigungen Ihrer Dateien in Ihrem Bucket.
   1. Erstellen Sie eine Konfigurationsdatei für Ihren Pod `test-permission` und geben Sie der Datei den Namen `test-permission.yaml`.
      ```
      apiVersion: v1
      kind: Pod
      metadata:
        name: test-permission
      spec:
        containers:
        - name: test-permission
          image: nginx
          volumeMounts:
          - name: cos-vol
            mountPath: /test
        volumes:
        - name: cos-vol
          persistentVolumeClaim:
            claimName: <pvc-name>
      ```
      {: codeblock}

   2. Erstellen Sie den Pod `test-permission`.
      ```
      kubectl apply -f test-permission.yaml
      ```
      {: pre}

   3. Melden Sie sich beim Pod an.
      ```
      kubectl exec test-permission -it bash
      ```
      {: pre}

   4. Navigieren Sie zu Ihrem Mountpfad und listen Sie die Berechtigungen für Ihre Dateien auf.
      ```
      cd test && ls -al
      ```
      {: pre}

      Beispielausgabe:
      ```
      d--------- 1 root root 0 Jan 1 1970 <dateiname>
      ```
      {: screen}

2. Löschen Sie den Pod.
   ```
   kubectl delete pod test-permission
   ```
   {: pre}

3. Erstellen Sie eine Konfigurationsdatei für den Pod, die Sie verwenden, um die Berechtigungen Ihrer Dateien zu korrigieren, und geben Sie ihr den Namen `fix-permission.yaml`.
   ```
   apiVersion: v1
   kind: Pod
   metadata:
     name: fix-permission
     namespace: <namensbereich>
   spec:
     containers:
     - name: fix-permission
       image: busybox
       command: ['sh', '-c']
       args: ['chown -R <nicht-root-benutzer-id> <mountpfad>/*; find <mountpfad>/ -type d -print -exec chmod u=+rwx,g=+rx {} \;']
       volumeMounts:
       - mountPath: "<mountpfad>"
         name: cos-volume
     volumes:
     - name: cos-volume
       persistentVolumeClaim:
         claimName: <pvc-name>
    ```
    {: codeblock}

3. Erstellen Sie den Pod `fix-permission`.
   ```
   kubectl apply -f fix-permission.yaml
   ```
   {: pre}

4. Warten Sie, bis der Pod in den Status `Abgeschlossen` wechselt.  
   ```
   kubectl get pod fix-permission
   ```
   {: pre}

5. Löschen Sie den Pod `fix-permission`.
   ```
   kubectl delete pod fix-permission
   ```
   {: pre}

5. Erstellen Sie wieder den Pod `test-permission`, den Sie zuvor verwendet haben, um die Berechtigungen zu überprüfen.
   ```
   kubectl apply -f test-permission.yaml
   ```
   {: pre}

5. Stellen Sie sicher, dass die Berechtigungen für Ihre Dateien aktualisiert wurden.
   1. Melden Sie sich beim Pod an.
      ```
      kubectl exec test-permission -it bash
      ```
      {: pre}

   2. Navigieren Sie zu Ihrem Mountpfad und listen Sie die Berechtigungen für Ihre Dateien auf.
      ```
      cd test && ls -al
      ```
      {: pre}

      Beispielausgabe:
      ```
      -rwxrwx--- 1 <nicht-root-benutzer-id> root 6193 Aug 21 17:06 <dateiname>
      ```
      {: screen}

6. Löschen Sie den Pod `test-permission`.
   ```
   kubectl delete pod test-permission
   ```
   {: pre}

7. Hängen Sie den PVC mit dem Benutzer ohne Rootberechtigung an die App an.

   Definieren Sie den Benutzer ohne Rootberechtigung als `runAsUser`, ohne gleichzeitig `fsGroup` in Ihrer YAML-Datei für die Bereitstellung zu definieren. Wenn Sie `fsGroup` festlegen, wird das {{site.data.keyword.cos_full_notm}}-Plug-in ausgelöst, das die Gruppenberechtigungen für alle Dateien in einem Bucket aktualisiert, wenn der Pod bereitgestellt wird. Das Aktualisieren der Berechtigungen ist eine Schreiboperation, die möglicherweise verhindert, dass Ihr Pod in den Status `Aktiv` wechselt.
   {: important}

Laden Sie keine Dateien über die Konsole oder die REST-API hoch, nachdem Sie die richtigen Dateiberechtigungen in Ihrer {{site.data.keyword.cos_full_notm}}-Serviceinstanz festgelegt haben. Verwenden Sie das {{site.data.keyword.cos_full_notm}}-Plug-in, um Dateien zu Ihrer Serviceinstanz hinzuzufügen.
{: tip}

<br />




## Hilfe und Unterstützung anfordern
{: #storage_getting_help}

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

