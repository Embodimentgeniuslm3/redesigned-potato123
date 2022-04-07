---

copyright:
  years: 2014, 2019
lastupdated: "2019-04-16"

keywords: kubernetes, iks, local persistent storage

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


# IBM Cloud-Speicherdienstprogramme
{: #utilities}

## IBM Cloud Block Storage Attacher-Plug-in installieren (Beta)
{: #block_storage_attacher}

Mithilfe des Plug-ins für {{site.data.keyword.Bluemix_notm}} Block Storage Attacher können Sie unaufbereiteten, unformatierten und nicht angehängten Blockspeicher einem Workerknoten in Ihrem Cluster zuordnen.  
{: shortdesc}

Sie wollen zum Beispiel Ihre Daten mit einer SDS-Lösung (SDS - Software Defined Storage, softwaredefinierter Speicher) wie [Portworx](/docs/containers?topic=containers-portworx) speichern, dazu jedoch keine Bare-Metal-Workerknoten verwenden, die für die SDS-Nutzung optimiert sind und mit zusätzlichen lokalen Platten bereitgestellt werden. Wenn Sie Ihrem Nicht-SDS-Workerknoten lokale Platten hinzufügen wollen, müssen Sie Ihre Blockspeichereinheiten manuell in Ihrem {{site.data.keyword.Bluemix_notm}}-Infrastrukturkonto erstellen und den Speicher mithilfe von {{site.data.keyword.Bluemix_notm}} Block Volume Attacher Ihrem Nicht-SDS-Workerknoten zuordnen.

Das {{site.data.keyword.Bluemix_notm}} Block Volume Attacher-Plug-in erstellt Pods auf jedem Workerknoten in Ihrem Cluster als Teil einer Dämongruppe und richtet eine Kubernetes-Speicherklasse ein, die Sie später zum Zuordnen der Blockspeichereinheit zu Ihren Nicht-SDS-Workerknoten verwenden können.

Suchen Sie nach Anweisungen zum Aktualisieren oder Entfernen des {{site.data.keyword.Bluemix_notm}} Block Volume Attacher-Plug-ins? Weitere Informationen hierzu finden Sie in den Abschnitten [Plug-in aktualisieren](#update_block_attacher) und [Plug-in entfernen](#remove_block_attacher).
{: tip}

1.  [Befolgen Sie die Anweisungen](/docs/containers?topic=containers-helm#public_helm_install) zum Installieren des Helm-Clients auf Ihrer lokalen Maschine und installieren Sie den Helm-Server (tiller) mit einem Servicekonto in Ihrem Cluster.

2.  Überprüfen Sie, ob 'tiller' mit einem Servicekonto installiert ist.

    ```
    kubectl get serviceaccount -n kube-system | grep tiller
    ```
    {: pre}

    Beispielausgabe:

    ```
    NAME                                 SECRETS   AGE
    tiller                               1         2m
    ```
    {: screen}

3. Aktualisieren Sie das Helm-Repository, um die aktuelle Version aller Helm-Diagramme in diesem Repository abzurufen.
   ```
   helm repo update
   ```
   {: pre}

4. Installieren Sie das {{site.data.keyword.Bluemix_notm}} Block Volume Attacher-Plug-in. Wenn Sie das Plug-in installieren, werden Ihrem Cluster vordefinierte Blockspeicherklassen hinzugefügt.
   ```
   helm install iks-charts/ibm-block-storage-attacher --name block-attacher
   ```
   {: pre}

   Beispielausgabe:
   ```
   NAME:   block-volume-attacher
   LAST DEPLOYED: Thu Sep 13 22:48:18 2018
   NAMESPACE: default
   STATUS: DEPLOYED

   RESOURCES:
   ==> v1beta1/ClusterRoleBinding
   NAME                             AGE
   ibmcloud-block-storage-attacher  1s

   ==> v1beta1/DaemonSet
   NAME                             DESIRED  CURRENT  READY  UP-TO-DATE  AVAILABLE  NODE SELECTOR  AGE
   ibmcloud-block-storage-attacher  0        0        0      0           0          <none>         1s

   ==> v1/StorageClass
   NAME                 PROVISIONER                AGE
   ibmc-block-attacher  ibm.io/ibmc-blockattacher  1s

   ==> v1/ServiceAccount
   NAME                             SECRETS  AGE
   ibmcloud-block-storage-attacher  1        1s

   ==> v1beta1/ClusterRole
   NAME                             AGE
   ibmcloud-block-storage-attacher  1s

   NOTES:
   Thank you for installing: ibmcloud-block-storage-attacher.   Your release is named: block-volume-attacher

   Please refer Chart README.md file for attaching a block storage
   Please refer Chart RELEASE.md to see the release details/fixes
   ```
   {: screen}

5. Überprüfen Sie, ob die {{site.data.keyword.Bluemix_notm}} Block Volume Attacher-Dämongruppe erfolgreich installiert wurde.
   ```
   kubectl get pod -n kube-system -o wide | grep attacher
   ```
   {: pre}

   Beispielausgabe:
   ```
   ibmcloud-block-storage-attacher-z7cv6           1/1       Running            0          19m
   ```
   {: screen}

   Die Installation war erfolgreich, wenn ein oder mehrere Pods des Typs **ibmcloud-block-storage-attacher** angezeigt werden. Die Anzahl der Pods entspricht der Anzahl der Workerknoten in Ihrem Cluster. Alle Pods müssen den Status **Running** (Aktiv) aufweisen.

6. Überprüfen Sie, ob die Speicherklasse für {{site.data.keyword.Bluemix_notm}} Block Volume Attacher erfolgreich erstellt wurde.
   ```
   kubectl get storageclasses | grep attacher
   ```
   {: pre}

   Beispielausgabe:
   ```
   ibmc-block-attacher       ibm.io/ibmc-blockattacher   11m
   ```
   {: screen}

### IBM Cloud Block Storage Attacher-Plug-in aktualisieren
{: #update_block_attacher}

Sie können ein Upgrade des vorhandenen {{site.data.keyword.Bluemix_notm}} Block Storage Attacher-Plug-ins auf die aktuelle Version durchführen.
{: shortdesc}

1. Aktualisieren Sie das Helm-Repository, um die aktuelle Version aller Helm-Diagramme in diesem Repository abzurufen.
   ```
   helm repo update
   ```
   {: pre}

2. Optional: Laden Sie das aktuellste Helm-Diagramm auf Ihre lokale Maschine herunter. Extrahieren Sie anschließend das Paket und überprüfen Sie die Datei `release.md` auf die neuesten Releaseinformationen.
   ```
   helm fetch iks-charts/ibmcloud-block-storage-plugin
   ```
   {: pre}

3. Ermitteln Sie den Namen des Helm-Diagramms für das {{site.data.keyword.Bluemix_notm}} Block Storage Attacher-Plug-in.
   ```
   helm ls | grep ibm-block-storage-attacher
   ```
   {: pre}

   Beispielausgabe:
   ```
   <helm-diagrammname>	1       	Wed Aug  1 14:55:15 2018	DEPLOYED	ibm-block-storage-attacher-1.0.0	default
   ```
   {: screen}

4. Führen Sie ein Upgrade von {{site.data.keyword.Bluemix_notm}} Block Storage Attacher auf die aktuelle Version durch.
   ```
   helm upgrade --force --recreate-pods <helm-diagrammname> ibm-block-storage-attacher
   ```
   {: pre}

### IBM Cloud Block Volume Attacher-Plug-in entfernen
{: #remove_block_attacher}

Wenn Sie in Ihrem Cluster das {{site.data.keyword.Bluemix_notm}} Block Storage Attacher-Plug-in nicht bereitstellen und verwenden wollen, können Sie das Helm-Diagramm deinstallieren.
{: shortdesc}

1. Ermitteln Sie den Namen des Helm-Diagramms für das {{site.data.keyword.Bluemix_notm}} Block Storage Attacher-Plug-in.
   ```
   helm ls | grep ibm-block-storage-attacher
   ```
   {: pre}

   Beispielausgabe:
   ```
   <helm-diagrammname>	1       	Wed Aug  1 14:55:15 2018	DEPLOYED	ibm-block-storage-attacher-1.0.0	default
   ```
   {: screen}

2. Löschen Sie das {{site.data.keyword.Bluemix_notm}} Block Storage Attacher-Plug-in, indem Sie das Helm-Diagramm entfernen.
   ```
   helm delete <helm-diagrammname> --purge
   ```
   {: pre}

3. Überprüfen Sie, ob die Pods für das {{site.data.keyword.Bluemix_notm}} Block Storage Attacher-Plug-in entfernt wurden.
   ```
   kubectl get pod -n kube-system -o wide | grep attacher
   ```
   {: pre}

   Das Entfernen der Pods war erfolgreich, wenn in Ihrer CLI-Ausgabe keine Pods angezeigt werden.

4. Überprüfen Sie, ob die Speicherklasse für {{site.data.keyword.Bluemix_notm}} Block Storage Attacher entfernt wurde.
   ```
   kubectl get storageclasses | grep attacher
   ```
   {: pre}

   Das Entfernen der Speicherklasse war erfolgreich, wenn in Ihrer CLI-Ausgabe keine Speicherklasse angezeigt wird.

## Unformatierten Blockspeicher automatisch bereitstellen und Workerknoten für den Zugriff auf den Speicher berechtigen
{: #automatic_block}

Mithilfe des {{site.data.keyword.Bluemix_notm}} Block Volume Attacher-Plug-ins können Sie allen Workerknoten in Ihrem Cluster automatisch unaufbereiteten, unformatierten und nicht angehängten Blockspeicher mit derselben Konfiguration hinzufügen.
{: shortdesc}

Der Container `mkpvyaml`, der im {{site.data.keyword.Bluemix_notm}} Block Volume Attacher-Plug-in enthalten ist, ist dazu konfiguriert, ein Script auszuführen, das alle Workerknoten in Ihrem Cluster sucht, unaufbereiteten Blockspeicher im {{site.data.keyword.Bluemix_notm}}-Infrastrukturportal erstellt und dann die Workerknoten zum Zugriff auf den Speicher berechtigt.

Wenn Sie unterschiedliche Blockspeicherkonfigurationen hinzufügen möchten, fügen Sie den Blockspeicher nur einer Untergruppe der Workerknoten hinzu oder, wenn Sie mehr Kontrolle über den Bereitstellungsprozess haben möchten, [fügen Sie Blockspeicher manuell hinzu](#manual_block).
{: tip}


1. Melden Sie sich bei {{site.data.keyword.Bluemix_notm}} an und definieren Sie die Ressourcengruppe, in der sich Ihr Cluster befindet, als Ziel
   ```
   ibmcloud login
   ```
   {: pre}

2.  Klonen Sie das Repository von {{site.data.keyword.Bluemix_notm}} Storage Utilities.
    ```
    git clone https://github.com/IBM/ibmcloud-storage-utilities.git
    ```
    {: pre}

3. Navigieren Sie in das Verzeichnis `block-storage-utilities`.
   ```
   cd ibmcloud-storage-utilities/block-storage-provisioner
   ```
   {: pre}

4. Öffnen Sie die Datei `yamlgen.yaml` und geben Sie die Blockspeicherkonfiguration an, die Sie jedem Workerknoten im Cluster hinzufügen möchten.
   ```
   #
   # Can only specify 'performance' OR 'endurance' and associated clause
   #
   cluster:  <clustername>     #  Enter the name of your cluster
   region:   <region>          #  Enter the IBM Cloud Kubernetes Service region where you created your cluster
   type:  <speichertyp>        #  Enter the type of storage that you want to provision. Choose between "performance" or "endurance"
   offering: storage_as_a_service
   # performance:
   #    - iops:  <iops>
   endurance:
     - tier:  2                #   [0.25|2|4|10]
   size:  [ 20 ]               #   Enter the size of your storage in gigabytes
   ```
   {: codeblock}

   <table>
   <caption>Erklärung der Komponenten der YAML-Datei</caption>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der YAML-Dateikomponenten</th>
   </thead>
   <tbody>
   <tr>
   <td><code>cluster</code></td>
   <td>Geben Sie den Namen Ihres Clusters ein, in dem Sie unaufbereiteten Blockspeicher hinzufügen wollen. </td>
   </tr>
   <tr>
   <td><code>region</code></td>
   <td>Geben Sie die {{site.data.keyword.containerlong_notm}}-Region ein, in der Sie Ihren Cluster erstellt haben. Führen Sie den Befehl <code>[bxcs] cluster-get --cluster &lt;clustername_oder_-id&gt;</code> aus, um die Region Ihres Clusters zu ermitteln.  </td>
   </tr>
   <tr>
   <td><code>type</code></td>
   <td>Geben Sie den Typ von Speicher ein, den Sie bereitstellen wollen. Wählen Sie <code>performance</code> oder <code>endurance</code> aus. Weitere Informationen finden Sie im Abschnitt zum [Festlegen der Blockspeicherkonfiguration](/docs/containers?topic=containers-block_storage#block_predefined_storageclass).  </td>
   </tr>
   <tr>
   <td><code>performance.iops</code></td>
   <td>Wenn Sie Speicher vom Typ `performance` bereitstellen wollen, geben Sie die Anzahl der E/A-Operationen pro Sekunde (IOPS) ein. Weitere Informationen finden Sie im Abschnitt zum [Festlegen der Blockspeicherkonfiguration](/docs/containers?topic=containers-block_storage#block_predefined_storageclass). Wenn Sie Speicher vom Typ `endurance` bereitstellen wollen, entfernen Sie diesen Abschnitt oder setzen ihn auf Kommentar, indem Sie das Zeichen `#` am Anfang jeder Zeile hinzufügen.
   </tr>
   <tr>
   <td><code>endurance.tier</code></td>
   <td>Wenn Sie Speicher vom Typ `endurance` bereitstellen wollen, geben Sie die Anzahl von E/A-Operationen pro Sekunde (IOPS) pro Gigabyte ein. Wenn Sie zum Beispiel Blockspeicher wie in der Speicherklasse `ibmc-block-bronze` definiert bereitstellen wollen, geben Sie 2 ein. Weitere Informationen finden Sie im Abschnitt zum [Festlegen der Blockspeicherkonfiguration](/docs/containers?topic=containers-block_storage#block_predefined_storageclass). Wenn Sie Speicher vom Typ `performance` bereitstellen wollen, entfernen Sie diesen Abschnitt oder setzen ihn auf Kommentar, indem Sie das Zeichen `#` am Anfang jeder Zeile hinzufügen. </td>
   </tr>
   <tr>
   <td><code>size</code></td>
   <td>Geben Sie die Größe Ihres Speichers in Gigabyte ein. Im Abschnitt zum [Festlegen der Blockspeicherkonfiguration](/docs/containers?topic=containers-block_storage#block_predefined_storageclass) finden Sie Informationen zu unterstützten Größen für Ihr Speichertier. </td>
   </tr>
   </tbody>
   </table>  

5. Rufen Sie den Benutzernamen und den API-Schlüssel für Ihre IBM Cloud-Infrastruktur (SoftLayer) ab. Der Benutzername und der API-Schlüssel werden vom Script `mkpvyaml` für den Zugriff auf den Cluster verwendet.
   1. Melden Sie sich bei der [{{site.data.keyword.Bluemix_notm}}-Konsole ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/) an.
   2. Wählen Sie im Menü ![Menüsymbol](../icons/icon_hamburger.svg "Menüsymbol") die Option **Infrastruktur** aus.
   3. Wählen Sie in der Menüleiste **Konto** > **Benutzer** > **Benutzerliste** aus.
   4. Suchen Sie den Benutzer, dessen Benutzernamen und API-Schlüssel Sie abrufen wollen.
   5. Klicken Sie auf **Generieren**, um einen API-Schlüssel zu generieren, oder auf **Anzeigen**, um Ihren vorhandenen API-Schlüssel anzuzeigen. Es wird ein Popup-Fenster geöffnet, das den Benutzernamen und den API-Schlüssel für die Infrastruktur anzeigt.

6. Speichern Sie die Berechtigungsnachweise in einer Umgebungsvariablen.
   1. Fügen Sie Umgebungsvariablen hinzu.
      ```
      export SL_USERNAME=<benutzername_der_infrastruktur>
      ```
      {: pre}

      ```
      export SL_API_KEY=<api-schlüssel_der_infrastruktur>
      ```
      {: pre}

   2. Überprüfen Sie, ob die Umgebungsvariablen erfolgreich erstellt wurden.
      ```
      printenv
      ```
      {: pre}

7.  Erstellen Sie den Container `mkpvyaml` und führen Sie ihn aus. Wenn Sie den Container aus dem Image ausführen, wird das Script `mkpvyaml.py` ausgeführt. Das Script fügt jedem Workerknoten im Cluster eine Blockspeichereinheit hinzu und berechtigt jeden Workerknoten zum Zugriff auf die Blockspeichereinheit. Am Ende des Scripts wird eine YAML-Datei mit dem Namen `pv-<cluster_name>.yaml` für Sie generiert, die Sie später zum Erstellen persistenter Datenträger im Cluster verwenden können.
    1.  Erstellen Sie den Container `mkpvyaml` (Build).
        ```
        docker build -t mkpvyaml .
        ```
        {: pre}
        Beispielausgabe:
        ```
        Sending build context to Docker daemon   29.7kB
        Step 1/16 : FROM ubuntu:18.10
        18.10: Pulling from library/ubuntu
        5940862bcfcd: Pull complete
        a496d03c4a24: Pull complete
        5d5e0ccd5d0c: Pull complete
        ba24b170ddf1: Pull complete
        Digest: sha256:20b5d52b03712e2ba8819eb53be07612c67bb87560f121cc195af27208da10e0
        Status: Downloaded newer image for ubuntu:18.10
         ---> 0bfd76efee03
        Step 2/16 : RUN apt-get update
         ---> Running in 85cedae315ce
        Get:1 http://security.ubuntu.com/ubuntu cosmic-security InRelease [83.2 kB]
        Get:2 http://archive.ubuntu.com/ubuntu cosmic InRelease [242 kB]
        ...
        Step 16/16 : ENTRYPOINT [ "/docker-entrypoint.sh" ]
         ---> Running in 9a6842f3dbe3
        Removing intermediate container 9a6842f3dbe3
         ---> 7926f5384fc7
        Successfully built 7926f5384fc7
        Successfully tagged mkpvyaml:latest
        ```
        {: screen}
    2.  Führen Sie den Container aus, um das Script `mkpvyaml.py` auszuführen.
        ```
        docker run --rm -v `pwd`:/data -v ~/.bluemix:/config -e SL_API_KEY=$SL_API_KEY -e SL_USERNAME=$SL_USERNAME mkpvyaml
        ```
        {: pre}

        Beispielausgabe:
        ```
        Unable to find image 'portworx/iks-mkpvyaml:latest' locally
        latest: Pulling from portworx/iks-mkpvyaml
        72f45ff89b78: Already exists
        9f034a33b165: Already exists
        386fee7ab4d3: Already exists
        f941b4ac6aa8: Already exists
        fe93e194fcda: Pull complete
        f29a13da1c0a: Pull complete
        41d6e46c1515: Pull complete
        e89af7a21257: Pull complete
        b8a7a212d72e: Pull complete
        5e07391a6f39: Pull complete
        51539879626c: Pull complete
        cdbc4e813dcb: Pull complete
        6cc28f4142cf: Pull complete
        45bbaad87b7c: Pull complete
        05b0c8595749: Pull complete
        Digest: sha256:43ac58a8e951994e65f89ed997c173ede1fa26fb41e5e764edfd3fc12daf0120
        Status: Downloaded newer image for portworx/iks-mkpvyaml:latest

        kube-dal10-abc1d23e123456ac8b12a3c1e12b123a4 10.XXX.XX.XX
        Creating Vol of size 20 with type:  endurance
                  Ordering block storage of size: 20 for host: kube-dal10-abc1d23e123456ac8b12a3c1e12b123a4
                  ORDER ID =  30087291
        kube-dal10-abc1d23e123456ac8b12a3c1e12b123a4 10.XXX.XX.XX
        Creating Vol of size 20 with type:  endurance
                  Ordering block storage of size: 20 for host: kube-dal10-abc1d23e123456ac8b12a3c1e12b123a4
                  ORDER ID =  30087293
        kube-dal12-abc1d23e123456ac8b12a3c1e12b456v1 10.XXX.XX.XX
        Creating Vol of size 20 with type:  endurance
                  Ordering block storage of size: 20 for host: kube-dal12-abc1d23e123456ac8b12a3c1e12b456v1
                  ORDER ID =  30085655
                  No volume yet ... for orderID :  30087291
                  No volume yet ... for orderID :  30087291
                  No volume yet ... for orderID :  30087291
                  No volume yet ... for orderID :  30087291
                  Order ID =  30087291 has created VolId =  12345678
                  Granting access to volume: 12345678 for HostIP: 10.XXX.XX.XX
                  Order ID =  30087293 has created VolId =  87654321
                  Granting access to volume: 87654321 for HostIP: 10.XXX.XX.XX
                  Vol 53002831 is not yet ready ...
                  Granting access to volume: 87654321 for HostIP: 10.XXX.XX.XX
                  Order ID =  30085655 has created VolId =  98712345
                  Granting access to volume: 98712345 for HostIP: 10.XXX.XX.XX
        Output file created as :  pv-<clustername>.yaml
        ```
        {: screen}

7. [Ordnen Sie die Blockspeichereinheiten Ihren Workerknoten zu](#attach_block).

## Blockspeicher bestimmten Workerknoten manuell hinzufügen
{: #manual_block}

Verwenden Sie diese Option, wenn Sie unterschiedliche Blockspeicherkonfigurationen hinzufügen, Blockspeicher nur einer Untergruppe von Workerknoten hinzufügen oder mehr Kontrolle über den Bereitstellungsprozess haben möchten.
{: shortdesc}

1. Listen Sie die Workerknoten in Ihrem Cluster auf und notieren Sie die private IP-Adresse und die Zone der Nicht-SDS-Workerknoten, denen Sie eine Blockspeichereinheit hinzufügen wollen.
   ```
   ibmcloud ks workers --cluster <clustername_oder_-id>
   ```
   {: pre}

2. Prüfen Sie Schritt 3 und 4 im Abschnitt zum [Festlegen der Blockspeicherkonfiguration](/docs/containers?topic=containers-block_storage#block_predefined_storageclass), um den Typ, die Größe und die Anzahl E/A-Operationen pro Sekunde (IOPS) für die Blockspeichereinheit auszuwählen, die Sie Ihrem Nicht-SDS-Workerknoten hinzufügen wollen.    

3. Erstellen Sie die Blockspeichereinheit in derselben Zone, in der sich auch Ihr Nicht-SDS-Workerknoten befindet.

   **Beispiel für die Bereitstellung eines 20 GB großen Blockspeichers vom Typ 'endurance' mit 2 IOPS pro GB:**
   ```
   ibmcloud sl block volume-order --storage-type endurance --size 20 --tier 2 --os-type LINUX --datacenter dal10
   ```
   {: pre}

   **Beispiel für die Bereitstellung von 20 GB Blockspeicher vom Typ 'performance' mit 100 IOPS:**
   ```
   ibmcloud sl block volume-order --storage-type performance --size 20 --iops 100 --os-type LINUX --datacenter dal10
   ```
   {: pre}

4. Überprüfen Sie, ob die Blockspeichereinheit erstellt wurde, und notieren Sie die ID (**`id`**) des Datenträgers. **Hinweis:** Wenn Ihre Blockspeichereinheit nicht sofort angezeigt wird, warten Sie einige Minuten. Führen Sie dann diesen Befehl erneut aus.
   ```
   ibmcloud sl block volume-list
   ```
   {: pre}

   Beispielausgabe:
   ```
   id         username          datacenter   storage_type                capacity_gb   bytes_used   ip_addr         lunId   active_transactions
   123456789  IBM02SL1234567-8  dal10        performance_block_storage   20            -            161.12.34.123   0       0   
   ```
   {: screen}

5. Prüfen Sie die Details für Ihren Datenträger und notieren Sie die Werte für **`Target IP`** und **`LUN Id`**.
   ```
   ibmcloud sl block volume-detail <datenträger-id>
   ```
   {: pre}

   Beispielausgabe:
   ```
   Name                       Value
   ID                         1234567890
   User name                  IBM123A4567890-1
   Type                       performance_block_storage
   Capacity (GB)              20
   LUN Id                     0
   IOPs                       100
   Datacenter                 dal10
   Target IP                  161.12.34.123
   # of Active Transactions   0
   Replicant Count            0
   ```
   {: screen}

6. Berechtigen Sie den Nicht-SDS-Workerknoten zum Zugriff auf die Blockspeichereinheit. Ersetzen Sie `<datenträger-id>` durch die Datenträger-ID Ihrer Blockspeichereinheit, die Sie zuvor abgerufen haben, und `<private_worker-ip>` durch die private IP-Adresse des Nicht-SDS-Workerknotens, dem Sie die Einheit zuordnen wollen.

   ```
   ibmcloud sl block access-authorize <datenträger-id> -p <private_worker-ip>
   ```
   {: pre}

   Beispielausgabe:
   ```
   The IP address 123456789 was authorized to access <datenträger-id>.
   ```
   {: screen}

7. Überprüfen Sie, ob Ihr Nicht-SDS-Workerknoten erfolgreich berechtigt wurde, und notieren Sie die Werte für **`host_iqn`**, **`username`** und **`password`**.
   ```
   ibmcloud sl block access-list <datenträger-id>
   ```
   {: pre}

   Beispielausgabe:
   ```
   ID          name                 type   private_ip_address   source_subnet   host_iqn                                      username   password           allowed_host_id
   123456789   <private_worker-ip>  IP     <private_worker-ip>  -               iqn.2018-09.com.ibm:ibm02su1543159-i106288771   IBM02SU1543159-I106288771   R6lqLBj9al6e2lbp   1146581   
   ```
   {: screen}

   Die Berechtigung war erfolgreich, wenn Werte für **`host_iqn`**, **`username`** und **`password`** zugewiesen wurden.

8. [Ordnen Sie die Blockspeichereinheiten Ihren Workerknoten zu](#attach_block).


## Unaufbereiteten Blockspeicher Nicht-SDS-Workerknoten zuordnen
{: #attach_block}

Für die Zuordnung der Blockspeichereinheit zu einem Nicht-SDS-Workerknoten müssen Sie einen persistenten Datenträger (PV - Persistent Volume) mit der {{site.data.keyword.Bluemix_notm}} Block Volume Attacher-Speicherklasse und den Details Ihrer Blockspeichereinheit erstellen.
{: shortdesc}

**Vorbereitende Schritte**:
- Stellen Sie sicher, dass unaufbereiteten, unformatiert und nicht angehängten Blockspeicher für Ihre Nicht-SDS-Workerknoten [automatisch](#automatic_block) oder [manuell](#manual_block) erstellt haben.
- [Melden Sie sich an Ihrem Konto an. Geben Sie als Ziel die entsprechende Region und, sofern zutreffend, die Ressourcengruppe an. Legen Sie den Kontext für den Cluster fest.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

**Gehen Sie wie folgt vor, unaufbereiteten Blockspeicher Nicht-SDS-Workerknoten zuzuordnen:**
1. Bereiten Sie die Erstellung des persistenten Datenträgers (PV) vor.  
   - **Wenn Sie den Container `mkpvyaml` verwendet haben:**
     1. Öffnen Sie die Datei `pv-<clustername>.yaml`.
        ```
        nano pv-<clustername>.yaml
        ```
        {: pre}

     2. Überprüfen Sie die Konfiguration für Ihre PVs.

   - **Wenn Sie Blockspeicher manuell hinzugefügt haben:**
     1. Erstellen Sie eine Datei `pv.yaml`. Der folgende Befehl erstellt die Datei mit dem Editor `nano`.
        ```
        nano pv.yaml
        ```
        {: pre}

     2. Fügen Sie die Details Ihrer Blockspeichereinheit dem PV hinzu.
        ```
        apiVersion: v1
        kind: PersistentVolume
        metadata:
          name: <pv-name>
          annotations:
            ibm.io/iqn: "<iqn-hostname>"
            ibm.io/username: "<benutzername>"
            ibm.io/password: "<kennwort>"
            ibm.io/targetip: "<ziel-ip>"
            ibm.io/lunid: "<lun-id>"
            ibm.io/nodeip: "<private_worker-ip>"
            ibm.io/volID: "<datenträger-id>"
        spec:
          capacity:
            storage: <größe>
          accessModes:
            - ReadWriteOnce
          hostPath:
              path: /
          storageClassName: ibmc-block-attacher
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
      	<td>Geben Sie einen Namen für Ihren persistenten Datenträger (PV) ein.</td>
      	</tr>
        <tr>
        <td><code>ibm.io/iqn</code></td>
        <td>Geben Sie den IQN-Hostnamen ein, den Sie zuvor abgerufen haben. </td>
        </tr>
        <tr>
        <td><code>ibm.io/username</code></td>
        <td>Geben Sie den Benutzernamen für die IBM Cloud-Infrastruktur (SoftLayer) ein, den Sie zuvor abgerufen haben. </td>
        </tr>
        <tr>
        <td><code>ibm.io/password</code></td>
        <td>Geben Sie das Kennwort für die IBM Cloud-Infrastruktur (SoftLayer) ein, das Sie zuvor abgerufen haben. </td>
        </tr>
        <tr>
        <td><code>ibm.io/targetip</code></td>
        <td>Geben Sie die Ziel-ID ein, die Sie zuvor abgerufen haben. </td>
        </tr>
        <tr>
        <td><code>ibm.io/lunid</code></td>
        <td>Geben Sie die LUN-ID Ihrer Blockspeichereinheit ein, die Sie zuvor abgerufen haben. </td>
        </tr>
        <tr>
        <td><code>ibm.io/nodeip</code></td>
        <td>Geben Sie die private IP-Adresse des Workerknotens ein, dem Sie die Blockspeichereinheit zuordnen wollen und den Sie zuvor zum Zugriff auf Ihre Blockspeichereinheit berechtigt haben. </td>
        </tr>
        <tr>
          <td><code>ibm.io/volID</code></td>
        <td>Geben Sie die ID des Blockspeicherdatenträgers ein, die Sie zuvor abgerufen haben. </td>
        </tr>
        <tr>
        <td><code>storage</code></td>
        <td>Geben Sie die Größe der Blockspeichereinheit ein, die Sie zuvor erstellt haben. Beispiel: Wenn Ihre Blockspeichereinheit 20 Gigabyte groß ist, geben Sie <code>20Gi</code> ein.  </td>
        </tr>
        </tbody>
        </table>
2. Erstellen Sie den persistenten Datenträger (PV), um die Blockspeichereinheit Ihrem Nicht-SDS-Workerknoten zuzuordnen.
   - **Wenn Sie den Container `mkpvyaml` verwendet haben:**
     ```
     kubectl apply -f pv-<clustername>.yaml
     ```
     {: pre}

   - **Wenn Sie Blockspeicher manuell hinzugefügt haben:**
     ```
     kubectl apply -f  block-volume-attacher/pv.yaml
     ```
     {: pre}

3. Überprüfen Sie, ob der Blockspeicher Ihren Workerknoten erfolgreich zugeordnet wurde.
   ```
   kubectl describe pv <pv-name>
   ```
   {: pre}

   Beispielausgabe:
   ```
   Name:            kube-wdc07-cr398f790bc285496dbeb8e9137bc6409a-w1-pv1
   Labels:          <none>
   Annotations:     ibm.io/attachstatus=attached
                    ibm.io/dm=/dev/dm-1
                    ibm.io/iqn=iqn.2018-09.com.ibm:ibm02su1543159-i106288771
                    ibm.io/lunid=0
                    ibm.io/mpath=3600a09803830445455244c4a38754c66
                    ibm.io/nodeip=10.176.48.67
                    ibm.io/password=R6lqLBj9al6e2lbp
                    ibm.io/targetip=161.26.98.114
                    ibm.io/username=IBM02SU1543159-I106288771
                    kubectl.kubernetes.io/last-applied-configuration={"apiVersion":"v1","kind":"PersistentVolume","metadata":{"annotations":{"ibm.io/iqn":"iqn.2018-09.com.ibm:ibm02su1543159-i106288771","ibm.io/lunid":"0"...
   Finalizers:      []
   StorageClass:    ibmc-block-attacher
   Status:          Available
   Claim:
   Reclaim Policy:  Retain
   Access Modes:    RWO
   Capacity:        20Gi
   Node Affinity:   <none>
   Message:
   Source:
       Type:          HostPath (bare host directory volume)
       Path:          /
       HostPathType:
   Events:            <none>
   ```
   {: screen}

   Die Blockspeichereinheit wurde erfolgreich zugeordnet, wenn **ibm.io/dm** auf eine Geräte-ID gesetzt wurde, wie zum Beispiel `/dev/dm/1`, und im Abschnitt **Annotations** Ihrer CLI-Ausgabe die Angabe **ibm.io/attachstatus=attached** angezeigt wird.

Wenn Sie die Zuordnung eines Datenträgers aufheben wollen, löschen Sie den PV. Es ist weiterhin ein bestimmter Workerknoten berechtigt, auf solche freigegebene Datenträger zuzugreifen. Freigegebene Datenträger werden erneut zugeordnet, wenn Sie einen neuen PV mit der {{site.data.keyword.Bluemix_notm}} Block Volume Attacher-Speicherklasse erstellen, um einen anderen Datenträger demselben Workerknoten zuzuordnen. Zur Vermeidung einer erneuten Zuordnung des alten, freigegebenen Datenträgers entziehen Sie dem Workerknoten die Berechtigung zum Zugriff auf den freigegebenen Datenträger, indem Sie den Befehl `ibmcloud sl block access-revoke` verwenden. Durch die Freigabe des Datenträgers wird der Datenträger selbst nicht aus Ihrem IBM Cloud-Infrastrukturkonto (SoftLayer) entfernt. Zur Stornierung der Rechnungsstellung für Ihren Datenträger müssen Sie [den Speicher manuell aus Ihrem IBM Cloud-Infrastrukturkonto (SoftLayer) entfernen](/docs/containers?topic=containers-cleanup).
{: note}
