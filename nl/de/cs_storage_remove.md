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


# Persistenten Speicher aus einem Cluster entfernen
{: #cleanup}

Wenn Sie in Ihrem Cluster persistenten Speicher konfigurieren, haben Sie drei Hauptkomponenten: den Persistent Volume Claim (PVC) von Kubernetes, der Speicher anfordert, den persistenten Datenträger (PV) von Kubernetes, der an einen Pod angehängt und im PVC beschrieben ist, sowie die Instanz der IBM Cloud-Infrastruktur (SoftLayer), z. B. NFS-Dateispeicher oder Blockspeicher. Je nachdem, wie Sie diese erstellt haben, müssen Sie möglicherweise alle drei separat löschen.
{:shortdesc}

## Persistenten Speicher bereinigen
{: #storage_remove}

Erklärung der Löschoptionen:

**Ich habe meinen Cluster gelöscht. Muss ich Weiteres löschen, um den persistenten Speicher zu entfernen?**</br>
Es kommt darauf an. Wenn Sie einen Cluster löschen, werden der PVC und der persistente Datenträger gelöscht. Sie können jedoch wählen, ob die zugeordnete Speicherinstanz in der IBM Cloud-Infrastruktur (SoftLayer) entfernt werden soll. Wenn Sie wählen, Sie nicht zu entfernen, ist die Speicherinstanz noch vorhanden. Wenn Sie den Cluster in einem nicht einwandfreien Zustand gelöscht haben, kann der Speicher auch dann noch vorhanden sein, wenn Sie ausgewählt haben, ihn zu entfernen. Führen Sie die Anweisungen aus, insbesondere den Schritt zum [Löschen Ihrer Speicherinstanz](#sl_delete_storage) in der IBM Cloud-Infrastruktur (SoftLayer).

**Kann ich den PVC löschen, um meinen gesamten Speicher zu entfernen?**</br>
Manchmal. Wenn Sie [den persistenten Speicher dynamisch erstellen](/docs/containers?topic=containers-kube_concepts#dynamic_provisioning) und eine Speicherklasse wählen, die im Namen kein `retain` führt, werden beim Löschen des PVC auch der persistente Datenträger und die Instanz der IBM Cloud-Infrastruktur (SoftLayer) gelöscht.

Führen Sie in allen anderen Fällen die Anweisungen zum Überprüfen des Status Ihres Persistent Volume Claim, des persistenten Datenträgers und der physischen Speichereinheit aus und löschen Sie sie, falls erforderlich, separat.

**Fallen auch nach dem Löschen des Speichers Gebühren an?**</br>
Dies hängt vom Abrechnungstyp sowie davon ab, welche Elemente Sie löschen. Wenn Sie den PVC und den persistenten Datenträger, aber nicht die Instanz in Ihrem Konto der IBM Cloud-Infrastruktur (SoftLayer) löschen, ist diese Instanz weiterhin vorhanden und Ihnen werden dafür Gebühren berechnet. Damit Ihnen keine Gebühren mehr berechnet werden, müssen Sie alles löschen. Wenn Sie außerdem im Persistent Volume Claim den Abrechnungstyp (`billingType`) angeben, können Sie zwischen einer Abrechnung auf Stundenbasis (`hourly`) und der monatlichen Abrechnung (`monthly`) wählen. Wenn Sie `monthly` auswählen, wird monatlich eine Abrechnung für Ihre Instanz erstellt. Wenn Sie die Instanz löschen, wird Ihnen der Rest des Monats in Rechnung gestellt.


<p class="important">Wenn Sie den persistenten Speicher bereinigen, werden alle Daten gelöscht, die in ihm gespeichert sind. Wenn Sie eine Kopie der Daten benötigen, müssen Sie für den [Dateispeicher](/docs/containers?topic=containers-file_storage#file_backup_restore) oder den [Blockspeicher](/docs/containers?topic=containers-block_storage#block_backup_restore) eine Sicherung ausführen.</p>

Vorbereitende Schritte: [Melden Sie sich an Ihrem Konto an. Geben Sie als Ziel die entsprechende Region und, sofern zutreffend, die Ressourcengruppe an. Legen Sie den Kontext für den Cluster fest.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

Gehen Sie wie folgt vor, um persistente Daten zu bereinigen:

1.  Listen Sie die PVCs in Ihrem Cluster auf und notieren Sie sich folgende Einstellungen: **`NAME`** des Persistent Volume Claim, die Speicherklasse (**`STORAGECLASS`**) und den Namen des persistenten Datenträgers, der an den Persistent Volume Claim gebunden ist und als **`VOLUME`** angezeigt wird.
    ```
    kubectl get pvc
    ```
    {: pre}

    Beispielausgabe:
    ```
    NAME                  STATUS    VOLUME                                     CAPACITY   ACCESSMODES   STORAGECLASS            AGE
    claim1-block-bronze   Bound     pvc-06886b77-102b-11e8-968a-f6612bb731fb   20Gi       RWO           ibmc-block-bronze       78d
    claim-file-bronze     Bound     pvc-457a2b96-fafc-11e7-8ff9-b6c8f770356c   4Gi        RWX           ibmc-file-bronze-retain 105d
    claim-file-silve      Bound     pvc-1efef0ba-0c48-11e8-968a-f6612bb731fb   24Gi       RWX           ibmc-file-silver        83d
    ```
    {: screen}

2. Überprüfen Sie für die Speicherklasse die Einstellungen für **`ReclaimPolicy`** und **`billingType`**.
   ```
   kubectl describe storageclass <name_der_speicherklasse>
   ```
   {: pre}

   Wenn die Zurückforderungsrichtlinie `Delete` (Löschen) enthält, werden Ihr persistenter Datenträger und der physische Speicher entfernt, wenn Sie den Persistent Volume Claim entfernen. Wenn die Zurückforderungsrichtlinie `Retain` (Beibehalten) enthält oder Sie Ihren Speicher ohne Speicherklasse bereitgestellt haben, werden Ihr persistenter Datenträger und der physische Speicher nicht entfernt, wenn Sie den Persistent Volume Claim entfernen. Sie müssen den PVC, den persistenten Datenträger und den physischen Speicher separat voneinander entfernen.

   Wenn Ihr Speicher monatlich berechnet wird, wird Ihnen der gesamte Monat auch dann in Rechnung gestellt, wenn Sie den Speicher vor Ende des Abrechnungszyklus entfernen.
   {: important}

3. Entfernen Sie alle Pods, die den Persistent Volume Claim anhängen.
   1. Listen Sie die Pods auf, die den PVC anhängen.
      ```
      kubectl get pods --all-namespaces -o=jsonpath='{range .items[*]}{"\n"}{.metadata.name}{":\t"}{range .spec.volumes[*]}{.persistentVolumeClaim.claimName}{" "}{end}{end}' | grep "<pvc-name>"
      ```
      {: pre}

      Beispielausgabe:
      ```
      blockdepl-12345-prz7b:	claim1-block-bronze  
      ```
      {: screen}

      Wenn in Ihrer CLI-Ausgabe kein Pod zurückgegeben wird, haben Sie keine Pods, die den PVC verwenden.

   2. Entfernen Sie den Pod, der den PVC verwendet. Wenn der Pod Teil einer Bereitstellung ist, entfernen Sie die Bereitstellung.
      ```
      kubectl delete pod <podname>
      ```
      {: pre}

   3. Überprüfen Sie, dass der Pod entfernt wurde.
      ```
      kubectl get pods
      ```
      {: pre}

4. Entfernen Sie den Persistent Volume Claim.
   ```
   kubectl delete pvc <pvc-name>
   ```
   {: pre}

5. Überprüfen Sie den Status Ihres persistenten Datenträgers. Verwenden Sie den Namen des persistenten Datenträgers (PV), den Sie zuvor als **`VOLUME`** abgerufen haben.
   ```
   kubectl get pv <pv-name>
   ```
   {: pre}

   Wenn Sie den PVC entfernen, wird der an den PVC gebundene persistente Datenträger freigegeben. Abhängig davon, wie Sie Ihren Speicher bereitgestellt haben, wird Ihr persistenter Datenträger bei automatischem Löschen in den Status `Deleting` (Wird gelöscht) oder bei manuellem Löschen in den Status `Released` (Freigegeben) versetzt. **Hinweis**: Bei automatisch zu löschenden persistenten Datenträgern kann vor dem Löschen kurzzeitig der Status `Released` angezeigt werden. Führen Sie den Befehl nach einigen Minuten erneut aus, um zu prüfen, ob der persistente Datenträger entfernt wurde.

6. Wenn der persistente Datenträger nicht entfernt wurde, entfernen Sie ihn manuell.
   ```
   kubectl delete pv <pv-name>
   ```
   {: pre}

7. Überprüfen Sie, dass der persistente Datenträger entfernt wurde.
   ```
   kubectl get pv
   ```
   {: pre}

8. {: #sl_delete_storage}Listen Sie die physische Speicherinstanz auf, auf die Ihr persistenter Datenträger verwiesen hat, und notieren Sie die **`id`** der physischen Speicherinstanz.

   **Dateispeicher:**
   ```
   ibmcloud sl file volume-list --columns id  --columns notes | grep <pv-name>
   ```
   {: pre}
   **Blockspeicher:**
   ```
   ibmcloud sl block volume-list --columns id --columns notes | grep <pv-name>
   ```
   {: pre}

   Wenn Sie Ihren Cluster entfernt haben und den Namen des persistenten Datenträgers nicht abrufen können, ersetzen Sie `grep <pv-name>` durch `grep <cluster-id>`, um alle Speichereinheiten aufzulisten, die Ihrem Cluster zugeordnet sind.
   {: tip}

   Beispielausgabe:
   ```
   id         notes   
   12345678   {"plugin":"ibm-file-plugin-5b55b7b77b-55bb7","region":"us-south","cluster":"aa1a11a1a11b2b2bb22b22222c3c3333","type":"Endurance","ns":"default","pvc":"mypvc","pv":"pvc-d979977d-d79d-77d9-9d7d-d7d97ddd99d7","storageclass":"ibmc-file-gold"}
   ```
   {: screen}

   Erklärung der Informationen im Feld **notes**:
   *  **`"plugin":"ibm-file-plugin-5b55b7b77b-55bb7"`**: Das Speicher-Plug-in, das der Cluster verwendet.
   *  **`"region":"us-south"`**: Die Region, in der sich Ihr Cluster befindet.
   *  **`"cluster":"aa1a11a1a11b2b2bb22b22222c3c3333"`**: Die Cluster-ID, die der Speicherinstanz zugeordnet ist.
   *  **`"type":"Endurance"`**: Der Typ des Datei- oder Blockspeichers, entweder `Endurance` oder `Performance`.
   *  **`"ns":"default"`**: Der Namensbereich, in dem die Speicherinstanz bereitgestellt wurde.
   *  **`"pvc":"mypvc"`**: Der Name des Persistent Volume Claim, der der Speicherinstanz zugeordnet ist.
   *  **`"pv":"pvc-d979977d-d79d-77d9-9d7d-d7d97ddd99d7"`**: Der persistente Datenträger, der der Speicherinstanz zugeordnet ist.
   *  **`"storageclass":"ibmc-file-gold"`**: Der Typ von Speicherklasse: 'bronze', 'silver', 'gold' oder 'custom'.

9. Entfernen Sie die physische Speicherinstanz.

   **Dateispeicher:**
   ```
   ibmcloud sl file volume-cancel <dateispeicher-id>
   ```
   {: pre}

   **Blockspeicher:**
   ```
   ibmcloud sl block volume-cancel <blockspeicher-id>
   ```
   {: pre}

9. Überprüfen Sie, dass die physische Speicherinstanz entfernt wurde. Beachten Sie, dass der Löschvorgang mehrere Tage dauern kann.

   **Dateispeicher:**
   ```
   ibmcloud sl file volume-list
   ```
   {: pre}
   **Blockspeicher:**
   ```
   ibmcloud sl block volume-list
   ```
   {: pre}
