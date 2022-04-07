---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-11"

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
{:preview: .preview}



# Eliminación del almacenamiento persistente de un clúster
{: #cleanup}

Cuando configura el almacenamiento persistente en el clúster, tiene tres componentes principales: la reclamación de volumen persistente (PVC) de Kubernetes que solicita almacenamiento, el volumen persistente (PV) de Kubernetes que se monta en un pod y se describe en la PVC y la instancia de infraestructura de IBM Cloud (SoftLayer), como por ejemplo un archivo NFS o almacenamiento en bloque. En función de cómo haya creado el almacenamiento, es posible que los tenga que suprimir por separado.
{:shortdesc}

## Limpieza del almacenamiento persistente
{: #storage_remove}

Varias opciones de supresión:

**He suprimido mi clúster. ¿Tengo que suprimir algo más para eliminar el almacenamiento persistente?**</br>
Depende. Cuando se suprime un clúster, se suprimen la PVC y el PV. Sin embargo, puede elegir si desea eliminar la instancia de almacenamiento asociada en la infraestructura de IBM Cloud (SoftLayer). Si ha elegido no eliminarla, la instancia de almacenamiento seguirá existiendo. Además, si ha suprimido el clúster en un estado no saludable, es posible que el almacenamiento siga existiendo incluso si ha elegido eliminarlo. Siga las instrucciones, en particular el paso para [suprimir la instancia de almacenamiento](#sl_delete_storage) en la infraestructura de IBM Cloud (SoftLayer).

**¿Puedo suprimir la PVC para eliminar todo mi almacenamiento?**</br>
A veces. Si [cree el almacenamiento persistente de forma dinámica](/docs/containers?topic=containers-kube_concepts#dynamic_provisioning) y seleccione una clase de almacenamiento sin la opción `retain` en su nombre, cuando suprima la PVC, también se suprimirán el PV y la instancia de almacenamiento de la infraestructura de IBM Cloud (SoftLayer).

En los demás casos, siga las instrucciones para comprobar el estado de su PVC, PV y del dispositivo de almacenamiento físico y suprímalos por separado si hace falta.

**¿Se me seguirá cobrando el almacenamiento después de que lo suprima?**</br>
Depende de lo que suprima y del tipo de facturación. Si suprime la PVC y el PV, pero no la instancia de la cuenta de infraestructura de IBM Cloud (SoftLayer), dicha instancia seguirá existiendo y se le facturará por ella. Debe suprimir todo para evitar cargos. Además, cuando especifique el tipo de facturación (`billingType`) en la PVC, puede elegir por hora (`hourly`) o mensual (`monthly`). Si elige `monthly`, la instancia se facturará mensualmente. Cuando se suprime la instancia, se le facturará durante el resto del mes.


<p class="important">Cuando se limpia el almacenamiento persistente, se suprimen todos los datos almacenados en el mismo. Si necesita una copia de los datos, realice una copia de seguridad del [almacenamiento de archivos](/docs/containers?topic=containers-file_storage#file_backup_restore) o del [almacenamiento en bloque](/docs/containers?topic=containers-block_storage#block_backup_restore).</p>

Antes de empezar: [Inicie la sesión en su cuenta. Si procede, apunte al grupo de recursos adecuado. Establezca el contexto para el clúster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

Para limpiar los datos persistentes:

1.  Obtenga una lista de las PVC del clúster y anote el nombre (**`NAME`**) de la PVC, su **`STORAGECLASS`** y el nombre del PV vinculado a la PVC que se muestra como **`VOLUME`**.
    ```
    kubectl get pvc
    ```
    {: pre}

    Salida de ejemplo:
    ```
    NAME                  STATUS    VOLUME                                     CAPACITY   ACCESSMODES   STORAGECLASS            AGE
    claim1-block-bronze   Bound     pvc-06886b77-102b-11e8-968a-f6612bb731fb   20Gi       RWO           ibmc-block-bronze       78d
    claim-file-bronze     Bound     pvc-457a2b96-fafc-11e7-8ff9-b6c8f770356c   4Gi        RWX           ibmc-file-bronze-retain 105d
    claim-file-silve      Bound     pvc-1efef0ba-0c48-11e8-968a-f6612bb731fb   24Gi       RWX           ibmc-file-silver        83d
    ```
    {: screen}

2. Revise los valores **`ReclaimPolicy`** y **`billingType`** para la clase de almacenamiento.
   ```
   kubectl describe storageclass <storageclass_name>
   ```
   {: pre}

   Si la política de reclamación indica `Delete`, el PV y el almacenamiento físico se eliminan cuando se elimina la PVC. Si la política de reclamación indica `Retain`, o si ha suministrado el almacenamiento sin una clase de almacenamiento, el PV y el almacenamiento físico no se eliminan cuando se elimina la PVC. Debe eliminar la PVC, el PV y el almacenamiento físico por separado.

   Si el almacenamiento se carga de forma mensual, se le facturará todo el mes, aunque elimine el almacenamiento antes de que finalice el ciclo de facturación.
   {: important}

3. Elimine los pods que montan la PVC.
   1. Obtenga una lista de los pods que montan la PVC.
      ```
      kubectl get pods --all-namespaces -o=jsonpath='{range .items[*]}{"\n"}{.metadata.name}{":\t"}{range .spec.volumes[*]}{.persistentVolumeClaim.claimName}{" "}{end}{end}' | grep "<pvc_name>"
      ```
      {: pre}

      Salida de ejemplo:
      ```
      blockdepl-12345-prz7b:	claim1-block-bronze  
      ```
      {: screen}

      Si no se devuelve ningún pod en la salida de la CLI, significa que no tiene ningún pod que utilice la PVC.

   2. Elimine el pod que utiliza la PVC. Si el pod forma parte de un despliegue, elimine el despliegue.
      ```
      kubectl delete pod <pod_name>
      ```
      {: pre}

   3. Verifique que el pod se ha eliminado.
      ```
      kubectl get pods
      ```
      {: pre}

4. Elimine la PVC.
   ```
   kubectl delete pvc <pvc_name>
   ```
   {: pre}

5. Revise el estado de su PV. Utilice el nombre del PV que ha recuperado antes como **`VOLUME`**.
   ```
   kubectl get pv <pv_name>
   ```
   {: pre}

   Cuando se elimina la PVC, se libera el PV vinculado a la PVC. En función de cómo haya suministrado el almacenamiento, el PV entra en estado `Deleting` si el PV se suprime automáticamente o en estado `Released` si debe suprimir manualmente el PV. **Nota**: para los PV que se suprimen automáticamente, el estado puede indicar brevemente `Released` antes de que se suprima. Vuelva a ejecutar el mandato después de unos minutos para ver si se ha eliminado el PV.

6. Si su PV no se ha suprimido, elimine manualmente el PV.
   ```
   kubectl delete pv <pv_name>
   ```
   {: pre}

7. Verifique que el PV se ha eliminado.
   ```
   kubectl get pv
   ```
   {: pre}

8. {: #sl_delete_storage}Obtenga una lista de la instancia de almacenamiento físico a la que apuntaba su PV y anote el **`id`** de la instancia de almacenamiento físico.

   **Almacenamiento de archivos:**
   ```
   ibmcloud sl file volume-list --columns id  --columns notes | grep <pv_name>
   ```
   {: pre}
   **Almacenamiento en bloque:**
   ```
   ibmcloud sl block volume-list --columns id --columns notes | grep <pv_name>
   ```
   {: pre}

   Si ha eliminado el clúster y no puede recuperar el nombre del PV, sustituya `grep <pv_name>` por `grep <cluster_id>` para obtener una lista de todos los dispositivos de almacenamiento asociados al clúster.
   {: tip}

   Salida de ejemplo:
   ```
   id         notes
   12345678   {"plugin":"ibm-file-plugin-5b55b7b77b-55bb7","region":"us-south","cluster":"aa1a11a1a11b2b2bb22b22222c3c3333","type":"Endurance","ns":"default","pvc":"mypvc","pv":"pvc-d979977d-d79d-77d9-9d7d-d7d97ddd99d7","storageclass":"ibmc-file-gold"}
   ```
   {: screen}

   Visión general de la información del campo **Notes**:
   *  **`"plugin":"ibm-file-plugin-5b55b7b77b-55bb7"`**: El plugin de almacenamiento que utiliza el clúster.
   *  **`"region":"us-south"`**: La región en la que está el clúster.
   *  **`"cluster":"aa1a11a1a11b2b2bb22b22222c3c3333"`**: El ID del clúster asociado a la instancia de almacenamiento.
   *  **`"type":"Endurance"`**: El tipo de almacenamiento de archivos o en bloque, que puede ser `Endurance` o `Performance`.
   *  **`"ns":"default"`**: El espacio de nombres en el que se ha desplegado la instancia de almacenamiento.
   *  **`"pvc":"mypvc"`**: El nombre de la PVC asociada a la instancia de almacenamiento.
   *  **`"pv":"pvc-d979977d-d79d-77d9-9d7d-d7d97ddd99d7"`**: El PV asociado a la instancia de almacenamiento.
   *  **`"storageclass":"ibmc-file-gold"`**: El tipo de clase de almacenamiento: bronze, silver, gold o custom.

9. Elimine la instancia de almacenamiento físico.

   **Almacenamiento de archivos:**
   ```
   ibmcloud sl file volume-cancel <filestorage_id>
   ```
   {: pre}

   **Almacenamiento en bloque:**
   ```
   ibmcloud sl block volume-cancel <blockstorage_id>
   ```
   {: pre}

9. Verifique que se ha eliminado la instancia de almacenamiento físico. El proceso de supresión puede tardar unos días en completarse.

   **Almacenamiento de archivos:**
   ```
   ibmcloud sl file volume-list
   ```
   {: pre}
   **Almacenamiento en bloque:**
   ```
   ibmcloud sl block volume-list
   ```
   {: pre}
