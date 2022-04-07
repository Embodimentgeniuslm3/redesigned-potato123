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


# Daten in IBM Blockspeicher für IBM Cloud speichern
{: #block_storage}

{{site.data.keyword.Bluemix_notm}}-Blockspeicher (Block Storage) ist ein persistenter, hochleistungsfähiger iSCSI-Speicher, den Sie Ihren Apps durch persistente Kubernetes-Datenträger (PVs) hinzufügen können. Sie können unter vordefinierten Speichertiers mit GB-Größen und E/A-Operationen pro Sekunde (IOPS) wählen, die die Anforderungen Ihrer Workloads erfüllen. Informationen zur Ermittlung, ob {{site.data.keyword.Bluemix_notm}}-Blockspeicher die richtige Speicheroption für Sie ist, finden Sie unter [Speicherlösung wählen](/docs/containers?topic=containers-storage_planning#choose_storage_solution). Preisinformationen finden Sie unter [Abrechnung](/docs/infrastructure/BlockStorage?topic=BlockStorage-About#billing).
{: shortdesc}

{{site.data.keyword.Bluemix_notm}}-Blockspeicher ist nur für Standardcluster verfügbar. Wenn Ihr Cluster auf das öffentliche Netz nicht zugreifen kann, wie dies zum Beispiel bei einem privaten Cluster hinter einer Firewall oder bei einem Cluster mit nur einem aktivierten privaten Serviceendpunkt der Fall ist, stellen Sie sicher, dass Sie das {{site.data.keyword.Bluemix_notm}} Block Storage-Plug-in Version 1.3.0 oder höher installiert haben, um eine Verbindung zu Ihrer Blockspeicherinstanz über das private Netz herzustellen. Blockspeicherinstanzen sind für eine einzelne Zone spezifisch. Wenn Sie einen Mehrzonencluster haben, ziehen Sie [Optionen für persistenten Speicher in mehreren Zonen](/docs/containers?topic=containers-storage_planning#persistent_storage_overview) in Betracht.
{: important}

## Plug-in für {{site.data.keyword.Bluemix_notm}}-Blockspeicher im Cluster installieren
{: #install_block}

Installieren Sie das {{site.data.keyword.Bluemix_notm}}-Blockspeicher-Plug-in mit einem Helm-Diagramm, um vordefinierte Speicherklassen für den Blockspeicher einzurichten. Mit diesen Speicherklassen können Sie einen PVC zum Bereitstellen von Blockspeicher für Ihre Apps erstellen.
{: shortdesc}

Vorbereitende Schritte: [Melden Sie sich an Ihrem Konto an. Geben Sie als Ziel die entsprechende Region und, sofern zutreffend, die Ressourcengruppe an. Legen Sie den Kontext für den Cluster fest.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1. Stellen Sie sicher, dass auf dem Workerknoten der neueste Patch für die Nebenversion angewendet wird.
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

2.  [Befolgen Sie die Anweisungen](/docs/containers?topic=containers-helm#public_helm_install) zum Installieren des Helm-Clients auf Ihrer lokalen Maschine und installieren Sie den Helm-Server (Tiller) mit einem Servicekonto in Ihrem Cluster.

    Die Installation des Helm-Servers Tiller erfordert eine öffentliche Netzverbindung zur öffentlichen Google-Container-Registry. Wenn Ihr Cluster auf das öffentliche Netz nicht zugreifen kann, wie dies zum Beispiel bei einem privaten Cluster hinter einer Firewall oder bei einem Cluster mit nur einem aktivierten privaten Serviceendpunkt der Fall ist, haben Sie die Wahl, [">das Tiller-Image auf Ihre lokale Maschine zu extrahieren und durch eine Push-Operation in Ihren Namensbereich in {{site.data.keyword.registryshort_notm}}](/docs/containers?topic=containers-helm#private_local_tiller) zu übertragen oder [das Helm-Diagramm ohne Verwendung von Tiller zu installieren](/docs/containers?topic=containers-helm#private_install_without_tiller).
    {: note}

3.  Überprüfen Sie, ob Tiller mit einem Servicekonto installiert ist.

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

4. Fügen Sie das {{site.data.keyword.Bluemix_notm}}-Helm-Diagramm-Repository zu dem Cluster hinzu, in dem Sie das {{site.data.keyword.Bluemix_notm}}-Blockspeicher-Plug-in verwenden möchten.
   ```
   helm repo add iks-charts https://icr.io/helm/iks-charts
   ```
   {: pre}

5. Aktualisieren Sie das Helm-Repository, um die aktuelle Version aller Helm-Diagramme in diesem Repository abzurufen.
   ```
   helm repo update
   ```
   {: pre}

6. Installieren Sie das {{site.data.keyword.Bluemix_notm}}-Blockspeicher-Plug-in. Wenn Sie das Plug-in installieren, werden vordefinierte Blockspeicherklassen zu Ihrem Cluster hinzugefügt.
   ```
   helm install iks-charts/ibmcloud-block-storage-plugin 
   ```
   {: pre}

   Beispielausgabe:
   ```
   NAME:   bald-olm
   LAST DEPLOYED: Wed Apr 18 10:02:55 2018
   NAMESPACE: default
   STATUS: DEPLOYED

   RESOURCES:
   ==> v1beta1/DaemonSet
   NAME                           DESIRED  CURRENT  READY  UP-TO-DATE  AVAILABLE  NODE SELECTOR  AGE
   ibmcloud-block-storage-driver  0        0        0      0           0          <none>         0s

   ==> v1beta1/Deployment
   NAME                           DESIRED  CURRENT  UP-TO-DATE  AVAILABLE  AGE
   ibmcloud-block-storage-plugin  1        0        0           0          0s

   ==> v1/StorageClass
   NAME                      PROVISIONER        AGE
   ibmc-block-bronze         ibm.io/ibmc-block  0s
   ibmc-block-custom         ibm.io/ibmc-block  0s
   ibmc-block-gold           ibm.io/ibmc-block  0s
   ibmc-block-retain-bronze  ibm.io/ibmc-block  0s
   ibmc-block-retain-custom  ibm.io/ibmc-block  0s
   ibmc-block-retain-gold    ibm.io/ibmc-block  0s
   ibmc-block-retain-silver  ibm.io/ibmc-block  0s
   ibmc-block-silver         ibm.io/ibmc-block  0s

   ==> v1/ServiceAccount
   NAME                           SECRETS  AGE
   ibmcloud-block-storage-plugin  1        0s

   ==> v1beta1/ClusterRole
   NAME                           AGE
   ibmcloud-block-storage-plugin  0s

   ==> v1beta1/ClusterRoleBinding
   NAME                           AGE
   ibmcloud-block-storage-plugin  0s

   NOTES:
   Thank you for installing: ibmcloud-block-storage-plugin.   Your release is named: bald-olm
   ```
   {: screen}

7. Überprüfen Sie, dass die Installation erfolgreich war.
   ```
   kubectl get pod -n kube-system | grep block
   ```
   {: pre}

   Beispielausgabe:
   ```
   ibmcloud-block-storage-driver-kh4mt                              1/1       Running   0          27d       10.118.98.19   10.118.98.19
   ibmcloud-block-storage-plugin-58c5f9dc86-pbl4t                   1/1       Running   0          14d       172.21.0.204   10.118.98.19
   ```
   {: screen}

   Die Installation ist erfolgreich, wenn ein Pod des Typs `ibmcloud-block-storage-plugin` und ein oder mehrere Pods des Typs `ibmcloud-block-storage-driver` angezeigt werden. Die Anzahl der Pods des Typs `ibmcloud-block-storage-driver` entspricht der Anzahl der Workerknoten in Ihrem Cluster. Alle Pods müssen den Status **Aktiv** aufweisen.

8. Überprüfen Sie, dass die Speicherklassen für Blockspeicher zu Ihrem Cluster hinzugefügt wurden.
   ```
   kubectl get storageclasses | grep block
   ```
   {: pre}

   Beispielausgabe:
   ```
   ibmc-block-bronze            ibm.io/ibmc-block
   ibmc-block-custom            ibm.io/ibmc-block
   ibmc-block-gold              ibm.io/ibmc-block
   ibmc-block-retain-bronze     ibm.io/ibmc-block
   ibmc-block-retain-custom     ibm.io/ibmc-block
   ibmc-block-retain-gold       ibm.io/ibmc-block
   ibmc-block-retain-silver     ibm.io/ibmc-block
   ibmc-block-silver            ibm.io/ibmc-block
   ```
   {: screen}

9. Wiederholen Sie diese Schritte für jeden Cluster, in dem Sie Blockspeicher bereitstellen möchten.

Sie können jetzt mit dem [Erstellen eines PVC](#add_block) zum Bereitstellen von Blockspeicher für Ihre App fortfahren.


### {{site.data.keyword.Bluemix_notm}}-Blockspeicher-Plug-in aktualisieren
Sie können ein Upgrade des vorhandenen {{site.data.keyword.Bluemix_notm}}-Blockspeicher-Plug-ins auf die aktuelle Version durchführen.
{: shortdesc}

Vorbereitende Schritte: [Melden Sie sich an Ihrem Konto an. Geben Sie als Ziel die entsprechende Region und, sofern zutreffend, die Ressourcengruppe an. Legen Sie den Kontext für den Cluster fest.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

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

3. Suchen Sie den Namen des Blockspeicher-Helm-Diagramms, das Sie in Ihrem Cluster installiert haben.
   ```
   helm ls | grep ibmcloud-block-storage-plugin
   ```
   {: pre}

   Beispielausgabe:
   ```
   myhelmchart 	1       	Mon Sep 18 15:31:40 2017	DEPLOYED	ibmcloud-block-storage-plugin-0.1.0	default
   ```
   {: screen}

4. Führen Sie ein Upgrade des {{site.data.keyword.Bluemix_notm}}-Blockspeicher-Plug-ins auf die aktuelle Version durch.
   ```
   helm upgrade --force --recreate-pods <helm_chart_name>  iks-charts/ibmcloud-block-storage-plugin
   ```
   {: pre}

5. Optional: Wenn Sie das Plug-in aktualisieren, wird die Festlegung auf die Speicherklasse `standard` aufgehoben. Wenn Sie statt der Speicherklasse 'standard' eine Speicherklasse Ihrer Wahl festlegen möchten, führen Sie den folgenden Befehl aus.
   ```
   kubectl patch storageclass <storageclass> -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'
   ```
   {: pre}


### Plug-in für {{site.data.keyword.Bluemix_notm}}-Blockspeicher entfernen
Wenn Sie in Ihrem Cluster {{site.data.keyword.Bluemix_notm}}-Blockspeicher nicht einrichten und verwenden möchten, können Sie das Helm-Diagramm deinstallieren.
{: shortdesc}

Durch das Entfernen des Plug-ins werden keine vorhandenen PVCs, PVs oder Daten entfernt. Wenn Sie das Plug-in entfernen, werden alle zugehörigen Pods und Dämongruppen aus Ihrem Cluster entfernt. Nachdem Sie das Plug-in entfernt haben, können Sie keinen neuen Blockspeicher für Ihren Cluster einrichten oder vorhandene Blockspeicher-PVCs und -PVs verwenden.
{: important}

Vorbereitende Schritte:
- [Melden Sie sich an Ihrem Konto an. Geben Sie als Ziel die entsprechende Region und, sofern zutreffend, die Ressourcengruppe an. Legen Sie den Kontext für den Cluster fest.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
- Stellen Sie sicher, dass in Ihrem Cluster keine PVCs oder persistenten Datenträger vorhanden sind, die Blockspeicher verwenden.

Gehen Sie wie folgt vor, um das Plug-in zu entfernen:

1. Suchen Sie den Namen des Blockspeicher-Helm-Diagramms, das Sie in Ihrem Cluster installiert haben.
   ```
   helm ls | grep ibmcloud-block-storage-plugin
   ```
   {: pre}

   Beispielausgabe:
   ```
   myhelmchart 	1       	Mon Sep 18 15:31:40 2017	DEPLOYED	ibmcloud-block-storage-plugin-0.1.0	default
   ```
   {: screen}

2. Löschen Sie das {{site.data.keyword.Bluemix_notm}}-Blockspeicher-Plug-in.
   ```
   helm delete <name_des_helm-diagramms>
   ```
   {: pre}

3. Überprüfen Sie, dass die Blockspeicher-Pods entfernt wurden.
   ```
   kubectl get pod -n kube-system | grep ibmcloud-block-storage-plugin
   ```
   {: pre}
   Das Entfernen der Pods war erfolgreich, wenn in Ihrer CLI-Ausgabe keine Pods angezeigt werden.

4. Überprüfen Sie, dass die Blockspeicherklassen entfernt wurden.
   ```
   kubectl get storageclasses | grep block
   ```
   {: pre}
   Das Entfernen der Speicherklassen war erfolgreich, wenn in Ihrer CLI-Ausgabe keine Speicherklassen angezeigt werden.

<br />



## Blockspeicherkonfiguration festlegen
{: #block_predefined_storageclass}

{{site.data.keyword.containerlong}} stellt vordefinierte Speicherklassen für Blockspeicher zur Verfügung, die Sie verwenden können, um Blockspeicher mit einer bestimmten Konfiguration bereitzustellen.
{: shortdesc}

Jede Speicherklasse gibt den Typ des Blockspeichers an, den Sie bereitstellen, einschließlich der verfügbaren Größe, der E/A-Operationen pro Sekunde, des Dateisystems und der Aufbewahrungsrichtlinie.  

Stellen Sie sicher, dass die Speicherkonfiguration sorgfältig ausgewählt ist, damit genügend Kapazität zum Speichern Ihrer Daten vorhanden ist. Nachdem Sie mithilfe einer Speicherklasse einen bestimmten Typ von Speicher bereitgestellt haben, können Sie die Größe, den Typ, die E/A-Operationen pro Sekunde oder die Aufbewahrungsrichtlinie für die Speichereinheit nicht mehr ändern. Wenn Sie mehr Speicher oder Speicher mit einer anderen Konfiguration benötigen, müssen Sie [eine neue Speicherinstanz erstellen und die Daten](/docs/containers?topic=containers-kube_concepts#update_storageclass) aus der alten Speicherinstanz in die neue kopieren.
{: important}

1. Listen Sie die in {{site.data.keyword.containerlong}} verfügbaren Speicherklassen auf.
    ```
    kubectl get storageclasses | grep block
    ```
    {: pre}

    Beispielausgabe:
    ```
    $ kubectl get storageclasses
    NAME                         TYPE
    ibmc-block-custom            ibm.io/ibmc-block
    ibmc-block-bronze            ibm.io/ibmc-block
    ibmc-block-gold              ibm.io/ibmc-block
    ibmc-block-silver            ibm.io/ibmc-block
    ibmc-block-retain-bronze     ibm.io/ibmc-block
    ibmc-block-retain-silver     ibm.io/ibmc-block
    ibmc-block-retain-gold       ibm.io/ibmc-block
    ibmc-block-retain-custom     ibm.io/ibmc-block
    ```
    {: screen}

2. Überprüfen Sie die Konfiguration einer Speicherklasse.
   ```
   kubectl describe storageclass <name_der_speicherklasse>
   ```
   {: pre}

   Weitere Informationen zu den einzelnen Speicherklassen finden Sie in der [Speicherklassenreferenz](#block_storageclass_reference). Wenn Sie nichts Entsprechendes finden, können Sie eine eigene angepasste Speicherklasse erstellen. Prüfen Sie zunächst die [Beispiele für angepasste Speicherklassen](#block_custom_storageclass).
   {: tip}

3. Wählen Sie den Typ des Blockspeichers aus, den Sie bereitstellen möchten.
   - **Speicherklassen 'bronze', 'silver' und 'gold':** Diese Speicherklassen stellen [Endurance-Speicher](/docs/infrastructure/BlockStorage?topic=BlockStorage-About#provendurance) bereit. Mit dem Endurance-Speicher können Sie in vordefinierten Tiers für E/A-Operationen pro Sekunde die Größe des Speichers in Gigabyte auswählen.
   - **Speicherklasse 'custom':** Diese Speicherklasse stellt [Leistungsspeicher](/docs/infrastructure/BlockStorage?topic=BlockStorage-About#provperformance) bereit. Mit dem Leistungsspeicher können Sie die Größe des Speichers und der E/A-Operationen pro Sekunde besser steuern.

4. Wählen Sie für Ihren Blockspeicher die Größe und die Anzahl E/A-Operationen pro Sekunde aus. Die Größe und die Anzahl der E/A-Operationen pro Sekunde definieren die Gesamtzahl der E/A-Operationen pro Sekunde, die als Indikator für die Geschwindigkeit des Speichers dient. Je höher die Anzahl der E/A-Operationen pro Sekunde für Ihren Speicher ist, desto höher ist dessen Verarbeitungsgeschwindigkeit für Lese- und Schreiboperationen.
   - **Speicherklassen 'bronze', 'silver' und 'gold':** Diese Speicherklassen haben eine feste Anzahl von pro Gigabyte angegebenen E/A-Operationen pro Sekunde und werden auf SSD-Festplatten bereitgestellt. Die Gesamtzahl der E/A-Operationen pro Sekunde hängt von der Größe des von Ihnen ausgewählten Speichers ab. Sie können innerhalb des zulässigen Größenbereichs eine beliebige ganze Zahl von Gigabyte auswählen, z. B. 20, 256 oder 11854 Gigabyte. Um die Gesamtzahl der E/A-Operationen pro Sekunde zu ermitteln, müssen Sie die E/A-Operationen pro Sekunde mit der ausgewählten Größe multiplizieren. Wenn Sie beispielsweise in der Speicherklasse 'silver', die 4 E/A-Operationen pro Sekunde aufweist, als Größe des Blockspeichers 1000 Gigabyte (1000Gi) auswählen, hat Ihr Speicher eine Gesamtzahl von 4000 E/A-Operationen pro Sekunde.  
     <table>
         <caption>Tabelle der Größenbereiche der Speicherklasse und der IOPS pro Gigabyte</caption>
         <thead>
         <th>Speicherklasse</th>
         <th>IOPS pro Gigabyte</th>
         <th>Größenbereich in Gigabyte</th>
         </thead>
         <tbody>
         <tr>
         <td>Bronze</td>
         <td>2 IOPS/GB</td>
         <td>20-12000 Gi</td>
         </tr>
         <tr>
         <td>Silver</td>
         <td>4 IOPS/GB</td>
         <td>20-12000 Gi</td>
         </tr>
         <tr>
         <td>Gold</td>
         <td>10 IOPS/GB</td>
         <td>20-4000 Gi</td>
         </tr>
         </tbody></table>
   - **Speicherklasse 'custom':** Wenn Sie diese Speicherklasse auswählen, können Sie die gewünschte Größe und Anzahl der E/A-Operationen pro Sekunde besser steuern. Als Größe können Sie innerhalb des zulässigen Größenbereichs eine beliebige ganze Zahl von Gigabyte auswählen. Die von Ihnen gewählte Größe legt den Bereich der für Sie verfügbaren E/A-Operationen pro Sekunde fest. Sie können eine Anzahl von E/A-Operationen pro Sekunde auswählen, bei der es sich um eine Vielzahl von 100 handelt und die sich im angegebenen Bereich befindet. Der von Ihnen ausgewählte Wert für E/A-Operationen pro Sekunde ist statisch und wird nicht mit der Größe des Speichers skaliert. Wenn Sie beispielsweise 40 Gigabyte (40Gi) mit 100 E/A-Operationen pro Sekunde auswählen, bleibt die Gesamtzahl der E/A-Operationen pro Sekunde bei 100. </br></br>Das Verhältnis zwischen der Anzahl E/A-Operationen pro Sekunde und Gigabyte bestimmt auch den Typ der Festplatte, die für Sie bereitgestellt wird. Beispiel: Wenn Sie beispielsweise 500 Gigabyte (500Gi) bei 100 E/A-Operationen pro Sekunde haben, ist das Verhältnis E/A-Operationen pro Sekunde zu Gigabyte 0,2. Speicher mit einem Verhältnis von kleiner-gleich 0,3 wird auf SATA-Festplatten bereitgestellt. Wenn das Verhältnis größer als 0,3 ist, wird Ihr Speicher auf SSD-Festplatten bereitgestellt.
     <table>
         <caption>Tabelle mit Größenbereichen und der Anzahl E/A-Operationen pro Sekunde (IOPS) für Speicherklassen des Typs 'custom'</caption>
         <thead>
         <th>Größenbereich in Gigabyte</th>
         <th>IOPS-Bereich in Vielfachen von 100</th>
         </thead>
         <tbody>
         <tr>
         <td>20-39 Gi</td>
         <td>100-1000 IOPS</td>
         </tr>
         <tr>
         <td>40-79 Gi</td>
         <td>100-2000 IOPS</td>
         </tr>
         <tr>
         <td>80-99 Gi</td>
         <td>100-4000 IOPS</td>
         </tr>
         <tr>
         <td>100-499 Gi</td>
         <td>100-6000 IOPS</td>
         </tr>
         <tr>
         <td>500-999 Gi</td>
         <td>100-10000 IOPS</td>
         </tr>
         <tr>
         <td>1000-1999 Gi</td>
         <td>100-20000 IOPS</td>
         </tr>
         <tr>
         <td>2000-2999 Gi</td>
         <td>200-40000 IOPS</td>
         </tr>
         <tr>
         <td>3000-3999 Gi</td>
         <td>200-48000 IOPS</td>
         </tr>
         <tr>
         <td>4000-7999 Gi</td>
         <td>300-48000 IOPS</td>
         </tr>
         <tr>
         <td>8000-9999 Gi</td>
         <td>500-48000 IOPS</td>
         </tr>
         <tr>
         <td>10000-12000 Gi</td>
         <td>1000-48000 IOPS</td>
         </tr>
         </tbody></table>

5. Wählen Sie diese Option aus, wenn die Daten nach dem Löschen des Clusters oder des Persistent Volume Claim (PVC) beibehalten werden sollen.
   - Wenn Sie die Daten beibehalten möchten, wählen Sie eine Speicherklasse vom Typ `retain` aus. Beim Löschen des PVC wird nur der PVC gelöscht. Der persistente Datenträger, die physische Speichereinheit im Konto der IBM Cloud-Infrastruktur (SoftLayer) und Ihre Daten sind weiterhin vorhanden. Um den Speicher zurückzufordern und in Ihrem Cluster erneut zu verwenden, müssen Sie den persistenten Datenträger entfernen und die Schritte zum [Verwenden von vorhandenem Blockspeicher](#existing_block) ausführen.
   - Wenn Sie möchten, dass der persistente Datenträger, die Daten und Ihre physische Blockspeichereinheit beim Löschen des Persistent Volume Claim (PVC) gelöscht werden, müssen Sie eine Speicherklasse ohne `retain` auswählen.

6. Wählen Sie aus, ob Sie die Abrechnung auf Stundenbasis oder monatlich erhalten möchten. Weitere Informationen finden Sie unter [Preisstruktur ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/cloud/block-storage/pricing). Standardmäßig werden alle Blockspeichereinheiten mit dem Abrechnungstyp 'Stündlich' (hourly) bereitgestellt.

<br />



## Blockspeicher zu Apps hinzufügen
{: #add_block}

Erstellen Sie einen Persistent Volume Claim (PVC), um Blockspeicher für Ihren Cluster [dynamisch bereitzustellen](/docs/containers?topic=containers-kube_concepts#dynamic_provisioning). Mithilfe der dynamischen Bereitstellung wird der übereinstimmende persistente Datenträger (PV) automatisch erstellt und die tatsächliche Speichereinheit in Ihrem Konto der IBM Cloud-Infrastruktur (SoftLayer) bestellt.
{:shortdesc}

Blockspeicher wird mit dem Zugriffsmodus `ReadWriteOnce` geliefert. Sie können ihn jeweils immer nur an einen Pod auf einem Workerknoten im Cluster anhängen.
{: note}

Vorbereitende Schritte:
- Wenn Sie über eine Firewall verfügen, [gewähren Sie Egress-Zugriff](/docs/containers?topic=containers-firewall#pvc) für die IBM Cloud-Infrastruktur-IP-Bereiche (SoftLayer) der Zonen, in denen sich Ihre Cluster befinden, damit Sie Persistent Volume Claims (PVCs) erstellen können.
- Installieren Sie das [{{site.data.keyword.Bluemix_notm}}-Blockspeicher-Plug-in](#install_block).
- [Entscheiden Sie sich für eine vordefinierte Speicherklasse](#block_predefined_storageclass) oder erstellen Sie eine [angepasste Speicherklasse](#block_custom_storageclass).

Möchten Sie den Blockspeicher in einer statusabhängigen Gruppe bereitstellen? Weitere Informationen finden Sie unter [Blockspeicher in statusabhängiger Gruppe verwenden](#block_statefulset).
{: tip}

Gegen Sie wie folgt vor, um Blockspeicher hinzuzufügen:

1.  Erstellen Sie eine Konfigurationsdatei, um Ihren PVC zu definieren, und speichern Sie die Konfiguration als `.yaml`-Datei.

    -  **Beispiel für die Speicherklassen 'bronze', 'silver' und 'gold'**:
       Die folgende `.yaml`-Datei erstellt eine Anforderung mit dem Namen `mypvc` der Speicherklasse `"ibmc-block-silver"` und einer Abrechnung auf Stundenbasis (`"hourly"`) mit einer Größe von `24Gi`.

       ```
       apiVersion: v1
       kind: PersistentVolumeClaim
       metadata:
         name: mypvc
         labels:
           billingType: "hourly"
	       region: us-south
           zone: dal13
       spec:
         accessModes:
           - ReadWriteOnce
         resources:
           requests:
             storage: 24Gi
	     storageClassName: ibmc-block-silver
       ```
       {: codeblock}

    -  **Beispiel für die Verwendung der angepassten Speicherklasse ('custom')**:
       Die folgende `.yaml`-Datei erstellt eine Anforderung mit dem Namen `mypvc` der Speicherklasse `ibmc-block-retain-custom` und einer Abrechnung auf Stundenbasis (`"hourly"`) mit einer Größe von `45Gi` und einer Anzahl von `"300"` E/A-Operationen pro Sekunde.

       ```
       apiVersion: v1
       kind: PersistentVolumeClaim
       metadata:
         name: mypvc
         labels:
           billingType: "hourly"
	       region: us-south
           zone: dal13
       spec:
         accessModes:
           - ReadWriteOnce
         resources:
           requests:
             storage: 45Gi
             iops: "300"
	     storageClassName: ibmc-block-retain-custom
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
         <td><code>metadata.labels.billingType</code></td>
         <td>Geben Sie die Häufigkeit an, mit der Ihre Speicherrechnung berechnet wird, monatlich ('monthly') oder stündlich ('hourly'). Der Standardwert ist 'hourly'.</td>
       </tr>
       <tr>
       <td><code>metadata.labels.region</code></td>
       <td>Geben Sie die Region an, in der der Blockspeicher bereitgestellt werden soll. Bei Angabe der Region müssen Sie auch eine Zone angeben. Wenn Sie keine Region angeben oder die angegebene Region nicht gefunden wird, wird der Speicher in derselben Region wie Ihr Cluster erstellt. <p class="note">Diese Option wird nur mit dem Blockspeicher-Plug-in Version 1.0.1 oder höher von IBM Cloud unterstützt. Wenn Sie einen Mehrzonencluster haben, wird bei älteren Plug-in-Versionen die Zone, in der Ihr Speicher bereitgestellt wird, im Umlaufverfahren ausgewählt, um die Datenträgeranforderungen gleichmäßig auf alle Zonen zu verteilen. Zur Angabe der Zone für Ihren Speicher können Sie zuerst eine [angepasste Speicherklasse](#block_multizone_yaml) erstellen. Erstellen Sie anschließend einen PVC mit der angepassten Speicherklasse.</p></td>
       </tr>
       <tr>
       <td><code>metadata.labels.zone</code></td>
	<td>Geben Sie die Zone an, in der der Blockspeicher bereitgestellt werden soll. Bei Angabe der Zone müssen Sie auch eine Region angeben. Wenn Sie keine Zone angeben oder die angegebene Zone in einem Mehrzonencluster nicht gefunden wird, wird die Zone im Umlaufverfahren ausgewählt. <p class="note">Diese Option wird nur mit dem Blockspeicher-Plug-in Version 1.0.1 oder höher von IBM Cloud unterstützt. Wenn Sie einen Mehrzonencluster haben, wird bei älteren Plug-in-Versionen die Zone, in der Ihr Speicher bereitgestellt wird, im Umlaufverfahren ausgewählt, um die Datenträgeranforderungen gleichmäßig auf alle Zonen zu verteilen. Zur Angabe der Zone für Ihren Speicher können Sie zuerst eine [angepasste Speicherklasse](#block_multizone_yaml) erstellen. Erstellen Sie anschließend einen PVC mit der angepassten Speicherklasse.</p></td>
	</tr>
        <tr>
        <td><code>spec.resources.requests.storage</code></td>
        <td>Geben Sie die Größe des Blockspeichers in Gigabyte (Gi) an. Nach dem Bereitstellen des Speichers können Sie die Größe Ihres Blockspeichers nicht mehr ändern. Stellen Sie sicher, dass Sie eine Größe angeben, die dem Umfang der Daten entspricht, die Sie speichern möchten. </td>
        </tr>
        <tr>
        <td><code>spec.resources.requests.iops</code></td>
        <td>Diese Option gilt nur für die angepassten Speicherklassen (`ibmc-block-custom / ibmc-block-retain-custom`). Geben Sie die Gesamtzahl der E/A-Operationen pro Sekunde für den Speicher an, indem Sie ein Vielfaches von 100 innerhalb des zulässigen Bereichs auswählen. Wenn Sie einen Wert für die E/A-Operationen pro Sekunde auswählen, der nicht aufgelistet ist, wird der Wert aufgerundet.</td>
        </tr>
	<tr>
	<td><code>spec.storageClassName</code></td>
	<td>Der Name der Speicherklasse, die Sie für die Bereitstellung von Blockspeicher verwenden möchten. Sie haben die Wahl zwischen der Verwendung einer der [von IBM bereitgestellten Speicherklassen](#block_storageclass_reference) oder der [Erstellung einer eigenen Speicherklasse](#block_custom_storageclass). </br> Wenn Sie keine Speicherklasse angeben, wird der persistente Datenträger mit der Standardspeicherklasse <code>ibmc-file-bronze</code> erstellt.<p>**Tipp:** Wenn Sie die Standardspeicherklasse ändern möchten, führen Sie den Befehl <code>kubectl patch storageclass &lt;speicherklasse&gt; -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'</code> aus und ersetzen Sie <code>&lt;speicherklasse&gt;</code> durch den Namen der Speicherklasse.</p></td>
	</tr>
        </tbody></table>

    Wenn Sie eine angepasste Speicherklasse verwenden möchten, erstellen Sie den PVC mit dem entsprechenden Speicherklassennamen, einem gültigen Wert für IOPS und einer Größe.   
    {: tip}

2.  Erstellen Sie den PVC.

    ```
    kubectl apply -f mypvc.yaml
    ```
    {: pre}

3.  Überprüfen Sie, ob Ihr PVC erstellt und an das PV gebunden wurde. Dieser Prozess kann einige Minuten dauern.

    ```
    kubectl describe pvc mypvc
    ```
    {: pre}

    Beispielausgabe:

    ```
    Name: mypvc
    Namespace: default
    StorageClass: ""
    Status:		Bound
    Volume:		pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2
    Labels:		<none>
    Capacity:	20Gi
    Access Modes:	RWX
    Events:
      FirstSeen	LastSeen	Count	From								SubObjectPath	Type		Reason			Message
      ---------	--------	-----	----								-------------	--------	------			-------
      3m 3m 1 {ibm.io/ibmc-block 31898035-3011-11e7-a6a4-7a08779efd33 } Normal Provisioning External provisioner is provisioning volume  for claim "default/my-persistent-volume-claim"
       3m 1m	 10 {persistentvolume-controller } Normal ExternalProvisioning cannot find provisioner "ibm.io/ibmc-block", expecting that  a volume for the claim is provisioned either manually or via external software
       1m 1m 1 {ibm.io/ibmc-block 31898035-3011-11e7-a6a4-7a08779efd33 } Normal ProvisioningSucceeded	Successfully provisioned volume  pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2

    ```
    {: screen}

4.  {: #block_app_volume_mount}Erstellen Sie zum Anhängen des persistenten Datenträgers an Ihre Bereitstellung eine `.yaml`-Konfigurationsdatei und geben Sie den Persistent Volume Claim (PVC) an, der den persistenten Datenträger bindet.

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
    <td><code>spec.containers.volumeMounts.mountPath</code></td>
    <td>Der absolute Pfad des Verzeichnisses, in dem der Datenträger innerhalb des Containers angehängt wird. Daten, die in den Mountpfad geschrieben werden, werden unter dem Stammverzeichnis in der physischen Blockspeicherinstanz gespeichert. Wenn Sie einen Datenträger zwischen verschiedenen Apps gemeinsam nutzen möchten, können Sie [Unterpfade für Datenträger  ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/storage/volumes/#using-subpath) für jede Ihrer Apps angeben. </td>
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

<br />




## Vorhandenen Blockspeicher in Ihrem Cluster verwenden
{: #existing_block}

Wenn Sie bereits über eine physische Speichereinheit verfügen, die Sie in Ihrem Cluster verwenden möchten, können Sie den persistenten Datenträger und den PVC manuell erstellen, um den Speicher [statisch bereitzustellen](/docs/containers?topic=containers-kube_concepts#static_provisioning).
{: shortdesc}

Bevor Sie mit dem Anhängen Ihres vorhandenen Speichers an eine App beginnen können, müssen Sie alle erforderlichen Informationen für Ihren persistenten Datenträger abrufen.  

### Schritt 1: Informationen zu Ihrem vorhandenen Blockspeicher abrufen
{: #existing-block-1}

1.  Rufen Sie einen API-Schlüssel für Ihr Konto der IBM Cloud-Infrastruktur (SoftLayer) ab oder generieren Sie einen API-Schlüssel.
    1. Melden Sie sich beim [Portal der IBM Cloud-Infrastruktur (SoftLayer) ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/classic?) an.
    2. Wählen Sie **Konto**, dann **Benutzer** und dann **Benutzerliste** aus.
    3. Suchen Sie nach Ihrer Benutzer-ID.
    4. Klicken Sie in der Spalte **API-SCHLÜSSEL** auf **Generieren**, um einen API-Schlüssel zu generieren, oder auf **Anzeigen**, um Ihren vorhandenen API-Schlüssel anzuzeigen.
2.  Rufen Sie den API-Benutzernamen für Ihr Konto der IBM Cloud-Infrastruktur (SoftLayer) ab.
    1. Wählen Sie im Menü **Benutzerliste** Ihre Benutzer-ID aus.
    2. Suchen Sie im Abschnitt **API-Zugriffsinformationen** nach Ihrem **API-Benutzernamen**.
3.  Melden Sie sich beim Befehlszeilen-Plug-in der IBM Cloud-Infrastruktur an.
    ```
    ibmcloud sl init
    ```
    {: pre}

4.  Wählen Sie die Authentifizierung anhand des Benutzernamens und API-Schlüssels für Ihr Konto der IBM Cloud-Infrastruktur (SoftLayer) aus.
5.  Geben Sie den Benutzernamen und den API-Schlüssel ein, die Sie in den vorherigen Schritten abgerufen haben.
6.  Listen Sie verfügbare Blockspeichereinheiten auf.
    ```
    ibmcloud sl block volume-list
    ```
    {: pre}

    Beispielausgabe:
    ```
    id         username            datacenter   storage_type              capacity_gb   bytes_used   ip_addr         lunId   active_transactions
    38642141   IBM02SEL1543159-1   dal10        endurance_block_storage   20            -            169.xx.xxx.xxx   170     0
    ```
    {: screen}

7.  Notieren Sie sich die Angaben `id`, `ip_addr`, `capacity_gb`, das Rechenzentrum (`datacenter`) und `lunId` der Blockspeichereinheit, die Sie an Ihren Cluster anhängen möchten. **Hinweis:** Wenn Sie einen vorhandenen Speicher an einen Cluster anhängen möchten, müssen Sie in derselben Zone, in der sich auch der Speicher befindet, einen Workerknoten haben. Zum Überprüfen der Zone Ihres Workerknotens führen Sie den Befehl `ibmcloud ks workers --cluster <clustername_oder_-id>` aus.

### Schritt 2: Persistenten Datenträger (PV) und übereinstimmenden Persistent Volume Claim (PVC) erstellen
{: #existing-block-2}

1.  Optional: Wenn Sie über Speicher verfügen, den Sie mit einer Speicherklasse des Typs `retain` bereitgestellt haben, werden der persistente Datenträger und die physische Speichereinheit beim Entfernen des Persistent Volume Claim (PVC) nicht entfernt. Zum Wiederverwenden des Speichers in Ihrem Cluster müssen Sie zuerst den persistenten Datenträger entfernen.
    1. Listen Sie vorhandene persistente Datenträger auf.
       ```
       kubectl get pv
       ```
       {: pre}

       Suchen Sie den persistenten Datenträger, der zu Ihrem persistenten Speicher gehört. Der persistente Datenträger hat den Status 'freigegeben' (`released`).

    2. Entfernen Sie den persistenten Datenträger.
       ```
       kubectl delete pv <pv-name>
       ```
       {: pre}

    3. Überprüfen Sie, dass der persistente Datenträger entfernt wurde.
       ```
       kubectl get pv
       ```
       {: pre}

2.  Erstellen Sie eine Konfigurationsdatei für Ihren persistenten Datenträger (PV). Geben Sie zum Blockspeicher die Angaben für `id`, `ip_addr`, `capacity_gb`, das Rechenzentrum (`datacenter`) und die `lunIdID` an, die Sie zuvor abgerufen haben.

    ```
    apiVersion: v1
    kind: PersistentVolume
    metadata:
      name: mypv
      labels:
         failure-domain.beta.kubernetes.io/region: <region>
         failure-domain.beta.kubernetes.io/zone: <zone>
    spec:
      capacity:
        storage: "<speichergröße>"
      accessModes:
        - ReadWriteOnce
      flexVolume:
        driver: "ibm/ibmc-block"
        fsType: "<fs-typ>"
        options:
          "Lun": "<lun-id>"
          "TargetPortal": "<ip-adresse>"
          "VolumeID": "<datenträger-id>"
          "volumeName": "<datenträgername>"
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
    <td>Geben Sie den Namen des PV ein, das Sie erstellen möchten.</td>
    </tr>
    <tr>
    <td><code>metadata.labels</code></td>
    <td>Geben Sie die Region und die Zone ein, die Sie zuvor abgerufen haben. Zum Anhängen des Speichers an Ihren Cluster müssen Sie mindestens einen Workerknoten in der Region und Zone haben, in der sich auch der persistente Speicher befindet. Wenn bereits ein persistenter Datenträger für Ihren Speicher vorhanden ist, [fügen Sie die Zonen- und Regionsbezeichnung](/docs/containers?topic=containers-kube_concepts#storage_multizone) zu Ihrem persistenten Datenträger hinzu.
    </tr>
    <tr>
    <td><code>spec.flexVolume.fsType</code></td>
    <td>Geben Sie den Dateisystemtyp ein, der für den vorhandenen Blockspeicher konfiguriert ist. Wählen Sie zwischen <code>ext4</code> oder <code>xfs</code>. Wenn Sie diese Option nicht angeben, nimmt das PV standardmäßig den Wert <code>ext4</code> ein. Wenn der falsche Wert für `fsType` definiert ist, ist zwar die PV-Erstellung erfolgreich, das Anhängen des PVs an einen Pod schlägt jedoch fehl. </td></tr>	    
    <tr>
    <td><code>spec.capacity.storage</code></td>
    <td>Geben Sie die Speichergröße des vorhandenen Blockspeichers ein, den Sie im vorherigen Schritt als <code>capacity-gb</code> abgerufen haben. Die Größe des Speichers muss in Gigabyte angegeben werden, z. B. 20Gi (20 GB) oder 1000Gi (1 TB).</td>
    </tr>
    <tr>
    <td><code>flexVolume.options.Lun</code></td>
    <td>Geben Sie die LUN-ID für Ihren Blockspeicher ein, die Sie zuvor als <code>lunId</code> abgerufen haben.</td>
    </tr>
    <tr>
    <td><code>flexVolume.options.TargetPortal</code></td>
    <td>Geben Sie die IP-Adresse Ihres Blockspeichers ein, die Sie zuvor als <code>ip_addr</code> abgerufen haben. </td>
    </tr>
    <tr>
	    <td><code>flexVolume.options.VolumeId</code></td>
	    <td>Geben Sie die ID Ihres Blockspeichers ein, die Sie zuvor als <code>id</code> abgerufen haben.</td>
	    </tr>
	    <tr>
		    <td><code>flexVolume.options.volumeName</code></td>
		    <td>Geben Sie einen Namen für Ihren Datenträger ein.</td>
	    </tr>
    </tbody></table>

3.  Erstellen Sie das PV in Ihrem Cluster.
    ```
    kubectl apply -f mypv.yaml
    ```
    {: pre}

4. Überprüfen Sie, dass das PV erstellt wurde.
    ```
    kubectl get pv
    ```
    {: pre}

5. Erstellen Sie eine weitere Konfigurationsdatei, um Ihren Persistent Volume Claim (PVC) zu erstellen. Damit der Persistent Volume Claim (PVC) mit dem zuvor erstellten Persistent Volume übereinstimmt, müssen Sie denselben Wert für `storage` und `accessMode` auswählen. Das Feld `storage-class` muss leer sein. Wenn eines dieser Felder nicht mit dem PV (Persistent Volume) übereinstimmt, wird stattdessen automatisch ein neues PV erstellt.

     ```
     kind: PersistentVolumeClaim
     apiVersion: v1
     metadata:
      name: mypvc
     spec:
      accessModes:
        - ReadWriteOnce
      resources:
        requests:
          storage: "<speichergröße>"
      storageClassName:
     ```
     {: codeblock}

6.  Erstellen Sie Ihren PVC.
     ```
     kubectl apply -f mypvc.yaml
     ```
     {: pre}

7.  Überprüfen Sie, ob Ihr PVC erstellt und an das zuvor erstellte PV gebunden wurde. Dieser Prozess kann einige Minuten dauern.
     ```
     kubectl describe pvc mypvc
     ```
     {: pre}

     Beispielausgabe:

     ```
     Name: mypvc
    Namespace: default
    StorageClass: ""
     Status: Bound
     Volume: pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2
     Labels: <none>
     Capacity: 20Gi
     Access Modes: RWO
     Events:
       FirstSeen LastSeen Count From        SubObjectPath Type Reason Message
       --------- -------- ----- ----        ------------- -------- ------ -------
       3m 3m 1 {ibm.io/ibmc-block 31898035-3011-11e7-a6a4-7a08779efd33 } Normal Provisioning External provisioner is provisioning volume  for claim "default/my-persistent-volume-claim"
       3m 1m	 10 {persistentvolume-controller } Normal ExternalProvisioning cannot find provisioner "ibm.io/ibmc-block", expecting that  a volume for the claim is provisioned either manually or via external software
       1m 1m 1 {ibm.io/ibmc-block 31898035-3011-11e7-a6a4-7a08779efd33 } Normal ProvisioningSucceeded	Successfully provisioned volume  pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2
     ```
     {: screen}

Sie haben erfolgreich ein PV (Persistent Volume) erstellt und an einen PVC (Persistent Volume Claim) gebunden. Clusterbenutzer können jetzt den [PVC (Persistent Volume Claim) an ihre Bereitstellungen anhängen](#block_app_volume_mount) und mit dem Lesen und Schreiben in dem PV (Persistent Volume) beginnen.

<br />



## Blockspeicher in statusabhängiger Gruppe verwenden
{: #block_statefulset}

Wenn Sie über eine statusabhängige App wie zum Beispiel eine Datenbank verfügen, können Sie statusabhängige Gruppen erstellen, von denen Blockspeicher zum Speichern der App-Daten verwendet wird. Alternativ können Sie eine {{site.data.keyword.Bluemix_notm}}-Database as a Service und die Daten in der Cloud speichern.
{: shortdesc}

**Worauf muss ich achten, wenn ich Blockspeicher zu einer statusabhängigen Gruppe hinzufüge?** </br>
Wenn Sie Speicher zu einer statusabhängigen Gruppe hinzufügen möchten, geben Sie die Speicherkonfiguration im Abschnitt `volumeClaimTemplates` der YAML-Datei für die statusabhängige Gruppe an. `volumeClaimTemplates` ist die Basis für den PVC (Persistent Volume Claim) und kann die Speicherklasse und die Größe der IOPS (E/A-Operationen pro Sekunde) des Blockspeichers enthalten, den Sie bereitstellen möchten. Wenn in `volumeClaimTemplates` jedoch Bezeichnungen enthalten sein sollen, werden diese Bezeichnungen von Kubernetes beim Erstellen des PVC nicht eingeschlossen. Stattdessen müssen Sie die Bezeichnungen direkt zur statusabhängigen Gruppe hinzufügen.

Sie können nicht zwei statusabhängige Gruppen gleichzeitig bereitstellen. Wenn Sie versuchen, eine statusabhängige Gruppe zu erstellen, bevor eine andere vollständig bereitgestellt wird, kann die Bereitstellung der statusabhängigen Gruppe zu unerwarteten Ergebnissen führen.
{: important}

**Wie kann ich eine statusabhängige Gruppe in einer bestimmten Zone erstellen?** </br>
In einem Mehrzonencluster können Sie die Zone und die Region, in der Sie die statusabhängige Gruppe erstellen möchten, in den Abschnitten `spec.selector.matchLabels` und `spec.template.metadata.labels` der YAML-Datei für die statusabhängige Gruppe angeben. Alternativ können Sie diese Bezeichnungen zu einer [angepassten Speicherklasse](/docs/containers?topic=containers-kube_concepts#customized_storageclass) hinzufügen und diese Speicherklasse im Abschnitt `volumeClaimTemplates` der statusabhängigen Gruppe verwenden.

**Kann ich das Binden eines PV an meinen statusabhängigen Pod verzögern, bis der Pod bereit ist?**<br>
Ja, Sie können [eine angepasste Speicherklasse für Ihren PVC erstellen](#topology_yaml), der das Feld [`volumeBindingMode: WaitForFirstConsumer` ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/storage/storage-classes/#volume-binding-mode) enthält.

**Welche Möglichkeiten habe ich, um Blockspeicher zu einer statusabhängigen Gruppe hinzuzufügen?** </br>
Falls Sie automatisch einen PVC erstellen möchten, wenn Sie eine statusabhängige Gruppe erstellen, verwenden Sie die [dynamische Bereitstellung](#block_dynamic_statefulset). Außerdem können Sie mit der statusabhängigen Gruppe [PVCs vorab bereitstellen oder vorhandene PVCs verwenden](#block_static_statefulset).  

### Dynamische Bereitstellung: PVC beim Erstellen einer statusabhängigen Gruppe erstellen
{: #block_dynamic_statefulset}

Verwenden Sie diese Option, wenn bei der Erstellung einer statusabhängigen Gruppe automatisch ein PVC erstellt werden soll.
{: shortdesc}

Vorbereitende Schritte: [Melden Sie sich an Ihrem Konto an. Geben Sie als Ziel die entsprechende Region und, sofern zutreffend, die Ressourcengruppe an. Legen Sie den Kontext für den Cluster fest.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1. Stellen Sie sicher, dass alle vorhandenen statusabhängigen Gruppen vollständig bereitgestellt werden. Falls die Bereitstellung einer statusabhängigen Gruppe noch andauert, können Sie nicht mit dem Erstellen der statusabhängigen Gruppe beginnen. Sie müssen warten, bis alle statusabhängigen Gruppen im Cluster vollständig bereitgestellt wurden, um unerwartete Ergebnisse zu vermeiden.
   1. Listen Sie vorhandene statusabhängige Gruppen im Cluster auf.
      ```
      kubectl get statefulset --all-namespaces
      ```
      {: pre}

      Beispielausgabe:
      ```
      NAME              DESIRED   CURRENT   AGE
      mystatefulset     3         3         6s
      ```
      {: screen}

   2. Zeigen Sie den **Podstatus** jeder einzelnen statusabhängigen Gruppe an, um sicherzustellen, dass die Bereitstellung der statusabhängigen Gruppe abgeschlossen ist.  
      ```
      kubectl describe statefulset <statefulset_name>
      ```
      {: pre}

      Beispielausgabe:
      ```
      Name:               nginx
      Namespace:          default
      CreationTimestamp:  Fri, 05 Oct 2018 13:22:41 -0400
      Selector:           app=nginx,billingType=hourly,region=us-south,zone=dal10
      Labels:             app=nginx
                          billingType=hourly
                          region=us-south
                          zone=dal10
      Annotations:        kubectl.kubernetes.io/last-applied-configuration={"apiVersion":"apps/v1","kind":"StatefulSet","metadata":{"annotations":{},"name":"nginx","namespace":"default"},"spec":{"podManagementPolicy":"Par...
      Replicas:           3 desired | 3 total
      Pods Status:        0 Running / 3 Waiting / 0 Succeeded / 0 Failed
      Pod Template:
        Labels:  app=nginx
                 billingType=hourly
                 region=us-south
                 zone=dal10
      ...
      ```
      {: screen}

      Eine statusabhängige Gruppe wird vollständig bereitgestellt, wenn die Anzahl der Replikate, die im Abschnitt **Replicas** (Replikate) der CLI-Ausgabe enthalten sind, mit der Anzahl der **aktiven** Pods im Abschnitt **Pods Status** (Podstatus) übereinstimmt. Wenn eine statusabhängige Gruppe noch nicht vollständig bereitgestellt wird, müssen Sie warten, bis die Bereitstellung abgeschlossen ist, bevor Sie fortfahren können.

2. Erstellen Sie eine Konfigurationsdatei für die statusabhängige Gruppe und den Service, den Sie verwenden, um die statusabhängige Gruppe zugänglich zu machen.

   - **Beispiel für eine statusabhängige Gruppe, die eine Zone angibt:**

     Im folgenden Beispiel wird veranschaulicht, wie NGINX als statusabhängige Gruppe mit drei Replikaten bereitgestellt wird. Für jedes Replikat wird eine 20-Gigabyte-Blockspeichereinheit basierend auf den Spezifikationen bereitgestellt, die in der Speicherklasse `ibmc-block-retain-bronze` definiert sind. Alle Speichereinheiten werden in der Zone `dal10` bereitgestellt. Da auf Blockspeicher nicht von anderen Zonen aus zugegriffen werden kann, werden alle Replikate der statusabhängigen Gruppe auch auf Workerknoten bereitgestellt, die sich in `dal10` befinden.

     ```
     apiVersion: v1
     kind: Service
     metadata:
      name: nginx
      labels:
        app: nginx
     spec:
      ports:
      - port: 80
        name: web
      clusterIP: None
      selector:
        app: nginx
     ---
     apiVersion: apps/v1
     kind: StatefulSet
     metadata:
      name: nginx
     spec:
      serviceName: "nginx"
      replicas: 3
      podManagementPolicy: Parallel
      selector:
        matchLabels:
          app: nginx
          billingType: "hourly"
          region: "us-south"
          zone: "dal10"
      template:
        metadata:
          labels:
            app: nginx
            billingType: "hourly"
            region: "us-south"
            zone: "dal10"
        spec:
          containers:
          - name: nginx
            image: k8s.gcr.io/nginx-slim:0.8
            ports:
            - containerPort: 80
              name: web
            volumeMounts:
            - name: myvol
              mountPath: /usr/share/nginx/html
      volumeClaimTemplates:
      - metadata:
          name: myvol
        spec:
          accessModes:
          - ReadWriteOnce
          resources:
            requests:
              storage: 20Gi
              iops: "300" #required only for performance storage
	      storageClassName: ibmc-block-retain-bronze
     ```
     {: codeblock}

   - **Beispiel für eine statusabhängige Gruppe mit Anti-Affinitätsregel und verzögerter Blockspeichererstellung:**

     Im folgenden Beispiel wird veranschaulicht, wie NGINX als statusabhängige Gruppe mit drei Replikaten bereitgestellt wird. Die statusabhängige Gruppe gibt die Region und Zone nicht an, in der der Blockspeicher erstellt wird. Stattdessen verwendet die statusabhängige Gruppe eine Anti-Affinitätsregel, um sicherzustellen, dass die Pods auf Workerknoten und Zonen verteilt werden. Durch die Definition von `topologykey: failure-domain.beta.kubernetes.io/zone` kann der Kubernetes-Scheduler einen Pod auf einem Workerknoten nicht planen, wenn sich der Workerknoten in derselben Zone wie ein Pod befindet, der die Bezeichnung `app: nginx` hat. Für jeden Pod einer statusabhängigen Gruppe werden zwei PVCs wie im Abschnitt `volumeClaimTemplates` definiert erstellt, jedoch wird die Erstellung der Blockspeicherinstanzen verzögert, bis ein Pod der statusabhängigen Gruppe geplant wird, der den Speicher verwendet. Diese Konfiguration wird als [topologieorientierte Datenträgerplanung (topology-aware volume scheduling)](https://kubernetes.io/blog/2018/10/11/topology-aware-volume-provisioning-in-kubernetes/) bezeichnet.

     ```
     apiVersion: storage.k8s.io/v1
     kind: StorageClass
     metadata:
       name: ibmc-block-bronze-delayed
     parameters:
       billingType: hourly
       classVersion: "2"
       fsType: ext4
       iopsPerGB: "2"
       sizeRange: '[20-12000]Gi'
       type: Endurance
     provisioner: ibm.io/ibmc-block
     reclaimPolicy: Delete
     volumeBindingMode: WaitForFirstConsumer
     ---
     apiVersion: v1
     kind: Service
     metadata:
       name: nginx
       labels:
         app: nginx
     spec:
       ports:
       - port: 80
         name: web
       clusterIP: None
       selector:
         app: nginx
     ---
     apiVersion: apps/v1
     kind: StatefulSet
     metadata:
       name: web
     spec:
       serviceName: "nginx"
       replicas: 3
       podManagementPolicy: "Parallel"
       selector:
         matchLabels:
           app: nginx
       template:
         metadata:
           labels:
             app: nginx
         spec:
           affinity:
             podAntiAffinity:
               preferredDuringSchedulingIgnoredDuringExecution:
               - weight: 100
        podAffinityTerm:
          labelSelector:
            matchExpressions:
                     - key: app
              operator: In
              values:
                       - nginx
                   topologyKey: failure-domain.beta.kubernetes.io/zone
           containers:
           - name: nginx
             image: k8s.gcr.io/nginx-slim:0.8
             ports:
             - containerPort: 80
               name: web
             volumeMounts:
             - name: www
               mountPath: /usr/share/nginx/html
             - name: wwwww
               mountPath: /tmp1
       volumeClaimTemplates:
       - metadata:
           name: myvol1
         spec:
           accessModes:
           - ReadWriteOnce # access mode
           resources:
             requests:
               storage: 20Gi
	       storageClassName: ibmc-block-bronze-delayed
       - metadata:
           name: myvol2
         spec:
           accessModes:
           - ReadWriteOnce # access mode
           resources:
             requests:
               storage: 20Gi
	       storageClassName: ibmc-block-bronze-delayed
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
     <td style="text-align:left"><code>spec.podManagementPolicy</code></td>
     <td style="text-align:left">Geben Sie die Richtlinie für die Podverwaltung ein, die Sie für die statusabhängige Gruppe verwenden möchten. Wählen Sie eine der beiden folgenden Optionen aus: <ul><li><strong>`OrderedReady`:</strong> Bei Verwendung dieser Option werden die Replikate der statusabhängigen Gruppe nacheinander bereitgestellt. Beispiel: Wenn Sie drei Replikate angeben, wird von Kubernetes der PVC für das erste Replikat erstellt, bis zur Bindung des PVC gewartet, das Replikat der statusabhängigen Gruppe bereitgestellt und der PVC an das Replikat angehängt. Wenn die Bereitstellung abgeschlossen ist, wird das zweite Replikat bereitgestellt. Weitere Informationen zu dieser Option finden Sie unter [Podverwaltung mit `OrderedReady` ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/#orderedready-pod-management). </li><li><strong>Parallel:</strong> Bei Verwendung dieser Option wird mit der Bereitstellung aller Replikate der statusabhängigen Gruppe zur gleichen Zeit begonnen. Wenn von Ihrer App die parallele Bereitstellung von Replikaten unterstützt wird, können Sie mit dieser Option Bereitstellungszeit für die PVCs und Replikate der statusabhängige Gruppe sparen. </li></ul></td>
     </tr>
     <tr>
     <td style="text-align:left"><code>spec.selector.matchLabels</code></td>
     <td style="text-align:left">Geben Sie alle Bezeichnungen ein, die Sie in die statusabhängige Gruppe und in den PVC einschließen möchten. Bezeichnungen, die Sie in <code>volumeClaimTemplates</code> der statusabhängigen Gruppe einschließen, werden von Kubernetes nicht erkannt. Nachfolgend werden Beispiele für Bezeichnungen aufgeführt, die Sie einschließen können: <ul><li><code><strong>region</strong></code> und <code><strong>zone</strong></code>: Falls alle Replikate der statusabhängigen Gruppe und PVCs in einer bestimmten Zone erstellt werden sollen, fügen Sie beide Bezeichnungen hinzu. Sie können die Zone und Region auch in der Speicherklasse angeben, die Sie verwenden. Falls Sie keine Zone und Region angeben und über einen Mehrzonencluster verfügen, wird die Zone, in der der Speicher bereitgestellt wird, im Umlaufverfahren ausgewählt, um die Datenträgeranforderungen gleichmäßig auf alle Zonen zu verteilen.</li><li><code><strong>billingType</strong></code>: Geben Sie den Abrechnungstyp ein, den Sie für die PVCs verwenden möchten. Sie können zwischen 'Stündlich' (<code>hourly</code>) und 'Monatlich' (<code>monthly</code>) auswählen. Wenn Sie diese Bezeichnung nicht angeben, werden alle PVCs mit stündlicher Abrechnung erstellt. </li></ul></td>
     </tr>
     <tr>
     <td style="text-align:left"><code>spec.template.metadata.labels</code></td>
     <td style="text-align:left">Geben Sie dieselben Bezeichnungen ein, die Sie zum Abschnitt <code>spec.selector.matchLabels</code> hinzugefügt haben. </td>
     </tr>
     <tr>
     <td style="text-align:left"><code>spec.template.spec.affinity</code></td>
     <td style="text-align:left">Geben Sie Ihre Anti-Affinitätsregel an, um sicherzustellen, dass Ihre Pods der statusabhängigen Gruppe auf Workerknoten und Zonen verteilt werden. Das Beispiel zeigt eine Anti-Affinitätsregel, bei der der Pod der statusabhängigen Gruppe es bevorzugt, nicht auf einem Workerknoten geplant zu werden, auf dem ein Pod ausgeführt wird, der die Bezeichnung `app: nginx` hat. Die Bezeichnung `topologykey: failure-domain.beta.kubernetes.io/zone` schränkt diese Anti-Affinitätregel noch weiter ein und verhindert, dass der Pod auf einem Workerknoten geplant wird, wenn sich der Workerknoten in derselben Zone wie ein Pod befindet, der die Bezeichnung `app: nginx` hat. Durch die Verwendung dieser Anti-Affinitätsregel können Sie Anti-Affinität über Workerknoten und Zonen hinweg erreichen. </td>
     </tr>
     <tr>
     <td style="text-align:left"><code>spec.volumeClaimTemplates.metadata.name</code></td>
     <td style="text-align:left">Geben Sie einen Namen für Ihren Datenträger ein. Verwenden Sie denselben Namen, den Sie im Abschnitt <code>spec.containers.volumeMount.name</code> definiert haben. Der hier eingegebene Name wird zum Erstellen des Namens für den PVC im folgenden Format verwendet: <code>&lt;datenträgername&gt;-&lt;name_der_statusabhängigen_gruppe&gt;-&lt;replikatnummer&gt;</code>. </td>
     </tr>
     <tr>
     <td style="text-align:left"><code>spec.volumeClaimTemplates.spec.resources.</code></br><code>requests.storage</code></td>
     <td style="text-align:left">Geben Sie die Größe des Blockspeichers in Gigabytes (Gi) an.</td>
     </tr>
     <tr>
     <td style="text-align:left"><code>spec.volumeClaimTemplates.spec.resources.</code></br><code>requests.iops</code></td>
     <td style="text-align:left">Wenn Sie [Performance-Speicher](#block_predefined_storageclass) bereitstellen möchten, geben Sie die Anzahl der IOPS (E/A-Operationen pro Sekunde) ein. Wenn Sie eine Endurance-Speicherklasse verwenden und einen IOPS-Wert angeben, wird der IOPS-Wert ignoriert. Stattdessen wird der in der Speicherklasse angegebene IOPS-Wert verwendet.  </td>
     </tr>
     <tr>
     <td style="text-align:left"><code>spec.volumeClaimTemplates.</code></br><code>spec.storageClassName</code></td>
     <td style="text-align:left">Geben Sie die Speicherklasse ein, die Sie verwenden möchten. Wenn Sie die vorhandenen Speicherklassen auflisten möchten, führen Sie <code>kubectl get storageclasses | grep block</code> aus. Wenn Sie keine Speicherklasse angeben, wird der PVC mit der Standardspeicherklasse erstellt, die im Cluster festgelegt wurde. Stellen Sie sicher, dass von der Standardspeicherklasse der Bereitsteller <code>ibm.io/ibmc-block</code> verwendet wird, damit die statusabhängige Gruppe mit Blockspeicher bereitgestellt wird.</td>
     </tr>
     </tbody></table>

4. Erstellen Sie die statusabhängige Gruppe.
   ```
   kubectl apply -f statefulset.yaml
   ```
   {: pre}

5. Warten Sie, bis die statusabhängige Gruppe bereitgestellt ist.
   ```
   kubectl describe statefulset <statefulset_name>
   ```
   {: pre}

   Wenn Sie den aktuellen Status der PVCs anzeigen möchten, führen Sie `kubectl get pvc` aus. Das Format für den Namen des PVC ist `<volume_name>-<statefulset_name>-<replica_number>`.
   {: tip}

### Statische Bereitstellung: Vorhandene PVCs mit einer statusabhängigen Gruppe verwenden
{: #block_static_statefulset}

Sie können die PVCs vor der Erstellung der statusabhängigen Gruppe vorab bereitstellen oder die vorhandenen PVCs mit der statusabhängigen Gruppe verwenden.
{: shortdesc}

Wenn Sie [die PVCs dynamisch beim Erstellen der statusabhängigen Gruppe bereitstellen](#block_dynamic_statefulset), wird der Name des PVC auf der Basis der Werte zugeordnet, die Sie in der YAML-Datei für die statusabhängige Gruppe verwendet haben. Wenn von der statusabhängigen Gruppe vorhandene PVCs verwendet werden sollen, muss der Name der PVCs mit dem Namen übereinstimmen, der automatisch bei Verwendung der dynamischen Bereitstellung verwendet werden würde.

Vorbereitende Schritte: [Melden Sie sich an Ihrem Konto an. Geben Sie als Ziel die entsprechende Region und, sofern zutreffend, die Ressourcengruppe an. Legen Sie den Kontext für den Cluster fest.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1. Wenn Sie den PVC für Ihre statusabhängige Gruppe vorab bereitstellen wollen, bevor Sie die statusabhängige Gruppe erstellen, führen Sie die Schritte 1 bis 3 unter [Blockspeicher zu Apps hinzufügen](#add_block) aus, um einen PVC für jedes Replikat der statusabhängigen Gruppe zu erstellen. Stellen Sie sicher, dass Sie den PVC mit einem Namen erstellen, der das folgende Format aufweist: `<volume_name>-<statefulset_name>-<replica_number>`.
   - **`<volume_name>`**: Verwenden Sie den Namen, den Sie im Abschnitt `spec.volumeClaimTemplates.metadata.name` der statusabhängigen Gruppe angeben möchten, zum Beispiel `nginxvol`.
   - **`<statefulset_name>`**: Verwenden Sie den Namen, den Sie im Abschnitt `metadata.name` der statusabhängigen Gruppe angeben möchten, zum Beispiel `nginx_statefulset`.
   - **`<replica_number>`**: Geben Sie die Nummer des Replikats ein und beginnen Sie dabei mit 0.

   Wenn Sie zum Beispiel drei Replikate für eine statusabhängige Gruppe erstellen müssen, erstellen Sie 3 PVCs mit den folgenden Namen: `nginxvol-nginx_statefulset-0`, `nginxvol-nginx_statefulset-1` und `nginxvol-nginx_statefulset-2`.  

   Wollen Sie einen PVC und einen PV für eine vorhandene Speichereinheit erstellen? Erstellen Sie Ihren PVC und Ihren PV mithilfe der [statischen Bereitstellung](#existing_block).

2. Führen Sie die Schritte unter [Dynamische Bereitstellung: PVC beim Erstellen einer statusabhängigen Gruppe erstellen](#block_dynamic_statefulset) aus, um Ihre statusabhängige Gruppe zu erstellen. Das Format für den Namen des PVC ist `<datenträgername>-<name_der_statusabhängigen_gruppe>-<anzahl_replikate>`. Stellen Sie sicher, dass Sie die folgenden Werte aus den PVC-Namen in der Spezifikation der statusabhängigen Gruppe verwenden:
   - **`spec.volumeClaimTemplates.metadata.name`**: Geben Sie den Wert für `<datenträgername>` Ihres PVC-Namens ein.
   - **`metadata.name`**: Geben Sie den Wert für `<name_der_statusabhängigen_gruppe>` Ihres PVC-Namens ein.
   - **`spec.replicas`**: Geben Sie die Anzahl der Replikate ein, die Sie für die statusabhängige Gruppe erstellen möchten. Die Anzahl der Replikate muss mit der Anzahl der PVCs identisch sein, die Sie zuvor erstellt haben.

   Wenn sich Ihre PVCs in verschiedenen Zonen befinden, schließen Sie keine Bezeichnung für eine Region oder Zone in die statusabhängige Gruppe ein.
   {: note}

3. Überprüfen Sie, ob die PVCs in den Pods der Replikate der statusabhängige Gruppe verwendet werden.
   1. Listen Sie die Pods in Ihrem Cluster auf. Suchen Sie die Pods, die zur statusabhängigen Gruppe gehören.
      ```
      kubectl get pods
      ```
      {: pre}

   2. Stellen Sie sicher, dass der vorhandene PVC an das Replikat der statusabhängigen Gruppe angehängt ist. Überprüfen Sie den Wert für **`ClaimName`** im Abschnitt **`Volumes`** (Datenträger) der CLI-Ausgabe.
      ```
      kubectl describe pod <podname>
      ```
      {: pre}

      Beispielausgabe:
      ```
      Name:           nginx-0
      Namespace:      default
      Node:           10.xxx.xx.xxx/10.xxx.xx.xxx
      Start Time:     Fri, 05 Oct 2018 13:24:59 -0400
      ...
      Volumes:
        myvol:
          Type:       PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
          ClaimName:  myvol-nginx-0
     ...
      ```
      {: screen}

<br />


## Größe und E/A-Operationen pro Sekunde Ihrer vorhandenen Speichereinheit ändern
{: #block_change_storage_configuration}

Wenn Sie die Speicherkapazität oder die Leistung erhöhen möchten, können Sie den vorhandenen Datenträger ändern.
{: shortdesc}

Informationen zur Abrechnung und die Schritte zur Verwendung der {{site.data.keyword.Bluemix_notm}}-Konsole zum Ändern Ihres Speichers finden Sie unter [Blockspeicherkapazität erweitern](/docs/infrastructure/BlockStorage?topic=BlockStorage-expandingcapacity#expandingcapacity). Wenn Sie die {{site.data.keyword.Bluemix_notm}}-Konsole verwenden, um Ihren Speicher zu ändern, müssen Sie die Schritte 4-7 in diesem Abschnitt ausführen, um die Änderung abzuschließen.
{: tip}

1. Listen Sie die PVCs in Ihrem Cluster auf und notieren Sie sich den Namen des zugehörigen persistenten Datenträgers in der Spalte **VOLUME**.
   ```
   kubectl get pvc
   ```
   {: pre}

   Beispielausgabe:
   ```
   NAME             STATUS    VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS        AGE
   myvol            Bound     pvc-01ac123a-123b-12c3-abcd-0a1234cb12d3   20Gi       RWX            ibmc-block-bronze    147d
   ```
   {: screen}

2. Rufen Sie die Datenträger-ID (**`VolumeID`**) und den Speichertyp (**`StorageType`**) des physischen Dateispeichers ab, die Ihrem PVC zugeordnet sind, indem Sie die Details des persistenten Datenträgers auflisten, an den Ihr PVC gebunden ist. Ersetzen Sie `<pv-name>` durch den Namen des persistenten Datenträgers (PV), den Sie im vorherigen Schritt abgerufen haben. Der Speichertyp wird im Abschnitt mit **Labels** (Bezeichnungen) angezeigt und die Datenträger-ID wird im Abschnitt **Source** (Quelle) > **Options** (Optionen) Ihrer CLI-Ausgabe angezeigt.
   ```
   kubectl describe pv <pv-name>
   ```
   {: pre}

   Beispielausgabe:
   ```
   Name:            pvc-c1839152-c333-11e8-b6a8-46ad53f2579a
   Labels:          CapacityGb=24
                    Datacenter=dal13
                    IOPS=4
                    StorageType=Endurance
                    billingType=hourly
                    failure-domain.beta.kubernetes.io/region=us-south
                    failure-domain.beta.kubernetes.io/zone=dal13
                    ibm-cloud.kubernetes.io/iaas-provider=softlayer
   ...
   Source:
       Type:       FlexVolume (eine generische Datenträgerressource, die mithilfe eines Exec-basierten Plug-ins bereitgestellt/angehängt wird)
       Driver:     ibm/ibmc-block
       FSType:     ext4
       SecretRef:  <nil>
       ReadOnly:   false
       Options:    map[volumeName:pvc-c1839152-c333-11e8-b6a8-46ad53f2579a Lun:1 TargetPortal:161.26.114.56 VolumeID:51889685]
   ...
   ```
   {: screen}

3. Ändern Sie die Größe oder die E/A-Operationen pro Sekunde Ihres Datenträgers in Ihrem IBM Cloud-Infrastrukturkonto (SoftLayer).

   Beispiel für einen Leistungsspeicher:
   ```
   ibmcloud sl block volume-modify <datenträger-id> --new-size <größe> --new-iops <iops>
   ```
   {: pre}

   Beispiel für Endurance-Speicher:
   ```
   ibmcloud sl block volume-modify <datenträger-id> --new-size <größe> --new-tier <iops>
   ```
   {: pre}

   <table>
   <caption>Erklärung der Komponenten des Befehls</caption>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der YAML-Dateikomponenten</th>
   </thead>
   <tbody>
   <tr>
   <td><code>&lt;datenträger-id&gt;</code></td>
   <td>Geben Sie die ID des Datenträgers ein, den Sie zuvor abgerufen haben.</td>
   </tr>
   <tr>
   <td><code>&lt;new-size&gt;</code></td>
   <td>Geben Sie die neue Größe in Gigabyte für Ihren Datenträger ein. Informationen zu gültigen Größen finden Sie unter [Blockspeicherkonfiguration festlegen](#block_predefined_storageclass). Die Größe, die Sie eingeben, muss größer-gleich der aktuellen Größe Ihres Datenträgers sein. Wenn Sie keine neue Größe angeben, wird die aktuelle Größe des Datenträgers verwendet. </td>
   </tr>
   <tr>
   <td><code>&lt;new-iops&gt;</code></td>
   <td>Nur für Leistungsspeicher. Geben Sie die gewünschte neue Anzahl von E/A-Operationen pro Sekunde ein. Informationen zu gültigen E/A-Operationen pro Sekunde finden Sie unter [Blockspeicherkonfiguration festlegen](#block_predefined_storageclass). Wenn Sie die E/A-Operationen pro Sekunde nicht angeben, wird die aktuelle Anzahl E/A-Operationen pro Sekunde verwendet. <p class="note">Wenn der ursprüngliche IOPS/GB-Faktor für den Datenträger kleiner als 0,3 ist, muss der neue IOPS/GB-Faktor kleiner als 0,3 sein. Wenn der ursprüngliche IOPS/GB-Faktor für den Datenträger größer-gleich 0,3 ist, muss der neue IOPS/GB-Faktor größer-gleich 0,3 sein.</p> </td>
   </tr>
   <tr>
   <td><code>&lt;new-tier&gt;</code></td>
   <td>Nur für Endurance-Speicher. Geben Sie die gewünschte neue Anzahl von E/A-Operationen pro Sekunde pro GB ein. Informationen zu gültigen E/A-Operationen pro Sekunde finden Sie unter [Blockspeicherkonfiguration festlegen](#block_predefined_storageclass). Wenn Sie die E/A-Operationen pro Sekunde nicht angeben, wird die aktuelle Anzahl E/A-Operationen pro Sekunde verwendet. <p class="note">Wenn der ursprüngliche IOPS/GB-Faktor für den Datenträger kleiner als 0,25 ist, muss der neue IOPS/GB-Faktor kleiner als 0,25 sein. Wenn der ursprüngliche IOPS/GB-Faktor für den Datenträger größer-gleich 0,25 ist, muss der neue IOPS/GB-Faktor größer-gleich 0,25 sein.</p> </td>
   </tr>
   </tbody>
   </table>

   Beispielausgabe:
   ```
   Order 31020713 was placed successfully!.
   > Storage as a Service

   > 40 GBs

   > 2 IOPS per GB

   > 20 GB Storage Space (Snapshot Space)

   You may run 'ibmcloud sl block volume-list --order 12345667' to find this block volume after it is ready.
   ```
   {: screen}

4. Korrigieren Sie die Konfiguration des persistenten Datenträgers, um die Annotation `autofix-resizefs` hinzuzufügen. Diese Annotation ändert die Größe des Dateisystems automatisch, wenn der Datenträger an einen Pod angehängt wird.  
   ```
   kubectl patch pv <name_des_persistenten_datenträgers> -p '{"metadata": {"annotations":{"ibm.io/autofix-resizefs":"true"}}}'
   ```
   {: pre}

5. Listen Sie alle Pods auf, die den PVC verwenden.
   ```
   kubectl get pods --all-namespaces -o=jsonpath='{range .items[*]}{"\n"}{.metadata.name}{":\t"}{range .spec.volumes[*]}{.persistentVolumeClaim.claimName}{" "}{end}{end}' | grep "<pvc-name>"
   ```
   {: pre}

         Pods werden im folgenden Format zurückgegeben: `<pod_name>: <pvc_name>`.


6. Wenn Sie über einen Pod verfügen, der den PVC verwendet, starten Sie den Pod erneut, indem Sie den Pod entfernen und ihn von Kubernetes erneut erstellen lassen. Wenn Sie einen Pod erstellt haben, ohne eine Kubernetes-Bereitstellung oder eine Replikatgruppe zu verwenden, müssen Sie Ihren Pod erneut erstellen, nachdem Sie ihn entfernt haben.
   Um die YAML-Datei abzurufen, mit der zuvor Ihr Pod erstellt wurde, führen Sie den Befehl `kubectl get pod <pod_name> -o yaml >pod.yaml` aus.
   {: tip}
   ```
   kubectl delete pod <podname>
   ```
   {: pre}

7. Wenn Sie die Größe Ihres Datenträgers geändert haben, melden Sie sich bei Ihrem Pod an, um die neue Größe zu überprüfen. Beachten Sie, dass die Größenänderung der Speicherinstanz eine Weile dauert und Sie die Größe erst überprüfen können,, wenn der Prozess abgeschlossen ist.
   1. Rufen Sie den Datenträgermountpfad ab, den Sie in Ihrem Pod verwendet haben, um auf Ihren Datenträger zuzugreifen.
      ```
      kubectl describe pod <podname>
      ```
      {: pre}

      Der Datenträgermountpfad wird im Abschnitt **Containers** > **block** > **Mounts** Ihrer CLI-Ausgabe angezeigt.
   2. Melden Sie sich beim Pod an.
      ```
      kubectl exec -it <podname> bash
      ```
      {: pre}

   3. Zeigen Sie die Statistikdaten zur Plattenbelegung an und suchen Sie den Mountpfad für Ihren Datenträger, den Sie zuvor abgerufen haben. Verifizieren Sie, dass die Spalte **Größe** die neue Größe Ihres Datenträgers anzeigt.
      ```
      df -h
      ```
      {: pre}

      Beispielausgabe:
      ```
      Filesystem                                     Size  Used Avail Use% Mounted on
      overlay                                         99G  3.2G   91G   4% /
      tmpfs                                           64M     0   64M   0% /dev
      tmpfs                                          7.9G     0  7.9G   0% /sys/fs/cgroup
      /dev/mapper/3600a098038304471562b4c4743384e4d   40G   44M   23G   1% /test
      ```
      {: screen}


## Daten sichern und wiederherstellen
{: #block_backup_restore}

Der Blockspeicher wird an derselben Position wie die Workerknoten in Ihrem Cluster bereitgestellt. Der Speicher wird auf in Gruppen zusammengefassten Servern von IBM gehostet, um Verfügbarkeit sicherzustellen, falls ein Server ausfallen sollte. Der Blockspeicher wird jedoch nicht automatisch gesichert und ist möglicherweise nicht zugänglich, wenn der gesamte Standort fehlschlägt. Um Ihre Daten vor Verlust oder Beschädigung zu schützen, können Sie regelmäßige Sicherungen konfigurieren, mit denen Sie bei Bedarf Daten wiederherstellen können.
{: shortdesc}

Überprüfen Sie die folgenden Optionen zum Sichern und Wiederherstellen Ihres Blockspeichers:

<dl>
  <dt>Regelmäßige Snapshots konfigurieren</dt>
  <dd><p>Sie können [für Ihren Blockspeicher das Erstellen regelmäßiger Snapshots](/docs/infrastructure/BlockStorage?topic=BlockStorage-snapshots#snapshots) konfigurieren. Dies ist ein schreibgeschütztes Image, das den Status der Instanz zu einem bestimmten Zeitpunkt erfasst. Um den Snapshot zu speichern, müssen Sie für den Snapshot Speicherplatz im Blockspeicher anfordern. Snapshots werden in der in derselben Zone vorhandenen Speicherinstanz gespeichert. Sie können Daten von einem Snapshot wiederherstellen, wenn ein Benutzer versehentlich wichtige Daten von dem Datenträger entfernt hat.</br></br> <strong>Gehen Sie wie folgt vor, um einen Snapshot für den Datenträger zu erstellen: </strong><ol><li>[Melden Sie sich an Ihrem Konto an. Geben Sie als Ziel die entsprechende Region und, sofern zutreffend, die Ressourcengruppe an. Legen Sie den Kontext für den Cluster fest.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)</li><li>Melden Sie sich an der Befehlszeilenschnittstelle `ibmcloud sl` an. <pre class="pre"><code>    ibmcloud sl init
    </code></pre></li><li>Listen Sie alle vorhandenen PVs in Ihrem Cluster auf. <pre class="pre"><code>kubectl get pv</code></pre></li><li>Rufen Sie die Details für das PV ab, für das Snapshotspeicherplatz angefordert werden soll, und notieren Sie sich die Datenträger-ID, die Größe und die E/A-Operationen pro Sekunde (IOPS). <pre class="pre"><code>kubectl describe pv &lt;pv-name&gt;</code></pre> Die Größe und die Anzahl der E/A-Operationen pro Sekunde werden im Abschnitt <strong>Labels</strong> der CLI-Ausgabe angezeigt. Um die Datenträger-ID zu finden, überprüfen Sie die Annotation <code>ibm.io/network-storage-id</code> der CLI-Ausgabe. </li><li>Erstellen Sie die Snapshotgröße für den vorhandenen Datenträger mit den Parametern, die Sie im vorherigen Schritt abgerufen haben. <pre class="pre"><code>ibmcloud sl block snapshot-order &lt;volume_ID&gt; --size &lt;size&gt; --tier &lt;iops&gt;</code></pre></li><li>Warten Sie, bis die Snapshotgröße erstellt wurde. <pre class="pre"><code>ibmcloud sl block volume-detail &lt;volume_ID&gt;</code></pre>Die Snapshotgröße wird erfolgreich bereitgestellt, wenn der Wert für <strong>Snapshot Size (GB)</strong> (Snapshotgröße (GB)) in der CLI-Ausgabe von '0' in die von Ihnen angeforderte Größe geändert wird. </li><li>Erstellen Sie einen Snapshot für den Datenträger und notieren Sie die ID des von Sie erstellten Snapshots. <pre class="pre"><code>ibmcloud sl block snapshot-create &lt;volume_ID&gt;</code></pre></li><li>Überprüfen Sie, dass der Snapshot erfolgreich erstellt wurde. <pre class="pre"><code>ibmcloud sl block snapshot-list &lt;volume_ID&gt;</code></pre></li></ol></br><strong>Gehen Sie wie folgt vor, um Daten aus einem Snapshot auf einem vorhandenen Datenträger wiederherzustellen: </strong><pre class="pre"><code>ibmcloud sl block snapshot-restore &lt;volume_ID&gt; &lt;snapshot_ID&gt;</code></pre></p></dd>
  <dt>Snapshots in eine andere Zone replizieren</dt>
 <dd><p>Um Daten vor einem Zonenausfall zu schützen, können Sie in einer Blockspeicherinstanz, die in einer anderen Zone konfiguriert ist, [Snapshots replizieren](/docs/infrastructure/BlockStorage?topic=BlockStorage-replication#replication). Daten können nur aus dem primären Speicher an den Sicherungsspeicher repliziert werden. Sie können eine replizierte Blockspeicherinstanz nicht an einen Cluster anhängen. Wenn Ihr primärer Speicher fehlschlägt, können Sie Ihren replizierten Sicherungsspeicher manuell als primären Speicher festlegen. Anschließend können Sie ihn an den Cluster anhängen. Nachdem Ihr primärer Speicher wiederhergestellt wurde, können Sie die Daten aus dem Sicherungsspeicher wiederherstellen.</p></dd>
 <dt>Speicher duplizieren</dt>
 <dd><p>Sie können Ihre [Blockspeicherinstanz in derselben Zone duplizieren](/docs/infrastructure/BlockStorage?topic=BlockStorage-duplicatevolume#duplicatevolume), in der sich auch die ursprüngliche Speicherinstanz befindet. Ein Duplikat hat dieselben Daten wie die Originalspeicherinstanz zu dem Zeitpunkt, an dem das Duplikat erstellt wurde. Verwenden Sie das Duplikat - im Gegensatz zu den Replikaten - als unabhängige Speicherinstanz. Erstellen Sie zur Vorbereitung einer Duplizierung zunächst Snapshots für den Datenträger.</p></dd>
  <dt>Daten in {{site.data.keyword.cos_full}} sichern</dt>
  <dd><p>Sie können den Befehl [**ibm-backup-restore image**](/docs/services/RegistryImages/ibm-backup-restore?topic=RegistryImages-ibmbackup_restore_starter#ibmbackup_restore_starter) verwenden, damit ein Pod für Sicherung und Wiederherstellung in Ihrem Cluster den Betrieb aufnimmt. Dieser Pod enthält ein Script zur Ausführung einer einmaligen oder regelmäßigen Sicherung für alle PVCs (Persistent Volume Claims) in Ihrem Cluster. Die Daten werden in Ihrer {{site.data.keyword.cos_full}}-Instanz gespeichert, die Sie in einer Zone konfiguriert haben.</p><p class="note">Blockspeicher wird mit dem RWO-Zugriffsmodus (ReadWriteOnce) angehängt. Dieser Zugriff ermöglicht, dass gleichzeitig nur ein einziger Pod an den Blockspeicher angehängt werden kann. Zum Sichern Ihrer Daten müssen Sie den Speicher von Ihrem App-Pod abhängen, ihn an Ihren Sicherungspod anhängen, die Daten sichern und den Speicher wieder an Ihren App-Pod anhängen. </p>
Damit Ihre Daten noch besser hoch verfügbar sind und um Ihre App vor einem Zonenausfall zu schützen, konfigurieren Sie eine zweite {{site.data.keyword.cos_short}}-Instanz und replizieren Sie die Daten zonenübergreifend. Falls Sie Daten von Ihrer {{site.data.keyword.cos_short}}-Instanz wiederherstellen müssen, verwenden Sie das Wiederherstellungsscript, das mit dem Image bereitgestellt wird.</dd>
<dt>Daten in und aus Pods und Containern kopieren</dt>
<dd><p>Sie können den [Befehl ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/reference/kubectl/overview/#cp) `kubectl cp` verwenden, um Dateien und Verzeichnisse in und aus Pods oder spezifischen Containern in Ihrem Cluster zu kopieren.</p>
<p>Vorbereitende Schritte: [Melden Sie sich an Ihrem Konto an. Geben Sie als Ziel die entsprechende Region und, sofern zutreffend, die Ressourcengruppe an. Legen Sie den Kontext für den Cluster fest.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure) Wenn Sie keinen Container mit <code>-c</code> angeben, verwendet der Befehl den ersten verfügbaren Container im Pod.</p>
<p>Sie können den Befehl auf verschiedene Weisen verwenden:</p>
<ul>
<li>Kopieren Sie Daten von Ihrer lokalen Maschine in einen Pod in Ihrem Cluster: <pre class="pre"><code>kubectl cp <var>&lt;lokaler_dateipfad&gt;/&lt;dateiname&gt;</var> <var>&lt;namensbereich&gt;/&lt;pod&gt;:&lt;dateipfad_des_pods&gt;</var></code></pre></li>
<li>Kopieren Sie Daten von einem Pod in Ihrem Cluster auf Ihre lokale Maschine: <pre class="pre"><code>kubectl cp <var>&lt;namensbereich&gt;/&lt;pod&gt;:&lt;dateipfad_des_pods&gt;/&lt;dateiname&gt;</var> <var>&lt;lokaler_dateipfad&gt;/&lt;dateiname&gt;</var></code></pre></li>
<li>Kopieren Sie Daten von Ihrer lokalen Maschine in einen bestimmten Container, der in einem Pod in Ihrem Cluster ausgeführt wird: <pre class="pre"><code>kubectl cp <var>&lt;lokaler_dateipfad&gt;/&lt;dateiname&gt;</var> <var>&lt;namensbereich&gt;/&lt;pod&gt;:&lt;pod-dateipfad&gt;</var> -c <var>&lt;container&gt;</var></code></pre></li>
</ul></dd>
  </dl>

<br />


## Speicherklassenreferenz
{: #block_storageclass_reference}

### Bronze
{: #block_bronze}

<table>
<caption>Blockspeicherklasse: bronze</caption>
<thead>
<th>Merkmale</th>
<th>Einstellung</th>
</thead>
<tbody>
<tr>
<td>Name</td>
<td><code>ibmc-block-bronze</code></br><code>ibmc-block-retain-bronze</code></td>
</tr>
<tr>
<td>Typ</td>
<td>[Endurance-Speicher](/docs/infrastructure/BlockStorage?topic=BlockStorage-About#provendurance)</td>
</tr>
<tr>
<td>Dateisystem</td>
<td>ext4</td>
</tr>
<tr>
<td>IOPS pro Gigabyte</td>
<td>2</td>
</tr>
<tr>
<td>Größenbereich in Gigabyte</td>
<td>20-12000 Gi</td>
</tr>
<tr>
<td>Festplatte</td>
<td>SSD</td>
</tr>
<tr>
<td>Abrechnung</td>
<td>Der Standardabrechnungstyp hängt von der Version Ihres Blockspeicher-Plug-ins von {{site.data.keyword.Bluemix_notm}} ab: <ul><li> Ab Version 1.0.1: Auf Stundenbasis (Hourly)</li><li>Bis Version 1.0.0: Monatlich (Monthly)</li></ul></td>
</tr>
<tr>
<td>Preisstruktur</td>
<td>[Informationen zur Preisstruktur ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/cloud/block-storage/pricing)</td>
</tr>
</tbody>
</table>


### Silver
{: #block_silver}

<table>
<caption>Blockspeicherklasse: silver</caption>
<thead>
<th>Merkmale</th>
<th>Einstellung</th>
</thead>
<tbody>
<tr>
<td>Name</td>
<td><code>ibmc-block-silver</code></br><code>ibmc-block-retain-silver</code></td>
</tr>
<tr>
<td>Typ</td>
<td>[Endurance-Speicher](/docs/infrastructure/BlockStorage?topic=BlockStorage-About#provendurance)</td>
</tr>
<tr>
<td>Dateisystem</td>
<td>ext4</td>
</tr>
<tr>
<td>IOPS pro Gigabyte</td>
<td>4</td>
</tr>
<tr>
<td>Größenbereich in Gigabyte</td>
<td>20-12000 Gi</td>
</tr>
<tr>
<td>Festplatte</td>
<td>SSD</td>
</tr>
<tr>
<td>Abrechnung</td>
<td>Der Standardabrechnungstyp hängt von der Version Ihres Blockspeicher-Plug-ins von {{site.data.keyword.Bluemix_notm}} ab: <ul><li> Ab Version 1.0.1: Auf Stundenbasis (Hourly)</li><li>Bis Version 1.0.0: Monatlich (Monthly)</li></ul></td>
</tr>
<tr>
<td>Preisstruktur</td>
<td>[Informationen zur Preisstruktur ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/cloud/block-storage/pricing)</td>
</tr>
</tbody>
</table>

### Gold
{: #block_gold}

<table>
<caption>Blockspeicherklasse: gold</caption>
<thead>
<th>Merkmale</th>
<th>Einstellung</th>
</thead>
<tbody>
<tr>
<td>Name</td>
<td><code>ibmc-block-gold</code></br><code>ibmc-block-retain-gold</code></td>
</tr>
<tr>
<td>Typ</td>
<td>[Endurance-Speicher](/docs/infrastructure/BlockStorage?topic=BlockStorage-About#provendurance)</td>
</tr>
<tr>
<td>Dateisystem</td>
<td>ext4</td>
</tr>
<tr>
<td>IOPS pro Gigabyte</td>
<td>10</td>
</tr>
<tr>
<td>Größenbereich in Gigabyte</td>
<td>20-4000 Gi</td>
</tr>
<tr>
<td>Festplatte</td>
<td>SSD</td>
</tr>
<tr>
<td>Abrechnung</td>
<td>Der Standardabrechnungstyp hängt von der Version Ihres Blockspeicher-Plug-ins von {{site.data.keyword.Bluemix_notm}} ab: <ul><li> Ab Version 1.0.1: Auf Stundenbasis (Hourly)</li><li>Bis Version 1.0.0: Monatlich (Monthly)</li></ul></td>
</tr>
<tr>
<td>Preisstruktur</td>
<td>[Informationen zur Preisstruktur ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/cloud/block-storage/pricing)</td>
</tr>
</tbody>
</table>

### Custom
{: #block_custom}

<table>
<caption>Blockspeicherklasse: custom</caption>
<thead>
<th>Merkmale</th>
<th>Einstellung</th>
</thead>
<tbody>
<tr>
<td>Name</td>
<td><code>ibmc-block-custom</code></br><code>ibmc-block-retain-custom</code></td>
</tr>
<tr>
<td>Typ</td>
<td>[Leistung](/docs/infrastructure/BlockStorage?topic=BlockStorage-About#provperformance)</td>
</tr>
<tr>
<td>Dateisystem</td>
<td>ext4</td>
</tr>
<tr>
<td>E/A-Operationen pro Sekunde (IOPS) und Größe</td>
<td><strong>Größenbereich in Gigabyte / IOPS-Bereich in Vielfachen von 100</strong><ul><li>20-39 Gi / 100-1000 IOPS</li><li>40-79 Gi / 100-2000 IOPS</li><li>80-99 Gi / 100-4000 IOPS</li><li>100-499 Gi / 100-6000 IOPS</li><li>500-999 Gi / 100-10000 IOPS</li><li>1000-1999 Gi / 100-20000 IOPS</li><li>2000-2999 Gi / 200-40000 IOPS</li><li>3000-3999 Gi / 200-48000 IOPS</li><li>4000-7999 Gi / 300-48000 IOPS</li><li>8000-9999 Gi / 500-48000 IOPS</li><li>10000-12000 Gi / 1000-48000 IOPS</li></ul></td>
</tr>
<tr>
<td>Festplatte</td>
<td>Das Verhältnis zwischen der Anzahl E/A-Operationen pro Sekunde und Gigabyte bestimmt den Typ der Festplatte, die bereitgestellt wird. Um das Verhältnis zwischen der Anzahl E/A-Operationen pro Sekunde zu Gigabyte zu bestimmen, teilen Sie die Anzahl E/A-Operationen pro Sekunde durch die Größe Ihres Speichers. </br></br>Beispiel: </br>Sie wählen 500 Gigabyte (500Gi) Speicher mit 100 E/A-Operationen pro Sekunde. Ihr Verhältnis ist 0,2 (100 E/A-Operationen pro Sekunde/500 Gigabyte). </br></br><strong>Zuordnung Festplattentypen/Verhältnis – Übersicht:</strong><ul><li>Kleiner-gleich 0,3: SATA</li><li>Größer als 0,3: SSD</li></ul></td>
</tr>
<tr>
<td>Abrechnung</td>
<td>Der Standardabrechnungstyp hängt von der Version Ihres Blockspeicher-Plug-ins von {{site.data.keyword.Bluemix_notm}} ab: <ul><li> Ab Version 1.0.1: Auf Stundenbasis (Hourly)</li><li>Bis Version 1.0.0: Monatlich (Monthly)</li></ul></td>
</tr>
<tr>
<td>Preisstruktur</td>
<td>[Informationen zur Preisstruktur ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/cloud/block-storage/pricing)</td>
</tr>
</tbody>
</table>

<br />


## Beispiele für angepasste Speicherklassen (custom)
{: #block_custom_storageclass}

Sie können eine angepasste Speicherklasse erstellen und die Speicherklasse im PVC verwenden.
{: shortdesc}

{{site.data.keyword.containerlong_notm}} stellt [vordefinierte Speicherklassen](#block_storageclass_reference) bereit, um Blockspeicher mit einem besonderem Tier und einer besonderen Konfiguration bereitzustellen. In manchen Fällen kann es sinnvoll sein, Speicher mit einer abweichenden Konfiguration bereitzustellen, die von den vordefinierten Speicherklassen nicht abgedeckt wird. Sie können die Beispiele in diesem Abschnitt verwenden, um Beispiele für angepasste Speicherklassen zu suchen.

Informationen zum Erstellen einer angepassten Speicherklasse finden Sie unter [Speicherklasse anpassen](/docs/containers?topic=containers-kube_concepts#customized_storageclass). [Verwenden Sie die angepasste Speicherklasse anschließend im PVC](#add_block).

### Topologieorientierten Speicher erstellen
{: #topology_yaml}

Zur Verwendung von Blockspeicher in einem Mehrzonencluster muss Ihr Pod in derselben Zone wie Ihre Blockspeicherinstanz geplant werden, damit Sie Schreib- und Leseoperationen auf dem Datenträger ausführen können. Bevor die topologieorientierte Datenträgerplanung von Kubernetes eingeführt wurde, wurde die Blockspeicherinstanz automatisch durch die dynamische Bereitstellung Ihres Speichers erstellt, wenn ein PVC erstellt wurde. Wenn Sie Ihren Pod erstellten, versuchte der Kubernetes-Scheduler, den Pod in demselben Rechenzentrum wie Ihre Blockspeicherinstanz bereitzustellen.
{: shortdesc}

Eine Erstellung der Blockspeicherinstanz ohne Kenntnis der Einschränkungen des Pods kann zu unerwünschten Ergebnissen führen. Zum Beispiel könnte Ihr Pod möglicherweise nicht auf demselben Workerknoten wie Ihr Speicher geplant werden, weil der Workerknoten über keine ausreichenden Ressourcen verfügt oder weil der Workerknoten einen Taint hat und das Planen des Pods nicht zulässt. Bei der topologieorientierten Datenträgerplanung wird die Blockspeicherinstanz verzögert, bis der erste Pod erstellt wird, der den Speicher verwendet.

Die topologieorierntiert Datenträgerplanung wird nur auf Clustern unterstützt, die Kubernetes Version 1.12 oder höher ausführen. Stellen Sie zur Verwendung dieses Features sicher, dass Sie das {{site.data.keyword.Bluemix_notm}} Block Storage-Plug-in der Version 1.2.0 oder höher installiert haben.
{: note}

Die folgenden Beispiele zeigen, wie Speicherklassen erstellt werden, die die Erstellung der Blockspeicherinstanz verzögern, bis der erste Pod, der diesen Speicher verwendet, zur Planung bereit ist. Zur Verzögerung der Erstellung müssen Sie die Option `volumeBindingMode: WaitForFirstConsumer` einschließen. Wenn Sie diese Option nicht einschließen, wird `volumeBindingMode` automatisch auf `Immediate` gesetzt und die Blockspeicherinstanz wird erstellt, wenn Sie den PVC erstellen.

- **Beispiel für Endurance-Blockspeicher:**
  ```
  apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
    name: ibmc-block-bronze-delayed
  parameters:
    billingType: hourly
    classVersion: "2"
    fsType: ext4
    iopsPerGB: "2"
    sizeRange: '[20-12000]Gi'
    type: Endurance
  provisioner: ibm.io/ibmc-block
  reclaimPolicy: Delete
  volumeBindingMode: WaitForFirstConsumer
  ```
  {: codeblock}

- **Beispiel für Performance-Blockspeicher:**
  ```
  apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
   name: ibmc-block-performance-storageclass
   labels:
     kubernetes.io/cluster-service: "true"
  provisioner: ibm.io/ibmc-block
  parameters:
   billingType: "hourly"
   classVersion: "2"
   sizeIOPSRange: |-
     "[20-39]Gi:[100-1000]"
     "[40-79]Gi:[100-2000]"
     "[80-99]Gi:[100-4000]"
     "[100-499]Gi:[100-6000]"
     "[500-999]Gi:[100-10000]"
     "[1000-1999]Gi:[100-20000]"
     "[2000-2999]Gi:[200-40000]"
     "[3000-3999]Gi:[200-48000]"
     "[4000-7999]Gi:[300-48000]"
     "[8000-9999]Gi:[500-48000]"
     "[10000-12000]Gi:[1000-48000]"
   type: "Performance"
  reclaimPolicy: Delete
  volumeBindingMode: WaitForFirstConsumer
  ```
  {: codeblock}

### Zone und Region angeben
{: #block_multizone_yaml}

Wenn Sie Ihren Blockspeicher in einer bestimmten Zone erstellen wollen, können Sie die Zone und die Region in einer angepassten Speicherklasse angeben.
{: shortdesc}

Verwenden Sie die angepasste Speicherklasse, wenn Sie das {{site.data.keyword.Bluemix_notm}}-Blockspeicher-Plug-in Version 1.0.0 verwenden oder wenn Sie in einer bestimmten Zone [Blockspeicher statisch bereitstellen](#existing_block) möchten. In allen anderen Fällen [geben Sie die Zone direkt im PVC an](#add_block).
{: note}

Die folgende `.yaml`-Datei passt eine Speicherklasse an, die auf der Speicherklasse `ibm-block-silver` ohne 'retain' (Beibehaltung) basiert: Der Typ (`type`) ist `"Endurance"`, der Wert für `iopsPerGB` ist `4`, der Wert für `sizeRange` ist `"[20-12000]Gi"` und für `reclaimPolicy` wird die Einstellung `"Delete"` festgelegt. Die Zone wird als `dal12` angegeben. Lesen Sie in der [Speicherklassenreferenz](#block_storageclass_reference) nach, wenn Sie eine andere Speicherklasse als Basis verwenden möchten.

Erstellen Sie die Speicherklasse in derselben Region und Zone wie Ihren Cluster und Ihre Workerknoten. Zum Abrufen der Region Ihres Clusters führen Sie den Befehl `ibmcloud ks cluster-get --cluster <clustername_oder_-id>` aus und suchen nach dem Regionspräfix in der Master-URL (**Master URL**), wie zum Beispiel `eu-de` in `https://c2.eu-de.containers.cloud.ibm.com:11111`. Zum Abrufen der Zone Ihres Workerknotens führen Sie den Befehl `ibmcloud ks workers --cluster <clustername_oder_-id>` aus.

- **Beispiel für Endurance-Blockspeicher:**
  ```
  apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
    name: ibmc-block-silver-mycustom-storageclass
    labels:
      kubernetes.io/cluster-service: "true"
  provisioner: ibm.io/ibmc-block
  parameters:
    zone: "dal12"
    region: "us-south"
    type: "Endurance"
    iopsPerGB: "4"
    sizeRange: "[20-12000]Gi"
  reclaimPolicy: "Delete"
  ```
  {: codeblock}

- **Beispiel für Performance-Blockspeicher:**
  ```
  apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
    name: ibmc-block-performance-storageclass
    labels:
      kubernetes.io/cluster-service: "true"
  provisioner: ibm.io/ibmc-block
  parameters:
    zone: "dal12"
    region: "us-south"
    type: "Performance"
    sizeIOPSRange: |-
      "[20-39]Gi:[100-1000]"
      "[40-79]Gi:[100-2000]"
      "[80-99]Gi:[100-4000]"
      "[100-499]Gi:[100-6000]"
      "[500-999]Gi:[100-10000]"
      "[1000-1999]Gi:[100-20000]"
      "[2000-2999]Gi:[200-40000]"
      "[3000-3999]Gi:[200-48000]"
      "[4000-7999]Gi:[300-48000]"
      "[8000-9999]Gi:[500-48000]"
      "[10000-12000]Gi:[1000-48000]"
  reclaimPolicy: "Delete"
  ```
  {: codeblock}

### Blockspeicher mit einem Dateisystem `XFS` anhängen
{: #xfs}

Im folgenden Beispiel wird eine Speicherklasse erstellt, die Blockspeicher mit einem Dateisystem `XFS` bereitstellt.
{: shortdesc}

- **Beispiel für Endurance-Blockspeicher:**
  ```
  apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
    name: ibmc-block-custom-xfs
    labels:
      addonmanager.kubernetes.io/mode: Reconcile
  provisioner: ibm.io/ibmc-block
  parameters:
    type: "Endurance"
    iopsPerGB: "4"
    sizeRange: "[20-12000]Gi"
    fsType: "xfs"
  reclaimPolicy: "Delete"
  ```

- **Beispiel für Performance-Blockspeicher:**
  ```
  apiVersion: storage.k8s.io/v1
   kind: StorageClass
   metadata:
     name: ibmc-block-custom-xfs
     labels:
       addonmanager.kubernetes.io/mode: Reconcile
   provisioner: ibm.io/ibmc-block
   parameters:
     type: "Performance"
     sizeIOPSRange: |-
       [20-39]Gi:[100-1000]
       [40-79]Gi:[100-2000]
       [80-99]Gi:[100-4000]
       [100-499]Gi:[100-6000]
       [500-999]Gi:[100-10000]
       [1000-1999]Gi:[100-20000]
       [2000-2999]Gi:[200-40000]
       [3000-3999]Gi:[200-48000]
       [4000-7999]Gi:[300-48000]
       [8000-9999]Gi:[500-48000]
       [10000-12000]Gi:[1000-48000]
    fsType: "xfs"
     reclaimPolicy: "Delete"
     classVersion: "2"
  ```
  {: codeblock}

<br />

